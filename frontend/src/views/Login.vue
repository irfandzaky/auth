<template>
  <form @submit.prevent="submit">
    <h1 class="h3 mb-3 fw-normal">Please sign in</h1>

    <input v-model="data.email" type="email" class="form-control" placeholder="Email" required>

    <input v-model="data.password" type="password" class="form-control" placeholder="Password" required>

    <button class="w-100 btn btn-lg btn-primary" type="submit">Sign in</button>
  </form>
</template>

<script lang="ts">
import {reactive} from 'vue';
import {useRouter} from "vue-router";

export default {
  name: "Login",
  setup() {
    const data = reactive({
      email: '',
      password: ''
    });
    const router = useRouter();

    const submit = async () => {
        const response = await fetch('http://localhost:8000/api/login', {
        method: 'POST',
        headers: {'Content-Type': 'application/json'},
        credentials: 'include',
        body: JSON.stringify(data)
      });

      if (response.ok) {
        const result = await response.json();
        console.log(result);
        localStorage.setItem('name', result.name);
        console.log(localStorage.getItem('name'));
        await router.push('/');
      } else {
        console.error('Login failed');
      }
    }

    return {
      data,
      submit
    }
  }
}
</script>