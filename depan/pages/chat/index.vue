<template>
  <div
    class="sm:hidden block absolute bottom-6 left-6 text-[#f8f8f8] bg-[#2e2e2e] font-semibold p-4 cursor-pointer rounded-sm select-none"
    @click="handleLogout"
  >
    Logout
  </div>
  <div
    class="sticky top-0 min-h-[10vh] bg-[#2e2e2e] w-full flex justify-between items-center py-4 px-6"
  >
    <form
      class="flex gap-2"
      @submit.prevent="startCategory"
      enctype="multipart/form-data"
    >
      <input
        type="text"
        v-model="categoryNow.name"
        placeholder="Add new category"
        class="bg-[#F8F8F8] border-none focus:outline-none text-lh rounded-sm px-4"
      />
      <button
        class="cursor-pointer text-[#2e2e2e] bg-[#f8f8f8] font-semibold p-4 rounded-sm select-none"
      >
        Add
      </button>
    </form>
    <!-- <div v-if="currentUser" class="text-xl font-semibold">
      Hi, {{ currentUser.full_name }}!
    </div> -->
    <div class="sm:hidden block"></div>
    <div
      class="hidden sm:block text-[#2e2e2e] bg-[#f8f8f8] font-semibold p-4 cursor-pointer rounded-sm select-none"
      @click="handleLogout"
    >
      Logout
    </div>
  </div>
  <div
    class="grid xl:grid-cols-4 lg:grid-cols-3 sm:grid-cols-2 grid-cols-1 gap-4 px-6 mt-6"
  >
    <div v-for="category in categories" :key="category.id" class="">
      <div
        class="bg-[#2e2e2e] text-[#F8F8F8] rounded-md h-[30vh] p-6 flex flex-col justify-between"
      >
        <div class="text-xl">{{ category.name }}</div>
        <div class="fill-[#f8f8f8] flex gap-2">
          <NuxtLink :to="`/category/${category.id}`" class="cursor-pointer"
            >Enter</NuxtLink
          >

          <svg
            xmlns="http://www.w3.org/2000/svg"
            height="24"
            width="24"
            viewBox="0 0 512 512"
          >
            <path
              d="M217.9 105.9L340.7 228.7c7.2 7.2 11.3 17.1 11.3 27.3s-4.1 20.1-11.3 27.3L217.9 406.1c-6.4 6.4-15 9.9-24 9.9c-18.7 0-33.9-15.2-33.9-33.9l0-62.1L32 320c-17.7 0-32-14.3-32-32l0-64c0-17.7 14.3-32 32-32l128 0 0-62.1c0-18.7 15.2-33.9 33.9-33.9c9 0 17.6 3.6 24 9.9zM352 416l64 0c17.7 0 32-14.3 32-32l0-256c0-17.7-14.3-32-32-32l-64 0c-17.7 0-32-14.3-32-32s14.3-32 32-32l64 0c53 0 96 43 96 96l0 256c0 53-43 96-96 96l-64 0c-17.7 0-32-14.3-32-32s14.3-32 32-32z"
            />
          </svg>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();

//Read Category
const categories = ref([]);
const fetchCategory = async () => {
  const response = await fetch("http://localhost:5000/api/category");
  const data = await response.json();
  categories.value = data.docs;
};

onMounted(() => {
  fetchCategory();
});

//Create Chat
const categoryNow = ref({
  name: "",
});
const startCategory = async () => {
  const { data: responseData } = await useFetch(
    "http://localhost:5000/api/category",
    {
      method: "POST",
      body: {
        name: categoryNow.value.name,
      },
    }
  );
  console.log(responseData.value);
  router.go();
};

//Read JSON
const currentUser = ref("");
onMounted(async () => {
  const userData = await getUser();
  if (!userData) {
    router.push("/login");
  } else {
    currentUser.value = userData;
  }
});

const getUser = async () => {
  const res = await fetch("http://localhost:5000/api/customers/me", {
    credentials: "include",
    method: "GET",
    headers: {
      "Content-Type": "application/json",
    },
  });
  const data = (await res.json())?.user;
  return data;
};

//Logout Request
const handleLogout = async () => {
  const res = await fetch("http://localhost:5000/api/customers/logout", {
    credentials: "include",
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
  });
  if (res.ok) {
    console.log("berhasil logout");
    router.go();
  }
};
</script>
