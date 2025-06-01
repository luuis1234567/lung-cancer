# 肺癌单细胞转录组全流程分析（GSE131907）

本项目以GSE131907单细胞转录组公开数据为对象，基于R语言及Seurat/Monocle/CopyKAT/CellChat等主流生信包，系统开展了肿瘤组织单细胞数据从预处理、质控到分群、亚群功能富集、拷贝数变异与细胞通讯等综合性分析。适合科研复现/方法学习/课程教学。

---

## 目录

- [项目简介](#项目简介)
- [依赖环境](#依赖环境)
- [数据下载](#数据下载)
- [分析流程总览](#分析流程总览)
- [核心模块说明](#核心模块说明)
- [主要输出结果](#主要输出结果)

---

## 项目简介

本分析以肺癌（GSE131907）单细胞转录组数据为例，完整覆盖：
- 基础质控、降维、分群、注释、marker基因
- 差异表达与功能富集分析（GO/KEGG）
- 亚群轨迹/发育拟时序
- 肿瘤/非瘤细胞拷贝数变异（CopyKAT）
- 细胞亚群通讯（CellChat）
- 稀有细胞发现（RaceID）

---

## 依赖环境

- **R ≥ 4.1**
- 推荐IDE: [RStudio](https://posit.co/download/rstudio-desktop/)
- 主要R包及安装方法：
  ```r
  install.packages(c("dplyr", "ggplot2", "patchwork"))
  BiocManager::install(c(
      "Seurat", "SeuratData", "clusterProfiler", "org.Hs.eg.db",
      "monocle3", "copykat", "CellChat", "RaceID"
  ))
  # 部分包需开发版或额外依赖，详见各包官网或CRAN/Bioconductor说明

## 数据下载

- 前往https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE131907
- 分别下载GSE131907_Lung_Cancer_cell_annotation.txt.gz和GSE131907_Lung_Cancer_raw_UMI_matrix.rds.gz
- 并解压

## 分析流程总览

1. **数据读取与预处理**：抽样、质控、注释、Seurat对象构建
2. **降维与分群**：PCA/UMAP，细胞聚类与可视化
3. **亚群统计与marker基因**：细胞类型分布、marker基因筛选与热图
4. **功能富集分析**：GO/KEGG富集、富集气泡图
5. **发育轨迹/拟时序分析**（Monocle3）
6. **肿瘤细胞拷贝数变异分析**（CopyKAT）
7. **细胞通讯分析**（CellChat）
8. **稀有亚群发现**（RaceID）
9. **全部分析结果导出/保存**

------

## 核心模块说明

### 1. 基础分析（Seurat为主）

- 读取原始表达矩阵，抽样细胞，加载注释，构建Seurat对象
- 质控指标（线粒体比例、基因数等）筛选高质量细胞
- 标准化、变量基因筛选、降维（PCA/UMAP），聚类分群
- 可视化各亚群分布（DimPlot、饼图、条形图等）
- Marker基因筛选（FindAllMarkers/FindMarkers），绘制热图/气泡图
- GO/KEGG富集分析，气泡图展示主要功能条目

### 2. 发育轨迹/拟时序（Monocle3）

- Seurat对象转Monocle3对象
- 构建trajectory graph，指定root细胞，拟时序排序与可视化
- 直观呈现细胞分化发展脉络

### 3. 细胞通讯分析（CellChat）

- 构建CellChat对象，选择人类/小鼠数据库
- 计算/展示不同细胞亚群间通讯通路（如MIF）
- 气泡图展示通路活跃亚群、发送/接收角色等

### 4. 拷贝数变异分析（CopyKAT）

- 随机抽样细胞表达量（降低计算压力）
- 运行CopyKAT，自动区分恶性细胞与正常细胞，结果整合进Seurat元数据
- 统计并可视化各亚群中恶性/正常细胞比例，辅助肿瘤分群生物学解释
- 对恶性与正常细胞做差异分析

### 5. 稀有亚群发现（RaceID）

- 构建RaceID对象，对表达矩阵进行高维聚类、tSNE降维
- 识别极少量/特征独特的稀有细胞亚群
- 输出稀有亚群细胞列表和概率，可进一步生物学解释

------

## 主要输出结果

- 分群/亚群UMAP图、拟时序轨迹图、通讯网络气泡图等
- Marker基因表、GO/KEGG富集表
- CopyKAT肿瘤/正常细胞分布统计及分群比例图
- RaceID稀有亚群识别结果、tSNE图
- 所有主要分析结果导出为csv/txt/png等格式
