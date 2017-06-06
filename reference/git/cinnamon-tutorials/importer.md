[Home](/) / 
[Reference](/reference/git/) / 
[Cinnamon Tutorials](/reference/git/cinnamon-tutorials) /
[CJS](/reference/git/cinnamon-tutorials/cjs.html)

## The Importer

To access code of other JavaScript files, cjs has the `imports` object.

In cjs in combination of Cinnamon you can use following statements to import statements:

```javascript
imports.*
imports.gi.*
imports.ui.*
imports.misc.*
```

### imports.*

This is the normal form of importing modules.

You can think of this object like nested objects which properties are the JavaScript files or directories.

All functions, variables (var, let, const) in a JavaScript file can be accessed like this file is an object.

To clarify, an example:

```javascript
//Direct access to file a.js
const A = imports.a;
//Directories must be also typed in, in order to get file c.js in directory b
const C = imports.b.c;

log(A.foo); //"Property foo"
log(A.bar()); //"Method bar"
log(C.baz); //"Property baz"
```

`a.js`

```javascript
let foo = "Property foo";

function bar(){
  return "Method bar";
}
```

`c.js` in a directory named `b`

```javascript
let baz = "Property baz";
```

In every case, you can include cjs core modules. Those provide you useful functions or (less often) bindings to C libraries.

Examples are:

```javascript
const Cairo = imports.cairo; //Cairo graphics
const Lang = imports.lang; //useful JavaScript functions for extensing the language
const Gettext = imports.gettext; //Gettext translation
const TweenEquations = imports.tweener.equations; //Tween equations for animations
```

As you can see, it is common to assign the import to a constant in UpperCamelCase which looks like the imported module.

To view the source of those cjs modules, you should visit [the GitHub page](https://github.com/linuxmint/cjs/tree/master/modules).

### imports.gi.*

As Cinnamon uses C libraries like Clutter, Muffin and more, there is a little problem: How can those libraries be used in cjs?

For this, there is <a class="ulink" href="https://wiki.gnome.org/Projects/GObjectIntrospection" target="_top">GObject Introspection</a>.

In short, it allows you to use C libraries in cjs, Python and other languages.

C libraries are included like this:

```javascript
const St = imports.gi.St; //Shell Toolkit, the normal way to display widgets on the Cinnamon screen
const Cinnamon = imports.gi.Cinnamon; //Cinnamon C libraries, e.g. AppSystem
```

Note: Not like normal `imports.*`, `imports.gi.*` imports needs to have the first letter after `gi.` be in upper case.

### imports.ui.*

Those imports under `imports.ui.*` are core Cinnamon modules.

Some important modules:

```javascript
const PopupMenu = imports.ui.popupMenu; //High-level classes for building menus for applets or context menus
const Applet = imports.ui.applet; //Base applet classes
```

The source is in `/usr/share/cinnamon/js/ui/`

### imports.misc.*

Those imports under `imports.misc.*` belong to Cinnamon, but aren't tied to it as much as `imports.ui.*`.

```javascript
const Util = imports.misc.util; //useful functions
const Interfaces = imports.misc.interfaces; //DBus stuff
```

The source is in `/usr/share/cinnamon/js/misc/`

### Importing xlet modules

When you want to split a big xlet code into smaller files, you'll need to import them.
A simple way is using `imports.xlet`, wher `xlet` is your xlet type
(`applet`, `desklet`, `extension`, `search_provider`)

```javascript
imports.applet.foo // get foo.js in your applet directory
```

### \_\_init__.js

When writing xlets, it is common that you have some functions or constants that you need in many files.
For that, there is `__init__.js`.
It is a normal JavaScript file, but every function or variable can be accessed directly via `import.*`.

Examples are often used functions, like a modified `_()` function for translating your xlet.

`__init__.js`

```javascript
const Gettext = imports.gettext;

const uuid = "xlet@uuid";

Gettext.bindtextdomain(uuid, GLib.get_home_dir() + "/.local/share/locale");

function _(str){
    return Gettext.dgettext(uuid, str);
}
```

In your other files:

```javascript
const uuid = imports.xlet.uuid;
const _ = imports.xlet._;
```

Remember: replace `xlet` in `imports.xlet` to your xlet type.

There is no harm renaming `__init__.js` to something else (like `util.js`) and using `imports.xlet.util.*`.