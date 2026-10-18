# AGENTS.md

roBa (分割型キーボード、右手にトラックボール) 用の ZMK ファームウェア設定リポジトリ。
kumamuk-git/zmk-config-roBa の fork で、itouuuuuuuuu の個人用にカスタマイズしている。

詳細なルールは `.claude/rules/` にある。ファイルを触る前に該当するルールを読むこと。

| ルール | 対象 |
|---|---|
| `.claude/rules/git-workflow.md` | ブランチ・PR・upstream の扱い (常に適用) |
| `.claude/rules/firmware-build.md` | ビルド・CI・書き込み手順 (常に適用) |
| `.claude/rules/trackball.md` | `boards/shields/roBa/roBa_R.conf` のトラックボール設定 |
| `.claude/rules/keymap.md` | `config/roBa.keymap` のレイヤー構成 |

## 絶対に守ること

- **upstream (kumamuk-git/zmk-config-roBa) へ push・PR・変更をしない**。常に origin (itouuuuuuuuu) を使う。
- **main に直接コミットしない**。必ず最新 main から feature ブランチを切る。
- マージはユーザーが行う。「マージして」と明示されたときだけ Claude がマージする。
- 実機の動作確認は Claude にはできない。PR の Test Plan にはユーザーが確認する項目として書く。

## ディレクトリ構成

```
build.yaml                      GitHub Actions のビルドマトリクス (3 ターゲット)
config/
  roBa.keymap                   キーマップ本体 (レイヤー 0〜8)
  west.yml                      ZMK 本体と pmw3610 ドライバの参照先
boards/shields/roBa/
  roBa.dtsi                     左右共通の devicetree
  roBa_L.overlay / roBa_L.conf  左手側 (キーのみ)
  roBa_R.overlay / roBa_R.conf  右手側 (トラックボール PMW3610 を含む)
  roBa.zmk.yml                  shield のメタデータ
.github/workflows/
  build.yml                     ZMK の build-user-config.yml を取り込んだビルド
  draw.yml                      keymap-drawer による図の生成 (手動実行)
keymap-drawer/                  draw.yml が生成する roBa.svg / roBa.yaml
zephyr/module.yml               このリポジトリを Zephyr モジュールとして読ませる
```

## 言語

- コミットメッセージ・PR タイトル・PR 本文は日本語。ブランチ名は英語の kebab-case。
- ユーザーへの返答は日本語。
