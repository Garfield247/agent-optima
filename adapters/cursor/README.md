# Cursor & Windsurf 适配器

## 核心配置文件清单
- `.cursor/rules/*.mdc`: 现代模块化规则（支持 `globs` 条件按需加载，防常驻 Token 浪费）；
- `.cursorrules`: 根目录轻量总纲。

## 核心特性
- `alwaysApply: false` 按需加载规约（实现真正的渐进式披露）；
- 符号级索引调阅，禁止整文件全量加载进 Composer 上下文；
- 原子级代码 diff 替换。
