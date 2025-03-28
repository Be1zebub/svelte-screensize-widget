<script>
	import {
		faCopy,
		faDesktop,
		faLaptop,
		faMobileRetro,
		faMobileScreen,
		faTabletScreenButton,
		faTv,
	} from "@fortawesome/free-solid-svg-icons"
	import { onMount } from "svelte"
	import Fa from "svelte-fa"

	// Screen size breakpoints (matching tailwind)
	const breakpoints = [
		{
			width: 320,
			shortName: "xs", // Extra Small
			name: "Small phone",
			icon: faMobileRetro,
		},
		{
			width: 640,
			shortName: "sm", // Small
			name: "Phone",
			icon: faMobileScreen,
		},
		{
			width: 768,
			shortName: "md", // Medium
			name: "Tablet",
			icon: faTabletScreenButton,
		},
		{
			width: 1024,
			shortName: "lg", // Large
			name: "Laptop",
			icon: faLaptop,
		},
		{
			width: 1280,
			shortName: "xl", // Extra Large
			name: "Desktop",
			icon: faDesktop,
		},
		{
			width: 1536,
			shortName: "2xl", // 2X Large
			name: "Large screen / TV",
			icon: faTv,
		},
	]

	let align = "bottom-left"
	let device
	let width = 0

	function updateScreenSize() {
		width = window.innerWidth

		for (let i = 0; i < breakpoints.length; i++) {
			if (width < breakpoints[i].width) {
				device = breakpoints[i]
				return
			}
		}

		device = breakpoints[breakpoints.length - 1]
	}

	function copySelector() {
		const selector = `@media (max-width: ${device.width}px) {\n\t/* ${device.name} (${device.shortName}) */\n}`
		navigator.clipboard.writeText(selector)
		console.log("copied", selector)
	}

	onMount(() => {
		updateScreenSize()
		window.addEventListener("resize", updateScreenSize)

		return () => {
			window.removeEventListener("resize", updateScreenSize)
		}
	})
</script>

<div class="screen-size-widget" widget-align={align}>
	{#if device}
		<span class="info">
			<div class="device">
				<Fa icon={device.icon} />
				<span>{device.name}</span>
			</div>
			<div class="details">
				<span class="size">{device.shortName}</span>
				<span class="width">{width}px</span>
			</div>
		</span>
		<span class="buttons">
			<span class="align">
				<button
					title="Move top-left"
					aria-label="Move top-left"
					on:click={() => (align = "top-left")}
				></button>
				<button
					title="Move top-right"
					aria-label="Move top-right"
					on:click={() => (align = "top-right")}
				></button>
				<button
					title="Move bottom-left"
					aria-label="Move bottom-left"
					on:click={() => (align = "bottom-left")}
				></button>
				<button
					title="Move bottom-right"
					aria-label="Move bottom-right"
					on:click={() => (align = "bottom-right")}
				></button>
			</span>
			<button
				class="copy"
				title="Copy selector"
				aria-label="Copy selector"
				on:click={copySelector}
			>
				<Fa icon={faCopy} />
			</button>
		</span>
	{/if}
</div>

<style>
	.screen-size-widget {
		position: fixed;
		z-index: 9999;

		background-color: rgb(15, 15, 15);
		box-shadow: 0 0 10px rgba(255, 255, 255, 0.15);

		padding: 4px 8px;
		border-radius: 4px;

		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center;
	}

	.screen-size-widget[widget-align="top-left"] {
		top: 10px;
		left: 10px;
	}

	.screen-size-widget[widget-align="top-right"] {
		top: 10px;
		right: 10px;
	}

	.screen-size-widget[widget-align="bottom-left"] {
		bottom: 10px;
		left: 10px;
	}

	.screen-size-widget[widget-align="bottom-right"] {
		bottom: 10px;
		right: 10px;
	}

	.info {
		font-family: monospace;
		font-size: 12px;

		display: flex;
		flex-direction: column;
		gap: 8px;
	}

	.info > div {
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center;
		gap: 6px;
	}

	.device {
		font-size: 14px;
		font-weight: normal;
		color: rgb(200, 200, 200);
	}

	.size {
		font-size: 11px;
		font-weight: bold;
		color: rgb(255, 255, 255);
	}

	.width {
		font-size: 10px;
		font-weight: normal;
		color: rgb(150, 150, 150);
	}

	.buttons {
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center;
		gap: 8px;

		transition: all 0.2s ease-in-out;

		width: 0;
		height: 0;
		margin-left: 0;
		opacity: 0;
		overflow: hidden;
	}

	.screen-size-widget:hover .buttons {
		width: auto;
		height: auto;
		margin-left: 16px;
		opacity: 1;
	}

	.copy {
		font-size: 12px;
		font-weight: normal;
		color: rgb(175, 175, 175);

		cursor: pointer;
		transition: color 0.2s ease-in-out;
	}

	.copy:hover {
		color: rgb(255, 255, 255);
	}

	.align {
		display: grid;
		grid-template-columns: repeat(2, 1fr);
		grid-template-rows: repeat(2, 1fr);
		gap: 2px;
	}

	.align > button {
		width: 5px;
		height: 5px;
		background-color: rgb(175, 175, 175);
		border-radius: 1px;
		cursor: pointer;
		transition: background-color 0.2s ease-in-out;
	}

	.align > button:hover {
		background-color: rgb(255, 255, 255);
	}
</style>
