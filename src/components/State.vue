<template>
  <p id="para">{{ fonction }}</p>
</template>

<script>
export default {
  name: "State",
  props: {
    fonction: String,
    onoff: Boolean
  },
  beforeUpdate() {
    let para = document.getElementById("para");
    if (this.onoff == false) {
      para.className = "off";
    } else {
      para.className = "on";
    }
  }
};
</script>

<style></style>
