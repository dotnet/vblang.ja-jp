# <a name="conversions"></a>変換

変換は、別の 1 つの型から値を変更するプロセスです。 たとえば、型の値`Integer`型の値に変換できる`Double`、または型の値`Derived`型の値に変換できる`Base`されている場合、`Base`と`Derived`は、両方のクラスおよび`Derived`継承`Base`します。 変換 (例のように、後者)、変更するには値自体は必要としないまたは (前の例では) のように値の表現に大幅な変更を必要な場合があります。

変換がある拡張または縮小します。 A*拡大変換*型から値ドメインが以上の容量を元の型の値のドメインよりも大きいにない場合、別の型に変換します。 拡大変換が失敗しないはずです。 A*縮小変換*は、型から値ドメインがドメインの元の型よりも小さい値か、十分に関係のないその余分な注意が必要場合に別の型への変換で変換を行う (例では、変換するときに`Integer`に`String`)。 縮小変換で、情報の損失を伴う場合がありますは失敗します。

Id 変換 (つまりからへの変換、型自体) と既定値の変換 (つまりからの変換`Nothing`) すべての種類に対して定義されています。

## <a name="implicit-and-explicit-conversions"></a>暗黙の型変換と明示的な型変換

変換には、いずれかを指定できる*暗黙的な*または*明示的な*します。 任意の特殊な構文を使用しないで、暗黙的な変換が行われます。 暗黙的な変換の例を次に、`Integer`値を`Long`値。

```vb
Module Test
    Sub Main()
        Dim intValue As Integer = 123
        Dim longValue As Long = intValue

        Console.WriteLine(intValue & " = " & longValue)
    End Sub
End Module
```

明示的な変換には、その一方で、キャスト演算子が必要です。 キャスト演算子を使用せずに、明示的な変換を行うとすると、コンパイル時エラーが発生します。 次の例では、変換する明示的な変換を使用して、`Long`値を`Integer`値。

```vb
Module Test
    Sub Main()
        Dim longValue As Long = 134
        Dim intValue As Integer = CInt(longValue)

        Console.WriteLine(longValue & " = " & intValue)
    End Sub
End Module
```

暗黙的な変換のセットは、コンパイル環境によって異なります、`Option Strict`ステートメント。 厳密な型を使用しているのみ拡大変換が暗黙的に発生します。 制限の緩やかなセマンティクスを使用しているすべての拡大と縮小変換 (つまり、すべての変換) が暗黙的に発生します。

## <a name="boolean-conversions"></a>ブール型変換

`Boolean` 、数値型ではないが列挙型の場合と同様に、数値型との変換を縮小します。 リテラル`True`リテラルに変換`255`の`Byte`、`65535`の`UShort`、`4294967295`の`UInteger`、`18446744073709551615`の`ULong`と式に`-1`の`SByte`、 `Short`、 `Integer`、 `Long`、 `Decimal`、 `Single`、および`Double`します。 リテラル`False`リテラルに変換`0`します。 ゼロの数値リテラルに変換`False`します。 その他のすべての数値の値が、リテラルに変換`True`します。

ブール値から変換する文字列への縮小変換がある`System.Boolean.TrueString`または`System.Boolean.FalseString`します。 縮小変換も`String`に`Boolean`: 文字列が等しい場合`TrueString`または`FalseString`(現在のカルチャで大文字小文字) の適切な値が使用し、; として文字列を解析しようとするそれ以外の場合、数値型 (16 進数または 8 進数の場合に可能であれば、それ以外の場合、浮動小数点数と)、上記の規則を使用してそれ以外の場合にスロー`System.InvalidCastException`します。

## <a name="numeric-conversions"></a>数値変換

数値変換は、型の間存在`Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、 `ULong`、 `Long`、 `Decimal`、`Single`と`Double`、すべての種類を列挙します。 変換されるときに、列挙型は、場合、基になる型と同様に扱われます。 列挙型への変換、変換元の値では、列挙型で定義されている値のセットに準拠する必要はありません。 例えば:

```vb
Enum Values
    One
    Two
    Three
End Enum

Module Test
    Sub Main()
        Dim x As Integer = 5

        ' OK, even though there is no enumerated value for 5.
        Dim y As Values = CType(x, Values)
    End Sub
End Module
```

数値変換が実行時に、次のように処理されます。

* 数値型からより広い数値型への変換では、値は単に幅の広い型に変換します。 変換`UInteger`、 `Integer`、 `ULong`、 `Long`、または`Decimal`に`Single`または`Double`に丸められますが、最も近い`Single`または`Double`値。 この変換は、精度の損失を引き起こす可能性があります、その 1 桁の損失は発生しません。

* 整数型から別の整数型との間の変換の`Single`、 `Double`、または`Decimal`、整数型に結果がで整数のオーバーフロー チェックがかどうかが異なります。

  *整数のオーバーフローがチェックされる: 場合*

  * ソースが整数型の場合は、ソース引数が変換先の型の範囲内にある場合、変換が成功します。 変換がスローされます、`System.OverflowException`ソース引数が変換先の型の範囲外にある場合は例外です。

  * ソースの場合`Single`、 `Double`、または`Decimal`ソース値は、最も近い整数値の上下に丸められますが、この整数値の変換の結果になります。 ソース値が同じように 2 つの整数値に近い値は、最下位ビットが 0 である値に丸められます。 結果の整数値が変換先の型の範囲外の場合、`System.OverflowException`例外がスローされます。

  *整数のオーバーフローがチェックされない: 場合*

  * ソースが整数型の場合は、変換は常に成功し、だけで構成される元の値の最上位ビットを破棄します。

  * ソースの場合`Single`、 `Double`、または`Decimal`変換は常に成功し、元の値を最も近い整数値に丸め処理を行うだけで構成されます。 ソース値が同じように 2 つの整数値に近い場合、値は常に偶数の最下位ビットが 0 である値に丸められます。

* 変換の`Double`に`Single`、`Double`値に丸められます、最も近い`Single`値。 場合、`Double`として表す値が小さすぎる、`Single`結果が正のゼロまたは負のゼロ。 場合、`Double`として表す値が大きすぎて、`Single`結果は正の無限大または負の無限大になります。 場合、`Double`値は`NaN`、結果も`NaN`します。

* 変換の`Single`または`Double`に`Decimal`、ソース値に変換されます`Decimal`表現し、必要な場合、28 小数点後近似値に丸められます。 ソース値が小さすぎてとして表すかどうか、`Decimal`結果はゼロになります。 元の値が場合`NaN`、無限大、または大きすぎてとして表す、 `Decimal`、`System.OverflowException`例外がスローされます。

* 変換の`Double`に`Single`、`Double`値に丸められます、最も近い`Single`値。 場合、`Double`として表す値が小さすぎる、`Single`結果が正のゼロまたは負のゼロ。 場合、`Double`として表す値が大きすぎて、`Single`結果は正の無限大または負の無限大になります。 場合、`Double`値は`NaN`、結果も`NaN`します。

## <a name="reference-conversions"></a>参照変換

参照型は、基本型の場合は、その逆に変換可能性があります。 変換される値が null 値を派生型自体、またはより強い派生型の場合、基本型からより強い派生型への変換は実行時にのみ成功します。

クラスとインターフェイスの型は、任意のインターフェイス型との間にキャストできます。 関係する実際の型が継承または実装の関係がある場合、型とインターフェイスの型の間の変換は実行時にのみ成功します。 インターフェイス型がから派生した型のインスタンスを常に格納されるため`Object`との間、インターフェイス型をキャストすることができますも必ず`Object`します。

__注意してください。__ 変換するとエラーには、`NotInheritable`クラスと COM クラスを表すクラスのインターフェイスの実装まで不明な可能性があるためにを実装していないインターフェイスの実行時間。 

実行時に、参照変換が失敗した場合、`System.InvalidCastException`例外がスローされます。

### <a name="reference-variance-conversions"></a>参照の差異の変換

ジェネリック インターフェイスまたはデリゲートには、互換性のあるバリアント型の間の変換を許可するバリアント型パラメーターがある場合があります。 そのため、実行時に、クラス型またはインターフェイス型からバリアント インターフェイス型から継承または実装と互換性のあるインターフェイス型への変換は成功します。 同様に、デリゲート型、および互換性のあるバリアントからデリゲートの型にキャストすることができます。 たとえば、デリゲート型

```vb
Delegate Function F(Of In A, Out R)(a As A) As R
```

変換を許可するよう`F(Of Object, Integer)`に`F(Of String, Integer)`します。 つまり、デリゲート`F`受け取り`Object`デリゲートとして安全に使用できる可能性があります`F`受け取り`String`します。 デリゲートが呼び出されたときに、対象のメソッドは、オブジェクトが想定され、文字列は、オブジェクト。

ジェネリック デリゲートまたはインターフェイス型`S(Of S1,...,Sn)`言います*バリアント型の互換性のある*ジェネリック インターフェイスまたはデリゲート型で`T(Of T1,...,Tn)`場合。

* `S` `T`同じジェネリック型から構築された両方`U(Of U1,...,Un)`します。

* 型パラメーターごと`Ux`:

  * なし、分散型のパラメーターで宣言された場合`Sx`と`Tx`同じ型でなければなりません。

  * 型パラメーターが宣言されていた場合`In`、拡大 id、既定値、参照、配列、または型がある必要がありますし、パラメーターの変換から`Sx`に`Tx`します。

  * 型パラメーターが宣言されていた場合`Out`、拡大 id、既定値、参照、配列、または型がある必要がありますし、パラメーターの変換から`Tx`に`Sx`します。

クラスは、バリアント 1 つ以上の互換性のあるインターフェイスを実装している場合は、バリアント型パラメーターを持つジェネリック インターフェイスをクラスから変換するときに非 variant の変換がない場合、変換はあいまいです。 例えば:

```vb
Class Base
End Class

Class Derived1
    Inherits Base
End Class

Class Derived2
    Inherits Base
End Class

Class OneAndTwo
    Implements IEnumerable(Of Derived1)
    Implements IEnumerable(Of Derived2)
End Class

Class BaseAndOneAndTwo
    Implements IEnumerable(Of Base)
    Implements IEnumerable(Of Derived1)
    Implements IEnumerable(Of Derived2)
End Class

Module Test
    Sub Main()
        ' Error: conversion is ambiguous
        Dim x As IEnumerable(Of Base) = New OneAndTwo()

        ' OK, will pick up the direct implementation of IEnumerable(Of Base)
        Dim y as IEnumerable(Of Base) = New BaseAndOneAndTwo()
    End Sub
End Module
```

### <a name="anonymous-delegate-conversions"></a>匿名デリゲートの変換

ラムダ メソッドとして分類される式がコンテキスト内の値として再分類とターゲットの型がない (たとえば、 `Dim x = Function(a As Integer, b As Integer) a + b`)、結果として得られる式の型、匿名デリゲートの型は、対象の型がデリゲート型ではない、またはラムダ メソッドのシグネチャと等価です。 この匿名のデリゲート型が互換性のあるデリゲート型への変換: 互換性のあるデリゲート型がデリゲート型と匿名デリゲート型のデリゲート作成式を使用して作成できる`Invoke`メソッドをパラメーターとして。 例えば:

```vb
' Anonymous delegate type similar to Func(Of Object, Object, Object)
Dim x = Function(x, y) x + y

' OK because delegate type is compatible
Dim y As Func(Of Integer, Integer, Integer) = x
```

なお、種類`System.Delegate`と`System.MulticastDelegate`自体ではないと見なされますデリゲート型 (でもすべてのデリゲート型は、そこから継承)。 また、互換性のあるデリゲート型への匿名デリゲートの型から変換が参照変換ではないことに注意してください。

## <a name="array-conversions"></a>配列の変換

だけでなく、実際には参照型であることにより、配列で定義されている、変換は、いくつかの特別な変換は、配列に存在します。

2 つの型`A`と`B`場合、両方の参照型または型パラメーターの値の型であるとわかっていない場合および`A`参照、配列、または入力パラメーターの変換に含まれています`B`の配列からの変換が存在します型`A`型の配列を`B`同じランクを持つ。 このリレーションシップと呼ばれる*配列の共変性*します。 配列の共変性であることを意味要素型が配列の要素`B`要素型が配列の要素が実際にあります`A`、その両方`A`と`B`は参照型とその`B`には、参照変換または配列に変換する`A`します。 次の例の 2 番目の呼び出しで`F`により、`System.ArrayTypeMismatchException`が、実際の要素の入力のためにスローされる例外`b`は`String`ではなく、 `Object`:

```vb
Module Test
    Sub F(ByRef x As Object)
    End Sub

    Sub Main()
        Dim a(10) As Object
        Dim b() As Object = New String(10) {}
        F(a(0)) ' OK.
        F(b(1)) ' Not allowed: System.ArrayTypeMismatchException.
   End Sub
End Module
```

配列の共変性により参照型の配列の要素への代入には、により許可されている型の配列の要素に割り当てられている値が実際には、実行時のチェックが含まれます。

```vb
Module Test
    Sub Fill(array() As Object, index As Integer, count As Integer, _
            value As Object)
        Dim i As Integer

        For i = index To (index + count) - 1
            array(i) = value
        Next i
    End Sub

    Sub Main()
        Dim strings(100) As String

        Fill(strings, 0, 101, "Undefined")
        Fill(strings, 0, 10, Nothing)
        Fill(strings, 91, 10, 0)
    End Sub
End Module
```

この例への代入で`array(i)`メソッドで`Fill`変数によって参照されるオブジェクトに、ランタイム チェックを暗黙的に含まれます`value`いずれかです`Nothing`またはと互換性がある型のインスタンス、配列の実際の要素型`array`します。 メソッドで`Main`、メソッドの最初の 2 つの呼び出し`Fill`失敗すると、3 つ目の呼び出しの原因が、`System.ArrayTypeMismatchException`への最初の割り当てを実行したときにスローされる例外`array(i)`。 例外が発生するため、`Integer`で格納することはできません、`String`配列。

配列の要素型のいずれかが、実行時に値型であることがわかりました型を持つ型パラメーターの場合、`System.InvalidCastException`例外がスローされます。 例えば:

```vb
Module Test
    Sub F(Of T As U, U)(x() As T)
        Dim y() As U = x
    End Sub

    Sub Main()
        ' F will throw an exception because Integer() cannot be
        ' converted to Object()
        F(New Integer() { 1, 2, 3 })
    End Sub
End Module
```

変換は、列挙型の配列の間も存在し、列挙型の配列の基になる型または同じの基になる型を持つ別の列挙型の配列、配列は同じランクを持っています。

```vb
Enum Color As Byte
    Red
    Green
    Blue
End Enum

Module Test
    Sub Main()
        Dim a(10) As Color
        Dim b() As Integer
        Dim c() As Byte

        b = a    ' Error: Integer is not the underlying type of Color
        c = a    ' OK
        a = c    ' OK
    End Sub
End Module
```

この例では、配列の`Color`の配列との間に変換`Byte`、`Color`の基になる型。 配列への変換`Integer`、ただしはエラーになりますので`Integer`の基になる型ではない`Color`します。

型のランク 1 の配列`A()`も、コレクション インターフェイス型への配列変換`IList(Of B)`、 `IReadOnlyList(Of B)`、 `ICollection(Of B)`、`IReadOnlyCollection(Of B)`と`IEnumerable(Of B)`で見つかった`System.Collections.Generic`次のいずれかが当てはまる場合に限り。

- `A` `B`は両方の参照型または型パラメーターの値の型があるとわかっていないと`A`に拡大参照、配列または型パラメーター変換`B`; または
- `A` `B`は、同じ基になる型の両方の列挙型、または
- いずれかの`A`と`B`は、列挙型であり、もう一方が基になる型。

任意のランクを持つタイプ A のすべての配列にも、非ジェネリック コレクション インターフェイス型への配列変換`IList`、`ICollection`と`IEnumerable`で見つかった`System.Collections`します。

使用して、結果として得られるインターフェイスを反復処理することは`For Each`、または呼び出すことによって、`GetEnumerator`メソッドを直接します。 配列のランク 1 の場合のジェネリック、または非ジェネリックの形式を変換`IList`または`ICollection`インデックスを使用して要素を取得することもできます。 ジェネリック、または非ジェネリックの形式に変換ランク 1 の配列の場合`IList`インデックスを使用して要素を設定することも、配列の共変性、同じランタイムを対象とは、前述の操作を確認します。 その他のすべてのインターフェイス メソッドの動作は VB 言語仕様で定義されていません基になるランタイムの責任です。

## <a name="value-type-conversions"></a>値型の変換

値型の値をそのベース参照型またはインターフェイス型と呼ばれるプロセスを実装するのいずれかに変換できる*ボックス化*します。 値型の値がボックス化するとき、値は、.NET Framework ヒープ上に存在する場所の場所からコピーされます。 ヒープ上のこの場所への参照は返され、参照型の変数に格納できます。 この参照と呼ばれることも、*手書き*値型のインスタンス。 ボックス化されたインスタンスには、値型ではなく、参照型と同じセマンティクスがあります。

ボックス化された値の型と呼ばれるプロセスを介して元の値型に変換できる*ボックス化解除*します。 ボックス化された値の型がボックス化された値はヒープから変数の場所にコピーします。 その時点から、値型の場合と動作します。 値の型をボックス化解除、ときに、値には、null 値または値型のインスタンスです。 それ以外の場合、`System.InvalidCastException`例外がスローされます。 値が列挙型のインスタンスの場合は、その値もボックス化解除できます列挙型の基になる型または別の列挙型を持つ同じ基になる型。 Null 値は、リテラルの場合と同様に扱われますが`Nothing`します。

Null 許容型をサポートする型、値の型に値`System.Nullable(Of T)`実行特別にする場合、ボックス化とボックス化解除が扱われます。 型の値をボックス化`Nullable(Of T)`結果の型のボックス化された値`T`場合、値の`HasValue`プロパティが`True`かの値を`Nothing`場合、値の`HasValue`プロパティが`False`します。 型の値をボックス化解除`T`に`Nullable(Of T)`のインスタンスで`Nullable(Of T)`が`Value`プロパティは、ボックス化された値と持つ`HasValue`プロパティは`True`します。 値`Nothing`にボックス化解除することができます`Nullable(Of T)`は`T`され、結果を値に持つ`HasValue`プロパティは`False`。 ボックス化された値の型が参照のように動作するため、型が同じ値に複数の参照を作成することです。 プリミティブ型と列挙型では、これは関連しませんそれらの型のインスタンスが*不変*します。 つまり、同じ値に複数の参照があるという事実を観察することはできませんので、それらの型のボックス化されたインスタンスを変更することはできません。

構造体、その一方で、可能性があります変更可能なインスタンス変数にアクセスできる場合、またはそのメソッドまたはプロパティは、そのインスタンス変数を変更する場合。 ボックス化された構造体への参照を 1 つを使用して構造を変更する場合、ボックス化された構造に対するすべての参照の間で変更が表示されます。 この結果は、予期できない可能性があります、ため場合、値として型指定された`Object`1 つの場所から別のボックス化された値型は使用するだけでそれらの参照がコピーではなく、ヒープ上に自動的に複製にコピーされます。 例えば:

```vb
Class Class1
    Public Value As Integer = 0
End Class

Structure Struct1
    Public Value As Integer
End Structure

Module Test
    Sub Main()
        Dim val1 As Object = New Struct1()
        Dim val2 As Object = val1

        val2.Value = 123

        Dim ref1 As Object = New Class1()
        Dim ref2 As Object = ref1

        ref2.Value = 123

        Console.WriteLine("Values: " & val1.Value & ", " & val2.Value)
        Console.WriteLine("Refs: " & ref1.Value & ", " & ref2.Value)
    End Sub
End Module
```

プログラムの出力は次のとおりです。

```
Values: 0, 123
Refs: 123, 123
```

ローカル変数のフィールドに割り当て`val2`ローカル変数のフィールドには影響しません`val1`ためと、ボックス化された`Struct1`に割り当てられた`val2`値のコピーが行われました。 これに対して、割り当て`ref2.Value = 123`オブジェクトに影響を両方`ref1`と`ref2`参照。

__注意してください。__ として型指定されたボックス化された構造体の構造のコピーが完了しない`System.ValueType`オフの遅延をバインドすることはできませんので`System.ValueType`します。

型は代入時にコピーする値をボックス化のルールに 1 つの例外があります。 ボックス化された値の型参照が別の型で格納されている場合、内部の参照はコピーされません。 例えば:

```vb
Structure Struct1
    Public Value As Object
End Structure

Module Test
    Sub Main()
        Dim val1 As Struct1
        Dim val2 As Struct1

        val1.Value = New Struct1()
        val1.Value.Value = 10

        val2 = val1
        val2.Value.Value = 123
        Console.WriteLine("Values: " & val1.Value.Value & ", " & _
            val2.Value.Value)
    End Sub
End Module
```

プログラムの出力は次のとおりです。

```
Values: 123, 123
```

これは、値をコピーするときに内部のボックス化された値がコピーされないためにです。 したがって、どちらも`val1.Value`と`val2.Value`同じボックス化された値型への参照します。

__注意してください。__ 内部のボックス化された値の型はコピーされません実際には、.NET の制限の入力型の値のたびにすべての内部のボックス化された値型がコピーされたことを確認する - システム`Object`がコピーされた非常にコスト高になります。

既に説明した、ボックス化された値と型のみボックス化解除できますを元の型。 ボックス化されたプリミティブ型、ただしは処理として型指定されたときに特別に`Object`します。 これらへの変換を持っている他のプリミティブ型に変換できます。 例えば:

```vb
Module Test
    Sub Main()
        Dim o As Object = 5
        Dim b As Byte = CByte(o)  ' Legal
        Console.WriteLine(b) ' Prints 5
    End Sub
End Module
```

通常、ボックス化`Integer`値`5`にボックス化解除がない、`Byte`変数。 ただし、ため`Integer`と`Byte`プリミティブ型であり、変換、変換は許可します。

値の型に変換するインターフェイスはジェネリック引数がインターフェイスに制限するものと異なる場合に重要です。 制約付きの型パラメーターでインターフェイス メンバーにアクセスするときに (またはメソッドの呼び出しで`Object`)、値型はインターフェイスに変換され、インターフェイス メンバーにアクセスするときのようにボックス化が発生しません。 たとえば、インターフェイス`ICounter`メソッドを含む`Increment`値の変更に使用できます。 場合`ICounter`制約の実装として提供される、`Increment`メソッドを呼び出すと、変数への参照を`Increment`で、ボックス化されたコピーではなくが呼び出されました。

```vb
Interface ICounter
    Sub Increment()
    ReadOnly Property Value() As Integer
End Interface

Structure Counter
    Implements ICounter

    Dim _value As Integer

    Property Value() As Integer Implements ICounter.Value
        Get
            Return _value
        End Get
    End Property

    Sub Increment() Implements ICounter.Increment
       value += 1
    End Sub
End Structure

Module Test
      Sub Test(Of T As ICounter)(x As T)
         Console.WriteLine(x.value)
         x.Increment()                     ' Modify x
         Console.WriteLine(x.value)
         CType(x, ICounter).Increment()    ' Modify boxed copy of x
         Console.WriteLine(x.value)
      End Sub

      Sub Main()
         Dim x As Counter
         Test(x)
      End Sub
End Module
```

最初の呼び出し`Increment`変数に値を変更します`x`します。 これは、2 番目の呼び出しに相当しない`Increment`、ボックス化されたコピーの値が変更されます`x`します。 そのため、プログラムの出力は次のとおりです。

```
0
1
1
```

### <a name="nullable-value-type-conversions"></a>Null 許容値型の変換

値型`T`、型の null 許容バージョンとの間に変換できる`T?`します。 変換`T?`に`T`がスローされます、`System.InvalidOperationException`変換される値がある場合は例外`Nothing`します。 また、`T?`型への変換が`S`場合`T`への組み込みの変換が`S`します。 場合`S`間に、次の組み込み変換が存在し、値の型は、`T?`と`S?`:

* 同じ分類 (縮小または拡大) からの変換`T?`に`S?`します。

* 同じ分類 (縮小または拡大) からの変換`T`に`S?`します。

* 縮小変換から`S?`に`T`します。

組み込みの拡大変換が存在するなど、`Integer?`に`Long?`から組み込みの拡大変換が存在するので、`Integer`に`Long`:

```vb
Dim i As Integer? = 10
Dim l As Long? = i
```

変換するときに`T?`に`S?`場合は、値`T?`は`Nothing`の値`S?`なります`Nothing`します。 変換するときに`S?`に`T`または`T?`に`S`場合は、値`T?`または`S?`は`Nothing`、`System.InvalidCastException`例外がスローされます。

基になる型の動作のため`System.Nullable(Of T)`ときに null 許容値型、`T?`は、結果は型のボックス化された値をボックス化、 `T`、型のボックス化された値ではなく`T?`します。 逆に、null 許容値型にボックス化解除と`T?`で、値が折り返される`System.Nullable(Of T)`、および`Nothing`型の null 値をボックス化されます`T?`します。 例えば:

```vb
Dim i1? As Integer = Nothing
Dim o1 As Object = i1

Console.WriteLine(o1 Is Nothing)                    ' Will print True
o1 = 10
i1 = CType(o1, Integer?)
Console.WriteLine(i1)                               ' Will print 10
```

この動作の副作用は、null 許容の値が入力`T?`すべてのインターフェイスを実装するために表示される`T`ボックス化される型、インターフェイスに値型に変換する必要があるため、します。 その結果、`T?`はすべてのインターフェイスに変換する`T`に変換可能です。 ただし、null 許容の値が入力することが重要`T?`のインターフェイスを実際には実装しません`T`ジェネリック制約チェックやリフレクションのためです。 例えば:

```vb
Interface I1
End Interface

Structure T1
    Implements I1
    ...
End Structure

Module Test
    Sub M1(Of T As I1)(ByVal x As T)
    End Sub

    Sub Main()
        Dim x? As T1 = Nothing
        Dim y As I1 = x                ' Valid
        M1(x)                          ' Error: x? does not satisfy I1 constraint
    End Sub
End Module
```

## <a name="string-conversions"></a>文字列の変換

変換する`Char`に`String`が最初の文字は文字の値の文字列にします。 変換する`String`に`Char`値が文字列の最初の文字を文字になります。 配列に変換する`Char`に`String`文字が、配列の要素の文字列にします。 変換する`String`の配列に`Char`要素は、文字列の文字の文字の配列になります。

間の正確な変換`String`と`Boolean`、 `Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、 `ULong`、 `Long`、 `Decimal`、 `Single`、 `Double`、 `Date`、およびその逆の場合、この仕様の範囲を超えていて、実装は、1 つの詳細を除く依存します。 文字列の変換には、実行時環境の現在のカルチャが常に検討してください。 そのため、実行時に実行する必要があります。

## <a name="widening-conversions"></a>拡大変換

拡大変換では、オーバーフローありませんが、精度の損失を伴う場合があります。 次の変換は拡大変換。

__Identity/既定の変換__

* それ自体の型。

* 匿名デリゲートの型から同じシグネチャを持つ任意のデリゲート型へのラムダ メソッドの再分類に対して生成されます。

* リテラルから`Nothing`型にします。

__数値変換__

* `Byte`に`UShort`、 `Short`、 `UInteger`、 `Integer`、 `ULong`、 `Long`、 `Decimal`、 `Single`、または`Double`します。

* `SByte`に`Short`、 `Integer`、 `Long`、 `Decimal`、 `Single`、または`Double`します。

* `UShort`に`UInteger`、 `Integer`、 `ULong`、 `Long`、 `Decimal`、 `Single`、または`Double`します。

* `Short`に`Integer`、 `Long`、 `Decimal`、`Single`または`Double`します。

* `UInteger`に`ULong`、 `Long`、 `Decimal`、 `Single`、または`Double`します。

* `Integer`に`Long`、 `Decimal`、`Single`または`Double`します。

* `ULong`に`Decimal`、 `Single`、または`Double`します。

* `Long`に`Decimal`、`Single`または`Double`します。

* `Decimal`に`Single`または`Double`します。

* `Single`に`Double`します。

* リテラルから`0`列挙型にします。 (__に注意してください。__ 変換`0`テスト フラグを簡略化するには、任意の列挙型に拡大変換します。 場合など、`Values`値を持つ列挙型は、 `One`、変数をテストできる`v`型の`Values`ことを示す`(v And Values.One) = 0`)。

* 基になる数値型または数値型、列挙型からへの拡大変換がその基になる数値を入力します。

* 型の定数式から`ULong`、 `Long`、 `UInteger`、 `Integer`、 `UShort`、 `Short`、 `Byte`、または`SByte`より狭い型に指定の定数式の値内では、変換先の型の範囲です。 (__に注意してください。__ 変換`UInteger`または`Integer`に`Single`、`ULong`または`Long`に`Single`または`Double`、または`Decimal`に`Single`または`Double`可能性がありますが、精度の損失を発生させることはありませんが1 桁の損失が発生します。 数値変換、拡大情報は失われません。)

__参照変換__

* 基本データ型への参照型。

* インターフェイス型への参照型から型が、インターフェイスまたはバリアントの互換性のあるインターフェイスを実装するを指定します。

* 対象のインターフェイス型から`Object`します。

* バリアントの互換性のあるインターフェイス型にインターフェイスの型。

* 互換性のあるバリアントにデリゲート型のデリゲートの型にします。 (__に注意してください。__ その他の多くの参照変換はこれらの規則によって暗黙的に指定します。 たとえば、匿名のデリゲートは、参照型から継承する`System.MulticastDelegate`; 配列型は参照型から継承する`System.Array`; 匿名型は参照型から継承する`System.Object`)。

__匿名デリゲートの変換__

* 匿名からより多くのデリゲート型にラムダ メソッドの再分類に対して生成された型を委任します。

__配列変換__

* 配列型から`S`要素型が`Se`が配列型に`T`要素型が`Te`true は、次のすべての提供。

  * `S` `T`要素の型のみが異なります。

  * 両方`Se`と`Te`は参照型に既知の型パラメーターまたは参照型。

  * 拡大の参照、配列、または型からパラメーターの変換が存在する`Se`に`Te`します。

* 配列型から`S`列挙された要素型が`Se`が配列型に`T`要素型が`Te`true は、次のすべての提供。

  * `S` `T`要素の型のみが異なります。

  * `Te` 基になる型は、`Se`します。

* 配列型から`S`列挙された要素型のランク 1 の`Se`を`System.Collections.Generic.IList(Of Te)`、 `IReadOnlyList(Of Te)`、 `ICollection(Of Te)`、 `IReadOnlyCollection(Of Te)`、および`IEnumerable(Of Te)`、指定された、次のいずれかが true:

  * 両方`Se`と`Te`は参照型または型パラメーターは参照である型、および拡大の参照が配列、またはから型パラメーター変換が存在する既知の`Se`に`Te`; または

  * `Te` 基になる型は、 `Se`; または

  * `Te` は `Se` と同じ

__値型の変換__

* 値の型を基本データ型。

* 型が実装するインターフェイス型に値型。

__Null 許容の値の型変換__

* 型から`T`型に`T?`します。

* 型から`T?`型`S?`型から拡大変換がある場合、`T`型に`S`します。

* 型から`T`型`S?`型から拡大変換がある場合、`T`型に`S`します。

* 型から`T?`インターフェイス型を型`T`を実装します。

__文字列の変換__

* `Char`に`String`します。

* `Char()`に`String`します。

__型パラメーター変換__

* 型パラメーターから`Object`します。

* インターフェイス型制約またはインターフェイス型の制約と互換性のあるインターフェイス バリアント型パラメーター。

* クラスの制約によって実装されるインターフェイスの型パラメーター。

* クラスの制約によって実装されるインターフェイスと互換性のあるインターフェイス バリアント型を型パラメーター。

* クラスの制約、または、クラス制約の基本型の型パラメーター。

* 型パラメーターから`T`型パラメーター制約に`Tx`、または何も`Tx`に拡大変換します。

## <a name="narrowing-conversions"></a>縮小変換

縮小変換では、常に成功する変換、情報が失われる可能性があることがわかっている変換および変換を縮小表記するだけの価値を十分にさまざまな種類のドメイン間で。 次の変換は縮小変換として分類されます。

__ブール型変換__

* `Boolean`に`Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、 `ULong`、 `Long`、 `Decimal`、 `Single`、または`Double`します。

* `Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、 `ULong`、 `Long`、 `Decimal`、 `Single`、または`Double`に`Boolean`します。

__数値変換__

* `Byte`に`SByte`します。

* `SByte`に`Byte`、 `UShort`、 `UInteger`、または`ULong`します。

* `UShort`に`Byte`、 `SByte`、または`Short`します。

* `Short`に`Byte`、 `SByte`、 `UShort`、 `UInteger`、または`ULong`します。

* `UInteger`に`Byte`、 `SByte`、 `UShort`、 `Short`、または`Integer`します。

* `Integer`に`Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、または`ULong`します。

* `ULong`に`Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、または`Long`します。

* `Long`に`Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、または`ULong`します。

* `Decimal`に`Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、 `ULong`、または`Long`します。

* `Single`に`Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、 `ULong`、 `Long`、または`Decimal`します。

* `Double`に`Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、 `ULong`、 `Long`、 `Decimal`、または`Single`します。

* 列挙型の数値型から

* 数値型の列挙型から基になる数値型への縮小変換が。

* 別の列挙型の列挙型。

__参照変換__

* より強い派生型への参照型。

* インターフェイス型に、クラス型からクラス型がインターフェイス型またはそれと互換性のあるインターフェイス型バリアントを実装しませんを提供します。

* クラス型へのインターフェイス型。

* インターフェイスからされている別のインターフェイス型には 2 つの型の間の継承関係がないと、variant と互換性のあるないを指定します。

__匿名デリゲートの変換__

* より狭いデリゲート型へのラムダ メソッドの再分類に対して生成される匿名デリゲートの型。

__配列変換__

* 配列型から`S`要素型が`Se`、配列型に`T`要素型が`Te`、その次のすべてに該当します。

  * `S` `T`要素の型のみが異なります。
  * 両方`Se`と`Te`は参照型または型パラメーターに認識されていない値型であります。
  * 縮小参照、配列、または型パラメーター変換が存在する`Se`に`Te`します。

* 配列型から`S`要素型が`Se`が配列型に`T`列挙された要素型が`Te`true は、次のすべての提供。

  * `S` `T`要素の型のみが異なります。
  * `Se` 基になる型は、 `Te` 、か、同じ基になる型を共有する両方のさまざまな列挙型。

* 配列型から`S`列挙された要素型のランク 1 の`Se`を`IList(Of Te)`、 `IReadOnlyList(Of Te)`、 `ICollection(Of Te)`、`IReadOnlyCollection(Of Te)`と`IEnumerable(Of Te)`、指定された、次のいずれかが true:

  * 両方`Se`と`Te`は参照型やは参照型では、既知の型パラメーターから縮小参照、配列、または型パラメーター変換が存在する`Se`に`Te`; または
  * `Se` 基になる型は、 `Te`、か、同じ基になる型を共有する両方のさまざまな列挙型。

__値型の変換__

* 派生値型への参照型。

* インターフェイスの型を値型から値の型がインターフェイス型を実装を提供します。

__Null 許容の値の型変換__

* 型から`T?`型`T`します。

* 型から`T?`型`S?`型から縮小変換がある場合、`T`型に`S`します。

* 型から`T`型`S?`型から縮小変換がある場合、`T`型に`S`します。

* 型から`S?`型`T`型からの変換がある場合、`S`型に`T`します。

__文字列の変換__

* `String`に`Char`します。

* `String`に`Char()`します。

* `String`に`Boolean`との間`Boolean`に`String`します。

* 間の変換`String`と`Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、 `ULong`、 `Long`、 `Decimal`、 `Single`、または`Double`します。

* `String`に`Date`との間`Date`に`String`します。

__型パラメーター変換__

* `Object`型パラメーターにします。

* 型から、インターフェイス型、型パラメーターを指定するパラメーターがないそのインターフェイスに制約付きまたはそのインターフェイスを実装するクラスに制限は。

* 型パラメーターをインターフェイスの型。

* クラス制約の派生型に型パラメーター。

* 型パラメーターから`T`型パラメーターの制約に`Tx`に縮小変換します。

## <a name="type-parameter-conversions"></a>型パラメーター変換

型パラメーターの変換は、配置、いずれかである場合、制約によって決定されます。 型パラメーター`T`と常に、それ自体に変換できる`Object`、およびとの間の任意のインターフェイス型。 型をされた場合に注意してください`T`ランタイムからの変換に値型では、`T`に`Object`インターフェイス型はボックス化変換および変換されます`Object`またはインターフェイス型`T`なボックス化解除されます変換を行います。 クラスの制約を指定された型パラメーター`C`には、型パラメーターの追加の変換を定義します`C`とその基本クラスでは、その逆です。 型パラメーター`T`型パラメーター制約`Tx`への変換を定義します。`Tx`および`Tx`に変換します。

配列要素型は、型パラメーターはインターフェイスの制約で`I`が配列要素型が同じ配列の共変変換`I`、その型パラメーターがあります、`Class`クラスの制約 (または参照の配列要素の型のみがあるため共変)。 配列要素型は、型パラメーター、クラスの制約で`C`が配列要素型が同じ配列の共変変換`C`します。

上記の変換規則は制約のない型パラメーターからインターフェイス以外の型への変換許可されていません驚くべきことになる可能性もあります。 この理由は、このような変換のセマンティクスについて混乱を避けるためです。 たとえば、次のような宣言があるとします。

```vb
Class X(Of T)
    Public Shared Function F(t As T) As Long 
        Return CLng(t)    ' Error, explicit conversion not permitted
    End Function
End Class
```

場合の変換`T`に`Integer`簡単に期待される、許可されたを`X(Of Integer).F(7)`は返します`7L`。 ただし、設定されませんでしたが、数値変換には、コンパイル時に数値型がわかっている場合にのみと見なされるためです。 セマンティクスを作成するには明確で上記の例では、代わりに記述する必要があります。

```vb
Class X(Of T)
    Public Shared Function F(t As T) As Long
        Return CLng(CObj(t))    ' OK, conversions permitted
    End Function
End Class
```

## <a name="user-defined-conversions"></a>ユーザー定義変換

*組み込み変換*変換中に (つまり表示されているこの仕様で)、言語によって定義されている*ユーザー定義の変換*オーバー ロードによって定義されます、`CType`演算子。 適用可能な組み込み変換がない場合は、型の間で変換するときに、ユーザー定義の変換が考慮されます。 ユーザー定義の変換がある場合*最も具体的*ソースとターゲットの種類では、そのユーザー定義の変換が使用されます。 それ以外の場合、コンパイル時エラーが発生します。 最も固有の変換がオペランドがソースの種類に「最も近い」には、結果の型がターゲットの型に「最も近い」です。 使用するどのようなユーザー定義の変換を決定するには、最も固有の拡大変換が使用されます。拡大しない場合は、変換は最も具体的に最も固有の縮小変換が使用されます。 最も関連の縮小変換は、変換が定義されているし、し、コンパイル時エラーが発生します。

次のセクションでは、最も固有の変換を決定する方法について説明します。 次の用語を使用します。

型から変換が存在する場合は、組み込みの拡大`A`型`B`、どちらの場合と`A`も`B`は、インターフェイス、`A`は*包含*によって`B`、`B` *包含*`A`します。

*最も外側にある*型のセット内の型は、セット内の他のすべての型を包含している 1 つの型。 その他のすべての型を包含する 1 つの型の場合セットに最も外側の型がありません。 直感的な用語では、最も外側の型は、set - の他の種類ごとに拡大変換を介して変換できる 1 つの型の「最大」の型です。

*最も内側の*一連の型の型が、セット内の他のすべての種類に包含されている 1 つの型。 他のすべての型によって 1 つの型が含まれていない場合、セットが最も内側の型。 直感的な用語では、最も内側の型は、set - の縮小変換を他の種類ごとに変換できる 1 つの型の「最小」の型です。

型の候補ユーザー定義の変換を収集するときに`T?`で定義されたユーザー定義の変換演算子`T`代わりに使用されます。 変換される型が null 許容値型では、次のいずれかでも場合`T`のユーザー定義変換演算子を伴う null 非許容値型のみが無効になります。 変換演算子`T`に`S`からの変換にリフトされた`T?`に`S?`変換によって評価されると`T?`に`T`必要に応じて、ユーザー定義の変換を評価し、演算子を`T`に`S`変換してみます`S`に`S?`のために必要な場合は、します。 変換される値が場合`Nothing`、リフトの変換演算子がの値に直接変換するただし、`Nothing`として型指定された`S?`します。 例えば:

```vb
Structure S
    ...
End Structure

Structure T
    Public Shared Widening Operator CType(ByVal v As T) As S
        ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim x As T?
        Dim y As S?

        y = x                ' Legal: y is still null
        x = New T()
        y = x                ' Legal: Converts from T to S
    End Sub
End Module
```

変換では、解決するユーザー定義されたときに変換演算子はリフト変換演算子より優先常にします。 例えば:

```vb
Structure S
    ...
End Structure

Structure T
    Public Shared Widening Operator CType(ByVal v As T) As S
        ...
    End Operator

    Public Shared Widening Operator CType(ByVal v As T?) As S?
        ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim x As T?
        Dim y As S?

        y = x                ' Calls user-defined conversion, not lifted conversion
    End Sub
End Module
```

実行時に、ユーザー定義の変換の評価に最大 3 つの手順を含むことができます。

1. 最初に、値は、必要に応じて、組み込みの変換を使用して、オペランドの型に元の型から変換されます。

2. 次に、ユーザー定義の変換が呼び出されます。

3. 最後に、ユーザー定義の変換の結果は、必要に応じて、組み込みの変換を使用して、ターゲット型に変換されます。

ユーザー定義の変換の評価は 1 つ以上のユーザー定義変換演算子が含まれることはないことに注意してください。

### <a name="most-specific-widening-conversion"></a>最も固有の拡大変換

最も固有ユーザー定義拡大変換演算子の 2 種類の間を決定するには、次の手順を使用して実現されます。

1. 最初に、すべての候補の変換演算子が収集されます。 変換演算子の候補は、ソースの種類のユーザー定義の拡大変換演算子のすべてとすべてのターゲットの型でユーザー定義の拡大変換演算子。

2. 次に、すべての非該当変換演算子は、セットから削除されます。 変換演算子は、演算子があります。 組み込み拡大変換元の型からオペランドの型へと、組み込み拡大への変換演算子、演算子の結果から対象の型がある場合、ソースの種類とターゲットの種類に適用されます。 適用可能な変換演算子がない場合は、し、固有がない最も拡大変換します。

3. 次に、適用できる変換演算子の特定のソースの種類が決定されます。

   * 変換演算子のいずれかの変換元の型から直接場合、元の型が特定のソースの種類です。

   * それ以外の場合、最も固有のソースの種類は、変換演算子のソースの種類の結合されたセット内の最も内側の型。 場合に最も内側のない型が見つからない、最も関連の変換を拡大し、します。

4. 次に、適用できる変換演算子の特定のターゲット型が決定されます。

   * 場合は、ターゲット型に直接変換演算子のいずれかの変換、対象の型は最も固有のターゲット型です。

   * それ以外の場合、最も固有のターゲット型は、対象の型の変換演算子の結合されたセット内の最も外側の型です。 最も外側の型が見つからない場合、固有がない最も拡大変換します。

5. 次に、正確に 1 つの変換演算子は、特定のソースの種類から最も固有のターゲット型に変換、した場合、これが最も固有の変換演算子。 このような 1 つ以上のオペレーターが存在する場合に最も固有の拡大変換はあるありません。

### <a name="most-specific-narrowing-conversion"></a>最も固有の縮小変換

最も固有ユーザー定義縮小変換演算子の 2 種類の間を決定するには、次の手順を使用して実現されます。

1. 最初に、すべての候補の変換演算子が収集されます。 変換演算子の候補は、ソースの種類のユーザー定義の変換演算子のすべてとすべてのターゲットの型で、ユーザー定義の変換演算子。

2. 次に、すべての非該当変換演算子は、セットから削除されます。 変換演算子は、ソースの種類からオペランドの型への組み込みの変換演算子があるし、演算子の結果から対象の型への組み込みの変換演算子がある場合、ソースの種類とターゲットの種類に適用されます。 適用可能な変換演算子がない場合は、最も固有の縮小変換はあるありません。

3. 次に、適用できる変換演算子の特定のソースの種類が決定されます。

   * 変換演算子のいずれかの変換元の型から直接場合、元の型が特定のソースの種類です。

   * それ以外の場合、変換演算子のいずれかの変換元の型を包含する型から場合、これらの変換演算子のソースの種類の結合されたセット内の最も内側の型は、特定のソースの種類にします。 場合に最も内側のない型が見つからない、最も固有の縮小変換はありません。

   * それ以外の場合、特定のソースの種類は、変換演算子のソースの種類の結合されたセット内の最も外側の型。 最も外側の型が見つからない場合に最も固有の縮小変換はあるありません。

4. 次に、適用できる変換演算子の特定のターゲット型が決定されます。

   * 場合は、ターゲット型に直接変換演算子のいずれかの変換、対象の型は最も固有のターゲット型です。

   * それ以外の場合、対象の型によって包含する型に変換、変換演算子のいずれかの場合これらの変換演算子のソースの種類の結合されたセット内の最も外側の型が特定のターゲット型にします。 最も外側の型が見つからない場合に最も固有の縮小変換はあるありません。

   * それ以外の場合、最も固有のターゲット型は、対象の型の変換演算子の結合されたセット内の最も内側の型。 場合に最も内側のない型が見つからない、最も固有の縮小変換はありません。

5. 次に、正確に 1 つの変換演算子は、特定のソースの種類から最も固有のターゲット型に変換、した場合、これが最も固有の変換演算子。 このような 1 つ以上のオペレーターが存在する場合に最も固有の縮小変換はあるありません。

## <a name="native-conversions"></a>ネイティブ変換

変換のいくつかに分類されます*ネイティブ変換*ネイティブ .NET Framework でサポートされているためです。 これらの変換ですが、使用して最適化することができます、`DirectCast`と`TryCast`変換の演算子とその他の特殊な動作です。 分類は、ネイティブの変換と変換: 恒等変換、既定の変換、参照変換、配列の変換、値型の変換、または型パラメーター変換します。

## <a name="dominant-type"></a>主要な型

型を指定する必要がある多くの場合、型の推定を決定するなどの状況で、*優先型*のセット。 型のセットの主要な型は、最初にその他の 1 つまたは複数の型への暗黙的な変換がない任意の型を削除することによって決定されます。 ない場合型左この時点で、主要な型はありません。 主要な型が、最も内側の残りの種類のします。 1 つ以上の型がある場合は、最も内側の主要な型はありません。
