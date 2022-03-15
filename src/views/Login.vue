<template>
<div class="container">
  <div class="form-container">
    <h1 class="text-center">Login</h1>
    <form>
      <div class="form-group pb-3">
        <label for="username">Username:</label>
        <input type="text" id="username" placeholder="Enter email" v-model="username">
      </div>
      <div class="form-group pb-3">
        <label for="password">Password:</label>
        <input type="password" id="password" placeholder="Password" v-model="password">
      </div>
      <div class="text-center pt-3">
        <button class="btn" @click.prevent="handleLogin">Login</button>
      </div>
    </form>
  </div>
</div>
</template>

<script>
export default {
  data() {
    return {
      username: '',
      password: ''
    }
  },
  methods: {
    handleLogin() {
      const formData = new FormData();

      formData.append('username', this.username)
      formData.append('password', this.password)
      
      const check_token =
                fetch('http://localhost:8000/login/token', {
					method: 'POST',
					// headers: {'Content-Type': 'application/x-www-form-urlencoded'},
					credentials: 'include',
          body: formData
          }
				).then(res => {
                    console.log(res.status)
                    console.log("logged")

                })
                .catch(err => {
                    console.log(err)
                })
      
    }
  }
}
</script>

<style>

</style>