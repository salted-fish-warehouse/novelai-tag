<script setup>
import tags from './assets/tags.yaml'
import Search from './components/Search.vue'
import Select from './components/Select.vue'
import Selected from './components/Selected.vue'

let num = 0
for (const i in tags) {
  for (const j in tags[i]) {
    for (const k in tags[i][j]) {
      num += 1
    }
  }
}
</script>

<template>
  <div class="main">
    <div class="title">
      NovelAI Tags <el-tag round size="small">{{ num }}</el-tag>
    </div>
    <Search />
    <Select />
  </div>
  <Selected />
</template>

<style lang="scss" scoped>
.main {
  min-height: calc(100vh - 10px - 40px);
}

.title {
  text-align: center;
  margin: 10px;
  font-size: 16px;
  font-weight: bold;
}
</style>
