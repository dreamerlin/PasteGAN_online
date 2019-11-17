<template>
	<div>
		<canvas :id="canv_id" :key="key" class="canvas" width="50px" height="50px"></canvas>
	</div>
</template>

<script>
	export default{
		props:{
			canv_id:{
				type:String
			},
      key: {
        type:String
      }
		},


	}
</script>

<style>
</style>
