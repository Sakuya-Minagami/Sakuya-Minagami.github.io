---
title: typescript
---

安装
``npm i -g typescript``
编译
`` tsc --init``会产生tsconfig.json
`` tsc``会产生ts文件
`` tsc -w``持续在文件保存时即编译

断言：
声明变量时，因为变量源可能未定义，因此变量可能有另一种类型即空，编译器会警告
在变量声明后面加as 变量类型，表示一定能获取到，消除警告
``const button = document.querySelector('button') as HTMLButtonElement;``
也可以在声明变量时 |null

interface接口：
```typescript
interface CatType {
    id: string;
    url: string;
}

class Cat implements CatType {
    id: string;
    url: string;
  
    constructor(id:string,url:string) {
        this.id = id;
        this.url = url;
    }
}
```
