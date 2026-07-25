<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTML 项目集 · 卡片网格</title>
    <!-- 使用 Google Font 提升字体质感 -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:opsz,wght@14..32,400;14..32,600;14..32,700&display=swap" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: #f8fafc;
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
            padding: 2rem 1.5rem;
            color: #0b1a33;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
        }

        /* ----- 头部区域 (保留原有风格) ----- */
        .header {
            text-align: center;
            margin-bottom: 2.5rem;
        }

        .header img {
            max-width: 100%;
            height: auto;
        }

        .badge-group {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 0.75rem 1rem;
            margin: 1.2rem 0 0.5rem;
        }

        .badge-group img {
            height: 28px;
            width: auto;
        }

        /* 模拟徽章 (因为原图是svg外链, 保留原样, 同时用内联样式兜底) */
        .badge-fallback {
            display: inline-flex;
            align-items: center;
            background: #eef2f6;
            padding: 0.3rem 1rem;
            border-radius: 30px;
            font-size: 0.85rem;
            font-weight: 600;
            color: #1e3a5f;
            gap: 0.3rem;
            box-shadow: 0 1px 3px rgba(0,0,0,0.02);
        }

        .badge-fallback span {
            color: #4f8cf7;
        }

        .badge-fallback i {
            font-style: normal;
        }

        /* ----- 卡片网格 (正方形 + 响应式多列) ----- */
        .project-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
            gap: 2rem 1.8rem;
            margin: 2.5rem 0;
        }

        /* 卡片主体: 正方形 (aspect-ratio: 1/1)  + 柔和卡片 */
        .project-card {
            background: #ffffff;
            border-radius: 2rem;
            box-shadow: 0 8px 20px rgba(0, 20, 40, 0.06), 0 2px 6px rgba(0, 0, 0, 0.02);
            padding: 1.8rem 1.5rem 1.5rem;
            aspect-ratio: 1 / 1;
            display: flex;
            flex-direction: column;
            transition: transform 0.18s ease, box-shadow 0.25s ease;
            border: 1px solid #f0f4fa;
            position: relative;
            overflow: hidden;
        }

        .project-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 18px 35px rgba(45, 85, 150, 0.08), 0 4px 12px rgba(0,0,0,0.02);
            border-color: #d6e2f0;
        }

        /* 卡片头部：标题 + 小标签 */
        .card-header {
            display: flex;
            align-items: flex-start;
            justify-content: space-between;
            gap: 0.5rem;
            margin-bottom: 0.8rem;
        }

        .card-title {
            font-size: 1.2rem;
            font-weight: 700;
            line-height: 1.3;
            color: #0b1f33;
            text-decoration: none;
            transition: color 0.15s;
            word-break: break-word;
        }

        .card-title a {
            color: inherit;
            text-decoration: none;
        }

        .card-title a:hover {
            color: #4f8cf7;
        }

        .card-badge {
            background: #eef4ff;
            border-radius: 30px;
            padding: 0.15rem 0.7rem;
            font-size: 0.7rem;
            font-weight: 600;
            color: #2b5ea7;
            white-space: nowrap;
            letter-spacing: 0.02em;
            border: 1px solid #dbe6f5;
            flex-shrink: 0;
            margin-top: 0.15rem;
        }

        /* 状态标签行 */
        .card-meta {
            display: flex;
            flex-wrap: wrap;
            gap: 0.4rem 0.6rem;
            margin-bottom: 1rem;
        }

        .card-meta span {
            font-size: 0.7rem;
            font-weight: 500;
            background: #f2f6fc;
            padding: 0.2rem 0.7rem;
            border-radius: 20px;
            color: #1f3b5c;
            border: 1px solid #e7eef7;
        }

        .card-meta .status-green {
            background: #e6f7ec;
            color: #1a6e4b;
            border-color: #b8e0cc;
        }

        .card-meta .status-orange {
            background: #fef3e6;
            color: #a8681a;
            border-color: #f5dbb8;
        }

        /* 简介文字 - 自动撑开剩余空间 */
        .card-desc {
            font-size: 0.9rem;
            line-height: 1.5;
            color: #1f3a5e;
            flex: 1 1 auto;
            margin-bottom: 1rem;
            display: flex;
            align-items: center;
        }

        .card-desc p {
            margin: 0;
            display: -webkit-box;
            -webkit-line-clamp: 4;
            -webkit-box-orient: vertical;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        /* 底部：预览链接 + 额外装饰 */
        .card-footer {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-top: 0.2rem;
            border-top: 1px solid #edf2f9;
            padding-top: 0.9rem;
        }

        .card-footer .preview-link {
            font-weight: 600;
            font-size: 0.9rem;
            background: #eef4ff;
            padding: 0.4rem 1.2rem;
            border-radius: 40px;
            color: #1f4a8a;
            text-decoration: none;
            transition: background 0.2s, color 0.2s;
            border: 1px solid transparent;
        }

        .card-footer .preview-link:hover {
            background: #dce8fe;
            color: #0b2d66;
            border-color: #b6cdf5;
        }

        .card-footer .lang-tag {
            font-size: 0.7rem;
            color: #5e7a9f;
            background: #f0f4fc;
            padding: 0.2rem 0.8rem;
            border-radius: 30px;
        }

        /* 为了让卡片更「正方形」，内容间距微调 */
        .project-card .emoji-big {
            font-size: 1.6rem;
            line-height: 1;
            margin-right: 0.2rem;
        }

        /* 统计区域 & 底部 */
        .stats-section {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            align-items: center;
            gap: 1.8rem 3rem;
            margin: 3rem 0 2rem;
            padding: 1.5rem 1rem;
            background: #ffffffd6;
            border-radius: 3rem;
            backdrop-filter: blur(2px);
            box-shadow: 0 2px 12px rgba(0,0,0,0.02);
        }

        .stats-section img {
            max-width: 100%;
            height: auto;
        }

        .lang-bar {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 0.8rem 1.5rem;
            font-size: 0.9rem;
            font-weight: 500;
        }

        .lang-bar span {
            background: #f0f4fc;
            padding: 0.2rem 1rem;
            border-radius: 40px;
        }

        .footer-note {
            text-align: center;
            margin-top: 2.5rem;
            font-size: 0.9rem;
            color: #3a5a7c;
            border-top: 1px solid #dce6f0;
            padding-top: 2rem;
        }

        .footer-note a {
            color: #4f8cf7;
            text-decoration: none;
        }

        /* 移动端适配 */
        @media (max-width: 640px) {
            body {
                padding: 1rem 0.8rem;
            }
            .project-grid {
                gap: 1.2rem;
            }
            .project-card {
                padding: 1.2rem 1rem;
                border-radius: 1.5rem;
            }
            .card-title {
                font-size: 1rem;
            }
            .card-footer .preview-link {
                padding: 0.25rem 1rem;
                font-size: 0.8rem;
            }
        }

        /* 保留原Markdown中的一些视觉元素 (模拟) */
        .hr-divider {
            border: none;
            border-top: 2px dashed #d7e2f0;
            margin: 2rem 0;
        }
    </style>
</head>
<body>
<div class="container">

    <!-- 头部 Banner (保留原打字动画 + 徽章) -->
    <div class="header">
        <p align="center">
            <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=24&pause=1000&color=4F8CF7&center=true&vCenter=true&width=435&lines=%F0%9F%8C%90+HTML+%E7%BD%91%E9%A1%B5%E9%A1%B9%E7%9B%AE%E9%9B%86;%E5%89%8D%E7%AB%AF%E5%AE%9E%E8%B7%B5%E4%BD%9C%E5%93%81%E5%B1%95%E7%A4%BA" alt="Typing SVG" style="max-width:100%;" />
        </p>
        <div class="badge-group">
            <!-- 保留原徽章，同时使用fallback文字以防图片加载慢 -->
            <span class="badge-fallback"><i>📦</i> 项目总数 <span>4</span></span>
            <span class="badge-fallback"><i>📅</i> 最后更新 2026.07</span>
            <span class="badge-fallback"><i>🛠️</i> 技术栈 HTML+CSS+JS</span>
            <br>
            <!-- 原徽章图片（兼容显示） -->
            <img src="https://img.shields.io/badge/📦_项目总数-4-4F8CF7?style=for-the-badge&logo=github" alt="项目总数" style="height:28px;" />
            <img src="https://img.shields.io/badge/📅_最后更新-2026.07-4FC7F7?style=for-the-badge" alt="最后更新" style="height:28px;" />
            <img src="https://img.shields.io/badge/🛠️_技术栈-HTML+CSS+JS-FFD700?style=for-the-badge&logo=javascript" alt="技术栈" style="height:28px;" />
        </div>
    </div>

    <!-- 项目卡片网格 (正方形、多列) -->
    <div class="project-grid">

        <!-- 1. 数织计算器 -->
        <div class="project-card">
            <div class="card-header">
                <div class="card-title">
                    <a href="https://qiaofeng6666.github.io//HTML/1.shuzhi/shuzhi.html">
                        <span class="emoji-big">🧩</span> 数织计算器
                    </a>
                </div>
                <span class="card-badge">15×15</span>
            </div>
            <div class="card-meta">
                <span class="status-green">✅ 可运行</span>
                <span>⭐⭐⭐ 难度</span>
            </div>
            <div class="card-desc">
                <p>📌 一个 15×15 的数织（Nonogram）计算器，帮助你快速求解像素谜题。</p>
            </div>
            <div class="card-footer">
                <a href="https://qiaofeng6666.github.io//HTML/1.shuzhi/shuzhi.html" class="preview-link">🔗 预览 →</a>
                <span class="lang-tag">HTML / CSS</span>
            </div>
        </div>

        <!-- 2. 滚灯弹幕 -->
        <div class="project-card">
            <div class="card-header">
                <div class="card-title">
                    <a href="https://qiaofeng6666.github.io//HTML/2.dashabi/dashabi.html">
                        <span class="emoji-big">🎡</span> 滚灯弹幕
                    </a>
                </div>
                <span class="card-badge">动态交互</span>
            </div>
            <div class="card-meta">
                <span class="status-green">✅ 可运行</span>
                <span>🎨 视觉效果</span>
            </div>
            <div class="card-desc">
                <p>📌 一个滚灯弹幕类型的网页，带来独特的动态视觉体验。</p>
            </div>
            <div class="card-footer">
                <a href="https://qiaofeng6666.github.io//HTML/2.dashabi/dashabi.html" class="preview-link">🔗 预览 →</a>
                <span class="lang-tag">JS / Canvas</span>
            </div>
        </div>

        <!-- 3. 股价涨跌幅计算器 -->
        <div class="project-card">
            <div class="card-header">
                <div class="card-title">
                    <a href="https://qiaofeng6666.github.io//HTML/3.gujia/gujia.html">
                        <span class="emoji-big">📈</span> 股价计算器
                    </a>
                </div>
                <span class="card-badge">涨跌幅分析</span>
            </div>
            <div class="card-meta">
                <span class="status-green">✅ 可运行</span>
                <span>📊 金融工具</span>
            </div>
            <div class="card-desc">
                <p>📌 一个计算股价涨跌幅的网页，帮你快速分析投资回报。</p>
            </div>
            <div class="card-footer">
                <a href="https://qiaofeng6666.github.io//HTML/3.gujia/gujia.html" class="preview-link">🔗 预览 →</a>
                <span class="lang-tag">JS / 金融</span>
            </div>
        </div>

        <!-- 4. 每日任务 -->
        <div class="project-card">
            <div class="card-header">
                <div class="card-title">
                    <a href="https://qiaofeng6666.github.io//HTML/4.meirirenwu/meirirenwu.html">
                        <span class="emoji-big">📋</span> 每日任务
                    </a>
                </div>
                <span class="card-badge">记录完成</span>
            </div>
            <div class="card-meta">
                <span class="status-green">✅ 可运行</span>
                <span>📌 实用工具</span>
            </div>
            <div class="card-desc">
                <p>📌 一个记录每日任务的网页，帮你规划每日任务。</p>
            </div>
            <div class="card-footer">
                <a href="https://qiaofeng6666.github.io//HTML/4.meirirenwu/meirirenwu.html" class="preview-link">🔗 预览 →</a>
                <span class="lang-tag">Todo / 本地</span>
            </div>
        </div>
    </div>

    <!-- 统计 & 语言 (保留原风格) -->
    <div class="stats-section">
        <img src="https://stats.justsong.cn/api/github?username=qiaofeng6666&theme=default&show_icons=true&hide_title=true&hide_rank=true" alt="GitHub 统计" width="400" style="max-width:100%;" />
        <div class="lang-bar">
            <span>📚 HTML 59%</span>
            <span>🎨 CSS 21%</span>
            <span>⚡ JavaScript 20%</span>
        </div>
    </div>

    <!-- 本地运行提示 + Star -->
    <div style="background:#f2f7ff; border-radius: 2.5rem; padding: 1.5rem 2rem; margin: 2rem 0 1rem; border:1px solid #dce8fa;">
        <h4 style="font-weight: 600; margin-bottom: 0.5rem; color: #16437e;">🚀 本地运行</h4>
        <pre style="background: #0b1f33; color: #d6eaff; padding: 0.8rem 1.2rem; border-radius: 1.2rem; overflow-x: auto; font-size: 0.8rem; margin: 0.5rem 0 0.2rem;"><code>git clone https://github.com/qiaofeng6666/qiaofeng6666.github.io.git
cd qiaofeng6666.github.io
# 用浏览器打开 HTML 文件夹里的对应文件即可</code></pre>
    </div>

    <div class="footer-note">
        <p>🌟 如果这些项目对你有帮助，欢迎给个 Star ⭐ <br>
        📝 持续更新中 · 更多有趣的项目敬请期待</p>
        <p style="font-size:0.75rem; margin-top: 0.8rem; opacity: 0.7;">© 2026 · 卡片网格展示 · 正方形多列布局</p>
    </div>

</div>
</body>
</html>
