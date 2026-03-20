<!DOCTYPE html>
<html lang="hi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>📱 रिश्तेदार मैनेजर (Local Storage + Import/Export)</title>

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
  padding: 15px;
  margin-bottom: 20px;
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
  align-items: center;
}

.search-box {
  flex: 1;
  position: relative;
  min-width: 200px;
}

.search-box i {
  position: absolute;
  left: 10px;
  top: 50%;
  transform: translateY(-50%);
  color: #999;
}

.search-box input {
  padding-left: 35px;
  width: 100%;
}

.filter-select {
  width: 180px;
  padding: 8px 10px;
  border: 2px solid #e0e0e0;
  border-radius: 10px;
  font-size: 14px;
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

/* Main Content */
.main-content {
  display: grid;
  grid-template-columns: 1fr 300px;
  gap: 20px;
}

/* Cards Grid */
.cards-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
  gap: 20px;
}

.relative-card {
  background: white;
  border-radius: 15px;
  overflow: hidden;
  box-shadow: 0 5px 20px rgba(0,0,0,0.1);
  transition: all 0.3s;
  position: relative;
}

.relative-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 30px rgba(0,0,0,0.2);
}

.card-header {
  background: linear-gradient(135deg, #667eea, #764ba2);
  color: white;
  padding: 15px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.relation-badge {
  background: rgba(255,255,255,0.2);
  padding: 5px 10px;
  border-radius: 20px;
  font-size: 12px;
}

.card-body {
  padding: 15px;
  display: flex;
  gap: 15px;
}

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
}

.info {
  flex: 1;
}

.info-item {
  margin: 8px 0;
  display: flex;
  align-items: center;
  gap: 8px;
}

.info-item i {
  color: #667eea;
  width: 20px;
}

.card-footer {
  padding: 15px;
  border-top: 1px solid #eee;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 8px;
}

.action-btn {
  padding: 8px 15px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 12px;
  display: inline-flex;
  align-items: center;
  gap: 5px;
  transition: all 0.3s;
  justify-content: center;
}

.call-btn { background: #10b981; color: white; }
.edit-btn { background: #3b82f6; color: white; }
.delete-btn { background: #ef4444; color: white; }
.whatsapp-btn { background: #25D366; color: white; }

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
  .main-content {
    grid-template-columns: 1fr;
  }
  
  .form-grid {
    grid-template-columns: 1fr;
  }
  
  .btn-group {
    flex-direction: column;
  }
  
  .btn {
    width: 100%;
  }
  
  .card-footer {
    grid-template-columns: 1fr;
  }
  
  .search-section {
    flex-direction: column;
    align-items: stretch;
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

  <!-- Form Section -->
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
    </div>

    <div class="form-group" style="margin-bottom: 15px;">
      <i class="fas fa-sticky-note"></i>
      <textarea id="notes" placeholder="नोट्स / यादें ..." rows="2"></textarea>
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
    <div class="search-box">
      <i class="fas fa-search"></i>
      <input type="text" id="search" placeholder="नाम, गांव, फोन, रिश्ता से खोजें..." onkeyup="searchData()">
    </div>
    <select class="filter-select" id="filterVillage" onchange="filterByVillage()">
      <option value="">सभी गांव</option>
    </select>
    <select class="filter-select" id="filterRelation" onchange="filterByRelation()">
      <option value="">सभी रिश्ते</option>
    </select>
  </div>

  <!-- Import/Export Section (नया फीचर) -->
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
    <div id="list" class="cards-grid"></div>

    <!-- Sidebar -->
    <div class="sidebar">
      <!-- Upcoming Birthdays -->
      <div class="sidebar-card">
        <h3><i class="fas fa-birthday-cake" style="color: #f59e0b;"></i> आने वाले जन्मदिन</h3>
        <div id="upcomingBirthdays"></div>
      </div>

      <!-- Quick Stats -->
      <div class="sidebar-card">
        <h3><i class="fas fa-chart-pie"></i> रिश्तों के आंकड़े</h3>
        <canvas id="relationChart" style="height: 200px;"></canvas>
      </div>

      <!-- Gotra/Caste Stats -->
      <div class="sidebar-card">
        <h3><i class="fas fa-om"></i> गोत्र/जाति आंकड़े</h3>
        <div id="gotraStats"></div>
      </div>
      
      <!-- Backup Info -->
      <div class="sidebar-card">
        <h3><i class="fas fa-info-circle"></i> जानकारी</h3>
        <p style="font-size: 14px; color: #666;">
          <i class="fas fa-database"></i> डेटा आपके ब्राउज़र की localStorage में सेव है।<br>
          <i class="fas fa-download"></i> JSON एक्सपोर्ट करके फोन में सेव कर सकते हैं।<br>
          <i class="fas fa-upload"></i> सेव की हुई फाइल इंपोर्ट कर सकते हैं।
        </p>
      </div>
    </div>
  </div>
</div>

<script>
// ==================== GLOBAL VARIABLES ====================
let people = [];
let editIndex = -1;

// DOM Elements
let nameEl, villageEl, relationEl, phoneEl, mapEl, birthdayEl, gotraEl, genderEl, notesEl;
let list, search, filterVillage, filterRelation;

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
  notesEl = document.getElementById("notes");
  list = document.getElementById("list");
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
  updateUI();
}

// Save data to localStorage
function saveToStorage() {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(people));
  updateStorageStatus();
}

// Update all UI components
function updateUI() {
  updateFilters();
  showData();
  updateStats();
  updateUpcomingBirthdays();
  updateGotraStats();
  initChart();
  updateStorageStatus();
}

// Update storage status
function updateStorageStatus() {
  document.getElementById('storageCount').textContent = people.length;
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

// ==================== SAVE DATA ====================
function saveData() {
  let name = nameEl.value.trim();
  let village = villageEl.value.trim();

  if (!name || !village) {
    showAlert('❌ नाम और गांव जरूरी है', 'error');
    return;
  }

  let person = {
    id: Date.now() + Math.random(),
    name: name,
    village: village,
    relation: relationEl.value,
    phone: phoneEl.value,
    map: mapEl.value,
    birthday: birthdayEl.value,
    gotra: gotraEl.value,
    gender: genderEl.value,
    notes: notesEl.value,
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
  updateUI();
}

// ==================== CLEAR FORM ====================
function clearForm() {
  nameEl.value = "";
  villageEl.value = "";
  relationEl.value = "";
  phoneEl.value = "";
  mapEl.value = "";
  birthdayEl.value = "";
  gotraEl.value = "";
  genderEl.value = "";
  notesEl.value = "";
  editIndex = -1;
}

// ==================== DELETE DATA ====================
function deleteData(id) {
  if (confirm("क्या आप सच में डिलीट करना चाहते हैं?")) {
    people = people.filter(p => p.id != id);
    saveToStorage();
    updateUI();
    showAlert('✅ डेटा डिलीट हो गया!', 'success');
  }
}

// ==================== CLEAR ALL DATA ====================
function clearAllData() {
  if (confirm("⚠️ क्या आप सच में सारा डेटा डिलीट करना चाहते हैं? यह क्रिया वापस नहीं की जा सकती!")) {
    people = [];
    saveToStorage();
    updateUI();
    showAlert('✅ सारा डेटा डिलीट हो गया!', 'success');
  }
}

// ==================== EDIT DATA ====================
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
  notesEl.value = p.notes || "";

  editIndex = index;
  
  // Scroll to form
  document.querySelector(".form-section").scrollIntoView({ behavior: "smooth" });
}

// ==================== DISPLAY DATA ====================
function showData(filteredData = null) {
  let dataToShow = filteredData || people;
  
  if (dataToShow.length === 0) {
    list.innerHTML = `
      <div style="grid-column: 1/-1; text-align: center; padding: 50px;">
        <i class="fas fa-users-slash" style="font-size: 50px; color: #ccc;"></i>
        <p style="margin-top: 20px;">कोई डेटा नहीं है</p>
      </div>
    `;
    return;
  }

  // Group by village
  let grouped = {};
  dataToShow.forEach(p => {
    if (!grouped[p.village]) grouped[p.village] = [];
    grouped[p.village].push(p);
  });

  let html = "";
  
  Object.keys(grouped).sort().forEach(village => {
    html += `<h3 style="grid-column: 1/-1; color: white; margin: 10px 0;">
      <i class="fas fa-home"></i> ${village} (${grouped[village].length})
    </h3>`;

    grouped[village].forEach(p => {
      let age = p.birthday ? calculateAge(p.birthday) : "";
      
      html += `
      <div class="relative-card" data-id="${p.id}">
        <div class="card-header">
          <span class="relation-badge">
            <i class="fas fa-${p.gender === 'पुरुष' ? 'mars' : p.gender === 'महिला' ? 'venus' : 'genderless'}"></i>
            ${p.relation || 'रिश्ता'}
          </span>
        </div>
        <div class="card-body">
          <div class="avatar">
            <i class="fas fa-user-circle"></i>
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
            <div class="info-item">
              <i class="fas fa-om"></i>
              <span>${p.gotra || 'गोत्र नहीं'}</span>
            </div>
            ${age ? `
            <div class="info-item">
              <i class="fas fa-calendar"></i>
              <span>${age}</span>
            </div>
            ` : ''}
            <div class="tags">
              ${p.birthday ? `<span class="tag"><i class="fas fa-birthday-cake"></i> ${formatDate(p.birthday)}</span>` : ''}
            </div>
          </div>
        </div>
        <div class="card-footer">
          ${p.phone ? `
          <a href="tel:${p.phone}" class="action-btn call-btn">
            <i class="fas fa-phone-alt"></i> कॉल
          </a>
          <a href="https://wa.me/${p.phone.replace(/\D/g,'')}" target="_blank" class="action-btn whatsapp-btn">
            <i class="fab fa-whatsapp"></i> WhatsApp
          </a>
          ` : ''}
          <button class="action-btn edit-btn" onclick="editData('${p.id}')">
            <i class="fas fa-edit"></i> एडिट
          </button>
          <button class="action-btn delete-btn" onclick="deleteData('${p.id}')">
            <i class="fas fa-trash"></i> डिलीट
          </button>
        </div>
        ${p.notes ? `
        <div style="padding: 10px; background: #f8f9fa; border-top: 1px solid #eee; font-size: 12px;">
          <i class="fas fa-sticky-note"></i> ${p.notes}
        </div>
        ` : ''}
      </div>
      `;
    });
  });

  list.innerHTML = html;
}

// ==================== HELPER FUNCTIONS ====================
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

// ==================== SEARCH & FILTER ====================
function searchData() {
  let term = search.value.toLowerCase();
  let filtered = people.filter(p => 
    p.name.toLowerCase().includes(term) ||
    p.village.toLowerCase().includes(term) ||
    (p.phone && p.phone.includes(term)) ||
    (p.relation && p.relation.toLowerCase().includes(term)) ||
    (p.gotra && p.gotra.toLowerCase().includes(term))
  );
  showData(filtered);
}

function filterByVillage() {
  let village = filterVillage.value;
  if (!village) {
    showData();
    return;
  }
  let filtered = people.filter(p => p.village === village);
  showData(filtered);
}

function filterByRelation() {
  let relation = filterRelation.value;
  if (!relation) {
    showData();
    return;
  }
  let filtered = people.filter(p => p.relation === relation);
  showData(filtered);
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

// ==================== STATS ====================
function updateStats() {
  document.getElementById("totalCount").textContent = people.length;
  
  let families = new Set(people.map(p => p.village)).size;
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
    html += `
    <div class="upcoming-item">
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

function updateGotraStats() {
  let gotras = {};
  people.forEach(p => {
    if (p.gotra) {
      gotras[p.gotra] = (gotras[p.gotra] || 0) + 1;
    }
  });

  let sortedGotras = Object.entries(gotras).sort((a, b) => b[1] - a[1]);
  
  let html = "";
  sortedGotras.forEach(([gotra, count]) => {
    html += `
    <div style="display: flex; justify-content: space-between; padding: 5px 0; border-bottom: 1px solid #eee;">
      <span><i class="fas fa-om"></i> ${gotra}</span>
      <span class="badge">${count}</span>
    </div>
    `;
  });

  if (sortedGotras.length === 0) {
    html = '<p style="color: #999; text-align: center;">कोई गोत्र डेटा नहीं</p>';
  }

  document.getElementById("gotraStats").innerHTML = html;
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

// ==================== EXPORT/IMPORT FUNCTIONS (नया फीचर) ====================

// JSON फाइल में एक्सपोर्ट करें
function exportToFile() {
  if (people.length === 0) {
    showAlert('कोई डेटा नहीं है!', 'error');
    return;
  }

  // डेटा को JSON में बदलें
  const dataStr = JSON.stringify(people, null, 2);
  
  // ब्लॉब बनाएँ
  const blob = new Blob([dataStr], { type: 'application/json' });
  
  // डाउनलोड लिंक बनाएँ
  const url = URL.createObjectURL(blob);
  const link = document.createElement('a');
  link.href = url;
  link.download = `rishtedar_backup_${new Date().toISOString().slice(0,10)}.json`;
  
  // डाउनलोड करें
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
  
  // URL रिलीज करें
  URL.revokeObjectURL(url);
  
  showAlert('✅ डेटा एक्सपोर्ट हो गया! फोन में सेव कर लें।', 'success');
}

// JSON फाइल से इंपोर्ट करें
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
      
      // वैलिडेशन - चेक करें कि यह ऐरे है या नहीं
      if (!Array.isArray(importedData)) {
        throw new Error('फाइल फॉर्मेट सही नहीं है');
      }
      
      // कन्फर्मेशन
      if (confirm(`क्या आप ${importedData.length} रिकॉर्ड इंपोर्ट करना चाहते हैं? मौजूदा डेटा मर्ज हो जाएगा।`)) {
        
        // नए आईडी जेनरेट करें
        importedData.forEach(item => {
          if (!item.id) {
            item.id = Date.now() + Math.random();
          }
        });
        
        // मौजूदा डेटा में मर्ज करें
        people = [...people, ...importedData];
        
        // डुप्लिकेट हटाएँ (अगर कोई हो)
        const uniqueIds = new Set();
        people = people.filter(item => {
          if (uniqueIds.has(item.id)) {
            return false;
          }
          uniqueIds.add(item.id);
          return true;
        });
        
        // सेव करें
        saveToStorage();
        updateUI();
        
        fileInput.value = ''; // फाइल इनपुट क्लियर करें
        showAlert(`✅ ${importedData.length} रिकॉर्ड इंपोर्ट हो गए!`, 'success');
      }
      
    } catch (error) {
      showAlert('फाइल इंपोर्ट करने में समस्या हुई! सही JSON फाइल चुनें।', 'error');
      console.error(error);
    }
  };
  
  reader.readAsText(file);
}

// Excel एक्सपोर्ट
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
    'नोट्स': p.notes
  }));

  let ws = XLSX.utils.json_to_sheet(data);
  let wb = XLSX.utils.book_new();
  XLSX.utils.book_append_sheet(wb, ws, "Rishtedar");
  XLSX.writeFile(wb, `rishtedar_data_${new Date().toISOString().slice(0,10)}.xlsx`);
  
  showAlert('✅ Excel एक्सपोर्ट हो गया!', 'success');
}

// PDF एक्सपोर्ट
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
    let line = `${i+1}. ${p.name} (${p.relation || 'रिश्ता'}) - ${p.village} - ${p.phone || 'नंबर नहीं'}`;
    
    // लंबी लाइनों को तोड़ें
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
