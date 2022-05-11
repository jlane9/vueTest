<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Google Maps</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Google Maps</ion-title>
        </ion-toolbar>
      </ion-header>
      <div :style="{ marginTop: '32px', paddingLeft: '8px', paddingRight: '8px' }">
        <capacitor-google-map id="map" ref="map" :style="{ display: 'inline-block', width: '100%', height: '480px' }" />
      </div>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { GoogleMap } from '@capacitor/google-maps';
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent } from '@ionic/vue';

export default defineComponent({
  name: 'GoogleMapsPage',
  components: { IonHeader, IonToolbar, IonTitle, IonContent, IonPage },
  data() {
    return {
      googleMap: undefined as GoogleMap | undefined
    }
  },
  async ionViewWillLeave () {
    await this.removeMap();
  },
  async ionViewDidEnter () {
    await this.newMap();
  },
  methods: {
    async removeMap() {
      if (this.googleMap) {
        await this.googleMap.destroy();
      }
    },
    async newMap() {

      if (!this.$refs['map']) {
        return
      }

      this.googleMap = await GoogleMap.create({
        id: 'my-map', // Unique identifier for this map instance
        element: this.$refs['map'] as HTMLElement, // reference to the capacitor-google-map element
        apiKey: 'YOUR_API_KEY_HERE', // Your Google Maps API Key
        config: {
          center: {
            // The initial position to be rendered by the map
            lat: 33.6,
            lng: -117.9,
          },
          zoom: 8, // The initial zoom level to be rendered by the map
        },
      });
    }
  }
});
</script>
