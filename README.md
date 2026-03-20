<!DOCTYPE html>
<html lang="hi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>📱 रिश्तेदार मैनेजर (फोटो के साथ)</title>

<!-- Libraries -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<!-- Font Awesome -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">

<style>
* { box-sizing: border-box; margin: 0; padding: 0; }

body {
  font-family: 'Segoe UI', Arial, sans-serif;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  min-height: 100vh;
  padding: 20px;
}

.container {
  max-width: 1400px;
  margin: 0 auto;
}

/* Header */
.header {
  background: white;
  border-radius: 20px;
  padding: 20px;
  margin-bottom: 20px;
  box-shadow: 0 10px 30px rgba(0,0,0,0.2);
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
}

.header h2 {
  font-size: 2em;
  background: linear-gradient(135deg, #667eea, #764ba2);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.stats {
  display: flex;
  gap: 20px;
}

.stat-card {
  background: linear-gradient(135deg, #667eea, #764ba2);
  color: white;
  padding: 10px 20px;
  border-radius: 10px;
  text-align: center;
  min-width: 100px;
}

.stat-card i { font-size: 24px; margin-bottom: 5px; }
.stat-card span { display: block; font-size: 20px; font-weight: bold; }

/* Storage Status */
.storage-status {
  display: flex;
  align-items: center;
  gap: 10px;
  background: #f0f0f0;
  padding: 8px 15px;
  border-radius: 50px;
  font-size: 14px;
}

.storage-status i {
  color: #667eea;
}

/* Form Section */
.form-section {
  background: white;
  border-radius: 20px;
  padding: 20px;
  margin-bottom: 20px;
  box-shadow: 0 10px 30px rgba(0,0,0,0.2);
}

.form-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 15px;
  margin-bottom: 15px;
}

.form-group {
  position: relative;
}

.form-group i {
  position: absolute;
  left: 10px;
  top: 50%;
  transform: translateY(-50%);
  color: #667eea;
}

input, select, textarea {
  width: 100%;
  padding: 12px 12px 12px 35px;
  border: 2px solid #e0e0e0;
  border-radius: 10px;
  font-size: 14px;
  transition: all 0.3s;
}

input:focus, select:focus, textarea:focus {
  border-color: #667eea;
  outline: none;
  box-shadow: 0 0 0 3px rgba(102,126,234,0.1);
}

.btn-group {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

.btn {
  padding: 12px 25px;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  font-size: 14px;
  font-weight: bold;
  display: inline-flex;
  align-items: center;
  gap: 8px;
  transition: all 0.3s;
}

.btn-primary { background: linear-gradient(135deg, #667eea, #764ba2); color: white; }
.btn-success { background: #10b981; color: white; }
.btn-warning { background: #f59e0b; color: white; }
.btn-danger { background: #ef4444; color: white; }
.btn-info { background: #3b82f6; color: white; }
.btn-purple { background: #8b5cf6; color: white; }

.btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 5px 15px rgba(0,0,0,0.2);
}

/* Search Section */
.search-section {
  background: white;
  border-radius: 20px;
  padding: 20px;
  margin-bottom: 20px;
  box-shadow: 0 10px 30px rgba(0,0,0,0.2);
}

.search-header {
  display: flex;
  align-items: center;
  gap: 15px;
  margin-bottom: 15px;
}

.search-header h3 {
  color: #333;
  font-size: 18px;
}

.search-header h3 i {
  color: #667eea;
  margin-right: 8px;
}

.search-box-container {
  display: flex;
  gap: 10px;
  align-items: center;
  flex-wrap: wrap;
}

.search-box {
  flex: 2;
  position: relative;
  min-width: 300px;
}

.search-box i {
  position: absolute;
  left: 15px;
  top: 50%;
  transform: translateY(-50%);
  color: #667eea;
  font-size: 16px;
}

.search-box input {
  width: 100%;
  padding: 15px 15px 15px 45px;
  border: 2px solid #e0e0e0;
  border-radius: 50px;
  font-size: 16px;
  transition: all 0.3s;
  background: #f8f9fa;
}

.search-box input:focus {
  border-color: #667eea;
  outline: none;
  box-shadow: 0 0 0 4px rgba(102,126,234,0.1);
  background: white;
}

.search-filters {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
  flex: 1;
}

.filter-select {
  flex: 1;
  min-width: 150px;
  padding: 12px 15px;
  border: 2px solid #e0e0e0;
  border-radius: 50px;
  font-size: 14px;
  background: #f8f9fa;
  cursor: pointer;
}

/* Navigation */
.navigation {
  background: white;
  border-radius: 20px;
  padding: 15px 20px;
  margin-bottom: 20px;
  box-shadow: 0 5px 15px rgba(0,0,0,0.1);
  display: flex;
  align-items: center;
  gap: 15px;
  flex-wrap: wrap;
}

.nav-item {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 8px 15px;
  border-radius: 50px;
  cursor: pointer;
  transition: all 0.3s;
}

.nav-item:hover {
  background: #f0f0f0;
}

.nav-item.active {
  background: linear-gradient(135deg, #667eea, #764ba2);
  color: white;
}

.nav-item i {
  font-size: 16px;
}

.current-family {
  margin-left: auto;
  font-weight: bold;
  color: #667eea;
  background: #f0f4ff;
  padding: 8px 20px;
  border-radius: 50px;
}

/* Family View */
.family-view {
  background: white;
  border-radius: 20px;
  padding: 20px;
  margin-bottom: 20px;
  box-shadow: 0 10px 30px rgba(0,0,0,0.2);
  border: 3px solid #667eea;
}

.family-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  padding-bottom: 15px;
  border-bottom: 2px solid #f0f0f0;
}

.family-header h2 {
  color: #333;
  font-size: 24px;
}

.family-header h2 i {
  color: #667eea;
  margin-right: 10px;
}

.family-info {
  background: linear-gradient(135deg, #667eea, #764ba2);
  color: white;
  padding: 10px 25px;
  border-radius: 50px;
  font-size: 16px;
}

.back-button {
  background: #f0f0f0;
  color: #333;
  padding: 10px 20px;
  border-radius: 50px;
  cursor: pointer;
  display: inline-flex;
  align-items: center;
  gap: 8px;
  transition: all 0.3s;
  margin-bottom: 20px;
}

.back-button:hover {
  background: #e0e0e0;
}

/* Family Head Card */
.family-head-card {
  background: white;
  border-radius: 15px;
  overflow: hidden;
  box-shadow: 0 5px 20px rgba(0,0,0,0.1);
  transition: all 0.3s;
  position: relative;
  cursor: pointer;
  border: 2px solid transparent;
}

.family-head-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 30px rgba(0,0,0,0.2);
  border-color: #f59e0b;
}

.family-head-card .card-header {
  background: linear-gradient(135deg, #f59e0b, #fbbf24);
  color: white;
  padding: 15px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.member-count {
  background: rgba(255,255,255,0.2);
  padding: 5px 12px;
  border-radius: 50px;
  font-size: 12px;
}

.family-head-card .card-body {
  padding: 15px;
  display: flex;
  gap: 15px;
}

.family-members-preview {
  margin-top: 10px;
  padding: 10px;
  background: #f8f9fa;
  border-radius: 10px;
  font-size: 12px;
  color: #666;
}

.family-members-preview i {
  color: #f59e0b;
  margin-right: 5px;
}

/* Member Card */
.member-card {
  background: white;
  border-radius: 15px;
  overflow: hidden;
  box-shadow: 0 5px 15px rgba(0,0,0,0.1);
  transition: all 0.3s;
  position: relative;
  border-left: 4px solid #667eea;
}

.member-card:hover {
  transform: translateX(5px);
  box-shadow: 0 5px 20px rgba(0,0,0,0.15);
}

.member-card .card-header {
  background: linear-gradient(135deg, #667eea, #764ba2);
  color: white;
  padding: 12px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.member-card .card-body {
  padding: 12px;
  display: flex;
  gap: 12px;
}

/* Avatar with Photo */
.avatar {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  object-fit: cover;
  border: 3px solid #667eea;
  background: #f0f0f0;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 30px;
  color: #667eea;
  cursor: pointer;
  transition: transform 0.3s;
}

.avatar:hover {
  transform: scale(1.05);
}

.avatar-img {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  object-fit: cover;
}

.avatar-placeholder {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  background: linear-gradient(135deg, #667eea, #764ba2);
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 30px;
}

/* Cards Grid */
.cards-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
}

/* Member Grid */
.member-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
  gap: 15px;
}

/* Import/Export Section */
.import-export-section {
  background: white;
  border-radius: 20px;
  padding: 15px;
  margin-bottom: 20px;
  box-shadow: 0 5px 15px rgba(0,0,0,0.1);
  display: flex;
  gap: 15px;
  flex-wrap: wrap;
  align-items: center;
  border: 2px dashed #667eea;
}

.import-export-title {
  display: flex;
  align-items: center;
  gap: 8px;
  color: #667eea;
  font-weight: bold;
}

.import-file-input {
  position: relative;
  display: inline-block;
}

.import-file-input input[type=file] {
  padding: 10px;
  border: 2px solid #e0e0e0;
  border-radius: 10px;
  width: 250px;
}

/* Photo Modal */
.photo-modal {
  display: none;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0,0,0,0.8);
  z-index: 2000;
  justify-content: center;
  align-items: center;
}

.photo-modal img {
  max-width: 90%;
  max-height: 90%;
  border-radius: 10px;
  box-shadow: 0 0 30px rgba(0,0,0,0.5);
}

.close-modal {
  position: absolute;
  top: 20px;
  right: 30px;
  color: white;
  font-size: 40px;
  cursor: pointer;
}

/* Sidebar */
.sidebar {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.sidebar-card {
  background: white;
  border-radius: 15px;
  padding: 15px;
  box-shadow: 0 5px 20px rgba(0,0,0,0.1);
}

.sidebar-card h3 {
  margin-bottom: 15px;
  color: #333;
  display: flex;
  align-items: center;
  gap: 8px;
}

.upcoming-item {
  padding: 10px;
  background: #f8f9fa;
  border-radius: 8px;
  margin-bottom: 8px;
  border-left: 4px solid #667eea;
  cursor: pointer;
  transition: all 0.3s;
}

.upcoming-item:hover {
  transform: translateX(5px);
  background: #f0f4ff;
}

.upcoming-item .date {
  font-size: 12px;
  color: #666;
  margin-bottom: 5px;
}

.upcoming-item .name {
  font-weight: bold;
  color: #333;
}

/* Tags */
.tags {
  display: flex;
  gap: 5px;
  flex-wrap: wrap;
  margin-top: 5px;
}

.tag {
  background: #f0f0f0;
  padding: 3px 8px;
  border-radius: 12px;
  font-size: 10px;
  display: inline-flex;
  align-items: center;
  gap: 3px;
}

/* Alert Messages */
.alert {
  padding: 15px;
  border-radius: 10px;
  margin-bottom: 15px;
  display: flex;
  align-items: center;
  gap: 10px;
  animation: slideIn 0.3s ease-out;
}

.alert-success {
  background: #d1fae5;
  color: #065f46;
  border-left: 5px solid #10b981;
}

.alert-error {
  background: #fee2e2;
  color: #991b1b;
  border-left: 5px solid #ef4444;
}

.alert-warning {
  background: #ffedd5;
  color: #92400e;
  border-left: 5px solid #f59e0b;
}

@keyframes slideIn {
  from {
    transform: translateY(-20px);
    opacity: 0;
  }
  to {
    transform: translateY(0);
    opacity: 1;
  }
}

/* Responsive */
@media (max-width: 768px) {
  .form-grid {
    grid-template-columns: 1fr;
  }
  
  .btn-group {
    flex-direction: column;
  }
  
  .btn {
    width: 100%;
  }
  
  .search-box-container {
    flex-direction: column;
  }
  
  .search-box {
    width: 100%;
  }
  
  .search-filters {
    width: 100%;
  }
  
  .filter-select {
    width: 100%;
  }
  
  .import-export-section {
    flex-direction: column;
    align-items: stretch;
  }
  
  .import-file-input input[type=file] {
    width: 100%;
  }
  
  .navigation {
    flex-direction: column;
    align-items: flex-start;
  }
  
  .current-family {
    margin-left: 0;
    width: 100%;
    text-align: center;
  }
}
</style>
</head>
<body>

<div class="container">
  <!-- Header -->
  <div class="header">
    <div style="display: flex; align-items: center; gap: 20px; flex-wrap: wrap;">
      <h2><i class="fas fa-people-arrows"></i> रिश्तेदार मैनेजर</h2>
      <div class="storage-status" id="storageStatus">
        <i class="fas fa-database"></i>
        <span>लोकल स्टोरेज: <span id="storageCount">0</span> रिकॉर्ड</span>
      </div>
    </div>
    <div class="stats">
      <div class="stat-card">
        <i class="fas fa-users"></i>
        <span id="totalCount">0</span>
        <small>कुल</small>
      </div>
      <div class="stat-card">
        <i class="fas fa-tree"></i>
        <span id="familyCount">0</span>
        <small>परिवार</small>
      </div>
      <div class="stat-card">
        <i class="fas fa-birthday-cake"></i>
        <span id="birthdayCount">0</span>
        <small>जन्मदिन</small>
      </div>
    </div>
  </div>

  <!-- Form Section (फोटो ऑप्शन के साथ) -->
  <div class="form-section">
    <div class="form-grid">
      <div class="form-group">
        <i class="fas fa-user"></i>
        <input type="text" id="name" placeholder="नाम *">
      </div>
      <div class="form-group">
        <i class="fas fa-home"></i>
        <input type="text" id="village" placeholder="गांव *">
      </div>
      <div class="form-group">
        <i class="fas fa-people-arrows"></i>
        <select id="relation">
          <option value="">रिश्ता चुनें</option>
          <optgroup label="👨‍👩‍👧‍👦 परिवार">
            <option value="पिता">पिता</option>
            <option value="माता">माता</option>
            <option value="भाई">भाई</option>
            <option value="बहन">बहन</option>
            <option value="दादा">दादा</option>
            <option value="दादी">दादी</option>
            <option value="नाना">नाना</option>
            <option value="नानी">नानी</option>
            <option value="बेटा">बेटा</option>
            <option value="बेटी">बेटी</option>
          </optgroup>
          <optgroup label="💍 वैवाहिक">
            <option value="पति">पति</option>
            <option value="पत्नी">पत्नी</option>
            <option value="ससुर">ससुर</option>
            <option value="सास">सास</option>
            <option value="देवर">देवर</option>
            <option value="जेठ">जेठ</option>
            <option value="ननद">ननद</option>
            <option value="बहू">बहू</option>
            <option value="दामाद">दामाद</option>
          </optgroup>
          <optgroup label="👶 खून के रिश्ते">
            <option value="चाचा">चाचा</option>
            <option value="ताऊ">ताऊ</option>
            <option value="बुआ">बुआ</option>
            <option value="मामा">मामा</option>
            <option value="मौसी">मौसी</option>
            <option value="चचेरा भाई">चचेरा भाई</option>
            <option value="चचेरी बहन">चचेरी बहन</option>
          </optgroup>
          <optgroup label="🤝 सामाजिक">
            <option value="दोस्त">दोस्त</option>
            <option value="पड़ोसी">पड़ोसी</option>
            <option value="गुरु">गुरु</option>
            <option value="सहकर्मी">सहकर्मी</option>
          </optgroup>
        </select>
      </div>
      <div class="form-group">
        <i class="fas fa-phone"></i>
        <input type="tel" id="phone" placeholder="मोबाइल नंबर">
      </div>
      <div class="form-group">
        <i class="fas fa-map-marker-alt"></i>
        <input type="url" id="map" placeholder="Google Map Link">
      </div>
      <div class="form-group">
        <i class="fas fa-calendar"></i>
        <input type="date" id="birthday" placeholder="जन्मदिन">
      </div>
      <div class="form-group">
        <i class="fas fa-om"></i>
        <input type="text" id="gotra" placeholder="गोत्र/जाति">
      </div>
      <div class="form-group">
        <i class="fas fa-venus-mars"></i>
        <select id="gender">
          <option value="">लिंग चुनें</option>
          <option value="पुरुष">पुरुष</option>
          <option value="महिला">महिला</option>
          <option value="अन्य">अन्य</option>
        </select>
      </div>
      <!-- परिवार मुखिया का ऑप्शन -->
      <div class="form-group">
        <i class="fas fa-crown"></i>
        <select id="familyHead">
          <option value="">परिवार मुखिया चुनें</option>
        </select>
      </div>
    </div>

    <div class="form-group" style="margin-bottom: 15px;">
      <i class="fas fa-sticky-note"></i>
      <textarea id="notes" placeholder="नोट्स / यादें ..." rows="2"></textarea>
    </div>

    <!-- फोटो अपलोड ऑप्शन (वापस जोड़ा) -->
    <div class="form-group" style="margin-bottom: 15px;">
      <i class="fas fa-camera"></i>
      <input type="file" id="photo" accept="image/*" style="padding: 12px;">
      <small style="display: block; margin-top: 5px; color: #666;">फोटो चुनें (JPG, PNG)</small>
    </div>

    <div class="btn-group">
      <button class="btn btn-primary" onclick="saveData()">
        <i class="fas fa-save"></i> सेव करें
      </button>
      <button class="btn btn-success" onclick="clearForm()">
        <i class="fas fa-undo"></i> क्लियर
      </button>
      <button class="btn btn-info" onclick="loadData()">
        <i class="fas fa-sync-alt"></i> रिफ्रेश
      </button>
    </div>
  </div>

  <!-- Search Section -->
  <div class="search-section">
    <div class="search-header">
      <h3><i class="fas fa-search"></i> खोजें</h3>
    </div>
    
    <div class="search-box-container">
      <div class="search-box">
        <i class="fas fa-search"></i>
        <input type="text" id="search" placeholder="नाम, गांव, फोन, रिश्ता से खोजें..." onkeyup="searchData()" autocomplete="off">
      </div>
      
      <div class="search-filters">
        <select class="filter-select" id="filterVillage" onchange="filterByVillage()">
          <option value="">सभी गांव</option>
        </select>
        <select class="filter-select" id="filterRelation" onchange="filterByRelation()">
          <option value="">सभी रिश्ते</option>
        </select>
      </div>
    </div>
  </div>

  <!-- Navigation -->
  <div class="navigation" id="navigation">
    <div class="nav-item active" onclick="showFamilyHeads()">
      <i class="fas fa-crown"></i>
      <span>परिवार मुखिया</span>
    </div>
    <div class="nav-item" onclick="showAllMembers()">
      <i class="fas fa-users"></i>
      <span>सभी सदस्य</span>
    </div>
    <div id="currentFamilyDisplay" class="current-family" style="display: none;">
      <i class="fas fa-home"></i> <span id="currentFamilyName"></span>
    </div>
  </div>

  <!-- Import/Export Section -->
  <div class="import-export-section">
    <div class="import-export-title">
      <i class="fas fa-exchange-alt"></i>
      <span>इंपोर्ट / एक्सपोर्ट</span>
    </div>
    
    <div class="btn-group">
      <button class="btn btn-success" onclick="exportExcel()">
        <i class="fas fa-file-excel"></i> Excel
      </button>
      <button class="btn btn-danger" onclick="exportPDF()">
        <i class="fas fa-file-pdf"></i> PDF
      </button>
      <button class="btn btn-warning" onclick="printData()">
        <i class="fas fa-print"></i> Print
      </button>
    </div>
    
    <div class="btn-group">
      <button class="btn btn-purple" onclick="exportToFile()">
        <i class="fas fa-download"></i> JSON एक्सपोर्ट
      </button>
      <div class="import-file-input">
        <input type="file" id="importFile" accept=".json" style="padding: 8px;">
        <button class="btn btn-info" onclick="importFromFile()" style="margin-left: 5px;">
          <i class="fas fa-upload"></i> इंपोर्ट
        </button>
      </div>
    </div>
    
    <button class="btn btn-danger" onclick="clearAllData()">
      <i class="fas fa-trash-alt"></i> सभी डेटा डिलीट
    </button>
  </div>

  <!-- Alert Container -->
  <div id="alertContainer"></div>

  <!-- Main Content -->
  <div class="main-content">
    <!-- Cards Grid -->
    <div id="contentArea">
      <!-- यहाँ डेटा दिखेगा -->
    </div>

    <!-- Sidebar -->
    <div class="sidebar">
      <!-- Upcoming Birthdays -->
      <div class="sidebar-card">
        <h3><i class="fas fa-birthday-cake" style="color: #f59e0b;"></i> आने वाले जन्मदिन</h3>
        <div id="upcomingBirthdays"></div>
      </div>

      <!-- Family Stats -->
      <div class="sidebar-card">
        <h3><i class="fas fa-chart-pie"></i> परिवार के आंकड़े</h3>
        <div id="familyStats"></div>
      </div>

      <!-- Quick Stats -->
      <div class="sidebar-card">
        <h3><i class="fas fa-chart-pie"></i> रिश्तों के आंकड़े</h3>
        <canvas id="relationChart" style="height: 200px;"></canvas>
      </div>
      
      <!-- Backup Info -->
      <div class="sidebar-card">
        <h3><i class="fas fa-info-circle"></i> जानकारी</h3>
        <p style="font-size: 14px; color: #666;">
          <i class="fas fa-database"></i> डेटा आपके ब्राउज़र की localStorage में सेव है।<br>
          <i class="fas fa-crown"></i> परिवार मुखिया पर क्लिक करें।<br>
          <i class="fas fa-users"></i> पूरा परिवार देखें।
        </p>
      </div>
    </div>
  </div>
</div>

<!-- Photo Modal -->
<div id="photoModal" class="photo-modal" onclick="closePhotoModal()">
  <span class="close-modal">&times;</span>
  <img id="modalImage" src="" alt="बड़ी फोटो">
</div>

<script>
// ==================== GLOBAL VARIABLES ====================
let people = [];
let editIndex = -1;
let currentView = 'familyHeads'; // 'familyHeads', 'allMembers', 'family'
let currentFamilyHeadId = null;
let searchResults = null;

// DOM Elements
let nameEl, villageEl, relationEl, phoneEl, mapEl, birthdayEl, gotraEl, genderEl, familyHeadEl, notesEl, photoEl;
let contentArea, search, filterVillage, filterRelation;

// Chart instance
let relationChart = null;

// Storage key
const STORAGE_KEY = 'rishtedar_people';

// ==================== INITIALIZATION ====================
document.addEventListener('DOMContentLoaded', function() {
  // Get DOM elements
  nameEl = document.getElementById("name");
  villageEl = document.getElementById("village");
  relationEl = document.getElementById("relation");
  phoneEl = document.getElementById("phone");
  mapEl = document.getElementById("map");
  birthdayEl = document.getElementById("birthday");
  gotraEl = document.getElementById("gotra");
  genderEl = document.getElementById("gender");
  familyHeadEl = document.getElementById("familyHead");
  notesEl = document.getElementById("notes");
  photoEl = document.getElementById("photo");
  contentArea = document.getElementById("contentArea");
  search = document.getElementById("search");
  filterVillage = document.getElementById("filterVillage");
  filterRelation = document.getElementById("filterRelation");
  
  // Load data from localStorage
  loadData();
});

// Load data from localStorage
function loadData() {
  let savedData = localStorage.getItem(STORAGE_KEY);
  if (savedData) {
    try {
      people = JSON.parse(savedData);
      showAlert('डेटा लोड हो गया!', 'success');
    } catch (e) {
      people = [];
      showAlert('डेटा लोड करने में समस्या हुई', 'error');
    }
  } else {
    people = [];
  }
  editIndex = -1;
  currentView = 'familyHeads';
  currentFamilyHeadId = null;
  searchResults = null;
  search.value = '';
  updateUI();
}

// Save data to localStorage
function saveToStorage() {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(people));
  updateStorageStatus();
}

// Update all UI components
function updateUI() {
  updateFamilyHeadOptions();
  updateFilters();
  updateStats();
  updateUpcomingBirthdays();
  updateFamilyStats();
  initChart();
  updateStorageStatus();
  updateNavigation();
  renderView();
}

// Update storage status
function updateStorageStatus() {
  document.getElementById('storageCount').textContent = people.length;
}

// Update navigation
function updateNavigation() {
  const navItems = document.querySelectorAll('.nav-item');
  navItems.forEach(item => {
    item.classList.remove('active');
  });
  
  if (currentView === 'familyHeads') {
    navItems[0].classList.add('active');
    document.getElementById('currentFamilyDisplay').style.display = 'none';
  } else if (currentView === 'allMembers') {
    navItems[1].classList.add('active');
    document.getElementById('currentFamilyDisplay').style.display = 'none';
  } else if (currentView === 'family') {
    navItems[0].classList.add('active');
    const head = people.find(p => p.id == currentFamilyHeadId);
    document.getElementById('currentFamilyName').textContent = head ? head.name : '';
    document.getElementById('currentFamilyDisplay').style.display = 'block';
  }
}

// Show family heads view
function showFamilyHeads() {
  currentView = 'familyHeads';
  currentFamilyHeadId = null;
  searchResults = null;
  search.value = '';
  updateNavigation();
  renderView();
}

// Show all members view
function showAllMembers() {
  currentView = 'allMembers';
  currentFamilyHeadId = null;
  searchResults = null;
  search.value = '';
  updateNavigation();
  renderView();
}

// Show specific family
function showFamily(headId) {
  currentView = 'family';
  currentFamilyHeadId = headId;
  searchResults = null;
  search.value = '';
  updateNavigation();
  renderView();
}

// Update family head dropdown
function updateFamilyHeadOptions() {
  familyHeadEl.innerHTML = '<option value="">कोई नहीं (खुद मुखिया)</option>';
  
  // सभी लोगों को ड्रॉपडाउन में दिखाएँ
  people.forEach(p => {
    if (p.name && p.id) {
      familyHeadEl.innerHTML += `<option value="${p.id}">${p.name} (${p.village})</option>`;
    }
  });
}

// Update family stats in sidebar
function updateFamilyStats() {
  // सभी मुखियाओं को ढूँढें
  const heads = getFamilyHeads();
  
  let html = "";
  if (heads.length === 0) {
    html = '<p style="color: #999; text-align: center;">कोई परिवार मुखिया नहीं</p>';
  } else {
    heads.forEach(head => {
      const members = getFamilyMembers(head.id);
      html += `
      <div class="upcoming-item" style="border-left-color: #f59e0b; cursor: pointer;" onclick="showFamily('${head.id}')">
        <div class="name">
          <i class="fas fa-crown" style="color: #f59e0b;"></i> 
          ${head.name}
        </div>
        <small>${head.village} • ${members.length} सदस्य</small>
      </div>
      `;
    });
  }
  
  document.getElementById('familyStats').innerHTML = html;
}

// Get all family heads (जिनका familyHead खाली है)
function getFamilyHeads() {
  return people.filter(p => !p.familyHead || p.familyHead === "");
}

// Get family members for a head
function getFamilyMembers(headId) {
  return people.filter(p => p.familyHead == headId);
}

// Show alert messages
function showAlert(message, type = 'success') {
  const alertContainer = document.getElementById('alertContainer');
  const icon = type === 'success' ? 'fa-check-circle' : (type === 'error' ? 'fa-exclamation-circle' : 'fa-exclamation-triangle');
  
  alertContainer.innerHTML = `
    <div class="alert alert-${type}">
      <i class="fas ${icon}"></i>
      <span>${message}</span>
    </div>
  `;
  
  setTimeout(() => {
    alertContainer.innerHTML = '';
  }, 3000);
}

// ==================== PHOTO FUNCTIONS ====================
function getBase64(file, callback) {
  let reader = new FileReader();
  reader.onload = () => callback(reader.result);
  reader.readAsDataURL(file);
}

function showPhoto(photoUrl) {
  if (!photoUrl) return;
  document.getElementById('modalImage').src = photoUrl;
  document.getElementById('photoModal').style.display = 'flex';
}

function closePhotoModal() {
  document.getElementById('photoModal').style.display = 'none';
}

// ==================== RENDER VIEWS ====================
function renderView() {
  if (searchResults) {
    renderSearchResults();
    return;
  }
  
  if (currentView === 'familyHeads') {
    renderFamilyHeads();
  } else if (currentView === 'allMembers') {
    renderAllMembers();
  } else if (currentView === 'family') {
    renderFamily();
  }
}

// Render family heads (मुख्य लिस्ट)
function renderFamilyHeads() {
  const heads = getFamilyHeads();
  
  if (heads.length === 0) {
    contentArea.innerHTML = `
      <div style="text-align: center; padding: 50px;">
        <i class="fas fa-crown" style="font-size: 50px; color: #ccc;"></i>
        <p style="margin-top: 20px;">कोई परिवार मुखिया नहीं है</p>
      </div>
    `;
    return;
  }

  let html = '<div class="cards-grid">';
  
  heads.forEach(head => {
    const members = getFamilyMembers(head.id);
    const memberNames = members.slice(0, 3).map(m => m.name).join(', ');
    const moreCount = members.length - 3;
    
    html += `
      <div class="family-head-card" onclick="showFamily('${head.id}')">
        <div class="card-header">
          <span><i class="fas fa-crown"></i> ${head.name}</span>
          <span class="member-count">${members.length} सदस्य</span>
        </div>
        <div class="card-body">
          <div class="avatar" onclick="event.stopPropagation(); showPhoto('${head.photo || ''}')">
            ${head.photo ? 
              `<img src="${head.photo}" class="avatar-img" alt="${head.name}">` : 
              `<div class="avatar-placeholder"><i class="fas fa-user-circle"></i></div>`
            }
          </div>
          <div class="info">
            <div class="info-item">
              <i class="fas fa-home"></i>
              <span>${head.village}</span>
            </div>
            <div class="info-item">
              <i class="fas fa-phone"></i>
              <span>${head.phone || 'नंबर नहीं'}</span>
            </div>
            ${head.gotra ? `
            <div class="info-item">
              <i class="fas fa-om"></i>
              <span>${head.gotra}</span>
            </div>
            ` : ''}
            <div class="family-members-preview">
              <i class="fas fa-users"></i>
              ${memberNames} ${moreCount > 0 ? `और ${moreCount} अन्य` : ''}
            </div>
          </div>
        </div>
      </div>
    `;
  });
  
  html += '</div>';
  contentArea.innerHTML = html;
}

// Render all members (सभी सदस्य)
function renderAllMembers() {
  if (people.length === 0) {
    contentArea.innerHTML = `
      <div style="text-align: center; padding: 50px;">
        <i class="fas fa-users-slash" style="font-size: 50px; color: #ccc;"></i>
        <p style="margin-top: 20px;">कोई डेटा नहीं है</p>
      </div>
    `;
    return;
  }

  // Group by village
  let grouped = {};
  people.forEach(p => {
    if (!grouped[p.village]) grouped[p.village] = [];
    grouped[p.village].push(p);
  });

  let html = "";
  
  Object.keys(grouped).sort().forEach(village => {
    html += `<h3 style="color: white; margin: 20px 0 10px 0;">
      <i class="fas fa-home"></i> ${village} (${grouped[village].length})
    </h3>`;
    
    html += '<div class="member-grid">';

    grouped[village].forEach(p => {
      let isHead = !p.familyHead || p.familyHead === "";
      let age = p.birthday ? calculateAge(p.birthday) : "";
      
      html += `
        <div class="member-card">
          <div class="card-header">
            <span class="relation-badge">
              <i class="fas fa-${p.gender === 'पुरुष' ? 'mars' : p.gender === 'महिला' ? 'venus' : 'genderless'}"></i>
              ${p.relation || 'रिश्ता'}
            </span>
            ${isHead ? '<span class="family-head-badge"><i class="fas fa-crown"></i> मुखिया</span>' : ''}
          </div>
          <div class="card-body">
            <div class="avatar" onclick="showPhoto('${p.photo || ''}')">
              ${p.photo ? 
                `<img src="${p.photo}" class="avatar-img" alt="${p.name}">` : 
                `<div class="avatar-placeholder"><i class="fas fa-user-circle"></i></div>`
              }
            </div>
            <div class="info">
              <div class="info-item">
                <i class="fas fa-user"></i>
                <strong>${p.name}</strong>
              </div>
              <div class="info-item">
                <i class="fas fa-phone"></i>
                <a href="tel:${p.phone}">${p.phone || 'नंबर नहीं'}</a>
              </div>
              ${age ? `
              <div class="info-item">
                <i class="fas fa-calendar"></i>
                <span>${age}</span>
              </div>
              ` : ''}
              ${p.familyHead ? `
              <div class="info-item">
                <i class="fas fa-crown"></i>
                <span>मुखिया: ${getPersonName(p.familyHead)}</span>
              </div>
              ` : ''}
              <div class="card-footer" style="padding: 10px 0 0 0; border: none;">
                ${p.phone ? `
                <a href="tel:${p.phone}" class="action-btn call-btn" style="padding: 5px 10px;">
                  <i class="fas fa-phone-alt"></i>
                </a>
                <a href="https://wa.me/${p.phone.replace(/\D/g,'')}" target="_blank" class="action-btn whatsapp-btn" style="padding: 5px 10px;">
                  <i class="fab fa-whatsapp"></i>
                </a>
                ` : ''}
                <button class="action-btn edit-btn" onclick="editData('${p.id}')" style="padding: 5px 10px;">
                  <i class="fas fa-edit"></i>
                </button>
                <button class="action-btn delete-btn" onclick="deleteData('${p.id}')" style="padding: 5px 10px;">
                  <i class="fas fa-trash"></i>
                </button>
              </div>
            </div>
          </div>
        </div>
      `;
    });
    
    html += '</div>';
  });

  contentArea.innerHTML = html;
}

// Render family members (एक परिवार के सभी सदस्य)
function renderFamily() {
  const head = people.find(p => p.id == currentFamilyHeadId);
  if (!head) {
    showFamilyHeads();
    return;
  }
  
  const members = getFamilyMembers(head.id);
  
  let html = `
    <div class="family-view">
      <div class="family-header">
        <h2><i class="fas fa-crown" style="color: #f59e0b;"></i> ${head.name} का परिवार</h2>
        <span class="family-info">${members.length + 1} सदस्य</span>
      </div>
      
      <div class="back-button" onclick="showFamilyHeads()">
        <i class="fas fa-arrow-left"></i> सभी परिवार देखें
      </div>
      
      <div class="member-grid">
        <!-- मुखिया को पहले दिखाएँ -->
        <div class="member-card" style="border-left-color: #f59e0b;">
          <div class="card-header" style="background: linear-gradient(135deg, #f59e0b, #fbbf24);">
            <span class="relation-badge">
              <i class="fas fa-crown"></i> परिवार मुखिया
            </span>
          </div>
          <div class="card-body">
            <div class="avatar" onclick="showPhoto('${head.photo || ''}')">
              ${head.photo ? 
                `<img src="${head.photo}" class="avatar-img" alt="${head.name}">` : 
                `<div class="avatar-placeholder"><i class="fas fa-user-circle"></i></div>`
              }
            </div>
            <div class="info">
              <div class="info-item">
                <i class="fas fa-user"></i>
                <strong>${head.name}</strong>
              </div>
              <div class="info-item">
                <i class="fas fa-phone"></i>
                <a href="tel:${head.phone}">${head.phone || 'नंबर नहीं'}</a>
              </div>
              <div class="info-item">
                <i class="fas fa-home"></i>
                <span>${head.village}</span>
              </div>
              ${head.gotra ? `
              <div class="info-item">
                <i class="fas fa-om"></i>
                <span>${head.gotra}</span>
              </div>
              ` : ''}
              <div class="card-footer" style="padding: 10px 0 0 0; border: none;">
                ${head.phone ? `
                <a href="tel:${head.phone}" class="action-btn call-btn" style="padding: 5px 10px;">
                  <i class="fas fa-phone-alt"></i>
                </a>
                <a href="https://wa.me/${head.phone.replace(/\D/g,'')}" target="_blank" class="action-btn whatsapp-btn" style="padding: 5px 10px;">
                  <i class="fab fa-whatsapp"></i>
                </a>
                ` : ''}
                <button class="action-btn edit-btn" onclick="editData('${head.id}')" style="padding: 5px 10px;">
                  <i class="fas fa-edit"></i>
                </button>
              </div>
            </div>
          </div>
        </div>
  `;
  
  // बाकी सदस्य
  members.forEach(p => {
    let age = p.birthday ? calculateAge(p.birthday) : "";
    
    html += `
      <div class="member-card">
        <div class="card-header">
          <span class="relation-badge">
            <i class="fas fa-${p.gender === 'पुरुष' ? 'mars' : p.gender === 'महिला' ? 'venus' : 'genderless'}"></i>
            ${p.relation || 'रिश्ता'}
          </span>
        </div>
        <div class="card-body">
          <div class="avatar" onclick="showPhoto('${p.photo || ''}')">
            ${p.photo ? 
              `<img src="${p.photo}" class="avatar-img" alt="${p.name}">` : 
              `<div class="avatar-placeholder"><i class="fas fa-user-circle"></i></div>`
            }
          </div>
          <div class="info">
            <div class="info-item">
              <i class="fas fa-user"></i>
              <strong>${p.name}</strong>
            </div>
            <div class="info-item">
              <i class="fas fa-phone"></i>
              <a href="tel:${p.phone}">${p.phone || 'नंबर नहीं'}</a>
            </div>
            ${age ? `
            <div class="info-item">
              <i class="fas fa-calendar"></i>
              <span>${age}</span>
            </div>
            ` : ''}
            <div class="card-footer" style="padding: 10px 0 0 0; border: none;">
              ${p.phone ? `
              <a href="tel:${p.phone}" class="action-btn call-btn" style="padding: 5px 10px;">
                <i class="fas fa-phone-alt"></i>
              </a>
              <a href="https://wa.me/${p.phone.replace(/\D/g,'')}" target="_blank" class="action-btn whatsapp-btn" style="padding: 5px 10px;">
                <i class="fab fa-whatsapp"></i>
              </a>
              ` : ''}
              <button class="action-btn edit-btn" onclick="editData('${p.id}')" style="padding: 5px 10px;">
                <i class="fas fa-edit"></i>
              </button>
              <button class="action-btn delete-btn" onclick="deleteData('${p.id}')" style="padding: 5px 10px;">
                <i class="fas fa-trash"></i>
              </button>
            </div>
          </div>
        </div>
      </div>
    `;
  });
  
  html += '</div></div>';
  contentArea.innerHTML = html;
}

// Render search results
function renderSearchResults() {
  if (!searchResults || searchResults.length === 0) {
    contentArea.innerHTML = `
      <div style="text-align: center; padding: 50px;">
        <i class="fas fa-search" style="font-size: 50px; color: #ccc;"></i>
        <p style="margin-top: 20px;">कोई रिजल्ट नहीं मिला</p>
      </div>
    `;
    return;
  }
  
  let html = '<div class="member-grid">';
  
  searchResults.forEach(p => {
    let isHead = !p.familyHead || p.familyHead === "";
    let age = p.birthday ? calculateAge(p.birthday) : "";
    
    html += `
      <div class="member-card">
        <div class="card-header">
          <span class="relation-badge">
            <i class="fas fa-${p.gender === 'पुरुष' ? 'mars' : p.gender === 'महिला' ? 'venus' : 'genderless'}"></i>
            ${p.relation || 'रिश्ता'}
          </span>
          ${isHead ? '<span class="family-head-badge"><i class="fas fa-crown"></i> मुखिया</span>' : ''}
        </div>
        <div class="card-body">
          <div class="avatar" onclick="showPhoto('${p.photo || ''}')">
            ${p.photo ? 
              `<img src="${p.photo}" class="avatar-img" alt="${p.name}">` : 
              `<div class="avatar-placeholder"><i class="fas fa-user-circle"></i></div>`
            }
          </div>
          <div class="info">
            <div class="info-item">
              <i class="fas fa-user"></i>
              <strong>${highlightText(p.name, lastSearchTerm)}</strong>
            </div>
            <div class="info-item">
              <i class="fas fa-phone"></i>
              <a href="tel:${p.phone}">${p.phone || 'नंबर नहीं'}</a>
            </div>
            <div class="info-item">
              <i class="fas fa-home"></i>
              <span>${p.village}</span>
            </div>
            ${p.familyHead ? `
            <div class="info-item">
              <i class="fas fa-crown"></i>
              <span>मुखिया: ${getPersonName(p.familyHead)}</span>
            </div>
            ` : ''}
            <div class="card-footer" style="padding: 10px 0 0 0; border: none;">
              ${p.phone ? `
              <a href="tel:${p.phone}" class="action-btn call-btn" style="padding: 5px 10px;">
                <i class="fas fa-phone-alt"></i>
              </a>
              <a href="https://wa.me/${p.phone.replace(/\D/g,'')}" target="_blank" class="action-btn whatsapp-btn" style="padding: 5px 10px;">
                <i class="fab fa-whatsapp"></i>
              </a>
              ` : ''}
              <button class="action-btn edit-btn" onclick="editData('${p.id}')" style="padding: 5px 10px;">
                <i class="fas fa-edit"></i>
              </button>
              <button class="action-btn delete-btn" onclick="deleteData('${p.id}')" style="padding: 5px 10px;">
                <i class="fas fa-trash"></i>
              </button>
            </div>
          </div>
        </div>
      </div>
    `;
  });
  
  html += '</div>';
  contentArea.innerHTML = html;
}

// Helper function to highlight text
function highlightText(text, term) {
  if (!term || !text) return text;
  const regex = new RegExp(`(${term})`, 'gi');
  return text.replace(regex, '<span style="background: #fef3c7; color: #92400e; padding: 2px 4px; border-radius: 4px;">$1</span>');
}

// ==================== CRUD OPERATIONS ====================
function saveData() {
  let name = nameEl.value.trim();
  let village = villageEl.value.trim();

  if (!name || !village) {
    showAlert('❌ नाम और गांव जरूरी है', 'error');
    return;
  }

  let file = photoEl.files[0];

  if (file) {
    getBase64(file, (base64) => processSave(base64));
  } else {
    // If editing, keep old photo
    let oldPhoto = "";
    if (editIndex !== -1 && people[editIndex]) {
      oldPhoto = people[editIndex].photo || "";
    }
    processSave(oldPhoto);
  }
}

function processSave(photo) {
  let person = {
    id: Date.now() + Math.random(),
    name: nameEl.value,
    village: villageEl.value,
    relation: relationEl.value,
    phone: phoneEl.value,
    map: mapEl.value,
    birthday: birthdayEl.value,
    gotra: gotraEl.value,
    gender: genderEl.value,
    familyHead: familyHeadEl.value || "",
    notes: notesEl.value,
    photo: photo,
    createdAt: new Date().toISOString()
  };

  if (editIndex === -1) {
    people.push(person);
    showAlert('✅ नया डेटा सेव हो गया!', 'success');
  } else {
    people[editIndex] = { ...person, id: people[editIndex].id };
    editIndex = -1;
    showAlert('✅ डेटा अपडेट हो गया!', 'success');
  }

  saveToStorage();
  clearForm();
  
  if (searchResults) {
    performSearch(lastSearchTerm);
  } else {
    updateUI();
  }
}

function clearForm() {
  nameEl.value = "";
  villageEl.value = "";
  relationEl.value = "";
  phoneEl.value = "";
  mapEl.value = "";
  birthdayEl.value = "";
  gotraEl.value = "";
  genderEl.value = "";
  familyHeadEl.value = "";
  notesEl.value = "";
  photoEl.value = "";
  editIndex = -1;
}

function deleteData(id) {
  if (confirm("क्या आप सच में डिलीट करना चाहते हैं?")) {
    // Check if this person is a family head
    const isFamilyHead = people.some(p => p.familyHead === id);
    if (isFamilyHead) {
      if (!confirm("⚠️ यह व्यक्ति किसी का परिवार मुखिया है। डिलीट करने पर उनका मुखिया हट जाएगा। क्या आपको यह डिलीट करना है?")) {
        return;
      }
    }
    
    people = people.filter(p => p.id != id);
    saveToStorage();
    
    if (currentView === 'family' && currentFamilyHeadId == id) {
      showFamilyHeads();
    } else if (searchResults) {
      performSearch(lastSearchTerm);
    } else {
      updateUI();
    }
    showAlert('✅ डेटा डिलीट हो गया!', 'success');
  }
}

function editData(id) {
  let index = people.findIndex(p => p.id == id);
  if (index === -1) return;

  let p = people[index];
  nameEl.value = p.name;
  villageEl.value = p.village;
  relationEl.value = p.relation || "";
  phoneEl.value = p.phone || "";
  mapEl.value = p.map || "";
  birthdayEl.value = p.birthday || "";
  gotraEl.value = p.gotra || "";
  genderEl.value = p.gender || "";
  familyHeadEl.value = p.familyHead || "";
  notesEl.value = p.notes || "";

  editIndex = index;
  
  // Scroll to form
  document.querySelector(".form-section").scrollIntoView({ behavior: "smooth" });
}

function clearAllData() {
  if (confirm("⚠️ क्या आप सच में सारा डेटा डिलीट करना चाहते हैं? यह क्रिया वापस नहीं की जा सकती!")) {
    people = [];
    saveToStorage();
    showFamilyHeads();
    showAlert('✅ सारा डेटा डिलीट हो गया!', 'success');
  }
}

// ==================== SEARCH FUNCTIONS ====================
let lastSearchTerm = '';

function searchData() {
  let term = search.value.toLowerCase().trim();
  lastSearchTerm = term;
  
  if (term === '') {
    searchResults = null;
    updateUI();
    return;
  }
  
  performSearch(term);
}

function performSearch(term) {
  searchResults = people.filter(p => 
    p.name.toLowerCase().includes(term) ||
    p.village.toLowerCase().includes(term) ||
    (p.phone && p.phone.includes(term)) ||
    (p.relation && p.relation.toLowerCase().includes(term)) ||
    (p.gotra && p.gotra.toLowerCase().includes(term))
  );
  
  renderSearchResults();
}

// ==================== FILTER FUNCTIONS ====================
function filterByVillage() {
  let village = filterVillage.value;
  if (village) {
    if (searchResults) {
      searchResults = searchResults.filter(p => p.village === village);
      renderSearchResults();
    }
  } else {
    if (searchResults) {
      performSearch(lastSearchTerm);
    }
  }
}

function filterByRelation() {
  let relation = filterRelation.value;
  if (relation) {
    if (searchResults) {
      searchResults = searchResults.filter(p => p.relation === relation);
      renderSearchResults();
    }
  } else {
    if (searchResults) {
      performSearch(lastSearchTerm);
    }
  }
}

function updateFilters() {
  // Village filter
  let villages = [...new Set(people.map(p => p.village))];
  filterVillage.innerHTML = '<option value="">सभी गांव</option>';
  villages.sort().forEach(v => {
    filterVillage.innerHTML += `<option value="${v}">${v}</option>`;
  });

  // Relation filter
  let relations = [...new Set(people.map(p => p.relation))];
  filterRelation.innerHTML = '<option value="">सभी रिश्ते</option>';
  relations.sort().forEach(r => {
    if (r) filterRelation.innerHTML += `<option value="${r}">${r}</option>`;
  });
}

// ==================== HELPER FUNCTIONS ====================
function getPersonName(id) {
  const person = people.find(p => p.id == id);
  return person ? person.name : 'अज्ञात';
}

function calculateAge(birthday) {
  let birthDate = new Date(birthday);
  let today = new Date();
  let age = today.getFullYear() - birthDate.getFullYear();
  let m = today.getMonth() - birthDate.getMonth();
  if (m < 0 || (m === 0 && today.getDate() < birthDate.getDate())) age--;
  return age + " साल";
}

function formatDate(date) {
  let d = new Date(date);
  return d.toLocaleDateString('hi-IN', { day: 'numeric', month: 'short' });
}

// ==================== STATS ====================
function updateStats() {
  document.getElementById("totalCount").textContent = people.length;
  
  let families = getFamilyHeads().length;
  document.getElementById("familyCount").textContent = families;
  
  let today = new Date();
  let next7Days = new Date(today.getTime() + 7 * 24 * 60 * 60 * 1000);
  
  let birthdays = people.filter(p => {
    if (!p.birthday) return false;
    let bd = new Date(p.birthday);
    bd.setFullYear(today.getFullYear());
    return bd >= today && bd <= next7Days;
  }).length;
  
  document.getElementById("birthdayCount").textContent = birthdays;
}

function updateUpcomingBirthdays() {
  let today = new Date();
  let upcoming = people.filter(p => p.birthday).map(p => {
    let bd = new Date(p.birthday);
    bd.setFullYear(today.getFullYear());
    if (bd < today) bd.setFullYear(today.getFullYear() + 1);
    return { ...p, nextBirthday: bd };
  }).sort((a, b) => a.nextBirthday - b.nextBirthday).slice(0, 5);

  let html = "";
  upcoming.forEach(p => {
    let days = Math.ceil((p.nextBirthday - today) / (1000 * 60 * 60 * 24));
    let head = people.find(h => h.id == p.familyHead);
    html += `
    <div class="upcoming-item" onclick="showFamily('${head ? head.id : p.id}')">
      <div class="date">
        <i class="fas fa-calendar-alt"></i> ${formatDate(p.birthday)}
        ${days <= 7 ? '<span style="color: #f59e0b;"> (अगले ' + days + ' दिन)</span>' : ''}
      </div>
      <div class="name">${p.name}</div>
      <small>${p.relation || ''} • ${p.village}</small>
    </div>
    `;
  });

  if (upcoming.length === 0) {
    html = '<p style="color: #999; text-align: center;">कोई आने वाला जन्मदिन नहीं</p>';
  }

  document.getElementById("upcomingBirthdays").innerHTML = html;
}

function initChart() {
  let ctx = document.getElementById('relationChart').getContext('2d');
  
  let relations = {};
  people.forEach(p => {
    if (p.relation) {
      relations[p.relation] = (relations[p.relation] || 0) + 1;
    }
  });

  if (relationChart) {
    relationChart.destroy();
  }

  relationChart = new Chart(ctx, {
    type: 'doughnut',
    data: {
      labels: Object.keys(relations),
      datasets: [{
        data: Object.values(relations),
        backgroundColor: ['#667eea', '#764ba2', '#f59e0b', '#10b981', '#ef4444', '#3b82f6', '#8b5cf6', '#ec4899']
      }]
    },
    options: {
      responsive: true,
      maintainAspectRatio: true,
      plugins: {
        legend: {
          display: false
        }
      }
    }
  });
}

// ==================== EXPORT/IMPORT FUNCTIONS ====================

function exportToFile() {
  if (people.length === 0) {
    showAlert('कोई डेटा नहीं है!', 'error');
    return;
  }

  const dataStr = JSON.stringify(people, null, 2);
  const blob = new Blob([dataStr], { type: 'application/json' });
  const url = URL.createObjectURL(blob);
  const link = document.createElement('a');
  link.href = url;
  link.download = `rishtedar_backup_${new Date().toISOString().slice(0,10)}.json`;
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
  URL.revokeObjectURL(url);
  
  showAlert('✅ डेटा एक्सपोर्ट हो गया! फोन में सेव कर लें।', 'success');
}

function importFromFile() {
  const fileInput = document.getElementById('importFile');
  const file = fileInput.files[0];
  
  if (!file) {
    showAlert('कृपया फाइल चुनें!', 'error');
    return;
  }
  
  const reader = new FileReader();
  
  reader.onload = function(e) {
    try {
      const importedData = JSON.parse(e.target.result);
      
      if (!Array.isArray(importedData)) {
        throw new Error('फाइल फॉर्मेट सही नहीं है');
      }
      
      if (confirm(`क्या आप ${importedData.length} रिकॉर्ड इंपोर्ट करना चाहते हैं? मौजूदा डेटा मर्ज हो जाएगा।`)) {
        
        importedData.forEach(item => {
          if (!item.id) {
            item.id = Date.now() + Math.random();
          }
        });
        
        people = [...people, ...importedData];
        
        const uniqueIds = new Set();
        people = people.filter(item => {
          if (uniqueIds.has(item.id)) {
            return false;
          }
          uniqueIds.add(item.id);
          return true;
        });
        
        saveToStorage();
        showFamilyHeads();
        
        fileInput.value = '';
        showAlert(`✅ ${importedData.length} रिकॉर्ड इंपोर्ट हो गए!`, 'success');
      }
      
    } catch (error) {
      showAlert('फाइल इंपोर्ट करने में समस्या हुई! सही JSON फाइल चुनें।', 'error');
      console.error(error);
    }
  };
  
  reader.readAsText(file);
}

function exportExcel() {
  if (people.length === 0) {
    showAlert('कोई डेटा नहीं है!', 'error');
    return;
  }

  let data = people.map(p => ({
    'नाम': p.name,
    'गांव': p.village,
    'रिश्ता': p.relation,
    'फोन': p.phone,
    'गोत्र': p.gotra,
    'जन्मदिन': p.birthday,
    'लिंग': p.gender,
    'मुखिया': p.familyHead ? getPersonName(p.familyHead) : 'खुद मुखिया',
    'नोट्स': p.notes
  }));

  let ws = XLSX.utils.json_to_sheet(data);
  let wb = XLSX.utils.book_new();
  XLSX.utils.book_append_sheet(wb, ws, "Rishtedar");
  XLSX.writeFile(wb, `rishtedar_data_${new Date().toISOString().slice(0,10)}.xlsx`);
  
  showAlert('✅ Excel एक्सपोर्ट हो गया!', 'success');
}

function exportPDF() {
  if (people.length === 0) {
    showAlert('कोई डेटा नहीं है!', 'error');
    return;
  }

  const { jsPDF } = window.jspdf;
  let doc = new jsPDF();
  
  doc.setFontSize(18);
  doc.text("रिश्तेदार सूची", 10, 10);
  doc.setFontSize(10);
  
  let y = 20;
  people.forEach((p, i) => {
    let familyHeadText = p.familyHead ? `(मुखिया: ${getPersonName(p.familyHead)})` : '(खुद मुखिया)';
    let line = `${i+1}. ${p.name} ${familyHeadText} - ${p.relation || 'रिश्ता'} - ${p.village} - ${p.phone || 'नंबर नहीं'}`;
    
    if (doc.getTextWidth(line) > 180) {
      let words = line.split(' ');
      let currentLine = '';
      words.forEach(word => {
        let testLine = currentLine + word + ' ';
        if (doc.getTextWidth(testLine) < 180) {
          currentLine = testLine;
        } else {
          doc.text(currentLine, 10, y);
          y += 5;
          currentLine = word + ' ';
        }
      });
      if (currentLine) {
        doc.text(currentLine, 10, y);
        y += 5;
      }
    } else {
      doc.text(line, 10, y);
      y += 5;
    }
    
    if (y > 280) {
      doc.addPage();
      y = 20;
    }
  });

  doc.save(`rishtedar_data_${new Date().toISOString().slice(0,10)}.pdf`);
  showAlert('✅ PDF एक्सपोर्ट हो गया!', 'success');
}

function printData() {
  window.print();
}
</script>

</body>
</html>
