<template>
  <b-carousel-slide
    class="slide"
    v-if="imageURL"
    :img-src="imageURL"
    style=" background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0))"
    ><div class="banner-text">
      <h1 class="banner-title">Puzzle of the Week</h1>
      <h2 class="banner-subtitle d-none d-sm-block">{{ title }}</h2>
      <img class="img d-none d-lg-block" :src="puzzleImageURL" />

      <b-button variant="primary" class="enter-lab" size="lg" :href="`/puzzles/${nid}`">{{
        $t('puzzle-slide:solve-now')
      }}</b-button>
      <router-link class="d-none d-sm-block" to="/puzzles/?search=:POTW">
        <p style="margin-right: 20px;color:white;font-weight:bold;font-size:14px">
          <i class="arrow_right"></i>{{ $t('puzzle-slide:past-potw') }}
        </p>
      </router-link>
      <router-link to="/puzzles/?search=:COVID19" class="link d-none d-sm-block">
        <p style="margin-right: 20px;color:white;font-weight:bold;font-size:14px">
          <i class="arrow_right"></i>{{ $t('puzzle-slide:past-training') }}
        </p>
      </router-link>
    </div></b-carousel-slide
  >
</template>

<script lang="ts">
  import { Component, Vue, Mixins, Prop } from 'vue-property-decorator';
  import { RouteCallback, Route } from 'vue-router';
  import { AxiosInstance } from 'axios';
  import Utils from '@/utils/utils';
  import bgimage from '@/assets/home/hero-blue-bg.png';

  @Component({
    components: {},
  })
  export default class PuzzleSlide extends Vue {
    @Prop({})
    title!: string;

    @Prop({}) nid!: string;

    get puzzleImageURL() {
      return Utils.getPuzzleMiddleThumbnail(this.nid);
    }

    get imageURL() {
      return bgimage;
      // return 'https://assets.wordpress.envato-static.com/uploads/2018/01/image1.png';
      //   return Utils.getPuzzleMiddleThumbnail(this.nid);
    }
  }
</script>

<style lang="scss" scoped>
  @import '@/styles/global.scss';

  .slide {
    max-width: 1200px;
    max-height: 519px;
  }

  .img {
    float: right;
    width: 400;
    height: 400;
  }

  ::v-deep img {
    min-height: 300px;
    object-fit: cover;
  }

  .banner-title {
    font-size: 42px;
    font-weight: bold;
  }

  .enter-lab {
    font-weight: bold;
    font-size: 20px;
    padding: 9px 12px;
    margin-bottom: 10px;
  }

  h1,
  h2,
  h3,
  h1 {
    font-size: 32px;
  }

  h2 {
    font-size: 26px;
  }

  h3 {
    font-size: 18px;
  }

  @include media-breakpoint-up(sm) {
    h1,
    h2,
    h3,
    .banner-text {
      text-align: left;
    }
  }
</style>
