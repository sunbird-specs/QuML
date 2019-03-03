## Common Elements

All of the common elements used within this specification are described in this Document. Common elements include the data types, their properties, and common classes used by other objects defined in this specificaiton.

### Cardinality

All the variables in the specification will have a cardinality. The cardinality attribute defines the number of values allowed for the variable and whether the variables are ordered. 

Supported types of cardinality are:
- _single_: only one value is allowed for the variable
- _multiple_: one or more values are allowed for the variable
- _ordered_: ordered list of values are allowed for the variable

### Data Types

#### string
String data types should be used to represent string literal values.

#### integer
Integer data types should be used to represent numbers without a fractional part.

#### float
Float data types should be used to represent numbers with a fractional part.

#### boolean
Boolean data types should be used to represent values with two states: true or false.

#### uri
URI data types should be used to represent Uniform Resource Identifier (URI) reference values.

#### map
Map data type should be used for variables whose values are key-value pairs. Map is a non-primitive data type with the following attributes.

| Attribute | Schema | Description |
| --- | ----- | ----------- |
| key | dataType: String, <br/> required: true | a map cannot contain duplicate keys and each key can map at most one value |
| value | dataType: any, <br/> required: false, defaultValue: NULL | value can be of any other data type defined in QML. value is optional and is by default set to NULL, if not provided |

### Asset class

