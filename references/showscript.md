````markdown
---
marp: true
theme: gaia
_class: default
paginate: true
allowHtml: true
mermaid: true
style: |
  section {
    font-size: 25px;
    line-height: 1.4;
    grid-template-columns: 88%;
    margin: 0 auto;
    padding: 20px;
    display: grid;
    align-content: center;
    justify-content: center;
    background: linear-gradient(135deg, #fffdf7 0%, #f3f8ff 55%, #fdf2f8 100%);
  }
  /* 标题：彩色粗下划线 + 微阴影，活泼醒目 */
  h1 {
    font-size: 1.9em;
    font-weight: 800;
    line-height: 1.15;
    margin: 0 0 0.6em;
    color: #1e2a4a;
    border-bottom: 6px solid #ffd166;
    border-radius: 4px;
    padding-bottom: 0.18em;
    text-shadow: 0 2px 0 rgba(255, 209, 102, 0.25);
  }
  h2 {
    font-size: 1.5em;
    font-weight: 800;
    line-height: 1.2;
    margin: 0 0 0.45em;
    color: #0f4c81;
    border-left: 10px solid #ff6b6b;
    background: rgba(255, 107, 107, 0.08);
    border-radius: 0 12px 12px 0;
    padding: 0.12em 0.35em 0.12em 0.4em;
  }
  h3 {
    font-size: 1.25em;
    font-weight: 700;
    line-height: 1.25;
    margin: 0 0 0.35em;
    color: #2a9d8f;
    border-bottom: 3px dashed #ffd166;
    padding-bottom: 0.15em;
  }
  p {
    margin: 0.5em;
  }
  ul,
  ol {
    margin-top: 0.3em;
    margin-bottom: 0.3em;
  }
  li {
    margin-bottom: 0.25em;
  }
  /* 列表项：彩色圆点标记，交替色系更活泼 */
  ul > li::marker {
    color: #ff6b6b;
    font-weight: bold;
  }
  ol > li::marker {
    color: #f77f00;
    font-weight: bold;
  }
  /* 表格：卡片式圆角 + 彩色表头 */
  table {
    width: 100%;
    border-collapse: separate;
    border-spacing: 0;
    margin: 0.5em 0;
    background: #ffffff;
    border-radius: 14px;
    overflow: hidden;
    box-shadow: 0 4px 14px rgba(30, 42, 74, 0.12);
  }
  thead th {
    background: linear-gradient(90deg, #0f4c81, #2a9d8f);
    color: #ffffff;
    font-weight: 700;
    padding: 0.45em 0.6em;
    text-align: left;
  }
  tbody td {
    padding: 0.4em 0.6em;
    border-top: 1px solid #eef1f6;
  }
  tbody tr:nth-child(even) td {
    background: #f6f9ff;
  }
  tbody tr:hover td {
    background: #fff6e5;
  }
  /* 代码块：透明底，无卡片边框（防盗水印文字） */
  pre {
    font-size: 0.85em;
    line-height: 1.35;
    margin: 0.3em 0;
    background: transparent;
    color: #9aa7bd;
    border-radius: 14px;
    padding: 0.1em 0.3em;
    box-shadow: none;
    white-space: pre-wrap !important;
    word-break: break-word;
  }
  pre code {
    font-size: 1em;
    color: inherit;
    background: transparent;
    border: none;
    padding: 0;
    white-space: pre-wrap !important;
    word-break: break-all;
  }
  /* 行内代码：浅色胶囊标签 */
  code {
    font-family: "SF Mono", Menlo, Consolas, monospace;
    background: #eef4ff;
    color: #0f4c81;
    border: 1px solid #d5e3f7;
    border-radius: 8px;
    padding: 0.08em 0.4em;
  }
  /* 强调文字：暖色高亮 */
  strong {
    color: #e63946;
  }
  /* 页码：彩色小圆片 */
  footer {
    font-size: 0.55em;
    color: #ffffff;
    background: #2a9d8f;
    border-radius: 999px;
    padding: 0.15em 0.8em;
    bottom: 18px;
    right: 22px;
  }
  img {
    margin:0.1em auto;
  }
  img[alt="mylogo"] {
    display:block;
    width: 130px ;
    height:130px ;
    border-radius: 50%;
    object-fit: contain;
    box-shadow: 0 6px 18px rgba(30, 42, 74, 0.25);
    border: 4px solid #ffffff;
  }

---

# 核心架构解析

## hi

---

## 章节一：背景介绍

- 向量检索与索引平衡
    - HNSW 索引平衡速度和精度
    - IVF_PQ 算法加速

---

## 章节二：核心代码示例

```python
def hello():
    print("hi")
```
````
