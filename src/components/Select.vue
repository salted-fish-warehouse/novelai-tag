<script setup>
import { ref, computed, watch } from 'vue'
import tags from '../assets/tags.yaml'
import { searchValue, searchList, selectList } from '../store/data'

const list = computed(() => (searchValue.value.length > 0 ? searchList.value : tags))
const activeName = ref(Object.keys(list.value)[0])

watch(searchList, () => {
  if (!searchList.value[activeName.value]) {
    activeName.value = Object.keys(list.value)[0]
  }
})

const onCardClick = (name, value) => {
  if (selectList.value[name]) {
    delete selectList.value[name]
  } else {
    selectList.value[name] = value
  }
}

const getNum = (tag) => {
  let num = 0
  for (const i in tag) {
    for (const j in tag[i]) {
      ++num
    }
  }
  return num
}
</script>

<template>
  <el-tabs v-model="activeName">
    <el-tab-pane v-for="(tag, key) in list" :name="key" :key="key">
      <template #label>
        <span class="custom-tabs-label">
          <span>{{ key }}</span>
          <el-tag
            style="margin-left: 5px"
            round
            :type="activeName === key ? '' : 'info'"
            size="small"
            effect="plain"
          >
            {{ getNum(tag) }}
          </el-tag>
        </span>
      </template>
      <div v-for="(sub, key, index) in tag" :key="`sub-${index}`">
        <el-divider v-if="key" content-position="left">
          {{ key }}
        </el-divider>
        <div class="card-list">
          <el-card
            v-for="(item, name) in sub"
            shadow="hover"
            class="card"
            :class="{ selected: selectList[name] }"
            @click="onCardClick(name, item.value)"
          >
            <div>{{ name }}</div>
            <div class="value">{{ item.value }}</div>
          </el-card>
        </div>
      </div>
    </el-tab-pane>
  </el-tabs>
</template>

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

<style lang="scss">
.el-tabs__header {
  z-index: 9;
  position: sticky;
  top: 0;
  background: #fff;
}
</style>
