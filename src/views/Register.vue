<template>
  <div class="register flex flex-col mx-auto mt-6">
    <h1 class="text-primary font-bold text-xl">Register</h1>
    <hr class="my-4" />
    <div class="flex gap-4">
      <div class="flex-1 flex flex-col">
        <form
          v-if="registrationStep === 1"
          class="register flex flex-col"
          @submit.prevent="nextRegistrationStepHandler"
        >
          <Input v-model="username" label="username" type="text" />
          <Input v-model="password" label="password" type="password" />
          <Input
            v-model="matchingPassword"
            label="matching password"
            type="password"
          />
          <button
            class="btn-primary p-2 rounded-full self-end px-6"
            type="submit"
          >
            Next
          </button>
        </form>
        <form v-if="registrationStep === 2" @submit.prevent="registerHandler">
          <p class="text-gray-500 mb-4">
            The below information is going to be used to generate statistics for
            polls that you have participated in. Before each poll you will be
            asked if you want to stay anonymous or would you like to share this
            information.
          </p>
          <Input v-model="username" label="username" type="text" />
          <Radio
            label="gender"
            :options="['Male', 'Female', 'Not Specified']"
            name="gender"
            v-model="gender"
            inline
          />
          <DatePicker label="Date of Birth" v-model="dateOfBirth" />
          <div class="flex justify-between">
            <button
              class="text-gray-400 underline"
              @click="previouseRegistrationStepHandler"
            >
              Back
            </button>
            <button
              class="
                btn-primary
                p-2
                rounded-full
                self-end
                px-6
                flex
                items-center
              "
              type="submit"
            >
              Register
              <InlineLoader v-if="isLoading" />
            </button>
          </div>
        </form>
        <router-link
          class="self-end text-sm text-gray-400 mt-3 underline"
          :to="{ name: Views.MAIN.HOME }"
        >
          Already have an account? Sign in
        </router-link>
      </div>
      <img
        class="flex-1 self-start"
        src="../assets/images/registration-image.svg"
        alt="registration image"
      />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import { useRouter } from "vue-router";
import { Views } from "@/router/viewNames";
import { RegisterService } from "@/service";
import InlineLoader from "@/components/Spinners/InlineLoader.vue";

export default defineComponent({
  name: "Register",
  components: {
    InlineLoader,
  },
  setup() {
    const registrationStep = ref(1);
    const username = ref("");
    const password = ref("");
    const matchingPassword = ref("");
    const gender = ref("");
    const dateOfBirth = ref("");

    const router = useRouter();

    const { isLoading, call } = RegisterService();

    const nextRegistrationStepHandler = () => {
      registrationStep.value += 1;
    };

    const previouseRegistrationStepHandler = () => {
      registrationStep.value -= 1;
    };

    const registerHandler = async () => {
      const res = await call({
        username: username.value,
        password: password.value,
        matchingPassword: matchingPassword.value,
      });
      router.replace({ name: Views.MAIN.LOGIN });
    };

    return {
      username,
      password,
      matchingPassword,
      registrationStep,
      gender,
      dateOfBirth,
      Views,
      isLoading,
      call,
      registerHandler,
      nextRegistrationStepHandler,
      previouseRegistrationStepHandler,
    };
  },
});
</script>

<style>
.register {
  max-width: 50rem;
}
</style>
