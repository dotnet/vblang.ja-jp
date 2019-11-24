---
ms.openlocfilehash: 7aef52145a71bff1d489772e81eb786a9dbd23d1
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306191"
---
# <a name="conversions"></a>変換

変換とは、ある型から別の型に値を変更する処理のことです。 たとえば、`Integer` 型の値を `Double`型の値に変換したり、`Derived` 型の値を `Base`型の値に変換したりすることができます。これは `Base` と `Derived` が両方ともクラスであり、`Derived` から継承されていることを前提としています。`Base` 変換では、値自体を変更する必要がない場合があります (後者の例のように)。または、(前の例のように) 値表現に大きな変更が必要になる場合があります。

変換は、拡大または縮小することができます。 *拡大変換*とは、ある型から別の型への変換のことです。この型は、値ドメインが元の型の値ドメインよりも大きい場合は、少なくとも大きくなります。 拡大変換は失敗しません。 *縮小変換*とは、ある型から別の型への変換のことです。この型は、値ドメインが元の型の値ドメインより小さいか、または変換を実行するときに余分な注意が必要である (たとえば、`Integer` から `String`に変換する場合など)。 情報の損失を伴う可能性がある縮小変換は失敗する可能性があります。

Id 変換 (型からそれ自体への変換) と既定値の変換 (つまり、`Nothing`からの変換) は、すべての型に対して定義されます。

## <a name="implicit-and-explicit-conversions"></a>暗黙の型変換と明示的な型変換

変換は、*暗黙的*または*明示的*に行うことができます。 暗黙の型変換は、特別な構文を使用せずに発生します。 `Integer` 値から `Long` 値への暗黙的な変換の例を次に示します。

```vb
Module Test
    Sub Main()
        Dim intValue As Integer = 123
        Dim longValue As Long = intValue

        Console.WriteLine(intValue & " = " & longValue)
    End Sub
End Module
```

一方、明示的な変換では、キャスト演算子が必要です。 Cast 演算子を使用せずに値に対して明示的な変換を実行しようとすると、コンパイル時エラーが発生します。 次の例では、明示的な変換を使用して、`Long` 値を `Integer` 値に変換します。

```vb
Module Test
    Sub Main()
        Dim longValue As Long = 134
        Dim intValue As Integer = CInt(longValue)

        Console.WriteLine(longValue & " = " & intValue)
    End Sub
End Module
```

暗黙的な変換のセットは、コンパイル環境と `Option Strict` ステートメントによって異なります。 厳密なセマンティクスが使用されている場合、暗黙的に拡張変換のみが発生する可能性があります。 寛容なセマンティクスが使用されている場合、すべての拡大変換および縮小変換 (つまり、すべての変換) が暗黙的に発生する可能性があります。

## <a name="boolean-conversions"></a>ブール型変換

`Boolean` は数値型ではありませんが、列挙型であるかのように、数値型との間で縮小変換が行われます。 リテラル `True` は、`Byte`の場合はリテラル `255` に変換されます。 `UShort`の場合は `65535`、`4294967295` の場合は `UInteger`、`18446744073709551615` の場合は `ULong`、`-1`、`SByte`、`Short`、`Integer`、`Long`、`Decimal`の場合は式 `Single`に変換されます。`Double` リテラル `False` は、リテラル `0`に変換されます。 数値が0の場合は、リテラル `False`に変換されます。 その他のすべての数値は、リテラル `True`に変換されます。

ブール値から文字列への縮小変換では、`System.Boolean.TrueString` または `System.Boolean.FalseString`のいずれかに変換されます。 `String` から `Boolean`への縮小変換もあります。文字列が `TrueString` または `FalseString` (現在のカルチャでは区別) と等しい場合は、適切な値が使用されます。それ以外の場合は、文字列を数値型として解析しようとします (可能な場合は16進数または8進数、それ以外の場合は float)。それ以外の場合は、`System.InvalidCastException`をスローします。

## <a name="numeric-conversions"></a>数値変換

型 `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`、`Decimal`、`Single`、`Double`、およびすべての列挙型の間には数値変換が存在します。 変換されるとき、列挙型は基になる型として扱われます。 列挙型に変換する場合、ソース値は列挙型で定義されている値のセットに準拠している必要はありません。 例 :

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

数値変換は、次のように実行時に処理されます。

* 数値型からより広い数値型への変換では、値は単純により広い型に変換されます。 `UInteger`、`Integer`、`ULong`、`Long`、または `Decimal` から `Single` または `Double` への変換は、最も近い `Single` または `Double` 値に丸められます。 この変換によって有効桁数が失われる可能性がありますが、マグニチュードが失われることはありません。

* 整数型から別の整数型への変換、または `Single`、`Double`、または `Decimal` から整数型への変換の結果は、整数のオーバーフローチェックがオンかどうかによって異なります。

  *整数オーバーフローを確認する場合は、次のようになります。*

  * ソースが整数型の場合、変換元の引数が変換先の型の範囲内にある場合、変換は成功します。 変換元の引数が変換先の型の範囲外の場合、変換は `System.OverflowException` 例外をスローします。

  * ソースが `Single`、`Double`、または `Decimal`の場合、ソース値は最も近い整数値に切り上げられます。この整数値は変換の結果になります。 ソース値が2つの整数値に均等に近い場合、値は、最下位桁の位置にある偶数を持つ値に丸められます。 結果の整数値が変換先の型の範囲外になると、`System.OverflowException` 例外がスローされます。

  *整数オーバーフローがチェックされていない場合は、次のようになります。*

  * ソースが整数型の場合、変換は常に成功し、ソース値の最上位ビットを破棄するだけで構成されます。

  * ソースが `Single`、`Double`、または `Decimal`の場合、変換は常に成功し、最も近い整数値に対するソース値の丸め処理が行われます。 ソース値が2つの整数値に均等に近い場合、値は常に、最下位桁の位置にある偶数の値に丸められます。

* `Double` から `Single`への変換では、`Double` 値は最も近い `Single` 値に丸められます。 `Double` 値が小さすぎて `Single`として表現できない場合、結果は正のゼロまたは負のゼロになります。 `Double` 値が大きすぎて `Single`として表現できない場合、結果は正の無限大または負の無限大になります。 `Double` 値が `NaN`場合は、結果も `NaN`ます。

* `Single` または `Double` から `Decimal`への変換では、変換元の値が `Decimal` 表現に変換され、必要に応じて28桁の小数点以下の最も近い数値に丸められます。 ソース値が小さすぎて `Decimal`として表現できない場合、結果は0になります。 ソース値が `NaN`、無限大、または大きすぎて `Decimal`として表現できない場合は、`System.OverflowException` 例外がスローされます。

* `Double` から `Single`への変換では、`Double` 値は最も近い `Single` 値に丸められます。 `Double` 値が小さすぎて `Single`として表現できない場合、結果は正のゼロまたは負のゼロになります。 `Double` 値が大きすぎて `Single`として表現できない場合、結果は正の無限大または負の無限大になります。 `Double` 値が `NaN`場合は、結果も `NaN`ます。

## <a name="reference-conversions"></a>参照変換

参照型は基本型に変換できますが、その逆も可能です。 変換される値が null 値、派生型自体、またはより派生型である場合、基本型からより派生型への変換は実行時にのみ成功します。

クラス型とインターフェイス型は、任意のインターフェイス型との間でキャストできます。 型とインターフェイス型の間の変換は、関連する実際の型が継承または実装関係を持つ場合にのみ、実行時に成功します。 インターフェイス型には常に `Object`から派生した型のインスタンスが含まれているので、インターフェイス型は常に `Object`との間でキャストできます。

__付箋.__ COM クラスを表すクラスには、実行時までわからないインターフェイスの実装が含まれている可能性があるため、`NotInheritable` クラスを、実装していないインターフェイスとの間で変換することはできません。 

実行時に参照変換が失敗した場合は、`System.InvalidCastException` 例外がスローされます。

### <a name="reference-variance-conversions"></a>参照の分散変換

ジェネリックインターフェイスまたはデリゲートは、型の互換性のあるバリアント間の変換を可能にするバリアント型パラメーターを持つことができます。 したがって、実行時に、クラス型またはインターフェイス型から、派生したインターフェイス型またはを実装するインターフェイス型との互換性を持つインターフェイス型への変換は成功します。 同様に、デリゲート型は、バリアント互換のデリゲート型との間でキャストできます。 たとえば、デリゲート型

```vb
Delegate Function F(Of In A, Out R)(a As A) As R
```

`F(Of Object, Integer)` から `F(Of String, Integer)`への変換を許可します。 つまり、`Object` を受け取るデリゲート `F` は、`String`を受け取るデリゲート `F` として安全に使用される場合があります。 デリゲートが呼び出されると、ターゲットメソッドはオブジェクトを想定し、文字列はオブジェクトになります。

一般的なデリゲートまたはインターフェイス型 `S(Of S1,...,Sn)` は、次の場合に `T(Of T1,...,Tn)` ジェネリックインターフェイスまたはデリゲート型との*バリアント互換*と呼ばれます。

* `S` と `T` は、両方とも同じジェネリック型 `U(Of U1,...,Un)`から構築されます。

* `Ux`型パラメーターごとに次のように入力します。

  * 型パラメーターが variance なしで宣言されている場合、`Sx` と `Tx` は同じ型である必要があります。

  * 型パラメーターが `In` 宣言されている場合は、`Sx` から `Tx`に拡張 id、既定値、参照、配列、または型パラメーターを変換する必要があります。

  * 型パラメーターが `Out` 宣言されている場合は、`Tx` から `Sx`に拡張 id、既定値、参照、配列、または型パラメーターを変換する必要があります。

クラスからバリアント型パラメーターを持つジェネリックインターフェイスに変換するときに、クラスが複数のバリアント互換インターフェイスを実装する場合、バリアント以外の変換がない場合、変換はあいまいです。 例 :

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

ラムダメソッドとして分類された式が、対象の型が存在しないコンテキスト (`Dim x = Function(a As Integer, b As Integer) a + b`など) の値として再分類される場合、またはターゲット型がデリゲート型ではない場合、結果として得られる式の型は、ラムダメソッドのシグネチャと等価な匿名デリゲート型になります。 この匿名デリゲート型には、互換性のある任意のデリゲート型への変換が含まれています。互換性のあるデリゲート型は、匿名デリゲート型の `Invoke` メソッドをパラメーターとして持つデリゲート作成式を使用して作成できる任意のデリゲート型です。 例 :

```vb
' Anonymous delegate type similar to Func(Of Object, Object, Object)
Dim x = Function(x, y) x + y

' OK because delegate type is compatible
Dim y As Func(Of Integer, Integer, Integer) = x
```

型 `System.Delegate` と `System.MulticastDelegate` は、デリゲート型とは異なります (すべてのデリゲート型が継承していますが)。 また、匿名デリゲート型から互換性のあるデリゲート型への変換は、参照変換ではないことに注意してください。

## <a name="array-conversions"></a>配列変換

配列に定義されている変換に加えて、参照型であるという事実によって、配列に対していくつかの特殊な変換が存在します。

`A` と `B`の2つの型では、それらが参照型であるか、値型として知られていない型パラメーターの両方であり、`A` に参照、配列、または型パラメーターの `B`への変換がある場合、`A` 型の配列から、同じ順位を持つ `B` 型の配列への変換が存在します。 このリレーションシップは、*配列の共変性*と呼ばれます。 配列の共変性は、要素型が `B` の配列の要素が、実際には要素型が `A`配列の要素であることを意味します。これは、`A` と `B` の両方が参照型であり、`B` に参照変換または配列変換が含まれていることを意味します。`A` 次の例では、`F` の2回目の呼び出しによって `System.ArrayTypeMismatchException` 例外がスローされます。これは、`b` の実際の要素の型が `String`であり、`Object`ではないためです。

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

配列の共変性により、参照型の配列の要素への代入には、配列要素に割り当てられている値が実際に許可されている型であることを保証するランタイムチェックが含まれます。

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

この例では、メソッド `Fill` に `array(i)` するための割り当てには、変数 `value` によって参照されるオブジェクトが `Nothing` であるか、配列 `array`の実際の要素型と互換性のある型のインスタンスであることを保証するランタイムチェックが暗黙的に含まれています。 メソッド `Main`では、`Fill` メソッドの最初の2回の呼び出しは成功しますが、3回目の呼び出しでは、`array(i)`への最初の割り当てを実行したときに `System.ArrayTypeMismatchException` 例外がスローされます。 この例外は、`Integer` を `String` 配列に格納できないために発生します。

配列要素の型の1つが、実行時に型が値型である型パラメーターである場合は、`System.InvalidCastException` 例外がスローされます。 例 :

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

また、配列のランクが同じ場合は、列挙型の配列と、列挙型の基になる型の配列または基になる型が同じ別の列挙型の配列の間にも、変換が存在します。

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

この例では、`Color` の配列は、`Byte`の配列、または `Color`の基になる型との間で変換されます。 ただし、`Integer` が `Color`の基になる型ではないため、`Integer`の配列への変換はエラーになります。

`A()` 型のランク1配列には、次のいずれかの条件を満たしている限り、`IList(Of B)`、`IReadOnlyList(Of B)`、`ICollection(Of B)`、`IReadOnlyCollection(Of B)`、`IEnumerable(Of B)` のコレクションインターフェイス型への配列変換も含まれます。`System.Collections.Generic`

- `A` と `B` は、値型として知られていない参照型または型パラメーターの両方を持ちます。および `A` には、`B`への拡大参照、配列、または型パラメーターの変換があります。もしくは
- `A` と `B` は両方とも同じ基になる型の列挙型です。もしくは
- `A` と `B` のいずれかが列挙型であり、もう1つは基になる型です。

任意のランクを持つ A 型の配列には、`System.Collections`で検出された非ジェネリックコレクションインターフェイス型 `IList`、`ICollection`、および `IEnumerable` への配列変換もあります。

`For Each`を使用するか、`GetEnumerator` メソッドを直接呼び出すことによって、結果のインターフェイスを反復処理することができます。 ランク1の配列で `IList` または `ICollection`の汎用形式または非ジェネリック形式が変換された場合は、インデックスを使用して要素を取得することもできます。 `IList`のジェネリックまたは非ジェネリック形式に変換されたランク1の配列の場合は、前に説明したのと同じランタイム配列の共変性のチェックに従って、インデックスを使用して要素を設定することもできます。 他のすべてのインターフェイスメソッドの動作は、VB 言語仕様によって定義されていません。これは、基になるランタイムによって作成されます。

## <a name="value-type-conversions"></a>値型の変換

値型の値は、その基本参照型または*ボックス*化と呼ばれるプロセスによって実装されるインターフェイス型のいずれかに変換できます。 値型の値がボックス化されている場合、値は .NET Framework ヒープ上に存在する場所からコピーされます。 次に、ヒープ上のこの場所への参照が返され、参照型の変数に格納できます。 この参照は、値型の*ボックス*化されたインスタンスとも呼ばれます。 ボックス化されたインスタンスは、値型ではなく、参照型と同じセマンティクスを持ちます。

ボックス化された値型は、*ボックス化解除*と呼ばれるプロセスを使用して、元の値型に変換できます。 ボックス化された値型がボックス化解除されると、値がヒープから変数の場所にコピーされます。 その時点から、値型であるかのように動作します。 値型のボックス化を解除するときは、値が null 値または値型のインスタンスである必要があります。 それ以外の場合は、`System.InvalidCastException` 例外がスローされます。 値が列挙型のインスタンスである場合、その値は列挙型の基になる型または基になる型が同じ別の列挙型にボックス化解除することもできます。 Null 値は、リテラル `Nothing`として扱われます。

Null 許容値型を適切にサポートするために、ボックス化とボックス化解除を行うときに、値型 `System.Nullable(Of T)` が特別に扱われます。 `Nullable(Of T)` 型の値をボックス化すると `T` 型のボックス化された値になります。値の `HasValue` プロパティが `True` の場合、または値の `Nothing` プロパティが `HasValue` の場合は `False`の値になります。 `T` 型の値のボックス化を解除すると、`Value` プロパティがボックス化された値であり、`HasValue` プロパティが `True`である `Nullable(Of T)` インスタンスの `Nullable(Of T)` 結果になります。 `Nothing` 値は、任意の `T` に対して `Nullable(Of T)` するようにボックス化を解除することができ、結果は `HasValue` プロパティが `False`の値になります。 ボックス化された値型は参照型と同じように動作するため、同じ値への複数の参照を作成することができます。 プリミティブ型と列挙型では、これらの型のインスタンスが*不変*であるため、これは関係ありません。 つまり、これらの型のボックス化されたインスタンスを変更することはできないため、同じ値への複数の参照があるという事実を観察することはできません。

一方、構造体は、そのインスタンス変数がアクセス可能である場合、またはそのメソッドやプロパティがインスタンス変数を変更する場合に、変更可能な場合があります。 ボックス化された構造体への1つの参照を使用して構造体を変更すると、ボックス化された構造体へのすべての参照に変更が表示されます。 この結果は予期しない結果になる可能性があるため、`Object` として型指定された値がある場所から別の場所にコピーされると、参照がコピーされるだけでなく、ヒープ上で自動的に複製されます。 例 :

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

プログラムの出力は次のようになります。

```console
Values: 0, 123
Refs: 123, 123
```

ローカル変数 `val2` のフィールドへの割り当ては、ローカル `val1` 変数のフィールドには影響しません。これは、ボックス化された `Struct1` が `val2`に割り当てられたときに、値のコピーが作成されたためです。 これに対して、割り当て `ref2.Value = 123` は、`ref1` と `ref2` の両方が参照するオブジェクトに影響します。

__付箋.__ `System.ValueType`から遅延バインドできないため、`System.ValueType` として入力されたボックス化された構造体の場合、構造のコピーは実行されません。

ルールには、ボックス化された値の型が割り当て時にコピーされるという例外が1つあります。 ボックス化された値型参照が別の型に格納されている場合、内部参照はコピーされません。 例 :

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

プログラムの出力は次のようになります。

```console
Values: 123, 123
```

これは、値をコピーするときに、ボックス化された内部の値がコピーされないためです。 したがって、`val1.Value` と `val2.Value` の両方に、同じボックス化された値型への参照があります。

__付箋.__ 内部のボックス化された値型がコピーされないという事実は、.NET 型システムの制限であり、`Object` コピーされた型の値が非常に高価になるたびに、すべての内部のボックス化された値型がコピーされるようにします。

既に説明したように、ボックス化された値型は、元の型にのみボックス化を解除できます。 ただし、ボックス化されたプリミティブ型は、`Object`として型指定されると、特別に扱われます。 これらは、変換先の他のプリミティブ型に変換できます。 例 :

```vb
Module Test
    Sub Main()
        Dim o As Object = 5
        Dim b As Byte = CByte(o)  ' Legal
        Console.WriteLine(b) ' Prints 5
    End Sub
End Module
```

通常、ボックス化された `Integer` 値 `5` を `Byte` 変数にボックス化解除できませんでした。 ただし、`Integer` と `Byte` はプリミティブ型であり、変換があるため、変換は許可されます。

値型をインターフェイスに変換することは、インターフェイスに制約される汎用引数とは異なることに注意してください。 制約された型パラメーターのインターフェイスメンバーにアクセスする (または `Object`のメソッドを呼び出す) 場合、ボックス化は、値型がインターフェイスに変換され、インターフェイスメンバーにアクセスするときと同様に発生します。 たとえば、インターフェイス `ICounter` に、値の変更に使用できるメソッド `Increment` が含まれているとします。 `ICounter` が制約として使用されている場合、`Increment` メソッドの実装は、ボックス化されたコピーではなく、`Increment` が呼び出された変数への参照を使用して呼び出されます。

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

`Increment` の最初の呼び出しでは `x`変数の値が変更されます。 これは、`x`のボックス化されたコピーの値を変更する `Increment`への2回目の呼び出しとは同じではありません。 このため、プログラムの出力は次のようになります。

```console
0
1
1
```

### <a name="nullable-value-type-conversions"></a>Null 許容値型の変換

`T` 値型は、型の null 許容バージョンとの間で変換を行うことができます `T?`です。 変換される値が `Nothing`場合、`T?` から `T` への変換は `System.InvalidOperationException` 例外をスローします。 また、`T` に `S`への組み込みの変換がある場合、`T?` は型 `S` に変換されます。 `S` が値型の場合、`T?` と `S?`の間には次の組み込み変換が存在します。

* `T?` から `S?`への同じ分類 (縮小または拡大) の変換。

* `T` から `S?`への同じ分類 (縮小または拡大) の変換。

* `S?` から `T`への縮小変換。

たとえば、組み込みの拡大変換は `Integer?` から `Long?` に存在します。これは、組み込みの拡大変換が `Integer` から `Long`に存在するためです。

```vb
Dim i As Integer? = 10
Dim l As Long? = i
```

`T?` から `S?`に変換する場合、`T?` の値が `Nothing`の場合、`S?` の値は `Nothing`になります。 `S?` から `T` または `T?` から `S`に変換する場合、`T?` または `S?` の値が `Nothing`の場合は、`System.InvalidCastException` 例外がスローされます。

基になる型 `System.Nullable(Of T)`の動作により、null 許容型の値型 `T?` がボックス化された場合、結果は `T?`型のボックス化された値ではなく、`T`型のボックス化された値になります。 逆に、null 許容値型 `T?`にボックス化を解除すると、値は `System.Nullable(Of T)`によってラップされ、`Nothing` は `T?`型の null 値にボックス化解除されます。 例 :

```vb
Dim i1? As Integer = Nothing
Dim o1 As Object = i1

Console.WriteLine(o1 Is Nothing)                    ' Will print True
o1 = 10
i1 = CType(o1, Integer?)
Console.WriteLine(i1)                               ' Will print 10
```

この動作の副作用として、`T`のすべてのインターフェイスを実装するために null 許容型 `T?` が表示されます。これは、値型をインターフェイスに変換するには、型をボックス化する必要があるためです。 その結果、`T?` は、`T` 変換可能なすべてのインターフェイスに変換できます。 ただし、null 許容型 `T?` では、ジェネリック制約チェックまたはリフレクションのために、実際には `T` のインターフェイスが実装されていないことに注意する必要があります。 例 :

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

`Char` を `String` に変換すると、最初の文字が文字値である文字列が生成されます。 `String` を `Char` に変換すると、文字列の最初の文字が値である文字が返されます。 `Char` の配列を `String` に変換すると、配列の要素を文字として持つ文字列が生成されます。 `String` を `Char` の配列に変換すると、文字列の文字を要素として持つ文字の配列が生成されます。

`String` と `Boolean`、`Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`、`Decimal`、`Single`、`Double`、`Date`、およびその逆の変換は、この仕様の範囲を超えており、1つの詳細を除き、実装に依存します。 文字列変換では、常に実行時環境の現在のカルチャが考慮されます。 そのため、実行時に実行する必要があります。

## <a name="widening-conversions"></a>拡大変換

拡大変換ではオーバーフローは発生しませんが、精度が失われる可能性があります。 次の変換は、拡大変換です。

__Id/既定の変換__

* 型からそれ自体へ。

* 同じシグネチャを持つ任意のデリゲート型に対してラムダメソッドを再分類するために生成された匿名デリゲート型から。

* リテラルから型への `Nothing`。

__数値変換__

* `Byte` から `UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`、`Decimal`、`Single`、`Double`になります。

* `SByte` から `Short`、`Integer`、`Long`、`Decimal`、`Single`、`Double`になります。

* `UShort` から `UInteger`、`Integer`、`ULong`、`Long`、`Decimal`、`Single`、`Double`になります。

* `Short` から `Integer`、`Long`、`Decimal`、`Single` または `Double`です。

* `UInteger` から `ULong`、`Long`、`Decimal`、`Single`、`Double`になります。

* `Integer` から `Long`、`Decimal`、`Single` または `Double`ます。

* `ULong` から `Decimal`、`Single`、または `Double`。

* `Long` から `Decimal`、`Single` または `Double`します。

* `Decimal` から `Single` または `Double`ます。

* `Single` から `Double`にします。

* リテラルから列挙型への `0`。 (__注:__ `0` から任意の列挙型への変換は、テストフラグを簡略化するために拡大されます。 たとえば、`Values` が `One`値を持つ列挙型である場合、`(v And Values.One) = 0`を指定して `Values` 型の変数 `v` をテストできます。)

* 列挙型から基になる数値型、または基になる数値型からへの拡大変換がある数値型。

* 定数式の値が変換先の型の範囲内にある場合、`ULong`型の定数式、`Long`、`UInteger`、`Integer`、`UShort`、`Short`、`Byte`、または `SByte` をより狭い型に変換します。 (__注:__ `UInteger` または `Integer` から `Single`、`ULong` または `Long` に変換して `Single` または `Double`に変換したり、`Decimal` または `Single` に変換したりすると、精度が失われる可能性がありますが、マグニチュードが失われることはありません。`Double` その他の拡大数値変換では、情報が失われることはありません)。

__参照変換__

* 参照型から基本型への。

* 型がインターフェイスまたは variant 互換インターフェイスを実装している場合は、参照型からインターフェイス型への。

* インターフェイス型から `Object`にします。

* インターフェイス型から variant 互換のインターフェイス型へ。

* デリゲート型から variant 互換のデリゲート型へ。 (__注:__ その他の多くの参照変換は、これらの規則によって暗黙的に示されます。 たとえば、匿名デリゲートは `System.MulticastDelegate`から継承する参照型です。配列型は `System.Array`から継承する参照型です。匿名型は、`System.Object`から継承する参照型です)。

__匿名デリゲートの変換__

* ラムダメソッドをより広範なデリゲート型に再分類するために生成された匿名デリゲート型から。

__配列の変換__

* 要素型を `Se` 持つ `S` 配列型から、要素型 `Te`を持つ `T` 配列型には、次のすべてが true であることが条件となります。

  * `S` と `T` は要素の型のみが異なります。

  * `Se` と `Te` はどちらも参照型であるか、参照型であると認識される型パラメーターです。

  * 拡大参照、配列、または型パラメーターの変換が `Se` から `Te`に存在します。

* 配列型から `S` 列挙された要素型を使用して、次のすべての条件を満たしている場合は、要素型 `Te`で `T` 配列型に `Se` します。

  * `S` と `T` は要素の型のみが異なります。

  * `Te` は `Se`の基になる型です。

* 次のいずれかの条件に該当する場合は、ランク1の `S` 配列型から、列挙された要素型 `Se`、`System.Collections.Generic.IList(Of Te)`、`IReadOnlyList(Of Te)`、`ICollection(Of Te)`、`IReadOnlyCollection(Of Te)`、および `IEnumerable(Of Te)`を使用します。

  * `Se` と `Te` はどちらも参照型であるか、参照型であると認識される型パラメーターであり、拡大参照、配列、または型パラメーターの変換は `Se` から `Te`に存在します。もしくは

  * `Te` は `Se`の基になる型です。もしくは

  * `Te` は `Se` と同じ

__値型の変換__

* 値型から基本型への。

* 値型から、型が実装するインターフェイス型になります。

__Null 許容値型の変換__

* 型から `T?`型に `T` ます。

* 型から `S?`型に `T?` ます。 `T` 型から `S`型への拡大変換があります。

* 型から `S?`型に `T` ます。 `T` 型から `S`型への拡大変換があります。

* 型から、型 `T` が実装するインターフェイス型に `T?` ます。

__文字列変換__

* `Char` から `String`にします。

* `Char()` から `String`にします。

__型パラメーターの変換__

* 型パラメーターから `Object`にします。

* 型パラメーターからインターフェイス型制約、またはインターフェイス型制約と互換性のあるインターフェイスバリアント。

* 型パラメーターから、クラス制約によって実装されるインターフェイスへの。

* 型パラメーターから、クラス制約によって実装されるインターフェイスと互換性のあるインターフェイスバリアントへの通信。

* 型パラメーターからクラス制約への、またはクラス制約の基本型。

* 型パラメーターから `T` 型パラメーターの制約 `Tx`、またはすべての `Tx` に対してへの拡大変換が含まれています。

## <a name="narrowing-conversions"></a>縮小変換

縮小変換は、常に成功するとは限りません。情報が失われる可能性がある変換、型のドメイン間での変換は、縮小表記という意味で十分に異なる変換です。 次の変換は縮小変換として分類されます。

__ブール型変換__

* `Boolean` から `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`、`Decimal`、`Single`、`Double`になります。

* `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`、`Decimal`、`Single`、`Double` を `Boolean`します。

__数値変換__

* `Byte` から `SByte`にします。

* `SByte` から `Byte`、`UShort`、`UInteger`、または `ULong`になります。

* `UShort` から `Byte`、`SByte`、または `Short`。

* `Short` から `Byte`、`SByte`、`UShort`、`UInteger`、`ULong`になります。

* `UInteger` から `Byte`、`SByte`、`UShort`、`Short`、`Integer`になります。

* `Integer` から `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`ULong`になります。

* `ULong` から `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`Long`になります。

* `Long` から `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`になります。

* `Decimal` から `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`になります。

* `Single` から `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`、`Decimal`になります。

* `Double` から `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`、`Decimal`、`Single`になります。

* 数値型から列挙型への。

* 列挙型から数値型への変換では、基になる数値型がに縮小変換されます。

* 列挙型から別の列挙型へ。

__参照変換__

* 参照型からより派生した型への。

* クラス型がインターフェイス型またはそれと互換性のあるインターフェイス型バリアントを実装していない場合、クラス型からインターフェイス型になります。

* インターフェイス型からクラス型へ。

* 2つの型の間に継承関係がなく、バリアントとの互換性がない場合は、インターフェイス型から別のインターフェイス型にします。

__匿名デリゲートの変換__

* 任意の幅の狭いデリゲート型に再分類するラムダメソッドに対して生成された匿名デリゲート型から。

__配列の変換__

* 次のすべての条件に該当する場合は、要素型が `Se`である `S` 配列型から、要素型が `Te`の配列型に `T` ます。

  * `S` と `T` は要素の型のみが異なります。
  * `Se` と `Te` はどちらも参照型であるか、または値型として知られていない型パラメーターです。
  * `Se` から `Te`への縮小参照、配列、または型パラメーターの変換が存在します。

* 次のすべての条件に該当する場合は、要素型を持つ `S` 配列型から、列挙された要素型 `Te`を持つ `T` 配列型に `Se` ます。

  * `S` と `T` は要素の型のみが異なります。
  * `Se` は `Te` の基になる型であるか、または基になる型が同じである列挙型が異なる場合があります。

* 次のいずれかに該当する場合は、ランク1の `S` 配列型から、列挙された要素型 `Se`、`IList(Of Te)`、`IReadOnlyList(Of Te)`、`ICollection(Of Te)`、`IReadOnlyCollection(Of Te)`、および `IEnumerable(Of Te)`になります。

  * `Se` と `Te` はどちらも参照型であるか、参照型であると認識される型パラメーターであり、縮小参照、配列、または型パラメーターの変換は `Se` から `Te`に存在します。もしくは
  * `Se` は `Te`の基になる型であるか、または基になる型が同じである列挙型が異なる場合があります。

__値型の変換__

* 参照型からより多くの派生値型へ。

* 値型がインターフェイス型を実装している場合は、インターフェイス型から値型になります。

__Null 許容値型の変換__

* 型から `T`型に `T?` ます。

* 型から `S?`型に `T?` します。 `T` 型から `S`型への縮小変換が存在します。

* 型から `S?`型に `T` します。 `T` 型から `S`型への縮小変換が存在します。

* 型から `T`型に `S?` ます。 `S` 型から `T`型への変換があります。

__文字列変換__

* `String` から `Char`にします。

* `String` から `Char()`にします。

* `String` から `Boolean`、`Boolean` から `String`にします。

* `String` と `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`、`Decimal`、`Single`、`Double`間の変換。

* `String` から `Date`、`Date` から `String`にします。

__型パラメーターの変換__

* `Object` から型パラメーターへ。

* 型パラメーターがインターフェイスに制約されていない場合、または、そのインターフェイスを実装するクラスに制約されている場合、型パラメーターからインターフェイス型になります。

* インターフェイス型から型パラメーターへ。

* 型パラメーターから、クラス制約の派生型にします。

* 型パラメーターから `T` 型パラメーターの制約には、への縮小変換が `Tx` れます。

## <a name="type-parameter-conversions"></a>型パラメーターの変換

型パラメーターの変換は、制約 (存在する場合) によって決定されます。 型パラメーター `T` は、任意のインターフェイス型に対して常に、または `Object`との間でいつでも変換できます。 型 `T` が実行時に値型である場合、`T` から `Object` またはインターフェイス型への変換はボックス化変換であり、`Object` またはインターフェイス型から `T` への変換はボックス化変換となります。 `C` クラス制約を持つ型パラメーターは、型パラメーターから `C` とその基本クラスへの追加の変換、およびその逆の変換を定義します。 型パラメーターの制約を持つ `T` 型パラメーターは、`Tx` への変換と、に変換 `Tx` すべてを定義 `Tx` ます。

要素型がインターフェイス制約を持つ型パラメーターである配列は、型パラメーターにも `Class` またはクラスの制約 (参照配列要素型のみを共変にすることができるため) を持つ `I`配列と同じ共変配列変換を `I` します。 要素型がクラス制約を持つ型パラメーターである配列は、要素型が `C`配列と同じ共変配列変換を `C` ます。

上記の変換規則では、制約のない型パラメーターから非インターフェイス型への変換は許可されていません。 その理由は、このような変換のセマンティクスについて混乱が生じないようにするためです。 たとえば、次のような宣言があるとします。

```vb
Class X(Of T)
    Public Shared Function F(t As T) As Long 
        Return CLng(t)    ' Error, explicit conversion not permitted
    End Function
End Class
```

`T` から `Integer` への変換が許可されている場合は、`X(Of Integer).F(7)` が `7L`を返すことが簡単であると考えられます。 ただし、数値変換はコンパイル時に型が数値であることがわかっている場合にのみ考慮されます。 このようなセマンティクスを明確にするために、上記の例を記述する必要があります。

```vb
Class X(Of T)
    Public Shared Function F(t As T) As Long
        Return CLng(CObj(t))    ' OK, conversions permitted
    End Function
End Class
```

## <a name="user-defined-conversions"></a>ユーザー定義の変換

*組み込み変換*は、言語によって定義された変換 (この仕様ではリスト) であり、*ユーザー定義の変換*は `CType` 演算子のオーバーロードによって定義されます。 型の間で変換を行う場合、組み込みの変換が適用されない場合は、ユーザー定義の変換が考慮されます。 ソースとターゲットの型に*最も固有*なユーザー定義の変換がある場合は、ユーザー定義の変換が使用されます。 それ以外の場合は、コンパイル時のエラーが発生します。 最も具体的な変換は、変換元の型に "最も近い" オペランドがあり、結果の型がターゲットの型に "最も近い" である変換です。 使用するユーザー定義の変換を決定するときに、最も具体的な拡大変換が使用されます。拡大変換が最も限定的でない場合は、最も限定的な縮小変換が使用されます。 特定の縮小変換がない場合、変換は定義されず、コンパイル時エラーが発生します。

以下のセクションでは、最も具体的な変換がどのように決定されるかについて説明します。 次の用語を使用します。

型 `A` から `B`型に組み込みの拡大変換が存在し、`A` も `B` もインターフェイスではない場合、 *`A` は `B`* に含まれ、`B` は `A`を*含み*ます。

型のセットの中で*最も外側*にある型は、セット内の他のすべての型を含む1つの型です。 1つの型に他のすべての型が含まれていない場合、そのセットには最も外側の型がありません。 直感的に言うと、最も外側の型は、セット内の "最大" 型です。これは、他の型を拡大変換することによって変換できる1つの型です。

型のセットの中で*最も内側*にある型は、セット内の他のすべての型に包含されている型です。 他のすべての型に包含されている型がない場合は、そのセットに最も包含されていない型はありません。 直感的に言うと、最も包含されている型は、セット内の "最小" 型です。これは、縮小変換を通じて他の型に変換できる1つの型です。

`T?`型のユーザー定義候補の候補を収集する場合、`T` で定義されたユーザー定義の変換演算子が代わりに使用されます。 変換先の型が null 許容の値型でもある場合、null 非許容の値型のみを含む、`T`のユーザー定義変換演算子はすべてリフトされます。 `T` から `S` への変換演算子は、`T?` から `S?` への変換として解除され、必要に応じて `T?` から `T`に変換し、`T` を `S` に変換することによって評価されます。`S``S?` ただし、変換される値が `Nothing`場合、リフトされた変換演算子は `S?`として型指定された `Nothing` の値に直接変換します。 例 :

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

変換を解決する際には、リフトされた変換演算子よりもユーザー定義の変換演算子を使用することをお勧めします。 例 :

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

実行時にユーザー定義の変換を評価するには、次の3つの手順を実行します。

1. まず、必要に応じて、組み込みの変換を使用して、変換元の型からオペランドの型に値を変換します。

2. 次に、ユーザー定義の変換が呼び出されます。

3. 最後に、必要に応じて、組み込みの変換を使用して、ユーザー定義の変換の結果を対象の型に変換します。

ユーザー定義の変換の評価では、複数のユーザー定義変換演算子が含まれないことに注意する必要があります。

### <a name="most-specific-widening-conversion"></a>最も限定的な拡大変換

次の手順を使用して、2つの型の間で、ユーザー定義の拡大変換演算子のうち最も限定的なものを決定します。

1. まず、候補となる変換演算子がすべて収集されます。 候補の変換演算子は、ソース型のすべてのユーザー定義の拡大変換演算子と、対象の型のすべてのユーザー定義の拡大変換演算子です。

2. 次に、適用できないすべての変換演算子がセットから削除されます。 変換演算子は、ソース型からオペランド型への組み込みの拡大変換演算子が存在し、演算子の結果からターゲット型への組み込みの拡大変換演算子が存在する場合に、ソースの型とターゲットの型に適用されます。 適用可能な変換演算子がない場合、特定の拡大変換は行われません。

3. 次に、適用可能な変換演算子の最も限定的なソースの種類が決定されます。

   * 変換演算子のいずれかが変換元の型から直接変換する場合、変換元の型は最も限定的なソースの種類です。

   * それ以外の場合、最も限定的なソースの種類は、変換演算子のソース型の組み合わせで最も包含される型です。 包含されている型の中に最も多くの包含型が見つからない場合は、特に特定の拡大変換はありません。

4. 次に、適用可能な変換演算子の最も限定的なターゲットの種類を決定します。

   * 変換演算子のいずれかがターゲット型に直接変換される場合、対象の型は最も限定的なターゲット型です。

   * それ以外の場合、最も限定的なターゲット型は、変換演算子の対象の型の組み合わせで最も外側にある型になります。 最も外側の型が見つからない場合、特定の拡大変換は行われません。

5. その後、ただ1つの変換演算子が最も具体的な変換元の型から最も限定的なターゲット型に変換する場合は、これが最も具体的な変換演算子です。 このような演算子が複数存在する場合、特定の拡大変換は行われません。

### <a name="most-specific-narrowing-conversion"></a>最も限定的な縮小変換

次の手順に従って、2つの型の間で最も限定的なユーザー定義の縮小変換演算子を決定します。

1. まず、候補となる変換演算子がすべて収集されます。 候補の変換演算子は、変換元の型のすべてのユーザー定義変換演算子と、対象の型のすべてのユーザー定義変換演算子です。

2. 次に、適用できないすべての変換演算子がセットから削除されます。 変換演算子は、ソース型からオペランド型への組み込みの変換演算子が存在し、演算子の結果からターゲット型への組み込みの変換演算子がある場合に、ソース型およびターゲット型に適用されます。 適用可能な変換演算子がない場合、特定の縮小変換は行われません。

3. 次に、適用可能な変換演算子の最も限定的なソースの種類が決定されます。

   * 変換演算子のいずれかが変換元の型から直接変換する場合、変換元の型は最も限定的なソースの種類です。

   * それ以外の場合、変換演算子のいずれかが、変換元の型を含む型から変換する場合、最も限定的なソース型は、それらの変換演算子のソース型の組み合わせに含まれる最も包含された型になります。 包含されていない型がほとんど見つからない場合、特定の縮小変換は行われません。

   * それ以外の場合、最も限定的なソースの種類は、変換演算子のソース型の組み合わせで最も外側にある型になります。 最も外側の型が見つからない場合、特定の縮小変換は行われません。

4. 次に、適用可能な変換演算子の最も限定的なターゲットの種類を決定します。

   * 変換演算子のいずれかがターゲット型に直接変換される場合、対象の型は最も限定的なターゲット型です。

   * それ以外の場合、変換演算子のいずれかが、対象の型によって包含されている型に変換される場合、最も限定的なターゲット型は、それらの変換演算子のソース型の組み合わせで最も外側にある型になります。 最も外側の型が見つからない場合、特定の縮小変換は行われません。

   * それ以外の場合、最も限定的なターゲット型は、変換演算子の対象の型の組み合わせで最も包含される型です。 包含されていない型がほとんど見つからない場合、特定の縮小変換は行われません。

5. その後、ただ1つの変換演算子が最も具体的な変換元の型から最も限定的なターゲット型に変換する場合は、これが最も具体的な変換演算子です。 このような演算子が複数存在する場合、特定の縮小変換は行われません。

## <a name="native-conversions"></a>ネイティブ変換

一部の変換は、.NET Framework によってネイティブにサポートされているため、*ネイティブ変換*として分類されます。 これらの変換は、`DirectCast` および `TryCast` 変換演算子とその他の特殊な動作を使用することによって最適化できるものです。 ネイティブ変換として分類される変換は、id 変換、既定の変換、参照変換、配列の変換、値型の変換、および型パラメーターの変換です。

## <a name="dominant-type"></a>優先する型

型のセットを指定した場合、通常は、型の推定などの状況で、セットの優先度の*高い型*を判断する必要があります。 型のセットの優先される型は、1つ以上の他の型が暗黙的に変換されていない型を最初に削除することによって決定されます。 この時点で型がない場合は、優先度の高い型はありません。 最も優先される型は、残りの型の最も内側の型になります。 最も包含されている型が複数ある場合は、優先度の高い型はありません。
