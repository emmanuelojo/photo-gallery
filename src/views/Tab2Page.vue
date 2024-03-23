<template>
    <ion-page>
        <ion-header>
            <ion-toolbar>
                <ion-title>Photo Gallery</ion-title>
            </ion-toolbar>
        </ion-header>
        <ion-content :fullscreen="true">
            <ion-grid>
                <ion-row>
                    <ion-col sie="6" v-for="photo in photos" :key="photo.filepath">
                        <ion-img :src="photo.webviewPath" @click="showActionSheet(photo)" />
                    </ion-col>
                </ion-row>
            </ion-grid>

            <ion-fab vertical="bottom" horizontal="center" slot="fixed">
                <ion-fab-button @click="takePhoto()">
                    <ion-icon :icon="camera" />
                </ion-fab-button>
            </ion-fab>
        </ion-content>
    </ion-page>
</template>

<script setup lang="ts">
import { camera, trash, close } from "ionicons/icons";
import { actionSheetController, IonPage, IonHeader, IonToolbar, IonTitle, IonContent } from "@ionic/vue";
import { usePhotoGallery, UserPhoto } from "@/composables/usePhotoGallery";

const { photos, takePhoto, deletePhoto } = usePhotoGallery();

const showActionSheet = async (photo: UserPhoto) => {
    const actionSheet = await actionSheetController.create({
        header: "Photos",
        buttons: [
            {
                text: "Delete",
                role: "destructive",
                icon: trash,
                handler: () => {
                    deletePhoto(photo);
                },
            },
            {
                text: "Cancel",
                icon: close,
                role: "cancel",
                handler: () => {
                    // Nothing to do, action sheet is automatically closed
                },
            },
        ],
    });
    await actionSheet.present();
};
</script>
