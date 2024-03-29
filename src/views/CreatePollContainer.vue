<template>
  <header
    class="bg-white flex justify-between items-center -mt-3 -mx-3 p-3 shadow-md"
  >
    <h1 class="text-primary font-bold text-xl">
      <fa :icon="headerTitle.icon" class="mr-1" />
      {{ headerTitle.title }}
    </h1>
    <div class="flex gap-2 items-center">
      <span :style="{ maxWidth: '10.5rem' }" class="text-xs text-gray-500">
        Once you click create you wn't be able to edit this poll
      </span>
      <button
        @click="createPollHandler"
        :disabled="isLoading"
        type="button"
        class="btn-primary p-2 rounded-md px-6 shadow-md"
      >
        Create
        <InlineLoader v-if="isLoading" />
        <fa v-else icon="plus-square" />
      </button>
    </div>
  </header>
  <router-view v-slot="{ Component }">
    <component ref="view" :is="Component" />
  </router-view>
</template>

<script lang="ts">
import { defineComponent, ref, computed } from "vue";
import { useRoute } from "vue-router";
import { Views } from "@/router/viewNames";
import InlineLoader from "@/components/Spinners/InlineLoader.vue";

export default defineComponent({
  name: Views.CREATE_POLL.CONTAINER,
  components: {
    InlineLoader,
  },
  setup() {
    const view = ref();
    const route = useRoute();

    const { pollType } = route.meta;

    const headerTitle = computed<{ title: string; icon: string }>(() => {
      switch (pollType) {
        case "BINARY":
          return { title: "Create Binary Poll", icon: "network-wired" };
        case "SELECT":
          return { title: "Create Select Poll", icon: "tasks" };
        case "METER":
          return { title: "Create Meter Poll", icon: "thermometer-half" };
        default:
          return { title: "Create Poll", icon: "folder-plus" };
      }
    });

    const isLoading = computed(() => {
      if (view.value) return view.value.$.ctx["isLoading"];
      return false;
    });

    const createPollHandler = () => {
      if (view.value) view.value.$.ctx["createPollHandler"]();
    };
    return {
      headerTitle,
      view,
      isLoading,
      createPollHandler,
    };
  },
});
</script>
