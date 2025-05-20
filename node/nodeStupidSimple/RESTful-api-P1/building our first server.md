---
tags:
  - node/express
---

- let's initialize express 
```js
const express = require('express');
const app = express();
```

- let's create a route that responds to a GET request to the root URL `/` 
```js 
app.get('/', (req, res) => {
		res.send('Hello World!');
});
```
- let's start the server on port 3000 
```js
const PORT = 3000;
app.listen(PORT, () => {
		console.log(`Server is running on http://localhost:${PORT}`);
});
```

here is the complete code:
```js
const express = require('express');
const app = express();
const PORT = 3000;
app.get('/', (req, res) => {
		res.send('Hello World!');
});
app.listen(PORT, () => {
		console.log(`Server is running on http://localhost:${PORT}`);
});
```




----
### environment variables 
- we have hardcoded the port number in the code 
- in a development environment we can use 3000 but in production we can use any port 
- the way to fix this is to use environment variables 
- an environment variable is a variable that is set outside the code 

to add an environment variable to our code example :
```js
const port = process.env.PORT || 3000; // if the environment variable is not set then use 3000
```

and now we set it outside the code by typing this in the terminal [spacing is important] 

```bash
set PORT=5000 && node app.js

```

so the full code would be 
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

  
  

// PORT

const port = process.env.PORT || 3000

  

app.listen(port, () => {

    console.log(`Server is running on port ${port}`)

})
```



---
## flashcards 

- write the exact replica of the following code but instead of http use express 
```js
const http = require('http');

const server = http.createServer((req, res) => {
    if (req.url === '/') {
        res.write('Hello World');
        res.end();
    }

    if (req.url === '/courses') {
        res.write(JSON.stringify([1, 2, 3])); // an array of object is returned 
        res.end();
    }
}); // this server is an event emitter


server.on('connection', (socket) => {  // listener 
    console.log(`New connection on ${socket}`);
});



server.listen(3000); // start listening for connections

console.log('Listening on port 3000...'); // we are listening for connections , on port 3000 
```
??
```js
const express = require('express');
const app = express();
const PORT = 3000;
app.get('/', (req, res) => {
		res.send('Hello World!');
});
app.get('/courses', (req, res) => {
		res.send(JSON.stringify([1, 2, 3]));
});
app.listen(PORT, () => {
		console.log(`Server is running on http://localhost:${PORT}`);
});
```
.

