---
tags:
  - node/express
---


- firstly we wanna add data to our program 
- lets say we have a list of courses objects
```js
const courses = [
		{ id: 1, name: "course 1" },
		{ id: 2, name: "course 2" },
		{ id: 3, name: "course 3" },
];
```

### GET method
- we can use the GET method to get data from our program 
- so there is 2 cases 
- the first case is when we found the course we want to get -> we will search for the course by id and if we find it we will return it
- the second case is when we don't find the course we want to get -> we will return a 404 error , and a message to the user saying that the course was not found

let's go 
```js
const express = require("express");
const app = express();
const PORT = 3000;
const courses = [
		{ id: 1, name: "course 1" },
		{ id: 2, name: "course 2" },
		{ id: 3, name: "course 3" },
];
app.get("api/cources/:id", (req, res) => {
	// get the id from the request
	const id = parseInt(req.params.id);
	// search for the course by id
	const course = courses.find((course) => course.id === id);
	// check if the course was found
	if (!course) {
		return res.status(404).send("course not found");
	}
	// return the course
	res.send(course);
});
)
app.listen(PORT, () => {
	console.log(`server is running on port ${PORT}`);
});
```
- so in the code above we used the GET method to get the course by id
- we used the find method to search for the course by id
- we used the `parseInt` method to convert the id from a string to a number
- we used the status method to set the status code to 404
- we used the `req.params` to get the id from the request
- we used the `res.send` to send the response to the user

- so for this URL which searches for the course by id = 1 
```bash
http://localhost:3000/api/cources/1
```
- the response will be 
```json
{
	"id": 1,
	"name": "course 1"
}
```

- and for this URL which searches for the course by id = 5 
```bash
http://localhost:3000/api/cources/5
```
- the response will be 
```json
{
	"error": "course not found"
}
```

great success !!!

### POST method
- now we want to add a new course to our program
- we will use the POST method to add a new course
- so there is 2 cases
- the first case is when we add the course successfully -> we will return the course
- the second case is when we don't add the course successfully -> we will return a 400 error , and a message to the user saying that the course was not added

but there is 2 catches actually :
1.
- in order to add a new course we need to send the course data in the request body
- but by default express doesn't parse the request body
- so we need to use a middleware to parse the request body
- so we must add this line of code to our program
```js
app.use(express.json());
```
- do not stress about this line of code, we will explain this later 
- just think of it as a middleware that parses the request body for now 


2.
- the URL for adding a new course should be 
```bash
http://localhost:3000/api/cources
```
- yes itis the same as GET 
- but we validate that our POST is working using postman 
- again do not stress about it too much we will discuss it later 

- so lets go 
```js
const express = require("express");
const app = express();
const PORT = 3000;
app.use(express.json()); // middleware to parse the request body
const courses = [
		{ id: 1, name: "course 1" },
		{ id: 2, name: "course 2" },
		{ id: 3, name: "course 3" },
];
app.post("/api/cources", (req, res) => {
	const course = req.body;
	// check if the course is valid
	if (!course.name || course.name.length < 3) {
		return res.status(400).send("course name is required and should be at least 3 characters");
	}
	// add the course to the courses array
	courses.push(course);
	// return the course
	res.send(course);
});

app.listen(PORT, () => {
	console.log(`server is running on port ${PORT}`);
});
```
- so in the code above we used the POST method to add a new course
- we used the `req.body` to get the course from the request body
- in case the course is not valid
- we used the status method to set the status code to 400 [which means bad request] 
- we used the `push` method to add the course to the courses array
- we used the `res.send` to send the response to the user [so we know what the updated course is]

----
### flash cards 

- create a GET method to get all courses with a specified id, if the course is not found return a 404 error with a message saying that the course was not found.
here is the code 
```js
const express = require("express");
const app = express();
const PORT = 3000;
const courses = [
		{ id: 1, name: "course 1" },
		{ id: 2, name: "course 2" },
		{ id: 3, name: "course 3" },
];

// code here 

app.listen(PORT, () => {
	console.log(`server is running on port ${PORT}`);
});
```
??
```js
app.get("/api/cources/:id", (req, res) => {
	// get the id from the request
	const id = parseInt(req.params.id);
	// search for the course by id
	const course = courses.find((course) => course.id === id);
	// check if the course was found
	if (!course) {
		return res.status(404).send("course not found");
	}
	// return the course
	res.send(course);
});
```
.

- create a POST method to add a new course, if the course is not valid return a 400 error with a message saying that the course name is required and should be at least 3 characters.
here is the code 
```js
const express = require("express");
const app = express();
const PORT = 3000;
app.use(express.json()); // middleware to parse the request body
const courses = [
		{ id: 1, name: "course 1" },
		{ id: 2, name: "course 2" },
		{ id: 3, name: "course 3" },
];
// code here
app.listen(PORT, () => {
	console.log(`server is running on port ${PORT}`);
});
```
??
```js
const express = require("express");
const app = express();
const PORT = 3000;
app.use(express.json()); // middleware to parse the request body
const courses = [
		{ id: 1, name: "course 1" },
		{ id: 2, name: "course 2" },
		{ id: 3, name: "course 3" },
];


app.post("/api/cources", (req, res) => {
	const course = req.body;
	// check if the course is valid
	if (!course.name || course.name.length < 3) {
		return res.status(400).send("course name is required and should be at least 3 characters");
	}
	// add the course to the courses array
	courses.push(course);
	// return the course
	res.send(course);
});

app..listen(PORT, () => {
	console.log(`server is running on port ${PORT}`);
});
```
.

