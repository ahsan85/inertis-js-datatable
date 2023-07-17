<script >
import { reactive, onMounted } from "vue";

import { Head, usePage, Link } from "@inertiajs/inertia-vue3";
import { Inertia } from "@inertiajs/inertia";

export default {
  components: {
    Head,
    Link,
  },

  props: ["links"],

  setup(props, ctx) {
    const links = props.links;

    const pagination = reactive({
      per_page: 10,
    });
    onMounted(() => {
      const params = new URLSearchParams(window.location.search);
      params.sort();

      if (params.has("per_page")) {
        pagination.per_page = params.get("per_page");
      }
    });
    function onPerPageChange(event) {
      ctx.emit("recordsPerPage", event.target.value);
    }

    return { onPerPageChange, pagination };
  },
};
</script>
<style scoped>
.text-color {
  color: white !important;
}
</style>


<template>
  <div class="flex items-center">
    <div class="flex items-center my-4 w-1/5">
      <span class="">Show</span>
      <select
        v-model="pagination.per_page"
        @change="onPerPageChange($event)"
        class="block w-20 h-full px-4 py-2 pr-8 mx-1 leading-tight text-gray-700 bg-white border border-gray-400 rounded appearance-none focus:outline-none focus:bg-white focus:border-gray-500"
      >
        <option value="5">5</option>
        <option value="10">10</option>
        <option value="15">15</option>
        <option value="20">20</option>
      </select>
      <span class="">rows</span>
    </div>
    <div v-if="links.length > 3" class="flex justify-end w-full py-4 space-x-2">
      <div v-for="(link, k) in links" :key="k">
        <ul v-if="link.url === null" v-html="link.label" />

        <Link
          v-else
          class="px-4"
          :class="{
            ' py-2 items-center   border border-transparent rounded-md font-semibold text-xs text-white uppercase tracking-widest  focus:outline-none  transition ease-in-out duration-150 focus:border-indigo-900 focus:shadow-outline-indigo hover:bg-indigo-800 active:bg-indigo-900 bg-indigo-900':
              link.active,
          }"
          :href="link.url"
          v-html="link.label"
        />
      </div>
    </div>
  </div>
</template>