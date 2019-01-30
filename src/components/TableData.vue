<template>
  <div>
    <!-- <input
      type="text"
      v-model="searchData"
    > -->
    <div
      class="table-wrapper"
      ref="table"
    >
      <div ref="album">
        <div
          class="album"
          v-for="(album, index) in albumsChunk"
          :key="album.id"
        >
          <span class="album-title">
            {{index + 1}}
            this Album -> {{album.title}}
          </span>
          <div
            class="photo"
            v-for="(photo, index) in album.photos"
            :key="photo.id"
          >
            {{index + 1}}
            title PHOTO -> {{photo}}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "TableData",

  props: {
    albums: {
      type: Array,
      required: true
    }
  },

  data() {
    return {
      albumsChunk: [],
      searchData: "",
      countAlbum: 1
    };
  },

  computed: {
    // filteredData() {
    //   return this.albums.filter(album => {
    //     return (
    //       // album.title.toLowerCase().indexOf(this.searchData.toLowerCase()) !==
    //       // -1
    //       album.title.toLowerCase().includes(this.searchData.toLowerCase())
    //     );
    //   });
    // }
  },

  methods: {
    scroll() {
      this.$refs.table.addEventListener("scroll", () => {
        const wrapperScroll = this.$refs.table.scrollTop;
        const wrapperHeight = this.$refs.table.clientHeight;
        const albumHeight = this.$refs.album.clientHeight;
        console.log(Math.ceil(wrapperScroll), wrapperHeight, albumHeight);
        if (wrapperScroll + wrapperHeight === albumHeight) {
          this.countAlbum++;
          this.albumsChunk = this.albums.slice(0, this.countAlbum);
        }
      });
    }
  },

  watch: {
    albums(newVal) {
      if (newVal) {
        this.albumsChunk = this.albums.slice(0, this.countAlbum);
      }
    }
  },

  mounted() {
    this.scroll();
  }
};
</script>

<style scoped>
.table-wrapper {
  height: 500px;
  border: 2px solid red;
  overflow: auto;
}

.album {
  position: relative;
  height: 100%;
  border: 1px solid blue;
  padding: 5px 10px;
}

.album-title {
  display: block;
  background: #fff;
  padding: 20px;
  text-align: center;
  border: 1px solid blue;
  margin: 0;
  position: sticky;
  top: -1px;
}

.photo {
  border: 1px solid red;
  background: pink;
}
</style>
