<script>
	import CompactDisc from './cd.svelte';
	// 0 = Addr view, 1 = Home view, 2 = Recording view
	let viewState = 0;

	// import { onDestoroy } from "svelte"; // imports the onDestroy lifecycle helper from Svelter.
	// let serverAddress = ""; //variable for holding server IP and port
	// let status = "Enter server address to connect."; //message to show to user
	// let clients = []; //hold the list of connected clients received from server
	// let socket = null; //websocket connection object ; null until a connection is created
	// let isConnected = false; //flag to show whether connected or not
	// let isConnecting = false; //flag to show whether connection is in progress currently or not

	// const initiateConnection = () => {
	//     //Checks if user typed a serveraddress : if not shows alert , if yes calls connect() with that address
	//     //if (!serverAddress.trim()) {
	//     //    alert("Please provide a server IP address and port");
	//     //    return;
	//     //}
	//     //connect(serverAddress.trim()); //starts real connection process
	// };

	// const connect = (address) => {
	//     //accepts address parameter which should be a string like 127.0.0.1:8000
	//     //status = "Connecting to ${address}..."; //updates status message
	//     //isConnecting = true; //sets connecting flag to true so UI can show a loading state
	//     //socket = new WebSocket("ws://${address}/host_monitor"); //creates a new websocket object and assigns it to socket
	//     //ws is a protocol for websocket connections ,it opens a two way pipebetween browser and server. Once opened client and server can send messages to each other at any time without reloading
	// };
	// socket.onopen = () => {
	//     //if connection succeeds show connected set flags
	//     //status = "✅ Connected to server";
	//     //isConnected = true;
	//     //isConnecting = false;
	// };
	// socket.onmessage = (event) => {
	//     //when a message is received from server this event handler is called
	//     //const data = JSON.parse(event.data); //parses the JSON string received into a JS object
	//     //if (data.type === "state_update") {
	//     //    clients = data.clients; //if its a state update message, update the clients array with the new list of connected clients
	//     //}
	// };
	// socket.onclose = () => {
	//     //status = "❌ Disconnected. Please reconnect"; //if connection closes update status and flags
	//     //resetControls(); //clear connection-related flags (sets isConnected and isConnecting to false)
	// };
	// socket.onerror = (err) => {
	//     //callback for websocket errors
	//     //console.error("Websocket error:", err);
	//     //status =
	//     //    "Error connecting to server. Check the address and ensure the server is running";
	//     //resetControls(); //on error also reset controls
	// };
	// const resetControls = () => {
	//     //resets UI state related to connection
	//     //isConnected = false;
	//     //isConnecting = false;
	// };
	// const startAll = () => {
	//     // send a command to server to start recording on all clients
	//     //if (socket && socket.readyState === WebSocket.OPEN) {
	//     //    socket.send(JSON.stringify({ command: "start_all" }));
	//     //}
	//     //checks if socket exists and checks if the connection is open and ready to send data
	// };
	// const stopAll = () => {
	//     //if (socket && socket.readyState === WebSocket.OPEN) {
	//     //    socket.send(JSON.stringify({ command: "stop_all" }));
	//     //}
	// };
	// onDestroy(() => {
	//     //if (socket) {
	//     //    socket.close(); //closes the websocket connection when the component is destroyed (user navigates away)
	//     //}
	// });
</script>

<main class:recording="{viewState === 2}">
	{#if viewState === 0}
		<!-- ADDR VIEW -->
		<div class="placeholder"></div>
		<div class="contents">
			<h1>IP ADDRESS</h1>
			<div class="matrix">
				<div class="matrix__row">
					<div class="matrix__column"><p>1</p></div>
					<div class="matrix__column"><p>2</p></div>
					<div class="matrix__column"><p>3</p></div>
				</div>
				<div class="matrix__row">
					<div class="matrix__column"><p>4</p></div>
					<div class="matrix__column"><p>5</p></div>
					<div class="matrix__column"><p>6</p></div>
				</div>
				<div class="matrix__row">
					<div class="matrix__column"><p>7</p></div>
					<div class="matrix__column"><p>8</p></div>
					<div class="matrix__column"><p>9</p></div>
				</div>
				<div class="matrix__row">
					<div class="matrix__column"><p>.</p></div>
					<div class="matrix__column"><p>0</p></div>
					<div class="matrix__column"><p>C</p></div>
				</div>
				<div class="matrix__row">
					<div class="matrix__column" on:click={() => (viewState = 1)}>
						<p>N</p>
					</div>
				</div>
			</div>
		</div>
	{:else if viewState === 1}
		<!-- HOME VIEW -->
		<div class="placeholder"></div>
		<div class="content">
			<h1>CONNECTED</h1>
			<div class="content__devices">
				<img src="/tablet-android.svg" class="inactive" alt="device" />
				<img src="/tablet-android.svg" class="inactive" alt="device" />
				<img src="/tablet-android.svg" class="inactive" alt="device" />
				<img src="/tablet-android.svg" class="inactive" alt="device" />
			</div>
		</div>
		<div class="placeholder">
			<div class="placeholder__action">
				<button class="btn--white" on:click={() => (viewState = 2)}>RECORD</button>
			</div>
		</div>
	{:else if viewState === 2}
		<!-- RECORDING VIEW -->
		<div class="placeholder"></div>
		<div class="content">
			<h1>RECORDING</h1>
		</div>
		<div class="placeholder">
			<div class="placeholder__action">
				<button class="btn--outline" on:click={() => (viewState = 1)}>STOP</button>
			</div>
		</div>
	{/if}
</main>

<style>
	main {
		background: black;
		height: 100vh;
		color: white;
		display: flex;
		flex-direction: column;
	}

	main.recording {
		background: #ff3434;
	}

	.placeholder {
		flex: 1;
	}

	.placeholder:last-child {
		display: flex;
		align-items: flex-end;
	}

	.placeholder__action {
		margin: 1rem;
		width: 100%;
	}

	/* ADDR VIEW STYLES */
	.contents {
		margin-inline: 2rem;
	}
	.contents h1 {
		text-align: center;
		font-size: 1rem;
		font-weight: 700;
		margin-bottom: 1rem;
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
	}
	.matrix__column p {
		font-weight: 700;
		color: #666;
		font-size: 1.25rem;
	}

	/* HOME & RECORDING VIEW STYLES */
	.content h1 {
		text-align: center;
		font-size: 1rem;
		font-weight: 700;
	}
	.content__devices {
		display: flex;
		justify-content: center;
		align-items: center;
		margin-top: 1rem;
	}
	.content__devices img {
		margin-inline: 0.1rem;
		height: 1.5rem;
		filter: brightness(0) saturate(100%) invert(100%) sepia(6%) saturate(144%) hue-rotate(230deg)
			brightness(119%) contrast(100%);
	}
	.content__devices img.inactive {
		filter: brightness(0) saturate(100%) invert(100%) sepia(6%) saturate(144%)
			hue-rotate(230deg) brightness(119%) contrast(100%) opacity(0.4);
	}

	/* BUTTONS - you might need to add these styles */
	.btn--white {
		/* Add styles for white button */
	}
	.btn--outline {
		/* Add styles for outline button */
	}
</style>

