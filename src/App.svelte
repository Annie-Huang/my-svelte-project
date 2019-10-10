<script>
    import Surname from "./Surname.svelte";
    import UserInfo from "./UserInfo.svelte";

    import { slide, fade, blur } from "svelte/transition";

	// export let name;
	let name = "John";

	let displayInfoMessage = false;
</script>

<style>
	.headerText {
		color: powderblue;
        font-family: "Comic Sans MS";
	}
</style>

<h1 class="headerText">My name is {name.toUpperCase()}!</h1>
<Surname />

<UserInfo name={'Tomasz'} surname={'Kowalski'} />
<UserInfo surname={'Kowalski'} />

<form>
    <div class="name-field">
        <label for="name">Name:</label>
        <input type="text" name="name" />
    </div>
    <div class="surname-field">
        <label for="surname">Surname:</label>
        <input type="text" name="surname" />
    </div>
    <label>
        <input type="checkbox" bind:checked={displayInfoMessage} />
        Do you want to give me a million dollars?
    </label>
</form>

{#if displayInfoMessage}
    <h1 transition:slide={{ duration: 5000, delay: 2000 }}>Thank you1!</h1>
    <h1 transition:fade={{ delay: 250, duration: 300 }}>Thank you2!</h1>
    <h1 transition:blur>Thank you3!</h1>
{/if}
