## Library Methods

QML players should provide implementation for the library methods which can be used in response, template and outcome processing of questions and question sets. 

### Question Library Methods

#### getResponseVariable
Returns the value of a response variable. If no response variable is found or if the value is not set, NULL value will be returned.

*Parameters:*
- name of the response variable

*Returns:*
- value of the response variable

#### getOutcomeVariable
Returns the value of a outcome variable. If no outcome variable is found or if the value is not set, NULL value will be returned.

*Parameters:*
- name of the outcome variable

*Returns:*
- value of the outcome variable

#### setOutcomeVariable
Sets the specified value for specified outcome variable. 

*Parameters:*
- name of the outcome variable
- value to be set for the specified outcome variable

*Returns:*
- true, if the outcome variable is updated successfully
- false, if the outcome variable is not updated, e.g.: if variable is not declared or if the data type of the value does not match with the declaration

#### getTemplateVariable
Returns the value of a template variable. If no template variable is found or if the value is not set, NULL value will be returned.

*Parameters:*
- name of the template variable

*Returns:*
- value of the template variable

#### setTemplateVariable
Sets the specified value for specified template variable. 

*Parameters:*
- name of the template variable
- value to be set for the specified template variable

*Returns:*
- true, if the template variable is updated successfully
- false, if the template variable is not updated, e.g.: if variable is not declared or if the data type of the value does not match with the declaration

#### matchCorrect
Compares the value of response variables with the correct response value configured in the response declaration. If values of all variables match with the correct response, the outcome variable SCORE is set to 1.0, else if the values do not match or any of the variables do not have correct response definition, SCORE is set to 0.0. If no response variables are defined, SCORE is set to NULL.

*Returns:*
- the value of outcome variable SCORE - either 1.0, 0.0 or NULL

#### mapResponse
Sets the value of outcome variable SCORE using mapping and areaMapping configuration of response variables provided in the response declaration. If mapping data is not present for any of the response variable, the response variable is not used for computing the score. Optionally if a mapping config is provided, other outcome variable values are also set using the mapping config.

*Parameters:*
- mappingConfig: optional parameter to set values of outcome variables other than SCORE

#### matchTemplate
Sets the value of outcome variable SCORE using matchTemplate configuration passed as input to the method. Optionally if a mapping config is provided, other outcome variable values are also set using the mapping config.

*Parameters:*
- matchTemplateConfig: required config to set the value of outcome variable SCORE based on the values of one or more template variables
- mappingConfig: optional parameter to set values of outcome variables other than SCORE


### Question Set Library Methods
