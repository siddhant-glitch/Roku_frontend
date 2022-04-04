<template>
  <section class="container">
    <div class="jumbotron">
        <h1>Welcome to Flashblack!</h1>
        <p class="lead">
        Before revisiting your favourite movies, tv shows or music from yesteryear, please log in with a valid username and password.
        </p>
    </div>

    <section class="log-in">
      <label class="sr-only" for="inlineFormInputName">Name</label>
      <input v-model="username" type="text" class="form-control" id="inlineFormInputName" placeholder="username" required>

      <label class="sr-only" for="inlineFormPassword">Name</label>
      <input v-model="password" type="password" class="form-control" id="inlineFormPassword" placeholder="password" required>
    </section>

    <button
        type="submit" 
        class="btn btn-primary login-submit"
        @click="login"
      >Go!
    </button>
  </section>
</template>

<script>
// @ is an alias to /src
export default {
  name: 'TheLogin',

  data() {
    return {
      username: '',
      password: ''
    }
  },

  methods: {
    login() {
      //hit the back end UMS with a user name and password
    
      debugger;

      this.$router.push({ name: 'UserSelect'});
      return;

      /* 
      let formData = new FormData();

      formData.append("username", this.username);
      formData.append("passoword", this.password); 

      let url = `someloginurl`;  //fix it later

      fetch(url, {
        method: 'POST',
        body : formData
      })
      .then(res => res.json())
      .then(data => {
        console.log(data);
        //either pass or fail our login attempt
      })
      .catch((error) => console.error(error));    */
    }
    
  }
}
</script>

<style lang="scss">
  @import "@/assets/sass/login.scss";
</style>

