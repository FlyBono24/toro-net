<template>
  <div v-if="!this.$store.state.user.displayName">
    <div class="container">
      <form @submit.prevent="validateBeforeSubmit" id="login" action="/auth/local" method="post">
        <div class="form-group" :class="{'has-error': errors.has('email') }" >
          <label for="email" class="pull-left">Email</label>
          <input name="email" id="email" v-validate="'required|email'" data-vv-delay="500" type="text" data-vv-as="email address" placeholder="Email" class="form-control">
          <p class="text-danger" align="left" v-if="errors.has('email')">{{ errors.first('email') }}</p>
        </div>
        <div class="form-group" :class="{'has-error': errors.has('password') }" >
          <label for="password" class="pull-left">Password</label>
          <input name="password" id="password" v-validate="'required'" data-vv-delay="500" type="text" data-vv-as="password" placeholder="Password" class="form-control" type="password">
          <p class="text-danger" align="left" v-if="errors.has('password')">{{ errors.first('password') }}</p>
        </div>
        <button class="btn btn-primary" form="login" type="submit">Login</button>
      </form>
      <h4 class="mx-auto">OR<h4>
      <a href="/auth/github" class="btn btn-github">
        <i class="fa fa-github"></i> Login with Github
      </a>
    </div>
  </div>
  <div v-else>
    <div class="container">
      <h4>You are already logged in, {{this.$store.state.user.displayName}}!</h4>
    </div>
  </div>
</template>


<!-- added button colors to match toro net and Github
-->
<style scoped>
.btn-github {
  border: 0;
  background: #363636;
  color: white;
}
.btn-github:hover {
  background: #2B2B2B;
  color: white;
}
</style>

<script>
export default {
  name: 'Login',
  methods: {
    validateBeforeSubmit(e) {
      e.preventDefault()
      this.$validator.validateAll().then((result) => {
        if (result) {
          // eslint-disable-next-line
          document.querySelector('#login').submit()
          return
        } 
      })
    }
  },
  mounted() {
    this.$store.dispatch('getUser')
  }
}
</script>
