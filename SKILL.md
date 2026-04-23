---
name: robots-checker
description: |
  检查URL是否允许爬虫抓取。用于判断某个网页是否被robots.txt允许访问。
  
  触发场景：
  (1) 用户询问某个URL是否能抓取/爬取
  (2) 需要检查网站的爬虫政策
  (3) 涉及爬虫合规性、robots.txt分析
---

# Robots.txt 检查工具

## 功能

给定任意URL，检查该网站是否允许爬虫抓取该URL。

## 工作流程

### 第一步：提取域名和Schema

从用户提供的URL中提取：
- **schema**: http 或 https
- **host**: 域名部分（不含路径）

例如：URL `https://example.com/blog/article` → schema=`https`, host=`example.com`

### 第二步：获取Robots.txt

使用 `web_fetch` 工具访问 `schema://host/robots.txt`，获取该网站的爬虫指导方针。

例如：`https://example.com/robots.txt`

### 第三步：大模型判断

将以下信息发送给大模型（使用默认模型）：

```
## 任务
判断网站是否允许抓取指定的URL。

## 网站的Robots.txt内容
[这里是robots.txt的全文]

## 要检查的URL
[用户提供的完整URL]

## 输出要求
请分析robots.txt的规则，判断是否允许抓取该URL。
如果允许，说明原因；如果禁止，说明依据哪条规则。
```

### 第四步：返回结果

将大模型的判断结果返回给用户，包含：
- 是否允许抓取
- 依据的robots.txt规则
- 建议

## 示例

用户输入：`https://example.com/blog/2024/article`

1. 提取：schema=https, host=example.com
2. 获取：`https://example.com/robots.txt`
3. 发送大模型分析
4. 返回结果