# Library Methods

QuML players should provide implementation for the library methods which can be used in response, template and outcome processing of questions and question sets.

## Question Library Methods

### getResponseVariable

Returns the value of a response variable. If no response variable is found or if the value is not set, NULL value will be returned.

_Parameters:_

* name of the response variable

_Returns:_

* value of the response variable

### getOutcomeVariable

Returns the value of a outcome variable. If no outcome variable is found or if the value is not set, NULL value will be returned.

_Parameters:_

* name of the outcome variable

_Returns:_

* value of the outcome variable

### setOutcomeVariable

Sets the specified value for specified outcome variable.

_Parameters:_

* name of the outcome variable
* value to be set for the specified outcome variable

_Returns:_

* true, if the outcome variable is updated successfully
* false, if the outcome variable is not updated, e.g.: if variable is not declared or if the data type of the value does not match with the declaration

### getTemplateVariable

Returns the value of a template variable. If no template variable is found or if the value is not set, NULL value will be returned.

_Parameters:_

* name of the template variable

_Returns:_

* value of the template variable

### setTemplateVariable

Sets the specified value for specified template variable.

_Parameters:_

* name of the template variable
* value to be set for the specified template variable

_Returns:_

* true, if the template variable is updated successfully
* false, if the template variable is not updated, e.g.: if variable is not declared or if the data type of the value does not match with the declaration

### matchCorrect

Compares the value of response variables with the correct response value configured in the response declaration. If values of all variables match with the correct response, the outcome variable SCORE is set to 1.0, else if the values do not match or any of the variables do not have correct response definition, SCORE is set to 0.0. If no response variables are defined, SCORE is set to NULL.

_Returns:_

* the value of outcome variable SCORE - either 1.0, 0.0 or NULL

### mapResponse

Sets the value of outcome variable SCORE using mapping and areaMapping configuration of response variables provided in the response declaration. If mapping data is not present for any of the response variable, the response variable is not used for computing the score. Optionally if a mapping config is provided, other outcome variable values are also set using the mapping config.

_Parameters:_

* mappingConfig: optional parameter to set values of outcome variables other than SCORE

### matchTemplate

Sets the value of outcome variable SCORE using matchTemplate configuration passed as input to the method. Optionally if a mapping config is provided, other outcome variable values are also set using the mapping config.

_Parameters:_

* matchTemplateConfig: required config to set the value of outcome variable SCORE based on the values of one or more template variables
* mappingConfig: optional parameter to set values of outcome variables other than SCORE

## Question Set Library Methods

### getQuestionSets

Returns the identifiers of question sets that are members of the question set.

_Returns:_

* list of question set identifiers, NULL if the question set do not have any member sets

### getQuestions

Returns the identifiers of questions that are members of the question set.

_Returns:_

* list of question identifiers, NULL if the question set do not have any member questions

### getQuestionSetOutcomeVariables

Returns a map of outcome variables and their values for the specified question set identifier. The question set should be a member of the current question set.

_Parameters:_

* identifier of the question set which is a member of the current question set

_Returns:_

* map of outcome variables and values of the specified question set, NULL if the question set is not a member of the current question set

### getQuestionSetDuration

Returns the time spent \(in milliseconds\) for the specified question set identifier. The question set should be a member of the current question set.

_Parameters:_

* identifier of the question set which is a member of the current question set

_Returns:_

* duration of the specified set in the current session, NULL if the question set is not a member of the current question set or if the question set is not attempted by the user in the current session

### getQuestionOutcomeVariables

Returns a map of outcome variables and their values for the specified question identifier. The question should be a member of the current question set.

_Parameters:_

* identifier of the question which is a member of the current question set

_Returns:_

* map of outcome variables and values of the specified question, NULL if the question is not a member of the current question set

### getQuestionResponseVariables

Returns a map of response variables \(including built-in variables numAttempts & duration\) and their values for the specified question identifier. The question should be a member of the current question set.

_Parameters:_

* identifier of the question which is a member of the current question set

_Returns:_

* map of response variables and values of the specified question, NULL if the question is not a member of the current question set

### getOutcomeVariable

Returns the value of a outcome variable. If no outcome variable is found or if the value is not set, NULL value will be returned.

_Parameters:_

* name of the outcome variable

_Returns:_

* value of the outcome variable

### setOutcomeVariable

Sets the specified value for specified outcome variable.

_Parameters:_

* name of the outcome variable
* value to be set for the specified outcome variable

_Returns:_

* true, if the outcome variable is updated successfully
* false, if the outcome variable is not updated, e.g.: if variable is not declared or if the data type of the value does not match with the declaration

