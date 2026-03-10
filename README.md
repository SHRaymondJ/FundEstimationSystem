# Fund Estimation System

一个面向中国基金用户的本地实时估值系统（Web + Mobile 自适应）。

> 目标：替代基金平台已下线的“当日估值”能力，帮助用户在买入/卖出前做更直观的盘中参考。

## Features

- 实时估值：按 60 秒周期刷新（交易时段）
- 自选基金：添加/删除 6 位基金代码
- 搜索 + 排序：默认 / 涨幅优先 / 跌幅优先
- 手动分组：分组 CRUD、重排、多分组归属、未分组视图
- 持仓快照：录入份额与成本，展示持仓市值、累计盈亏、当日盈亏
- 历史分析：7D / 30D / 90D / 180D / 1Y 区间收益与回撤
- 日内走势：固定交易时段时间轴（含中间刻度），动态 Y 轴
- 状态反馈：空态、无结果、错误、延迟、局部刷新提示
- 跨平台一键启动：macOS / Windows 自动安装依赖并启动

## Tech Stack

- Frontend: Next.js App Router, React 19, TypeScript
- Backend: Next.js Route Handlers (Node runtime)
- Database: SQLite (`better-sqlite3`)
- Scheduler: in-process collector loop (60s)
- Data source: EastMoney（与 AkShare `fund_value_estimation_em` 同源）

## Quick Start (Developer)

### 1) Requirements

- Node.js 22+
- npm 10+

### 2) Install & Run

```bash
npm install
npm run dev
```

Open: [http://localhost:3000](http://localhost:3000)

### 3) Test & Build

```bash
npm test
npm run build
npm run start
```

## One-Click Start (Non-Technical Users)

### macOS

Double click:

- `scripts/start-mac.command`

### Windows

Double click:

- `scripts/start-windows.bat`

These launchers will:

- auto-install Node/npm when missing
- install project dependencies
- build and start service
- open browser on `http://localhost:3000`

## Runtime Behavior

- Timezone: `Asia/Shanghai`
- Collection window:
  - 09:30-11:30
  - 13:00-15:00
- Refresh interval: 60s
- Stale threshold: 180s (front-end shows delay hint)
- Retry on failures: 2s, then 5s
- Data retention: intraday points kept for 30 days

## Environment Variables

- `DATABASE_PATH` (optional)
  - default: `./fund-valuation.db`

## API Overview (v1)

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

## Project Structure

```text
app/
  api/v1/...                 # REST API routes
  globals.css                # UI tokens and global styles
components/
  dashboard.tsx              # Main dashboard page
  sparkline.tsx              # Intraday chart
  history-line-chart.tsx     # History analysis chart
lib/
  db.ts                      # SQLite schema + queries
  collector.ts               # 60s collector + retries
  provider/eastmoney.ts      # Data provider adapters
  trading.ts                 # Trading session logic
scripts/
  start-mac.command
  start-windows.bat
  bootstrap-and-start.sh
  bootstrap-and-start.ps1
tests/
  *.test.ts                  # unit + integration tests
fund-valuation-demo.pen      # Pencil high-fidelity design file
```

## Roadmap

- [ ] Docker image + compose deployment
- [ ] Optional SSE/WebSocket push
- [ ] finshare integration for richer ETF/LOF minute data
- [ ] Multi-user auth & access control
- [ ] More portfolio analytics (cost averaging, cashflow-based PnL)

## Contributing

欢迎提交 Issue / PR：

1. Fork this repo
2. Create feature branch
3. Add or update tests
4. Ensure `npm test` and `npm run build` pass
5. Open Pull Request

## Disclaimer

本项目仅用于信息展示与研究演示，不构成任何投资建议。基金投资有风险，决策请基于你自己的独立判断。

## License

当前仓库尚未添加正式开源许可证。

如果你计划公开分发或商用，建议先补充 `LICENSE`（如 MIT/Apache-2.0）。
