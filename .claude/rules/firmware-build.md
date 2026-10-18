# ファームウェアのビルドと書き込み

## ビルドターゲット (build.yaml)

| board | shield | 備考 |
|---|---|---|
| seeeduino_xiao_ble | roBa_R | snippet `studio-rpc-usb-uart` 付き。トラックボールはこちら |
| seeeduino_xiao_ble | roBa_L | 左手 |
| seeeduino_xiao_ble | settings_reset | BLE ペアリング情報のリセット用 |

GitHub Actions (`build.yml`) が push / PR ごとに 3 ターゲットをビルドし、Artifacts の `firmware` に `.uf2` をまとめる。
ローカルに west / Zephyr 環境は無い前提。ビルド確認は Actions の結果を `gh run list` / `gh run view` で見る。

## バージョン固定の経緯 (変更しないこと)

- `config/west.yml` の zmk は **`v0.3-branch`** に固定。ZMK main は Zephyr 4.1 + HWMv2 に移行し、
  ボード名 `seeeduino_xiao_ble` が消えたため main を追うとビルドが壊れる (PR #1)。
- `.github/workflows/build.yml` は ZMK の `build-user-config.yml@v0.3-branch` を取り込んだコピー。
  再利用ワークフロー参照に戻すと actions の Node.js 20 非推奨警告が出る (PR #6)。
  ZMK main 側の再利用ワークフローは、west.yml が v0.3-branch のままだと `KeyError: 'qualifiers'` で失敗するので使えない。
- ランナーは `ubuntu-24.04` に固定 (PR #7)。matrix ジョブのみ `ubuntu-22.04`。
- `zmk-pmw3610-driver` (kumamuk-git) は `main` のまま。v0.3 系と互換。
- `CONFIG_NFCT_PINS_AS_GPIOS=y` と `roBa.zmk.yml` の `requires: [seeeduino_xiao_ble]` は v0.3 系では有効なので残す。

HWMv2 への正式移行 (`xiao_ble//zmk` など) は将来の別タスク。

## 書き込み

- 右手側の設定 (`roBa_R.conf` / `roBa_R.overlay`) だけ変えた場合は右手側の `.uf2` のみ書き込めばよい。
- keymap (`config/roBa.keymap`) を変えた場合は左右両方を書き込む。
- 無線がおかしいときは `settings_reset` → `roBa_L` → `roBa_R` の順に書き込む。
- 実機確認は Claude にはできない。PR の Test Plan にチェックボックスとして書く。

## キーマップ図

`draw.yml` は `workflow_dispatch` の手動実行。実行すると keymap-drawer が `keymap-drawer/roBa.svg` を再生成してコミットする。
README はこの SVG を表示しているだけ。
