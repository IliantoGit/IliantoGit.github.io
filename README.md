<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Шахматы на HTML</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: Arial, sans-serif;
            background: linear-gradient(135deg, #1a2a6c, #b21f1f, #1a2a6c);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }
        
        .chess-container {
            text-align: center;
            color: white;
            background: rgba(0, 0, 0, 0.7);
            padding: 20px;
            border-radius: 20px;
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.5);
        }
        
        h1 {
            margin-bottom: 20px;
            font-size: 2.5rem;
            text-shadow: 0 2px 4px rgba(0,0,0,0.3);
        }
        
        .chess-board {
            display: grid;
            grid-template-columns: repeat(8, 1fr);
            grid-template-rows: repeat(8, 1fr);
            width: 560px;
            height: 560px;
            margin: 20px auto;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
            border-radius: 10px;
            overflow: hidden;
            position: relative;
        }
        
        .square {
            aspect-ratio: 1;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2.5rem;
            cursor: pointer;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }
        
        .square.white {
            background-color: #f0d9b5;
        }
        
        .square.black {
            background-color: #b58863;
        }
        
        .square:hover {
            transform: scale(1.05);
            box-shadow: 0 0 20px rgba(255, 255, 255, 0.3);
        }
        
        .piece {
            position: absolute;
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2.2rem;
            transition: all 0.5s cubic-bezier(0.25, 0.46, 0.45, 0.94);
            z-index: 10;
        }
        
        .piece.white {
            color: #fff;
            text-shadow: 0 0 5px rgba(255,255,255,0.7);
        }
        
        .piece.black {
            color: #000;
            text-shadow: 0 0 5px rgba(0,0,0,0.7);
        }
        
        .piece.moving {
            animation: movePiece 0.5s ease-out;
            z-index: 20;
        }
        
        @keyframes movePiece {
            0% {
                transform: translate(0, 0) scale(1);
                opacity: 1;
            }
            50% {
                transform: translate(0, -10px) scale(1.1);
                opacity: 0.9;
            }
            100% {
                transform: translate(0, 0) scale(1);
                opacity: 1;
            }
        }
        
        .square.highlight {
            box-shadow: inset 0 0 0 3px #ffeb3b, 0 0 15px rgba(255, 235, 59, 0.7);
            animation: pulse 1.5s infinite;
        }
        
        @keyframes pulse {
            0% { box-shadow: inset 0 0 0 3px #ffeb3b, 0 0 10px rgba(255, 235, 59, 0.5); }
            50% { box-shadow: inset 0 0 0 5px #ffeb3b, 0 0 20px rgba(255, 235, 59, 0.8); }
            100% { box-shadow: inset 0 0 0 3px #ffeb3b, 0 0 10px rgba(255, 235, 59, 0.5); }
        }
        
        .square.possible-move {
            background-color: rgba(255, 255, 255, 0.3) !important;
        }
        
        .status-bar {
            margin: 20px 0;
            padding: 15px;
            background: rgba(0, 0, 0, 0.5);
            border-radius: 15px;
            font-size: 1.2rem;
            min-height: 40px;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        
        .reset-btn {
            margin-top: 20px;
            padding: 12px 30px;
            background: linear-gradient(45deg, #ff6b6b, #ff5252);
            color: white;
            border: none;
            border-radius: 30px;
            font-size: 1.1rem;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 5px 15px rgba(255, 107, 107, 0.4);
            font-weight: bold;
        }
        
        .reset-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(255, 107, 107, 0.6);
            background: linear-gradient(45deg, #ff5252, #ff6b6b);
        }
        
        .reset-btn:active {
            transform: translateY(0);
        }
        
        .info {
            margin-top: 15px;
            font-size: 0.9rem;
            opacity: 0.8;
        }
        
        @media (max-width: 600px) {
            .chess-board {
                width: 90vw;
                height: 90vw;
                max-width: 400px;
                max-height: 400px;
            }
            
            .square {
                font-size: 1.8rem;
            }
            
            .piece {
                font-size: 1.6rem;
            }
            
            h1 {
                font-size: 2rem;
            }
            
            .status-bar {
                font-size: 1rem;
            }
        }
    </style>
</head>
<body>
    <div class="chess-container">
        <h1>♚ ♛ ♜ ♝ ♞ ♟ ♙ ♖ ♗ ♘ ♕ ♔ ♚ ♛ ♜ ♝ ♞ ♟ ♙ ♖ ♗ ♘ ♕ ♔ ♚</h1>
        <div class="status-bar" id="status">
            Белые ходят первыми
        </div>
        <div class="chess-board" id="chessBoard"></div>
        <button class="reset-btn" id="resetBtn">Начать новую игру</button>
        <div class="info">
            Кликните на фигуру, чтобы выбрать ее, затем кликните на клетку для хода
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const board = document.getElementById('chessBoard');
            const statusEl = document.getElementById('status');
            const resetBtn = document.getElementById('resetBtn');
            
            // Chess board setup
            const boardSize = 8;
            let selectedPiece = null;
            let selectedPosition = null;
            let currentPlayer = 'white'; // white starts
            let possibleMoves = [];
            
            // Chess piece unicode
            const pieces = {
                white: {
                    king: '♔',
                    queen: '♕',
                    rook: '♖',
                    bishop: '♗',
                    knight: '♘',
                    pawn: '♙'
                },
                black: {
                    king: '♚',
                    queen: '♛',
                    rook: '♜',
                    bishop: '♝',
                    knight: '♞',
                    pawn: '♟'
                }
            };
            
            // Initialize board state
            const initialBoard = [
                ['♜', '♞', '♝', '♛', '♚', '♝', '♞', '♜'],
                ['♟', '♟', '♟', '♟', '♟', '♟', '♟', '♟'],
                ['', '', '', '', '', '', '', ''],
                ['', '', '', '', '', '', '', ''],
                ['', '', '', '', '', '', '', ''],
                ['', '', '', '', '', '', '', ''],
                ['♙', '♙', '♙', '♙', '♙', '♙', '♙', '♙'],
                ['♖', '♘', '♗', '♕', '♔', '♗', '♘', '♖']
            ];
            
            let boardState = JSON.parse(JSON.stringify(initialBoard));
            
            // Create board squares
            function createBoard() {
                board.innerHTML = '';
                for (let row = 0; row < boardSize; row++) {
                    for (let col = 0; col < boardSize; col++) {
                        const square = document.createElement('div');
                        square.classList.add('square');
                        square.classList.add((row + col) % 2 === 0 ? 'white' : 'black');
                        square.dataset.row = row;
                        square.dataset.col = col;
                        
                        // Add piece if exists
                        const piece = boardState[row][col];
                        if (piece) {
                            const pieceElement = document.createElement('div');
                            pieceElement.classList.add('piece');
                            pieceElement.classList.add(getPieceColor(piece));
                            pieceElement.textContent = piece;
                            square.appendChild(pieceElement);
                        }
                        
                        square.addEventListener('click', handleSquareClick);
                        board.appendChild(square);
                    }
                }
            }
            
            // Get piece color
            function getPieceColor(piece) {
                const whitePieces = Object.values(pieces.white);
                const blackPieces = Object.values(pieces.black);
                return whitePieces.includes(piece) ? 'white' : 'black';
            }
            
            // Handle square click
            function handleSquareClick(e) {
                const square = e.target.closest('.square');
                if (!square) return;
                
                const row = parseInt(square.dataset.row);
                const col = parseInt(square.dataset.col);
                const piece = boardState[row][col];
                
                // Clear previous highlights
                clearHighlights();
                
                // If clicking on a piece
                if (piece) {
                    const pieceColor = getPieceColor(piece);
                    
                    // Only allow selecting own pieces
                    if (pieceColor !== currentPlayer) {
                        showStatus(`Ходят ${currentPlayer === 'white' ? 'белые' : 'черные'}!`, '#ff6b6b');
                        return;
                    }
                    
                    // Select this piece
                    selectPiece(row, col, piece);
                } 
                // If clicking on empty square
                else if (selectedPiece) {
                    // Check if it's a valid move
                    const move = possibleMoves.find(m => m.row === row && m.col === col);
                    if (move) {
                        makeMove(selectedPosition.row, selectedPosition.col, row, col);
                    } else {
                        // Deselect if clicking elsewhere
                        selectedPiece = null;
                        selectedPosition = null;
                    }
                }
            }
            
            // Select a piece and show possible moves
            function selectPiece(row, col, piece) {
                selectedPiece = piece;
                selectedPosition = { row, col };
                
                // Highlight selected square
                const square = document.querySelector(`.square[data-row="${row}"][data-col="${col}"]`);
                square.classList.add('highlight');
                
                // Calculate and show possible moves
                possibleMoves = calculatePossibleMoves(row, col, piece);
                possibleMoves.forEach(move => {
                    const square = document.querySelector(`.square[data-row="${move.row}"][data-col="${move.col}"]`);
                    if (square) {
                        square.classList.add('possible-move');
                    }
                });
                
                showStatus(`Выбрана фигура: ${piece}`, '#4ecdc4');
            }
            
            // Calculate possible moves for a piece (simplified)
            function calculatePossibleMoves(row, col, piece) {
                const moves = [];
                const pieceType = getPieceType(piece);
                const color = getPieceColor(piece);
                const direction = color === 'white' ? -1 : 1;
                
                switch(pieceType) {
                    case 'pawn':
                        // Forward move
                        const newRow = row + direction;
                        if (isValidSquare(newRow, col) && !boardState[newRow][col]) {
                            moves.push({ row: newRow, col });
                            
                            // Double move from starting position
                            const startRow = color === 'white' ? 6 : 1;
                            if (row === startRow && !boardState[newRow][col] && !boardState[newRow + direction][col]) {
                                moves.push({ row: newRow + direction, col });
                            }
                        }
                        
                        // Captures
                        [-1, 1].forEach(deltaCol => {
                            const captureRow = row + direction;
                            const captureCol = col + deltaCol;
                            if (isValidSquare(captureRow, captureCol)) {
                                const targetPiece = boardState[captureRow][captureCol];
                                if (targetPiece && getPieceColor(targetPiece) !== color) {
                                    moves.push({ row: captureRow, col: captureCol });
                                }
                            }
                        });
                        break;
                        
                    case 'rook':
                        // Horizontal and vertical
                        [[0, 1], [1, 0], [0, -1], [-1, 0]].forEach(([dRow, dCol]) => {
                            let r = row + dRow;
                            let c = col + dCol;
                            while (isValidSquare(r, c)) {
                                if (!boardState[r][c]) {
                                    moves.push({ row: r, col: c });
                                } else {
                                    if (getPieceColor(boardState[r][c]) !== color) {
                                        moves.push({ row: r, col: c });
                                    }
                                    break;
                                }
                                r += dRow;
                                c += dCol;
                            }
                        });
                        break;
                        
                    case 'bishop':
                        // Diagonals
                        [[1, 1], [1, -1], [-1, 1], [-1, -1]].forEach(([dRow, dCol]) => {
                            let r = row + dRow;
                            let c = col + dCol;
                            while (isValidSquare(r, c)) {
                                if (!boardState[r][c]) {
                                    moves.push({ row: r, col: c });
                                } else {
                                    if (getPieceColor(boardState[r][c]) !== color) {
                                        moves.push({ row: r, col: c });
                                    }
                                    break;
                                }
                                r += dRow;
                                c += dCol;
                            }
                        });
                        break;
                        
                    case 'queen':
                        // Combine rook and bishop
                        [[0, 1], [1, 0], [0, -1], [-1, 0], [1, 1], [1, -1], [-1, 1], [-1, -1]].forEach(([dRow, dCol]) => {
                            let r = row + dRow;
                            let c = col + dCol;
                            while (isValidSquare(r, c)) {
                                if (!boardState[r][c]) {
                                    moves.push({ row: r, col: c });
                                } else {
                                    if (getPieceColor(boardState[r][c]) !== color) {
                                        moves.push({ row: r, col: c });
                                    }
                                    break;
                                }
                                r += dRow;
                                c += dCol;
                            }
                        });
                        break;
                        
                    case 'king':
                        // One square in any direction
                        [[-1, -1], [-1, 0], [-1, 1], [0, -1], [0, 1], [1, -1], [1, 0], [1, 1]].forEach(([dRow, dCol]) => {
                            const r = row + dRow;
                            const c = col + dCol;
                            if (isValidSquare(r, c)) {
                                const targetPiece = boardState[r][c];
                                if (!targetPiece || getPieceColor(targetPiece) !== color) {
                                    moves.push({ row: r, col: c });
                                }
                            }
                        });
                        break;
                        
                    case 'knight':
                        // L-shaped moves
                        [[-2, -1], [-2, 1], [-1, -2], [-1, 2], [1, -2], [1, 2], [2, -1], [2, 1]].forEach(([dRow, dCol]) => {
                            const r = row + dRow;
                            const c = col + dCol;
                            if (isValidSquare(r, c)) {
                                const targetPiece = boardState[r][c];
                                if (!targetPiece || getPieceColor(targetPiece) !== color) {
                                    moves.push({ row: r, col: c });
                                }
                            }
                        });
                        break;
                }
                
                return moves;
            }
            
            // Get piece type from unicode
            function getPieceType(piece) {
                const pieceMap = {
                    '♔': 'king', '♚': 'king',
                    '♕': 'queen', '♛': 'queen',
                    '♖': 'rook', '♜': 'rook',
                    '♗': 'bishop', '♝': 'bishop',
                    '♘': 'knight', '♞': 'knight',
                    '♙': 'pawn', '♟': 'pawn'
                };
                return pieceMap[piece] || 'pawn';
            }
            
            // Check if square is valid
            function isValidSquare(row, col) {
                return row >= 0 && row < boardSize && col >= 0 && col < boardSize;
            }
            
            // Make a move
            function makeMove(fromRow, fromCol, toRow, toCol) {
                // Clear selection
                clearHighlights();
                
                // Get the moving piece
                const piece = boardState[fromRow][fromCol];
                
                // Animate the move
                animatePiece(fromRow, fromCol, toRow, toCol, piece);
                
                // Update board state after animation delay
                setTimeout(() => {
                    boardState[toRow][toCol] = piece;
                    boardState[fromRow][fromCol] = '';
                    
                    // Switch player
                    currentPlayer = currentPlayer === 'white' ? 'black' : 'white';
                    
                    // Update status
                    showStatus(`Ходят ${currentPlayer === 'white' ? 'белые' : 'черные'}`, '#4ecdc4');
                    
                    // Check for game end conditions (simplified)
                    checkGameEnd();
                    
                    // Reset selection
                    selectedPiece = null;
                    selectedPosition = null;
                }, 500);
            }
            
            // Animate piece movement
            function animatePiece(fromRow, fromCol, toRow, toCol, piece) {
                const fromSquare = document.querySelector(`.square[data-row="${fromRow}"][data-col="${fromCol}"]`);
                const toSquare = document.querySelector(`.square[data-row="${toRow}"][data-col="${toCol}"]`);
                const pieceElement = fromSquare.querySelector('.piece');
                
                if (!pieceElement || !toSquare) return;
                
                // Add moving class for animation
                pieceElement.classList.add('moving');
                
                // Move the piece element to the target square
                toSquare.appendChild(pieceElement);
                
                // Remove moving class after animation
                setTimeout(() => {
                    pieceElement.classList.remove('moving');
                }, 500);
            }
            
            // Clear highlights
            function clearHighlights() {
                document.querySelectorAll('.square').forEach(square => {
                    square.classList.remove('highlight', 'possible-move');
                });
            }
            
            // Show status message
            function showStatus(message, color = '#4ecdc4') {
                statusEl.textContent = message;
                statusEl.style.color = color;
            }
            
            // Check for game end (simplified)
            function checkGameEnd() {
                // Simple check: if king is captured
                const whiteKingExists = boardState.some(row => row.includes('♔'));
                const blackKingExists = boardState.some(row => row.includes('♚'));
                
                if (!whiteKingExists) {
                    showStatus('Черные победили!', '#ff6b6b');
                    setTimeout(resetGame, 3000);
                } else if (!blackKingExists) {
                    showStatus('Белые победили!', '#4ecdc4');
                    setTimeout(resetGame, 3000);
                }
            }
            
            // Reset game
            function resetGame() {
                boardState = JSON.parse(JSON.stringify(initialBoard));
                currentPlayer = 'white';
                selectedPiece = null;
                selectedPosition = null;
                possibleMoves = [];
                createBoard();
                showStatus('Белые ходят первыми', '#4ecdc4');
            }
            
            // Reset button handler
            resetBtn.addEventListener('click', resetGame);
            
            // Initialize board
            createBoard();
            showStatus('Белые ходят первыми', '#4ecdc4');
        });
    </script>
</body>
</html>
