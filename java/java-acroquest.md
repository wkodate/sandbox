Java本格入門
～モダンスタイルによる基礎からオブジェクト指向・実用ライブラリまで
==

# 基本

## 修飾子

* transient
    * 指定したフィールドをシリアライズの対象から除外
    * シリアライズは、オブジェクトをバイト列に変換すること。オブジェクトをファイルやDBに保存したりネットワークで送受信するときに使う。staticなフィールドのシリアライズの対象外
* volatile
    * マルチスレッドからアクセスされるフィールドに対して、スレッド毎に値がキャッシュされないように指定
* synchronized
    * マルチスレッドで同時に1つのスレッドからしかアクセスされないことを保証

# 型

## プリミティブ型とラッパークラス(参照型)

* プリミティブ型の初期値は0, ラッパークラスの初期値はnull
* 数値計算に利用する変数はプリミティブ型を使う
* ファイルやデータベース、HTTPリクエストなどを保持する場合はラッパークラスを使う
* オートボクシングやアンボクシングは原則利用せず、明示的な変換を行う。記述量の削減が有効の場合にだけ使う
    * プリミティブ型からラッパークラスへの自動変換をオートボクシングと呼ぶ

## オブジェクトの等価性

* オブジェクトが等価であると判定するには以下を満たす必要がある
    * オブジェクトのhashCodeメソッドの値が一致
    * equalsで等価
* equalsは値の比較、hashCodeはオブジェクトのハッシュ値による比較
* hashCodeのハッシュは異なるオブジェクトでもハッシュ値が同じになることがあるため、hashCodeでオブジェウトを比較した後equalsで厳密な判定を行う必要がある

## ジェネリクス

* 汎用的な処理を記述する際に利用される型。<>のダイヤモンドオペレータを使う
* Listなどで特定のオブジェクトしか追加できない、値を取り出すときにキャストが不要になる
* クラスを作成するには仮想パラメータを用いてパラメータ化された型として定義する

# 配列、コレクション

## 配列

* Arrays.toStringで配列の要素一覧を文字列で確認できる
* 配列ではほとんどComparatorを使う、Comparableを利用することはあまりない
* 配列は一度作成してしまうと要素を変更できない。要素数を変更したい場合は、新しい配列を作成した上で古い配列をそこにコピーする

## List

* indexOfメソッドは、引数で指定した値と一致する要素のインデックスを取得する。先頭から最初に見つかった要素のインデックスを返す
* ArrayList
    * インスタンスを作るときは、長さを指定しないと内部で長さ10の配列が作られる
    * 配列のサイズ以上の要素を追加しようとした際には、新たに配列を作って元の配列をコピー、が実行されるため、可能であればコンストラクタで初期値を指定したほうが良い
    * インデックスを指定して要素を追加削除するのは高速。内部に配列を持っているため
    * リストの途中に要素を追加するときは、それ移行の要素をすべて1つずつ後ろにずらさなければいけないので時間がかかってしまう
* LinkedList
    * 要素自身が前後の要素のリンクをもっているため、ArrayListのような初期サイズの概念はない
    * 途中の要素の追加は高速。前後の要素のリンクを変更するだけのため
    * インデックスを指定して要素の追加は時間がかかる。先頭から順番にリストをたどる必要があるため
* CopyOnWriteArrayList
    * ArrayListをスレッドセーフ化したクラス
* 使い分け
    * 配列の途中で要素の追加や削除を行うことが多い場合は、LinkedList
    * for文などを使った全体的な繰り返し処理が多い場合は、ArrayList
    * 複数スレッドから同時にアクセスする場合は、CopyOnWriteArrayList

## Map

* HashMap
    * ハッシュテーブルでキーと値を持つ
    * キーはhashCodeメソッドの結果をハッシュテーブルのサイズで割ったときの余りがインデックスになる
    * 要素を追加する際にハッシュテーブルのサイズを拡張する処理と要素の追加処理が同時に行われると、無限ループになる可能性があるため、複数スレッドから扱うときには注意が必要
* LinkedHashMap
    * 要素が前後の情報を持っているHashMap
* TreeMap
    * 二分探索で要素をソートするクラス
    * ソートされた状態で保持される
    * 追加・削除・検索はO(logn)の計算量になる
* 使い分け
    * キーの大小を意識した部分集合を取り扱う場合は、TreeMap
    * 要素の順序を保持する必要がある場合は、LinkedHashMap
    * 複数スレッドから同時にアクセスする場合は、ConcurrentHashMap
    * その他の場合は、HashMap

## Set

* Setの内部にはMapが存在し、Setに追加された要素はMapのキーとして保持される。値にはObject型のインスタンスが格納されるがこれが使われることはない

# 例外

* Exception
    * プログラム作成時に想定できる以上を通知するために使用
    * 検査例外
    * プログラム作成時に想定できない異常
    * 例外処理を記述したかどうかをコンパイラが検査する例外
    * Javaはこっちが基本
    * catchするかthrowするかが必須。これをしないとコンパイルエラーになる
* RuntimeException
    * プログラム作成時に想定されないエラーを通知するために使用
    * 非検査例外
    * プログラム作成時に想定できる異常
    * コンパイラが検査しない例外。コンパイルエラーにはならない
    * try-catchを強制されない、throws句で宣言しなくても問題ない
* Error
    * システムの動作を継続できない致命的なエラー
    * エラー
    * 回復不可能なトラブルが発生
    * OutOfMemoryErrorなど
    * 例外処理することを求められていない

# 文字列処理

* +やconcatメソッドは、ループの回数だけオブジェクトや配列の生成処理をしているので処理が遅い
* StringBuilderは文字列を配列で保持している。余裕を持ったサイズの配列を作っている
* 使い分け
    * ローカル変数など、複数スレッドからアクセスされない場合は、StringBuilderを使う
    * 複数スレッドから使われる場合は、StringBufferを使う

# ファイル操作

* Pathクラスを使用する
* テキストファイルの読み込みはnewBufferedReaderメソッドでBufferedReaderインスタンスを生成する

# 日付処理

* Date and Time APIが追加され、従来のDateクラスとCalendarクラスであった問題を解消
* 日時はLocalDateTimeクラスを使う。日付はLocalDateクラス、時間はLocalTimeクラス
* 文字列変換はDateTimeFomatterクラス。スレッドセーフでありインスタンスの使い回しが可能

# オブジェクト指向

* 可視性のグッドプラクティス
    * 原則、フィールドはprivate、外部からアクセスするメソッドのみpublic
    * 拡張性やテスト容易性を上げるためにprotectedにする
    * privateなフィールドやメソッドにアクセスするためにリフレクションを使うより、フィールドの可視性を広くするほうが手軽なのでおすすめ
* ライフサイクルのグッドプラクティス
    * クラス変数よりインスタンス変数、インスタンス変数よりローカル変数を使用する
        * メソッドの引数に渡す変数が多いからと言ってインスタンス変数にするのはやめよう。マルチスレッドでセットしてから処理するまでに値が変わる可能性がある
    * フィールドを持たないクラスはライフサイクルを長くする。GCの量を減らすため
* ユーティリティクラスは自体はstaticでないメソッドで構成して、呼び出す側でインスタンスをstaticで生成するのがおすすめ
* インターフェースと抽象クラス
    * ポリモーフィズムの概念を実現するための機能
    * インターフェース
        * インスタンス変数を持つことができない。定数を持つことはできる
    * 抽象クラス
        * abstractメソッドを宣言することができる。抽象クラス自体はインスタンスを生成できない。それ以外はクラスと同じ
    * インターフェースは定義に使う、抽象クラスは雛形や共通処理に使う

# スレッドセーフ

* スレッドセーフの条件は、複数のスレッドから読み書きを行っても
    * データが破損しない
    * 処理エラーが発生しない
    * デッドロックや処理停止が発生しない
* インスタンス変数を複数のスレッドで共有するときに問題が起きやすい

# ライブラリ

* Apache Commonsが便利
    * StringUtils.isEmptyメソッド、HashCodeBuilderクラス、EqualsBuilderクラスなど
* CSV処理はSuperCSV
* JSONを扱うならJackson
* ロギングはSLF4J + Logback
    * SLF4Jは様々なロギングライブラリをラップして統一的に扱えるようにしたライブラリ
    * LogbackはLog4jの後継になるロギングライブラリ
    * 設定ファイルはlogback.xml
