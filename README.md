# Field plug-ins

This repository contains the developer documentation for SurveyCTO field plug-ins. It also contains links to certain featured plug-ins, which will help you get started developing your own.

## Overview

Field plug-ins override the default webview that is loaded when a field is shown in SurveyCTO Collect. When filling out a form, if you arrive at a field which is using a field plug-in, the plug-in’s code will be loaded and processed instead of the default SurveyCTO code.

Field plug-ins themselves are just zip files, with “.fieldplugin” added to the file extension (for example: “myfirstplugin.fieldplugin.zip”).

## Getting started

In order to use a field plug-in, you need to do two things:

1. **Attach it to the form.**
    Attach the “[name].fieldplugin.zip” file to the form definition just like you would do with any other form attachment (like attached .csv files or images).
1. **Attach it to the appropriate fields.**
    For each field that should use your plug-in, set the field’s appearance to “custom-[name]” (in the appearance column of the form definition).

> **EXAMPLE**  
> Let’s say you created a field plug-in with the filename “myplugin.fieldplugin.zip”. You would enter `custom-myplugin` in the appearance column for each field that should use it.

## Basic templates

These field plug-ins reproduce the default behavior of regular SurveyCTO field types. There should be virtually no differences in field appearance/behavior when these are used. They are meant to be used as starter templates for you to use when developing your own field plug-ins.

* [Basic text field](https://github.com/SurveyCTO-field-plug-ins/basic-text-field)
* [Basic integer field](https://github.com/SurveyCTO-field-plug-ins/basic-text-field)
* [Basic decimal field](https://github.com/SurveyCTO-field-plug-ins/basic-text-field)
* [Basic select-one field](https://github.com/SurveyCTO-field-plug-ins/basic-text-field)
* [Basic select-multiple field](https://github.com/SurveyCTO-field-plug-ins/basic-text-field)

## Developer documentation

* [Field plug-in definition](docs/plug-in-definition.md)
* [API reference](docs/api-reference.md)

## Additional resources

* [Full list of all field plug-ins developed by SurveyCTO](https://github.com/SurveyCTO-field-plug-ins)
