<template>
  <div class="tag">
    <span class="tag-text">{{tagText}} <span class="tag-cross" @click = "removeTag(tagText)">X</span></span>
  </div>
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
.tag {
    background-color: rgba(239,246,252,1);
    padding: 4px;
    text-align: center;
    color: rgba(0,0,0,.55);
    margin: 6px;
    line-height: 12px;
}
.tag-text {
  font-size: 12px;
}

.tag-cross {
  cursor: pointer;
  font-size: 9px;
  margin-left: 8px;
}

.tag-cross:hover {
  color: black;
  font-size: 10px;
}

</style>
