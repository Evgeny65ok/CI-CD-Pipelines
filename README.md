# 🚀 Учебные CI/CD пайплайны

Сборник из **7 учебных проектов** — пайплайны CI/CD для разных языков и сценариев публикации артефактов.

---

## 📋 Сводная таблица

| # | Проект | Язык | Артефакт | Куда публикует | Триггер |
|:-:|---|---|---|---|---|
| **1** | [hello-rust](https://github.com/Evgeny65ok/hello-rust) | 🦀 Rust | Docker-образ | **GHCR** | push в `main` |
| **2** | [hello-go](https://github.com/Evgeny65ok/hello-go) | 🐹 Go | Docker-образ | **GHCR** | push в `main` |
| **3** | [hello-go-releases](https://github.com/Evgeny65ok/hello-go-releases) | 🐹 Go | Бинарники (5 платформ) | **GitHub Releases** | тег `v*` |
| **4** | [PythonCLIRep](https://github.com/Evgeny65ok/PythonCLIRep) | 🐍 Python | Бинарники (3 платформы) | **GitHub Releases** | тег `v*` |
| **5** | [hello-dotnet](https://github.com/Evgeny65ok/hello-dotnet) | C#/.NET | Бинарники (2 платформы) | **GitHub Releases** | тег `v*` |
| **6** | [hello-gui](https://github.com/Evgeny65ok/hello-gui) | 🐹 Go + Fyne | Бинарники (3 платформы) | **GitHub Releases** | тег `v*` |
| **7** | [hex-loader](https://github.com/Evgeny65ok/hex-loader) | 🐹 Go + Fyne | Бинарники (3 платформы) | **GitHub Releases** | тег `v*` |

## 1. 🦀 hello-rust — Docker в GHCR

[![Repo](https://img.shields.io/badge/GitHub-hello--rust-181717?logo=github)](https://github.com/Evgeny65ok/hello-rust)
[![Actions](https://github.com/Evgeny65ok/hello-rust/actions/workflows/ci.yml/badge.svg)](https://github.com/Evgeny65ok/hello-rust/actions)

**Что делает пайплайн:**
* `cargo fmt --check` — проверка форматирования
* `cargo clippy -D warnings` — линтинг
* `cargo test` — тесты
* `cargo build --release` — сборка
* Docker build + push в **GHCR** при push в ветку `main`

**Артефакт:** `ghcr.io/evgeny65ok/hello-rust:latest`

```bash
docker pull ghcr.io/evgeny65ok/hello-rust:latest
docker run --rm ghcr.io/evgeny65ok/hello-rust
```
*Результаты: Actions + пакет в GHCR.*

---

## 2. 🐹 hello-go — Docker в GHCR

[![Repo](https://img.shields.io/badge/GitHub-hello--go-181717?logo=github)](https://github.com/Evgeny65ok/hello-go)
[![Actions](https://github.com/Evgeny65ok/hello-go/actions/workflows/ci.yml/badge.svg)](https://github.com/Evgeny65ok/hello-go/actions)

**Что делает пайплайн:**
* `gofmt -l` — проверка форматирования
* `go vet ./...` — линтинг
* `go test ./... -v` — тесты
* Multi-stage Docker build + push в **GHCR** при push в ветку `main`

**Артефакт:** `ghcr.io/evgeny65ok/hello-go:latest`

```bash
docker pull ghcr.io/evgeny65ok/hello-go:latest
docker run --rm ghcr.io/evgeny65ok/hello-go
```

**Вывод:**
```text
Hello from Go in Docker! 🐹🐳
OS: linux
Arch: amd64
Hello, Docker!
Sum 1..10 = 55
```
*Результаты: Actions + пакет в GHCR.*

---

## 3. 🐹 hello-go-releases — бинарники в GitHub Releases

[![Repo](https://img.shields.io/badge/GitHub-hello--go--releases-181717?logo=github)](https://github.com/Evgeny65ok/hello-go-releases)
[![Actions](https://github.com/Evgeny65ok/hello-go-releases/actions/workflows/ci.yml/badge.svg)](https://github.com/Evgeny65ok/hello-go-releases/actions)
[![Release](https://img.shields.io/github/v/release/Evgeny65ok/hello-go-releases?color=success)](https://github.com)

**Что делает пайплайн:**
* `gofmt -l`, `go vet`, `go test` — CI-проверки
* Матричная сборка под 5 платформ через `GOOS`/`GOARCH`
* Публикация бинарников в GitHub Release при push тега `v*`
* Версия динамически вшивается через `-ldflags "-X main.version=..."`

### Артефакты релиза v1.0.0

| ОС | Архитектура | Имя файла |
|:---|:---|:---|
| 🐧 Linux | amd64 | `hello-go-linux-amd64` |
| 🐧 Linux | arm64 | `hello-go-linux-arm64` |
| 🍎 macOS | amd64 | `hello-go-darwin-amd64` |
| 🍎 macOS | arm64 | `hello-go-darwin-arm64` |
| 🪟 Windows | amd64 | `hello-go-windows-amd64.exe` |

*Результаты: Actions + Release с 5 файлами.*

---

## 4. 🐍 PythonCLIRep — бинарники (PyInstaller) в GitHub Releases

[![Repo](https://img.shields.io/badge/GitHub-PythonCLIRep-181717?logo=github)](https://github.com/Evgeny65ok/PythonCLIRep)
[![Actions](https://github.com/Evgeny65ok/PythonCLIRep/actions/workflows/ci.yml/badge.svg)](https://github.com/Evgeny65ok/PythonCLIRep/actions)
[![Release](https://img.shields.io/github/v/release/Evgeny65ok/PythonCLIRep?color=success)](https://github.com)

**Что делает пайплайн:**
* `ruff check`, `ruff format --check` — линтинг и форматирование
* `pytest -v` — тестирование
* Матричная сборка PyInstaller на 3 ОС (cross-compile в Python нет!)
* Публикация готовых бинарников в GitHub Release при push тега `v*`

### Артефакты релиза v1.0.0

| ОС | Имя файла | Размер |
|:---|:---|:---|
| 🐧 Linux x64 | `hello-python-linux-x64` | ~19 MB |
| 🍎 macOS arm64 | `hello-python-macos-arm64` | ~7.7 MB |
| 🪟 Windows x64 | `hello-python-windows-x64.exe` | ~7.9 MB |

*Результаты: Actions + Release с 3 файлами.*

---

## 🎓 Чему учит каждый пайплайн

| Проект | Ключевые навыки |
|:---|:---|
| **hello-rust** | Rust toolchain, cargo, Docker, GHCR |
| **hello-go** | Go toolchain, multi-stage Docker, GHCR |
| **hello-go-releases** | Cross-compilation (GOOS/GOARCH), matrix builds, Releases |
| **PythonCLIRep** | venv, pytest, ruff, PyInstaller, matrix builds, Releases |

---

## 🛠 Общий стек

![Rust](https://img.shields.io/badge/Rust-1.75+-000000?logo=rust&logoColor=white)
![Go](https://img.shields.io/badge/Go-1.23-00ADD8?logo=go&logoColor=white)
![Python](https://img.shields.io/badge/Python-3.12-3776AB?logo=python&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-multi--stage-2496ED?logo=docker&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-CI%2FCD-2088FF?logo=githubactions&logoColor=white)
![GHCR](https://img.shields.io/badge/GHCR-Container%20Registry-181717?logo=github&logoColor=white)

<p align="center"><i>Учебный проект · 4 CI/CD пайплайна · 2026</i></p>

