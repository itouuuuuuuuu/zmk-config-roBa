---
paths:
  - boards/shields/roBa/roBa_R.conf
  - boards/shields/roBa/roBa_R.overlay
---

# トラックボール (PMW3610) の設定

センサーは PixArt PMW3610、ドライバは kumamuk-git/zmk-pmw3610-driver。設定はすべて `roBa_R.conf` の Kconfig。

## 感度

| 設定 | 現在値 | 意味 |
|---|---|---|
| `CONFIG_PMW3610_CPI` | 600 | カーソル感度。200〜3200 を 200 刻み。大きいほど速い |
| `CONFIG_PMW3610_CPI_DIVIDOR` | 1 | 移動量をこの値で割る。1 より大きくすると遅くなる |
| `CONFIG_PMW3610_SCROLL_TICK` | 16 | スクロール 1 段あたりに必要な移動量。小さいほどスクロールが速い |
| `CONFIG_PMW3610_SNIPE_CPI` | (コメントアウト) | 精密モード用 CPI。使うならレイヤーと組み合わせる |

履歴: 400 (本家の初期値) → 800 (PR #8、速すぎた) → 600 (PR #9)。
感度を変える依頼は CPI のみ変更し、DIVIDOR は 1 のままにする。CPI を上げるとスクロールも速くなるので、必要なら SCROLL_TICK で補正する。

## 向き

- `CONFIG_PMW3610_ORIENTATION_0=y`、`CONFIG_PMW3610_INVERT_X=n`、`CONFIG_PMW3610_INVERT_SCROLL_X=y` が現状。
- 上下左右の反転は PR #4 で調整済み。変えるときは実機で確認が必要なので Test Plan に書く。

## オートマウスレイヤー

- `&trackball` の `automouse-layer` は PR #5 で**意図的に削除**した。球が動いても自動でレイヤー 4 に入らない。
- 理由: 静電容量タッチセンサー (itouuuuuuuuu/trackball-touch-sensor) で親指が触れている間だけレイヤー 4 を有効にする設計へ移行中のため。
  モーション検知式と併用すると競合する。
- `CONFIG_PMW3610_AUTOMOUSE_TIMEOUT_MS=700` は残っているが、automouse-layer が無いので効いていない。復活させる提案はしない。
- `scroll-layers = <5>;` (`config/roBa.keymap`) は有効。レイヤー 5 ではボールの動きがスクロールになる。

## 省電力

`RUN_DOWNSHIFT_TIME_MS` / `REST1_SAMPLE_TIME_MS` / `POLLING_RATE_125_SW` は電池持ちとレスポンスのトレードオフ。依頼がない限り触らない。
