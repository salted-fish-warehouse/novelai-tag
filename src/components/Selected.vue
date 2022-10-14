<template>
  <div class="bottom-bar" v-show="isShow">
    <el-button
      v-for="(_item, key) in selectList"
      :key="`selected-${key}`"
      @click="onSelectedClick(key)"
      class="btn"
    >
      {{ key }}
      <el-icon class="el-icon--right">
        <Close />
      </el-icon>
    </el-button>
    <div style="margin-left: auto">
      <el-button type="warning" @click="onOkClick" :icon="Picture" v-if="showAll" />
      <el-button type="primary" @click="onOkClick" class="btn">
        复制
        <el-icon class="el-icon--right">
          <Check />
        </el-icon>
      </el-button>
    </div>
  </div>
</template>

<script setup>
import { Close, Check, Picture } from '@element-plus/icons-vue'
import { computed } from '@vue/reactivity'
import { selectList } from '../store/data'
import { ElNotification } from 'element-plus'
import 'element-plus/es/components/notification/style/css'
import { ref } from 'vue'

const isShow = computed(() => Object.keys(selectList.value).length > 0)

const showAll = ref(false)

const onSelectedClick = (key) => {
  delete selectList.value[key]
}

const onOkClick = () => {
  let tags = ''
  for (const i in selectList.value) {
    tags += `${selectList.value[i]},`
  }
  tags = tags.slice(0, tags.length - 1)

  navigator.clipboard.writeText(tags).then((res) => {
    ElNotification({
      title: '已复制到剪切板',
      message: tags,
      type: 'success',
    })
  })
}
</script>

<style lang="scss" scoped>
.bottom-bar {
  position: sticky;
  bottom: 0;
  display: flex;
  justify-content: flex-start;
  align-items: center;
  flex-wrap: wrap;
  background: #eee;

  .btn {
    margin: 3px 5px;
  }
}
</style>
