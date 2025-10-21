<script>
	import "../main.scss";
	import "@fontsource/ntr";
	import Icon from "@iconify/svelte";
	import favicon from '$lib/assets/favicon.png';
	import { onMount, onDestroy } from "svelte";

	let { children } = $props();

	let showScrollButton = $state(false);
	let scrollPercentage = $state(0);

	const scrollToTop = () => {
		if (typeof window !== "undefined") {
			window.scrollTo({ top: 0, behavior: "smooth" });
		}
	};

	const handleScroll = () => {
		if (typeof window !== "undefined") {
			const windowHeight = window.innerHeight;
			const documentHeight = document.documentElement.scrollHeight;
			const scrollTop = window.scrollY;
			const scrollableHeight = documentHeight - windowHeight;

			scrollPercentage = Math.round((scrollTop / scrollableHeight) * 100);

			if (window.scrollY > 100) {
				showScrollButton = true;
			} else {
				showScrollButton = false;
			}
		}
	};

	onMount(() => {
		if (typeof window !== "undefined") {
			window.addEventListener("scroll", handleScroll);
		}
	});

	onDestroy(() => {
		if (typeof window !== "undefined") {
			window.removeEventListener("scroll", handleScroll);
		}
	});
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

<main>
	<div class="nav">
		<div class="logo">
			<a href="/" class="logo-link">
				<h1>Chloe Teo</h1>
			</a>
		</div>
		<ul class="menulist">
			<!-- <a href="/">Home</a> -->
			<a href="#intro">Home</a>
			<a href="#about">About</a>
			<a href="#projects">Projects</a>
			<a href="#contact">Contact</a>
		</ul>
	</div>

	{@render children()}

	{#if showScrollButton}
		<button class="scroll-to-top" onclick={scrollToTop}>
			<svg
				class="progress-ring"
				width="60"
				height="60"
				viewBox="0 0 60 60"
			>
				<circle
					class="progress-ring-background"
					cx="30"
					cy="30"
					r="26"
				/>
				<circle
					class="progress-ring-bar"
					cx="30"
					cy="30"
					r="26"
					stroke-dasharray={2 * Math.PI * 26}
					stroke-dashoffset={2 *
						Math.PI *
						26 *
						(1 - scrollPercentage / 100)}
				/>
			</svg>
			<Icon icon="mdi:arrow-up" width="24" height="24" />
		</button>
	{/if}

	<footer class="footer">
		<div class="footer-content">
			<p>Built and designed by Chloe Teo.</p>
			<p>All rights reserved. &copy;</p>
		</div>
	</footer>
</main>

<style lang="scss">
	:global(html, body) {
		margin: 0;
		padding: 0;
		width: 100%;
		height: 100%;
	}

	:global(*) {
		box-sizing: border-box;
	}
	
	main {
		font-family: "NTR", sans-serif;
	}

	.nav {
		position: fixed;
		top: 0;
		right: 0;
		padding: 5px 10%;
		width: 100%;
		z-index: 1000;
		display: flex;
		align-items: center;
		justify-content: space-between;
		transition: all 0.6s ease;
		background-color: var(--navy);
		color: var(--lightest-slate);

		.logo-link {
			font-size: 0.7rem;
			font-weight: bold;
			color: var(--lightest-slate);
			text-decoration: none;
		}

		.menulist {
			display: flex;
			gap: 30px;

			a {
				position: relative;
				text-decoration: none;
				font-weight: bold;
				color: var(--lightest-slate);

				&::after {
					content: "";
					position: absolute;
					left: 50%;
					bottom: -2px;
					width: 0;
					height: 2px;
					background-color: var(--green-bright);
					transition: all 0.3s ease-in-out;
				}

				&:hover::after {
					width: 100%;
					left: 0;
				}
			}
		}

		.menu-right {
			display: flex;
			align-items: center;

			.menu-btn {
				display: flex;
				align-items: center;
				gap: 8px;
				text-decoration: none;
				padding: 10px 15px;
				border-radius: 13px;
				transition: all 0.3s ease-in-out;
				box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);

				&:hover {
					transform: scale(1.05);
					box-shadow: 0px 6px 15px rgba(0, 0, 0, 0.15);
				}
			}
		}
	}

	.scroll-to-top {
		position: fixed;
		bottom: 20px;
		right: 20px;
		border: none;
		background: white;
		border-radius: 50%;
		width: 60px;
		height: 60px;
		display: flex;
		justify-content: center;
		align-items: center;
		cursor: pointer;
		z-index: 1000;
		padding: 0;
		box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
		transition:
			transform 0.3s,
			box-shadow 0.3s;

		&:hover {
			transform: translateY(-3px);
			box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
		}

		:global(svg.iconify) {
			position: absolute;
			top: 50%;
			left: 50%;
			transform: translate(-50%, -50%);
			z-index: 2;
		}

		.progress-ring {
			position: absolute;
			top: 0;
			left: 0;
			width: 100%;
			height: 100%;
			transform: rotate(-90deg);
		}

		.progress-ring-background {
			fill: none;
			stroke: rgba(0, 0, 0, 0.1);
			stroke-width: 4;
		}

		.progress-ring-bar {
			fill: none;
			stroke: var(--navy);
			stroke-width: 4;
			transition: stroke-dashoffset 0.3s ease;
		}
	}

	.footer {
		width: 100%;
		padding: 10px 10% 50px;
		background-color: var(--navy);
		position: relative;
		text-align: center;

		p {
			margin: 0;
			color: var(--slate);
		}
	}
</style>
