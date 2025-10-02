📦 Catálogo de Productos Interactivo

Este proyecto es un catálogo de productos interactivo construido con Vue 3, Vite y json-server.
Permite:

Visualizar productos en un carrusel responsive.

Agregar productos al carrito sumando cantidades.

Descontar/restaurar stock automáticamente.

Ver detalle de productos dinámicamente.

Usar un carrito como componente reutilizable.

🛠 Tecnologías

Vue 3 + Composition API

Vite

json-server (API REST falsa)

vue3-carousel (carrusel de productos)

📁 Estructura del proyecto
src/
├─ components/
│  ├─ TarjetaProducto.vue
│  ├─ DetalleProducto.vue
│  └─ Carrito.vue
├─ views/
│  └─ CatalogoProductos.vue
└─ main.js
db.json
package.json

🚀 Instalación

Clonar el repositorio:

git clone <URL_DEL_REPOSITORIO>
cd nombre-del-proyecto


Instalar dependencias:

npm install


Instalar json-server (si no está global):

npm install -g json-server

⚡ Desarrollo

Se configuró para ejecutar Vite y json-server en paralelo con concurrently.

npm run dev


Esto hará:

Vite: http://localhost:5173/

json-server: http://localhost:3000/

json-server servirá tu db.json y actualizará automáticamente el stock y el carrito.

🛒 Uso

Los productos se muestran en un carrusel.

Cada producto tiene un botón “Agregar al carrito”:

Si ya existe, aumenta la cantidad.

Si no existe, se agrega como nuevo item.

El carrito es un componente separado (Carrito.vue) que muestra:

Nombre, precio y cantidad de cada producto.

Botón para eliminar o disminuir cantidad.

Total del carrito calculado dinámicamente.

Hacer click sobre un producto abre un detalle dinámico (DetalleProducto.vue).

⚙ Configuración de json-server

El archivo db.json tiene la estructura:

{
  "productos": [
    {
      "id": 1,
      "nombre": "Laptop Gamer",
      "precio": 2500,
      "imagen": "https://picsum.photos/id/180/200/150",
      "stock": 5,
      "descripcion": "Laptop potente para gaming.",
      "oferta": true
    }
  ],
  "carrito": []
}


Puedes agregar más productos con imágenes válidas.

json-server actualiza stock y carrito en tiempo real.

🎨 Estilos y componentes

TarjetaProducto.vue: componente reutilizable con props, slots y ciclo de vida (created, mounted, destroyed).

Carrito.vue: componente separado para mostrar carrito.

DetalleProducto.vue: se renderiza dinámicamente al seleccionar un producto.

Carrusel: responsive, autoplay y navegación con vue3-carousel.

🖥 Comandos útiles
Comando	Descripción
npm run dev	Inicia Vite + json-server en paralelo
npm run build	Construye la app para producción
npm run serve	Sirve la build localmente
json-server --watch db.json --port 3000	Inicia json-server manualmente
📌 Tips

El carrito suma cantidades y resta stock automáticamente.

Se puede inyectar contenido extra en TarjetaProducto.vue usando slots (por ejemplo etiquetas de oferta).

Responsive:

Móvil: 1 producto visible

Tablet: 2 productos visibles

Desktop: 3 productos visibles
