<template>
  <div id="tag-list" :class="`w-48 flex flex-col bg-card-light dark:bg-card-dark mt-4 shadow-lg dark:shadow-shadow-dark hover:shadow-none hover:rounded motion-safe:animate-fade-in-fast transition ${classes}`">
    <nav-item
      v-for="(tag, i) in tags"
      :key="tag.title"
      :ref="tag.slug"
      :href="`/tags/${tagGroup}/${tag.slug}`"
      class="text-center mx-4 transition"
      :style="`animation: fade-in ${((i + 1) * 0.25)}s ease-in-out;`"
      :active="currentPage === tag.title"
    >
      {{ tag.title }}
    </nav-item>
    <p v-if="isErrorLoadingTags">Error</p>
  </div>
</template>

<script>
export default {
  name: 'tag-list',
  props: {
    tagGroup: {
      type: String,
      default: ''
    },
    classes: {
      type: String,
      default: ''
    },
    currentPage: {
      type: String,
      default: ''
    }
  },
  data: () => ({
    tags: [],
    isErrorLoadingTags: false,
  }),
  async fetch() {
    const content = await this.$content(`tags/${this.tagGroup}/${this.tagGroup}`)
    .fetch()
    .catch(() => {
      this.isErrorLoadingTags = true;
    });
    
    this.tags = content?.tags;
  },
};
</script>
