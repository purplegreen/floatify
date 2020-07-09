<template>
  <main>
    <div class="bottomone">
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
        <span class="magnified-element">|</span>
      </div>
      <div class="e">
        <span class="magnified-element">\</span>
      </div>
    </div>

    <div class="topone">DEhlix SIGN</div>
    <ul class="list">
      <PostCard v-for="post in posts" class="panel" :key="post._id" v-bind:post="post" />
    </ul>
  </main>
</template>

<script>
import PostCard from "@/components/PostCard.vue";
import sanity from "../sanity";
import gsap from "gsap";
import ScrollTrigger from "gsap/ScrollTrigger";

gsap.registerPlugin(ScrollTrigger);

// import BlockContent from "sanity-blocks-vue-component";
// let sections = gsap.utils.toArray(".panel");
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
    // BlockContent,
  },
  data() {
    return {
      loading: true,
      posts: []
    };
  },
  created() {
    this.fetchData();
  },
  watch: {
    $route: "fetchData"
  },
  mounted: function() {
    this.startAnimation();
  },
  methods: {
    fetchData() {
      this.error = this.post = null;
      this.loading = true;
      sanity.fetch(query).then(
        posts => {
          this.loading = false;
          this.posts = posts;
          // this.texts = this.post.title;
        },
        error => {
          this.error = error;
        }
      );
    },
    startAnimation: function() {
      ScrollTrigger.defaults({
        toggleActions: "restart pause resume none"
        // markers: true
      });

      gsap.to(".a, .d", {
        scrollTrigger: {
          trigger: ".a",
          endTrigger: ".b",
          start: "top center",
          end: "+=100",
          scrub: true,

          toggleActions: "restart pause reverse pause"
        },
        x: 100,
        rotation: 190,
        duration: 6
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
        duration: 3
      });

      gsap.to(".c", {
        scrollTrigger: {
          trigger: ".c",
          endTrigger: ".c",
          start: "top center",
          end: "bottom 100px",
          scrub: 3,

          toggleActions: "restart pause reverse pause"
        },
        x: 600,
        rotation: 90,
        duration: 6
      });

      gsap.utils.toArray(".panel").forEach((panel, i) => {
        ScrollTrigger.create({
          trigger: panel,
          start: "top top",
          pin: true,
          pinSpacing: false
        });
        console.log(i);
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
.f {
  background: rgb(230, 255, 226);
  background: radial-gradient(
    circle,
    rgba(230, 255, 226, 0.5984552556818181) 0%,
    rgba(192, 148, 233, 1) 100%
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

/* first svg animation */

.bottomone {
  position: absolute;
  top: 60px;
  height: 80vh;
  width: 100vw;
}

.topone {
  position: absolute;
  top: 60px;
  height: 80vh;
  width: 100vw;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  font-size: 11.7em;
  background: rgb(139, 134, 221);
  background: linear-gradient(
    90deg,
    rgba(139, 134, 221, 1) 0%,
    rgba(113, 162, 126, 0.7717507102272727) 100%
  );
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

/* end first animation */

.list {
  display: flex;
  flex-wrap: wrap;
  flex-direction: row;
  width: 100vw;
  justify-content: space-around;
}

@media (max-width: 700px) {
}
</style>
