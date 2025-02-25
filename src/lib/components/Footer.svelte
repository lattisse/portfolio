<script lang="ts">
	let isMobile = $state(false);

	function checkMobile() {
		isMobile = window.innerWidth < 768;
	}

	$effect(() => {
		if (typeof window !== 'undefined') {
			checkMobile();
			window.addEventListener('resize', checkMobile);

			return () => {
				window.removeEventListener('resize', checkMobile);
			};
		}
	});
</script>

<footer class = {`fixed bottom-0 font-medium md:text-xl ${isMobile ? 'left-1/2 transform -translate-x-1/2 pb-2' : 'left-0 p-6'}`}>
    <p class="text-paragraph whitespace-nowrap">Made on Earth by <span class="text-highlight">Crystal Bajgai</span></p>
</footer>
