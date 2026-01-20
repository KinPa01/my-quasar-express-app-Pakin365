<template>
  <q-page padding>
    <div class="text-h4 q-mb-md">
      Advanced Full-Stack Demo (Quasar + Express)
    </div>


    <!-- Git Workflow (จากตัวอย่างก่อน) -->
    <q-card class="q-mb-md">
      <q-card-section>
        <div class="text-h6">Git Workflow</div>
        <q-list bordered separator class="q-mt-sm">
          <q-item v-for="(step, index) in gitSteps" :key="index">
            <q-item-section avatar>
              <q-badge>{{ index + 1 }}</q-badge>
            </q-item-section>
            <q-item-section>
              <q-item-label>{{ step.title }}</q-item-label>
              <q-item-label caption>{{ step.detail }}</q-item-label>
            </q-item-section>
          </q-item>
        </q-list>
      </q-card-section>
    </q-card>


    <!-- Docker Concepts (จากตัวอย่างก่อน) -->
    <q-card class="q-mb-md">
      <q-card-section>
        <div class="text-h6">Docker Concepts</div>
        <q-list bordered separator class="q-mt-sm">
          <q-item v-for="(item, index) in dockerItems" :key="index">
            <q-item-section>
              <q-item-label>{{ item.title }}</q-item-label>
              <q-item-label caption>{{ item.detail }}</q-item-label>
            </q-item-section>
          </q-item>
        </q-list>
      </q-card-section>
    </q-card>


    <!-- New: API Data from Backend -->
    <q-card>
      <q-card-section>
        <div class="text-h6">Data from Backend API</div>
        <q-spinner v-if="loading" color="primary" size="2em" />
        <q-list v-else bordered separator class="q-mt-sm">
          <q-item>
            <q-item-section>
              <q-item-label>Advanced Git</q-item-label>
              <q-item-label caption>{{ apiData.git.detail }}</q-item-label>
            </q-item-section>
          </q-item>
          <q-item>
            <q-item-section>
              <q-item-label>Advanced Docker</q-item-label>
              <q-item-label caption>{{ apiData.docker.detail }}</q-item-label>
            </q-item-section>
          </q-item>
        </q-list>
        <q-btn v-if="!loading" color="primary" @click="fetchData">Refresh Data</q-btn>
      </q-card-section>
    </q-card>
  </q-page>
</template>


<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';


// Git workflow steps
const gitSteps = [
  {
    title: 'Feature Branch',
    detail: 'สร้าง branch ใหม่จาก main สำหรับ feature (git checkout -b feature/name)'
  },
  {
    title: 'Commit Changes',
    detail: 'ใช้ Conventional Commits (feat:, fix:, docs:) เพื่อ history ที่ชัดเจน'
  },
  {
    title: 'Push & PR',
    detail: 'Push branch และเปิด Pull Request บน GitHub สำหรับ code review'
  },
  {
    title: 'Code Review',
    detail: 'ทีมรีวิว code, แนะนำการปรับปรุง, ตรวจสอบ tests'
  },
  {
    title: 'Merge',
    detail: 'ใช้ squash merge เพื่อรวม commits และลบ branch หลัง merge'
  }
];

const dockerItems = [
  {
    title: 'Multi-stage Build',
    detail: 'แยก build stage และ production stage เพื่อลดขนาด image'
  },
  {
    title: 'Healthcheck',
    detail: 'ตรวจสอบสถานะ container อัตโนมัติด้วย HEALTHCHECK instruction'
  },
  {
    title: 'Docker Compose',
    detail: 'จัดการหลาย services พร้อม network และ volumes ในไฟล์เดียว'
  },
  {
    title: 'Volumes',
    detail: 'เก็บข้อมูลถาวร (persistent data) เช่น logs, database'
  },
  {
    title: 'Networks',
    detail: 'สร้าง network สำหรับการสื่อสารระหว่าง containers'
  }
];


const apiData = ref({ git: {}, docker: {} });
const loading = ref(true);


const fetchData = async () => {
  loading.value = true;
  try {
    const response = await axios.get(import.meta.env.VITE_API_URL + '/api/demo');
    apiData.value = response.data;
  } catch (error) {
    console.error('API Error:', error);
  } finally {
    loading.value = false;
  }
};


onMounted(fetchData);
</script>
