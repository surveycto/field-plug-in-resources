# API Reference

## Table of contents

1. [Global APIs](#global-apis)
    1. [Field properties](#field-properties)
    1. [CSS classes](#css-classes)
    1. [Provided global JS functions](#provided-global-js-functions)
    1. [Called global JS functions](#called-global-js-functions)
1. [Field-specific APIs](#field-specific-apis)
    1. [Text field APIs](#text-field-apis)
    1. [Integer field APIs](#integer-field-apis)
    1. [Decimal field APIs](#decimal-field-apis)
    1. [Select_one and select_multiple field APIs](#select_one-and-select_multiple-field-apis)

Please note: the following is a list of APIs that we've developed to allow your field plug-in to interact with SurveyCTO. This list is in addition to all the existing functionality available by using normal HTML, CSS, and Javascript.

# Global APIs

## Field properties

When a field is rendered with a field plug-in, a set of field properties are made available to the field plug-in. This set contains all the information about the field from the form definition and the current form instance. You can access properties of this object in two different ways: special Mustache tags in the HTML, or as attributes of the *fieldProperties* object in JS. In general, these will all follow the same structure:

| Property name | How to reference in HTML | How to reference in JS |
| ---- | ---- | ---- |
| EXAMPLE | `{{EXAMPLE}}` | `fieldProperties.EXAMPLE` |

*Note: when using Mustache tags to return anything which might contain HTML, you should use `{{{three brackets}}}` instead of two. Field **labels** and field **hints** can contain HTML, so you should always use `{{{LABEL}}}` instead of `{{LABEL}}`. While “{{LABEL}}” will work when the field label is only plain text, it will not work properly if the field label contains HTML (because the HTML tags will be escaped).*

* `FIELDTYPE`  
    The simple field type of the current field, ignoring aliases and other things which may be in the *type* column in the form definition. Possible values are: `text`, `integer`, `decimal`, `select_one`, and `select_multiple`.

* `LANGUAGE`  
    The currently-selected language.

* `LABEL`  
    The field label from the form definition in the currently-selected language.

* `HINT`  
    The field hint from the form definition in the currently-selected language.

* `APPEARANCE`  
    The contents of the *appearance* column from the form definition.

* `CONSTRAINTMESSAGE`  
    The message which will be shown if a constraint is configured and violated. This will contain the default SurveyCTO constraint message if there is no *constraint message* specified in the form definition.

* `REQUIREDMESSAGE`  
    The message which will be shown if a field is marked *required* and the user attempts to skip it without giving an answer. This will contain the default SurveyCTO required message if there is no *required message* specified in the form definition.

* `QUESTION_PLACEHOLDER_LABEL`  
    This is the default placeholder text for supported field types, in the currently-selected language. For *text*, *integer*, and *decimal* fields, the English placeholder is "Your answer here...". For *select_one* fields using the *minimal* appearance, the placeholder is "Select one answer". 

* `READONLY`  
    A boolean representing whether or not a field is read only.

* `MEDIAIMAGE`  
    If there is an image file specified in the *media:image* column in the form definition, this will return the filename.

* `MEDIAAUDIO`  
    If there is an audio file specified in the *media:audio* column in the form definition, this will return the filename.

* `MEDIAVIDEO`  
    If there is a video file specified in the *media:video* column in the form definition, this will return the filename.

* `METADATA`  
    A place to store session metadata about the field. This value will be saved separately from the response data, and remain accessible to the form until it is finalized. Metadata can be accessed by your field plug-in using the `getMetaData()` JS function (see below), or by another field in the form using the `plug-in-metadata(field)` function (see [this page](https://docs.surveycto.com/02-designing-forms/01-core-concepts/09.expressions.html) for more info about form functions). Metadata will not show up in exported form data, but it will be present in the raw XML of submitted data (and could be seen by those inspecting SurveyCTO Desktop local storage). From a technical standpoint, the metadata is treated exactly like response data – it will be encrypted if the form and field are encrypted, and it will not be encrypted if the field is publishable.  
    *Note: field metadata will persist while navigating around different fields in the form, exiting and resuming the form, even resuming from a crashed form.*

* `PARAMETERS`  
    Form designers may pass custom parameters to your field plug-in. Each parameter will be a key/value pair defined in the *appearance* column of the form definition. Parameter values will be evaluated by the form before the field is rendered (before your field plug-in code is loaded), so you can use `${field}` references and expressions.  
    > **EXAMPLE (STATIC VALUES)**  
    > Field appearance:
    >
    >      custom-my-plugin(A=123.45, B='some static string')
    > `fieldProperties` object:  
    >
    >     ...
    >     "PARAMETERS":[
    >       {  
    >           "key":"A",  
    >           "value":123.45  
    >       },
    >       {
    >           "key":"B",
    >           "value":"some static string"  
    >       }
    >     ],
    >     ...

    > **EXAMPLE (DYNAMIC VALUES)**  
    > Field appearance:
    >
    >      custom-my-plugin(A=${firstname}, B=concat(${firstname}, ' ', ${lastname}))
    > `fieldProperties` object:  
    >
    >     ...
    >     "PARAMETERS":[
    >       {
    >           "key":"A",
    >           "value":"Jane"
    >       },
    >       {
    >           "key":"B",
    >           "value":"Jane Doe"
    >       }
    >     ],
    >     ...

* `PRECONFIGURED_INTENT`  
    A boolean representing whether or not a field is configured to communicate with an external app (via 'ex:' appearance).  
    *Note: this is feature available only in Android, so this flag will be always false in WebCollect and iOS Collect.*

*Note:* there may be additional properties attached to the field as well, which are specific to the field type. See the *Field-specific APIs* section below.

## CSS Classes

* `android-collect`  
    Added to the document level if the field plug-in is being rendered in Android.  

* `web-collect`  
    Added to the document level if the field plug-in is being rendered in WebCollect.  

* `ios-collect`  
    Added to the document level if the field plug-in is being rendered in iOS.  

* `is-read-only`  
    Added to the document level if the field is supposed to be readonly.  

* `default-question-text-size`  
    Use this class in your HTML templates to apply the default font size for question text. In Android and iOS, this will match "General settings -> Font size". In WebCollect, this will match the size used by native widgets.  
    > EXAMPLE
    >
    >      <p class=”default-question-text-size”>Here is my question.</p>
    > This will make your question text size dynamic, so that it can be controlled by the Collect device settings.

* `default-answer-text-size`  
    Use this class in your HTML templates to apply the default font size for answer text.  

* `default-hint-text-size`  
    Use this class in your HTML templates to apply the default size for hint text.  

* `language-[user-selected-language]`  
    Appended to the document level. Special characters and spaces in the language name are either converted to simple characters — when possible — or replaced with hyphens. For example: if the selected language is `idioma español`, the class name will be `language-idioma-espanol`.  For forms with a single language, the class name will be `language-default`.

## Provided global JS functions

Provided JS functions are ones that are provided to you, so that you may call them from your code.

* `goToNextField([skipValidation])`  
    Moves to the next question.
  * `goToNextField()` will attempt to validate the current field's response before moving to the next field.
  * `goToNextField(true)` will override the default behavior, and will skip the validation of the current field's response.
  * `goToNextField(false)` will function exactly the same as `goToNextField()`.

* `goToPreviousField([skipValidation])`  
   Moves back to the previous question.
  * `goToPreviousField()` will *not* attempt to validate the current field's response.
  * `goToPreviousField(true)` will function exactly the same as `goToPreviousField()`.
  * `goToPreviousField(false)` will override the default behavior, and attempt to validate the current response before moving to the previous field.

* `showSoftKeyboard()` (Android only)  
    Displays Android's soft keyboard.

* `setMetaData(string)`  
    Stores a `string` in the metadata for that field.  

* `getMetaData()`  
    Gets the current value stored in the field's metadata.

* `getPluginParameter(parameter)`  
    Returns the value of a given `parameter` passed to the field plug-in.

* `launchPreconfiguredIntent(callback(error, result))` (Android only)  
    Launches the external app configured via 'ex:' appearance and invokes the `callback` function either with an `error` or the `result` from the external app when it returns.  
    *Note: the field's current value is passed to the external app as a `value` parameter.*

* `launchIntent(intentName, parameters, callback(error, result))` (Android only)  
    Launches  an external app using the [Android intent class](https://developer.android.com/reference/android/content/Intent), given an `intentName` and a map of `parameters`. When this function is called, Collect will check the `parameters` map for a parameter named `uri_data`. The value of `uri_data` will be passed as the intent's `data` attribute. All other parameters (if any) will be passed as intent `extras`. Invokes the `callback` function either with an `error` or the `result` from the external app when it returns.

* `makePhoneCall(phone_number, phone_number_label, hide_phone_number, callback(error, result))` (Android only)  
    Places a phone call to the given `phone_number`. If _Collect_ is currently set as the default phone app, the call will be placed and managed by _Collect_. If _Collect_ is not currently the default phone app, and `hide_phone_number` is not set to `1`, then _Collect_ will call out with an intent so that the user can make the call with another phone app. For an example of how this works, take a look at the [phone-call](https://github.com/surveycto/phone-call) field plug-in.
  * `phone_number` The phone number to call.
  * `hide_phone_number` (optional) Set this to `1` to hide the phone number from the user. The phone number will not be shown anywhere in _Collect_. Please note that other phone apps on the device will still be able to access and display the phone number in _their_ call logs. If you want to make sure the phone number is not accessible in any way to the enumerator, you should disable all other phone apps on the device, and make sure the enumerator is using a SIM card that you control. If using their own SIM card, the enumerator will be able to contact their network provider to obtain call records containing all the phone numbers.
  * `phone_number_label` (optional) Supply an alternate label to use for the phone number when `hide_phone_number=1`.  

* `getPhoneCallStatus(callback(error, result))` (Android only)  
    If the phone is not in use, i.e. if there is no active call, this will return `-1`. If the phone is in use, this will return the integer state value from the [Android getState() method](https://developer.android.com/reference/android/telecom/Call#getState()). For an example of how this works, take a look at the [phone-call](https://github.com/surveycto/phone-call) field plug-in.

## Called global JS functions

Called JS functions are called by the form, and should be handled by your code.

* `clearAnswer()` (required)  
    This function should be defined in the plug-in’s JS code and it will be called when the field's answer is cleared.  
    > EXAMPLE  
    >
    >     var input = document.getElementById('txt');
    >     // When the user attempts to clear the answer, clear the value of the input element
    >
    >     function clearAnswer() {
    >       input.value = '';
    >     }

* `handleRequiredMessage(message)`  
    If defined, this will be called when a required field is left empty. The `message` parameter will contain the required message in the user-selected language.  

* `handleConstraintMessage(message)`  
    If defined, this will be called when a constraint fails for the field. The `message` parameter will contain the constraint message in the user-selected language.  

* `setFocus()`  
    If defined, this will be called to focus on the field.

# Field-specific APIs

These properties and functions only apply to certain field types.

## Text field APIs

### Properties

* `CURRENT_ANSWER`  
    Current text value.  
    `{{CURRENT_ANSWER}}` in HTML  
    `fieldProperties.CURRENT_ANSWER` in JS  

### Provided JS functions

* `setAnswer(newTextValue)` (required)  
    This should be called to change the field's answer to `newTextValue`.  
    *Note: this should be called whenever it’s desired to save a value for the field, i.e, the last value set here will persist even after skipping the field, saving the form, changing languages, navigating using the Go-To UI, etc.*

## Integer field APIs

### Properties

* `CURRENT_ANSWER`  
    Current integer value.  
    `{{CURRENT_ANSWER}}` in HTML  
    `fieldProperties.CURRENT_ANSWER` in JS  

### Provided JS functions

* `setAnswer(newIntegerValue)` (required)  
    This should be called to change the field's answer to `newIntegerValue`.  
    *Note: this should be called whenever it’s desired to save a value for the field, i.e, the last value set here will persist even after skipping the field, saving the form, changing languages, navigating using the Go-To UI, etc.*

## Decimal field APIs

### Properties

* `CURRENT_ANSWER`  
    Current decimal value.  
    `{{CURRENT_ANSWER}}` in HTML  
    `fieldProperties.CURRENT_ANSWER` in JS  

### Provided JS functions

* `setAnswer(newDecimalValue)` (required)  
    This should be called to change the field's answer to `newDecimalValue`. The decimal separator must be .(dot). If a non-valid decimal is provided, the value will default to empty.  
    *Note: this should be called whenever it’s desired to save a value for the field, i.e, the last value set here will persist even after skipping the field, saving the form, changing languages, navigating using the Go-To UI, etc.*

## Select_one and select_multiple field APIs

### Properties

* `CHOICES`  
    This is the set of choices that should be shown. It is an object containing a list of individual choices, each of which is its own object. In the HTML, you may use [Mustache’s ‘sections’ syntax](https://mustache.github.io/mustache.5.html) to loop over the set of choices.  
    `fieldProperties.CHOICES` in JS  
    > EXAMPLE (HTML)
    >
    >     {{#CHOICES}}
    >       <h3>{{CHOICE_LABEL}}</h3>
    >     {{/CHOICES}}
    > *Note: the above code will iterate over the list of choices, and create an \<h3> element for each choice in the choice list.*  

  * `CHOICE_VALUE`  
    This is the choice value, as it will appear in the exported data.  
    `{{CHOICE_VALUE}}` in HTML  
    `fieldProperties.CHOICES[i].CHOICE_VALUE` in JS

  * `CHOICE_INDEX`  
    This is the index number of the choice, based on the order it should appear in the form. Choice index values start at 0 for the first choice, then 1 for the second choice, etc…  
    `{{CHOICE_INDEX}}` in HTML  
    `fieldProperties.CHOICES[i].CHOICE_INDEX` in JS

  * `CHOICE_LABEL`  
    This is the choice label, in the currently-selected language.  
    `{{CHOICE_LABEL}}` in HTML  
    `fieldProperties.CHOICES[i].CHOICE_LABEL` in JS  

  * `CHOICE_SELECTED`  
    Whether or not the current choice is selected (true|false).  
    `{{CHOICE_SELECTED}}` in HTML  
    `fieldProperties.CHOICES[i].CHOICE_SELECTED` in JS

  * `CHOICE_IMAGE`  
    If the choice has an image, this will return its filename.  
    `{{CHOICE_IMAGE}}` in HTML  
    `fieldProperties.CHOICES[i].CHOICE_IMAGE` in JS  

> EXAMPLE  
> This is the structure of the `CHOICES` object
>
>     CHOICES:[ 
>       { 
>           CHOICE_VALUE: "opt_1",
>           CHOICE_INDEX: 0,
>           CHOICE_LABEL: "Option 1",
>           CHOICE_SELECTED: true|false,
>           CHOICE_IMAGE: "option1.png"
>       },
>       { 
>           CHOICE_VALUE: "opt_2",
>           CHOICE_INDEX: 1,
>           CHOICE_LABEL: "Option 2",
>           CHOICE_SELECTED: true|false,
>           CHOICE_IMAGE: "option2.png"
>       },
>         { 
>           CHOICE_VALUE: "opt_3",
>           CHOICE_INDEX: 2,
>           CHOICE_LABEL: "Option 3",
>           CHOICE_SELECTED: true|false,
>           CHOICE_IMAGE: "option3.png"
>         }
>     ]

### Provided JS functions

* `setAnswer(newSelectedChoiceValue)` (required)  
    This should be called to change the field's answer.  

    For *select_one* fields, the choice value of the selected choice should be passed as a single value. If a non-valid choice value is passed, the value will default to null/no-selected choice. Example: `setAnswer(opt_1)`  

    For *select_multiple* fields, the choice values of all selected choices should be passed as a space-separated list of values.  If a non-valid list of choice values is passed, the value will default to null/no-selected choice. Example: `setAnswer(opt_1 opt_3 opt_2)`  

    *Note: this should be called whenever it’s desired to save a value for the field, i.e, the last value set here will persist even after skipping the field, saving the form, changing languages, navigating using the Go-To UI, etc.*
