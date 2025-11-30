<template>
  <div class="container mx-auto px-6 py-8">
    <div class="flex items-center mb-6">
      <button @click="$router.back()" class="mr-4 p-2 hover:bg-gray-100 rounded-full">
        <span class="iconify text-xl" data-icon="mdi:arrow-left"></span>
      </button>
      <h1 class="text-2xl font-bold text-gray-800">我的收藏</h1>
    </div>

    <!-- Tabs -->
    <div class="bg-white rounded-lg shadow-sm mb-6">
      <div class="flex border-b border-gray-200">
        <button 
          @click="activeTab = 'courses'"
          :class="['px-6 py-3 font-medium focus:outline-none', activeTab === 'courses' ? 'text-blue-600 border-b-2 border-blue-600' : 'text-gray-500 hover:text-gray-700']"
        >
          收藏的课程
        </button>
        <button 
          @click="activeTab = 'resources'"
          :class="['px-6 py-3 font-medium focus:outline-none', activeTab === 'resources' ? 'text-blue-600 border-b-2 border-blue-600' : 'text-gray-500 hover:text-gray-700']"
        >
          收藏的资源
        </button>
      </div>
    </div>

    <!-- Content -->
    <div v-if="activeTab === 'courses'">
      <div v-if="favoriteCourses.length > 0" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
        <div v-for="course in favoriteCourses" :key="course.id" class="bg-white rounded-lg shadow-sm overflow-hidden hover:shadow-md transition-shadow cursor-pointer" @click="goToCourse(course.id)">
          <div class="h-40 bg-gray-200 relative">
            <img :src="course.image || 'https://via.placeholder.com/400x200'" class="w-full h-full object-cover" alt="Course cover">
            <div class="absolute top-2 right-2 bg-white p-1 rounded-full shadow cursor-pointer hover:bg-red-50" @click.stop="removeFavoriteCourse(course.id)">
               <span class="iconify text-red-500" data-icon="mdi:heart"></span>
            </div>
          </div>
          <div class="p-4">
            <h3 class="font-bold text-lg mb-1 truncate">{{ course.title }}</h3>
            <p class="text-sm text-gray-500 mb-2">{{ course.college }}</p>
            <div class="flex justify-between items-center mt-4">
              <span class="text-blue-600 text-sm font-medium hover:underline">查看详情</span>
            </div>
          </div>
        </div>
      </div>
      <div v-else class="text-center py-12 bg-white rounded-lg shadow-sm">
        <span class="iconify text-6xl text-gray-300 mb-4 mx-auto block" data-icon="mdi:book-open-page-variant"></span>
        <p class="text-gray-500">您还没有收藏任何课程</p>
        <router-link to="/courses" class="mt-4 inline-block text-blue-600 hover:underline">去浏览课程</router-link>
      </div>
    </div>

    <div v-if="activeTab === 'resources'">
      <div v-if="favoriteResources.length > 0" class="bg-white rounded-lg shadow-sm overflow-hidden">
        <div v-for="(resource, index) in favoriteResources" :key="resource.id" :class="['p-4 flex items-center hover:bg-gray-50 cursor-pointer', index !== favoriteResources.length - 1 ? 'border-b border-gray-100' : '']" @click="goToResource(resource.id)">
          <div class="bg-blue-100 p-3 rounded mr-4">
             <span class="iconify text-blue-600 text-xl" :data-icon="getFileIcon(resource.type)"></span>
          </div>
          <div class="flex-1">
            <h4 class="font-bold text-gray-800 hover:text-blue-600">{{ resource.title }}</h4>
            <p class="text-sm text-gray-500">{{ resource.course }} · {{ resource.size }} · {{ resource.date }}</p>
          </div>
          <div class="flex items-center" @click.stop>
             <button class="text-gray-400 hover:text-red-500 mr-4" @click="removeFavoriteResource(resource.id)" title="取消收藏">
               <span class="iconify text-xl" data-icon="mdi:heart"></span>
             </button>
             <button class="text-blue-600 hover:text-blue-800">
               <span class="iconify text-xl" data-icon="mdi:download"></span>
             </button>
          </div>
        </div>
      </div>
      <div v-else class="text-center py-12 bg-white rounded-lg shadow-sm">
        <span class="iconify text-6xl text-gray-300 mb-4 mx-auto block" data-icon="mdi:file-document-multiple"></span>
        <p class="text-gray-500">您还没有收藏任何资源</p>
        <router-link to="/resources" class="mt-4 inline-block text-blue-600 hover:underline">去浏览资源</router-link>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MyFavorites',
  data() {
    return {
      activeTab: 'courses',
      // Mock 数据，稍后替换为 Store 或 API 调用
      favoriteCourses: [
        { id: 1, title: '数据结构与算法', college: '计算机学院', image: '' },
        { id: 2, title: '操作系统原理', college: '计算机学院', image: '' }
      ],
      favoriteResources: [
        { id: 101, title: '期末复习重点总结', course: '高等数学', type: 'pdf', size: '2.4MB', date: '2023-06-10' },
        { id: 102, title: '实验报告模板', course: '大学物理', type: 'doc', size: '500KB', date: '2023-05-20' }
      ]
    }
  },
  methods: {
    getFileIcon(type) {
      const icons = {
        pdf: 'mdi:file-pdf-box',
        doc: 'mdi:file-word-box',
        docx: 'mdi:file-word-box',
        ppt: 'mdi:file-powerpoint-box',
        zip: 'mdi:zip-box',
        default: 'mdi:file'
      }
      return icons[type] || icons.default
    },
    goToCourse(id) {
      this.$router.push(`/course/${id}`)
    },
    goToResource(id) {
      // 假设资源详情页通过 query 参数传递 ID
      this.$router.push({ path: '/resource', query: { id } })
    },
    removeFavoriteCourse(id) {
      if(confirm('确定取消收藏该课程吗？')) {
        this.favoriteCourses = this.favoriteCourses.filter(c => c.id !== id)
        // TODO: Call API to remove favorite
      }
    },
    removeFavoriteResource(id) {
      if(confirm('确定取消收藏该资源吗？')) {
        this.favoriteResources = this.favoriteResources.filter(r => r.id !== id)
        // TODO: Call API to remove favorite
      }
    }
  }
}
</script>