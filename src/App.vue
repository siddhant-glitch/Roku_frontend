<template>
  <section class="app-wrapper">
    <header class="col-sm-12 roku-header">
      <img src="@/assets/images/roku.svg" alt="Roku logo" width="150"><span>FlashBack</span>
      <nav class="float-right">

      <ul v-if="authenticated">            
        <li @click="switchUser"><i class="fas fa-user-circle"></i></li>
        <li><i class="fas fa-cog"></i></li>
        <li @click="logOut"><i class="fas fa-power-off"></i></li>					
      </ul> 

      </nav>
    </header>

    <router-view @setauth="setAuthenticated"></router-view>
  </section>
</template>

<script>
export default {
  name: "TheRokuFlashbackApp",

  created() {
    if (localStorage.getItem('user')) {
      this.$router.push( {name: 'UserHome', params: JSON.parse(localStorage.getItem('user')) });
    }
  },

  data() {
    return {
      authenticated: false
    }
  },

  methods: {
    setAuthenticated(status) {
      this.authenticated = status;
    },

    logOut() {
      if (localStorage.getItem('user')) {

        localStorage.removeItem('user');
      }

      // not authenticated anymore
      this.setAuthenticated(false);
       this.$router.push({name:'Login'});
    },

    switchUser() {
      this.$router.push({ name: 'UserSelect'});
    }
   

  }

}
</script>

<style lang="scss">
@import "@/assets/sass/main.scss";
</style> 
