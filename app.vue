<script lang="ts">
export default {
  setup() {
    const languages: Array<string> = ["ru", "en", "fr"];
    const language = ref<string | null>(null);
    const tree = ref<Array<object> | null>(null);
    onMounted(() => {
      useFetch("/api/read")
        .then((resp) => resp.data)
        .then((data) => {
          tree.value = JSON.parse(data.value ?? "") ?? null;
        });
    });

    return {
      languages,
      language,
      tree,
    };
  },
};
</script>

<template>
  <select
    v-model="language"
    class="border-2 border-black rounded my-5 mx-auto block"
  >
    <option v-for="languageEl in languages" :value="languageEl">
      {{ languageEl }}
    </option>
  </select>
  <div v-if="language" class="m-2">
    <node v-for="node in tree" :nodeValue="node" :language="language" />
  </div>
</template>
