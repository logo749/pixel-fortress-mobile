# Pixel Fortress · Mobile

Pixel Fortress 手机浏览器版——基于开源 RTS [dorianbayart/pixel-fortress](https://github.com/dorianbayart/pixel-fortress) 的二创移植（GPL-3.0-or-later），目标是竖屏触屏手机上开一局 8–12 分钟的完整遭遇战。

**在线游玩**：`https://logo749.github.io/pixel-fortress-mobile/`（手机浏览器打开即玩；进入 `/play.html` 可跳过落地页）

设计文档见飞书《Pixel Fortress 手机版 · 改版设计稿 v0.1》。游戏规则、建筑树、经济与胜负判定全部沿用上游，未做数值改动。

## 手机版改了什么

| 模块 | 改动 |
|---|---|
| 触控输入 | pointer 与 touch 双通道修为单通道；8px 拖拽死区；松手惯性滑动；双指捏合缩放 |
| 建造交互 | 两段式建造：点槽位 → 幽灵建筑吸附屏幕中心格（自动吸附最近合法格）→ 底部「放置 / 取消」确认；非法位只给一条原因 |
| 详情查看 | 长按 350ms（位移 <8px）打开己方建筑详情卡 |
| 视野 | 小屏初始短边 12 格（每格 ≥24 CSS px），可捏合缩放 |
| HUD | 底栏高度内容自适应（详情/确认条展开时自动压缩画布）；小地图默认折叠，展开后点按可跳转镜头，⌂ 按钮一键回基地 |
| 生命周期 | 切后台自动暂停并弹出菜单，回前台保持暂停，点「继续」才恢复；逻辑帧 delay 已有 50ms 封顶，无快进结算 |
| 音频 | 大文件（环境音/音乐）`preload='none'` 惰性加载，首屏不被音频阻塞 |
| 默认对局 | Small 地图 + 1 AI（Easy）+ 2× 速度，新玩家开箱即得 8–12 分钟一局 |

桌面版行为保持原样（鼠标幽灵放置、右键取消、失焦自动暂停）。

## 仓库结构

本仓库以**补丁**形式保存手机版改动：上游代码与二进制素材（精灵图、音效、音乐、字体，共数十 MB）都不入库，CI 在构建时拉取上游固定 commit `41dbece` 并应用 `fork.patch`，组装出完整站点后部署到 Pages——仓库因此只有几 KB 文本，且每次构建结果可复现。

```
fork.patch                     # 手机版全部改动（对上游 41dbece 的 git diff，19 个文件）
.github/workflows/deploy.yml   # 拉上游 → 应用补丁 → 组装 → 部署 Pages
LICENSE                        # GPL-3.0（与上游一致）
```

## 本地运行

```bash
git clone https://github.com/dorianbayart/pixel-fortress upstream
cd upstream
git checkout 41dbece4287080da01ccb8e77a734ae1a380ecf0
git apply /path/to/fork.patch     # 应用手机版改动
python3 -m http.server 8000
# 浏览器打开 http://localhost:8000/play.html
```

## Roadmap（v1 之后）

- 首局三步引导（造木材棚 → 派兵 → 进攻）
- 闲置农民同屏渲染上限（性能档位）
- 中文语言包、横屏适配、对局设置持久化

## 许可

GPL-3.0-or-later，见 [LICENSE](LICENSE)。本项目派生自 [dorianbayart/pixel-fortress](https://github.com/dorianbayart/pixel-fortress)（上游 commit `41dbece`），感谢原作者 Dorian Bayart 的开源贡献。上游的分析脚本（Umami beacon）已在手机版中移除。
