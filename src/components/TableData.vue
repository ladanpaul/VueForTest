<template>
  <div>
    <h2 class="table-title">{{title}}</h2>
    <div class="filters">
      <input
        class="input"
        type="text"
        v-model="searchData"
        placeholder="Search albums"
      >
      <span>
        Albums count: {{filteredAlbum.length}}
      </span>
      <input
        class="input"
        type="text"
        v-model="searchPhoto"
        placeholder="Search photos"
      >
      <span>
        Photos count: {{filteredPhoto.length}}
      </span>
      <button
        @click="sortAlbum"
        class="filter-btn"
      >
        Sort By Albums
      </button>
      <button
        @click="sortPhotos"
        class="filter-btn"
      >
        Sort By Photos
      </button>
    </div>
    <div
      class="table-wrapper"
      ref="table"
      @scroll="scroll"
    >
      <div ref="album">
        <span
          v-if="!filteredAlbum.length || !filteredPhoto.length"
          class="search-emtpy"
        >Empty Search Data</span>
        <div
          class="album"
          v-for="album in searchData ? filteredAlbum : albumsChunk"
          :key="album.id"
        >
          <span
            class="album-title"
            v-if="filteredAlbum.length && !searchPhoto"
          >
            {{album.id}} {{album.title}}
          </span>
          <div
            class="photo"
            v-for="photo in searchPhoto ? filteredPhoto : album.photos"
            :key="photo.id"
          >
            <span class="photo-id">
              {{photo.id}}
            </span>
            <span class="photo-text">
              {{photo.title}}
            </span>
            <img
              :src="photo.thumbnailUrl"
              alt="photo"
              class="thumbnail"
            >
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
    },
    title: {
      tyoe: String,
      default: "TABLE"
    }
  },

  data() {
    return {
      albumsChunk: [],
      photos: [],
      searchData: "",
      searchPhoto: "",
      countAlbum: 1
    };
  },

  computed: {
    filteredAlbum() {
      return this.albums.filter(album => {
        return album.title
          .toLowerCase()
          .includes(this.searchData.toLowerCase());
      });
    },

    filteredPhoto() {
      this.albumsChunk = this.albums.slice(0, 1);
      return this.photos.filter(photo => {
        return photo.title
          .toLowerCase()
          .includes(this.searchPhoto.toLowerCase());
      });
    }
  },

  methods: {
    //     I know that DOM don't like to render a lot of elements, cuz it's bad for fps. But for this situation it's possible
    //     We can cut old elements from us albumsChunk and add new (when user is scrolling), to be on page just for example 50-60 elements.
    //     If you want I can do it))
    scroll() {
      const wrapperScroll = this.$refs.table.scrollTop;
      const wrapperHeight = this.$refs.table.clientHeight;
      const albumHeight = this.$refs.album.clientHeight;
      if (!this.searchPhoto) {
        if (wrapperScroll + wrapperHeight === albumHeight) {
          this.countAlbum++;
          this.albumsChunk = this.albums.slice(0, this.countAlbum);
        }
      }
    },

    sortAlbum() {
      this.albumsChunk.sort().reverse();
    },

    sortPhotos() {
      this.albumsChunk.forEach(item => {
        item.photos.sort().reverse();
      });
    }
  },

  watch: {
    albums(newVal) {
      if (newVal) {
        this.albumsChunk = this.albums.slice(0, this.countAlbum);
        this.albums.forEach(item => {
          this.photos.push(...item.photos);
        });
      }
    }
  }
};
</script>

<style scoped>
.table-title {
  text-align: center;
}

.table-wrapper {
  margin: 5px;
  height: 500px;
  overflow: auto;
  background-color: #d1e6da;
  border: 2px solid #ad60da;
}

.album {
  position: relative;
  padding: 2px 10px;
}

.album-title {
  display: block;
  position: sticky;
  top: -1px;
  margin-top: 5px;
  padding: 20px;
  text-align: center;
  box-shadow: 0px 3px 5px rgba(71, 67, 67, 0.5);
  background: rgb(175, 217, 230);
}

.photo {
  display: flex;
  align-items: center;
  justify-content: space-around;
  padding: 5px 15px;
  background: rgb(255, 255, 255);
  border: 1px solid rgb(186, 70, 209);
}

.search-emtpy {
  display: block;
  margin-top: 20%;
  text-align: center;
}

.filters {
  display: flex;
  justify-content: space-around;
  align-items: center;
}

.filter-btn {
  padding: 10px;
  border-radius: 8px;
  cursor: pointer;
  outline: none;
  background-color: lightblue;
  border: 2px solid grey;
}

.filter-btn:hover {
  background-color: #c5f1ff;
  transition: background-color 0.3s ease-in-out;
}

.input {
  padding: 10px;
  border-radius: 8px;
  outline: none;
  border: 2px solid grey;
}

.photo-id {
  flex-grow: 1;
}

.photo-text {
  flex-grow: 2;
}

.thumbnail {
  width: 50px;
  height: 50px;
}
</style>
