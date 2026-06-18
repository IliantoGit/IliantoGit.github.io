<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Шахматы</title>
<style>
* { margin: 0; padding: 0; box-sizing: border-box; }
body {
  background: #1a1a2e;
  color: #eee;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}
#app {
  display: flex;
  gap: 30px;
  align-items: flex-start;
}
#board-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}
#info-panel {
  background: #16213e;
  border-radius: 12px;
  padding: 24px;
  min-width: 220px;
  box-shadow: 0 4px 20px rgba(0,0,0,0.3);
}
#info-panel h2 {
  text-align: center;
  margin-bottom: 16px;
  font-size: 1.4em;
  color: #e94560;
}
.info-row {
  margin-bottom: 12px;
  font-size: 1em;
}
.info-label { color: #888; }
.info-value { color: #fff; font-weight: bold; }
#turn-indicator {
  text-align: center;
  font-size: 1.2em;
  padding: 10px;
  border-radius: 8px;
  margin-bottom: 16px;
  font-weight: bold;
}
.turn-white { background: #f0d9b5; color: #333; }
.turn-black { background: #b58863; color: #fff; }
#status {
  text-align: center;
  font-size: 1.1em;
  padding: 8px;
  border-radius: 8px;
  margin-bottom: 16px;
  min-height: 40px;
  font-weight: bold;
}
.status-check { background: #ff6b3520; color: #ff6b35; border: 1px solid #ff6b35; }
.status-checkmate { background: #e9456020; color: #e94560; border: 1px solid #e94560; }
.status-stalemate { background: #0f346020; color: #4fc3f7; border: 1px solid #4fc3f7; }

.captured-section {
  margin-bottom: 12px;
}
.captured-label { color: #888; font-size: 0.9em; margin-bottom: 4px; }
.captured-pieces {
  font-size: 1.5em;
  min-height: 30px;
  letter-spacing: 2px;
}

#btn-new-game {
  width: 100%;
  padding: 12px;
  font-size: 1em;
  border: none;
  border-radius: 8px;
  background: #e94560;
  color: #fff;
  cursor: pointer;
  font-weight: bold;
  transition: background 0.2s;
  margin-top: 12px;
}
#btn-new-game:hover { background: #c73650; }

#board {
  display: grid;
  grid-template-columns: repeat(8, 70px);
  grid-template-rows: repeat(8, 70px);
  border: 3px solid #b58863;
  border-radius: 4px;
  box-shadow: 0 8px 30px rgba(0,0,0,0.5);
}
.cell {
  width: 70px;
  height: 70px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 42px;
  cursor: pointer;
  position: relative;
  user-select: none;
  transition: background 0.15s;
}
.cell-light { background: #f0d9b5; }
.cell-dark { background: #b58863; }
.cell.selected { background: #7fc97f !important; }
.cell.valid-move::after {
  content: '';
  position: absolute;
  width: 20px;
  height: 20px;
  background: rgba(0,0,0,0.2);
  border-radius: 50%;
}
.cell.valid-capture {
  background: rgba(255,100,100,0.4) !important;
  border-radius: 50%;
}
.cell.last-move { background: rgba(255,255,100,0.35) !important; }
.cell.king-check {
  background: radial-gradient(circle, #ff000080, #ff000030) !important;
}

.coords-row, .coords-col {
  display: flex;
  font-size: 14px;
  color: #aaa;
}
.coords-col {
  justify-content: space-around;
  width: 560px;
  padding: 4px 0;
}
.coords-row {
  flex-direction: column;
  justify-content: space-around;
  height: 560px;
  padding: 0 8px;
}
.coord { width: 70px; text-align: center; line-height: 70px; }

/* Promotion modal */
#promotion-modal {
  display: none;
  position: fixed;
  top: 0; left: 0; right: 0; bottom: 0;
  background: rgba(0,0,0,0.7);
  z-index: 100;
  justify-content: center;
  align-items: center;
}
#promotion-modal.active { display: flex; }
#promotion-box {
  background: #16213e;
  border-radius: 16px;
  padding: 24px;
  text-align: center;
  box-shadow: 0 8px 30px rgba(0,0,0,0.5);
}
#promotion-box h3 { margin-bottom: 16px; color: #e94560; }
#promotion-choices {
  display: flex;
  gap: 12px;
  justify-content: center;
}
.promo-btn {
  width: 70px;
  height: 70px;
  font-size: 42px;
  border: 2px solid #444;
  border-radius: 12px;
  background: #1a1a2e;
  cursor: pointer;
  transition: all 0.2s;
  display: flex;
  align-items: center;
  justify-content: center;
}
.promo-btn:hover { border-color: #e94560; background: #e9456020; }
</style>
</head>
<body>

<div id="app">
  <div id="board-container">
    <div style="display:flex;align-items:stretch;">
      <div class="coords-row" id="row-coords"></div>
      <div>
        <div id="board"></div>
        <div class="coords-col" id="col-coords"></div>
      </div>
    </div>
  </div>
  <div id="info-panel">
    <h2>♚ Шахматы</h2>
    <div id="turn-indicator" class="turn-white">Ход белых</div>
    <div id="status"></div>
    <div class="captured-section">
      <div class="captured-label">Съедено белыми:</div>
      <div class="captured-pieces" id="captured-by-white"></div>
    </div>
    <div class="captured-section">
      <div class="captured-label">Съедено чёрными:</div>
      <div class="captured-pieces" id="captured-by-black"></div>
    </div>
    <button id="btn-new-game" onclick="newGame()">Новая игра</button>
  </div>
</div>

<div id="promotion-modal">
  <div id="promotion-box">
    <h3>Превращение пешки</h3>
    <div id="promotion-choices"></div>
  </div>
</div>

<script>
// ==================== GAME STATE ====================
let board = [];
let currentTurn = 'white';
let selectedCell = null;
let validMoves = [];
let lastMove = null;
let capturedByWhite = [];
let capturedByBlack = [];
let gameOver = false;
let enPassantTarget = null; // {row, col} — target square for en passant
let castlingRights = {
  white: { kingSide: true, queenSide: true },
  black: { kingSide: true, queenSide: true }
};
let promotionCallback = null;

const PIECES = {
  king:   { white: '♔', black: '♚' },
  queen:  { white: '♕', black: '♛' },
  rook:   { white: '♖', black: '♜' },
  bishop: { white: '♗', black: '♝' },
  knight: { white: '♘', black: '♞' },
  pawn:   { white: '♙', black: '♟' }
};

const PIECE_VALUES = { pawn: 1, knight: 3, bishop: 3, rook: 5, queen: 9, king: 0 };

// ==================== INITIALIZATION ====================
function newGame() {
  board = createInitialBoard();
  currentTurn = 'white';
  selectedCell = null;
  validMoves = [];
  lastMove = null;
  capturedByWhite = [];
  capturedByBlack = [];
  gameOver = false;
  enPassantTarget = null;
  castlingRights = {
    white: { kingSide: true, queenSide: true },
    black: { kingSide: true, queenSide: true }
  };
  document.getElementById('promotion-modal').classList.remove('active');
  renderBoard();
  updateInfo();
}

function createInitialBoard() {
  const b = Array.from({length:8}, () => Array(8).fill(null));
  const backRank = ['rook','knight','bishop','queen','king','bishop','knight','rook'];
  for (let c = 0; c < 8; c++) {
    b[0][c] = { type: backRank[c], color: 'black' };
    b[1][c] = { type: 'pawn', color: 'black' };
    b[6][c] = { type: 'pawn', color: 'white' };
    b[7][c] = { type: backRank[c], color: 'white' };
  }
  return b;
}

// ==================== RENDERING ====================
function renderBoard() {
  const boardEl = document.getElementById('board');
  boardEl.innerHTML = '';
  for (let r = 0; r < 8; r++) {
    for (let c = 0; c < 8; c++) {
      const cell = document.createElement('div');
      const isLight = (r + c) % 2 === 0;
      cell.className = 'cell ' + (isLight ? 'cell-light' : 'cell-dark');
      cell.dataset.row = r;
      cell.dataset.col = c;

      // Last move highlight
      if (lastMove) {
        if ((r === lastMove.fromRow && c === lastMove.fromCol) ||
            (r === lastMove.toRow && c === lastMove.toCol)) {
          cell.classList.add('last-move');
        }
      }

      // Selected cell
      if (selectedCell && selectedCell.row === r && selectedCell.col === c) {
        cell.classList.add('selected');
      }

      // Valid moves
      const vm = validMoves.find(m => m.row === r && m.col === c);
      if (vm) {
        if (board[r][c] || vm.enPassant) {
          cell.classList.add('valid-capture');
        } else {
          cell.classList.add('valid-move');
        }
      }

      // King in check highlight
      const piece = board[r][c];
      if (piece && piece.type === 'king' && isKingInCheck(piece.color)) {
        cell.classList.add('king-check');
      }

      // Piece
      if (piece) {
        cell.textContent = PIECES[piece.type][piece.color];
      }

      cell.addEventListener('click', () => onCellClick(r, c));
      boardEl.appendChild(cell);
    }
  }
  renderCoords();
}

function renderCoords() {
  const rowCoords = document.getElementById('row-coords');
  const colCoords = document.getElementById('col-coords');
  rowCoords.innerHTML = '';
  colCoords.innerHTML = '';
  for (let r = 0; r < 8; r++) {
    const d = document.createElement('div');
    d.className = 'coord';
    d.textContent = 8 - r;
    rowCoords.appendChild(d);
  }
  const cols = 'abcdefgh';
  for (let c = 0; c < 8; c++) {
    const d = document.createElement('div');
    d.className = 'coord';
    d.style.width = '70px';
    d.textContent = cols[c];
    colCoords.appendChild(d);
  }
}

function updateInfo() {
  const turnEl = document.getElementById('turn-indicator');
  turnEl.textContent = currentTurn === 'white' ? 'Ход белых' : 'Ход чёрных';
  turnEl.className = currentTurn === 'white' ? 'turn-white' : 'turn-black';

  const statusEl = document.getElementById('status');
  statusEl.className = '';
  statusEl.textContent = '';

  if (!gameOver) {
    if (isKingInCheck(currentTurn)) {
      if (isCheckmate(currentTurn)) {
        statusEl.textContent = (currentTurn === 'white' ? 'Чёрные' : 'Белые') + ' победили! Мат!';
        statusEl.className = 'status-checkmate';
        gameOver = true;
      } else {
        statusEl.textContent = 'Шах!';
        statusEl.className = 'status-check';
      }
    } else if (isStalemate(currentTurn)) {
      statusEl.textContent = 'Пат! Ничья.';
      statusEl.className = 'status-stalemate';
      gameOver = true;
    }
  }

  document.getElementById('captured-by-white').textContent =
    capturedByWhite.map(p => PIECES[p.type][p.color]).join(' ');
  document.getElementById('captured-by-black').textContent =
    capturedByBlack.map(p => PIECES[p.type][p.color]).join(' ');
}

// ==================== CLICK HANDLING ====================
function onCellClick(row, col) {
  if (gameOver) return;

  const piece = board[row][col];

  // If a valid move is clicked — make the move
  const vm = validMoves.find(m => m.row === row && m.col === col);
  if (selectedCell && vm) {
    makeMove(selectedCell.row, selectedCell.col, row, col, vm);
    selectedCell = null;
    validMoves = [];
    return;
  }

  // If clicking own piece — select it
  if (piece && piece.color === currentTurn) {
    selectedCell = { row, col };
    validMoves = getLegalMoves(row, col);
    renderBoard();
    return;
  }

  // Deselect
  selectedCell = null;
  validMoves = [];
  renderBoard();
}

// ==================== MOVE EXECUTION ====================
function makeMove(fromRow, fromCol, toRow, toCol, moveInfo) {
  const piece = board[fromRow][fromCol];
  const captured = board[toRow][toCol];

  // En passant capture
  if (moveInfo.enPassant) {
    const epPawnRow = piece.color === 'white' ? toRow + 1 : toRow - 1;
    const epPawn = board[epPawnRow][toCol];
    if (epPawn) {
      if (piece.color === 'white') capturedByWhite.push(epPawn);
      else capturedByBlack.push(epPawn);
    }
    board[epPawnRow][toCol] = null;
  }

  // Normal capture
  if (captured) {
    if (piece.color === 'white') capturedByWhite.push(captured);
    else capturedByBlack.push(captured);
  }

  // Castling
  if (moveInfo.castling) {
    if (moveInfo.castling === 'kingSide') {
      board[fromRow][5] = board[fromRow][7];
      board[fromRow][7] = null;
    } else {
      board[fromRow][3] = board[fromRow][0];
      board[fromRow][0] = null;
    }
  }

  // Move piece
  board[toRow][toCol] = piece;
  board[fromRow][fromCol] = null;

  // Update castling rights
  updateCastlingRights(piece, fromRow, fromCol, toRow, toCol);

  // En passant target
  if (piece.type === 'pawn' && Math.abs(toRow - fromRow) === 2) {
    enPassantTarget = { row: (fromRow + toRow) / 2, col: fromCol };
  } else {
    enPassantTarget = null;
  }

  // Pawn promotion
  if (piece.type === 'pawn' && (toRow === 0 || toRow === 7)) {
    showPromotionDialog(piece.color, toRow, toCol);
    lastMove = { fromRow, fromCol, toRow, toCol };
    renderBoard();
    updateInfo();
    return; // Turn switches after promotion choice
  }

  lastMove = { fromRow, fromCol, toRow, toCol };
  switchTurn();
  renderBoard();
  updateInfo();
}

function updateCastlingRights(piece, fromRow, fromCol, toRow, toCol) {
  // King moved
  if (piece.type === 'king') {
    castlingRights[piece.color].kingSide = false;
    castlingRights[piece.color].queenSide = false;
  }
  // Rook moved or captured
  if (piece.type === 'rook') {
    if (fromCol === 0) castlingRights[piece.color].queenSide = false;
    if (fromCol === 7) castlingRights[piece.color].kingSide = false;
  }
  // Rook captured
  const oppColor = piece.color === 'white' ? 'black' : 'white';
  if (toRow === 0 && toCol === 0) castlingRights[oppColor].queenSide = false;
  if (toRow === 0 && toCol === 7) castlingRights[oppColor].kingSide = false;
  if (toRow === 7 && toCol === 0) castlingRights[oppColor].queenSide = false;
  if (toRow === 7 && toCol === 7) castlingRights[oppColor].kingSide = false;
}

function switchTurn() {
  currentTurn = currentTurn === 'white' ? 'black' : 'white';
}

// ==================== PROMOTION ====================
function showPromotionDialog(color, row, col) {
  const modal = document.getElementById('promotion-modal');
  const choices = document.getElementById('promotion-choices');
  choices.innerHTML = '';
  const types = ['queen', 'rook', 'bishop', 'knight'];
  types.forEach(type => {
    const btn = document.createElement('button');
    btn.className = 'promo-btn';
    btn.textContent = PIECES[type][color];
    btn.addEventListener('click', () => {
      board[row][col] = { type, color };
      modal.classList.remove('active');
      switchTurn();
      renderBoard();
      updateInfo();
    });
    choices.appendChild(btn);
  });
  modal.classList.add('active');
}

// ==================== MOVE GENERATION ====================
function getPseudoLegalMoves(row, col) {
  const piece = board[row][col];
  if (!piece) return [];
  const moves = [];
  const color = piece.color;
  const opp = color === 'white' ? 'black' : 'white';

  function inBounds(r, c) { return r >= 0 && r < 8 && c >= 0 && c < 8; }
  function isEmpty(r, c) { return !board[r][c]; }
  function isEnemy(r, c) { return board[r][c] && board[r][c].color === opp; }
  function canMoveTo(r, c) { return inBounds(r, c) && (!board[r][c] || board[r][c].color === opp); }

  function addSlidingMoves(directions) {
    for (const [dr, dc] of directions) {
      let r = row + dr, c = col + dc;
      while (inBounds(r, c)) {
        if (isEmpty(r, c)) {
          moves.push({ row: r, col: c });
        } else if (isEnemy(r, c)) {
          moves.push({ row: r, col: c });
          break;
        } else {
          break;
        }
        r += dr; c += dc;
      }
    }
  }

  switch (piece.type) {
    case 'pawn': {
      const dir = color === 'white' ? -1 : 1;
      const startRow = color === 'white' ? 6 : 1;
      // Forward
      if (inBounds(row + dir, col) && isEmpty(row + dir, col)) {
        moves.push({ row: row + dir, col });
        // Double forward
        if (row === startRow && isEmpty(row + 2 * dir, col)) {
          moves.push({ row: row + 2 * dir, col });
        }
      }
      // Captures
      for (const dc of [-1, 1]) {
        const nr = row + dir, nc = col + dc;
        if (inBounds(nr, nc)) {
          if (isEnemy(nr, nc)) {
            moves.push({ row: nr, col: nc });
          }
          // En passant
          if (enPassantTarget && enPassantTarget.row === nr && enPassantTarget.col === nc) {
            moves.push({ row: nr, col: nc, enPassant: true });
          }
        }
      }
      break;
    }
    case 'knight': {
      const offsets = [[-2,-1],[-2,1],[-1,-2],[-1,2],[1,-2],[1,2],[2,-1],[2,1]];
      for (const [dr, dc] of offsets) {
        const nr = row + dr, nc = col + dc;
        if (canMoveTo(nr, nc)) moves.push({ row: nr, col: nc });
      }
      break;
    }
    case 'bishop':
      addSlidingMoves([[-1,-1],[-1,1],[1,-1],[1,1]]);
      break;
    case 'rook':
      addSlidingMoves([[-1,0],[1,0],[0,-1],[0,1]]);
      break;
    case 'queen':
      addSlidingMoves([[-1,-1],[-1,1],[1,-1],[1,1],[-1,0],[1,0],[0,-1],[0,1]]);
      break;
    case 'king': {
      const offsets = [[-1,-1],[-1,0],[-1,1],[0,-1],[0,1],[1,-1],[1,0],[1,1]];
      for (const [dr, dc] of offsets) {
        const nr = row + dr, nc = col + dc;
        if (canMoveTo(nr, nc)) moves.push({ row: nr, col: nc });
      }
      // Castling
      if (!isKingInCheck(color)) {
        const baseRow = color === 'white' ? 7 : 0;
        if (row === baseRow && col === 4) {
          // King side
          if (castlingRights[color].kingSide &&
              isEmpty(baseRow, 5) && isEmpty(baseRow, 6) &&
              !isSquareAttacked(baseRow, 5, opp) && !isSquareAttacked(baseRow, 6, opp)) {
            moves.push({ row: baseRow, col: 6, castling: 'kingSide' });
          }
          // Queen side
          if (castlingRights[color].queenSide &&
              isEmpty(baseRow, 3) && isEmpty(baseRow, 2) && isEmpty(baseRow, 1) &&
              !isSquareAttacked(baseRow, 3, opp) && !isSquareAttacked(baseRow, 2, opp)) {
            moves.push({ row: baseRow, col: 2, castling: 'queenSide' });
          }
        }
      }
      break;
    }
  }
  return moves;
}

function getLegalMoves(row, col) {
  const piece = board[row][col];
  if (!piece) return [];
  const pseudoMoves = getPseudoLegalMoves(row, col);
  return pseudoMoves.filter(move => {
    // Simulate the move and check if own king is in check
    const savedBoard = board.map(r => r.map(c => c ? {...c} : null));
    const savedEP = enPassantTarget ? {...enPassantTarget} : null;

    // Execute move on board
    const movingPiece = board[row][col];
    if (move.enPassant) {
      const epRow = movingPiece.color === 'white' ? move.row + 1 : move.row - 1;
      board[epRow][move.col] = null;
    }
    if (move.castling) {
      if (move.castling === 'kingSide') {
        board[row][5] = board[row][7];
        board[row][7] = null;
      } else {
        board[row][3] = board[row][0];
        board[row][0] = null;
      }
    }
    board[move.row][move.col] = movingPiece;
    board[row][col] = null;

    const inCheck = isKingInCheck(piece.color);

    // Restore board
    for (let r = 0; r < 8; r++) {
      for (let c = 0; c < 8; c++) {
        board[r][c] = savedBoard[r][c];
      }
    }
    enPassantTarget = savedEP;

    return !inCheck;
  });
}

// ==================== CHECK / CHECKMATE / STALEMATE ====================
function findKing(color) {
  for (let r = 0; r < 8; r++) {
    for (let c = 0; c < 8; c++) {
      if (board[r][c] && board[r][c].type === 'king' && board[r][c].color === color) {
        return { row: r, col: c };
      }
    }
  }
  return null;
}

function isSquareAttacked(row, col, byColor) {
  // Check if any piece of byColor attacks (row, col)
  for (let r = 0; r < 8; r++) {
    for (let c = 0; c < 8; c++) {
      const p = board[r][c];
      if (!p || p.color !== byColor) continue;
      const moves = getPseudoLegalMoves(r, c);
      // For pawns, only diagonal captures attack (not forward moves)
      if (p.type === 'pawn') {
        const dir = p.color === 'white' ? -1 : 1;
        if (r + dir === row && (c - 1 === col || c + 1 === col)) return true;
        continue;
      }
      // For king, only adjacent squares
      if (p.type === 'king') {
        if (Math.abs(r - row) <= 1 && Math.abs(c - col) <= 1) return true;
        continue;
      }
      if (moves.some(m => m.row === row && m.col === col)) return true;
    }
  }
  return false;
}

function isKingInCheck(color) {
  const king = findKing(color);
  if (!king) return false;
  const opp = color === 'white' ? 'black' : 'white';
  return isSquareAttacked(king.row, king.col, opp);
}

function hasAnyLegalMoves(color) {
  for (let r = 0; r < 8; r++) {
    for (let c = 0; c < 8; c++) {
      if (board[r][c] && board[r][c].color === color) {
        if (getLegalMoves(r, c).length > 0) return true;
      }
    }
  }
  return false;
}

function isCheckmate(color) {
  return isKingInCheck(color) && !hasAnyLegalMoves(color);
}

function isStalemate(color) {
  return !isKingInCheck(color) && !hasAnyLegalMoves(color);
}

// ==================== START ====================
newGame();
</script>
</body>
</html>
