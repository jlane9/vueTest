<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Geolocation</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Geolocation</ion-title>
        </ion-toolbar>
      </ion-header>
      <ion-button v-on:click="requestPermissions">Request Permission</ion-button>
      <div v-if="hasPermission">Permission - Granted</div>
      <div v-if="!hasPermission">Permission - Not Granted</div>
      <ion-button v-on:click="getCurrentPosition">Get Location</ion-button>
      <div>{{coordinates}}</div>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { Geolocation } from '@capacitor/geolocation';
import { IonButton, IonContent, IonHeader, IonPage, IonTitle, IonToolbar } from '@ionic/vue';

export default defineComponent({
  name: 'GeolocationPage',
  data: () => {
    return {
      coordinates: {},
      hasPermission: false
    }
  },
  components: { IonButton, IonContent, IonHeader, IonPage, IonTitle, IonToolbar },
  methods: {
    async getCurrentPosition(){
      if (this.hasPermission) {
        const coordinates = await Geolocation.getCurrentPosition();
        console.log(coordinates);

        this.coordinates = coordinates;
      } else {
        alert('Please request permissions first');
      }
    },
    async requestPermissions() {
      const permissionStatus = await Geolocation.requestPermissions();
      console.log(permissionStatus);

      this.hasPermission = permissionStatus.location === 'granted';
    },
  }
});
</script>
