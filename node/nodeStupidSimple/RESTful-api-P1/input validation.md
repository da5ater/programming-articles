
- in the former routs we handled input validation in manually
- and that was not the best way to do it 
- better way is using `joi` library 

write this in the terminal to install the latest version of `joi` library
```bash
npm install joi
```

### understanding joi 
- `joi` is a powerful schema description language and data validator for JavaScript objects.
- it allows you to define a schema for your data and then validate that data against that schema.
- so instead of manually checking if the data is valid or not, you can use `joi` to do that for you.


```js
app.post('/api/courses', (req, res) => {
    const course = {
        id: courses.length + 1, // Generate new id
        name: req.body.name // Get name from request body
    }
    courses.push(course) // Add course to array
    res.send(courses) // Return updated courses
})
```
for our post request we need to validate the following fields:
- 'name' is required and should be a string , and not less than 3 characters 

and that's it 


> remember this is part of our application we are building from the beginning 


- so let's create a schema for our course object using `joi` library

```js
const Joi = require('joi') // Import Joi library
const schema = Joi.object({ // Define schema
		name: Joi.string().min(3).required() // Define name field
})
```
- here we are using `joi` to define a schema for our course object
- we are saying that the `name` field should be a string, have a minimum length of 3 characters, and is required.


#### schema object 
- the `schema` object is created using the `Joi.object()` method.
- this method takes an object as an argument, where each key represents a field in the object you want to validate.
- the value of each key is a `Joi` validation rule that defines the constraints for that field.


- we can use the `schema.validate()` method to validate the incoming data against the schema.
- which return an object with two properties:
- `error` and `value`.
- if the validation fails, the `error` property will contain the error message.
- if the validation passes, the `value` property will contain the validated data.

in our app use this to log the result of the validation to see what it looks like 
```js
const result = schema.validate(req.body) // Validate incoming data
console.log(result) // Log result
```
here is the output of the validation 
```js
{
	value: { name: 'course1' },
	error: undefined
}
```

- now here is the full route with validation
```js
const Joi = require('joi') // Import Joi library
const schema = Joi.object({ // Define schema
		name: Joi.string().min(3).required() // Define name field
	})
app.post('/api/courses', (req, res) => {
		const { error } = schema.validate(req.body) // Validate incoming data
		if (error) return res.status(400).send(error.details[0].message) // Return error if validation fails

		const course = {
				id: courses.length + 1, // Generate new id
				name: req.body.name // Get name from request body
		}
		courses.push(course) // Add course to array
		res.send(courses) // Return updated courses
		})
		```

- now in postman make a post request to the `/api/courses` endpoint with the following body
```json
{
		"name": "course1"
}
```

- and you should see the following response
```json
[
		{
				"id": 1,
				"name": "course1"
		}
]
```

- now try to send a request with an invalid name 
```json
{
		"name": "co"
}
```
- and you should see the following response
```json
{
		"message": "\"name\" length must be at least 3 characters long"
}
```


here is our app so far 
```js

const express = require('express') // Import express module

const joi = require('joi') // Import Joi for validation

const app = express() // Create express app instance

  
  

app.use(express.json()) // Parse incoming JSON requests

  

const courses = [ // In-memory array of course objects

    { id: 1, name: 'course1' },

    { id: 2, name: 'course2' },

    { id: 3, name: 'course3' },

]

  

// Root route returns greeting

app.get('/', (req, res) => {

    res.send('Hello World!')

})

  

// Return all courses

app.get('/api/courses', (req, res) => {

    res.send(courses)

});

  

// Return a course by id

app.get('/api/courses/:id', (req, res) => {

    const course = courses.find(c => c.id === parseInt(req.params.id)) // Find course by id

    if (!course) return res.status(404).send('Course not found') // 404 if not found

    res.send(course) // Send course

})

  
  

//-----------------------------------------

// Add a new course

app.post('/api/courses', (req, res) => {

    const schema = joi.object({ // Define validation schema

        name: joi.string().min(3).required() // Name must be a string, min length 3, required

    })

    const result = schema.validate(req.body) // Validate request body against schema

  

    if (result.error) return res.status(400).send(result.error.details[0].message) // 400 if invalid

  

    const course = {

        id: courses.length + 1, // Generate new id

        name: req.body.name // Get name from request body

    }

    courses.push(course) // Add course to array

    res.send(courses) // Return updated courses

})

  

    //------------------------------------------

  
  

    // Return year and month from route params

    app.get(`/api/posts/:year/:month`, (req, res) => {

        res.send({

            year: req.params.year,

            month: req.params.month

        })

    })

  

    // Return query parameters as response

    app.get('/api/posts', (req, res) => {

        res.send(req.query)

    })

  

    // Set port from env or default to 3000

    const port = process.env.PORT || 3000

  

    // Start server and log port

    app.listen(port, () => {

        console.log(`Server is running on port ${port}`)

    })
```

----
## flash cards 
- implement a joi validation for the following where name' is required and should be a string , and not less than 3 characters 
here is our starter code 
```js

const express = require('express') // Import express module

const joi = require('joi') // Import Joi for validation

const app = express() // Create express app instance

  
  

app.use(express.json()) // Parse incoming JSON requests

  

const courses = [ // In-memory array of course objects

    { id: 1, name: 'course1' },

    { id: 2, name: 'course2' },

    { id: 3, name: 'course3' },

]

app.post('/api/courses', (req, res) => {
  // validate here 
	const course = {
        id: courses.length + 1, // Generate new id
        name: req.body.name // Get name from request body
    }
    courses.push(course) // Add course to array
    res.send(courses) // Return updated courses
})

app.listen(3000, () => {
		console.log('Server is running on port 3000')
})

```
??
here is the solution 
```js
const express = require('express') // Import express module
const joi = require('joi') // Import Joi for validation
const app = express() // Create express app instance
app.use(express.json()) // Parse incoming JSON requests
const courses = [ // In-memory array of course objects
    { id: 1, name: 'course1' },
    { id: 2, name: 'course2' },
    { id: 3, name: 'course3' },
]
app.post('/api/courses', (req, res) => {
		const schema = joi.object({ // Define validation schema
		        name: joi.string().min(3).required() // Name must be a string, min length 3, required
		            })
const result = schema.validate(req.body) // Validate request body against schema
if (result.error) return res.status(400).send(result.error.details[0].message) // 400 if invalid
		const course = {
				id: courses.length + 1, // Generate new id
				name: req.body.name // Get name from request body
		}
		courses.push(course) // Add course to array
		res.send(courses) // Return updated courses
})
app.listen(3000, () => {
		console.log('Server is running on port 3000')
})
```

