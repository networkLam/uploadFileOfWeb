<script setup lang="ts">
import axios from 'axios';
import { ref } from 'vue'
import type { UploadInstance } from 'element-plus'
defineProps<{ msg: string }>()
type Submit = () => void;
type Decomposition = (e: any) => void;
const file = ref<HTMLInputElement | null>(null); //定位到file的元素

const submit: Submit = () => {
  console.log("submit");
  if (file.value && file.value.files?.length) {
    // const name = file.value.files[0].name;
    const formData = new FormData();  //搞文件上传一定要想到formData
    const fileToUpload = file.value.files[0];  
    const name = fileToUpload.name;  
    formData.append('name', name);  
    formData.append('multipartFile', fileToUpload);  
    axios({
      url: "http://localhost:8080/test/upload",
      method: "post",
      headers: {
        'Content-Type': 'multipart/form-data'//这里需要加入请求头便于让系统识别是传输的文件
      },
      data:formData
    }).then(response=>{
      console.log(response)
    }).catch(err=>{
      console.log(err)
    })
  }
}

const decomposition: Decomposition = (e) => {
  if (file.value) {
    console.log(file.value.files);
  }

}


// const uploadRef = ref<UploadInstance>()

// const submitUpload = () => {

//   const formData = new FormData();
//   formData.append("name","你好");
//   // formData.append("multipartFile",uploadRef.value?.fileList[0])
//   uploadRef.value!.submit()
// }


</script>

<template>
  <div class="wrapper">
    <!-- 文件上传 -->
    <input type="file" name="file" id="f" ref="file" @change="decomposition">
    <button type="button" @click="submit">上传文件</button>
    <!-- <el-upload
    ref="uploadRef"
    class="upload-demo"
    action="http://localhost:8080/test/upload"
    name="multipartFile"
    :auto-upload="false"
  >
    <template #trigger>
      <el-button type="primary">select file</el-button>
    </template>

    <el-button class="ml-3" type="success" @click="submitUpload">
      upload to server
    </el-button>

    <template #tip>
      <div class="el-upload__tip">
        jpg/png files with a size less than 500kb
      </div>
    </template>
  </el-upload> -->
  </div>
</template>

<style scoped></style>
