# Field plug-in definition

We will be using the term “plug-in definition” interchangeably with “field plug-in” to refer to the x.fieldplugin.zip file and all of its contents. Similarly to how a form definition is a file (or set of files) which contains all the information needed to load and fill out a form, a plug-in definition is a .zip file which contains all the information needed to use the field plug-in.

> **File names for field plug-ins**
>
> * Are case-insensitive.
> * Must only contain letters, numbers, periods, dashes, and underscores.
> * Must start with a letter.
> * Must not exceed 100 characters.

## Required files

| Filename | Description |
| ---- | ----|
| `manifest.json` | This file contains all the meta-information about the plug-in. |
| `template.html` | This file is used to define the basic structure of the field’s appearance. |
| `style.css` | This file is used to define the visual styles of the field’s appearance. |
| `script.js` | This file is used to add functionality and interactivity to a field. |

*Note: These four required files must be named exactly as listed above.*

> **File names for other files within field plug-ins**
>
> * Are case-sensitive.
> * Must only contain letters, numbers, periods, dashes, and underscores.
> * Must start with a letter.
> * Must not exceed 100 characters.

## manifest.json

This file contains all the meta-information about the plug-in. Here is a list of the attributes contained in the manifest.json file:

| Attribute | Description |
| ------------- |------------- |
| `name` * | A human-readable version of the plug-in name *(maximum length 100 characters)*|
| `author` * | The author(s) of the plug-in |
| `version` * | The plug-in version. This should follow the [semver](https://semver.org/) spec. This will be used when updating plug-ins |
| `supportedFieldTypes` * | A list of field types supported by the plug-in. Currently, these may only include the following options: `text`, `integer`, `decimal`, `select_one`, `select_multiple`. |
| `externalCss` | Optional: a list of additional CSS files to load |
| `externalJs` | Optional: a list of additional Javascript files to load |
| `hideDefaultRequiredMessage` | Optional: whether to disable the native error message when a required field is left blank. This may be useful when the field plug-in implements its own logic for that. If set as true, the default toast notification (in Android and in iOS) and alert bar (WebCollect) which appear when a required field is left blank will be omitted. Defaults to false. |
| `hideDefaultConstraintMessage` | Optional: whether to disable the native error message when there’s a constraint violation. This may be useful when the field plug-in implements its own logic for that. If set as true, the default toast notification (in Android and in iOS) and alert bar (WebCollect) which appear when a constraint fails will be omitted. Defaults to false. |

\* *An asterisk indicates that the attribute is required*

> **EXAMPLE MANIFEST.JSON**  
>
>     {
>         "name": "Field plug-in example",
>         "author": "Dobility",
>         "version": "1.0",
>         "supportedFieldTypes": [ "text", "integer", "decimal" ],
>         "externalCss": [ "extra1.css", "extra2.css" ],
>         "externalJs": [ "library1.js" ],
>         "hideDefaultRequiredMessage": true,
>         "hideDefaultConstraintMessage": true
>     }

## template.html

This file is used to define the basic structure of the field’s appearance. Mustache tags and tag syntax are supported in this file. For a list of all available tags, please see Global APIs and Field-specific APIs below.

> **EXAMPLE TEMPLATE.HTML**  
>
>     <h3>{{{LABEL}}}</h3>
>     <small>{{{HINT}}}</small>
>     <input value=”{{CURRENT_ANSWER}}” id=”txt” />

*Note: to ensure compatibility with HTML in field labels and field hints, it is best to use three brackets: `{{{LABEL}}}`. While “{{LABEL}}” will work when the field label is only plain text, it will not work properly if the field label contains HTML (because the HTML tags will be escaped).*

## style.css

This file is used to define the visual styles of the field’s appearance. This file is interpreted after all external CSS files (if any).

> **EXAMPLE STYLE.CSS**
>
>     input {
>         color: green;
>         background-color: black;
>     }

## script.js

This file is used to add functionality and interactivity to a field. This file is interpreted after all external Javascript files (if any).

> **EXAMPLE SCRIPT.JS**
>
>     var input = document.getElementById('txt');
>         function clearAnswer() {
>         input.value = '';
>     }

## Optional files

### External CSS files

Besides the plug-in's core CSS file (`style.css`), additional CSS files can be loaded if they are specified in the `externalCss` attribute of the `manifest.json` file. The CSS files will be loaded in the order they've been specified and will load BEFORE the plug-in's own CSS code. This can be useful for including CSS libraries from third-parties in your field plug-in.

### External Javascript files

Besides the plug-in's core JavaScript file (`script.js`), additional JS files can be loaded as specified in the `externalJs` attribute of the `manifest.json` file. The JS files will be loaded in the order they've been specified and will load BEFORE the plug-in's own JS code. This can be useful for including JS libraries from third-parties in your field plug-in.

### Plug-in attachments

In addition to the files loaded automatically by `manifest.json`, you might want to use other files in your plug-in (like images). You can include extra files in your .fieldplugin.zip root directory, and reference them using a special `{{PLUGINDIR}}` variable.

*Note: the pattern `{{PLUGINDIR}}` is replaced with the actual path before rendering the core HTML, CSS and JS for the plug-in using a simple search-and-replace logic.*

*Note: subdirectories within field plug-ins are not supported. Please keep all files in the root of the .zip directory.*

> **EXAMPLE (HTML)**
>
>     <img src="{{PLUGINDIR}}/image.png"/> 

> **EXAMPLE (CSS)**
>
>     .img {
>         background-image: url("{{PLUGINDIR}}/image.png")
>     }

> **EXAMPLE (JS)**
>
>     img.src = "{{PLUGINDIR}}/image.png"

### Form attachments

Field plug-ins can access any file attached to the form definition simply by specifying the filename.

For example: if you have a "image.png" attached directly to the form definition, you can use it like:

> **EXAMPLE (HTML)**
>
>     <img src="image.png"/> 

> **EXAMPLE (CSS)**
>
>     .img {
>         background-image: url("image.png")
>     }

> **EXAMPLE (JS)**
>
>     img.src = "image.png"