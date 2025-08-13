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
