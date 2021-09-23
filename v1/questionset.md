# QuML for Tests

Questions from the bank are pulled together to form a test. Selecting the questions requires a deep understanding of the subject and teaching process. For example, which concepts to cover in the test, how many questions should be picked, in what order? All of these have an impact on the design of a test. It is therefore a task that is generally done by the subject matter experts.

## Conceptual Model

At the simplest level, a test is a collection of pre-selected questions, carefully arranged in an order. The issue with such tests is that they are static, and multiple attempts become rote.

> ## In reality, tests need to be a lot more dynamic so that new questions, in random order are presented to the student. Even if the same student attempts the test multiple times, he/she is presented with a different set of questions during each attempt.

In addition to explicitly adding questions to a test, the test can also be created using a dynamic criteria/query - so that all questions in the question bank that match that criteria become candidates for inclusion in the test. Each such criteria is nothing but a set of questions as defined in the underlying question bank.

A test therefore is a group of such question sets and questions with an associated set of rules that determine which of the questions the student sees, in what order, and in what way the student interacts with them. The rules describe the valid paths through the test, when responses are submitted for response processing and when \(if at all\) feedback is to be given.

![Sample test](/v1/images/QuestionSet_sample_test.png)

```text
Figure - Test/Question Set and Questions
```

> “In the example shown above, the test has 5 questions that match the criteria defined in the first set. Let’s say the question bank has 20 questions that are “easy” difficulty level, and related to the concept of “Inertia”. Therefore, at runtime, when a test is being constructed, ANY 5 from the available 20 can be selected.”

Modeling the tests \(Question Sets\) like this maintains a ​Balance of the questions in the test, ensure that even though questions are picked at random for different students, they all still get questions that are similar but not necessarily identical \(in this case they all get Easy questions\) - ensuring ​fairness​ of the assessment.

A test \(represented using question set in QuML\) must contain at least one other question set or question. Question sets enable the following capabilities:

### ➢ Divide test into parts that could be undertaken in separate test sessions or a single test session

### ➢ Establish groups of questions that have some common pedagogic testing objective

### ➢ Collect together questions which will then be presented to the student. The order of presentation can be controlled using selection and ordering algorithms

The below figure shows an overview of different components and actors involved in tests creation and delivery.

![Question Set systems view](/v1/images/QuestionSet_system_view.png)

```text
Figure: Overall system view for Tests
```

Similar to questions, teachers and students interact with question sets via QuML players. QuML players are responsible for managing test sessions.

For each test session, question sets are selected and arranged into order according to rules defined in the containing question set. This process of selection and ordering defines a basic structure for each part of the test on a per-session basis. The paths that a student may take through this structure are then controlled by the mode settings for the question set and possibly by further preConditions or branchRules evaluated during the test session itself.

![Question set structure](/v1/images/Question_Set_Structure_1.png)

```text
Figure - Structure of the test with question sets and questions
```

The below figure illustrates a specific instance of the same question set after the application of selection and ordering rules. A rule in question set S 01 selects just one of S 01 A and S 01 B, a rule in S 02 shuffles the order of the items contained by it and, finally, rules in S 03 select 1 out of the 2 items it contains and shuffles the result.

![Question set materialised](/v1/images/Question_Set_Structure_2.png)

```text
Figure - Delivered test after selection and ordering
```

### Navigation mode

This specification defines a way in which the overall behaviour of a question set can be controlled: the **navigation mode**. The navigation mode determines the general paths that the student may take.

A question set in linear mode restricts the student to attempt each question in turn. Once the student moves on they are not permitted to return. A question set in nonlinear mode removes this restriction - the student is free to navigate to any question in the question set at any time. Navigation mode is applicable only to question sets that have questions as members and not other question sets. QuML players are free to implement their own user interface elements to facilitate navigation provided they honour the navigation mode currently in effect.

### Pre-conditions and Branching

These are advanced concepts used to introduce an element of adaptivity into the specification of a test. Pre-conditions enable some question sets to be skipped depending on the outcomes of some of the question sets presented earlier in the test.

A branch rule is a simple expression attached to a question set that is evaluated after the question set has been presented to the student. If the expression evaluates to true the test jumps forward to the question set referred to by the target identifier or exit the test.

An implication with branch rules is that this configuration might result in cyclic loops some times. Though this can be used in drill and practice use cases, QuML players should not allow unbounded repetition. The player should exit the test after a certain number of repetitions.

### Time Limits

In the context of a test, a question or a question set, may be subject to a time constraint. This specification supports both minimum and maximum time constraints. The controlled time for a single question is simply the duration of the question session as defined by the built-in response variable _duration_. For question sets, the time limits relate to the duration of all the question sessions plus any other time spent navigating the question set. In other words, the time includes time spent in states where no question is being interacted with, such as dedicated navigation screens.

QuML players are required to track and report the time spent on each question set when time limits are in force. If no time limit is in force for a question set, then the time spent may be tracked and reported but it is not required.

The time spent on the question set is recorded using a built-in response variable called _duration_. The values of these durations can be referred to during **outcomeProcessing** by using the variable name ​duration​.

## Question Set Data Model

This section describes the data model of a question set. Question set data is used by QuML players while rendering a question set. In case when the question set is added to another question set, then the settings of the containing question set will override the configurations of the contained question set.

### Instructions

Similar to questions, question sets can also have instructions. Such instructions are defined in HTML format and stored in the instructions part of the question set data \(similar to question instructions\).

Instructions:

```text
{
    “instructions”: “<div>...</div>”
}
```

### Feedback

The feedback configuration for question sets is same as that of questions. Multiple feedbacks can be configured and each feedback should be defined in HTML format.

Feedback:

```text
{
    “feedback”: {
        “<feedback_1>”: “<div>...</div>”,
        “<feedback_2>”: “<div>...</div>”,
        … 
    }
}
```

### Hints

The hints configuration for question sets is same as that of questions. Multiple hints can be configured and each hint should be defined in HTML format.

Hints:

```text
{
    “hints”: {
        “<hint_1>”: “<div>...</div>”,
        “<hint_2>”: “<div>...</div>”,
        … 
    }
}
```

### i18n data

Texts in a question set \(instructions, feedback, hints\) can also be rendered in multiple locales. QuML allows instructions, feedback and hints to be defined in multiple locales \(in the same way as defined for questions\).

### Navigation mode

The navigation mode determines the general paths that the student may take during the test session. It is an enumeration with two possible values: “linear” and “non-linear”.

Navigation Mode:

```text
{
    “navigationMode”: “linear | non-linear”
}
```

* A question set in linear mode restricts the student to attempt each question in turn. Once the student moves on they are not permitted to return. 
* A question set in nonlinear mode removes this restriction - the student is free to navigate to any question in the question set at any time.

### Time limits

The time limits \(if any\) for a question set can be defined as **timeLimits** data. Both minimum and maximum time constraints can be provided for the complete set and/or for one question as well. This configuration is used by QuML players to impose time limits \(both minimum and maximum\) for attempting a question set.

Time Limits:

```text
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

### Show Hints

This configuration is used by QuML players to enable/disable hints for the student while using the question set. It should be set to either true or false based on the context in which the question set is being used.

Show Hints:

```text
{
    “showHints”: “true | false”
}
```

### Questions

If a question set contains questions, the association should be defined in the “questions” section. This section is mutually exclusive with “questionSets” section, i.e. a question set contain only either question sets or questions but not both.

Questions:

```text
{
“questions”: {QuestionDef Object}
}
```

_QuestionDef_

| Attribute | Schema | Description |
| :--- | :--- | :--- |
| shuffle | dataType: boolean, required: false, defaultValue: false |  |
| totalQuestions | dataType: integer, required: true |  |
| maxQuestions | dataType: integer, required: true |  |
| list | dataType: list of string, required: true | List of question identifiers that are added to the question set. |

### Question Sets

A question set can contain either other question sets or individual questions. If a question set contains other question sets, the association should be specified in the “questionSets” section.

QuestionSets:

```text
{
“questionSets”: [{QuestionSetDef Object}, {QuestionSetDef Object}, … ]
}
```

_QuestionSetDef_

| Attribute | Schema | Description |
| :--- | :--- | :--- |
| questionSetId | dataType: string, required: true | identifier of the member question set |
| shuffle | dataType: boolean, required: false, defaultValue: false | if questions in the member question set should be shuffled or not when presented to the student |
| totalQuestions | dataType: integer, required: true | total number of questions in the member question set. applicable only if the member question set has questions |
| maxQuestions | dataType: integer, required: true | number of questions in the member question set that should be used in one session. applicable only if the member question set has questions |
| preConditions | dataType: list of PreConditionDef objects, required: false | conditions to be validated before rendering the question set. generally depends on the outcomes of question sets presented earlier in the session |
| branchRules | dataType: list of BranchRuleDef objects, required: false | Evaluated after question set is complete and jumps forward to the specified target. |

_PreConditionDef_

| Attribute | Schema | Description |
| :--- | :--- | :--- |
| questionSetId | dataType: string, required: true | Identifier of the question set whose outcome will be used to do the precondition check |
| match | dataType: list of OutcomeMatchDef objects, required: true |  |

_OutcomeMatchDef_

| Attribute | Schema | Description |
| :--- | :--- | :--- |
| outcomeVariable | dataType: string, required: true |  |
| operator | dataType: boolean, required: true, range: lt, le, eq, gt, ge, in |  |
| value | dataType: any, required: true |  |

_BranchRuleDef_

| Attribute | Schema | Description |
| :--- | :--- | :--- |
| target | dataType: TargetSetDef, required: true |  |
| match | dataType: list of OutcomeMatchDef objects, required: true |  |

_TargetSetDef_

Target of a branch rule can be either another question set or exit of the question set. One \(and only one\) of these two must configured as a target for a branch rule.

| Attribute | Schema | Description |
| :--- | :--- | :--- |
| questionSetId | dataType: string, required: false |  |
| exit | dataType: boolean, required: false, defaultValue: false |  |

### Outcome Declaration

Question Sets also have outcome variables similar to Question outcome variables. They are also declared by outcome declarations. The same set of reserved and built-in variables defined for questions are applicable to question sets also. The only difference is that their value is set during outcome processing \(in contrast to response processing of questions\).

Outcome declaration is a JSON object in key-value format \(same as for questions\). The keys in the JSON are the outcome variables and values are of type OutcomeVariableDef.

OutcomeDeclaration:

```text
{
    “outcomeDeclaration”: {
      “<outcome_variable_1>”: OutcomeVariableDef Object,
          “<outcome_variable_2>”: OutcomeVariableDef Object,
    … 
  }
}
```

_OutcomeVariableDef_

| Attribute | Schema | Description |
| :--- | :--- | :--- |
| cardinality | dataType: string, required: true, range: “single”, “multiple”, “ordered” | Used to specify whether the outcome variable will have a single value, multiple values or an ordered list of values. |
| type | dataType: string, required: true, range: “string”, “integer”, “float”, “boolean”, “map”, “uri”, “points”, “coordinate” |  |
| defaultValue | dataType: any, required: false |  |
| range | dataType: list of any, required: false |  |

### Outcome Processing

Outcome processing takes place each time the student submits the responses for a question. It happens after any \(question level\) response processing triggered by the submission. Because outcome processing occurs each time the student submits responses, the resulting values of the question set outcomes may be used to activate question set level feedback during the test or to control the behaviour of subsequent parts through the use of preConditions and branchRules.

Similar to response processing of questions, outcome processing rules of a question set can be defined using custom evaluation logic or use one of the existing outcome processing templates.

#### Custom Outcome Processing

The custom outcome processing logic using javascript should be defined as part of the “eval” attribute of “outcomeProcessing” data.

```text
{
    “outcomeProcessing”: {
           “eval”: “<javascript code to set the outcome variables. Library methods can be used to refer and set question and question set variables>“
    }
}
```

> “eval” data is mandatory if outcome processing templates are not used for the question set. If both template and evaluation logic are present, template is executed first and then the custom evaluation logic is executed.

#### Outcome Processing Templates

1. **SUM\_OF\_SCORES**: This outcome processing template adds the value of SCORE outcome variables of questions or question sets that are part of the question set and sets the computed value as the SCORE outcome of the question set.
2. **AVG\_OF\_SCORES**: This outcome processing template computes the average of SCORE outcome values of questions or question sets that are part of the question set and sets the computed value as the SCORE outcome of the question set.
3. **WEIGHTED\_AVG\_OF\_SCORES**: This template is similar to the AVG\_OF\_SCORES templates with an additional configuration to specify weightage for each question or question set. The weightage configuration can be provided using an additional parameter **weightageConfig** in outcome processing.

Outcome processing schema for using outcome processing templates:

| Attribute | Schema | Description |
| :--- | :--- | :--- |
| template | dataType: string, required: false, range: SUM\_OF\_SCORES, AVG\_OF\_SCORES, WEIGHTED\_AVG\_OF\_SCORES | name of the outcome processing template to be used for the question. “template” is mandatory if outcome processing templates are used for the question |
| ignoreNullValues | dataType: string, required: false, defaultValue: false | If set to true, the processing will ignore any SCORE outcomes that are null. This is relevant while using average based templates |
| weightageConfig | dataType: map, required: false | configuration to set weightages for questions or question sets when WEIGHTED\_AVG\_OF\_SCORES template is used. If not provided, same weightage is given to all |
| mappingConfig | dataType: string, required: false | configuration to set additional outcome variables \(other than SCORE\). Same as the mappingConfig defined in question responseProcessing specification |

## Question Set Metadata

Similar to question metadata, question set metadata should also capture enough detail that is needed for discovering, delivering and composing question sets into tests.

### questionSetType

A question set can be comprised of a materialized list of questions, or can also be dynamically built at runtime by using a criteria to select member questions. A set can be either - _materialized_ or _dynamic_.

### criteria

Criteria to be used when the set type is dynamic . Criteria should be provided in JSON map format where key should be a metadata field of question and value should be either a single value or list of values.

```text
{
    “<metadata_field_1>”: <value>, 
    “<metadata_field_2>”: [ <value>, <value>, ... ], 
    ...
}
```

### usedFor

A question set can be used either for formative or summative assessments. A question set tagged for usage in summative assessment should not be used in formative assessment. Allowed values for this field are - _practice_ \(formative assessments\) or _exam_ \(summative assessments\).

### purpose

This metadata field should be used to tag the purpose served by the question set - _recall, explore, sense, assess, teach, revise_.

### visibility

Some question sets can be made available only for those who created it and/or for some specific systems or use cases. Also, a question set could be created to be used only within one test and others cannot use it. This metadata can be used to tag the question set visibility - _private, public or parent_.

### version

Version of the QuML specification using which the question set is created.

### members

List of questions or question set identifiers that are added to the question set.

### Curricular Metadata

This is a group of metadata fields to capture the pedagogic intent of the question set - which concepts are tested by the question set, etc. The metadata fields in this category vary for each domain. For example, questions of K-12 domain will have the following curricular metadata fields: _board, grade, subject, medium and topics_.

