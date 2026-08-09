# 亲友理财模拟娱乐沙盘展示站

私人熟人记账看板 · 非金融产品 · 非集资平台 · 前端只读 + JSON 管理

## 目录结构

```
sandbox-dashboard/
├── index.html                 # 公开只读前端（博客可嵌入）
├── data/sandbox.json          # 数据源（改这个即可同步前端）
├── admin-x7k9m2/index.html    # 私密管理端（勿外链、勿写进博客导航）
├── docs/亲友娱乐合伙协议.md
└── README.md
```

## 本地预览（必须用 HTTP，勿 file://）

在 `sandbox-dashboard` 目录：

```powershell
cd C:\Users\ccf\Desktop\hugo-github-pages-blog\sandbox-dashboard
python -m http.server 8765
```

| 页面 | 地址 |
|------|------|
| 前端展示 | http://127.0.0.1:8765/ |
| 管理后台（通俗版） | http://127.0.0.1:8765/admin-x7k9m2/ |
| 默认口令 | `sandbox2026`（登入后请立即更换） |

后台已按「步骤 1～6」拆开，每个输入框下方都有「作用：……」说明。改完只记一件事：点 **① 保存成数据文件**，再上传覆盖 `data/sandbox.json`。

管理端「写入本机预览」后，前端用：http://127.0.0.1:8765/?preview=1

## 如何改数据并同步

### 方式 A（推荐）：改 JSON 上传

1. 打开管理端 → 成员/投资/分红/公告编辑  
2. 点 **导出 JSON**，得到 `sandbox.json`  
3. 覆盖上传到服务器 `data/sandbox.json`  
4. 前端每 30 秒自动 `fetch` 刷新；也可手动刷新页面  

### 方式 B：直接编辑 JSON

用编辑器打开 `data/sandbox.json`，按字段修改后保存上传即可。  
仓位占比、个人市值、浮动盈亏由前端按公式自动算：

`个人仓位 = 个人娱乐本金 ÷ 总娱乐本金`  
`个人持仓市值 = 当前总资产 × 个人仓位`  
`浮动盈亏 = (总资产 − 投资投入合计) × 个人仓位`

## 嵌入 Hugo / 博客

### 方案 1：整目录丢进 `static`

复制整个 `sandbox-dashboard` 到 Hugo 站点：

`dev/static/sandbox-dashboard/`

线上访问（示例）：

`https://2004ccf.github.io/io/sandbox-dashboard/`

**注意：** 不要把 `admin-x7k9m2` 写进菜单；需要管理时用完整私密 URL。也可只部署前端，管理端留在本机。

### 方案 2：iframe 嵌入某篇文章

```html
<iframe src="/sandbox-dashboard/" style="width:100%;min-height:1400px;border:0;"></iframe>
```

## 合规用词（已全局规避）

| 禁止 | 替换 |
|------|------|
| 基金 / 募资 / 投资人 / 入股 / 保本收益 | 沙盘仓位 / 娱乐本金 / 玩伴 / 沙盘分成 / 模拟仓位 |

固定标语：熟人小额理财模拟娱乐看板，仅用于记账对账，不存在募资理财行为

## 风控内置

- 人数 ≤ 15；单人出资与总额不设金额上限（`maxPerPerson`/`maxTotal` 为 0 表示不限制）  
- 前端无充值、转账、注册、招募按钮  
- 置顶免责声明不可关闭  
- 退伙说明见公告与协议文档  

## 更换管理口令

1. 本地计算新口令的 SHA-256（PowerShell / Python）  
2. 替换 `admin-x7k9m2/index.html` 内 `PASS_HASH`  
3. 删除或改掉 `FALLBACK_PLAIN`  
4. 重新上传管理端页面  

```python
import hashlib
print(hashlib.sha256(b"你的新口令").hexdigest())
```

## 交付清单

- [x] 前端单页 HTML + ECharts 双环图 + 净值折线 + 查询 + 台账/流水  
- [x] JSON 数据模板 `data/sandbox.json`  
- [x] 简易管理后台（口令门禁、成员/投资/分红/公告/导出）  
- [x] 《亲友娱乐合伙协议》  
- [x] 本部署说明  
