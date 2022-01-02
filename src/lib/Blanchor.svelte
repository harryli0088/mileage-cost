<script lang="ts">
	export let href:string = ""
  export let style:string = ""
</script>

<a {href} target="_blank" rel="noopener noreferrer" {style}><slot/></a>