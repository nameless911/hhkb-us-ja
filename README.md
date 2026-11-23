# HHKB を US 配列のまま、日本語入力を快適に

HHKB Professional Classic(US 配列)を、日本語を書く人が快適に使うための設定のまとめです。

- **左 Alt を押しながら Esc の位置を押す → Alt + `` ` ``**(Windows の US 配列で、日本語入力のオン/オフ)
- **左 Alt だけなら普通の Alt**(Alt+Tab なども今まで通り)
- 右上の `` ` `` キーまで指を伸ばさなくていい
- VIA の画面を日本語で読みやすくする(任意)

使うもの:

| もの | 役割 |
|---|---|
| [nameless911/dmk](https://github.com/nameless911/dmk/releases/tag/hhkb-classic-lm-v1)(HHKB 用 DMK ファームのフォーク) | HHKB の中身を書き換えて、VIA で自由に配列を変えられるようにする。今回 `LM()` キーに対応させた |
| [VIA](https://www.usevia.app/) | ブラウザでキー配列を変える |
| [nameless911/via-readable](https://github.com/nameless911/via-readable)(任意) | VIA の見た目を読みやすくして、HHKB メニューを日本語にする |

## 仕組み

QMK でおなじみの **Layer-Mod キー `LM(layer, mods)`** を使います。

```
左 Alt の位置 = LM(2, MOD_LALT)
  押している間: Alt が押された状態 + キー配置がレイヤー2 に切り替わる

レイヤー2 の Esc の位置 = `(KC_GRV)
レイヤー2 のほかのキー = 透過(▽)→ レイヤー0 のキーがそのまま出る

左 Alt + Esc の位置 → Alt + `  → 日本語入力のオン/オフ
左 Alt + Tab       → Alt + Tab(普通どおり)
左 Alt だけ        → Alt(普通どおり)
```

## 必要なもの

- **HHKB Professional Classic(US 配列 / PD-KB401B など / モデル `AHHX01`)**
  - Type-S、JIS、Hybrid は、このまとめのファームでは**使えません**
- Windows(US 配列で Alt + `` ` `` が日本語入力の切り替えになる環境)
- Chrome か Edge(VIA は WebHID を使うため)

## 手順

### 1. ファームを書き込む

> [!WARNING]
> ファームの書き込みは自己責任です。先に [バックアップと戻し方](https://github.com/nameless911/dmk/blob/fix/windows-toolchain-exe/keyboards/hhkb/docs/en/backup-restore.md) を読んでおいてください。

1. [Release](https://github.com/nameless911/dmk/releases/tag/hhkb-classic-lm-v1) から `dmk-hhkb-classic-lm.hfb` と `via.json` をダウンロードする
2. 書き込む
   - **純正ファームのままの HHKB**:公式の「HHKB Keymap Tool」で `.hfb` を開いて、普通のファーム更新と同じように書き込む
   - **すでに DMK が入っている HHKB**:電源を切った状態で裏の DIP スイッチ **SW6 を ON** → USB をつなぐ → **SW6 を OFF** に戻す。これで純正の書き込み係(Bank 1)が起動するので、あとは上と同じ
3. 書き込みが終わると、キーボードは DMK として認識し直されます。公式ツールが「デバイスが見つからない」などと言っても、それは正常です(DMK は USB の ID が違うため)

コマンドで書き込みたい人は、DMK の `tools/hhkb/hhkb_flasher.py` も使えます(詳しくは [install.md](https://github.com/nameless911/dmk/blob/fix/windows-toolchain-exe/keyboards/hhkb/docs/en/install.md))。

### 2. VIA で設定する

1. [usevia.app](https://www.usevia.app/) を開く
2. 設定(歯車)→「Show Design tab」をオン → Design タブで `via.json` を読み込む
   - HHKB メニューを日本語にしたいときは、via-readable の [`hhkb-ja/via.ja.json`](https://github.com/nameless911/via-readable/tree/main/hhkb-ja) を代わりに読み込む
3. 「Authorize device」でキーボードを選ぶ(DMK は VIA の公式一覧に無いので、開くたびに必要です)
4. **レイヤー0** の左 Alt をクリック → 下の一覧の「Any」→ `LM(2,MOD_LALT)` と入力 → 確認
   - 16 進で入れるなら `0x5044`
5. **レイヤー2** の Esc の位置をクリック → `` ` ``(`KC_GRV`)を入れる
6. レイヤー2 のほかのキーは、透過(▽ / `KC_TRNS`)にしておくのがおすすめ。左 Alt + ほかのキーが、普通の Alt + キーとして出ます

これで「左 Alt + Esc の位置」で日本語入力が切り替わります。

### 3. VIA を読みやすくする(任意)

VIA の文字が小さくて読みにくい、英語で分かりにくい、という人向けです。入れ方は [via-readable](https://github.com/nameless911/via-readable) の README を見てください(Chrome の拡張機能「Stylus」に CSS を入れるだけ)。

## うまくいかないとき・元に戻したいとき

- **左 Alt を普通の Alt に戻す**:VIA でレイヤー0 の左 Alt に `KC_LALT` を入れる
- **ファームを入れ直す / 純正に戻す**:上の SW6 の手順で書き込み係を起動して、入れたいファームを書き込む。詳しくは [backup-restore.md](https://github.com/nameless911/dmk/blob/fix/windows-toolchain-exe/keyboards/hhkb/docs/en/backup-restore.md)
- **左 Alt を押したまま LM のキーを離すと**:左 Alt を物理キーと LM で同時に押していた場合、LM を離した時点で Alt も離れた扱いになります

## クレジット

- [DMK](https://github.com/aroum/dmk) — aroum さん作のキーボードファーム(MIT License)。HHKB 対応もここから
- `LM(layer, mods)` の考え方と VIA のキーコード番号 — [QMK Firmware](https://github.com/qmk/qmk_firmware)
- [VIA](https://www.usevia.app/)

## ライセンス

- このリポジトリの文章:**[CC0 1.0](LICENSE)**(パブリックドメイン。断りなく自由に使ってください)
- ファーム([nameless911/dmk](https://github.com/nameless911/dmk)):上流と同じ MIT License
- [via-readable](https://github.com/nameless911/via-readable):そちらの表記に従います

---

## 公知化と権利放棄の宣言

**公開日:2026 年 11 月 23 日** / 公開者:GitHub ユーザー [nameless911](https://github.com/nameless911)

この文書は、以下の技術を**誰でも自由に使える公知のもの**にするために公開します。

### 公開する技術

キーボードで、**修飾キー(例:Alt)を押している間だけ、別のキー(例:Esc の位置)の出力を、OS の入力切り替えなどのショートカットに使う文字(例:`` ` ``)に置き換え、修飾キーそのものは普通の修飾キーとして働かせ続ける**ことで、**修飾キー + そのキー**の組み合わせから、目的のショートカット(例:Windows の US 配列での Alt + `` ` `` による日本語入力のオン/オフ)を出す方法。

具体例と、同じ考え方の変形をすべて含みます。

- 修飾キーの位置に「押している間、指定したレイヤーを有効にし、同時に修飾キーも押す」キー(QMK の `LM(layer, mods)` など)を置き、そのレイヤーの任意の位置に置き換え後のキーを置く方法
- 修飾キーは Alt に限らず、Ctrl、Shift、GUI(Windows / Command)、左右どちらでもよい。複数の組み合わせでもよい
- 置き換える位置は Esc に限らず、どのキーでもよい。置き換え後の出力は `` ` `` に限らず、どのキーやキーの組み合わせでもよい
- 目的は日本語入力の切り替えに限らず、他言語の入力切り替えや、任意のショートカットでもよい
- キーボードのファームウェア(QMK、VIA、Vial、DMK、ZMK など)、キーボードと PC の間に入れる変換器(HID Remapper など)、PC 側のソフト(AutoHotkey、Karabiner-Elements など)、そのほかどんな方法で作ってもよい
- タップとホールドの区別、タイマー、別のレイヤー切り替えとの組み合わせなど、上の方法に一般的な機能を足したもの

### 権利について

1. この文書とこのリポジトリの中身は、[CC0 1.0](LICENSE) でパブリックドメインに捧げます。
2. 公開者は、上の技術について、特許、実用新案、意匠、そのほかの産業財産権を**出願せず、権利も主張しません**。誰でも、どんな目的でも、無償で、許可なく実施できます。
3. この文書は、上の技術の**先行技術(公知技術)の証拠**として公開しています。日本の特許法 29 条 1 項 3 号、実用新案法 3 条 1 項 3 号にいう「電気通信回線を通じて公衆に利用可能となった」技術として、また他の国・地域でも同じように扱われることを意図しています。
4. この技術は、QMK の `LM()` キーや、Windows の Alt + `` ` `` による入力切り替えなど、**以前から公開されている機能の組み合わせ**です。公開者は自分が最初の考案者だとは主張しません。この文書の目的は、この組み合わせを誰かが独占できないように、記録をはっきり残すことです。

この宣言は法律相談ではありません。公開によって第三者の出願そのものを止めることはできませんが、この文書は、その出願の新規性・進歩性を判断するときの資料として使えます。

### Defensive publication (English summary)

**Published 2026-11-23 by GitHub user nameless911.** This document is a defensive publication intended to place the following technique in the public domain and to serve as prior art:

> On a keyboard, while a modifier key (e.g. Alt) is held, another key position (e.g. Esc) is remapped to a different output (e.g. the grave/backtick key) while the modifier itself keeps acting as a normal modifier, so that "modifier + that position" produces a target shortcut (e.g. Alt + `` ` ``, which toggles the Japanese IME on Windows with a US layout). This includes, without limitation: implementing it with a layer-mod key such as QMK `LM(layer, mods)` and a key placed on that layer; any modifier (Ctrl, Shift, Alt, GUI, left or right, or combinations); any key position and any output key or key combination; any target shortcut or input-method switch; and any implementation (keyboard firmware such as QMK / VIA / Vial / DMK / ZMK, inline HID remappers such as HID Remapper, host software such as AutoHotkey or Karabiner-Elements, or otherwise), including combinations with tap/hold behaviour, timers or other layer switching.

The content of this repository is dedicated to the public domain under [CC0 1.0](LICENSE). The publisher will not file for, and will not assert, any patent, utility model, design or other industrial property right in this technique; anyone may practice it for any purpose, free of charge and without permission. The technique combines previously published features (QMK `LM()`, the Windows Alt + `` ` `` IME toggle), and the publisher does not claim to be its first inventor. This is not legal advice.
