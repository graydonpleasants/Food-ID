## Specification

### Format

The files describing the RESTful API in accordance with the Food-ID specification are represented as JSON objects and conform to the JSON standards. YAML, being a superset of JSON, can be used as well to 
represent a Food-id specification file.

For example, if a field is said to have an array value, the JSON array representation will be used:

```js
{
   "field" : [...]
}
```

While the API is described using JSON it does not impose a JSON input/output to the API itself.

All field names in the specification are **case sensitive**.

The schema exposes two types of fields. Fixed fields, which have a declared name, and Patterned fields, which declare a regex pattern for the field name. Patterned fields can have multiple occurrences as long as each has a unique name. 

### Data Types

Primitive data types in the Swagger Specification are based on the types supported by the [JSON-Schema Draft 4](https://tools.ietf.org/html/draft-zyp-json-schema-04#section-3.5). Models are described using the [Schema Object](#schemaObject) which is a subset of JSON Schema Draft 4.

An additional primitive data type `"file"` is used by the [Parameter Object](#parameterObject) and the [Response Object](#responseObject) to set the parameter type or the response as being a file.

<a name="dataTypeFormat"></a>Primitives have an optional modifier property `format`. Swagger uses several known formats to more finely define the data type being used. However, the `format` property is an open `string`-valued property, and can have any value to support documentation needs. Formats such as `"email"`, `"uuid"`, etc., can be used even though they are not defined by this specification. Types that are not accompanied by a `format` property follow their definition from the JSON Schema (except for `file` type which is defined above). The formats defined by the Swagger Specification are:


Common Name | [`type`](#dataTypeType) | [`format`](#dataTypeFormat) | Comments
----------- | ------ | -------- | --------
integer | `integer` | `int32` | signed 32 bits
long | `integer` | `int64` | signed 64 bits
float | `number` | `float` | |
double | `number` | `double` | |
string | `string` | | |
byte | `string` | `byte` | base64 encoded characters
binary | `string` | `binary` | any sequence of octets
boolean | `boolean` | | |
date | `string` | `date` | As defined by `full-date` - [RFC3339](http://xml2rfc.ietf.org/public/rfc/html/rfc3339.html#anchor14)
dateTime | `string` | `date-time` | As defined by `date-time` - [RFC3339](http://xml2rfc.ietf.org/public/rfc/html/rfc3339.html#anchor14)
password | `string` | `password` | Used to hint UIs the input needs to be obscured.

### Schema

#### <a name="swaggerObject"></a>Swagger Object

This is the root document object for the API specification. It combines what previously was the Resource Listing and API Declaration (version 1.2 and earlier) together into one document.

##### Fixed Fields

Field Name | Type | Description
---|:---:|---
<a name="swaggerSwagger"></a>swagger | `string` | **Required.** Specifies the Swagger Specification version being used. It can be used by the Swagger UI and other clients to interpret the API listing. The value MUST be `"2.0"`.
<a name="swaggerInfo"></a>info | [Info Object](#infoObject) | **Required.** Provides metadata about the API. The metadata can be used by the clients if needed.
<a name="swaggerHost"></a>host | `string` | The host (name or ip) serving the API. This MUST be the host only and does not include the scheme nor sub-paths. It MAY include a port. If the `host` is not included, the host serving the documentation is to be used (including the port). The `host` does not support [path templating](#pathTemplating).
<a name="swaggerBasePath"></a>basePath | `string` | The base path on which the API is served, which is relative to the [`host`](#swaggerHost). If it is not included, the API is served directly under the `host`. The value MUST start with a leading slash (`/`). The `basePath` does not support [path templating](#pathTemplating). 
<a name="swaggerSchemes"></a>schemes | [`string`] | The transfer protocol of the API. Values MUST be from the list: `"http"`, `"https"`, `"ws"`, `"wss"`. If the `schemes` is not included, the default scheme to be used is the one used to access the Swagger definition itself.
<a name="swaggerConsumes"></a>consumes | [`string`] | A list of MIME types the APIs can consume. This is global to all APIs but can be overridden on specific API calls. Value MUST be as described under [Mime Types](#mimeTypes).
<a name="swaggerProduces"></a>produces | [`string`] | A list of MIME types the APIs can produce. This is global to all APIs but can be overridden on specific API calls. Value MUST be as described under [Mime Types](#mimeTypes).
<a name="swaggerPaths"></a>paths | [Paths Object](#pathsObject) | **Required.** The available paths and operations for the API.
<a name="swaggerDefinitions"></a>definitions | [Definitions Object](#definitionsObject) | An object to hold data types produced and consumed by operations.
<a name="swaggerParameters"></a>parameters | [Parameters Definitions Object](#parametersDefinitionsObject) | An object to hold parameters that can be used across operations. This property *does not* define global parameters for all operations.
<a name="swaggerResponses"></a>responses | [Responses Definitions Object](#responsesDefinitionsObject) | An object to hold responses that can be used across operations. This property *does not* define global responses for all operations.
<a name="swaggerSecurityDefinitions"></a>securityDefinitions | [Security Definitions Object](#securityDefinitionsObject) | Security scheme definitions that can be used across the specification.
<a name="swaggerSecurity"></a>security | [[Security Requirement Object](#securityRequirementObject)] | A declaration of which security schemes are applied for the API as a whole. The list of values describes alternative security schemes that can be used (that is, there is a logical OR between the security requirements). Individual operations can override this definition.
<a name="swaggerTags"></a>tags | [[Tag Object](#tagObject)] | A list of tags used by the specification with additional metadata. The order of the tags can be used to reflect on their order by the parsing tools. Not all tags that are used by the [Operation Object](#operationObject) must be declared. The tags that are not declared may be organized randomly or based on the tools' logic. Each tag name in the list MUST be unique.
<a name="swaggerExternalDocs"></a>externalDocs | [External Documentation Object](#externalDocumentationObject) | Additional external documentation.

##### Patterned Objects 

Field Pattern | Type | Description
---|:---:|---
<a name="swaggerExtensions"></a>^x- | Any | Allows extensions to the Swagger Schema. The field name MUST begin with `x-`, for example, `x-internal-id`. The value can be `null`, a primitive, an array or an object. See [Vendor Extensions](#vendorExtensions) for further details.

#### <a name="infoObject"></a>Info Object

The object provides metadata about the API. The metadata can be used by the clients if needed, and can be presented in the Swagger-UI for convenience.

##### Fixed Fields

Field Name | Type | Description
---|:---:|---
<a name="infoTitle"></a>title | `string` | **Required.** The title of the application.
<a name="infoDescription"></a>description | `string` | A short description of the application. [GFM syntax](https://guides.github.com/features/mastering-markdown/#GitHub-flavored-markdown) can be used for rich text representation.
<a name="infoTermsOfService"></a>termsOfService | `string` | The Terms of Service for the API.
<a name="infoContact"></a>contact | [Contact Object](#contactObject) | The contact information for the exposed API.
<a name="infoLicense"></a>license | [License Object](#licenseObject) | The license information for the exposed API.
<a name="infoVersion"></a>version | `string` | **Required** Provides the version of the application API (not to be confused with the specification version).

##### Patterned Objects 

Field Pattern | Type | Description
---|:---:|---
<a name="infoExtensions"></a>^x- | Any | Allows extensions to the Swagger Schema. The field name MUST begin with `x-`, for example, `x-internal-id`. The value can be `null`, a primitive, an array or an object. See [Vendor Extensions](#vendorExtensions) for further details.

##### Info Object Example:

```js
{
  "title": "Swagger Sample App",
  "description": "This is a sample server Petstore server.",
  "termsOfService": "http://swagger.io/terms/",
  "contact": {
    "name": "API Support",
    "url": "http://www.swagger.io/support",
    "email": "support@swagger.io"
  },
  "license": {
    "name": "Apache 2.0",
    "url": "http://www.apache.org/licenses/LICENSE-2.0.html"
  },
  "version": "1.0.1"
}
```

```yaml
title: Swagger Sample App
description: This is a sample server Petstore server.
termsOfService: http://swagger.io/terms/
contact:
  name: API Support
  url: http://www.swagger.io/support
  email: support@swagger.io
license:
  name: Apache 2.0
  url: http://www.apache.org/licenses/LICENSE-2.0.html
version: 1.0.1
```

#### <a name="referenceObject"></a>Reference Object

A simple object to allow referencing other definitions in the specification. It can be used to reference parameters and responses that are defined at the top level for reuse.

The Reference Object is a [JSON Reference](http://tools.ietf.org/html/draft-pbryan-zyp-json-ref-02) that uses a [JSON Pointer](http://tools.ietf.org/html/rfc6901) as its value. For this specification, only [canonical dereferencing](https://tools.ietf.org/html/draft-zyp-json-schema-04#section-7.2.3) is supported.

##### Fixed Fields
Field Name | Type | Description
---|:---:|---
<a name="referenceRef"></a>$ref | `string` | **Required.** The reference string. 

##### Reference Object Example

```js
{
	"$ref": "#/definitions/Pet"
}
```

```yaml
$ref: '#/definitions/Pet'
```

##### Relative Schema File Example
```js
{
  "$ref": "Pet.json"
}
```

```yaml
$ref: 'Pet.yaml'
```

##### Relative Files With Embedded Schema Example
```js
{
  "$ref": "definitions.json#/Pet"
}
```

```yaml
$ref: 'definitions.yaml#/Pet'
```

#### <a name="schemaObject"></a>Schema Object

The Schema Object allows the definition of input and output data types. These types can be objects, but also primitives and arrays. This object is based on the [JSON Schema Specification Draft 4](http://json-schema.org/) and uses a predefined subset of it. On top of this subset, there are extensions provided by this specification to allow for more complete documentation.

Further information about the properties can be found in [JSON Schema Core](https://tools.ietf.org/html/draft-zyp-json-schema-04) and [JSON Schema Validation](https://tools.ietf.org/html/draft-fge-json-schema-validation-00). Unless stated otherwise, the property definitions follow the JSON Schema specification as referenced here.

The following properties are taken directly from the JSON Schema definition and follow the same specifications:
- $ref - As a [JSON Reference](https://tools.ietf.org/html/draft-pbryan-zyp-json-ref-03)
- format (See [Data Type Formats](#dataTypeFormat) for further details)
- title
- description ([GFM syntax](https://guides.github.com/features/mastering-markdown/#GitHub-flavored-markdown) can be used for rich text representation)
- default (Unlike JSON Schema, the value MUST conform to the defined type for the Schema Object)
- multipleOf
- maximum
- exclusiveMaximum
- minimum
- exclusiveMinimum
- maxLength
- minLength
- pattern
- maxItems
- minItems
- uniqueItems
- maxProperties
- minProperties
- required
- enum
- type

The following properties are taken from the JSON Schema definition but their definitions were adjusted to the Swagger Specification. Their definition is the same as the one from JSON Schema, only where the original definition references the JSON Schema definition, the [Schema Object](#schemaObject) definition is used instead.
- items
- allOf
- properties
- additionalProperties

Other than the JSON Schema subset fields, the following fields may be used for further schema documentation.
