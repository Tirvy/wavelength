<template>
  <v-app id="inspire">
    <v-main>
      <v-spacer />

      <v-container>
        <v-stepper value="2">
          <v-stepper-header>
            <template v-for="(item, index) in steps">
              <v-divider :key="index" v-if="index > 0"></v-divider>

              <v-stepper-step
                :key="item.name"
                :step="index + 1"
                :complete="index < currentStep"
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
            <v-row justify="space-between">
              <v-col class="text-left subtitle-1">
                {{ roundInfo.concepts[0] }}
              </v-col>
              <v-col class="text-right subtitle-1">
                {{ roundInfo.concepts[1] }}
              </v-col>
            </v-row>

            <v-slider :value="guessValue" min="0" max="100" step="2"></v-slider>

            <v-btn> Do the thing </v-btn>
          </v-col>
          <v-col class="text-lg-right">
            <v-card outlined :color="teamColors[1]">
              <v-card-title> {{ teamColors[1] }} team </v-card-title>
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
      currentStep: 3,
      currentColor: 'blue',
      opponentColor: 'red',
      currentSituation: 'Blue team chooses value',
      teamColors: ['blue', 'red'],
      guessValue: 50,
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
    steps() {
      return STEPS;
    },
    roundInfo() {
      return {
        concepts: ['Least used color', 'Most used color'],
        activeTeam: 0,
        clue: undefined,
      };
    },
  },
};
</script>

<style></style>
