<script>
    import Surname from "./Surname.svelte";
    import UserInfo from "./UserInfo.svelte";
    import { store } from "./store";
    import Incrementer from "./Incremementer.svelte";
    import Decrementer from "./Decremementer.svelte";
    import Setter from "./Setter.svelte";

    import { slide, fade, blur } from "svelte/transition";
    import { tweened } from "svelte/motion";
    import { bounceInOut } from "svelte/easing";
    import { onMount, onDestroy } from "svelte";
    import { writable } from "svelte/store";

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

///////////////////////////////////////////////
    let name1;
    function handleClick1() {
        alert("Hello " + name1);
    }
    
    function handleInput1(event) {
        name1 = event.target.value;
    }

///////////////////////////////////////////////

    let characters = [];

    onMount(async () => {
        const apiResponse = await fetch(`https://swapi.co/api/people/`);
        const swPeopleJSON = await apiResponse.json();

        characters = swPeopleJSON.results;

        // Important thing to know about the onmount is that whenever a function passed in as an argument is going to
        // return another function, this function is going to actually get called whenever the component is being destroyed.
        return () => console.log('Destroyed');
    });


///////////////////////////////////////////////
    let countValue;
    // const unsubscribe = store.subscribe(value => (countValue = value));
    // onDestroy(unsubscribe);

///////////////////////////////////////////////
//     const progress = writable(0);
    const progress = tweened(0, {
        duration: 5000,
        easing: bounceInOut
    });
</script>

<style>
	.headerText {
		color: powderblue;
        font-family: "Comic Sans MS";
	}

    progress {
        display: block;
        width: 100%;
    }
</style>

<h1>Installing Windows 95...</h1>
<progress value={$progress} />
<sub>2 weeks remaining...</sub>

<button on:click={() => progress.set(0)}>0%</button>
<button on:click={() => progress.set(0.75)}>75%</button>
<button on:click={() => progress.set(1.0)}>100%</button>

{#if $progress === 1.0}
    <h1>Windows 95 installed, hooray!</h1>
{/if}

<!-------------------------------------------------------------------->

<!--Basically, if you have a $ and the variable name imported from the store, Svelte is going to assume
    that we want to refer to the value stored within the store. Right now, if I increment, decrement,
    and send the counter value, it's going to work as intended with much less boilerplate.
    This value is going to be unsubscribed by default whenever we destroy the component.
    As such, we won't have any memory leaks.-->
<!--<h1>The count is {countValue}</h1>-->
<h1>The count is {$store}</h1>
<Incrementer />
<Decrementer />
<Setter />

<!-------------------------------------------------------------------->

<ul>
    {#each characters as { name, height, birth_year }}
        <li>
            <strong>{name}</strong>
            (height: {height}cm, birth year: {birth_year})
        </li>
    {:else}
        <p>Loading...</p>
    {/each}
</ul>

<!-------------------------------------------------------------------->


<h1>Introduce yourself: </h1>
<input type="text" on:change={handleInput1}/>
<!--Once trigger once for this event handler-->
<button on:click|once={handleClick1}>Click me</button>
<button on:click|preventDefault|once={handleClick1}>Click me</button>

<!-------------------------------------------------------------------->

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
