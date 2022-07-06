<script setup>
import HelloWorld from './components/HelloWorld.vue';
</script>

<template>
  <header>
    <img
      alt="Vue logo"
      class="logo"
      src="./assets/logo.svg"
      width="125"
      height="125"
    />

    <div class="wrapper">
      <HelloWorld msg="Click on buttons" />
    </div>
  </header>

  <main>
    <div>Without Proxy: <button @click="withoutProxy">Test</button></div>
    <div>With Proxy: <button @click="withProxy">Test</button></div>
    <textarea id="response" v-model="responseText"></textarea>
  </main>
</template>
<script>
export default {
  data() {
    return { responseText: '' };
  },
  methods: {
    async withoutProxy() {
      this.responseText += 'Without Proxy\n';
      this.appentResponseText(
        await this.sendData('https://www.freeforexapi.com/api/live')
      );
    },
    async withProxy() {
      this.responseText += 'With Proxy\n';
      this.appentResponseText(await this.sendData('/api-proxy/api/live'));
    },
    appentResponseText(text) {
      this.responseText +=
        (text instanceof Error ? text : JSON.stringify(text)) +
        '\n------------\n';
    },
    sendData(url) {
      return fetch(url)
        .then(async (response) => {
          const data = await response.json();

          // check for error response
          if (!response.ok) {
            // get error message from body or default to response statusText
            const error = (data && data.message) || response.statusText;
            return Promise.reject(error);
          }

          return data;
        })
        .catch((error) => {
          console.error('There was an error!', error);
          return error;
        });
    },
  },
};
</script>
<style>
@import './assets/base.css';

#app {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;

  font-weight: normal;
}

header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

a,
.green {
  text-decoration: none;
  color: hsla(160, 100%, 37%, 1);
  transition: 0.4s;
}

textarea#response {
  width: 100%;
  height: 250px;
}

@media (hover: hover) {
  a:hover {
    background-color: hsla(160, 100%, 37%, 0.2);
  }
}

@media (min-width: 1024px) {
  body {
    display: flex;
    place-items: center;
  }

  #app {
    display: grid;
    grid-template-columns: 1fr 1fr;
    padding: 0 2rem;
  }

  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  .logo {
    margin: 0 2rem 0 0;
  }
}
</style>
