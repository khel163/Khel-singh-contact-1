<!DOCTYPE html>
<html lang="hi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>🚀 खेल परिवार - रिश्तेदार मैनेजर</title>

<!-- Libraries -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdn.jsdelivr.net/npm/party-js@latest/bundle/party.min.js"></script>

<!-- Firebase SDKs -->
<script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-database-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-storage-compat.js"></script>

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
  box-shadow: 0 5px 15px rgba(0,0,0,0.2);
}

.stat-card i { font-size: 24px; margin-bottom: 5px; }
.stat-card span { display: block; font-size: 20px; font-weight: bold; }

/* Sync Status */
.sync-status {
  display: flex;
  align-items: center;
  gap: 10px;
  background: #f0f0f0;
  padding: 8px 15px;
  border-radius: 50px;
  font-size: 14px;
}

.sync-status i.fa-sync-alt { color: #f59e0b; }
.sync-status i.fa-check-circle { color: #10b981; }
.sync-status i.fa-exclamation-circle { color: #ef4444; }

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
  cursor: pointer;
  transition: transform 0.3s;
}

.avatar:hover {
  transform: scale(1.05);
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
  transition: transform 0.3s;
}

.upcoming-item:hover {
  transform: translateX(5px);
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

/* Loading Spinner */
.loading-spinner {
  display: none;
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 0 30px rgba(0,0,0,0.3);
  z-index: 3000;
  text-align: center;
}

.loading-spinner i {
  font-size: 40px;
  color: #667eea;
  margin-bottom: 10px;
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
}

/* Animations */
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

.relative-card {
  animation: fadeIn 0.5s ease-out;
}
</style>
</head>
<body>

<div class="container">
  <!-- Header -->
  <div class="header">
    <div style="display: flex; align-items: center; gap: 20px; flex-wrap: wrap;">
      <h2><i class="fas fa-people-arrows"></i> खेल परिवार - रिश्तेदार मैनेजर</h2>
      <div class="sync-status" id="syncStatus">
        <i class="fas fa-sync-alt fa-spin"></i>
        <span>कनेक्ट हो रहा है...</span>
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
          <optgroup label="👨‍👩‍👧‍👦 परिवार (Family)">
            <option value="पिता">पिता (Father)</option>
            <option value="माता">माता (Mother)</option>
            <option value="भाई">भाई (Brother)</option>
            <option value="बहन">बहन (Sister)</option>
            <option value="दादा">दादा (Grandfather)</option>
            <option value="दादी">दादी (Grandmother)</option>
            <option value="नाना">नाना (Maternal Grandfather)</option>
            <option value="नानी">नानी (Maternal Grandmother)</option>
            <option value="बेटा">बेटा (Son)</option>
            <option value="बेटी">बेटी (Daughter)</option>
          </optgroup>
          <optgroup label="💍 वैवाहिक रिश्ते (Marital)">
            <option value="पति">पति (Husband)</option>
            <option value="पत्नी">पत्नी (Wife)</option>
            <option value="ससुर">ससुर (Father-in-law)</option>
            <option value="सास">सास (Mother-in-law)</option>
            <option value="देवर">देवर (Husband's younger brother)</option>
            <option value="जेठ">जेठ (Husband's elder brother)</option>
            <option value="ननद">ननद (Husband's sister)</option>
            <option value="बहू">बहू (Daughter-in-law)</option>
            <option value="दामाद">दामाद (Son-in-law)</option>
          </optgroup>
          <optgroup label="👶 खून के रिश्ते (Blood Relations)">
            <option value="चाचा">चाचा (Uncle - Father's younger brother)</option>
            <option value="ताऊ">ताऊ (Father's elder brother)</option>
            <option value="बुआ">बुआ (Father's sister)</option>
            <option value="मामा">मामा (Mother's brother)</option>
            <option value="मौसी">मौसी (Mother's sister)</option>
            <option value="चचेरा भाई">चचेरा भाई (Cousin - Paternal)</option>
            <option value="चचेरी बहन">चचेरी बहन (Cousin - Paternal)</option>
            <option value="ममेरा भाई">ममेरा भाई (Cousin - Maternal)</option>
            <option value="ममेरी बहन">ममेरी बहन (Cousin - Maternal)</option>
          </optgroup>
          <optgroup label="🤝 सामाजिक रिश्ते (Social)">
            <option value="दोस्त">दोस्त (Friend)</option>
            <option value="पड़ोसी">पड़ोसी (Neighbor)</option>
            <option value="गुरु">गुरु (Teacher)</option>
            <option value="शिष्य">शिष्य (Student)</option>
            <option value="सहकर्मी">सहकर्मी (Colleague)</option>
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
        <input type="text" id="gotra" placeholder="गोत्र/जाति (Gotra/Caste)">
      </div>
      <div class="form-group">
        <i class="fas fa-venus-mars"></i>
        <select id="gender">
          <option value="">लिंग चुनें</option>
          <option value="पुरुष">पुरुष (Male)</option>
          <option value="महिला">महिला (Female)</option>
          <option value="अन्य">अन्य (Other)</option>
        </select>
      </div>
      <div class="form-group">
        <i class="fas fa-link"></i>
        <select id="familyHead">
          <option value="">परिवार मुखिया चुनें</option>
        </select>
      </div>
    </div>

    <div class="form-group" style="margin-bottom: 15px;">
      <i class="fas fa-sticky-note"></i>
      <textarea id="notes" placeholder="नोट्स / यादें / खास बातें ..." rows="2"></textarea>
    </div>

    <div class="form-group">
      <i class="fas fa-camera"></i>
      <input type="file" id="photo" accept="image/*" style="padding: 12px;">
      <small style="display: block; margin-top: 5px; color: #666;">अधिकतम 5MB (JPG, PNG)</small>
    </div>

    <div class="btn-group">
      <button class="btn btn-primary" onclick="saveData()">
        <i class="fas fa-save"></i> सेव करें
      </button>
      <button class="btn btn-success" onclick="clearForm()">
        <i class="fas fa-undo"></i> क्लियर
      </button>
      <button class="btn btn-info" onclick="loadFromFirebase()">
        <i class="fas fa-sync-alt"></i> रिफ्रेश
      </button>
    </div>
  </div>

  <!-- Search Section -->
  <div class="search-section">
    <div class="search-box">
      <i class="fas fa-search"></i>
      <input type="text" id="search" placeholder="नाम, गांव, फोन, रिश्ता, गोत्र से खोजें..." onkeyup="searchData()">
    </div>
    <select class="filter-select" id="filterVillage" onchange="filterByVillage()">
      <option value="">सभी गांव</option>
    </select>
    <select class="filter-select" id="filterRelation" onchange="filterByRelation()">
      <option value="">सभी रिश्ते</option>
    </select>
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
  </div>

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
    </div>
  </div>
</div>

<!-- Photo Modal -->
<div id="photoModal" class="photo-modal" onclick="closePhotoModal()">
  <span class="close-modal">&times;</span>
  <img id="modalImage" src="" alt="बड़ी फोटो">
</div>

<!-- Loading Spinner -->
<div id="loadingSpinner" class="loading-spinner">
  <i class="fas fa-circle-notch fa-spin"></i>
  <p>कृपया प्रतीक्षा करें...</p>
</div>

<script>
// ==================== FIREBASE CONFIGURATION (आपका दिया हुआ) ====================
const firebaseConfig = {
  apiKey: "AIzaSyAIcK_AaaQSqCF_-4L9CPkBvNuD1EkQpcU",
  authDomain: "khel-family.firebaseapp.com",
  projectId: "khel-family",
  storageBucket: "khel-family.firebasestorage.app",
  messagingSenderId: "306385476526",
  appId: "1:306385476526:web:8cf4df596a36b48f68863f",
  measurementId: "G-FCNXL01ZKF"
};

// Initialize Firebase
firebase.initializeApp(firebaseConfig);
const database = firebase.database();
const storage = firebase.storage();
const peopleRef = database.ref('people');

// ==================== GLOBAL VARIABLES ====================
let people = [];
let editIndex = -1;
let currentFilteredData = null;
let isLoading = false;

// DOM Elements
let nameEl, villageEl, relationEl, phoneEl, mapEl, birthdayEl, gotraEl, genderEl, familyHeadEl, notesEl, photoEl;
let list, search, filterVillage, filterRelation;

// Chart instance
let relationChart = null;

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
  list = document.getElementById("list");
  search = document.getElementById("search");
  filterVillage = document.getElementById("filterVillage");
  filterRelation = document.getElementById("filterRelation");
  
  // Load data from Firebase
  loadFromFirebase();
  
  // Listen for real-time updates
  peopleRef.on('value', (snapshot) => {
    if (!isLoading) {
      const data = snapshot.val();
      if (data) {
        // Convert object to array
        people = Object.keys(data).map(key => ({
          firebaseKey: key,
          id: key, // Use firebase key as id
          ...data[key]
        }));
      } else {
        people = [];
      }
      updateUI();
      updateSyncStatus('synced');
    }
  });
});

// Show/Hide Loading Spinner
function showLoading(show) {
  document.getElementById('loadingSpinner').style.display = show ? 'block' : 'none';
}

// Update sync status
function updateSyncStatus(status) {
  const syncEl = document.getElementById('syncStatus');
  if (status === 'syncing') {
    syncEl.innerHTML = '<i class="fas fa-sync-alt fa-spin"></i> <span>सिंक हो रहा है...</span>';
  } else if (status === 'synced') {
    syncEl.innerHTML = '<i class="fas fa-check-circle" style="color: #10b981;"></i> <span>सिंक हो गया</span>';
  } else if (status === 'error') {
    syncEl.innerHTML = '<i class="fas fa-exclamation-circle" style="color: #ef4444;"></i> <span>सिंक एरर</span>';
  }
}

// Load data from Firebase
function loadFromFirebase() {
  isLoading = true;
  updateSyncStatus('syncing');
  showLoading(true);
  
  peopleRef.once('value', (snapshot) => {
    const data = snapshot.val();
    if (data) {
      // Convert object to array
      people = Object.keys(data).map(key => ({
        firebaseKey: key,
        id: key,
        ...data[key]
      }));
    } else {
      people = [];
    }
    
    isLoading = false;
    showLoading(false);
    updateUI();
    updateSyncStatus('synced');
  }, (error) => {
    console.error("Firebase load error:", error);
    updateSyncStatus('error');
    isLoading = false;
    showLoading(false);
    alert("Firebase से डेटा लोड करने में समस्या हुई! कृपया इंटरनेट चेक करें।");
  });
}

// Update all UI components
function updateUI() {
  updateFamilyHeadOptions();
  updateFilters();
  showData();
  updateStats();
  updateUpcomingBirthdays();
  updateGotraStats();
  initChart();
}

// ==================== PHOTO HANDLING ====================
async function uploadPhoto(file) {
  if (!file) return "";
  
  // Check file size (max 5MB)
  if (file.size > 5 * 1024 * 1024) {
    alert("फोटो का साइज 5MB से कम होना चाहिए!");
    return "";
  }
  
  // Check file type
  if (!file.type.startsWith('image/')) {
    alert("कृपया केवल फोटो फाइल अपलोड करें!");
    return "";
  }
  
  showLoading(true);
  updateSyncStatus('syncing');
  
  try {
    // Create a unique filename
    const timestamp = Date.now();
    const fileName = `photos/${timestamp}_${file.name}`;
    const storageRef = storage.ref(fileName);
    
    // Upload file
    await storageRef.put(file);
    
    // Get download URL
    const downloadURL = await storageRef.getDownloadURL();
    
    showLoading(false);
    updateSyncStatus('synced');
    return downloadURL;
  } catch (error) {
    console.error("Photo upload error:", error);
    showLoading(false);
    updateSyncStatus('error');
    alert("फोटो अपलोड करने में समस्या हुई!");
    return "";
  }
}

// Show photo in modal
function showPhoto(photoUrl) {
  if (!photoUrl) return;
  document.getElementById('modalImage').src = photoUrl;
  document.getElementById('photoModal').style.display = 'flex';
}

function closePhotoModal() {
  document.getElementById('photoModal').style.display = 'none';
}

// ==================== SAVE DATA ====================
async function saveData() {
  let name = nameEl.value.trim();
  let village = villageEl.value.trim();

  if (!name || !village) {
    alert("❌ नाम और गांव जरूरी है");
    return;
  }

  let file = photoEl.files[0];
  let photoURL = "";

  if (file) {
    photoURL = await uploadPhoto(file);
    if (!photoURL) return; // Upload failed
  } else {
    // If editing, keep old photo
    if (editIndex !== -1 && people[editIndex]) {
      photoURL = people[editIndex].photo || "";
    }
  }

  processSave(photoURL);
}

function processSave(photo) {
  updateSyncStatus('syncing');
  showLoading(true);
  
  let person = {
    name: nameEl.value,
    village: villageEl.value,
    relation: relationEl.value,
    phone: phoneEl.value,
    map: mapEl.value,
    birthday: birthdayEl.value,
    gotra: gotraEl.value,
    gender: genderEl.value,
    familyHead: familyHeadEl.value,
    notes: notesEl.value,
    photo: photo,
    updatedAt: new Date().toISOString()
  };

  if (editIndex === -1) {
    // Add new person to Firebase
    person.createdAt = new Date().toISOString();
    peopleRef.push(person)
      .then(() => {
        clearForm();
        showLoading(false);
        updateSyncStatus('synced');
        party.confetti();
        alert("✅ डेटा सफलतापूर्वक सेव हो गया!");
      })
      .catch((error) => {
        console.error("Firebase save error:", error);
        showLoading(false);
        updateSyncStatus('error');
        alert("डेटा सेव करने में समस्या हुई! कृपया पुनः प्रयास करें।");
      });
  } else {
    // Update existing person in Firebase
    const firebaseKey = people[editIndex].firebaseKey;
    peopleRef.child(firebaseKey).update(person)
      .then(() => {
        clearForm();
        editIndex = -1;
        showLoading(false);
        updateSyncStatus('synced');
        alert("✅ डेटा अपडेट हो गया!");
      })
      .catch((error) => {
        console.error("Firebase update error:", error);
        showLoading(false);
        updateSyncStatus('error');
        alert("डेटा अपडेट करने में समस्या हुई! कृपया पुनः प्रयास करें।");
      });
  }
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
  familyHeadEl.value = "";
  notesEl.value = "";
  photoEl.value = "";
  editIndex = -1;
}

// ==================== DELETE DATA ====================
function deleteData(id) {
  if (confirm("क्या आप सच में डिलीट करना चाहते हैं?")) {
    updateSyncStatus('syncing');
    showLoading(true);
    
    // Find the person by id
    const person = people.find(p => p.id === id);
    if (!person || !person.firebaseKey) {
      showLoading(false);
      return;
    }
    
    // Delete from Firebase
    peopleRef.child(person.firebaseKey).remove()
      .then(() => {
        showLoading(false);
        updateSyncStatus('synced');
        alert("✅ डेटा डिलीट हो गया!");
      })
      .catch((error) => {
        console.error("Firebase delete error:", error);
        showLoading(false);
        updateSyncStatus('error');
        alert("डेटा डिलीट करने में समस्या हुई!");
      });
  }
}

// ==================== EDIT DATA ====================
function editData(id) {
  let index = people.findIndex(p => p.id === id);
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

// ==================== DISPLAY DATA ====================
function showData(filteredData = null) {
  let dataToShow = filteredData || people;
  currentFilteredData = filteredData;
  
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

  let html =
