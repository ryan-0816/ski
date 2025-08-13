<svelte:head>
	<title>Events</title>
	<meta name="description" content="Chinese Conversation Table Events" />
	<meta name="viewport" content="width=device-width, initial-scale=1.0" />
</svelte:head>

<div class="text-column">
	<h1>Events</h1>
	<div class="calendar-container">
		<iframe 
			src="https://calendar.google.com/calendar/embed?src=c_6ac1852bd4d4f0dda436ea5287d982ec447e70bdb49a2bb3c3764c7012cb1b63%40group.calendar.google.com&ctz=America%2FNew_York" 
			style="border: 0" 
			width="100%" 
			height="600" 
			frameborder="0" 
			scrolling="no"
			title="Chinese Conversation Table Events Calendar"
		></iframe>
	</div>
</div>

<style>
	.text-column {
		max-width: 1000px;
		margin: 0 auto;
		padding: 1rem;
	}

	h1 {
		text-align: center;
		margin-bottom: 1.5rem;
	}

	.calendar-container {
		position: relative;
		overflow: hidden;
		width: 100%;
		padding-top: 75%; /* 4:3 Aspect Ratio */
		border-radius: 8px;
		box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
	}

	iframe {
		position: absolute;
		top: 0;
		left: 0;
		bottom: 0;
		right: 0;
		width: 100%;
		height: 100%;
		border: none;
	}

	@media (min-width: 768px) {
		.text-column {
			padding: 2rem;
		}

		.calendar-container {
			padding-top: 60%; /* Adjust aspect ratio for desktop */
		}
	}
</style>