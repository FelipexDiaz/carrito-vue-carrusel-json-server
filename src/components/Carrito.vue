<template>
  <div class="carrito">
    <h2>🛒 Carrito</h2>
    <ul>
      <li v-for="item in carrito" :key="item.id" class="item-carrito">
        {{ item.nombre }} - ${{ item.precio }} × {{ item.cantidad }}
        <button @click="$emit('eliminar', item.id)">❌</button>
      </li>
    </ul>
    <p v-if="carrito.length === 0">El carrito está vacío</p>
    <p v-else class="total">💰 Total: ${{ total }}</p>
  </div>
</template>

<script>
export default {
  name: "Carrito",
  props: {
    carrito: {
      type: Array,
      required: true
    }
  },
  computed: {
    total() {
      return this.carrito.reduce((acc, item) => acc + item.precio * item.cantidad, 0);
    }
  }
};
</script>

<style scoped>
.carrito {
  margin-top: 2rem;
  padding: 1rem;
  border-top: 2px solid #ddd;
}
.item-carrito {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.3rem 0;
}
button {
  background: #dc3545;
  border: none;
  color: white;
  padding: 0.3rem 0.6rem;
  cursor: pointer;
  border-radius: 4px;
}
button:hover {
  background: #b02a37;
}
.total {
  font-weight: bold;
  margin-top: 1rem;
}
</style>
