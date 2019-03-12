# OpenApi specification and fuzzing.

This document will go over parts of OpenApi [3.0.2 spec](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.2.md) and their usability in fuzz case generation. 

## Brief description of the structure of the specification

Below is a partial breakdown of the OpenApi spec. Fields that are required in every spec are marked with (REQUIRED). Fuzzing wise interesting fields are **bolded**

* openapi: string field describing the openAPI version of the object
* info: object containing metadata of the API (REQUIRED)
  * title (REQUIRED)
  * description
  * termsOfService
  * contact
  * license
  * **version** (REQUIRED)
* servers: Array of server objects containing connectivity information
  * **url**
  * description
  * **variables**
* paths: (REQUIRED) contains all the available endpoints and operations
  * **$ref**
  * summary
  * description
  * GET/PUT/POST/DELETE/OPTIONS/HEAD/PATCH/TRACE
    * tags
    * summary
    * description
    * externalDocs
    * operationId
    * **parameters**
    * **requestBody**
    * **responses**(REQUIRED)
    * callbacks
    * deprecated
    * **security**
    * **servers**
  * **servers**
  * **parameters**
    * **name**
    * **in**
    * description
    * **required**
    * deprecated
    * **allowEmptyValue**
* **component**: contains various schemas of different objects
* **security**: declaration of the different security mechanisms found in the api
* tags: list of tags
* externalDocs: Additional external documentation

Out of these fields openapi, info and paths are required. 

## Mandatory fields

List of mandatory fields that are currenly used or are planned to be used by OAF 

****Field****: openapi

****Object****: OpenAPI 

**Description**: Contains sematic version number of the OpenAPI specification version.

**Usability**: Mandatory. Parser needs to know if the api is in 2.x or 3.x version

---

**Field**: title

**Object**: info

**Description**: Title of the application

**Usability**: No direct fuzzing use. Currently used for logging purposes

---

**Field**: version

**Object**: info

**Description**: Version of the OpenAPI document. 

**Usability**: No direct fuzzing use. Can be used to name logs and other generated files

---

**Field**: paths

**Object**: OpenAPI

**Description**: Contains all the available enpoints and their operations

**Usability**: Mandatory. This gives you all the endpoints you want to fuzz and their intended responses

---

**Field**: responses

**Object**: paths/{operation}

**Description**: Describes all the possbile responses that are returned when executing this operation

**Usability**: Mandatory. Can be used to verify if the generated test case is valid and in fuzzing to idenify potentially wrong behaviour

---

Content of these mandatory fields should be enough to generate bare bone tests. You are however missing vital information, like the exact structure of the requests. Next we will go over fields that can be used to enhance the test creation

## Optional fields

**Field**: url

**Object**: Server

**Description**: Url to target host

**Usability**: Mandatory. Contains the url to the API

---

**Field**: variables

**Object**: Server

**Description**: If the url is in form www.api.com/{variable} it contains the variable 

**Usability**: Mandatory. Needed to reach the target

---

**Field**: Components

**Object**: OpenAPI

**Description**: Contains reusable objects for different aspects of the api. For example schemas and responses/parameters that are used multiple times 

**Usability**: Mandatory. Especially in larger APIs it is likely that you have to fetch a lot of information from components object

---

**Field**: security

**Object**: OpenAPI and operation

**Description**: Contains security mechanisms used by the API/operation. OpenAPI object contains the default mechanims and operation object has an alternative version if it happens to use something different

**Usability**: Mandatory. If the API uses any security mechanisms they has to be taken into account when creating tests

---

**Field**: servers

**Object**: path

**Description**: Contains alternative server array for this path. Replaces defauls server if present

**Usability**: Mandatory. Replaces the default server so is needed to reach this path

---

**Field**: parameters

**Object**: path and operation

**Description**: Contains parameters and their information used by this path/operation. Contains multiple fields but the most important are parameter name, parameters location(in url, cookies, etc) and wheater the parameter is mandatory. 

**Usability**: Mandatory. This object contains all the parameters and their locations in the request. Is vital to creating good test cases

---

**Field**: deprecated

**Object**: operation

**Description**: Indicates if the operation is deprecated

**Usability**: Potentially useful. You could make it possible to not utilize deprecated operations or make so that test genertion doesn't use the that much. 

---

**Field**: requestBody

**Object**: operation

**Description**: The request body applicable for this operation. The requestBody is only supported in HTTP methods where the HTTP 1.1 specification RFC7231 has explicitly defined semantics for request bodies. In other cases where the HTTP spec is vague, requestBody SHALL be ignored by consumers.

**Usability**: Mandatory. If present should be parsed and used in test case creation


---
**Field**: $ref

**Object**: Can be found in most of the objects

**Description**: Marks an external definition of the item. Item is defined in components section or in other file etc.

**Usability**: Not directly fuzzing related but has to be taken in to account when parsing the spec

---







