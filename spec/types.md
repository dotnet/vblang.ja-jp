---
ms.openlocfilehash: 6815d084e180bb615fcc5c880e2ee5127b56c4a4
ms.sourcegitcommit: 6eca149bdc736113e0adb709212bd266c9503c33
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2019
ms.locfileid: "47426768"
---
# <a name="types"></a>種類

Visual Basic における型の 2 つの基本的なカテゴリは*値の型*と*参照型*します。 プリミティブ型 (文字列) を除く、列挙型、および構造体には値の種類です。 クラス、文字列、標準的なモジュール、インターフェイス、配列、およびデリゲートは、参照型です。

すべての型が、*既定値*、これは、初期化時にその型の変数に割り当てられている値。

```antlr
TypeName
    : ArrayTypeName
    | NonArrayTypeName
    ;

NonArrayTypeName
    : SimpleTypeName
    | NullableTypeName
    ;

SimpleTypeName
    : QualifiedTypeName
    | BuiltInTypeName
    ;

QualifiedTypeName
    : Identifier TypeArguments? (Period IdentifierOrKeyword TypeArguments?)*
    | 'Global' Period IdentifierOrKeyword TypeArguments?
      (Period IdentifierOrKeyword TypeArguments?)*
    ;

TypeArguments
    : OpenParenthesis 'Of' TypeArgumentList CloseParenthesis
    ;

TypeArgumentList
    : TypeName ( Comma TypeName )*
    ;

BuiltInTypeName
    : 'Object'
    | PrimitiveTypeName
    ;

TypeModifier
    : AccessModifier
    | 'Shadows'
    ;

IdentifierModifiers
    : NullableNameModifier? ArrayNameModifier?
    ;
```

## <a name="value-types-and-reference-types"></a>値型と参照型

値型と参照型指定できますが、宣言の構文と使用状況と同様なセマンティクスが異なります。

参照型は、ランタイム ヒープに格納されています。また、その記憶域への参照によってのみアクセスできます。 参照型は、参照を使用して常にアクセスされる、ため、その有効期間は、.NET Framework によって管理されます。 特定のインスタンスに未解決の参照を追跡し、残っている参照がない場合にのみ、インスタンスは破棄されます。 参照型の変数には、その型の値より強い派生型の値または null 値への参照が含まれています。 A *null 値*指す何もありません。 代入以外、null 値を持つことが何もすることはできません。 参照型の変数への代入では、参照されている値のコピーではなく、参照のコピーを作成します。 参照型の変数は、既定値は、null 値は。

値の型が直接アレイ内または別の型では、スタックに格納されています。その記憶域は、直接のみアクセスできます。 値の型は変数内に直接格納されている、ため、有効期間は、それを含む変数の有効期間によって決まります。 値型のインスタンスを格納している場所が破棄されると、値型のインスタンスは破棄されます。 値の型は直接は常にアクセスします。値型への参照を作成することはできません。 このような参照を禁止することと、破棄されている値クラスのインスタンスを指すため不可能になります。 値の型が常にため`NotInheritable`値型の変数には常にその型の値が含まれています。 このため、値型の値が null の値にすることはできません。 またより強い派生型のオブジェクトを参照することができます。 値型の変数への代入では、割り当てられている値のコピーを作成します。 値型の変数は、既定値は、その既定値の型の各変数のメンバーの初期化の結果は。

次の例は、参照型と値の型の違いを示しています。

```vb
Class Class1
    Public Value As Integer = 0
End Class

Module Test
    Sub Main()
        Dim val1 As Integer = 0
        Dim val2 As Integer = val1
        val2 = 123
        Dim ref1 As Class1 = New Class1()
        Dim ref2 As Class1 = ref1
        ref2.Value = 123
        Console.WriteLine("Values: " & val1 & ", " & val2)
        Console.WriteLine("Refs: " & ref1.Value & ", " & ref2.Value)
    End Sub
End Module
```

プログラムの出力は次のとおりです。

```
Values: 0, 123
Refs: 123, 123
```

ローカル変数に代入`val2`ローカル変数には影響しません`val1`両方のローカル変数は値型であるため (種類`Integer`) 値の型のそれぞれのローカル変数が独自のストレージとします。 これに対して、割り当て`ref2.Value = 123;`オブジェクトに影響を両方`ref1`と`ref2`参照。

.NET Framework の型システムについて注意が必要ですがいる場合でも構造体、列挙型とプリミティブ型 (以外の`String`) は、値の型すべて参照型から継承します。 構造体およびプリミティブ型が参照型から継承`System.ValueType`から継承される`Object`します。 列挙型が参照型から継承`System.Enum`から継承される`System.ValueType`します。

### <a name="nullable-value-types"></a>null 許容値型

値型の場合、`?`修飾子を表す型名に追加できる、 *null 許容*その型のバージョン。

```antlr
NullableTypeName
    : NonArrayTypeName '?'
    ;

NullableNameModifier
    : '?'
    ;
```

Null 許容値型は、型の null 非許容のバージョンと同じ値と null 値に含めることができます。 そのため、null 許容値型の割り当て`Nothing`型の変数に null 値にゼロ値ではなく、値型の変数の値を設定します。 例:

```vb
Dim x As Integer = Nothing
Dim y As Integer? = Nothing

' Prints zero
Console.WriteLine(x)
' Prints nothing (because the value of y is the null value)
Console.WriteLine(y)
```

変数名を null 許容型修飾子を配置することで null 許容値型である変数を宣言することも。 わかりやすくするため、同じ宣言で変数名と型名の両方で null 許容型修飾子があることはできません。 Null 許容型は、型を使用して実装されるため`System.Nullable(Of T)`、型`T?`型には`System.Nullable(Of T)`、2 つの名前を同じ意味で使用できます。 `?`修飾子は、null 許容の型に配置できません。 そのため、型を宣言することはできません`Integer??`または`System.Nullable(Of Integer)?`します。

Null 許容値型`T?`のメンバーを持つ`System.Nullable(Of T)`演算子または変換と*リフト*基になる型から`T`型に`T?`します。 ほとんどの場合、null 非許容値型の null 許容値型の置換コピー演算子と、基になる型から変換を変換します。 これにより、同じ変換とに適用される操作の多く`T`に適用する`T?`もします。


## <a name="interface-implementation"></a>インターフェイスの実装

構造体とクラスの宣言が 1 つ以上のインターフェイス型のセットを実装することを宣言`Implements`句。

```antlr
TypeImplementsClause
    : 'Implements' TypeImplements StatementTerminator
    ;

TypeImplements
    : NonArrayTypeName ( Comma NonArrayTypeName )*
    ;
```

指定されたすべての型、`Implements`句は、インターフェイスである必要があり、型がインターフェイスのすべてのメンバーを実装する必要があります。 例:

```vb
Interface ICloneable
    Function Clone() As Object
End Interface 

Interface IComparable
    Function CompareTo(other As Object) As Integer
End Interface 

Structure ListEntry
    Implements ICloneable, IComparable

    ...

    Public Function Clone() As Object Implements ICloneable.Clone
        ...
    End Function 

    Public Function CompareTo(other As Object) As Integer _
        Implements IComparable.CompareTo
        ...
    End Function 
End Structure
```

暗黙的にインターフェイスを実装する型では、すべてのインターフェイスの基本インターフェイスを実装します。 これは、型がですべての基本インターフェイスを明示的に表示されない場合でも true、`Implements`句。 この例で、`TextBox`両方を実装して`IControl`と`ITextBox`します。

```vb
Interface IControl
    Sub Paint()
End Interface 

Interface ITextBox
    Inherits IControl

    Sub SetText(text As String)
End Interface 

Structure TextBox
    Implements ITextBox

    ...

    Public Sub Paint() Implements ITextBox.Paint
        ...
    End Sub

    Public Sub SetText(text As String) Implements ITextBox.SetText
        ...
    End Sub
End Structure
```

宣言の型がインターフェイスを実装するの場合、型の宣言領域には何も宣言していません。 そのため、これはメソッドを使用して、同じ名前で 2 つのインターフェイスを実装するために有効です。

型はスコープ内の型パラメーターがありますが、独自の型パラメーターを実装できません。

```vb
Class C1(Of V)
    Implements V  ' Error, can't implement type parameter directly
    Implements IEnumerable(Of V)  ' OK, not directly implementing

    ...
End Class
```

ジェネリック インターフェイスは実装されている複数の異なる型引数を使用して回を指定できます。 ただし、ジェネリック型は、(型制約) に関係なく指定した型パラメーターがそのインターフェイスの別の実装と重複する場合は、型パラメーターを使用してジェネリック インターフェイスを実装することはできません。 例:

```vb
Interface I1(Of T)
End Interface

Class C1
    Implements I1(Of Integer)
    Implements I1(Of Double)    ' OK, no overlap
End Class

Class C2(Of T)
    Implements I1(Of Integer)
    Implements I1(Of T)         ' Error, T could be Integer
End Class
```


## <a name="primitive-types"></a>プリミティブ型

*プリミティブ型*、定義済みのエイリアスであるキーワードを使って識別型、`System`名前空間。 プリミティブ型は型の完全に区別は、エイリアス: 予約語の書き込み`Byte`が正確に記述と同じ`System.Byte`します。 プリミティブ型とも呼ばれます*組み込み型*します。

```antlr
PrimitiveTypeName
    : NumericTypeName
    | 'Boolean'
    | 'Date'
    | 'Char'
    | 'String'
    ;

NumericTypeName
    : IntegralTypeName
    | FloatingPointTypeName
    | 'Decimal'
    ;

IntegralTypeName
    : 'Byte' | 'SByte' | 'UShort' | 'Short' | 'UInteger'
    | 'Integer' | 'ULong' | 'Long'
    ;

FloatingPointTypeName
    : 'Single' | 'Double'
    ;
```


プリミティブ型の別名の通常の型は、ため、すべてのプリミティブ型はメンバーがあります。 たとえば、`Integer`で宣言されたメンバーを持つ`System.Int32`します。 リテラルは、対応する型のインスタンスとして扱うことができます。

* プリミティブ型は、特定の追加操作を行うことで他の構造体の型とは異なります。

* プリミティブ型では、値のリテラルを記述することで作成されることを許可します。 たとえば、`123I`型のリテラルは、`Integer`します。

* プリミティブ型の定数を宣言することになります。

* すべてのプリミティブ型の定数を式のオペランドには、コンパイラはコンパイル時に式の評価になります。 このような式は定数式と呼ばれます。

Visual Basic では、次のプリミティブ型を定義します。

* 整数値の型`Byte`(1 バイトの符号なし整数)、 `SByte` (1 バイトの符号付き整数)、 `UShort` (2 バイト符号なし整数)、 `Short` (2 バイト符号付き整数)、 `UInteger` (4 バイト符号なし整数)、 `Integer` (4 バイト符号付き整数)、 `ULong` (8 バイト符号なし整数)、および`Long`(8 バイト符号付き整数)。 これらの型にマップ`System.Byte`、 `System.SByte`、 `System.UInt16`、 `System.Int16`、 `System.UInt32`、 `System.Int32`、`System.UInt64`と`System.Int64`、それぞれします。 整数型の既定値は、リテラルに相当`0`します。

* 浮動小数点値の型`Single`(4 バイト浮動小数点) と`Double`(8 バイト浮動小数点)。 これらの型にマップ`System.Single`と`System.Double`、それぞれします。 浮動小数点型の既定値は、リテラルに相当`0`します。

* `Decimal`型 (16 バイト 10 進値) にマップする`System.Decimal`します。 10 進数の既定値は、リテラルに相当`0D`します。

* `Boolean`値の型で、リレーショナルまたは論理操作の結果では通常、実際の値を表します。 型のリテラルは、`System.Boolean`します。 既定値、`Boolean`型がリテラルと等価`False`します。

* `Date`値の型を日付や時刻を表します。 マップ`System.DateTime`します。 既定値、`Date`型がリテラルと等価`# 01/01/0001 12:00:00AM #`します。

* `Char`値を 1 つの Unicode 文字を表します。 マップの種類、`System.Char`します。 既定値、`Char`型が定数式と等価`ChrW(0)`します。

* `String`参照型では、Unicode 文字のシーケンスを表しにマップする`System.String`します。 既定値、`String`型が null の値。



## <a name="enumerations"></a>列挙

*列挙体*値の型から継承するは`System.Enum`一連のプリミティブの整数型のいずれかの値を表します。

```antlr
EnumDeclaration
    : Attributes? TypeModifier* 'Enum' Identifier
      ( 'As' NonArrayTypeName )? StatementTerminator
      EnumMemberDeclaration+
      'End' 'Enum' StatementTerminator
    ;
```

列挙型の`E`、既定値は、式によって生成される値`CType(0, E)`します。

列挙体の基になる型は、列挙で定義されているすべての列挙子の値を表す整数型である必要があります。 基になる型が指定されている場合があります`Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、 `ULong`、 `Long`、またはいずれかに対応する型、 `System`名前空間。 基になる型が明示的に指定されていない場合、既定値は`Integer`します。

次の例では、基になる型の列挙体の宣言`Long`:

```vb
Enum Color As Long
    Red
    Green
    Blue
End Enum
```

開発者は、基になる型を使用することができます`Long`などの範囲内にある値の使用を有効にするには、`Long`の範囲内にありませんが、 `Integer`、または将来には、このオプションを保持するためにします。


### <a name="enumeration-members"></a>列挙型メンバー

列挙体のメンバーは、列挙体で宣言された列挙値およびクラスから継承されたメンバー`System.Enum`します。

列挙体のメンバーのスコープは、列挙型宣言の本体です。 意味する列挙体の宣言の外側列挙体のメンバーする必要があります常に修飾 (種類が名前空間インポートを使用して、名前空間にインポートされていない場合)。

定数式の値を省略すると、列挙メンバーの宣言の順序は重要です。 列挙型メンバーが暗黙的が`Public`のみへのアクセスは、列挙体メンバーの宣言でアクセス修飾子は許可されません。

```antlr
EnumMemberDeclaration
    : Attributes? Identifier ( Equals ConstantExpression )? StatementTerminator
    ;
```

### <a name="enumeration-values"></a>列挙値

列挙体のメンバー一覧の列挙値は、基になる列挙型として型指定された定数と定数が必要な任意の場所に表示されることができますに宣言されます。 列挙メンバーの定義に`=`定数式で表される値に関連付けられているメンバーを提供します。 定数式では、基になる型に暗黙的に変換できる整数型に評価される必要があり、基になる型で表現できる値の範囲内である必要があります。 に、次の例がエラーでは、定数値`1.5`、 `2.3`、および`3.3`は、基になる整数型に暗黙的に変換できません`Long`厳密なセマンティクスを持つ。

```vb
Option Strict On

Enum Color As Long
    Red = 1.5
    Green = 2.3
    Blue = 3.3
End Enum
```

次に示すよう、複数の列挙体メンバーは、同じ関連付けられた値を共有可能性があります。

```vb
Enum Color
    Red
    Green
    Blue
    Max = Blue
End Enum
```

-2 つの列挙体メンバーを持つ列挙体の例を示します`Blue`と`Max`--ことが同じ関連付けられている値。

列挙体の最初の列挙子の値の定義が初期化子を持たない場合、対応する定数の値は`0`します。 初期化子のない列挙値の定義は、列挙子が前の列挙値の値を増やすことで得られた値`1`します。 この増加する値は、基になる型で表すことができる値の範囲内にある必要があります。

```vb
Enum Color
    Red
    Green = 10
    Blue
End Enum 

Module Test
    Sub Main()
        Console.WriteLine(StringFromColor(Color.Red))
        Console.WriteLine(StringFromColor(Color.Green))
        Console.WriteLine(StringFromColor(Color.Blue))
    End Sub

    Function StringFromColor(c As Color) As String
        Select Case c
            Case Color.Red
                Return String.Format("Red = " & CInt(c))

            Case Color.Green
                Return String.Format("Green = " & CInt(c))

            Case Color.Blue
                Return String.Format("Blue = " & CInt(c))

            Case Else
                Return "Invalid color"
        End Select
    End Function
End Module
```

上記の例では、列挙値と関連付けられた値を出力します。 出力は次のようになります。

```
Red = 0
Green = 10
Blue = 11
```

値の理由は次のとおりです。

* 列挙値`Red`、値が自動的に割り当てられて`0`(初期化子を持たない列挙型値の最初のメンバーであるため)。

* 列挙値`Green`値が明示的に指定`10`します。

* 列挙値`Blue`直前列挙の値より 1 大きい値を自動的に割り当てられます。

定数式が直接または間接的に値を使用しません独自の関連する列挙値の (つまり、定数式での循環は許可されません)。 次の例が有効でないための宣言`A`と`B`が循環します。

```vb
Enum Circular
    A = B
    B
End Enum
```

`A` 依存`B`明示的と`B`異なります`A`暗黙的にします。

## <a name="classes"></a>クラス

A*クラス*はデータ メンバー (定数、変数、およびイベント)、関数メンバー (メソッド、プロパティ、インデクサー、演算子、およびコンストラクター) および入れ子にされた型を含む可能性のあるデータ構造です。 クラスは、参照型です。

```antlr
ClassDeclaration
    : Attributes? ClassModifier* 'Class' Identifier TypeParameterList? StatementTerminator
      ClassBase?
      TypeImplementsClause*
      ClassMemberDeclaration*
      'End' 'Class' StatementTerminator
    ;

ClassModifier
    : TypeModifier
    | 'MustInherit'
    | 'NotInheritable'
    | 'Partial'
    ;
```

次の例では、各種類のメンバーを含むクラスを示します。

```vb
Class AClass
    Public Sub New()
        Console.WriteLine("Constructor")
    End Sub

    Public Sub New(value As Integer)
        MyVariable = value
        Console.WriteLine("Constructor")
    End Sub

    Public Const MyConst As Integer = 12
    Public MyVariable As Integer = 34

    Public Sub MyMethod()
        Console.WriteLine("MyClass.MyMethod")
    End Sub

    Public Property MyProperty() As Integer
        Get
            Return MyVariable
        End Get

        Set (value As Integer)
            MyVariable = value
        End Set
    End Property

    Default Public Property Item(index As Integer) As Integer
        Get
            Return 0
        End Get

        Set (value As Integer)
            Console.WriteLine("Item(" & index & ") = " & value)
        End Set
    End Property

    Public Event MyEvent()

    Friend Class MyNestedClass
    End Class 
End Class
```

次の例では、これらのメンバーの使用を示します。

```vb
Module Test

    ' Event usage.
    Dim WithEvents aInstance As AClass

    Sub Main()
        ' Constructor usage.
        Dim a As AClass = New AClass()
        Dim b As AClass = New AClass(123)

        ' Constant usage.
        Console.WriteLine("MyConst = " & AClass.MyConst)

        ' Variable usage.
        a.MyVariable += 1
        Console.WriteLine("a.MyVariable = " & a.MyVariable)

        ' Method usage.
        a.MyMethod()

        ' Property usage.
        a.MyProperty += 1
        Console.WriteLine("a.MyProperty = " & a.MyProperty)
        a(1) = 1

        ' Event usage.
        aInstance = a
    End Sub 

    Sub MyHandler() Handles aInstance.MyEvent
        Console.WriteLine("Test.MyHandler")
    End Sub 
End Module
```

2 つのクラスに固有の修飾子がある`MustInherit`と`NotInheritable`します。 両方を指定することはできません。


### <a name="class-base-specification"></a>クラスの基本仕様

クラス宣言では、クラスの直接の基本型を定義する基本データ型仕様を含めることができます。

```antlr
ClassBase
    : 'Inherits' NonArrayTypeName StatementTerminator
    ;
```

明示的な基本型、クラス宣言がない場合は、直接の基本型は暗黙的に`Object`します。 例:

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class
```

スコープ内の型パラメーターがありますが、基本データ型は、独自の型パラメーターにすることはできません。

```vb
Class C1(Of V) 
End Class

Class C2(Of V)
    Inherits V    ' Error, type parameter used as base class 
End Class

Class C3(Of V)
    Inherits C1(Of V)    ' OK: not directly inheriting from V.
End Class
```

クラスからのみ派生`Object`とクラス。 クラスから派生することはできません`System.ValueType`、 `System.Enum`、 `System.Array`、`System.MulticastDelegate`または`System.Delegate`します。 ジェネリック クラスから派生できません`System.Attribute`またはそこから派生するクラス。

すべてのクラスが 1 つだけの直接基底クラスと派生での循環は禁止されています。 派生することはできません、`NotInheritable`クラス、および基底クラスのアクセシビリティ ドメインは、クラス自体のアクセシビリティ ドメインのスーパー セットであるかと同じにする必要があります。


### <a name="class-members"></a>クラス メンバー

クラスのメンバーは、クラス メンバー宣言で導入されたメンバーとその直接の基本クラスから継承されたメンバーで構成されます。

```antlr
ClassMemberDeclaration
    : NonModuleDeclaration
    | EventMemberDeclaration
    | VariableMemberDeclaration
    | ConstantMemberDeclaration
    | MethodMemberDeclaration
    | PropertyMemberDeclaration
    | ConstructorMemberDeclaration
    | OperatorDeclaration
    ;
```

クラス メンバーの宣言があります`Public`、 `Protected`、 `Friend`、 `Protected Friend`、または`Private`アクセスします。 クラス メンバーの宣言に、アクセス修飾子が含まれていない場合、宣言既定`Public`アクセス、変数の宣言がある場合を除き、その場合は既定値は`Private`アクセスします。

クラス メンバーのスコープは、(ジェネリックし、制約があります) 場合は、メンバーの宣言が行われているクラス本体とそのクラスの制約リストです。 メンバーがある場合`Friend`を同じプログラム内で任意の派生クラスまたは与えられているすべてのアセンブリのクラスの本文にアクセス、そのスコープの拡張`Friend`アクセス、メンバーがある場合と`Public`、 `Protected`、または`Protected Friend`そのスコープにアクセスします。すべてのプログラムで任意の派生クラスのクラス本体に拡張されます。


## <a name="structures"></a>構造体

*構造体*値の型から継承するは`System.ValueType`します。 データ メンバーおよび関数メンバーを格納できるデータ構造を表しているという点では、構造体をクラスに似ています。 クラスと異なり、ただし、構造体は不要ヒープ割り当てです。

```antlr
StructureDeclaration
    : Attributes? StructureModifier* 'Structure' Identifier
      TypeParameterList? StatementTerminator
      TypeImplementsClause*
      StructMemberDeclaration*
      'End' 'Structure' StatementTerminator
    ;

StructureModifier
    : TypeModifier
    | 'Partial'
    ;
```

クラスの場合は、同じオブジェクトを参照する 2 つの変数とできるため、他の変数によって参照されるオブジェクトに影響を与える 1 つの変数に対する操作です。 各変数以外の独自のコピーのある構造を持つ`Shared`データ、ため、次の例に示すように影響を与える、他のいずれかの操作にことはできません。

```vb
Structure Point
    Public x, y As Integer

    Public Sub New(x As Integer, y As Integer)
        Me.x = x
        Me.y = y
    End Sub
End Structure
```

次のコードは、値を出力宣言`10`:

```vb
Module Test
    Sub Main()
        Dim a As New Point(10, 10)
        Dim b As Point = a

        a.x = 100
        Console.WriteLine(b.x)
    End Sub
End Module
```

割り当て`a`に`b`、値のコピーを作成および`b`はそのためへの割り当てによって影響を受けません`a.x`。 `Point`されて代わりに出力になりますが、クラスとして宣言すると、`100`ため`a`と`b`同じオブジェクトを参照します。


### <a name="structure-members"></a>構造体のメンバー

構造体のメンバーは、その構造体のメンバー宣言で導入されたメンバーとメンバーが継承`System.ValueType`します。

```antlr
StructMemberDeclaration
    : NonModuleDeclaration
    | VariableMemberDeclaration
    | ConstantMemberDeclaration
    | EventMemberDeclaration
    | MethodMemberDeclaration
    | PropertyMemberDeclaration
    | ConstructorMemberDeclaration
    | OperatorDeclaration
    ;
```

すべての構造体は暗黙的に、`Public`構造体の既定値を生成するパラメーターなしコンストラクター。 その結果、構造型宣言のパラメーターなしインターフェイス コンストラクターを宣言することはできません。 構造体の型が宣言で、ただし、許可されている*パラメーター化された*コンストラクターの場合は、次の例のように、インスタンスします。

```vb
Structure Point
    Private x, y As Integer

    Public Sub New(x As Integer, y As Integer)
        Me.x = x
        Me.y = y
    End Sub
End Structure
```

上記の宣言を指定するには、次のステートメントは両方の作成、`Point`で`x`と`y`0 に初期化します。

```vb
Dim p1 As Point = New Point()
Dim p2 As Point = New Point(0, 0)
```

直接構造体には、そのフィールドの値 (なくそれらの値への参照) が含まれている、ために、構造体は、直接または間接的にそれ自体を参照するフィールドを含めることはできません。 たとえば、次のコードが無効です。

```vb
Structure S1
    Dim f1 As S2
End Structure

Structure S2
    ' This would require S1 to contain itself.
    Dim f1 As S1
End Structure
```

通常、構造体のメンバー宣言の必要がありますのみ`Public`、 `Friend`、または`Private`から継承されたメンバーをオーバーライドするときに、アクセス`Object`、`Protected`と`Protected Friend`へのアクセスにも使用できます。 構造体メンバーの宣言に、アクセス修飾子が含まれていない場合、宣言既定`Public`アクセスします。 構造体で宣言されたメンバーのスコープは、(ジェネリックあったし、制約がありました) 場合は、宣言が行われている構造体の本体とその構造の制約です。


## <a name="standard-modules"></a>標準的なモジュール

A*標準モジュール*は、暗黙的にメンバーを含む型は、`Shared`標準モジュール宣言自体にではなく、標準のモジュールを含む名前空間の宣言領域にスコープとします。 標準的なモジュールをインスタンス化されないことがあります。 標準的なモジュール型の変数を宣言するとエラーになります。

```antlr
ModuleDeclaration
    : Attributes? TypeModifier* 'Module' Identifier StatementTerminator
      ModuleMemberDeclaration*
      'End' 'Module' StatementTerminator
    ;
```

標準モジュールのメンバーでは、もう 1 つ、標準のモジュール名のない標準のモジュールの名前を持つ 2 つの完全修飾名を持ちます。 名前空間内の 1 つ以上の標準モジュールは、特定の名前を持つメンバーを定義できます。モジュールのいずれかの外部名への非修飾の参照があいまいです。 例:

```vb
Namespace N1
    Module M1
        Sub S1()
        End Sub

        Sub S2()
        End Sub
    End Module

    Module M2
        Sub S2()
        End Sub
    End Module

    Module M3
        Sub Main()
            S1()       ' Valid: Calls N1.M1.S1.
            N1.S1()    ' Valid: Calls N1.M1.S1.
            S2()       ' Not valid: ambiguous.
            N1.S2()    ' Not valid: ambiguous.
            N1.M2.S2() ' Valid: Calls N1.M2.S2.
        End Sub
    End Module
End Namespace
```

モジュールは、名前空間で宣言することは、別の型に入れ子にすることがありますできません。 標準的なモジュールはインターフェイスを実装していないから暗黙的に派生`Object`、およびのみが`Shared`コンストラクター。


### <a name="standard-module-members"></a>標準モジュールのメンバー

標準モジュールのメンバーは、そのメンバーの宣言で導入されたメンバーとメンバーが継承`Object`します。 標準的なモジュールには、インスタンス コンストラクター以外のメンバーの任意の型があります。 すべての標準的なモジュール型のメンバーは、暗黙的に`Shared`します。

```antlr
ModuleMemberDeclaration
    : NonModuleDeclaration
    | VariableMemberDeclaration
    | ConstantMemberDeclaration
    | EventMemberDeclaration
    | MethodMemberDeclaration
    | PropertyMemberDeclaration
    | ConstructorMemberDeclaration
    ;
```

通常、標準モジュールのメンバー宣言の必要がありますのみ`Public`、 `Friend`、または`Private`から継承されたメンバーをオーバーライドするときに、アクセス`Object`、`Protected`と`Protected Friend`アクセス修飾子を指定することがあります。 標準モジュール メンバーの宣言に、アクセス修飾子が含まれていない場合、宣言既定`Public`アクセス、既定では、変数である場合を除き`Private`アクセスします。

前述のようは、標準モジュール メンバーのスコープは、標準のモジュール宣言を含む宣言です。 継承したメンバー`Object`この特別なスコープ規則に含まれないメンバー スコープがない場合や、モジュールの名前で常に修飾する必要があります。 メンバーがある場合`Friend`アクセス、そのスコープが同じプログラムまたは指定されているアセンブリで宣言された名前空間のメンバーにのみ拡張`Friend`アクセスします。


## <a name="interfaces"></a>インターフェイス

*インターフェイス*は参照型の特定のメソッドをサポートしていることを保証するための他の型に実装します。 インターフェイスは直接作成され、実際の表現がありません - その他の型はインターフェイス型に変換する必要があります。 インターフェイスは、コントラクトを定義します。 クラスまたはインターフェイスを実装する構造体は、そのコントラクトに従う必要があります。

```antlr
InterfaceDeclaration
    : Attributes? TypeModifier* 'Interface' Identifier
      TypeParameterList? StatementTerminator
      InterfaceBase*
      InterfaceMemberDeclaration*
      'End' 'Interface' StatementTerminator
    ;
```


次の例では、既定のプロパティを格納しているインターフェイス`Item`、イベント`E`、メソッド`F`、プロパティと、 `P`:

```vb
Interface IExample
    Default Property Item(index As Integer) As String

    Event E()

    Sub F(value As Integer)

    Property P() As String
End Interface
```

インターフェイスは、多重継承を採用できます。 次の例では、インターフェイスで`IComboBox`両方から継承`ITextBox`と`IListBox`:

```vb
Interface IControl
    Sub Paint()
End Interface 

Interface ITextBox
    Inherits IControl

    Sub SetText(text As String)
End Interface 

Interface IListBox
    Inherits IControl

    Sub SetItems(items() As String)
End Interface 

Interface IComboBox
    Inherits ITextBox, IListBox 
End Interface
```

クラスと構造体には、複数のインターフェイスを実装できます。 次の例では、クラスで`EditBox`クラスから派生`Control`両方を実装および`IControl`と`IDataBound`:

```vb
Interface IDataBound
    Sub Bind(b As Binder)
End Interface 

Public Class EditBox
    Inherits Control
    Implements IControl, IDataBound

    Public Sub Paint() Implements IControl.Paint
        ...
    End Sub

    Public Sub Bind(b As Binder) Implements IDataBound.Bind
        ...
    End Sub
End Class
```


### <a name="interface-inheritance"></a>インターフェイスの継承

インターフェイスの基本インターフェイスは、明示的な基本インターフェイスの基本インターフェイスです。 つまり、基本インターフェイスのセットは、明示的な基本インターフェイス、明示的な基本インターフェイス、およびなどの完全な推移閉包です。 インターフェイスの宣言を持たない明示的なインターフェイス ベース、型の基本インターフェイスはありません--インターフェイスから継承しない場合`Object`(があるへの拡大変換が`Object`)。

```antlr
InterfaceBase
    : 'Inherits' InterfaceBases StatementTerminator
    ;

InterfaceBases
    : NonArrayTypeName ( Comma NonArrayTypeName )*
    ;
```

次の例では、インターフェイスの基本`IComboBox`は`IControl`、 `ITextBox`、および`IListBox`します。

```vb
Interface IControl
    Sub Paint()
End Interface 

Interface ITextBox
    Inherits IControl

    Sub SetText(text As String)
End Interface 

Interface IListBox
    Inherits IControl

    Sub SetItems(items() As String)
End Interface 

Interface IComboBox
    Inherits ITextBox, IListBox 
End Interface
```

インターフェイスは、その基本インターフェイスのすべてのメンバーを継承します。 つまり、`IComboBox`上記のインターフェイス メンバーを継承する`SetText`と`SetItems`だけでなく`Paint`します。

クラスまたは構造体も暗黙的にインターフェイスを実装するには、すべてのインターフェイスの基本インターフェイスを実装します。

基本インターフェイスの推移的なクロージャでインターフェイスが複数回表示された場合のみ機能がわかりやすい派生インターフェイスをそのメンバーを 1 回です。 基底インターフェイスを 1 回定義のみ、派生インターフェイスは、乗算のメソッドを実装するには実装する型。 次の例では、`Paint`クラスを実装する場合でも、1 回実装するだけ`IComboBox`と`IControl`します。

```vb
Class ComboBox
    Implements IControl, IComboBox

    Sub SetText(text As String) Implements IComboBox.SetText
    End Sub

    Sub SetItems(items() As String) Implements IComboBox.SetItems
    End Sub

    Sub Print() Implements IComboBox.Paint
    End Sub
End Class
```

`Inherits`句が他の影響を与えません`Inherits`句。 次の例では、`IDerived`の名前を修飾する必要があります`INested`で`IBase`します。

```vb
Interface IBase
    Interface INested
        Sub Nested()
    End Interface

    Sub Base()
End Interface

Interface IDerived
    Inherits IBase, INested   ' Error: Must specify IBase.INested.
End Interface
```

基底インターフェイスのアクセシビリティ ドメインは、インターフェイス自体のアクセシビリティ ドメインのスーパー セットであるかと同じである必要があります。


### <a name="interface-members"></a>インターフェイスのメンバー

インターフェイスのメンバーは、そのメンバーの宣言で導入されたメンバーとその基底インターフェイスから継承されたメンバーで構成されます。

```antlr
InterfaceMemberDeclaration
    : NonModuleDeclaration
    | InterfaceEventMemberDeclaration
    | InterfaceMethodMemberDeclaration
    | InterfacePropertyMemberDeclaration
    ;
```

インターフェイス メンバーを継承しない`Object`からすべてのクラスまたはインターフェイスを実装する構造体を継承しているため、`Object`のメンバー `Object`、拡張メソッドなど、インターフェイスのメンバーと見なされますキャストを必要とせずに直接インターフェイスで呼び出すことができますと`Object`します。 例:

```vb
Interface I1
End Interface

Class C1
    Implements I1
End Class

Module Test
    Sub Main()
        Dim i As I1 = New C1()
        Dim h As Integer = i.GetHashCode()
    End Sub
End Module
```

メンバーとして同じ名前のインターフェイスのメンバー`Object`暗黙的にシャドウ`Object`メンバー。 インターフェイスのメンバーには、入れ子にされた型、メソッド、プロパティ、およびイベントのみを含めることができます。 メソッドとプロパティには、本文がありません。 インターフェイスのメンバーは暗黙的に`Public`アクセス修飾子が指定されていない可能性があります。 インターフェイスで宣言されたメンバーのスコープは、(ジェネリックし、制約があります) 場合は、宣言が行われているインターフェイスの本体とそのインターフェイスの制約リストです。


## <a name="arrays"></a>配列

*配列*変数を使用してアクセスを含む参照型である*インデックス*配列内の変数の順序で一対一で対応します。 を配列に含まれる変数とも呼ばれます、*要素*、配列のすべて、同じ種類は、この型が呼び出される、*要素型*の配列。

```antlr
ArrayTypeName
    : NonArrayTypeName ArrayTypeModifiers
    ;

ArrayTypeModifiers
    : ArrayTypeModifier+
    ;

ArrayTypeModifier
    : OpenParenthesis RankList? CloseParenthesis
    ;

RankList
    : Comma*
    ;

ArrayNameModifier
    : ArrayTypeModifiers
    | ArraySizeInitializationModifier
    ;
```

配列の要素は、配列インスタンスの作成時に存在することになるし、配列のインスタンスが破棄されるときに存在しなくなります。 配列の各要素は、その型の既定値に初期化されます。 型`System.Array`のすべての配列型の基本型は、インスタンス化されないことがあります。 すべての配列型で宣言されたメンバーの継承、`System.Array`を入力し、それに変換可能です (と`Object`)。 要素を持つ 1 次元配列型`T`もインターフェイスを実装`System.Collections.Generic.IList(Of T)`と`IReadOnlyList(Of T)`場合`T`が参照型では、配列の型も実装`IList(Of U)`と`IReadOnlyList(Of U)`任意の`U`拡大変換からの変換の参照を持つ`T`します。

配列が、*ランク*配列の各要素に関連付けられているインデックスの数を決定します。 配列のランクの数を決定する*ディメンション*の配列。 たとえばのいずれかのランクを持つ配列は、1 次元の配列と呼ばれ、1 より大きいランクを持つ配列は多次元配列と呼ばれます。

次の例では、整数値の 1 次元配列を作成するには、配列の要素を初期化しますおよびからそれらの出力します。

```vb
Module Test
    Sub Main()
        Dim arr(5) As Integer
        Dim i As Integer

        For i = 0 To arr.Length - 1
            arr(i) = i * i
        Next i

        For i = 0 To arr.Length - 1
            Console.WriteLine("arr(" & i & ") = " & arr(i))
        Next i
    End Sub
End Module
```

プログラムは、次を出力します。

```
arr(0) = 0
arr(1) = 1
arr(2) = 4
arr(3) = 9
arr(4) = 16
arr(5) = 25
```

配列の各次元が、関連付けられた長さ。 次元の長さは、配列の型の一部ではありませんが、配列型のインスタンスが実行時に作成されたときに確立されます。 次元の長さは、その次元のインデックスの有効な範囲を決定します。 長さのディメンションの`N`、インデックスの範囲は 0 ~ `N-1`。 ディメンションが長さ 0 の場合は、その次元のインデックスが有効なことはありません。 配列内の要素の合計数は、配列の各次元の長さの製品です。 長さ 0 の配列の次元のいずれかの場合は、配列は空にすることはできます。 配列の要素型には、任意の型を指定できます。

配列型は、既存の型名を修飾子を追加することによって指定されます。 修飾子は、一連のコンマの 0 個以上で、左かっこと右かっこで構成されます。 変更の種類は、配列の要素の型と次元数は 1 を足したコンマの数。 1 つ以上の修飾子が指定されている場合、配列の要素型は配列です。 修飾子は最も外側の配列をされている一番左の修飾子で、左から右に読み取らします。 例

```vb
Module Test
    Dim arr As Integer(,)(,,)()
End Module
```

要素型`arr`の 1 次元配列の 3 次元の配列の 2 次元の配列は、`Integer`します。

変数名の配列型修飾子または、配列のサイズの初期化の修飾子を配置することで、配列型である変数を宣言することも。 その場合は、配列要素の型は、宣言で指定された型と配列の次元は、変数名修飾子によって決まります。 わかりやすくするため、同じ宣言内で変数名と型名の両方の配列型修飾子があることはできません。

次の例では、ローカル変数の宣言で配列の型を使用するさまざまな`Integer`要素の型として。

```vb
Module Test
    Sub Main()
        Dim a1() As Integer    ' Declares 1-dimensional array of integers.
        Dim a2(,) As Integer   ' Declares 2-dimensional array of integers.
        Dim a3(,,) As Integer  ' Declares 3-dimensional array of integers.

        Dim a4 As Integer()    ' Declares 1-dimensional array of integers.
        Dim a5 As Integer(,)   ' Declares 2-dimensional array of integers.
        Dim a6 As Integer(,,)  ' Declares 3-dimensional array of integers.

        ' Declare 1-dimensional array of 2-dimensional arrays of integers 
        Dim a7()(,) As Integer
        ' Declare 2-dimensional array of 1-dimensional arrays of integers.
        Dim a8(,)() As Integer

        Dim a9() As Integer() ' Not allowed.
    End Sub
End Module
```

配列の型名修飾子は、それに続くかっこのすべてのセットを拡張します。 これはこと型名の後にかっこで囲まれた引数のセットを許可する場所の状況でない配列の型名の引数を指定することを意味します。 例:

```vb
Module Test
    Sub Main()
        ' This calls the Integer constructor.
        Dim x As New Integer(3)

        ' This declares a variable of Integer().
        Dim y As Integer()

        ' This gives an error.
        ' Array sizes can not be specified in a type name.
        Dim z As Integer()(3)
    End Sub
End Module
```

最後の場合、`(3)`コンストラクターの引数のセットとしてではなく、型名の一部として解釈されます。


## <a name="delegates"></a>デリゲート

A*委任*参照型を参照するには、`Shared`メソッド、型またはオブジェクトのインスタンス メソッドにします。

```antlr
DelegateDeclaration
    : Attributes? TypeModifier* 'Delegate' MethodSignature StatementTerminator
    ;

MethodSignature
    : SubSignature
    | FunctionSignature
    ;
```

 その他の言語では、デリゲートの最も近いは、関数ポインターに対してのみ、関数ポインターを参照できますが、`Shared`関数、デリゲートは、両方を参照できます`Shared`メソッドとインスタンスします。 後者の場合、デリゲートは、メソッドのエントリ ポイントへの参照だけでなく、メソッドの呼び出しに使用するオブジェクトのインスタンスへの参照を格納します。

デリゲートの宣言がない可能性があります、`Handles`句では、`Implements`句では、メソッドの本体で、または`End`を構築します。 デリゲート宣言のパラメーター リストがない場合があります`Optional`または`ParamArray`パラメーター。 戻り値の型とパラメーターの型のアクセシビリティ ドメインは、デリゲート自体のアクセシビリティ ドメインのスーパー セットであるかと同じである必要があります。

デリゲートのメンバーは、クラスから継承されたメンバー`System.Delegate`します。 デリゲートは、次のメソッドも定義します。

* 型の 1 つ、2 つのパラメーターを受け取るコンストラクター`Object`型の 1 つ`System.IntPtr`します。

* `Invoke`デリゲートと同じシグネチャを持つメソッド。

* A`BeginInvoke`メソッド シグネチャがデリゲートのシグネチャを次の 3 つの点が異なります。 戻り値の型を変更する最初に、`System.IAsyncResult`します。 次に、2 つのパラメーターがパラメーター リストの末尾に追加されます: 型の最初の`System.AsyncCallback`と型の 2 番目の`Object`します。 最後に、すべて`ByRef`パラメーターがある変更`ByVal`します。

* `EndInvoke`メソッドの戻り値の型は、デリゲートと同じです。 メソッドのパラメーターはデリゲート パラメーターを`ByRef`デリゲート シグネチャに出現する順序と同じ順序でのパラメーター。  型の追加のパラメーターがあるこれらのパラメーターだけでなく`System.IAsyncResult`パラメーター リストの末尾。

3 つのステップを定義すると、デリゲートの使用: 宣言、インスタンス化、および呼び出し。

デリゲートは、デリゲート宣言の構文を使用して宣言されます。 次の例は、という名前のデリゲートを宣言`SimpleDelegate`する引数を受け取りません。

```vb
Delegate Sub SimpleDelegate()
```

次の例では、作成、`SimpleDelegate`インスタンスし、すぐに呼び出します。

```vb
Module Test
    Sub F()
        System.Console.WriteLine("Test.F")
    End Sub 

    Sub Main()
        Dim d As SimpleDelegate = AddressOf F
        d()
    End Sub 
End Module
```

メソッドのデリゲートをインスタンス化すると、すぐに呼び出して、デリゲート経由で多くのポイントにメソッドを直接呼び出すやすくなること。 デリゲートは、その匿名性を使用すると、その有用性を表示します。 例を次に示します、`MultiCall`を繰り返し呼び出すメソッドを`SimpleDelegate`インスタンス。

```vb
Sub MultiCall(d As SimpleDelegate, count As Integer)
    Dim i As Integer

    For i = 0 To count - 1
        d()
    Next i
End Sub
```

重要ではない、`MultiCall`メソッド ターゲット メソッドの`SimpleDelegate`、どのようなユーザー補助機能は、このメソッドは、またはメソッドは、かどうか`Shared`かどうか。 ターゲット メソッドのシグネチャと互換性がある、重要な`SimpleDelegate`します。


## <a name="partial-types"></a>部分型

クラスと構造体の宣言ができる*部分*宣言します。 部分宣言は、宣言内で宣言された型が完全に記述しない場合があります。 代わりに、型の宣言は、プログラム内で複数の部分宣言間で分散可能性があります。プログラムの境界を越えて、部分型を宣言することはできません。 部分型の宣言を指定します、`Partial`修飾子を宣言します。 次に、同じ完全修飾名を持つ型のプログラムでその他の宣言は、1 つの型宣言を形成するコンパイル時に、部分的な宣言と共に結合されます。 たとえば、次のコードは 1 つのクラスを宣言します。`Test`メンバーと`Test.C1`と`Test.C2`します。

a.vb:

```vb
Public Partial Class Test
    Public Sub S1()
    End Sub
End Class
```

b.vb:

```vb
Public Class Test
    Public Sub S2()
    End Sub
End Class
```

部分型の宣言を結合するときに、少なくとも 1 つの宣言では、必要があります、`Partial`修飾子は、それ以外の場合、コンパイル時エラーが発生します。

__注意してください。__ 指定することはできますが`Partial`、多くの部分宣言間で 1 つだけ宣言はすべての部分宣言で指定するフォームが向上します。 1 つの部分宣言が表示されるが 1 つまたは複数の部分宣言は非表示 (ツールによって生成されたコードを拡張する場合) などの状況ではのままに許容される、`Partial`可視の宣言から修飾子がで指定します非表示の宣言。

部分的な宣言を使用して、クラスと構造体のみを宣言できます。 型のアリティは部分的な宣言を一緒に一致すると見なされます。 名前が同じで型パラメーターの数が異なる 2 つのクラスは、同時の partial 宣言には考慮されません。 部分的な宣言が属性を指定する、クラスの修飾子、`Inherits`ステートメントまたは`Implements`ステートメント。 コンパイル時にのすべての部分宣言の結合させるとは、型宣言の一部として使用あり。 修飾子を属性間の競合がある場合ベース、インターフェイス、または型のメンバー、コンパイル時エラーの結果。 例:

```vb
Public Partial Class Test1
    Implements IDisposable
End Class

Class Test1
    Inherits Object
    Implements IComparable
End Class

Public Partial Class Test2
End Class

Private Partial Class Test2
End Class
```

前の例では、型を宣言する`Test1`つまり`Public`から継承`Object`実装と`System.IDisposable`と`System.IComparable`します。 部分宣言`Test2`宣言では、いずれかのことを示すために、コンパイル時エラーが発生`Test2`は`Public`ことを示す別および`Test2`は`Private`。

制約と分散型のパラメーターの型パラメーターを持つ部分の型を宣言できますが、制約と各部分宣言からの差異が一致する必要があります。 そのため、制約との差異でその他の修飾子のように自動的に結合されません。

```vb
Partial Public Class List(Of T As IEnumerable)
End Class

' Error: Constraints on T don't match
Class List(Of T As IComparable)
End Class
```

複数の部分宣言を使用して、型が宣言されているという事実は、型の中で名前のルックアップ規則には影響しません。 その結果、部分型の宣言は、他の部分型の宣言で宣言されたメンバーを使用できます。 またはその他の部分型の宣言で宣言されたインターフェイスにメソッドを実装することがあります。 例:

```vb
Public Partial Class Test1
    Implements IDisposable

    Private IsDisposed As Boolean = False
End Class

Class Test1
    Private Sub Dispose() Implements IDisposable.Dispose
        If Not IsDisposed Then
            ...
        End If
    End Sub
End Class
```

入れ子にされた型には、同様の部分宣言を持つことができます。 例:

```vb
Public Partial Class Test
    Public Partial Class NestedTest
        Public Sub S1()
        End Sub
    End Class
End Class

Public Partial Class Test
    Public Partial Class NestedTest
        Public Sub S2()
        End Sub
    End Class
End Class
```

部分的な宣言内で初期化子が宣言の順序で実行されます。ただし、別の部分宣言で発生する初期化子の実行の順序は保証はありません。

## <a name="constructed-types"></a>構築された型

ジェネリック型の宣言では、単独では型を表していません。 代わりに、型引数を適用することでさまざまな種類のフォームにするには、「ブルー プリント」としてジェネリック型の宣言を使用します。 適用された型引数を持つジェネリック型が呼び出される、*構築型*します。 構築された型の型引数に一致する型パラメーターに対する制約を常に満たす必要があります。

型名によって型パラメーターを直接指定しない場合でも、構築された型がわかります。 これは、型がジェネリック クラス宣言の中で入れ子になった、外側の宣言のインスタンスの型が暗黙的に名前参照の使用に発生することができます。

```vb
Class Outer(Of T) 
    Public Class Inner 
    End Class

    ' Type of i is the constructed type Outer(Of T).Inner
    Public i As Inner 
End Class
```

構築された型`C(Of T1,...,Tn)`はジェネリック型とすべての型引数にアクセスするときにアクセスします。 たとえば場合は、ジェネリック型`C`は`Public`とすべての型引数`T1,...,Tn`は`Public`、構築された型は`Public`。 型名または型引数のいずれかのいずれかが場合`Private`、ただし、構築された型のアクセシビリティは`Private`します。 構築された型の 1 つの型引数がある場合`Protected`別の型引数は`Friend`、構築された型は、クラスとで与えられている任意のアセンブリのアセンブリ、またはそのサブクラス内だけでアクセスし、`Friend`アクセスします。 つまり、構築された型のアクセシビリティ ドメインは、その構成要素のアクセシビリティ ドメインの交差部分。

__注意してください。__ 構築された型のアクセシビリティ ドメインが構成された部分の積集合であるという事実では、新しいアクセシビリティ レベルを定義する興味深い副作用があります。 構築された型にある要素を含む`Protected`と要素が`Friend`にアクセスできるコンテキストでのみアクセスできる*両方* `Friend` *と* `Protected`メンバー。 ただし、ユーザー補助機能と、言語では、このアクセシビリティ レベルを表現する方法はありません`Protected Friend`エンティティにアクセスできるコンテキストでアクセスできることを意味*か* `Friend` *または*`Protected`メンバー。

基本して実装されるインターフェイスと構築された型のメンバーは、ジェネリック型の型パラメーターのうち、指定された型引数を代入することによって決定されます。

### <a name="open-types-and-closed-types"></a>オープン型とクローズ型

構築された型を含んでいる型またはメソッドの型パラメーターが 1 つまたは複数の型引数にはユーザーと呼ばれる、*オープン型*します。 これは、します型の実際の図形が完全に認識していませんので、不明なので、型の型パラメーターの一部は引き続き。 これに対し、型引数はすべての非型パラメーター、ジェネリック型と呼ばれる、*クローズ型*します。 クローズ型の図形は常に完全に呼ばれます。 例:

```vb
Class Base(Of T, V)
End Class

Class Derived(Of V)
    Inherits Base(Of Integer, V)
End Class

Class MoreDerived
    Inherits Derived(Of Double)
End Class
```

構築された型`Base(Of Integer, V)`オープン型は、ためが型パラメーター`T`指定されています型パラメーター`U`が別の型パラメーターを指定します。 そのため、型の完全な図形がまだ不明です。 構築された型`Derived(Of Double)`がクローズ型を継承階層内のすべての型パラメーターが指定されているためです。

オープン型の定義は次のとおりです。

* 型パラメーターは、オープン型です。

* 要素の型がオープン型の場合、配列型はオープン型が。

* 構築された型がオープン型場合、1 つまたは複数の型引数、オープン型。

* 閉じている型は、オープン型ではない型です。

プログラムのエントリ ポイントは、ジェネリック型にすることはできません、ため、実行時に使用されるすべての型はクローズ型になります。

## <a name="special-types"></a>特殊な種類

.NET Framework には、多数 Visual Basic 言語と .NET Framework によって特別に扱われるクラスにはが含まれています。

型`System.Void`、.NET framework では、void 型を表す直接参照できるだけで`GetType`式。

種類`System.RuntimeArgumentHandle`、`System.ArgIterator`と`System.TypedReference`すべてスタックへのポインターを含めることができ、.NET Framework ヒープ上に表示できないようにします。 そのため、配列の要素型、戻り値の型、フィールドの型、ジェネリック型引数、null 許容型として使用することはできません`ByRef`パラメーターの型に変換される値の型`Object`または`System.ValueType`インスタンスへの呼び出しのターゲットメンバーの`Object`または`System.ValueType`クロージャにリフトされたか。
