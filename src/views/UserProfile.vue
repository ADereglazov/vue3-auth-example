<template>
  <div v-show="isLogged" class="user-profile">
    <h1 class="user-profile__title">Profile</h1>

    <p class="user-profile__field">{{ authUser?.firstName || "unknown" }}</p>
    <p class="user-profile__field">{{ authUser?.lastName || "unknown" }}</p>
    <p class="user-profile__field">{{ authUser?.email }}</p>

    <ActionButton
      type="button"
      text="Log out"
      class="user-profile__button"
      @click="logout"
    />
  </div>
  <LoadingSpinner v-if="pending" />
</template>

<script>
import { computed, ref } from "vue";
import { storeToRefs } from "pinia";
import { useAuthStore } from "@/store";
import ActionButton from "@/components/ActionButton.vue";
import LoadingSpinner from "@/components/LoadingSpinner.vue";

export default {
  name: "UserProfile",
  components: { ActionButton, LoadingSpinner },
  setup() {
    const isLogged = ref(true);
    const authStore = useAuthStore();
    const { user: authUser } = storeToRefs(authStore);
    const pending = computed(() => authStore.pending);

    function logout() {
      isLogged.value = false;
      authStore.pending = true;
      authStore.logout();
      authStore.pending = false;
    }

    return { isLogged, authUser, pending, logout };
  },
};
</script>

<style scoped lang="less">
.user-profile {
  width: 100%;
  min-width: 320px;
  max-width: 400px;
  margin: 0 auto;

  &__title {
    margin-top: 20px;
    margin-bottom: 20px;
    color: var(--vt-c-black);
  }

  &__field {
    margin-top: 0;
    margin-bottom: 0;
    border-bottom: 1px solid var(--vt-c-black-mute);
    color: var(--vt-c-black);

    & + & {
      margin-top: 15px;
    }
  }

  &__button {
    margin-top: 30px;
  }
}
</style>
