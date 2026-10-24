# 延时回响 · Delayed Echo（游戏）

一款以「足迹记录 + 回响回放」为核心的 2D 物理解谜平台跳跃游戏。玩家走过的每一步都会被世界记住，攒够足迹后可以释放一个「影子」，让几秒钟前的自己沿着完全相同的路线循环回放，替玩家踩下此刻踩不到的机关。

> 纯前端（原生 HTML / CSS / JavaScript），无框架、无构建、无后端，**双击 index.html 即可运行**。

---

## 部署到 GitHub Pages

本仓库要上传的**只有 `index.html` 一个文件**（6.1 MB，音频与贴纸已全部 base64 内嵌）。

```bash
git init
git add index.html
git commit -m "延时回响 Delayed Echo"
git remote add origin https://github.com/<你的用户名>/delayed-echo-game.git
git push -u origin main
```

然后在仓库 **Settings → Pages** 里把 Source 选成 `main` 分支 / `root`，访问：

```
https://<你的用户名>.github.io/delayed-echo-game/
```

**不需要**任何构建步骤、npm、base path 配置。整份游戏就是一个 HTML 文件，双击也能离线运行。

> `dev/` 目录是开发脚本与素材，**不参与运行、不需要上传**（仓库体积小、页面加载快）。

---

## 目录结构

```
delayed-echo-game/
├── index.html            ← 游戏本体（唯一需要部署的文件）
├── README.md
└── dev/                  开发用，不上传
    ├── scripts/          生成器与调试脚本
    │                     gen_lv6-v6.js（生成第 6 关三幕地图）
    │                     build_audio.js（合成 BGM）
    │                     gen_plates.js / gen_doc.js（出样张与策划案）
    ├── tests/            自动化测试（326 + 19 + 23 项）
    ├── audio-src/        音频源 mp3 + cue.json
    ├── assets/           地图样张、预览渲染
    ├── screenshots/      调试截图
    ├── design/           第 6 关策划案与地图数据
    └── backup/           历史版本存档
```

---

## 开发者命令

```bash
cd dev/tests
node test_echo.js            # 326 项：核心玩法 + 第 6 关三幕
node test_editor_blocks.js   # 19 项：编辑器方块合并（跨仓库读编辑器）
node test_link.js            # 23 项：编辑器 ↔ 游戏 联动
```

```bash
cd dev/scripts
node gen_lv6-v6.js           # 重新生成第 6 关三幕地图 → dev/design/
node gen_plates.js           # 地图样张 → dev/assets/plates/
node build_audio.js          # 合成音频 → dev/audio-src/
```

> **改地图必须先跑生成器再重出样张**，不要手改 `index.html` 里的 `WORLD_L6_A/B/C` 数组。

---

## 关卡一览

| 关卡 | 名称 | 尺寸 | 定位 | 新引入内容 |
|---|---|---|---|---|
| 1 | 回响初现 | 22 × 12 | 教学关 | 影子基础、告示牌、尖刺 |
| 2 | 回声借道 | 24 × 12 | 进阶关 | 血量与牺牲、升降木板、半砖地形 |
| 3 | 群山回响 | 40 × 30 | 综合关 | 摄像机滚动、切换开关、星星收集、迷雾 |
| 4 | 钥影回廊 | 48 × 51 | 编排关 | 钥匙收集、传送门、断桥、装置绑定表 |
| 5 | 假面回廊 | 40 × 30 | 辨伪关 | 假开关、机关墙、假传送门 |
| 6 | 一曲三折 | 62 / 50 / 53 × 20 | 收束关 | 一关三幕、J 弹簧、V 气流柱、m 脆板 |

---

## 核心机制速览

- **足迹记录**：走一格或跳一次算 1 步，先进先出，上限 10 步（第 3 关为无限）
- **回响（影子）**：满 5 步可按 `R` 释放，影子沿你走过的原路回放，替你去踩机关
- **第 6 关换幕**：只认幕门，不看音乐进度——走进尽头那道拱门，世界才闪一下换成下一张图

---

## 配套仓库

关卡编辑器在**另一个独立仓库**：`delayed-echo-editor`。

两者通过浏览器 `localStorage`（键 `delayed_echo_inbox`）通信，因此**必须在同一域名下访问**（GitHub Pages 上两个仓库同属 `用户名.github.io`，天然满足）。在编辑器顶部的「游戏页」输入框填上游戏网址，即可一键发送关卡并自动打开。

---

**原创声明**：本作玩法、关卡、代码与美术均为原创，未抄袭任何现有作品。
