<template>
  <router-link
    :to="{ name: Views.VOTING_PANNEL.CONTAINER, params: { pollId: id } }"
    class="
      bg-white
      text-gray-500
      shadow-lg
      p-3
      rounded-md
      flex
      justify-between
      items-center
    "
  >
    <span>{{ title }}</span>
    <fa v-if="isLive" icon="broadcast-tower" />
    <fa v-else icon="check" />
  </router-link>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { Views } from "@/router/viewNames";

export default defineComponent({
  name: "PollCard",
  props: {
    id: {
      type: String,
      requied: true,
    },
    title: {
      type: String,
      required: true,
    },
    isLive: {
      type: Boolean,
      default: false,
    },
  },
  setup() {
    return {
      Views,
    };
  },
});
</script>
