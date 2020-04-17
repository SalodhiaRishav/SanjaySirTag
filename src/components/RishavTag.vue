<template>
  <div>
    <tag v-for="tag in tags" :key="tag" @removeTag="removeTag" :tagText="tag"></tag>
    <span v-if="!isTagInputVisible" class="add-tag-text" @click="addTag">Add tag</span>
    <div class="tag-input-container" v-if="isTagInputVisible">
      <input ref="tagInput"  class ="add-tag-input" v-model="newTag" @keydown.enter.stop.prevent="pushTag(newTag)" type="text">
      <ul class="tag-options">
        <li v-for="tag in filteredTagOptions" @click="pushTag(tag)" :key="tag">{{tag}}</li>
      </ul>
    </div>
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
  mounted () {
    this.filteredTagOptions = this.tagOptions
  },
  watch: {
    newTag (newVal) {
      if (newVal === '') {
        this.filteredTagOptions = this.tagOptions
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
    }
  }
}
</script>

<style scoped>
.add-tag-text {
  background-color: rgba(239,246,252,1);
  font-size: 12px;
  padding: 4px;
  color: rgba(0,0,0,.55);
  cursor: pointer;
}

.add-tag-text:hover {
  background-color: rgba(206,229,248,1)
}
.tag-input-container {
  display: inline-block;
  padding-left: 6px;
  font-size: 12px;
  color: rgba(0,0,0,.55);
  width: 150px;
}
.add-tag-input {
    padding-left: 2px;
    font-size: 12px;
    color: rgba(0,0,0,.55);
    width: 146px;
    overflow: hidden;
    margin: 0;
    outline: none;
    border: 1px solid;
    border-color: rgba(200,200,200,1);
    border-color: rgba(var(--palette-neutral-20,200, 200, 200),1);
    background-color:  rgba(239,246,252,1);
}

.tag-options {
  list-style: none;
  text-align: left;
  padding-left: 8px;
  border: 1px solid;
  border-top: 0px;
  border-color: rgba(200,200,200,1);
  margin-top: 0px;
  border-color: rgba(var(--palette-neutral-20,200, 200, 200),1);
}

.tag-options > option {
  color: rgba(0,0,0,.55);
  width: 146px;
  background-color: rgba(239,246,252,1);
}

datalist{
  font-weight: 12px;
  color: rgba(0,0,0,.55);
  width: 146px;
  background-color: rgba(239,246,252,1);
}
.tag-options > li {
  margin-top: 2px;
  margin-bottom: 2px;
}

.tag-options > li:hover {
  background-color: rgba(239,246,252,1);
}

.plus-sign {
  background-color: rgba(239,246,252,1);
  font-size: 12px;
  padding: 4px;
  color: rgba(0,0,0,.55);
  cursor: pointer;
}
</style>
