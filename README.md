# 淘宝天猫商品信息爬取RPA机器人 / Taobao Tmall Product Information Crawling RPA Robot
*自动化商品信息爬取工具 - 为企业采购决策提供数据支持 / Automated Product Information Crawling Tool - Providing Data Support for Enterprise Procurement Decisions*

## 项目概述 / Project Overview

此RPA机器人使用**实在智能RPA平台**开发，专门用于爬取淘宝天猫平台的商品信息。通过与企业内部商品价格数据进行对比分析，为企业采购提供价格参考和选品建议。
This RPA robot is developed using the **Real Intelligence RPA Platform** and is specifically designed to crawl product information from the Taobao and Tmall platforms. By comparing and analyzing the data against internal enterprise product price data, it provides price references and product selection suggestions for procurement.

### 爬取数据字段 / Data Fields Crawled
| 字段名<br>Field Name | 说明<br>Description | 字段名<br>Field Name | 说明<br>Description |
|----------|--------------|--------|------------|
| qdpt     | 渠道平台<br>Sales Channel Platform | sj     | 售价<br>Selling Price |
| pp       | 品牌<br>Brand | yj     | 原价<br>Original Price |
| lb       | 类别<br>Category | xsl    | 销售量<br>Sales Volume |
| spmc     | 商品名称<br>Product Name | pjs    | 评价数<br>Number of Reviews |
| gg       | 规格<br>Specifications | dw     | 单位<br>Unit |

## 技术特点 / Technical Features

### 反爬策略解决方案 / Anti-Crawling Strategy Solutions
- ✅ 已解决直播窗口突然弹出的干扰问题 /  Resolved interference from sudden pop-up live stream windows
- ✅ 已处理界面按钮位置偏移的识别问题 /  Addressed the issue of identifying UI buttons with positional offsets
- ✅ 针对特殊页面设计：无法识别的元素默认设为0，确保流程持续执行 / Designed for special pages: Unrecognizable elements are defaulted to 0 to ensure process continuity

## 工作流程 / Workflow
# 淘宝天猫商品爬取RPA流程图 / Taobao Tmall Product Crawling RPA Flowchart


```mermaid
graph TD
    A([开始 / Start]) --> B[打开淘宝首页 / Open Taobao Homepage]
    B --> C{检测登录状态 / Check Login Status}
    C -->|已登录 / Logged In| D[执行登出 / Perform Logout]
    C -->|未登录 / Not Logged In| E[执行登录 / Perform Login]
    D --> E
    E --> F[连接数据库 / Connect to DB]
    F --> G[执行SQL查询 / Execute SQL Query]
    G[SELECT DISTINCT 品牌 FROM spzd] --> H[提取品牌列表 / Extract Brand List]
    H --> I[关闭数据库连接 / Close DB Connection]
    I --> J[循环品牌列表 / Loop Brand List]
    
    J --> K[搜索品牌旗舰店 / Search Brand Flagship Store]
    K --> L{检测直播弹窗? / Detect Live Popup?}
    L -->|是 / Yes| M[关闭弹窗 / Close Popup]
    L -->|否 / No| N[进入店铺 / Enter Store]
    M --> N
    
    N --> O[获取店铺商品列表 / Get Store Product List]
    O --> P[循环商品列表 / Loop Product List]
    P --> Q[进入商品详情页 / Enter Product Detail Page]
    Q --> R{检测详情页弹窗? / Detect Detail Page Popup?}
    R -->|是 / Yes| S[关闭弹窗 / Close Popup]
    R -->|否 / No| T[获取规格列表 / Get Specification List]
    S --> T
    
    T --> U[循环规格列表 / Loop Specification List]
    U --> V[选择规格 / Select Specification]
    V --> W[爬取价格数据 / Crawl Price Data]
    W --> X[保存到临时表 / Save to Temp Table]
    X --> U
    U -->|规格循环结束 / Spec Loop End| Y[返回店铺页 / Return to Store Page]
    Y --> P
    P -->|商品循环结束 / Product Loop End| Z[返回搜索页 / Return to Search Page]
    Z --> J
    
    J -->|品牌循环结束 / Brand Loop End| AA[连接数据库 / Connect to DB]
    AA --> AB[读取临时表数据 / Read Temp Table Data]
    AB --> AC[循环每行数据 / Loop Each Data Row]
    AC --> AD[提取字段值 / Extract Field Values]
    AD --> AE[插入数据库 / Insert into DB]
    AE --> AC
    AC -->|数据处理完成 / Data Processing Complete| AF[关闭数据库连接 / Close DB Connection]
    AF --> AG([结束 / End])
    
    %% 异常处理流程 / Exception Handling Process
    subgraph 异常处理 / Exception Handling
        direction LR
        E1[流程执行 / Process Execution] --> E2{是否出错? / Error?}
        E2 -->|是 / Yes| E3[记录异常 / Log Exception]
        E3 --> E4[继续执行 / Continue Execution]
        E2 -->|否 / No| E5[正常流程 / Normal Flow]
    end
    
    W --> E2
    AD --> E2
    G --> E2
    F --> E2
```

## 流程图说明 / Flowchart Explanation

### 1. 初始化阶段 / Initialization Phase

```mermaid
graph LR
    A[开始 / Start] --> B[打开淘宝首页 / Open Taobao Homepage]
    B --> C{登录检测 / Login Check}
    C --> D[登出 / Logout]
    C --> E[登录 / Login]
    D --> E
    E --> F[连接数据库 / Connect to DB]
    F --> G[查询品牌列表 / Query Brand List]
    G --> H[提取品牌数据 / Extract Brand Data]
    H --> I[关闭连接 / Close Connection]
```

### 2. 爬取主循环 / Main Crawling Loop

```mermaid
graph TB
    J[循环品牌 / Loop Brands] --> K[搜索旗舰店 / Search Flagship Store]
    K --> L{弹窗检测 / Popup Detection}
    L --> M[关闭弹窗 / Close Popup]
    L --> N[进入店铺 / Enter Store]
    M --> N
    N --> O[获取商品列表 / Get Product List]
    O --> P[循环商品 / Loop Products]
    P --> Q[进入详情页 / Enter Detail Page]
    Q --> R{弹窗检测 / Popup Detection}
    R --> S[关闭弹窗 / Close Popup]
    R --> T[获取规格 / Get Specifications]
    S --> T
    T --> U[循环规格 / Loop Specifications]
    U --> V[选择规格 / Select Spec]
    V --> W[爬取数据 / Crawl Data]
    W --> X[保存数据 / Save Data]
    X --> U
```

### 3. 数据存储阶段 / Data Storage Phase

```mermaid
graph LR
    AA[连接数据库 / Connect to DB] --> AB[读取临时表 / Read Temp Table]
    AB --> AC[循环数据行 / Loop Data Rows]
    AC --> AD[提取字段 / Extract Fields]
    AD --> AE[插入数据库 / Insert into DB]
    AE --> AC
    AC --> AF[关闭连接 / Close Connection]
    AF --> AG[结束 / End]
```

### 4. 异常处理机制 / Exception Handling Mechanism

```mermaid
graph LR
    E1[执行操作 / Execute Operation] --> E2{是否出错 / Error?}
    E2 -->|是 / Yes| E3[记录异常 / Log Exception]
    E3 --> E4[继续执行 / Continue]
    E2 -->|否 / No| E5[正常流程 / Normal Flow]
```

## 技术要点说明 / Key Technical Points

1.  **三层循环结构 / Three-Layer Loop Structure**：
    -   外层：品牌循环（从数据库获取） / Outer: Brand loop (from database)
    -   中层：商品循环（店铺页面） / Middle: Product loop (store page)
    -   内层：规格循环（商品详情页） / Inner: Specification loop (product detail page)

2.  **弹窗处理机制 / Popup Handling Mechanism**：
    -   直播弹窗检测 / Live stream popup detection
    -   详情页弹窗检测 / Detail page popup detection
    -   位置偏移容错处理 / Position offset fault tolerance

3.  **数据流设计 / Data Flow Design**：
    ```mermaid
    graph LR
        DB1[(cj_rw_spzd表 / Table)] -->|品牌数据 / Brand Data| RPA
        RPA -->|临时数据 / Temp Data| DataFrame
        DataFrame -->|最终数据 / Final Data| DB2[(cj_rw_spxx表 / Table)]
    ```

4.  **关键SQL操作 / Key SQL Operations**：
    ```sql
    -- 获取品牌列表 / Get brand list
    SELECT DISTINCT spmc FROM cj_rw_spzd;
    
    -- 插入爬取数据 / Insert crawled data
    INSERT INTO cj_rw_spxx (qdpt,pp,lb,spmc,gg,dw,sj,yj,xsl,pjs,rq,sj)
    VALUES (@qdpt, @pp,@lb, @spmc, @gg, @dw,@sj, @yj, @xsl, @pjs,CONVERT(varchar(100), GETDATE(), 23),CONVERT(varchar(100), GETDATE(), 108));
    ```

## 使用说明 / Usage Instructions

1.  **环境准备 / Environment Setup**：
    -   安装实在智能RPA平台 / Install Real Intelligence RPA Platform
    -   Chrome浏览器（最新版） / Chrome Browser (Latest Version)
    -   Python 3.8+ 环境 / Python 3.8+ Environment

2.  **运行机器人 / Run the Robot**：
    -   在实在智能RPA平台导入项目 / Import the project in the Real Intelligence RPA Platform
    -   配置数据库连接参数 / Configure database connection parameters
    -   执行主流程文件 / Execute the main process file

## 注意事项 / Important Notes

1.  请遵守淘宝天猫平台的爬虫协议 / Please comply with the crawling policies of Taobao and Tmall platforms.
2.  合理设置爬取间隔，避免对目标网站造成过大压力 / Set reasonable crawling intervals to avoid excessive pressure on the target website.
3.  定期更新元素定位器，以应对网站改版 / Regularly update element selectors to adapt to website redesigns.
4.  敏感信息（如数据库凭证）请使用环境变量管理 / Manage sensitive information (e.g., database credentials) using environment variables.

## 维护与支持 / Maintenance & Support

如遇问题，请提交Issue或联系： / If you encounter any problems, please submit an Issue or contact:
-   维护者：smytz6@163.com / Maintainer: smytz6@163.com
-   最后更新时间：2025-8-24 / Last Updated: 2025-8-24

---
