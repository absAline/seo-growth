# Google Analytics 4 + Search Console 配置指南

## 一、Google Analytics 4 (GA4)

### 创建 GA4 属性
1. 访问 https://analytics.google.com/
2. 创建账号 → 创建 GA4 属性
3. 获取衡量 ID（格式: `G-XXXXXXXXXX`）

### Astro 集成

创建 `src/components/Analytics.astro`:

```astro
---
const { measurementId } = Astro.props;
---

{
  measurementId && (
    <>
      <script is:inline src={`https://www.googletagmanager.com/gtag/js?id=${measurementId}`} async />
      <script is:inline>
        window.dataLayer = window.dataLayer || [];
        function gtag(){ dataLayer.push(arguments); }
        gtag('js', new Date());
        gtag('config', '{measurementId}');
      </script>
    </>
  )
}
```

在布局中引入:

```astro
---
import Analytics from '../components/Analytics.astro';
---

<html lang="zh-CN">
<head>
  <Analytics measurementId={import.meta.env.PUBLIC_GA_ID} />
</head>
```

环境变量 (`.env`):

```
PUBLIC_GA_ID=G-XXXXXXXXXX
```

### 关键指标
| 指标 | 说明 | 关注频率 |
|------|------|----------|
| 用户 (Users) | 独立访客数 | 每日 |
| 会话 (Sessions) | 访问次数 | 每日 |
| 页面浏览量 (Page Views) | 所有页面被看次数 | 每日 |
| 平均参与时长 | 用户停留时间 | 每周 |
| 事件数 | 按钮点击等交互 | 每周 |
| 转化率 | 目标完成率 | 每月 |

## 二、Google Search Console (GSC)

### 添加站点
1. 访问 https://search.google.com/search-console/
2. 添加属性 → 输入域名
3. 选择 DNS 验证（推荐）或 HTML 文件验证

### 验证方式 (DNS)
在你的域名 DNS 设置中添加 TXT 记录：
```
名称: @
类型: TXT
值: google-site-verification=xxxxxxxxx
```

### 提交 Sitemap
在 GSC → Sitemaps → 输入:
```
https://YOUR_SITE.com/sitemap-index.xml
```

### 关注指标
| 指标 | 说明 | 行动 |
|------|------|------|
| 展示次数 | 搜索结果中出现次数 | 趋势监控 |
| 点击次数 | 实际点击次数 | 优化标题/描述 |
| 平均排名 | 搜索平均位置 | 低于 20 需优化 |
| 索引状态 | 被收录的页面数 | 未收录需排查 |
| Core Web Vitals | 用户体验指标 | 优化 LCP/FID/CLS |

## 三、Vercel Analytics (零配置方案)

如果使用 Vercel 部署，可在 Dashboard 直接启用：
1. Vercel 项目 → Analytics 标签
2. 点击 "Enable Analytics"
3. 无需额外代码

## 四、SEO 数据看板模板

```markdown
# 月度 SEO 报告

## 概况
- 总流量：___ (较上月 ±___%)
- 搜索流量：___ (较上月 ±___%)
- 收录页面：___ (新增 ___)
- 平均排名：___

## Top 10 流量页面
| 页面 | 浏览量 | 来源 | 关键词 |
|------|--------|------|--------|
| | | | |

## 待优化项
- [ ] 页面 A：跳出率高，需改进内容
- [ ] 页面 B：排名下降，需更新
- [ ] 新文章未收录，提交 GSC 索引
```

## 五、AI 数据分析 Prompt

```
以下是本月 GA4 数据，请分析：

总用户：[数字] | 新用户：[数字]
会话数：[数字] | 平均参与时长：[秒]
热门页面：[列表]
流量来源：Organic [%] / Direct [%] / Social [%] / Referral [%]
跳出率最高页面：[列表]
转化事件：[事件名] 完成 [数字] 次

请输出：
1. 本月数据亮点和问题
2. 下月优化建议（按优先级排序）
3. 应重点关注的关键指标
```
