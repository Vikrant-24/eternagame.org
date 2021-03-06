<template>
  <div class="container" ref="container">
    <div class="row justify-content-between">
      <div class="col p-0" style="text-align:right">
        <div class="b" style="opacity: 0.5; ">{{ timeCreated }}</div>
      </div>
    </div>

    <div :style="{ marginLeft: !first && '50px' }">
      <div class="row d-flex" :style="{ marginTop: '10px' }">
        <SmartLink :link="'/players/' + sender">
          <img
            class="d-none d-sm-block rounded-circle player-image"
            :src="avatar"
            style="margin-right:10px"
          />
        </SmartLink>
        <p v-if="senderName" style="margin-top:5px">
          <template v-if="type == 'message'"> {{ senderName }} > {{ getterName }}: </template>
          <template v-else>
            {{ senderName + ' ' }} {{ $t('activity-feed:commented-on') + ' ' }}
            <a :href="`/${content.node.node_type}s/${content.node.id}`">
              {{ content.node.title }}</a
            >
          </template>
        </p>
      </div>
      <div v-dompurify-html="content.body || content" style="word-wrap: break-word;" />
    </div>
  </div>
</template>
<script lang="ts">
  import { Component, Prop, Vue, Watch, Ref } from 'vue-property-decorator';
  import Utils from '@/utils/utils';
  import SmartLink from '@/components/Common/SmartLink.vue';
  import VueScrollTo from 'vue-scrollto';
  import { NewsItem } from '@/types/common-types';

  Vue.use(VueScrollTo);

  @Component({
    components: { SmartLink },
  })
  export default class MessageItem extends Vue {
    @Prop() private created!: string;

    @Prop({ default: '' }) private content!: string | object;

    @Prop({ default: false }) private first!: boolean;

    @Prop() private sender!: string;

    @Prop() private article!: NewsItem;

    @Prop() private type!: string;

    @Ref('container') readonly scrollTarget!: HTMLElement;

    maybeScroll() {
      if (window.location.hash && window.location.hash.substr(1) === String(this.created)) {
        VueScrollTo.scrollTo(this.scrollTarget, { offset: -100 });
      }
    }

    @Watch('$route', { immediate: true, deep: true })
    onUrlChange() {
      this.maybeScroll();
    }

    mounted() {
      this.maybeScroll();
    }

    private senderName =
      this.sender === this.article.target_uid
        ? this.article.target_name
        : this.article.target2_name;

    private getterName =
      this.sender === this.article.target_uid
        ? this.article.target2_name
        : this.article.target_name;

    get avatar() {
      return Utils.getAvatar(
        this.sender === this.article.target_uid
          ? this.article.target_picture
          : this.article.target2_picture,
      );
    }

    get timeCreated() {
      return new Date(Number(this.created) * 1000).toLocaleString(undefined, {
        year: 'numeric',
        month: 'short',
        day: 'numeric',
      });
    }
  }
</script>

<style lang="scss" scoped>
  @import '@/styles/global.scss';

  .player-image {
    width: 30px;
    height: 30px;
  }

  .btn {
    display: inline-block;
    width: 48%;
    margin-bottom: 0px;
  }

  ::v-deep .card-body {
    padding: 11.25px !important;
  }

  .card-title {
    font-size: 1.3rem;
    font-weight: bold;
    margin: 0.4rem 0;
    color: $white;
  }

  .b {
    font-weight: bold;
  }

  .icon-text {
    margin-left: 7px;
    position: relative;
    top: 7px;
    font-weight: bold;
  }

  .card {
    color: $white;

    padding: 1rem 2rem;
    margin-bottom: 1.5rem;
    max-height: 600px;
    // cursor: pointer;
    transition: background-color 0.5s ease;
  }

  .card:hover {
    background-color: #21508c;
  }

  a:hover {
    text-decoration: none;
  }

  .text ::v-deep img {
    max-width: 100% !important;
  }
</style>
