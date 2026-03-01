# WeChat-tweets-counterattacker
  你是否厌倦了家庭群中泛滥的伪科学说教与过度养生文案？本项目不采取硬刚模式，而是通过技术手段以其人之道还治其人，生成视觉与语义高度混淆的老王式推文，用长辈最熟悉的逻辑重塑家庭群话语权。
 （必备openclaw）
 
向您介绍——————————————————
  中老年公众号文案生成工具，支持AI生成内容并一键发布到微信公众号。（非认证公众号可见草稿箱），针对群内错误思想、过分说教的推文关键词直接生成反义文章，转发到群里狠狠抨击！
包括但不限于：孩子赖床才是聪明的选择、没对象+收入低=差？听听高智商父母怎么说、保健品如何蚕食你的钱包等。

## 👤 Author

**Sosa**-（准）算法工程师
*   Github: [@sosathegitter](https://github.com/sosathegitter)
*   Email: [919479311@qq.com]

**老王**-（准）算法工程师
*   Email: [738714836@qq.com]


## Details
  技术核心上，项目基于OpenClaw构建Skill架构，将AI封装为老王人设。系统集成Qwen3-Embedding向量模型与RAG技术，通过对本地范文库的相似度检索，实现动态文风匹配，确保生成的每一段话都精准踩在中老年营销号的节奏点上。交互层面，利用Slash Command（如 /老王推文）驱动逻辑编排，实现人在回路的交互微调，用户可实时审查草稿并一键下令。执行末梢则协同Python脚本自动化调度微信API，针对性抓取荷花、山水等特定审美配图，并注入18px超大字号内联样式进行护眼排版，将反击内容投送到公众号草稿箱。


功能特性
- 🎯 AI文案生成：基于Qwen3-0.6b嵌入模型保留稳定的风格 + MiniMax生成中老年风格文章。

- 📱 公众号发布：自动匹配封面/内页图（可选网图/aigc），一键创建微信草稿，随时反击。

- 👴 中老年风格：反直觉观点、接地气表达、温暖亲切的文风

## 项目结构

```
elderly-copywriter/
├── SKILL.md                    # Skill 定义（角色、写作规范、输出格式）
├── slash-commands.json         # 命令配置（可选，增加发文参与感）
├── scripts/
│   ├── build_prompt.py         # Prompt 构建脚本（可自定义）
│   └── save_article.py         # 文章保存脚本
├── publish_oa.py               # 微信公众号发布脚本
├── models/
│   └── qwen/                   # Qwen3 嵌入模型（可自定义）
├── .env.example                # 环境变量模板（必须修改！包含aigc以及公众号api）
└── README.md                   # 使用说明
```

## 快速开始

### 1. 安装依赖

```bash
pip3.11 install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install python-dotenv requests
python3.11 -c "from modelscope import snapshot_download; snapshot_download('qwen/Qwen3-Embedding-0.6B', cache_dir='/root/.openclaw/models')

### 2. 配置环境变量

复制 `.env.example` 为 `.env` 并填写配置：

```bash
cp .env.example .env
# 编辑 .env 填入你的微信 AppID 和 AppSecret 以及minimax
```

### 3. 在 OpenClaw 中使用

将 skill 路径添加到 OpenClaw 配置，或直接复制到 `~/.openclaw/skills/elderly-copywriter/`。

### 4. 生成文案

在 OpenClaw 中激活 skill 后，输入主题和观点即可生成文章。格式：@elderly-copywriter 孩子玩游戏 爱玩游戏并非坏事  '@skill 论点 观点'.

### 5. 发布到公众号

当 AI 生成完文章后，说"发布"或"发布到公众号"即可自动：
1. 根据标题关键词匹配图片
2. 下载并上传到微信素材库
3. 创建微信草稿

## 环境变量说明

| 变量名 | 说明 | 必需 |
|--------|------|------|
| WECHAT_APPID | 微信公众号 AppID | ✅ |
| WECHAT_APPSECRET | 微信公众号 AppSecret | ✅ |
| MINIMAX_API_KEY | MiniMax API Key（可选） | ❌可选启用 |
| MINIMAX_GROUP_ID | MiniMax Group ID（可选） | ❌可选启用 |

## 工作流程

```
用户输入 → @elderly-copywriter 主题 观点
    ↓
LLM 生成 JSON 文章（title, hook, body, closing）
    ↓
用户确认 → "发布"
    ↓
publish_oa.py:
  1. 读取 latest_article.json
  2. 根据标题匹配图片 URL
  3. 下载图片
  4. 上传到微信素材库
  5. 创建草稿
    ↓
公众号后台手动发布
```

## 文件说明

| 文件 | 功能 |
|------|------|
| SKILL.md | Skill 定义，包含角色说明、写作规范、输出格式、发布流程指令 |
| slash-commands.json | 命令配置（/老王推文、/微调、/发布微信） |
| scripts/build_prompt.py | 构建 LLM prompt |
| scripts/save_article.py | 保存生成的 JSON 文章 |
| publish_oa.py | 微信公众号发布主脚本 |

## 依赖

- Python 3.11+
- requests
- python-dotenv
- Qwen3-Embedding-0.6B 模型（不包含，可自行选择）

## 注意事项

1. 首次发布需要确保 `/root/.openclaw/assets/default_cover.jpg` 存在
2. 草稿创建后需登录公众号后台手动发布
3. 模型文件较大（约 1.2GB），下载时注意存储空间

注：灵感来源-老赵讲道理。本项目基于openclaw实现。
