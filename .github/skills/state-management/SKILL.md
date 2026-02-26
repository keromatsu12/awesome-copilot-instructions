---
name: State Management
description: 状態管理（Zustand, TanStack Query, React Context）の使い分け、設計指針、パフォーマンス最適化に関するベストプラクティス。
---

# State Management

## When to Use
- コンポーネント間で共有する必要がある状態（ユーザーセッション、テーマ設定など）を実装する際
- サーバーデータ（APIレスポンス）のキャッシュや同期を行う際
- 複雑なUI状態（ウィザード形式のフォームなど）を管理する際

## Quick Start
1. 管理したい状態が「サーバーデータ（Server State）」か「クライアント状態（Client State）」かを判断する。
   - サーバーデータ → TanStack Queryを使用。
   - クライアント状態 → ローカルステート（useState）で十分か確認する。
2. 複数のコンポーネントで共有が必要なクライアント状態の場合のみ、Zustandなどのグローバルステート管理ライブラリの使用を検討する。

## Guidelines
(現在、詳細なガイドラインは策定中です)

## Templates
- Path: `.github/skills/state-management/templates/`
