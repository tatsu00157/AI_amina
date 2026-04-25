# AI Amina — CLAUDE.md

## プロジェクト概要

**AIあみな** は Karin Lab が提供する LINE 連携 AI チャットボットのランディングページ。
静的 HTML + CSS のみで構成（フレームワーク・ビルドツールなし）。

- ホスティング: GitHub Pages (`tatsu00157.github.io/AI_amina/`)
- 運営: Karin Lab（連絡先: support@karineffort.com）
- LINE公式アカウント: https://page.line.me/349hkgne

---

## ファイル構成

```
AI_amina/
├── index.html                          # メインLP
├── style.css                           # 全ページ共通スタイル
├── favicon.ico
├── robots.txt
├── sitemap.xml
├── googlefc6569cbbc2f8051.html         # Google Search Console 認証
├── images/
│   ├── AI_amina_logo.png               # ヘッダーロゴ（1000×300px、横長）
│   ├── AI_Amina.png
│   ├── AI_Amina_1.png                  # 丸形アイコン用
│   └── AI_amina_page.png
├── pryvacy/
│   └── anima-privacy-policy.html       # プライバシーポリシー（※ typo: pryvacy）
├── terms/
│   └── amina-terms-of-service.html     # 利用規約
└── law/
    └── amina-law.html                  # 特定商取引法に基づく表記
```

> `pryvacy/` のディレクトリ名は typo だが、canonical URL や既存リンクに影響するため変更しない。

---

## デザインシステム

### ブランドカラー

| 変数名          | 値        | 用途                    |
|----------------|-----------|-------------------------|
| `--pink`       | `#D4327A` | メインブランドカラー      |
| `--pink-dark`  | `#A8235C` | ホバー・グラデーション濃側 |
| `--pink-light` | `#F7A8C8` | ボーダー・アクセント      |
| `--pink-bg`    | `#FFF0F5` | セクション背景            |
| `--pink-card`  | `#FDE8F1` | カード内アクセント背景     |
| `--dark`       | `#1A1A2E` | フッター・body 背景       |
| `--mid`        | `#4A4A6A` | サブテキスト              |
| `--muted`      | `#8A8AA8` | 補足テキスト              |
| `--line-green` | `#06C755` | LINE ボタン専用           |
| `--border`     | `#F0D0E0` | ボーダー全般              |
| `--white`      | `#FFFFFF` | カード・コンテンツ背景     |

### タイポグラフィ

```css
font-family: 'Helvetica Neue', Arial, 'Hiragino Kaku Gothic ProN', 'Hiragino Sans', Meiryo, sans-serif;
```

### 共通トークン

- `--radius`: 12px（カード角丸）
- `--radius-lg`: 20px（プランカード等）
- `--shadow`: `0 4px 24px rgba(212,50,122,0.10)`
- `--shadow-card`: `0 2px 12px rgba(212,50,122,0.08)`

---

## HTML 構造（全ページ共通）

```
<header id="header">        ← sticky、全ページ共通
<main class="page-body">    ← white背景コンテンツ領域
  ...ページ固有コンテンツ...
</main>
<footer class="footer">     ← dark背景
```

### なぜ `<main class="page-body">` が必要か

`body { background: var(--dark) }` を設定しているため、`<main class="page-body">` で明示的に白背景を当てている。
これにより、フッター下に白い線が出るCSS仕様の問題（body背景のcanvas伝播）を解消。

`display: flow-root` を `.page-body` に設定することで BFC（ブロック整形コンテキスト）を生成し、
内部要素のマージン崩壊（margin collapse）が外に漏れるのを完全防止している。

---

## ページ構成（index.html）

| # | セクション   | id          | 内容                                     |
|---|------------|-------------|------------------------------------------|
| 1 | Header     | `#header`   | ロゴ＋PCナビ＋ハンバーガー（sticky）       |
| 2 | Hero       | —           | キャッチコピー＋LINEボタン＋バッジ         |
| 3 | Features   | `#features` | 機能カード3枚（チャット/画像生成/LINE完結） |
| 4 | Pricing    | `#pricing`  | 料金プラン3列カード                        |
| 5 | How to Use | `#howto`    | ステップ形式（番号＋縦ライン）             |
| 6 | Keywords   | —           | LINEキーワード3種                          |
| 7 | CTA Banner | —           | ピンクグラデーション全幅バナー＋LINEボタン |
| 8 | Contact    | `#contact`  | お問い合わせ＋返金ポリシー                 |
| 9 | Footer     | —           | 法的リンク3点                              |

---

## ヘッダー仕様

- 高さ: 60px（`position: sticky; top: 0; z-index: 100`）
- ロゴ: `height: 44px`（画像は 1000×300px）
- **PC（701px以上）**: ロゴ左 ＋ テキストナビ右（4項目）
- **スマホ（700px以下）**: ロゴ左 ＋ ハンバーガーボタン右
  - JS で `.is-open` クラスをトグル
  - ドロワーは `max-height` トランジションでスライド表示
  - メニュー項目タップで自動クローズ
- ナビリンク先: `#features` / `#pricing` / `#howto` / `#contact`
- サブページ（法的ページ）ではリンク先が `../index.html#features` 形式

---

## レスポンシブ

ブレークポイント: **700px**

| 要素              | PC           | スマホ       |
|------------------|--------------|-------------|
| ヘッダーナビ       | テキストリンク | ハンバーガー |
| Features グリッド | 3カラム       | 1カラム      |
| Pricing グリッド  | 3カラム       | 1カラム      |
| Keywords グリッド | 3カラム       | 1カラム      |

---

## サブページ（法的ページ）共通仕様

- 全ページが `../style.css` を参照
- ヘッダーは index.html と同一（ロゴ＋ナビ＋ハンバーガー）
- `.legal-wrap` クラスで統一レイアウト（`max-width: 760px; margin: 0 auto; padding: 48px 24px 80px`）
  - ※ margin ではなく padding でスペースを確保（margin collapse 防止のため）
- フッターは `footer.footer > div.footer-inner` 構成で統一
- JS（ハンバーガー制御）は各ページの `</body>` 直前にインライン記述

---

## ビジネスロジック（コンテンツ）

### 料金プラン

| プラン名  | 月額    | 画像生成回数 |
|---------|---------|------------|
| Free    | ¥0      | 月3回       |
| Standard| ¥500    | 月30回      |
| Premium | ¥1,000  | 月100回     |

- テキストチャットは全プラン無料・無制限
- 利用回数は毎月1日にリセット
- 決済: クレジットカード（Stripe）
- 返金不可、解約は LINE で「プラン解約」送信

### LINE キーワード

| キーワード | 機能                              |
|----------|----------------------------------|
| 画像ヘルプ | 生成方法・プロンプトのコツを返答    |
| プラン確認 | 現在のプラン・請求日・次回切替を返答 |
| プラン解約 | 解約予約を実行                     |

---

## SEO 設定

- `<link rel="canonical">`: `https://tatsu00157.github.io/AI_amina/`
- Google Search Console 認証ファイル: `googlefc6569cbbc2f8051.html`
- `robots.txt` / `sitemap.xml` あり
- サブページの canonical は `karin-lab.netlify.app` ベース（旧URL）

---

## 注意事項・既知の状態

- `pryvacy/` ディレクトリ名の typo は意図的に維持（リンク変更コスト）
- SVG の LINE アイコンはインライン埋め込み（外部依存なし）
- `body { background: var(--dark) }` のため、コンテンツは必ず `<main class="page-body">` でラップすること
- 法的ページでスペーシングが必要な場合は margin ではなく padding を使うこと（margin collapse がフッター上に暗いギャップを生む）
