<!DOCTYPE html>
<html lang="hi">
<head>
<meta charset="UTF-8">
<title>Rishtedar Manager</title>

<style>
body {
  font-family: Arial;
  padding: 10px;
  background: #f5f5f5;
}

input, button {
  padding: 8px;
  margin: 5px;
  width: 95%;
}

button {
  background: green;
  color: white;
  border: none;
}

.card {
  background: white;
  padding: 10px;
  margin: 10px 0;
  border-radius: 10px;
}

img {
  width: 80px;
  height: 80px;
  border-radius: 50%;
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
<input type="text" id="search" placeholder="नाम या गांव से खोजें" onkeyup="searchData()">

<button onclick="showData()">Reset</button>
<button onclick="exportJSON()">Backup (JSON)</button>

<div id="list"></div>

<script>
let people = JSON.parse(localStorage.getItem("people")) || [];

// फोटो को base64 में बदलना
function getBase64(file, callback) {
  let reader = new FileReader();
  reader.onload = function () {
    callback(reader.result);
  };
  reader.readAsDataURL(file);
}

// डेटा सेव
function saveData() {
  let name = document.getElementById("name").value;
  let village = document.getElementById("village").value;
  let relation = document.getElementById("relation").value;
  let map = document.getElementById("map").value;
  let file = document.getElementById("photo").files[0];

  if (!name || !village) {
    alert("नाम और गांव जरूरी है");
    return;
  }

  if (file) {
    getBase64(file, function(base64) {
      addPerson(name, village, relation, map, base64);
    });
  } else {
    addPerson(name, village, relation, map, "");
  }
}

function addPerson(name, village, relation, map, photo) {
  let person = { name, village, relation, map, photo };

  people.push(person);
  localStorage.setItem("people", JSON.stringify(people));

  showData();
}

// डेटा दिखाना (गांव के हिसाब से group)
function showData() {
  let list = document.getElementById("list");
  list.innerHTML = "";

  let grouped = {};

  people.forEach(p => {
    if (!grouped[p.village]) {
      grouped[p.village] = [];
    }
    grouped[p.village].push(p);
  });

  for (let village in grouped) {
    list.innerHTML += `<h3>🏡 ${village}</h3>`;

    grouped[village].forEach(p => {
      list.innerHTML += `
        <div class="card">
          <img src="${p.photo || 'https://via.placeholder.com/80'}"><br>
          <b>${p.name}</b><br>
          रिश्ता: ${p.relation}<br>
          <a href="${p.map}" target="_blank">📍 Map</a>
        </div>
      `;
    });
  }
}

// search function
function searchData() {
  let input = document.getElementById("search").value.toLowerCase();
  let list = document.getElementById("list");
  list.innerHTML = "";

  let filtered = people.filter(p =>
    p.name.toLowerCase().includes(input) ||
    p.village.toLowerCase().includes(input)
  );

  let grouped = {};

  filtered.forEach(p => {
    if (!grouped[p.village]) {
      grouped[p.village] = [];
    }
    grouped[p.village].push(p);
  });

  for (let village in grouped) {
    list.innerHTML += `<h3>🏡 ${village}</h3>`;

    grouped[village].forEach(p => {
      list.innerHTML += `
        <div class="card">
          <img src="${p.photo || 'https://via.placeholder.com/80'}"><br>
          <b>${p.name}</b><br>
          रिश्ता: ${p.relation}<br>
          <a href="${p.map}" target="_blank">📍 Map</a>
        </div>
      `;
    });
  }
}

// JSON backup
function exportJSON() {
  let dataStr = JSON.stringify(people);
  let blob = new Blob([dataStr], {type: "application/json"});
  let a = document.createElement("a");
  a.href = URL.createObjectURL(blob);
  a.download = "data.json";
  a.click();
}

showData();
</script>

</body>
</html>
