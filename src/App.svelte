<script>
  import { fade } from "svelte/transition";
  // import { watchResize} from "svelte-watch-resize";
  const urls = [
    "https://images.unsplash.com/photo-1619476266550-bc9f04e57952?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8NXx8YmVhcnN8ZW58MHwxfDB8fHww&auto=format&fit=crop&w=500&q=60",
    "https://images.unsplash.com/photo-1592658242534-4fd3f54f468a?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=735&q=80",
    "https://images.unsplash.com/photo-1561127954-65393e6644d1?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8NHx8YmVhcnN8ZW58MHwxfDB8fHww&auto=format&fit=crop&w=500&q=60",
    "https://images.unsplash.com/photo-1576076819613-26f8537ae375?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8Nnx8YmVhcnN8ZW58MHwxfDB8fHww&auto=format&fit=crop&w=500&q=60",
    "https://images.unsplash.com/photo-1595173425119-1c54835c1874?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MTB8fGJlYXJzfGVufDB8MXwwfHx8MA%3D%3D&auto=format&fit=crop&w=500&q=60",
    "https://images.unsplash.com/photo-1581098031793-61531ade4318?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MTZ8fGJlYXJzfGVufDB8MXwwfHx8MA%3D%3D&auto=format&fit=crop&w=500&q=60",
    "https://images.unsplash.com/photo-1569545957151-ad2f428c2242?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MjF8fGJlYXJzfGVufDB8MXwwfHx8MA%3D%3D&auto=format&fit=crop&w=500&q=60",
    "https://images.unsplash.com/photo-1496297679486-1a1607669129?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8OHx8Z3Jpemx5JTIwYmVhciUyMGNhcnRvb258ZW58MHwxfDB8fHww&auto=format&fit=crop&w=500&q=60",
  ];

  const size = 4;
  $: prev = 0;
  $: next = 4;
  $: current = urls.slice(prev, next);

  $: main_ix = 0;

  // $: icon_1 = window.innerWidth <= 425 ? "left" : "up";
  // $: icon_2 = window.innerWidth <= 425 ? "right" : "down";

  const bp = 768

  $: icon_1 = window.innerWidth <= bp ? "left" : "up";
  $: icon_2 = window.innerWidth <= bp ? "right" : "down";

  function change_icon() {
      icon_1 = window.innerWidth <= bp ? "left" : "up";
      icon_2 = window.innerWidth <= bp ? "right" : "down"
      console.log(window.innerWidth);
  }

  function go_back() {
    if (prev === 0) {
      return -1;
    }
    prev--;
    next--;
  }

  function go_next() {
    if (next >= urls.length) {
      console.log(next);
      return -1;
    }
    prev++;
    next++;
  }
</script>

<svelte:window on:resize={change_icon}></svelte:window>

<link
  rel="stylesheet"
  href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@48,400,0,0"
/>

<h1>Caroussel</h1>
<div class="outer">
  <div class="caroussel">
    <div class="main-img">
      <img
        class="central-img"
        src={current[main_ix]}
        out:fade={{ duration: 300 }}
        alt=""
      />
    </div>
    <div class="reel">
      <button on:click={go_back}
        ><span class="material-symbols-outlined"> keyboard_arrow_{icon_1} </span></button
      >
      <section class="reel-items">
        {#each current as url, i}
          {#if main_ix === i}
            <div class="reel-item selected">
              <img
                src={url}
                alt=""
                class="reel-img"
                on:click={() => (main_ix = i)}
              />
            </div>
          {:else}
            <div class="reel-item">
              <img
                src={url}
                alt=""
                class="reel-img"
                on:click={() => (main_ix = i)}
              />
            </div>
          {/if}
        {/each}
        <!-- <div class="reel-item">
			<img src={current[0]} alt="" class="reel-img">
		</div> -->
      </section>
      <button on:click={go_next}>
        <span class="material-symbols-outlined"> keyboard_arrow_{icon_2} </span>
      </button>
    </div>
  </div>
</div>

<style>

  :root {
    --s : 30vw
  }
  * {
    margin: 0%;
    box-sizing: border-box;
  }

  .caroussel {
    display: flex;
    flex-direction: row-reverse;
    align-items: center;
  }
  .outer {
    display: flex;
    height: 100%;
    justify-content: center;
    align-items: center;
  }
  .main-img {
    width: var(--s);
    height: var(--s);
    overflow: hidden;
    border-radius: 25px;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .central-img {
    width: 100%;
    transition: opacity 0.3s ease-in-out;
  }

  .reel {
    display: flex;
    flex-direction: column;
    margin: 1vh 1vw;
    /* width: 300px; */
    height: var(--s);
    justify-content: center;
    align-items: center;
  }

  .reel-items {
    /* width: 100%; */
    display: flex;
    flex-direction: column;
    height: 100%;
    width: 5vw;
  }

  .reel-item {
    flex: 0.25;
    border: 1px solid black;
    border-radius: 25%;
    /* margin: 1% 0%; */
    transition: border-color 500ms ease-in-out;
    /* width: 25%; */
    /* width: 5vw; */
    /* height: 5vh; */
    overflow: hidden;
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 10%;
  }

  .selected {
    border: 4px solid rgb(255, 196, 0);
  }

  .reel-img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  .reel-item:hover {
    border-color: rgb(255, 196, 0);
  }

  button {
    /* width: 2vw; */
    width: 100%;
    background: none;
    border: none;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  button:hover {
    background-color: rgb(218, 218, 218);
  }

  @media (max-width: 768px) {

    :root {
      --s: 50vw;
    }
    .caroussel {
      flex-direction: column;
    }

    .main-img {
      width: var(--s);
      height: var(--s);
    }

    .reel, .reel-items {
      flex-direction: row;
    }

    .reel {
      width: 70vw;
      height: auto;
    }

    .reel-items {
      width: auto;
      height: auto;
    }

    .reel-item {
      margin: 0% 1%;
    }

  }
</style>
