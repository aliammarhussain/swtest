<template>
  <div id="installContainer">
    <img alt="Vue logo" src="./assets/logo.png">

    <button id="butInstall" @click.prevent="install">Install App</button>
  </div>
</template>

<script>
const divInstall = document.getElementById("installContainer");

export default {
  name: 'App',
  mounted() {
    this.init()
  },
  methods: {
    init() {
      window.addEventListener('beforeinstallprompt', (event) => {
        // Prevent the mini-infobar from appearing on mobile.
        event.preventDefault();
        console.log('👍', 'beforeinstallprompt', event);
        // Stash the event so it can be triggered later.
        window.deferredPrompt = event;
        // Remove the 'hidden' class from the install button container.
        divInstall.classList.toggle('hidden', false);
      });

      window.addEventListener('appinstalled', (event) => {
        console.log('👍', 'appinstalled', event);
        // Clear the deferredPrompt so it can be garbage collected
        window.deferredPrompt = null;
      });
    },
    async install() {
      console.log('👍', 'butInstall-clicked');
      const promptEvent = window.deferredPrompt;
      if (!promptEvent) {
        // The deferred prompt isn't available.
        return;
      }
      // Show the install prompt.
      promptEvent.prompt();
      // Log the result
      const result = await promptEvent.userChoice;
      console.log('👍', 'userChoice', result);
      // Reset the deferred prompt variable, since
      // prompt() can only be called once.
      window.deferredPrompt = null;
      // Hide the install button.
      divInstall.classList.toggle('hidden', true);
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
