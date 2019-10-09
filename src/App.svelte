<script>
    import Surname from "./Surname.svelte";

	// export let name;
	let name = "John";
</script>

<style>
	.headerText {
		color: powderblue;
        font-family: "Comic Sans MS";
	}
</style>

<h1 class="headerText">My name is {name.toUpperCase()}!</h1>
<Surname />
