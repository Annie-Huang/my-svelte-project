<script>
    import {store} from "./store";

    function decrement() {
        // Decrement the value
        store.update(n => n - 1);
    }
</script>

<button on:click={decrement}>-</button>
