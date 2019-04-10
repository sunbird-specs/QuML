## Common Elements

All of the common elements used within this specification are described in this Document. Common elements include the data types, and their properties used in this specificaiton.

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
| key | dataType: *string*, <br/> required: *true* | a map cannot contain duplicate keys and each key can map at most one value |
| value | dataType: *any*, <br/> required: *false*, defaultValue: *NULL* | value can be of any other data type defined in QuML. value is optional and is by default set to NULL, if not provided |

#### coordinate
Coordinate data type should be used to represent coordinates (x and y) of a single point in a canvas.

| Attribute | Schema | Description |
| --- | ----- | ----------- |
| x | dataType: *float*, <br/> required: *true* | x-coordinate value of the point |
| y | dataType: *float*, <br/> required: *true* | y-coordinate value of the point |

#### points
Points data type should be used for variables whose values represent an area on a canvas. Points are made up of a shape and the coordinates that define the boundary of the area.

| Attribute | Schema | Description |
| --- | ----- | ----------- |
| shape | dataType: *string*, <br/> required: *true*, <br/> range: *“point”, “circle”, “ellipse”, “poly”, “rect”* | represents the shape of points. it must be equal one of the predefined values defined in the range |
| coordinates | dataType: *list of coordinate objects*, <br/> required: *true* | Coordinates should have at least one coordinate object. The number of coordinates depend on the shape. |

#### media
Media data type should be used for representing a single media file. Media are re-usable to create assets which are used in questions and/or tests.

| Attribute | Schema | Description |
| -- | ------ | --------- |
| name | dataType: *string*, <br/> required: *true* | name of the media object |
| mimeType | dataType: *string*, <br/> required: *true* | technical mime type of the asset used by QuML players to understand the format of the asset. Supported types are image/png, audio/mp3, video/mp4, and video/webm |
| mediaType | dataType: *string*, required: *true* | type of the asset. this should be auto-derived from the mimeType value, current supported types are image, audio and video |
| url | dataType: *string*, required: *true* | path of the media file. QuML players will use this value to load all the media used in a question/test |

#### asset
Asset data type should be used for representing a media asset which can be used in questions and/or tests. An asset is made up of multiple media, like variants of the same media file (mp4 vs webm, multiple resolutions of the same media file, etc), thumbnails, poster images, and icons. An asset contains all the information required to render a media.

| Attribute | Schema | Description |
| -- | ------ | --------- |
| name | dataType: *string*, <br/> required: *true* | name of the asset |
| mainMedia | dataType: *media*, <br/> required: *true* | main media object for the asset |
| media | dataType: *list of media objects*, required: *false* | list of alternate media for the asset |
| icon | dataType: *media*, required: *false* | icon for the asset |
| thumbnails | dataType: *list of media objects*, required: *false* | list of thumbnails for the asset |
| posterImage | dataType: *media object*, required: *false* | poster image for the asset |





