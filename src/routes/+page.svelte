<script>
	import { onDestroy } from 'svelte';
	import CompactDisc from './cd.svelte';

	// UI State - using $state runes
	let page = $state(0); // 0: Address, 1: Home, 2: Recording
	let serverAddressDisplay = $state('IP ADDRESS');
	
	// WebSocket State
	let serverAddress = $state('');
	let status = $state('Enter server address to connect.');
	let clients = $state([]);
	let socket = $state(null);
	let isConnected = $state(false);
	let isConnecting = $state(false);

	// Auto-redirect to address page when disconnected
	$effect(() => {
		if (!isConnected && !isConnecting && page !== 0) {
			page = 0;
		}
	});

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

	const initiateConnection = () => {
		const address = serverAddressDisplay.trim();
		
		if (!address || address === 'IP ADDRESS') {
			alert('Please provide a server IP address');
			return;
		}

		serverAddress = address.includes(':') ? address : `${address}:8000`;
		connect(serverAddress);
	};

	const connect = (address) => {
		status = `Connecting to ${address}...`;
		isConnecting = true;
		socket = new WebSocket(`ws://${address}/host_monitor`);

		socket.onopen = () => {
			status = '✅ Connected to server';
			isConnected = true;
			isConnecting = false;
			page = 1;
		};

		socket.onmessage = (event) => {
			const data = JSON.parse(event.data);
			if (data.type === 'state_update') {
				clients = data.clients;
			}
		};

		socket.onclose = () => {
			status = '❌ Disconnected. Please reconnect';
			resetControls();
		};

		socket.onerror = (err) => {
			console.error('WebSocket error:', err);
			status = 'Error connecting. Check the address and ensure the server is running';
			resetControls();
		};
	};

	const resetControls = () => {
		isConnected = false;
		isConnecting = false;
		clients = [];
	};

	const sendCommand = (command) => {
		if (socket?.readyState === WebSocket.OPEN) {
			socket.send(JSON.stringify({ command }));
		}
	};

	const startRecording = () => {
		sendCommand('start_all');
		page = 2;
	};

	const stopRecording = () => {
		sendCommand('stop_all');
		page = 1;
	};

	onDestroy(() => {
		socket?.close();
	});

	// Keypad numbers for cleaner template
	const keypadRows = [
		['1', '2', '3'],
		['4', '5', '6'],
		['7', '8', '9'],
		['.', '0', 'C']
	];
</script>

<main class:recording-active={page === 2}>
	<CompactDisc />

	{#if page === 0}
		<div class="page-container">
			<div class="placeholder"></div>
			<div class="contents">
				<h1>{serverAddressDisplay}</h1>
				<div class="matrix">
					{#each keypadRows as row}
						<div class="matrix__row">
							{#each row as key}
								<div class="matrix__column" onclick={() => handleKeyPress(key)}>
									<p>{key}</p>
								</div>
							{/each}
						</div>
					{/each}
					<div class="matrix__row">
						<button
							class="matrix__column connect-btn"
							onclick={initiateConnection}
							disabled={isConnecting}
						>
							<p>N</p>
						</button>
					</div>
				</div>
			</div>
		</div>
	{:else if page === 1}
		<div class="page-container">
			<div class="placeholder"></div>
			<div class="content">
				<h1>CONNECTED</h1>
				<div class="content__devices">
					{#each Array(4) as _, i}
						<img 
							src="/tablet-android.svg" 
							class:inactive={clients.length <= i} 
							alt="Device {i + 1}" 
						/>
					{/each}
				</div>
			</div>
			<div class="placeholder">
				<div class="placeholder__action">
					<button class="btn--white" onclick={startRecording}>RECORD</button>
				</div>
			</div>
		</div>
	{:else if page === 2}
		<div class="page-container">
			<div class="placeholder"></div>
			<div class="content">
				<h1>RECORDING</h1>
			</div>
			<div class="placeholder">
				<div class="placeholder__action">
					<button class="btn--outline" onclick={stopRecording}>STOP</button>
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
		display: flex;
		flex-direction: column;
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
	
	.content h1,
	.contents h1 {
		text-align: center;
		font-size: 1rem;
		font-weight: 700;
	}
	
	.placeholder:last-child {
		display: flex;
		align-items: flex-end;
	}
	
	.placeholder__action {
		margin: 1rem;
		width: 100%;
	}
	
	/* Address Page Styles */
	.contents {
		margin-inline: 2rem;
	}
	
	.contents h1 {
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
	
	.matrix__column {
		padding: 1rem;
		cursor: pointer;
		user-select: none;
	}
	
	.matrix__column p {
		font-weight: 700;
		color: #666;
		font-size: 1.25rem;
	}
	
	.connect-btn {
		background: none;
		border: none;
		padding: 1rem;
		cursor: pointer;
		user-select: none;
	}
	
	.connect-btn p {
		color: white;
	}
	
	.connect-btn:disabled p {
		color: #555;
		cursor: not-allowed;
	}
	
	/* Home Page Styles */
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
</style>