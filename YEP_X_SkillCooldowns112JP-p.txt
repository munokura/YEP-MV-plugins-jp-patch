/*:ja
 * @plugindesc v1.12 (要YEP_SkillCore.js)スキルが連続で使われるのを防ぐクールダウンのシステムを導入できます。
 * @author Yanfly Engine Plugins
 *
 * @param ---クールダウン---
 * @default
 *
 * @param Cooldown Format
 * @text 表示形式
 * @parent ---クールダウン---
 * @desc クールダウンの表示形式
 * 残りターン数:%1
 * @default %1CD
 *
 * @param Cooldown Font Size
 * @text フォントサイズ
 * @parent ---クールダウン---
 * @type number
 * @min 0
 * @desc クールダウン表示のフォントサイズ
 * デフォルト:28
 * @default 20
 *
 * @param Cooldown Text Color
 * @text フォント色
 * @parent ---クールダウン---
 * @type number
 * @min 0
 * @max 31
 * @desc クールダウン表示のフォント色
 * @default 6
 *
 * @param Cooldown Icon
 * @text アイコン
 * @parent ---クールダウン---
 * @type number
 * @min 0
 * @desc クールダウン表示に使われるアイコン。アイコン無しは0
 * @default 75
 *
 * @param Cooldown After Battle
 * @text 戦闘後処理
 * @parent ---クールダウン---
 * @type number
 * @desc 戦闘後のクールダウンへの処理
 * @default -10
 *
 * @param Cooldown Steps
 * @text マップ歩数
 * @parent ---クールダウン---
 * @type number
 * @min 0
 * @desc マップ上でクールダウンを 1 するために必要な歩数
 * @default 5
 *
 * @param Cooldown Bypass
 * @text 影響を受けないスキル
 * @parent ---クールダウン---
 * @desc クールダウンの影響を受けないスキル(例えば攻撃/防御など)
 * @default 1 2 3 4 5 6 7
 *
 * @param Cooldown Bypass List
 * @text 影響を受けないスキル一覧
 * @parent ---クールダウン---
 * @type skill[]
 * @desc クールダウンの影響を受けないスキル(例えば攻撃/防御など)のリスト。RPGツクールMV 1.5.0以降が必要です。
 * @default []
 *
 * @param ---ウォームアップ---
 * @default
 *
 * @param Warmup Format
 * @text 表示形式
 * @parent ---ウォームアップ---
 * @desc ウォームアップの表示テキスト
 * 残りターン数:%1
 * @default %1WU
 *
 * @param Warmup Font Size
 * @text フォントサイズ
 * @parent ---ウォームアップ---
 * @type number
 * @min 1
 * @desc ウォームアップ表示のフォントサイズ
 * デフォルト:28
 * @default 20
 *
 * @param Warmup Text Color
 * @text フォント色
 * @parent ---ウォームアップ---
 * @type number
 * @min 0
 * @max 31
 * @desc ウォームアップ表示のフォント色
 * @default 4
 *
 * @param Warmup Icon
 * @text アイコン
 * @parent ---ウォームアップ---
 * @type number
 * @min 0
 * @desc ウォームアップの表示アイコン
 * 0を入力するとアイコン無し
 * @default 75
 *
 * @param ---Battle Core---
 * @text ---バトルコア---
 * @default
 *
 * @param Time Based
 * @text クールダウン基準
 * @parent ---Battle Core---
 * @type boolean
 * @on 時間基準
 * @off ターン基準
 * @desc 戦闘システムがティック基準の場合、クールダウン基準をティックか時間か設定。ターン:false / 時間:true
 * @default false
 *
 * @param Turn Time
 * @text ティック数
 * @parent ---Battle Core---
 * @type number
 * @min 1
 * @desc 1クールダウン・ターンに相当するのに必要なティック数
 * @default 100
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
 * このプラグインを利用するには、YEP_SkillCoreが必要です。
 * リスト内ではYEP_SkillCoreの下にこのプラグインが来るようにしてください。
 *
 * このプラグインで、スキルに「クールダウン」を導入できます。
 * これにより、対象スキルの連続使用を制限することができます。
 *
 * ===========================================================================
 * クールダウンタイプ
 * ===========================================================================
 *
 * Cooldown (Standard)
 * この機能が働くと、特定のターンの間そのスキルを使うことができません。
 * また、クールダウンをさせるための方法はいくつかあります。
 * まずは、ただ単純に待つという方法。
 * 戦闘中、各ターンごとにクールダウンが進行します。
 * スキルなどで、これを早めることもできます。
 * 2つ目の方法は、戦闘を終了することです。
 * 戦闘終了によって、全てのクールダウンを特定の値だけ進行することができます。
 * (値はパラメータで指定可能)
 * 3つ目の方法は、フィールドマップを歩き回ることです。
 * 特定の歩数を歩くことでスキルのクールダウンを進行することができます。
 *
 * Warmups
 * ウォームアップは、クールダウンと同様の働きを持ちます。
 * 一定の時間が経つまでスキルを使わせないようにします。
 * しかしクールダウンと異なる点として、
 * 戦闘の最初に一度だけ起こる、という点が挙げられます。
 * ウォームアップタイマーにより、
 * 戦闘でそのスキルが使われるタイミングを制限することができ、
 * またそれは戦闘終了後は一時的に消去されます。
 * ウォームアップは、既存のクールダウン上に重ねられることは有りません。
 * スキルがウォームアップ段階の時にクールダウンが既に起こっている場合、
 * 両方が同時にアップデートされます。
 *
 * Linked Cooldowns
 * 使用したスキルが、ライブラリにクールダウンを持った他のスキルを
 * 発動させた時に起こります。
 * このクールダウンの他の特性は、通常のクールダウン(前述)と同様のものです。
 * このクールダウンタイプは、
 * スキルタイプクールダウン(後述)やグローバルクールダウンよりも優先されます。
 *
 * Skill Type Cooldowns
 * スキルタイプクールダウンが起こると、
 * バトラーのスキルライブラリ内で該当する全てのスキルが、
 * クールダウンの対象となります。
 * このクールダウンの他の特性は、通常のクールダウン(前述)と同様のものです。
 * このクールダウンタイプは、グローバルクールダウンよりも優先されます。
 *
 * 既にクールダウンを持つスキルに、さらに別のクールダウンが適用される場合、
 * 常に最大値が採用されます。
 * つまり、もしスキルが3ターンのクールダウンを持ち、
 * スキルタイプクールダウンは1に設定されていた時、3ターンの方が採用されます。
 * 同様に、もしスキルが3ターン、スキルタイプクールダウンが5ターンだった場合、
 * 5ターンの方が採用されます。
 *
 * ===========================================================================
 * メモタグ
 * ===========================================================================
 *
 * スキルのクールダウンを変更するには下記のメモタグを用いてください。
 *
 * スキルのメモタグ:
 *   <Cooldown: x>
 *   スキルに対するクールダウンを x ターンに設定します。
 *   このクールダウンは、このスキルにのみ単独で有効です。
 *   これはスキルタイプクールダウンやグローバルクールダウンより優先されます。
 *
 *   <Warmup: x>
 *   スキルに対するウォームアップを x ターンに設定します。
 *   新たな戦闘に入る時、スキルはウォームアップ中になり、
 *   ウォームアップが完了するまで使用することはできません。
 *
 *   <After Battle Cooldown: +x>
 *   <After Battle Cooldown: -x>
 *   戦闘終了後(勝利/敗北/逃走)、
 *    +x / -x だけ、スキルのクールダウン数を増減させます。
 *
 *   <Cooldown Steps: x>
 *   戦闘外で、毎 x 歩ごとにスキルのクールダウンが 1 進行します。
 *
 *   <Skill x Cooldown: y>
 *   <Skill name Cooldown: y>
 *   このスキルを使う時、スキルコストを消費した後で、
 *   x に該当するタイプを持つスキルは、ターン数 y のクールダウンを持ちます。
 *   これはグローバルクールダウンより優先されます。
 *
 *   <SType x Cooldown: y>
 *   このスキルを使う時、スキルコストを消費した後で、
 *   x に該当するタイプを持つスキルは、ターン数 y のクールダウンを持ちます。
 *   これはグローバルクールダウンより優先されます。
 *
 *   <Global Cooldown: x>
 *   このスキルを使う時、バトラーのライブラリにある全てのスキルには、
 *   ターン数 x のクールダウンが課せられます。
 *   これは個々のクールダウンやスキルタイプクールダウンよりも
 *   低い優先度として扱われます。
 *
 *   <Bypass Cooldown>
 *   その種類に関わらずスキルにクールダウンを無視させます。
 *   攻撃、防御、逃走等、クールダウンを適用しないスキルに使ってください。
 *
 * スキル、アイテムのメモタグ:
 *   <Skill x Cooldown: +y>
 *   <Skill x Cooldown: -y>
 *   <Skill name Cooldown: +y>
 *   <Skill name Cooldown: -y>
 *   このスキルにヒットした対象のスキル x が y の値で調整されます。
 *   これは使用者には無効で、対象のみに有効です。
 *
 *   <SType x Cooldown: +y>
 *   <SType x Cooldown: -y>
 *   このスキルにヒットした対象について、
 *   スキルライブラリ内の x タイプのクールダウンが y の値で調整されます。
 *   使用者には無効で、対象のみに有効です。
 *
 *   <Global Cooldown: +x>
 *   <Global Cooldown: -x>
 *   このスキルにヒットした対象について、
 *   スキルライブラリ内の全てのスキルのクールダウンが y の値で調整されます。
 *   使用者には無効で、対象のみに有効です。
 *
 * アクター、職業、敵、武器、防具、ステータスのメモタグ:
 *
 *   <Skill x Cooldown Duration: y%>
 *   <Skill name Cooldown Duration: y%>
 *   クールダウンのコストが適用された時、
 *   スキル x のクールダウン継続時間を、y% に変更します。
 *   これはスキル x にのみ作用します。
 *
 *   <SType x Cooldown Duration: y%>
 *   クールダウンのコストが適用された時、
 *   タイプ x のスキルのクールダウン継続時間を y% に変更します。
 *   これはスキル x にのみ作用します。
 *
 *   <Global Cooldown Duration: x%>
 *   クールダウンのコストが適用された時、全てのスキルのクールダウン継続時間を
 *   x% に変更します。
 *
 *   <Skill x Cooldown Rate: y%>
 *   <Skill name Cooldown Rate: y%>
 *   クールダウンカウンターが下がった時、
 *   スキル x に対するクールダウンレートを y% に変更します。
 *   これはスキル x にのみ作用します。
 *
 *   <SType x Cooldown Rate: y%>
 *   クールダウンカウンターが下がった時、
 *   タイプ x のスキルのクールダウンレートを y% に変更します。
 *   これはスキルタイプ x にのみ作用します。
 *
 *   <Global Cooldown Rate: x%>
 *   クールダウンカウンターが下がった時、
 *   全てのスキルのクールダウンレートを x% に変更します。
 *
 *   <Skill x Cooldown: +y>
 *   <Skill x Cooldown: -y>
 *   <Skill name Cooldown: +y>
 *   <Skill name Cooldown: -y>
 *   使用者が x というスキルを使った場合、
 *   そのスキルのクールダウンの値は増加/減少します。
 *   これは使用者がアクター、職業、クラス、敵である、
 *   またはその武器や防具を身に着けている、
 *   もしくはこのメモタグを持ったステートに影響を受けている時に発動します。
 *   これらの修正はレートと継続時間の計算が完了した時に適用されます。
 *
 *   <SType x Cooldown: +y>
 *   <SType x Cooldown: -y>
 *   使用者が x タイプのスキルを使った場合、
 *   そのスキルのクールダウンの値は増加/減少します。
 *   これは使用者がアクター、職業、クラス、敵である、
 *   またはその武器や防具を身に着けている、
 *   もしくはこのメモタグを持ったステートに影響を受けている時に発動します。
 *   これらの修正はレートと継続時間の計算が完了した時に適用されます。
 *
 *   <Global Cooldown: +x>
 *   <Global Cooldown: -x>
 *   使用者がスキルを使った場合(種類問わず)、
 *   そのスキルのクールダウンの値は増加/減少します。
 *   これは使用者がアクター、職業、クラス、敵である、
 *   またはその武器や防具を身に着けている、
 *   もしくはこのメモタグを持ったステートに影響を受けている時に発動します。
 *   これらの修正はレートと継続時間の計算が完了した時に適用されます。
 *
 *   <Skill x Warmup: +y>
 *   <Skill x Warmup: -y>
 *   <Skill name Warmup: +y>
 *   <Skill name Warmup: -y>
 *   戦闘開始時、スキル x のウォームアップの値は増加/減少します。
 *   これは使用者がアクター、職業、クラス、敵である、
 *   またはその武器や防具を身に着けている、
 *   もしくはこのメモタグを持ったステートに影響を受けている時に発動します。
 *   これらの修正はレートと継続時間の計算が完了した時に適用されます。
 *
 *   <SType x Warmup: +y>
 *   <SType x Warmup: -y>
 *   戦闘開始時、x タイプの全てのスキルのウォームアップの値は増加/減少します。
 *   これは使用者がアクター、職業、クラス、敵である、
 *   またはその武器や防具を身に着けている、
 *   もしくはこのメモタグを持ったステートに影響を受けている時に発動します。
 *   これらの修正はレートと継続時間の計算が完了した時に適用されます。
 *
 *   <Global Warmup: +x>
 *   <Global Warmup: -x>
 *   戦闘開始時、全てのスキルのウォームアップの値は増加/減少します。
 *   これは使用者がアクター、職業、クラス、敵である、
 *   またはその武器や防具を身に着けている、
 *   もしくはこのメモタグを持ったステートに影響を受けている時に発動します。
 *   これらの修正はレートと継続時間の計算が完了した時に適用されます。
 *
 * ===========================================================================
 * ルナティックモード - 特殊なクールダウン
 * ===========================================================================
 *
 * 特殊なコードをスキルのメモ欄に用いて、
 * スキルが使われる時のクールダウンの値を指定することができます。
 *
 * スキルのメモタグ:
 *   <Cooldown Eval>
 *   cooldown = x;
 *   cooldown += x;
 *   </Cooldown Eval>
 *   スキルに上記のタグを挿入し、クールダウンに必要なターン数を決定
 *
 *   <Warmup Eval>
 *   warmup = x;
 *   warmup += x;
 *   </Warmup Eval>
 *   スキルの上記のタグを挿入し、ウォームアップに必要なターン数を決定
 *
 * ===========================================================================
 * Yanfly Engine Plugins - Battle Engine Extension - Action Sequence Commands
 * ===========================================================================
 *
 * プラグインマネージャーに、このプラグインと共にYEP_BattleEngineCore.jsを、
 * 入れていれば、下記のクールダウン関連のアクションシーケンスも利用できます。
 *
 *===========================================================================
 * GLOBAL COOLDOWN: targets, +X
 * GLOBAL COOLDOWN: targets, -X
 * GLOBAL COOLDOWN: targets, X
 * - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
 * x の値で、クールダウンを全ての対象に設定できます。
 * クールダウンが有効な全てのスキルに対して適用されます。
 * - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
 * 例: global cooldown: target, +5
 *     global cooldown: user, -3
 *     global cooldown: enemies, 10
 *===========================================================================
 *
 *===========================================================================
 * SKILL X COOLDOWN: targets, +Y
 * SKILL X COOLDOWN: targets, -Y
 * SKILL X COOLDOWN: targets, Y
 * - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
 * 対象に対して、X というスキルに Y の値でクールダウンを付与します。
 * 特定のスキル X にのみ適用されます。
 * - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
 * 例: skill 10 cooldown: target, +5
 *     skill 12 cooldown: user, -3
 *     skill 15 cooldown: enemies, 10
 *===========================================================================
 *
 *===========================================================================
 * SKILL TYPE X COOLDOWN: targets, +Y
 * SKILL TYPE X COOLDOWN: targets, -Y
 * SKILL TYPE X COOLDOWN: targets, Y
 * - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
 * 対象に対して、X というスキルタイプに Y の値でクールダウンを付与します。
 * 特定のスキル X にのみ適用されます。
 * - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
 * 例: skill type 1 cooldown: target, +5
 *     skill type 2 cooldown: user, -3
 *     skill type 5 cooldown: enemies, 10
 *===========================================================================
 *
 * ===========================================================================
 * Changelog
 * ===========================================================================
 *
 * Version 1.12:
 * - Updated for RPG Maker MV version 1.5.0.
 * - Added Parameter: Cooldown Bypass List
 *
 * Version 1.11:
 * - Lunatic Mode fail safes added.
 *
 * Version 1.10:
 * - Compatibility update with Equip Battle Skills.
 * - Documentation update. Added help information for <warmup: x>.
 *
 * Version 1.09:
 * - Fixed a bug with the <Skill x Cooldown Rate: y%>,
 * <SType x Cooldown Rate: y%>, and <Global Cooldown Rate: x%> notetags not
 * working as intended.
 *
 * Version 1.08:
 * - Updated for RPG Maker MV version 1.1.0.
 *
 * Version 1.07:
 * - Named versions of these notetags have been added:
 * <Skill x Cooldown: y>, <Skill x Cooldown: +/-y>,
 * <Skill x Cooldown Duration: y%>, <Skill x Cooldown: +/-y>,
 * <Skill x Warmup: +/-y>
 *
 * Version 1.06a:
 * - Fixed a bug with cooldown duration modifiers not modifying by the correct
 * value indicated.
 * - Added a fail safe for when there are no targets.
 *
 * Version 1.05:
 * - Fixed a bug that prevented <Cooldown Eval> from running properly.
 *
 * Version 1.04:
 * - Fixed a bug that didn't alter cooldowns correctly.
 *
 * Version 1.03:
 * - Optimized for Battle Engine Core v1.08.
 *
 * Version 1.02a:
 * - Added return for drawSkillCost to assist others scripters when making
 * compatibility notes.
 *
 * Version 1.01:
 * - Cooldowns can now be applied to skills that aren't learned by the actor.
 *
 * Version 1.00:
 * - Finished plugin!
 */
