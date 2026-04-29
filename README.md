# CMS - Classroom Management System

A comprehensive educational management system with multi-role support (Teachers, Parents, Students, Admins) featuring face recognition authentication, attendance tracking, quiz management, and behavioral monitoring.

## Table of Contents
- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Configuration](#configuration)
- [Project Structure](#project-structure)
- [Usage](#usage)
- [API Endpoints](#api-endpoints)
- [Database](#database)
- [Authentication](#authentication)
- [Contributing](#contributing)
- [License](#license)

## Features

### 🔐 Multi-Role Authentication
- **Teacher Login**: Account creation and management
- **Parent Registration**: Account access for guardians
- **Student Face Login**: AI-powered facial recognition authentication
- **Admin Dashboard**: Administrative controls

### 📱 Attendance Management
- QR code generation and scanning
- Automated attendance marking
- Face recognition-based check-in
- Attendance calendar views
- Attendance records and summaries
- Out-of-time request system

### 🤖 Face Recognition
- Student face registration and enrollment
- Face vector descriptor storage and caching
- Facial recognition for secure student login
- Face cache management and reload functionality
- Real-time classroom snapshot with face detection

### 📚 Quiz & Grading System
- Quiz creation and publishing
- Question management and randomization
- Real-time quiz progress tracking
- Student quiz responses and submissions
- Automated grading and scoring
- Quiz leaderboards and rankings
- Quarter and final grade calculations
- Grade publishing and access control

### 📊 Student Management
- Student roster and enrollment tracking
- Classroom grouping and seating arrangements
- Student profile information
- Avatar management

### 📈 Behavior Tracking
- Behavior logging (in-class and kiosk)
- Behavior status monitoring
- Bulk behavior logging
- Student behavior analytics

### 👨‍👩‍👧‍👦 Parent Portal
- Parent login and registration
- Child profile access
- Attendance visibility
- Grade access
- Announcement viewing

### 🔔 Communications
- SMS notifications (configurable)
- Announcements for students and parents
- Real-time status updates

### 🎯 Additional Features
- School year management
- Leaderboard systems (quiz, behavioral, academic)
- Classroom lobbies and status
- Session management
- Action logging and debugging

## Requirements

- **PHP** >= 7.4
- **MySQL** >= 5.7
- **Apache/Nginx** web server with URL rewriting
- **Composer** (for dependency management)
- **OpenCV** or similar (for face recognition processing)

### PHP Extensions
- MySQLi
- JSON
- OpenSSL
- cURL (for SMS integration)

### Dependencies
- `phpoffice/phpspreadsheet` ^5.6 (for spreadsheet operations)

## Installation

### 1. Clone the Repository
```bash
git clone https://github.com/yourusername/cms.git
cd cms
```

### 2. Install Dependencies
```bash
composer install
```

### 3. Database Setup
- Create a MySQL database named `cms_copy` (or modify in config)
- Import the database schema (if provided)
- Update database credentials in `config/db.php`

### 4. Configure Web Server
Ensure your web server root points to the CMS directory and URL rewriting is enabled.

### 5. File Permissions
```bash
chmod 755 config/
chmod 755 config/student_faces/
chmod 755 config/student_avatars/
chmod 755 student_faces/
chmod 755 avatar/
chmod 755 config/logs/
```

### 6. Environment Setup
Update `config/db.php` with your database credentials:
```php
$host = 'localhost';
$db   = 'cms_copy';
$user = 'root';
$pass = '';
```

## Configuration

### Database Configuration
Edit `config/db.php`:
```php
$host = 'your_host';
$db = 'your_database';
$user = 'your_user';
$pass = 'your_password';
$charset = 'utf8mb4';
```

### SMS Configuration
Configure SMS settings in `config/sms.php` with your SMS provider credentials.

### Face Recognition Settings
- Face vector models are stored in `config/student_faces/`
- Ensure adequate storage for face descriptors
- Configure face cache reload intervals

## Project Structure

```
cms/
├── admin/                    # Admin dashboard and controls
├── api/                      # RESTful API endpoints
│   ├── attendance/
│   ├── grades/
│   ├── quiz/
│   ├── behavior/
│   └── parent/
├── config/                   # Configuration files and handlers
│   ├── db.php               # Database connection
│   ├── login.php            # Authentication logic
│   ├── student_faces/       # Face vector storage
│   └── student_avatars/     # Student profile images
├── models/                  # Data models
├── user/                    # User-specific pages
│   ├── teacher/
│   ├── parent/
│   └── student/
├── avatar/                  # Avatar storage
├── student_faces/           # Student face images
├── vendor/                  # Composer dependencies
├── index.php                # Login portal
└── composer.json            # Project dependencies
```

## Usage

### For Teachers
1. Login at the root URL with teacher credentials
2. Manage class roster and seating
3. Create and publish quizzes
4. Track student attendance
5. Monitor student behavior
6. View and publish grades

### For Students
1. Use face recognition to log in
2. Take assigned quizzes
3. View grades and attendance
4. Check announcements
5. Submit out-of-time requests

### For Parents
1. Register for a parent account
2. Login with credentials
3. View child's attendance and grades
4. Check announcements
5. Contact teachers (if enabled)

### For Administrators
1. Access admin dashboard
2. Manage all users (teachers, parents, students)
3. Configure system settings
4. View system logs
5. Manage school years and semesters

## API Endpoints

### Attendance
- `POST /api/mark_attendance.php` - Mark student attendance
- `GET /api/check_attendance.php` - Verify attendance status
- `GET /api/get_attendance_records.php` - Retrieve attendance history
- `GET /api/get_attendance_summary.php` - Get attendance summary

### Quizzes
- `POST /api/start_quiz.php` - Initiate quiz session
- `POST /api/submit_quiz_answer.php` - Submit quiz answer
- `GET /api/get_quiz_questions.php` - Retrieve quiz questions
- `GET /api/get_quiz_leaderboard.php` - Get quiz rankings

### Grades
- `POST /api/save_grades.php` - Save student grades
- `GET /api/get_grades.php` - Retrieve student grades
- `GET /api/get_final_grades.php` - Get final grades

### Behavior
- `POST /api/log_behavior.php` - Log student behavior
- `POST /api/log_behavior_bulk.php` - Bulk behavior logging
- `GET /api/get_behavior_status.php` - Get behavior status

### Face Recognition
- `POST /api/save_face_descriptor.php` - Save face vector
- `GET /api/list_faces_all.php` - List all registered faces
- `POST /api/register_face.php` - Register student face

### Students
- `GET /api/get_students.php` - Get student list
- `GET /api/classroom_roster.php` - Get class roster
- `POST /api/save_seating.php` - Update seating arrangement

## Database

### Key Tables (Expected Structure)
- `users` - All user accounts (teachers, parents, students, admins)
- `students` - Student information and enrollment
- `attendance` - Attendance records
- `quizzes` - Quiz definitions
- `quiz_questions` - Quiz questions
- `quiz_responses` - Student quiz answers
- `grades` - Grade records
- `behavior_logs` - Student behavior entries
- `face_descriptors` - Face vector data for recognition
- `announcements` - System announcements
- `school_years` - Academic calendar

## Authentication

### Session Management
- Sessions stored in PHP `$_SESSION`
- Guard files for role-based access control:
  - `config/student_guard.php` - Student access control
  - `config/teacher_guard.php` - Teacher access control
  - `config/admin_guard.php` - Admin access control

### Login Flow
1. User submits credentials
2. System validates in database
3. Session created with user role
4. Redirected to role-appropriate dashboard

### Face Recognition Login
1. Student triggers face login
2. Camera captures facial data
3. Face vectors compared against database
4. Automatic session creation on match

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## Support

For issues, feature requests, or questions, please open an issue on GitHub or contact the development team.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

---

**Last Updated**: April 2026

**Version**: 1.0.0
