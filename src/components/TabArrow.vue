<template>
  <div>
    <h3 id="tab-list">More Great Deals From Le Creuset</h3>
    <div role="tablist" aria-labelledby="tab-list">
      <button 
        v-for="(tab, index) in tabs" 
        :key="index"
        role="tab"
        :id="`tab-${index + 1}`"
        :aria-selected="index === activeTabIndex ? 'true' : 'false'"
        :aria-controls="`tab-panel-${index + 1}`"
        :tabindex="index === activeTabIndex ? '0' : '-1'"
        class="tab"
        :class="{ active: index === activeTabIndex }"
        @click="changeTab(index)"
        @keydown="handleTabKey(index, $event)"
        ref="tabButton"
      >
        {{ tab.title }}
      </button>
    </div>
    <div 
      v-for="(tab, index) in tabs" 
      :key="index"
      :id="`tab-panel-${index + 1}`"
      class="tab-panel"
      :class="{ active: index === activeTabIndex }"
      :tabindex="index === activeTabIndex ? '0' : '-1'"
      role="tabpanel"
      :aria-labelledby="`tab-${index + 1}`"
    >
      <!-- Content for Tab {{ index + 1 }} -->
      <img :src="tab.image" alt="" />
      <p>{{ tab.content }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      activeTabIndex: 0,
      tabs: [
        { title: 'Enamel Cast', content: 'Content of Tab 1', image: '/Images/image1.png' },
        { title: 'Classic Demi', content: 'Content of Tab 2', image: '/Images/image2.png' },
        { title: 'Open Rect', content: 'Content of Tab 3', image: '/Images/image3.png' }
      ]
    };
  },
  methods: {
    changeTab(index) {
      this.activeTabIndex = index;
      this.$nextTick(() => {
        if (this.$refs.tabButton[index]) {
          this.$refs.tabButton[index].focus();
        }
      });
    },
    handleTabKey(index, event) {
      if (event.key === 'ArrowRight' || event.key === 'ArrowDown') {
        event.preventDefault();
        const nextIndex = index === this.tabs.length - 1 ? 0 : index + 1;
        this.changeTab(nextIndex);
      } else if (event.key === 'ArrowLeft' || event.key === 'ArrowUp') {
        event.preventDefault();
        const prevIndex = index === 0 ? this.tabs.length - 1 : index - 1;
        this.changeTab(prevIndex);
      }
    }
  }
};
</script>

<style>
  button {
    padding: 10px;
    margin: 2px;
  }
  .tab-panel {
    display: none;
  }
  .tab-panel.active {
    display: block;
  }
  .tab.active {   
    background-color: white;
  }
</style>
