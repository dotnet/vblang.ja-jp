---
ms.openlocfilehash: 8599ffc0ad3313f4e7e42da220c22ffa7a37e56b
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306182"
---
# <a name="general-concepts"></a>一般的な概念

この章では、Microsoft Visual Basic 言語のセマンティクスを理解するために必要ないくつかの概念について説明します。 概念の多くは、プログラマーや C/C++プログラマーの Visual Basic を理解している必要がありますが、正確な定義は異なる場合があります。

## <a name="declarations"></a>宣言

Visual Basic プログラムは、名前付きエンティティで構成されます。 これらのエンティティは、*宣言*によって導入され、プログラムの "意味" を表します。

最上位レベルでは、*名前空間*は、入れ子になった名前空間や型など、他のエンティティを編成するエンティティです。 *型*は、値を記述し、実行可能コードを定義するエンティティです。 型には、入れ子にされた型と型のメンバーを含めることができます。 *型のメンバー*は、定数、変数、メソッド、演算子、プロパティ、イベント、列挙値、およびコンストラクターです。

他のエンティティを含むことができるエンティティは、*宣言空間*を定義します。 エンティティは、宣言または継承によって宣言領域に導入されます。含まれている宣言領域は、エンティティの*宣言コンテキスト*と呼ばれます。 宣言空間でエンティティを宣言すると、さらに入れ子になったエンティティ宣言を含めることができる新しい宣言空間が定義されます。したがって、プログラム内の宣言は、宣言スペースの階層を形成します。

オーバーロードされた型のメンバーの場合を除き、同じ種類の同じ名前を持つエンティティを同じ宣言コンテキストに導入する宣言では無効です。 また、宣言空間には、同じ名前を持つ異なる種類のエンティティを含めることはできません。たとえば、宣言空間には、変数とメソッドを同じ名前で含めることはできません。

__付箋.__ 他の言語では、同じ名前を持つ異なる種類のエンティティを含む宣言空間を作成することができます (たとえば、言語で大文字と小文字が区別され、大文字と小文字に基づく宣言が異なる場合など)。 そのような場合、最もアクセスしやすいエンティティは、その名前にバインドされていると見なされます。複数の種類のエンティティが最もアクセス可能な場合は、名前があいまいになります。 `Public` は `Protected Friend` よりもアクセスが可能です。 `Protected Friend` の方が、`Protected` または @no__t よりもアクセスしやすく、`Protected` または `Friend` よりも @no__t にアクセスできるようになりました。

名前空間の宣言空間は "open 終了" です。そのため、同じ完全修飾名を持つ2つの名前空間宣言は、同じ宣言領域に関与します。 次の例では、2つの名前空間宣言が同じ宣言領域に関与しています。この場合、完全修飾名を持つ2つのクラスを宣言する `Data.Customer` と `Data.Order:` です。

```vb
Namespace Data
    Class Customer
    End Class
End Namespace

Namespace Data
    Class Order
    End Class
End Namespace
```

2つの宣言は同じ宣言空間に関与するため、それぞれに同じ名前のクラスの宣言が含まれていると、コンパイル時エラーが発生します。

### <a name="overloading-and-signatures"></a>オーバーロードとシグネチャ

宣言空間で同じ種類の同じ名前を持つエンティティを宣言する唯一の方法は、*オーバーロード*を使用することです。 メソッド、演算子、インスタンスコンストラクター、およびプロパティのみがオーバーロードされる可能性があります。

オーバーロードされた型のメンバーは、一意のシグネチャを持つ必要があります。 型のメンバーのシグネチャは、型パラメーターの数と、メンバーのパラメーターの数と型で構成されます。 変換演算子には、シグネチャ内の演算子の戻り値の型も含まれます。

次のはメンバーのシグネチャの一部ではないため、でオーバーロードすることはできません。

* 型のメンバーに対する修飾子 (たとえば、`Shared` または `Private`)。

* パラメーターに修飾子を指定します (たとえば、`ByVal` または `ByRef`)。

* パラメーターの名前。

* メソッドまたは演算子の戻り値の型 (変換演算子を除く)、またはプロパティの要素型。

* 型パラメーターに対する制約。

次の例は、オーバーロードされたメソッド宣言のセットとそのシグネチャを示しています。 いくつかのメソッド宣言のシグネチャが同じであるため、この宣言は有効ではありません。

```vb
Interface ITest
    Sub F1()                              ' Signature is ().
    Sub F2(x As Integer)                  ' Signature is (Integer).
    Sub F3(ByRef x As Integer)            ' Signature is (Integer).
    Sub F4(x As Integer, y As Integer)    ' Signature is (Integer, Integer).
    Function F5(s As String) As Integer   ' Signature is (String).
    Function F6(x As Integer) As Integer  ' Signature is (Integer).
    Sub F7(a() As String)                 ' Signature is (String()).
    Sub F8(ParamArray a() As String)      ' Signature is (String()).
    Sub F9(Of T)()                        ' Signature is !1().
    Sub F10(Of T, U)(x As T, y As U)      ' Signature is !2(!1, !2)
    Sub F11(Of U, T)(x As T, y As U)      ' Signature is !2(!2, !1)
    Sub F12(Of T)(x As T)                 ' Signature is !1(!1)
    Sub F13(Of T As IDisposable)(x As T)  ' Signature is !1(!1)
End Interface
```

指定された型引数に基づいて、同じシグネチャを持つメンバーを含む可能性のあるジェネリック型を定義することは有効です。 オーバーロードの解決規則は、このようなオーバーロードを試すために使用されますが、あいまいさを解消できない場合もあります。 以下に例を示します。

```vb
Class C(Of T)
    Sub F(x As Integer)
    End Sub

    Sub F(x As T)
    End Sub

    Sub G(Of U)(x As T, y As U)
    End Sub

    Sub G(Of U)(x As U, y As T)
    End Sub
End Class

Module Test
    Sub Main()
        Dim x As New C(Of Integer)
        x.F(10)                   ' Calls C(Of T).F(Integer)
        x.G(Of Integer)(10, 10)    ' Error: Can't choose between overloads
    End Sub
End Module
```

## <a name="scope"></a>スコープ

エンティティの名前の*スコープ*は、その名前を修飾なしで参照できるすべての宣言空間のセットです。 一般に、エンティティの名前のスコープは、宣言コンテキスト全体です。ただし、エンティティの宣言には、同じ名前のエンティティの入れ子になった宣言が含まれている場合があります。 その場合、入れ子になったエンティティは、外側のエンティティを*シャドウ*したり非表示にしたりできます。また、シャドウされたエンティティへのアクセスは、限定によってのみ可能です。

入れ子によるシャドウ処理は、名前空間、または他の型で入れ子にされた型、および型メンバーの本体で入れ子になっている名前空間または型に発生します。 宣言の入れ子によるシャドウは、常に暗黙的に発生します。明示的な構文は必要ありません。

次の例では、`F` メソッド内で、インスタンス変数 `i` がローカル変数 `i` にシャドウされていますが、`G` メソッド内では、@no__t は引き続きインスタンス変数を参照しています。

```vb
Class Test
    Private i As Integer = 0

    Sub F()
        Dim i As Integer = 1
    End Sub

    Sub G()
        i = 1
    End Sub
End Class
```

内側のスコープ内の名前が外側のスコープ内の名前を非表示にすると、その名前のすべてのオーバーロードされた出現がシャドウされます。 次の @no__t 例では、`Inner` で宣言された `F(1)` が呼び出されます。これは、`F` のすべての外部オカレンスが内部宣言によって非表示になるためです。 同じ理由から、-0 @no__t の呼び出しはエラーになります。

```vb
Class Outer
    Shared Sub F(i As Integer)
    End Sub

    Shared Sub F(s As String)
    End Sub

    Class Inner
        Shared Sub F(l As Long)
        End Sub

        Sub G()
            F(1) ' Invokes Outer.Inner.F.
            F("Hello") ' Error.
        End Sub
    End Class
End Class
```

## <a name="inheritance"></a>継承

継承関係とは、1つの型 (*派生*型) が別の型 (*基本*型) から派生したもので、派生型の宣言空間に、アクセス可能な非コンストラクター型メンバーと入れ子にされた型が暗黙的に含まれるようにするためです。その基本型。 次の例では、クラス `A` は `B` の基本クラスであり、`B` は `A` から派生しています。

```vb
Class A
End Class

Class B
    Inherits A
End Class
```

@No__t-0 では基底クラスが明示的に指定されていないため、基本クラスは暗黙的に `Object` になります。

継承の重要な側面は次のとおりです。

* 継承は推移的です。 型*c*が型*b*から派生し、型*b*が型*a*から派生している場合、型*c*は型*b*で宣言された型のメンバーと、型*a*で宣言された型のメンバーを継承します。

* 派生型は、その基本型を拡張しますが、限定することはできません。 派生型は新しい型のメンバーを追加でき、継承された型のメンバーをシャドウすることはできますが、継承された型のメンバーの定義を削除することはできません。

* 型のインスタンスには、その基本型のすべての型メンバーが含まれているため、常に派生型からその基本型への変換が存在します。

* 型 `Object` 以外のすべての型には基本型が必要です。 したがって、`Object` はすべての型の最終的な基本型であり、すべての型をこの型に変換できます。

* 派生での循環は許可されていません。 つまり、型 `B` が @no__t 型から派生している場合、型 `A` は、型 @no__t から直接または間接的に派生するエラーになります。

* 型は、その中に入れ子にされた型から直接的または間接的に派生することはできません。

次の例では、クラスが循環的に依存しているため、コンパイル時エラーが生成されます。

```vb
Class A
    Inherits B
End Class

Class B
    Inherits C
End Class

Class C
    Inherits A
End Class
```

次の例では、`B` が、クラス `A` を通じて入れ子になった @no__t クラスから間接的に派生しているため、コンパイル時エラーが発生します。

```vb
Class A
    Inherits B.C
End Class

Class B
    Inherits A

    Public Class C
    End Class 
End Class
```

次の例では、クラス `A` はクラス `B` から派生しないため、エラーは生成されません。

```vb
Class A
    Class B
        Inherits A
    End Class 
End Class
```

### <a name="mustinherit-and-notinheritable-classes"></a>MustInherit クラスと NotInheritable クラス

@No__t 0 クラスは、基本型としてのみ動作できる不完全な型です。 @No__t 0 クラスはインスタンス化できないため、1つに対して `New` 演算子を使用するとエラーになります。 @No__t-0 クラスの変数を宣言することは有効です。このような変数に割り当てることができるのは、-1 または @no__t クラスから派生したクラスの値 @no__t だけです。

通常のクラスが @no__t 0 クラスから派生した場合、通常のクラスは継承されたすべての `MustOverride` メンバーをオーバーライドする必要があります。 以下に例を示します。

```vb
MustInherit Class A
    Public MustOverride Sub F()
End Class

MustInherit Class B
    Inherits A

    Public Sub G()
    End Sub
End Class 

Class C
    Inherits B

    Public Overrides Sub F()
    End Sub 
End Class
```

@No__t-0 クラス `A` では、@no__t の2つの @no__t メソッドが導入されています。 クラス `B` には、追加のメソッド `G` が導入されていますが、`F` の実装は提供されていません。 クラス `B` も `MustInherit` と宣言されている必要があります。 クラス `C` は `F` をオーバーライドし、実際の実装を提供します。 クラス `C` の未処理の @no__t 0 メンバーは存在しないため、`MustInherit` である必要はありません。

@No__t 0 クラスは、別のクラスを派生させることができないクラスです。 @no__t 0 のクラスは、意図しない派生を防ぐために主に使用されます。

この例では、クラス `B` は、`NotInheritable` クラス `A` からの派生を試行するため、エラーになります。 クラスを `MustInherit` と `NotInheritable` の両方に設定することはできません。

```vb
NotInheritable Class A
End Class

Class B
    ' Error, a class cannot derive from a NotInheritable class.
    Inherits A
End Class
```

### <a name="interfaces-and-multiple-inheritance"></a>インターフェイスと多重継承

1つの基本型から派生した他の型とは異なり、インターフェイスは複数の基本インターフェイスから派生できます。 このため、インターフェイスは、異なる基本インターフェイスから同じ名前を持つ型メンバーを継承できます。 このような場合、派生インターフェイスでは、多重継承された名前を使用できません。また、派生インターフェイスを使用してこれらの型のメンバーを参照すると、シグネチャやオーバーロードに関係なくコンパイル時のエラーが発生します。 代わりに、競合する型メンバーは、基本インターフェイス名を使用して参照する必要があります。

次の例では、最初の2つのステートメントによってコンパイル時にエラーが発生します。これは、多重継承されたメンバー `Count` がインターフェイス `IListCounter` で使用できないためです。

```vb
Interface IList
    Property Count() As Integer
End Interface

Interface ICounter
    Sub Count(i As Integer)
End Interface

Interface IListCounter
    Inherits IList
    Inherits ICounter 
End Interface 

Module Test
    Sub F(x As IListCounter)
        x.Count(1)                  ' Error, Count is not available.
        x.Count = 1                 ' Error, Count is not available.
        CType(x, IList).Count = 1   ' Ok, invokes IList.Count.
        CType(x, ICounter).Count(1) ' Ok, invokes ICounter.Count.
    End Sub 
End Module
```

例に示すように、あいまいさを解決するには `x` を適切な基本インターフェイス型にキャストします。 このようなキャストには実行時のコストがありません。コンパイル時にインスタンスを弱い派生型として表示するだけで構成されます。

1つの型のメンバーが複数のパスを介して同じ基本インターフェイスから継承された場合、型のメンバーは1回だけ継承されているかのように扱われます。 つまり、派生インターフェイスには、特定の基本インターフェイスから継承された各型メンバーのインスタンスが1つだけ含まれます。 以下に例を示します。

```vb
Interface IBase
    Sub F(i As Integer)
End Interface

Interface ILeft
    Inherits IBase
End Interface

Interface IRight
    Inherits IBase
End Interface

Interface IDerived
    Inherits ILeft, IRight
End Interface

Class Derived
    Implements IDerived

    ' Only have to implement F once.
    Sub F(i As Integer) Implements IDerived.F
    End Sub
End Class
```

継承階層を介して1つのパスで型のメンバー名がシャドウされている場合、その名前はすべてのパスでシャドウされます。 次の例では、@no__t 0 のメンバーが `ILeft.F` メンバーによってシャドウされていますが、`IRight` ではシャドウされていません。

```vb
Interface IBase
    Sub F(i As Integer)
End Interface 

Interface ILeft
    Inherits IBase

    Shadows Sub F(i As Integer)
End Interface 

Interface IRight
    Inherits IBase

    Sub G()
End Interface 

Interface IDerived
    Inherits ILeft, IRight 
End Interface 

Class Test
    Sub H(d As IDerived)
        d.F(1)                  ' Invokes ILeft.F.
        CType(d, IBase).F(1)    ' Invokes IBase.F.
        CType(d, ILeft).F(1)    ' Invokes ILeft.F.
        CType(d, IRight).F(1)   ' Invokes IBase.F.
    End Sub 
End Class
```

@No__t-0 を選択すると、`IRight` をリードするアクセスパスで `IBase.F` がシャドウされていないように見えるにもかかわらず `ILeft.F` が選択されます。 @No__t-0 から `ILeft` までのアクセスパスは `IBase` のシャドウ `IBase.F` であるため、メンバーは、`IDerived` から `IRight` へのアクセスパスにもシャドウされます。

### <a name="shadowing"></a>シャドウ

派生型は、継承された型のメンバーの名前を、再宣言することによってシャドウします。 名前をシャドウすると、その名前を持つ継承された型のメンバーは削除されません。派生クラスでは、その名前を持つすべての継承された型メンバーを使用できなくなります。 シャドウ宣言には、任意の型のエンティティを指定できます。

オーバーロードできるエンティティは、2つの形式のシャドウのうちの1つを選択できます。 *名前によるシャドウ*処理は、`Shadows` キーワードを使用して指定されます。 名前で影を指定するエンティティは、すべてのオーバーロードを含め、基本クラスのその名前のすべてを非表示にします。 *名前と署名によるシャドウ*処理は、`Overloads` キーワードを使用して指定されます。 名前とシグネチャでシャドウするエンティティは、エンティティと同じシグネチャを持つその名前のすべてを非表示にします。 以下に例を示します。

```vb
Class Base
    Sub F()
    End Sub

    Sub F(i As Integer)
    End Sub

    Sub G()
    End Sub

    Sub G(i As Integer)
    End Sub
End Class

Class Derived
    Inherits Base

    ' Only hides F(Integer).
    Overloads Sub F(i As Integer)
    End Sub

    ' Hides G() and G(Integer).
    Shadows Sub G(i As Integer)
    End Sub
End Class

Module Test
    Sub Main()
        Dim x As New Derived()

        x.F() ' Calls Base.F().
        x.G() ' Error: Missing parameter.
    End Sub
End Module
```

名前とシグネチャによって @no__t 0 引数を使用してメソッドをシャドウすると、すべての拡張署名ではなく、個々の署名のみが隠蔽されます。 これは、シャドウするメソッドのシグネチャが、シャドウされたメソッドの展開されていないシグネチャと一致する場合にも当てはまります。 次のような例です。

```vb
Class Base
    Sub F(ParamArray x() As Integer)
        Console.WriteLine("Base")
    End Sub
End Class

Class Derived 
    Inherits Base

    Overloads Sub F(x() As Integer)
        Console.WriteLine("Derived")
    End Sub
End Class

Module Test
    Sub Main
        Dim d As New Derived()
        d.F(10)
    End Sub
End Module
```

`Derived.F` が展開されていない形式の `Base.F` と同じシグネチャを持つ場合でも、@no__t 0 を出力します。

逆に、@no__t 0 引数を持つメソッドは、同じシグネチャを持つメソッドをシャドウするだけで、展開される可能性のあるすべてのシグネチャではありません。 次のような例です。

```vb
Class Base
    Sub F(x As Integer)
        Console.WriteLine("Base")
    End Sub
End Class

Class Derived
    Inherits Base

    Overloads Sub F(ParamArray x() As Integer)
        Console.WriteLine("Derived")
    End Sub
End Class

Module Test
    Sub Main()
        Dim d As New Derived()
        d.F(10)
    End Sub
End Module
```

`Derived.F` に `Base.F` と同じシグネチャを持つ拡張フォームがある場合でも、@no__t 0 を出力します。

@No__t-0 または `Overloads` が指定されていないシャドウメソッドまたはプロパティは、メソッドまたはプロパティが `Overrides` で宣言されている場合は `Overloads` を、それ以外の場合は @no__t を想定します。 オーバーロードされたエンティティのセットの1つのメンバーが `Shadows` または `Overloads` のキーワードを指定している場合は、すべてを指定する必要があります。 @No__t-0 および `Overloads` のキーワードを同時に指定することはできません。 @No__t-0 も `Overloads` も、標準モジュールでは指定できません。標準モジュールのメンバーは、`Object` から継承されたメンバーを暗黙的にシャドウします。

インターフェイスの継承によって多重継承された型のメンバーの名前をシャドウすることができます (その結果、使用できなくなります)。したがって、派生インターフェイスで名前を使用できるようになります。

以下に例を示します。

```vb
Interface ILeft
    Sub F()
End Interface

Interface IRight
    Sub F()
End Interface

Interface ILeftRight
    Inherits ILeft, IRight

    Shadows Sub F()
End Interface

Module Test
    Sub G(i As ILeftRight)
        i.F() ' Calls ILeftRight.F.
        CType(i, ILeft).F() ' Calls ILeft.F.
        CType(i, IRight).F() ' Calls IRight.F.
    End Sub
End Module
```

メソッドは継承されたメソッドをシャドウすることが許可されるため、クラスには、同じシグネチャを持つ複数の @no__t 0 メソッドを含めることができます。 これは、最も派生したメソッドのみが表示されるため、あいまいさの問題はありません。 次の例では、`C` および `D` クラスに、同じシグネチャを持つ2つの @no__t 2 つのメソッドが含まれています。

```vb
Class A
    Public Overridable Sub F()
        Console.WriteLine("A.F")
    End Sub 
End Class 

Class B
    Inherits A

    Public Overrides Sub F()
        Console.WriteLine("B.F")
    End Sub 
End Class 

Class C
    Inherits B

    Public Shadows Overridable Sub F()
        Console.WriteLine("C.F")
    End Sub 
End Class 

Class D
    Inherits C

    Public Overrides Sub F()
        Console.WriteLine("D.F")
    End Sub 
End Class 

Module Test
    Sub Main()
        Dim d As New D()
        Dim a As A = d
        Dim b As B = d
        Dim c As C = d
        a.F()
        b.F()
        c.F()
        d.F()
    End Sub 
End Module
```

ここでは @no__t 0 のメソッドが2つあります。1つはクラス `A`、もう1つはクラス `C` で導入されています。 クラス `C` によって導入されたメソッドは、クラス @no__t から継承されたメソッドを非表示にします。 したがって、クラス @no__t の @no__t 0 宣言は、クラス `C` によって導入されたメソッドをオーバーライドします。クラス `D` では、クラス @no__t によって導入されたメソッドをオーバーライドすることはできません。 この例では、次の出力が生成されます。

```console
B.F
B.F
D.F
D.F
```

メソッドが非表示になっていない派生型を使用して、クラス `D` のインスタンスにアクセスすることにより、非表示の `Overridable` メソッドを呼び出すことができます。

@No__t 0 のメソッドをシャドウすることは無効です。ほとんどの場合、クラスは使用できなくなります。 以下に例を示します。

```vb
MustInherit Class Base
    Public MustOverride Sub F()
End Class

MustInherit Class Derived
    Inherits Base

    Public Shadows Sub F()
    End Sub
End Class

Class MoreDerived
    Inherits Derived

    ' Error: MustOverride method Base.F is not overridden.
End Class
```

この場合、`MustOverride` メソッド `Base.F` をオーバーライドするには、クラス `MoreDerived` が必要ですが、クラス @no__t-@no__t 3 の場合は-4 が使用できません。 @No__t-0 の有効な子孫を宣言することはできません。

外側のスコープから名前をシャドウする場合とは異なり、次の例のように、継承されたスコープからアクセス可能な名前をシャドウすると、警告が報告されます。

```vb
Class Base
    Public Sub F()
    End Sub

    Private Sub G()
    End Sub 
End Class

Class Derived
    Inherits Base

    Public Sub F() ' Warning: shadowing an inherited name.
    End Sub

    Public Sub G() ' No warning, Base.G is not accessible here.
    End Sub
End Class
```

クラス `Derived` のメソッド `F` の宣言により、警告が報告されます。 継承された名前のシャドウは、基本クラスの個別の進化を妨げるため、特にエラーではありません。 たとえば、より新しいバージョンのクラス `Base` では、クラスの以前のバージョンには存在しないメソッド `F` が導入されたため、上記の状況が発生する可能性があります。 上記の状況でエラーが発生した場合、個別にバージョン管理されたクラスライブラリの基底クラス*に変更が*加えられると、派生クラスが無効になる可能性があります。

継承された名前のシャドウによって発生した警告は、`Shadows` または `Overloads` 修飾子を使用して削除できます。

```vb
Class Base
    Public Sub F()
    End Sub 
End Class 

Class Derived
    Inherits Base

    Public Shadows Sub F() 'OK.
    End Sub
End Class
```

@No__t-0 修飾子は、継承されたメンバーをシャドウする目的を示します。 シャドウする型のメンバー名がない場合、`Shadows` または `Overloads` 修飾子を指定してもエラーにはなりません。

新しいメンバーの宣言は、次の例に示すように、新しいメンバーのスコープ内でのみ継承されるメンバーをシャドウします。

```vb
Class Base
    Public Shared Sub F()
    End Sub 
End Class 

Class Derived
    Inherits Base

    Private Shared Shadows Sub F() ' Shadows Base.F in class Derived only.
    End Sub 
End Class 

Class MoreDerived
    Inherits Derived

    Shared Sub G()
        F() ' Invokes Base.F.
    End Sub 
End Class
```

上の例では、クラス `Derived` のメソッド `F` の宣言によって、クラス `Base` から継承されたメソッド `F` がシャドウされますが、クラス `Derived` の新しいメソッド @no__t は @no__t 6 にアクセスするため、そのスコープはクラス `MoreDerived` に拡張されません。 したがって、`MoreDerived.G` の `F()` は有効で、`Base.F` を呼び出します。 オーバーロードされた型のメンバーの場合は、オーバーロードされた型のメンバーのセット全体が、シャドウの目的で最も制限のないアクセス権を持つものとして扱われます。

```vb
Class Base
    Public Sub F()
    End Sub
End Class

Class Derived
    Inherits Base

    Private Shadows Sub F()
    End Sub

    Public Shadows Sub F(i As Integer)
    End Sub
End Class

Class MoreDerived
    Inherits Derived

    Public Sub G()
        F()   ' Error. No accessible member with this signature.
    End Sub
End Class
```

この例では、`Derived` の `F()` の宣言が @no__t 2 のアクセスで宣言されていますが、オーバーロードされた `F(Integer)` は @no__t 4 のアクセスで宣言されています。 このため、シャドウを行うために、`Derived` の @no__t の名前は、@no__t が2であるかのように処理されるため、両方のメソッドが `Base` でシャドウ `F` になります。

## <a name="implementation"></a>実装

*実装*関係は、型がインターフェイスを実装することを宣言し、型がインターフェイスのすべての型メンバーを実装する場合に存在します。 特定のインターフェイスを実装する型は、そのインターフェイスに変換できます。 インターフェイスをインスタンス化することはできませんが、インターフェイスの変数を宣言することは有効です。このような変数には、インターフェイスを実装するクラスの値のみを割り当てることができます。 以下に例を示します。

```vb
Interface ITestable
    Function Test(value As Byte) As Boolean
End Interface

Class TestableClass
    Implements ITestable

    Function Test(value As Byte) As Boolean Implements ITestable.Test
        Return value > 128
    End Function
End Class

Module Test
    Sub F()
        Dim x As ITestable = New TestableClass
        Dim b As Boolean

        b = x.Test(34)
    End Sub
End Module
```

多重継承型メンバーを持つインターフェイスを実装する型は、実装されている派生インターフェイスから直接アクセスできない場合でも、これらのメソッドを実装する必要があります。 以下に例を示します。

```vb
Interface ILeft
    Sub Test()
End Interface

Interface IRight
    Sub Test()
End Interface

Interface ILeftRight
    Inherits ILeft, IRight
End Interface

Class LeftRight
    Implements ILeftRight

    ' Has to reference ILeft explicitly.
    Sub TestLeft() Implements ILeft.Test
    End Sub

    ' Has to reference IRight explicitly.
    Sub TestRight() Implements IRight.Test
    End Sub

    ' Error: Test is not available in ILeftRight.
    Sub TestLeftRight() Implements ILeftRight.Test
    End Sub
End Class
```

@No__t-0 のクラスでも、実装されたインターフェイスのすべてのメンバーの実装を提供する必要があります。ただし、これらのメソッドを `MustOverride` として宣言することで、これらのメソッドの実装を遅らせることができます。 以下に例を示します。

```vb
Interface ITest
    Sub Test1()
    Sub Test2()
End Interface

MustInherit Class TestBase
    Implements ITest

    ' Provides an implementation.
    Sub Test1() Implements ITest.Test1
    End Sub

    ' Defers implementation.
    MustOverride Sub Test2() Implements ITest.Test2
End Class

Class TestDerived
    Inherits TestBase

    ' Have to implement MustOverride method.
    Overrides Sub Test2()
    End Sub
End Class
```

型は、基本型が実装するインターフェイスを再実装することを選択できます。 インターフェイスを再実装するには、型がインターフェイスを実装することを明示的に示す必要があります。 インターフェイスを再実装する型は、インターフェイスのメンバーの一部だけを再実装することを選択できます。再実装されていないメンバーは、基本型の実装を引き続き使用します。 以下に例を示します。

```vb
Class TestBase
    Implements ITest

    Sub Test1() Implements ITest.Test1
        Console.WriteLine("TestBase.Test1")
    End Sub

    Sub Test2() Implements ITest.Test2
        Console.WriteLine("TestBase.Test2")
    End Sub
End Class

Class TestDerived
    Inherits TestBase
    Implements ITest  ' Required to re-implement

    Sub DerivedTest1() Implements ITest.Test1
        Console.WriteLine("TestDerived.DerivedTest1")
    End Sub
End Class

Module Test
    Sub Main()
        Dim Test As ITest = New TestDerived()
        Test.Test1()
        Test.Test2()
    End Sub
End Module
```

次の例では、が出力されます。

```console
TestDerived.DerivedTest1
TestBase.Test2
```

派生型が派生型の基本型によって実装された基本インターフェイスを持つインターフェイスを実装する場合、派生型は、基本型によって実装されていないインターフェイスの型のメンバーのみを実装することを選択できます。 以下に例を示します。

```vb
Interface IBase
    Sub Base()
End Interface

Interface IDerived
    Inherits IBase

    Sub Derived()
End Interface

Class Base
    Implements IBase

    Public Sub Base() Implements IBase.Base
    End Sub
End Class

Class Derived
    Inherits Base
    Implements IDerived

    ' Required: IDerived.Derived not implemented by Base.
    Public Sub Derived() Implements IDerived.Derived
    End Sub
End Class
```

インターフェイスメソッドは、基本型のオーバーライド可能なメソッドを使用して実装することもできます。 その場合、派生型は、オーバーライド可能なメソッドをオーバーライドし、インターフェイスの実装を変更することもできます。 以下に例を示します。

```vb
Class Base
    Implements ITest

    Public Sub Test1() Implements ITest.Test1
        Console.WriteLine("TestBase.Test1")
    End Sub

    Public Overridable Sub Test2() Implements ITest.Test2
        Console.WriteLine("TestBase.Test2")
    End Sub
End Class

Class Derived
    Inherits Base

    ' Overrides base implementation.
    Public Overrides Sub Test2()
        Console.WriteLine("TestDerived.Test2")
    End Sub
End Class
```

### <a name="implementing-methods"></a>メソッドの実装

型は、`Implements` 句を持つメソッドを指定することによって、実装されたインターフェイスの型のメンバーを*実装*します。 2つの型のメンバーは同じ数のパラメーターを持つ必要があります。パラメーターのすべての型と修飾子が一致している必要があります。これには、省略可能なパラメーターの既定値、戻り値の型、およびメソッドのパラメーターに対するすべての制約が一致している必要があります。 以下に例を示します。

```vb
Interface ITest
    Sub F(ByRef x As Integer)
    Sub G(Optional y As Integer = 20)
    Sub H(Paramarray z() As Integer)
End Interface

Class Test
    Implements ITest

    ' Error: ByRef/ByVal mismatch.
    Sub F(x As Integer) Implements ITest.F
    End Sub

    ' Error: Defaults do not match.
    Sub G(Optional y As Integer = 10) Implements ITest.G
    End Sub

    ' Error: Paramarray does not match.
    Sub H(z() As Integer) Implements ITest.H
    End Sub
End Class
```

1つのメソッドが、上記の条件を満たしていれば、任意の数のインターフェイス型メンバーを実装できます。 以下に例を示します。

```vb
Interface ITest
    Sub F(i As Integer)
    Sub G(i As Integer)
End Interface

Class Test

    Implements ITest

    Sub F(i As Integer) Implements ITest.F, ITest.G
    End Sub
End Class
```

ジェネリックインターフェイスでメソッドを実装する場合、実装するメソッドは、インターフェイスの型パラメーターに対応する型引数を指定する必要があります。 以下に例を示します。

```vb
Interface I1(Of U, V) 
    Sub M(x As U, y As List(Of V)) 
End Interface

Class C1(Of W, X)
    Implements I1(Of W, X)

    ' W corresponds to U and X corresponds to V
    Public Sub M(x As W, y As List(Of X)) Implements I1(Of W, X).M
    End Sub 
End Class

Class C2
    Implements I1(Of String, Integer)

    ' String corresponds to U and Integer corresponds to V
    Public Sub M(x As String, y As List(Of Integer)) _
        Implements I1(Of String, Integer).M
    End Sub
End Class
```

ジェネリックインターフェイスは、型引数のセットに対して実装できない可能性があることに注意してください。

```vb
Interface I1(Of T, U)
    Sub S1(x As T)
    Sub S1(y As U)
End Interface

Class C1
    ' Unable to implement because I1.S1 has two identical signatures
    Implements I1(Of Integer, Integer)
End Class
```

## <a name="polymorphism"></a>ポリモーフィズム

*ポリモーフィズム*は、メソッドまたはプロパティの実装を変更する機能を提供します。 ポリモーフィズムでは、同じメソッドまたはプロパティは、それを呼び出すインスタンスの実行時の型に応じて、異なるアクションを実行できます。 ポリモーフィックなメソッドまたはプロパティは、 *overridable*と呼ばれます。 これに対して、オーバーライド不可能なメソッドまたはプロパティの実装は不変です。実装は、メソッドまたはプロパティが宣言されているクラスのインスタンスまたは派生クラスのインスタンスで呼び出されているかどうかと同じです。 オーバーライドできないメソッドまたはプロパティが呼び出されると、インスタンスのコンパイル時の型が決定要因になります。 以下に例を示します。

```vb
Class Base
    Public Overridable Property X() As Integer
        Get
        End Get

        Set
        End Set
    End Property
End Class

Class Derived
    Inherits Base

    Public Overrides Property X() As Integer
        Get
        End Get

        Set
        End Set
    End Property
End Class

Module Test
    Sub F()
        Dim Z As Base

        Z = New Base()
        Z.X = 10            ' Calls Base.X
        Z = New Derived()
        Z.X = 10            ' Calls Derived.X
    End Sub
End Module
```

オーバーライド可能なメソッドは @no__t 0 にすることもできます。これは、メソッドの本体を提供せず、オーバーライドする必要があることを意味します。 `MustOverride` メソッドは、`MustInherit` クラスでのみ使用できます。

次の例では、クラス `Shape` によって、描画に使用できる幾何学図形オブジェクトの抽象概念が定義されています。

```vb
MustInherit Public Class Shape
    Public MustOverride Sub Paint(g As Graphics, r As Rectangle)
End Class 

Public Class Ellipse
    Inherits Shape

    Public Overrides Sub Paint(g As Graphics, r As Rectangle)
        g.drawEllipse(r)
    End Sub 
End Class 

Public Class Box
    Inherits Shape

    Public Overrides Sub Paint(g As Graphics, r As Rectangle)
        g.drawRect(r)
    End Sub 
End Class
```

@No__t-0 メソッドは、意味のある既定の実装がないため、-1 @no__t です。 @No__t-0 および `Box` クラスは、具体的な `Shape` 実装です。 これらのクラスは @no__t 0 ではないため、`Paint` メソッドをオーバーライドし、実際の実装を提供する必要があります。

次の例に示すように、ベースアクセスで @no__t 0 メソッドを参照すると、エラーになります。

```vb
MustInherit Class A
    Public MustOverride Sub F()
End Class

Class B
    Inherits A

    Public Overrides Sub F()
        MyBase.F() ' Error, MyBase.F is MustOverride.
    End Sub 
End Class
```

@No__t-1 メソッドを参照しているため、`MyBase.F()` 呼び出しに対してエラーが報告されます。

### <a name="overriding-methods"></a>オーバーライド (メソッドを)

型は、同じ名前およびシグネチャを持つメソッドを宣言し、`Overrides` 修飾子で宣言をマークすることによって、継承されたオーバーライド可能なメソッドを*オーバーライド*できます。メソッドのオーバーライドには、次に示す追加の要件があります。 @No__t-0 メソッド宣言によって新しいメソッドが導入されるのに対し、メソッドの継承された実装は、`Overrides` メソッドの宣言に置き換えられます。

オーバーライドするメソッドは、`NotOverridable` として宣言できます。これにより、派生型のメソッドをさらにオーバーライドできなくなります。 実際には、@no__t 0 のメソッドは、それ以降の派生クラスではオーバーライド不可能になります。

次に例を示します。

```vb
Class A
    Public Overridable Sub F()
        Console.WriteLine("A.F")
    End Sub

    Public Overridable Sub G()
        Console.WriteLine("A.G")
    End Sub
End Class

Class B
    Inherits A

    Public Overrides NotOverridable Sub F()
        Console.WriteLine("B.F")
    End Sub

    Public Overrides Sub G()
        Console.WriteLine("B.G")
    End Sub
End Class

Class C
    Inherits B

    Public Overrides Sub G()
        Console.WriteLine("C.G")
    End Sub
End Class
```

この例では、クラス `B` は2つの @no__t のメソッドを提供します。1つは `NotOverridable` の修飾子を持ち、@no__t もう1つは @no__t ないメソッドです。 @No__t-0 修飾子を使用すると、クラス `C` は、メソッド `F` をさらにオーバーライドできなくなります。

オーバーライドするメソッドは、オーバーライドするメソッドが-1 @no__t 宣言されていない場合でも、`MustOverride` として宣言できます。 これを行うには、含んでいるクラスが-0 として @no__t 宣言されている必要があります。また、-1 @no__t 宣言されていないその他の派生クラスは、メソッドをオーバーライドする必要があります。 以下に例を示します。

```vb
Class A
    Public Overridable Sub F()
        Console.WriteLine("A.F")
    End Sub
End Class

MustInherit Class B
    Inherits A

    Public Overrides MustOverride Sub F()
End Class
```

この例では、クラス `B` は、`MustOverride` メソッドを使用して `A.F` をオーバーライドします。 これは、`B` から派生したすべてのクラスが `MustInherit` と宣言されている場合を除き、`F` をオーバーライドする必要があることを意味します。

次のすべてがオーバーライドメソッドに当てはまる場合を除き、コンパイル時エラーが発生します。

* 宣言コンテキストには、オーバーライドするメソッドと同じシグネチャと戻り値の型 (存在する場合) を持つ、アクセス可能な単一の継承メソッドが含まれています。
* オーバーライドされる継承メソッドはオーバーライド可能です。 つまり、継承されたメソッドがオーバーライドされると、`Shared` または `NotOverridable` ではありません。
* 宣言されるメソッドのアクセシビリティドメインは、オーバーライドされる継承メソッドのアクセシビリティドメインと同じです。 例外が1つあります。他のメソッドが別のアセンブリ内にあり、オーバーライドする側のメソッドがに @no__t 2 のアクセス権を持っていない場合、`Protected` メソッドによって @no__t 0 メソッドがオーバーライドされる必要があります。
* オーバーライドするメソッドのパラメーターは、省略可能なパラメーターに指定された値を含む `ByVal`、`ByRef`、`ParamArray,` および `Optional` の各修飾子の使用に関して、オーバーライドされたメソッドのパラメーターと一致します。
* オーバーライドするメソッドの型パラメーターは、型制約に関してオーバーライドされたメソッドの型パラメーターと一致します。

基本ジェネリック型のメソッドをオーバーライドする場合、オーバーライドする側のメソッドは、基本型のパラメーターに対応する型引数を指定する必要があります。 以下に例を示します。

```vb
Class Base(Of U, V) 
    Public Overridable Sub M(x As U, y As List(Of V)) 
    End Sub
End Class

Class Derived(Of W, X)
    Inherits Base(Of W, X)

    ' W corresponds to U and X corresponds to V
    Public Overrides Sub M(x As W, y As List(Of X)) 
    End Sub 
End Class

Class MoreDerived
    Inherits Derived(Of String, Integer)

    ' String corresponds to U and Integer corresponds to V
    Public Overrides Sub M(x As String, y As List(Of Integer))
    End Sub
End Class
```

ジェネリッククラスのオーバーライド可能なメソッドは、型引数のセットに対してオーバーライドできない可能性があることに注意してください。 メソッドが `MustOverride` として宣言されている場合は、一部の継承チェーンができないことを意味します。 以下に例を示します。

```vb
MustInherit Class Base(Of T, U)
    Public MustOverride Sub S1(x As T)
    Public MustOverride Sub S1(y As U)
End Class

Class Derived
    Inherits Base(Of Integer, Integer)

    ' Error: Can't override both S1's at once
    Public Overrides Sub S1(x As Integer)
    End Sub
End Class
```

オーバーライド宣言は、次の例のように、ベースアクセスを使用してオーバーライドされた基本メソッドにアクセスできます。

```vb
Class Base
    Private x As Integer

    Public Overridable Sub PrintVariables()
        Console.WriteLine("x = " & x)
    End Sub
End Class

Class Derived
    Inherits Base

    Private y As Integer

    Public Overrides Sub PrintVariables()
        MyBase.PrintVariables()
        Console.WriteLine("y = " & y)
    End Sub
End Class
```

この例では、クラス `Derived` の `MyBase.PrintVariables()` を呼び出すと、クラス `Base` で宣言された `PrintVariables` メソッドが呼び出されます。 基本アクセスは、オーバーライド可能な呼び出し機構を無効にし、単に基本メソッドをオーバーライド不可能なメソッドとして扱います。 @No__t 0 での呼び出しが-1 @no__t 書き込まれている場合、`Base` で宣言されているものではなく、`Derived` で宣言されている `PrintVariables` メソッドを再帰的に呼び出します。

@No__t 0 修飾子を含む場合にのみ、メソッドが別のメソッドをオーバーライドできます。 それ以外の場合、継承されたメソッドと同じシグネチャを持つメソッドは、次の例のように、継承されたメソッドを単純にします。

```vb
Class Base
    Public Overridable Sub F()
    End Sub
End Class

Class Derived
    Inherits Base

    Public Overridable Sub F() ' Warning, shadowing inherited F().
    End Sub
End Class
```

この例では、クラス `Derived` のメソッド `F` は `Overrides` 修飾子を含まないため、クラス @no__t のメソッド `F` はオーバーライドされません。 代わりに、クラス `Derived` のメソッド `F` はクラス `Base` のメソッドをシャドウし、宣言には `Shadows` または `Overloads` 修飾子が含まれていないため、警告が報告されます。

次の例では、クラス `Derived` のメソッド `F` は、クラス `Base` から継承されたオーバーライド可能なメソッド @no__t をシャドウします。

```vb
Class Base
    Public Overridable Sub F()
    End Sub
End Class

Class Derived
    Inherits Base

    Private Shadows Sub F() ' Shadows Base.F within Derived.
    End Sub
End Class

Class MoreDerived
    Inherits Derived

    Public Overrides Sub F() ' Ok, overrides Base.F.
    End Sub
End Class
```

クラス `Derived` の新しいメソッド `F` は @no__t 2 にアクセスしているため、そのスコープには `Derived` のクラス本体のみが含まれ、クラス @no__t には拡張されません。 このため、クラス `MoreDerived` のメソッド `F` の宣言では、クラス `Base` から継承されたメソッド @no__t をオーバーライドできます。

@No__t 0 のメソッドが呼び出されると、インスタンスの型に基づいて、インスタンスメソッドの最も派生した実装が呼び出されます。これは、呼び出しが基底クラスのメソッドに対して行われるか、派生クラスで行われるかには関係ありません。 クラス @no__t @no__t に関して、@no__t 0 メソッドの最も派生する実装は、次のように決定されます。

* @No__t-0 に `M` の導入 @no__t の宣言が含まれている場合、これは `M` の最も派生した実装です。

* それ以外の場合、`R` に `M` のオーバーライドが含まれていると、これが `M` の最も派生した実装になります。

* それ以外の場合、`M` の最も派生する実装は、`R` の直接基底クラスの実装と同じです。

## <a name="accessibility"></a>ユーザー補助

宣言は、宣言するエンティティの*アクセシビリティ*を指定します。 エンティティのアクセシビリティでは、エンティティの名前のスコープは変更されません。 宣言の*アクセシビリティドメイン*は、宣言されたエンティティにアクセスできるすべての宣言空間のセットです。

5つのアクセスの種類は `Public`、`Protected`、`Friend`、`Protected Friend`、および `Private` です。 `Public` は最も制限の厳しいアクセスの種類であり、他の4つの種類はすべて `Public` のサブセットです。 最も制限の緩いアクセスの種類は 0 @no__t で、他の4つのアクセスの種類はすべて `Private` のスーパーセットになります。

```antlr
AccessModifier
    : 'Public'
    | 'Protected'
    | 'Friend'
    | 'Private'
    | 'Protected' 'Friend'
    ;
```

宣言のアクセスの種類は、オプションのアクセス修飾子を使用して指定します。これは、@no__t 0、`Protected`、`Friend`、`Private`、または `Protected` と `Friend` の組み合わせにすることができます。 アクセス修飾子が指定されていない場合、既定のアクセスの種類は宣言コンテキストに依存します。許可されるアクセスの種類は、宣言のコンテキストにも依存します。

* @No__t-0 修飾子を使用して宣言されたエンティティは、@no__t に1つのアクセス権を持ちます。 @No__t-0 エンティティの使用に関する制限はありません。

* @No__t-0 修飾子を使用して宣言されたエンティティは、@no__t に1つのアクセス権を持ちます。 `Protected` アクセスを指定できるのは、クラスのメンバー (通常の型メンバーと入れ子になったクラスの両方)、または標準モジュールおよび構造体の `Overridable` メンバー (定義上、`System.Object` または @no__t から継承する必要があります) のメンバーだけです。 メンバーがインスタンスメンバーでない場合、または派生クラスのインスタンスを介してアクセスが行われる場合は、派生クラスに @no__t 0 のメンバーにアクセスできます。 `Protected` アクセスは `Friend` アクセスのスーパーセットではありません。

* @No__t-0 修飾子を使用して宣言されたエンティティは、@no__t に1つのアクセス権を持ちます。 @No__t 0 のアクセス権を持つエンティティは、エンティティ宣言を含むプログラム内、または `System.Runtime.CompilerServices.InternalsVisibleToAttribute` 属性で `Friend` アクセスが与えられているすべてのアセンブリを含むプログラム内でのみアクセスできます。

* @No__t-0 修飾子で宣言されたエンティティは、`Protected` と @no__t 2 のアクセスの和集合を持ちます。

* @No__t-0 修飾子を使用して宣言されたエンティティは、@no__t に1つのアクセス権を持ちます。 @No__t 0 エンティティは、入れ子になったエンティティも含め、その宣言コンテキスト内でのみアクセスできます。

宣言のアクセシビリティは、宣言コンテキストのアクセシビリティに依存しません。 たとえば、`Private` アクセスで宣言された型には、@no__t が1つのアクセス権を持つ型のメンバーが含まれる場合があります。

次のコードは、さまざまなアクセシビリティドメインを示しています。

```vb
Public Class A
    Public Shared X As Integer
    Friend Shared Y As Integer
    Private Shared Z As Integer
End Class

Friend Class B
    Public Shared X As Integer
    Friend Shared Y As Integer
    Private Shared Z As Integer

    Public Class C
        Public Shared X As Integer
        Friend Shared Y As Integer
        Private Shared Z As Integer
    End Class

    Private Class D
        Public Shared X As Integer
        Friend Shared Y As Integer
        Private Shared Z As Integer
    End Class
End Class
```

この例のクラスとメンバーには、次のアクセシビリティドメインがあります。

* @No__t-0 および `A.X` のアクセシビリティドメインは無制限です。

* @No__t-0、`B`、`B.X`、`B.Y`、`B.C`、`B.C.X`、および `B.C.Y` のアクセシビリティドメインは、含まれているプログラムです。

* @No__t-0 のアクセシビリティドメインが `A.`

* @No__t-0、`B.D`、`B.D.X`、および `B.D.Y` のアクセシビリティドメインは、`B.C` および @no__t を含む `B` です。

* @No__t-0 のアクセシビリティドメインは `B.C` です。

* @No__t-0 のアクセシビリティドメインは `B.D` です。

例に示すように、メンバーのアクセシビリティドメインは、それを含んでいる型のアクセシビリティドメインよりも大きくなることはありません。 たとえば、すべての @no__t 0 のメンバー @no__t がアクセシビリティとして宣言されている場合でも、`A.X` は、包含する型によって制約されるアクセシビリティドメインを持ちます。

関連付けられていない型が互いの保護されたメンバーにアクセスできないようにするには、@no__t 0 のインスタンスメンバーへのアクセスが派生型のインスタンスを経由する必要があります。 以下に例を示します。

```vb
Class User
    Protected Password As String
End Class

Class Employee
    Inherits User
End Class

Class Guest
    Inherits User

    Public Function GetPassword(u As User) As String
        ' Error: protected access has to go through derived type.
        Return U.Password
    End Function
End Class
```

上の例では、クラス `Guest` は、`Guest` のインスタンスで修飾されている場合にのみ、protected `Password` フィールドにアクセスできます。 これにより、`Guest` は `User` にキャストするだけで、`Employee` オブジェクトの `Password` フィールドにアクセスできなくなります。

ジェネリック型での @no__t 0 のメンバーアクセスのために、宣言コンテキストには型パラメーターが含まれています。 これは、型引数の1つのセットを持つ派生型が、型引数の異なるセットを持つ派生型の @no__t 0 のメンバーにアクセスできないことを意味します。 以下に例を示します。

```vb
Class Base(Of T)
    Protected x As T
End Class

Class Derived(Of T)
    Inherits Base(Of T)

    Public Sub F(y As Derived(Of String))
        ' Error: Derived(Of T) cannot access Derived(Of String)'s 
        '     protected members
        y.x = "a"
    End Sub
End Class
```

__付箋.__ このC#言語 (および場合によっては他の言語) では、ジェネリック型は、指定された型引数に関係なく `Protected` のメンバーにアクセスできます。 @No__t 0 のメンバーを含むジェネリッククラスを設計する場合は、この点に留意する必要があります。


### <a name="constituent-types"></a>構成型

宣言の*構成型*は、宣言によって参照される型です。 たとえば、定数の型、メソッドの戻り値の型、コンストラクターのパラメーターの型はすべて構成型です。 宣言の構成型のアクセシビリティドメインは、または宣言自体のアクセシビリティドメインのスーパーセットと同じである必要があります。 以下に例を示します。

```vb
Public Class X
    Private Class Y
    End Class

    ' Error: Exposing private class Y outside of X.
    Public Function Z() As Y
    End Function

    ' Valid: Not exposing outside of X.
    Private Function A() As Y
    End Function
End Class

Friend Class B
    Private Class C
    End Class

    ' Error: Exposing private class Y outside of B.
    Public Function D() As C
    End Function
End Class
```

## <a name="type-and-namespace-names"></a>型と名前空間の名前

多くの言語構成要素では、名前空間または型を指定する必要があります。これらは、名前空間または型の名前の修飾された形式を使用して指定できます。 *修飾名*は、ピリオドで区切られた一連の識別子で構成されます。ピリオドの右側にある識別子は、期間の左側の識別子で指定された宣言空間で解決されます。

名前空間または型の*完全修飾名*は、名前空間と型を含むすべての名前を含む修飾名です。 つまり、名前空間または型の完全修飾名は `N.T` です。ここで `T` はエンティティの名前で、`N` はそのエンティティを含むエンティティの完全修飾名です。

次の例では、いくつかの名前空間と型の宣言と、それに関連付けられた完全修飾名をインラインコメントに示しています。

```vb
Class A            ' A.
End Class

Namespace X        ' X.
    Class B        ' X.B.
        Class C    ' X.B.C.
        End Class
    End Class

    Namespace Y    ' X.Y.
        Class D    ' X.Y.D.
        End Class
    End Namespace 
End Namespace 

Namespace X.Y      ' X.Y.
    Class E        ' X.Y.E.
    End Class
End Namespace
```

名前空間の X.y がソースコード内の2つの異なる場所で宣言されていることに注意してください。ただし、これらの2つの部分宣言は、クラス D とクラス E の両方を含む、X.y という名前空間を1つだけ構成します。

場合によっては、修飾名の先頭にキーワード `Global` が使用されることがあります。 キーワードは、名前のない最も外側の名前空間を表します。これは、宣言が外側の名前空間をシャドウする場合に便利です。 @No__t-0 キーワードを使用すると、そのような状況で最も外側の名前空間に "エスケープ" できます。 以下に例を示します。

```vb
Namespace NS1
    Class System
    End Class

    Module Test
        Sub Main()
            ' Error: Class System does not contain Int32
            Dim x As System.Int32


            ' Legal, binds to System in outermost namespace
            Dim y As Global.System.Int32
        End Sub
    End Module
End Namespace
```

上の例では、最初のメソッド呼び出しは無効です。識別子 `System` は、名前空間 `System` ではなく、クラス `System` にバインドされています。 @No__t-0 名前空間にアクセスする唯一の方法は、`Global` を使用して、最も外側の名前空間をエスケープすることです。 `Global` は、`Imports` ステートメントまたは `Namespace` 宣言では使用できません。

言語のキーワードと一致する型と名前空間が他の言語で導入される可能性があるため、Visual Basic は、ピリオドの後に続く限り、修飾名の一部としてキーワードを認識します。 このように使用されるキーワードは、識別子として扱われます。 たとえば、修飾識別子 `X.Default.Class` は有効な修飾識別子ですが、`Default.Class` は有効ではありません。

### <a name="qualified-name-resolution-for-namespaces-and-types"></a>名前空間と型の修飾名の解決

@No__t-0 という形式の修飾された名前空間または型名を指定した場合、`R` は修飾名の右端の識別子、`A` は省略可能な型引数リストです。次の手順では、名前空間を決定する方法または修飾名を入力する方法について説明します。呼び

1. 修飾名または非修飾名前解決の規則を使用して `N` を解決します。

2. @No__t-0 の解決が失敗した場合、または型パラメーターに解決された場合は、コンパイル時エラーが発生します。

3. それ以外の場合、`R` が N の名前空間の名前と一致し、型引数が指定されていないか、または `R` が型引数と同じ数の型パラメーターを持つ `N` のアクセス可能な型 (存在する場合) と一致する場合、修飾名はその名前空間または型を参照します.

4. それ以外の場合、`N` に1つ以上の標準モジュールが含まれており、`R` が型引数と同じ数の型パラメーターを持つアクセス可能な型の名前 (存在する場合) が1つの標準モジュールで一致する場合、修飾名はその型を参照します。 @No__t-0 が型引数と同じ数の型パラメーターを持つアクセス可能な型の名前と一致する場合 (存在する場合)、コンパイル時エラーが発生します。

5. それ以外の場合は、コンパイル時のエラーが発生します。

__付箋.__ この解決プロセスの意味では、型のメンバーは名前空間または型名を解決するときに名前空間または型をシャドウしません。

### <a name="unqualified-name-resolution-for-namespaces-and-types"></a>名前空間と型の非修飾名前解決

修飾されていない名前 `R(Of A)` の場合、`A` は省略可能な型引数リストです。次の手順では、非修飾名が参照している名前空間または型を特定する方法について説明します。

1. R が現在のメソッドの型パラメーターの名前と一致し、型引数が指定されていない場合、非修飾名はその型パラメーターを参照します。

2.  名前参照を含む入れ子にされた型ごとに、最も内側の型から外側のに移動します。
    1. @No__t-0 が現在の型の型パラメーターの名前と一致し、型引数が指定されていない場合、非修飾名はその型パラメーターを参照します。
    2. それ以外の場合、`R` が型引数と同じ数の型パラメーターを持つ、アクセス可能な入れ子にされた型の名前と一致すると、非修飾名はその型を参照します。

3. 名前参照を含む入れ子になった名前空間ごとに、最も内側の名前空間から始まり、最も外側の名前空間に移動します。
    1. @No__t-0 が現在の名前空間の入れ子になった名前空間の名前と一致し、型引数リストが指定されていない場合、非修飾名はその入れ子になった名前空間を参照します。
    2. それ以外の場合、`R` は、現在の名前空間で型引数と同じ数の型パラメーター (存在する場合) を持つアクセス可能な型の名前と一致する場合、非修飾名はその型を参照します。
    3. それ以外の場合、名前空間にアクセス可能な標準モジュールが1つ以上含まれていて、`R` が型引数と同じ数の型パラメーター (存在する場合) と同じ数の型パラメーターを持つ、アクセス可能な入れ子にされた型の名前と一致する場合は、非修飾名は、その入れ子になった型を参照します。 @No__t-0 が、1つの型引数と同じ数の型パラメーター (存在する場合) を持つ、アクセス可能な入れ子にされた型の名前と一致する場合、コンパイル時エラーが発生します。

4. ソースファイルに1つ以上のインポートエイリアスがあり、`R` がそのいずれかの名前と一致する場合、非修飾名はそのインポートエイリアスを参照します。 型引数リストが指定されている場合、コンパイル時エラーが発生します。

5. 名前参照を含むソースファイルに1つ以上のインポートがある場合は、次のようになります。
    1. @No__t-0 が型引数と同じ数の型パラメーターを持つアクセス可能な型の名前と一致する場合 (ある場合)、厳密に1つのインポートでは、非修飾名はその型を参照します。 @No__t-0 が型引数と同じ数の型パラメーターを持つアクセス可能な型の名前と一致する場合 (存在する場合)、複数のインポートとすべてが同じ型ではない場合、コンパイル時エラーが発生します。
    2. それ以外の場合、型引数リストが指定されておらず、`R` は、厳密に1つのインポートでアクセス可能な型を持つ名前空間の名前と一致する場合、非修飾名はその名前空間を参照します。 型引数リストが指定されておらず、`R` が2つ以上のインポートでアクセス可能な型の名前空間の名前と一致し、すべてが同じ名前空間ではない場合、コンパイル時エラーが発生します。
    3. それ以外の場合、インポートにアクセス可能な標準モジュールが1つ以上含まれていて、`R` が型引数と同じ数の型パラメーター (存在する場合) と同じ数の型パラメーターを持つ、アクセス可能な入れ子にされた型の名前と一致する場合、非修飾名はその型。 @No__t-0 が、1つの型引数と同じ数の型パラメーター (存在する場合) を持つ、アクセス可能な入れ子にされた型の名前と一致する場合、コンパイル時エラーが発生します。

6. コンパイル環境で1つ以上のインポートエイリアスが定義されていて、`R` がそのいずれかの名前と一致する場合、非修飾名はそのインポートエイリアスを参照します。 型引数リストが指定されている場合、コンパイル時エラーが発生します。

7. コンパイル環境で1つ以上のインポートを定義する場合は、次のようになります。
    1. @No__t-0 が型引数と同じ数の型パラメーターを持つアクセス可能な型の名前と一致する場合 (ある場合)、厳密に1つのインポートでは、非修飾名はその型を参照します。 @No__t-0 が型引数と同じ数の型パラメーターを持つアクセス可能な型の名前と一致する場合 (存在する場合)、コンパイル時エラーが発生します。
    2. それ以外の場合、型引数リストが指定されておらず、`R` は、厳密に1つのインポートでアクセス可能な型を持つ名前空間の名前と一致する場合、非修飾名はその名前空間を参照します。 型引数リストが指定されておらず、`R` が2つ以上のインポートでアクセス可能な型の名前空間の名前と一致する場合、コンパイル時エラーが発生します。
    3. それ以外の場合、インポートにアクセス可能な標準モジュールが1つ以上含まれていて、`R` が型引数と同じ数の型パラメーター (存在する場合) と同じ数の型パラメーターを持つ、アクセス可能な入れ子にされた型の名前と一致する場合、非修飾名はその型。 @No__t-0 が、1つの型引数と同じ数の型パラメーター (存在する場合) を持つ、アクセス可能な入れ子にされた型の名前と一致する場合、コンパイル時エラーが発生します。

8. それ以外の場合は、コンパイル時のエラーが発生します。

__付箋.__ この解決プロセスの意味では、型のメンバーは名前空間または型名を解決するときに名前空間または型をシャドウしません。

通常、名前は特定の名前空間に1回だけ出現できます。 ただし、複数の .NET アセンブリにわたって名前空間を宣言できるため、2つのアセンブリが同じ完全修飾名を持つ型を定義する状況が発生する可能性があります。 この場合、ソースファイルの現在のセットで宣言されている型は、外部の .NET アセンブリで宣言されている型よりも優先されます。 それ以外の場合、名前はあいまいであり、名前を明確に区別する方法はありません。

## <a name="variables"></a>変数:

*変数*は、格納場所を表します。 すべての変数には、変数に格納できる値を決定する型があります。 Visual Basic はタイプセーフな言語であるため、プログラム内のすべての変数には型があり、言語は変数に格納されている値が常に適切な型であることを保証します。 変数への参照を行う前に、変数は常にその型の既定値に初期化されます。 初期化されていないメモリにアクセスすることはできません。

## <a name="generic-types-and-methods"></a>ジェネリック型とジェネリックメソッド

型 (標準モジュールと列挙型を除く) とメソッドでは、型パラメーターを宣言できます。*型パラメーター*は、型のインスタンスが宣言されるか、メソッドが呼び出されるまで提供されない型です。 型パラメーターを持つ型とメソッドは、それぞれ*ジェネリック型*および*ジェネリックメソッド*とも呼ばれます。これは、型またはメソッドを使用するコードによって提供される型に関する特定の知識がなくても、型またはメソッドが一般に記述されている必要があるためです。型またはメソッド。

__付箋.__ 現時点では、メソッドとデリゲートはジェネリックにすることができますが、プロパティ、イベント、および演算子をジェネリックにすることはできません。 ただし、包含クラスの型パラメーターを使用することもできます。

ジェネリック型またはジェネリックメソッドの観点からは、型パラメーターは、型またはメソッドが使用されたときに実際の型を格納するプレースホルダー型です。 型引数は、型またはメソッドが使用されている点で、型またはメソッドの型パラメーターの代わりに使用されます。 たとえば、ジェネリックスタッククラスは次のように実装できます。

```vb
Public Class Stack(Of ItemType)
    Protected Items(0 To 99) As ItemType
    Protected CurrentIndex As Integer = 0

    Public Sub Push(data As ItemType)
        If CurrentIndex = 100 Then
            Throw New ArgumentException("Stack is full.")
        End If

        Items(CurrentIndex) = Data
        CurrentIndex += 1
    End Sub

    Public Function Pop() As ItemType
        If CurrentIndex = 0 Then
            Throw New ArgumentException("Stack is empty.")
        End If

        CurrentIndex -= 1
        Return Items(CurrentIndex + 1) 
    End Function
End Class
```

@No__t-0 クラスを使用する宣言では、型パラメーター `ItemType` に型引数を指定する必要があります。 この型は、クラス内で `ItemType` が使用されている場所に格納されます。

```vb
Option Strict On

Module Test
    Sub Main()
        Dim s1 As New Stack(Of Integer)()
        Dim s2 As New Stack(Of Double)()

        s1.Push(10.10)   ' Error: Stack(Of Integer).Push takes an Integer
        s2.Push(10.10)   ' OK: Stack(Of Double).Push takes a Double
        Console.WriteLine(s2.Pop().GetType().ToString()) ' Prints: Double
    End Sub
End Module
```

### <a name="type-parameters"></a>型パラメーター

型パラメーターは、型またはメソッドの宣言で指定できます。 各型パラメーターは識別子であり、構築された型またはメソッドを作成するために提供される型引数のプレースホルダーです。 これに対し、型引数は、ジェネリック型またはジェネリックメソッドが使用されている場合に、型パラメーターに置き換えられる実際の型です。

```antlr
TypeParameterList
    : OpenParenthesis 'Of' TypeParameter ( Comma TypeParameter )* CloseParenthesis
    ;

TypeParameter
    : VarianceModifier? Identifier TypeParameterConstraints?
    ;

VarianceModifier
    : 'In' | 'Out'
    ;
```

型またはメソッドの宣言のそれぞれの型パラメーターは、その型またはメソッドの宣言空間で名前を定義します。 そのため、別の型パラメーター、型メンバー、メソッドパラメーター、またはローカル変数と同じ名前を指定することはできません。 型またはメソッドの型パラメーターのスコープは、型またはメソッド全体です。 型パラメーターは型宣言全体にスコープが設定されているので、入れ子にされた型は外部型パラメーターを使用できます。 また、ジェネリック型内で入れ子にされた型にアクセスするときは、常に型パラメーターを指定する必要があります。

```vb
Public Class Outer(Of T)
    Public Class Inner
        Public Sub F(x As T)
            ...
        End Sub
    End Class
End Class

Module Test
    Sub Main()
        Dim x As New Outer(Of Integer).Inner()
        ...
    End Sub
End Module
```

クラスの他のメンバーとは異なり、型パラメーターは継承されません。 型の型パラメーターは、単純な名前によってのみ参照できます。つまり、包含する型名で修飾することはできません。 無効なプログラミングスタイルですが、入れ子にされた型の型パラメーターは、外側の型で宣言されたメンバーまたは型パラメーターを隠ぺいすることができます。

```vb
Class Outer(Of T)
    Class Inner(Of T)
        Public t1 As T    ' Refers to Inner's T
    End Class
End Class
```

型およびメソッドは、型またはメソッドが宣言する型パラメーター (または*アリティ*) の数に基づいてオーバーロードできます。 たとえば、次の宣言は有効です。

```vb
Module C
    Sub M()
    End Sub

    Sub M(Of T)()
    End Sub

    Sub M(Of T, U)()
    End Sub
End Module

Structure C(Of T)
    Dim x As T
End Structure

Class C(Of T, U)
End Class
```

型の場合、オーバーロードは常に、指定された型引数の数と一致します。 これは、ジェネリックと非ジェネリックの両方のクラスを同じプログラムで一緒に使用する場合に便利です。

```vb
Class Queue 
End Class      

Class Queue(Of T)
End Class

Class X
    Dim q1 As Queue                 ' Non-generic queue
    Dim q2 As Queue(Of Integer)     ' Generic queue
End Class
```

型パラメーターでオーバーロードされたメソッドの規則については、メソッドのオーバーロードの解決に関するセクションで説明します。

包含宣言内では、型パラメーターは完全な型と見なされます。 型パラメーターは、さまざまな異なる実際の型引数を使用してインスタンス化できるため、次に示すように、型パラメーターには、他の型とは若干異なる操作と制限があります。

* 型パラメーターを直接使用して基底クラスまたはインターフェイスを宣言することはできません。

* 型パラメーターに対するメンバー参照の規則は、型パラメーターに適用される制約によって異なります。

* 型パラメーターに使用できる変換は、型パラメーターに適用される制約によって異なります。

* @No__t 0 の制約がない場合、型パラメーターによって表される型の値は、`Is` および @no__t を使用して `Nothing` と比較できます。

* 型パラメーターが `New` または `Structure` の制約によって制約されている場合、`New` 式でのみ使用できます。

* 型パラメーターは、`GetType` 式内の属性例外内の任意の場所で使用することはできません。

* 型パラメーターは、他のジェネリック型およびパラメーターの型引数として使用できます。

次の例は、`Stack(Of ItemType)` クラスを拡張するジェネリック型です。

```vb
Class MyStack(Of ItemType)
    Inherits Stack(Of ItemType)

    Public ReadOnly Property Size() As Integer
        Get
            Return CurrentIndex
        End Get
    End Property
End Class
```

宣言で `MyStack` に型引数を指定すると、同じ型引数が `Stack` にも適用されます。

型として、型パラメーターは純粋にコンパイル時の構成要素です。 実行時に、各型パラメーターは、ジェネリック宣言に型引数を指定して指定されたランタイム型にバインドされます。 したがって、型パラメーターで宣言された変数の型は、実行時に非ジェネリック型または特定の構築型になります。 型パラメーターを含むすべてのステートメントおよび式の実行時の実行では、そのパラメーターの型引数として指定された実際の型を使用します。


### <a name="type-constraints"></a>型の制約

型引数は型システム内の任意の型にすることができるため、ジェネリック型またはジェネリックメソッドは型パラメーターについての想定を行うことができません。 したがって、型パラメーターのメンバーは、すべての型が @no__t から派生しているため、型のメンバーとして `Object` と見なされます。

@No__t-0 のようなコレクションの場合、この事実は特に重要な制限とは言えませんが、ジェネリック型が型引数として指定される型について想定することが必要になる場合があります。 型の*制約*は、型パラメーターとして指定できる型を制限する型パラメーターに配置できます。また、ジェネリック型またはメソッドが型パラメーターについてより多くのものを想定できるようにします。

```antlr
TypeParameterConstraints
    : 'As' Constraint
    | 'As' OpenCurlyBrace ConstraintList CloseCurlyBrace
    ;

ConstraintList
    : Constraint ( Comma Constraint )*
    ;

Constraint
    : TypeName
    | 'New'
    | 'Structure'
    | 'Class'
    ;
```


```vb
Public Class DisposableStack(Of ItemType As IDisposable)
    Implements IDisposable

    Private _items(0 To 99) As ItemType
    Private _currentIndex As Integer = 0

    Public Sub Push(data As ItemType)
        ...
    End Sub

    Public Function Pop() As ItemType
        ...
    End Function

    Private Sub Dispose() Implements IDisposable.Dispose
        For Each item As IDisposable In _items
            If item IsNot Nothing Then
                item.Dispose()
            End If
        Next item
    End Sub
End Class
```

この例では、`DisposableStack(Of ItemType)` は、その型パラメーターを、インターフェイスを実装する型のみを `System.IDisposable` に制限します。 その結果、キューに残っているオブジェクトを破棄する @no__t 0 のメソッドを実装できます。

型制約は、特別な制約の1つである必要があります (-0、`Structure`、または `New`)。または、次のいずれかの @no__t 型である必要があります: @no__t。

* `T` はクラス、インターフェイス、または型パラメーターです。

* `T` が `NotInheritable` ではありません。

* `T` は、次の特殊な型 (`System.Array`、`System.Delegate`、`System.MulticastDelegate`、`System.Enum`、または `System.ValueType`) のいずれかではありません。

* `T` が `Object` ではありません。 すべての型は @no__t 0 から派生するため、このような制約は、許可されている場合は効果がありません。

* `T` は、宣言するジェネリック型またはメソッドと同じように、少なくともアクセス可能である必要があります。

型制約を中かっこ (`{}`) で囲むことによって、1つの型パラメーターに複数の型制約を指定できます。 クラスには、特定の型パラメーターに対して1つの型制約のみを指定できます。 @No__t 0 の特殊な制約を名前付きクラスの制約または @no__t の特殊な制約と組み合わせると、エラーになります。

```vb
Class ControlFactory(Of T As {Control, New})
    ...
End Class
```

型制約は、含んでいる型、またはそれを含む型の型パラメーターを使用できます。 次の例では、制約によって、指定された型引数が、それ自体を型引数として使用するジェネリックインターフェイスを実装する必要があります。

```vb
Class Sorter(Of V As IComparable(Of V))
    ...
End Class
```

特殊な型制約 `Class` は、指定された型引数を任意の参照型に制限します。

__付箋.__ 特別な型制約 `Class` は、インターフェイスで満たすことができます。 構造体はインターフェイスを実装できます。 したがって、構造体 (`Class` の特殊な制約を満たしていない)、および "U" が実装するインターフェイス (@no__t 2 の特殊な制約を満たす) では、制約 `(Of T As U, U As Class)` になることがあります。

特殊な型制約 `Structure` は、指定された型引数を、`System.Nullable(Of T)` 以外の任意の値型に制限します。

__付箋.__ 構造体の制約では、型引数として `System.Nullable(Of T)` を指定することができないように、`System.Nullable(Of T)` は許可されません。

特別な型制約 `New` では、指定された型引数にアクセス可能なパラメーターなしのコンストラクターが必要であり、`MustInherit` として宣言できない必要があります。 以下に例を示します。

```vb
Class Factory(Of T As New)
    Function CreateInstance() As T
        Return New T()
    End Function
End Class
```

クラス型の制約では、指定された型引数が型であるか、それを継承している必要があります。 インターフェイス型の制約では、指定された型引数にそのインターフェイスを実装する必要があります。 型パラメーター制約では、指定された型引数が、対応する型パラメーターに指定されたすべての境界から派生するか、またはすべてを実装する必要があります。 以下に例を示します。

```vb
Class List(Of T)
    Sub AddRange(Of S As T)(collection As IEnumerable(Of S))
        ...
    End Sub
End Class
```

この例では、`AddRange` の型パラメーター `S` は、`List` の型パラメーター @no__t に制限されています。 つまり、`List(Of Control)` は、`AddRange` の型パラメーターを `Control` から継承する任意の型に制限します。

型パラメーターの制約 `Of S As T` は、特別な制約 (`Class`、`Structure`、`New`) を除き、`T` のすべての制約を `S` に推移的に追加することによって解決されます。 循環制約 (`Of S As T, T As S` など) を設定するとエラーになります。 型パラメーターの制約を持つことはできませんが、それ自体には @no__t 0 の制約があります。 制約を追加した後、さまざまな特殊な状況が発生する可能性があります。

* 複数のクラス制約が存在する場合、ほとんどの派生クラスは制約と見なされます。 1つ以上のクラス制約に継承関係がない場合、制約は満たしであり、エラーになります。

 * 型パラメーターが、名前付きのクラス制約または `Class` の特殊な制約を持つ @no__t 0 の特殊な制約を結合する場合、エラーになります。 クラスの制約は @no__t 0 にすることができます。この場合、その制約の派生型は受け入れられず、エラーになります。

型には、のいずれか、またはから継承された型 (`System.Array`、`System.Delegate`、`System.MulticastDelegate`、`System.Enum`、または `System.ValueType`) を指定できます。 その場合は、型またはそれから継承された型のみが受け入れられます。 これらの型のいずれかに制限されている型パラメーターでは、`DirectCast` 演算子で許可されている変換のみ使用できます。 以下に例を示します。

```vb
MustInherit Class Base(Of T)
    MustOverride Sub S1(Of U As T)(x As U)
End Class

Class Derived
    Inherits Base(Of Integer)

    ' The constraint of U must be Integer, which is normally not allowed.
    Overrides Sub S1(Of U As Integer)(x As U)
        Dim y As Integer = x    ' OK
        Dim z As Long = x       ' Error: Can't convert
    End Sub
End Class
```

また、上記のいずれかのリラクゼーションによって値型に制約される型パラメーターは、その値型で定義されているメソッドを呼び出すことはできません。 以下に例を示します。

```vb
Class C1(Of T)
    Overridable Sub F(Of G As T)(x As G)
    End Sub
End Class

Class C2
    Inherits C1(Of IntPtr)

    Overrides Sub F(Of G As IntPtr)(ByVal x As G)
        ' Error: Cannot access structure members
         x.ToInt32()
    End Sub
End Class
```

置換後の制約が配列型として終了する場合は、共変の配列型も許可されます。 以下に例を示します。

```vb
Module Test
    Class B
    End Class

    Class D
        Inherits B
    End Class

    Function F(Of T, U As T)(x As U) As T
        Return x
    End Function

    Sub Main()
        Dim a(9) As B
        Dim b(9) As D

        a = F(Of B(), D())(b)
    End Sub
End Module
```

クラスまたはインターフェイスの制約を持つ型パラメーターは、そのクラスまたはインターフェイスの制約と同じメンバーを持つと見なされます。 型パラメーターに複数の制約がある場合、型パラメーターは、制約のすべてのメンバーの和集合を持つと見なされます。 複数の制約に同じ名前のメンバーがある場合、メンバーは次の順序で非表示になります。クラスの制約は、インターフェイスの制約のメンバーを非表示にします。これにより、`Structure` の制約が指定されている場合に `System.ValueType` のメンバーが非表示になります。`Object` のメンバー。 同じ名前のメンバーが複数のインターフェイスの制約に含まれている場合は、(複数のインターフェイスの継承のように) メンバーを使用できず、型パラメーターを目的のインターフェイスにキャストする必要があります。 以下に例を示します。

```vb
Class C1
    Sub S1(x As Integer)
    End Sub
End Class

Interface I1
    Sub S1(x As Integer)
End Interface

Interface I2
    Sub S1(y As Double)
End Interface

Module Test
    Sub T1(Of T As {C1, I1, I2})()
        Dim a As T
        a.S1(10)       ' Calls C1.S1, which is preferred
        a.S1(10.10)    ' Also calls C1.S1, class is still preferred
    End Sub

    Sub T2(Of T As {I1, I2})()
        Dim a As T
        a.S1(10)    ' Error: Call is ambiguous between I1.S1, I2.S1
    End Sub
End Module
```

型パラメーターを型引数として指定する場合は、型パラメーターが一致する型パラメーターの制約を満たしている必要があります。

```vb
Class Base(Of T As Class)
End Class

Class Derived(Of V)
    ' Error: V does not satisfy the constraints of T
    Inherits Base(Of V)
End Class
```

制約付き型パラメーターの値を使用して、制約で指定されたインスタンスメソッドを含むインスタンスメンバーにアクセスできます。

```vb
Interface IPrintable
    Sub Print()
End Interface

Class Printer(Of V As IPrintable)
    Sub PrintOne(v1 As V)
        V1.Print()
    End Sub
End Class
```

### <a name="type-parameter-variance"></a>型パラメーターの分散

インターフェイスまたはデリゲート型の宣言の型パラメーターは、必要に応じて、*分散修飾子*を指定できます。 型パラメーターと変位修飾子を使用すると、インターフェイスまたはデリゲート型で型パラメーターを使用する方法が制限されますが、ジェネリックインターフェイスまたはデリゲート型を、バリアントと互換性のある型引数を持つ別のジェネリック型に変換することができます。 以下に例を示します。

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class

Module Test
    Sub Main()
        Dim x As IEnumerable(Of Derived) = ...

        ' OK, as IEnumerable(Of Base) is variant compatible
        ' with IEnumerable(Of Derived)
        Dim y As IEnumerable(Of Base) = x
    End Sub
End Module
```

変位修飾子を持つ型パラメーターを持つジェネリックインターフェイスには、いくつかの制限があります。

* パラメーターリストを指定するイベント宣言を含めることはできません (ただし、カスタムイベント宣言や、デリゲート型を持つイベント宣言は許可されます)。

* 入れ子になったクラス、構造体、または列挙型を含めることはできません。

__付箋.__ これらの制限は、ジェネリック型で入れ子にされた型がその親のジェネリックパラメーターを暗黙的にコピーするという事実に起因します。 クラス、構造体、または列挙型が入れ子になっている場合、それらの型の型パラメーターには変位修飾子を設定できません。 パラメーターリストを持つイベント宣言の場合、生成された入れ子になったデリゲートクラスでは、@no__t 0 の位置 (パラメーター型) で使用されている型が実際には、`Out` の位置 (つまり、パラメーター型) で使用されていると、混乱を招く可能性があります。イベント)。

Out 修飾子で宣言された型パラメーターは*共変*です。 非公式には、共変の型パラメーターは、出力位置 (インターフェイスまたはデリゲート型から返される値) でのみ使用できます。これは、入力位置では使用できません。 次の場合、型 `T` は*有効*であると見なされます。

* `T` はクラス、構造体、または列挙型です。

* `T` は、非ジェネリックデリゲートまたはインターフェイス型です。

* `T` は、要素型が有効である配列型です。

* `T` は、`Out` の型パラメーターとして宣言されていない型パラメーターです。

* `T` は、次のように、型引数 `X(Of P1,...,Pn)` を持つ、構築されたインターフェイスまたはデリゲート型 @no__t です。

  * @No__t-0 が Out 型パラメーターとして宣言されている場合、`Ai` は有効です。

  * @No__t-0 が型パラメーターとして宣言されている場合、`Ai` は有効な contravariantly です。

インターフェイスまたはデリゲート型では、次のものが有効である必要があります。

* インターフェイスの基本インターフェイス。

* 関数またはデリゲート型の戻り値の型。

* @No__t 0 のアクセサーがある場合のプロパティの型。

* @No__t-0 パラメーターの型。

以下に例を示します。

```vb
Delegate Function D(Of Out T, U)(x As U) As T

Interface I1(Of Out T)
End Interface

Interface I2(Of Out T)
    Inherits I1(Of T)

    ' OK, T is only used in an Out position
    Function M1(x As I1(Of T)) As T

    ' Error: T is used in an In position
    Function M2(x As T) As T
End Interface
```

__付箋.__ `Out` は予約語ではありません。

In 修飾子で宣言された型パラメーターは*反変*です。 これに対して、反変の型パラメーターは、入力位置 (つまり、インターフェイスまたはデリゲート型に渡される値) でのみ使用でき、出力位置では使用できません。 次の場合、型 `T` は*有効な contravariantly*と見なされます。

* `T` はクラス、構造体、または列挙型です。

* `T` は、非ジェネリックデリゲートまたはインターフェイス型です。

* `T` は、要素型が有効な contravariantly である配列型です。

* `T` は、型パラメーターとして宣言されていない型パラメーターです。

* `T` は、次のように、型引数 `X(Of P1,...,Pn)` を持つ、構築されたインターフェイスまたはデリゲート型 @no__t です。

  * @No__t-0 が `Out` の型パラメーターとして宣言されている場合、`Ai` は有効な contravariantly です。

  * @No__t-0 が `In` の型パラメーターとして宣言されている場合、`Ai` は有効です。

次のは、インターフェイスまたはデリゲート型の有効な contravariantly である必要があります。

* パラメーターの型。

* メソッド型パラメーターの型制約。

* @No__t 0 のアクセサーがある場合のプロパティの型。

* イベントの種類。

以下に例を示します。

```vb
Delegate Function D(Of T, In U)(x As U) As T

Interface I1(Of In T)
End Interface

Interface I2(Of In T)
    ' OK, T is only used in an In position
    Sub M1(x As I1(Of T))

    ' Error: T is used in an Out position
    Function M2() As T
End Interface
```

型が有効である必要がある場合 (@no__t 0 と `Set` の両方のアクセサーを持つプロパティや、`ByRef` のパラメーター)、バリアント型のパラメーターは使用できません。


同時変性と反変性により、"ダイヤモンドのあいまいさに関する問題" が増加します。 次のコードがあるとします。

```vb
Class C
    Implements IEnumerable(Of String)
    Implements IEnumerable(Of Exception)
     
    Public Function GetEnumerator1() As IEnumerator(Of String) _
       Implements IEnumerable(Of String).GetEnumerator
       Console.WriteLine("string")
    End Function
     
    Public Function GetEnumerator2() As IEnumerator(Of Exception) _
       Implements IEnumerable(Of Execption).GetEnumerator
       Console.WriteLine("exception")
    End Function
End Class
     
Dim c As IEnumerable(Of Object) = New C
c.GetEnumerator()
```

クラス `C` は、2つの方法で `IEnumerable(Of Object)` に変換できます。これには、`IEnumerable(Of String)` からの共変の変換と、`IEnumerable(Of Exception)` からの共変の変換を使用します。 CLR では、2つのメソッドのどちらが `c.GetEnumerator()` によって呼び出されるかは指定されていません。 一般に、共通のスーパータイプを持つ2つの異なるジェネリック引数を持つ共変のインターフェイスを実装するようにクラスが宣言されている場合 (この例では `String` と @no__t に共通のスーパータイプ `Object`)、またはクラスがを実装するように宣言されています。共通のサブタイプを持つ2つの異なるジェネリック引数を持つ反変のインターフェイスは、あいまいさが発生する可能性があります。 コンパイラは、このような宣言に対して警告を出します。
