# Zubex · Portal de Calidad

Punto de entrada al Sistema de Gestión de Inocuidad y Calidad. Abre en las **tareas del
día** — lo que cada quien tiene que sacar hoy y cómo va el equipo — y desde ahí se llega a
los módulos, a los proyectos y a la revisión por la dirección.

El portal **no vigila los vencimientos de cada módulo**: el seguimiento es de quien lo
tiene asignado. Lo que sí concentra es la asignación, el pendiente diario y el avance.

- **Base de datos:** Firebase Firestore, proyecto `zubex-microbiologia`
- **Publicación:** GitHub Pages
- **Imagen corporativa:** logotipo oficial y azul Zubex `#0d18a8`

---

## Puesta en marcha

### 1. Pegar las llaves

En `index.html`, cerca del inicio del script:

```js
const firebaseConfig = {
  apiKey: "PEGA_AQUI_TU_API_KEY",
  ...
  appId: "PEGA_AQUI_TU_APP_ID"
};
```

Sustituye los dos valores por los de tu proyecto en Firebase
(Configuración del proyecto → Tus apps → Configuración del SDK). Son valores públicos:
viajan dentro de la página, como en cualquier app web de Firebase.

### 2. Activar el acceso y dar de alta al equipo

Consola de Firebase → **Authentication → Sign-in method → Correo electrónico/contraseña → Habilitar**.
Luego **Authentication → Users → Add user** para cada persona.

### 3. Cargar las reglas de seguridad

**Firestore Database → Reglas**, pega el contenido de [`firestore.rules`](firestore.rules) y publica.
Cubren tanto el portal como el módulo de Microbiología.

> Cuando todo el equipo esté dado de alta, endurece la regla de creación de usuarios:
> comenta la línea `allow create` activa y descomenta la que exige `rol == 'lectura'`.

### 4. Publicar

Sube `index.html`, `logo-zubex.png`, `logo-zubex@2x.png`, `favicon.png`, `.nojekyll` y este README
al repositorio, y activa **Settings → Pages → Deploy from a branch → main / (root)**.

### 5. Primer ingreso

La **primera persona que entra queda como administrador**. Las demás entran con perfil de
consulta; el administrador les cambia el perfil en **Administrador → Usuarios y permisos**.

---

## Cómo está organizado

| Sección | Para qué |
|---|---|
| **Tareas del día** | La carátula. Saludo, fecha, y las tareas agrupadas en Vencidas · Para hoy · Esta semana · Más adelante · Sin fecha · Completadas. El interruptor **Mis tareas / Todo el equipo** cambia entre lo propio y el seguimiento del equipo. |
| **Seguimiento del equipo** | Dentro de la carátula, en vista «Todo el equipo»: una tarjeta por persona con abiertas, vencidas, completadas, barra de avance y sus próximas tareas. Es lo que el jefe revisa en la mañana. |
| **Proyectos** | Avance de los proyectos del área, con responsable y fecha meta. |
| **Revisión dirección** | Resumen imprimible para la junta: avance del periodo, tareas cerradas, módulos con responsable y avance por persona. Botón **Imprimir o guardar en PDF**. |
| **Todos los módulos** | Tabla limpia: módulo, responsable, quién verifica, frecuencia, documento de respaldo y estado. Sin semáforos de vencimiento. |
| **Módulos** (menú) | Acceso directo a cada tablero. La casilla «solo los míos» deja en el menú los que tienes a tu cargo. |
| **Agregar módulo** | 🔒 Solo administrador y bajo contraseña. |
| **Administrador** | Matriz de quién lleva qué, usuarios y permisos, y respaldos. |
| **Configuración** | Identidad del portal, referencias normativas, metas, seguridad y qué ve cada perfil. |
| **Bitácora** | Quién cambió qué, cuándo y por qué. |

---

## El candado de edición

Administrador y Configuración abren en **solo lectura**. Para cambiar algo hay que
confirmar **tu propia contraseña** — no una clave compartida. Se desbloquea por el número
de minutos que definas (15 por defecto) y se vuelve a bloquear solo.

Dos razones para hacerlo así: nadie puede cambiar nada desde una sesión que alguien dejó
abierta, y la bitácora registra un nombre real en vez de «el administrador».

Si cambias un criterio del sistema — la meta de cumplimiento, la edición del código SQF o
la cláusula de referencia — el portal **pide el motivo** y lo guarda junto al cambio.
Es lo primero que pregunta un auditor y lo único que no se puede reconstruir después.

Ambas cosas se apagan en **Configuración → Seguridad** si estorban.

---

## Perfiles

| Perfil | Puede |
|---|---|
| **Administrador** | Todo: alta y baja de módulos, asignación de responsables, permisos, configuración y eliminar registros. |
| **Captura** | Asignar y cerrar pendientes, actualizar proyectos. No toca módulos ni configuración. |
| **Consulta** | Ver y exportar. |

---

## Módulos que vienen cargados

Calibraciones, Microbiología, Folios de Calidad, Vidrio y Plástico Quebradizo,
Auditorías de Inocuidad, Quejas a Proveedor, Documentación de Proveedores,
Control de Plagas, Migración de Metales Pesados y Cultura de Calidad.

Cada uno tiene estado, descripción, responsable, quién verifica, con qué frecuencia y el
procedimiento o formato que lo respalda, más la liga a su tablero.

**Tú asignas cada módulo** desde Administrador → Matriz de responsables, o desde la ficha
del módulo. Para **agregar uno nuevo** está el botón 🔒 *Agregar módulo* al pie del menú:
pide tu contraseña antes de dejarte crearlo.

---

## Respaldo

**Administrador → Respaldos → Descargar respaldo completo** baja un Excel con módulos,
pendientes, proyectos, bitácora y usuarios. Hazlo una vez al mes y guárdalo en la carpeta
de calidad de la red interna: es el respaldo que pide el control de registros, y la fecha
del último queda a la vista en esa misma pantalla.

---

## Modo local

Si faltan las llaves o no hay conexión, el portal entra en **modo local**: funciona y
guarda en el navegador, con el indicador del menú en rojo. Sirve para probar, no para
operar — los datos no se comparten entre computadoras.
