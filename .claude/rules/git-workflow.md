# Git / PR ワークフロー

## リモート

| リモート | URL | 用途 |
|---|---|---|
| origin | github.com/itouuuuuuuuu/zmk-config-roBa | 唯一のリモート。push と PR はここだけ |

- 2026-10-19 に fork ネットワークから離脱し、`upstream` リモートも削除した。本家 (kumamuk-git/zmk-config-roBa) への push・PR はしない。
- 本家の変更を取り込みたいときは、一時的に remote を追加して feature ブランチに cherry-pick し、PR にする。取り込み後は remote を削除する。
- `.claude/settings.local.json` に、本家宛ての push や gh 操作を deny する PreToolUse hook が残っている (ローカル専用、保険)。

## ブランチと PR

1. `git checkout main && git pull` で最新化する。keymap-editor[bot] が main に直接コミットするので、pull を省略すると分岐する。
2. main から英語 kebab-case のブランチを切る (`trackball-cpi-600`、`ci/pin-ubuntu-runner` など)。
3. コミットメッセージ・PR タイトル・本文は日本語。本文には「概要 (Summary)」と「Test Plan」を書く。
4. origin に push して `gh pr create --repo itouuuuuuuuu/zmk-config-roBa` で PR を作る。
5. マージはユーザーが行う。「マージして」と言われた場合のみ Claude がマージする。
6. マージ後は `git checkout main && git pull` し、ローカルの feature ブランチを削除する。

## keymap-editor との共存

- ユーザーは Web の keymap-editor でも `config/roBa.keymap` を編集する。その変更は `keymap-editor[bot]` 名義で main に直接入る。
- 過去に、ブランチ分岐で keymap-editor の編集が孤立コミット化して失われたことがある (PR #4 で復元)。
  keymap を触る PR は短命にし、作業前に必ず main を pull する。
- keymap-editor は `roBa.keymap` を整形し直すので、手動編集時も同じ整形 (カラム揃え) を崩さない。
