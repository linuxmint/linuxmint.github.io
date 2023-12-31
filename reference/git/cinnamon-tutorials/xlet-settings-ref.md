[Home](/) / 
[Reference](/reference/git/) / 
[Cinnamon Tutorials](/reference/git/cinnamon-tutorials) /
[Extension System](/reference/git/cinnamon-tutorials/extension-system.html)

## Applet, desklet and extension settings reference

This is the reference for the settings API.

### Widget types and required fields

#### checkbox

- `type`: should be `checkbox`
- `default`: `true` or `false` (no quotes)
- `description`: String describing the setting

A simple checkbox that controls a `boolean` type value

#### entry

- `type`: should be `entry`
- `default`: default string value
- `description`: String describing the setting

A text entry field that stores a `string`

#### colorchooser

- `type`: should be `colorchooser`
- `default`: default color string - can be `"red"` or `"rgba(x,x,x,x)"`, etc...
- `description`: String describing the setting

A Color button that lets you choose a RGBA color code as a `string`

#### keybinding

- `type`: should be `keybinding`
- `default`: default keybinding string - i.e. `&lt;Control&gt;F8` or other string parseable by gtk_accelerator_parse.
- `description`: String describing the setting

An input that allows you to select a keybinding for an action.

#### radiogroup

- `type`: should be `radiogroup`
- `default`: default value from the list of options, or it can be a custom value if `custom` is defined
- `description`: String describing the setting
- `options`: node of desc:val pair options, where desc is the displayed option name, val is the stored value

A group of radio buttons whose description and values are defined by `options` in
`description:value` pairs. Values may be `string` or
`number`. also have a value of `custom`, and a text
entry will be provided next to that option, to allow entering a custom value.
Options might be:

```json
{
  "options" : {
      "Option 1" : "this value",
      "Option 2" : "that value",
      "Option 3" : "other value"
  }
}
```

#### filechooser

- `type`: should be `filechooser`
- `description`: String describing the setting
- `default`: Default filename to use
- `select-dir`: (optional) true or false, or leave off entirely. Forces directory selection.

Opens a file picker dialog to allow you to choose a filename. If `select-dir` is
`true`, it will only allow directories to be selected. Stores as a `string`.

#### iconfilechooser

- `type`: should be `iconfilechooser`
- `description`: String describing the setting
- `default`: default icon path or icon name to use

Provides a preview button and text entry field. You can open a file dialog to pick an image-type file, or
enter a registered icon name in the text field. Stores as a `string`.

#### combobox

- `type`: should be `combobox`
- `default`: default value to set
- `description`: String describing the setting
- `options`: node of desc:val pair options, where desc is the displayed option name, val is the stored value

Provides a dropdown list from which you can select from `description:value` pairs defined by 
`options`. The values can be `string`, 
`number`, or `boolean`.

#### spinbutton

- `type`: should be `spinbutton`
- `default`: default value to use - int or leading
- `min`: minimum value
- `max`: maximum value
- `units`: String describing what the number is a unit of (pixels, bytes, etc..)
- `step`: adjustment amount
- `description`: String describing the setting

Provides a spin button and entry for changing setting a `number` value. This can
be integer or floating point format. For floating point, all values must have leading 0's.

#### scale

- `type`: should be `scale`
- `default`: default value to use - int or leading
- `min`: minimum value
- `max`: maximum value
- `step`: adjustment amount
- `description`: String describing the setting

Provides a scale widget to allow you to pick a `number` value between min and
max, by step amount. Integer or floating point numbers can be used. For floating point, all values must
have leading 0's.

#### generic

- `type`: should be `generic`
- `default`: default value

A generic storage object for any type of value. This is generally intended for internal settings that
won't be adjusted by the user. For example, a history, or most recent command. There is no corresponding
widget for it in Cinnamon Settings.

#### header

- `type`: should be `header`
- `description`: String to display as a bold header

A <em>non-setting</em> widget, this provides a bold-faced label for assisting in organizing your settings

#### separator

- `type`: should be `separator`

A <em>non-setting</em> widget, this draws a horizontal separator for assisting in organizing your settings

#### button

- `type`: should be `button`
- `description`: Label for the button
- `callback`: string of callback method name (no "this", just "myFunc")

A <em>non-setting</em> widget, this provides a button, which, when clicked,
activates the `callback` method in your applet. Note: the callback value should
be a string of the method name only. For instance, to call `this.myCallback()`,
you would put `myCallback` for the callback value.

#### Additional Setting Options

These fields can be added to any widget:

- `indent: true`: Indent the widget in the settings page to help with organizing your layout.
- `dependency: "<key>"`: where `key` is the name of a `checkbox` or `combobox` setting.
  If it is a checkbox and that checkbox setting is un-checked, this setting will be hidden.
  If it is a combobox, you need to state an option of this combobox too, e.g. `"type=http"`. Your setting will only be visible if the given option is selected in the combobox.
  The checkbox/combobox must occur <em>before</em> the setting that depends on it.
- `tooltip`: Adds a popup tooltip to the widget

#### Signals

#### settings-changed

Signal when the underlying config file has changed and the in-memory values have been updated.

#### changed::&lt;key&gt;

Signals when `key` has changed in the configuration file. Use this in conjunction
with `getValue` if you want to handle your own updating in a more traditional
way (like gsettings).

The callback function will be called with three paramenters: `settingProvider, oldval, newval`, which are, respectively, 
the settings object (which you usually don't need), the original value and the updated value.

### Additional options in metadata.json

You can add the following items to `metadata.json` to affect how the settings are
presented to the user:

-   `hide-configuration`: Hides the configure button in Cinnamon Settings. Set
    to `true` if you are using only `generic`-type
    settings that should be hidden from the user. This is not a mandatory key. Omitting it will allow
    the configuration button to hide or display depending on whether you are utilizing the settings API
    or not.
-   `external-configuration-app`: Allows you to define an external settings app
    to use instead of the built-in settings GUI. This should be a `string` with
    the name of your executable settings app (path relative to the applet's install directory). Note,
    this key can be overridden by the `hide-configuration` key. This is not a
    mandatory key. Omitting it will allow the configuration button to hide or display depending on
    whether you are utilizing the settings API or not.
