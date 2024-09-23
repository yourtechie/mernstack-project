# Self Side Study

### 1. Types of Database Management Systems (DBMS)

**Types of DBMS:**
1. **Hierarchical DBMS**: Organizes data in a tree-like structure with parent-child relationships. Suitable for applications with a clear hierarchical relationship, like organizational structures.
2. **Network DBMS**: Uses a network structure to create relationships between entities. Suitable for complex relationships where entities can have multiple parent and child records.
3. **Relational DBMS (RDBMS)**: Stores data in tables with rows and columns. Suitable for applications requiring complex queries and transactions, like banking systems.
4. **NoSQL DBMS**: Designed for large volumes of unstructured or semi-structured data. Includes document databases, key-value stores, column-family stores, and graph databases. Suitable for big data applications, real-time web apps, and IoT.
5. **Object-Oriented DBMS (OODBMS)**: Stores data as objects, similar to object-oriented programming. Suitable for applications requiring complex data representations, like CAD/CAM systems⁶⁷.

**Difference between Relational DBMS and NoSQL:**
- **Relational DBMS (RDBMS)**: Uses structured tables and SQL for data manipulation. Ensures ACID (Atomicity, Consistency, Isolation, Durability) properties. Suitable for structured data and complex queries.
- **NoSQL DBMS**: Uses various data models (document, key-value, column-family, graph). Designed for scalability and flexibility. Suitable for unstructured data and applications requiring high performance and availability⁶⁷.

### 2. Web Application Frameworks

**Server-side (Backend) Frameworks:**
- **Django** (Python): High-level framework that encourages rapid development and clean, pragmatic design.
- **Ruby on Rails** (Ruby): Known for its simplicity and speed of development.
- **ASP.NET Core** (C#): Developed by Microsoft, used for building robust web applications.
- **Express.js** (Node.js): Minimalist framework for building web applications and APIs.

**Client-side (Frontend) Frameworks:**
- **React**: JavaScript library for building user interfaces, maintained by Facebook.
- **Angular**: Framework for building dynamic web apps, maintained by Google.
- **Vue.js**: Progressive framework for building user interfaces.
- **Svelte**: Modern framework that compiles components to highly efficient vanilla JavaScript¹¹¹²¹³.

### 3. Basic JavaScript Syntax

Here's a quick overview of some basic JavaScript syntax:

```javascript
// Variables
let name = "John";
const age = 30;

// Functions
function greet() {
    console.log("Hello, " + name);
}

// Loops
for (let i = 0; i < 5; i++) {
    console.log(i);
}

// Conditionals
if (age > 18) {
    console.log("Adult");
} else {
    console.log("Minor");
}
```

### 4. RESTful API in Web Development

**RESTful API**: REST (Representational State Transfer) is an architectural style for designing networked applications. It uses standard HTTP methods (GET, POST, PUT, DELETE) and is stateless, meaning each request from a client to server must contain all the information needed to understand and process the request¹²³.

**Uses in Web Development**:
- **Interoperability**: Allows different systems to communicate over the web.
- **Scalability**: Supports high-performing and reliable communication at scale.
- **Flexibility**: Can be used with various data formats like JSON, XML¹².

### 5. Cascading Style Sheets (CSS)

**CSS**: A stylesheet language used to describe the presentation of a document written in HTML or XML. It controls the layout of multiple web pages all at once.

**Basic Syntax**:
```css
/* CSS Syntax */
selector {
    property: value;
}

/* Example */
body {
    background-color: lightblue;
}

h1 {
    color: navy;
    margin-left: 20px;
}
```

**Properties**:
- **Color**: `color`, `background-color`
- **Text**: `font-size`, `font-family`, `text-align`
- **Box Model**: `margin`, `padding`, `border`
- **Positioning**: `position`, `top`, `bottom`, `left`, `right`¹⁶¹⁷¹⁸.

