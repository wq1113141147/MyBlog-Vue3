<template>
  <form
      id="data-form-container"
      ref="form"
      @submit.prevent="handleSubmit"
      class="dataForm-container"
  >
    <div class="form-item">
      <div class="input-area">
        <input
            type="text"
            maxlength="10"
            v-model="formData.nickname"
            placeholder="用户昵称"
        />
        <span class="tip">{{ formData.nickname.length }}/10</span>
      </div>
      <div class="error">{{ error.nickname }}</div>
    </div>
    <div class="form-item">
      <div class="text-area">
        <textarea
            maxlength="300"
            placeholder="输入内容"
            v-model="formData.content"
        ></textarea>
        <span class="tip">{{ formData.content.length }}/300</span>
      </div>
      <div class="error">{{ error.content }}</div>
    </div>
    <div class="form-item">
      <div class="button-area">
        <button :disabled="isSubmitting">
          {{ isSubmitting ? "提交中..." : "提交" }}
        </button>
      </div>
    </div>
  </form>
</template>

<script>
import {ref} from "vue";
import {ElMessageBox,ElMessage} from 'element-plus';

export default {
  setup(props,ctx) {
    const formData = ref({
      nickname: "",
      content: "",
    });
    const error = ref({
      nickname: "",
      content: "",
    })
    const isSubmitting = ref(false)
    const handleSubmit = () => {
      error.value.nickname = formData.value.nickname ? "" : "请填写昵称";
      error.value.content = formData.value.content ? "" : "请填写内容";
      if (error.value.nickname || error.value.content) {
        // 有错误
        return;
      }
      isSubmitting.value = true; // 正在提交，防止重复点击
      ctx.emit("submit",formData.value,(msg)=>{
        ElMessage.success(msg)
        isSubmitting.value=false;
        formData.value.nickname="";
        formData.value.content="";
      })
    }
    return {
      formData,
      error,
      isSubmitting,
      handleSubmit
    }
  }
};
</script>

<style scoped lang="less">
@import "public/style/globalColor";

.dataForm-container {
  margin-bottom: 20px;
  overflow: hidden;
}

.form-item {
  margin-bottom: 8px;
}

.input-area {
  width: 50%;
  position: relative;
}

.text-area {
  position: relative;
}

.tip {
  position: absolute;
  right: 5px;
  bottom: 5px;
  color: #b4b8bc;
  font-size: 12px;
}

input,
textarea {
  display: block;
  width: 100%;
  box-sizing: border-box;
  border: 1px dashed @gray;
  outline: none;
  color: @words;
  font-size: 14px;
  border-radius: 4px;

  &:focus {
    border-color: @primary;
  }
}

input {
  padding: 0 15px;
  height: 40px;
}

textarea {
  resize: none;
  padding: 8px 15px;
  height: 120px;
}

.error {
  margin-top: 6px;
  color: @danger;
  font-size: 14px;
  height: 20px;
  line-height: 20px;
}

button {
  position: relative;
  cursor: pointer;
  border: none;
  outline: none;
  width: 100px;
  height: 34px;
  color: #fff;
  border-radius: 4px;
  background: @primary;

  &:hover {
    background: darken(@primary, 10%);
  }

  &:disabled {
    background: lighten(@primary, 20%);
    cursor: not-allowed;
  }
}
</style>
