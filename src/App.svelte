<script>
    import Surname from "./Surname.svelte";
    import UserInfo from "./UserInfo.svelte";

    import { slide, fade, blur } from "svelte/transition";

	// export let name;
	let name = "John";

	let displayInfoMessage = false;

    let count = 0;
    // let squared;
    $: squared = count ** 2;
    $: squaredMinusOne = squared - 1;
    function handleClick() {
        count += 1;
    }

    let cities = [
        {
            id: "3287",
            name: "London"
        },
        {
            id: "3914",
            name: "Warsaw"
        },
        {
            id: "8833",
            name: "Berlin"
        },
        {
            id: "3231",
            name: "Lisbon"
        }
    ];

///////////////////////////////////////////////
    async function getRandomStarWarsCharacter() {
        const randomNumber = Math.floor(Math.random() * 10) + 1;
        const apiResponse = await fetch(
                `https://swapi.co/api/people/${randomNumber}/`
        );

        return await apiResponse.json();
    }

    // let character;
    // getRandomStarWarsCharacter().then(value => (character = value));
    let promise = getRandomStarWarsCharacter();

</script>

<style>
	.headerText {
		color: powderblue;
        font-family: "Comic Sans MS";
	}
</style>

<!--<h1>{!character ? 'Loading...' : character.name}</h1>-->
<!--{#await promise then character}-->
{#await promise}
    <h1>Loading...</h1>
{:then character}
    <h1>{character.name}</h1>
{/await}


<!-------------------------------------------------------------------->

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

<button on:click={handleClick}>Counter: {count}</button>
<h1>Sqaure of the counter is {squared}</h1>
<h1>Sqaure of the counter minus one is {squaredMinusOne}</h1>

<h1>European cities:</h1>
<!--<ui>-->
<!--    {#each cities as city, index}-->
<!--        <li>{index + 1}: {city.name}</li>-->
<!--    {/each}-->
<!--</ui>-->
<ui>
    {#each cities as {id, name}, index}
        <li>{index + 1}: {name} (id: {id})</li>
    {/each}
</ui>
