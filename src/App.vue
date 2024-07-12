<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <TextTags :tags="tags" alignment="left" />
      </v-col>
      <v-col cols="12">
        <TextTags :tags="tags" alignment="justify" />
      </v-col>
      <v-col cols="6">
        <TextTags :tags="tags" alignment="left" />
      </v-col>
      <v-col cols="6">
        <TextTags :tags="tags" alignment="justify" />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import TextTags from './components/TextTags.vue';

export default {
  name: 'App',
  components: {
    TextTags,
  },
  data() {
    return {
      tags: [
        { text: 'бум' },
        { text: 'бам', icon: 'mdi-star' },
        { text: 'пих' },
        { text: 'пах', icon: 'mdi-heart' },
      ],
    };
  },
};
</script>

<style>
body {
  font-family: Roboto, sans-serif;
}
</style>
