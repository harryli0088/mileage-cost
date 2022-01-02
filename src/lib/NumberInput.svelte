<script lang="ts">
  import Fa from 'svelte-fa'
  import { faMinus, faPlus } from '@fortawesome/free-solid-svg-icons'

  export let id:string = ""
  export let label:string = ""
	export let value:number = 0
  export let step:number = 1
</script>

<span>
  <label for={id}>{label}</label>
  <div class="number-input">
    <button class="icon-container left" on:click={() => value = value - step}>
      <Fa icon={faMinus}/>
    </button>

    <input
      bind:value={value}
      {id}
      {step}
      type="number"
    />

    <button class="icon-container right" on:click={() => value = value + step}>
      <Fa icon={faPlus}/>
    </button>
  </div>
</span>

<style>
  span {
    display: inline-block;
  }

  input {
    font-size: 1.25em;
    width: 4em;
    margin-bottom: 0;
    text-align: center;
  }

  label {
    font-weight: bold;
  }

  .number-input {
    display: flex;
    align-items: center;
  }

  .number-input button.icon-container {
    font-size: 1.3em;
    cursor: pointer;
    transition: 0.25s;
    margin-bottom: 0;
    padding-left: 0.5em;
    padding-right: 0.5em;
    background-color: #ddd;
  }
  .number-input button.icon-container:hover {
    transform: scale(1.1);
  }
  .number-input button.icon-container:active {
    transform: scale(1);
  }
</style>