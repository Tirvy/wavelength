<template>
  <v-app id="inspire">
    <v-main>
      <v-spacer />

      <v-container>
        <v-stepper :value="currentStepIndex + 1">
          <v-stepper-header>
            <template v-for="(item, index) in steps">
              <v-divider :key="index" v-if="index > 0"></v-divider>

              <v-stepper-step
                :key="item.name"
                :step="index + 1"
                :complete="index < currentStepIndex"
                :color="colors[item.color]"
              >
                {{ item.name }}
                <small>{{ item.description }}</small>
              </v-stepper-step>
            </template>
          </v-stepper-header>
        </v-stepper>
      </v-container>

      <v-spacer />

      <v-container>
        <v-row>
          <v-col :color="teamColors[0]">
            <v-card outlined :color="teamColors[0]">
              <v-card-title> {{ teamColors[0] }} team </v-card-title>
              <v-list>
                <v-list-item v-for="item in teamUsers[0]" :key="item">
                  <v-list-item-content>
                    <v-list-item-title v-text="item"></v-list-item-title>
                  </v-list-item-content>
                </v-list-item>
              </v-list>
            </v-card>
          </v-col>
          <v-col cols="8">
            <v-card>
              <v-row justify="space-between">
                <v-col>
                  {{ roundInfo.concepts[0] }}
                </v-col>
                <v-spacer />
                <v-col class="text-right">
                  {{ roundInfo.concepts[1] }}
                </v-col>
              </v-row>

              <v-slider :disabled="!canSetPosition" :value="inputData.position" min="0" max="100" step="2"></v-slider>

              <template v-if="canSetClue">
                <v-row>
                  <v-col>
                    <v-text-field v-model="inputData.clue" @keyup.enter="setClue" />
                  </v-col>
                  <v-col>
                    <v-btn @click="setClue"> Set clue </v-btn>
                  </v-col>
                </v-row>
              </template>

              <template v-else-if="canSetPosition">
                <v-row>
                  <v-col>
                    <v-btn @click="setPosition"> Agree on position </v-btn>
                  </v-col>
                </v-row>
              </template>

              <template v-else-if="canSetSide">
                <v-row>
                  <v-spacer />
                  <v-col>
                    <v-btn @click="setSide(false)"> left </v-btn>
                  </v-col>
                  <v-spacer />
                  <v-col>
                    <v-btn @click="setSide(true)"> right </v-btn>
                  </v-col>
                  <v-spacer />
                </v-row>
              </template>

              <v-btn> Do the thing </v-btn>
            </v-card>
          </v-col>
          <v-col class="text-lg-right">
            <v-card outlined :color="teamColors[1]">
              <v-card-title class="justify-end"> {{ teamColors[1] }} team </v-card-title>
              <v-list>
                <v-list-item v-for="item in teamUsers[1]" :key="item">
                  <v-list-item-content>
                    <v-list-item-title v-text="item"></v-list-item-title>
                  </v-list-item-content>
                </v-list-item>
              </v-list>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
    <footer-item></footer-item>
  </v-app>
</template>

<script>
import FooterItem from '@/components/FooterItem';
import STEPS from '@/common/constants.js';

export default {
  components: {
    FooterItem,
  },
  data() {
    return {
      currentColor: 'blue',
      opponentColor: 'red',
      currentSituation: 'Blue team chooses value',
      teamColors: ['blue', 'red'],
      roundData: {
        clue: null,
        position: null,
        side: null,
      },
      inputData: {
        clue: '',
        position: 50,
        side: false,
      },
      teamUsers: [
        ['Ernie', 'Artem', 'Thy Nyamka'],
        ['VBlinchik', 'Janessa', 'incitementfuck'],
      ],
    };
  },
  computed: {
    colors() {
      return {
        current: 'blue',
        opponent: 'red',
      };
    },
    currentStepIndex() {
      if (!this.roundData.clue) return 0;
      if (!this.roundData.position) return 1;
      if (!this.roundData.side) return 2;
      return 3;
    },
    canSetClue() {
      return this.currentStep.alias === 'clue' && (this.currentUser.isCaptain || this.currentUser.isAdmin);
    },
    canSetPosition() {
      return (
        this.currentStep.alias === 'position' &&
        ((this.currentUser.isActiveTeam && !this.currentUser.isCaptain) || this.currentUser.isAdmin)
      );
    },
    canSetSide() {
      return this.currentStep.alias === 'side' && (!this.currentUser.isActiveTeam || this.currentUser.isAdmin);
    },
    currentUser() {
      return {
        isCaptain: true,
        isActiveTeam: true,
        isAdmin: true,
      };
    },
    currentStep() {
      if (!this.roundData.clue) return this.steps[0];
      if (!this.roundData.position) return this.steps[1];
      if (!this.roundData.side) return this.steps[2];
      return this.steps[3];
    },
    steps() {
      return STEPS;
    },
    roundInfo() {
      return {
        concepts: ['Least used color', 'Most used color'],
        activeTeam: 0,
      };
    },
    errors() {
      let ret = {
        clue: false,
      };

      if (!this.inputData.clue || this.inputData.clue.length < 3) {
        ret.clue = 'Input a clue of 3 or more symbols';
      }

      return ret;
    },
  },
  methods: {
    setClue() {
      if (!this.errors.clue) {
        this.roundData.clue = this.inputData.clue;
        this.inputData.clue = '';
      }
    },
    setPosition() {
      this.roundData.position = this.inputData.position;
      this.inputData.position = 50;
    },
    setSide(value) {
      this.roundData.side = value;
    },
  },
};
</script>

<style></style>
