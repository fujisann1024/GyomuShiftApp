<template>
  <div class="p-6 w-full">
    <h1 class="text-3xl font-bold mb-6 text-center">シフト表</h1>
    <div class="overflow-x-auto overflow-y-auto max-h-[70vh]">
      <table class="w-full border border-gray-400 text-xl">
        <thead class="sticky top-0 z-10 bg-gray-300">
          <tr>
            <th class="sticky left-0 border px-16 py-6 bg-gray-300">従業員</th>
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
                  v-model="getShift(employee, date).starttime"
                  class="w-full text-center border rounded-lg p-2 text-xl">
                <input 
                  type="text" placeholder="HH:MM"
                  v-model="getShift(employee, date).endtime"
                  class="w-full text-center border rounded-lg p-2 text-xl">
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

interface EmployeeShift {
  workday: string;
  starttime?: string;
  endtime?: string;
  breakflg: boolean;
}

interface Employee {
  id: number;
  name: string;
  shfits: EmployeeShift[];
}

const dates = ref<string[]>([]);
const employees = ref<Employee[]>([
  { id: 1, name: 'Tanaka', shfits: [
    { workday: '2025-02-01', starttime: '09:00', endtime: '18:00', breakflg: false },
    { workday: '2025-02-02', starttime: '09:00', endtime: '18:00', breakflg: false },
  ]},
  { id: 2, name: 'Sato', shfits: [
    { workday: '2025-02-01', starttime: '18:00', endtime: '22:00', breakflg: false },
    { workday: '2025-02-02', starttime: '18:00', endtime: '22:00', breakflg: false },
  ]},
  {
    id: 3,
    name: 'Suzuki',
    shfits: [
      { workday: '2025-02-01', starttime: '09:00', endtime: '18:00', breakflg: false },
      { workday: '2025-02-02', starttime: '09:00', endtime: '18:00', breakflg: false },
    ],
  }
]);

/**
 * 月の日付を取得する
 */
const getMonthListDates = () => {
  const result: string[] = [];
  const today = new Date(2025,2,24); // TODO: 本来は現在の日付を取得
  const year = today.getFullYear();
  const month = today.getMonth();
  const daysInMonth = new Date(year, month + 1, 0).getDate();
  for (let i = 1; i <= daysInMonth; i++) {
    const date = new Date(year, month, i);
    date.setHours(date.getHours() + 9); // 日本標準時に対応
    result.push(date.toISOString().split('T')[0]);
  }
  return result;
};

/**
 * 日付文字列をフォーマットする
 */
const formatDate = (dateString: string) => {
  const date = new Date(dateString);
  const day = date.getDate();
  const weekday = ['日', '月', '火', '水', '木', '金', '土'][date.getDay()];
  return `${day} (${weekday})`;
};

/**
 * 土曜日の判定
 */
const isSaturday = (dateString: string) => {
  return new Date(dateString).getDay() === 6;
};

/**
 * 日曜日の判定
 */
const isSunday = (dateString: string) => {
  return new Date(dateString).getDay() === 0;
};

/**
 * 指定した従業員と日付に対応するシフトを取得
 */
const getShift = (employee: Employee, date: string) => {

  let shift = employee.shfits.find(shift => shift.workday === date);
  if (!shift) {
    shift = { workday: date, starttime: '', endtime: '', breakflg: false };
    employee.shfits.push(shift);
  }
  return shift;
};

onMounted(() => {
  dates.value = getMonthListDates();
});
</script>
