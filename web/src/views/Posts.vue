<template>
  <main>
    <div class="above">
      <div class="a">
        <span class="magnified-element">|</span>
      </div>
      <div class="b">
        <span class="magnified-element">|</span>
      </div>
      <div class="c">
        <span class="magnified-element">\</span>
      </div>
      <div class="d">
        <span class="magnified-element">x</span>
      </div>
      <div class="e">
        <span class="magnified-element">\</span>
      </div>
      <div class="f">
        <span class="magnified-element">\</span>
      </div>
      <div class="g">
        <span class="magnified-element">/</span>
      </div>
      <div class="h">
        <span class="magnified-element">\</span>
      </div>
      <div class="i">
        <span class="magnified-element">|</span>
      </div>
      <div class="l">
        <span class="magnified-element">\</span>
      </div>
    </div>

    <div class="list" v-if="loading == false">
      <PostCard
        ref="post"
        v-for="post in posts"
        class="cards-anim"
        :key="post._id"
        v-bind:post="post"
      />
    </div>
  </main>
</template>

<script>
import PostCard from "@/components/PostCard.vue";
import sanity from "../sanity";
import gsap from "gsap";
import ScrollTrigger from "gsap/ScrollTrigger";

gsap.registerPlugin(ScrollTrigger);
const query = `*[_type == "post" ] | order(releaseDate desc)
{
  _id,
  title,
   posterImage,
  author,
  reference,
  createdAt,
  releaseDate,
  "categories": categories[]->{
    _id,
    description,
    title
  },
  body,
  favoriteColor
}[0...29]`;

export default {
  name: "Posts",
  components: {
    PostCard
  },
  data() {
    return {
      loading: true,
      posts: []
    };
  },
  created() {},
  watch: {
    $route: "fetchData"
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    fetchData() {
      this.error = this.post = null;
      this.loading = true;
      sanity
        .fetch(query)
        .then(
          posts => {
            this.posts = posts;
            this.loading = false;
          },
          error => {
            this.error = error;
          }
        )
        .then(() => {
          this.startAnimation();
        });
    },
    startAnimation: function() {
      ScrollTrigger.defaults({
        toggleActions: "restart pause resume none"
        // markers: true
      });

      gsap.to(".a, .d", {
        scrollTrigger: {
          trigger: ".a, .l, .f, a",
          endTrigger: ".b, .d, .l, .c",
          start: "top center",
          end: "+=100",
          scrub: 7,
          toggleActions: "restart pause reverse pause"
        },
        x: 100,
        rotation: 190,
        duration: 6
      });

      gsap.to(".f, .h, .i", {
        scrollTrigger: {
          trigger: ".c, .b, .i",
          endTrigger: ".c, .h, .i",
          start: "top center",
          end: "top 60px",
          scrub: 3,
          toggleActions: "restart pause reverse pause"
        },
        x: -90,
        rotation: 90,
        duration: 3
      });

      gsap.to(".b, .e", {
        scrollTrigger: {
          trigger: ".b, .a",
          endTrigger: ".a, .c",
          start: "top center",
          end: "bottom 100px",
          scrub: 6,
          toggleActions: "restart pause reverse pause"
        },
        x: 200,
        rotation: 360,
        duration: 9,
        scale: gsap.utils.distribute({
          base: 2.7,
          amount: 0.5,
          from: "center"
        })
      });

      gsap.to(".g", {
        scrollTrigger: {
          trigger: ".c",
          endTrigger: ".e",
          start: "top center",
          end: "bottom 100px",
          scrub: 2,
          toggleActions: "restart pause reverse pause"
        },
        x: 190,
        rotation: 360,
        duration: 3,
        scale: gsap.utils.distribute({
          base: 2.7,
          amount: 1.2,
          from: "center"
        })
      });

      gsap.to(".c", {
        scrollTrigger: {
          trigger: ".c",
          endTrigger: ".e",
          start: "top center",
          end: "bottom 100px",
          scrub: 3,
          toggleActions: "restart pause reverse pause"
        },
        x: 160,
        rotation: 90,
        duration: 9,
        scale: gsap.utils.distribute({
          base: 3,
          amount: 0.5,
          from: "center"
        })
      });

      gsap.to(".l", {
        scrollTrigger: {
          trigger: ".b",
          endTrigger: ".a",
          start: "top center",
          end: "bottom 100px",
          scrub: 2,
          toggleActions: "restart pause reverse pause"
        },
        x: -260,
        rotation: 180,
        duration: 6,
        scale: gsap.utils.distribute({
          base: 2,
          amount: 1.6,
          from: "center"
        })
      });

      gsap.utils.toArray(".cards-anim").forEach((el, i) => {
        gsap.to(el, {
          scrollTrigger: {
            trigger: el,
            start: "top center",
            end: "bottom 100px",
            scrub: i * 0.1
            // markers: true
          },
          y: 48,
          duration: 9
        });
        console.log(i, "hello there");
      });
    }
  }
};
</script>

<style scoped>
main {
  position: relative;
  padding-top: 80vh;
  display: flex;
  flex-direction: column;
  font-size: 1rem;
  overflow: hidden;
}

.a,
.b,
.c,
.d,
.e,
.f,
.g,
.h,
.i,
.l {
  background: rgb(230, 255, 226);
  background: radial-gradient(
    circle,
    rgba(230, 255, 226, 0.5984552556818181) 0%,
    rgb(200, 200, 204) 100%
  );
  position: relative;

  min-height: 40px;
  height: auto;
  width: 3.3rem;
  margin: 13px;
  padding: 7px;
  border-radius: 12px;
}

.magnified-element {
  font-size: 3.3rem;
  color: mediumslateblue;
}

.above {
  position: absolute;
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  top: 30px;
  height: 76vh;
  width: 100vw;
}

.list {
  display: flex;
  flex-wrap: wrap;
  flex-direction: row;
  width: 96vw;
  margin: auto;
  justify-content: space-evenly;
  padding-bottom: 100px;
}

@media (max-width: 700px) {
}
</style>
