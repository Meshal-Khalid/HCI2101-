<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <title>لعبة XO - مشعل</title>
    <style>
        * {
            box-sizing: border-box;
        }

        body {
            margin: 0;
            min-height: 100vh;
            font-family: "Segoe UI", Tahoma, sans-serif;
            background: linear-gradient(135deg, #1d2671, #c33764);
            display: flex;
            flex-direction: column;
            align-items: center;
            color: #fff;
        }

        h1 {
            margin-top: 30px;
            font-size: 36px;
        }

        .controls {
            margin: 20px 0;
        }

        button {
            background: #ffffff;
            color: #333;
            border: none;
            padding: 12px 22px;
            margin: 5px;
            border-radius: 10px;
            font-size: 16px;
            cursor: pointer;
            transition: 0.3s;
        }

        button:hover {
            background: #f0f0f0;
            transform: scale(1.05);
        }

        .board {
            display: grid;
            grid-template-columns: repeat(3, 110px);
            gap: 12px;
            margin-top: 20px;
        }

        .cell {
            width: 110px;
            height: 110px;
            background: rgba(255, 255, 255, 0.95);
            border-radius: 16px;
            font-size: 52px;
            color: #333;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: 0.2s;
        }

        .cell:hover {
            background: #eaeaea;
        }

        .status {
            margin-top: 25px;
            font-size: 20px;
            font-weight: bold;
        }

        footer {
            margin-top: auto;
            width: 100%;
            text-align: center;
            padding: 15px;
            background: rgba(0, 0, 0, 0.35);
            font-size: 14px;
        }
    </style>
</head>
<body>

    <h1>🎮 لعبة XO</h1>

    <div class="controls">
        <button onclick="setMode('single')">لاعب واحد</button>
        <button onclick="setMode('two')">لاعبين</button>
        <button onclick="resetGame()">إعادة اللعب</button>
    </div>

    <div class="board" id="board"></div>

    <div class="status" id="status">اختر نمط اللعب</div>

    <footer>
        Student Name: <strong>Meshal</strong> |
        ID: <strong>446006130</strong> |
        Supervised by: Mohammed Jabali
    </footer>

    <script>
        const boardElement = document.getElementById("board");
        const statusElement = document.getElementById("status");

        let board = Array(9).fill("");
        let currentPlayer = "X";
        let gameActive = false;
        let mode = "two";

        const winPatterns = [
            [0,1,2],[3,4,5],[6,7,8],
            [0,3,6],[1,4,7],[2,5,8],
            [0,4,8],[2,4,6]
        ];

        function setMode(selectedMode) {
            mode = selectedMode;
            resetGame();
            statusElement.textContent =
                mode === "single" ? "أنت تلعب ضد الكمبيوتر" : "وضع لاعبين";
        }

        function createBoard() {
            boardElement.innerHTML = "";
            board.forEach((value, index) => {
                const cell = document.createElement("div");
                cell.className = "cell";
                cell.textContent = value;
                cell.onclick = () => handleMove(index);
                boardElement.appendChild(cell);
            });
        }

        function handleMove(index) {
            if (!gameActive || board[index] !== "") return;

            board[index] = currentPlayer;
            createBoard();
            checkWinner();

            if (gameActive && mode === "single" && currentPlayer === "O") {
                setTimeout(computerMove, 500);
            }
        }

        function computerMove() {
            const emptyCells = board
                .map((v, i) => v === "" ? i : null)
                .filter(v => v !== null);

            if (emptyCells.length === 0) return;

            const randomIndex = emptyCells[Math.floor(Math.random() * emptyCells.length)];
            board[randomIndex] = "O";
            createBoard();
            checkWinner();
        }

        function checkWinner() {
            for (let pattern of winPatterns) {
                const [a, b, c] = pattern;
                if (board[a] && board[a] === board[b] && board[a] === board[c]) {
                    statusElement.textContent = `🎉 اللاعب ${currentPlayer} فاز!`;
                    gameActive = false;
                    return;
                }
            }

            if (!board.includes("")) {
                statusElement.textContent = "🤝 تعادل!";
                gameActive = false;
                return;
            }

            currentPlayer = currentPlayer === "X" ? "O" : "X";
            statusElement.textContent = `الدور على اللاعب ${currentPlayer}`;
        }

        function resetGame() {
            board = Array(9).fill("");
            currentPlayer = "X";
            gameActive = true;
            statusElement.textContent = "الدور على اللاعب X";
            createBoard();
        }

        createBoard();
    </script>

</body>
</html>

