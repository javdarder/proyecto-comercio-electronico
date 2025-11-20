{\rtf1\ansi\ansicpg1252\cocoartf2867
\cocoatextscaling0\cocoaplatform0{\fonttbl\f0\fswiss\fcharset0 Helvetica;}
{\colortbl;\red255\green255\blue255;}
{\*\expandedcolortbl;;}
\margl1440\margr1440\vieww11520\viewh8400\viewkind0
\pard\tx720\tx1440\tx2160\tx2880\tx3600\tx4320\tx5040\tx5760\tx6480\tx7200\tx7920\tx8640\pardirnatural\partightenfactor0

\f0\fs24 \cf0 Aqu\'ed tienes la planificaci\'f3n adaptada a **Markdown (.md)** para tu proyecto de tienda virtual (Next.js + MongoDB). Este formato es ideal para guardarlo en tu repositorio (por ejemplo, como `PLANIFICACION_SCRUM.md` o dentro de la Wiki de tu proyecto).\
\
***\
\
# \uc0\u55357 \u56517  Planificaci\'f3n Scrum: Tienda Virtual (Integraci\'f3n Bancaria)\
\
**Equipo:** Comercio (3 Desarrolladores)\
**Tecnolog\'edas:** Next.js (Frontend/API), MongoDB (Base de datos)\
**Contexto:** Proyecto con interdependencia cr\'edtica de servicios externos (API de Bancos y otros Comercios).\
\
## \uc0\u55356 \u57281  Sprint 1: Inception, Protocolos y Esqueleto\
**Duraci\'f3n:** Semana 1\
**Objetivo del Sprint:** Establecer el contrato t\'e9cnico con los equipos "Banco" y levantar la infraestructura base para no bloquear el desarrollo.\
\
### \uc0\u55357 \u56523  Backlog del Sprint 1\
| Prioridad | Rol Responsable | Tarea | Criterios de Aceptaci\'f3n |\
| :--- | :--- | :--- | :--- |\
| **Alta** | **Todo el Equipo** | **Negociaci\'f3n de API (Reuni\'f3n Inter-equipos)** | Definir JSON de Request/Response para `/procesar_pago`. Acordar c\'f3digos de error HTTP (200, 400, 402, 500). Documentar acuerdos en `API.md`. |\
| Alta | Dev 1 (Fullstack) | Setup del Repositorio y Next.js | Estructura de carpetas creada (`/pages`, `/components`, `/lib`). ESLint y Prettier configurados. Deploy inicial en Vercel/Netlify (opcional pero recomendado). |\
| Media | Dev 2 (Backend) | Configuraci\'f3n MongoDB & Modelos Base | Cluster de Atlas creado. Esquemas de Mongoose definidos para `Producto` (id, nombre, precio, imagen, stock). |\
| Media | Dev 3 (Integration) | **Crear Mock del Banco** | Crear un endpoint temporal (`/api/mock-banco`) que simule respuestas de \'e9xito/error acordadas, para no depender de que los otros equipos terminen su API hoy. |\
\
***\
\
## \uc0\u55357 \u56960  Sprint Intermedio (Ej. Sprint 3): Core Transaccional\
**Duraci\'f3n:** Semana 3\
**Objetivo del Sprint:** Lograr una compra completa, desde el carrito hasta la comunicaci\'f3n con la API (real o simulada) del Banco Adquiriente.\
\
### \uc0\u55357 \u56523  Backlog del Sprint 3\
| Prioridad | Rol Responsable | Tarea | Criterios de Aceptaci\'f3n |\
| :--- | :--- | :--- | :--- |\
| **Alta** | Dev 2 (Backend) | Endpoint `/api/checkout` | Recibe el carrito + datos de tarjeta del frontend. Valida stocks en MongoDB. Env\'eda petici\'f3n `POST` a la API externa del Banco. Maneja la respuesta JSON. |\
| Alta | Dev 1 (Frontend) | UI Formulario de Pago | Formulario validado (inputs requeridos, formato fecha tarjeta). **IMPORTANTE:** No guardar CVV en base de datos, solo transmitirlo. |\
| Media | Dev 3 (QA/UX) | Manejo de Estados de Respuesta | Pantalla de "Compra Exitosa" (si Banco responde `status: 'ok'`). Pantalla de "Saldo Insuficiente" (si Banco responde error). Manejo de *Timeouts* (si el Banco tarda m\'e1s de 5s). |\
| Media | Todo el Equipo | Sincronizaci\'f3n de Stocks | Al confirmar el pago exitoso, descontar la cantidad comprada de la colecci\'f3n `Productos` en MongoDB. |\
\
***\
\
## \uc0\u55356 \u57281  \'daltimo Sprint: Integraci\'f3n E2E y Entrega\
**Duraci\'f3n:** Semana Final\
**Objetivo del Sprint:** Validar el flujo real entre equipos (Tu Tienda -> Banco A -> Banco B) y generar la evidencia para la entrega final.\
\
### \uc0\u55357 \u56523  Backlog del Sprint Final\
| Prioridad | Rol Responsable | Tarea | Criterios de Aceptaci\'f3n |\
| :--- | :--- | :--- | :--- |\
| **Critica** | Dev 3 (QA Lead) | **Pruebas Cruzadas (Integration Testing)** | Realizar compras reales usando tarjetas de los equipos "Banco". Verificar que el dinero se "mueva" (simb\'f3licamente) y la respuesta llegue correctamente a tu tienda. |\
| Alta | Dev 1 & 2 | Refactorizaci\'f3n y Limpieza | Eliminar datos *hardcodeados* y Mocks del Sprint 1. Asegurar que las variables de entorno (`ENV`) apunten a las URLs reales de los Bancos de los compa\'f1eros. |\
| Media | Dev 1 (Frontend) | Pulido Visual (CSS/Tailwind) | Mejorar feedback visual (loaders/spinners mientras se procesa el pago). Verificar responsividad m\'f3vil. |\
| Alta | Todo el Equipo | **Informe Final de Pruebas** | Documentar casos de prueba: <br>1. Compra exitosa (Mismo banco). <br>2. Compra exitosa (Interbancaria). <br>3. Fallo por fondos insuficientes. <br>4. Fallo por ca\'edda del sistema bancario (simulado). |\
\
***\
\
### \uc0\u55357 \u56481  Notas T\'e9cnicas para el Equipo\
*   **Next.js API Routes:** Usen las API Routes (`/pages/api/*` o App Router Handlers) como "middleware" seguro. Nunca llamen al Banco directamente desde el componente React del cliente (`useEffect`), ya que expondr\'edan sus credenciales o tokens si los tuvieran, y evitar\'e1n problemas de CORS iniciales.\
*   **MongoDB:** Para este MVP, usen una colecci\'f3n `Ordenes` para guardar el historial de intentos de compra (exitosos y fallidos) con su `transactionId` que devuelva el banco. Esto les servir\'e1 de evidencia.\
\
Sources\
[1] Plantillas gratuitas de Scrum en varios formatos - Smartsheet https://es.smartsheet.com/content/scrum-templates\
[2] 10 plantillas Scrum gratuitas para controlar su flujo de trabajo https://clickup.com/es-ES/blog/50976/plantillas-scrum\
[3] Formato Ejemplo para Documentar El Uso de Scrum en Un Proyecto https://es.scribd.com/document/544425633/298980786-Formato-Ejemplo-Para-Documentar-El-Uso-de-Scrum-en-Un-Proyecto-Doc\
[4] Plantillas de Planificaci\'f3n del Sprint para Equipos \'c1giles - Miro https://miro.com/es/plantillas/sprint-planning/\
[5] Plantilla para planificaci\'f3n de sprints \'95 Asana [2025] https://asana.com/es/templates/sprint-planning\
[6] 4 Plantillas de documentos interesantes para las Retrospectivas de ... https://echometerapp.com/es/sprint-retrospective-document-templates/\
[7] 10 plantillas de metodolog\'eda \'e1gil para mejorar la gesti\'f3n de proyectos https://www.atlassian.com/es/agile/project-management/templates\
[8] Principales plantillas de Tablero Scrum - Notion https://www.notion.com/es/templates/category/best-scrum-board-templates\
}
