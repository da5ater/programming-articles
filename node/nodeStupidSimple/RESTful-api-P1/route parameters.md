---
tags:
  - node/express
---

## what is a route 
- A route is a path that the client can access on the server.
- It is a combination of an HTTP method (GET, POST, PUT, DELETE) and a URL path.
- For example, a GET request to the URL `/api/customers` is a route that retrieves a list of customers. [the url would be http://vidly.com/api/customers] ]
- A POST request to the URL `/api/customers` is a route that creates a new customer.
- A PUT request to the URL `/api/customers/:id` is a route that updates an existing customer with the specified ID.
- A DELETE request to the URL `/api/customers/:id` is a route that deletes an existing customer with the specified ID.

## what is a route parameter
- A route parameter is a variable part of the URL path that can change based on the request.
- It is defined by a colon (`:`) followed by the parameter name in the URL path.
- For example, in the URL `/api/customers/:id`, `:id` is a route parameter that represents the ID of the customer.
- When a client makes a request to this URL, the server can extract the value of the `id` parameter and use it to identify the specific customer being requested.
- Route parameters are useful for creating dynamic routes that can handle different values without needing to define separate routes for each possible value.


so this is a route that responds to a GET request to the root URL (`/`) and sends back a response with the text "Hello World!".
```js
app.get('/', (req, res) => { res.send('Hello World!'); });
```


> but what if we want to create a route that responds to whatever the user types in the URL? 

---
### in simple terms 
- we want to get dynamic information from the URL or endpoint 
- and based on it we want to do something 
- anything?? yes anything.

- so for a URL with a query parameter name `shaboom` we want to respond with `boooom`
```bash
http://localhost:3000/api/shaboom
```
- so our route would look like this 
```js
app.get('/api/:query', (req, res) => {
	if(req.params.query === 'shaboom') {
		res.send('boooom');
	} else {
		res.send('no boom');
	})
	});
```
- so now if we go to the URL 
```bash
http://localhost:3000/api/shaboom
```
- we will get the output 
```json
{
	"response": "boooom"
}
```
- and if we go to the URL 
```bash
http://localhost:3000/api/ahmed
```
- we will get the output 
```json
{
	"response": "no boom"
}
```

cool right !!! 

---

### lets get a specific single course 

- in order to get a single course we need to get the id of the course in the URL 
- so to get course with id 1 the URL would be 
```bash
http://localhost:3000/api/courses/1
```

- and our express route would look like this 
```js
app.get(`api/cources/1`, (req, res) => {
		res.send(courses[0]);
});
```
- but this is not a good idea because we want to get any course with any id 


so instead of hardcoding the id in the URL we can use a route parameter 
- so our route would look like this 
```js
app.get('/api/courses/:id', (req, res) => {
		res.send(courses[req.params.id]);
});
```
- so now we can get any course with any id

- so if we want to get the course with id 1 we can use this URL 
```bash
http://localhost:3000/api/courses/1
```
and the out put would be 
```json
{
	"id": 1,
	"name": "course 1"
}
```

- and if we want to get the course with id 2 we can use this URL 
```bash
http://localhost:3000/api/courses/2
```
and the out put would be 
```json
{
	"id": 2,
	"name": "course 2"
}
```

automatically the express will get the id from the URL and use it to get the course from the array. 

isn't that cool !!!

---

## multiple parameters 

- imagine we are building a blog 
- so we want to get a post within a specific date 
- so for an endpoint like this  
```bash
http://localhost:3000/api/posts/2023/10/1
```
- our route would look like this 
```js
app.get('/api/posts/:year/:month/:day', (req, res) => {
		res.send(req.params);
});
```
- and the output would be 
```json
{
	"year": "2023",
	"month": "10",
	"day": "1"
}
```
- and later you may use that object to get the post from the database 
for example 
```js
app.get('/api/posts/:year/:month/:day', (req, res) => {
		return res.send(`post with id ${req.params.id} and date ${req.params.year}/${req.params.month}/${req.params.day}`);
});
```

- and the output would be 
```json
{
	"post": "post with id 1 and date 2023/10/1"
}
```


#### remember what is this solving 
- this is solving the problem of hardcoding the id in the URL
- so for each date we would have to create a new route 
- but with this approach we can create a dynamic route that can handle any date 

---
## query string parameters 
- sometimes we need to pass additional information to the server
- for example we may want to filter the results based on a specific criteria [name or age]]
- so we can use query string parameters to pass this information 

here is a query string parameter add it to the last example  
```bash
http://localhost:3000/api/posts/2023/10/1?name=ahmed&age=20
```
- so in English this URL means :  give me the post with id 1 and date 2023/10/1 and the name is Ahmed and the age is 20
- so our route would look like this 
```js
app.get('/api/posts/:year/:month/:day', (req, res) => {
		res.send(req.query);
});
```

- and the output would be 
```json
{
	"name": "ahmed",
	"age": "20"
}
```

----
### here is our full code  so far 
```js
const express = require('express')

const app = express()

  

// this how you define a route in express

app.get('/', (req, res) => {

    res.send('Hello World!')

})

  
  

app.get('/api/courses', (req, res) => {

    res.send([

        { id: 1, name: 'course1' },

    ])

});

  

// this is how you define a route with a parameter in express

app.get('/api/courses/:id', (req, res) => {

    res.send({ id: req.params.id, name: 'course1' })

})

  
  

app.get(`/api/posts/:year/:month`, (req, res) => {

    res.send({

        year: req.params.year,

        month: req.params.month

    })

})

  

// query parameters

app.get('/api/posts', (req, res) => {

    res.send(req.query)

})

  

// PORT

const port = process.env.PORT || 3000

  

app.listen(port, () => {

    console.log(`Server is running on port ${port}`)

})
```
---
### flash cards 
- what is a route :: is a path that the client can access on the server via an HTTP method (GET, POST, PUT, DELETE) and a URL path. 
- what is a route parameter :: is a variable part of the URL path that can change based on the request.
- what is a query string parameter :: is a way to pass additional information to the server in the URL.

- create a route that responds to a GET request to the URL `/api/courses/1` and returns the course with the specified ID [any id]
??
```js
app.get('/api/courses/:id', (req, res) => {
    res.send(courses[req.params.id]);
});
```
.

- create a route that responds to a GET request to the URL `/api/posts/2023/10/1` and returns the post with the specified date [any date]
??
```js
app.get('/api/posts/:year/:month/:day', (req, res) => {
    res.send(req.params);
});
```
.
- create a route that responds to a GET request to the URL `/api/posts/2023/10/1?name=ahmed&age=20` and returns the query parameters it got 
??
```js
app.get('/api/posts/:year/:month/:day', (req, res) => {
    res.send( req.query);
});

```

