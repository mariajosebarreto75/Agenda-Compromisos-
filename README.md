# Agenda-Compromisos-
# 📅 Agenda de Compromisos
 
Aplicación web para hacer seguimiento a los **compromisos que salen de las reuniones de trabajo**: quién es responsable, para cuándo, en qué estado está y con qué evidencia se cerró. Incluye programación semanal de reuniones, checklist de reunión e indicadores de cumplimiento.
 
🔗 **Demo:** [agendacompromisos.pages.dev](https://agendacompromisos.pages.dev)
 
<!-- 📸 Agrega aquí 2–3 capturas (con datos de prueba):
![Panel de compromisos](docs/compromisos.png)
![Agenda semanal](docs/agenda.png)
-->
 
---
 
## 🎯 Problema que resuelve
 
En muchos equipos los compromisos de las reuniones quedan en actas, correos o archivos de Excel sueltos, y nadie sabe cuáles están atrasados hasta que es tarde. Esta app los centraliza, por contrato y responsable, y muestra de un vistazo qué está **pendiente, reprogramado, cerrado o atrasado**.
 
---
 
## ✨ Funcionalidades
 
**Compromisos**
- Crear, filtrar y eliminar compromisos (individual o masivo).
- Filtros combinables: contrato, responsable, varios estados a la vez, atrasados y fecha (día o mes).
- **Reprogramar** con historial de cambios y **cerrar** con observación.
- **Evidencia fotográfica** por compromiso, almacenada en Cloudinary (ver, descargar y eliminar).
**Indicadores**
- Resumen en tiempo real: total, pendientes, reprogramados, cerrados y **atrasados** (no cerrados con fecha de entrega vencida), calculados en SQL y respetando los filtros activos.
**Excel**
- **Exportar** a Excel respetando los filtros aplicados.
- **Importar** compromisos de forma masiva desde un archivo `.xlsx`.
**Reuniones**
- Programación semanal de reuniones (crear, editar, eliminar).
- Checklist de reunión para no olvidar los puntos clave.
---
 
## 🛠️ Tecnologías
 
| Capa | Tecnología |
|------|-----------|
| Frontend | HTML, CSS y JavaScript (vanilla) — publicado en **Cloudflare Pages** |
| Backend | **Node.js + Express 5** (API REST) — publicado en **Render** |
| Base de datos | **MySQL** (`mysql2`, consultas parametrizadas) |
| Archivos | **Cloudinary** (evidencias) + **Multer** (subida) |
| Excel | **ExcelJS** (importar / exportar) |
 
---
 
## 🏗️ Arquitectura
 
```
[ Navegador ]  ──>  Frontend estático (Cloudflare Pages)
                          │  fetch (CORS restringido)
                          ▼
                   API REST Express (Render)
                     │              │
                     ▼              ▼
                  MySQL        Cloudinary
             (compromisos,     (evidencias)
          contratos, reuniones)
```
 
---
 
## 📡 Endpoints principales
 
| Método | Ruta | Descripción |
|--------|------|-------------|
| GET / POST | `/contratos` | Listar / crear contratos |
| GET / POST | `/compromisos` | Listar con filtros / crear |
| GET | `/compromisos/stats` | Indicadores por estado y atrasados |
| POST | `/compromisos/:id/reprogramar` | Reprogramar con historial |
| POST | `/compromisos/:id/cerrar` | Cerrar compromiso |
| GET | `/compromisos/:id/historial` | Historial de cambios |
| POST / GET / DELETE | `/compromisos/:id/evidencia` | Gestionar evidencia |
| GET | `/compromisos/export` | Exportar a Excel |
| POST | `/compromisos/import` | Importar desde Excel |
| POST | `/compromisos/delete-bulk` | Eliminación masiva |
| GET / POST / PUT / DELETE | `/reuniones` | Programación de reuniones |
 
---
 
## 🚀 Cómo ejecutarlo localmente
 
**Requisitos:** Node.js 18+ y MySQL.
 
```bash
# 1. Clonar
git clone https://github.com/mariajosebarreto75/Agenda-Compromisos-.git
cd Agenda-Compromisos-/backend
 
# 2. Instalar dependencias
npm install
 
# 3. Crear el archivo .env (ver variables abajo)
 
# 4. Iniciar la API
npm start
```
 
Luego abre `frontend/index.html` con Live Server (puerto 5500) y, en `frontend/config.js`, cambia `API_BASE` a `http://localhost:3000`.
 
### Variables de entorno (`backend/.env`)
 
```env
PORT=3000
DB_HOST=localhost
DB_PORT=3306
DB_USER=
DB_PASSWORD=
DB_NAME=
CLOUDINARY_CLOUD_NAME=
CLOUDINARY_API_KEY=
CLOUDINARY_API_SECRET=
```
 
---
 
## 🧩 Retos técnicos
 
- **Filtros dinámicos seguros:** una única función construye el `WHERE` según los filtros recibidos, con parámetros (`?`) para evitar inyección SQL, y se reutiliza en el listado, los indicadores y la exportación.
- **Indicadores en una sola consulta:** los conteos por estado y los atrasados se calculan con `SUM(condición)` en SQL, sin traer todos los registros al navegador.
- **Importación desde Excel:** lectura y validación fila por fila de archivos `.xlsx` para cargar compromisos de forma masiva.
- **Separación frontend / backend:** frontend estático y API desplegados por separado, con CORS restringido a los orígenes permitidos.
---
 
## 🔜 Próximas mejoras
 
- [ ] Autenticación de usuarios y roles
- [ ] Notificaciones por correo de compromisos próximos a vencer
- [ ] Dashboard con gráficas de cumplimiento por responsable y contrato
---
 
👩‍💻 Desarrollado por [María José Barreto](https://github.com/mariajosebarreto75)
