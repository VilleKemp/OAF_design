# Request

## Request contents

Destination: For example www.api.a/endpoint

Method: POST, GET etc

Content: 
  * Headers
  * Cookies
  * Query parameters
  * Body

## Locations in OpenAPI spec
---

**Component**: Destination

**Location**: Server object + path object + potentially parameter object in case where there are path parameters

---

**Component**: Method

**Location**: Operation object contained in path object

---

**Component**: Headers

**Location**: Parameter object and security object. Parameter object in cases where there are header parameters. Security object in cases where security scheme uses headers

---

**Component**: Cookies

**Location**: Parameter object and security object. Parameter object in cases where there are cookie parameters. Security object in cases where security scheme uses cookies

---

**Component**: Query parameters

**Location**: Parameter object and security object. Parameter object in cases where there are query parameters. Security object in cases where security scheme uses query

---

**Component**: Body

**Location**: requestBody object. For example if the API needs input as a json the structure of it is found in requestBody.

---