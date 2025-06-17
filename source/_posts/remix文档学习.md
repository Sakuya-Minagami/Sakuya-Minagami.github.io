---
title: remix文档学习
---
尝试从项目反向学习文档，内容也主要来自文档

# Hook
## useActionData
用于返回解析的JSON数据
```tsx
import type { ActionArgs } from "@remix-run/node"; 
import { json } from "@remix-run/node"; 
import { Form, useActionData } from "@remix-run/react";
// 请求数据过程
export async function action({ request }: ActionArgs) {
  const body = await request.formData();
  const name = body.get("visitorsName");
  return json({ message: `Hello, ${name}` });
}

export default function Invoices() {
  const data = useActionData<typeof action>();
  return (
    <Form method="post">
      <p>
        <label>
          What is your name?
          <input type="text" name="visitorsName" />
        </label>
      </p>
      <p>{data ? data.message : "Waiting..."}</p>
    </Form>
  );
}
```
具体用法：验证表单错误