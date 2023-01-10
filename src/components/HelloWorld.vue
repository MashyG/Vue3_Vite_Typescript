<script setup lang="ts">
import { computed, ref } from 'vue'
import { read, utils } from 'xlsx'

defineProps<{ msg: string }>()

const fileList = ref([])
const fileAfter = ref({})

const showData = ref({})

const handleFileChange = (file: any, files: any) => {
  const fileReader = new FileReader()
  fileReader.onload = (ev: any) => {
    try {
      const data = ev.target.result
      const workbook = read(data, {
        type: 'binary'
      })
      fileAfter.value = utils.sheet_to_json(workbook.Sheets[workbook.SheetNames[0]])
      console.log(fileAfter.value)
    } catch(err) {
      console.error(err)
    }
  }
  fileReader.readAsBinaryString(new Blob([file.raw]));
  return false
}

const tableData = computed(() => {
  return fileAfter.value
})

const clearList = () => {
  fileList.value = []
  fileAfter.value = []
}

</script>

<template>
  <h1>{{ msg }}</h1>
  <h2>Parse XLSX File</h2>
  <ElButton @click="clearList">清空</ElButton>
  <div>
    <el-upload
      v-model:file-list="fileList"
      class="upload-demo"
      action="#"
      :limit="1"
      accept=".xls,.xlsx"
      :auto-upload="false"
      :on-change="handleFileChange"
    >
      <el-button type="primary">Click to upload</el-button>
      <template #tip>
        <div class="el-upload__tip">only support Excel file</div>
      </template>
    </el-upload>
    After:
    <p>
      {{ tableData }}
    </p>
  </div>
</template>

<style scoped>
a {
  color: #42b983;
}

label {
  margin: 0 0.5em;
  font-weight: bold;
}

code {
  background-color: #eee;
  padding: 2px 4px;
  border-radius: 4px;
  color: #304455;
}
</style>
