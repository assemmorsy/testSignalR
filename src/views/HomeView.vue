<script setup lang="ts">
import * as signalR from "@microsoft/signalr";
import { onMounted, ref } from "vue";

const connection = new signalR.HubConnectionBuilder()
  .withUrl("https://sam-baloot-admin.online/qydha/baloot-games-hub", {
    withCredentials: true
  })
  .build();

onMounted(async () => {
  try {
    await connection.start();
    let res = await connection.invoke("addToBoardGroup", "3803a189-53af-4bde-b36c-b8e7937984c6");
    console.log(res);
    gameString.value = res;
  }
  catch (err) {
    console.error(err);
  }

  connection.on("balootGameStateChanged", (eventsEffect: string, game: string) => {
    console.log("function called");
    console.log(eventsEffect);
    console.log(game);
    gameString.value = game;
  });
})

const gameString = ref<null | string>(null);
</script>

<template>
  <main>
    {{ gameString }}
  </main>
</template>
