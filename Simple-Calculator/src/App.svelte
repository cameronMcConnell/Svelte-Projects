<script>
	var num1 = '';
	var num2 = '';
	var op = ''
	var sol = 0;
	var solved = false;
	var pressedOp = false;
	var hasDecimal = false;
	var firstNum = false;

	function pressedNum(num) {
		if (!solved) {
			if (!pressedOp) {
				if (num1.length < 11) {
					if (!firstNum && num === '-') {
						num1 += num;
						firstNum = true;
					}
					else if (!hasDecimal) {
						if (!(firstNum && num === '-')) {
							num1 += num;
							firstNum = true;
							if (num === '.')
								hasDecimal = true;
						}
					}
					else if (hasDecimal && num !== '.' && num !== '-')
						num1 += num;
				}
			} 
			else {
				if (num2.length < 11) {
					if (!firstNum && num === '-') {
						num2 += num;
						firstNum = true;
					}
					else if (!hasDecimal) {
						if (!(firstNum && num === '-')) {
							num2 += num;
							firstNum = true;
							if (num === '.')
								hasDecimal = true;
						}
					}
					else if (hasDecimal && num !== '.' && num !== '-')
						num2 += num;
				}
			}
		}
	}

	function opHandler(oper) {
		if (!solved) {
			if (num1 === '')
				num1 = '0';

			op = oper;
			pressedOp = true;
			hasDecimal = false;
			firstNum = false;
		}
	}

	function clearNums() {
		num1 = '';
		num2 = '';
		op = '';
		pressedOp = false;
		solved = false;
		firstNum = false;
		hasDecimal = false;
		sol = 0;
	}

	function solve() {
		if (num1 !== '' && num2 !== '') {
			solved = true;
			switch (op) {
				case '÷':
					sol = parseFloat(num1) / parseFloat(num2);
					break;
				case '*':
					sol = parseFloat(num1) * parseFloat(num2);
					break;
				case '+':
					sol = parseFloat(num1) + parseFloat(num2);
					break;
				case '-':
					sol = parseFloat(num1) - parseFloat(num2);
					break;
			}
			if (!Number.isInteger(sol))
				sol = sol.toPrecision(5);
			else {
				var temp = sol.toString();
				if (temp.length > 11) {
					temp = temp.slice(temp.length-11, temp.length);
					sol = parseInt(temp);
				}
			}
		}
	}
</script>

<main>
	<h1>Simple-Calculator</h1>
	<div id='calc-back'>
		<div id='display'>
		{#if solved}
			{sol}
		{:else if num2 !== ''}
			{num2}
		{:else if pressedOp}
			{op}
		{:else if num1 !== ''}
			{num1}
		{/if}
		</div>
		<button on:click={() => clearNums()} id='c-but'>Clear</button>
		<div id='but-container'>
			<div id='nums'>
				<button on:click={() => pressedNum('9')} class="num-but">9</button>
				<button on:click={() => pressedNum('8')} class="num-but">8</button>
				<button on:click={() => pressedNum('7')} class="num-but">7</button>
				<button on:click={() => pressedNum('6')} class="num-but">6</button>
				<button on:click={() => pressedNum('5')} class="num-but">5</button>
				<button on:click={() => pressedNum('4')} class="num-but">4</button>
				<button on:click={() => pressedNum('3')} class="num-but">3</button>
				<button on:click={() => pressedNum('2')} class="num-but">2</button>
				<button on:click={() => pressedNum('1')} class="num-but">1</button>
				<button on:click={() => pressedNum('0')} class="num-but">0</button>
				<button on:click={() => pressedNum('.')} class="num-but">.</button>
				<button on:click={() => pressedNum('-')} class="num-but">(-)</button>
			</div>
			<div id='ops'>
				<button on:click={() => opHandler('÷')} class="op-but">÷</button>
				<button on:click={() => opHandler('*')}  class="op-but">*</button>
				<button on:click={() => opHandler('+')}  class="op-but">+</button>
				<button on:click={() => opHandler('-')}  class="op-but">-</button>
				<button on:click={() => solve()}  class="op-but">=</button>
			</div>
		</div>
	</div>
</main>

<style>
	main {
		display: flex;
		flex-direction: column;
		align-items: center;
		font-family: Open Sans;
		font-weight: bold;
	}

	h1 {
		font-size: 50px;
		text-decoration: underline;
	}

	button:hover {cursor: pointer;}

	#calc-back {
		width: 500px;
		height: 650px;
		background-color: darkgray;
		border: 5px dimgray solid;
		border-radius: 5px;
	}

	#c-but {
		width: 200px;
		height: 90px;
		margin-left: 10px;
		border: 2px dimgray solid;
		border-radius: 10px;
		background-color: white;
		font-family: Open Sans;
		font-size: 25px;
	}

	#but-container {
		display: flex;
	}

	#display {
		margin: 5px;
		padding: 5px 10px 5px 5px;
		width: auto;
		height: 100px;
		background-color: white;
		border: 5px dimgray solid;
		border-radius: 10px;
		text-align: right;
		font-size: 70px;
	}

	#nums {
		width: 325px;
		height: 425px;
		margin: 7px;
	}

	.num-but {
		width: 100px;
		height: 100px;
		background-color: white;
		border: 2px dimgray solid;
		border-radius: 10px;
		margin: 2.5px;
		font-size: 25px;
		font-family: Open Sans;
	}

	#ops {
		width: 100px;
		height: 500px;
		margin: 15px 10px 10px 10px;
		flex-grow: 1;
	}

	.op-but {
		width: 130px;
		height: 75px;
		background-color: white;
		border: 2px dimgray solid;
		border-radius: 10px;
		margin: 2.5px;
		font-size: 25px;
		font-family: Open Sans;
	}
</style>