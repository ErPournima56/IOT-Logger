# IOT-Logger
An IoT gateway was used to bridge hardware devices with the central MySQL database. This gateway acted as both:
 * A local controller, receiving data from sensors or controllers via Ethernet/Wi-Fi.
 * A database client, handling database operations through a Visual Basic application using ODBC connections.
The gateway provided local data persistence, enabling analysis and logging even during temporary network disruptions.

# Database Schema
The MySQL database, named meter, included a primary data table results to store measurement records.

# Data Access and Processing
Software was developed to automate interaction with the MySQL database using ADODB
The system connects through an ODBC Data Source Name (DSN) — meter — and performs CRUD (Create, Read, Update, Delete) operations programmatically. 

# Functionality Summary
 * Establishes a secure ODBC connection to MySQL.
 * Fetches the table cup_results.
 * Inserts a new record dynamically by iterating through field indices.
 * Provides feedback upon successful data insertion.
 
# Features
- **Device Integration** – capture data from hardware/machines  
- **MySQL Storage** – store records in structured tables via ODBC  
- **Real-Time Logging** – process incoming data as it arrives  
- **Extensible** – integrate with dashboards, APIs, or analytics tools  
- **Reliable** – designed for continuous data collection  

# Tech Stack
- **MySQL Server** – core database storage  
- **ODBC** – connection layer for MySQL  
- **Custom GUI Application** – manages communication with devices  

# Use Cases
- Industrial machine telemetry  
- IoT sensor data collection  
- Smart factory monitoring  
- Research data logging  

# Outcome
The system successfully logged and visualized metallurgical data in real time.
The use of MySQL + ODBC created a reliable data-handling pipeline.
This setup proved suitable for small-to-medium industrial setups where edge computing and local storage are essential.

This method demonstrates a direct way to perform data logging, monitoring, and automation control in an IoT-enabled environment using simple, lightweight software tools.
