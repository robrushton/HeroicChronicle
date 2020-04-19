<template>
  <v-app>
    <v-app-bar
      absolute
      dark
      shrink-on-scroll
      prominent
      src="https://media.dnd.wizards.com/styles/story_banner/public/images/head-banner/kJaPUuWzE0.jpg"
    >
      <v-toolbar-title>Wildemount Heroic Chronicle</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn icon>
        <v-icon>mdi-invert-colors</v-icon>
      </v-btn>
    </v-app-bar>
    <v-content style="padding-top:150px">
      <v-container>
        <v-row justify="center">
          <v-col cols="6" lg="4">
            <v-text-field v-model="newName" label="Character Name" outlined rounded clearable></v-text-field>
          </v-col>
          <v-col cols="6" lg="6">
            <v-btn v-if="noCharGenerated" x-large color="success" elevation="8" @click="generate" transition="fade-transition" :disabled="disabledButton">
              Generate badass character
            </v-btn>
            <v-btn v-else x-large color="success" elevation="8" @click="generate" transition="fade-transition" :disabled="disabledButton">
              Generate...another...badass character
            </v-btn>
          </v-col>
        </v-row>
        <!-- <v-row>
          <v-col>
            <v-progress-linear v-if="!noCharGenerated" color="red" height="5" indeterminate></v-progress-linear>
          </v-col>
        </v-row> -->
        <v-row v-if="chronicle.name" justify="center">
          <v-col>
            <div class="display-1">{{chronicle.name}}</div>
          </v-col>
        </v-row>
        <v-row v-if="chronicle.name">
          <v-col cols="4">
            <v-card class="mb-3" hover outlined>
              <v-card-title>Homeland</v-card-title>
              <v-card-subtitle class="text-left">{{chronicle.Homeland}}</v-card-subtitle>
            </v-card>
            <v-card class="my-3" hover outlined>
              <v-card-title>Settlement</v-card-title>
              <v-card-subtitle class="text-left">{{chronicle.Settlement.Name}}</v-card-subtitle>
            </v-card="8">
            <v-card class="my-3" hover outlined>
              <v-card-title>Favorite Food</v-card-title>
              <v-card-subtitle class="text-left">{{chronicle.FavoriteFood}}</v-card-subtitle>
            </v-card>
            <v-card class="my-3" hover outlined>
              <v-card-title>Background</v-card-title>
              <v-card-subtitle class="text-left">{{chronicle.Background}}</v-card-subtitle>
            </v-card>
            <v-card class="mt-3" hover outlined>
              <v-card-title>Family</v-card-title>
              <v-card-subtitle class="text-left">
                <div v-if="chronicle.Family.Parents > 2">{{chronicle.Family.Parents}}+ Parent(s)</div>
                <div v-else>{{chronicle.Family.Parents}} Parent(s)</div>
                <div>{{chronicle.Family.Siblings}} Sibling(s)</div>
                <div>{{chronicle.Family.Powerful}} Powerful Family Member(s)</div>
              </v-card-subtitle>
            </v-card>
          </v-col>
          <v-col cols="4" align-self="stretch">
            <v-card class="match-height" hover outlined>
              <v-card-title>Allies</v-card-title>
              <div v-if="chronicle.AlliesRivals.allies.length > 0">
                <v-card-subtitle v-for="(person, index) in chronicle.AlliesRivals.allies" :key="index" class="text-left pt-0">
                  <div class="font-weight-bold">{{person.Name}}</div>
                  <div>{{person.relationship}}</div>
                </v-card-subtitle>
              </div>
              <div v-else>
                <v-card-subtitle class="text-left pt-0">Nope, no one likes you. Sorry bro</v-card-subtitle>
              </div>
            </v-card>
          </v-col>
          <v-col cols="4">
            <v-card class="match-height" hover outlined>
              <v-card-title>Rivals</v-card-title>
              <div v-if="chronicle.AlliesRivals.rivals.length > 0">
                <v-card-subtitle v-for="(person, index) in chronicle.AlliesRivals.rivals" :key="index" class="text-left pt-0">
                  <div class="font-weight-bold">{{person.Name}}</div>
                  <div>{{person.relationship}}</div>
                </v-card-subtitle>
              </div>
              <div v-else>
                <v-card-subtitle class="text-left pt-0">Yay! No one hates you!</v-card-subtitle>
              </div>
            </v-card>
          </v-col>
        </v-row>
        <v-row v-if="chronicle.name">
          <v-col cols="12">
            <v-card class="mb-3" hover outlined>
              <v-card-title class="pb-0">Fateful Moments</v-card-title>
              <div v-if="chronicle.FatefulMoments.length > 0">
                <v-card-subtitle v-for="moment in chronicle.FatefulMoments" :key="moment" class="text-left pt-0">{{moment}}</v-card-subtitle>
              </div>
              <div v-else>
                <v-card-subtitle class="text-left pt-0">Sadly, Fate has not revealed itself to you. Yet.</v-card-subtitle>
              </div>
            </v-card>
            <v-card class="mb-3" hover outlined>
              <v-card-title>Mysterious Secret</v-card-title>
              <v-card-subtitle class="text-left">{{chronicle.Secret}}</v-card-subtitle>
            </v-card>
            <v-card class="mb-3" hover outlined>
              <v-card-title>Prophecy</v-card-title>
              <v-card-subtitle class="text-left">{{chronicle.Prophecy}}</v-card-subtitle>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import backgrounds from "./character_info/backgrounds.json";

export default {
  name: 'app',
  data () {
    return {
      noCharGenerated: true,
      newName: "",
      chronicle: {
        name: "",
        Homeland: "",
        Background: "",
        SocialStatus: "",
        Settlement: "",
        Family: "",
        FamilyRelationships: "",
        BackgroundAllyRivalInfo: "",
        FamilyAllyRivalInfo: "",
        FatefulMoments: "",
        FavoriteFood: "",
        Secret: "",
        Prophecy: "",
      },
    }
  },
  computed: {
    disabledButton: function() {
      return this.newName === null || this.newName === undefined || this.newName === '';
    }
  },
  methods: {
    generate() {
      this.noCharGenerated = false;
      this.progress = 0;
      this.chronicle.name = this.newName;
      this.chronicle.Homeland = this.getHomeland();
      // Set timeout is async, so i'd need to call each of the methods to generate data within a set timeout function
      // If I want the progress bar to be incremental
      this.chronicle.Background = this.getBackground();
      this.chronicle.SocialStatus = this.getSocialStatus(this.chronicle.Homeland, this.chronicle.Background);
      this.chronicle.Settlement = this.getSettlement(this.chronicle.Homeland);
      this.chronicle.Family = this.getFamily(this.chronicle.Settlement);
      // Find all allies/rivals, then aggregate them into one object on the chronicle
      let FamilyRelationships = this.getFamilyRelationships(this.chronicle.Family);
      let BackgroundAllyRivalInfo = this.getBackgroundAllyRivalInfo(this.chronicle.SocialStatus);
      this.chronicle.AlliesRivals = this.calculateAlliesRivals(FamilyRelationships, BackgroundAllyRivalInfo);
      // this.chronicle.FamilyAllyRivalInfo = this.getFamilyAllyRivalInfo(this.chronicle.FamilyRelationships);
      this.chronicle.FatefulMoments = this.getFatefulMoments(BackgroundAllyRivalInfo, FamilyRelationships);
      this.chronicle.FavoriteFood = this.getFavoriteFood(this.chronicle.Homeland);
      this.chronicle.Secret = this.getSecret();
      this.chronicle.Prophecy = this.getProphecy();
    },
    getRandomInt(max) {
      return Math.floor(Math.random() * max) + 1;
    },
    getHomeland() {
      let result = this.getRandomInt(100);

      if (result <= 21) {
        return "Menagerie Coast";
      } else if (result <= 40) {
        return "Marrow Valley";
      } else if (result <= 72) {
        return "Zemni Fields";
      } else if (result <= 77) {
        return "Greying Wildlands";
      } else {
        return "Xhorhas";
      }
    },
    getBackground() {
      const result = this.getRandomInt(20) - 1;
      return backgrounds[result];
    },
    getSocialStatus(homeland, background) {
      let region = "";

      if (homeland in ["Menagerie Coast"]) {
        region = "Clovis Concord";
      } else if (homeland in ["Marrow Valley", "Zemni Fields"]) {
        region = "Dwendalian Empire";
      } else if (homeland in ["Greying Wildlands"]) {
        region = "Greying Wildlands";
      } else {
        region = "Kryn Dynasty"
      }

      const table = {
        "Acolyte": {
          "Clovis Concord":{
            "Ally": true,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Dwendalian Empire": {
            "Ally": false,
            "Rival": false,
            "And": false,
            "Or": true
          },
          "Greying Wildlands": {
            "Ally": false,
            "Rival": true,
            "And": false,
            "Or": false
          },
          "Kryn Dynasty": {
            "Ally": true,
            "Rival": false,
            "And": false,
            "Or": false
          }
        },
        "Acolyte (Luxonborn)": {
          "Clovis Concord":{
            "Ally": false,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Dwendalian Empire": {
            "Ally": false,
            "Rival": true,
            "And": false,
            "Or": false
          },
          "Greying Wildlands": {
            "Ally": false,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Kryn Dynasty": {
            "Ally": true,
            "Rival": false,
            "And": false,
            "Or": false
          }
        },
        "Charlatan": {
          "Clovis Concord":{
            "Ally": true,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Dwendalian Empire": {
            "Ally": false,
            "Rival": true,
            "And": false,
            "Or": false
          },
          "Greying Wildlands": {
            "Ally": true,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Kryn Dynasty": {
            "Ally": false,
            "Rival": true,
            "And": false,
            "Or": false
          }
        },
        "Criminal": {
          "Clovis Concord":{
            "Ally": false,
            "Rival": true,
            "And": false,
            "Or": false
          },
          "Dwendalian Empire": {
            "Ally": false,
            "Rival": true,
            "And": false,
            "Or": false
          },
          "Greying Wildlands": {
            "Ally": true,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Kryn Dynasty": {
            "Ally": false,
            "Rival": true,
            "And": false,
            "Or": false
          }
        },
        "Criminal (Myriad Operative)": {
          "Clovis Concord":{
            "Ally": false,
            "Rival": true,
            "And": false,
            "Or": false
          },
          "Dwendalian Empire": {
            "Ally": true,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Greying Wildlands": {
            "Ally": false,
            "Rival": true,
            "And": false,
            "Or": false
          },
          "Kryn Dynasty": {
            "Ally": false,
            "Rival": false,
            "And": false,
            "Or": false
          }
        },
        "Entertainer": {
          "Clovis Concord":{
            "Ally": true,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Dwendalian Empire": {
            "Ally": true,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Greying Wildlands": {
            "Ally": false,
            "Rival": true,
            "And": false,
            "Or": false
          },
          "Kryn Dynasty": {
            "Ally": true,
            "Rival": false,
            "And": false,
            "Or": false
          }
        },
        "Folk Hero": {
          "Clovis Concord":{
            "Ally": true,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Dwendalian Empire": {
            "Ally": false,
            "Rival": true,
            "And": false,
            "Or": false
          },
          "Greying Wildlands": {
            "Ally": false,
            "Rival": true,
            "And": false,
            "Or": false
          },
          "Kryn Dynasty": {
            "Ally": true,
            "Rival": false,
            "And": false,
            "Or": false
          }
        },
        "Grinner": {
          "Clovis Concord":{
            "Ally": true,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Dwendalian Empire": {
            "Ally": false,
            "Rival": true,
            "And": false,
            "Or": false
          },
          "Greying Wildlands": {
            "Ally": false,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Kryn Dynasty": {
            "Ally": false,
            "Rival": false,
            "And": false,
            "Or": false
          }
        },
        "Guild Artisan": {
          "Clovis Concord":{
            "Ally": false,
            "Rival": true,
            "And": false,
            "Or": false
          },
          "Dwendalian Empire": {
            "Ally": true,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Greying Wildlands": {
            "Ally": false,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Kryn Dynasty": {
            "Ally": true,
            "Rival": false,
            "And": false,
            "Or": false
          }
        },
        "Hermit": {
          "Clovis Concord":{
            "Ally": false,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Dwendalian Empire": {
            "Ally": false,
            "Rival": true,
            "And": false,
            "Or": false
          },
          "Greying Wildlands": {
            "Ally": true,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Kryn Dynasty": {
            "Ally": true,
            "Rival": false,
            "And": false,
            "Or": false
          }
        },
        "Noble": {
          "Clovis Concord":{
            "Ally": true,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Dwendalian Empire": {
            "Ally": false,
            "Rival": false,
            "And": true,
            "Or": false
          },
          "Greying Wildlands": {
            "Ally": false,
            "Rival": true,
            "And": false,
            "Or": false
          },
          "Kryn Dynasty": {
            "Ally": false,
            "Rival": false,
            "And": true,
            "Or": false
          }
        },
        "Outlander": {
          "Clovis Concord":{
            "Ally": false,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Dwendalian Empire": {
            "Ally": false,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Greying Wildlands": {
            "Ally": true,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Kryn Dynasty": {
            "Ally": false,
            "Rival": false,
            "And": false,
            "Or": false
          }
        },
        "Sage": {
          "Clovis Concord":{
            "Ally": false,
            "Rival": true,
            "And": false,
            "Or": false
          },
          "Dwendalian Empire": {
            "Ally": false,
            "Rival": true,
            "And": false,
            "Or": false
          },
          "Greying Wildlands": {
            "Ally": false,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Kryn Dynasty": {
            "Ally": true,
            "Rival": false,
            "And": false,
            "Or": false
          }
        },
        "Sage (Cobalt Soul)": {
          "Clovis Concord":{
            "Ally": true,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Dwendalian Empire": {
            "Ally": false,
            "Rival": false,
            "And": true,
            "Or": false
          },
          "Greying Wildlands": {
            "Ally": false,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Kryn Dynasty": {
            "Ally": true,
            "Rival": false,
            "And": false,
            "Or": false
          }
        },
        "Sailor": {
          "Clovis Concord":{
            "Ally": false,
            "Rival": false,
            "And": true,
            "Or": false
          },
          "Dwendalian Empire": {
            "Ally": false,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Greying Wildlands": {
            "Ally": false,
            "Rival": true,
            "And": false,
            "Or": false
          },
          "Kryn Dynasty": {
            "Ally": true,
            "Rival": false,
            "And": false,
            "Or": false
          }
        },
        "Sailor (Revelry Pirate)": {
          "Clovis Concord":{
            "Ally": false,
            "Rival": false,
            "And": true,
            "Or": false
          },
          "Dwendalian Empire": {
            "Ally": false,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Greying Wildlands": {
            "Ally": true,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Kryn Dynasty": {
            "Ally": false,
            "Rival": false,
            "And": false,
            "Or": false
          }
        },
        "Soldier": {
          "Clovis Concord":{
            "Ally": true,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Dwendalian Empire": {
            "Ally": false,
            "Rival": false,
            "And": true,
            "Or": false
          },
          "Greying Wildlands": {
            "Ally": false,
            "Rival": true,
            "And": false,
            "Or": false
          },
          "Kryn Dynasty": {
            "Ally": false,
            "Rival": false,
            "And": true,
            "Or": false
          }
        },
        "Spy (Augentrust)": {
          "Clovis Concord":{
            "Ally": false,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Dwendalian Empire": {
            "Ally": true,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Greying Wildlands": {
            "Ally": false,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Kryn Dynasty": {
            "Ally": false,
            "Rival": true,
            "And": false,
            "Or": false
          }
        },
        "Urchin": {
          "Clovis Concord":{
            "Ally": true,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Dwendalian Empire": {
            "Ally": false,
            "Rival": true,
            "And": false,
            "Or": false
          },
          "Greying Wildlands": {
            "Ally": false,
            "Rival": true,
            "And": false,
            "Or": false
          },
          "Kryn Dynasty": {
            "Ally": false,
            "Rival": true,
            "And": false,
            "Or": false
          }
        },
        "Volstrucker Agent": {
          "Clovis Concord":{
            "Ally": false,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Dwendalian Empire": {
            "Ally": true,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Greying Wildlands": {
            "Ally": false,
            "Rival": false,
            "And": false,
            "Or": false
          },
          "Kryn Dynasty": {
            "Ally": false,
            "Rival": true,
            "And": false,
            "Or": false
          }
        }
      }

      return table[background][region];
    },
    getSettlement(homeland) {
      let result = this.getRandomInt(100);

      let settlement = {};

      if (homeland == "Menagerie Coast") {
        if (result <= 1) {
          settlement.Name = "Brokenbank";
          settlement.Type = "Village";
        } else if (result <= 2) {
          settlement.Name = "Darktow";
          settlement.Type = "Village";
        } else if (result <= 9) {
          settlement.Name = "Feolinn";
          settlement.Type = "City";
        } else if (result <= 19) {
          settlement.Name = "Gwardan";
          settlement.Type = "City";
        } else if (result <= 36) {
          settlement.Name = "Nicodranas";
          settlement.Type = "City";
        } else if (result <= 40) {
          settlement.Name = "Othe";
          settlement.Type = "City";
        } else if (result <= 41) {
          settlement.Name = "Palma Flora";
          settlement.Type = "Village";
        } else if (result <= 84) {
          settlement.Name = "Port Damali";
          settlement.Type = "City";
        } else if (result <= 93) {
          settlement.Name = "Port Zoon";
          settlement.Type = "City";
        } else {
          settlement.Name = "Tussoa";
          settlement.Type = "City";
        }
      } else if (homeland == "Marrow Valey") {
        if (result <= 2) {
          settlement.Name = "Allfield";
          settlement.Type = "Village";
        } else if (result <= 5) {
          settlement.Name = "Ashguard Garrison";
          settlement.Type = "Village";
        } else if (result <= 7) {
          settlement.Name = "Berleben";
          settlement.Type = "Village";
        } else if (result <= 12) {
          settlement.Name = "Bladegarden";
          settlement.Type = "City";
        } else if (result <= 18) {
          settlement.Name = "Deastok";
          settlement.Type = "City";
        } else if (result <= 22) {
          settlement.Name = "Felderwin";
          settlement.Type = "City";
        } else if (result <= 32) {
          settlement.Name = "Grimgolir";
          settlement.Type = "City";
        } else if (result <= 40) {
          settlement.Name = "Hupperdook";
          settlement.Type = "City";
        } else if (result <= 44) {
          settlement.Name = "Kamordah";
          settlement.Type = "City";
        } else if (result <= 45) {
          settlement.Name = "Talonstadt";
          settlement.Type = "Village";
        } else if (result <= 50) {
          settlement.Name = "Trostenwald";
          settlement.Type = "City";
        } else if (result <= 52) {
          settlement.Name = "Vol'antim";
          settlement.Type = "Village";
        } else {
          settlement.Name = "Zadash";
          settlement.Type = "City";
        }
      } else if (homeland == "Zemni Fields") {
        if (result <= 1) {
          settlement.Name = "Blumenthal";
          settlement.Type = "Village";
        } else if (result <= 7) {
          settlement.Name = "Bysaes Tyl";
          settlement.Type = "City";
        } else if (result <= 11) {
          settlement.Name = "Druvenlode";
          settlement.Type = "City";
        } else if (result <= 13) {
          settlement.Name = "Icehaven";
          settlement.Type = "Village";
        } else if (result <= 18) {
          settlement.Name = "Nogvurot";
          settlement.Type = "City";
        } else if (result <= 20) {
          settlement.Name = "Odessloe";
          settlement.Type = "City";
        } else if (result <= 26) {
          settlement.Name = "Pride's Call";
          settlement.Type = "City";
        } else if (result <= 96) {
          settlement.Name = "Rexxentrum";
          settlement.Type = "City";
        } else if (result <= 98) {
          settlement.Name = "Rocksguard Garrison";
          settlement.Type = "Village";
        } else if (result <= 99) {
          settlement.Name = "Velvin Thicket";
          settlement.Type = "Village";
        } else {
          settlement.Name = "Yrrosa";
          settlement.Type = "Village";
        }
      } else if (homeland == "Greying Wildlands") {
        if (result <= 3) {
          settlement.Name = "Boroftkrah";
          settlement.Type = "Village";
        } else if (result <= 6) {
          settlement.Name = "Palebank Village";
          settlement.Type = "Village";
        } else if (result <= 30) {
          settlement.Name = "Shadycreek Run";
          settlement.Type = "City";
        } else {
          settlement.Name = "Uthodurn";
          settlement.Type = "City";
        }
      } else {
        if (result <= 20) {
          settlement.Name = "Asarius";
          settlement.Type = "City";
        } else if (result == 21) {
          settlement.Name = "Bazzoxan";
          settlement.Type = "Village";
        } else if (result == 22) {
          settlement.Name = "Charis";
          settlement.Type = "Village";
        } else if (result <= 30) {
          settlement.Name = "Igrathad";
          settlement.Type = "Village";
        } else if (result <= 36) {
          settlement.Name = "Jigow";
          settlement.Type = "City";
        } else if (result == 37) {
          settlement.Name = "New Haxon";
          settlement.Type = "Village";
        } else if (result <= 89) {
          settlement.Name = "Rosohna (Ghor Dranas)";
          settlement.Type = "City";
        } else if (result <= 93) {
          settlement.Name = "Rotthold";
          settlement.Type = "City";
        } else if (result <= 96) {
          settlement.Name = "Urzin";
          settlement.Type = "Village";
        } else {
          settlement.Name = "Xarzith Kitrik";
          settlement.Type = "City";
        }
      }

      return settlement;
    },
    getFamily(settlement) {
      let result = this.getRandomInt(100);

      let family = {};

      if (settlement.Type == "City") {
        if (result <= 5) {
          family.Parents = 3;
          family.Siblings = this.getRandomInt(4) + this.getRandomInt(4) + 2;
        } else if (result <= 60) {
          family.Parents = 2;
          family.Siblings = this.getRandomInt(4) + this.getRandomInt(4);
        } else if (result <= 80) {
          family.Parents = 1;
          family.Siblings = this.getRandomInt(4);
        } else {
          family.Parents = 0;
          family.Siblings = 0;
        }
      } else {
        if (result <= 10) {
          family.Parents = 3;
          family.Siblings = this.getRandomInt(4) + this.getRandomInt(4) + 2;
        } else if (result <= 50) {
          family.Parents = 2;
          family.Siblings = this.getRandomInt(4) + this.getRandomInt(4);
        } else if (result <= 89) {
          family.Parents = 1;
          family.Siblings = this.getRandomInt(4);
        } else {
          family.Parents = 0;
          family.Siblings = 0;
        }
      }

      let powerful = this.getRandomInt(3);
      let possiblePowerful = family.Parents + family.Siblings;
      family.Powerful = powerful > possiblePowerful ? possiblePowerful : powerful;

      return family;
    },
    getFamilyRelationships(family) {
      let relationships = [];
      let loopVar = family.Powerful;

      if (family.Parents + family.Siblings > 0) {
        if (family.Parents + family.Siblings < family.Powerful) {
          loopVar = family.Parents + family.Siblings;
        }

        for (let i = 0; i < loopVar; i++) {
          const result = this.getRandomInt(100);
          let newIdentity = this.getAllyRivalIdentity();

          if (result <= 10) {
            newIdentity.relationship = "You thought you killed this family member, whether by accident or otherwise. You never expected to see them again—but now they’re out for your blood. You gain one rival.";
            newIdentity.type = "rival";
          } else if (result <= 20) {
            newIdentity.relationship = "You insulted this family member so gravely that they left your life forever. If they ever return, it will be to settle the score. You gain one rival.";
            newIdentity.type = "rival";
          } else if (result <= 30) {
            newIdentity.relationship = "You have always been better than this family member at a particular activity. They grew jealous and abandoned you, so that they could return and best you one day. You gain one rival.";
            newIdentity.type = "rival";
          } else if (result <= 40) {
            newIdentity.relationship = "You uncovered a secret about this family member, whether a tiny embarrassment or a life-changing scandal. They now seek to unveil your darkest secret. You gain one rival.";
            newIdentity.type = "rival";
          } else if (result <= 50) {
            newIdentity.relationship = "You and this family member have a friendly rivalry, and are constantly trying to best each other in an activity, craft, or other pursuit. You visit occasionally to test each other’s skills. You gain one rival.";
            newIdentity.type = "rival";
          } else if (result <= 60) {
            newIdentity.relationship = "This family member owes you a debt, and they don’t like it. They’ll help you out when you need it, but only to clear the slate. You gain one ally.";
            newIdentity.type = "ally";
          } else if (result <= 70) {
            newIdentity.relationship = "This family member loves you, but you were never that close. They’ll do anything to help you—as long as they won’t be at risk of injury or death. You gain one ally.";
            newIdentity.type = "ally";
          } else if (result <= 80) {
            newIdentity.relationship = "This family member caused you to have a horrible accident when you were a child. They still feel incredible guilt, which they would do anything to assuage. You gain one ally.";
            newIdentity.type = "ally";
          } else if (result <= 90) {
            newIdentity.relationship = "This family member left long ago for reasons you don’t understand or won’t talk about. Before they left, they promised you that they would return in your hour of greatest need. You gain one ally.";
            newIdentity.type = "ally";
          } else {
            newIdentity.relationship = "This family member has always loved you with all their heart, and would do anything for you. You gain one ally.";
            newIdentity.type = "ally";
          }
          relationships.push(newIdentity);
        }
      }

      return relationships;
    },
    getAllyRivalIdentity() {
      const result = this.getRandomInt(100);

      if (result <= 5) {
        return {"Name": "Commoner", "FatefulMoment": false}
      } else if (result <= 10) {
        return {"Name": "Acolyte", "FatefulMoment": false}
      } else if (result <= 15) {
        return {"Name": "Bandit", "FatefulMoment": false}
      } else if (result <= 20) {
        return {"Name": "Bandit Captain", "FatefulMoment": false}
      } else if (result <= 25) {
        return {"Name": "Berserker", "FatefulMoment": false}
      } else if (result <= 30) {
        return {"Name": "Cultist", "FatefulMoment": false}
      } else if (result <= 35) {
        return {"Name": "Cult Fanatic", "FatefulMoment": true}
      } else if (result <= 40) {
        return {"Name": "Druid", "FatefulMoment": false}
      } else if (result <= 45) {
        return {"Name": "Gladiator", "FatefulMoment": true}
      } else if (result <= 50) {
        return {"Name": "Guard", "FatefulMoment": false}
      } else if (result <= 55) {
        return {"Name": "Knight", "FatefulMoment": false}
      } else if (result <= 60) {
        return {"Name": "Priest", "FatefulMoment": false}
      } else if (result <= 65) {
        return {"Name": "Scout", "FatefulMoment": false}
      } else if (result <= 70) {
        return {"Name": "Spy", "FatefulMoment": false}
      } else if (result <= 75) {
        return {"Name": "Tribal Warrior", "FatefulMoment": false}
      } else if (result <= 80) {
        return {"Name": "Veteran", "FatefulMoment": false}
      } else if (result <= 84) {
        return {"Name": "Mage", "FatefulMoment": true}
      } else if (result <= 88) {
        return {"Name": "Noble", "FatefulMoment": true}
      } else if (result <= 92) {
        return {"Name": "Assassin", "FatefulMoment": true}
      } else if (result <= 94) {
        return {"Name": "Blood Hunter", "FatefulMoment": true}
      } else if (result <= 96) {
        return {"Name": "Werebear/Weretiger", "FatefulMoment": true}
      } else if (result <= 98) {
        return {"Name": "Wereboar/Wererat/Werewolf", "FatefulMoment": true}
      } else if (result <= 99) {
        return {"Name": "Archmage", "FatefulMoment": true}
      } else {
        return {"Name": "Adult Gold/Red Dragon", "FatefulMoment": true}
      }
    },
    getAllyRelationship() {
      const result = this.getRandomInt(100);

      if (result <= 10) {
        return "This ally is a loyal pet. Rather than rolling on the Ally and Rival Identities table, choose one beast of CR 1/8 or lower as your pet.";
      } else if (result <= 20) {
        return "This person once lost a bet to you and is still trying to scrounge up the cash to pay you back. They’ve decided you’d both be better off if they put you in their debt instead.";
      } else if (result <= 30) {
        return "This person was once a beggar to whom you gave a large sum of money. They have transformed their life thanks to you, and now want to repay your generosity.";
      } else if (result <= 40) {
        return "You were this person’s favorite drinking buddy, and their home is always open to you and your friends.";
      } else if (result <= 50) {
        return "This person was once your mentor, but you left before you could complete your training. You are welcome to return and finish what you started, but only when you are ready.";
      } else if (result <= 60) {
        return "You bonded with this person over a traumatic event such as a battle or an armed robbery. If you ever tell them that you are in danger, they will try to aid you.";
      } else if (result <= 70) {
        return "You and this person share a terrible secret, and you have sworn to never reveal it to anyone. They will help you keep this secret if it is ever in danger of being revealed.";
      } else if (result <= 80) {
        return "This person fell in love with you. If you reciprocated, they always stand at your side. If you didn’t, they took it well, and still consider you their closest friend.";
      } else if (result <= 90) {
        return "You and this person were affected by powerful magic, and now you both share a telepathic connection that functions while you are within 1 mile of each other.";
      } else {
        return "This person owes you their life. Even if they can’t follow you everywhere you go, they will do anything they can to protect you.";
      }
    },
    getRivalRelationship() {
      const result = this.getRandomInt(100);

      if (result <= 10) {
        return "This person believes that you murdered their sibling. Regardless of your guilt or innocence, they are out for your blood.";
      } else if (result <= 20) {
        return "You bested this person in combat, but they believe you cheated to defeat them. They long to prove that they are the superior warrior.";
      } else if (result <= 30) {
        return "You broke a promise to this person, and it caused them to suffer greatly. Now they conspire to make someone else break a valuable promise to you.";
      } else if (result <= 40) {
        return "You once loved this person, but broke their heart. They are now obsessed with making you feel the same pain they felt.";
      } else if (result <= 50) {
        return "This person was ordered to arrest you, and doggedly hunts you wherever you go.";
      } else if (result <= 60) {
        return "This person thinks that you were replaced by a doppelganger or possessed by a spirit or monster. They are now trying to defeat you, so as to find or free the original you.";
      } else if (result <= 70) {
        return "You fled from your home under mysterious circumstances. This person is obsessed with finding out the truth of what caused you to leave.";
      } else if (result <= 80) {
        return "You and this person tried to harness power beyond your control, and it left them disfigured and in constant pain. Having since mastered the power that nearly destroyed them, they now seek to turn it upon you.";
      } else if (result <= 90) {
        return "You helped this person out once when they were down on their luck, and now they go to you whenever they need help.";
      } else {
        return "This person wants to be your friend, but their help has always made your life harder.";
      }
    },
    getBackgroundAllyRivalInfo(socialStatus) {
      if (socialStatus.Ally) {
        const identity = this.getAllyRivalIdentity();
        const relationship = this.getAllyRelationship();
        return [
          {
            "Name": identity.Name,
            "FateFulMoment": identity.FatefulMoment,
            "relationship": relationship,
            "type": "ally",
          }
        ]
      } else if (socialStatus.Rival) {
        const identity = this.getAllyRivalIdentity();
        const relationship = this.getRivalRelationship();
        return [
          {
            "Name": identity.Name,
            "FatefulMoment": identity.FatefulMoment,
            "relationship": relationship,
            "type": "rival"
          }
        ]
      } else if (socialStatus.And) {
        const allyIdentity = this.getAllyRivalIdentity();
        const allyRelationship = this.getAllyRelationship();
        const rivalIdentity = this.getAllyRivalIdentity();
        const rivalRelationship = this.getRivalRelationship();
        return [
          {
            "Name": allyIdentity.Name,
            "FateFulMoment": allyIdentity.FatefulMoment,
            "relationship": allyRelationship,
            "type": "ally",
          },
          {
            "Name": rivalIdentity.Name,
            "FatefulMoment": rivalIdentity.FatefulMoment,
            "relationship": rivalRelationship,
            "type": "rival"
          }
        ]
      } else {
        const allyIdentity = this.getAllyRivalIdentity();
        const allyRelationship = this.getAllyRelationship();
        const rivalIdentity = this.getAllyRivalIdentity();
        const rivalRelationship = this.getRivalRelationship();
        return [
          {
            "Name": allyIdentity.Name,
            "FateFulMoment": allyIdentity.FatefulMoment,
            "relationship": allyRelationship,
            "type": "ally",
          },
          {
            "Name": rivalIdentity.Name,
            "FatefulMoment": rivalIdentity.FatefulMoment,
            "relationship": rivalRelationship,
            "type": "rival"
          }
        ]
      }
    },
    calculateAlliesRivals(famInfo, bgInfo) {
      let alliesRivals = {
        "allies": [],
        "rivals": []
      };
      famInfo.forEach((element) => {
        if (element.type === "ally") {
          alliesRivals.allies.push(element);
        } else {
          alliesRivals.rivals.push(element);
        }
      });
      bgInfo.forEach((element) => {
        if (element.type === "ally") {
          alliesRivals.allies.push(element);
        } else {
          alliesRivals.rivals.push(element);
        }
      });

      return alliesRivals;
    },
    // getFamilyAllyRivalInfo(familyRelationships) {
    //   const famAllies = familyRelationships.Allies;
    //   const famRivals = familyRelationships.Rivals;

    //   let allies = [];
    //   let rivals = [];

    //   for (let i = 0; i < famAllies; i++) {
    //     allies.push(this.getAllyRivalIdentity());
    //   }

    //   for (let i = 0; i < famRivals; i++) {
    //     rivals.push(this.getAllyRivalIdentity());
    //   }

    //   return {
    //     "FamilyAllyIdentities": allies,
    //     "FamilyRivalIdentities": rivals
    //   }
    // },
    getFatefulMoments(bgInfo, famInfo) {
      const moments = [
        "Your parents were murdered in front of you. Roll on the Ally and Rival Identities table to determine the type of creature that killed them. You have proficiency in the Stealth and Survival skills.",
        "You met a dark elf dying in the wilderness. Around their neck was a silver talisman containing a cameo of their child and the name “Il’viranya.” It is an amulet of proof against detection and location.",
        "A mysterious stranger gave you a gift that saved your life while you were lost in the wilderness. Roll on the Ally and Rival Identities table to determine the identity of the stranger. Then roll on Magic Item Table B in the Dungeon Master’s Guide to determine the item. If you roll a consumable item from the table, roll again.",
        "You were caught in a terrible storm but miraculously survived. Now your dreams contain visions sent by a mysterious god or demigod. You have proficiency in the Arcana or Religion skill (your choice).",
        "A famous warrior trained you with what has become your signature weapon. You have proficiency with a martial weapon of your choice, and you own one such weapon. It has special features as detailed in chapter 7 of the Dungeon Master’s Guide. You also have the Martial Adept feat from the Player’s Handbook.",
        "You were the sole survivor when a horde of rampaging monsters raided your village or your neighborhood. You have proficiency in the Stealth skill or proficiency with martial weapons (your choice).",
        "A famous mage saw potential in you and tutored you in the arcane arts. You have a spellbook and the Magic Initiate feat from the Player’s Handbook.",
        "While on a long journey, you were picked up by a traveling circus, spending a year with them before returning to your home. You have proficiency in the Acrobatics or Performance skill (your choice) and proficiency with the disguise kit.",
        "You were transformed into a bear by mysterious magic, and lived for a year as an animal before you were saved by a druid. Magic still lingers within you, though, and you can cast speak with animals at will.",
        "You were press-ganged into military service, and were left shaken by what you saw on the battlefield. You have proficiency with medium armor, shields, and martial weapons. You also have a random form of indefinite madness, determined by rolling on the Indefinite Madness table in chapter 8 of the Dungeon Master’s Guide.",
        "You were kidnapped by bandits while traveling between towns. While captured, you met an old thief who helped you escape. You have proficiency with thieves’ tools and proficiency in the Stealth skill.",
        "You were visited by a demon lord in a dream. You awakened knowing the find familiar spell and are now able to cast it as a ritual, but your familiar always takes the form of a quasit. You also have a random form of indefinite madness, determined by rolling on the Indefinite Madness table in chapter 8 of the Dungeon Master’s Guide.",
        "While exploring a remote forest, you were attacked by evil lycanthropes but escaped before being killed. You are cursed with wereboar, wererat, or werewolf lycanthropy (DM’s choice).",
        "While lost in a remote forest or jungle, you were saved by a werebear or weretiger (DM’s choice). The lycanthrope believed you were destined for greatness and granted you the gift of lycanthropy with your consent.",
        "You saved a pseudodragon from being eaten by a giant spider in a dark forest. The pseudodragon now loyally follows you wherever you go, even if you’d rather it stay hidden. It is controlled by the DM but obeys your commands if treated well.",
        "You nearly died from a virulent disease (the DM’s choice of cackle fever, sewer plague, or sight rot; see chapter 8 of the Dungeon Master’s Guide). Your life was saved by an agent of the Cobalt Soul, who could not cure the disease, but who gave you a periapt of health that suppresses it.",
        "You were accused of a crime and were exiled or imprisoned, regardless of whether or not you were guilty. Having spent time among criminals, you have proficiency in the Intimidation skill and you know thieves’ cant.",
        "You saved a riderless horse wearing full tack and harness from wolves. You own a riding horse and a saddle, and you have proficiency in the Animal Handling skill.",
        "While reading through a mysterious tome once owned by your parent, you found a treasure map that points toward a place in Wildemount of the DM’s choice.",
        "You received a letter revealing that you were the secret child of a wealthy noble family living in Rexxentrum within the Dwendalian Empire. They enclosed 100 gp to ensure your safe passage to the imperial capital, and a signet ring bearing your true family’s seal."
      ];

      let totalFatefulMoments = 0;
      let rolledMoments = [];

      for (let i = 0; i < bgInfo.length; i++) {
        if (bgInfo[i].FatefulMoment) {
          totalFatefulMoments++;
        }
      }

      for (let i = 0; i < famInfo.length; i++) {
        if (famInfo[i].FatefulMoment) {
          totalFatefulMoments++;
        }
      }

      for (let i = 0; i < totalFatefulMoments; i++) {
        const result = this.getRandomInt(20) - 1;
        rolledMoments.push(moments[result]);
      }

      return rolledMoments;
    },
    getFavoriteFood(homeland) {
      const result = this.getRandomInt(8) - 1;

      const menagerieFoods = [
        "Paella—a working-class dish made with rice, white beans, and seafood",
        "Plantain cups—a sweet and savory dish of fried plantains stuffed with meat and rice",
        "Gazpacho—a cold soup served on hot days, made from pounded vegetables and fruit",
        "Honeyflame bread—a fried dessert soaked in honey and coated in Marquesian spices",
        "Fusaka fish—seafood cutlets smothered in Marquesian fusaka spice and fried in oil",
        "Snakelocks noodles—sea anemone tendrils coated in honey batter and delicately fried",
        "Queen’s water—a soft drink made from syrup, honey, guava, and tamarind",
        "Blacksand coffee—a tiny shot of coffee, brewed atop red-hot sand, Marquet-style"
      ];

      const dwendalianFoods = [
        "Dumplings—a steamed potato bread that can be served with any meal",
        "Sauerbraten—a Zemnian peasant dish of pickled horse meat served with cabbage",
        "Brawn, also known as head cheese—a meat jelly made from boiled calf’s head",
        "Schweinshaxe—a Zemnian peasant dish of long-marinated roasted pork knuckle",
        "Dampfnudel—a regal steamed roll served in sweet custard or with savory potatoes",
        "Spanferkel—an expensive dish of suckling pig, roasted and served at royal parties",
        "Trost—a sweet, dark ale brewed in Trostenwald",
        "Radler—a sweet, expensive drink made from imported lemonade mixed with beer"
      ];

      const wildlandsFoods = [
        "Imperial pickled plums, smuggled from the Dwendalian Empire by Myriad agents",
        "Charred venison and roasted potatoes, prepared with local game and local tubers",
        "Raw venison still dripping with blood",
        "Elf-mash—a creamy dish made from overripe cloudberries",
        "Dwarven rootbake—a hearty casserole of roots and tubers wrapped in seaweed",
        "Jam porridge—made from Xhorhasian rice and topped with salmonberry jam",
        "Blazing tea—a beverage blended from fermented fireroot and mouth-scalding spices",
        "Sbiten—a drink made from honey and spices, best enjoyed hot on snowy days"
      ];

      const xhorhasFoods = [
        "Rzukaal—a dish made from sautéed rice noodles, hearty mushrooms, and giant spider legs",
        "Yuyandl—grilled yuyo (a zucchini-like vegetable that grows in Rosohna’s sunless gardens) spiced and served with rice",
        "Mastodon kor’rundl—grilled mastodon served with sunless kor’run (a squash-like vegetable that grows in Rosohna’s sunless gardens) and rice",
        "Kinespaji spaaldl—soup made from mushrooms or vegetables and the boiled spit of a horizonback turtle",
        "Umarindaly—a dessert akin to rice pudding, topped with spiced gooseberry jam",
        "Keltaly—heavy cream mixed with pulverized black currants and frozen into a fluffy, sweet, creamy dessert",
        "Erzfaalyu—a potent spirit made from fermented rice",
        "Yunfaalyu—a fragrant plum liquor served at frigid temperatures and garnished with currants"
      ];

      if (["Menagerie Coast"].includes(homeland)) {
        return menagerieFoods[result];
      } else if (["Marrow Valley", "Zemni Fields"].includes(homeland)) {
        return dwendalianFoods[result];
      } else if (["Greying Wildlands"].includes(homeland)) {
        return wildlandsFoods[result];
      } else {
        return xhorhasFoods[result];
      }
    },
    getSecret() {
      const result = this.getRandomInt(20) - 1;

      const secrets = [
        "Years ago, my best friend came to me in the middle of the night and gave me a key that glowed with an icy blue light. I never saw that friend again.",
        "I was the only witness to a cold-blooded murder. In the aftermath, I saw the killer take a gold coin with a ruby inlaid at its center from the victim’s body.",
        "I once had a dream where an old stranger looked me dead in the eye, screamed “Scourger!” at the top of their lungs, and then exploded into a column of flame.",
        "While exploring near my home, I found a cliff with a bunch of caverns dug out of it, all of them large enough for people to hide within.",
        "I once saw a cat that seemed to be moving with a strange sense of purpose. I followed it to the dwelling of an important local elder, where it gazed through the window for an hour. Then it suddenly shook itself and raced away, as though a spell had been broken.",
        "One of my parents left home in the middle of a storm, in the middle of the night. They had their sword and shield. They came back a week later, with the shield practically in pieces. They never talked about that night.",
        "I had a friend who was a farmer. Every week, their crops doubled in size until they had pumpkins as big as houses. Then the next week, the friend was gone and their fields were torched. I never heard from them again.",
        "I once saw an enormous figure walking through the clouds on a stormy night. At one point, they looked at me, and then kept walking.",
        "I woke up one night to find one of my siblings perched on my chest, staring into my eyes. They said, “The time is soon,” and then giggled and ran off. When I asked them, they had no memory of the event.",
        "I once saw a giant bird soar past overhead. It croaked out a cry that sounded like my name, then disappeared beyond the clouds.",
        "I ate a fruit whose seeds spelled out a magic word where I’d cast them onto the ground. Years later, I saw the same word spelled out within a slice of bread.",
        "A warrior friend of mine died. But every so often, I swear I see that friend in their old armor, at the corner of my vision.",
        "An old seer once touched my forehead and gave me a vision of a flaming bird chained beneath a mountain, squirming and wailing in the darkness.",
        "Once while on a boat, I heard a voice rumbling around me. I looked down below the water, and I swear I saw golden eyes looking up at me.",
        "While I was eating, a whole potato exploded into worms, and I suffered a vision of a gigantic worm eating the world like a giant apple.",
        "While picking flowers, I saw a tall figure with red skin and horns wandering the meadow. The flowers grew taller where they walked, but I fled in fear and never saw the figure again.",
        "I was attacked by wolves in the woods one day, and was saved by a stranger with a bandage over their eyes. This person shone with silver light and was covered with scars—and I think I might have seen black wings tucked in at their back.",
        "While trying to forge a sword, I accidentally burned myself with the red-hot blade. A strange vision then came to me, of powerful weapons calling out for me to wield them.",
        "I once met someone fleeing through the woods who said they had escaped from some evil place. I asked what that meant, but the stranger fell dead on the spot. When I returned with help to collect the body, it was gone.",
        "I once caught a falling star. It looked up from my hands and smiled, then told me to look for it on the day when fire erupts from the earth."
      ];

      return secrets[result]
    },
    getProphecy() {
      const result = this.getRandomInt(20) - 1;

      const prophecies = [
        "I will defeat the creature that killed my parents. Its defeat might make me question my purpose in life.",
        "I will uncover the reason that the Cerberus Assembly took my sibling away. Finding my sibling will set political events beyond my control into motion.",
        "I will save my village from the gnoll tribe that has raided us for the past year. Their defeat will inspire me to perform even greater feats of heroism.",
        "I will unlock the secrets of consecution that the Kryn are hiding. This knowledge will open my mind to a terrifying truth.",
        "I will join a blood hunter order. My new comrades-in-arms will make me powerful, but I must pay a steep price for that power.",
        "I will steal a king’s ransom from a Revelry pirate. That wealth will make me happy, but it will also draw unsavory characters to me.",
        "I will become a hero of the war between the Dwendalian Empire and the Kryn. I will be haunted by the atrocities I witness on the battlefield.",
        "I will cleanse the Savalirwood of its corruption while welcoming that corruption into my own body.",
        "I will infiltrate the Myriad, but doing so will compel me to commit evil acts.",
        "I will uncover a relic from Eiselcross and become famous, but the relic will exact a terrible cost.",
        "I will speak to a dragon, live to tell the tale, and provoke the dragon’s everlasting wrath.",
        "I will steal a holy relic of the Kryn Dynasty, making me a target for anyone who desires its power.",
        "I will hesitate at an important moment. Another person will suffer for it.",
        "I will stumble by accident into the arms of the Golden Grin. Though I will reject their call at first, something will draw me back.",
        "I will befriend a flying beast and ride it through the skies. Others will envy me for the bond I have with this creature.",
        "I will kneel before Bright Queen Leylas Kryn without knowing who she is. This moment of uncertainty will lead to danger.",
        "I will stand before Princess Suria Dwendal and briefly hold the fate of the empire in my hand. I won’t realize the gravity of my decision until it is too late.",
        "My actions will lead to the death of a marquis of the Clovis Concord. I will know exactly who killed them, but no one in power will believe me.",
        "I will anger one of the Tribes of Shadycreek Run. As its members hunt me in retribution, others will suffer and I will pay the price.",
        "I will meet my birth parents. Doing so reveals a secret about my birth that will change the way I look at the world."
      ];

      return prophecies[result];
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.match-height {
  height: 100%;
}
</style>
