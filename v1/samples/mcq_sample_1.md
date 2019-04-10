
This sample demonstrates how QuML is used to model a simple multiple choice question (with multiple answers), configure the interactions & process the responses to produce results.

![sample mcq image](https://github.com/sunbird-specs/qml/blob/master/v1/images/sample_question_mcq_1.png)

This question is a multiple choice question where the correct answer has two values - “Hydrogen” and “Oxygen”. Below is the logic for evaluating and processing the question:
- If student selects both the correct answers, the score should be 1.0
- If student selects only one of the correct answers, the score should be 0.5
- If student selects any of the wrong answers, a value of 0.5 should be removed from the total score
- If total score if 1.0, show the message “Well done!!!” 
- If total score is between 0 and 1.0 (not including 0 and 1.0, i.e. 0 < score < 1.0), show the message “Better luck next time!!!”
- If total score is less than or equal to 0, show the message “You need to work harder!!!”

**Body**

```
<!-- Question Title -->
<div class=”title”>Composition of water</div>

<!-- Question Text -->
<div class=”sub-title”>Which of the following elements are used to form water?</div>

<!-- Answer options defined using html checkbox elements -->
<div class=”vertical-options”>
<input type="checkbox" name="element" data-multi-choice-interaction data-response-variable="RESPONSE" value="Carbon"/>
         <span class=”paragraph”>Carbon</span><br>

<input type="checkbox" name="element" data-multi-choice-interaction data-response-variable="RESPONSE" value="Oxygen">
         <span class=”paragraph”>Oxygen</span><br>

<input type="checkbox" name="element" data-multi-choice-interaction data-response-variable="RESPONSE" value="Hydrogen">
         <span class=”paragraph”>Hydrogen</span><br>

<input type="checkbox" name="element" data-multi-choice-interaction data-response-variable="RESPONSE" value="Nitrogen">
         <span class=”paragraph”>Nitrogen</span><br>

</div>
```

**Response Declaration**

```
{
    “responseDeclaration”: {
            “RESPONSE”: {
                    “cardinality”: “multiple”,
                    “type”: “string”,
                    “correctResponse”: {
                            value: [“Oxygen”, “Hydrogen”]
                    },
                    “mapping”: [
                            { “key”: “Carbon”, “value”: -0.5, “caseSensitive”: false},
                            { “key”: “Oxygen”, “value”: 0.5, “caseSensitive”: false},
                            { “key”: “Hydrogen”, “value”: 0.5, “caseSensitive”: false},
                            { “key”: “Nitrogen”, “value”: -0.5, “caseSensitive”: false}
                    ]
            }
    }
}
```

**Outcome Declaration**

```
{
    “outcomeDeclaration”: {
            “SCORE”: {
                    “cardinality”: “single”,
                    “type”: “float”,
                    “defaultValue”: 0.0
            },
            “FEEDBACK”: {
                    “cardinality”: “single”,
                    “type”: “string”,
                    “range”: [“feedback_01”, “feedback_02”, “feedback_03”],
            }
    }
}
```

**Response Processing**

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

**Feedback**

```
{
    “feedback”: {
           “feedback_01”: “<h1>Well done!!!</h1>”,
           “feedback_02”: “<h1>Better luck next time!!!</h1>”
           “feedback_03”: “<h1>You need to work harder!!!</h1>”
    }
}
```
