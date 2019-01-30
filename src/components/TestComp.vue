<template>
  <div
    ref="table"
    class="table"
    id="table"
  >
    <div
      v-for="album in albums"
      :key="album.id"
    >
      {{album.title}}
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
      countAlbum: 1
    };
  },

  methods: {
    // scroll() {
    //   this.$refs.table.addEventListener("scroll", item => {
    //     const wrapperScroll = this.$refs.table.scrollTop;
    //     const wrapperHeight = this.$refs.table.clientHeight;
    //     const albumHeight = this.$refs.album.clientHeight;
    //     console.log(item.path[0].scrollTop, wrapperHeight, albumHeight);
    //     if (wrapperScroll + wrapperHeight === albumHeight) {
    //       console.log("dasdas");
    //       // this.countAlbum++;
    //       // this.albumsChunk = this.albums.slice(0, this.countAlbum);
    //     }
    //   });
    // }
    scroll() {
      const wrapper = document.getElementById("table");
      wrapper.addEventListener("scroll", item => {
        const wrapperScroll = wrapper.scrollTop;
        console.log(wrapperScroll);
      });
    }
  },

  mounted() {
    this.scroll();
  }
};
</script>

<style scoped>
.table {
  height: 500px;
  overflow: auto;
}
</style>
