# Rust Template

一个开箱即用的 Rust 项目模板：固定工具链、提交前检查、CI 与自动发版。

## 包含什么

| 文件 | 作用 |
| --- | --- |
| `rust-toolchain.toml` | 固定工具链版本并声明 `rustfmt` / `clippy` 组件，本地与 CI 用同一个编译器 |
| `Cargo.toml` | edition 2024，声明 `rust-version`（MSRV），`[lints]` 中 deny `unsafe_code` |
| `.pre-commit-config.yaml` | 格式化、lint、测试、依赖检查、拼写检查 |
| `.github/workflows/build.yml` | 三个 job：构建检查、MSRV 验证、tag 触发的发版 |
| `.github/dependabot.yml` | 跟踪 action、pre-commit hook、工具链与依赖的版本更新 |
| `deny.toml` | 依赖的许可证与安全公告策略 |
| `cliff.toml` | 由 commit 生成 changelog |

## 使用

```bash
git clone https://github.com/wmjim/rust-template
cd rust-template
```

### 开发

工具链无需手动安装：进入仓库后 rustup 会按 `rust-toolchain.toml` 自动装好固定版本及其组件。

```bash
cargo run
cargo nextest run --all-features   # 或 cargo test
cargo fmt
cargo clippy --all-targets --all-features -- -D warnings
```

### 提交前检查

```bash
pipx install pre-commit   # 或 pip install pre-commit
pre-commit install
```

### 依赖检查

```bash
cargo install --locked cargo-deny
cargo deny check
```

### 生成 changelog

```bash
cargo install git-cliff
git-cliff -o CHANGELOG.md
```

## 发版

推一个 `v*` 形式的 tag 即可：CI 会跑完构建检查，再用 git-cliff 生成本次 changelog 作为 GitHub Release 的内容。

## License

MIT
