# X API Post Viewer

This project is a Vue.js application that fetches and displays recent tweets from the X (formerly Twitter) API. It uses Axios for API requests, Tailwind CSS and Vuetify for styling, and is deployed on a Hostinger subdomain via SFTP with FileZilla.

## Features

- Fetches recent tweets using Axios
- Displays posts with a Vue.js UI
- Deployed on Hostinger subdomain (x-api-vue.alenhuric.com)
- Built using Vite for fast performance
- [Vuetify](https://vuetifyjs.com/en/), a Vue UI library

## Laravel Backend

The backend is built using Laravel and fetches tweets from the X API using Guzzle HTTP Client. The API responses are cached for 15 minutes using Laravel's caching mechanism to reduce redundant API calls and improve performance.

## Vuetify Integration

This project uses Vuetify, a Vue UI library, to enhance the user interface with pre-built components and Material Design styling.
