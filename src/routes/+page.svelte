<script context="module">
  import Device from 'svelte-device-info'
</script>

<!-- component -->
<script>
  import ChatBox from "../shared/ChatBox.svelte";
  import BouncingDots from "../shared/BouncingDots.svelte";
  import Home from "../shared/testdrive/Home.svelte";
  import { onMount } from "svelte";
  import { afterUpdate, tick } from "svelte";
  import Intro from "../shared/widgets/Intro.svelte";

  const BotMessage = {
    Intro: "Hi! I'm Jane D. The 8888 virtual assistant. How can I help you today?",
    ImJustaBot: "Sorry, I don't understand that. I'm just a bot.",
    FirstName: "Please enter your first name",
    LastName: "Enter your lastname",
  };

  let lastbotmessage = BotMessage.Intro;

  let userchat = '';

  let userchatInput;

  let lockinput = true;

  function sleep(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
  }

  function readingTime(text) {
    const wps = 225 / 60;
    const words = text.trim().split(/\s+/).length;
    const time = Math.ceil((words / wps) * 1000);
    return time;
  }

  const text = BotMessage.Intro;

  const readtime = readingTime(text);
  const time = readtime < 2000 ? 100 : 200;

  let current = [
    {
      text: text,
      who: "them",
      ready: false,
      isolateDelay: time,
      widget: 'intro',
    },
  ];

  let active_index = 0;
  let displacement = 0;

  const popchat = () => {
    if (displacement < current[active_index].isolateDelay) {
      displacement += 1;
      requestAnimationFrame(popchat);
    } else {
      current[active_index].ready = true;
      lockinput = false;

      console.log('this device is ' + (Device.isMobile ? '' : 'not') + ' mobile')
  
      switch (true) {
        case Device.isPhone:  console.log('this device is a smartphone'); break
        case Device.isTablet: console.log('this device is a tablet');     break
        default:              userchatInput.focus() // console.log('this device is neither a smartphone nor a tablet')
      }

    }
  };

  onMount(() => {
    let frame;

    popchat();

    return () => {
      cancelAnimationFrame(frame);
    };
  });

  function handleBot(reply) {
    // const text =
    //   "Lorem Ipsum is simply dummy text of the printing and typesetting industry.";

    const text = reply;

    const readtime = readingTime(text);
    // const time = readtime < 2000 ? 100 : 200;
    const time = readtime < 2000 ? 300 : 400;

    // determine widget to render here

    current = [
      ...current,
      {
        text: text,
        who: "them",
        ready: false,
        isolateDelay: time,
        widget: '',
      },
    ];
    displacement = 0;
    active_index = current.length - 1;
    popchat();
  }

  function handleKeydown(event) {
		if (event.key === 'Enter') {
      lockinput = true;
			const text = event.target.value;
			if (!text) return;

      userchat = text;

      handleUser();

      event.target.value = '';
    }
  }

  async function handleUser() {

    lockinput = true;

    const text = userchat;

    // const text =
    //   "Lorem Ipsum is simply dummy text of the printing and typesetting industry.";

    const readtime = readingTime(text);
    const time = readtime < 2000 ? 100 : 200;

    // current = [
    //   ...current,
    //   {
    //     text: text,
    //     who: "you",
    //     ready: false,
    //     isolateDelay: time,
    //   },
    // ];

    current = current.concat({
        text: text,
        who: "you",
        ready: false,
        isolateDelay: time,
      });

    userchat = '';

    displacement = 0;
    active_index = current.length - 1;

    popchat();

    await sleep(1600);

    let reply = '';

    switch (lastbotmessage) {
        case BotMessage.Intro     :  {
          reply = BotMessage.ImJustaBot;
          break;
        }
        case BotMessage.FirstName : console.log('first name');     break
        case BotMessage.LastName  : console.log('last name');     break
        default:              console.log('default');     break
      }
      
    handleBot(reply);

  }


  // scroll to bottom
  let element;

  // Either afterUpdate()
  afterUpdate(() => {
    // console.log("afterUpdate");
    if (current) scrollToBottom(element);
  });

  $: if (current && element) {
    // console.log("tick");
    scrollToBottom(element);
  }

  const scrollToBottom = async (node) => {
    node.scroll({ top: node.scrollHeight, behavior: "smooth" });
  };
  // end scroll to bottom


  // options widgets handler functions
	function handleIntroAssistance(event) {
		alert(event.detail.text);
	}
	function handleIntroComplaint(event) {
		alert(event.detail.text);
	}
  // options widgets handers

</script>

<div class="flex-1 p:2 sm:p-6 justify-between flex flex-col h-screen">
  <div
    class="flex sm:items-center justify-between py-3 border-b-2 border-gray-200"
  >
    <div class="relative flex items-center space-x-4">
      <div class="relative">
        <span class="absolute text-green-500 right-0 bottom-0">
          <svg width="20" height="20">
            <circle cx="8" cy="8" r="8" fill="currentColor" />
          </svg>
        </span>
        <img
          src="https://images.unsplash.com/photo-1597621804952-2ac4a9a67359?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=facearea&facepad=3&w=144&h=144"
          alt=""
          class="w-10 sm:w-16 h-10 sm:h-16 rounded-full"
        />
      </div>
      <div class="flex flex-col leading-tight">
        <div class="text-2xl mt-1 flex items-center">
          <span class="text-gray-700 mr-3">Jane D.</span>
        </div>
        <span class="text-lg text-gray-600">8888 Virtual Assistant</span>
      </div>
    </div>
    <div class="flex items-center space-x-2">
      <button
        type="button"
        class="inline-flex items-center justify-center rounded-lg border h-10 w-10 transition duration-500 ease-in-out text-gray-500 hover:bg-gray-300 focus:outline-none"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
          class="h-6 w-6"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"
          />
        </svg>
      </button>
      <button
        type="button"
        class="inline-flex items-center justify-center rounded-lg border h-10 w-10 transition duration-500 ease-in-out text-gray-500 hover:bg-gray-300 focus:outline-none"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
          class="h-6 w-6"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z"
          />
        </svg>
      </button>
      <button
        type="button"
        class="inline-flex items-center justify-center rounded-lg border h-10 w-10 transition duration-500 ease-in-out text-gray-500 hover:bg-gray-300 focus:outline-none"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
          class="h-6 w-6"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6.002 6.002 0 00-4-5.659V5a2 2 0 10-4 0v.341C7.67 6.165 6 8.388 6 11v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9"
          />
        </svg>
      </button>
    </div>
  </div>
  <div
    bind:this={element}
    class="flex flex-col space-y-4 p-3 overflow-y-auto scrollbar-thumb-blue scrollbar-thumb-rounded scrollbar-track-blue-lighter scrollbar-w-2 scrolling-touch"
  >
    {#each current as { text, who, ready, isolateDelay, widget }}
      <ChatBox {who} {text} {ready} {isolateDelay} {widget} >
        <div slot="widgets">
          {#if widget}
            {#if widget === 'intro'}
            <Intro on:introassistance={handleIntroAssistance} on:introcomplaint={handleIntroComplaint} />
            {/if}
          {/if}
        </div>
      </ChatBox>
    {/each}
  </div>
  <div class="border-t-2 border-gray-200 px-4 pt-4 mb-2 sm:mb-0">
    <div class="relative flex">
      <!-- mic button -->
      <!-- <span class="absolute inset-y-0 flex items-center">
             <button type="button" class="inline-flex items-center justify-center rounded-full h-12 w-12 transition duration-500 ease-in-out text-gray-500 hover:bg-gray-300 focus:outline-none">
                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" class="h-6 w-6 text-gray-600">
                   <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 11a7 7 0 01-7 7m0 0a7 7 0 01-7-7m7 7v4m0 0H8m4 0h4m-4-8a3 3 0 01-3-3V5a3 3 0 116 0v6a3 3 0 01-3 3z"></path>
                </svg>
             </button>
          </span> -->
      <!-- file attach big button -->
      <span class="absolute inset-y-0 flex items-center">
        <button type="button" class="inline-flex items-center justify-center rounded-full h-12 w-12 transition duration-500 ease-in-out text-gray-500 hover:bg-gray-300 focus:outline-none">
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" class="h-6 w-6 text-gray-600">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.172 7l-6.586 6.586a2 2 0 102.828 2.828l6.414-6.586a4 4 0 00-5.656-5.656l-6.415 6.585a6 6 0 108.486 8.486L20.5 13"></path>
          </svg>
        </button>
      </span>
      <!-- camera button - before -->
      <!-- <span class="absolute inset-y-0 flex items-center">
        <button
          type="button"
          class="inline-flex items-center justify-center rounded-full h-12 w-12 transition duration-500 ease-in-out text-gray-500 hover:bg-gray-300 focus:outline-none"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
            class="h-6 w-6 text-gray-600"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M3 9a2 2 0 012-2h.93a2 2 0 001.664-.89l.812-1.22A2 2 0 0110.07 4h3.86a2 2 0 011.664.89l.812 1.22A2 2 0 0018.07 7H19a2 2 0 012 2v9a2 2 0 01-2 2H5a2 2 0 01-2-2V9z"
            />
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M15 13a3 3 0 11-6 0 3 3 0 016 0z"
            />
          </svg>
        </button>
      </span> -->
      <!-- <input
        type="text"
        placeholder="Write your message!"
        class="w-full focus:outline-none focus:placeholder-gray-400 text-gray-600 placeholder-gray-600 pl-12 bg-gray-200 rounded-md py-3"
      /> -->
      <input
        disabled={lockinput}
        on:keydown={handleKeydown}
        bind:value={userchat}
        bind:this={userchatInput}
        type="text"
        placeholder="Write your message!"
        class="w-full focus:outline-none focus:placeholder-gray-400 text-gray-600 placeholder-gray-600 pl-12 pr-28 bg-gray-200 rounded-md py-3"
      />
      <!-- <div class="absolute right-0 items-center inset-y-0 hidden sm:flex"> -->
      <div class="absolute right-0 items-center inset-y-0 sm:flex">
        <!-- file attach button -->
        <!-- <button type="button" class="inline-flex items-center justify-center rounded-full h-10 w-10 transition duration-500 ease-in-out text-gray-500 hover:bg-gray-300 focus:outline-none">
                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" class="h-6 w-6 text-gray-600">
                   <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.172 7l-6.586 6.586a2 2 0 102.828 2.828l6.414-6.586a4 4 0 00-5.656-5.656l-6.415 6.585a6 6 0 108.486 8.486L20.5 13"></path>
                </svg>
             </button> -->
        <!-- camera button -->
        <!-- <button type="button" class="inline-flex items-center justify-center rounded-full h-10 w-10 transition duration-500 ease-in-out text-gray-500 hover:bg-gray-300 focus:outline-none">
                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" class="h-6 w-6 text-gray-600">
                   <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 9a2 2 0 012-2h.93a2 2 0 001.664-.89l.812-1.22A2 2 0 0110.07 4h3.86a2 2 0 011.664.89l.812 1.22A2 2 0 0018.07 7H19a2 2 0 012 2v9a2 2 0 01-2 2H5a2 2 0 01-2-2V9z"></path>
                   <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 13a3 3 0 11-6 0 3 3 0 016 0z"></path>
                </svg>
             </button> -->
        <!-- emojis button -->
        <!-- <button type="button" class="inline-flex items-center justify-center rounded-full h-10 w-10 transition duration-500 ease-in-out text-gray-500 hover:bg-gray-300 focus:outline-none">
                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" class="h-6 w-6 text-gray-600">
                   <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14.828 14.828a4 4 0 01-5.656 0M9 10h.01M15 10h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path>
                </svg>
             </button>              -->
        <button
          on:click={handleUser}
          type="button"
          class="inline-flex items-center justify-center rounded-lg px-4 py-3 transition duration-500 ease-in-out text-white bg-blue-500 hover:bg-blue-400 focus:outline-none"
        >
          <span class="font-bold">Send</span>
          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 20 20"
            fill="currentColor"
            class="h-6 w-6 ml-2 transform rotate-90"
          >
            <path
              d="M10.894 2.553a1 1 0 00-1.788 0l-7 14a1 1 0 001.169 1.409l5-1.429A1 1 0 009 15.571V11a1 1 0 112 0v4.571a1 1 0 00.725.962l5 1.428a1 1 0 001.17-1.408l-7-14z"
            />
          </svg>
        </button>
      </div>
    </div>
  </div>
</div>
<!--
<div class="flex flex-col">
  <Home />
</div>
-->
<style>
  .scrollbar-w-2::-webkit-scrollbar {
    width: 0.25rem;
    height: 0.25rem;
  }

  .scrollbar-track-blue-lighter::-webkit-scrollbar-track {
    --bg-opacity: 1;
    background-color: #f7fafc;
    background-color: rgba(247, 250, 252, var(--bg-opacity));
  }

  .scrollbar-thumb-blue::-webkit-scrollbar-thumb {
    --bg-opacity: 1;
    background-color: #edf2f7;
    background-color: rgba(237, 242, 247, var(--bg-opacity));
  }

  .scrollbar-thumb-rounded::-webkit-scrollbar-thumb {
    border-radius: 0.25rem;
  }
</style>
