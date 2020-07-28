/*:ja
 * @plugindesc v1.04 パッシブ特性としてライフスティールを有効にします。
 * @author Yanfly Engine Plugins
 *
 * @param Enable HP Overheal
 * @text HPオーバーヒール有効化
 * @type boolean
 * @on 有効
 * @off 無効
 * @desc ライフスティールにダメージより多いHP回復を有効化
 * 無効:false / 有効:true
 * @default false
 *
 * @param Enable MP Overheal
 * @text MPオーバーヒール有効化
 * @type boolean
 * @on 有効
 * @off 無効
 * @desc ライフスティールにダメージより多いMP回復を有効化
 * 無効:false / 有効:true
 * @default false
 *
 * @param Negative HP LifeSteal
 * @text マイナスHPライフスティール有効化
 * @type boolean
 * @on 可能
 * @off 不可
 * @desc HPライフスティール値をマイナスにし攻撃者にダメージを与える
 * 不可:false / 可能:true
 * @default false
 *
 * @param Negative MP LifeSteal
 * @text マイナスMPライフスティール有効化
 * @type boolean
 * @on 可能
 * @off 不可
 * @desc MPライフスティール値をマイナスにし攻撃者にダメージを与える
 * 不可:false / 可能:true
 * @default false
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
 * HP吸収はRPGツクールMVの機能で、
 * 特定のスキルやアイテムの形でのみ存在します。
 * 物理攻撃、魔法攻撃、必中からHP吸収を設定する方法はありません。
 * このプラグインを使用すると、HPとMPの両方の値に対して、
 * 物理的、魔法的、必中に対して
 * パッシブなHPライフスティール特性を設定できます。
 *
 * ※このプラグインで発生するHP/MP吸収をライフスティール、
 * ツクールのデフォルトにある機能をHP/MP吸収はそのままの名称で呼びます。
 *
 * ===========================================================================
 * メモタグ
 * ===========================================================================
 *
 * 以下のメモタグを使用して、
 * HPライフスティールがデータベースにどのように機能するかを変更できます。
 *
 * ---
 *
 * スキル、アイテムのメモタグ
 *
 *   <HP Life Steal: x%>
 *   <MP Life Steal: x%>
 *   この攻撃はHP・MPのダメージ量に対してx%のHPライフスティールします。
 *
 *   <HP Life Steal: x>
 *   <MP Life Steal: x>
 *   この攻撃はHP・MPのダメージ量に関わらずx量のHPライフスティールします。
 *
 *   <Cancel Life Steal>
 *   パッシブなライフスティール効果を取り消します。
 *   ただし、HP吸収とMP吸収は引き続き発生します。
 *
 *   <Cancel HP Life Steal>
 *   <Cancel MP Life Steal>
 *   HP/MPライフスティールの効果のパッシブ化を取り消します。
 *   ただし、HP吸収とMP吸収は引き続き発生します。
 *
 * ---
 *
 * アクター、職業、敵、武器、防具、ステートのメモタグ
 *
 *   <HP Life Steal Physical: +x%>
 *   <HP Life Steal Magical: +x%>
 *   <HP Life Steal Certain: +x%>
 *
 *   <MP Life Steal Physical: +x%>
 *   <MP Life Steal Magical: +x%>
 *   <MP Life Steal Certain: +x%>
 *
 *   <HP Life Steal Physical: -x%>
 *   <HP Life Steal Magical: -x%>
 *   <HP Life Steal Certain: -x%>
 *
 *   <MP Life Steal Physical: -x%>
 *   <MP Life Steal Magical: -x%>
 *   <MP Life Steal Certain: -x%>
 *   バトラーは、物理的、魔法的、必中に
 *   与えられたダメージの+x%・-x%だけ、
 *   パッシブライフスティールを増加させます。
 *   この効果は乗法的に重なります。
 *
 *   <HP Life Steal Physical: +x>
 *   <HP Life Steal Magical: +x>
 *   <HP Life Steal Certain: +x>
 *
 *   <MP Life Steal Physical: +x>
 *   <MP Life Steal Magical: +x>
 *   <MP Life Steal Certain: +x>
 *
 *   <HP Life Steal Physical: -x>
 *   <HP Life Steal Magical: -x>
 *   <HP Life Steal Certain: -x>
 *
 *   <MP Life Steal Physical: -x>
 *   <MP Life Steal Magical: -x>
 *   <MP Life Steal Certain: -x>
 *   バトラーは、物理的、魔法的、必中に対して
 *   与えられたダメージの固定値+x・-xだけ
 *   パッシブライフスティールを追加的に増加させる。
 *   この効果は相加的に重なります。
 *
 *   <Guard Life Steal>
 *   バトラーは、HPとMPのどちらもライフスティールできません。
 *
 *   <Guard HP Life Steal>
 *   <Guard MP Life Steal>
 *   バトラーは、HPとMPのどちらもライフスティールできません。
 *
 *   <Cancel Life Steal>
 *   バトラーは、HPとMPのどちらもパッシブにライフスティールできません。
 *
 *   <Cancel HP Life Steal>
 *   <Cancel MP Life Steal>
 *   バトラーはHPやMPをパッシブにライフスティールできません。
 *
 *
 * ---
 *
 * ===========================================================================
 * ルナティックモード - カスタムライフスティール
 * ===========================================================================
 *
 * JavaScript を使ったメモタグを利用して、
 * データベースオブジェクトに動的なライフスティール値を持たせられます。
 *
 * --- スキルとアイテムのメモタグ ---
 *
 *   <Custom HP Life Steal Rate>
 *    rate = user.hpRate();
 *   </Custom HP Life Steal Rate>
 *
 *   <Custom MP Life Steal Rate>
 *    rate = user.hpRate();
 *   </Custom MP Life Steal Rate>
 *   'rate'変数は、対象に与えられたダメージに基づいて、
 *   スキル/アイテムが対象からライフスティールするHP/MPの量です。
 *   これは百分率の値です。
 *
 *   --- --- ---
 *
 *   <Custom HP Life Steal Flat>
 *    flat = user.mhp;
 *   </Custom HP Life Steal Flat>
 *
 *   <Custom MP Life Steal Flat>
 *    flat = user.mhp;
 *   </Custom MP Life Steal Flat>
 *   'flat'変数は、スキル/アイテムが対象に与えたダメージに基づいて
 *   対象からライフスティールするHP/MPの量です。
 *   これはflat値です。
 *
 * --- アクター、職業、敵、武器、防具、ステートのメモタグ ---
 *
 *   <Custom HP Life Steal Physical Rate>
 *    rate = user.hpRate();
 *   </Custom HP Life Steal Physical Rate>
 *
 *   <Custom HP Life Steal Magical Rate>
 *    rate = user.hpRate();
 *   </Custom HP Life Steal Magical Rate>
 *
 *   <Custom HP Life Steal Certain Rate>
 *    rate = user.hpRate();
 *   </Custom HP Life Steal Certain Rate>
 *
 *   <Custom MP Life Steal Physical Rate>
 *    rate = user.hpRate();
 *   </Custom MP Life Steal Physical Rate>
 *
 *   <Custom MP Life Steal Magical Rate>
 *    rate = user.hpRate();
 *   </Custom MP Life Steal Magical Rate>
 *
 *   <Custom MP Life Steal Certain Rate>
 *    rate = user.hpRate();
 *   </Custom MP Life Steal Certain Rate>
 *   'rate'変数は、バトラーが与えられたダメージに対する
 *   HP/MPの目標からのライフスティール率のボーナスです。
 *   これは百分率であり、乗法的に積み重ねられます。
 *
 *   --- --- ---
 *
 *   <Custom HP Life Steal Physical Flat>
 *    flat = user.mhp;
 *   </Custom HP Life Steal Physical Flat>
 *
 *   <Custom HP Life Steal Magical Flat>
 *    flat = user.mhp;
 *   </Custom HP Life Steal Magical Flat>
 *
 *   <Custom HP Life Steal Certain Flat>
 *    flat = user.mhp;
 *   </Custom HP Life Steal Certain Flat>
 *
 *   <Custom MP Life Steal Physical Flat>
 *    flat = user.mhp;
 *   </Custom MP Life Steal Physical Flat>
 *
 *   <Custom MP Life Steal Magical Flat>
 *    flat = user.mhp;
 *   </Custom MP Life Steal Magical Flat>
 *
 *   <Custom MP Life Steal Certain Flat>
 *    flat = user.mhp;
 *   </Custom MP Life Steal Certain Flat>
 *   'flat'変数は、バトラーが与えられたダメージに対する
 *   HP/MPの目標からのライフスティールスティールのボーナス量です。
 *   これはフラットなボーナス値であり、相加的に積み重ねられます。
 *
 * ===========================================================================
 * Changelog
 * ===========================================================================
 *
 * Version 1.04:
 * - Bypass the isDevToolsOpen() error when bad code is inserted into a script
 * call or custom Lunatic Mode code segment due to updating to MV 1.6.1.
 *
 * Version 1.03:
 * - Updated for RPG Maker MV version 1.5.0.
 *
 * Version 1.02:
 * - Lunatic Mode fail safes added.
 *
 * Version 1.01:
 * - Updated for RPG Maker MV version 1.1.0.
 *
 * Version 1.00:
 * - Finished Plugin!
 */
