<template>
  <div>
    <!-- Selector de color -->
    <div class="color-selector">
      <div v-for="(color, index) in colors" :key="index" :style="{ backgroundColor: color }" @click="updateColor(color)"></div>
    </div>

    <!-- Producto XS -->
    <div class="product">
      <li class="attribute_slug">XS</li>
      <input type="number" inputmode="decimal" step="1" autocomplete="off" v-model="quantity" @input="updateTotal" class="wmvatc_qty" min="0" data-min_qty="1" data-price="4.54" data-display-price="4,54" max="2000" data-drop="" id="wmvatc_qty_XS" data-date="05/08/2024" data-id="XS" data-colortype="591" data-size="XS" placeholder="0">
      <li class="display_price_XS">{{ displayPrice }}</li>
    </div>

    <!-- Selector de estampado -->
    <div class="product">
      <label for="estampado">Estampado:</label>
      <select id="estampado" v-model="selectedEstampado" @change="updateEstampado">
        <option v-for="(price, option) in estampadoPrices" :key="option" :value="price">{{ option }}</option>
      </select>
    </div>

    <!-- Total -->
    <div>
      <label>Total:</label>
      <input type="text" id="total" v-model="total" readonly>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      colors: ['#ffffff', '#000000', '#001d43'], // Agrega más colores aquí si es necesario
      selectedColor: '',
      quantity: 0,
      displayPrice: '4.54€', // Precio inicial para el tamaño XS
      estampadoPrices: {
        'Sin estampado': 0,
        'Estampado Delante y Detrás (+12.5€)': 12.5,
        'Estampado Delante (+5.00€)': 5,
        'Estampado Trasero (+7.50€)': 7.5,
        'Lateral Derecho e Izquierdo (+5€)': 5,
        'Lateral Derecho (+2.50€)': 2.5,
        'Lateral Izquierdo (+2.50€)': 2.5,
        // Agrega más opciones de estampado aquí si es necesario
      },
      selectedEstampado: 0,
      estampadoPrice: 0,
      total: ''
    };
  },
  methods: {
    updateColor(color) {
      this.selectedColor = color;
      this.updatePrices();
    },
    updatePrices() {
      if (this.selectedColor) {
        Object.keys(this.estampadoPrices).forEach((option) => {
          this.updatePrice(option);
        });
        this.updateTotal();
      } else {
        this.hidePrices();
      }
    },
    hidePrices() {
      Object.keys(this.estampadoPrices).forEach((option) => {
        let priceElement = document.getElementById("display_price_" + option);
        priceElement.textContent = ""; // Oculta el precio
      });
      document.getElementById("total").value = ""; // Oculta el precio total
    },
    updatePrice(option) {
      let quantity = parseInt(this.quantity) || 0;
      let priceElement = document.getElementById("display_price_" + option);
      let price;

      if (this.selectedColor === "white") {
        price = 4.54;
      } else {
        price = 5.54;
      }

      if (quantity >= 5 && quantity < 100) {
        price = this.selectedColor === "white" ? 3.97 : 5.04;
      } else if (quantity >= 100) {
        price = this.selectedColor === "white" ? 3.25 : 3.8;
      }

      priceElement.textContent = price.toFixed(2) + "€";

      // Actualiza el precio total
      this.updateTotal();
    },
    updateEstampado() {
      this.estampadoPrice = parseFloat(this.selectedEstampado) || 0;
      this.updateTotal();
    },
    updateTotal() {
      let total = 0;
      Object.keys(this.estampadoPrices).forEach((option) => {
        let quantity = parseInt(this.quantity) || 0;
        let priceElement = document.getElementById("display_price_" + option);
        let price = parseFloat(priceElement.textContent);
        total += quantity * price;
      });
      total += this.estampadoPrice * (parseInt(this.quantity) || 0); // Suma el precio del estampado multiplicado por la cantidad de camisetas XS
      this.total = total.toFixed(2) + "€";
    }
  },
  created() {
    this.hidePrices();
  }
};
</script>

<style scoped>
/* Agrega tus estilos CSS aquí si es necesario */
</style>
