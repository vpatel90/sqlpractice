1. How many users are there?
	* 50 `SELECT COUNT(*) FROM users;`
* What are the 5 most expensive items?
	`SELECT * FROM items ORDER BY price DESC LIMIT 5;`
	* 25    Small Cotton Gloves  Automotive, Shoes & Beauty  Multi-layered modular service-desk  9984
	* 83    Small Wooden Computer  Health  Re-engineered fault-tolerant adapter  9859
	* 100   Awesome Granite Pants  Toys & Books  Upgradable 24/7 access  9790
	* 40    Sleek Wooden Hat  Music & Baby  Quality-focused heuristic info-mediaries  9390
	* 60    Ergonomic Steel Car  Books & Outdoors  Enterprise-wide secondary firmware  9341
* What's the cheapest book? (Does that change for "category is exactly 'book'" versus "category contains 'book'"?)
	* Ergonomic Granite Chair  1496 `SELECT title, MIN(price) FROM items WHERE category LIKE "Books";`
	* Ergonomic Granite Chair  1496 `SELECT title, MIN(price) FROM items WHERE category LIKE "%Books%";`
* Who lives at "6439 Zetta Hills, Willmouth, WY"? Do they have another address? `SELECT first_name, last_name, street, city, state FROM addresses JOIN users ON users.id = addresses.user_id WHERE user_id IN (SELECT user_id FROM addresses WHERE street = "6439 Zetta Hills" AND city = "Willmouth" AND state = "WY");`
	* Corrine Little
	* She has 2 addresses
* Correct Virginie Mitchell's address to "New York, NY, 10108".
	* `UPDATE addresses SET city = "New York", state = "NY", zip = "10108" WHERE street = "12263 Jake Crossing";`
* How much would it cost to buy one of each tool?
	* 46477 `SELECT SUM(price) FROM items WHERE category LIKE "%Tools%";`
* How many total items did we sell?
	* 2125 `SELECT SUM(quantity) FROM orders`
* How much was spent on books?
	* 1081352 `SELECT SUM(items.price * orders.quantity) FROM items JOIN orders ON items.id = orders.item_id WHERE category LIKE "%Books%";`
	* 420566 `SELECT SUM(items.price * orders.quantity) FROM items JOIN orders ON items.id = orders.item_id WHERE category LIKE "Books";`
* Simulate buying an item by inserting a User for yourself and an Order for that User.
	* Insert user `INSERT INTO users (first_name, last_name, email) VALUES ("Vivek", "Patel", "vnp229@gmail.com");`
	* Inset orders `INSERT INTO orders (user_id, item_id, quantity) VALUES (51, 1, 2);`
	* `SELECT first_name, last_name, item_id, quantity FROM users JOIN orders ON users.id = orders.user_id WHERE first_name = "Vivek";`

	* Vivek  Patel          1     2
	
* What item was ordered most often? Grossed the most money?
	* Incredible Granite Car 72 `SELECT title, SUM(quantity) FROM orders JOIN items ON items.id = orders.item_id GROUP BY item_id ORDER BY SUM(quantity) DESC LIMIT 1;`
* What user spent the most?
	* Hassan  Runte 639386 `SELECT first_name, last_name, SUM(items.price * orders.quantity) FROM orders JOIN items ON items.id = orders.item_id JOIN users ON orders.user_id = users.id GROUP BY user_id ORDER BY SUM(items.price * orders.quantity) DESC LIMIT 1;`
* What were the top 3 highest grossing categories?
	* Music, Sports & Clothing  525240
Beauty, Toys & Sports  449496
Sports  448410 `SELECT category, SUM(items.price * orders.quantity) FROM orders JOIN items ON orders.item_id = items.id GROUP BY category ORDER BY SUM(items.price * orders.quantity) DESC LIMIT 3;`
