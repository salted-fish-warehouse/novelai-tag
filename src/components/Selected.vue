<script setup>
import { Close, Check, Delete, Switch } from '@element-plus/icons-vue'
import { computed } from '@vue/reactivity'
import { selectList } from '../store/data'
import { ElMessage } from 'element-plus'
import 'element-plus/es/components/message/style/css'
import { ref, watch } from 'vue'

const isShow = computed(() => Object.keys(selectList.value).length > 0)

const showMode = ref(false)
const txt = ref('')

watch(selectList.value, () => {
  if (Object.keys(selectList.value).length < 1) {
    txt.value = ''
    return
  }
  let tags = ''
  for (const i in selectList.value) {
    tags += `${selectList.value[i]},`
  }
  tags = tags.slice(0, tags.length - 1)
  txt.value = tags
})

const onSelectedClick = (key) => {
  delete selectList.value[key]
}

const onChangeClick = () => {
  showMode.value = !showMode.value
}

const onClearClick = () => {
  for (const i in selectList.value) {
    delete selectList.value[i]
  }
  showMode.value = false
}

const onOkClick = () => {
  navigator.clipboard.writeText(txt.value).then((res) => {
    ElMessage({
      showClose: true,
      message: '已复制到剪切板'
    })
  })
}
</script>

<template>
  <div class="bottom-bar" v-show="isShow">
    <div class="tags-box" v-if="showMode">
      <el-card shadow="hover" class="card">
        <div>{{ txt }}</div>
      </el-card>
    </div>
    <div class="btn-list" v-else>
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
    </div>
    <div class="setting-btn-list">
      <el-button type="warning" @click="onChangeClick" class="btn">
        切换
        <el-icon class="el-icon--right">
          <Switch />
        </el-icon>
      </el-button>
      <el-button type="danger" @click="onClearClick" class="btn">
        清空
        <el-icon class="el-icon--right">
          <Delete />
        </el-icon>
      </el-button>
      <el-button type="primary" @click="onOkClick" class="btn">
        复制
        <el-icon class="el-icon--right">
          <Check />
        </el-icon>
      </el-button>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.bottom-bar {
  position: sticky;
  bottom: 0;
  display: flex;
  background: #eee;
  flex-direction: column;
  align-items: center;
  padding: 5px 5px 0 5px;
}

.card {
  font-size: 12px;
  color: #999;
}

.btn {
  margin: 3px 5px;
}
</style>
