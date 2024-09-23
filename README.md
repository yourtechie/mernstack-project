# MERN Stack

### Create a new EC2 Instance of t2.micro family with Ubuntu Server 24.04 LTS (HVM) image.
Check my previous project where I showed how to provision your EC2 Instance. [LEMPstack Server](https://github.com/yourtechie/lempstack-project)

**Hint #1**: When you create your EC2 Instances, you can add a Tag "Name" to it with a value that corresponds to a current project you are working on - it will be reflected in the name of the EC2 Instance.
   ![](images/1.png)

---

## Task

Deploy a simple To-Do application that creates To-Do lists.

---

### Step 1 - Backend Configuration
   ![](images/2.png)

1. **Update Ubuntu**
   ```bash
   sudo apt update
   ```

2. **Upgrade Ubuntu**
   ```bash
   sudo apt upgrade
   ```

3. **Install Node.js**
   Get the location of Node.js software from Ubuntu repositories:
   ```bash
   curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
   ```
   ![](images/3.png)

   Then, install Node.js on the server:
   ```bash
   sudo apt-get install -y nodejs
   ```
   ![](images/4.png)


4. **Verify Node.js installation**
   ```bash
   node -v
   npm -v
   ```
   ![](images/5.png)

---

### Application Code Setup

1. **Create a new directory for your To-Do project**:
   ```bash
   mkdir Todo
   ```
   Run `ls` command to verify that the `Todo` directory is created:
   ```bash
   ls
   ```

2. **Navigate to the new directory**:
   ```bash
   cd Todo
   ```


3. **Initialize your project with npm**:
   ```bash
   npm init
   ```

   Follow the prompts to create `package.json`.

4. **Install ExpressJS**:
   ```bash
   npm install express
   ```

5. **Create a file `index.js`**:
   ```bash
   touch index.js
   ```

6. **Install dotenv module**:
   ```bash
   npm install dotenv
   ```
   ![](images/6.png)


7. **Open `index.js` and insert the following code**:
   ```javascript
   const express = require('express');
   require('dotenv').config();
   const app = express();
   const port = process.env.PORT || 5000;

   app.use((req, res, next) => {
       res.header("Access-Control-Allow-Origin", "*");
       res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
       next();
   });

   app.use((req, res, next) => {
       res.send('Welcome to Express');
   });

   app.listen(port, () => {
       console.log(`Server running on port ${port}`);
   });
   ```
   ![](images/7.png)


8. **Start your server**:
   ```bash
   node index.js
   ```

9. **Open port 5000 in EC2 Security Groups** and access your server's public IP or DNS at:
   ![](images/8.png)

   ```
   http://<PublicIP-or-PublicDNS>:5000
   ```
   ![](images/9.png)

---

### Routes

1. **Create a new directory `routes`**:
   ```bash
   mkdir routes
   cd routes
   ```

2. **Create `api.js` file**:
   ```bash
   touch api.js
   vim api.js
   ```
   ![](images/10.png)

3. **Add the following code to `api.js`**:
   ```javascript
   const express = require('express');
   const router = express.Router();

   router.get('/todos', (req, res, next) => {});
   router.post('/todos', (req, res, next) => {});
   router.delete('/todos/:id', (req, res, next) => {});

   module.exports = router;
   ```
---

### Models

1. **Navigate to the `Todo` directory and install Mongoose**:
   ```bash
   npm install mongoose
   ```

2. **Create a new folder `models`**:
   ```bash
   mkdir models
   cd models
   ```

3. **Create `todo.js`**:
   ```bash
   touch todo.js
   vim todo.js
   ```
   ![](images/11.png)


4. **Add the following code to `todo.js`**:
   ```javascript
   const mongoose = require('mongoose');
   const Schema = mongoose.Schema;

   // Create schema for todo
   const TodoSchema = new Schema({
       action: {
           type: String,
           required: [true, 'The todo text field is required']
       }
   });

   // Create model for todo
   const Todo = mongoose.model('todo', TodoSchema);
   module.exports = Todo;
   ```
   ![](images/12.png)

---

### Update Routes with Models

1. **Update `api.js`**:
   ```bash
   vim api.js
   ```

2. **Replace the existing content with the following code**:
   ```javascript
   const express = require('express');
   const router = express.Router();
   const Todo = require('../models/todo');

   router.get('/todos', (req, res, next) => {
       Todo.find({}, 'action')
           .then(data => res.json(data))
           .catch(next);
   });

   router.post('/todos', (req, res, next) => {
       if (req.body.action) {
           Todo.create(req.body)
               .then(data => res.json(data))
               .catch(next);
       } else {
           res.json({ error: "The input field is empty" });
       }
   });

   router.delete('/todos/:id', (req, res, next) => {
       Todo.findOneAndDelete({ "_id": req.params.id })
           .then(data => res.json(data))
           .catch(next);
   });

   module.exports = router;
   ```

---

### Configure MongoDB Database
In todo
1. **Create `.env` file**:
   ```bash
   tnaouch .env
   vim .env
   ```
   ![](images/15.png)

   ![](images/13.png)


2. **Add your MongoDB connection string in `.env`**:
   ```env
   DB='mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'
   
   #Here's mine
   DB='mongodb+srv://brookey:12345@cluster0.y4dte.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0'
   ```
   ![](images/14.png)

3. **Update `index.js` with the following code**:
   ```javascript
   const express = require('express');
   const bodyParser = require('body-parser');
   const mongoose = require('mongoose');
   const routes = require('./routes/api');
   require('dotenv').config();

   const app = express();
   const port = process.env.PORT || 5000;

   mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
       .then(() => console.log(`Database connected successfully`))
       .catch(err => console.log(err));

   mongoose.Promise = global.Promise;

   app.use(bodyParser.json());
   app.use('/api', routes);

   app.listen(port, () => {
       console.log(`Server running on port ${port}`);
   });
   ```

4. **Start your server**:
   ```bash
   node index.js
   ```
   ![](images/16.png)

---

### Step 2 - Frontend Creation

1. **Create React App**:
   ```bash
   npm create vite@latest

   cd client

   npm install
   ```
   ![](images/17.png)

2. **Install dependencies**:
   ```bash
   npm install concurrently --save-dev
   npm install nodemon --save-dev
   ```
   ![](images/18.png)

3. **Update `package.json`**:
   ```json
   "scripts": {
       "start": "node index.js",
       "start-watch": "nodemon index.js",
       "dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
   },
   ```

4. **Configure proxy in `client/package.json`**:
   ```bash
   cd client

   vi package.json
   ```
   ![](images/19.png)

   ```json
   "proxy": "http://localhost:5000"
   ```

5. **Run your app**:
   ```bash
   npm run dev
   ```

Make sure to open TCP port 3000 in EC2 Security Groups to access the app from the internet.
   ![](images/20.png)


Creating your React Components
One of the advantages of react is that it makes use of components, which are reusable and also makes code modular. For our Todo app, there will be two stateful components and one stateless component. From your Todo directory run
```bash
cd client
```

move to the src directory
```
cd src
```

Inside your src folder create another folder called components
mkdir components


Move into the components directory with cd components, Inside 'components' directory create three
files Input.js, ListTodo.js and Todo.js.

```bash
    touch Input.js ListTodo.js Todo.js
    #Open Input.js file
    vi Input.js
```
Copy and paste the following

```bash
    import React, { Component } from 'react';
    import axios from 'axios';
    class Input extends Component {
    state = {
    action: ""
    }
    addTodo = () => {
    const task = {action: this.state.action}
    if(task.action && task.action.length > 0){
    axios.post('/api/todos', task)
    .then(res => {
    if(res.data){
    this.props.getTodos();
    this.setState({action: ""})
    }
    })
    .catch(err => console.log(err))
    }else {
    console.log('input field required')
    }
    }
    handleChange = (e) => {
    this.setState({
    action: e.target.value
    })
    }
    render() {
    let { action } = this.state;
    return (
    <div>
    <input type="text" onChange={this.handleChange} value={action} />
    <button onClick={this.addTodo}>add todo</button>
    </div>
    )
    }
    }
    export default Input
```
   ![](images/21.png)

To make use of Axios, which is a Promise based HTTP client for the browser
and node.js, you need to cd into your client from your terminal and run
yarn add axios or npm install axios.
Move to the src folder
cd ..
Move to clients folder
cd ..
Install Axios
$ npm install axios
   ![](images/22.png)


Go to 'components' directory
cd src/components
After that open your ListTodo.js
vi ListTodo.js
in the ListTodo.js copy and paste the following code
```bash
    import React from 'react';
    const ListTodo = ({ todos, deleteTodo }) => {
    return (
    <ul>
    {
    todos &&
    todos.length > 0 ?
    (
    todos.map(todo => {
    return (
    <li key={todo._id} onClick={() => deleteTodo(todo._id)}>{todo.action}
    </li>
    )
    })
    )
    :
    (
    <li>No todo(s) left</li>
    )
    }
    </ul>
    )
    }
    export default ListTodo
```

Then in your Todo.js file you write the following code

```bash
    import React, {Component} from 'react';
    import axios from 'axios';
    import Input from './Input';
    import ListTodo from './ListTodo';
    class Todo extends Component {
    state = {
    todos: []
    }
    componentDidMount(){
    this.getTodos();
    }
    getTodos = () => {
    axios.get('/api/todos')
    .then(res => {
    if(res.data){
    this.setState({
    todos: res.data
    })
    }
    })
    .catch(err => console.log(err))
    }
    deleteTodo = (id) => {
    axios.delete(`/api/todos/${id}`)
    .then(res => {
    if(res.data){
    this.getTodos()
    }
    })
    .catch(err => console.log(err))
    }
    render() {
    let { todos } = this.state;
    return(
    <div>
    <h1>My Todo(s)</h1>


    <Input getTodos={this.getTodos}/>
    <ListTodo todos={todos} deleteTodo={this.deleteTodo}/>
    </div>
    )
    }
    }
    export default Todo;
```

We need to make little adjustment to our react code. Delete the logo and
adjust our App.js to look like this.
Move to the src folder

```bash
    cd ..
```

Make sure that you are in the src folder and run

```bash
    vi App.js
```
Copy and paste the code below into it
```bash
    import React from 'react';
    import Todo from './components/Todo';
    import './App.css';
    const App = () => {
    return (
    <div className="App">
    <Todo />
    </div>
    );
    }
    export default App;
```

After pasting, exit the editor.
In the src directory open the App.css
vi App.css
Then paste the following code into App.css:
```css
    .App {
    text-align: center;
    font-size: calc(10px + 2vmin);
    width: 60%;
    margin-left: auto;
    margin-right: auto;
    }
    input {
    height: 40px;
    width: 50%;
    border: none;
    border-bottom: 2px #101113 solid;
    background: none;
    font-size: 1.5rem;
    color: #787a80;
    }
    input:focus {
    outline: none;
    }
    button {
    width: 25%;
    height: 45px;
    border: none;
    margin-left: 10px;
    font-size: 25px;
    background: #101113;
    border-radius: 5px;
    color: #787a80;
    cursor: pointer;
    }
    button:focus {


    outline: none;
    }
    ul {
    list-style: none;
    text-align: left;
    padding: 15px;
    background: #171a1f;
    border-radius: 5px;
    }
    li {
    padding: 15px;
    font-size: 1.5rem;
    margin-bottom: 15px;
    background: #282c34;
    border-radius: 5px;
    overflow-wrap: break-word;
    cursor: pointer;
    }
    @media only screen and (min-width: 300px) {
    .App {
    width: 80%;
    }
    input {
    width: 100%
    }
    button {
    width: 100%;
    margin-top: 15px;
    margin-left: 0;
    }
    }
    @media only screen and (min-width: 640px) {
    .App {
    width: 60%;
    }
    input {
    width: 50%;
    }
    button {
    width: 30%;
    margin-left: 10px;
    margin-top: 0;
    }
    }
```
Exit
   ![](images/23.png)

In the src directory open the index.css
vim index.css
Copy and paste the code below:
```css
    body {
    margin: 0;
    padding: 0;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto",
    "Oxygen",
    "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue",
    sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    box-sizing: border-box;
    background-color: #282c34;
    color: #787a80;
    }
    code {
    font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New",
    monospace;
    }
```
   ![](images/24.png)

Go to the Todo directory

    ```
    cd ../..
    ```
When you are in the Todo directory run:

    ```bash
    npm run dev
    ```
   ![](images/25.png)

   Congratulations we have created a simple To-Do App and deployed it to MERN stack. We built a frontend application using React.js that communicates with a backend application written using Expressjs. You also created a Mongodb backend for storing tasks in a database.
   
   ---



