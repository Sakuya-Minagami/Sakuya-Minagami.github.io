---
title: remix-jokes
---
官方更长和完善的示例
前端框架部分不多余赘述
# prisma
```shell
npm install --save-dev prisma
npm install @prisma/client
npx prisma init --datasource-provider sqlite
npm install --save-dev esbuild-register
```
在prisma/schema.prisma末尾配置
```
model Joke {
  id         String   @id @default(uuid())
  createdAt  DateTime @default(now())
  updatedAt  DateTime @updatedAt
  name       String
  content    String
}
```
运行``npx prisma db push``
## seed
作prisma/seed.ts
```typescript
import { PrismaClient } from "@prisma/client";
const db = new PrismaClient();

async function seed() {
  await Promise.all(
    getJokes().map((joke) => {
      return db.joke.create({ data: joke });
    })
  );
}

seed();

function getJokes() {
	return [
    {
      name: "Road worker",
      content: `I never wanted to believe that my Dad was stealing from his job as a road worker. But when I got home, all the signs were there.`,
    },
//...
	]
}
```
执行``node --require esbuild-register prisma/seed.ts``
并在package.json记住脚本指令
```
// ...
  "prisma": {
    "seed": "node --require esbuild-register prisma/seed.ts"
  },
  "scripts": {
// ...
```
### 数据库连接
prisma/seed/ts下
```
import { PrismaClient } from "@prisma/client";
const db = new PrismaClient();
```
作app/utils/db.server.ts
```typescript
import { PrismaClient } from "@prisma/client";

let db: PrismaClient;

declare global {
  var __db: PrismaClient | undefined;
}

if (process.env.NODE_ENV === "production") {
  db = new PrismaClient();
} else {
  if (!global.__db) {
    global.__db = new PrismaClient();
  }
  db = global.__db;
}

export { db };
```

### 表单处理
