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
- It should contian only **structural and input HTML elements**. Structural elements include \<div>, \<span>, \<ul> and \<li>. Input html elements are text input, text area, select, options, file upload and canvas elements.
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
