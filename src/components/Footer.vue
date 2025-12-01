<template>
  <footer class="bg-gradient-to-r from-blue-900 to-indigo-900 dark:from-gray-900 dark:to-gray-950 text-white transition-colors duration-300">
    <div class="container mx-auto px-8 py-6">
      <div class="flex flex-col md:flex-row justify-between items-center">
        <div class="flex items-center mb-2 md:mb-0">
          <span class="iconify text-2xl mr-2 text-blue-400" data-icon="mdi:school"></span>
          <span class="font-semibold text-white dark:text-gray-200">智慧选课平台</span>
        </div>
        <div class="flex items-center space-x-6 text-sm">
          <router-link to="/" class="text-gray-300 hover:text-white dark:text-gray-400 dark:hover:text-white transition-colors">首页</router-link>
          <router-link to="/resources" class="text-gray-300 hover:text-white dark:text-gray-400 dark:hover:text-white transition-colors">资源广场</router-link>
          <router-link to="/courses" class="text-gray-300 hover:text-white dark:text-gray-400 dark:hover:text-white transition-colors">全部课程</router-link>
        </div>
        <div class="text-center md:text-right mt-2 md:mt-0">
          <p class="text-gray-400 dark:text-gray-500 text-xs">
            © 2024 智慧选课平台. All rights reserved.
          </p>
        </div>
      </div>
    </div>
  </footer>
</template>

<script>
export default {
  name: 'Footer',
  methods: {
    handleLogoError(event) {
      event.target.style.display = 'none'
    }
  }
}
</script>

<style scoped>
/* Removed hardcoded gradient to allow Tailwind dark mode classes to work */

/* 响应式调整 */
@media (max-width: 768px) {
  footer {
    text-align: center;
  }
}
</style>
