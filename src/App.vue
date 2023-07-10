<script  lang="ts">

import { getContent, RenderContent, isPreviewing } from '@builder.io/sdk-vue/vue3'

import { RouterLink, RouterView } from 'vue-router'

import HomeHero from './components/HomeHero.vue'
import HomeHeroOriginal from './components/HomeHeroOriginal.vue'


// Register your Builder components
const REGISTERED_COMPONENTS = [
  {
    component: HomeHero,
    name: 'Home Hero',
    canHaveChildren: true,
    inputs: [
      {
        name: 'title',
        type: 'string',
        defaultValue: 'World',
      },
      {
        name: 'subtitle',
        type: 'string',
        defaultValue: 'Subtitle description',
      },
    ],
  },
    {
    component: HomeHeroOriginal,
    name: 'Home Hero Oroginal',
    canHaveChildren: true,
    inputs: [
      {
        name: 'title',
        type: 'string',
        defaultValue: 'Hello Original',
      },
      {
        name: 'subtitle',
        type: 'string',
        defaultValue: 'Subtitle description for Original',
      },
    ],
  },

];
// TODO: enter your public API key
const BUILDER_PUBLIC_API_KEY = '717f6f07a937428fb26823965e8bc41c'; // ggignore
export default {
  name: 'DynamicallyRenderBuilderPage',
  components: {
    'builder-render-content': RenderContent,
  },
  data: () => ({
    canShowContent: false,
    content: null,
    apiKey: BUILDER_PUBLIC_API_KEY,
  }),
  methods: {
    getRegisteredComponents() {
      return REGISTERED_COMPONENTS;
    },
  },
  mounted() {
    getContent({
      model: 'page',
      apiKey: BUILDER_PUBLIC_API_KEY,
      userAttributes: {
        urlPath: window.location.pathname,
      },
    }).then(res => {
      this.content = res;
      this.canShowContent = this.content || isPreviewing();
    });
  },
}

</script>

<template>
  <RouterView />
    <div>
    <div v-if="canShowContent">
      <div>
        page title:
        {{ (content && content.data && content.data.title) || 'Unpublished' }}
      </div>
      <builder-render-content
        model="page"
        :content="content"
        :api-key="apiKey"
        :customComponents="getRegisteredComponents()"
      />
    </div>
    <div v-else>Content not Found</div>
  </div>
</template>

<style scoped>
header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
