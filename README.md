# dealership_database
practice project for SQL(mysql) constraints/cascade


### Ticket Breakdown: Automotive Dealership Database Example

#### Ticket 1: Create the Database and Tables

**Tasks:**
1. Create a new database named `AutomotiveDB`.
   - **Hint:** Use the `CREATE DATABASE IF NOT EXISTS` statement.
2. Create the `Cars` table with the following columns:
   - `CarID`: INT, Primary key, Auto Increment
   - `CarModel`: VARCHAR(100), Not Null
   - `Year`: INT, Not Null
   - `Price`: DECIMAL(10, 2), Not Null
   - `Color`: ENUM('Red', 'Blue', 'Green', 'Black', 'White'), Not Null
3. Create the `Owners` table with the following columns:
   - `OwnerID`: INT, Primary key, Auto Increment
   - `OwnerName`: VARCHAR(100), Not Null
   - `OwnerAddress`: VARCHAR(255), Not Null
   - `OwnerPhone`: VARCHAR(20), Not Null
4. Create the `Services` table with the following columns:
   - `ServiceID`: INT, Primary key, Auto Increment
   - `ServiceName`: VARCHAR(100), Not Null
   - `ServiceDescription`: TEXT
   - `ServiceCost`: DECIMAL(10, 2), Not Null

#### Ticket 2: Create Relationships and Constraints
**Description:** Define relationships between tables and add constraints to enforce data integrity.

**Tasks:**
1. Create the `Ownerships` table with the following columns:
   - `OwnershipID`: INT, Primary key, Auto Increment
   - `CarID`: INT, Foreign key referencing `Cars(CarID)` with `ON DELETE CASCADE ON UPDATE CASCADE`
   - `OwnerID`: INT, Foreign key referencing `Owners(OwnerID)` with `ON DELETE CASCADE ON UPDATE CASCADE`
   - `PurchaseDate`: DATE, Not Null
2. Create the `CarServices` table with the following columns:
   - `CarID`: INT, Primary key (part of composite key), Foreign key referencing `Cars(CarID)` with `ON DELETE CASCADE ON UPDATE CASCADE`
   - `ServiceID`: INT, Primary key (part of composite key), Foreign key referencing `Services(ServiceID)` with `ON DELETE CASCADE ON UPDATE CASCADE`
   - `ServiceDate`: DATE, Not Null
3. Create the `Mechanics` table with the following columns:
   - `MechanicID`: INT, Primary key, Auto Increment
   - `MechanicName`: VARCHAR(100), Not Null
   - `PhoneNumber`: VARCHAR(20), Not Null
   - `HireDate`: TIMESTAMP, Default `CURRENT_TIMESTAMP`
4. Create the `Garages` table with the following columns:
   - `GarageID`: INT, Primary key, Auto Increment
   - `GarageName`: VARCHAR(100), Not Null, Unique
   - `Location`: VARCHAR(255), Not Null
5. Create the `CarMechanics` table with the following columns:
   - `CarID`: INT, Primary key (part of composite key), Foreign key referencing `Cars(CarID)` with `ON DELETE CASCADE ON UPDATE CASCADE`
   - `MechanicID`: INT, Primary key (part of composite key), Foreign key referencing `Mechanics(MechanicID)` with `ON DELETE CASCADE ON UPDATE CASCADE`
   - `ServiceDate`: DATE, Not Null

#### Ticket 3: Insert Sample Data
**Description:**
Insert sample data into the tables to illustrate relationships and constraints.

**Insert Sample Data (for testing):**
```sql
INSERT INTO Cars (CarModel, Year, Price, Color) VALUES
('Toyota Camry', 2020, 25000.00, 'Red'),
('Honda Accord', 2021, 27000.00, 'Blue'),
('Ford Focus', 2019, 22000.00, 'Green');

INSERT INTO Owners (OwnerName, OwnerAddress, OwnerPhone) VALUES
('John Doe', '123 Elm St', '555-1234'),
('Alice Johnson', '456 Oak St', '555-8765');

INSERT INTO Services (ServiceName, ServiceDescription, ServiceCost) VALUES
('Oil Change', 'Change the engine oil', 50.00),
('Tire Rotation', 'Rotate the tires', 30.00);

INSERT INTO Ownerships (CarID, OwnerID, PurchaseDate) VALUES
(1, 1, '2020-05-10'),
(2, 2, '2021-08-15');

INSERT INTO CarServices (CarID, ServiceID, ServiceDate) VALUES
(1, 1, '2021-01-10'),
(1, 2, '2021-06-15'),
(2, 1, '2021-02-20');

INSERT INTO Mechanics (MechanicName, PhoneNumber) VALUES
('Mike Smith', '555-6789'),
('Sarah Brown', '555-9876');

INSERT INTO Garages (GarageName, Location) VALUES
('Downtown Garage', '789 Maple St'),
('Uptown Garage', '101 Oak St');

INSERT INTO CarMechanics (CarID, MechanicID, ServiceDate) VALUES
(1, 1, '2021-01-10'),
(2, 2, '2021-08-15');
```

#### Ticket 4: Test Cascading Behavior
**Description:** Test cascading delete and update actions to ensure referential integrity is maintained.

**Tasks:**
1. Test cascading delete by deleting a car and checking related tables.
   - Delete a car: `DELETE FROM Cars WHERE CarID = 1`.
   - Check the state of `Ownerships` and `CarServices` tables to confirm cascading delete.
2. Test cascading update by updating a car ID and checking related tables.
   - Update CarID: `UPDATE Cars SET CarID = 100 WHERE CarID = 2`.
   - Check the state of `Ownerships` and `CarServices` tables to confirm cascading update.
3. Test cascading delete by deleting an owner and checking related tables.
   - Delete an owner: `DELETE FROM Owners WHERE OwnerID = 1`.
   - Check the state of `Ownerships` table to confirm cascading delete.
4. Test cascading delete by deleting a service and checking related tables.
   - Delete a service: `DELETE FROM Services WHERE ServiceID = 1`.
   - Check the state of `CarServices` table to confirm cascading delete.
5. Test cascading update by updating a service ID and checking related tables.
   - Update ServiceID: `UPDATE Services SET ServiceID = 101 WHERE ServiceID = 2`.
   - Check the state of `CarServices` table to confirm cascading update.
