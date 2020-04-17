<template>
  <div>
    <div class="tag-flex-container">
        <tag v-for="tag in tags" :key="tag" @removeTag="removeTag" :tagText="tag"></tag>
        <input placeholder="Add tag" ref="tagInput" v-model="newTag" @keydown.enter.stop.prevent="pushTag(newTag)" @keydown="handleKeydown" type="text" size="1" class="tag-input">
    </div>
    <ul class="tag-options">
      <li v-for="tag in filteredTagOptions" @click="pushTag(tag)" :key="tag">{{tag}}</li>
    </ul>
  </div>
</template>

<script>
import Tag from './Tag'

export default {
  components: {
    Tag
  },
  data () {
    return {
      newTag: '',
      tags: [],
      tagOptions: ['one', 'two', 'three'],
      filteredTagOptions: [],
      isTagInputVisible: false
    }
  },
  watch: {
    newTag (newVal) {
      if (newVal === '') {
        this.filteredTagOptions = []
      } else {
        const filterOptions = []
        this.tagOptions.forEach(tag => {
          var patt = new RegExp(newVal)
          var res = patt.test(tag)
          if (res) {
            filterOptions.push(tag)
          }
        })
        this.filteredTagOptions = filterOptions
      }
    }
  },
  methods: {
    addTag () {
      this.isTagInputVisible = true
      this.$nextTick(() => this.$refs.tagInput.focus())
    },
    pushTag (newTag) {
      if (newTag === '') {
        return
      }
      if (this.tags.indexOf(newTag) === -1) {
        if (this.tagOptions.indexOf(newTag) === -1) {
          this.tagOptions.push(newTag)
          this.filteredTagOptions = this.tagOptions
        }
        this.tags.push(newTag)
        this.newTag = ''
      }
    },
    removeTag (tagText) {
      const index = this.tags.indexOf(tagText)
      this.tags.splice(index, 1)
    },
    handleKeydown (e) {
      if (e.key === 'Backspace') {
        if (this.newTag === '') {
          if (this.tags.length === 0) {
            return
          }
          this.tags.pop()
        }
      }
    }
  }
}
</script>

<style scoped>
.tag-flex-container {
  max-width: 450px;
  display: flex;
  padding: 4px;
  flex-wrap: wrap;
  border: 1px solid #8b9396;
}

.tag-flex-container > .tag-input {
    flex-grow: 1;
    flex-shrink: 0;
    flex-basis: auto;
    min-width: 100px;
    border: none;
    padding: 0px;
    margin: 6px;
}

.tag-flex-container > .tag-input:focus{
  outline: none;
}

.tag-options {
  max-width: 450px;
  list-style: none;
  text-align: left;
  padding-left: 8px;
  border: 1px solid;
  border-top: 0px;
  border-color: rgba(200,200,200,1);
  margin-top: 0px;
  border-color: rgba(var(--palette-neutral-20,200, 200, 200),1);
  font-size: 13px;
}

.tag-options > li {
  margin-top: 2px;
  margin-bottom: 2px;
}

.tag-options > li:hover {
  background-color: rgba(239,246,252,1);
}
</style>
