```
import Vue from "vue";
import Router from "vue-router";

import TopIndex from "../pages/top/index";
import TaskIndex from "../pages/task/index";

Vue.use(Router)

const router = new Router({
  mode: "history",
  routes: [
    {
      path: "/",
      component: TopIndex,
      name: "TopIndex",

    },
    {
      path: "/tasks",
      component: TaskIndex,
      name: "TaskIndex",
    },
  ],
})

export default router
```

vue routerではrouteを指定するための手法としてvuerouterが使用されている

一番上二つの2行はvue.jsを使用し、routeを定義する宣言文

その下二つ目のコードからimport後の文字がどこのファイルに遷移するかの宣言

formの後がurlの指定

Vue.use(Router)でrouterを登録している

その後 constrouterで定義し

routers で一つずつ定義していく

一つのrouteを{}で囲み記述していく

pathはその名の通りurnの部分

componenntはrubyでいうhtml.erb

nameはcomponentを呼び出すときの名前がどのようにして呼び出すかのこと


# vue.jsを使用したmvcのrails　との違い


rails でvue.jsを使用したspaを使用するには
routeに以下の記述が必要

```
Rails.application.routes.draw do
  root 'home#index'
  get '*path', to: 'home#index'
end
```

また対応するhtmlファイルには
```
<%= javascript_pack_tag 'hello_vue' %>
```
こちらの記述が必要


vue.jsを使用する場合には

レイアウトなども

app.vueなどに埋め込み最終的に読み込む形になっている

```
  import Vue from 'vue'
  import App from '../app.vue'
+ import 'bootstrap/dist/css/bootstrap.css'

Vue.config.productionTip = false

document.addEventListener('DOMContentLoaded', () => {
  const app = new Vue({
    render: h => h(App)
  }).$mount()
  document.body.appendChild(app.$el)
})

```

vueファイルは`単一ファイルコンポーネント`(html,js,cssを一つのファイルに組み込んだもの)

```
<template>
<!--HTMLを書く場所 -->  
</template>

<script>
<!--JavaScriptを書く場所 -->
</script>

<style>

<!--CSSを書く場所 -->
</style>
```

### 例

```
<template>
  <div id="app" class="d-flex flex-column min-vh-100">
    <header class= "mb-auto">
      <nav class= "navbar navbar-expand navbar-dark bg-dark justify-content-between">
        <span class= "navbar-brand mb-0 h1">{{ title }}</span>
      </nav>
    </header>
    <div class="text-center">
      <h3>タスクを管理しよう！</h3>
      <div class="mt-4">生活や仕事に関するタスクを見える化して抜け漏れを防ぎましょう。</div>
      <button type="button" class="btn btn-dark mt-5">はじめる</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Top",
  data() {
    return {
      title: "タスク管理"
    }
  }
}
</script>

<style scoped>
 h3{
    color: red;
  }
</style>
```

export defaultはjsをモジュール化し外から呼び出せるようにする

こちらを定義することで中身がvue.jsのファイルの処理になる
