<template>
  <main class="main_body">
    <h1>Gallery Items</h1>

    <div class="main" :key="index" v-for="(coreValue, index) in data">
      <img class="se" :src="coreValue.url" @error="imagenotfound"/>

      <p class="Label">{{ coreValue.label }}</p>
    </div>
  </main>
</template>

<script setup lang="ts">
import { onMounted, onUnmounted, ref } from "vue";
import image from "@/assets/no-image.png";

const data = ref();
const images = ref(image);
const notFound = ref(0);
const dataFetch = ref(false);
const currentRetry = ref(0);
let retrydelay = ref(1000);
const maxretry = ref(10);

 function imagenotfound(event: any) {
  notFound.value++;
  console.log("value", notFound.value);
  event.target.src = images.value;

 }

async function info() {
  const response = await fetch("https://s3.eu-west-2.amazonaws.com/assets-test.fast-thinking.co.uk/recruitment/data.json");
  data.value = await response.json();
  console.log("response",response);
  console.log("data.valueinside info",data.value);
  dataFetch.value = true;
}

 async function fisrt_retry() {
  console.log("datafetch", dataFetch.value);
  console.log("notfound", notFound.value);

    if (notFound.value > 0) {
      console.log(retrydelay.value);
      console.log(retrydelay.value);
      console.log("retrydelay 2", retrydelay.value);
      console.log("currentRetry 2", currentRetry.value);
      const response = await fetch("https://s3.eu-west-2.amazonaws.com/assets-test.fast-thinking.co.uk/recruitment/data.json");
      data.value = await response.json();
      console.log("data.valueinside first",data.value);
       setTimeout(retry, 1000);
    } else {
      return false;
    }
}

async function retry() {
  console.log("datafetch", dataFetch.value);
  console.log("notfound", notFound.value);
    if (notFound.value > 0) {
      if (currentRetry.value < maxretry.value) {
        retrydelay.value = retrydelay.value * 2;
        console.log(retrydelay.value);
        console.log(retrydelay.value);
        currentRetry.value++;
        console.log("retrydelay 1", retrydelay.value);
        console.log("currentRetry 1", currentRetry.value);
      await info();
       const f =  setTimeout(retry, retrydelay.value);
       console.log("f", f);
      }
    } else {
      return false;
    }
  
}

// async function retry() {
//   console.log("retry");

//   if (dataFetch.value && notFound.value > 0 && currentRetry.value < maxretry.value) {
//     retrydelay.value = retrydelay.value * 2;
//     console.log(retrydelay.value);
//     currentRetry.value++;
//     console.log("retrydelay 1", retrydelay.value);
//     // console.log("currentRetry 1", currentRetry.value);
//     await info();
//     await new Promise(resolve => setTimeout(resolve, retrydelay.value));
//   }

//   return false;
// }

onMounted(() => {

   info();
   setTimeout(fisrt_retry, 4000);
  setTimeout(() => {
    const mockEvent = { target: { tagName: 'img' } };
    console.log(mockEvent);
  imagenotfound(mockEvent)
   }, 2000);


  setTimeout(retry, retrydelay.value);

});

onUnmounted(()=>{
  console.log("unmounted");

})

// Call the retry function initially

//  settimeintreval sec dynamic
//  if api fetch not fetch return false
//  if api fetch 1) && image not found && count > trials (10)
//  if image not found call api again (call info()) & inrease inyervew by *2
</script>

<style lang="scss" scoped>
.se {
  height: 140px;
  width: 160px;
}
.main {
  display: inline-block;
  padding: 6px;
  text-align: center;
}
.Label {
  margin: 0px 35px;
  width: 102px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  font-family: serif;
  font-size: 12px;
  color: black;
}
</style>
