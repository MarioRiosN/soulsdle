<template>
    <div id="header" class="border-bottom border-opacity-75">
        <div></div>
        <div id="title">
            <img class="blueSoul" src="../assets/blueSoul.png" alt="" style="margin-left: 50px;">
            <h1>Soulsdle</h1>
            <img class="blueSoul" src="../assets/blueSoul.png" alt="">
        </div>
        <div id="help" data-bs-toggle="modal" data-bs-target="#staticBackdrop">
            <img id="helpIcon" class="img-fluid" src="../assets/question_2601675.png" alt="">
        </div>
    </div>
    <!-- <hr> -->
    <div id="contents">
        <div id="board" class="align-middle"></div>
        <div id="kb" class="align-middle">
            <div class="row1">
                <button class="kb-button">q</button>
                <button class="kb-button">w</button>
                <button class="kb-button">e</button>
                <button class="kb-button">r</button>
                <button class="kb-button">t</button>
                <button class="kb-button">y</button>
                <button class="kb-button">u</button>
                <button class="kb-button">i</button>
                <button class="kb-button">o</button>
                <button class="kb-button">p</button>
            </div>
            <div class="row2">
                <button class="kb-button">a</button>
                <button class="kb-button">s</button>
                <button class="kb-button">d</button>
                <button class="kb-button">f</button>
                <button class="kb-button">g</button>
                <button class="kb-button">h</button>
                <button class="kb-button">j</button>
                <button class="kb-button">k</button>
                <button class="kb-button">l</button>
            </div>
            <div class="row3">
                <button class="kb-button">Del</button>
                <button class="kb-button">z</button>
                <button class="kb-button">x</button>
                <button class="kb-button">c</button>
                <button class="kb-button">v</button>
                <button class="kb-button">b</button>
                <button class="kb-button">n</button>
                <button class="kb-button">m</button>
                <button class="kb-button">Enter</button>
            </div>
        </div>
    </div>

    <div class="modal fade" id="staticBackdrop" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1"
        aria-labelledby="staticBackdropLabel" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content">
                <div class="modal-header">
                    <h1 class="modal-title fs-5" id="staticBackdropLabel">Modal title</h1>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    ...
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                    <button type="button" class="btn btn-primary">Understood</button>
                </div>
            </div>
        </div>
    </div>
</template>

<style src="./styles.css"></style>

<script>
export default {
    name: 'v-game',
    data() {
        return {
            tries: 6,
            words: [],
            triesRemaining: 6,
            currentTry: [],
            nextLetter: 0,
            pressedKey: '',
            answer: ''
        }
    },
    methods: {
        add(pressedKey) {
            if (this.nextLetter === 5) {
                return 0;
            }
            pressedKey = pressedKey.toLowerCase();
            let row = document.getElementsByClassName("letter-row")[6 - this.triesRemaining];
            let box = row.children[this.nextLetter];
            box.textContent = pressedKey;
            box.classList.add("filled-box");
            this.currentTry.push(pressedKey);
            this.nextLetter += 1;
        },
        del() {
            let row = document.getElementsByClassName("letter-row")[6 - this.triesRemaining];
            let box = row.children[this.nextLetter - 1];
            box.textContent = "";
            box.classList.remove("filled-box");
            this.currentTry.pop();
            this.nextLetter -= 1;
        },
        shadeKeyBoard(letter, color) {
            for (const elem of document.getElementsByClassName("kb-button")) {
                if (elem.textContent === letter) {
                    let oldColor = elem.style.backgroundColor;
                    if (oldColor === 'green') {
                        return 0;
                    }
                    if (oldColor === 'yellow' && color !== 'green') {
                        return 0;
                    }
                    elem.style.backgroundColor = color;
                    break;
                }
            }
        },
        check() {
            let row = document.getElementsByClassName("letter-row")[6 - this.triesRemaining];
            let guessString = '';
            let rightGuess = Array.from(this.answer);
            for (const val of this.currentTry) {
                guessString += val;
            }
            if (guessString.length != 5) {
                alert("Only 5 letter words");
                return 0;
            }
            if (!this.words.includes(guessString)) {
                alert("Word not found!");
                return 0;
            }
            for (let i = 0; i < 5; i++) {
                let letterColor = '';
                let box = row.children[i];
                let letter = this.currentTry[i];
                let letterPosition = rightGuess.indexOf(this.currentTry[i]);
                if (letterPosition === -1) {
                    letterColor = 'grey';
                } else {
                    if (this.currentTry[i] === rightGuess[i]) {
                        letterColor = 'green';
                    } else {
                        letterColor = 'yellow';
                    }
                    rightGuess[letterPosition] = "#";
                }
                box.style.backgroundColor = letterColor;
                this.shadeKeyBoard(letter, letterColor);
            }
            if (guessString === this.answer) {
                alert("You guessed the word!");
                this.triesRemaining = 0;
                return 0;
            } else {
                this.triesRemaining -= 1;
                this.currentTry = [];
                this.nextLetter = 0;
                if (this.triesRemaining === 0) {
                    alert(`Out of attemps. The answer was: "${this.answer}"`);
                }
            }
        }
    },
    mounted() {
        let board = document.getElementById("board");
        for (let i = 0; i < this.tries; i++) {
            let row = document.createElement("div");
            row.className = "letter-row";
            for (let j = 0; j < 5; j++) {
                let box = document.createElement("div");
                box.className = "letter-box";
                row.appendChild(box);
            }
            board.appendChild(row);
        }
        this.words =
            ['demon', 'selen', 'yuria', 'freke', 'doran', 'blige', 'biorr', 'andre', 'logan', 'giant', 'witch',
                'gough', 'carim', 'swamp', 'souls', 'oscar', 'shiva', 'vamos', 'velka', 'vince', 'yulva',
                'drake', 'aldia', 'curse', 'ashen', 'alken', 'lords', 'tillo', 'grave', 'licia', 'eleum', 'loyce',
                'ashes', 'sword', 'yhorm', 'slave', 'shira', 'queen', 'pygmy', 'karla', 'irina', 'havel', 'drang', 'eygon', 'kamui',
                'yurie', 'viola', 'valtr', 'simon', 'jozef', 'blood', 'antal', 'dores', 'edgar', 'maria', 'mergo', 'oedon',
                'vitus', 'djura', 'shura', 'tengu', 'ensha', 'elden', 'beast', 'crone', 'morne', 'okina', 'albus',
                'pidia', 'lusat', 'order', 'ranni', 'gowry', 'ofnir', 'thops', 'trina', 'rosus', 'varre', 'golem',
                'flame', 'manus', 'estoc', 'spear', 'iaito', 'chaos', 'abyss', 'blade', 'knife', 'grant', 'londo',
                'chasm', 'amana', 'crypt', 'ihyll', 'loran', 'dream', 'ruins', 'renna', 'altus', 'tower', 'human',
                'farum', 'azula', 'irith', 'elleh', 'faith', 'poise', 'bleed', 'magic', 'skill', 'crown', 'scale',
                'arrow', 'death', 'steel', 'favor', 'light', 'raime', 'royal', 'heide', 'thief', 'rogue', 'estus'];
        this.answer = this.words[Math.floor(Math.random() * this.words.length)];
        document.addEventListener("keyup", (e) => {
            if (this.triesRemaining === 0) {
                return 0;
            }
            this.pressedKey = String(e.key);
            if (this.pressedKey === "Backspace" && this.nextLetter !== 0) {
                this.del();
                return 0;
            }
            if (this.pressedKey === "Enter") {
                this.check();
                return 0;
            }
            let found = this.pressedKey.match(/[a-z]/gi);
            if (!found || found.length > 1) {
                return 0;
            } else {
                this.add(this.pressedKey);
            }
        })
        document.getElementById("kb").addEventListener("click", (e) => {
            const target = e.target;
            if (!target.classList.contains("kb-button")) {
                return 0;
            }
            let key = target.textContent;
            if (key === "Del") {
                key = "Backspace";
            }
            document.dispatchEvent(new KeyboardEvent("keyup", { 'key': key }));
        })
    }
}
</script>