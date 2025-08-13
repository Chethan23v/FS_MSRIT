EXPRESS
**1-User-Management.js**
import express from 'express';

const app = express()
const port = 3000

app.use(express.json())
let users = [
    {id:1,name:"jude",email:"jude@hotmail.com"},
    {id:2,name:"carlo",email:"carlo@gmail.com"}
]

app.get('/',(req,res) => {
    res.send(users);
})

app.post('/users', (req,res)=>{
    const {name, email} = req.body
    const newUser = {
        id:users.length+1,
        name,
        email
    }
    users.push(newUser);
    res.status(201).send(`Users added: ${JSON.stringify(newUser)}`)
})

app.put('/users/:id',(req,res)=>{
    const userID = parseInt(req.params.id);
    const {name, email} = req.body
    const user = users.find(u=> u.id===userID);

    if(!user){
        return res.status(404).send("User not found")
    }

    user.name = name || user.name;
    user.email = email || user.email;
    
    res.send(`User updated: ${JSON.stringify(user)}`)
})

app.delete('/users/:id',(req,res)=>{
    const userID = parseInt(req.params.id);
    const index = users.findIndex(u=>u.id === userID)

    if(index === -1){
        return res.status(404).send("User not found")
    }

    const deleteUser = users.splice(index,1)
    res.send(`User deleted: ${JSON.stringify(deleteUser[0])}`)
})


app.listen(port,()=>{
    console.log(`Port running on http://localhost:${port}`);
})

------------------------------------------------***---------------------------------------------------
2-Product-Catalog.js
import express from 'express';

const app = express();
const port = 3000;

app.use(express.json());

let products = [
    { id: 1, name: 'watch', price: 2000, stock "available" },
    { id: 2, name: 'trimmer', price: 1500, stock: "unavailable" }
];

// GET all products
app.get('/products', (req, res) => {
    res.json(products);
});

// POST new product
app.post('/products', (req, res) => {
    const { name, price, stock } = req.body;
    const newProduct = {
        id: products.length + 1,
        name,
        price,
        stock
    };
    products.push(newProduct);
    res.status(201).json({ message: 'Product added', product: newProduct });
});

// PUT update product by ID
app.put('/products/:id', (req, res) => {
    const productid = parseInt(req.params.id);
    const { name, price, stock } = req.body;
    const product = products.find(p => p.id === productid);

    if (!product) {
        return res.status(404).json({ error: 'Product not found' });
    }

    product.name = name || product.name;
    product.price = price || product.price;
    product.stock = stock || product.stock;

    res.json({ message: 'Product updated', product });
});

// DELETE product by ID
app.delete('/products/:id', (req, res) => {
    const productid = parseInt(req.params.id);
    const index = products.findIndex(p => p.id === productid);

    if (index === -1) {
        return res.status(404).json({ error: 'Product not found' });
    }

    const deletedProduct = products.splice(index, 1);
    res.json({ message: 'Product deleted', product: deletedProduct[0] });
});

app.listen(port, () => {
    console.log(`Server is running at http://localhost:${port}`);
});
---------------------------------------------------------------------------------------------------------------
**3-Book-Management.js**
import express from 'express';

const app = express();
const port = 3000;

app.use(express.json());

let books = [
    { id: 1, title: 'The Great Gatsby', author: 'F. Scott Fitzgerald' },
    { id: 2, title: 'Pride and Prejudice', author: 'Jane Austen' }
];

app.get('/books', (req, res) => {
    res.send(JSON.stringify(books));
});

app.post('/books', (req, res) => {
    const { title, author } = req.body;

    if (!title || !author) {
        return res.status(400).send('Both title and author are required.');
    }

    const newBook = {
        id: books.length + 1,
        title,
        author
    };

    books.push(newBook);
    res.status(201).send(`Book added: ${JSON.stringify(newBook)}`);
});

app.put('/books/:id', (req, res) => {
    const bookId = parseInt(req.params.id);
    const { title, author } = req.body;

    const book = books.find(b => b.id === bookId);

    if (!book) {
        return res.status(404).send('Book not found.');
    }

    if (title) book.title = title;
    if (author) book.author = author;

    res.send(`Book updated: ${JSON.stringify(book)}`);
});

app.delete('/books/:id', (req, res) => {
    const bookId = parseInt(req.params.id);
    const index = books.findIndex(b => b.id === bookId);

    if (index === -1) {
        return res.status(404).send('Book not found.');
    }

    const deletedBook = books.splice(index, 1)[0];
    res.send(`Book deleted: ${JSON.stringify(deletedBook)}`);
});

app.listen(port, () => {
    console.log(`Server is running at http://localhost:${port}`);
});
-----------------------------------------------------------**-------------------------------------
**4-student-record-management.js**

import express from 'express';

const app = express();
const port = 3000;

app.use(express.json());

let students = [
    { id: 1, name: 'Alice Johnson', course: 'Mathematics' },
    { id: 2, name: 'Bob Smith', course: 'Physics' },
    { id: 3, name: 'Carol Lee', course: 'Computer Science' }
];

app.get('/students', (req, res) => {
    res.send(students);
});

app.get('/students/:id', (req, res) => {
    const studentId = parseInt(req.params.id);
    const student = students.find(s => s.id === studentId);

    if (!student) {
        return res.status(404).send('Student not found.');
    }

    res.send(student);
});

app.post('/students', (req, res) => {
    const { name, course } = req.body;

    if (!name || !course) {
        return res.status(400).send('Both name and course are required.');
    }

    const newId = students.length > 0 ? Math.max(...students.map(s => s.id)) + 1 : 1;

    const newStudent = {
        id: newId,
        name,
        course
    };

    students.push(newStudent);
    res.status(201).send(`Student added: ${JSON.stringify(newStudent)}`);
});

app.put('/students/:id', (req, res) => {
    const studentId = parseInt(req.params.id);
    const { name, course } = req.body;

    const student = students.find(s => s.id === studentId);

    if (!student) {
        return res.status(404).send('Student not found.');
    }

    if (name) student.name = name;
    if (course) student.course = course;

    res.send(`Student updated: ${JSON.stringify(student)}`);
});

app.delete('/students/:id', (req, res) => {
    const studentId = parseInt(req.params.id);
    const index = students.findIndex(s => s.id === studentId);

    if (index === -1) {
        return res.status(404).send('Student not found.');
    }

    const deletedStudent = students.splice(index, 1)[0];
    res.send(`Student deleted: ${JSON.stringify(deletedStudent)}`);
});

app.listen(port, () => {
    console.log(`Server is running at http://localhost:${port}`);
});
------------------------------------------------------------**--------------------------------
**5-Task-Management.js**


const express = require('express');
const app = express();
const PORT = 3000;

app.use(express.json());

let tasks = [
    {
        id: 1,
        title: "Buy groceries",
        description: "Milk, Eggs, Bread",
        status: "pending"
    },
    {
        id: 2,
        title: "Finish project",
        description: "Complete task manager API",
        status: "in-progress"
    }
];

// Generate unique task IDs
let nextId = 3;

app.get('/tasks', (req, res) => {
    res.send(tasks);
});

app.get('/tasks/:id', (req, res) => {
    const taskId = parseInt(req.params.id);
    const task = tasks.find(t => t.id === taskId);

    if (!task) {
        return res.status(404).send({ message: 'Task not found' });
    }

    res.send(task);
});

app.post('/tasks', (req, res) => {
    const { title, description } = req.body;

    if (!title || !description) {
        return res.status(400).send({ message: 'Title and description are required' });
    }

    const newTask = {
        id: nextId++,
        title,
        description,
        status: "pending"
    };

    tasks.push(newTask);
    res.send(newTask);
});
app.put('/tasks/:id', (req, res) => {
    const taskId = parseInt(req.params.id);
    const { title, description, status } = req.body;

    const task = tasks.find(t => t.id === taskId);
    if (!task) {
        return res.status(404).send({ message: 'Task not found' });
    }

    if (title) task.title = title;
    if (description) task.description = description;
    if (status) task.status = status;

    res.send({ message: 'Task updated', task });
});


app.delete('/tasks/:id', (req, res) => {
    const taskId = parseInt(req.params.id);
    const taskIndex = tasks.findIndex(t => t.id === taskId);

    if (taskIndex === -1) {
        return res.status(404).send({ message: 'Task not found' });
    }

    const deletedTask = tasks.splice(taskIndex, 1)[0];
    res.send({ message: 'Task deleted', task: deletedTask });
});

app.listen(PORT, () => {
    console.log(`Task Manager API is running on http://localhost:${PORT}`);
});
------------------------------------------------------------**-------------------------
**6-Library-Movies.js**

const express = require('express');
const app = express();

app.use(express.json());

let movies = [
  { id: 1, title: 'Inception', genre: 'Sci-Fi', rating: 9 },
  { id: 2, title: 'The Godfather', genre: 'Crime', rating: 10 },
  { id: 3, title: 'Toy Story', genre: 'Animation', rating: 8 },
];

function findMovieIndex(id) {
  return movies.findIndex(m => m.id === id);
}

app.get('/movies', (req, res) => {
  let allMovies = movies.map(m => JSON.stringify(m)).join('\n');
  res.send(allMovies || 'No movies found.');
});

app.get('/movies/:id', (req, res) => {
  const id = parseInt(req.params.id);
  if (isNaN(id)) return res.send('Invalid movie ID.');

  const movie = movies.find(m => m.id === id);
  if (!movie) return res.send(`Movie with ID ${id} not found.`);
  res.send(JSON.stringify(movie));
});

app.post('/movies', (req, res) => {
  const { id, title, genre, rating } = req.body;

  if (
    typeof id !== 'number' ||
    typeof title !== 'string' ||
    typeof genre !== 'string' ||
    typeof rating !== 'number' ||
    rating < 1 ||
    rating > 10
  ) {
    return res.send('Invalid input. Ensure id (number), title (string), genre (string), rating (1-10 number) are provided.');
  }

  if (movies.some(m => m.id === id)) {
    return res.send(`Movie with ID ${id} already exists.`);
  }

  movies.push({ id, title, genre, rating });
  res.send(`Movie with ID ${id} added successfully.`);
});

app.put('/movies/:id', (req, res) => {
  const id = parseInt(req.params.id);
  if (isNaN(id)) return res.send('Invalid movie ID.');

  const movieIndex = findMovieIndex(id);
  if (movieIndex === -1) return res.send(`Movie with ID ${id} not found.`);

  const { title, genre, rating } = req.body;

  if (title !== undefined && typeof title !== 'string') {
    return res.send('Invalid title.');
  }
  if (genre !== undefined && typeof genre !== 'string') {
    return res.send('Invalid genre.');
  }
  if (rating !== undefined && (typeof rating !== 'number' || rating < 1 || rating > 10)) {
    return res.send('Invalid rating. Must be a number between 1 and 10.');
  }

  if (title !== undefined) movies[movieIndex].title = title;
  if (genre !== undefined) movies[movieIndex].genre = genre;
  if (rating !== undefined) movies[movieIndex].rating = rating;

  res.send(`Movie with ID ${id} updated successfully.`);
});

app.delete('/movies/:id', (req, res) => {
  const id = parseInt(req.params.id);
  if (isNaN(id)) return res.send('Invalid movie ID.');

  const movieIndex = findMovieIndex(id);
  if (movieIndex === -1) {
    return res.send(`Movie with ID ${id} does not exist.`);
  }

  movies.splice(movieIndex, 1);
  res.send(`Movie with ID ${id} deleted successfully.`);
});

const PORT = 3000;
app.listen(PORT, () => {
  console.log(`Movie library app listening on port ${PORT}`);
});
