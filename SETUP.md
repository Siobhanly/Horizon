# Horizon 快速部署指南

## 配置步骤

### 1. 修改 config.json

编辑 `data/config.json`，将以下占位符替换为你的实际值：

```json
{
  "ai": {
    "model": "YOUR_MODEL_NAME",      // 替换为你的模型名，如 gpt-4, deepseek-chat, qwen-turbo
    "base_url": "YOUR_BASE_URL"      // 替换为你的 API 地址，如 https://api.xxx.com/v1
  }
}
```

### 2. Push 到 GitHub

```bash
cd Horizon
git add .
git commit -m "feat: 配置个人信息源和 AI 接口"
git push origin main
```

### 3. 设置 GitHub Secrets

进入你的 GitHub 仓库页面：**Settings → Secrets and variables → Actions → New repository secret**

添加以下 Secrets：

| Secret 名称 | 说明 | 必需 |
|---|---|---|
| `OPENAI_API_KEY` | 你的 AI API Key | ✅ |
| `HORIZON_WEBHOOK_URL` | 飞书机器人 Webhook URL | ✅ |
| `GITHUB_TOKEN` | GitHub Personal Access Token | 推荐 |
| `APIFY_TOKEN` | Apify Token（抓取 Twitter 需要） | 可选 |
| `LWN_KEY` | LWN.net 订阅密钥 | 可选 |

### 4. 获取飞书 Webhook URL

1. 打开飞书，进入你想推送的群聊
2. 点击群设置 → 群机器人 → 添加机器人
3. 选择「自定义机器人」
4. 复制 Webhook URL（格式：`https://open.feishu.cn/open-apis/bot/v2/hook/xxx`）

### 5. 启用 GitHub Actions

1. 进入仓库页面 → **Actions** 标签
2. 点击 **"I understand my workflows, go ahead and enable them"**
3. 工作流已配置为每天北京时间 09:00 自动运行

### 6. 手动触发测试

进入 **Actions → Daily Horizon Summary → Run workflow**，点击运行按钮测试一次。

## 自定义信息源

编辑 `data/config.json` 中的 `sources` 部分：

- **Hacker News**: 调整 `fetch_top_stories` 和 `min_score` 控制数量和质量
- **RSS**: 添加/删除你关注的 RSS 源
- **Reddit**: 添加你感兴趣的 subreddit
- **Telegram**: 添加 Telegram 频道
- **Twitter**: 添加 Twitter 用户（需要 Apify Token）
- **GitHub**: 添加关注的用户或仓库

## 调整过滤阈值

```json
"filtering": {
  "ai_score_threshold": 8.0,  // 降低此值获取更多内容，升高获取更精简内容
  "time_window_hours": 24     // 时间窗口
}
```
