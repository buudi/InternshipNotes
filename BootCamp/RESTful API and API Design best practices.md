sources:
- [RESTful API - TechTarget](https://www.techtarget.com/searchapparchitecture/definition/RESTful-API)
- [RESTful Web API Design - Microsoft Learn](https://learn.microsoft.com/en-us/azure/architecture/best-practices/api-design)
- [The Web API Checklist: designing, testing and releasing - Mathieu Fenniak](https://mathieu.fenniak.net/the-api-checklist/)

what's with the naming?
```c#
"RESTful Web API" == "REST Api" == "RESTful Web Services";
```

#### what's an API
An API is [code](https://www.techtarget.com/whatis/definition/code) that lets two software programs communicate with one another.

## What is a RESTful API
- A RESTful [API](https://www.techtarget.com/searchapparchitecture/definition/application-program-interface-API) is an architectural style for an application programming interface that uses [HTTP requests](https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/HTTP-methods) to access and use data.
- They're based on representational state transfer.

Representational state transfer is an architectural style that defines how a distributed system (like the Internet) should work

REST is commonly preferred than other technologies as it uses less Bandwidth but it also depends on the use case, most of the time RESTful API's are sufficient.

#### What makes an API RESTful? 
1. Resource and URIs: assign unique URIs to each resource
2. HTTP methods
3. Stateless : Each request is separate and unconnected, and no client information is stored between requests, so a request needs to contain all info needed for server to respond.
4. Cacheable data : help streamline interactions between client and server, (common responses can be cached for better performance)
5. Uniform Interface : information is transferred in a standard format (like JSON but not limited to).
6. Hypermedia [(HATEOAS)](allow clients to navigate the API dynamically. Include links or references within the API responses that guide clients to related resources or actions): allow clients to navigate the API dynamically. Include links or references within the API responses that guide clients to related resources or actions

> JSON stands for _JavaScript Object Notation_. JSON is a lightweight format for storing and transporting data, it is often used as the format when transferring information in a RESTful API.
#### Technologies other than RESTful APIs:
- GraphQL
- SOAP
- gRPC
- tRPC
- etc ...

#### HTTP methods: Commands corresponding to CRUD:
A RESTful API uses commands to obtain resources. 
example `GET /api/users`

| **HTTP verb** | **CRUD action**         |
| ------------- | ----------------------- |
| POST          | Create                  |
| GET           | Read                    |
| PUT           | Update (replace object) |
| PATCH         | Update (patch object)   |
| DELETE        | Delete                  |
 >The state of a resource at any given timestamp is called a **resource representation.**
 
![[Pasted image 20240807154242.png]]



