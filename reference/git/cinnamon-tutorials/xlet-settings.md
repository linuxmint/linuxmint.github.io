[Home](/) / 
[Reference](/reference/git/) / 
[Cinnamon Tutorials](/reference/git/cinnamon-tutorials) /
[Extension System](/reference/git/cinnamon-tutorials/extension-system.html)

## Applet, desklet and extension settings

### Introduction

This document is intended to serve as a guide and reference to the new Cinnamon applet, desklet, and extension settings API, new for Cinnamon 1.8.  The goal of this API was to provide for an easy to implement and use storage system for settings.

We will start with a broad overview of the system, and then work our way into the details.

### Overview

In a nutshell, your applet will provide a JSON file that defines various setting types and their details, including a key identifier, description, default value, minimum and maximums (in the case of numeric types) and other options pertinent to each type of setting.

When your applet is added to the panel, you instantiate a settings provider, which looks for this file, parses it, applies default values, and saves an "instance" copy to work with.

Once that setting provider is initialized, your applet can then bind properties to the various setting keys from that file, and use them to set up how your applet will look, feel, and operate. There are a few types of bindings available, depending on how you want your applet to control or be controlled by these settings.

That is the "live", applet side of the picture.

The other side of this, is that same JSON file you created for your applet is also used by Cinnamon Settings (or System Settings as it will be known in 1.8,) to automatically generate a configuration panel to adjust all these settings. There are a number of formatting options, as well as the capability for some settings to be "dependent" on another setting - allowing you to deactivate or "grey out" settings that cannot be used in certain situations. Changes made to this panel are saved immediately, and can be handled by the applet via callbacks that you defined in your applet.
    
### The settings-schema.json file

Here’s a basic example of a <code class="code">settings-schema.json</code> file, that can define settings for your applet:
    
```json
{
    "width": {
        "type" : "scale",
        "default" : 10,
        "min" : 2,
        "max" : 400,
        "step" : 1,
        "description" : "Amount of space in pixels"
    }
}
```

The most important item here to start is the key: <code class="code">"width"</code>. This is what your applet is going to specify when binding a property to the setting provider. Within the <code class="code">width</code> node are the various details specific to that key, and differ depending on what type of setting you’ll be using.
    
In this case:
    
- <code class="code">type</code>: defines what type of widget you want to use to adjust this value. In general the names of these types will be similar to what GTK uses, but always lowercase, and single word.
- <code class="code">default</code>: defines what you want your default, or starting value to be for this setting. Since scale defines a numeric type, the default will be a numeral (no quotations). You can use floating point, but you must have a leading 0 for floating numbers less than 1 (i.e. 0.1).
- <code class="code">min</code>: the lower limit for your setting.
- <code class="code">max</code>: the upper limit for your setting.
- <code class="code">step</code>: the step amount for this setting - how much the value changes for each <span class="quote">“<span class="quote">click</span>”</span> or notch of the widget.
- <code class="code">description</code>: The description you want by your setting in the configuration panel, to explain what the setting controls.

The JSON file must be valid - it must pass any structure tests, not have extra commas, and so forth. There are a number of tools available online where you can paste the contents of your file in, and it will tell you if it is valid or not, and point out problem areas. One such site is <a class="ulink" href="http://jsonlint.com/" target="_top">http://jsonlint.com/</a>.
    
So now we’ve got a <code class="code">settings-schema.json</code> file. It goes in your applet directory, alongside the <code class="code">metadata.json</code>, <code class="code">applet.js</code>, and any other files you have.
    
### Preparing your applet:

The first thing you need to do is add Settings to your <span class="quote">“<span class="quote">imports</span>”</span> at the top of your applet:
    
```javascript
const Settings = imports.ui.settings;
```

You’ll also need to ensure that you’re passing along the <code class="code">instanceId</code> of the applet from the <code class="code">main()</code> function:
    
```javascript
function main(metadata, orientation, panel_height, instanceId) {
    let myApplet = new MyApplet(metadata, orientation, panel_height, instanceId);
    return myApplet;
}
```

and
    
```javascript
function MyApplet(metadata, orientation, panel_height, instanceId) {
    this._init(metadata, orientation, panel_height, instanceId);
}

MyApplet.prototype = {
    __proto__: Applet.TextIconApplet.prototype,

    _init: function(metadata, orientation, panel_height, instanceId) {
        //....
        //....
        //....
    }
```

Note: Desklets only need to pass <code class="code">metadata</code> and <code class="code">instanceId</code> through this chain, and extensions have no need of any of this.
    
### Initializing the settings provider

Now, to actually initialize your settings provider, usually somewhere in the <code class="code">_init</code> method of <code class="code">MyApplet</code>, you’ll put something like this, which will use your applet’s own instance variables:
    
```javascript
this.settings = new Settings.AppletSettings(this, "settings-example@cinnamon.org", instanceId);
```

If you prefer to have your preferences in a separate object, you can do like this, and the object will be populated with your settings variables:
    
```javascript
this._preferences = {};
this.settings = new Settings.AppletSettings(this._preferences, "settings-example@cinnamon.org", instanceId);
```

In other words, the object reference you pass in can be any object. The UUID there can be just hardcoded, or can be drawn from the metadata (<code class="code">metadata["uuid"]</code>), whichever you prefer. You can also leave off the instanceId if you haven’t defined your applet as multiple-instance capable.

Obviously, if instead of an applet, you have a desklet or an extension, you would use <code class="code">new Settings.DeskletSettings</code> and <code class="code">new Settings.ExtensionSettings</code> respectively.
    
### Binding your settings

Since we only have one setting in our example, this will be very simple:
    
```javascript
this.settings.bindProperty(Settings.BindingDirection.IN,
                           "width",
                           "width",
                           this.on_settings_changed,
                           null);
```

To explain:
    
- <code class="code">.bindProperty</code>: tells the settings provider you want to bind one of your settings keys to an applet property.
- <code class="code">Settings.BindingDirection.IN</code>: This tells the setting provider that you want your applet property to be updated by changes in the configuration file. Other flags exist to provide the opposite direction, as well as bidirectional updating. More on those later.
- <code class="code">"width"</code>: This is the key value you defined in your settings-schema.json file that you want to match up with.
- <code class="code">"width"</code>: This is the name of the property you want to store the value in your applet. You’ll refer to it traditionally as <code class="code">this.width</code> from here on out in the applet code.
- <code class="code">this.on_settings_changed</code>: This is the method you want called when the setting value changes. <span class="emphasis"><em>NOTE: The property (<code class="code">this.width</code>) already contains the new value when this method is called. All you have to do is act on it.</em></span>
- <code class="code">null</code>: this is optional - it can be left off entirely, or used to pass any extra object to the callback if desired.

That’s basically it - your property, <code class="code">this.width</code>, or <code class="code">this._preferences.width</code>, is now managed by the setting provider, and will be automatically updated when the configuration changes, and your callback method automatically called so you can deal with the change.
    
### Editing your configuration:

Once you’ve activated your applet, you can then go in to Cinnamon Settings, Applets, and the option to Configure will be displayed when you select it.
    
For our sample, we’d see:
    
<img src="settings.png">
      
This has been automatically generated by our JSON file. If we adjust the scale, the JSON config file is rewritten, the settings provider for our applet notices, and reloads the new value into the applet’s <code class="code">this.width</code> property, and <code class="code">this.on_settings_changed()</code> is called.
    
Alternatively, this can be launched by running the command <code class="code">cinnamon-settings applets *uuid*</code> (with obvious modifications for desklets and extensions).
    
### More information

The available settings items and widgets can be found an the <a class="xref" href="xlet-settings-ref.html" title="Applet, desklet and extension settings reference"><i>Applet, desklet and extension settings reference</i></a>, alongside with some extra options. The settings objects themselves are <code class="code">Settings.AppletSettings</code>, <code class="code">Settings.DeskletSettings</code> and <code class="code">Settings.ExtensionSettings</code>, which all inherit <code class="code">Settings._provider</code>. Most useful functions will be found inside the <code class="code">_provider</code> object.
