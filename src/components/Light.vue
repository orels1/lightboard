<template lang="pug">
  .light(@click="toggle")
    .state
      icon(:icon="lightIcon" :class="['bulb', light.on && 'on']")
    .content
      .title
        .action {{actionLabel}}
        .text {{light.name}}
      .scene(v-if="scene")
        .color(
          v-for="color in scene.colors"
          :style="{ background: color, width: parseInt(140/scene.colors.length, 10) + 'px' }"
        )
</template>

<script>
import Vue from 'vue';
import Component from 'vue-class-component';
import FontAwesomeIcon from '@fortawesome/vue-fontawesome';
import faBulbOn from '@fortawesome/fontawesome-free-solid/faLightbulb';
import faBulbOff from '@fortawesome/fontawesome-free-regular/faLightbulb';
import find from 'lodash/find';

@Component({
  props: {
    light: {
      type: Object,
      default: () => ({
        id: 0,
        name: '',
        on: false,
        scene: null,
      }),
    },
    scenes: {
      type: Array,
      default: [],
    },
  },
  components: {
    icon: FontAwesomeIcon,
  },
})
export default class Light extends Vue {
  scene = null;

  get lightIcon() {
    return this.light.on ? faBulbOn : faBulbOff;
  }

  get actionLabel() {
    return this.light.on ? 'Turn off?' : 'Turn on?';
  }

  toggle() {
    this.$emit('click', this.light.id);
  }

  findScene(scenes, scene) {
    this.scene = find(scenes, { name: scene }) || null;
  }

  mounted() {
    this.findScene(this.scenes, this.light.scene);
  }

  updated() {
    this.findScene(this.scenes, this.light.scene);
  }
}
</script>

<style scoped>
.light {
  display: flex;
  text-align: center;
  width: 100%;
  height: 100%;
  background: #272C35;
  border-radius: 15px;
  padding: 15px 20px 15px 0;
  box-sizing: border-box;
  cursor: pointer;
  transition: background 150ms ease;
}

.light:hover {
  background: rgb(49, 55, 65);
}

.bulb {
  opacity: .3;
  color: rgb(189, 189, 189);
  transition: all 1s ease-in-out;
}

.on {
  color: #FFCC00;
  opacity: 1;
  transition-duration: 150ms;
}

.state {
  padding: 0 15px;
  width: 10px;
}

.content {
  text-align: left;
}

.title {
  height: 22px;
  width: 100%;
  overflow: hidden;
}

.text, .action {
  transition: transform 150ms ease-in-out;
  position: relative;
  transform: translateY(-22px);
}

.light:hover .text, .light:hover .action {
  transform: translateY(0);
}

.scene {
  margin: 5px 0 0 0;
  display: flex;
}

.color {
  height: 10px;
}
</style>
