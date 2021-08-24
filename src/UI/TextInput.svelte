<script>
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();
    export let value = "";
    export let placeholder = "Landscape, portrait";
    function save(event) {
        if (event.key == "Enter" && value.length > 0) {
            dispatch('save', value);
            value = "";
            //console.log("Enter"+value);
        } else if ((event.key == "," || event.code == "Comma" ) && value.length > 1) {
            value = value.replace(',','')
            dispatch('save', value);
            value = "";
        }
    }
</script>

<p><slot><!-- optional fallback --></slot></p>
<input on:keyup={save} type="text" bind:value {placeholder} />

<style>
    input {
        margin-bottom: 1rem;
        width: 100%;
        border: 0px;
        border-bottom: 2px solid #1b91db;
        background-color: #f0f7fa;
        margin-bottom: 1rem;
    }
    input:focus {
        outline-width: 0;
        border: 2px solid #1b91db;
        border-radius: 5px;
    }
    p {
        font-weight: 600;
        margin-top: 2rem;
    }
</style>