<template>
  <div class="tags">
    <div class="new">
      <button @click="createTag">新增标签</button>
    </div>
    <ul class="current">
      <li
        :class="{ selected: value.map(item => item.id).indexOf(tag.id) >= 0 }"
        v-for="tag in tagList"
        :key="tag.id"
        @click="toggle(tag)"
      >
        {{ tag.name }}
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
// import Vue from "vue";
import { Component, Prop } from "vue-property-decorator";
import { mixins } from 'vue-class-component';
import TagHelper from '@/mixins/TagHelper';
import clone from '@/lib/clone';

@Component
export default class Tags extends mixins(TagHelper) {
  @Prop({default: []}) readonly value!: Tag[];

  get tagList() {
    return this.$store.state.tagList
  }

  created() {
    this.$store.commit('fetchTags')
  }

  toggle(tag: Tag) {
    const tags = clone(this.value)
    const index = tags.map(item => item.id).indexOf(tag.id);
    if (index >= 0) {
      tags.splice(index, 1);
    } else {
      tags.push(tag);
    }
    this.$emit('update:value', tags)
  }
}
</script>

<style lang="scss" scoped>
@import "~@/assets/style/helper.scss";

.tags {
  flex-grow: 1;
  font-size: 14px;
  padding: 16px;
  display: flex;
  flex-direction: column-reverse;
  background: #fff;
  > .current {
    display: flex;
    flex-wrap: wrap;
    > li {
      $bg: #d9d9d9;
      background: $bg;
      $h: 24px;
      height: $h;
      line-height: $h;
      border-radius: $h / 2;
      padding: 0 16px;
      margin-right: 12px;
      margin-top: 4px;
      &.selected {
        background: darken($bg, 50%);
        color: #fff;
      }
    }
  }
  > .new {
    padding-top: 16px;
    button {
      background: transparent;
      border: none;
      color: #999;
      border-bottom: 1px solid;
      padding: 0 4px;
    }
  }
}
</style>
