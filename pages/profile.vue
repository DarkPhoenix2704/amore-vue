<script setup>
useHead({
  title: "Amore | Profile",
  meta: [
    {
      name: "description",
      content: "Confess Your Secrets Here",
    },
  ],
});

definePageMeta({
  middleware: ["auth"],
});
const db = useSupabaseClient();
const user = useSupabaseUser();
const { data, err } = await db.from("Profile").select().eq("id", user.value.id);
if (err) {
  console.error(err);
  navigateTo("/auth");
}

const confessions = await db
  .from("Confession")
  .select()
  .eq("creator", user.value.id)
  .order("created_at", { ascending: false });
if (confessions.err) {
  console.error(confessions.err);
  navigateTo("/auth");
}
</script>

<template>
  <Navbar />
  <div class="gap-4 flex flex-col">
    <div class="flex items-center mt-8">
      <div class="flex flex-col gap-4">
        <img :src="data[0].avatar_url" class="w-24 h-24 rounded-full" />
        <div>
          <p class="text-2xl font-medium text-white">{{ data[0].name }}</p>
          <p class="text-lg font-regular text-[#737373]">{{ data[0].email }}</p>
        </div>
      </div>
    </div>
    <div>
      <h1 class="text-white text-xl pb-1">My Confessions</h1>
      <div
        class="text-[#737373] text-lg"
        v-for="confession in confessions.data"
      >
        {{ confession.content }}
      </div>
    </div>
  </div>
</template>
