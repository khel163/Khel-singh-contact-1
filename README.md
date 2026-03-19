<!DOCTYPE html>
<html lang="hi">
<head>
<meta charset="UTF-8">
<title>Rishtedar App Firebase</title>

<!-- Excel -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>

<!-- PDF -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

<style>
body { font-family: Arial; background:#f5f5f5; padding:10px; }
input, button { padding:8px; margin:5px; width:95%; }
button { background:green; color:white; border:none; }

.card {
  background:white; padding:10px; margin:10px 0;
  border-radius:10px;
}
img { width:80px; height:80px; border-radius:50%; }

.edit { background:blue; }
.delete { background:red; }
.call { background:purple; }
</style>
</head>

<body>

<h2>👨‍👩‍👧 रिश्तेदार मैनेजर (Online)</h2>

<input id="name" placeholder="नाम">
<input id="village" placeholder="गांव">
<input id="relation" placeholder="रिश्ता">
<input id="phone" placeholder="मोबाइल नंबर">
<input id="map" placeholder="Google Map Link">
<input type="file" id="photo">

<button onclick="saveData()">Save</button>

<hr>

<input id="search" placeholder="Search (नाम / गांव / फोन)" onkeyup="searchData()">

<button onclick="loadData()">Reset</button>
<button onclick="exportExcel()">Excel</button>
<button onclick="exportPDF()">PDF</button>

<div id="list"></div>

<!-- Firebase -->
<script type="module">

import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
import {
  getFirestore, collection, addDoc, getDocs,
  deleteDoc, doc, updateDoc
} from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

const firebaseConfig = {
  apiKey: "AIzaSyBcm4mLT1OuToCUKZJgrRSjOEizOP34aTQ",
  authDomain: "khel-singh-contact.firebaseapp.com",
  projectId: "khel-singh-contact",
  storageBucket: "khel-singh-contact.firebasestorage.app",
  messagingSenderId: "409710118100",
  appId: "1:409710118100:web:fadec48695b0010620e741"
};

const app = initializeApp(firebaseConfig);
const db = getFirestore(app);

let people = [];
let editId = null;

// photo base64
function getBase64(file) {
  return new Promise((resolve) => {
    let reader = new FileReader();
    reader.onload = () => resolve(reader.result);
    reader.readAsDataURL(file);
  });
}

// SAVE
window.saveData = async function() {
  let name = nameEl.value.trim();
  let village = villageEl.value.trim();

  if (!name || !village) {
    alert("नाम और गांव जरूरी है");
    return;
  }

  let photo = "";
  if (photoEl.files[0]) {
    photo = await getBase64(photoEl.files[0]);
  }

  let person = {
    name,
    village,
    relation: relationEl.value,
    phone: phoneEl.value,
    map: mapEl.value,
    photo
  };

  if (editId) {
    await updateDoc(doc(db, "people", editId), person);
    editId = null;
  } else {
    await addDoc(collection(db, "people"), person);
  }

  clearForm();
  loadData();
}

// LOAD DATA
window.loadData = async function() {
  people = [];
  let snapshot = await getDocs(collection(db, "people"));

  snapshot.forEach(docSnap => {
    people.push({ id: docSnap.id, ...docSnap.data() });
  });

  showData(people);
}

// DELETE
window.deleteData = async function(id) {
  if (confirm("Delete करना है?")) {
    await deleteDoc(doc(db, "people", id));
    loadData();
  }
}

// EDIT
window.editData = function(p) {
  nameEl.value = p.name;
  villageEl.value = p.village;
  relationEl.value = p.relation;
  phoneEl.value = p.phone;
  mapEl.value = p.map;
  editId = p.id;
}

// SHOW
function showData(data) {
  list.innerHTML = "";

  let grouped = {};

  data.forEach(p => {
    if (!grouped[p.village]) grouped[p.village] = [];
    grouped[p.village].push(p);
  });

  for (let village in grouped) {
    list.innerHTML += `<h3>🏡 ${village}</h3>`;

    grouped[village].forEach(p => {
      list.innerHTML += `
      <div class="card">
        <img src="${p.photo || 'https://via.placeholder.com/80'}"><br>
        <b>${p.name}</b><br>
        📞 ${p.phone}<br>
        रिश्ता: ${p.relation}<br>
        <a href="${p.map}" target="_blank">📍 Map</a><br>

        <a href="tel:${p.phone}">
          <button class="call">Call</button>
        </a>

        <button class="edit" onclick='editData(${JSON.stringify(p)})'>Edit</button>
        <button class="delete" onclick="deleteData('${p.id}')">Delete</button>
      </div>
      `;
    });
  }
}

// SEARCH
window.searchData = function() {
  let input = search.value.toLowerCase();

  let filtered = people.filter(p =>
    p.name.toLowerCase().includes(input) ||
    p.village.toLowerCase().includes(input) ||
    (p.phone && p.phone.includes(input))
  );

  showData(filtered);
}

// EXCEL
window.exportExcel = function() {
  let data = people.map(p => ({
    Name: p.name,
    Village: p.village,
    Relation: p.relation,
    Phone: p.phone,
    Map: p.map
  }));

  let ws = XLSX.utils.json_to_sheet(data);
  let wb = XLSX.utils.book_new();
  XLSX.utils.book_append_sheet(wb, ws, "Data");
  XLSX.writeFile(wb, "Rishtedar.xlsx");
}

// PDF
window.exportPDF = function() {
  const { jsPDF } = window.jspdf;
  let doc = new jsPDF();

  people.forEach((p, i) => {
    doc.text(
      `${i+1}. ${p.name} | ${p.village} | ${p.relation} | ${p.phone}`,
      10,
      10 + i * 10
    );
  });

  doc.save("Rishtedar.pdf");
}

// CLEAR
function clearForm() {
  nameEl.value = "";
  villageEl.value = "";
  relationEl.value = "";
  phoneEl.value = "";
  mapEl.value = "";
  photoEl.value = "";
}

// elements
let nameEl = document.getElementById("name");
let villageEl = document.getElementById("village");
let relationEl = document.getElementById("relation");
let phoneEl = document.getElementById("phone");
let mapEl = document.getElementById("map");
let photoEl = document.getElementById("photo");
let list = document.getElementById("list");
let search = document.getElementById("search");

// start
loadData();

</script>

</body>
</html>
