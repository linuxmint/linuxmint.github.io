[Home](/) / 
[Reference](/reference/git/) / 
[Cinnamon Tutorials](/reference/git/cinnamon-tutorials) /
[Documentation](/reference/git/cinnamon-tutorials/documentation.html)

## Using documentation

### Overview

The documentation of Cinnamon is separated into 4 different parts (5 if you count muffin). What you are currently reading is the tutorials, which includes the general top-level overviews and tutorials you will need for Cinnamon. This is named "Cinnamon Tutorials".

The second part is the Javascript reference, which describes the Javascript part of Cinnamon. This is named the "Cinnamon Javascript Reference Manual". This is a technical reference for the individual functions and objects available in Cinnamon. Note that this documentation is aimed at both applet/extension developers and Cinnamon developers themselves. So depending on who you are, some of the information might be entirely irrelevant.

The third part of the documentation is for the C part of Cinnamon, which is simply referred to as the "Cinnamon Reference Manual".

The last part is the documentation for Shell toolkit, or St. This is the graphical toolkit used to draw widgets on the screen (similar to Gtk).

The modules covered by the Javascript documentation are those imported via <code class="code">imports.ui.*</code> and <code class="code">imports.misc.*</code>. The <code class="code">global</code> object is documented in the C part of Cinnamon, as well as things accessed through <code class="code">imports.gi.Cinnamon</code>. Things accessed through <code class="code">imports.gi.St</code> are, unsurprisingly, documented in the St part.

<code class="code">imports.gi.Meta</code> refers to Muffin, while others (eg. <code class="code">imports.gi.Gio</code>) are third-party (usually GNOME) libraries that are documented elsewhere.

### Accessing the documentation

There are two ways of accessing this documentation, one of which is what you are currently using. The first method is accessing it online, which will be available at <a class="ulink" href="http://linuxmint.github.io" target="_top">http://linuxmint.github.io</a>.

The second method is to access it locally. Install the program <code class="code">devhelp</code> and the <code class="code">cinnamon-doc</code> package (might be named differently in different distros or included in the <code class="code">cinnamon</code> package itself). Then run the program <code class="code">devhelp</code> to access all documentations you have installed in your system (not limited to Cinnamon).

### Javascript documentation

The Javascript documentation is divided into sections according to the files they are from. The title of each section is how you want to import it. For example, if you want to use the <code class="code">IconApplet</code> object from <code class="code">imports.ui.applet</code>, then you type <code class="code">imports.ui.applet.IconApplet</code>. Since that is very cumbersome to type, we usually declare it as a constant first. For example, you will often see

```javascript
const Applet = imports.ui.applet;
```

This way you can access the <code class="code">IconApplet</code> object via <code class="code">Applet.IconApplet</code> instead. This is how the individual pages are named as well.

The <code class="code">_init</code> function of each object is the constructor. So if you want to know what happens when you call <code class="code">new Applet.Applet</code>, you look at the <code class="code">_init</code> function of <code class="code">Applet.Applet</code>. There usually isn't much helpful information apart form what each argument does.

Certain variables and functions are prepended with <code class="code">_</code>, eg. <code class="code">_showPanel</code>. These are private variables and functions which shouldn't be accessed normally, and is there purely for referencial purposes. It is generally not a good idea to use them in applets/desklets. Within Cinnamon itself, you may use them when necessary.

Note that the <code class="code">_init</code> is also a private function, and you also shouldn't call it directly (unless you are inheriting the object). It is implicitly called when you call the constructor (which technically can be totally unrelated to the <code class="code">_init</code> function.

Each object comes with an object hierarchy, documented in the Object Hierarchy section. For example, <code class="code">Applet.IconApplet</code> inherits <code class="code">Applet.Applet</code>. The child can, obviously, access the functions and properties of the parent. However, this information is not repeated in every child. So if you cannot find the function you want in the <code class="code">Applet.IconApplet</code> page, you can look at the <code class="code">Applet.Applet</code> page as well. This is also true for the C documentation.

### C documentation

The items described here are common to all C modules imported, not just those relating to Cinnamon.

Reading the documentation of C modules is tricky, since they are written for C, and we are in Javascript. In particular, C has no concept of objects, while Javascript does. Thus the naming conventions of many things are different when used in Cinnamon, and the documentation is not directly applicable. The following sections will describe how to translate C into Javascript.

Note that most of these apply to Python and other non-C languages.

#### Objects

C has no concept of objects, but GObject allows C code to create things that look like objects. For example, <code class="code">St.Bin</code> is something that looks like an object, and acts like an object in Javascript. If you want to look up the documentation of <code class="code">St.Bin</code>, you search for <code class="code">StBin</code> (without the ".") and the documentation will come up.

The "." is removed since, in Javascript, we first import the whole St, and then take the "<code class="code">Bin</code>" object from St. This concept is non-existent in C. In C, it is simply StBin.

A notable exception is with <code class="code">Gio</code> and <code class="code">GLib</code> objects, in which the <code class="code">G</code> prefix is used in C for both while <code class="code">Gio</code> and <code class="code">GLib</code> is used in Javascript. For example, the C object is <code class="code">GSettings</code> while the Javascript object is <code class="code">Gio.Settings</code>.

Translating object names is the easiest of all.

#### Functions

If you look at the page of <code class="code">StBin</code>, you will see that the functions have very long names like <code class="code">st_bin_get_child</code>. Since we are just pretending to have objects via GObjects, and objects do not genuinely exist in C, the functions do not "belong" to the objects. We simply put the name of the object in front of the function name and trust people will actually use it on the proper object.

In C, objects are imported as if they were objects, and functions <span class="emphasis"><em>do</em></span> belong to the objects. So if <code class="code">actor</code> is an <code class="code">StBin</code>, we do not do <code class="code">st_bin_get_child(actor)</code>, but simply <code class="code">actor.get_child()</code>.

Note that the C documentation always tells you that the first argument to the function is the object itself. For example, <code class="code">st_bin_get_child</code> takes in an <code class="code">StBin</code> as the argument. We do not need to supply this.

Note also that not all functions are available for use in Javascript. If some functions are considered to be too C-like to be used in other languages, it will not be available. There is generally no simple way to figure out whether the functions are available, but usually they are. If they are not, Cinnamon will complain loudly that the function doesn't exist. So the best way to know is to try it.

Again, <code class="code">Gio</code> and <code class="code">GLib</code> functions use the <code class="code">g_</code> prefix, eg <code class="code">g_settings_set_int</code> instead of <code class="code">gio_settings_get_int</code>.

#### Properties

GObjects have properties. These can generally be accessed directly. For example, if <code class="code">actor</code> is an <code class="code">StBin</code> and you want to access the <code class="code">x-fill</code> property, you can use <code class="code">actor.x_fill</code>. Note that we translate <code class="code">-</code> to <code class="code">_</code>.

Sometimes, for some absurd reason, the properties cannot be accessed directly. In this case, there is still hope. You can access the property via <code class="code">actor.get_property("x-fill")</code> and <code class="code">actor.set_property("x-fill", false)</code>.

#### Enums

Enums are the trickiest of all. An example of an enum you might see is as follows:

### enum StButtonMask

A mask representing which mouse buttons an StButton responds to.

#### Members
<table width="100%" border="0">
<colgroup>
<col width="300px" class="enum_members_name">
<col class="enum_members_description">
<col width="200px" class="enum_members_annotations">
</colgroup>
<tbody>
<tr>
<td class="enum_member_name">ST_BUTTON_ONE</td>
<td class="enum_member_description">button 1 (left)</td>
<td class="enum_member_annotations"> </td>
</tr>
<tr>
<td class="enum_member_name">ST_BUTTON_TWO</td>
<td class="enum_member_description">button 2 (middle)</td>
<td class="enum_member_annotations"> </td>
</tr>
<tr>
<td class="enum_member_name">ST_BUTTON_THREE</td>
<td class="enum_member_description">button 3 (right)</td>
<td class="enum_member_annotations"> </td>
</tr>
</tbody>
</table>

The correct way to access these enums is <code class="code">St.ButtonMask.ONE</code>. We first start with the namespace St, then the name of the enums. Note that this does <span class="emphasis"><em>not</em></span> relate to the name of the members of the enum. For example, if the individual enum members are called <code class="code">HELLO_WORLD</code> in C, you still access them by starting with <code class="code">St.ButtonMask</code>.

What to put after <code class="code">St.ButtonMask</code> is trickier - it is the name of the member, but what should you take? You usually have to strip out some things from the left. In this case, you remove the <code class="code">ST_BUTTON</code> and retain <code class="code">ONE</code>. There is no fixed rule to what to remove. However, it is usually the name of the object (<code class="code">StButton</code> in this case), or the name of the whole group of enum.

You can save yourself some guessing by typing <code class="code">St.ButtonMask</code> in looking glass. Double-clicking into it will show you what you can access. In this case it shows <code class="code">ONE, TWO, THREE</code>.

Note that both <code class="code">St</code> and <code class="code">ButtonMask</code> are in CamelCase, but the name of the enum member uses CAPS_WITH_UNDERSCORE.
