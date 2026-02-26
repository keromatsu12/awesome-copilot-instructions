---
name: Backend Integration
description: API連携、データフェッチ（TanStack Query）、エラーハンドリング、認証などバックエンドサービスとの統合に関するガイドライン。
---

# Backend Integration

## When to Use
- API通信を行う処理（GET, POST, PUT, DELETE）を実装する際
- データの取得・更新・キャッシュ戦略を検討する際
- ローディング状態やエラー状態のハンドリングを実装する際
- 認証・認可に関する処理を実装する際

## Quick Start
1. 必要なAPIエンドポイントの仕様を確認する（OpenAPIドキュメントなど）。
2. プロジェクト標準のデータフェッチライブラリ（TanStack Queryなど）を使用して、データ取得・更新ロジックを実装する。
3. ローディング中、エラー発生時のUI/UXを考慮して実装する。

## Guidelines
(現在、詳細なガイドラインは策定中です)

## Templates
- Path: `.github/skills/backend-integration/templates/`
