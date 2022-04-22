<script>
	import BoardLine from './BoardLine.svelte';
	import InputPanel from './InputPanel.svelte';

	export let title;

	let board = [
		[0, 0, 0, 0, 0, 0, 0, 0, 0],
		[0, 0, 0, 0, 0, 0, 0, 0, 0],
		[0, 0, 0, 0, 0, 0, 0, 0, 0],
		[0, 0, 0, 0, 0, 0, 0, 0, 0],
		[0, 0, 0, 0, 0, 0, 0, 0, 0],
		[0, 0, 0, 0, 0, 0, 0, 0, 0],
		[0, 0, 0, 0, 0, 0, 0, 0, 0],
		[0, 0, 0, 0, 0, 0, 0, 0, 0],
		[0, 0, 0, 0, 0, 0, 0, 0, 0]
	];

	let isLocked = [
		[false, false, false, false, false, false, false, false, false],
		[false, false, false, false, false, false, false, false, false],
		[false, false, false, false, false, false, false, false, false],
		[false, false, false, false, false, false, false, false, false],
		[false, false, false, false, false, false, false, false, false],
		[false, false, false, false, false, false, false, false, false],
		[false, false, false, false, false, false, false, false, false],
		[false, false, false, false, false, false, false, false, false],
		[false, false, false, false, false, false, false, false, false]
	];

	let fault = [
		[false, false, false, false, false, false, false, false, false],
		[false, false, false, false, false, false, false, false, false],
		[false, false, false, false, false, false, false, false, false],
		[false, false, false, false, false, false, false, false, false],
		[false, false, false, false, false, false, false, false, false],
		[false, false, false, false, false, false, false, false, false],
		[false, false, false, false, false, false, false, false, false],
		[false, false, false, false, false, false, false, false, false],
		[false, false, false, false, false, false, false, false, false]
	];

	let [selectedRow, selectedCol] = [-1, -1];

	let complete = false;

	function checkBoard() {
		const isFault = [
			[false, false, false, false, false, false, false, false, false],
			[false, false, false, false, false, false, false, false, false],
			[false, false, false, false, false, false, false, false, false],
			[false, false, false, false, false, false, false, false, false],
			[false, false, false, false, false, false, false, false, false],
			[false, false, false, false, false, false, false, false, false],
			[false, false, false, false, false, false, false, false, false],
			[false, false, false, false, false, false, false, false, false],
			[false, false, false, false, false, false, false, false, false]
		];

		for (let i = 0; i < 9; i++) {
			for (let j = 0; j < 9; j++) {
				// Skip locked
				if (isLocked[i][j]) {
					continue;
				}

				// Across
				for (let k = 0; k < 9; k++) {
					if (j === k || board[i][k] === 0) {
						continue;
					}
					if (board[i][k] === board[i][j]) {
						isFault[i][j] = true;
					}
				}

				// Down
				for (let k = 0; k < 9; k++) {
					if (i === k || board[k][j] === 0) {
						continue;
					}
					if (board[k][j] === board[i][j]) {
						isFault[i][j] = true;
					}
				}

				// Square
				const [startR, startC] = [Math.floor(i / 3) * 3, Math.floor(j / 3) * 3];
				for (let sqi = 0; sqi < 3; sqi++) {
					for (let sqj = 0; sqj < 3; sqj++) {
						const [curR, curC] = [startR + sqi, startC + sqj];
						if ((curR === i && curC === j) || board[curR][curC] === 0) {
							continue;
						}
						if (board[curR][curC] === board[i][j]) {
							isFault[i][j] = true;
						}
					}
				}
			}
		}

		return isFault;
	}

	function backtrackBoard(current) {
		const [curR, curC] = [Math.floor(current / 9), current % 9];
		const rotation = [1, 2, 3, 4, 5, 6, 7, 8, 9].sort(() => (Math.random() - 0.5));

		for (let i = 0; i < 9; i++) {
			board[curR][curC] = rotation[i];

			let fail = false;
			const isFault = checkBoard();

			/*
			let failMap = '';
			printBoard();
			for (let y = 0; y < 9 && !fail; y++) {
				for (let x = 0; x < 9 && !fail; x++) {
					failMap += (isFault[y][x] ? 'X' : '.');
				}
				failMap += '\n';
			}
			failMap += '\n';
			console.log(failMap);
			*/

			for (let y = 0; y < 9 && !fail; y++) {
				for (let x = 0; x < 9 && !fail; x++) {
					if (isFault[y][x]) {
						fail = true;
					}
				}
			}

			if (fail) {
				continue;
			}

			if (current === 80) {
				return true;
			} else if (backtrackBoard(current + 1)) {
				return true;
			}
		}

		// If failed, restore
		board[curR][curC] = 0;
		return false;
	}

	function initBoard() {
		backtrackBoard(0);
	}

	function holeBoard(maxCount) {
		let count = 0;
		
		if (maxCount < 0 || maxCount > 81) {
			return;
		}

		while (count < maxCount) {
			const current = Math.floor(Math.random() * 81);
			const [curR, curC] = [Math.floor(current / 9), current % 9];

			if (board[curR][curC] === 0) {
				continue;
			}

			board[curR][curC] = 0;
			count++;
		}

		//console.log(board);
	}

	function lockBoard() {
		for (let i = 0; i < 9; i++) {
			for (let j = 0; j < 9; j++) {
				if (board[i][j] === 0) {
					continue;
				}

				isLocked[i][j] = true;
			}
		}
	}

	function moveCursor(row, col) {
		//console.log(row, col);
		if (row < 0 || row >= 9 || col < 0 || col >= 9) {
			return;
		}
		[selectedRow, selectedCol] = [row, col];
	}

	function setValue(row, col, value) {
		if (row < 0 || row >= 9 || col < 0 || col >= 9) {
			return;
		}
		if (value < 0 || value > 9) {
			return;
		}
		board[row][col] = value;
		fault = checkBoard();
		complete = checkComplete();
	}

	function checkComplete() {
		for (let i = 0; i < 9; i++) {
			for (let j = 0; j < 9; j++) {
				if (board[i][j] === 0 || fault[i][j]) {
					return false;
				}
			}
		}
		return true;
	}

	initBoard();
	fault = checkBoard();
	holeBoard(32);
	lockBoard();
</script>


<svelte:window
	on:keydown={event => {
		//console.log(event.key);
		let newRow = (0 <= selectedRow && selectedRow < 9) ? selectedRow : 4;
		let newCol = (0 <= selectedCol && selectedCol < 9) ? selectedCol : 4;
		switch (event.key) {
			case 'ArrowUp':
				moveCursor((9 + newRow - 1) % 9, newCol);
				break;
			case 'ArrowDown':
				moveCursor((newRow + 1) % 9, newCol);
				break;
			case 'ArrowLeft':
				moveCursor(newRow, (9 + newCol - 1) % 9);
				break;
			case 'ArrowRight':
				moveCursor(newRow, (newCol + 1) % 9);
				break;
			case '1': case '2': case '3': case '4': case '5':
			case '6': case '7': case '8': case '9': case '0':
				setValue(selectedRow, selectedCol, parseInt(event.key));
				break;
		}
	}}
/>
<main>
	<h1>{title}</h1>
	<div
		on:click={event => {
			//console.dir(event.target);
			if (event.target.nodeName !== 'BUTTON') {
				return;
			}
			const [row, col] = Array.from(event.target.id.split('-')[1]).map(item => parseInt(item));
			if (row < 0 || row >= 9 || col < 0 || col >= 9) {
				return;
			}
			moveCursor(row, col);
		}}
	>
		{#each board as line, row}
			<BoardLine
				boardLine={line}
				lockedLine={isLocked[row]}
				faultLine={fault[row]}
				{row}
				{selectedRow}
				{selectedCol}
			/>
		{/each}
	</div>
	<div
		on:click={event => {
			if (event.target.nodeName !== 'BUTTON') {
				return;
			}
			if (selectedRow < 0 || selectedRow >= 9 || selectedCol < 0 || selectedCol >= 9) {
				return;
			}
			const value = parseInt(event.target.id.split('-')[1]);
			setValue(selectedRow, selectedCol, value);
		}}
	>
		<InputPanel
			{complete}
			{selectedRow}
			{selectedCol}
			locked={(selectedRow < 0 || selectedRow >= 9 || selectedCol < 0 || selectedCol >= 9) ? false : isLocked[selectedRow][selectedCol]}
		/>
	</div>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 3vh;
		font-weight: 100;
	}
</style>
