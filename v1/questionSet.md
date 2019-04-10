## QuML for Tests
Questions from the bank are pulled together to form a test. Selecting the questions requires a deep understanding of the subject and teaching process. For example, which concepts to cover in the test, how many questions should be picked, in what order? All of these have an impact on the design of a test. It is therefore a task that is generally done by the subject matter experts.

### Conceptual Model

At the simplest level, a test is a collection of pre-selected questions, carefully arranged
in an order. The issue with such tests is that they are static, and multiple attempts
become rote.

> ### In reality, tests need to be a lot more dynamic so that new questions, in random order are presented to the student. Even if the same student attempts the test multiple times, he/she is presented with a different set of questions during each attempt.

In addition to explicitly adding questions to a test, the test can also be created using a
dynamic criteria/query - so that all questions in the question bank that match that
criteria become candidates for inclusion in the test. Each such criteria is nothing but a
set of questions as defined in the underlying question bank.

A test therefore is a group of such question sets and questions with an associated set of
rules that determine which of the questions the student sees, in what order, and in
what way the student interacts with them. The rules describe the valid paths through
the test, when responses are submitted for response processing and when (if at all)
feedback is to be given.

```
Figure - Test/Question Set and Questions
```

> “In the example shown above, the test has  5  questions that match the criteria defined in the first set. Let’s say the question bank has  20  questions that are “easy” difficulty level, and related to the concept of “Inertia”. Therefore, at runtime, when a test is being constructed, ANY 5 from the available 20 can be selected.”

Modeling the tests (Question Sets) like this maintains a ​Balance of the questions in the
test, ensure that even though questions are picked at random for different students,
they all still get questions that are similar but not necessarily identical (in this case
they all get Easy questions) - ensuring ​fairness​ of the assessment.

A test (represented using question set in QuML) must contain at least one other
question set or question. Question sets enable the following capabilities:

#### ➢ Divide test into parts that could be undertaken in separate test sessions or a single test session

#### ➢ Establish groups of questions that have some common pedagogic testing objective

#### ➢ Collect together questions which will then be presented to the student. The order of presentation can be controlled using selection and ordering algorithms

The below figure shows an overview of different components and actors involved in
tests creation and delivery.

```
Figure: Overall system view for Tests
```

Similar to questions, teachers and students interact with question sets via QuML
players. QuML players are responsible for managing test sessions.

For each test session, question sets are selected and arranged into order according to
rules defined in the containing question set. This process of selection and ordering
defines a basic structure for each part of the test on a per-session basis. The paths that
a student may take through this structure are then controlled by the mode settings for
the question set and possibly by further preConditions or branchRules evaluated during
the test session itself.

```
Figure - Structure of the test with question sets and questions
```

The below figure illustrates a specific instance of the same question set after the
application of selection and ordering rules. A rule in question set S 01  selects just one of
S 01 A and S 01 B, a rule in S 02  shuffles the order of the items contained by it and, finally,
rules in S 03 select 1 out of the 2 items it contains and shuffles the result.

```
Figure - Delivered test after selection and ordering
```

#### Navigation mode

This specification defines a way in which the overall behaviour of a question set can be
controlled: the **navigation mode**. The navigation mode determines the general paths
that the student may take.

A question set in linear mode restricts the student to attempt each question in turn.
Once the student moves on they are not permitted to return. A question set in
nonlinear mode removes this restriction - the student is free to navigate to any
question in the question set at any time. Navigation mode is applicable only to
question sets that have questions as members and not other question sets. QuML
players are free to implement their own user interface elements to facilitate navigation
provided they honour the navigation mode currently in effect.

#### Pre-conditions and Branching

These are advanced concepts used to introduce an element of adaptivity into the
specification of a test. Pre-conditions enable some question sets to be skipped
depending on the outcomes of some of the question sets presented earlier in the test.


A branch rule is a simple expression attached to a question set that is evaluated after
the question set has been presented to the student. If the expression evaluates to true
the test jumps forward to the question set referred to by the target identifier or exit the
test.

An implication with branch rules is that this configuration might result in cyclic loops
some times. Though this can be used in drill and practice use cases, QuML players
should not allow unbounded repetition. The player should exit the test after a certain
number of repetitions.

#### Time Limits

In the context of a test, a question or a question set, may be subject to a time
constraint. This specification supports both minimum and maximum time constraints.
The controlled time for a single question is simply the duration of the question session
as defined by the built-in response variable *duration*. For question sets, the time limits
relate to the duration of all the question sessions plus any other time spent navigating
the question set. In other words, the time includes time spent in states where no
question is being interacted with, such as dedicated navigation screens.

QuML players are required to track and report the time spent on each question set
when time limits are in force. If no time limit is in force for a question set, then the
time spent may be tracked and reported but it is not required.

The time spent on the question set is recorded using a built-in response variable called
*duration*. The values of these durations can be referred to during **outcomeProcessing**
by using the variable name ​duration​.

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
“questions”: {QuestionDef Object}
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
A question set can contain either other question sets or individual questions. If a question set contains other question sets, the association should be specified in the “questionSets” section. 

QuestionSets:

```
{
“questionSets”: [{QuestionSetDef Object}, {QuestionSetDef Object}, … ]
}
```

*QuestionSetDef*

| Attribute | Schema | Description |
| --- | ----- | ----------- |
| questionSetId | dataType: string, required: true | |
| shuffle | dataType: boolean, required: false, defaultValue: false | |
| totalQuestions | dataType: integer, required: true | |
| maxQuestions | dataType: integer, required: true | |
| preConditions | dataType: list of PreConditionDef objects, required: false | Used to skip the question set depending on the outcome of question sets presented earlier.  |
| branchRules | dataType: list of BranchRuleDef objects, required: false | Evaluated after question set is complete and jumps forward to the specified target. |

*PreConditionDef*

*OutcomeMatchDef*

*BranchRuleDef*

*TargetSetDef*

### Question Set Metadata

### Usage Data and Results Reporting
