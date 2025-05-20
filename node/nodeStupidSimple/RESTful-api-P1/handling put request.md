
- put request is used to update a resource 

so we want to update our course with id of number 1 to have a name of "new course"

ok we will write the whole solution and explain what we have done in parts 
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

//--------------------------PUT---------------------------------
app.put('/api/courses/:id', (req, res) => {
    const schema = joi.object({
        name: joi.string().min(3).required() // Validate name: string, min 3 chars, required
    })
    const {error} = schema.validate(req.body) // Validate request body

    if (error) return res.status(400).send(result.error.details[0].message) // 400 if invalid

    const course = courses.find(c => c.id === parseInt(req.params.id)) // Find course by id
    if (!course) return res.status(404).send('Course not found') // 404 if not found

    course.name = req.body.name // Update course name
    res.send(courses) // Return updated courses array
})
//---------------------------------------------------------------

// Start server and log port
app.listen(port, () => {
    console.log(`Server is running on port ${port}`)
})

```

### Explanation of the code:

- inside our put request our logic is in three parts 
- first we validate the request body using joi
- then we check if the course exists
- finally we update the course name and return the updated courses array

#### validation 
- we have already talked about validation 
- but here is a new idea, instead of doing `result.error` we can use `error` directly by leveraging the destructuring feature of js .

> and the rest of the code is self explanatory 

### Testing the code
- we can use postman to test our code
- if we make a put request to `http://localhost:3000/api/courses/1` with the body of 
```json
{
		"name": "new course"
}
```
- we should get a response of 
```json
[
	{
		"id": 1,
		"name": "new course"
	},
	{
		"id": 2,
		"name": "course2"
	},
	{
		"id": 3,
		"name": "course3"
	}
]
```

----
