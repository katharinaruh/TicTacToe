
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Neon Tic-Tac-Toe</title>
    <style>
        body {
            background-color: #000;
            color: #fff;
            font-family: Arial, sans-serif;
            text-align: center;
        }
        .board {
            display: grid;
            grid-template-columns: repeat(3, 100px);
            grid-template-rows: repeat(3, 100px);
            gap: 5px;
            margin: 50px auto;
        }
        .cell {
            width: 100px;
            height: 100px;
            background-color: #fff;
            border: 2px solid #ff00ff;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 2.5rem;
            font-weight: bold;
            color: #ff00ff;
            cursor: pointer;
        }
        .cell.taken {
            pointer-events: none;
        }
        .scoreboard {
            margin-top: 20px;
        }
        #winner {
            margin-top: 20px;
            font-size: 1.5rem;
            color: #ff00ff;
        }
        #confetti {
            display: none;
            width: 300px;
            margin: 20px auto;
        }
    </style>
</head>
<body>
    <h1 style="color: #ff00ff;">Neon Tic-Tac-Toe</h1>
    <div class="board" id="board"></div>
    <div class="scoreboard">
        <p>Runde: <span id="round">1</span>/5</p>
        <p>Punkte: Kreuz - <span id="scoreX">0</span>, Kreis - <span id="scoreO">0</span></p>
    </div>
    <div id="winner"></div>
    <img id="confetti" src="https://media.giphy.com/media/lPeJu7nXfBSAdRxhpH/giphy.gif" alt="Winner Confetti">

    <script>
        const board = document.getElementById('board');
        const roundDisplay = document.getElementById('round');
        const scoreXDisplay = document.getElementById('scoreX');
        const scoreODisplay = document.getElementById('scoreO');
        const winnerDisplay = document.getElementById('winner');
        const confetti = document.getElementById('confetti');

        let currentPlayer = 'X';
        let round = 1;
        let scoreX = 0;
        let scoreO = 0;

        function createBoard() {
            board.innerHTML = '';
            for (let i = 0; i < 9; i++) {
                const cell = document.createElement('div');
                cell.classList.add('cell');
                cell.dataset.index = i;
                cell.addEventListener('click', handleMove);
                board.appendChild(cell);
            }
        }

        function handleMove(event) {
            const cell = event.target;
            if (!cell.classList.contains('taken')) {
                cell.textContent = currentPlayer;
                cell.classList.add('taken');

                if (checkWin()) {
                    updateScore();
                    showWinner();
                } else if (Array.from(document.querySelectorAll('.cell')).every(cell => cell.classList.contains('taken'))) {
                    nextRound();
                } else {
                    currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
                }
            }
        }

        function checkWin() {
            const winPatterns = [
                [0, 1, 2], [3, 4, 5], [6, 7, 8], // Rows
                [0, 3, 6], [1, 4, 7], [2, 5, 8], // Columns
                [0, 4, 8], [2, 4, 6]             // Diagonals
            ];
            const cells = Array.from(document.querySelectorAll('.cell')).map(cell => cell.textContent);

            return winPatterns.some(pattern =>
                pattern.every(index => cells[index] === currentPlayer)
            );
        }

        function updateScore() {
            if (currentPlayer === 'X') {
                scoreX++;
                scoreXDisplay.textContent = scoreX;
            } else {
                scoreO++;
                scoreODisplay.textContent = scoreO;
            }
        }

        function showWinner() {
            winnerDisplay.textContent = `${currentPlayer} gewinnt diese Runde!`;
            confetti.style.display = 'block';
            setTimeout(() => {
                confetti.style.display = 'none';
                nextRound();
            }, 3000);
        }

        function nextRound() {
            if (round === 5) {
                declareFinalWinner();
                return;
            }
            round++;
            roundDisplay.textContent = round;
            winnerDisplay.textContent = '';
            currentPlayer = 'X';
            createBoard();
        }

        function declareFinalWinner() {
            const finalWinner = scoreX > scoreO ? 'Kreuz' : 'Kreis';
            winnerDisplay.textContent = `${finalWinner} ist der endgültige Gewinner!`;
            confetti.style.display = 'block';
        }

        createBoard();
    </script>
</body>
</html>

