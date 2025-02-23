<template>
  <div class="p-6 w-full">
    <h1 class="text-3xl font-bold mb-6 text-center">シフト表</h1>
    <div class="overflow-x-auto overflow-y-auto max-h-[70vh]"> <!-- 横スクロール許可 -->
      <table class="w-full border border-gray-400 text-xl">  <!-- 親サイズまで幅を広げる。ボーダーはグレーで文字は少し大きめ -->
        <thead class="sticky top-0 bg-gray-300 z-10">
          <tr class="bg-gray-300">
            <th class="border px-16 py-6 sticky left-0 bg-gray-300">従業員</th>
            <th v-for="date in dates" :key="date" class="border px-16 py-6"
              :class="{'bg-blue-100': isSaturday(date), 'bg-red-100': isSunday(date)}">
              {{ formatDate(date) }}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="employee in employees" :key="employee.id">
            <td class="border px-6 py-6 font-semibold sticky left-0 bg-white">{{ employee.name }}</td>
            <td v-for="date in dates" :key="date" class="border px-6 py-6"
              :class="{'bg-blue-100': isSaturday(date), 'bg-red-100': isSunday(date)}">
              <div class="flex flex-col space-y-2">
                <input 
                  type="text" placeholder="HH:MM"
                  v-model="shifts[employee.id][date].start" 
                  class="w-full text-center border rounded-lg p-2 text-xl"
                  >
                <input 
                  type="text" placeholder="HH:MM" 
                  v-model="shifts[employee.id][date].end" 
                  class="w-full text-center border rounded-lg p-2 text-xl"
                  >
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';

interface Employee {
  id: number;
  name: string;
}

interface Shift {
  start: string;
  end: string;
}

const dates = ref<string[]>([]);
const employees = ref<Employee[]>([
  { id: 1, name: '田中' },
  { id: 2, name: '佐藤' },
  { id: 3, name: '鈴木' },
  { id: 4, name: '山田' },
  { id: 5, name: '斎藤' },
  { id: 6, name: '松本' },
  { id: 7, name: '中村' },
  { id: 8, name: '小林' },
  { id: 9, name: '加藤' },
  { id: 10, name: '吉田' },
]);
const shifts = ref<Record<number, Record<string, Shift>>>({});

const getDates = () => {
  const result: string[] = [];
  const today = new Date();
  const year = today.getFullYear();
  const month = today.getMonth();
  const daysInMonth = new Date(year, month + 1, 0).getDate();
  for (let i = 1; i <= daysInMonth; i++) {
    const date = new Date(year, month, i);
    date.setHours(date.getHours() + 9); // 日本標準時に対応
    result.push(date.toISOString().split('T')[0]);
  }
  return result;
}

const formatDate = (dateString: string) => {
  const date = new Date(dateString);
  const day = date.getDate();
  const weekday = ['日', '月', '火', '水', '木', '金', '土'][date.getDay()];
  return `${day} (${weekday})`;
};

const initializeShifts = () => {
  employees.value.forEach(employee => {
    shifts.value[employee.id] = {};
    dates.value.forEach(date => {
      shifts.value[employee.id][date] = { start: '', end: '' };
    });
  });
};

onMounted(() => {
  dates.value = getDates();
  initializeShifts();
});

const isSaturday = (dateString: string) => {
  return new Date(dateString).getDay() === 6;
};

const isSunday = (dateString: string) => {
  return new Date(dateString).getDay() === 0;
};
  dates.value = getDates();
  initializeShifts();
</script>
