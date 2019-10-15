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

3.  次に、1つ以上の引数が @no__t 0 またはラムダ式である場合は、次のように各引数の*デリゲート緩和レベル*を計算します。 @No__t-0 の最も低い (最低) デリゲート緩和 level が `M` の最小デリゲート緩和レベルよりも悪い場合は、セットから `N` を削除します。 Delegate 緩和レベルは次のとおりです。

    31. *緩和レベルを委任*するときにエラーが発生しました--`AddressOf` またはラムダをデリゲート型に変換できません。
  
    32. *戻り値の型またはパラメーターの縮小デリゲート緩和*--引数が `AddressOf`、または宣言された型のラムダで、戻り値の型からデリゲートの戻り値の型への変換が縮小されます。または、引数が標準のラムダで、その戻り式からデリゲートの戻り値の型への変換が縮小されている場合、または引数が非同期ラムダであり、デリゲートの戻り値の型が `Task(Of T)` で、その戻り式からの変換がの場合は、@no__ t-3 は縮小です。または、引数が反復子ラムダであり、デリゲートの戻り値の型が-4 または @no__t @no__t で、その yield オペランドから `T` への変換が縮小されている場合は。

    33. デリゲート*緩和をシグネチャなしでデリゲートするように拡張*しました--デリゲート型が `System.Delegate` または `System.MultiCastDelegate` または `System.Object`。

    34. *Drop return または arguments delegate 緩和*--引数が `AddressOf`、または宣言された戻り値の型を持つラムダで、デリゲート型に戻り値の型がありません。引数が1つ以上の return 式を持つラムダであり、デリゲート型に戻り値の型がない場合は。引数が `AddressOf` またはラムダのパラメーターがなく、デリゲート型にパラメーターがある場合は。

    35. *緩和戻り値の型の拡大デリゲートの*--引数が-1 または宣言 @no__t された戻り値の型を持つラムダで、戻り値の型からデリゲートの型への拡大変換がある場合は、または、引数が通常のラムダであり、すべての戻り式からデリゲートの戻り値の型への変換が、少なくとも1つの拡大を伴う、拡大または id である場合、または、引数が非同期ラムダであり、デリゲートが `Task(Of T)` または `Task` で、すべての戻り式から `T` @ no__t-5 に変換する場合は、それぞれ、少なくとも1つの拡大を含む拡大または id です。または、引数が反復子ラムダであり、デリゲートが `IEnumerator(Of T)` または `IEnumerable(Of T)` または `IEnumerator` または `AddressOf`0 であり、すべての戻り式から 1 @ no__t-12 @ no__t に変換する場合は、少なくとも1つの拡大があります。

    36. *Id デリゲート緩和*--引数が `AddressOf` またはデリゲートと完全に一致するラムダ (パラメーターの拡大/縮小または削除を使用しない)、またはを返すまたはを返します。次に、セットの一部のメンバーが、引数のいずれかに適用される縮小変換を必要としない場合は、を実行するすべてのメンバーを削除します。 以下に例を示します。

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

    41. Set の一部のインスタンスメンバーで、引数の式の型が 0 @no__t である縮小変換のみが必要な場合は、他のすべてのメンバーを削除します。
    42. @No__t-0 からの縮小のみを必要とするメンバーがセットに複数含まれている場合、呼び出し先の式は遅延バインディングされたメソッドアクセスとして再分類されます (メソッドグループを含む型がインターフェイスの場合、または、適用可能なメンバーは拡張メンバーでした)。
    43. 数値リテラルからの縮小のみを必要とする候補がある場合は、次の手順で残りのすべての候補の中から最も限定的なものを選択します。 勝者が数値リテラルのみを縮小する必要がある場合は、オーバーロードの解決の結果として選択されます。それ以外の場合はエラーになります。

    __付箋.__ この規則の根拠は、プログラムが弱い型指定の場合 (つまり、ほとんどまたはすべての変数が @no__t 0 として宣言されている場合)、`Object` からの多くの変換が縮小されると、オーバーロードの解決が困難になることがあります。 多くの状況でオーバーロードの解決が失敗する (メソッド呼び出しに引数を厳密に入力する必要がある) のではなく、呼び出しに適したオーバーロードされたメソッドが実行時まで遅延されることを解決します。 これにより、緩やかに型指定された呼び出しを、追加のキャストなしで成功させることができます。 ただし、このような副作用は残念ですが、遅延バインディングの呼び出しを実行するには、呼び出しターゲットを `Object` にキャストする必要があります。 構造体の値の場合、これは値が一時的なにボックス化される必要があることを意味します。 メソッドが最終的に構造体のフィールドを変更しようとした場合、メソッドが返されると、この変更は失われます。 遅延バインディングは常にランタイムクラスまたは構造体型のメンバーに対して解決されるため、インターフェイスはこの特別な規則から除外されます。これは、実装するインターフェイスのメンバーとは名前が異なる場合があります。

5.  次に、縮小が不要なインスタンスメソッドがセットに残っている場合は、セットからすべての拡張メソッドを削除します。 以下に例を示します。

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

6.  次に、セットの2つのメンバー `M` と `N` が指定されている場合は、引数リストで指定された `N` よりも `M` がより*具体的*に ([引数リストを指定したメンバー/型の特定の](overload-resolution.md#specificity-of-memberstypes-given-an-argument-list)部分)、set から @no__t 6 が削除されます。 セットに複数のメンバーが残っており、その他のメンバーが引数リストに等しくない場合は、コンパイル時エラーが発生します。

7.  それ以外の場合は、セットの2つのメンバー (`M` と `N`) を指定した場合、次の規則が適用されます。

    71. @No__t-0 に ParamArray パラメーターが指定されておらず `N` がの場合、または両方の操作 @no__t で ParamArray パラメーターに渡される引数が `N` の場合よりも少なくなる場合は、set から @no__t 4 を削除します。 以下に例を示します。

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

    72. @No__t-0 が `N` よりも多くの派生型で定義されている場合は、セットから `N` を削除します。 以下に例を示します。

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

        この規則は、拡張メソッドが定義されている型にも適用されます。 以下に例を示します。

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

    73. @No__t-0 および `N` が拡張メソッドであり、`M` のターゲット型がクラスまたは構造体であり、`N` のターゲット型がインターフェイスである場合は、セットから @no__t 4 を削除します。 以下に例を示します。

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

    74. @No__t-0 および `N` が拡張メソッドであり、`M` と `N` のターゲット型が型パラメーターの置換後に同一であり、型パラメーターの置換の前に @no__t のターゲット型が型パラメーターを含まないが、ターゲットの型がである場合`N` は、@no__t の対象の型よりも少数の型パラメーターを持つため、セットから `N` を削除します。 以下に例を示します。

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

    75. 型引数が置換される前に、`M` が `N` よりも*少ないジェネリック*(セクション[Genericity](overload-resolution.md#genericity)) の場合は、セットから `N` を削除します。

    76. @No__t-0 が拡張メソッドではなく、`N` の場合は、セットから `N` を削除します。

    77. @No__t-0 および `N` が拡張メソッドであり、`N` (セクション[拡張メソッドのコレクション](expressions.md#extension-method-collection)) の前に `M` が見つかった場合は、セットから @no__t を削除します。 以下に例を示します。

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

        拡張メソッドが同じ手順で見つかった場合、これらの拡張メソッドはあいまいです。 この呼び出しは、拡張メソッドを含む標準モジュールの名前を使用し、通常のメンバーであるかのように拡張メソッドを呼び出すことで、常に明確することができます。 以下に例を示します。

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

    78. @No__t-0 および `N` が型引数を生成するために必要な型の推論の両方である場合、`M` では、型引数のいずれかに対して最も優先される型を決定する必要 @no__t @no__t がありませんでした (つまり、1つの型に対して推論される型引数)。を設定します。

        __付箋.__ この規則により、以前のバージョンで成功したオーバーロードの解決 (型引数に対して複数の型を推論するとエラーが発生する) が確実になり、同じ結果が生成されます。

    79. @No__t-0 式からデリゲート作成式のターゲットを解決するためにオーバーロードの解決が行われていて、デリゲートと `M` の両方が関数である場合、`N` はサブルーチンであるため、セットから `N` を削除します。 同様に、デリゲートと @no__t 0 の両方がサブルーチンで、`N` が関数の場合は、セットから `N` を削除します。

    710. @No__t-0 で、明示的な引数の代わりに省略可能なパラメーターの既定値が使用されなかったが、`N` だった場合は、セットから `N` を削除します。

    711. 型引数が置換される前に、`M` が `N` よりも*深さが多い genericity* (セクション[genericity](overload-resolution.md#genericity)) である場合は、セットから `N` を削除します。

8. それ以外の場合、呼び出しはあいまいになり、コンパイル時エラーが発生します。

#### <a name="specificity-of-memberstypes-given-an-argument-list"></a>引数リストを指定した場合のメンバー/型の特異性

メンバー `M` は、引数リスト `A` が指定されている場合は、@no__t と*同じように扱われ*ます。これらのシグネチャが同じである場合、または `M` の各パラメーターの型が `N` の対応するパラメーターの型と同じである場合。

__付箋.__ 2つのメンバーは、拡張メソッドによって同じシグネチャを持つメソッドグループに終了できます。 2つのメンバーを同じように指定することもできますが、型パラメーターまたは paramarray 拡張のために同じシグネチャを持つことはできません。

メンバー `M` は、署名が異なる場合には @no__t よりも*具体的*であると見なされます。 @no__t 3 のパラメーターの型が少なくとも1つは、`N` のパラメーターの型よりも限定的であり、`N` のパラメーターの型がパラメーターよりも固有ではありません`M` を入力します。 次のいずれかの条件に該当する場合、-0 と `Nj` の2つの @no__t 引数に一致するパラメーターのペアが指定されている場合は、-3 @no__t の型は、`Nj` の型よりも*具体的*な値と見なされます (@no__t)。

* @No__t-0 の型から型 `Nj` への拡大変換が存在します。 (__注:__ この場合、パラメーターの型は、実際の引数に関係なく比較されるため、定数式から数値型への拡大変換は、この場合は考慮されません。

* `Aj` はリテラル `0`、`Mj` は数値型、`Nj` は列挙型です。 (__注:__ この規則は、リテラル `0` が任意の列挙型に拡大変換されるために必要です。 列挙型は基になる型に拡大変換されるため、`0` のオーバーロードの解決では、既定で数値型に対する列挙型が優先されます。 この動作が反するていたというフィードバックが多数寄せられています)。

* `Mj` と `Nj` は両方とも数値型です。 @no__t-@no__t 2 は、リスト @no__t 4、`SByte`、@no__t 6、`UShort`、`Integer`、`UInteger`、0、1、2、3、4 のいずれかになります。 (__注:__ 数値型に関する規則は便利です。これは、特定のサイズの符号付き数値型と符号なし数値型の間で縮小変換が行われるためです。 上記の規則は、2つの型の間の相互関係を分割し、より "自然な" 数値型を優先します。 これは、特定のサイズの符号付きおよび符号なしの数値型の両方に拡大変換される型 (両方に収まる数値リテラルなど) に対してオーバーロードの解決を実行する場合に特に重要です。

* `Mj` および `Nj` はデリゲート関数型であり、`Mj` の戻り値の型は、ラムダメソッドとして分類されている場合は `Nj` の戻り値の型よりも具体的で、`Mj` または `Nj` が @no__t の場合は、型の型引数 (比較する型の代わりに、デリゲート型) が使用されます。

* `Mj` は `Aj` の型と同じであり、`Nj` はと同じではありません。 (__注:__ 前の規則はとは若干異なることに注意しC#てください。 C#では、戻り値の型を比較する前に、デリゲート関数型のパラメーターリストが同一である必要がありますが、Visual Basic はそうではありません)。

#### <a name="genericity"></a>Genericity

メンバー `M` は、次のように、メンバー `N` より*汎用的*ではないと判断されます。

1. 一致するパラメーターの各ペアについて、`Mj` および `Nj` の場合、`Mj` は、メソッドの型パラメーターに関しては `Nj` よりも少なく、または等しくありません。また、メソッドの型パラメーターに関して、少なくとも1つの `Mj` がジェネリックになりません。
2. それ以外の場合、対応するパラメーターの各ペアに対して-0 および `Nj` の @no__t、`Mj` は、型の型パラメーターに関しては `Nj` よりも少なく、または同等であり、型の型パラメーターに関して少なくとも1つの `Mj` はジェネリックではありませんでは、`M` は `N` よりも一般的ではありません。

パラメーター `M` は、パラメーターに対して同じように汎用的と見なされます。 @no__t パラメーターの型 `Mt` と @no__t の両方が型パラメーターを参照しているか、両方とも型パラメーターを参照していない場合は-1 です。 `M` は、`Mt` が型パラメーターを参照しておらず、`Nt` である場合に `N` よりも汎用的ではないと見なされます。

以下に例を示します。

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

カリー化中に修正された拡張メソッドの型パラメーターは、メソッドの型パラメーターではなく、型の型パラメーターと見なされます。 以下に例を示します。

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

@No__t-3 と `Nj` の一致するパラメーターの各ペアに対して、`Mj`*の genericity の深さ*が @no__t の値以上で、少なくとも1つの @no__t が指定されている場合、メンバーの `M` は、@no__t メンバーよりも*詳細な genericity*を持つことになります。の深さは genericity です。 Genericity の深さは次のように定義されています。

* 型パラメーター以外のものは、型パラメーターよりも genericity の深さが多くなります。

* 少なくとも1つの型引数の深さが genericity で、型引数の深さが対応する型よりも少ない場合、構築された型は再帰的に (型引数の数が同じである) 他の構築された型と比べて genericity の深さが大きくなります。もう一方の引数。

* 配列型は、1番目の要素の型が2番目の要素の型よりも genericity の深さを持つ場合、(次元数が同じ) 別の配列型よりも genericity の深さが大きくなります。

以下に例を示します。

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
6. 型引数がメソッドの制約に違反している場合 (手順3の推定型引数を含む)、メソッドは適用されません。 以下に例を示します。

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

単一の引数式が paramarray パラメーターと一致し、引数式の型が paramarray パラメーターの型と paramarray 要素の型の両方に変換可能な場合、メソッドは展開されているフォームと展開されていない形式の両方に適用できます。2つの例外があります。 引数式の型から paramarray 型への変換が縮小されている場合、メソッドは拡張された形式でのみ適用されます。 引数式がリテラル `Nothing` の場合、メソッドは、その展開されていない形式でのみ適用できます。 以下に例を示します。

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

@No__t-0 の最初と最後の呼び出しでは、引数の型からパラメーターの型 (両方とも `Object()`) への拡大変換が存在し、引数が通常の値のパラメーターとして渡されるため、`F` の通常の形式が適用されます。 2番目と3番目の呼び出しでは、引数の型からパラメーターの型への拡大変換が存在しないため、通常の形式の `F` は適用されません (`Object` から `Object()` への変換は縮小)。 ただし、`F` の展開された形式が適用可能であり、1つの要素 @no__t 1 が呼び出しによって作成されます。 配列の1つの要素が、指定された引数の値で初期化されています (それ自体が `Object()` への参照)。

### <a name="passing-arguments-and-picking-arguments-for-optional-parameters"></a>渡す (引数を)、省略可能なパラメーターの引数を選択する

パラメーターが値パラメーターの場合は、一致する引数式を値として分類する必要があります。 値は、パラメーターの型に変換され、実行時にパラメーターとして渡されます。 パラメーターが参照パラメーターであり、一致する引数式が、パラメーターと同じ型を持つ変数として分類されている場合は、変数への参照が実行時にパラメーターとして渡されます。

それ以外の場合、一致する引数式が変数、値、またはプロパティアクセスとして分類されると、パラメーターの型の一時変数が割り当てられます。 実行時にメソッドが呼び出される前に、引数式が値として再分類され、パラメーターの型に変換され、一時変数に割り当てられます。 次に、一時変数への参照がパラメーターとして渡されます。 メソッドの呼び出しが評価された後、引数式が変数またはプロパティアクセスとして分類されている場合、一時変数は変数式またはプロパティアクセス式に割り当てられます。 プロパティアクセス式に @no__t 0 のアクセサーがない場合、割り当ては実行されません。

引数が指定されていない省略可能なパラメーターの場合、コンパイラは、次に示すように引数を選択します。 いずれの場合も、ジェネリック型の置換後にパラメーター型に対してテストされます。

* 省略可能なパラメーターに `System.Runtime.CompilerServices.CallerLineNumber` という属性があり、呼び出しがソースコード内の場所からのものであり、その位置の行番号を表す数値リテラルにパラメーター型への組み込みの変換がある場合は、数値リテラルが使用されます。 呼び出しが複数行にまたがっている場合、使用する行の選択は実装に依存します。

* 省略可能なパラメーターに `System.Runtime.CompilerServices.CallerFilePath` という属性があり、呼び出しがソースコード内の場所からのものであり、その場所のファイルパスを表す文字列リテラルにパラメーター型への組み込みの変換がある場合は、文字列リテラルが使用されます。 ファイルパスの形式は実装に依存します。

* 省略可能なパラメーターの属性が 0 @no__t で、呼び出しが型のメンバーの本体、またはその型のメンバーの任意の部分に適用されている属性に含まれていて、そのメンバー名を表す文字列リテラルがパラメーター型への組み込みの変換を持っている場合。では、文字列リテラルが使用されます。 プロパティアクセサーまたはカスタムイベントハンドラーの一部である呼び出しの場合、使用されるメンバー名は、プロパティまたはイベント自体の名前になります。 演算子またはコンストラクターの一部である呼び出しの場合は、実装固有の名前が使用されます。

上記のいずれも当てはまらない場合は、省略可能なパラメーターの既定値が使用されます (または、既定値が指定されていない場合は `Nothing`)。 また、上記のいずれかが当てはまる場合は、実装に依存するオプションが使用されます。

@No__t-0 および `CallerFilePath` 属性は、ログ記録に便利です。 @No__t-0 は、`INotifyPropertyChanged` を実装する場合に便利です。 次に例を示します。

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

* メタデータからインポートした場合は、パラメーターが省略可能であることを示すためにパラメーター `<Optional>` を Visual Basic も処理します。この方法では、省略可能なパラメーターを持つ宣言をインポートできますが、既定値は指定できません (ただし、これは表現できません)。`Optional` キーワードを使用します。

* 省略可能なパラメーターに `Microsoft.VisualBasic.CompilerServices.OptionCompareAttribute` という属性があり、数値リテラル1または0がパラメーター型に変換されている場合、コンパイラは、`Option Compare Text` の場合はリテラル1、有効になっている @no__t 場合はリテラル0のいずれかを引数として使用します。

* 省略可能なパラメーターに `System.Runtime.CompilerServices.IDispatchConstantAttribute` という属性があり、type が `Object` で、既定値が指定されていない場合、コンパイラは引数 `New System.Runtime.InteropServices.DispatchWrapper(Nothing)` を使用します。

* 省略可能なパラメーターに `System.Runtime.CompilerServices.IUnknownConstantAttribute` という属性があり、type が `Object` で、既定値が指定されていない場合、コンパイラは引数 `New System.Runtime.InteropServices.UnknownWrapper(Nothing)` を使用します。

* 省略可能なパラメーターの型が 0 @no__t で、既定値が指定されていない場合、コンパイラは引数 `System.Reflection.Missing.Value` を使用します。

### <a name="conditional-methods"></a>条件付きメソッド

呼び出し式が参照するターゲットメソッドがインターフェイスのメンバーではないサブルーチンであり、メソッドに1つ以上の @no__t 0 属性がある場合、式の評価はそのポイントで定義されている条件付きコンパイル定数に依存しますソースファイル内。 属性の各インスタンスは、条件付きコンパイル定数に名前を指定する文字列を指定します。 条件付きコンパイル定数は、条件付きコンパイルステートメントに含まれているかのように評価されます。 定数が `True` に評価される場合、式は実行時に正常に評価されます。 定数が `False` と評価された場合、式はまったく評価されません。

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

型引数の推定により、型引数 `Integer` および `String` は、メソッドの引数から決定されます。

型引数の推定は、ラムダメソッドまたは引数リスト内のメソッドポインターに対して式の再分類が実行される*前に*発生します。これら2種類の式を再分類するには、パラメーターの型がわかっている必要があるためです。  一連の引数 `A1,...,An`、一致するパラメーターのセット `P1,...,Pn`、メソッドの型 @no__t パラメーターのセット (-2) を指定すると、引数とメソッドの型パラメーターの間の依存関係は、次のように最初に収集されます。

* @No__t-0 が `Nothing` のリテラルである場合、依存関係は生成されません。

* @No__t-0 がラムダメソッドで、@no__t の型が構築されたデリゲート型または `System.Linq.Expressions.Expression(Of T)` の場合、`T` は構築されたデリゲート型です。

* ラムダメソッドのパラメーターの型が対応する @no__t パラメーターの型から推論され、パラメーターの型がメソッド型パラメーター `Tn` に依存している場合は、`An` が `Tn` に依存しています。

* ラムダメソッドのパラメーターの型が指定されていて、対応するパラメーターの型 `Pn` がメソッドの型パラメーター `Tn` の場合、`Tn` は `An` に依存しています。

* @No__t-0 の戻り値の型が、メソッドの型パラメーター `Tn` に依存している場合、`Tn` は `An` に依存しています。

* @No__t-0 がメソッドポインターで、@no__t の型が構築されたデリゲート型である場合は、

* @No__t-0 の戻り値の型が、メソッドの型パラメーター `Tn` に依存している場合、`Tn` は `An` に依存しています。

* @No__t-0 が構築された型で、@no__t の型がメソッドの型パラメーター `Tn` に依存している場合、`Tn` は @no__t に依存しています。

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

引数の型が-1 の引数 `A` で、パラメーターの型 `Tp` の場合、パラメーター `P` の @no__t、型ヒントは次のように生成されます。

* @No__t-0 にメソッドの型パラメーターが含まれていない場合、ヒントは生成されません。

* @No__t-0 および `Ta` が同じランクの配列型である場合は、`Ta` と `Tp` を `Ta` および `Tp` の要素の型に置き換え、配列要素の制限でこのプロセスを再起動します。

* @No__t-0 がメソッド型パラメーターの場合、`Ta` は、現在の制限を持つ型ヒントとして追加されます (存在する場合)。

* @No__t-0 がラムダメソッドで、`Tp` が構築されたデリゲート型または `System.Linq.Expressions.Expression(Of T)` である場合 (`T` は構築されたデリゲート型)、各ラムダメソッドのパラメーターの型である-4 および対応するデリゲートパラメーターの型 `TD` は、`Ta` を @no__t に置き換えます。`TD` を使用して `Tp` を実行し、制限なくプロセスを再起動します。 次に、`Ta` を、ラムダメソッドの戻り値の型とに置き換えます。

  * `A` が通常のラムダメソッドの場合は、`Tp` をデリゲート型の戻り値の型に置き換えます。
  * `A` が非同期ラムダメソッドであり、デリゲート型の戻り値の型が `T` の `Task(Of T)` の場合は、`Tp` をその @no__t に置き換えます。
  * `A` が反復子ラムダメソッドで @no__t あり、デリゲート型の戻り値の型の形式が `IEnumerator(Of T)` または `IEnumerable(Of T)` である場合は、@no__t を `T` に置き換えます。
  * 次に、制限なしでプロセスを再起動します。

* @No__t-0 がメソッドポインターで、`Tp` が構築されたデリゲート型である場合は、`Tp` のパラメーターの型を使用して、どのメソッドが `Tp` に最も適しているかを判断します。 最も適切なメソッドがある場合は、`Ta` をメソッドの戻り値の型に置き換え、`Tp` をデリゲート型の戻り値の型に置き換えて、制限なしでプロセスを再開します。

* それ以外の場合、`Tp` は構築された型である必要があります。 @No__t-0、@no__t のジェネリック型-1、

  * @No__t-0 が `TG` の場合、@no__t から継承された場合、または @no__t 型を1回だけ実装している場合は、対応する型引数ごとに `Ta` と `Tpx` の @no__t を @no__t に置き換えてから @no__t を再起動します。汎用引数の制限で処理します。

  * それ以外の場合、ジェネリックメソッドの型の推定は失敗します。

型の推定が成功しても、とでは、メソッドが適用可能であることが保証されます。
