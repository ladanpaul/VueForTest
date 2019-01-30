<template>
  <div>
    <div>
      <span>
        Cost Of Product For Qty Greater Than:
      </span>
      <span v-if="qtyGreater">
        {{ filteredGreater }}
      </span>
    </div>
    <div>
      <span>
        Cost Of Product By Name: {{nameProduct}} count =
      </span>
      <span v-if="nameProduct">
        {{ countByName }}
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: "ProductsList",

  props: {
    qtyGreater: String,
    nameProduct: String
  },

  data() {
    return {
      filteredGreater: null,
      countByName: null,
      productCart: [
        { productname: "iphone-x", qty: 10, price: 1000 },
        { productname: "macbook pro", qty: 200, price: 2000 },
        { productname: "iwatch", qty: 100, price: 550 },
        { productname: "macbook air", qty: 100, price: 1000 },
        { productname: "iphone 8", qty: 300, price: 700 },
        { productname: "iphone 7", qty: 100, price: 550 },
        { productname: "ipad Retina", qty: 20, price: 1000 },
        { productname: "ipad", qty: 10, price: 700 },
        { productname: "Magic Mouse", qty: 50, price: 300 },
        { productname: "Magic Trackpad", qty: 75, price: 200 }
      ]
    };
  },

  watch: {
    qtyGreater() {
      this.CostOfProductForQtyGreaterThan(this.qtyGreater);
    },
    nameProduct() {
      this.CostOfProduct(this.nameProduct);
    }
  },

  methods: {
    reduceByProduct(arr, firstAcc) {
      return arr.reduce((acc, product) => {
        return (acc += product.qty * product.price);
      }, firstAcc);
    },

    CostOfProductForQtyGreaterThan(number) {
      this.filteredGreater = this.productCart.filter(item => {
        return item.qty > number;
      });

      this.filteredGreater = this.reduceByProduct(this.filteredGreater, 0);
    },

    CostOfProduct(name) {
      this.countByName = this.productCart.filter(item => {
        return item.productname === name;
      });

      this.countByName = this.reduceByProduct(this.countByName, 0);
    }
  }
};
</script>

