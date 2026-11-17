# chat-demo-harmony

> [!NOTE]
> 本项目为社区移植的非官方版本，与 DeepSeek（杭州深度求索人工智能基础技术研究有限公司）无关联、未被其认可或赞助。仅移植并分发其 MIT 许可的开源代码；应用图标使用 DeepSeek 标志仅用于标识其上游产品，相关商标权利归 DeepSeek 所有。

[DeepSeek chat-demo](https://github.com/deepseek-ai/chat-demo) 的 HarmonyOS（鸿蒙）移植版：将原 React 网页应用打包为可通过 Web 组件离线加载的 HAP 安装包。

应用内无后端，浏览器（Web 组件）直连 `https://api.deepseek.com`。首次使用请在应用内「设置」中填入你的 DeepSeek API Key（保存在应用本地）。

## 功能

- 多会话管理：自动标题、重命名、删除；编辑提示词、重新生成回复、消息分支切换
- 流式回复、深度思考、联网搜索、链接阅读与来源引用；可停止生成或继续回复
- 上传 / 粘贴 / 拖拽附件；语音输入与朗读
- Markdown、语法高亮、KaTeX 数学公式、Mermaid 图表
- 简体中文 / 英文，浅色 / 深色主题，移动端布局，聊天记录导入导出（ZIP）

功能与原项目一致，详见[原项目 README](web/README.zh-CN.md)。

## 仓库结构

```
├── AppScope/                  应用级配置
├── entry/                     HarmonyOS 工程模块（HAP 入口）
│   └── src/main/
│       ├── ets/pages/Index.ets    Web 组件壳页面
│       └── resources/rawfile/     由 web/ 构建出的静态网页（已预构建）
├── web/                       上游 React 源码（MIT，deepseek-ai/chat-demo）
├── build-profile.json5
└── hvigorfile.ts
```

## 构建HAP

环境要求：[DevEco Studio](https://developer.huawei.com/consumer/cn/deveco-studio/)（或配套命令行工具）。

1. 使用 DevEco Studio 打开仓库根目录，或在命令行执行：

   ```sh
   devecocli build
   ```

2. 产物位于 `entry/build/default/outputs/default/`。

### 签名

默认未配置签名（构建产出未签名 HAP，仅供调试）。如需安装到真机：

- DevEco Studio：File → Project Structure → Signing Configs，勾选 **Automatically generate signature** 自动生成调试签名；
- 或使用 `devecocli signature generate` 生成签名材料并写入配置。

签名材料（`.p12` / `.cer` / `.p7b`）与 `build-profile.json5` 中的密码请勿提交到仓库。

## 更新网页资源

`entry/src/main/resources/rawfile/` 内是 `web/` 的预构建产物。修改网页源码后重新构建并复制：

```sh
cd web
pnpm install
pnpm build          # 产出 web/dist
# 将 dist 内全部文件复制到 entry/src/main/resources/rawfile/
```

## 许可证

[MIT](LICENSE)，继承自上游项目。`web/` 目录为上游源码，第三方依赖遵循各自许可证。
