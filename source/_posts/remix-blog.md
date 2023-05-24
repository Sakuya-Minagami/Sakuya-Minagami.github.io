---
title: remix-blog
---
remix官方示例
问题都在于前端从数据库访问数据
# 数据流
app/routes/post/index.tsx下
```typescript
import { getPosts } from "~/models/post.server";

export const loader = async () => {
  return json({ posts: await getPosts() });
};

export default function Posts() {
  const { posts } = useLoaderData<typeof loader>();
  //...posts拿到整个对象
}
```
models/post.server.tsx下
```typescript
import { prisma } from "~/db.server";

export async function getPosts() {
    return prisma.post.findMany();
}

//后续部分是原内容 即死数据
type Post = {
  slug: string;
  title: string;
};

export async function getPosts(): Promise<Array<Post>> {
  return [
    {
      slug: "my-first-post",
      title: "My First Post",
    },
    {
      slug: "90s-mixtape",
      title: "A Mixtape I Made Just For You",
    },
  ];
}
```
post是从post.server获取的
db.server下
```typescript
import { PrismaClient } from "@prisma/client";

let prisma: PrismaClient;

declare global {
  var __db__: PrismaClient;
}

if (process.env.NODE_ENV === "production") {
  prisma = new PrismaClient();
} else {
  if (!global.__db__) {
    global.__db__ = new PrismaClient();
  }
  prisma = global.__db__;
  prisma.$connect();
}

export { prisma };

```
导出一个prisma对象
事实上，prisma的键值都装在schema.prisma中，用model关键字定义

# 问题
## await
```
prisma/seed.ts:93:3 - error TS1378: Top-level 'await' expressions are only allowed when the 'module' option is set to 'es2022', 'esnext', 'system', 'node16', or 'nodenext', and the 'target' option is set to 'es2017' or higher.
```
