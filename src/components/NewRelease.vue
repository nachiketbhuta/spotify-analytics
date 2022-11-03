<template>
  <!-- Hello -->
  <!-- <div v-for="(img, index) in release.images" :key="index">
    <img :src="img.url"/>
  </div> -->
  <!-- <img :src="release" /> -->
  <div v-for="(release, index) in displayReleaseList" :key="index">
    Name: {{ release.name }} Release Date: {{ release.releaseDate }} Total
    Tracks: {{ release.totalTracks }}
    <img :src="release.imageUrl" />
  </div>
</template>

<script lang="ts">
export default {
  props: {
    releases: {
      type: Array,
      required: true,
    },
  },
  computed: {
    displayReleaseList() {
      return this.releases?.map((release: any) => {
        return {
          name: release["name"],
          releaseDate: release["release_date"],
          totalTracks: release["total_tracks"],
          imageUrl: release["images"]?.find(
            (img: any) => img?.width === 300 && img.height === 300
          )?.url,
        };
      });
    },
  },
};
</script>
