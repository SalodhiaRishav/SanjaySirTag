<template>
  <span>
    <span class="tag-text">{{tagText}} <span class="tag-cross" @click = "removeTag(tagText)">X</span></span>
  </span>
</template>

<script>
export default {
  props: {
    tagText: {
      type: String,
      required: true
    }
  },
  methods: {
    removeTag (tagText) {
      this.$emit('removeTag', tagText)
    }
  }
}
</script>

<style scoped>
.tag-text {
  vertical-align: text-top;
  vertical-align: top;
  background-color: rgba(239,246,252,1);
  font-size: 12px;
  padding: 4px;
  color: rgba(0,0,0,.55);
  margin-right: 10px;
}

.tag-cross {
  cursor: pointer;
  font-size: 10px;
}

.tag-cross:hover {
  color: black;
  font-size: 12px;
}

</style>
