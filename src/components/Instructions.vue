<template>
    <div class="modal-overlay" id="instructionsWordle">
        <div class="modal">
        <div class="modal-close-button" @click="closeInstructions()">&#10799;</div>
        <div class="modal-help-define-paragraph modal-help-define-font modal-help-guess-text">Guess the <span class="modal-help-define-bold-font">WORDLE</span> in six tries.</div>
        <div class="modal-help-define-paragraph modal-help-define-font modal-help-text">Each guess must be a valid five-letter word. Hit the enter button to submit.</div>
        <div class="modal-help-define-paragraph modal-help-define-font modal-help-text">After each guess, the color of the tiles will change to show how close your guess was to the word.</div>
        <hr class="modal-help-hr">
        <div class="modal-help-define-paragraph modal-help-define-font modal-help-text modal-help-define-bold-font">Examples</div>
        <div class="wordle-info-row">
            <div class="wordle-info-item-letter wordle-info-status-placeRight">W</div>
            <div class="wordle-info-item-letter wordle-info-status-default">E</div>
            <div class="wordle-info-item-letter wordle-info-status-default">A</div>
            <div class="wordle-info-item-letter wordle-info-status-default">R</div>
            <div class="wordle-info-item-letter wordle-info-status-default">Y</div>
        </div>
        <div class="modal-help-define-paragraph modal-help-define-font modal-help-text">The letter <span class="modal-help-define-bold-font">W</span> is in the word and in the correct spot.</div>
        <div class="wordle-info-row">
            <div class="wordle-info-item-letter wordle-info-status-default">P</div>
            <div class="wordle-info-item-letter wordle-info-status-wordRight">I</div>
            <div class="wordle-info-item-letter wordle-info-status-default">L</div>
            <div class="wordle-info-item-letter wordle-info-status-default">L</div>
            <div class="wordle-info-item-letter wordle-info-status-default">S</div>
        </div>
        <div class="modal-help-define-paragraph modal-help-define-font modal-help-text">The letter <span class="modal-help-define-bold-font">I</span> is in the word but in the wrong spot.</div>
        <div class="wordle-info-row">
            <div class="wordle-info-item-letter wordle-info-status-default">V</div>
            <div class="wordle-info-item-letter wordle-info-status-default">A</div>
            <div class="wordle-info-item-letter wordle-info-status-default">G</div>
            <div class="wordle-info-item-letter wordle-info-status-exclude">U</div>
            <div class="wordle-info-item-letter wordle-info-status-default">E</div>
        </div>
        <div class="modal-help-define-paragraph modal-help-define-font modal-help-text">The letter <span class="modal-help-define-bold-font">U</span> is not in the word in any spot.</div>
        <hr class="modal-help-hr">
        <div class="modal-help-define-paragraph modal-help-define-font modal-help-text modal-help-define-bold-font">A new WORDLE will be available each day!</div>
    </div>
  </div>
</template>
<script>

export default {
    name: "WordleModal",
    methods: {
        closeInstructions() {
            this.$emit("closeHelpCall")
        }
    }
}
</script>

<style scoped>
.modal-overlay {
    position: fixed;
    display: flex;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    justify-content: center;
    background-color: #00000000;
}
.modal-show {
    display: flex;
}
.modal-hide {
    display: none;
}
.modal {
    text-align: center;
    background-color: white;
    height: 560px;
    width: 490px;
    margin-top: 1%;
    border-radius: 7px;
    -webkit-box-shadow: 4px 4px 8px 0px rgba(34, 60, 80, 0.2);
    -moz-box-shadow: 4px 4px 8px 0px rgba(34, 60, 80, 0.2);
    box-shadow: 4px 4px 8px 0px rgba(34, 60, 80, 0.2);
}
.modal-close-button {
    background: none;
    color: #2f2f2f;
    border: none;
    padding-top: 15px;
    padding-right: 15px;
    font-size: 20px;
    font-weight: 600;
    cursor: pointer;
    position: relative;
    float: right;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;   
}
.modal-help-define-font {
    font-size: 14px;
    color: #120100;
}
.modal-help-define-bold-font {
    font-weight: 700;
}
.modal-help-define-paragraph {
    text-align: left;
    margin-bottom: 15px;
}
.modal-help-guess-text {
    margin-top: 3em;
    margin-left: 1em;
}
.modal-help-text {
    margin-top: 0.5em;
    margin-left: 1em;
}
.modal-help-hr {
    margin-left: 1em;
    margin-right: 1em;
}
.wordle-info-row {
    margin: 0;
    padding: 0;
    margin-left: 15px;
    width: 295px;
    margin-top: 1.5em;
}
.wordle-info-item-letter {
    display: inline-block;
    margin: 0 5px 5px 0;
    width: 50px;
    height: 50px;
    line-height: 52px;
    text-align: center;
    background: #fff;
    border: 2px solid #d3d6db;
    font-weight: 700;
}
.wordle-info-status-input {
    border-color: #69748f;
}
.wordle-info-status-placeRight {
    border-color: #d3d6db;
    background-color: #6baa64;
    color: white;
}
.wordle-info-status-wordRight {
    border-color: #d3d6db;
    background-color: #f3c237;
    color: white;
}
.wordle-info-status-exclude {
    border-color: #d3d6db;
    background-color: #787c7f;
    color: white;
}
</style>
