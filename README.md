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
<summary>コマンドで入れたい人（winget）— 探す→確認→入れる の3ステップ</summary>

winget は Windows 標準のアプリ管理コマンドです。名前しか知らないアプリを入れるときの実務手順はこの3段階:

**① 検索 — カタログから探して「ID」を調べる**

```powershell
winget search ollama
```

似た名前のアプリも一緒に出てくるので、一覧の **Id 列** から本物（`Ollama.Ollama`）を見つけます。以後の指定はすべてこの ID で行います（名前より確実）。

**② 確認 — 本物かどうか素性を見る**

```powershell
winget show Ollama.Ollama
```

`Publisher Url` / `Homepage` が公式サイト（https://ollama.com/）になっているかを確認。
winget のカタログは誰でも登録申請できるので、初めて入れるアプリでは発行元チェックを癖にすると安全です。

**③ インストール**

```powershell
winget install Ollama.Ollama
```

ダウンロード → 改ざんチェック（SHA256 照合）→ インストールまで自動で走ります。
落ちてくるファイルは公式サイトの `OllamaSetup.exe` と同一なので、出来上がりは手動インストールと同じです。

入ったかどうかの確認は:

```powershell
winget list ollama
```

（`search` = 入れられるものを探す / `list` = 入っているものを探す、と覚えると混乱しません）
</details>

## 2. モデルをダウンロード

PowerShell（またはターミナル）を開いて:

```powershell
ollama pull phi4-mini
```

2.5GB のダウンロードが走ります。回線次第で数分〜。
（16GB RAM でも安心な一番軽いモデルから始めるのがおすすめ。物足りなくなったら `qwen3:8b` へ）

※ `pull` を飛ばして `run` してもモデルがなければ自動でダウンロードされます。分けているのは「待ち時間があるのはここ」と分かるようにするためです。

## 3. 動かす

```powershell
ollama run phi4-mini --verbose
```

プロンプトが出たら日本語で話しかけてください。終了は `/bye`。

**オプションの意味:**

| オプション | 何が起きるか |
|---|---|
| `--verbose` | **統計モード**。応答が終わるたびに処理の内訳が表示される（下記） |
| `--think=false` | **思考モードOFF**。qwen3 など「考えるモード」搭載モデル専用（phi4-mini には不要） |

`--verbose` を付けると応答のあとにこんな統計が出ます:

```
total duration:       12.4s      ← 全体の所要時間
load duration:        3.1s       ← モデルをRAMに読み込んだ時間（2回目以降はほぼ0に）
prompt eval rate:     45.2 tokens/s  ← あなたの入力を読んだ速度
eval rate:            8.3 tokens/s   ← 返事を生成した速度 ★ここが「体感速度」
```

「自分のPCで何 tokens/s 出るか」が数字で分かるので、モデル選びの物差しになります。

> **⚠️ qwen3 系を使う場合の注意:** qwen3 は「考えるモード」が標準ONで、
> 最初の返事まで**2分近く無言**になります（故障ではありません）。
> `ollama run qwen3:8b --think=false` と付けるか、対話中に `/set nothink` と入力してください。

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
