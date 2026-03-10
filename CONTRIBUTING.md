# Contributing Guide / 贡献指南

Thanks for your interest in contributing to Fund Estimation System.

感谢你参与 Fund Estimation System 的建设。

## 1) Workflow / 提交流程

1. Fork this repository
2. Create a branch from `main`
3. Make your changes
4. Add/adjust tests when needed
5. Ensure checks pass
6. Open a Pull Request

```bash
git checkout -b feat/your-change
npm test
npm run build
```

## 2) Commit Style / 提交规范

Use Conventional Commits where possible:

- `feat: ...`
- `fix: ...`
- `docs: ...`
- `refactor: ...`
- `test: ...`

## 3) Code Expectations / 代码要求

- Keep TypeScript strict and readable
- Avoid breaking existing API contracts unless discussed first
- Keep UI consistent with design tokens
- Update docs for user-facing changes

## 4) Pull Request Checklist / PR 检查清单

- [ ] PR description explains purpose and scope
- [ ] `npm test` passes
- [ ] `npm run build` passes
- [ ] No secrets or local machine paths added
- [ ] README/docs updated when behavior changes

## 5) Reporting Issues / 问题反馈

When opening an issue, please include:

- Expected behavior / 期望行为
- Actual behavior / 实际行为
- Repro steps / 复现步骤
- Logs or screenshots if available / 日志或截图
- Environment (OS, Node version) / 环境信息

## 6) Security / 安全问题

Please do not open public issues for sensitive vulnerabilities.
For security concerns, contact the maintainer privately first.

涉及敏感漏洞请勿直接公开提交 Issue，建议先私下联系维护者。
