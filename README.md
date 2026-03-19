<!DOCTYPE html>
<html lang="hi">
<head>
<meta charset="UTF-8">
<title>Rishtedar Manager</title>

<!-- Excel -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>

<!-- PDF -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

<style>
body { font-family: Arial; background:#f5f5f5; padding:10px; }

input, button {
  padding:8px; margin:5px; width:95%;
}

button { background:green; color:white; border:none; }

.card {
  background:white; padding:10px; margin:10px 0;
  border-radius:10px;
}

img { width:80px; height:80px; border-radius:50%; }

.action-btn {
  background:blue; margin-top:5px;
}
.delete-btn {
  background:red;
}
</style>
</head>

<body>

<h2>👨‍👩‍👧 रिश्तेदार जोड़ें</h2>

<input type="text" id="name" placeholder="नाम">
<input type="text" id="village" placeholder="गांव">
<input type="text" id="relation" placeholder="रिश्ता">
<input type="text" id="map" placeholder="Google Map Link">
<input type="file" id="photo">

<button onclick="saveData()">Save</button>

<hr>

<h3>🔍 Search</h3>
<input type="text" id="search" onkeyup="searchData()" placeholder="नाम या गांव">

<button onclick="showData()">Reset</button>
<button onclick="exportExcel()">Excel</button>
<button onclick="exportPDF()">PDF</button>

<div id="list"></div>

<script>
let people = JSON.parse(localStorage.getItem("people")) || [];
let editIndex = -1;

// Base64 photo
function getBase64(file, callback) {
  let reader = new FileReader();
  reader.onload = () => callback(reader.result);
  reader.readAsDataURL(file);
}

// SAVE / UPDATE
function saveData() {
  let name = nameEl.value;
  let village = villageEl.value;
  let relation = relationEl.value;
  let map = mapEl.value;
  let file = photoEl.files[0];

  if (!name || !village) {
    alert("नाम और गांव जरूरी है");
    return;
  }

  if (file) {
    getBase64(file, (base64) => processSave(base64));
  } else {
    processSave("");
  }
}

function processSave(photo) {
  let person = {
    name: nameEl.value,
    village: villageEl.value,
    relation: relationEl.value,
    map: mapEl.value,
    photo: photo
  };

  if (editIndex === -1) {
    people.push(person);
  } else {
    people[editIndex] = person;
    editIndex = -1;
  }

  localStorage.setItem("people", JSON.stringify(people));
  clearForm();
  showData();
}

// FORM CLEAR
function clearForm() {
  nameEl.value = "";
  villageEl.value = "";
  relationEl.value = "";
  mapEl.value = "";
  photoEl.value = "";
}

// DELETE
function deleteData(index) {
  if (confirm("Delete करना है?")) {
    people.splice(index, 1);
    localStorage.setItem("people", JSON.stringify(people));
    showData();
  }
}

// EDIT
function editData(index) {
  let p = people[index];

  nameEl.value = p.name;
  villageEl.value = p.village;
  relationEl.value = p.relation;
  mapEl.value = p.map;

  editIndex = index;
}

// SHOW
function showData(data = people) {
  list.innerHTML = "";

  let grouped = {};

  data.forEach((p, i) => {
    if (!grouped[p.village]) grouped[p.village] = [];
    grouped[p.village].push({ ...p, index: i });
  });

  for (let village in grouped) {
    list.innerHTML += `<h3>🏡 ${village}</h3>`;

    grouped[village].forEach(p => {
      list.innerHTML += `
      <div class="card">
        <img src="${p.photo || 'https://via.placeholder.com/80'}"><br>
        <b>${p.name}</b><br>
        रिश्ता: ${p.relation}<br>
        <a href="${p.map}" target="_blank">📍 Map</a><br>

        <button class="action-btn" onclick="editData(${p.index})">Edit</button>
        <button class="delete-btn" onclick="deleteData(${p.index})">Delete</button>
      </div>
      `;
    });
  }
}

// SEARCH
function searchData() {
  let input = search.value.toLowerCase();

  let filtered = people.filter(p =>
    p.name.toLowerCase().includes(input) ||
    p.village.toLowerCase().includes(input)
  );

  showData(filtered);
}

// EXCEL
function exportExcel() {
  let ws = XLSX.utils.json_to_sheet(people);
  let wb = XLSX.utils.book_new();
  XLSX.utils.book_append_sheet(wb, ws, "Data");
  XLSX.writeFile(wb, "data.xlsx");
}

// PDF
function exportPDF() {
  const { jsPDF } = window.jspdf;
  let doc = new jsPDF();

  people.forEach((p, i) => {
    doc.text(
      `${p.name} | ${p.village} | ${p.relation}`,
      10,
      10 + i * 10
    );
  });

  doc.save("data.pdf");
}

// shortcuts
let nameEl = document.getElementById("name");
let villageEl = document.getElementById("village");
let relationEl = document.getElementById("relation");
let mapEl = document.getElementById("map");
let photoEl = document.getElementById("photo");
let list = document.getElementById("list");
let search = document.getElementById("search");

showData();
</script>

</body>
</html>
