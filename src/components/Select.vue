<template>
  <el-tabs v-model="activeName" class="demo-tabs">
    <el-tab-pane v-for="(tag, key) in tags" :label="key" :name="key" :key="key">
      <div v-for="(sub, key, index) in tag" :key="`sub-${index}`">
        <el-divider v-if="key" content-position="left">{{ key }}</el-divider>
        <div class="card-list">
          <el-card
            v-for="(item, name) in sub"
            shadow="hover"
            class="card"
            :class="{ selected: selectList[name] }"
            @click="onCardClick(name, item.value)"
            v-show="search(name, item.value)"
          >
            <div>{{ name }}</div>
            <div class="value">{{ item.value }}</div>
          </el-card>
        </div>
      </div>
    </el-tab-pane>
  </el-tabs>
</template>
<script setup>
import { ref } from 'vue'
import tags from '../assets/tags.yaml'
import { searchValue, selectList } from '../store/data'

const activeName = ref(Object.keys(tags)[0])

const onCardClick = (name, value) => {
  if (selectList.value[name]) {
    delete selectList.value[name]
  } else {
    selectList.value[name] = value
  }
}

const search = (name, value) => {
  if (searchValue.value.length < 1) return true
  const reg = new RegExp(searchValue.value, 'i')
  return reg.test(name) || reg.test(value) || selectList.value[name]
}
</script>

<style lang="scss" scoped>
.card-list {
  display: flex;
  flex-wrap: wrap;
  flex-direction: row;

  .card {
    cursor: pointer;
    margin: 5px;
    height: 80px;

    .value {
      font-size: 12px;
      color: #999;
    }
  }
}

.selected {
  border: 1px solid var(--el-color-primary);
}
</style>
