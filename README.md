<div align="center">
  <h1>🏢 OSIM System</h1>
  <p>An Organizational Simulation and Internal Management (OSIM) system built in C++.</p>

  [![Language: C++](https://img.shields.io/badge/Language-C++-blue.svg)]()
  [![Build: Make](https://img.shields.io/badge/Build-Make-green.svg)]()
  [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
</div>

---

> [!NOTE]
> **Academic Project**
> This repository demonstrates a robust C++ programming project built under strict constraints. It implements complex software engineering concepts such as Role-Based Access Control (RBAC), hierarchical permissions, custom hashing, OTP generation, and persistent audit logging within a monolithic architecture.

## 📖 Table of Contents
- [About the Project](#about-the-project)
- [Key Features](#key-features)
- [Hierarchical Roles](#hierarchical-roles)
- [Getting Started](#getting-started)
- [Usage](#usage)
- [License](#license)

## 🕵️‍♂️ About the Project

The **Organizational Simulation and Internal Management (OSIM) System** is a terminal-based application designed to simulate the internal task delegation and auditing processes of a corporate environment. 

It heavily utilizes Object-Oriented Programming (OOP) concepts like Inheritance and Polymorphism to model corporate hierarchy, enforcing strict rules on who can delegate tasks to whom. It relies on standard C++ file I/O to maintain persistent databases of users, tasks, and audit logs.

## ✨ Key Features

- **Role-Based Authentication**: Custom registration and login system featuring a proprietary simple hash algorithm for password obfuscation.
- **OTP Verification**: Implements a One-Time Password generator required for high-level actions and logins.
- **Hierarchical Task Delegation**: Enforces a strict corporate hierarchy (e.g., a Manager can assign tasks to an Employee, but an Employee cannot assign tasks to a Director).
- **Task Lifecycle Management**: Track tasks through their entire lifecycle: Created, Assigned, In Progress, Completed, and Expired.
- **Comprehensive Audit Logging**: Automatically tracks and timestamps all major actions (logins, task delegations, status changes) to an `audit.txt` file for compliance checking.
- **Policy Engine Simulator**: A built-in class designed to validate if a user's role has the required permissions to execute specific actions.

## 👥 Hierarchical Roles

The system uses C++ inheritance to model the following ranks (from lowest to highest):

1. **Junior**
2. **Employee**
3. **Manager**
4. **Director**
5. **Executive**

*Rule of Delegation: A user can only delegate tasks to roles ranked strictly below them.*

## 🚀 Getting Started

### Prerequisites
You need a standard C++ compiler (like `g++`) installed on your machine.
- **Windows**: Install MinGW-w64.
- **Linux/Mac**: `sudo apt install build-essential` or install Xcode Command Line Tools.

### Build Instructions
Clone the repository and compile using the included `Makefile`:

```bash
git clone https://github.com/your-username/OSIM-System.git
cd OSIM-System
make
```

## 🎮 Usage

Run the compiled executable:

```bash
# On Linux/Mac:
./osim_system

# On Windows:
.\osim_system.exe
```

1. **First Run**: Select `1. Register` to create an `Executive` or `Director` account.
2. **Login**: Select `2. Login`, pass the OTP check, and access the system.
3. **Explore**: Create new employees, delegate tasks downward in the hierarchy, and review the generated `audit.txt` to see your footprint.

## 📝 License

Distributed under the MIT License. See `LICENSE` for more information.
