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

