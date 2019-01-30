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
      <!-- <TableData :albums="fullData || []" /> -->
    </div>
    <TestComp :albums="fullData || []" />
  </div>
</template>

<script>
import PallindromeCheck from "./components/PalindromeCheck";
import FruitsValidate from "./components/FruitsValidate";
import ProductsList from "./components/ProductsList";
import TableData from "./components/TableData";
import TestComp from "./components/TestComp";

export default {
  name: "app",

  components: {
    PallindromeCheck,
    FruitsValidate,
    ProductsList,
    TableData,
    TestComp
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

    // ===========================
    Promise.all([
      fetch("https://jsonplaceholder.typicode.com/albums").then(response =>
        response.json()
      ),
      fetch("https://jsonplaceholder.typicode.com/photos").then(response =>
        response.json()
      )
    ]).then(data => {
      let newArr = [];

      data[0].forEach(album => {
        const createAlbum = [];
        createAlbum.push(album);
        createAlbum[0].photos = [];
        data[1].forEach(photo => {
          if (album.id === photo.albumId) {
            createAlbum[0].photos.push(photo);
          }
          return;
        });
        newArr.push(...createAlbum);
      });
      this.fullData = newArr.slice();
    });
  }
};
</script>
// Write a Vue app to display data in table with the following functionalities
// fixed header row (album.id, album.title, photos.title, photos.thumbnail)
// Specific fields sortable(album.id,albums.title, photos.title)
// Searchable on albums.title and photos.title
// Scrollable (show 25 rows by default)
// Custom filters - album.title
// thumbnail should appear as an image
// Data url:
// https://jsonplaceholder.typicode.com/albums
// https://jsonplaceholder.typicode.com/photos


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
</style>
