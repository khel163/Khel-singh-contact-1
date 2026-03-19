<!DOCTYPE html>
<html lang="hi">
<head>
<meta charset="UTF-8">
<title>Rishtedar Manager Final</title>

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

img {
  width:80px; height:80px; border-radius:50%;
}

.edit { background:blue; }
.delete { background:red; }
</style>
</head>

<body>

<h2>👨‍👩‍👧 रिश्तेदार मैनेजर</h2>

<input id="name" placeholder="नाम">
<input id="village" placeholder="गांव">
<input id="relation" placeholder="रिश्ता">
<input id="map" placeholder="Google Map Link">
<input type="file" id="photo">

<button onclick="saveData()">Save</button>

<hr>

<input id="search" placeholder="Search (नाम / गांव)" onkeyup="searchData()">

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

// SAVE
function saveData() {
  let name = nameEl.value.trim();
  let village = villageEl.value.trim();

  if (!name || !village) {
    alert("नाम और गांव जरूरी है");
    return;
  }

  let relation = relationEl.value;
  let map = mapEl.value;
  let file = photoEl.files[0];

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

// CLEAR
function clearForm() {
  nameEl.value = "";
  villageEl.value = "";
  relationEl.value = "";
  mapEl.value = "";
  photoEl.value = "";
}

// DELETE
function deleteData(i) {
  if (confirm("Delete करना है?")) {
    people.splice(i, 1);
    localStorage.setItem("people", JSON.stringify(people));
    showData();
  }
}

// EDIT
function editData(i) {
  let p = people[i];

  nameEl.value = p.name;
  villageEl.value = p.village;
  relationEl.value = p.relation;
  mapEl.value = p.map;

  editIndex = i;
}

// SHOW (group by village)
function showData(data = people) {
  list.innerHTML = "";

  if (data.length === 0) {
    list.innerHTML = "<p>कोई डेटा नहीं है</p>";
    return;
  }

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

        <button class="edit" onclick="editData(${p.index})">Edit</button>
        <button class="delete" onclick="deleteData(${p.index})">Delete</button>
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

// EXCEL (fixed)
function exportExcel() {
  if (typeof XLSX === "undefined") {
    alert("Internet ON करो (Excel library load नहीं हुई)");
    return;
  }

  if (people.length === 0) {
    alert("कोई डेटा नहीं है");
    return;
  }

  let data = people.map(p => ({
    Name: p.name,
    Village: p.village,
    Relation: p.relation,
    Map: p.map
  }));

  let ws = XLSX.utils.json_to_sheet(data);
  let wb = XLSX.utils.book_new();
  XLSX.utils.book_append_sheet(wb, ws, "Rishtedar");

  XLSX.writeFile(wb, "Rishtedar_Data.xlsx");
}

// PDF (better)
function exportPDF() {
  const { jsPDF } = window.jspdf;
  let doc = new jsPDF();

  doc.text("Rishtedar List", 10, 10);

  people.forEach((p, i) => {
    doc.text(
      `${i+1}. ${p.name} | ${p.village} | ${p.relation}`,
      10,
      20 + i * 10
    );
  });

  doc.save("Rishtedar.pdf");
}

// shortcut
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
