<template>
  <div class="site-wrapper" :class="{ 'site-loaded': siteLoaded }">
    <header ref="headerEl">
      <div class="logo" ref="logoEl">
        <div class="logo-wrapper">
          <svg
            class="logo-mark"
            viewBox="0 0 100 100"
            width="16"
            height="16"
            ref="logoMark"
          >
            <rect width="30" height="60" fill="white" y="20" x="50" />
          </svg>
          <h1 class="logo-word split-text">
            <span
              v-for="char in formattedText"
              :key="char + 'logo'"
              class="char"
            >
              {{ char }}
            </span>
          </h1>
        </div>
      </div>
      <div class="site-heading till-load fake-heading" ref="siteHeading">
        <h1 ref="siteHeadingH1">_projects_</h1>
        <span class="heading-border"></span>
      </div>

      <div class="real-site-heading till-load">
        <h1>_projects_</h1>
      </div>
      <fancy-button class="menu-button till-load" />
      <span class="header-background"></span>
    </header>
    <main class="till-load main-body">
      <div class="projects">
        <div class="project" v-for="(proj, i) in projects" :key="proj + i">
          <div class="project-content">
            <h4 class="project-name">Parks & Recs</h4>
            <div class="labels">
              <span v-for="label in labels" :key="label + proj"> GSAP</span>
            </div>
          </div>
          <div class="project-bg">
            <div class="project-bg-inner"></div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import { gsap } from "gsap";

import { ScrollTrigger } from "gsap/all";

gsap.registerPlugin(ScrollTrigger);

import FancyButton from "./components/FancyButton.vue";

const logoTl = gsap.timeline();
const headingTl = gsap.timeline({
  scrollTrigger: {
    scrub: true,
    end: 50,
  },
});

export default {
  components: { FancyButton },
  data() {
    return {
      text: "regency",
      allChars: "0123456789abcdefghijklmnopqrstuvxyz",
      siteLoaded: false,
      projects: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
      labels: [1, 2, 3],
    };
  },
  computed: {
    formattedText() {
      return this.text.split("");
    },
    formattedAllChars() {
      return this.allChars.split("");
    },
  },
  mounted() {
    const logoEl = this.$refs.logoEl;
    const logoBox = logoEl.getBoundingClientRect();
    const logoMarkBox = this.$refs.logoMark.getBoundingClientRect();

    const siteHeadingH1El = this.$refs.siteHeadingH1;

    const screenX = window.innerWidth;
    const screenY = window.innerHeight;

    const logoAnimation = () => {
      const charArray = gsap.utils.toArray("span.char");
      logoTl
        .set(".logo", {
          x: screenX / 2 - logoBox.width - logoBox.x,
          y: screenY / 2 - logoBox.height - logoBox.y,
          scale: 5,
        })
        .set(".logo .logo-wrapper", {
          x: -logoMarkBox.width,
        })
        .set(".logo-word", {
          x: logoMarkBox.width,
        })
        .from(charArray, {
          yPercent: 100,
          duration: 0.5,
          stagger: {
            amount: 0.5,
            ease: "back.out",
          },
        })
        .to(".logo-wrapper", {
          x: 0,
          duration: 0.5,
          ease: "slow(0.7, 0.7, false)",
          delay: 0.2,
        })
        .to(
          ".logo-word",
          {
            x: 0,
            duration: 0.5,
            ease: "slow(0.7, 0.7, false)",
          },
          "<"
        )
        .to(".logo-wrapper", {
          yPercent: -100,
          delay: 1.5,
          ease: "slow(0.7, 0.7, false)",
        })
        .set(".logo", {
          x: 0,
          y: 0,
          scale: 1,
        })
        .to(".logo-wrapper", {
          yPercent: 0,
          ease: "slow(0.7, 0.7, false)",
          delay: 0.2,
        })
        .call(() => {
          this.siteLoaded = true;
        });
    };

    const siteHeadingAnimation = () => {
      headingTl
        .from(".site-heading h1", {
          y: 80,
          duration: 6,
          scale: 7.5,
          ease: "linear",
        })
        .from(
          ".real-site-heading h1",
          {
            y: 110,
            duration: 6,
            ease: "linear",
          },
          "<"
        );
    };

    // Border Position relative to the heading
    const headingYSet = gsap.quickSetter(".heading-border", "y", "px");
    const setHeadingBorder = () => {
      const siteHeadingH1Box = siteHeadingH1El.getBoundingClientRect();
      const boxHeight = siteHeadingH1Box.height;
      const boxTop = siteHeadingH1Box.y;
      headingYSet(boxTop + boxHeight);
    };
    gsap.ticker.add(setHeadingBorder);

    // Main body position relative to heading;
    const mainYSet = gsap.quickSetter(".main-body", "y", "px");
    const mainTop = () => {
      const borderBox = document
        .querySelector(".heading-border")
        .getBoundingClientRect();
      mainYSet(borderBox.y + 20);
    };
    gsap.ticker.add(mainTop);

    logoAnimation();
    siteHeadingAnimation();
  },
};
</script>

<style scoped>
.logo {
  transform-origin: center;
  width: fit-content;
  overflow: hidden;
}

.logo-wrapper {
  display: flex;
  align-items: center;
  gap: 9px;
}
.logo-mark {
  background: black;
}

.logo-word {
  font-size: 14px;
  font-weight: 300;
  text-transform: uppercase;
  margin: 0;
  text-align: center;
}

span.char {
  display: inline-block;
}

header {
  display: flex;
  height: 80px;
  align-items: center;
  padding: 0 60px;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  justify-content: space-between;
  z-index: 10;
  /* background: white; */
}

.till-load {
  display: none;
}

.site-loaded .till-load {
  display: block;
  animation: fadeIn 2s;
}

.main-body {
  background: white;
  position: relative;
}

.site-loaded header::before {
  position: absolute;
  content: " ";
  left: 0;
  right: 0;
  bottom: 0;
  height: 2px;
  background: rgba(0, 0, 0, 0.2);
  animation: fadeIn 2s;
}

.site-heading {
  position: absolute;
  left: 0;
  right: 0;
  text-align: center;
  transform-origin: 50% 0%;
  /* z-index: -3; */
}

.site-heading h1 {
  margin: 0;
  text-transform: uppercase;
  transform-origin: 50% 0%;
  font-size: 2vw;
  position: relative;
}

.header-background {
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  background: white;
  z-index: -1;
}

.heading-border {
  position: absolute;
  left: 0;
  right: 0;
  height: 2px;
  top: -2px;
  transform-origin: 0% 0%;
}

.fake-heading {
  z-index: -3;
}

.real-site-heading {
  position: absolute;
  left: 0;
  right: 0;
  text-align: center;
  overflow: hidden;
}

.real-site-heading h1 {
  font-size: 2vw;
  text-transform: uppercase;
  margin: 0;
}

.projects {
  transform: translateY(var(--projects-top));
}

.project {
  border-bottom: solid 2px rgba(0, 0, 0, 0.2);
  height: 136px;
  position: relative;
  overflow: hidden;
  transition: height 0.2s;
}

.project-content {
  padding: 0 24px;
  height: 100%;
  display: flex;
  align-items: start;
  flex-direction: column;
  justify-content: center;
}

.project-name {
  font-size: 4vw;
  margin: 0;
  text-transform: uppercase;
  font-weight: 400;
  transform: skewX(0);
  transition: all 0.2s;
  position: relative;
  z-index: 10;
  cursor: pointer;
}

.project:hover .project-name {
  transform: skewX(-10deg);
}

.project-bg {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  top: 0;
  transform: translateY(-100%);
  transition: transform 1s;
  z-index: 5;
  overflow: hidden;
}

.project-bg-inner {
  transform: translateY(100%);
  background-image: url(./assets/project-bg.png);
  /* filter: grayscale(1); */
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  top: 0;
  background-position: 50%;
  background-size: 200%;
  transition: transform 1s, background-size 1s;
}

.labels {
  display: flex;
  gap: 10px;
  overflow: hidden;
  position: relative;
  z-index: 10;
}

.labels span {
  width: 50px;
  height: 25px;
  border-radius: 100px;
  border: 2px solid black;
  text-align: center;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0 5px;
  transform: translateY(150%);
  transition: transform 1s;
}

.project:hover {
  height: 200px;
  /* color: white; */
}

.project:hover .project-bg {
  transform: translateY(0%);
}

.project:hover .project-bg-inner {
  transform: translateY(0%);
  background-size: 100%;
}

.project:hover .labels span {
  transform: translateY(0%);
}

main {
  transform-origin: 50% 0%;
  border-top: 2px solid rgba(0, 0, 0, 0.2);
}

@keyframes fadeIn {
  0% {
    opacity: 0;
  }

  100% {
    opacity: 1;
  }
}
</style>