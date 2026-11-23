zmk-config for CCK_BALL (4x6)
![白色成品+手托](https://github.com/user-attachments/assets/7b2834f0-ca09-4f8c-9fc1-12455f67ae44)
![白色成品2](https://github.com/user-attachments/assets/7fe073d5-bb1c-4e42-b150-3521b4418ae0)
![黑色成品1](https://github.com/user-attachments/assets/4a7023ca-07ca-4a63-9d32-9af96ff9c73f)
![黑色成品2](https://github.com/user-attachments/assets/34d4351d-cbd9-4c82-a2e0-c7c9d28c5942)

## キーマップの変更と書き込み手順（自分用メモ）

### ブランチについて
- キーマップの編集とビルドは **`build-v0.3` ブランチ**で行う。
- `CCK-BALL-4x6` ブランチのままビルドすると失敗する。
  - 原因：ZMK の main が 2025年12月に Zephyr 4.1 へ移行し、`nice_nano_v2` と PMW3610 ドライバ（DoctorWangWang 版）が通らなくなった。
  - 対策：`config/west.yml` の zmk の `revision` と `.github/workflows/build.yml` の `@main` を、どちらも `v0.3` に固定している。

### ビルド
1. `config/cck_ball.keymap` を編集する。
2. `build-v0.3` ブランチにコミットして push する。
3. GitHub Actions が自動でビルドする。終わったら Actions の画面から `firmware` をダウンロードする（`gh run download <run id> -n firmware` でも可）。

### 書き込み
- **右手側がメイン（central）**。キーマップだけを変えたときは、右手側だけ書き込めばよい。
1. 右手側を USB で Mac につなぐ。
2. リセットボタンをすばやく2回押す。
3. Finder のサイドバーに `NICENANO` が出る。そこへ `cck_ball_right-nice_nano_v2-zmk.uf2` をコピーする。
4. コピーが終わるとドライブが消えて、キーボードが再起動する。
   - `could not copy extended attributes ... Device not configured` と出ても問題ない（再起動で接続が切れるだけ）。
- 左手側も書き込むときは、同じ手順で `cck_ball_left-nice_nano_v2-zmk.uf2` をコピーする。

### 左右がつながらないとき
1. `settings_reset-nice_nano_v2-zmk.uf2` を左右両方に書き込む。
2. 左右それぞれのファームウェアを書き込み直す。
3. Mac とのペアリングをやり直す。

### ZMK Studio について
- 右手側は Studio を使える設定でビルドしているが、keymap に `&studio_unlock` がないため、このままではロックを解除できない。
- 使うなら、`config/cck_ball.conf` に `CONFIG_ZMK_STUDIO_LOCKING=n` を足すか、keymap に `&studio_unlock` を割り当ててビルドする。
- Studio での変更はキーボード本体に保存される。keymap ファイルには反映されず、settings_reset で消える。
