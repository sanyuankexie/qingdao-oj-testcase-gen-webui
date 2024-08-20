<script setup lang="ts">
import { ref } from 'vue';
import JSZip from 'jszip';
import { saveAs } from 'file-saver';

const testCases = ref([{ input: '', output: '' }]);

const addTestCase = (index: number) => {
  testCases.value.splice(index + 1, 0, { input: '', output: '' });
};

const removeTestCase = (index: number) => {
  testCases.value.splice(index, 1);
};

const generateZip = () => {
  const zip = new JSZip();

  testCases.value.forEach((testCase, index) => {
    zip.file(`${index + 1}.in`, testCase.input);
    zip.file(`${index + 1}.out`, testCase.output);
  });

  zip.generateAsync({ type: 'blob' }).then((content) => {
    saveAs(content, 'test-cases.zip');
  });
};
</script>

<template>
  <div class="container">
    <header class="fixed-header">
      <h1>Qingdao-OJ 测试用例生成</h1>
      <h3>生成并导出测试用例</h3>
    </header>

    <main class="main-content">
      <div v-for="(testCase, index) in testCases" :key="index" class="test-case">
        <div class="test-case-header">
          <h3>测试用例 {{ index + 1 }}</h3>
        </div>
        <div class="io-fields">
          <textarea v-model="testCase.input" placeholder="输入内容"></textarea>
          <textarea v-model="testCase.output" placeholder="输出内容"></textarea>
          <div class="buttons">
            <button class="add-btn" @click="addTestCase(index)">+</button>
            <button class="remove-btn" @click="removeTestCase(index)" :disabled="testCases.length === 1">-</button>
          </div>
        </div>
      </div>
      <div class="button-wrapper">
        <button class="export-btn" @click="generateZip">导出测试用例</button>
      </div>
    </main>
  </div>
</template>

<style scoped>
.container {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.fixed-header {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  background-color: white;
  text-align: center;
  padding: 20px 0;
  z-index: 1000;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.main-content {
  margin: 0 auto;
  padding: 120px 20px 20px; /* 确保与顶部的fixed-header有足够的间距 */
  width: 100%;
}

.test-case {
  margin-bottom: 30px;
  display: flex;
  align-items: center;
}

.test-case-header {
  flex: 0 0 150px;
  text-align: center;
}

.io-fields {
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 10px;
  flex-grow: 1;
}

textarea {
  flex: 1;
  height: 80px;
  padding: 10px;
  font-size: 14px;
  border-radius: 5px;
  border: 1px solid #ccc;
  box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.1);
}

.buttons {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 5px;
}

.add-btn, .remove-btn {
  width: 30px;
  height: 30px;
  font-size: 24px;
  color: #007bff;
  background-color: transparent;
  border: none;
  border-radius: 50%;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background-color 0.3s ease, color 0.3s ease;
}

.add-btn:hover, .remove-btn:hover {
  color: #0056b3;
  background-color: #f0f0f0;
}

.remove-btn:disabled {
  color: #ccc;
  cursor: not-allowed;
}

.button-wrapper {
  display: flex;
  justify-content: flex-end;
  margin-top: 20px;
}

.export-btn {
  width: calc(100% - 150px - 40px); /* 与输入框和输出框组合后的宽度对齐 */
  padding: 12px;
  font-size: 16px;
  color: #fff;
  background-color: #007bff; /* 蓝色 */
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.export-btn:hover {
  background-color: #0056b3; /* 深蓝色悬停效果 */
}
</style>
