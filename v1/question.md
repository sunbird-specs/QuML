## Question Information Model

This document has the following sections:

1. Question Data Model - the core that defines the actual question data exchange (including the content itself)
2. Question Metadata - QML specific metadata
3. Usage data and Results Reporting - statistical data about the usage and reporting of the results assigned by outcomes and response processing of the question

### Table of contents
1. [Question Data Model](#Question-Data-Model)
    1. [Body](#Body)
2. [Question Metadata](#question-metadata)
3. [Usage Data and Results Reporting](#Usage-Data-and-Results-Reporting)

### Question Data Model

A QML Question comprises of the information model and associated binding that can be used to store, represent and deliver questions. A question can be defined as a set of interactions (possibly empty) collected together with any supporting material and an optional set of rules for converting the student’s response(s) into outcomes. Question contains the definition for the interactions, variables to store responses, logic to process the responses, outcome data and other components that are needed for question delivery.

QML specification makes use of web standards like HTML, CSS, JSON, and Javascript to store the question data. The below diagram shows the components within a question and format used to store/represent the component.

![Structure of a Question](https://github.com/sunbird-specs/qml/blob/master/v1/images/Question_Structure.png)

#### Body
Question body contains the text, graphics, media objects and interactions that describe the question’s content and information about how it is structured. The body is presented by combining it with stylesheet and/or internationalization information, either explicitly or implicitly using the default style rules of the QML player.

The structural elements of the item body are taken from HTML with the following restrictions:
- It should contian only **structural and input HTML elements**. Structural elements include \<div>, \<span>, \<ul> and \<li>. Input html elements are text input, text area, select, options, checkbox, radio buttons, file upload and canvas elements.
- It should not contain a **\<form> element**
- It should not have any **javascript code**, i.e. \<script> elements or onXYZ (onClick, etc) for HTML elements should not be used
- It should not have any **javascript import statements**
- It should not have any **css import statements**

In addition to the standard HTML elements and attributes, the specification allows the following in the question body: 
- Usage for “data-” attributes to specify the interactions on HTML elements, bind response variables to the interactions, use asset, template and i18n variables.
- Usage of standard style classes defined by the specificaiton. QML players should provide implementation for the defined classes.

Body:
```
{
	“itemBody”: “<HTML>...</HTML>”
}
```

#### Interactions
A question can contain zero or more interactions with the user. Interactions can be defined in the item body using HTML data attributes as shown in the below sample (**“data-<interaction_name>-interaction”**). 

```
<input type="checkbox" name="element" data-multi-choice-interaction data-response-variable="response_01" value="Oxygen">
```

QML specification defines the following interaction types. QML players should provide the implementation for all the defines interaction types. The behaviour of the interaction may vary in each implementation but should ensure that the defined specification is supported.

##### Simple Choice Interaction

Simple choice interaction is used when a set of choices are presented to the student. The student’s task is to select one of the choices. This interaction must be bound to a response variable with single cardinality.

| **Data Attribute** | **data-simple-choice-interaction** |
| --- | ----- |
| cardinality | *single* |
| type | string, integer or float |
| html elements | any HTML element which is clickable and have a value attribute |

##### Multi Choice Interaction

Multi choice interaction is used when a set of choices are presented to the candidate. The candidate’s task is to select one or more of the choices. This interaction must be bound to a response variable with multiple cardinality. An array variable will be used to store the values for response variables of this interaction.

| **Data Attribute** | **data-multi-choice-interaction** |
| --- | ----- |
| cardinality | *multiple* |
| type | string, integer or float |
| html elements | any HTML element which is clickable and have a value attribute |

##### Text Interaction

A text interaction obtains a piece of text from the candidate. The QML player must allow the candidate to review their choice within the context of the question. The text interaction must be bound to a response variable with single cardinality only. The type must be one of string, integer, or float. 

| **Data Attribute** | **data-text-interaction** |
| --- | ----- |
| cardinality | *string* |
| type | string, integer or float |
| html elements | HTML elements where user can enter text, i.e. text input, or text area |

##### Order Interaction

In an order interaction, the candidate’s task is to reorder the choices. When specified, the candidate must select the choices and impose an ordering on them. If a default value is specified for the response variable associated with an order interaction then its value should be used to override the order of the choices specified here. The order interaction must be bound to a response variable with ordered cardinality and type must be one of string, integer or float.

| **Data Attribute** | **data-ordered-interaction** |
| --- | ----- |
| cardinality | *ordered* |
| type | string, integer or float |
| html elements | \<ul> element. The values of all \<li> elements should be set in the same order as the value for response variable bound with the interaction |
	
> The order interaction implementation in the player should enable drag and drop of the \<li> elements in the \<ul> element with this interaction. QML players may choose to provide enhanced graphics/animation for order interaction.

##### Match Interaction

A match interaction presents candidates with two sets of choices and allows them to create associates between pairs of choices in the two sets, but not between pairs of choices in the same set. The match interaction must be bound to a response variable with type map and single cardinality.

| **Data Attribute** | **data-match-interaction** |
| --- | ----- |
| cardinality | *single* |
| type | map |
| html elements | Two \<ul> elements. Both the \<ul> elements should be bound to the same response variable. The list of values of \<li> element pairs (mapped with each other) should be set as the value of the response variable. |
	
> Similar to order interaction, the match interaction implementation in the player should enable drag and drop of \<li> elements in one \<ul> element on to \<li> elements in another \<ul> element with this interaction. QML players may choose to provide enhanced graphics/animation for match interaction.

##### Upload Interaction

The upload interaction allows the candidate to upload a pre-prepared file representing their response. It must be bound to a response variable with type file and single cardinality.

| **Data Attribute** | **data-upload-interaction** |
| --- | ----- |
| cardinality | *single* |
| type | uri |
| html elements | HTML file upload element |
	
> The upload interaction implementation in the player should upload the input file to a configured server and set the uploaded file url as the value for the response variable.

##### Map Interaction

The map interaction is a graphic interaction. The candidate’s task is to select one or more points on a canvas. The associated response may have a mapping that scores the response on the basis of comparing it against predefined areas but QML players must not indicate these areas of the image. Only the actual point(s) selected by the candidate shall be indicated. The map interaction must be bound to a response variable with type points and multiple cardinality.

| **Data Attribute** | **data-map-interaction** |
| --- | ----- |
| cardinality | *multiple* |
| type | points |
| html elements | HTML canvas element |

##### Custom Interaction

QML implementations can implement a set of custom interactions that go beyond the standard interactions described above. Authors wishing to write questions for those implementations can use these custom interactions. However these custom templates should be published to ensure that these questions can be used with QML players of other implementations. Otherwise, such questions will be limited to use only by that implementation.

#### Response Variables
Response variables are used to capture the response of candidate. These variables are generated as a result of an interaction of candidate with the question.

Response variables can be used within the question body using the html data attribute: “data-response-variable”. This data attribute should be set on HTML elements that are used by candidates for interacting with the question. And when the candidate uses the HTML element for interaction, the corresponding response variable should be updated with the value of the HTML element. 

```
<input type="checkbox" name="element" data-response-variable="response_01" value="Oxygen" data-multi-choice-interaction>
```

In the above sample, there is response variable named “response_01” defined on the HTML checkbox element. When a candidate selects the checkbox, the value of the checkbox element (Oxygen, in this example) is set as the value for the variable “response_01”. 

##### Built-in Response Variables
There are two built-in response variables: 'numAttempts' and 'duration'. These are declared implicitly and should be updated by the QML player when a candidate is interacting with the question.

- **numAttempts**: An integer that records the number of attempts at each question the candidate has taken (if multiple attempts are allowed in the context where the question is being used).
- **duration**: A float that records the accumulated time (in seconds) of all candidate sessions for all attempts. In other words, the time between the beginning and the end of the question session minus any time the session was in the suspended state. 

#### Asset Variables
Asset variables are used to load assets during the rendering of a question. Values for asset variables are declared in “asset declarations” and bound to HTML elements in the question. Asset variables can be used in a question using the html data attribute: **“data-asset-variable”**. This data attribute should be set on HTML elements that are used for rendering assets (e.g. img, audio and video elements). And when the question is being rendered, the value of asset variable is set as the source of the HTML element on which the asset variable is declared.

```
<img src="assets/apple.png" data-asset-variable="asset_01" >
```

Optionally, a default source can also be provided for the asset HTML elements. The default gets overridden by the value from asset declaration at the time of rendering the question. If an asset variable is not declared or a value is not set in asset declaration, the default value of the source will be used by the QML players.

> QML implementations should upload assets to the QML repository server and only relative paths should be used in asset variables and asset declarations. QML players should proxy the relative paths to render the assets either from the QML repository server or from local storage (offline mode).

#### i18n Variables
i18n variables can be referred to by i18nVariable objects in the question body, instructions, answers, feedback, and hints. The text, that needs to be localized, should be mapped with an i18n variable using the data attribute **“data-i18n-variable”**. This data attribute should be set on a \<span> element around a text that needs to be localised. And when the question session starts, the text within the \<span> gets replaced with the locale specific value of the text.

```
<span data-i18n-variable="text_1">Ram has 3 apples</span>.
```

#### Template Variables
Template variables can be referred to by using the html data attribute **“data-template-variable”** in the question body. The value of the template variable is used to create an appropriate run of text that is displayed. This data attribute should be set on a \<span> element around a text that needs to be templatized. And when the question session starts, the text within the \<span> gets replaced with the value generated by template processing rule for the corresponding template variable.

```
Shyam has <span data-template-variable="template_var_fruit_number_1">5</span> <span data-template-variable="template_var_fruit_name">apples</span>.

Ram borrowed <span data-template-variable="template_var_fruit_number_2">3</span> <span data-template-variable="template_var_fruit_name">apples</span> from Shyam.

Shyam now has <input type="text" data-text-interaction data-response-variable="response_01" /> <span data-template-variable="template_var_fruit_name">apples</span>.
```

The above question is for subtraction, where the learner is asked to identify the number of apples Shyam has after Ram borrowed 3 of the 5 apples which Shyam had. Instead of showing the same numbers and fruit name always, the numbers and name of the fruit is templatized in the item body using template variables.

#### Outcome Variables
Outcome variables are declared by outcome declarations. Their value is set either from a default given in the declaration itself or during “response processing”. These variables are used for returning the score or for controlling the feedback to the student.

There are six reserved outcome variables:

- Questions that declare a numeric outcome variable representing the student’s overall performance on the question should use the outcome name **‘SCORE’** for the variable. SCORE needs to be a float.
- Questions that provide a feedback to student after the attempt should use the outcome variable **‘FEEDBACK’** to show the appropriate feedback modal. FEEDBACK needs to be a string.
- Questions that have in-built hints to help students in attempting the question should use the outcome variable **‘HINT’**. The value of this variable should be used by the QML players to render the corresponding hint modal. HINT needs to be a string.
- Questions that declare a maximum score (in multiple response choice questions, for example) should do so by declaring the **‘MAXSCORE’** variable. MAXSCORE needs to be a float.
- Questions that declare a minimum score (minimum score to mark the student as passed) should do so by declaring the **‘MINSCORE’** variable. MINSCORE needs to be a float.
- Questions or tests that want to make the fact that the student scored above a predefined threshold available as a variable should use the **‘PASSED’** variable. PASSED needs to be a boolean. This variable is automatically set after response processing using the SCORE and MINSCORE outcome variables.

##### Built-in Outcome Variables
There is one built-in outcome variable, **‘completionStatus’**, that is declared implicitly and must not appear in outcome variables declaration. QML players must maintain the value of the built-in outcome variable completionStatus. It starts with the reserved value “not_attempted”. At the start of the first attempt, it changes to the reserved value “unknown”. It remains with this value for the duration of the question session unless set to a different value by a setOutcomeValue in responseProcessing. This variable changes to the reserved value “completed” by default if it is not updated during response processing. 

There are four permitted values:
1. ‘completed’ - the student has experienced enough of the question to consider it completed;
2. ‘incomplete’ - the student has not experienced enough of the question to consider it completed;
3. ‘not_attempted’ - the student is considered to have not used the question in any significant way;
4. ‘unknown’ - no assertion on the state of completion can be made.

#### Style classes

#### Instructions
Some questions will have instructions of how to understand, attempt or how the question will be evaluated. Such instructions are defined in HTML format and stored in the instructions part of the question data. Instructions can contain i18n variables to support internationalization.

Instructions:
```
{
	“instructions”: “<HTML>... … … </HTML>”
}
```

#### i18n Data
Internationalization is supported by QML to render texts inside body (instructions, answers, feedback and hints) in different locales. QML players should render all i18n variables in the selected locale. If localised value for a variable is not present in one of the locales, QML players should use the default value (provided for the variable in the declaration).

The i18n data should be stored in the question data in JSON format. The values for each of the i18n variables should be provided for each of the supported locales.

i18n:
```
{
	“i18n”: {
		“<locale_1>”: {
			“<i18_variable_1>”: “<localised_text>”,
			… 
		},
		“<locale_2>”: {
			“<i18_variable_1>”: “<localised_text>”,
			… 
		},
		… 
	}
}
```

#### Asset Declaration
An **“assetDeclaration”** contains information about the assets used in the question. QML players should load the declared assets before rendering the question (before interacting state). Asset Declaration should have declaration for every asset variable used in the question. Asset declaration is defined in JSON format and stored in the assetDeclaration part of the question data.

Asset Declaration:
```
{
	“assetDeclaration”: {
		“<asset_01>”: Asset object,
		“<asset_02>”: Asset object,
		… 
	}
}
```

#### Response Declaration
A **“responseDeclaration”** contains information about the answer (the response) to a question: When is it correct, and (optionally) how is it scored? 

Response Declaration should have declaration for every response variable in the question body. Response declaration is a JSON object in key-value format. The keys in the JSON are the response variables defined in the body and values are of type ResponseVariableDef.

ResponseDeclaration:
```
{
	“responseDeclaration”: {
		“<response_variable_1>”: ResponseVariableDef Object,
		“<response_variable_2>”: ResponseVariableDef Object,
		… 
	}
}
```

*ResponseVariableDef:*

Each response variable should have exactly one response variable definition in the response declaration.

| Attribute | Schema | Description |
| --- | ----- | ----------- |
| cardinality | dataType: string, required: true, range: “single”, “multiple”, “ordered” | Used to specify whether the response variable will have a single value, multiple values or an ordered list of values. |
| type | dataType: string, required: true, range: “string”, “integer”, “float”, “boolean”, “map”, “uri”, “points”, “coordinate” |  |
| correctResponse | dataType: CorrectResponseDef, required: false |  |
| mapping | dataType: MappingDef, required: false | Map different answers to scores so that an answer can have more nuances than plain right or wrong |
| areaMapping | dataType: AreaMappingDef, required: false | Similar to mapping attribute, but for response variables associated to map interactions |

*CorrectResponseDef:*

| Attribute | Schema | Description |
| --- | ----- | ----------- |
| value | dataType: any, required: true |  |


*MappingDef:*

| Attribute | Schema | Description |
| --- | ----- | ----------- |
| key | dataType: any, required: true |  |
| value | dataType: float, required: true |  |
| caseSensitive | dataType: boolean, required: false, defaultValue: false |  |

*AreaMappingDef:*

| Attribute | Schema | Description |
| --- | ----- | ----------- |
| key | dataType: points, required: true |  |
| value | dataType: float, required: true |  |

#### Outcome Declaration
An “outcomeDeclaration” contains information about the outcome variables of the question, i.e the values that are output of a question session (QML player should make the outcome variables available to the context in which the question is being used).

Outcome declaration is a JSON object in key-value format. The keys in the JSON are the outcome variables and values are of type OutcomeVariableDef.

OutcomeDeclaration:
```
{
	“outcomeDeclaration”: {
		“<outcome_variable_1>”: OutcomeVariableDef Object,
		“<outcome_variable_2>”: OutcomeVariableDef Object,
		… 
	}
}
```

*OutcomeVariableDef:*

Each outcome variable should have exactly one outcome variable definition in the outcome declaration.

| Attribute | Schema | Description |
| --- | ----- | ----------- |
| cardinality | dataType: string, required: true, range: “single”, “multiple”, “ordered” | Used to specify whether the outcome variable will have a single value, multiple values or an ordered list of values. |
| type | dataType: string, required: true, range: “string”, “integer”, “float”, “boolean”, “map”, “uri”, “points”, “coordinate” |  |
| defaultValue | dataType: any, required: false |  |
| range | dataType: List of any, required: false |  |

#### Feedback
Feedback is a JSON object in key-value format. The keys in the JSON are the identifiers of different feedbacks for the question and values are HTML snippet to be shown to the candidate. After the response processing, the QML player renders the feedback HTML mapped to the value that is set to the FEEDBACK outcome variable. Feedback can contain i18n variables to support internationalization.

Feedback:

```
{
	“feedback”: {
		“<feedback_1>”: “<HTML>...</HTML>”,
		“<feedback_2>”: “<HTML>...</HTML>”,
		… 
	}
}
```

#### Hints
Hints are shown to the candidates after response processing or when the student requests for hints.
- HINT outcome variable value is used to render the hint after response processing.
- Alternatively, QML players can provide an option for students to request for hints. Upon request, the QML player renders all the configured hints in a sequence (as defined in the hints configuration).

In either case, hints are shown only if they are allowed in the context where the question is being used. Hints is a JSON object in key-value format. The keys in the JSON are the identifiers of different hints for the question and values are HTML snippet for hints.

Hints is a JSON object in key-value format. The keys in the JSON are the identifiers of different hints for the question and values are HTML snippet for hints. Hints can contain i18n variables to support internationalization.

Hints:

```
{
	“hints”: {
		“<hint_1>”: “<HTML>...</HTML>”,
		“<hint_2>”: “<HTML>...</HTML>”
		… 
	}
}
```

#### Answers
Providing exemplar answers for questions aid candidates in-depth learning and enhance candidate’s understanding of the concepts. These answers are mainly helpful for candidates in preparing for exams. Multiple answers can be configured for a question and each answer can have one or more parts, each part could be either text, rich text (HTML5), or a set of assets (images, audios, or videos).

Answers:

```
{
	“answers”: [
	{
		"parts": [{
				"body": “<HTML>...</HTML>”,
				"assets": [list of Asset objects],
				"comment": "optional comment for the answer part"
			},
			{ ... }
		]
		“comment”: “optional comment for the answer”
	},
	{ ... }
	]
}
```

#### Response Processing
There are two ways of configuring response processing:
1. Question authors can provide custom response processing logic as javascript code for their questions. This logic can use the available library methods to get/set question variables.
2. Question authors can use/configure one of the provided response processing templates to process the response of the question.

##### Custom Response Processing
The custom response processing logic using javascript should be defined as part of the “eval” attribute of “responseProcessing” data. 

Sample Custom Response Processing:

```
{
    “responseProcessing”: {
           “eval”: “
                    <!-- mapResponse() is a built-in library method --> 
                    mapResponse();
                    var score = getOutcomeVariable(\“SCORE\”);
                    If (score >= 1.0) {
                            setOutcomeVariable(\“FEEDBACK\”, “feedback_01”);
                    } else if (score > 0 && score < 1.0) {
                            setOutcomeVariable(\“FEEDBACK\”, “feedback_02”);
                    } else if (score <= 0) {
                            setOutcomeVariable(\“FEEDBACK\”, “feedback_03”);
                    }
            “
    }
}
```

> custom “eval” data is mandatory if response processing templates are not used for the question. If both template and evaluation logic are present, template is executed first and then the custom evaluation logic is executed.

##### Response Processing Templates
Custom response processing involves the application of a set of response rules, including the testing of response conditions and the evaluation of logic involving the question variables. For QML implementations that are only designed to support very simple use cases, the implementation of a system for carrying out this evaluation, conditional testing and processing may pose a barrier to the adoption of the specification.

To alleviate this problem, the implementation of generalized response processing is an optional feature. Players that do not support it can instead implement a smaller number of standard response processors called response processing templates. These templates are also defined using javascript for evaluation of logic and should be implemented by the QML implementation. 

Following are two standard response processing templates defined in QML (an additional template for response processing using templates is defined in the Template Processing section):

**MATCH_CORRECT**

This response processing template matches the value of response variables with their correct value using correctAnswer attribute defined in responseDeclaration. It sets the outcome variable SCORE to either 0 or 1 depending on the outcome of the test. To use this template, all response variables must have been declared and have an associated correct value. Similarly, the outcome variable SCORE must also have been declared.

**MAP_RESPONSE**

The map response processing template uses the mapping and areaMapping definitions of response variables in responseDefinition to set value for the outcome SCORE. To use this template, all response variables must have been declared and must have an associated mapping. Similarly, the outcome variable SCORE must also have been declared.

The map response processing template optionally takes a mapping config as input. This config is to set other outcome variables based on the value of SCORE outcome variable. If no config is provided, these templates set the value of SCORE outcome only. The structure of mapping config is as shown below.

*MappingConfig:*

Mapping config should be provided in JSON format. It comprises of the list of outcome variables and their values for various possible scores.

```
{
<!-- mappingConfig is a list of configurations for different possible SCORE values -->
    “mappingConfig”: [
           {
<!-- each config has a SCORE section to define the range of SCORE values and an outcomeVariables section to define the values to be set when the SCORE falls in the defined range -->
                 “SCORE”: {
<!-- SCORE definition allows the usage of the operators: le (less than or equal to), lt (less than), eq (equals to), ge (greater than or equals to), gt (greater than) and in (in a list of values). One operator can be used only once in a SCORE definition. e.g.: “lt”: 1.0-->
                          “<operator_1>”: <float value>,
			  “<operator_2>”: <float value>, ...
<!-- alternatively, a regex can also be used to define the SCORE range-->
                          “regex”: <regular expression>
                 },
                 “outcomeVariables”: {
<!-- list of outcome variables, defined in outcomeDeclaration, and the values to be set for each of the variables when the computed SCORE falls in the defined range -->
                          “<outcome_variable>”: <value>
                 }
           }
    ]
}

```

Following is the sample usage of response processing templates. The sample javascript response processing provided above (in the Custom Response Processing section) can also be done using MAP_RESPONSE template:

```
{
    “responseProcessing”: {
           “template”: “MAP_RESPONSE”,
           “mappingConfig”: [
                   {
                           “SCORE”: {“ge”: 1.0},
                           “outcomeVariables”: {“FEEDBACK”: “feedback_01”}
                   },
                   {
                           “SCORE”: {“gt”: 0, “lt”: 1},
                           “outcomeVariables”: {“FEEDBACK”: “feedback_02”}
                   },
                   {
                           “SCORE”: {“le”: 1.0},
                           “outcomeVariables”: {“FEEDBACK”: “feedback_03”}
                   }
           ]
    }
}
```

> QML implementations that do not support custom response processing but do support response processing mechanisms that go beyond the standard templates described above can define templates of their own. Authors wishing to write questions for those implementations can then refer to these custom templates. Publishing these custom templates will then ensure that these questions can be used with QML players that do support generalized response processing.

#### Template Declaration
Template declarations declare variables that are to be used specifically for the purposes of cloning items. They can have their value set only during templateProcessing. They are referred to within the question body in order to individualize the clone and possibly also within the responseProcessing rules if the cloning process affects the way the question is scored.

Template declaration is a JSON object in key-value format. The keys in the JSON are the template variables used in the question and values are of type TemplateVariableDef. 

TemplateDeclaration:

```
{
	“templateDeclaration”: {
		“<template_variable_1>”: TemplateVariableDef Object,
		“<template_variable_2>”: TemplateVariableDef Object,
		… 
	}
}
```

*TemplateVariableDef:*

Each template variable should have exactly one template variable definition in the template declaration.

| Attribute | Schema | Description |
| --- | ----- | ----------- |
| cardinality | dataType: string, required: true, range: “single”, “multiple”, “ordered” | Used to specify whether the template variable will have a single value, multiple values or an ordered list of values. |
| type | dataType: string, required: true, range: “string”, “integer”, “float”, “boolean”, “map”, “uri”, “points”, “coordinate” |  |
| defaultValue | dataType: any, required: false |  |

A question can have template variable that are not used in the question “body”. There will be cases when some template variables are needed for template and/or response processing but not visible for the candidate. Below is a sample template declaration. In addition to the template variables used in the body, there is an additional template variable “template_var_temp_number” defined. This additional template variable can be used in both template and response processing.

```
{
    “templateDeclaration”: {
            “template_var_fruit_number_1”: {
                    “cardinality”: “single”, “type”: “integer”, “defaultValue”: 5
            },
            “template_var_fruit_number_2”: {
                    “cardinality”: “single”, “type”: “integer”, “defaultValue”: 3
            },
            “template_var_fruit_name”: {
                    “cardinality”: “single”, “type”: “string”, “defaultValue”: “apples”
            },
            “template_var_temp_number”: {
                    “cardinality”: “single”, “type”: “integer”, “defaultValue”: 2
            }
    }
}
```

#### Template Processing
Template processing consists of one or more templateRules that are followed by the QML players in order to assign values to the template variables. Template processing is identical in form to responseProcessing except that the purpose is to assign values to template variables, not outcome variables, at the start of a question session.

Template processing is a JSON object in key-value format. The keys in the JSON are the template variables used in the question and values are of type TemplateProcessingDef. 

TemplateProcessing:

```
{
	“templateProcessing”: {
		“<template_variable_1>”: List of TemplateProcessingDef Objects (one per locale),
		“<template_variable_2>”: List of TemplateProcessingDef Objects,
		… 
	}
}
```

*TemplateProcessingDef:*

Each template variable should have the template processing rule defined using TemplateProcessingDef object.

| Attribute | Schema | Description |
| --- | ----- | ----------- |
| random | dataType: RandomVariableDef, required: false | Used to generate a random value for the template variable |
| regex | dataType: string, required: false | Used to provide a regular expression for generating value for the template variable |
| eval | dataType: string, required: false | Used to provide custom logic (in javascript) for generating value for the template variable |
| locale | dataType: string, required: false, defaultValue: en | Internationalization is supported in template processing. Template processing specific to locale can be defined by setting the locale value. |

> For each template variable, at least one and only one of “random”, “regex” and “eval” should be defined as the processing rule. 

*RandomVariableDef:*

| Attribute | Schema | Description |
| --- | ----- | ----------- |
| list | dataType: list of any, required: false | Used to pick a value randomly from the provided list |
| number | dataType: RandomNumberDef, required: false | Used to generate a random number (integer or float) |

*RandomNumberDef:*

| Attribute | Schema | Description |
| --- | ----- | ----------- |
| type | dataType: string, required: true, range: integer, float | |
| min | dataType: integer/float, required: true | |
| max | dataType: integer/float, required: true | |
| step | dataType: integer/float, required: false | |

Below is the sample template processing configuration for the template variables referred to in the previous section. 

```
{
    “templateProcessing”: {
            “template_var_fruit_number_1”: [{
<!-- a random integer between 3 and 9 is set as the value for this variable -->
                    “random”: {
                            “type”: “integer”, “min”: 3, “min”: 9
                    }
            }],
            “template_var_temp_number”: [{
<!-- a random integer between 3 and 6 is set as the value for this variable -->
                    “random”: {
                            “type”: “integer”, “min”: 3, “min”: 6
                    }
            }],
            “template_var_fruit_number_2”: [{
<!-- this template variable is dependent on the value of above two variables. Hence the processing rule for those two variables should be defined before this variable. -->
                    “eval”: “
                            if (getTemplateVariable(‘template_var_temp_number’) > getTemplateVariable(template_var_fruit_number_1)) {
                                    return getTemplateVariable(template_var_fruit_number_1) - 3;
                            } else {
                                    return getTemplateVariable(template_var_fruit_number_1) - getTemplateVariable(‘template_var_temp_number’);
                            }
                    ”
            }],
            “template_var_fruit_name”: [{
<!-- a random value from the below list is set as the value for this variable -->
                    “random”: {
                            “list”: [“apples”, “mangoes”, “bananas”, “oranges”, “pineapples”]
                    },
                    “locale”: “en”
            }, {
                    “random”: {
                            “list”: [“सेब”, “आम”, “केले”, “संतरे”, “अनानास”]
                    },
                    “locale”: “hi”
            }]
    }
}
```

##### Using Template Variables in Response Processing

Template variables are also used in response processing in cases when these variables affect the outcome of the question. When the response processing has custom logic , the template variables can be accessed using the library method *getTemplateVariable*. Alternatively, the pre-defined response processing template **“MATCH_TEMPLATE”** can also be used to process the response using template variables.

**MATCH_TEMPLATE**

This response processing template matches the value of response variables with specified template variables and sets the outcome variable SCORE. 

The map response processing template takes a match template config as input. This config is to set SCORE outcome variable based on the value of a template variable. Optionally, a mapping config (similar to the one provided for MAP_RESPONSE template) can also be provided to set other outcome variables.

*MatchTemplateConfig:*

Template variable config should be provided in JSON format. It comprises of the list of outcome variables and their values for various possible scores.

```
{
<!-- matchTemplateConfig is a list of configurations, each for different SCORE outcome -->
    “matchTemplateConfig”: [
           {
<!-- each config has a mapping section to define the mapping between each response and one or more template variables-->
                 “mapping”: {
<!-- for each response variable, a list of conditions can be configured -->
                        <response_variable_1>: [
                            {
<!-- operators: le (less than or equal to), lt (less than), eq (equals to), ge (greater than or equals to), gt (greater than) and in (in a list of values) -->
                                “operator”: <operator_1>,
<!-- one or more template variables can be specified for each operator. If more than one template variable is provided, an OR condition will be applied -->
                                “templateVariables”: [<template_variable_1>, <template_variable_2>]
                            },
<!-- AND operation is applied between multiple conditions of a response variable -->
                            {   … }
                        ],
<!-- AND operation is applied between multiple response variable conditions-->
                        <response_variable_2>: [{ … }]
                 }
<!-- a SCORE value should be configured to be set when the specified mapping resolves to be true -->
                 “SCORE”: 1.0
           }
    ]
<!-- if none of the mappings are resolved to true, the SCORE will be set to 0 by the MATCH_TEMPLATE -->
}
```

Following is the sample usage of MATCH_TEMPLATE response processing template:

```
{
    “responseProcessing”: {
           “template”: “MATCH_TEMPLATE”,
           “matchTemplateConfig”: [
                   {
                           “mapping”: {
                                   “response_01”: [{
                                           “eq”: “template_var_temp_number”
                                   }]
                           },
                           “SCORE”: 1.0
                   }           
           ]
    }
}
```

In the above sample, the SCORE is set to 1.0 when the value of response variable “RESPONSE” is equal to the value of the template variable “template_var_temp_number”.

### Question Metadata
QML Question metadata has detail that is needed for discovering, delivering and composing questions into tests. QML classifies question metadata into four categories:

#### Learning Metadata
This category of metadata contains pedagogic information of the question - how difficult it is, what level of skill is the question assessing, where is it intended to be used, etc.

- *bloomsTaxonomyLevel*: the cognitive processes involved to answer the question - remember, understand, apply, analyse, evaluate, create.
- *difficultyLevel*: difficulty level of the question for a average learner - easy, medium, hard.
- *purpose*: the purpose served by the question - recall, explore, sense, assess, teach, revise.
- *expectedDuration*: expected time for one attempt of the question.
- *maxScore*: maximum score that can be awarded for the question.

#### Technical Metadata
This category of metadata contains information that can be used by delivery engines to deliver the question. Some of the metadata fields of this category can be automatically derived and set by the creation tools, e.g.: mime type. 

- *mimeType*: mime type of the question - HTML is the only supported mime type in this version of the specification.
- *version*: version of the QML specification using which the question is created.
- *questionType*: one of the standard question types - mcq, mtf, ftb, mmcq, essay, short answers, programming, other. this can be auto-derived at times based on the interactions used in the question.
- *visibility*: if the question is visible for all or only for those who created it and/or for some specific systems or use cases - private, public.
- *isTemplate*: set to true if question data has template variables and template processing, else it is set to false.
- *interactions*: list of interactions present in the question.
- *solutionAvailable*: true, if question data has answers, else, set to false
- *scoringMode*: one of the values: responseProcessing (if question has inbuild response processing), offline (if scoring will be done offline and/or manually) or external (if an external system does the evaluation and submit the score). 

#### Curricular Metadata
This category of metadata captures the pedagogic intent of the question - which concepts are tested by the question, etc. The metadata fields in this category vary for each domain. For example, questions of K-12 domain may have the following curricular metadata fields: board, grade, subject, medium and topics.

#### Usage Metadata
This category of metadata captures the statistical data about the usage of a question. These metadata fields are not provided by users but are set by analytics systems based on the usage telemetry generated by QML players.

- *totalTimeSpent*: total cumulative time spent, in milliseconds, on the question by all users.
- *avgTimeSpent*: average time spent per attempt, in milliseconds.
- *numAttempts*: total number of attempts.
- *numCorrectAttempts*: number of attempts where the user response is correct.
- *numInCorrectAttempts*: number of attempts where the user response is incorrect.
- *numSkips*: total number of attempts where the user did not give a response.
- *avgRating*: average rating of the question.
- *totalRatings*: total number of ratings given for the question.

### Usage Data and Results Reporting
