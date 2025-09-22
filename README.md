<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>TrustDocs</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"/>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
  <style>
    *{margin:0;padding:0;box-sizing:border-box;font-family:'Segoe UI',Tahoma,Geneva,Verdana,sans-serif}
    body{background:linear-gradient(135deg,#f5f7fa 0%,#e4e8f0 100%);color:#333;line-height:1.6;min-height:100vh;padding:20px}
    .container{max-width:1200px;margin:0 auto}
    header{text-align:center;margin-bottom:30px;padding:30px;background:linear-gradient(135deg,#2c3e50,#1a2530);color:white;border-radius:15px;box-shadow:0 10px 30px rgba(0,0,0,.15)}
    h1{font-size:2.5rem;margin-bottom:10px;font-weight:700}
    .subtitle{font-size:1.1rem;opacity:.9;max-width:600px;margin:0 auto}
    .role-selection{display:flex;justify-content:center;flex-wrap:wrap;gap:30px;margin-top:40px}
    .role-card{background:white;border-radius:15px;box-shadow:0 5px 20px rgba(0,0,0,.1);padding:30px;width:280px;text-align:center;transition:.3s;cursor:pointer}
    .role-card:hover{transform:translateY(-5px);box-shadow:0 15px 30px rgba(0,0,0,.15)}
    .role-icon{font-size:70px;margin-bottom:20px;height:100px;display:flex;align-items:center;justify-content:center}
    .student-icon{color:#3498db}.issuer-icon{color:#2ecc71}.verifier-icon{color:#e74c3c}
    .role-title{font-size:1.5rem;margin-bottom:15px;color:#2c3e50}
    .role-description{color:#7f8c8d;margin-bottom:20px;min-height:80px}
    .btn{display:inline-block;padding:12px 25px;color:white;border:none;border-radius:8px;font-size:1rem;font-weight:600;cursor:pointer;text-decoration:none}
    .student-btn{background:#3498db}.student-btn:hover{background:#2980b9}
    .issuer-btn{background:#2ecc71}.issuer-btn:hover{background:#27ae60}
    .verifier-btn{background:#e74c3c}.verifier-btn:hover{background:#c0392b}
    .card{background:white;border-radius:15px;box-shadow:0 5px 20px rgba(0,0,0,.1);padding:30px;margin-bottom:30px}
    .section-title{font-size:1.5rem;margin-bottom:25px;color:#2c3e50;padding-bottom:15px;border-bottom:2px solid #eaecef;display:flex;align-items:center}
    .section-title i{margin-right:12px;font-size:1.8rem;color:#3498db}
    .form-row{display:flex;flex-wrap:wrap;gap:25px;margin-bottom:25px}
    .form-group{flex:1;min-width:250px}
    .form-group label{display:block;margin-bottom:8px;font-weight:600;color:#2c3e50}
    .form-control{width:100%;padding:14px;border:2px solid #e0e3e8;border-radius:8px;font-size:1rem;transition:.3s}
    .form-control:focus{border-color:#3498db;box-shadow:0 0 0 3px rgba(52,152,219,.2);outline:none}
    .drop-area{border:3px dashed #3498db;border-radius:10px;padding:40px 20px;text-align:center;cursor:pointer;margin-bottom:25px;background:#f8fafc;transition:.3s}
    .drop-area:hover{background:#f0f9ff;border-color:#2980b9}
    .drop-area i{font-size:60px;color:#3498db;margin-bottom:20px}
    .drop-text{font-size:1.3rem;margin-bottom:20px;color:#2c3e50}
    .file-input{display:none}
    .browse-btn{background:linear-gradient(to right,#3498db,#2980b9);color:white;padding:14px 30px;border:none;border-radius:8px;cursor:pointer;font-size:1.1rem;font-weight:600;box-shadow:0 4px 10px rgba(52,152,219,.3)}
    .file-info{margin-top:20px;padding:20px;background:#f8f9fa;border-radius:10px;display:none}
    .file-name{font-weight:bold;margin-bottom:8px;font-size:1.1rem}
    footer{text-align:center;padding:20px 0;color:#7f8c8d;font-size:.9rem;margin-top:40px}
    .popup-overlay{position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.5);display:none;justify-content:center;align-items:center;z-index:1000}
    .popup-message{background:white;border-radius:15px;padding:30px;max-width:400px;text-align:center;box-shadow:0 10px 30px rgba(0,0,0,0.2);animation:fadeIn 0.3s ease-in-out}
    .popup-message p{font-size:1.2rem;color:#2c3e50;margin-bottom:20px}
    .popup-message .btn{background:#3498db;padding:10px 20px;font-size:1rem}
    .popup-message .btn:hover{background:#2980b9}
    @keyframes fadeIn{from{opacity:0;transform:scale(0.8)}to{opacity:1;transform:scale(1)}}
    table {width: 100%; border-collapse: collapse; margin-top: 20px;}
    th, td {padding: 12px; border: 1px solid #ddd; text-align: left;}
    th {background-color: #f2f2f2;}
    .action-btn {padding: 8px 12px; margin: 0 5px; border: none; border-radius: 5px; cursor: pointer; color: white;}
    .view-btn {background: #3498db;}
    .valid-btn {background: #2ecc71;}
    .invalid-btn {background: #e74c3c;}
    .status-icon.valid {color: green; font-size: 1.2rem;}
    .status-icon.invalid {color: red; font-size: 1.2rem;}
    #qrCanvas {display: none;}
    @media(max-width:768px){.form-row{flex-direction:column;gap:15px}.role-selection{flex-direction:column;align-items:center}.role-card{width:100%;max-width:350px}}
  </style>
</head>
<body>
<div class="container">

  <!-- POPUP MESSAGE -->
  <div class="popup-overlay" id="popupOverlay">
    <div class="popup-message" id="popupMessage">
      <p id="popupText"></p>
      <button class="btn" id="popupOkButton" onclick="closePopup()">OK</button>
    </div>
  </div>

  <!-- HEADER -->
  <header>
    <h1>TrustDocs</h1>
    <p class="subtitle">Blockchain-based verification system with AI validation</p>
  </header>

  <!-- ROLE SELECTION -->
  <div id="rolePage">
    <div class="role-selection">
      <div class="role-card" data-action="student">
        <div class="role-icon"><i class="fas fa-user-graduate student-icon"></i></div>
        <h2 class="role-title">Student</h2>
        <p class="role-description">Submit documents for verification and track status</p>
        <div class="btn student-btn">Enter as Student</div>
      </div>
      <div class="role-card" data-action="issuer">
        <div class="role-icon"><i class="fas fa-university issuer-icon"></i></div>
        <h2 class="role-title">Issuer</h2>
        <p class="role-description">Issue and manage digital documents with blockchain security</p>
        <div class="btn issuer-btn">Enter as Issuer</div>
      </div>
      <div class="role-card" data-action="verifier">
        <div class="role-icon"><i class="fas fa-search verifier-icon"></i></div>
        <h2 class="role-title">Verifier</h2>
        <p class="role-description">Validate document authenticity using AI verification</p>
        <div class="btn verifier-btn">Enter as Verifier</div>
      </div>
    </div>
  </div>

  <!-- STUDENT AUTH SELECTION -->
  <div id="studentAuthPage" style="display:none;">
    <header>
      <button onclick="goBackToRoles()" style="float:left;background:#2c3e50;color:white;border:none;padding:8px 18px;border-radius:6px;cursor:pointer;">← Back</button>
      <h1>Student Authentication</h1>
      <p class="subtitle">Choose to register or sign in</p>
    </header>
    <div class="role-selection">
      <div class="role-card" data-action="student-register">
        <div class="role-icon"><i class="fas fa-user-plus student-icon"></i></div>
        <h2 class="role-title">Register New User</h2>
        <p class="role-description">Create a new account to get started</p>
        <div class="btn student-btn">Register</div>
      </div>
      <div class="role-card" data-action="student-login">
        <div class="role-icon"><i class="fas fa-sign-in-alt student-icon"></i></div>
        <h2 class="role-title">Sign In</h2>
        <p class="role-description">Log in with your existing account</p>
        <div class="btn student-btn">Sign In</div>
      </div>
    </div>
  </div>

  <!-- STUDENT REGISTRATION -->
  <div id="studentRegisterPage" style="display:none;">
    <header>
      <button onclick="goBackToAuth()" style="float:left;background:#2c3e50;color:white;border:none;padding:8px 18px;border-radius:6px;cursor:pointer;">← Back</button>
      <h1>Student Registration</h1>
      <p class="subtitle">Create your account</p>
    </header>
    <div class="card">
      <h2 class="section-title"><i class="fas fa-user-plus"></i> Registration Form</h2>
      <div class="form-row">
        <div class="form-group">
          <label for="regUsername">Username *</label>
          <input type="text" id="regUsername" class="form-control" placeholder="Enter username" required/>
        </div>
        <div class="form-group">
          <label for="regEmail">Email Address *</label>
          <input type="email" id="regEmail" class="form-control" placeholder="Enter your email" required/>
        </div>
      </div>
      <div class="form-row">
        <div class="form-group">
          <label for="regPassword">Password *</label>
          <input type="password" id="regPassword" class="form-control" placeholder="Enter password" required/>
        </div>
        <div class="form-group">
          <label for="regConfirmPassword">Confirm Password *</label>
          <input type="password" id="regConfirmPassword" class="form-control" placeholder="Confirm password" required/>
        </div>
      </div>
      <div style="text-align:center; margin-bottom:30px;">
        <button class="btn student-btn" onclick="registerStudent()"><i class="fas fa-user-plus"></i> Create Account</button>
      </div>
    </div>
  </div>

  <!-- STUDENT LOGIN -->
  <div id="studentLoginPage" style="display:none;">
    <header>
      <button onclick="goBackToAuth()" style="float:left;background:#2c3e50;color:white;border:none;padding:8px 18px;border-radius:6px;cursor:pointer;">← Back</button>
      <h1>Student Login</h1>
      <p class="subtitle">Sign in to your account</p>
    </header>
    <div class="card">
      <h2 class="section-title"><i class="fas fa-sign-in-alt"></i> Login Form</h2>
      <div class="form-row">
        <div class="form-group">
          <label for="loginUsername">Username *</label>
          <input type="text" id="loginUsername" class="form-control" placeholder="Enter username" required/>
        </div>
        <div class="form-group">
          <label for="loginPassword">Password *</label>
          <input type="password" id="loginPassword" class="form-control" placeholder="Enter password" required/>
        </div>
      </div>
      <div style="text-align:center; margin-bottom:30px;">
        <button class="btn student-btn" onclick="loginStudent()"><i class="fas fa-sign-in-alt"></i> Sign In</button>
      </div>
    </div>
  </div>

  <!-- STUDENT PORTAL -->
  <div id="studentPage" style="display:none;">
    <header>
      <button onclick="goBackToRoles()" style="float:left;background:#2c3e50;color:white;border:none;padding:8px 18px;border-radius:6px;cursor:pointer;">← Back</button>
      <button onclick="logoutStudent()" style="float:right;background:#e74c3c;color:white;border:none;padding:8px 18px;border-radius:6px;cursor:pointer;">Logout</button>
      <h1>Student Portal</h1>
      <p class="subtitle">Upload your documents for verification</p>
    </header>
    <div class="card">
      <h2 class="section-title"><i class="fas fa-user-circle"></i> Student Information</h2>
      <div class="form-row">
        <div class="form-group">
          <label for="fullName">Full Name *</label>
          <input type="text" id="fullName" class="form-control" placeholder="Enter your full name" required/>
        </div>
        <div class="form-group">
          <label for="email">Email Address *</label>
          <input type="email" id="email" class="form-control" placeholder="Enter your email" required/>
        </div>
        <div class="form-group">
          <label for="institution">Institution *</label>
          <select id="institution" class="form-control" required>
            <option value="">Select Institution</option>
            <option>Vellore Institute of Technology (All Branches)</option>
            <option>IIT Madras</option>
            <option>IIT Bombay</option>
            <option>Anna University</option>
            <option>Delhi University</option>
            <option>Jadavpur University</option>
            <option>Amrita Vishwa Vidyapeetham</option>
            <option>SRM Institute of Science and Technology</option>
            <option>Christ University</option>
            <option>Manipal Academy of Higher Education</option>
          </select>
        </div>
      </div>
      <div class="form-row">
        <div class="form-group">
          <label for="documentType">Document Type *</label>
          <select id="documentType" class="form-control" required>
            <option value="">Select document type</option>
            <option value="degree">Degree Certificate</option>
            <option value="diploma">Diploma</option>
            <option value="transcript">Academic Transcript</option>
            <option value="license">Professional License</option>
            <option value="certificate">Training Certificate</option>
            <option value="other">Other Document</option>
          </select>
        </div>
      </div>
    </div>
    <div class="card">
      <h2 class="section-title"><i class="fas fa-file-upload"></i> Upload Document</h2>
      <div class="drop-area" id="studentDrop">
        <i class="fas fa-cloud-upload-alt"></i>
        <p class="drop-text">Drag & Drop or Click to Upload</p>
        <input type="file" id="studentFile" class="file-input" accept=".pdf,.jpg,.png,.jpeg"/>
        <button type="button" class="browse-btn" onclick="document.getElementById('studentFile').click()">Browse File</button>
        <div class="file-info" id="studentFileInfo"><p class="file-name"></p></div>
      </div>
    </div>
    <!-- Upload Button -->
    <div style="text-align:center; margin-bottom:30px;">
      <button class="btn student-btn" onclick="uploadDocument()"><i class="fas fa-upload"></i> Upload</button>
    </div>
    <!-- Student Documents Status -->
    <div class="card">
      <h2 class="section-title"><i class="fas fa-list"></i> My Documents Status</h2>
      <table id="studentDocumentsTable">
        <thead>
          <tr>
            <th>Serial</th>
            <th>Document Type</th>
            <th>Name</th>
            <th>Status</th>
            <th>Verification</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody></tbody>
      </table>
    </div>
  </div>
  <!-- ISSUER AUTH SELECTION -->
  <div id="issuerAuthPage" style="display:none;">
    <header>
      <button onclick="goBackToRoles()" style="float:left;background:#2c3e50;color:white;border:none;padding:8px 18px;border-radius:6px;cursor:pointer;">← Back</button>
      <h1>Issuer Authentication</h1>
      <p class="subtitle">Choose to register or sign in</p>
    </header>
    <div class="role-selection">
      <div class="role-card" data-action="issuer-register">
        <div class="role-icon"><i class="fas fa-user-plus issuer-icon"></i></div>
        <h2 class="role-title">Register New User</h2>
        <p class="role-description">Create a new account to get started</p>
        <div class="btn issuer-btn">Register</div>
      </div>
      <div class="role-card" data-action="issuer-login">
        <div class="role-icon"><i class="fas fa-sign-in-alt issuer-icon"></i></div>
        <h2 class="role-title">Sign In</h2>
        <p class="role-description">Log in with your existing account</p>
        <div class="btn issuer-btn">Sign In</div>
      </div>
    </div>
  </div>

  <!-- ISSUER REGISTRATION -->
  <div id="issuerRegisterPage" style="display:none;">
    <header>
      <button onclick="goBackToIssuerAuth()" style="float:left;background:#2c3e50;color:white;border:none;padding:8px 18px;border-radius:6px;cursor:pointer;">← Back</button>
      <h1>Issuer Registration</h1>
      <p class="subtitle">Create your account</p>
    </header>
    <div class="card">
      <h2 class="section-title"><i class="fas fa-user-plus"></i> Registration Form</h2>
      <div class="form-row">
        <div class="form-group">
          <label for="issuerRegUsername">Username *</label>
          <input type="text" id="issuerRegUsername" class="form-control" placeholder="Enter username" required/>
        </div>
        <div class="form-group">
          <label for="issuerRegEmail">Email Address *</label>
          <input type="email" id="issuerRegEmail" class="form-control" placeholder="Enter your email" required/>
        </div>
      </div>
      <div class="form-row">
        <div class="form-group">
          <label for="issuerRegPassword">Password *</label>
          <input type="password" id="issuerRegPassword" class="form-control" placeholder="Enter password" required/>
        </div>
        <div class="form-group">
          <label for="issuerRegConfirmPassword">Confirm Password *</label>
          <input type="password" id="issuerRegConfirmPassword" class="form-control" placeholder="Confirm password" required/>
        </div>
      </div>
      <div class="form-row">
        <div class="form-group">
          <label for="issuerRegInstitution">Institution *</label>
          <select id="issuerRegInstitution" class="form-control" required>
            <option value="">Select Institution</option>
            <option>Vellore Institute of Technology (All Branches)</option>
            <option>IIT Madras</option>
            <option>IIT Bombay</option>
            <option>Anna University</option>
            <option>Delhi University</option>
            <option>Jadavpur University</option>
            <option>Amrita Vishwa Vidyapeetham</option>
            <option>SRM Institute of Science and Technology</option>
            <option>Christ University</option>
            <option>Manipal Academy of Higher Education</option>
          </select>
        </div>
      </div>
      <div style="text-align:center; margin-bottom:30px;">
        <button class="btn issuer-btn" onclick="registerIssuer()"><i class="fas fa-user-plus"></i> Create Account</button>
      </div>
    </div>
  </div>

  <!-- ISSUER LOGIN -->
  <div id="issuerLoginPage" style="display:none;">
    <header>
      <button onclick="goBackToIssuerAuth()" style="float:left;background:#2c3e50;color:white;border:none;padding:8px 18px;border-radius:6px;cursor:pointer;">← Back</button>
      <h1>Issuer Login</h1>
      <p class="subtitle">Sign in to your account</p>
    </header>
    <div class="card">
      <h2 class="section-title"><i class="fas fa-sign-in-alt"></i> Login Form</h2>
      <div class="form-row">
        <div class="form-group">
          <label for="issuerLoginUsername">Username *</label>
          <input type="text" id="issuerLoginUsername" class="form-control" placeholder="Enter username" required/>
        </div>
        <div class="form-group">
          <label for="issuerLoginPassword">Password *</label>
          <input type="password" id="issuerLoginPassword" class="form-control" placeholder="Enter password" required/>
        </div>
      </div>
      <div style="text-align:center; margin-bottom:30px;">
        <button class="btn issuer-btn" onclick="loginIssuer()"><i class="fas fa-sign-in-alt"></i> Sign In</button>
      </div>
    </div>
  </div>

  <!-- ISSUER DASHBOARD -->
  <div id="issuerDashboardPage" style="display:none;">
    <header>
      <button onclick="goBackToRoles()" style="float:left;background:#2c3e50;color:white;border:none;padding:8px 18px;border-radius:6px;cursor:pointer;">← Back</button>
      <button onclick="logoutIssuer()" style="float:right;background:#e74c3c;color:white;border:none;padding:8px 18px;border-radius:6px;cursor:pointer;">Logout</button>
      <h1>Issuer Dashboard</h1>
      <p class="subtitle">Choose action: Issue or Verify documents</p>
    </header>
    <div class="role-selection">
      <div class="role-card" data-action="issuer-issue">
        <div class="role-icon"><i class="fas fa-file-signature issuer-icon"></i></div>
        <h2 class="role-title">Issue Document</h2>
        <p class="role-description">Issue new digital documents</p>
        <div class="btn issuer-btn">Issue</div>
      </div>
      <div class="role-card" data-action="issuer-verify">
        <div class="role-icon"><i class="fas fa-check-double issuer-icon"></i></div>
        <h2 class="role-title">Verify Documents</h2>
        <p class="role-description">Review and validate pending documents</p>
        <div class="btn issuer-btn">Verify</div>
      </div>
    </div>
  </div>

  <!-- ISSUER ISSUE PORTAL -->
  <div id="issuerIssuePage" style="display:none;">
    <header>
      <button onclick="goBackToIssuerDashboard()" style="float:left;background:#2c3e50;color:white;border:none;padding:8px 18px;border-radius:6px;cursor:pointer;">← Back</button>
      <button onclick="logoutIssuer()" style="float:right;background:#e74c3c;color:white;border:none;padding:8px 18px;border-radius:6px;cursor:pointer;">Logout</button>
      <h1>Issuer Issue Portal</h1>
      <p class="subtitle">Issue and manage digital documents with blockchain security</p>
    </header>
    <div class="card">
      <h2 class="section-title"><i class="fas fa-user-shield"></i> Issuer Information</h2>
      <div class="form-row">
        <div class="form-group">
          <label for="issuerName">Issuer Name *</label>
          <input type="text" id="issuerName" class="form-control" placeholder="Enter issuer full name" required>
        </div>
        <div class="form-group">
          <label for="issuerEmail">Issuer Email *</label>
          <input type="email" id="issuerEmail" class="form-control" placeholder="Enter issuer email" required>
        </div>
        <div class="form-group">
          <label for="issuerInstitution">Institution *</label>
          <select id="issuerInstitution" class="form-control" required>
            <option value="">Select Institution</option>
            <option>Vellore Institute of Technology (All Branches)</option>
            <option>IIT Madras</option>
            <option>IIT Bombay</option>
            <option>Anna University</option>
            <option>Delhi University</option>
            <option>Jadavpur University</option>
            <option>Amrita Vishwa Vidyapeetham</option>
            <option>SRM Institute of Science and Technology</option>
            <option>Christ University</option>
            <option>Manipal Academy of Higher Education</option>
          </select>
        </div>
      </div>
      <div class="form-row">
        <div class="form-group">
          <label for="issuerDocumentType">Document Type *</label>
          <select id="issuerDocumentType" class="form-control" required>
            <option value="">Select document type</option>
            <option value="degree">Degree Certificate</option>
            <option value="diploma">Diploma</option>
            <option value="transcript">Academic Transcript</option>
            <option value="license">Professional License</option>
            <option value="certificate">Training Certificate</option>
            <option value="other">Other Document</option>
          </select>
        </div>
      </div>
    </div>
    <div class="card">
      <h2 class="section-title"><i class="fas fa-file-upload"></i> Upload Issued Document</h2>
      <div class="drop-area" id="issuerDrop">
        <i class="fas fa-cloud-upload-alt"></i>
        <p class="drop-text">Drag & Drop or Click to Upload</p>
        <input type="file" id="issuerFile" class="file-input" accept=".pdf,.jpg,.png,.jpeg"/>
        <button type="button" class="browse-btn" onclick="document.getElementById('issuerFile').click()">Browse File</button>
        <div class="file-info" id="issuerFileInfo"><p class="file-name"></p></div>
      </div>
    </div>
    <!-- Issue Button -->
    <div style="text-align:center; margin-bottom:30px;">
      <button class="btn issuer-btn" onclick="issueDocument()"><i class="fas fa-file-signature"></i> Issue Document</button>
    </div>
  </div>

  <!-- ISSUER VERIFY PORTAL -->
  <div id="issuerVerifyPage" style="display:none;">
    <header>
      <button onclick="goBackToIssuerDashboard()" style="float:left;background:#2c3e50;color:white;border:none;padding:8px 18px;border-radius:6px;cursor:pointer;">← Back</button>
      <button onclick="logoutIssuer()" style="float:right;background:#e74c3c;color:white;border:none;padding:8px 18px;border-radius:6px;cursor:pointer;">Logout</button>
      <h1>Issuer Verify Portal</h1>
      <p class="subtitle">Review and validate pending student documents</p>
    </header>
    <div class="card">
      <h2 class="section-title"><i class="fas fa-check-double"></i> Pending Documents</h2>
      <table id="pendingDocumentsTable">
        <thead>
          <tr>
            <th>Serial</th>
            <th>Student Username</th>
            <th>Document Type</th>
            <th>Name</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody></tbody>
      </table>
    </div>
  </div>

  <!-- VERIFIER PORTAL -->
  <div id="verifierPage" style="display:none;">
    <header>
      <button onclick="goBackToRoles()" style="float:left;background:#2c3e50;color:white;border:none;padding:8px 18px;border-radius:6px;cursor:pointer;">← Back</button>
      <h1>Verifier Portal</h1>
      <p class="subtitle">Validate document authenticity</p>
    </header>
    <div class="card">
      <h2 class="section-title"><i class="fas fa-search"></i> Upload Document</h2>
      <div class="drop-area" id="verifierDrop">
        <i class="fas fa-cloud-upload-alt"></i>
        <p class="drop-text">Drag & Drop or Click to Upload</p>
        <input type="file" id="verifierFile" class="file-input" accept=".pdf,.jpg,.png,.jpeg"/>
        <button type="button" class="browse-btn" onclick="document.getElementById('verifierFile').click()">Browse File</button>
        <div class="file-info" id="verifierFileInfo"><p class="file-name"></p></div>
      </div>
    </div>
   <!-- Verification Button -->
    <div style="text-align:center; margin-bottom:30px;">
      <button class="btn verifier-btn" onclick="verifyDocument('Verifier')"><i class="fas fa-check-circle"></i> Verify Document</button>
    </div>
  </div>

  <!-- Hidden canvas for QR code generation -->
  <canvas id="qrCanvas"></canvas>

  <footer>
    <p>Secure Document Verification System © 2025 | Project Exhibition</p>
  </footer>
</div>

<script>
  // Global file storage to avoid localStorage quota issues
  let fileStorage = {};

  // Initialize event listeners for role cards
  document.addEventListener('DOMContentLoaded', () => {
    const roleCards = document.querySelectorAll('.role-card');
    roleCards.forEach(card => {
      card.addEventListener('click', (e) => {
        const action = card.getAttribute('data-action');
        switch (action) {
          case 'student':
            showStudentAuth();
            break;
          case 'issuer':
            showIssuerAuth();
            break;
          case 'verifier':
            showVerifierForm();
            break;
          case 'student-register':
            showStudentRegister();
            break;
          case 'student-login':
            showStudentLogin();
            break;
          case 'issuer-register':
            showIssuerRegister();
            break;
          case 'issuer-login':
            showIssuerLogin();
            break;
          case 'issuer-issue':
            showIssuerIssue();
            break;
          case 'issuer-verify':
            showIssuerVerify();
            break;
          default:
            console.error('Unknown action:', action);
        }
      });
    });

    // Setup drag and drop for student file input
    const studentDropArea = document.getElementById('studentDrop');
    if (studentDropArea) {
      ['dragenter', 'dragover', 'dragleave', 'drop'].forEach(eventName => {
        studentDropArea.addEventListener(eventName, preventDefaults, false);
      });
      ['dragenter', 'dragover'].forEach(eventName => {
        studentDropArea.addEventListener(eventName, () => studentDropArea.classList.add('highlight'), false);
      });
      ['dragleave', 'drop'].forEach(eventName => {
        studentDropArea.addEventListener(eventName, () => studentDropArea.classList.remove('highlight'), false);
      });
      studentDropArea.addEventListener('drop', handleStudentDrop, false);
    }

    // Setup drag and drop for issuer file input
    const issuerDropArea = document.getElementById('issuerDrop');
    if (issuerDropArea) {
      ['dragenter', 'dragover', 'dragleave', 'drop'].forEach(eventName => {
        issuerDropArea.addEventListener(eventName, preventDefaults, false);
      });
      ['dragenter', 'dragover'].forEach(eventName => {
        issuerDropArea.addEventListener(eventName, () => issuerDropArea.classList.add('highlight'), false);
      });
      ['dragleave', 'drop'].forEach(eventName => {
        issuerDropArea.addEventListener(eventName, () => issuerDropArea.classList.remove('highlight'), false);
      });
      issuerDropArea.addEventListener('drop', handleIssuerDrop, false);
    }

    // Setup drag and drop for verifier file input
    const verifierDropArea = document.getElementById('verifierDrop');
    if (verifierDropArea) {
      ['dragenter', 'dragover', 'dragleave', 'drop'].forEach(eventName => {
        verifierDropArea.addEventListener(eventName, preventDefaults, false);
      });
      ['dragenter', 'dragover'].forEach(eventName => {
        verifierDropArea.addEventListener(eventName, () => verifierDropArea.classList.add('highlight'), false);
      });
      ['dragleave', 'drop'].forEach(eventName => {
        verifierDropArea.addEventListener(eventName, () => verifierDropArea.classList.remove('highlight'), false);
      });
      verifierDropArea.addEventListener('drop', handleVerifierDrop, false);
    }
  });

  function preventDefaults(e) {
    e.preventDefault();
    e.stopPropagation();
  }

  function handleStudentDrop(e) {
    const dt = e.dataTransfer;
    const files = dt.files;
    document.getElementById('studentFile').files = files;
    setupFileInput('studentFile', 'studentFileInfo');
  }

  function handleIssuerDrop(e) {
    const dt = e.dataTransfer;
    const files = dt.files;
    document.getElementById('issuerFile').files = files;
    setupFileInput('issuerFile', 'issuerFileInfo');
  }

  function handleVerifierDrop(e) {
    const dt = e.dataTransfer;
    const files = dt.files;
    document.getElementById('verifierFile').files = files;
    setupFileInput('verifierFile', 'verifierFileInfo');
  }

  function hideAllPages() {
    const pages = [
      'rolePage', 'studentAuthPage', 'studentRegisterPage', 'studentLoginPage',
      'studentPage', 'issuerAuthPage', 'issuerRegisterPage', 'issuerLoginPage',
      'issuerDashboardPage', 'issuerIssuePage', 'issuerVerifyPage', 'verifierPage'
    ];
    pages.forEach(pageId => {
      const page = document.getElementById(pageId);
      if (page) page.style.display = 'none';
    });
  }

  function showStudentAuth() {
    hideAllPages();
    const page = document.getElementById('studentAuthPage');
    if (page) page.style.display = 'block';
  }

  function showStudentRegister() {
    hideAllPages();
    const page = document.getElementById('studentRegisterPage');
    if (page) page.style.display = 'block';
  }

  function showStudentLogin() {
    hideAllPages();
    const page = document.getElementById('studentLoginPage');
    if (page) page.style.display = 'block';
  }

  function showStudentForm() {
    hideAllPages();
    const page = document.getElementById('studentPage');
    if (page) page.style.display = 'block';
    loadStudentDocuments();
  }

  function showIssuerAuth() {
    hideAllPages();
    const page = document.getElementById('issuerAuthPage');
    if (page) page.style.display = 'block';
  }

  function showIssuerRegister() {
    hideAllPages();
    const page = document.getElementById('issuerRegisterPage');
    if (page) page.style.display = 'block';
  }

  function showIssuerLogin() {
    hideAllPages();
    const page = document.getElementById('issuerLoginPage');
    if (page) page.style.display = 'block';
  }

  function showIssuerDashboard() {
    hideAllPages();
    const page = document.getElementById('issuerDashboardPage');
    if (page) page.style.display = 'block';
  }

  function showIssuerIssue() {
    hideAllPages();
    const page = document.getElementById('issuerIssuePage');
    if (page) page.style.display = 'block';
  }

  function showIssuerVerify() {
    hideAllPages();
    const page = document.getElementById('issuerVerifyPage');
    if (page) page.style.display = 'block';
    loadPendingDocuments();
  }

  function showVerifierForm() {
    hideAllPages();
    const page = document.getElementById('verifierPage');
    if (page) page.style.display = 'block';
  }

  function goBackToRoles() {
    hideAllPages();
    const page = document.getElementById('rolePage');
    if (page) page.style.display = 'block';
  }

  function goBackToAuth() {
    hideAllPages();
    const page = document.getElementById('studentAuthPage');
    if (page) page.style.display = 'block';
  }

  function goBackToIssuerAuth() {
    hideAllPages();
    const page = document.getElementById('issuerAuthPage');
    if (page) page.style.display = 'block';
  }

  function goBackToIssuerDashboard() {
    hideAllPages();
    const page = document.getElementById('issuerDashboardPage');
    if (page) page.style.display = 'block';
  }

  function logoutStudent() {
    localStorage.removeItem('currentStudent');
    showPopup("Logged out successfully.");
    setTimeout(goBackToRoles, 1000);
  }

  function logoutIssuer() {
    localStorage.removeItem('currentIssuer');
    showPopup("Logged out successfully.");
    setTimeout(goBackToRoles, 1000);
  }

  // Show selected file name
  function setupFileInput(inputId, infoId) {
    const fileInput = document.getElementById(inputId);
    const fileInfo = document.getElementById(infoId);
    if (fileInput && fileInfo) {
      fileInput.addEventListener("change", () => {
        if (fileInput.files.length > 0) {
          fileInfo.style.display = "block";
          fileInfo.querySelector(".file-name").textContent = "Selected: " + fileInput.files[0].name;
        } else {
          fileInfo.style.display = "none";
        }
      });
      fileInput.dispatchEvent(new Event('change'));
    } else {
      console.error(File input or info element not found: ${inputId}, ${infoId});
    }
  }

  setupFileInput("studentFile", "studentFileInfo");
  setupFileInput("issuerFile", "issuerFileInfo");
  setupFileInput("verifierFile", "verifierFileInfo");

  // Popup message functions
  function showPopup(message, showOkButton = true) {
    const popupOverlay = document.getElementById("popupOverlay");
    const popupText = document.getElementById("popupText");
    const popupOkButton = document.getElementById("popupOkButton");
    if (popupOverlay && popupText && popupOkButton) {
      popupText.textContent = message;
      popupOkButton.style.display = showOkButton ? "inline-block" : "none";
      popupOverlay.style.display = "flex";
    } else {
      console.error('Popup elements not found');
    }
  }

  function closePopup() {
    const popupOverlay = document.getElementById("popupOverlay");
    if (popupOverlay) {
      popupOverlay.style.display = "none";
    }
  }

  // Generate QR code
  function generateQRCode(docId, status) {
    const qrCanvas = document.getElementById('qrCanvas');
    qrCanvas.innerHTML = ''; // Clear previous QR code
    const qrData = JSON.stringify({ docId, status });
    const qrCode = new QRCode(qrCanvas, {
      text: qrData,
      width: 100,
      height: 100,
      colorDark: "#000000",
      colorLight: "#ffffff",
      correctLevel: QRCode.CorrectLevel.H
    });
    return qrCanvas.toDataURL('image/png');
  }

  // Show document with QR code and optional watermark for students
  function showDocumentWithQR(doc, isStudent = false) {
    const fileData = fileStorage[doc.id];
    if (!fileData) {
      showPopup("File data not found.");
      return;
    }

    const qrDataURL = generateQRCode(doc.id, doc.status);

    if (doc.fileMime.startsWith('image/')) {
      const img = new Image();
      img.src = URL.createObjectURL(fileData);
      img.onload = function() {
        const canvas = document.createElement('canvas');
        canvas.width = img.width;
        canvas.height = img.height + (isStudent ? 120 : 0); // Extra space for QR code
        const ctx = canvas.getContext('2d');
        ctx.drawImage(img, 0, 0);
        
        if (isStudent) {
          // Add QR code
          const qrImg = new Image();
          qrImg.src = qrDataURL;
          qrImg.onload = function() {
            ctx.drawImage(qrImg, 20, img.height + 10, 100, 100);
            // Add watermark for valid documents
            if (doc.status === 'valid') {
              ctx.font = 'bold 40px Arial';
              ctx.fillStyle = 'rgba(0, 255, 0, 0.5)';
              ctx.fillText('TrustDocs Verified', 20, img.height - 20);
            }
            const modifiedDataURL = canvas.toDataURL(doc.fileMime);
            window.open(modifiedDataURL);
            URL.revokeObjectURL(img.src);
          };
        } else {
          // Issuer view, no QR code, only watermark if valid
          if (doc.status === 'valid') {
            ctx.font = 'bold 40px Arial';
            ctx.fillStyle = 'rgba(0, 255, 0, 0.5)';
            ctx.fillText('TrustDocs Verified', 20, img.height - 20);
          }
          const modifiedDataURL = canvas.toDataURL(doc.fileMime);
          window.open(modifiedDataURL);
          URL.revokeObjectURL(img.src);
        }
      };
    } else if (doc.fileMime === 'application/pdf') {
      const blob = new Blob([fileData], {type: 'application/pdf'});
      const url = URL.createObjectURL(blob);
      const html = `
        <html>
        <body style="margin:0; position:relative;">
          <embed src="${url}" width="100%" height="100vh" type="application/pdf">
          ${isStudent ? <img src="${qrDataURL}" style="position:absolute; top:10px; right:10px; width:100px; height:100px;" /> : ''}
          ${doc.status === 'valid' ? `
            <div style="position:absolute; top:50%; left:50%; transform:translate(-50%,-50%); background:rgba(0,255,0,0.3); padding:20px; font-size:30px; color:green;">
              TrustDocs Verified
            </div>
          ` : ''}
        </body>
        </html>
      `;
      const htmlBlob = new Blob([html], {type: 'text/html'});
      const htmlUrl = URL.createObjectURL(htmlBlob);
      window.open(htmlUrl);
      setTimeout(() => {
        URL.revokeObjectURL(url);
        URL.revokeObjectURL(htmlUrl);
      }, 1000);
    } else {
      const url = URL.createObjectURL(fileData);
      window.open(url);
      setTimeout(() => URL.revokeObjectURL(url), 1000);
    }
  }

  // View document function
  function viewDocument(id, isStudent = false) {
    const documents = JSON.parse(localStorage.getItem("documents")) || [];
    const doc = documents.find(d => d.id === id);
    if (!doc) {
      showPopup("Document not found.");
      return;
    }
    const fileData = fileStorage[doc.id];
    if (!fileData) {
      showPopup("File data not found.");
      return;
    }
    showDocumentWithQR(doc, isStudent);
  }

  // Student view wrapper
  function viewStudentDocument(id) {
    viewDocument(id, true);
  }

  // Simulated QR code scanning for verification
  function verifyDocument(role) {
    const verifierFile = document.getElementById("verifierFile");
    if (!verifierFile?.files?.[0]) {
      showPopup("Please select a file to verify.");
      return;
    }

    // Simulate QR code scanning by prompting for document ID
    // In a real system, this would involve actual QR code parsing
    const docId = prompt("Enter the Document ID from the QR code:");
    if (!docId) {
      showPopup("No Document ID provided.");
      return;
    }

    const documents = JSON.parse(localStorage.getItem("documents")) || [];
    const doc = documents.find(d => d.id === parseInt(docId));
    if (!doc) {
      showPopup("Document not found.");
      return;
    }

    if (doc.status === 'valid') {
      showPopup("Document is VALID.");
    } else if (doc.status === 'invalid') {
      showPopup("Document is INVALID.");
    } else {
      showPopup("Document is PENDING verification.");
    }
  }

  // Simulated registration and login using localStorage for students
  function registerStudent() {
    const username = document.getElementById("regUsername")?.value;
    const email = document.getElementById("regEmail")?.value;
    const password = document.getElementById("regPassword")?.value;
    const confirmPassword = document.getElementById("regConfirmPassword")?.value;

    if (!username || !email || !password || !confirmPassword) {
      showPopup("Please fill all fields.");
      return;
    }

    if (password !== confirmPassword) {
      showPopup("Passwords do not match.");
      return;
    }

    let users = JSON.parse(localStorage.getItem("studentUsers")) || [];
    const existingUser = users.find(user => user.username === username);
    if (existingUser) {
      showPopup("Username already exists.");
      return;
    }

    users.push({ username, email, password });
    try {
      localStorage.setItem("studentUsers", JSON.stringify(users));
    } catch (e) {
      showPopup("Storage limit reached. Please clear some data.");
      return;
    }
    showPopup("Account created successfully. Please log in.");
    setTimeout(showStudentLogin, 1000);
  }

  function loginStudent() {
    const username = document.getElementById("loginUsername")?.value;
    const password = document.getElementById("loginPassword")?.value;

    if (!username || !password) {
      showPopup("Please fill all fields.");
      return;
    }

    let users = JSON.parse(localStorage.getItem("studentUsers")) || [];
    const user = users.find(user => user.username === username && user.password === password);
    if (!user) {
      showPopup("Invalid username or password.");
      return;
    }

    localStorage.setItem('currentStudent', username);
    showPopup("Logged in successfully.");
    setTimeout(showStudentForm, 1000);
  }

  // Issuer registration and login
  function registerIssuer() {
    const username = document.getElementById("issuerRegUsername")?.value;
    const email = document.getElementById("issuerRegEmail")?.value;
    const password = document.getElementById("issuerRegPassword")?.value;
    const confirmPassword = document.getElementById("issuerRegConfirmPassword")?.value;
    const institution = document.getElementById("issuerRegInstitution")?.value;

    if (!username || !email || !password || !confirmPassword || !institution) {
      showPopup("Please fill all fields.");
      return;
    }

    if (password !== confirmPassword) {
      showPopup("Passwords do not match.");
      return;
    }

    let issuers = JSON.parse(localStorage.getItem("issuerUsers")) || [];
    const existingUser = issuers.find(user => user.username === username);
    if (existingUser) {
      showPopup("Username already exists.");
      return;
    }

    issuers.push({ username, email, password, institution });
    try {
      localStorage.setItem("issuerUsers", JSON.stringify(issuers));
    } catch (e) {
      showPopup("Storage limit reached. Please clear some data.");
      return;
    }
    showPopup("Account created successfully. Please log in.");
    setTimeout(showIssuerLogin, 1000);
  }

  function loginIssuer() {
    const username = document.getElementById("issuerLoginUsername")?.value;
    const password = document.getElementById("issuerLoginPassword")?.value;

    if (!username || !password) {
      showPopup("Please fill all fields.");
      return;
    }

    let issuers = JSON.parse(localStorage.getItem("issuerUsers")) || [];
    const user = issuers.find(user => user.username === username && user.password === password);
    if (!user) {
      showPopup("Invalid username or password.");
      return;
    }

    localStorage.setItem('currentIssuer', JSON.stringify({username: user.username, institution: user.institution}));
    showPopup("Logged in successfully.");
    setTimeout(showIssuerDashboard, 1000);
  }

  // Student upload document
  function uploadDocument() {
    const fullName = document.getElementById("fullName")?.value;
    const email = document.getElementById("email")?.value;
    const institution = document.getElementById("institution")?.value;
    const documentType = document.getElementById("documentType")?.value;
    const studentFile = document.getElementById("studentFile");

    if (!fullName || !email || !institution || !documentType || !studentFile?.files?.[0]) {
      showPopup("Please fill all fields and select a file.");
      return;
    }

    const currentStudent = localStorage.getItem('currentStudent');
    if (!currentStudent) {
      showPopup("Not logged in.");
      return;
    }

    // Show "Uploading" popup without OK button
    showPopup("Uploading", false);

    let documents = JSON.parse(localStorage.getItem("documents")) || [];
    let nextId = parseInt(localStorage.getItem("nextDocId")) || 1;

    const file = studentFile.files[0];
    const newDoc = {
      id: nextId,
      studentUsername: currentStudent,
      institution: institution,
      documentType: documentType,
      documentName: file.name,
      status: 'pending',
      fileMime: file.type
    };

    // Store file in memory
    fileStorage[nextId] = file;

    documents.push(newDoc);
    try {
      localStorage.setItem("documents", JSON.stringify(documents));
      localStorage.setItem("nextDocId", nextId + 1);
    } catch (e) {
      showPopup("Storage limit reached. Please clear some data.");
      delete fileStorage[nextId];
      return;
    }

    // Hide "Uploading" popup and show "Verification in progress" with OK button
    setTimeout(() => {
      closePopup();
      showPopup("Verification in progress", true);
      loadStudentDocuments();
      // Clear file input
      studentFile.value = '';
      document.getElementById("studentFileInfo").style.display = "none";
    }, 1000);
  }

  // Issuer issue document
  function issueDocument() {
    const issuerName = document.getElementById("issuerName")?.value;
    const issuerEmail = document.getElementById("issuerEmail")?.value;
    const issuerInstitution = document.getElementById("issuerInstitution")?.value;
    const issuerDocumentType = document.getElementById("issuerDocumentType")?.value;
    const issuerFile = document.getElementById("issuerFile");

    if (!issuerName || !issuerEmail || !issuerInstitution || !issuerDocumentType || !issuerFile?.files[0]) {
      showPopup("Please fill all fields and select a file.");
      return;
    }

    const currentIssuer = JSON.parse(localStorage.getItem('currentIssuer'));
    if (!currentIssuer) {
      showPopup("Not logged in.");
      return;
    }

    let documents = JSON.parse(localStorage.getItem("documents")) || [];
    let nextId = parseInt(localStorage.getItem("nextDocId")) || 1;

    const file = issuerFile.files[0];
    const newDoc = {
      id: nextId,
      studentUsername: '',
      issuerUsername: currentIssuer.username,
      institution: issuerInstitution,
      documentType: issuerDocumentType,
      documentName: file.name,
      status: 'valid',
      fileMime: file.type
    };

    // Store file in memory
    fileStorage[nextId] = file;

    documents.push(newDoc);
    try {
      localStorage.setItem("documents", JSON.stringify(documents));
      localStorage.setItem("nextDocId", nextId + 1);
    } catch (e) {
      showPopup("Storage limit reached. Please clear some data.");
      delete fileStorage[nextId];
      return;
    }

    showPopup("Document issued successfully.");
    // Clear file input
    issuerFile.value = '';
    document.getElementById("issuerFileInfo").style.display = "none";
  }

  // Load student documents status
  function loadStudentDocuments() {
    const tbody = document.querySelector("#studentDocumentsTable tbody");
    if (!tbody) {
      console.error("Student documents table body not found");
      return;
    }
    tbody.innerHTML = '';

    const documents = JSON.parse(localStorage.getItem("documents")) || [];
    const currentStudent = localStorage.getItem('currentStudent');
    const myDocs = documents.filter(doc => doc.studentUsername === currentStudent);

    myDocs.forEach((doc, index) => {
      const verificationIcon = doc.status === 'valid' ? '<span class="status-icon valid">✔</span>' :
                              doc.status === 'invalid' ? '<span class="status-icon invalid">✖</span>' : '';
      const row = document.createElement('tr');
      row.innerHTML = `
        <td>${index + 1}</td>
        <td>${doc.documentType}</td>
        <td>${doc.documentName}</td>
        <td>${doc.status.toUpperCase()}</td>
        <td>${verificationIcon}</td>
        <td>
          <button class="action-btn view-btn" onclick="viewStudentDocument(${doc.id})">View</button>
        </td>
      `;
      tbody.appendChild(row);
    });
  }

  // Load pending documents for issuer
  function loadPendingDocuments() {
    const tbody = document.querySelector("#pendingDocumentsTable tbody");
    if (!tbody) {
      console.error("Pending documents table body not found");
      return;
    }
    tbody.innerHTML = '';

    const documents = JSON.parse(localStorage.getItem("documents")) || [];
    const currentIssuer = JSON.parse(localStorage.getItem('currentIssuer'));
    if (!currentIssuer) {
      showPopup("Not logged in.");
      return;
    }
    const pendingDocs = documents.filter(doc => doc.institution === currentIssuer.institution && doc.status === 'pending');

    pendingDocs.forEach((doc, index) => {
      const row = document.createElement('tr');
      row.innerHTML = `
        <td>${index + 1}</td>
        <td>${doc.studentUsername}</td>
        <td>${doc.documentType}</td>
        <td>${doc.documentName}</td>
        <td>
          <button class="action-btn view-btn" onclick="viewDocument(${doc.id})">View</button>
          <button class="action-btn valid-btn" onclick="markValid(${doc.id})">Valid</button>
          <button class="action-btn invalid-btn" onclick="markInvalid(${doc.id})">Invalid</button>
        </td>
      `;
      tbody.appendChild(row);
    });
  }

  // Mark valid
  function markValid(id) {
    updateDocumentStatus(id, 'valid');
  }

  // Mark invalid
  function markInvalid(id) {
    updateDocumentStatus(id, 'invalid');
  }

  // Update document status
  function updateDocumentStatus(id, status) {
    let documents = JSON.parse(localStorage.getItem("documents")) || [];
    const docIndex = documents.findIndex(d => d.id === id);
    if (docIndex !== -1) {
      documents[docIndex].status = status;
      documents[docIndex].verifier = JSON.parse(localStorage.getItem('currentIssuer')).username;
      documents[docIndex].verifyDate = new Date().toISOString();
      try {
        localStorage.setItem("documents", JSON.stringify(documents));
      } catch (e) {
        showPopup("Storage limit reached. Please clear some data.");
        return;
      }
      showPopup(Document marked as ${status.toUpperCase()});
      loadPendingDocuments();
      // Update student table if open
      if (document.getElementById('studentPage').style.display === 'block') {
        loadStudentDocuments();
      }
    }
  }
</script>
</body>
</html>
