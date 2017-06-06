[Home](/) / 
[Reference](/reference/git/) / 
[Cinnamon Tutorials](/reference/git/cinnamon-tutorials) /
[Extension System](/reference/git/cinnamon-tutorials/extension-system.html)

## Writing applets

### Introduction

In this tutorial, we will go through the process of creating a simple applet to learn about the Applet API. Here we will create a Force Quit applet.

For those who are unfamiliar with “force-quitting”: When a window becomes unresponsive and doesn’t want to close, the most efficient way to force it to close is to kill its process. You could use the “`ps`” command to find its process ID and kill it with the “`kill`” command. Or alternatively you could run the “`xkill`” command, and simply click on the window you want to kill. And that’s exactly what our “Force Quit” applet is going to do... after you click on it, your mouse cursor will change into a window killer, which you’ll target at the window you want to get rid of :)
    


### Creating the basic structure of the applet

An applet has to be given a unique ID (uuid). In this tutorial we will name it "force-quit@cinnamon.org". Of course, you should give your applets your own UUIDs, using either your name or your domain name behind the @ sign.

An applet is basically a directory, whose name is the UUID, containing two files:
    
- A “`metadata.json`” file which contains information about the applet, such as its name, description etc..
- An “`applet.js`” file which contains its code.

Applets go in `~/.local/share/cinnamon/applets` (or in `/usr/share/cinnamon/applets` if you want them installed system-wide). So let’s go there and let’s create the files and folders necessary for any Cinnamon applet.
    
```bash
cd
mkdir -p .local/share/cinnamon/applets/force-quit@cinnamon.org
cd .local/share/cinnamon/applets/force-quit@cinnamon.org
touch metadata.json
touch applet.js
```

### Defining the applet metadata

Let's open `metadata.json` and describe our applet. This file defines the UUID, name, description, and icon of your applet and is used by Cinnamon to identify it and display it to the user in Cinnamon Settings.
    
```json
{
    "uuid": "force-quit@cinnamon.org",
    "name": "Force Quit",
    "description": "Click on the applet to launch xkill and force any window to quit immediately",
    "icon": "force-exit"
}
```

By default, only one instance of every applet can be placed on the user's panel. But if the user has multiple panels, they cannot have one force-quit applet on each panel, which is bad. Hence we should also add a `max-instance` property. We can specify any number we want (eg `3`), or we can make it unlimited by making it `-1`. In this case, our new `metadata.json` would be
    
```json
{
    "uuid": "force-quit@cinnamon.org",
    "name": "Force Quit",
    "description": "Click on the applet to launch xkill and force any window to quit immediately",
    "icon": "force-exit",
    "max-instances": "-1"
}
```

There are more things we can add to `metadata.json`, but they will not be covered here.

### Writing our applet
 Here is the code for our applet:
    
```javascript
const Applet = imports.ui.applet;
const Util = imports.misc.util;

function MyApplet(orientation, panel_height, instance_id) {
    this._init(orientation, panel_height, instance_id);
}

MyApplet.prototype = {
    __proto__: Applet.IconApplet.prototype,

    _init: function(orientation, panel_height, instance_id) {
        Applet.IconApplet.prototype._init.call(this, orientation, panel_height, instance_id);

        this.set_applet_icon_name("force-exit");
        this.set_applet_tooltip(_("Click here to kill a window"));
    },

    on_applet_clicked: function() {
        Util.spawn('xkill');
    }
};

function main(metadata, orientation, panel_height, instance_id) {
    return new MyApplet(orientation, panel_height, instance_id);
}
```

Now we'll go through the code line by line, starting from the bottom.
    
```javascript
function main(metadata, orientation, panel_height, instance_id) {
    return new MyApplet(orientation, panel_height, instance_id);
}
```

The `main` function is the only thing Cinnamon understands. To load an applet, Cinnamon calls the `main` function in the applet's code, and expects to get an Applet object, which it will add to the panel. So here we instantiate a `MyApplet` object (whose details are defined above), and returns it.

You will notice that there are a lot of parameters floating around. `metadata` contains, basically, the information inside `metadata.json` plus some more. This is not very helpful in general, but can sometimes provide some useful information.

`orientation` is whether the panel is at the top or the bottom. The applet can behave differently depending on its position, eg. to make the popup menus show up on the right side.

`panel_height` tells you, unsurprisingly, the height of the panel. This way we can scale the icons up and down depending on how large the panel is.

`instance_id` tells you which instance of the applet you are, since there can be multiple instances of the applet present. While this is just a number assigned to the applet and doesn't have much meaning by itself, it is required to access, say, the individual settings of an applet (which will be described later).

If that sounds like a lot of things to take care of, you should be relieved that the applet API handles all that for you! All you have to do is to pass it to the applet API. Here we are passing this information to the constructor, and the constructor will later pass it to the applet API.

So what exactly happens when we create the applet? We first look at the first two lines:
    
```javascript
const Applet = imports.ui.applet;
const Util = imports.misc.util;
```

Here we import certain APIs provided by Cinnamon. The most important one is, of course, the `Applet` API. We will also need `Util` in order to run an external program (`xkill`). When we write `imports.ui.applet`, we are accessing the functions defined in `/usr/share/cinnamon/js/ui/applet.js`. Similarly, `imports.misc.util` accesses `/usr/share/cinnamon/js/misc/util.js`. We then call these things `Applet` and `Util` respectively to avoid typing the long bunch of code.

Not very interesting. Next!
    
```javascript
function MyApplet(orientation, panel_height, instance_id) {
    this._init(orientation, panel_height, instance_id);
}
```

This is the standard constructor of a Javascript Object. When someone calls it, it calls the `_init` function of the object, and does stuff. Note that here we called our applet `MyApplet`. You are free to call it whatever you want (and change the `main` function accordingly, of course), but there really is no reason to do so!.

Also note that we pass all the `orientation` etc. information down the chain until it ultimately reaches the applet API.

What we've gone through is mostly boilerplate. Now we get to the meat of the code:
    
```javascript
MyApplet.prototype = {
    __proto__: Applet.IconApplet.prototype,

    _init: function(orientation, panel_height, instance_id) {
        Applet.IconApplet.prototype._init.call(this, orientation, panel_height, instance_id);

        this.set_applet_icon_name("force-exit");
        this.set_applet_tooltip(_("Click here to kill a window"));
    },

    on_applet_clicked: function() {
        Util.spawn('xkill');
    }
};
```

Roughly speaking, the `prototype` is the list of all functions of the object. Here we have the `_init` function and the `on_applet_clicked` function, which is all we need.

Here we see what I've referred to as the "applet API" all the time. A few barebone applet objects are defined in `applet.js`, and we inherit one of them. Here we choose to inherit `Applet.IconApplet`, which is an applet that displays an icon.

Inheritance in JavaScript is slightly weird. We first copy over `Applet.IconApplet`'s prototype using the
    
```javascript
__proto__: Applet.IconApplet.prototype,
```

line. This copies all the functions found in `Applet.IconApplet` to our applet, which we are going to use.

Next, in our `_init` function, we call the `_init` function of `Applet.IconApplet`. Here we pass on all the information about `orientation` etc. to this `_init` function, and this function will help us sort out all the mess required to make the applet display properly.

However, contrary to popular belief, the applet API is not psychic. It has no idea what your applet wants to do (apart from displaying an icon). You have to first tell it what icon you want, in the line
    
```javascript
this.set_applet_icon_name("force-exit");
```

Note that `set_applet_icon_name` is a function defined inside `Applet.IconApplet`, which makes the applet display the corresponding icon. By using the applet API, we have saved ourselves from the hassle of creating icons and putting them in the right place. (`force-exit` is the name of an icon from the user's icon set. The icons available for use can be found in `/usr/share/icons/`)

The next line is
    
```javascript
this.set_applet_tooltip(_("Click here to kill a window"));
```

which says that the applet should have a tooltip called `_("Click here to kill a window")`. By wrapping the string with the `_( )` function, we are telling Cinnamon to translate the string to the correct language, if translations are available.

That is all the code in our `_init` function. Now we have to wait for the user to click on the applet. The applet API automatically helps you to listen to these events, and when the user presses the applet, the API will call the `on_applet_clicked` of your applet. Here we have
    
```javascript
on_applet_clicked: function() {
    Util.spawn('xkill');
}
```

which calls the `spawn` function from `Util`, and launches an external command, `xkill`.