<script>
  import TextInput from "./UI/TextInput.svelte";
  import Tag from "./UI/Tag.svelte";
  import Button from "./UI/Button.svelte";
  import HashtagList from "./UI/HashtagList.svelte";
  // Manejo de blacklist de emails
  let keywords = [];
  let hashtags = [];
  function newKeyword(event) {
    // check if keywords contains event.detail
    // if not, add it
    if (keywords.indexOf(event.detail) === -1) {
      keywords = keywords.concat(event.detail);
    }
  }
  function deleteKeyword(event) {
    // Delete event.detail from keywords
    keywords = keywords.filter(function (item) {
      return item !== event.detail;
    });
  }

  async function apiFetcher(keyword) {
    console.log("fetching " + keyword);
    // Fetch data from API
    let url =
      "https://unsplash-hashtags.ferminrp.workers.dev/corsproxy/?apiurl=https://unsplash.com/nautocomplete/" +
      keyword;
    console.log(url);
    let response = await fetch(url);
    let data = await response.json();
    for (let i = 0; i < (await data["autocomplete"].length); i++) {
      let hashtag = await data["autocomplete"][i]["query"];
      hashtags = [hashtag, ...hashtags];
    }
    console.log(await data);
  }

  // Hashtags generator
  async function generator() {
    console.log("Generator starting!");
    for (let i = 0; i < keywords.length; i++) {
      let keyword = keywords[i];
      hashtags = [keyword, ...hashtags];
      console.log("fetching " + keyword);
      apiFetcher(keyword);
    }
  }

  function deleteHashtag(event) {
    // Delete event.detail from keywords
    hashtags = hashtags.filter(function (item) {
      return item !== event.detail;
    });
  }

  function clipboarder() {
    // Copy hashtags to clipboard
    let text = hashtags.join(", ");
    let dummy = document.createElement("textarea");
    document.body.appendChild(dummy);
    dummy.value = text;
    dummy.select();
    document.execCommand("copy");
    document.body.removeChild(dummy);
  }
</script>

<a
  data-v-553b04e0=""
  data-v-7c1a213e=""
  href="https://github.com/ferminrp/unsplash-hashtag-generator"
  aria-label="View source on GitHub"
  class="github-corner"
  ><svg
    data-v-553b04e0=""
    width="80"
    height="80"
    viewBox="0 0 250 250"
    aria-hidden="true"
    style="fill: rgb(100, 206, 170); color: rgb(25, 45, 56); position: absolute; top: 0px; border: 0px; right: 0px;"
    ><path
      data-v-553b04e0=""
      d="M0,0 L115,115 L130,115 L142,142 L250,250 L250,0 Z"
    /><path
      data-v-553b04e0=""
      d="M128.3,109.0 C113.8,99.7 119.0,89.6 119.0,89.6 C122.0,82.7 120.5,78.6 120.5,78.6 C119.2,72.0 123.4,76.3 123.4,76.3 C127.3,80.9 125.5,87.3 125.5,87.3 C122.9,97.6 130.6,101.9 134.4,103.2"
      fill="currentColor"
      class="octo-arm"
      style="transform-origin: 130px 106px;"
    /><path
      data-v-553b04e0=""
      d="M115.0,115.0 C114.9,115.1 118.7,116.5 119.8,115.4 L133.7,101.6 C136.9,99.2 139.9,98.4 142.2,98.6 C133.8,88.0 127.5,74.4 143.8,58.0 C148.5,53.4 154.0,51.2 159.7,51.0 C160.3,49.4 163.2,43.6 171.4,40.1 C171.4,40.1 176.1,42.5 178.8,56.2 C183.1,58.6 187.2,61.8 190.9,65.4 C194.5,69.0 197.7,73.2 200.1,77.6 C213.8,80.2 216.3,84.9 216.3,84.9 C212.7,93.1 206.9,96.0 205.4,96.6 C205.1,102.4 203.0,107.8 198.3,112.5 C181.9,128.9 168.3,122.5 157.7,114.1 C157.9,116.9 156.7,120.9 152.7,124.9 L141.0,136.5 C139.8,137.7 141.6,141.9 141.8,141.8 Z"
      fill="currentColor"
      class="octo-body"
    /></svg
  ></a
>

<main>
  <h1>Welcome!</h1>
  <p>This tool will help you create tags for your unsplash pics.</p>

  <TextInput on:save={newKeyword}
    >Please submit a few keywords separated by commas:</TextInput
  >

  {#each keywords as keyword}
    <Tag value={keyword} on:delete={deleteKeyword} />
  {/each}

  <h3>Once you are ready, hit the button!</h3>
  <Button on:click={generator}>Generate!</Button>

  <HashtagList on:delete={deleteHashtag} {hashtags} />

  {#if hashtags.length > 0}
    <Button on:click={clipboarder}>Copy to clipboard</Button>
  {/if}
</main>

<style>
  main {
    width: 720px;
    max-width: 90vw;
    margin: 0 auto;
  }

  h3 {
    margin-top: 2rem;
  }
</style>
