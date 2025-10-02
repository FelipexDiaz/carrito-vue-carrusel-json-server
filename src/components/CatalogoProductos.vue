<template>
  <div>
    <h1>Catálogo de Productos</h1>

    <div v-if="loading">Cargando productos...</div>

    <!-- Carrusel de productos -->
    <div v-else>
      <Carousel
        :items-to-show="3"
        :wrap-around="true"
        :autoplay="3000"
        :breakpoints="breakpoints"
        class="carrusel"
      >
        <Slide v-for="prod in productos" :key="prod.id">
          <TarjetaProducto
            v-bind="{ ...prod, id: Number(prod.id) }"
            @agregar="agregarAlCarrito"
            @click="mostrarDetalle(prod)"
          >
            <template v-if="prod.oferta">
              <span class="oferta">🔥 En oferta!</span>
            </template>
          </TarjetaProducto>
        </Slide>

        <!-- Botones de navegación y paginación -->
        <template #addons>
          <Navigation />
          <Pagination />
        </template>
      </Carousel>
    </div>

    <!-- Componente dinámico para detalle -->
    <component
      v-if="productoSeleccionado"
      :is="DetalleProducto"
      :producto="productoSeleccionado"
      @cerrar="productoSeleccionado = null"
    />

    <!-- Componente Carrito -->
    <Carrito :carrito="carrito" @eliminar="eliminarDelCarrito" />
  </div>
</template>

<script>
import TarjetaProducto from "./TarjetaProducto.vue";
import DetalleProducto from "./DetalleProducto.vue";
import Carrito from "./Carrito.vue";
import { Carousel, Slide, Navigation, Pagination } from "vue3-carousel";
import "vue3-carousel/dist/carousel.css";

export default {
  name: "CatalogoProductos",
  components: { TarjetaProducto, DetalleProducto, Carrito, Carousel, Slide, Navigation, Pagination },
  data() {
    return {
      productos: [],
      productoSeleccionado: null,
      carrito: [],
      loading: true,
      breakpoints: {
        640: { itemsToShow: 1 },
        1024: { itemsToShow: 2 },
        1440: { itemsToShow: 3 }
      }
    };
  },
  methods: {
    async cargarProductos() {
      try {
        const res = await fetch("http://localhost:3000/productos");
        this.productos = await res.json();
      } catch (error) {
        console.error("Error cargando productos:", error);
      } finally {
        this.loading = false;
      }
    },
    async cargarCarrito() {
      try {
        const res = await fetch("http://localhost:3000/carrito");
        this.carrito = await res.json();
      } catch (error) {
        console.error("Error cargando carrito:", error);
      }
    },
    async agregarAlCarrito(id) {
      const producto = this.productos.find(p => Number(p.id) === Number(id));
      if (!producto || producto.stock <= 0) {
        alert("No hay stock disponible");
        return;
      }

      try {
        // Descontar stock en productos
        const nuevoStock = producto.stock - 1;
        await fetch(`http://localhost:3000/productos/${producto.id}`, {
          method: "PATCH",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ stock: nuevoStock })
        });
        producto.stock = nuevoStock;

        // Si ya existe en carrito, sumar cantidad
        const itemEnCarrito = this.carrito.find(item => Number(item.id) === Number(id));
        if (itemEnCarrito) {
          const nuevaCantidad = itemEnCarrito.cantidad + 1;
          await fetch(`http://localhost:3000/carrito/${itemEnCarrito.id}`, {
            method: "PATCH",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({ cantidad: nuevaCantidad })
          });
          itemEnCarrito.cantidad = nuevaCantidad;
        } else {
          const nuevoItem = { ...producto, cantidad: 1 };
          const res = await fetch("http://localhost:3000/carrito", {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify(nuevoItem)
          });
          const data = await res.json();
          this.carrito.push(data);
        }

        alert(`${producto.nombre} agregado al carrito 🛒`);
      } catch (error) {
        console.error("Error agregando al carrito:", error);
      }
    },
    mostrarDetalle(prod) {
      this.productoSeleccionado = prod;
    },
    async eliminarDelCarrito(id) {
      try {
        const item = this.carrito.find(i => Number(i.id) === Number(id));
        if (!item) return;

        if (item.cantidad > 1) {
          const nuevaCantidad = item.cantidad - 1;
          await fetch(`http://localhost:3000/carrito/${item.id}`, {
            method: "PATCH",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({ cantidad: nuevaCantidad })
          });
          item.cantidad = nuevaCantidad;
        } else {
          await fetch(`http://localhost:3000/carrito/${item.id}`, { method: "DELETE" });
          this.carrito = this.carrito.filter(i => Number(i.id) !== Number(id));
        }

        // Restaurar stock en productos
        const producto = this.productos.find(p => Number(p.id) === Number(id));
        if (producto) {
          const nuevoStock = producto.stock + 1;
          await fetch(`http://localhost:3000/productos/${producto.id}`, {
            method: "PATCH",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({ stock: nuevoStock })
          });
          producto.stock = nuevoStock;
        }
      } catch (error) {
        console.error("Error eliminando del carrito:", error);
      }
    }
  },
  async mounted() {
    await this.cargarProductos();
    await this.cargarCarrito();
  }
};
</script>

<style scoped>
.oferta {
  display: inline-block;
  margin-top: 0.5rem;
  padding: 0.2rem 0.5rem;
  background: red;
  color: white;
  font-weight: bold;
  border-radius: 4px;
}
.carrusel {
  margin: 1rem 0;
}
</style>
