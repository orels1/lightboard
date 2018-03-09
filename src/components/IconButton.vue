<template lang="pug">
  button.iconButton(v-if="icon" @click="click")
    icon.icon(:icon="icon")
</template>

<script>
import Vue from 'vue';
import Component from 'vue-class-component';
import FontAwesomeIcon from '@fortawesome/vue-fontawesome';

@Component({
  props: {
    icon: Object,
  },
  components: {
    icon: FontAwesomeIcon,
  },
})
export default class IconButton extends Vue {
  click() {
    this.$emit('click');
  }
}
</script>

<style scoped>
.iconButton {
  background: #282C34;
  border-radius: 10px;
  padding: 10px;
  border: none;
  transition: background 150ms ease;
  cursor: pointer;
  outline: none !important;
  width: 100%;
  box-sizing: border-box;
  margin: 0 5px 0 0;
}

.iconButton:hover {
  background: rgb(49, 55, 65);
}

.icon {
  font-size: 2rem;
  color: rgba(255,255,255,.5);
}
</style>
