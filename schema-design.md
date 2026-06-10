# Smart Clinic Management System - Database Schema Design

## 1. Entity-Relationship (ER) Overview
* **Users & Roles:** Managed via a unified `users` table with a `role` attribute (`'Admin'`, `'Doctor'`, `'Patient'`).
* **Doctors & Patients:** Specialized profiles linked back to the core `users` credentials.
* **Appointments:** Bridges `patients` and `doctors` to schedule consultations.
* **Medical Records:** Connects an `appointment` to the respective patient's health and prescription history.

---

## 2. Table Definitions (MySQL)

### `users` Table
Stores authentication and basic profile data for all system users.
| Column Name | Data Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | INT | PRIMARY KEY, AUTO_INCREMENT | Unique identifier for the user |
| `email` | VARCHAR(100) | UNIQUE, NOT NULL | Login email |
| `password_hash` | VARCHAR(255) | NOT NULL | Securely hashed password |
| `first_name` | VARCHAR(50) | NOT NULL | User's first name |
| `last_name` | VARCHAR(50) | NOT NULL | User's last name |
| `role` | ENUM('Admin', 'Doctor', 'Patient') | NOT NULL | System access level |
| `created_at` | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP | Record creation time |

### `doctors` Table
Contains specialized information unique to medical staff.
| Column Name | Data Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | INT | PRIMARY KEY, AUTO_INCREMENT | Unique doctor ID |
| `user_id` | INT | FOREIGN KEY REFERENCES `users(id)` | Links to core user profile |
| `specialization` | VARCHAR(100) | NOT NULL | Area of medical expertise |
| `phone` | VARCHAR(20) | NOT NULL | Contact number |

### `patients` Table
Contains specialized information unique to patients.
| Column Name | Data Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | INT | PRIMARY KEY, AUTO_INCREMENT | Unique patient ID |
| `user_id` | INT | FOREIGN KEY REFERENCES `users(id)` | Links to core user profile |
| `date_of_birth` | DATE | NOT NULL | Patient's birthdate |
| `gender` | ENUM('Male', 'Female', 'Other') | NOT NULL | Gender identity |
| `phone` | VARCHAR(20) | NOT NULL | Contact number |

### `appointments` Table
Tracks bookings between patients and doctors.
| Column Name | Data Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | INT | PRIMARY KEY, AUTO_INCREMENT | Unique appointment ID |
| `patient_id` | INT | FOREIGN KEY REFERENCES `patients(id)` | The patient booking |
| `doctor_id` | INT | FOREIGN KEY REFERENCES `doctors(id)` | The assigned doctor |
| `appointment_date` | DATETIME | NOT NULL | Scheduled date and time |
| `status` | ENUM('Scheduled', 'Completed', 'Cancelled') | DEFAULT 'Scheduled' | Current status |

### `medical_records` Table
Stores health data generated during a completed appointment.
| Column Name | Data Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | INT | PRIMARY KEY, AUTO_INCREMENT | Unique record ID |
| `appointment_id` | INT | FOREIGN KEY REFERENCES `appointments(id)` | Associated appointment |
| `patient_id` | INT | FOREIGN KEY REFERENCES `patients(id)` | Associated patient |
| `diagnosis` | TEXT | NOT NULL | Medical assessment findings |
| `prescription` | TEXT | Optional | Prescribed drugs/treatments |
| `updated_at` | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP | Last update time |
