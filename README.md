<!DOCTYPE html>
<html lang="hi">
<head>
<meta charset="UTF-8">
<title>Rishtedar App</title>

<!-- Excel -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>

<!-- PDF -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

<style>
body {
  font-family: Arial;
  background: linear-gradient(135deg, #4facfe, #00f2fe);
  margin: 0;
  padding: 10px;
}

.container {
  max-width: 500px;
  margin: auto;
  background: white;
  padding: 15px;
  border-radius: 15px;
}

h2 {
  text-align: center;
}

input {
  width: 100%;
  padding: 10px;
  margin: 5px 0;
  border-radius: 8px;
  border: 1px solid #ccc;
}

button {
  width: 100%;
  padding: 10px;
  margin-top: 5px;
  border: none;
  border-radius: 8px;
  background: #4facfe;
  color: white;
  font-weight: bold;
}

.card {
  background: #f9f9f9;
  padding: 10px;
  margin-top: 10px;
  border-radius: 10px;
}

img {
  width: 70px;
  height: 70px;
  border-radius: 50%;
}

.actions button {
  width: 48%;
  margin: 2px;
}

.delete { background:red; }
.edit { background:blue; }
.call { background:green; }
</style>
</head>

<body>

<div class="container">

<h2>👨‍👩‍👧 रिश्तेदार ऐप</h2>

<input id="name" placeholder="नाम">
<input id="village" placeholder="गांव">
<input id="relation" placeholder="रिश्ता">
<input id="phone" placeholder="मोबाइल नंबर">
<input id="map" placeholder="Google Map Link">
<input type="file" id="photo">

<button onclick="saveData()">Save</button>

<hr>

<input id="search" placeholder="Search..." onkeyup="searchData()">

<button onclick="loadData()">Reset</button>
<button onclick="exportExcel()">Excel</button>
<button onclick="exportPDF()">PDF</button>

<div id="list"></div>

</div>

<script type="module">

import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
import { getFirestore, collection, addDoc, getDocs, deleteDoc, doc, updateDoc } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

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

function getBase64(file) {
  return new Promise(res => {
    let reader = new FileReader();
    reader.onload = () => res(reader.result);
    reader.readAsDataURL(file);
  });
}

window.saveData = async function() {
  let name = nameEl.value.trim();
  let village = villageEl.value.trim();

  if (!name || !village) return alert("नाम और गांव जरूरी है");

  let photo = "";
  if (photoEl.files[0]) photo = await getBase64(photoEl.files[0]);

  let person = {
    name, village,
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

window.loadData = async function() {
  people = [];
  let snap = await getDocs(collection(db, "people"));
  snap.forEach(d => people.push({ id:d.id, ...d.data() }));
  showData(people);
}

window.deleteData = async function(id) {
  if (confirm("Delete?")) {
    await deleteDoc(doc(db, "people", id));
    loadData();
  }
}

window.editData = function(p) {
  nameEl.value = p.name;
  villageEl.value = p.village;
  relationEl.value = p.relation;
  phoneEl.value = p.phone;
  mapEl.value = p.map;
  editId = p.id;
}

function showData(data) {
  list.innerHTML = "";

  let grouped = {};
  data.forEach(p => {
    if (!grouped[p.village]) grouped[p.village] = [];
    grouped[p.village].push(p);
  });

  for (let v in grouped) {
    list.innerHTML += `<h3>🏡 ${v}</h3>`;
    grouped[v].forEach(p => {
      list.innerHTML += `
      <div class="card">
        <img src="${p.photo || 'https://via.placeholder.com/70'}"><br>
        <b>${p.name}</b><br>
        📞 ${p.phone}<br>
        ${p.relation}<br>
        <a href="${p.map}" target="_blank">📍 Map</a><br>

        <div class="actions">
          <a href="tel:${p.phone}"><button class="call">Call</button></a>
          <button class="edit" onclick='editData(${JSON.stringify(p)})'>Edit</button>
          <button class="delete" onclick="deleteData('${p.id}')">Delete</button>
        </div>
      </div>`;
    });
  }
}

window.searchData = function() {
  let s = search.value.toLowerCase();
  let f = people.filter(p =>
    p.name.toLowerCase().includes(s) ||
    p.village.toLowerCase().includes(s) ||
    (p.phone && p.phone.includes(s))
  );
  showData(f);
}

window.exportExcel = function() {
  let data = people.map(p => ({
    Name:p.name, Village:p.village, Relation:p.relation, Phone:p.phone
  }));
  let ws = XLSX.utils.json_to_sheet(data);
  let wb = XLSX.utils.book_new();
  XLSX.utils.book_append_sheet(wb, ws, "Data");
  XLSX.writeFile(wb, "Rishtedar.xlsx");
}

window.exportPDF = function() {
  const { jsPDF } = window.jspdf;
  let doc = new jsPDF();
  people.forEach((p,i)=>{
    doc.text(`${i+1}. ${p.name} | ${p.village} | ${p.phone}`,10,10+i*10);
  });
  doc.save("Rishtedar.pdf");
}

function clearForm(){
  nameEl.value="";
  villageEl.value="";
  relationEl.value="";
  phoneEl.value="";
  mapEl.value="";
  photoEl.value="";
}

let nameEl = document.getElementById("name");
let villageEl = document.getElementById("village");
let relationEl = document.getElementById("relation");
let phoneEl = document.getElementById("phone");
let mapEl = document.getElementById("map");
let photoEl = document.getElementById("photo");
let list = document.getElementById("list");
let search = document.getElementById("search");

loadData();

</script>

</body>
</html>
