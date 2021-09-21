# Common Elements

All of the common elements used within this specification are described in this Document. Common elements include the data types, and their properties used in this specificaiton.

## Cardinality

All the variables in the specification will have a cardinality. The cardinality attribute defines the number of values allowed for the variable and whether the variables are ordered.

Supported types of cardinality are:

* _single_: only one value is allowed for the variable
* _multiple_: one or more values are allowed for the variable
* _ordered_: ordered list of values are allowed for the variable

## Data Types

### string

String data types should be used to represent string literal values.

### integer

Integer data types should be used to represent numbers without a fractional part.

### float

Float data types should be used to represent numbers with a fractional part.

### boolean

Boolean data types should be used to represent values with two states: true or false.

### uri

URI data types should be used to represent Uniform Resource Identifier \(URI\) reference values.

### map

Map data type should be used for variables whose values are key-value pairs. Map is a non-primitive data type with the following attributes.

| Attribute | Schema | Description |
| :--- | :--- | :--- |
| key | dataType: _string_,   required: _true_ | a map cannot contain duplicate keys and each key can map at most one value |
| value | dataType: _any_,   required: _false_, defaultValue: _NULL_ | value can be of any other data type defined in QuML. value is optional and is by default set to NULL, if not provided |

### coordinate

Coordinate data type should be used to represent coordinates \(x and y\) of a single point in a canvas.

| Attribute | Schema | Description |
| :--- | :--- | :--- |
| x | dataType: _float_,   required: _true_ | x-coordinate value of the point |
| y | dataType: _float_,   required: _true_ | y-coordinate value of the point |

### points

Points data type should be used for variables whose values represent an area on a canvas. Points are made up of a shape and the coordinates that define the boundary of the area.

| Attribute | Schema | Description |
| :--- | :--- | :--- |
| shape | dataType: _string_,   required: _true_,   range: _“point”, “circle”, “ellipse”, “poly”, “rect”_ | represents the shape of points. it must be equal one of the predefined values defined in the range |
| coordinates | dataType: _list of coordinate objects_,   required: _true_ | Coordinates should have at least one coordinate object. The number of coordinates depend on the shape. |

### media

Media data type should be used for representing a single media file. Media are re-usable to create assets which are used in questions and/or tests.

| Attribute | Schema | Description |
| :--- | :--- | :--- |
| id | dataType: _string_,   required: _true_ | identifier of the media object |
| mimeType | dataType: _string_,   required: _true_ | technical mime type of the asset used by QuML players to understand the format of the asset. Supported types are image/png, audio/mp3, video/mp4, and video/webm |
| mediaType | dataType: _string_, required: _true_ | type of the asset. this should be auto-derived from the mimeType value, current supported types are image, audio and video |
| src | dataType: _string_, required: _true_ | path of the media file. QuML players will use this value to load all the media used in a question/test |
| baseUrl | dataType: _string_, required: _false_ | baseUrl of the server where the media is stored. Base URL can be used by QuML players to load media in a browser without CORS issues. this is achieved by QuML server proxying path to the provided base url. |

