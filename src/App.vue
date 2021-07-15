<template>
  <div id="app">
    <div id="logo">
      <img
        :src="require('./assets/bleu_logo+Waterbased+domotic.png')"
        alt="menu"
      />
    </div>
    <div id="up">
      <div id="data">
        <p><span class="sensorInfo">pH</span> {{ sensorph }}</p>
        <p>
          <span class="sensorInfo">ORP</span> {{ sensororp }} <span>mV</span>
        </p>
        <p>
          <span class="sensorInfo">EC</span> {{ sensorec }} <span>g/L</span>
        </p>
      </div>
      <div id="temp">
        <div id="air">
          <div id="tempair">
            <span class="type">air</span>
            <span> {{ tempair }}°</span>
            <p class="uv">UV {{ uv }}</p>
          </div>
          <div id="logoair"></div>
        </div>
        <div id="water">
          <span class="type">eau</span>
          <span> {{ tempwater }}°</span>
        </div>
      </div>
    </div>
    <div id="down">
      <!--BLOCK ONE : ELECTROLYSEUR-->
      <div id="blockOne" class="block">
        <div class="upBlock">
          <img src="./assets/eclair-eclair.png" alt="icone eclair" />
          <Button
            v-on:test-click="clickec"
            :bool="ec"
            idname="toggleec"
          ></Button>
        </div>
        <p class="title">Electolyseur</p>
        <div class="btn">
          <p class="optionBtn off" id="optionEcManu" @click="manuEc">MANU</p>
          <p class="optionBtn off" id="optionEcAuto" @click="autoEc">AUTO</p>
          <p class="optionBtn off" id="optionEcChoc" @click="chocEc">CHOC</p>
        </div>
      </div>
      <!--BLOCK TWO : ECLAIRAGE-->
      <div id="blockTwo" class="block">
        <div class="upBlock">
          <img src="./assets/ampoule-icon.png" alt="icone ampoule" />
          <Button
            v-on:test-click="clicklight"
            :bool="light"
            idname="togglelight"
          ></Button>
        </div>
        <p class="title" id="titlelight">Éclairage</p>
        <div id="colorChoice">
          <p class="off" id="colorlight" @click="cologChange">COULEUR ></p>
        </div>
        <div class="btn">
          <p class="optionBtn off" id="optionLightManu" @click="manuLight">
            MANU
          </p>
          <p class="optionBtn off" id="optionLightTimer" @click="timerLight">
            TIMER
          </p>
        </div>
        <Timer
          v-if="timerlight"
          mode="TimerEclairage"
          :hourUp="hourmorninglight"
          :minuteUp="minutemorninglight"
          :hourDown="hournightlight"
          :minuteDown="minutenightlight"
          v-on:add-up="addMorningLight"
          v-on:takeoff-up="takeoffMorningLight"
          v-on:add-down="addNightLight"
          v-on:takeoff-down="takeoffNightLight"
          v-on:set="settimerlight"
        ></Timer>
      </div>
      <!--BLOCK THREE : PAC-->
      <!--ATTENTION, dans la PAC tous ce qui est manu est en faite TIMER-->
      <div id="blockThree" class="block">
        <div class="upBlock">
          <img src="./assets/flame-icon.png" alt="icone ampoule" />
          <Button
            v-on:test-click="clickpac"
            :bool="pac"
            idname="togglepac"
          ></Button>
        </div>
        <div id="titletemp">
          <p class="title">PAC</p>
          <div id="tempPac">
            <img
              src="./assets/fleche-gauche.png"
              alt="fleche haute"
              @click="tempPlus"
            />
            <p class="tempPac off" id="setTempPac">{{ temppac }}°C</p>
            <img
              src="./assets/fleche-right.png"
              alt="fleche basse"
              @click="tempMinus"
            />
          </div>
        </div>
        <div class="btn">
          <p class="optionBtn off" id="optionPacAuto" @click="autoPac">AUTO</p>
          <p class="optionBtn off" id="optionPacManu" @click="manuPac">TIMER</p>
        </div>
        <Timer
          v-if="manupac"
          mode="TimerPAC"
          :hourUp="hourmorningpac"
          :minuteUp="minutemorningpac"
          :hourDown="hournightpac"
          :minuteDown="minutenightpac"
          v-on:add-up="addMorningPac"
          v-on:takeoff-up="takeoffMorningPac"
          v-on:add-down="addNightPac"
          v-on:takeoff-down="takeoffNightLightPac"
          v-on:set="settimerpac"
        ></Timer>
      </div>
      <!--BLOCK FOUR : FILTRATION-->
      <div id="blockFour" class="block">
        <div class="upBlock">
          <img src="./assets/filtre-icon.png" alt="icone ampoule" />
          <Button
            v-on:test-click="clickfiltr"
            :bool="filtr"
            idname="togglefiltr"
          ></Button>
        </div>
        <p class="title">Filtration</p>
        <div class="btn">
          <p class="optionBtn off" id="optionFilManu" @click="manuFil">MANU</p>
          <p class="optionBtn off" id="optionFilAuto" @click="autoFil">AUTO</p>
          <p class="optionBtn off" id="optionFilTimer" @click="timerFil">
            TIMER
          </p>
          <p class="optionBtn off" id="optionFilChoc" @click="chocFil">CHOC</p>
        </div>
        <Timer
          v-if="timerfil"
          mode="TimerFiltration"
          :hourUp="hourmorningfil"
          :minuteUp="minutemorningfil"
          :hourDown="hournightfil"
          :minuteDown="minutenightfil"
          v-on:add-up="addMorningFil"
          v-on:takeoff-up="takeoffMorningFil"
          v-on:add-down="addNightLightFil"
          v-on:takeoff-down="takeoffNightLightFil"
          v-on:set="settimerfil"
        ></Timer>
      </div>
    </div>
  </div>
</template>

<script>
import Button from "./components/Button.vue";
import Timer from "./components/Timer.vue";

export default {
  name: "App",
  components: {
    Button,
    Timer,
  },
  data() {
    return {
      //Récupération donnée des capteurs
      sensorph: "9.1",
      sensororp: "330",
      sensorec: "9.4",
      tempair: "32",
      tempwater: "28",
      //Récupération API météo
      uv: "7",
      //Activation ou non des éléments
      ec: false,
      light: false,
      pac: false,
      filtr: false,
      /* EC */
      chocec: false,
      autoec: false,
      manuec: false,
      /* light */
      timerlight: false,
      manulight: false,
      /* PAC */
      autopac: false,
      manupac: false,
      /* filtration */
      chocfil: false,
      timerfil: false,
      autofil: false,
      manufil: false,
      //Temp PAC et Timer
      temppac: 30,
      timeStep: 15,
      //Timer light
      hourmorninglight: 8,
      minutemorninglight: 30,
      hournightlight: 17,
      minutenightlight: 30,
      //Timer PAC
      hourmorningpac: 8,
      minutemorningpac: 30,
      hournightpac: 17,
      minutenightpac: 30,
      //Timer Filtration
      hourmorningfil: 8,
      minutemorningfil: 30,
      hournightfil: 17,
      minutenightfil: 30,
      //test
      test: 1,
    };
  },
  methods: {
    /*-----TRAITEMENT DE DONNEES-----*/
    setData(string) {
      let list = string.split(",");
      console.log("la liste splitée");
      console.log(list);
      //Timer filtration
      this.hourmorningfil = parseInt(list[0], 10);
      this.minutemorningfil = parseInt(list[1], 10);
      this.hournightfil = parseInt(list[2], 10);
      this.minutenightfil = parseInt(list[3], 10);
      //Timer éclairage
      this.hourmorninglight = parseInt(list[4], 10);
      this.minutemorninglight = parseInt(list[5], 10);
      this.hournightlight = parseInt(list[6], 10);
      this.minutenightlight = parseInt(list[7], 10);
      //Timer PAC
      this.hourmorningpac = parseInt(list[8], 10);
      this.minutemorningpac = parseInt(list[9], 10);
      this.hournightpac = parseInt(list[10], 10);
      this.minutenightpac = parseInt(list[11], 10);
      //Mode Filtration [auto,manu,timer,choc,off]
      if (parseInt(list[12], 10) == 0) {
        this.filtr = true;
        this.autofil = true;
        this.chocfil = false;
        this.timerfil = false;
        this.manufil = false;
      } else if (parseInt(list[12], 10) == 1) {
        this.filtr = true;
        this.autofil = false;
        this.chocfil = false;
        this.timerfil = false;
        this.manufil = true;
      } else if (parseInt(list[12], 10) == 2) {
        this.filtr = true;
        this.autofil = false;
        this.chocfil = false;
        this.timerfil = true;
        this.manufil = false;
      } else if (parseInt(list[12], 10) == 3) {
        this.filtr = true;
        this.autofil = false;
        this.chocfil = true;
        this.timerfil = false;
        this.manufil = false;
      } else if (parseInt(list[12], 10) == 4) {
        this.filtr = false;
        this.autofil = false;
        this.chocfil = false;
        this.timerfil = false;
        this.manufil = false;
      }
      //Mode Eclairage [manuel,timer,off]
      if (parseInt(list[13], 10) == 0) {
        this.light = true;
        this.timerlight = false;
        this.manulight = true;
      } else if (parseInt(list[13], 10) == 1) {
        this.light = true;
        this.timerlight = true;
        this.manulight = false;
      } else if (parseInt(list[13], 10) == 2) {
        this.light = false;
        this.timerlight = false;
        this.manulight = false;
      }
      //Mode PAC [auto,timer,off]
      if (parseInt(list[14], 10) == 0) {
        this.pac = true;
        this.autopac = true;
        this.manupac = false; //correspond à timer
      } else if (parseInt(list[14], 10) == 1) {
        this.pac = true;
        this.autopac = false;
        this.manupac = true; //correspond à timer
      } else if (parseInt(list[14], 10) == 2) {
        this.pac = false;
        this.autopac = false;
        this.manupac = false; //correspond à timer
      }
      //Mode EC [auto,manu,choc,off]
      if (parseInt(list[15], 10) == 0) {
        this.ec = true;
        this.chocec = false;
        this.autoec = true;
        this.manuec = false;
      } else if (parseInt(list[15], 10) == 1) {
        this.ec = true;
        this.chocec = false;
        this.autoec = false;
        this.manuec = true;
      } else if (parseInt(list[15], 10) == 2) {
        this.ec = true;
        this.chocec = true;
        this.autoec = false;
        this.manuec = false;
      } else if (parseInt(list[15], 10) == 3) {
        this.ec = false;
        this.chocec = false;
        this.autoec = false;
        this.manuec = false;
      }
      //Température pompe à chaleur
      this.temppac = parseInt(list[16], 10);
      //Sonde pH, ORP, EC, tempEau
      this.sensorph = parseFloat(list[17]).toFixed(1);
      this.sensororp = parseFloat(list[18]).toFixed(1);
      this.sensorec = parseFloat(list[19]).toFixed(1);
      this.tempwater = parseFloat(list[20]).toFixed(1);
      console.log("la lumiere true ou false");
      console.log(this.light);
    },
    /*-----REQUETES POST-----*/
    async postRequest(path) {
      try {
        const response = await fetch("/" + path, {
          method: "POST",
          headers: {
            //"Content-Type": "application/json" // En-tête du type de données envoyé
          },
          //body: JSON.stringify({ params: message })
        });
        const data = await response.text();
        console.log(data); //Debug
      } catch (error) {
        console.log(error);
      }
    },
    async postTimerRequest(
      mode,
      hourmorning,
      minutemorning,
      hournight,
      minutenight
    ) {
      try {
        const response = await fetch(
          `/timer?params=${mode}-${hourmorning}-${minutemorning}-${hournight}-${minutenight}`,
          {
            method: "POST",
          }
        );
        const data = await response.text();
        console.log(data); //Debug
      } catch (error) {
        console.log(error);
      }
    },
    /*-----REQUETES GET (on cheat en requetant un POST au lieu d'un GET)-----*/
    async getrequest() {
      try {
        const response = await fetch("/get", {
          method: "POST",
        });
        const data = await response.text();
        console.log("Reponse à la requête GET :");
        console.log(data); //Debug
        //this.setData(data);
      } catch (error) {
        console.log(error);
      }
    },
    /*-----Fonction mode-----*/
    onoffThreeOption(optionOne, optionTwo, optionThree, bool) {
      let optionAuto = document.getElementById(optionOne);
      let optionManu = document.getElementById(optionTwo);
      let optionTimer = document.getElementById(optionThree);
      if (bool) {
        /*---A ajouter--- Requete POST : Acvtivation lumières mode auto*/
        optionAuto.classList.replace("off", "on");
        optionManu.classList.replace("off", "act");
        optionTimer.classList.replace("off", "act");
      } else {
        //On grise tous les boutons car l'éclairage est OFF
        optionAuto.classList.replace("act", "off");
        optionAuto.classList.replace("on", "off");
        optionManu.classList.replace("act", "off");
        optionManu.classList.replace("on", "off");
        optionTimer.classList.replace("act", "off");
        optionTimer.classList.replace("on", "off");
      }
    },
    fourOption(
      optionOne,
      optionTwo,
      optionThree,
      optionFour,
      mainBool,
      boolOne
    ) {
      let optFour = document.getElementById(optionFour);
      let optThree = document.getElementById(optionThree);
      let optTwo = document.getElementById(optionTwo);
      let optOne = document.getElementById(optionOne);
      if (mainBool && boolOne) {
        /*---A ajouter--- Requete POST : Mode timer*/
        optOne.classList.replace("act", "on");
        optTwo.classList.replace("on", "act");
        optThree.classList.replace("on", "act");
        optFour.classList.replace("on", "act");
      } else {
        optOne.classList.replace("on", "act");
      }
    },
    threeOption(optionOne, optionTwo, optionThree, mainBool, boolOne) {
      let optThree = document.getElementById(optionThree);
      let optTwo = document.getElementById(optionTwo);
      let optOne = document.getElementById(optionOne);
      if (mainBool && boolOne) {
        /*---A ajouter--- Requete POST : Mode timer*/
        optOne.classList.replace("act", "on");
        optTwo.classList.replace("on", "act");
        optThree.classList.replace("on", "act");
      } else {
        optOne.classList.replace("on", "act");
      }
    },
    twoOption(optionOne, optionTwo, mainBool, boolOne) {
      let optTwo = document.getElementById(optionTwo);
      let optOne = document.getElementById(optionOne);
      if (mainBool && boolOne) {
        /*---A ajouter--- Requete POST : Mode timer*/
        optOne.classList.replace("act", "on");
        optTwo.classList.replace("on", "act");
      } else {
        optOne.classList.replace("on", "act");
      }
    },
    /*-----ELECTROLYSEUR-----*/
    clickec() {
      this.ec = !this.ec;
    },
    manuEc() {
      if (this.ec) {
        this.manuec = true;
        this.postRequest("mode?params=ElectrolyseurModeManuel");
        this.threeOption(
          "optionEcManu",
          "optionEcAuto",
          "optionEcChoc",
          this.ec,
          this.manuec
        );
        this.autoec = false;
        this.chocec = false;
      }
    },
    autoEc() {
      if (this.ec) {
        this.autoec = true;
        this.postRequest("mode?params=ElectrolyseurModeAuto");
        this.threeOption(
          "optionEcAuto",
          "optionEcManu",
          "optionEcChoc",
          this.ec,
          this.autoec
        );
        this.manuec = false;
        this.chocec = false;
      }
    },
    chocEc() {
      if (this.ec) {
        this.chocec = true;
        this.postRequest("mode?params=ElectrolyseurModeChoc");
        this.threeOption(
          "optionEcChoc",
          "optionEcManu",
          "optionEcAuto",
          this.ec,
          this.chocec
        );
        this.manuec = false;
        this.autoec = false;
      }
    },
    /*-----ECLAIRAGE-----*/
    clicklight() {
      this.light = !this.light;
    },
    manuLight() {
      if (this.light) {
        this.manulight = true;
        this.postRequest("mode?params=EclairageModeManuel");
        this.twoOption(
          "optionLightManu",
          "optionLightTimer",
          this.light,
          this.manulight
        );
        this.timerlight = false;
      }
    },
    timerLight() {
      if (this.light) {
        this.timerlight = true;
        this.twoOption(
          "optionLightTimer",
          "optionLightManu",
          this.light,
          this.timerlight
        );
        this.manulight = false;
      }
    },
    secondRequestColorChange() {
      try {
        const response = fetch("/mode?params=EclairageModeManuel", {
          method: "POST",
        });
        console.log("before test couleur lumière");
        const data = response.text();
        console.log(data); //Debug
      } catch (error) {
        console.log(error);
      }
    },
    cologChange() {
      if (this.light) {
        //First request
        try {
          const response = fetch("/mode?params=EclairageModeOff", {
            method: "POST",
          });
          console.log("before test couleur lumière");
          const data = response.text();
          console.log(data); //Debug
        } catch (error) {
          console.log(error);
        }
        //Second request
        setTimeout(this.secondRequestColorChange, 400);
      }
    },
    /*-----PAC-----*/
    clickpac() {
      this.pac = !this.pac;
    },
    //ATTENTION ! Correspond au timer !!!
    manuPac() {
      if (this.pac) {
        this.manupac = true;
        this.twoOption(
          "optionPacManu",
          "optionPacAuto",
          this.pac,
          this.manupac
        );
        this.autopac = false;
      }
    },
    autoPac() {
      if (this.pac) {
        this.autopac = true;
        this.postRequest("mode?params=PACModeAuto");
        this.twoOption(
          "optionPacAuto",
          "optionPacManu",
          this.pac,
          this.autopac
        );
        this.manupac = false;
      }
    },
    /*-----FILTRATION-----*/
    clickfiltr() {
      this.filtr = !this.filtr;
    },
    manuFil() {
      if (this.filtr) {
        this.manufil = true;
        this.postRequest("mode?params=FiltrationModeManuel");
        this.fourOption(
          "optionFilManu",
          "optionFilAuto",
          "optionFilTimer",
          "optionFilChoc",
          this.filtr,
          this.manufil
        );
        this.autofil = false;
        this.timerfil = false;
        this.chocfil = false;
      }
    },
    autoFil() {
      if (this.filtr) {
        this.autofil = true;
        this.postRequest("mode?params=FiltrationModeAuto");
        this.fourOption(
          "optionFilAuto",
          "optionFilManu",
          "optionFilTimer",
          "optionFilChoc",
          this.filtr,
          this.autofil
        );
        this.manufil = false;
        this.timerfil = false;
        this.chocfil = false;
      }
    },
    timerFil() {
      if (this.filtr) {
        this.timerfil = true;
        this.fourOption(
          "optionFilTimer",
          "optionFilManu",
          "optionFilAuto",
          "optionFilChoc",
          this.filtr,
          this.timerfil
        );
        this.manufil = false;
        this.autofil = false;
        this.chocfil = false;
      }
    },
    chocFil() {
      if (this.filtr) {
        this.chocfil = true;
        this.postRequest("mode?params=FiltrationModeChoc");
        this.fourOption(
          "optionFilChoc",
          "optionFilManu",
          "optionFilAuto",
          "optionFilTimer",
          this.filtr,
          this.chocfil
        );
        this.manufil = false;
        this.autofil = false;
        this.timerfil = false;
      }
    },
    /*-----Réglage temp PAC-----*/
    tempPlus() {
      if (this.pac && this.temppac < 40) {
        this.temppac += 1;
        this.postRequest(`tCommande?params=${this.temppac}`);
      }
    },
    tempMinus() {
      if (this.pac && this.temppac > 20) {
        this.temppac -= 1;
        this.postRequest(`tCommande?params=${this.temppac}`);
      }
    },
    /*-----Réglage TIMER-----*/
    addMorning(minutemorning, hourmorning, timeStep) {
      if (minutemorning <= 60 - timeStep) {
        minutemorning += timeStep;
        return minutemorning;
      }
      if (minutemorning == 60) {
        hourmorning += 1;
        minutemorning = 0;
        return hourmorning;
      }
      if (hourmorning == 24) {
        hourmorning = 0;
        return hourmorning;
      }
    },
    takeoffMorning(minutemorning, hourmorning, timeStep) {
      if (minutemorning >= timeStep) {
        minutemorning -= timeStep;
      }
      if (minutemorning == 0) {
        hourmorning -= 1;
        minutemorning = 60 - timeStep;
      }
      if (hourmorning == -1) {
        hourmorning = 23;
      }
    },
    addNight(minutenight, hournight, timeStep) {
      if (minutenight <= 60 - timeStep) {
        minutenight += timeStep;
      }
      if (minutenight == 60) {
        hournight += 1;
        minutenight = 0;
      }
      if (hournight == 24) {
        hournight = 0;
      }
    },
    takeoffNight(minutenight, hournight, timeStep) {
      if (minutenight >= timeStep) {
        minutenight -= timeStep;
      }
      if (minutenight == 0) {
        hournight -= 1;
        minutenight = 60 - timeStep;
      }
      if (hournight == -1) {
        hournight = 23;
      }
    },
    /*Timer light*/
    addMorningLight() {
      this.addMorning(
        this.minutemorninglight,
        this.hourmorninglight,
        this.timeStep
      );
    },
    takeoffMorningLight() {
      this.takeoffMorning(
        this.minutemorninglight,
        this.hourmorninglight,
        this.timeStep
      );
    },
    addNightLight() {
      this.addNight(this.minutenightlight, this.hournightlight, this.timeStep);
    },
    takeoffNightLight() {
      this.takeoffNight(
        this.minutenightlight,
        this.hournightlight,
        this.timeStep
      );
    },
    settimerlight() {
      this.postTimerRequest(
        "TimerEclairage",
        this.hourmorninglight,
        this.minutemorninglight,
        this.hournightlight,
        this.minutenightlight
      );
    },
    /*Timer pac*/
    addMorningPac() {
      this.addMorning(
        this.minutemorningpac,
        this.hourmorningpac,
        this.timeStep
      );
    },
    takeoffMorningPac() {
      this.takeoffMorning(
        this.minutemorningpac,
        this.hourmorningpac,
        this.timeStep
      );
    },
    addNightLightPac() {
      this.addNight(this.minutenightpac, this.hournightpac, this.timeStep);
    },
    takeoffNightLightPac() {
      this.takeoffNight(this.minutenightpac, this.hournightpac, this.timeStep);
    },
    settimerpac() {
      this.postTimerRequest(
        "TimerPAC",
        this.hourmorningpac,
        this.minutemorningpac,
        this.hournightpac,
        this.minutenightpac
      );
    },
    /*Timer Filtration*/
    addMorningFil() {
      this.addMorning(
        this.minutemorningfil,
        this.hourmorningfil,
        this.timeStep
      );
    },
    takeoffMorningFil() {
      this.takeoffMorning(
        this.minutemorningfil,
        this.hourmorningfil,
        this.timeStep
      );
    },
    addNightLightFil() {
      this.addNight(this.minutenightfil, this.hournightfil, this.timeStep);
    },
    takeoffNightLightFil() {
      this.takeoffNight(this.minutenightfil, this.hournightfil, this.timeStep);
    },
    settimerfil() {
      this.postTimerRequest(
        "TimerFiltration",
        this.hourmorningfil,
        this.minutemorningfil,
        this.hournightfil,
        this.minutenightfil
      );
    },
    /*--------------------- */
    testfun() {
      console.log(this.test);
      this.test += 1;
    },
  },
  /*-----WATCHERS-----*/
  watch: {
    //Electrolyseur
    ec: function(ec) {
      let optionAuto = document.getElementById("optionEcAuto");
      let optionManu = document.getElementById("optionEcManu");
      let optionTimer = document.getElementById("optionEcChoc");
      if (
        ec &&
        this.chocec == false &&
        this.autoec == false &&
        this.manuec == false
      ) {
        this.autoec = true;
        this.postRequest("mode?params=ElectrolyseurModeAuto"); //Requete pour allumer les l'ec
        optionAuto.classList.replace("off", "on");
        optionManu.classList.replace("off", "act");
        optionTimer.classList.replace("off", "act");
      } else if (ec == false) {
        this.chocec = false;
        this.autoec = false;
        this.manuec = false;
        this.postRequest("mode?params=ElectrolyseurModeOff"); //Requete pour allumer les l'ec
        //On grise tous les boutons car l'éclairage est OFF
        optionAuto.classList.replace("act", "off");
        optionAuto.classList.replace("on", "off");
        optionManu.classList.replace("act", "off");
        optionManu.classList.replace("on", "off");
        optionTimer.classList.replace("act", "off");
        optionTimer.classList.replace("on", "off");
      }
    },
    chocec: function(chocec) {
      let optionAuto = document.getElementById("optionEcAuto");
      let optionManu = document.getElementById("optionEcManu");
      let optionTimer = document.getElementById("optionEcChoc");
      if (chocec && this.ec) {
        this.autoec = false;
        this.manuec = false;
        optionAuto.classList.replace("off", "act");
        optionManu.classList.replace("off", "act");
        optionTimer.classList.replace("off", "on");
        optionTimer.classList.replace("act", "on");
      } else if (this.ec && chocec == false) {
        optionTimer.classList.replace("on", "act");
        optionTimer.classList.replace("off", "act");
      }
    },
    autoec: function(autoec) {
      let optionAuto = document.getElementById("optionEcAuto");
      let optionManu = document.getElementById("optionEcManu");
      let optionTimer = document.getElementById("optionEcChoc");
      if (autoec && this.ec) {
        this.chocec = false;
        this.manuec = false;
        optionAuto.classList.replace("off", "on");
        optionAuto.classList.replace("act", "on");
        optionManu.classList.replace("off", "act");
        optionTimer.classList.replace("off", "act");
      } else if (this.ec && autoec == false) {
        optionAuto.classList.replace("on", "act");
        optionAuto.classList.replace("off", "act");
      }
    },
    manuec: function(manuec) {
      let optionAuto = document.getElementById("optionEcAuto");
      let optionManu = document.getElementById("optionEcManu");
      let optionTimer = document.getElementById("optionEcChoc");
      if (manuec && this.ec) {
        this.chocec = false;
        this.autoec = false;
        optionAuto.classList.replace("off", "act");
        optionManu.classList.replace("off", "on");
        optionManu.classList.replace("act", "on");
        optionTimer.classList.replace("off", "act");
      } else if (this.ec && manuec == false) {
        optionManu.classList.replace("on", "act");
        optionManu.classList.replace("off", "act");
      }
    },
    //Light
    light: function(light) {
      let optionManu = document.getElementById("optionLightManu");
      let optionTimer = document.getElementById("optionLightTimer");
      let lightcolor = document.getElementById("colorlight");
      if (light && this.manulight == false && this.timerlight == false) {
        this.manulight = true;
        this.postRequest("mode?params=EclairageModeManuel"); //Requete pour allumer les eclairages
        optionManu.classList.replace("off", "on");
        optionTimer.classList.replace("off", "act");
        lightcolor.classList.replace("off", "on");
      } else if (light == false) {
        this.manulight = false;
        this.timerlight = false;
        this.postRequest("mode?params=EclairageModeOff"); //Requete pour éteindre les eclairages
        //On grise tous les boutons car l'éclairage est OFF
        optionManu.classList.replace("act", "off");
        optionManu.classList.replace("on", "off");
        optionTimer.classList.replace("act", "off");
        optionTimer.classList.replace("on", "off");
        lightcolor.classList.replace("on", "off");
      }
    },
    manulight: function(manulight) {
      let optionManu = document.getElementById("optionLightManu");
      let optionTimer = document.getElementById("optionLightTimer");
      let lightcolor = document.getElementById("colorlight");
      if (manulight && this.light) {
        this.timerlight = false;
        optionManu.classList.replace("off", "on");
        optionManu.classList.replace("act", "on");
        optionTimer.classList.replace("off", "act");
        lightcolor.classList.replace("off", "on");
      } else if (this.light && manulight == false) {
        optionManu.classList.replace("on", "act");
        optionManu.classList.replace("act", "act");
      }
    },
    timerlight: function(timerlight) {
      let optionManu = document.getElementById("optionLightManu");
      let optionTimer = document.getElementById("optionLightTimer");
      let lightcolor = document.getElementById("colorlight");
      if (timerlight && this.light) {
        this.manulight = false;
        optionManu.classList.replace("off", "act");
        optionTimer.classList.replace("off", "on");
        optionTimer.classList.replace("act", "on");
        lightcolor.classList.replace("off", "on");
      } else if (this.light && timerlight == false) {
        optionTimer.classList.replace("on", "act");
        optionTimer.classList.replace("act", "act");
      }
    },
    //PAC
    pac: function(pac) {
      let optionAuto = document.getElementById("optionPacAuto");
      let optionManu = document.getElementById("optionPacManu");
      let temp = document.getElementById("setTempPac");
      if (pac && this.autopac == false && this.manupac == false) {
        this.autopac = true;
        this.postRequest("mode?params=PACModeAuto"); //Requete pour allumer la pac
        optionAuto.classList.replace("off", "on");
        optionManu.classList.replace("off", "act");
        temp.classList.replace("off", "on");
      } else if (pac == false) {
        this.autopac = false;
        this.manupac = false;
        this.postRequest("mode?params=PACModeOff"); //Requete pour allumer la pac
        //On grise tous les boutons car l'éclairage est OFF
        optionAuto.classList.replace("act", "off");
        optionAuto.classList.replace("on", "off");
        optionManu.classList.replace("act", "off");
        optionManu.classList.replace("on", "off");
        temp.classList.replace("on", "off");
      }
    },
    autopac: function(autopac) {
      let optionAuto = document.getElementById("optionPacAuto");
      let optionManu = document.getElementById("optionPacManu");
      let temp = document.getElementById("setTempPac");
      if (autopac && this.pac) {
        this.manupac = false;
        optionAuto.classList.replace("off", "on");
        optionAuto.classList.replace("act", "on");
        optionManu.classList.replace("off", "act");
        temp.classList.replace("off", "on");
      } else if (this.pac && autopac == false) {
        optionAuto.classList.replace("on", "act");
        optionAuto.classList.replace("act", "act");
      }
    },
    manupac: function(manupac) {
      let optionAuto = document.getElementById("optionPacAuto");
      let optionManu = document.getElementById("optionPacManu");
      let temp = document.getElementById("setTempPac");
      if (manupac && this.pac) {
        this.autopac = false;
        optionAuto.classList.replace("off", "act");
        optionManu.classList.replace("off", "on");
        optionManu.classList.replace("act", "on");
        temp.classList.replace("off", "on");
      } else if (this.pac && manupac == false) {
        optionManu.classList.replace("on", "act");
        optionManu.classList.replace("act", "act");
      }
    },
    //Filtration
    filtr: function(filtr) {
      let optionAuto = document.getElementById("optionFilAuto");
      let optionManu = document.getElementById("optionFilManu");
      let optionTimer = document.getElementById("optionFilTimer");
      let optionChoc = document.getElementById("optionFilChoc");
      if (
        filtr &&
        this.manufil == false &&
        this.autofil == false &&
        this.timerfil == false &&
        this.chocfil == false
      ) {
        this.autofil = true;
        this.postRequest("mode?params=FiltrationModeAuto"); //Requete pour allumer les l'ec
        /*---A ajouter--- Requete POST : Acvtivation lumières mode auto*/
        optionAuto.classList.replace("off", "on");
        optionManu.classList.replace("off", "act");
        optionTimer.classList.replace("off", "act");
        optionChoc.classList.replace("off", "act");
      } else if (filtr == false) {
        this.manufil = false;
        this.autofil = false;
        this.timerfil = false;
        this.chocfil = false;
        this.postRequest("mode?params=FiltrationModeOff"); //Requete pour allumer les l'ec
        //On grise tous les boutons car l'éclairage est OFF
        optionAuto.classList.replace("act", "off");
        optionAuto.classList.replace("on", "off");
        optionManu.classList.replace("act", "off");
        optionManu.classList.replace("on", "off");
        optionTimer.classList.replace("act", "off");
        optionTimer.classList.replace("on", "off");
        optionChoc.classList.replace("act", "off");
        optionChoc.classList.replace("on", "off");
      }
    },
    manufil: function(manufil) {
      let optionAuto = document.getElementById("optionFilAuto");
      let optionManu = document.getElementById("optionFilManu");
      let optionTimer = document.getElementById("optionFilTimer");
      let optionChoc = document.getElementById("optionFilChoc");
      if (manufil && this.filtr) {
        this.autofil = false;
        this.timerfil = false;
        this.chocfil = false;
        optionAuto.classList.replace("off", "act");
        optionManu.classList.replace("off", "on");
        optionManu.classList.replace("act", "on");
        optionTimer.classList.replace("off", "act");
        optionChoc.classList.replace("off", "act");
      } else if (this.filtr && manufil == false) {
        optionManu.classList.replace("on", "act");
        optionManu.classList.replace("off", "act");
      }
    },
    autofil: function(autofil) {
      let optionAuto = document.getElementById("optionFilAuto");
      let optionManu = document.getElementById("optionFilManu");
      let optionTimer = document.getElementById("optionFilTimer");
      let optionChoc = document.getElementById("optionFilChoc");
      if (autofil && this.filtr) {
        this.manufil = false;
        this.timerfil = false;
        this.chocfil = false;
        optionAuto.classList.replace("off", "on");
        optionAuto.classList.replace("act", "on");
        optionManu.classList.replace("off", "act");
        optionTimer.classList.replace("off", "act");
        optionChoc.classList.replace("off", "act");
      } else if (this.filtr && autofil == false) {
        optionAuto.classList.replace("on", "act");
        optionAuto.classList.replace("off", "act");
      }
    },
    timerfil: function(timerfil) {
      let optionAuto = document.getElementById("optionFilAuto");
      let optionManu = document.getElementById("optionFilManu");
      let optionTimer = document.getElementById("optionFilTimer");
      let optionChoc = document.getElementById("optionFilChoc");
      if (timerfil && this.filtr) {
        this.manufil = false;
        this.autofil = false;
        this.chocfil = false;
        optionAuto.classList.replace("off", "act");
        optionManu.classList.replace("off", "act");
        optionTimer.classList.replace("off", "on");
        optionTimer.classList.replace("act", "on");
        optionChoc.classList.replace("off", "act");
      } else if (this.filtr && timerfil == false) {
        optionTimer.classList.replace("on", "act");
        optionTimer.classList.replace("off", "act");
      }
    },
    chocfil: function(chocfil) {
      let optionAuto = document.getElementById("optionFilAuto");
      let optionManu = document.getElementById("optionFilManu");
      let optionTimer = document.getElementById("optionFilTimer");
      let optionChoc = document.getElementById("optionFilChoc");
      if (chocfil && this.filtr) {
        this.manufil = false;
        this.autofil = false;
        this.timerfil = false;
        optionAuto.classList.replace("off", "act");
        optionManu.classList.replace("off", "act");
        optionTimer.classList.replace("off", "act");
        optionChoc.classList.replace("off", "on");
        optionChoc.classList.replace("act", "on");
      } else if (this.filtr && chocfil == false) {
        optionChoc.classList.replace("on", "act");
        optionChoc.classList.replace("off", "act");
      }
    },
  },
  /*---------------MOUNTED---------------*/
  mounted: function() {
    //setInterval(this.getrequest, 100);
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: rgb(33, 52, 66);
  background-image: url(./assets/background.svg);
  background-size: cover;
  min-height: 100vh;
  height: scroll;
  width: 100vw;
}
/*-----Partie haute (logo)-----*/
#logo {
  margin: 10px;
}
#logo img {
  width: 30vw;
  margin-right: 10px;
}
#logo h1 {
  color: rgb(54, 200, 214);
  font-style: italic;
  padding-top: 20px;
  margin-left: 20px;
}
/*-----Partie haute GAUCHE (hors logo)-----*/
#up {
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
}
#data {
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;
  width: 60vw;
  margin-top: 1vw;
}
#data p {
  font-size: 250%;
  border: 4px solid rgb(54, 200, 214);
  border-radius: 15px;
  padding: 10px;
  height: 60px;
  margin-top: 1vw;
  margin-bottom: 1vw;
}
#data p .sensorInfo {
  font-size: 100%;
  font-weight: bold;
}
#data p span {
  font-size: 60%;
}
/*-----Partie haute DROITE (hors logo)-----*/
#temp {
  position: relative;
  top: -2vw;
  margin-right: 10px;
}
#temp #air,
#temp #water {
  margin-top: 6vw;
  margin-bottom: 3vw;
}
#temp #water {
  color: white;
}
#temp #air span,
#temp #water span {
  font-size: 50px;
}
#temp #air #tempair,
#temp #water {
  display: flex;
}
#temp #air #tempair .type,
#temp #water .type {
  font-size: 20px;
}
#temp #air #tempair .uv {
  font-size: 15px;
  border: 2px solid rgb(33, 52, 66);
  border-radius: 5px;
  font-weight: bold;
  padding: 3px 15px 3px 15px;
  height: 21px;
  align-self: flex-end;
}
/*-----Partie basse-----*/
#down {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
}
#down .block {
  width: 220px;
  min-height: 220px;
  border-radius: 20px;
  background-color: white;
  margin-top: 5vw;
  margin-bottom: 5vw;
  margin-right: 10px;
  margin-left: 10px;
}
#down .block .upBlock {
  display: flex;
  justify-content: space-between;
  margin: 15px;
}
#down .block .upBlock img {
  height: 35px;
}
#down .block .btn .optionBtn {
  color: white;
}
#down .block .btn .off {
  background-color: #ccc;
}
#down .block .btn .on {
  background-color: rgb(54, 200, 214);
}
#down .block .btn .act {
  color: rgb(54, 200, 214);
  border: 1px solid rgb(54, 200, 214);
}
#down .block .btn {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
#down .block .btn p {
  width: 30%;
  text-align: center;
  margin: 3%;
  padding: 2%;
  border-radius: 15px;
}
#down .block .title {
  font-weight: bold;
  font-size: 110%;
  margin-left: 10px;
  height: 60px;
}
#down .block #titlelight {
  height: 30px;
}
#down .block #colorChoice {
  height: 30px;
  text-align: center;
}
#down .block #colorChoice .on,
#down .block #colorChoice .off {
  background-color: rgb(54, 200, 214);
  border-radius: 20px;
  padding: 1px;
  width: 100px;
  font-size: 80%;
  margin-left: auto;
  margin-right: auto;
}
#down .block #colorChoice .on:active {
  background-color: white;
  border: 1px solid rgb(54, 200, 214);
  color: rgb(54, 200, 214);
}
#down .block #colorChoice .off {
  background-color: #ccc;
  color: white;
}
/*-----Temp PAC et Timer-----*/
#blockThree #titletemp {
  display: flex;
  height: 60px;
}
#tempPac {
  display: flex;
  flex-direction: column;
  margin-left: 30px;
  position: relative;
  top: -28px;
}
#tempPac .on {
  color: rgb(54, 200, 214);
  font-weight: bold;
}
#tempPac .off {
  color: #ccc;
  font-weight: bold;
}
#tempPac img {
  width: 30px;
}
/*RESPONSIVE*/
@media only screen and (max-width: 962px) {
  #up {
    justify-content: center;
  }
  #data {
    width: 100vw;
    margin-top: 0;
  }
  #data p {
    height: 50px;
  }
  #temp #air {
    margin-top: 2vw;
  }
  #temp #water {
    margin-top: 7vw;
    margin-bottom: 0;
  }
}
@media only screen and (min-width: 578px) and (max-width: 955px) {
  #temp #air {
    margin-top: 5vw;
    margin-bottom: 10vw;
    margin-right: 10px;
    display: inline-block;
    position: relative;
    top: 1vw;
  }
  #temp #water {
    margin-top: 5vw;
    margin-bottom: 0;
    margin-left: 10px;
    display: inline-block;
    color: rgb(33, 52, 66);
  }
  #temp #air #tempair .type {
    align-self: flex-end;
    position: relative;
    top: -1vw;
    margin-right: 10px;
  }
  #down .block {
    margin-top: 1vw;
    margin-bottom: 1vw;
  }
}
@media only screen and (min-width: 477px) and (max-width: 577px) {
  #data {
    margin-top: 5vw;
    margin-bottom: 5vw;
  }
  #data p {
    font-size: 200%;
    padding: 10px;
    height: 40px;
    margin-top: 1vw;
    margin-bottom: 1vw;
  }
  #data p .sensorInfo {
    font-size: 100%;
    font-weight: bold;
  }
  #data p span {
    font-size: 60%;
  }
  #temp #water {
    margin-top: 8vw;
  }
}
@media only screen and (max-width: 476px) {
  #app {
    background-image: url(./assets/background_mobile.svg);
  }
  #up {
    justify-content: space-around;
    margin-top: 10vw;
    margin-bottom: 5vw;
  }
  #data {
    text-align: center;
    width: 45vw;
    flex-direction: column;
  }
  #data p {
    font-size: 180%;
    height: 40px;
  }
  #temp {
    position: relative;
    top: 8vw;
  }
  #temp #water {
    color: rgb(33, 52, 66);
  }
  #temp #air span,
  #temp #water span {
    font-size: 280%;
  }
}
@media only screen and (max-width: 393px) {
  #data p {
    font-size: 170%;
  }
  #data p .sensorInfo {
    font-size: 90%;
  }
  #data p span {
    font-size: 50%;
  }
  #temp #air span,
  #temp #water span {
    font-size: 260%;
  }
}
@media only screen and (max-width: 354px) {
  #data p {
    font-size: 160%;
  }
  #data p .sensorInfo {
    font-size: 80%;
  }
  #data p span {
    font-size: 40%;
  }
  #temp #air span,
  #temp #water span {
    font-size: 250%;
  }
  #temp #air #tempair .uv {
    font-size: 70%;
    height: 4vw;
  }
  #temp #air #tempair .type,
  #temp #water .type {
    font-size: 90%;
  }
}
@media only screen and (max-width: 320px) {
  #up {
    justify-content: center;
    margin-top: 1vw;
  }
  #data {
    width: 100vw;
    margin-top: 0;
  }
  #data p {
    width: 50vw;
    height: 30px;
    align-self: center;
  }
  #temp {
    margin-top: 10vw;
    margin-bottom: 10vw;
  }
}
</style>
