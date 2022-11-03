<template>
  <new-release :releases="newReleaseList" />
</template>
<script lang="ts">
import NewRelease from "./NewRelease.vue";

export default {
  components: { NewRelease },
  data() {
    return {
      newReleaseList: [],
    };
  },
  async created() {
    const BODY = {
      grant_type: "client_credentials",
      client_id: import.meta.env.VITE_CLIENT_ID,
      client_secret: import.meta.env.VITE_CLIENT_SECRET,
    };
    fetch("https://accounts.spotify.com/api/token", {
      method: "POST",
      body:
        "grant_type=client_credentials&client_id=" +
        BODY.client_id +
        "&client_secret=" +
        BODY.client_secret,
      headers: {
        "Content-Type": "application/x-www-form-urlencoded",
      },
    })
      .then((res) => res.json())
      .then((resp) => {
        const token = resp.access_token;
        return fetch("https://api.spotify.com/v1/browse/new-releases", {
          headers: {
            Authorization: "Bearer " + token,
          },
        });
      })
      .then((res) => res.json())
      .then((resp) => {
        // console.log(resp.albums.items);
        this.newReleaseList = resp.albums.items;
      });
  },
};
</script>
