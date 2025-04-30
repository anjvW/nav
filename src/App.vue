<script setup>
import { ref, computed, onMounted } from 'vue'
import axios from 'axios'
import { Search } from '@element-plus/icons-vue'

const categories = ref([])
const searchQuery = ref('')

const loadNavData = async () => {
  try {
    const response = await axios.get('/navdata.json')
    categories.value = response.data.categories
  } catch (error) {
    console.error('加载导航数据失败:', error)
  }
}

const filteredCategories = computed(() => {
  if (!searchQuery.value) return categories.value
  
  return categories.value.map(category => ({
    ...category,
    sites: category.sites.filter(site => 
      site.name.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
      site.description.toLowerCase().includes(searchQuery.value.toLowerCase())
    )
  })).filter(category => category.sites.length > 0)
})

const openSite = (url) => {
  window.open(url, '_blank')
}

onMounted(() => {
  loadNavData()
})
</script>

<template>
  <div class="app-container">
    <el-container>
      <el-header class="header">
        <div class="header-content">
          <div>
            <h1 class="title">ANJV·未来导航中心</h1>
            <div class="subtitle">Empowering Your Digital World</div>
          </div>
          <div class="search-box">
            <el-input
              v-model="searchQuery"
              placeholder="搜索网站..."
              :prefix-icon="Search"
              clearable
              @input="filterSites"
            />
          </div>
        </div>
      </el-header>
      <el-main>
        <div class="category-container">
          <el-card 
            v-for="category in filteredCategories" 
            :key="category.name" 
            class="category-card"
            :class="{ 'has-search-results': searchQuery }"
          >
            <template #header>
              <div class="category-header">
                <h2>{{ category.name }}</h2>
                <span class="site-count">{{ category.sites.length }}个网站</span>
              </div>
            </template>
            <div class="sites-container">
              <el-card 
                v-for="site in category.sites" 
                :key="site.name" 
                class="site-card" 
                @click="openSite(site.url)"
                shadow="hover"
              >
                <div class="site-content">
                  <div class="site-icon-wrapper">
                    <img :src="site.icon" :alt="site.name" class="site-icon" />
                  </div>
                  <div class="site-info">
                    <h3>{{ site.name }}</h3>
                    <p>{{ site.description }}</p>
                  </div>
                </div>
              </el-card>
            </div>
          </el-card>
        </div>
      </el-main>
    </el-container>
  </div>
</template>

<style scoped>
html, body, #app {
  width: 100%;
  overflow-x: hidden;
  background: #101522;
}

.app-container {
  width: 100%;
  min-height: 100vh;
  background: linear-gradient(135deg, #101522 0%, #1a233a 100%);
  box-sizing: border-box;
  overflow-x: hidden;
}

.header {
  width: 100%;
  background: rgba(20, 30, 50, 0.85);
  backdrop-filter: blur(10px);
  box-shadow: 0 2px 20px 0 #0ff2ff33;
  position: sticky;
  top: 0;
  z-index: 100;
  border-bottom: 1.5px solid #0ff2ff55;
}

.header-content {
  width: 100%;
  max-width: 1400px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px 20px;
  box-sizing: border-box;
}

.title {
  margin: 0;
  font-size: clamp(1.5rem, 4vw, 2.2rem);
  font-weight: 700;
  background: linear-gradient(90deg, #00eaff 20%, #00bfff 80%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  text-shadow: 0 0 8px #00eaff99, 0 0 2px #fff;
  letter-spacing: 2px;
}

.subtitle {
  font-size: 1.1rem;
  color: #7eefff;
  letter-spacing: 2px;
  margin-top: 2px;
  margin-bottom: 0;
  text-shadow: 0 0 8px #00eaff66, 0 0 2px #fff;
  font-family: 'Orbitron', 'Segoe UI', Arial, sans-serif;
}

.search-box {
  width: clamp(200px, 30vw, 300px);
}

:deep(.el-input__wrapper) {
  background: rgba(20, 30, 50, 0.8) !important;
  border: 1.5px solid #00eaff !important;
  box-shadow: 0 0 8px #00eaff55;
}
:deep(.el-input__inner) {
  color: #fff !important;
}

.category-container {
  width: 100%;
  max-width: 1400px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
  gap: 24px;
  padding: 24px;
  box-sizing: border-box;
}

.category-card {
  width: 100%;
  background: rgba(20, 30, 50, 0.85);
  border-radius: 16px;
  border: 1.5px solid #00eaff55;
  box-shadow: 0 0 24px #00eaff22, 0 2px 16px #000a;
  transition: all 0.3s cubic-bezier(.4,2,.6,1);
  box-sizing: border-box;
  backdrop-filter: blur(6px);
}
.category-card:hover {
  box-shadow: 0 0 32px #00eaff99, 0 2px 24px #000c;
  border-color: #00eaff;
}

.category-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 15px;
}

.category-header h2 {
  margin: 0;
  color: #00eaff;
  font-size: clamp(1.2rem, 3vw, 1.5rem);
  font-weight: 600;
  letter-spacing: 1px;
  text-shadow: 0 0 8px #00eaff99;
}

.site-count {
  color: #7eefff;
  font-size: clamp(0.8rem, 2vw, 0.9rem);
  text-shadow: 0 0 4px #00eaff55;
}

.sites-container {
  width: 100%;
  display: grid;
  grid-template-columns: 1fr;
  gap: 18px;
  padding: 18px;
  box-sizing: border-box;
}

.site-card {
  width: 100%;
  background: rgba(30, 40, 60, 0.92);
  border-radius: 10px;
  border: 1.5px solid #00eaff33;
  box-shadow: 0 0 12px #00eaff22;
  transition: all 0.3s cubic-bezier(.4,2,.6,1);
  cursor: pointer;
  box-sizing: border-box;
}
.site-card:hover {
  border-color: #00eaff;
  box-shadow: 0 0 24px #00eaff99, 0 2px 12px #000c;
  background: rgba(30, 40, 60, 0.98);
}

.site-content {
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 16px;
}

.site-icon-wrapper {
  flex-shrink: 0;
  width: clamp(38px, 5vw, 44px);
  height: clamp(38px, 5vw, 44px);
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(0, 234, 255, 0.10);
  border-radius: 10px;
  box-shadow: 0 0 12px #00eaff55;
  padding: 6px;
}

.site-icon {
  width: 100%;
  height: 100%;
  object-fit: contain;
}

.site-info {
  flex: 1;
  min-width: 0;
}

.site-info h3 {
  margin: 0;
  font-size: clamp(0.95rem, 2vw, 1.1rem);
  color: #00eaff;
  font-weight: 600;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  text-shadow: 0 0 6px #00eaff99;
}

.site-info p {
  margin: 5px 0 0;
  font-size: clamp(0.78rem, 1.8vw, 0.92rem);
  color: #b2eaff;
  line-height: 1.4;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  text-shadow: 0 0 4px #00eaff33;
}

.has-search-results {
  background: rgba(255, 255, 255, 0.95);
}

@media (max-width: 1024px) {
  .category-container {
    grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
    padding: 15px;
  }
}

@media (max-width: 768px) {
  .header-content {
    flex-direction: column;
    gap: 15px;
    padding: 15px;
  }
  .search-box {
    width: 100%;
  }
  .category-container {
    grid-template-columns: 1fr;
    padding: 10px;
  }
  .sites-container {
    grid-template-columns: 1fr;
    padding: 10px;
  }
}

@media (max-width: 480px) {
  .header-content {
    padding: 10px;
  }
  .category-container {
    padding: 5px;
    gap: 10px;
  }
  .sites-container {
    gap: 8px;
    padding: 5px;
  }
  .site-content {
    padding: 8px;
  }
}
</style>
