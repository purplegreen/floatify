<template>
  <div id="container">
    <span class="first lix">D</span>
    <span class="second lix">E</span>
    <span class="third lix">hlix</span>
    <span class="fourth lix">S</span>
    <span class="deh lix">I</span>
    <span class="deh lix">G</span>
    <span class="deh lix">N</span>
  </div>
</template>

<script>
import gsap from "gsap";
let masterTL = gsap.timeline();

export default {
  name: "NameAnim",
  methods: {
    play() {
      masterTL.add(this.pawsTL());
      masterTL.play();
    },
    pawsTL() {
      let tl = gsap.timeline();
      tl.to(".first", {
        opacity: 1,
        scale: 1,
        duration: 0.5,
        ease: "bounce.out"
      });
      tl.to(
        ".second",
        { opacity: 1, scale: 1, duration: 0.5, ease: "bounce.out" },
        "<.3"
      );
      tl.to(
        ".third",
        { opacity: 1, scale: 1, duration: 0.5, ease: "bounce.out" },
        "<.3"
      );
      tl.to(
        ".fourth",
        { opacity: 1, scale: 1, duration: 0.5, ease: "bounce.out" },
        "<.3"
      );
      return tl;
    }
  }
};
</script>

<style scoped>
#container {
}

.lix {
  font-size: 1.7em;
  background: rgb(139, 134, 221);
  background: linear-gradient(
    90deg,
    rgba(139, 134, 221, 1) 0%,
    rgba(113, 162, 126, 0.7717507102272727) 100%
  );
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.paws {
  transform: scale(0);
  width: 2.5em;
  height: 2.5em;
  margin-top: 50px;
  margin-right: 0.8em;
  opacity: 0;
}

button {
  margin-top: 5em;
}
</style>
