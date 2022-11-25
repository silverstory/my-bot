<script>
  import { onMount } from "svelte";
  import { fade, fly } from "svelte/transition";
  import { spring } from "svelte/motion";
  import BouncingDots from "./BouncingDots.svelte";

  export let who = "you";
  export let text = "";
  export let ready = false;
  export let isolateDelay;
  export let widget = '';

  $: _ready = who === "you" ? true : ready;

  let el;
  let el1;

  const height = spring(35);
  const width = spring(60);
  const height1 = spring(35);
  const width1 = spring(60);

  // $: _ready && width.set(300) && height.set(el + 30);
  $: _ready && width.set(300) && height.set(el + 20);
  $: _ready && width1.set(300) && height1.set(el1 + 10);

  onMount(() => {
    setTimeout(() => {
      window.scrollTo({
        top: document.documentElement.scrollHeight,
        behavior: "smooth",
      });
    }, isolateDelay * 2 - 200);
  });
</script>

<!-- <div style="height: {el + 60 || 35}px"> -->
<div style="height: {el + 60 || 25}px">
  <div
    in:fly={{
      y: 50,
      opacity: 0,
      duration: 250,
      delay: who === "you" ? 1000 : 200,
    }}
    class="box {who}"
    class:ready={_ready}
    style="width: {$width}px; height: {$height}px"
  >
    {#if !_ready && who !== "you"}
      <div transition:fade={{ duration: 100 }} class="bouncy-box">
        <BouncingDots />
      </div>
    {/if}
    <div class="text-box" class:show={_ready} bind:clientHeight={el}>
      <p>
        {text}
      </p>
    </div>
  </div>
</div>

{#if $$slots.widgets}
  {#if widget !== ''}
    <div style="height: {el1 +40 || 15}px">
      <div
        in:fly={{
          y: 50,
          opacity: 0,
          duration: 250,
          delay: who === "you" ? 1000 : 200,
        }}
        class="widget {who}"
        class:ready={_ready}
        style="width: {$width1}px; height: {$height1}px"
      >
        <div class="text-box" class:show={_ready} bind:clientHeight={el1}>
          <slot name="widgets"></slot>
        </div>
      </div>
    </div>
  {/if}
{/if}

<style>

  .has-options::after {
		content: '';
		background-color: white;
		height: 0px;
		width: 0px;
	}

  .box {
    background-color: #eee;
    border-radius: 5px;
    font-family: helvetica;
    background-color: #eee;
    border-radius: 25px;
    /* margin-bottom: 30px; */
    margin-bottom: 20px;
    font-family: helvetica;
  }

  .widget {
    background-color: #eee;
    border-radius: 5px;
    font-family: helvetica;
    background-color: #eee;
    border-radius: 50px;
    /* margin-bottom: 30px; */
    margin-bottom: 10px;
    font-family: helvetica;
  }

  .bouncy-box {
    width: 55px; /* 60 */
    height: 82px; /* 45 */
    padding-bottom: 15px;
  }

  .text-box {
    width: 300px;
    opacity: 0;
    transition: 0.2s;
    transition-delay: 0.2s;
  }
  .show {
    opacity: 1;
  }

  .ready {
    width: 300px;
  }

  p {
    padding: 5px 20px;
  }

  .you {
    border-bottom-right-radius: 0px;
    float: right;
    clear: both;
    /* background: #3865f4;
    color: #fff; */
    --tw-bg-opacity: 1;
    background-color: rgb(37 99 235 / var(--tw-bg-opacity));
    --tw-text-opacity: 1;
    color: rgb(255 255 255 / var(--tw-text-opacity));
  }

  .them {
    border-top-left-radius: 0px;
    float: left;
    clear: both;
    --tw-bg-opacity: 1;
    background-color: rgb(209 213 219 / var(--tw-bg-opacity));
    --tw-text-opacity: 1;
    color: rgb(75 85 99 / var(--tw-text-opacity));
  }

  .bg-gray-300 {
    --tw-bg-opacity: 1;
    background-color: rgb(209 213 219 / var(--tw-bg-opacity));
  }

  .text-gray-600 {
    --tw-text-opacity: 1;
    color: rgb(75 85 99 / var(--tw-text-opacity));
  }

  .bg-blue-600 {
    --tw-bg-opacity: 1;
    background-color: rgb(37 99 235 / var(--tw-bg-opacity));
  }

  .text-white {
    --tw-text-opacity: 1;
    color: rgb(255 255 255 / var(--tw-text-opacity));
  }

</style>
