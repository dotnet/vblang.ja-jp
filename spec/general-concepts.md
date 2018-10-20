# <a name="general-concepts"></a>一般的な概念

この章では、さまざまな Microsoft Visual Basic 言語のセマンティクスを理解するために必要な概念について説明します。 Visual Basic プログラマまたは C と C++ のプログラマにとって馴染み深い概念の多くする必要がありますが、厳密な定義が異なる場合があります。

## <a name="declarations"></a>宣言

Visual Basic プログラムは名前付きエンティティで構成をされます。 これらのエンティティがで導入された*宣言*プログラムの「意味」を表します。

最上位レベルの場合で*名前空間*は入れ子になった名前空間と型などの他のエンティティを整理するエンティティです。 *型*は値を記述し、実行可能コードを定義するエンティティです。 型は、入れ子にされた型を含めることがよぶ型のメンバー場合があります。 *メンバーの入力*定数、変数、メソッド、演算子、プロパティ、イベント、列挙値、およびコンス トラクター。

その他のエンティティを含むことのできるエンティティの定義、*宣言領域*します。 宣言または継承を通じていずれかの宣言領域にエンティティが導入されます。エンティティのコンテナーの宣言領域と呼びます*宣言コンテキスト*します。 さらに入れ子になったエンティティ宣言を含むことのできる新しい宣言領域を定義する宣言領域内のエンティティをさらに宣言します。そのため、プログラム内の宣言は、宣言のスペースの階層を形成します。

除くをオーバー ロードされた型のメンバーの場合は同じ宣言のコンテキストに同じ種類の同じ名前のエンティティを導入する宣言に対して無効です。 さらに、宣言領域で可能性があります。 同じ名前のエンティティのさまざまな種類を含めることはありません。たとえば、宣言領域ことはありませんがあります変数とメソッド、同じ名前で。

__注意してください。__ 他の言語 (たとえば、可能な場合、言語大文字と小文字は大文字小文字の区別に基づいてさまざまな宣言) の種類の異なる同じ名前のエンティティを含む宣言領域を作成する可能性がある場合があります。 そのような状況で最もアクセスしやすいエンティティはその名前にバインドされたと見なされますエンティティの 1 つ以上の型が最もアクセスしやすい場合、名前があいまいです。 `Public` 方がより使いやすく`Protected Friend`、`Protected Friend`方がより使いやすく`Protected`または`Friend`、および`Protected`または`Friend`方がより使いやすく`Private`します。

名前空間の宣言領域は、「開いている終了した場合は、」同じ完全修飾名で 2 つの名前空間宣言が同じ宣言領域に貢献するようにします。 次の例で、2 つの名前空間宣言はここで完全修飾の名前を持つ 2 つのクラスを宣言する、同じ宣言領域に貢献`Data.Customer`と `Data.Order:`

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

同じ宣言領域に 2 つの宣言がドキュメントに投稿するため、宣言と同じ名前のクラスのそれぞれに含まれる場合、コンパイル時エラーが発生します。

### <a name="overloading-and-signatures"></a>オーバー ロードと署名

宣言領域内の同じ名前を持つエンティティの種類を宣言する唯一の方法は*オーバー ロード*します。 メソッド、演算子、インスタンス コンス トラクターとプロパティのみがオーバー ロードします。

オーバー ロードされた型のメンバーは、一意のシグネチャを持つ必要があります。 型のメンバーのシグネチャは、メンバーのパラメーターの型パラメーター、および数の数と種類で構成されます。 変換演算子には、シグネチャに、演算子の戻り値の型も含まれます。

次のメンバーのシグネチャの一部ではないとそのオーバー ロードできませんで。

* 修飾子を型のメンバー (たとえば、`Shared`または`Private`)。

* パラメーター修飾子 (たとえば、`ByVal`または`ByRef`)。

* パラメーターの名前。

* メソッドまたは (変換演算子) を除く演算子またはプロパティの要素の型の戻り値の型。

* 型パラメーターに対する制約。

次の例では、一連のオーバー ロードされたメソッドの宣言とそのシグネチャを示します。 この宣言は、同じシグネチャがあるいくつかのメソッド宣言のため、有効でないです。

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

指定された型引数に基づいて、同じシグネチャを持つメンバーを含む可能性のあるジェネリック型定義に有効です。 オーバー ロード解決規則は、することはできませんを明確にする場合がありますが、このようなオーバー ロードでは、あいまいさを解消してみてくださいに使用されます。 例えば:

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

*スコープ*のエンティティの名前は修飾なしには、その名前を参照することを内のすべての宣言スペースのセット。 一般に、エンティティの名前のスコープには、宣言全体のコンテキストただし、エンティティの宣言では、同じ名前のエンティティの入れ子になった宣言を含めることができます。 その場合、入れ子になった entity *shadows*、または非表示に、外部のエンティティ、およびシャドウされたエンティティへのアクセスが修飾を通してのみです。

入れ子によるシャドウするには、名前空間または名前空間内で入れ子になった型で、他の種類内で入れ子にされた型と型のメンバーの本文に発生します。 宣言の入れ子は常にによるシャドウ; 暗黙的に発生します明示的な構文は必要ありません。

次の例では、内で、`F`メソッドは、インスタンス変数`i`ローカル変数でシャドウが`i`が内、`G`メソッド、`i`インスタンス変数を参照します。

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

内側のスコープに名前が外側のスコープでの名前を非表示にする場合は、その名前のすべてのオーバー ロードされた出現箇所をシャドウします。 次の例では、呼び出し`F(1)`呼び出す、`F`で宣言されている`Inner`ため、外部出現するすべての`F`内部宣言では表示されません。 同じ理由、呼び出しから`F("Hello")`エラーが発生します。

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

継承リレーションシップは 1 種類のいずれか (、*派生*型) から別の派生 (、*基本*型)、派生型の宣言領域を暗黙的に含む、アクセスできるように、コンス トラクターではない型のメンバーとその基本型の入れ子にされた型。 次の例では、クラス`A`の基本クラスは、 `B`、および`B`から派生`A`します。

```vb
Class A
End Class

Class B
    Inherits A
End Class
```

`A`は基底クラスを明示的に指定する、その基底クラスが暗黙的に`Object`します。

以下は、継承の重要な側面です。

* 継承は推移的です。 場合型*C*型から派生*B*、および種類*B*型から派生*A*、型*C*継承、入力の型で宣言されたメンバー *B*型で宣言された型のメンバーも*A*します。

* 派生型では、拡張しますが、絞り込むことはできません、その基本型。 派生型が型の新しいメンバーを追加できますと継承した型のメンバーをシャドウすることができますが、継承した型のメンバーの定義を削除できません。

* 型のインスタンスには、その基本型の型のメンバーのすべてが含まれている、ためには、派生型から変換を常にその基本型が存在します。

* すべての種類は、基本型を型を除くを設定する必要があります`Object`します。 したがって、`Object`は、すべての型の究極の基本型ですし、すべての型に変換できます。

* 派生での循環は許可されていません。 つまり、ときに、型`B`型から派生`A`、エラーの種類には`A`型から直接または間接的に派生する`B`します。

* 型が直接または間接的にから派生していません内に入れ子にされた型。

次の例では、クラスは、互いに循環依存しているために、コンパイル時エラーが生成されます。

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

次の例は、ためも、コンパイル時エラーを生成`B`、入れ子になったクラスから直接派生`C`クラスを通じて`A`します。

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

に、次の例でエラーが発生しないクラス`A`クラスから派生していない`B`します。

```vb
Class A
    Class B
        Inherits A
    End Class 
End Class
```

### <a name="mustinherit-and-notinheritable-classes"></a>MustInherit と NotInheritable クラス

A`MustInherit`クラスが基本型としてのみ機能する不完全な型。 A`MustInherit`クラス、インスタンス化できないため、使用するとエラー、`New`演算子に対して 1 つ。 変数を宣言することは`MustInherit`クラスです。 このような変数を割り当てることできますのみ`Nothing`またはから派生したクラスのある値、`MustInherit`クラス。

通常のクラスを派生する場合、`MustInherit`クラスでは、通常のクラスをオーバーライドする必要がありますすべて継承された`MustOverride`メンバー。 例えば:

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

`MustInherit`クラス`A`が導入されています、`MustOverride`メソッド`F`します。 クラス`B`その他の方法が導入されました`G`の実装は提供されません`F`します。 クラス`B`も宣言する必要がありますので`MustInherit`します。 クラス`C`オーバーライド`F`し、実際の実装を提供します。 いいえ未処理があるので`MustOverride`クラスでメンバー `C`、する必要はありません`MustInherit`します。

A`NotInheritable`クラスは、クラスが別のクラスを派生できません。 `NotInheritable` クラスは、意図しない派生を防ぐために主に使用されます。

この例では、クラス`B`しようとするから派生するために、エラーでは、`NotInheritable`クラス`A`します。 両方のクラスをマークできません`MustInherit`と`NotInheritable`します。

```vb
NotInheritable Class A
End Class

Class B
    ' Error, a class cannot derive from a NotInheritable class.
    Inherits A
End Class
```

### <a name="interfaces-and-multiple-inheritance"></a>インターフェイスと多重継承

単一の基本型からのみ派生するには、他の型とは異なり、インターフェイスは、複数の基底インターフェイスから派生可能性があります。 このため、インターフェイスは、別の基底インターフェイスから、同じ名前を持つ型のメンバーを継承できます。 このような場合は、派生インターフェイスでは多重継承の名前は利用できず、派生インターフェイスを通じてこれらの型メンバーのいずれかを参照するによって署名やオーバー ロードに関係なく、コンパイル時エラーが発生します。 代わりに、競合する型のメンバーは、基底インターフェイス名を使用して参照する必要があります。

ため、次の例では、最初の 2 つのステートメントがコンパイル時エラーを発生多重継承メンバー`Count`インターフェイスでは利用できません`IListCounter`:

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

キャストすることで、あいまいさが解決されるように、例に示すように、`x`基底インターフェイスの適切な型にします。 このようなキャストが実行時のコストはあるありません。単なる階層の派生型としてコンパイル時に、インスタンスを表示するので構成されます。

1 つの型のメンバーが複数のパスを通じて同じ基底インターフェイスから継承されると、型のメンバーは 1 回のみ継承がいるかのように扱われます。 つまり、派生インターフェイスには、特定の基底インターフェイスから継承された各型のメンバーの 1 つのインスタンスにはのみが含まれます。 例えば:

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

型メンバーの名前が継承階層の 1 つのパスでシャドウされている場合は、すべてのパスに名前が影付き。 次の例では、`IBase.F`によってメンバーが影付き、`ILeft.F`のメンバーでシャドウはありませんが、 `IRight`:

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

呼び出し`d.F(1)`選択`ILeft.F`場合でも、`IBase.F`を通じて潜在顧客のアクセス パスでシャドウされませんが、`IRight`します。 からのアクセス パス`IDerived`に`ILeft`に`IBase`shadows`IBase.F`からのアクセス パスで、メンバーが影付きも`IDerived`に`IRight`に`IBase`します。

### <a name="shadowing"></a>シャドウ

派生型では、再宣言することにより、継承された型のメンバーの名前をシャドウします。 名前をシャドウします。 その名前を持つ継承された型のメンバーは削除されません。単に、その名前を持つ継承された型のメンバーの派生クラスでは使用できません。 シャドウの宣言には、任意の種類のエンティティの可能性があります。

オーバー ロード可能なエンティティには、シャドウの 2 つの形式のいずれかを選択できます。 *名前によるシャドウ*を使用して指定は、`Shadows`キーワード。 名前でシャドウするエンティティ名ですべてのオーバー ロードを含む、基本クラスに非表示にします。 *名前とシグネチャによるシャドウ*を使用して指定は、`Overloads`キーワード。 名前とシグネチャでシャドウするエンティティは、その名前のエンティティとして同じシグネチャを持つすべてを隠ぺいします。 例えば:

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

持つメソッドをシャドウする`ParamArray`個別の署名、展開された署名のないすべての可能な引数を名前とシグネチャを非表示にします。 これは、シャドウ メソッドのシグネチャが展開されていないシャドウされたメソッドのシグネチャと一致する場合でも当てはまります。 次の例では:

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

印刷`Base`場合でも、`Derived.F`の展開されていない形式と同じシグネチャを持つ`Base.F`します。

逆に、使用してメソッドを`ParamArray`引数のみ可能なすべての拡張されたシグネチャ、同じシグネチャを持つメソッドをシャドウします。 次の例では:

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

印刷`Base`場合でも、`Derived.F`として同じシグネチャを持つ拡張の形式が`Base.F`します。

シャドウ メソッドまたはプロパティが指定されていない`Shadows`または`Overloads`前提としています`Overloads`メソッドまたはプロパティが宣言されている場合`Overrides`、`Shadows`それ以外の場合。 一連のオーバー ロードされたエンティティの 1 つのメンバーを指定しているかどうか、`Shadows`または`Overloads`キーワードをすべて指定する必要あります。 `Shadows`と`Overloads`キーワードを同時に指定することはできません。 どちらも`Shadows`も`Overloads`標準モジュールで指定することができます。 標準的なモジュールのメンバーが暗黙的にから継承されたメンバーをシャドウ`Object`します。

インターフェイスの継承によって多重継承された (および、それによって使用できない)、型のメンバーの名前をシャドウすることはそのため、名前使用できるようにする派生インターフェイスでします。

例えば:

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

シャドウの継承されたメソッドには、メソッドが許可されている、ため、クラスにいくつか含まれてはことが`Overridable`同じシグネチャを持つメソッド。 これはありません、あいまいさの問題、最も多く派生されたメソッドのみが表示されるためです。 次の例では、`C`と`D`クラスを含む 2 つ`Overridable`同じシグネチャを持つメソッド。

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

2 つ`Overridable`メソッド: クラスによって導入された 1 つ`A`クラスによって導入された、1 つ`C`します。 クラスによって導入されたメソッド`C`クラスから継承されたメソッドを非表示に`A`します。 つまり、`Overrides`クラスの宣言で`D`クラスによって導入されたメソッドをオーバーライド`C`、クラスのことはできませんと`D`クラスによって導入されたメソッドをオーバーライドする`A`します。 この例では、出力が生成されます。

```
B.F
B.F
D.F
D.F
```

非表示を起動することは`Overridable`クラスのインスタンスにアクセスしてメソッド`D`メソッドが非表示しない階層の派生型を使って。

シャドウすることはできません、`MustOverride`メソッド、ほとんどの場合そうクラスは、使用できないためです。 例えば:

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

ここでは、クラス`MoreDerived`をオーバーライドするために必要な`MustOverride`メソッド`Base.F`、だクラス`Derived`shadows `Base.F`、このことはできません。 有効なを宣言する方法はありませんの子孫`Derived`します。

外側のスコープから名前をシャドウ処理とは対照的継承したスコープからをアクセス可能な名前をシャドウすると、次の例のように、報告されることを警告が発生します。

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

メソッドの宣言`F`クラスで`Derived`により警告が報告されます。 継承された名前をシャドウ具体的にはないエラーが、基底クラスの個別の進化を来たすためです。 たとえば、上記のような状況に発生していますのでクラスの以降のバージョン`Base`メソッドを導入`F`クラスの以前のバージョンにするはありません。 上記のような状況で、エラーをされていた*任意*派生クラスを無効になりますが、個別にバージョン管理されたクラス ライブラリで基底クラスに加えられた変更可能性です。

使用して、継承された名前をシャドウ処理に起因する警告を取り除くことができます、`Shadows`または`Overloads`修飾子。

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

`Shadows`修飾子は、継承されたメンバーをシャドウすることを指定します。 指定するとエラーには、`Shadows`または`Overloads`修飾子をシャドウする型のメンバー名がない場合。

新しいメンバーの宣言では、次の例のように、新しいメンバーのスコープ内でのみ、継承されたメンバーをシャドウします。

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

メソッドの宣言の前の例では`F`クラスで`Derived`メソッドをシャドウ`F`クラスから継承されている`Base`、以降、新しいメソッドが、`F`クラスで`Derived`が`Private`アクセス、そのスコープはクラスに拡張されません`MoreDerived`します。 したがって、呼び出し`F()`で`MoreDerived.G`が有効では呼び出す`Base.F`します。 をオーバー ロードされた型のメンバーの場合は、オーバー ロードされた型のメンバーのセット全体はシャドウのための最も制限の少ないアクセス権を持ってこれらはすべてかのように扱われます。

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

この例でもの宣言`F()`で`Derived`で宣言されて`Private`アクセス、オーバー ロードされた`F(Integer)`で宣言されて`Public`アクセスします。 名前をシャドウするために、そのため、`F`で`Derived`だとして扱われます`Public`、ため両方のメソッドのシャドウ`F`で`Base`します。

## <a name="implementation"></a>実装

*実装*インターフェイスを実装して、型がインターフェイスのすべての型のメンバーを実装する型で宣言されている場合、リレーションシップが存在します。 特定のインターフェイスを実装する型は、そのインターフェイスに変換できます。 インターフェイスをインスタンス化することはできませんが、インターフェイスの変数を宣言することはこのような変数はできるインターフェイスを実装するクラスのある値のみ割り当てること。 例えば:

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

多重継承の種類のメンバーを持つインターフェイスを実装する型は、実装される派生インターフェイスから直接アクセスできない場合でも、これらのメソッドを実装もする必要があります。 例えば:

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

でも`MustInherit`クラスで実装されたインターフェイスのすべてのメンバーの実装を提供する必要があります。 これらのメソッドの実装を延期として宣言することで、ただし、`MustOverride`します。 例えば:

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

型は、その基本型を実装するインターフェイスを再実装するために選択できます。 インターフェイスを再実装するには、型する必要があります明示的にインターフェイスを実装すること。 インターフェイスを再実装する型が一部のみを再実装を選択できますがない再実装されるメンバー - インターフェイスのメンバーの基本型の実装を使用します。 例えば:

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

この例が出力されます。

```
TestDerived.DerivedTest1
TestBase.Test2
```

派生型では、基本インターフェイスが派生型の基本型によって実装されるインターフェイスを実装するときにのみ、基本型で既に実装されていないインターフェイスの型のメンバーを実装する派生型を選択できます。 例えば:

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

基本データ型のオーバーライド可能なメソッドを使用してインターフェイス メソッドを実装することもできます。 その場合は、派生型がオーバーライド可能なメソッドも、およびインターフェイスの実装を変更する可能性があります。 例えば:

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

### <a name="implementing-methods"></a>メソッドを実装します。

型*実装*メソッドを指定することによって実装されたインターフェイスの型のメンバー、`Implements`句。 2 つの型のメンバーは同じ数のパラメーターが必要、すべての型およびパラメーターの修飾子が一致している省略可能なパラメーターの既定値を含む、戻り値の型が一致する必要があります、およびすべてのメソッド パラメーターに対する制約と一致する必要があります。 例えば:

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

1 つのメソッドは、上記の条件を満たす場合、任意の数のインターフェイス型のメンバーを実装できます。 例えば:

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

メソッドをジェネリック インターフェイスを実装する場合、メソッドの実装は、インターフェイスの型パラメーターに対応する型の引数を指定する必要があります。 例えば:

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

これは、ジェネリック インターフェイスがいくつかの一連の型引数に実装可能ないない可能性がありますに注意してください。

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

*ポリモーフィズム*メソッドまたはプロパティの実装を変更する機能を提供します。 ポリモーフィズムと同じメソッドまたはプロパティを呼び出すインスタンスの実行時の型に応じて異なるアクションを実行できます。 メソッドまたはポリモーフィックなプロパティが呼び出されます*オーバーライド可能な*します。 これに対し、オーバーライドできないメソッドまたはプロパティの実装が不変です。メソッドまたはプロパティがクラスのインスタンスで呼び出されるかどうか、実装は同じ宣言されているか、派生クラスのインスタンス。 オーバーライドできないメソッドまたはプロパティが呼び出されると、インスタンスのコンパイル時の型が決定要因にします。 例えば:

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

オーバーライド可能なメソッドがありますも`MustOverride`、つまり、メソッド本体を提供しないとオーバーライドする必要があります。 `MustOverride` メソッドでのみ許可`MustInherit`クラス。

次の例では、クラスで`Shape`自体を描画できる幾何学的な図形オブジェクトの抽象概念を定義します。

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

`Paint`メソッドは`MustOverride`意味のある既定の実装がないためです。 `Ellipse`と`Box`クラスは具象`Shape`実装します。 これらのクラスがないため、 `MustInherit`、オーバーライドする必要が、`Paint`メソッドと、実際の実装を提供します。

エラーを参照するベースのアクセスには、`MustOverride`メソッドでは、次の例として示します。

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

エラーは、`MyBase.F()`呼び出し参照しているため、`MustOverride`メソッド。

### <a name="overriding-methods"></a>メソッドのオーバーライド

型が*オーバーライド*、継承されたオーバーライド可能なメソッドを使用して宣言をマークすること、同じ名前と、シグネチャを持つメソッドを宣言することによって、`Overrides`修飾子。以下に示すメソッドのオーバーライドの追加要件があります。 一方、`Overridable`メソッドの宣言には、新しいメソッドが導入されています、`Overrides`メソッドの宣言には、継承されたメソッドの実装が置き換えられます。

オーバーライドするメソッドを宣言することも`NotOverridable`、派生型でメソッドのさらにオーバーライドを防ぐことができます。 実際には、`NotOverridable`派生クラスのメソッドのオーバーライドをこれ以上になります。

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

クラスの例では、`B`は、2`Overrides`メソッド: メソッド`F`を持つ、`NotOverridable`修飾子とメソッド`G`をしません。 使用、`NotOverridable`修飾子がクラス`C`からさらにメソッドをオーバーライドする`F`します。

オーバーライドするメソッドを宣言することも`MustOverride`はオーバーライドするメソッドが宣言されていない場合でも、`MustOverride`します。 外側のクラスを宣言する必要があります`MustInherit`さらに派生した任意のクラスが宣言されていないと`MustInherit`メソッドをオーバーライドする必要があります。 例えば:

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

クラスの例では、`B`オーバーライド`A.F`で、`MustOverride`メソッド。 派生するクラス、つまり`B`をオーバーライドする必要があります`F`宣言されている場合を除き、`MustInherit`もします。

コンパイル時エラーは、次のすべてがオーバーライドするメソッドの場合は true でない場合に発生します。

* 宣言コンテキストには、オーバーライドするメソッドとしてアクセス可能な継承されたメソッドと同じシグネチャと戻り値の型 (存在する場合) が含まれています。
* 無効にされている継承されたメソッドはオーバーライド可能です。 つまり、オーバーライドされている継承されたメソッドが`Shared`または`NotOverridable`します。
* 宣言されているメソッドのアクセシビリティ ドメインは、無効にされている継承されたメソッドのアクセシビリティ ドメインと同じです。 1 つの例外:`Protected Friend`でメソッドをオーバーライドする必要があります、`Protected`メソッドの場合、もう一方のメソッドをオーバーライドするメソッドがない別のアセンブリ内`Friend`へのアクセスします。
* オーバーライドするメソッドのパラメーターに一致の使用状況において、オーバーライドされたメソッドのパラメーター、 `ByVal`、 `ByRef`、`ParamArray,`と`Optional`省略可能なパラメーターの値を含む修飾子。
* オーバーライド元のメソッドの型パラメーターでは、制約の型において、オーバーライドされたメソッドの型パラメーターと一致します。

基本のジェネリック型のメソッドをオーバーライドするときにオーバーライドするメソッドは基本データ型のパラメーターに対応する型引数を指定する必要があります。 例えば:

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

これは、ジェネリック クラスでオーバーライド可能なメソッドが型引数のいくつかの設定をオーバーライドすることがない可能性がありますに注意してください。 メソッドが宣言されている場合`MustOverride`、つまり、あるいくつかの継承チェーンが使用できない場合があります。 例えば:

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

上書きの宣言は、次の例のように、基本のアクセスを使用して、オーバーライドされる基本メソッドにアクセスできます。

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

呼び出しの例で`MyBase.PrintVariables()`クラスで`Derived`呼び出す、`PrintVariables`メソッドがクラスで宣言された`Base`します。 基本アクセスでは、オーバーライド可能な呼び出しの機構を無効にし、単に基本のメソッドをオーバーライドできないメソッドとして扱われます。 呼び出す必要がある`Derived`されて書き込まれる`CType(Me, Base).PrintVariables()`、再帰的に存在するかを呼び出す、`PrintVariables`メソッドで宣言`Derived`で宣言されている 1 つではない`Base`します。

のみが含まれる場合、`Overrides`修飾子は、メソッドは、別のメソッドをオーバーライドします。 その他のすべてのケースでは、継承されたメソッドとして同じシグネチャを持つメソッドは単に次の例のように、継承されたメソッドをシャドウします。

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

メソッドの例で`F`クラスで`Derived`は含まれません、`Overrides`修飾子とメソッドをオーバーライドしません`F`クラスで`Base`します。 メソッドではなく、`F`クラスで`Derived`クラスのメソッドをシャドウ`Base`、宣言が含まれていないために、警告が報告されると、`Shadows`または`Overloads`修飾子。

次の例では、メソッドで`F`クラスで`Derived`オーバーライド可能なメソッドをシャドウ`F`クラスから継承された`Base`:

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

新しいメソッドから`F`クラスで`Derived`が`Private`アクセスにはのみが含まれますのクラス本体にはスコープには`Derived`クラスには拡張されず`MoreDerived`します。 メソッドの宣言`F`クラスで`MoreDerived`メソッドをオーバーライドするために許可されて`F`クラスから継承された`Base`します。

ときに、`Overridable`メソッドが呼び出される場合、基底クラスまたは派生クラスでメソッド呼び出しではかどうかに関係なく、インスタンスの型に基づいて、インスタンス メソッドの最多派生実装が呼び出されます。 最派生実装、`Overridable`メソッド`M`クラスに関して`R`は次のように決定されます。

* 場合`R`、導入が含まれています。`Overridable`の宣言`M`、これは、最派生実装の`M`します。

* の場合`R`のオーバーライドを含む`M`、これは、最派生実装の`M`します。

* それ以外の場合の実装の派生を最大限に`M`との直接基底クラスの同じ`R`します。

## <a name="accessibility"></a>ユーザー補助

宣言を指定します、*アクセシビリティ*のエンティティを宣言します。 エンティティのユーザー補助機能では、エンティティの名前のスコープは変更されません。 *アクセシビリティ ドメイン*の宣言をすべての宣言空間の宣言されたエンティティがアクセス可能なセット。

5 つのアクセスの種類は`Public`、 `Protected`、 `Friend`、 `Protected Friend`、および`Private`します。 `Public` 最も制限の少ないアクセスの種類とその他の 4 つの型はすべてのサブセットの`Public`します。 最も制限の多いアクセスの種類は`Private`、およびその他の 4 つのアクセス型はのすべてのスーパセットです`Private`します。

```antlr
AccessModifier
    : 'Public'
    | 'Protected'
    | 'Friend'
    | 'Private'
    | 'Protected' 'Friend'
    ;
```

可能性がある、省略可能なアクセス修飾子で宣言用のアクセスの種類を指定した`Public`、 `Protected`、 `Friend`、 `Private`、または組み合わせた`Protected`と`Friend`します。 既定のアクセスの種類によって異なります、宣言のコンテキストへのアクセス修飾子が指定されていない場合許可されているアクセスの種類は、宣言コンテキストにも依存します。

* 宣言されたエンティティ、`Public`修飾子が`Public`アクセスします。 使用に関する制約がない`Public`エンティティ。

* 宣言されたエンティティ、`Protected`修飾子が`Protected`アクセスします。 `Protected` アクセスは、クラス (正規型のメンバーと入れ子になったクラスの両方) のまたはのメンバーでのみ指定できます`Overridable`標準的なモジュールと構造体のメンバー (から継承する必要があります、定義上、`System.Object`または`System.ValueType`)。 A`Protected`メンバーは、そのメンバーは、インスタンス メンバーはありません。 または、派生クラスのインスタンスを通して、アクセスが行わ、派生クラスにアクセスします。 `Protected` アクセスのスーパー セットでない`Friend`アクセスします。

* 宣言されたエンティティ、`Friend`修飾子が`Friend`アクセスします。 持つエンティティ`Friend`アクセスは、エンティティ宣言または指定されているすべてのアセンブリを含むプログラム内でのみアクセス可能な`Friend`経由でアクセス、`System.Runtime.CompilerServices.InternalsVisibleToAttribute`属性。

* 宣言されたエンティティ、`Protected Friend`修飾子の和集合がある`Protected`と`Friend`アクセスします。

* 宣言されたエンティティ、`Private`修飾子が`Private`アクセスします。 A`Private`エンティティは、入れ子になったエンティティを含め、その宣言コンテキスト内でのみアクセスできます。

宣言内のアクセシビリティは宣言コンテキストのアクセシビリティに依存しません。 宣言された型の例では、`Private`へのアクセスを持つ型のメンバーを含めることができます`Public`アクセスします。

次のコードでは、さまざまなアクセシビリティ ドメインを示します。

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

クラスとメンバーがこの例では、次のアクセシビリティ ドメインがあります。

* アクセシビリティ ドメイン`A`と`A.X`に制限はありません。

* アクセシビリティ ドメイン`A.Y`、 `B`、 `B.X`、 `B.Y`、 `B.C`、 `B.C.X`、および`B.C.Y`は含んでいるプログラムです。

* アクセシビリティ ドメイン`A.Z`は `A.`

* アクセシビリティ ドメイン`B.Z`、 `B.D`、 `B.D.X`、および`B.D.Y`は`B`など、`B.C`と`B.D`します。

* アクセシビリティ ドメイン`B.C.Z`は`B.C`します。

* アクセシビリティ ドメイン`B.D.Z`は`B.D`します。

例に示すように、メンバーのアクセシビリティ ドメインを含んでいる型よりも大きいことはありません。 たとえば、場合でもすべて`X`メンバーが`Public`宣言されたアクセシビリティ、以外のすべて`A.X`アクセシビリティ ドメインは、それを含む型によって制限されます。

アクセスを`Protected`がプロテクト メンバーのインスタンスの関係のない型は互いにアクセスできないように、派生型のインスタンスでメンバーがある必要があります。 例えば:

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

上記の例では、クラスで`Guest`だけに、保護されたアクセス`Password`フィールドのインスタンスで修飾される場合`Guest`します。 これにより、`Guest`へアクセスできない、`Password`のフィールド、`Employee`オブジェクトにキャストするだけで`User`します。

目的で`Protected`メンバー アクセス宣言のコンテキストにはで、ジェネリック型には型パラメーターが含まれています。 つまり、型引数の 1 つのセットを持つ派生型にへのアクセスがないこと、`Protected`異なる一連の型引数を持つ派生型のメンバー。 例えば:

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

__注意してください。__ C# 言語 (と可能性があるその他の言語) にアクセスするジェネリック型は、`Protected`にかかわらず、どのような型引数が指定されたメンバー。 これを含むジェネリック クラスを設計する際に注意してくださいで維持するか`Protected`メンバー。


### <a name="constituent-types"></a>構成要素型

*構成要素型*の宣言は、宣言によって参照される型。 たとえば、定数、メソッドの戻り値の型コンス トラクターのパラメーターの型の型は、すべての構成要素の型です。 宣言の構成要素の型のアクセシビリティ ドメインは、宣言自体のアクセシビリティ ドメインのスーパー セットであるかと同じである必要があります。 例えば:

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

## <a name="type-and-namespace-names"></a>型と Namespace 名

多くの言語構成要素は、名前空間または型指定が必要です。これらは、名前空間または型の名前の修飾形式を使用して指定できます。 A*修飾名*から成るピリオドで区切られた一連の識別子。 期間の左側にある、識別子によって指定された宣言領域の右側にある期間の識別子を解決します。

*完全修飾名*名前空間または型のすべての名前を含む修飾名を含む名前空間と型。 名前空間または型の完全修飾名は、つまり、`N.T`ここで、`T`エンティティの名前を指定し、`N`はそれを含むエンティティの完全修飾名です。

次の例では、行のコメントで、関連付けられている完全修飾名と名前空間と型の宣言がいくつかを示します。

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

名前空間 X.Y は、ソース コード内の 2 つの異なる場所で宣言されていますが、これら 2 つの部分宣言がクラス D および E. クラスの両方を含む X.Y と呼ばれる単一の名前空間だけを構成することを確認します。

場合によっては、修飾名がキーワードを使用して開始可能性があります`Global`します。 キーワードは、これは、宣言が外側の名前空間をシャドウする場合に便利です無名の最も外側にある名前空間を表します。 `Global`キーワードは、そのような状況で最も外側の名前空間を"エスケープ"処理を使用します。 例えば:

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

上記の例でも最初のメソッド呼び出しが無効ですので識別子`System`、クラスにバインド`System`、名前空間ではない`System`します。 アクセスする唯一の方法、`System`名前空間は、使用する`Global`最も外側の名前空間をエスケープします。 `Global` 使用することはできません、`Imports`ステートメントまたは`Namespace`宣言します。

他の言語には、型と言語のキーワードに一致する名前空間を生じる可能性があります、ために、Visual Basic は、一定期間に従っていれば、修飾名の一部であるキーワードを認識します。 この方法で使用されるキーワードは、識別子として扱われます。 修飾された識別子など、`X.Default.Class`は有効な修飾された識別子では、中に`Default.Class`はありません。

### <a name="qualified-name-resolution-for-namespaces-and-types"></a>名前空間と型の修飾の名前解決

フォームの修飾の名前空間または型の名前を指定`N.R(Of A)`ここで、`R`は修飾名の右端の識別子と`A`は省略可能な型引数リストでは、次の手順は、どの名前空間を決定する方法を説明または修飾名は参照の種類:

1. 解決`N`のいずれかの修飾付きまたは修飾されていない名前解決規則を使用します。

2. 場合の解決`N`失敗した場合、または、コンパイル時エラーが発生した、型パラメーターに解決されます。

3. の場合`R`N と型引数なしで名前空間の名前が指定された一致または`R`でアクセス可能な型と一致する`N`型引数として型パラメーターの同じ番号、存在する場合、修飾名を参照その名前空間または型。

4. の場合`N`1 つまたは複数の標準的なモジュールが含まれていますと`R`をいずれか 1 つの標準のモジュール修飾名で参照された場合、型引数として型パラメーターの同じ番号を持つアクセス可能な型の名前に一致入力します。 場合`R`いずれかで 1 つ以上の標準のモジュール、コンパイル時エラーが発生した場合、型引数として型パラメーターの同じ番号を持つアクセス可能な型の名前に一致します。

5. それ以外の場合は、コンパイル時のエラーが発生します。

__注意してください。__ この解決プロセスの影響がある型のメンバーをシャドウしません名前空間または型名前空間または型の名前を解決するときにします。

### <a name="unqualified-name-resolution-for-namespaces-and-types"></a>名前空間と型の非修飾の名前解決

非修飾名を指定`R(Of A)`ここで、`A`は省略可能な型引数リストでは、次の手順を参照する名前空間または型修飾されていない名前を確認する方法を説明します。

1. R が現在のメソッドの型パラメーターの名前と一致する型引数が指定されていない場合は、非修飾名はその型パラメーターを参照します。

2.  それぞれの入れ子にされた型名の参照を格納している最も内側の型から開始し、最も外側に。
    1. 場合`R`非修飾名は、その型パラメーターを参照し、現在の型でない型の引数が指定された型パラメーターの名前に一致します。
    2. の場合`R`のアクセス可能な名前には、同じ数の型が入れ子になったの一致が存在する場合に、型の引数としてパラメーターを入力し、非修飾名は、その型を参照します。

3. 入れ子になった名前空間ごとに、最も内側の名前空間から開始し、最も外側の名前空間には、名前の参照を含みます。
    1. 場合`R`非修飾名がその入れ子になった名前空間を参照し、現在の名前空間となしの型引数リスト内の入れ子になった名前空間の名前が指定した一致。
    2. の場合`R`いずれかで現在の名前空間では、非修飾名がその型を表している場合、型引数として型パラメーターの同じ番号を持つアクセス可能な型の名前に一致します。
    3. それ以外の場合、名前空間には、1 つまたは複数のアクセス可能な標準的なモジュールが含まれている場合と`R`し、修飾されていない、ただ 1 つの標準的なモジュールに存在する場合、型引数として型パラメーターの同じ番号を持つアクセス可能な入れ子になった型の名前に一致名前は、その入れ子にされた型を参照します。 場合`R`いずれかで 1 つ以上の標準のモジュール、コンパイル時エラーが発生した場合、型引数として型パラメーターの同じ番号を持つアクセス可能な入れ子になった型の名前に一致します。

4. ソース ファイルが 1 つまたは複数のインポート エイリアス、および`R`非修飾名は、そのインポート エイリアスを参照し、それらのいずれかの名前に一致します。 型引数リストを指定すると、コンパイル時エラーが発生します。

5. 名前参照を含むソース ファイルの 1 つまたは複数のインポート: 場合
    1. 場合`R`いずれかで 1 つだけインポートは、非修飾名がその型を表している場合、型引数として型パラメーターの同じ番号を持つアクセス可能な型の名前に一致します。 場合`R`いずれか 1 つ以上のインポートとすべてがない場合、同じ型で、コンパイル時エラーが発生した型の引数として型パラメーターの同じ番号を持つアクセス可能な型の名前に一致します。
    2. それ以外の場合、型引数リストが指定されていない場合、`R`非修飾名は、その名前空間を参照し、1 つだけのインポートでアクセス可能な型で、名前空間の名前に一致します。 型引数リストが指定されていない場合、`R`一致するすべての 1 つ以上のインポートでアクセス可能な型と名前空間の名前は、同じ名前空間、コンパイル時エラーが発生します。
    3. それ以外の場合、インポートには、1 つまたは複数のアクセス可能な標準的なモジュールが含まれている場合と`R`し、1 つの標準のモジュールに存在する場合、型引数として型パラメーターの同じ番号を持つアクセス可能な入れ子になった型の名前、非修飾名と一致します。その型を参照します。 場合`R`いずれかで 1 つ以上の標準のモジュール、コンパイル時エラーが発生した場合、型引数として型パラメーターの同じ番号を持つアクセス可能な入れ子になった型の名前に一致します。

6. コンパイル環境に 1 つまたは複数のインポート エイリアスが定義されている場合と`R`非修飾名は、そのインポート エイリアスを参照し、それらのいずれかの名前に一致します。 型引数リストを指定すると、コンパイル時エラーが発生します。

7. 場合は、コンパイル環境では、1 つまたは複数のインポートを定義します。
    1. 場合`R`いずれかで 1 つだけインポートは、非修飾名がその型を表している場合、型引数として型パラメーターの同じ番号を持つアクセス可能な型の名前に一致します。 場合`R`いずれか 1 つ以上のインポート、コンパイル時エラーが発生した場合、型引数として型パラメーターの同じ番号を持つアクセス可能な型の名前に一致します。
    2. それ以外の場合、型引数リストが指定されていない場合、`R`非修飾名は、その名前空間を参照し、1 つだけのインポートでアクセス可能な型で、名前空間の名前に一致します。 型引数リストが指定されていない場合、`R`コンパイル時エラーが発生した 1 つ以上のインポートでアクセス可能な型と名前空間の名前に一致します。
    3. それ以外の場合、インポートには、1 つまたは複数のアクセス可能な標準的なモジュールが含まれている場合と`R`し、1 つの標準のモジュールに存在する場合、型引数として型パラメーターの同じ番号を持つアクセス可能な入れ子になった型の名前、非修飾名と一致します。その型を参照します。 場合`R`いずれかで 1 つ以上の標準のモジュール、コンパイル時エラーが発生した場合、型引数として型パラメーターの同じ番号を持つアクセス可能な入れ子になった型の名前に一致します。

8. それ以外の場合は、コンパイル時のエラーが発生します。

__注意してください。__ この解決プロセスの影響がある型のメンバーをシャドウしません名前空間または型名前空間または型の名前を解決するときにします。

通常、名前できます 1 回だけでは、特定の名前空間。 ただし、ため、複数の .NET アセンブリ間で名前空間を宣言することができます、2 つのアセンブリが同じ完全修飾名を持つ型を定義する状況を含めることは可能です。 その場合は、ソース ファイルの現在のセットで宣言された型は、外部 .NET アセンブリで宣言された型を優先します。 それ以外の場合、名前があいまいと名前のあいまいさを解消する方法はありません。

## <a name="variables"></a>変数

A*変数*記憶域の場所を表します。 すべての変数ができる値を指定する型の変数に格納します。 Visual Basic はタイプ セーフな言語、プログラムのすべての変数に型があり、言語に値が格納されていることを保証するため、適切な型の変数は常に、します。 変数は、変数への参照を行う前に常にその型の既定値に初期化されます。 初期化されていないメモリにアクセスすることはできません。

## <a name="generic-types-and-methods"></a>ジェネリック型とメソッド

(標準的なモジュール型および列挙型) を除く型およびメソッドを宣言できます*パラメーター入力*、宣言されていない型のインスタンスまで提供される型である、またはメソッドが呼び出されます。 型と型パラメーターを持つメソッドとも呼ばれますが*ジェネリック型*と*ジェネリック メソッド*、それぞれ、型またはメソッドは、特定の知識のない一般的に、記述する必要がありますので、型またはメソッドを使用するコードによって指定される型。

__注意してください。__ この時点でメソッドとデリゲートがジェネリックであることができる場合でもプロパティ、イベント、および演算子することはできませんジェネリック自体。 外側のクラスからの型パラメーターが使用される可能性があります、ただし、します。

ジェネリック型またはメソッドの観点からは、型パラメーターは、型またはメソッドを使用する場合に、実際の型を使用して入力がプレース ホルダーの型です。 型または型またはメソッドを使用する時点でメソッドの型パラメーターに対して型引数が置き換えられます。 たとえば、としてジェネリック スタック クラスを実装できます。

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

使用して、宣言、`Stack(Of ItemType)`クラスは、型パラメーターに対して型引数を指定する必要があります`ItemType`します。 この型が入力し、どこにも`ItemType`クラス内で使用されます。

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

型またはメソッドの宣言では、型パラメーターを指定することがあります。 それぞれの型パラメーターでは、作成、構築された型またはメソッドに渡される型引数のプレース ホルダーである識別子です。 これに対し、型引数は、ジェネリック型またはメソッドを使用する場合、型パラメーターを置き換える実際の型です。

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

型またはメソッドの宣言で型パラメーターごとでは、その型またはメソッドの宣言領域の名前を定義します。 したがってが別の型パラメーター、型のメンバー、メソッドのパラメーターまたはローカル変数と同じ名前を含めることはできません。 型またはメソッドに型パラメーターのスコープは、型全体またはメソッドです。 型パラメーター、全体の型宣言のスコープは、ので、入れ子にされた型は、外側の型パラメーターを使用できます。 つまり型へのアクセス、ジェネリック型の内部で入れ子になったときに型パラメーターを指定常にする必要があります。

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

クラスの他のメンバーとは異なり、型パラメーターは継承されません。 簡単な名前を指定して型の型パラメーターを参照することができますのみつまり、親の型名で修飾することはできません。 不適切なプログラミング スタイルでは、入れ子にされた型の型パラメーター メンバーを非表示にしたり、外側の型で宣言されたパラメーターを入力します。

```vb
Class Outer(Of T)
    Class Inner(Of T)
        Public t1 As T    ' Refers to Inner's T
    End Class
End Class
```

型およびメソッドがオーバー ロードする型パラメーターの数に基づく (または*アリティ*) 型またはメソッドを宣言します。 たとえば、次の宣言は有効です。

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

型の場合、オーバー ロードは常に、指定された型引数の数と照らし合わせて照合します。 これは、機能は、同じプログラム内で、ジェネリックと非ジェネリック クラスを共に使用する場合に便利です。

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

メソッド オーバー ロードの解決に関するセクションでは、メソッド型パラメーターでオーバー ロードの規則がについて説明します。

コンテナーの宣言内で型パラメーターは完全な型と見なされます。 型パラメーターは、多くのさまざまな実際の型引数でインスタンス化することができます、ため若干異なる操作と次のように他の型よりも制限がある型パラメーター。

* 型パラメーターを使用して、直接基底クラスまたはインターフェイスを宣言することはできません。

* パラメーターは、存在する場合、制約とは異なります。 型のメンバー検索の規則は、型パラメーターに適用されます。

* 使用可能な変換型パラメーターは、存在する場合、制約に依存しているは、型パラメーターに適用されます。

* ない場合、`Structure`制約、型パラメーターによって表される型の値と比較できる`Nothing`を使用して`Is`と`IsNot`します。

* 型パラメーターでのみ使用できます、`New`式で型パラメーターの制約がある場合、`New`または`Structure`制約。

* 内で属性例外内で任意の場所は型パラメーターを使用することはできません、`GetType`式。

* 型パラメーターは、その他のジェネリック型とパラメーターの型引数として使用できます。

次の例では、ジェネリック型を拡張するは、`Stack(Of ItemType)`クラス。

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

宣言がへの型引数を提供するときに`MyStack`、同じ型の引数に適用される`Stack`もします。

型パラメーター、型では、純粋なコンパイル時の構成要素があります。 実行時に、それぞれの型パラメーターは、ジェネリック宣言する型引数を指定することによって指定された実行時の型にバインドされます。 したがって、型パラメーターで宣言された変数の型は、実行時には、非ジェネリック型または構築された特定の型。 すべての型パラメーターを含む式とステートメントの実行時の実行は、そのパラメーターの型引数として渡された実際の型を使用します。


### <a name="type-constraints"></a>型の制約

型引数を指定できますが、任意の型、型システムであるため、ジェネリック型またはメソッドは型パラメーターに関してどのような想定をことはできません。 型パラメーターのメンバーが型のメンバーであると見なされるため、`Object`からすべての型を派生させるため、`Object`します。

ようなコレクションの場合`Stack(Of ItemType)`とジェネリック型はするが型引数として提供する型を想定する場合もありますが、この事実に特に重要な制限事項では、できない場合があります。 *制約入力*どの型が型パラメーターとして指定でき、ジェネリック型またはメソッドの詳細については、型パラメーターを想定することを許可するが制限されている型パラメーターに配置できます。

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

この例で、`DisposableStack(Of ItemType)`インターフェイスを実装する型のみをその型パラメーター制約`System.IDisposable`します。 実装できるため、`Dispose`キューのままにメソッドを任意のオブジェクトを破棄します。

型制約は特殊な制約のいずれかを指定する必要があります`Class`、 `Structure`、または`New`、型がありますまたは`T`場所。

* `T` クラス、インターフェイス、または型パラメーターです。

* `T` が `NotInheritable` ではありません。

* `T` 、のいずれかでがないか、次の特殊な種類のいずれかから継承された型: `System.Array`、 `System.Delegate`、 `System.MulticastDelegate`、 `System.Enum`、または`System.ValueType`します。

* `T` が `Object` ではありません。 すべての型から派生するので`Object`、このような制約は効果がありませんが、許可された場合。

* `T` ジェネリック型またはメソッドが宣言されていると、少なくともアクセス可能である必要があります。

型の制約を中かっこで囲むことで、1 つの型パラメーターの型の複数の制約を指定できます (`{}`). 特定の型パラメーターの 1 つだけの型制約はクラスであることができます。 結合するとエラーには、`Structure`クラスの名前付き制約を持つ特殊な制約、または`Class`特殊な制約。

```vb
Class ControlFactory(Of T As {Control, New})
    ...
End Class
```

型の制約には、含む型または含んでいる型の型パラメーターのいずれかを使用できます。 次の例では、制約は、指定された型引数が型引数として使用してジェネリック インターフェイスを実装する必要があります。

```vb
Class Sorter(Of V As IComparable(Of V))
    ...
End Class
```

特殊な制約`Class`任意の参照型に指定された型引数を制限します。

__注意してください。__ 特殊な制約`Class`インターフェイスによって満たすことができます。 構造体はインターフェイスを実装できます。 制約ではそのため、 `(Of T As U, U As Class)` "T"構造体で満足する可能性があります (これを満たさない、`Class`特殊な制約)、"u"を実装するインターフェイス (が満たしている、`Class`特殊な制約)。

特殊な制約`Structure`以外の値の型に指定された型引数の制約`System.Nullable(Of T)`します。

__注意してください。__ 構造体の制約は許可しない`System.Nullable(Of T)`を指定することはできませんように`System.Nullable(Of T)`自体への型引数として。

特殊な制約`New`として宣言できません。 指定された型引数、アクセス可能なパラメーターなしのコンス トラクターが必要する必要があります`MustInherit`します。 例えば:

```vb
Class Factory(Of T As New)
    Function CreateInstance() As T
        Return New T()
    End Function
End Class
```

クラス型制約は、指定された型引数する必要があります、として入力する、またはそれを継承するかが必要です。 インターフェイスの型制約は、指定された型引数がそのインターフェイスを実装する必要がありますが必要です。 型パラメーターの制約は、指定された型引数する必要がありますから派生または一致する型パラメーターに指定された境界内のすべてを実装している必要があります。 例えば:

```vb
Class List(Of T)
    Sub AddRange(Of S As T)(collection As IEnumerable(Of S))
        ...
    End Sub
End Class
```

この例では、型パラメーターで`S`で`AddRange`型パラメーターに制約される`T`の`List`します。 つまり、`List(Of Control)`制限は`AddRange`の入力パラメーターはかから継承する任意の型を`Control`します。

型パラメーター制約`Of S As T`が推移的にすべての追加することによって解決`T`の制約に`S`、その他の特殊な制約よりも (`Class`、 `Structure`、 `New`)。 エラーに循環制約があることになります (例: `Of S As T, T As S`)。 それ自体が、エラー、型パラメーター制約には、`Structure`制約。 制約を追加した後は、さまざまな特別な状況が発生する可能性があります。

* 複数のクラスの制約が存在しない場合、最派生クラスは、制約と見なされます。 1 つまたは複数のクラスの制約には、継承関係がない、制約が満たされていないと、エラーになります。

 * 型パラメーターを結合する場合、`Structure`クラスの名前付き制約を持つ特殊な制約、または`Class`特殊な制約は、エラーになります。 クラスの制約があります`NotInheritable`、この場合その制約の派生型は受け入れられませんされ、エラーになります。

種類がありますのいずれかまたは型が、次の特殊な型から継承: `System.Array`、 `System.Delegate`、 `System.MulticastDelegate`、 `System.Enum`、または`System.ValueType`します。 その場合、型のみ、またはそれから継承された型が受け入れられます。 これらの型のいずれかに制限されている型パラメーターのみで許可されている変換を使用できます、`DirectCast`演算子。 例えば:

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

さらに、上記リラクゼーションのいずれかの値の型を制約付きの型パラメーターは、その値の型で定義されているすべてのメソッドを呼び出すことはできません。 例えば:

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

制約、置換では、後に最終的に、配列型として、任意の配列の共変の型も許可されます。 例えば:

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

クラスまたはインターフェイス制約と型パラメーターは、そのクラスまたはインターフェイスの制約と同じメンバーを持つと見なされます。 型パラメーターに複数の制約がある場合、型パラメーターと見なされます、制約のすべてのメンバーの和集合。 1 つ以上の制約で同じ名前のメンバーがあるかどうかは、次の順序でメンバーを非表示: クラスの制約内のメンバーを非表示にするインターフェイスの制約は、内のメンバーを非表示に`System.ValueType`(場合`Structure`制約を指定)、内のメンバーが隠ぺいされます`Object`します。 1 つ以上のインターフェイス制約で同じ名前のメンバーが表示された場合、メンバーは (複数のインターフェイスの継承) のように使用できませんし、型パラメーターは、必要なインターフェイスにキャストする必要があります。 例えば:

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

型引数として型パラメーターを指定するとき、型パラメーターは、一致する型パラメーターの制約を満たす必要があります。

```vb
Class Base(Of T As Class)
End Class

Class Derived(Of V)
    ' Error: V does not satisfy the constraints of T
    Inherits Base(Of V)
End Class
```

制約付きの型パラメーターの値は、制約で指定された、インスタンス メソッドを含む、インスタンス メンバーへのアクセスに使用できます。

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

インターフェイスまたはデリゲートの型宣言の型パラメーターを指定できます必要に応じて、*分散修飾子*します。 分散修飾子のある型パラメーターは、型パラメーターがインターフェイスまたはデリゲート型で使用できますがバリアント互換型引数を持つ別のジェネリック型に変換するジェネリック インターフェイスまたはデリゲート型を許可する方法を制限します。 例えば:

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

分散修飾子を持つ型のパラメーターを持つジェネリック インターフェイスでは、いくつかの制限があります。

* パラメーター リストを指定するイベントの宣言を含めることはできません (ただし、カスタム イベント宣言またはイベントのデリゲート型で宣言が許可されている)。

* 入れ子になったクラス、構造体、または列挙型を含めることはできません。

__注意してください。__ これらの制限は、入れ子になったジェネリック型に暗黙的に型の親のジェネリック パラメーターをコピーするという事実が原因には。 入れ子になったクラス、構造体、または列挙型の場合は、これらの型、型パラメーターの分散修飾子ことはできません。 生成された入れ子になったデリゲート クラスの場合は、イベントは、パラメーター リストで宣言して、エラー、型で使用される表示されるときに混乱を招くことができます、`In`で位置 (パラメーターの型など) が実際に使用する`Out`位置 (つまり、イベントの種類)。

Out 修飾子で宣言された型パラメーターが*共変*します。 非公式には、共変の型パラメーターは出力位置 - つまり、インターフェイスまたはデリゲートの型から返される値--でのみ使用でき、入力の位置では使用できません。 型`T`と見なされます*有効 covariantly*場合。

* `T` クラス、構造体、または列挙型。

* `T` 非ジェネリック デリゲートまたはインターフェイス型です。

* `T` 要素型が covariantly 有効な配列型です。

* `T` 型パラメーターとして宣言されませんでしたが、`Out`パラメーターを入力します。

* `T` 構築されたインターフェイスまたはデリゲートの型は、`X(Of P1,...,Pn)`型引数を持つ`A1,...,An`ように。

  * 場合`Pi`し Out 型パラメーターとして宣言された`Ai`covariantly は有効です。

  * 場合`Pi`し In 型パラメーターとして宣言された`Ai`変に有効な機能です。

次の covariantly インターフェイスまたはデリゲート型で有効な必要があります。

* インターフェイスの基本インターフェイスです。

* 関数の戻り値の型またはデリゲートの型。

* 存在する場合のプロパティの型を`Get`アクセサー。

* いずれかの種類`ByRef`パラメーター。

例えば:

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

__注意してください。__ `Out` 予約語ではありません。

修飾子で宣言された型パラメーターが*反変*します。 非公式、反変の型パラメーターは入力の位置 - で、インターフェイスまたはデリゲート型に渡される値つまり--でのみ使用でき、出力位置では使用できません。 型`T`と見なされます*変に有効な機能*場合。

* `T` クラス、構造体、または列挙型。

* `T` 非ジェネリック デリゲートまたはインターフェイス型です。

* `T` 要素型を持つ変に有効な機能は、配列型です。

* `T` In 型パラメーターとして宣言されませんでしたが、型パラメーターです。

* `T` 構築されたインターフェイスまたはデリゲートの型は、`X(Of P1,...,Pn)`型引数を持つ`A1,...,An`ように。

  * 場合`Pi`として宣言されている、`Out`パラメーターを入力し、`Ai`変に有効な機能です。

  * 場合`Pi`として宣言されている、`In`パラメーターを入力し、 `Ai` covariantly は有効です。

次のインターフェイスまたはデリゲート型で有効な変に機能があります。

* パラメーターの型。

* 型の制約をメソッド型パラメーター。

* ある場合、プロパティの型を`Set`アクセサー。

* イベントの種類。

例えば:

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

場合は、型が有効なある変に機能と covariantly (両方を持つプロパティなど、`Get`と`Set`アクセサーまたは`ByRef`パラメーター)、バリアント型パラメーターを使用することはできません。


Co および反変性は、「ダイヤモンドあいまいさが問題」に上昇を付与します。 次のコードがあるとします。

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

クラスは、`C`に変換できる`IEnumerable(Of Object)`両方からの共変性の変換を 2 つの方法で`IEnumerable(Of String)`と共変性の変換から`IEnumerable(Of Exception)`します。 CLR でによって呼び出される 2 つの方法が指定されていない`c.GetEnumerator()`します。 共通のスーパー タイプを持つ 2 つの異なるジェネリック引数の共変のインターフェイスを実装するクラスが宣言されたときに一般に、(この場合の例:`String`と`Exception`共通のスーパー タイプがある`Object`)、またはするクラスが宣言されています。2 つの異なるジェネリック引数を持つ一般的なサブタイプの場合は、反変のインターフェイスを実装し、あいまいさが発生する可能性があります。 コンパイラは、このような宣言に関する警告を提供します。
