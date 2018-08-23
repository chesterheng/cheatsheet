#### Principles of Designing RESTful APIs
* Use nouns: /products
* Use Plurals: /products
* Use parameters: /products?name='ABC'
* Versioning: /v1/products
* Use Pagination: /products?limit=25&offset=50
* Supported Formats: JSON
* Use Proper Error Messages: 
```
{
  "error": {
    "message": "(#803) Some of the aliases you requested do not exist: products",
    "type": "OAuthException",
    "code": 803,
    "fbtrace_id": "FOXX2AhLh80"
  }
}
```

##### Use of right HTTP methods
* GET — To get a resource or collection of resources.
* POST — To create a resource or collection of resources.
* PUT/PATCH — To update the existing resource or collection of resources.
* DELETE — To delete the existing resource or the collection of resources.

##### Use proper HTTP codes
* 200 OK — This is most commonly used HTTP code to show that the operation performed is successful.
* 201 CREATED — This can be used when you use POST method to create a new resource.
* 202 ACCEPTED — This can be used to acknowledge the request sent to the server.
* 400 BAD REQUEST — This can be used when client side input validation fails.
* 401 UNAUTHORIZED / 403 FORBIDDEN— This can be used if the user or the system is not authorised to perform certain operation.
* 404 NOT FOUND— This can be used if you are looking for certain resource and it is not available in the system.
* 500 INTERNAL SERVER ERROR — This should never be thrown explicitly but might occur if the system fails.
* 502 BAD GATEWAY — This can be used if server received an invalid response from the upstream server.

##### Reference
* https://hackernoon.com/restful-api-design-step-by-step-guide-2f2c9f9fcdbf
