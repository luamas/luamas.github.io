---
title: Angular4学习笔记(九) Angular4 路由
layout: post
date: '2017-03-27 00:00:00'
categories: angular2,angular4
tags: angular2,angular4
author: luamas
original: true
---

* content
{:toc}



这回直接进入4.x时代学习,其实和2.x没啥区别,具体升级步骤可以查看下面链接,或者直接克隆源代码

<{{site.url}}/2017/03/26/angular4-angular2-upgrade>




下面我们重新创建一个分支来演示

```shell
ng new angular2-sample
```

安装bootstrap
```shell
npm install bootstrap --save
```

安装字体图标库
```
npm install font-awesome
```

引用bootstrap
```
"styles": [
        "../node_modules/bootstrap/dist/css/bootstrap.min.css",
        "../node_modules/font-awesome/css/font-awesome.css",
        "styles.css"
      ],
```

```shell
// 创建layout模型
ng g m layout
// 创建layout(布局)组件
ng g c layout
// 创建header(头部)组件
ng g c layout/header
// 创建footer(尾部)组件
ng g c layout/footer
// 创建sidebar(侧边栏)组件
ng g c layout/sidebar
// 创建routes模型
ng g m routes
// 创建home模型
ng g m routes/home
// 创建首页组件
ng g c routes/home/home
// 创建路由类
ng g cl routes/routes
```

**注意使用路由的模块千万别忘记导入`RouterModule`模块,自己在这里卡了好久才找到,本以为布局中没有用到,不需要单独import,最后找到原来是这里的问题**


### 路由的使用

routes.ts

```ts
export const routes = [

  {
    path: '',
    component: LayoutComponent,
    children: [
      { path: '', redirectTo: 'home', pathMatch: 'full' },
      { path: 'home', loadChildren: 'app/routes/home/home.module#HomeModule' },
      { path: 'input', loadChildren: 'app/routes/input/input.module#InputModule' },
      { path: 'myform', loadChildren: 'app/routes/myform/myform.module#MyformModule' }
    ]
  },

  // Not found
  { path: '**', redirectTo: 'home' }

];
```

routes.module.ts

```ts
imports: [
     CommonModule,
     RouterModule.forRoot(routes)
   ],
```

使用页面

```
<router-outlet></router-outlet>
```

链接方式
```
<a class="dropdown-item" [routerLink]="['/input/keyup']" [routerLinkActive]="['active']" class="list-group-item">
        <i class="fa fa-fw fa-dashboard"></i> 键盘事件1
      </a>
```

上面的`routerLink`代表地址,`routerLinkActive`代表如果状态生效则会附加`active`的css样式

这节基本没啥太多的东西,主要是要组织好结构,至于手动导航页面直接取官网例子看下

```
// router直接注入router对象即可,注意要import下
this.router.navigate(['/hero', hero.id]);
```

具体查看下代码,不得不说自己不是搞前端的,哎,被css的东西把自己搞的整天都没啥心情.


[路由中文官方文档](https://angular.cn/docs/ts/latest/guide/router.html)

[路由英文官方文档](https://angular.io/docs/ts/latest/guide/router.html)

<br>

[查看源代码](https://github.com/luamas/angular2-sample/tree/v3)

[演示地址](http://blog.luamas.com/angular2-sample)


