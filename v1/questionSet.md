## Question Set Information Model

### Table of contents
1. [Question Set Data Model](#Question-Set-Data-Model)
    1. [Instructions](#Instructions)
	2. [i18n Variables](#i18n-Variables)
	3. [i18n Data](#i18n-Data)
	4. [Feedback](#Feedback)
	5. [Hints](#Hints)
	6. [Navigation Mode](#Navigation-Mode)
	7. [Time Limits](#Time-Limits)
	8. [Show Hints](#Show-Hints)
	9. [Instructions](#Instructions)
	10. [Outcome Declaration](#Outcome-Declaration)
	11. [Outcome Processing](#Outcome-Processing)
		1. [Custom Outcome Processing](#Custom-Outcome-Processing)
		2. [Outcome Processing Templates](#Outcome-Processing-Templates)
	12. [Questions](#Questions)
	13. [Question Sets](#Question-Sets)
2. [Question Set Metadata](#question-set-metadata)
3. [Usage Data and Results Reporting](#Usage-Data-and-Results-Reporting)

### Question Set Data Model

A test is a group of question sets and questions with an associated set of rules that determine which of the questions the candidate sees, in what order, and in what way the candidate interacts with them. The rules describe the valid paths through the test, when responses are submitted for response processing and when (if at all) feedback is to be given. Tests are represented using Question Sets in QML.

For each test session, question sets are selected and arranged into order according to rules defined in the containing question set. This process of selection and ordering defines a basic structure for each part of the test on a per-session basis. The paths that a student may take through this structure are then controlled by the mode settings for the question set and possibly by further preConditions or branchRules evaluated during the test session itself. 

![Question set](https://github.com/sunbird-specs/qml/blob/master/v1/images/Question_Set_Structure_1.png)

The below figure illustrates a specific instance of the same question set after the application of selection and ordering rules. A rule in question set S01 selects just one of S01A and S01B, a rule in S02 shuffles the order of the items contained by it and, finally, rules in S03 select 1 out of the 2 items it contains and shuffles the result.

![Question set 2](https://github.com/sunbird-specs/qml/blob/master/v1/images/Question_Set_Structure_2.png)

#### Instructions
Instructions for the question set can be defined in HTML format (similar to question instructions). Instructions can contain i18n variables to support internationalization.

Instructions:

```
{
	“instructions”: “<HTML>...</HTML>”
}
```

#### i18n variables
i18n variables are used to enable internationalization, which can be used to localize the text used in question set. i18n variables can be referred to by i18nVariable objects in the instructions, feedback, and hints. The text, that needs to be localized, should be mapped with an i18n variable using the data attribute **“data-i18n-variable”** (similar to i18n variables of questions).

#### i18n data
Similar to questions, texts in a question set (instructions, feedback, hints) can also be rendered in multiple locales. The values for i18n variables in different locales should be defined as i18n data for the question set. The format of i18n data of question sets is same as that of questions.

#### Feedback
The feedback configuration for question sets is same as that of questions. Multiple feedbacks can be configured and each feedback should be defined in HTML format.

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
The hints configuration for question sets is same as that of questions. Multiple hints can be configured and each hint should be defined in HTML format.

Hints:

```
{
	“hints”: {
		“<hint_1>”: “<HTML>...</HTML>”,
		“<hint_2>”: “<HTML>...</HTML>”,
		… 
	}
}	
```

#### Navigation mode
The navigation mode determines the general paths that the student may take during the test session. It is an enumeration with two possible values: “linear” and “non-linear”.

Navigation Mode:

```
{
	“navigationMode”: “linear | non-linear”
}	
```

- A question set in linear mode restricts the student to attempt each question in turn. Once the student moves on they are not permitted to return. 
- A question set in nonlinear mode removes this restriction - the student is free to navigate to any question in the question set at any time.

#### Time limits
This configuration is used by QML players to impose time limits (both minimum and maximum) for attempting a question set. The limits can be specified for the complete set or for one question in the set.

Time Limits:

```
{
	“timeLimits”: {
		“questionSet”: { // time limits for the question set and for any member sets
			“min”: <milli_seconds>,
			“max”: <milli_seconds>
		},
		“question”: { // time limits for the questions in the question set
			“min”: <milli_seconds>,
			“max”: <milli_seconds>
		}
	}
}
```

#### Show Hints
This configuration is used by QML players to enable/disable hints for the student while using the question set. It should be set to either true or false based on the context in which the question set is being used. 

Show Hints:

```
{
	“showHints”: “true | false”
}
```

#### Outcome Declaration
Outcome declaration is a JSON object in key-value format (same as for questions). The keys in the JSON are the outcome variables and values are of type OutcomeVariableDef.

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

#### Outcome Processing
Similar to response processing of questions, outcome processing rules of a question set can be defined using custom evaluation logic or use one of the existing outcome processing templates.

##### Custom Outcome Processing
The custom outcome processing logic using javascript should be defined as part of the “eval” attribute of “outcomeProcessing” data.

```
{
    “outcomeProcessing”: {
           “eval”: “<javascript code to set the outcome variables. Library methods can be used to refer and set question and question set variables>“
    }
}
```

> “eval” data is mandatory if outcome processing templates are not used for the question set. If both template and evaluation logic are present, template is executed first and then the custom evaluation logic is executed.

##### Outcome Processing Templates
Outcome processing takes place each time the student submits the responses for a question. It happens after any (question level) response processing triggered by the submission. Because outcome processing occurs each time the student submits responses, the resulting values of the question set outcomes may be used to activate question set level feedback during the test or to control the behaviour of subsequent parts through the use of preConditions and branchRules. 

Similar to response processing of questions, outcome processing rules of a question set can be defined using custom evaluation logic or use one of the existing outcome processing templates. Following are the outcome processing templates defined in QML:

1. **SUM_OF_SCORES**: This outcome processing template adds the value of SCORE outcome variables of questions or question sets that are part of the question set and sets the computed value as the SCORE outcome of the question set.
2. **AVG_OF_SCORES**: This outcome processing template computes the average of SCORE outcome values of questions or question sets that are part of the question set and sets the computed value as the SCORE outcome of the question set.
3. **WEIGHTED_AVG_OF_SCORES**: This template is similar to the AVG_OF_SCORES templates with an additional configuration to specify weightage for each question or question set. The weightage configuration can be provided using an additional parameter **weightageConfig** in outcome processing.

Outcome processing schema for using outcome processing templates:

| Attribute | Schema | Description |
| --- | ----- | ----------- |
| template | dataType: string, required: false, range: SUM_OF_SCORES, AVG_OF_SCORES, WEIGHTED_AVG_OF_SCORES | name of the outcome processing template to be used for the question. “template” is mandatory if outcome processing templates are used for the question |
| ignoreNullValues | dataType: string, required: false, defaultValue: false | If set to true, the processing will ignore any SCORE outcomes that are null. This is relevant while using average based templates |
| weightageConfig | dataType: map<string, float>, required: false | configuration to set weightages for questions or question sets when WEIGHTED_AVG_OF_SCORES template is used. If not provided, same weightage is given to all |
| mappingConfig | dataType: string, required: false | configuration to set additional outcome variables (other than SCORE). Same as the mappingConfig defined in question responseProcessing specification |

#### Questions
If a question set contains questions, the association should be defined in the “questions” section. This section is mutually exclusive with “questionSets” section, i.e. a question set contain only either question sets or questions but not both.

Questions:

```
{
“questions”: [{QuestionDef Object}, {QuestionDef Object}, … ]
}
```

*QuestionDef*

| Attribute | Schema | Description |
| --- | ----- | ----------- |
| shuffle | dataType: boolean, required: false, defaultValue: false | |
| totalQuestions | dataType: integer, required: true | |
| maxQuestions | dataType: integer, required: true | |
| list | dataType: list of string, required: true | List of question identifiers that are added to the question set. |

#### Question Sets

### Question Set Metadata

### Usage Data and Results Reporting
