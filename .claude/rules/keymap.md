---
paths:
  - config/roBa.keymap
  - config/roBa.dtsi
---

# キーマップ (config/roBa.keymap)

macOS 前提 (LG = Command、LA = Option)。日本語入力は `LANG1` / `LANG2` で切り替える。

## レイヤー

| 番号 | 名前 | 入り方 | 内容 |
|---|---|---|---|
| 0 | default_layer | - | QWERTY。ホームロウの A と X・C は mod-tap |
| 1 | layer_1 | `&mo 1` (左親指) | 記号と矢印 |
| 2 | layer_2_mouse_click_and_func | Space 長押し (`&lt 2 SPACE`) | F1〜F12、マウスクリック、Ctrl+矢印 |
| 3 | layer_3_text_select | `&mo 3` | Cmd+矢印、Cmd+Shift+矢印 (テキスト選択・ウィンドウスナップ) |
| 4 | layer_4_mouse_layer | **現在入る経路なし** | マウスクリック。タッチセンサー配線後に位置 16 を `&mo 4` にする予定 (PR #5) |
| 5 | layer_5_mouse_layer | Tab 長押し (`&lt_to_layer_0 5 TAB`) | テンキー、スクリーンショット。トラックボールがスクロールになる (`scroll-layers`) |
| 6 | layer_6 | `&mo 6` | Option+矢印、Option+Shift+矢印 |
| 7 | layer_7_change_browser_tab | LANG2 長押し (`&lt 7 LANG2`) | Cmd+数字 (ブラウザタブ切替) |
| 8 | layer_8_move_window | Q 長押し (`&lt 8 Q`) | Ctrl+Shift+文字 (ウィンドウ管理アプリ用) |

## カスタム behavior

| 名前 | 内容 |
|---|---|
| `mt_slow` | hold-tap、tapping-term 300ms、balanced。ホームロウ修飾キー用 |
| `mt_fast` | hold-tap、tapping-term 145ms、balanced。`RCTRL SLASH` 用 |
| `lt_to_layer_0` | 長押しで `&mo`、タップで `&to 0` してからキー送出。Tab に使用 |
| `to_layer_0` | `&to 0` + キー送出のマクロ |
| combo `lang1` | 位置 34 + 40 の同時押しで `LANG1` |

グローバル `&mt` は `flavor = "balanced"`、`quick-tap-ms = <0>`。

## 編集時の注意

- keymap-editor (Web) がこのファイルを整形し直す。手動編集でもカラム揃えを維持し、コメントアウトされた `arrows` ブロックは残す。
- `#include <dt-bindings/zmk/pointing.h>` が `&mkp` に必要。消さない。
- レイヤー番号は `scroll-layers = <5>;` や `&lt 8 Q` などの参照と結びついている。レイヤーを増減するときは参照先をすべて更新する。
- 変更後は左右両方の `.uf2` を書き込む必要がある。
