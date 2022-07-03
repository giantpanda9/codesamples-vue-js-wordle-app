<template>
    <div class="WordleApp">
        <div class="wordle-stages-container">
            <div class="wordle-stage-row" v-for="(stage,stageKey) in stages" :key="stageKey">
                <div class="wordle-stage-item-letter" v-for="n in letterCount" :key="n"
                    :class = "{'wordle-status-input': stage.status[n-1] === 4,
                    'wordle-status-placeRight': stage.status[n-1] === 1,
                    'wordle-status-wordRight': stage.status[n-1] === 2,
                    'wordle-status-exclude': stage.status[n-1] === 3
                    }"
                >
                    {{ stage.letters[n-1] }}
                </div>
            </div>
        </div>
        <div class="wordle-info-message"
        :class = "{ 'wordle-info-visible': userMessage !== '',
         'wordle-info-hidden': userMessage === '',
         'wordle-info-message-error': messageCode===1,
         'wordle-info-message-win': messageCode===2,
         'wordle-info-message-lost': messageCode===3,
        }"
        >{{ userMessage }}  <button class="wordle-restart-game" @click="reStartGame" v-if="gameStatus === 1">Restart</button></div>
        <div class="wordle-keyboard-container">
            <ul class="wordle-keyboard-row">
                <li class="wrd-button-letter" v-for="(value,key) in alphapad" :key="key"
                :class = "{ 'wordle-status-placeRight': value.status === 1,
                'wordle-status-wordRight': value.status === 2,
                'wordle-status-exclude': value.status === 3,
                }" @click="inputController(key)"
                >{{ value.displayChar ? value.displayChar : key }}</li>
            </ul>
        </div>
    </div>
    <Instructions v-show="showInstructions" @closeHelpCall="closeHelp" />
</template>

<script>
import { watchEffect } from 'vue'
import { ref, reactive } from '@vue/reactivity'

import Instructions from './Instructions.vue'

import dictionary from "../dictionary/dictionary.json"
export default {
    name: 'WordleApp',
    components: {
        Instructions
    },
    // system methods not related to the game logic itself- display and close help for now
    // to demonstrate that I can work both with props and sections adn unified setup mode
    // and to organize the application a bit by the main subjects related to the logic
    methods: {
        closeHelp() {
            this.showInstructions = false
            document.body.classList.remove("wordle-body-noScroll")
        },
        showHelp() {
            this.showInstructions = true
            document.body.classList.add("wordle-body-noScroll")
        }
    },
    setup() {
        // game logic located here, other system tasks will be in methods
        // to demonstrate that I can work both with props and sections adn unified setup mode
        // and to organize the application a bit by the main subjects related to the logic
        const currentStage = ref(0)
        const letterCount = 5
        const guessedWord = ref("")
        const gameStatus = ref(0) // 0 - in progress, 1 - completed
        // status codes:
        // 0 - default,
        // letter place correct - 1
        // letter found in current word - 2
        // exclude letter - 3
        // input status for game board only - 4
        const alphapad = reactive({
            "q": {"status": 0},
            "w": {"status": 0},
            "e": {"status": 0},
            "r": {"status": 0},
            "t": {"status": 0},
            "y": {"status": 0},
            "u": {"status": 0},
            "i": {"status": 0},
            "o": {"status": 0},
            "p": {"status": 0},
            "a": {"status": 0},
            "s": {"status": 0},
            "d": {"status": 0},
            "f": {"status": 0},
            "g": {"status": 0},
            "h": {"status": 0},
            "j": {"status": 0},
            "k": {"status": 0},
            "l": {"status": 0},
            "enter": {},
            "z": {"status": 0},
            "x": {"status": 0},
            "c": {"status": 0},
            "v": {"status": 0},
            "b": {"status": 0},
            "n": {"status": 0},
            "m": {"status": 0},
            "bcsp": {displayChar: "\u232b"}
        })
        const stages = reactive([
            {
                letters: [],
                status: []
            },
            {
                letters: [],
                status: []
               
            },
            {
                letters: [],
                status: []
            },
            {
                letters: [],
                status: []
            },
            {
                letters: [],
                status: []
            },
            {
                letters: [],
                status: []
            }
        ])
        const userMessage = ref("")
        const messageCode = ref(0)
        const showInstructions = ref(false)
        function resetUserMessage() {
            userMessage.value = ""
            messageCode.value = 0
        }
        function inputLetter(symbol) {
            stages[currentStage.value].status.push(4)
            stages[currentStage.value].letters.push(symbol.toUpperCase())
        }
        function deleteSymbol() {
            stages[currentStage.value].status.pop()
            stages[currentStage.value].letters.pop()
        }
        function fillGameBoardStatuses() {
            stages[currentStage.value].status = stages[currentStage.value].status.fill(1,0,letterCount)
        }
        function fillKeyboardStatuses() {
            for (let i=0; i < stages[currentStage.value].letters.length; i++) {
                let inputKey = stages[currentStage.value].letters[i].toLowerCase()
                alphapad[inputKey].status=1
            }
        }
        function arbiterCheck() {
            let wordleCheck = guessedWord.value.toLowerCase().split('')
            // check for correct word positions
            for (let i=0; i < stages[currentStage.value].letters.length; i++) {
                let oursLetter = stages[currentStage.value].letters[i].toLowerCase()
                if (wordleCheck.includes(oursLetter)) {
                    if (alphapad[oursLetter].status === 0) alphapad[oursLetter].status = 2
                    if (stages[currentStage.value].status[i] === 0 || stages[currentStage.value].status[i] === 4) stages[currentStage.value].status[i] = 2
                }
                for (let j=0; j < wordleCheck.length; j++) {          
                    let theirsLetter = wordleCheck[j].toLowerCase()
                        if ((oursLetter === theirsLetter) && (i==j)) {
                            alphapad[oursLetter].status = 1
                            stages[currentStage.value].status[i] = 1
                        } else {
                            if (alphapad[oursLetter].status === 0) alphapad[oursLetter].status = 3
                            if (stages[currentStage.value].status[i] === 0 || stages[currentStage.value].status[i] === 4) stages[currentStage.value].status[i] = 3
                        }
                
                }
            }
        }
        function processInput() {
            if (stages[currentStage.value].letters.length !== letterCount) {
                userMessage.value = "The string is too short"
                messageCode.value = 1
                return
            } else if (!dictionary.includes(stages[currentStage.value].letters.join('').toLowerCase())) {
                userMessage.value = "The string was not found in dictionary"
                messageCode.value = 1
                return
            } else if (stages[currentStage.value].letters.join('').toLowerCase() === guessedWord.value.toLowerCase()) {
                fillGameBoardStatuses()
                fillKeyboardStatuses()
                userMessage.value = "You won! The word is '" + guessedWord.value + "'"
                messageCode.value = 2
                gameStatus.value = 1
                return
            } else {
                arbiterCheck()
                currentStage.value = currentStage.value + 1
                if (currentStage.value === 6) {
                    userMessage.value = "You lost! The word is '" + guessedWord.value + "'"
                    messageCode.value = 3
                    gameStatus.value = 1
                }
            }

        }
        function inputController(symbol) {
            if (gameStatus.value === 1) return
            resetUserMessage()
            if (symbol.toLowerCase() === "enter") {
                processInput()
            } else if (symbol.toLowerCase() === "bcsp") {
                deleteSymbol()
            } else {
                inputLetter(symbol)
            }
        }
        function startGame() {
            let dictionaryLength = dictionary.length - 1
            let wordNumber = Math.floor(Math.random() * dictionaryLength)
            guessedWord.value = dictionary[wordNumber]
            gameStatus.value = 0
        }
        function reStartGame() {
            for (let i=0; i <= letterCount; i++) {
                for (let j=0; j < stages[i].letters.length; j++) {
                    stages[i].letters = []
                    stages[i].status = []
                }
            }
            let keyboardKeys;
            watchEffect(() => {
                keyboardKeys = Object.keys(alphapad);
            });
            for (let i=0; i < keyboardKeys.length; i++) {
                let currentKey = keyboardKeys[i].toLowerCase()
                alphapad[currentKey].status = 0
            }
            currentStage.value = 0
            userMessage.value = ""
            messageCode.value = 0
            guessedWord.value = ""
            gameStatus.value = 0
            startGame()
        }
        return {
            alphapad,
            stages,
            currentStage,
            inputController,
            startGame,
            letterCount,
            userMessage,
            messageCode,
            gameStatus,
            reStartGame,
            Instructions,
            showInstructions
        }
    },
    mounted() {
        this.startGame() // this comes from game logis / setup() section
        this.showHelp() // this comes from methods section
    }
}
</script>

<style>
.wordle-body-noScroll {
    overflow: hidden;
}
</style>
<style scoped>
.wordle-keyboard-container {
    margin-left: auto;
    margin-right: auto;
    margin-top: -110px;
    margin-bottom: 0px;
    width: 750px;
    -webkit-touch-callout: none;
    -webkit-user-select: none;
    -khtml-user-select: none;
    -moz-user-select: none; 
    -ms-user-select: none; 
    user-select: none;
}
.wordle-keyboard-row {
    margin: 114px;
    padding: 0;
    list-style: none;
}
.wrd-button-letter {
    display: inline-block;
    margin: 0.25rem 0.125rem;
    padding: 0.5rem;
    width: 30px;
    height: 40px;
    line-height: 2.6em;
    text-align: center;
    text-transform: uppercase;
    font-weight: 700;
    font-size: 15px;
    border: 1px solid #f9f9f9;
    -moz-border-radius: 5px;
    -webkit-border-radius: 5px;
    cursor: pointer;
    background-color: #d3d6db;
    text-transform: uppercase;
    color: #353535;
}
.wrd-button-letter:nth-child(11) {
    margin-left: 5px;
}
.wrd-button-letter:nth-child(20) {
    width: 50px;
}
.wrd-button-letter:nth-child(28) {
    width: 60px;
}
.wordle-stages-container {
    margin-left: auto;
    margin-right: auto;
    margin-top: -55px;
    margin-bottom: 0px;
    width: 350px;
    -webkit-touch-callout: none;
    -webkit-user-select: none;
    -khtml-user-select: none;
    -moz-user-select: none; 
    -ms-user-select: none; 
    user-select: none;
}
.wordle-stage-item-letter {
    float: left;
    margin: 0 5px 5px 0;
    width: 50px;
    height: 50px;
    line-height: 52px;
    text-align: center;
    background: #fff;
    border: 2px solid #d3d6db;
    font-color: black;
    font-weight: 700;
}
.wordle-stage-row {
    margin: 0;
    padding: 0;
    display: inline-block;
}
.wordle-status-input {
    border-color: #69748f;
}
.wordle-status-placeRight {
    border-color: #d3d6db;
    background-color: #6baa64;
    color: white;
}
.wordle-status-wordRight {
    border-color: #d3d6db;
    background-color: #f3c237;
    color: white;
}
.wordle-status-exclude {
    border-color: #d3d6db;
    background-color: #787c7f;
    color: white;
}
.wordle-info-message {
    height: 25px;
    width: 380px;
    text-align: center;
    background-color: #d3d6db;
    border: 1px solid #353535;
     -moz-border-radius: 5px;
    -webkit-border-radius: 5px;
    margin-left: auto;
    margin-right: auto;
    font-weight: 700;
    line-height: 23px;
}
.wordle-info-message-error {
    color: red;
}
.wordle-info-message-win {
    color: green;
}
.wordle-info-message-lost {
    color: red;
}
.wordle-restart-game {
    font-weight: 700;
    text-decoration: underline;
    color: black;
}
/* Reserve space for inline element */
.wordle-info-visible {
    visibility: visible;
}
.wordle-info-hidden {
    visibility: hidden;
}
</style>
