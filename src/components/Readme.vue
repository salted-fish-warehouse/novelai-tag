<script setup>
import tags from '../assets/tags.yaml'
import { ref } from 'vue'

const activeNames = ref('1')

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
  <div class="readme">
    <el-collapse v-model="activeNames">
      <el-collapse-item title="说明" name="1">
        <template #title>
          <div class="title">
            NovelAI Tags <el-tag round type="info" size="small" effect="plain">{{ num }}</el-tag>
          </div>
        </template>
        <div class="text">
          <!-- <span>没想好</span> -->
          <el-link href="https://github.com/blacktunes/novelai-tag">Github</el-link>
        </div>
      </el-collapse-item>
    </el-collapse>
  </div>
</template>

<style lang="scss" scoped>
.readme {
  margin: 5px;
}

.title {
  text-align: center;
  margin: 10px;
  font-size: 16px;
  font-weight: bold;
  width: 100%;
}

.text {
  font-size: 12px;
  color: #999;
  text-align: center;
}
</style>
