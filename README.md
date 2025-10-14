---
public_date: 2025-10-14
update_date: 2025-10-14
version: 1.0.0
---
# Electron-Product-Template

本リポジトリは、Electronアプリの**配布専用テンプレート**です。

ソースコードを含まず、完成済みアプリの配布・検証・セキュリティガイドを提供します。

## 概要

このテンプレートは、開発用リポジトリ（非公開）から生成されたElectronアプリを、安全かつ再現性をもって一般ユーザーへ配布するための構成です。

Appleの開発者プラン未加入者でも、Gatekeeperの正規ルートを通じてアプリを配布できます。

## 特徴

- **未署名アプリ配布に最適化**（右クリック→開く／隔離属性削除に対応）  
- **自然言語によるコード解説**を含むドキュメント構成（`docs/`配下）  
- **GitHub Releases自動化**（`.github/workflows/release.yml`）  
- **法的通知・検証手順・セキュリティ明示**を完備  

## ファイル構成

```txt
electron-product-template/
├── README.md
├── CHANGELOG.md
├── VERIFY_GUIDE.md
├── TROUBLESHOOTING.md
├── SECURITY.md
├── docs/
│   ├── ARCHITECTURE_OVERVIEW.md
│   ├── MODULES_AND_FLOWS.md
│   └── PRIVACY_AND_DATA.md
├── public/
│   ├── screenshots/
│   └── legal/
│       └── NOTICE
└── .github/
    └── workflows/
        └── release.yml
```

## ダウンロードと起動手順（macOS）

1. [Releases](../../releases) ページから `.dmg` または `.zip` をダウンロード。  
2. Finderで右クリック → 「開く」 → ダイアログで「開く」を選択。  
3. 起動後、警告が出る場合は以下を実行：  
   ```bash
   xattr -dr com.apple.quarantine "/Applications/YourAppName.app"
   ```

## Windows版について

Windowsでは署名なしでも実行可能です。

初回起動時にSmartScreen警告が出た場合は、「詳細情報」→「実行」を選択してください。

## ドキュメント参照

- `docs/ARCHITECTURE_OVERVIEW.md`：全体設計と構造概要  
- `docs/MODULES_AND_FLOWS.md`：UIフロー・通信処理の解説  
- `docs/PRIVACY_AND_DATA.md`：データ保存・送信に関する方針  

## 注意事項

- `.dmg` および `.zip` は未署名のため、初回のみ明示的な起動許可が必要です。  
- セキュリティおよび検証手順は `SECURITY.md` と `VERIFY_GUIDE.md` を参照してください。  
- ライセンス関連の情報は `public/legal/NOTICE` に記載しています。

## ライセンス

本テンプレートはMITライセンスのもと公開されています。詳細は`NOTICE`を参照してください。
