<script setup lang="ts">
const supabase = useSupabaseAuthClient();
const user = useSupabaseUser();

const logout = async () => {
  const { error } = await supabase.auth.signOut();
  if (error) {
    console.error(error);
  } else {
    navigateTo("/auth");
  }
};

const login = async () => {
  const { error } = await supabase.auth.signInWithOAuth({
    provider: "github",
  });
  if (error) {
    console.error(error);
  } else {
    navigateTo("/");
  }
};
</script>

<template>
  <div class="flex text-lg flex-row gap-3 bg-transparent">
    <NuxtLink
      activeClass="text-white pointer-events-none"
      class="text-neutral-500 hover:text-white"
      to="/"
      >home</NuxtLink
    >
    <NuxtLink
      class="text-neutral-500 hover:text-white"
      to="/profile"
      activeClass="text-white pointer-events-none"
      >profile</NuxtLink
    >
    <a v-if="user" class="text-neutral-500 hover:text-white" @click="logout"
      >logout</a
    >
    <a v-else class="text-neutral-500 hover:text-white" @click="login">login</a>
  </div>
</template>
