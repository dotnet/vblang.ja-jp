---
ms.openlocfilehash: a9499e51a67a9b311ae54410c001f8bd22c30d65
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306069"
---
# <a name="overloaded-method-resolution"></a>オーバーロードされたメソッドの解決

実際には、オーバーロードの解決を決定するための規則は、指定された実際の引数に "最も近い" オーバーロードを見つけることを目的としています。 パラメーターの型が引数の型と一致するメソッドがある場合、そのメソッドは明らかに最も近いものになります。 そのようにしないと、1つのメソッドは、他のメソッドのパラメーター型よりも幅が狭く (または同じである)、他のメソッドよりも近くにあります。 どちらのパラメーターも他のパラメーターよりも狭い場合、では、どのメソッドが引数に近いかを判断する方法はありません。

__付箋.__ オーバーロードの解決では、メソッドの予期される戻り値の型は考慮されません。 

また、名前付きパラメーターの構文が原因で、実際のパラメーターと仮パラメーターの順序が同じでない場合もあることに注意してください。

メソッドグループが指定されている場合、引数リストのグループ内の最も適切なメソッドは、次の手順を使用して決定されます。 特定のステップを適用した後に、セット内にメンバーが残っていない場合は、コンパイル時エラーが発生します。 セットに含まれるメンバーが1つだけの場合は、そのメンバーが最も適切なメンバーになります。 手順は次のとおりです。

1.  まず、型引数が指定されていない場合は、型パラメーターを持つ任意のメソッドに型の推定を適用します。 メソッドに対して型の推定が成功すると、その特定のメソッドに対して推論される型引数が使用されます。 メソッドの型の推定が失敗した場合、そのメソッドはセットから除外されます。

2.  次に、アクセスできない、または適用されていないセットのすべてのメンバー[を引数リスト](overload-resolution.md#applicability-to-argument-list)に対して削除します。

3.  次に、1つ以上の引数が `AddressOf` またはラムダ式である場合は、次のように各引数の*デリゲート緩和レベル*を計算します。 `N` の最も低い (最低) デリゲート緩和 level が `M`の最も低いデリゲートの緩和レベルよりも悪い場合は、セットから `N` を削除します。 Delegate 緩和レベルは次のとおりです。

    31. *緩和レベルを委任*するときにエラーが発生しました--`AddressOf` またはラムダをデリゲート型に変換できません。
  
    32. *戻り値の型またはパラメーターの縮小デリゲート緩和*。引数が `AddressOf` の場合、または宣言された型のラムダで、戻り値の型からデリゲートの戻り値の型への変換が縮小される場合は。引数が標準のラムダで、その戻り式からデリゲートの戻り値の型への変換が縮小されている場合、または引数が非同期 `Task(Of T)` ラムダであり、その戻り値の式から `T` への変換が縮小される場合は。または、引数が反復子ラムダであり、デリゲートの戻り値の `IEnumerator(Of T)` 型がまたは `IEnumerable(Of T)` であり、その yield オペランドから `T` への変換が縮小されている場合は、またはです。

    33. デリゲート*緩和をシグネチャなしでデリゲートするように拡張*します--デリゲート型が `System.Delegate`、`System.MultiCastDelegate` または `System.Object`。

    34. *Drop return または arguments delegate 緩和*--引数が `AddressOf` か、宣言された戻り値の型を持つラムダで、デリゲート型に戻り値の型がありません。引数が1つ以上の return 式を持つラムダであり、デリゲート型に戻り値の型がない場合は。引数が `AddressOf` か、パラメーターを持たないラムダで、デリゲート型にパラメーターがある場合は。

    35. *緩和戻り値の型の上位変換デリゲート*。引数が `AddressOf` か、宣言された戻り値の型を持つラムダで、戻り値の型からデリゲートの型への拡大変換が存在する場合は、または、引数が通常のラムダであり、すべての戻り式からデリゲートの戻り値の型への変換が、少なくとも1つの拡大を伴う、拡大または id である場合、または、引数が非同期ラムダであり、デリゲートが `Task(Of T)` または `Task` であり、すべての戻り式から `T`/`Object` への変換が、それぞれ少なくとも1つの拡大を伴うまたは id である場合、または、引数が反復子ラムダであり、デリゲートが `IEnumerator(Of T)` または `IEnumerable(Of T)` または `IEnumerator` または `IEnumerable` で、すべての戻り式から `T`/への変換が、少なくとも1つの拡大による拡大または id である場合。`Object`

    36. *Id デリゲート緩和*--引数がデリゲートと完全に一致する `AddressOf` またはラムダ。パラメーターの拡大、縮小、または削除を行わず、またはを返します。次に、セットの一部のメンバーが、引数のいずれかに適用される縮小変換を必要としない場合は、を実行するすべてのメンバーを削除します。 例 :

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

4.  次に、次のように縮小に基づいて削除を行います。 (Option Strict がオンの場合、縮小が必要なすべてのメンバーは、既に適用されていないと判断されていることに注意してください (「[引数リストへの適用](overload-resolution.md#applicability-to-argument-list)」セクションを参照してください)。

    41. Set の一部のインスタンスメンバーで、引数式の型が `Object`である縮小変換のみが必要な場合は、他のすべてのメンバーを削除します。
    42. `Object`からの縮小のみを必要とする複数のメンバーがセットに含まれている場合、呼び出し先の式は遅延バインディングされたメソッドアクセスとして再分類されます (メソッドグループを含む型がインターフェイスの場合、または該当するメンバーのいずれかが拡張メンバーである場合は、エラーが表示されます)。
    43. 数値リテラルからの縮小のみを必要とする候補がある場合は、次の手順で残りのすべての候補の中から最も限定的なものを選択します。 勝者が数値リテラルのみを縮小する必要がある場合は、オーバーロードの解決の結果として選択されます。それ以外の場合はエラーになります。

    __付箋.__ この規則の理由は、プログラムが弱い型指定 (つまり、ほとんどまたはすべての変数が `Object`として宣言されている) の場合、`Object` からの多くの変換が縮小されると、オーバーロードの解決が困難になることがあります。 多くの状況でオーバーロードの解決が失敗する (メソッド呼び出しに引数を厳密に入力する必要がある) のではなく、呼び出しに適したオーバーロードされたメソッドが実行時まで遅延されることを解決します。 これにより、緩やかに型指定された呼び出しを、追加のキャストなしで成功させることができます。 ただし、このような副作用は残念ですが、遅延バインディングの呼び出しを実行するには、呼び出しターゲットを `Object`にキャストする必要があります。 構造体の値の場合、これは値が一時的なにボックス化される必要があることを意味します。 メソッドが最終的に構造体のフィールドを変更しようとした場合、メソッドが返されると、この変更は失われます。 遅延バインディングは常にランタイムクラスまたは構造体型のメンバーに対して解決されるため、インターフェイスはこの特別な規則から除外されます。これは、実装するインターフェイスのメンバーとは名前が異なる場合があります。

5.  次に、縮小が不要なインスタンスメソッドがセットに残っている場合は、セットからすべての拡張メソッドを削除します。 例 :

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

    __付箋.__ 拡張メソッドは、(新しい拡張メソッドをスコープに取り込む可能性がある) インポートの追加によって既存のインスタンスメソッドに対する呼び出しが拡張メソッドに再バインドされないことを保証するために、適用可能なインスタンスメソッドがある場合は無視されます。 一部の拡張メソッド (インターフェイスや型パラメーターで定義されたもの) の範囲が広いため、これは拡張メソッドにバインドするためのより安全な方法です。

6.  次に、set `M` と `N`の2つのメンバーが指定されている場合は、引数リストに指定された `N` よりも `M` より*具体的*に ([引数リストが指定されたメンバー/型の特異性](overload-resolution.md#specificity-of-memberstypes-given-an-argument-list)がある)、set から `N` が削除されます。 セットに複数のメンバーが残っており、その他のメンバーが引数リストに等しくない場合は、コンパイル時エラーが発生します。

7.  それ以外の場合は、セットの2つのメンバー (`M` と `N`が指定されている場合は、次のように一致する規則を順に適用します。

    71. `M` に ParamArray パラメーターがなく、`N` が実行されている場合、または両方とも `M` が `N` の場合よりも ParamArray パラメーターに渡す引数が少なくなる場合は、セットから `N` を削除します。 例 :

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

        ```console
        F(Object, Object())
        F(Object, Object, Object())
        F(Object, Object, Object())
        G(Object)
        ```

        __付箋.__ クラスが paramarray パラメーターを使用してメソッドを宣言する場合、一部の拡張されたフォームも通常のメソッドとして含めることは珍しくありません。 これにより、paramarray パラメーターを使用して拡張された形式のメソッドが呼び出されたときに発生する配列インスタンスの割り当てを回避できます。

    72. `N`よりも多くの派生型で `M` が定義されている場合は、セットから `N` を削除します。 例 :

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

        この規則は、拡張メソッドが定義されている型にも適用されます。 例 :

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

    73. `M` と `N` が拡張メソッドであり、`M` のターゲット型がクラスまたは構造体であり、`N` のターゲット型がインターフェイスである場合は、セットからの `N` を削除します。 例 :

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

    74. `M` と `N` が拡張メソッドであり、`M` と `N` のターゲットの型が型パラメーターの置換の後に同一であり、型パラメーターの置換の前に `M` のターゲットの型が型パラメーターを含まないが、`N` のターゲット型がである場合は、`N`の対象の型よりも型パラメーターが少なくなります。`N` 例 :

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

    75. 型引数が置換される前に、`M` が `N`よりも*低いジェネリック*(セクション[Genericity](overload-resolution.md#genericity)) の場合は、セットから `N` を削除します。

    76. `M` が拡張メソッドではなく `N` がの場合は、セットから `N` を削除します。

    77. `M` と `N` が拡張メソッドであり、`N` (セクション[拡張メソッドのコレクション](expressions.md#extension-method-collection)) の前に `M` が見つかった場合は、セットからの `N` を削除します。 例 :

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

        拡張メソッドが同じ手順で見つかった場合、これらの拡張メソッドはあいまいです。 この呼び出しは、拡張メソッドを含む標準モジュールの名前を使用し、通常のメンバーであるかのように拡張メソッドを呼び出すことで、常に明確することができます。 例 :

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

    78. `M` とが型引数を生成するために必要な型の推論の両方を `N` 場合、`M` では、型引数のいずれか (つまり、1つの型に対して推論される型引数) の中で最も優先される型を決定する必要 `N` `N` がありませんでした。

        __付箋.__ この規則により、以前のバージョンで成功したオーバーロードの解決 (型引数に対して複数の型を推論するとエラーが発生する) が確実になり、同じ結果が生成されます。

    79. `AddressOf` 式からデリゲート作成式のターゲットを解決するためにオーバーロードの解決が行われており、デリゲートと `M` の両方が関数である場合 `N` がサブルーチンである場合は、セットからの `N` を削除します。 同様に、デリゲートと `M` の両方がサブルーチンで、`N` が関数の場合は、セットから `N` を削除します。

    710. `M` が明示的な引数の代わりに省略可能なパラメーターの既定値を使用しなかったが、`N` した場合は、セットから `N` を削除します。

    711. 型引数が置換される前に、`M` が `N`よりも*深さ genericity* (セクション[genericity](overload-resolution.md#genericity)) を超える場合は、セットから `N` を削除します。

8. それ以外の場合、呼び出しはあいまいになり、コンパイル時エラーが発生します。

#### <a name="specificity-of-memberstypes-given-an-argument-list"></a>引数リストを指定した場合のメンバー/型の特異性

メンバー `M` は、引数リスト `A`が指定されている場合は *`N`と同じように扱われ*ます。シグネチャが同じである場合、または `M` の各パラメーターの型が `N`の対応するパラメーターの型と同じである場合。

__付箋.__ 2つのメンバーは、拡張メソッドによって同じシグネチャを持つメソッドグループに終了できます。 2つのメンバーを同じように指定することもできますが、型パラメーターまたは paramarray 拡張のために同じシグネチャを持つことはできません。

メンバー `M` は、シグネチャが異なる場合は `N` よりも*具体的*であると見なされます。 `M` 内の少なくとも1つのパラメーターの型が `N`のパラメーターの型よりも限定的であり、`N` のパラメーターの型が `M`のパラメーターの型よりも固有ではありません。 引数 `Aj`と一致するパラメーター `Mj` と `Nj` のペアが指定されている場合、次のいずれかの条件に該当する場合、`Mj` の型は `Nj` の型よりも*具体的*なものと見なされます。

* `Mj` の型から `Nj`型への拡大変換が存在します。 (__注:__ この場合、パラメーターの型は、実際の引数に関係なく比較されるため、定数式から数値型への拡大変換は、この場合は考慮されません。

* `Aj` はリテラル `0`であり、`Mj` は数値型で `Nj` は列挙型です。 (__注:__ リテラル `0` 列挙型に拡大変換されるため、この規則が必要です。 列挙型は基になる型に拡大変換されるため、`0` でのオーバーロードの解決では、既定で数値型に対して列挙型を優先します。 この動作が反するていたというフィードバックが多数寄せられています)。

* `Mj` と `Nj` は両方とも数値型であり、`Mj` は、リスト `Byte`、`SByte`、`Short`、`UShort`、`Integer`、`UInteger`、`Long`、`ULong`、`Decimal`、`Single`、`Double`の `Nj` より前のものです。 (__注:__ 数値型に関する規則は便利です。これは、特定のサイズの符号付き数値型と符号なし数値型の間で縮小変換が行われるためです。 上記の規則は、2つの型の間の相互関係を分割し、より "自然な" 数値型を優先します。 これは、特定のサイズの符号付きおよび符号なしの数値型の両方に拡大変換される型 (両方に収まる数値リテラルなど) に対してオーバーロードの解決を実行する場合に特に重要です。

* `Mj` と `Nj` はデリゲート関数型であり、`Mj` の戻り値の型は `Nj` の戻り値の型よりも具体的であり、`Aj` がラムダメソッドとして分類され、`Mj` または `Nj` が `System.Linq.Expressions.Expression(Of T)`場合、型の型引数 (デリゲート型であると仮定) は比較対象の型に置き換えられます。

* `Mj` は `Aj`の型と同じであり、`Nj` はありません。 (__注:__ 前の規則はとは若干異なることに注意しC#てください。 C#では、戻り値の型を比較する前に、デリゲート関数型のパラメーターリストが同一である必要がありますが、Visual Basic はそうではありません)。

#### <a name="genericity"></a>Genericity

メンバー `M` は、次のように、メンバー `N` よりも*汎用的*ではないと判断されます。

1. 一致するパラメーターの各ペア `Mj` および `Nj`の場合、`Mj` は、メソッドの型パラメーターに関しては `Nj` よりも低いか、または等しくありません。また、メソッドの型パラメーターに関して、少なくとも1つの `Mj` がジェネリックではないことになります。
2. それ以外の場合、一致するパラメーターの各ペア `Mj` および `Nj`では、`Mj` が型の型パラメーターに関して `Nj` よりも低いか、または同等であり、型の型パラメーターに関して、少なくとも1つの `Mj` が型パラメーターに対して汎用的ではない場合、`M` は `N`より汎用的ではありません。

パラメーター `M` は、パラメーター `N` 型 `Mt` と `Nt` 両方が型パラメーターを参照する場合、または両方とも型パラメーターを参照しない場合に、パラメーターと同じようにジェネリックと見なされます。 `M` は、`Mt` が型パラメーターを参照 `Nt` していない場合に、`N` よりも汎用的ではないと見なされます。

例 :

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

カリー化中に修正された拡張メソッドの型パラメーターは、メソッドの型パラメーターではなく、型の型パラメーターと見なされます。 例 :

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

#### <a name="depth-of-genericity"></a>Genericity の深さ

メンバー `M` は、一致するパラメーターの各ペア `Mj` および `Nj`では、`Mj` の*genericity の深さ*が `Nj`よりも大きいか等しいか、または1つ以上の `Mj` が genericity の深さを上回っている場合に、メンバー `N` よりも*詳細な genericity*を持つことが決定されます。 Genericity の深さは次のように定義されています。

* 型パラメーター以外のものは、型パラメーターよりも genericity の深さが多くなります。

* 少なくとも1つの型引数の深さが genericity で、型引数の深さが対応する型よりも少ない場合、構築された型は再帰的に (型引数の数が同じである) 他の構築された型と比べて genericity の深さが大きくなります。もう一方の引数。

* 配列型は、1番目の要素の型が2番目の要素の型よりも genericity の深さを持つ場合、(次元数が同じ) 別の配列型よりも genericity の深さが大きくなります。

例 :

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

引数リストを使用してメソッドを呼び出すことができる場合、メソッドは、型引数のセット、位置指定引数、および名前付き引数に*適用*されます。 引数リストは、次のようにパラメーターリストと照合されます。

1. 最初に、各位置引数をメソッドパラメーターの一覧に一致させます。 パラメーターよりも位置指定引数が多く、最後のパラメーターが paramarray でない場合、メソッドは適用できません。 それ以外の場合、paramarray パラメーターは、位置引数の数と一致するように、paramarray 要素型のパラメーターを使用して展開されます。 Paramarray に入る位置引数を省略した場合、メソッドは適用されません。
2. 次に、各名前付き引数を、指定した名前のパラメーターに一致させます。 名前付き引数のいずれかが一致しなかった場合、paramarray パラメーターに一致する場合、または既に別の位置指定引数または名前付き引数と一致した引数と一致する場合、メソッドは適用できません。
3. 次に、型引数が指定されている場合は、型パラメーターリストと照合されます。 2つのリストに同じ数の要素がない場合、型引数リストが空でない限り、メソッドは適用されません。 型引数リストが空の場合、型引数リストを試すために型の推定が使用されます。 Type 推論が失敗した場合、メソッドは適用されません。 それ以外の場合、型引数はシグネチャの型パラメーターの代わりに格納されます。一致していないパラメーターが省略可能でない場合、メソッドは適用できません。
4. 引数式が一致するパラメーターの型に暗黙的に変換できない場合、メソッドは適用されません。
5. パラメーターが ByRef で、パラメーターの型から引数の型への暗黙的な変換がない場合、メソッドは適用されません。
6. 型引数がメソッドの制約に違反している場合 (手順3の推定型引数を含む)、メソッドは適用されません。 例 :

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

単一の引数式が paramarray パラメーターと一致し、引数式の型が paramarray パラメーターの型と paramarray 要素の型の両方に変換可能な場合、メソッドは展開されているフォームと展開されていない形式の両方に適用できます。2つの例外があります。 引数式の型から paramarray 型への変換が縮小されている場合、メソッドは拡張された形式でのみ適用されます。 引数式がリテラル `Nothing`の場合、メソッドは、その展開されていない形式でのみ適用できます。 例 :

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

```console
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

`F`の最初と最後の呼び出しでは、標準形式の `F` が適用されます。これは、引数の型からパラメーターの型 (両方とも `Object()`) に拡大変換が存在し、引数が通常の値パラメーターとして渡されるためです。 2番目と3番目の呼び出しでは、標準形式の `F` は適用されません。これは、引数の型からパラメーターの型への拡大変換が存在しないためです (`Object` から `Object()` への変換は縮小)。 ただし、`F` の展開された形式が適用可能であり、1つの要素 `Object()` が呼び出しによって作成されます。 配列の1つの要素が、指定された引数の値 (それ自体が `Object()`への参照) で初期化されます。

### <a name="passing-arguments-and-picking-arguments-for-optional-parameters"></a>渡す (引数を)、省略可能なパラメーターの引数を選択する

パラメーターが値パラメーターの場合は、一致する引数式を値として分類する必要があります。 値は、パラメーターの型に変換され、実行時にパラメーターとして渡されます。 パラメーターが参照パラメーターであり、一致する引数式が、パラメーターと同じ型を持つ変数として分類されている場合は、変数への参照が実行時にパラメーターとして渡されます。

それ以外の場合、一致する引数式が変数、値、またはプロパティアクセスとして分類されると、パラメーターの型の一時変数が割り当てられます。 実行時にメソッドが呼び出される前に、引数式が値として再分類され、パラメーターの型に変換され、一時変数に割り当てられます。 次に、一時変数への参照がパラメーターとして渡されます。 メソッドの呼び出しが評価された後、引数式が変数またはプロパティアクセスとして分類されている場合、一時変数は変数式またはプロパティアクセス式に割り当てられます。 プロパティアクセス式に `Set` アクセサーがない場合、割り当ては実行されません。

引数が指定されていない省略可能なパラメーターの場合、コンパイラは、次に示すように引数を選択します。 いずれの場合も、ジェネリック型の置換後にパラメーター型に対してテストされます。

* 省略可能なパラメーターに属性 `System.Runtime.CompilerServices.CallerLineNumber`があり、呼び出しがソースコード内の場所からのものであり、その位置の行番号を表す数値リテラルにパラメーター型への組み込みの変換がある場合は、数値リテラルが使用されます。 呼び出しが複数行にまたがっている場合、使用する行の選択は実装に依存します。

* 省略可能なパラメーターに属性 `System.Runtime.CompilerServices.CallerFilePath`があり、呼び出しがソースコード内の場所からのものであり、その場所のファイルパスを表す文字列リテラルにパラメーター型への組み込みの変換がある場合は、文字列リテラルが使用されます。 ファイルパスの形式は実装に依存します。

* 省略可能なパラメーターに属性 `System.Runtime.CompilerServices.CallerMemberName`があり、呼び出しが型のメンバーの本体またはその型のメンバーの任意の部分に適用された属性に含まれており、そのメンバー名を表す文字列リテラルにパラメーター型への組み込みの変換がある場合は、文字列リテラルが使用されます。 プロパティアクセサーまたはカスタムイベントハンドラーの一部である呼び出しの場合、使用されるメンバー名は、プロパティまたはイベント自体の名前になります。 演算子またはコンストラクターの一部である呼び出しの場合は、実装固有の名前が使用されます。

上記のいずれも当てはまらない場合は、省略可能なパラメーターの既定値が使用されます (既定値が指定されていない場合は `Nothing`)。 また、上記のいずれかが当てはまる場合は、実装に依存するオプションが使用されます。

`CallerLineNumber` 属性と `CallerFilePath` 属性は、ログ記録に便利です。 `CallerMemberName` は、`INotifyPropertyChanged`を実装する場合に便利です。 次に例を示します。

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

Microsoft Visual Basic は、上記の省略可能なパラメーターに加えて、メタデータからインポートした場合 (つまり、DLL 参照から)、いくつかの追加のオプションパラメーターも認識します。

* メタデータからインポートした場合、Visual Basic は、パラメーターが省略可能であることを示すように、パラメーター `<Optional>` も扱います。この方法では、省略可能なパラメーターを持つ宣言をインポートできますが、既定値はありませんが `Optional` キーワードを使用して表すことはできません。

* 省略可能なパラメーターに属性 `Microsoft.VisualBasic.CompilerServices.OptionCompareAttribute`があり、数値リテラル1または0がパラメーターの型に変換されている場合、コンパイラは、`Option Compare Text` が有効な場合はリテラル1、または `Optional Compare Binary` が有効な場合はリテラル0のいずれかを引数として使用します。

* 省略可能なパラメーターに属性 `System.Runtime.CompilerServices.IDispatchConstantAttribute`があり、型が `Object`で、既定値が指定されていない場合、コンパイラは `New System.Runtime.InteropServices.DispatchWrapper(Nothing)`引数を使用します。

* 省略可能なパラメーターに属性 `System.Runtime.CompilerServices.IUnknownConstantAttribute`があり、型が `Object`で、既定値が指定されていない場合、コンパイラは `New System.Runtime.InteropServices.UnknownWrapper(Nothing)`引数を使用します。

* 省略可能なパラメーターの型が `Object`で、既定値が指定されていない場合、コンパイラは引数 `System.Reflection.Missing.Value`を使用します。

### <a name="conditional-methods"></a>条件付きメソッド

呼び出し式が参照するターゲットメソッドがインターフェイスのメンバーではないサブルーチンであり、メソッドに1つ以上の `System.Diagnostics.ConditionalAttribute` 属性がある場合、式の評価は、ソースファイル内のそのポイントで定義されている条件付きコンパイル定数に依存します。 属性の各インスタンスは、条件付きコンパイル定数に名前を指定する文字列を指定します。 条件付きコンパイル定数は、条件付きコンパイルステートメントに含まれているかのように評価されます。 定数が `True`に評価される場合、式は実行時に正常に評価されます。 定数が `False`に評価される場合、式はまったく評価されません。

属性を検索すると、オーバーライド可能なメソッドの最も派生した宣言がチェックされます。

__付箋.__ 属性は関数またはインターフェイスメソッドでは無効であり、どちらの種類のメソッドでも指定されている場合は無視されます。 したがって、条件付きメソッドは、呼び出しステートメントでのみ表示されます。

### <a name="type-argument-inference"></a>型引数の推論

型引数を指定せずに型パラメーターを持つメソッドが呼び出された場合、型引数の*推定*を使用して、呼び出しの型引数を試行して推論します。 これにより、型引数を普通に推論できる場合に、型パラメーターを使用してメソッドを呼び出すために、より自然な構文を使用できます。 たとえば、次のメソッド宣言があるとします。

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

型引数を明示的に指定しなくても、`Choose` メソッドを呼び出すことができます。

```vb
' calls Choose(Of Integer)
Dim i As Integer = Util.Choose(True, 5, 213)
' calls Choose(Of String)
Dim s As String = Util.Choose(False, "a", "b") 
```

型引数の推定により、型引数 `Integer` と `String` は、メソッドの引数から決定されます。

型引数の推定は、ラムダメソッドまたは引数リスト内のメソッドポインターに対して式の再分類が実行される*前に*発生します。これら2種類の式を再分類するには、パラメーターの型がわかっている必要があるためです。  一連の引数 `A1,...,An`、一連の一致するパラメーター `P1,...,Pn` および一連のメソッドの型 `T1,...,Tn`パラメーターを指定すると、引数とメソッドの型パラメーターの間の依存関係は、次のように最初に収集されます。

* `An` が `Nothing` リテラルである場合、依存関係は生成されません。

* `An` がラムダメソッドであり、`Pn` の型が構築されたデリゲート型または `System.Linq.Expressions.Expression(Of T)`である場合、`T` は構築されたデリゲート型です。

* ラムダメソッドのパラメーターの型が、対応するパラメーター `Pn`の型から推論され、パラメーターの型がメソッド型パラメーター `Tn`に依存している場合、`An` は `Tn`に依存します。

* ラムダメソッドパラメーターの型が指定されていて、対応するパラメーター `Pn` の型がメソッド型パラメーター `Tn`に依存している場合、`Tn` は `An`に依存しています。

* `Pn` の戻り値の型がメソッドの型パラメーター `Tn`に依存している場合、`Tn` は `An`に依存しています。

* `An` がメソッドポインターで、`Pn` の型が構築されたデリゲート型である場合、

* `Pn` の戻り値の型がメソッドの型パラメーター `Tn`に依存している場合、`Tn` は `An`に依存しています。

* `Pn` が構築された型で `Pn` の型がメソッド型パラメーター `Tn`に依存している場合、`Tn` は `An`に依存しています。

* それ以外の場合、依存関係は生成されません。

依存関係を収集した後、依存関係のない引数はすべて削除されます。 メソッド型パラメーターに送信依存関係がない場合 (つまり、メソッド型パラメーターが引数に依存していない場合)、型の推定は失敗します。 それ以外の場合、残りの引数とメソッドの型パラメーターは、厳密に接続されたコンポーネントにグループ化されます。 厳密に接続されたコンポーネントは、引数とメソッド型パラメーターのセットであり、コンポーネント内の要素は、他の要素の依存関係を介して到達できます。

その後、厳密に接続されたコンポーネントは、位相的に並べ替えられ、次の順序で処理されます。

* 厳密に型指定されたコンポーネントに含まれる要素が1つだけの場合、

  * 要素が既に完了としてマークされている場合は、スキップします。

  * 要素が引数の場合は、引数の型ヒントを、それに依存するメソッド型パラメーターに追加し、要素を完了としてマークします。 引数が、推論された型を必要とするパラメーターを持つラムダメソッドである場合は、そのパラメーターの型に対して `Object` を推論します。

  * 要素がメソッド型パラメーターの場合、引数の型ヒントの中で最も優先度の高い型になるようにメソッド型パラメーターを推論し、要素を完了としてマークします。 型ヒントに配列要素の制限がある場合、指定された型の配列間で有効な変換だけが考慮されます (つまり、共変および組み込み配列の変換)。 型ヒントにジェネリック引数の制限がある場合、id 変換だけが考慮されます。 優先される型を選択できない場合、推定は失敗します。 ラムダメソッドの引数の型がこのメソッドの型パラメーターに依存している場合、型はラムダメソッドに反映されます。

* 厳密に型指定されたコンポーネントに複数の要素が含まれている場合、コンポーネントには循環参照が含まれます。

  * コンポーネント内の要素であるメソッド型パラメーターごとに、メソッドの型パラメーターが、完了とマークされていない引数に依存している場合は、その依存関係を、推論プロセスの終了時にチェックされるアサーションに変換します。

  * 厳密に型指定されたコンポーネントが決定された時点で、推論プロセスを再起動します。

すべてのメソッドの型パラメーターに対して型の推定が成功すると、アサーションに変更された依存関係がすべてチェックされます。 引数の型がメソッド型パラメーターの推論された型に暗黙的に変換できる場合、アサーションは成功します。 アサーションが失敗した場合、型引数の推定は失敗します。

引数 `A` に対して `Ta` 引数の型とパラメーターの型 `P``Tp` を指定すると、型ヒントは次のように生成されます。

* `Tp` にメソッドの型パラメーターが含まれていない場合、ヒントは生成されません。

* `Tp` と `Ta` が同じランクの配列型である場合、`Ta` と `Tp` を `Ta` および `Tp` の要素の型に置き換え、配列要素の制限でこのプロセスを再起動します。

* `Tp` がメソッド型パラメーターの場合、`Ta` は、現在の制限を持つ型ヒントとして追加されます (存在する場合)。

* `A` がラムダメソッドで、`Tp` が構築されたデリゲート型または `System.Linq.Expressions.Expression(Of T)`である場合、`T` は構築されたデリゲート型であり、は構築されたデリゲート型であり、`TL` を `TD`に置き換え、`Ta` を使用して `TL` を変更し、制限なしでプロセスを再起動します。`Tp``TD` 次に、`Ta` をラムダメソッドの戻り値の型に置き換え、次のようにします。

  * `A` が通常のラムダメソッドの場合は、`Tp` をデリゲート型の戻り値の型に置き換えます。
  * `A` が非同期ラムダメソッドであり、デリゲート型の戻り値の型が一部の `T`のフォーム `Task(Of T)` を持っている場合は、`Tp` をその `T`に置き換えます。
  * `A` が反復子ラムダメソッドであり、デリゲート型の戻り値の型が一部の `T`に対してフォーム `IEnumerator(Of T)` または `IEnumerable(Of T)` の場合は、`Tp` をその `T`に置き換えます。
  * 次に、制限なしでプロセスを再起動します。

* `A` がメソッドポインターで `Tp` が構築されたデリゲート型である場合、`Tp` のパラメーターの型を使用して、どのメソッドが `Tp`に最も適しているかを判断します。 最も適切なメソッドがある場合は、`Ta` をメソッドの戻り値の型に置き換えて、デリゲート型の戻り値の型で `Tp` し、制限なしでプロセスを再開します。

* それ以外の場合、`Tp` は構築された型である必要があります。 指定された `TG`、`Tp`のジェネリック型です。

  * `Ta` が `TG`の場合、`TG`から継承されるか、または `TG` 型を厳密に1回実装します。次に、`Tax` の `Ta` と `Tpx` を `Tp`に置き換え、汎用引数の制限でプロセスを再起動します。`Ta``Tax``Tp``Tpx`

  * それ以外の場合、ジェネリックメソッドの型の推定は失敗します。

型の推定が成功しても、とでは、メソッドが適用可能であることが保証されます。
