---
ms.openlocfilehash: 4560eac9f4ab52d07c77724aeca696d0195da91a
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306021"
---
# <a name="types"></a>種類

Visual Basic の型の2つの基本カテゴリは、*値型*と*参照型*です。 プリミティブ型 (文字列を除く)、列挙型、および構造体は、値型です。 クラス、文字列、標準モジュール、インターフェイス、配列、およびデリゲートは参照型です。

すべての型には*既定値*があり、これは初期化時にその型の変数に割り当てられる値です。

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

値型と参照型は、宣言の構文と使用方法とでは似ていますが、セマンティクスは異なります。

参照型はランタイムヒープに格納されます。そのストレージへの参照によってのみアクセスできます。 参照型は常に参照によってアクセスされるため、その有効期間は .NET Framework によって管理されます。 特定のインスタンスへの未処理の参照は追跡され、その他の参照が残っていない場合にのみインスタンスが破棄されます。 参照型の変数には、その型の値、より多くの派生型の値、または null 値への参照が含まれています。 *Null 値*は nothing を参照します。割り当てを除き、null 値を使用することはできません。 参照型の変数への代入では、参照される値のコピーではなく、参照のコピーが作成されます。 参照型の変数の場合、既定値は null 値になります。

値型は、配列内または別の型のいずれかのスタックに直接格納されます。ストレージには直接アクセスできます。 値型は変数に直接格納されるので、有効期間はそれらを含む変数の有効期間によって決まります。 値型のインスタンスを含む場所が破棄されると、値型のインスタンスも破棄されます。 値型には常に直接アクセスします。値型への参照を作成することはできません。 このような参照を禁止すると、破棄された値クラスのインスタンスを参照できなくなります。 値型は常に `NotInheritable`ので、値型の変数には常にその型の値が含まれます。 このため、値型の値を null 値にすることはできません。また、より多くの派生型のオブジェクトを参照することもできません。 値型の変数への代入では、割り当てられている値のコピーが作成されます。 値型の変数の既定値は、その型の各変数メンバーを既定値に初期化した結果です。

次の例は、参照型と値型の違いを示しています。

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

プログラムの出力は次のようになります。

```console
Values: 0, 123
Refs: 123, 123
```

ローカル変数 `val2` への代入はローカル変数 `val1` に影響しません。ローカル変数は両方とも値型 (型 `Integer`) であり、値型の各ローカル変数には独自のストレージがあるためです。 これに対して、割り当て `ref2.Value = 123;` は `ref1` と `ref2` の両方が参照するオブジェクトに影響します。

.NET Framework 型システムに関する注意点の1つは、構造体、列挙型、およびプリミティブ型 (`String`を除く) が値型であっても、すべてが参照型から継承することです。 構造体とプリミティブ型は、`Object`から継承する参照型 `System.ValueType`から継承されます。 列挙型は、`System.ValueType`から継承する参照型 `System.Enum`から継承されます。

### <a name="nullable-value-types"></a>null 許容値型

値型の場合は、型名に `?` 修飾子を追加して、その型の*null 許容*バージョンを表すことができます。

```antlr
NullableTypeName
    : NonArrayTypeName '?'
    ;

NullableNameModifier
    : '?'
    ;
```

Null 許容値型には、null 非許容の型と null 値の両方の値を含めることができます。 したがって、null 許容値型の場合、型の変数に `Nothing` を割り当てると、値型のゼロ値ではなく、変数の値が null 値に設定されます。 例 :

```vb
Dim x As Integer = Nothing
Dim y As Integer? = Nothing

' Prints zero
Console.WriteLine(x)
' Prints nothing (because the value of y is the null value)
Console.WriteLine(y)
```

変数名に null 許容型修飾子を指定することで、変数を null 許容値型として宣言することもできます。 わかりやすくするために、同じ宣言内の変数名と型名の両方に null 許容型修飾子を指定することは無効です。 Null 許容型は `System.Nullable(Of T)`型を使用して実装されるため、型 `T?` は `System.Nullable(Of T)`型と同義であり、2つの名前を同義に使用できます。 `?` 修飾子は、既に null 値が許容されている型には配置できません。したがって、型 `Integer??` または `System.Nullable(Of Integer)?`を宣言することはできません。

Null 許容型 `T?` には、`System.Nullable(Of T)` のメンバーだけでなく、基になる型 `T` から型 `T?`に*リフト*された演算子や変換が含まれています。 リフトは、基になる型から演算子と変換をコピーします。ほとんどの場合、null 非許容の値型に対して null 許容値型を代入します。 これにより、`T` に適用される同じ変換と操作の多くが `T?` にも適用されます。


## <a name="interface-implementation"></a>インターフェイスの実装

構造体とクラス宣言は、1つ以上の `Implements` 句を使用してインターフェイス型のセットを実装することを宣言できます。

```antlr
TypeImplementsClause
    : 'Implements' TypeImplements StatementTerminator
    ;

TypeImplements
    : NonArrayTypeName ( Comma NonArrayTypeName )*
    ;
```

`Implements` 句で指定するすべての型はインターフェイスである必要があり、型はインターフェイスのすべてのメンバーを実装する必要があります。 例 :

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

インターフェイスを実装する型も、すべてのインターフェイスの基本インターフェイスを暗黙的に実装します。 これは、型が `Implements` 句のすべての基本インターフェイスを明示的に一覧表示しない場合でも同様です。 この例では、`TextBox` 構造体が `IControl` と `ITextBox`の両方を実装しています。

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

型がのインターフェイスを実装する宣言では、型の宣言空間では何も宣言されません。 このため、同じ名前のメソッドを使用して2つのインターフェイスを実装することは有効です。

型は、型パラメーターを独自に実装することはできませんが、スコープ内の型パラメーターが含まれる場合があります。

```vb
Class C1(Of V)
    Implements V  ' Error, can't implement type parameter directly
    Implements IEnumerable(Of V)  ' OK, not directly implementing

    ...
End Class
```

ジェネリックインターフェイスは、異なる型引数を使用して複数回実装できます。 ただし、ジェネリック型は、指定された型パラメーター (型制約に関係なく) がそのインターフェイスの別の実装と重複する可能性がある場合に、型パラメーターを使用してジェネリックインターフェイスを実装することはできません。 例 :

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

*プリミティブ型*は、`System` 名前空間の定義済みの型のエイリアスであるキーワードを使用して識別されます。 プリミティブ型は、エイリアスの型と完全に区別できません。予約語 `Byte` は、`System.Byte`の書き込みとまったく同じです。 プリミティブ型は、*組み込み型*とも呼ばれます。

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


プリミティブ型は標準型のエイリアスであるため、すべてのプリミティブ型にはメンバーがあります。 たとえば、`Integer` には `System.Int32`で宣言されたメンバーがあります。 リテラルは、対応する型のインスタンスとして扱うことができます。

* プリミティブ型は、特定の追加操作を許可するという点で、他の構造体型とは異なります。

* プリミティブ型では、リテラルを記述することによって値を作成できます。 たとえば、`123I` は `Integer`型のリテラルです。

* プリミティブ型の定数を宣言することができます。

* 式のオペランドがすべてのプリミティブ型定数である場合、コンパイラはコンパイル時に式を評価することができます。 このような式は、定数式と呼ばれます。

Visual Basic は、次のプリミティブ型を定義します。

* 整数値型 `Byte` (1 バイト符号なし整数)、`SByte` (1 バイト符号付き整数)、`UShort` (2 バイト符号なし整数)、`Short` (2 バイト符号付き整数)、`UInteger` (4 バイト符号なし整数)、`Integer` (8 バイト符号付き整数)、`ULong` (8 バイト符号付き整数)、および `Long` (8 バイト符号付き整数) です。 これらの型は、それぞれ `System.Byte`、`System.SByte`、`System.UInt16`、`System.Int16`、`System.UInt32`、`System.Int32`、`System.UInt64`、および `System.Int64`にマップされます。 整数型の既定値は、リテラル `0`に相当します。

* 浮動小数点値型 `Single` (4 バイト浮動小数点) と `Double` (8 バイト浮動小数点) です。 これらの型は、それぞれ `System.Single` と `System.Double`にマップされます。 浮動小数点型の既定値は、リテラル `0`に相当します。

* `System.Decimal`にマップされる `Decimal` の種類 (16 バイトの10進値)。 Decimal の既定値は、リテラル `0D`に相当します。

* 実際の値を表す `Boolean` 値の型。通常は、リレーショナル操作または論理演算の結果です。 リテラルの型は `System.Boolean`です。 `Boolean` 型の既定値は、リテラル `False`に相当します。

* 日付や時刻を表す `Date` 値の型を `System.DateTime`に割り当てます。 `Date` 型の既定値は、リテラル `# 01/01/0001 12:00:00AM #`に相当します。

* `Char` 値の型。1つの Unicode 文字を表し、`System.Char`にマップされます。 `Char` 型の既定値は `ChrW(0)`定数式に相当します。

* `String` 参照型。 Unicode 文字のシーケンスを表し、`System.String`にマップされます。 `String` 型の既定値は null 値です。



## <a name="enumerations"></a>列挙体

*列挙*型は、`System.Enum` から継承し、1つのプリミティブ整数型の値のセットを表す値型です。

```antlr
EnumDeclaration
    : Attributes? TypeModifier* 'Enum' Identifier
      ( 'As' NonArrayTypeName )? StatementTerminator
      EnumMemberDeclaration+
      'End' 'Enum' StatementTerminator
    ;
```

`E`列挙型の場合、既定値は `CType(0, E)`式によって生成される値です。

列挙型の基になる型は、列挙体で定義されているすべての列挙子の値を表すことができる整数型である必要があります。 基になる型が指定されている場合は、`Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`、または `System` 名前空間内の対応する型のいずれかである必要があります。 基になる型が明示的に指定されていない場合、既定値は `Integer`です。

次の例では、基になる型 `Long`の列挙型を宣言しています。

```vb
Enum Color As Long
    Red
    Green
    Blue
End Enum
```

開発者は、例に示すように、基になる型の `Long`を使用して、`Long`の範囲内の値を使用できるようにすることも、`Integer`の範囲内ではなく、将来のオプションを保持することもできます。


### <a name="enumeration-members"></a>列挙型のメンバー

列挙体のメンバーは、列挙体で宣言された列挙値と、クラス `System.Enum`から継承されたメンバーです。

列挙体メンバーのスコープは、列挙型宣言の本体です。 つまり、列挙体の宣言の外部では、列挙体のメンバーを常に修飾する必要があります (型が名前空間のインポートによって明示的に名前空間にインポートされる場合を除きます)。

列挙メンバー宣言の宣言順序は、定数式の値が省略されている場合に重要です。 列挙型のメンバーには、暗黙的に `Public` アクセスのみが与えられます。列挙メンバー宣言でアクセス修飾子を使用することはできません。

```antlr
EnumMemberDeclaration
    : Attributes? Identifier ( Equals ConstantExpression )? StatementTerminator
    ;
```

### <a name="enumeration-values"></a>列挙値

列挙メンバーリスト内の列挙値は、基になる列挙型として型指定された定数として宣言され、定数が必要な場所に表示できます。 `=` を持つ列挙メンバー定義には、定数式によって示される値が関連付けられたメンバーに与えられます。 定数式は、基になる型に暗黙的に変換可能な整数型に評価される必要があり、基になる型で表すことができる値の範囲内である必要があります。 次の例では、定数値 `1.5`、`2.3`、および `3.3` は、厳密なセマンティクスで `Long` 基になる整数型に暗黙的に変換できないため、エラーになります。

```vb
Option Strict On

Enum Color As Long
    Red = 1.5
    Green = 2.3
    Blue = 3.3
End Enum
```

次に示すように、複数の列挙メンバーが同じ関連する値を共有する場合があります。

```vb
Enum Color
    Red
    Green
    Blue
    Max = Blue
End Enum
```

この例は、2つの列挙型メンバーを持つ列挙体を示しています。これには、`Blue` と `Max`、同じ値が関連付けられています。

列挙体の最初の列挙値の定義に初期化子がない場合、対応する定数の値は `0`です。 初期化子を持たない列挙値の定義では、`1`によって前の列挙値の値を増やすことによって取得される値が列挙子に与えられます。 この増加した値は、基になる型で表すことができる値の範囲内である必要があります。

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

上記の例では、列挙値とそれに関連付けられた値を出力します。 出力は次のようになります。

```console
Red = 0
Green = 10
Blue = 11
```

値の理由は次のとおりです。

* 列挙値 `Red` には、(初期化子がなく、最初の列挙値のメンバーであるため) `0` 値が自動的に割り当てられます。

* 列挙値 `Green` には、`10`値が明示的に指定されます。

* 列挙値 `Blue` には、その前にある列挙値よりも大きい値が自動的に割り当てられます。

定数式は、それ自体に関連付けられた列挙値の値を直接または間接的に使用することはできません (つまり、定数式での循環は許可されません)。 次の例は、`A` と `B` の宣言が循環しているため無効です。

```vb
Enum Circular
    A = B
    B
End Enum
```

`A` は `B` に明示的に依存し、`B` は暗黙的に `A` に依存します。

## <a name="classes"></a>クラス

*クラス*は、データメンバー (定数、変数、およびイベント)、関数メンバー (メソッド、プロパティ、インデクサー、演算子、およびコンストラクター)、および入れ子にされた型を含むことができるデータ構造です。 クラスは参照型です。

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

次の例は、各メンバーの種類を含むクラスを示しています。

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

これらのメンバーの使用例を次に示します。

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

クラス固有の修飾子には、`MustInherit` と `NotInheritable`の2つがあります。 両方を指定することはできません。


### <a name="class-base-specification"></a>クラスの基本指定

クラス宣言には、クラスの直接の基本型を定義する基本型の仕様を含めることができます。

```antlr
ClassBase
    : 'Inherits' NonArrayTypeName StatementTerminator
    ;
```

クラス宣言に明示的な基本型がない場合、直接の基本型は暗黙的に `Object`ます。 例 :

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class
```

基本型は独自の型パラメーターにすることはできませんが、スコープ内の型パラメーターが含まれる場合もあります。

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

クラスは、`Object` クラスおよびクラスからのみ派生できます。 クラスを `System.ValueType`、`System.Enum`、`System.Array`、`System.MulticastDelegate`、または `System.Delegate`から派生させることはできません。 ジェネリッククラスは、`System.Attribute` またはそれから派生したクラスから派生することはできません。

各クラスには直接基底クラスが1つだけあり、派生での循環は禁止されています。 `NotInheritable` クラスから派生させることはできません。また、基底クラスのアクセシビリティドメインは、クラス自体のアクセシビリティドメインのスーパーセットと同じであるか、またはそのスーパーセットと同じである必要があります。


### <a name="class-members"></a>クラス メンバー

クラスのメンバーは、そのクラスメンバー宣言によって導入されたメンバーと、その直接の基底クラスから継承されたメンバーで構成されます。

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

クラスメンバーの宣言には、`Public`、`Protected`、`Friend`、`Protected Friend`、または `Private` アクセスを含めることができます。 クラスメンバーの宣言にアクセス修飾子が含まれていない場合、宣言は、変数宣言でない限り、既定で `Public` アクセスになります。その場合は、既定で `Private` アクセスになります。

クラスメンバーのスコープは、メンバー宣言が発生するクラス本体と、そのクラスの制約リスト (ジェネリックであり、制約がある場合) です。 メンバーが `Friend` アクセス権を持っている場合、そのスコープは、同じプログラム内の任意の派生クラスのクラス本体、または `Friend` アクセスが与えられている任意のアセンブリにまで拡張されます。また、メンバーが `Public`、`Protected`、または `Protected Friend` アクセス権を持っている場合、そのスコープは任意のプログラム内の任意の派生クラスのクラス


## <a name="structures"></a>構造体

*構造体*は、`System.ValueType`から継承する値型です。 構造体は、データメンバーと関数メンバーを含むことができるデータ構造を表すという点でクラスに似ています。 ただし、クラスとは異なり、構造体はヒープ割り当てを必要としません。

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

クラスの場合、2つの変数が同じオブジェクトを参照する可能性があります。したがって、ある変数に対する操作が、もう一方の変数によって参照されるオブジェクトに影響を与える可能性があります。 構造体を使用すると、各変数には`Shared` 以外のデータの独自のコピーが含まれるため、次の例に示すように、1つの変数に対する操作がもう一方に影響することはありません。

```vb
Structure Point
    Public x, y As Integer

    Public Sub New(x As Integer, y As Integer)
        Me.x = x
        Me.y = y
    End Sub
End Structure
```

上記の宣言を指定すると、次のコードによって `10`値が出力されます。

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

`b` への `a` の割り当てによって値のコピーが作成されるため、`b` は `a.x`への割り当ての影響を受けません。 `Point` はクラスとして宣言されていましたが、`a` と `b` は同じオブジェクトを参照するため、出力が `100` されます。


### <a name="structure-members"></a>構造体のメンバー

構造体のメンバーは、構造体のメンバー宣言および `System.ValueType`から継承されたメンバーによって導入されるメンバーです。

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

すべての構造体には、構造体の既定値を生成する `Public` パラメーターなしのインスタンスコンストラクターが暗黙的に設定されます。 その結果、構造体型の宣言でパラメーターなしのインスタンスコンストラクターを宣言することはできません。 ただし、構造体型は、次の例のように、*パラメーター化*されたインスタンスコンストラクターを宣言できます。

```vb
Structure Point
    Private x, y As Integer

    Public Sub New(x As Integer, y As Integer)
        Me.x = x
        Me.y = y
    End Sub
End Structure
```

上記の宣言の場合、次のステートメントはどちらも `x` を持つ `Point` を作成し、`y` 0 に初期化されます。

```vb
Dim p1 As Point = New Point()
Dim p2 As Point = New Point(0, 0)
```

構造体には、それらの値を参照するのではなく、フィールド値が直接含まれているため、構造体には、自身を直接または間接的に参照するフィールドを含めることはできません。 たとえば、次のコードは無効です。

```vb
Structure S1
    Dim f1 As S2
End Structure

Structure S2
    ' This would require S1 to contain itself.
    Dim f1 As S1
End Structure
```

通常、構造体メンバーの宣言には、`Public`、`Friend`、または `Private` アクセスのみを指定できますが、`Object`から継承されたメンバーをオーバーライドする場合、`Protected` および `Protected Friend` のアクセスも使用できます。 構造体メンバーの宣言にアクセス修飾子が含まれていない場合、宣言は既定で `Public` アクセスになります。 構造体によって宣言されるメンバーのスコープは、宣言が発生する構造体の本体と、その構造体の制約 (ジェネリックであり、制約がある場合) です。


## <a name="standard-modules"></a>標準モジュール

*標準モジュール*は、標準モジュールの宣言自体に対してではなく、メンバーが暗黙的に `Shared` され、標準モジュールの名前空間の宣言空間をスコープとする型です。 標準モジュールをインスタンス化することはできません。 標準モジュール型の変数を宣言すると、エラーになります。

```antlr
ModuleDeclaration
    : Attributes? TypeModifier* 'Module' Identifier StatementTerminator
      ModuleMemberDeclaration*
      'End' 'Module' StatementTerminator
    ;
```

標準モジュールのメンバーには、2つの完全修飾名があります。1つは標準モジュール名を除いた名前で、もう1つは標準モジュール名を持ちます。 名前空間の複数の標準モジュールで、特定の名前を持つメンバーを定義できます。どちらのモジュールにも含まれていない名前への非修飾参照があいまいです。 例 :

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

モジュールは名前空間でのみ宣言できます。別の型に入れ子にすることはできません。 標準モジュールは、インターフェイスを実装しない場合があり、`Object`から暗黙的に派生し、`Shared` コンストラクターのみを持ちます。


### <a name="standard-module-members"></a>標準モジュールメンバー

標準モジュールのメンバーは、メンバー宣言および `Object`から継承されたメンバーによって導入されたメンバーです。 標準モジュールには、インスタンスコンストラクター以外の任意の型のメンバーを含めることができます。 標準モジュールの型のメンバーはすべて、暗黙的に `Shared`ます。

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

通常、標準モジュールメンバーの宣言には、`Public`、`Friend`、または `Private` アクセスのみがありますが、`Object`から継承されたメンバーをオーバーライドする場合は、`Protected` および `Protected Friend` のアクセス修飾子を指定できます。 標準モジュールメンバーの宣言にアクセス修飾子が含まれていない場合、既定では、変数でない限り、宣言は既定で `Public` アクセスになります。これは、既定で `Private` アクセスになります。

前述のように、標準モジュールメンバーのスコープは、標準モジュール宣言を含む宣言です。 `Object` から継承されたメンバーは、この特別なスコープには含まれません。これらのメンバーにはスコープがなく、常にモジュール名で修飾する必要があります。 メンバーが `Friend` アクセス権を持っている場合、そのスコープは、`Friend` アクセスが与えられている同じプログラムまたはアセンブリで宣言されている名前空間メンバーだけになります。


## <a name="interfaces"></a>インターフェイス

*インターフェイス*は、他の型が特定のメソッドをサポートすることを保証するために実装する参照型です。 インターフェイスは直接作成されることはなく、実際の表現はありません。他の型はインターフェイス型に変換する必要があります。 インターフェイスはコントラクトを定義します。 インターフェイスを実装するクラスまたは構造体は、コントラクトに従う必要があります。

```antlr
InterfaceDeclaration
    : Attributes? TypeModifier* 'Interface' Identifier
      TypeParameterList? StatementTerminator
      InterfaceBase*
      InterfaceMemberDeclaration*
      'End' 'Interface' StatementTerminator
    ;
```


次の例は、既定のプロパティ `Item`、イベント `E`、メソッド `F`、およびプロパティ `P`を含むインターフェイスを示しています。

```vb
Interface IExample
    Default Property Item(index As Integer) As String

    Event E()

    Sub F(value As Integer)

    Property P() As String
End Interface
```

インターフェイスは、多重継承を使用する場合があります。 次の例では、インターフェイス `IComboBox` `ITextBox` と `IListBox`の両方から継承されます。

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

クラスと構造体は、複数のインターフェイスを実装できます。 次の例では、クラス `EditBox` クラス `Control` から派生し、`IControl` と `IDataBound`の両方を実装しています。

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

インターフェイスの基本インターフェイスは、明示的な基本インターフェイスとその基本インターフェイスです。 つまり、基本インターフェイスのセットは、明示的な基本インターフェイス、その明示的な基本インターフェイスなどの完全な推移的なクロージャです。 インターフェイス宣言に明示的なインターフェイスベースがない場合、型の基本インターフェイスはありません。インターフェイスは `Object` から継承されません (ただし、`Object`への拡大変換があります)。

```antlr
InterfaceBase
    : 'Inherits' InterfaceBases StatementTerminator
    ;

InterfaceBases
    : NonArrayTypeName ( Comma NonArrayTypeName )*
    ;
```

次の例では、`IComboBox` の基本インターフェイスが `IControl`、`ITextBox`、および `IListBox`です。

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

インターフェイスは、その基本インターフェイスのすべてのメンバーを継承します。 言い換えると、上の `IComboBox` インターフェイスは、メンバー `SetText`、`SetItems` および `Paint`を継承します。

インターフェイスを実装するクラスまたは構造体は、インターフェイスのすべての基本インターフェイスも暗黙的に実装します。

インターフェイスが基底インターフェイスの推移的なクロージャに複数回出現する場合、そのインターフェイスは派生インターフェイスに1回だけメンバーを提供します。 派生インターフェイスを実装する型は、多重定義された基本インターフェイスのメソッドを1回だけ実装する必要があります。 次の例では、クラスが `IComboBox` と `IControl`を実装している場合でも、`Paint` を1回だけ実装する必要があります。

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

`Inherits` 句は、他の `Inherits` 句には影響しません。 次の例では、`IDerived` `INested` の名前を `IBase`で修飾する必要があります。

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

基底インターフェイスのアクセシビリティドメインは、インターフェイス自体のアクセシビリティドメインのスーパーセットと同じである必要があります。


### <a name="interface-members"></a>インターフェイスのメンバー

インターフェイスのメンバーは、メンバー宣言によって導入されたメンバーと、その基本インターフェイスから継承されたメンバーで構成されます。

```antlr
InterfaceMemberDeclaration
    : NonModuleDeclaration
    | InterfaceEventMemberDeclaration
    | InterfaceMethodMemberDeclaration
    | InterfacePropertyMemberDeclaration
    ;
```

インターフェイスは `Object`からメンバーを継承しませんが、インターフェイスを実装するすべてのクラスまたは構造体は `Object`から継承するため、拡張メソッドを含む `Object`のメンバーはインターフェイスのメンバーと見なされ、`Object`へのキャストを必要とせずにインターフェイスで直接呼び出すことができます。 例 :

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

`Object` のメンバーと同じ名前を持つインターフェイスのメンバーは、`Object` メンバーを暗黙的にシャドウします。 インターフェイスのメンバーには、入れ子にされた型、メソッド、プロパティ、およびイベントのみを含めることができます。 メソッドとプロパティに本体を含めることはできません。 インターフェイスメンバーは暗黙的に `Public` ますが、アクセス修飾子を指定することはできません。 インターフェイスで宣言されているメンバーのスコープは、宣言が行われるインターフェイス本体と、そのインターフェイスの制約リスト (ジェネリックで、制約がある場合) です。


## <a name="arrays"></a>配列

*配列*は、配列内の変数の順序を使用して、一対一の方法で*インデックス*を使用してアクセスされる変数を含む参照型です。 配列に含まれる変数は、配列の*要素*とも呼ばれ、すべて同じ型である必要があり、この型は配列の*要素型*と呼ばれます。

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

配列の要素は、配列のインスタンスが作成されるときに存在し、配列のインスタンスが破棄されると存在しなくなります。 配列の各要素は、その型の既定値に初期化されます。 `System.Array` 型は、すべての配列型の基本型であり、インスタンス化することはできません。 すべての配列型は、`System.Array` 型によって宣言されたメンバーを継承し、それに変換できます (と `Object`)。 要素 `T` を持つ1次元配列型は、インターフェイス `System.Collections.Generic.IList(Of T)` と `IReadOnlyList(Of T)`も実装します。`T` が参照型の場合、配列型は、`T`からの拡大参照変換を持つすべての `U` に対して `IList(Of U)` と `IReadOnlyList(Of U)` も実装します。

配列には、各配列要素に関連付けられているインデックスの数を決定する*ランク*があります。 配列のランクによって、配列の*次元*数が決まります。 たとえば、ランクが1の配列は1次元配列と呼ばれ、ランクが1より大きい配列は多次元配列と呼ばれます。

次の例では、整数値の1次元配列を作成し、配列要素を初期化して、それぞれを出力します。

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

プログラムは次の出力を出力します。

```console
arr(0) = 0
arr(1) = 1
arr(2) = 4
arr(3) = 9
arr(4) = 16
arr(5) = 25
```

配列の各次元には、関連付けられた長さがあります。 次元の長さは配列の型の一部ではなく、実行時に配列型のインスタンスが作成されるときに設定されます。 ディメンションの長さによって、そのディメンションのインデックスの有効範囲が決まります。長さが `N`のディメンションの場合、インデックスの範囲は 0 ~ `N-1`になります。 ディメンションの長さがゼロの場合、そのディメンションに有効なインデックスはありません。 配列内の要素の合計数は、配列内の各次元の長さの積です。 配列のいずれかの次元の長さがゼロの場合、配列は空であると言われます。 配列の要素型には、任意の型を指定できます。

配列型は、既存の型名に修飾子を追加することによって指定されます。 修飾子は、左かっこ、0個以上のコンマのセット、および右かっこで構成されます。 変更される型は配列の要素型で、次元の数はコンマと1を加算した数になります。 複数の修飾子が指定されている場合、配列の要素の型は配列になります。 修飾子は左から右に読み取られ、一番左の修飾子は最も外側の配列になります。 この例では、

```vb
Module Test
    Dim arr As Integer(,)(,,)()
End Module
```

`arr` の要素の型は、`Integer`の1次元配列の3次元配列の2次元配列です。

変数名に配列型修飾子または配列サイズ初期化修飾子を指定することにより、変数を配列型として宣言することもできます。 この場合、配列要素の型は宣言で指定された型であり、配列の次元は変数名修飾子によって決定されます。 わかりやすくするために、同じ宣言内の変数名と型名の両方に配列型修飾子を指定することは無効です。

次の例は、要素型として `Integer` を持つ配列型を使用する、さまざまなローカル変数宣言を示しています。

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

配列型の名前修飾子は、その後に続くすべてのかっこのセットに拡張されます。 これは、かっこで囲まれた一連の引数が型名の後で許可されている場合に、配列型名の引数を指定できないことを意味します。 例 :

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

最後の例では、`(3)` は、コンストラクター引数のセットとしてではなく、型名の一部として解釈されます。


## <a name="delegates"></a>デリゲート

*デリゲート*は、型の `Shared` メソッドまたはオブジェクトのインスタンスメソッドを参照する参照型です。

```antlr
DelegateDeclaration
    : Attributes? TypeModifier* 'Delegate' MethodSignature StatementTerminator
    ;

MethodSignature
    : SubSignature
    | FunctionSignature
    ;
```

 他の言語のデリゲートに最も近いものは関数ポインターですが、関数ポインターは `Shared` 関数だけを参照できますが、デリゲートは `Shared` とインスタンスメソッドの両方を参照できます。 後者の場合、デリゲートは、メソッドのエントリポイントへの参照だけでなく、メソッドを呼び出すために使用するオブジェクトインスタンスへの参照も格納します。

デリゲート宣言には、`Handles` 句、`Implements` 句、メソッド本体、または `End` コンストラクトを含めることはできません。 デリゲート宣言のパラメーターリストに `Optional` または `ParamArray` パラメーターを含めることはできません。 戻り値の型とパラメーターの型のアクセシビリティドメインは、またはデリゲート自体のアクセシビリティドメインのスーパーセットと同じである必要があります。

デリゲートのメンバーは、クラス `System.Delegate`から継承されたメンバーです。 デリゲートは、次のメソッドも定義します。

* `Object` 型の1つで `System.IntPtr`型の1つである2つのパラメーターを受け取るコンストラクター。

* デリゲートと同じシグネチャを持つ `Invoke` メソッド。

* シグネチャがデリゲートシグネチャで、3つの違いがある `BeginInvoke` メソッド。 最初に、戻り値の型が `System.IAsyncResult`に変更されます。 次に、2つの追加パラメーターがパラメーターリストの末尾に追加されます。 `System.AsyncCallback` 型の最初のパラメーターと `Object`型の2番目のパラメーターです。 最後に、すべての `ByRef` パラメーターが `ByVal`に変更されます。

* 戻り値の型がデリゲートと同じである `EndInvoke` メソッド。 メソッドのパラメーターは、デリゲートシグネチャで発生する順序と同じ順序で、`ByRef` パラメーターであるデリゲートパラメーターのみです。  これらのパラメーターに加えて、パラメーターリストの末尾に `System.IAsyncResult` 型の追加パラメーターがあります。

デリゲートの定義と使用には、宣言、インスタンス化、および呼び出しという3つの手順があります。

デリゲートは、デリゲート宣言の構文を使用して宣言されます。 次の例では、引数を受け取らない `SimpleDelegate` という名前のデリゲートを宣言しています。

```vb
Delegate Sub SimpleDelegate()
```

次の例では、`SimpleDelegate` インスタンスを作成し、すぐにそれを呼び出します。

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

メソッドを直接呼び出す方が簡単であるため、メソッドのデリゲートをインスタンス化してから、デリゲートを使用してすぐに呼び出す点はあまりありません。 デリゲートは、その匿名性が使用されている場合にその有用性を示します。 次の例は、`SimpleDelegate` インスタンスを繰り返し呼び出す `MultiCall` メソッドを示しています。

```vb
Sub MultiCall(d As SimpleDelegate, count As Integer)
    Dim i As Integer

    For i = 0 To count - 1
        d()
    Next i
End Sub
```

`SimpleDelegate` のターゲットメソッドがどのようなものであるか、このメソッドにどのようなアクセシビリティがあるか、またはメソッドが `Shared` かどうかにかかわらず、`MultiCall` メソッドにとっては重要ではありません。 重要なのは、ターゲットメソッドのシグネチャが `SimpleDelegate`と互換性があることです。


## <a name="partial-types"></a>部分型

クラス宣言と構造体宣言は、*部分*宣言にすることができます。 宣言内の宣言された型を部分宣言で完全に記述することはできません。 代わりに、型の宣言をプログラム内の複数の部分宣言にわたって分散させることができます。部分型は、プログラムの境界を越えて宣言することはできません。 部分型の宣言では、宣言の `Partial` 修飾子を指定します。 次に、同じ完全修飾名を持つ型のプログラム内のその他の宣言は、コンパイル時に部分宣言と共にマージされ、単一の型宣言を形成します。 たとえば、次のコードでは、`Test.C1` と `Test.C2`メンバーを持つ1つのクラス `Test` を宣言しています。

.vb:

```vb
Public Partial Class Test
    Public Sub S1()
    End Sub
End Class
```

b. vb:

```vb
Public Class Test
    Public Sub S2()
    End Sub
End Class
```

部分型宣言を組み合わせる場合、少なくとも1つの宣言に `Partial` 修飾子が必要です。それ以外の場合は、コンパイル時エラーが発生します。

__付箋.__ 複数の部分宣言の中で1つの宣言に対してのみ `Partial` を指定することもできますが、すべての部分宣言で指定する方が適切な形式です。 1つの部分宣言が表示されていても、1つまたは複数の部分宣言が非表示になっている場合 (ツールによって生成されたコードを拡張する場合など)、表示されている宣言の `Partial` 修飾子をそのままにしておくことはできますが、非表示の宣言では指定できます。

部分宣言を使用して宣言できるのは、クラスと構造体だけです。 部分宣言を照合するときに、型のアリティが考慮されます。同じ名前の2つのクラスが、型パラメーターの数が異なる場合、同じ時刻の部分宣言とは見なされません。 部分宣言では、属性、クラス修飾子、`Inherits` ステートメントまたは `Implements` ステートメントを指定できます。 コンパイル時に、部分宣言のすべての部分が結合され、型宣言の一部として使用されます。 属性、修飾子、基底クラス、インターフェイス、または型のメンバーの間に競合がある場合、コンパイル時エラーが発生します。 例 :

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

前の例では、`Public`であり、`Object` から継承され `System.IDisposable` と `System.IComparable`を実装する型 `Test1` を宣言しています。 宣言の1つは `Test2` が `Public` であり、もう1つは `Test2` が `Private`であると示されているため、`Test2` の部分宣言によってコンパイル時エラーが発生します。

型パラメーターを持つ部分型では、型パラメーターの制約と分散を宣言できますが、各部分宣言の制約と分散は一致している必要があります。 そのため、制約と分散は、他の修飾子と同様に自動的に結合されないという意味で特別です。

```vb
Partial Public Class List(Of T As IEnumerable)
End Class

' Error: Constraints on T don't match
Class List(Of T As IComparable)
End Class
```

型が複数の部分宣言を使用して宣言されているという事実は、型内の名前の参照規則には影響しません。 その結果、部分型宣言は、他の部分型宣言で宣言されたメンバーを使用したり、他の部分型宣言で宣言されたインターフェイスにメソッドを実装したりすることができます。 例 :

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

入れ子になった型にも、部分宣言を含めることができます。 例 :

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

部分宣言内の初期化子は、宣言の順序で実行されます。ただし、個別の部分宣言で発生する初期化子の実行順序は保証されていません。

## <a name="constructed-types"></a>構築された型

ジェネリック型の宣言自体が型を表していません。 代わりに、ジェネリック型の宣言を "ブループリント" として使用して、型引数を適用してさまざまな型を形成できます。 型引数が適用されているジェネリック型は、*構築型*と呼ばれます。 構築された型の型引数は、一致する型パラメーターに対して設定された制約を常に満たしている必要があります。

型パラメーターを直接指定しない場合でも、型名は構築された型を識別することがあります。 これは、ジェネリッククラス宣言内で型が入れ子になっている場合に発生する可能性があり、包含する宣言のインスタンスの型は、暗黙的に名前の参照に使用されます。

```vb
Class Outer(Of T) 
    Public Class Inner 
    End Class

    ' Type of i is the constructed type Outer(Of T).Inner
    Public i As Inner 
End Class
```

ジェネリック型とすべての型引数にアクセスできる場合は、構築された型 `C(Of T1,...,Tn)` にアクセスできます。 たとえば、ジェネリック型 `C` が `Public`、すべての型引数 `T1,...,Tn` が `Public`の場合、構築された型は `Public`になります。 ただし、型名または型引数のいずれかが `Private`場合は、構築された型のアクセシビリティが `Private`ます。 構築された型の1つの型引数が `Protected`、別の型引数が `Friend`場合、構築された型は、このアセンブリ内のクラスとそのサブクラス、および `Friend` アクセス権が付与されているアセンブリ内でのみアクセス可能です。 つまり、構築された型のアクセシビリティドメインは、その構成要素のアクセシビリティドメインの積集合になります。

__付箋.__ 構築された型のアクセシビリティドメインは、そのた層の部分の積集合であるため、新しいアクセシビリティレベルを定義するという興味深い副作用があります。 `Protected` の要素と `Friend` されている要素を含む構築された型は、`Friend`*と*`Protected` の*両方*のメンバーにアクセスできるコンテキストでのみアクセスできます。 ただし、このアクセシビリティレベルを言語で表す方法はありません。アクセシビリティ `Protected Friend` とは、`Friend`*または*`Protected` メンバーにアクセスできるコンテキストでエンティティに*アクセスできる*ことを意味します。

基本の実装されたインターフェイスと構築された型のメンバーは、ジェネリック型の型パラメーターが出現するたびに、指定された型引数を置き換えることによって決定されます。

### <a name="open-types-and-closed-types"></a>オープン型と閉じられた型

1つ以上の型引数が、包含する型またはメソッドの型パラメーターである場合、構築された型は*オープン型*と呼ばれます。 これは、型の一部の型パラメーターがまだ認識されていないため、型の実際の図形はまだ完全には認識されていないためです。 これに対し、型引数がすべて非型パラメーターであるジェネリック型は、 *closed 型*と呼ばれます。 クローズ型の形状は常に完全に認識されます。 例 :

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

型パラメーター `T` が指定されていますが、型パラメーター `U` に別の型パラメーターが指定されているため、構築された型 `Base(Of Integer, V)` はオープン型です。 したがって、型の完全な形はまだ知られていません。 ただし、構築された型 `Derived(Of Double)`は、継承階層内のすべての型パラメーターが指定されているため、閉じられた型です。

オープン型は次のように定義されます。

* 型パラメーターはオープン型です。

* 要素型がオープン型である場合、配列型はオープン型です。

* 1つ以上の型引数がオープン型である場合、構築された型はオープン型になります。

* 閉じられた型は、オープン型ではない型です。

プログラムのエントリポイントをジェネリック型にすることはできないため、実行時に使用されるすべての型は閉じられた型になります。

## <a name="special-types"></a>特殊な型

.NET Framework には、.NET Framework と Visual Basic 言語によって特別に処理される多数のクラスが含まれています。

.NET Framework 内の void 型を表す `System.Void`型は、`GetType` 式でのみ直接参照できます。

`System.RuntimeArgumentHandle`、`System.ArgIterator`、および `System.TypedReference` の型には、スタックへのポインターを含めることができます。したがって、.NET Framework ヒープには記述できません。 したがって、配列要素型、戻り値の型、フィールドの型、ジェネリック型引数、null 許容型、`ByRef` パラメーターの型、`Object` または `System.ValueType`に変換される値の型、`Object` または `System.ValueType`のインスタンスメンバーへの呼び出しの対象、またはクロージャへのリフトされた値の型として使用することはできません。
