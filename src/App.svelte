<script>
	import TextInput from "./UI/TextInput.svelte";
	import Tag from "./UI/Tag.svelte";
	import Button from "./UI/Button.svelte"
	import HashtagList from "./UI/HashtagList.svelte"
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
		console.log("fetching "+keyword);
		// Fetch data from API
		let url = "https://unsplash-hashtags.ferminrp.workers.dev/corsproxy/?apiurl=https://unsplash.com/nautocomplete/" + keyword;
		console.log(url);
		let response = await fetch(url);
		let data = await response.json();
		for (let i = 0; i < await data["autocomplete"].length; i++) {
			let hashtag = await data["autocomplete"][i]["query"];
			hashtags = [hashtag, ...hashtags];
		}
		console.log(await data);
	}

	// Hashtags generator
	async function generator() {
		console.log("Generator starting!")
		for (let i = 0; i < keywords.length; i++) {
			let keyword = keywords[i];
			hashtags = [keyword, ...hashtags];
			console.log("fetching "+keyword);
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

<main>
	<h1>Welcome!</h1>
	<p>This tool will help you create tags for your unsplash pics. </p>

	<TextInput on:save={newKeyword}>Please submit a few keywords separated by commas:</TextInput>

	{#each keywords as keyword}
		<Tag value={keyword} on:delete={deleteKeyword} />
	{/each}

	<h3>Once you are ready, hit the button!</h3>
	<Button on:click="{generator}">Generate!</Button>

	
	<HashtagList on:delete={deleteHashtag} {hashtags}></HashtagList>

	{#if hashtags.length > 0}
	<Button on:click="{clipboarder}">Copy to clipboard</Button>
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