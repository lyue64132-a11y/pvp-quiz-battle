# 资源目录规范

## 角色模型

把外部模型放在对应角色目录，命名固定为 `model.<format>`：

```text
public/assets/characters/<character-id>/
├── model.glb                # 或 model.gltf / model.fbx / model.obj
├── textures/
│   ├── body.png             # 贴图：PNG / JPG / TGA
│   └── body_normal.png      # Normal Map（可选）
└── animations/              # 独立动画资源（可选）
```

`<character-id>` 必须与 `src/data/characters.json` 中的 `id` 一致。

## 支持的格式

| 类型 | 支持格式 | 说明 |
| --- | --- | --- |
| 模型 | GLB / GLTF / FBX / OBJ | GLB/GLTF 优先；FBX/OBJ 由 Three.js Loader 加载 |
| 贴图 | PNG / JPG / TGA | TGA 通过 TGALoader 加载 |
| 动画 | 模型内 Animation Clip | 通过 `characters.json` 的 `animations` 字段映射 |

## 动画映射

`characters.json` 中的 `animations` 字段把统一状态名映射到模型内的动画名：

```json
"animations": {
  "idle": "Idle",
  "attack": "Attack",
  "hurt": "Hurt",
  "victory": "Victory",
  "defeat": "Defeat"
}
```

匹配规则：动画 Clip 名称完全等于配置名，或相互包含。

## 回退机制

模型文件缺失或格式不受支持时不会报错：`ResourceManager` 返回空结果，
`CharacterSystem` 自动生成程序化角色（待机、攻击、受击、胜利、失败动画均为内置状态）。

## 特效资源

特效模型和贴图放在 `public/assets/effects/` 下，完整的新增步骤和 JSON 字段说明见
`docs/自定义特效.md`。支持 GLB/GLTF/FBX/OBJ 模型特效与 PNG/JPG/TGA 贴图特效，
也支持程序化粒子、圆环、闪电。
