## QuML for Questions

Digital assessments become easier with a standard format for the exchange of
questions and structures, scoring information, the ability to analyze results, and ways
to accommodate students’ personal needs and preferences. Digital assessments are
especially important at a time when most institutions are looking to eliminate or
greatly reduce the use of paper in classrooms within the next few years. QuML
specification for questions defines a standard format that enables interoperability of
questions and digital assessments.

This document has the following sections:

1. Anatomy of a Question - the structure and composition of a QuML question
2. Conceptual Model - a conceptual model of the actors and components related to QuML quetions
3. Question Data Model - the core that defines the actual question data exchange (including the content itself)
4. Question Metadata - QuML specific metadata

### Anatomy of a Question

Traditionally, questions are monolithic in nature and are used like black boxes. Such
questions are primarily used for only one purpose - getting a score when a user
interacts with the question. There is no (or very little) knowledge of what the question
consists of, what is the purpose and for whom it is intended for. This limits the re-use
of questions in different scenarios and also by different systems.

![Questions used as black box](https://github.com/sunbird-specs/inQuiry/blob/master/v1/images/Question_black_box.png)

```
Figure: Questions used only for SCORE output (black box model)
```

QuML introduces a model where a question incorporates the information that defines
the question, information that is presented to a student, instructions of how to present
to the student & instructions of how to score the question. For example, in QuML,
every interaction of student with the question gets captured as response variables and
scoring takes place when student responses are transformed into outcomes by response
processing rules. The response processing rules and the response variables are well
defined entities in QuML.

> ### In QuML, question is a first class citizen. It can be used for different purposes, and in different forms, mainly due to its modular structure.

There are multiple components in a QuML question. These components can be broadly
classified into four categories.

#### ➢ Content​: The content category comprises of the data that is needed to render and process the question. It includes labels & texts, responses and outcomes of the question.

#### ➢ Appearance​: This category comprises of the sections that define the user experience of the question. It includes the HTML markup and styles to format the question.

#### ➢ Behaviour​: This category comprises of the processing logic for assigning values to template variables and evaluating the student’s responses.

#### ➢ Metadata​: This category comprises of the data that is needed to search, discover and compose questions into tests.

![Anatomy of a QuML question](https://github.com/sunbird-specs/inQuiry/blob/master/v1/images/Question_anatomy_quml.png)

```
Figure: Anatomy of a QuML Question
```

Since a QuML question is not monolithic in nature and has a well-defined structure, a
question can be deconstructed and used for different purposes. For example, a testing
system can get questions which are tagged for “exam” purpose and render them
without feedback or hints to the student. And another e-learning system can get
questions, from the same repository, which are tagged for “practice” purpose and
render them with feedback and hints enabled.

### Conceptual Model

The below figure shows an overview of different components and actors involved in the
questions creation and delivery.

![System view](https://github.com/sunbird-specs/inQuiry/blob/master/v1/images/Question_system_view.png)

```
Figure: Overall system view for Questions
```

Question Bank is a repository of QuML questions. Authoring tools are used by authors
to create questions which get stored in the question bank. Learning and Assessment
systems are responsible for presenting the questions to teachers and/or students
depending on the context where the question is being used. Teachers and students interact with questions via QuML players. QuML players are responsible for managing the question sessions.

A question session is the accumulation of all the attempts at a particular instance of a
question made by a student. In some types of tests, the same question may be
presented to the candidate multiple times e.g. during ‘drill and practice’. Each
occurrence or instance of the question is associated with its own question session. The
following figure illustrates the user-perceived states of the question session.

![Question session workflow](https://github.com/sunbird-specs/inQuiry/blob/master/v1/images/Question_session_workflow.png)

```
Figure: The life cycle of a question session
```

Not all states will apply to every scenario, for example feedback may not be provided
for a question or it may not be allowed in the context in which the question is being
used. Similarly, the student may not be permitted to review their responses and/or view
the solution.

As mentioned earlier, a QuML question comprises of multiple components with a
well-defined structure. Each component of the question comes into play at different
states of a question session. QuML implementations (players) use these components
and perform the required action in-between state transitions. The below figure shows
the actions performed by QuML players in between states of a question session.

![Question session processes](https://github.com/sunbird-specs/inQuiry/blob/master/v1/images/Question_session_components.png)

```
Figure: Processes in a question session
```

For example, body along with style sheet data is used to present the question to the
student before entering ​interacting state. User responses are captured and set during
interacting state. User’s responses are processed and outcomes are set before moving
into ​finished state. Finally, QuML players generate telemetry events (explained later)
that capture details of all user interactions with the question, including the user’s
responses to the question.

### Question Data Model

A QuML Question comprises of the information model and associated binding that can be used to store, represent and deliver questions. A question can be defined as a set of interactions (possibly empty) collected together with any supporting material and an optional set of rules for converting the student’s response(s) into outcomes. Question contains the definition for the interactions, variables to store responses, logic to process the responses, outcome data and other components that are needed for question delivery.

QuML specification makes use of web standards like HTML, CSS, JSON, and Javascript to store the question data. The below figure shows the parts that comprise a QuML question:

![Structure of a Question](https://github.com/sunbird-specs/inQuiry/blob/master/v1/images/Question_structural_model.png)

```
Figure: QuML Question Structural Model
```

#### Question Variables

QuML defines multiple types of variables that are useful for different purposes at different states of a question session. The specification defines the places where and how these variables can be declared, initialised, updated and used.

Question variables are declared by variable declarations: response, outcome or template variables. All variables must be declared except for the built-in variables, referred to below, which are declared implicitly and must not be declared. The purpose of the declaration is to associate an identifier with the variable and to identify the runtime type of the variable's value. The variables are also optionally initialised at the time of declaration.

**Response Variables**

Response variables are used to capture the response of student. These variables are generated as a result of an interaction of student with the question. The expected values for response variables are declared in “response declarations” and bound to interactions in the question. Each response variable declared may be bound to one and only one interaction. At runtime, response variables are instantiated as part of a question session.

A response variable with a NULL value indicates that the student has not offered a response, either because they have not attempted the question at all or because they have attempted it and chosen not to provide a response. If a default value has been provided for a response variable then the variable is set to this value at the start of the first attempt. If the student never attempts the question, in other words, the question session passes straight from the  “start” state to the  “finished” state without going through the interacting state, then the response variable remains NULL and the default value is never used.

Response variables can be used within the question body using the html data attribute: **“data-response-variable”**. This data attribute should be set on HTML elements that are used by candidates for interacting with the question. And when the candidate uses the HTML element for interaction, the corresponding response variable should be updated with the value of the HTML element. 

```
<input type="checkbox" name="element" data-response-variable="response_01" value="Oxygen" data-multi-choice-interaction>
```

In the above sample, there is response variable named “response_01” defined on the HTML checkbox element. When a candidate selects the checkbox, the value of the checkbox element (Oxygen, in this example) is set as the value for the variable “response_01”. 

##### Built-in Response Variables
There are two built-in response variables: 'numAttempts' and 'duration'. These are declared implicitly and should be updated by QuML players when a candidate is interacting with the question.

- **numAttempts**: An integer that records the number of attempts at each question the candidate has taken (if multiple attempts are allowed in the context where the question is being used). The value defaults to 0 initially and then increases by 1 at the start of each attempt.
- **duration**: A float that records the accumulated time (in seconds) of all candidate sessions for all attempts. In other words, the time between the beginning and the end of the question session minus any time the session was in the suspended state. 

**Template Variables**

Template variables are used in templatized questions, which can be used to produce large number of similar questions. Template variables can be referred to by templateVariable objects in the question body.

Template variables can be referred to by using the html data attribute **“data-template-variable”** in the question body. This data attribute should be set on HTML elements which need to be templatized. And the template variables are resolved and replaced by an absolute value before the question is presented to a student. The rules for assigning values to template variables should be defined in the **“templateProcessing”** section of the question data.

This data attribute should be set on a \<span> element around a text that needs to be templatized. And when the question session starts, the text within the \<span> gets replaced with the value generated by template processing rule for the corresponding template variable.

```
Shyam has <span data-template-variable="template_var_fruit_number_1">5</span> <span data-template-variable="template_var_fruit_name">apples</span>.

Ram borrowed <span data-template-variable="template_var_fruit_number_2">3</span> <span data-template-variable="template_var_fruit_name">apples</span> from Shyam.

Shyam now has <input type="text" data-text-interaction data-response-variable="response_01" /> <span data-template-variable="template_var_fruit_name">apples</span>.
```

The above question is for subtraction, where the learner is asked to identify the number of apples Shyam has after Ram borrowed 3 of the 5 apples which Shyam had. Instead of showing the same numbers and fruit name always, the numbers and name of the fruit is templatized in the item body using template variables.

**Outcome Variables**

Outcome variables are declared by outcome declarations. Their value is set either from a default given in the declaration itself or during “response processing”. These variables are used for returning the score or for controlling the feedback to the student.

There are six reserved outcome variables:

- Questions that declare a numeric outcome variable representing the student’s overall performance on the question should use the outcome name **‘SCORE’** for the variable. SCORE needs to be a float.
- Questions that provide a feedback to student after the attempt should use the outcome variable **‘FEEDBACK’** to show the appropriate feedback modal. FEEDBACK needs to be a string.
- Questions that have in-built hints to help students in attempting the question should use the outcome variable **‘HINT’**. The value of this variable should be used by the QuML players to render the corresponding hint. HINT needs to be a string.
- Questions that declare a maximum score (in multiple response choice questions, for example) should do so by declaring the **‘MAXSCORE’** variable. MAXSCORE needs to be a float.
- Questions that declare a minimum score (minimum score to mark the student as passed) should do so by declaring the **‘MINSCORE’** variable. MINSCORE needs to be a float.
- Questions or tests that want to make the fact that the student scored above a predefined threshold available as a variable should use the **‘PASSED’** variable. PASSED needs to be a boolean. This variable is automatically set after response processing using the SCORE and MINSCORE outcome variables.

##### Built-in Outcome Variables
There is one built-in outcome variable, **‘completionStatus’**, that is declared implicitly and must not appear in outcome variables declaration. QuML players must maintain the value of the built-in outcome variable completionStatus. It starts with the reserved value “not_attempted”. At the start of the first attempt, it changes to the reserved value “unknown”. It remains with this value for the duration of the question session unless set to a different value by a setOutcomeValue in responseProcessing. This variable changes to the reserved value “completed” by default if it is not updated during response processing. 

There are four permitted values:
1. ‘completed’ - the student has experienced enough of the question to consider it completed;
2. ‘incomplete’ - the student has not experienced enough of the question to consider it completed;
3. ‘not_attempted’ - the student is considered to have not used the question in any significant way;
4. ‘unknown’ - no assertion on the state of completion can be made.

#### Body
Body contains the text, graphics, media objects and interactions that describe the question’s content and information about how it is structured. The body is presented by combining it with stylesheet and/or internationalization information, either explicitly or implicitly using the default style rules of the QuML player.

Body is presented to the student when the associated question session is in the *interacting* state. In this state, the student must be able to interact with each of the visible interactions and therefore set or update the values of the associated response variables.

The body may be presented to the student when the question session is in the *finished* or *review* state. In these states, although the student’s responses should be visible, the interactions or the setting and/or updating the values of the associated response variables must be disabled.

Finally, the body may be presented to the student in the *solution* state, in which case the correct values of the response variables must be visible and the associated interactions and/or response variable updates disabled.

The structural elements of the item body are taken from HTML with the following restrictions:
- It should contian only **structural, media and input HTML elements**. Structural elements include \<div>, \<span>, \<ul> and \<li>. Media html elements are img, audio and video elements. Input html elements are text input, text area, select, options, checkbox, radio buttons, file upload and canvas elements.
- It should not contain **\<form> element**
- It should not have any **javascript code**, i.e. \<script> elements or onXYZ (onClick, etc) for HTML elements should not be used
- It should not have any **javascript import statements**
- It should not have any **css import statements**

In addition to the standard HTML elements and attributes, the specification allows the following in the question body: 
- Usage for “data-” attributes to specify the interactions on HTML elements, bind response variables to the interactions, use template and i18n variables.
- Usage of standard style classes defined by the specificaiton. QuML players should provide implementation for the defined classes.

Body:
```
{
	“itemBody”: “<HTML>...</HTML>”
}
```

#### Interactions
A question can contain zero or more interactions with the user. Most of these interactions are typical question types (for example, multiple choice, order elements, fill in the blanks). We can also use interactions for activities like "upload document," "draw a picture," and "start a film".

Interactions can be defined in the item body using HTML data attributes as shown in the below sample (**“data-<interaction_name>-interaction”**). Each interaction should have an associated response variable that binds the interaction to a **“responseDeclaration”** with the same identifier. This is the link for the result and response processing, which are described later in this document.

```
<input type="checkbox" name="element" data-multi-choice-interaction data-response-variable="response_01" value="Oxygen">
```

QuML specification defines the following interaction types. QuML players should provide the implementation for all the defined interaction types. The behaviour of the interaction may vary in each implementation but should ensure that the defined specification is supported.

##### Simple Choice Interaction

The simple choice interaction is used when a set of choices are presented to the student. The student’s task is to select one of the choices. This interaction must be bound to a response variable with single cardinality (i.e. set only one value to the associated response variable).

| **Data Attribute** | **data-simple-choice-interaction** |
| --- | ----- |
| cardinality | *single* |
| type | string, integer or float |
| html elements | any HTML element which is clickable and have a value attribute |

##### Multi Choice Interaction

The multi choice interaction is used when a set of choices are presented to the student. The student’s task is to select one or more of the choices. This interaction must be bound to a response variable with multiple cardinality (i.e. set a list of values to the associated response variable). An array variable will be used to store the values of response variable for this interaction.

| **Data Attribute** | **data-multi-choice-interaction** |
| --- | ----- |
| cardinality | *multiple* |
| type | string, integer or float |
| html elements | any HTML element which is clickable and have a value attribute |

##### Text Interaction

A text interaction obtains a piece of text from the student. The QuML player must allow the student to review their choice within the context of the question. The text interaction must be bound to a response variable with single cardinality only. The type must be one of string, integer, or float. 

| **Data Attribute** | **data-text-interaction** |
| --- | ----- |
| cardinality | *string* |
| type | string, integer or float |
| html elements | HTML elements where user can enter text, i.e. text input, or text area |

##### Order Interaction

In an order interaction, the student’s task is to reorder the choices. When specified, the student must select the choices and impose an ordering on them. If a default value is specified for the response variable associated with an order interaction then its value should be used to override the order of the choices specified here. The order interaction must be bound to a response variable with ordered cardinality (i.e. list of ordered values).

| **Data Attribute** | **data-ordered-interaction** |
| --- | ----- |
| cardinality | *ordered* |
| type | string, integer or float |
| html elements | \<ul> element. The values of all \<li> elements should be set in the same order as the value for response variable bound with the interaction |
	
> The order interaction implementation in the player should enable drag and drop of the \<li> elements in the \<ul> element with this interaction. QuML players may choose to provide enhanced graphics/animation for order interaction.

##### Match Interaction

A match interaction presents candidates with two sets of choices and allows them to create associates between pairs of choices in the two sets, but not between pairs of choices in the same set. The match interaction must be bound to a response variable with type map and single cardinality.

| **Data Attribute** | **data-match-interaction** |
| --- | ----- |
| cardinality | *single* |
| type | map |
| html elements | Two \<ul> elements. Both the \<ul> elements should be bound to the same response variable. The list of values of \<li> element pairs (mapped with each other) should be set as the value of the response variable. |
	
> Similar to order interaction, the match interaction implementation in the player should enable drag and drop of \<li> elements in one \<ul> element on to \<li> elements in another \<ul> element with this interaction. QuML players may choose to provide enhanced graphics/animation for match interaction.

##### Upload Interaction

The upload interaction allows the candidate to upload a pre-prepared file representing their response. It must be bound to a response variable with type file and single cardinality.

| **Data Attribute** | **data-upload-interaction** |
| --- | ----- |
| cardinality | *single* |
| type | uri |
| html elements | HTML file upload element |
	
> The upload interaction implementation in the player should upload the input file to a configured server and set the uploaded file url as the value for the response variable.

##### Map Interaction

The map interaction is a graphic interaction. The candidate’s task is to select one or more points on a canvas. The associated response may have a mapping that scores the response on the basis of comparing it against predefined areas but QuML players must not indicate these areas of the image. Only the actual point(s) selected by the candidate shall be indicated. The map interaction must be bound to a response variable with type points and multiple cardinality.

| **Data Attribute** | **data-map-interaction** |
| --- | ----- |
| cardinality | *multiple* |
| type | points |
| html elements | HTML canvas element |

##### Custom Interaction

The custom interaction provides an opportunity for extensibility of this specification to include support for interactions not currently documented. QuML implementations can implement a set of custom interactions that go beyond the standard interactions described above. Authors wishing to write questions for those implementations can use these custom interactions. However these custom templates should be published to ensure that these questions can be used with QuML players of other implementations. Otherwise, such questions will be limited to use only by that implementation.

#### CSS classes
QuML recommends the use of Cascading Style Sheets (CSS) for controlling the content formatting. QuML has a defined set of standard classes for various different elements and interactions. A question can use those classes for formatting and QuML players should provide implementation for these classes.

- *1-col-layout*: style to create a one column layout
- *2-col-layout*: style to create a two column layout
- *3-col-layout*: style to create a three column layout
- *4-col-layout*: style to create a four column layout
- *6-col-layout*: style to create a six column layout
- *8-col-layout*: style to create a eight column layout
- *row*: style to add a row in the question
- *title*: for formatting title texts of the quesiton
- *sub-title*: for formatting sub-headings in the question
- *paragraph*: for formatting paragraph texts
- *small-image*: style to display small images
- *medium-image*: style to display medium size images
- *large-image*: style to display large images
- *horizontal-options*: style to layout options horizontally (can be used for choice, match and order interactions)
- *vertical-options*: style to layout options vertically (can be used for choice, match and order interactions)
- *file-upload*: styling for file upload elements (can be used for upload interaction)
- *canvas*: styling for canvas elements (can be used for map interaction)

Alternatively when there is a need for formatting that is not provided by the defined classes, a question can also have custom css. The support of the CSS elements used for formatting is totally dependent on the browser used to render the question. Hence it is advised to use only CSS elements that are supported by all the commonly used browsers and browser versions.

#### Instructions
Some questions will have instructions on how to understand, attempt or how the question will be evaluated. Such instructions are defined in HTML format and stored in the instructions part of the question data. 

Instructions:
```
{
	“instructions”: “<HTML>... … … </HTML>”
}
```

#### i18n Data
Internationalization is supported by QuML to render texts inside  body (answers, feedback and hints) in different locales. To enable this, QuML allows storage of content in multiple locales.

For example, **body** in different locales should be stored in  body part of the question data as below:

body:
```
{
	“body”: {
		“<locale_1>”: "<div>...</div>",
		“<locale_2>”: "<div>...</div>"
		… 
	}
}
```

In cases when internationalization is not required, the value of body (or of answers, instructions, feedback and hints) can be defined without the locale information.

#### Response Declaration
A **“responseDeclaration”** contains information about the answer (the response) to a question: When is it correct, and (optionally) how is it scored? 

Response Declaration should have declaration for every response variable in the question body. Optionally, the declaration can have the following details:

- *Correct Response:*  Correct (or optimal) values for the response variable
- *Mapping to Score:* Map different values to score so that a response can have more nuances than plain right or wrong, e.g.: a multiple choice question with more than one correct answer can support partial scoring

Response declaration is a JSON object in key-value format. The keys in the JSON are the response variables defined in the body and values are of type ResponseVariableDef.

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
An “outcomeDeclaration” contains information about the outcome variables of the question, i.e the values that are output of a question session. QuML players should make the outcome variables available to the context in which the question is being used. For example, a test session uses the values of outcome variables of constituent questions to process the outcome of the test.

Outcome declaration is a JSON object in key-value format. The keys in the JSON are the outcome variables  and values are of type OutcomeVariableDef.

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

#### Response Processing
Response processing is the process by which QuML players assign outcomes based on the student’s responses. The state representation for the response processing is shown in the following figure. The outcomes may also be used to provide feedback to the student. Feedback is either provided immediately following the end of the student’s attempt or it is provided at some later time, perhaps as part of a summary report on the question session.

The end of an attempt, and therefore response processing, must only take place in direct response to a user action or in response to some expected event, such as the end of a test. A question session that enters the suspended state may have values for the response variables that have yet to be submitted for response processing.

![Question response processing](https://github.com/sunbird-specs/inQuiry/blob/master/v1/images/Question_response_processing.png)

```
Figure - The state diagram for response processing
```

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
Custom response processing involves the application of a set of response rules, including the testing of response conditions and the evaluation of logic involving the question variables. For QuML implementations that are only designed to support very simple use cases, the implementation of a system for carrying out this evaluation, conditional testing and processing may pose a barrier to the adoption of the specification.

To alleviate this problem, the implementation of generalized response processing is an optional feature. Players that do not support it can instead implement a smaller number of standard response processors called response processing templates. These templates are also defined using javascript for evaluation of logic and should be implemented by the QuML implementation. 

Following are two standard response processing templates defined in QuML (an additional template for response processing using templates is defined in the Template Processing section):

**MATCH_CORRECT**

This response processing template matches the value of response variables with their correct value using **correctAnswer** attribute defined in **responseDeclaration**. It sets the outcome variable SCORE to either 0 or 1 depending on the outcome of the test. To use this template, all response variables must have been declared and have an associated correct value. Similarly, the outcome variable SCORE must also have been declared.

**MAP_RESPONSE**

The map response processing template uses the **mapping** and **areaMapping** definitions of response variables in **responseDefinition** to set value for the outcome SCORE. To use this template, all response variables must have been declared and must have an associated mapping. Similarly, the outcome variable SCORE must also have been declared.

The map response processing template optionally takes a **mapping config** as input. This config is to set other outcome variables based on the value of SCORE outcome variable. If no config is provided, these templates set the value of SCORE outcome only. The structure of mapping config is as shown below.

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

> QuML implementations that do not support generalized response processing (custom evaluation) but do support response processing mechanisms that go beyond the standard templates described above can define templates of their own. Authors wishing to write questions for those implementations can then refer to these custom templates. Publishing these custom templates will then ensure that these questions can be used with QuML players that do support generalized response processing.

#### Feedback
Feedback is shown to the students following response processing. The value of outcome variable “FEEDBACK” is used to determine whether or not the feedback is shown. Feedback is shown only if it is allowed in the context where the question is being used.

Feedback is a JSON object in key-value format. The keys in the JSON are the identifiers of different feedbacks for the question and values are HTML snippet to be shown to the student. After the response processing, the QuML player renders the feedback HTML mapped to the value that is set to the FEEDBACK outcome variable.

Feedback:

```
{
	“feedback”: {
		“<feedback_1>”: “<div>...</div>”,
		“<feedback_2>”: “<div>...</div>”,
		… 
	}
}
```

To support internationalization, feedback in multiple locales should be provided (for each value of feedback) as shown below:

```
{
	“feedback”: {
		“<feedback_1>”: {
		        "locale_1": “<div>...</div>”,
			"locale_2": “<div>...</div>”,
			...
		}

		“<feedback_2>”: {
		        "locale_1": “<div>...</div>”,
			"locale_2": “<div>...</div>”,
			...
		},
		… 
	}
}
```

#### Hints
Hints are shown to the candidates after response processing or when the student requests for hints.
- **HINT** outcome variable value is used to render the hint after response processing.
- Alternatively, QuML players can provide an option for students to request for hints. Upon request, the QuML player renders all the configured hints in a sequence (as defined in the hints configuration).

In either case, hints are shown only if they are allowed in the context where the question is being used. Hints is a JSON object in key-value format. The keys in the JSON are the identifiers of different hints for the question and values are HTML snippet for hints.

Hints is a JSON object in key-value format. The keys in the JSON are the identifiers of different hints for the question and values are HTML snippet for hints. 

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

> Similar to feedback, internationalization is supported for hints also.

#### Answers
Providing exemplar answers for questions aid candidates in-depth learning and enhance candidate’s understanding of the concepts. These answers are mainly helpful for candidates in preparing for exams. Multiple answers can be configured for a question and each answer can have one or more parts, each part should be stored in HTML format. Similar to feedback & hints, answers in multiple locales can be specified.

Answers:

```
{
	“answers”: [
	{
		"parts": [{
				"body": “<div>...</div>”,
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

Answers for QuML questions are shown to the students in *solution* state of question session, and if the context in which the question is being used allows the students to view the solution.

#### Template Declaration
Question templates are templates that can be used for producing large numbers of similar questions. Such questions are often called cloned questions. Question templates can be used to produce a dynamically chosen clone at the start of a question session.

Each question cloned from a template is identical except for the values given to a set of template variables. A question is therefore a question template if it contains **templateDeclaration** and a set of **templateProcessing** rules for assigning values for template variables. The value of the template variable is used to create an appropriate run of text that is displayed. When the question session starts, the template variable gets replaced with the value generated by template processing rule for the corresponding template variable.

Template declarations declare variables that are to be used specifically for the purposes of cloning items. They can have their value set only during **templateProcessing**. They are referred to within the question body in order to individualize the clone and possibly also within the **responseProcessing** rules if the cloning process affects the way the question is scored. 

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
Template processing consists of one or more templateRules that are followed by the QuML players in order to assign values to the template variables. Template processing is identical in form to  responseProcessing except that the purpose is to assign values to template variables, not outcome variables, at the start of a question session.

![Question template processing](https://github.com/sunbird-specs/inQuiry/blob/master/v1/images/Question_template_processing.png)

```
Figure - The state diagram for template processing
```

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
As per definition, *metadata is a set of data that describes and gives information about other data*, the other data is question in this case. Question metadata should capture enough detail that is needed for discovering, delivering and composing questions into tests. QuML classifies question metadata into four categories:

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
- *version*: version of the QuML specification using which the question is created.
- *questionType*: one of the standard question types - mcq, mtf, ftb, mmcq, essay, short answers, programming, other. this can be auto-derived at times based on the interactions used in the question.
- *visibility*: if the question is visible for all or only for those who created it and/or for some specific systems or use cases - private, public.
- *isTemplate*: set to true if question data has template variables and template processing, else it is set to false.
- *interactions*: list of interactions present in the question.
- *solutionAvailable*: true, if question data has answers, else, set to false
- *scoringMode*: one of the values: responseProcessing (if question has inbuild response processing), offline (if scoring will be done offline and/or manually) or external (if an external system does the evaluation and submit the score). 

#### Curricular Metadata
This category of metadata captures the pedagogic intent of the question - which concepts are tested by the question, etc. The metadata fields in this category vary for each domain. For example, questions of K-12 domain may have the following curricular metadata fields: *board, grade, subject, medium and topics*.

#### Usage Metadata
This category of metadata captures the statistical data about the usage of a question. These metadata fields are not provided by users but are set by analytics systems based on the usage telemetry generated by QuML players.

- *totalTimeSpent*: total cumulative time spent, in milliseconds, on the question by all users.
- *avgTimeSpent*: average time spent per attempt, in milliseconds.
- *numAttempts*: total number of attempts.
- *numCorrectAttempts*: number of attempts where the user response is correct.
- *numInCorrectAttempts*: number of attempts where the user response is incorrect.
- *numSkips*: total number of attempts where the user did not give a response.
- *avgRating*: average rating of the question.
- *totalRatings*: total number of ratings given for the question.

