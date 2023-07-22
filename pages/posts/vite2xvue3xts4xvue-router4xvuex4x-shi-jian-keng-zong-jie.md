---
title: 'vite2.x+vue3.x+ts4.x+vue-router4.x+vuex4.x实践坑总结'
date: 2022-07-14 16:59:03
tags: [web前端]
published: true
hideInList: false
feature: 
isTop: false
---
###1.配置别名alias
```
1.需要在vite.config.ts下面做如下配置
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import path from 'path'

// https://vitejs.dev/config/
export default defineConfig({
  resolve: {
    alias: {
      '@': path.resolve(__dirname, 'src'),
    },
  },
  plugins: [vue()],
})

2.由于使用了ts所以在ts下面也要配置相应的path
{
  "compilerOptions": {
    "target": "esnext",
    "module": "esnext",
    "moduleResolution": "node",
    "strict": true,
    "jsx": "preserve",
    "sourceMap": true,
    "resolveJsonModule": true,
    "esModuleInterop": true,
    "lib": ["esnext", "dom"],
    "paths": {
      "@/*": ["./src/*"]
    }
  },
  "include": ["src/**/*.ts", "src/**/*.d.ts", "src/**/*.tsx", "src/**/*.vue"]
}

3.然后可以在文件中这样引用
import { useCount } from '@/hooks/useCount'
import logo from '@/assets/logo.png'
import test from '@/utils/test'
```
<!-- more -->
