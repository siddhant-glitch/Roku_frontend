<template>
  <section class="container">
    <video id="background-video" autoplay loop muted poster="https://assets.codepen.io/6093409/river.jpg">
      <source src="@/assets/images/bgVideo.mp4" type="video/mp4">
    </video>
    <div class="jumbotron">
        <h1>Welcome to  ROKUFlashback!</h1>
        <p class="lead">
           Please log in the fields below to get to the users screen...
        </p>
    </div>

  <div class="inputBox">

    <section class="log-in">
      <label class="sr-only" for="inlineFormInputName">Name</label>
      <input v-model="username" type="text" class="form-control" id="inlineFormInputName" placeholder="username" required>

      <label class="sr-only" for="inlineFormPassword">Name</label>
      <input v-model="password" type="password" class="form-control" id="inlineFormPassword" placeholder="password" required>

      <button
        type="submit" 
        class="btn btn-primary login-submit"
        @click="login"
      >Go!
      </button>

      <button
        v-if="signup"
        type="submit" 
        class="btn btn-primary login-submit signup"
        @click="trySignUp"
      >Sign up!
    </button>

    </section>

  <div>
    <h2 class="login-flash" v-if="signup">{{ flash }}</h2>
    <h2 class="login-flash" v-if="errors">{{ flash }}</h2> 
  </div>

  </div>

  </section>
</template>

<script>
// @ is an alias to /src
export default {
  name: 'TheLogin',

  data() {
    return {
      username: '',
      password: '',
      url: 'users/getone',
      signup: false,
      errors: false,
      flash: ''
    }
  },

  methods: {

    tryLogin() {
      this.url = '/users/getone';
      this.login();
    },

    trySignUp() {
      this.url = '/users/signup';
      this.login();
    },

    goToUsers(time, vm) {
      setTimeout(function() {
        vm.$emit('setauth', true);
        vm.$router.push({ name: 'UserSelect' });
      }, time);
    },

    login() {
      //hit the back end UMS with a user name and password
      
      let formData = { username: this.username, password: this.password };

     let formData2 = new FormData();

      formData2.append('username', this.username);
      formData2.append('password', this.password);

      fetch('/movies', {
        method:'POST',
        //headers: { "Content-type" : "application/json"},
        //mode: 'no-cors',
        body: (formData2)
      })
    

      let url = this.url;  

      fetch(url, {
        method: 'POST',
        headers: {
          "Content-type" : "application/json"
        },
        body : JSON.stringify(formData)
      })
      
      .then(res => res.json())
      .then(data => {
        console.log(data);

        switch (data.action) {
          case 'add':   // user doesn't exist
            this.signup = true;
            this.username = '';
            this.password = '';
            this.flash = `Hmmm... your username doesn't seem to exist. Do you want to sign up? Or you can try again.`;
            break;
          
          case 'added':
            this.flash = 'Added you to the Roku Flashback Enjoy!         ...redirecting...';
            this.goToUsers(2500, this);
            break;

          case 'retry': //wrong password
            document.querySelector(`input[type=${data.field}]`).classList.add('error');
            this.errors = true;
            this.flash = `Your login info is not correct. Please retry.`;
            break;

          default: //login valid, send the user in to the userSelect page
            this.goToUsers(0, this);
        }
      })
      .catch(err => console.error(err));    
    }
    
  }
}
</script>

<style lang="scss">
  @import "@/assets/sass/login.scss";
</style>

