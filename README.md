# IOT-Logger -- Edge-to-MySQL Real-Time Data Logging
IoT Logger is an edge-based data capture system that collects telemetry from sensors and industrial machines, buffers the data locally on a Windows-based or single-board gateway, and stores structured records in a MySQL database for later analysis. The design emphasizes reliability (local persistence during network outages), portability (runs on an IoT gateway or a laptop), and standard connectivity (ODBC + DSN for database access).

# What this post contains
High-level architecture & features
Exact tools I used 
Reproducible implementation notes so others can build a similar pipeline on a laptop or gateway

# Key features
Device integration: captures live telemetry from sensors/controllers over Ethernet or Wi-Fi.
Local buffering: gateway stores data locally to tolerate intermittent connectivity.
Structured storage: records saved in MySQL for long-term analysis and export (CSV).
Extensible: integrates later with dashboards, APIs, or analytics engines.
Portable: works on a Windows gateway, Raspberry Pi, or a personal laptop.

# Tools & components
Hardware
Sensors/controllers (depends on project)
Edge gateway: Windows-based PC or single-board computer (e.g., Raspberry Pi)
Network: Ethernet/Wi-Fi

Software
MySQL Server — relational storage (runs on gateway or remote server)
MySQL Workbench — for schema design, querying, and administration (recommended)
ODBC (Data Source Name / DSN) — standardized connectivity layer between applications and MySQL
Lightweight application code on the gateway (any language/runtime that can use ODBC — e.g., Python, C#, Node.js with appropriate ODBC/DB drivers)
Optional: export to CSV and visualization tools 

High-level architecture
Sensors → Gateway
Sensors push or are polled by the gateway through standard interfaces (serial, Modbus, HTTP, MQTT, etc.). The gateway handles any necessary preprocessing (units conversion, validation, timestamp stamping).

Local buffer / store
The gateway temporarily stores incoming records and attempts to write them into the MySQL server. Local persistence ensures no data loss when remote connectivity is unavailable.

ODBC / DSN → MySQL
The gateway application uses an ODBC DSN to connect to the MySQL server and perform structured inserts/updates. The DSN decouples code from connection strings and simplifies redeployment.

Querying & export
Data is queried using MySQL Workbench. Exports (CSV) and downstream analytics are performed as needed.

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

# Implementation notes (reproducible guidance)
1) MySQL & Workbench
Install MySQL Server locally (or on gateway).
Use MySQL Workbench to create schema, design tables, and run queries.
Create a user account for the gateway application with minimum necessary privileges (INSERT/SELECT/UPDATE as required). Do not use the server root account in production.

2) ODBC DSN
Create a DSN (system or user) that points to the MySQL instance.
The DSN stores server host, port (default 3306), and the driver to use. Applications reference the DSN name instead of embedding full connection strings.

3) Gateway application (general pattern)
Language-agnostic pattern — the gateway app must:
Read and validate incoming sensor data.
Format data to match the database schema.
Connect via ODBC DSN and perform parameterized inserts/updates.
Retry or queue on failure, and flush the queue when connectivity returns.
Provide logging and minimal local monitoring.

4) Local testing on a laptop
You can run the same stack on your laptop:
Install MySQL locally, set up a DSN referencing localhost.
Run the gateway app locally (it will behave the same way as on a gateway device).
Use MySQL Workbench for inspection and exports.

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
