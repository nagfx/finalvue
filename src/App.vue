<template>
  <div class="container mx-auto px-4">
    <div class="flex items-center justify-between">
      <p class="text-xl md:text-5xl font-bold text-center py-3 my-4">
        Digimon library Vue App
      </p>
      <input
        type="text"
        class="px-4 py-2 bg-gray-200 rounded"
        placeholder="Search Digimons"
      />
    </div>
    <div class="grid grid-cols-1 md:grid-cols-3 gap-4 my-3">
      <div
        class="card scrollingDescription scrollbar-hide"
        v-for="(digimon, index) in digimons"
        :key="index"
      >
        <img :src="digimon.images[0].href" alt="" class="display-image" />
        <p class="text-2xl font-bold text-center my-2 capitalize">
          {{ digimon.name }}
        </p>
        <p class="text-md font-bold text-left">
          ID:<span class="font-normal"> #{{ digimon.id }}</span>
        </p>
        <p
          class="text-md font-bold text-left"
          v-if="digimon.types && digimon.types.length > 0"
        >
          Type:
          <span class="font-normal">{{ digimon.types[0].type }}</span>
        </p>
        <p
          class="text-md font-bold text-left"
          v-if="
            digimon.descriptions &&
            digimon.descriptions.length > 1 &&
            digimon.descriptions[1].description &&
            digimon.descriptions[1].description.length > 0
          "
        >
          Description:
          <span class="font-normal">{{
            digimon.descriptions[1].description
          }}</span>
        </p>
      </div>
    </div>
    <div class="my-3 text-center">
      <!-- The previous button only shows when the offset is not 0. -->
      <button
        v-if="page !== 0"
        @click="showPreviousDigimons"
        class="bg-amber-400 text-white py-2 mt-5 px-4 mx-5 rounded-full"
      >
        Previous
      </button>

      <button
        @click="showNextDigimons"
        class="bg-blue-400 text-white py-2 mt-5 px-4 rounded-full"
      >
        Next
      </button>
    </div>
  </div>
  <footer class="bg-indigo-100">
    <div class="container mx-auto text-left mt-10 py-3">
      <p class="text-xl">Copyright Naman</p>
    </div>
  </footer>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      digimons: [],
      page: 0,
    };
  },
  methods: {
    async fetchData() {
      try {
        let { data } = await axios.get(
          `https://digimon-api.com/api/v1/digimon?page=${this.page}`
        );
        this.digimons.length = 0;
        data.content.map(async (obj) => {
          let { data } = await axios.get(obj.href);
          // reset the array to display specific digimons according to assigned offset and limit
          this.digimons.push(data);
        });
        // sort function to ensure the Digimons appear sorted
        this.digimons.sort((a, b) => {
          return a.id - b.id;
        });
      } catch (err) {
        alert(err);
      }
    },
    async showNextDigimons() {
      this.page++;
      this.fetchData();
    },
    showPreviousDigimons() {
      this.page--;
      this.fetchData();
    },
  },
  mounted() {
    this.fetchData();
  },
};
</script>

<style>
body {
  background-color: azure;
}
img.display-image {
  width: 180px;
  border-radius: 50%;
  padding: 20px;
  display: block;
  margin: 0 auto;
}

div.card {
  border-radius: 20px;
  background: white;
  height: 480px;
  box-shadow: 1px 2px 15px 4px rgb(164, 164, 164);
  padding: 20px;
}
.ability {
  border-radius: 35px;
  padding: 6px;
  display: inline-block;
}
</style>
