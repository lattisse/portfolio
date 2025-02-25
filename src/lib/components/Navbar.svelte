<script lang="ts">
	let isMenuOpen = $state(false);
	let isMobile = $state(false);

	function toggleMenu() {
		isMenuOpen = !isMenuOpen;
		// toggle body scroll
		if (isMenuOpen) {
			document.body.style.overflow = 'hidden';
		} else {
			document.body.style.overflow = '';
		}
	}

	function checkMobile() {
		isMobile = window.innerWidth <= 768;

		// resets body overflow when window is resized to desktop view when menu is open
		if (!isMobile && isMenuOpen) {
			isMenuOpen = false;
			document.body.style.overflow = '';
		}
	}

	$effect(() => {
		if (typeof window !== 'undefined') {
			checkMobile();
			window.addEventListener('resize', checkMobile);

			return () => {
				window.removeEventListener('resize', checkMobile);
				document.body.style.overflow = '';
			};
		}
	});
</script>

<nav class={`fixed z-50 ${isMobile ? 'top-0 right-0 p-4' : 'right-0 bottom-0 p-6'}`}>
	<!--Mobile Hamburger Button-->
	{#if isMobile}
		<button
			class="text-paragraph hover:text-heading relative z-50 focus:outline-none"
			onclick={toggleMenu}
			aria-label="Toggle Menu"
		>
			<!--Hamburger Icon-->
			{#if !isMenuOpen}
				<svg
					xmlns="http://www.w3.org/2000/svg"
					class="h-6 w-6"
					fill="none"
					viewBox="0 0 24 24"
					stroke="currentColor"
				>
					<path
						stroke-linecap="round"
						stroke-linejoin="round"
						stroke-width="2"
						d="M4 6h16M4 12h16M4 18h16"
					/>
				</svg>
				<!-- Close Icon -->
			{:else}
				<svg
					xmlns="http://www.w3.org/2000/svg"
					class="h-6 w-6"
					fill="none"
					viewBox="0 0 24 24"
					stroke="currentColor"
				>
					<path
						stroke-linecap="round"
						stroke-linejoin="round"
						stroke-width="2"
						d="M6 18L18 6M6 6l12 12"
					/>
				</svg>
			{/if}
		</button>

		<!--Mobile Nav Menu-->
		<div
			class={`bg-bg fixed inset-0 z-40 flex flex-col items-center justify-center font-medium transition-all duration-200 ease-in-out ${isMenuOpen ? 'visible opacity-100' : 'invisible opacity-0'}`}
		>
			<div
				class={`text-paragraph flex flex-col items-center space-y-12 text-2xl transition-transform duration-200 ease-in-out ${isMenuOpen ? 'translate-y-0' : 'translate-y-10'}`}
			>
				<a href="/" class="" onclick={toggleMenu}> Home </a>
				<a href="/projects" onclick={toggleMenu}> Projects </a>
				<a href="/blog" onclick={toggleMenu}> Blog </a>
			</div>
		</div>
	{:else}
		<div class="text-paragraph flex space-x-6 text-xl font-medium">
			<a href="/" class="hover:text-heading">Home</a>
			<a href="/projects" class="hover:text-heading">Projects</a>
			<a href="/blog" class="hover:text-heading">Blog</a>
		</div>
	{/if}
</nav>
