#Sample Rest Client:
ClientConfig config = new ClientConfig();
Client client = ClientBuilder.newClient(config);
WebTarget service = client.target(getBaseURI());

// create one todo
Todo todo = new Todo("3", "Blabla");
Response response = service.path("rest").path("todos").path(todo.getId()).request(MediaType.APPLICATION_XML).put(Entity.entity(todo,MediaType.APPLICATION_XML),Response.class);
// Return code should be 201 == created resource
System.out.println(response.getStatus());

#Rest Service life cycle
If resource class is not annotated with Singleton it will create new object for each request and it will be threadsafe, 
So it's request scoped


