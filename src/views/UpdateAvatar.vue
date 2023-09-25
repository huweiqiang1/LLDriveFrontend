<template>
  <div>
    <Dialog
      :show="dialogConfig.show"
      :title="dialogConfig.title"
      :buttons="dialogConfig.buttons"
      width="400px"
      :showCancel="false"
      @close="dialogConfig.show = false"
    >
        <el-form
        :model="formData"
        :rules="rules"
         ref="formDataRef"
          label-width="80px"
          @submit.prevent
        >
            <el-form-item label="昵称" >
                {{formData.nickName}}
            </el-form-item>
            <el-form-item label="头像" prop="" >
               
            </el-form-item>
        </el-form>
    </Dialog>
  </div>
</template>

<script setup>
import { ref, reactive, getCurrentInstance, nextTick } from "vue"
const { proxy } = getCurrentInstance();

const api = {
    updateUserAvater: "updateUserAvater",

}

const formData = ref({});
const formDataRef = ref();
const rules = {
  title: [{ required: true, message: "请输入内容" }],
};

const dialogConfig = ref({
  show: false,
  title: "标题",
  buttons: [
   {
    type: "primary",
    text: "确定",
    click: (e) => {
      submitForm();
    },
  },
 ],
});

</script>

<style lang="scss" scoped>
</style>