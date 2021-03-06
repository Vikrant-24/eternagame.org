<template>
  <EternaPage :title="$t('activity-feed:title')">
    <div v-if="news.length">
      <MessageCompose @submit-message="sentMessage" />
      <Gallery :sm="12" :md="12" style="margin-top:25px">
        <ActivityCard v-for="article in news" :key="article.nid" :article="article" />
      </Gallery>
      <Pagination :key="news.length" />
    </div>
    <div v-else>
      <Preloader />
    </div>
    <template #sidebar="{ isInSidebar }">
      <SearchPanel v-if="isInSidebar" :isInSidebar="isInSidebar" />
      <DropdownSidebarPanel
        :options="options"
        paramName="filter"
        replace
        :isInSidebar="isInSidebar"
      />
    </template>
    <template #mobileSearchbar>
      <SearchPanel :isInSidebar="false" />
    </template>
  </EternaPage>
</template>

<script lang="ts">
  import { Component, Prop, Vue, Mixins, Watch } from 'vue-property-decorator';
  import { RouteCallback, Route } from 'vue-router';
  import axios, { AxiosInstance } from 'axios';
  import SearchPanel from '@/components/Sidebar/SearchPanel.vue';
  import EternaPage from '@/components/PageLayout/EternaPage.vue';
  import FiltersPanel, { Filter } from '@/components/Sidebar/FiltersPanel.vue';
  import DropdownSidebarPanel, { Option } from '@/components/Sidebar/DropdownSidebarPanel.vue';
  import PageDataMixin from '@/mixins/PageData';
  import TagsPanel from '@/components/Sidebar/TagsPanel.vue';
  import Pagination from '@/components/PageLayout/Pagination.vue';
  import Preloader from '@/components/PageLayout/Preloader.vue';
  import Utils from '@/utils/utils';
  import ActivityCard from './components/ActivityCard.vue';
  import MessageCompose from './components/MessageCompose.vue';

  const INITIAL_NUMBER = 18;

  const ROUTE = '/get/?type=newsfeed&combined=true';

  @Component({
    components: {
      EternaPage,
      FiltersPanel,
      DropdownSidebarPanel,
      TagsPanel,
      ActivityCard,
      Pagination,
      Preloader,
      MessageCompose,
      SearchPanel,
    },
  })
  export default class ActivityFeed extends Vue {
    news = [];

    async fetchData() {
      const { filter, search, size } = this.$route.query;
      const res = (
        await axios.get(ROUTE, {
          params: {
            size: size || INITIAL_NUMBER,
            search,
            filter: filter || 'all',
          },
        })
      ).data.data;
      this.news = res.entries;
    }

    mounted() {
      this.fetchData();
    }

    @Watch('$route', { immediate: true, deep: true })
    onUrlChange() {
      this.fetchData();
    }

    sentMessage() {
      this.fetchData();
    }

    private options: Option[] = [
      { value: 'all', text: 'side-panel-options:all-activity' },
      { value: 'groups', text: 'side-panel-options:my-groups' },
      { value: 'notifications', text: 'side-panel-options:my-messages' },
    ];
  }
</script>
