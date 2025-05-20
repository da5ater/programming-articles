

- download postman and sign up and com here to follow with me

- remember in the last section we setup an post request but we could not validate it using a browser like we do with get requests 
- here our best friend postman comes in 
- postman is a tool that allows us to send requests to our server and see the responses
- which is exactly what we need huh !
- so let's open postman and create a new request

remember here is our app so far 
```js
const express = require('express') // Import express module
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

// we are testing this in postman
//-----------------------------------------
// Add a new course
app.post('/api/courses', (req, res) => {
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

- so lets open postman and create a new request
![[Pasted image 20250520164036.png]]
- select post request and enter the url of our server [in my case its http://localhost:3000/api/courses]
- make sure to select post request
- then click on the body tab
- select raw and json
- enter the following data
```json
{
		"name": "course4"
}
```
- then click send

here is our response :
![[Pasted image 20250520164139.png]]

great success !!

---
