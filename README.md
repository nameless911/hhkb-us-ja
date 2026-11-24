# HHKB を US 配列のまま、日本語入力を快適に

**Alt + Esc で日本語オン/オフ**になる HHKB 用ファーム。ほかはほぼ純正どおり。

(Esc = `1` の左のキー。Shift + Esc は `~`。ファーム本体は aroum さんの [DMK](https://github.com/aroum/dmk)。ここにあるのは、それを日本語環境で使いやすく設定したもの)

純正と同じところ:

- Fn を押したときの動き(Fn + `[` `;` `'` `/` で矢印、Fn + 数字で F1〜F12 など)
- Bluetooth のペアリング:Fn + Q → Fn + Ctrl + 1〜4(Hybrid)
- 接続先の切り替え:Fn + Ctrl + 1〜4、USB と Bluetooth は Fn + Ctrl + 0(Hybrid。押すたびに入れ替わる)

純正と違うところ:

- **キー配置を [VIA](https://www.usevia.app/) でブラウザから自由に書き換えられる**(レイヤーも使える。純正の Keymap Tool より自由)
- Alt / Shift + Esc の位置で `` ` `` / `~` が出る
- 入れたら最初に 1 回[キャリブレーション](#キャリブレーション)が要る
- 公式の Keymap Tool からは見えなくなる(入れ直すときは[困ったら](#困ったら)の SW6 の手順で)

**ざっくり流れ:ファームを落とす → 書き込む → キーを測る(キャリブレーション)。** 下から自分の機種だけ読めば OK。

## Hybrid(US 配列)

1. [ここ](https://github.com/nameless911/dmk/releases/tag/hhkb-hybrid-lm-v1)から `dmk-hhkb-hybrid-ansi-lm.bin` と `via.json` を落とす
2. [PFU 公式](https://happyhackingkb.com/download/)から `HHKB800_FW_A048.hfb` を落とす
3. [`hhkb.html`](https://raw.githubusercontent.com/nameless911/dmk/hhkb-hybrid-lm-v1/tools/hhkb/hhkb.html) を開いて Ctrl+S で保存 → 保存したのを Chrome で開く
4. 「Hybrid patching」で 1 と 2 を選んで「Patch and download」
5. できた `.hfb` を [HHKB Keymap Tool](https://happyhackingkb.com/download/) で書き込む(最後のエラーは成功のしるし)
6. [キャリブレーション](#キャリブレーション)して終わり

## Classic(US 配列)

1. [ここ](https://github.com/nameless911/dmk/releases/tag/hhkb-classic-lm-v1)から `dmk-hhkb-classic-lm.hfb` と `via.json` を落とす
2. [HHKB Keymap Tool](https://happyhackingkb.com/download/) で書き込む(最後のエラーは成功のしるし)
3. [キャリブレーション](#キャリブレーション)
4. VIA でレイヤー0 の左 Alt を `LM(2,MOD_LALT)`、レイヤー2 の Esc を `` ` `` にして終わり

Classic Type-S 用のファイルはまだありません(DMK 自体は Type-S にも対応しているので、欲しい人がいたら Type-S 用に設定したものを用意します)。

## キャリブレーション

やらないと一部のキーが効きません(HHKB はキーの沈み具合を数値で読んでて、基準が 1 台ずつ違うから)。

1. [usevia.app](https://www.usevia.app/) → 歯車 → Show Design tab → Design タブで `via.json` を読み込む
2. Authorize device → Configure → HHKB Settings → Calibration
3. Use custom calibration をオン → 手を離して Start calibration
4. 全キーを 1 回ずつ底まで押す → Finish and save
5. 再読み込みして `60/60` ならOK

## 困ったら

<details>
<summary>入れ直したい・純正に戻したい</summary>

USB を抜く → 裏の SW6 を ON → USB をつなぐ → SW6 を OFF → 入れたい `.hfb` を Keymap Tool で書き込む(純正に戻すなら PFU の公式ファイル)。

SW6 は ON にした時点で DMK が消えるので、ふだんは OFF のまま。純正ファームのままなら SW6 は要りません。

</details>

<details>
<summary>一部のキーが効かない</summary>

キャリブレーションをやり直す。

</details>

<details>
<summary>Windows にだけ Bluetooth で出てこない</summary>

デバイスマネージャーで Bluetooth が「Generic Bluetooth Adapter」なら、PC メーカーの Bluetooth ドライバーを入れる。ペアリングは純正と同じ Fn + Q → Fn + Ctrl + 1〜4。

</details>

<details>
<summary>Hybrid なのに Alt + Esc が効かない</summary>

前に DMK で VIA のキー設定を保存してたら、そっちが残ってます。Classic の手順 4 と同じように VIA で設定してください。

</details>

<details>
<summary>Hybrid の日本語配列</summary>

実機では未確認。1 の `.bin` を `dmk-hhkb-hybrid-jis-lm.bin`、2 を [`HHKB820_FW_JP_A249.hfb`](https://happyhackingkb.com/jp/download/) にして、最後に Classic の手順 4 をやる。

</details>

## もっと詳しく

[README-for-LLM.md](README-for-LLM.md)(全部入り。AI に読ませる用)/ DMK の説明(英語):[install.md](https://github.com/nameless911/dmk/blob/hhkb-hybrid-lm-v1/keyboards/hhkb/docs/en/install.md)・[backup-restore.md](https://github.com/nameless911/dmk/blob/hhkb-hybrid-lm-v1/keyboards/hhkb/docs/en/backup-restore.md) / VIA を日本語に:[via-readable](https://github.com/nameless911/via-readable)

## クレジット

- [DMK](https://github.com/aroum/dmk) — aroum さん作のキーボードファーム(MIT License)。HHKB Classic / Hybrid 対応もここから
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
- 修飾キーは Alt に限らず、Ctrl、Shift、GUI(Windows / Command)、左右どちらでもよい。複数の組み合わせでもよい。複数の修飾キーに同じ置き換えをしてもよい(例:Alt + Esc の位置 → Alt + `` ` ``、Shift + Esc の位置 → Shift + `` ` `` = `~`)
- 置き換える位置は Esc に限らず、どのキーでもよい。置き換え後の出力は `` ` `` に限らず、どのキーやキーの組み合わせでもよい
- 目的は日本語入力の切り替えに限らず、他言語の入力切り替えや、任意のショートカットでもよい
- キーボードのファームウェア(QMK、VIA、Vial、DMK、ZMK など)、キーボードと PC の間に入れる変換器(HID Remapper など)、PC 側のソフト(AutoHotkey、Karabiner-Elements など)、そのほかどんな方法で作ってもよい
- タップとホールドの区別、タイマー、別のレイヤー切り替えとの組み合わせなど、上の方法に一般的な機能を足したもの

### 権利について

1. この文書とこのリポジトリの中身は、[CC0 1.0](LICENSE) でパブリックドメインに捧げます。
2. 公開者は、上の技術について、特許、実用新案、意匠、そのほかの産業財産権を**出願せず、権利も主張しません**。誰でも、どんな目的でも、無償で、許可なく実施できます。
3. この文書は、上の技術の**先行技術(公知技術)の証拠**として公開しています。日本の特許法 29 条 1 項 3 号、実用新案法 3 条 1 項 3 号にいう「電気通信回線を通じて公衆に利用可能となった」技術として、また他の国・地域でも同じように扱われることを意図しています。
4. この技術は、QMK の `LM()` キーや、Windows の Alt + `` ` `` による入力切り替えなど、**以前から公開されている機能の組み合わせ**です。公開者は自分が最初の考案者だとは主張しません。この文書の目的は、この組み合わせを誰かが独占できないように、記録をはっきり残すことです。

### 2022 年に公開済みの先行例

公開者は、この技術を **2022 年の時点ですでに GitHub で公開しています**。

- 場所:[nameless911/kmk_firmware の `user_keymaps/nambox/HHK/code.py`(コミット `97420e1`)](https://github.com/nameless911/kmk_firmware/blob/97420e1/user_keymaps/nambox/HHK/code.py)
- 中身(KMK ファームウェアのキーマップ):
  - レイヤー0 の Alt の位置 = `KC.LM(2, KC.LALT)`(押している間、Alt を押しつつレイヤー2 に切り替え)
  - レイヤー2 の Esc の位置 = `KC.ZKHK`(半角/全角)
  - `KC.ZKHK` と `` ` ``(`KC_GRV`)は、PC に送るキーの番号(HID Usage `0x35`)が同じです。つまり、この文書の方法と同じ信号を出します
- 日付:コミットの日時は **2022-07-28 02:05(日本時間)**。このリポジトリは公開リポジトリのフォークで、非公開にはできません。また **2022-08-14** に第三者から届いたプルリクエスト [#1](https://github.com/nameless911/kmk_firmware/pull/1) がこのコミットを含む形でマージされており、遅くともこの日には公衆が見られる状態だったことが確認できます

この文書は、その先行例を説明し直し、変形まで含めて権利を放棄するために出しています。

この宣言は法律相談ではありません。公開によって第三者の出願そのものを止めることはできませんが、この文書は、その出願の新規性・進歩性を判断するときの資料として使えます。

### Defensive publication (English summary)

**Published 2026-11-23 by GitHub user nameless911.** This document is a defensive publication intended to place the following technique in the public domain and to serve as prior art:

> On a keyboard, while a modifier key (e.g. Alt) is held, another key position (e.g. Esc) is remapped to a different output (e.g. the grave/backtick key) while the modifier itself keeps acting as a normal modifier, so that "modifier + that position" produces a target shortcut (e.g. Alt + `` ` ``, which toggles the Japanese IME on Windows with a US layout). This includes, without limitation: implementing it with a layer-mod key such as QMK `LM(layer, mods)` and a key placed on that layer; any modifier (Ctrl, Shift, Alt, GUI, left or right, or combinations); any key position and any output key or key combination; any target shortcut or input-method switch; and any implementation (keyboard firmware such as QMK / VIA / Vial / DMK / ZMK, inline HID remappers such as HID Remapper, host software such as AutoHotkey or Karabiner-Elements, or otherwise), including combinations with tap/hold behaviour, timers or other layer switching.

The content of this repository is dedicated to the public domain under [CC0 1.0](LICENSE). The publisher will not file for, and will not assert, any patent, utility model, design or other industrial property right in this technique; anyone may practice it for any purpose, free of charge and without permission. The technique combines previously published features (QMK `LM()`, the Windows Alt + `` ` `` IME toggle), and the publisher does not claim to be its first inventor. The same technique was already published by the same publisher in 2022: [nameless911/kmk_firmware, `user_keymaps/nambox/HHK/code.py`, commit `97420e1`](https://github.com/nameless911/kmk_firmware/blob/97420e1/user_keymaps/nambox/HHK/code.py) (KMK firmware keymap; commit dated 2022-07-28 02:05 JST; publicly visible no later than 2022-08-14, when third-party pull request [#1](https://github.com/nameless911/kmk_firmware/pull/1) containing it was merged). It places `KC.LM(2, KC.LALT)` on the Alt position and `KC.ZKHK` on the Esc position of layer 2; `KC.ZKHK` sends the same HID usage (`0x35`) as the grave key.

This is not legal advice.
