# Slicer AI 技能 (slicer-skill)

这个文件夹是一个专门给 AI（如 Claude、OpenCode、Cursor）看的**“Slicer 开发说明书”**。

它的作用是告诉 AI：**“当用户问你关于 3D Slicer 的问题时，不要瞎猜代码，去官方文档和 GitHub 源代码里搜索准确的写法。”**

---

## 💡 如何使用？

这个技能**不需要你下载任何源代码**，也**不需要配置任何环境变量**。它的核心就是纯粹的 Prompt（提示词）指令。

### 使用场景 1：日常写代码、查 API（零配置）

**你什么都不用做**，只需要把这个文件夹（`slicer-skill`）留在你的项目里即可。

当你向 AI 提问时（例如：*"帮我写一段 Slicer 代码，加载一个 DICOM 文件夹"*）：
1. AI 会自动读取这个文件夹里的 `SKILL.md`。
2. AI 会根据 `SKILL.md` 的指示，在后台**自动联网搜索** Slicer 的官方文档、API 参考和源码。
3. AI 查到准确的写法后，会把正确的代码回复给你。

**示例提问：**
- *"Slicer 里 `vtkMRMLVolumeNode` 怎么获取 numpy 数组？"*
- *"我想在我的 Python 扩展里加一个按钮，点击后给 3D 视图截图，代码怎么写？"*
- *"Slicer 的 Segment Editor 怎么用 Python 自动化执行阈值分割？"*

---

### 使用场景 2：让 AI 直接操控你运行中的 Slicer（高阶玩法）

这个文件夹里包含了一个名为 `slicer-mcp-server.py` (from: https://github.com/pieper/slicer-skill/blob/main/slicer-mcp-server.py)的脚本。它可以启动一个本地服务器，让 AI 能直接读取你 Slicer 里的数据，甚至直接在里面执行代码。

**操作步骤：**

1. **打开你的 3D Slicer 软件。**
2. **运行服务器：** 打开 Slicer 的 Python 控制台（Python Console），将 `slicer-mcp-server.py` 拖进去运行，或者输入以下代码回车：
   ```python
   exec(open("/你的项目路径/.opencode/skills/slicer-skill/slicer-mcp-server.py").read())
   ```
   *（运行后，Slicer 就变成了一个可以被 AI 控制的服务器）*
3. **配置你的 AI 客户端：** 在你的 AI 编辑器（例如 OpenCode / Claude Code）的 `.mcp.json` 中添加：
   ```json
   {
     "mcpServers": {
       "slicer": {
         "type": "http",
         "url": "http://localhost:2026/mcp"
       }
     }
   }
   ```
4. **开始对话：** 现在你可以直接让 AI 操作 Slicer 了！
   - *"帮我看看我现在 Slicer 里有几个 Volume 节点？"*
   - *"把当前 3D 视图里的模型颜色改成红色。"*
   - *"读取名为 'CT' 的图像，把它转换成 numpy 数组算一下平均值。"*

---

## 📁 文件说明

| 文件 | 作用 |
|------|------|
| `SKILL.md` | 给 AI 看的指令手册，告诉它去哪里搜索 Slicer 的资料。 |
| `README.md` | 就是你现在看的这份说明文档。 |
| `slicer-mcp-server.py` | （可选）运行在 Slicer 内部的服务器脚本，用于让 AI 直接操控 Slicer。 |

## 这么做出来的:

参考: https://github.com/pieper/slicer-skill

用`opencode` + `Gemini-3.1`


