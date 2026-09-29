# Rust Template

> 本 README 描述的是模板本身。生成项目后请换成你自己项目的说明。
>
> 本模板基于 [tyr-rust-bootcamp/template](https://github.com/tyr-rust-bootcamp/template) 继续维护，上游 master 自 2024-03 起没有新提交。

一个开箱即用的 Rust 项目模板：统一 stable 工具链、提交前检查、CI 与自动发版。

## 包含什么

| 文件 | 作用 |
| --- | --- |
| `cargo-generate.toml` | 生成项目时的配置：排除 `cliff.toml`，因为里面是 git-cliff 自己的模板语法 |
| `Cargo.toml` | 包名是 cargo-generate 占位符（生成时替换为项目名）；edition 2024、声明 `rust-version`（MSRV）、`[lints]` 中 deny `unsafe_code` |
| `rust-toolchain.toml` | 跟随 stable 频道并声明 `rustfmt` / `clippy` 组件，进仓库即自动安装 |
| `.pre-commit-config.yaml` | 格式化、lint、测试、依赖检查、拼写检查 |
| `.github/workflows/build.yml` | 构建检查（在模板仓库里先由模板生成一个项目、再检查该项目），以及 tag 触发的发版 |
| `.github/dependabot.yml` | 跟踪 action、pre-commit hook 与依赖的版本更新 |
| `deny.toml` | 依赖的许可证与安全公告策略 |
| `cliff.toml` | 由 commit 生成 changelog |

## 使用

本仓库是 cargo-generate 模板，它自己的包名是占位符，因此 **clone 下来不能直接 `cargo run`**，要先由它生成项目：

```bash
cargo install cargo-generate
cargo generate wmjim/rust-template --name my-project
cd my-project
```

刚生成的项目由 `git init` 建出，没有初始提交、也没有跟踪任何文件，所以第一次提交要用 `git add -A && git commit`：这里 `git commit -a` 不会暂存任何东西，钩子也就无从运行。钩子本身还需要先在本项目里执行 `pre-commit install` 才生效（见下文「提交前检查」）。

### 生成后需要手工处理的几处

占位符只能替换 Liquid 认得出的内容，以下三处仍指向模板作者：

| 位置 | 改成 |
| --- | --- |
| `Cargo.toml` 的 `description` | 本项目的描述 |
| `cliff.toml` 里的 `replace = "https://github.com/wmjim/rust-template"` | 本仓库地址，否则 changelog 链接会指回模板 |
| `LICENSE` 的版权人 | 你自己 |

### 开发

工具链无需手动安装：进入仓库后 rustup 会按 `rust-toolchain.toml` 自动装好 stable 及其组件。

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

在模板仓库本体里，几个 cargo 钩子会自动让开 —— 那里没有可检查的 crate。

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

推一个 `v*` 形式的 tag 即可：CI 会先跑构建检查，再用 git-cliff 生成本次 changelog 作为 GitHub Release 的内容。

## License

MIT
