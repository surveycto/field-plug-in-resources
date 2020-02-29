# Getting started using field plug-ins

In order to use a field plug-in, you need to do three things:

1. **Make sure you are using SurveyCTO version 2.70 or later.**  
    Log in to your server console, scroll all the way to the bottom, and make sure it says **SurveyCTO Server v2.70** (or later) in the footer. If you're using an older version, you can update for free at [https://account.surveycto.com](https://account.surveycto.com).
1. **Attach the field plug-in to your form.**
1. **Select the field plug-in at the appropriate field(s).**  

## Attaching a field plug-in to a form

Attach the “[name].fieldplugin.zip” file to your form. If you're using the online Form Designer to edit your form, you can attach a field plug-in by opening *Form settings* and scrolling down to the *Attachments* section. You may also attach a field plug-in while editing a field's *appearance* option (see below).

If you're uploading spreadsheet form definitions, you can attach it just like you would do with any other form attachment (like attached .csv files or images).

## Selecting a field plug-in at a field

If you're using the online Form Designer, when editing a field, scroll down to the *appearance* setting in the *Other options* section. If you select the *Use a field plug-in to render this field* checkbox, you will see a dropdown list of all the available field plug-ins that are attached to your form and support the current field type. If none are available, you can select the *Add new field plug-in* option and attach a new one.

If you're manually editing the spreadsheet form definition, you can specify the field plug-in to use in the *appearance* column of a field. For each field that should use your plug-in, set the field’s appearance to “custom-[plug-in name]”.

> **EXAMPLE**  
> Let’s say you have a field plug-in with the filename “myplugin.fieldplugin.zip”. You would enter `custom-myplugin` in the appearance column for each field that should use it.

## What to expect

Please keep in mind, when you use a field plug-in, that plug-in will completely take over the appearance of a field. Make sure you trust the developer before you use their field plug-in and, as always, perform adequate testing before you deploy your form. Some things to keep in mind:

* A field plug-in may not support your current appearance options. Check the developer's documentation to see what options are supported before you attach it to your form, and test it thoroughly before you start actually collecting data.
* The "Label display behavior" setting (in Collect's Admin Settings) will have no effect on a field plug-in.

## Parameters

Some field plug-ins will accept or even require parameters. Each field plug-in can have slightly different requirements with regard to parameters, so be sure to read the developer's documentation carefuly.

If you're using the online Form Designer, adding parameters is easy. Once you've selected a field plug-in in the *appearance* section, you will see an input box labeled *Plug-in parameters:*. Enter a comma-separated list of parameters according to the requirements of the field plug-in.

> **EXAMPLE**  
> Let’s say you have a field plug-in that requires `min` and `max` parameters. you could enter "`min=0, max=10`" in the box. If the same field plug-in accepts an optional parameter for `color`, you might enter "`min=0, max=10, color='blue'`".

If you're manually editing the spreadsheet form definition, you can specify parameters when you specify the field plug-in (in the *appearance* column). Add a comma-separated list of parameters after the plug-in name, enclosed by parenthesis.

> **EXAMPLE**
> If your field plug-in file is named "slider.fieldplugin.zip", and accepts parameters for `min` and `max`, your appearance column could look like this: "`custom-slider(min=0, max=10)`"

**Note:** parameters can evaluate expressions and field references. So if you want to define a parameter's value based on the response from a previous field, you could do something like this: "`min=0, max=${field}`".