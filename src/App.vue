<template>
  <div class="container">
    <div class="card-container">
      <h2 class="title">預約管理系統</h2>
      <el-table 
        :data="reservations" 
        border 
        class="table"
        :stripe="true"
        :highlight-current-row="true">
        <el-table-column prop="reservation_id" label="預約編號" min-width="100" />
        <el-table-column prop="student_id" label="學生編號" min-width="100" />
        <el-table-column prop="seat_id" label="座位編號" min-width="100" />
        <el-table-column prop="timeslot_id" label="時段編號" min-width="100" />
        <el-table-column prop="create_time" label="創建時間" min-width="160" />
      </el-table>
      <div class="loading-container" v-if="loading">
        <el-loading :visible="loading" text="載入中..." />
      </div>
      <div class="pagination-container" v-if="reservations.length > 0">
        <el-pagination
          background
          layout="prev, pager, next"
          :total="100"
          :page-size="10"
        />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue';
import type { Reservation } from './interfaces/Reservations';
import { asyncGet } from './utils/fetch';
import { apis } from './enum/api';

const reservations = ref<Array<Reservation>>([]);
const loading = ref(true);

onMounted(() => {
  asyncGet(apis.test)
    .then((resp: Array<Reservation>) => {
      reservations.value = resp;
    })
    .catch((error) => {
      console.error('獲取預約數據失敗:', error);
    })
    .finally(() => {
      loading.value = false;
    });
});
</script>

<style scoped lang="scss">
.container {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2rem;
  box-sizing: border-box;
  background-color: #f5f7fa;
  
  .card-container {
    width: 90%;
    max-width: 1200px;
    background-color: #ffffff;
    border-radius: 8px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    padding: 2rem;
    position: relative;
    
    .title {
      font-size: 1.8rem;
      color: #303133;
      margin-bottom: 1.5rem;
      text-align: left;
      font-weight: 600;
      position: relative;
      padding-left: 1rem;
      
      &:before {
        content: '';
        position: absolute;
        left: 0;
        top: 0;
        height: 100%;
        width: 4px;
        background-color: #409eff;
        border-radius: 2px;
      }
    }
    
    .table {
      width: 100%;
      border-radius: 4px;
      overflow: hidden;
    }
    
    .loading-container {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 200px;
    }
    
    .pagination-container {
      margin-top: 1.5rem;
      display: flex;
      justify-content: flex-end;
    }
  }
}

:deep(.el-table) {
  --el-table-header-bg-color: #f5f7fa;
  --el-table-header-text-color: #606266;
  --el-table-row-hover-bg-color: #ecf5ff;
  
  th {
    font-weight: 600;
  }
}
</style>