## Question Information Model

This document has the following sections:

1. Question Data Model - the core that defines the actual question data exchange (including the content itself)
2. Question Metadata - QML specific metadata
3. Usage data and Results Reporting - statistical data about the usage and reporting of the results assigned by outcomes and response processing of the question

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

**Body:**
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

__*Simple Choice Interaction*__

Simple choice interaction is used when a set of choices are presented to the student. The student’s task is to select one of the choices. This interaction must be bound to a response variable with single cardinality.

| **Data Attribute** | **data-simple-choice-interaction** |
| --- | ----- |
| cardinality | *single* |
| type | string, integer or float |
| html elements | any HTML element which is clickable and have a value attribute |

__*Multi Choice Interaction*__

Multi choice interaction is used when a set of choices are presented to the candidate. The candidate’s task is to select one or more of the choices. This interaction must be bound to a response variable with multiple cardinality. An array variable will be used to store the values for response variables of this interaction.

| **Data Attribute** | **data-multi-choice-interaction** |
| --- | ----- |
| cardinality | *multiple* |
| type | string, integer or float |
| html elements | any HTML element which is clickable and have a value attribute |

__*Text Interaction*__

A text interaction obtains a piece of text from the candidate. The QML player must allow the candidate to review their choice within the context of the question. The text interaction must be bound to a response variable with single cardinality only. The type must be one of string, integer, or float. 

| **Data Attribute** | **data-text-interaction** |
| --- | ----- |
| cardinality | *string* |
| type | string, integer or float |
| html elements | HTML elements where user can enter text, i.e. text input, or text area |

__*Order Interaction*__

In an order interaction, the candidate’s task is to reorder the choices. When specified, the candidate must select the choices and impose an ordering on them. If a default value is specified for the response variable associated with an order interaction then its value should be used to override the order of the choices specified here. The order interaction must be bound to a response variable with ordered cardinality and type must be one of string, integer or float.

| **Data Attribute** | **data-ordered-interaction** |
| --- | ----- |
| cardinality | *ordered* |
| type | string, integer or float |
| html elements | \<ul> element. The values of all \<li> elements should be set in the same order as the value for response variable bound with the interaction |
	
> The order interaction implementation in the player should enable drag and drop of the \<li> elements in the \<ul> element with this interaction. QML players may choose to provide enhanced graphics/animation for order interaction.

__*Match Interaction*__

A match interaction presents candidates with two sets of choices and allows them to create associates between pairs of choices in the two sets, but not between pairs of choices in the same set. The match interaction must be bound to a response variable with type map and single cardinality.

| **Data Attribute** | **data-match-interaction** |
| --- | ----- |
| cardinality | *single* |
| type | map |
| html elements | Two \<ul> elements. Both the \<ul> elements should be bound to the same response variable. The list of values of \<li> element pairs (mapped with each other) should be set as the value of the response variable. |
	
> Similar to order interaction, the match interaction implementation in the player should enable drag and drop of \<li> elements in one \<ul> element on to \<li> elements in another \<ul> element with this interaction. QML players may choose to provide enhanced graphics/animation for match interaction.

__*Upload Interaction*__

The upload interaction allows the candidate to upload a pre-prepared file representing their response. It must be bound to a response variable with type file and single cardinality.

| **Data Attribute** | **data-upload-interaction** |
| --- | ----- |
| cardinality | *single* |
| type | uri |
| html elements | HTML file upload element |
	
> The upload interaction implementation in the player should upload the input file to a configured server and set the uploaded file url as the value for the response variable.

__*Map Interaction*__

The map interaction is a graphic interaction. The candidate’s task is to select one or more points on a canvas. The associated response may have a mapping that scores the response on the basis of comparing it against predefined areas but QML players must not indicate these areas of the image. Only the actual point(s) selected by the candidate shall be indicated. The map interaction must be bound to a response variable with type points and multiple cardinality.

| **Data Attribute** | **data-map-interaction** |
| --- | ----- |
| cardinality | *multiple* |
| type | points |
| html elements | HTML canvas element |

__*Custom Interaction*__

QML implementations can implement a set of custom interactions that go beyond the standard interactions described above. Authors wishing to write questions for those implementations can use these custom interactions. However these custom templates should be published to ensure that these questions can be used with QML players of other implementations. Otherwise, such questions will be limited to use only by that implementation.

#### Response Variables

#### Asset Variables

#### i18n Variables

#### Template Variables

#### Instructions

#### i18n Data

#### Asset Declaration

#### Response Declaration

#### Outcome Declaration

#### Feedback

#### Hints

#### Answers

#### Response Processing

#### Template Processing

### Question Metadata

### Usage Data and Results Reporting
