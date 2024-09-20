<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Gestión de Clientes</title>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      background-color: #f0f0f0;
      margin: 0;
      padding: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }
    h1 {
      color: #4CAF50;
      text-align: center;
    }
    .container {
      background-color: white;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
      max-width: 600px;
      width: 100%;
    }
    input, select, textarea {
      width: 100%;
      padding: 10px;
      margin: 5px 0 15px 0;
      border: 1px solid #ccc;
      border-radius: 5px;
      font-size: 16px;
    }
    button {
      background-color: #4CAF50;
      color: white;
      padding: 10px 15px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-size: 16px;
      transition: background-color 0.3s ease;
    }
    button:hover {
      background-color: #45a049;
    }
    .listaClientes {
      margin-top: 20px;
    }
    ul {
      list-style-type: none;
      padding: 0;
    }
    li {
      background-color: #f9f9f9;
      padding: 10px;
      margin: 5px 0;
      border-radius: 5px;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    }
  </style>
</head>
<body>

  <div class="container">
    <h1>Gestión de Clientes</h1>
    
    <div>
      <h2>Agregar Cliente</h2>
      <input type="text" id="nombre" placeholder="Nombre">
      <input type="text" id="telefono" placeholder="Teléfono">
      <input type="email" id="correo" placeholder="Correo">
      <input type="text" id="direccion" placeholder="Dirección (opcional)">
      <input type="text" id="empresa" placeholder="Empresa (opcional)">
      <textarea id="notas" placeholder="Notas adicionales (opcional)"></textarea>
      <label for="categoria">Categoría</label>
      <select id="categoria">
        <option value="normal">Normal</option>
        <option value="premium">Premium</option>
        <option value="vip">VIP</option>
      </select>
      <button onclick="agregarCliente()">Agregar Cliente</button>
    </div>

    <div class="listaClientes">
      <h2>Lista de Clientes</h2>
      <ul id="listaClientes"></ul>
    </div>
  </div>

  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/9.22.0/firebase-app.js";
    import { getFirestore, collection, addDoc, getDocs } from "https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore.js";

    // Inicializar Firebase
    // Tu configuración de Firebase
  const firebaseConfig = {
    apiKey: "AIzaSyAZCTfgzV1SWaUzKjFDrLh3GPY_iSLvUWw",
    authDomain: "simplio-2ef68.firebaseapp.com",
    projectId: "simplio-2ef68",
    storageBucket: "simplio-2ef68.appspot.com",
    messagingSenderId: "721348909635",
    appId: "1:721348909635:web:4c93a969fb3e57e8ba12f6"
  };

  // Inicializar Firebase
  const app = initializeApp(firebaseConfig);
  const db = getFirestore(app);


    // Función para agregar un cliente
    window.agregarCliente = async () => {
      const nombre = document.getElementById('nombre').value;
      const telefono = document.getElementById('telefono').value;
      const correo = document.getElementById('correo').value;
      const direccion = document.getElementById('direccion').value || "No proporcionada";
      const empresa = document.getElementById('empresa').value || "No proporcionada";
      const notas = document.getElementById('notas').value || "Sin notas";
      const categoria = document.getElementById('categoria').value;

      try {
        await addDoc(collection(db, "clientes"), {
          nombre: nombre,
          telefono: telefono,
          correo: correo,
          direccion: direccion,
          empresa: empresa,
          notas: notas,
          categoria: categoria
        });
        alert('Cliente agregado con éxito');
        document.getElementById('nombre').value = '';
        document.getElementById('telefono').value = '';
        document.getElementById('correo').value = '';
        document.getElementById('direccion').value = '';
        document.getElementById('empresa').value = '';
        document.getElementById('notas').value = '';
        cargarClientes();
      } catch (error) {
        console.error('Error al agregar cliente: ', error);
      }
    };

    // Función para cargar la lista de clientes
    window.cargarClientes = async () => {
      const listaClientes = document.getElementById('listaClientes');
      listaClientes.innerHTML = '';
      const querySnapshot = await getDocs(collection(db, "clientes"));
      querySnapshot.forEach((doc) => {
        const cliente = doc.data();
        const li = document.createElement('li');
        li.textContent = `${cliente.nombre} - ${cliente.telefono} - ${cliente.correo} - ${cliente.categoria}`;
        listaClientes.appendChild(li);
      });
    };

    // Cargar clientes al inicio
    window.addEventListener('DOMContentLoaded', (event) => {
      cargarClientes();
    });
  </script>
</body>
</html>
