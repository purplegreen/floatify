<template>
  <main>
    <div class="a">
      <span class="magnified-element">|</span>
    </div>
    <div class="b">
      <span class="magnified-element">|</span>
    </div>
    <div class="c">
      <span class="magnified-element">\</span>
    </div>

    <div class="topone position">
      <div class="top-anim">
        <div class="box-one">
          <p id="d0">d</p>
          <p id="i">i</p>
          <p id="g">g</p>
          <p id="i">i</p>
          <p id="t">t</p>
          <p id="a">a</p>
          <p id="l">l</p>
        </div>
        <div class="box-two">
          <p id="d">d</p>
          <p id="e">e</p>
          <p id="s">s</p>
          <p id="i">i</p>
          <p id="g">g</p>
          <p id="n">n</p>
        </div>
      </div>
    </div>
    <ul class="list">
      <PostCard v-for="post in posts" class="list__item" :key="post._id" v-bind:post="post" />
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
      gsap.to(".c", {
        scrollTrigger: {
          trigger: ".c",
          markers: true,
          toggleActions: "restart pause reverse reset"
        },
        x: 400,
        rotation: 360,
        duration: 3
      });
    }
  }
};
</script>

<style scoped>
main {
  display: flex;
  flex-direction: column;
  font-size: 1rem;
}

.wrap-top {
  padding-top: 17vh;
  padding-bottom: 17vh;
  position: relative;
  width: 90vw;
  height: 18%;
  margin: auto;
  text-align: center;
}

.wrap-woo {
  width: 90vw;
  margin: auto;
  justify-content: center;
  align-items: center;
  display: flex;
}

.a,
.b,
.c {
  background-color: lightgreen;
  width: 60px;
  height: 60px;
  margin: 13px;
  padding: 7px;
}

/* first svg animation */

.topone {
  position: relative;
  height: 400px;
}

.magnified-element {
  font-size: 2.3rem;
  color: black;
}

.top-anim {
  position: absolute;
  top: 80%;
  left: 50%;
  transform: translate(-50%, -99%);
  width: 70vw;
  height: 300px;
  margin: auto;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: flex-start;
  font-size: 6.7em;
  background: -webkit-linear-gradient(mediumslateblue, #8ca369);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.box-one,
.box-two {
  display: flex;
  padding: 0 20px;
}

#d:hover,
#d0:hover,
#e:hover,
#s:hover,
#i:hover,
#g:hover,
#n:hover,
#t:hover,
#a:hover,
#l:hover {
  animation-name: wiggleWiggle;
  animation-duration: 0.7s;
  animation-iteration-count: infinite;
}

#d:hover {
  animation-delay: 0s;
  -webkit-text-fill-color: #dbe2f1;
  border: 1px solid slategrey;
  transform: translateY(2px);
}

#e:hover {
  animation-delay: 0.1s;
  -webkit-text-fill-color: #cee6f0;
  transform: translateY(3px);
}

#s:hover {
  animation-delay: 0.2s;
  border: 1px solid rgb(14, 252, 65);
  -webkit-text-fill-color: #cbe8f0;
  transform: translateY(4px);
}

#i:hover {
  animation-delay: 0.3s;
  -webkit-text-fill-color: #bdeef0;
  transform: translateY(5px);
}

#g:hover {
  animation-delay: 0.4s;
  -webkit-text-fill-color: #b5f1ef;
  transform: translateY(6px);
}

#n:hover {
  animation-delay: 0.5s;
  -webkit-text-fill-color: #73eef9;
  transform: translateY(1px);
}

#d0:hover {
  animation-delay: 0.5s;
  -webkit-text-fill-color: #b0f2dc;
  transform: translateY(-1px);
}

#t:hover {
  animation-delay: 0.3s;
  -webkit-text-fill-color: #aef4c6;
  transform: translateY(-3px);
}

#a:hover {
  animation-delay: 0.4s;
  -webkit-text-fill-color: #abf6b2;
  transform: translateY(-6px);
}

#l:hover {
  animation-delay: 0.5s;
  -webkit-text-fill-color: #a9f79e;
  transform: translateY(-7px);
}

@keyframes wiggleWiggle {
  20%,
  100% {
    transform: translate(0, 3px);
    display: none;
    opacity: 0.2;
  }

  0% {
    transform: translate(0, 0px);
    display: block;
    opacity: 0.5;
  }
  10% {
    transform: translate(0, 2px);
    display: block;
    opacity: 1;
  }
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
  .topone {
    height: 250px;
  }

  .kinesis-container {
    width: 90vw;
    height: 250px;
    margin: auto;
  }

  .top-anim {
    position: absolute;
    top: 80%;
    left: 50%;
    width: 90vw;
    height: auto;
    margin: auto;
    font-size: 2.7em;
    line-height: 0.1em;
  }
}
</style>
