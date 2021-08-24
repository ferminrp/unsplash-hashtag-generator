<script>
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();

    import Tag from './Tag.svelte'
    export let hashtags = [];

</script>

{#if hashtags.length > 0}
<section>
    {#each hashtags as hashtag}
    <Tag value={hashtag} on:delete />
    {/each}
</section>
{/if}

<style>
    section {
        display: flex;
        flex-wrap: wrap;
    }
</style>