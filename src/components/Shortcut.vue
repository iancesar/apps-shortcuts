<template>
  <a @click="navigate(to)" style="cursor:pointer">
    <q-card :style="`background:${color}`">
      <q-card-section>
        <q-avatar square>
          <img :src="getImgUrl()" />
        </q-avatar>
        <span class="text-h6 text-weight-light" style="margin-left:10px">{{name}}</span>
      </q-card-section>

      <!--Edit or remove -->
      <slot></slot>

      
    </q-card>
  </a>
</template>

<script>
import open from "open";

export default {
  name: "Shortcut",
  props: ["name", "icon", "color", "to"],
  data() {
    return {};
  },
  methods: {
    navigate(to) {
      open(to);
      // window.open(to, "_blank", "nodeIntegration=no");
    },
    getImgUrl() {
      return require("../assets/" + this.icon);
    }
  }
};
</script>
