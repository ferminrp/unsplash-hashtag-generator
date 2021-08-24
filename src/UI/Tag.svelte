<script>
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();
    export let value = "";
    function eliminar() {
        dispatch('delete', value);
    }
</script>

<div>
    <span>{value}</span>
    <span on:click={eliminar} class="material-icons-outlined"> cancel </span>
</div>

<style>
    div {
        background-color: azure;
        display: inline;
        box-sizing: border-box;
        padding: 10px;
        margin: 10px 10px 0 0;
    }
    .material-icons-outlined {
        font-family: "Material Icons";
        font-family: "Material Icons";
        font-weight: normal;
        font-style: normal;
        font-size: 1rem; /* Preferred icon size */
        display: inline-block;
        line-height: 1rem;
        text-transform: none;
        letter-spacing: normal;
        word-wrap: normal;
        white-space: nowrap;
        direction: ltr;
        cursor: pointer;
    }
</style>