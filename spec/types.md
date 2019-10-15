---
ms.openlocfilehash: 4560eac9f4ab52d07c77724aeca696d0195da91a
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306021"
---
# <a name="types"></a><span data-ttu-id="3b12e-101">種類</span><span class="sxs-lookup"><span data-stu-id="3b12e-101">Types</span></span>

<span data-ttu-id="3b12e-102">Visual Basic の型の2つの基本カテゴリは、*値型*と*参照型*です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-102">The two fundamental categories of types in Visual Basic are *value types* and *reference types*.</span></span> <span data-ttu-id="3b12e-103">プリミティブ型 (文字列を除く)、列挙型、および構造体は、値型です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-103">Primitive types (except strings), enumerations, and structures are value types.</span></span> <span data-ttu-id="3b12e-104">クラス、文字列、標準モジュール、インターフェイス、配列、およびデリゲートは参照型です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-104">Classes, strings, standard modules, interfaces, arrays, and delegates are reference types.</span></span>

<span data-ttu-id="3b12e-105">すべての型には*既定値*があり、これは初期化時にその型の変数に割り当てられる値です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-105">Every type has a *default value*, which is the value that is assigned to variables of that type upon initialization.</span></span>

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

## <a name="value-types-and-reference-types"></a><span data-ttu-id="3b12e-106">値型と参照型</span><span class="sxs-lookup"><span data-stu-id="3b12e-106">Value Types and Reference Types</span></span>

<span data-ttu-id="3b12e-107">値型と参照型は、宣言の構文と使用方法とでは似ていますが、セマンティクスは異なります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-107">Although value types and reference types can be similar in terms of declaration syntax and usage, their semantics are distinct.</span></span>

<span data-ttu-id="3b12e-108">参照型はランタイムヒープに格納されます。そのストレージへの参照によってのみアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-108">Reference types are stored on the run-time heap; they may only be accessed through a reference to that storage.</span></span> <span data-ttu-id="3b12e-109">参照型は常に参照によってアクセスされるため、その有効期間は .NET Framework によって管理されます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-109">Because reference types are always accessed through references, their lifetime is managed by the .NET Framework.</span></span> <span data-ttu-id="3b12e-110">特定のインスタンスへの未処理の参照は追跡され、その他の参照が残っていない場合にのみインスタンスが破棄されます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-110">Outstanding references to a particular instance are tracked and the instance is destroyed only when no more references remain.</span></span> <span data-ttu-id="3b12e-111">参照型の変数には、その型の値、より多くの派生型の値、または null 値への参照が含まれています。</span><span class="sxs-lookup"><span data-stu-id="3b12e-111">A variable of reference type contains a reference to a value of that type, a value of a more derived type, or a null value.</span></span> <span data-ttu-id="3b12e-112">*Null 値*は nothing を参照します。割り当てを除き、null 値を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-112">A *null value* refers to nothing; it is not possible to do anything with a null value except assign it.</span></span> <span data-ttu-id="3b12e-113">参照型の変数への代入では、参照される値のコピーではなく、参照のコピーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-113">Assignment to a variable of a reference type creates a copy of the reference rather than a copy of the referenced value.</span></span> <span data-ttu-id="3b12e-114">参照型の変数の場合、既定値は null 値になります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-114">For a variable of a reference type, the default value is a null value.</span></span>

<span data-ttu-id="3b12e-115">値型は、配列内または別の型のいずれかのスタックに直接格納されます。ストレージには直接アクセスできます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-115">Value types are stored directly on the stack, either within an array or within another type; their storage can only be accessed directly.</span></span> <span data-ttu-id="3b12e-116">値型は変数に直接格納されるので、有効期間はそれらを含む変数の有効期間によって決まります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-116">Because value types are stored directly within variables, their lifetime is determined by the lifetime of the variable that contains them.</span></span> <span data-ttu-id="3b12e-117">値型のインスタンスを含む場所が破棄されると、値型のインスタンスも破棄されます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-117">When the location containing a value type instance is destroyed, the value type instance is also destroyed.</span></span> <span data-ttu-id="3b12e-118">値型には常に直接アクセスします。値型への参照を作成することはできません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-118">Value types are always accessed directly; it is not possible to create a reference to a value type.</span></span> <span data-ttu-id="3b12e-119">このような参照を禁止すると、破棄された値クラスのインスタンスを参照できなくなります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-119">Prohibiting such a reference makes it impossible to refer to a value class instance that has been destroyed.</span></span> <span data-ttu-id="3b12e-120">値型は常に 0 @no__t であるため、値型の変数には常にその型の値が含まれます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-120">Because value types are always `NotInheritable`, a variable of a value type always contains a value of that type.</span></span> <span data-ttu-id="3b12e-121">このため、値型の値を null 値にすることはできません。また、より多くの派生型のオブジェクトを参照することもできません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-121">Because of this, the value of a value type cannot be a null value, nor can it reference an object of a more derived type.</span></span> <span data-ttu-id="3b12e-122">値型の変数への代入では、割り当てられている値のコピーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-122">Assignment to a variable of a value type creates a copy of the value being assigned.</span></span> <span data-ttu-id="3b12e-123">値型の変数の既定値は、その型の各変数メンバーを既定値に初期化した結果です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-123">For a variable of a value type, the default value is the result of initializing each variable member of the type to its default value.</span></span>

<span data-ttu-id="3b12e-124">次の例は、参照型と値型の違いを示しています。</span><span class="sxs-lookup"><span data-stu-id="3b12e-124">The following example shows the difference between reference types and value types:</span></span>

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

<span data-ttu-id="3b12e-125">プログラムの出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-125">The output of the program is:</span></span>

```console
Values: 0, 123
Refs: 123, 123
```

<span data-ttu-id="3b12e-126">ローカル変数 `val2` への代入はローカル @no__t 変数に影響しません。これは、両方のローカル変数が値型 (型 `Integer`) であり、値型の各ローカル変数が独自のストレージを持つためです。</span><span class="sxs-lookup"><span data-stu-id="3b12e-126">The assignment to the local variable `val2` does not impact the local variable `val1` because both local variables are of a value type (the type `Integer`) and each local variable of a value type has its own storage.</span></span> <span data-ttu-id="3b12e-127">これに対し、`ref2.Value = 123;` の割り当ては、`ref1` と `ref2` の両方の参照を持つオブジェクトに影響します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-127">In contrast, the assignment `ref2.Value = 123;` affects the object that both `ref1` and `ref2` reference.</span></span>

<span data-ttu-id="3b12e-128">.NET Framework 型システムに関する注意点の1つは、構造体、列挙型、およびプリミティブ型 (`String` を除く) が値型であっても、すべてが参照型から継承していることです。</span><span class="sxs-lookup"><span data-stu-id="3b12e-128">One thing to note about the .NET Framework type system is that even though structures, enumerations and primitive types (except for `String`) are value types, they all inherit from reference types.</span></span> <span data-ttu-id="3b12e-129">構造体とプリミティブ型は、参照型から継承される `System.ValueType` で `Object` から継承されます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-129">Structures and the primitive types inherit from the reference type `System.ValueType`, which inherits from `Object`.</span></span> <span data-ttu-id="3b12e-130">列挙型は、参照型を継承します。この型は、`System.ValueType` から継承される `System.Enum` です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-130">Enumerated types inherit from the reference type `System.Enum`, which inherits from `System.ValueType`.</span></span>

### <a name="nullable-value-types"></a><span data-ttu-id="3b12e-131">null 許容値型</span><span class="sxs-lookup"><span data-stu-id="3b12e-131">Nullable Value Types</span></span>

<span data-ttu-id="3b12e-132">値型の場合は、型名に @no__t 0 修飾子を追加して、その型の*null 許容*バージョンを表すことができます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-132">For value types, a `?` modifier can be added to a type name to represent the *nullable* version of that type.</span></span>

```antlr
NullableTypeName
    : NonArrayTypeName '?'
    ;

NullableNameModifier
    : '?'
    ;
```

<span data-ttu-id="3b12e-133">Null 許容値型には、null 非許容の型と null 値の両方の値を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-133">A nullable value type can contain the same values as the non-nullable version of the type as well as the null value.</span></span> <span data-ttu-id="3b12e-134">したがって、null 許容値型の場合、型の変数に `Nothing` を割り当てると、値の型の0ではなく、変数の値が null 値に設定されます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-134">Thus, for a nullable value type, assigning `Nothing` to a variable of the type sets the value of the variable to the null value, not the zero value of the value type.</span></span> <span data-ttu-id="3b12e-135">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-135">For example:</span></span>

```vb
Dim x As Integer = Nothing
Dim y As Integer? = Nothing

' Prints zero
Console.WriteLine(x)
' Prints nothing (because the value of y is the null value)
Console.WriteLine(y)
```

<span data-ttu-id="3b12e-136">変数名に null 許容型修飾子を指定することで、変数を null 許容値型として宣言することもできます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-136">A variable may also be declared to be of a nullable value type by putting a nullable type modifier on the variable name.</span></span> <span data-ttu-id="3b12e-137">わかりやすくするために、同じ宣言内の変数名と型名の両方に null 許容型修飾子を指定することは無効です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-137">For clarity, it is not valid to have a nullable type modifier on both a variable name and a type name in the same declaration.</span></span> <span data-ttu-id="3b12e-138">Null 許容型は、型 `System.Nullable(Of T)` を使用して実装されているため、型 `T?` は `System.Nullable(Of T)` 型と同義であり、2つの名前を同義に使用できます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-138">Since nullable types are implemented using the type `System.Nullable(Of T)`, the type `T?` is synonymous to the type `System.Nullable(Of T)`, and the two names can be used interchangeably.</span></span> <span data-ttu-id="3b12e-139">@No__t-0 修飾子は、既に null 値が許容されている型には配置できません。したがって、型 `Integer??` または `System.Nullable(Of Integer)?` で宣言することはできません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-139">The `?` modifier cannot be placed on a type that is already nullable; thus, it is not possible to declare the type `Integer??` or `System.Nullable(Of Integer)?`.</span></span>

<span data-ttu-id="3b12e-140">Null 許容値型 `T?` には、@no__t のメンバーと、基になる型から `T` の @no__t 型に*リフト*されたすべての演算子または変換が含まれます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-140">A nullable value type `T?` has the members of `System.Nullable(Of T)` as well as any operators or conversions *lifted* from the underlying type `T` into the type `T?`.</span></span> <span data-ttu-id="3b12e-141">リフトは、基になる型から演算子と変換をコピーします。ほとんどの場合、null 非許容の値型に対して null 許容値型を代入します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-141">Lifting copies operators and conversions from the underlying type, in most cases substituting nullable value types for non-nullable value types.</span></span> <span data-ttu-id="3b12e-142">これにより、`T` に適用される同じ変換と操作の多くが `T?` にも適用されます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-142">This allows many of the same conversions and operations that apply to `T` to apply to `T?` as well.</span></span>


## <a name="interface-implementation"></a><span data-ttu-id="3b12e-143">インターフェイスの実装</span><span class="sxs-lookup"><span data-stu-id="3b12e-143">Interface Implementation</span></span>

<span data-ttu-id="3b12e-144">構造体とクラス宣言は、1つ以上の `Implements` 句を使用してインターフェイス型のセットを実装することを宣言できます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-144">Structure and class declarations may declare that they implement a set of interface types through one or more `Implements` clauses.</span></span>

```antlr
TypeImplementsClause
    : 'Implements' TypeImplements StatementTerminator
    ;

TypeImplements
    : NonArrayTypeName ( Comma NonArrayTypeName )*
    ;
```

<span data-ttu-id="3b12e-145">@No__t-0 句で指定するすべての型はインターフェイスである必要があり、型はインターフェイスのすべてのメンバーを実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-145">All the types specified in the `Implements` clause must be interfaces, and the type must implement all members of the interfaces.</span></span> <span data-ttu-id="3b12e-146">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-146">For example:</span></span>

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

<span data-ttu-id="3b12e-147">インターフェイスを実装する型も、すべてのインターフェイスの基本インターフェイスを暗黙的に実装します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-147">A type that implements an interface also implicitly implements all of the interface's base interfaces.</span></span> <span data-ttu-id="3b12e-148">これは、型によって、`Implements` 句のすべての基本インターフェイスが明示的に一覧表示されない場合でも同様です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-148">This is true even if the type does not explicitly list all base interfaces in the `Implements` clause.</span></span> <span data-ttu-id="3b12e-149">この例では、@no__t 0 の構造体が `IControl` と `ITextBox` の両方を実装しています。</span><span class="sxs-lookup"><span data-stu-id="3b12e-149">In this example, the `TextBox` structure implements both `IControl` and `ITextBox`.</span></span>

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

<span data-ttu-id="3b12e-150">型がのインターフェイスを実装する宣言では、型の宣言空間では何も宣言されません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-150">Declaring that a type implements an interface in and of itself does not declare anything in the declaration space of the type.</span></span> <span data-ttu-id="3b12e-151">このため、同じ名前のメソッドを使用して2つのインターフェイスを実装することは有効です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-151">Thus, it is valid to implement two interfaces with a method by the same name.</span></span>

<span data-ttu-id="3b12e-152">型は、型パラメーターを独自に実装することはできませんが、スコープ内の型パラメーターが含まれる場合があります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-152">Types cannot implement a type parameter on its own, although it may involve the type parameters that are in scope.</span></span>

```vb
Class C1(Of V)
    Implements V  ' Error, can't implement type parameter directly
    Implements IEnumerable(Of V)  ' OK, not directly implementing

    ...
End Class
```

<span data-ttu-id="3b12e-153">ジェネリックインターフェイスは、異なる型引数を使用して複数回実装できます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-153">Generic interfaces can be implemented multiple times using different type arguments.</span></span> <span data-ttu-id="3b12e-154">ただし、ジェネリック型は、指定された型パラメーター (型制約に関係なく) がそのインターフェイスの別の実装と重複する可能性がある場合に、型パラメーターを使用してジェネリックインターフェイスを実装することはできません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-154">However, a generic type cannot implement a generic interface using a type parameter if the supplied type parameter (regardless of type constraints) could overlap with another implementation of that interface.</span></span> <span data-ttu-id="3b12e-155">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-155">For example:</span></span>

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


## <a name="primitive-types"></a><span data-ttu-id="3b12e-156">プリミティブ型</span><span class="sxs-lookup"><span data-stu-id="3b12e-156">Primitive Types</span></span>

<span data-ttu-id="3b12e-157">*プリミティブ型*は、`System` 名前空間の定義済みの型のエイリアスであるキーワードを使用して識別されます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-157">The *primitive types* are identified through keywords, which are aliases for predefined types in the `System` namespace.</span></span> <span data-ttu-id="3b12e-158">プリミティブ型は、エイリアスの型と完全には区別されません。予約語を書き込む `Byte` は `System.Byte` の書き込みとまったく同じです。</span><span class="sxs-lookup"><span data-stu-id="3b12e-158">A primitive type is completely indistinguishable from the type it aliases: writing the reserved word `Byte` is exactly the same as writing `System.Byte`.</span></span> <span data-ttu-id="3b12e-159">プリミティブ型は、*組み込み型*とも呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-159">Primitive types are also known as *intrinsic types*.</span></span>

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


<span data-ttu-id="3b12e-160">プリミティブ型は標準型のエイリアスであるため、すべてのプリミティブ型にはメンバーがあります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-160">Because a primitive type aliases a regular type, every primitive type has members.</span></span> <span data-ttu-id="3b12e-161">たとえば、`Integer` には `System.Int32` で宣言されたメンバーがあります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-161">For example, `Integer` has the members declared in `System.Int32`.</span></span> <span data-ttu-id="3b12e-162">リテラルは、対応する型のインスタンスとして扱うことができます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-162">Literals can be treated as instances of their corresponding types.</span></span>

* <span data-ttu-id="3b12e-163">プリミティブ型は、特定の追加操作を許可するという点で、他の構造体型とは異なります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-163">The primitive types differ from other structure types in that they permit certain additional operations:</span></span>

* <span data-ttu-id="3b12e-164">プリミティブ型では、リテラルを記述することによって値を作成できます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-164">Primitive types permit values to be created by writing literals.</span></span> <span data-ttu-id="3b12e-165">たとえば、`123I` は `Integer` 型のリテラルです。</span><span class="sxs-lookup"><span data-stu-id="3b12e-165">For example, `123I` is a literal of type `Integer`.</span></span>

* <span data-ttu-id="3b12e-166">プリミティブ型の定数を宣言することができます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-166">It is possible to declare constants of the primitive types.</span></span>

* <span data-ttu-id="3b12e-167">式のオペランドがすべてのプリミティブ型定数である場合、コンパイラはコンパイル時に式を評価することができます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-167">When the operands of an expression are all primitive type constants, it is possible for the compiler to evaluate the expression at compile time.</span></span> <span data-ttu-id="3b12e-168">このような式は、定数式と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-168">Such an expression is known as a constant expression.</span></span>

<span data-ttu-id="3b12e-169">Visual Basic は、次のプリミティブ型を定義します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-169">Visual Basic defines the following primitive types:</span></span>

* <span data-ttu-id="3b12e-170">整数値型 `Byte` (1 バイト符号なし整数)、`SByte` (1 バイト符号付き整数)、`UShort` (2 バイト符号なし整数)、`Short` (2 バイト符号付き整数)、`UInteger` (4 バイト符号なし整数)、`ULong` (4 バイト符号付き整数)、 (8 バイト符号なし整数)、および `Long` (8 バイト符号付き整数)。</span><span class="sxs-lookup"><span data-stu-id="3b12e-170">The integral value types `Byte` (1-byte unsigned integer), `SByte` (1-byte signed integer), `UShort` (2-byte unsigned integer), `Short` (2-byte signed integer), `UInteger` (4-byte unsigned integer), `Integer` (4-byte signed integer), `ULong` (8-byte unsigned integer), and `Long` (8-byte signed integer).</span></span> <span data-ttu-id="3b12e-171">これらの型は、それぞれ `System.Byte`、`System.SByte`、`System.UInt16`、`System.Int16`、`System.UInt32`、`System.Int32`、`System.UInt64`、`System.Int64` にマップされます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-171">These types map to `System.Byte`, `System.SByte`, `System.UInt16`, `System.Int16`, `System.UInt32`, `System.Int32`, `System.UInt64` and `System.Int64`, respectively.</span></span> <span data-ttu-id="3b12e-172">整数型の既定値は、リテラル `0` に相当します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-172">The default value of an integral type is equivalent to the literal `0`.</span></span>

* <span data-ttu-id="3b12e-173">浮動小数点値型 `Single` (4 バイト浮動小数点) と `Double` (8 バイト浮動小数点) です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-173">The floating-point value types `Single` (4-byte floating point) and `Double` (8-byte floating point).</span></span> <span data-ttu-id="3b12e-174">これらの型は、それぞれ `System.Single` および `System.Double` にマップされます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-174">These types map to `System.Single` and `System.Double`, respectively.</span></span> <span data-ttu-id="3b12e-175">浮動小数点型の既定値は、リテラル `0` に相当します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-175">The default value of a floating-point type is equivalent to the literal `0`.</span></span>

* <span data-ttu-id="3b12e-176">@No__t-0 型 (16 バイトの10進値)。 `System.Decimal` にマップされます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-176">The `Decimal` type (16-byte decimal value), which maps to `System.Decimal`.</span></span> <span data-ttu-id="3b12e-177">Decimal の既定値は、リテラル @no__t 0 に相当します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-177">The default value of decimal is equivalent to the literal `0D`.</span></span>

* <span data-ttu-id="3b12e-178">実際の値を表す @no__t 0 値の型。通常は、リレーショナル操作または論理演算の結果です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-178">The `Boolean` value type, which represents a truth value, typically the result of a relational or logical operation.</span></span> <span data-ttu-id="3b12e-179">リテラルの型は `System.Boolean` です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-179">The literal is of type `System.Boolean`.</span></span> <span data-ttu-id="3b12e-180">@No__t-0 型の既定値は、リテラル `False` に相当します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-180">The default value of the `Boolean` type is equivalent to the literal `False`.</span></span>

* <span data-ttu-id="3b12e-181">日付や時刻を表す @no__t 0 の値の型を `System.DateTime` にマップします。</span><span class="sxs-lookup"><span data-stu-id="3b12e-181">The `Date` value type, which represents a date and/or a time and maps to `System.DateTime`.</span></span> <span data-ttu-id="3b12e-182">@No__t-0 型の既定値は、リテラル `# 01/01/0001 12:00:00AM #` に相当します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-182">The default value of the `Date` type is equivalent to the literal `# 01/01/0001 12:00:00AM #`.</span></span>

* <span data-ttu-id="3b12e-183">@No__t-0 値型。1つの Unicode 文字を表し、`System.Char` にマップされます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-183">The `Char` value type, which represents a single Unicode character and maps to `System.Char`.</span></span> <span data-ttu-id="3b12e-184">@No__t-0 型の既定値は、定数式 `ChrW(0)` に相当します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-184">The default value of the `Char` type is equivalent to the constant expression `ChrW(0)`.</span></span>

* <span data-ttu-id="3b12e-185">@No__t 0 の参照型。 Unicode 文字のシーケンスを表し、`System.String` にマップされます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-185">The `String` reference type, which represents a sequence of Unicode characters and maps to `System.String`.</span></span> <span data-ttu-id="3b12e-186">@No__t-0 型の既定値は null 値です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-186">The default value of the `String` type is a null value.</span></span>



## <a name="enumerations"></a><span data-ttu-id="3b12e-187">列挙</span><span class="sxs-lookup"><span data-stu-id="3b12e-187">Enumerations</span></span>

<span data-ttu-id="3b12e-188">*列挙*型は、`System.Enum` から継承し、シンボリックシンボルを表す値型で、プリミティブ整数型の1つの値のセットを表します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-188">*Enumerations* are value types that inherit from `System.Enum` and symbolically represent a set of values of one of the primitive integral types.</span></span>

```antlr
EnumDeclaration
    : Attributes? TypeModifier* 'Enum' Identifier
      ( 'As' NonArrayTypeName )? StatementTerminator
      EnumMemberDeclaration+
      'End' 'Enum' StatementTerminator
    ;
```

<span data-ttu-id="3b12e-189">列挙型が 0 @no__t の場合、既定値は式によって生成される値 `CType(0, E)` です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-189">For an enumeration type `E`, the default value is the value produced by the expression `CType(0, E)`.</span></span>

<span data-ttu-id="3b12e-190">列挙型の基になる型は、列挙体で定義されているすべての列挙子の値を表すことができる整数型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-190">The underlying type of an enumeration must be an integral type that can represent all the enumerator values defined in the enumeration.</span></span> <span data-ttu-id="3b12e-191">基になる型が指定されている場合は、`Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、@no__t、または @no__t 8 名前空間の対応する型のいずれかである必要があります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-191">If an underlying type is specified, it must be `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, or one of their corresponding types in the `System` namespace.</span></span> <span data-ttu-id="3b12e-192">基になる型が明示的に指定されていない場合、既定値は `Integer` になります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-192">If no underlying type is explicitly specified, the default is `Integer`.</span></span>

<span data-ttu-id="3b12e-193">次の例では、基になる型が `Long` の列挙型を宣言します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-193">The following example declares an enumeration with an underlying type of `Long`:</span></span>

```vb
Enum Color As Long
    Red
    Green
    Blue
End Enum
```

<span data-ttu-id="3b12e-194">開発者は、例に示すように、基になる型 `Long` を使用して、@no__t の範囲内の値を使用できるようにすることができます。ただし、`Integer` の範囲に含まれていない値、または今後このオプションを保持することができます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-194">A developer might choose to use an underlying type of `Long`, as in the example, to enable the use of values that are in the range of `Long`, but not in the range of `Integer`, or to preserve this option for the future.</span></span>


### <a name="enumeration-members"></a><span data-ttu-id="3b12e-195">列挙型のメンバー</span><span class="sxs-lookup"><span data-stu-id="3b12e-195">Enumeration Members</span></span>

<span data-ttu-id="3b12e-196">列挙体のメンバーは、列挙体で宣言された列挙値と、クラス `System.Enum` から継承されたメンバーです。</span><span class="sxs-lookup"><span data-stu-id="3b12e-196">The members of an enumeration are the enumerated values declared in the enumeration and the members inherited from class `System.Enum`.</span></span>

<span data-ttu-id="3b12e-197">列挙体メンバーのスコープは、列挙型宣言の本体です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-197">The scope of an enumeration member is the enumeration declaration body.</span></span> <span data-ttu-id="3b12e-198">つまり、列挙体の宣言の外部では、列挙体のメンバーを常に修飾する必要があります (型が名前空間のインポートによって明示的に名前空間にインポートされる場合を除きます)。</span><span class="sxs-lookup"><span data-stu-id="3b12e-198">This means that outside of an enumeration declaration, an enumeration member must always be qualified (unless the type is specifically imported into a namespace through a namespace import).</span></span>

<span data-ttu-id="3b12e-199">列挙メンバー宣言の宣言順序は、定数式の値が省略されている場合に重要です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-199">Declaration order for enumeration member declarations is significant when constant expression values are omitted.</span></span> <span data-ttu-id="3b12e-200">列挙型のメンバーには、暗黙的に @no__t 0 のアクセスのみが与えられます。列挙メンバー宣言でアクセス修飾子を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-200">Enumeration members implicitly have `Public` access only; no access modifiers are allowed on enumeration member declarations.</span></span>

```antlr
EnumMemberDeclaration
    : Attributes? Identifier ( Equals ConstantExpression )? StatementTerminator
    ;
```

### <a name="enumeration-values"></a><span data-ttu-id="3b12e-201">列挙値</span><span class="sxs-lookup"><span data-stu-id="3b12e-201">Enumeration Values</span></span>

<span data-ttu-id="3b12e-202">列挙メンバーリスト内の列挙値は、基になる列挙型として型指定された定数として宣言され、定数が必要な場所に表示できます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-202">The enumerated values in an enumeration member list are declared as constants typed as the underlying enumeration type, and they can appear wherever constants are required.</span></span> <span data-ttu-id="3b12e-203">@No__t 0 の列挙体メンバー定義は、関連付けられているメンバーに定数式によって示される値を与えます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-203">An enumeration member definition with `=` gives the associated member the value indicated by the constant expression.</span></span> <span data-ttu-id="3b12e-204">定数式は、基になる型に暗黙的に変換可能な整数型に評価される必要があり、基になる型で表すことができる値の範囲内である必要があります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-204">The constant expression must evaluate to an integral type that is implicitly convertible to the underlying type and must be within the range of values that can be represented by the underlying type.</span></span> <span data-ttu-id="3b12e-205">次の例では、定数値 `1.5`、`2.3`、および `3.3` は、厳密なセマンティクスを持つ基になる整数型 `Long` に暗黙的に変換できないため、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-205">The following example is in error because the constant values `1.5`, `2.3`, and `3.3` are not implicitly convertible to the underlying integral type `Long` with strict semantics.</span></span>

```vb
Option Strict On

Enum Color As Long
    Red = 1.5
    Green = 2.3
    Blue = 3.3
End Enum
```

<span data-ttu-id="3b12e-206">次に示すように、複数の列挙メンバーが同じ関連する値を共有する場合があります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-206">Multiple enumeration members may share the same associated value, as shown below:</span></span>

```vb
Enum Color
    Red
    Green
    Blue
    Max = Blue
End Enum
```

<span data-ttu-id="3b12e-207">この例では、2つの列挙メンバー (-`Blue` と `Max`) を持つ列挙体を示しています。これには、同じ値が関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="3b12e-207">The example shows an enumeration that has two enumeration members -- `Blue` and `Max` -- that have the same associated value.</span></span>

<span data-ttu-id="3b12e-208">列挙体の最初の列挙子の値の定義に初期化子がない場合、対応する定数の値は `0` になります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-208">If the first enumerator value definition in the enumeration has no initializer, the value of the corresponding constant is `0`.</span></span> <span data-ttu-id="3b12e-209">初期化子を持たない列挙値の定義では、前の列挙値の値を `1` で増やすことによって取得された値を列挙子に与えます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-209">An enumeration value definition without an initializer gives the enumerator the value obtained by increasing the value of the previous enumeration value by `1`.</span></span> <span data-ttu-id="3b12e-210">この増加した値は、基になる型で表すことができる値の範囲内である必要があります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-210">This increased value must be within the range of values that can be represented by the underlying type.</span></span>

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

<span data-ttu-id="3b12e-211">上記の例では、列挙値とそれに関連付けられた値を出力します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-211">The example above prints the enumeration values and their associated values.</span></span> <span data-ttu-id="3b12e-212">出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-212">The output is:</span></span>

```console
Red = 0
Green = 10
Blue = 11
```

<span data-ttu-id="3b12e-213">値の理由は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="3b12e-213">The reasons for the values are as follows:</span></span>

* <span data-ttu-id="3b12e-214">列挙値 `Red` には、値 `0` が自動的に割り当てられます。これは初期化子がなく、最初の列挙値のメンバーであるためです。</span><span class="sxs-lookup"><span data-stu-id="3b12e-214">The enumeration value `Red` is automatically assigned the value `0` (since it has no initializer and is the first enumeration value member).</span></span>

* <span data-ttu-id="3b12e-215">列挙値 `Green` には、値 `10` が明示的に指定されています。</span><span class="sxs-lookup"><span data-stu-id="3b12e-215">The enumeration value `Green` is explicitly given the value `10`.</span></span>

* <span data-ttu-id="3b12e-216">列挙値 `Blue` には、その前に指定された列挙値より1大きい値が自動的に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-216">The enumeration value `Blue` is automatically assigned the value one greater than the enumeration value that textually precedes it.</span></span>

<span data-ttu-id="3b12e-217">定数式は、それ自体に関連付けられた列挙値の値を直接または間接的に使用することはできません (つまり、定数式での循環は許可されません)。</span><span class="sxs-lookup"><span data-stu-id="3b12e-217">The constant expression may not directly or indirectly use the value of its own associated enumeration value (that is, circularity in the constant expression is not allowed).</span></span> <span data-ttu-id="3b12e-218">次の例は、`A` および `B` の宣言が循環しているため、無効です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-218">The following example is invalid because the declarations of `A` and `B` are circular.</span></span>

```vb
Enum Circular
    A = B
    B
End Enum
```

<span data-ttu-id="3b12e-219">`A` は `B` に明示的に依存し、@no__t は暗黙的に @no__t に依存します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-219">`A` depends on `B` explicitly, and `B` depends on `A` implicitly.</span></span>

## <a name="classes"></a><span data-ttu-id="3b12e-220">クラス</span><span class="sxs-lookup"><span data-stu-id="3b12e-220">Classes</span></span>

<span data-ttu-id="3b12e-221">*クラス*は、データメンバー (定数、変数、およびイベント)、関数メンバー (メソッド、プロパティ、インデクサー、演算子、およびコンストラクター)、および入れ子にされた型を含むことができるデータ構造です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-221">A *class* is a data structure that may contain data members (constants, variables, and events), function members (methods, properties, indexers, operators, and constructors), and nested types.</span></span> <span data-ttu-id="3b12e-222">クラスは参照型です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-222">Classes are reference types.</span></span>

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

<span data-ttu-id="3b12e-223">次の例は、各メンバーの種類を含むクラスを示しています。</span><span class="sxs-lookup"><span data-stu-id="3b12e-223">The following example shows a class that contains each kind of member:</span></span>

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

<span data-ttu-id="3b12e-224">これらのメンバーの使用例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-224">The following example shows uses of these members:</span></span>

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

<span data-ttu-id="3b12e-225">クラス固有の修飾子には、@no__t 0 と `NotInheritable` の2つがあります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-225">There are two class-specific modifiers, `MustInherit` and `NotInheritable`.</span></span> <span data-ttu-id="3b12e-226">両方を指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-226">It is invalid to specify them both.</span></span>


### <a name="class-base-specification"></a><span data-ttu-id="3b12e-227">クラスの基本指定</span><span class="sxs-lookup"><span data-stu-id="3b12e-227">Class Base Specification</span></span>

<span data-ttu-id="3b12e-228">クラス宣言には、クラスの直接の基本型を定義する基本型の仕様を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-228">A class declaration may include a base type specification that defines the direct base type of the class.</span></span>

```antlr
ClassBase
    : 'Inherits' NonArrayTypeName StatementTerminator
    ;
```

<span data-ttu-id="3b12e-229">クラス宣言に明示的な基本型がない場合、直接の基本型は暗黙的に `Object` になります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-229">If a class declaration has no explicit base type, the direct base type is implicitly `Object`.</span></span> <span data-ttu-id="3b12e-230">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-230">For example:</span></span>

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class
```

<span data-ttu-id="3b12e-231">基本型は独自の型パラメーターにすることはできませんが、スコープ内の型パラメーターが含まれる場合もあります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-231">Base types cannot be a type parameter on its own, although it may involve the type parameters that are in scope.</span></span>

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

<span data-ttu-id="3b12e-232">クラスは、`Object` およびクラスからのみ派生することができます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-232">Classes may only derive from `Object` and classes.</span></span> <span data-ttu-id="3b12e-233">クラスを `System.ValueType`、`System.Enum`、`System.Array`、`System.MulticastDelegate`、または @no__t から派生させることはできません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-233">It is invalid for a class to derive from `System.ValueType`, `System.Enum`, `System.Array`, `System.MulticastDelegate` or `System.Delegate`.</span></span> <span data-ttu-id="3b12e-234">ジェネリッククラスは、`System.Attribute` から派生したクラスから派生することはできません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-234">A generic class cannot derive from `System.Attribute` or from a class that derives from it.</span></span>

<span data-ttu-id="3b12e-235">各クラスには直接基底クラスが1つだけあり、派生での循環は禁止されています。</span><span class="sxs-lookup"><span data-stu-id="3b12e-235">Every class has exactly one direct base class, and circularity in derivation is prohibited.</span></span> <span data-ttu-id="3b12e-236">@No__t 0 クラスから派生させることはできません。また、基底クラスのアクセシビリティドメインは、クラス自体のアクセシビリティドメインのスーパーセットと同じであるか、またはそのスーパーセットと同じである必要があります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-236">It is not possible to derive from a `NotInheritable` class, and the accessibility domain of the base class must be the same as or a superset of the accessibility domain of the class itself.</span></span>


### <a name="class-members"></a><span data-ttu-id="3b12e-237">クラス メンバー</span><span class="sxs-lookup"><span data-stu-id="3b12e-237">Class Members</span></span>

<span data-ttu-id="3b12e-238">クラスのメンバーは、そのクラスメンバー宣言によって導入されたメンバーと、その直接の基底クラスから継承されたメンバーで構成されます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-238">The members of a class consist of the members introduced by its class member declarations and the members inherited from its direct base class.</span></span>

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

<span data-ttu-id="3b12e-239">クラスメンバーの宣言には、@no__t 0、`Protected`、`Friend`、`Protected Friend`、または @no__t 4 のアクセスが含まれている場合があります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-239">A class member declaration may have `Public`, `Protected`, `Friend`, `Protected Friend`, or `Private` access.</span></span> <span data-ttu-id="3b12e-240">クラスメンバーの宣言にアクセス修飾子が含まれていない場合、宣言は、変数宣言でない限り、既定で `Public` アクセスに設定されます。この場合、既定では `Private` アクセスに設定されます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-240">When a class member declaration does not include an access modifier, the declaration defaults to `Public` access, unless it is a variable declaration; in that case it defaults to `Private` access.</span></span>

<span data-ttu-id="3b12e-241">クラスメンバーのスコープは、メンバー宣言が発生するクラス本体と、そのクラスの制約リスト (ジェネリックであり、制約がある場合) です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-241">The scope of a class member is the class body in which the member declaration occurs, plus the constraint list of that class (if it is generic and has constraints).</span></span> <span data-ttu-id="3b12e-242">メンバーが @no__t 0 のアクセス権を持っている場合、そのスコープは、同じプログラム内の任意の派生クラスのクラス本体または `Friend` アクセスが与えられている任意のアセンブリにまで拡張されます。また、メンバーが `Public`、`Protected`、または @no__t 4 のアクセス権を持っている場合、そのスコープは任意の任意のプログラムの v) クラス。</span><span class="sxs-lookup"><span data-stu-id="3b12e-242">If the member has `Friend` access, its scope extends to the class body of any derived class in the same program or any assembly that has been given `Friend` access, and if the member has `Public`, `Protected`, or `Protected Friend` access, its scope extends to the class body of any derived class in any program.</span></span>


## <a name="structures"></a><span data-ttu-id="3b12e-243">構造体</span><span class="sxs-lookup"><span data-stu-id="3b12e-243">Structures</span></span>

<span data-ttu-id="3b12e-244">*構造体*は、`System.ValueType` から継承する値型です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-244">*Structures* are value types that inherit from `System.ValueType`.</span></span> <span data-ttu-id="3b12e-245">構造体は、データメンバーと関数メンバーを含むことができるデータ構造を表すという点でクラスに似ています。</span><span class="sxs-lookup"><span data-stu-id="3b12e-245">Structures are similar to classes in that they represent data structures that can contain data members and function members.</span></span> <span data-ttu-id="3b12e-246">ただし、クラスとは異なり、構造体はヒープ割り当てを必要としません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-246">Unlike classes, however, structures do not require heap allocation.</span></span>

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

<span data-ttu-id="3b12e-247">クラスの場合、2つの変数が同じオブジェクトを参照する可能性があります。したがって、ある変数に対する操作が、もう一方の変数によって参照されるオブジェクトに影響を与える可能性があります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-247">In the case of classes, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="3b12e-248">構造体を使用すると、各変数には @no__t 0 以外のデータの独自のコピーが含まれるため、次の例に示すように、1つの変数に対する操作がもう一方に影響を与えることはできません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-248">With structures, the variables each have their own copy of the non-`Shared` data, so it is not possible for operations on one to affect the other, as the following example illustrates:</span></span>

```vb
Structure Point
    Public x, y As Integer

    Public Sub New(x As Integer, y As Integer)
        Me.x = x
        Me.y = y
    End Sub
End Structure
```

<span data-ttu-id="3b12e-249">上記の宣言を指定した場合、次のコードは値を `10` に出力します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-249">Given the above declaration the following code outputs the value `10`:</span></span>

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

<span data-ttu-id="3b12e-250">@No__t-0 から `b` への割り当てによって値のコピーが作成されるため、`b` は `a.x` への割り当ての影響を受けません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-250">The assignment of `a` to `b` creates a copy of the value, and `b` is thus unaffected by the assignment to `a.x`.</span></span> <span data-ttu-id="3b12e-251">@No__t-0 はクラスとして宣言されていましたが、`a` と `b` が同じオブジェクトを参照するため、出力は-1 @no__t ます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-251">Had `Point` instead been declared as a class, the output would be `100` because `a` and `b` would reference the same object.</span></span>


### <a name="structure-members"></a><span data-ttu-id="3b12e-252">構造体のメンバー</span><span class="sxs-lookup"><span data-stu-id="3b12e-252">Structure Members</span></span>

<span data-ttu-id="3b12e-253">構造体のメンバーは、構造体のメンバー宣言および @no__t から継承されたメンバーによって導入されたメンバーです。</span><span class="sxs-lookup"><span data-stu-id="3b12e-253">The members of a structure are the members introduced by its structure member declarations and the members inherited from `System.ValueType`.</span></span>

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

<span data-ttu-id="3b12e-254">すべての構造体には、構造体の既定値を生成する @no__t 0 のパラメーターなしのコンストラクターが暗黙的に設定されます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-254">Every structure implicitly has a `Public` parameterless instance constructor that produces the default value of the structure.</span></span> <span data-ttu-id="3b12e-255">その結果、構造体型の宣言でパラメーターなしのインスタンスコンストラクターを宣言することはできません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-255">As a result, it is not possible for a structure type declaration to declare a parameterless instance constructor.</span></span> <span data-ttu-id="3b12e-256">ただし、構造体型は、次の例のように、*パラメーター化*されたインスタンスコンストラクターを宣言できます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-256">A structure type is, however, permitted to declare *parameterized* instance constructors, as in the following example:</span></span>

```vb
Structure Point
    Private x, y As Integer

    Public Sub New(x As Integer, y As Integer)
        Me.x = x
        Me.y = y
    End Sub
End Structure
```

<span data-ttu-id="3b12e-257">上記の宣言を指定した場合、次のステートメントはどちらも `x` と @no__t が0に初期化された @no__t 0 を作成します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-257">Given the above declaration, the following statements both create a `Point` with `x` and `y` initialized to zero.</span></span>

```vb
Dim p1 As Point = New Point()
Dim p2 As Point = New Point(0, 0)
```

<span data-ttu-id="3b12e-258">構造体には、それらの値を参照するのではなく、フィールド値が直接含まれているため、構造体には、自身を直接または間接的に参照するフィールドを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-258">Because structures directly contain their field values (rather than references to those values), structures cannot contain fields that directly or indirectly reference themselves.</span></span> <span data-ttu-id="3b12e-259">たとえば、次のコードは無効です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-259">For example, the following code is not valid:</span></span>

```vb
Structure S1
    Dim f1 As S2
End Structure

Structure S2
    ' This would require S1 to contain itself.
    Dim f1 As S1
End Structure
```

<span data-ttu-id="3b12e-260">通常、構造体メンバーの宣言には、@no__t 0、`Friend`、または @no__t 2 のアクセスのみが許可されますが、`Object` から継承されたメンバーをオーバーライドする場合は、@no__t と @no__t を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-260">Normally, a structure member declaration may only have `Public`, `Friend`, or `Private` access, but when overriding members inherited from `Object`, `Protected` and `Protected Friend` access may also be used.</span></span> <span data-ttu-id="3b12e-261">構造体メンバーの宣言にアクセス修飾子が含まれていない場合、宣言は既定で `Public` アクセスになります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-261">When a structure member declaration does not include an access modifier, the declaration defaults to `Public` access.</span></span> <span data-ttu-id="3b12e-262">構造体によって宣言されるメンバーのスコープは、宣言が発生する構造体の本体と、その構造体の制約 (ジェネリックであり、制約がある場合) です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-262">The scope of a member declared by a structure is the structure body in which the declaration occurs, plus the constraints of that structure (if it was generic and had constraints).</span></span>


## <a name="standard-modules"></a><span data-ttu-id="3b12e-263">標準モジュール</span><span class="sxs-lookup"><span data-stu-id="3b12e-263">Standard Modules</span></span>

<span data-ttu-id="3b12e-264">*標準モジュール*は、メンバーが暗黙的に `Shared` であり、標準モジュールの宣言自体に対してではなく、名前空間が含まれる標準モジュールの宣言空間をスコープとする型です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-264">A *standard module* is a type whose members are implicitly `Shared` and scoped to the declaration space of the standard module's containing namespace, rather than just to the standard module declaration itself.</span></span> <span data-ttu-id="3b12e-265">標準モジュールをインスタンス化することはできません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-265">Standard modules may never be instantiated.</span></span> <span data-ttu-id="3b12e-266">標準モジュール型の変数を宣言すると、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-266">It is an error to declare a variable of a standard module type.</span></span>

```antlr
ModuleDeclaration
    : Attributes? TypeModifier* 'Module' Identifier StatementTerminator
      ModuleMemberDeclaration*
      'End' 'Module' StatementTerminator
    ;
```

<span data-ttu-id="3b12e-267">標準モジュールのメンバーには、2つの完全修飾名があります。1つは標準モジュール名を除いた名前で、もう1つは標準モジュール名を持ちます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-267">A member of a standard module has two fully qualified names, one without the standard module name and one with the standard module name.</span></span> <span data-ttu-id="3b12e-268">名前空間の複数の標準モジュールで、特定の名前を持つメンバーを定義できます。どちらのモジュールにも含まれていない名前への非修飾参照があいまいです。</span><span class="sxs-lookup"><span data-stu-id="3b12e-268">More than one standard module in a namespace may define a member with a particular name; unqualified references to the name outside of either module are ambiguous.</span></span> <span data-ttu-id="3b12e-269">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-269">For example:</span></span>

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

<span data-ttu-id="3b12e-270">モジュールは名前空間でのみ宣言できます。別の型に入れ子にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-270">A module may only be declared in a namespace and may not be nested in another type.</span></span> <span data-ttu-id="3b12e-271">標準モジュールは、インターフェイスを実装することはできず、`Object` から暗黙的に派生し、@no__t コンストラクターのみを持ちます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-271">Standard modules may not implement interfaces, they implicitly derive from `Object`, and they have only `Shared` constructors.</span></span>


### <a name="standard-module-members"></a><span data-ttu-id="3b12e-272">標準モジュールメンバー</span><span class="sxs-lookup"><span data-stu-id="3b12e-272">Standard Module Members</span></span>

<span data-ttu-id="3b12e-273">標準モジュールのメンバーは、メンバー宣言によって導入されたメンバーと `Object` から継承されたメンバーです。</span><span class="sxs-lookup"><span data-stu-id="3b12e-273">The members of a standard module are the members introduced by its member declarations and the members inherited from `Object`.</span></span> <span data-ttu-id="3b12e-274">標準モジュールには、インスタンスコンストラクター以外の任意の型のメンバーを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-274">Standard modules may have any type of member except instance constructors.</span></span> <span data-ttu-id="3b12e-275">標準モジュールの型のメンバーはすべて、暗黙的に @no__t 0 になります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-275">All standard module type members are implicitly `Shared`.</span></span>

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

<span data-ttu-id="3b12e-276">通常、標準モジュールメンバーの宣言には、@no__t 0、`Friend`、または @no__t 2 のアクセスのみが許可されますが、`Object` から継承されたメンバーをオーバーライドする場合は、`Protected` および `Protected Friend` のアクセス修飾子を指定できます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-276">Normally, a standard module member declaration may only have `Public`, `Friend`, or `Private` access, but when overriding members inherited from `Object`, the `Protected` and `Protected Friend` access modifiers may be specified.</span></span> <span data-ttu-id="3b12e-277">標準モジュールメンバー宣言にアクセス修飾子が含まれていない場合、既定では、変数でない限り、宣言は @no__t 0 アクセスに設定されます。ただし、既定では `Private` アクセスが使用されます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-277">When a standard module member declaration does not include an access modifier, the declaration defaults to `Public` access, unless it is a variable, which defaults to `Private` access.</span></span>

<span data-ttu-id="3b12e-278">前述のように、標準モジュールメンバーのスコープは、標準モジュール宣言を含む宣言です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-278">As previously noted, the scope of a standard module member is the declaration containing the standard module declaration.</span></span> <span data-ttu-id="3b12e-279">@No__t-0 から継承されたメンバーは、この特別なスコープには含まれません。これらのメンバーにはスコープがなく、常にモジュール名で修飾する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-279">Members inherited from `Object` are not included in this special scoping; those members have no scope and must always be qualified with the name of the module.</span></span> <span data-ttu-id="3b12e-280">メンバーが @no__t 0 のアクセス権を持っている場合、そのスコープは、同じプログラムで宣言されている名前空間メンバーまたは `Friend` アクセスが与えられているアセンブリに対してのみ拡張されます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-280">If the member has `Friend` access, its scope extends only to namespace members declared in the same program or assemblies that have been given `Friend` access.</span></span>


## <a name="interfaces"></a><span data-ttu-id="3b12e-281">インターフェイス</span><span class="sxs-lookup"><span data-stu-id="3b12e-281">Interfaces</span></span>

<span data-ttu-id="3b12e-282">*インターフェイス*は、他の型が特定のメソッドをサポートすることを保証するために実装する参照型です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-282">*Interfaces* are reference types that other types implement to guarantee that they support certain methods.</span></span> <span data-ttu-id="3b12e-283">インターフェイスは直接作成されることはなく、実際の表現はありません。他の型はインターフェイス型に変換する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-283">An interface is never directly created and has no actual representation -- other types must be converted to an interface type.</span></span> <span data-ttu-id="3b12e-284">インターフェイスはコントラクトを定義します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-284">An interface defines a contract.</span></span> <span data-ttu-id="3b12e-285">インターフェイスを実装するクラスまたは構造体は、コントラクトに従う必要があります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-285">A class or structure that implements an interface must adhere to its contract.</span></span>

```antlr
InterfaceDeclaration
    : Attributes? TypeModifier* 'Interface' Identifier
      TypeParameterList? StatementTerminator
      InterfaceBase*
      InterfaceMemberDeclaration*
      'End' 'Interface' StatementTerminator
    ;
```


<span data-ttu-id="3b12e-286">次の例は、既定のプロパティ `Item`、イベント `E`、メソッド `F`、およびプロパティ @no__t を含むインターフェイスを示しています。</span><span class="sxs-lookup"><span data-stu-id="3b12e-286">The following example shows an interface that contains a default property `Item`, an event `E`, a method `F`, and a property `P`:</span></span>

```vb
Interface IExample
    Default Property Item(index As Integer) As String

    Event E()

    Sub F(value As Integer)

    Property P() As String
End Interface
```

<span data-ttu-id="3b12e-287">インターフェイスは、多重継承を使用する場合があります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-287">Interfaces may employ multiple inheritance.</span></span> <span data-ttu-id="3b12e-288">次の例では、-0 @no__t のインターフェイスが `ITextBox` と `IListBox` の両方から継承されます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-288">In the following example, the interface `IComboBox` inherits from both `ITextBox` and `IListBox`:</span></span>

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

<span data-ttu-id="3b12e-289">クラスと構造体は、複数のインターフェイスを実装できます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-289">Classes and structures can implement multiple interfaces.</span></span> <span data-ttu-id="3b12e-290">次の例では、クラス `EditBox` はクラス @no__t から派生し、`IControl` と `IDataBound` の両方を実装しています。</span><span class="sxs-lookup"><span data-stu-id="3b12e-290">In the following example, the class `EditBox` derives from the class `Control` and implements both `IControl` and `IDataBound`:</span></span>

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


### <a name="interface-inheritance"></a><span data-ttu-id="3b12e-291">インターフェイスの継承</span><span class="sxs-lookup"><span data-stu-id="3b12e-291">Interface Inheritance</span></span>

<span data-ttu-id="3b12e-292">インターフェイスの基本インターフェイスは、明示的な基本インターフェイスとその基本インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="3b12e-292">The base interfaces of an interface are the explicit base interfaces and their base interfaces.</span></span> <span data-ttu-id="3b12e-293">つまり、基本インターフェイスのセットは、明示的な基本インターフェイス、その明示的な基本インターフェイスなどの完全な推移的なクロージャです。</span><span class="sxs-lookup"><span data-stu-id="3b12e-293">In other words, the set of base interfaces is the complete transitive closure of the explicit base interfaces, their explicit base interfaces, and so on.</span></span> <span data-ttu-id="3b12e-294">インターフェイス宣言に明示的なインターフェイスベースがない場合、型の基本インターフェイスはありません。--インターフェイスは `Object` から継承しません (ただし、`Object` への拡大変換があります)。</span><span class="sxs-lookup"><span data-stu-id="3b12e-294">If an interface declaration has no explicit interface base, then there is no base interface for the type -- interfaces do not inherit from `Object` (although they do have a widening conversion to `Object`).</span></span>

```antlr
InterfaceBase
    : 'Inherits' InterfaceBases StatementTerminator
    ;

InterfaceBases
    : NonArrayTypeName ( Comma NonArrayTypeName )*
    ;
```

<span data-ttu-id="3b12e-295">次の例では、`IComboBox` の基本インターフェイスが `IControl`、`ITextBox`、および `IListBox` です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-295">In the following example, the base interfaces of `IComboBox` are `IControl`, `ITextBox`, and `IListBox`.</span></span>

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

<span data-ttu-id="3b12e-296">インターフェイスは、その基本インターフェイスのすべてのメンバーを継承します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-296">An interface inherits all members of its base interfaces.</span></span> <span data-ttu-id="3b12e-297">言い換えると、上の @no__t 0 インターフェイスは、-1、`SetItems`、および `Paint` のメンバー @no__t を継承します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-297">In other words, the `IComboBox` interface above inherits members `SetText` and `SetItems` as well as `Paint`.</span></span>

<span data-ttu-id="3b12e-298">インターフェイスを実装するクラスまたは構造体は、インターフェイスのすべての基本インターフェイスも暗黙的に実装します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-298">A class or structure that implements an interface also implicitly implements all of the interface's base interfaces.</span></span>

<span data-ttu-id="3b12e-299">インターフェイスが基底インターフェイスの推移的なクロージャに複数回出現する場合、そのインターフェイスは派生インターフェイスに1回だけメンバーを提供します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-299">If an interface appears more than once in the transitive closure of the base interfaces, it only contributes its members to the derived interface once.</span></span> <span data-ttu-id="3b12e-300">派生インターフェイスを実装する型は、多重定義された基本インターフェイスのメソッドを1回だけ実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-300">A type implementing the derived interface only has to implement the methods of the multiply defined base interface once.</span></span> <span data-ttu-id="3b12e-301">次の例では、クラスが `IComboBox` と `IControl` を実装している場合でも、`Paint` のみを実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-301">In the following example, `Paint` only needs to be implemented once, even though the class implements `IComboBox` and `IControl`.</span></span>

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

<span data-ttu-id="3b12e-302">@No__t-0 句は、他の `Inherits` 句には影響しません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-302">An `Inherits` clause has no effect on other `Inherits` clauses.</span></span> <span data-ttu-id="3b12e-303">次の例では、`IDerived` は、`INested` の名前を `IBase` で修飾する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-303">In the following example, `IDerived` must qualify the name of `INested` with `IBase`.</span></span>

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

<span data-ttu-id="3b12e-304">基底インターフェイスのアクセシビリティドメインは、インターフェイス自体のアクセシビリティドメインのスーパーセットと同じである必要があります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-304">The accessibility domain of a base interface must be the same as or a superset of the accessibility domain of the interface itself.</span></span>


### <a name="interface-members"></a><span data-ttu-id="3b12e-305">インターフェイスのメンバー</span><span class="sxs-lookup"><span data-stu-id="3b12e-305">Interface Members</span></span>

<span data-ttu-id="3b12e-306">インターフェイスのメンバーは、メンバー宣言によって導入されたメンバーと、その基本インターフェイスから継承されたメンバーで構成されます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-306">The members of an interface consist of the members introduced by its member declarations and the members inherited from its base interfaces.</span></span>

```antlr
InterfaceMemberDeclaration
    : NonModuleDeclaration
    | InterfaceEventMemberDeclaration
    | InterfaceMethodMemberDeclaration
    | InterfacePropertyMemberDeclaration
    ;
```

<span data-ttu-id="3b12e-307">インターフェイスは `Object` のメンバーを継承しませんが、インターフェイスを実装するすべてのクラスまたは構造体は `Object` から継承するため、拡張メソッドを含む @no__t のメンバーはインターフェイスのメンバーと見なされ、`Object` へのキャストを必要としない直接インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="3b12e-307">Although interfaces do not inherit members from `Object`, because every class or structure that implements an interface does inherit from `Object`, the members of `Object`, including extension methods, are considered members of an interface and can be called on an interface directly without requiring a cast to `Object`.</span></span> <span data-ttu-id="3b12e-308">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-308">For example:</span></span>

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

<span data-ttu-id="3b12e-309">@No__t-0 のメンバーと同じ名前を持つインターフェイスのメンバーは、`Object` のメンバーを暗黙的にシャドウします。</span><span class="sxs-lookup"><span data-stu-id="3b12e-309">Members of an interface with the same name as members of `Object` implicitly shadow `Object` members.</span></span> <span data-ttu-id="3b12e-310">インターフェイスのメンバーには、入れ子にされた型、メソッド、プロパティ、およびイベントのみを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-310">Only nested types, methods, properties, and events may be members of an interface.</span></span> <span data-ttu-id="3b12e-311">メソッドとプロパティに本体を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-311">Methods and properties may not have a body.</span></span> <span data-ttu-id="3b12e-312">インターフェイスメンバーは、暗黙的に 0 @no__t、アクセス修飾子を指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-312">Interface members are implicitly `Public` and may not specify an access modifier.</span></span> <span data-ttu-id="3b12e-313">インターフェイスで宣言されているメンバーのスコープは、宣言が行われるインターフェイス本体と、そのインターフェイスの制約リスト (ジェネリックで、制約がある場合) です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-313">The scope of a member declared in an interface is the interface body in which the declaration occurs, plus the constraint list of that interface (if it is generic and has constraints).</span></span>


## <a name="arrays"></a><span data-ttu-id="3b12e-314">配列</span><span class="sxs-lookup"><span data-stu-id="3b12e-314">Arrays</span></span>

<span data-ttu-id="3b12e-315">*配列*は、配列内の変数の順序を使用して、一対一の方法で*インデックス*を使用してアクセスされる変数を含む参照型です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-315">An *array* is a reference type that contains variables accessed through *indices* corresponding in a one-to-one fashion with the order of the variables in the array.</span></span> <span data-ttu-id="3b12e-316">配列に含まれる変数は、配列の*要素*とも呼ばれ、すべて同じ型である必要があり、この型は配列の*要素型*と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-316">The variables contained in an array, also called the *elements* of the array, must all be of the same type, and this type is called the *element type* of the array.</span></span>

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

<span data-ttu-id="3b12e-317">配列の要素は、配列のインスタンスが作成されるときに存在し、配列のインスタンスが破棄されると存在しなくなります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-317">The elements of an array come into existence when an array instance is created, and cease to exist when the array instance is destroyed.</span></span> <span data-ttu-id="3b12e-318">配列の各要素は、その型の既定値に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-318">Each element of an array is initialized to the default value of its type.</span></span> <span data-ttu-id="3b12e-319">型 `System.Array` はすべての配列型の基本型であり、インスタンス化することはできません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-319">The type `System.Array` is the base type of all array types and may not be instantiated.</span></span> <span data-ttu-id="3b12e-320">すべての配列型は、@no__t 0 型によって宣言されたメンバーを継承し、それに変換できます (と `Object`)。</span><span class="sxs-lookup"><span data-stu-id="3b12e-320">Every array type inherits the members declared by the `System.Array` type and is convertible to it (and `Object`).</span></span> <span data-ttu-id="3b12e-321">要素 @no__t が0の1次元配列型は、インターフェイス `System.Collections.Generic.IList(Of T)` および `IReadOnlyList(Of T)`; も実装します。`T` が参照型の場合、配列型は、`T` からの拡大参照変換を持つすべての `U` に対して `IList(Of U)` と `IReadOnlyList(Of U)` を実装します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-321">A one-dimensional array type with element `T` also implements the interfaces `System.Collections.Generic.IList(Of T)` and `IReadOnlyList(Of T)`; if `T` is a reference type, then the array type also implements `IList(Of U)` and `IReadOnlyList(Of U)` for any `U` that has a widening  reference conversion from `T`.</span></span>

<span data-ttu-id="3b12e-322">配列には、各配列要素に関連付けられているインデックスの数を決定する*ランク*があります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-322">An array has a *rank* that determines the number of indices associated with each array element.</span></span> <span data-ttu-id="3b12e-323">配列のランクによって、配列の*次元*数が決まります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-323">The rank of an array determines the number of *dimensions* of the array.</span></span> <span data-ttu-id="3b12e-324">たとえば、ランクが1の配列は1次元配列と呼ばれ、ランクが1より大きい配列は多次元配列と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-324">For example, an array with a rank of one is called a single-dimensional array, and an array with a rank greater than one is called a multidimensional array.</span></span>

<span data-ttu-id="3b12e-325">次の例では、整数値の1次元配列を作成し、配列要素を初期化して、それぞれを出力します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-325">The following example creates a single-dimensional array of integer values, initializes the array elements, and then prints each of them out:</span></span>

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

<span data-ttu-id="3b12e-326">プログラムは次の出力を出力します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-326">The program outputs the following:</span></span>

```console
arr(0) = 0
arr(1) = 1
arr(2) = 4
arr(3) = 9
arr(4) = 16
arr(5) = 25
```

<span data-ttu-id="3b12e-327">配列の各次元には、関連付けられた長さがあります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-327">Each dimension of an array has an associated length.</span></span> <span data-ttu-id="3b12e-328">次元の長さは配列の型の一部ではなく、実行時に配列型のインスタンスが作成されるときに設定されます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-328">Dimension lengths are not part of the type of the array, but rather are established when an instance of the array type is created at run time.</span></span> <span data-ttu-id="3b12e-329">ディメンションの長さによって、そのディメンションのインデックスの有効範囲が決まります。長さが `N` の場合、インデックスの範囲は 0 ~ `N-1` です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-329">The length of a dimension determines the valid range of indices for that dimension: for a dimension of length `N`, indices can range from zero to `N-1`.</span></span> <span data-ttu-id="3b12e-330">ディメンションの長さがゼロの場合、そのディメンションに有効なインデックスはありません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-330">If a dimension is of length zero, there are no valid indices for that dimension.</span></span> <span data-ttu-id="3b12e-331">配列内の要素の合計数は、配列内の各次元の長さの積です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-331">The total number of elements in an array is the product of the lengths of each dimension in the array.</span></span> <span data-ttu-id="3b12e-332">配列のいずれかの次元の長さがゼロの場合、配列は空であると言われます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-332">If any of the dimensions of an array has a length of zero, the array is said to be empty.</span></span> <span data-ttu-id="3b12e-333">配列の要素型には、任意の型を指定できます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-333">The element type of an array can be any type.</span></span>

<span data-ttu-id="3b12e-334">配列型は、既存の型名に修飾子を追加することによって指定されます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-334">Array types are specified by adding a modifier to an existing type name.</span></span> <span data-ttu-id="3b12e-335">修飾子は、左かっこ、0個以上のコンマのセット、および右かっこで構成されます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-335">The modifier consists of a left parenthesis, a set of zero or more commas, and a right parenthesis.</span></span> <span data-ttu-id="3b12e-336">変更される型は配列の要素型で、次元の数はコンマと1を加算した数になります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-336">The type modified is the element type of the array, and the number of dimensions is the number of commas plus one.</span></span> <span data-ttu-id="3b12e-337">複数の修飾子が指定されている場合、配列の要素の型は配列になります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-337">If more than one modifier is specified, then the element type of the array is an array.</span></span> <span data-ttu-id="3b12e-338">修飾子は左から右に読み取られ、一番左の修飾子は最も外側の配列になります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-338">The modifiers are read left to right, with the leftmost modifier being the outermost array.</span></span> <span data-ttu-id="3b12e-339">この例では、</span><span class="sxs-lookup"><span data-stu-id="3b12e-339">In the example</span></span>

```vb
Module Test
    Dim arr As Integer(,)(,,)()
End Module
```

<span data-ttu-id="3b12e-340">`arr` の要素の型は、`Integer` の1次元配列の3次元配列の2次元配列です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-340">the element type of `arr` is a two-dimensional array of three-dimensional arrays of one-dimensional arrays of `Integer`.</span></span>

<span data-ttu-id="3b12e-341">変数名に配列型修飾子または配列サイズ初期化修飾子を指定することにより、変数を配列型として宣言することもできます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-341">A variable may also be declared to be of an array type by putting an array type modifier or an array-size initialization modifier on the variable name.</span></span> <span data-ttu-id="3b12e-342">この場合、配列要素の型は宣言で指定された型であり、配列の次元は変数名修飾子によって決定されます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-342">In that case, the array element type is the type given in the declaration, and the array dimensions are determined by the variable name modifier.</span></span> <span data-ttu-id="3b12e-343">わかりやすくするために、同じ宣言内の変数名と型名の両方に配列型修飾子を指定することは無効です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-343">For clarity, it is not valid to have an array type modifier on both a variable name and a type name in the same declaration.</span></span>

<span data-ttu-id="3b12e-344">次の例は、要素型として `Integer` の配列型を使用する、さまざまなローカル変数宣言を示しています。</span><span class="sxs-lookup"><span data-stu-id="3b12e-344">The following example shows a variety of local variable declarations that use array types with `Integer` as the element type:</span></span>

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

<span data-ttu-id="3b12e-345">配列型の名前修飾子は、その後に続くすべてのかっこのセットに拡張されます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-345">An array type name modifier extends to all sets of parentheses that follow it.</span></span> <span data-ttu-id="3b12e-346">これは、かっこで囲まれた一連の引数が型名の後で許可されている場合に、配列型名の引数を指定できないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-346">This means that in the situations where a set of arguments enclosed in parenthesis is allowed after a type name, it is not possible to specify the arguments for an array type name.</span></span> <span data-ttu-id="3b12e-347">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-347">For example:</span></span>

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

<span data-ttu-id="3b12e-348">最後の例では、`(3)` は、コンストラクター引数のセットとしてではなく、型名の一部として解釈されます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-348">In the last case, `(3)` is interpreted as part of the type name rather than as a set of constructor arguments.</span></span>


## <a name="delegates"></a><span data-ttu-id="3b12e-349">デリゲート</span><span class="sxs-lookup"><span data-stu-id="3b12e-349">Delegates</span></span>

<span data-ttu-id="3b12e-350">*デリゲート*は、型の `Shared` メソッドまたはオブジェクトのインスタンスメソッドを参照する参照型です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-350">A *delegate* is a reference type that refers to a `Shared` method of a type or to an instance method of an object.</span></span>

```antlr
DelegateDeclaration
    : Attributes? TypeModifier* 'Delegate' MethodSignature StatementTerminator
    ;

MethodSignature
    : SubSignature
    | FunctionSignature
    ;
```

 <span data-ttu-id="3b12e-351">他の言語のデリゲートに最も近いものは関数ポインターですが、関数ポインターは @no__t 0 の関数のみを参照できますが、デリゲートは `Shared` メソッドとインスタンスメソッドの両方を参照できます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-351">The closest equivalent of a delegate in other languages is a function pointer, but whereas a function pointer can only reference `Shared` functions, a delegate can reference both `Shared` and instance methods.</span></span> <span data-ttu-id="3b12e-352">後者の場合、デリゲートは、メソッドのエントリポイントへの参照だけでなく、メソッドを呼び出すために使用するオブジェクトインスタンスへの参照も格納します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-352">In the latter case, the delegate stores not only a reference to the method's entry point, but also a reference to the object instance with which to invoke the method.</span></span>

<span data-ttu-id="3b12e-353">デリゲート宣言には、@no__t 0 句、`Implements` 句、メソッド本体、または `End` コンストラクトを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-353">The delegate declaration may not have  a `Handles` clause, an `Implements` clause, a method body, or an `End` construct.</span></span> <span data-ttu-id="3b12e-354">デリゲート宣言のパラメーターリストに `Optional` または `ParamArray` パラメーターを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-354">The parameter list of the delegate declaration may not contain `Optional` or `ParamArray` parameters.</span></span> <span data-ttu-id="3b12e-355">戻り値の型とパラメーターの型のアクセシビリティドメインは、またはデリゲート自体のアクセシビリティドメインのスーパーセットと同じである必要があります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-355">The accessibility domain of the return type and parameter types must be the same as or a superset of the accessibility domain of the delegate itself.</span></span>

<span data-ttu-id="3b12e-356">デリゲートのメンバーは、クラス `System.Delegate` から継承されたメンバーです。</span><span class="sxs-lookup"><span data-stu-id="3b12e-356">The members of a delegate are the members inherited from class `System.Delegate`.</span></span> <span data-ttu-id="3b12e-357">デリゲートは、次のメソッドも定義します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-357">A delegate also defines the following methods:</span></span>

* <span data-ttu-id="3b12e-358">2つのパラメーターを受け取るコンストラクター。 `Object` の型と `System.IntPtr` の型のいずれかです。</span><span class="sxs-lookup"><span data-stu-id="3b12e-358">A constructor that takes two parameters, one of type `Object` and one of type `System.IntPtr`.</span></span>

* <span data-ttu-id="3b12e-359">デリゲートと同じシグネチャを持つ @no__t 0 のメソッド。</span><span class="sxs-lookup"><span data-stu-id="3b12e-359">An `Invoke` method that has the same signature as the delegate.</span></span>

* <span data-ttu-id="3b12e-360">シグネチャがデリゲートシグネチャで、3つの違いがある `BeginInvoke` メソッド。</span><span class="sxs-lookup"><span data-stu-id="3b12e-360">A `BeginInvoke` method whose signature is the delegate signature, with three differences.</span></span> <span data-ttu-id="3b12e-361">最初に、戻り値の型が `System.IAsyncResult` に変更されます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-361">First, the return type is changed to `System.IAsyncResult`.</span></span> <span data-ttu-id="3b12e-362">次に、2つの追加のパラメーターがパラメーターリストの末尾に追加されます。1番目の型 `System.AsyncCallback` で、2番目の型 `Object` です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-362">Second, two additional parameters are added to the end of the parameter list: the first of type `System.AsyncCallback` and the second of type `Object`.</span></span> <span data-ttu-id="3b12e-363">最後に、`ByRef` のすべてのパラメーターが `ByVal` に変更されます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-363">And finally, all `ByRef` parameters are changed to be `ByVal`.</span></span>

* <span data-ttu-id="3b12e-364">戻り値の型がデリゲートと同じである @no__t 0 のメソッド。</span><span class="sxs-lookup"><span data-stu-id="3b12e-364">An `EndInvoke` method whose return type is the same as the delegate.</span></span> <span data-ttu-id="3b12e-365">メソッドのパラメーターは、デリゲートシグネチャで発生する順序と同じ順序で、@no__t 0 パラメーターであるデリゲートパラメーターのみです。</span><span class="sxs-lookup"><span data-stu-id="3b12e-365">The parameters of the method are only the delegate parameters exactly that are `ByRef` parameters, in the same order they occur in the delegate signature.</span></span>  <span data-ttu-id="3b12e-366">これらのパラメーターに加えて、パラメーターリストの最後に型 `System.IAsyncResult` の追加パラメーターがあります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-366">In addition to those parameters, there is an additional parameter of type `System.IAsyncResult` at the end of the parameter list.</span></span>

<span data-ttu-id="3b12e-367">デリゲートの定義と使用には、宣言、インスタンス化、および呼び出しという3つの手順があります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-367">There are three steps in defining and using delegates: declaration, instantiation, and invocation.</span></span>

<span data-ttu-id="3b12e-368">デリゲートは、デリゲート宣言の構文を使用して宣言されます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-368">Delegates are declared using delegate declaration syntax.</span></span> <span data-ttu-id="3b12e-369">次の例では、引数を受け取らない `SimpleDelegate` という名前のデリゲートを宣言しています。</span><span class="sxs-lookup"><span data-stu-id="3b12e-369">The following example declares a delegate named `SimpleDelegate` that takes no arguments:</span></span>

```vb
Delegate Sub SimpleDelegate()
```

<span data-ttu-id="3b12e-370">次の例では、@no__t 0 のインスタンスを作成し、そのインスタンスを直ちに呼び出します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-370">The next example creates a `SimpleDelegate` instance and then immediately calls it:</span></span>

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

<span data-ttu-id="3b12e-371">メソッドを直接呼び出す方が簡単であるため、メソッドのデリゲートをインスタンス化してから、デリゲートを使用してすぐに呼び出す点はあまりありません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-371">There is not much point in instantiating a delegate for a method and then immediately calling via the delegate, as it would be simpler to call the method directly.</span></span> <span data-ttu-id="3b12e-372">デリゲートは、その匿名性が使用されている場合にその有用性を示します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-372">Delegates show their usefulness when their anonymity is used.</span></span> <span data-ttu-id="3b12e-373">次の例は、1つの @no__t インスタンスを繰り返し呼び出す @no__t 0 のメソッドを示しています。</span><span class="sxs-lookup"><span data-stu-id="3b12e-373">The next example shows a `MultiCall` method that repeatedly calls a `SimpleDelegate` instance:</span></span>

```vb
Sub MultiCall(d As SimpleDelegate, count As Integer)
    Dim i As Integer

    For i = 0 To count - 1
        d()
    Next i
End Sub
```

<span data-ttu-id="3b12e-374">@No__t-0 のメソッドでは、@no__t のターゲットメソッドがどのようなものであるか、このメソッドにどのようなアクセシビリティがあるか、またはメソッドが @no__t であるかどうかを確認することは重要ではありません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-374">It is unimportant to the `MultiCall` method what the target method for the `SimpleDelegate` is, what accessibility this method has, or whether the method is `Shared` or not.</span></span> <span data-ttu-id="3b12e-375">重要なのは、ターゲットメソッドのシグネチャが `SimpleDelegate` と互換性があることです。</span><span class="sxs-lookup"><span data-stu-id="3b12e-375">All that matters is that the signature of the target method is compatible with `SimpleDelegate`.</span></span>


## <a name="partial-types"></a><span data-ttu-id="3b12e-376">部分型</span><span class="sxs-lookup"><span data-stu-id="3b12e-376">Partial types</span></span>

<span data-ttu-id="3b12e-377">クラス宣言と構造体宣言は、*部分*宣言にすることができます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-377">Class and structure declarations can be *partial* declarations.</span></span> <span data-ttu-id="3b12e-378">宣言内の宣言された型を部分宣言で完全に記述することはできません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-378">A partial declaration may or may not fully describe the declared type within the declaration.</span></span> <span data-ttu-id="3b12e-379">代わりに、型の宣言をプログラム内の複数の部分宣言にわたって分散させることができます。部分型は、プログラムの境界を越えて宣言することはできません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-379">Instead, the declaration of the type may be spread across multiple partial declarations within the program; partial types cannot be declared across program boundaries.</span></span> <span data-ttu-id="3b12e-380">部分型の宣言では、宣言の `Partial` 修飾子を指定します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-380">A partial type declaration specifies the `Partial` modifier on the declaration.</span></span> <span data-ttu-id="3b12e-381">次に、同じ完全修飾名を持つ型のプログラム内のその他の宣言は、コンパイル時に部分宣言と共にマージされ、単一の型宣言を形成します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-381">Then, any other declarations in the program for a type with the same fully-qualified name will be merged together with the partial declaration at compile-time to form a single type declaration.</span></span> <span data-ttu-id="3b12e-382">たとえば、次のコードでは、1つのクラス `Test`、メンバー `Test.C1`、`Test.C2` が宣言されています。</span><span class="sxs-lookup"><span data-stu-id="3b12e-382">For example, the following code declares a single class `Test` with members `Test.C1` and `Test.C2`.</span></span>

<span data-ttu-id="3b12e-383">.vb:</span><span class="sxs-lookup"><span data-stu-id="3b12e-383">a.vb:</span></span>

```vb
Public Partial Class Test
    Public Sub S1()
    End Sub
End Class
```

<span data-ttu-id="3b12e-384">b. vb:</span><span class="sxs-lookup"><span data-stu-id="3b12e-384">b.vb:</span></span>

```vb
Public Class Test
    Public Sub S2()
    End Sub
End Class
```

<span data-ttu-id="3b12e-385">部分型宣言を組み合わせる場合、少なくとも1つの宣言に @no__t 0 修飾子が必要です。それ以外の場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-385">When combining partial type declarations, at least one of the declarations must have a `Partial` modifier, otherwise a compile-time error results.</span></span>

<span data-ttu-id="3b12e-386">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="3b12e-386">__Note.__</span></span> <span data-ttu-id="3b12e-387">複数の部分宣言の中で1つの宣言に対してのみ `Partial` を指定することもできますが、すべての部分宣言で指定する方が適切な形式です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-387">Although it is possible to specify `Partial` on only one declaration among many partial declarations, it is better form to specify it on all partial declarations.</span></span> <span data-ttu-id="3b12e-388">1つの部分宣言が表示されていても、1つまたは複数の部分宣言が非表示になっている状況では (ツールによって生成されたコードを拡張する場合など)、表示されている宣言の @no__t 0 修飾子をそのままにしておくことはできますが、非表示ではありません宣言.</span><span class="sxs-lookup"><span data-stu-id="3b12e-388">In the situation where one partial declaration is visible but one or more partial declarations are hidden (such as the case of extending tool-generated code), it is acceptable to leave the `Partial` modifier off of the visible declaration but specify it on the hidden declarations.</span></span>

<span data-ttu-id="3b12e-389">部分宣言を使用して宣言できるのは、クラスと構造体だけです。</span><span class="sxs-lookup"><span data-stu-id="3b12e-389">Only classes and structures can be declared using partial declarations.</span></span> <span data-ttu-id="3b12e-390">部分宣言を照合するときに、型のアリティが考慮されます。同じ名前の2つのクラスが、型パラメーターの数が異なる場合、同じ時刻の部分宣言とは見なされません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-390">The arity of a type is considered when matching partial declarations together: two classes with the same name but different numbers of type parameters are not considered to be partial declarations of the same time.</span></span> <span data-ttu-id="3b12e-391">部分宣言では、属性、クラス修飾子、`Inherits` ステートメント、または `Implements` ステートメントを指定できます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-391">Partial declarations can specify attributes, class modifiers, `Inherits` statement or `Implements` statement.</span></span> <span data-ttu-id="3b12e-392">コンパイル時に、部分宣言のすべての部分が結合され、型宣言の一部として使用されます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-392">At compile time, all of the pieces of the partial declarations are combined together and used as a part of the type declaration.</span></span> <span data-ttu-id="3b12e-393">属性、修飾子、基底クラス、インターフェイス、または型のメンバーの間に競合がある場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-393">If there are any conflicts between attributes, modifiers, bases, interfaces, or type members, a compile-time error results.</span></span> <span data-ttu-id="3b12e-394">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-394">For example:</span></span>

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

<span data-ttu-id="3b12e-395">前の例では、-1 @no__t、`Object` から継承し、`System.IDisposable` と @no__t を実装する型 `Test1` を宣言しています。</span><span class="sxs-lookup"><span data-stu-id="3b12e-395">The previous example declares a type `Test1` that is `Public`, inherits from `Object` and implements `System.IDisposable` and `System.IComparable`.</span></span> <span data-ttu-id="3b12e-396">@No__t-0 の部分宣言では、`Test2` が `Public` であり、もう1つは `Private` である @no__t ことが示されるため、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-396">The partial declarations of `Test2` will cause a compile-time error because one of the declarations says that `Test2` is `Public` and another says that `Test2` is `Private`.</span></span>

<span data-ttu-id="3b12e-397">型パラメーターを持つ部分型では、型パラメーターの制約と分散を宣言できますが、各部分宣言の制約と分散は一致している必要があります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-397">Partial types with type parameters can declare constraints and variance for the type parameters, but the constraints and variance from each partial declaration must match.</span></span> <span data-ttu-id="3b12e-398">そのため、制約と分散は、他の修飾子と同様に自動的に結合されないという意味で特別です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-398">Thus, constraints and variance are special in that they are not automatically combined like other modifiers:</span></span>

```vb
Partial Public Class List(Of T As IEnumerable)
End Class

' Error: Constraints on T don't match
Class List(Of T As IComparable)
End Class
```

<span data-ttu-id="3b12e-399">型が複数の部分宣言を使用して宣言されているという事実は、型内の名前の参照規則には影響しません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-399">The fact that a type is declared using multiple partial declarations does not affect the name lookup rules within the type.</span></span> <span data-ttu-id="3b12e-400">その結果、部分型宣言は、他の部分型宣言で宣言されたメンバーを使用したり、他の部分型宣言で宣言されたインターフェイスにメソッドを実装したりすることができます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-400">As a result, a partial type declaration can use members declared in other partial type declarations, or may implement methods on interfaces declared in other partial type declarations.</span></span> <span data-ttu-id="3b12e-401">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-401">For example:</span></span>

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

<span data-ttu-id="3b12e-402">入れ子になった型にも、部分宣言を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-402">Nested types can have partial declarations as well.</span></span> <span data-ttu-id="3b12e-403">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-403">For example:</span></span>

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

<span data-ttu-id="3b12e-404">部分宣言内の初期化子は、宣言の順序で実行されます。ただし、個別の部分宣言で発生する初期化子の実行順序は保証されていません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-404">Initializers within a partial declaration will still be executed in declaration order; however, there is no guaranteed order of execution for initializers that occur in separate partial declarations.</span></span>

## <a name="constructed-types"></a><span data-ttu-id="3b12e-405">構築された型</span><span class="sxs-lookup"><span data-stu-id="3b12e-405">Constructed Types</span></span>

<span data-ttu-id="3b12e-406">ジェネリック型の宣言自体が型を表していません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-406">A generic type declaration, by itself, does not denote a type.</span></span> <span data-ttu-id="3b12e-407">代わりに、ジェネリック型の宣言を "ブループリント" として使用して、型引数を適用してさまざまな型を形成できます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-407">Instead, a generic type declaration can be used as a "blueprint" to form many different types by applying type arguments.</span></span> <span data-ttu-id="3b12e-408">型引数が適用されているジェネリック型は、*構築型*と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-408">A generic type that has type arguments applied to it is called a *constructed type*.</span></span> <span data-ttu-id="3b12e-409">構築された型の型引数は、一致する型パラメーターに対して設定された制約を常に満たしている必要があります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-409">The type arguments in a constructed type must always satisfy the constraints placed on the type parameters they match to.</span></span>

<span data-ttu-id="3b12e-410">型パラメーターを直接指定しない場合でも、型名は構築された型を識別することがあります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-410">A type name might identify a constructed type even though it doesn't specify type parameters directly.</span></span> <span data-ttu-id="3b12e-411">これは、ジェネリッククラス宣言内で型が入れ子になっている場合に発生する可能性があり、包含する宣言のインスタンスの型は、暗黙的に名前の参照に使用されます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-411">This can occur where a type is nested within a generic class declaration, and the instance type of the containing declaration is implicitly used for name lookup:</span></span>

```vb
Class Outer(Of T) 
    Public Class Inner 
    End Class

    ' Type of i is the constructed type Outer(Of T).Inner
    Public i As Inner 
End Class
```

<span data-ttu-id="3b12e-412">ジェネリック型とすべての型引数にアクセスできる場合は、構築された型 `C(Of T1,...,Tn)` にアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-412">A constructed type `C(Of T1,...,Tn)` is accessible when the generic type and all the type arguments are accessible.</span></span> <span data-ttu-id="3b12e-413">たとえば、ジェネリック型 `C` が @no__t で、すべての型引数 @no__t @no__t が-3 の場合、構築された型は `Public` になります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-413">For instance, if the generic type `C` is `Public` and all of the type arguments `T1,...,Tn` are `Public`, then the constructed type is `Public`.</span></span> <span data-ttu-id="3b12e-414">ただし、型名または型引数のいずれかが `Private` の場合は、構築された型のアクセシビリティが-1 @no__t ます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-414">If either the type name or one of the type arguments is `Private`, however, then the accessibility of the constructed type is `Private`.</span></span> <span data-ttu-id="3b12e-415">構築された型の1つの型引数が 0 @no__t、別の型引数が-1 @no__t 場合、構築された型は、このアセンブリ内のクラスとそのサブクラス、および `Friend` アクセスが与えられているアセンブリ内でのみアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-415">If one type argument of the constructed type is `Protected` and another type argument is `Friend`, then the constructed type is accessible only in the class and its subclasses in this assembly or any assembly that has been given `Friend` access.</span></span> <span data-ttu-id="3b12e-416">つまり、構築された型のアクセシビリティドメインは、その構成要素のアクセシビリティドメインの積集合になります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-416">In other words, the accessibility domain for a constructed type is the intersection of the accessibility domains of its constituent parts.</span></span>

<span data-ttu-id="3b12e-417">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="3b12e-417">__Note.__</span></span> <span data-ttu-id="3b12e-418">構築された型のアクセシビリティドメインは、そのた層の部分の積集合であるため、新しいアクセシビリティレベルを定義するという興味深い副作用があります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-418">The fact that the accessibility domain of constructed type is the intersection of its constituted parts has the interesting side effect of defining a new accessibility level.</span></span> <span data-ttu-id="3b12e-419">@No__t が0で @no__t が-1 である要素を含む構築された型は、`Friend`*と*`Protected` の*両方*のメンバーにアクセスできるコンテキストでのみアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-419">A constructed type that contains an element that is `Protected` and an element that is `Friend` can only be accessed in contexts that can access *both* `Friend` *and* `Protected` members.</span></span> <span data-ttu-id="3b12e-420">ただし、このアクセシビリティレベルを言語で表す方法はありません。アクセシビリティ @no__t 0 は、`Friend`*または*`Protected` のメンバーにアクセスできるコンテキストでエンティティ*にアクセスできる*ことを意味します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-420">However, there is no way to express this accessibility level in the language, as the accessibility `Protected Friend` means that an entity can be accessed in a context that can access *either* `Friend` *or* `Protected` members.</span></span>

<span data-ttu-id="3b12e-421">基本の実装されたインターフェイスと構築された型のメンバーは、ジェネリック型の型パラメーターが出現するたびに、指定された型引数を置き換えることによって決定されます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-421">The base, implemented interfaces and members of constructed types are determined by substituting the supplied type arguments for each occurrence of the type parameter in the generic type.</span></span>

### <a name="open-types-and-closed-types"></a><span data-ttu-id="3b12e-422">オープン型と閉じられた型</span><span class="sxs-lookup"><span data-stu-id="3b12e-422">Open Types and Closed Types</span></span>

<span data-ttu-id="3b12e-423">1つ以上の型引数が、包含する型またはメソッドの型パラメーターである場合、構築された型は*オープン型*と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-423">A constructed type for who one or more type arguments are type parameters of a containing type or method is called an *open type*.</span></span> <span data-ttu-id="3b12e-424">これは、型の一部の型パラメーターがまだ認識されていないため、型の実際の図形はまだ完全には認識されていないためです。</span><span class="sxs-lookup"><span data-stu-id="3b12e-424">This is because some of the type parameters of the type are still not known, so the actual shape of the type is not yet fully known.</span></span> <span data-ttu-id="3b12e-425">これに対し、型引数がすべて非型パラメーターであるジェネリック型は、 *closed 型*と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-425">In contrast, a generic type whose type arguments are all non-type parameters is called a *closed type*.</span></span> <span data-ttu-id="3b12e-426">クローズ型の形状は常に完全に認識されます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-426">The shape of a closed type is always fully known.</span></span> <span data-ttu-id="3b12e-427">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3b12e-427">For example:</span></span>

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

<span data-ttu-id="3b12e-428">構築された型 `Base(Of Integer, V)` はオープン型です。型パラメーター `T` が指定されていますが、型パラメーター `U` に別の型パラメーターが指定されています。</span><span class="sxs-lookup"><span data-stu-id="3b12e-428">The constructed type `Base(Of Integer, V)` is an open type because although the type parameter `T` has been supplied, the type parameter `U` has been supplied another type parameter.</span></span> <span data-ttu-id="3b12e-429">したがって、型の完全な形はまだ知られていません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-429">Thus, the full shape of the type is not yet known.</span></span> <span data-ttu-id="3b12e-430">ただし、構築された型 `Derived(Of Double)` ですが、継承階層のすべての型パラメーターが指定されているため、閉じられた型です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-430">The constructed type `Derived(Of Double)`, however, is a closed type because all type parameters in the inheritance hierarchy have been supplied.</span></span>

<span data-ttu-id="3b12e-431">オープン型は次のように定義されます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-431">Open types are defined as follows:</span></span>

* <span data-ttu-id="3b12e-432">型パラメーターはオープン型です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-432">A type parameter is an open type.</span></span>

* <span data-ttu-id="3b12e-433">要素型がオープン型である場合、配列型はオープン型です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-433">An array type is an open type if its element type is an open type.</span></span>

* <span data-ttu-id="3b12e-434">1つ以上の型引数がオープン型である場合、構築された型はオープン型になります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-434">A constructed type is an open type if one or more of its type arguments are an open type.</span></span>

* <span data-ttu-id="3b12e-435">閉じられた型は、オープン型ではない型です。</span><span class="sxs-lookup"><span data-stu-id="3b12e-435">A closed type is a type that is not an open type.</span></span>

<span data-ttu-id="3b12e-436">プログラムのエントリポイントをジェネリック型にすることはできないため、実行時に使用されるすべての型は閉じられた型になります。</span><span class="sxs-lookup"><span data-stu-id="3b12e-436">Because the program entry point cannot be in a generic type, all types used at run-time will be closed types.</span></span>

## <a name="special-types"></a><span data-ttu-id="3b12e-437">特殊な型</span><span class="sxs-lookup"><span data-stu-id="3b12e-437">Special Types</span></span>

<span data-ttu-id="3b12e-438">.NET Framework には、.NET Framework と Visual Basic 言語によって特別に処理される多数のクラスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="3b12e-438">The .NET Framework contains a number of classes that are treated specially by the .NET Framework and by the Visual Basic language:</span></span>

<span data-ttu-id="3b12e-439">.NET Framework 内の void 型を表す `System.Void` 型は、`GetType` 式でのみ直接参照できます。</span><span class="sxs-lookup"><span data-stu-id="3b12e-439">The type `System.Void`, which represents a void type in the .NET Framework, can be directly referenced only in `GetType` expressions.</span></span>

<span data-ttu-id="3b12e-440">@No__t-0、`System.ArgIterator`、`System.TypedReference` の型には、スタックへのポインターを含めることができます。したがって、.NET Framework ヒープには記述できません。</span><span class="sxs-lookup"><span data-stu-id="3b12e-440">The types `System.RuntimeArgumentHandle`, `System.ArgIterator` and `System.TypedReference` all can contain pointers into the stack and so cannot appear on the .NET Framework heap.</span></span> <span data-ttu-id="3b12e-441">したがって、配列の要素型、戻り値の型、フィールドの型、ジェネリック型引数、null 許容型、@no__t 0 のパラメーター型、`Object` または `System.ValueType` に変換される値の型は、`Object` また @no はのインスタンスメンバーへの呼び出しのターゲットとして使用することはできません。__ t-4、またはクロージャにリフトします。</span><span class="sxs-lookup"><span data-stu-id="3b12e-441">Therefore, they cannot be used as array element types, return types, field types, generic type arguments, nullable types, `ByRef` parameter types, the type of a value being converted to `Object` or `System.ValueType`, the target of a call to instance members of `Object` or `System.ValueType`, or lifted into a closure.</span></span>
