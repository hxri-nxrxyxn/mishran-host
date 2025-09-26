<script>
	import CompactDisc from './cd.svelte';

	// Variable to control which page is shown
	// 0: Address Page, 1: Home Page, 2: Recording Page
	let page = 0;

	// This variable will be shown in the h1 tag
	let serverAddressDisplay = 'IP ADDRESS';

	const handleKeyPress = (key) => {
		if (key === 'C') {
			serverAddressDisplay = 'IP ADDRESS';
			return;
		}

		if (serverAddressDisplay === 'IP ADDRESS') {
			serverAddressDisplay = '';
		}

		serverAddressDisplay += key;
	};

	// --- WebSocket Logic Variables ---
	// import { onDestoroy } from "svelte"; // imports the onDestroy lifecycle helper from Svelter.
	let serverAddress = ''; //variable for holding server IP and port
	// let status = "Enter server address to connect."; //message to show to user
	// let clients = []; //hold the list of connected clients received from server
	// let socket = null; //websocket connection object ; null until a connection is created
	// let isConnected = false; //flag to show whether connected or not
	// let isConnecting = false; //flag to show whether connection is in progress currently or not

	// const initiateConnection = () => {
	// 	 //Checks if user typed a serveraddress : if not shows alert , if yes calls connect() with that address
	// 	 if (!serverAddress.trim()) {
	// 		  alert("Please provide a server IP address and port");
	// 		  return;
	// 	 }
	// 	 connect(serverAddress.trim()); //starts real connection process
	// };

	// const connect = (address) => {
	// 	 //accepts address parameter which should be a string like 127.0.0.1:8000
	// 	 status = "Connecting to ${address}..."; //updates status message
	// 	 isConnecting = true; //sets connecting flag to true so UI can show a loading state
	// 	 socket = new WebSocket("ws://${address}/host_monitor"); //creates a new websocket object and assigns it to socket
	// 	 //ws is a protocol for websocket connections ,it opens a two way pipebetween browser and server. Once opened client and server can send messages to each other at any time without reloading
	// };
	// socket.onopen = () => {
	// 	 //if connection succeeds show connected set flags
	// 	 status = "✅ Connected to server";
	// 	 isConnected = true;
	// 	 isConnecting = false;
	// };
	// socket.onmessage = (event) => {
	// 	 //when a message is received from server this event handler is called
	// 	 const data = JSON.parse(event.data); //parses the JSON string received into a JS object
	// 	 if (data.type === "state_update") {
	// 		  clients = data.clients; //if its a state update message, update the clients array with the new list of connected clients
	// 	 }
	// };
	// socket.onclose = () => {
	// 	 status = "❌ Disconnected. Please reconnect"; //if connection closes update status and flags
	// 	 resetControls(); //clear connection-related flags (sets isConnected and isConnecting to false)
	// };
	// socket.onerror = (err) => {
	// 	 //callback for websocket errors
	// 	 console.error("Websocket error:", err);
	// 	 status =
	// 		  "Error connecting to server. Check the address and ensure the server is running";
	// 	 resetControls(); //on error also reset controls
	// };
	// const resetControls = () => {
	// 	 //resets UI state related to connection
	// 	 isConnected = false;
	// 	 isConnecting = false;
	// };
	// const startAll = () => {
	// 	 // send a command to server to start recording on all clients
	// 	 if (socket && socket.readyState === WebSocket.OPEN) {
	// 		  socket.send(JSON.stringify({ command: "start_all" }));
	// 	 }
	// 	 //checks if socket exists and checks if the connection is open and ready to send data
	// };
	// const stopAll = () => {
	// 	 if (socket && socket.readyState === WebSocket.OPEN) {
	// 		  socket.send(JSON.stringify({ command: "stop_all" }));
	// 	 }
	// };
	// onDestroy(() => {
	// 	 if (socket) {
	// 		  socket.close(); //closes the websocket connection when the component is destroyed (user navigates away)
	// 	 }
	// });
</script>

<main class:recording-active={page === 2}>
	<CompactDisc />

	{#if page === 0}
		<div class="page-container">
			<div class="placeholder" />
			<div class="contents">
				<h1>{serverAddressDisplay}</h1>
				<div class="matrix">
					<div class="matrix__row">
						<div class="matrix__column" on:click={() => handleKeyPress('1')}><p>1</p></div>
						<div class="matrix__column" on:click={() => handleKeyPress('2')}><p>2</p></div>
						<div class="matrix__column" on:click={() => handleKeyPress('3')}><p>3</p></div>
					</div>
					<div class="matrix__row">
						<div class="matrix__column" on:click={() => handleKeyPress('4')}><p>4</p></div>
						<div class="matrix__column" on:click={() => handleKeyPress('5')}><p>5</p></div>
						<div class="matrix__column" on:click={() => handleKeyPress('6')}><p>6</p></div>
					</div>
					<div class="matrix__row">
						<div class="matrix__column" on:click={() => handleKeyPress('7')}><p>7</p></div>
						<div class="matrix__column" on:click={() => handleKeyPress('8')}><p>8</p></div>
						<div class="matrix__column" on:click={() => handleKeyPress('9')}><p>9</p></div>
					</div>
					<div class="matrix__row">
						<div class="matrix__column" on:click={() => handleKeyPress('.')}><p>.</p></div>
						<div class="matrix__column" on:click={() => handleKeyPress('0')}><p>0</p></div>
						<div class="matrix__column" on:click={() => handleKeyPress('C')}><p>C</p></div>
					</div>
					<div class="matrix__row">
						<div
							class="matrix__column"
							on:click={() => {
								serverAddress = serverAddressDisplay; // Save the value
								page = 1; // Go to the next page
							}}
						>
							<p>N</p>
						</div>
					</div>
				</div>
			</div>
		</div>
	{:else if page === 1}
		<div class="page-container">
			<div class="placeholder" />
			<div class="content">
				<h1>CONNECTED</h1>
				<div class="content__devices">
					<img src="/tablet-android.svg" class="inactive" alt="" />
					<img src="/tablet-android.svg" class="inactive" alt="" />
					<img src="/tablet-android.svg" class="inactive" alt="" />
					<img src="/tablet-android.svg" class="inactive" alt="" />
				</div>
			</div>
			<div class="placeholder">
				<div class="placeholder__action">
					<button class="btn--white" on:click={() => (page = 2)}>RECORD</button>
				</div>
			</div>
		</div>
	{:else if page === 2}
		<div class="page-container">
			<div class="placeholder" />
			<div class="content">
				<h1>RECORDING</h1>
			</div>
			<div class="placeholder">
				<div class="placeholder__action">
					<button class="btn--outline" on:click={() => (page = 1)}>STOP</button>
				</div>
			</div>
		</div>
	{/if}
</main>

<style>
	main {
		background: black;
		height: 100vh;
		color: white;
		position: relative;
		z-index: 1;
		transition: background-color 0.3s ease;
	}

	main.recording-active {
		background: #ff3434;
	}

	.page-container {
		height: 100%;
		display: flex;
		flex-direction: column;
	}

	.placeholder {
		flex: 1;
	}

	/* Common Content Styles */
	.content h1,
	.contents h1 {
		text-align: center;
		font-size: 1rem;
		font-weight: 700;
        /* Added for better display of long IP */
        word-wrap: break-word;
	}

	.placeholder:last-child {
		display: flex;
		align-items: flex-end;
	}
	.placeholder__action {
		margin: 1rem;
		width: 100%;
	}

	/* --- ADDR Page Styles --- */
	.contents {
		margin-inline: 2rem;
	}
	.contents h1 {
		margin-bottom: 1rem;
        min-height: 1.2rem; /* Reserve space for the text */
	}
	.matrix {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		margin: 0 1rem;
	}
	.matrix__row {
		display: flex;
		width: 100%;
		justify-content: space-between;
		margin-block: 1.25rem;
	}
	.matrix__row:last-child {
		justify-content: center;
	}
	.matrix__row:last-child .matrix__column p {
		color: white;
	}
	.matrix__column {
		padding: 1rem;
        cursor: pointer;
        user-select: none; /* Prevents text selection on rapid clicks */
	}
	.matrix__column p {
		font-weight: 700;
		color: #666;
		font-size: 1.25rem;
	}

	/* --- HOME Page Styles --- */
	.content__devices {
		display: flex;
		justify-content: center;
		align-items: center;
		margin-top: 1rem;
	}
	.content__devices img {
		margin-inline: 0.1rem;
		height: 1.5rem;
		filter: brightness(0) saturate(100%) invert(100%) sepia(6%) saturate(144%)
			hue-rotate(230deg) brightness(119%) contrast(100%);
	}
	.content__devices img.inactive {
		filter: brightness(0) saturate(100%) invert(100%) sepia(6%) saturate(144%)
			hue-rotate(230deg) brightness(119%) contrast(100%) opacity(0.4);
	}

	/* You would need to add styles for .btn--white and .btn--outline in a global stylesheet or here */
</style>