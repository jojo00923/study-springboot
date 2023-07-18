A REST API (Representational State Transfer Application Programming Interface) is a set of rules and conventions that allow software systems to communicate with each other over the internet. It is an architectural style for designing networked applications.

In a RESTful API, resources are identified by URLs (Uniform Resource Locators), and the API provides a set of operations (such as GET, POST, PUT, DELETE) to manipulate these resources. These operations are typically performed using the HTTP protocol.

Here are some key principles and concepts of a REST API:

Resources: In a REST API, everything is considered a resource, such as users, products, or orders. Each resource is uniquely identified by a URL.

HTTP Verbs: RESTful APIs use HTTP verbs to indicate the type of operation to be performed on a resource. The most commonly used verbs are:

GET: Retrieves a representation of a resource.
POST: Creates a new resource.
PUT: Updates an existing resource.
DELETE: Removes a resource.
Request and Response: A client sends a request to the server, specifying the desired resource and the operation to perform. The server processes the request and sends back an appropriate response, typically in JSON or XML format.

Stateless: REST APIs are stateless, meaning each request from a client must contain all the necessary information for the server to understand and process the request. The server does not store any information about the client's previous requests.

Uniform Interface: REST APIs follow a uniform interface, which means they have a consistent structure and standardized ways of accessing and manipulating resources. This simplifies the development process and makes the API more predictable.

Authentication and Authorization: REST APIs often use authentication mechanisms like API keys, tokens, or OAuth to verify the identity of clients. Authorization mechanisms control what actions a client can perform on specific resources.

Developers can create their own REST APIs using various programming languages and frameworks. When designing a REST API, it is essential to carefully define the resources, endpoints, and the data format to ensure a well-organized and intuitive API for clients to consume.

```java
@RestController
class EmployeeController {

  private final EmployeeRepository repository;

  EmployeeController(EmployeeRepository repository) {
    this.repository = repository;
  }


  // Aggregate root
  // tag::get-aggregate-root[]
  @GetMapping("/employees")
  List<Employee> all() {
    return repository.findAll();
  }
  // end::get-aggregate-root[]

  @PostMapping("/employees")
  Employee newEmployee(@RequestBody Employee newEmployee) {
    return repository.save(newEmployee);
  }

  // Single item
  
  @GetMapping("/employees/{id}")
  Employee one(@PathVariable Long id) {
    
    return repository.findById(id)
      .orElseThrow(() -> new EmployeeNotFoundException(id));
  }

  @PutMapping("/employees/{id}")
  Employee replaceEmployee(@RequestBody Employee newEmployee, @PathVariable Long id) {
    
    return repository.findById(id)
      .map(employee -> {
        employee.setName(newEmployee.getName());
        employee.setRole(newEmployee.getRole());
        return repository.save(employee);
      })
      .orElseGet(() -> {
        newEmployee.setId(id);
        return repository.save(newEmployee);
      });
  }

  @DeleteMapping("/employees/{id}")
  void deleteEmployee(@PathVariable Long id) {
    repository.deleteById(id);
  }
}
```

OAuth2 관련 자료 -> 시큐리티 학습때 사용 예정
https://knoc-story.tistory.com/80

