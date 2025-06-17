---
title: HITFP杂问
---
# 表单提交的流程
## 表单封装
[remix表单](https://www.jb51.net/article/278904.htm)
### 模板表单
```tsx
import {useRef} from "react";

// 此处封装了一个上传文件的组件，用于上传文件
interface UploadFieldProps { // 上传文件的属性
    htmlFor: string; // 上传文件的id
    label: string; // 上传文件的标签
    value?: any; // 上传文件的值
    className?: string; // 上传文件的样式
    onChange?: (...args: any) => any; // 上传文件的回调事件
}

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
### 表单实例
```tsx
<UploadField
                        htmlFor="cover"
                        label="上传头像"
                        value={formData.cover}
                        className="file-input file-input-bordered w-96"
                        onChange={e => handleInputChange(e, 'cover')}
                    />
```

## 表单处理
暂略
## 表单提交
表单处理完数据后，封装好game对象，调用addGameFile方法
```ts
//上传游戏文件
export async function addGameFile(game: GameFileForm) {
    const newGameFile = await createGameFile(game);
    if (!newGameFile) {
        return json({
                error: "上传失败",
            },
            {status: 400},
        );
    }
    return json({}, {status: 200});
}

//创建游戏文件
export const createGameFile = async (game: GameFileForm) => {
    const dict = await prisma.dataDictionary.findFirst({
        where: {
            dictType: "game_environment",
            dictValue: game.environment,
        }
    });
    const newGameFile = await prisma.gameFile.create({
        data: {
            name: game.name,
            environment: game.environment,
            dictId: dict!.id,
            coverPath: game.coverUrl,
            filePath: game.fileUrl,
            docPath: game.docUrl,
            userId: game.userId,
        },
    });
    return {id: newGameFile.id, name: newGameFile.name};
}
```
# 数据访问
终端执行``npx prisma studio``，数据库可视化，可以看见上传的游戏文件
**大部分都写在hitfp了**
