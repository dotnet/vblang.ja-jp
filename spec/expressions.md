---
ms.openlocfilehash: 8f250f8cee957fa60e7970ace250e8d7ce145fd7
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306169"
---
# <a name="expressions"></a>式

式は、値の計算を指定する、または変数または定数を指定する演算子とオペランドのシーケンスです。 この章では、構文、オペランドと演算子の評価順序、および式の意味を定義します。

```antlr
Expression
    : SimpleExpression
    | TypeExpression
    | MemberAccessExpression
    | DictionaryAccessExpression
    | InvocationExpression
    | IndexExpression
    | NewExpression
    | CastExpression
    | OperatorExpression
    | ConditionalExpression
    | LambdaExpression
    | QueryExpression
    | XMLLiteralExpression
    | XMLMemberAccessExpression
    ;
```

## <a name="expression-classifications"></a>式の分類

すべての式は、次のいずれかに分類されます。

* *値。* すべての値には、型が関連付けられています。

* *変数。* すべての変数には、関連付けられた型、つまり変数の宣言型があります。

* *名前空間。* この分類の式は、メンバーアクセスの左側としてのみ使用できます。 その他のコンテキストでは、名前空間として分類された式によってコンパイル時エラーが発生します。

* *型。* この分類の式は、メンバーアクセスの左側としてのみ使用できます。 その他のコンテキストでは、型として分類された式によってコンパイル時エラーが発生します。

* *メソッドグループ*。同じ名前でオーバーロードされた一連のメソッドです。 メソッドグループには、関連付けられたターゲット式と、関連付けられた型引数リストを含めることができます。

* メソッドの場所を表す*メソッドポインター* 。 メソッドポインターには、関連付けられたターゲット式と、関連する型引数リストを含めることができます。

* *ラムダメソッド (* 匿名メソッド)。

* *プロパティグループ*。同じ名前でオーバーロードされた一連のプロパティです。 プロパティグループには、関連付けられたターゲット式を含めることができます。

* *プロパティアクセス。* すべてのプロパティアクセスには、関連付けられた型、つまりプロパティの型があります。 プロパティアクセスには、関連付けられたターゲット式を含めることができます。

* *遅延バインディングアクセス*。実行時まで遅延されるメソッドまたはプロパティアクセスを表します。 遅延バインディングアクセスには、関連付けられたターゲット式と、関連付けられた型引数リストが含まれる場合があります。 遅延バインディングアクセスの種類は常に `Object` です。

* *イベントアクセス。* すべてのイベントアクセスには、関連付けられた型、つまりイベントの種類があります。 イベントアクセスには、関連付けられたターゲット式を含めることができます。 イベントアクセスは、`RaiseEvent`、`AddHandler`、および `RemoveHandler` ステートメントの最初の引数として表示される場合があります。 その他のコンテキストでは、イベントアクセスとして分類された式によってコンパイル時エラーが発生します。

* *配列リテラル*。型がまだ決定されていない配列の初期値を表します。

* *無効化.* このエラーは、式がサブルーチンの呼び出し、または結果のない await 演算子式の場合に発生します。 Void として分類される式は、呼び出しステートメントまたは await ステートメントのコンテキストでのみ有効です。

* *既定値。* この分類を生成するのは、リテラル `Nothing` だけです。

式の最終的な結果は、通常、値または変数です。他のカテゴリの式は、特定のコンテキストでのみ許可される中間値として機能します。

型パラメーターを持つ式は、式の型に特定の特性 (参照型、値型、何らかの型からの派生など) を指定する必要がある場合に、制約が適用されている場合に使用できます。型パラメーターでは、これらの特性を満たしています。

### <a name="expression-reclassification"></a>式の再分類

通常、式が式とは異なる分類を必要とするコンテキストで使用されると、コンパイル時エラーが発生します。たとえば、リテラルに値を割り当てようとした場合などです。 ただし、多くの場合、再*分類*のプロセスを通じて、式の分類を変更することができます。

再分類が成功した場合、再分類は拡大または縮小として判断されます。 特に明記されていない限り、この一覧のすべての再分類は拡大されます。

次の種類の式を再分類できます。

* 変数は、値として再分類できます。 変数に格納された値がフェッチされます。

* メソッドグループは、値として再分類できます。 メソッドグループ式は、関連するターゲット式と型パラメーターリストを持つ呼び出し式として解釈されます。また、空のかっこ (つまり、@no__t 0 は `f()` と解釈され、`f(Of Integer)` は `f(Of Integer)()` と解釈されます)。 この再分類により、式がさらに void として再分類される可能性があります。

* メソッドポインターは、値として再分類できます。 この再分類は、対象の型がわかっている変換のコンテキストでのみ発生します。 メソッドポインター式は、関連付けられた型引数リストを使用して、適切な型のデリゲートインスタンス化式への引数として解釈されます。 以下に例を示します。
    
    ```vb
    Delegate Sub D(i As Integer)
    
    Module Test
        Sub F(i As Integer)
        End Sub
    
        Sub Main()
            Dim del As D
    
            ' The next two lines are equivalent.
            del = AddressOf F
            del = New D(AddressOf F)
        End Sub
    End Module
    ```

* ラムダメソッドは、値として再分類できます。 ターゲットの型がわかっている変換のコンテキストで再分類が行われた場合、次の2つの再分類のいずれかが発生する可能性があります。
    
  1. 対象の型がデリゲート型の場合、ラムダメソッドは、適切な型のデリゲートコンストラクション式の引数として解釈されます。
    
  2. 対象の型が `System.Linq.Expressions.Expression(Of T)` で `T` がデリゲート型である場合、ラムダメソッドは @no__t 2 のデリゲート型構築式で使用されているかのように解釈され、式ツリーに変換されます。
    
  デリゲートに ByRef パラメーターがない場合、非同期または反復子のラムダメソッドは、デリゲートの構築式の引数としてのみ解釈できます。
    
  デリゲートのいずれかのパラメーター型から対応するラムダパラメーターの型への変換が縮小変換である場合、再分類は縮小として判断されます。それ以外の場合は、拡大されます。
    
  __付箋.__ ラムダメソッドと式ツリー間の正確な変換は、コンパイラのバージョン間では修正されない場合があり、この仕様の範囲を超えています。 Microsoft Visual Basic 11.0 では、次の制限に従って、すべてのラムダ式を式ツリーに変換できます。(1) 1。  式ツリーに変換できるのは、ByRef パラメーターを持たない単一行のラムダ式だけです。 単一行の @no__t 0 のラムダの場合、式ツリーに変換できるのは呼び出しステートメントだけです。 (2) 前のフィールド初期化子を使用して後続のフィールド初期化子を初期化する場合、匿名型式を式ツリーに変換することはできません (`New With {.a=1, .b=.a}` など)。 (3) 初期化中の現在のオブジェクトのメンバーがフィールド初期化子の1つ (`New C1 With {.a=1, .b=.Method1()}` など) で使用されている場合、オブジェクト初期化子式を式ツリーに変換することはできません。 (4) 多次元配列作成式は、要素の型を明示的に宣言している場合にのみ、式ツリーに変換できます。 (5) 遅延バインディング式を式ツリーに変換することはできません。 (6) 変数またはフィールドを呼び出し式に渡すが、ByRef パラメーターとまったく同じ型がない場合、またはプロパティが byref で渡される場合、通常の VB セマンティクスとして、引数のコピーが ByRef で渡され、その最終的な値がコピーされます。 変数、フィールド、またはプロパティに戻ります。 式ツリーでは、コピーバックは行われません。 (7) これらすべての制限は、入れ子になったラムダ式にも適用されます。
    
  対象の型が不明な場合、ラムダメソッドは、ラムダメソッドの同じシグネチャを持つ匿名デリゲート型のデリゲートインスタンス化式への引数として解釈されます。 厳密なセマンティクスが使用されていて、いずれかのパラメーターの型が省略されている場合、コンパイル時エラーが発生します。それ以外の場合は、不足しているパラメーターの型に対して `Object` が置き換えられます。 以下に例を示します。
    
  ```vb
  Module Test
      Sub Main()
          ' Type of x will be equivalent to Func(Of Object, Object, Object)
          Dim x = Function(a, b) a + b
  
          ' Type of y will be equivalent to Action(Of Object, Object)
          Dim y = Sub(a, b) Console.WriteLine(a + b)
      End Sub
  End Module
  ```

* プロパティグループは、プロパティアクセスとして再分類できます。 プロパティグループ式は、空のかっこを含むインデックス式として解釈されます (つまり、@no__t 0 は `f()` と解釈されます)。

* プロパティ アクセスは、値として再分類できます。 プロパティアクセス式は、プロパティの @no__t 0 アクセサーの呼び出し式として解釈されます。 プロパティに getter がない場合、コンパイル時エラーが発生します。

* 遅延バインディングアクセスは、遅延バインディングされたメソッドまたは遅延バインディングプロパティアクセスとして再分類できます。 遅延バインディングアクセスをメソッドアクセスとして、またプロパティアクセスとして再分類できる状況では、プロパティアクセスへの再分類が推奨されます。

* 遅延バインディングされたアクセスは、値として再分類できます。

* 配列リテラルは、値として再分類できます。 値の型は次のように決定されます。

  1. 対象の型が認識され、ターゲットの型が配列型である変換のコンテキストで再分類が行われた場合、配列リテラルは T () 型の値として再分類されます。 ターゲットの型が `System.Collections.Generic.IList(Of T)`、`IReadOnlyList(Of T)`、`ICollection(Of T)`、`IReadOnlyCollection(Of T)`、または `IEnumerable(Of T)` で、配列リテラルに1つの入れ子レベルがある場合、配列リテラルは `T()` 型の値として再分類されます。

  2. それ以外の場合、配列リテラルは、入れ子のレベルと等しいランクの配列である型を持つ値に再分類されます。要素型は、初期化子の要素の中で最も優先度の高い型によって決定されます。優先される型を特定できない場合は、`Object` が使用されます。 以下に例を示します。

     ```vb
     ' x Is GetType(Double(,,))
     Dim x = { { { 1, 2.0 }, { 3, 4 } }, { { 5, 6 }, { 7, 8 } } }.GetType()
        
     ' y Is GetType(Integer())
     Dim y = { 1, 2, 3 }.GetType()
        
     ' z Is GetType(Object())
     Dim z = { 1, "2" }.GetType()
        
     ' Error: Inconsistent nesting
     Dim a = { { 10 }, { 20, 30 } }.GetType()
     ```

  __付箋.__ バージョン9.0 とバージョン10.0 の言語の動作が若干変更されています。 10.0 より前では、配列要素初期化子はローカル変数型の推定に影響を与えませんでした。 そのため `Dim a() = { 1, 2, 3 }` は、言語のバージョン9.0 で `a` の型として `Object()` を推定し、バージョン10.0 では `Integer()` になります。

  その後、再分類は配列作成式として配列リテラルを再解釈します。 次に例を示します。

  ```vb
  Dim x As Double = { 1, 2, 3, 4 }
  Dim y = { "a", "b" }
  ```

  は次と同じです。

  ```vb
  Dim x As Double = New Double() { 1, 2, 3, 4 }
  Dim y = New String() { "a", "b" }
  ```

  要素式から配列要素型への変換が縮小されている場合、再分類は縮小として判断されます。それ以外の場合は、拡大として判断されます。

* 既定値 `Nothing` は、値として再分類できます。 ターゲット型がわかっているコンテキストでは、結果は対象の型の既定値になります。 対象の型が不明なコンテキストでは、結果は `Object` 型の null 値になります。

名前空間式、型式、イベントアクセス式、または void 式を再分類することはできません。 複数の再分類を同時に実行できます。 以下に例を示します。

```vb
Module Test
    Sub F(i As Integer)
    End Sub

    ReadOnly Property P() As Integer
        Get
        End Get
    End Sub

    Sub Main()
        F(P)
    End Property
End Module
```

この場合、プロパティグループ式 `P` は、まずプロパティグループからプロパティアクセスに再分類し、次にプロパティアクセスから値に再分類します。 コンテキスト内の有効な分類に至るまで、最も少ない再分類の数が実行されます。

## <a name="constant-expressions"></a>定数式

*定数式*は、コンパイル時に値を完全に評価できる式です。

```antlr
ConstantExpression
    : Expression
    ;
```

定数式の型は、0、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`、`Char`、`Single`、3、4、5、または任意の列挙型に @no__t することができます。 定数式では、次の構造体を使用できます。

* リテラル (@no__t 0 を含む)。

* 定数型のメンバーまたは定数のローカル変数への参照。

* 列挙型のメンバーへの参照。

* かっこで囲まれた部分式。

* 変換式は、対象の型が上記のいずれかの型である場合に指定します。 @No__t-0 との間の強制変換はこの規則の例外であり、`String` の変換は実行時に実行環境の現在のカルチャで常に実行されるため、null 値でのみ使用できます。 定数強制変換式では、組み込み変換のみを使用できます。

* @No__t-0、`-`、`Not` の単項演算子は、オペランドと結果が上記の型になります。

* @No__t-0、`-`、`*`、`^`、`Mod`、`/`、`\`、`<<`、`>>`、`&`、@no__t、1、2、3、4、5、6、7、8、9、0 の2項演算子各オペランドと結果は、上に示した型になります。

* 条件演算子は、各オペランドと結果が、上に示した型である場合に指定します。

* 次の実行時関数: `Microsoft.VisualBasic.Strings.ChrW`;定数値が 0 ~ 128 の場合は-1 を @no__t します。定数文字列が空でない場合は、`Microsoft.VisualBasic.Strings.AscW`定数文字列が空でない場合は、-3 を @no__t ます。

定数式では、次のコンストラクトは使用でき*ません*。

* @No__t 0 コンテキストを使用した暗黙的なバインド。

整数型の定数式 (`ULong`、`Long`、`UInteger`、`Integer`、`UShort`、`Short`、`SByte`、または @no__t) は、より狭い整数型に暗黙的に変換できます。また、型 @no__t の定数式を暗黙的にに変換することもでき `Single`。定数式の値が変換先の型の範囲内にあることを示します。 これらの縮小変換は、寛容または厳格なセマンティクスが使用されているかどうかにかかわらず許可されます。


## <a name="late-bound-expressions"></a>遅延バインディング式

メンバーアクセス式またはインデックス式の対象が型 `Object` の場合、実行時まで式の処理が遅延される可能性があります。 このように処理を延期すると、*遅延バインディング*と呼ばれます。 遅延バインディングでは、@no__t 0 の変数を型指定されてい*ない方法で*使用できます。この場合、メンバーのすべての解決は、変数の値の実際の実行時の型に基づいています。 厳密なセマンティクスがコンパイル環境で指定される場合、または @no__t 0 によって指定される場合は、遅延バインディングによってコンパイル時エラーが発生します。 非パブリックメンバーは、オーバーロードの解決のために、遅延バインディングを行うときに無視されます。 事前バインディングされたケースとは異なり、`Shared` メンバーの遅延バインディングを呼び出したり、アクセスしたりすると、実行時に呼び出しターゲットが評価されることに注意してください。 式が `System.Object` で定義されたメンバーの呼び出し式である場合、遅延バインディングは行われません。

一般に、遅延バインディングのアクセスは、式の実際の実行時の型で識別子を検索することによって、実行時に解決されます。 実行時に遅延バインディングメンバー参照が失敗した場合は、@no__t 0 の例外がスローされます。 遅延バインディングされたメンバーの参照は、関連付けられているターゲット式の実行時の型からのみ行われるため、オブジェクトの実行時の型はインターフェイスではありません。 したがって、遅延バインディングされたメンバーアクセス式でインターフェイスメンバーにアクセスすることはできません。

遅延バインディングメンバーアクセスの引数は、メンバーアクセス式に出現する順序で評価されます。これは、遅延バインディングされたメンバーでパラメーターが宣言されている順序ではありません。 次の例は、この違いを示しています。

```vb
Class C
    Public Sub f(ByVal x As Integer, ByVal y As Integer)
    End Sub
End Class

Module Module1
    Sub Main()
        Console.Write("Early-bound: ")
        Dim c As C = New C
        c.f(y:=t("y"), x:=t("x"))

        Console.Write(vbCrLf & "Late-bound: ")
        Dim o As Object = New C
        o.f(y:=t("y"), x:=t("x"))
    End Sub

    Function t(ByVal s As String) As Integer
        Console.Write(s)
        Return 0
    End Function
End Module
```

次のコードが表示されます。

```console
Early-bound: xy
Late-bound: yx
```

遅延バインディングされたオーバーロードの解決は、引数の実行時の型に対して行われるため、コンパイル時または実行時のどちらで評価されるかに基づいて、式が異なる結果を生成する可能性があります。 次の例は、この違いを示しています。

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class

Module Test
    Sub F(b As Base)
        Console.WriteLine("F(Base)")
    End Sub

    Sub F(d As Derived)
        Console.WriteLine("F(Derived)")
    End Sub

    Sub Main()
        Dim b As Base = New Derived()
        Dim o As Object = b

        F(b)
        F(o)
    End Sub
End Module
```

次のコードが表示されます。

```console
F(Base)
F(Derived)
```

## <a name="simple-expressions"></a>単純式

単純式は、リテラル、かっこで囲まれた式、インスタンス式、または単純な名前式です。

```antlr
SimpleExpression
    : LiteralExpression
    | ParenthesizedExpression
    | InstanceExpression
    | SimpleNameExpression
    | AddressOfExpression
    ;
```

### <a name="literal-expressions"></a>リテラル式

リテラル式は、リテラルによって表される値に評価されます。 リテラル式は、値として分類されます。ただし、リテラル `Nothing` は既定値として分類されます。

```antlr
LiteralExpression
    : Literal
    ;
```

### <a name="parenthesized-expressions"></a>かっこで囲まれる式

かっこで囲まれた式は、かっこで囲まれた式で構成されます。 かっこで囲まれた式は値として分類され、囲まれた式は値として分類される必要があります。 かっこで囲まれた式は、かっこ内の式の値に評価されます。

```antlr
ParenthesizedExpression
    : OpenParenthesis Expression CloseParenthesis
    ;
```

### <a name="instance-expressions"></a>インスタンス式

*インスタンス式*は、キーワード `Me` です。 これは、共有されていないメソッド、コンストラクター、またはプロパティアクセサーの本体内でのみ使用できます。 値として分類されます。 キーワード `Me` は、実行されるメソッドまたはプロパティアクセサーを格納している型のインスタンスを表します。 コンストラクターが別のコンストラクター (セクション[コンストラクター](type-members.md#constructors)) を明示的に呼び出す場合、そのコンストラクターが呼び出されるまでは、インスタンスがまだ構築されていないため、`Me` を使用することはできません。

```antlr
InstanceExpression
    : 'Me'
    ;
```

### <a name="simple-name-expressions"></a>単純な名前の式

*単純な名前の式*は、単一の識別子の後に省略可能な型引数リストを指定して構成されます。

```antlr
SimpleNameExpression
    : Identifier ( OpenParenthesis 'Of' TypeArgumentList CloseParenthesis )?
    ;
```

名前は解決され、次の "単純な名前解決規則" によって分類されます。

1.  すぐ外側のブロックから開始し、外側の外側の各ブロック (存在する場合) を続けて、識別子がローカル変数、静的変数、定数のローカル、メソッドの型パラメーター、またはパラメーターの名前と一致する場合、識別子はを参照します。一致するエンティティ。

    識別子がローカル変数、静的変数、または定数 local に一致し、型引数リストが指定されている場合、コンパイル時エラーが発生します。 識別子がメソッド型パラメーターと一致し、型引数リストが指定されている場合、一致は発生せず、解決は続行されます。 識別子がローカル変数と一致する場合、一致したローカル変数は暗黙的な関数または `Get` アクセサーはローカル変数を返し、式は呼び出し式、呼び出しステートメント、または `AddressOf` 式の一部になり、次に一致しません。が発生し、解決が続行されます。

    式は、ローカル変数、静的変数、またはパラメーターである場合、変数として分類されます。 式は、メソッド型パラメーターの場合、型として分類されます。 定数がローカルである場合、式は値として分類されます。

2.  式を含む入れ子にされた型ごとに、最も内側から外側に向かっています。型の識別子を参照すると、アクセス可能なメンバーとの一致が生成されます。

    21. 一致する型のメンバーが型パラメーターの場合、結果は型として分類され、対応する型パラメーターになります。 型引数リストが指定された場合、一致は発生せず、解決は続行されます。
    22. それ以外の場合、型がすぐ外側の型であり、参照によって共有されていない型のメンバーが識別されると、結果は `Me.E(Of A)` という形式のメンバーアクセスと同じになります。ここで `E` は識別子、`A` は型引数リストです。(存在する場合)。
    23. それ以外の場合、結果は `T.E(Of A)` という形式のメンバーアクセスとまったく同じです。ここで `T` は一致するメンバーを含む型、`E` は @no__t 識別子、は型引数リスト (存在する場合) です。 この場合、識別子が非共有メンバーを参照しているとエラーになります。

3.  入れ子になった名前空間ごとに、最も内側から最も外側の名前空間に移動し、次の手順を実行します。

    31. 名前空間に、指定した名前のアクセス可能な型が含まれていて、型引数リストで指定されたものと同じ数の型パラメーターを持っている場合、識別子はその型を参照し、型として分類されます。
    32. それ以外の場合、型引数リストが指定されておらず、名前空間に指定された名前の名前空間メンバーが含まれている場合、識別子はその名前空間を参照し、名前空間として分類されます。
    33. それ以外の場合、名前空間にアクセス可能な標準モジュールが1つ以上含まれており、その識別子のメンバー名の参照によって1つの標準モジュールでアクセス可能な一致が生成されると、結果は `M.E(Of A)` の形式のメンバーアクセスとまったく同じになり @no__t は、対応するメンバーが含まれている標準モジュールで、@no__t 2 は識別子、`A` は型引数リスト (存在する場合) です。 識別子が複数の標準モジュールのアクセス可能な型メンバーと一致する場合、コンパイル時エラーが発生します。

4.  ソースファイルに1つ以上のインポートエイリアスがあり、識別子がいずれか1つの名前と一致する場合、識別子はその名前空間または型を参照します。 型引数リストが指定されている場合、コンパイル時エラーが発生します。

5. 名前参照を含むソースファイルに1つ以上のインポートがある場合は、次のようになります。

    51. 識別子が、型引数リストで指定されたものと同じ数の型パラメーターを持つアクセス可能な型の名前 (存在する場合) または型のメンバーである場合、識別子はその型または型のメンバーを参照します。 識別子が複数のインポートで一致する場合、型引数リストで指定された型パラメーターの数と同じ数の型パラメーターを持つアクセス可能な型の名前 (存在する場合)、またはアクセス可能な型のメンバーである場合は、コンパイル時エラーが発生します。
    52. それ以外の場合、型引数リストが指定されておらず、識別子がアクセス可能な型の名前空間の名前を厳密に1つのインポートで一致する場合、識別子はその名前空間を参照します。 型引数リストが指定されておらず、識別子がアクセス可能な型の名前空間の名前を複数のインポートで一致する場合、コンパイル時エラーが発生します。
    53. それ以外の場合、インポートにアクセス可能な標準モジュールが1つ以上含まれており、その識別子のメンバー名参照によって1つの標準モジュールでアクセス可能な一致が生成された場合、結果は `M.E(Of A)` の形式のメンバーアクセスとまったく同じになり `M` は、一致するメンバーを含む標準モジュールであり、@no__t 2 は識別子、`A` は型引数リスト (存在する場合) です。 識別子が複数の標準モジュールのアクセス可能な型メンバーと一致する場合、コンパイル時エラーが発生します。

6.  コンパイル環境で1つ以上のインポートエイリアスが定義されており、その識別子がいずれか1つの名前と一致する場合、識別子はその名前空間または型を参照します。 型引数リストが指定されている場合、コンパイル時エラーが発生します。

7. コンパイル環境で1つ以上のインポートを定義する場合は、次のようになります。

    71. 識別子が、型引数リストで指定されたものと同じ数の型パラメーターを持つアクセス可能な型の名前 (存在する場合) または型のメンバーである場合、識別子はその型または型のメンバーを参照します。 識別子が複数のインポートで一致する場合は、型引数リストで指定されたものと同じ数の型パラメーターを持つアクセス可能な型の名前 (存在する場合)、または型のメンバーである場合、コンパイル時エラーが発生します。
    72. それ以外の場合、型引数リストが指定されておらず、識別子がアクセス可能な型の名前空間の名前を厳密に1つのインポートで一致する場合、識別子はその名前空間を参照します。 型引数リストが指定されておらず、識別子がアクセス可能な型の名前空間の名前を複数のインポートで一致する場合、コンパイル時エラーが発生します。
    73. それ以外の場合、インポートにアクセス可能な標準モジュールが1つ以上含まれており、その識別子のメンバー名参照によって1つの標準モジュールでアクセス可能な一致が生成された場合、結果は `M.E(Of A)` の形式のメンバーアクセスとまったく同じになり `M` は、一致するメンバーを含む標準モジュールであり、@no__t 2 は識別子、`A` は型引数リスト (存在する場合) です。 識別子が複数の標準モジュールのアクセス可能な型メンバーと一致する場合、コンパイル時エラーが発生します。

8. それ以外の場合、識別子によって指定された名前は定義されません。

未定義の単純な名前式は、コンパイル時エラーになります。

通常、名前は特定の名前空間に1回だけ出現できます。 ただし、複数の .NET アセンブリにわたって名前空間を宣言できるため、2つのアセンブリが同じ完全修飾名を持つ型を定義する状況が発生する可能性があります。 この場合、ソースファイルの現在のセットで宣言されている型は、外部の .NET アセンブリで宣言されている型よりも優先されます。 それ以外の場合、名前はあいまいであり、名前を明確に区別する方法はありません。


### <a name="addressof-expressions"></a>AddressOf 式

メソッドポインターを生成するには、@no__t 0 の式を使用します。 式は、@no__t 0 キーワードと、メソッドグループまたは遅延バインディングアクセスとして分類する必要がある式で構成されます。 メソッドグループはコンストラクターを参照できません。

結果はメソッドポインターとして分類され、メソッドグループと同じ関連するターゲット式と型引数リスト (存在する場合) があります。

```antlr
AddressOfExpression
    : 'AddressOf' Expression
    ;
```

## <a name="type-expressions"></a>型の式

*型の式*は、@no__t 1 の式、@no__t 2 の式、`Is` の式、または `GetXmlNamespace` 式です。

```antlr
TypeExpression
    : GetTypeExpression
    | TypeOfIsExpression
    | IsExpression
    | GetXmlNamespaceExpression
    ;
```

### <a name="gettype-expressions"></a>GetType 式

@No__t 0 の式は、キーワード `GetType`、型の名前で構成されます。

```antlr
GetTypeExpression
    : 'GetType' OpenParenthesis GetTypeTypeName CloseParenthesis
    ;

GetTypeTypeName
    : TypeName
    | QualifiedOpenTypeName
    ;

QualifiedOpenTypeName
    : Identifier TypeArityList? (Period IdentifierOrKeyword TypeArityList?)*
    | 'Global' Period IdentifierOrKeyword TypeArityList?
      (Period IdentifierOrKeyword TypeArityList?)*
    ;

TypeArityList
    : OpenParenthesis 'Of' CommaList? CloseParenthesis
    ;

CommaList
    : Comma Comma*
    ;
```

@No__t 0 の式は値として分類され、その値は*Gettypetypename*を表すリフレクション (`System.Type`) クラスです。 *Gettypetypename*が型パラメーターの場合、式は、実行時に型パラメーターに指定された型引数に対応する `System.Type` オブジェクトを返します。

*Gettypetypename*は、次の2つの方法で特別です。

* この型名が参照される言語の唯一の場所である @no__t 0 にすることができます。

* 型引数を省略して構築されたジェネリック型である可能性があります。 これにより、`GetType` 式は、ジェネリック型自体に対応する @no__t 1 オブジェクトを返すことができます。

次の例は、@no__t 0 の式を示しています。

```vb
Module Test
    Sub Main()
        Dim t As Type() = { GetType(Integer), GetType(System.Int32), _
            GetType(String), GetType(Double()) }
        Dim i As Integer

        For i = 0 To t.Length - 1
            Console.WriteLine(t(i).Name)
        Next i
    End Sub
End Module
```

結果の出力は次のようになります。

```console
Int32
Int32
String
Double[]
```


### <a name="typeofis-expressions"></a>TypeOf...Is 式

値の実行時の型が特定の型と互換性があるかどうかを確認するには、`TypeOf...Is` 式を使用します。 最初のオペランドは、値として分類する必要があります。また、再分類するラムダメソッドにすることはできません。また、参照型または制約のない型パラメーター型である必要があります。 2番目のオペランドは型名である必要があります。 式の結果は値として分類され、@no__t 0 の値になります。 オペランドのランタイム型に id、既定値、参照、配列、値型、または型パラメーターから型への変換がある場合、式は `True` に評価されます。それ以外の場合は、`False` になります。 式の型と特定の型の間に変換が存在しない場合、コンパイル時にエラーが発生します。

```antlr
TypeOfIsExpression
    : 'TypeOf' Expression 'Is' LineTerminator? TypeName
    ;
```

### <a name="is-expressions"></a>Is 式

参照の等価性の比較を実行するには、@no__t 0 または `IsNot` 式を使用します。

```antlr
IsExpression
    : Expression 'Is' LineTerminator? Expression
    | Expression 'IsNot' LineTerminator? Expression
    ;
```

各式は値として分類する必要があり、各式の型は参照型、制約のない型パラメーター型、または null 許容値型である必要があります。 一方の式の型が制約のない型パラメーター型または null 許容値型である場合は、もう一方の式がリテラル `Nothing` である必要があります。

結果は値として分類され、`Boolean` として型指定されます。 両方の値が同じインスタンスを参照している場合、または両方の値が `Nothing` @no__t の場合、またはそれ以外の場合は、@no__t 0 演算が `True` と評価されます。 両方の値が同じインスタンスを参照している場合、または両方の値が `Nothing` @no__t の場合、またはそれ以外の場合は、@no__t 0 演算が `False` と評価されます。


### <a name="getxmlnamespace-expressions"></a>GetXmlNamespace 式

@No__t 0 の式は、キーワード `GetXmlNamespace` と、ソースファイルまたはコンパイル環境によって宣言された XML 名前空間の名前で構成されます。

```antlr
GetXmlNamespaceExpression
    : 'GetXmlNamespace' OpenParenthesis XMLNamespaceName? CloseParenthesis
    ;
```

@No__t 0 の式は値として分類され、その値は*XMLNamespaceName*を表す `System.Xml.Linq.XNamespace` のインスタンスになります。 その型が使用できない場合は、コンパイル時エラーが発生します。

以下に例を示します。

```vb
Imports <xmlns:db="http://example.org/database">

Module Test
    Sub Main()
        Dim db = GetXmlNamespace(db)

        ' The following are equivalent
        Dim customer1 = _
            New System.Xml.Linq.XElement(db + "customer", "Bob")
        Dim customer2 = <db:customer>Bob</>
    End Sub
End Module
```

かっこ内のすべての要素は名前空間名の一部と見なされるため、空白文字などの XML ルールが適用されます。 以下に例を示します。

```vb
Imports <xmlns:db-ns="http://example.org/database">

Module Test
    Sub Main()

        ' Error, XML name expected
        Dim db1 = GetXmlNamespace( db-ns )

        ' Error, ')' expected
        Dim db2 = GetXmlNamespace(db _
            )

        ' OK.
        Dim db3 = GetXmlNamespace(db-ns)
    End Sub
End Module
```

XML 名前空間式を省略することもできます。この場合、式は、既定の XML 名前空間を表すオブジェクトを返します。


## <a name="member-access-expressions"></a>メンバー アクセス式

メンバーアクセス式は、エンティティのメンバーにアクセスするために使用されます。

```antlr
MemberAccessExpression
    : MemberAccessBase? Period IdentifierOrKeyword
      ( OpenParenthesis 'Of' TypeArgumentList CloseParenthesis )?
    ;

MemberAccessBase
    : Expression
    | NonArrayTypeName
    | 'Global'
    | 'MyClass'
    | 'MyBase'
    ;
```

@No__t-0 の形式のメンバーアクセス。ここで `E` は式、非配列型の名前、キーワード `Global`、または省略されています。また、`I` は、省略可能な型引数リスト @no__t を持つ識別子で、次のように評価および分類されます。

1. @No__t-0 を省略した場合、直ちに `With` ステートメントを含む式が `E` に置き換えられ、メンバーアクセスが実行されます。 @No__t-0 ステートメントが含まれていない場合は、コンパイル時エラーが発生します。

2. @No__t-0 が名前空間として分類されているか `E` がキーワード `Global` の場合、メンバー参照は指定された名前空間のコンテキストで実行されます。 @No__t-0 は、型引数リストで指定されたものと同じ数の型パラメーターを持つ、その名前空間のアクセス可能なメンバーの名前である場合、結果はそのメンバーになります。 結果は、メンバーに応じて名前空間または型として分類されます。 それ以外の場合は、コンパイル時のエラーが発生します。

3. @No__t-0 が型または型として分類された式の場合、メンバー参照は、指定された型のコンテキストで実行されます。 @No__t-0 が `E` のアクセス可能なメンバーの名前である場合、`E.I` が評価され、次のように分類されます。

    31. @No__t-0 がキーワード `New` で、`E` が列挙型ではない場合、コンパイル時エラーが発生します。
    32. @No__t-0 の場合、型引数リストで指定された型パラメーターの数と同じ数の型パラメーターがある場合は、その型が返されます。
    33. @No__t-0 が1つ以上のメソッドを識別する場合、結果は、関連付けられた型引数リストを持ち、関連付けられたターゲット式がないメソッドグループになります。
    34. @No__t-0 が1つ以上のプロパティを識別し、型引数リストが指定されていない場合、結果はターゲット式が関連付けられていないプロパティグループになります。
    35. @No__t-0 が共有変数を識別し、型引数リストが指定されていない場合、結果は変数または値になります。 変数が読み取り専用で、参照が変数が宣言されている型の共有コンストラクターの外部で行われた場合、結果は `E` の共有変数 `I` になります。 それ以外の場合、結果は `E` の共有変数 `I` になります。
    36. @No__t-0 が共有イベントを識別し、型引数リストが指定されていない場合、結果はターゲット式が関連付けられていないイベントアクセスになります。
    37. @No__t-0 が定数を識別し、型引数リストが指定されていない場合、結果はその定数の値になります。
    38. @No__t-0 が列挙型のメンバーを識別し、型引数リストが指定されていない場合、結果はその列挙メンバーの値になります。
    39. それ以外の場合、`E.I` は無効なメンバー参照であり、コンパイル時エラーが発生します。

4. @No__t-0 が変数または値として分類されている場合、その型が-1 @no__t の場合、メンバー参照は `T` のコンテキストで実行されます。 @No__t-0 が `T` のアクセス可能なメンバーの名前である場合、`E.I` が評価され、次のように分類されます。

    41. @No__t-0 がキーワード `New`、`E` が `Me`、`MyBase`、または `MyClass` で、型引数が指定されていない場合、結果は @no__t に関連付けられたターゲット式を持つ @no__t の型のインスタンスコンストラクターを表すメソッドグループになります。型引数リストがありません。 それ以外の場合は、コンパイル時のエラーが発生します。
    42. @No__t-0 が1つ以上のメソッドを識別する場合 (`T` が @no__t ではない場合)、結果は、関連付けられた型引数リストと、関連付けられている対象式 `E` を持つメソッドグループになります。
    43. @No__t-0 が1つ以上のプロパティを識別し、型引数が指定されなかった場合、結果は `E` の対象式が関連付けられたプロパティグループになります。
    44. @No__t-0 が共有変数またはインスタンス変数を識別し、型引数が指定されていない場合、結果は変数または値のいずれかになります。 変数が読み取り専用で、変数が変数の種類 (shared または instance) に対して適切に宣言されているクラスのコンストラクターの外側で参照が発生した場合、結果はによって参照されるオブジェクトの変数 `I` になり `E`。 @No__t-0 が参照型の場合、結果は `E` によって参照されるオブジェクトの変数 `I` になります。 それ以外の場合、`T` が値の型で、式 `E` が変数として分類されると、結果は変数になります。それ以外の場合、結果は値になります。
    45. @No__t-0 によってイベントが識別され、型引数が指定されなかった場合、結果は `E` の関連付けられたターゲット式を使用したイベントアクセスになります。
    46. @No__t-0 が定数を識別し、型引数が指定されなかった場合、結果はその定数の値になります。
    47. @No__t-0 が列挙体のメンバーを識別し、型引数が指定されなかった場合、結果はその列挙体のメンバーの値になります。
    48. @No__t-0 @no__t が-1 の場合、結果は、関連付けられている型引数リストと、`E` の関連付けられたターゲット式を使用して、遅延バインディングアクセスとして分類された遅延バインディングメンバー参照になります。

5. それ以外の場合、`E.I` は無効なメンバー参照であり、コンパイル時エラーが発生します。

@No__t-0 という形式のメンバーアクセスは `Me.I(Of A)` と同じですが、アクセスされるすべてのメンバーは、メンバーがオーバーライド不可能であるかのように扱われます。 したがって、アクセスされるメンバーは、メンバーがアクセスされている値の実行時の型の影響を受けません。

@No__t-0 という形式のメンバーアクセスは、`CType(Me, T).I(Of A)` と同じです。 `T` は、メンバーアクセス式を含む型の直接の基本型です。 メソッド呼び出しは、呼び出されるメソッドがオーバーライド不可能であるかのように処理されます。 この形式のメンバーアクセスは、*基本アクセス*とも呼ばれます。

次の例は、`Me`、`MyBase`、`MyClass` がどのように関連しているかを示しています。

```vb
Class Base
    Public Overridable Sub F()
        Console.WriteLine("Base.F")
    End Sub
End Class

Class Derived
    Inherits Base

    Public Overrides Sub F()
        Console.WriteLine("Derived.F")
    End Sub

    Public Sub G()
        MyClass.F()
    End Sub
End Class

Class MoreDerived
    Inherits Derived

    Public Overrides Sub F()
        Console.WriteLine("MoreDerived.F")
    End Sub

    Public Sub H()
        MyBase.F()
    End Sub
End Class

Module Test
    Sub Main()
        Dim x As MoreDerived = new MoreDerived()

        x.F()
        x.G()
        x.H()
    End Sub

End Module
```

このコードは次の出力を出力します。

```console
MoreDerived.F
Derived.F
Derived.F
```

メンバーアクセス式がキーワード `Global` で始まる場合、キーワードは最も外側の名前空間を表します。これは、宣言が外側の名前空間をシャドウする場合に便利です。 @No__t-0 キーワードを使用すると、そのような状況で最も外側の名前空間に "エスケープ" できます。 以下に例を示します。

```vb
Class System
End Class

Module Test
    Sub Main()
        ' Error: Class System does not contain Console
        System.Console.WriteLine("Hello, world!") 


        ' Legal, binds to System in outermost namespace
        Global.System.Console.WriteLine("Hello, world!") 
    End Sub
End Module
```

上の例では、最初のメソッド呼び出しは無効です。識別子 `System` は、名前空間 `System` ではなく、クラス `System` にバインドされています。 @No__t-0 名前空間にアクセスする唯一の方法は、`Global` を使用して、最も外側の名前空間をエスケープすることです。

アクセスされているメンバーが共有されている場合、期間の左側の式は不要であり、メンバーアクセスが遅延バインディングされない限り評価されません。 次に例を示します。

```vb
Class C
    Public Shared F As Integer = 10
End Class

Module Test
    Public Function ReturnC() As C
        Console.WriteLine("Returning a new instance of C.")
        Return New C()
    End Function

    Public Sub Main()
        Console.WriteLine("The value of F is: " & ReturnC().F)
    End Sub
End Module
```

@No__t-0 を出力します。これは、共有メンバー @no__t 3 にアクセスするために `C` のインスタンスを提供するために、関数 `ReturnC` を呼び出す必要がないためです。


### <a name="identical-type-and-member-names"></a>同じ型とメンバー名

型と同じ名前を使用してメンバーの名前を指定することは珍しくありません。 ただし、このような状況では、わかりにくい名前の隠ぺいが発生する可能性があります。

```vb
Enum Color
    Red
    Green
    Yellow
End Enum

Class Test
    ReadOnly Property Color() As Color
        Get
            Return Color.Red
        End Get
    End Property

    Shared Function DefaultColor() As Color
        Return Color.Green    ' Binds to the instance property!
    End Function
End Class
```

前の例では、`DefaultColor` の簡易名 `Color` は、型ではなくインスタンスプロパティにバインドされています。 インスタンスメンバーは共有メンバーでは参照できないため、通常はエラーになります。

ただし、特別な規則によって、この場合は型にアクセスできます。 メンバーアクセス式の基本式が単純な名前で、型が同じである定数、フィールド、プロパティ、ローカル変数、またはパラメーターにバインドする場合、基本式はメンバーまたは型のいずれかを参照できます。 どちらか一方からアクセスできるメンバーが同じであるため、あいまいさが生じることはありません。

### <a name="default-instances"></a>既定のインスタンス

場合によっては、共通の基底クラスから派生したクラスは、通常、インスタンスを1つだけ持ちます。 たとえば、ユーザーインターフェイスに表示されるほとんどのウィンドウでは、常に1つのインスタンスが画面上に表示されます。 これらの種類のクラスを簡単に操作できるように、Visual Basic では、クラスの*既定のインスタンス*を自動的に生成できます。これらのクラスは、各クラスに対して簡単に参照できる単一のインスタンスを提供します。

既定のインスタンスは、特定の1つの型ではなく、型の*ファミリ*に対して常に作成されます。 そのため、フォームから派生したクラス Form1 の既定のインスタンスを作成する代わりに、フォームから派生したすべてのクラスに対して既定のインスタンスが作成されます。 これは、基本クラスから派生した個々のクラスが、既定のインスタンスを持つように特別にマークする必要がないことを意味します。

クラスの既定のインスタンスは、そのクラスの既定のインスタンスを返すコンパイラによって生成されるプロパティによって表されます。 特定の基本クラスから派生したすべてのクラスの既定のインスタンスの割り当てと破棄を管理する*グループクラス*と呼ばれるクラスのメンバーとして生成されたプロパティ。 たとえば、`Form` から派生したクラスの既定のインスタンスプロパティはすべて、`MyForms` クラスで収集できます。 Group クラスのインスタンスが式によって返される場合 `My.Forms`,、次のコードは、派生クラスの既定のインスタンスにアクセスする-1 と `Form2` @no__t します。

```vb
Class Form1
    Inherits Form
    Public x As Integer
End Class

Class Form2
    Inherits Form
    Public y As Integer
End Class

Module Main
    Sub Main()
        My.Forms.Form1.x = 10
        Console.WriteLine(My.Forms.Form2.y)
    End Sub
End Module
```

既定のインスタンスは、そのインスタンスへの最初の参照までは作成されません。既定のインスタンスを表すプロパティを取得すると、既定のインスタンスがまだ作成されていない場合、または `Nothing` に設定されている場合は、既定のインスタンスが作成されます。 既定のインスタンスが存在するかどうかのテストを許可するために、既定のインスタンスが `Is` または `IsNot` 演算子のターゲットである場合、既定のインスタンスは作成されません。 したがって、既定のインスタンスが作成されないように、既定のインスタンスが @no__t 0 またはその他の参照であるかどうかをテストできます。

既定のインスタンスは、既定のインスタンスを持つクラスの外部から既定のインスタンスを簡単に参照できるようにすることを目的としています。 を定義するクラス内から既定のインスタンスを使用すると、参照されているインスタンス (つまり、既定のインスタンスまたは現在のインスタンス) との混同が生じる可能性があります。 たとえば、次のコードでは、別のインスタンスから呼び出されている場合でも、既定のインスタンスの値 `x` のみを変更します。 したがって、コードは `10` ではなく 0 @no__t 値を出力します。

```vb
Class Form1
    Inherits Form

    Public x As Integer = 5

    Public Sub ChangeX()
        Form1.x = 10
    End Sub
End Class

Module Main
    Sub Main()
        Dim f As Form1 = New Form1()
        f.ChangeX()
        Console.WriteLine(f.x)
    End Sub
End Module
```

このような混乱を防ぐために、既定のインスタンスの型のインスタンスメソッド内から既定のインスタンスを参照することは無効です。

#### <a name="default-instances-and-type-names"></a>既定のインスタンスと型名

既定のインスタンスには、その型の名前を使用して直接アクセスすることもできます。 この場合、型名が許可されていない式のコンテキストでは、式 `E` は、`E` は既定のインスタンスを持つクラスの完全修飾名を表し、は `E'` に変更されます。ここで、`E'` は、をフェッチする式を表します。既定のインスタンスプロパティ。 たとえば、`Form` から派生したクラスの既定のインスタンスで、型名を使用して既定のインスタンスにアクセスできる場合、次のコードは前の例のコードに相当します。

```vb
Module Main
    Sub Main()
        Form1.x = 10
        Console.WriteLine(Form2.y)
    End Sub
End Module
```

これはまた、型の名前を使用してアクセスできる既定のインスタンスも、型名を使用して割り当てることができることを意味します。 たとえば、次のコードでは、`Form1` の既定のインスタンスを `Nothing` に設定しています。

```vb
Module Main
    Sub Main()
        Form1 = Nothing
    End Sub
End Module
```

@No__t-0 の意味は、-1 @no__t がクラスを表し、`I` が共有メンバーを変更しないことを表していることに注意してください。 このような式は、引き続きクラスインスタンスから直接共有メンバーにアクセスし、既定のインスタンスを参照しません。

#### <a name="group-classes"></a>グループクラス

@No__t-0 属性は、既定のインスタンスのファミリのグループクラスを示します。 属性には、次の4つのパラメーターがあります。

* パラメーター `TypeToCollect` は、グループの基本クラスを指定します。 (型パラメーターに関係なく) この名前を持つ型から派生するオープン型パラメーターを持たないインスタンス化可能なクラスはすべて、自動的に既定のインスタンスを持ちます。

* パラメーター `CreateInstanceMethodName` は、既定のインスタンスプロパティに新しいインスタンスを作成するために、group クラスで呼び出すメソッドを指定します。

* パラメーター `DisposeInstanceMethodName` は、既定のインスタンスプロパティに値 `Nothing` が割り当てられている場合に、既定のインスタンスプロパティを破棄するために、group クラスで呼び出すメソッドを指定します。

* パラメーター @no__t-@no__t 0 に指定すると、既定のインスタンスに型名を使用して直接アクセスできる場合は、クラス名の代わりに-1 が指定されます。 このパラメーターが @no__t 0 または空の文字列の場合、このグループの種類の既定のインスタンスには、その型の名前を使用して直接アクセスすることはできません。 (__注:__ Visual Basic 言語の現在のすべての実装では、コンパイラが提供するコードを除き、`DefaultInstanceAlias` パラメーターは無視されます)。

最初の3つのパラメーターの型とメソッドの名前をコンマで区切ることで、同じグループに複数の型を集めることができます。 各パラメーターには同じ数の項目が必要です。また、リストの要素は順番に一致します。 たとえば、次の属性の宣言は、`C1`、`C2`、または @no__t から派生した型を1つのグループに収集します。

```vb
<Microsoft.VisualBasic.MyGroupCollection("C1, C2, C3", _
    "CreateC1, CreateC2, CreateC3", _
    "DisposeC1, DisposeC2, DisposeC3", "My.Cs")>
Public NotInheritable Class MyCs
    ...
End Class
```

Create メソッドのシグネチャは、`Shared Function <Name>(Of T As {New, <Type>})(Instance Of T) As T` の形式にする必要があります。 Dispose メソッドは、`Shared Sub <Name>(Of T As <Type>)(ByRef Instance Of T)` の形式にする必要があります。 したがって、前のセクションの例の group クラスは次のように宣言できます。

```vb
<Microsoft.VisualBasic.MyGroupCollection("Form", "Create", _
    "Dispose", "My.Forms")> _
Public NotInheritable Class MyForms
    Private Shared Function Create(Of T As {New, Form}) _
        (Instance As T) As T
        If Instance Is Nothing Then
            Return New T()
        Else
            Return Instance
        End If
    End Function

    Private Shared Sub Dispose(Of T As Form)(ByRef Instance As T)
        Instance.Close()
        Instance = Nothing
    End Sub
End Class
```

派生クラスを宣言したソースファイル `Form1` の場合、生成された group クラスは次のようになります。

```vb
<Microsoft.VisualBasic.MyGroupCollection("Form", "Create", _
    "Dispose", "My.Forms")> _
Public NotInheritable Class MyForms
    Private Shared Function Create(Of T As {New, Form}) _
        (Instance As T) As T
        If Instance Is Nothing Then
            Return New T()
        Else
            Return Instance
        End If
    End Function

    Private Shared Sub Dispose(Of T As Form)(ByRef Instance As T)
        Instance.Close()
        Instance = Nothing
    End Sub

    Private m_Form1 As Form1

    Public Property Form1() As Form1
        Get
            Return Create(m_Form1)
        End Get
        Set (Value As Form1)
            If Value IsNot Nothing AndAlso Value IsNot m_Form1 Then
                Throw New ArgumentException( _
                    "Property can only be set to Nothing.")
            End If
            Dispose(m_Form1)
        End Set
    End Property
End Class
```

### <a name="extension-method-collection"></a>拡張メソッドのコレクション

メンバーアクセス式の拡張メソッド `E.I` は、現在のコンテキストで使用可能な名前 `I` を持つすべての拡張メソッドを収集することによって収集されます。

1. まず、式を含む入れ子になった各型がチェックされ、最も内側から外側に向かっています。
2. 次に、入れ子になった各名前空間が、最も内側から最も外側の名前空間に向かってチェックされます。
3. 次に、ソースファイルのインポートが確認されます。
4. 次に、コンパイル環境で定義されているインポートがチェックされます。

拡張メソッドは、対象の式の型から拡張メソッドの最初のパラメーターの型への拡張ネイティブ変換がある場合にのみ収集されます。 通常の簡易名式のバインドとは異なり、検索では*すべて*の拡張メソッドが収集されます。拡張メソッドが見つかった場合、コレクションは停止しません。 以下に例を示します。

```vb
Imports System.Runtime.CompilerServices

Class C1
End Class


Namespace N1
    Module N1C1Extensions
        <Extension> _
        Sub M1(c As C1, x As Integer)
        End Sub
    End Module
End Namespace

Namespace N1.N2
    Module N2C1Extensions
        <Extension> _
        Sub M1(c As C1, y As Double)
        End Sub
    End Module
End Namespace

Namespace N1.N2.N3
    Module Test
        Sub Main()
            Dim x As New C1()

            ' Calls N1C1Extensions.M1
            x.M1(10)
        End Sub
    End Module
End Namespace
```

この例では、-1 @no__t 前に `N2C1Extensions.M1` が検出された場合でも、両方とも拡張メソッドと見なされます。 すべての拡張メソッドが収集されると、そのメソッドは*カリー*化されます。 カリー化は、拡張メソッド呼び出しのターゲットを取得し、拡張メソッド呼び出しに適用します。これにより、最初のパラメーター (指定されているため) が削除された新しいメソッドシグネチャが生成されます。 以下に例を示します。

```vb
Imports System.Runtime.CompilerServices

Module Ext1
    <Extension> _
    Sub M(x As Integer, y As Integer)
    End Sub
End Module

Module Ext2
    <Extension> _
    Sub M(x As Integer, y As Double)
    End Sub
End Module

Module Main
    Sub Test()
        Dim v As Integer = 10

        ' The curried method signatures considered are:
        '        Ext1.M(y As Integer)
        '        Ext2.M(y As Double)
        v.M(10)
    End Sub
End Module
```

上の例では、`v` を `Ext1.M` に適用したカリー化された結果が、メソッドシグネチャ `Sub M(y As Integer)` です。

カリー化は、拡張メソッドの最初のパラメーターを削除するだけでなく、最初のパラメーターの型の一部であるメソッド型パラメーターもすべて削除します。 カリー化がメソッド型パラメーターを使用して拡張メソッドを実行すると、最初のパラメーターに型の推定が適用され、推論されるすべての型パラメーターに対して結果が固定されます。 型の推定が失敗した場合、メソッドは無視されます。 以下に例を示します。

```vb
Imports System.Runtime.CompilerServices

Module Ext1
    <Extension> _
    Sub M(Of T, U)(x As T, y As U)
    End Sub
End Module

Module Ext2
    <Extension> _
    Sub M(Of T)(x As T, y As T)
    End Sub
End Module

Module Main
    Sub Test()
        Dim v As Integer = 10

        ' The curried method signatures considered are:
        '        Ext1.M(Of U)(y As U)
        '        Ext2.M(y As Integer)
        v.M(10)
    End Sub
End Module
```

上の例では、`v` を `Ext1.M` に適用したカリー化された結果は、メソッドシグネチャ @no__t です。これは、型パラメーター `T` がカリー化の結果として推論され、現在は修正されているためです。 型パラメーター `U` は、カリー化の一部として推論されなかったため、開いているパラメーターのままです。 同様に、型パラメーター `T` は `v` を `Ext2.M` に適用した結果として推論されるため、パラメーター `y` の型は `Integer` として固定されます。 他の型として推論されることはありません。 署名をカリー化すると、`New` 制約を除くすべての制約も適用されます。 制約が満たされない場合、またはカリー化の一部として推論されなかった型に依存している場合、拡張メソッドは無視されます。 以下に例を示します。

```vb
Imports System.Runtime.CompilerServices

Module Ext1
    <Extension> _
    Sub M1(Of T As Structure)(x As T, y As Integer)
    End Sub

    <Extension> _
    Sub M2(Of T As U, U)(x As T, y As U)
    End Sub
End Module

Module Main
    Sub Test()
        Dim s As String = "abc"

        ' Error: String does not satisfy the Structure constraint
        s.M1(10)

        ' Error: T depends on U, which cannot be inferred
        s.M2(10)
    End Sub
End Module
```

__付箋.__ カリー化の拡張メソッドを実行する主な理由の1つは、クエリパターンメソッドの引数を評価する前に、クエリ式で反復処理の型を推論できることです。 ほとんどのクエリパターンメソッドでは、型の推定を必要とするラムダ式が使用されるため、クエリ式を評価するプロセスが非常に簡単になります。

通常のインターフェイスの継承とは異なり、相互に関連しない2つのインターフェイスを拡張する拡張メソッドは、同じカリー化シグネチャを持たない限り使用できます。

```vb
Imports System.Runtime.CompilerServices

Interface I1
End Interface

Interface I2
End Interface

Class C1
    Implements I1, I2
End Class

Module I1Ext
    <Extension> _
    Sub M1(i As I1, x As Integer)
    End Sub

    <Extension> _
    Sub M2(i As I1, x As Integer)
    End Sub
End Module

Module I2Ext
    <Extension> _
    Sub M1(i As I2, x As Integer)
    End Sub

    <Extension> _
    Sub M2(I As I2, x As Double)
    End Sub
End Module

Module Main
    Sub Test()
        Dim c As New C1()

        ' Error: M is ambiguous between I1Ext.M1 and I2Ext.M1.
        c.M1(10)

        ' Calls I1Ext.M2
        c.M2(10)
    End Sub
End Module
```

最後に、遅延バインディングを実行する場合、拡張メソッドは考慮されないことに注意することが重要です。

```vb
Module Test
    Sub Main()
        Dim o As Object = ...

        ' Ignores extension methods
        o.M1()
    End Sub
End Module
```

## <a name="dictionary-member-access-expressions"></a>ディクショナリメンバーアクセス式

*ディクショナリメンバーアクセス式*は、コレクションのメンバーを検索するために使用されます。 ディクショナリメンバーへのアクセスには `E!I` の形式を使用します。ここで `E` は値として分類される式で、`I` は識別子です。

```antlr
DictionaryAccessExpression
    : Expression? '!' IdentifierOrKeyword
    ;
```

式の型には、単一の `String` パラメーターによってインデックス付けされた既定のプロパティが必要です。 ディクショナリメンバーアクセス式 `E!I` は、式 `E.D("I")` に変換されます。 `D` は `E` の既定のプロパティです。 以下に例を示します。

```vb
Class Keys
    Public ReadOnly Default Property Item(s As String) As Integer
        Get
            Return 10
        End Get
    End Property 
End Class

Module Test
    Sub Main()
        Dim x As Keys = new Keys()
        Dim y As Integer
        ' The two statements are equivalent.
        y = x!abc
        y = x("abc")
    End Sub
End Module
```

感嘆符が式なしで指定されている場合は、直ちに `With` ステートメントを含む式が使用されます。 @No__t-0 ステートメントが含まれていない場合は、コンパイル時エラーが発生します。


## <a name="invocation-expressions"></a>Invocation 式

呼び出し式は、呼び出し先と省略可能な引数リストで構成されます。

```antlr
InvocationExpression
    : Expression ( OpenParenthesis ArgumentList? CloseParenthesis )?
    ;

ArgumentList
    : PositionalArgumentList
    | PositionalArgumentList Comma NamedArgumentList
    | NamedArgumentList
    ;

PositionalArgumentList
    : Expression? ( Comma Expression? )*
    ;

NamedArgumentList
    : IdentifierOrKeyword ColonEquals Expression
      ( Comma IdentifierOrKeyword ColonEquals Expression )*
    ;
```

ターゲット式は、メソッドグループ、または型がデリゲート型である値として分類される必要があります。 対象の式が型がデリゲート型である値の場合、呼び出し式のターゲットはデリゲート型の `Invoke` メンバーのメソッドグループになり、対象の式がメソッドグループの関連付けられたターゲット式になります。

引数リストには、位置引数と名前付き引数の2つのセクションがあります。 *位置指定引数*は式であり、名前付き引数の前に指定する必要があります。 *名前付き引数*は、キーワードに一致できる識別子で始まり、その後に `:=` と式が続きます。

メソッドグループにインスタンスと拡張メソッドの両方を含むアクセス可能なメソッドが1つだけ含まれており、そのメソッドが引数を取らず、関数である場合、メソッドグループは、空の引数リストを持つ呼び出し式として解釈され、結果はになります。指定された引数リストを持つ呼び出し式のターゲットとして使用されます。 以下に例を示します。

```vb
Class C1
    Function M1() As Integer()
        Return New Integer() { 1, 2, 3 }
    End Sub
End Class

Module Test
    Sub Main()
        Dim c As New C1()

        ' Prints 3
        Console.WriteLine(c.M1(2))
    End Sub
End Module
```

それ以外の場合は、オーバーロードの解決がメソッドに適用され、指定された引数リストに対して最も適切なメソッドが選択されます。 最も適切なメソッドが関数の場合、呼び出し式の結果は、関数の戻り値の型として型指定された値として分類されます。 最も適切なメソッドがサブルーチンの場合、結果は void として分類されます。 最も適切なメソッドが、本体のない部分メソッドである場合、呼び出し式は無視され、結果は void として分類されます。

事前バインディングされた呼び出し式の場合、引数は、対応するパラメーターがターゲットメソッドで宣言されている順序で評価されます。 遅延バインディングされたメンバーアクセス式では、メンバーアクセス式に出現する順序で評価されます。「遅延バインディング[式](expressions.md#late-bound-expressions)」を参照してください。

## <a name="overloaded-method-resolution"></a>オーバーロードされたメソッドの解決:
オーバーロードの解決については、引数リストを指定したメンバー/型の特異性、Genericity、引数リストへの適用性、引数の引き渡し、省略可能なパラメーターの引数の指定、条件付きメソッド、および型引数の推定については、「」を参照してください[。オーバーロードの解決](overload-resolution.md)。

## <a name="index-expressions"></a>インデックス式

*インデックス式*は、配列要素を生成するか、プロパティグループをプロパティアクセスに再分類します。 インデックス式は、式、左かっこ、インデックス引数リスト、および閉じかっこで構成されます。

```antlr
IndexExpression
    : Expression OpenParenthesis ArgumentList? CloseParenthesis
    ;
```

インデックス式のターゲットは、プロパティグループまたは値のいずれかとして分類される必要があります。 インデックス式は、次のように処理されます。

* ターゲット式が値として分類され、その型が配列型でない場合、`Object` または `System.Array` の場合、型には既定のプロパティが必要です。 インデックスは、型のすべての既定のプロパティを表すプロパティグループで実行されます。 Visual Basic でパラメーターなしの既定のプロパティを宣言することは無効ですが、他の言語ではこのようなプロパティを宣言できます。 そのため、引数を指定せずにプロパティのインデックスを作成することはできます。

* 式の結果が配列型の値である場合、引数リスト内の引数の数は配列型のランクと同じである必要があり、名前付き引数を含めることはできません。 実行時に無効なインデックスがある場合は、@no__t 0 の例外がスローされます。 各式は、型 `Integer` に暗黙的に変換できる必要があります。 インデックス式の結果は、指定されたインデックス位置にある変数で、変数として分類されます。

* 式がプロパティグループとして分類されている場合、オーバーロードの解決は、プロパティのいずれかがインデックス引数リストに適用可能かどうかを判断するために使用されます。 プロパティグループに @no__t 0 のアクセサーを持つ1つのプロパティのみが含まれていて、そのアクセサーが引数をとらない場合、プロパティグループは空の引数リストを持つインデックス式として解釈されます。 結果は、現在のインデックス式のターゲットとして使用されます。 適用可能なプロパティがない場合は、コンパイル時エラーが発生します。 それ以外の場合、式は、プロパティグループの関連付けられた対象の式 (存在する場合) を使用して、プロパティにアクセスします。

* 式が遅延バインディングされたプロパティグループ、または型が `Object` または `System.Array` である値として分類される場合、インデックス式の処理は実行時まで遅延され、インデックス作成は遅延バインディングされます。 式は、`Object` として型指定された遅延バインディングプロパティのアクセスを実行します。 関連するターゲット式は、対象の式 (値の場合)、またはプロパティグループの関連付けられた対象式のいずれかです。 実行時に、式は次のように処理されます。

* 式が遅延バインディングされたプロパティグループとして分類されている場合、式は、メソッドグループ、プロパティグループ、または値 (メンバーがインスタンスまたは共有変数の場合) になることがあります。 結果がメソッドグループまたはプロパティグループの場合は、オーバーロードの解決がグループに適用され、引数リストの正しいメソッドが決定されます。 オーバーロードの解決に失敗した場合は、@no__t 0 の例外がスローされます。 その後、結果はプロパティアクセスまたは呼び出しとして処理され、結果が返されます。 呼び出しがサブルーチンの場合、結果は `Nothing` になります。

* 対象の式の実行時の型が配列型または `System.Array` の場合、インデックス式の結果は、指定されたインデックス位置にある変数の値になります。

* それ以外の場合、式の実行時の型には既定のプロパティが必要です。また、インデックスは、型のすべての既定のプロパティを表すプロパティグループで実行されます。 型に既定のプロパティがない場合は、`System.MissingMemberException` 例外がスローされます。


## <a name="new-expressions"></a>新しい式

@No__t-0 演算子は、型の新しいインスタンスを作成するために使用されます。 @No__t 0 の式には、次の4つの形式があります。

* オブジェクト作成式は、クラス型と値型の新しいインスタンスを作成するために使用されます。

* 配列作成式は、配列型の新しいインスタンスを作成するために使用されます。

* デリゲート作成式 (オブジェクト作成式とは異なる構文を持たない) は、デリゲート型の新しいインスタンスを作成するために使用されます。

* 匿名のオブジェクト作成式は、匿名クラス型の新しいインスタンスを作成するために使用されます。

```antlr
NewExpression
    : ObjectCreationExpression
    | ArrayExpression
    | AnonymousObjectCreationExpression
    ;
```

@No__t-0 式は値として分類され、結果は型の新しいインスタンスになります。


### <a name="object-creation-expressions"></a>オブジェクト作成式

オブジェクト作成式は、クラス型または構造体型の新しいインスタンスを作成するために使用されます。

```antlr
ObjectCreationExpression
    : 'New' NonArrayTypeName ( OpenParenthesis ArgumentList? CloseParenthesis )?
      ObjectCreationExpressionInitializer?
    ;

ObjectCreationExpressionInitializer
    : ObjectMemberInitializer
    | ObjectCollectionInitializer
    ;

ObjectMemberInitializer
    : 'With' OpenCurlyBrace FieldInitializerList CloseCurlyBrace
    ;

FieldInitializerList
    : FieldInitializer ( Comma FieldInitializer )*
    ;

FieldInitializer
    : 'Key'? ('.' IdentifierOrKeyword Equals )? Expression
    ;

ObjectCollectionInitializer
    : 'From' CollectionInitializer
    ;

CollectionInitializer
    : OpenCurlyBrace CollectionElementList? CloseCurlyBrace
    ;

CollectionElementList
    : CollectionElement ( Comma CollectionElement )*
    ;

CollectionElement
    : Expression
    | CollectionInitializer
    ;
```

オブジェクト作成式の型は、クラス型、構造体型、または @no__t 0 制約を持つ型パラメーターである必要があり、`MustInherit` クラスにすることはできません。 @No__t-0 という形式のオブジェクト作成式が指定されている場合、`T` はクラス型または構造体型であり、`A` は省略可能な引数リストです。オーバーロードの解決では、呼び出し対象の `T` の正しいコンストラクターを決定します。 @No__t 0 の制約のある型パラメーターは、1つのパラメーターなしのコンストラクターを持つと見なされます。 呼び出し可能なコンストラクターがない場合、コンパイル時エラーが発生します。それ以外の場合、式は、選択したコンストラクターを使用して `T` の新しいインスタンスを作成します。 引数がない場合は、かっこを省略できます。

インスタンスが割り当てられる場所は、インスタンスがクラス型であるか値型であるかによって異なります。 `New` クラス型のインスタンスはシステムヒープ上に作成されますが、値型の新しいインスタンスはスタックに直接作成されます。

オブジェクト作成式では、必要に応じて、コンストラクター引数の後にメンバー初期化子のリストを指定できます。 これらのメンバー初期化子には、キーワード `With` がプレフィックスとして付けられます。また、初期化子リストは、`With` ステートメントのコンテキスト内にあるかのように解釈されます。 たとえば、次のようなクラスがあるとします。

```vb
Class Customer
    Dim Name As String
    Dim Address As String
End Class
```

コード:

```vb
Module Test
    Sub Main()
        Dim x As New Customer() With { .Name = "Bob Smith", _
            .Address = "123 Main St." }
    End Sub
End Module
```

は、次の場合とほぼ同じです。

```vb
Module Test
    Sub Main()
        Dim x, _t1 As Customer

        _t1 = New Customer()
        With _t1
            .Name = "Bob Smith"
            .Address = "123 Main St."
        End With

        x = _t1
    End Sub
End Module
```

各初期化子は割り当てる名前を指定する必要があり、名前は、@no__t 0 以外のインスタンス変数、または構築される型のプロパティである必要があります。構築される型が-1 @no__t 場合、メンバーアクセスは遅延バインディングされません。 初期化子は `Key` キーワードを使用できません。 型の各メンバーは、1回だけ初期化できます。 ただし、初期化子式は相互に参照できます。 以下に例を示します。

```vb
Module Test
    Sub Main()
        Dim x As New Customer() With { .Name = "Bob Smith", _
            .Address = .Name & " St." }
    End Sub
End Module
```

初期化子は左から右に割り当てられます。したがって、初期化子がまだ初期化されていないメンバーを参照している場合は、コンストラクターの実行後にインスタンス変数の値がすべて表示されます。

```vb
Module Test
    Sub Main()
        ' The value of Address will be " St." since Name has not been
        ' assigned yet.
        Dim x As New Customer() With { .Address = .Name & " St." }
    End Sub
End Module
```

初期化子は入れ子にすることができます。

```vb
Class Customer
    Dim Name As String
    Dim Address As Address
    Dim Age As Integer
End Class

Class Address
    Dim Street As String
    Dim City As String
    Dim State As String
    Dim ZIP As String
End Class

Module Test
    Sub Main()
        Dim c As New Customer() With { _
            .Name = "John Smith", _
            .Address = New Address() With { _
                .Street = "23 Main St.", _
                .City = "Peoria", _
                .State = "IL", _
                .ZIP = "13934" }, _
            .Age = 34 }
    End Sub
End Module
```

作成される型がコレクション型であり、`Add` という名前のインスタンスメソッド (拡張メソッドと共有メソッドを含む) を持っている場合、オブジェクト作成式では、キーワード `From` で始まるコレクション初期化子を指定できます。 オブジェクト作成式では、メンバー初期化子とコレクション初期化子の両方を指定することはできません。 コレクション初期化子内の各要素は、`Add` 関数の呼び出しに引数として渡されます。 以下に例を示します。

```vb
Dim list = New List(Of Integer)() From { 1, 2, 3, 4 }
```

上の式は、下の式と同等です。

```vb
Dim list = New List(Of Integer)()
list.Add(1)
list.Add(2)
list.Add(3)
```

要素がコレクション初期化子の場合、サブコレクション初期化子の各要素は、個別の引数として @no__t 0 関数に渡されます。 たとえば、次のようになります。

```vb
Dim dict = Dictionary(Of Integer, String) From { { 1, "One" },{ 2, "Two" } }
```

上の式は、下の式と同等です。

```vb
Dim dict = New Dictionary(Of Integer, String)
dict.Add(1, "One")
dict.Add(2, "Two")
```

この拡張は常に実行され、1つのレベルの深さに対してのみ実行されます。その後、サブ初期化子は配列リテラルと見なされます。 以下に例を示します。

```vb
' Error: List(Of T) does not have an Add method that takes two parameters.
Dim list = New List(Of Integer())() From { { 1, 2 }, { 3, 4 } }

' OK, this initializes the dictionary with (Integer, Integer()) pairs.
Dim dict = New Dictionary(Of Integer, Integer())() From _
        { {  1, { 2, 3 } }, { 3, { 4, 5 } } }
```


### <a name="array-expressions"></a>配列式

配列式は、配列型の新しいインスタンスを作成するために使用されます。 配列式には、配列作成式と配列リテラルの2種類があります。

#### <a name="array-creation-expressions"></a>配列作成式

配列サイズ初期化修飾子が指定されている場合、結果の配列型は、配列サイズ初期化引数リストから個々の引数をそれぞれ削除することによって派生されます。 各引数の値によって、新しく割り当てられた配列インスタンス内の対応する次元の上限が決まります。 式に空でないコレクション初期化子がある場合、引数リストの各引数は定数である必要があります。また、式リストで指定された順位と次元の長さは、コレクション初期化子のものと一致する必要があります。

```vb
Dim a() As Integer = New Integer(2) {}
Dim b() As Integer = New Integer(2) { 1, 2, 3 }
Dim c(,) As Integer = New Integer(1, 2) { { 1, 2, 3 } , { 4, 5, 6 } }

' Error, length/initializer mismatch.
Dim d() As Integer = New Integer(2) { 0, 1, 2, 3 }
```

配列サイズ初期化修飾子が指定されていない場合、型名は配列型である必要があり、コレクション初期化子は空であるか、または指定された配列型のランクと同じ数の入れ子レベルを持つ必要があります。 最も内側の入れ子レベルにあるすべての要素は、配列の要素型に暗黙的に変換できる必要があり、値として分類される必要があります。 入れ子になった各コレクション初期化子の要素の数は、同じレベルの他のコレクションのサイズと常に一致している必要があります。 個々の次元の長さは、コレクション初期化子のそれぞれの入れ子レベルに含まれる要素の数から推論されます。 コレクション初期化子が空の場合、各次元の長さは0になります。

```vb
Dim e() As Integer = New Integer() { 1, 2, 3 }
Dim f(,) As Integer = New Integer(,) { { 1, 2, 3 } , { 4, 5, 6 } }

' Error: Inconsistent numbers of elements!
Dim g(,) As Integer = New Integer(,) { { 1, 2 }, { 4, 5, 6 } }

' Error: Inconsistent levels of nesting!
Dim h(,) As Integer = New Integer(,) { 1, 2, { 3, 4 } }
```

コレクション初期化子の最も外側の入れ子レベルは、配列の左端の次元に対応し、最も内側の入れ子レベルが右端の次元に対応します。 次に例を示します。

```vb
Dim array As Integer(,) = _
    { { 0, 1 }, { 2, 3 }, { 4, 5 }, { 6, 7 }, { 8, 9 } }
```

は、次の場合と同じです。

```vb
Dim array(4, 1) As Integer

array(0, 0) = 0: array(0, 1) = 1
array(1, 0) = 2: array(1, 1) = 3
array(2, 0) = 4: array(2, 1) = 5
array(3, 0) = 6: array(3, 1) = 7
array(4, 0) = 8: array(4, 1) = 9
```

コレクション初期化子が空の場合 (つまり、中かっこが含まれているが初期化子リストがない場合)、初期化される配列の次元の境界がわかっていれば、空のコレクション初期化子は、指定されたサイズの配列インスタンスを表します。すべての要素が要素型の既定値に初期化されています。 初期化されている配列の次元の境界が不明な場合、空のコレクション初期化子は、すべての次元のサイズがゼロである配列インスタンスを表します。

各次元の配列インスタンスのランクと長さは、インスタンスの有効期間全体にわたって一定です。 つまり、既存の配列インスタンスのランクを変更することはできません。また、次元のサイズを変更することもできません。

#### <a name="array-literals"></a>配列リテラル

配列リテラルは、式コンテキストとコレクション初期化子の組み合わせから推論される要素型、ランク、および境界を持つ配列を表します。 これについては、「式の再[分類](expressions.md#expression-reclassification)」を参照してください。

```antlr
ArrayExpression
    : ArrayCreationExpression
    | ArrayLiteralExpression
    ;

ArrayCreationExpression
    : 'New' NonArrayTypeName ArrayNameModifier CollectionInitializer
    ;

ArrayLiteralExpression
    : CollectionInitializer
    ;
```

以下に例を示します。

```vb
' array of integers
Dim a = {1, 2, 3}

' array of shorts
Dim b = {1S, 2S, 3S}

' array of shorts whose type is taken from the context
Dim c As Short() = {1, 2, 3}

' array of type Integer(,)
Dim d = {{1, 0}, {0, 1}}

' jagged array of rank ()()
Dim e = {({1, 0}), ({0, 1})}

' error: inconsistent rank
Dim f = {{1}, {2, 3}}

' error: inconsistent rank
Dim g = {1, {2}}
```

配列リテラルのコレクション初期化子の形式と要件は、配列作成式のコレクション初期化子とまったく同じです。

__付箋.__ 配列リテラルでは、配列はで作成されません。代わりに、式を値に再分類して、配列が作成されるようにします。 たとえば、`Integer()` から `Short()` への変換が行われていないため、`CType(new Integer() {1,2,3}, Short())` は変換できません。ただし、@no__t 式は、配列のリテラルを配列作成式 `New Short() {1,2,3}` に最初に分類するため、使用できます。


### <a name="delegate-creation-expressions"></a>デリゲート作成式

デリゲート作成式は、デリゲート型の新しいインスタンスを作成するために使用されます。 デリゲート作成式の引数は、メソッドポインターまたはラムダメソッドとして分類される式である必要があります。

引数がメソッドポインターの場合は、メソッドポインターによって参照されるメソッドの1つが、デリゲート型のシグネチャに適用可能である必要があります。 次の場合、メソッド `M` はデリゲート型 @no__t に適用できます。

* `M` が-1 でないか、または本文を含んで @no__t います。

* @No__t-0 と `D` の両方が関数であるか、または `D` がサブルーチンです。

* `M` および `D` のパラメーターの数が同じです。

* @No__t 0 のパラメーターの型はそれぞれ、`D` の対応するパラメーターの型からの変換と、その修飾子 (`ByRef`、`ByVal`) が一致します。

* 戻り値の型 `M` (存在する場合) は、戻り値の型 `D` に変換されます。

メソッドポインターが遅延バインディングアクセスを参照する場合、遅延バインディングアクセスは、デリゲート型と同じ数のパラメーターを持つ関数と見なされます。

厳密なセマンティクスが使用されておらず、メソッドポインターによって参照されているメソッドが1つだけである場合、パラメーターを持たず、デリゲート型であることが原因で適用できない場合は、メソッドが適用可能であると見なされ、パラメーターまたは戻り値 a が返されます。単に無視されます。 以下に例を示します。

```vb
Delegate Sub F(x As Integer)

Module Test
    Sub M()
    End Sub

    Sub Main()
        ' Valid
        Dim x As F = AddressOf M
    End Sub
End Module
```

__付箋.__ この緩和は、拡張メソッドのために厳密なセマンティクスが使用されていない場合にのみ許可されます。 拡張メソッドは、通常のメソッドが適用されていない場合にのみ考慮されるため、デリゲートの構築のためにパラメーターを使用して拡張メソッドを非表示にするパラメーターを指定せずにインスタンスメソッドを使用することができます。

メソッドポインターによって参照される複数のメソッドがデリゲート型に適用可能な場合は、オーバーロードの解決を使用して候補のメソッドの中から選択します。 デリゲートに対するパラメーターの型は、オーバーロードの解決のために引数の型として使用されます。 1つのメソッド候補が最も適していない場合は、コンパイル時エラーが発生します。 次の例では、ローカル変数は、2番目の `Square` メソッドを参照するデリゲートを使用して初期化されています。これは、そのメソッドが @no__t のシグネチャと戻り値の型に適用可能であるためです。

```vb
Delegate Function DoubleFunc(x As Double) As Double

Module Test
    Function Square(x As Single) As Single
        Return x * x
    End Function 

    Function Square(x As Double) As Double
        Return x * x
    End Function

    Sub Main()
        Dim a As New DoubleFunc(AddressOf Square)
    End Sub
End Module
```

2番目の `Square` メソッドが存在しませんでした。最初の `Square` メソッドが選択されています。 厳密なセマンティクスがコンパイル環境で指定されている場合、または `Option Strict` の場合、メソッドポインターによって参照される最も具体的なメソッドがデリゲートシグネチャよりも狭い場合、コンパイル時エラーが発生します。 次の場合、メソッド `M` はデリゲート型よりも狭くなります `D` です。

* @No__t-0 のパラメーターの型には、対応するパラメーターの型 `D` への拡大変換が含まれています。

* または、戻り値の型 (存在する場合) がある場合は、戻り値の型 `D` の縮小変換が @no__t ます。

型引数がメソッドポインターに関連付けられている場合、同じ数の型引数を持つメソッドだけが考慮されます。 型引数がメソッドポインターに関連付けられていない場合は、ジェネリックメソッドに対して署名を照合するときに、型の推定が使用されます。 他の通常の型の推論とは異なり、デリゲートの戻り値の型は型引数を推論するときに使用されますが、最も一般的でないオーバーロードを決定するときに戻り値の型は考慮されません。 次の例では、デリゲート作成式に型引数を指定する両方の方法を示しています。

```vb
Delegate Function D(s As String, i As Integer) As Integer
Delegate Function E() As Integer

Module Test
    Public Function F(Of T)(s As String, t1 As T) As T
    End Function

    Public Function G(Of T)() As T
    End Function

    Sub Main()
        Dim d1 As D = AddressOf f(Of Integer)    ' OK, type arg explicit
        Dim d2 As D = AddressOf f                ' OK, type arg inferred

        Dim e1 As E = AddressOf g(Of Integer)    ' OK, type arg explicit
        Dim e2 As E = AddressOf g                ' OK, infer from return
  End Sub
End Module
```

上記の例では、ジェネリックメソッドを使用して非ジェネリックデリゲート型がインスタンス化されています。 ジェネリックメソッドを使用して、構築されたデリゲート型のインスタンスを作成することもできます。 以下に例を示します。

```vb
Delegate Function Predicate(Of U)(u1 As U, u2 As U) As Boolean

Module Test
    Function Compare(Of T)(t1 As List(of T), t2 As List(of T)) As Boolean
        ...
    End Function

    Sub Main()
        Dim p As Predicate(Of List(Of Integer))
        p = AddressOf Compare(Of Integer)
    End Sub
End Module
```

デリゲート作成式の引数がラムダメソッドの場合は、ラムダメソッドがデリゲート型のシグネチャに適用可能である必要があります。 次の場合は、ラムダメソッド `L` がデリゲート型に適用されます-1 @no__t。

* @No__t-0 にパラメーターがある場合、`D` のパラメーターの数は同じになります。 (@No__t-0 にパラメーターがない場合、`D` のパラメーターは無視されます)。

* @No__t 0 のパラメーターの型はそれぞれ、`D` の対応するパラメーターの型に変換され、その修飾子 (`ByRef`、`ByVal`) が一致します。

* @No__t-0 が関数の場合、@no__t の戻り値の型は `D` の戻り値の型に変換されます。 (@No__t-0 がサブルーチンの場合、`L` の戻り値は無視されます)。

@No__t-0 のパラメーターの型が省略されている場合は、`D` の対応するパラメーターの型が推論されます。`L` のパラメーターに配列または null 許容の名前修飾子がある場合、コンパイル時エラーが発生します。 @No__t-0 のすべてのパラメーターの型を使用できるようになると、ラムダメソッド内の式の型が推論されます。 以下に例を示します。

```vb
Delegate Function F(x As Integer, y As Long) As Long

Module Test
    Sub Main()
        ' b inferred to Integer, c and return type inferred to Long
        Dim a As F = Function(b, c) b + c

        ' e and return type inferred to Integer, f inferred to Long
        Dim d As F = Function(e, f) e + CInt(f)
    End Sub
End Module
```

デリゲートシグネチャがラムダメソッドまたはメソッドシグネチャと完全に一致しない場合、.NET Framework はデリゲートの作成をネイティブでサポートしていないことがあります。 そのような場合は、ラムダメソッド式を使用して2つのメソッドを照合します。 以下に例を示します。

```vb
Delegate Function IntFunc(x As Integer) As Integer

Module Test
    Function SquareString(x As String) As String
        Return CInt(x) * CInt(x)
    End Function 

    Sub Main()
        ' The following two lines are equivalent
        Dim a As New IntFunc(AddressOf SquareString)
        Dim b As New IntFunc( _
            Function(x As Integer) CInt(SquareString(CStr(x))))
    End Sub
End Module
```

デリゲート作成式の結果は、メソッドポインター式から、関連付けられている対象式 (存在する場合) と一致するメソッドを参照するデリゲートインスタンスです。 ターゲット式が値型として型指定されている場合、デリゲートはヒープ上のオブジェクトのメソッドを指すことしかできないため、値型はシステムヒープにコピーされます。 デリゲートが参照するメソッドとオブジェクトは、デリゲートの有効期間全体にわたって定数を保持します。 つまり、デリゲートを作成した後に、そのデリゲートのターゲットまたはオブジェクトを変更することはできません。

### <a name="anonymous-object-creation-expressions"></a>匿名オブジェクト作成式

メンバー初期化子を持つオブジェクト作成式では、型名を完全に省略することもできます。

```antlr
AnonymousObjectCreationExpression
    : 'New' ObjectMemberInitializer
    ;
```

その場合、匿名型は、式の一部として初期化されたメンバーの型と名前に基づいて作成されます。 以下に例を示します。

```vb
Module Test
    Sub Main()
        Dim Customer = New With { .Name = "John Smith", .Age = 34 }

        Console.WriteLine(Customer.Name)
    End Sub
End Module
```

匿名オブジェクト作成式によって作成される型は、名前のないクラスであり、@no__t から直接継承されます。また、メンバーの初期化子リスト内のに割り当てられているメンバーと同じ名前のプロパティのセットがあります。 各プロパティの型は、ローカル変数の型の推論と同じ規則を使用して推論されます。 生成された匿名型も `ToString` をオーバーライドし、すべてのメンバーとその値の文字列形式を返します。 (この文字列の正確な形式は、この仕様の範囲を超えています)。

既定では、匿名型によって生成されるプロパティは、読み取り/書き込み可能です。 @No__t-0 修飾子を使用して、匿名型プロパティを読み取り専用としてマークすることができます。 @No__t-0 修飾子は、匿名型が表す値を一意に識別するためにフィールドを使用できることを指定します。 プロパティを読み取り専用にするだけでなく、匿名型によって `Equals` と @no__t がオーバーライドされ、インターフェイス `System.IEquatable(Of T)` が実装されます (`T` の匿名型を入力します)。 メンバーは次のように定義されます。

`Function Equals(obj As Object) As Boolean` および `Function Equals(val As T) As Boolean` は、2つのインスタンスが同じ型であることを検証し、`Object.Equals` を使用して各 `Key` メンバーを比較することによって実装されます。 すべての `Key` メンバーが等しい場合、`Equals` は `True` を返します。それ @no__t 以外の場合は、-@no__t 3 を返します。

`Function GetHashCode() As Integer` が実装されている場合、匿名型の2つのインスタンスに対して `Equals` が true の場合、`GetHashCode` は同じ値を返します。 ハッシュはシード値で始まり、`Key` の各メンバーについて、ハッシュを31で乗算し、メンバーが参照型ではない場合は `Key` メンバーのハッシュ値 (`GetHashCode`) を加算し、`Nothing` の値を持つ null 許容値型ではなくなります。

たとえば、ステートメントで作成された型は次のようになります。

```vb
Dim zipState = New With { Key .ZipCode = 98112, .State = "WA" }
```

次のようなクラスを作成します (厳密な実装は異なる場合もあります)。

```vb
Friend NotInheritable Class $Anonymous1
    Implements IEquatable(Of $Anonymous1)

    Private ReadOnly _zipCode As Integer
    Private _state As String

    Public Sub New(zipCode As Integer, state As String)
        _zipCode = zipcode
        _state = state
    End Sub

    Public ReadOnly Property ZipCode As Integer
        Get
            Return _zipCode
        End Get
    End Property

    Public Property State As String
        Get
            Return _state
        End Get
        Set (value As Integer)
            _state = value
        End Set
    End Property

    Public Overrides Function Equals(obj As Object) As Boolean
        Dim val As $Anonymous1 = TryCast(obj, $Anonymous1)
        Return Equals(val)
    End Function

    Public Overloads Function Equals(val As $Anonymous1) As Boolean _
        Implements IEquatable(Of $Anonymous1).Equals

        If val Is Nothing Then 
            Return False
        End If

        If Not Object.Equals(_zipCode, val._zipCode) Then 
            Return False
        End If

        Return True
    End Function

    Public Overrides Function GetHashCode() As Integer
        Dim hash As Integer = 0

        hash = hash Xor _zipCode.GetHashCode()

        Return hash
    End Function

    Public Overrides Function ToString() As String
        Return "{ Key .ZipCode = " & _zipCode & ", .State = " & _state & " }"
    End Function
End Class
```

匿名型が別の型のフィールドから作成される状況を簡単にするために、次の場合に式からフィールド名を直接推論できます。

* 単純な名前の式 `x` は、名前 `x` を推測します。

* メンバーアクセス式 `x.y` は、名前 `y` を推測します。

* ディクショナリ参照式 `x!y` は、名前 `y` を推測します。

* 引数のない呼び出しまたはインデックス式 `x()` は名前 `x` を推測します。

* XML メンバーアクセス式 `x.<y>`、`x...<y>`、`x.@y` は、名前 `y` を推測します。

* メンバーアクセス式のターゲットである XML メンバーアクセス式 `x.<y>.z` は、名前 `z` を推測します。

* 引数のない呼び出しまたはインデックス式のターゲットである XML メンバーアクセス式 `x.<y>.z()` は名前 `z` を推測します。

* 呼び出しまたはインデックス式のターゲットである XML メンバーアクセス式 `x.<y>(0)` は、名前 `y` を推測します。

初期化子は、推定名への式の代入として解釈されます。 たとえば、次の初期化子は同等です。

```vb
Class Address
    Public Street As String
    Public City As String
    Public State As String
    Public ZIP As String
End Class

Class C1
    Sub Test(a As Address)
        Dim cityState1 = New With { .City = a.City, .State = a.State }
        Dim cityState2 = New With { a.City, a.State }
    End Sub
End Class
```

@No__t-0 など、型の既存のメンバーと競合するメンバー名が推論されると、コンパイル時エラーが発生します。 通常のメンバー初期化子とは異なり、匿名のオブジェクト作成式では、メンバー初期化子が循環参照を持つことを許可したり、メンバーを初期化する前に参照したりすることはできません。 以下に例を示します。

```vb
Module Test
    Sub Main()
        ' Error: Circular references
        Dim x = New With { .a = .b, .b = .a }

        ' Error: Referring to .b before it has been assigned to
        Dim y = New With { .a = .b, .b = 10 }

        ' Error: Referring to .a before it has been assigned to
        Dim z = New With { .a = .a }
    End Sub
End Module
```

2つの匿名クラス作成式が同じメソッド内で発生し、結果として得られる同じ構造を生成する場合、プロパティの順序、プロパティ名、およびプロパティの型がすべて一致すると、両方とも同じ匿名クラスを参照します。 初期化子を使用したインスタンスまたは共有メンバー変数のメソッドスコープは、変数が初期化されるコンストラクターです。

__付箋.__ コンパイラは、アセンブリレベルなどで、匿名型をさらに統合することができますが、現時点ではこのような型に依存することはできません。


## <a name="cast-expressions"></a>キャスト式

キャスト式は、式を指定された型に強制的に変換します。 特定の cast キーワードは、プリミティブ型に式を強制的に変換します。 3つの一般的なキャストキーワード (@no__t 0、`TryCast`、`DirectCast`) は、式を型に強制します。

```antlr
CastExpression
    : 'DirectCast' OpenParenthesis Expression Comma TypeName CloseParenthesis
    | 'TryCast' OpenParenthesis Expression Comma TypeName CloseParenthesis
    | 'CType' OpenParenthesis Expression Comma TypeName CloseParenthesis
    | CastTarget OpenParenthesis Expression CloseParenthesis
    ;

CastTarget
    : 'CBool' | 'CByte' | 'CChar'  | 'CDate'  | 'CDec' | 'CDbl' | 'CInt'
    | 'CLng'  | 'CObj'  | 'CSByte' | 'CShort' | 'CSng' | 'CStr' | 'CUInt'
    | 'CULng' | 'CUShort'
    ;
```

`DirectCast` および `TryCast` には特殊な動作があります。 このため、ネイティブ変換のみがサポートされます。 また、`TryCast` 式の対象の型を値型にすることはできません。 @No__t-0 または `TryCast` が使用されている場合、ユーザー定義の変換演算子は考慮されません。 (__注:__ -0 および `TryCast` のサポート @no__t する変換セットは、"ネイティブ CLR" 変換を実装するため、制限されます。 @No__t-0 の目的は、"ボックス化解除" 命令の機能を提供することです。一方、`TryCast` の目的は、"isinst" 命令の機能を提供することです。 Clr 命令にマップされるため、CLR によって直接サポートされていない変換をサポートすると、意図した目的が損なわれます。

`DirectCast` @no__t として型指定された式を `CType` とは異なる方法で変換します。 @No__t-0 型の式を変換するときに、実行時の型がプリミティブ値型である場合、`DirectCast` は、指定された型が式の実行時の型と異なる場合は `System.InvalidCastException` の例外をスローし、式が @no__t に評価される場合は `System.NullReferenceException` をスローします。 (__注:__ 前述のように、式の型が-1 @no__t 場合、`DirectCast` は CLR 命令に直接マップされます。 これに対し、`CType` は、プリミティブ型間の変換がサポートされるように、ランタイムヘルパーを呼び出して変換を実行します。 @No__t 0 式がプリミティブ値型に変換され、実際のインスタンスの型がターゲット型と一致する場合、`DirectCast` は `CType` より大幅に高速になります)。

`TryCast` は式を変換しますが、式を対象の型に変換できない場合は例外をスローしません。 代わりに、`TryCast` を指定すると、実行時に式を変換できない場合は `Nothing` になります。 (__注:__ 前述のように、`TryCast` は CLR 命令 "isinst" に直接マップされます。 型チェックと変換を組み合わせて1つの操作を行うことにより、`TryCast` は、`TypeOf ... Is` と `CType` を実行するよりも低コストになります)。

以下に例を示します。

```vb
Interface ITest
    Sub Test()
End Interface

Module Test
    Sub Convert(o As Object)
        Dim i As ITest = TryCast(o, ITest)

        If i IsNot Nothing Then
            i.Test()
        End If
    End Sub
End Module
```

式の型から指定された型への変換が存在しない場合、コンパイル時エラーが発生します。 それ以外の場合、式は値として分類され、結果は変換によって生成される値になります。


## <a name="operator-expressions"></a>演算子式

演算子には、次の2種類があります。 *単項演算子*は1つのオペランドを受け取り、プレフィックス表記を使用します (たとえば、`-x`)。 *二項演算子*は2つのオペランドを受け取り、挿入辞表記を使用します (たとえば、`x + y`)。 常に @no__t 0 になる関係演算子を除き、特定の型に対して定義された演算子はその型になります。 演算子のオペランドは、常に値として分類される必要があります。演算子式の結果は、値として分類されます。

```antlr
OperatorExpression
    : ArithmeticOperatorExpression
    | RelationalOperatorExpression
    | LikeOperatorExpression
    | ConcatenationOperatorExpression
    | ShortCircuitLogicalOperatorExpression
    | LogicalOperatorExpression
    | ShiftOperatorExpression
    | AwaitOperatorExpression
    ;
```

### <a name="operator-precedence-and-associativity"></a>演算子の優先順位と結合規則

式に複数の二項演算子が含まれている場合、演算子の*優先順位*によって、個々の二項演算子の評価順序が制御されます。 たとえば、式 `x + y * z` の評価は `x + (y * z)` ですが、これは `*` 演算子が `+` 演算子より高い優先順位だからです。 次の表は、二項演算子を優先順位の降順で示しています。


| __カテゴリ__     | __演算子__                                          | 
|------------------|--------------------------------------------------------|
| 1 次式          | すべての非演算子式                           |
| Await            | `Await`                                                |
| 累乗   | `^`                                                    |
| 単項マイナス符号   | `+`, `-`                                               |
| 乗法   | `*`, `/`                                               |
| 整数の除算 | `\`                                                    |
| 剰余          | `Mod`                                                  |
| 加法         | `+`, `-`                                               |
| 連結    | `&`                                                    |
| Shift            | `<<`, `>>`                                             |
| 関係       | `=`, `<>`, `<`, `>`, `<=`, `>=`, `Like`, `Is`, `IsNot` |
| 論理 NOT      | `Not`                                                  |
| 論理 AND      | `And`, `AndAlso`                                       |
| 論理 OR       | `Or`, `OrElse`                                         |
| 論理 XOR      | `Xor`                                                  |

同じ優先順位を持つ2つの演算子が式に含まれている場合、演算子の*結合規則*によって、操作の実行順序が制御されます。 すべての二項演算子は左から右へと結合されます。つまり、操作は左から右に実行されます。 優先順位と結合規則は、かっこで囲まれた式を使用して制御できます。

### <a name="object-operands"></a>オブジェクトのオペランド

各演算子でサポートされている通常の型に加えて、すべての演算子は `Object` 型のオペランドをサポートします。 @No__t 0 のオペランドに適用される演算子は、`Object` の値に対して行われるメソッド呼び出しと同様に処理されます。遅延バインディングのメソッド呼び出しが選択される場合があります。この場合、コンパイル時の型ではなく、オペランドの実行時の型によって、運用. 厳密なセマンティクスがコンパイル環境または @no__t 0 によって指定されている場合、型 `Object` のオペランドを持つ演算子は、`TypeOf...Is`、`Is`、および `IsNot` の各演算子を除き、コンパイル時エラーになります。

演算子の解決によって、操作を遅延バインディングで実行する必要があると判断された場合、演算の結果は、オペランドのランタイム型が演算子でサポートされている型である場合、オペランドの型に演算子を適用した結果になります。 値 `Nothing` は、二項演算子式の他のオペランドの型の既定値として処理されます。 単項演算子式の場合、または両方のオペランドが二項演算子式で `Nothing` の場合、演算の型は `Integer` または演算子の唯一の結果型です (演算子によって `Integer` が生成されない場合)。 操作の結果は、常に `Object` にキャストされます。 オペランドの型に有効な演算子がない場合は、@no__t 0 の例外がスローされます。 実行時の変換は、暗黙的なものでも明示的であるかに関係なく実行されます。

数値二項演算の結果としてオーバーフロー例外が発生する場合 (整数オーバーフローチェックがオンかオフかに関係なく)、結果型は可能であれば、次に大きい数値型に上位変換されます。 次に例を示します。

```vb
Module Test
    Sub Main()
        Dim o As Object = CObj(CByte(2)) * CObj(CByte(255))

        Console.WriteLine(o.GetType().ToString() & " = " & o)
    End Sub
End Module
```

次の結果が出力されます。

```console
System.Int16 = 512
```

数値の範囲を超える数値型を使用できない場合は、@no__t 0 の例外がスローされます。

### <a name="operator-resolution"></a>演算子の解決

演算子の種類とオペランドのセットを指定すると、演算子の解決によって、オペランドに使用する演算子が決まります。 演算子を解決する際、次の手順に従って、ユーザー定義の演算子が最初に考慮されます。

1. まず、候補となる演算子がすべて収集されます。 候補演算子は、ソース型の特定の演算子型のすべてのユーザー定義演算子と、対象の型の特定の型のすべてのユーザー定義演算子です。 変換元の型と変換先の型が関連付けられている場合、共通の演算子は1回だけ考慮されます。

2. 次に、演算子とオペランドにオーバーロードの解決を適用して、最も限定的な演算子を選択します。 バイナリ演算子の場合は、遅延バインディングの呼び出しが発生する可能性があります。

型の候補演算子 `T?` を収集する場合は、代わりに型 @no__t の演算子が使用されます。 Null 非許容の値型のみを含む、@no__t 0 のユーザー定義演算子も、すべてリフトされています。 リフトされた演算子は、任意の値型の null 許容バージョンを使用しますが、例外として `IsTrue` の戻り値の型と `IsFalse` (`Boolean` である必要があります) です。 リフトされた演算子は、オペランドを null 非許容バージョンに変換し、ユーザー定義の演算子を評価した後、結果の型を null 許容バージョンに変換することによって評価されます。 イーサオペランドが 0 @no__t の場合、式の結果は、結果型の null 許容バージョンとして型指定された `Nothing` の値になります。 以下に例を示します。

```vb
Structure T
    ...
End Structure

Structure S
    Public Shared Operator +(ByVal op1 As S, ByVal op2 As T) As T
        ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim x As S?
        Dim y, z As T?

        ' Valid, as S + T = T is lifted to S? + T? = T?
        z = x + y 
    End Sub
End Module
```

演算子が二項演算子で、いずれかのオペランドが参照型の場合は、演算子もリフトされますが、演算子へのバインドではエラーが発生します。 以下に例を示します。

```vb
Structure S1
    Public F1 As Integer

    Public Shared Operator +(left As S1, right As String) As S1
       ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim a? As S1
        Dim s As String
        
        ' Error: '+' is not defined for S1? and String
        a = a + s
    End Sub
End Module
```

__付箋.__ この規則は、将来のバージョンで null を反映する参照型を追加するかどうかを検討しているために存在します。この場合、2つの型の間で二項演算子を使用した場合の動作は変更されます。

変換と同様に、ユーザー定義の演算子は、リフトされた演算子よりも常に優先されます。

オーバーロードされた演算子を解決する場合、Visual Basic で定義されているクラスと、他の言語で定義されているクラスに違いがある可能性があります。

* 他の言語では、`Not`、`And`、`Or` は、論理演算子とビット処理演算子の両方としてオーバーロードされる場合があります。 外部アセンブリからインポートした場合、どちらの形式も、これらの演算子の有効なオーバーロードとして受け入れられます。 ただし、論理演算子とビット処理演算子の両方を定義する型の場合は、ビットごとの実装だけが考慮されます。

* 他の言語では、`>>` および `<<` は、符号付き演算子と符号なし演算子の両方としてオーバーロードされる場合があります。 外部アセンブリからのインポート時に、いずれかの形式が有効なオーバーロードとして受け入れられます。 ただし、符号付きと符号なしの両方の演算子を定義する型では、署名付きの実装のみが考慮されます。

* ユーザー定義演算子がオペランドに最も固有でない場合は、組み込み演算子が考慮されます。 オペランドに対して組み込み演算子が定義されておらず、いずれかのオペランドに Object 型オブジェクトがある場合、演算子は遅延バインディングされます。それ以外の場合は、コンパイル時のエラーが発生します。

以前のバージョンの Visual Basic では、Object 型のオペランドが1つだけあり、適用可能なユーザー定義演算子がない場合は、それがエラーになりました。 Visual Basic 11 の時点で、遅延バインディングが解決されました。 以下に例を示します。

```vb
Module Module1
  Sub Main()
      Dim p As Object = Nothing
      Dim U As New Uri("http://www.microsoft.com")
      Dim j = U * p  ' is now resolved late-bound
   End Sub
End Module
```

組み込み演算子を持つ型 `T` も `T?` に対して同じ演算子を定義します。 @No__t-0 に対する演算子の結果は、`T` の場合と同じになります。ただし、いずれかのオペランドが `Nothing` の場合、演算子の結果は `Nothing` になります (つまり、null 値が反映されます)。 操作の型を解決するために、`?` は、それを持つすべてのオペランドから削除され、演算の型が決定されます。いずれかのオペランドが null 許容の値型である場合は、操作の型に `?` が追加されます。 以下に例を示します。

```vb
Dim v1? As Integer = 10
Dim v2 As Long = 20

' Type of operation will be Long?
Console.WriteLine(v1 + v2)
```

各演算子は、定義されている組み込み型と、オペランド型を指定して実行された操作の型を一覧表示します。 組み込み操作の型の結果は、次の一般的な規則に従います。

* すべてのオペランドが同じ型であり、演算子が型に対して定義されている場合、変換は行われず、その型の演算子が使用されます。

* 演算子に対して型が定義されていないオペランドは、次の手順を使用して変換され、演算子は新しい型に対して解決されます。

  * オペランドは、演算子とオペランドの両方に対して定義され、暗黙的に変換可能な、次に最も幅の広い型に変換されます。

  * このような型が存在しない場合、オペランドは演算子とオペランドの両方に対して定義され、暗黙的に変換可能な次の型に変換されます。

  * そのような型がない場合、または変換を実行できない場合は、コンパイル時エラーが発生します。

* それ以外の場合は、オペランドがより広いオペランド型に変換され、その型の演算子が使用されます。 幅の狭いオペランドの型を、より広い演算子の型に暗黙的に変換できない場合、コンパイル時エラーが発生します。

ただし、これらの一般的な規則にかかわらず、演算子の結果テーブルにはいくつかの特殊なケースがあります。

__付箋.__ 書式設定の理由により、演算子の種類の表では、定義済みの名前が最初の2文字に省略されています。 したがって、"By" は @no__t 0、"UI" は `UInteger`、"St" は `String` などです。"Err" は、指定されたオペランド型に対して操作が定義されていないことを意味します。

## <a name="arithmetic-operators"></a>算術演算子

@No__t-0、`/`、`\`、`^`、`Mod`、`+`、および `-` 演算子は*算術演算子*です。

```antlr
ArithmeticOperatorExpression
    : UnaryPlusExpression
    | UnaryMinusExpression
    | AdditionOperatorExpression
    | SubtractionOperatorExpression
    | MultiplicationOperatorExpression
    | DivisionOperatorExpression
    | ModuloOperatorExpression
    | ExponentOperatorExpression
    ;
```

浮動小数点算術演算は、演算の結果の型よりも高い精度で実行される場合があります。 たとえば、一部のハードウェアアーキテクチャでは、"extended" または "long double" 浮動小数点型がサポートされています。これは、@no__t 0 型よりも範囲と有効桁数が大きく、この高い精度の型を使用してすべての浮動小数点演算を暗黙的に実行します。 低精度で浮動小数点演算を実行するためにハードウェアアーキテクチャを作成できますが、パフォーマンスが過剰に低下します。Visual Basic では、パフォーマンスと精度の両方をプランに実装する必要がなく、すべての浮動小数点演算に対して高い精度の型を使用できます。 より正確な結果を提供する以外にも、測定可能な効果はほとんどありません。 ただし、`x * y / z` という形式の式では、乗算によって `Double` の範囲外の結果が生成されますが、その後の除算では、一時的な結果が @no__t 2 の範囲に戻されます。これは、式がで評価されるという事実です。範囲の広い形式では、無限大ではなく、有限の結果が生成される場合があります。


### <a name="unary-plus-operator"></a>単項プラス演算子

```antlr
UnaryPlusExpression
    : '+' Expression
    ;
```

単項プラス演算子は @no__t 0、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`、`Single`、`Double`、および 0 の各型に対して定義されます。

__操作の種類:__


| __コンボ__ | __SB__ | __は__ | __悪夢__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __解放__ | __Si__ | __Do__ | __オーディオ__  | __ハーフ__  | __&__ | __Ob__ | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| 悪夢 | SB | By | 悪夢 | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do | Ob | 


### <a name="unary-minus-operator"></a>単項マイナス演算子

```antlr
UnaryMinusExpression
    : '-' Expression
    ;
```

単項マイナス演算子は、次の型に対して定義されます。

`SByte`、 `Short`、 `Integer`、および `Long`。 結果は、オペランドを0から減算することによって計算されます。 整数のオーバーフローチェックがオンで、オペランドの値が負の最大 `SByte`、`Short`、`Integer`、または `Long` の場合は、@no__t 4 の例外がスローされます。 それ以外の場合、オペランドの値が負の最大 `SByte`、`Short`、`Integer`、または `Long` の場合、結果は同じ値になり、オーバーフローは報告されません。

`Single` および `Double`。 結果は、符号を反転したオペランドの値 (0 と無限大を含む) です。 オペランドが NaN の場合、結果は NaN にもなります。

`Decimal`。 結果は、オペランドを0から減算することによって計算されます。

__操作の種類:__

| __コンボ__ | __SB__ | __は__ | __悪夢__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __解放__ | __Si__ | __Do__ | __オーディオ__  | __ハーフ__  | __&__ | __Ob__ | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| 悪夢 | SB | 悪夢 | 悪夢 | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do | Ob | 


### <a name="addition-operator"></a>加算演算子

加算演算子は、2つのオペランドの合計を計算します。

```antlr
AdditionOperatorExpression
    : Expression '+' LineTerminator? Expression
    ;
```

加算演算子は、次の型に対して定義されます。

* `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、および `Long`。 整数のオーバーフローチェックがオンで、合計が結果の型の範囲外である場合は、@no__t 0 の例外がスローされます。 それ以外の場合、オーバーフローは報告されず、結果の重要な上位ビットはすべて破棄されます。

* `Single` および `Double`。 合計は、IEEE 754 算術のルールに従って計算されます。

* `Decimal`。 結果の値が大きすぎて decimal 形式で表現できない場合は、@no__t 0 の例外がスローされます。 小数点以下の値が小さすぎる場合、結果は0になります。

* `String`。 2つの @no__t 0 のオペランドが連結されます。

* `Date`。 @No__t-0 型は、オーバーロードされた加算演算子を定義します。 @No__t-0 は組み込みの `Date` 型と同じであるため、これらの演算子は `Date` 型でも使用できます。

__操作の種類:__

|        | __コンボ__ | __SB__ | __は__ | __悪夢__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __解放__ | __Si__ | __Do__ | __オーディオ__  | __ハーフ__  | __&__ | __Ob__ | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| __コンボ__ | 悪夢 | SB | 悪夢 | 悪夢 | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do | Ob | 
| __SB__ |    | SB | 悪夢 | 悪夢 | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do | Ob | 
| __は__ |    |    | By | 悪夢 | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do | Ob | 
| __悪夢__ |    |    |    | 悪夢 | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do | Ob | 
| __米国__ |    |    |    |    | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do | Ob | 
| __In__ |    |    |    |    |    | In | Lo | Lo | De | De | Si | Do | Err | Err | Do | Ob | 
| __UI__ |    |    |    |    |    |    | UI | Lo | UL | De | Si | Do | Err | Err | Do | Ob | 
| __Lo__ |    |    |    |    |    |    |    | Lo | De | De | Si | Do | Err | Err | Do | Ob | 
| __UL__ |    |    |    |    |    |    |    |    | UL | De | Si | Do | Err | Err | Do | Ob | 
| __解放__ |    |    |    |    |    |    |    |    |    | De | Si | Do | Err | Err | Do | Ob | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Si | Do | Err | Err | Do | Ob | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Do | Err | Err | Do | Ob | 
| __オーディオ__ |    |    |    |    |    |    |    |    |    |    |    |    | &  | Err | & | Ob | 
| __ハーフ__ |    |    |    |    |    |    |    |    |    |    |    |    |     | &  | & | Ob | 
| __&__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | & | Ob | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |    | Ob | 


### <a name="subtraction-operator"></a>減算演算子

減算演算子は、最初のオペランドから2番目のオペランドを減算します。

```antlr
SubtractionOperatorExpression
    : Expression '-' LineTerminator? Expression
    ;
```

減算演算子は、次の型に対して定義されます。

* `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、および `Long`。 整数のオーバーフローチェックがオンで、その差が結果の型の範囲外である場合は、@no__t 0 の例外がスローされます。 それ以外の場合、オーバーフローは報告されず、結果の重要な上位ビットはすべて破棄されます。

* `Single` および `Double`。 この違いは、IEEE 754 算術のルールに従って計算されます。

* `Decimal`。 結果の値が大きすぎて decimal 形式で表現できない場合は、@no__t 0 の例外がスローされます。 小数点以下の値が小さすぎる場合、結果は0になります。

* `Date`。 @No__t-0 型は、オーバーロードされた減算演算子を定義します。 @No__t-0 は組み込みの `Date` 型と同じであるため、これらの演算子は `Date` 型でも使用できます。

__操作の種類:__

|        | __コンボ__ | __SB__ | __は__ | __悪夢__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __解放__ | __Si__ | __Do__ | __オーディオ__  | __ハーフ__  | __&__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __コンボ__ | 悪夢 | SB | 悪夢 | 悪夢 | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __SB__ |    | SB | 悪夢 | 悪夢 | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __は__ |    |    | By | 悪夢 | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __悪夢__ |    |    |    | 悪夢 | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __米国__ |    |    |    |    | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __In__ |    |    |    |    |    | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __UI__ |    |    |    |    |    |    | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __解放__ |    |    |    |    |    |    |    |    |    | De | Si | Do | Err | Err | Do  | Ob  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Si | Do | Err | Err | Do  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Do | Err | Err | Do  | Ob  | 
| __オーディオ__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __ハーフ__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __&__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Do  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


### <a name="multiplication-operator"></a>乗算演算子

乗算演算子は、2つのオペランドの積を計算します。

```antlr
MultiplicationOperatorExpression
    : Expression '*' LineTerminator? Expression
    ;
```

乗算演算子は、次の型に対して定義されます。

* `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、および `Long`。 整数のオーバーフローチェックがオンで、製品が結果の型の範囲外にある場合は、@no__t 0 の例外がスローされます。 それ以外の場合、オーバーフローは報告されず、結果の重要な上位ビットはすべて破棄されます。

* `Single` および `Double`。 この製品は、IEEE 754 算術のルールに従って計算されます。

* `Decimal`。 結果の値が大きすぎて decimal 形式で表現できない場合は、@no__t 0 の例外がスローされます。 小数点以下の値が小さすぎる場合、結果は0になります。

__操作の種類:__

|        | __コンボ__ | __SB__ | __は__ | __悪夢__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __解放__ | __Si__ | __Do__ | __オーディオ__  | __ハーフ__  | __&__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __コンボ__ | 悪夢 | SB | 悪夢 | 悪夢 | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __SB__ |    | SB | 悪夢 | 悪夢 | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __は__ |    |    | By | 悪夢 | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __悪夢__ |    |    |    | 悪夢 | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __米国__ |    |    |    |    | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __In__ |    |    |    |    |    | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __UI__ |    |    |    |    |    |    | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __解放__ |    |    |    |    |    |    |    |    |    | De | Si | Do | Err | Err | Do  | Ob  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Si | Do | Err | Err | Do  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Do | Err | Err | Do  | Ob  | 
| __オーディオ__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __ハーフ__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __&__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Do  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


### <a name="division-operators"></a>除算演算子

除算演算子は、2つのオペランドの商を計算します。 除算演算子には、標準 (浮動小数点) 除算演算子と整数除算演算子の2つがあります。

```antlr
DivisionOperatorExpression
    : FPDivisionOperatorExpression
    | IntegerDivisionOperatorExpression
    ;

FPDivisionOperatorExpression
    : Expression '/' LineTerminator? Expression
    ;

IntegerDivisionOperatorExpression
    : Expression '\\' LineTerminator? Expression
    ;
```

通常の除算演算子は、次の型に対して定義されます。

* `Single` および `Double`。 商は、IEEE 754 算術のルールに従って計算されます。

* `Decimal`。 右オペランドの値が0の場合は、@no__t 0 の例外がスローされます。 結果の値が大きすぎて decimal 形式で表現できない場合は、@no__t 0 の例外がスローされます。 小数点以下の値が小さすぎる場合、結果は0になります。 結果の小数点以下の小数点以下桁数は、最適なスケールに最も近いスケールになり、結果が正確な結果と同じになります。  優先する小数点以下桁数は、2番目のオペランドの小数点以下桁数よりも小さい1番目のオペランドの小数点以下桁数です。

通常の演算子の解決規則に従って、`Byte`、`Short`、`Integer`、`Long` などの型のオペランドの間に純粋に分割されると、両方のオペランドが型 `Decimal` に変換されます。 ただし、いずれの型も `Decimal` でない場合に除算演算子に対して演算子の解決を実行すると、`Double` は `Decimal` よりも狭く見なされます。 この規則に従うのは、`Double` 除算が `Decimal` 除算よりも効率的であるためです。

__操作の種類:__

|        | __コンボ__ | __SB__ | __は__ | __悪夢__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __解放__ | __Si__ | __Do__ | __オーディオ__  | __ハーフ__  | __&__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __コンボ__ | Do | Do | Do | Do | Do | Do | Do | Do | Do | De | Si | Do | Err | Err | Do  | Ob  | 
| __SB__ |    | Do | Do | Do | Do | Do | Do | Do | Do | De | Si | Do | Err | Err | Do  | Ob  | 
| __は__ |    |    | Do | Do | Do | Do | Do | Do | Do | De | Si | Do | Err | Err | Do  | Ob  | 
| __悪夢__ |    |    |    | Do | Do | Do | Do | Do | Do | De | Si | Do | Err | Err | Do  | Ob  | 
| __米国__ |    |    |    |    | Do | Do | Do | Do | Do | De | Si | Do | Err | Err | Do  | Ob  | 
| __In__ |    |    |    |    |    | Do | Do | Do | Do | De | Si | Do | Err | Err | Do  | Ob  | 
| __UI__ |    |    |    |    |    |    | Do | Do | Do | De | Si | Do | Err | Err | Do  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Do | Do | De | Si | Do | Err | Err | Do  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | Do | De | Si | Do | Err | Err | Do  | Ob  | 
| __解放__ |    |    |    |    |    |    |    |    |    | De | Si | Do | Err | Err | Do  | Ob  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Si | Do | Err | Err | Do  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Do | Err | Err | Do  | Ob  | 
| __オーディオ__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __ハーフ__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __&__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Do  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 

整数除算演算子は `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、および @no__t に対して定義されています。 右オペランドの値が0の場合は、@no__t 0 の例外がスローされます。 除算は、結果を0方向に丸め、結果の絶対値は、2つのオペランドの商の絶対値より小さい最大の整数になります。 2つのオペランドの符号が同じ場合、結果は0または正になります。2つのオペランドの符号が逆の場合は、0または負の値になります。 左側のオペランドが最大値 `SByte`、`Short`、`Integer`、または `Long` で、右オペランドが 4 @no__t の場合は、オーバーフローが発生します。整数オーバーフローのチェックがオンになっている場合は、@no__t 5 の例外がスローされます。 それ以外の場合、オーバーフローは報告されず、結果は左オペランドの値になります。

__付箋.__ 符号なしの型の2つのオペランドは常に0または正の値になるため、結果は常に0または正になります。  式の結果は常に2つのオペランドのうちの最大値以下であるため、オーバーフローが発生する可能性はありません。  そのため、2つの符号なし整数を使用した整数除算では、このような整数オーバーフローチェックは実行されません。 結果は、左オペランドの型と同じになります。


__操作の種類:__

|        | __コンボ__ | __SB__ | __は__ | __悪夢__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __解放__ | __Si__ | __Do__ | __オーディオ__  | __ハーフ__  | __&__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __コンボ__ | 悪夢 | SB | 悪夢 | 悪夢 | In | In | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __SB__ |    | SB | 悪夢 | 悪夢 | In | In | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __は__ |    |    | By | 悪夢 | US | In | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __悪夢__ |    |    |    | 悪夢 | In | In | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __米国__ |    |    |    |    | US | In | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __In__ |    |    |    |    |    | In | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __UI__ |    |    |    |    |    |    | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __解放__ |    |    |    |    |    |    |    |    |    | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Lo | Err | Err | Lo  | Ob  | 
| __オーディオ__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __ハーフ__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __&__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Lo  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


### <a name="mod-operator"></a>Mod 演算子

@No__t-0 (剰余) 演算子は、2つのオペランド間の除算の剰余を計算します。

```antlr
ModuloOperatorExpression
    : Expression 'Mod' LineTerminator? Expression
    ;
```

@No__t-0 演算子は、次の型に対して定義されます。

* `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、および `Long`。 @No__t-0 の結果は `x - (x \ y) * y` によって生成される値になります。 @No__t-0 が0の場合は、`System.DivideByZeroException` の例外がスローされます。 剰余演算子はオーバーフローを発生させません。

* `Single` および `Double`。 剰余は、IEEE 754 算術のルールに従って計算されます。

* `Decimal`。 右オペランドの値が0の場合は、@no__t 0 の例外がスローされます。 結果の値が大きすぎて decimal 形式で表現できない場合は、@no__t 0 の例外がスローされます。 小数点以下の値が小さすぎる場合、結果は0になります。

__操作の種類:__

|        | __コンボ__ | __SB__ | __は__ | __悪夢__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __解放__ | __Si__ | __Do__ | __オーディオ__  | __ハーフ__  | __&__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __コンボ__ | 悪夢 | SB | 悪夢 | 悪夢 | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __SB__ |    | SB | 悪夢 | 悪夢 | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __は__ |    |    | By | 悪夢 | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __悪夢__ |    |    |    | 悪夢 | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __米国__ |    |    |    |    | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __In__ |    |    |    |    |    | In | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __UI__ |    |    |    |    |    |    | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __解放__ |    |    |    |    |    |    |    |    |    | De | Si | Do | Err | Err | Do  | Ob  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Si | Do | Err | Err | Do  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Do | Err | Err | Do  | Ob  | 
| __オーディオ__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __ハーフ__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __&__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Do  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


### <a name="exponentiation-operator"></a>指数演算子

累乗演算子は、2番目のオペランドの累乗に累乗された最初のオペランドを計算します。

```antlr
ExponentOperatorExpression
    : Expression '^' LineTerminator? Expression
    ;
```

指数演算子が型 `Double` に対して定義されています。 この値は、IEEE 754 算術の規則に従って計算されます。

__操作の種類:__

|        | __コンボ__ | __SB__ | __は__ | __悪夢__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __解放__ | __Si__ | __Do__ | __オーディオ__  | __ハーフ__  | __&__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __コンボ__ | Do | Do | Do | Do | Do | Do | Do | Do | Do | Do | Do | Do | Err | Err | Do  | Ob  | 
| __SB__ |    | Do | Do | Do | Do | Do | Do | Do | Do | Do | Do | Do | Err | Err | Do  | Ob  | 
| __は__ |    |    | Do | Do | Do | Do | Do | Do | Do | Do | Do | Do | Err | Err | Do  | Ob  | 
| __悪夢__ |    |    |    | Do | Do | Do | Do | Do | Do | Do | Do | Do | Err | Err | Do  | Ob  | 
| __米国__ |    |    |    |    | Do | Do | Do | Do | Do | Do | Do | Do | Err | Err | Do  | Ob  | 
| __In__ |    |    |    |    |    | Do | Do | Do | Do | Do | Do | Do | Err | Err | Do  | Ob  | 
| __UI__ |    |    |    |    |    |    | Do | Do | Do | Do | Do | Do | Err | Err | Do  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Do | Do | Do | Do | Do | Err | Err | Do  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | Do | Do | Do | Do | Err | Err | Do  | Ob  | 
| __解放__ |    |    |    |    |    |    |    |    |    | Do | Do | Do | Err | Err | Do  | Ob  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Do | Do | Err | Err | Do  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Do | Err | Err | Do  | Ob  | 
| __オーディオ__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __ハーフ__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __&__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Do  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


## <a name="relational-operators"></a>関係演算子

*関係演算子*は、値を互いに比較します。 比較演算子は @no__t 0、`<>`、`<`、`>`、`<=`、および `>=` です。

```antlr
RelationalOperatorExpression
    : Expression '=' LineTerminator? Expression
    | Expression '<' '>' LineTerminator? Expression
    | Expression '<' LineTerminator? Expression
    | Expression '>' LineTerminator? Expression
    | Expression '<' '=' LineTerminator? Expression
    | Expression '>' '=' LineTerminator? Expression
    ;
```

すべての関係演算子は、@no__t 0 の値になります。

関係演算子には、次の一般的な意味があります。

* @No__t-0 演算子は、2つのオペランドが等しいかどうかをテストします。

* @No__t-0 演算子は、2つのオペランドが等しくないかどうかをテストします。

* @No__t-0 演算子は、最初のオペランドが2番目のオペランドより小さいかどうかをテストします。

* @No__t-0 演算子は、最初のオペランドが2番目のオペランドより大きいかどうかをテストします。

* @No__t-0 演算子は、最初のオペランドが2番目のオペランド以下かどうかをテストします。

* @No__t-0 演算子は、最初のオペランドが2番目のオペランド以上であるかどうかをテストします。

関係演算子は、次の型に対して定義されます。

* `Boolean`。 演算子は、2つのオペランドの真の値を比較します。 `True` は、数値と一致する `False` より小さいと見なされます。

* `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、および `Long`。 演算子は、2つの整数オペランドの数値を比較します。

* `Single` および `Double`。 演算子は、IEEE 754 標準の規則に従って、オペランドを比較します。

* `Decimal`。 演算子は、2つの10進オペランドの数値を比較します。

* `Date`。 これらの演算子は、2つの日付/時刻値を比較した結果を返します。

* `Char`。 これらの演算子は、2つの Unicode 値を比較した結果を返します。

* `String`。 これらの演算子は、2つの値を比較した結果を、バイナリ比較とテキスト比較のどちらかを使用して返します。 使用される比較は、コンパイル環境と `Option Compare` ステートメントによって決まります。 バイナリ比較では、各文字列の各文字の Unicode 値の数値が同じであるかどうかが判断されます。 テキスト比較では、.NET Framework で使用されている現在のカルチャに基づいて、Unicode テキストの比較が行われます。 文字列比較を実行する場合、null 値は、文字列リテラル `""` に相当します。

__操作の種類:__
        
|        | __コンボ__ | __SB__ | __は__ | __悪夢__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __解放__ | __Si__ | __Do__ | __オーディオ__  | __ハーフ__  | __&__ | __Ob__ | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| __コンボ__ | コンボ | SB | 悪夢 | 悪夢 | In | In | Lo | Lo | De | De | Si | Do | Err | Err | コンボ | Ob | 
| __SB__ |    | SB | 悪夢 | 悪夢 | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do | Ob | 
| __は__ |    |    | By | 悪夢 | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do | Ob | 
| __悪夢__ |    |    |    | 悪夢 | In | In | Lo | Lo | De | De | Si | Do | Err | Err | Do | Ob | 
| __米国__ |    |    |    |    | US | In | UI | Lo | UL | De | Si | Do | Err | Err | Do | Ob | 
| __In__ |    |    |    |    |    | In | Lo | Lo | De | De | Si | Do | Err | Err | Do | Ob | 
| __UI__ |    |    |    |    |    |    | UI | Lo | UL | De | Si | Do | Err | Err | Do | Ob | 
| __Lo__ |    |    |    |    |    |    |    | Lo | De | De | Si | Do | Err | Err | Do | Ob | 
| __UL__ |    |    |    |    |    |    |    |    | UL | De | Si | Do | Err | Err | Do | Ob | 
| __解放__ |    |    |    |    |    |    |    |    |    | De | Si | Do | Err | Err | Do | Ob | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Si | Do | Err | Err | Do | Ob | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Do | Err | Err | Do | Ob | 
| __オーディオ__ |    |    |    |    |    |    |    |    |    |    |    |    | オーディオ  | Err | オーディオ | Ob | 
| __ハーフ__ |    |    |    |    |    |    |    |    |    |    |    |    |     | ハーフ  | & | Ob | 
| __&__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | & | Ob | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |    | Ob | 


## <a name="like-operator"></a>Like 演算子

@No__t-0 演算子は、文字列が特定のパターンに一致するかどうかを判断します。

```antlr
LikeOperatorExpression
    : Expression 'Like' LineTerminator? Expression
    ;
```

@No__t-0 演算子が `String` 型に対して定義されています。 最初のオペランドは照合される文字列で、2番目のオペランドは照合するパターンです。 このパターンは、Unicode 文字で構成されています。 次の文字シーケンスには特別な意味があります。

* 文字 `?` は任意の1文字と一致します。

* 文字 `*` は、0個以上の文字と一致します。

* 文字 `#` は任意の1桁 (0-9) に一致します。

* 角かっこ (`[ab...]`) で囲まれた文字の一覧は、リスト内の任意の1文字と一致します。

* 角かっこで囲まれ、感嘆符 (@no__t 0) で始まる文字のリストは、文字リストに含まれていない任意の1文字と一致します。

* ハイフン (`-`) で区切られた文字リスト内の2つの文字は、最初の文字で始まり、2番目の文字で終わる Unicode 文字の範囲を指定します。 2番目の文字が、最初の文字より後の並べ替え順序ではない場合、実行時例外が発生します。 文字リストの先頭または末尾にあるハイフンは、それ自体を指定します。

特殊文字の左角かっこ (`[`)、疑問符 (`?`)、番号記号 (`#`)、アスタリスク (`*`) を一致させるには、角かっこで囲む必要があります。 右角かっこ (`]`) は、それ自体に一致するグループ内では使用できませんが、グループの外側で個別の文字として使用することはできます。 @No__t-0 という文字シーケンスは、文字列リテラル `""` と見なされます。 

文字の比較と文字リストの順序付けは、使用される比較の種類によって異なります。 バイナリ比較が使用されている場合、文字の比較と順序付けは、数値の Unicode 値に基づいて行われます。 テキスト比較が使用されている場合、文字の比較と順序付けは、.NET Framework で現在使用されているロケールに基づいて行われます。

言語によっては、アルファベットの特殊文字は2つの異なる文字を表し、その逆も同様です。 たとえば、複数の言語では、文字 `æ` を使用して文字 `a` と `e` が一緒に表示されます。一方、文字 `^` と @no__t は、文字 @no__t を表すために使用できます。 テキスト比較を使用する場合、@no__t 0 演算子は、このようなカルチャのとっを認識します。 その場合、pattern または string 内の1つの特殊文字が、他の文字列の等価の2文字シーケンスと一致します。 同様に、角かっこで囲まれたパターン内の1つの特殊文字 (単独、リスト、または範囲内) は、文字列内の等価の2文字シーケンスと一致します。また、その逆も同様です。

両方のオペランドが `Nothing` または1つのオペランドが `String` への組み込みの変換であり、もう一方のオペランド @no__t が-3 である @no__t 0 の式では、@no__t は空の文字列リテラルとして処理されます `""`。

__操作の種類:__

|        | __コンボ__ | __SB__ | __は__ | __悪夢__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __解放__ | __Si__ | __Do__ | __オーディオ__  | __ハーフ__  | __&__ | __Ob__ | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| __コンボ__ | & | & | & | & | & | & | & | & | & | & | & | & | & | & | & | Ob | 
| __SB__ |    | & | & | & | & | & | & | & | & | & | & | & | & | & | & | Ob | 
| __は__ |    |    | & | & | & | & | & | & | & | & | & | & | & | & | & | Ob | 
| __悪夢__ |    |    |    | & | & | & | & | & | & | & | & | & | & | & | & | Ob | 
| __米国__ |    |    |    |    | & | & | & | & | & | & | & | & | & | & | & | Ob | 
| __In__ |    |    |    |    |    | & | & | & | & | & | & | & | & | & | & | Ob | 
| __UI__ |    |    |    |    |    |    | & | & | & | & | & | & | & | & | & | Ob | 
| __Lo__ |    |    |    |    |    |    |    | & | & | & | & | & | & | & | & | Ob | 
| __UL__ |    |    |    |    |    |    |    |    | & | & | & | & | & | & | & | Ob | 
| __解放__ |    |    |    |    |    |    |    |    |    | & | & | & | & | & | & | Ob | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | & | & | & | & | & | Ob | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | & | & | & | & | Ob | 
| __オーディオ__ |    |    |    |    |    |    |    |    |    |    |    |    | & | & | & | Ob | 
| __ハーフ__ |    |    |    |    |    |    |    |    |    |    |    |    |    | & | & | Ob | 
| __&__ |    |    |    |    |    |    |    |    |    |    |    |    |    |    | & | Ob | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    | Ob | 


## <a name="concatenation-operator"></a>連結演算子

```antlr
ConcatenationOperatorExpression
    : Expression '&' LineTerminator? Expression
    ;
```

*連結演算子*は、組み込み値型の null 許容バージョンを含む、すべての組み込み型に対して定義されます。 また、前述の型と `System.DBNull` の連結にも定義されています。これは、@no__t 1 の文字列として扱われます。 連結演算子は、そのすべてのオペランドを `String` に変換します。式では、厳密なセマンティクスが使用されているかどうかにかかわらず、`String` への変換はすべて、拡大されていると見なされます。 @No__t-0 の値は、`String` として型指定されたリテラル @no__t に変換されます。 値が @no__t 0 の null 許容値型は、実行時エラーをスローするのではなく、`String` として型指定されたリテラル @no__t にも変換されます。

連結演算では、2つのオペランドを左から右に連結した文字列が生成されます。 値 `Nothing` は、空の文字列リテラル `""` として扱われます。

__操作の種類:__

|        | __コンボ__ | __SB__ | __は__ | __悪夢__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __解放__ | __Si__ | __Do__ | __オーディオ__  | __ハーフ__  | __&__ | __Ob__ | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| __コンボ__ | & | & | & | & | & | & | & | & | & | & | & | & | & | & | & | Ob | 
| __SB__ |    | & | & | & | & | & | & | & | & | & | & | & | & | & | & | Ob | 
| __は__ |    |    | & | & | & | & | & | & | & | & | & | & | & | & | & | Ob | 
| __悪夢__ |    |    |    | & | & | & | & | & | & | & | & | & | & | & | & | Ob | 
| __米国__ |    |    |    |    | & | & | & | & | & | & | & | & | & | & | & | Ob | 
| __In__ |    |    |    |    |    | & | & | & | & | & | & | & | & | & | & | Ob | 
| __UI__ |    |    |    |    |    |    | & | & | & | & | & | & | & | & | & | Ob | 
| __Lo__ |    |    |    |    |    |    |    | & | & | & | & | & | & | & | & | Ob | 
| __UL__ |    |    |    |    |    |    |    |    | & | & | & | & | & | & | & | Ob | 
| __解放__ |    |    |    |    |    |    |    |    |    | & | & | & | & | & | & | Ob | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | & | & | & | & | & | Ob | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | & | & | & | & | Ob | 
| __オーディオ__ |    |    |    |    |    |    |    |    |    |    |    |    | & | & | & | Ob | 
| __ハーフ__ |    |    |    |    |    |    |    |    |    |    |    |    |    | & | & | Ob | 
| __&__ |    |    |    |    |    |    |    |    |    |    |    |    |    |    | & | Ob | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    | Ob | 


## <a name="logical-operators"></a>論理演算子

@No__t-0、`Not`、`Or`、および `Xor` の各演算子は、論理演算子と呼ばれます。

```antlr
LogicalOperatorExpression
    : 'Not' Expression
    | Expression 'And' LineTerminator? Expression
    | Expression 'Or' LineTerminator? Expression
    | Expression 'Xor' LineTerminator? Expression
    ;
```

論理演算子は次のように評価されます。

* @No__t-0 型の場合:

  * 2つのオペランドに対して論理 @no__t 0 操作が実行されます。

  * オペランドに対して論理 @no__t 0 演算が実行されます。

  * 2つのオペランドに対して論理 @no__t 0 操作が実行されます。

  * 2つのオペランドに対して、論理的な排他的 `Or` 演算が実行されます。

* @No__t 0、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、@no__t、およびすべての列挙型の場合、指定された操作は、2つのオペランドのバイナリ表現の各ビットに対して実行されます。

  * `And`:両方のビットが1の場合、結果のビットは1になります。それ以外の場合、結果のビットは0になります。

  * `Not`:ビットが0の場合、結果のビットは1になります。それ以外の場合、結果のビットは1になります。

  * `Or`:いずれかのビットが1の場合、結果のビットは1になります。それ以外の場合、結果のビットは0になります。

  * `Xor`:いずれかのビットが1で、両方のビットが1ではない場合、結果のビットは1になります。それ以外の場合、結果のビットは 0 (つまり、1 `Xor` 0 = 1、1 `Xor` 1 = 0) になります。

* 論理演算子 `And` および `Or` が型 `Boolean?` でリフトされている場合、次のように3つの値を持つブール型のロジックが含まれるように拡張されます。

  * `And` は、両方のオペランドが true の場合、true に評価されます。オペランドのいずれかが false の場合は false。それ以外の場合は `Nothing`。

  * `Or` のいずれかのオペランドが true の場合、true に評価されます。false は両方とも false です。それ以外の場合は `Nothing`。

以下に例を示します。

```vb
Module Test
    Sub Main()
        Dim x?, y? As Boolean

        x = Nothing
        y = True 

        If x Or y Then
            ' Will execute
        End If
    End Sub
End Module
```

__付箋.__ 理想的には、ブール式 (たとえば、`IsTrue` と `IsFalse` を実装する型) で使用できる任意の型に対して3つの値のロジックを使用して、論理演算子 `And` および @no__t をリフトします。 @no__t これは、任意のブール式で使用できる型。 残念ながら、3つの値を持つリフトは `Boolean?` にのみ適用されるため、3つの値を持つロジックを必要とするユーザー定義型では、null 許容バージョンに対して `And` および `Or` 演算子を定義することで、手動で行う必要があります。

これらの操作によってオーバーフローが発生することはありません。 列挙型の演算子は、列挙型の基になる型に対してビットごとの演算を実行しますが、戻り値は列挙型です。

__操作の種類がありません:__

| __コンボ__ | __SB__ | __は__ | __悪夢__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __解放__ | __Si__ | __Do__ | __オーディオ__  | __ハーフ__  | __&__ | __Ob__ | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| コンボ | SB | By | 悪夢 | US | In | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo | Ob | 

__And、Or、Xor 演算型:__

|        | __コンボ__ | __SB__ | __は__ | __悪夢__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __解放__ | __Si__ | __Do__ | __オーディオ__  | __ハーフ__  | __&__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __コンボ__ | コンボ | SB | 悪夢 | 悪夢 | In | In | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | コンボ  | Ob  | 
| __SB__ |    | SB | 悪夢 | 悪夢 | In | In | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __は__ |    |    | By | 悪夢 | US | In | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __悪夢__ |    |    |    | 悪夢 | In | In | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __米国__ |    |    |    |    | US | In | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __In__ |    |    |    |    |    | In | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __UI__ |    |    |    |    |    |    | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __解放__ |    |    |    |    |    |    |    |    |    | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Lo | Err | Err | Lo  | Ob  | 
| __オーディオ__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __ハーフ__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __&__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Lo  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


### <a name="short-circuiting-logical-operators"></a>ショートサーキット論理演算子

@No__t-0 および `OrElse` 演算子は、`And` および `Or` 論理演算子のショートサーキットバージョンです。

```antlr
ShortCircuitLogicalOperatorExpression
    : Expression 'AndAlso' LineTerminator? Expression
    | Expression 'OrElse' LineTerminator? Expression
    ;
```

2番目のオペランドは、最初のオペランドを評価した後に演算子の結果がわかっている場合に、実行時に評価されません。

ショートサーキット論理演算子は、次のように評価されます。

* @No__t 0 演算の最初のオペランドが `False` に評価された場合、または `IsFalse` 演算子から True が返された場合、式は最初のオペランドを返します。 それ以外の場合は、2番目のオペランドが評価され、2つの結果に対して論理 @no__t 0 演算が実行されます。

* @No__t 0 演算の最初のオペランドが `True` に評価された場合、または `IsTrue` 演算子から True が返された場合、式は最初のオペランドを返します。 それ以外の場合は、2番目のオペランドが評価され、2つの結果に対して論理 @no__t 0 演算が実行されます。

@No__t-0 および `OrElse` 演算子は、型 `Boolean`、または次の演算子をオーバーロードする型 `T` に対して定義されています。

```vb
Public Shared Operator IsTrue(op As T) As Boolean
Public Shared Operator IsFalse(op As T) As Boolean
```

また、対応する `And` または `Or` 演算子をオーバーロードすることもできます。

```vb
Public Shared Operator And(op1 As T, op2 As T) As T
Public Shared Operator Or(op1 As T, op2 As T) As T
```

@No__t-0 または `OrElse` の演算子を評価する場合、最初のオペランドは1回だけ評価され、2番目のオペランドは評価も評価もされません。 次に例を示します。

```vb
Module Test
    Function TrueValue() As Boolean
        Console.Write(" True")
        Return True
    End Function

    Function FalseValue() As Boolean
        Console.Write(" False")
        Return False
    End Function

    Sub Main()
        Console.Write("And:")
        If FalseValue() And TrueValue() Then
        End If
        Console.WriteLine()

        Console.Write("Or:")
        If TrueValue() Or FalseValue() Then
        End If
        Console.WriteLine()

        Console.Write("AndAlso:")
        If FalseValue() AndAlso TrueValue() Then
        End If
        Console.WriteLine()

        Console.Write("OrElse:")
        If TrueValue() OrElse FalseValue() Then
        End If
        Console.WriteLine()
    End Sub
End Module
```

次の結果が出力されます。

```console
And: False True
Or: True False
AndAlso: False
OrElse: True
```

@No__t-0 および `OrElse` 演算子のリフトされた形式では、最初のオペランドが null `Boolean?` の場合、2番目のオペランドが評価されますが、結果は常に null `Boolean?` になります。

__操作の種類:__

|        | __コンボ__ | __SB__ | __は__ | __悪夢__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __解放__ | __Si__ | __Do__ | __オーディオ__  | __ハーフ__  | __&__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __コンボ__ | コンボ | コンボ | コンボ | コンボ | コンボ | コンボ | コンボ | コンボ | コンボ | コンボ | コンボ | コンボ | Err | Err | コンボ  | Ob  | 
| __SB__ |    | コンボ | コンボ | コンボ | コンボ | コンボ | コンボ | コンボ | コンボ | コンボ | コンボ | コンボ | Err | Err | コンボ  | Ob  | 
| __は__ |    |    | コンボ | コンボ | コンボ | コンボ | コンボ | コンボ | コンボ | コンボ | コンボ | コンボ | Err | Err | コンボ  | Ob  | 
| __悪夢__ |    |    |    | コンボ | コンボ | コンボ | コンボ | コンボ | コンボ | コンボ | コンボ | コンボ | Err | Err | コンボ  | Ob  | 
| __米国__ |    |    |    |    | コンボ | コンボ | コンボ | コンボ | コンボ | コンボ | コンボ | コンボ | Err | Err | コンボ  | Ob  | 
| __In__ |    |    |    |    |    | コンボ | コンボ | コンボ | コンボ | コンボ | コンボ | コンボ | Err | Err | コンボ  | Ob  | 
| __UI__ |    |    |    |    |    |    | コンボ | コンボ | コンボ | コンボ | コンボ | コンボ | Err | Err | コンボ  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | コンボ | コンボ | コンボ | コンボ | コンボ | Err | Err | コンボ  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | コンボ | コンボ | コンボ | コンボ | Err | Err | コンボ  | Ob  | 
| __解放__ |    |    |    |    |    |    |    |    |    | コンボ | コンボ | コンボ | Err | Err | コンボ  | Ob  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | コンボ | コンボ | Err | Err | コンボ  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | コンボ | Err | Err | コンボ  | Ob  | 
| __オーディオ__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __ハーフ__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __&__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | コンボ  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


## <a name="shift-operators"></a>シフト演算子

二項演算子 `<<` および `>>` は、ビットシフト演算を実行します。

```antlr
ShiftOperatorExpression
    : Expression '<' '<' LineTerminator? Expression
    | Expression '>' '>' LineTerminator? Expression
    ;
```

これらの演算子は、@no__t 0、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、および @no__t 7 の各型に対して定義されています。 他の二項演算子とは異なり、シフト演算の結果の型は、演算子が左側のオペランドだけを持つ単項演算子であったかのように決定されます。 右オペランドの型は `Integer` に暗黙的に変換できる必要があり、操作の結果の型を決定するときには使用されません。

@No__t-0 演算子を使用すると、最初のオペランドのビットがシフトの量によって指定された桁数から左にシフトされます。 結果型の範囲外の上位ビットは破棄され、下位空いているビット位置はゼロで埋められます。

@No__t-0 演算子を使用すると、最初のオペランドのビットが、シフト量で指定された場所の数だけ右にシフトされます。 下位ビットは破棄され、左オペランドが正の場合は0に、負の場合は1に設定されます。 左オペランドの型が `Byte`、`UShort`、`UInteger`、または `ULong` の場合、空席の上位ビットはゼロで埋められます。

シフト演算子は、最初のオペランドの基になる表現のビットを2番目のオペランドの量でシフトします。 2番目のオペランドの値が1番目のオペランドのビット数より大きい場合、または負の値の場合、シフトの量は @no__t 0 として計算されます。 `SizeMask` は次のようになります。

| __LeftOperand 型__  | __SizeMask__ | 
|-----------------------|--------------|
| `Byte`, `SByte`       | 7 (`&H7`)    | 
| `UShort`, `Short`     | 15 (`&HF`)   | 
| `UInteger`, `Integer` | 31 (`&H1F`)  | 
| `ULong`, `Long`       | 63 (`&H3F`)  | 

シフトの量がゼロの場合、演算の結果は最初のオペランドの値と同じになります。 これらの操作によってオーバーフローが発生することはありません。

__操作の種類:__


| __コンボ__ | __SB__ | __は__ | __悪夢__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __解放__ | __Si__ | __Do__ | __オーディオ__  | __ハーフ__  | __&__ | __Ob__ | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| 悪夢 | SB | By | 悪夢 | US | In | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo | Ob | 


## <a name="boolean-expressions"></a>Boolean 式

ブール式は、true であるか false であるかを確認するためにテストできる式です。

```antlr
BooleanExpression
    : Expression
    ;
```

ブール式では、次のような優先順位で型 `T` を使用できます。

* `T` が `Boolean` または `Boolean?` です。

* `T` は `Boolean` への拡大変換を行っています

* `T` は `Boolean?` への拡大変換を行っています

* `T` は、2つの擬似演算子 (`IsTrue` と `IsFalse`) を定義します。

* `T` には、`Boolean` から `Boolean?` への変換が関係しない `Boolean?` への縮小変換が含まれています。

* `T` は `Boolean` に縮小変換します。

__付箋.__ @No__t-0 がオフの場合、`Boolean` への縮小変換を含む式は、コンパイル時エラーが発生することなく受け入れられますが、言語では、存在する場合は `IsTrue` 演算子が優先されます。 これは、`Option Strict` は、その言語で受け入れられていないものだけを変更し、式の実際の意味を変更しないためです。 したがって、`Option Strict` に関係なく、`IsTrue` は常に縮小変換よりも優先される必要があります。

たとえば、次のクラスは `Boolean` への拡大変換を定義していません。 その結果、`If` ステートメントで使用すると、`IsTrue` 演算子が呼び出されます。

```vb
Class MyBool
    Public Shared Widening Operator CType(b As Boolean) As MyBool
        ...
    End Operator

    Public Shared Narrowing Operator CType(b As MyBool) As Boolean
        ...
    End Operator

    Public Shared Operator IsTrue(b As MyBool) As Boolean
        ...
    End Operator

    Public Shared Operator IsFalse(b As MyBool) As Boolean
        ...
    End Operator
End Class

Module Test
    Sub Main()
        Dim b As New MyBool

        If b Then Console.WriteLine("True")
    End Sub
End Module
```

ブール式がとして型指定された場合、または `Boolean` または `Boolean?` に変換された場合は、値が `True` および false の場合は true になります。

それ以外の場合、ブール式は `IsTrue` 演算子を呼び出し、演算子が `True`; を返した場合は `True` を返します。それ以外の場合は false になります (ただし、`IsFalse` 演算子は呼び出されません)。

次の例では、`Integer` に `Boolean` への縮小変換が含まれているため、null `Integer?` では `Boolean?` (null @no__t) と `Boolean` (例外がスローされます) の両方に縮小変換されます。 @No__t-0 への縮小変換が優先されるため、ブール式として "`i`" の値が `False` になります。

```vb
Dim i As Integer? = Nothing
If i Then Console.WriteLine()
```


## <a name="lambda-expressions"></a>ラムダ式

*ラムダ式*は、*ラムダメソッド*と呼ばれる匿名メソッドを定義します。 ラムダメソッドを使用すると、デリゲート型を受け取る他のメソッドに "インライン" メソッドを簡単に渡すことができます。

```antlr
LambdaExpression
    : SingleLineLambda
    | MultiLineLambda
    ;

SingleLineLambda
    : LambdaModifier* 'Function' ( OpenParenthesis ParameterList? CloseParenthesis )? Expression
    | 'Sub' ( OpenParenthesis ParameterList? CloseParenthesis )? Statement
    ;

MultiLineLambda
    : MultiLineFunctionLambda
    | MultiLineSubLambda
    ;

MultiLineFunctionLambda
    : LambdaModifier? 'Function' ( OpenParenthesis ParameterList? CloseParenthesis )? ( 'As' TypeName )? LineTerminator
      Block
      'End' 'Function'
    ;

MultiLineSubLambda
    : LambdaModifier? 'Sub' ( OpenParenthesis ParameterList? CloseParenthesis )? LineTerminator
      Block
      'End' 'Sub'
    ;

LambdaModifier
    : 'Async' | 'Iterator'
    ;
```

次に例を示します。

```vb
Module Test
    Delegate Function IntFunc(x As Integer) As Integer

    Sub Apply(a() As Integer, func As IntFunc)
        For index As Integer = 0 To a.Length - 1
            a(index) = func(a(index))
        Next index
    End Sub

    Sub Main()
        Dim a() As Integer = { 1, 2, 3, 4 }

        Apply(a, Function(x As Integer) x * 2)

        For Each value In a
            Console.Write(value & " ")
        Next value
    End Sub
End Module
```

次のように出力されます。

```console
2 4 6 8
```

ラムダ式は、オプションの修飾子 `Async` または `Iterator` で始まり、その後にキーワード `Function` または `Sub` とパラメーターリストが続きます。 ラムダ式のパラメーターを `Optional` または `ParamArray` に宣言することはできません。また、属性を指定することもできません。 通常のメソッドとは異なり、ラムダメソッドのパラメーター型を省略しても、`Object` は自動的に推論されません。 代わりに、ラムダメソッドが再分類されると、省略されたパラメーターの型と @no__t 0 の修飾子が対象の型から推論されます。 前の例では、ラムダ式は `Function(x) x * 2` として記述されている可能性があり、ラムダメソッドを使用して `IntFunc` デリゲート型のインスタンスを作成すると、`x` の型が `Integer` であると推論されました。 ローカル変数の推論とは異なり、ラムダメソッドのパラメーターが型を省略しても、配列または null 許容の名前修飾子がある場合は、コンパイル時エラーが発生します。

__通常のラムダ式__は、`Async` でも @no__t でもありません。

__反復子ラムダ式__は、`Iterator` 修飾子を持ち、`Async` 修飾子を指定していません。 関数である必要があります。 値に再分類されると、戻り値の型が @no__t 0、または `IEnumerable` であるデリゲート型の値にのみ再分類できます。また、一部の @no__t では `IEnumerator(Of T)` または `IEnumerable(Of T)` であり、ByRef パラメーターはありません。

__非同期ラムダ式__は、`Async` 修飾子を持ち、`Iterator` 修飾子を指定していません。 非同期サブラムダは、ByRef パラメーターを持たないサブデリゲート型の値にのみ再分類できます。 非同期関数ラムダは、戻り値の型が @no__t 0 であるか、または `T` の `Task(Of T)` で、ByRef パラメーターを持たない関数デリゲート型の値にのみ再分類できます。

ラムダ式は、単一行または複数行にすることができます。 単一行 `Function` ラムダ式には、ラムダメソッドから返された値を表す単一の式が含まれます。 単一行 `Sub` ラムダ式には、1つのステートメントが含まれています。その場合、`StatementTerminator` は終わりません。 以下に例を示します。

```vb
Module Test
    Sub Do(a() As Integer, action As Action(Of Integer))
        For index As Integer = 0 To a.Length - 1
            action(a(index))
        Next index
    End Sub

    Sub Main()
        Dim a() As Integer = { 1, 2, 3, 4 }

        Do(a, Sub(x As Integer) Console.WriteLine(x))
    End Sub
End Module
```

単一行のラムダ構造は、他のすべての式およびステートメントよりも密にバインドされます。 したがって、たとえば、"`Function() x + 5`" は、"`(Function() x) + 5`" ではなく "`Function() (x+5)"`" に相当します。 あいまいさを避けるために、単一行の `Sub` ラムダ式には、Dim ステートメントまたはラベル宣言ステートメントを含めることはできません。 また、かっこで囲まれていない限り、単一行の `Sub` ラムダ式は、直後にコロン ":"、メンバーアクセス演算子 "."、ディクショナリメンバーアクセス演算子 "!"、または始めかっこ "(" を指定することはできません。 これには、block ステートメント (`With`、`SyncLock, If...EndIf`、`While`、`For`、`Do`、`Using`) と `OnError` は含まれません。

__付箋.__ ラムダ式 `Function(i) x=i` の場合、本体は*式*として解釈されます (`x` と @no__t が等しいかどうかをテストします)。 ただし、ラムダ式 `Sub(i) x=i` の場合、本文はステートメントとして解釈されます (`i` は `x` に割り当てられます)。

複数行のラムダ式には、ステートメントブロックが含まれており、適切な @no__t 0 ステートメント (つまり `End Function` または `End Sub`) で終わる必要があります。 通常のメソッドと同様に、複数行のラムダメソッドの `Function` または @no__t 1 つのステートメントと @no__t 2 つのステートメントをそれぞれの行に記述する必要があります。 以下に例を示します。

```vb
' Error: Function statement must be on its own line!
Dim x = Sub(x As Integer) : Console.WriteLine(x) : End Sub

' OK
Dim y = Sub(x As Integer)
               Console.WriteLine(x)
          End Sub
```

複数行の `Function` ラムダ式は戻り値の型を宣言できますが、属性を配置することはできません。 複数行の `Function` ラムダ式で戻り値の型が宣言されていないが、戻り値の型をラムダ式が使用されているコンテキストから推論できる場合、その戻り値の型が使用されます。 それ以外の場合、関数の戻り値の型は次のように計算されます。

* 通常のラムダ式では、戻り値の型は、ステートメントブロック内のすべての `Return` ステートメント内の式の中で最も優先的な型になります。

* 非同期ラムダ式では、戻り値の型は 0 @no__t。ここで `T` は、ステートメントブロック内のすべての `Return` ステートメント内の式の中で最も優先される型です。

* 反復子ラムダ式では、戻り値の型は 0 @no__t。ここで `T` は、ステートメントブロック内のすべての `Yield` ステートメント内の式の中で最も優先される型です。

以下に例を示します。

```vb
Function f(min As Integer, max As Integer) As IEnumerable(Of Integer)
    If min > max Then Throw New ArgumentException()
    Dim x = Iterator Function()
                  For i = min To max
                    Yield i
                Next
               End Function

    ' infers x to be a delegate with return type IEnumerable(Of Integer)
    Return x()
End Function
```

どのような場合でも、`Return` (それぞれ `Yield`) ステートメントが存在しない場合、または優先度の高い型が存在せず、厳密なセマンティクスが使用されている場合は、コンパイル時エラーが発生します。それ以外の場合、優先される型は暗黙的に `Object` です。

戻り値の型は、到達できない場合でも、すべての `Return` ステートメントから計算されることに注意してください。 以下に例を示します。

```vb
' Return type is Double
Dim x = Function()
              Return 10
               Return 10.50
          End Function
```

変数の名前が存在しないため、暗黙的な戻り値の変数はありません。

複数行のラムダ式内のステートメントブロックには、次の制限があります。

* `On Error` および `Resume` ステートメントは許可されていませんが、`Try` ステートメントは許可されています。

* 複数行のラムダ式では、静的ローカル変数を宣言できません。

* 複数行のラムダ式のステートメントブロックに分岐することはできませんが、通常の分岐規則が適用されます。 以下に例を示します。

  ```vb
  Label1:
  Dim x = Sub()
                 ' Error: Cannot branch out
                 GoTo Label1

                 ' OK: Wholly within the lamba.
                 GoTo Label2:
            Label2:
            End Sub

  ' Error: Cannot branch in
  GoTo Label2
  ```

ラムダ式は、含んでいる型で宣言された匿名メソッドとほぼ同じです。 最初の例は次のようにほぼ同じです。

```vb
Module Test
    Delegate Function IntFunc(x As Integer) As Integer

    Sub Apply(a() As Integer, func As IntFunc)
        For index As Integer = 0 To a.Length - 1
            a(index) = func(a(index))
        Next index
    End Sub

    Function $Lambda1(x As Integer) As Integer
        Return x * 2
    End Function

    Sub Main()
        Dim a() As Integer = { 1, 2, 3, 4 }

        Apply(a, AddressOf $Lambda1)

        For Each value In a
            Console.Write(value & " ")
        Next value
    End Sub
End Module
```


### <a name="closures"></a>休業

ラムダ式は、スコープ内のすべての変数にアクセスできます。これには、外側のメソッドとラムダ式で定義されているローカル変数やパラメーターも含まれます。 ラムダ式がローカル変数またはパラメーターを参照する場合、ラムダ式は、参照先の変数をクロージャにキャプチャします。 クロージャは、スタックではなくヒープ上に存在し、変数がキャプチャされると、その変数へのすべての参照がクロージャにリダイレクトされるオブジェクトです。 これにより、ラムダ式は、外側のメソッドが完了した後でも、ローカル変数とパラメーターを引き続き参照できます。 以下に例を示します。

```vb
Module Test
    Delegate Function D() As Integer

    Function M() As D
        Dim x As Integer = 10
        Return Function() x
    End Function

    Sub Main()
        Dim y As D = M()

        ' Prints 10
        Console.WriteLine(y())
    End Sub
End Module
```

は、次の場合とほぼ同じです。

```vb
Module Test
    Delegate Function D() As Integer

    Class $Closure1
        Public x As Integer

        Function $Lambda1() As Integer
            Return x
        End Function
    End Class

    Function M() As D
        Dim c As New $Closure1()
        c.x = 10
        Return AddressOf c.$Lambda1
    End Function

    Sub Main()
        Dim y As D = M()

        ' Prints 10
        Console.WriteLine(y())
    End Sub
End Module
```

クロージャは、ローカル変数が宣言されているブロックに入るたびにローカル変数の新しいコピーをキャプチャしますが、新しいコピーは、前のコピーの値 (存在する場合) を使用して初期化されます。 以下に例を示します。

```vb
Module Test
    Delegate Function D() As Integer

    Function M() As D()
        Dim a(9) As D

        For i As Integer = 0 To 9
            Dim x
            a(i) = Function() x
            x += 1
        Next i

        Return a
    End Function

    Sub Main()
        Dim y() As D = M()

        For i As Integer = 0 To 9
            Console.Write(y(i)() & " ")
        Next i
    End Sub
End Module
```

出力

```console
1 2 3 4 5 6 7 8 9 10
```

代わりに

```console
9 9 9 9 9 9 9 9 9 9
```

クロージャはブロックを入力するときに初期化する必要があるため、そのブロックの外部からクロージャを持つブロックには0を @no__t ことはできませんが、クロージャを持つブロックには-1 を @no__t ことができます。 以下に例を示します。

```vb
Module Test
    Sub Main()
        Dim a = 10

        If a = 10 Then
L1:
            Dim x = Function() a

            ' Valid, source is within block
            GoTo L2
L2:
        End If

        ' ERROR: target is inside block with closure
        GoTo L1
    End Sub
End Module
```

クロージャにキャプチャすることはできないため、ラムダ式の内部に次の文字列を含めることはできません。

* 参照パラメーター。

* @No__t-3 の型がクラスでない場合は、インスタンス式 (`Me`、`MyClass`、`MyBase`)。

ラムダ式が式の一部である場合は、匿名型作成式のメンバー。 以下に例を示します。

```vb
' Error: Lambda cannot refer to anonymous type field
Dim x = New With { .a = 12, .b = Function() .a }
```

インスタンスコンストラクター内のインスタンス変数、または値のないコンテキストで変数が使用されている共有コンストラクター内の @no__t 共有変数の @no__t 0 です。 以下に例を示します。

```vb
Class C1
    ReadOnly F1 As Integer

    Sub New()
        ' Valid, doesn't modify F1
        Dim x = Function() F1

        ' Error, tries to modify F1
        Dim f = Function() ModifyValue(F1)
    End Sub

    Sub ModifyValue(ByRef x As Integer)
    End Sub
End Class
```

## <a name="query-expressions"></a>クエリ式

*クエリ式*は、クエリ可能*なコレクションの*要素に一連の*クエリ演算子*を適用する式です。 たとえば、次の式では、@no__t 0 のオブジェクトのコレクションを受け取り、ワシントン州のすべての顧客の名前を返します。

```vb
Dim names = _
    From cust In Customers _
    Where cust.State = "WA" _
    Select cust.Name
```

クエリ式は、@no__t 0 または `Aggregate` 演算子で始まり、任意のクエリ演算子で終わることができます。 クエリ式の結果は、値として分類されます。式の結果型は、式の最後のクエリ演算子の結果の型によって異なります。

```antlr
QueryExpression
    : FromOrAggregateQueryOperator QueryOperator*
    ;

FromOrAggregateQueryOperator
    : FromQueryOperator
    | AggregateQueryOperator
    ;

QueryOperator
    : FromQueryOperator
    | AggregateQueryOperator
    | SelectQueryOperator
    | DistinctQueryOperator
    | WhereQueryOperator
    | OrderByQueryOperator
    | PartitionQueryOperator
    | LetQueryOperator
    | GroupByQueryOperator
    | JoinOrGroupJoinQueryOperator
    ;

JoinOrGroupJoinQueryOperator
    : JoinQueryOperator
    | GroupJoinQueryOperator
    ;
```

### <a name="range-variables"></a>範囲変数

一部のクエリ演算子は、*範囲変数*と呼ばれる特殊な種類の変数を導入します。 範囲変数は実際の変数ではありません。代わりに、入力コレクションに対するクエリの評価時に個々の値を表します。

```antlr
CollectionRangeVariableDeclarationList
    : CollectionRangeVariableDeclaration ( Comma CollectionRangeVariableDeclaration )*
    ;

CollectionRangeVariableDeclaration
    : Identifier ( 'As' TypeName )? 'In' LineTerminator? Expression
    ;

ExpressionRangeVariableDeclarationList
    : ExpressionRangeVariableDeclaration ( Comma ExpressionRangeVariableDeclaration )*
    ;

ExpressionRangeVariableDeclaration
    : Identifier ( 'As' TypeName )? Equals Expression
    ;
```

範囲変数は、クエリ演算子の導入からクエリ式の末尾まで、またはクエリ演算子 (非表示にする `Select` など) にスコープが設定されます。 たとえば、次のクエリでは、

```vb
Dim waCusts = _
    From cust As Customer In Customers _
    Where cust.State = "WA"
```

`From` クエリ演算子では、`Customers` コレクション内の各顧客を表す `Customer` として指定された範囲変数 `cust` を導入します。 次の `Where` クエリ演算子は、フィルター式の範囲変数 `cust` を参照して、結果として得られるコレクションから個々の顧客をフィルター処理するかどうかを決定します。

範囲変数には、*コレクション範囲*変数と*式範囲変数*の2種類があります。 コレクション範囲変数は、クエリ対象のコレクションの要素から値を取得します。 コレクション範囲変数宣言内のコレクション式は、型がクエリ可能である値として分類する必要があります。 コレクション範囲変数の型が省略されている場合は、コレクション式の要素型であると推論されます。または、コレクション式に要素型がない場合は `Object` (つまり、`Cast` のメソッドのみを定義します) を指定します。 コレクション式がクエリ可能でない場合 (つまり、コレクションの要素型を推論できない場合)、コンパイル時エラーが発生します。

式の範囲変数は、コレクションではなく、式によって計算される値を持つ範囲変数です。 次の例では、`Select` クエリ演算子によって、2つのフィールドから計算された `cityState` という式の範囲変数が導入されています。

```vb
Dim cityStates = _
    From cust As Customer In Customers _
    Select cityState = cust.City & "," & cust.State _
    Where cityState.Length() < 10
```

式の範囲変数は、別の範囲変数を参照するためには必要ありませんが、このような変数はあいまい値にすることができます。 式の範囲変数に割り当てられた式は、値として分類される必要があります。また、指定されている場合は、範囲変数の型に暗黙的に変換可能である必要があります。

Let 演算子内でのみ、式の範囲変数の型を指定できます。 他の演算子では、または型が指定されていない場合は、範囲変数の型を決定するためにローカル変数型の推定が使用されます。

範囲変数は、シャドウ処理に関してローカル変数を宣言するための規則に従う必要があります。 したがって、範囲変数は、外側のメソッドまたは別の範囲変数のローカル変数またはパラメーターの名前を非表示にすることはできません (クエリ演算子によってスコープ内の現在の範囲変数がすべて非表示にされている場合を除きます)。


### <a name="queryable-types"></a>クエリ可能な型

クエリ式は、式をコレクション型の既知のメソッドの呼び出しに変換することによって実装されます。 これらの適切に定義されたメソッドは、クエリ可能なコレクションの要素型と、コレクションに対して実行されるクエリ演算子の結果の型を定義します。 各クエリ演算子は、クエリ演算子が一般的に変換されるメソッドを指定します。ただし、特定の変換は実装に依存します。 メソッドは、次のような一般的な形式を使用して仕様で指定されます。

```vb
Function Select(selector As Func(Of T, R)) As CR
```

メソッドには次のものが適用されます。

* メソッドは、コレクション型のインスタンスまたは拡張メンバーであり、アクセス可能である必要があります。

* メソッドは、すべての型引数を推論できる場合に、ジェネリックである可能性があります。

* メソッドはオーバーロードされる場合があります。この場合、オーバーロードの解決を使用して、使用するメソッドを正確に決定します。

* 同じシグネチャ (戻り値の型を含む) が一致する `Func` 型として指定されている場合、デリゲート `Func` 型の代わりに別のデリゲート型を使用できます。

* @No__t-2 は、対応する `Func` 型と同じシグネチャを持つデリゲート型 (戻り値の型を含む) である場合、@no__t デリゲートの代わりに `System.Linq.Expressions.Expression(Of D)` 型を使用できます。

* 型 `T` は入力コレクションの要素型を表します。 コレクション型によって定義されるすべてのメソッドは、コレクション型がクエリ可能である必要があります。

* 型 `S` は、結合を実行するクエリ演算子の場合の2番目の入力コレクションの要素の型を表します。

* 型 `K` は、キーとして機能する一連の範囲変数を持つクエリ演算子の場合、キーの種類を表します。

* 型 `N` は、数値型として使用される型を表します (ただし、これはユーザー定義型であり、組み込みの数値型ではない場合もあります)。

* @No__t-0 型は、ブール式で使用できる型を表します。

* クエリ演算子によって結果コレクションが生成される場合、型 `R` は結果コレクションの要素型を表します。 `R` は、クエリ演算子の最後にスコープ内の範囲変数の数に依存します。 1つの範囲変数がスコープ内にある場合は、`R` がその範囲変数の型になります。 この例では、

  ```vb
  Dim custNames = From c In Customers
                  Select c.Name
  ```

  クエリの結果は、要素の型が `String` のコレクション型になります。 複数の範囲変数がスコープ内にある場合、`R` は、スコープ内のすべての範囲変数を `Key` フィールドとして含む匿名型になります。 次に例を示します。

  ```vb
  Dim custNames = From c In Customers, o In c.Orders 
                  Select Name = c.Name, ProductName = o.ProductName
  ```

  クエリの結果は、匿名型の要素型を持つコレクション型であり、型 `String` の `Name` という名前の読み取り専用プロパティと、型 `String` の `ProductName` という名前の読み取り専用プロパティが設定されています。

  クエリ式内では、範囲変数を含むように生成された匿名型は*透過的*であり、範囲変数は常に修飾なしで使用できることを意味します。 たとえば、前の例では、入力コレクションの要素の型が匿名型であったとしても、範囲変数 `c` および `o` は、`Select` クエリ演算子で修飾なしでアクセスできます。

* 型 `CX` はコレクション型を表します。必ずしも入力コレクション型ではなく、要素型が-1 @no__t 型であることを示します。

クエリ可能なコレクション型は、次のいずれかの条件を優先順に満たす必要があります。

* @No__t-0 に準拠したメソッドを定義する必要があります。

* 次のいずれかのメソッドが含まれている必要があります。

  ```vb
  Function AsEnumerable() As CT
  Function AsQueryable() As CT
  ```

  クエリ可能なコレクションを取得するために呼び出すことができます。 両方の方法が指定されている場合は、`AsQueryable` が `AsEnumerable` よりも優先されます。

* メソッドが必要です。

  ```vb
  Function Cast(Of T)() As CT
  ```

  クエリ可能なコレクションを生成するために、範囲変数の型を使用して呼び出すことができます。

コレクションの要素の型は、実際のメソッドの呼び出しとは独立して判断されるため、特定のメソッドの適用性を判断することはできません。 したがって、既知のメソッドに一致するインスタンスメソッドがある場合、コレクションの要素の型を決定すると、既知のメソッドに一致する拡張メソッドは無視されます。

クエリ演算子の変換は、クエリ演算子が式で使用されている順序で行われます。 コレクションオブジェクトは、すべてのクエリ演算子で必要なすべてのメソッドを実装する必要はありませんが、すべてのコレクションオブジェクトが少なくとも `Select` クエリ演算子をサポートする必要があります。 必要なメソッドが存在しない場合は、コンパイル時エラーが発生します。 よく知られているメソッド名をバインドする場合、非メソッドは、インターフェイスおよび拡張メソッドのバインディングでの多重継承のために無視されます。ただし、シャドウセマンティクスは引き続き適用されます。 以下に例を示します。

```vb
Class Q1
    Public Function [Select](selector As Func(Of Integer, Integer)) As Q1
    End Function
End Class

Class Q2
    Inherits Q1

    Public [Select] As Integer
End Class

Module Test
    Sub Main()
        Dim qs As New Q2()

        ' Error: Q2.Select still hides Q1.Select
        Dim zs = From q In qs Select q
    End Sub
End Module
```

### <a name="default-query-indexer"></a>既定のクエリインデクサー

要素型が 0 @no__t で、既定のプロパティを持たないすべてのクエリ可能なコレクション型は、次の一般的な形式の既定のプロパティを持つと見なされます。

```vb
Public ReadOnly Default Property Item(index As Integer) As T
    Get
        Return Me.ElementAtOrDefault(index)
    End Get
End Property
```

既定のプロパティは、既定のプロパティアクセスの構文を使用してのみ参照できます。既定のプロパティを名前で参照することはできません。 以下に例を示します。

```vb
Dim customers As IEnumerable(Of Customer) = ...
Dim customerThree = customers(2)

' Error, no such property
Dim customerFour = customers.Item(4)
```

コレクション型に @no__t 0 のメンバーがない場合は、コンパイル時エラーが発生します。

### <a name="from-query-operator"></a>From クエリ演算子

@No__t-0 クエリ演算子は、クエリ対象のコレクションの個々のメンバーを表すコレクション範囲変数を導入します。

```antlr
FromQueryOperator
    : LineTerminator? 'From' LineTerminator? CollectionRangeVariableDeclarationList
    ;
```

たとえば、クエリ式は次のようになります。

```vb
From c As Customer In Customers ...
```

はと同じであると考えることができます。

```vb
For Each c As Customer In Customers
        ...
Next c
```

@No__t 0 のクエリ演算子で複数のコレクション範囲変数が宣言されている場合、またはクエリ式の最初の `From` クエリ演算子ではない場合、新しいコレクション範囲変数は、既存の一連の範囲変数に*クロス結合*されます。 結果として、結合されたコレクション内のすべての要素のクロス積に対してクエリが評価されます。 たとえば、式は次のようになります。

```vb
From c In Customers _
From e In Employees _
...
```

次の場合と同じであると考えることができます。

```vb
For Each c In Customers
    For Each e In Employees
            ...
    Next e
Next c
```

とは、次とまったく同じです。

```vb
From c In Customers, e In Employees ...
```

前のクエリ演算子で導入された範囲変数は、後の `From` クエリ演算子で使用できます。 たとえば、次のクエリ式では、2番目の `From` クエリ演算子は、最初の範囲変数の値を参照します。

```vb
From c As Customer In Customers _
From o As Order In c.Orders _
Select c.Name, o
```

@No__t-0 クエリ演算子または複数の `From` クエリ演算子内の複数の範囲変数は、コレクション型に次のメソッドのいずれかまたは両方が含まれている場合にのみサポートされます。

```vb
Function SelectMany(selector As Func(Of T, CR)) As CR
Function SelectMany(selector As Func(Of T, CS), _
                          resultsSelector As Func(Of T, S, R)) As CR
```

コード

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs, y In ys ...
```

通常はに変換されます。

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = _
    xs.SelectMany( _
        Function(x As Integer) ys, _
        Function(x As Integer, y As Integer) New With {x, y})...
```

__付箋.__ `From` は予約語ではありません。


### <a name="join-query-operator"></a>Join クエリ演算子

@No__t-0 クエリ演算子は、既存の範囲変数を新しいコレクション範囲変数と結合し、等値式に基づいて要素が結合された単一のコレクションを生成します。

```antlr
JoinQueryOperator
    : LineTerminator? 'Join' LineTerminator? CollectionRangeVariableDeclaration
      JoinOrGroupJoinQueryOperator? LineTerminator? 'On' LineTerminator? JoinConditionList
    ;

JoinConditionList
    : JoinCondition ( 'And' LineTerminator? JoinCondition )*
    ;

JoinCondition
    : Expression 'Equals' LineTerminator? Expression
    ;
```

以下に例を示します。

```vb
Dim customersAndOrders = _
    From cust In Customers _
    Join ord In Orders On cust.ID Equals ord.CustomerID
```

等値式は、通常の等値式よりも制限されています。

* 両方の式が値として分類される必要があります。

* 両方の式が少なくとも1つの範囲変数を参照している必要があります。

* Join クエリ演算子で宣言された範囲変数は、いずれかの式で参照されている必要があり、その式は他の範囲変数を参照することはできません。

2つの式の型がまったく同じ型でない場合は、

* 等値演算子が2つの型に対して定義されている場合、両方の式が暗黙的に変換可能であり、@no__t 0 ではありません。その後、両方の式をその型に変換します。

* それ以外の場合、両方の式を暗黙的にに変換できる、優先される型がある場合は、両方の式をその型に変換します。

* それ以外の場合は、コンパイル時のエラーが発生します。

式は、効率的に等値演算子を使用するのではなく、ハッシュ値 (つまり `GetHashCode()` を呼び出すことによって) を使用して比較されます。 @No__t 0 クエリ演算子は、同じ演算子で複数の結合または等値条件を実行できます。 @No__t-0 クエリ演算子は、コレクション型にメソッドが含まれている場合にのみサポートされます。

```vb
Function Join(inner As CS, _
                  outerSelector As Func(Of T, K), _
                  innerSelector As Func(Of S, K), _
                  resultSelector As Func(Of T, S, R)) As CR
```

コード

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs _
            Join y In ys On x Equals y _
            ...
```

通常はに変換されます。

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = _
    xs.Join( _
        ys, _
        Function(x As Integer) x, _
        Function(y As Integer) y, _
        Function(x As Integer, y As Integer) New With {x, y})...
```

__付箋.__ `Join`、`On`、`Equals` は予約語ではありません。


### <a name="let-query-operator"></a>Let クエリ演算子

@No__t-0 クエリ演算子は、式の範囲変数を導入します。 これにより、後のクエリ演算子で複数回使用される中間値を1回計算できます。

```antlr
LetQueryOperator
    : LineTerminator? 'Let' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

以下に例を示します。

```vb
Dim taxedPrices = _
    From o In Orders _
    Let tax = o.Price * 0.088 _
    Where tax > 3.50 _
    Select o.Price, tax, total = o.Price + tax
```

次の場合と同じであると考えることができます。

```vb
For Each o In Orders
    Dim tax = o.Price * 0.088
    ...
Next o
```

@No__t-0 クエリ演算子は、コレクション型にメソッドが含まれている場合にのみサポートされます。

```vb
Function Select(selector As Func(Of T, R)) As CR
```

コード

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Let y = x * 10 _
            ...
```

通常はに変換されます。

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Select(Function(x As Integer) New With {x, .y = x * 10})...
```


### <a name="select-query-operator"></a>Select クエリ演算子

@No__t-0 クエリ演算子は、式の範囲変数を導入するというの `Let` クエリ演算子に似ています。ただし、`Select` クエリ演算子は、現在使用できる範囲変数を追加するのではなく、非表示にします。 また、`Select` クエリ演算子によって導入される式範囲変数の型は、常にローカル変数型の推論規則を使用して推論されます。明示的な型を指定することはできません。また、推論できる型がない場合は、コンパイル時エラーが発生します。

```antlr
SelectQueryOperator
    : LineTerminator? 'Select' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

たとえば、次のクエリについて考えます。

```vb
Dim smiths = _
    From cust In Customers _
    Select name = cust.name _
    Where name.EndsWith("Smith")
```

`Where` クエリ演算子は、`Select` 演算子によって導入された `name` 範囲変数にのみアクセスできます。`Where` オペレーターが `cust` を参照しようとした場合、コンパイル時エラーが発生した可能性があります。

範囲変数の名前を明示的に指定する代わりに、`Select` クエリ演算子は、匿名型オブジェクト作成式と同じ規則を使用して、範囲変数の名前を推論できます。 以下に例を示します。

```vb
Dim custAndOrderNames = _
      From cust In Customers, ord In cust.Orders _
      Select cust.name, ord.ProductName _
        Where name.EndsWith("Smith")
```

範囲変数の名前が指定されておらず、名前が推論できない場合は、コンパイル時エラーが発生します。 @No__t-0 クエリ演算子に1つの式だけが含まれている場合、その範囲変数の名前を推論できないが、範囲変数には名前が付いていない場合、エラーは発生しません。  以下に例を示します。

```vb
Dim custAndOrderNames = _
      From cust In Customers, ord In cust.Orders _
      Select cust.Name & " bought " & ord.ProductName _
        Take 10
```

範囲変数と等値式に名前を割り当てる間に、`Select` クエリ演算子にあいまいさがある場合は、名前の割り当てが優先されます。 以下に例を示します。

```vb
Dim badCustNames = _
      From c In Customers _
        Let name = "John Smith" _
      Select name = c.Name ' Creates a range variable named "name"


Dim goodCustNames = _
      From c In Customers _
        Let name = "John Smith" _
      Select match = (name = c.Name)
```

@No__t-0 クエリ演算子の各式は、値として分類される必要があります。 @No__t-0 クエリ演算子は、コレクション型にメソッドが含まれている場合にのみサポートされます。

```vb
Function Select(selector As Func(Of T, R)) As CR
```

コード

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Select x, y = x * 10 _
            ...
```

通常はに変換されます。

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Select(Function(x As Integer) New With {x, .y = x * 10})...
```


### <a name="distinct-query-operator"></a>Distinct クエリ演算子

@No__t-0 クエリ演算子は、要素の型が等しいかどうかを比較することによって、コレクション内の値を個別の値を持つ値のみに制限します。

```antlr
DistinctQueryOperator
    : LineTerminator? 'Distinct' LineTerminator?
    ;
```

たとえば、クエリは次のようになります。

```vb
Dim distinctCustomerPrice = _
    From cust In Customers, ord In cust.Orders _
    Select cust.Name, ord.Price _
    Distinct
```

は、顧客名と注文価格の個別の組み合わせごとに1つの行のみを返します。これは、顧客が同じ価格を持つ複数の注文を持っている場合でも同様です。 @No__t-0 クエリ演算子は、コレクション型にメソッドが含まれている場合にのみサポートされます。

```vb
Function Distinct() As CT
```

コード

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Distinct _
            ...
```

通常はに変換されます。

```vb
Dim xs() As Integer = ...
Dim zs = xs.Distinct()...
```

__付箋.__ `Distinct` は予約語ではありません。


### <a name="where-query-operator"></a>Where クエリ演算子

@No__t-0 クエリ演算子は、コレクション内の値を、特定の条件を満たす値に制限します。

```antlr
WhereQueryOperator
    : LineTerminator? 'Where' LineTerminator? BooleanExpression
    ;
```

@No__t-0 クエリ演算子は、範囲変数値のセットごとに評価されるブール式を受け取ります。式の値が true の場合、値は出力コレクションに表示されます。それ以外の場合は、値がスキップされます。 たとえば、クエリ式は次のようになります。

```vb
From cust In Customers, ord In Orders _
Where cust.ID = ord.CustomerID _
...
```

は、入れ子になったループと同じであると考えることができます。

```vb
For Each cust In Customers
    For Each ord In Orders
            If cust.ID = ord.CustomerID Then
                ...
            End If
    Next ord
Next cust
```

@No__t-0 クエリ演算子は、コレクション型にメソッドが含まれている場合にのみサポートされます。

```vb
Function Where(predicate As Func(Of T, B)) As CT
```

コード

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Where x < 10 _
            ...
```

通常はに変換されます。

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Where(Function(x As Integer) x < 10)...
```

__付箋.__ `Where` は予約語ではありません。


### <a name="partition-query-operators"></a>パーティションクエリ演算子

```antlr
PartitionQueryOperator
    : LineTerminator? 'Take' LineTerminator? Expression
    | LineTerminator? 'Take' 'While' LineTerminator? BooleanExpression
    | LineTerminator? 'Skip' LineTerminator? Expression
    | LineTerminator? 'Skip' 'While' LineTerminator? BooleanExpression
    ;
```

@No__t-0 クエリ演算子は、コレクションの最初の @no__t 要素になります。 @No__t-0 修飾子と共に使用すると、`Take` 演算子は、ブール式を満たすコレクションの最初の @no__t 2 要素になります。 @No__t-0 演算子は、コレクションの最初の @no__t 要素をスキップし、コレクションの残りの部分を返します。  @No__t-0 修飾子と組み合わせて使用すると、`Skip` 演算子は、ブール式を満たすコレクションの最初の `n` 要素をスキップし、コレクションの残りの部分を返します。 @No__t-0 または `Skip` クエリ演算子内の式は、値として分類される必要があります。

@No__t-0 クエリ演算子は、コレクション型にメソッドが含まれている場合にのみサポートされます。

```vb
Function Take(count As N) As CT
```

@No__t-0 クエリ演算子は、コレクション型にメソッドが含まれている場合にのみサポートされます。

```vb
Function Skip(count As N) As CT
```

@No__t-0 クエリ演算子は、コレクション型にメソッドが含まれている場合にのみサポートされます。

```vb
Function TakeWhile(predicate As Func(Of T, B)) As CT
```

@No__t-0 クエリ演算子は、コレクション型にメソッドが含まれている場合にのみサポートされます。

```vb
Function SkipWhile(predicate As Func(Of T, B)) As CT
```

コード

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Skip 10 _
            Take 5 _
            Skip While x < 10 _
            Take While x > 5 _
            ...
```

通常はに変換されます。

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Skip(10). _
        Take(5). _
        SkipWhile(Function(x) x < 10). _
        TakeWhile(Function(x) x > 5)...
```

__付箋.__ `Take` および `Skip` は予約語ではありません。


### <a name="order-by-query-operator"></a>Order By クエリ演算子

@No__t-0 クエリ演算子は、範囲変数に表示される値を並べ替えます。 

```antlr
OrderByQueryOperator
    : LineTerminator? 'Order' 'By' LineTerminator? OrderExpressionList
    ;

OrderExpressionList
    : OrderExpression ( Comma OrderExpression )*
    ;

OrderExpression
    : Expression Ordering?
    ;

Ordering
    : 'Ascending' | 'Descending'
    ;
```

@No__t 0 クエリ演算子は、反復変数の順序付けに使用するキー値を指定する式を受け取ります。 たとえば、次のクエリでは、価格で並べ替えられた製品が返されます。

```vb
Dim productsByPrice = _
    From p In Products _
    Order By p.Price _
    Select p.Name
```

順序付けは `Ascending` としてマークできます。この場合、小さい値の方が大きい値の前になるか、または `Descending` になります。この場合、大きな値は小さい方の値よりも前になります。 順序が指定されていない場合の既定値は `Ascending` です。 たとえば、次のクエリでは、最もコストの高い製品を最初に価格で並べ替えた製品が返されます。

```vb
Dim productsByPriceDesc = _
    From p In Products _
    Order By p.Price Descending _
    Select p.Name
```

@No__t-0 クエリ演算子では、順序付けのために複数の式を指定できます。この場合、コレクションは入れ子になった方法で並べ替えられます。 たとえば、次のクエリでは、州別に顧客を注文した後、州ごとに市区町村別に、各都市内の郵便番号によって顧客を並べ替えます。

```vb
Dim customersByLocation = _
    From c In Customers _
    Order By c.State, c.City, c.ZIP _
    Select c.Name, c.State, c.City, c.ZIP
```

@No__t-0 クエリ演算子内の式は、値として分類される必要があります。 @No__t-0 クエリ演算子は、コレクション型に次のメソッドのいずれかまたは両方が含まれている場合にのみサポートされます。

```vb
Function OrderBy(keySelector As Func(Of T, K)) As CT
Function OrderByDescending(keySelector As Func(Of T, K)) As CT
```

戻り値の型 `CT` は*順序付け*られたコレクションである必要があります。 順序付けられたコレクションは、次のいずれかまたは両方のメソッドを含むコレクション型です。

```vb
Function ThenBy(keySelector As Func(Of T, K)) As CT
Function ThenByDescending(keySelector As Func(Of T, K)) As CT
```

コード

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Order By x Ascending, x Mod 2 Descending _
            ...
```

通常はに変換されます。

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.OrderBy(Function(x) x).ThenByDescending(Function(x) x Mod 2)...
```

__付箋.__ クエリ演算子は、特定のクエリ操作を実装するメソッドに構文をマップするだけなので、順序の保持は言語によって決まりません。また、演算子自体の実装によって決定されます。 これはユーザー定義の演算子とよく似ています。これは、ユーザー定義の数値型に対して加算演算子をオーバーロードする実装によって、加算に似た処理が行われない場合があるという点です。 もちろん、予測可能性を維持するために、ユーザーの期待に一致しないものを実装することは推奨されません。

__付箋.__ `Order` および `By` は予約語ではありません。


### <a name="group-by-query-operator"></a>Group By クエリ演算子

@No__t-0 クエリ演算子は、1つまたは複数の式に基づいてスコープ内の範囲変数をグループ化し、それらのグループに基づいて新しい範囲変数を生成します。

```antlr
GroupByQueryOperator
    : LineTerminator? 'Group' ( LineTerminator? ExpressionRangeVariableDeclarationList )?
      LineTerminator? 'By' LineTerminator? ExpressionRangeVariableDeclarationList
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

たとえば、次のクエリでは、すべての顧客を `State` でグループ化し、各グループのカウントと平均経過時間を計算します。

```vb
Dim averageAges = _
    From cust In Customers _
    Group By cust.State _
    Into Count(), Average(cust.Age)
```

@No__t-0 クエリ演算子には、省略可能な `Group` 句、`By` 句、および `Into` 句の3つの句があります。 @No__t-0 句は、`Select` クエリ演算子と同じ構文と効果がありますが、`By` 句ではなく `Into` 句で使用できる範囲変数にのみ影響します。 以下に例を示します。

```vb
Dim averageAges = _
    From cust In Customers _
    Group cust.Age By cust.State _
    Into Count(), Average(Age)
```

@No__t-0 句は、グループ化操作でキー値として使用される式の範囲変数を宣言します。 @No__t-0 句を使用すると、`By` 句で形成される各グループに対して集計を計算する式範囲変数を宣言できます。 @No__t-0 句内では、式の範囲変数には、*集計関数*のメソッド呼び出しである式のみを割り当てることができます。 集計関数は、次のいずれかのメソッドのような、グループのコレクション型に対する関数です (元のコレクションと同じコレクション型であるとは限りません)。

```vb
Function _name_() As _type_
Function _name_(selector As Func(Of T, R)) As R
```

集計関数がデリゲート引数を受け取る場合、呼び出し式は、値として分類される必要がある引数式を持つことができます。  引数式では、スコープ内の範囲変数を使用できます。集計関数の呼び出し内では、これらの範囲変数は、コレクション内のすべての値ではなく、形成されているグループ内の値を表します。 たとえば、このセクションの元の例では、`Average` 関数は、すべての顧客に対してではなく、顧客の年齢ごとの平均を計算します。

すべてのコレクション型には、集計関数が定義されていると見なされます。この関数は、パラメーターを取らず、単にグループを返します。 @no__t。 コレクション型によって提供されるその他の標準の集計関数は次のとおりです。

`Count` および `LongCount` で、グループ内の要素の数またはブール式を満たすグループ内の要素の数を返します。 `Count` および `LongCount` は、コレクション型にメソッドが1つ含まれている場合にのみサポートされます。

```vb
Function Count() As N
Function Count(selector As Func(Of T, B)) As N
Function LongCount() As N
Function LongCount(selector As Func(Of T, B)) As N
```

`Sum`。グループ内のすべての要素にわたる式の合計を返します。 `Sum` がサポートされるのは、コレクション型にメソッドが1つ含まれている場合のみです。

```vb
Function Sum() As N
Function Sum(selector As Func(Of T, N)) As N
```

`Min` を指定すると、グループ内のすべての要素にわたる式の最小値が返されます。 `Min` がサポートされるのは、コレクション型にメソッドが1つ含まれている場合のみです。

```vb
Function Min() As N
Function Min(selector As Func(Of T, N)) As N
```

`Max`。グループ内のすべての要素にわたる式の最大値を返します。 `Max` がサポートされるのは、コレクション型にメソッドが1つ含まれている場合のみです。

```vb
Function Max() As N
Function Max(selector As Func(Of T, N)) As N
```

`Average`。グループ内のすべての要素にわたる式の平均を返します。 `Average` がサポートされるのは、コレクション型にメソッドが1つ含まれている場合のみです。

```vb
Function Average() As N
Function Average(selector As Func(Of T, N)) As N
```

`Any`。グループにメンバーが含まれているかどうか、またはグループ内の任意の要素に対してブール式が true であるかどうかを判断します。 `Any` は、ブール式で使用できる値を返します。この値は、コレクション型にメソッドが1つ含まれている場合にのみサポートされます。

```vb
Function Any() As B
Function Any(predicate As Func(Of T, B)) As B
```

`All`。グループ内のすべての要素に対してブール式が true かどうかを決定します。 `All` は、ブール式で使用できる値を返します。この値は、コレクション型にメソッドが含まれている場合にのみサポートされます。

```vb
Function All(predicate As Func(Of T, B)) As B
```

@No__t 0 のクエリ演算子の後に、スコープ内の以前の範囲変数は非表示になり、`By` および `Into` の各句によって導入された範囲変数が使用可能になります。 @No__t-0 クエリ演算子は、コレクション型にメソッドが含まれている場合にのみサポートされます。

```vb
Function GroupBy(keySelector As Func(Of T, K), _
                      resultSelector As Func(Of K, CT, R)) As CR
```

@No__t-0 句の範囲変数宣言は、コレクション型にメソッドが含まれている場合にのみサポートされます。

```vb
Function GroupBy(keySelector As Func(Of T, K), _
                      elementSelector As Func(Of T, S), _
                      resultSelector As Func(Of K, CS, R)) As CR
```

コード

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Group y = x * 10, z = x / 10 By evenOdd = x Mod 2 _
            Into Sum(y), Average(z) _
            ...
```

通常はに変換されます。

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.GroupBy( _
        Function(x As Integer) x Mod 2, _
        Function(x As Integer) New With {.y = x * 10, .z = x / 10}, _
        Function(evenOdd, group) New With { _
            evenOdd, _
            .Sum = group.Sum(Function(e) e.y), _
            .Average = group.Average(Function(e) e.z)})...
```

__付箋.__ `Group`、`By`、`Into` は予約語ではありません。


### <a name="aggregate-query-operator"></a>集計クエリ演算子

@No__t-0 クエリ演算子は、`Group By` 演算子と同様の関数を実行します。ただし、既に形成されているグループを集計することはできます。 グループは既に形成されているため、`Aggregate` クエリ演算子の `Into` 句はスコープ内の範囲変数を非表示にしません (この方法では、`Aggregate` は `Let` のようになり、@no__t 4 は `Select` に似ています)。

```antlr
AggregateQueryOperator
    : LineTerminator? 'Aggregate' LineTerminator? CollectionRangeVariableDeclaration QueryOperator*
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

たとえば、次のクエリは、ワシントン州の顧客によって行われたすべての注文の合計を集計します。

```vb
Dim orderTotals = _
    From cust In Customers _
    Where cust.State = "WA" _
    Aggregate order In cust.Orders _
    Into Sum(order.Total)
```

このクエリの結果は、`Customer` として型指定された `cust` という名前のプロパティと、`Integer` として型指定された `Sum` という名前のプロパティを持つ匿名型を持つコレクションです。

@No__t-0 とは異なり、`Aggregate` 句と `Into` 句の間に追加のクエリ演算子を配置できます。 @No__t-0 句と `Into` 句の末尾の間に、スコープ内のすべての範囲変数を指定します。これには、`Aggregate` 句で宣言された変数も使用できます。 たとえば、次のクエリでは、2006より前に、ワシントン州の顧客によって行われたすべての注文の合計を集計しています。

```vb
Dim orderTotals = _
    From cust In Customers _
    Where cust.State = "WA" _
    Aggregate order In cust.Orders _
    Where order.Date <= #01/01/2006# _
    Into Sum = Sum(order.Total)
```

@No__t-0 演算子を使用してクエリ式を開始することもできます。 この場合、クエリ式の結果は、`Into` 句によって計算された単一の値になります。 たとえば、次のクエリでは、2006年1月1日より前のすべての注文合計の合計が計算されます。

```vb
Dim ordersTotal = _
    Aggregate order In Orders _
    Where order.Date <= #01/01/2006# _
    Into Sum(order.Total)
```

クエリの結果は、単一の @no__t 0 の値になります。 @No__t-0 クエリ演算子は常に使用できます (ただし、式を有効にするには集計関数も使用できる必要があります)。 コード

```vb
Dim xs() As Integer = ...
Dim zs = _
    Aggregate x In xs _
    Where x < 5 _
    Into Sum()
```

通常はに変換されます。

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Where(Function(x) x < 5).Sum()
```

__メモ。__  `Aggregate` と `Into` は予約語ではありません。


### <a name="group-join-query-operator"></a>Group Join クエリ演算子

@No__t-0 クエリ演算子は、`Join` および `Group By` クエリ演算子の関数を1つの演算子に結合します。 `Group Join` は、要素から抽出された一致するキーに基づいて2つのコレクションを結合し、結合の左側の特定の要素に一致する結合の右側にあるすべての要素をグループ化します。 したがって、演算子は一連の階層的な結果を生成します。

```antlr
GroupJoinQueryOperator
    : LineTerminator? 'Group' 'Join' LineTerminator? CollectionRangeVariableDeclaration
      JoinOrGroupJoinQueryOperator? LineTerminator? 'On' LineTerminator? JoinConditionList
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

たとえば、次のクエリでは、1人の顧客の名前、すべての注文のグループ、およびそれらすべての注文の合計金額を含む要素が生成されます。

```vb
Dim custsWithOrders = _
    From cust In Customers _
    Group Join order In Orders On cust.ID Equals order.CustomerID _ 
    Into Orders = Group, OrdersTotal = Sum(order.Total) _
    Select cust.Name, Orders, OrdersTotal
```

このクエリの結果は、3つのプロパティを持つ匿名型である要素型を持つコレクションです。 @no__t 0 の場合は、`String` として型指定され、`Orders` は要素の型が @no__t 3 で、@no__t が `OrdersTotal` であるコレクションとして型指定されます。 @No__t-0 クエリ演算子は、コレクション型にメソッドが含まれている場合にのみサポートされます。

```vb
Function GroupJoin(inner As CS, _
                         outerSelector As Func(Of T, K), _
                         innerSelector As Func(Of S, K), _
                         resultSelector As Func(Of T, CS, R)) As CR
```

コード

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs _
            Group Join y in ys On x Equals y _
            Into g = Group _
            ...
```

通常はに変換されます。

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = _
    xs.GroupJoin( _
        ys, _
        Function(x As Integer) x, _
        Function(y As Integer) y, _
        Function(x, group) New With {x, .g = group})...
```

__付箋.__ `Group`、`Join`、`Into` は予約語ではありません。


## <a name="conditional-expressions"></a>条件式

条件付き `If` 式は、式をテストして値を返します。

```antlr
ConditionalExpression
    : 'If' OpenParenthesis BooleanExpression Comma Expression Comma Expression CloseParenthesis
    | 'If' OpenParenthesis Expression Comma Expression CloseParenthesis
    ;
```

ただし、`IIF` ランタイム関数とは異なり、条件式は必要に応じてそのオペランドだけを評価します。 したがって、たとえば、`c` の値が `Nothing` の場合、式 `If(c Is Nothing, c.Name, "Unknown")` は例外をスローしません。 条件式には2つの形式があります。1つは2つのオペランドを受け取り、もう1つは3つのオペランドを受け取ります。

3つのオペランドが指定されている場合、3つの式はすべて値として分類する必要があり、最初のオペランドはブール式である必要があります。 式の結果が true の場合、2番目の式は演算子の結果になります。それ以外の場合は、3番目の式が演算子の結果になります。 式の結果型は、2番目と3番目の式の型の間で最も優先度の高い型になります。 優先度の高い型がない場合は、コンパイル時エラーが発生します。

2つのオペランドを指定する場合は、両方のオペランドが値として分類され、1番目のオペランドが参照型または null 許容値型である必要があります。 式 `If(x, y)` は、次の2つの例外を除き、式が-1 @no__t のように評価されます。 最初の式は1回だけ評価されます。2番目のオペランドの型が null 非許容の値型で、最初のオペランドの型がの場合は、結果の優先度の高い型を決定するときに、最初のオペランドの型から `?` が削除されます。式の型。 以下に例を示します。

```vb
Module Test
    Sub Main()
        Dim x?, y As Integer
        Dim a?, b As Long

        a = If(x, a)        ' Result type: Long?
        y = If(x, 0)        ' Result type: Integer
    End Sub
End Module
```

式の両方の形式で、オペランドが 0 @no__t 場合は、その型を使用して、優先される型を決定しません。 式 `If(<expression>, Nothing, Nothing)` の場合、優先される型は `Object` と見なされます。


## <a name="xml-literal-expressions"></a>XML リテラル式

XML リテラル式は、XML (拡張マークアップ言語) 1.0 値を表します。

```antlr
XMLLiteralExpression
    : XMLDocument
    | XMLElement
    | XMLProcessingInstruction
    | XMLComment
    | XMLCDATASection
    ;
```

XML リテラル式の結果は、`System.Xml.Linq` 名前空間のいずれかの型として型指定された値になります。 その名前空間の型が使用できない場合は、XML リテラル式によってコンパイル時エラーが発生します。 値は、XML リテラル式から変換されたコンストラクター呼び出しによって生成されます。 たとえば、コードは次のようになります。

```vb
Dim book As System.Xml.Linq.XElement = _
    <book title="My book"></book>
```

は、次のコードとほぼ同じです。

```vb
Dim book As System.Xml.Linq.XElement = _
    New System.Xml.Linq.XElement( _
        "book", _
        New System.Xml.Linq.XAttribute("title", "My book"))
```

Xml リテラル式は、xml ドキュメント、XML 要素、XML 処理命令、XML コメント、または CDATA セクションの形式を取ることができます。

__付箋.__ この仕様には、Visual Basic 言語の動作を説明するのに十分な XML 記述が含まれています。 XML の詳細については、@no__t 0 を参照してください。


### <a name="lexical-rules"></a>字句規則

```antlr
XMLCharacter
    : '<Unicode tab character (0x0009)>'
    | '<Unicode linefeed character (0x000A)>'
    | '<Unicode carriage return character (0x000D)>'
    | '<Unicode characters 0x0020 - 0xD7FF>'
    | '<Unicode characters 0xE000 - 0xFFFD>'
    | '<Unicode characters 0x10000 - 0x10FFFF>'
    ;

XMLString
    : XMLCharacter+
    ;

XMLWhitespace
    : XMLWhitespaceCharacter+
    ;

XMLWhitespaceCharacter
    : '<Unicode carriage return character (0x000D)>'
    | '<Unicode linefeed character (0x000A)>'
    | '<Unicode space character (0x0020)>'
    | '<Unicode tab character (0x0009)>'
    ;

XMLNameCharacter
    : XMLLetter
    | XMLDigit
    | '.'
    | '-'
    | '_'
    | ':'
    | XMLCombiningCharacter
    | XMLExtender
    ;

XMLNameStartCharacter
    : XMLLetter
    | '_'
    | ':'
    ;

XMLName
    : XMLNameStartCharacter XMLNameCharacter*
    ;

XMLLetter
    : '<Unicode character as defined in the Letter production of the XML 1.0 specification>'
    ;

XMLDigit
    : '<Unicode character as defined in the Digit production of the XML 1.0 specification>'
    ;

XMLCombiningCharacter
    : '<Unicode character as defined in the CombiningChar production of the XML 1.0 specification>'
    ;

XMLExtender
    : '<Unicode character as defined in the Extender production of the XML 1.0 specification>'
    ;
```

XML リテラル式は、通常の Visual Basic コードの字句規則ではなく、XML の字句規則を使用して解釈されます。 2つの規則セットは、一般に次の点で異なります。

* XML では、空白文字が重要です。 その結果、XML リテラル式の文法では、空白が許可されている場所が明示的に示されます。 要素内の文字データのコンテキストで発生する場合を除き、空白は保持されません。 以下に例を示します。

  ```vb
  ' The following element preserves no whitespace
  Dim e1 = _
      <customer>
          <name>Bob</>
      </>

  ' The following element preserves all of the whitespace
  Dim e2 = _
      <customer>
          Bob
      </>
  ```

* XML の行末の空白文字は、XML 仕様に従って正規化されます。

* XML は大文字と小文字を区別します。 キーワードは大文字と小文字を正確に一致させる必要があります。そうしないと、コンパイル時エラーが発生します。

* 行ターミネータは、XML の空白文字と見なされます。 そのため、XML リテラル式には行連結文字は必要ありません。

* XML では、全角文字は使用できません。 全角文字が使用されている場合は、コンパイル時エラーが発生します。


### <a name="embedded-expressions"></a>埋め込み式

XML リテラル式には、*埋め込み式*を含めることができます。 埋め込み式は Visual Basic 式で、評価され、埋め込み式の場所に1つ以上の値を入力するために使用されます。

```antlr
XMLEmbeddedExpression
    : '<' '%' '=' LineTerminator? Expression LineTerminator? '%' '>'
    ;
```

たとえば、次のコードでは、XML 要素の値として文字列 `John Smith` に配置します。

```vb
Dim name as String = "John Smith"
Dim element As System.Xml.Linq.XElement = <customer><%= name %></customer>
```

式は、さまざまなコンテキストに埋め込むことができます。 たとえば、次のコードでは `customer` という名前の要素が生成されます。

```vb
Dim name As String = "customer"
Dim element As System.Xml.Linq.XElement = <<%= name %>>John Smith</>
```

埋め込み式を使用できる各コンテキストは、受け入れられる型を指定します。 埋め込み式の式部分のコンテキスト内では、Visual Basic コードの通常の構文規則が引き続き適用されます。たとえば、行の継続を使用する必要があります。

```vb
' Visual Basic expression uses line continuation, XML does not
Dim element As System.Xml.Linq.XElement = _
    <<%= name & _
          name %>>John 
                     Smith</>
```


### <a name="xml-documents"></a>XML ドキュメント

```antlr
XMLDocument
    : XMLDocumentPrologue XMLMisc* XMLDocumentBody XMLMisc*
    ;

XMLDocumentPrologue
    : '<' '?' 'xml' XMLVersion XMLEncoding? XMLStandalone? XMLWhitespace? '?' '>'
    ;

XMLVersion
    : XMLWhitespace 'version' XMLWhitespace? '=' XMLWhitespace? XMLVersionNumberValue
    ;

XMLVersionNumberValue
    : SingleQuoteCharacter '1' '.' '0' SingleQuoteCharacter
    | DoubleQuoteCharacter '1' '.' '0' DoubleQuoteCharacter
    ;

XMLEncoding
    : XMLWhitespace 'encoding' XMLWhitespace? '=' XMLWhitespace? XMLEncodingNameValue
    ;

XMLEncodingNameValue
    : SingleQuoteCharacter XMLEncodingName SingleQuoteCharacter
    | DoubleQuoteCharacter XMLEncodingName DoubleQuoteCharacter
    ;

XMLEncodingName
    : XMLLatinAlphaCharacter XMLEncodingNameCharacter*
    ;

XMLEncodingNameCharacter
    : XMLUnderscoreCharacter
    | XMLLatinAlphaCharacter
    | XMLNumericCharacter
    | XMLPeriodCharacter
    | XMLDashCharacter
    ;

XMLLatinAlphaCharacter
    : '<Unicode Latin alphabetic character (0x0041-0x005a, 0x0061-0x007a)>'
    ;

XMLNumericCharacter
    : '<Unicode digit character (0x0030-0x0039)>'
    ;

XMLHexNumericCharacter
    : XMLNumericCharacter
    | '<Unicode Latin hex alphabetic character (0x0041-0x0046, 0x0061-0x0066)>'
    ;

XMLPeriodCharacter
    : '<Unicode period character (0x002e)>'
    ;

XMLUnderscoreCharacter
    : '<Unicode underscore character (0x005f)>'
    ;

XMLDashCharacter
    : '<Unicode dash character (0x002d)>'
    ;

XMLStandalone
    : XMLWhitespace 'standalone' XMLWhitespace? '=' XMLWhitespace? XMLYesNoValue
    ;

XMLYesNoValue
    : SingleQuoteCharacter XMLYesNo SingleQuoteCharacter
    | DoubleQuoteCharacter XMLYesNo DoubleQuoteCharacter
    ;

XMLYesNo
    : 'yes'
    | 'no'
    ;

XMLMisc
    : XMLComment
    | XMLProcessingInstruction
    | XMLWhitespace
    ;

XMLDocumentBody
    : XMLElement
    | XMLEmbeddedExpression
    ;
```

XML ドキュメントでは、`System.Xml.Linq.XDocument` として型指定された値が返されます。 Xml 1.0 仕様とは異なり、xml ドキュメントプロローグを指定するには xml リテラル式の XML ドキュメントが必要です。XML ドキュメントプロローグのない XML リテラル式は、個々のエンティティとして解釈されます。 以下に例を示します。

```vb
Dim doc As System.Xml.Linq.XDocument = _
    <?xml version="1.0"?>
    <?instruction?>
    <customer>Bob</>

Dim pi As System.Xml.Linq.XProcessingInstruction = _
    <?instruction?>
```

XML ドキュメントには、任意の型を持つことができる埋め込み式を含めることができます。ただし、実行時には、オブジェクトが `XDocument` コンストラクターの要件を満たしている必要があります。指定されていない場合は、実行時エラーが発生します。

通常の XML とは異なり、XML ドキュメント式では Dtd (ドキュメント型宣言) はサポートされません。 また、指定した場合、encoding 属性は無視されます。これは、Xml リテラル式のエンコーディングがソースファイル自体のエンコーディングと常に同じであるためです。

__付箋.__ Encoding 属性は無視されますが、有効な属性であるため、有効な Xml 1.0 ドキュメントをソースコードに含めることができます。


### <a name="xml-elements"></a>XML 要素

```antlr
XMLElement
    : XMLEmptyElement
    | XMLElementStart XMLContent XMLElementEnd
    ;

XMLEmptyElement
    : '<' XMLQualifiedNameOrExpression XMLAttribute* XMLWhitespace? '/' '>'
    ;

XMLElementStart
    : '<' XMLQualifiedNameOrExpression XMLAttribute* XMLWhitespace? '>'
    ;

XMLElementEnd
    : '<' '/' '>'
    | '<' '/' XMLQualifiedName XMLWhitespace? '>'
    ;

XMLContent
    : XMLCharacterData? ( XMLNestedContent XMLCharacterData? )+
    ;

XMLCharacterData
    : '<Any XMLCharacterDataString that does not contain the string "]]>">'
    ;

XMLCharacterDataString
    : '<Any Unicode character except < or &>'+
    ;

XMLNestedContent
    : XMLElement
    | XMLReference
    | XMLCDATASection
    | XMLProcessingInstruction
    | XMLComment
    | XMLEmbeddedExpression
    ;

XMLAttribute
    : XMLWhitespace XMLAttributeName XMLWhitespace? '=' XMLWhitespace? XMLAttributeValue
    | XMLWhitespace XMLEmbeddedExpression
    ;

XMLAttributeName
    : XMLQualifiedNameOrExpression
    | XMLNamespaceAttributeName
    ;

XMLAttributeValue
    : DoubleQuoteCharacter XMLAttributeDoubleQuoteValueCharacter* DoubleQuoteCharacter
    | SingleQuoteCharacter XMLAttributeSingleQuoteValueCharacter* SingleQuoteCharacter
    | XMLEmbeddedExpression
    ;

XMLAttributeDoubleQuoteValueCharacter
    : '<Any XMLCharacter except <, &, or DoubleQuoteCharacter>'
    | XMLReference
    ;

XMLAttributeSingleQuoteValueCharacter
    : '<Any XMLCharacter except <, &, or SingleQuoteCharacter>'
    | XMLReference
    ;

XMLReference
    : XMLEntityReference
    | XMLCharacterReference
    ;

XMLEntityReference
    : '&' XMLEntityName ';'
    ;

XMLEntityName
    : 'lt' | 'gt' | 'amp' | 'apos' | 'quot'
    ;

XMLCharacterReference
    : '&' '#' XMLNumericCharacter+ ';'
    | '&' '#' 'x' XMLHexNumericCharacter+ ';'
    ;
```

XML 要素は、`System.Xml.Linq.XElement` として型指定された値になります。 通常の XML とは異なり、XML 要素は終了タグ内の名前を省略し、現在最も入れ子になっている要素を閉じることができます。 以下に例を示します。

```vb
Dim name = <name>Bob</>
```

XML 要素の属性宣言は、`System.Xml.Linq.XAttribute` として型指定された値になります。 属性値は、XML 仕様に従って正規化されます。 属性の値が @no__t 0 の場合、属性は作成されないため、`Nothing` の属性値式をチェックする必要はありません。 以下に例を示します。

```vb
Dim expr = Nothing

' Throws null argument exception
Dim direct = New System.Xml.Linq.XElement( _
    "Name", _
    New System.Xml.Linq.XAttribute("Length", expr))

' Doesn't throw exception, the result is <Name/>
Dim literal = <Name Length=<%= expr %>/>
```

XML 要素と属性には、次の場所で入れ子になった式を含めることができます。

要素の名前。この場合、埋め込み式は `System.Xml.Linq.XName` に暗黙的に変換できる型の値である必要があります。 以下に例を示します。

```vb
Dim name = <<%= "name" %>>Bob</>
```

要素の属性の名前。この場合、埋め込み式は `System.Xml.Linq.XName` に暗黙的に変換できる型の値である必要があります。 以下に例を示します。

```vb
Dim name = <name <%= "length" %>="3">Bob</>
```

要素の属性の値。この場合、埋め込み式には任意の型の値を指定できます。 以下に例を示します。

```vb
Dim name = <name length=<%= 3 %>>Bob</>
```

要素の属性。この場合、埋め込み式には任意の型の値を指定できます。 以下に例を示します。

```vb
Dim name = <name <%= new XAttribute("length", 3) %>>Bob</>
```

要素の内容。この場合、埋め込み式には任意の型の値を指定できます。 以下に例を示します。

```vb
Dim name = <name><%= "Bob" %></>
```

埋め込み式の型が `Object()` の場合、配列は paramarray として `XElement` コンストラクターに渡されます。


### <a name="xml-namespaces"></a>XML 名前空間

Xml 要素には、xml 名前空間1.0 仕様で定義されている XML 名前空間宣言を含めることができます。

```antlr
XMLNamespaceAttributeName
    : XMLPrefixedNamespaceAttributeName
    | XMLDefaultNamespaceAttributeName
    ;

XMLPrefixedNamespaceAttributeName
    : 'xmlns' ':' XMLNamespaceName
    ;

XMLDefaultNamespaceAttributeName
    : 'xmlns'
    ;

XMLNamespaceName
    : XMLNamespaceNameStartCharacter XMLNamespaceNameCharacter*
    ;

XMLNamespaceNameStartCharacter
    : '<Any XMLNameCharacter except :>'
    ;

XMLNamespaceNameCharacter
    : XMLLetter
    | '_'
    ;

XMLQualifiedNameOrExpression
    : XMLQualifiedName
    | XMLEmbeddedExpression
    ;

XMLQualifiedName
    : XMLPrefixedName
    | XMLUnprefixedName
    ;

XMLPrefixedName
    : XMLNamespaceName ':' XMLNamespaceName
    ;

XMLUnprefixedName
    : XMLNamespaceName
    ;
```

@No__t-0 および `xmlns` という名前空間の定義に関する制限が適用され、コンパイル時エラーが発生します。 XML 名前空間の宣言では、その値に対して埋め込み式を使用することはできません。指定された値は、空でない文字列リテラルである必要があります。 以下に例を示します。

```vb
' Declares a valid namespace
Dim customer = <db:customer xmlns:db="http://example.org/database">Bob</>

' Error: xmlns cannot be re-defined
Dim bad1 = <elem xmlns:xmlns="http://example.org/namespace"/>

' Error: cannot have an embedded expression
Dim bad2 = <elem xmlns:db=<%= "http://example.org/database" %>>Bob</>
```

__付箋.__ この仕様には、Visual Basic 言語の動作を記述するのに十分な XML 名前空間の説明が含まれています。 XML 名前空間の詳細については、@no__t 0 を参照してください。

XML 要素名と属性名は、名前空間名を使用して修飾できます。 名前空間は、通常の XML のようにバインドされます。ただし、ファイルレベルで宣言された名前空間インポートは、その宣言を囲むコンテキストで宣言されていると見なされます。これは、コンパイルによって宣言された名前空間インポートによって囲まれています。environment. 名前空間の名前が見つからない場合は、コンパイル時エラーが発生します。 以下に例を示します。

```vb
Imports System.Xml.Linq
Imports <xmlns:db="http://example.org/database">

Module Test
    Sub Main()
        ' Binds to the imported namespace above.
        Dim c1 = <db:customer>Bob</>

        ' Binds to the namespace declaration in the element
        Dim c2 = _
            <db:customer xmlns:db="http://example.org/database-other">Mary</>

        ' Binds to the inner namespace declaration
        Dim c3 = _
            <database xmlns:db="http://example.org/database-one">
                <db:customer xmlns:db="http://example.org/database-two">Joe</>
            </>

        ' Error: namespace db2 cannot be found
        Dim c4 = _
            <db2:customer>Jim</>
    End Sub
End Module
```

要素で宣言された XML 名前空間は、埋め込み式内の XML リテラルには適用されません。 以下に例を示します。

```vb
' Error: Namespace prefix 'db' is not declared
Dim customer = _
    <db:customer xmlns:db="http://example.org/database">
        <%= <db:customer>Bob</> %>
    </>
```

__付箋.__ これは、埋め込み式が関数呼び出しを含む任意のものになる可能性があるためです。 関数呼び出しに XML リテラル式が含まれている場合は、XML 名前空間を適用するか無視するかをプログラマが想定するかどうかが明確ではありません。


### <a name="xml-processing-instructions"></a>XML 処理命令

XML 処理命令は、`System.Xml.Linq.XProcessingInstruction` として型指定された値になります。 XML 処理命令は、処理命令内の有効な構文であるため、埋め込み式を含むことはできません。

```antlr
XMLProcessingInstruction
    : '<' '?' XMLProcessingTarget ( XMLWhitespace XMLProcessingValue? )? '?' '>'
    ;

XMLProcessingTarget
    : '<Any XMLName except a casing permutation of the string "xml">'
    ;

XMLProcessingValue
    : '<Any XMLString that does not contain a question-mark followed by ">">'
    ;
```

### <a name="xml-comments"></a>XML コメント

XML コメントは、`System.Xml.Linq.XComment` として型指定された値になります。 XML コメントは、コメント内の有効な構文であるため、埋め込み式を含むことはできません。

```antlr
XMLComment
    : '<' '!' '-' '-' XMLCommentCharacter* '-' '-' '>'
    ;

XMLCommentCharacter
    : '<Any XMLCharacter except dash (0x002D)>'
    | '-' '<Any XMLCharacter except dash (0x002D)>'
    ;
```

### <a name="cdata-sections"></a>CDATA セクション

CDATA セクションは、`System.Xml.Linq.XCData` として型指定された値になります。 Cdata セクションには、CDATA セクション内の有効な構文であるため、埋め込み式を含めることはできません。

```antlr
XMLCDATASection
    : '<' '!' ( 'CDATA' '[' XMLCDATASectionString? ']' )? '>'
    ;

XMLCDATASectionString
    : '<Any XMLString that does not contain the string "]]>">'
    ;
```

## <a name="xml-member-access-expressions"></a>XML メンバーアクセス式

Xml メンバーアクセス式は、XML 値のメンバーにアクセスします。

```antlr
XMLMemberAccessExpression
    : Expression '.' LineTerminator? '<' XMLQualifiedName '>'
    | Expression '.' LineTerminator? '@' LineTerminator? '<' XMLQualifiedName '>'
    | Expression '.' LineTerminator? '@' LineTerminator? IdentifierOrKeyword
    | Expression '.' '.' '.' LineTerminator? '<' XMLQualifiedName '>'
    ;
```

XML メンバーアクセス式には、次の3種類があります。

* *要素アクセス*。 XML 名は1つのドットに従います。 以下に例を示します。

  ```vb
  Dim customer = _
      <customer>
          <name>Bob</>
      </>
  Dim customerName = customer.<name>.Value
  ```

  要素アクセスは、次の関数にマップされます。

  ```vb
  Function Elements(name As System.Xml.Linq.XName) As _
      System.Collections.Generic.IEnumerable(Of _
          System.Xml.Linq.XNode)
  ```

  上の例は次のようになります。

  ```vb
  Dim customerName = customer.Elements("name").Value
  ```

* *属性アクセス*。 Visual Basic 識別子がドットとアットマークの後に続く場合、または XML 名がドットとアットマークの後に続く場合。 以下に例を示します。

  ```vb
  Dim customer = <customer age="30"/>
  Dim customerAge = customer.@age
  ```

  属性へのアクセスは、次の関数にマップされます。

  ```vb
  Function AttributeValue(name As System.Xml.Linq.XName) as String
  ```

  上の例は次のようになります。

  ```vb
  Dim customerAge = customer.AttributeValue("age")
  ```

  __付箋.__ @No__t-0 拡張メソッド (および関連する拡張機能プロパティ `Value`) は、現在、どのアセンブリでも定義されていません。 拡張機能のメンバーが必要な場合は、生成されるアセンブリに自動的に定義されます。

* *子孫アクセス*。 XML 名が3つのドットの後に続きます。 以下に例を示します。

  ```vb
  Dim company = _
      <company>
          <customers>
              <customer>Bob</>
              <customer>Mary</>
              <customer>Joe</>
          </>
      </>
  Dim customers = company...<customer>
  ```

  子孫アクセスは、次の関数にマップされます。

  ```vb
  Function Descendents(name As System.Xml.Linq.XName) As _
      System.Collections.Generic.IEnumerable(Of _
          System.Xml.Linq.XElement)
  ```

  上の例は次のようになります。

  ```vb
  Dim customers = company.Descendants("customer")
  ```

XML メンバーアクセス式の基本式は、値である必要があり、型である必要があります。

* 要素または子孫がにアクセスする場合は、`System.Xml.Linq.XContainer` または派生型、または `System.Collections.Generic.IEnumerable(Of T)` または派生型。 `T` は `System.Xml.Linq.XContainer` または派生型です。

* 属性にアクセスする場合は、@no__t 0 または派生型、または `System.Collections.Generic.IEnumerable(Of T)` または派生型のいずれかを指定します。 `T` は `System.Xml.Linq.XElement` または派生型です。

XML メンバーアクセス式の名前を空にすることはできません。 インポートで定義された名前空間を使用して、名前空間を修飾することができます。 以下に例を示します。

```vb
Imports <xmlns:db="http://example.org/database">

Module Test
    Sub Main()
        Dim customer = _
            <db:customer>
                <db:name>Bob</>
            </>
        Dim name = customer.<db:name>
    End Sub
End Module
```

XML メンバーアクセス式のドットの後、または山かっこと名前の間に空白を使用することはできません。 以下に例を示します。

```vb
Dim customer = _
    <customer age="30">
        <name>Bob</>
    </>
' All the following are error cases
Dim age = customer.@ age
Dim name = customer.< name >
Dim names = customer...< name >
```

@No__t-0 名前空間の型が使用できない場合は、XML メンバーアクセス式によってコンパイル時エラーが発生します。


## <a name="await-operator"></a>Await 演算子

Await 演算子は、「[非同期メソッド](statements.md#async-methods)」で説明されている非同期メソッドに関連しています。

```antlr
AwaitOperatorExpression
    : 'Await' Expression
    ;
```

`Await` は、すぐ外側のメソッドまたはラムダ式が表示されている場合に、その単語が `Async` 修飾子を持ち、`Await` が `Async` 修飾子の後に続く場合は、予約語です。他の場所では予約されていません。 プリプロセッサディレクティブでも予約されていません。 Await 演算子は、予約語であるメソッドまたはラムダ式の本体でのみ使用できます。 すぐ外側のメソッドまたはラムダ内では、await 式は、`Catch` または `Finally` ブロックの本体内、または `SyncLock` ステートメントの本体の内部、またはクエリ式の内部には出現しません。

Await 演算子は、値として分類される必要があり、型が*待機可能*型であるか、または `Object` である必要がある単一の式を受け取ります。 その型が `Object` の場合、すべての処理が実行時まで延期されます。 次のすべての条件に該当する場合は、型 `C` と呼ばれます。

* `C` には、引数を持たず、@no__t 型を返す `GetAwaiter` という名前のアクセス可能なインスタンスまたは拡張メソッドが含まれています。

* `E` には、読み取り可能なインスタンスまたは拡張プロパティが含まれています。このプロパティは、引数を取らず、ブール型の @no__t です。

* `E` には、引数を受け取らない `GetResult` という名前のアクセス可能なインスタンスまたは拡張メソッドが含まれています。

* `E` `System.Runtime.CompilerServices.INotifyCompletion` または `ICriticalNotifyCompletion` のいずれかを実装します。

@No__t-0 が `Sub` の場合、await 式は void として分類されます。 それ以外の場合、await 式は値として分類され、型は `GetResult` メソッドの戻り値の型になります。

待機できるクラスの例を次に示します。

```vb
Class MyTask(Of T)
    Function GetAwaiter() As MyTaskAwaiter(Of T)
        Return New MyTaskAwaiter With {.m_Task = Me}
    End Function

    ...
End Class

Structure MyTaskAwaiter(Of T)
    Implements INotifyCompletion

    Friend m_Task As MyTask(Of T)

    ReadOnly Property IsCompleted As Boolean
        Get
            Return m_Task.IsCompleted
        End Get
    End Property

    Sub OnCompleted(r As Action) Implements INotifyCompletion.OnCompleted
        ' r is the "resumptionDelegate"
        Dim sc = SynchronizationContext.Current
        If sc Is Nothing Then
            m_Task.ContinueWith(Sub() r())
        Else
            m_Task.ContinueWith(Sub() sc.Post(Sub() r(), Nothing))
        End If
    End Sub

    Function GetResult() As T
        If m_Task.IsCanceled Then Throw New TaskCanceledException(m_Task)
        If m_Task.IsFaulted Then Throw m_Task.Exception.InnerException
        Return m_Task.Result
    End Function
End Structure
```

__付箋.__ ライブラリの作成者は、同じ @no__t で継続デリゲートを呼び出すパターンに従うことをお勧めします。これは、`OnCompleted` がそれ自体で呼び出された場合と同じです。 また、再開デリゲートは、stack overflow につながる可能性があるため、`OnCompleted` メソッド内で同期的に実行することはできません。代わりに、デリゲートを後続の実行のためにキューに配置する必要があります。

制御フローが @no__t 0 演算子に到達すると、動作は次のようになります。

1.  Await オペランドの `GetAwaiter` メソッドが呼び出されます。 この呼び出しの結果は、 *awaiter*と呼ばれます。

2.  Awaiter の `IsCompleted` プロパティが取得されます。 結果が true の場合は、次のようになります。

    21. Awaiter の `GetResult` メソッドが呼び出されます。 @No__t-0 が関数の場合、await 式の値はこの関数の戻り値になります。

3.  IsCompleted プロパティが true でない場合は、次のようになります。

    31. @No__t-0 は、awaiter に対して呼び出されます (awaiter の @no__t 型が `ICriticalNotifyCompletion` を実装している場合)。または `INotifyCompletion.OnCompleted` を実装している場合は。それ以外の場合は。 どちらの場合も、非同期メソッドの現在のインスタンスに関連付けられている*再開デリゲート*を渡します。

    32. 現在の非同期メソッドインスタンスの制御点は中断され、*現在の呼び出し元*で制御フローが再開されます (セクションの[非同期メソッド](statements.md#async-methods)で定義されています)。

    33. 後で再開デリゲートが呼び出された場合、

        331. 再開デリゲートは、最初に `System.Threading.Thread.CurrentThread.ExecutionContext` が呼び出されたとき `OnCompleted` を呼び出しました。
        332. 次に、非同期メソッドインスタンスの制御ポイントで制御フローを再開します (「[非同期メソッド](statements.md#async-methods)」を参照)。
        333. ここでは、2.1 のように、awaiter の `GetResult` メソッドを呼び出します。

Await オペランドに型オブジェクトがある場合、この動作は実行時まで遅延されます。

- 手順1は、引数を指定せずに GetAwaiter () を呼び出すことで実現されます。そのため、実行時に、オプションのパラメーターを受け取るインスタンスメソッドにバインドできます。
- 手順2は、引数を指定せずに IsCompleted () プロパティを取得し、ブール型への組み込みの変換を試行することで実現されます。
- 手順 3. を実行すると `TryCast(awaiter, ICriticalNotifyCompletion)` を試行し、失敗した場合は-1 を @no__t ます。

3\. で渡される再開デリゲート。1回だけ呼び出すことができます。 2回以上呼び出された場合、動作は未定義になります。

