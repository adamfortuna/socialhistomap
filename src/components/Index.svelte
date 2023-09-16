<script>
	import SocialHistomap from "$components/SocialHistomap/index.svelte";

	let width = 400;
</script>

<main>
	<div class="container" bind:clientWidth={width}>
		<header>
			<h1>
				The Social Media Histomap
			</h1>
			<p class="subtitle">
				Thirty Three years of Internet &amp; Social History
			</p>
			<p class="description">
				Area indicates relative active users of each social network
			</p>
			<p class="description">
				Created by{' '}
				<a href="/">
					Adam Fortuna
				</a>{' '}
				and inspired{' '}
				<a
					href="https://www.thehistomap.com/"
					target="_blank"
					rel="noreferrer"
				>
					The Histomap
				</a>{' '}
				(1931) by John B. Sparks
			</p>
		</header>

		<SocialHistomap width={width} height={100} />
	</div>
</main>

<style>

main {
	margin-top: 1rem;
}
.container {
	border: 3px solid black;
	max-width: 1280px;
	margin: 8px auto 0;
}

header {
	margin-bottom: 0 4px 8px 4px;
	padding: 6px;
	text-align: center;
}
h1 {
	font-family: var(--font-title);
}
.subtitle {
	font-size: var(--28px, 28px);
	text-transform: uppercase;
	line-height: 1.75rem;
	font-weight: 600;
}
.description {
	font-size: var(--20px, 20px);
	font-weight: 500;
}
</style>