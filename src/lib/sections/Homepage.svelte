<script lang="ts">
	import { box } from '$lib';

	import Button from '$lib/common/Button.svelte';
	import {
		generateId,
		getDataFromString,
		getOrSetPreviousSims,
		loadedData,
		simId,
		companyName,
		founders,
		events,
		exit,
		LOCAL_STORAGE_KEY,
		resetData
	} from '$lib/store';
	import { onMount } from 'svelte';
	import type { Founder, Exit, Event } from '$lib/types';
	import { flip } from 'svelte/animate';
	import { quintOut } from 'svelte/easing';
	import PreviousSimulation from '$lib/common/PreviousSimulation.svelte';

	let previousSims: string[] = [];
	let parsedSims: {
		id: string;
		name: string;
		founders: Founder[];
		events: Event[];
		exit: Exit | null;
	}[] = [];

	onMount(() => {
		previousSims = getOrSetPreviousSims();
		parsedSims = previousSims.map(getDataFromString);
	});

	const deleteSim = (id: number) => {
		previousSims = previousSims.filter((_, sId) => sId !== id);
		parsedSims = previousSims.map(getDataFromString);
		localStorage.setItem(LOCAL_STORAGE_KEY, JSON.stringify(previousSims));
	};
</script>

<div
	out:box={{ duration: 300, scale: 25 }}
	in:box={{ delay: 100, duration: 300, scale: 25 }}
	class="will-change-transform origin-top h-full min-h-screen flex flex-col max-w-[300px] mx-auto absolute w-full left-[50%] -translate-x-[50%] top-0"
>

	<div class="text-xl text-textLight py-10 text-center"
	style ="display: flex;
	padding:10%;
    justify-content: center;
    align-items: center;"
	>
	
	<svg version="1.1" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 604 222"><style type="text/css"> .st0{fill:#bccf0f}</style> <g> <path d="M55,43.4c3.1,0,7.7,0.6,10.3,1l0.2-3.7c-2.5-0.5-6.8-1.3-10.9-1.3c-11.9,0-14.4,7.1-14.4,20.4c0,13,2.6,20.2,14.4,20.2 c3.8,0,8.1-0.6,10.9-1.2l-0.2-3.8c-2.7,0.5-7,1-10.2,1c-9,0-10.4-5.2-10.4-16.3C44.7,48.6,46.2,43.4,55,43.4z"></path> <path d="M78.6,40L67.7,79.5h4.3l2.9-10.2h16.9l2.9,10.2H99L88.1,40H78.6z M75.9,65.4l6-21.6h3l6,21.6H75.9z"></path> <polygon points="122.9,73.4 111.7,40 103.8,40 103.8,79.5 108.2,79.5 108.2,44.7 108.9,44.7 120.4,78.1 125.3,78.1 136.8,44.7 137.6,44.7 137.6,79.5 142,79.5 142,40 134,40  "></polygon> <path d="M164.7,40h-14.4v39.5h4.4V66.2h10c8.6,0,12.6-4.8,12.6-13.5C177.3,44.2,173.3,40,164.7,40z M164.6,62.3h-10V43.9h10 c5.8,0,8.2,2.8,8.2,8.8C172.8,58.8,170.4,62.3,164.6,62.3z"></path> <path d="M205.6,67.6c0,6.2-3.5,8.6-9.6,8.6c-5.8,0-9.2-2.4-9.2-8.6V40h-4.4v27.5c0,9.1,5,12.5,13.6,12.5c9,0,14-3.5,14-12.5V40 h-4.3V67.6z"></path> <path d="M229.9,57.2c-6.9-1.5-9.6-2.4-9.6-7.5c0-4.4,3-6.4,8.6-6.4c3.3,0,11.2,1,11.2,1l0.4-3.7c0,0-7.2-1.1-11.9-1.1 c-7.7,0-12.7,3-12.7,10.6c0,7.5,4.1,9.5,12.3,11.2c6.4,1.3,8.8,2.5,8.8,7c0,5.5-2.9,7.9-8.4,7.9c-4,0-11.8-1-11.8-1l-0.5,3.6 c0,0,7.8,1.3,12.5,1.3c7.8,0,12.6-3.4,12.6-12.1C241.2,60.9,237.7,59,229.9,57.2z"></path> <polygon points="260.1,78.7 268,78.7 268,65 281.8,65 281.8,58.1 268,58.1 268,46.7 284.9,46.7 284.9,39.9 260.1,39.9  "></polygon> <path d="M303.4,39.2c-12.2,0-16.1,7.2-16.1,20.3s3.9,19.8,16.1,19.8s16.1-6.7,16.1-19.8S315.6,39.2,303.4,39.2z M303.4,72.5 c-6.4,0-8-4-8-13c0-9,1.6-13.5,8-13.5s8,4.5,8,13.5C311.3,68.5,309.8,72.5,303.4,72.5z"></path> <path d="M346.1,65.7c0,4.6-2.3,6.8-6.9,6.8c-4.6,0-6.9-2.2-6.9-6.8V39.9h-7.9v25.7c0,9.7,5.1,13.8,14.8,13.8 c9.7,0,14.8-4.1,14.8-13.8V39.9h-7.9V65.7z"></path> <polygon points="383.8,71.8 383.2,71.8 373.9,39.9 360.6,39.9 360.6,78.7 368.5,78.7 368.5,46.7 369,46.7 378.7,78.7 391.7,78.7 391.7,39.9 383.8,39.9  "></polygon> <path d="M411.8,39.9h-13.1v38.8h13.1c13.9,0,16.5-6.9,16.5-20.1C428.2,45.6,425.3,39.9,411.8,39.9z M411.8,71.8h-5.2V46.7h5.2 c7.6,0,8.4,3.3,8.4,11.8C420.1,67.1,419.4,71.8,411.8,71.8z"></path> <polygon points="433.6,78.7 458.7,78.7 458.7,71.8 441.5,71.8 441.5,62.5 455.3,62.5 455.3,55.8 441.5,55.8 441.5,46.7 458.7,46.7 458.7,39.9 433.6,39.9  "></polygon> <path d="M492.5,52.7c0-8.8-4.8-12.8-13.7-12.8h-15.1v38.8h7.9v-13h7.6l4.5,13h8.6l-5.5-14.8C490.4,61.6,492.5,58,492.5,52.7z M479,58.9h-7.3V46.6h7.2c3.8,0,5.6,2.5,5.6,6.2C484.5,56.2,482.9,58.9,479,58.9z"></path> <path d="M511.5,55.5c-6.5-2.1-8-2.9-8-5.4c0-2.6,2-4.1,6.2-4.1C513,46,521,47,521,47l0.6-6.2c0,0-7.6-1.5-12.8-1.5 c-7.9,0-13.2,3.5-13.2,11.5c0,6.2,3.3,9.2,12,11.7c5.6,1.6,7,2.7,7,5.1c0,3-1.8,5-6,5c-4,0-11.7-1-11.7-1l-0.8,6.1 c0,0,7.6,1.8,12.9,1.8c7.6,0,13.4-3.9,13.4-12.4C522.4,60.5,519.9,58.2,511.5,55.5z"></path> </g> <polygon class="st0" points="259.1,118.9 249,106.8 12.4,106.8 12.4,12.5 90.1,12.5 80,0.3 12.4,0.3 12.4,0.3 0.2,0.3 0.2,118.9 "></polygon>
        <path id="white"  d="M42.91,177l0-31.832l19.044,0l0,3.128l-15.502,0l0,12.42l13.11,0l0,3.128l-13.11,0l0,13.156l-3.542,0z M80.032,154l3.45,0l0,23l-3.45,0l0-1.61q-3.45,2.07-6.808,2.07q-4.692,0-6.233-2.438t-1.541-9.016l0-12.006l3.45,0l0,11.96q0,5.014,0.874,6.716t3.956,1.702q1.518,0,3.082-0.437t2.392-0.851l0.828-0.414l0-18.676z M93.556,177l-3.45,0l0-23l3.404,0l0,1.61q3.726-2.07,7.13-2.07q4.6,0,6.187,2.484t1.587,8.832l0,12.144l-3.404,0l0-12.052q0-4.784-0.943-6.555t-4.025-1.771q-1.472,0-3.105,0.437t-2.507,0.851l-0.874,0.414l0,18.676z M132.472,143.972l0,33.028l-3.404,0l0-1.564q-3.542,2.024-6.992,2.024q-1.84,0-3.22-0.46t-2.599-1.679t-1.84-3.634t-0.621-6.003q0-6.21,2.231-9.177t7.383-2.967q2.668,0,5.658,0.598l0-10.166l3.404,0z M122.49,174.378q1.472,0,3.105-0.437t2.553-0.851l0.92-0.414l0-15.594q-2.944-0.552-5.474-0.552q-3.496,0-4.876,2.231t-1.38,6.923q0,2.668,0.414,4.485t1.173,2.691t1.587,1.196t1.978,0.322z M139.142,177l0-23l3.45,0l0,23l-3.45,0z M139.142,148.802l0-4.002l3.45,0l0,4.002l-3.45,0z M152.666,177l-3.45,0l0-23l3.404,0l0,1.61q3.726-2.07,7.13-2.07q4.6,0,6.187,2.484t1.587,8.832l0,12.144l-3.404,0l0-12.052q0-4.784-0.943-6.555t-4.025-1.771q-1.472,0-3.105,0.437t-2.507,0.851l-0.874,0.414l0,18.676z M182.796,187.81q-5.52,0-7.705-1.495t-2.185-5.451q0-1.886,0.828-3.059t2.806-2.645q-1.288-0.874-1.288-3.174q0-0.368,0.391-1.288t0.759-1.61l0.414-0.736q-3.634-1.656-3.634-6.946q0-7.866,8.602-7.866q1.012,0,2.208,0.138t1.886,0.322l0.69,0.138l6.808-0.184l0,2.944l-4.37-0.092q1.518,1.518,1.518,4.6q0,4.324-2.139,6.049t-6.739,1.725q-1.242,0-2.254-0.184q-0.828,2.024-0.828,2.622q0,1.426,0.874,1.771t4.692,0.345q5.014,0,6.946,1.38t1.932,5.382q0,7.314-10.212,7.314z M183.854,176.77l-4.968-0.23q-1.518,1.104-2.047,1.932t-0.529,2.116q0,2.438,1.357,3.381t5.175,0.943q3.496,0,5.083-1.012t1.587-3.266q0-2.484-1.15-3.174t-4.508-0.69z M181.784,166.42q2.898,0,4.094-1.15t1.196-3.864q0-2.76-1.196-3.91t-4.094-1.15q-2.714,0-3.933,1.173t-1.219,3.887t1.219,3.864t3.933,1.15z M226.45,145.582l-0.322,2.99q-6.394-0.782-9.062-0.782q-6.9,0-6.9,5.198q0,2.898,1.587,3.979t6.187,2.093q5.106,1.15,7.13,2.875t2.024,5.681q0,9.798-10.166,9.798q-1.656,0-4.186-0.253t-4.232-0.529l-1.656-0.23l0.368-2.944q6.256,0.828,9.522,0.828q6.808,0,6.808-6.394q0-2.576-1.495-3.703t-5.589-1.909q-5.382-1.15-7.636-2.967t-2.254-6.095q0-8.556,10.258-8.556q1.656,0,4.071,0.23t3.979,0.46z M232.614,177l0-23l3.45,0l0,23l-3.45,0z M232.614,148.802l0-4.002l3.45,0l0,4.002l-3.45,0z M246.138,177l-3.45,0l0-23l3.404,0l0,1.61q3.45-2.07,6.716-2.07q4.278,0,5.98,2.346q0.46-0.276,1.242-0.667t2.944-1.035t4.002-0.644q4.6,0,6.187,2.461t1.587,8.855l0,12.144l-3.45,0l0-12.052q0-4.784-0.92-6.555t-3.91-1.771q-1.518,0-3.128,0.437t-2.484,0.851l-0.874,0.414q0.598,1.518,0.598,6.716l0,11.96l-3.45,0l0-11.868q0-4.968-0.897-6.739t-3.933-1.771q-1.472,0-3.013,0.437t-2.369,0.851l-0.782,0.414l0,18.676z M295.496,154l3.45,0l0,23l-3.45,0l0-1.61q-3.45,2.07-6.808,2.07q-4.692,0-6.233-2.438t-1.541-9.016l0-12.006l3.45,0l0,11.96q0,5.014,0.874,6.716t3.956,1.702q1.518,0,3.082-0.437t2.392-0.851l0.828-0.414l0-18.676z M305.846,177l0-33.028l3.45,0l0,33.028l-3.45,0z M332.434,160.946l0,11.776q0.138,1.702,2.668,2.024l-0.138,2.714q-3.634,0-5.474-1.84q-4.14,1.84-8.28,1.84q-3.174,0-4.83-1.794t-1.656-5.244q0-3.266,1.702-4.853t5.336-1.955l7.222-0.69l0-1.978q0-2.346-1.012-3.358t-3.128-1.012q-1.426,0-3.611,0.138t-3.657,0.322l-1.426,0.138l-0.138-2.622q5.06-1.012,9.016-1.012q3.864,0,5.635,1.794t1.771,5.612z M328.984,165.546l-6.808,0.644q-2.07,0.184-2.99,1.196t-0.92,2.944q0,4.232,3.496,4.232q1.518,0,3.312-0.345t2.852-0.713l1.058-0.368l0-7.59z M351.156,156.99l-7.314,0l0,10.994q0,3.956,0.575,5.198t2.737,1.242l4.094-0.276l0.23,2.852q-3.082,0.506-4.692,0.506q-3.588,0-4.968-1.748t-1.38-6.67l0-12.098l-3.266,0l0-2.99l3.266,0l0-7.038l3.404,0l0,7.038l7.314,0l0,2.99z M364.22,153.54q5.382,0,7.613,2.806t2.231,8.878q0,6.532-2.116,9.384t-7.728,2.852t-7.728-2.852t-2.116-9.384q0-6.072,2.254-8.878t7.59-2.806z M364.22,174.47q3.91,0,5.106-2.001t1.196-7.245q0-4.876-1.334-6.785t-4.968-1.909q-3.588,0-4.945,1.909t-1.357,6.785q0,5.198,1.219,7.222t5.083,2.024z M379.676,177l0-23l3.404,0l0,3.128q4.002-2.76,8.418-3.634l0,3.496q-1.932,0.368-4.025,1.15t-3.197,1.38l-1.15,0.598l0,16.882l-3.45,0z"></path></svg>
	</div>
	{#if parsedSims.length > 0}
		<div class="text-xs py-7 text-center">Previous simulations</div>

		<div class="flex flex-col gap-3">
			{#each parsedSims as sim, simIndex (sim.id)}
				<div class="relative group px-11" animate:flip={{ duration: 250, easing: quintOut }}>
					<PreviousSimulation
						{sim}
						ondelete={() => deleteSim(simIndex)}
						onclick={() => {
							$simId = sim.id;
							$companyName = sim.name;
							$founders = sim.founders;
							$events = sim.events;
							$exit = sim.exit;
							$loadedData = true;

							const url = new URL(window.location.href);
							url.searchParams.set('data', previousSims[simIndex]);
							window.history.replaceState({}, '', url);
						}}
					/>
				</div>
			{/each}
		</div>
	{/if}

	<div class="flex-1 flex items-center pb-40 justify-center">
		<Button
			onclick={() => {
				resetData();
				$simId = generateId();
				$loadedData = true;
			}}>New simulation</Button
		>
	</div>
</div>
