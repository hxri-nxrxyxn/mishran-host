<script>
    import { onDestroy } from "svelte"; // imports the onDestroy lifecycle helper from Svelter.
    import { Connect } from "vite";
    let serverAddress = ""; //variable for holding server IP and port
    let status = "Enter server address to connect."; //message to show to user
    let clients = []; //hold the list of connected clients received from server
    let socket = null; //websocket connection object ; null until a connection is created
    let isConnected = false; //flag to show whether connected or not
    let isConnecting = false; //flag to show whether connection is in progress currently or not

    const initiateConnection = () => {
        //Checks if user typed a serveraddress : if not shows alert , if yes calls connect() with that address
        if (!serverAddress.trim()) {
            alert("Please provide a server IP address and port");
            return;
        }
        connect(serverAddress.trim()); //starts real connection process
    };

    const connect = (address) => {
        //accepts address parameter which should be a string like 127.0.0.1:8000
        status = "Connecting to ${address}..."; //updates status message
        isConnecting = true; //sets connecting flag to true so UI can show a loading state
        socket = new WebSocket("ws://${address}"); //creates a new websocket object and assigns it to socket
        //ws is a protocol for websocket connections ,it opens a two way pipebetween browser and server. Once opened client and server can send messages to each other at any time without reloading
    };
    socket.onopen = () => {
        //if connection succeeds show connected set flags
        status = "✅ Connected to server";
        isConnected = true;
        isConnecting = false;
    };
    socket.onmessage = (event) => {
        //when a message is received from server this event handler is called
        const data = JSON.parse(event.data); //parses the JSON string received into a JS object
        if (data.type === "state_update") {
            clients = data.clients; //if its a state update message, update the clients array with the new list of connected clients
        }
    };
    socket.onclose = () => {
        status = "❌ Disconnected. Please reconnect"; //if connection closes update status and flags
        resetControls(); //clear connection-related flags (sets isConnected and isConnecting to false)
    };
    socket.onerror = (err) => {
        //callback for websocket errors
        console.error("Websocket error:;", err);
        status =
            "Error connecting to server. Check the addredd and ensure the server is running";
        resetControls(); //on error also reset controls
    };
    const resetControls = () => {
        //resets UI state related to connection
        isConnected = false;
        isConnecting = false;
    };
</script>
