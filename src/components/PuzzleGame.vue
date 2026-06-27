<template>
  <div class="game-container">
    <p class="game-intro">
      Trò chơi “Truy tìm mảnh ghép” bao gồm 4 bức tranh bí mật. Mỗi bức tranh
      được chia thành 4 mảnh ghép. Bạn cần lần lượt lật mở các mảnh ghép bằng
      cách trả lời các câu hỏi để khám phá toàn bộ bức tranh. Hãy cố gắng để lật
      mở được toàn bộ 4 bức tranh nhé!
    </p>

    <!-- Reset button -->
    <div class="reset-container">
      <button class="btn-reset" @click="resetGame" v-if="hasProgress">
        🔄 Đặt lại trò chơi
      </button>
    </div>

    <!-- 2x2 Grid of boards -->
    <div class="boards-grid">
      <div
        v-for="board in 4"
        :key="board"
        class="board-card"
        :class="{ completed: boardState[board].finalAnswered }"
      >
        <div class="board-header">
          <span class="board-title">Bức tranh #{{ board }}</span>
          <span class="board-progress">
            {{ boardState[board].revealed.filter(Boolean).length }}/4
            <span v-if="boardState[board].finalAnswered">✅</span>
          </span>
        </div>
        <div class="pieces-grid">
          <div
            v-for="piece in 4"
            :key="piece"
            class="piece"
            :class="{ revealed: boardState[board].revealed[piece - 1] }"
            @click="onPieceClick(board, piece - 1)"
            :style="getPieceStyle(board, piece - 1)"
          >
            <div
              v-if="!boardState[board].revealed[piece - 1]"
              class="piece-overlay"
            >
              <span class="piece-number">{{ piece }}</span>
              <span class="lock-icon">🔒</span>
            </div>
            <div v-else class="piece-revealed-check">
              <span class="check-icon">✓</span>
            </div>
          </div>
        </div>
        <div
          v-if="
            boardState[board].revealed.every(Boolean) &&
            !boardState[board].finalAnswered
          "
          class="board-final-hint"
        >
          <button class="btn-gold" @click.stop="openFinalQuestion(board)">
            Câu hỏi cuối cùng
          </button>
        </div>
        <div
          v-if="boardState[board].finalAnswered"
          class="board-complete-badge"
        >
          ✅ Hoàn thành
        </div>
      </div>
    </div>

    <!-- Large final image -->
    <div class="final-image-container" @click="onFinalClick">
      <div
        class="final-image"
        :style="{ backgroundImage: `url(${finalImage})` }"
      >
        <div class="final-overlay" v-if="!allBoardsCompleted">
          <span class="lock-icon">🔒</span>
          <span class="final-label">Mở khóa khi hoàn thành 4 bức tranh</span>
          <span class="final-progress">{{ completedBoards }}/4</span>
        </div>
        <div class="final-overlay" v-else>
          <span class="final-label"
            >🎉 Bấm để khám phá bức tranh cuối cùng</span
          >
        </div>
      </div>
    </div>

    <!-- Question Modal -->
    <QuestionModal
      v-model:visible="modalVisible"
      :title="modalTitle"
      :question="modalQuestion"
      :options="modalOptions"
      :correct-index="modalCorrectIndex"
      :explanation="modalExplanation"
      :is-final="modalIsFinal"
      :learn-more-link="modalLearnMoreLink"
      @answer="onModalAnswer"
      @close="closeModal"
      @continue="onModalContinue"
    />
  </div>
</template>

<script setup>
import { reactive, ref, computed, watch, onMounted } from "vue";
import { puzzleQuestions, finalQuestions } from "@/data/questionData";
import QuestionModal from "./QuestionModal.vue";

// ------ State ------
const STORAGE_KEY = "puzzle_progress";

const defaultState = {
  1: { revealed: [false, false, false, false], finalAnswered: false },
  2: { revealed: [false, false, false, false], finalAnswered: false },
  3: { revealed: [false, false, false, false], finalAnswered: false },
  4: { revealed: [false, false, false, false], finalAnswered: false },
};

const loadState = () => {
  const stored = localStorage.getItem(STORAGE_KEY);
  if (stored) {
    try {
      const parsed = JSON.parse(stored);
      for (const board of [1, 2, 3, 4]) {
        if (!parsed[board])
          parsed[board] = {
            revealed: [false, false, false, false],
            finalAnswered: false,
          };
        if (!parsed[board].revealed || parsed[board].revealed.length !== 4) {
          parsed[board].revealed = [false, false, false, false];
        }
      }
      return parsed;
    } catch (e) {
      return defaultState;
    }
  }
  return defaultState;
};

const boardState = reactive(loadState());

watch(
  boardState,
  (newState) => {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(newState));
  },
  { deep: true },
);

// ------ Computed ------
const allBoardsCompleted = computed(() => {
  for (const board of [1, 2, 3, 4]) {
    if (!boardState[board].finalAnswered) return false;
  }
  return true;
});

const completedBoards = computed(() => {
  let count = 0;
  for (const board of [1, 2, 3, 4]) {
    if (boardState[board].finalAnswered) count++;
  }
  return count;
});

// Check if there is any progress to reset
const hasProgress = computed(() => {
  for (const board of [1, 2, 3, 4]) {
    const state = boardState[board];
    if (state.revealed.some(Boolean) || state.finalAnswered) return true;
  }
  return false;
});

// Reset function
function resetGame() {
  if (!confirm("Bạn có chắc chắn muốn đặt lại toàn bộ tiến trình trò chơi?"))
    return;

  localStorage.removeItem(STORAGE_KEY);
  // Reset boardState to default
  const newState = {
    1: { revealed: [false, false, false, false], finalAnswered: false },
    2: { revealed: [false, false, false, false], finalAnswered: false },
    3: { revealed: [false, false, false, false], finalAnswered: false },
    4: { revealed: [false, false, false, false], finalAnswered: false },
  };
  Object.assign(boardState, newState);

  // Close any open modal
  modalVisible.value = false;
  currentBoard = null;
  currentPiece = null;
  isFinalMode = false;
}

// -------- IMAGE PATHS (FIXED) ----------
// Use encodeURI to handle spaces and special characters
// Place your images in public/museum_photos/Quiz/...
const boardImages = {
  1: encodeURI("/museum_photos/Quiz/Câu 1/Quiz Tên lửa SAM-2.png"),
  2: encodeURI("/museum_photos/Quiz/Câu 2/Q2 - MIG-17.png"),
  3: encodeURI("/museum_photos/Quiz/Câu 3/Quiz 3 - MIG 21.png"),
  4: encodeURI("/museum_photos/artifacts/1965 - 1975/Radar P35.png"),
};
const finalImage =
  "https://via.placeholder.com/600x400/195484/ffffff?text=Final+Image";

// ------ Modal state ------
const modalVisible = ref(false);
const modalTitle = ref("");
const modalQuestion = ref("");
const modalOptions = ref([]);
const modalCorrectIndex = ref(0);
const modalExplanation = ref("");
const modalIsFinal = ref(false);
const modalLearnMoreLink = ref("");

let currentBoard = null;
let currentPiece = null;
let isFinalMode = false;

// ------ Piece styling ------
function getPieceStyle(board, piece) {
  const imageUrl = boardImages[board] || "";
  let bgPos = "";
  if (piece === 0) bgPos = "0% 0%";
  else if (piece === 1) bgPos = "100% 0%";
  else if (piece === 2) bgPos = "0% 100%";
  else if (piece === 3) bgPos = "100% 100%";
  const blur = boardState[board].revealed[piece] ? "blur(0px)" : "blur(14px)";
  return {
    // Use encodeURI again in case the URL wasn't encoded previously
    backgroundImage: imageUrl ? `url(${imageUrl})` : "none",
    backgroundSize: "200% 200%",
    backgroundPosition: bgPos,
    filter: blur,
    transition: "filter 0.6s cubic-bezier(0.34, 1.56, 0.64, 1)",
    backgroundColor: "#e8e8e8", // fallback if image fails
  };
}

// ------ Click handlers ------
function onPieceClick(board, piece) {
  if (boardState[board].revealed[piece]) return;
  if (boardState[board].finalAnswered) return;
  openQuestion(board, piece);
}

function openQuestion(board, piece) {
  const key = `${board}-${piece}`;
  const q = puzzleQuestions[key];
  if (!q) return;

  currentBoard = board;
  currentPiece = piece;
  isFinalMode = false;
  modalTitle.value = `🧩 Mảnh ghép #${piece + 1}`;
  modalQuestion.value = q.q;
  modalOptions.value = q.options;
  modalCorrectIndex.value = q.correct;
  modalExplanation.value = q.explain;
  modalIsFinal.value = false;
  modalLearnMoreLink.value = q.link || "";
  modalVisible.value = true;
}

function openFinalQuestion(board) {
  const q = finalQuestions[board];
  if (!q) return;

  currentBoard = board;
  currentPiece = null;
  isFinalMode = true;
  modalTitle.value = `🎯 Câu hỏi cuối cùng - Bức tranh #${board}`;
  modalQuestion.value = q.q;
  modalOptions.value = q.options;
  modalCorrectIndex.value = q.correct;
  modalExplanation.value = q.explain;
  modalIsFinal.value = true;
  modalLearnMoreLink.value = q.link || "";
  modalVisible.value = true;
}

function onModalAnswer(selected) {
  // Answer recorded, but we handle reveal in onModalContinue
}

function onModalContinue() {
  if (isFinalMode) {
    if (currentBoard !== null) {
      boardState[currentBoard].finalAnswered = true;
    }
    modalVisible.value = false;
    currentBoard = null;
    currentPiece = null;
    isFinalMode = false;
    if (allBoardsCompleted.value) {
      // Optionally show a congratulation
    }
    return;
  }

  // Reveal the piece one by one
  if (currentBoard !== null && currentPiece !== null) {
    if (!boardState[currentBoard].revealed[currentPiece]) {
      boardState[currentBoard].revealed[currentPiece] = true;
      if (boardState[currentBoard].revealed.every(Boolean)) {
        setTimeout(() => {
          openFinalQuestion(currentBoard);
        }, 500);
      }
    }
  }
  modalVisible.value = false;
  currentBoard = null;
  currentPiece = null;
  isFinalMode = false;
}

function closeModal() {
  modalVisible.value = false;
  currentBoard = null;
  currentPiece = null;
  isFinalMode = false;
}

function onFinalClick() {
  if (!allBoardsCompleted.value) {
    alert(
      "Bạn cần hoàn thành tất cả 4 bức tranh trước khi mở khóa bức tranh cuối cùng!",
    );
    return;
  }
  alert(
    "🎉 Chúc mừng bạn đã hoàn thành toàn bộ thử thách! Bức tranh cuối cùng sẽ được hiển thị ở đây.",
  );
}

onMounted(() => {
  // Load state already done
});
</script>

<style scoped>
.game-container {
  background: var(--white);
  border-radius: var(--radius);
  padding: 30px;
  box-shadow: var(--shadow);
  margin: 20px 0;
}
.game-intro {
  font-size: 15px;
  color: var(--text-light);
  line-height: 1.8;
  margin-bottom: 24px;
  text-align: center;
}

.reset-container {
  text-align: center;
  margin-bottom: 16px;
}
.btn-reset {
  background: #c62828;
  color: white;
  border: none;
  padding: 6px 16px;
  border-radius: 20px;
  font-size: 13px;
  font-weight: 500;
  cursor: pointer;
  transition: var(--transition);
}
.btn-reset:hover {
  background: #b71c1c;
  transform: scale(1.05);
}

/* ... rest of styles (boards-grid, board-card, pieces-grid, etc.) ... */
.boards-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 24px;
  margin-bottom: 30px;
}
.board-card {
  background: var(--white);
  border-radius: var(--radius);
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  transition: var(--transition);
  border: 2px solid transparent;
}
.board-card.completed {
  border-color: var(--gold);
}
.board-card:hover:not(.completed) {
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.15);
}
.board-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 8px 12px;
  background: var(--cream);
  font-size: 14px;
  font-weight: 600;
  color: var(--primary-dark);
}
.board-title {
  font-family: var(--font-title);
  letter-spacing: 0.5px;
}
.board-progress {
  font-size: 13px;
  font-weight: 500;
}
.pieces-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: 1fr 1fr;
  aspect-ratio: 1/1;
  background: #e8e8e8;
}
.piece {
  position: relative;
  background-size: 200% 200%;
  cursor: pointer;
  border: 1px solid rgba(255, 255, 255, 0.3);
  overflow: hidden;
}
.piece.revealed {
  cursor: default;
}
.piece:not(.revealed):hover .piece-overlay {
  background: rgba(0, 0, 0, 0.4);
}
.piece:not(.revealed):hover {
  transform: scale(1.02);
  z-index: 2;
}
.piece-overlay {
  position: absolute;
  inset: 0;
  background: rgba(0, 0, 0, 0.35);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  color: white;
  transition: background 0.3s ease;
  pointer-events: none;
}
.piece-number {
  font-size: 28px;
  font-weight: 700;
  font-family: var(--font-title);
  text-shadow: 0 2px 8px rgba(0, 0, 0, 0.6);
}
.lock-icon {
  font-size: 20px;
  margin-top: 4px;
  opacity: 0.8;
}
.piece-revealed-check {
  position: absolute;
  bottom: 4px;
  right: 6px;
  font-size: 14px;
  color: rgba(46, 125, 50, 0.8);
  background: rgba(255, 255, 255, 0.7);
  border-radius: 50%;
  width: 22px;
  height: 22px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 700;
}
.board-final-hint {
  text-align: center;
  padding: 10px;
}
.board-final-hint .btn-gold {
  background: var(--gold);
  color: var(--primary-dark);
  padding: 6px 20px;
  border-radius: 30px;
  font-weight: 600;
  font-size: 13px;
  border: none;
  cursor: pointer;
  transition: var(--transition);
}
.board-final-hint .btn-gold:hover {
  background: var(--gold-light);
}
.board-complete-badge {
  text-align: center;
  padding: 8px;
  background: var(--primary-dark);
  color: white;
  font-weight: 600;
  font-size: 14px;
}
.final-image-container {
  border-radius: var(--radius);
  overflow: hidden;
  cursor: pointer;
  transition: var(--transition);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}
.final-image-container:hover {
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.15);
}
.final-image {
  width: 100%;
  aspect-ratio: 16/9;
  background-size: cover;
  background-position: center;
  position: relative;
}
.final-overlay {
  position: absolute;
  inset: 0;
  background: rgba(0, 0, 0, 0.6);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  color: var(--white);
  backdrop-filter: blur(6px);
  transition: var(--transition);
}
.final-overlay .lock-icon {
  font-size: 48px;
  margin-bottom: 8px;
}
.final-label {
  font-size: 18px;
  font-weight: 500;
  text-align: center;
  padding: 0 20px;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.5);
}
.final-progress {
  font-size: 14px;
  opacity: 0.8;
  margin-top: 6px;
}

@media (max-width: 768px) {
  .boards-grid {
    gap: 16px;
  }
  .piece-number {
    font-size: 20px;
  }
  .lock-icon {
    font-size: 16px;
  }
}
@media (max-width: 500px) {
  .boards-grid {
    grid-template-columns: 1fr 1fr;
    gap: 12px;
  }
  .board-header {
    font-size: 12px;
    padding: 6px 8px;
  }
  .game-container {
    padding: 16px;
  }
}
</style>
