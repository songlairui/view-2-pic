<template>
  <div class="wrapper">
    <div class="actions">
      <div>已选 {{ files.length }} 个文件</div>
      <div>
        <span>
          <input
            :key="uniqueId"
            multiple
            :max="2"
            type="file"
            accept="jpg,jpeg,png,gif"
            @change="chooseTwoFile"
          />
        </span>
        <span>
          <button @click="swap">交换</button>
          <button @click="reload">刷新</button>
          {{ urls }}
        </span>
      </div>
    </div>
    <div class="stage">
      <div class="left">
        <img v-if="urls[0]" :src="urls[0]" />
      </div>
      <div class="right">
        <img v-if="urls[1]" :src="urls[1]" />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, watch } from "vue";

export default defineComponent({
  name: "App",
  setup() {
    const uniqueId = ref("");
    const files = ref<any[]>([]);
    function chooseTwoFile(e: any) {
      files.value = [...e.target.files, ...files.value].slice(0, 2);
      uniqueId.value = Math.random().toString(36).slice(2, 8);
    }
    function swap() {
      if (files.value.length !== 2) {
        return;
      }
      files.value = [...files.value].reverse();
    }

    const urls = ref<string[]>([]);
    watch(files, (val, old) => {
      urls.value = files.value.map(
        (file) => file && window.URL.createObjectURL(file)
      );
    });

    function reload() {
      window.location.reload();
    }

    return {
      files,
      uniqueId,
      chooseTwoFile,
      swap,
      urls,
      reload,
    };
  },
});
</script>

<style>
html,
body,
#app {
  width: 100vw;
  height: 100vh;
  margin: 0;
  padding: 0;
}
#app {
  display: flex;
}
</style>
<style lang='less' scoped>
.wrapper {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 6px;
  padding: 6px;
}
.actions {
  display: flex;
}
.stage {
  // width: 10cm;
  flex: 1;
  display: flex;
  gap: 6px;
  .left,
  .right {
    flex: 1;
    box-shadow: inset 0 0 2px salmon;
    img {
      max-width: 100%;
    }
  }
}
</style>
