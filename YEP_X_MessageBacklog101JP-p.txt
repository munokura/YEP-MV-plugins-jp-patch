/*:ja
 * @plugindesc v1.01 (要YEP_MessageCore) メッセージシステムにバックログ機能を追加します。
 * @author Yanfly Engine Plugins
 *
 * @help
 * 翻訳:ムノクラ
 * https://fungamemake.com/
 * https://twitter.com/munokura/
 *
 * ===========================================================================
 * 導入
 * ===========================================================================
 *
 * このプラグインには YEP_MessageCore プラグインが必要です。
 *
 * 注:Extended Message Packプラグインを使用している場合、
 * このプラグインをプラグイン管理でそれらの下に配置します。
 *
 * RPGがメッセージバックログを持つことは珍しいことではありません。
 * プレイヤーが以前に遭遇した全ての対話を確認するためのゲーム内ツールです。
 * このツールは、プレイヤーが誤って会話をスキップしたり、
 * 重要な瞬間に決定を下す前に過去のメッセージを再確認したい場合に役立ちます。
 *
 * このプラグインはあなたのゲームのメッセージシステム用の
 * メッセージバックログシステムを作成します。
 * ※「メッセージ中に」'Shift'（またはあなたが望む他のボタン）を押すと、
 * プレイヤーはバックログウィンドウを開いて
 * 直近20個のメッセージを見直すことができます
 * （保存されるメッセージ数はパラメータ内で変更できます）。
 * これらの保存されたメッセージには、選択肢からの選択結果、
 * 入力した番号、アイテム選択した項目を含めることもできます。
 *
 * プレイヤーはメッセージを見逃してしまったとしても、
 * 心配する必要がほとんどないことを十分に知っているので、
 * 先に進むことができます。
 *
 * ===========================================================================
 * 制御文字の無効化
 * ===========================================================================
 *
 * 一部の制御文字の性質上、メッセージログに対して無効になっています。
 * RPGツクールMVのデフォルトの制御文字の大部分は機能しますが、
 * Message Core または Extension Pack を通じて追加されたカスタムコードは
 * 様々な理由で無効になっています。
 * これらの無効にされる制御文字の一覧は以下です。
 *
 *    RPGツクールMVデフォルト:
 *    \{          フォントサイズを変更すると、バックログにエラーが発生します。
 *    \}          フォントサイズを変更すると、バックログにエラーが発生します。
 *
 *    Message Core:
 *    \AF[x]      バックログには顔画像は表示されません。
 *    \PF[x]      バックログには顔画像は表示されません。
 *    \FS[X]      フォントサイズを変更すると、バックログにエラーが発生します。
 *
 *    \PY[X]      一応動作しますが、
 *                いくつかの問題を引き起こすことが分かっています。
 *
 *    Extended Message Pack 1:
 *    \LSON       バックログの文字表示音は無効になっています。
 *    \LSOFF      バックログの文字表示音は無効になっています。
 *    \LSR        バックログの文字表示音は無効になっています。
 *    \FACEINDEX  バックログには顔画像は表示されません。
 *    \MSGPOSX    バックログはメッセージウィンドウサイズを変更しません。
 *    \MSGPOSY    バックログはメッセージウィンドウサイズを変更しません。
 *    \MSGEVENT   バックログはメッセージウィンドウサイズを変更しません。
 *    \MSGACTOR   バックログはメッセージウィンドウサイズを変更しません。
 *    \MSGPARTY   バックログはメッセージウィンドウサイズを変更しません。
 *    \MSGENEMY   バックログはメッセージウィンドウサイズを変更しません。
 *    \AUTOEVENT  バックログはメッセージウィンドウサイズを変更しません。
 *    \AUTOACTOR  バックログはメッセージウィンドウサイズを変更しません。
 *    \AUTOPARTY  バックログはメッセージウィンドウサイズを変更しません。
 *    \AUTOENEMY  バックログはメッセージウィンドウサイズを変更しません。
 *    \MSGROWS    バックログはメッセージウィンドウサイズを変更しません。
 *    \MSGWIDTH   バックログはメッセージウィンドウサイズを変更しません。
 *    \AUTO       バックログはメッセージウィンドウサイズを変更しません。
 *    \MSGRESET   バックログはメッセージウィンドウサイズを変更しません。
 *
 *    Extended Message Pack 2:
 *    - 数、アクター、敵の制御文字は追加前に変換されます。
 *    作成されたデータが最新のデータではなくローカライズされるためです。
 *
 * ===========================================================================
 * プラグインコマンド
 * ===========================================================================
 *
 * 以下のプラグインコマンドを使用して、
 * メッセージバックログに関するさまざまな設定を変更できます。
 *
 * プラグインコマンド:
 *
 *   EnableMessageBacklog
 *   DisableMessageBacklog
 *   - メッセージバックログが再生されないようにします。
 *
 *   EnableMessageLogging
 *   DisableMessageLogging
 *   - 有効にすると、新しいメッセージがバックログに記録されます。
 *   無効にした場合、それらは記録されません。
 *
 *   OpenMessageBacklog
 *   - マップシーンでこれを行うと、メッセージバックログが強制的に開かれます。
 *   （ボタンコモンイベントとの併用を強く推奨）
 *
 * ===========================================================================
 * Changelog
 * ===========================================================================
 *
 * Version 1.01:
 * - Message Backlog now removes more text codes regarding Letter Sounds.
 *
 * Version 1.00:
 * - Finished Plugin!
 *
 * ===========================================================================
 * End of Helpfile
 * ===========================================================================
 *
 * @param ---バックログキー---
 * @default
 *
 * @param LogKeyButton
 * @text キー・ボタン
 * @parent ---バックログキー---
 * @type combo
 * @option tab
 * @option shift
 * @option control
 * @option pageup
 * @option pagedown
 * @desc メッセージ中に押すと、バックログウィンドウを開くボタン
 * @default shift
 *
 * @param EnableLogKey
 * @text 有効化
 * @parent ---バックログキー---
 * @type boolean
 * @on 有効
 * @off 無効
 * @desc メッセージのバックログボタンの有効化
 * 無効:false / 有効:true
 * @default true
 *
 * @param ---設定---
 * @default
 *
 * @param DefaultLogging
 * @text デフォルト記録
 * @parent ---設定---
 * @type boolean
 * @on 全て記録
 * @off 記録しない
 * @desc メッセージのデフォルト記録設定
 * 記録しない:false / 全て記録:true
 * @default true
 *
 * @param LogSpecialInput
 * @text 特別入力を記録
 * @parent ---設定---
 * @type boolean
 * @on 記録する
 * @off 記録しない
 * @desc プレイヤーの選択・入力を記録
 * 記録しない:false / 記録する:true
 * @default true
 *
 * @param SpecialInputFmt
 * @text 特別入力の形式
 * @parent LogSpecialInput
 * @desc バックログにプレイヤーの選択・入力を表示するために使用される形式
 * %1:選択・入力したテキスト
 * @default »%1
 *
 * @param MaximumEntries
 * @text 記録最大数
 * @parent ---設定---
 * @type number
 * @min 1
 * @desc バックログの記録最大数。最大数を超えて追加された場合、最も古いエントリが削除されます。
 * @default 20
 *
 * @param ScrollBarEnabled
 * @text スクロールバー表示
 * @parent ---設定---
 * @type boolean
 * @on 表示
 * @off 非表示
 * @desc メッセージログのスクロールバー表示
 * 非表示:false / 表示:true
 * @default true
 *
 * @param ScrollBarColor
 * @text スクロールバー色
 * @parent ScrollBarEnabled
 * @type number
 * @min 0
 * @max 31
 * @desc スクロールバーのテキスト色
 * @default 0
 *
 * @param ScrollBarSpriteCode
 * @text スプライトコード
 * @parent ScrollBarEnabled
 * @type note
 * @desc これはスクロールバーのスプライトに使用されるコードです。
 * カスタマイズしたい場合、これを編集してください。
 * @default "// Establish basic measurements\nvar padding = backlogWindow.standardPadding();\nvar width = padding / 2;\nvar height = Graphics.boxHeight;\n\n// Calculate number of visible rows\nvar visibleRows = backlogWindow.height - padding * 2;\nvisibleRows = Math.floor(visibleRows / backlogWindow.lineHeight());\nif (visibleRows < backlogWindow.maxItems()) {\n  height *= visibleRows / Math.max(1, backlogWindow.maxItems());\n}\n\n// Calculate the size of a basic scrolling increment\nvar max = Math.floor(Math.max(1, backlogWindow.maxItems()) / backlogWindow.maxCols()) - 1;\nthis._increment = Graphics.boxHeight / Math.max(1, max);\n\n// Generate the bitmap\nthis.bitmap = new Bitmap(width, height);\nthis.bitmap.fillAll(backlogWindow.textColor(scrollBarColor));"
 *
 * @param ScrollSpeed
 * @text スクロール速度
 * @parent ---設定---
 * @type number
 * @min 0
 * @desc バックログに使用されるスクロール速度。 数値が小さいと速く大きいと遅くなります。
 * @default 4
 *
 * @param ScrollWrap
 * @text スクロール繰返し
 * @parent ---設定---
 * @type boolean
 * @on 有効
 * @off 無効
 * @desc 一番下のログエントリを選択している時、下を押すとエントリの一番上に戻る設定
 * @default true
 *
 * @param ---ビジュアル---
 * @default
 *
 * @param BackgroundType
 * @text ウィンドウ背景
 * @parent ---ビジュアル---
 * @type select
 * @option ウィンドウ
 * @value 0
 * @option ぼかし
 * @value 1
 * @option 画像
 * @value 2
 * @option 透明
 * @value 3
 * @desc バックログウィンドウの背景
 * @default 1
 *
 * @param BackOpacity
 * @text 背景の不透明度
 * @parent ---ビジュアル---
 * @type number
 * @min 0
 * @max 255
 * @desc ウィンドウの背景の不透明度
 * @default 192
 *
 * @param DimColor1
 * @text ぼかし背景色1
 * @parent ---ビジュアル---
 * @desc ぼかし背景の色1
 * デフォルト:rgba(0, 0, 0, 0.6)
 * @default rgba(0, 0, 0, 0.8)
 *
 * @param DimColor2
 * @text ぼかし背景色2
 * @parent ---ビジュアル---
 * @desc ぼかし背景の色2
 * デフォルト:rgba(0, 0, 0, 0)
 * @default rgba(0, 0, 0, 0.8)
 *
 * @param Picture
 * @text 背景画像
 * @parent ---ビジュアル---
 * @type file
 * @dir img/pictures/
 * @require 1
 * @desc 背景の使用画像
 * @default
 *
 * @param PictureOpacity
 * @text 背景画像の不透明度
 * @parent ---ビジュアル---
 * @type number
 * @min 0
 * @max 255
 * @desc 背景画像の不透明度
 * @default 255
 *
 * @param PictureStretch
 * @text 背景画像拡大
 * @parent ---ビジュアル---
 * @type boolean
 * @on 拡大する
 * @off 拡大しない
 * @desc 背景画像が画面より小さい場合、画面に合わせて拡大
 * @default true
 *
 */
