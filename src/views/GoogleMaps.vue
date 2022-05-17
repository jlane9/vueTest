<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Google Maps</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <div class="map-container">
        <capacitor-google-map id="map" ref="map" />
      </div>
      <ion-fab class="reset-location" vertical="bottom" horizontal="end" slot="fixed">
        <ion-fab-button v-on:click="resetLocation">
          <ion-icon :icon="navigateOutline" />
        </ion-fab-button>
      </ion-fab>
    </ion-content>
  </ion-page>
</template>

<style>
  .map-container {
    height: 100%;
  }

  .map-container capacitor-google-map {
    display: inline-block;
    width: 100%;
    height: 100%;
  }

  .map-container .gm-style-cc,
  .map-container .gm-fullscreen-control,
  .map-container .gm-bundled-control,
  .map-container .gm-style-mtc {
    display: none;
  }

  .reset-location {
    margin-right: 16px;
    margin-bottom: 16px;
  }
</style>

<script lang="ts">
import { navigateOutline } from 'ionicons/icons';
import { defineComponent } from 'vue';
import { GoogleMap } from '@capacitor/google-maps';
import { IonContent, IonFab, IonFabButton, IonHeader, IonIcon, IonPage, IonTitle, IonToolbar } from '@ionic/vue';
import { Geolocation, Coordinates } from '@ionic-native/geolocation';

const DEFAULT_COORDINATES: Coordinates = {
  latitude: 39.1109,
  longitude: -94.5841,
  accuracy: 0,
  altitude: 900,
  altitudeAccuracy: 0,
  heading: 0,
  speed: 0,
}

export default defineComponent({
  name: 'GoogleMapsPage',
  components: { IonContent, IonFab, IonFabButton, IonHeader, IonIcon, IonPage, IonToolbar, IonTitle },
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

      if (!this.$refs['map']) return;

      const position: Coordinates = await Geolocation.getCurrentPosition()
          .then((position) => position.coords)
          .catch(
              (): Coordinates => (DEFAULT_COORDINATES)
          );

      this.googleMap = await GoogleMap.create({
        id: 'my-map', // Unique identifier for this map instance
        element: this.$refs['map'] as HTMLElement, // reference to the capacitor-google-map element
        apiKey: process.env.VUE_APP_GOOGLE_MAPS_API_KEY, // Your Google Maps API Key
        config: {
          center: {
            lat: position.latitude,
            lng: position.longitude,
          },
          zoom: 15,
        },
      });
    },
    async resetLocation() {
      const position: Coordinates = await Geolocation.getCurrentPosition()
          .then((position) => position.coords)
          .catch(
              (): Coordinates => (DEFAULT_COORDINATES)
          );

      this.googleMap?.setCamera({
        coordinate: {
          lat: position.latitude,
          lng: position.longitude,
        },
        zoom: 15,
        animate: true,
      });
    }
  },
  setup() {
    return {
      navigateOutline
    }
  }
});
</script>
