
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Gestión de Clientes</title>
 
</head>
<body>
  <h1>Gestión de Clientes</h1>

  <div>
    <h2>Agregar Cliente</h2>
    <input type="text" id="nombre" placeholder="Nombre">
    <input type="text" id="telefono" placeholder="Teléfono">
    <input type="email" id="correo" placeholder="Correo">
   <button onclick="agregarCliente()">Agregar Cliente</button>
  </div>

  <div>
    <h2>Lista de Clientes</h2>
    <ul id="listaClientes"></ul>
  </div>
  dddddddd
  
  <script>
  <script type="module">
  import { initializeApp } from "https://www.gstatic.com/firebasejs/9.22.0/firebase-app.js";
  import { getFirestore, collection, addDoc, getDocs } from "https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore.js";

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

    try {
      await addDoc(collection(db, "clientes"), {
        nombre: nombre,
        telefono: telefono,
        correo: correo
      });
      alert('Cliente agregado con éxito');
      document.getElementById('nombre').value = '';
      document.getElementById('telefono').value = '';
      document.getElementById('correo').value = '';
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
      li.textContent = `${cliente.nombre} - ${cliente.telefono} - ${cliente.correo}`;
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
