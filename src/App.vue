<template>
  <v-app>
    <v-row
      v-if="!isHidden"
      class="mt-16"
    >
      <v-col
        md="6"
        offset-md="3"
      >
        <v-text-field
          v-model="wordToGuess"
          rounded
          outlined
          label="Your word to guess"
          placeholder="Write sth ¯\_(ツ)_/¯"
          maxlength="16"
          counter
        />
        <v-btn
          height="50"
          width="660"
          elevation="2"
          rounded
          color="green lighten-1"
          @click="play"
        >
          Play!
        </v-btn>
        <v-row
          class="mt-16"
        >
          <v-col
            md="10"
            offset-md="1"
          >
            <v-autocomplete
              v-model="wordToGuess"
              label="Choose word form list"
              class="mt-2"
              clearable
              rounded
              solo
              :items="wordList"
            />
            <v-card
              class="mt-16"
            >
              <v-card-title
                class="justify-center"
              >
                COMING SOON:
              </v-card-title>
              <v-card-text>
                <li>Choosing word from list</li>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-col>
    </v-row>
    <v-row
      v-if="isHidden && !gameOver"
      class="mt-5"
    >
      <v-col
        md="10"
        offset-md="1"
      >
        <v-card
          height="250"
          outlined
        >
          <v-container
            fluid
            fill-height
          >
            <v-row
              justify="center"
            >
              <v-col
                v-for="(literka, index) in wordsLetters"
                :key="index"
                md="auto"
              >
                <v-card
                  id="letterCard"
                  class="mt-2"
                  elevation="1"
                  min-height="65"
                  :min-width="literka === ' ' ? 20 : 50"
                  :color="literka === ' ' ? '#EEEEEE' : 'white'"
                >
                  <v-card-title
                    v-if="selectedCorrect.includes(literka)"
                    class="justify-center"
                  >
                    {{ literka }}
                  </v-card-title>
                </v-card>
              </v-col>
            </v-row>
          </v-container>
        </v-card>
        <v-row
          class="mt-4"
        >
          <v-col
            class="col-4"
          >
            <v-card
              elevation="2"
              outlined
              height="400"
            >
              <img
                :src="hangmanPicture"
                :height="400"
              >
            </v-card>
          </v-col>
          <v-col
            class="col"
          >
            <v-card
              elevation="2"
              outlined
              height="400"
            >
              <v-container
                fluid
                fill-height
              >
                <v-row
                  justify="center"
                  align="center"
                >
                  <v-col
                    v-for="(letter, index) in alphabet"
                    :key="index"
                    md="auto"
                  >
                    <v-btn
                      elevation="2"
                      rounded
                      fab
                      :disabled="selectedCorrect.includes(letter) || selectedIncorrect.includes(letter)"
                      @click="checkLetter(letter)"
                    >
                      {{ letter }}
                    </v-btn>
                  </v-col>
                </v-row>
              </v-container>
            </v-card>
          </v-col>
        </v-row>
      </v-col>
    </v-row>
    <v-row
      v-if="isHidden && gameOver"
      class="mt-16"
    >
      <v-col
        md="10"
        offset-md="5"
      >
        <v-card
          width="200"
          class="mt-16"
          elevation="3"
        >
          <v-card-title
            class="justify-center"
          >
            {{ text }}
          </v-card-title>
        </v-card>
      </v-col>
    </v-row>
    <v-footer padless>
      <v-col
        class="text-center"
        cols="12"
      >
        <strong>2020 - {{ new Date().getFullYear() }}</strong> — ©Copyright, All rights reserved
      </v-col>
    </v-footer>
  </v-app>
</template>

<script>

export default {
  name: 'App',
  components: {

  },

  data: () => ({
    wordToGuess: "",
    gameOver: false,
    isHidden: false,
    wordsLetters: [],
    alphabet: ['A','B','C','D','E','F','G','H','I','J','K','L','M','N','O','P','Q','R','S','T',
      'U','V','W','X','Y','Z'],
    selectedCorrect: [],
    selectedIncorrect: [],
    hangmanPicture: "/HM01.png",
    index: 0,
    text: "",
    wordList: [],
  }),

  methods: {
    play() {
      if (this.wordToGuess.length !== 0) {
        this.isHidden = true;
        this.wordsLetters = this.wordToGuess.toUpperCase().split("");
        if (!this.wordList.includes(this.wordToGuess)) {
          this.wordList.push(this.wordToGuess);
        }
      }
    },
    checkLetter(letter) {
      if (this.wordsLetters.includes(letter)) {
        this.selectedCorrect.push(letter);
        //this.selectedCorrect.filter(letterInArray => !this.wordsLetters.includes(letterInArray))
        if(this.wordsLetters.filter(
            (letterInArray) => this.selectedCorrect.includes(letterInArray)
                || letterInArray === ' ').length === this.wordsLetters.length
        ) {
          this.selectedCorrect.push('A','B','C','D','E','F','G','H','I','J','K','L','M','N','O','P','Q','R','S','T',
              'U','V','W','X','Y','Z');
          setTimeout(() => {
            this.text = "You won!";
            this.gameOver = true;
            this.index = 0;
            this.wordToGuess = "";
            this.selectedCorrect = [];
            this.selectedIncorrect = [];
            this.hangmanPicture = "/HM01.png";
          }, 1000);
          setTimeout(() => {
            this.isHidden = false;
            this.gameOver = false;
            }, 3000);

        }
      } else {
        this.drawHangman();
        this.selectedIncorrect.push(letter);
      }
    },
    drawHangman() {
      this.index ++;
      switch (this.index) {
        case 1:
          this.hangmanPicture = "/HM02.png";
          break;
        case 2:
          this.hangmanPicture = "/HM03.png";
          break;
        case 3:
          this.hangmanPicture = "/HM04.png";
          break;
        case 4:
          this.hangmanPicture = "/HM05.png";
          break;
        case 5:
          this.hangmanPicture = "/HM06.png";
          break;
        case 6:
          this.hangmanPicture = "/HM07.png";
          break;
        case 0:
          this.hangmanPicture = "/HM01.png";
          break;
      }
      if (this.index === 6) {
        this.selectedCorrect.push('A','B','C','D','E','F','G','H','I','J','K','L','M','N','O','P','Q','R','S','T',
            'U','V','W','X','Y','Z');
        setTimeout(() => {
          this.text = "You lose!";
          this.gameOver = true;
        }, 1000);
        setTimeout(() => {
          this.hangmanPicture = "/HM01.png";
          this.index = 0;
          this.wordToGuess = "";
          this.selectedCorrect = [];
          this.selectedIncorrect = [];
          this.isHidden = false;
          this.gameOver = false;
        }, 3000);

      }
    },
  },
};
</script>
<style>
#letterCard {
  border-radius: 20px;
}
</style>