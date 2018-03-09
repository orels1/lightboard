<template lang="pug">
  .dashboard
    .sidepanel
      h2 Quick Actions
      .shadow
      .header All lights switch
      .row
        icon-button(:icon="lightsOnIcon" @click="toggleAllLights(true)")
        icon-button(:icon="lightsOffIcon" @click="toggleAllLights(false)")
    .main
      h2 Lights
      .lights(v-if="loaded")
        light(
          :light="light"
          :scenes="scenes"
          @click="toggleLight"
          v-for="light in lights"
          :key="light.id"
        )
</template>

<script>
import Vue from 'vue';
import Component from 'vue-class-component';
import findIndex from 'lodash/findIndex';
import faBulbOn from '@fortawesome/fontawesome-free-solid/faLightbulb';
import faBulbOff from '@fortawesome/fontawesome-free-regular/faLightbulb';
import Light from '@/components/Light';
import IconButton from '@/components/IconButton';


const API_ROOT = 'https://19a1d43a-365b-4bbf-b2bc-b73a3c26d521.mock.pstmn.io';

@Component({
  components: {
    light: Light,
    'icon-button': IconButton,
  },
})
export default class Dashboard extends Vue {
  lights = [];
  scenes = [];
  loaded = false;
  lightsOnIcon = faBulbOn;
  lightsOffIcon = faBulbOff;

  toggleLight(id) {
    const lightIndex = findIndex(this.lights, ['id', id]);
    this.lights[lightIndex].on = !this.lights[lightIndex].on;
  }

  toggleAllLights(state) {
    this.lights.forEach((light, i) => { this.lights[i].on = state; });
  }

  async created() {
    const resp = await fetch(`${API_ROOT}/lights`);
    const json = await resp.json();
    const lights = json.results;
    this.lights = lights;

    const scenesResp = await fetch(`${API_ROOT}/scenes`);
    const scenesJson = await scenesResp.json();
    const scenes = scenesJson.results;
    this.scenes = scenes;

    this.loaded = true;
  }
}
</script>

<style scoped>
.dashboard {
  display: grid;
  grid-template: minmax(400px, 1fr) / 220px 1fr
}

.main {
  text-align: left;
  margin: 0 0 0 10px;
}

.sidepanel {
  text-align: left;
  padding: 0 20px 20px 20px;
  margin: 0 10px 0 0;
  position: relative;
  z-index: 1;
  overflow: hidden;
}

.shadow {
  position: absolute;
  height: 90%;
  width: 10px;
  top: 30px;
  right: -10px;
  box-shadow: -4px 0 26px rgba(0,0,0,0.3);
  z-index: 2;
}

.lights {
  display: grid;
  grid-template-columns: repeat(3, 200px);
  grid-gap: 15px;
  grid-auto-rows: auto;
}

.row {
  display: grid;
  grid-gap: 10px;
  grid-template: 1fr / repeat(2, 1fr);
}

.header {
  margin: 0 0 10px 0;
}
</style>
