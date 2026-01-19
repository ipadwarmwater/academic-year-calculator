<script setup lang="ts">
    import { computed, ref } from 'vue' // 必須匯入 ref 才能使用
    const date = new Date();
    const currentYear = date.getFullYear();
    const currentMonth = date.getMonth() + 1;
    const birthday = ref<string>('');
    const academicYear = ref<number>(currentMonth >= 8 ? currentYear - 1911 : currentYear -1 - 1911);
    const assignYear = (ref<number | null>(null));
    const calculateYear = () => {
        // 檢查是否有輸入生日，避免 split 報錯
        if (!birthday.value) {
            alert("請先選擇生日！");
            return;
        }

        const parts = birthday.value.split('-');
        const birthYear = parseInt(parts[0]);
        const birthMonth = parseInt(parts[1]);
        const birthDay = parseInt(parts[2]);
        // 以9/1為分界點，決定入學學年度
        if (birthMonth >= 9 && birthDay > 1) {
            assignYear.value = birthYear + 1 - 1911;
        } else {
            assignYear.value = birthYear - 1911;
        }
    }
    
    const enrollmentYears = computed(() => {

    return {
        toddler: assignYear.value !== null ? assignYear.value + 2 : null,
        junior: assignYear.value !== null ? assignYear.value + 3 : null,
        middle: assignYear.value !== null ? assignYear.value + 4 : null,
        senior: assignYear.value !== null ? assignYear.value + 5 : null,
        elementary: assignYear.value !== null ? assignYear.value + 6 : null,
    };
});
    const clear = () => {
        birthday.value = '';
        assignYear.value = null;
    }
</script>

<template>
    <div class="birthdayInput" v-if="assignYear === null">
    <p> 請輸入您家寶貝的生日 </p>
    <!--資料的雙向綁定-->
    <!--v-model-->
    <input type="date" v-model="birthday" />
    <button @click="calculateYear()">計算入學學年度</button>
    </div>

    <div class="showAssignYear" v-if="assignYear !== null && enrollmentYears">
    <!--資料的單向綁定-->
    <p>現在是 {{ academicYear }} 學年度</p>
    <p> 您家寶貝的生日是：{{ birthday }} </p>
    <p> 您家寶貝於{{ enrollmentYears.toddler }}學年度入幼幼班。</p>
    <p> 您家寶貝於{{ enrollmentYears.junior }}學年度入小班。</p>
    <p> 您家寶貝於{{ enrollmentYears.middle }}學年度入中班。</p>
    <p> 您家寶貝於{{ enrollmentYears.senior }}學年度入大班。</p>
    <p> 您家寶貝於{{ enrollmentYears.elementary }}學年度入小學。</p>
    <button @click="clear()">清除輸入資料</button>
    </div>
</template>

<style scoped>
  /* 讓整個畫面置中，且字體好看一點 */
:host {
  display: flex;
  justify-content: center;
}

/* 讓輸入框和按鈕不要黏在一起 */
input, button {
  display: block; /* 讓它們各佔一行 */
  width: 100%;    /* 寬度填滿卡片 */
  padding: 10px;
  margin: 10px 0;
  border-radius: 5px;
  border: 1px solid #ccc;
  box-sizing: border-box; /* 確保 padding 不會撐破寬度 */
  font-size: 20px;
}

/* 幫按鈕換個亮眼的顏色 */
button {
  background-color: #42b883; /* Vue 的綠色 */
  color: white;
  border: none;
  cursor: pointer;
  font-weight: bold;
}

button:hover {
  background-color: #33a06f; /* 滑鼠移過去變深一點 */
}

p{
    font-size: 24px;
}
/* 讓結果的文字稍微加強閱讀性 */
.showAssignYear p {
  border-bottom: 1px solid #eee;
  padding-bottom: 8px;
  color: #333;
}
</style>
