<script lang="ts" setup>
import { ref, computed } from "vue";
import EmojiField from "@/components/EmojiField.vue";
import ArrowCircleRight from "@/assets/icons/arrow-circle-right.svg";
import type Emoji from "@/types/Emoji";

const text = ref("");
const emoji = ref<Emoji | null>(null);
const charCount = computed(() => text.value.length);
const maxChars = 280;

defineEmits<{
  (evt: "@create", entry: { text: string; emoji: Emoji | null }): void;
}>();

const handleTextInput = (evt: Event) => {
  const textarea = evt.target as HTMLTextAreaElement;
  if (textarea.value.length <= maxChars) {
    text.value = textarea.value;
  } else {
    textarea.value = textarea.value.substring(0, maxChars);
    text.value = textarea.value;
  }
};
</script>

<template>
  <form class="entry-form" @submit.prevent="$emit('@create', { text, emoji })">
    <textarea
      :value="text"
      @keyup="handleTextInput"
      placeholder="New Journal Entry for vitaly_stk"
    ></textarea>
    <EmojiField v-model="emoji" />
    <div class="entry-form-footer">
      <span>{{ charCount }} / {{ maxChars }}</span>
      <button>Remember <ArrowCircleRight width="20" /></button>
    </div>
  </form>
</template>
