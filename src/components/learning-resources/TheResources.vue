<template>
  <base-card>
    <base-button
      @click="setSelectedTab('stored-resources')"
      :class="storedResourceButtonMode"
      >Stored Resources</base-button
    >
    <base-button
      @click="setSelectedTab('add-resource')"
      :class="addResourceButtonMode"
      >Add Resource</base-button
    >
  </base-card>
  <keep-alive>
    <component :is="selectedTab"></component>
  </keep-alive>
</template>
<script>
import StoredResources from './StoredResources.vue';
import AddResource from './AddResource.vue';

export default {
  components: {
    StoredResources,
    AddResource,
  },
  data() {
    return {
      selectedTab: 'stored-resources',
      storedResources: [
        {
          id: 'official-guide',
          title: 'Official Guide',
          description: 'The official Vue.js documentation',
          link: 'https://vuejs.org',
        },
        {
          id: 'google',
          title: 'Google',
          description: 'Home of the Internet',
          link: 'https://google.pl',
        },
      ],
    };
  },
  computed: {
    storedResourceButtonMode() {
      return this.selectedTab === 'stored-resources' ? null : 'flat';
    },
    addResourceButtonMode() {
      return this.selectedTab === 'add-resource' ? null : 'flat';
    },
  },
  provide() {
    return {
      storedResources: this.storedResources,
      addResource: this.addResource,
      removeResource: this.removeResource,
    };
  },
  methods: {
    setSelectedTab(tab) {
      this.selectedTab = tab;
    },
    addResource(title, description, url) {
      const userData = {
        id: new Date().toISOString(),
        title: title,
        description: description,
        link: url,
      };
      this.storedResources.unshift(userData);
      this.setSelectedTab('stored-resources');
    },
    removeResource(id) {
      const indexToDelete = this.storedResources.findIndex((resource) => resource.id === id);
      this.storedResources.splice(indexToDelete, 1);
    },
  },
};
</script>
<style scoped></style>
