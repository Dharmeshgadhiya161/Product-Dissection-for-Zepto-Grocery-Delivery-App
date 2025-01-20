## Case Study 

## Product Dissection Zepto Grocery Delivery App
Product Dissection for Zepto Grocery Delivery App

#
Zepto's marketing strategy has helped the brand grow as India's fastest delivery app.
Zepto app has plans to expand into newer cities as quickly as possible. They are also planning to increase their coverage to micro-markets and be the leaders in the sector.
Several other competitors are trying to participate in the market and grow quickly. However, they cannot promise things like 10-minute delivery consistently.
Zepto is a detail-oriented and intense business with technical and operating discipline that allows them to ensure 10-minute delivery.

#

### USERS TABLE 
CREATE TABLE Users (
    user_id INT PRIMARY KEY AUTO_INCREMENT,
    user_name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    address TEXT,
    phone_number VARCHAR(15),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

INSERT INTO Users (user_name, email, address, phone_number) VALUES
('Alice', 'alice@example.com', '123 Main St, Springfield', '1234567890'),
('Bob', 'bob@example.com', '456 Elm St, Shelbyville', '9876543210'),
('Charlie', 'charlie@example.com', '789 Oak St, Capital City', '5555555555'),
('Diana', 'diana@example.com', '321 Pine St, Ogdenville', '4444444444'),
('Eve', 'eve@example.com', '654 Willow St, Springfield', '3333333333');

### CATEGORIES TABLE
CREATE TABLE Categories (
    category_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    description TEXT
);

INSERT INTO Categories (name, description) VALUES
('Electronics', 'Devices and gadgets'),
('Books', 'Printed and digital books'),
('Clothing', 'Apparel and accessories'),
('Home & Kitchen', 'Home appliances and kitchenware'),
('Toys', 'Toys and games for all ages');

### PRODUCTS TABLE 
CREATE TABLE Products (
    product_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    image_url TEXT,
    description TEXT,
    price DECIMAL(10, 2) NOT NULL,
    stock_quantity INT NOT NULL,
    category_id INT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (category_id) REFERENCES Categories(category_id)
);

INSERT INTO Products (name, image_url, description, price, stock_quantity, category_id) VALUES
('Smartphone', 'https://example.com/smartphone.jpg', 'High-end smartphone', 699.99, 50, 1),
('Laptop', 'https://example.com/laptop.jpg', 'Powerful laptop', 1199.99, 30, 1),
('Novel', 'https://example.com/novel.jpg', 'Bestselling novel', 19.99, 100, 2),
('T-shirt', 'https://example.com/tshirt.jpg', 'Comfortable T-shirt', 15.99, 200, 3),
('Blender', 'https://example.com/blender.jpg', 'High-speed blender', 49.99, 70, 4);

### ORDERS TABLE
CREATE TABLE Orders (
    order_id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NOT NULL,
    order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    total_amount DECIMAL(10, 2) NOT NULL,
    status VARCHAR(50),
    delivery_address TEXT,
    delivery_time_slot VARCHAR(50),
    payment_status VARCHAR(50),
    delivery_method VARCHAR(50),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES Users(user_id)
);

INSERT INTO Orders (user_id, total_amount, status, delivery_address, delivery_time_slot, payment_status, delivery_method) VALUES
(1, 720.00, 'Pending', '123 Main St, Springfield', '9 AM - 12 PM', 'Paid', 'Standard'),
(2, 1250.00, 'In Progress', '456 Elm St, Shelbyville', '1 PM - 3 PM', 'Pending', 'Express'),
(3, 20.00, 'Delivered', '789 Oak St, Capital City', '3 PM - 6 PM', 'Paid', 'Standard'),
(4, 16.00, 'Canceled', '321 Pine St, Ogdenville', '10 AM - 12 PM', 'Pending', 'Express'),
(5, 50.00, 'Delivered', '654 Willow St, Springfield', '1 PM - 3 PM', 'Paid', 'Standard');

### ORDER ITEMS TABLE
CREATE TABLE OrderItems (
    order_item_id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL,
    unit_price DECIMAL(10, 2) NOT NULL,
    total_price DECIMAL(10, 2) GENERATED ALWAYS AS (quantity * unit_price) STORED,
    FOREIGN KEY (order_id) REFERENCES Orders(order_id),
    FOREIGN KEY (product_id) REFERENCES Products(product_id)
);

INSERT INTO OrderItems (order_id, product_id, quantity, unit_price) VALUES
(1, 1, 1, 699.99),
(2, 2, 1, 1199.99),
(3, 3, 1, 19.99),
(4, 4, 1, 15.99),
(5, 5, 1, 49.99);

### PAYMENTS TABLE
CREATE TABLE Payments (
    payment_id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT NOT NULL,
    payment_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    amount_paid DECIMAL(10, 2),
    payment_method VARCHAR(50),
    payment_status VARCHAR(50),
    transaction_id VARCHAR(100),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (order_id) REFERENCES Orders(order_id)
);

INSERT INTO Payments (order_id, amount_paid, payment_method, payment_status, transaction_id) VALUES
(1, 720.00, 'Credit Card', 'Completed', 'TXN001'),
(2, 1250.00, 'PayPal', 'Pending', 'TXN002'),
(3, 20.00, 'Debit Card', 'Completed', 'TXN003'),
(4, 16.00, 'UPI', 'Failed', 'TXN004'),
(5, 50.00, 'Cash', 'Completed', 'TXN005');

### DELIVERIES TABLE
CREATE TABLE Deliveries (
    delivery_id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT NOT NULL,
    delivery_person_name VARCHAR(100),
    delivery_address TEXT,
    delivery_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    status VARCHAR(50),
    delivery_notes TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (order_id) REFERENCES Orders(order_id)
);

INSERT INTO Deliveries (order_id, delivery_person_name, delivery_address, status, delivery_notes) VALUES
(1, 'Sam', '123 Main St, Springfield', 'Delivered', 'Delivered on time'),
(2, 'Max', '456 Elm St, Shelbyville', 'In Transit', 'Traffic delay'),
(3, 'Amy', '789 Oak St, Capital City', 'Delivered', 'Left at front door'),
(4, 'Jack', '321 Pine St, Ogdenville', 'Canceled', 'Order was canceled'),
(5, 'Emma', '654 Willow St, Springfield', 'Delivered', 'Delivered with care');

## REVIEWS TABLE
CREATE TABLE Reviews (
    review_id INT PRIMARY KEY AUTO_INCREMENT,
    product_id INT NOT NULL,
    user_id INT NOT NULL,
    rating INT CHECK (rating BETWEEN 1 AND 5),
    review_text TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (product_id) REFERENCES Products(product_id),
    FOREIGN KEY (user_id) REFERENCES Users(user_id)
);

INSERT INTO Reviews (product_id, user_id, rating, review_text) VALUES
(1, 1, 5, 'Amazing product!'),
(2, 2, 4, 'Very good quality'),
(3, 3, 3, 'Average experience'),
(4, 4, 2, 'Not as expected'),
(5, 5, 5, 'Excellent value for money');

### DARK STORES TABLE
CREATE TABLE DarkStores (
    store_id INT PRIMARY KEY AUTO_INCREMENT,
    product_id INT NOT NULL,
    location VARCHAR(255),
    operating_hours VARCHAR(100),
    status VARCHAR(50),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (product_id) REFERENCES Products(product_id)
);

INSERT INTO DarkStores (product_id, location, operating_hours, status) VALUES
(1, 'Springfield Warehouse', '9 AM - 9 PM', 'Operational'),
(2, 'Shelbyville Warehouse', '8 AM - 10 PM', 'Operational'),
(3, 'Capital City Warehouse', '7 AM - 8 PM', 'Operational'),
(4, 'Ogdenville Warehouse', '6 AM - 6 PM', 'Closed'),
(5, 'Springfield West Warehouse', '10 AM - 8 PM', 'Operational');
