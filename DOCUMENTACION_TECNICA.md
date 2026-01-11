# Documentación Técnica - PokeAPI React

## 📋 Índice

1. [Descripción General](#descripción-general)
2. [Arquitectura del Proyecto](#arquitectura-del-proyecto)
3. [Tecnologías Utilizadas](#tecnologías-utilizadas)
4. [Estructura de Directorios](#estructura-de-directorios)
5. [Componentes](#componentes)
6. [Flujo de Datos](#flujo-de-datos)
7. [API Externa](#api-externa)
8. [Instalación y Configuración](#instalación-y-configuración)
9. [Scripts Disponibles](#scripts-disponibles)
10. [Consideraciones Técnicas](#consideraciones-técnicas)

---

## Descripción General

**PokeAPI React** es una aplicación web desarrollada con React que permite consultar información de Pokémon utilizando la API pública de [PokeAPI](https://pokeapi.co). La aplicación ofrece funcionalidades de búsqueda por nombre, visualización en grid con scroll infinito, y modales detallados con estadísticas completas de cada Pokémon.

### Características Principales

- 🔍 Búsqueda de Pokémon por nombre
- 📱 Diseño responsive
- 🎨 Interfaz visual con colores dinámicos según el tipo de Pokémon
- 📊 Visualización de estadísticas y habilidades
- ♾️ Scroll infinito para cargar más Pokémon
- 🎯 Modal detallado con información completa

---

## Arquitectura del Proyecto

El proyecto sigue una arquitectura basada en componentes de React con las siguientes características:

- **Patrón de Componentes**: Uso de componentes de clase de React
- **Estado Centralizado**: El estado principal se maneja en el componente `App`
- **Componentes Presentacionales**: Componentes reutilizables para UI
- **Layout Components**: Componentes de estructura (Container, Grid)
- **Utilidades**: Funciones auxiliares para manejo de iconos

### Diagrama de Componentes

```
App (Estado Principal)
├── Navbar
├── Search
├── Grid
│   └── Card (múltiples instancias)
│       └── Modal
└── Footer
```

---

## Tecnologías Utilizadas

### Dependencias Principales

| Paquete | Versión | Propósito |
|---------|---------|-----------|
| `react` | ^18.1.0 | Biblioteca principal de React |
| `react-dom` | ^18.1.0 | Renderizado de React en el DOM |
| `react-scripts` | 5.0.1 | Scripts y configuración de Create React App |

### Dependencias de Desarrollo

| Paquete | Versión | Propósito |
|---------|---------|-----------|
| `@testing-library/react` | ^13.1.1 | Testing de componentes React |
| `@testing-library/jest-dom` | ^5.16.4 | Matchers adicionales para Jest |
| `@testing-library/user-event` | ^13.5.0 | Simulación de eventos de usuario |
| `gh-pages` | ^3.2.3 | Despliegue en GitHub Pages |
| `web-vitals` | ^2.1.4 | Métricas de rendimiento web |

### Tecnologías Adicionales

- **HTML5**: Estructura semántica
- **CSS3**: Estilos y diseño responsive
- **JavaScript ES6+**: Lógica de la aplicación
- **Fetch API**: Comunicación con la API externa

---

## Estructura de Directorios

```
pokeapi-react-master/
├── public/
│   ├── favicon.ico
│   ├── index.html
│   ├── logo192.png
│   ├── logo512.png
│   ├── manifest.json
│   └── robots.txt
├── src/
│   ├── assets/
│   │   ├── icons/
│   │   │   ├── busqueda.svg
│   │   │   ├── pokeball.png
│   │   │   └── pokemon/
│   │   │       └── types/ (18 tipos de Pokémon)
│   │   └── screenshots/
│   ├── components/
│   │   ├── card.jsx
│   │   ├── footer.jsx
│   │   ├── modal.jsx
│   │   ├── navbar.jsx
│   │   └── search.jsx
│   ├── layout/
│   │   ├── container.jsx
│   │   └── grid.jsx
│   ├── utils/
│   │   └── icons.js
│   ├── App.css
│   ├── App.jsx
│   ├── colors.css
│   ├── index.css
│   └── index.jsx
├── package.json
├── package-lock.json
└── README.md
```

---

## Componentes

### App.jsx

**Tipo**: Componente de Clase  
**Responsabilidad**: Componente principal que gestiona el estado global de la aplicación

#### Estado

```javascript
{
  pokemons: [],      // Lista de Pokémon cargados
  total: 0,          // Contador total de Pokémon cargados
  notFound: false,   // Flag para Pokémon no encontrado
  search: [],        // Resultados de búsqueda
  searching: false   // Flag de estado de búsqueda
}
```

#### Métodos Principales

- **`handleSearch(textSearch)`**: Maneja la búsqueda de Pokémon por nombre
  - Realiza petición a `https://pokeapi.co/api/v2/pokemon/{name}`
  - Actualiza el estado con el resultado o marca como no encontrado

- **`showPokemons(limit, offset)`**: Carga una lista de Pokémon
  - Por defecto carga 20 Pokémon
  - Realiza múltiples peticiones para obtener datos completos
  - Implementa paginación mediante offset

- **`nextPokemon()`**: Carga el siguiente lote de Pokémon
  - Incrementa el offset basado en el total actual

#### Ciclo de Vida

- **`componentDidMount()`**: Carga los primeros 20 Pokémon al montar el componente

---

### Search.jsx

**Tipo**: Componente de Clase  
**Responsabilidad**: Componente de búsqueda con input y botón

#### Estado

```javascript
{
  search: ''  // Texto de búsqueda actual
}
```

#### Funcionalidad

- Input de búsqueda con placeholder
- Búsqueda al presionar Enter (keyCode 13) o hacer clic en el botón
- Limpia la búsqueda cuando el input está vacío
- Auto-focus en el input

#### Props

- `onHandleSearch`: Función callback que recibe el texto de búsqueda

---

### Grid.jsx

**Tipo**: Componente de Clase  
**Responsabilidad**: Renderiza una grilla de tarjetas de Pokémon

#### Props

- `pokemons`: Array de objetos Pokémon a mostrar
- `next`: Función callback para cargar más Pokémon

#### Funcionalidad

- Renderiza múltiples componentes `Card`
- Muestra botón "Show more" cuando hay 20 o más Pokémon
- Implementa scroll infinito

---

### Card.jsx

**Tipo**: Componente de Clase  
**Responsabilidad**: Tarjeta individual de Pokémon

#### Estado

```javascript
{
  showModal: false  // Control de visibilidad del modal
}
```

#### Props

- `pokemon`: Objeto con información del Pokémon

#### Funcionalidad

- Muestra información básica: nombre, número de orden, tipo, imagen
- Colores dinámicos según el tipo del Pokémon usando variables CSS
- Al hacer clic, abre el modal con detalles
- Toggle de clase `dark` en el body al abrir/cerrar modal

#### Datos Mostrados

- Número de orden (`pokemon.order`)
- Nombre (`pokemon.name`)
- Tipo principal (`pokemon.types[0].type.name`)
- Imagen frontal (`pokemon.sprites.front_default`)

---

### Modal.jsx

**Tipo**: Componente de Clase  
**Responsabilidad**: Modal con información detallada del Pokémon

#### Props

- `pokemon`: Objeto con información completa del Pokémon
- `onHandleModal`: Función callback para cerrar el modal

#### Información Mostrada

1. **Características**:
   - Tipo del Pokémon (con icono)
   - Altura (`pokemon.height`)
   - Peso (`pokemon.weight`)
   - Generación (si aplica, `pokemon.past_types`)

2. **Descripción**:
   - Imagen del Pokémon
   - Nombre
   - Texto descriptivo (actualmente placeholder)

3. **Habilidades** (`pokemon.abilities`):
   - Lista de todas las habilidades del Pokémon

4. **Estadísticas** (`pokemon.stats`):
   - Nombre de la estadística
   - Valor base (`base_stat`)
   - Barra de progreso visual con colores dinámicos
   - Límite máximo visual del 100%

#### Colores de Estadísticas

```javascript
['#FC6B6E', '#2196F3', '#094BE8', '#2196F3', '#3ED1E0', '#CF9B48']
```

---

### Navbar.jsx

**Tipo**: Componente de Clase  
**Responsabilidad**: Barra de navegación superior

#### Props

- `title`: Título a mostrar en la navbar

---

### Container.jsx

**Tipo**: Componente de Clase  
**Responsabilidad**: Contenedor wrapper para el layout

#### Props

- `children`: Contenido a renderizar dentro del contenedor

---

### Footer.jsx

**Tipo**: Componente Funcional/Clase  
**Responsabilidad**: Pie de página de la aplicación

---

## Flujo de Datos

### Flujo de Búsqueda

```
Usuario escribe en Search
    ↓
Search actualiza su estado local
    ↓
Usuario presiona Enter o clic en botón
    ↓
Search llama a App.handleSearch(text)
    ↓
App realiza fetch a PokeAPI
    ↓
App actualiza estado (search o notFound)
    ↓
App renderiza Grid con resultados de búsqueda
    ↓
Grid renderiza Cards con Pokémon encontrado
```

### Flujo de Carga Inicial

```
App se monta (componentDidMount)
    ↓
App.showPokemons() se ejecuta
    ↓
Fetch a PokeAPI para lista (limit=20, offset=0)
    ↓
Múltiples fetches para datos completos de cada Pokémon
    ↓
Promise.all() espera todas las peticiones
    ↓
Estado actualizado con 20 Pokémon
    ↓
Grid renderiza 20 Cards
```

### Flujo de Scroll Infinito

```
Usuario hace clic en "Show more"
    ↓
Grid.handleButton() ejecuta
    ↓
Grid llama a App.nextPokemon()
    ↓
App.showPokemons(20, total) con nuevo offset
    ↓
Nuevos Pokémon se agregan al array existente
    ↓
Grid re-renderiza con más Cards
```

### Flujo de Modal

```
Usuario hace clic en Card
    ↓
Card.handleModal() ejecuta
    ↓
Card actualiza estado (showModal: true)
    ↓
Body recibe clase 'dark'
    ↓
Modal se renderiza con datos del Pokémon
    ↓
Usuario cierra modal
    ↓
Card.handleModal() ejecuta nuevamente
    ↓
showModal: false, clase 'dark' removida
```

---

## API Externa

### PokeAPI

**URL Base**: `https://pokeapi.co/api/v2`

### Endpoints Utilizados

#### 1. Obtener Pokémon por Nombre

```
GET https://pokeapi.co/api/v2/pokemon/{name}
```

**Parámetros**:
- `name`: Nombre del Pokémon (case-insensitive)

**Respuesta**: Objeto con información completa del Pokémon

**Ejemplo**:
```javascript
fetch('https://pokeapi.co/api/v2/pokemon/pikachu')
```

#### 2. Listar Pokémon (Paginado)

```
GET https://pokeapi.co/api/v2/pokemon?limit={limit}&offset={offset}
```

**Parámetros**:
- `limit`: Número de resultados por página (default: 20)
- `offset`: Número de resultados a saltar

**Respuesta**:
```json
{
  "count": 1154,
  "next": "https://pokeapi.co/api/v2/pokemon?offset=20&limit=20",
  "previous": null,
  "results": [
    {
      "name": "bulbasaur",
      "url": "https://pokeapi.co/api/v2/pokemon/1/"
    }
  ]
}
```

### Estructura de Datos del Pokémon

```typescript
interface Pokemon {
  id: number;
  name: string;
  order: number;
  height: number;
  weight: number;
  types: Array<{
    slot: number;
    type: {
      name: string;
      url: string;
    };
  }>;
  abilities: Array<{
    ability: {
      name: string;
      url: string;
    };
    is_hidden: boolean;
    slot: number;
  }>;
  stats: Array<{
    base_stat: number;
    effort: number;
    stat: {
      name: string;
      url: string;
    };
  }>;
  sprites: {
    front_default: string;
    // ... más sprites
  };
  past_types?: Array<{
    generation: {
      name: string;
      url: string;
    };
  }>;
}
```

### Manejo de Errores

- Si un Pokémon no se encuentra, la API retorna 404
- La aplicación captura el error y muestra mensaje "Pokemon not found"
- Se utiliza `.catch()` para manejar errores de JSON parsing

---

## Instalación y Configuración

### Requisitos Previos

- **Node.js**: Versión 14.0.0 o superior
- **npm**: Versión 6.0.0 o superior (incluido con Node.js)
- **Git**: Para clonar el repositorio (opcional)

### Pasos de Instalación

1. **Clonar el repositorio** (si aplica):
```bash
git clone https://github.com/Onnichan/pokeapi-react.git
cd pokeapi-react
```

2. **Instalar dependencias**:
```bash
npm install
```

O con yarn:
```bash
yarn install
```

3. **Iniciar el servidor de desarrollo**:
```bash
npm start
```

La aplicación se abrirá automáticamente en `http://localhost:3000`

### Variables de Entorno

El proyecto no requiere variables de entorno adicionales. La API de PokeAPI es pública y no requiere autenticación.

---

## Scripts Disponibles

### `npm start`

Inicia el servidor de desarrollo con hot-reload.

- **Puerto**: 3000 (por defecto)
- **Hot Reload**: Habilitado
- **Open Browser**: Automático

### `npm run build`

Crea una versión optimizada para producción.

- **Output**: Carpeta `build/`
- **Optimizaciones**: Minificación, tree-shaking, code splitting
- **Formato**: Archivos estáticos listos para desplegar

### `npm test`

Ejecuta la suite de tests.

- **Framework**: Jest + React Testing Library
- **Modo**: Watch mode por defecto
- **Coverage**: Disponible con flag `--coverage`

### `npm run eject`

Expone la configuración de webpack y otras herramientas.

⚠️ **Advertencia**: Esta operación es irreversible. Solo usar si necesitas personalización avanzada.

### `npm run deploy`

Despliega la aplicación en GitHub Pages.

- **Requisito**: Configuración de `homepage` en `package.json`
- **Branch**: `gh-pages`
- **Carpeta**: `build/`

---

## Consideraciones Técnicas

### Rendimiento

1. **Lazy Loading de Imágenes**:
   - Las imágenes de Pokémon usan `loading='lazy'` para carga diferida

2. **Múltiples Peticiones**:
   - Al cargar la lista inicial, se realizan múltiples peticiones HTTP
   - Se usa `Promise.all()` para paralelizar las peticiones
   - Consideración: Podría optimizarse con un endpoint batch si estuviera disponible

3. **Estado y Re-renders**:
   - El estado se actualiza de forma inmutada usando spread operator
   - Los componentes se re-renderizan cuando cambia el estado

### Manejo de Estado

- **Estado Local**: Cada componente gestiona su propio estado cuando es apropiado
- **Estado Compartido**: El componente `App` maneja el estado global
- **No se usa Redux o Context API**: El estado se pasa mediante props

### Estilos

- **CSS Modules**: No se utilizan, se usa CSS global
- **Variables CSS**: Se utilizan para colores dinámicos de tipos de Pokémon
- **Responsive Design**: Implementado mediante media queries en CSS

### Accesibilidad

- **Alt Text**: Las imágenes tienen atributos `alt` (algunos vacíos)
- **Semantic HTML**: Uso de elementos semánticos
- **Keyboard Navigation**: Búsqueda funciona con Enter
- **Auto-focus**: Input de búsqueda tiene auto-focus

### Mejoras Potenciales

1. **Manejo de Errores**:
   - Implementar retry logic para peticiones fallidas
   - Mostrar mensajes de error más descriptivos
   - Implementar loading states más granulares

2. **Optimización de Peticiones**:
   - Implementar caché de resultados
   - Usar React Query o SWR para manejo de datos
   - Implementar debounce en la búsqueda

3. **Testing**:
   - Aumentar cobertura de tests
   - Tests de integración para flujos completos
   - Tests E2E con Cypress o Playwright

4. **TypeScript**:
   - Migrar a TypeScript para type safety
   - Definir interfaces para datos de la API

5. **Estado Global**:
   - Considerar Context API o Redux para estado complejo
   - Implementar estado de carga y errores global

6. **PWA**:
   - Implementar Service Workers
   - Agregar funcionalidad offline
   - Mejorar manifest.json

7. **SEO**:
   - Implementar React Helmet para meta tags
   - Server-side rendering (Next.js)

---

## Estructura de Archivos CSS

### Archivos CSS Principales

- **`App.css`**: Estilos del componente App y componentes principales
- **`index.css`**: Estilos globales y reset
- **`colors.css`**: Variables CSS para colores de tipos de Pokémon

### Variables CSS de Tipos

El archivo `colors.css` define variables para cada tipo de Pokémon:

```css
--bg-poke-color-light-{tipo}
--bg-poke-color-dark-{tipo}
```

Ejemplo:
- `--bg-poke-color-light-fire`
- `--bg-poke-color-dark-fire`

---

## Compatibilidad de Navegadores

Según `browserslist` en `package.json`:

### Producción
- Navegadores con >0.2% de uso
- Excluye navegadores sin soporte
- Excluye Opera Mini

### Desarrollo
- Última versión de Chrome
- Última versión de Firefox
- Última versión de Safari

---

## Licencia

Este proyecto es privado según se indica en `package.json` (`"private": true`).

---

## Contacto y Soporte

Para más información sobre el proyecto, consultar el README.md o contactar al desarrollador original a través de [LinkedIn](https://www.linkedin.com/in/walter-daniel-huaynapata-aguilar-391041197/).

---

**Última actualización**: 2024  
**Versión del proyecto**: 0.1.0

