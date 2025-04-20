# bus-reservation-system

# Bus Reservation System - Database Design

This is the database schema for the **Bus Reservation System**. It contains tables for managing admins, users, operators, buses, routes, trips, bookings, and payments. The system allows users to book bus tickets, view available trips, and handle cancellations due to emergencies like landslides or other disruptions.

##ERD :  https://dbdiagram.io/d/6801549a1ca52373f56c912f

## Table of Contents

1. [Admins](#admins)
2. [Users](#users)
3. [Operators](#operators)
4. [Buses](#buses)
5. [Routes](#routes)
6. [Trips](#trips)
7. [Seats](#seats)
8. [Bookings](#bookings)
9. [Payments](#payments)
10. [Data Flow Process](#data-flow-process)


## Database Tables

### 1. **Admins**
The `admins` table stores details about the admin users who can manage the system.

| Field         | Type      | Description                               |
|---------------|-----------|-------------------------------------------|
| id            | int       | Primary key, auto-incremented admin ID   |
| email         | text      | Unique email address of the admin        |
| password      | text      | Admin's password                         |
| full_name     | text      | Admin's full name                        |
| role          | text      | Role of the admin (default: 'ADMIN')     |
| last_login    | timestamp | Timestamp of the last login              |
| is_active     | boolean   | Whether the admin is active (default: true)|
| created_at    | timestamp | Timestamp of when the admin was created  |
| updated_at    | timestamp | Timestamp of the last update             |

### 2. **Users**
The `users` table stores details about customers who book bus tickets.

| Field         | Type      | Description                               |
|---------------|-----------|-------------------------------------------|
| id            | int       | Primary key, auto-incremented user ID    |
| full_name     | text      | Full name of the user                    |
| email         | text      | Unique email address of the user         |
| phone         | text      | Unique phone number of the user          |
| password      | text      | User's password                          |
| last_login    | timestamp | Timestamp of the last login              |
| is_active     | boolean   | Whether the user is active (default: true)|
| created_at    | timestamp | Timestamp of when the user was created   |
| updated_at    | timestamp | Timestamp of the last update             |

### 3. **Operators**
The `operators` table stores information about bus operators.

| Field         | Type      | Description                               |
|---------------|-----------|-------------------------------------------|
| id            | int       | Primary key, auto-incremented operator ID|
| name          | text      | Name of the operator                     |
| contact       | text      | Contact information of the operator      |
| email         | text      | Email address of the operator            |
| logo_url      | text      | URL to the operator's logo               |
| is_active     | boolean   | Whether the operator is active (default: true)|
| created_by    | int       | Reference to the admin who created the operator |
| created_at    | timestamp | Timestamp of when the operator was created |
| updated_at    | timestamp | Timestamp of the last update             |

### 4. **Buses**
The `buses` table stores information about buses operated by the operators.

| Field         | Type      | Description                               |
|---------------|-----------|-------------------------------------------|
| id            | int       | Primary key, auto-incremented bus ID     |
| operator_id   | int       | Foreign key, reference to the operator   |
| plate_number  | text      | Unique plate number of the bus           |
| model         | text      | Model of the bus                         |
| total_seats   | int       | Total number of seats available in the bus|
| created_by    | int       | Reference to the admin who created the bus|
| created_at    | timestamp | Timestamp of when the bus was created    |
| updated_at    | timestamp | Timestamp of the last update             |

### 5. **Routes**
The `routes` table stores details about the bus routes.

| Field         | Type      | Description                               |
|---------------|-----------|-------------------------------------------|
| id            | int       | Primary key, auto-incremented route ID   |
| start_location| text      | Starting location of the route           |
| end_location  | text      | Ending location of the route             |
| distance_km   | decimal   | Distance of the route in kilometers      |
| estimated_time| text      | Estimated travel time for the route      |
| created_by    | int       | Reference to the admin who created the route |
| created_at    | timestamp | Timestamp of when the route was created  |
| updated_at    | timestamp | Timestamp of the last update             |

### 6. **Trips**
The `trips` table stores information about each bus trip.

| Field            | Type      | Description                               |
|------------------|-----------|-------------------------------------------|
| id               | int       | Primary key, auto-incremented trip ID    |
| bus_id           | int       | Foreign key, reference to the bus        |
| route_id         | int       | Foreign key, reference to the route      |
| departure_time   | timestamp | Departure time of the trip               |
| arrival_time     | timestamp | Arrival time of the trip                 |
| price            | numeric   | Price for the trip                       |
| available_seats  | int       | Number of available seats for the trip   |
| status           | text      | Status of the trip (SCHEDULED, CANCELLED, COMPLETED) |
| cancellation_reason | text    | Reason for cancellation (if any)         |
| canceled_by      | text      | Who canceled the trip (USER, OPERATOR, ADMIN, SYSTEM) |
| created_by       | int       | Reference to the admin who created the trip |
| created_at       | timestamp | Timestamp of when the trip was created   |
| updated_at       | timestamp | Timestamp of the last update            |

### 7. **Seats**
The `seats` table stores information about the available seats for each trip.

| Field         | Type      | Description                               |
|---------------|-----------|-------------------------------------------|
| id            | int       | Primary key, auto-incremented seat ID    |
| trip_id       | int       | Foreign key, reference to the trip       |
| seat_number   | text      | Seat number (e.g., A1, B2)               |
| is_booked     | boolean   | Whether the seat is booked (default: false) |

### 8. **Bookings**
The `bookings` table stores details about user bookings for trips.

| Field             | Type      | Description                               |
|-------------------|-----------|-------------------------------------------|
| id                | int       | Primary key, auto-incremented booking ID |
| user_id           | int       | Foreign key, reference to the user       |
| trip_id           | int       | Foreign key, reference to the trip       |
| seat_id           | int       | Foreign key, reference to the seat       |
| booking_time      | timestamp | Time when the booking was made           |
| status            | text      | Status of the booking (CONFIRMED, CANCELLED, EXPIRED) |
| cancellation_reason | text     | Reason for cancellation (if any)         |
| canceled_by       | text      | Who canceled the booking (USER, OPERATOR, ADMIN, SYSTEM) |
| created_at        | timestamp | Timestamp of when the booking was created |
| updated_at        | timestamp | Timestamp of the last update            |

### 9. **Payments**
The `payments` table stores information about payments for bookings.

| Field         | Type      | Description                               |
|---------------|-----------|-------------------------------------------|
| id            | int       | Primary key, auto-incremented payment ID |
| booking_id    | int       | Foreign key, reference to the booking    |
| amount        | numeric   | Amount paid for the booking              |
| method        | text      | Payment method (eSewa, Khalti, Card, Cash)|
| payment_time  | timestamp | Time when the payment was made           |
| status        | text      | Payment status (PAID, PENDING, FAILED)   |
| created_at    | timestamp | Timestamp of when the payment was made   |

---

##Dummy data


## Admins

The `admins` table stores information about system administrators. These admins are responsible for managing operators, buses, routes, and overall system operations.

| id  | email                | password  | full_name    | role  | last_login         | is_active | created_at         | updated_at         |
|-----|----------------------|-----------|--------------|-------|--------------------|-----------|--------------------|--------------------|
| 1   | admin1@example.com   | admin123  | Adam Lambert | ADMIN | 2025-04-20 09:00   | true      | 2025-04-20 09:00   | 2025-04-20 09:00   |
| 2   | admin2@example.com   | admin456  | Eve Smith    | ADMIN | 2025-04-19 08:45   | true      | 2025-04-19 08:45   | 2025-04-19 08:45   |
| 3   | admin3@example.com   | admin789  | John Doe     | ADMIN | 2025-04-18 07:30   | true      | 2025-04-18 07:30   | 2025-04-18 07:30   |

---

## Users

The `users` table contains the registered users who will book tickets. Each user has a unique ID, and their login credentials and status are tracked.

| id  | full_name         | email              | phone      | password  | last_login         | is_active | created_at         | updated_at         |
|-----|-------------------|--------------------|------------|-----------|--------------------|-----------|--------------------|--------------------|
| 1   | Sisan Baniya      | sisan@gmail.com    | 988223334  | hashed123 | 2025-04-20 10:00   | true      | 2025-04-20 10:00   | 2025-04-20 10:00   |
| 2   | Nabin Shrestha    | nabin@gmail.com    | 980000111  | pass456   | 2025-04-19 11:30   | true      | 2025-04-19 11:30   | 2025-04-19 11:30   |
| 3   | Ravi Kumar        | ravi@gmail.com     | 981234567  | secure789 | 2025-04-18 12:00   | true      | 2025-04-18 12:00   | 2025-04-18 12:00   |

---

## Operators

The `operators` table stores information about bus operators. These operators manage the buses and trips.

| id  | name              | contact     | email               | created_by  | is_active | created_at         | updated_at         | logo_url            |
|-----|-------------------|-------------|---------------------|-------------|-----------|--------------------|--------------------|---------------------|
| 1   | Nepal Deluxe      | 9801111222  | deluxe@bus.com      | 1           | true      | 2025-04-20 09:00   | 2025-04-20 09:00   | NULL                |
| 2   | Mountain Express  | 9802555444  | mountain@express.com| 2           | true      | 2025-04-19 08:45   | 2025-04-19 08:45   | NULL                |
| 3   | City Travels      | 9803777888  | city@travels.com    | 1           | true      | 2025-04-18 07:30   | 2025-04-18 07:30   | NULL                |

---

## Buses

The `buses` table contains the information about buses used by operators, including bus details and seat count.

| id  | operator_id     | plate_number      | model     | total_seats| created_by | created_at         | updated_at         |
|-----|-----------------|-------------------|-----------|------------|------------|--------------------|--------------------|
| 1   | 1               | BA 1 JHA 123      | TATA Ace  | 40         | 1          | 2025-04-20 09:00   | 2025-04-20 09:00   |
| 2   | 2               | BA 2 KTU 456      | Mercedes  | 50         | 2          | 2025-04-19 08:45   | 2025-04-19 08:45   |
| 3   | 3               | BA 3 MNO 789      | Volvo     | 60         | 1          | 2025-04-18 07:30   | 2025-04-18 07:30   |

---

## Routes

The `routes` table stores information about bus routes, including start and end locations, distance, and estimated travel time.

| id  | start_location   | end_location   | distance_km| estimated_time | created_by  | created_at         | updated_at         |
|-----|------------------|----------------|------------|----------------|-------------|--------------------|--------------------|
| 1   | Kathmandu        | Pokhara         | 200.50     | 6 hours        | 1           | 2025-04-20 09:00   | 2025-04-20 09:00   |
| 2   | Kathmandu        | Chitwan         | 150.00     | 5 hours        | 2           | 2025-04-19 08:45   | 2025-04-19 08:45   |
| 3   | Pokhara          | Butwal          | 120.00     | 4 hours        | 1           | 2025-04-18 07:30   | 2025-04-18 07:30   |

---

## Trips

The `trips` table contains trip details, including the bus, route, departure and arrival times, price, available seats, and status (whether the trip is canceled or scheduled).

| id  | bus_id   | route_id | departure_time      | arrival_time        | price  | available_seats     | status            | cancellation_reason   | canceled_by    | created_by | created_at          | updated_at          |
|-----|----------|----------|---------------------|---------------------|--------|---------------------|-------------------|------------------------|----------------|------------|---------------------|---------------------|
| 1   | 1        | 1        | 2025-05-01 08:00    | 2025-05-01 14:00    | 1000   | 38                  | CANCELLED         | Landslide near Mugling | OPERATOR       | 1          | 2025-04-20 09:00   | 2025-04-20 09:00   |
| 2   | 2        | 2        | 2025-05-02 09:00    | 2025-05-02 14:00    | 900    | 50                  | SCHEDULED         | NULL                   | NULL           | 2          | 2025-04-19 08:45   | 2025-04-19 08:45   |
| 3   | 3        | 3        | 2025-05-03 10:00    | 2025-05-03 14:00    | 1200   | 60                  | SCHEDULED         | NULL                   | NULL           | 1          | 2025-04-18 07:30   | 2025-04-18 07:30   |

---

## Seats

The `seats` table stores the availability of seats for each trip. Each seat can be booked or left unbooked.

| id  | trip_id  | seat_number  | is_booked |
|-----|----------|--------------|-----------|
| 1   | 1        | A1           | true      |
| 2   | 1        | A2           | true      |
| 3   | 2        | B1           | false     |
| 4   | 2        | B2           | true      |

---

## Bookings

The `bookings` table stores user bookings for trips, including the status of the booking and cancellation reasons.

| id  | user_id | trip_id | seat_id | status    | cancellation_reason   | canceled_by | created_at         | updated_at         |
|-----|---------|---------|--------|-----------|-----------------------|-------------|--------------------|--------------------|
| 1   | 1       | 1       | 1      | CANCELLED | Landslide near Mugling | OPERATOR   | 2025-04-20 09:30   | 2025-04-20 09:31   |
| 2   | 2       | 2       | 2      | CONFIRMED | NULL                  | NULL        | 2025-04-20 10:00   | 2025-04-20 10:01   |
| 3   | 3       | 3       | 4      | CONFIRMED | NULL                  | NULL        | 2025-04-20 11:00   | 2025-04-20 11:01   |

---

## Payments

The `payments` table records user payments for bookings, including the method of payment and payment status.

| id  | booking_id | amount | method | payment_time        | status     | created_at         | updated_at         |
|-----|------------|--------|--------|---------------------|------------|--------------------|--------------------|
| 1   | 1          | 1000   | Credit | 2025-04-20 09:30    | FAILED     | 2025-04-20 09:30   | 2025-04-20 09:30   |
| 2   | 2          | 900    | Debit  | 2025-04-20 10:01    | SUCCESS    | 2025-04-20 10:01   | 2025-04-20 10:01   |
| 3   | 3          | 1200   | Cash   | 2025-04-20 11:01    | SUCCESS    | 2025-04-20 11:01   | 2025-04-20 11:01   |

---



## Data Flow

1. **Admins** manage the system by adding new **operators**, **buses**, and **routes**.
2. **Operators** provide bus services and schedule **trips**.
3. **Users** can view available **trips** and book **seats**.
4. Users can cancel bookings, and **operators** or **admins** can also cancel trips due to unforeseen events like landslides.
5. **Payments** are made through various methods and are linked to bookings.

---
#Process

1. **User Registration**: A user registers by providing their full name, email, and phone number. This data is stored in the `users` table.
2. **Search for Routes**: Users search for routes and view available trips based on their desired start and end locations.
3. **Booking a Seat**: Users select available seats for a trip, which are recorded in the `seats` table and booked in the `bookings` table.
4. **Payment**: After selecting a seat, users proceed to make payment. The payment status is recorded in the `payments` table.
5. **Trip Management**: Admins manage trips by assigning buses, routes, and operators. Each trip has a schedule and seat availability.
6. **Cancellation**: If a trip is canceled, the `trips` table status is updated, and bookings are marked as canceled in the `bookings` table.


## How to Use This Database

1. Set up the tables in your PostgreSQL or compatible database.
2. Add data to the tables using `INSERT` statements or through your application's UI.
3. Use SQL queries to retrieve data for the bus booking system (e.g., checking available trips, booking seats, viewing payment status).

Feel free to adapt this structure for your specific use case!

---

This simple design can be expanded with additional features like trip reviews, seat reservations, etc. The current system handles the core functionality of a bus reservation system.

