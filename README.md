# codesamples-vue-js-wordle-app
A code example of Wordle Application in universally accepted design. 
Modern npm is required, at least Node.js version 12.x to 14.x

# Installation
1) git clone https://github.com/giantpanda9/codesamples-vue-js-wordle-app.git
2) cd codesamples-vue-js-wordle-app
3) npm install
4) npm run serve
5) direct you browser to http://localhost:8080/ and you should notice the application and instructions
6) Keyboard and popup window modules are self-written to properly adapt them to the task in hand
7) Modal window with rules appears when page load and re-load, when the user press Restart button, Vue3 simply clean up the variables used in a previous game session

# The questions that could be asked on the first place for this code (FAQ based on probability):

 ## Q: Why there are some deviations from the initial design? 
 **A:** Although I tried to follow the proposed design as precise as possible, there few minor deviations, which could easily be fixed, if needed and required specifications provided:
 
 - colors are brighter a little than in PDF as there are lots of very similar Wordle games in the Internet, so, to proof that this one was written by myself a small color variations are accepted
 
 - the keyboard is a little closer to the gameboard to allow a comfort gameplay on 1366x768 notebook screens - this comfort resoultion for the game is expanded
 
 - when instructions window appears the document scrolling is blocked - this is required to avoid the screen flickering under the the modal window, when user accidnetally touch mouse wheel during the intructions reading


## Q: Why the implementation is based on two Vue.js techniques - setup() implementation and classic divided to sections implementation?
 **A:** Since, this is a demo application, i thought I need to demonstrate that I can work with both styles of Vue.js programming. I'd also logically devided the functional - the main game logic is imported from setup() - Vue 3 new style - and the system functions like displaying a window are moved into methods: section

## Q: Why dictionary is located in *.json file?
**A:** The PDF with instructions does not provide any specific requirements for the dictionary file and the example is not properly loaded and not being displayed, however, when the Vue3 SPA implemented via CLI mode (as seprate modules) the textual data is accustomed to be in the *.json files
  
## Q: Why dictionary is limited to several words only?
**A:** The amount of words included into the dictioary.json files allows proper development testing of the functions of the game. More to it, I have no access to the library of 5 letter words, which may be copyrighted, etc.

## Q: Why modal window content is located in Vue3 component?
**A:** Again, the implementation of popup window is not mentioned in the PDF or not properly loaded, so a took a liberty to follow the rules of standalone Vue3 modules and created a Modal window component for the instructions on screen
