# Secure Team-Based Collaborative Workflow Management with Encrypted Data Transmission and Multi-Client TCP Communication 

## Overview

**SecureCollab** is a team-based collaborative workflow management system designed to facilitate secure communication, task management, and collaboration in an organization. The system leverages encrypted data transmission over SSL, multi-client TCP communication, and role-based access control to ensure that tasks are efficiently assigned, tracked, and completed. The solution consists of a multi-threaded C backend server running on an Ubuntu-based WSL system and a Python-based GUI client running on a Windows machine, offering a seamless experience for secure team collaboration.

## Features

- **Secure Communication**: All communication between the Python client and the C server is secured using SSL, ensuring that sensitive data is encrypted and protected during transit.
- **Role-Based Access Control**: The system supports two roles — *admin* and *user*. Admins have full privileges to manage users and tasks, while regular users can only view tasks and update their status.
- **Multi-Client Support**: The C server handles multiple client connections simultaneously using multi-threading, ensuring that multiple users can collaborate and manage their tasks without any performance degradation.
- **Task Management**: Users can create tasks, view tasks, update task status, and admins can delete tasks. Tasks are assigned to specific users and have a due date, ensuring that workflow management is organized and transparent.
- **MySQL Database**: All user and task data is securely stored in a MySQL database, with the server interacting with the database to verify login credentials, store user and task data, and perform CRUD operations.

## Tech Stack

- **C**: Backend server written in C that handles communication, task management, and database operations.
- **Python**: Frontend GUI client built with the `customtkinter` library for an intuitive and user-friendly interface.
- **MySQL**: Used for securely storing user credentials, roles, and task data.
- **SSL/TCP**: Secured communication between the client and the server is achieved through SSL encryption over TCP sockets.
- **Multi-Threading**: The server is designed to handle multiple client requests concurrently, improving scalability and responsiveness.

## System Architecture

The architecture of the system consists of three main components:
1. **Server (C backend on Ubuntu WSL)**: The server listens for incoming TCP connections and handles requests such as user login, task creation, task viewing, and task completion. It uses SSL for secure communication and multi-threading to handle multiple clients simultaneously.
2. **Client (Python GUI on Windows)**: The client communicates with the server over a secure TCP connection. The user interface is built using `customtkinter`, providing a simple and effective way for users to interact with the system.
3. **MySQL Database**:
    - **MySQL** is used as the relational database to store user and task data.
    - The database contains two main tables: `users` and `tasks`.
      - **Users Table**: Stores user details such as usernames, passwords (hashed), and roles (admin/user).
      - **Tasks Table**: Stores tasks assigned to users, including the task title, description, status (pending/completed), and due dates.
        
## How It Works

1. **Login & Authentication**: Users log in using their credentials (username and password). The server authenticates the user against the MySQL database and grants access based on the role (admin or user).
2. **Role-Based Operations**:
   - *Admins* can create tasks, view all tasks, delete tasks, and manage users.
   - *Users* can only view their tasks and mark tasks as complete.
3. **Secure Communication**: The entire communication between the client and server is encrypted via SSL, ensuring that all data is protected from potential interception.
4. **Multi-Client Handling**: The server can handle multiple clients simultaneously, each interacting with their own set of tasks or performing actions like viewing and updating tasks.

## Features in Detail

- **Admin Dashboard**: Admins can view all users, create new users, delete users, create tasks for users, and view all tasks assigned to users.
- **User Dashboard**: Regular users can view their tasks and update their status, e.g., mark tasks as complete. Users can only see tasks assigned to them.
- **Task Creation & Management**: Tasks are assigned with a title, description, and due date. Admins can delete tasks if necessary. 
- **Secure Data Handling**: All sensitive data transmitted between the client and server is encrypted using SSL.

## Prerequisites

### Server Side (C backend on WSL/Ubuntu)
- WSL (Windows Subsystem for Linux) running Ubuntu (or any Linux-based system).
- OpenSSL for secure communication.
- MySQL for data storage.
- pthread for multi-threading support.

### Client Side (Python)
- Python 3.x
- `customtkinter` for the GUI.
- `ssl` module for secure communication.
- MySQL Python client for database interaction (if needed for client-side database operations).

## Installation

## SSL Certificate Setup

Since this project uses SSL for encrypted communication, you will need to generate SSL certificates for the server. If you don't have an existing certificate, follow these steps to create a self-signed certificate:

### Generating SSL Certificates

1. **Install OpenSSL** (if not already installed):

    On **Ubuntu** (WSL):
    ```bash
    sudo apt update
    sudo apt install openssl
    ```

2. **Generate a Self-Signed SSL Certificate**:

    Run the following commands to generate a self-signed SSL certificate and a private key:

    ```bash
    openssl genpkey -algorithm RSA -out server.key
    openssl req -new -key server.key -out server.csr
    openssl x509 -req -in server.csr -signkey server.key -out server.crt
    ```

    - `server.key`: The private key.
    - `server.crt`: The public certificate.

3. **Place the Certificates in the certs Folder**:

    After generating the certificate files (`server.crt` and `server.key`), place them in the dcerts folder



### Setting Up the Server (C backend on WSL)
1. Install **WSL** (Windows Subsystem for Linux) and set up an Ubuntu distribution.
2. Install **OpenSSL**, **MySQL**, and **pthread** on the Ubuntu WSL system.
    ```bash
    sudo apt update
    sudo apt install openssl libssl-dev mysql-server pthread
    ```
3. Clone the repository:
    ```bash
    git clone https://github.com/amrutha-m206/SecureCollab.git
    cd SecureCollab
    ```
4. Compile the C server code:
    ```bash
   gcc -o server main.c db_interface.c ssl_setup.c mysql_config --cflags --libs -lssl -lcrypto -lpthread
    ```
5. Start the server:
    ```bash
    ./server
    ```

### Setting Up the Client (Python GUI on Windows)
1. Install **Python 3.x** and **customtkinter**:
    ```bash
    pip install customtkinter
    ```
  
2. Run the Python client:
    ```bash
    python main.py
    ```

### Database Setup
1. Set up the MySQL database and tables. You can find the schema in the `init_db.sql` file.

## Running the Project

- Start the **C server** on your WSL/Ubuntu system using the provided server executable.
- Run the **Python client** on your Windows machine to interact with the server and perform operations like login, task management, etc.
  
