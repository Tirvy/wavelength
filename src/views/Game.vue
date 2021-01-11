<template>
    <v-app id="inspire">
        <v-main>
            <v-spacer/>
            <v-container>
                <v-stepper value="2">
                    <v-stepper-header>
                        <v-stepper-step
                            complete
                            step="1"
                        >
                            {{currentColor}} captain chooses tip
                        </v-stepper-step>

                        <v-divider></v-divider>

                        <v-stepper-step
                            step="2"
                        >
                            {{currentColor}} team chooses value
                        </v-stepper-step>

                        <v-divider></v-divider>

                        <v-stepper-step step="3">
                            {{opponentColor}} team chooses side
                        </v-stepper-step>
                    </v-stepper-header>
                </v-stepper>
            </v-container>
            <v-spacer/>
            <v-container>
                <v-row>
                    <v-col :color="teamColors[0]">
                        <v-card outlined :color="teamColors[0]">
                            <v-card-title>
                                {{teamColors[0]}} team
                            </v-card-title>
                            <v-list>
                                <v-list-item
                                    v-for="item in teamUsers[0]"
                                    :key="item"
                                >
                                    <v-list-item-content>
                                        <v-list-item-title v-text="item"></v-list-item-title>
                                    </v-list-item-content>
                                </v-list-item>
                            </v-list>
                        </v-card>
                    </v-col>
                    <v-col cols="8">
                        <v-row justify="space-between">
                            <v-col class="text-left">
                                {{currentScales[0]}}
                            </v-col>
                            <v-col class="text-right">
                                {{currentScales[1]}}
                            </v-col>
                        </v-row>

                        <v-slider :value="guessValue" min="0" max="100" step="2"></v-slider>

                        <v-btn>
                            Do the thing
                        </v-btn>
                    </v-col>
                    <v-col class="text-lg-right">
                        <v-card outlined :color="teamColors[1]">
                            <v-card-title>
                                {{teamColors[1]}} team
                            </v-card-title>
                            <v-list>
                                <v-list-item
                                    v-for="item in teamUsers[1]"
                                    :key="item"
                                >
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

export default {   
    components: {
        FooterItem
    },
    data() {
        return {
            currentColor: 'blue',
            opponentColor: 'red',
            currentSituation: 'Blue team chooses value',
            currentScales: ['Least used color', 'Most used color'],
            teamColors: ['blue', 'red'],
            guessValue: 50,
            teamUsers: [
                [
                    'Ernie',
                    'Artem',
                    'Thy Nyamka'
                ],
                [
                    'VBlinchik',
                    'Janessa',
                    'incitementfuck'
                ]
            ]
        }
    },
}
</script>

<style>

</style>