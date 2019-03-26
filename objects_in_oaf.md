# OAF design

OAF intends to create fuzz test cases from OpenApi specification. 

## Components

OAF consist of multiple components. These are:
* Parser
* Request generator
* Fuzz case generator

# Parser

Parser parses through the OpenApi specification. From that it generates an API object

## API object
Api object is a reduced version of the OpenApi 3.0.2 spec. It aims to keep a similar structure as the OpenApi spec but only containing useful information. API object contains multiple other objects and they are pictured below

### Structure

**API**: Object
  * **Info** object
    * openApiVersion: String
    * apiName: string
    * apiDescription: string
    * apiVersion: string
  * **Path** object
    * path: string
    * **GET** method object
      * operationID: string
      * parameters: array
        * **Parameter** object
          * name: string
          * location: string
          * required: boolean
          * value: -
          * options: array
            * option: string
      * **requestBody** object
        * type_: string
        * required: boolean
        * params: array
          * **Parameter** object
      * **responses** object WIP
        * code: string
        * content: string
      * **security** object WIP
        * type_: specific strings
        * name: 
        * location: specific string
        * scheme
        * flows
        * openidconnecturl
        * apikey
      * server: array
        * server: string
    * **POST** method object
      * ...
    * **PUT** method object
      * ...
    * **DELETE** method object
      * ... 

## Request generator
Request generator generates succesful requests and request combinations. This is done by taking relevant information from the API object and putting it in to a Request object. HTTP request is then generated based on information found in this object and send to the api with randomized parameter values until the api returns 200 or other valid response code or until request limit runs out.

## Request generation logic

### Generating a single request

1. Generate a request from all or just required parameters and fields. 
2. Randomize variables. 
3. If API returns a desired response code, save the request. If not randomize the variables and try again. During this randomization we also use values from previous successful requests. This is done because it is possible that one request creates a resource and this resource is needed for other request to work. For example if you have a API with users, you can't get user information without first creating a user. Do this X more times. 
4. If there is no success move to next variable. 
5. Continue this until all endpoints and methods have a valid request or the generator gets stuck. 

### Generating request chains

Request chain generation logic is not yet decided. Below few thoughts

* Brute force
  * Basically same idea as generating single request but this time we do multiple consecutive requests.
  * This should work in smaller scale APIs but likely isn't feasible in larger ones.
  * Might not create "good" chains. Brute force doesn't take in to account if there is any connection between the requests. It is likely that we want chains like "Create resource, Get resource, Modify resource, get resource, delete resource" and some random requests in between. 

* "Smart" chaining
  * In essence brute force with guided logic. Below some ideas 
    * See if parameters with the same name are used in multiple endpoints/methods. Start by trying to combine these.
    * Try to find connections between endpoints and parameters. For example if the API has an endpoint called User, with POST, GET, DELETE and PUT methods, and there is a parameter named user used in other endpoints it is possible that this endpoint creates this type of resources. Take this in to account when generating chains.



## Request object

Request: Object
  * url: object
    * base: string (server url)
    * endpoint: string (path url)
  * parameters: array (contains Parameter objects relevant to this endpoint)
  * method: string (GET, POST, etc)
  * content: RequestBody object. 
  * security: Security object 

## Fuzz case generator

Fuzz case generator takes the requests and request combinations that the request generator creates and changes the parameter values to fuzz variables. Plan is to generate them with some of the following:
 
 * Defensics. 
    * Need to study the SDK to see what I can do with it. WIP

 * Radamsa 
    * Use radamsa to generate a large set of fuzz variables based on TBD corpus.
    * Easy to do with command line commands
    * Basically read file and insert varaible to parameter.

 * Existing corpus of "naughty words"
    * As long as a good corpus has been found this should be the most trivial way to do fuzzing. Should work like radamsa variant but no need to do any command line stuff. 