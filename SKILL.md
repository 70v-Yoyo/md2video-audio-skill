---
name: md2video-audio
description: 将指定Markdown文件一键转换为带免费真人配音和讲解画面的 MP4 视频。零成本。当用户要求把 markdown 转成视频、制作口播视频或音视频合成时自动触发。
---

# Markdown 免费口播视频生成技能 (md2video-audio)

- 用途：将本地Markdown 文件自动转化为包含画面与免费语音的 MP4 视频。

## 执行步骤与规范

- 依次按步骤串行，从原稿到排版与优化稿到展示稿到口播稿，前项生成后项。

### 1. 环境依赖检查与安装

首先检查是否具备相关依赖，若缺失则请求安装：
```bash
pip install edge-tts moviepy markdown 

#安装一整套完美对齐、针对 Mermaid 优化的新一代测试版全家桶
npm install @marp-team/marp-cli@latest \
  @marp-team/marp-core@next \
  shiki \
  beautiful-mermaid \
  katex \
  @mathjax/src \
  @mathjax/mathjax-bbm-font-extension \
  @mathjax/mathjax-bboldx-font-extension \
  @mathjax/mathjax-dsfont-font-extension \
  @mathjax/mathjax-mhchem-font-extension
  
```

(注：若系统提示缺少 ffmpeg，需引导或自动通过 `apt-get install -y ffmpeg` 进行安装)

### 2. 生成排版与优化稿

- **语义审阅+内容优化**：检查该文件是否用markdown结构化符号按逻辑分段。如果不满足则进行**逻辑分段与排版优化**，注意不要在原输入md文件上修改，而是保存成`new-原名前缀-时间戳`的新md文件

  - 检查并补全文章的层级标题（确保有 `#` 和 `##` 等作为自然的视觉分镜切换点）。
  - 在合适位置自然放置彩色表情/图标（如emoji）但要保证系统兼容性、HTML能正常渲染出来。
  
  
    - 代码块里：一行过长按逻辑换行、行数过多按逻辑拆分成合适行数的多个代码块。代码块外前后补充自然过渡引导语。md文件里正常写mermaid没有报错则不用动它。
  

### 3. 生成展示稿

- 将上个步骤得到的md文件`new-原名前缀-时间戳`拆分为逻辑自然的展示稿，要求如下：
1. Markdown 文件最顶部加上几行配置。这里暂停询问用户要用哪种Marp风格展示，以及是否加入`allowHtml: true`，如果选默认风格则按下面这个配置
  ```markdown
  ---
  marp: true
  theme: gaia
  _class: default
  paginate: true
  allowHtml: true
  mermaid: true
  style: |
    section {
      /* ===== 全局基准 ===== */
      font-size: 25px;
      line-height: 1.4;
      /* ===== 页面布局 ===== */
      grid-template-columns: 88%; 
      margin: 0 auto;
      padding: 20px;
      display: grid;
      align-content: center;
      justify-content: center; /* 水平居中 */
    }
      /* ===== 代码块 ===== */
    pre {
      font-size: 1em;
      line-height: 1.35;
      margin: 0.5em 0;
      /* 关键：允许内部代码在达到 max-width 时自动换行 */
    	white-space: pre-wrap !important; 
   		word-break: break-word;
    }

    pre code {
      font-size: 1em;
      white-space: pre-wrap !important; /* 允许在长单词、长行内自动换行 */
    	word-break: break-all;
    }

    /* ===== 标题体系 ===== */
    h1 {
      font-size: 1.8em;
      line-height: 1.15;
      margin: 0 0 0.5em;
    }
    h2 {
      font-size: 1.4em;
      line-height: 1.2;
      margin: 0 0 0.4em;
    }
    h3 {
      font-size: 1.2em;
      line-height: 1.25;
      margin: 0 0 0.3em;
    }
    
    /* ===== 正文体系 ===== */
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
    
    /* ===== 图片 ===== */
    img {
      margin:0.1em auto;
    }
    img[alt="mylogo"] {
      display:block;
      width: 130px ;
      height:130px ;
      border-radius: 50%;
      object-fit: contain;
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

2. 将HTML标签尽可能都转换为markdown形式表示，如`<img>`转换为`![]()`而里面参数不变
3. 根据语义逻辑、md段落结构，用 `---` 来分隔每一页 PPT。
4. 检查展示稿每页是否符合内容分页规则（如下3条）：
   1. 判断单页内容是否超出页面安全容量（为marp单页高度的85%）：展示稿每页内容按不同样式统计行数，结合顶部YAML Front Matter里的style得到不同样式每行内容所占高度，动态估算。如果超过安全容量边界内，则拆分为几个逻辑小单元各占1页（保证每页内容都不超页面安全容量且布局排版好看情况下，数量应尽可能少）
   2. 判断单块内容所占高度是否超350px：可拆分的（如表格、代码块）将其内部按接近于350px且逻辑自然拆分成多个块。
   3. 当代码一行过长时按逻辑自然插入换行
5. 保存成`show-原名前缀-时间戳`的新md文件。

### 4. 生成口播稿
- 根据展示稿，生成逻辑自然的口播稿：口播稿最开头第一行先加上两个 `---`，换行再写第一页的口播。去掉除分隔符外的乱七八糟的表情图标等仅供展示的念出来不自然的字符。保存成`speaking-原名前缀-时间戳`的新md文件
- 检查口播稿和展示稿的**内容部分**的 `---` 页分隔符数量必须保持一致：用正则表达式执行`grep -E '^---[[:space:]]*$' your_file.md | wc -l`。若数量对不上，无非是在其中一个较少数量的开头或末尾加入单独一行的`---`页分割符，一般是在口播稿最开头加入。
- 这两个保存好的**规范的中间文件名**作为后续调用脚本的参数

### 5. 用Python 脚本一键打包成视频

- 用上述过程中生成的2个中间结果md文件，分别是口播稿和展示稿作为输入，调用执行本skill目录下名为`ai-2md2marp2av.py`，`python ai-2md2marp2av.py 展示稿.md 口播稿.md`

  脚本逻辑为用 Marp 生成的高颜值 PPT 图片后，把图片和 Edge-TTS 生成的语音按对应页数拼起来即可。

### 6. 降级方案

- 若生成视频失败，则调用降级方案：执行本skill目录下名为`md2marp2av.py`脚本。若仍失败则执行本skill目录下名为`md2video.py`脚本