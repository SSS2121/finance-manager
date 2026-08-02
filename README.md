<!--
Archivo: README.md
Propósito: documentar exclusivamente la arquitectura prevista del proyecto educativo
"Gestor de finanzas". Este documento no contiene código de implementación.
-->

# Gestor de finanzas — Arquitectura

## 1. Propósito

Este proyecto servirá para aprender desarrollo frontend construyendo un gestor de
finanzas personales de manera progresiva.

La primera versión se desarrollará con HTML, CSS y JavaScript sin framework.
Node.js se utilizará como entorno de herramientas y Vite como servidor de
desarrollo y sistema de construcción. Más adelante, la interfaz migrará a React
y se añadirá un backend con FastAPI, autenticación real y base de datos.

Este README describe solamente:

- La organización prevista del proyecto.
- La responsabilidad de cada tecnología y capa.
- Las secciones funcionales de la aplicación.
- El modelo conceptual de los datos financieros.
- La evolución planificada del frontend y el backend.

No incluye código, comandos de instalación ni instrucciones de implementación.

## 2. Alcance por etapas

| Etapa | Interfaz | Persistencia | Acceso | Objetivo |
| --- | --- | --- | --- | --- |
| 1. Estructura | HTML semántico | Ninguna | Pantalla estática | Comprender documentos, etiquetas y formularios |
| 2. Presentación | HTML y CSS | Ninguna | Pantalla estática | Aprender cascada, layout y diseño adaptable |
| 3. Comportamiento | JavaScript modular | Estado en memoria, perdido al recargar | Login simulado | Aprender DOM, eventos, validación y estado |
| 4. Datos locales | JavaScript modular | `localStorage` y respaldo JSON | Login simulado | Aprender serialización y persistencia local |
| 5. Migración de UI | React | `localStorage` y respaldo JSON | Login simulado | Aprender componentes, propiedades y estado React |
| 6. Aplicación completa | React | Base de datos mediante una API FastAPI | Autenticación real | Aprender comunicación cliente-servidor y seguridad |

La etapa inicial será de un solo usuario y un solo navegador. La sincronización
entre dispositivos y usuarios pertenece a la etapa con backend.

## 3. Responsabilidad de cada tecnología

### HTML

HTML define el significado y la estructura de la página. `index.html` será el
documento de entrada que el navegador interpreta para construir el DOM.

Sus responsabilidades serán:

- Organizar títulos, navegación, secciones y formularios.
- Mantener una estructura semántica y accesible.
- Proporcionar los elementos que JavaScript podrá consultar y actualizar.

HTML no será responsable de los cálculos financieros ni de guardar datos.

### CSS

CSS define la presentación visual:

- Colores, tipografía y espaciado.
- Distribución mediante Flexbox y Grid.
- Diseño adaptable para móvil y escritorio.
- Estados visuales de validación, error, éxito y carga.

CSS no debe contener reglas de negocio.

### JavaScript

JavaScript será responsable del comportamiento:

- Escuchar eventos del usuario.
- Leer y validar formularios.
- Crear, editar y eliminar movimientos.
- Calcular saldos y resúmenes.
- Actualizar el DOM.
- Convertir datos desde y hacia JSON.
- Coordinar el almacenamiento local.

El archivo de entrada de JavaScript solo iniciará y conectará los módulos. Las
reglas no se concentrarán en un único archivo grande.

### Node.js

Node.js permite ejecutar JavaScript fuera del navegador. En la primera etapa no
será el backend del gestor: se usará localmente para ejecutar herramientas como
npm y Vite.

La aplicación financiera seguirá ejecutándose en el navegador.

### Vite

Vite será la herramienta de desarrollo del frontend. Sus funciones serán:

- Servir el proyecto localmente durante el aprendizaje.
- Servir, resolver y transformar módulos modernos de JavaScript.
- Reflejar rápidamente los cambios durante el desarrollo.
- Preparar una versión optimizada para publicación.

Vite no reemplaza HTML, CSS, JavaScript, React ni un backend. En este proyecto,
`frontend/index.html` seguirá siendo el punto de entrada.

## 4. Estructura prevista de directorios

La siguiente estructura representa el destino arquitectónico. No implica que
todos los directorios deban crearse desde la primera lección.

```text
gestor-de-finanzas/
├── frontend/
│   ├── index.html
│   ├── public/
│   │   └── imagenes/
│   ├── src/
│   │   ├── main.js
│   │   ├── components/
│   │   │   ├── auth/
│   │   │   ├── dashboard/
│   │   │   ├── movements/
│   │   │   └── recurring/
│   │   ├── domain/
│   │   │   ├── finance/
│   │   │   └── validation/
│   │   ├── state/
│   │   ├── services/
│   │   │   ├── storage/
│   │   │   └── json-transfer/
│   │   └── styles/
│   │       ├── base.css
│   │       ├── layout.css
│   │       └── components.css
│   ├── .env.example
│   └── package.json
├── backend/
│   └── .env.example
├── .gitignore
└── README.md
```

Responsabilidades:

- `frontend/public/imagenes`: únicos assets gráficos públicos del frontend.
- `frontend/src/components`: comportamiento agrupado por función visible.
- `frontend/src/domain`: reglas financieras puras, independientes del DOM.
- `frontend/src/state`: fuente de verdad de la aplicación durante la sesión.
- `frontend/src/services`: acceso a mecanismos externos como almacenamiento y
  archivos.
- `frontend/src/styles`: estilos base, de distribución y de componentes.
- `backend`: reservado para la futura API de FastAPI.

## 5. Capas y dependencias

```text
Interfaz HTML/CSS y módulos de cada sección
                        ↓
             Estado y casos de uso
                ↙                 ↘
Reglas financieras puras    Servicio de persistencia
                                      ↓
                    localStorage e intercambio JSON
```

Reglas de separación:

- La interfaz presenta información y captura acciones del usuario.
- El estado conserva los datos actuales durante la sesión.
- El dominio valida movimientos y realiza cálculos.
- Los servicios conocen `localStorage` y los archivos JSON.
- El dominio no conoce el DOM, Vite, `localStorage` ni React.
- La interfaz no calcula por su cuenta un segundo saldo.

Esta separación permitirá cambiar la interfaz Vanilla por React y después
cambiar el almacenamiento local por una API sin reescribir las reglas
financieras.

## 6. Secciones funcionales

### Acceso de demostración

Mostrará un formulario para practicar HTML, validación y navegación. No será una
autenticación segura y no utilizará credenciales reales.

### Panel principal

Mostrará:

- Capital inicial.
- Saldo calculado.
- Total de ingresos.
- Total de gastos.
- Resumen del periodo seleccionado.

### Registro de movimientos

Permitirá registrar ingresos y gastos con importe, concepto, categoría y fecha.

### Historial

Listará los movimientos y permitirá filtrar por tipo, categoría y periodo. Las
funciones de edición y eliminación se incorporarán en una lección posterior.

### Movimientos recurrentes

Guardará plantillas para ingresos o gastos mensuales. Una plantilla recurrente
no será todavía un movimiento efectuado: deberá convertirse una sola vez en
movimiento para cada periodo, evitando duplicados.

### Copias de datos

Permitirá exportar e importar un respaldo JSON. La importación deberá validar la
versión y la estructura antes de reemplazar los datos locales.

## 7. Modelo conceptual de datos

El estado financiero tendrá cuatro grupos principales:

| Grupo | Contenido |
| --- | --- |
| Versión | Versión del esquema para poder migrar respaldos en el futuro |
| Configuración | Moneda y capital inicial |
| Movimientos | Ingresos y gastos realmente efectuados |
| Reglas recurrentes | Plantillas mensuales activas o inactivas |

Cada movimiento tendrá conceptualmente:

- Un identificador único.
- Un tipo limitado a ingreso o gasto.
- Un importe entero positivo.
- Un concepto o descripción.
- Una categoría.
- La fecha del movimiento.
- La fecha en la que fue creado.

Cada regla recurrente tendrá conceptualmente:

- Un identificador único.
- Tipo, importe, concepto y categoría.
- Frecuencia y día previsto del mes.
- Estado activo o inactivo.
- Referencia del último periodo procesado para evitar duplicados.

## 8. Reglas financieras

El saldo será un valor calculado:

```text
saldo = capital inicial + suma de ingresos - suma de gastos
```

El saldo no se guardará como una segunda fuente de verdad. Guardar tanto los
movimientos como un saldo modificable podría producir inconsistencias.

Reglas adicionales:

- Los importes se representarán como enteros en pesos colombianos, no como
  números decimales de punto flotante.
- El importe siempre será positivo; el tipo indicará si suma o resta.
- Las fechas usarán un formato estándar y no ambiguo.
- “5.000 pesos” y “5.000 mil pesos” no significan lo mismo; la interfaz exigirá
  una cantidad exacta.
- Se guardarán movimientos financieros, no cada clic del usuario.
- La primera fase no tendrá auditoría: editar actualizará un movimiento y
  eliminar lo retirará del estado. Un historial inmutable de cambios será una
  posible función avanzada del backend.

## 9. Flujo de un movimiento

```text
El usuario completa el formulario
                ↓
La interfaz captura la acción
                ↓
El dominio valida tipo, importe, concepto y fecha
                ↓
El caso de uso crea el movimiento
                ↓
El estado incorpora el movimiento
                ↓
El servicio persiste el estado
                ↓
La interfaz vuelve a mostrar totales e historial
```

Si la validación falla, el estado y el almacenamiento no deben modificarse.

## 10. JSON y persistencia local

JSON será un formato de intercambio y respaldo, no una base de datos.

En la etapa frontend:

- El estado existirá primero como objetos de JavaScript.
- Se serializará a JSON para guardarlo en `localStorage`.
- Podrá descargarse como respaldo.
- Un respaldo podrá importarse únicamente después de validarlo.

El navegador no puede modificar silenciosamente un archivo JSON dentro del
proyecto. Además, `localStorage` pertenece a un navegador, origen y dispositivo:
no sincroniza datos con el colaborador ni con otra computadora y puede borrarse.

Los datos financieros personales generados por la aplicación no se subirán al
repositorio de GitHub.

## 11. Login y variables de entorno

### Etapa frontend

El acceso será una simulación educativa. Servirá para aprender formularios,
validación, eventos y estado de sesión, pero no protegerá información.

Un `.env` del frontend no es un lugar seguro para contraseñas, tokens privados
ni claves secretas. Los valores que el frontend necesita terminan disponibles
para el navegador.

El frontend podrá usar variables de entorno únicamente para configuración
pública, como la futura URL de una API.

### Etapa backend

La autenticación real se implementará cuando exista FastAPI:

- El backend validará las credenciales.
- Las contraseñas se almacenarán mediante hash, nunca en texto plano.
- Los secretos existirán solamente en el entorno del backend.
- La base de datos asociará cada movimiento con su usuario.
- El backend aplicará autorización sobre cada operación.
- La sesión se transportará mediante un mecanismo seguro.

El archivo `.env` real no se versionará. El repositorio tendrá un
`.env.example` sin secretos para documentar los nombres de las variables.

## 12. Evolución prevista

### De Vanilla a React

```text
HTML + DOM manual → componentes React
Estado JavaScript → estado administrado por React
Eventos del DOM   → eventos declarativos de React
```

React sustituirá principalmente la forma de construir y actualizar la interfaz.
No sustituirá JavaScript, las reglas financieras ni necesariamente Vite.

### De almacenamiento local a FastAPI

```text
Actual:
Interfaz Vanilla → dominio JavaScript → localStorage/JSON

Intermedia:
Interfaz React → dominio JavaScript → localStorage/JSON

Final:
Interfaz React → cliente HTTP → FastAPI → base de datos
                                   └── autenticación real
```

FastAPI y la base de datos se convertirán en la fuente de verdad. JSON seguirá
siendo el formato de comunicación entre frontend y backend y un posible formato
de exportación.

## 13. Impacto en mantenibilidad y rendimiento

- Separar dominio e interfaz reduce el trabajo de la futura migración a React.
- Calcular el saldo desde los movimientos evita datos duplicados y errores de
  sincronización.
- Para el volumen educativo inicial, recorrer los movimientos para calcular
  totales será sencillo, rápido y más confiable que mantener varios totales.
- La modularidad permitirá probar las reglas financieras sin necesitar el
  navegador.
- Vite mejorará la experiencia de desarrollo y generará assets optimizados sin
  convertirse en una dependencia de las reglas del negocio.

## 14. Despliegue previsto

Durante las etapas Vanilla y React, el resultado estático del frontend podrá
publicarse en Vercel. Cuando exista FastAPI, el frontend desplegado se comunicará
con una API alojada en un entorno de backend y configurada mediante una URL
pública.

Las instrucciones operativas de despliegue se documentarán cuando exista una
aplicación ejecutable, porque todavía no hay un build ni variables reales que
configurar.

## 15. Fuera del alcance inicial

- Autenticación real.
- Credenciales reales en el frontend.
- Sincronización entre dispositivos.
- Datos compartidos entre usuarios.
- Base de datos.
- Backend.
- React.
- Automatización de movimientos recurrentes sin confirmación.
- Modelos de inteligencia artificial u ONNX.
