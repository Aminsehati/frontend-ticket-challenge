<template>
  <div
    v-for="(seat, indexCloumn) in seatItem"
    :key="indexCloumn"
    class="mb-20 cursor-pointer"
    @click="ReservationItem(indexCloumn)"
  >
    <img
      :src="icons.seatIcon"
      width="40"
      height="40"
      :class="{ active: seat === 1 }"
    />
  </div>
</template>
<script>
export default {
  props: {
    seatItem: {
      type: Array,
      default: () => [],
    }
  },
  data() {
    return {
      icons: {
        seatIcon: require("../../assets/icons/seat.svg"),
      },
    };
  },
  methods: {
    ReservationItem(indexCloumn) {
      this.$emit("ReservationItem",indexCloumn);
    },
  },
};
</script>