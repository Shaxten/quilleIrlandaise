<script setup>
import WelcomeItem from './WelcomeItem.vue'
import DocumentationIcon from './icons/IconDocumentation.vue'
import ToolingIcon from './icons/IconTooling.vue'
import EcosystemIcon from './icons/IconEcosystem.vue'
import CommunityIcon from './icons/IconCommunity.vue'
import SupportIcon from './icons/IconSupport.vue'
import IconEcosystem from './icons/IconEcosystem.vue'
import IconSupport from './icons/IconSupport.vue'
import IconDocumentation from './icons/IconDocumentation.vue'
import IconCommunity from './icons/IconCommunity.vue'
</script>

<template>
  <h1 class="primColor title">Quille Irlandaise</h1>

  <div id="game" class="playerInGame mt5 mb5">
    <div id="victory" class="center">
      <h1 class="title primColor" v-show="playerTurn == k" v-for="(input,k) in inputs" :key="k">Victoire de : {{input.name}}</h1>
      <button class="ajoutPoint" type="button" v-on:click="replay()">Rejouer</button>
    </div>
    <div>
      <div class="center">
        <h2 v-show="playerStart == k" v-for="(input,k) in inputs" :key="k">Joueur qui a commencé: {{input.name}}</h2>
        <h2>Point pour gagner: {{maxPoint}}</h2>
      </div>
      <div class="center mt2" v-show="playerTurn == k" v-for="(input,k) in inputs" :key="k">
        <h2>Tour de jeu: {{input.name}}</h2>
        <input type="number" class="inputPoint" v-bind:id="`point${input.id}`" placeholder="4" />
        <button class="ajoutPoint" value="0" v-on:click="addPoints(k)">Ajouter points</button>  
      </div>
      <div class="divPlayerStat">
        <h2 class="playerStat" v-show="k != 0" v-for="(input,k) in inputs" :key="k">{{input.name}} | {{input.points}} | <span v-bind:id="`strike${input.id}`"></span></h2> 
      </div>      
    </div>

  </div>

<div id="inscription">
  <h2 class="center mt5 subtitle">Paramètres et Joueurs</h2>
  <div class="addPlayers">
    <h2>Point à atteindre:</h2>
   <input type="number" id="maxPoint" placeholder="50" value="0" /> 
  </div>
  
  <div class="Players" v-for="(input,k) in inputs" :key="k">
    <input v-bind:id="`${input.id}`"  v-show="k != 0" class="primColor inputactive" v-model="input.name" disabled />
    <button v-show="k != 0" class="center mr-1 delete" v-on:click="remove(k)">Enlever</button> 
  </div>
  <div class="mt2 buttons">
    <h2>Ajouter les joueurs:</h2>
    <input id="name" placeholder="Nom du joueur" />
    <button class="center mr-1 ajoutPoint" v-on:click="add(k)">Ajouter un joueur</button>   
  </div>
  <div class="addPlayers mb5">
    <button class="center round mt2" v-on:click="play(k)"> Play </button>
  </div>
</div>
</template>

<script>
  export default {
  components: { IconEcosystem, IconSupport, IconDocumentation, IconCommunity },
    data() {
      return {
        players: 0,
        playersALL: 0,
        playerTurn: 2,
        maxPlayer: 0,
        maxPoint: 20,
        victory: 0,
        playerStart: 2,
        inputs: [
          {
            name: "",
            id: "",
            points: "",
            strike: ""
          }
        ]
      };
    },
    methods: {
      add(index) {
        this.players++;
        this.playersALL++;
        this.inputs.push({ name: document.getElementById("name").value, id: this.playersALL, points: "0"});
      },
      remove(index) {
        this.players--;
        this.inputs.splice(index, 1);      
      },
      play(index) {
        this.maxPoint = parseInt(document.getElementById("maxPoint").value);
        document.getElementById("inscription").style.display = "none";
        document.getElementById("game").style.display = "flex";
        this.maxPlayer = this.players;
        if(this.maxPoint <= 0 || this.maxPoint == null){
          this.maxPoint = 50;
        }
      },
      addPoints(index) {
        var currentPoint = parseInt(this.inputs[index].points);
        var point = parseInt(document.getElementById("point" + this.inputs[index].id).value);     
        var totalPoint = point + currentPoint;
        var strikeOut = document.getElementById("strike" + this.inputs[index].id).innerHTML;

        if(point == 0 && strikeOut == "X X" && this.maxPlayer == this.playerTurn){
          this.inputs.splice((this.playerTurn), 1);
          this.playerTurn = 1;
          this.players = this.players - 1;
          this.maxPlayer = this.maxPlayer - 1;
        }
        else if(point == 0 && strikeOut == "X X"){
          this.inputs.splice((this.playerTurn), 1);
          this.playerTurn = this.playerTurn - 1;
          this.players = this.players - 1;
          this.maxPlayer = this.maxPlayer - 1;
        }
        else if(point == 0 && strikeOut == "X"){
          document.getElementById("strike" + this.inputs[index].id).innerHTML = "X X";
        }
        else if(point == 0){
          document.getElementById("strike" + this.inputs[index].id).innerHTML = "X";
        }
        else{
          document.getElementById("strike" + this.inputs[index].id).innerHTML = "";
        }

        if(totalPoint == this.maxPoint){
          document.getElementById("victory").style.display = "block";
          this.victory = 1;
        }

        if(totalPoint > this.maxPoint){          
          totalPoint = Math.round(totalPoint / 2);
        }

        var points = totalPoint.toString();       
        this.inputs[index].points = points;

        if(this.playerTurn >= this.maxPlayer && this.victory != 1){
          this.playerTurn = 1;
        }
        else if(this.victory != 1){
          this.playerTurn++;
        }
        else{

        }
        document.getElementById("point" + this.inputs[index].id).value = 0;
      },
      replay(){
        window.location.reload();
      }
    }
  };
</script>
