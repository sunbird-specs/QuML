## Question Set Information Model

A test is a group of question sets and questions with an associated set of rules that determine which of the questions the candidate sees, in what order, and in what way the candidate interacts with them. The rules describe the valid paths through the test, when responses are submitted for response processing and when (if at all) feedback is to be given. Tests are represented using Question Sets in QML.

For each test session, question sets are selected and arranged into order according to rules defined in the containing question set. This process of selection and ordering defines a basic structure for each part of the test on a per-session basis. The paths that a student may take through this structure are then controlled by the mode settings for the question set and possibly by further preConditions or branchRules evaluated during the test session itself. 

![Question set](https://github.com/sunbird-specs/qml/blob/master/v1/images/Question_Set_Structure_1.png)

The below figure illustrates a specific instance of the same question set after the application of selection and ordering rules. A rule in question set S01 selects just one of S01A and S01B, a rule in S02 shuffles the order of the items contained by it and, finally, rules in S03 select 1 out of the 2 items it contains and shuffles the result.

![Question set 2](https://github.com/sunbird-specs/qml/blob/master/v1/images/Question_Set_Structure_2.png)

