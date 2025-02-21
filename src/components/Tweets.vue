<template>
  <v-app>
    <v-container>
      <p class="heading text-center mt-4 mb-2 font-weight-bold primary--text">
        Recent tweets from the alen_developer X account
      </p>

      <p class="text-center description mb-6">
        This page showcases a Vue.js frontend that interfaces with a Laravel
        backend to fetch recent tweets from the X API.
      </p>

      <v-card class="mx-auto" max-width="600" elevation="2">
        <v-card-text>
          <v-alert v-if="loading" type="info" class="mb-4">
            Loading tweets...
          </v-alert>

          <v-alert v-else-if="error" type="error" class="mb-4">
            {{ error }}
          </v-alert>

          <template v-else>
            <v-list v-if="tweets.length > 0">
              <v-list-item
                v-for="tweet in tweets"
                :key="tweet.id"
                class="tweet-item"
                @click="openTweet(tweet.id)"
              >
                <div class="user-info">
                  <img
                    src="https://abs.twimg.com/sticky/default_profile_images/default_profile_400x400.png"
                    alt="Profile Image"
                    class="profile-img"
                  />
                  <div class="user-details">
                    <span class="user-name">Alen Huric</span>
                    <span class="user-handle">@alen_developer</span>
                  </div>
                </div>

                <p class="tweet-text mt-2">
                  {{ tweet.text }}
                </p>

                <div class="tweet-timestamp">
                  <small>{{ formatTimestamp(tweet.created_at) }}</small>
                </div>
              </v-list-item>
            </v-list>

            <p v-else class="text-center">No tweets available...</p>
          </template>
        </v-card-text>
      </v-card>

      <div class="footer text-center mt-8">
        <p>
          Check out the source code on
          <a
            href="https://github.com/alenhuric/x-api-vue"
            target="_blank"
            class="primary--text"
          >
            GitHub.
            <v-icon small>mdi-github</v-icon>
          </a>
        </p>
      </div>

      <v-tooltip>
        <template #activator="{ props }">
          <v-btn
            class="laravel-btn"
            fab
            dark
            :color="laravelRed"
            @click="goToLaravel"
            :width="$vuetify.display.smAndDown ? 50 : 60"
            :height="$vuetify.display.smAndDown ? 50 : 60"
            v-bind="props"
          >
            <v-icon :size="$vuetify.display.smAndDown ? 28 : 36"
              >mdi-laravel</v-icon
            >
          </v-btn>
        </template>
        <span>Go to the Laravel API</span>
      </v-tooltip>
    </v-container>
  </v-app>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from "vue";
import axios from "axios";

export default defineComponent({
  setup() {
    const tweets = ref<{ id: string; text: string; created_at: string }[]>([]);
    const loading = ref(true);
    const error = ref<string | null>(null);
    const laravelRed = ref("#FF2D20");

    const fetchTweets = async () => {
      try {
        const response = await axios.get("https://api.alenhuric.com");
        tweets.value = response.data;
      } catch (err: any) {
        error.value = err.response?.data?.error || "Failed to fetch tweets";
      } finally {
        loading.value = false;
      }
    };

    const openTweet = (tweetId: string) => {
      window.open(
        `https://twitter.com/alen_developer/status/${tweetId}`,
        "_blank"
      );
    };

    const formatTimestamp = (date: string) => {
      const options: Intl.DateTimeFormatOptions = {
        hour: "numeric",
        minute: "numeric",
        hour12: true,
        month: "short",
        day: "numeric",
        year: "numeric",
      };
      return new Date(date).toLocaleString("en-US", options);
    };

    const goToLaravel = () => {
      window.location.href = "https://api.alenhuric.com";
    };

    onMounted(fetchTweets);

    return {
      tweets,
      loading,
      error,
      formatTimestamp,
      goToLaravel,
      laravelRed,
      openTweet,
    };
  },
});
</script>

<style scoped>
.heading {
  font-size: 26px;
  font-weight: bold;
  color: #1d9bf0;
}

.description {
  font-size: 14px;
  color: #657786;
  width: 80%;
  margin: 0 auto;
}

.tweet-item {
  cursor: pointer;
  background-color: #ffffff;
  border-radius: 12px;
  padding: 15px;
  margin-bottom: 12px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: background-color 0.2s ease-in-out;
}

.v-list-item {
  border-radius: 12px !important;
  overflow: hidden;
}

.tweet-item:hover {
  background-color: #ebeef0;
}

.user-info {
  display: flex;
  align-items: center;
  gap: 10px;
}

.profile-img {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  border: 2px solid #e1e8ed;
}

.user-details {
  display: flex;
  flex-direction: column;
}

.user-name {
  font-weight: bold;
  color: #14171a;
}

.user-handle {
  font-size: 14px;
  color: #657786;
}

.tweet-text {
  font-size: 16px;
  line-height: 1.5;
  color: #14171a;
  margin-top: 8px;
}

.tweet-timestamp {
  font-size: 12px;
  color: #657786;
  text-align: left;
  margin-top: 8px;
}

.footer {
  font-size: 14px;
  color: #657786;
}

.footer a {
  color: #1d9bf0;
  text-decoration: none;
}

.footer a:hover {
  text-decoration: underline;
}

.laravel-btn {
  position: fixed;
  bottom: 20px;
  right: 20px;
  z-index: 9999;
  border-radius: 50%;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
  transition: transform 0.2s ease-in-out, box-shadow 0.2s ease-in-out;
}

.laravel-btn:hover {
  transform: scale(1.1);
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
}
</style>
