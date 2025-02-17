<template>
  <div>
    <Tweets />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Tweets from "./components/Tweets.vue";

export default defineComponent({
  components: { Tweets },
});
</script>
