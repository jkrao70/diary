You can add diary activities.

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Teacher's Diary</title>
    <!-- Include jsPDF library for PDF generation -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.28/jspdf.plugin.autotable.min.js"></script>
    <!-- Add Font Awesome for icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        /* Common Styles */
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f5f5f5;
            margin: 0;
            padding: 20px;
            color: #333;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }
        .hidden {
            display: none;
        }
        .text-center {
            text-align: center;
        }
        .text-link {
            color: #4a6fa5;
            cursor: pointer;
            text-decoration: underline;
        }
        .text-link:hover {
            color: #3a5a80;
        }

        /* Login Screen Styles */
        .login-container {
            width: 100%;
            max-width: 400px;
            background-color: white;
            border-radius: 10px;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
            overflow: hidden;
            transition: all 0.3s;
        }
        .login-header {
            background-color: #4a6fa5;
            color: white;
            padding: 20px;
            text-align: center;
            font-size: 24px;
            font-weight: bold;
        }
        .login-form {
            padding: 30px;
        }
        .form-group {
            margin-bottom: 20px;
        }
        .form-label {
            font-weight: bold;
            margin-bottom: 8px;
            display: block;
            color: #455a64;
        }
        .form-input {
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 4px;
            width: 100%;
            box-sizing: border-box;
            font-size: 16px;
        }
        .login-btn {
            background-color: #4a6fa5;
            color: white;
            border: none;
            padding: 14px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
            width: 100%;
            transition: background-color 0.3s;
            margin-top: 10px;
        }
        .login-btn:hover {
            background-color: #3a5a80;
        }
        .create-account-btn {
            background-color: #66bb6a;
            color: white;
            border: none;
            padding: 14px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
            width: 100%;
            transition: background-color 0.3s;
            margin-top: 10px;
        }
        .create-account-btn:hover {
            background-color: #4caf50;
        }

        /* Create Account Screen Styles */
        .create-account-container {
            width: 100%;
            max-width: 400px;
            background-color: white;
            border-radius: 10px;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
            overflow: hidden;
            transition: all 0.3s;
        }
        .create-account-header {
            background-color: #66bb6a;
            color: white;
            padding: 20px;
            text-align: center;
            font-size: 24px;
            font-weight: bold;
        }

        /* Diary Form Styles */
        .diary-container {
            width: 100%;
            max-width: 1000px;
            background-color: white;
            border-radius: 10px;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
            overflow: hidden;
        }
        .diary-header {
            background-color: #4a6fa5;
            color: white;
            padding: 20px;
            text-align: center;
            font-size: 24px;
            font-weight: bold;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .header-actions {
            display: flex;
            gap: 10px;
        }
        .logout-btn, .download-btn {
            padding: 8px 15px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 14px;
            border: none;
            transition: all 0.3s;
        }
        .logout-btn {
            background-color: #f44336;
            color: white;
        }
        .logout-btn:hover {
            background-color: #d32f2f;
        }
        .download-btn {
            background-color: #4CAF50;
            color: white;
        }
        .download-btn:hover {
            background-color: #388E3C;
        }
        .form-section {
            padding: 20px;
        }
        .form-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
            margin-bottom: 20px;
        }
        .form-group {
            display: flex;
            flex-direction: column;
            padding: 15px;
            border-radius: 8px;
        }
        .date-group {
            background-color: #e3f2fd;
            border-left: 4px solid #2196f3;
        }
        .class-group {
            background-color: #e8f5e9;
            border-left: 4px solid #4caf50;
        }
        .subject-group {
            background-color: #fff3e0;
            border-left: 4px solid #ff9800;
        }
        .period-group {
            background-color: #f3e5f5;
            border-left: 4px solid #9c27b0;
        }
        .medium-group {
            background-color: #e0f7fa;
            border-left: 4px solid #00bcd4;
        }
        .auth-group {
            background-color: #f5f5f5;
            border-left: 4px solid #607d8b;
            margin-bottom: 20px;
        }
        .date-label {
            color: #1976d2;
        }
        .class-label {
            color: #2e7d32;
        }
        .subject-label {
            color: #ef6c00;
        }
        .period-label {
            color: #7b1fa2;
        }
        .medium-label {
            color: #00838f;
        }
        .auth-label {
            color: #455a64;
        }
        select.form-input {
            background-color: white;
            appearance: none;
            background-image: url("data:image/svg+xml;charset=UTF-8,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='currentColor' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3e%3cpolyline points='6 9 12 15 18 9'%3e%3c/polyline%3e%3c/svg%3e");
            background-repeat: no-repeat;
            background-position: right 10px center;
            background-size: 15px;
        }
        .dropdown-options {
            font-size: 12px;
            color: #666;
            margin-top: 5px;
            font-style: italic;
        }
        .large-input {
            height: 100px;
            resize: vertical;
        }
        .button-row {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 15px;
            margin-top: 20px;
        }
        .btn {
            padding: 12px 20px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
            border: none;
            transition: all 0.3s;
            text-align: center;
        }
        .retrieve-btn {
            background-color: #5c6bc0;
            color: white;
        }
        .retrieve-btn:hover {
            background-color: #3949ab;
        }
        .delete-btn {
            background-color: #ef5350;
            color: white;
        }
        .delete-btn:hover {
            background-color: #d32f2f;
        }
        .save-btn {
            background-color: #4a6fa5;
            color: white;
        }
        .save-btn:hover {
            background-color: #3a5a80;
        }
        .topic-group {
            margin-bottom: 20px;
        }
        .text-area-group {
            margin-bottom: 15px;
        }
        
        /* Notification styles */
        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            padding: 15px 25px;
            border-radius: 4px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
            z-index: 1000;
            display: none;
            animation: slideIn 0.5s, fadeOut 0.5s 2.5s forwards;
        }
        
        .success-notification {
            background-color: #4CAF50;
            color: white;
        }
        
        .info-notification {
            background-color: #2196F3;
            color: white;
        }
        
        .warning-notification {
            background-color: #ff9800;
            color: white;
        }
        
        @keyframes slideIn {
            from {right: -300px; opacity: 0;}
            to {right: 20px; opacity: 1;}
        }
        
        @keyframes fadeOut {
            from {opacity: 1;}
            to {opacity: 0;}
        }
    </style>
</head>
<body>
    <!-- Notifications -->
    <div class="notification success-notification" id="successNotification">
        Saved successfully!
    </div>
    <div class="notification info-notification" id="retrieveNotification">
        Retrieved successfully!
    </div>
    <div class="notification warning-notification" id="deleteNotification">
        Deleted successfully!
    </div>

    <!-- Login Screen (shown first) -->
    <div class="login-container" id="loginScreen">
        <div class="login-header">
            AP TEACHER'S DIARY - LOGIN
        </div>
        
        <div class="login-form">
            <div class="form-group">
                <label class="form-label">USERNAME</label>
                <input type="text" class="form-input" placeholder="Enter your username" id="username">
            </div>

            <div class="form-group">
                <label class="form-label">PASSWORD</label>
                <input type="password" class="form-input" placeholder="Enter your password" id="password">
            </div>
            
            <button class="login-btn" id="loginButton">Login</button>
            <button class="create-account-btn" id="showCreateAccount">Create New Account</button>
            <p class="text-center" style="margin-top: 15px;">Don't have an account? <span class="text-link" id="showCreateAccountLink">Sign up here</span></p>
        </div>
    </div>

    <!-- Create Account Screen (hidden by default) -->
    <div class="create-account-container hidden" id="createAccountScreen">
        <div class="create-account-header">
            CREATE NEW ACCOUNT
        </div>
        
        <div class="login-form">
            <div class="form-group">
                <label class="form-label">FULL NAME</label>
                <input type="text" class="form-input" placeholder="Enter your full name" id="fullName">
            </div>
            
            <div class="form-group">
                <label class="form-label">EMAIL</label>
                <input type="email" class="form-input" placeholder="Enter your email" id="email">
            </div>

            <div class="form-group">
                <label class="form-label">USERNAME</label>
                <input type="text" class="form-input" placeholder="Choose a username" id="newUsername">
            </div>

            <div class="form-group">
                <label class="form-label">PASSWORD</label>
                <input type="password" class="form-input" placeholder="Create a password" id="newPassword">
            </div>
            
            <div class="form-group">
                <label class="form-label">CONFIRM PASSWORD</label>
                <input type="password" class="form-input" placeholder="Confirm your password" id="confirmPassword">
            </div>
            
            <button class="create-account-btn" id="createAccountButton">Create Account</button>
            <p class="text-center" style="margin-top: 15px;">Already have an account? <span class="text-link" id="showLoginLink">Login here</span></p>
        </div>
    </div>

    <!-- Diary Form (hidden until login) -->
    <div class="diary-container hidden" id="diaryForm">
        <div class="diary-header">
            <span>AP TEACHER'S DIARY</span>
            <div class="header-actions">
                <button class="download-btn" id="downloadPdf">
                    <i class="fas fa-download"></i> Download PDF
                </button>
                <button class="logout-btn" id="logoutButton">
                    <i class="fas fa-sign-out-alt"></i> Logout
                </button>
            </div>
        </div>
        
        <div class="form-section">
            <!-- Moved the auth group to the top -->
            <div class="form-group auth-group">
                <label class="form-label auth-label">LOGGED IN AS</label>
                <input type="text" class="form-input" id="diaryUsername" readonly>
            </div>

            <div class="form-grid">
                <div class="form-group date-group">
                    <label class="form-label date-label">DATE</label>
                    <input type="date" class="form-input" id="diaryDate">
                    <span class="dropdown-options">Select date from dropdown</span>
                </div>
                
                <div class="form-group class-group">
                    <label class="form-label class-label">CLASS</label>
                    <select class="form-input" id="diaryClass">
                        <option value="">Select Class</option>
                        <option value="1">1</option>
                        <option value="2">2</option>
                        <option value="3">3</option>
                        <option value="4">4</option>
                        <option value="5">5</option>
                        <option value="6">6</option>
                        <option value="7">7</option>
                        <option value="8">8</option>
                        <option value="9">9</option>
                        <option value="10">10</option>
                    </select>
                    <span class="dropdown-options">1 TO 10</span>
                </div>
                
                <div class="form-group subject-group">
                    <label class="form-label subject-label">SUBJECT</label>
                    <select class="form-input" id="diarySubject">
                        <option value="">Select Subject</option>
                        <option value="telugu">Telugu</option>
                        <option value="hindi">Hindi</option>
                        <option value="english">English</option>
                        <option value="maths">Maths</option>
                        <option value="physics">Physics</option>
                        <option value="biology">Biology</option>
                        <option value="social">Social</option>
                    </select>
                </div>
                
                <div class="form-group period-group">
                    <label class="form-label period-label">PERIOD NO</label>
                    <select class="form-input" id="diaryPeriod">
                        <option value="">Select Period</option>
                        <option value="1">1</option>
                        <option value="2">2</option>
                        <option value="3">3</option>
                        <option value="4">4</option>
                        <option value="5">5</option>
                        <option value="6">6</option>
                        <option value="7">7</option>
                        <option value="8">8</option>
                    </select>
                    <span class="dropdown-options">1 TO 8</span>
                </div>
                
                <div class="form-group medium-group">
                    <label class="form-label medium-label">MEDIUM</label>
                    <select class="form-input" id="diaryMedium">
                        <option value="">Select Medium</option>
                        <option value="telugu">Telugu</option>
                        <option value="english">English</option>
                    </select>
                </div>
            </div>
            
            <div class="form-group topic-group">
                <label class="form-label">TOPIC/EVENT</label>
                <input type="text" class="form-input" id="diaryTopic">
            </div>
            
            <div class="form-group text-area-group">
                <label class="form-label">OBSERVATIONS & CHALLENGES</label>
                <textarea class="form-input large-input" id="diaryObservations"></textarea>
            </div>
            
            <div class="form-group text-area-group">
                <label class="form-label">PLAN OF ACTION FOR NEXT CLASS</label>
                <textarea class="form-input large-input" id="diaryActionPlan"></textarea>
            </div>
            
            <div class="form-group text-area-group">
                <label class="form-label">REMARKS</label>
                <textarea class="form-input large-input" id="diaryRemarks"></textarea>
            </div>

            <div class="button-row">
                <button class="btn retrieve-btn" id="retrieveButton">Retrieve</button>
                <button class="btn delete-btn" id="deleteButton">Delete</button>
                <button class="btn save-btn" id="saveButton">Save</button>
            </div>
        </div>
    </div>

    <script>
        // Initialize jsPDF
        const { jsPDF } = window.jspdf;
        
        // Simple data storage (in a real app, this would be a database)
        let diaryEntries = [];
        let currentUser = null;
        
        // Login and Create Account toggle
        document.getElementById('showCreateAccount').addEventListener('click', showCreateAccount);
        document.getElementById('showCreateAccountLink').addEventListener('click', showCreateAccount);
        document.getElementById('showLoginLink').addEventListener('click', showLogin);

        function showCreateAccount() {
            document.getElementById('loginScreen').classList.add('hidden');
            document.getElementById('createAccountScreen').classList.remove('hidden');
        }

        function showLogin() {
            document.getElementById('createAccountScreen').classList.add('hidden');
            document.getElementById('loginScreen').classList.remove('hidden');
        }

        // Login functionality
        document.getElementById('loginButton').addEventListener('click', function() {
            const username = document.getElementById('username').value;
            const password = document.getElementById('password').value;
            
            if(username && password) {
                // Set current user
                currentUser = username;
                
                // Hide login, show diary form
                document.getElementById('loginScreen').classList.add('hidden');
                document.getElementById('diaryForm').classList.remove('hidden');
                
                // Display username in diary form
                document.getElementById('diaryUsername').value = username;
                
                // Clear all fields
                clearForm();
            } else {
                alert('Please enter both username and password');
            }
        });

        // Create account functionality
        document.getElementById('createAccountButton').addEventListener('click', function() {
            const fullName = document.getElementById('fullName').value;
            const email = document.getElementById('email').value;
            const username = document.getElementById('newUsername').value;
            const password = document.getElementById('newPassword').value;
            const confirmPassword = document.getElementById('confirmPassword').value;
            
            if(!fullName || !email || !username || !password || !confirmPassword) {
                alert('Please fill in all fields');
                return;
            }
            
            if(password !== confirmPassword) {
                alert('Passwords do not match');
                return;
            }
            
            // In a real app, you would send this data to your backend
            alert('Account created successfully! Please login with your new credentials.');
            
            // Switch back to login screen
            showLogin();
            
            // Pre-fill the username field
            document.getElementById('username').value = username;
            
            // Clear create account form
            document.getElementById('fullName').value = '';
            document.getElementById('email').value = '';
            document.getElementById('newUsername').value = '';
            document.getElementById('newPassword').value = '';
            document.getElementById('confirmPassword').value = '';
        });

        // Save button functionality
        document.getElementById('saveButton').addEventListener('click', function() {
            if (!currentUser) return;
            
            // Get all form values
            const date = document.getElementById('diaryDate').value;
            const classVal = document.getElementById('diaryClass').value;
            const subject = document.getElementById('diarySubject').value;
            const period = document.getElementById('diaryPeriod').value;
            const medium = document.getElementById('diaryMedium').value;
            const topic = document.getElementById('diaryTopic').value;
            const observations = document.getElementById('diaryObservations').value;
            const actionPlan = document.getElementById('diaryActionPlan').value;
            const remarks = document.getElementById('diaryRemarks').value;
            
            // Simple validation
            if (!date || !classVal || !subject || !period || !medium || !topic) {
                alert('Please fill in all required fields (Date, Class, Subject, Period, Medium, Topic)');
                return;
            }
            
            // Check if entry already exists
            const existingIndex = diaryEntries.findIndex(entry => 
                entry.date === date && 
                entry.class === classVal &&
                entry.subject === subject &&
                entry.medium === medium &&
                entry.username === currentUser
            );
            
            // Create entry object
            const entry = {
                date,
                class: classVal,
                subject,
                period,
                medium,
                topic,
                observations,
                actionPlan,
                remarks,
                username: currentUser
            };
            
            if (existingIndex !== -1) {
                // Update existing entry
                diaryEntries[existingIndex] = entry;
            } else {
                // Add new entry
                diaryEntries.push(entry);
            }
            
            // Show success notification
            showNotification('successNotification');
            
            // Clear the form
            clearForm();
        });

        // Retrieve button functionality - UPDATED to include Subject and Medium
        document.getElementById('retrieveButton').addEventListener('click', function() {
            if (!currentUser) return;
            
            const date = document.getElementById('diaryDate').value;
            const classVal = document.getElementById('diaryClass').value;
            const subject = document.getElementById('diarySubject').value;
            const medium = document.getElementById('diaryMedium').value;
            
            if (!date || !classVal || !subject || !medium) {
                alert('Please enter Date, Class, Subject and Medium to retrieve');
                return;
            }
            
            // Find matching entries with all criteria
            const matchingEntries = diaryEntries.filter(entry => 
                entry.date === date && 
                entry.class === classVal &&
                entry.subject === subject &&
                entry.medium === medium &&
                entry.username === currentUser
            );
            
            if (matchingEntries.length === 0) {
                alert('No entries found for the selected criteria');
                return;
            }
            
            // For simplicity, we'll just use the first matching entry
            const entry = matchingEntries[0];
            
            // Fill the form with retrieved data
            document.getElementById('diaryDate').value = entry.date;
            document.getElementById('diaryClass').value = entry.class;
            document.getElementById('diarySubject').value = entry.subject;
            document.getElementById('diaryPeriod').value = entry.period;
            document.getElementById('diaryMedium').value = entry.medium;
            document.getElementById('diaryTopic').value = entry.topic;
            document.getElementById('diaryObservations').value = entry.observations;
            document.getElementById('diaryActionPlan').value = entry.actionPlan;
            document.getElementById('diaryRemarks').value = entry.remarks;
            
            // Show retrieve notification
            showNotification('retrieveNotification');
        });

        // Delete button functionality
        document.getElementById('deleteButton').addEventListener('click', function() {
            if (!currentUser) return;
            
            const date = document.getElementById('diaryDate').value;
            const classVal = document.getElementById('diaryClass').value;
            const subject = document.getElementById('diarySubject').value;
            const medium = document.getElementById('diaryMedium').value;
            
            if (!date || !classVal || !subject || !medium) {
                alert('Please enter Date, Class, Subject and Medium to delete');
                return;
            }
            
            if (!confirm('Are you sure you want to delete this entry?')) {
                return;
            }
            
            // Find index of matching entry
            const index = diaryEntries.findIndex(entry => 
                entry.date === date && 
                entry.class === classVal &&
                entry.subject === subject &&
                entry.medium === medium &&
                entry.username === currentUser
            );
            
            if (index === -1) {
                alert('No entries found for the selected criteria');
                return;
            }
            
            // Remove the entry
            diaryEntries.splice(index, 1);
            
            // Show delete notification
            showNotification('deleteNotification');
            
            // Clear the form
            clearForm();
        });

        // Logout functionality
        document.getElementById('logoutButton').addEventListener('click', function() {
            currentUser = null;
            document.getElementById('diaryForm').classList.add('hidden');
            document.getElementById('loginScreen').classList.remove('hidden');
            clearForm();
        });

        // PDF Download functionality
        document.getElementById('downloadPdf').addEventListener('click', function() {
            if (!currentUser) return;
            
            const date = document.getElementById('diaryDate').value;
            const classVal = document.getElementById('diaryClass').value;
            const subject = document.getElementById('diarySubject').value;
            const medium = document.getElementById('diaryMedium').value;
            
            // Filter entries based on current view or all entries
            let entriesToExport = [];
            if (date && classVal && subject && medium) {
                // Export current entry if form is filled
                entriesToExport = [{
                    date,
                    class: classVal,
                    subject,
                    period: document.getElementById('diaryPeriod').value,
                    medium,
                    topic: document.getElementById('diaryTopic').value,
                    observations: document.getElementById('diaryObservations').value,
                    actionPlan: document.getElementById('diaryActionPlan').value,
                    remarks: document.getElementById('diaryRemarks').value
                }];
            } else {
                // Export all entries for current user
                entriesToExport = diaryEntries.filter(entry => entry.username === currentUser);
            }
            
            if (entriesToExport.length === 0) {
                alert('No entries to export');
                return;
            }
            
            // Create PDF
            const doc = new jsPDF();
            
            // Add title
            doc.setFontSize(18);
            doc.text('Teacher Diary Entries', 105, 15, { align: 'center' });
            
            // Add user info
            doc.setFontSize(12);
            doc.text(`Teacher: ${currentUser}`, 14, 25);
            doc.text(`Generated on: ${new Date().toLocaleDateString()}`, 14, 32);
            
            // Prepare data for table
            const tableData = entriesToExport.map(entry => [
                entry.date,
                entry.class,
                entry.subject,
                entry.period,
                entry.medium,
                entry.topic || '-',
                entry.observations || '-',
                entry.actionPlan || '-',
                entry.remarks || '-'
            ]);
            
            // Add table
            doc.autoTable({
                startY: 40,
                head: [['Date', 'Class', 'Subject', 'Period', 'Medium', 'Topic', 'Observations', 'Action Plan', 'Remarks']],
                body: tableData,
                styles: { fontSize: 8 },
                columnStyles: {
                    0: { cellWidth: 15 },
                    1: { cellWidth: 10 },
                    2: { cellWidth: 15 },
                    3: { cellWidth: 10 },
                    4: { cellWidth: 15 },
                    5: { cellWidth: 20 },
                    6: { cellWidth: 30 },
                    7: { cellWidth: 30 },
                    8: { cellWidth: 30 }
                },
                margin: { left: 10 }
            });
            
            // Save PDF
            doc.save(`Teacher_Diary_${currentUser}_${new Date().toISOString().slice(0,10)}.pdf`);
        });

        // Function to show notification
        function showNotification(id) {
            const notification = document.getElementById(id);
            notification.style.display = 'block';
            
            // Hide notification after animation completes
            setTimeout(function() {
                notification.style.display = 'none';
            }, 3000);
        }

        // Function to clear all form fields
        function clearForm() {
            document.getElementById('diaryDate').value = '';
            document.getElementById('diaryClass').value = '';
            document.getElementById('diarySubject').value = '';
            document.getElementById('diaryPeriod').value = '';
            document.getElementById('diaryMedium').value = '';
            document.getElementById('diaryTopic').value = '';
            document.getElementById('diaryObservations').value = '';
            document.getElementById('diaryActionPlan').value = '';
            document.getElementById('diaryRemarks').value = '';
        }
    </script>
</body>
</html>
