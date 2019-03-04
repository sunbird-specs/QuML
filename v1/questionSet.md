## Question Set Information Model


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

#### Navigation mode
The navigation mode determines the general paths that the student may take during the test session. It is an enumeration with two possible values: “linear” and “non-linear”.

Navigation Mode:

```
{
	“navigationMode”: “linear | non-linear”
}	
```

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


### Question Set Metadata

### Usage Data and Results Reporting
