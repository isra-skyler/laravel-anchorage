
# A Guide to Creating Hypermedia-Driven APIs with Node.js
As a backend developer focused on crafting maintainable APIs, I regularly challenge myself to experiment with new techniques. My latest side project at [Hybrid Web Agency](https://hybridwebagency.com/) involved building a hypermedia-driven Node API from scratch.

I aimed to implement HATEOAS thoroughly by following the HAL and JSON:API specifications through testing. This would provide hands-on experience with these emerging standards.

Over half a year, I developed a full-fledged order system using hypermedia principles. Evaluating responses' link representations between related resources allowed deep comparison of the two approaches. This uncovered nuanced differences between them.

In this post, I discuss insights from my research and implementation. Code examples demonstrate each specification's resource structuring.

Sharing learnings from this endeavor aims to benefit fellow API designers exploring RESTful techniques. While my work at Hybrid Web Agency focuses on complex PHP solutions, expanding standards knowledge benefits both my career andclients.








### What is HATEOAS?

HATEOAS (Hypermedia as the Engine of Application State) defines a RESTful architectural approach centering on hypermedia controls. It establishes that clients interact programmatically by traversing linked resources within API responses, rather than depending on out-of-band knowledge of endpoints or relationships.

By including hyperlinks, HATEOAS-designed APIs provide the instructions needed for clients to autonomously navigate available interactions. For example, retrieving an order may disclose links to associated invoices, products, or status history. Through these relationships expressed as hypermedia, services Guide clients to valid next steps.

Crucially, the API interface can evolve independently of existing clients through new resource additions or hypermedia enhancements. Past users will seamlessly follow prompts to discover updated or formerly hidden functionality.

Adopting this hypermedia-driven design divorces clients from codified dependencies on specific implementation details. Services following HATEOAS principles cultivate inherent explorability, resilience to structural changes, and independence from static expectations.

Overall, HATEOAS establishes a RESTful paradigm wherein hypertext linkages steer engaged clients through interactions rather than requiring foreknowledge of a fixed structure or action schema. This confers APIs innate discoverability, flexibility and freedom to progress their domain models over time through the relationships conveyed with each response.







### Advantages of the HATEOAS Approach

By centering on hypermedia as the representation of application state, REST APIs that follow HATEOAS principles realize several key strengths:

**Self-Documenting Dialogs** - Each response cryptically outlines the valid transitions for the given context through embedded links, removing the need for out-of-band documentation.

**Organic Extensibility** - The service can unveil new capabilities and refactor internally over time through hyperlink augmentations, independently of clients.

**Resiliency to Change** - Modifying the domain model through structural changes becomes transparent to all consumers simply by updating relational prompts.

**Post-Deployment Discovery** - Hypermedia responses facilitate unveiling expanded functionality after launch through an inside-out growth methodology. 

**Exploratory Interfaces** - Clients can uncover overlooked interactions by dynamically traversing embedded affinity relationships within responses. 

**Reactive Traversal** - HATEOAS grants clients the freedom to prioritize navigational paths tailored to each unique operational situation.

Fundamentally, focusing on hypermedia as the driver produces self-educating, long-lasting and adaptable REST APIs able to flexibly evolve and guide their own comprehension.






## Evaluating Hypermedia Options 

When adopting HATEOAS in a Node API, two common approaches for encoding hypermedia links are the Hypertext Application Language (HAL) and JSON:API specifications.

### HAL

The HAL format provides a streamlined representation, using a top-level "_links" property to associate resources via URLs.

   ````json
   {
     "_links": {
       "self": {"href": "/orders/1"},
       "items": {"href": "/orders/1/items"}  
     }
   }
   ````
   
It keeps implementation simple yet satisfies basic hypermedia needs.

### JSON:API 

JSON:API defines a robust standard, explicitly connecting resources through a "relationships" object holding related IDs and links.

   ````
   "relationships": {
     "items": {  
       "links": {
         "related": "/orders/1/relationships/items"
       },
       "data": [...ids]
     }
   }
   ````

This richer format supports advanced use cases but with increased complexity.

### Evaluation Criteria

For lightweight applications, HAL's simplicity suffices. JSON:API acts as a full specification framework well-suited to robust, standardized APIs. Consider an API's hypermedia requirements, design goals and workload to determine the best-fitting approach. Both uphold HATEOAS principles.


## Building a HAL API in Node.js

Developing a RESTful API that follows the Hypertext Application Language (HAL) specification using Express.

### Installing Dependencies

Adding the `express-hal-builder` module to generate HAL compliant representations.

```
npm install express-hal-builder
```

### Defining Resource Models

Creating schema objects representing Order and OrderItem resources.

```js
const Order = {...};
const OrderItem = {...};  
```

### Generating Hypermedia Links 

Rendering resources and embedding associated links using the builder library:

```js
app.get('/orders/:id', (req, res) => {

  const order = //...
  const builder = new HalBuilder();  

  builder.representEntity(order)
    .childLink(order.items, '/orders/'+id+'/items');

  res.json(builder.get());

});
```

This establishes the fundamental framework for exposing resources and their relationships through HAL representations.


## Exploring a HAL-Based API

Making initial GET requests to uncover the starting endpoints and relationship graph:

- Parse responses to map resource and link types  
- Extract URL values used to navigate between connected resources
- Gradually expose the domain model by traversing embedded links

## Developing a Linked Data REST API according to JSON:API

This guide demonstrates how to build a backend service exposing relationships through the JSON:API standard using Node and Express.

### Importing Necessary Libraries

The `jsonapi-serializer` module supports generating standardized JSON:API responses.

### Modeling Core Resource Entities 

Schemas define the structure of resources like Orders and LineItems.

### Generating JSON:API-Compliant Responses

Responses connect related data by:

1. Representing a parent resource 

2. Embedding references to associated child resources

```js
app.get('/orders/:id', async (req, res) => {

  const order = await Order.findById(req.params.id);

  res.json(order.serialize({ 
    include: 'lineItems'
  }));

});
```

This provides the foundation for developing linked REST APIs according to the JSON:API specification through standardized resource and relationship representations.

# Conclusion
In summary, utilizing established API specifications provides structure, discoverability and future-proofs applications. Adhering to standards helps ensure intuitive and navigable services for users.

Building specification-driven, interconnected backends can pose complex challenges requiring diverse skills. Partnering with an experienced Node.js firm can expedite this process.

As a full-service Node.js company located in Anchorage, we offer [Node.js Development Services in Anchorage](https://hybridwebagency.com/anchorage-ak/custom-laravel-development-services/) to help build robust, specification-aligned APIs. Our senior engineers have extensive knowledge implementing HAL, JSON:API and other norms to represent relations. Whether for proofs-of-concept, scaling services or managing projects, we aim to deliver production-ready REST endpoints on time and on budget.

By capitalizing on our Node.js and framework expertise, in addition to best practices for architectural design, your team can prioritize core work while we ensure technical execution meets quality and performance standards. Contact us to discuss how our Anchorage-based services could benefit your project goals.
## References

- https://www.halspec.org/
The official specification website for HAL (Hypertext Application Language), detailing the standard.

- https://jsonapi.org/
The official website for the JSON:API specification, including documentation on compliant API design. 

- https://expressjs.com/en/advanced/best-practice-performance.html
Best practices guide from the Express.js website covering performance, security, and standard compliance for Node.js APIs.  

- https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design
Documentation from Microsoft on API design best practices, including modeling relationships and using standards.

- https://swagger.io/resources/building-blocks/hypermedia/
Swagger overview of hypermedia APIs and industry standards like HAL and JSON:API for enabling discoverability.
