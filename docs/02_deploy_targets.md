# 02. やられアプリの展開

Target VM（Ubuntu + Docker）上でやられアプリを起動する。

## 起動手順

1. `targets/docker-compose.yml` を Target VM に置く（scp かエディタで作成）
2. 起動
   ```bash
   cd <compose のあるディレクトリ>
   docker compose up -d
   docker compose ps        # 起動確認
   ```
3. イメージの取得が終わったら、`docs/01_lab_setup.md` の手順で **NAT を外して隔離** する

## 各アプリの入口

| アプリ | URL | 初期設定・ログイン |
|---|---|---|
| DVWA | `http://<Target IP>:8080` | `admin` / `password`。初回は Setup ページで「Create / Reset Database」。`DVWA Security` で Low→High を切り替え |
| Juice Shop | `http://<Target IP>:3000` | 登録不要で開始。右上メニューの Scoreboard (`#/score-board`) で課題一覧を確認 |

## 使い方の方針

- **DVWA** … Low で「成功する攻撃の形」を体験 → Medium/High で「防御が入るとどう変わるか」を学ぶ
- **Juice Shop** … Scoreboard の課題を Easy から順に解き、攻撃の発想を鍛える

## 停止・リセット

```bash
docker compose stop          # 一時停止
docker compose down          # 削除（コンテナを作り直したいとき）
docker compose up -d --force-recreate   # 作り直し
```

VM ごと汚れたら、`clean-base` スナップショットに戻すのが最速。
