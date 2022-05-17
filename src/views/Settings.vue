<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Settings</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Settings</ion-title>
        </ion-toolbar>
      </ion-header>
      <ion-button v-on:click="allowNotifications">Allow notifications</ion-button>
      <ion-button v-on:click="sendNotification">Send notification</ion-button>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { LocalNotifications } from '@capacitor/local-notifications';
import { IonButton, IonContent, IonHeader, IonPage, IonTitle, IonToolbar } from '@ionic/vue';

export default defineComponent({
  name: 'SettingsPage',
  components: { IonButton, IonContent, IonHeader, IonPage, IonTitle, IonToolbar },
  methods: {
    async allowNotifications() {
      await LocalNotifications.requestPermissions();
    },
    async sendNotification() {
      await LocalNotifications.schedule({
        notifications: [{
          title: 'Hello World!',
          body: 'This is a test',
          id: 1,
          schedule: {
            at: new Date()
          }
        }]
      })
    }
  }
});
</script>
