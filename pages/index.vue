<script setup>
definePageMeta({
  middleware: ["auth"],
});

useHead({
  title: "Amore | Confess",
  meta: [
    {
      name: "description",
      content: "Confess Your Secrets Here",
    },
  ],
});

const db = useSupabaseClient();
const user = useSupabaseUser();

const inputField = ref("");

const confessions = await db
  .from("Confession")
  .select(
    `content,
  creator (avatar_url, email, name)`
  )
  .order("created_at", { ascending: false });
if (confessions.error) {
  console.error(confessions.error);
}

const onSubmit = async () => {
  const { data, error } = await db.from("Confession").insert([
    {
      content: inputField.value,
      creator: user.value.id,
    },
  ]);
  if (error) {
    console.error(error);
  }
};
</script>

<template>
  <Navbar />
  <form class="mt-8 flex" @submit="onSubmit">
    <input
      class="w-full py-2 border-[#404040] border-r-[1px] rounded-l-md placeholder:text-neutral-500 outline-none bg-[#262626] px-3 text-md text-white"
      type="text"
      v-model="inputField"
      minlength="4"
      placeholder="Confess Your Secrets Here"
    />
    <input
      class="bg-[#262626] border-[#404040] border-r-[1px] flex flex-row items-center justify-center gap-1 text-white px-6 py-1 rounded-r-md"
      type="submit"
      value="Confess"
    />
  </form>
  <div class="flex items-center">
    <h1 class="text-white text-xl pb-1 mt-4">All Confessions</h1>
  </div>
  <div
    class="flex text-md mt-2 flex-row gap-2"
    v-for="confession in confessions.data"
  >
    <img :src="confession.creator.avatar_url" class="w-6 h-6 rounded-full" />
    <div class="text-[#737373]">
      {{ confession.creator.name }}:
      <span class="text-white">
        {{ confession.content }}
      </span>
    </div>
  </div>
</template>
