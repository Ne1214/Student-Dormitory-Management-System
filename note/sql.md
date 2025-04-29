
CREATE TABLE room (
    room_id INT PRIMARY KEY,
    room_number VARCHAR(50),
    building VARCHAR(50),
    capacity INT,
    current_occupants INT,
    electricity_usage FLOAT,
    electricity_fee FLOAT,
    water_usage FLOAT,
    water_fee FLOAT,
    is_paid BOOLEAN,
    last_payment_date DATE,
    note TEXT
);


CREATE TABLE resident (
    resident_id INT PRIMARY KEY,
    name VARCHAR(100),
    gender ENUM('male', 'female', 'other'),
    dob DATE,
    phone VARCHAR(20),
    email VARCHAR(100),
    room_id INT,
    FOREIGN KEY (room_id) REFERENCES room(room_id)
);


CREATE TABLE repair (
    repair_id INT PRIMARY KEY,
    resident_id INT,
    room_id INT,
    issue TEXT,
    status ENUM('pending', 'in_progress', 'completed', 'cancelled'),
    request_date DATE,
    completion_date DATE,
    FOREIGN KEY (resident_id) REFERENCES resident(resident_id),
    FOREIGN KEY (room_id) REFERENCES room(room_id)
);


CREATE TABLE announcement (
    announcement_id INT PRIMARY KEY,
    title VARCHAR(200),
    content TEXT,
    created_at DATE,
    updated_at DATE
);


CREATE TABLE rental (
    rental_id INT PRIMARY KEY,
    resident_id INT,
    item_name VARCHAR(100),
    quantity INT,
    status ENUM('borrowed', 'returned', 'overdue'),
    borrow_date DATE,
    return_date DATE,
    FOREIGN KEY (resident_id) REFERENCES resident(resident_id)
);


CREATE TABLE visitor (
    visitor_id INT PRIMARY KEY,
    name VARCHAR(100),
    phone VARCHAR(20),
    purpose TEXT,
    resident_id INT,
    check_in DATETIME,
    check_out DATETIME,
    FOREIGN KEY (resident_id) REFERENCES resident(resident_id)
);

CREATE TABLE lottery (
    lottery_id INT PRIMARY KEY,
    resident_id INT,
    application_date DATE,
    status ENUM('pending', 'selected', 'not_selected'),
    acceptance BOOLEAN,
    room_id INT,
    FOREIGN KEY (resident_id) REFERENCES resident(resident_id),
    FOREIGN KEY (room_id) REFERENCES room(room_id)
);
