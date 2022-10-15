<script setup>
import tags from '../assets/tags.yaml'
import { searchValue, searchList, selectList } from '../store/data'

const search = () => {
  searchList.value = {}
  const reg = new RegExp(searchValue.value, 'i')

  for (const i in tags) {
    for (const j in tags[i]) {
      for (const k in tags[i][j]) {
        if (reg.test(k) || reg.test(tags[i][j][k].value) || selectList.value[k]) {
          if (!searchList.value[i]) {
            searchList.value[i] = {}
          }
          if (!searchList.value[i][j]) {
            searchList.value[i][j] = {}
          }
          searchList.value[i][j][k] = tags[i][j][k]
        }
      }
    }
  }
}
</script>

<template>
  <el-input class="input" v-model="searchValue" placeholder="请输入搜索关键字" @input="search" />
</template>

<style lang="scss" scoped>
.input {
  margin: 5px;
  width: calc(100% - 10px);
}
</style>
