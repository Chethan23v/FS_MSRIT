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




*************************************************************************************
EJS
1.Movies-gallery
movie-gallery/
│ app.js
│ package.json
└── views/
│    ├── layout.ejs
│    └── index.ejs
└── public/
     └── styles.css
npm init -y
npm install express ejs
node app.js
++++++++++++++++++++++++++++++     
app.js
const express = require('express');
const path = require('path');

const app = express();

// Set EJS view engine
app.set('view engine', 'ejs');
app.set('views', path.join(__dirname, 'views'));

// Serve static CSS files
app.use(express.static(path.join(__dirname, 'public')));

// Movie data
const movies = [
  { title: 'The Shawshank Redemption', rating: 9.3 },
  { title: 'The Godfather', rating: 9.2 },
  { title: 'Inception', rating: 8.8 },
  { title: 'Interstellar', rating: 8.6 },
  { title: 'The Dark Knight', rating: 9.0 },
  { title: 'Jumanji', rating: 6.9 },
  { title: 'Avengers: Endgame', rating: 8.4 },
  { title: 'Frozen', rating: 7.5 }
];

// Routes
app.get('/', (req, res) => {
  res.render('index', { movies });
});

// Start server
const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Server running at http://localhost:${PORT}`);
});
*********************************layout.ejs
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Movie Gallery</title>
  <link rel="stylesheet" href="/styles.css" />
</head>
<body>

<header>
  <h1>My Movie Gallery</h1>
</header>

<main>
  <%- body %>
</main>

<footer>
  <p>&copy; 2025 Movie Gallery. All rights reserved.</p>
</footer>

</body>
</html>
**********************************index.ejs
<% movies.forEach(movie => { %>
  <div class="card <%= movie.rating > 8 ? 'highlight' : '' %>">
    <h2><%= movie.title %></h2>
    <p>Rating: <%= movie.rating %></p>
  </div>
<% }) %>
***************************************sytle.css
body {
  font-family: Arial, sans-serif;
  margin: 0;
  background: #f4f4f4;
}

header, footer {
  background: #333;
  color: white;
  text-align: center;
  padding: 1rem 0;
}

main {
  padding: 20px;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 20px;
}

.card {
  background: white;
  border-radius: 8px;
  padding: 15px;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
  transition: transform 0.2s ease;
}

.card:hover {
  transform: translateY(-5px);
}

.highlight {
  border-left: 5px solid #ff5722;
  background-color: #fff3e0;
}

----------------------------------------------*************------------------------
2.REceipe-page
recipe-app/
│
├── app.js
├── views/
│   └── recipe.pug       node.app.js
├── public/
│   ├── styles.css
│   └── images/
│       └── recipe.jpg

app.js
const express = require('express');
const path = require('path');

const app = express();

// Set Pug as the view engine
app.set('view engine', 'pug');
app.set('views', path.join(__dirname, 'views'));

// Static files
app.use('/assets', express.static(path.join(__dirname, 'public')));

// Sample recipe data
const recipe = {
  name: 'Spaghetti Aglio e Olio',
  ingredients: [
    { name: 'Spaghetti', highlight: false },
    { name: 'Garlic', highlight: true },
    { name: 'Olive Oil', highlight: true },
    { name: 'Chili Flakes', highlight: false },
    { name: 'Parsley', highlight: false },
    { name: 'Salt', highlight: false }
  ],
  steps: [
    'Boil spaghetti in salted water until al dente.',
    'Heat olive oil in a pan, add garlic and chili flakes.',
    'Drain pasta and toss with the garlic oil.',
    'Add parsley and adjust seasoning.',
    'Serve hot.'
  ]
};

app.get('/', (req, res) => {
  res.render('recipe', { recipe });
});

const PORT = 3000;
app.listen(PORT, () => {
  console.log(`Recipe app running at http://localhost:${PORT}`);
});
-----------------------------*********   views/recipe.pug
doctype html
html
  head
    meta(charset="UTF-8")
    meta(name="viewport" content="width=device-width, initial-scale=1.0")
    title= recipe.name
    link(rel="stylesheet", href="/assets/styles.css")
  body
    header
      h1= recipe.name
    main
      img(src="/assets/images/recipe.jpg", alt=recipe.name)
      
      h2 Ingredients
      ul
        each ingredient in recipe.ingredients
          li(style= ingredient.highlight ? 'color: red; font-weight: bold;' : '') #{ingredient.name}

      h2 Steps
      ol
        each step in recipe.steps
          li= step
    footer
      p &copy; 2025 My Recipe App
------------------------------------**************public/style.css
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
  background: #f4f4f4;
}

header, footer {
  background: #333;
  color: white;
  text-align: center;
  padding: 1rem;
}

main {
  padding: 20px;
  max-width: 600px;
  margin: auto;
  background: white;
  border-radius: 8px;
}

img {
  width: 100%;
  border-radius: 8px;
  margin-bottom: 20px;
}

ul, ol {
  padding-left: 20px;
}
-----------------------------------------------------------------------------------
3-Feedback.form
travel-feedback/
│── app.js
│── views/
│    ├── form.ejs
│    └── result.ejs
│── uploads/
│── public/
│    └── styles.css
│── package.json
-----------------------------app.js
const express = require('express');
const multer = require('multer');
const path = require('path');

const app = express();

// Static files
app.use(express.static(path.join(__dirname, 'public')));
app.use('/uploads', express.static(path.join(__dirname, 'uploads')));

// EJS
app.set('view engine', 'ejs');

// Multer storage
const storage = multer.diskStorage({
  destination: (req, file, cb) => {
    cb(null, 'uploads/');
  },
  filename: (req, file, cb) => {
    cb(null, Date.now() + path.extname(file.originalname));
  }
});
const upload = multer({ storage });

// Routes
app.get('/', (req, res) => {
  res.render('form');
});

app.post('/submit', upload.single('screenshot'), (req, res) => {
  const { name, email } = req.body;
  const imagePath = `/uploads/${req.file.filename}`;
  res.render('result', { name, email, imagePath });
});

const PORT = 3000;
app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
-----------------------------------views/form.ejs
<!DOCTYPE html>
<html>
<head>
  <title>Travel Feedback Form</title>
  <link rel="stylesheet" href="/styles.css">
</head>
<body>
  <h1>Travel Website Feedback</h1>
  <form action="/submit" method="POST" enctype="multipart/form-data">
    <label>Name:</label>
    <input type="text" name="name" required>

    <label>Email:</label>
    <input type="email" name="email" required>

    <label>Screenshot of Issue:</label>
    <input type="file" name="screenshot" accept="image/*" required>

    <button type="submit">Submit Feedback</button>
  </form>
</body>
</html>
-------------------------------views/result.ejs
<!DOCTYPE html>
<html>
<head>
  <title>Feedback Submitted</title>
  <link rel="stylesheet" href="/styles.css">
</head>
<body>
  <h1>Thank You for Your Feedback!</h1>
  <p><strong>Name:</strong> <%= name %></p>
  <p><strong>Email:</strong> <%= email %></p>
  <h2>Screenshot:</h2>
  <img src="<%= imagePath %>" alt="Screenshot" style="max-width:300px;">
</body>
</html>
----------------------------public/style.css
body {
  font-family: Arial, sans-serif;
  background: #f4f4f4;
  padding: 20px;
}

form {
  background: white;
  padding: 20px;
  border-radius: 8px;
  width: 300px;
}

label {
  display: block;
  margin-top: 10px;
}

input, button {
  width: 100%;
  padding: 8px;
  margin-top: 5px;
}

button {
  background: #28a745;
  border: none;
  color: white;
  cursor: pointer;
}

button:hover {
  background: #218838;
}
----------------------------------------************----------------------------------
