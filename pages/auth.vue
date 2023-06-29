<script setup lang="ts">
useHead({
  title: "Amore | Login",
  meta: [
    {
      name: "description",
      content: "Confess Your Secrets Here",
    },
  ],
});
const supabase = useSupabaseAuthClient();

const loginGithub = async () => {
  const { error } = await supabase.auth.signInWithOAuth({
    provider: "github",
  });

  if (error) {
    console.error(error);
  } else {
    navigateTo("/");
  }
};

const loginGoogle = async () => {
  const { error } = await supabase.auth.signInWithOAuth({
    provider: "google",
  });

  if (error) {
    console.error(error);
  } else {
    navigateTo("/");
  }
};
</script>

<template>
  <div class="flex flex-col gap-2 items-center justify-center min-h-screen">
    <h2 class="text-2xl text-white font-semibold">Amore</h2>
    <p class="font-regular text-white">Confess Your Secrets Here</p>
    <button
      class="bg-[#262626] border-[#404040] border-[1px] flex flex-row items-center justify-center gap-1 text-white px-8 py-1 rounded-md"
      @click="loginGithub"
    >
      <Icon name="uil:github" color="grey" class="text-2xl" />

      Sign In With Github
    </button>
    <button
      class="bg-[#262626] border-[#404040] border-[1px] flex flex-row items-center justify-center gap-1 text-white px-8 py-1 rounded-md"
      @click="loginGoogle"
    >
      <Icon name="uil:google" color="grey" class="text-2xl" />

      Sign In With Google
    </button>
  </div>
</template>
