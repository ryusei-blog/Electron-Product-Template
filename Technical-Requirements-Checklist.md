---
create_date: 2025-10-15
update_date: 2025-10-15
---
# Electron開発仕様チェックシート（ステップバイステップ）

このチェックシートは、**設計→開発→配布**までの意思決定を、質問に答えるだけで固められるように設計しています。各問いは**単一選択（原則）**です。最後に**決定事項サマリー（YAML）**を書き出せます。

## 問1：プロジェクト形態（リポジトリ構造）

* [ ] 単一プロダクト（非モノレポ）
* [ ] 傘モノレポ（複数プロダクトを同居）
* [ ] 将来モノレポ化を見据えた単一プロダクト

**決定される仕様**：ワークスペース運用方針、ディレクトリ深さ、CIのpaths。

## 問2：ターゲットOS／アーキテクチャ

* [ ] macOS（x64）
* [ ] macOS（arm64/Apple Silicon）
* [ ] Windows（x64）
* [ ] Linux（x64）
* [ ] 複数OS併用（上記を組み合わせ）

**決定される仕様**：ビルドターゲット、コード署名、ネイティブ拡張のABI対応。

## 問3：UI技術（Renderer）

* [ ] React + Next.js
* [ ] Vue + Nuxt
* [ ] Svelte + SvelteKit
* [ ] Preact / Solid / Qwik
* [ ] Vanilla（HTML + JS）

**決定される仕様**：ルーティング方式、ビルド手段、開発サーバ利用有無。

## 問4：レンダリング／配信方式

* [ ] SPA（`loadFile`で静的配信）
* [ ] SSG（静的書き出し→`loadFile`）
* [ ] SSR（開発時のみ`http://localhost:*`に`loadURL`）
* [ ] 常時ローカルHTTPサーバを同梱して配信

**決定される仕様**：`BrowserWindow.loadURL/loadFile`戦略、起動順序、デバッグ手順。

## 問5：パッケージマネージャ

* [ ] npm
* [ ] yarn
* [ ] pnpm（推奨：高速・省容量・厳密解決）

**決定される仕様**：lockファイル、CIのキャッシュ、ワークスペース設定。

## 問6：Node/Electronのバージョン管理

* [ ] nvm（.nvmrcで固定）
* [ ] Volta
* [ ] システム標準を使用（固定しない）

**決定される仕様**：チーム再現性、CIセットアップ、LTS方針。

## 問7：セキュリティモデル（Preload/Context）

* [ ] `contextIsolation: true`（推奨）＋ `preload` + `contextBridge`
* [ ] `nodeIntegration: false`（推奨）
* [ ] 例外的に`nodeIntegration: true`を許可
* [ ] `sandbox: true`を併用

**決定される仕様**：IPC境界、XSS/権限分離ポリシー、CSP初期値。

## 問8：IPC設計（Main⇄Renderer）

* [ ] `ipcMain`/`ipcRenderer` + `contextBridge`（型付きAPI）
* [ ] MessagePort/Custom Protocol
* [ ] WebSocket（内部サーバor外部）

**決定される仕様**：API表面、スキーマ（型定義・バリデーション）、エラーハンドリング方針。

## 問9：資格情報・設定管理

* [ ] `.env` + 安全ロード（Mainのみ読み込み）
* [ ] OSキーチェーン利用（macOS Keychain / Windows Credential）
* [ ] 暗号化ストア（例：secure-store, keytar）

**決定される仕様**：シークレットの配置、環境差分管理、バックアップ方針。

## 問10：ローカルデータ永続化

* [ ] SQLite（例：better-sqlite3）
* [ ] ファイルベース（JSON/NDJSON）
* [ ] IndexedDB（Renderer側）
* [ ] クラウド同期（REST/GraphQL/S3等）

**決定される仕様**：リポジトリ、トランザクション、マイグレーション戦略。

## 問11：アップデート戦略

* [ ] `autoUpdater` + GitHub Releases
* [ ] `autoUpdater` + S3/自前配布
* [ ] 手動配布のみ（アップデータなし）

**決定される仕様**：差分更新、署名／公証（notarization）、チャンネル（stable/beta）。

## 問12：ビルドツール／配布形式

* [ ] electron-builder（dmg/exe/AppImage）
* [ ] Electron Forge + Maker
* [ ] 手動スクリプト（高度な独自要件）

**決定される仕様**：アーティファクト形式、CIジョブ、署名鍵の保管。

## 問13：ネイティブ依存と再ビルド

* [ ] あり（例：better-sqlite3, sharp, @parcel/watcher）
* [ ] なし（Web純正のみ）

**決定される仕様**：`electron-rebuild`実行、アーキ固有バイナリ、ビルドキャッシュ戦略。

## 問14：プロセス設計

* [ ] シングルウィンドウ（標準）
* [ ] マルチウィンドウ（子ウィンドウ/モーダル）
* [ ] バックグラウンド（tray/auto-launch）

**決定される仕様**：ライフサイクル、IPCチャンネル、クラッシュ回復。

## 問15：ログ＆テレメトリ

* [ ] ローカルログ（winston等）
* [ ] クラッシュレポート（Sentry/Crashpad）
* [ ] 収集しない（オフライン特化）

**決定される仕様**：PII扱い、保持期間、ユーザー同意UI。

## 問16：パフォーマンス方針

* [ ] GPU加速を維持（既定）
* [ ] 省電力重視（バックグラウンド抑制）
* [ ] レンダリング最適化（仮想リスト、分割レンダ）

**決定される仕様**：フレームワーク設定、スロットリング、メモリ上限。

## 問17：テスト戦略

* [ ] Unit（Vitest/Jest）
* [ ] E2E（Playwright + Electron）
* [ ] スナップショット/UI回帰（Chromiumベース）
* [ ] 最小限（手動検証のみ）

**決定される仕様**：CI並列度、モック層、ゴールデンファイル管理。

## 問18：国際化/i18n

* [ ] なし（単一言語）
* [ ] i18n対応（メッセージ辞書）
* [ ] LTR/RTL両対応

**決定される仕様**：文言運用、ビルド時抽出、翻訳更新フロー。

## 問19：アクセシビリティ

* [ ] 標準（キーボード/コントラスト）
* [ ] 強化（スクリーンリーダー最適化）
* [ ] 要件外

**決定される仕様**：ARIA付与方針、ショートカット、フォーカス管理。

## 問20：ライセンス／法務

* [ ] OSSのみ（MIT/Apache-2.0等）
* [ ] 商用コンポーネント併用
* [ ] 内製のみ（配布限定）

**決定される仕様**：NOTICE生成、依存ライセンス検査、配布条件。

## 決定事項サマリー（YAMLテンプレート）

```yaml
project:
  repo_structure: ""           # single|mono|single_future_mono
  package_manager: ""          # npm|yarn|pnpm
  node_manager: ""             # nvm|volta|system
targets:
  os: []                       # [macos-arm64, macos-x64, windows-x64, linux-x64]
  electron_version: ""
ui:
  framework: ""                # react-next|vue-nuxt|svelte-kit|vanilla|other
  delivery: ""                 # spa|ssg|ssr|local_http
security:
  context_isolation: true
  node_integration: false
  sandbox: false
  preload_api: true
ipc:
  transport: ""                # ipc|messageport|websocket
  schema: ""                   # zod|typescript|json-schema|none
config:
  secrets: ""                  # env|keychain|encrypted_store
  storage: ""                  # sqlite|file|indexeddb|cloud
build:
  tool: ""                     # builder|forge|custom
  artifacts: []                # [dmg, exe, appimage]
  code_signing: ""             # apple|microsoft|none
  auto_update: ""              # github|s3|none
native:
  addons: []                   # [better-sqlite3, sharp]
  electron_rebuild: true
runtime:
  windows: ""                  # single|multi|tray
  performance: ""              # default|power_saving|render_opt
quality:
  tests: []                    # [unit, e2e, snapshot]
  telemetry: ""                # local|sentry|none
i18n_a11y:
  i18n: ""                     # none|basic|full
  accessibility: ""            # standard|enhanced|none
legal:
  license_model: ""            # oss|commercial|inhouse
```

## 使い方のヒント

1. 上から順に**各問いで一択を選ぶ**。
2. 最後のYAMLテンプレートに**選択結果を書き写す**。
3. そのYAMLを**README/要件定義/CI設定**の参照元にし、実装とドキュメントを同期させる。

このフォームで仕様が固まれば、**ElectronのMain/Preload/Rendererの役割分担**や**配布パイプライン**までブレずに進められます。次は、あなたの現状に合わせて**デフォルト回答セット**も用意できます。