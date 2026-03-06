<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const tasks = ref([])
const loading = ref(false)
const errorMessage = ref('')

const API_URL = 'https://exam-313.onrender.com'

const fetchTasks = async () => {
  loading.value = true
  errorMessage.value = ''

  try {
    const res = await axios.get(`${API_URL}/tasks`)
    tasks.value = res.data.data
  } catch (err) {
    console.error('API Error:', err)
    const status = err.response ? err.response.status : 'Network Error'
    errorMessage.value = `โหลดงานไม่สำเร็จ (${status})`
  } finally {
    loading.value = false
  }
}

const formatDate = (dateStr) => {
  return new Date(dateStr).toLocaleString('th-TH', {
    year: 'numeric',
    month: 'long',
    day: 'numeric',
    hour: '2-digit',
    minute: '2-digit',
    second: '2-digit'
  })
}

onMounted(fetchTasks)
</script>
