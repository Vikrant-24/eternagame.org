<template>
  <div>
    <div v-if="picture">
      <img
        class="d-block d-sm-none rounded-circle player-image"
        :src="picture"
        alt="player-image"
      />
      <img
        class="d-none d-sm-block rounded-circle player-image-large"
        :src="picture"
        alt="player-image"
      />
    </div>
    <img
      v-else
      class="d-none d-sm-block rounded-circle player-image-large"
      src="@/assets/front-page/img/icon_user.png"
    />
  </div>
</template>

<script lang="ts">
  import { Component, Vue, Mixins, Prop } from 'vue-property-decorator';
  import { UserData } from '@/types/common-types';
  import { DEFAULT_PLAYER_PICTURE } from '@/utils/constants';

  @Component({
    components: {},
  })
  export default class PlayerHeaderImage extends Vue {
    get user(): UserData {
      return this.$vxm.user.userDetails as UserData;
    }

    get picture() {
      return this.user.picture && `${process.env.VUE_APP_API_BASE_URL}/${this.user.picture}`;
    }
  }
</script>

<style lang="scss" scoped>
  @import '@/styles/global.scss';

  .player-image {
    object-fit: scale-down;
    margin-right: 20px;
    width: 61.58px;
    height: 61.58px;
  }

  .player-image-large {
    width: 115.93px;
    height: 115.93px;
    object-fit: scale-down;
    margin-right: 20px;
  }
</style>
