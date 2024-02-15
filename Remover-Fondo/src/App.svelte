<script lang="ts">
  import ImportDropzone from "./ImportDropzone.svelte";
  import { imageStatus } from "./store";
  import { ImageStatus } from "./types.d";
  import StepEdit from "./StepEdit.svelte";

</script>


<div class=" max-w-xl m-auto grid grid-cols-1 place-content-center w-full h-screen p-4">
    <header class="flex justify-center py-10">
      <h1 class="text-5xl font-bold text-gray-800 tracking-tighter">
        Remove<span class="text-gray-400">BG</span>
       </h1>
    </header>    
  

  <main>
    {#if $imageStatus === ImageStatus.READY || $imageStatus === ImageStatus.UPLOADING}
      <ImportDropzone/>
    {:else if $imageStatus === ImageStatus.DONE}
      <StepEdit/>
    {/if}
   </main>

</div>

<footer class="text-center p-5 shadow shadow-black" >
  <div class="flex justify-center gap-1">
    <div class="w-12 h-8 bg-gray-800"></div>
    <div class="w-12 h-8 bg-blue-500"></div>
    <div class="w-12 h-8 bg-gray-400"></div>
</div>

  <div class="mt-2 font-sans italic font-medium">
    © 2022 All rights reserved by - Alexis Acevedo.
  </div>
</footer>