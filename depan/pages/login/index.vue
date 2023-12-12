<template>
  <div
    class="flex flex-col justify-center items-center bg-gradient-to-r from-[#43C6AC] to-[#191654] min-h-screen w-full"
  >
    <div class="flex justify-between lg:w-[40%] w-[80%]">
      <div></div>
      <NuxtLink
        to="/"
        class="text-[#2e2e2e] bg-[#f8f8f8] p-3 rounded-sm mb-4 font-semibold cursor-pointer"
        >back Home</NuxtLink
      >
    </div>
    <form
      @submit.prevent="loginUser"
      class="lg:w-[40%] w-[80%] bg-[#FFFFFF] py-10 px-10 rounded-md flex flex-col gap-10"
    >
      <div class="text-3xl text-center font-bold">Login Form</div>
      <div class="w-full border-b border-[#2e2e2e]">
        <input
          class="w-full border-none focus:outline-none text-lg"
          type="email"
          v-model="email"
          placeholder="Email"
          required
        />
      </div>
      <div class="w-full border-b border-[#2e2e2e]">
        <input
          type="password"
          v-model="password"
          placeholder="Password"
          class="w-full border-none focus:outline-none text-lg"
          required
        />
      </div>
      <div>
        First time using <b>Local Chat</b>?
        <NuxtLink to="/register" class="underline">Register Here</NuxtLink>
      </div>
      <button
        type="submit"
        class="w-full text-lg py-2 px-4 rounded-md bg-[#2e2e2e] text-[#f8f8f8]"
      >
        Login
      </button>
    </form>
  </div>
</template>

<script setup>
import { ref } from "vue";

import { useRouter } from "vue-router";

const router = useRouter();

const email = ref("");
const password = ref("");

const loginUser = async () => {
  const res = await fetch("http://localhost:5000/api/customers/login", {
    method: "POST",
    credentials: "include",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      email: email.value,
      password: password.value,
    }),
  });

  const json = await res.json();
  console.log(json);
  if (!res.ok) {
    alert("email " + email.value + " gagal login");
  } else {
    console.log("email " + email.value + " telah berhasil login");
    router.push("/chat");
  }
};
</script>
