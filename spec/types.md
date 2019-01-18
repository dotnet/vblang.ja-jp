---
ms.openlocfilehash: 6815d084e180bb615fcc5c880e2ee5127b56c4a4
ms.sourcegitcommit: 6eca149bdc736113e0adb709212bd266c9503c33
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2019
ms.locfileid: "47426768"
---
# <a name="types"></a><span data-ttu-id="770bc-101">種類</span><span class="sxs-lookup"><span data-stu-id="770bc-101">Types</span></span>

<span data-ttu-id="770bc-102">Visual Basic における型の 2 つの基本的なカテゴリは*値の型*と*参照型*します。</span><span class="sxs-lookup"><span data-stu-id="770bc-102">The two fundamental categories of types in Visual Basic are *value types* and *reference types*.</span></span> <span data-ttu-id="770bc-103">プリミティブ型 (文字列) を除く、列挙型、および構造体には値の種類です。</span><span class="sxs-lookup"><span data-stu-id="770bc-103">Primitive types (except strings), enumerations, and structures are value types.</span></span> <span data-ttu-id="770bc-104">クラス、文字列、標準的なモジュール、インターフェイス、配列、およびデリゲートは、参照型です。</span><span class="sxs-lookup"><span data-stu-id="770bc-104">Classes, strings, standard modules, interfaces, arrays, and delegates are reference types.</span></span>

<span data-ttu-id="770bc-105">すべての型が、*既定値*、これは、初期化時にその型の変数に割り当てられている値。</span><span class="sxs-lookup"><span data-stu-id="770bc-105">Every type has a *default value*, which is the value that is assigned to variables of that type upon initialization.</span></span>

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

## <a name="value-types-and-reference-types"></a><span data-ttu-id="770bc-106">値型と参照型</span><span class="sxs-lookup"><span data-stu-id="770bc-106">Value Types and Reference Types</span></span>

<span data-ttu-id="770bc-107">値型と参照型指定できますが、宣言の構文と使用状況と同様なセマンティクスが異なります。</span><span class="sxs-lookup"><span data-stu-id="770bc-107">Although value types and reference types can be similar in terms of declaration syntax and usage, their semantics are distinct.</span></span>

<span data-ttu-id="770bc-108">参照型は、ランタイム ヒープに格納されています。また、その記憶域への参照によってのみアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="770bc-108">Reference types are stored on the run-time heap; they may only be accessed through a reference to that storage.</span></span> <span data-ttu-id="770bc-109">参照型は、参照を使用して常にアクセスされる、ため、その有効期間は、.NET Framework によって管理されます。</span><span class="sxs-lookup"><span data-stu-id="770bc-109">Because reference types are always accessed through references, their lifetime is managed by the .NET Framework.</span></span> <span data-ttu-id="770bc-110">特定のインスタンスに未解決の参照を追跡し、残っている参照がない場合にのみ、インスタンスは破棄されます。</span><span class="sxs-lookup"><span data-stu-id="770bc-110">Outstanding references to a particular instance are tracked and the instance is destroyed only when no more references remain.</span></span> <span data-ttu-id="770bc-111">参照型の変数には、その型の値より強い派生型の値または null 値への参照が含まれています。</span><span class="sxs-lookup"><span data-stu-id="770bc-111">A variable of reference type contains a reference to a value of that type, a value of a more derived type, or a null value.</span></span> <span data-ttu-id="770bc-112">A *null 値*指す何もありません。 代入以外、null 値を持つことが何もすることはできません。</span><span class="sxs-lookup"><span data-stu-id="770bc-112">A *null value* refers to nothing; it is not possible to do anything with a null value except assign it.</span></span> <span data-ttu-id="770bc-113">参照型の変数への代入では、参照されている値のコピーではなく、参照のコピーを作成します。</span><span class="sxs-lookup"><span data-stu-id="770bc-113">Assignment to a variable of a reference type creates a copy of the reference rather than a copy of the referenced value.</span></span> <span data-ttu-id="770bc-114">参照型の変数は、既定値は、null 値は。</span><span class="sxs-lookup"><span data-stu-id="770bc-114">For a variable of a reference type, the default value is a null value.</span></span>

<span data-ttu-id="770bc-115">値の型が直接アレイ内または別の型では、スタックに格納されています。その記憶域は、直接のみアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="770bc-115">Value types are stored directly on the stack, either within an array or within another type; their storage can only be accessed directly.</span></span> <span data-ttu-id="770bc-116">値の型は変数内に直接格納されている、ため、有効期間は、それを含む変数の有効期間によって決まります。</span><span class="sxs-lookup"><span data-stu-id="770bc-116">Because value types are stored directly within variables, their lifetime is determined by the lifetime of the variable that contains them.</span></span> <span data-ttu-id="770bc-117">値型のインスタンスを格納している場所が破棄されると、値型のインスタンスは破棄されます。</span><span class="sxs-lookup"><span data-stu-id="770bc-117">When the location containing a value type instance is destroyed, the value type instance is also destroyed.</span></span> <span data-ttu-id="770bc-118">値の型は直接は常にアクセスします。値型への参照を作成することはできません。</span><span class="sxs-lookup"><span data-stu-id="770bc-118">Value types are always accessed directly; it is not possible to create a reference to a value type.</span></span> <span data-ttu-id="770bc-119">このような参照を禁止することと、破棄されている値クラスのインスタンスを指すため不可能になります。</span><span class="sxs-lookup"><span data-stu-id="770bc-119">Prohibiting such a reference makes it impossible to refer to a value class instance that has been destroyed.</span></span> <span data-ttu-id="770bc-120">値の型が常にため`NotInheritable`値型の変数には常にその型の値が含まれています。</span><span class="sxs-lookup"><span data-stu-id="770bc-120">Because value types are always `NotInheritable`, a variable of a value type always contains a value of that type.</span></span> <span data-ttu-id="770bc-121">このため、値型の値が null の値にすることはできません。 またより強い派生型のオブジェクトを参照することができます。</span><span class="sxs-lookup"><span data-stu-id="770bc-121">Because of this, the value of a value type cannot be a null value, nor can it reference an object of a more derived type.</span></span> <span data-ttu-id="770bc-122">値型の変数への代入では、割り当てられている値のコピーを作成します。</span><span class="sxs-lookup"><span data-stu-id="770bc-122">Assignment to a variable of a value type creates a copy of the value being assigned.</span></span> <span data-ttu-id="770bc-123">値型の変数は、既定値は、その既定値の型の各変数のメンバーの初期化の結果は。</span><span class="sxs-lookup"><span data-stu-id="770bc-123">For a variable of a value type, the default value is the result of initializing each variable member of the type to its default value.</span></span>

<span data-ttu-id="770bc-124">次の例は、参照型と値の型の違いを示しています。</span><span class="sxs-lookup"><span data-stu-id="770bc-124">The following example shows the difference between reference types and value types:</span></span>

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

<span data-ttu-id="770bc-125">プログラムの出力は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="770bc-125">The output of the program is:</span></span>

```
Values: 0, 123
Refs: 123, 123
```

<span data-ttu-id="770bc-126">ローカル変数に代入`val2`ローカル変数には影響しません`val1`両方のローカル変数は値型であるため (種類`Integer`) 値の型のそれぞれのローカル変数が独自のストレージとします。</span><span class="sxs-lookup"><span data-stu-id="770bc-126">The assignment to the local variable `val2` does not impact the local variable `val1` because both local variables are of a value type (the type `Integer`) and each local variable of a value type has its own storage.</span></span> <span data-ttu-id="770bc-127">これに対して、割り当て`ref2.Value = 123;`オブジェクトに影響を両方`ref1`と`ref2`参照。</span><span class="sxs-lookup"><span data-stu-id="770bc-127">In contrast, the assignment `ref2.Value = 123;` affects the object that both `ref1` and `ref2` reference.</span></span>

<span data-ttu-id="770bc-128">.NET Framework の型システムについて注意が必要ですがいる場合でも構造体、列挙型とプリミティブ型 (以外の`String`) は、値の型すべて参照型から継承します。</span><span class="sxs-lookup"><span data-stu-id="770bc-128">One thing to note about the .NET Framework type system is that even though structures, enumerations and primitive types (except for `String`) are value types, they all inherit from reference types.</span></span> <span data-ttu-id="770bc-129">構造体およびプリミティブ型が参照型から継承`System.ValueType`から継承される`Object`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-129">Structures and the primitive types inherit from the reference type `System.ValueType`, which inherits from `Object`.</span></span> <span data-ttu-id="770bc-130">列挙型が参照型から継承`System.Enum`から継承される`System.ValueType`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-130">Enumerated types inherit from the reference type `System.Enum`, which inherits from `System.ValueType`.</span></span>

### <a name="nullable-value-types"></a><span data-ttu-id="770bc-131">null 許容値型</span><span class="sxs-lookup"><span data-stu-id="770bc-131">Nullable Value Types</span></span>

<span data-ttu-id="770bc-132">値型の場合、`?`修飾子を表す型名に追加できる、 *null 許容*その型のバージョン。</span><span class="sxs-lookup"><span data-stu-id="770bc-132">For value types, a `?` modifier can be added to a type name to represent the *nullable* version of that type.</span></span>

```antlr
NullableTypeName
    : NonArrayTypeName '?'
    ;

NullableNameModifier
    : '?'
    ;
```

<span data-ttu-id="770bc-133">Null 許容値型は、型の null 非許容のバージョンと同じ値と null 値に含めることができます。</span><span class="sxs-lookup"><span data-stu-id="770bc-133">A nullable value type can contain the same values as the non-nullable version of the type as well as the null value.</span></span> <span data-ttu-id="770bc-134">そのため、null 許容値型の割り当て`Nothing`型の変数に null 値にゼロ値ではなく、値型の変数の値を設定します。</span><span class="sxs-lookup"><span data-stu-id="770bc-134">Thus, for a nullable value type, assigning `Nothing` to a variable of the type sets the value of the variable to the null value, not the zero value of the value type.</span></span> <span data-ttu-id="770bc-135">例:</span><span class="sxs-lookup"><span data-stu-id="770bc-135">For example:</span></span>

```vb
Dim x As Integer = Nothing
Dim y As Integer? = Nothing

' Prints zero
Console.WriteLine(x)
' Prints nothing (because the value of y is the null value)
Console.WriteLine(y)
```

<span data-ttu-id="770bc-136">変数名を null 許容型修飾子を配置することで null 許容値型である変数を宣言することも。</span><span class="sxs-lookup"><span data-stu-id="770bc-136">A variable may also be declared to be of a nullable value type by putting a nullable type modifier on the variable name.</span></span> <span data-ttu-id="770bc-137">わかりやすくするため、同じ宣言で変数名と型名の両方で null 許容型修飾子があることはできません。</span><span class="sxs-lookup"><span data-stu-id="770bc-137">For clarity, it is not valid to have a nullable type modifier on both a variable name and a type name in the same declaration.</span></span> <span data-ttu-id="770bc-138">Null 許容型は、型を使用して実装されるため`System.Nullable(Of T)`、型`T?`型には`System.Nullable(Of T)`、2 つの名前を同じ意味で使用できます。</span><span class="sxs-lookup"><span data-stu-id="770bc-138">Since nullable types are implemented using the type `System.Nullable(Of T)`, the type `T?` is synonymous to the type `System.Nullable(Of T)`, and the two names can be used interchangeably.</span></span> <span data-ttu-id="770bc-139">`?`修飾子は、null 許容の型に配置できません。 そのため、型を宣言することはできません`Integer??`または`System.Nullable(Of Integer)?`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-139">The `?` modifier cannot be placed on a type that is already nullable; thus, it is not possible to declare the type `Integer??` or `System.Nullable(Of Integer)?`.</span></span>

<span data-ttu-id="770bc-140">Null 許容値型`T?`のメンバーを持つ`System.Nullable(Of T)`演算子または変換と*リフト*基になる型から`T`型に`T?`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-140">A nullable value type `T?` has the members of `System.Nullable(Of T)` as well as any operators or conversions *lifted* from the underlying type `T` into the type `T?`.</span></span> <span data-ttu-id="770bc-141">ほとんどの場合、null 非許容値型の null 許容値型の置換コピー演算子と、基になる型から変換を変換します。</span><span class="sxs-lookup"><span data-stu-id="770bc-141">Lifting copies operators and conversions from the underlying type, in most cases substituting nullable value types for non-nullable value types.</span></span> <span data-ttu-id="770bc-142">これにより、同じ変換とに適用される操作の多く`T`に適用する`T?`もします。</span><span class="sxs-lookup"><span data-stu-id="770bc-142">This allows many of the same conversions and operations that apply to `T` to apply to `T?` as well.</span></span>


## <a name="interface-implementation"></a><span data-ttu-id="770bc-143">インターフェイスの実装</span><span class="sxs-lookup"><span data-stu-id="770bc-143">Interface Implementation</span></span>

<span data-ttu-id="770bc-144">構造体とクラスの宣言が 1 つ以上のインターフェイス型のセットを実装することを宣言`Implements`句。</span><span class="sxs-lookup"><span data-stu-id="770bc-144">Structure and class declarations may declare that they implement a set of interface types through one or more `Implements` clauses.</span></span>

```antlr
TypeImplementsClause
    : 'Implements' TypeImplements StatementTerminator
    ;

TypeImplements
    : NonArrayTypeName ( Comma NonArrayTypeName )*
    ;
```

<span data-ttu-id="770bc-145">指定されたすべての型、`Implements`句は、インターフェイスである必要があり、型がインターフェイスのすべてのメンバーを実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="770bc-145">All the types specified in the `Implements` clause must be interfaces, and the type must implement all members of the interfaces.</span></span> <span data-ttu-id="770bc-146">例:</span><span class="sxs-lookup"><span data-stu-id="770bc-146">For example:</span></span>

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

<span data-ttu-id="770bc-147">暗黙的にインターフェイスを実装する型では、すべてのインターフェイスの基本インターフェイスを実装します。</span><span class="sxs-lookup"><span data-stu-id="770bc-147">A type that implements an interface also implicitly implements all of the interface's base interfaces.</span></span> <span data-ttu-id="770bc-148">これは、型がですべての基本インターフェイスを明示的に表示されない場合でも true、`Implements`句。</span><span class="sxs-lookup"><span data-stu-id="770bc-148">This is true even if the type does not explicitly list all base interfaces in the `Implements` clause.</span></span> <span data-ttu-id="770bc-149">この例で、`TextBox`両方を実装して`IControl`と`ITextBox`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-149">In this example, the `TextBox` structure implements both `IControl` and `ITextBox`.</span></span>

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

<span data-ttu-id="770bc-150">宣言の型がインターフェイスを実装するの場合、型の宣言領域には何も宣言していません。</span><span class="sxs-lookup"><span data-stu-id="770bc-150">Declaring that a type implements an interface in and of itself does not declare anything in the declaration space of the type.</span></span> <span data-ttu-id="770bc-151">そのため、これはメソッドを使用して、同じ名前で 2 つのインターフェイスを実装するために有効です。</span><span class="sxs-lookup"><span data-stu-id="770bc-151">Thus, it is valid to implement two interfaces with a method by the same name.</span></span>

<span data-ttu-id="770bc-152">型はスコープ内の型パラメーターがありますが、独自の型パラメーターを実装できません。</span><span class="sxs-lookup"><span data-stu-id="770bc-152">Types cannot implement a type parameter on its own, although it may involve the type parameters that are in scope.</span></span>

```vb
Class C1(Of V)
    Implements V  ' Error, can't implement type parameter directly
    Implements IEnumerable(Of V)  ' OK, not directly implementing

    ...
End Class
```

<span data-ttu-id="770bc-153">ジェネリック インターフェイスは実装されている複数の異なる型引数を使用して回を指定できます。</span><span class="sxs-lookup"><span data-stu-id="770bc-153">Generic interfaces can be implemented multiple times using different type arguments.</span></span> <span data-ttu-id="770bc-154">ただし、ジェネリック型は、(型制約) に関係なく指定した型パラメーターがそのインターフェイスの別の実装と重複する場合は、型パラメーターを使用してジェネリック インターフェイスを実装することはできません。</span><span class="sxs-lookup"><span data-stu-id="770bc-154">However, a generic type cannot implement a generic interface using a type parameter if the supplied type parameter (regardless of type constraints) could overlap with another implementation of that interface.</span></span> <span data-ttu-id="770bc-155">例:</span><span class="sxs-lookup"><span data-stu-id="770bc-155">For example:</span></span>

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


## <a name="primitive-types"></a><span data-ttu-id="770bc-156">プリミティブ型</span><span class="sxs-lookup"><span data-stu-id="770bc-156">Primitive Types</span></span>

<span data-ttu-id="770bc-157">*プリミティブ型*、定義済みのエイリアスであるキーワードを使って識別型、`System`名前空間。</span><span class="sxs-lookup"><span data-stu-id="770bc-157">The *primitive types* are identified through keywords, which are aliases for predefined types in the `System` namespace.</span></span> <span data-ttu-id="770bc-158">プリミティブ型は型の完全に区別は、エイリアス: 予約語の書き込み`Byte`が正確に記述と同じ`System.Byte`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-158">A primitive type is completely indistinguishable from the type it aliases: writing the reserved word `Byte` is exactly the same as writing `System.Byte`.</span></span> <span data-ttu-id="770bc-159">プリミティブ型とも呼ばれます*組み込み型*します。</span><span class="sxs-lookup"><span data-stu-id="770bc-159">Primitive types are also known as *intrinsic types*.</span></span>

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


<span data-ttu-id="770bc-160">プリミティブ型の別名の通常の型は、ため、すべてのプリミティブ型はメンバーがあります。</span><span class="sxs-lookup"><span data-stu-id="770bc-160">Because a primitive type aliases a regular type, every primitive type has members.</span></span> <span data-ttu-id="770bc-161">たとえば、`Integer`で宣言されたメンバーを持つ`System.Int32`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-161">For example, `Integer` has the members declared in `System.Int32`.</span></span> <span data-ttu-id="770bc-162">リテラルは、対応する型のインスタンスとして扱うことができます。</span><span class="sxs-lookup"><span data-stu-id="770bc-162">Literals can be treated as instances of their corresponding types.</span></span>

* <span data-ttu-id="770bc-163">プリミティブ型は、特定の追加操作を行うことで他の構造体の型とは異なります。</span><span class="sxs-lookup"><span data-stu-id="770bc-163">The primitive types differ from other structure types in that they permit certain additional operations:</span></span>

* <span data-ttu-id="770bc-164">プリミティブ型では、値のリテラルを記述することで作成されることを許可します。</span><span class="sxs-lookup"><span data-stu-id="770bc-164">Primitive types permit values to be created by writing literals.</span></span> <span data-ttu-id="770bc-165">たとえば、`123I`型のリテラルは、`Integer`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-165">For example, `123I` is a literal of type `Integer`.</span></span>

* <span data-ttu-id="770bc-166">プリミティブ型の定数を宣言することになります。</span><span class="sxs-lookup"><span data-stu-id="770bc-166">It is possible to declare constants of the primitive types.</span></span>

* <span data-ttu-id="770bc-167">すべてのプリミティブ型の定数を式のオペランドには、コンパイラはコンパイル時に式の評価になります。</span><span class="sxs-lookup"><span data-stu-id="770bc-167">When the operands of an expression are all primitive type constants, it is possible for the compiler to evaluate the expression at compile time.</span></span> <span data-ttu-id="770bc-168">このような式は定数式と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="770bc-168">Such an expression is known as a constant expression.</span></span>

<span data-ttu-id="770bc-169">Visual Basic では、次のプリミティブ型を定義します。</span><span class="sxs-lookup"><span data-stu-id="770bc-169">Visual Basic defines the following primitive types:</span></span>

* <span data-ttu-id="770bc-170">整数値の型`Byte`(1 バイトの符号なし整数)、 `SByte` (1 バイトの符号付き整数)、 `UShort` (2 バイト符号なし整数)、 `Short` (2 バイト符号付き整数)、 `UInteger` (4 バイト符号なし整数)、 `Integer` (4 バイト符号付き整数)、 `ULong` (8 バイト符号なし整数)、および`Long`(8 バイト符号付き整数)。</span><span class="sxs-lookup"><span data-stu-id="770bc-170">The integral value types `Byte` (1-byte unsigned integer), `SByte` (1-byte signed integer), `UShort` (2-byte unsigned integer), `Short` (2-byte signed integer), `UInteger` (4-byte unsigned integer), `Integer` (4-byte signed integer), `ULong` (8-byte unsigned integer), and `Long` (8-byte signed integer).</span></span> <span data-ttu-id="770bc-171">これらの型にマップ`System.Byte`、 `System.SByte`、 `System.UInt16`、 `System.Int16`、 `System.UInt32`、 `System.Int32`、`System.UInt64`と`System.Int64`、それぞれします。</span><span class="sxs-lookup"><span data-stu-id="770bc-171">These types map to `System.Byte`, `System.SByte`, `System.UInt16`, `System.Int16`, `System.UInt32`, `System.Int32`, `System.UInt64` and `System.Int64`, respectively.</span></span> <span data-ttu-id="770bc-172">整数型の既定値は、リテラルに相当`0`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-172">The default value of an integral type is equivalent to the literal `0`.</span></span>

* <span data-ttu-id="770bc-173">浮動小数点値の型`Single`(4 バイト浮動小数点) と`Double`(8 バイト浮動小数点)。</span><span class="sxs-lookup"><span data-stu-id="770bc-173">The floating-point value types `Single` (4-byte floating point) and `Double` (8-byte floating point).</span></span> <span data-ttu-id="770bc-174">これらの型にマップ`System.Single`と`System.Double`、それぞれします。</span><span class="sxs-lookup"><span data-stu-id="770bc-174">These types map to `System.Single` and `System.Double`, respectively.</span></span> <span data-ttu-id="770bc-175">浮動小数点型の既定値は、リテラルに相当`0`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-175">The default value of a floating-point type is equivalent to the literal `0`.</span></span>

* <span data-ttu-id="770bc-176">`Decimal`型 (16 バイト 10 進値) にマップする`System.Decimal`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-176">The `Decimal` type (16-byte decimal value), which maps to `System.Decimal`.</span></span> <span data-ttu-id="770bc-177">10 進数の既定値は、リテラルに相当`0D`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-177">The default value of decimal is equivalent to the literal `0D`.</span></span>

* <span data-ttu-id="770bc-178">`Boolean`値の型で、リレーショナルまたは論理操作の結果では通常、実際の値を表します。</span><span class="sxs-lookup"><span data-stu-id="770bc-178">The `Boolean` value type, which represents a truth value, typically the result of a relational or logical operation.</span></span> <span data-ttu-id="770bc-179">型のリテラルは、`System.Boolean`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-179">The literal is of type `System.Boolean`.</span></span> <span data-ttu-id="770bc-180">既定値、`Boolean`型がリテラルと等価`False`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-180">The default value of the `Boolean` type is equivalent to the literal `False`.</span></span>

* <span data-ttu-id="770bc-181">`Date`値の型を日付や時刻を表します。 マップ`System.DateTime`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-181">The `Date` value type, which represents a date and/or a time and maps to `System.DateTime`.</span></span> <span data-ttu-id="770bc-182">既定値、`Date`型がリテラルと等価`# 01/01/0001 12:00:00AM #`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-182">The default value of the `Date` type is equivalent to the literal `# 01/01/0001 12:00:00AM #`.</span></span>

* <span data-ttu-id="770bc-183">`Char`値を 1 つの Unicode 文字を表します。 マップの種類、`System.Char`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-183">The `Char` value type, which represents a single Unicode character and maps to `System.Char`.</span></span> <span data-ttu-id="770bc-184">既定値、`Char`型が定数式と等価`ChrW(0)`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-184">The default value of the `Char` type is equivalent to the constant expression `ChrW(0)`.</span></span>

* <span data-ttu-id="770bc-185">`String`参照型では、Unicode 文字のシーケンスを表しにマップする`System.String`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-185">The `String` reference type, which represents a sequence of Unicode characters and maps to `System.String`.</span></span> <span data-ttu-id="770bc-186">既定値、`String`型が null の値。</span><span class="sxs-lookup"><span data-stu-id="770bc-186">The default value of the `String` type is a null value.</span></span>



## <a name="enumerations"></a><span data-ttu-id="770bc-187">列挙</span><span class="sxs-lookup"><span data-stu-id="770bc-187">Enumerations</span></span>

<span data-ttu-id="770bc-188">*列挙体*値の型から継承するは`System.Enum`一連のプリミティブの整数型のいずれかの値を表します。</span><span class="sxs-lookup"><span data-stu-id="770bc-188">*Enumerations* are value types that inherit from `System.Enum` and symbolically represent a set of values of one of the primitive integral types.</span></span>

```antlr
EnumDeclaration
    : Attributes? TypeModifier* 'Enum' Identifier
      ( 'As' NonArrayTypeName )? StatementTerminator
      EnumMemberDeclaration+
      'End' 'Enum' StatementTerminator
    ;
```

<span data-ttu-id="770bc-189">列挙型の`E`、既定値は、式によって生成される値`CType(0, E)`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-189">For an enumeration type `E`, the default value is the value produced by the expression `CType(0, E)`.</span></span>

<span data-ttu-id="770bc-190">列挙体の基になる型は、列挙で定義されているすべての列挙子の値を表す整数型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="770bc-190">The underlying type of an enumeration must be an integral type that can represent all the enumerator values defined in the enumeration.</span></span> <span data-ttu-id="770bc-191">基になる型が指定されている場合があります`Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、 `ULong`、 `Long`、またはいずれかに対応する型、 `System`名前空間。</span><span class="sxs-lookup"><span data-stu-id="770bc-191">If an underlying type is specified, it must be `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, or one of their corresponding types in the `System` namespace.</span></span> <span data-ttu-id="770bc-192">基になる型が明示的に指定されていない場合、既定値は`Integer`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-192">If no underlying type is explicitly specified, the default is `Integer`.</span></span>

<span data-ttu-id="770bc-193">次の例では、基になる型の列挙体の宣言`Long`:</span><span class="sxs-lookup"><span data-stu-id="770bc-193">The following example declares an enumeration with an underlying type of `Long`:</span></span>

```vb
Enum Color As Long
    Red
    Green
    Blue
End Enum
```

<span data-ttu-id="770bc-194">開発者は、基になる型を使用することができます`Long`などの範囲内にある値の使用を有効にするには、`Long`の範囲内にありませんが、 `Integer`、または将来には、このオプションを保持するためにします。</span><span class="sxs-lookup"><span data-stu-id="770bc-194">A developer might choose to use an underlying type of `Long`, as in the example, to enable the use of values that are in the range of `Long`, but not in the range of `Integer`, or to preserve this option for the future.</span></span>


### <a name="enumeration-members"></a><span data-ttu-id="770bc-195">列挙型メンバー</span><span class="sxs-lookup"><span data-stu-id="770bc-195">Enumeration Members</span></span>

<span data-ttu-id="770bc-196">列挙体のメンバーは、列挙体で宣言された列挙値およびクラスから継承されたメンバー`System.Enum`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-196">The members of an enumeration are the enumerated values declared in the enumeration and the members inherited from class `System.Enum`.</span></span>

<span data-ttu-id="770bc-197">列挙体のメンバーのスコープは、列挙型宣言の本体です。</span><span class="sxs-lookup"><span data-stu-id="770bc-197">The scope of an enumeration member is the enumeration declaration body.</span></span> <span data-ttu-id="770bc-198">意味する列挙体の宣言の外側列挙体のメンバーする必要があります常に修飾 (種類が名前空間インポートを使用して、名前空間にインポートされていない場合)。</span><span class="sxs-lookup"><span data-stu-id="770bc-198">This means that outside of an enumeration declaration, an enumeration member must always be qualified (unless the type is specifically imported into a namespace through a namespace import).</span></span>

<span data-ttu-id="770bc-199">定数式の値を省略すると、列挙メンバーの宣言の順序は重要です。</span><span class="sxs-lookup"><span data-stu-id="770bc-199">Declaration order for enumeration member declarations is significant when constant expression values are omitted.</span></span> <span data-ttu-id="770bc-200">列挙型メンバーが暗黙的が`Public`のみへのアクセスは、列挙体メンバーの宣言でアクセス修飾子は許可されません。</span><span class="sxs-lookup"><span data-stu-id="770bc-200">Enumeration members implicitly have `Public` access only; no access modifiers are allowed on enumeration member declarations.</span></span>

```antlr
EnumMemberDeclaration
    : Attributes? Identifier ( Equals ConstantExpression )? StatementTerminator
    ;
```

### <a name="enumeration-values"></a><span data-ttu-id="770bc-201">列挙値</span><span class="sxs-lookup"><span data-stu-id="770bc-201">Enumeration Values</span></span>

<span data-ttu-id="770bc-202">列挙体のメンバー一覧の列挙値は、基になる列挙型として型指定された定数と定数が必要な任意の場所に表示されることができますに宣言されます。</span><span class="sxs-lookup"><span data-stu-id="770bc-202">The enumerated values in an enumeration member list are declared as constants typed as the underlying enumeration type, and they can appear wherever constants are required.</span></span> <span data-ttu-id="770bc-203">列挙メンバーの定義に`=`定数式で表される値に関連付けられているメンバーを提供します。</span><span class="sxs-lookup"><span data-stu-id="770bc-203">An enumeration member definition with `=` gives the associated member the value indicated by the constant expression.</span></span> <span data-ttu-id="770bc-204">定数式では、基になる型に暗黙的に変換できる整数型に評価される必要があり、基になる型で表現できる値の範囲内である必要があります。</span><span class="sxs-lookup"><span data-stu-id="770bc-204">The constant expression must evaluate to an integral type that is implicitly convertible to the underlying type and must be within the range of values that can be represented by the underlying type.</span></span> <span data-ttu-id="770bc-205">に、次の例がエラーでは、定数値`1.5`、 `2.3`、および`3.3`は、基になる整数型に暗黙的に変換できません`Long`厳密なセマンティクスを持つ。</span><span class="sxs-lookup"><span data-stu-id="770bc-205">The following example is in error because the constant values `1.5`, `2.3`, and `3.3` are not implicitly convertible to the underlying integral type `Long` with strict semantics.</span></span>

```vb
Option Strict On

Enum Color As Long
    Red = 1.5
    Green = 2.3
    Blue = 3.3
End Enum
```

<span data-ttu-id="770bc-206">次に示すよう、複数の列挙体メンバーは、同じ関連付けられた値を共有可能性があります。</span><span class="sxs-lookup"><span data-stu-id="770bc-206">Multiple enumeration members may share the same associated value, as shown below:</span></span>

```vb
Enum Color
    Red
    Green
    Blue
    Max = Blue
End Enum
```

<span data-ttu-id="770bc-207">-2 つの列挙体メンバーを持つ列挙体の例を示します`Blue`と`Max`--ことが同じ関連付けられている値。</span><span class="sxs-lookup"><span data-stu-id="770bc-207">The example shows an enumeration that has two enumeration members -- `Blue` and `Max` -- that have the same associated value.</span></span>

<span data-ttu-id="770bc-208">列挙体の最初の列挙子の値の定義が初期化子を持たない場合、対応する定数の値は`0`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-208">If the first enumerator value definition in the enumeration has no initializer, the value of the corresponding constant is `0`.</span></span> <span data-ttu-id="770bc-209">初期化子のない列挙値の定義は、列挙子が前の列挙値の値を増やすことで得られた値`1`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-209">An enumeration value definition without an initializer gives the enumerator the value obtained by increasing the value of the previous enumeration value by `1`.</span></span> <span data-ttu-id="770bc-210">この増加する値は、基になる型で表すことができる値の範囲内にある必要があります。</span><span class="sxs-lookup"><span data-stu-id="770bc-210">This increased value must be within the range of values that can be represented by the underlying type.</span></span>

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

<span data-ttu-id="770bc-211">上記の例では、列挙値と関連付けられた値を出力します。</span><span class="sxs-lookup"><span data-stu-id="770bc-211">The example above prints the enumeration values and their associated values.</span></span> <span data-ttu-id="770bc-212">出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="770bc-212">The output is:</span></span>

```
Red = 0
Green = 10
Blue = 11
```

<span data-ttu-id="770bc-213">値の理由は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="770bc-213">The reasons for the values are as follows:</span></span>

* <span data-ttu-id="770bc-214">列挙値`Red`、値が自動的に割り当てられて`0`(初期化子を持たない列挙型値の最初のメンバーであるため)。</span><span class="sxs-lookup"><span data-stu-id="770bc-214">The enumeration value `Red` is automatically assigned the value `0` (since it has no initializer and is the first enumeration value member).</span></span>

* <span data-ttu-id="770bc-215">列挙値`Green`値が明示的に指定`10`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-215">The enumeration value `Green` is explicitly given the value `10`.</span></span>

* <span data-ttu-id="770bc-216">列挙値`Blue`直前列挙の値より 1 大きい値を自動的に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="770bc-216">The enumeration value `Blue` is automatically assigned the value one greater than the enumeration value that textually precedes it.</span></span>

<span data-ttu-id="770bc-217">定数式が直接または間接的に値を使用しません独自の関連する列挙値の (つまり、定数式での循環は許可されません)。</span><span class="sxs-lookup"><span data-stu-id="770bc-217">The constant expression may not directly or indirectly use the value of its own associated enumeration value (that is, circularity in the constant expression is not allowed).</span></span> <span data-ttu-id="770bc-218">次の例が有効でないための宣言`A`と`B`が循環します。</span><span class="sxs-lookup"><span data-stu-id="770bc-218">The following example is invalid because the declarations of `A` and `B` are circular.</span></span>

```vb
Enum Circular
    A = B
    B
End Enum
```

<span data-ttu-id="770bc-219">`A` 依存`B`明示的と`B`異なります`A`暗黙的にします。</span><span class="sxs-lookup"><span data-stu-id="770bc-219">`A` depends on `B` explicitly, and `B` depends on `A` implicitly.</span></span>

## <a name="classes"></a><span data-ttu-id="770bc-220">クラス</span><span class="sxs-lookup"><span data-stu-id="770bc-220">Classes</span></span>

<span data-ttu-id="770bc-221">A*クラス*はデータ メンバー (定数、変数、およびイベント)、関数メンバー (メソッド、プロパティ、インデクサー、演算子、およびコンス トラクター) および入れ子にされた型を含む可能性のあるデータ構造です。</span><span class="sxs-lookup"><span data-stu-id="770bc-221">A *class* is a data structure that may contain data members (constants, variables, and events), function members (methods, properties, indexers, operators, and constructors), and nested types.</span></span> <span data-ttu-id="770bc-222">クラスは、参照型です。</span><span class="sxs-lookup"><span data-stu-id="770bc-222">Classes are reference types.</span></span>

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

<span data-ttu-id="770bc-223">次の例では、各種類のメンバーを含むクラスを示します。</span><span class="sxs-lookup"><span data-stu-id="770bc-223">The following example shows a class that contains each kind of member:</span></span>

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

<span data-ttu-id="770bc-224">次の例では、これらのメンバーの使用を示します。</span><span class="sxs-lookup"><span data-stu-id="770bc-224">The following example shows uses of these members:</span></span>

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

<span data-ttu-id="770bc-225">2 つのクラスに固有の修飾子がある`MustInherit`と`NotInheritable`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-225">There are two class-specific modifiers, `MustInherit` and `NotInheritable`.</span></span> <span data-ttu-id="770bc-226">両方を指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="770bc-226">It is invalid to specify them both.</span></span>


### <a name="class-base-specification"></a><span data-ttu-id="770bc-227">クラスの基本仕様</span><span class="sxs-lookup"><span data-stu-id="770bc-227">Class Base Specification</span></span>

<span data-ttu-id="770bc-228">クラス宣言では、クラスの直接の基本型を定義する基本データ型仕様を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="770bc-228">A class declaration may include a base type specification that defines the direct base type of the class.</span></span>

```antlr
ClassBase
    : 'Inherits' NonArrayTypeName StatementTerminator
    ;
```

<span data-ttu-id="770bc-229">明示的な基本型、クラス宣言がない場合は、直接の基本型は暗黙的に`Object`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-229">If a class declaration has no explicit base type, the direct base type is implicitly `Object`.</span></span> <span data-ttu-id="770bc-230">例:</span><span class="sxs-lookup"><span data-stu-id="770bc-230">For example:</span></span>

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class
```

<span data-ttu-id="770bc-231">スコープ内の型パラメーターがありますが、基本データ型は、独自の型パラメーターにすることはできません。</span><span class="sxs-lookup"><span data-stu-id="770bc-231">Base types cannot be a type parameter on its own, although it may involve the type parameters that are in scope.</span></span>

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

<span data-ttu-id="770bc-232">クラスからのみ派生`Object`とクラス。</span><span class="sxs-lookup"><span data-stu-id="770bc-232">Classes may only derive from `Object` and classes.</span></span> <span data-ttu-id="770bc-233">クラスから派生することはできません`System.ValueType`、 `System.Enum`、 `System.Array`、`System.MulticastDelegate`または`System.Delegate`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-233">It is invalid for a class to derive from `System.ValueType`, `System.Enum`, `System.Array`, `System.MulticastDelegate` or `System.Delegate`.</span></span> <span data-ttu-id="770bc-234">ジェネリック クラスから派生できません`System.Attribute`またはそこから派生するクラス。</span><span class="sxs-lookup"><span data-stu-id="770bc-234">A generic class cannot derive from `System.Attribute` or from a class that derives from it.</span></span>

<span data-ttu-id="770bc-235">すべてのクラスが 1 つだけの直接基底クラスと派生での循環は禁止されています。</span><span class="sxs-lookup"><span data-stu-id="770bc-235">Every class has exactly one direct base class, and circularity in derivation is prohibited.</span></span> <span data-ttu-id="770bc-236">派生することはできません、`NotInheritable`クラス、および基底クラスのアクセシビリティ ドメインは、クラス自体のアクセシビリティ ドメインのスーパー セットであるかと同じにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="770bc-236">It is not possible to derive from a `NotInheritable` class, and the accessibility domain of the base class must be the same as or a superset of the accessibility domain of the class itself.</span></span>


### <a name="class-members"></a><span data-ttu-id="770bc-237">クラス メンバー</span><span class="sxs-lookup"><span data-stu-id="770bc-237">Class Members</span></span>

<span data-ttu-id="770bc-238">クラスのメンバーは、クラス メンバー宣言で導入されたメンバーとその直接の基本クラスから継承されたメンバーで構成されます。</span><span class="sxs-lookup"><span data-stu-id="770bc-238">The members of a class consist of the members introduced by its class member declarations and the members inherited from its direct base class.</span></span>

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

<span data-ttu-id="770bc-239">クラス メンバーの宣言があります`Public`、 `Protected`、 `Friend`、 `Protected Friend`、または`Private`アクセスします。</span><span class="sxs-lookup"><span data-stu-id="770bc-239">A class member declaration may have `Public`, `Protected`, `Friend`, `Protected Friend`, or `Private` access.</span></span> <span data-ttu-id="770bc-240">クラス メンバーの宣言に、アクセス修飾子が含まれていない場合、宣言既定`Public`アクセス、変数の宣言がある場合を除き、その場合は既定値は`Private`アクセスします。</span><span class="sxs-lookup"><span data-stu-id="770bc-240">When a class member declaration does not include an access modifier, the declaration defaults to `Public` access, unless it is a variable declaration; in that case it defaults to `Private` access.</span></span>

<span data-ttu-id="770bc-241">クラス メンバーのスコープは、(ジェネリックし、制約があります) 場合は、メンバーの宣言が行われているクラス本体とそのクラスの制約リストです。</span><span class="sxs-lookup"><span data-stu-id="770bc-241">The scope of a class member is the class body in which the member declaration occurs, plus the constraint list of that class (if it is generic and has constraints).</span></span> <span data-ttu-id="770bc-242">メンバーがある場合`Friend`を同じプログラム内で任意の派生クラスまたは与えられているすべてのアセンブリのクラスの本文にアクセス、そのスコープの拡張`Friend`アクセス、メンバーがある場合と`Public`、 `Protected`、または`Protected Friend`そのスコープにアクセスします。すべてのプログラムで任意の派生クラスのクラス本体に拡張されます。</span><span class="sxs-lookup"><span data-stu-id="770bc-242">If the member has `Friend` access, its scope extends to the class body of any derived class in the same program or any assembly that has been given `Friend` access, and if the member has `Public`, `Protected`, or `Protected Friend` access, its scope extends to the class body of any derived class in any program.</span></span>


## <a name="structures"></a><span data-ttu-id="770bc-243">構造体</span><span class="sxs-lookup"><span data-stu-id="770bc-243">Structures</span></span>

<span data-ttu-id="770bc-244">*構造体*値の型から継承するは`System.ValueType`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-244">*Structures* are value types that inherit from `System.ValueType`.</span></span> <span data-ttu-id="770bc-245">データ メンバーおよび関数メンバーを格納できるデータ構造を表しているという点では、構造体をクラスに似ています。</span><span class="sxs-lookup"><span data-stu-id="770bc-245">Structures are similar to classes in that they represent data structures that can contain data members and function members.</span></span> <span data-ttu-id="770bc-246">クラスと異なり、ただし、構造体は不要ヒープ割り当てです。</span><span class="sxs-lookup"><span data-stu-id="770bc-246">Unlike classes, however, structures do not require heap allocation.</span></span>

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

<span data-ttu-id="770bc-247">クラスの場合は、同じオブジェクトを参照する 2 つの変数とできるため、他の変数によって参照されるオブジェクトに影響を与える 1 つの変数に対する操作です。</span><span class="sxs-lookup"><span data-stu-id="770bc-247">In the case of classes, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="770bc-248">各変数以外の独自のコピーのある構造を持つ`Shared`データ、ため、次の例に示すように影響を与える、他のいずれかの操作にことはできません。</span><span class="sxs-lookup"><span data-stu-id="770bc-248">With structures, the variables each have their own copy of the non-`Shared` data, so it is not possible for operations on one to affect the other, as the following example illustrates:</span></span>

```vb
Structure Point
    Public x, y As Integer

    Public Sub New(x As Integer, y As Integer)
        Me.x = x
        Me.y = y
    End Sub
End Structure
```

<span data-ttu-id="770bc-249">次のコードは、値を出力宣言`10`:</span><span class="sxs-lookup"><span data-stu-id="770bc-249">Given the above declaration the following code outputs the value `10`:</span></span>

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

<span data-ttu-id="770bc-250">割り当て`a`に`b`、値のコピーを作成および`b`はそのためへの割り当てによって影響を受けません`a.x`。</span><span class="sxs-lookup"><span data-stu-id="770bc-250">The assignment of `a` to `b` creates a copy of the value, and `b` is thus unaffected by the assignment to `a.x`.</span></span> <span data-ttu-id="770bc-251">`Point`されて代わりに出力になりますが、クラスとして宣言すると、`100`ため`a`と`b`同じオブジェクトを参照します。</span><span class="sxs-lookup"><span data-stu-id="770bc-251">Had `Point` instead been declared as a class, the output would be `100` because `a` and `b` would reference the same object.</span></span>


### <a name="structure-members"></a><span data-ttu-id="770bc-252">構造体のメンバー</span><span class="sxs-lookup"><span data-stu-id="770bc-252">Structure Members</span></span>

<span data-ttu-id="770bc-253">構造体のメンバーは、その構造体のメンバー宣言で導入されたメンバーとメンバーが継承`System.ValueType`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-253">The members of a structure are the members introduced by its structure member declarations and the members inherited from `System.ValueType`.</span></span>

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

<span data-ttu-id="770bc-254">すべての構造体は暗黙的に、`Public`構造体の既定値を生成するパラメーターなしコンス トラクター。</span><span class="sxs-lookup"><span data-stu-id="770bc-254">Every structure implicitly has a `Public` parameterless instance constructor that produces the default value of the structure.</span></span> <span data-ttu-id="770bc-255">その結果、構造型宣言のパラメーターなしインターフェイス コンス トラクターを宣言することはできません。</span><span class="sxs-lookup"><span data-stu-id="770bc-255">As a result, it is not possible for a structure type declaration to declare a parameterless instance constructor.</span></span> <span data-ttu-id="770bc-256">構造体の型が宣言で、ただし、許可されている*パラメーター化された*コンス トラクターの場合は、次の例のように、インスタンスします。</span><span class="sxs-lookup"><span data-stu-id="770bc-256">A structure type is, however, permitted to declare *parameterized* instance constructors, as in the following example:</span></span>

```vb
Structure Point
    Private x, y As Integer

    Public Sub New(x As Integer, y As Integer)
        Me.x = x
        Me.y = y
    End Sub
End Structure
```

<span data-ttu-id="770bc-257">上記の宣言を指定するには、次のステートメントは両方の作成、`Point`で`x`と`y`0 に初期化します。</span><span class="sxs-lookup"><span data-stu-id="770bc-257">Given the above declaration, the following statements both create a `Point` with `x` and `y` initialized to zero.</span></span>

```vb
Dim p1 As Point = New Point()
Dim p2 As Point = New Point(0, 0)
```

<span data-ttu-id="770bc-258">直接構造体には、そのフィールドの値 (なくそれらの値への参照) が含まれている、ために、構造体は、直接または間接的にそれ自体を参照するフィールドを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="770bc-258">Because structures directly contain their field values (rather than references to those values), structures cannot contain fields that directly or indirectly reference themselves.</span></span> <span data-ttu-id="770bc-259">たとえば、次のコードが無効です。</span><span class="sxs-lookup"><span data-stu-id="770bc-259">For example, the following code is not valid:</span></span>

```vb
Structure S1
    Dim f1 As S2
End Structure

Structure S2
    ' This would require S1 to contain itself.
    Dim f1 As S1
End Structure
```

<span data-ttu-id="770bc-260">通常、構造体のメンバー宣言の必要がありますのみ`Public`、 `Friend`、または`Private`から継承されたメンバーをオーバーライドするときに、アクセス`Object`、`Protected`と`Protected Friend`へのアクセスにも使用できます。</span><span class="sxs-lookup"><span data-stu-id="770bc-260">Normally, a structure member declaration may only have `Public`, `Friend`, or `Private` access, but when overriding members inherited from `Object`, `Protected` and `Protected Friend` access may also be used.</span></span> <span data-ttu-id="770bc-261">構造体メンバーの宣言に、アクセス修飾子が含まれていない場合、宣言既定`Public`アクセスします。</span><span class="sxs-lookup"><span data-stu-id="770bc-261">When a structure member declaration does not include an access modifier, the declaration defaults to `Public` access.</span></span> <span data-ttu-id="770bc-262">構造体で宣言されたメンバーのスコープは、(ジェネリックあったし、制約がありました) 場合は、宣言が行われている構造体の本体とその構造の制約です。</span><span class="sxs-lookup"><span data-stu-id="770bc-262">The scope of a member declared by a structure is the structure body in which the declaration occurs, plus the constraints of that structure (if it was generic and had constraints).</span></span>


## <a name="standard-modules"></a><span data-ttu-id="770bc-263">標準的なモジュール</span><span class="sxs-lookup"><span data-stu-id="770bc-263">Standard Modules</span></span>

<span data-ttu-id="770bc-264">A*標準モジュール*は、暗黙的にメンバーを含む型は、`Shared`標準モジュール宣言自体にではなく、標準のモジュールを含む名前空間の宣言領域にスコープとします。</span><span class="sxs-lookup"><span data-stu-id="770bc-264">A *standard module* is a type whose members are implicitly `Shared` and scoped to the declaration space of the standard module's containing namespace, rather than just to the standard module declaration itself.</span></span> <span data-ttu-id="770bc-265">標準的なモジュールをインスタンス化されないことがあります。</span><span class="sxs-lookup"><span data-stu-id="770bc-265">Standard modules may never be instantiated.</span></span> <span data-ttu-id="770bc-266">標準的なモジュール型の変数を宣言するとエラーになります。</span><span class="sxs-lookup"><span data-stu-id="770bc-266">It is an error to declare a variable of a standard module type.</span></span>

```antlr
ModuleDeclaration
    : Attributes? TypeModifier* 'Module' Identifier StatementTerminator
      ModuleMemberDeclaration*
      'End' 'Module' StatementTerminator
    ;
```

<span data-ttu-id="770bc-267">標準モジュールのメンバーでは、もう 1 つ、標準のモジュール名のない標準のモジュールの名前を持つ 2 つの完全修飾名を持ちます。</span><span class="sxs-lookup"><span data-stu-id="770bc-267">A member of a standard module has two fully qualified names, one without the standard module name and one with the standard module name.</span></span> <span data-ttu-id="770bc-268">名前空間内の 1 つ以上の標準モジュールは、特定の名前を持つメンバーを定義できます。モジュールのいずれかの外部名への非修飾の参照があいまいです。</span><span class="sxs-lookup"><span data-stu-id="770bc-268">More than one standard module in a namespace may define a member with a particular name; unqualified references to the name outside of either module are ambiguous.</span></span> <span data-ttu-id="770bc-269">例:</span><span class="sxs-lookup"><span data-stu-id="770bc-269">For example:</span></span>

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

<span data-ttu-id="770bc-270">モジュールは、名前空間で宣言することは、別の型に入れ子にすることがありますできません。</span><span class="sxs-lookup"><span data-stu-id="770bc-270">A module may only be declared in a namespace and may not be nested in another type.</span></span> <span data-ttu-id="770bc-271">標準的なモジュールはインターフェイスを実装していないから暗黙的に派生`Object`、およびのみが`Shared`コンス トラクター。</span><span class="sxs-lookup"><span data-stu-id="770bc-271">Standard modules may not implement interfaces, they implicitly derive from `Object`, and they have only `Shared` constructors.</span></span>


### <a name="standard-module-members"></a><span data-ttu-id="770bc-272">標準モジュールのメンバー</span><span class="sxs-lookup"><span data-stu-id="770bc-272">Standard Module Members</span></span>

<span data-ttu-id="770bc-273">標準モジュールのメンバーは、そのメンバーの宣言で導入されたメンバーとメンバーが継承`Object`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-273">The members of a standard module are the members introduced by its member declarations and the members inherited from `Object`.</span></span> <span data-ttu-id="770bc-274">標準的なモジュールには、インスタンス コンス トラクター以外のメンバーの任意の型があります。</span><span class="sxs-lookup"><span data-stu-id="770bc-274">Standard modules may have any type of member except instance constructors.</span></span> <span data-ttu-id="770bc-275">すべての標準的なモジュール型のメンバーは、暗黙的に`Shared`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-275">All standard module type members are implicitly `Shared`.</span></span>

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

<span data-ttu-id="770bc-276">通常、標準モジュールのメンバー宣言の必要がありますのみ`Public`、 `Friend`、または`Private`から継承されたメンバーをオーバーライドするときに、アクセス`Object`、`Protected`と`Protected Friend`アクセス修飾子を指定することがあります。</span><span class="sxs-lookup"><span data-stu-id="770bc-276">Normally, a standard module member declaration may only have `Public`, `Friend`, or `Private` access, but when overriding members inherited from `Object`, the `Protected` and `Protected Friend` access modifiers may be specified.</span></span> <span data-ttu-id="770bc-277">標準モジュール メンバーの宣言に、アクセス修飾子が含まれていない場合、宣言既定`Public`アクセス、既定では、変数である場合を除き`Private`アクセスします。</span><span class="sxs-lookup"><span data-stu-id="770bc-277">When a standard module member declaration does not include an access modifier, the declaration defaults to `Public` access, unless it is a variable, which defaults to `Private` access.</span></span>

<span data-ttu-id="770bc-278">前述のようは、標準モジュール メンバーのスコープは、標準のモジュール宣言を含む宣言です。</span><span class="sxs-lookup"><span data-stu-id="770bc-278">As previously noted, the scope of a standard module member is the declaration containing the standard module declaration.</span></span> <span data-ttu-id="770bc-279">継承したメンバー`Object`この特別なスコープ規則に含まれないメンバー スコープがない場合や、モジュールの名前で常に修飾する必要があります。</span><span class="sxs-lookup"><span data-stu-id="770bc-279">Members inherited from `Object` are not included in this special scoping; those members have no scope and must always be qualified with the name of the module.</span></span> <span data-ttu-id="770bc-280">メンバーがある場合`Friend`アクセス、そのスコープが同じプログラムまたは指定されているアセンブリで宣言された名前空間のメンバーにのみ拡張`Friend`アクセスします。</span><span class="sxs-lookup"><span data-stu-id="770bc-280">If the member has `Friend` access, its scope extends only to namespace members declared in the same program or assemblies that have been given `Friend` access.</span></span>


## <a name="interfaces"></a><span data-ttu-id="770bc-281">インターフェイス</span><span class="sxs-lookup"><span data-stu-id="770bc-281">Interfaces</span></span>

<span data-ttu-id="770bc-282">*インターフェイス*は参照型の特定のメソッドをサポートしていることを保証するための他の型に実装します。</span><span class="sxs-lookup"><span data-stu-id="770bc-282">*Interfaces* are reference types that other types implement to guarantee that they support certain methods.</span></span> <span data-ttu-id="770bc-283">インターフェイスは直接作成され、実際の表現がありません - その他の型はインターフェイス型に変換する必要があります。</span><span class="sxs-lookup"><span data-stu-id="770bc-283">An interface is never directly created and has no actual representation -- other types must be converted to an interface type.</span></span> <span data-ttu-id="770bc-284">インターフェイスは、コントラクトを定義します。</span><span class="sxs-lookup"><span data-stu-id="770bc-284">An interface defines a contract.</span></span> <span data-ttu-id="770bc-285">クラスまたはインターフェイスを実装する構造体は、そのコントラクトに従う必要があります。</span><span class="sxs-lookup"><span data-stu-id="770bc-285">A class or structure that implements an interface must adhere to its contract.</span></span>

```antlr
InterfaceDeclaration
    : Attributes? TypeModifier* 'Interface' Identifier
      TypeParameterList? StatementTerminator
      InterfaceBase*
      InterfaceMemberDeclaration*
      'End' 'Interface' StatementTerminator
    ;
```


<span data-ttu-id="770bc-286">次の例では、既定のプロパティを格納しているインターフェイス`Item`、イベント`E`、メソッド`F`、プロパティと、 `P`:</span><span class="sxs-lookup"><span data-stu-id="770bc-286">The following example shows an interface that contains a default property `Item`, an event `E`, a method `F`, and a property `P`:</span></span>

```vb
Interface IExample
    Default Property Item(index As Integer) As String

    Event E()

    Sub F(value As Integer)

    Property P() As String
End Interface
```

<span data-ttu-id="770bc-287">インターフェイスは、多重継承を採用できます。</span><span class="sxs-lookup"><span data-stu-id="770bc-287">Interfaces may employ multiple inheritance.</span></span> <span data-ttu-id="770bc-288">次の例では、インターフェイスで`IComboBox`両方から継承`ITextBox`と`IListBox`:</span><span class="sxs-lookup"><span data-stu-id="770bc-288">In the following example, the interface `IComboBox` inherits from both `ITextBox` and `IListBox`:</span></span>

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

<span data-ttu-id="770bc-289">クラスと構造体には、複数のインターフェイスを実装できます。</span><span class="sxs-lookup"><span data-stu-id="770bc-289">Classes and structures can implement multiple interfaces.</span></span> <span data-ttu-id="770bc-290">次の例では、クラスで`EditBox`クラスから派生`Control`両方を実装および`IControl`と`IDataBound`:</span><span class="sxs-lookup"><span data-stu-id="770bc-290">In the following example, the class `EditBox` derives from the class `Control` and implements both `IControl` and `IDataBound`:</span></span>

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


### <a name="interface-inheritance"></a><span data-ttu-id="770bc-291">インターフェイスの継承</span><span class="sxs-lookup"><span data-stu-id="770bc-291">Interface Inheritance</span></span>

<span data-ttu-id="770bc-292">インターフェイスの基本インターフェイスは、明示的な基本インターフェイスの基本インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="770bc-292">The base interfaces of an interface are the explicit base interfaces and their base interfaces.</span></span> <span data-ttu-id="770bc-293">つまり、基本インターフェイスのセットは、明示的な基本インターフェイス、明示的な基本インターフェイス、およびなどの完全な推移閉包です。</span><span class="sxs-lookup"><span data-stu-id="770bc-293">In other words, the set of base interfaces is the complete transitive closure of the explicit base interfaces, their explicit base interfaces, and so on.</span></span> <span data-ttu-id="770bc-294">インターフェイスの宣言を持たない明示的なインターフェイス ベース、型の基本インターフェイスはありません--インターフェイスから継承しない場合`Object`(があるへの拡大変換が`Object`)。</span><span class="sxs-lookup"><span data-stu-id="770bc-294">If an interface declaration has no explicit interface base, then there is no base interface for the type -- interfaces do not inherit from `Object` (although they do have a widening conversion to `Object`).</span></span>

```antlr
InterfaceBase
    : 'Inherits' InterfaceBases StatementTerminator
    ;

InterfaceBases
    : NonArrayTypeName ( Comma NonArrayTypeName )*
    ;
```

<span data-ttu-id="770bc-295">次の例では、インターフェイスの基本`IComboBox`は`IControl`、 `ITextBox`、および`IListBox`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-295">In the following example, the base interfaces of `IComboBox` are `IControl`, `ITextBox`, and `IListBox`.</span></span>

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

<span data-ttu-id="770bc-296">インターフェイスは、その基本インターフェイスのすべてのメンバーを継承します。</span><span class="sxs-lookup"><span data-stu-id="770bc-296">An interface inherits all members of its base interfaces.</span></span> <span data-ttu-id="770bc-297">つまり、`IComboBox`上記のインターフェイス メンバーを継承する`SetText`と`SetItems`だけでなく`Paint`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-297">In other words, the `IComboBox` interface above inherits members `SetText` and `SetItems` as well as `Paint`.</span></span>

<span data-ttu-id="770bc-298">クラスまたは構造体も暗黙的にインターフェイスを実装するには、すべてのインターフェイスの基本インターフェイスを実装します。</span><span class="sxs-lookup"><span data-stu-id="770bc-298">A class or structure that implements an interface also implicitly implements all of the interface's base interfaces.</span></span>

<span data-ttu-id="770bc-299">基本インターフェイスの推移的なクロージャでインターフェイスが複数回表示された場合のみ機能がわかりやすい派生インターフェイスをそのメンバーを 1 回です。</span><span class="sxs-lookup"><span data-stu-id="770bc-299">If an interface appears more than once in the transitive closure of the base interfaces, it only contributes its members to the derived interface once.</span></span> <span data-ttu-id="770bc-300">基底インターフェイスを 1 回定義のみ、派生インターフェイスは、乗算のメソッドを実装するには実装する型。</span><span class="sxs-lookup"><span data-stu-id="770bc-300">A type implementing the derived interface only has to implement the methods of the multiply defined base interface once.</span></span> <span data-ttu-id="770bc-301">次の例では、`Paint`クラスを実装する場合でも、1 回実装するだけ`IComboBox`と`IControl`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-301">In the following example, `Paint` only needs to be implemented once, even though the class implements `IComboBox` and `IControl`.</span></span>

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

<span data-ttu-id="770bc-302">`Inherits`句が他の影響を与えません`Inherits`句。</span><span class="sxs-lookup"><span data-stu-id="770bc-302">An `Inherits` clause has no effect on other `Inherits` clauses.</span></span> <span data-ttu-id="770bc-303">次の例では、`IDerived`の名前を修飾する必要があります`INested`で`IBase`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-303">In the following example, `IDerived` must qualify the name of `INested` with `IBase`.</span></span>

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

<span data-ttu-id="770bc-304">基底インターフェイスのアクセシビリティ ドメインは、インターフェイス自体のアクセシビリティ ドメインのスーパー セットであるかと同じである必要があります。</span><span class="sxs-lookup"><span data-stu-id="770bc-304">The accessibility domain of a base interface must be the same as or a superset of the accessibility domain of the interface itself.</span></span>


### <a name="interface-members"></a><span data-ttu-id="770bc-305">インターフェイスのメンバー</span><span class="sxs-lookup"><span data-stu-id="770bc-305">Interface Members</span></span>

<span data-ttu-id="770bc-306">インターフェイスのメンバーは、そのメンバーの宣言で導入されたメンバーとその基底インターフェイスから継承されたメンバーで構成されます。</span><span class="sxs-lookup"><span data-stu-id="770bc-306">The members of an interface consist of the members introduced by its member declarations and the members inherited from its base interfaces.</span></span>

```antlr
InterfaceMemberDeclaration
    : NonModuleDeclaration
    | InterfaceEventMemberDeclaration
    | InterfaceMethodMemberDeclaration
    | InterfacePropertyMemberDeclaration
    ;
```

<span data-ttu-id="770bc-307">インターフェイス メンバーを継承しない`Object`からすべてのクラスまたはインターフェイスを実装する構造体を継承しているため、`Object`のメンバー `Object`、拡張メソッドなど、インターフェイスのメンバーと見なされますキャストを必要とせずに直接インターフェイスで呼び出すことができますと`Object`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-307">Although interfaces do not inherit members from `Object`, because every class or structure that implements an interface does inherit from `Object`, the members of `Object`, including extension methods, are considered members of an interface and can be called on an interface directly without requiring a cast to `Object`.</span></span> <span data-ttu-id="770bc-308">例:</span><span class="sxs-lookup"><span data-stu-id="770bc-308">For example:</span></span>

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

<span data-ttu-id="770bc-309">メンバーとして同じ名前のインターフェイスのメンバー`Object`暗黙的にシャドウ`Object`メンバー。</span><span class="sxs-lookup"><span data-stu-id="770bc-309">Members of an interface with the same name as members of `Object` implicitly shadow `Object` members.</span></span> <span data-ttu-id="770bc-310">インターフェイスのメンバーには、入れ子にされた型、メソッド、プロパティ、およびイベントのみを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="770bc-310">Only nested types, methods, properties, and events may be members of an interface.</span></span> <span data-ttu-id="770bc-311">メソッドとプロパティには、本文がありません。</span><span class="sxs-lookup"><span data-stu-id="770bc-311">Methods and properties may not have a body.</span></span> <span data-ttu-id="770bc-312">インターフェイスのメンバーは暗黙的に`Public`アクセス修飾子が指定されていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="770bc-312">Interface members are implicitly `Public` and may not specify an access modifier.</span></span> <span data-ttu-id="770bc-313">インターフェイスで宣言されたメンバーのスコープは、(ジェネリックし、制約があります) 場合は、宣言が行われているインターフェイスの本体とそのインターフェイスの制約リストです。</span><span class="sxs-lookup"><span data-stu-id="770bc-313">The scope of a member declared in an interface is the interface body in which the declaration occurs, plus the constraint list of that interface (if it is generic and has constraints).</span></span>


## <a name="arrays"></a><span data-ttu-id="770bc-314">配列</span><span class="sxs-lookup"><span data-stu-id="770bc-314">Arrays</span></span>

<span data-ttu-id="770bc-315">*配列*変数を使用してアクセスを含む参照型である*インデックス*配列内の変数の順序で一対一で対応します。</span><span class="sxs-lookup"><span data-stu-id="770bc-315">An *array* is a reference type that contains variables accessed through *indices* corresponding in a one-to-one fashion with the order of the variables in the array.</span></span> <span data-ttu-id="770bc-316">を配列に含まれる変数とも呼ばれます、*要素*、配列のすべて、同じ種類は、この型が呼び出される、*要素型*の配列。</span><span class="sxs-lookup"><span data-stu-id="770bc-316">The variables contained in an array, also called the *elements* of the array, must all be of the same type, and this type is called the *element type* of the array.</span></span>

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

<span data-ttu-id="770bc-317">配列の要素は、配列インスタンスの作成時に存在することになるし、配列のインスタンスが破棄されるときに存在しなくなります。</span><span class="sxs-lookup"><span data-stu-id="770bc-317">The elements of an array come into existence when an array instance is created, and cease to exist when the array instance is destroyed.</span></span> <span data-ttu-id="770bc-318">配列の各要素は、その型の既定値に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="770bc-318">Each element of an array is initialized to the default value of its type.</span></span> <span data-ttu-id="770bc-319">型`System.Array`のすべての配列型の基本型は、インスタンス化されないことがあります。</span><span class="sxs-lookup"><span data-stu-id="770bc-319">The type `System.Array` is the base type of all array types and may not be instantiated.</span></span> <span data-ttu-id="770bc-320">すべての配列型で宣言されたメンバーの継承、`System.Array`を入力し、それに変換可能です (と`Object`)。</span><span class="sxs-lookup"><span data-stu-id="770bc-320">Every array type inherits the members declared by the `System.Array` type and is convertible to it (and `Object`).</span></span> <span data-ttu-id="770bc-321">要素を持つ 1 次元配列型`T`もインターフェイスを実装`System.Collections.Generic.IList(Of T)`と`IReadOnlyList(Of T)`場合`T`が参照型では、配列の型も実装`IList(Of U)`と`IReadOnlyList(Of U)`任意の`U`拡大変換からの変換の参照を持つ`T`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-321">A one-dimensional array type with element `T` also implements the interfaces `System.Collections.Generic.IList(Of T)` and `IReadOnlyList(Of T)`; if `T` is a reference type, then the array type also implements `IList(Of U)` and `IReadOnlyList(Of U)` for any `U` that has a widening  reference conversion from `T`.</span></span>

<span data-ttu-id="770bc-322">配列が、*ランク*配列の各要素に関連付けられているインデックスの数を決定します。</span><span class="sxs-lookup"><span data-stu-id="770bc-322">An array has a *rank* that determines the number of indices associated with each array element.</span></span> <span data-ttu-id="770bc-323">配列のランクの数を決定する*ディメンション*の配列。</span><span class="sxs-lookup"><span data-stu-id="770bc-323">The rank of an array determines the number of *dimensions* of the array.</span></span> <span data-ttu-id="770bc-324">たとえばのいずれかのランクを持つ配列は、1 次元の配列と呼ばれ、1 より大きいランクを持つ配列は多次元配列と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="770bc-324">For example, an array with a rank of one is called a single-dimensional array, and an array with a rank greater than one is called a multidimensional array.</span></span>

<span data-ttu-id="770bc-325">次の例では、整数値の 1 次元配列を作成するには、配列の要素を初期化しますおよびからそれらの出力します。</span><span class="sxs-lookup"><span data-stu-id="770bc-325">The following example creates a single-dimensional array of integer values, initializes the array elements, and then prints each of them out:</span></span>

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

<span data-ttu-id="770bc-326">プログラムは、次を出力します。</span><span class="sxs-lookup"><span data-stu-id="770bc-326">The program outputs the following:</span></span>

```
arr(0) = 0
arr(1) = 1
arr(2) = 4
arr(3) = 9
arr(4) = 16
arr(5) = 25
```

<span data-ttu-id="770bc-327">配列の各次元が、関連付けられた長さ。</span><span class="sxs-lookup"><span data-stu-id="770bc-327">Each dimension of an array has an associated length.</span></span> <span data-ttu-id="770bc-328">次元の長さは、配列の型の一部ではありませんが、配列型のインスタンスが実行時に作成されたときに確立されます。</span><span class="sxs-lookup"><span data-stu-id="770bc-328">Dimension lengths are not part of the type of the array, but rather are established when an instance of the array type is created at run time.</span></span> <span data-ttu-id="770bc-329">次元の長さは、その次元のインデックスの有効な範囲を決定します。 長さのディメンションの`N`、インデックスの範囲は 0 ~ `N-1`。</span><span class="sxs-lookup"><span data-stu-id="770bc-329">The length of a dimension determines the valid range of indices for that dimension: for a dimension of length `N`, indices can range from zero to `N-1`.</span></span> <span data-ttu-id="770bc-330">ディメンションが長さ 0 の場合は、その次元のインデックスが有効なことはありません。</span><span class="sxs-lookup"><span data-stu-id="770bc-330">If a dimension is of length zero, there are no valid indices for that dimension.</span></span> <span data-ttu-id="770bc-331">配列内の要素の合計数は、配列の各次元の長さの製品です。</span><span class="sxs-lookup"><span data-stu-id="770bc-331">The total number of elements in an array is the product of the lengths of each dimension in the array.</span></span> <span data-ttu-id="770bc-332">長さ 0 の配列の次元のいずれかの場合は、配列は空にすることはできます。</span><span class="sxs-lookup"><span data-stu-id="770bc-332">If any of the dimensions of an array has a length of zero, the array is said to be empty.</span></span> <span data-ttu-id="770bc-333">配列の要素型には、任意の型を指定できます。</span><span class="sxs-lookup"><span data-stu-id="770bc-333">The element type of an array can be any type.</span></span>

<span data-ttu-id="770bc-334">配列型は、既存の型名を修飾子を追加することによって指定されます。</span><span class="sxs-lookup"><span data-stu-id="770bc-334">Array types are specified by adding a modifier to an existing type name.</span></span> <span data-ttu-id="770bc-335">修飾子は、一連のコンマの 0 個以上で、左かっこと右かっこで構成されます。</span><span class="sxs-lookup"><span data-stu-id="770bc-335">The modifier consists of a left parenthesis, a set of zero or more commas, and a right parenthesis.</span></span> <span data-ttu-id="770bc-336">変更の種類は、配列の要素の型と次元数は 1 を足したコンマの数。</span><span class="sxs-lookup"><span data-stu-id="770bc-336">The type modified is the element type of the array, and the number of dimensions is the number of commas plus one.</span></span> <span data-ttu-id="770bc-337">1 つ以上の修飾子が指定されている場合、配列の要素型は配列です。</span><span class="sxs-lookup"><span data-stu-id="770bc-337">If more than one modifier is specified, then the element type of the array is an array.</span></span> <span data-ttu-id="770bc-338">修飾子は最も外側の配列をされている一番左の修飾子で、左から右に読み取らします。</span><span class="sxs-lookup"><span data-stu-id="770bc-338">The modifiers are read left to right, with the leftmost modifier being the outermost array.</span></span> <span data-ttu-id="770bc-339">例</span><span class="sxs-lookup"><span data-stu-id="770bc-339">In the example</span></span>

```vb
Module Test
    Dim arr As Integer(,)(,,)()
End Module
```

<span data-ttu-id="770bc-340">要素型`arr`の 1 次元配列の 3 次元の配列の 2 次元の配列は、`Integer`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-340">the element type of `arr` is a two-dimensional array of three-dimensional arrays of one-dimensional arrays of `Integer`.</span></span>

<span data-ttu-id="770bc-341">変数名の配列型修飾子または、配列のサイズの初期化の修飾子を配置することで、配列型である変数を宣言することも。</span><span class="sxs-lookup"><span data-stu-id="770bc-341">A variable may also be declared to be of an array type by putting an array type modifier or an array-size initialization modifier on the variable name.</span></span> <span data-ttu-id="770bc-342">その場合は、配列要素の型は、宣言で指定された型と配列の次元は、変数名修飾子によって決まります。</span><span class="sxs-lookup"><span data-stu-id="770bc-342">In that case, the array element type is the type given in the declaration, and the array dimensions are determined by the variable name modifier.</span></span> <span data-ttu-id="770bc-343">わかりやすくするため、同じ宣言内で変数名と型名の両方の配列型修飾子があることはできません。</span><span class="sxs-lookup"><span data-stu-id="770bc-343">For clarity, it is not valid to have an array type modifier on both a variable name and a type name in the same declaration.</span></span>

<span data-ttu-id="770bc-344">次の例では、ローカル変数の宣言で配列の型を使用するさまざまな`Integer`要素の型として。</span><span class="sxs-lookup"><span data-stu-id="770bc-344">The following example shows a variety of local variable declarations that use array types with `Integer` as the element type:</span></span>

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

<span data-ttu-id="770bc-345">配列の型名修飾子は、それに続くかっこのすべてのセットを拡張します。</span><span class="sxs-lookup"><span data-stu-id="770bc-345">An array type name modifier extends to all sets of parentheses that follow it.</span></span> <span data-ttu-id="770bc-346">これはこと型名の後にかっこで囲まれた引数のセットを許可する場所の状況でない配列の型名の引数を指定することを意味します。</span><span class="sxs-lookup"><span data-stu-id="770bc-346">This means that in the situations where a set of arguments enclosed in parenthesis is allowed after a type name, it is not possible to specify the arguments for an array type name.</span></span> <span data-ttu-id="770bc-347">例:</span><span class="sxs-lookup"><span data-stu-id="770bc-347">For example:</span></span>

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

<span data-ttu-id="770bc-348">最後の場合、`(3)`コンス トラクターの引数のセットとしてではなく、型名の一部として解釈されます。</span><span class="sxs-lookup"><span data-stu-id="770bc-348">In the last case, `(3)` is interpreted as part of the type name rather than as a set of constructor arguments.</span></span>


## <a name="delegates"></a><span data-ttu-id="770bc-349">デリゲート</span><span class="sxs-lookup"><span data-stu-id="770bc-349">Delegates</span></span>

<span data-ttu-id="770bc-350">A*委任*参照型を参照するには、`Shared`メソッド、型またはオブジェクトのインスタンス メソッドにします。</span><span class="sxs-lookup"><span data-stu-id="770bc-350">A *delegate* is a reference type that refers to a `Shared` method of a type or to an instance method of an object.</span></span>

```antlr
DelegateDeclaration
    : Attributes? TypeModifier* 'Delegate' MethodSignature StatementTerminator
    ;

MethodSignature
    : SubSignature
    | FunctionSignature
    ;
```

 <span data-ttu-id="770bc-351">その他の言語では、デリゲートの最も近いは、関数ポインターに対してのみ、関数ポインターを参照できますが、`Shared`関数、デリゲートは、両方を参照できます`Shared`メソッドとインスタンスします。</span><span class="sxs-lookup"><span data-stu-id="770bc-351">The closest equivalent of a delegate in other languages is a function pointer, but whereas a function pointer can only reference `Shared` functions, a delegate can reference both `Shared` and instance methods.</span></span> <span data-ttu-id="770bc-352">後者の場合、デリゲートは、メソッドのエントリ ポイントへの参照だけでなく、メソッドの呼び出しに使用するオブジェクトのインスタンスへの参照を格納します。</span><span class="sxs-lookup"><span data-stu-id="770bc-352">In the latter case, the delegate stores not only a reference to the method's entry point, but also a reference to the object instance with which to invoke the method.</span></span>

<span data-ttu-id="770bc-353">デリゲートの宣言がない可能性があります、`Handles`句では、`Implements`句では、メソッドの本体で、または`End`を構築します。</span><span class="sxs-lookup"><span data-stu-id="770bc-353">The delegate declaration may not have  a `Handles` clause, an `Implements` clause, a method body, or an `End` construct.</span></span> <span data-ttu-id="770bc-354">デリゲート宣言のパラメーター リストがない場合があります`Optional`または`ParamArray`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="770bc-354">The parameter list of the delegate declaration may not contain `Optional` or `ParamArray` parameters.</span></span> <span data-ttu-id="770bc-355">戻り値の型とパラメーターの型のアクセシビリティ ドメインは、デリゲート自体のアクセシビリティ ドメインのスーパー セットであるかと同じである必要があります。</span><span class="sxs-lookup"><span data-stu-id="770bc-355">The accessibility domain of the return type and parameter types must be the same as or a superset of the accessibility domain of the delegate itself.</span></span>

<span data-ttu-id="770bc-356">デリゲートのメンバーは、クラスから継承されたメンバー`System.Delegate`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-356">The members of a delegate are the members inherited from class `System.Delegate`.</span></span> <span data-ttu-id="770bc-357">デリゲートは、次のメソッドも定義します。</span><span class="sxs-lookup"><span data-stu-id="770bc-357">A delegate also defines the following methods:</span></span>

* <span data-ttu-id="770bc-358">型の 1 つ、2 つのパラメーターを受け取るコンス トラクター`Object`型の 1 つ`System.IntPtr`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-358">A constructor that takes two parameters, one of type `Object` and one of type `System.IntPtr`.</span></span>

* <span data-ttu-id="770bc-359">`Invoke`デリゲートと同じシグネチャを持つメソッド。</span><span class="sxs-lookup"><span data-stu-id="770bc-359">An `Invoke` method that has the same signature as the delegate.</span></span>

* <span data-ttu-id="770bc-360">A`BeginInvoke`メソッド シグネチャがデリゲートのシグネチャを次の 3 つの点が異なります。</span><span class="sxs-lookup"><span data-stu-id="770bc-360">A `BeginInvoke` method whose signature is the delegate signature, with three differences.</span></span> <span data-ttu-id="770bc-361">戻り値の型を変更する最初に、`System.IAsyncResult`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-361">First, the return type is changed to `System.IAsyncResult`.</span></span> <span data-ttu-id="770bc-362">次に、2 つのパラメーターがパラメーター リストの末尾に追加されます: 型の最初の`System.AsyncCallback`と型の 2 番目の`Object`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-362">Second, two additional parameters are added to the end of the parameter list: the first of type `System.AsyncCallback` and the second of type `Object`.</span></span> <span data-ttu-id="770bc-363">最後に、すべて`ByRef`パラメーターがある変更`ByVal`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-363">And finally, all `ByRef` parameters are changed to be `ByVal`.</span></span>

* <span data-ttu-id="770bc-364">`EndInvoke`メソッドの戻り値の型は、デリゲートと同じです。</span><span class="sxs-lookup"><span data-stu-id="770bc-364">An `EndInvoke` method whose return type is the same as the delegate.</span></span> <span data-ttu-id="770bc-365">メソッドのパラメーターはデリゲート パラメーターを`ByRef`デリゲート シグネチャに出現する順序と同じ順序でのパラメーター。</span><span class="sxs-lookup"><span data-stu-id="770bc-365">The parameters of the method are only the delegate parameters exactly that are `ByRef` parameters, in the same order they occur in the delegate signature.</span></span>  <span data-ttu-id="770bc-366">型の追加のパラメーターがあるこれらのパラメーターだけでなく`System.IAsyncResult`パラメーター リストの末尾。</span><span class="sxs-lookup"><span data-stu-id="770bc-366">In addition to those parameters, there is an additional parameter of type `System.IAsyncResult` at the end of the parameter list.</span></span>

<span data-ttu-id="770bc-367">3 つのステップを定義すると、デリゲートの使用: 宣言、インスタンス化、および呼び出し。</span><span class="sxs-lookup"><span data-stu-id="770bc-367">There are three steps in defining and using delegates: declaration, instantiation, and invocation.</span></span>

<span data-ttu-id="770bc-368">デリゲートは、デリゲート宣言の構文を使用して宣言されます。</span><span class="sxs-lookup"><span data-stu-id="770bc-368">Delegates are declared using delegate declaration syntax.</span></span> <span data-ttu-id="770bc-369">次の例は、という名前のデリゲートを宣言`SimpleDelegate`する引数を受け取りません。</span><span class="sxs-lookup"><span data-stu-id="770bc-369">The following example declares a delegate named `SimpleDelegate` that takes no arguments:</span></span>

```vb
Delegate Sub SimpleDelegate()
```

<span data-ttu-id="770bc-370">次の例では、作成、`SimpleDelegate`インスタンスし、すぐに呼び出します。</span><span class="sxs-lookup"><span data-stu-id="770bc-370">The next example creates a `SimpleDelegate` instance and then immediately calls it:</span></span>

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

<span data-ttu-id="770bc-371">メソッドのデリゲートをインスタンス化すると、すぐに呼び出して、デリゲート経由で多くのポイントにメソッドを直接呼び出すやすくなること。</span><span class="sxs-lookup"><span data-stu-id="770bc-371">There is not much point in instantiating a delegate for a method and then immediately calling via the delegate, as it would be simpler to call the method directly.</span></span> <span data-ttu-id="770bc-372">デリゲートは、その匿名性を使用すると、その有用性を表示します。</span><span class="sxs-lookup"><span data-stu-id="770bc-372">Delegates show their usefulness when their anonymity is used.</span></span> <span data-ttu-id="770bc-373">例を次に示します、`MultiCall`を繰り返し呼び出すメソッドを`SimpleDelegate`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="770bc-373">The next example shows a `MultiCall` method that repeatedly calls a `SimpleDelegate` instance:</span></span>

```vb
Sub MultiCall(d As SimpleDelegate, count As Integer)
    Dim i As Integer

    For i = 0 To count - 1
        d()
    Next i
End Sub
```

<span data-ttu-id="770bc-374">重要ではない、`MultiCall`メソッド ターゲット メソッドの`SimpleDelegate`、どのようなユーザー補助機能は、このメソッドは、またはメソッドは、かどうか`Shared`かどうか。</span><span class="sxs-lookup"><span data-stu-id="770bc-374">It is unimportant to the `MultiCall` method what the target method for the `SimpleDelegate` is, what accessibility this method has, or whether the method is `Shared` or not.</span></span> <span data-ttu-id="770bc-375">ターゲット メソッドのシグネチャと互換性がある、重要な`SimpleDelegate`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-375">All that matters is that the signature of the target method is compatible with `SimpleDelegate`.</span></span>


## <a name="partial-types"></a><span data-ttu-id="770bc-376">部分型</span><span class="sxs-lookup"><span data-stu-id="770bc-376">Partial types</span></span>

<span data-ttu-id="770bc-377">クラスと構造体の宣言ができる*部分*宣言します。</span><span class="sxs-lookup"><span data-stu-id="770bc-377">Class and structure declarations can be *partial* declarations.</span></span> <span data-ttu-id="770bc-378">部分宣言は、宣言内で宣言された型が完全に記述しない場合があります。</span><span class="sxs-lookup"><span data-stu-id="770bc-378">A partial declaration may or may not fully describe the declared type within the declaration.</span></span> <span data-ttu-id="770bc-379">代わりに、型の宣言は、プログラム内で複数の部分宣言間で分散可能性があります。プログラムの境界を越えて、部分型を宣言することはできません。</span><span class="sxs-lookup"><span data-stu-id="770bc-379">Instead, the declaration of the type may be spread across multiple partial declarations within the program; partial types cannot be declared across program boundaries.</span></span> <span data-ttu-id="770bc-380">部分型の宣言を指定します、`Partial`修飾子を宣言します。</span><span class="sxs-lookup"><span data-stu-id="770bc-380">A partial type declaration specifies the `Partial` modifier on the declaration.</span></span> <span data-ttu-id="770bc-381">次に、同じ完全修飾名を持つ型のプログラムでその他の宣言は、1 つの型宣言を形成するコンパイル時に、部分的な宣言と共に結合されます。</span><span class="sxs-lookup"><span data-stu-id="770bc-381">Then, any other declarations in the program for a type with the same fully-qualified name will be merged together with the partial declaration at compile-time to form a single type declaration.</span></span> <span data-ttu-id="770bc-382">たとえば、次のコードは 1 つのクラスを宣言します。`Test`メンバーと`Test.C1`と`Test.C2`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-382">For example, the following code declares a single class `Test` with members `Test.C1` and `Test.C2`.</span></span>

<span data-ttu-id="770bc-383">a.vb:</span><span class="sxs-lookup"><span data-stu-id="770bc-383">a.vb:</span></span>

```vb
Public Partial Class Test
    Public Sub S1()
    End Sub
End Class
```

<span data-ttu-id="770bc-384">b.vb:</span><span class="sxs-lookup"><span data-stu-id="770bc-384">b.vb:</span></span>

```vb
Public Class Test
    Public Sub S2()
    End Sub
End Class
```

<span data-ttu-id="770bc-385">部分型の宣言を結合するときに、少なくとも 1 つの宣言では、必要があります、`Partial`修飾子は、それ以外の場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="770bc-385">When combining partial type declarations, at least one of the declarations must have a `Partial` modifier, otherwise a compile-time error results.</span></span>

<span data-ttu-id="770bc-386">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="770bc-386">__Note.__</span></span> <span data-ttu-id="770bc-387">指定することはできますが`Partial`、多くの部分宣言間で 1 つだけ宣言はすべての部分宣言で指定するフォームが向上します。</span><span class="sxs-lookup"><span data-stu-id="770bc-387">Although it is possible to specify `Partial` on only one declaration among many partial declarations, it is better form to specify it on all partial declarations.</span></span> <span data-ttu-id="770bc-388">1 つの部分宣言が表示されるが 1 つまたは複数の部分宣言は非表示 (ツールによって生成されたコードを拡張する場合) などの状況ではのままに許容される、`Partial`可視の宣言から修飾子がで指定します非表示の宣言。</span><span class="sxs-lookup"><span data-stu-id="770bc-388">In the situation where one partial declaration is visible but one or more partial declarations are hidden (such as the case of extending tool-generated code), it is acceptable to leave the `Partial` modifier off of the visible declaration but specify it on the hidden declarations.</span></span>

<span data-ttu-id="770bc-389">部分的な宣言を使用して、クラスと構造体のみを宣言できます。</span><span class="sxs-lookup"><span data-stu-id="770bc-389">Only classes and structures can be declared using partial declarations.</span></span> <span data-ttu-id="770bc-390">型のアリティは部分的な宣言を一緒に一致すると見なされます。 名前が同じで型パラメーターの数が異なる 2 つのクラスは、同時の partial 宣言には考慮されません。</span><span class="sxs-lookup"><span data-stu-id="770bc-390">The arity of a type is considered when matching partial declarations together: two classes with the same name but different numbers of type parameters are not considered to be partial declarations of the same time.</span></span> <span data-ttu-id="770bc-391">部分的な宣言が属性を指定する、クラスの修飾子、`Inherits`ステートメントまたは`Implements`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="770bc-391">Partial declarations can specify attributes, class modifiers, `Inherits` statement or `Implements` statement.</span></span> <span data-ttu-id="770bc-392">コンパイル時にのすべての部分宣言の結合させるとは、型宣言の一部として使用あり。</span><span class="sxs-lookup"><span data-stu-id="770bc-392">At compile time, all of the pieces of the partial declarations are combined together and used as a part of the type declaration.</span></span> <span data-ttu-id="770bc-393">修飾子を属性間の競合がある場合ベース、インターフェイス、または型のメンバー、コンパイル時エラーの結果。</span><span class="sxs-lookup"><span data-stu-id="770bc-393">If there are any conflicts between attributes, modifiers, bases, interfaces, or type members, a compile-time error results.</span></span> <span data-ttu-id="770bc-394">例:</span><span class="sxs-lookup"><span data-stu-id="770bc-394">For example:</span></span>

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

<span data-ttu-id="770bc-395">前の例では、型を宣言する`Test1`つまり`Public`から継承`Object`実装と`System.IDisposable`と`System.IComparable`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-395">The previous example declares a type `Test1` that is `Public`, inherits from `Object` and implements `System.IDisposable` and `System.IComparable`.</span></span> <span data-ttu-id="770bc-396">部分宣言`Test2`宣言では、いずれかのことを示すために、コンパイル時エラーが発生`Test2`は`Public`ことを示す別および`Test2`は`Private`。</span><span class="sxs-lookup"><span data-stu-id="770bc-396">The partial declarations of `Test2` will cause a compile-time error because one of the declarations says that `Test2` is `Public` and another says that `Test2` is `Private`.</span></span>

<span data-ttu-id="770bc-397">制約と分散型のパラメーターの型パラメーターを持つ部分の型を宣言できますが、制約と各部分宣言からの差異が一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="770bc-397">Partial types with type parameters can declare constraints and variance for the type parameters, but the constraints and variance from each partial declaration must match.</span></span> <span data-ttu-id="770bc-398">そのため、制約との差異でその他の修飾子のように自動的に結合されません。</span><span class="sxs-lookup"><span data-stu-id="770bc-398">Thus, constraints and variance are special in that they are not automatically combined like other modifiers:</span></span>

```vb
Partial Public Class List(Of T As IEnumerable)
End Class

' Error: Constraints on T don't match
Class List(Of T As IComparable)
End Class
```

<span data-ttu-id="770bc-399">複数の部分宣言を使用して、型が宣言されているという事実は、型の中で名前のルックアップ規則には影響しません。</span><span class="sxs-lookup"><span data-stu-id="770bc-399">The fact that a type is declared using multiple partial declarations does not affect the name lookup rules within the type.</span></span> <span data-ttu-id="770bc-400">その結果、部分型の宣言は、他の部分型の宣言で宣言されたメンバーを使用できます。 またはその他の部分型の宣言で宣言されたインターフェイスにメソッドを実装することがあります。</span><span class="sxs-lookup"><span data-stu-id="770bc-400">As a result, a partial type declaration can use members declared in other partial type declarations, or may implement methods on interfaces declared in other partial type declarations.</span></span> <span data-ttu-id="770bc-401">例:</span><span class="sxs-lookup"><span data-stu-id="770bc-401">For example:</span></span>

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

<span data-ttu-id="770bc-402">入れ子にされた型には、同様の部分宣言を持つことができます。</span><span class="sxs-lookup"><span data-stu-id="770bc-402">Nested types can have partial declarations as well.</span></span> <span data-ttu-id="770bc-403">例:</span><span class="sxs-lookup"><span data-stu-id="770bc-403">For example:</span></span>

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

<span data-ttu-id="770bc-404">部分的な宣言内で初期化子が宣言の順序で実行されます。ただし、別の部分宣言で発生する初期化子の実行の順序は保証はありません。</span><span class="sxs-lookup"><span data-stu-id="770bc-404">Initializers within a partial declaration will still be executed in declaration order; however, there is no guaranteed order of execution for initializers that occur in separate partial declarations.</span></span>

## <a name="constructed-types"></a><span data-ttu-id="770bc-405">構築された型</span><span class="sxs-lookup"><span data-stu-id="770bc-405">Constructed Types</span></span>

<span data-ttu-id="770bc-406">ジェネリック型の宣言では、単独では型を表していません。</span><span class="sxs-lookup"><span data-stu-id="770bc-406">A generic type declaration, by itself, does not denote a type.</span></span> <span data-ttu-id="770bc-407">代わりに、型引数を適用することでさまざまな種類のフォームにするには、「ブルー プリント」としてジェネリック型の宣言を使用します。</span><span class="sxs-lookup"><span data-stu-id="770bc-407">Instead, a generic type declaration can be used as a "blueprint" to form many different types by applying type arguments.</span></span> <span data-ttu-id="770bc-408">適用された型引数を持つジェネリック型が呼び出される、*構築型*します。</span><span class="sxs-lookup"><span data-stu-id="770bc-408">A generic type that has type arguments applied to it is called a *constructed type*.</span></span> <span data-ttu-id="770bc-409">構築された型の型引数に一致する型パラメーターに対する制約を常に満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="770bc-409">The type arguments in a constructed type must always satisfy the constraints placed on the type parameters they match to.</span></span>

<span data-ttu-id="770bc-410">型名によって型パラメーターを直接指定しない場合でも、構築された型がわかります。</span><span class="sxs-lookup"><span data-stu-id="770bc-410">A type name might identify a constructed type even though it doesn't specify type parameters directly.</span></span> <span data-ttu-id="770bc-411">これは、型がジェネリック クラス宣言の中で入れ子になった、外側の宣言のインスタンスの型が暗黙的に名前参照の使用に発生することができます。</span><span class="sxs-lookup"><span data-stu-id="770bc-411">This can occur where a type is nested within a generic class declaration, and the instance type of the containing declaration is implicitly used for name lookup:</span></span>

```vb
Class Outer(Of T) 
    Public Class Inner 
    End Class

    ' Type of i is the constructed type Outer(Of T).Inner
    Public i As Inner 
End Class
```

<span data-ttu-id="770bc-412">構築された型`C(Of T1,...,Tn)`はジェネリック型とすべての型引数にアクセスするときにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="770bc-412">A constructed type `C(Of T1,...,Tn)` is accessible when the generic type and all the type arguments are accessible.</span></span> <span data-ttu-id="770bc-413">たとえば場合は、ジェネリック型`C`は`Public`とすべての型引数`T1,...,Tn`は`Public`、構築された型は`Public`。</span><span class="sxs-lookup"><span data-stu-id="770bc-413">For instance, if the generic type `C` is `Public` and all of the type arguments `T1,...,Tn` are `Public`, then the constructed type is `Public`.</span></span> <span data-ttu-id="770bc-414">型名または型引数のいずれかのいずれかが場合`Private`、ただし、構築された型のアクセシビリティは`Private`します。</span><span class="sxs-lookup"><span data-stu-id="770bc-414">If either the type name or one of the type arguments is `Private`, however, then the accessibility of the constructed type is `Private`.</span></span> <span data-ttu-id="770bc-415">構築された型の 1 つの型引数がある場合`Protected`別の型引数は`Friend`、構築された型は、クラスとで与えられている任意のアセンブリのアセンブリ、またはそのサブクラス内だけでアクセスし、`Friend`アクセスします。</span><span class="sxs-lookup"><span data-stu-id="770bc-415">If one type argument of the constructed type is `Protected` and another type argument is `Friend`, then the constructed type is accessible only in the class and its subclasses in this assembly or any assembly that has been given `Friend` access.</span></span> <span data-ttu-id="770bc-416">つまり、構築された型のアクセシビリティ ドメインは、その構成要素のアクセシビリティ ドメインの交差部分。</span><span class="sxs-lookup"><span data-stu-id="770bc-416">In other words, the accessibility domain for a constructed type is the intersection of the accessibility domains of its constituent parts.</span></span>

<span data-ttu-id="770bc-417">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="770bc-417">__Note.__</span></span> <span data-ttu-id="770bc-418">構築された型のアクセシビリティ ドメインが構成された部分の積集合であるという事実では、新しいアクセシビリティ レベルを定義する興味深い副作用があります。</span><span class="sxs-lookup"><span data-stu-id="770bc-418">The fact that the accessibility domain of constructed type is the intersection of its constituted parts has the interesting side effect of defining a new accessibility level.</span></span> <span data-ttu-id="770bc-419">構築された型にある要素を含む`Protected`と要素が`Friend`にアクセスできるコンテキストでのみアクセスできる*両方* `Friend` *と* `Protected`メンバー。</span><span class="sxs-lookup"><span data-stu-id="770bc-419">A constructed type that contains an element that is `Protected` and an element that is `Friend` can only be accessed in contexts that can access *both* `Friend` *and* `Protected` members.</span></span> <span data-ttu-id="770bc-420">ただし、ユーザー補助機能と、言語では、このアクセシビリティ レベルを表現する方法はありません`Protected Friend`エンティティにアクセスできるコンテキストでアクセスできることを意味*か* `Friend` *または*`Protected`メンバー。</span><span class="sxs-lookup"><span data-stu-id="770bc-420">However, there is no way to express this accessibility level in the language, as the accessibility `Protected Friend` means that an entity can be accessed in a context that can access *either* `Friend` *or* `Protected` members.</span></span>

<span data-ttu-id="770bc-421">基本して実装されるインターフェイスと構築された型のメンバーは、ジェネリック型の型パラメーターのうち、指定された型引数を代入することによって決定されます。</span><span class="sxs-lookup"><span data-stu-id="770bc-421">The base, implemented interfaces and members of constructed types are determined by substituting the supplied type arguments for each occurrence of the type parameter in the generic type.</span></span>

### <a name="open-types-and-closed-types"></a><span data-ttu-id="770bc-422">オープン型とクローズ型</span><span class="sxs-lookup"><span data-stu-id="770bc-422">Open Types and Closed Types</span></span>

<span data-ttu-id="770bc-423">構築された型を含んでいる型またはメソッドの型パラメーターが 1 つまたは複数の型引数にはユーザーと呼ばれる、*オープン型*します。</span><span class="sxs-lookup"><span data-stu-id="770bc-423">A constructed type for who one or more type arguments are type parameters of a containing type or method is called an *open type*.</span></span> <span data-ttu-id="770bc-424">これは、します型の実際の図形が完全に認識していませんので、不明なので、型の型パラメーターの一部は引き続き。</span><span class="sxs-lookup"><span data-stu-id="770bc-424">This is because some of the type parameters of the type are still not known, so the actual shape of the type is not yet fully known.</span></span> <span data-ttu-id="770bc-425">これに対し、型引数はすべての非型パラメーター、ジェネリック型と呼ばれる、*クローズ型*します。</span><span class="sxs-lookup"><span data-stu-id="770bc-425">In contrast, a generic type whose type arguments are all non-type parameters is called a *closed type*.</span></span> <span data-ttu-id="770bc-426">クローズ型の図形は常に完全に呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="770bc-426">The shape of a closed type is always fully known.</span></span> <span data-ttu-id="770bc-427">例:</span><span class="sxs-lookup"><span data-stu-id="770bc-427">For example:</span></span>

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

<span data-ttu-id="770bc-428">構築された型`Base(Of Integer, V)`オープン型は、ためが型パラメーター`T`指定されています型パラメーター`U`が別の型パラメーターを指定します。</span><span class="sxs-lookup"><span data-stu-id="770bc-428">The constructed type `Base(Of Integer, V)` is an open type because although the type parameter `T` has been supplied, the type parameter `U` has been supplied another type parameter.</span></span> <span data-ttu-id="770bc-429">そのため、型の完全な図形がまだ不明です。</span><span class="sxs-lookup"><span data-stu-id="770bc-429">Thus, the full shape of the type is not yet known.</span></span> <span data-ttu-id="770bc-430">構築された型`Derived(Of Double)`がクローズ型を継承階層内のすべての型パラメーターが指定されているためです。</span><span class="sxs-lookup"><span data-stu-id="770bc-430">The constructed type `Derived(Of Double)`, however, is a closed type because all type parameters in the inheritance hierarchy have been supplied.</span></span>

<span data-ttu-id="770bc-431">オープン型の定義は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="770bc-431">Open types are defined as follows:</span></span>

* <span data-ttu-id="770bc-432">型パラメーターは、オープン型です。</span><span class="sxs-lookup"><span data-stu-id="770bc-432">A type parameter is an open type.</span></span>

* <span data-ttu-id="770bc-433">要素の型がオープン型の場合、配列型はオープン型が。</span><span class="sxs-lookup"><span data-stu-id="770bc-433">An array type is an open type if its element type is an open type.</span></span>

* <span data-ttu-id="770bc-434">構築された型がオープン型場合、1 つまたは複数の型引数、オープン型。</span><span class="sxs-lookup"><span data-stu-id="770bc-434">A constructed type is an open type if one or more of its type arguments are an open type.</span></span>

* <span data-ttu-id="770bc-435">閉じている型は、オープン型ではない型です。</span><span class="sxs-lookup"><span data-stu-id="770bc-435">A closed type is a type that is not an open type.</span></span>

<span data-ttu-id="770bc-436">プログラムのエントリ ポイントは、ジェネリック型にすることはできません、ため、実行時に使用されるすべての型はクローズ型になります。</span><span class="sxs-lookup"><span data-stu-id="770bc-436">Because the program entry point cannot be in a generic type, all types used at run-time will be closed types.</span></span>

## <a name="special-types"></a><span data-ttu-id="770bc-437">特殊な種類</span><span class="sxs-lookup"><span data-stu-id="770bc-437">Special Types</span></span>

<span data-ttu-id="770bc-438">.NET Framework には、多数 Visual Basic 言語と .NET Framework によって特別に扱われるクラスにはが含まれています。</span><span class="sxs-lookup"><span data-stu-id="770bc-438">The .NET Framework contains a number of classes that are treated specially by the .NET Framework and by the Visual Basic language:</span></span>

<span data-ttu-id="770bc-439">型`System.Void`、.NET framework では、void 型を表す直接参照できるだけで`GetType`式。</span><span class="sxs-lookup"><span data-stu-id="770bc-439">The type `System.Void`, which represents a void type in the .NET Framework, can be directly referenced only in `GetType` expressions.</span></span>

<span data-ttu-id="770bc-440">種類`System.RuntimeArgumentHandle`、`System.ArgIterator`と`System.TypedReference`すべてスタックへのポインターを含めることができ、.NET Framework ヒープ上に表示できないようにします。</span><span class="sxs-lookup"><span data-stu-id="770bc-440">The types `System.RuntimeArgumentHandle`, `System.ArgIterator` and `System.TypedReference` all can contain pointers into the stack and so cannot appear on the .NET Framework heap.</span></span> <span data-ttu-id="770bc-441">そのため、配列の要素型、戻り値の型、フィールドの型、ジェネリック型引数、null 許容型として使用することはできません`ByRef`パラメーターの型に変換される値の型`Object`または`System.ValueType`インスタンスへの呼び出しのターゲットメンバーの`Object`または`System.ValueType`クロージャにリフトされたか。</span><span class="sxs-lookup"><span data-stu-id="770bc-441">Therefore, they cannot be used as array element types, return types, field types, generic type arguments, nullable types, `ByRef` parameter types, the type of a value being converted to `Object` or `System.ValueType`, the target of a call to instance members of `Object` or `System.ValueType`, or lifted into a closure.</span></span>
