<template>
  <!-- Output -->
  <NuxtLink
    to="/chat"
    class="hidden md:block absolute top-6 left-6 text-[#f8f8f8] bg-[#2e2e2e] font-semibold p-3 cursor-pointer rounded-sm select-none"
  >
    go Back
  </NuxtLink>
  <div class="flex justify-center py-6 w-full">
    <div
      class="block w-[90%] md:w-[50%] h-[83vh] rounded-md overflow-y-auto scrollbar p-3 bg-[#F3F3F3]"
    >
      <div v-for="display in displays" :key="display.id">
        <div class="flex justify-between">
          <div
            :class="`${display.alignment_1} ${
              display.user.id !== chatNow.user ? 'hidden' : ''
            }`"
          ></div>
          <div
            :class="`${display.alignment_text} ${
              display.user.id == chatNow.user ? 'text-right' : 'text-left'
            } rounded-md bg-[#2e2e2e] text-[#f8f8f8] px-6 py-2 min-w-[50%] max-w-[90%] mb-4`"
          >
            <div v-if="display.user.id !== chatNow.user" class="font-semibold">
              {{ display.user.full_name }}
            </div>
            <div>{{ display.text }}</div>
          </div>
          <div
            :class="`${display.alignment_2} ${
              display.user.id !== chatNow.user ? 'block' : 'hidden'
            }`"
          ></div>
        </div>
      </div>
    </div>
  </div>

  <!-- Input -->
  <div
    class="fixed bottom-6 w-[90%] md:w-[50%] left-1/2 transform -translate-x-1/2"
  >
    <form
      @submit.prevent="startChat"
      enctype="multipart/form-data"
      class="flex gap-3"
    >
      <input type="text" v-model="chatNow.user" class="hidden" />
      <input type="text" v-model="chatNow.category" class="hidden" />
      <input
        type="text"
        v-model="chatNow.text"
        class="bg-[#F3F3F3] rounded-md px-3 w-full focus:outline-none"
        :placeholder="`Message from ${full_name}`"
      />
      <button class="text-3xl p-1 bg-[#00A6ED] rounded-md">➡️</button>
    </form>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();

//Display Chat
const displays = ref([]);

const displayChat = async (categoryId) => {
  const res = await fetch(
    `http://localhost:5000/api/SMS?limit=100&sort=createdAt&page=1&where%5Bor%5D%5B0%5D%5Band%5D%5B0%5D%5Bcategory%5D%5Bequals%5D=${categoryId}`
  );
  const data = await res.json();
  displays.value = data.docs;
};

//Create Chat
const chatNow = ref({
  user: "",
  category: "",
  text: "",
});

const startChat = async () => {
  const currentDate = new Date();
  const formattedDate = currentDate.toLocaleString();
  const { data: responseData } = await useFetch(
    "http://localhost:5000/api/SMS",
    {
      method: "POST",
      body: {
        user: chatNow.value.user,
        category: chatNow.value.category,
        text: chatNow.value.text,
      },
    }
  );
  displays.value.push({
    user: chatNow.value.user,
    text: chatNow.value.text,
    alignment_1: "!block",
    alignment_text: "!text-right",
    alignment_2: "hidden",
  });
  chatNow.value.text = "";
  console.log(responseData.value);
};

// Fetch Category Id
const fetchCategory = async () => {
  const id = router.currentRoute.value.params.id;
  const response = await fetch(`http://localhost:5000/api/category/${id}`, {
    method: "GET",
    headers: {
      "Content-Type": "application/json",
    },
  });
  const data = await response.json();
  chatNow.value.category = data.id;
  displayChat(data.id);
};

// Fetch current user data
const full_name = ref("");
const fetchCurrentUser = async () => {
  const response = await fetch("http://localhost:5000/api/customers/me", {
    credentials: "include",
    method: "GET",
    headers: {
      "Content-Type": "application/json",
    },
  });

  const userData = await response.json();
  chatNow.value.user = userData.user.id;
  full_name.value = userData.user.full_name;
  return userData.user;
};

onMounted(async () => {
  displayChat();
  await fetchCategory();
  await fetchCurrentUser();
});
</script>
