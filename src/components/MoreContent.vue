<template>
  <div class="act-form">
    <iframe :src="src"></iframe>
  </div>
</template>

<style lang="scss" scoped>
.act-form {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100vh; // 使父容器占满整个视口高度
}

iframe {
  width: 100%;
  height: 100%;
  border: none; // 可选，移除iframe边框
  border-radius: 10px; // 设置圆角，可以根据需要调整数值
  overflow: hidden; // 确保内容不会溢出圆角边界
}
</style>

<script>
export default {
  data () {
    return {
      src: 'https://ai.lawxq.cn/chat/share?shareId=p5vi0ajwyvq9ri6d0u99w8hg&showHistory=0'
    }
  }
}
</script>
