<template>
  <div :class="['cell', color]" @click="cellClick"></div>
</template>

<script>
export default {
  props: {
    color: {
      type: String,
      required: true,
    },
  },
  methods: {
    cellClick() {
      this.$emit("cell-click");
    },
  },
};
</script>

<style scoped>
.cell {
  width: 30px;
  height: 30px;
}

.yellow {
  background-color: yellow;
}

.green {
  background-color: green;
}

.red {
  background-color: red;
}

.blue {
  background-color: blue;
}
</style>
