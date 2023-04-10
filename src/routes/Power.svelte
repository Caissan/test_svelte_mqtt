<script lang="ts">
	import * as mqtt from "mqtt";
	import type { IClientOptions } from "mqtt";
	import { onDestroy } from "svelte";
	import { browser } from "$app/environment";

	const clientId = `test_power_${Math.random().toString(16).substring(2, 8)}`;
	const connectTimeout = 30 * 1000;
	const reconnectPeriod = 4 * 1000;
	const protocol = 'ws';
	const host = 'test.mosquitto.org';
	const port = browser ? 8081 : 8080;
	const topic = 'test-power-topic';

	const options: IClientOptions = {clientId, connectTimeout, reconnectPeriod};
	let power = 0;

	console.log('Connecting mqtt client');
	const client = mqtt.connect(`${protocol}://${host}:${port}`, options);

	client.on('error', (err) => {
		console.log('Connection error: ', err);
		client.end();
	});

	client.on('reconnect', () => {
		console.log('Reconnecting...');
	});

	client.on('connect', () => {
		client.subscribe(topic, { qos: 0 });
	});

	client.on('message', (topic, message) => {
		power = parseInt(message.toString());
	});

	const onChange = (event) => {
		if (client.connected) client.publish(topic, event.target.value);
	};

	onDestroy(() => {
		client.unsubscribe(topic);
	});
</script>

<div class="flex flex-col items-center gap-7">
	<div class="font-semibold text-4xl">Мощность:</div>
	<div class="flex flex-col items-center gap-3 w-full">
		<div class="text-7xl">{power}%</div>
		<input type="range" class="w-full" bind:value={power} on:change={onChange}>
	</div>
</div>