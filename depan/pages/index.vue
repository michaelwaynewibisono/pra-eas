<template>
  <div class="flex">
    <div
      class="min-h-screen w-full flex flex-col gap-4 justify-center items-center"
    >
      <div class="text-5xl font-bold text-center">
        Get Started <br />
        with
        <span class="text-6xl text-[#1B4242]">Local Chat</span>
      </div>
      <div class="flex gap-3 text-[#F8F8F8]">
        <NuxtLink
          to="/chat"
          class="cursor-pointer bg-[#2e2e2e] py-3 px-4 rounded-sm text-xl"
        >
          Chat
        </NuxtLink>
        <NuxtLink
          to="/register"
          class="cursor-pointer bg-[#2e2e2e] py-3 px-4 rounded-sm text-xl"
        >
          Register
        </NuxtLink>
      </div>
    </div>
    <div class="hidden lg:block min-h-screen w-full">
      <img
        src="/chat-conversation-communication-connection-concept.jpg"
        alt="background-image"
        class="min-h-screen w-full object-cover"
      />
    </div>
  </div>
</template>

<script setup></script>
