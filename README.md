# 记忆方块 | Memory Cube

一个基于 Three.js 的 3D 视觉记忆游戏。记忆残缺立方体上缺失的方块位置，然后在完整的立方体上将它们一一消除。

灵感来自 [Human Benchmark - Memory Test](https://humanbenchmark.com/tests/memory) 与 [Machine Party](https://machineparty.com/) 的 Chisel Gauntlet 关卡。

**在线试玩：[howerlin.top/cube-memory](https://howerlin.top/cube-memory/)**

---

## 玩法

1. **记忆阶段**：展示一个残缺的 3x3x3 立方体，记住缺失方块的位置
2. **还原阶段**：在完整的立方体上，点击消除对应的方块
3. **一命制**：每关只有一次机会，消除错误或遗漏任何方块即游戏结束
4. **难度递增**：关卡越高，缺失方块越多，记忆和还原时间越短
5. **通关条件**：通过 Level 10 通关，通关后可选择继续挑战，无封顶关卡

### 难度曲线

| 参数 | 公式 | 说明 |
|---|---|---|
| 缺失方块数 | `min(level + 1, 9)` | Level 1 缺 2 块，Level 8 起封顶 9 块 |
| 记忆时间 | Level 1-10: 固定 `3000`<br>Level 11+: `max(2500, 3000 - (level - 10) * 100)` | 1-10 关固定 3s，通关后每级 -0.1s，最低 2.5s（Level 15 达到） |
| 还原时间 | `max(15000, 30000 - level * 1000)` | 从 30s 递减，最低 15s |
| 通关等级 | `10` | 通过 Level 10 通关，通关后可继续挑战，无封顶 |

## 特性

- **3D 等距视角**：三面等大的透视相机，固定不动，专注记忆
- **无间隙实体方块**：方块紧邻排列，视觉一体感强
- **Hover 高亮**：桌面端鼠标悬停高亮，移动端触摸预览
- **渐进式音效**：消除方块时播放五声音阶（C-D-E-G-A-B-C-D-E），逐级升高
- **时间紧迫感**：还原最后 5 秒进度条脉冲 + 每秒 tick 音
- **完美通关庆祝**：光环波纹扩散 + 立方体弹跳动画
- **通关成就**：通过 Level 10 后显示通关画面 + 上行五声音阶庆祝音效，可选择继续挑战无封顶
- **进度提示**：实时显示「已消除 X / Y」
- **键盘快捷键**：`Enter` 确认 / `R` 重置 / `Space` 开始
- **最高记录**：localStorage 持久化，破纪录时高亮提示
- **分享链接**：一键复制游戏链接到剪贴板
- **响应式适配**：桌面 / 平板 / 手机 / 横屏全适配
- **移动端优化**：动画速度比桌面快 ~15-20%，手感更紧凑

## 技术栈

- **Three.js 0.160.0**（CDN + importmap，ES Modules）
- **Line2 + LineMaterial**：实现真正可控的粗边线渲染（WebGL 中 `LineBasicMaterial.linewidth` 不生效）
- **Web Audio API**：实时合成五声音阶音效，无外部音频文件
- **PerspectiveCamera**：FOV 35°，等距位置 (7.5, 7.5, 7.5)，动态距离适配屏幕

### 性能优化

- **共享几何体**：27 个方块共用 1 个 `BoxGeometry` + 1 个 `LineSegmentsGeometry`（GPU 内存降低 96%）
- **DOM 引用缓存**：`init()` 时一次性缓存所有 DOM 元素，避免每帧 `getElementById`
- **Raycaster 缓存**：预存 mesh 数组，避免每次 `pointermove` 创建临时数组
- **空闲跳过渲染**：菜单 / 游戏结束界面跳过 `animateVoxels()` 和 `renderer.render()`
- **WebGLRenderer 优化**：`powerPreference: 'high-performance'` + `alpha: false`

## 本地运行

无需构建，直接用任意静态服务器即可：

```bash
# Python
python -m http.server 8080

# Node.js
npx serve .
```

然后打开 `http://localhost:8080`。

## 项目结构

```
cube-memory/
├── index.html    # 全部游戏代码（HTML + CSS + JS 单文件）
└── README.md
```

## License

MIT
