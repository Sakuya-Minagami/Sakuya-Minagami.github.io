---
title: hitfp
---
记录咱在组里的学习进度
# 10-3
想想真是丢人呢，连GitHub都不熟
发一个仓库过来，布置写一个登录界面的任务
居然一时愣住不知道要干什么

现在理理思路哦
首先做一点GitHub工作流复习
然后把克隆的仓库跑看看，知道了项目的运行方式

app文件夹应该就是源码了，一个player一个organizer文件夹，下面一堆tsx文件
搜一下，发现是typescripe语言，启动的整个项目是remix
那么要学的就是这两方面的基础了
# 10-5
昨天搞定了react路由系统，redux暂时放一放吧
想想项目需求，要写一个登录界面
那么要做的就是在主页面选择开发者或玩家后，进入首页
检测状态，未登录则显示登录组件，登录则显示个人信息
不过有个问题就是是否支持游客状态访问其他组件，也就是是否需要路由守卫呢？

还有就是哦，看了看项目还用了bootstrap，学！
不过那之前是否需要补一补less和scss基础呢？

哎呀这个组长，名字念错了，daisyui念成daysiui了，找半天找不到
[daisyui官网](https://daisyui.com/)
# 10-6
今天弄gitlab花了很多时间。。麻烦死人家了
好好学一下Git吧
明天就要开会安排下一次任务了，自己的项目是否算上手了呢？
我觉得完全不算吧，还一片空白呢
# 10-8
想一想剩下的房间安排
两个首页都写成，头像，昵称，个人简介吧
房间列表应该两边一样卡片

player这边，练习，相当于自己发起比赛？
比赛记录就列个表格，存比赛类型，日期，结果，回放（？），发起者

organizer这边，发起比赛，应该只是向服务器发起请求，所以只是提交几个建房数据？比赛名+文档？
上传游戏，给个接口加几句话
历史记录，应该跟比赛记录差不多
# 10-9
组长说，房间列表用于观战，正确的入口在参加比赛栏
那么这块就显示，选手的可选比赛项目

再想一下练习，可以个人发起比赛，那么就提供三个难度的官方ai

想一想情景，发起人可以提前上传游戏，那么发起比赛就是已上传游戏的列表，包括已发起和未发起，还应显示发起日期和结束日期

player参加比赛，有点疑惑，怎么让两位选手对接呢？
比方说自己有一个程序管理库，可以选择是否让程序出战
然后可以在参加比赛界面寻找别人的程序挑战
# 10-12
今天看了一下上传游戏的界面，游戏还有封面信息，那就都要带上
# 11-18
记一下prisma的指令
·``npx prisma generate``或者``npx priama db push``

**2023**

# 3-21
第一次有了正式的任务了，写个人界面的功能，个人资料的展示和编辑
## 怎么加载用户数据
```typescript
import type {LoaderFunction} from "@remix-run/node";
import {json} from "@remix-run/node";
import {getUser, requireUserId} from "~/utils/auth.server";
import {useLoaderData} from "@remix-run/react";

export const loader: LoaderFunction = async ({request}) => {
    await requireUserId(request);
    const user = await getUser(request);
    //console.log(user)
    return json({userName: user?.userName, email: user?.email}, {status: 200});
};

export default function Index() {
    const loaderData = useLoaderData();
    //console.log(loaderData)
    return (
        <div>
            <div>{loaderData.email}</div>
        </div>
    )
}

```
上面的方法可以层层解析到用户数据，使用loaderData.email表示
loaderData由useLoaderData返回得到，但是上面怎么解析出来的不清楚
可以修改json内容决定返回给的loaderData数据

user对象只有三个键值对：id，email，userName
*因为user是getUser的返回值*
其实user作为全局变量比较好，之后还得加一个头像，也得访问到
总结地说，获取数据的途经是
1.引入封装在auth.server.ts的获取数据函数
2.引入json，useLoaderData作为工具函数
3.加载loader函数，执行获取函数，以json格式返回数据
4.在页面函数中将useLoaderData返回给loaderData对象

## 获取用户数据
从上面的获取过程可以看到，加载一定会通过loader函数封装
实际上因为服务端渲染的缘故，我们需要手写查询函数
```typescript
// auth.server.ts
// 获取用户id
export async function getUserId(request: Request) {
    const session = await getUserSession(request);
    const userId = session.get('userId');
    if (!userId || typeof userId !== 'string') return null;
    return userId;
}

// 获取用户信息
export async function getUser(request: Request) {
    const userId = await getUserId(request);
    if (typeof userId !== 'string') return null;

    try {
        return await prisma.user.findFirst({
            where: {id: userId, delFlag: false},
            select: {id: true, email: true, userName: true},
        });
    } catch {
        return logout(request);
    }
}
```
前者是通过session获取id，后者是通过id访问该id对应的user字段
```
//schema.pirsma
model User {
  id          String     @id @default(uuid()) @map("_id") @db.Uuid
  email       String     @unique
  userName    String     @map("user_name")
  name        String
  password    String
  status      Role       @default(PLAYER) //用户权限等级
  avatarUrl   String     @map("avatar_url")
  createdTime DateTime   @default(now()) @map("create_time")
  updatedTime DateTime   @default(now()) @updatedAt @map("update_time")
  delFlag     Boolean    @default(false) @map("del_flag")
  GameFile    GameFile[]
  Room        Room[]
}
```
也就是说，我们能访问到的每个user下，都有对应字段
在``where: {id: userId, delFlag: false}``和``select: {id: true, email: true, userName: true}``中
前者表示查询条件，后者表示返回内容
从获取数据的过程中``const user = await getUser(request)``，直接console.log(user)，
会返回id，email，userName，返回内容由select决定
能返回的json内容也因此被限制
``return json({userName: user?.userName, email: user?.email})``
**所以当想要访问user下的特定数据时，都需要专门写查询函数**
*顺便一提，最好用findFirst方法，用findMany方法返回一个数组，需要加下标[0]*
# 3-27
提一嘴，头像功能上传时候是文件，访问时候是user下的avatarUrl字段，用接口访问图片
## 怎么上传数据到服务端
以上传游戏为例
### 设置表单
```tsx
// uploadgame.tsx
// 这些是封装好的表单格式
import {FormField} from "~/components/form/form-field";
import {UploadField} from "~/components/form/upload-field";
import {SelectField} from "~/components/form/select-field";
<Form
                    method="post"
                    encType="multipart/form-data"
                    className="card-body items-center"
                    ref={formRef}
                >
                    <UploadField
                        htmlFor="cover"
                        label="游戏封面"
                        value={formData.cover}
                        className="file-input file-input-bordered w-96"
                        onChange={e => handleInputChange(e, 'cover')}
                    />
                    <div className='w-96 text-red-500'>{ErrorMsg}</div>
                    <div className='space-x-8'>
                        <button type='submit' className='btn w-44'>上传</button>
                        <button type='reset' className='btn w-44' onClick={() => {
                            setFormData({
                                cover: ''
                            });
                            formRef.current?.reset();
                            setErrMsg("");
                        }}>清空
                        </button>
                    </div>
                </Form>
```
这是在前端显示的表单界面
补充一下表单原型，不太重要
```tsx
// upload-field.tsx
export function UploadField({
                                htmlFor,
                                label,
                                value,
                                className,
                                onChange = () => {
                                },
                            }: UploadFieldProps) {
    const ref = useRef<HTMLInputElement>(null);

    return (
        <>
            <div className="form-control justify-center">
                <label htmlFor={htmlFor} className='label'>
                    <span className="label-text">{label}</span>
                </label>
                <input
                    type="file"
                    id={htmlFor}
                    name={htmlFor}
                    value={value}
                    ref={ref}
                    onChange={onChange}
                    className={className}
                />
            </div>
        </>
    );
}
```
### 表单逻辑
表单中有两个关键信息：``formData.cover``和``onChange={e => handleInputChange(e, 'cover')``
前者表示数据传递，后者表示变化操作
```ts
export default function Uploadgame() {
	const loaderData = useLoaderData();
    const [formData, setFormData] = useState({
        cover: '',
        name: '',
        file: '',
        doc: '',
        environment: 'placeholder',
    });
    
    const handleInputChange = (event: React.ChangeEvent<HTMLInputElement>, field: string) => {
        setFormData(form => ({...form, [field]: event.target.value}));
    };
    
    const formRef = useRef<HTMLFormElement>(null);
    const actionData = useActionData();
    const [ErrorMsg, setErrMsg] = useState("");
    // ...
}
```
准备将数据分离出来
```tsx
export const action: ActionFunction = async ({request}) => {
	const formData = await unstable_parseMultipartFormData(request, uploadHandler); // 绑定处理器

    // 从表单中获取数据
    let name = formData.get("name") as string;
    const environment_f = formData.get("environment");
    let coverUrl = formData.get("cover") as any;// 文件类型会获取到文件存储的地址
    let fileUrl = formData.get("file") as any;
    let docUrl = formData.get("doc") as any;
    const userId = await requireUserId(request);

    fileUrl = fileUrl?.filepath as string;
    coverUrl = coverUrl?.filepath as string;
    docUrl = docUrl?.filepath as string;
}
```
### 数据库可视化
终端执行指令``npx prisma studio``，在浏览器生成可视化数据库，可以用于调试所上传的文件
### 回顾一下业务逻辑
在数据库里我们可以看到，总共有五个models，每个在schema.prisma中都有对应
并且user中的字段，gamefile和room，也会连接到对应的model类

# 3-29
感觉碰到了很多难点
本地服务器只能上传本地的路径，那头像功能只能做成开发者访问自己本地图片作为头像了吗
上传游戏文件的程序大概没问题，但是之后上传头像，还要转url，不知道服务器那边要怎么安排
