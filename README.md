# 📅 Planificación Scrum: Tienda Virtual (Integración Bancaria)

**Equipo:** Comercio (3 Desarrolladores)
**Tecnologías:** Next.js (Frontend/API), MongoDB (Base de datos)
**Contexto:** Proyecto con interdependencia crítica de servicios externos (API de Bancos y otros Comercios).

## 🏁 Sprint 1: Inception, Protocolos y Esqueleto
**Duración:** Semana 1
**Objetivo del Sprint:** Establecer el contrato técnico con los equipos "Banco" y levantar la infraestructura base para no bloquear el desarrollo.

### 📋 Backlog del Sprint 1
| Prioridad | Rol Responsable | Tarea | Criterios de Aceptación |
| :--- | :--- | :--- | :--- |
| **Alta** | **Todo el Equipo** | **Negociación de API (Reunión Inter-equipos)** | Definir JSON de Request/Response para `/procesar_pago`. Acordar códigos de error HTTP (200, 400, 402, 500). Documentar acuerdos en `API.md`. |
| Alta | Dev 1 (Fullstack) | Setup del Repositorio y Next.js | Estructura de carpetas creada (`/pages`, `/components`, `/lib`). ESLint y Prettier configurados. Deploy inicial en Vercel/Netlify (opcional pero recomendado). |
| Media | Dev 2 (Backend) | Configuración MongoDB & Modelos Base | Cluster de Atlas creado. Esquemas de Mongoose definidos para `Producto` (id, nombre, precio, imagen, stock). |
| Media | Dev 3 (Integration) | **Crear Mock del Banco** | Crear un endpoint temporal (`/api/mock-banco`) que simule respuestas de éxito/error acordadas, para no depender de que los otros equipos terminen su API hoy. |

***

## 🚀 Sprint Intermedio (Ej. Sprint 3): Core Transaccional
**Duración:** Semana 3
**Objetivo del Sprint:** Lograr una compra completa, desde el carrito hasta la comunicación con la API (real o simulada) del Banco Adquiriente.

### 📋 Backlog del Sprint 3
| Prioridad | Rol Responsable | Tarea | Criterios de Aceptación |
| :--- | :--- | :--- | :--- |
| **Alta** | Dev 2 (Backend) | Endpoint `/api/checkout` | Recibe el carrito + datos de tarjeta del frontend. Valida stocks en MongoDB. Envía petición `POST` a la API externa del Banco. Maneja la respuesta JSON. |
| Alta | Dev 1 (Frontend) | UI Formulario de Pago | Formulario validado (inputs requeridos, formato fecha tarjeta). **IMPORTANTE:** No guardar CVV en base de datos, solo transmitirlo. |
| Media | Dev 3 (QA/UX) | Manejo de Estados de Respuesta | Pantalla de "Compra Exitosa" (si Banco responde `status: 'ok'`). Pantalla de "Saldo Insuficiente" (si Banco responde error). Manejo de *Timeouts* (si el Banco tarda más de 5s). |
| Media | Todo el Equipo | Sincronización de Stocks | Al confirmar el pago exitoso, descontar la cantidad comprada de la colección `Productos` en MongoDB. |

***

## 🏁 Último Sprint: Integración E2E y Entrega
**Duración:** Semana Final
**Objetivo del Sprint:** Validar el flujo real entre equipos (Tu Tienda -> Banco A -> Banco B) y generar la evidencia para la entrega final.

### 📋 Backlog del Sprint Final
| Prioridad | Rol Responsable | Tarea | Criterios de Aceptación |
| :--- | :--- | :--- | :--- |
| **Critica** | Dev 3 (QA Lead) | **Pruebas Cruzadas (Integration Testing)** | Realizar compras reales usando tarjetas de los equipos "Banco". Verificar que el dinero se "mueva" (simbólicamente) y la respuesta llegue correctamente a tu tienda. |
| Alta | Dev 1 & 2 | Refactorización y Limpieza | Eliminar datos *hardcodeados* y Mocks del Sprint 1. Asegurar que las variables de entorno (`ENV`) apunten a las URLs reales de los Bancos de los compañeros. |
| Media | Dev 1 (Frontend) | Pulido Visual (CSS/Tailwind) | Mejorar feedback visual (loaders/spinners mientras se procesa el pago). Verificar responsividad móvil. |
| Alta | Todo el Equipo | **Informe Final de Pruebas** | Documentar casos de prueba: <br>1. Compra exitosa (Mismo banco). <br>2. Compra exitosa (Interbancaria). <br>3. Fallo por fondos insuficientes. <br>4. Fallo por caída del sistema bancario (simulado). |

***

### 💡 Notas Técnicas para el Equipo
*   **Next.js API Routes:** Usen las API Routes (`/pages/api/*` o App Router Handlers) como "middleware" seguro. Nunca llamen al Banco directamente desde el componente React del cliente (`useEffect`), ya que expondrían sus credenciales o tokens si los tuvieran, y evitarán problemas de CORS iniciales.
*   **MongoDB:** Para este MVP, usen una colección `Ordenes` para guardar el historial de intentos de compra (exitosos y fallidos) con su `transactionId` que devuelva el banco. Esto les servirá de evidencia.

Sources
[1] Plantillas gratuitas de Scrum en varios formatos - Smartsheet https://es.smartsheet.com/content/scrum-templates
[2] 10 plantillas Scrum gratuitas para controlar su flujo de trabajo https://clickup.com/es-ES/blog/50976/plantillas-scrum
[3] Formato Ejemplo para Documentar El Uso de Scrum en Un Proyecto https://es.scribd.com/document/544425633/298980786-Formato-Ejemplo-Para-Documentar-El-Uso-de-Scrum-en-Un-Proyecto-Doc
[4] Plantillas de Planificación del Sprint para Equipos Ágiles - Miro https://miro.com/es/plantillas/sprint-planning/
[5] Plantilla para planificación de sprints • Asana [2025] https://asana.com/es/templates/sprint-planning
[6] 4 Plantillas de documentos interesantes para las Retrospectivas de ... https://echometerapp.com/es/sprint-retrospective-document-templates/
[7] 10 plantillas de metodología ágil para mejorar la gestión de proyectos https://www.atlassian.com/es/agile/project-management/templates
[8] Principales plantillas de Tablero Scrum - Notion https://www.notion.com/es/templates/category/best-scrum-board-templates
