# Ollama セットアップ手順書（Windows・GPUなしOK）

YouTube「皇帝ペンギンラボ」の動画補足です。コマンドはここからコピペしてください。

## 0. 事前確認（自分のRAMを知る）

`Ctrl + Shift + Esc` でタスクマネージャ → パフォーマンス → メモリ。

| RAM | 使えるモデルの目安 |
|---|---|
| 16GB | phi4-mini / qwen3:8b まで |
| 32GB | gemma4:12b、余裕があれば 27B クラスも |

## 1. インストール

[ollama.com](https://ollama.com/) → **Download for Windows** → `OllamaSetup.exe` を実行。

完了後、タスクトレイ（画面右下）にラマのアイコンが出ればOK。

<details>
<summary>コマンドで入れたい人（winget）</summary>

```powershell
winget install Ollama.Ollama
```
</details>

## 2. モデルをダウンロード

PowerShell（またはターミナル）を開いて:

```powershell
ollama pull qwen3:8b
```

5.2GB のダウンロードが走ります。回線次第で数分〜。

## 3. 動かす

```powershell
ollama run qwen3:8b --think=false
```

プロンプトが出たら日本語で話しかけてください。終了は `/bye`。

> **⚠️ `--think=false` を忘れずに。** qwen3 系は「考えるモード」が標準ONで、
> 最初の返事まで**2分近く無言**になります（故障ではありません）。
> 対話中に切り替える場合は `/set nothink` と入力。

## 4. 本当にCPUで動いてる？の確認

別のターミナルで:

```powershell
ollama ps
```

`PROCESSOR` 列が `100% CPU` なら、GPUなしのCPUだけで動いています。

## よく使うコマンド

| コマンド | 意味 |
|---|---|
| `ollama list` | 手元にあるモデル一覧 |
| `ollama pull <モデル名>` | モデルを取得 |
| `ollama run <モデル名>` | モデルと対話 |
| `ollama ps` | 実行中モデルとCPU/GPU使用状況 |
| `ollama rm <モデル名>` | モデルを削除（容量返却） |

## モデルサイズ早見表（実測環境で使用したもの）

| モデル | サイズ | RAM目安 |
|---|---|---|
| phi4-mini | 2.5GB | 16GB |
| qwen3:8b | 5.2GB | 16GB |
| gemma4:12b | 7.6GB | 32GB推奨 |
| deepseek-r1:14b | 9.0GB | 32GB |
| qwen3.6:27b | 17GB | 32GB〜 |

## アンインストールしたくなったら

設定 → アプリ → Ollama → アンインストール（または `winget uninstall Ollama.Ollama`）。

**注意**: モデル本体（`C:\Users\<あなた>\.ollama`）は残ります。容量を空けたい場合は
アンインストール前に `ollama rm` で消すか、アンインストール後にこのフォルダを削除してください。

---

📺 YouTube: [皇帝ペンギンラボ](https://www.youtube.com/channel/UCw-ytk4y6y4MntCyeiSa_-w)
📝 ブログ: https://chibasyuta.org/
