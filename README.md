# Field plug-ins

## Overview

Field plug-ins override the default screen that is loaded when a field is shown in SurveyCTO Collect. When filling out a form, if you arrive at a field which is using a field plug-in, the plug-in’s code will be loaded and processed instead of the default SurveyCTO code.

## Getting started using field plug-ins

If you already have a field plug-in that you'd like to use, please [click here to view the getting started guide](getting-started.md).

## Getting started developing field plug-ins

If you're interested in developing your own field plug-ins, we suggest taking the following steps:

1. Read the [field plug-in definition](docs/plug-in-definition.md) documentation.
1. Familiarize yourself with the [API reference](docs/api-reference.md) documentation.
1. Fork one of our baseline templates from below.
1. If you don't yet have access to a SurveyCTO server, you'll need one! You can sign up for free [here](https://login.surveycto.com/signup/step1.html).

## Baseline templates

These field plug-ins reproduce the default behavior of regular SurveyCTO field types. There should be virtually no differences in field appearance/behavior when these are used. They are meant to be used as starter templates for you to use when developing your own field plug-ins.

* [Baseline text field](https://github.com/SurveyCTO-field-plug-ins/baseline-text)
* [Baseline integer field](https://github.com/SurveyCTO-field-plug-ins/baseline-integer)
* [Baseline decimal field](https://github.com/SurveyCTO-field-plug-ins/baseline-decimal)
* [Baseline select_one field](https://github.com/SurveyCTO-field-plug-ins/baseline-select_one)
* [Baseline select_multiple field](https://github.com/SurveyCTO-field-plug-ins/baseline-select_multiple)

## Example templates

We've developed a few field plug-ins to demonstrate some of their more advanced features. These are not inteded to be used for actual data collection, but are offered as tools to help you learn or test out these advanced features.

* [Parameters](https://github.com/SurveyCTO-field-plug-ins/example-parameters)  
    Parameters can be used to pass values into a field plug-in (including values from other form fields).
* [Metadata](https://github.com/SurveyCTO-field-plug-ins/example-metadata)  
    Metadata can be used by your field plug-in to store data about itself while the form is being filled out.
* [Intents](https://github.com/SurveyCTO-field-plug-ins/example-intents)  
    Intents can be used by Android Collect to communicate with other apps on the device.
* [Launch SMS](https://github.com/SurveyCTO-field-plug-ins/example-launch-sms)  
    This is a more use-case-specific version of the *intents* plugin (above).

## Developer documentation

* [Field plug-in definition](docs/plug-in-definition.md)
* [API reference](docs/api-reference.md)

## Additional resources

* [Full list of all field plug-ins developed by SurveyCTO](https://github.com/SurveyCTO-field-plug-ins)
