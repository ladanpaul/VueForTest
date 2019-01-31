<template>
  <div>
    <span>
      pls, look to console
    </span>
    <h1>
      Tasks:
    </h1>

    <div class="palindrome-wrapper box">
      <input
        type="text"
        placeholder="palindrome validation"
        v-model.lazy="word"
      >

      <PallindromeCheck
        :word="word"
        class="palindrome"
      />
    </div>
    <div class="fruits-validation box">
      <input
        type="text"
        placeholder="enter your fruit"
        @change="fruit = $event.target.value"
      >

      <FruitsValidate
        class="fruits-text"
        v-model="fruit"
      />
    </div>
    <div class="products-wrapper box">
      <div>
        <input
          type="text"
          placeholder="greater than number"
          @change="qtyGreater = $event.target.value"
        >
        <input
          type="text"
          placeholder="cost by name"
          @change="costByName = $event.target.value"
        >
      </div>

      <ProductsList
        :qtyGreater="qtyGreater"
        :nameProduct="costByName"
      />
    </div>
    <div class="table-wrapper">
      <TableData :albums="fullData || []" />
    </div>
  </div>
</template>

<script>
import PallindromeCheck from "./components/PalindromeCheck";
import FruitsValidate from "./components/FruitsValidate";
import ProductsList from "./components/ProductsList";
import TableData from "./components/TableData";

export default {
  name: "app",

  components: {
    PallindromeCheck,
    FruitsValidate,
    ProductsList,
    TableData
  },

  data() {
    return {
      word: "",
      fruit: "",
      qtyGreater: null,
      costByName: null,
      fullData: null
    };
  },

  methods: {
    getFullData(data) {
      let newArr = [];
      const albums = data[0];
      const photos = data[1];

      albums.forEach(album => {
        const createAlbum = [];
        createAlbum.push(album);
        createAlbum[0].photos = [];

        photos.forEach(photo => {
          if (album.id === photo.albumId) {
            createAlbum[0].photos.push(photo);
          }
          return;
        });

        newArr.push(...createAlbum);
      });

      this.fullData = newArr.slice();
    }
  },

  created() {
    // destructuring, task 1;
    const expense = {
      type: "Business",
      amount: "$50"
    };

    const { type, amount } = expense;
    console.log("type -> ", type);
    console.log("amount -> ", amount);

    // this ( context )
    const team = {
      members: ["Superman", "Batman", "Wonder Woman"],
      teamName: "Justice league",
      teamSummary: function() {
        return this.members.map(function(member) {
          return `${member} is on team ${this.teamName}`;
        });
      }
    }; //Won't work, cuz "this" for "map function" is teamSummary, where we don't have teamName ( undefined ).
    // We can use the next variants:

    const teamSelf = {
      members: ["Superman", "Batman", "Wonder Woman"],
      teamName: "Justice league",
      teamSummary: function() {
        let self = this;
        return this.members.map(function(member) {
          console.log(`Self context -> ${member} is on team ${self.teamName}`);
          return `${member} is on team ${self.teamName}`;
        });
      }
    };

    teamSelf.teamSummary();

    // also we can use ES6 arrow function

    const teamArrowFunc = {
      members: ["Superman", "Batman", "Wonder Woman"],
      teamName: "Justice league",
      teamSummary() {
        return this.members.map(member => {
          console.log(`ES6 arrow -> ${member} is on team ${this.teamName}`);
          return `${member} is on team ${this.teamName}`;
        });
      }
    };

    teamArrowFunc.teamSummary();

    // =========================== TO TABLE
    const getAlbum = fetch("https://jsonplaceholder.typicode.com/albums");
    const getPhotos = fetch("https://jsonplaceholder.typicode.com/photos");

    Promise.all([
      getAlbum.then(response => response.json()),
      getPhotos.then(response => response.json())
    ]).then(data => {
      // I know that this isn't good way, but I need to get like this)
      // In real life back end can give like this....
      this.getFullData(data);
    });
  }
};
</script>

<style scoped>
* {
  font-family: Arial;
}

.box {
  margin-top: 10px;
  padding: 10px;
  border: 1px solid grey;
}

.palindrome-wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
}

.palindrome {
  margin-left: 10px;
}

.fruits-validation {
  display: flex;
  justify-content: center;
  align-items: center;
}

.fruits-text {
  margin-left: 10px;
}

.products-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.table-wrapper {
  margin-top: 20px;
}
</style>
