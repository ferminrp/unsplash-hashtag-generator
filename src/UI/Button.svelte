<button on:click><slot></slot></button>

<style>
    button {
        background-color: #F0F7FA;
        border: 2px solid #1b91db;
        border-radius: 5px;
        box-sizing: border-box;
        padding: 0.5rem 1rem;
        margin-top: 2rem;
        cursor: pointer;
    }

    button:hover {
        background-color: #badfef;
    }
</style>