# Global APIs

## Field properties

*Note: properties can be used in HTML as Mustache tags, or in JS as attributes of the fieldProperties object.*

*Note: to ensure compatibility with HTML in field labels and field hints, it is best to use three brackets, like this: `{{{LABEL}}}`. While “{{LABEL}}” will work when the field label is only plain text, it will not work properly if the field label contains HTML (because the HTML tags will be escaped).*

* `LABEL`  
    The field label from the form definition in the currently-selected language.  
    `{{{LABEL}}}` in HTML  
    `fieldProperties.LABEL` in JS  

* `HINT`  
    The field hint from the form definition in the currently-selected language.  
    `{{{HINT}}}` in HTML  
    `fieldProperties.HINT` in JS  

* `LANGUAGE`  
    The currently-selected language.
    `{{LANGUAGE}}` in HTML  
    `fieldProperties.LANGUAGE` in JS

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

## Provided JS functions

Provided JS functions are ones that are provided to you, so that you may call them from your code. 

* `goToPrevousField()`  
    Moves to the previous question, skipping any validation for the current field's answer.  

* `goToNextField()`  
    Moves to the next question, skipping any validation for the current field's answer.  
    > EXAMPLE  
    > 
    >     // Find the button in your plug-in template with id=”skipNext”  
    >     var skipNext = document.getElementById('skipNext');
    >     
    >     // When that button is clicked, go to the next field.
    >     skipNext.onclick = function() {
    >       goToNextField();
    >     } 

* `showSoftKeyboard()` (Android only)  
    Displays Android's soft keyboard.

## Called JS functions

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
    If defined, this will be called when a required field is left empty. The message parameter will contain the required message in the user-selected language.  

* `handleConstraintMessage(message)`  
    If defined, this will be called when a constraint fails for the field. The message parameter will contain the constraint message in the user-selected language.  

* `setFocus()`  
    If defined, this will be called to focus on the field.

# Field-specific APIs

These properties and functions only apply to certain field types.

## Text fields

### Properties

* `CURENT_ANSWER`  
    Current text value.  
    `{{CURRENT_ANSWER}}` in HTML  
    `fieldProperties.CURRENT_ANSWER` in JS  

### Provided JS functions

* `setAnswer(newTextValue)` (required)  
    This should be called to change the field's answer to `newTextValue`.  
    *Note: this should be called whenever it’s desired to save a value for the field, i.e, the last value set here will persist even after skipping the field, saving the form, changing languages, navigating using the Go-To UI, etc.*

## Integer fields

### Properties

* `CURENT_ANSWER`  
    Current integer value.  
    `{{CURRENT_ANSWER}}` in HTML  
    `fieldProperties.CURRENT_ANSWER` in JS  

### Provided JS functions

* `setAnswer(newIntegerValue)` (required)  
    This should be called to change the field's answer to `newIntegerValue`.  
    *Note: this should be called whenever it’s desired to save a value for the field, i.e, the last value set here will persist even after skipping the field, saving the form, changing languages, navigating using the Go-To UI, etc.*

## Decimal fields

### Properties

* `CURENT_ANSWER`  
    Current decimal value.  
    `{{CURRENT_ANSWER}}` in HTML  
    `fieldProperties.CURRENT_ANSWER` in JS  

### Provided JS functions

* `setAnswer(newDecimalValue)` (required)  
    This should be called to change the field's answer to `newDecimalValue`. The decimal separator must be .(dot). If a non-valid decimal is provided, the value will default to empty.  
    *Note: this should be called whenever it’s desired to save a value for the field, i.e, the last value set here will persist even after skipping the field, saving the form, changing languages, navigating using the Go-To UI, etc.*

## Select_one and select_multiple fields

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
