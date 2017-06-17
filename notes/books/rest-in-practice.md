# Key Concepts

 * Scalability by decoupling client from server.
 * Caching is a way of decoupling and helps on scalability.
 * The more decoupled and scalable, the harder it gets to be consistent (applies to databases too :-).
 * A REST API is explorable (HATEOAS), the contract is not documented on detail.
 * URI templating is not REST, you only know entry points.
 * Auto documentation by using HTTP idioms, uniform interfaces (eg. CRUD).
 * What needs to be documented should be done in-band with the service, not out-of-band on another place (rel links with documentation).
 * On a distributed system hiding necessary complexity is a bad idea.
 * Do not hide from the application that the system is distributed (JMI/RPC opposes this idea).


# How to document ?

Well, REST does not fit a IDL, it is more loose. But you have the need to build a contract.
The contract is a set of protocols, where each protocol is a set of operations to achieve a business goal.
Each protocol consists on a set of URIs, HTTP idioms (like verbs and status codes) and media types (different representations of information).
