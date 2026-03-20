<!DOCTYPE html>
<html lang="hi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>🚀 एडवांस रिश्तेदार मैनेजर</title>

<!-- Libraries -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdn.jsdelivr.net/npm/party-js@latest/bundle/party.min.js"></script>

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
.family-btn { background: #8b5cf6; color: white; }

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

/* Family Tree Modal */
.modal {
  display: none;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0,0,0,0.5);
  z-index: 1000;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background: white;
  border-radius: 20px;
  padding: 20px;
  max-width: 800px;
  width: 90%;
  max-height: 80vh;
  overflow-y: auto;
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
}
</style>
</head>
<body>

<div class="container">
  <!-- Header -->
  <div class="header">
    <h2><i class="fas fa-people-arrows"></i> एडवांस रिश्तेदार मैनेजर</h2>
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
    </div>

    <div class="btn-group">
      <button class="btn btn-primary" onclick="saveData()">
        <i class="fas fa-save"></i> सेव करें
      </button>
      <button class="btn btn-success" onclick="clearForm()">
        <i class="fas fa-undo"></i> क्लियर
      </button>
      <button class="btn btn-info" onclick="init()">
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
    <select id="filterVillage" onchange="filterByVillage()" style="width: 200px; padding-left: 10px;">
      <option value="">सभी गांव</option>
    </select>
    <select id="filterRelation" onchange="filterByRelation()" style="width: 200px; padding-left: 10px;">
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
      <button class="btn btn-info" onclick="backupData()">
        <i class="fas fa-download"></i> Backup
      </button>
      <button class="btn btn-primary" onclick="restoreData()">
        <i class="fas fa-upload"></i> Restore
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

<script>
let people = JSON.parse(localStorage.getItem("advancePeople")) || [];
let editIndex = -1;

// DOM Elements
let nameEl = document.getElementById("name");
let villageEl = document.getElementById("village");
let relationEl = document.getElementById("relation");
let phoneEl = document.getElementById("phone");
let mapEl = document.getElementById("map");
let birthdayEl = document.getElementById("birthday");
let gotraEl = document.getElementById("gotra");
let genderEl = document.getElementById("gender");
let familyHeadEl = document.getElementById("familyHead");
let notesEl = document.getElementById("notes");
let photoEl = document.getElementById("photo");
let list = document.getElementById("list");
let search = document.getElementById("search");
let filterVillage = document.getElementById("filterVillage");
let filterRelation = document.getElementById("filterRelation");

// Chart instance
let relationChart = null;

// Initialize
function init() {
  updateFamilyHeadOptions();
  updateFilters();
  showData();
  updateStats();
  updateUpcomingBirthdays();
  updateGotraStats();
  initChart();
}

// Photo to Base64
function getBase64(file, callback) {
  let reader = new FileReader();
  reader.onload = () => callback(reader.result);
  reader.readAsDataURL(file);
}

// Save Data
function saveData() {
  let name = nameEl.value.trim();
  let village = villageEl.value.trim();

  if (!name || !village) {
    party.confetti();
    alert("❌ नाम और गांव जरूरी है");
    return;
  }

  let file = photoEl.files[0];

  if (file) {
    getBase64(file, (base64) => processSave(base64));
  } else {
    processSave(people[editIndex]?.photo || "");
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
    familyHead: familyHeadEl.value,
    notes: notesEl.value,
    photo: photo,
    createdAt: new Date().toISOString()
  };

  if (editIndex === -1) {
    people.push(person);
    party.confetti();
  } else {
    people[editIndex] = person;
    editIndex = -1;
  }

  localStorage.setItem("advancePeople", JSON.stringify(people));
  clearForm();
  init();
}

// Clear Form
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

// Delete with animation
function deleteData(id) {
  if (confirm("क्या आप सच में डिलीट करना चाहते हैं?")) {
    let card = document.querySelector(`[data-id="${id}"]`);
    if (card) {
      card.style.transform = "scale(0)";
      card.style.opacity = "0";
      setTimeout(() => {
        people = people.filter(p => p.id !== id);
        localStorage.setItem("advancePeople", JSON.stringify(people));
        init();
      }, 300);
    } else {
      people = people.filter(p => p.id !== id);
      localStorage.setItem("advancePeople", JSON.stringify(people));
      init();
    }
  }
}

// Edit
function editData(id) {
  let index = people.findIndex(p => p.id === id);
  if (index === -1) return;

  let p = people[index];
  nameEl.value = p.name;
  villageEl.value = p.village;
  relationEl.value = p.relation;
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

// Show Data
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
  
  // Sort villages
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
          <img src="${p.photo || 'https://via.placeholder.com/80?text=' + p.name.charAt(0)}" class="avatar" 
               onerror="this.src='https://via.placeholder.com/80?text=' + this.alt" alt="${p.name}">
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
              ${p.familyHead ? `<span class="tag"><i class="fas fa-crown"></i> मुखिया</span>` : ''}
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
          <button class="action-btn edit-btn" onclick="editData(${p.id})">
            <i class="fas fa-edit"></i> एडिट
          </button>
          <button class="action-btn delete-btn" onclick="deleteData(${p.id})">
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

// Calculate Age
function calculateAge(birthday) {
  let birthDate = new Date(birthday);
  let today = new Date();
  let age = today.getFullYear() - birthDate.getFullYear();
  let m = today.getMonth() - birthDate.getMonth();
  if (m < 0 || (m === 0 && today.getDate() < birthDate.getDate())) age--;
  return age + " साल";
}

// Format Date
function formatDate(date) {
  let d = new Date(date);
  return d.toLocaleDateString('hi-IN', { day: 'numeric', month: 'short' });
}

// Search
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

// Filter by Village
function filterByVillage() {
  let village = filterVillage.value;
  if (!village) {
    showData();
    return;
  }
  let filtered = people.filter(p => p.village === village);
  showData(filtered);
}

// Filter by Relation
function filterByRelation() {
  let relation = filterRelation.value;
  if (!relation) {
    showData();
    return;
  }
  let filtered = people.filter(p => p.relation === relation);
  showData(filtered);
}

// Update Filters
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

// Update Family Head Options
function updateFamilyHeadOptions() {
  familyHeadEl.innerHTML = '<option value="">कोई नहीं (खुद मुखिया)</option>';
  people.forEach(p => {
    familyHeadEl.innerHTML += `<option value="${p.id}">${p.name} (${p.village})</option>`;
  });
}

// Update Stats
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

// Update Upcoming Birthdays
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

// Update Gotra Stats
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

// Initialize Chart
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
        backgroundColor: ['
