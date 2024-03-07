[Home](/) / 
[Reference](/reference/git/) / 
[Cinnamon Tutorials](/reference/git/cinnamon-tutorials) /
[Extension System](/reference/git/cinnamon-tutorials/extension-system.html)

## Gathering information to display and working with subprocesses

### Introduction

You probably want to display information on your xlet which need to be queried from the system. This can be done using different ways, and the most common is to execute a command (spawn a subprocess) and check it's output. But you should check first if you can replace subprocesses with any other method:

- **Reading file contents**  
  For example, if you want to query the system's memory usage, you can execute `free` and parse it's output. But memory information are also exposed in the file `/proc/meminfo`. You should prefer the file reading method in this case.

- **Use functions of integrated libraries**  
  You can search the [Gjs docs](https://gjs-docs.gnome.org/) if there exists an appropriate library for you. For example, GLib provides functions for getting free/used disk space and the Soup lib can make HTTP(S) requests.

- **Subprocesses**  
  If none of the above methods fulfill your needs, you can call an arbitray command (binary or script) to get the information you need. But be careful with subprocesses, they can cause stuttering on the desktop if used improperly. Of course, you can use them to not only get information but to do any other useful work too, like starting background tasks or GUI applications.


### Reading file contents

Make sure you have imported:
```
const Gio = imports.gi.Gio;
const GLib = imports.gi.GLib;
const Lang = imports.lang;
```

Now, create a function inside your xlet object to read the file.

```
let file = Gio.file_new_for_path('/proc/meminfo');
file.load_contents_async(null, (file, response) => {
	let [success, contents, tag] = file.load_contents_finish(response);
	if(success) {
		let proc_out = ByteArray.toString(contents);
		// now, you can do anything with proc_out to get you information,
		// e.g. split lines and whitespaces: split("\n")[0].split(/\s+/)
		// then, call a function to update your UI,
		// e.g. set the text to a label: this.label_info.set_text(proc_out);
	} else {
		// handle read error
	}
	GLib.free(contents);
});
```

You can use a file monitor to watch for changes on a file, so you don't need to pull periodically. Note that this only works with regular files and not with device files like `/proc/meminfo`.

```
let file = Gio.file_new_for_path('/home/user/note');
this.monitor = file.monitor_file(Gio.FileMonitorFlags.NONE, null);
this.monitor.connect('changed', Lang.bind(this, function(monitor, file, other_file, event_type) {
	if (event_type == Gio.FileMonitorEvent.CHANGES_DONE_HINT || event_type == Gio.FileMonitorEvent.DELETED) {
		// loadFileContent() can contain the code of the previous example,
		// just without the Gio.file_new_for_path initialization,
		// since the file object can be directly passed as parameter
		this.loadFileContent(file);
	}
}));
```

### Subprocesses

The initilization of a subprocess, whether it's done via `Gio.Subprocess()` or `Glib.spawn_async_with_pipes_and_fds()`, needs some milliseconds in which the desktop will not be responsive. Depending on how many subprocesses you spawn and how many xlet instances the user created, this sums up to a visible and annoying drawback. But GLib has an optimized codepath which is used if the following requirements are met. In other words this means that not all features of a GLib subprocess can be used.

1. `G_SPAWN_DO_NOT_REAP_CHILD` must be set
2. `G_SPAWN_LEAVE_DESCRIPTORS_OPEN` must be set
3. `G_SPAWN_SEARCH_PATH_FROM_ENVP` must not be set
4. `working_directory` must be `NULL`
5. `child_setup` must be `NULL`
6. The program being spawned is of a recognised binary format, or has a shebang. Otherwise, GLib will have to execute the program through the shell, which is not done using the optimized codepath.

```
try {
	let [success, child_pid, std_in, std_out, std_err] = GLib.spawn_async_with_pipes(
		/*working_directory*/ null,
		/*argv*/  ['/bin/ping', 'linuxmint.com', '-c1', '-w2'],
		/*envp*/  null,
		/*flags*/ GLib.SpawnFlags.DO_NOT_REAP_CHILD | GLib.SpawnFlags.LEAVE_DESCRIPTORS_OPEN,
		/*child_setup*/ null
	);
	GLib.close(std_in);
	GLib.close(std_err);
	if(!success) {
		throw new Error(_('Error executing command!'));
	}

	// this is necessary for correctly ending the subprocess
	// without this, a zombie process would be left behind, e.g. visible with `ps -e | grep ping`
	GLib.child_watch_add(GLib.PRIORITY_DEFAULT, child_pid, function(pid, wait_status, user_data) {
		GLib.spawn_close_pid(child_pid);
	});

	// store a reference of our xlet in a local variable
	// we need this to report the result back to the UI of our xlet
	let deskletInstance = this;

	// create a IOChannel to get the command output
	let ioChannelStdOut = GLib.IOChannel.unix_new(std_out);
	let tagWatchStdOut = GLib.io_add_watch(
		ioChannelStdOut, GLib.PRIORITY_DEFAULT,
		GLib.IOCondition.IN | GLib.IOCondition.HUP,
		function(channel, condition, data) {
			if(condition != GLib.IOCondition.HUP) {
				let [status, out] = channel.read_to_end();
				let out_string = out.toString();
				//global.log("Command returned: " + out_string); // debug log into Looking Glass
				// now, do whatever you want with out_string and report the result back to your xlet UI
				deskletInstance.label_info.set_text(out_string);
			}
			// don't forget to close the file descriptors
			// otherwise, you will run into errors like "Too many open files" when too many processes were created
			GLib.source_remove(tagWatchStdOut);
			channel.shutdown(true);
		}
	);

	// uncomment the following if you want to get stderr output
	//this.ioChannelStdErr = GLib.IOChannel.unix_new(this.std_err);
	//this.tagWatchStdErr = GLib.io_add_watch(
	//	this.ioChannelStdErr, GLib.PRIORITY_DEFAULT,
	//	GLib.IOCondition.IN | GLib.IOCondition.HUP,
	//	function(channel, condition, data) {}
	//);
} catch(error) {
	// handle the error appropriately!
}
```
