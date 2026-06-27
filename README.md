# E-Commerce-Website-
This is a full-stack e-commerce web application developed using PHP and MySQL. It simulates an online shopping system where users can browse products, add items to a cart, and view total price using session management.  The project demonstrates core concepts of backend development, database integration, and session handling.
## README.md

# PHP E-Commerce Website

Simple PHP + MySQL e-commerce project.

## Features
- Product listing
- Add to cart
- View cart

## Run Steps
1. Import SQL database
2. Start XAMPP
3. Place folder in htdocs
4. Open:
   http://localhost/ecommerce/index.php



## database.php 
<?php
$conn = mysqli_connect("localhost", "root", "", "ecommerce");

if (!$conn) {
    die("Connection failed: " . mysqli_connect_error());
}
?>
<?php
include "database.php";

$result = mysqli_query($conn, "SELECT * FROM products");
?>

<h1>Products</h1>

##  index.php (Product List
<?php while($row = mysqli_fetch_assoc($result)) { ?>

<div style="border:1px solid #000; padding:10px; margin:10px;">
    <img src="images/<?php echo $row['image']; ?>" width="150"><br>
    <h3><?php echo $row['name']; ?></h3>
    <p>₹<?php echo $row['price']; ?></p>

    <a href="cart.php?id=<?php echo $row['id']; ?>">Add to Cart</a>
</div>

<?php } ?>

## SQL Database

CREATE DATABASE ecommerce;

USE ecommerce;

CREATE TABLE products (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255),
    price DECIMAL(10,2),
    image VARCHAR(255)
);

CREATE TABLE cart (
    id INT AUTO_INCREMENT PRIMARY KEY,
    product_id INT,
    qty INT
);
## cart.php
<?php
session_start();

$id = $_GET['id'];

if(!isset($_SESSION['cart'])){
    $_SESSION['cart'] = [];
}

if(isset($_SESSION['cart'][$id])){
    $_SESSION['cart'][$id]++;
} else {
    $_SESSION['cart'][$id] = 1;
}

echo "Product added to cart!";
echo "<br><a href='index.php'>Continue Shopping</a>";
?>
## viewcart.php
<?php
session_start();
include "database.php";

echo "<h2>Your Cart</h2>";

if(!isset($_SESSION['cart']) || empty($_SESSION['cart'])){
    echo "Cart is empty";
    exit;
}

$total = 0;

foreach($_SESSION['cart'] as $id => $qty){

    $res = mysqli_query($conn, "SELECT * FROM products WHERE id=$id");
    $row = mysqli_fetch_assoc($res);

    echo $row['name']." - ₹".$row['price']." x ".$qty."<br>";

    $total += $row['price'] * $qty;
}

echo "<h3>Total: ₹".$total."</h3>";
?>
