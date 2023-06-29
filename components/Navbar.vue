<script setup>
const supabase = useSupabaseAuthClient();
const logout = async () => {
  const { error } = await supabase.auth.signOut();
  if (error) {
    console.error(error);
  } else {
    navigateTo("/auth");
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
    <a class="text-neutral-500 hover:text-white" @click="logout">logout</a>
  </div>
</template>
