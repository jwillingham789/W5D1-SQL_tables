How many users are there?
    -  **50**
    -  `SELECT COUNT(id) FROM users;`

---

What are the 5 most expensive items?
      - **Small Cotton Gloves**
      - **Small Wooden Computer**
      - **Awesome Granite Pants**
      - **Sleek Wooden Hat**
      - **Ergonomic Steel Car**

      - `SELECT * FROM items ORDER BY price DESC LIMIT 5;`

---

What's the cheapest book? (Does that change for "category is exactly 'book'"
versus "category contains 'book'"?)
      - **Ergonomic Granite Chair**

      - `SELECT * FROM items WHERE category LIKE '%Books%' ORDER BY price LIMIT 1;`

      This does not change when we search for a category that is exactly "Books"

      - `SELECT * FROM items WHERE category = 'Books' ORDER BY price LIMIT 1;`

---

Who lives at "6439 Zetta Hills, Willmouth, WY"? Do they have another address?

      - **Corrine Little**

      - `SELECT users.first_name, users.last_name, addresses.street FROM users INNER JOIN addresses WHERE addresses.user_id = users.id AND addresses.street = '6439 Zetta Hills';`

      She also has the following address:

      - **54369 Wolff Forges, Lake Bryon, CA**

      - `SELECT users.first_name, users.last_name, addresses.street, addresses.city, addresses.state FROM users INNER JOIN addresses WHERE addresses.user_id = users.id AND users.first_name = 'Corrine';`

---

Correct Virginie Mitchell's address to "New York, NY, 10108".

      - `UPDATE addresses SET city = 'New York' WHERE user_id IN (SELECT id FROM users WHERE first_name = 'Virginie' AND state = 'NY');`

---

How much would it cost to buy one of each tool?

      - **$46,477**

      - `SELECT SUM(price) FROM items WHERE category LIKE '%Tools%';`

---

How many total items did we sell?

      - **$2,125**

      - `SELECT SUM(quantity) FROM orders;`

---

How much was spent on books?

      - **$1,081,352**

      - `SELECT SUM(items.price * orders.quantity) FROM items INNER JOIN orders WHERE orders.item_id = items.id AND items.category LIKE '%Book%';`

---

Simulate buying an item by inserting a User for yourself and an Order for that User.

      - `INSERT INTO users VALUES (null,'Jake','Willingham','jakewillingham789@gmail.com');`

      - `INSERT INTO orders VALUES (null, 51, 6, 15, datetime());`

---

What item was ordered most often? Grossed the most money?

      -Both the **Incredible Granite Car** and **Rustic Steel Shirt** were ordered 72 times

      - `SELECT items.title, SUM(orders.quantity), SUM(orders.quantity * items.price) FROM orders INNER JOIN items WHERE orders.item_id = items.id GROUP BY items.title ORDER BY SUM(orders.quantity) DESC;`

      - **Incredible Granite Car** grossed the most money at **$52,524**

      - `SELECT items.title, SUM(orders.quantity), SUM(orders.quantity * items.price) FROM orders INNER JOIN items WHERE orders.item_id = items.id GROUP BY items.title ORDER BY SUM(orders.quantity * items.price) DESC;`

---

What user spent the most?

      - **Hassan Runte** spent the most at **$639,386**

      - `SELECT users.first_name, users.last_name, SUM(orders.quantity * items.price) FROM items INNER JOIN orders ON orders.user_id = users.id INNER JOIN users WHERE orders.item_id = items.id GROUP BY orders.user_id ORDER BY SUM(orders.quantity * items.price) DESC;`

---

What were the top 3 highest grossing categories?

      - **Music, Sports & Clothing** at **$525,240**
      - **Beauty, Toys & Sports** at **$449,496**
      - **Sports** at **$448,410**

      - `SELECT items.category, SUM(orders.quantity * items.price) FROM items INNER JOIN orders WHERE orders.item_id = items.id GROUP BY items.category ORDER BY SUM(orders.quantity * items.price) DESC LIMIT 3;`
