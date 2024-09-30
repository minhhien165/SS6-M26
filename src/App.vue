<template>
  <div>
    <input 
      v-model="valueInput"
      type="text" 
      placeholder="Thêm nội dung công việc"/>
    <button @click="addJob">Thêm</button>

    <div class="list">
      <h1>Danh sách công việc</h1>
      <!-- Hiển thị danh sách công việc -->
      <ul>
        <li v-for="job in jobs" :key="job.id">
          <!-- Checkbox được kiểm tra theo trạng thái của công việc -->
          <input type="checkbox" v-model="job.status" @change="saveToLocalStorage">
          <!-- Áp dụng gạch ngang nếu công việc đã hoàn thành -->
          <span :class="{ completed: job.status }">{{ job.name }}</span>
          <button @click="deleteJob(job.id)">Xóa</button>
        </li>
      </ul>
    </div>

    <h2>Số công việc đã hoàn thành: {{ completeJob }}</h2>
  </div>
</template>

<script setup>
import { computed, reactive, ref } from 'vue'

// Tạo tên công việc
const valueInput = ref('')

// Tạo mảng chứa các công việc
const jobs = reactive(JSON.parse(localStorage.getItem('jobs')) || []);

// Lưu danh sách công việc vào localStorage
const saveToLocalStorage = () => {
  localStorage.setItem('jobs', JSON.stringify(jobs));
}

// Khai báo hàm thêm công việc
const addJob = () => {
  if (valueInput.value.trim()) {
    let job = {
      name: valueInput.value,
      status: false,
      id: Math.ceil(Math.random() * 9888888888888888),
    };
    jobs.push(job);
    saveToLocalStorage();
    valueInput.value = '';
  }
};

// Tính số lượng công việc hoàn thành
const completeJob = computed(() => {
  return jobs.reduce((accumulator, currentValue) => {
    return accumulator + (currentValue.status ? 1 : 0);
  }, 0);
});

// Hàm xóa công việc
const deleteJob = (id) => {
  let index = jobs.findIndex((item) => item.id === id);
  if (index !== -1) {
    jobs.splice(index, 1);
    saveToLocalStorage();
  }
};
</script>

<style>
/* Thêm style cho công việc đã hoàn thành (gạch ngang) */
.completed {
  text-decoration: line-through;
  color: grey;
}
</style>
