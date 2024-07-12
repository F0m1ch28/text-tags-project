<template>
  <div :class="bemBlock">
    <v-row
      :class="`${bemBlock}__row`"
      :style="rowStyle"
      ref="tagRow"
    >
      <v-col
        v-for="(tag, index) in visibleTags"
        :key="index"
        :class="`${bemBlock}__tag`"
        :style="colStyle"
      >
        <span>{{ tag.text }}</span>
        <v-icon v-if="tag.icon" :class="`${bemBlock}__icon`">{{ tag.icon }}</v-icon>
        <v-icon v-if="index !== visibleTags.length - 1" :class="`${bemBlock}__separator`">mdi-circle-small</v-icon>
      </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  name: 'TextTags',
  props: {
    tags: {
      type: Array,
      required: true,
    },
    alignment: {
      type: String,
      default: 'left',
      validator: value => ['left', 'justify'].includes(value),
    },
  },
  data() {
    return {
      containerWidth: 0,
    };
  },
  computed: {
    bemBlock() {
      return 'text-tags';
    },
    rowStyle() {
      return {
        justifyContent: this.alignment === 'justify' ? 'space-between' : 'flex-start',
      };
    },
    colStyle() {
      return {
        display: 'flex',
        alignItems: 'center',
        whiteSpace: 'nowrap',
      };
    },
    visibleTags() {
      let totalWidth = 0;
      const visibleTags = [];

      for (const tag of this.tags) {
        const tagWidth = this.getTagWidth(tag);
        if (totalWidth + tagWidth > this.containerWidth) break;
        visibleTags.push(tag);
        totalWidth += tagWidth;
      }

      return visibleTags;
    },
  },
  watch: {
    tags: {
      immediate: true,
      handler() {
        this.adjustTags();
      },
    },
  },
  mounted() {
    window.addEventListener('resize', this.adjustTags);
    this.adjustTags();
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.adjustTags);
  },
  methods: {
    adjustTags() {
      this.$nextTick(() => {
        this.containerWidth = this.$refs.tagRow.clientWidth;
      });
    },
    getTagWidth(tag) {
      const span = document.createElement('span');
      span.style.visibility = 'hidden';
      span.style.position = 'absolute';
      span.style.whiteSpace = 'nowrap';
      span.textContent = tag.text;

      if (tag.icon) {
        const icon = document.createElement('i');
        icon.className = 'v-icon mdi ' + tag.icon;
        span.appendChild(icon);
      }

      const separator = document.createElement('i');
      separator.className = 'v-icon mdi mdi-circle-small';
      span.appendChild(separator);

      document.body.appendChild(span);
      const width = span.offsetWidth;
      document.body.removeChild(span);

      return width;
    },
  },
};
</script>

<style scoped lang="scss">
.text-tags {
  &__row {
    flex-wrap: nowrap;
  }
  &__tag {
    display: flex;
    align-items: center;
    white-space: nowrap;
  }
  &__separator {
    margin-left: 8px;
  }
  &__icon {
    margin-left: 4px;
  }
}
</style>
