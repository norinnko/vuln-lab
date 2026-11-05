# やられサーバー脆弱性診断ラボ

隔離した仮想環境に「意図的に脆弱なWebアプリ（やられサーバー）」を立て、
攻撃視点を学びながら脆弱性診断を行い、診断レポートにまとめる学習プロジェクト。

## 目的

- Webアプリの代表的な脆弱性（OWASP Top 10）を、攻撃と防御の両面から理解する
- Kali の診断ツール（Burp / ZAP / sqlmap / nikto 等）の使い方を身につける
- 実務同等の **脆弱性診断レポート** を自分で書けるようになる

## 構成

```
[ホストPC] 観察のみ
   └ VirtualBox ホストオンリー網（インターネット・家庭LANから隔離）
        ├ Kali VM         … 診断を行う側
        └ Target VM       … Ubuntu Server + Docker
              ├ DVWA         (http://<Target IP>:8080)
              └ Juice Shop   (http://<Target IP>:3000)
```

## ⚠️ 取り扱いルール（厳守）

1. 診断してよいのは **自分で立てた学習用アプリのみ**。外部サイト・他人の資産は対象にしない。
2. ネットワークは **ホストオンリー（隔離）**。やられアプリを外部に公開しない。
3. 各VMは使う前に **スナップショット** を取り、終わったら電源を落とす。
4. 検体マルウェア等は扱わない。本ラボは Web アプリ診断の学習に限定する。

## ディレクトリ

| パス | 内容 |
|---|---|
| `docs/01_lab_setup.md`    | VirtualBox 隔離ネットワークと VM の作成手順 |
| `docs/02_deploy_targets.md` | やられアプリ（DVWA / Juice Shop）の展開手順 |
| `docs/03_methodology.md`  | OWASP Top 10 ベースの診断手順・チェックリスト |
| `docs/04_tools.md`        | Kali 診断ツールのチートシート |
| `targets/docker-compose.yml` | やられアプリ一式を立てる compose ファイル |
| `reports/report_template.md` | 脆弱性診断レポートの雛形 |
| `reports/findings/`       | 発見した脆弱性を1件1ファイルで記録 |

## 進め方

1. `docs/01_lab_setup.md` … 隔離ネットワークと Target VM を用意
2. `docs/02_deploy_targets.md` … やられアプリを起動
3. `docs/03_methodology.md` … チェックリストに沿って診断
4. 見つけた脆弱性を `reports/findings/` に記録 → `reports/report_template.md` にまとめる
