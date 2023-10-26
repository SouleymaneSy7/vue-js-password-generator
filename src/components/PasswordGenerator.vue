<template>
  <h1 class="primary-title">
    Password
    <span class="fancy-heading"
      >Generator
      <img
        class="arrow"
        src="../assets/hand-drawn-arrow.svg"
        alt="Arrow Draw by hand"
    /></span>
  </h1>

  <main class="main-password">
    <section class="password__generated">
      <h2 class="password__generated--text">{{ generatedPassword }}</h2>
      <button
        class="password__generated-copy--btn"
        type="button"
        @click="copyPassword"
        aria-label="Copy Button"
        title="Copy the generated password in the Clipboard"
      >
        <i class="fa-solid fa-copy fa-flip-horizontal"></i>
      </button>
    </section>

    <section class="password__options">
      <!-- Character Length -->
      <div class="character-length">
        <div class="character-length__header">
          <label for="character-length" class="character-length__title"
            >Character Length</label
          >
          <p class="character-length--number">#{{ characterLength }}</p>
        </div>

        <input
          type="range"
          name="character-length"
          id="character-length"
          class="character-length--range"
          min="0"
          max="20"
          v-model="characterLength"
          @input="rangeProgress($event)"
          title="Change the length of the generated password."
        />
      </div>

      <!-- Options 1 -->
      <div class="options">
        <label for="checkbox-1">
          <input
            type="checkbox"
            name="checkbox-1"
            id="checkbox-1"
            v-model="includeUpperCase"
            title="Include Uppercase Characters in the generated password"
          />
          <p class="option__text">Include Uppercase Letters</p>
        </label>
      </div>

      <!-- Options 2 -->

      <div class="options">
        <label for="checkbox-2">
          <input
            type="checkbox"
            name="checkbox-2"
            id="checkbox-2"
            v-model="includeNumber"
            title="Include Number in the generated password."
          />
          <p class="option__text">Include Numbers</p>
        </label>
      </div>

      <!-- Options 3 -->

      <div class="options">
        <label for="checkbox-3">
          <input
            type="checkbox"
            name="checkbox-3"
            id="checkbox-3"
            v-model="includeSymbol"
            title="Include Symbols in the generated password."
          />
          <p class="option__text">Include Symbols</p>
        </label>
      </div>
    </section>

    <button
      type="button"
      title="Click to generate random password"
      class="password--btn"
      @click="createPassword()"
    >
      Generate Password
    </button>
  </main>
  <BaseNotification
    :notification-errors-msg="notificationErrorsMsg"
    :notification-msg="notificationMsg"
    :show-notification="showNotification"
    :errors-notifications="errorsNotifications"
  />
</template>

<script setup>
import { ref } from "vue";
import BaseNotification from "./BaseNotification.vue";

// Variables
const generatedPassword = ref("Generate Password");
const characterLength = ref(10);
const showNotification = ref(false);
const errorsNotifications = ref(false);
const notificationMsg = ref("Password Copied In The Clipboard");
const notificationErrorsMsg = ref("You Have To Generate Password Before");
const includeUpperCase = ref(false);
const includeNumber = ref(false);
const includeSymbol = ref(false);

// Range Input Progress Bar Functions
const rangeProgress = (event) => {
  const element = event.target;
  const sliderValue = element.value;

  const progress = (sliderValue / element.max) * 100;

  element.style.background = `linear-gradient(to right, var(--range-thumb-clr) ${progress}%, var(--bg-clr) ${progress}%)`;
};

// Generate Password
const createPassword = () => {
  if (characterLength.value == 0) {
    showNotification.value = false;
    errorsNotifications.value = false;
    return;
  }

  let randomPassword = "";
  let lowercaseCharacters = "abcdefghijklmnopqrstuvwxyz";
  let uppercaseCharacters = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
  let numbers = "0123456789";
  let symbols = "!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~";

  includeUpperCase.value === true
    ? (lowercaseCharacters += uppercaseCharacters)
    : "";
  includeNumber.value === true ? (lowercaseCharacters += numbers) : "";
  includeSymbol.value === true ? (lowercaseCharacters += symbols) : "";

  for (let i = 0; i < characterLength.value; i++) {
    let charactersCalc = Math.floor(
      Math.random() * lowercaseCharacters.length + 1
    );

    randomPassword += lowercaseCharacters.charAt(charactersCalc);
  }

  generatedPassword.value = randomPassword;
};

// Copying Password in the Clipboard
const copyPassword = async () => {
  try {
    if (generatedPassword.value.includes("Generate Password")) {
      errorsNotifications.value = true;

      setTimeout(() => {
        errorsNotifications.value = false;
      }, 5000);

      return;
    } else {
      await navigator.clipboard.writeText(generatedPassword.value);
      showNotification.value = true;

      setTimeout(() => {
        showNotification.value = false;
      }, 5000);
    }
  } catch (error) {
    console.error("Failed to Copy the Text", error);
    showNotification.value = false;
    errorsNotifications.value = false;
  }
};
</script>
