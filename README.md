# 简易RAG的探索
我们可以把单纯的大模型（如 ChatGPT）看作是一个正在参加**‘闭卷考试’的学生。他只能靠脑子里的死记硬背来回答，忘了就瞎编。
而 RAG 系统，就是让这个学生改为参加**‘开卷考试’**。

RAG 数据准备流程（Retrieval 数据链路）

1. 文档收集（Data Collection）
收集所有需要让模型检索的资料
（教材、论文、产品文档、内部报告、网页内容等）
将不同格式统一转换为纯文本（PDF / doc / HTML → text）

2. 文档清洗（Cleaning）
去除噪声：广告、页脚、目录、脚注、乱码
规范文本结构：统一段落、标题、列表格式
目的：得到干净、结构化、可切分的文本

3. 文本切分（Chunking）
将长文档拆成较小的文本块（如 200–500 tokens）
使用 重叠切分（Overlap 20%–50%）避免语义被切断
每个 chunk 都保留 metadata（来源、章节、页码等）

4. 文档向量化（Embedding）
通过 embedding 模型将每个 chunk 转成向量
可用模型：Sentence-BERT / OpenAI Embedding / BGE 等
向量的目标：语义相似的内容距离更近

5. 建立向量索引（Indexing）
将所有向量存入向量数据库
FAISS / Milvus / Pinecone / Weaviate
提供高效的相似度搜索（Top-K）
用于 RAG 检索阶段快速找到相关文本块

下面是rag的检索部分
用户问题（Query）
        ↓
Query 向量化（Embedding）
        ↓
向量数据库检索（Top-K 相似度搜索）
        ↓
找到最相关的文本块（Relevant Chunks）
        ↓
内容过滤 / 排序 / 去重（Optional）
        ↓
将检索结果加入提示（Augmented Prompt）
        ↓
送入大模型生成答案（LLM Generation）
