<template>
  {{ message }}
</template>

<script lang="ts">
import {onMounted, ref} from 'vue';
import {useStore} from "vuex";

export default {
  name: "Home",
  setup() {
    const message = ref('You are not logged in!');
    const store = useStore();

    onMounted(async () => {
      try {
        const userName = localStorage.getItem('name');
        if (userName) {
          message.value = `Hi ${userName}`;
          await store.dispatch('setAuth', true);
        } else {
          await store.dispatch('setAuth', false);
        }
      } catch (error) {
        console.error(error);
    }
    });

    return {
      message
    }
  }
}
</script>
