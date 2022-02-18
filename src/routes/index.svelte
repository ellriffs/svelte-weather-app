<script>
	const apiKey = import.meta.env.VITE_API_KEY;
	import axios from 'axios';
	let loading = false;
	let city;
	let temp;
	let feelsLike;
	let humidity;
	let desc;
	let icon;

	function fetchWeather() {
		loading = true;
		axios
			.get(`https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}`)
			.then((res) => {
				console.log(res.data);
				city = res.data.name;
				temp = res.data.main.temp;
				feelsLike = res.data.main.feels_like;
				desc = res.data.weather[0].description;
				icon = res.data.weather[0].icon;
				humidity = res.data.main.humidity;
			})
			.catch((err) => {
				console.log(err);
			});
		console.log(city);
	}
</script>

<main>
	<div class="app">
		<form on:submit|preventDefault={fetchWeather}>
			<input bind:value={city} type="text" />
			<button>submit</button>
		</form>
		{#if loading === !false}
			<ul class="weather-container">
				<li>{city}</li>
				<li>{temp} &deg</li>
				<li>{desc}</li>
				<li>{feelsLike}</li>
				<li>{humidity} %</li>
				<img src={`http://openweathermap.org/img/wn/${icon}@4x.png`} alt="weather_icon" />
			</ul>
		{:else}
			<pre />
		{/if}
	</div>
</main>

<style>
	:global(body) {
		margin: 0px;
	}
	.app {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		height: fit-content;
		width: 100vw;
		background: cyan;
	}

	.weather-container {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		height: 100vh;
		list-style: none;
	}

	ul {
		height: fit-content;
		margin: 0px;
		padding: 0px;
	}
	li {
		font-size: 2rem;
		text-align: center;
		width: 100%;
		margin: 5px;
		font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial,
			sans-serif;
	}
</style>
