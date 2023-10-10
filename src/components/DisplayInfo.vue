<template>
  <main v-if="IP_Data">
    <section class="info">
      <div>
        <p>ip adress</p>
        <h2>{{ setIP }}</h2>
      </div>
      <div>
        <p>location</p>
        <h2>{{ setLocation }}</h2>
      </div>
      <div>
        <p>timezone</p>
        <h2>{{ setTimezone }}</h2>
      </div>
      <div>
        <p>isp</p>
        <h2>{{ setISP }}</h2>
      </div>
    </section>

    <section style="height:50vh; width:100vw"  class="map" >
      <l-map ref="map" v-model:zoom="zoom" :center="setLatLang || [0, 0]" :use-global-leaflet="false">
        <l-tile-layer
          url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
          layer-type="base"
          name="OpenStreetMap"
        ></l-tile-layer>
        <l-marker :lat-lng="setLatLang || [0, 0]" ></l-marker>
      </l-map>
    </section>
  </main>

  <main v-else>
    <h2>Loading...</h2>
  </main>
</template>

<script>
import "leaflet/dist/leaflet.css";
import { LMap, LTileLayer, LMarker } from "@vue-leaflet/vue-leaflet";

export default {
  name: "DisplayInfo",
  props: {
    value: { String },
  },
  components: {
    LMap,
    LTileLayer,
    LMarker,
  },

  data() {
    return {
      IP_Data: {},
      API_KEY: "at_Rto3a1CvNSmgGJeGVrj0H83doUyDh",
      zoom: 12,
    };
  },

  methods: {
    async fetchData() {
      try {
        const res = await fetch(`https://geo.ipify.org/api/v2/country,city?apiKey=${this.API_KEY}&ipAddress=${this.value}`);
        if(res.status === 200) {
          const json = await res.json();
          return this.IP_Data = json;
        }
        throw new Error(res.status);
      } catch (error) {
        return this.IP_Data = error;
      }
    },

  },

  computed: {
    setIP() {
      return this.IP_Data.ip && this.IP_Data.ip;
    },
    setLocation() {
      return this.IP_Data.location && `${this.IP_Data.location.city}, ${this.IP_Data.location.region}`;
    },
    setTimezone() {
      return this.IP_Data.location && `UTC ${this.IP_Data.location.timezone}`;
    },
    setISP() {
      return this.IP_Data.isp && this.IP_Data.isp;
    },
    setLatLang() {
      return this.IP_Data.location && [this.IP_Data.location.lat, this.IP_Data.location.lng];
    }
  },

  created() {
    this.fetchData();
  },

  updated() {
    console.log(this.setLatLang);
  }
}
</script>
