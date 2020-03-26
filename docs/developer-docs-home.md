# Developer documentation for SurveyCTO field plug-ins

## Table of contents

1. [Documentation index](#documentation-index)
1. [How to start](#how-to-start)
1. [Sharing your field plug-ins](#sharing-your-field-plug-ins)
1. [Useful field plug-ins for developers](#useful-field-plug-ins-for-developers)

## Documentation index

* [Field plug-in definition](https://github.com/surveycto/field-plug-in-resources/blob/master/docs/plug-in-definition.md)  
    This document describes the structure and format of field plug-ins. It is recommended that you read this first.  
* [API reference](https://github.com/surveycto/field-plug-in-resources/blob/master/docs/api-reference.md)  
    This document contains a comprehensive list of available APIs that your field plug-in can use to interact with the form.  
* [Using field plug-ins (user documentation)](https://docs.surveycto.com/02-designing-forms/03-advanced-topics/06.using-field-plug-ins.html)  
    This article is written for form designers and will explain how to install and use field plug-ins in SurveyCTO.

## How to start

The easiest way to get started developing your own field plug-ins is to follow the steps below to fork one of our baseline plug-ins. This will get you started with a fully functional template from which to build off of. 

1. Read the [field plug-in definition](docs/plug-in-definition.md) documentation.
1. Familiarize yourself with the [API reference](docs/api-reference.md) documentation.
1. Fork one of our baseline field plug-in repos from below.
1. Each baseline repo contains a test form. Upload that test form (and it's associated attachments) to your SurveyCTO server. 

**Note:** you'll need access to a SurveyCTO server. You can sign up for a free trial [here](https://login.surveycto.com/signup/step1.html). If you want to keep evaluating SurveyCTO after the 15-day trial period, you can use our free **community subscription**. This special subscription unlocks all features of the platform while providing just the right limits for your “sandbox”: 10 forms, 200 submissions per month, and 200 MB of storage. You can learn more by reading our [community contract](https://www.surveycto.com/community-contract/).

## Sharing your field plug-ins

We recommend trying to match the structure of our baseline plug-in repositories as closely as possible. This will ensure that users can easily find and navigate your plug-in repository. Below are some guidelines:

1. **Always include a readme.**  
    Your readme should at the very least contain a title and description of what your field plug-in is supposed to do. It is also recommended to include a "Default SurveyCTO feature support" section, a "How to use" section, and an image preview of how your plug-in looks.
1. **Always include a test form.**  
    This should be the simplest form possible that demonstrates the functionality of your field plug-in.
1. **Add the topic `scto-field-plug-in` to your repository.**  
    This will allow your field plug-in to show up when users search for that topic.
1. **Keep all source files in a `source` folder.**  
1. **Keep your .zip file up-to-date**  
    Keep the most recent version of your *x.fieldpliugin.zip* file in the root of your repository. Most users will only be looking for that file, and they will not bother looking around in subdirectories, or zipping their own version. Every time you push changes to your source code, you should update that .zip file as well.
1. **Keep all other files in an `extras` folder.**  
    This includes image previews, test forms, and other associated files that you may want to store in your repository. 

## Useful field plug-ins for developers

### Baseline field plug-ins

SurveyCTO develops and maintains a set of baseline field plug-ins which are designed to reproduce the default behavior of regular SurveyCTO field types. There should be virtually no differences in field appearance/behavior when these are used. They are meant to be used as starter templates for you to use when developing your own field plug-ins.

* [baseline-text](https://github.com/surveycto/baseline-text)
* [baseline-integer](https://github.com/surveycto/baseline-integer)
* [baseline-decimal](https://github.com/surveycto/baseline-decimal)
* [baseline-select_one](https://github.com/surveycto/baseline-select_one)
* [baseline-select_multiple](https://github.com/surveycto/baseline-select_multiple)

### Feature demonstration field plug-ins

SurveyCTO has developed a few field plug-ins to demonstrate some advanced features that plug-ins are capable of. These are not inteded to be used for actual data collection, but are offered as tools to help you learn or test out these advanced features while learning to develop your own plug-ins.

* [feature-demo-parameters](https://github.com/surveycto/feature-demo-parameters)  
    Parameters can be used to pass values to a field plug-in (including values from other form fields).
* [feature-demo-metadata](https://github.com/surveycto/feature-demo-metadata)  
    Metadata can be used by your field plug-in to store data about itself while the form is being filled out.
* [feature-demo-intents](https://github.com/surveycto/feature-demo-intents)  
    Intents can be used by Android Collect to communicate with other apps on the device.
