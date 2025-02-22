<template>
  <div class="container mx-auto p-4">
    <h1 class="text-2xl font-bold mb-4">シフト表</h1>
    <div class="overflow-x-auto">
      <table class="min-w-full border border-gray-300">
        <thead>
          <tr class="bg-gray-200">
            <th class="border px-4 py-2">従業員</th>
            <th v-for="date in dates" :key="date" class="border px-4 py-2">
              {{ date }}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="employee in employees" :key="employee.id">
            <td class="border px-4 py-2">{{ employee.name }}</td>
            <td v-for="date in dates" :key="date" class="border px-4 py-2">
              <input 
                type="text" 
                v-model="shifts[employee.id][date]" 
                class="w-full text-center border rounded p-1">
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

const dates = ref<string[]>([]);
const employees = ref<Employee[]>([
  { id: 1, name: '田中' },
  { id: 2, name: '佐藤' },
  { id: 3, name: '鈴木' }
]);
const shifts = ref<Record<number, Record<string, string>>>({});

const getDates = () => {
  const today = new Date();
  const result: string[] = [];
  for (let i = 0; i < 7; i++) {
    const date = new Date();
    date.setDate(today.getDate() + i);
    result.push(date.toISOString().split('T')[0]);
  }
  return result;
};

const initializeShifts = () => {
  employees.value.forEach(employee => {
    shifts.value[employee.id] = {};
    dates.value.forEach(date => {
      shifts.value[employee.id][date] = '';
    });
  });
};

onMounted(() => {
  dates.value = getDates();
  initializeShifts();
});
</script>
