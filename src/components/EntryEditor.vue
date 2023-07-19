<script lang="ts" setup>
import { ref, computed, onMounted } from "vue";
import EmojiField from "@/components/EmojiField.vue";
import ArrowCircleRight from "@/assets/icons/arrow-circle-right.svg";
import type Emoji from "@/types/Emoji";
import type Entry from "@/types/Entry";

const body = ref("");
const emoji = ref<Emoji | null>(null);
const charCount = computed(() => body.value.length);
const maxChars = 280;

const textarea = ref<HTMLTextAreaElement | null>(null);

onMounted(() => textarea.value?.focus());

const emit = defineEmits<{
  (evt: "@create", entry: Entry): void;
}>();

const handleTextInput = (evt: Event) => {
  const textarea = evt.target as HTMLTextAreaElement;
  if (textarea.value.length <= maxChars) {
    body.value = textarea.value;
  } else {
    textarea.value = textarea.value.substring(0, maxChars);
    body.value = textarea.value;
  }
};

const handleSubmit = () => {
  emit("@create", {
    body: body.value,
    emoji: emoji.value,
    createdAt: new Date(),
    userId: 1,
    id: Math.random(),
  });
  body.value = "";
  emoji.value = null;
};
</script>

<template>
  <form class="entry-form" @submit.prevent="handleSubmit">
    <textarea
      :value="body"
      ref="textarea"
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
