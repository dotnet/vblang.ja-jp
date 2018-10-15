# <a name="expressions"></a>式

式は、値の計算を指定するか、変数または定数を指定する演算子とオペランドのシーケンスです。 この章では、構文、オペランドと演算子の評価と式の意味の順序を定義します。

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

すべての式は、次のいずれかとして分類されます。

* *値。* すべての値には、型が関連付けられています。

* *変数です。* すべての変数は、関連付けられた型、変数の宣言型つまりを持っています。

* *名前空間。* メンバー アクセスの左側にあることにのみこの分類の式。 他のコンテキストでは、名前空間として分類される式は、コンパイル時エラーを発生します。

* *型。* メンバー アクセスの左側にあることにのみこの分類の式。 他のコンテキストでは、型として分類される式は、コンパイル時エラーを発生します。

* *メソッド グループ、* 同じ名前でオーバー ロードされたメソッドのセットです。 メソッド グループに関連付けられたターゲット式と関連付けられている型の引数リストがあります。

* *メソッドのポインター、* メソッドの場所を表します。 メソッドのポインターに関連付けられたターゲット式と関連付けられている型の引数リストがあります。

* *メソッドのラムダ*匿名メソッドであります。

* *プロパティ グループでは、* プロパティが同じ名前でオーバー ロードのセットです。 プロパティ グループに関連付けられたターゲット式があります。

* *プロパティ アクセス。* すべてのプロパティ アクセスは、関連付けられた型、つまり、プロパティの型を持ちます。 プロパティ アクセスに関連付けられたターゲット式があります。

* *遅延バインディング アクセス、* を表すメソッドまたはプロパティのアクセスが実行時まで延期します。 関連付けられたターゲット式と関連付けられている型の引数リスト、遅延バインディング アクセスがあります。 遅延バインディング アクセスの種類は常に`Object`します。

* *イベントへのアクセス。* イベントのすべてのアクセスが、関連付けられた型、つまり、イベントの種類。 イベントへのアクセスに関連付けられたターゲット式があります。 イベントへのアクセスは、最初の引数として表示される可能性、 `RaiseEvent`、 `AddHandler`、および`RemoveHandler`ステートメント。 他のコンテキストでは、イベント アクセスとして分類される式は、コンパイル時エラーを発生します。

* *配列リテラル、* を表す型が決定されていない配列の初期値。

* *無効にします。* これは、式がサブルーチン、または結果なしで await 演算子の式の呼び出しの場合に発生します。 Void として分類される式は、呼び出しステートメントや、await ステートメントのコンテキストでのみです。

* *既定値です。* リテラルのみ`Nothing`この分類が生成されます。

値または特定のコンテキストでのみ許可されている中間値として機能している式の他のカテゴリで、変数、式の最終的な結果は通常は。

ステートメントと式を式が特定の特性 (参照型、値の型、いくつかの種類などから派生されている) などの型を必要とする式の型が型パラメーターを使用できることに注意してください、制約が課される場合型パラメーターでは、このような特性を満たします。

### <a name="expression-reclassification"></a>式の再分類

通常、式は、式から別の分類が必要なコンテキストで使用されるが、コンパイル時エラーが発生する--などにリテラル値を代入しようとしています。 ただし、多くの場合は、式の分類のプロセスを変更すること*再分類*します。

再分類が成功するは、拡大または縮小として再分類が判定します。 明記されていない限りは、この一覧にすべての再分類を拡大します。

次の式の型を再分類できます。

* 変数は、値として再分類できます。 変数に格納された値がフェッチされます。

* メソッド グループは、値として再分類できます。 メソッド グループ式が関連付けられている対象の式と型パラメーターのリスト、および空のかっこを持つ invocation 式として解釈されます (つまり、`f`として解釈される`f()`と`f(Of Integer)`として解釈されます`f(Of Integer)()`). この再分類すると、さらに再分類される void として表現する可能性がありますが、

* メソッドのポインターは、値として再分類することができます。 この分類は、対象の型がわかっている変換のコンテキストでのみ実行できます。 ポインターのメソッドは、関連付けられている型の引数リストで適切な型のデリゲートのインスタンス化の式に引数として解釈されます。 例えば:
    
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

* Lambda メソッドは、値として再分類できます。 対象の型がわかっている変換のコンテキストで、再分類する場合は、2 つの再分類のいずれかが発生できます。
    
  1. 対象の型がデリゲート型の場合は、ラムダ メソッドは、適切な型のデリゲート構築式を引数として解釈されます。
    
  2. ターゲット型がある場合`System.Linq.Expressions.Expression(Of T)`、および`T`ラムダ メソッドがデリゲート構築式で使用されているかのように解釈されますが、デリゲート型`T`し、式ツリーに変換されます。
    
  デリゲートが ByRef パラメーターを持たない場合、非同期または反復子のラムダ メソッドをデリゲート構築式を引数として解釈のみ可能性があります。
    
  デリゲートのパラメーターの型のいずれかから対応するラムダ パラメーターの型への変換が縮小変換の場合は、再分類が縮小; として評価します。それ以外の場合は、拡大変換します。
    
  __注意してください。__ ラムダ メソッドと式ツリー間の正確な変換は、コンパイラのバージョン間で解決されはこの仕様の範囲外です。 Microsoft Visual Basic の 11.0 のすべてのラムダ式を次の制限に従い、式ツリーに変換可能性があります: (1) 1。  ByRef パラメーターを指定せずに 1 行のラムダ式だけは、式ツリーに変換することがあります。 単一行の`Sub`のみ呼び出しステートメント形式のラムダは式ツリーに変換可能性があります。 (2) は、匿名型の式はなど、後続のフィールドの初期化子を初期化するために以前のフィールド初期化子を使用する場合、式ツリーに変換できない`New With {.a=1, .b=.a}`します。 (3) は、オブジェクト初期化子式に変換できない式ツリーの現在のオブジェクトが初期化されるメンバーがフィールド初期化子のいずれかで使用する場合など`New C1 With {.a=1, .b=.Method1()}`します。 (要素の型を明示的に宣言する場合、4) 多次元配列作成式を式ツリーに変換のみできます。 (5) 遅延バインディング式は、式ツリーに変換できません。 (6) 変数またはフィールドは、invocation 式に ByRef を渡された場合、ByRef パラメーターと正確に同じ型ではありませんが、またはプロパティが ByRef を渡されるときに、通常の VB セマンティクスが ByRef を渡される引数のコピーを最終的な値はコピーされます。 変数またはフィールドやプロパティに戻す 式のツリーでは、コピー バックアップは発生しません。 (入れ子になったラムダ式もに、これらの 7) すべての制限が適用されます。
    
  対象の型が不明である場合、メソッドのラムダは、匿名のデリゲート型のラムダ メソッドの同じシグネチャを持つデリゲートのインスタンス化の式を引数として解釈されます。 厳密な型が使用されているし、任意のパラメーターの型を省略すると、コンパイル時エラーが発生します。それ以外の場合、`Object`不足しているパラメーターの型の代わりに使用します。 例えば:
    
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

* プロパティ アクセスには、プロパティ グループを再分類できます。 プロパティのグループ式が空のかっこインデックス式として解釈されます (つまり、`f`として解釈される`f()`)。

* プロパティ アクセスは、値として再分類できます。 プロパティ アクセス式の invocation 式として解釈されます、`Get`プロパティのアクセサー。 プロパティは、get アクセス操作子を持たない、コンパイル時エラーが発生します。

* 遅延バインディング アクセスは、遅延バインディング メソッドまたはプロパティの遅延バインディング アクセスとして再分類することができます。 場合、遅延バインディング アクセス再分類できますメソッドへのアクセスとプロパティへのアクセスの両方では、再分類プロパティへのアクセスをお勧めします。

* 遅延バインディング アクセスは、値として再分類することができます。

* 配列リテラルは、値として再分類することができます。 値の型は、次のように決定されます。

  1. 再分類が、ターゲット型がわかっているし、ターゲット型が配列型の変換のコンテキストで発生した場合、使用型の値として、配列リテラルが再分類します。 対象の型は場合`System.Collections.Generic.IList(Of T)`、 `IReadOnlyList(Of T)`、 `ICollection(Of T)`、 `IReadOnlyCollection(Of T)`、または`IEnumerable(Of T)`、配列リテラルが型の値として再分類し、配列リテラルが、入れ子のレベルを 1 つ`T()`します。

  2. それ以外の場合、配列リテラルが型が配列のランクと等しい入れ子のレベルと一緒に使用して、; 初期化子内の要素の主要な型によって決定された要素型の値に再分類します。主要な型を決定できない場合`Object`使用されます。 例えば:

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

  __注意してください。__ バージョン 9.0 と言語のバージョン 10.0 の間の動作が若干変更があります。 10.0 より前に、ローカル変数の型の推定によって影響されなかった配列要素の初期化子とその実行のようになりました。 したがって`Dim a() = { 1, 2, 3 }`あると推論された`Object()`の種類として`a`バージョン 9.0 の言語でと`Integer()`version 10.0 です。

  再分類し、配列配列作成式としてリテラルをとして再解釈します。 したがって、例。

  ```vb
  Dim x As Double = { 1, 2, 3, 4 }
  Dim y = { "a", "b" }
  ```

  同等です。

  ```vb
  Dim x As Double = New Double() { 1, 2, 3, 4 }
  Dim y = New String() { "a", "b" }
  ```

  再分類が縮小要素式から配列の要素の型へのすべての変換は縮小; 場合として判定します。それ以外の場合として拡大変換と判断されます。

* 既定値`Nothing`値として再分類できます。 対象の型がわかっている場合は、結果は、対象の型の既定値です。 対象の型が不明のコンテキストで、結果は型の null 値を`Object`します。

式の名前空間、型の式、イベントへのアクセス式、または void 式を再分類することはできません。 複数の再分類は、同時に実行できます。 例えば:

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

この場合、プロパティはグループ式`P`が最初に、プロパティ アクセス プロパティ グループから再分類して、値プロパティ アクセスから分類し直されます。 コンテキストで有効な分類に到達する再分類の最小数が実行されます。

## <a name="constant-expressions"></a>定数式

A*定数式*値はコンパイル時に完全に評価できる式です。

```antlr
ConstantExpression
    : Expression
    ;
```

定数式の型は、 `Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、 `ULong`、 `Long`、 `Char`、 `Single`、 `Double`、 `Decimal`、 `Date`、 `Boolean`、 `String`、 `Object`、または列挙型。 次の構成要素は、定数式で許可されています。

* リテラル (含む`Nothing`)。

* 定数の型のメンバーまたはローカル変数を定数への参照。

* 列挙型のメンバーへの参照。

* かっこで囲まれた部分式。

* 強制型変換の式では、対象の型は、上記の型の 1 つ用意されています。 強制型変換との間`String`この規則の例外は、ために null 値でのみ使用できます`String`変換が実行環境の現在のカルチャで実行時に常に行われます。 強制型変換の定数式が組み込みの変換を使用できますのみに注意してください。

* `+`、`-`と`Not`単項演算子、オペランドを指定して、結果は上記の型。

* `+`、 `-`、 `*`、 `^`、 `Mod`、 `/`、 `\`、 `<<`、 `>>`、 `&`、 `And`、 `Or`、 `Xor`、 `AndAlso`、 `OrElse`、 `=`、 `<`、 `>`、 `<>`、 `<=`、および`=>`二項演算子は、各オペランドを指定して、結果は上記の型。

* 条件演算子場合、各オペランドと結果が上に示した型を提供します。

* 次のランタイム関数: `Microsoft.VisualBasic.Strings.ChrW`;`Microsoft.VisualBasic.Strings.Chr`定数の値は 0 ~ 128; 場合`Microsoft.VisualBasic.Strings.AscW`定数文字列が空でない場合`Microsoft.VisualBasic.Strings.Asc`定数文字列が空でない場合。

次の構成体は*いない*定数式で使用できます。

* 暗黙的なバインドを通じて、`With`コンテキスト。

整数型の定数式 (`ULong`、 `Long`、 `UInteger`、 `Integer`、 `UShort`、 `Short`、 `SByte`、または`Byte`) より狭いの整数型に暗黙的に変換できると型の定数式`Double`に暗黙的に変換できる`Single`定数式の値が変換先の型の範囲内に提供します。 制限の少ないまたは strict セマンティクスが使用されているかどうかに関係なくこのような縮小変換が許可されます。


## <a name="late-bound-expressions"></a>遅延バインディング式

型の場合、メンバー アクセス式またはインデックスの式のターゲット`Object`式の処理は実行時まで遅延する可能性があります。 このように処理を遅延させることが呼び出されます*遅延バインディング*します。 遅延バインディングで`Object`で使用される変数を*型宣言不要な*メンバーのすべての解決は、変数の値の実際の実行時の型をに基づいて、方法です。 コンパイル環境または厳密な型が指定されて場合`Option Strict`、遅延バインディングによってコンパイル時エラーが発生します。 遅延バインディングを行うなどのオーバー ロードの解決の目的で非パブリック メンバーは無視されます。 呼び出しまたはへのアクセスは、事前バインディングの場合とは異なりに注意してください、`Shared`メンバーの遅延バインディングを実行時に評価される呼び出しの対象となります。 Invocation 式で定義されているメンバーのかどうか、式には`System.Object`、遅延バインディングは行われません。

一般に、遅延バインディング アクセスは解決実行時に、式の実際の実行時の型の識別子を参照しています。 実行時に、遅延バインディング メンバー検索が失敗した場合、`System.MissingMemberException`例外がスローされます。 遅延バインディング メンバー ルックアップが行われるため、関連付けられている対象の式の実行時の型、オフだけはありませんインターフェイスに、オブジェクトの実行時の型。 そのため、遅延バインディング メンバー アクセス式でインターフェイス メンバーにアクセスすることはできません。

遅延バインディング メンバー アクセスへの引数は、メンバー アクセス式に表示される順序で評価されます。 パラメーターが、遅延バインディング メンバーで宣言されている順序されません。 次の例は、この違いを示しています。

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

このコードが表示されます。

```
Early-bound: xy
Late-bound: yx
```

引数の実行時の型で、遅延バインディングされたオーバー ロードの解決が行われるために、式がコンパイル時または実行時に評価されるかどうかに基づいて異なる結果を生成することです。 次の例は、この違いを示しています。

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

このコードが表示されます。

```
F(Base)
F(Derived)
```

## <a name="simple-expressions"></a>単純な式

単純式は、リテラル、かっこで囲まれた式、インスタンスの式または簡易名式です。

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

リテラル式はリテラルで表される値に評価されます。 リテラル式はリテラル以外の値として分類`Nothing`、これは既定値として分類します。

```antlr
LiteralExpression
    : Literal
    ;
```

### <a name="parenthesized-expressions"></a>かっこで囲まれた式

かっこで囲まれた式は、かっこで囲まれた式で構成されます。 かっこで囲まれた式は、値として分類され、値として、囲まれた式を分類する必要があります。 かっこで囲まれた式は、かっこで囲まれた式の値に評価されます。

```antlr
ParenthesizedExpression
    : OpenParenthesis Expression CloseParenthesis
    ;
```

### <a name="instance-expressions"></a>インスタンス式

*インスタンス式*キーワード`Me`します。 非共有メソッド、コンス トラクター、またはプロパティ アクセサーの本体でのみ使用できます。 これは、値として分類されます。 キーワード`Me`実行されているメソッドまたはプロパティのアクセサーを含む型のインスタンスを表します。 コンス トラクターが明示的に別のコンス トラクターを呼び出す場合 (セクション[コンス トラクター](type-members.md#constructors))、`Me`そのコンス トラクターの呼び出し後までは使用できません、インスタンスがまだ作成していないためです。

```antlr
InstanceExpression
    : 'Me'
    ;
```

### <a name="simple-name-expressions"></a>簡易名式

A*簡易名式*省略可能な型引数リストの後に 1 つの識別子で構成されています。

```antlr
SimpleNameExpression
    : Identifier ( OpenParenthesis 'Of' TypeArgumentList CloseParenthesis )?
    ;
```

名前が解決されており、以下の"単純な名前解決ルールで"に分類されます。

1.  すぐ外側から始まるブロックし、各それを囲む外側のブロック (ある場合)、進める識別子には、ローカル変数、静的変数、定数のローカルの名前が一致する場合、メソッドの型パラメーター、またはパラメーターの識別子を参照します一致するエンティティ。

    ローカル変数、静的変数、またはローカル定数識別子に一致して、型引数リストが指定されて、コンパイル時エラーが発生します。 識別子、メソッド型パラメーターに一致する型引数リストが提供された場合は、一致が発生せず、解像度が続行されます。 一致するローカル変数が暗黙的な関数には、識別子には、ローカル変数が一致すると、または`Get`アクセサーは、ローカル変数を返すし、式は、呼び出しステートメントでは、invocation 式の一部または`AddressOf`し式一致が行われずの解像度が続行されます。

    式は、ローカル変数、静的変数、またはパラメーターである場合、変数として分類されます。 式は、メソッド型パラメーターである場合、型として分類されます。 式は、ローカル定数である場合、値として分類されます。

2.  式を含む入れ子になった型ごとに、最も内側から開始し、参照型の識別子の場合に、最も外側に移動には、アクセス可能なメンバーとの一致が生成されます。

    21. 一致する型のメンバーが型パラメーターの場合、結果は型として分類され、一致する型パラメーターは。 型引数リストが提供されている場合は、一致が行われず、解決処理が続行します。
    22. それ以外の場合、型は、すぐ外側の型と参照型の非共有メンバーを識別する場合、結果は、フォームのメンバー アクセスと同じ`Me.E(Of A)`ここで、`E`識別子と`A`型引数リストには、存在する場合。
    23. それ以外の場合、結果はまったく同じ形式のメンバー アクセス`T.E(Of A)`ここで、`T`が一致するメンバーを含む型`E`識別子、および`A`型引数リストがある場合。 この場合、非共有メンバーを参照する識別子のエラーを勧めします。

3.  入れ子になった名前空間ごとに、最も内側から開始し、最も外側の名前空間には、次の操作を行います。

    31. 名前空間では、指定した名前でアクセス可能な型が含まれていてがで指定される型引数リストで、存在する場合、識別子がその型を表してされ、型に分類が、同じ数の型パラメーターを持ちます。
    32. それ以外の場合、型引数リストが指定されていません、名前空間には、指定した名前の名前空間のメンバーが含まれている場合は、識別子、名前空間を参照し、名前空間として分類されます。
    33. それ以外の場合、名前空間には、1 つまたは複数のアクセス可能な標準的なモジュールが含まれていますメンバー名の識別子の参照がただ 1 つの標準的なモジュールでアクセス可能な一致を生成、場合、結果は正確に形式のメンバー アクセスと同じ`M.E(Of A)`、。場所`M`一致するメンバーを含む標準のモジュールは、`E`識別子、および`A`型引数リストがある場合。 識別子には、1 つ以上の標準的なモジュールでアクセス可能な型のメンバーが一致すると、コンパイル時エラーが発生します。

4.  ソース ファイルが 1 つまたは複数のインポート エイリアス、識別子のいずれかの名前に一致する場合は、識別子はその名前空間または型を参照します。 型引数リストを指定すると、コンパイル時エラーが発生します。

5. 名前参照を含むソース ファイルの 1 つまたは複数のインポート: 場合

    51. 1 つだけで、識別子と一致する場合、型引数リストが表示され、存在する場合、または型のメンバーでは、指定された型パラメーターの同じ番号を持つアクセス可能な型の名前をインポートし、識別子は、その型または型のメンバーを参照します。 識別子に一致する 1 つ以上のインポートの型パラメーターの同じ番号を持つアクセス可能な型の名前として存在する場合、型引数リストで指定されたか、アクセス可能な型、メンバー、コンパイル時エラーが発生します。
    52. それ以外の場合、型引数リストが指定されていません、識別子で正確に 1 つのインポートでアクセス可能な型を持つ名前空間の名前と一致する場合は、その名前空間を識別子が参照します。 型引数リストが指定されていないと、識別子では、複数のインポートでアクセス可能な型を持つ名前空間の名前と一致する、コンパイル時エラーが発生します。
    53. それ以外の場合、インポートには、1 つまたは複数のアクセス可能な標準的なモジュールが含まれ、識別子のメンバー名の参照がただ 1 つの標準的なモジュールでアクセス可能な一致を生成、結果は、フォームのメンバー アクセスと同じ`M.E(Of A)`ここで、`M`一致するメンバーを含む標準のモジュールは、`E`識別子、および`A`型引数リストがある場合。 識別子には、1 つ以上の標準的なモジュールでアクセス可能な型のメンバーが一致すると、コンパイル時エラーが発生します。

6.  コンパイル環境は、1 つまたは複数のインポート エイリアスを定義、識別子のいずれかの名前に一致する場合は、識別子はその名前空間または型を参照します。 型引数リストを指定すると、コンパイル時エラーが発生します。

7. 場合は、コンパイル環境では、1 つまたは複数のインポートを定義します。

    71. 1 つだけで、識別子と一致する場合、型引数リストが表示され、存在する場合、または型のメンバーでは、指定された型パラメーターの同じ番号を持つアクセス可能な型の名前をインポートし、識別子は、その型または型のメンバーを参照します。 識別子に一致する 1 つ以上のインポートの型パラメーターの同じ番号を持つアクセス可能な型の名前として存在する場合、型引数リストで指定されたか、型のメンバーでは、コンパイル時エラーが発生します。
    72. それ以外の場合、型引数リストが指定されていません、識別子で正確に 1 つのインポートでアクセス可能な型を持つ名前空間の名前と一致する場合は、その名前空間を識別子が参照します。 型引数リストが指定されていないと、識別子では、複数のインポートでアクセス可能な型を持つ名前空間の名前と一致する、コンパイル時エラーが発生します。
    73. それ以外の場合、インポートには、1 つまたは複数のアクセス可能な標準的なモジュールが含まれ、識別子のメンバー名の参照がただ 1 つの標準的なモジュールでアクセス可能な一致を生成、結果は、フォームのメンバー アクセスと同じ`M.E(Of A)`ここで、`M`一致するメンバーを含む標準のモジュールは、`E`識別子、および`A`型引数リストがある場合。 識別子には、1 つ以上の標準的なモジュールでアクセス可能な型のメンバーが一致すると、コンパイル時エラーが発生します。

8. それ以外の場合、識別子によって指定された名前は定義されません。

定義されていない簡易名の式は、コンパイル時エラーです。

通常、名前できます 1 回だけでは、特定の名前空間。 ただし、ため、複数の .NET アセンブリ間で名前空間を宣言することができます、2 つのアセンブリが同じ完全修飾名を持つ型を定義する状況を含めることは可能です。 その場合は、ソース ファイルの現在のセットで宣言された型は、外部 .NET アセンブリで宣言された型を優先します。 それ以外の場合、名前があいまいと名前のあいまいさを解消する方法はありません。


### <a name="addressof-expressions"></a>AddressOf 式

`AddressOf`式はメソッドのポインターを生成するために使用します。 式から成る、`AddressOf`キーワードと式をメソッド グループ、または遅延バインディング アクセスとして分類する必要があります。 メソッド グループは、コンス トラクターを参照できません。

結果は、メソッド グループとしての型引数リスト (ある場合) と同じ関連付けられている対象の式で、メソッドのポインターとして分類されます。

```antlr
AddressOfExpression
    : 'AddressOf' Expression
    ;
```

## <a name="type-expressions"></a>型の式

A*式を入力*は、`GetType`式、`TypeOf...Is`式、`Is`式、または`GetXmlNamespace`式。

```antlr
TypeExpression
    : GetTypeExpression
    | TypeOfIsExpression
    | IsExpression
    | GetXmlNamespaceExpression
    ;
```

### <a name="gettype-expressions"></a>GetType 式

A`GetType`式から成るキーワード`GetType`と型の名前。

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

A`GetType`式は、値として分類され、その値は、リフレクション (`System.Type`) を表すクラス、 *GetTypeTypeName*します。 場合、 *GetTypeTypeName* 、型パラメーターは、式を返す、`System.Type`実行時に、型パラメーターに対して指定された型引数に対応するオブジェクト。

*GetTypeTypeName* 2 つの方法で特別な。

* それが許容`System.Void`、この型の名前を参照する言語で唯一の場所。

* 省略すると、型引数で構築されたジェネリック型である可能性があります。 これにより、`GetType`を返す式、`System.Type`ジェネリック型パラメーター自体に対応するオブジェクト。

次の例で、`GetType`式。

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

結果の出力は次のとおりです。

```
Int32
Int32
String
Double[]
```


### <a name="typeofis-expressions"></a>TypeOf.式は、します。

A`TypeOf...Is`式を使用して、実行時の型の値が指定された型と互換性のあるかどうかを確認します。 最初のオペランドは値として分類する必要があります、再分類のラムダ メソッドにすることはできません、および参照型の制約のない型パラメーターの型である必要があります。 2 番目のオペランドは、型名である必要があります。 値として分類があり、式の結果、`Boolean`値。 式の評価が`True`場合は、オペランドの実行時の型がある、id、既定、参照、配列、値の型または型に型パラメーター変換`False`それ以外の場合。 式の型と特定の型の間の変換が存在しない場合、コンパイル時エラーが発生します。

```antlr
TypeOfIsExpression
    : 'TypeOf' Expression 'Is' LineTerminator? TypeName
    ;
```

### <a name="is-expressions"></a>式は、します。

`Is`または`IsNot`参照が等値比較を行う式を使用します。

```antlr
IsExpression
    : Expression 'Is' LineTerminator? Expression
    | Expression 'IsNot' LineTerminator? Expression
    ;
```

各式は、値として分類する必要があり、各式の型が参照型、制約のない型パラメーターの型、または null 許容値型にする必要があります。 1 つの式の型が、制約のない型パラメーターの型または null 許容値型の場合は、ただし、その他の式があります、リテラル`Nothing`します。

結果を値として分類され、として型指定された`Boolean`します。 `Is`操作の評価に`True`両方の値が同じインスタンスを参照するか、両方の値が`Nothing`、または`False`それ以外の場合。 `IsNot`操作の評価に`False`両方の値が同じインスタンスを参照するか、両方の値が`Nothing`、または`True`それ以外の場合。


### <a name="getxmlnamespace-expressions"></a>GetXmlNamespace 式

A`GetXmlNamespace`式から成るキーワード`GetXmlNamespace`とソース ファイルまたはコンパイル環境で宣言されている XML 名前空間の名前。

```antlr
GetXmlNamespaceExpression
    : 'GetXmlNamespace' OpenParenthesis XMLNamespaceName? CloseParenthesis
    ;
```

`GetXmlNamespace`式は、値として分類され、その値のインスタンスである`System.Xml.Linq.XNamespace`を表す、 *XMLNamespaceName*します。 その型が使用できない場合は、コンパイル時エラーが発生します。

例えば:

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

名前空間の名前の一部は、空白などの周囲の XML 規則が適用されますので、かっこ間にあるすべてと見なされます。 例えば:

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

XML 名前空間の式も省略できる場合、式が既定の XML 名前空間を表すオブジェクトを返します。


## <a name="member-access-expressions"></a>メンバー アクセス式

メンバー アクセス式は、エンティティのメンバーへのアクセスに使用されます。

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

形式のメンバー アクセス`E.I(Of A)`ここで、`E`式、非配列の型名、キーワード`Global`、または省略した場合と`I`は省略可能な型引数リストを持つ識別子`A`を評価および分類次のようにします。

1. 場合`E`を省略すると、すぐに含むから式では、`With`ステートメントは、代わりに使用`E`され、メンバー アクセスが実行されます。 格納しているがない場合は`With`ステートメントでは、コンパイル時エラーが発生します。

2. 場合`E`名前空間として分類されますまたは`E`キーワード`Global`、メンバー検索は指定した名前空間のコンテキストで実行し、します。 場合`I`がで指定された型引数リストで、存在する場合、結果はそのメンバーは、同じ数の型パラメーターでその名前空間のアクセス可能なメンバーの名前。 結果は、名前空間または型によっては、メンバーとして分類されます。 それ以外の場合は、コンパイル時のエラーが発生します。

3. 場合`E`が、型または型として分類される式、メンバー検索は、指定した型のコンテキストで実行します。 場合`I`のアクセス可能なメンバーの名前を指定`E`、し`E.I`が評価され、次のように分類されます。

    31. 場合`I`キーワード`New`と`E`コンパイル時エラーが発生し、列挙体ではありません。
    32. 場合`I`がで指定される型引数リストで、存在する場合、結果はその型の型パラメーターの同じ番号を持つ型を識別します。
    33. 場合`I`結果は、関連付けられている型の引数リストと関連付けられているターゲット式を指定せず、メソッド グループに 1 つまたは複数のメソッドを識別します。
    34. 場合`I`1 つまたは複数のプロパティと型を持たないを識別する引数リストが指定されて、結果は、プロパティ式のないグループに関連付けられたターゲット。
    35. 場合`I`共有変数となしの型を識別する引数リストが指定されて、結果は、変数または値のいずれか。 かどうか、変数は読み取り専用と、変数が宣言されている型の共有のコンス トラクターの外部参照が発生した、結果は、共有変数の値`I`で`E`します。 それ以外の場合、結果は、共有変数`I`で`E`します。
    36. 場合`I`共有イベントとなしの型を識別する引数リストが指定されて、イベントに関連付けられたターゲット式を指定しないアクセスになります。
    37. 場合`I`定数となしの型を識別する引数リストを提供が、その結果は、その定数の値。
    38. 場合`I`列挙体のメンバーと型を持たないを識別し、引数リストが指定されて、結果は、その列挙体メンバーの値。
    39. それ以外の場合、`E.I`無効なメンバー参照であり、コンパイル時エラーが発生します。

4. 場合`E`変数または、型が値として分類されます`T`、メンバー検索のコンテキストで実行し、`T`します。 場合`I`のアクセス可能なメンバーの名前を指定`T`、し`E.I`が評価され、次のように分類されます。

    41. 場合`I`キーワード`New`、`E`は`Me`、 `MyBase`、または`MyClass`、型引数が指定されていない、結果はの型のインスタンスコンストラクターを表すメソッドグループ`E`の式が関連付けられているターゲット`E`と型引数の一覧がありません。 それ以外の場合は、コンパイル時のエラーが発生します。
    42. 場合`I`場合は、拡張メソッドを含む 1 つまたは複数のメソッドを識別する`T`ない`Object`、結果は、関連付けられている型の引数リストとの関連付けられている対象の式は、メソッド グループ`E`します。
    43. 場合`I`、結果の式が関連付けられているターゲット プロパティ グループは、1 つまたは複数のプロパティと引数が指定された種類がありませんを識別する`E`します。
    44. 場合`I`結果は、変数または値のいずれか共有変数またはインスタンス変数と引数が指定されました、なしの型を識別します。 変数は読み取り専用変数が宣言されている変数 (共有またはインスタンス) の種類に適したクラスのコンス トラクターの外部参照が発生し、かどうか、結果は、変数の値`I`で参照されるオブジェクトによって`E`します。 場合`T`、参照型では、結果は、変数、`I`によって参照されるオブジェクトで`E`します。 の場合`T`値型と式は、`E`変数として分類されると、結果は、変数は、それ以外の場合、結果は、値。
    45. 場合`I`イベントとなしの型を識別する引数に指定された場合、結果は、イベントに関連付けられたターゲット式のアクセスは`E`します。
    46. 場合`I`結果はその定数の値は、定数と引数が指定された種類がありませんを識別します。
    47. 場合`I`結果はその列挙体メンバーの値は、列挙体のメンバーと引数が指定された種類がありませんを識別します。
    48. 場合`T`は`Object`、結果は、関連付けられている型の引数リストと、関連付けられている対象の式の遅延バインディング アクセスとして分類される遅延バインディング メンバー ルックアップ`E`します。

5. それ以外の場合、`E.I`無効なメンバー参照であり、コンパイル時エラーが発生します。

形式のメンバー アクセス`MyClass.I(Of A)`と等価`Me.I(Of A)`がすべてのメンバーにアクセスが、メンバーはオーバーライドできないかのように扱われます。 このため、アクセスされるメンバーは、メンバーがアクセスされている値の実行時の型によっては影響ありません。

形式のメンバー アクセス`MyBase.I(Of A)`と等価`CType(Me, T).I(Of A)`場所`T`はメンバー アクセス式を含む型の直接の基本型です。 すべてのメソッド呼び出しは、呼び出されるメソッドはオーバーライドできないとして扱われます。 この形式のメンバー アクセスとも呼ばれますが、 *base アクセス*します。

次の例でどのように`Me`、`MyBase`と`MyClass`関連。

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

このコードを出力します。

```
MoreDerived.F
Derived.F
Derived.F
```

キーワードを使用して、メンバー アクセス式を開始する`Global`、キーワード表す最も外側の名前なし名前空間宣言が外側の名前空間をシャドウする場合に便利です。 `Global`キーワードは、そのような状況で最も外側の名前空間を"エスケープ"処理を使用します。 例えば:

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

上記の例でも最初のメソッド呼び出しが無効ですので識別子`System`、クラスにバインド`System`、名前空間ではない`System`します。 アクセスする唯一の方法、`System`名前空間は、使用する`Global`最も外側の名前空間をエスケープします。

期間の左側にある任意の式は不要であり、メンバー アクセスを実行しない限りは評価されない場合、アクセスされるメンバーを共有すると、遅延バインディング。 次に例を示します。

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

印刷`The value of F is: 10`ため関数`ReturnC`のインスタンスを指定するために呼び出す必要はありません`C`共有メンバーにアクセスする`F`します。


### <a name="identical-type-and-member-names"></a>同一の型およびメンバー名

名前のメンバーの種類と同じ名前を使用することは珍しくありません。 この場合、ただし、不都合な名前の非表示に発生します。

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

前の例では、簡易名で`Color`で`DefaultColor`型の代わりに、インスタンス プロパティにバインドします。 インスタンス メンバーは、共有メンバーでは参照できません、ため、通常なりますエラー。

ただし、特別な規則は、型へのアクセスをここでできます。 メンバー アクセス式の基本の式が簡易名、定数、フィールド、プロパティ、ローカル変数または型に同じ名前を持つパラメーターにバインドする場合は、メンバーまたは型のいずれか、基本式は参照できます。 これは、ことができますいずれか 1 つからアクセスできるメンバーが同じであるために、曖昧になることはありません。

### <a name="default-instances"></a>既定のインスタンス

場合によっては、共通から派生したクラスは基底クラスは、通常、または常に 1 つのインスタンスのみがあります。 たとえば、しかユーザー インターフェイスに表示するほとんどの windows では、いつでも、画面上に表示する 1 つのインスタンスがあります。 Visual Basic を自動的に生成するクラスのような作業を簡素化する*既定インスタンス*の各クラスに簡単に参照される、1 つのインスタンスを提供するクラス。

既定のインスタンスが常に作成、*ファミリ*1 つの特定の種類ではなく型のです。 したがって Form1 フォームから派生したクラスの既定のインスタンスを作成する代わりには、既定のインスタンスはフォームから派生したすべてのクラスに対して作成されます。 これは、既定のインスタンスに特別にマークする個々 の基本クラスから派生したクラスがないことを意味します。

クラスの既定のインスタンスは、そのクラスの既定のインスタンスを返すコンパイラによって生成されたプロパティで表されます。 クラスのメンバーとして生成されたプロパティと呼ばれる、*グループ クラス*の割り当てを管理して、特定の基底クラスから派生したすべてのクラスの既定のインスタンスを破棄します。 派生したすべてのクラスの既定のインスタンスのプロパティの例では、`Form`で収集される可能性があります、`MyForms`クラス。 グループ クラスのインスタンスが、式によって返されるかどうか`My.Forms`、次のコードが派生クラスの既定のインスタンスにアクセスし、`Form1`と`Form2`:

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

既定のインスタンスは、それが最初に参照されるまでは作成されません。既に作成されていないかに設定されている場合に作成される既定のインスタンスの既定のインスタンスを表すプロパティをフェッチしています`Nothing`します。 既定のインスタンスの対象である場合、既定のインスタンスの存在をテストできるように、`Is`または`IsNot`オペレーターは、既定のインスタンスは作成されません。 したがって、既定のインスタンスがあるかどうかをテストすることは`Nothing`または既定のインスタンスを作成することがなく他のいくつか参照します。

既定のインスタンスは、簡単に既定のインスタンスがあるクラスの外部から既定のインスタンスを参照してください。 それを定義するクラス内での既定のインスタンスを使用すると、どのインスタンスで参照される、既定のインスタンスまたは現在のインスタンスつまり混乱が生じる場合があります。 たとえば、次のコードは値のみを変更します。 `x` 、既定のインスタンスの場合でも呼び出された別のインスタンスから。 コードは、値を出力するため`5`の代わりに`10`:

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

このような混乱を防ぐためには、既定のインスタンスから、インスタンス メソッド内で、既定のインスタンスの型を参照することはできません。

#### <a name="default-instances-and-type-names"></a>既定のインスタンスと型名

既定のインスタンスは、その型の名前を使用して直接アクセスできる場合もあります。 場所の種類名では、式を使用できません、式のコンテキストでこの例では、`E`ここで、`E`既定のインスタンスを使用して、クラスの完全修飾名を表すに変更が`E'`ここで、`E'`を表します既定のインスタンスのプロパティをフェッチする式。 たとえば、既定のインスタンスの派生クラスから`Form`次のコードは、前の例のコードに相当し、型の名前を既定のインスタンスへのアクセスを許可します。

```vb
Module Main
    Sub Main()
        Form1.x = 10
        Console.WriteLine(Form2.y)
    End Sub
End Module
```

つまり、型の名前からアクセス可能な既定のインスタンスは、型名を割り当てることもします。 たとえば、次のコードの既定のインスタンスを設定します`Form1`に`Nothing`:。

```vb
Module Main
    Sub Main()
        Form1 = Nothing
    End Sub
End Module
```

注意の意味`E.I`された`E`クラスを表しますと`I`表します共有メンバーは変更されません。 まだ、このような式では、クラスのインスタンスから直接共有メンバーにアクセスしては既定のインスタンスを参照していません。

#### <a name="group-classes"></a>クラスにグループ化

`Microsoft.VisualBasic.MyGroupCollectionAttribute`属性の既定のインスタンスのファミリのグループのクラスを示します。 属性には、4 つのパラメーターがあります。

* パラメーター`TypeToCollect`グループの基本クラスを指定します。 すべてのインスタンス化可能なクラス (型パラメーター) に関係なく、この名前を持つ型から派生したオープン型のパラメーターを指定せずに自動的に既定のインスタンスになります。

* パラメーター`CreateInstanceMethodName`既定インスタンスのプロパティの新しいインスタンスを作成するグループのクラスを呼び出す方法を指定します。

* パラメーター`DisposeInstanceMethodName`既定インスタンスのプロパティの値が割り当てられている場合、既定のインスタンスのプロパティを破棄するグループのクラスを呼び出す方法を指定します`Nothing`します。

* パラメーター`DefaultInstanceAlias`詳細式`E'`既定のインスタンスが、型名から直接アクセスできる場合は、クラス名の代わりにします。 このパラメーターが場合`Nothing`または空の文字列の場合は、このグループの種類の既定のインスタンスは、型の名前を使用して直接アクセスできません。 (__に注意してください。__ Visual Basic 言語のすべての現在の実装で、`DefaultInstanceAlias`パラメーターは無視されます、コンパイラのコードでは可します)。

複数の種類は、コンマを使用して最初の 3 つのパラメーターのメソッドや型の名前を分離することで、同じグループに収集できます。 同じ数、各パラメーター内の項目の必要があり、リストの要素は順番に照合します。 たとえば、次の属性宣言はから派生した型を収集します。 `C1`、`C2`または`C3`1 つのグループ。

```vb
<Microsoft.VisualBasic.MyGroupCollection("C1, C2, C3", _
    "CreateC1, CreateC2, CreateC3", _
    "DisposeC1, DisposeC2, DisposeC3", "My.Cs")>
Public NotInheritable Class MyCs
    ...
End Class
```

Create メソッドのシグネチャは、の形式にする必要があります`Shared Function <Name>(Of T As {New, <Type>})(Instance Of T) As T`します。 Dispose メソッドは、の形式にする必要があります`Shared Sub <Name>(Of T As <Type>)(ByRef Instance Of T)`します。 したがって、前のセクションの例でグループ クラスを次のように宣言する可能性があります。

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

ソース ファイルには、派生クラスが宣言されている場合`Form1`、生成されるグループ クラスと等価になります。

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

拡張メソッド、メンバー アクセス式`E.I`名前を持つ拡張メソッドのすべての収集が収集した`I`現在のコンテキストで使用できます。

1. 最初に、式を含む各入れ子にされた型がオンになって、最も内側から開始し、最も外側にします。
2. 次に、入れ子になった各名前空間がオンになって、最も内側から開始し、最も外側の名前空間にします。
3. 次に、ソース ファイルのインポートがチェックされます。
4. 次に、コンパイル環境で定義されているインポートがチェックされます。

拡張メソッドは、拡大ネイティブへの変換対象の式の型から最初のパラメーターの型の拡張メソッドがある場合にのみ収集されます。 定期的な簡易名式のバインディングとは異なり、検索の収集と*すべて*拡張メソッドはコレクションでは、拡張メソッドが見つかった場合は停止しません。 例えば:

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

この例でも`N2C1Extensions.M1`が前に見つかった`N1C1Extensions.M1`、両者は拡張メソッドとしてと見なされます。 収集されたすべての拡張メソッドと*カリー化*します。 カリー化拡張メソッドの呼び出しのターゲットを受け取るし、新しいメソッドのシグネチャ (指定されている) ために、削除する最初のパラメーターとその結果、拡張メソッドの呼び出しに適用されます。 例えば:

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

上記の例では、適用したカリー化された結果で`v`に`Ext1.M`メソッドのシグネチャは、`Sub M(y As Integer)`します。

拡張メソッドの最初のパラメーターを削除するだけでなくは最初のパラメーターの型の一部であるメソッドの型パラメーターの削除カリー化もします。 メソッド型パラメーターを持つ拡張メソッドをカリー化型の推定は、最初のパラメーターに適用され、結果は固定の推定される型パラメーターです。 型の推定が失敗した場合、メソッドは無視されます。 例えば:

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

上記の例では、適用したカリー化された結果で`v`に`Ext1.M`メソッドのシグネチャは、`Sub M(Of U)(y As U)`ため、型パラメーター`T`カリー化の結果として推論され、は修正されています。 ため、型パラメーター`U`オープンのパラメーターは、カリー化の一部として推論ができません。 同様に、ため、型パラメーター`T`適用の結果として推論される`v`に`Ext2.M`、パラメーターの型`y`として固定になります`Integer`します。 その他の種類を推論できません。 署名を除くすべての制約をカリー化と`New`も制約が適用されます。 制約が満たされていない、または、カリー化の一部としてしない推定された型に依存、拡張メソッドは無視されます。 例えば:

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

__注意してください。__ 拡張メソッドのカリー化を行うための主な理由の 1 つは、クエリ パターンのメソッドの引数を評価する前に、イテレーションの型を推論するクエリ式でできることです。 ほとんどのクエリ パターンのメソッド、ラムダ式を必要とする型の推定自体を実行するためのクエリ式の評価プロセスかなりシンプルです。

通常のインターフェイスの継承とは異なり互いに関連していない 2 つのインターフェイスを拡張する拡張メソッドは、同じカリー化されたシグネチャがあるない限り、使用可能なは。

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

最後に、遅延バインディングの実行時に、メソッドは考慮されませんが、その拡張機能に注意してください。

```vb
Module Test
    Sub Main()
        Dim o As Object = ...

        ' Ignores extension methods
        o.M1()
    End Sub
End Module
```

## <a name="dictionary-member-access-expressions"></a>ディクショナリ メンバー アクセス式

A*ディクショナリ メンバー アクセス式*コレクションのメンバーを検索するために使用します。 ディクショナリ メンバー アクセスは、の形式をとります`E!I`ここで、`E`値として分類される式を指定し、`I`識別子です。

```antlr
DictionaryAccessExpression
    : Expression? '!' IdentifierOrKeyword
    ;
```

式の型は、1 つによってインデックス付けされた既定のプロパティをいる必要があります`String`パラメーター。 ディクショナリ メンバー アクセス式`E!I`式に変換されます`E.D("I")`ここで、`D`の既定のプロパティは、`E`します。 例えば:

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

感嘆符がない式では、すぐに含むから式で指定されている場合`With`ステートメントが使用されます。 格納しているがない場合は`With`ステートメントでは、コンパイル時エラーが発生します。


## <a name="invocation-expressions"></a>Invocation 式

Invocation 式は、呼び出し対象と省略可能な引数リストで構成されます。

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

対象の式はメソッド グループ、またはデリゲート型の値として分類する必要があります。 対象の式がデリゲート型の値としている場合、invocation 式のターゲットのメソッドのグループになります、`Invoke`デリゲートの型と、対象の式のメンバーは、メソッドの関連付けられている対象の式になりますグループ。

引数リストが 2 つのセクション: 位置指定引数と名前付き引数。 *位置指定引数*式は、名前付き引数の前にする必要があります。 *名前付き引数*開始後に、キーワードに一致する識別子を持つ`:=`と式。

メソッド グループには、1 つのアクセス可能なメソッドのみが含まれる場合、インスタンスと拡張機能の両方のメソッド、およびそのメソッドを含む引数を受け取らない関数は、メソッド グループは空の引数リストを持つ invocation 式として解釈され、結果は指定された引数リストで、invocation 式のターゲットとして使用されます。 例えば:

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

それ以外の場合、オーバー ロードの解決は、指定した引数リストの最適な方法を選択するメソッドに適用されます。 最適なメソッドが関数の場合は、invocation 式の結果は、関数の戻り値の型として型指定された値として分類されます。 最適なメソッドがサブルーチンの場合は、結果が void として分類されます。 最適なメソッドが本文がない部分メソッドの場合は、invocation 式は無視され、結果は void として分類されます。

事前バインディングされた呼び出し式の引数は、対応するパラメーターが、対象のメソッドで宣言されている順序で評価されます。 遅延バインディング メンバー アクセス式の場合、メンバー アクセス式で表示される順序で評価されます。 セクションを参照して[遅延バインディング式](expressions.md#late-bound-expressions)。

## <a name="overloaded-method-resolution"></a>オーバー ロードされたメソッドの解決方法:
汎用性、引数リスト、渡す引数、および省略可能なパラメーター、条件付きのメソッドの引数のピッキングへの適用性を一覧表示し、入力引数を推定メンバー/型の引数が指定の特異性、オーバー ロードの解決: 」セクションを参照してください。[オーバー ロードの解決](overload-resolution.md)します。

## <a name="index-expressions"></a>インデックス式

*インデックス式*結果が配列の要素またはプロパティ アクセスにプロパティ グループを再分類します。 順序、式、開きかっこを入力、インデックス引数リスト、および閉じかっこでインデックスの式で構成されます。

```antlr
IndexExpression
    : Expression OpenParenthesis ArgumentList? CloseParenthesis
    ;
```

インデックスの式のターゲットは、プロパティ グループまたは値のいずれかとして分類する必要があります。 インデックスの式は、次のように処理されます。

* 対象の式が値として分類される場合と、その型が配列型でない場合`Object`、または`System.Array`種類が既定のプロパティをいる必要があります。 インデックスは、すべての型の既定のプロパティを表すプロパティ グループで実行されます。 Visual Basic でのパラメーターなしの既定のプロパティの宣言ではありませんが、他の言語は、このようなプロパティを宣言することができます。 そのため、引数なしでのプロパティのインデックス作成が許可されます。

* 式の結果配列型の値に、引数リストの引数の数が配列型のランクと同じである必要があり、名前付き引数を含まない場合があります。 インデックスのいずれかが無効の場合、実行時に、`System.IndexOutOfRangeException`例外がスローされます。 各式は、型に暗黙的に変換可能である必要があります`Integer`します。 インデックスの式の結果は、指定したインデックス位置にある変数し、変数として分類されます。

* 式は、プロパティ グループに分類は、プロパティの 1 つはインデックス引数のリストに適用できるかどうかを判断するオーバー ロードの解決が使用されます。 プロパティ グループにのみを持つ 1 つのプロパティが含まれるかどうか、`Get`アクセサー、およびそのアクセサーには引数を受け取らないかどうかは、プロパティ グループは空の引数リストを持つインデックス式として解釈されます。 結果は、現在のインデックスの式のターゲットとして使用されます。 適用可能なプロパティがない場合は、コンパイル時エラーが発生します。 それ以外の場合、式の結果には、プロパティ アクセスのプロパティ グループに関連付けられたターゲット式 (ある場合)。

* 遅延バインディング プロパティ グループ、または型が値として式が分類される場合`Object`または`System.Array`インデックスの式の処理が実行時まで延期および遅延バインディングには、インデックスを作成します。 として型指定された式の結果、遅延バインディング プロパティ アクセスで`Object`します。 関連付けられている対象の式は、値である場合は、対象の式または、プロパティ グループの関連付けられている対象の式のいずれかです。 実行時に、式は、次のように処理されます。

* 場合は、式は、遅延バインディング プロパティ グループに分類されます、式の可能性があります (メンバーが、インスタンスまたは共有変数の場合)、メソッド グループ、プロパティ グループ、または値の結果。 結果が、メソッド グループまたはプロパティのグループの場合は、オーバー ロードの解決は、引数の一覧については、適切な方法をグループに適用されます。 オーバー ロードの解決に失敗した場合、`System.Reflection.AmbiguousMatchException`例外がスローされます。 プロパティ アクセス、または呼び出しとして結果を処理し、結果が返されます。 サブルーチンの呼び出しがある場合、結果は、`Nothing`します。

* 対象の式の実行時の型が配列型または`System.Array`インデックスの式の結果は、指定したインデックス位置にある変数の値。

* それ以外の場合、式の実行時の型は、既定のプロパティをいる必要があり、インデックスを表すすべての型の既定のプロパティのプロパティ グループで実行します。 型の既定のプロパティがない、`System.MissingMemberException`例外がスローされます。


## <a name="new-expressions"></a>新しい式

`New`演算子を使用して、型の新しいインスタンスを作成します。 次の 4 つの形式としては、`New`式。

* オブジェクト作成式は、クラス型と値の型の新しいインスタンスを作成に使用されます。

* 配列作成式は、配列型の新しいインスタンスを作成に使用されます。

* 型のデリゲートの新しいインスタンスを作成するデリゲート作成式 (これは、オブジェクト作成式から異なる構文がありません) が使用されます。

* 匿名クラス型の新しいインスタンスを作成する匿名オブジェクト作成式が使用されます。

```antlr
NewExpression
    : ObjectCreationExpression
    | ArrayExpression
    | AnonymousObjectCreationExpression
    ;
```

A`New`式は値として分類され、結果は、型の新しいインスタンス。


### <a name="object-creation-expressions"></a>オブジェクト作成式

オブジェクト作成式を使用して、クラス型または構造型の新しいインスタンスを作成します。

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

オブジェクト作成式の型がクラス型、構造体型、または型パラメーターである必要があります、`New`制約することはできません、`MustInherit`クラス。 フォームのオブジェクト作成式が指定`New T(A)`ここで、`T`がクラス型または構造体の型と`A`、省略可能な引数リストは、オーバー ロードの解決の正しいコンス トラクターを決定します`T`を呼び出します。 型パラメーターで、`New`制約は、1 つのパラメーターなしのコンス トラクターを持つと見なされます。 呼び出し可能なコンス トラクターがない場合は、コンパイル時エラーが発生します。それ以外の場合、式の結果の新しいインスタンスの作成に`T`選択したコンス トラクターを使用します。 引数がない場合は、かっこは省略できます。

インスタンスが割り当てられているインスタンスは、クラス型または値型かどうかによって異なります。 `New` クラス型のインスタンスは、スタックに直接値型の新しいインスタンスを作成中に、システム ヒープに作成されます。

必要に応じて、オブジェクト作成式は、コンス トラクターの引数の後にメンバーの初期化子の一覧を指定できます。 これらのメンバー初期化子のキーワードが付いて`With`、初期化子リストは、のコンテキストでの場合と解釈されます、`With`ステートメント。 たとえば、クラスを考えてみます。

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

約と同等です。

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

各初期化子に割り当てるには、名前を指定する必要があり、名前にする必要があります以外`ReadOnly`; 構築される型のプロパティのインスタンス変数または構築される型がある場合に、メンバー アクセスが遅延バインディングできません`Object`します。 初期化子を使用していない可能性があります、`Key`キーワード。 型内の各メンバーは、1 回のみ初期化できます。 ただし、初期化子式では、相互に参照可能性があります。 例えば:

```vb
Module Test
    Sub Main()
        Dim x As New Customer() With { .Name = "Bob Smith", _
            .Address = .Name & " St." }
    End Sub
End Module
```

初期化子が左から右に割り当てられた、コンス トラクターが実行された後にインスタンス変数初期化子は、まだ初期化されていないメンバーを参照している場合は表示されるように値します。

```vb
Module Test
    Sub Main()
        ' The value of Address will be " St." since Name has not been
        ' assigned yet.
        Dim x As New Customer() With { .Address = .Name & " St." }
    End Sub
End Module
```

初期化子を入れ子になんだことができます。

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

作成される型がコレクション型とインスタンス メソッドの名前が場合`Add`(拡張メソッドと共有メソッド) を含むし、オブジェクト作成式を指定できます、キーワードでコレクション初期化子`From`します。 オブジェクト作成式には、メンバー初期化子とコレクション初期化子の両方を指定できません。 コレクション初期化子内の各要素がの呼び出しに引数として渡される、`Add`関数。 例えば:

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

要素がそれ自体コレクション初期化子である場合は、サブ コレクション初期化子の各要素は、個々 の引数として渡されます、`Add`関数。 次: たとえば、

```vb
Dim dict = Dictionary(Of Integer, String) From { { 1, "One" },{ 2, "Two" } }
```

上の式は、下の式と同等です。

```vb
Dim dict = New Dictionary(Of Integer, String)
dict.Add(1, "One")
dict.Add(2, "Two")
```

この拡張は常に実行し、1 レベル深い; はしか行われますその後、サブの初期化子は配列リテラルと見なされます。 例えば:

```vb
' Error: List(Of T) does not have an Add method that takes two parameters.
Dim list = New List(Of Integer())() From { { 1, 2 }, { 3, 4 } }

' OK, this initializes the dictionary with (Integer, Integer()) pairs.
Dim dict = New Dictionary(Of Integer, Integer())() From _
        { {  1, { 2, 3 } }, { 3, { 4, 5 } } }
```


### <a name="array-expressions"></a>配列式

配列式を使用して、配列型の新しいインスタンスを作成します。 配列式の 2 種類があります。 配列作成式では、および配列リテラル。

#### <a name="array-creation-expressions"></a>配列作成式

配列サイズ初期化修飾子は、指定した場合、個々 の引数の配列のサイズの初期化の引数リストから削除することで、結果の配列型が派生します。 各引数の値は、新しく割り当てられた配列のインスタンスに対応する次元の上限を決定します。 空のコレクション初期化子の式には、引数リストの各引数は、定数である必要があり、コレクション初期化子の式のリストで指定されたランクと次元の長さが一致する必要があります。

```vb
Dim a() As Integer = New Integer(2) {}
Dim b() As Integer = New Integer(2) { 1, 2, 3 }
Dim c(,) As Integer = New Integer(1, 2) { { 1, 2, 3 } , { 4, 5, 6 } }

' Error, length/initializer mismatch.
Dim d() As Integer = New Integer(2) { 0, 1, 2, 3 }
```

配列サイズ初期化修飾子が指定されていない場合、型名は、配列型である必要があり、コレクション初期化子が空にするか、含まれる指定された配列型のランクと入れ子のレベル数が同じ必要があります。 最も内側の入れ子レベル内の要素のすべての配列の要素の型に暗黙的に変換できる必要があり、値として分類する必要があります。 入れ子になったコレクション初期化子のそれぞれの要素の数は、常に同じレベルの他のコレクションのサイズと一致である必要があります。 各次元の長さは、コレクション初期化子の対応する入れ子のレベルの各要素の数から推論されます。 コレクション初期化子が空の場合は、各次元の長さは 0 です。

```vb
Dim e() As Integer = New Integer() { 1, 2, 3 }
Dim f(,) As Integer = New Integer(,) { { 1, 2, 3 } , { 4, 5, 6 } }

' Error: Inconsistent numbers of elements!
Dim g(,) As Integer = New Integer(,) { { 1, 2 }, { 4, 5, 6 } }

' Error: Inconsistent levels of nesting!
Dim h(,) As Integer = New Integer(,) { 1, 2, { 3, 4 } }
```

コレクション初期化子の最も外側の入れ子レベルは、配列の一番左のディメンションに対応し、最も内側の入れ子レベルが最も右にあるディメンションに対応します。 例:

```vb
Dim array As Integer(,) = _
    { { 0, 1 }, { 2, 3 }, { 4, 5 }, { 6, 7 }, { 8, 9 } }
```

次のと同等です。

```vb
Dim array(4, 1) As Integer

array(0, 0) = 0: array(0, 1) = 1
array(1, 0) = 2: array(1, 1) = 3
array(2, 0) = 4: array(2, 1) = 5
array(3, 0) = 6: array(3, 1) = 7
array(4, 0) = 8: array(4, 1) = 9
```

初期化される配列の次元が呼ばれるコレクション初期化子が空 (つまり、1 つを中かっこを含む) がない初期化子リストとの境界の場合は、空のコレクション初期化子は、指定したサイズの配列インスタンスを表します場所のすべての要素は、要素の型の既定値に初期化されています。 初期化される配列の次元の境界が不明な場合、空のコレクション初期化子は、すべてのディメンションのサイズがゼロ配列インスタンスを表します。

配列インスタンスのランクと各次元の長さは、インスタンスの有効期間にわたって定数。 つまり、既存の配列インスタンスのランクを変更することはできませんも、そのディメンションのサイズを変更することはできます。

#### <a name="array-literals"></a>配列リテラル

配列リテラルでは、ある要素の型、ランク、および境界は、式のコンテキストとコレクション初期化子の組み合わせから推論される配列を表します。 これは、セクションで説明[式の再分類](expressions.md#expression-reclassification)します。

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

例えば:

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

形式と、配列リテラル内のコレクション初期化子の要件はまったく同じと配列作成式で初期化子のコレクション。

__注意してください。__ 自体は、配列リテラルが、配列を作成できません。代わりに、作成される配列を原因となる値には、式の再分類になります。 変換では、`CType(new Integer() {1,2,3}, Short())`からの変換がないために、ことはできません`Integer()`に`Short()`; 式が、`CType({1,2,3},Short())`は、配列作成式に、配列リテラルを最初に再して分類ため可能です`New Short() {1,2,3}`.


### <a name="delegate-creation-expressions"></a>デリゲート作成式

デリゲート作成式はデリゲート型の新しいインスタンスを作成するために使用します。 デリゲート作成式の引数は、メソッドのポインターまたはメソッドのラムダに分類される式である必要があります。

引数がメソッドのポインターの場合、デリゲートのシグネチャに適用可能なメソッドのポインターによって参照されるメソッドのいずれかの必要があります。 メソッド`M`デリゲート型に適用できる`D`場合。

* `M` ない`Partial`本文が含まれるか。

* 両方`M`と`D`関数、または`D`サブルーチンです。

* `M` `D`同じ数のパラメーターがあります。

* パラメーターの型`M`の対応するパラメーターの型の型からの変換がある各`D`、およびその修飾子 (つまり`ByRef`、 `ByVal`) と一致します。

* 戻り値の型`M`の戻り値の型への変換がある存在する場合は、`D`します。

メソッドのポインターは、遅延バインディング アクセスを参照する場合をデリゲート型と同じ数のパラメーターを持つ関数に遅延バインディング アクセスが使用されます。

厳密な型が使用されていないとは、メソッドのポインターによって参照される 1 つだけのメソッドがパラメーターを持たないとデリゲート型は、そのメソッドは、該当すると見なされますのファクトとパラメーターまたは戻り値により適用可能でない場合、re は単に無視されます。 例えば:

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

__注意してください。__ この緩和したトークンは厳密な型が拡張メソッドのために使用されていない場合にのみ使用できます。 拡張メソッドは、通常のメソッドが適用できなかった場合にのみと見なされる、ために、インスタンス メソッドをデリゲート構築するためにパラメーターを持つ拡張メソッドを非表示にパラメーターのない可能性があります。

数より多い場合、メソッドのポインターによって参照される 1 つのメソッドは、デリゲート型に適用し、候補メソッド間での選択にオーバー ロードの解決を使用します。 デリゲートのパラメーターの型は、オーバー ロードの解決のための引数の型として使用されます。 最適な候補の 1 つのメソッドがない場合は、コンパイル時エラーが発生します。 次の例では、ローカル変数は、2 番目に参照するデリゲートで初期化`Square`メソッドの複数のシグネチャと戻り値の型に適用可能なため、`DoubleFunc`します。

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

2 番目の`Square`存在していないメソッドは、最初の`Square`メソッドが選択されています。 コンパイル環境または厳密な型が指定されて場合`Option Strict`メソッドのポインターによって参照される最も固有のメソッドがデリゲート シグネチャより狭い幅の場合、コンパイル時エラーが発生します。 メソッド`M`と見なされますが、デリゲート型よりも幅の狭い`D`場合。

* パラメーターの型の`M`の対応するパラメーターの型への拡大変換が`D`します。

* または、戻り値の型が存在する場合の`M`の戻り値の型への縮小変換が`D`します。

型引数がメソッドのポインターに関連付けられている場合は、同じ数の型引数を持つメソッドのみと見なされます。 型引数がメソッドのポインターに関連付けられていない場合は、型の推定は、ジェネリック メソッドの署名を照合するときに使用されます。 その他の通常の型推論とは異なり、デリゲートの戻り値の型が型引数を推論するときに使用されるがでも、戻り値の型は少なくともジェネリック オーバー ロードを決定する際に考慮されません。 次の例では、両方のデリゲート作成式に型引数を指定する方法を示します。

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

上記の例では、非ジェネリック デリゲート型がインスタンス化されたジェネリック メソッドを使用します。 ジェネリック メソッドを使用して構築されたデリゲート型のインスタンスを作成することもできます。 例えば:

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

デリゲート作成式に引数がラムダ メソッドである場合は、ラムダ メソッドがデリゲート型のシグネチャに該当する場合があります。 Lambda メソッド`L`デリゲート型に適用できる`D`場合。

* 場合`L`のパラメーターを持つ`D`が同じ数のパラメーター。 (場合`L`パラメーター、パラメーターを持たない`D`は無視されます)。

* パラメーターの型`L`の対応するパラメーターの型の型への変換がある各`D`、およびその修飾子 (つまり`ByRef`、 `ByVal`) と一致します。

* 場合`D`関数の場合、戻り値の型は、`L`の戻り値の型への変換が`D`します。 (場合`D`の戻り値、サブルーチン`L`は無視されます)。

パラメーターの型のパラメーターの場合`L`を省略するの対応するパラメーターの型`D`推論; 場合のパラメーターは、`L`配列または null 許容型名の修飾子が、コンパイル時エラーが発生します。 すべてのパラメーターの型の 1 回`L`が使用可能なメソッドのラムダ式の型を推論します。 例えば:

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

場合によっては、デリゲートのシグネチャが一致しないラムダ メソッドまたはメソッドのシグネチャ、.NET Framework がサポートしていませんデリゲートの作成ネイティブ。 そのような状況では、ラムダ メソッド式を使用して、2 つのメソッドを一致させる。 例えば:

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

デリゲート作成式の結果は、メソッドのポインター式から (ある場合) に関連付けられたターゲット式と一致するメソッドを参照するデリゲート インスタンスです。 値の型として、対象の式を入力すると、し、値の型がコピー システム ヒープにデリゲートが、ヒープ上のオブジェクトのメソッドをポイントできますのみためされます。 メソッドとデリゲートで参照するオブジェクトのまま、デリゲートの有効期間にわたって定数。 つまり、作成した後、デリゲートのオブジェクトまたは複数のターゲットを変更することはできません。

### <a name="anonymous-object-creation-expressions"></a>匿名のオブジェクト作成式

メンバー初期化子を含むオブジェクトの作成式を完全型名に省略できます。

```antlr
AnonymousObjectCreationExpression
    : 'New' ObjectMemberInitializer
    ;
```

その場合は、匿名型は、型と式の一部として初期化されるメンバーの名前に基づいて構築されます。 例えば:

```vb
Module Test
    Sub Main()
        Dim Customer = New With { .Name = "John Smith", .Age = 34 }

        Console.WriteLine(Customer.Name)
    End Sub
End Module
```

匿名のオブジェクト作成式によって作成された型は、名前を持たないから直接継承するクラス`Object`、メンバー初期化子リストに割り当てられているメンバーと同じ名前のプロパティのセットを持つとします。 ローカル変数の型の推論と同じ規則を使って、各プロパティの型が推論されます。 生成された匿名型をオーバーライドも`ToString`、すべてのメンバーとその値の文字列表現を返します。 (この文字列の正確な形式はこの仕様では扱いません) です。

既定では、匿名の型によって生成されたプロパティは、読み取り/書き込みです。 使用して読み取り専用として匿名型のプロパティをマークすることは、`Key`修飾子。 `Key`修飾子は、匿名型を表す値を一意に識別するフィールドを使用できることを指定します。 オーバーライドする匿名型が発生も読み取り専用プロパティを行う、だけでなく`Equals`と`GetHashCode`とインターフェイスを実装する`System.IEquatable(Of T)`(匿名型の入力`T`)。 メンバーの定義は次のとおりです。

`Function Equals(obj As Object) As Boolean` `Function Equals(val As T) As Boolean` 、同じ型の 2 つのインスタンスの検証と各を比較することによって実装される`Key`を使用してメンバー`Object.Equals`します。 すべて`Key`メンバーが等しいか、`Equals`を返します`True`それ以外の場合、`Equals`返します`False`。

`Function GetHashCode() As Integer` 実装されているように場合に`Equals`しは、匿名型の 2 つのインスタンスは true、`GetHashCode`は同じ値を返します。 ハッシュは開始シード値を持つとし、各`Key`メンバーは、順序が 31 をハッシュを乗算し、追加、`Key`メンバーのハッシュ値 (によって提供される`GetHashCode`) メンバーが参照型またはの値は、null許容値型ではない場合`Nothing`.

たとえば、ステートメントで作成した型:

```vb
Dim zipState = New With { Key .ZipCode = 98112, .State = "WA" }
```

次のような約 (ただし、正確な実装が異なる場合があります) クラスを作成します。

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

匿名型が別の型のフィールドから作成されているような状況を簡素化するには、フィールド名は、次の場合、式から直接推論できます。

* 簡易名式`x`名前を推測`x`します。

* メンバー アクセス式`x.y`名前を推測`y`します。

* ディクショナリ lookup 式`x!y`名前を推測`y`します。

* 引数なしでの呼び出しまたはインデックス式`x()`名前を推測`x`します。

* XML メンバー アクセス式`x.<y>`、 `x...<y>`、`x.@y`名前を推測`y`します。

* XML メンバー アクセス式、メンバー アクセス式の対象となっている`x.<y>.z`名前を推測`z`します。

* 引数なしでの呼び出しまたはインデックス式のターゲットである XML メンバー アクセス式`x.<y>.z()`名前を推測`z`します。

* XML メンバー アクセス式を呼び出し、またはインデックス式の対象となっている`x.<y>(0)`名前を推測`y`します。

初期化子は、推論された名前の式の割り当てとして解釈されます。 たとえば、次の初期化子は同等です。

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

など、型の既存のメンバーと競合するメンバーの名前が推論される場合`GetHashCode`コンパイル時のエラーが発生します。 通常のメンバー初期化子とは異なり匿名オブジェクト作成式を許可しないメンバー初期化子の循環参照、または初期化前に、メンバーを参照してください。 例えば:

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

匿名クラスの作成の 2 つの式は、同じメソッド内で発生し、プロパティの順序、プロパティ名、およびプロパティの型すべて一致 - 場合は、同一の結果として得られる図形--が生成する場合は両方を参照している同じ匿名クラス。 インスタンスまたは共有メンバー変数は初期化子でのメソッドのスコープは、変数を初期化するコンス トラクターです。

__注意してください。__ コンパイラは匿名を統一できます型さらに、このようなアセンブリ レベルでは、この時点で依存このことはできません。


## <a name="cast-expressions"></a>キャスト式

キャスト式は、式を指定した型を変換します。 特定のキャストのキーワードは、プリミティブ型に式を強制します。 次の 3 つの一般的な cast キーワード`CType`、`TryCast`と`DirectCast`型に式を強制します。

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

`DirectCast` `TryCast`特殊な動作があります。 このため、ネイティブ変換のみサポートします。 さらに、ターゲット型を`TryCast`式が値型にすることはできません。 ユーザー定義変換演算子ときに考慮されません`DirectCast`または`TryCast`使用されます。 (__に注意してください。__ 変換を設定する`DirectCast`と`TryCast`"ネイティブ CLR"変換を実装するためのサポートが制限されます。 目的は、`DirectCast`の目的は、中に、「ボックス化解除」命令の機能を提供する、 `TryCast` "isinst"命令の機能を提供することです。 手順については CLR 上に、マップ、ため、CLR によって直接サポートされている変換をサポートしているが損なわれる目的。)

`DirectCast` として型指定される式に変換します`Object`とは異なる`CType`します。 型の式を変換するときに`Object`が実行時の型がプリミティブ値型では、`DirectCast`がスローされます、`System.InvalidCastException`例外指定の型でない式の実行時の型と同じ場合、または`System.NullReferenceException`場合、式評価される`Nothing`します。 (__に注意してください。__ 前述のよう`DirectCast`CLR 命令に直接マップ「ボックス化解除」式の型が`Object`します。 これに対し、`CType`プリミティブ型間の変換をサポートできるように、変換するランタイム ヘルパーへの呼び出しに変換します。 場合と、`Object`式に変換されるプリミティブ値型と実際のインスタンスの一致の種類がターゲット型`DirectCast`よりも大幅に時間が短くなります`CType`)。

`TryCast` 式に変換しますが、式は、対象の型に変換できない場合、例外はスローされません。 代わりに、`TryCast`なります`Nothing`式は、実行時に変換できない場合。 (__に注意してください。__ 前述のよう`TryCast`"isinst"CLR 命令に直接マップされます。 型チェックと 1 つの操作に変換を組み合わせることで`TryCast`これよりも低コストにことができます、`TypeOf ... Is`をクリックし、 `CType`)。

例えば:

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

指定した型に式の型から変換が存在しない、コンパイル時エラーが発生します。 それ以外の場合、式は値として分類され、変換によって生成された値になります。


## <a name="operator-expressions"></a>演算子式

演算子の 2 種類があります。 *単項演算子*1 つのオペランドを使用して、プレフィックス表記を使用して (たとえば、 `-x`)。 *二項演算子*2 つのオペランドを取るし、挿入辞表記を使用して (たとえば、 `x + y`)。 関係演算子を除く結果が常に`Boolean`、その型の結果、特定の種類に対して定義された演算子。 演算子のオペランドは値として常に分類する必要があります。演算子の式の結果は、値として分類されます。

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

式には、複数のバイナリ演算子が含まれている場合、*優先順位*演算子の個々 のバイナリ演算子が評価される順序を制御します。 たとえば、式 `x + y * z` の評価は `x + (y * z)` ですが、これは `*` 演算子が `+` 演算子より高い優先順位だからです。 次の表では、優先順位の降順で二項演算子を示します。


| __カテゴリ__     | __演算子__                                          | 
|------------------|--------------------------------------------------------|
| 1 次式          | すべての非演算子式                           |
| Await             | `Await`                                                |
| 指数演算   | `^`                                                    |
| 単項マイナス符号   | `+`, `-`                                               |
| 乗法   | `*`, `/`                                               |
| 整数除算 | `\`                                                    |
| 剰余          | `Mod`                                                  |
| 加法         | `+`, `-`                                               |
| 連結    | `&`                                                    |
| シフト            | `<<`, `>>`                                             |
| 関係       | `=`, `<>`, `<`, `>`, `<=`, `>=`, `Like`, `Is`, `IsNot` |
| 論理 NOT      | `Not`                                                  |
| 論理 AND      | `And`, `AndAlso`                                       |
| 論理 OR       | `Or`, `OrElse`                                         |
| 論理 XOR      | `Xor`                                                  |

式では、同じ優先順位が 2 つの演算子が含まれている場合、*結合規則*演算子の操作が実行される順序を制御します。 すべてのバイナリ演算子は、左から右に操作を実行する意味左結合です。 優先順位と結合規則は、かっこで囲まれた式を使用して制御できます。

### <a name="object-operands"></a>オブジェクトのオペランド

すべての演算子が型のオペランドをサポートする各演算子でサポートされている通常の型だけでなく`Object`します。 適用される演算子`Object`オペランドは、上のメソッド呼び出しと同様に処理が`Object`値: である場合、コンパイル時の型ではなく、オペランドの実行時の型、有効性を判断、遅延バインディング メソッドの呼び出しを選択する場合があります操作の種類です。 コンパイル環境または厳密な型が指定されて場合`Option Strict`、型のオペランドの任意のオペレーター`Object`を除き、コンパイル時エラーが発生する、 `TypeOf...Is`、`Is`と`IsNot`演算子。

演算子の解決操作に対して、遅延バインディングを実行する必要があると判断した場合、操作の結果はオペランドの実行時の型が演算子でサポートされている型の場合は、オペランドの型に演算子を適用した結果を示します。 値`Nothing`は二項演算式でもう一方のオペランドの型の既定値として扱われます。 単項演算子式の場合、または両方のオペランドが`Nothing`二項演算子の式では、操作の種類が`Integer`または演算子、演算子にならない場合の唯一の結果型`Integer`します。 操作の結果が常に、キャストに戻す`Object`します。 オペランドの型で有効な演算子がない場合、`System.InvalidCastException`例外がスローされます。 実行時に変換が暗黙的または明示的なされるかどうかに関係なく行われます。

数値の二項演算の結果が生成されます (かどうかの整数オーバーフローのチェックが有効か無効) に関係なく、オーバーフロー例外、し、結果の型が昇格 [次へ] の幅の広い数値型に可能な場合。 次に例を示します。

```vb
Module Test
    Sub Main()
        Dim o As Object = CObj(CByte(2)) * CObj(CByte(255))

        Console.WriteLine(o.GetType().ToString() & " = " & o)
    End Sub
End Module
```

次の結果が出力されます。

```
System.Int16 = 512
```

幅の広い数値型が、数を保持するために使用できない場合、`System.OverflowException`例外がスローされます。

### <a name="operator-resolution"></a>演算子の解決

演算子の種類とオペランドのセットを指定するには、演算子の解決のオペランドを使用する演算子を決定します。 演算子を解決するときに、ユーザー定義演算子には、最初に、次の手順を使用して、考えられます。

1. 最初に、すべての候補演算子収集されます。 演算子の候補には、すべてのソースの種類で特定の演算子の種類のユーザー定義演算子、およびすべてのターゲットの型では、特定の種類のユーザー定義演算子です。 ソースの種類と変換先の型は関連して、一般的な演算子は 1 回考慮のみ。

2. 次に、オーバー ロードの解決は、最も固有の演算子を選択するには、演算子とオペランドに適用されます。 二項演算子は、の場合、遅延バインディング呼び出しにあります。

型の演算子の候補を収集するときに`T?`、型の演算子`T`代わりに使用されます。 いずれかの`T`のユーザー定義された null 非許容値型のみに関連する演算子が無効になることもできます。 リフトされた演算子を使用して、値の型の null 許容バージョン、例外での戻り値の型`IsTrue`と`IsFalse`(する必要があります`Boolean`)。 リフトされた演算子がオペランドを null 非許容のバージョンに変換すると、評価し、ユーザー定義演算子を評価し、変換した結果が、null 許容バージョンを入力します。 イーサ オペランドが場合`Nothing`、式の結果の値は、`Nothing`結果型の null 許容バージョンとして型指定します。 例えば:

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

演算子が二項演算子とオペランドの 1 つは、参照型、演算子が無効になることもが演算子の任意のバインディングには、エラーが生成されます。 例えば:

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

__注意してください。__ このルールは、ケースの場合は 2 種類の二項演算子の動作が変更は、将来のバージョンで参照型の null 値反映を追加するかどうかに考慮事項が発生したために存在します。

変換とユーザー定義演算子は常に優先リフトされた演算子よりです。

演算子はオーバー ロードを解決するときに Visual Basic で定義されているクラスとその他の言語で定義されている間の違いである可能性があります。

* 他の言語で`Not`、 `And`、および`Or`の両方として、論理演算子とビットごとの演算子はオーバー ロードできます。 外部アセンブリから、インポート時にいずれかの形式は、これらの演算子の有効なオーバー ロードとして承認されました。 ただし、論理とビットごとの演算子を定義する型の場合、ビットごとの実装のみが考慮されます。

* 他の言語で`>>`と`<<`の両方として、演算子の符号付きと符号なしの演算子はオーバー ロードできます。 外部アセンブリから、インポート時に、いずれかの形式は有効なオーバー ロードとして受け入れられます。 ただし、符号付きと符号なしの両方の演算子を定義する型の場合、署名付きの実装のみが考慮されます。

* ユーザー定義の演算子がオペランドに最も固有でない場合、組み込みの演算子を考慮します。 オペランドの組み込みの演算子が定義されていないと、いずれかのオペランドが Object 型の場合、オペレーターは解決されます遅延バインディング;それ以外の場合、コンパイル時エラーが発生します。

以前のバージョンの Visual Basic では、オブジェクト、型の 1 つのオペランドと該当するユーザー定義オペレーター、およびいない適用可能な組み込み演算子があった場合、エラーがあった。 Visual Basic の 11 の時点では、遅延バインディングを解決にされています。 例えば:

```vb
Module Module1
  Sub Main()
      Dim p As Object = Nothing
      Dim U As New Uri("http://www.microsoft.com")
      Dim j = U * p  ' is now resolved late-bound
   End Sub
End Module
```

型`T`を持つ組み込み演算子は、その同じ演算子も定義されています。`T?`します。 演算子の結果`T?`場合と同じになります`T`点を除き、いずれかのオペランドが場合`Nothing`、演算子の結果になります`Nothing`(つまり、null 値が反映されます)。 操作の型の解決のため、`?`が削除、設定されているオペランドから、操作の種類が決定されます、および`?`null 許容値型のいずれかのオペランドの場合、操作の種類に追加されます。 例えば:

```vb
Dim v1? As Integer = 10
Dim v2 As Long = 20

' Type of operation will be Long?
Console.WriteLine(v1 + v2)
```

各演算子には、組み込みの型に定義されていると実行される操作のオペランドの型を指定された型が一覧表示します。 組み込みの操作の種類の結果では、これらの一般的な規則に従います。

* すべてのオペランドが同じ型の型の演算子が定義されている場合は、変換は行われませんし、その型の演算子を使用します。

* 演算子の型が定義されていないいずれかのオペランドは、次の手順を使用して変換されますされ、演算子は、新しい型に対して解決されます。

  * オペランドは、演算子とオペランドの両方で定義されている次の最も幅の広い型で暗黙的に変換に変換されます。

  * このような型がない場合は、演算子とオペランドの両方で定義されている次の最も狭い型で暗黙的に変換し、オペランドは変換されます。

  * このような型がないか、変換が発生することはできません、コンパイル時エラーが発生します。

* オペランドを変換する場合は、オペランドの型とその型の演算子の幅が使用されます。 多くの演算子の種類より狭いオペランドの型を暗黙的に変換できません、コンパイル時エラーが発生します。

次の一般的な規則に関係なくただしがいくつかの特殊なケースが演算子の結果テーブルで呼び出されます。

__注意してください。__ 演算子の種類のテーブル上の理由から、書式設定する定義済みの名前を最初の 2 つの文字の省略形します。 「を」 `Byte`、"UI"は`UInteger`、"st"`String`など。「エラー」指定されたオペランドの型に対して定義されている操作がないことを意味します。

## <a name="arithmetic-operators"></a>算術演算子

`*`、 `/`、 `\`、 `^`、 `Mod`、 `+`、および`-`演算子は、*算術演算子*します。

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

浮動小数点算術演算子は、操作の結果の型よりも高い精度で実行できます。 一部のハードウェア アーキテクチャでの大きい範囲とより有効桁数を持つ「拡張」または"long double"浮動小数点型のサポートなど、 `Double` 「」暗黙的にこの高精度型を使用してすべての浮動小数点演算を実行します。 パフォーマンス負荷精度の低い浮動小数点演算を実行するハードウェア アーキテクチャを行んだことができます。パフォーマンスと精度の両方を犠牲にする必要があるのではなく Visual Basic ではすべての浮動小数点演算に使用する高精度の型。 正確な結果を提供する以外はこのことはほとんどありません、測定可能な影響を与えます。 ただし、フォームの式で`x * y / z`乗算が外部にある結果を生成します、`Double`範囲が、後続の除算に一時的な結果を表示、`Double`式であるという事実の範囲上の範囲で評価形式無限大ではなく生成する有限の結果が発生する可能性があります。


### <a name="unary-plus-operator"></a>単項プラス演算子

```antlr
UnaryPlusExpression
    : '+' Expression
    ;
```

単項プラス演算子の定義は、 `Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、 `ULong`、 `Long`、 `Single`、 `Double`、および`Decimal`型.

__操作の種類:__


| __bo__ | __SB__ | __によって__ | __Sh__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __de__ | __si__ | __Do__ | __Da__  | __ch__  | __st__ | __Ob__ | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| Sh | SB | By | Sh | US | イン | UI | Lo | UL | De | Si | Do | Err | Err | Do | Ob | 


### <a name="unary-minus-operator"></a>単項マイナス演算子

```antlr
UnaryMinusExpression
    : '-' Expression
    ;
```

単項マイナス演算子は、次の種類に対して定義されます。

`SByte`、 `Short`、 `Integer`、および `Long`。 結果は、0 からオペランドを減算して計算されます。 整数のオーバーフロー チェックが入っていて、オペランドの値が負の最大場合`SByte`、 `Short`、 `Integer`、または`Long`、`System.OverflowException`例外がスローされます。 それ以外の場合、オペランドの値が負の最大値の場合`SByte`、 `Short`、 `Integer`、または`Long`結果は同じ値をおよび、オーバーフローが報告されません。

`Single` および `Double`。 結果は、反転させて、その記号の値 0 と無限大オペランドの値です。 オペランドが NaN の場合、結果も NaN にします。

`Decimal`。 結果は、0 からオペランドを減算して計算されます。

__操作の種類:__

| __bo__ | __SB__ | __によって__ | __Sh__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __de__ | __si__ | __Do__ | __Da__  | __ch__  | __st__ | __Ob__ | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| Sh | SB | Sh | Sh | イン | イン | Lo | Lo | De | De | Si | Do | Err | Err | Do | Ob | 


### <a name="addition-operator"></a>加算演算子

加算演算子は、2 つのオペランドの合計を計算します。

```antlr
AdditionOperatorExpression
    : Expression '+' LineTerminator? Expression
    ;
```

加算演算子は、次の種類に対して定義されます。

* `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、および `Long`。 整数のオーバーフロー チェックが入っていて、結果の型の範囲外の合計が場合、`System.OverflowException`例外がスローされます。 それ以外の場合、オーバーフローが報告していないと、結果の任意の大きな上位ビットが破棄されます。

* `Single` および `Double`。 合計は、IEEE 754 の演算の規則に従って計算されます。

* `Decimal`。 10 進数の形式で表すには、結果の値が大きすぎる場合、`System.OverflowException`例外がスローされます。 結果の値が 10 進数形式で表す小さすぎる場合、結果は 0 です。

* `String`。 2 つ`String`オペランドを連結します。

* `Date`。 `System.DateTime`型オーバー ロードされた加算演算子を定義します。 `System.DateTime`は、組み込みの等価`Date`型では、これらの演算子はできるも、`Date`型。

__操作の種類:__

|        | __bo__ | __SB__ | __によって__ | __Sh__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __de__ | __si__ | __Do__ | __Da__  | __ch__  | __st__ | __Ob__ | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| __bo__ | Sh | SB | Sh | Sh | イン | イン | Lo | Lo | De | De | Si | Do | Err | Err | Do | Ob | 
| __SB__ |    | SB | Sh | Sh | イン | イン | Lo | Lo | De | De | Si | Do | Err | Err | Do | Ob | 
| __によって__ |    |    | By | Sh | US | イン | UI | Lo | UL | De | Si | Do | Err | Err | Do | Ob | 
| __Sh__ |    |    |    | Sh | イン | イン | Lo | Lo | De | De | Si | Do | Err | Err | Do | Ob | 
| __米国__ |    |    |    |    | US | イン | UI | Lo | UL | De | Si | Do | Err | Err | Do | Ob | 
| __In__ |    |    |    |    |    | イン | Lo | Lo | De | De | Si | Do | Err | Err | Do | Ob | 
| __UI__ |    |    |    |    |    |    | UI | Lo | UL | De | Si | Do | Err | Err | Do | Ob | 
| __Lo__ |    |    |    |    |    |    |    | Lo | De | De | Si | Do | Err | Err | Do | Ob | 
| __UL__ |    |    |    |    |    |    |    |    | UL | De | Si | Do | Err | Err | Do | Ob | 
| __de__ |    |    |    |    |    |    |    |    |    | De | Si | Do | Err | Err | Do | Ob | 
| __si__ |    |    |    |    |    |    |    |    |    |    | Si | Do | Err | Err | Do | Ob | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Do | Err | Err | Do | Ob | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | st  | Err | st | Ob | 
| __ch__ |    |    |    |    |    |    |    |    |    |    |    |    |     | st  | st | Ob | 
| __st__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | st | Ob | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |    | Ob | 


### <a name="subtraction-operator"></a>減算演算子

減算演算子は、最初のオペランドから 2 番目のオペランドを減算します。

```antlr
SubtractionOperatorExpression
    : Expression '-' LineTerminator? Expression
    ;
```

減算演算子は、次の種類に対して定義されます。

* `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、および `Long`。 整数オーバーフローのチェックと違いは、結果の型の範囲外の場合、`System.OverflowException`例外がスローされます。 それ以外の場合、オーバーフローが報告していないと、結果の任意の大きな上位ビットが破棄されます。

* `Single` および `Double`。 違いは、IEEE 754 の演算の規則に従って計算されます。

* `Decimal`。 10 進数の形式で表すには、結果の値が大きすぎる場合、`System.OverflowException`例外がスローされます。 結果の値が 10 進数形式で表す小さすぎる場合、結果は 0 です。

* `Date`。 `System.DateTime`型オーバー ロードされた減算演算子を定義します。 `System.DateTime`は、組み込みの等価`Date`型では、これらの演算子はできるも、`Date`型。

__操作の種類:__

|        | __bo__ | __SB__ | __によって__ | __Sh__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __de__ | __si__ | __Do__ | __Da__  | __ch__  | __st__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __bo__ | Sh | SB | Sh | Sh | イン | イン | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __SB__ |    | SB | Sh | Sh | イン | イン | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __によって__ |    |    | By | Sh | US | イン | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __Sh__ |    |    |    | Sh | イン | イン | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __米国__ |    |    |    |    | US | イン | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __In__ |    |    |    |    |    | イン | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __UI__ |    |    |    |    |    |    | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __de__ |    |    |    |    |    |    |    |    |    | De | Si | Do | Err | Err | Do  | Ob  | 
| __si__ |    |    |    |    |    |    |    |    |    |    | Si | Do | Err | Err | Do  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Do | Err | Err | Do  | Ob  | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __ch__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __st__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Do  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


### <a name="multiplication-operator"></a>乗算演算子

乗算演算子は、2 つのオペランドの積を計算します。

```antlr
MultiplicationOperatorExpression
    : Expression '*' LineTerminator? Expression
    ;
```

乗算演算子は、次の種類に対して定義されます。

* `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、および `Long`。 整数オーバーフローのチェックでは、製品は結果の型の範囲外の場合、`System.OverflowException`例外がスローされます。 それ以外の場合、オーバーフローが報告していないと、結果の任意の大きな上位ビットが破棄されます。

* `Single` および `Double`。 製品は、IEEE 754 の演算の規則に従って計算されます。

* `Decimal`。 10 進数の形式で表すには、結果の値が大きすぎる場合、`System.OverflowException`例外がスローされます。 結果の値が 10 進数形式で表す小さすぎる場合、結果は 0 です。

__操作の種類:__

|        | __bo__ | __SB__ | __によって__ | __Sh__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __de__ | __si__ | __Do__ | __Da__  | __ch__  | __st__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __bo__ | Sh | SB | Sh | Sh | イン | イン | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __SB__ |    | SB | Sh | Sh | イン | イン | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __によって__ |    |    | By | Sh | US | イン | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __Sh__ |    |    |    | Sh | イン | イン | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __米国__ |    |    |    |    | US | イン | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __In__ |    |    |    |    |    | イン | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __UI__ |    |    |    |    |    |    | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __de__ |    |    |    |    |    |    |    |    |    | De | Si | Do | Err | Err | Do  | Ob  | 
| __si__ |    |    |    |    |    |    |    |    |    |    | Si | Do | Err | Err | Do  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Do | Err | Err | Do  | Ob  | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __ch__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __st__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Do  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


### <a name="division-operators"></a>除算演算子

除算演算子は、2 つのオペランドの商を計算します。 2 つの除算演算子があります。 通常の (浮動小数点) 除算演算子および整数の除算演算子。

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

通常の除算演算子は、次の種類に対して定義されます。

* `Single` および `Double`。 商は、IEEE 754 の演算の規則に従って計算されます。

* `Decimal`。 右側のオペランドの値が 0 の場合、`System.DivideByZeroException`例外がスローされます。 10 進数の形式で表すには、結果の値が大きすぎる場合、`System.OverflowException`例外がスローされます。 結果の値が 10 進数形式で表す小さすぎる場合、結果は 0 です。 丸めを行う前に、結果の小数点以下桁数は、最も近いスケール優先の小数点以下桁数が、正確な結果と等しく、結果を保持します。  推奨されるスケールは、2 番目のオペランドの小数点以下桁数以下の最初のオペランドの小数点以下桁数です。

通常の演算子の解決規則、通常の除算などの型のオペランドの間で純粋に従って`Byte`、 `Short`、 `Integer`、および`Long`型に変換する両方のオペランドとなる`Decimal`します。 ただし、どちらの種類がときに除算演算子での解決に演算子を行って`Decimal`、`Double`より狭い幅と見なされます`Decimal`します。 ため、この規則に従った`Double`部門がよりも効率的です。`Decimal`除算します。

__操作の種類:__

|        | __bo__ | __SB__ | __によって__ | __Sh__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __de__ | __si__ | __Do__ | __Da__  | __ch__  | __st__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __bo__ | Do | Do | Do | Do | Do | Do | Do | Do | Do | De | Si | Do | Err | Err | Do  | Ob  | 
| __SB__ |    | Do | Do | Do | Do | Do | Do | Do | Do | De | Si | Do | Err | Err | Do  | Ob  | 
| __によって__ |    |    | Do | Do | Do | Do | Do | Do | Do | De | Si | Do | Err | Err | Do  | Ob  | 
| __Sh__ |    |    |    | Do | Do | Do | Do | Do | Do | De | Si | Do | Err | Err | Do  | Ob  | 
| __米国__ |    |    |    |    | Do | Do | Do | Do | Do | De | Si | Do | Err | Err | Do  | Ob  | 
| __In__ |    |    |    |    |    | Do | Do | Do | Do | De | Si | Do | Err | Err | Do  | Ob  | 
| __UI__ |    |    |    |    |    |    | Do | Do | Do | De | Si | Do | Err | Err | Do  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Do | Do | De | Si | Do | Err | Err | Do  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | Do | De | Si | Do | Err | Err | Do  | Ob  | 
| __de__ |    |    |    |    |    |    |    |    |    | De | Si | Do | Err | Err | Do  | Ob  | 
| __si__ |    |    |    |    |    |    |    |    |    |    | Si | Do | Err | Err | Do  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Do | Err | Err | Do  | Ob  | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __ch__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __st__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Do  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 

整数の除算演算子が定義されて`Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、 `ULong`、および`Long`します。 右側のオペランドの値が 0 の場合、`System.DivideByZeroException`例外がスローされます。 除算を 0 方向に結果を丸めます、結果の絶対値は、2 つのオペランドの商の絶対値より小さい最大整数。 結果は、2 つのオペランドが同じの符号を持つと 0 または負の記号の反対側の 2 つのオペランドがときに 0 または正の値。 場合は、左側のオペランドが負の最大`SByte`、 `Short`、 `Integer`、または`Long`、および右のオペランドが`-1`、オーバーフローが発生します。 場合、整数のオーバーフロー チェックを`System.OverflowException`例外がスローされます。 それ以外の場合、オーバーフローが報告されないと、結果は、左側のオペランドの値では代わりにします。

__注意してください。__ 符号なしの型の 2 つのオペランドは常に 0 または正の場合、結果は常に 0 または正の値。  式の結果では、以下に、最大 2 つのオペランドには常のオーバーフローが発生することはできません。  そのための 2 つの符号なし整数値で整数除算整数オーバーフローのチェックは実行されません。 左側のオペランドの型になります。


__操作の種類:__

|        | __bo__ | __SB__ | __によって__ | __Sh__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __de__ | __si__ | __Do__ | __Da__  | __ch__  | __st__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __bo__ | Sh | SB | Sh | Sh | イン | イン | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __SB__ |    | SB | Sh | Sh | イン | イン | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __によって__ |    |    | By | Sh | US | イン | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Sh__ |    |    |    | Sh | イン | イン | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __米国__ |    |    |    |    | US | イン | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __In__ |    |    |    |    |    | イン | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __UI__ |    |    |    |    |    |    | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __de__ |    |    |    |    |    |    |    |    |    | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __si__ |    |    |    |    |    |    |    |    |    |    | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Lo | Err | Err | Lo  | Ob  | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __ch__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __st__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Lo  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


### <a name="mod-operator"></a>Mod 演算子

`Mod` (剰余) 演算子には、2 つのオペランドの除算の剰余を計算します。

```antlr
ModuloOperatorExpression
    : Expression 'Mod' LineTerminator? Expression
    ;
```

`Mod`演算子は、次の種類の定義します。

* `Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、`ULong`と`Long`します。 結果`x Mod y`によって値が生成される`x - (x \ y) * y`します。 場合`y`0 の場合は、`System.DivideByZeroException`例外がスローされます。 剰余演算子、オーバーフローを引き起こすことはありません。

* `Single` および `Double`。 残りの部分は、IEEE 754 の演算の規則に従って計算されます。

* `Decimal`。 右側のオペランドの値が 0 の場合、`System.DivideByZeroException`例外がスローされます。 10 進数の形式で表すには、結果の値が大きすぎる場合、`System.OverflowException`例外がスローされます。 結果の値が 10 進数形式で表す小さすぎる場合、結果は 0 です。

__操作の種類:__

|        | __bo__ | __SB__ | __によって__ | __Sh__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __de__ | __si__ | __Do__ | __Da__  | __ch__  | __st__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __bo__ | Sh | SB | Sh | Sh | イン | イン | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __SB__ |    | SB | Sh | Sh | イン | イン | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __によって__ |    |    | By | Sh | US | イン | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __Sh__ |    |    |    | Sh | イン | イン | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __米国__ |    |    |    |    | US | イン | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __In__ |    |    |    |    |    | イン | Lo | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __UI__ |    |    |    |    |    |    | UI | Lo | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | De | De | Si | Do | Err | Err | Do  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | UL | De | Si | Do | Err | Err | Do  | Ob  | 
| __de__ |    |    |    |    |    |    |    |    |    | De | Si | Do | Err | Err | Do  | Ob  | 
| __si__ |    |    |    |    |    |    |    |    |    |    | Si | Do | Err | Err | Do  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Do | Err | Err | Do  | Ob  | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __ch__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __st__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Do  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


### <a name="exponentiation-operator"></a>指数演算子

指数演算子は、最初のオペランドが 2 番目のオペランドの累乗を計算します。

```antlr
ExponentOperatorExpression
    : Expression '^' LineTerminator? Expression
    ;
```

型の累乗演算子が定義されている`Double`します。 値は、IEEE 754 の演算の規則に従って計算されます。

__操作の種類:__

|        | __bo__ | __SB__ | __によって__ | __Sh__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __de__ | __si__ | __Do__ | __Da__  | __ch__  | __st__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __bo__ | Do | Do | Do | Do | Do | Do | Do | Do | Do | Do | Do | Do | Err | Err | Do  | Ob  | 
| __SB__ |    | Do | Do | Do | Do | Do | Do | Do | Do | Do | Do | Do | Err | Err | Do  | Ob  | 
| __によって__ |    |    | Do | Do | Do | Do | Do | Do | Do | Do | Do | Do | Err | Err | Do  | Ob  | 
| __Sh__ |    |    |    | Do | Do | Do | Do | Do | Do | Do | Do | Do | Err | Err | Do  | Ob  | 
| __米国__ |    |    |    |    | Do | Do | Do | Do | Do | Do | Do | Do | Err | Err | Do  | Ob  | 
| __In__ |    |    |    |    |    | Do | Do | Do | Do | Do | Do | Do | Err | Err | Do  | Ob  | 
| __UI__ |    |    |    |    |    |    | Do | Do | Do | Do | Do | Do | Err | Err | Do  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Do | Do | Do | Do | Do | Err | Err | Do  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | Do | Do | Do | Do | Err | Err | Do  | Ob  | 
| __de__ |    |    |    |    |    |    |    |    |    | Do | Do | Do | Err | Err | Do  | Ob  | 
| __si__ |    |    |    |    |    |    |    |    |    |    | Do | Do | Err | Err | Do  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Do | Err | Err | Do  | Ob  | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __ch__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __st__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Do  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


## <a name="relational-operators"></a>関係演算子

*関係演算子*他の 2 つの値を比較します。 比較演算子は`=`、 `<>`、 `<`、 `>`、 `<=`、および`>=`します。

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

関係演算子のすべてになります、`Boolean`値。

関係演算子では、次の一般的な意味があります。

* `=`演算子は 2 つのオペランドが等しいかどうかをテストします。

* `<>`演算子は 2 つのオペランドが等しくないかどうかをテストします。

* `<`演算子は、最初のオペランドが小さいかどうかをテストします。 2 番目のオペランドよりもします。

* `>`演算子は最初のオペランドが 2 番目のオペランドより大きいかどうかをテストします。

* `<=`演算子は最初のオペランドが 2 番目のオペランド以下かどうかをテストします。

* `>=`演算子は最初のオペランドが 2 番目のオペランド以上かどうかをテストします。

次の種類は、関係演算子が定義されています。

* `Boolean`。 演算子は、2 つのオペランドの真の値を比較します。 `True` あると見なされますより小さい`False`、その数値の値と一致します。

* `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、および `Long`。 演算子は、2 つの整数オペランドの数値を比較します。

* `Single` および `Double`。 演算子は、IEEE 754 標準の規則に従ってオペランドを比較します。

* `Decimal`。 演算子は、2 つの 10 進数のオペランドの数値を比較します。

* `Date`。 演算子は、2 つの日付/時刻値を比較した結果を返します。

* `Char`。 演算子は、2 つの Unicode 値を比較した結果を返します。

* `String`。 演算子は、二項比較またはテキスト比較を使用して 2 つの値を比較した結果を返します。 使用する比較は、コンパイル環境によって決定されます、`Option Compare`ステートメント。 バイナリ比較では、各文字列内の各文字の Unicode 数値が同じかどうかを判断します。 文字比較では、.NET Framework で使用中の現在のカルチャに基づいて、Unicode テキスト比較します。 文字列比較を行うと、null 値は文字列リテラルと同じ`""`します。

__操作の種類:__
        
|        | __bo__ | __SB__ | __によって__ | __Sh__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __de__ | __si__ | __Do__ | __Da__  | __ch__  | __st__ | __Ob__ | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| __bo__ | bo | SB | Sh | Sh | イン | イン | Lo | Lo | De | De | Si | Do | Err | Err | bo | Ob | 
| __SB__ |    | SB | Sh | Sh | イン | イン | Lo | Lo | De | De | Si | Do | Err | Err | Do | Ob | 
| __によって__ |    |    | By | Sh | US | イン | UI | Lo | UL | De | Si | Do | Err | Err | Do | Ob | 
| __Sh__ |    |    |    | Sh | イン | イン | Lo | Lo | De | De | Si | Do | Err | Err | Do | Ob | 
| __米国__ |    |    |    |    | US | イン | UI | Lo | UL | De | Si | Do | Err | Err | Do | Ob | 
| __In__ |    |    |    |    |    | イン | Lo | Lo | De | De | Si | Do | Err | Err | Do | Ob | 
| __UI__ |    |    |    |    |    |    | UI | Lo | UL | De | Si | Do | Err | Err | Do | Ob | 
| __Lo__ |    |    |    |    |    |    |    | Lo | De | De | Si | Do | Err | Err | Do | Ob | 
| __UL__ |    |    |    |    |    |    |    |    | UL | De | Si | Do | Err | Err | Do | Ob | 
| __de__ |    |    |    |    |    |    |    |    |    | De | Si | Do | Err | Err | Do | Ob | 
| __si__ |    |    |    |    |    |    |    |    |    |    | Si | Do | Err | Err | Do | Ob | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Do | Err | Err | Do | Ob | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | Da  | Err | Da | Ob | 
| __ch__ |    |    |    |    |    |    |    |    |    |    |    |    |     | ch  | st | Ob | 
| __st__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | st | Ob | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |    | Ob | 


## <a name="like-operator"></a>Like 演算子

`Like`演算子が文字列に指定されたパターンが一致するかどうかを判断します。

```antlr
LikeOperatorExpression
    : Expression 'Like' LineTerminator? Expression
    ;
```

`Like`演算子が定義されて、`String`型。 最初のオペランドが一致する文字列と 2 番目のオペランドは照合するパターンです。 パターンは Unicode 文字で構成をされます。 次の文字シーケンスは、特別な意味を持ちます。

* 文字`?`任意の 1 文字と一致します。

* 文字`*`0 個以上の文字と一致します。

* 文字`#`1 つの数字 (0 ~ 9) と一致します。

* 角かっこで囲まれた文字の一覧 (`[ab...]`) の一覧で任意の 1 文字と一致します。

* 文字の一覧が角かっこで囲まれているし、感嘆符が付いて (`[!ab...]`) 文字の一覧にない任意の 1 文字と一致します。

* ハイフンで区切られた一連の文字の 2 文字 (`-`) の範囲の Unicode 文字の最初の文字で始まり、2 番目の文字で終わるを指定します。 2 番目の文字でない場合、最初の文字よりも並べ替え順序で後で、実行時例外が発生します。 先頭または末尾の文字の一覧に表示されるハイフンでは、それ自体を指定します。

特殊文字の左角かっこの一致するように (`[`)、疑問符 (`?`)、シャープ記号 (`#`)、およびアスタリスク (`*`)、角かっこが囲む必要があります。 右の角かっこ (`]`) 自体には、一致するように、グループ内で使用できませんが、グループ外で個別の文字として使用することができます。 文字シーケンス`[]`文字列リテラルと見なされます`""`します。 

文字の比較と、文字リストの順序付けに使用する比較の種類に依存していることに注意してください。 バイナリの比較を使用している場合文字の比較と順序付けが数値の Unicode 値に対するベースします。 テキスト比較を使用している場合、.NET Framework で使用されている現在のロケールの文字の比較と順序付けがベースします。

言語によっては、特殊文字、アルファベット表す 2 つの文字、またはその逆です。 たとえば、複数の言語が文字を使用して`æ`の文字を表す`a`と`e`表示された場合に、文字の中に`^`と`O`の文字を表すために使用できます`Ô`. テキスト比較を行うときに、`Like`演算子は、このような同等のカルチャを認識します。 その場合は、パターンまたは文字列のいずれかで 1 つの特殊文字の発生は、その他の文字列で同等の 2 文字のシーケンスと一致します。 同様に、1 つの特殊文字角かっこで囲まれたパターンと一致と等価の 2 文字のシーケンスを文字列、またはその逆を (単独で、リスト、または範囲内)。

`Like`式の両方のオペランドが`Nothing`1 つのオペランドがへの組み込みの変換または`String`し、もう一方のオペランドが`Nothing`、`Nothing`は、空の文字列リテラルの場合と同様に扱われます`""`します。

__操作の種類:__

|        | __bo__ | __SB__ | __によって__ | __Sh__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __de__ | __si__ | __Do__ | __Da__  | __ch__  | __st__ | __Ob__ | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| __bo__ | st | st | st | st | st | st | st | st | st | st | st | st | st | st | st | Ob | 
| __SB__ |    | st | st | st | st | st | st | st | st | st | st | st | st | st | st | Ob | 
| __によって__ |    |    | st | st | st | st | st | st | st | st | st | st | st | st | st | Ob | 
| __Sh__ |    |    |    | st | st | st | st | st | st | st | st | st | st | st | st | Ob | 
| __米国__ |    |    |    |    | st | st | st | st | st | st | st | st | st | st | st | Ob | 
| __In__ |    |    |    |    |    | st | st | st | st | st | st | st | st | st | st | Ob | 
| __UI__ |    |    |    |    |    |    | st | st | st | st | st | st | st | st | st | Ob | 
| __Lo__ |    |    |    |    |    |    |    | st | st | st | st | st | st | st | st | Ob | 
| __UL__ |    |    |    |    |    |    |    |    | st | st | st | st | st | st | st | Ob | 
| __de__ |    |    |    |    |    |    |    |    |    | st | st | st | st | st | st | Ob | 
| __si__ |    |    |    |    |    |    |    |    |    |    | st | st | st | st | st | Ob | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | st | st | st | st | Ob | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | st | st | st | Ob | 
| __ch__ |    |    |    |    |    |    |    |    |    |    |    |    |    | st | st | Ob | 
| __st__ |    |    |    |    |    |    |    |    |    |    |    |    |    |    | st | Ob | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    | Ob | 


## <a name="concatenation-operator"></a>連結演算子

```antlr
ConcatenationOperatorExpression
    : Expression '&' LineTerminator? Expression
    ;
```

*連結演算子*に対してすべての組み込みの値の型の null 許容バージョンを含む、組み込み型が定義されています。 上記で説明した型の間の連結用も定義されていると`System.DBNull`、として扱われる、`Nothing`文字列。 連結演算子では、すべてのオペランドに変換します`String`; へのすべての変換、式に`String`は厳密な型を使用するかどうかに関係なく、拡大変換と見なされます。 A`System.DBNull`リテラルに変換される値`Nothing`として型指定された`String`します。 値が null 許容値型`Nothing`リテラルに変換も`Nothing`として型指定された`String`実行時エラーがスローされるのではなく。

連結演算の結果を 2 つのオペランドが左から右への順序で連結した文字列にします。 値`Nothing`は、空の文字列リテラルの場合と同様に扱われます`""`します。

__操作の種類:__

|        | __bo__ | __SB__ | __によって__ | __Sh__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __de__ | __si__ | __Do__ | __Da__  | __ch__  | __st__ | __Ob__ | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| __bo__ | st | st | st | st | st | st | st | st | st | st | st | st | st | st | st | Ob | 
| __SB__ |    | st | st | st | st | st | st | st | st | st | st | st | st | st | st | Ob | 
| __によって__ |    |    | st | st | st | st | st | st | st | st | st | st | st | st | st | Ob | 
| __Sh__ |    |    |    | st | st | st | st | st | st | st | st | st | st | st | st | Ob | 
| __米国__ |    |    |    |    | st | st | st | st | st | st | st | st | st | st | st | Ob | 
| __In__ |    |    |    |    |    | st | st | st | st | st | st | st | st | st | st | Ob | 
| __UI__ |    |    |    |    |    |    | st | st | st | st | st | st | st | st | st | Ob | 
| __Lo__ |    |    |    |    |    |    |    | st | st | st | st | st | st | st | st | Ob | 
| __UL__ |    |    |    |    |    |    |    |    | st | st | st | st | st | st | st | Ob | 
| __de__ |    |    |    |    |    |    |    |    |    | st | st | st | st | st | st | Ob | 
| __si__ |    |    |    |    |    |    |    |    |    |    | st | st | st | st | st | Ob | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | st | st | st | st | Ob | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | st | st | st | Ob | 
| __ch__ |    |    |    |    |    |    |    |    |    |    |    |    |    | st | st | Ob | 
| __st__ |    |    |    |    |    |    |    |    |    |    |    |    |    |    | st | Ob | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    | Ob | 


## <a name="logical-operators"></a>論理演算子

`And`、 `Not`、 `Or`、および`Xor`演算子は論理演算子と呼ばれます。

```antlr
LogicalOperatorExpression
    : 'Not' Expression
    | Expression 'And' LineTerminator? Expression
    | Expression 'Or' LineTerminator? Expression
    | Expression 'Xor' LineTerminator? Expression
    ;
```

論理演算子は、次のように評価されます。

* `Boolean`型。

  * 論理`And`2 つのオペランドに対して操作を実行します。

  * 論理`Not`オペランドに対して操作を実行します。

  * 論理`Or`2 つのオペランドに対して操作を実行します。

  * 論理の排他-`Or` 2 つのオペランドに対して操作を実行します。

* `Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、 `ULong`、 `Long`、すべての列挙型のバイナリ表現の各ビットに対して指定された操作が実行を2 つのオペランド。

  * `And`: 結果のビットが 1 の場合、両方のビットが 1 になります。それ以外の場合、結果のビットは 0 です。

  * `Not`: 結果のビットが 1 のビットが 0 以外の場合それ以外の場合、結果のビットは 1 です。

  * `Or`: 結果のビットが 1 のいずれかのビットが 1 以外の場合それ以外の場合、結果のビットは 0 です。

  * `Xor`: 結果のビットが 1 の場合、いずれかのビットは 1 が両方のビットです。それ以外の場合、結果のビットは 0 です (つまり、1 `Xor` 0 = 1, 1 `Xor` 1 = 0)。

* ときに、論理演算子`And`と`Or`の種類が無効になる`Boolean?`、ブール ロジックの 3 つの値として含まれるように拡張します。

  * `And` 両方のオペランドが true である場合は true に評価されます。false の場合は、オペランドの 1 つは false です。`Nothing`それ以外の場合。

  * `Or` どちらかのオペランドが true である場合は true に評価されます。false は両方のオペランドが false です。`Nothing`それ以外の場合。

例えば:

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

__注意してください。__ 理想的には、論理演算子`And`と`Or`3 値ロジックを使用して、ブール式で使用できる任意の型のリフトが (実装する型つまり`IsTrue`と`IsFalse`)、同じ方法`AndAlso``OrElse`ブール式で使用できる任意の型の間でのショート サーキットします。 残念ながら、3 つの値を持ち上げるにのみ適用`Boolean?`3 値ロジックを求めているユーザー定義の型が定義することで手動で行う必要がありますので、`And`と`Or`null 許容のバージョンの演算子。

オーバーフローは、これらの操作からではありません。 列挙型の演算子は、列挙型の基になる型でビットごとの演算が、戻り値は列挙型です。

__ない操作の種類:__

| __bo__ | __SB__ | __によって__ | __Sh__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __de__ | __si__ | __Do__ | __Da__  | __ch__  | __st__ | __Ob__ | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| bo | SB | By | Sh | US | イン | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo | Ob | 

__または Xor 操作の種類。__

|        | __bo__ | __SB__ | __によって__ | __Sh__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __de__ | __si__ | __Do__ | __Da__  | __ch__  | __st__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __bo__ | bo | SB | Sh | Sh | イン | イン | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | bo  | Ob  | 
| __SB__ |    | SB | Sh | Sh | イン | イン | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __によって__ |    |    | By | Sh | US | イン | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Sh__ |    |    |    | Sh | イン | イン | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __米国__ |    |    |    |    | US | イン | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __In__ |    |    |    |    |    | イン | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __UI__ |    |    |    |    |    |    | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __de__ |    |    |    |    |    |    |    |    |    | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __si__ |    |    |    |    |    |    |    |    |    |    | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Lo | Err | Err | Lo  | Ob  | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __ch__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __st__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Lo  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


### <a name="short-circuiting-logical-operators"></a>ショート サーキット論理演算子

`AndAlso`と`OrElse`演算子はショート サーキットのバージョンの`And`と`Or`論理演算子です。

```antlr
ShortCircuitLogicalOperatorExpression
    : Expression 'AndAlso' LineTerminator? Expression
    | Expression 'OrElse' LineTerminator? Expression
    ;
```

ショート サーキット動作、ため 2 番目のオペランドは評価されません実行時に最初のオペランドを評価した後、演算子の結果がわかっている場合。

ショート サーキット論理演算子は、次のように評価されます。

* 場合、最初のオペランドで、`AndAlso`操作の評価に`False`から True を返しますまたはその`IsFalse`演算子、式は、最初のオペランドを返します。 それ以外の場合、2 番目のオペランドが評価され、論理`And`2 つの結果に対して操作を実行します。

* 場合、最初のオペランドで、`OrElse`操作の評価に`True`から True を返しますまたはその`IsTrue`演算子、式は、最初のオペランドを返します。 それ以外の場合、2 番目のオペランドが評価され、論理`Or`その 2 つの結果に対して操作を実行します。

`AndAlso`と`OrElse`型の演算子が定義されている`Boolean`、または任意の型の`T`次の演算子をオーバー ロードします。

```vb
Public Shared Operator IsTrue(op As T) As Boolean
Public Shared Operator IsFalse(op As T) As Boolean
```

対応するオーバー ロードと`And`または`Or`演算子。

```vb
Public Shared Operator And(op1 As T, op2 As T) As T
Public Shared Operator Or(op1 As T, op2 As T) As T
```

評価するときに、`AndAlso`または`OrElse`演算子、最初のオペランドが 1 回だけ評価されると、2 番目のオペランドは、評価されず、1 回だけ評価します。 次に例を示します。

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

```
And: False True
Or: True False
AndAlso: False
OrElse: True
```

リフトの形式で、`AndAlso`と`OrElse`演算子、最初のオペランドが null 場合`Boolean?`、2 番目のオペランドが評価されますが、結果が null では常に`Boolean?`します。

__操作の種類:__

|        | __bo__ | __SB__ | __によって__ | __Sh__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __de__ | __si__ | __Do__ | __Da__  | __ch__  | __st__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __bo__ | bo | bo | bo | bo | bo | bo | bo | bo | bo | bo | bo | bo | Err | Err | bo  | Ob  | 
| __SB__ |    | bo | bo | bo | bo | bo | bo | bo | bo | bo | bo | bo | Err | Err | bo  | Ob  | 
| __によって__ |    |    | bo | bo | bo | bo | bo | bo | bo | bo | bo | bo | Err | Err | bo  | Ob  | 
| __Sh__ |    |    |    | bo | bo | bo | bo | bo | bo | bo | bo | bo | Err | Err | bo  | Ob  | 
| __米国__ |    |    |    |    | bo | bo | bo | bo | bo | bo | bo | bo | Err | Err | bo  | Ob  | 
| __In__ |    |    |    |    |    | bo | bo | bo | bo | bo | bo | bo | Err | Err | bo  | Ob  | 
| __UI__ |    |    |    |    |    |    | bo | bo | bo | bo | bo | bo | Err | Err | bo  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | bo | bo | bo | bo | bo | Err | Err | bo  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | bo | bo | bo | bo | Err | Err | bo  | Ob  | 
| __de__ |    |    |    |    |    |    |    |    |    | bo | bo | bo | Err | Err | bo  | Ob  | 
| __si__ |    |    |    |    |    |    |    |    |    |    | bo | bo | Err | Err | bo  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | bo | Err | Err | bo  | Ob  | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __ch__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __st__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | bo  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


## <a name="shift-operators"></a>シフト演算子

二項演算子`<<`と`>>`ビット シフト演算を実行します。

```antlr
ShiftOperatorExpression
    : Expression '<' '<' LineTerminator? Expression
    | Expression '>' '>' LineTerminator? Expression
    ;
```

演算子が定義されている、 `Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、`ULong`と`Long`型。 他の二項演算子とは異なり、演算子は左のオペランドだけの単項演算子が場合と、シフト演算の結果の型が決まります。 右側のオペランドの型に暗黙的に変換できる必要があります`Integer`され、操作の結果の型を決定するのには使用されません。

`<<`演算子は左シフト数で指定された位置の数のシフトするときは、最初のオペランドでビットを発生します。 結果の型の範囲外の上位ビットは破棄され、下位の空いたビット位置はゼロで埋められます。

`>>`演算子は、シフトするときは、最初のオペランドでビット右シフト数で指定された位置の数。 下位ビットは破棄され、高位の空いたビット位置は、左側のオペランドが正の場合は 0 または負の値のいずれかに設定されます。 左側のオペランドの型の場合`Byte`、 `UShort`、 `UInteger`、または`ULong`空いたビットはゼロで埋められます。

シフト演算子では、2 番目のオペランドの量によっては、最初のオペランドの基になる表現のビットをシフトします。 としてシフト数が計算されたかどうかは、2 番目のオペランドの値が最初のオペランドのビット数より大きいまたは負の値、`RightOperand And SizeMask`場所`SizeMask`は。

| __演算子の種類__  | __SizeMask__ | 
|-----------------------|--------------|
| `Byte`, `SByte`       | 7 (`&H7`)    | 
| `UShort`, `Short`     | 15 (`&HF`)   | 
| `UInteger`, `Integer` | 31 (`&H1F`)  | 
| `ULong`, `Long`       | 63 (`&H3F`)  | 

シフト数が 0 の場合、操作の結果は最初のオペランドの値と同じです。 オーバーフローは、これらの操作からではありません。

__操作の種類:__


| __bo__ | __SB__ | __によって__ | __Sh__ | __米国__ | __In__ | __UI__ | __Lo__ | __UL__ | __de__ | __si__ | __Do__ | __Da__  | __ch__  | __st__ | __Ob__ | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| Sh | SB | By | Sh | US | イン | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo | Ob | 


## <a name="boolean-expressions"></a>Boolean 式

ブール式では、true の場合、または false の場合にテストできる式です。

```antlr
BooleanExpression
    : Expression
    ;
```

型`T`ブール式で使用できる場合は、優先順位で。

* `T` が `Boolean` または `Boolean?` です。

* `T` 拡大変換するには `Boolean`

* `T` 拡大変換するには `Boolean?`

* `T` 2 つの擬似演算子を定義します。`IsTrue`と`IsFalse`します。

* `T` 縮小変換が`Boolean?`からの変換を伴わない`Boolean`に`Boolean?`します。

* `T` 縮小変換が`Boolean`します。

__注意してください。__ されている場合に興味深いは、`Option Strict`式への縮小変換は、オフ`Boolean`、コンパイル時エラーが、言語はそれでも選択せずに受け入れ、`IsTrue`演算子が存在する場合。 これは、ため`Option Strict`が、言語によって受け入れられないし、式の実際の意味を決して変更内容が変わるだけです。 したがって、`IsTrue`縮小変換では、上に関係なく常に優先されますが、`Option Strict`します。

たとえば、次のクラスがへの拡大変換を定義しません`Boolean`します。 その結果での使用、`If`ステートメントへの呼び出しがあると、`IsTrue`演算子。

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

ブール式として型指定されたかどうかに変換または`Boolean`または`Boolean?`、値の場合は true が`True`false、それ以外の場合。

それ以外の場合、ブール式を呼び出す、`IsTrue`演算子を返す`True`演算子は、返された場合`True`; それ以外の場合は false (呼び出すことはありませんが、`IsFalse`演算子)。

次の例では、`Integer`への縮小変換が`Boolean`ため、null`Integer?`両方への縮小変換が`Boolean?`(null 値を生成`Boolean`) および`Boolean`(例外をスローする)。 縮小変換`Boolean?`をお勧めなどの値"`i`"をブール値として式はそのため、`False`します。

```vb
Dim i As Integer? = Nothing
If i Then Console.WriteLine()
```


## <a name="lambda-expressions"></a>ラムダ式

A*ラムダ式*と呼ばれる、匿名メソッドを定義、*メソッドのラムダ*します。 ラムダ メソッドでは、"line"メソッドを他のデリゲート型を取るメソッドに渡すやすいようにします。

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

例:

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

表示されます。

```
2 4 6 8
```

ラムダ式は、オプションの修飾子で始まる`Async`または`Iterator`キーワードと、その後`Function`または`Sub`およびパラメーターのリスト。 ラムダ式でパラメーターを宣言することはできません`Optional`または`ParamArray`と属性を持つことはできません。 通常のメソッドとは異なり、ラムダ メソッドのパラメーターの型を省略すると自動的に推論しません`Object`します。 代わりに、ときに、ラムダ メソッドは、再分類、省略されたパラメーターの型と`ByRef`修飾子は、対象の型から推論されます。 前の例では、ラムダ式が書き込まれたとして`Function(x) x * 2`の型推論は`x`する`Integer`ラムダ メソッドのインスタンスを作成に使用されたときに、`IntFunc`デリゲート型。 ローカル変数の推定とは異なりラムダ メソッドのパラメーターは、型が省略されますが、配列または null 許容型名の修飾子がある場合は、コンパイル時エラーが発生します。

A__正規のラムダ式__で、どちらも`Async`も`Iterator`修飾子。

__反復子ラムダ式__で 1 つは、`Iterator`修飾子と no`Async`修飾子。 関数があります。 値に、再分類するがときに、のみ再分類できます戻り値の型がデリゲート型の値に`IEnumerator`、または`IEnumerable`、または`IEnumerator(Of T)`または`IEnumerable(Of T)`一部`T`が ByRef パラメーターを持たないとします。

__非同期ラムダ式__で 1 つは、`Async`修飾子と no`Iterator`修飾子。 非同期サブルーチンのラムダは、ByRef パラメーターなしでサブ デリゲート型の値にのみ再分類可能性があります。 非同期のラムダ関数が戻り値の型は関数デリゲート型の値に再分類する可能性がありますのみ`Task`または`Task(Of T)`一部`T`が ByRef パラメーターを持たないとします。

単一行または複数行ラムダ式はできますか。 単一行`Function`ラムダ式は、ラムダ メソッドから返される値を表す 1 つの式を含めることができます。 単一行`Sub`ラムダ式は、その閉じずに 1 つのステートメントを含めることが`StatementTerminator`します。 例えば:

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

単一行のラムダは、その他のすべての式およびステートメントよりも小さい緊密にバインドを構築します。 たとえば、このため、"`Function() x + 5`「は等価である」`Function() (x+5)"`なく"`(Function() x) + 5`"。 単一行、あいまいさを回避するために`Sub`Dim ステートメントまたはラベルの宣言ステートメントが、ラムダ式に含まれていない可能性があります。 また、単一行、かっこで囲まれている場合を除き、`Sub`ラムダ式がない直後、コロンで":"、メンバー アクセス演算子"."、ディクショナリ メンバー アクセス演算子"!"またはかっこ"(". 任意のブロックのステートメントがない場合があります (`With`、 `SyncLock, If...EndIf`、 `While`、 `For`、 `Do`、 `Using`) も`OnError`も`Resume`します。

__注意してください。__ ラムダ式で`Function(i) x=i`、本文として解釈されます、*式*(テストするかどうか`x`と`i`が等しい)。 ラムダ式で`Sub(i) x=i`、本文は、ステートメントとして解釈されます (どの割り当てます`i`に`x`)。

複数行ラムダ式がステートメント ブロックが含まれ、適切な終了する必要があります`End`ステートメント (つまり`End Function`または`End Sub`)。 通常のメソッドを複数行ラムダ メソッドのと同様`Function`または`Sub`ステートメントと`End`ステートメントは、独自の行に配置する必要があります。 例えば:

```vb
' Error: Function statement must be on its own line!
Dim x = Sub(x As Integer) : Console.WriteLine(x) : End Sub

' OK
Dim y = Sub(x As Integer)
               Console.WriteLine(x)
          End Sub
```

複数行`Function`ラムダ式が戻り値の型を宣言できますが、上の属性を配置することはできません。 複数行の場合`Function`ラムダ式が戻り値の型を宣言しませんが、ラムダ式を使用し、その戻り値の型が使用されるコンテキストから戻り値の型を推論することができます。 それ以外の場合、関数の戻り値の型は、次のように計算します。

* 通常のラムダ式では、戻り値の型はすべての式の主要な型、`Return`ステートメント ブロック内のステートメント。

* 非同期ラムダ式では、戻り値の型は`Task(Of T)`場所`T`内のすべての式の主要な型には、`Return`ステートメント ブロック内のステートメント。

* 反復子ラムダ式では、戻り値の型が`IEnumerable(Of T)`場所`T`内のすべての式の主要な型には、`Yield`ステートメント ブロック内のステートメント。

例えば:

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

ある場合は常にない`Return`(それぞれ`Yield`) ステートメント、または主要な型、それらの間ではなく、厳密な型が使用されている、コンパイル時エラーが発生します。 それ以外の場合、主要な型は、暗黙的に`Object`します。

すべての戻り値の型が計算されることに注意してください。`Return`ステートメント、到達可能でない場合でも。 例えば:

```vb
' Return type is Double
Dim x = Function()
              Return 10
               Return 10.50
          End Function
```

変数の名前が存在しないために、暗黙の戻り変数はありません。

複数行ラムダ式の中でステートメント ブロックには、次の制限があります。

* `On Error` `Resume`ですが、ステートメントは使用できません`Try`ステートメントを使用できます。

* 複数行ラムダ式では、静的ローカル変数を宣言することはできません。

* そこに通常の分岐の規則が適用されますが、複数行ラムダ式のステートメント ブロックの内外に分岐することはできません。 例えば:

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

ラムダ式は、含んでいる型で宣言された匿名メソッドとほぼ同じです。 最初の例では、約と同等です。

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


### <a name="closures"></a>クロージャ

ラムダ式では、ローカル変数またはパラメーターを含むメソッドとラムダ式で定義されているなど、スコープですべての変数へのアクセスがあります。 ラムダ式は、ローカル変数またはパラメーターを参照しているときに、ラムダ式はクロージャに参照される変数をキャプチャします。 クロージャは、スタック上の代わりに、ヒープ上に存在するオブジェクトとは、変数をキャプチャしたら、すべての参照を変数には、クロージャにリダイレクトされます。 これにより、ラムダ式を含むメソッドが完了した後でも、ローカル変数とパラメーターを参照してください。 例えば:

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

約と同等です。

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

ローカルの新しいコピーをキャプチャするクロージャ変数をブロックに入るたびに、宣言されたローカル変数が存在する場合、新しいコピーが以前のコピーの値で初期化されます。 例えば:

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

プリント

```
1 2 3 4 5 6 7 8 9 10
```

代わりに

```
9 9 9 9 9 9 9 9 9 9
```

許可されていないブロックを入力するときに初期化するため、クロージャ`GoTo`から、そのブロックの外部でのクロージャでのブロックにすることができますが`Resume`クロージャでのブロックにします。 例えば:

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

これらは、クロージャにキャプチャすることはできません、ためには、ラムダ式の内部で、次のことはできません表示されます。

* 参照パラメーター。

* 式のインスタンス (`Me`、 `MyClass`、 `MyBase`) 場合は、型`Me`クラスではありません。

ラムダ式が式の一部である場合、匿名型の作成式のメンバー。 例えば:

```vb
' Error: Lambda cannot refer to anonymous type field
Dim x = New With { .a = 12, .b = Function() .a }
```

`ReadOnly` インスタンス コンス トラクターのインスタンス変数または`ReadOnly`値以外のコンテキストで、変数が使用されている共有のコンス トラクター内の変数を共有します。 例えば:

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

A*クエリ式*一連が適用される式は、*クエリ演算子*の要素に、*クエリ可能な*コレクション。 たとえば、次の式のコレクションを受け取り`Customer`オブジェクトし、ワシントン州内のすべての顧客の名前を返します。

```vb
Dim names = _
    From cust In Customers _
    Where cust.State = "WA" _
    Select cust.Name
```

クエリ式で始まらなければなりません、`From`または`Aggregate`演算子のクエリ演算子で終わります。 クエリ式の結果値として分類されます。式の結果型は、式内の最後のクエリ演算子の結果の型に依存します。

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

いくつかのクエリ演算子という名前の変数の特殊な種類の導入を*範囲変数*します。 範囲変数が実際の変数です。代わりに、個々 の値は、クエリの評価中に、入力コレクションに対してこれら表します。

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

範囲変数のスコープは、クエリ演算子の概要から、クエリ式の末尾に、またはクエリ演算子になど`Select`を非表示にします。 たとえば、次のクエリで

```vb
Dim waCusts = _
    From cust As Customer In Customers _
    Where cust.State = "WA"
```

`From`クエリ操作は、範囲変数を導入`cust`として型指定された`Customer`では、各顧客を表す、`Customers`コレクション。 次`Where`クエリ演算子は、範囲変数を参照`cust`結果のコレクションから個々 のユーザーをフィルター処理するかどうかを決定するフィルター式で。

範囲変数の 2 種類があります:*コレクションの範囲変数*と*式の範囲変数*します。 コレクションの範囲変数は、クエリ対象のコレクションの要素からその値を取得します。 コレクションの範囲変数宣言でコレクションの式は、型がクエリ可能な値として分類する必要があります。 コレクションの範囲変数の型を省略すると、コレクションの式の要素の型を推論または`Object`コレクション式が要素の型を持たないかどうか (つまりのみを定義、`Cast`メソッド)。 コレクション式がクエリ可能な場合 (つまり、コレクションの要素の型が推測できない)、コンパイル時エラーが発生します。

式の範囲変数は、範囲変数がその値がコレクションではなく、式で計算されます。 次の例では、`Select`クエリ演算子という式の範囲変数が導入されています`cityState`2 つのフィールドから計算されます。

```vb
Dim cityStates = _
    From cust As Customer In Customers _
    Select cityState = cust.City & "," & cust.State _
    Where cityState.Length() < 10
```

式の範囲変数は、疑わしい値のような変数があります。 別の範囲変数を参照する必要はありません。 式の範囲変数に割り当てられた式は、値として分類する必要があり、指定した場合、範囲変数の型に暗黙的に変換できる必要があります。

Let 演算子でのみがあります式の範囲変数をその型を指定します。 その他の演算子でまたはその型が指定されていないかどうかは、ローカル変数の型推論を使用して、範囲変数の種類を決定します。

範囲変数は、シャドウする点では、ローカル変数の宣言に関する規則に従う必要があります。 そのため、範囲変数は、(具体的には、クエリ演算子には、スコープ内の現在の範囲変数がすべて非表示になります) しない限り、ローカル変数または外側のメソッドまたは別の範囲変数のパラメーターの名前を隠すことはできません。


### <a name="queryable-types"></a>クエリ可能型

クエリ式は、よく知られているコレクション型メソッドを呼び出すには、式を変換することによって実装されます。 これらの明確に定義されたメソッドは、クエリ可能なコレクションの要素の型と、コレクションで実行されるクエリ演算子の結果の型を定義します。 各クエリ演算子では、特定の変換は実装に依存するクエリ演算子に変換される一般に、メソッドまたはメソッドを指定します。 メソッドは、次のような一般的な形式を使用して、仕様で与えられます。

```vb
Function Select(selector As Func(Of T, R)) As CR
```

次のメソッドに適用されます。

* メソッドは、インスタンスまたはコレクション型の拡張機能のメンバーである必要があり、アクセス可能である必要があります。

* メソッドは、すべての型引数の推論することでは、汎用的な可能性があります。

* メソッドをオーバー ロードを決定するオーバー ロードの解決を使用する場合、使用する方法を正確にします。

* デリゲートの代わりに別のデリゲート型を使用できます`Func`入力すると、一致すると、戻り値の型を含む、同じシグネチャを持っていれば、`Func`型。

* 型`System.Linq.Expressions.Expression(Of D)`デリゲートの代わりに使用する場合があります`Func`る型`D`が一致すると、戻り値の型を含む、同じシグネチャを持つデリゲート型`Func`型。

* 型`T`入力コレクションの要素型を表します。 同じ種類の入力の要素にクエリ可能なコレクション型のすべてのコレクション型によって定義されるメソッドが必要です。

* 型`S`結合を実行するクエリ演算子の場合、2 番目の入力コレクションの要素型を表します。

* 型`K`をキーとして機能する範囲変数のセットを持つクエリ演算子の場合の主要な型を表します。

* 型`N`(ただし、ユーザー定義型と組み込みの数値型ではない可能性があります) は、数値型として使用する型を表します。

* 型`B`ブール式で使用できる型を表します。

* 型`R`クエリ演算子は、結果のコレクションを生成した場合、結果のコレクションの要素型を表します。 `R` クエリ演算子の終了時にスコープ内の範囲変数の数によって異なります。 かどうか、単一の範囲変数がスコープ内、し`R`範囲変数の型です。 例

  ```vb
  Dim custNames = From c In Customers
                  Select c.Name
  ```

  クエリの結果の要素型のコレクション型であるが`String`します。 は、スコープの複数の範囲変数場合、`R`範囲変数とスコープ内のすべてを含む匿名型は、`Key`フィールド。 次に例を示します。

  ```vb
  Dim custNames = From c In Customers, o In c.Orders 
                  Select Name = c.Name, ProductName = o.ProductName
  ```

  クエリの結果をという名前の読み取り専用プロパティを持つ匿名型の要素型のコレクション型となります`Name`型の`String`という名前の読み取り専用プロパティと`ProductName`型の`String`します。

  クエリ式では、範囲変数を格納する生成された匿名型は*透明*ことを示す範囲変数の修飾なしで利用は常にします。 たとえば、前の例で範囲変数`c`と`o`で修飾なしでアクセスする可能性があります、`Select`入力コレクションの要素の型が、匿名型も、演算子のクエリを実行します。

* 型`CX`コレクション型、必ずしも、入力コレクション型、要素型がいくつかの型を表す`X`します。

クエリ可能なコレクション型には、優先順位で、次の条件のいずれかを満たす必要があります。

* 準拠を定義する必要があります、`Select`メソッド。

* する必要がありますが、次のメソッドのいずれか

  ```vb
  Function AsEnumerable() As CT
  Function AsQueryable() As CT
  ```

  クエリ可能なコレクションを取得すると呼ばれることができます。 どちらの方法が指定されている場合`AsQueryable`が優先`AsEnumerable`します。

* メソッドがあります。

  ```vb
  Function Cast(Of T)() As CT
  ```

  クエリ可能なコレクションを生成するために、範囲変数の型を使用すると呼ばれることができます。

コレクションの要素の型を決定するが、実際のメソッド呼び出しとは無関係に発生するための特定のメソッドの適用性を特定できません。 したがって、よく知られているメソッドに一致するインスタンス メソッドがある場合は、コレクションの要素の型を決定する、ときに、よく知られているメソッドと一致する任意の拡張メソッドは無視されます。

クエリ演算子の変換は、クエリ演算子の式で行われる順序で発生します。 少なくともすべてのコレクション オブジェクトをサポートする必要がありますが、すべての必要なすべてのクエリ演算子メソッドを実装するコレクション オブジェクトに対する必要はありません、`Select`クエリ演算子。 必要なメソッドが存在しない場合は、コンパイル時エラーが発生します。 よく知られているメソッドの名前をバインドするときに、非メソッドはシャドウのセマンティクスが引き続き適用されますがインターフェイスとバインドするには、拡張メソッドで複数の継承するために無視されます。 例えば:

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

### <a name="default-query-indexer"></a>既定のクエリのインデクサー

要素型があるすべてのクエリ可能なコレクション型`T`まだ、既定値がないとプロパティは、次の一般的な形式の既定のプロパティを持つと見なされます。

```vb
Public ReadOnly Default Property Item(index As Integer) As T
    Get
        Return Me.ElementAtOrDefault(index)
    End Get
End Property
```

既定のプロパティは、既定値プロパティ アクセスの構文を使用する場合にのみ参照できます。既定のプロパティは、名前によって参照することはできません。 例えば:

```vb
Dim customers As IEnumerable(Of Customer) = ...
Dim customerThree = customers(2)

' Error, no such property
Dim customerFour = customers.Item(4)
```

コレクション型がない場合、`ElementAtOrDefault`メンバー、コンパイル時エラーが発生します。

### <a name="from-query-operator"></a>クエリ演算子から

`From`クエリ演算子には、クエリを実行するコレクションの個々 のメンバーを表すコレクションの範囲変数が導入されています。

```antlr
FromQueryOperator
    : LineTerminator? 'From' LineTerminator? CollectionRangeVariableDeclarationList
    ;
```

たとえば、クエリ式。

```vb
From c As Customer In Customers ...
```

相当する見なすことができます。

```vb
For Each c As Customer In Customers
        ...
Next c
```

ときに、`From`クエリ演算子が複数のコレクションの範囲変数を宣言またはこの問題は`From`クエリ演算子がクエリ式で、コレクションの新しい各範囲変数は*クロス結合*の既存のセットに範囲変数。 クエリが参加しているコレクション内のすべての要素のクロス積に対して評価されることになります。 たとえば、次のような式があるとします。

```vb
From c In Customers _
From e In Employees _
...
```

相当する考えることができます。

```vb
For Each c In Customers
    For Each e In Employees
            ...
    Next e
Next c
```

まったく同じです。

```vb
From c In Customers, e In Employees ...
```

前のクエリ演算子で導入された範囲変数は、後で使用できます`From`クエリ演算子。 たとえば、次のクエリ式で、2 つ目`From`クエリ演算子が最初の範囲変数の値を示します。

```vb
From c As Customer In Customers _
From o As Order In c.Orders _
Select c.Name, o
```

内の複数の範囲変数、`From`演算子または複数のクエリ`From`クエリ演算子は、コレクション型には、次のメソッドの一方または両方が含まれている場合にのみサポートされます。

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

一般に翻訳されます。

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = _
    xs.SelectMany( _
        Function(x As Integer) ys, _
        Function(x As Integer, y As Integer) New With {x, y})...
```

__注意してください。__ `From` 予約語ではありません。


### <a name="join-query-operator"></a>Join クエリ演算子

`Join`等値式に基づいてクエリ演算子を結合する要素を結合されている 1 つのコレクションを作成、新しいコレクションの範囲変数の既存の範囲変数。

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

例えば:

```vb
Dim customersAndOrders = _
    From cust In Customers _
    Join ord In Orders On cust.ID Equals ord.CustomerID
```

等しいかどうかの式は、正規表現の等価性の式よりも制限。

* 両方の式は、値として分類する必要があります。

* 両方の式は、少なくとも 1 つの範囲変数を参照する必要があります。

* 範囲変数が、結合に宣言するは、クエリ演算子は、式のいずれかで参照する必要があり、表現する必要があります、その他の範囲変数を参照していないことです。

2 つの式の型でない場合、まったく同じ型、

* 等値演算子は 2 種類の定義、両方の式が、暗黙的に変換されない`Object`、両方の式をその型に変換します。

* それ以外の場合、両方の式を暗黙的に変換する主要な型がある場合、両方の式を型に変換します。

* それ以外の場合は、コンパイル時のエラーが発生します。

ハッシュ値を使用して、式を比較 (つまり、呼び出すことによって`GetHashCode()`) 効率を高めるための等値演算子を使用するではなく。 A`Join`クエリ演算子は、複数の結合または等値演算子と同じ条件を行うことができます。 A`Join`クエリ演算子がコレクション型には、メソッドが含まれている場合にのみサポートされています。

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

一般に翻訳されます。

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

__注意してください。__ `Join`、`On`と`Equals`予約された単語は使用されません。


### <a name="let-query-operator"></a>Let クエリ演算子

`Let`クエリ演算子に式の範囲変数が導入されています。 これにより、複数回以降のクエリ演算子で使用すると、中間の値を計算できます。

```antlr
LetQueryOperator
    : LineTerminator? 'Let' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

例えば:

```vb
Dim taxedPrices = _
    From o In Orders _
    Let tax = o.Price * 0.088 _
    Where tax > 3.50 _
    Select o.Price, tax, total = o.Price + tax
```

相当する考えることができます。

```vb
For Each o In Orders
    Dim tax = o.Price * 0.088
    ...
Next o
```

A`Let`クエリ演算子がコレクション型には、メソッドが含まれている場合にのみサポートされています。

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

一般に翻訳されます。

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Select(Function(x As Integer) New With {x, .y = x * 10})...
```


### <a name="select-query-operator"></a>クエリ演算子を選択します。

`Select`クエリ演算子は似ています、`Let`クエリ演算子の式の範囲変数が導入されている、`Select`クエリ演算子には、それらを追加する代わりに、現在利用可能な範囲変数が非表示になります。 導入された式の範囲変数の型も、`Select`クエリ演算子は、ローカル変数の型の推論規則を使用して常に推論されます。 明示的な型を指定することはできませんと型を推論されない場合、コンパイル時エラーが発生します。

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

`Where`クエリ演算子のみへのアクセスを持つ、`name`で導入された範囲変数、`Select`演算子場合、`Where`参照しようとしていた演算子`cust`コンパイル時エラーが発生します。

範囲変数の名前を明示的に指定する代わりに、`Select`演算子は、範囲変数の名前を推論できる匿名型のオブジェクト作成式と同じ規則を使ってクエリ。 例えば:

```vb
Dim custAndOrderNames = _
      From cust In Customers, ord In cust.Orders _
      Select cust.name, ord.ProductName _
        Where name.EndsWith("Smith")
```

範囲変数の名前が指定されていない、名前が推測できない場合は、コンパイル時エラーが発生します。 場合、`Select`クエリ演算子には、1 つの式のみが含まれています、範囲変数の名前を推論することはできませんが、範囲変数は名前のないかどうかはエラーは発生しません。  例えば:

```vb
Dim custAndOrderNames = _
      From cust In Customers, ord In cust.Orders _
      Select cust.Name & " bought " & ord.ProductName _
        Take 10
```

あいまいさがある場合、`Select`範囲変数と等値式に名前を割り当てることの間のクエリ演算子、名前の割り当てはお勧めします。 例えば:

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

内の各式、`Select`クエリ演算子は、値として分類する必要があります。 A`Select`クエリ演算子はコレクション型には、メソッドが含まれている場合にのみサポートされています。

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

一般に翻訳されます。

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Select(Function(x As Integer) New With {x, .y = x * 10})...
```


### <a name="distinct-query-operator"></a>Distinct クエリ演算子

`Distinct`クエリ演算子と等しいかどうか要素型を比較することによって決定される個別の値にのみ、コレクション内の値を制限します。

```antlr
DistinctQueryOperator
    : LineTerminator? 'Distinct' LineTerminator?
    ;
```

たとえば、次のようなクエリがあるとします。

```vb
Dim distinctCustomerPrice = _
    From cust In Customers, ord In cust.Orders _
    Select cust.Name, ord.Price _
    Distinct
```

お客様は、同じ価格、複数の注文を持っている場合でも、各顧客の名前と順序の価格のペアリングを個別には 1 つの行を返しますのみは。 A`Distinct`クエリ演算子はコレクション型には、メソッドが含まれている場合にのみサポートされています。

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

一般に翻訳されます。

```vb
Dim xs() As Integer = ...
Dim zs = xs.Distinct()...
```

__注意してください。__ `Distinct` 予約語ではありません。


### <a name="where-query-operator"></a>Where クエリ演算子

`Where`クエリ演算子が指定された条件を満たすコレクションに値を制限します。

```antlr
WhereQueryOperator
    : LineTerminator? 'Where' LineTerminator? BooleanExpression
    ;
```

A`Where`クエリ演算子は、範囲変数の値のセットごとに評価されるブール式; 式の値が出力コレクションに値が表示されます、true の場合、それ以外の場合、値はスキップされます。 たとえば、クエリ式。

```vb
From cust In Customers, ord In Orders _
Where cust.ID = ord.CustomerID _
...
```

入れ子になったループに相当する見なすことができます。

```vb
For Each cust In Customers
    For Each ord In Orders
            If cust.ID = ord.CustomerID Then
                ...
            End If
    Next ord
Next cust
```

A`Where`クエリ演算子はコレクション型には、メソッドが含まれている場合にのみサポートされています。

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

一般に翻訳されます。

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Where(Function(x As Integer) x < 10)...
```

__注意してください。__ `Where` 予約語ではありません。


### <a name="partition-query-operators"></a>パーティションのクエリ演算子

```antlr
PartitionQueryOperator
    : LineTerminator? 'Take' LineTerminator? Expression
    | LineTerminator? 'Take' 'While' LineTerminator? BooleanExpression
    | LineTerminator? 'Skip' LineTerminator? Expression
    | LineTerminator? 'Skip' 'While' LineTerminator? BooleanExpression
    ;
```

`Take`クエリ演算子の結果、最初に`n`コレクションの要素。 使用すると、`While`修飾子、`Take`最初の演算子の結果`n`ブール式を満たすコレクションの要素。 `Skip`演算子は、1 つ目をスキップします。`n`コレクションの要素をコレクションの残りの部分を返します。  組み合わせて使用すると、`While`修飾子、`Skip`演算子は、1 つ目をスキップします。`n`ブール式を満たすと、コレクションの残りの部分を返しますが、コレクションの要素。 内の式を`Take`または`Skip`クエリ演算子は、値として分類する必要があります。

A`Take`クエリ演算子はコレクション型には、メソッドが含まれている場合にのみサポートされています。

```vb
Function Take(count As N) As CT
```

A`Skip`クエリ演算子はコレクション型には、メソッドが含まれている場合にのみサポートされています。

```vb
Function Skip(count As N) As CT
```

A`Take While`クエリ演算子はコレクション型には、メソッドが含まれている場合にのみサポートされています。

```vb
Function TakeWhile(predicate As Func(Of T, B)) As CT
```

A`Skip While`クエリ演算子はコレクション型には、メソッドが含まれている場合にのみサポートされています。

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

一般に翻訳されます。

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Skip(10). _
        Take(5). _
        SkipWhile(Function(x) x < 10). _
        TakeWhile(Function(x) x > 5)...
```

__注意してください。__ `Take` `Skip`予約された単語は使用されません。


### <a name="order-by-query-operator"></a>Order By クエリ演算子

`Order By`クエリ演算子は、範囲変数に表示される値を並べ替えます。 

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

`Order By`クエリ演算子は、反復変数の並べ替えに使用されるキーの値を指定する式。 たとえば、次のクエリでは、製品の価格で並べ替えられますが返されます。

```vb
Dim productsByPrice = _
    From p In Products _
    Order By p.Price _
    Select p.Name
```

としてマークできる順序付け`Ascending`より大きな値は、前に小さい値を取得する場合または`Descending`値を小さくする前に大きい値を取得する場合。 順序付けが指定されないかどうかの既定値は`Ascending`します。 たとえば、次のクエリには、最も負荷の高い製品に価格で並べ替えた製品が返されます。

```vb
Dim productsByPriceDesc = _
    From p In Products _
    Order By p.Price Descending _
    Select p.Name
```

`Order By`クエリ演算子は、順序付けの複数の式を指定できます、コレクションが入れ子になった方法で順序付けられた場合。 たとえば、次のクエリでは、市区町村内の各状態によってし、各都市の郵便番号/ZIP code によってし、状態でお客様が注文します。

```vb
Dim customersByLocation = _
    From c In Customers _
    Order By c.State, c.City, c.ZIP _
    Select c.Name, c.State, c.City, c.ZIP
```

内の式、`Order By`クエリ演算子は、値として分類する必要があります。 `Order By`コレクション型には、次のメソッドの一方または両方が含まれています。 場合にのみ、クエリ演算子がサポートされます。

```vb
Function OrderBy(keySelector As Func(Of T, K)) As CT
Function OrderByDescending(keySelector As Func(Of T, K)) As CT
```

戻り値の型`CT`必要があります、*順序付けられたコレクション*します。 順序付きコレクションでは、メソッドの一方または両方を含むコレクション型を示します。

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

一般に翻訳されます。

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.OrderBy(Function(x) x).ThenByDescending(Function(x) x Mod 2)...
```

__注意してください。__ クエリ演算子は、特定のクエリ操作を実装するメソッドに構文を単にマップ、ため、順序の維持は言語によっては指定されていませんし、演算子自体の実装によって決定されます。 これは、機能は、ユーザー定義の数値型の加算演算子をオーバー ロードを実装可能性があります、追加のようなものを実行しない点でユーザー定義演算子によく似ています。 もちろん、予測可能性を保持するために実装するユーザーの期待に一致しないものは推奨されません。

__注意してください。__ `Order` `By`予約された単語は使用されません。


### <a name="group-by-query-operator"></a>Group By クエリ演算子

`Group By`クエリ演算子は、1 つまたは複数の式に基づくスコープ内の範囲変数をグループ化し、新しい範囲グループに基づいて変数が生成されます。

```antlr
GroupByQueryOperator
    : LineTerminator? 'Group' ( LineTerminator? ExpressionRangeVariableDeclarationList )?
      LineTerminator? 'By' LineTerminator? ExpressionRangeVariableDeclarationList
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

たとえば、次のクエリはグループによってすべての顧客`State`、し、各グループの数、平均年齢を計算します。

```vb
Dim averageAges = _
    From cust In Customers _
    Group By cust.State _
    Into Count(), Average(cust.Age)
```

`Group By`クエリ演算子は、3 つの句: 省略可能な`Group`句、`By`句、および`Into`句。 `Group`句と同じ構文およびと効果には、`Select`で使用できる範囲変数のみに影響する点を除いて、演算子のクエリを実行、`Into`句および not、`By`句。 例えば:

```vb
Dim averageAges = _
    From cust In Customers _
    Group cust.Age By cust.State _
    Into Count(), Average(Age)
```

`By`句はグループ化操作でキーの値として使用されている範囲変数の式を宣言します。 `Into`句は集計を計算して形成されたグループのそれぞれの範囲変数の式の宣言を使用できます、`By`句。 内で、`Into`句では、式の範囲変数のみ割り当てることができます、メソッドの呼び出しである式の*集計関数*します。 集計関数は、次のいずれかのようになります (これは必ずしも元のコレクションの同じコレクション型) グループのコレクション型の関数を示します。

```vb
Function _name_() As _type_
Function _name_(selector As Func(Of T, R)) As R
```

集計関数がデリゲートの引数を受け取る場合、invocation 式は、値として分類する必要があります引数式を持つことができます。  引数式のスコープ内の範囲変数を使用できます。集計関数の呼び出し内では、これらの範囲変数は、コレクション内の値のすべての形成中、グループ内の値を表します。 たとえば、このセクションでは、元の例で、`Average`関数がまとめて、顧客の年齢層のすべての顧客ではなく、状態ごとの平均を計算します。

すべてのコレクション型を集計関数があると見なされます`Group`、定義されているパラメーターを受け取らない構成して、グループを返すだけです。 その他のコレクション型を提供する、標準の集計関数は次のとおりです。

`Count` `LongCount`、グループまたはブール式を満たすグループ内の要素の数の要素の数を返すをします。 `Count` `LongCount`コレクション型を含むメソッドのいずれかの場合にのみサポートされます。

```vb
Function Count() As N
Function Count(selector As Func(Of T, B)) As N
Function LongCount() As N
Function LongCount(selector As Func(Of T, B)) As N
```

`Sum`、グループ内のすべての要素間で式の合計を返します。 `Sum` コレクション型には、メソッドのいずれかが含まれている場合にのみサポートされます。

```vb
Function Sum() As N
Function Sum(selector As Func(Of T, N)) As N
```

`Min` これは、グループ内のすべての要素間で、式の最小値を返します。 `Min` コレクション型には、メソッドのいずれかが含まれている場合にのみサポートされます。

```vb
Function Min() As N
Function Min(selector As Func(Of T, N)) As N
```

`Max`、式の最大値を、グループ内のすべての要素が返されます。 `Max` コレクション型には、メソッドのいずれかが含まれている場合にのみサポートされます。

```vb
Function Max() As N
Function Max(selector As Func(Of T, N)) As N
```

`Average`、グループ内のすべての要素間で式の平均値が返されます。 `Average` コレクション型には、メソッドのいずれかが含まれている場合にのみサポートされます。

```vb
Function Average() As N
Function Average(selector As Func(Of T, N)) As N
```

`Any`、グループにメンバーが含まれているかどうかブール式がグループ内の任意の要素に対して true のかどうかを決定します。 `Any` ブール式で使用でき、コレクション型には、メソッドのいずれかが含まれている場合にのみサポートされている値が返されます。

```vb
Function Any() As B
Function Any(predicate As Func(Of T, B)) As B
```

`All`、ブール式がグループのすべての要素に対して true かどうかを決定します。 `All` ブール式で使用でき、コレクション型には、メソッドが含まれている場合にのみサポートされている値が返されます。

```vb
Function All(predicate As Func(Of T, B)) As B
```

後に、`Group By`クエリ演算子、以前のスコープ内の範囲変数は表示されず、および範囲変数がで導入された、`By`と`Into`句が使用可能な。 A`Group By`クエリ演算子はコレクション型には、メソッドが含まれている場合にのみサポートされています。

```vb
Function GroupBy(keySelector As Func(Of T, K), _
                      resultSelector As Func(Of K, CT, R)) As CR
```

範囲内の変数宣言、`Group`句がコレクション型には、メソッドが含まれている場合にのみサポートされています。

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

一般に翻訳されます。

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

__注意してください。__ `Group`、 `By`、および`Into`予約された単語は使用されません。


### <a name="aggregate-query-operator"></a>集計クエリ演算子

`Aggregate`クエリ演算子と同様の関数の実行、`Group By`オペレーターは、既に形成されたされたグループを集計することができる点が異なります。 グループが既に形成されたされているため、`Into`の句、`Aggregate`クエリ演算子には、スコープ内の範囲変数は非 (これにより、`Aggregate`ように、`Let`と`Group By`ように、 `Select`)。

```antlr
AggregateQueryOperator
    : LineTerminator? 'Aggregate' LineTerminator? CollectionRangeVariableDeclaration QueryOperator*
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

たとえば、次のクエリは米国ワシントン州の顧客によって行われるすべての注文の合計、集計します。

```vb
Dim orderTotals = _
    From cust In Customers _
    Where cust.State = "WA" _
    Aggregate order In cust.Orders _
    Into Sum(order.Total)
```

このクエリの結果は、コレクションの要素の入力がという名前のプロパティを持つ匿名型`cust`として型指定された`Customer`という名前のプロパティと`Sum`として型指定された`Integer`します。

異なり`Group By`、追加のクエリ演算子の間に配置できる、`Aggregate`と`Into`句。 間、`Aggregate`句との終了、`Into`句で宣言されているものも含め、スコープ内のすべての範囲変数、`Aggregate`句を使用できます。 たとえば、次クエリ集計配置のすべての注文の合計 2006 年に米国ワシントン州の顧客:

```vb
Dim orderTotals = _
    From cust In Customers _
    Where cust.State = "WA" _
    Aggregate order In cust.Orders _
    Where order.Date <= #01/01/2006# _
    Into Sum = Sum(order.Total)
```

`Aggregate`演算子がクエリ式を開始することもできます。 ここでは、クエリ式の結果値になります、1 つによって計算された、`Into`句。 たとえば、次のクエリは、2006 年 1 月 1 日の前にすべての注文合計の合計を計算します。

```vb
Dim ordersTotal = _
    Aggregate order In Orders _
    Where order.Date <= #01/01/2006# _
    Into Sum(order.Total)
```

クエリの結果は、1 つ`Integer`値。 `Aggregate`クエリ演算子は、です常に利用可能な (が集計関数する必要があるも有効な式を使用できます)。 コード

```vb
Dim xs() As Integer = ...
Dim zs = _
    Aggregate x In xs _
    Where x < 5 _
    Into Sum()
```

一般に翻訳されます。

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Where(Function(x) x < 5).Sum()
```

__注意してください。__ `Aggregate` `Into`予約された単語は使用されません。


### <a name="group-join-query-operator"></a>グループ結合のクエリ演算子

`Group Join`クエリ演算子の関数を組み合わせて、`Join`と`Group By`に 1 つの演算子のクエリ演算子。 `Group Join` 一致する要素をまとめてグループ化から抽出したキーに基づいて 2 つのコレクションを結合するすべての結合の左側にある特定の要素に一致する結合の右側にある要素。 そのため、演算子は、階層的な結果セットを生成します。

```antlr
GroupJoinQueryOperator
    : LineTerminator? 'Group' 'Join' LineTerminator? CollectionRangeVariableDeclaration
      JoinOrGroupJoinQueryOperator? LineTerminator? 'On' LineTerminator? JoinConditionList
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

たとえば、次のクエリでは、単一の顧客の名前、すべての注文の合計量と、注文のすべてのグループにはが含まれている要素が生成されます。

```vb
Dim custsWithOrders = _
    From cust In Customers _
    Group Join order In Orders On cust.ID Equals order.CustomerID _ 
    Into Orders = Group, OrdersTotal = Sum(order.Total) _
    Select cust.Name, Orders, OrdersTotal
```

クエリの結果は、要素型が 3 つのプロパティを持つ匿名型コレクション:`Name`として型指定された、 `String`、`Orders`要素型がコレクションとして型指定された`Order`、および`OrdersTotal`として型指定された、`Integer`. A`Group Join`クエリ演算子はコレクション型には、メソッドが含まれている場合にのみサポートされています。

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

一般に翻訳されます。

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

__注意してください。__ `Group`、 `Join`、および`Into`予約された単語は使用されません。


## <a name="conditional-expressions"></a>条件式

条件文`If`式は、式をテストし、値を返します。

```antlr
ConditionalExpression
    : 'If' OpenParenthesis BooleanExpression Comma Expression Comma Expression CloseParenthesis
    | 'If' OpenParenthesis Expression Comma Expression CloseParenthesis
    ;
```

異なり、`IIF`ランタイム関数、ただし、条件付きの式だけが評価に必要な場合は、そのオペランド。 したがって、例では、式の`If(c Is Nothing, c.Name, "Unknown")`例外をスローしない場合の値`c`は`Nothing`。 条件式に 2 つの形式: 次の 3 つのオペランドを受け取る 2 つのオペランドと 1 つを受け取るいずれか。

3 つのオペランドが指定されている場合は、3 つすべての式は、値として分類する必要があり、最初のオペランドがブール式にする必要があります。 結果は場合、式が true の場合、2 番目の式は、結果になります、演算子のそれ以外の場合、3 番目の式になります、演算子の結果。 式の結果型は、2 番目と 3 番目の式の型の間の主要な型です。 主要な型がない場合は、コンパイル時エラーが発生します。

2 つのオペランドが指定されている場合は、両方のオペランドの値として分類する必要があり、最初のオペランドが参照型または null 許容値型のいずれかにある必要があります。 式`If(x, y)`は評価されますが、式の場合と`If(x IsNot Nothing, x, y)`、2 つの例外。 最初に、最初の式しか評価 1 回、2 つ目は、2 番目のオペランドの型が null 非許容値型と、最初のオペランドの型が場合、`?`の主要な型を決定するときに、最初のオペランドの型から削除します式の結果型。 例えば:

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

オペランドの場合は、式の両方の形式で`Nothing`、主要な型を決定するはその型は使用されません。 式の場合`If(<expression>, Nothing, Nothing)`、主要な型があると見なされます`Object`します。


## <a name="xml-literal-expressions"></a>XML リテラル式

XML (eXtensible Markup Language) を表す XML リテラル式には 1.0 値。

```antlr
XMLLiteralExpression
    : XMLDocument
    | XMLElement
    | XMLProcessingInstruction
    | XMLComment
    | XMLCDATASection
    ;
```

XML リテラル式の結果は、型からの型の 1 つとして型指定された値、`System.Xml.Linq`名前空間。 その名前空間の型が使用できない場合、XML リテラル式では、コンパイル時エラーが発生します。 値は、XML リテラル式から変換コンス トラクターの呼び出しを使用して生成されます。 たとえば、次のようなコードがあるとします。

```vb
Dim book As System.Xml.Linq.XElement = _
    <book title="My book"></book>
```

コードとほぼ同じです。

```vb
Dim book As System.Xml.Linq.XElement = _
    New System.Xml.Linq.XElement( _
        "book", _
        New System.Xml.Linq.XAttribute("title", "My book"))
```

XML リテラル式には、XML ドキュメント、XML 要素、XML 処理命令、XML コメント、または CDATA セクションのフォームを実行できます。

__注意してください。__ この仕様のみを含む Visual Basic 言語の動作を記述する XML の説明は十分です。 XML の詳細についてで見つかります http://www.w3.org/TR/REC-xml/します。


### <a name="lexical-rules"></a>語彙の規則

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

XML リテラル式では、通常の Visual Basic コードの語彙の規則ではなく、XML の語彙規則を使用して解釈されます。 ルールの 2 つのセットは、一般に、次の点で異なります。

* 空白は XML で重要です。 その結果、XML リテラル式の文法を明示的に表して空白が許可されています。 要素内の文字データのコンテキストで発生するタイミングを除く、空白は保持されません。 例えば:

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

* XML の行の末尾に空白文字は、XML 仕様に従って正規化されます。

* XML は大文字と小文字を区別します。 キーワードに大文字小文字の区別を正確に一致する必要があります。 そう、コンパイル時エラーが発生します。

* 行ターミネータは、XML 内の空白と見なされます。 その結果、XML リテラル式で行継続文字は必要ありません。

* XML では、全角文字は受け入れられません。 全角文字を使用すると、コンパイル時エラーが発生します。


### <a name="embedded-expressions"></a>埋め込み式

XML リテラル式に含めることができます*埋め込み式*します。 埋め込み式は、Visual Basic の式が評価され、埋め込み式の位置にある 1 つまたは複数の値を入力するために使用します。

```antlr
XMLEmbeddedExpression
    : '<' '%' '=' LineTerminator? Expression LineTerminator? '%' '>'
    ;
```

たとえば、次のコードは、文字列を配置`John Smith`XML 要素の値として。

```vb
Dim name as String = "John Smith"
Dim element As System.Xml.Linq.XElement = <customer><%= name %></customer>
```

さまざまなコンテキストで式を埋め込むことができます。 たとえば、次のコードは、という名前の要素を生成します`customer`:。

```vb
Dim name As String = "customer"
Dim element As System.Xml.Linq.XElement = <<%= name %>>John Smith</>
```

埋め込み式を使用できる各コンテキストは、許容される型を指定します。 埋め込み式の式の一部のコンテキスト内では、これは、Visual Basic コードの通常の語彙の規則が引き続き適用、たとえば、行連結文字を使用する必要があります。

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

XML ドキュメントの結果として型指定された値に`System.Xml.Linq.XDocument`します。 XML 1.0 の仕様とは異なり、XML リテラル式の XML ドキュメントが XML ドキュメントのプロローグ; を指定する必要はXML ドキュメントのプロローグせず、XML リテラル式は、個別のエンティティとして解釈されます。 例えば:

```vb
Dim doc As System.Xml.Linq.XDocument = _
    <?xml version="1.0"?>
    <?instruction?>
    <customer>Bob</>

Dim pi As System.Xml.Linq.XProcessingInstruction = _
    <?instruction?>
```

XML ドキュメントは、型を持つ、任意の型の埋め込み式を含めることができます。実行時に、ただし、オブジェクトする必要がありますの要件を満たす、`XDocument`コンス トラクターまたは実行時エラーが発生します。

通常の XML とは異なり XML ドキュメントの式は Dtd (文書型宣言) をサポートしていません。 また、エンコーディングの属性では、指定した場合は無視されます Xml リテラル式のエンコードは常にソース ファイルのエンコーディングと同じであるためです。

__注意してください。__ エンコーディング属性を無視すると、まだ有効な属性をソース コードの任意の有効な Xml 1.0 ドキュメントを含める機能を維持するためにです。


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

XML 要素の結果として型指定された値に`System.Xml.Linq.XElement`します。 通常の XML とは異なり XML 要素は、終了タグの名前を省略でき、現在のほとんどの入れ子になった要素が閉じられます。 例えば:

```vb
Dim name = <name>Bob</>
```

として型指定された値で、結果の XML 要素内の宣言を属性`System.Xml.Linq.XAttribute`します。 属性の値は、XML 仕様に従って正規化されます。 属性の値が`Nothing`属性は作成されません、属性値の式をチェックする必要はありませんので`Nothing`します。 例えば:

```vb
Dim expr = Nothing

' Throws null argument exception
Dim direct = New System.Xml.Linq.XElement( _
    "Name", _
    New System.Xml.Linq.XAttribute("Length", expr))

' Doesn't throw exception, the result is <Name/>
Dim literal = <Name Length=<%= expr %>/>
```

XML 要素と属性は、次の場所に入れ子になった式を含めることができます。

場合、埋め込み式が暗黙的に変換できる型の値をある必要があります、要素の名前`System.Xml.Linq.XName`します。 例えば:

```vb
Dim name = <<%= "name" %>>Bob</>
```

場合、埋め込み式が暗黙的に変換できる型の値をある必要があります、要素の属性の名前`System.Xml.Linq.XName`します。 例えば:

```vb
Dim name = <name <%= "length" %>="3">Bob</>
```

埋め込み式のケースが任意の型の値をすることができます、要素の属性の値。 例えば:

```vb
Dim name = <name length=<%= 3 %>>Bob</>
```

場合、埋め込み式が任意の型の値をすることができます、要素の属性です。 例えば:

```vb
Dim name = <name <%= new XAttribute("length", 3) %>>Bob</>
```

場合、埋め込み式が任意の型の値をすることができます、要素のコンテンツ。 例えば:

```vb
Dim name = <name><%= "Bob" %></>
```

埋め込み式の型が場合`Object()`、配列に paramarray として渡されます、`XElement`コンス トラクター。


### <a name="xml-namespaces"></a>XML 名前空間

XML 要素は、XML 1.0 名前空間の仕様で定義された XML 名前空間の宣言を含めることができます。

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

名前空間を定義するのには、制限`xml`と`xmlns`適用され、コンパイル時エラーが生成されます。 XML 名前空間宣言は、値の埋め込み式を含めることはできません。指定された値は空の文字列リテラルである必要があります。 例えば:

```vb
' Declares a valid namespace
Dim customer = <db:customer xmlns:db="http://example.org/database">Bob</>

' Error: xmlns cannot be re-defined
Dim bad1 = <elem xmlns:xmlns="http://example.org/namespace"/>

' Error: cannot have an embedded expression
Dim bad2 = <elem xmlns:db=<%= "http://example.org/database" %>>Bob</>
```

__注意してください。__ この仕様のみを含む Visual Basic 言語の動作を記述する XML 名前空間の説明は十分です。 XML 名前空間の詳細についてで見つかります http://www.w3.org/TR/REC-xml-names/します。

名前空間の名前を使用して XML 要素と属性名を修飾することができます。 名前空間を正規 XML に示すようにバインドは、ファイル レベルで宣言された任意の名前空間をインポートする例外では、コンパイルで宣言された名前空間インポートので囲まれた自体と、宣言の外側のコンテキストで宣言すると見なされます環境。 名前空間の名前が見つからない場合は、コンパイル時エラーが発生します。 例えば:

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

要素で宣言されている XML 名前空間は、埋め込み式の中で XML リテラルには適用されません。 例えば:

```vb
' Error: Namespace prefix 'db' is not declared
Dim customer = _
    <db:customer xmlns:db="http://example.org/database">
        <%= <db:customer>Bob</> %>
    </>
```

__注意してください。__ 埋め込み式は関数呼び出しをなど、何でもできるためです。 関数呼び出しには、XML リテラル式が含まれている、プログラマが適用または無視する XML 名前空間に期待するかどうかをオフすることをお勧めします。


### <a name="xml-processing-instructions"></a>XML 処理命令

XML 処理命令の結果として型指定された値に`System.Xml.Linq.XProcessingInstruction`します。 処理命令内で有効な構文は、XML 処理命令は、埋め込み式を含めることはできません。

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

XML コメントの結果として型指定された値に`System.Xml.Linq.XComment`します。 XML コメントは、コメント内で有効な構文は、埋め込み式を含めることはできません。

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

CDATA セクションの結果として型指定された値に`System.Xml.Linq.XCData`します。 CDATA セクション内の有効な構文は、CDATA セクションは、埋め込み式を含めることはできません。

```antlr
XMLCDATASection
    : '<' '!' ( 'CDATA' '[' XMLCDATASectionString? ']' )? '>'
    ;

XMLCDATASectionString
    : '<Any XMLString that does not contain the string "]]>">'
    ;
```

## <a name="xml-member-access-expressions"></a>XML メンバー アクセス式

XML メンバー アクセス式では、XML 値のメンバーにアクセスします。

```antlr
XMLMemberAccessExpression
    : Expression '.' LineTerminator? '<' XMLQualifiedName '>'
    | Expression '.' LineTerminator? '@' LineTerminator? '<' XMLQualifiedName '>'
    | Expression '.' LineTerminator? '@' LineTerminator? IdentifierOrKeyword
    | Expression '.' '.' '.' LineTerminator? '<' XMLQualifiedName '>'
    ;
```

XML メンバー アクセス式の 3 つの種類があります。

* *要素へのアクセス*XML 名が 1 つのドットに従います。 例えば:

  ```vb
  Dim customer = _
      <customer>
          <name>Bob</>
      </>
  Dim customerName = customer.<name>.Value
  ```

  要素へのアクセスは、関数にマップします。

  ```vb
  Function Elements(name As System.Xml.Linq.XName) As _
      System.Collections.Generic.IEnumerable(Of _
          System.Xml.Linq.XNode)
  ```

  このため、上記の例と同等です。

  ```vb
  Dim customerName = customer.Elements("name").Value
  ```

* *属性アクセス*ドットに依存して、Visual Basic 識別子で、名前に依存してドット記号、または XML で、アット マーク。 例えば:

  ```vb
  Dim customer = <customer age="30"/>
  Dim customerAge = customer.@age
  ```

  属性へのアクセスは、関数にマップされます。

  ```vb
  Function AttributeValue(name As System.Xml.Linq.XName) as String
  ```

  このため、上記の例と同等です。

  ```vb
  Dim customerAge = customer.AttributeValue("age")
  ```

  __注意してください。__ `AttributeValue`拡張メソッド (関連する拡張機能プロパティと`Value`) 任意のアセンブリで定義されていません。 拡張メンバーが必要な場合は生成されているアセンブリで自動的に定義します。

* *子孫アクセス*XML の名前が 3 つのドットに従います。 例えば:

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

  子孫へのアクセスは、関数にマップします。

  ```vb
  Function Descendents(name As System.Xml.Linq.XName) As _
      System.Collections.Generic.IEnumerable(Of _
          System.Xml.Linq.XElement)
  ```

  このため、上記の例と同等です。

  ```vb
  Dim customers = company.Descendants("customer")
  ```

XML メンバー アクセス式の基本の式は、値を指定する必要があり、型でなければなりません。

* 要素または子孫にアクセスする場合`System.Xml.Linq.XContainer`または派生型、または`System.Collections.Generic.IEnumerable(Of T)`または派生型では、場所`T`は`System.Xml.Linq.XContainer`または派生型。

* 場合、属性へのアクセス`System.Xml.Linq.XElement`または派生型、または`System.Collections.Generic.IEnumerable(Of T)`または派生型では、場所`T`は`System.Xml.Linq.XElement`または派生型。

XML メンバー アクセス式の名前を空にすることはできません。 名前空間で修飾、インポートによって定義された任意の名前空間を使用できます。 例えば:

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

XML メンバー アクセス式で、山かっこと名の間または dot(s) 後は、空白が許可されていません。 例えば:

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

場合内の型、`System.Xml.Linq`名前空間は、使用し、XML メンバー アクセス式には、コンパイル時エラーが発生します。


## <a name="await-operator"></a>Await 演算子

Await 演算子がセクションで説明されている非同期メソッドに関連する[非同期メソッド](statements.md#async-methods)します。

```antlr
AwaitOperatorExpression
    : 'Await' Expression
    ;
```

`Await` 予約語は、すぐ外側のメソッドまたはラムダ式が表示される場合、`Async`修飾子は、場合に、`Await`を後に表示`Async`修飾子; は予約されている他の場所。 ないプリプロセッサ ディレクティブ内に確保できます。 Await 演算子は予約語であるメソッドまたはラムダ式の本体でのみ使用できます。 すぐ外側のメソッドまたはラムダ内で await 式には発生しませんの本体内で、`Catch`または`Finally`ブロック、または内部の本文を`SyncLock`ステートメント、または内部クエリ式。

Await 演算子は、1 つの式の値として分類する必要があります型を持つ必要があります、*待機可能物*型、または`Object`します。 型が場合`Object`し、すべての処理が実行時まで延期されます。 型`C`する場合は、次のすべてに該当する待機可能なことを言います。

* `C` アクセス可能なインスタンスまたは拡張メソッドがという名前を含む`GetAwaiter`引数がありませんが、いくつかの型を返す`E`;

* `E` という名前の読み取り可能なインスタンスまたは拡張機能プロパティが含まれています`IsCompleted`引数をとらないし、; ブール値の型があります

* `E` アクセス可能なインスタンスまたは拡張メソッドがという名前を含む`GetResult`引数ですこれではありません。

* `E` いずれかを実装して`System.Runtime.CompilerServices.INotifyCompletion`または`ICriticalNotifyCompletion`します。

場合`GetResult`が、 `Sub`、await 式は void として分類されます。 それ以外の場合、await 式は値として分類され、その型は、戻り値の型、`GetResult`メソッド。

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

__注意してください。__ ライブラリの作成者が同じで、継続のデリゲートが呼び出されてパターンに従うお勧めします`SynchronizationContext`としてその`OnCompleted`自体でが呼び出されました。 また、再開のデリゲートは実行されませんが同期的に内、`OnCompleted`スタック オーバーフローが発生するためのメソッド: 代わりに、デリゲートする必要がありますキューに置かれる後続の実行。

コントロール フローに達すると、`Await`演算子、動作は次のようにします。

1.  `GetAwaiter` Await のオペランドのメソッドが呼び出されます。 この呼び出しの結果と呼ばれる、 *awaiter*します。

2.  Awaiter の`IsCompleted`プロパティを取得します。 結果が true の場合。

    21. `GetResult` Awaiter のメソッドが呼び出されます。 場合`GetResult`が関数の場合、await 式の値は、この関数の戻り値。

3.  IsCompleted プロパティが true でない場合。

    31. いずれか`ICriticalNotifyCompletion.UnsafeOnCompleted`awaiter で呼び出される (場合 awaiter の型`E`実装`ICriticalNotifyCompletion`) または`INotifyCompletion.OnCompleted`(それ以外の場合)。 どちらの場合でも、パス、*再開デリゲート*非同期メソッドの現在のインスタンスに関連付けられています。

    32. 現在の非同期メソッドのインスタンスの制御点は中断しています。 でフローが再開、*現在の呼び出し元*(セクションで定義されている[非同期メソッド](statements.md#async-methods))。

    33. 後で再開デリゲートが呼び出される場合

        331. 再開のデリゲートがまず復元`System.Threading.Thread.CurrentThread.ExecutionContext`された時点を`OnCompleted`が呼び出されました
        332. 非同期メソッドのインスタンスの管理ポイントでの制御フローを再開し、(を参照してください[非同期メソッド](statements.md#async-methods))、
        333. 呼び出す場合に、 `GetResult` 2.1 上記のように、awaiter のメソッド。

Await オペランドの型がオブジェクトの場合、この動作は、実行時まで延期されます。

- 手順 1 には呼び出し元の GetAwaiter(); 引数なしで、省略可能なパラメーターを受け取るインスタンス メソッドに実行時にバインド可能性がありますので。
- 引数なしで IsCompleted() プロパティを取得することによって、ブール値への組み込みの変換を試みることで、手順 2 が実現します。
- 手順 3. a にはしようとすると、 `TryCast(awaiter, ICriticalNotifyCompletion)`、これが失敗した場合と`DirectCast(awaiter, INotifyCompletion)`します。

3. a に渡される再開デリゲートは 1 回のみ呼び出すことがあります。 複数回呼び出された場合、動作は定義されません。

