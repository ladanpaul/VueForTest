<template>
  <div v-if="value">
    <span> {{ findedFruit }} </span>
  </div>
</template>

<script>
export default {
  name: "FruitsValidate",

  props: {
    value: {
      type: String,
      required: true
    }
  },

  data() {
    return {
      findedFruit: "",
      fruits: [
        "Apple",
        "Apricot",
        "Avocado",
        "banana",
        "Blackberry",
        "Blueberry",
        "Boysenberry",
        "Crab apples",
        "Currant",
        "Cherry",
        "Cherimoya",
        "Chico fruit",
        "Coconut",
        "Cranberry",
        "Custard apple",
        "Date",
        "dragonfruit",
        "durian",
        "Elderberry",
        "Feijoa",
        "Fig",
        "Goji berry",
        "Gooseberry",
        "grape",
        "Grapefruit",
        "Guava",
        "Honeyberry",
        "Huckleberry",
        "Jabuticaba",
        "Jackfruit",
        "Jambul",
        "jujube",
        "Juniper berry",
        "Kiwifruit",
        "Kumquat",
        "Lemon",
        "Lime",
        "Loquat",
        "Longan",
        "Lychee",
        "Mango",
        "Mangosteen",
        "Marionberry",
        "Melon",
        "Cantaloupe",
        "Honeydew",
        "Watermelon",
        "Miracle fruit",
        "Mulberry",
        "Nectarine",
        "Nance",
        "Olive",
        "Orange",
        "Blood orange",
        "Clementine",
        "Mandarine",
        "Tangerine",
        "Papaya",
        "Passionfruit",
        "Peach",
        "Pear",
        "Persimmon",
        "Plantain",
        "Plum",
        "Prune",
        "Pineapple",
        "Pluot",
        "Pomegranate",
        "Pomelo",
        "Quince",
        "Raspberry",
        "Rambutan",
        "Redcurrant",
        "Salal berry",
        "Salak",
        "Salmonberry",
        "Satsuma",
        "Soursop",
        "Star fruit",
        "Strawberry",
        "Tamarillo",
        "Tamarind",
        "Ugli fruit",
        "Yuzu"
      ]
    };
  },

  watch: {
    value() {
      this.fruitPresent(this.value);
    }
  },

  methods: {
    fruitPresent(item) {
      const fruit = item.toLowerCase();
      this.findedFruit = this.fruits.some(item => item.toLowerCase() === fruit);
    }
  }
};
</script>

