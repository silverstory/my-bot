<script>
  import { onMount } from "svelte";
  import { fade, fly } from "svelte/transition";
  import { spring } from "svelte/motion";
  import BouncingDots from "./BouncingDots.svelte";

  export let who = "you";
  export let text = "";
  export let ready = false;
  export let isolateDelay;

  $: _ready = who === "you" ? true : ready;

  let el;

  const height = spring(55); // 35
  const width = spring(120); // 60

  // $: _ready && width.set(300) && height.set(el + 30);

  $: _ready && height.set(el + 30);

  onMount(() => {
    setTimeout(() => {
      window.scrollTo({
        top: document.documentElement.scrollHeight,
        behavior: "smooth",
      });
    }, isolateDelay * 2 - 200);
  });
</script>

<div style="height: {el + 60 || 35}px">
  <div
    in:fly={{
      y: 50,
      opacity: 0,
      duration: 250,
      delay: who === "you" ? 1000 : 200,
    }}
    style="height: {$height}px"
  >
    <!-- <div
      in:fly={{
        y: 50,
        opacity: 0,
        duration: 250,
        delay: who === "you" ? 1000 : 200,
      }}
      class="box {who}"
      class:ready={_ready}
      style="width: {$width}px; height: {$height}px"
    > -->
    {#if !_ready && who !== "you"}
      <div transition:fade={{ duration: 100 }} class="flex justify-start ml-4">
        <span class="circle animate-loader" />
        <span class="circle animate-loader animation-delay-200" />
        <span class="circle animate-loader animation-delay-400" />
      </div>

      <!-- skeletons sample -->
      <!-- <div transition:fade={{ duration: 100 }} class="chat-message">
          <div class="flex items-end">
            <div
              class="border border-blue-300 shadow rounded-md p-4 max-w-sm w-full mx-auto"
            >
              <div class="animate-pulse flex space-x-4">
                <div class="rounded-full bg-slate-700 h-10 w-10" />
                <div class="flex-1 space-y-6 py-1">
                  <div class="h-2 bg-slate-700 rounded" />
                  <div class="space-y-3">
                    <div class="grid grid-cols-3 gap-4">
                      <div class="h-2 bg-slate-700 rounded col-span-2" />
                      <div class="h-2 bg-slate-700 rounded col-span-1" />
                    </div>
                    <div class="h-2 bg-slate-700 rounded" />
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div> -->
    {/if}

    <!-- <div class="text-box" class:show={_ready} bind:clientHeight={el}>
        <p>
          {text}
        </p>
      </div> -->

    {#if _ready && who === "you"}
      <div class="chat-message" bind:clientHeight={el}>
        <div class="flex items-end justify-end">
          <div
            class="flex flex-col space-y-2 text-xs max-w-xs mx-2 order-1 items-end"
          >
            <div>
              <span
                class="px-4 py-2 rounded-lg inline-block rounded-br-none bg-blue-600 text-white"
                >{text}</span
              >
            </div>
          </div>
          <img
            src="https://images.unsplash.com/photo-1590031905470-a1a1feacbb0b?ixlib=rb-1.2.1&amp;ixid=eyJhcHBfaWQiOjEyMDd9&amp;auto=format&amp;fit=facearea&amp;facepad=3&amp;w=144&amp;h=144"
            alt="My profile"
            class="w-6 h-6 rounded-full order-2"
          />
        </div>
      </div>
    {/if}

    {#if _ready && who === "them"}
      <div class="chat-message" bind:clientHeight={el}>
        <div class="flex items-end">
          <div
            class="flex flex-col space-y-2 text-xs max-w-xs mx-2 order-2 items-start"
          >
            <div>
              <span
                class="px-4 py-2 rounded-lg inline-block rounded-bl-none bg-gray-300 text-gray-600"
                >{text}</span
              >
            </div>
          </div>
          <img
            src="https://images.unsplash.com/photo-1597621804952-2ac4a9a67359?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=facearea&facepad=3&w=144&h=144"
            alt="My profile"
            class="w-6 h-6 rounded-full order-1"
          />
        </div>
      </div>
    {/if}
  </div>
</div>
