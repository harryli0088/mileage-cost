<script lang="ts">
  import Fa from 'svelte-fa'
  import { faBolt, faBurn } from '@fortawesome/free-solid-svg-icons'
  import { faGithub } from '@fortawesome/free-brands-svg-icons'

  import Blanchor from '$lib/Blanchor.svelte';
import NumberInput from '$lib/NumberInput.svelte';

  const ALL_UNITS = {
    imperial: {
      distance: "Miles",
      volume: "Gallon",
    },
    metric: {
      distance: "Kilometers",
      volume: "Liter",
    },
  }

  let costPerKwh:number = 0.23
  let gasCostPerVolume:number = 3.5 //ie price of gas per gallon
  let gasDistancePerVolume: number = 29 //ie mpg
  let kwhPer100Distance:number = 27 // Tesla Model Y Long Range https://www.fueleconomy.gov/feg/Find.do?action=sbs&id=43406
  let unitType: "imperial" | "metric" = "imperial"

  $: electricCost = kwhPer100Distance * costPerKwh //kwh / 100 distance * cost / kwh = cost / 100 distance
  $: gasCost = 100 * gasCostPerVolume / gasDistancePerVolume //100 distance * (cost/volume) / ( distance/volume ) = cost / 100 distance
  $: units = unitType==="imperial" ? ALL_UNITS.imperial : ALL_UNITS.metric
</script>

<div id="calculator">
	<!-- <header>
    <div>
      <h1>Svelte Kit Template</h1>
      <br/>
      <div style="font-size:2em">
        <Blanchor href="https://github.com/harryli0088/svelte-kit-template">
          <Fa class="icon-button" icon={faGithub} style="color:white;"/>
        </Blanchor>
      </div>
    </div>
  </header> -->

  <div id="gas-container">
    <h2>Gas Car <Fa icon={faBurn} style="color:red"/></h2>
  
    <div class="inputs-container">
      <div>
        <NumberInput
          bind:value={gasDistancePerVolume}
          id="miles-per-volume"
          label={`Miles per ${units.volume}`}
          step={1}
        />
      </div>

      <div>
        <NumberInput
          bind:value={gasCostPerVolume}
          id="cost-per-volume"
          label={`Cost per ${units.volume}`}
          step={.1}
        />
      </div>
    </div>

    <br/>

    <div>
      <div><b>Cost per 100 {units.distance}</b></div>
      <div>100 * {gasCostPerVolume} / {gasDistancePerVolume} = </div>
      <br/>
      <div class="cost">${gasCost.toFixed(2)}</div>
    </div>
  </div>

  <div id="electric-container">
    <h2>Electric Car <Fa icon={faBolt} style="color:gold"/></h2>

    <div class="inputs-container">
      <div>
        <NumberInput
          bind:value={costPerKwh}
          id="cost-per-kwh"
          label={`Cost per KWH`}
          step={.01}
        />
      </div>
      
      <div>
        <NumberInput
          bind:value={kwhPer100Distance}
          id="kwh-per-distance"
          label={`KWH per 100 ${units.distance}`}
          step={1}
        />
      </div>
    </div>
    

    <br/>

    <div>
      <div><b>Cost per 100 {units.distance}</b></div>
      <div>{kwhPer100Distance} * {costPerKwh} = </div>
      <br/>
      <div class="cost">${electricCost.toFixed(2)}</div>
    </div>
  </div>
</div>

<section>
  <h1>Gas vs Electric Car Mileage Cost Calculator</h1>

  <p>This website lets you compare how much it costs to drive 100 {units.distance.toLowerCase()} in a gas vs electric powered car. For the gas car, enter the cost of gas and fuel efficiency. For the electric car, enter how much you pay per kilowatt hour (KWH), ie from your electric bill or supercharging, and how many KWHs it takes for your car to drive 100 {units.distance.toLowerCase()}.</p>
  <p>How I set the default numbers:</p>
  <ul>
    <li><b>Miles per {units.volume}:</b> an estimate for the <Blanchor href="https://www.fueleconomy.gov/feg/noframes/43475.shtml">2021 Honda CRV AWD</Blanchor>, a mid-sized SUV</li>
    <li><b>Cost per {units.volume}:</b> a dollar estimate for the cost of gas</li>
    <li><b>Cost per KWH:</b> my residential electricity cost in the Greater Boston Area</li>
    <li><b>KWH per 100 {units.distance}:</b> the EPA estimate for the <Blanchor href="https://www.fueleconomy.gov/feg/Find.do?action=sbs&id=43406">2021 Tesla Model Y Long Range</Blanchor>, a mid-sized SUV</li>
  </ul>

  <p>You should also check out the Environment Protection Agency <Blanchor href="https://www.epa.gov/greenvehicles/electric-vehicle-myths#Myth5">Electric Vehicle Myths</Blanchor>.</p>
</section>

<style>
	/* header {
		display: flex;
		align-items: center;
		justify-content: center;
		overflow: hidden;
		background-color: #17202A;
		color: white;
    text-align: center;
    height: 70vh;
	}
  @media only screen and (min-width: 600px) {
		header {
			height: 50vh;
		}
	} */

  #calculator {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  #gas-container, #electric-container {
    width: 100%;
    padding: 1em;
    box-sizing: border-box;
    text-align: center;
  }

  #gas-container {
    background-color: #FDEDEC;
    border-bottom: 1px solid gray;
  }

  #electric-container {
    background-color: #EAFAF1;
  }

  .inputs-container > div {
    display: inline-block;
  }
  .inputs-container > div:first-child {
    padding-right: 0.25em;
  }


  /* @media only screen and (min-width: 600px) {
  } */
  @media only screen and (min-width: 1000px) {
    #calculator {
      display: flex;
      flex-direction: row;
    }

    #gas-container, #electric-container {
      width: 50%;
    }
    
    #gas-container {
      border-right: 1px solid gray;
      border-bottom: none;
      text-align: right;
    }

    #electric-container {
      text-align: left;
    }

    .inputs-container > div {
      display: block;
    }
    .inputs-container > div:first-child {
      padding-right: 0;
      padding-bottom: 1em;
    }
  }

  .cost {
    font-size: 1.5em;
    font-weight: bold;
    display: inline-block;
    padding-left: 0.5em;
    padding-right: 0.5em;
    padding-top: 0.25em;
    padding-bottom: 0.25em;
    color: white;
  }
  #gas-container .cost {
    background-color: #E74C3C;
  }
  #electric-container .cost {
    background-color: #2ECC71;
  }

  section {}
</style>