# 01. ラボ環境の構築（隔離ネットワーク + VM）

ホストPC（Windows 11 / RAM 32GB / VirtualBox）上に、隔離した診断ラボを作る。

## 1. 隔離ネットワークを用意する

やられアプリを家庭LAN・インターネットから遮断するため、**ホストオンリーアダプター** を使う。

1. VirtualBox を開く → メニュー `ツール` → `ネットワーク` → `ホストオンリーネットワーク`
2. `作成` を押す（例: `VirtualBox Host-Only Ethernet Adapter`）
3. DHCP サーバーは有効のままでよい（例: 192.168.56.0/24）

> ホストオンリー網は「Kali ↔ Target ↔ ホスト」だけが通信でき、外には出られない。
> さらに厳しく隔離したい場合は「内部ネットワーク(Internal Network)」を使うとホストからも切り離せる。
> 本ラボはホスト側から様子を見られる **ホストオンリー** を基本とする。

## 2. Target VM（やられアプリを載せる側）を作る

Ubuntu Server を使い、Docker でやられアプリを動かす。

1. [Ubuntu Server LTS](https://ubuntu.com/download/server) の ISO を入手
2. VirtualBox で新規VM作成
   - メモリ: 4096 MB / CPU: 2 / ディスク: 25 GB
   - ネットワーク アダプター1: **NAT**（インストールと docker pull のため一時的に使う）
3. Ubuntu をインストール（OpenSSH を有効にしておくと Kali から操作しやすい）
4. Docker をインストール
   ```bash
   sudo apt update && sudo apt install -y docker.io docker-compose-plugin
   sudo usermod -aG docker $USER      # 再ログインで反映
   ```
5. `targets/docker-compose.yml` を Target VM に配置し、`docs/02_deploy_targets.md` に従って起動
6. **イメージの pull が終わったら NAT を外す**
   - VM設定 → ネットワーク → アダプター1 を **ホストオンリー** に変更（外部から隔離）

## 3. Kali VM（診断を行う側）を設定

既存の `kali-linux-2026.1-virtualbox-amd64` を使う。

1. ネットワーク アダプター1: **ホストオンリー**（Target と同じ網に接続）
   - ツールを最新化する間だけ NAT を足してもよいが、診断中はホストオンリーのみにする
2. 起動して疎通確認
   ```bash
   ip a                       # 192.168.56.x が付いているか
   ping <Target VM の IP>     # Target に届くか
   ```

## 4. スナップショット

診断を始める前に、両VMを「クリーン状態」で保存しておく。

- VirtualBox で各VMを選択 → `スナップショット` → `作成`（例: `clean-base`）
- 壊したり汚したりしたら、このスナップショットに戻すだけで復旧できる

## 5. 疎通チェックリスト

- [ ] Kali から Target に ping が通る
- [ ] Kali のブラウザで `http://<Target IP>:8080`（DVWA）が開く
- [ ] Kali のブラウザで `http://<Target IP>:3000`（Juice Shop）が開く
- [ ] Target VM から外部（例 `ping 8.8.8.8`）に **出られない**（＝隔離できている）
- [ ] 両VMのスナップショットを取得済み

ここまで出来たら `docs/03_methodology.md` に進む。
