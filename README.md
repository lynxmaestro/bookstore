
DataBase Setup

sudo yum install mariadb105-server -y
sudo systemctl restart mariadb.service
sudo systemctl enable mariadb.service

mysql_secure_installation

[ec2-user@fujikomalan zomato-production]$ sudo mysql_secure_installation

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user. If you've just installed MariaDB, and
haven't set the root password yet, you should just press enter here.

Enter current password for root (enter for none): <<========== Press Enter
OK, successfully used password, moving on...

Setting the root password or using the unix_socket ensures that nobody
can log into the MariaDB root user without the proper authorisation.

You already have your root account protected, so you can safely answer 'n'.

Switch to unix_socket authentication [Y/n] n
 ... skipping.

You already have your root account protected, so you can safely answer 'n'.

Change the root password? [Y/n] y
New password:   <<=== my password (( mysqlroot123 ))
Re-enter new password: <<=== my password (( mysqlroot123 ))
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] y
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!

Creating Databse,table

-- 1. Create database
CREATE DATABASE store;

-- 2. Use the database
USE store;

-- 3. Create table
CREATE TABLE books (
    id INT AUTO_INCREMENT PRIMARY KEY,
    book_name VARCHAR(255),
    author_name VARCHAR(255),
    price DECIMAL(10,2),
    no_of_pages INT,
    type VARCHAR(100),
    year_of_release YEAR,
    isbn_number VARCHAR(20)
);

-- 4. Insert 20 demo entries
INSERT INTO books (book_name, author_name, price, no_of_pages, type, year_of_release, isbn_number) VALUES
('The Silent Forest', 'Arun Nair', 299.99, 320, 'Fiction', 2015, '978000000001'),
('Deep Learning Basics', 'Suresh Kumar', 599.50, 450, 'Technology', 2020, '978000000002'),
('History of India', 'Ravi Menon', 450.00, 500, 'History', 2010, '978000000003'),
('Ocean Secrets', 'Meera Pillai', 350.75, 280, 'Science', 2018, '978000000004'),
('Python for Beginners', 'Anil Das', 399.99, 350, 'Technology', 2021, '978000000005'),
('Ancient Civilizations', 'John Mathew', 550.00, 600, 'History', 2008, '978000000006'),
('Mystery of Mountains', 'Kiran Varma', 275.20, 310, 'Fiction', 2016, '978000000007'),
('Space Exploration', 'Nikhil Rao', 620.00, 420, 'Science', 2019, '978000000008'),
('Data Structures', 'Priya Iyer', 480.00, 390, 'Technology', 2017, '978000000009'),
('The Last Kingdom', 'Ajay Krishnan', 330.00, 340, 'Fiction', 2014, '978000000010'),
('Modern Physics', 'Albert Dsouza', 700.00, 520, 'Science', 2012, '978000000011'),
('Cooking Made Easy', 'Lakshmi Nair', 250.00, 200, 'Lifestyle', 2022, '978000000012'),
('Gardening Guide', 'Thomas Joseph', 275.00, 220, 'Lifestyle', 2019, '978000000013'),
('Machine Learning Pro', 'Rahul Sharma', 800.00, 480, 'Technology', 2023, '978000000014'),
('World War II', 'George Thomas', 600.00, 550, 'History', 2011, '978000000015'),
('The Hidden Truth', 'Sneha Reddy', 310.00, 330, 'Fiction', 2016, '978000000016'),
('Biology Basics', 'Neha Gupta', 420.00, 360, 'Science', 2013, '978000000017'),
('Startup Guide', 'Vikram Singh', 390.00, 300, 'Business', 2020, '978000000018'),
('Invest Smart', 'Rohit Mehta', 450.00, 280, 'Business', 2021, '978000000019'),
('Travel Diaries', 'Ananya Bose', 320.00, 260, 'Travel', 2018, '978000000020');

Creating User For Application

[ec2-user@fujikomalan zomato-production]$ mysql -u root -pmysqlroot123

Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 11
Server version: 10.5.29-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.


MariaDB [(none)]> CREATE USER 'appuser'@'%' IDENTIFIED BY 'appuser123';
Query OK, 0 rows affected (0.002 sec)

MariaDB [(none)]> GRANT ALL PRIVILEGES ON store.* TO 'appuser'@'%';
Query OK, 0 rows affected (0.002 sec)

MariaDB [(none)]> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.000 sec)

Testing User Access

[ec2-user@fujikomalan zomato-production]$ mysql -u appuser -pappuser123

Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 13
Server version: 10.5.29-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.


MariaDB [(none)]> select book_name,author_name from store.books;
+-----------------------+---------------+
| book_name             | author_name   |
+-----------------------+---------------+
| The Silent Forest     | Arun Nair     |
| Deep Learning Basics  | Suresh Kumar  |
| History of India      | Ravi Menon    |
| Ocean Secrets         | Meera Pillai  |
| Python for Beginners  | Anil Das      |
| Ancient Civilizations | John Mathew   |
| Mystery of Mountains  | Kiran Varma   |
| Space Exploration     | Nikhil Rao    |
| Data Structures       | Priya Iyer    |
| The Last Kingdom      | Ajay Krishnan |
| Modern Physics        | Albert Dsouza |
| Cooking Made Easy     | Lakshmi Nair  |
| Gardening Guide       | Thomas Joseph |
| Machine Learning Pro  | Rahul Sharma  |
| World War II          | George Thomas |
| The Hidden Truth      | Sneha Reddy   |
| Biology Basics        | Neha Gupta    |
| Startup Guide         | Vikram Singh  |
| Invest Smart          | Rohit Mehta   |
| Travel Diaries        | Ananya Bose   |
+-----------------------+---------------+
20 rows in set (0.000 sec)

MariaDB [(none)]> exit

On application Server

sudo yum install httpd php-fpm php-mysqlnd -y
sudo systemctl restart httpd.service php-fpm.service
sudo systemctl enable httpd.service php-fpm.service

/var/www/html/
 ├── config.php
 ├── index.php
 ├── book.php
 └── style.css

config.php

<?php
$host = "localhost";
$user = "store_user";
$password = "StrongPassword@123";
$database = "store";

$conn = new mysqli($host, $user, $password, $database);

// Check connection
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}
?>

index.php

<?php
include 'config.php';

$sql = "SELECT id, book_name FROM books";
$result = $conn->query($sql);
?>

<!DOCTYPE html>
<html>
<head>
    <title>Book Store</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

<h1>📚 Book Store</h1>

<ul>
<?php
while ($row = $result->fetch_assoc()) {
    echo "<li>";
    echo "<a href='book.php?id=" . $row['id'] . "'>";
    echo htmlspecialchars($row['book_name']);
    echo "</a>";
    echo "</li>";
}
?>
</ul>

</body>
</html>

book.php

<?php
include 'config.php';

if (!isset($_GET['id'])) {
    die("Invalid request");
}

$id = intval($_GET['id']);

// Prepared statement (proper way)
$stmt = $conn->prepare("SELECT * FROM books WHERE id = ?");
$stmt->bind_param("i", $id);
$stmt->execute();
$result = $stmt->get_result();


if ($result->num_rows === 0) {
    die("Book not found");
}

$book = $result->fetch_assoc();
?>

<!DOCTYPE html>
<html>
<head>
    <title><?php echo htmlspecialchars($book['book_name']); ?></title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

<a href="index.php" class="home-btn">⬅ Home</a>

<h1><?php echo htmlspecialchars($book['book_name']); ?></h1>

<div class="card">
    <p><strong>Author:</strong> <?php echo htmlspecialchars($book['author_name']); ?></p>
    <p><strong>Price:</strong> ₹<?php echo $book['price']; ?></p>
    <p><strong>Pages:</strong> <?php echo $book['no_of_pages']; ?></p>
    <p><strong>Type:</strong> <?php echo htmlspecialchars($book['type']); ?></p>
    <p><strong>Year:</strong> <?php echo $book['year_of_release']; ?></p>
    <p><strong>ISBN:</strong> <?php echo $book['isbn_number']; ?></p>
</div>

</body>
</html>

style.css

/* =========================
   RESET
========================= */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

/* =========================
   BODY
========================= */
body {
    font-family: "Segoe UI", Tahoma, sans-serif;
    background: linear-gradient(135deg, #eef2f7, #d9e4f5);
    min-height: 100vh;
    padding: 40px;
    color: #2c3e50;
}

/* =========================
   TITLE
========================= */
h1 {
    text-align: center;
    margin-bottom: 30px;
    font-size: 2.5rem;
    letter-spacing: 1px;
}

/* =========================
   GRID LAYOUT
========================= */
ul {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 20px;
    list-style: none;
}

/* =========================
   BOOK CARD
========================= */
li {
    background: white;
    padding: 20px;
    border-radius: 15px;
    box-shadow: 0 10px 25px rgba(0,0,0,0.08);
    transition: all 0.3s ease;
    position: relative;
    overflow: hidden;
    cursor: pointer;
}

/* Hover effect */
li:hover {
    transform: translateY(-5px);
    box-shadow: 0 15px 35px rgba(0,0,0,0.15);
}

/* =========================
   GRADIENT BORDER (FIXED)
========================= */
li::before {
    content: "";
    position: absolute;
    inset: 0;
    border-radius: 15px;
    padding: 2px;
    background: linear-gradient(45deg, #007BFF, #00c6ff);

    -webkit-mask:
        linear-gradient(#fff 0 0) content-box,
        linear-gradient(#fff 0 0);
    -webkit-mask-composite: xor;
    mask-composite: exclude;

    pointer-events: none; /* ✅ critical fix */
}

/* =========================
   LINK STYLE
========================= */
a {
    text-decoration: none;
    color: #007BFF;
    font-size: 1.2rem;
    font-weight: 600;
    display: block;
}

a:hover {
    color: #0056b3;
}

/* =========================
   DETAIL CARD
========================= */
.card {
    max-width: 500px;
    margin: auto;
    background: white;
    padding: 30px;
    border-radius: 20px;
    box-shadow: 0 20px 40px rgba(0,0,0,0.1);
}

/* Card text */
.card p {
    margin: 12px 0;
    font-size: 1.1rem;
}

/* Labels */
.card strong {
    color: #34495e;
}

/* =========================
   HOME BUTTON
========================= */
.home-btn {
    display: inline-block;
    margin-bottom: 20px;
    padding: 10px 18px;
    background: linear-gradient(45deg, #007BFF, #00c6ff);
    color: white;
    border-radius: 25px;
    font-weight: 600;
    transition: all 0.3s ease;
}

.home-btn:hover {
    transform: scale(1.05);
    box-shadow: 0 5px 15px rgba(0,123,255,0.4);
}

/* =========================
   RESPONSIVE
========================= */
@media (max-width: 600px) {
    body {
        padding: 20px;
    }

    h1 {
        font-size: 2rem;
    }
}

