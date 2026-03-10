# Fund Estimation System / 基金实时估值系统

A local real-time fund estimation system with adaptive Web + Mobile UI.

一个本地运行的基金实时估值系统，支持 Web + Mobile 自适应界面。

---

## Overview / 项目简介

**EN**: This project restores same-day fund estimation workflows for users after many platforms removed that feature. It is designed for local single-user usage with fast setup.

**中文**：本项目用于在基金平台下线“当日估值”后，为用户提供本地实时估值参考，默认面向单用户本地部署。

> Disclaimer / 免责声明: This project is for informational/demo use only and is **not** investment advice.  
> 本项目仅用于信息展示与演示，不构成投资建议。

---

## Features / 功能特性

- **Real-time refresh / 实时刷新**: 60s collection + UI update during CN market sessions.
- **Watchlist / 自选基金**: Add/remove 6-digit fund codes.
- **Search & Sort / 搜索与排序**: default / deltaDesc / deltaAsc.
- **Grouping / 分组管理**: CRUD groups, reorder, multi-group mapping, ungrouped view.
- **Position snapshot / 持仓快照**: shares + cost per unit with market value, total PnL, daily PnL.
- **History analysis / 历史分析**: 7D / 30D / 90D / 180D / 1Y return and drawdown.
- **Intraday chart / 日内曲线**: fixed trading-time X-axis, dynamic Y-axis.
- **Operational UX / 状态反馈**: empty/no-result/error/stale/loading states.
- **One-click launch / 一键启动**: macOS and Windows bootstrap scripts.

---

## Tech Stack / 技术栈

- Next.js App Router + React 19 + TypeScript
- Next.js Route Handlers (Node runtime)
- SQLite (`better-sqlite3`)
- In-process collector scheduler (60s)
- EastMoney-based fund data source (compatible with AkShare source lineage)

---

## Quick Start (Developers) / 开发者快速开始

### 1) Requirements / 环境要求

- Node.js 22+
- npm 10+

### 2) Install & Run / 安装运行

```bash
npm install
npm run dev
```

Open / 访问: [http://localhost:3000](http://localhost:3000)

### 3) Test & Build / 测试与构建

```bash
npm test
npm run build
npm run start
```

---

## One-Click Start (Users) / 普通用户一键启动

### macOS

Double click / 双击:

- `scripts/start-mac.command`

### Windows

Double click / 双击:

- `scripts/start-windows.bat`

Bootstrap scripts will / 启动脚本会自动：

- install Node/npm when missing / 缺失时自动安装 Node/npm
- install dependencies / 安装依赖
- build and launch service / 构建并启动服务
- open browser at `http://localhost:3000` / 自动打开浏览器

---

## Runtime Rules / 运行规则

- Timezone / 时区: `Asia/Shanghai`
- Trading sessions / 交易时段:
  - `09:30-11:30`
  - `13:00-15:00`
- Collector interval / 采集间隔: `60s`
- Stale threshold / 延迟阈值: `180s`
- Retry delays / 重试回退: `2s`, `5s`
- Retention / 数据保留: intraday points for `30 days`（日内点位保留 30 天）

---

## Environment Variables / 环境变量

- `DATABASE_PATH` (optional / 可选)
  - default: `./fund-valuation.db`

---

## API (v1)

### Funds

- `GET /api/v1/funds?query=&sortBy=&groupId=`
- `POST /api/v1/funds`
  - body: `{ "code": "161725", "groupIds": ["<groupId>"] }`
- `DELETE /api/v1/funds/{code}`
- `PUT /api/v1/funds/{code}/groups`
  - body: `{ "groupIds": ["<groupId>"] }`
- `GET /api/v1/funds/{code}/trend?date=YYYY-MM-DD`
- `GET /api/v1/funds/{code}/history?range=7D|30D|90D|180D|1Y`

### Position

- `PUT /api/v1/funds/{code}/position`
  - body: `{ "shares": 1280.5, "costPerUnit": 2.18 }`
- `DELETE /api/v1/funds/{code}/position`

### Groups

- `GET /api/v1/groups`
- `POST /api/v1/groups`
  - body: `{ "name": "半导体" }`
- `PATCH /api/v1/groups/{id}`
  - body: `{ "name": "半导体精选" }`
- `DELETE /api/v1/groups/{id}`
- `PUT /api/v1/groups/reorder`
  - body: `{ "ids": ["idA", "idB"] }`

### System

- `GET /api/v1/system/status`

---

## Project Structure / 项目结构

```text
app/
  api/v1/...                 # REST API routes
  globals.css                # Global tokens/styles
components/
  dashboard.tsx              # Main dashboard
  sparkline.tsx              # Intraday chart
  history-line-chart.tsx     # History analysis chart
lib/
  db.ts                      # SQLite schema + queries
  collector.ts               # 60s collector + retry
  provider/eastmoney.ts      # Data provider
scripts/
  start-mac.command
  start-windows.bat
  bootstrap-and-start.sh
  bootstrap-and-start.ps1
tests/
  *.test.ts                  # Unit + integration tests
fund-valuation-demo.pen      # Pencil high-fidelity design file
```

---

## Contributing / 参与贡献

Please read [CONTRIBUTING.md](./CONTRIBUTING.md) before opening a PR.  
提交 PR 前请先阅读 [CONTRIBUTING.md](./CONTRIBUTING.md)。

---

## Roadmap / 路线图

- [ ] Docker deployment support / Docker 部署支持
- [ ] Optional SSE/WebSocket mode / 可选 SSE/WebSocket 推送
- [ ] finshare extension for richer ETF/LOF data / finshare 增强 ETF/LOF 数据
- [ ] Multi-user auth / 多用户与权限
- [ ] Advanced portfolio analytics / 更完整的持仓分析能力

---

## License / 开源许可证

MIT License. See [LICENSE](./LICENSE).
