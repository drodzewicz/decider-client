<template>
  <form @submit.prevent="createBinaryPollHandler">
    <div class="flex gap-2">
      <Input label="Poll title" v-model="title" />
      <Input label="Poll description" v-model="description" class="flex-1" />
      <button class="btn-primary p-2 rounded-full px-6 my-4">Create</button>
    </div>
    <div class="flex gap-2">
      <DatePicker :disabled="!isLimitedByTime">
        <template #label>
          <CheckBox
            v-model="isLimitedByTime"
            class="self-start"
            label="Limited time"
          />
        </template>
      </DatePicker>
      <div class="flex gap-2 items-center">
        <Input
          v-model="numberOfChoices"
          type="number"
          :disabled="!isMultipleChoice"
        >
          <template #label>
            <CheckBox
              v-model="isMultipleChoice"
              class="self-start"
              label="Multiple Choice"
            />
          </template>
        </Input>
        <Slider
          v-model="numberOfChoices"
          :min="1"
          :max="pollOptions.length"
          :disabled="!isMultipleChoice"
        />
      </div>
    </div>
    <PollOptionManager v-model:options="pollOptions" />
  </form>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import PollOptionManager from "@/components/PollOptions/PollOptionManager.vue";
import { useCreatePoll } from "@/composables";
import { Views } from "@/router/viewNames";

export default defineComponent({
  name: Views.CREATE_POLL.SELECT,
  components: {
    PollOptionManager,
  },
  setup() {
    const { title, description, isLimitedByTime, pollOptions } =
      useCreatePoll();

    const isMultipleChoice = ref<boolean>(false);
    const numberOfChoices = ref<number>(1);

    const createBinaryPollHandler = async () => {
      const data = {
        title: title.value,
        description: description.value,
        isLimitedByTime: isLimitedByTime.value,
        options: pollOptions.value,
      };
      console.log("SUBMITING POLL");
      console.log(data);
    };

    return {
      title,
      description,
      isLimitedByTime,
      isMultipleChoice,
      pollOptions,
      numberOfChoices,
      createBinaryPollHandler,
    };
  },
});
</script>