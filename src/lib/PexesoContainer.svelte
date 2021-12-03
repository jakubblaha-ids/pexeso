<script>
	export let cols = 1;
</script>

<div class="grid place-items-center">
	<div class="grid gap-2" style="grid-template-columns: repeat({cols}, minmax(0, 1fr));">
		<slot />
	</div>
</div>
