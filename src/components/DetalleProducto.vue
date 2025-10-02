<template>
  <div class="detalle">
    <h2>{{ producto.nombre }}</h2>
    <img :src="producto.imagen" alt="detalle" />
    <p>{{ producto.descripcion }}</p>
    <p><strong>Precio:</strong> ${{ producto.precio }}</p>
    <p v-if="producto.stock > 0">Stock disponible: {{ producto.stock }}</p>
    <p v-else>Producto agotado</p>

    <button @click="$emit('cerrar')">Cerrar</button>
  </div>
</template>

<script>
export default {
  name: "DetalleProducto",
  props: {
    producto: { type: Object, required: true }
  }
};
</script>

<style scoped>
.detalle {
  border: 2px solid #007bff;
  padding: 1rem;
  margin: 1rem;
  border-radius: 8px;
  background: #f0f8ff;
}
</style>
