# hiragana_katakana_flashcard

/* Card Flip Container */
.card-outer {
  perspective: 1000px;
  margin-bottom: 24px;
  min-height: 340px; 
  width: 100%;
  max-width: 600px;          
  margin-left: auto;
  margin-right: auto;
}

.card {
  background: white;
  border-radius: 16px;
  box-shadow: 0 10px 25px rgba(0,0,0,0.12);
  text-align: center;
  position: relative;
  transition: transform 0.6s ease;
  transform-style: preserve-3d;
  user-select: none;
  height: 340px;              
  min-height: 300px;
  display: flex;             
}

.card.flipped {
  transform: rotateY(180deg);
}

.card-front,
.card-back {
  position: absolute;
  inset: 0;                   
  backface-visibility: hidden;
  border-radius: 16px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 24px;
  box-sizing: border-box;
}

.card-front {
  background: white;
}

.card-back {
  background: #f9fafb;
  transform: rotateY(180deg);
}

/* Konten di dalam card */
.character {
  font-size: clamp(5rem, 18vw, 8rem);  
  margin: 8px 0;
  font-weight: bold;
  color: #2c3e50;
  line-height: 1;
}

.romaji {
  font-size: clamp(2.5rem, 10vw, 3.8rem);
  color: #e74c3c;
  margin: 8px 0;
  font-weight: 600;
}

.small-info {
  font-size: 1.1rem;
  color: #7f8c8d;
  margin-top: 12px;
  text-align: center;
  max-width: 90%;
}

/* Buttons area */
.buttons {
  display: flex;
  justify-content: center;
  gap: 16px;
  flex-wrap: wrap;
  margin: 24px 0;
}

button {
  padding: 12px 28px;
  font-size: 1.1rem;
  font-weight: 500;
  cursor: pointer;
  border: none;
  border-radius: 10px;
  background: #3498db;
  color: white;
  transition: all 0.22s ease;
  touch-action: manipulation;
}

button:hover:not(:disabled) {
  background: #2980b9;
  transform: translateY(-1px);
}

button:disabled {
  background: #95a5a6;
  cursor: not-allowed;
  transform: none;
}

.answer-btn {
  background: #27ae60;
}

.answer-btn:hover:not(:disabled) {
  background: #219653;
}

/* Quiz */
#quiz-area {
  margin-top: 24px;
}

.quiz-options {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(110px, 1fr));
  gap: 16px;
  margin-top: 20px;
  max-width: 600px;
  margin-left: auto;
  margin-right: auto;
}

.option-btn {
  padding: 18px 12px;
  font-size: clamp(1.6rem, 6vw, 2.1rem);
  background: #ecf0f1;
  color: #2c3e50;
  border: 2px solid #bdc3c7;
  border-radius: 12px;
  cursor: pointer;
  transition: all 0.18s ease;
  touch-action: manipulation;
}

.option-btn:hover:not(.correct):not(.wrong) {
  background: #d5dbdb;
  border-color: #95a5a6;
}

.correct {
  background: #2ecc71 !important;
  color: white !important;
  border-color: #27ae60 !important;
}

.wrong {
  background: #e74c3c !important;
  color: white !important;
  border-color: #c0392b !important;
}

/* Score */
#score-area {
  text-align: center;
  font-size: 1.35rem;
  font-weight: 500;
  margin: 16px 0;
  color: #2c3e50;
  line-height: 1.4;
}

/* Footer */
footer {
  text-align: center;
  margin-top: 48px;
  color: #7f8c8d;
  font-size: 0.95rem;
  padding-bottom: 20px;
}

/* Responsif kecil */
@media (max-width: 480px) {
  .card {
    padding: 16px;
    height: auto;
    min-height: 280px;
  }
  
  .character {
    font-size: clamp(4.5rem, 22vw, 6.5rem);
  }
  
  .romaji {
    font-size: clamp(2.2rem, 12vw, 3rem);
  }
  
  .buttons {
    gap: 12px;
  }
  
  button {
    padding: 10px 20px;
    font-size: 1rem;
  }
}
