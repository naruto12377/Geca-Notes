# College Notes Admin Panel Installation Guide

## Overview
This admin panel system provides comprehensive management capabilities for your college notes website, including user management, notes management, category management, and system monitoring.

## Features

### Super Admin Features
- **Complete User Management**: Create, approve, reject, and delete all user accounts
- **Admin Management**: Create and manage admin accounts
- **Full Notes Control**: Manage all uploaded notes, activate/deactivate, delete
- **Category Management**: Add, edit, delete categories
- **System Monitoring**: Dashboard with statistics and recent activities

### Admin Features
- **User Management**: Approve/reject student and teacher accounts
- **Notes Management**: Manage uploaded notes, moderate content
- **Category Management**: Manage note categories
- **Dashboard**: View system statistics and activities

## Installation Steps

### 1. Database Setup
First, ensure your database has the required structure. Update your existing `college_notes.sql` file:

```sql
-- Add the super admin with the specified credentials
UPDATE users SET 
    username = 'superadmin', 
    password = '$2y$10$92IXUNpkjO0rOQ5byMi.Ye4oKoEa3Ro9llC/.og/at2.uheWG/igi',
    user_type = 'super_admin',
    status = 'approved'
WHERE id = 1;

-- Or insert if not exists
INSERT INTO users (username, email, password, full_name, user_type, status) VALUES
('superadmin', 'admin@college.edu', '$2y$10$92IXUNpkjO0rOQ5byMi.Ye4oKoEa3Ro9llC/.og/at2.uheWG/igi', 'Super Administrator', 'super_admin', 'approved')
ON DUPLICATE KEY UPDATE 
    password = '$2y$10$92IXUNpkjO0rOQ5byMi.Ye4oKoEa3Ro9llC/.og/at2.uheWG/igi',
    user_type = 'super_admin',
    status = 'approved';
```

### 2. File Structure
Place all the admin panel files in your project directory:

```
your-project/
├── config/
│   ├── config.php
│   └── database.php
├── admin_auth.php
├── admin_users.php
├── admin_notes.php
├── admin_categories.php
├── admin_dashboard.php
├── admin.php (main admin panel page)
├── auth.php (your existing auth file)
└── uploads/ (ensure this directory exists)
    ├── covers/
    └── notes/
```

### 3. Configuration Updates

Update your `config/config.php` to include any missing constants:

```php
<?php
// Existing config
define('BASE_URL', 'http://localhost/college-notes/');
define('UPLOAD_DIR', 'uploads/');
define('COVERS_DIR', 'uploads/covers/');
define('NOTES_DIR', 'uploads/notes/');

// Admin panel specific (if not already present)
define('ADMIN_SESSION_TIMEOUT', 3600); // 1 hour
define('MAX_LOGIN_ATTEMPTS', 5);

session_start();
?>
```

### 4. File Permissions
Ensure proper file permissions:

```bash
chmod 755 admin*.php
chmod 777 uploads/
chmod 777 uploads/covers/
chmod 777 uploads/notes/
```

### 5. Access the Admin Panel

#### Super Admin Login
- **URL**: `http://yourdomain.com/admin.php`
- **Username**: `superadmin`
- **Password**: `superadmin@12377`

#### Create Additional Admins
1. Login as Super Admin
2. Navigate to User Management
3. Click "Create Admin"
4. Fill in the admin details
5. The new admin can login with their credentials

## Default Login Credentials

### Super Admin
- **Username**: `superadmin`
- **Password**: `superadmin@12377`
- **Capabilities**: Complete system control

### Demo Admin (if created)
- **Username**: `admin`
- **Password**: `admin123`
- **Capabilities**: Limited admin features

## Security Features

1. **Session Management**: Secure admin sessions separate from user sessions
2. **Password Hashing**: All passwords are hashed using PHP's `password_hash()`
3. **SQL Injection Prevention**: All queries use prepared statements
4. **XSS Protection**: Output is properly escaped
5. **CSRF Protection**: Forms include security tokens (can be enhanced)

## File Descriptions

### Core Admin Files
- **`admin.php`**: Main admin panel interface
- **`admin_auth.php`**: Admin authentication handling
- **`admin_users.php`**: User management backend
- **`admin_notes.php`**: Notes management backend
- **`admin_categories.php`**: Category management backend
- **`admin_dashboard.php`**: Dashboard data provider

### Key Features Per File

#### admin_auth.php
- Admin login/logout
- Session management
- Permission checking
- Super admin verification

#### admin_users.php
- Get all users with pagination
- Approve/reject user accounts
- Delete users (with restrictions)
- Create admin accounts (Super Admin only)
- User statistics

#### admin_notes.php
- Get all notes with filtering
- Update note status (active/inactive)
- Delete notes with file cleanup
- Note statistics
- Search functionality

#### admin_categories.php
- CRUD operations for categories
- Category usage statistics
- Validation to prevent deletion of categories with notes

#### admin_dashboard.php
- System statistics
- Recent activities
- Popular content
- System information
- Chart data for visualizations

## Customization Options

### 1. Styling
The admin panel uses inline CSS for easy customization. Key areas to modify:
- **Colors**: Update the gradient and color variables
- **Layout**: Modify the sidebar width and responsive breakpoints
- **Typography**: Change font families and sizes

### 2. Functionality Extensions
- Add more user roles
- Implement detailed logging
- Add email notifications
- Create backup functionality
- Implement advanced search filters

### 3. Security Enhancements
- Add CSRF tokens to all forms
- Implement rate limiting
- Add IP whitelisting for admin access
- Two-factor authentication
- Password complexity requirements

## Troubleshooting

### Common Issues

1. **"Unauthorized" Errors**
   - Check if admin session is properly started
   - Verify user has admin/super_admin role
   - Clear browser cookies and try again

2. **Database Connection Errors**
   - Verify database credentials in `config/database.php`
   - Ensure database exists and is accessible
   - Check if required tables exist

3. **File Upload Issues**
   - Check directory permissions (uploads/, uploads/covers/, uploads/notes/)
   - Verify PHP upload settings (upload_max_filesize, post_max_size)
   - Ensure adequate disk space

4. **Login Issues**
   - Verify super admin credentials
   - Check if user exists in database with correct role
   - Clear browser cache/cookies

### Debug Mode
To enable debug mode, add this to the top of admin files:
```php
error_reporting(E_ALL);
ini_set('display_errors', 1);
```

## Performance Optimization

1. **Database Indexing**
   - Ensure indexes exist on frequently queried columns
   - Consider composite indexes for complex queries

2. **Caching**
   - Implement Redis or Memcached for dashboard statistics
   - Cache category lists and user counts

3. **File Management**
   - Implement file compression for uploads
   - Use CDN for static assets
   - Regular cleanup of unused files

## Backup and Maintenance

### Regular Backups
1. Database: `mysqldump -u username -p database_name > backup.sql`
2. Files: Regular backup of uploads directory
3. Configuration: Backup all PHP configuration files

### Maintenance Tasks
- Regular log file cleanup
- Database optimization
- Security updates
- Performance monitoring

## Support and Updates

For support or feature requests:
1. Check the troubleshooting section
2. Review error logs
3. Test with minimal configuration
4. Document steps to reproduce issues

## License and Credits

This admin panel system is designed specifically for the College Notes website system. Modify and distribute according to your needs.

---

**Note**: Remember to change default passwords and implement additional security measures for production use.
