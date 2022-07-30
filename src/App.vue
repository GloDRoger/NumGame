<template>
  <div class="table">
    <div
      class="row"
      v-for="(row, index) in base"
      :key="index"
      :class="'a' + index"
    >
      <div
        class="item"
        v-for="item in row"
        :key="item"
        :class="item === 0 ? 'zero' : ''"
        @click="canMove(item)"
      >
        <span v-if="item !== 0">{{ item }}</span>
      </div>
    </div>
  </div>
  <button @click="reStart">ReStart</button>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { flattenDeep } from "lodash";

const base = ref([
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 0],
]);
const Zero = ref();
/* 
  00 01 02
  10 11 12
  20 21 22
  */

function getZero() {
  // 得到0点坐标
  for (let i = 0; i < base.value.length; i++) {
    if (base.value[i].indexOf(0) > -1) {
      Zero.value = [i, base.value[i].indexOf(0)];
    }
  }
}

function move(y, x, moveY, moveX) {
  // 移动
  const num = base.value[y][x];
  base.value[y][x] = base.value[moveY][moveX];
  base.value[moveY][moveX] = num;
  const t = setTimeout(() => {
    isFinish();
  }, 0);
}

function isFinish() {
  const result = flattenDeep(base.value);
  if (result.join("") === "123456780") {
    alert("恭喜🎉游戏结束🎉🎉");
  }
}

function canMove(num) {
  getZero();
  let numPoint = [];
  for (let i = 0; i < base.value.length; i++) {
    if (base.value[i].indexOf(num) > -1) {
      numPoint[0] = i;
      numPoint[1] = base.value[i].indexOf(num);
    }
  }
  if (Zero.value[0] === numPoint[0] || Zero.value[1] === numPoint[1]) {
    if (
      Zero.value[0] - numPoint[0] === 1 ||
      Zero.value[0] - numPoint[0] === -1 ||
      Zero.value[1] - numPoint[1] === 1 ||
      Zero.value[1] - numPoint[1] === -1
    ) {
      //和0相邻
      move(Zero.value[0], Zero.value[1], numPoint[0], numPoint[1]);
      return;
    }
  }
}

function reStart() {
  for (let i = 0; i < 80; i++) {
    let num = Math.ceil(Math.random() * 8);
    canMove(num);
  }
}

onMounted(() => {
  reStart();
});
</script>

<style scoped>
.table {
  width: 320px;
  height: 320px;
  background-color: #e0e0e0;
  display: flex;
  flex-direction: column;
}
.row {
  display: flex;
}
.item {
  width: 100px;
  height: 100px;
  background-color: #1cd99a;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 5px;
  margin-left: 5px;
}
.zero {
  background-color: #e0e0e0;
}
button {
  margin-top: 10px;
}
</style>
