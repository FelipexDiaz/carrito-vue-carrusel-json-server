<template>
  <div class="tarjeta" :class="{ 'sin-stock': stock === 0 }">
    <img :src="imagen" :alt="nombre" class="imagen" />

    <h2>{{ nombre }}</h2>
    <p>Precio: ${{ precio }}</p>
    <p v-if="stock > 0">Stock: {{ stock }}</p>
    <p v-else>Sin stock</p>
    <p>{{ descripcion }}</p>

    <!-- Botón para emitir evento -->
    <button :disabled="stock === 0" @click="agregarAlCarrito">
      Agregar al carrito
    </button>

    <!-- Slot para contenido extra -->
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: "TarjetaProducto",
  props: {
    id: { type: Number, required: true },
    nombre: { type: String, required: true },
    precio: { type: Number, required: true },
    imagen: { type: String, required: true },
    stock: { type: Number, required: true },
    descripcion: { type: String, required: true }
  },
  methods: {
    agregarAlCarrito() {
      this.$emit("agregar", this.id);
    }
  },
  created() {
    console.log(`🟢 Componente TarjetaProducto creado: ${this.nombre}`);
  },
  mounted() {
    console.log(`✅ TarjetaProducto montado en el DOM: ${this.nombre}`);
  },
  unmounted() {
    console.log(`❌ TarjetaProducto destruido: ${this.nombre}`);
  }
};
</script>

<style scoped>
.tarjeta {
  border: 1px solid #ddd;
  padding: 1rem;
  margin: 1rem;
  border-radius: 8px;
  text-align: center;
  transition: 0.3s;
}
.tarjeta img {
  width: 120px;
  height: 120px;
  object-fit: contain;
}
.sin-stock {
  opacity: 0.5;
  background-color: #f8d7da;
}
button {
  margin-top: 0.5rem;
  padding: 0.5rem;
  cursor: pointer;
}
</style>
