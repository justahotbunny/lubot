<template>
  <div>
    <div
      v-if="info != null"
      class="courses-container"
      v-bind:class="{ slide: slideIn, hide: hide }"
    >
      <div class="course">
        <div class="course-preview">
          <h1>{{ info["summonerName"] }}</h1>
          <img
            v-if="info['tier'] === 'PLATINUM'"
            alt="elo"
            class="eloImg"
            src="@/assets/PLATINUM.png"
          />
          <img
            v-if="info['tier'] === 'DIAMOND'"
            alt="elo"
            class="eloImg"
            src="@/assets/DIAMOND.png"
          />
          <img
            v-if="info['tier'] === 'MASTER'"
            alt="elo"
            class="eloImg"
            src="@/assets/MASTER.png"
          />
          <img
            v-if="info['tier'] === 'GRANDMASTER'"
            alt="elo"
            class="eloImg"
            src="@/assets/GRANDMASTER.png"
          />
          <img
            v-if="info['tier'] === 'CHALLENGER'"
            alt="elo"
            class="eloImg"
            src="@/assets/CHALLENGER.png"
          />
          <h2>{{ info["rank"] }}</h2>
          <h6>Winrate</h6>
          <h6>
            {{
              ((info["wins"] / (info["wins"] + info["losses"])) * 100).toFixed(
                2
              )
            }}
          </h6>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

const tmi = require("tmi.js");

export default {
  name: "LolCard",
  props: {
    summoner: String,
    API: String,
  },
  data: function() {
    return {
      info: null,
      slideIn: false,
      hide: false,
      image: null,
    };
  },
  metods: {},
  mounted() {
    let options = {
      options: {
        debug: true,
      },
      connection: {
        reconnect: true,
        secure: true,
      },
      channels: ["rivallesss"],
    };

    let client = new tmi.client(options);

    // Connect the client to the server..
    client.connect();

    client.on("message", (channel, tags, message) => {
      if (message.toLowerCase() === "!elo") {
        if (this.info == null) {
          axios
            .get(
              "https://cors-anywhere.herokuapp.com/https://euw1.api.riotgames.com/lol/league/v4/entries/by-summoner/" +
                this.summoner +
                "?api_key=" +
                this.API
            )
            .then((response) => {
              // JSON responses are automatically parsed.
              if (response.data[0]["queueType"] == "RANKED_SOLO_5x5") {
                this.info = response.data[0];
                this.image = "@/assets" + response.data[0]["rank"] + ".png";
              } else {
                this.info = response.data[1];
              }
            })
            .catch((e) => {
              this.errors.push(e);
            });
        }
        this.hide = false;
        this.slideIn = true;
        setTimeout(() => {
          this.slideIn = false;
          this.hide = true;
        }, 8000);
      }
    });
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import url("https://fonts.googleapis.com/css?family=Muli&display=swap");

* {
  box-sizing: border-box;
}

body {
  background-image: linear-gradient(45deg, #7175da, #9790f2);
  font-family: "Muli", sans-serif;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  min-height: 100vh;
  margin: 0;
}

.courses-container {
  opacity: 0;
}
.course {
  background-color: #fff;
  border-radius: 10px;
  box-shadow: 0 10px 10px rgba(0, 0, 0, 0.2);
  display: flex;
  max-width: 100%;
  margin: 20px;
  overflow: hidden;
  width: 210px;
}

.course h6 {
  opacity: 0.6;
  margin: 0;
  letter-spacing: 1px;
  text-transform: uppercase;
}

.course h2 {
  letter-spacing: 1px;
  margin: 10px 0;
}

.course-preview {
  background-color: #2a265f;
  color: #fff;
  padding: 30px;
  max-width: 250px;
}

.course-preview a {
  color: #fff;
  display: inline-block;
  font-size: 12px;
  opacity: 0.6;
  margin-top: 30px;
  text-decoration: none;
}

.course-info {
  padding: 30px;
  position: relative;
  width: 100%;
}

.eloImg {
  width: 150px;
  height: auto;
}

.progress-container {
  position: absolute;
  top: 30px;
  right: 30px;
  text-align: right;
  width: 150px;
}

.progress {
  background-color: #ddd;
  border-radius: 3px;
  height: 5px;
  width: 100%;
}

.progress::after {
  border-radius: 3px;
  background-color: #2a265f;
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  height: 5px;
  width: 66%;
}

.progress-text {
  font-size: 10px;
  opacity: 0.6;
  letter-spacing: 1px;
}

.btn {
  background-color: #2a265f;
  border: 0;
  border-radius: 50px;
  box-shadow: 0 10px 10px rgba(0, 0, 0, 0.2);
  color: #fff;
  font-size: 16px;
  padding: 12px 25px;
  position: absolute;
  bottom: 30px;
  right: 30px;
  letter-spacing: 1px;
}

/* SOCIAL PANEL CSS */
.social-panel-container {
  position: fixed;
  right: 0;
  bottom: 80px;
  transform: translateX(100%);
  transition: transform 0.4s ease-in-out;
}

.social-panel-container.visible {
  transform: translateX(-10px);
}

.social-panel {
  background-color: #fff;
  border-radius: 16px;
  box-shadow: 0 16px 31px -17px rgba(0, 31, 97, 0.6);
  border: 5px solid #001f61;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  font-family: "Muli";
  position: relative;
  height: 169px;
  width: 370px;
  max-width: calc(100% - 10px);
}

.social-panel button.close-btn {
  border: 0;
  color: #97a5ce;
  cursor: pointer;
  font-size: 20px;
  position: absolute;
  top: 5px;
  right: 5px;
}

.social-panel button.close-btn:focus {
  outline: none;
}

.social-panel p {
  background-color: #001f61;
  border-radius: 0 0 10px 10px;
  color: #fff;
  font-size: 14px;
  line-height: 18px;
  padding: 2px 17px 6px;
  position: absolute;
  top: 0;
  left: 50%;
  margin: 0;
  transform: translateX(-50%);
  text-align: center;
  width: 235px;
}

.social-panel p i {
  margin: 0 5px;
}

.social-panel p a {
  color: #ff7500;
  text-decoration: none;
}

.social-panel h4 {
  margin: 20px 0;
  color: #97a5ce;
  font-family: "Muli";
  font-size: 14px;
  line-height: 18px;
  text-transform: uppercase;
}

.social-panel ul {
  display: flex;
  list-style-type: none;
  padding: 0;
  margin: 0;
}

.social-panel ul li {
  margin: 0 10px;
}

.social-panel ul li a {
  border: 1px solid #dce1f2;
  border-radius: 50%;
  color: #001f61;
  font-size: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 50px;
  width: 50px;
  text-decoration: none;
}

.social-panel ul li a:hover {
  border-color: #ff6a00;
  box-shadow: 0 9px 12px -9px #ff6a00;
}

.floating-btn {
  border-radius: 26.5px;
  background-color: #001f61;
  border: 1px solid #001f61;
  box-shadow: 0 16px 22px -17px #03153b;
  color: #fff;
  cursor: pointer;
  font-size: 16px;
  line-height: 20px;
  padding: 12px 20px;
  position: fixed;
  bottom: 20px;
  right: 20px;
  z-index: 999;
}

.floating-btn:hover {
  background-color: #ffffff;
  color: #001f61;
}

.floating-btn:focus {
  outline: none;
}

.floating-text {
  background-color: #001f61;
  border-radius: 10px 10px 0 0;
  color: #fff;
  font-family: "Muli";
  padding: 7px 15px;
  position: fixed;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  text-align: center;
  z-index: 998;
}

.floating-text a {
  color: #ff7500;
  text-decoration: none;
}

@media screen and (max-width: 480px) {
  .social-panel-container.visible {
    transform: translateX(0px);
  }

  .floating-btn {
    right: 10px;
  }
}

.slide {
  position: absolute;
  left: -500px;
  -webkit-animation: slide 0.5s forwards;
  -webkit-animation-delay: 2s;
  animation: slide 0.5s forwards;
  animation-delay: 2s;
  opacity: 1;
}

@-webkit-keyframes slide {
  100% {
    left: 0;
  }
}

@keyframes slide {
  100% {
    left: 0;
  }
}

.hide {
  position: absolute;
  visibility: hidden;
  opacity: 0;
  transition: visibility 0s 2s, opacity 2s linear;
}
</style>
