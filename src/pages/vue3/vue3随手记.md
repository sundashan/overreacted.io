---
title: vue3随手记
date: 2021-08-12 21:40:17
spoiler: 省略.
cta: 'vue'
---

vue3出来好久了，项目一直没更新，这几天试了试，这边零碎的记一下
nextTick改成：
```jsx
import { nextTick } from 'vue'
nextTick(() => {
    ...
})
```

```jsx
data created  =>  setup

onMounted
示例：

this.$router  useRouter()
this.$route   useRoute()

记得return里面导出

return {
    ...toRefs(state)
}
```

ref this.$refs 改成  
```jsx 
const divref = ref(null)
console.log(divref.value)
```

watch
```jsx
watch(() => return state.count, (count, preCount) => {})
```

vue-router 创建 滑动 mode base都有改变
```jsx
import { createRouter, createWebHistory } from 'vue-router'
const router = VueRouter.createRouter({
  history: VueRouter.createWebHashHistory(), // mode改成history
  <!-- history: VueRouter.createWebHistory('/base-directory/'), // base被删除，作为createWebHistory的参数传递 -->
  routes,
  scrollBehavior (to, from, savedPosition) {
    // return 期望滚动到哪个的位置
    x,y改成left,top
  }
})
```

i18
安装后创建一个i18.js文件
```jsx
import { createI18n } from 'vue-i18n' //引入vue-i18n组件
import messages from './index'
const language = (
  (navigator.language ? navigator.language : navigator.userLanguage) || "zh"
).toLowerCase();
const i18n = createI18n({
  fallbackLocale: 'ch',
  globalInjection:true, // 重点
  legacy: false, // you must specify 'legacy: false' option
  locale: language.split("-")[0] || "zh",
  messages,
});

export default i18n
```

```jsx
<script>
import { getCurrentInstance } from "vue";
import { useI18n } from 'vue-i18n'//要在js中使用国际化

export default {
  name: "App",
  setup() {
    const { proxy } = getCurrentInstance();
    function change(type) {
      proxy.$i18n.locale = type;
    }
　　　　const { t } = useI18n()
　　　　console.log(t('message.pagesDes'))
return { change };
  },
};
</script>
```