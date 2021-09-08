<!--
===============
 COMPONENT JS 
===============
-->

<script lang='ts'>
	import { fade } from 'svelte/transition';

	// initialize the timer
	let hour: number = 0
	let minute: number = 0
	let seconds: number = 0
	let currentDate
	let timerSet: boolean = false
	let timeInputValid: boolean = true
	let dateObjDif
	let countdownInteval
	

	/**
	* Description:
	* ~~~~~~~~~~~
	* Resets the Timer
	* Clears the Interval
	*/
	function resetTimer() {
		console.log('resetting timer!');
		timerSet = false;
		window.clearInterval(countdownInteval);
		countdownInteval = undefined
	}

	
	/**
     * Description:
	 * ~~~~~~~~~~~~
	 * Starts the timer.
	 * Sets a target Future Time to CountDown to;
	 * Initializes the timer;
	*/	
	function startTimer() {
		console.log('starting timer!');
		// get the current date-time;
		currentDate = new Date();
		// set the current future time;
		currentDate.setHours(currentDate.getHours() + hour);
		currentDate.setMinutes(currentDate.getMinutes() + minute);
		currentDate.setSeconds(currentDate.getSeconds() + seconds);
		// TEST - DEV;
		// console.log('currentDate', currentDate) // correctly increases the timer time
		// initialize the timer;
		timerSet = true;
	}
	

	/**
	 * Description:
	 * ~~~~~~~~~~~~
	 * Validation Field;
	 * 
	 * Check if the input fields have been correctly
	 * input and formated;
	*/
	$: if (hour > 0 || minute > 0 || seconds > 0) {
		// console.log('input is valid!')
		timeInputValid = false // `enable` START Button
	} else {
		// console.log('input is not valid!')
		timeInputValid = true // `disable` START Button
	}
	

	/**
     * Description:
	 * ~~~~~~~~~~~~
	 * Set the Timer in Motion;
	 * And Calcualte the difference between target
	 * time and the current time;
	*/
	$: if (timerSet) {
		// TEST - DEV
		// console.log('setting the timer in motion! 🥳')
		dateObjDif = Date.parse(currentDate) - Date.parse(new Date())

		countdownInteval = setInterval(() => {
			// calculate the difference in the time;
			dateObjDif = Date.parse(currentDate) - Date.parse(new Date())
			// check if the timer has reached its goal of (00:00:00);
			if (dateObjDif < 0) {
				resetTimer()
			}
			// TEST - DEV
			// console.log('dateObjDif', dateObjDif)
		}, 1000);
	}


	/**
	 * Svelte JS Reactivity Methods:
	 * ~~~~~~~~~~~~~~~
	 * Seconds Counter;
	 * Minutes Counter;
	 * Hours Counter;
	*/
	$: countD_sec = Math.floor((dateObjDif / 1000) % 60).toString();
	$: if (countD_sec < 10) {
		countD_sec = "0" + countD_sec
	}
	$: countD_min = Math.floor((dateObjDif / 1000 / 60) % 60).toString();
	$: if (countD_min < 10) {
		countD_min = "0" + countD_min
	}
	$: countD_h = Math.floor((dateObjDif / (1000 * 60 * 60)) % 24).toString();
	$: if (countD_h < 10) {
		countD_h = "0" + countD_h
	}
</script>

<!--
===============
COMPONENT STYLE 
===============
-->

<style>
	:global(body) {
		background-color: black !important;
		width: 100vw;
    	overflow: hidden;
	}

	section {
		position: absolute;
		display: grid;
		bottom: 50%;
		top: 50%;
		width: 100%;
		/* height: 100%; */
		justify-items: center;
	}

	/* Timer CSS */
	#timer-container {
		display: flex;
		width: 100%;
		justify-content: space-evenly;
		margin-bottom: 50px;
	}
	#timer-container p {
		color: white;
		font-weight: bold;
		margin-left: 7.5px;
	}

	/* Timer Buttons Actions */
	#user-button-actions {
		display: flex;
		width: 100%;
		justify-content: space-around;
	}

	.timer-input {
		display: flex;
		align-content: center;
		align-items: center;
	}

	input,
	p {
		margin: 0;
		font-size: 20px;
	}

	/* Chrome, Safari, Edge, Opera */
	input::-webkit-outer-spin-button,
	input::-webkit-inner-spin-button {
		-webkit-appearance: none;
		margin: 0;
	}

	/* Firefox */
	input[type=number] {
		-moz-appearance: textfield;
	}

	input[type='number'] {
		outline: none;
		font-size: 20px;
		width: 50px;
		color: white;
		background-color: black;
		border: none;
		border: 2px solid transparent;
	} input[type='number']:hover,
	  input[type='number']:focus {
		border: 2px solid green;
		transition: all 0.3s ease;
	}


	button {
		transition: all 0.3s ease;
		cursor: pointer;
		width: fit-content;
    	height: fit-content;
	}

	#reset-btn {
		border-radius: 50%;
		padding: 25px 15px;
		background-color: transparent;
		color: white;
		font-weight: bold;
		border: 2px solid #cccccc
	} #reset-btn:hover {
		background-color: #cccccc;
		color: black;
	}

	#start-btn {
		border-radius: 50%;
		padding: 25px 15px;
		background-color: black;
		color: #ededed;
		font-weight: bold;
		border: 4px solid #cccccc;
	} #start-btn:hover {
		background-color: green;
		border: 4px solid green;
	}

	.active {
		border: 4px solid green !important;
	}
</style>

<!--
===============
COMPONENT HTML
===============
-->

<section>

	{#if !timerSet}
		<!-- 
		count-down-timer SELECT -->
		<div id='timer-container' in:fade>
			<!-- 
			hour time -->
			<div class='timer-input'>
				<input 
					type='number'
					bind:value={hour}
					required
				/>
				<p> hour </p>
			</div>
			<!-- 
			minute time -->
			<div class='timer-input'>
				<input 
					type='number'
					bind:value={minute}
					required
				/>
				<p> min </p>
			</div>
			<!-- 
			seconds time -->
			<div class='timer-input'>
				<input 
					type='number'
					bind:value={seconds}
					required
				/>
				<p> sec </p>
			</div>
		</div>
	{:else}
		<!-- 
		count-down-timer VIEW -->
		<div id='timer-container' in:fade>
			<!-- 
			hour time -->
			<div>
				<p> {countD_h} </p>
			</div>
			<p>:</p>
			<!-- 
			minute time -->
			<div>
				<p> {countD_min} </p>
			</div>
			<p>:</p>
			<!-- 
			seconds time -->
			<div>
				<p> {countD_sec} </p>
			</div>
		</div>
	{/if}

	<!-- 
	user-action-buttons -->
	<div id='user-button-actions'>

		<button id='reset-btn'
			on:click={() => resetTimer()}
			>
			RESET
		</button>


		{#if !timerSet}
			<button id='start-btn'
				disabled={timeInputValid}
				class:active={timeInputValid === false}
				on:click={() => startTimer()}
				>
				START
			</button>
		{:else}
			<div />
		{/if}
	</div>

</section>
