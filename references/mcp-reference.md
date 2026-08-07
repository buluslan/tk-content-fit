# MCP 工具调用手册

> **何时读**:你(或用户)已配置达人精灵 / 卖家精灵 MCP,走自动取数通路时,照此调参。
> **没配 MCP 则跳过本文件** —— skill 全程手填数据也能跑(两条通路平等,见 SKILL.md「数据源」)。
> 配套规范:召回质量校验见 [`listing-frontload.md`](listing-frontload.md) §2;L3 字幕降级见 [`tk-validation.md`](tk-validation.md) §3 L3。

---

## 一、达人精灵(KOLSprite)MCP —— TK 侧

TK 侧商品 / 视频 / 达人 / 店铺 / 字幕全走这一套。分两个 server:`universal`(4 个 search)、`caption`(字幕提取,独立计费)。

### 通用规则(5 工具通用)

- **分页**:`page_num`(默认 1)+ `page_size`(默认 20,最大 100),单次 `page_num × page_size ≤ 1000`。返回结构 `{code, message, data: {page_num, page_size, total, list[]}}`,`code=OK` 成功。
- **命名**:本手册参数一律写 **snake_case**(MCP 实际调用层)。底层 API 文档是 camelCase,经 MCP 封装后转 snake_case(`page_num`/`fans_cnt_from`/`category_list`),别传错。
- **返回字段不裁剪**:universal MCP 不提供字段级收窄(无 projection/returnFields),返回全字段。**控体积靠 `page_size`,不靠字段筛选**。
- **region**:TK 站点国家码(`US`/`GB`/`ID`/`TH`/`VN`/`PH`/`MY`/`SG`/`BR`…),默认 `US`。

### 1. `product_search` — 商品搜索

**用在**:第 2 步主搜词召回校验探针 / L1 商品需求信号。

- **关键入参**:`keyword`、`region`、`search_type`(`K` 关键词 / `N` 商品ID)、`exclude_keyword`、`cat_ids[]`、`price_from`/`price_to`、`sales_volume_lst7d_from`/`sales_volume_lst30d_from`(近7/30天销量)、`sales_volume_gr7d_from`/`gr30d_from`(增长率)、`sales_lst30d_from`(销售额)、`rating`、`reviews`、`influencer`(带货达人数)、`videos`(带货视频数)、`ft`(`fbt` 全托管 / `pop` 自运营)、`lt`(`local` 本地 / `crossborder` 跨境)、`order_field`、`order_type`。
- **返回字段**:
  - 商品:`id` `title` `price` `originalPrice` `catId` `catName` `cnCatName` `region` `link`
  - 销量:`salesVolumeLst7d` `salesVolumeGr7d` `salesVolumeLst30d` `salesVolumeGr30d` `salesLst30d` `totalSalesVolume`
  - 口碑:`rating` `reviews`
  - 带货生态:`influencers`(达人数) `videos`(视频数) `lives`(直播数)
  - 店铺:`shopId` `shopName` `shopSalesVolume` `shopProducts`
  - 履约:`ft` `lt` `listingDate`

> ⚠️ **召回易偏品类**(标题堆词污染):`keyword` 匹配标题不保证类目相关。**主搜词必须走召回校验闭环** —— 探针 → 算「同形态直接竞品数 + 类目一致比例(噪音度)」→ 同形态竞品 <3 换词重生,最多 2 次。完整步骤见 [`listing-frontload.md`](listing-frontload.md) §2。

### 2. `video_search` — 视频搜索

**用在**:L1 内容土壤 + 可拍性终审;L3 降级时的样本来源。

- **关键入参**:`keyword`、`region`、`play_cnt_from`/`to`、`interact_from`/`to`(互动率)、`like_cnt_from`/`to`、`video_type`、`video_ids[]`(≤50)、`pub_date_from`/`to`、`product_category_list[]`、`sales_volume`/`sales_volume_lst30d`、`seconds`(时长)、`product`(是否带货)、`shop_id`、`product_id`、`order_field`(默认近30天销量)。
- **返回字段**:
  - 视频:`id` `title` `cover` `seconds` `pubTime` `region`
  - 互动:`playCnt` `likeCnt` `commentCnt` `collectCnt`(收藏) `forwardCnt`(转发) `interactionRate`
  - 带货:`salesVolumeLst30d` `totalSalesVolume` `salesLst30d` `currency`
  - 创作者:`creator.{id,name,handleName,fansCnt,region,categoryList}`
  - 商品:`product.{id,title,imageUrl,price,catName,cnCatName,salesVolumeLst30d}`

### 3. `creator_search` — 达人搜索

**用在**:L2 达人可获取性 + 两组雏形(内容参考组 / 首轮合作组)。

- **关键入参**:`keyword`、`region`、`search_type`(`N` 昵称 / `K` 关键词)、`fans_cnt_from`/`to`、`play_cnt`(平均播放)、`interact`(互动率)、`video_cnt`、`fans_gr`(粉丝增长率)、`avg_like`、`category_list[]`、`product_category_list[]`(带货分类)、`product_cnt`、`sales_volume`、`order_field`(默认带货销量)。
- **返回字段**:
  - 基础:`id` `name` `uniqueId` `region` `category[]` `lastVideoDate`
  - 体量质量:`fansCnt` `fansGr` `videoCnt` `avgViewsPerVideo` `interactionRate` `avgLikeCnt`
  - 带货:`gpm` `productCnt` `salesVolume` `currency`
  - 带货分类:`goodsCategory[]{catId,catName,cnCatName}`
  - **联系**:`mailAddress`(邮箱) `contactList[]{type,contactAddress}` ← 寄样/合作关键

> ⚠️ **体积防护 · 强制 `page_size ≤ 3`**:creator 单条含 `category[]`/`goodsCategory[]`/`contactList[]` 长数组(约 1.6 万字符/条),`page_size=10` 会返回十多万字符炸上下文。**要大样本就 `page_num++` 翻页,绝不加大 `page_size`**。
>
> ⚠️ **召回后用 `jq` 抽核心字段,别整条读**(`page_size ≤ 3` 仍可能上万字符):`category[]`/`goodsCategory[]`/`contactList[]` 长数组占体积却非判断必需。**先把返回存文件,用 `jq` 抽核心字段再喂判断**:
> ```
> jq '[.data.list[] | {uniqueId,name,fansCnt,interactionRate,salesVolume,mailAddress,contactCnt:(.contactList|length),goodsCategory:[.goodsCategory[]?.catName]}]' raw.json
> ```
> - 核心字段(判断 + 两组雏形够用):`uniqueId`(handle) `name` `fansCnt` `interactionRate` `salesVolume`(带货销量) `mailAddress`(邮箱=可联系) `contactList`(取 length 判有无) `goodsCategory[].catName`(垂直度二次过滤用)。
> - `goodsCategory` 可能为 null → jq 里用 `?` 容错(上面的 `.goodsCategory[]?.catName`)。
>
> ⚠️ **召回 `keyword` 影响弱 → 召回后必须 `goodsCategory[]` 二次过滤**:`keyword` 对召回排序影响弱,召回常按全局带货销量排序、未必贴品类;`category_list` 传中文值不识别(返回 0)。**正解**:召回后按 `goodsCategory[]` 的 `catName`/`cnCatName` 含品类关键词过滤(粉底 → `Foundation`/`粉底`);跨品类 catId 不同,从 raw 召回现场读。过滤后目标达人不足 5 则 `page_num++` 翻页;翻 3 页仍不够或 `goodsCategory` 全空(冷门品类)→ 回退 raw 召回,声明「达人侧未品类验证,L2 结论降权」。完整步骤见 [`tk-validation.md`](tk-validation.md) §3 L2。

### 4. `shop_search` — 店铺搜索

**用在**:L2 内容竞争强度(头部集中度 / 销量归因 / 进入门槛)。

- **关键入参**:`keyword`、`region`、`search_type`(`N` 店铺ID / `K` 关键词)、`shop_type`(卖家类型)、`service_type`(运营模式)、`category[]`、`rating`、`influencer`、`price`(均价)、`sales_volume`/`sales_volume_lst30d`、`sales_volume_gr30d`、`videos`、`products`、`order_field`。
- **返回字段**:
  - 基础:`id` `shopId` `shopName` `region` `link` `categories` `cnCategories` `shopType`(`L` 本地仓 / `C` 跨境) `serviceType`(`S` 自运营 POP / `F` 全托管 FBT)
  - 规模:`products`(在售商品数) `avgPrice` `salesVolumeLst30d` `salesVolumeGr30d` `salesLst30d` `totalSalesVolume`
  - 生态:`influencers`(达人数) `videos`(视频数)
  - 服务:`rating` `respondRate`(24h回复率) `shipRate`(2天发货率) `ratingRate`(4+评分占比)

> ⚠️ **别用纯通用品类词**搜 shop(易召回白牌集合店),改用**辅证词(竞品名)**收窄。
>
> ⚠️ **`shop_search` 返回错误(500 等)的降级 SOP(三档,别直接弹用户手填)**:
> - **档1 自动**:`shop_search`(辅证词/竞品名)正常返回 → 直接用。
> - **档2 复用反推**(0 额外 call):`shop_search` 报错 → 从 L1 已调的 `product_search`/`video_search` 结果里聚合店铺字段(`shopName`/`shopSalesVolume`/`shopProducts`),反推**在售店铺数 / 头部店销量分布 / 有无单店垄断迹象**,标「未验证-降级估算」(样本是 product/video 召回、非 shop 专搜,有偏;拿不到 `respondRate`/`shipRate` 服务指标)。
> - **档3 用户手填**:服务指标(`respondRate`/`shipRate`)也要 → 用户填竞品店铺链接。
> 完整降级链见 [`tk-validation.md`](tk-validation.md) §3 L2。

### 5. `caption_extract_url` — 字幕提取(独立计费 · 可选 · 默认不触发)

**用在**:L3 脚本级内容方向(钩子/痛点/演示/卖点/证明/异议/CTA)。

- **入参**:`url`(TikTok 视频长链接 `tiktok.com/@handle/video/{id}`)+ `is_southeast_asia`(bool,**东南亚视频 ID/TH/VN/PH/MY/SG 传 `true`,其余默认 `false`**)。
- **返回**:字幕全文 + 时间戳 + 原语言(成功返回 `[{sort, start_time, end_time, text}]` 数组)。单视频上限 **5 分钟**。
- **全地区可用,个别视频失败属偶发**(视频被删/限流等视频级原因,非区域级):单视频失败 → 换 L1 召回列表下一个视频重试;**连续多个(如 3 个)失败才走两档降级**(见 [`tk-validation.md`](tk-validation.md) §3 L3)。

---

## 二、卖家精灵(SellerSprite)MCP —— 亚马逊侧(可选)

第 1 步「需求锚定」的**增强层**,非必填。用户没配则走手填,核心流程不受影响。

| 工具 | 用途 |
|------|------|
| `asin_detail` | 单 ASIN 详情(标题/类目/价格/评分/评论/变体/卖家/配送) |
| `asin_prediction` | 单 ASIN 销量预测(近14个月,日/月维度,销量·销售额·价格·BSR) |
| `competitor_lookup` | 亚马逊商品列表(按品牌/卖家/ASIN/类目/关键词筛选,返回销量·销售额·BSR·价格·评分) |
| `review` | 单 ASIN 评论列表(标题/内容/评分/时间;可按 `starList`/`typeList` 筛选)→ **评论 UGC 聚类原料**,筛选与聚类规则见 [`listing-frontload.md`](listing-frontload.md) §1 |

> 参数与字段以卖家精灵官方文档为准(独立计费,与达人精灵不同钱包)。skill 第 1 步用它取 **Listing 结构化详情 + 销量/BSR 预测 + 竞品列表 + 评论原文(UGC 聚类)**;缺它则用户手填 Listing(必给输入)或粘贴热评,销量/BSR/竞品字段标「⚠️ 假设」,结论降置信度。

---

## 三、计费规则(规则不写价格)

- **达人精灵 Data Search MCP(universal)**:按**调用次数(call)**计费,4 个 search 工具同价(1 次调用 = 1 call);有**每分钟频率限制** + **月度 call 配额**;超额需续费;无公开免费层。**具体价格/配额以 kolsprite 官网(kolsprite.com)实时为准,会变动,不在 skill 内写死。**
- **caption(Video Script MCP)**:**独立 credit 计费**,每次成功解析 = 1 credit(**失败不计费**),与 search 是不同钱包。具体档位见官网。
- **错误码自检**:`ERROR_API_MONTH_MAX`(月额度用完)/ `ERROR_API_MINUTE_MAX`(超频率)/ `ERROR_API_KEY_EXPIRE`(key 过期)/ `ERROR_API_KEY_INVALID`(无效)—— 命中即提示用户查额度/续费,并走对应降级。
- **卖家精灵**:用户的独立付费工具,按其自有套餐计费,skill 不另计。

> skill 不替用户管额度。call 前给估算(每层 call 量见 [`tk-validation.md`](tk-validation.md) 各层「call 量」),用户自行决定跑到哪层。

---

## 四、指针

| 要做的事 | 去哪 |
|---------|------|
| product 主搜词召回校验(探针→同形态竞品数+噪音度→重生) | [`listing-frontload.md`](listing-frontload.md) §2 |
| creator 召回后 goodsCategory 二次过滤 + 两组雏形 | [`tk-validation.md`](tk-validation.md) §3 L2 |
| caption 连续失败的两档降级(自贴字幕→脚本级 / video 字段归纳→方向级) | [`tk-validation.md`](tk-validation.md) §3 L3 |
| 数据规范(口径/时间窗/未验证标注,8 条) | SKILL.md「通用规则 · 数据规范」 |
