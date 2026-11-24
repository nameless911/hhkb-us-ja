> **これは AI(LLM)に読ませる用の詳しい版です。** 人間が導入するなら短い [README.md](README.md) を読んでください。公知化宣言・ライセンス・クレジットは README.md にあります。
>
> 位置づけ:これは「DMK を入れて純正っぽい動きに戻すのが面倒なので、ほぼ純正の動き + Alt/Shift + Esc(`1` の左)で `` ` `` を出す設定を済ませたファーム」。初期キーマップに LM が入っているのは Hybrid ANSI のみ(dmk コミット c31509ff、keyboards/hhkb/config.h の `HHKB_HYBRID` 分岐)。Classic と Hybrid JIS は LM 対応ファームだが初期キーマップは変えておらず、VIA で設定する。
>
> 補足(dmk の install.md / backup-restore.md より):SW6 は DMK では Bank 1(純正の書き込み係)へ戻すためだけに使われ、DMK が SW6 ON で起動すると Bank 2 の先頭 128 バイトを消してから Bank 1 に入る。以後、完全なファームを書き込むまで Bank 2 は起動しない。純正ファームのままの個体では SW6 を使わない。書き込み時とふだんは SW6 OFF。SW1〜SW5 は DMK では何もしない。リンク先はブランチではなくタグ `hhkb-hybrid-lm-v1`(= Hybrid Release のコミット f7399670)に固定している。

# HHKB を US 配列のまま、日本語入力を快適に

HHKB Professional Classic と HHKB Professional HYBRID(Type-S 含む)を、US 配列のまま、日本語を書く人が快適に使うための設定のまとめです。

- **左 Alt を押しながら Esc の位置を押す → Alt + `` ` ``**(Windows の US 配列で、日本語入力のオン/オフ)
- **左 Alt だけなら普通の Alt**(Alt+Tab なども今まで通り)
- **Shift を押しながら Esc の位置を押す → `~`**(任意。左右どちらの Shift でも)
- 右上の `` ` `` キーまで指を伸ばさなくていい
- VIA の画面を日本語で読みやすくする(任意)

使うもの:

| もの | 役割 |
|---|---|
| [nameless911/dmk](https://github.com/nameless911/dmk)(HHKB 用 DMK ファームのフォーク。[Classic 用](https://github.com/nameless911/dmk/releases/tag/hhkb-classic-lm-v1) / [Hybrid 用](https://github.com/nameless911/dmk/releases/tag/hhkb-hybrid-lm-v1)) | HHKB の中身を書き換えて、VIA で自由に配列を変えられるようにする。今回 `LM()` キーに対応させた |
| [VIA](https://www.usevia.app/) | ブラウザでキー配列を変える |
| [nameless911/via-readable](https://github.com/nameless911/via-readable)(任意) | VIA の見た目を読みやすくして、HHKB メニューを日本語にする |

## 導入に必要なこと(最初に読んでください)

純正ファームの書き換えです。QMK や ZMK を知っている人ほど飛ばしがちなところだけ並べます。

- **キャリブレーションを必ずやる**:HHKB は「押した / 離した」のスイッチではなく、キーの沈み具合を数値で読んでいます。その基準は1台ずつ違うので、測らないと一部のキーが反応しません。手を離してから開始 → 全キーを底まで → 再読み込みして `60/60` を確認([手順 2](#2-via-につないでキーを測定する必須))
- **戻し方を先に知っておく**:裏の DIP スイッチ **SW6** で純正の書き込み係を起動できます([バックアップと戻し方](https://github.com/nameless911/dmk/blob/hhkb-hybrid-lm-v1/keyboards/hhkb/docs/en/backup-restore.md))
- **Classic**:書き込みは公式の HHKB Keymap Tool で。書き込み後に「デバイスが見つからない」と出るのは正常です(USB の ID が変わるため)。`LM()` は VIA で自分で設定します。配っている `.hfb` は Classic(`AHHX01`)用の `dmk-hhkb-classic-lm.hfb` と、Classic Type-S(`AHHS01`)用の `dmk-hhkb-classic-types-lm.hfb`([Release](https://github.com/nameless911/dmk/releases/tag/hhkb-classic-types-lm-v1)。ビルドのみ、実機では未確認)。型番が違うと書き込みツールにはじかれるので、機種に合ったほうを使う
- **Hybrid**:公式の更新ファイルに自分でパッチを当てます(PFU の Bluetooth ファームは配れないため)。Bluetooth は純正のままなので、ZMK などの知識は当てはまりません。ペアリングは純正と同じ **Fn + Q → Fn + Ctrl + 1〜4** です

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

(任意)左右の Shift の位置 = LM(2, MOD_LSFT) / LM(2, MOD_RSFT)
Shift + Esc の位置 → Shift + `  → ~
Shift + A          → A(普通どおり)
```

## ほかのやり方との違い

日本語入力の切り替えは、ほかのやり方でもできます。それぞれに向き不向きがあります。

| やり方 | どこで設定するか | ほかの PC につないだとき | 気をつけること |
|---|---|---|---|
| IME の設定で Ctrl+Space などを切り替えキーにする | PC(IME) | PC ごとに設定し直す | アプリによっては Ctrl+Space を補完などに使っていて、ぶつかることがある。会社の PC だと設定を変えられないこともある |
| AutoHotkey などのソフトでキーを置き換える | PC | PC ごとにソフトを入れる | ソフトを入れられない PC では使えない |
| HID Remapper などの変換器をキーボードと PC の間に挟む | 変換器 | そのまま使える | 機械が1つ増える |
| **この方法(キーボードの `LM()` キー)** | **キーボード** | **そのまま使える** | `LM()` に対応したファーム(QMK / VIA / Vial / DMK など)のキーボードが必要 |

この方法のいいところ:

- **OS の設定を変えなくていい**:キーボードの中だけで完結するので、どの Windows PC につないでも、IME の設定をいじらずにすぐ使えます
- **Windows 標準の Alt + `` ` `` のまま**:特別な切り替えキーを覚えなくてよく、ほかのキーボードやノート PC のキーボードに持ち替えても同じ指の動きで使えます
- **ほかのショートカットとぶつかりにくい**:左 Alt を押していないときは、Esc の位置はいつも通り Esc です

自分の PC 1台だけで使っていて、IME の設定で困っていないなら、そのやり方で十分です。いろいろな PC につなぐ人や、OS の設定を変えられない人に向いています。

## 必要なもの

次のどれか:

| 機種 | 型番の例 | モデル ID | 配るファーム | 実機での確認 |
|---|---|---|---|---|
| HHKB Professional Classic(US 配列) | PD-KB401B など | `AHHX01` | 書き込める `.hfb` | 確認済み |
| HHKB Professional Classic Type-S(US 配列) | PD-KB401WSC、PD-KB401BSC、PD-KB401YSC | `AHHS01` | 書き込める `.hfb`([Release](https://github.com/nameless911/dmk/releases/tag/hhkb-classic-types-lm-v1)) | **ビルドのみ。実機では未確認** |
| HHKB Professional HYBRID / Type-S(US 配列) | PD-KB800B、PD-KB800WS など | `AHUX01` | STM32 の `.bin`(各自で公式ファイルにパッチ) | 確認済み(PD-KB800WS。USB 接続と、Bluetooth でのペアリング:iPad / iPhone) |
| HHKB Professional HYBRID / Type-S(日本語配列) | PD-KB820 系 | `AHJX01` | STM32 の `.bin`(各自で公式ファイルにパッチ) | **ビルドのみ。実機では未確認** |

Classic Type-S 用は、ハードが Classic と同じ STM32 なので、Classic 用と同じコミットを DMK の `type-s` ビルド(`HHKB_CLASSIC_TYPE_S`)でビルドしたもの。ファームの本体部分は Classic 用と同じで、違うのはファイルの最後の型番(`AHHS01` / A8.50)だけ。作者が Type-S を持っていないので、**ビルドのみ。実機では未確認**。

- Windows(US 配列で Alt + `` ` `` が日本語入力の切り替えになる環境)
- Chrome か Edge(VIA は WebHID を使うため)

## 手順

### 1. ファームを書き込む

> [!WARNING]
> ファームの書き込みは自己責任です。先に [バックアップと戻し方](https://github.com/nameless911/dmk/blob/hhkb-hybrid-lm-v1/keyboards/hhkb/docs/en/backup-restore.md) を読んでおいてください。

#### Classic の場合

1. [Release](https://github.com/nameless911/dmk/releases/tag/hhkb-classic-lm-v1) から `dmk-hhkb-classic-lm.hfb` と `via.json` をダウンロードする
2. 書き込む
   - **純正ファームのままの HHKB**:公式の「HHKB Keymap Tool」で `.hfb` を開いて、普通のファーム更新と同じように書き込む
   - **すでに DMK が入っている HHKB**:電源を切った状態で裏の DIP スイッチ **SW6 を ON** → USB をつなぐ → **SW6 を OFF** に戻す。これで純正の書き込み係(Bank 1)が起動するので、あとは上と同じ
3. 書き込みが終わると、キーボードは DMK として認識し直されます。公式ツールが「デバイスが見つからない」などと言っても、それは正常です(DMK は USB の ID が違うため)

コマンドで書き込みたい人は、DMK の `tools/hhkb/hhkb_flasher.py` も使えます(詳しくは [install.md](https://github.com/nameless911/dmk/blob/hhkb-hybrid-lm-v1/keyboards/hhkb/docs/en/install.md))。

#### Hybrid の場合

Hybrid の更新ファイルには PFU の Bluetooth 用ファームが入っているので、出来上がった `.hfb` は配れません。公式の更新ファイルに、DMK の部分だけを自分で差し込みます(パソコンの中だけで済み、ファイルはどこにも送られません)。

1. [Hybrid 用 Release](https://github.com/nameless911/dmk/releases/tag/hhkb-hybrid-lm-v1) から、配列に合う `.bin` と `via.json` をダウンロードする
   - US 配列:`dmk-hhkb-hybrid-ansi-lm.bin`
   - 日本語配列:`dmk-hhkb-hybrid-jis-lm.bin`(実機では未確認)
2. PFU の公式サイトから、元になる更新ファイルを自分でダウンロードする
   - US 配列:[`HHKB800_FW_A048.hfb`](https://happyhackingkb.com/download/)
   - 日本語配列:[`HHKB820_FW_JP_A249.hfb`](https://happyhackingkb.com/jp/download/)
3. [`hhkb.html`](https://raw.githubusercontent.com/nameless911/dmk/hhkb-hybrid-lm-v1/tools/hhkb/hhkb.html)(raw。開いて Ctrl+S で保存。`/blob/` ページを保存すると GitHub の画面ごと落ちて動かない)を Chrome か Edge で開く → 「Hybrid patching」タブで、2 の公式ファイルと 1 の `.bin` を選んで「Patch and download」
4. できた `.hfb` を書き込む。`hhkb.html` の「Flash keyboard」タブ、公式の「HHKB Keymap Tool」、`hhkb_flasher.py` のどれでも書き込めます
   - **純正ファームのまま**:そのまま書き込める
   - **すでに DMK が入っている**:Classic と同じく SW6 で書き込み係を起動してから

詳しくは [install.md](https://github.com/nameless911/dmk/blob/hhkb-hybrid-lm-v1/keyboards/hhkb/docs/en/install.md) と [build.md](https://github.com/nameless911/dmk/blob/hhkb-hybrid-lm-v1/keyboards/hhkb/docs/en/build.md#hybrid-local-patching)。Hybrid を純正に戻すときは、SW6 で書き込み係を起動して、2 の公式ファイルをそのまま書き込みます。

### 2. VIA につないで、キーを測定する(必須)

> [!IMPORTANT]
> **DMK を入れたら、最初に必ずキーの測定(キャリブレーション)をしてください。** 測定しないと、工場出荷時の値が個体に合わず、**一部のキーが反応しない**ことがあります(このリポジトリの作者の Hybrid Type-S で実際に起きました)。

1. [usevia.app](https://www.usevia.app/) を開く
2. 設定(歯車)→「Show Design tab」をオン → Design タブで `via.json` を読み込む
   - HHKB メニューを日本語にしたいときは、via-readable の [`hhkb-ja/via.ja.json`](https://github.com/nameless911/via-readable/tree/main/hhkb-ja) を代わりに読み込む(Classic 向けに作ったもの。Hybrid の新しい電源メニューは英語の `via.json` のほうが正確です)
3. 「Authorize device」でキーボードを選ぶ(DMK は VIA の公式一覧に無いので、開くたびに必要です)
4. 構成(Configure)→ HHKB Settings → **Calibration** を開く
5. **Use custom calibration** をオンにする
6. **全部のキーから手を離して**、**Start calibration** を押す(指を乗せたままだと基準がずれます)
7. **全部のキーを 1 つずつ、底まで押し込む**。「キーテスト」タブで **Test Matrix** をオンにすると、押し終わったキーが光るので押し忘れが分かります
8. Calibration に戻って **Finish and save** を押す
9. ページを再読み込みして、**Calibration status** が `60/60` になっているか確かめる(押した直後は古い表示のことがあります)

> [!WARNING]
> VIA を開いている間は、ほかのツール(`hhkb_flasher.py` 以外の自作スクリプトなど)からキーボードに話しかけないでください。返事が混ざって VIA の画面が真っ白になります。そうなったら VIA のタブを閉じて開き直してください。

### 3. キーを設定する

VIA の画面では、**左から 1 番目のタブがレイヤー0**、2 番目がレイヤー1(Fn)、3 番目がレイヤー2 です(0 から数えます)。

#### Hybrid(US 配列)の場合:最初から入っています

Hybrid US 配列用のファームは、**最初から次の配置になっています**。VIA で設定しなくてそのまま使えます。

- 左右の Alt と Shift が `LM()`:**Alt + Esc の位置 → Alt + `` ` ``(日本語入力の切り替え)**、**Shift + Esc の位置 → `~`**
- 純正と同じ Bluetooth のキー:**Fn + Q** のあと 30 秒以内に **Fn + Ctrl + 1〜4** でその番号にペアリング、ふだんの **Fn + Ctrl + 1〜4** は接続先の切り替え、**Fn + Ctrl + 0** で USB と Bluetooth の切り替え(DMK では押すたびに入れ替わります)
- Fn の矢印やファンクションキーは純正と同じ(Fn + `[` `;` `'` `/` で矢印など)

> [!NOTE]
> この初期配置が使われるのは、キーボードに**キー設定がまだ保存されていないとき**だけです。前に DMK を入れて VIA でキーを変えたことがある Hybrid では、保存済みの設定がそのまま使われます。その場合は、下の Classic と同じ手順で設定してください。

#### Classic の場合:VIA で設定します

1. **レイヤー0** の左 Alt をクリック → 下の一覧の「Any」→ `LM(2,MOD_LALT)` と入力 → 確認
   - 16 進で入れるなら `0x5044`
2. **レイヤー2** の Esc の位置をクリック → `` ` ``(`KC_GRV`)を入れる
3. レイヤー2 のほかのキーは、透過(▽ / `KC_TRNS`)にしておくのがおすすめ。左 Alt + ほかのキーが、普通の Alt + キーとして出ます

これで「左 Alt + Esc の位置」で日本語入力が切り替わります。

**(任意)Shift + Esc の位置で `~` を打つ**:レイヤー0 の左 Shift に `LM(2,MOD_LSFT)`(`0x5042`)、右 Shift に `LM(2,MOD_RSFT)`(`0x5052`)を入れます。レイヤー2 の Esc の位置はさっきの `` ` `` のままで、Shift + `` ` `` = `~` になります。

### 4. VIA を読みやすくする(任意)

VIA の文字が小さくて読みにくい、英語で分かりにくい、という人向けです。入れ方は [via-readable](https://github.com/nameless911/via-readable) の README を見てください(Chrome の拡張機能「Stylus」に CSS を入れるだけ)。

## うまくいかないとき・元に戻したいとき

- **左 Alt を普通の Alt に戻す**:VIA でレイヤー0 の左 Alt に `KC_LALT` を入れる
- **ファームを入れ直す / 純正に戻す**:上の SW6 の手順で書き込み係を起動して、入れたいファームを書き込む。詳しくは [backup-restore.md](https://github.com/nameless911/dmk/blob/hhkb-hybrid-lm-v1/keyboards/hhkb/docs/en/backup-restore.md)
- **Shift + Esc が要るとき**:Shift も LM にすると、Shift + Esc の位置は `~` になります(Chrome のタスクマネージャーなど、Shift + Esc を使うアプリでは押せなくなります)
- **LM のキーを2つ同時に押したとき**:たとえば Shift と Alt の両方を LM にして同時に押し、片方だけ離すと、もう片方を押していてもレイヤー2 が解除されます。もう一度押し直せば元に戻ります
- **左 Alt を押したまま LM のキーを離すと**:左 Alt を物理キーと LM で同時に押していた場合、LM を離した時点で Alt も離れた扱いになります
- **Windows の PC にだけ Bluetooth で出てこない**:iPhone や iPad には出るのに Windows の「デバイスの追加」に出ないときは、HHKB ではなく PC 側のドライバーが原因のことがあります。デバイスマネージャーの「Bluetooth」で、アダプターが **Generic Bluetooth Adapter**(Microsoft の汎用ドライバー)になっていたら、PC メーカーのサポートページから Bluetooth ドライバーを入れて再起動してください。名前が「Realtek Bluetooth Adapter」のように、チップのメーカー名に変われば OK です。Realtek のチップは、汎用ドライバーのままだと周りの機器を探せないことがあります(ThinkPad X13 Gen 2 AMD で確認。ほかの Bluetooth 機器も同じように出てきません)
