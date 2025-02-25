<script lang="ts">
	import { page } from '$app/state';
	let isMenuOpen = $state(false);
	let isMobile = $state(false);

	// Function to toggle the menu and control body scroll
	function toggleMenu() {
		isMenuOpen = !isMenuOpen;
		if (isMenuOpen) {
			document.body.style.overflow = 'hidden';
		} else {
			document.body.style.overflow = '';
		}
	}

	// Check for mobile screen size
	function checkMobile() {
		isMobile = window.innerWidth <= 768;
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

	// Function to check if the current path matches exactly or starts with the given path
	function isActive(path: string) {
		if (path === '/') {
			return page.url.pathname === '/'; // Exact match for home
		}
		return page.url.pathname.startsWith(path); // If starts with path for other pages
	}
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
				<a
					href="/"
					class={isActive('/') ? 'underline underline-offset-2 ' : ''}
					onclick={toggleMenu}
				>
					Home
				</a>
				<a
					href="/projects"
					class={isActive('/projects') ? 'underline underline-offset-2' : ''}
					onclick={toggleMenu}
				>
					Projects
				</a>
				<a
					href="/blog"
					class={isActive('/blog') ? 'underline underline-offset-2' : ''}
					onclick={toggleMenu}
				>
					Blog
				</a>
			</div>
		</div>
	{:else}
		<div class="text-paragraph flex space-x-6 text-xl font-medium">
			<a href="/" class={isActive('/') ? 'underline underline-offset-2' : ''}>Home</a>
			<a href="/projects" class={isActive('/projects') ? 'underline underline-offset-2' : ''}
				>Projects</a
			>
			<a href="/blog" class={isActive('/blog') ? ' underline underline-offset-2' : ''}>Blog</a>
		</div>
	{/if}
</nav>
