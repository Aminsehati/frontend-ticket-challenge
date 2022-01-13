<template>
  <div class="home">
    <div class="wrapper">
      <Spinner v-if="filters.loading" />
      <div class="content" v-else>
        <div class="item" v-for="(item, indexRow) in items" :key="indexRow">
          <div
            v-for="(seatItem, indexCloumn) in item"
            :key="indexCloumn"
            class="mb-20 cursor-pointer"
            @click="ReservationItem(indexCloumn, indexRow)"
          >
            <img
              :src="icons.seatIcon"
              width="40"
              height="40"
              :class="{ active: seatItem === 1 }"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Seat from "../components/Seat";
import Spinner from "../components/Spinner";
import "./style.css";
export default {
  components: {
    Seat,
    Spinner,
  },
  name: "Home",
  data() {
    return {
      id: null,
      items: [],
      filters: {
        loading: false,
      },
      icons: {
        seatIcon: require("../assets/icons/seat.svg"),
      },
    };
  },
  mounted() {
    this.init();
  },
  methods: {
    async getRandomIdMap() {
      try {
        const httpResponse = await axios.get(
          "https://ticket-challange.herokuapp.com/map"
        );
        const data = httpResponse?.data?.data?.map_ids;
        const rndInt = Math.floor(Math.random() * 6) + 1;
        this.id = data[rndInt];
        console.log(this.id);
      } catch (error) {
        console.log(error);
      }
    },
    async getSeatItems() {
      this.filters.loading = true;
      try {
        if (this.id) {
          const config = {
            headers: {
              "Access-Control-Allow-Origin": "*",
              "Access-Control-Allow-Methods":
                "GET,PUT,POST,DELETE,PATCH,OPTIONS",
            },
          };
          const httpResponse = await axios.get(
            `https://ticket-challange.herokuapp.com/map/${this.id}`,
            config
          );
          const data = httpResponse?.data?.data;
          this.items = data.slice(0, 10);
          this.filters.loading = false;
        }
      } catch (error) {
        console.log(error);
      }
    },
    async init() {
      await this.getRandomIdMap();
      await this.getSeatItems();
    },
    ReservationItem(indexRow, indexCloumn) {
      this.buyTikcet(indexRow, indexCloumn);
    },
    async buyTikcet(y, x) {
      try {
        var formData = new FormData();
        formData.append("x", x);
        formData.append("y", y);
        const httpResponse = await axios.post(
          `
        https://ticket-challange.herokuapp.com/map/${this.id}/ticket`,
          formData
        );
        this.items[x][y] = 1;
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>