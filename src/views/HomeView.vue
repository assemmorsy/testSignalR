<script setup lang="ts">
import * as signalR from "@microsoft/signalr";
import { onMounted, ref } from "vue";

const connection = new signalR.HubConnectionBuilder()
  .withUrl("https://sam-baloot-admin.online/saudi-baloot-olympics/dev/bracket-hub", {
    withCredentials: true
  })
  .build();

onMounted(async () => {
  try {
    await connection.start();
    gameString.value = await connection.invoke('GetGroupBracket', 17);
  }
  catch (err) {
    console.error(err);
  }

  connection.on("BracketChanged", (groupId: number, groupData: string) => {
    console.log(groupData);
    gameString.value = groupData;
  });
})

const gameString = ref<null | string>(null);
</script>

<template>
  <main>
    <RouterLink to="camera">go to camera</RouterLink>
    <!-- {{ gameString }} -->
  </main>
</template>
