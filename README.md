<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Top Personas</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background-color: #f0f0f5;
      margin: 0;
      padding: 2rem;
      text-align: center;
    }

    h1 {
      margin-bottom: 1rem;
    }

    button {
      padding: 0.5rem 1rem;
      font-size: 1rem;
      border: none;
      background-color: #4e73df;
      color: white;
      border-radius: 5px;
      cursor: pointer;
    }

    .cards {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 1rem;
      margin-top: 2rem;
    }

    .card {
      background-color: white;
      padding: 1rem;
      border-radius: 10px;
      width: 250px;
      box-shadow: 0 4px 8px rgba(0,0,0,0.1);
      text-align: left;
    }

    .card img {
      width: 100%;
      height: 180px;
      object-fit: cover;
      border-radius: 8px;
    }

    .card h3 {
      margin: 0.5rem 0 0.2rem;
    }

    .modal {
      display: none;
      position: fixed;
      top: 0; left: 0;
      width: 100%; height: 100%;
      background: rgba(0, 0, 0, 0.6);
      justify-content: center;
      align-items: center;
    }

    .modal-content {
      background: white;
      padding: 1.5rem;
      border-radius: 10px;
      width: 300px;
    }

    .modal-content input, .modal-content textarea {
      width: 100%;
      margin-bottom: 1rem;
      padding: 0.5rem;
      border-radius: 5px;
      border: 1px solid #ccc;
    }

    .close-btn {
      float: right;
      cursor: pointer;
      font-weight: bold;
    }
  </style>
</head>
<body>

  <h1>Top Personas</h1>
  <button onclick="openModal()">Añadir Tarjeta</button>

  <div class="cards" id="cardsContainer"></div>

  <div class="modal" id="modal">
    <div class="modal-content">
      <span class="close-btn" onclick="closeModal()">X</span>
      <input type="text" id="nameInput" placeholder="Nombre">
      <textarea id="descInput" placeholder="Descripción"></textarea>
      <input type="url" id="imgInput" placeholder="URL de imagen">
      <input type="number" id="topInput" placeholder="Ranking (1, 2, 3...)">
      <button onclick="addCard()">Añadir</button>
    </div>
  </div>

  <script>
    let cards = [];

    // Cargar datos desde localStorage
    function loadFromStorage() {
      const stored = localStorage.getItem("topCards");
      if (stored) {
        cards = JSON.parse(stored);
        renderCards();
      }
    }

    // Guardar datos en localStorage
    function saveToStorage() {
      localStorage.setItem("topCards", JSON.stringify(cards));
    }

    function openModal() {
      document.getElementById("modal").style.display = "flex";
    }

    function closeModal() {
      document.getElementById("modal").style.display = "none";
    }

    function addCard() {
      const name = document.getElementById("nameInput").value;
      const desc = document.getElementById("descInput").value;
      const img = document.getElementById("imgInput").value;
      const top = parseInt(document.getElementById("topInput").value);

      if (!name || !desc || !img || isNaN(top)) return alert("Completa todos los campos correctamente.");

      cards.push({ name, desc, img, top });
      cards.sort((a, b) => a.top - b.top);
      saveToStorage();
      renderCards();
      closeModal();
    }

    function renderCards() {
      const container = document.getElementById("cardsContainer");
      container.innerHTML = "";
      cards.forEach(card => {
        container.innerHTML += `
          <div class="card">
            <img src="${card.img}" alt="${card.name}">
            <h3>#${card.top} - ${card.name}</h3>
            <p>${card.desc}</p>
          </div>
        `;
      });
    }

    // Iniciar
    loadFromStorage();
  </script>

</body>
</html>
