---
ms.openlocfilehash: 5d129dfe24c488a800dcf1707597a24663c1257f
ms.sourcegitcommit: 7bba6e32a84257b40789fc60e76289f7a21c91f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/20/2019
ms.locfileid: "56439718"
---
# <a name="overloaded-method-resolution"></a>オーバー ロードされたメソッドの解決

実際には、オーバー ロードの解決を決定するルールは、オーバー ロードは実際に指定された引数に「最も近い」を検索する対象としています。 パラメーター型、引数の型が一致メソッドの場合、そのメソッドが明らかに最も近いです。 1 つのメソッドはより狭い幅はすべて、パラメーターの型の場合、別のよりも近い、なし機能 (またはと同じ) 他のメソッドのパラメーターの型。 どちらのメソッドのパラメーターが他よりも狭い場合は、しがないの引数に近い方法を決定します。

__注意してください。__ オーバー ロードの解決は考慮されませんメソッドの戻り値の型。 

また、名前付きパラメーターの構文が実際と仮パラメーターの順序付けできない可能性があります、同じに注意してください。

メソッド グループを指定するには、次の手順を使用して、引数リストのグループに最適なメソッドが決まります。 、特定のステップを適用すると、メンバーが残っていない場合、セット内では、コンパイル時エラーが発生します。 1 つのメンバーは、セットに残っている場合のみそのメンバーは、最も適切なメンバーです。 手順は次のとおりです。

1.  最初に、型引数が指定されていない場合は、型パラメーターを持つメソッドが存在する型の推定を適用します。 メソッドの型の推論に成功した場合、推論された型引数は、特定のメソッドの使用されます。 メソッドの型の推定が失敗した場合、そのメソッドは、セットから削除されます。

2.  次に、アクセスできないか該当なしであるセットからすべてのメンバーを排除 (セクション[引数リストへの適用性](overload-resolution.md#applicability-to-argument-list)) 引数リストへ

3.  次に、1 つまたは複数の引数がある場合`AddressOf`、ラムダ式を計算し、または、*果てるレベルの委任*次に示すように各このような引数。 最悪の事態 (最低) の委任でくつろぎレベル場合`N`がの最下位のデリゲートを緩和したトークン レベルよりも悪い`M`から除外`N`セットから。 デリゲートを緩和したトークンのレベルは次のとおりです。

    31. *デリゲートを緩和したトークンのエラー レベル*場合--、`AddressOf`またはラムダをデリゲート型に変換できません。
  
    32. *デリゲートの戻り値の型またはパラメーターを緩和したトークンを縮小*引数の場合--`AddressOf`またはラムダ宣言された型と戻り値の型からデリゲートへの変換の戻り型が縮小します引数が正規のラムダの場合、または、。型の縮小または引数が非同期のラムダとデリゲートの戻り型は return 式のいずれかからデリゲートへの変換が返す`Task(Of T)`とその戻り値の式のいずれかから変換`T`は縮小; 場合引数が、反復子ラムダでは、デリゲートの戻り値の型と`IEnumerator(Of T)`または`IEnumerable(Of T)`に yield オペランドのいずれかからの変換と`T`は縮小します。

    33. *デリゲートを緩和したトークンに署名のない委任を拡大*デリゲート型の場合--`System.Delegate`または`System.MultiCastDelegate`または`System.Object`します。

    34. *戻り値または引数をデリゲート緩和したトークンをドロップ*引数の場合--`AddressOf`ラムダ宣言された戻り値の型とデリゲート型と戻り値の型がないか、または引数が 1 つのラムダまたは式とデリゲート型を返す詳細戻り値の型が不足しています引数の場合、または`AddressOf`ラムダ パラメーターとデリゲート型を持つパラメーターまたはします。

    35. *戻り値の型のデリゲートを緩和したトークンを拡大*引数の場合--`AddressOf`または宣言された戻り値の型のラムダは、デリゲートの戻り値の型から拡大変換と引数が正規のラムダの場合、または場所、すべての return 式からデリゲートの戻り値の型への変換は拡大変換または id を少なくとも 1 つの拡大します。引数が非同期のラムダでは、デリゲートが場合や`Task(Of T)`または`Task`とするすべての return 式から変換`T` / `Object`拡大変換または id を少なくとも 1 つの拡大; がそれぞれ場合引数が、反復子ラムダでは、デリゲートが`IEnumerator(Of T)`または`IEnumerable(Of T)`または`IEnumerator`または`IEnumerable`とするすべての return 式から変換`T` / `Object`は拡大または少なくとも 1 つの拡大を持つ id。

    36. *デリゲート緩和の identity*引数の場合--、`AddressOf`なしで、デリゲートを正確に一致するラムダ式の拡大または縮小、またはパラメーターを返します。 または結果を削除していますか。次に、セットの一部のメンバーが、引数のいずれかに適用できる縮小変換を必要としない場合を実行するすべてのメンバーを除外します。 例:

    ```vb
    Sub f(x As Object)
    End Sub

    Sub f(x As Short)
    End Sub

    Sub f(x As Short())
    End Sub

    f("5") ' picks the Object overload, since String->Short is narrowing
    f(5)   ' picks the Object overload, since Integer->Short is narrowing
    f({5}) ' picks the Object overload, since Integer->Short is narrowing
    f({})  ' a tie-breaker rule subsequent to [3] picks the Short() overload

    ```

4.  次に、削除は、次のように縮小に基づいて行われます。 (、Option Strict がオンの場合、縮小変換を必要とするすべてのメンバーが既に判定できないに注意してください (セクション[引数リストへの適用性](overload-resolution.md#applicability-to-argument-list))、手順 2. で削除します)。

    41. 場合は、セットの一部のインスタンス メンバーが縮小変換は、引数式の型が必要なだけ`Object`、その他のすべてのメンバーを削除します。
    42. のみに絞り込む必要があります 1 つ以上のメンバーがセットに含まれる場合`Object`、呼び出し対象の式が遅延バインディング メソッド アクセスとして再分類し (され、エラーは、メソッド グループを含む型がインターフェイス、またはのいずれか。該当するメンバーが拡張メンバー)。
    43. のみ数値リテラルから縮小変換を必要とするすべての候補がある場合は、選択し残りのすべての応募者の間で最も具体的で次の手順。 優勝者は、数値リテラルからのみに絞り込む必要がある場合に、選択したオーバー ロードの解決の結果としてそれ以外の場合、エラーになります。

    __注意してください。__ この規則の妥当性は、プログラムが疎に型指定された場合 (としてのほとんどまたはすべての変数の宣言は、 `Object`)、オーバー ロードの解決は難しい場合に多くの変換から`Object`は縮小変換します。 (メソッド呼び出しの引数の厳密な型指定が必要)、多くの状況で、適切なオーバー ロードの解決が失敗するオーバー ロードの解決にはなくに呼び出すメソッドが実行時まで延期します。 これにより、キャストなしで成功を柔軟に型指定された呼び出しができます。 影響を及ぼしますが、ただしはその実行の遅延バインディングによる呼び出しでは、呼び出しターゲットへのキャストが必要です`Object`します。 場合は、構造体の値を一時的に値をボックス化する必要があります、つまりです。 最終的に呼び出されるメソッドが、構造体のフィールドを変更しようとすると場合、は、メソッドから戻ると変更が失われます。 インターフェイスは、常に遅延バインディングに対して、型のメンバー、ランタイム クラスまたは構造体、異なる名前を実装するインターフェイスのメンバーがあります。 これにより解決されるため、この特別な規則から除外されます。

5.  次に、インスタンス メソッドは、縮小変換を必要としないセットに残っている場合はその、セットからすべての拡張メソッドを削除します。 例:

    ```vb
    Imports System.Runtime.CompilerServices

    Class C3
        Sub M1(d As Integer)
        End Sub
    End Class

    Module C3Extensions
        <Extension> _
        Sub M1(c3 As C3, c As Long)
        End Sub

        <Extension> _
        Sub M1(c3 As C3, c As Short)
        End Sub
    End Module

    Module Test
        Sub Main()
            Dim c As New C3()
            Dim sVal As Short = 10
            Dim lVal As Long = 20

            ' Calls C3.M1, since C3.M1 is applicable.
            c.M1(sVal)

            ' Calls C3Extensions.M1 since C3.M1 requires a narrowing conversion
            c.M1(lVal)
        End Sub
    End Module
    ```

    __注意してください。__ 拡張メソッドには、こと (スコープに新しい拡張メソッドを含めることがあります) をインポートの追加は発生しませんの呼び出しで拡張メソッドを再バインドする既存のインスタンス メソッドを保証するために該当するインスタンス メソッドがある場合は無視されます。 拡張メソッドにバインドするより安全なアプローチになります (インターフェイスまたは型パラメーターで定義されているものなど) 一部の拡張メソッドのさまざまなスコープを指定します。

6.  次に、if、2 つのメンバー、セットの`M`と`N`、`M`は*特定*(セクション[メンバー/種類を引数リストの特定の特異性](overload-resolution.md#specificity-of-memberstypes-given-an-argument-list))より`N`引数リストを指定するには、排除`N`セットから。 1 つ以上のメンバーがセットには残りのメンバーはなく、同じように引数リストを指定、コンパイル時エラーが発生します。

7.  それ以外の場合、2 つのメンバー、セットの指定された`M`と`N`順序で、次の対フスェケェルェニェヒ ルールを適用します。

    71. 場合`M`ParamArray パラメーターはありませんが、 `N` 、または両方の操作を行いますが、`M`より ParamArray パラメーターに以下の引数を渡します`N`から除外は、`N`セットから。 例:

        ```vb
        Module Test
            Sub F(a As Object, ParamArray b As Object())
                Console.WriteLine("F(Object, Object())")
            End Sub

            Sub F(a As Object, b As Object, ParamArray c As Object())
                Console.WriteLine("F(Object, Object, Object())")
            End Sub

           Sub G(Optional a As Object = Nothing)
              Console.WriteLine("G(Object)")
           End Sub

           Sub G(ParamArray a As Object())
              Console.WriteLine("G(Object())")
           End Sub    Sub Main()
                F(1)
                F(1, 2)
                F(1, 2, 3)
              G()
            End Sub
        End Module
        ```

        上記の例では、次の出力が生成されます。

        ```
        F(Object, Object())
        F(Object, Object, Object())
        F(Object, Object, Object())
        G(Object)
        ```

        __注意してください。__ クラスは、パラメーターを持つメソッドを宣言して、それはも通常のメソッドとして展開されたフォームの一部を含めるには珍しくありません。 により、配列の割り当てを回避することが、paramarray パラメーターを持つメソッドの拡張の形式のときに発生するインスタンスが呼び出されます。

    72. 場合`M`よりも強い派生型で定義されて`N`、排除`N`セットから。 例:

        ```vb
        Class Base
            Sub F(Of T, U)(x As T, y As U)
            End Sub
        End Class

        Class Derived
            Inherits Base

            Overloads Sub F(Of T, U)(x As U, y As T)
            End Sub
        End Class

        Module Test
            Sub Main()
                Dim d As New Derived()

                ' Calls Derived.F
                d.F(10, 10)
            End Sub
        End Module
        ```

        このルールは、拡張メソッドで定義されている型にも適用されます。 例:

        ```vb
        Imports System.Runtime.CompilerServices

        Class Base
        End Class

        Class Derived
            Inherits Base
        End Class

        Module BaseExt
            <Extension> _
            Sub M(b As Base, x As Integer)
            End Sub
        End Module

        Module DerivedExt
            <Extension> _
            Sub M(d As Derived, x As Integer)
            End Sub
        End Module

        Module Test
            Sub Main()
                Dim b As New Base()
                Dim d As New Derived()

                ' Calls BaseExt.M
                b.M(10)

                ' Calls DerivedExt.M 
                d.M(10)
            End Sub
        End Module
        ```

    73. 場合`M`と`N`拡張メソッドは、対象の型の`M`がクラスまたは構造体でありのターゲット型`N`インターフェイスで排除`N`セットから。 例:

        ```vb
        Imports System.Runtime.CompilerServices

        Interface I1
        End Interface

        Class C1
            Implements I1
        End Class

        Module Ext1
            <Extension> _
            Sub M(i As I1, x As Integer)
            End Sub
        End Module

        Module Ext2
            <Extension> _
            Sub M(c As C1, y As Integer)
            End Sub
        End Module

        Module Test
            Sub Main()
                Dim c As New C1()

                ' Calls Ext2.M, because Ext1.M is hidden since it extends
                ' an interface.
                c.M(10)

                ' Calls Ext1.M
                CType(c, I1).M(10)
            End Sub
        End Module
        ```

    74. 場合`M`と`N`は拡張メソッドをおよびのターゲット型`M`と`N`が型パラメーターの代用とターゲットの後に同じ`M`パラメーターの置換が含まれていない型の前に型パラメーターのターゲット型`N`は、次のターゲット型よりも少ない型パラメーターを持ち`N`、排除`N`セットから。 例:

        ```vb
        Imports System.Runtime.CompilerServices

        Module Module1
            Sub Main()
                Dim x As Integer = 1
                x.f(1) ' Calls first "f" extension method

                Dim y As New Dictionary(Of Integer, Integer)
                y.g(1) ' Ambiguity error
            End Sub

            <Extension()> Sub f(x As Integer, z As Integer)
            End Sub

            <Extension()> Sub f(Of T)(x As T, z As T)
            End Sub

            <Extension()> Sub g(Of T)(y As Dictionary(Of T, Integer), z As T)
            End Sub

            <Extension()> Sub g(Of T)(y As Dictionary(Of T, T), z As T)
            End Sub
        End Module
        ```

    75. 型の前に引数が置き換えられた場合、`M`は*ジェネリック以下*(セクション[汎用性](overload-resolution.md#genericity)) よりも`N`、排除`N`セットから。

    76. 場合`M`拡張メソッドでないと`N`排除、`N`セットから。

    77. 場合`M`と`N`拡張メソッドと`M`する前に見つかった`N`(セクション[拡張メソッドのコレクション](expressions.md#extension-method-collection))、排除`N`セットから。 例:

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
                Sub M1(c As C1, y As Integer)
                End Sub
            End Module
        End Namespace

        Namespace N1.N2.N3
            Module Test
                Sub Main()
                    Dim x As New C1()

                    ' Calls N2C1Extensions.M1
                    x.M1(10)
                End Sub
            End Module
        End Namespace
        ```

        拡張メソッドは、同じ手順で検出された場合は、それらの拡張メソッドはあいまいです。 呼び出しは、拡張メソッドを格納していると、通常のメンバーの場合と、拡張メソッドを呼び出すことは、標準のモジュールの名前を使用して常に区別可能性があります。 例:

        ```vb
        Imports System.Runtime.CompilerServices

        Class C1
        End Class

        Module C1ExtA
            <Extension> _
            Sub M(c As C1)
            End Sub
        End Module

        Module C1ExtB
            <Extension> _
            Sub M(c As C1)
            End Sub
        End Module

        Module Main
            Sub Test()
                Dim c As New C1()

                C1.M()               ' Ambiguous between C1ExtA.M and BExtB.M
                C1ExtA.M(c)          ' Calls C1ExtA.M
                C1ExtB.M(c)          ' Calls C1ExtB.M
            End Sub
        End Module
        ```

    78. 場合`M`と`N`両方に必要な型の引数を生成する型の推定と`M`が(つまり各型引数を1つの型を推論)、型引数のいずれかの主要な型を決定するを必要としない`N`排除、`N`セットから。

        __注意してください。__ このルールは、そのオーバー ロードの解決 (で複数の型を型引数の推論はエラーが発生する) 以前のバージョンが成功したことを確認の場合、同じ結果に進みます。

    79. デリゲート作成式のターゲットを解決するのにはオーバー ロードの解決が実行されている場合、`AddressOf`式と、両方のデリゲートと`M`中に関数`N`サブルーチン、排除`N`セットから。 同様に場合、両方のデリゲートと`M`サブルーチン、中に`N`関数の場合は、排除`N`セットから。

    710. 場合`M`明示的な引数は、代わりにすべて省略可能なパラメーターの既定値を使用していないが、`N`から除外して、`N`セットから。

    711. 型の前に引数が置き換えられた場合、`M`が*汎用性のさらに詳しく*(セクション[汎用性](overload-resolution.md#genericity)) よりも`N`から除外`N`セットから。

8. それ以外の場合、呼び出しがあいまいですし、コンパイル時エラーが発生します。

#### <a name="specificity-of-memberstypes-given-an-argument-list"></a>メンバー/種類を引数リストの特定の特異性

メンバー`M`と見なされます*均等に特定*として`N`、引数リストを指定された`A`に各パラメーターを入力する場合、そのシグネチャが同じである場合または`M`対応すると同じですパラメーターの型`N`します。

__注意してください。__ 2 つのメンバーは、拡張メソッドにより、同じシグネチャを持つメソッド グループで終了することができます。 2 つのメンバーはできるも同じように特定するが、型パラメーターまたは paramarray 拡張により、同じシグネチャがありません。

メンバー`M`と見なされます*より具体的*よりも`N`かどうかそのシグネチャは異なるし、少なくとも 1 つのパラメーター入力`M`パラメーターの型よりも特定`N`、および noパラメーターの型`N`パラメーターの型よりも特定`M`します。 2 つのパラメーターを指定された`Mj`と`Nj`引数と一致する`Aj`の型`Mj`と見なされます*より具体的*の型よりも`Nj`場合、次のいずれか条件が true:

* 型から拡大変換が存在する`Mj`型に`Nj`します。 (__に注意してください。__ パラメーターの型は、ここで、実引数に関係なく比較している、ため、拡大変換定数式から数値型に値が適合しないと見なされますここでします。)

* `Aj` リテラルは、 `0`、`Mj`が数値型と`Nj`は列挙型。 (__に注意してください。__ このルールは、必要なため、リテラル`0`任意の列挙型に拡大変換されます。 これにより、列挙型は、基になる型に拡大変換されます、ためにオーバー ロードの解決`0`は、既定よりも優先列挙型の数値型。 受け取りました多くのフィードバックのこの動作が直感に反することです。)

* `Mj` `Nj`は、両方の数値型と`Mj`ものよりも前`Nj`の一覧で`Byte`、 `SByte`、 `Short`、 `UShort`、 `Integer`、 `UInteger`、 `Long`、 `ULong`, `Decimal`, `Single`, `Double`. (__に注意してください。__ 数値型のルールは、特定のサイズの符号付きと符号なし数値型のみがある縮小変換に間ので便利です。 上記のルールは、複数の「自然」数値型を優先して 2 つの型間のつながりを中断します。 これは特に重要ですたとえば、両方に適合する数値リテラルの両方、符号付きと符号なし数値の種類に特定のサイズの拡大型でオーバー ロードの解決を行う場合)。

* `Mj` `Nj`デリゲートを関数の型と戻り値の型は`Mj`の戻り値の型よりも特定`Nj`場合`Aj`メソッドのラムダに分類されますと`Mj`または`Nj`は`System.Linq.Expressions.Expression(Of T)`、し、比較される種類 (デリゲート型でを想定) 型の型引数に置き換えられます。

* `Mj` 型と同じ`Aj`、および`Nj`はありません。 (__に注意してください。__ 興味深いことに注意して以前のルールとは若干異なります (C#) でその C# をデリゲート型の関数は、Visual Basic ではないときに、戻り値の型を比較する前に同一のパラメーター リストを持っている必要があります。)

#### <a name="genericity"></a>汎用性

メンバー`M`判断されました*ジェネリック以下*メンバーよりも`N`次のようにします。

1. 一致するパラメーターの各ペアの場合、`Mj`と`Nj`、`Mj`未満かより均等にジェネリック`Nj`、メソッド、および少なくとも 1 つの型パラメーターに関して`Mj`は入力に関するあまり汎用メソッドのパラメーターです。
2. 一致するパラメーターの各ペアの場合はそれ以外の場合、`Mj`と`Nj`、`Mj`未満かより均等にジェネリック`Nj`、型、および少なくとも 1 つの型パラメーターに関して`Mj`は入力に関するあまり汎用パラメーター入力で、`M`がよりも小さいジェネリック`N`します。

パラメーター`M`パラメーターに均等にジェネリックと見なされます`N`場合、型`Mt`と`Nt`両方参照パラメーターを入力するか、両方ない入力パラメーターを参照してください。 `M` も小さいジェネリックと見なされます`N`場合`Mt`型パラメーターを参照していないと`Nt`は。

例:

```vb
Class C1(Of T)
    Sub S1(Of U)(x As U, y As T)
    End Sub

    Sub S1(Of U)(x As U, y As U)
    End Sub

    Sub S2(x As Integer, y As T)
    End Sub

    Sub S2(x As T, y As T)
    End Sub
End Class

Module Test
    Sub Main()
        Dim x As C1(Of Integer) = New C1(Of Integer)

        x.S1(10, 10)    ' Calls S1(U, T)
        x.S2(10, 10)    ' Calls S2(Integer, T)
    End Sub
End Module
```

カリー化中に修正されている拡張メソッドの型パラメーターは、型、メソッドでない型パラメーターの型パラメーターと見なされます。 例:

```vb
Imports System.Runtime.CompilerServices

Module Ext1
    <Extension> _
    Sub M1(Of T, U)(x As T, y As U, z As U)
    End Sub
End Module

Module Ext2
    <Extension> _
    Sub M1(Of T, U)(x As T, y As U, z As T)
    End Sub
End Module

Module Test
    Sub Main()
        Dim i As Integer = 10

        i.M1(10, 10)
    End Sub
End Module
```

#### <a name="depth-of-genericity"></a>汎用性の深さ

メンバー`M`が判断されました*汎用性のさらに詳しく*メンバーよりも`N`場合、一致するパラメーターの各ペアの`Mj`と`Nj`、`Mj`が大きい以上*汎用性の深さ*よりも`Nj`、および少なくとも 1 つ`Mj`は汎用性の深さが大きいが。 汎用性の深さの定義は次のとおりです。

* 型パラメーター以外のものが、汎用性の型パラメーターの場合よりもさらに詳しく

* 再帰的に構築された型が (型引数の同じ番号) を持つ別の構築型よりもさらに詳しく汎用性の少なくとも 1 つの型引数が汎用性のさらに詳しく型引数に対応する型よりも少ないの深さがない場合もう一方の引数。

* 配列型では、最初の要素の型がある 2 番目の要素の型よりもさらに詳しく汎用性の場合 (同じ次元数) で別の配列型よりも汎用性のさらに詳しくがあります。

例:

```vb
Module Test

    Sub f(Of T)(x As Task(Of T))
    End Sub

    Sub f(Of T)(x As T)
    End Sub

    Sub Main()
        Dim x As Task(Of Integer) = Nothing
        f(x)            ' Calls the first overload
    End Sub
End Module
```

### <a name="applicability-to-argument-list"></a>引数リストへの適用性

メソッドは、*適用*型引数、引数の位置指定、および引数リストを使用するメソッドを呼び出すことができる場合、名前付き引数のセットにします。 次のように、引数リストのパラメーターが照合されますを示します。

1. 最初に、メソッドのパラメーターの一覧の順序で各位置指定引数を一致します。 位置指定引数のパラメーターよりもがあり、最後のパラメーターが、paramarray ではない場合、メソッドは適用されません。 それ以外の場合、paramarray パラメーターは位置指定引数の数と一致する paramarray 要素型のパラメーターを持つ展開されます。 位置指定引数を省略した場合は、paramarray に移動すると、メソッドは適用されません。
2. 次に、各名前付き引数を指定した名前のパラメーターに一致します。 Paramarray パラメーターと一致する、または別の位置または名前付き引数と既に一致、引数と一致する名前付き引数のいずれかに一致しない場合、メソッドは適用されません。
3. 次に、型引数が指定されている場合は、型パラメーター リストとが照合されます。 2 つのリストが同じ数の要素を持たない場合、メソッドは、型引数リストが空でない限りと、適用されません。 型引数リストが空の場合、型の推定は、型引数リストを推測してみてくださいに使用されます。 型の推論が失敗した場合、メソッドは適用されません。 それ以外の場合、型引数は、シグネチャの型パラメーターの代わりに入力されます。対応していないパラメーターが省略可能でない場合、メソッドは適用されません。
4. 引数の式は、パラメーターの型に暗黙的に変換がない場合は、一致すると、し、メソッドは適用されません。
5. パラメーターが ByRef パラメーターの型から、引数の型への暗黙的な変換がない場合は、このメソッドは適用されません。
6. (手順 3 から推論された型引数を含む)、メソッドの制約に違反すると、型引数がある場合、メソッドは適用されません。 例:

```vb
Module Module1
    Sub Main()
        f(Of Integer)(New Exception)
        ' picks the first overload (narrowing),
        ' since the second overload (widening) violates constraints 
    End Sub

    Sub f(Of T)(x As IComparable)
    End Sub

    Sub f(Of T As Class)(x As Object)
    End Sub
End Module
```

メソッドがどちらの展開と展開されていない形式で、該当する 1 つの引数式が paramarray パラメーターと一致する引数式の型が paramarray パラメーターの型と paramarray 要素の型に変換できる場合は、2 つの例外。 引数式の型から paramarray 型への変換を絞り込む場合、メソッドは拡張形式で適用可能なのみ。 引数式が、リテラルの場合`Nothing`メソッドは、展開されていない形式で該当するだけです。 例:

```vb
Module Test
    Sub F(ParamArray a As Object())
        Dim o As Object

        For Each o In a
            Console.Write(o.GetType().FullName)
            Console.Write(" ")
        Next o
        Console.WriteLine()
    End Sub

    Sub Main()
        Dim a As Object() = { 1, "Hello", 123.456 }
        Dim o As Object = a

        F(a)
        F(CType(a, Object))
        F(o)
        F(CType(o, Object()))
    End Sub
End Module
```

上記の例では、次の出力が生成されます。

```
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

最初と最後の呼び出しで`F`、通常の形式`F`はパラメーターの型に引数の型から拡大変換が存在するために適用 (型の両方が`Object()`)、通常の値として渡される引数、およびパラメーター。 2 番目と 3 番目の呼び出し、通常の形式で`F`拡大変換が存在しないため、引数の型からパラメーターの型には適用されません (から変換`Object`に`Object()`縮小)。 ただし、拡張の形式の`F`該当する、1 つの要素は、`Object()`呼び出しによって作成されます。 配列の 1 つの要素が指定された引数の値で初期化されます (それ自体がへの参照、 `Object()`)。

### <a name="passing-arguments-and-picking-arguments-for-optional-parameters"></a>引数を渡すと、省略可能なパラメーターの引数を選択

パラメーターが値を持つパラメーターの場合は、一致する引数の式が値として分類する必要があります。 値では、パラメーターの型に変換され、実行時に、パラメーターとして渡されました。 このパラメーターは参照パラメーターと一致する引数式は、パラメーターと同じ型の変数として分類されます場合、変数への参照がパラメーターとして渡さ、実行時に。

それ以外の場合、一致する引数の式は、変数、値、またはプロパティへのアクセスに分類されるが場合、は、パラメーターの型の一時変数が割り当てられます。 引数式が値として再分類は実行時にメソッドの呼び出しの前には、パラメーターの型に変換され、一時変数に割り当てられています。 一時変数への参照は、パラメーターとしてに渡されます。 メソッドの呼び出しを評価すると、引数式が変数またはプロパティ アクセスに分類される場合は、変数の式またはプロパティ アクセス式に一時変数が割り当てられます。 プロパティ アクセス式がにない場合`Set`アクセサー、割り当ては実行されません。

省略可能なパラメーターが位置引数が指定されていない、コンパイラは、以下に示すように引数を取得します。 常にテスト パラメーターの型をジェネリック型の置換後にします。

* 省略可能なパラメーターに属性がある場合`System.Runtime.CompilerServices.CallerLineNumber`、および呼び出しは、ソース コード内の場所からはその場所の行番号を表す数値リテラルが、パラメーターの型への組み込みの変換し、数値リテラルを使用します。 呼び出しは、複数の行にまたがっている場合は、実装に依存はのどの行で使用するかを選択します。

* 省略可能なパラメーターに属性がある場合`System.Runtime.CompilerServices.CallerFilePath`、および呼び出しは、ソース コード内の場所からはその場所のファイル パスを表す文字列リテラルは、パラメーターの型への組み込みの変換し、文字列リテラルを使用します。 ファイルのパスの形式は、実装によって異なります。

* 省略可能なパラメーターに属性がある場合`System.Runtime.CompilerServices.CallerMemberName`呼び出しが、その型のメンバーの任意の部分に適用する属性または型のメンバーの本文内と、そのメンバー名を表す文字列リテラルは、パラメーターへの組み込みの変換型、文字列リテラルを使用します。 呼び出しではプロパティ アクセサーまたはカスタム イベント ハンドラーの一部であるし、メンバー名が使用されるプロパティまたはイベント自体の。 演算子またはコンストラクターの一部である呼び出し、実装に固有の名前を使用します。

上記のいずれの場合は省略可能なパラメーターの既定値が使用 (または`Nothing`既定値が指定されていない場合)。 数より多い場合、上記のいずれかのどちらを使用する選択肢は実装に依存します。

`CallerLineNumber`と`CallerFilePath`属性は、ログ記録のために便利です。 `CallerMemberName`を実装するために役立ちます`INotifyPropertyChanged`します。 例を示します。

```vb
Sub Log(msg As String,
        <CallerFilePath> Optional file As String = Nothing,
        <CallerLineNumber> Optional line As Integer? = Nothing)
    Console.WriteLine("{0}:{1} - {2}", file, line, msg)
End Sub

WriteOnly Property p As Integer
    Set(value As Integer)
        Notify(_p, value)
    End Set
End Property

Private _p As Integer

Sub Notify(Of T As IEquatable(Of T))(ByRef v1 As T, v2 As T,
        <CallerMemberName> Optional prop As String = Nothing)
    If v1 IsNot Nothing AndAlso v1.Equals(v2) Then Return
    If v1 Is Nothing AndAlso v2 Is Nothing Then Return
    v1 = v2
    RaiseEvent PropertyChanged(Me, New PropertyChangedEventArgs(prop))
End Sub
```

上記のオプション パラメーターに加えて Microsoft Visual Basic では、(つまり、DLL の参照) からのメタデータからインポートされた場合は、いくつか追加のオプション パラメーターも認識します。

* メタデータからインポートする時に Visual Basic も扱いますパラメーター`<Optional>`パラメーターは省略可能であると示しています場合でも、このことはできませんが、省略可能なパラメーターが既定値はありません、宣言をインポートすることは、この方法で。使用して表現、`Optional`キーワード。

* 省略可能なパラメーターに属性がある場合`Microsoft.VisualBasic.CompilerServices.OptionCompareAttribute`、数値リテラルの 1 または 0 がパラメーターの型に変換し、リテラルの 1 場合を引数として使用して、コンパイラ`Option Compare Text`特殊効果、またはリテラルの場合は 0 では、`Optional Compare Binary`が有効でします。

* 省略可能なパラメーターに属性がある場合`System.Runtime.CompilerServices.IDispatchConstantAttribute`、型があると`Object`、コンパイラは引数を使用し、既定値は指定しません`New System.Runtime.InteropServices.DispatchWrapper(Nothing)`します。

* 省略可能なパラメーターに属性がある場合`System.Runtime.CompilerServices.IUnknownConstantAttribute`、型があると`Object`、コンパイラは引数を使用し、既定値は指定しません`New System.Runtime.InteropServices.UnknownWrapper(Nothing)`します。

* 省略可能なパラメーターの型の場合`Object`、コンパイラは引数を使用し、既定値は指定しません`System.Reflection.Missing.Value`します。

### <a name="conditional-methods"></a>条件付きメソッド

Invocation 式が参照する対象のメソッドがインターフェイスのメンバーでないサブルーチンの場合と、メソッドが 1 つまたは複数`System.Diagnostics.ConditionalAttribute`属性、式の評価によって異なりますで定義した条件付きコンパイル定数ソース ファイルをポイントします。 属性の各インスタンスには、文字列は、条件付きコンパイル定数の名前を指定します。 条件付きコンパイル ステートメントの一部の場合と同様に、各条件付きコンパイル定数が評価されます。 評価されると、定数`True`、通常実行時に式が評価されます。 評価されると、定数`False`式はすべての評価されません。

属性を探すときに、オーバーライド可能なメソッドの最多派生宣言がチェックされます。

__注意してください。__ この属性は、関数またはインターフェイスのメソッドでは無効ですし、メソッドのいずれかの種類に指定されている場合は無視されます。 したがって、条件付きメソッドは、呼び出しステートメントでのみ表示されます。

### <a name="type-argument-inference"></a>型引数の推論

型引数を指定せずに型パラメーターを持つメソッドが呼び出されたときに*入力引数を推定*し、呼び出しの引数の型を推測するために使用します。 これにより、型引数を普通に推論するときに、型パラメーターを持つメソッドを呼び出すために使用するより自然な構文です。 たとえば、次のメソッド宣言を考えてみます。

```vb
Module Util
    Function Choose(Of T)(b As Boolean, first As T, second As T) As T
        If b Then
            Return first
        Else
            Return second
        End If
    End Function
End Class
```

呼び出すことは、`Choose`型引数を明示的に指定しないでメソッド。

```vb
' calls Choose(Of Integer)
Dim i As Integer = Util.Choose(True, 5, 213)
' calls Choose(Of String)
Dim s As String = Util.Choose(False, "a", "b") 
```

型引数を推定、型引数を介して`Integer`と`String`メソッドに引数から決定されます。

入力引数の推論が発生した*する前に*これら 2 つの式の再分類の種類がかかる場合のため、ラムダ メソッドまたはメソッドのポインター引数リスト内で式の再分類は実行、既知であるパラメーターです。  与えられた一連の引数の`A1,...,An`、一連のパラメーターに一致する`P1,...,Pn`とメソッドのパラメーターの入力セット`T1,...,Tn`引数とメソッドの型パラメーター間の依存関係が次のように収集された最初。

* 場合`An`は、`Nothing`リテラルには、依存関係は生成されません。

* 場合`An`ラムダ メソッドの型が`Pn`が構築されたデリゲート型または`System.Linq.Expressions.Expression(Of T)`ここで、`T`が構築されたデリゲート型では、

* ラムダ メソッドのパラメーターの型は、対応するパラメーターの型から推論する場合`Pn`、パラメーターの型は、メソッド型パラメーターに依存して`Tn`、し`An`に依存している`Tn`します。

* ラムダ メソッドのパラメーターの型が指定されているかどうかと、対応するパラメーターの型`Pn`、メソッド型パラメーターに依存`Tn`、し`Tn`に依存している`An`します。

* 戻り値の型の場合`Pn`、メソッド型パラメーターに依存`Tn`、し`Tn`に依存している`An`します。

* 場合`An`メソッドのポインターの種類が`Pn`は、構築されたデリゲート型です。

* 戻り値の型の場合`Pn`、メソッド型パラメーターに依存`Tn`、し`Tn`に依存している`An`します。

* 場合`Pn`が構築された型の種類`Pn`、メソッド型パラメーターに依存`Tn`、し`Tn`に依存している`An`します。

* それ以外の場合、依存関係は生成されません。

依存関係を収集した後、依存関係を持たない任意の引数は除外されます。 メソッド型パラメーターには、出力の依存関係 (つまり、メソッド型パラメーターに依存しない引数) がない、型の推論は失敗します。 それ以外の場合、残りの引数とメソッドの型パラメーターは、厳密に接続されているコンポーネントにグループ化されます。 厳密に接続されているコンポーネントとは、コンポーネント内の任意の要素が他の要素への依存関係で到達可能な引数と、メソッド型パラメーターのセットです。

厳密に接続されているコンポーネントの位相的並べ替えおよびトポロジの順序で処理します。

* 厳密に型指定されたコンポーネントには、1 つだけの要素が含まれている場合

  * 要素が既にマークされている完全なスキップします。

  * 要素が、引数の場合は型ヒント引数からを依存していると、要素を完了としてマークするメソッドの型パラメーターを追加します。 引数が型を推論する必要があるパラメーターを持つラムダ メソッドの場合、推論`Object`これらのパラメーターの型。

  * 要素が、メソッド型パラメーターの場合は、引数の型ヒントの間での主要な型にして、要素を完了としてマークするメソッドの型パラメーターを推測します。 場合は、型ヒントでは、上に、配列要素の制限がある、指定された型の配列間で有効な変換のみは (つまり共変と組み込みの配列の変換) と見なされます。 型ヒントは、汎用引数の制限を上にあるを場合は、恒等変換のみと見なされます。 主要な型を選択しない場合は、推論が失敗します。 任意のラムダ メソッド引数の型は、このメソッドの型パラメーターに依存している場合、型がラムダ メソッドに反映されます。

* 厳密に型指定されたコンポーネントには、複数の要素が含まれている場合、コンポーネントが循環しています。

  * コンポーネント内の要素を各メソッドの型パラメーター、メソッドの型パラメーターは、完了マークされていない引数に依存している場合は、推論プロセスの最後にチェックされるアサーションにその依存関係を変換します。

  * 厳密に型指定されたコンポーネントの判断ポイントで推論プロセスを再起動します。

すべてのメソッドの型パラメーターの型の推定が成功すると、アサーションに変更されたすべての依存関係がチェックされます。 アサーションは、引数の型がメソッドの型パラメーターの推論された型に暗黙的に変換できる場合は成功します。 アサーションに失敗した場合は、型引数の推論が失敗します。

引数の型指定された`Ta`引数`A`パラメーターの型と`Tp`パラメーターの`P`、型ヒントは次のように生成されます。

* 場合`Tp`メソッド型パラメーターを含まない場合、ヒントは生成されません。

* 場合`Tp`と`Ta`、同じランクの配列型は、置換`Ta`と`Tp`要素の種類の`Ta`と`Tp`に配列要素の制限は、このプロセスを再起動します。

* 場合`Tp`し、メソッド型パラメーターは、`Ta`存在する場合は、現在の制限では、型ヒントとして追加します。

* 場合`A`ラムダ メソッドと`Tp`が構築されたデリゲート型または`System.Linq.Expressions.Expression(Of T)`ここで、`T`ラムダ メソッド パラメーターの種類ごとの構築されたデリゲート型は、`TL`とデリゲート パラメーター型に対応します。`TD`、置き換える`Ta`で`TL`と`Tp`で`TD`の制限なしでプロセスを再起動しています。 次に置き換えます。`Ta`ラムダ メソッドの戻り値の型と。

  * 場合`A`正規のラムダ メソッドで置き換えます`Tp`デリゲート型の戻り値の型
  * 場合`A`非同期のラムダ メソッドであるし、デリゲート型の戻り値の型がフォーム`Task(Of T)`一部`T`、置き換える`Tp`を`T`;
  * 場合`A`反復子ラムダ メソッドは、デリゲート型の戻り値の型がフォーム`IEnumerator(Of T)`または`IEnumerable(Of T)`一部`T`、置換`Tp`を`T`します。
  * 次に、制限なしでプロセスを再起動します。

* 場合`A`メソッドのポインターと`Tp`は構築のパラメーターの型を使用してデリゲート型を`Tp`どの方法が示されるを決定するに最も適した`Tp`。 最適なメソッドがある場合は、置き換える`Ta`メソッドの戻り値の型と`Tp`再起動プロセスの制限なしで、デリゲート型の戻り値の型。

* それ以外の場合、`Tp`構築された型である必要があります。 指定された`TG`のジェネリック型`Tp`、

  * 場合`Ta`は`TG`から継承`TG`、型を実装または`TG`に 1 回だけの一致する型引数はそれぞれ、`Tax`から`Ta`と`Tpx`から`Tp`を置き換えます`Ta`で`Tax`と`Tp`で`Tpx`ジェネリック引数の制限を使用して、プロセスを再起動します。

  * それ以外の場合、ジェネリック メソッドの型の推定が失敗します。

型の推論の成功した場合でと、それ自体の保証されません、メソッドは、適用されます。
