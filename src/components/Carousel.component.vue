<template>
  <section id="carousel">
    <div class="carousel_content">
      <span class="page-title">Início</span>
      <h1 class="title from-left">{{ list[current].title }}</h1>

      <span class="carousel_content-price from-left">
        {{ list[current].price }}
      </span>

      <p class="description from-left">{{ list[current].description }}</p>
      <Button
        id="buy-now"
        classname="from-left"
        text="Garanta já"
        :onclick="seeMore"
      />
    </div>

    <div
      class="carousel_img from-blur"
      :style="{
        backgroundImage: 'url(' + list[current].imgUrl + ')',
      }"
    >
      <div class="carousel_indicator">
        <button
          :class="item.id === current ? 'carousel_indicator-active' : ''"
          @click="setCurrent(item.id)"
          v-for="item in list"
          :key="item.id"
        >
          .
        </button>
      </div>
      <div class="carousel_buttons">
        <button class=".prev" @click="prev()">{{ prevText }}</button>
        <button class=".next" @click="next()">{{ nextText }}</button>
      </div>
    </div>
  </section>
</template>

<script>
import carouselList from "../data/carouselList";
import Button from "../components/Button.component";

export default {
  name: "Carousel",
  components: { Button },
  data() {
    return {
      list: carouselList || [],
      current: 0,
      prevText: "<",
      nextText: ">",
      currentTime: 10,
    };
  },

  created() {
    this.countDown();
  },

  beforeDestroy() {
    clearInterval();
  },

  methods: {
    prev() {
      if (this.current > 0) {
        this.removeAnimation();
        setTimeout(() => {
          this.current -= 1;
          this.animate();
        }, 50);
      }
    },
    next() {
      if (this.current < this.list.length - 1) {
        this.removeAnimation();
        setTimeout(() => {
          this.current += 1;
          this.animate();
        }, 50);
      }
    },
    setCurrent(value) {
      this.removeAnimation();
      setTimeout(() => {
        this.current = value;
        this.animate();
      }, 50);
    },

    countDown() {
      setInterval(() => {
        if (this.current === this.list.length - 1) this.setCurrent(0);
        else this.next();
      }, 10000);
    },

    removeAnimation() {
      const button = document.querySelector("#buy-now");
      const img = document.querySelector(".carousel_img");
      const span = document.querySelector(".carousel_content-price");
      const h1 = document.querySelector(".title");
      const p = document.querySelector("p");

      if (!!img && !!button && !!span && !!h1 && !!p) {
        img.classList.remove("from-blur");
        button.classList.remove("from-left");
        span.classList.remove("from-left");
        h1.classList.remove("from-left");
        p.classList.remove("from-left");
      }
    },

    animate() {
      const button = document.querySelector("#buy-now");
      const img = document.querySelector(".carousel_img");
      const span = document.querySelector(".carousel_content-price");
      const h1 = document.querySelector(".title");
      const p = document.querySelector("p");

      if (!!img && !!button && !!span && !!h1 && !!p) {
        img.classList.add("from-blur");
        button.classList.add("from-left");
        span.classList.add("from-left");
        h1.classList.add("from-left");
        p.classList.add("from-left");
      }
    },

    seeMore() {
      this.$router.push("/search");
    },
  },
};
</script>

<style scoped>
#carousel {
  position: relative;
  width: 100vw;
  height: 100vh;
}

.page-title {
  position: absolute;
  top: 100px;

  font-weight: 300;
  font-size: 1.2rem;
}

.carousel_img {
  background-size: cover;
  background-position: bottom right;

  height: 100vh;
  width: 100vw;
}

.carousel_content {
  position: absolute;
  left: 0;

  width: 40%;
  min-width: 400px;
  height: 100%;
  padding: 100px;

  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: center;

  background-color: rgba(0, 0, 0, 0.8);
  z-index: 2;
}

.carousel_content h1 {
  text-align: initial;
  font-size: 5rem;
  font-weight: bold;
  text-transform: uppercase;
}

.carousel_content-price {
  font-size: 1.6rem;
  font-weight: thin;

  margin-left: 5px;
  margin-bottom: 50px;
}

.carousel_content p {
  width: 100%;
  max-height: 200px;
  overflow-y: auto;

  text-align: start;
  line-height: 1.5rem;
}

.description::-webkit-scrollbar-track {
  -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
}

.description::-webkit-scrollbar {
  width: 5px;
  background-color: #000000;
}

.description::-webkit-scrollbar-thumb {
  background-color: #f5f5f5;
}

.carousel_buttons {
  position: absolute;
  right: 100px;
  bottom: 100px;

  display: flex;
}

.carousel_buttons button {
  display: flex;
  align-items: center;
  justify-content: center;

  font-size: 2rem;

  background-color: rgba(0, 0, 0, 0.5);
  margin: 5px;

  height: 50px;
  width: 50px;

  cursor: pointer;
  transition: 0.4s;
}

.carousel_buttons button:hover {
  background-color: rgba(0, 0, 0, 1);
}

.carousel_indicator {
  position: absolute;
  right: 25px;
  top: 0;

  height: calc(100% - 100px);

  display: flex;
  flex-direction: column;
  justify-content: center;
}

.carousel_indicator button {
  font-size: 2.5rem;
  line-height: 2rem;

  height: 50px;
  width: 50px;
  color: rgba(0, 0, 0, 0.8);

  cursor: pointer;
}

.carousel_indicator-active {
  font-size: 3rem !important;
  color: #fff !important;
}

/* ================== RESPONSIVE ================== */
@media (max-width: 1200px) {
  .carousel_content h1 {
    font-size: 2.5rem;
  }

  .carousel_content {
    padding: 100px 50px;
  }
}

@media (max-width: 850px) {
  #carousel {
    display: flex;
    flex-direction: column-reverse;
  }

  .carousel_img {
    height: 50vh;
    width: 100vw;
  }

  .page-title {
    position: absolute;
    top: 25px;
  }

  .carousel_content {
    position: relative;

    width: 100%;
    height: 50%;

    padding: 50px;

    background-color: rgb(0, 0, 0);
  }

  .carousel_buttons {
    position: absolute;
    right: 50px;
    bottom: 25px;

    display: flex;
  }

  .carousel_indicator {
    display: none;
  }

  .carousel_content p {
    max-height: 100px;
  }
}
</style>