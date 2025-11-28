🎮 IKERNATOR – Documentación Técnica y Guía de Ejecución

Proyecto desarrollado en C# (Windows Forms) con MySQL
Autor: Iker Rubalcaba
Fecha: 27/11/2025

📌 Introducción

IkerNator es un videojuego interactivo tipo Akinator inverso, en el que el usuario intenta adivinar la identidad de un personaje real (registrado en la base de datos) mediante pistas progresivas.
Cuenta con sistema de usuarios, slots de partidas, registro de métricas, datos persistentes, y gestión dinámica de personajes y pistas.

El sistema permite:
✔ Crear usuarios con autenticación
✔ Crear hasta 5 slots por usuario
✔ Jugar adivinando personajes reales desde la BD
✔ Registrar métricas como victoria, derrota, pistas, intentos, puntaje, fechas
✔ Mostrar estadísticas generales y por slot

🏗️ Tecnologías utilizadas
Componente	Herramienta
Lenguaje principal	C# (.NET Windows Forms)
Base de datos	MySQL (XAMPP)
Conector SQL	MySql.Data (NuGet)
Arquitectura	Modelo basado en formularios + clases de gestión
Gestión de sesiones	Variables globales en formularios
Documentación	Markdown / Word / UML
📁 Estructura real del proyecto
Videojuego_Iker/
│
├── Program.cs
├── MenuPrincipal.cs
├── MenuPrincipal.Designer.cs
├── PantallaSlots.cs
├── PantallaSlots.Designer.cs
├── Form1.cs            (Pantalla de juego)
├── Form1.Designer.cs
│
├── PartidaManager.cs   (Manejo de slots y partidas)
├── ConexionDB.cs       (Conexión y consultas SQL)
│
├── bin/
├── obj/
│
└── README.md (este documento)

💾 Base de Datos MySQL

📌 Nombre de base de datos: juego_akinator_inverso

Tablas principales:

Tabla	Función
usuario	Manejo de cuentas
partida	Historial de partidas
personaje	Personajes disponibles
atributo	Atributos y pistas
personaje_atributo	Relación (personaje-pista)
puntaje_usuario	Métricas globales
partida_pista	Registro de pistas usadas
🔌 Conexión a base de datos (archivo ConexionDB.cs)
using MySql.Data.MySqlClient;

public class ConexionDB
{
    private static string connectionString =
        "Server=localhost;Database=juego_akinator_inverso;User ID=root;Password=;";

    public static MySqlConnection GetConnection()
    {
        return new MySqlConnection(connectionString);
    }
}

📦 Instalación de dependencias
1️⃣ Instalar XAMPP y activar MySQL

– Iniciar Apache y MySQL desde XAMPP Control Panel.

2️⃣ Importar la base de datos

– Abrir http://localhost/phpmyadmin
– Crear BD: juego_akinator_inverso
– Importar el archivo .sql del proyecto.

3️⃣ Instalar MySQL Connector desde NuGet en Visual Studio
Herramientas → Administrar paquetes NuGet → Buscar → MySql.Data → Instalar

🚀 Instrucciones de ejecución
1️⃣ Abrir proyecto en Visual Studio

– Abrir Videojuego_Iker.sln
– Compilar en modo Debug o Release

2️⃣ Reparar conexión si es necesario

Si MySQL tiene contraseña agrega:

"Server=localhost;Database=juego_akinator_inverso;User ID=root;Password=tuPass;"

3️⃣ Ejecutar el sistema

Presionar botón ▶ (Start)
Se abre la Pantalla de Menú Principal

🎯 Flujo correcto:

👉 Crear Usuario → Validar duplicados
👉 Iniciar Sesión → Ir a pantalla Slots
👉 Crear / Seleccionar Slot → Jugar
👉 Pantalla de Juego → Pistas → Elegir personaje
👉 Guardar métrica → Terminar partida → Consultar estadísticas

🧠 Lógica interna del juego
🎮 Selección aleatoria de personaje
SELECT personaje_id FROM personaje ORDER BY RAND() LIMIT 1;

🔍 Generación de pista desde atributos
SELECT atributo.nombre, personaje_atributo.valor
FROM personaje_atributo
WHERE personaje_id = @id
ORDER BY RAND() LIMIT 1;

📝 Registro de métrica al finalizar partida
INSERT INTO partida(usuario_id, personaje_id, dificultad, pistas_usadas, intentos_incorrectos, puntos_obtenidos, fecha_inicio, fecha_fin)
VALUES(@usuario, @personaje, @dif, @pistas, @errores, @puntos, NOW(), NOW());

🧪 Pruebas recomendadas
ID	Prueba	Resultado esperado
CP1	Crear usuario nuevo	Usuario se registra correctamente
CP2	Intentar crear usuario duplicado	Se bloquea con mensaje de error
CP3	Crear slot nuevo	Slot se almacena y queda disponible
CP4	Eliminar slot existente	Slot eliminado, espacio liberado
CP5	Adivinar personaje correcto	Se registra victoria y métrica
CP6	Consultar métricas generales	Se muestran estadísticas completas
🛠 Posibles errores y soluciones
Error	Causa	Solución
“MySqlCommand no existe”	Falta NuGet	Instalar MySql.Data
“No se puede conectar al servidor”	MySQL apagado	Activar MySQL desde XAMPP
“Tabla no encontrada”	BD mal importada	Reimportar .sql
“Access denied for user root”	Contraseña activada	Agregar Password en connectionString
🧭 Futuras mejoras

✔ Diseño visual con WPF o MAUI
✔ Integrar fotos reales de personajes
✔ Modo multijugador / Ranking global
✔ Migración a Unity o Web (ASP.NET)
✔ Generación de pistas con IA

📌 Créditos

🧑‍💻 Desarrollador: Iker Rubalcaba
👨‍🏫 Profesor: Octavio Villavicencio
📚 Materia: Base de Datos
🏫 Semestre: 5° B
📅 Entrega: 27 de noviembre de 2025
