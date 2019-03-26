# OAF design

OAF intends to create test cases from OpenApi specification. 

## Components

OAF consist of multiple components. These are:
* Parser
* Request generator
* Test case generator
* Fuzz case generator

# Parser

Parser parses through the OpenApi specification. From that it generates an API object

## API object
Api object is a reduced version of the OpenApi 3.0.2 spec. It aims to keep a similar structure as the OpenApi spec but only containing useful information. API object contains multiple other objects and they are pictured below

### Structure

API: Object
  * Info object
    * openApiVersion: String
    * apiName: string
    * apiDescription: string
    * apiVersion: string
  * Path object
    * path: string
    * GET method object
      * operationID: string
      * parameters: array
        * Parameter object
          * name: string
          * location: string
          * required: boolean
          * value: -
          * options: array
            * option: string
      * requestBody object
        * type_ string
        * required: boolean
        * params: array
          * Parameter object
      * responses object WIP
        * code: string
        * content: string
      * security object WIP
        * type_: specific strings
        * name: 
        * location: specific string
        * scheme
        * flows
        * openidconnecturl
        * apikey
      * server array
        * server: string
    * POST method object
      * ...
    * PUT method object
      * ...
    * DELETE method object
      * ... 


