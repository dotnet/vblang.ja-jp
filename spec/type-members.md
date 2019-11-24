---
ms.openlocfilehash: 3d5c1e90283b6d6ec8cdeccd35e32c78f997cc27
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306041"
---
# <a name="type-members"></a><span data-ttu-id="5529b-101">型のメンバー</span><span class="sxs-lookup"><span data-stu-id="5529b-101">Type Members</span></span>

<span data-ttu-id="5529b-102">型のメンバーは、ストレージの場所と実行可能コードを定義します。</span><span class="sxs-lookup"><span data-stu-id="5529b-102">Type members define storage locations and executable code.</span></span> <span data-ttu-id="5529b-103">メソッド、コンストラクター、イベント、定数、変数、およびプロパティを指定できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-103">They can be methods, constructors, events, constants, variables, and properties.</span></span>

## <a name="interface-method-implementation"></a><span data-ttu-id="5529b-104">インターフェイスメソッドの実装</span><span class="sxs-lookup"><span data-stu-id="5529b-104">Interface Method Implementation</span></span>

<span data-ttu-id="5529b-105">メソッド、イベント、およびプロパティは、インターフェイスメンバーを実装できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-105">Methods, events, and properties can implement interface members.</span></span> <span data-ttu-id="5529b-106">インターフェイスメンバーを実装するには、メンバー宣言で `Implements` キーワードを指定し、1つまたは複数のインターフェイスメンバーを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="5529b-106">To implement an interface member, a member declaration specifies the `Implements` keyword and lists one or more interface members.</span></span>

```antlr
ImplementsClause
    : ( 'Implements' ImplementsList )?
    ;

ImplementsList
    : InterfaceMemberSpecifier ( Comma InterfaceMemberSpecifier )*
    ;

InterfaceMemberSpecifier
    : NonArrayTypeName Period IdentifierOrKeyword
    ;
```

<span data-ttu-id="5529b-107">インターフェイスメンバーを実装するメソッドとプロパティは、`MustOverride`、`Overridable`、または別のメンバーをオーバーライドするように宣言されている場合を除き、暗黙的に `NotOverridable` ます。</span><span class="sxs-lookup"><span data-stu-id="5529b-107">Methods and properties that implement interface members are implicitly `NotOverridable` unless declared to be `MustOverride`, `Overridable`, or overriding another member.</span></span> <span data-ttu-id="5529b-108">インターフェイスメンバーを実装するメンバーが `Shared`されると、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="5529b-108">It is an error for a member implementing an interface member to be `Shared`.</span></span> <span data-ttu-id="5529b-109">メンバーのアクセシビリティは、インターフェイスメンバーを実装する機能には影響しません。</span><span class="sxs-lookup"><span data-stu-id="5529b-109">A member's accessibility has no effect on its ability to implement interface members.</span></span>

<span data-ttu-id="5529b-110">インターフェイスの実装を有効にするには、含んでいる型の実装リストが、互換性のあるメンバーを含むインターフェイスに名前を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-110">For an interface implementation to be valid, the implements list of the containing type must name an interface that contains a compatible member.</span></span> <span data-ttu-id="5529b-111">互換性のあるメンバーとは、実装するメンバーのシグネチャと一致するシグネチャを持つメンバーのことです。</span><span class="sxs-lookup"><span data-stu-id="5529b-111">A compatible member is one whose signature matches the signature of the implementing member.</span></span> <span data-ttu-id="5529b-112">ジェネリックインターフェイスが実装されている場合は、Implements 句に指定された型引数が、互換性をチェックするときにシグネチャに置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="5529b-112">If a generic interface is being implemented, then the type argument supplied in the Implements clause is substituted into the signature when checking compatibility.</span></span> <span data-ttu-id="5529b-113">例 :</span><span class="sxs-lookup"><span data-stu-id="5529b-113">For example:</span></span>

```vb
Interface I1(Of T)
    Sub F(x As T)
End Interface

Class C1
    Implements I1(Of Integer)

    Sub F(x As Integer) Implements I1(Of Integer).F
    End Sub
End Class

Class C2(Of U)
    Implements I1(Of U)

    Sub F(x As U) Implements I1(Of U).F
    End Sub
End Class
```

<span data-ttu-id="5529b-114">デリゲート型を使用して宣言されたイベントがインターフェイスイベントを実装している場合、互換性のあるイベントとは、基になるデリゲート型が同じ型であるイベントのことです。</span><span class="sxs-lookup"><span data-stu-id="5529b-114">If an event declared using a delegate type is implementing an interface event, then a compatible event is one whose underlying delegate type is the same type.</span></span> <span data-ttu-id="5529b-115">それ以外の場合、イベントは、実装しているインターフェイスイベントのデリゲート型を使用します。</span><span class="sxs-lookup"><span data-stu-id="5529b-115">Otherwise, the event uses the delegate type from the interface event it is implementing.</span></span> <span data-ttu-id="5529b-116">このようなイベントに複数のインターフェイスイベントが実装されている場合、すべてのインターフェイスイベントの基になるデリゲート型が同じである必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-116">If such an event implements multiple interface events, all the interface events must have the same underlying delegate type.</span></span> <span data-ttu-id="5529b-117">例 :</span><span class="sxs-lookup"><span data-stu-id="5529b-117">For example:</span></span>

```vb
Interface ClickEvents
    Event LeftClick(x As Integer, y As Integer)
    Event RightClick(x As Integer, y As Integer)
End Interface

Class Button
    Implements ClickEvents

    ' OK. Signatures match, delegate type = ClickEvents.LeftClickHandler.
    Event LeftClick(x As Integer, y As Integer) _
        Implements ClickEvents.LeftClick

    ' OK. Signatures match, delegate type = ClickEvents.RightClickHandler.
    Event RightClick(x As Integer, y As Integer) _
        Implements ClickEvents.RightClick
End Class

Class Label
    Implements ClickEvents

    ' Error. Signatures match, but can't be both delegate types.
    Event Click(x As Integer, y As Integer) _
        Implements ClickEvents.LeftClick, ClickEvents.RightClick
End Class
```

<span data-ttu-id="5529b-118">Implements リストのインターフェイスメンバーは、型名、ピリオド、および識別子を使用して指定されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-118">An interface member in the implements list is specified using a type name, a period, and an identifier.</span></span> <span data-ttu-id="5529b-119">型名は、implements リスト内のインターフェイス、または implements リスト内のインターフェイスの基本インターフェイスである必要があります。また、識別子は、指定されたインターフェイスのメンバーである必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-119">The type name must be an interface in the implements list or a base interface of an interface in the implements list, and the identifier must be a member of the specified interface.</span></span> <span data-ttu-id="5529b-120">1つのメンバーは、複数の一致するインターフェイスメンバーを実装できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-120">A single member can implement more than one matching interface member.</span></span>

```vb
Interface ILeft
    Sub F()
End Interface

Interface IRight
    Sub F()
End Interface

Class Test
    Implements ILeft, IRight

    Sub F() Implements ILeft.F, IRight.F
    End Sub
End Class
```

<span data-ttu-id="5529b-121">実装するインターフェイスメンバーが、複数のインターフェイスの継承によって明示的に実装されたすべてのインターフェイスで使用できない場合は、そのメンバーが使用可能な基本インターフェイスを実装するメンバーが明示的に参照する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-121">If the interface member being implemented is unavailable in all explicitly implemented interfaces because of multiple interface inheritance, the implementing member must explicitly reference a base interface on which the member is available.</span></span> <span data-ttu-id="5529b-122">たとえば、`I1` と `I2` にメンバー `M`が含まれており、`I3` が `I1` と `I2`を継承している場合、`I3` を実装する型は `I1.M` と `I2.M`を実装します。</span><span class="sxs-lookup"><span data-stu-id="5529b-122">For example, if `I1` and `I2` contain a member `M`, and `I3` inherits from `I1` and `I2`, a type implementing `I3` will implement `I1.M` and `I2.M`.</span></span> <span data-ttu-id="5529b-123">インターフェイスのシャドウが継承されたメンバーを乗算する場合、実装する型は、継承されたメンバーとそのメンバーをシャドウするメンバーを実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-123">If an interface shadows multiply inherited members, an implementing type will have to implement the inherited members and the member(s) shadowing them.</span></span>

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

Class Test
    Implements ILeftRight

    Sub LeftF() Implements ILeft.F
    End Sub

    Sub RightF() Implements IRight.F
    End Sub

    Sub LeftRightF() Implements ILeftRight.F
    End Sub
End Class
```

<span data-ttu-id="5529b-124">インターフェイスメンバーのコンテナーインターフェイスを実装する場合は、実装するインターフェイスと同じ型引数を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-124">If the containing interface of the interface member be implemented is generic, the same type arguments as the interface being implements must be supplied.</span></span> <span data-ttu-id="5529b-125">例 :</span><span class="sxs-lookup"><span data-stu-id="5529b-125">For example:</span></span>

```vb
Interface I1(Of T)
    Function F() As T
End Interface

Class C1
    Implements I1(Of Integer)
    Implements I1(Of Double)

    Function F1() As Integer Implements I1(Of Integer).F
    End Function

    Function F2() As Double Implements I1(Of Double).F
    End Function

    ' Error: I1(Of String) is not implemented by C1
    Function F3() As String Implements I1(Of String).F
    End Function
End Class

Class C2(Of U)
    Implements I1(Of U)

    Function F() As U Implements I1(Of U).F
    End Function
End Class
```


## <a name="methods"></a><span data-ttu-id="5529b-126">メソッド</span><span class="sxs-lookup"><span data-stu-id="5529b-126">Methods</span></span>

<span data-ttu-id="5529b-127">メソッドには、プログラムの実行可能なステートメントが含まれています。</span><span class="sxs-lookup"><span data-stu-id="5529b-127">Methods contain the executable statements of a program.</span></span>

```antlr
MethodMemberDeclaration
    : MethodDeclaration
    | ExternalMethodDeclaration
    ;

InterfaceMethodMemberDeclaration
    : InterfaceMethodDeclaration
    ;

MethodDeclaration
    : SubDeclaration
    | MustOverrideSubDeclaration
    | FunctionDeclaration
    | MustOverrideFunctionDeclaration
    ;

InterfaceMethodDeclaration
    : InterfaceSubDeclaration
    | InterfaceFunctionDeclaration
    ;

SubSignature
    : 'Sub' Identifier TypeParameterList?
      ( OpenParenthesis ParameterList? CloseParenthesis )?
    ;

FunctionSignature
    : 'Function' Identifier TypeParameterList?
      ( OpenParenthesis ParameterList? CloseParenthesis )?
      ( 'As' Attributes? TypeName )?
    ;

SubDeclaration
    : Attributes? ProcedureModifier* SubSignature
      HandlesOrImplements? LineTerminator
      Block
      'End' 'Sub' StatementTerminator
    ;

MustOverrideSubDeclaration
    : Attributes? MustOverrideProcedureModifier+ SubSignature
      HandlesOrImplements? StatementTerminator
    ;

InterfaceSubDeclaration
    : Attributes? InterfaceProcedureModifier* SubSignature StatementTerminator
    ;

FunctionDeclaration
    : Attributes? ProcedureModifier* FunctionSignature
      HandlesOrImplements? LineTerminator
      Block
      'End' 'Function' StatementTerminator
    ;

MustOverrideFunctionDeclaration
    : Attributes? MustOverrideProcedureModifier+ FunctionSignature
      HandlesOrImplements? StatementTerminator
    ;

InterfaceFunctionDeclaration
    : Attributes? InterfaceProcedureModifier* FunctionSignature StatementTerminator
    ;

ProcedureModifier
    : AccessModifier | 'Shadows' | 'Shared' | 'Overridable' | 'NotOverridable' | 'Overrides'
    | 'Overloads' | 'Partial' | 'Iterator' | 'Async'
    ;

MustOverrideProcedureModifier
    : ProcedureModifier
    | 'MustOverride'
    ;

InterfaceProcedureModifier
    : 'Shadows' | 'Overloads'
    ;

HandlesOrImplements
    : HandlesClause
    | ImplementsClause
    ;
```

<span data-ttu-id="5529b-128">省略可能なパラメーターのリストと戻り値 (省略可能) を持つメソッドは、共有されているか、共有されていません。</span><span class="sxs-lookup"><span data-stu-id="5529b-128">Methods, which have an optional list of parameters and an optional return value, are either shared or not shared.</span></span> <span data-ttu-id="5529b-129">共有メソッドには、クラスまたはクラスのインスタンスを使用してアクセスします。</span><span class="sxs-lookup"><span data-stu-id="5529b-129">Shared methods are accessed through the class or instances of the class.</span></span> <span data-ttu-id="5529b-130">非共有メソッド (インスタンスメソッドとも呼ばれます) には、クラスのインスタンスを介してアクセスします。</span><span class="sxs-lookup"><span data-stu-id="5529b-130">Non-shared methods, also called instance methods, are accessed through instances of the class.</span></span> <span data-ttu-id="5529b-131">次の例は、いくつかの共有メソッド (`Clone` と `Flip`) といくつかのインスタンスメソッド (`Push`、`Pop`、および `ToString`) を持つ `Stack` クラスを示しています。</span><span class="sxs-lookup"><span data-stu-id="5529b-131">The following example shows a class `Stack` that has several shared methods (`Clone` and `Flip`), and several instance methods (`Push`, `Pop`, and `ToString`):</span></span>

```vb
Public Class Stack
    Public Shared Function Clone(s As Stack) As Stack
        ...
    End Function

    Public Shared Function Flip(s As Stack) As Stack
        ...
    End Function

    Public Function Pop() As Object
        ...
    End Function

    Public Sub Push(o As Object)
        ...
    End Sub 

    Public Overrides Function ToString() As String
        ...
    End Function 
End Class 

Module Test
    Sub Main()
        Dim s As Stack = New Stack()
        Dim i As Integer

        While i < 10
            s.Push(i)
        End While

        Dim flipped As Stack = Stack.Flip(s)
        Dim cloned As Stack = Stack.Clone(s)

        Console.WriteLine("Original stack: " & s.ToString())
        Console.WriteLine("Flipped stack: " & flipped.ToString())
        Console.WriteLine("Cloned stack: " & cloned.ToString())
    End Sub
End Module
```

<span data-ttu-id="5529b-132">メソッドはオーバーロードすることができます。つまり、複数のメソッドが一意のシグネチャを持つ限り、同じ名前を持つことができます。</span><span class="sxs-lookup"><span data-stu-id="5529b-132">Methods can be overloaded, which means that multiple methods may have the same name so long as they have unique signatures.</span></span> <span data-ttu-id="5529b-133">メソッドのシグネチャは、パラメーターの数と型で構成されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-133">The signature of a method consists of the number and types of its parameters.</span></span> <span data-ttu-id="5529b-134">メソッドのシグネチャには、戻り値の型やパラメーター修飾子 (省略可能、ByRef、ParamArray など) は含まれません。</span><span class="sxs-lookup"><span data-stu-id="5529b-134">The signature of a method specifically does not include the return type or parameter modifiers such as Optional, ByRef or ParamArray.</span></span> <span data-ttu-id="5529b-135">次の例は、多数のオーバーロードを持つクラスを示しています。</span><span class="sxs-lookup"><span data-stu-id="5529b-135">The following example shows a class with a number of overloads:</span></span>

```vb
Module Test
    Sub F()
        Console.WriteLine("F()")
    End Sub 

    Sub F(o As Object)
        Console.WriteLine("F(Object)")
    End Sub

    Sub F(value As Integer)
        Console.WriteLine("F(Integer)")
    End Sub 

    Sub F(a As Integer, b As Integer)
        Console.WriteLine("F(Integer, Integer)")
    End Sub 

    Sub F(values() As Integer)
        Console.WriteLine("F(Integer())")
    End Sub 

    Sub G(s As String, Optional s2 As String = 5)
        Console.WriteLine("G(String, Optional String")
    End Sub

    Sub G(s As String)
        Console.WriteLine("G(String)")
    End Sub


    Sub Main()
        F()
        F(1)
        F(CType(1, Object))
        F(1, 2)
        F(New Integer() { 1, 2, 3 })
        G("hello")
        G("hello", "world")
    End Sub
End Module
```

<span data-ttu-id="5529b-136">プログラムの出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="5529b-136">The output of the program is:</span></span>

```console
F()
F(Integer)
F(Object)
F(Integer, Integer)
F(Integer())
G(String)
G(String, Optional String)
```

<span data-ttu-id="5529b-137">省略可能なパラメーターのみが異なるオーバーロードは、ライブラリの "バージョン管理" に使用できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-137">Overloads that differ only in optional parameters can be used for "versioning" of libraries.</span></span> <span data-ttu-id="5529b-138">たとえば、ライブラリの v1 には、省略可能なパラメーターを持つ関数が含まれる場合があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-138">For instance, v1 of a library might include a function with optional parameters:</span></span>

```vb
Sub fopen(fileName As String, Optional accessMode as Integer = 0)
```

<span data-ttu-id="5529b-139">次に、ライブラリの v2 は、もう1つの省略可能なパラメーター "password" を追加する必要があります。この場合、ソースの互換性を損なうことなく (つまり、v1 をターゲットとするアプリケーションを再コンパイルできます)、バイナリ互換性を損なうことなく、参照 v1 は、再コンパイルなしで v2 を参照できるようになりました)。</span><span class="sxs-lookup"><span data-stu-id="5529b-139">Then v2 of the library wants to add another optional parameter "password", and it wants to do so without breaking source compatibility (so applications that used to target v1 can be recompiled), and without breaking binary compatibility (so applications that used to reference v1 can now reference v2 without recompilation).</span></span> <span data-ttu-id="5529b-140">V2 は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="5529b-140">This is how v2 will look:</span></span>

```vb
Sub fopen(file As String, mode as Integer)
Sub fopen(file As String, Optional mode as Integer = 0, Optional pword As String = "")
```

<span data-ttu-id="5529b-141">パブリック API の省略可能なパラメーターは CLS に準拠していないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="5529b-141">Note that optional parameters in a public API are not CLS-compliant.</span></span> <span data-ttu-id="5529b-142">ただし、少なくとも Visual Basic と C# 4 およびF#で使用できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-142">However, they can be consumed at least by Visual Basic and C#4 and F#.</span></span>



### <a name="regular-async-and-iterator-method-declarations"></a><span data-ttu-id="5529b-143">標準、非同期、および反復子メソッドの宣言</span><span class="sxs-lookup"><span data-stu-id="5529b-143">Regular, Async and Iterator Method Declarations</span></span>

<span data-ttu-id="5529b-144">2種類のメソッドがあります。値を返さない*サブルーチン*と、関数を実行する*関数*です。</span><span class="sxs-lookup"><span data-stu-id="5529b-144">There are two types of methods: *subroutines*, which do not return values, and *functions*, which do.</span></span> <span data-ttu-id="5529b-145">メソッドがインターフェイスで定義されているか、または `MustOverride` 修飾子を持っている場合にのみ、メソッドの本体および `End` コンストラクトを省略できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-145">The body and `End` construct of a method may only be omitted if the method is defined in an interface or has the `MustOverride` modifier.</span></span> <span data-ttu-id="5529b-146">関数に戻り値の型が指定されておらず、厳密なセマンティクスが使用されている場合、コンパイル時エラーが発生します。それ以外の場合、型は暗黙的に `Object` か、またはメソッドの型文字の型になります。</span><span class="sxs-lookup"><span data-stu-id="5529b-146">If no return type is specified on a function and strict semantics are being used, a compile-time error occurs; otherwise the type is implicitly `Object` or the type of the method's type character.</span></span> <span data-ttu-id="5529b-147">メソッドの戻り値の型とパラメーターの型のアクセシビリティドメインは、またはメソッド自体のアクセシビリティドメインのスーパーセットと同じである必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-147">The accessibility domain of the return type and parameter types of a method must be the same as or a superset of the accessibility domain of the method itself.</span></span>

<span data-ttu-id="5529b-148">__通常のメソッド__は、`Async` でも `Iterator` 修飾子でもありません。</span><span class="sxs-lookup"><span data-stu-id="5529b-148">A __regular method__ is one with neither `Async` nor `Iterator` modifiers.</span></span> <span data-ttu-id="5529b-149">これは、サブルーチンまたは関数である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-149">It may be a subroutine or a function.</span></span> <span data-ttu-id="5529b-150">セクションの[通常のメソッド](statements.md#regular-methods)では、通常のメソッドが呼び出されたときの動作について詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="5529b-150">Section [Regular Methods](statements.md#regular-methods) details what happens when a regular method is invoked.</span></span>

<span data-ttu-id="5529b-151">__Iterator メソッド__は、`Iterator` の修飾子を持ち、`Async` 修飾子を持たないメソッドです。</span><span class="sxs-lookup"><span data-stu-id="5529b-151">An __iterator method__ is one with the `Iterator` modifier and no `Async` modifier.</span></span> <span data-ttu-id="5529b-152">これは関数である必要があり、その戻り値の型は、一部の `T`に対して `IEnumerator`、`IEnumerable`、または `IEnumerator(Of T)` または `IEnumerable(Of T)` である必要があります。また、`ByRef` パラメーターを持つことはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-152">It must be a function, and its return type must be `IEnumerator`, `IEnumerable`, or `IEnumerator(Of T)` or `IEnumerable(Of T)` for some `T`, and it must have no `ByRef` parameters.</span></span> <span data-ttu-id="5529b-153">「[反復子メソッド](statements.md#iterator-methods)」セクションでは、反復子メソッドが呼び出されたときの動作について詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="5529b-153">Section [Iterator Methods](statements.md#iterator-methods) details what happens when an iterator method is invoked.</span></span>

<span data-ttu-id="5529b-154">__非同期メソッド__は、`Async` 修飾子を持ち、`Iterator` 修飾子はありません。</span><span class="sxs-lookup"><span data-stu-id="5529b-154">An __async method__ is one with the `Async` modifier and no `Iterator` modifier.</span></span> <span data-ttu-id="5529b-155">これは、サブルーチン、または一部の `T`に対して戻り値の型が `Task` または `Task(Of T)` の関数である必要があります。また、`ByRef` パラメーターは使用できません。</span><span class="sxs-lookup"><span data-stu-id="5529b-155">It must be either a subroutine, or a function with return type `Task` or `Task(Of T)` for some `T`, and must have no `ByRef` parameters.</span></span> <span data-ttu-id="5529b-156">「[非同期メソッド](statements.md#async-methods)」セクションでは、非同期メソッドが呼び出されたときの動作について詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="5529b-156">Section [Async Methods](statements.md#async-methods) details what happens when an async method is invoked.</span></span>

<span data-ttu-id="5529b-157">メソッドがこれら3種類のメソッドのいずれでもない場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="5529b-157">It is a compile-time error if a method is not one of these three kinds of method.</span></span>

<span data-ttu-id="5529b-158">サブルーチンと関数の宣言は、先頭と末尾のステートメントが論理行の先頭から開始する必要があることに特に意味があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-158">Subroutine and function declarations are special in that their beginning and end statements must each start at the beginning of a logical line.</span></span> <span data-ttu-id="5529b-159">また、`MustOverride` 以外のサブルーチンまたは関数宣言の本体は、論理行の先頭から始める必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-159">Additionally, the body of a non-`MustOverride` subroutine or function declaration must start at the beginning of a logical line.</span></span> <span data-ttu-id="5529b-160">例 :</span><span class="sxs-lookup"><span data-stu-id="5529b-160">For example:</span></span>

```vb
Module Test
    ' Illegal: Subroutine doesn't start the line
    Public x As Integer : Sub F() : End Sub

    ' Illegal: First statement doesn't start the line
    Sub G() : Console.WriteLine("G")
    End Sub

    ' Illegal: End Sub doesn't start the line
    Sub H() : End Sub
End Module
```


### <a name="external-method-declarations"></a><span data-ttu-id="5529b-161">外部メソッドの宣言</span><span class="sxs-lookup"><span data-stu-id="5529b-161">External Method Declarations</span></span>

<span data-ttu-id="5529b-162">外部メソッドの宣言には、プログラムの外部で実装が提供される新しいメソッドが導入されています。</span><span class="sxs-lookup"><span data-stu-id="5529b-162">An external method declaration introduces a new method whose implementation is provided external to the program.</span></span>

```antlr
ExternalMethodDeclaration
    : ExternalSubDeclaration
    | ExternalFunctionDeclaration
    ;

ExternalSubDeclaration
    : Attributes? ExternalMethodModifier* 'Declare' CharsetModifier? 'Sub'
      Identifier LibraryClause AliasClause?
      ( OpenParenthesis ParameterList? CloseParenthesis )? StatementTerminator
    ;

ExternalFunctionDeclaration
    : Attributes? ExternalMethodModifier* 'Declare' CharsetModifier? 'Function'
      Identifier LibraryClause AliasClause?
      ( OpenParenthesis ParameterList? CloseParenthesis )?
      ( 'As' Attributes? TypeName )?
      StatementTerminator
    ;

ExternalMethodModifier
    : AccessModifier
    | 'Shadows'
    | 'Overloads'
    ;

CharsetModifier
    : 'Ansi' | 'Unicode' | 'Auto'
    ;

LibraryClause
    : 'Lib' StringLiteral
    ;

AliasClause
    : 'Alias' StringLiteral
    ;
```

<span data-ttu-id="5529b-163">外部メソッドの宣言には実際の実装がないため、メソッド本体も `End` コンストラクトもありません。</span><span class="sxs-lookup"><span data-stu-id="5529b-163">Because an external method declaration provides no actual implementation, it has no method body or `End` construct.</span></span> <span data-ttu-id="5529b-164">外部メソッドは暗黙的に共有され、型パラメーターを持つことはできません。また、イベントを処理したり、インターフェイスメンバーを実装したりすることはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-164">External methods are implicitly shared, may not have type parameters, and may not handle events or implement interface members.</span></span> <span data-ttu-id="5529b-165">関数に戻り値の型が指定されておらず、厳密なセマンティクスが使用されている場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="5529b-165">If no return type is specified on a function and strict semantics are being used, a compile-time error occurs.</span></span> <span data-ttu-id="5529b-166">それ以外の場合、型は暗黙的に `Object` か、またはメソッドの型文字の型になります。</span><span class="sxs-lookup"><span data-stu-id="5529b-166">Otherwise the type is implicitly `Object` or the type of the method's type character.</span></span> <span data-ttu-id="5529b-167">外部メソッドの戻り値の型とパラメーターの型のアクセシビリティドメインは、外部メソッド自体のアクセシビリティドメインのスーパーセットと同じである必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-167">The accessibility domain of the return type and parameter types of an external method must be the same as or a superset of the accessibility domain of the external method itself.</span></span>

<span data-ttu-id="5529b-168">外部メソッドの宣言の library 句は、メソッドを実装する外部ファイルの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="5529b-168">The library clause of an external method declaration specifies the name of the external file that implements the method.</span></span> <span data-ttu-id="5529b-169">省略可能な alias 句は、外部ファイル内の数値の序数 (プレフィックスとして `#` 文字) またはメソッドの名前を指定する文字列です。</span><span class="sxs-lookup"><span data-stu-id="5529b-169">The optional alias clause is a string that specifies the numeric ordinal (prefixed by a `#` character) or name of the method in the external file.</span></span> <span data-ttu-id="5529b-170">1文字セット修飾子を指定することもできます。これは、外部メソッドの呼び出し中に文字列をマーシャリングするために使用される文字セットを制御します。</span><span class="sxs-lookup"><span data-stu-id="5529b-170">A single-character set modifier may also be specified, which governs the character set used to marshal strings during a call to the external method.</span></span> <span data-ttu-id="5529b-171">`Unicode` 修飾子は、すべての文字列を Unicode 値にマーシャリングし、`Ansi` 修飾子はすべての文字列を ANSI 値にマーシャリングします。また、`Auto` 修飾子は、メソッドの名前に基づいて .NET Framework 規則に従って文字列をマーシャリングします。または、指定されている場合はエイリアス名をマーシャリングします。</span><span class="sxs-lookup"><span data-stu-id="5529b-171">The `Unicode` modifier marshals all strings to Unicode values, the `Ansi` modifier marshals all strings to ANSI values, and the `Auto` modifier marshals the strings according to .NET Framework rules based on the name of the method, or the alias name if specified.</span></span> <span data-ttu-id="5529b-172">修飾子が指定されていない場合、既定値は `Ansi`です。</span><span class="sxs-lookup"><span data-stu-id="5529b-172">If no modifier is specified, the default is `Ansi`.</span></span>

<span data-ttu-id="5529b-173">`Ansi` または `Unicode` が指定されている場合、メソッド名は外部ファイルでは変更されずに検索されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-173">If `Ansi` or `Unicode` is specified, then the method name is looked up in the external file with no modification.</span></span> <span data-ttu-id="5529b-174">`Auto` が指定されている場合、メソッド名の参照はプラットフォームに依存します。</span><span class="sxs-lookup"><span data-stu-id="5529b-174">If `Auto` is specified, then method name lookup depends on the platform.</span></span> <span data-ttu-id="5529b-175">プラットフォームが ANSI (たとえば、Windows 95、Windows 98、Windows ME) と見なされた場合、メソッド名は変更されずに検索されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-175">If the platform is considered to be ANSI (for example, Windows 95, Windows 98, Windows ME), then the method name is looked up with no modification.</span></span> <span data-ttu-id="5529b-176">参照が失敗した場合は、`A` が追加され、参照が再試行されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-176">If the lookup fails, an `A` is appended and the lookup tried again.</span></span> <span data-ttu-id="5529b-177">プラットフォームが Unicode (Windows NT、Windows 2000、Windows XP など) と見なされた場合、`W` が追加され、名前が検索されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-177">If the platform is considered to be Unicode (for example, Windows NT, Windows 2000, Windows XP), then a `W` is appended and the name is looked up.</span></span> <span data-ttu-id="5529b-178">参照が失敗した場合は、`W`なしで参照が再試行されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-178">If the lookup fails, the lookup is tried again without the `W`.</span></span> <span data-ttu-id="5529b-179">例 :</span><span class="sxs-lookup"><span data-stu-id="5529b-179">For example:</span></span>

```vb
Module Test
    ' All platforms bind to "ExternSub".
    Declare Ansi Sub ExternSub Lib "ExternDLL" ()

    ' All platforms bind to "ExternSub".
    Declare Unicode Sub ExternSub Lib "ExternDLL" ()

    ' ANSI platforms: bind to "ExternSub" then "ExternSubA".
    ' Unicode platforms: bind to "ExternSubW" then "ExternSub".
    Declare Auto Sub ExternSub Lib "ExternDLL" ()
End Module
```

<span data-ttu-id="5529b-180">外部メソッドに渡されるデータ型は、.NET Framework のデータマーシャリング規則に従って、1つの例外でマーシャリングされます。</span><span class="sxs-lookup"><span data-stu-id="5529b-180">Data types being passed to external methods are marshaled according to the .NET Framework data marshalling conventions with one exception.</span></span> <span data-ttu-id="5529b-181">値によって渡される文字列変数 (つまり `ByVal x As String`) は OLE オートメーション BSTR 型にマーシャリングされ、外部メソッドで BSTR に加えられた変更は文字列引数に戻されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-181">String variables that are passed by value (that is, `ByVal x As String`) are marshaled to the OLE Automation BSTR type, and changes made to the BSTR in the external method are reflected back in the string argument.</span></span> <span data-ttu-id="5529b-182">これは、外部メソッド内の `String` 型は変更可能であるため、この特殊なマーシャリングはその動作を模倣しているためです。</span><span class="sxs-lookup"><span data-stu-id="5529b-182">This is because the type `String` in external methods is mutable, and this special marshalling mimics that behavior.</span></span> <span data-ttu-id="5529b-183">参照によって渡される文字列パラメーター (つまり `ByRef x As String`) は、OLE オートメーション BSTR 型へのポインターとしてマーシャリングされます。</span><span class="sxs-lookup"><span data-stu-id="5529b-183">String parameters that are passed by reference (i.e. `ByRef x As String`) are marshaled as a pointer to the OLE Automation BSTR type.</span></span> <span data-ttu-id="5529b-184">パラメーターに `System.Runtime.InteropServices.MarshalAsAttribute` 属性を指定することで、これらの特殊な動作をオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="5529b-184">It is possible to override these special behaviors by specifying the `System.Runtime.InteropServices.MarshalAsAttribute` attribute on the parameter.</span></span>

<span data-ttu-id="5529b-185">外部メソッドの使用例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="5529b-185">The example demonstrates use of external methods:</span></span>

```vb
Class Path
    Declare Function CreateDirectory Lib "kernel32" ( _
        Name As String, sa As SecurityAttributes) As Boolean
    Declare Function RemoveDirectory Lib "kernel32" ( _
        Name As String) As Boolean
    Declare Function GetCurrentDirectory Lib "kernel32" ( _
        BufSize As Integer, Buf As String) As Integer
    Declare Function SetCurrentDirectory Lib "kernel32" ( _
        Name As String) As Boolean
End Class
```


### <a name="overridable-methods"></a><span data-ttu-id="5529b-186">オーバーライド可能なメソッド</span><span class="sxs-lookup"><span data-stu-id="5529b-186">Overridable Methods</span></span>

<span data-ttu-id="5529b-187">`Overridable` 修飾子は、メソッドがオーバーライド可能であることを示します。</span><span class="sxs-lookup"><span data-stu-id="5529b-187">The `Overridable` modifier indicates that a method is overridable.</span></span> <span data-ttu-id="5529b-188">`Overrides` 修飾子は、メソッドが、同じシグネチャを持つ基本型のオーバーライド可能なメソッドをオーバーライドすることを示します。</span><span class="sxs-lookup"><span data-stu-id="5529b-188">The `Overrides` modifier indicates that a method overrides a base-type overridable method that has the same signature.</span></span> <span data-ttu-id="5529b-189">`NotOverridable` 修飾子は、オーバーライド可能なメソッドをさらにオーバーライドできないことを示します。</span><span class="sxs-lookup"><span data-stu-id="5529b-189">The `NotOverridable` modifier indicates that an overridable method cannot be further overridden.</span></span> <span data-ttu-id="5529b-190">`MustOverride` 修飾子は、派生クラスでメソッドをオーバーライドする必要があることを示します。</span><span class="sxs-lookup"><span data-stu-id="5529b-190">The `MustOverride` modifier indicates that a method must be overridden in derived classes.</span></span>

<span data-ttu-id="5529b-191">これらの修飾子の特定の組み合わせは無効です。</span><span class="sxs-lookup"><span data-stu-id="5529b-191">Certain combinations of these modifiers are not valid:</span></span>

* <span data-ttu-id="5529b-192">`Overridable` と `NotOverridable` は相互に排他的であるため、組み合わせることはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-192">`Overridable` and `NotOverridable` are mutually exclusive and cannot be combined.</span></span>

* <span data-ttu-id="5529b-193">`MustOverride` は `Overridable` を意味します (そのため、指定することはできません)。また、`NotOverridable`と組み合わせることはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-193">`MustOverride` implies `Overridable` (and so cannot specify it) and cannot be combined with `NotOverridable`.</span></span>

* <span data-ttu-id="5529b-194">`NotOverridable` を `Overridable` または `MustOverride` と組み合わせることはできません。また、`Overrides`と組み合わせて使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-194">`NotOverridable` cannot be combined with `Overridable` or `MustOverride` and must be combined with `Overrides`.</span></span>

* <span data-ttu-id="5529b-195">`Overrides` は `Overridable` を意味します (そのため、指定することはできません)。また、`MustOverride`と組み合わせることはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-195">`Overrides` implies `Overridable` (and so cannot specify it) and cannot be combined with `MustOverride`.</span></span>

<span data-ttu-id="5529b-196">また、オーバーライド可能なメソッドにも追加の制限があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-196">There are also additional restrictions on overridable methods:</span></span>

* <span data-ttu-id="5529b-197">`MustOverride` メソッドは、メソッド本体または `End` コンストラクトを含むことはできません。別のメソッドをオーバーライドすることはできません。また、`MustInherit` クラスでのみ表示できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-197">A `MustOverride` method may not include a method body or an `End` construct, may not override another method, and may only appear in `MustInherit` classes.</span></span>

* <span data-ttu-id="5529b-198">メソッドで `Overrides` が指定されていて、オーバーライドする対応する基本メソッドが存在しない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="5529b-198">If a method specifies `Overrides` and there is no matching base method to override, a compile-time error occurs.</span></span> <span data-ttu-id="5529b-199">オーバーライドするメソッドで `Shadows`を指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-199">An overriding method may not specify `Shadows`.</span></span>

* <span data-ttu-id="5529b-200">オーバーライドするメソッドのアクセシビリティドメインがオーバーライドされるメソッドのアクセシビリティドメインと等しくない場合、メソッドは別のメソッドをオーバーライドすることはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-200">A method may not override another method if the overriding method's accessibility domain is not equal to the accessibility domain of the method being overridden.</span></span> <span data-ttu-id="5529b-201">1つの例外として、`Friend` アクセス権を持たない別のアセンブリの `Protected Friend` メソッドをオーバーライドするメソッドは `Protected` (`Protected Friend`ではなく) を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-201">The one exception is that a method overriding a `Protected Friend` method in another assembly that does not have `Friend` access must specify `Protected` (not `Protected Friend`).</span></span>

* <span data-ttu-id="5529b-202">`Private` メソッドは、`Overridable`、`NotOverridable`、`MustOverride`、または他のメソッドをオーバーライドすることはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-202">`Private` methods may not be `Overridable`, `NotOverridable`, or `MustOverride`, nor may they override other methods.</span></span>

* <span data-ttu-id="5529b-203">`NotInheritable` クラスのメソッドが `Overridable` または `MustOverride`として宣言されていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-203">Methods in `NotInheritable` classes may not be declared `Overridable` or `MustOverride`.</span></span>

<span data-ttu-id="5529b-204">次の例は、overridable メソッドと overridable 以外のメソッドの違いを示しています。</span><span class="sxs-lookup"><span data-stu-id="5529b-204">The following example illustrates the differences between overridable and nonoverridable methods:</span></span>

```vb
Class Base
    Public Sub F()
        Console.WriteLine("Base.F")
    End Sub

    Public Overridable Sub G()
        Console.WriteLine("Base.G")
    End Sub
End Class

Class Derived
    Inherits Base

    Public Shadows Sub F()
        Console.WriteLine("Derived.F")
    End Sub

    Public Overrides Sub G()
        Console.WriteLine("Derived.G")
    End Sub
End Class

Module Test
    Sub Main()
        Dim d As Derived = New Derived()
        Dim b As Base = d

        b.F()
        d.F()
        b.G()
        d.G()
    End Sub
End Module
```

<span data-ttu-id="5529b-205">この例では、クラス `Base` に、メソッド `F` と `Overridable` メソッド `G`が導入されています。</span><span class="sxs-lookup"><span data-stu-id="5529b-205">In the example, class `Base` introduces a method `F` and an `Overridable` method `G`.</span></span> <span data-ttu-id="5529b-206">クラス `Derived` は、新しいメソッド `F`を導入し、継承された `F`をシャドウします。また、継承されたメソッド `G`もオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="5529b-206">The class `Derived` introduces a new method `F`, thus shadowing the inherited `F`, and also overrides the inherited method `G`.</span></span> <span data-ttu-id="5529b-207">この例では次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-207">The example produces the following output:</span></span>

```console
Base.F
Derived.F
Derived.G
Derived.G
```

<span data-ttu-id="5529b-208">ステートメント `b.G()` が `Base.G`ではなく `Derived.G`を呼び出すことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="5529b-208">Notice that the statement `b.G()` invokes `Derived.G`, not `Base.G`.</span></span> <span data-ttu-id="5529b-209">これは、インスタンス (`Base`) のコンパイル時の型ではなく、インスタンスの実行時の型 (`Derived`) が、呼び出す実際のメソッドの実装を決定するためです。</span><span class="sxs-lookup"><span data-stu-id="5529b-209">This is because the run-time type of the instance (which is `Derived`) rather than the compile-time type of the instance (which is `Base`) determines the actual method implementation to invoke.</span></span>

### <a name="shared-methods"></a><span data-ttu-id="5529b-210">共有メソッド</span><span class="sxs-lookup"><span data-stu-id="5529b-210">Shared Methods</span></span>

<span data-ttu-id="5529b-211">`Shared` 修飾子は、メソッドが*共有メソッド*であることを示します。</span><span class="sxs-lookup"><span data-stu-id="5529b-211">The `Shared` modifier indicates a method is a *shared method*.</span></span> <span data-ttu-id="5529b-212">共有メソッドは、型の特定のインスタンスでは動作せず、型の特定のインスタンスを使用するのではなく、型から直接呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="5529b-212">A shared method does not operate on a specific instance of a type and may be invoked directly from a type rather than through a particular instance of a type.</span></span> <span data-ttu-id="5529b-213">ただし、インスタンスを使用して共有メソッドを修飾することは有効です。</span><span class="sxs-lookup"><span data-stu-id="5529b-213">It is valid, however, to use an instance to qualify a shared method.</span></span> <span data-ttu-id="5529b-214">共有メソッドで `Me`、`MyClass`、または `MyBase` を参照することはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-214">It is invalid to refer to `Me`, `MyClass`, or `MyBase` in a shared method.</span></span> <span data-ttu-id="5529b-215">共有メソッドを `Overridable`、`NotOverridable`、または `MustOverride`にすることはできません。また、メソッドをオーバーライドすることもできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-215">Shared methods may not be `Overridable`, `NotOverridable`, or `MustOverride`, and they may not override methods.</span></span> <span data-ttu-id="5529b-216">標準モジュールおよびインターフェイスで定義されているメソッドは、暗黙的に `Shared` されているため、`Shared`を指定できません。</span><span class="sxs-lookup"><span data-stu-id="5529b-216">Methods defined in standard modules and interfaces may not specify `Shared`, because they are implicitly `Shared` already.</span></span>

<span data-ttu-id="5529b-217">`Shared` 修飾子のない構造体またはクラスで宣言されたメソッドは、*インスタンスメソッド*です。</span><span class="sxs-lookup"><span data-stu-id="5529b-217">A method declared in a structure or class without a `Shared` modifier is an *instance method*.</span></span> <span data-ttu-id="5529b-218">インスタンスメソッドは、型の特定のインスタンスに対して動作します。</span><span class="sxs-lookup"><span data-stu-id="5529b-218">An instance method operates on a given instance of a type.</span></span> <span data-ttu-id="5529b-219">インスタンスメソッドは、型のインスタンスを介してのみ呼び出すことができ、`Me` 式を通じてインスタンスを参照することができます。</span><span class="sxs-lookup"><span data-stu-id="5529b-219">Instance methods can only be invoked through an instance of a type and may refer to the instance through the `Me` expression.</span></span>

<span data-ttu-id="5529b-220">次の例は、共有メンバーとインスタンスメンバーにアクセスするための規則を示しています。</span><span class="sxs-lookup"><span data-stu-id="5529b-220">The following example illustrates the rules for accessing shared and instance members:</span></span>

```vb
Class Test
    Private x As Integer
    Private Shared y As Integer

    Sub F()
        x = 1 ' Ok, same as Me.x = 1.
        y = 1 ' Ok, same as Test.y = 1.
    End Sub

    Shared Sub G()
        x = 1 ' Error, cannot access Me.x.
        y = 1 ' Ok, same as Test.y = 1.
    End Sub

    Shared Sub Main()
        Dim t As Test = New Test()

        t.x = 1 ' Ok.
        t.y = 1 ' Ok.
        Test.x = 1 ' Error, cannot access instance member through type.
        Test.y = 1 ' Ok.
    End Sub
End Class
```

<span data-ttu-id="5529b-221">メソッド `F` は、インスタンス関数メンバーで識別子を使用してインスタンスメンバーと共有メンバーの両方にアクセスできることを示しています。</span><span class="sxs-lookup"><span data-stu-id="5529b-221">Method `F` shows that in an instance function member, an identifier can be used to access both instance members and shared members.</span></span> <span data-ttu-id="5529b-222">メソッド `G` は、共有関数メンバーで、識別子を使用してインスタンスメンバーにアクセスする際にエラーになることを示しています。</span><span class="sxs-lookup"><span data-stu-id="5529b-222">Method `G` shows that in a shared function member, it is an error to access an instance member through an identifier.</span></span> <span data-ttu-id="5529b-223">メソッド `Main` は、メンバーアクセス式でインスタンスメンバーにはインスタンスを介してアクセスする必要があることを示していますが、共有メンバーは、型またはインスタンスを使用してアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="5529b-223">Method `Main` shows that in a member access expression, instance members must be accessed through instances, but shared members can be accessed through types or instances.</span></span>

### <a name="method-parameters"></a><span data-ttu-id="5529b-224">メソッド パラメーター</span><span class="sxs-lookup"><span data-stu-id="5529b-224">Method Parameters</span></span>

<span data-ttu-id="5529b-225">*パラメーター*は、メソッドとの間で情報を渡すために使用できる変数です。</span><span class="sxs-lookup"><span data-stu-id="5529b-225">A *parameter* is a variable that can be used to pass information into and out of a method.</span></span> <span data-ttu-id="5529b-226">メソッドのパラメーターは、メソッドのパラメーターリストによって宣言されます。これは、コンマで区切られた1つ以上のパラメーターで構成されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-226">Parameters of a method are declared by the method's parameter list, which consists of one or more parameters separated by commas.</span></span>

```antlr
ParameterList
    : Parameter ( Comma Parameter )*
    ;

Parameter
    : Attributes? ParameterModifier* ParameterIdentifier ( 'As' TypeName )?
      ( Equals ConstantExpression )?
    ;

ParameterModifier
    : 'ByVal' | 'ByRef' | 'Optional' | 'ParamArray'
    ;

ParameterIdentifier
    : Identifier IdentifierModifiers
    ;
```

<span data-ttu-id="5529b-227">パラメーターに型が指定されておらず、厳密なセマンティクスが使用されている場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="5529b-227">If no type is specified for a parameter and strict semantics are used, a compile-time error occurs.</span></span> <span data-ttu-id="5529b-228">それ以外の場合、既定の型は `Object` か、パラメーターの型文字の型になります。</span><span class="sxs-lookup"><span data-stu-id="5529b-228">Otherwise the default type is `Object` or the type of the parameter's type character.</span></span> <span data-ttu-id="5529b-229">寛容なセマンティクスでも、1つのパラメーターに `As` 句が含まれている場合、すべてのパラメーターで型を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-229">Even under permissive semantics, if one parameter includes an `As` clause, all parameters must specify types.</span></span>

<span data-ttu-id="5529b-230">パラメーターは、それぞれ、修飾子 `ByVal`、`ByRef`、`Optional`、および `ParamArray`によって value、reference、optional、または paramarray パラメーターとして指定されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-230">Parameters are specified as value, reference, optional, or paramarray parameters by the modifiers `ByVal`, `ByRef`, `Optional`, and `ParamArray`, respectively.</span></span> <span data-ttu-id="5529b-231">`ByRef` または `ByVal` が指定されていないパラメーターは、既定 `ByVal`に設定されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-231">A parameter that does not specify `ByRef` or `ByVal` defaults to `ByVal`.</span></span>

<span data-ttu-id="5529b-232">パラメーター名は、メソッドの本体全体にスコープが設定され、常にパブリックにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="5529b-232">Parameter names are scoped to the entire body of the method and are always publicly accessible.</span></span> <span data-ttu-id="5529b-233">メソッドの呼び出しでは、その呼び出しに固有のコピーがパラメーターによって作成され、呼び出しの引数リストによって、新しく作成されたパラメーターに値または変数参照が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="5529b-233">A method invocation creates a copy, specific to that invocation, of the parameters, and the argument list of the invocation assigns values or variable references to the newly created parameters.</span></span> <span data-ttu-id="5529b-234">外部メソッドの宣言とデリゲート宣言には本体がないため、パラメーターリストで重複するパラメーター名を使用できますが、これは推奨されません。</span><span class="sxs-lookup"><span data-stu-id="5529b-234">Because external method declarations and delegate declarations have no body, duplicate parameter names are allowed in parameter lists, but discouraged.</span></span>

<span data-ttu-id="5529b-235">識別子の後に null 許容型の名前修飾子を指定することによって、null 値を許容するかどうかを示すことができます。また、配列が配列であることを示すために配列の名前修飾子によっても `?` ます。</span><span class="sxs-lookup"><span data-stu-id="5529b-235">The identifier may be followed by the nullable name modifier `?` to indicate that it is nullable, and also by array name modifiers to indicate that it is an array.</span></span> <span data-ttu-id="5529b-236">たとえば、"`ByVal x?() As Integer`" のように組み合わせて使用できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-236">They may be combined, e.g. "`ByVal x?() As Integer`".</span></span> <span data-ttu-id="5529b-237">明示的な配列の境界を使用することはできません。また、nullable name 修飾子が指定されている場合は、`As` 句が存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-237">It is not allowed to use explicit array bounds; also, if the nullable name modifier is present then an `As` clause must be present.</span></span>


#### <a name="value-parameters"></a><span data-ttu-id="5529b-238">値パラメーター</span><span class="sxs-lookup"><span data-stu-id="5529b-238">Value Parameters</span></span>

<span data-ttu-id="5529b-239">*値パラメーター*は、明示的な `ByVal` 修飾子で宣言されています。</span><span class="sxs-lookup"><span data-stu-id="5529b-239">A *value parameter* is declared with an explicit `ByVal` modifier.</span></span> <span data-ttu-id="5529b-240">`ByVal` 修飾子が使用されている場合、`ByRef` 修飾子を指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-240">If the `ByVal` modifier is used, the `ByRef` modifier may not be specified.</span></span> <span data-ttu-id="5529b-241">値パラメーターは、パラメーターが属しているメンバーの呼び出しと共に存在し、呼び出しで指定された引数の値で初期化されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-241">A value parameter comes into existence with the invocation of the member the parameter belongs to, and is initialized with the value of the argument given in the invocation.</span></span> <span data-ttu-id="5529b-242">値パラメーターは、メンバーが返されたときに存在しなくなります。</span><span class="sxs-lookup"><span data-stu-id="5529b-242">A value parameter ceases to exist upon return of the member.</span></span>

<span data-ttu-id="5529b-243">メソッドは、値パラメーターに新しい値を割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="5529b-243">A method is permitted to assign new values to a value parameter.</span></span> <span data-ttu-id="5529b-244">このような割り当ては、値パラメーターによって表されるローカルストレージの場所にのみ影響します。メソッドの呼び出しで指定された実際の引数には影響しません。</span><span class="sxs-lookup"><span data-stu-id="5529b-244">Such assignments only affect the local storage location represented by the value parameter; they have no effect on the actual argument given in the method invocation.</span></span>

<span data-ttu-id="5529b-245">値パラメーターは、引数の値がメソッドに渡されるときに使用されます。パラメーターを変更しても、元の引数には影響しません。</span><span class="sxs-lookup"><span data-stu-id="5529b-245">A value parameter is used when the value of an argument is passed into a method, and modifications of the parameter do not impact the original argument.</span></span> <span data-ttu-id="5529b-246">値パラメーターは、対応する引数の変数とは別の変数である、独自の変数を参照します。</span><span class="sxs-lookup"><span data-stu-id="5529b-246">A value parameter refers to its own variable, one that is distinct from the variable of the corresponding argument.</span></span> <span data-ttu-id="5529b-247">この変数は、対応する引数の値をコピーすることによって初期化されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-247">This variable is initialized by copying the value of the corresponding argument.</span></span> <span data-ttu-id="5529b-248">次の例は、`p`という名前の値パラメーターを持つメソッド `F` を示しています。</span><span class="sxs-lookup"><span data-stu-id="5529b-248">The following example shows a method `F` that has a value parameter named `p`:</span></span>

```vb
Module Test
    Sub F(p As Integer)
        Console.WriteLine("p = " & p)
        p += 1
    End Sub 

    Sub Main()
        Dim a As Integer = 1

        Console.WriteLine("pre: a = " & a)
        F(a)
        Console.WriteLine("post: a = " & a)
    End Sub
End Module
```

<span data-ttu-id="5529b-249">この例では、値パラメーター `p` が変更されても、次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-249">The example produces the following output, even though the value parameter `p` is modified:</span></span>

```console
pre: a = 1
p = 1
post: a = 1
```

#### <a name="reference-parameters"></a><span data-ttu-id="5529b-250">参照パラメーター</span><span class="sxs-lookup"><span data-stu-id="5529b-250">Reference Parameters</span></span>

<span data-ttu-id="5529b-251">参照パラメーターは、`ByRef` 修飾子で宣言されたパラメーターです。</span><span class="sxs-lookup"><span data-stu-id="5529b-251">A reference parameter is a parameter declared with a `ByRef` modifier.</span></span> <span data-ttu-id="5529b-252">`ByRef` 修飾子が指定されている場合、`ByVal` 修飾子は使用できません。</span><span class="sxs-lookup"><span data-stu-id="5529b-252">If the `ByRef` modifier is specified, the `ByVal` modifier may not be used.</span></span> <span data-ttu-id="5529b-253">参照パラメーターでは、新しいストレージの場所は作成されません。</span><span class="sxs-lookup"><span data-stu-id="5529b-253">A reference parameter does not create a new storage location.</span></span> <span data-ttu-id="5529b-254">代わりに、参照パラメーターは、メソッドまたはコンストラクターの呼び出しで引数として指定された変数を表します。</span><span class="sxs-lookup"><span data-stu-id="5529b-254">Instead, a reference parameter represents the variable given as the argument in the method or constructor invocation.</span></span> <span data-ttu-id="5529b-255">概念的には、参照パラメーターの値は、基になる変数と常に同じです。</span><span class="sxs-lookup"><span data-stu-id="5529b-255">Conceptually, the value of a reference parameter is always the same as the underlying variable.</span></span>

<span data-ttu-id="5529b-256">参照パラメーターは、*エイリアス*として、または*コピーインコピーバック*を使用して、2つのモードで動作します。</span><span class="sxs-lookup"><span data-stu-id="5529b-256">Reference parameters act in two modes, either as *aliases* or through *copy-in copy-back.*</span></span>

<span data-ttu-id="5529b-257">__エイリアス.__</span><span class="sxs-lookup"><span data-stu-id="5529b-257">__Aliases.__</span></span> <span data-ttu-id="5529b-258">参照パラメーターは、パラメーターが、呼び出し元が指定した引数のエイリアスとして機能する場合に使用されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-258">A reference parameter is used when the parameter acts as an alias for a caller-provided argument.</span></span> <span data-ttu-id="5529b-259">参照パラメーターは、それ自体は変数を定義するのではなく、対応する引数の変数を参照します。</span><span class="sxs-lookup"><span data-stu-id="5529b-259">A reference parameter does not itself define a variable, but rather refers to the variable of the corresponding argument.</span></span> <span data-ttu-id="5529b-260">参照パラメーターを直接変更し、対応する引数に直ちに影響を与えます。</span><span class="sxs-lookup"><span data-stu-id="5529b-260">Modifications of a reference parameter directly and immediately impact the corresponding argument.</span></span> <span data-ttu-id="5529b-261">次の例は、2つの参照パラメーターを持つ `Swap` メソッドを示しています。</span><span class="sxs-lookup"><span data-stu-id="5529b-261">The following example shows a method `Swap` that has two reference parameters:</span></span>

```vb
Module Test
    Sub Swap(ByRef a As Integer, ByRef b As Integer)
        Dim t As Integer = a
        a = b
        b = t
    End Sub 

    Sub Main()
        Dim x As Integer = 1
        Dim y As Integer = 2

        Console.WriteLine("pre: x = " & x & ", y = " & y)
        Swap(x, y)
        Console.WriteLine("post: x = " & x & ", y = " & y)
    End Sub 
End Module
```

<span data-ttu-id="5529b-262">プログラムの出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="5529b-262">The output of the program is:</span></span>

```console
pre: x = 1, y = 2
post: x = 2, y = 1
```

<span data-ttu-id="5529b-263">クラス `Main`でメソッド `Swap` を呼び出す場合、`a` は `x,` を表し、`b` は `y`を表します。</span><span class="sxs-lookup"><span data-stu-id="5529b-263">For the invocation of method `Swap` in class `Main`, `a` represents `x,` and `b` represents `y`.</span></span> <span data-ttu-id="5529b-264">このため、呼び出しは `x` と `y`の値をスワップする効果を持ちます。</span><span class="sxs-lookup"><span data-stu-id="5529b-264">Thus, the invocation has the effect of swapping the values of `x` and `y`.</span></span>

<span data-ttu-id="5529b-265">参照パラメーターを受け取るメソッドでは、複数の名前が同じストレージの場所を表すことができます。</span><span class="sxs-lookup"><span data-stu-id="5529b-265">In a method that takes reference parameters, it is possible for multiple names to represent the same storage location:</span></span>

```vb
Module Test
    Private s As String

    Sub F(ByRef a As String, ByRef b As String)
        s = "One"
        a = "Two"
        b = "Three"
    End Sub

    Sub G()
        F(s, s)
    End Sub
End Module
```

<span data-ttu-id="5529b-266">この例では、`G` でメソッド `F` を呼び出すと、`a` と `b`の両方に対して `s` への参照が渡されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-266">In the example the invocation of method `F` in `G` passes a reference to `s` for both `a` and `b`.</span></span> <span data-ttu-id="5529b-267">そのため、この呼び出しでは、`s`、`a`、および `b` の名前はすべて同じストレージの場所を参照し、3つの割り当てはすべてインスタンス変数 `s`を変更します。</span><span class="sxs-lookup"><span data-stu-id="5529b-267">Thus, for that invocation, the names `s`, `a`, and `b` all refer to the same storage location, and the three assignments all modify the instance variable `s`.</span></span>

<span data-ttu-id="5529b-268">__コピーインコピーバック。__</span><span class="sxs-lookup"><span data-stu-id="5529b-268">__Copy-in copy-back.__</span></span> <span data-ttu-id="5529b-269">参照パラメーターに渡される変数の型が参照パラメーターの型と互換性がない場合、または非変数 (プロパティなど) が参照パラメーターに引数として渡された場合、または呼び出しが遅延バインディングの場合は、一時的な変数が割り当てられ、参照パラメーターに渡されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-269">If the type of the variable being passed to a reference parameter is not compatible with the reference parameter's type, or if a non-variable (e.g. a property) is passed as an argument to a reference parameter, or if the invocation is late-bound, then a temporary variable is allocated and passed to the reference parameter.</span></span> <span data-ttu-id="5529b-270">渡された値は、メソッドが呼び出される前にこの一時変数にコピーされ、メソッドから制御が戻ったときに、元の変数 (存在する場合は、書き込み可能な場合) にコピーされます。</span><span class="sxs-lookup"><span data-stu-id="5529b-270">The value being passed in will be copied into this temporary variable before the method is invoked and will be copied back to the original variable (if there is one and if it's writable) when the method returns.</span></span> <span data-ttu-id="5529b-271">このため、参照パラメーターには、渡される変数の正確な格納場所への参照が含まれるとは限りません。また、メソッドが終了するまで、参照パラメーターに対する変更は変数に反映されない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-271">Thus, a reference parameter may not necessarily contain a reference to the exact storage of the variable being passed in, and any changes to the reference parameter may not be reflected in the variable until the method exits.</span></span> <span data-ttu-id="5529b-272">例 :</span><span class="sxs-lookup"><span data-stu-id="5529b-272">For example:</span></span>

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class

Module Test
    Sub F(ByRef b As Base)
        b = New Base()
    End Sub

    Property G() As Base
        Get
        End Get
        Set
        End Set
    End Property

    Sub Main()
        Dim d As Derived

        F(G)   ' OK.
        F(d)   ' Throws System.InvalidCastException after F returns.
    End Sub
End Module
```

<span data-ttu-id="5529b-273">`F`の最初の呼び出しの場合は、一時変数が作成され、プロパティ `G` の値が割り当てられ `F`に渡されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-273">In the case of the first invocation of `F`, a temporary variable is created and the value of the property `G` is assigned to it and passed into `F`.</span></span> <span data-ttu-id="5529b-274">`F`から戻ると、一時変数の値が `G`のプロパティに割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="5529b-274">Upon return from `F`, the value in the temporary variable is assigned back to the property of `G`.</span></span> <span data-ttu-id="5529b-275">2番目のケースでは、別の一時変数が作成され、`d` の値が割り当てられ、`F`に渡されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-275">In the second case, another temporary variable is created and the value of `d` is assigned to it and passed into `F`.</span></span> <span data-ttu-id="5529b-276">`F`から戻ると、一時変数の値が変数の型にキャストされ `Derived`、`d`に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="5529b-276">When returning from `F`, the value in the temporary variable is cast back to the type of the variable, `Derived`, and assigned to `d`.</span></span> <span data-ttu-id="5529b-277">返される値は `Derived`にキャストできないため、実行時に例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="5529b-277">Since the value being passed back cannot be cast to `Derived`, an exception is thrown at run time.</span></span>

#### <a name="optional-parameters"></a><span data-ttu-id="5529b-278">省略可能なパラメーター</span><span class="sxs-lookup"><span data-stu-id="5529b-278">Optional Parameters</span></span>

<span data-ttu-id="5529b-279">省略可能なパラメーターが `Optional` 修飾子を使用して宣言されています。</span><span class="sxs-lookup"><span data-stu-id="5529b-279">An optional parameter is declared with the `Optional` modifier.</span></span> <span data-ttu-id="5529b-280">仮パラメーターリスト内の省略可能なパラメーターの後に続くパラメーターは、省略可能である必要があります。次のパラメーターで `Optional` 修飾子を指定しないと、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="5529b-280">Parameters that follow an optional parameter in the formal parameter list must be optional as well; failure to specify the `Optional` modifier on the following parameters will trigger a compile-time error.</span></span> <span data-ttu-id="5529b-281">引数が指定されていない場合は、null 許容型 `T?` 型、または null 非許容型 `T` の省略可能なパラメーターを既定値として使用するには `e` 定数式を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-281">An optional parameter of some type nullable type `T?` or non-nullable type `T` must specify a constant expression `e` to be used as a default value if no argument is specified.</span></span> <span data-ttu-id="5529b-282">`e` が Object 型の `Nothing` に評価される場合、パラメーターの*型*の既定値がパラメーターの既定値として使用されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-282">If `e` evaluates to `Nothing` of type Object, then the default value of the *parameter type* will be used as the default for the parameter.</span></span> <span data-ttu-id="5529b-283">それ以外の場合、`CType(e, T)` は定数式である必要があり、パラメーターの既定値として取得されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-283">Otherwise, `CType(e, T)` must be a constant expression and it is taken as the default for the parameter.</span></span>

<span data-ttu-id="5529b-284">パラメーターの初期化子が有効である唯一の状況は、省略可能なパラメーターです。</span><span class="sxs-lookup"><span data-stu-id="5529b-284">Optional parameters are the only situation in which an initializer on a parameter is valid.</span></span> <span data-ttu-id="5529b-285">初期化は常に、メソッド本体内ではなく、呼び出し式の一部として実行されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-285">The initialization is always done as a part of the invocation expression, not within the method body itself.</span></span>

```vb
Module Test
    Sub F(x As Integer, Optional y As Integer = 20)
        Console.WriteLine("x = " & x & ", y = " & y)
    End Sub

    Sub Main()
        F(10)
        F(30,40)
    End Sub
End Module
```

<span data-ttu-id="5529b-286">プログラムの出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="5529b-286">The output of the program is:</span></span>

```console
x = 10, y = 20
x = 30, y = 40
```

<span data-ttu-id="5529b-287">省略可能なパラメーターは、デリゲートまたはイベント宣言では指定できません。また、ラムダ式でも指定できません。</span><span class="sxs-lookup"><span data-stu-id="5529b-287">Optional parameters may not be specified in delegate or event declarations, nor in lambda expressions.</span></span>

#### <a name="paramarray-parameters"></a><span data-ttu-id="5529b-288">ParamArray パラメーター</span><span class="sxs-lookup"><span data-stu-id="5529b-288">ParamArray Parameters</span></span>

<span data-ttu-id="5529b-289">`ParamArray` パラメーターは、`ParamArray` 修飾子を使用して宣言されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-289">`ParamArray` parameters are declared with the `ParamArray` modifier.</span></span> <span data-ttu-id="5529b-290">`ParamArray` 修飾子が存在する場合は、`ByVal` 修飾子を指定する必要があり、他のパラメーターでは `ParamArray` 修飾子を使用できません。</span><span class="sxs-lookup"><span data-stu-id="5529b-290">If the `ParamArray` modifier is present, the `ByVal` modifier must be specified, and no other parameter may use the `ParamArray` modifier.</span></span> <span data-ttu-id="5529b-291">`ParamArray` パラメーターの型は1次元配列でなければなりません。また、パラメーターリストの最後のパラメーターである必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-291">The `ParamArray` parameter's type must be a one-dimensional array, and it must be the last parameter in the parameter list.</span></span>

<span data-ttu-id="5529b-292">`ParamArray` パラメーターは、`ParamArray`の型のパラメーターの不確定数を表します。</span><span class="sxs-lookup"><span data-stu-id="5529b-292">A `ParamArray` parameter represents an indeterminate number of parameters of the type of the `ParamArray`.</span></span> <span data-ttu-id="5529b-293">メソッド自体では、`ParamArray` パラメーターは宣言された型として扱われ、特別なセマンティクスはありません。</span><span class="sxs-lookup"><span data-stu-id="5529b-293">Within the method itself, a `ParamArray` parameter is treated as its declared type and has no special semantics.</span></span> <span data-ttu-id="5529b-294">`ParamArray` パラメーターは暗黙的に省略可能であり、既定値は `ParamArray`の型の空の1次元配列です。</span><span class="sxs-lookup"><span data-stu-id="5529b-294">A `ParamArray` parameter is implicitly optional, with a default value of an empty one-dimensional array of the type of the `ParamArray`.</span></span>

<span data-ttu-id="5529b-295">`ParamArray` では、メソッド呼び出しの2つの方法のいずれかで引数を指定できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-295">A `ParamArray` permits arguments to be specified in one of two ways in a method invocation:</span></span>

* <span data-ttu-id="5529b-296">`ParamArray` に指定する引数には、`ParamArray` 型に拡大変換される型の1つの式を指定できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-296">The argument given for a `ParamArray` can be a single expression of a type that widens to the `ParamArray` type.</span></span> <span data-ttu-id="5529b-297">この場合、`ParamArray` は、値パラメーターとまったく同様に動作します。</span><span class="sxs-lookup"><span data-stu-id="5529b-297">In this case, the `ParamArray` acts precisely like a value parameter.</span></span>

* <span data-ttu-id="5529b-298">または、呼び出しで `ParamArray`に0個以上の引数を指定できます。各引数は、`ParamArray`の要素型に暗黙的に変換できる型の式です。</span><span class="sxs-lookup"><span data-stu-id="5529b-298">Alternatively, the invocation can specify zero or more arguments for the `ParamArray`, where each argument is an expression of a type that is implicitly convertible to the element type of the `ParamArray`.</span></span> <span data-ttu-id="5529b-299">この場合、呼び出しは、引数の数に対応する長さの `ParamArray` 型のインスタンスを作成し、指定された引数値を使用して配列インスタンスの要素を初期化し、新しく作成された配列インスタンスを実引数として使用します。</span><span class="sxs-lookup"><span data-stu-id="5529b-299">In this case, the invocation creates an instance of the `ParamArray` type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="5529b-300">呼び出しで可変個の引数を許可する場合を除き、次の例に示すように、`ParamArray` は同じ型の値パラメーターとまったく同じ意味を持ちます。</span><span class="sxs-lookup"><span data-stu-id="5529b-300">Except for allowing a variable number of arguments in an invocation, a `ParamArray` is precisely equivalent to a value parameter of the same type, as the following example illustrates.</span></span>

```vb
Module Test
    Sub F(ParamArray args() As Integer)
        Dim i As Integer

        Console.Write("Array contains " & args.Length & " elements:")
        For Each i In args
            Console.Write(" " & i)
        Next i
        Console.WriteLine()
    End Sub

    Sub Main()
        Dim a As Integer() = { 1, 2, 3 }

        F(a)
        F(10, 20, 30, 40)
        F()
    End Sub
End Module
```

<span data-ttu-id="5529b-301">この例では、出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-301">The example produces the output</span></span>

```console
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

<span data-ttu-id="5529b-302">`F` の最初の呼び出しでは、単に配列 `a` を値パラメーターとして渡します。</span><span class="sxs-lookup"><span data-stu-id="5529b-302">The first invocation of `F` simply passes the array `a` as a value parameter.</span></span> <span data-ttu-id="5529b-303">2番目の `F` の呼び出しでは、指定された要素の値を持つ4要素の配列が自動的に作成され、その配列インスタンスが値パラメーターとして渡されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-303">The second invocation of `F` automatically creates a four-element array with the given element values and passes that array instance as a value parameter.</span></span> <span data-ttu-id="5529b-304">同様に、`F` の3回目の呼び出しでは、ゼロ要素の配列を作成し、そのインスタンスを値パラメーターとして渡します。</span><span class="sxs-lookup"><span data-stu-id="5529b-304">Likewise, the third invocation of `F` creates a zero-element array and passes that instance as a value parameter.</span></span> <span data-ttu-id="5529b-305">2番目と3番目の呼び出しは、書き込みとまったく同じです。</span><span class="sxs-lookup"><span data-stu-id="5529b-305">The second and third invocations are precisely equivalent to writing:</span></span>

```vb
F(New Integer() {10, 20, 30, 40})
F(New Integer() {})
```

<span data-ttu-id="5529b-306">デリゲートまたはイベント宣言で `ParamArray` パラメーターを指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-306">`ParamArray` parameters may not be specified in delegate or event declarations.</span></span>

### <a name="event-handling"></a><span data-ttu-id="5529b-307">イベント処理</span><span class="sxs-lookup"><span data-stu-id="5529b-307">Event Handling</span></span>

<span data-ttu-id="5529b-308">メソッドは、インスタンスまたは共有変数のオブジェクトによって生成されたイベントを宣言によって処理できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-308">Methods can declaratively handle events raised by objects in instance or shared variables.</span></span> <span data-ttu-id="5529b-309">イベントを処理するには、メソッドの宣言で `Handles` キーワードを指定し、1つまたは複数のイベントを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="5529b-309">To handle events, a method declaration specifies the `Handles` keyword and lists one or more events.</span></span>

```antlr
HandlesClause
    : ( 'Handles' EventHandlesList )?
    ;

EventHandlesList
    : EventMemberSpecifier ( Comma EventMemberSpecifier )*
    ;

EventMemberSpecifier
    : Identifier Period IdentifierOrKeyword
    | 'MyBase' Period IdentifierOrKeyword
    | 'MyClass' Period IdentifierOrKeyword
    | 'Me' Period IdentifierOrKeyword
    ;
```

<span data-ttu-id="5529b-310">`Handles` リストのイベントは、ピリオドで区切られた2つの識別子によって指定されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-310">An event in the `Handles` list is specified by two identifiers separated by a period:</span></span>

* <span data-ttu-id="5529b-311">最初の識別子は、`WithEvents` 修飾子、`MyBase` または `MyClass` または `Me` キーワードを指定する、含んでいる型のインスタンスまたは共有変数である必要があります。それ以外の場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="5529b-311">The first identifier must be an instance or shared variable in the containing type that specifies the `WithEvents` modifier or the `MyBase` or `MyClass` or `Me` keyword; otherwise, a compile-time error occurs.</span></span> <span data-ttu-id="5529b-312">この変数には、このメソッドによって処理されるイベントを発生させるオブジェクトが格納されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-312">This variable contains the object that will raise the events handled by this method.</span></span>

* <span data-ttu-id="5529b-313">2番目の識別子では、最初の識別子の型のメンバーを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-313">The second identifier must specify a member of the type of the first identifier.</span></span> <span data-ttu-id="5529b-314">メンバーはイベントである必要があり、共有されている可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-314">The member must be an event, and may be shared.</span></span> <span data-ttu-id="5529b-315">最初の識別子に共有変数が指定されている場合は、イベントを共有するか、エラーの結果を生成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-315">If a shared variable is specified for the first identifier, then the event must be shared, or an error results.</span></span>

<span data-ttu-id="5529b-316">ハンドラーメソッド `M` は、ステートメント `AddHandler E, AddressOf M` も有効である場合 `E` イベントの有効なイベントハンドラーと見なされます。</span><span class="sxs-lookup"><span data-stu-id="5529b-316">A handler method `M` is considered a valid event handler for an event `E` if the statement `AddHandler E, AddressOf M` would also be valid.</span></span> <span data-ttu-id="5529b-317">ただし、`AddHandler` ステートメントとは異なり、明示的なイベントハンドラーを使用すると、厳密なセマンティクスが使用されているかどうかに関係なく、引数のないメソッドでイベントを処理できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-317">Unlike an `AddHandler` statement, however, explicit event handlers allow handling an event with a method with no arguments regardless of whether strict semantics are being used or not:</span></span>

```vb
Option Strict On

Class C1
    Event E(x As Integer)
End Class

Class C2
    withEvents C1 As New C1()

    ' Valid
    Sub M1() Handles C1.E
    End Sub

    Sub M2()
        ' Invalid
        AddHandler C1.E, AddressOf M1
    End Sub
End Class
```

<span data-ttu-id="5529b-318">1つのメンバーが複数の一致するイベントを処理でき、複数のメソッドが1つのイベントを処理する場合があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-318">A single member can handle multiple matching events, and multiple methods may handle a single event.</span></span> <span data-ttu-id="5529b-319">メソッドのアクセシビリティは、イベントを処理する機能には影響しません。</span><span class="sxs-lookup"><span data-stu-id="5529b-319">A method's accessibility has no effect on its ability to handle events.</span></span> <span data-ttu-id="5529b-320">次の例は、メソッドがイベントを処理する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="5529b-320">The following example shows how a method can handle events:</span></span>

```vb
Class Raiser
    Event E1()

    Sub Raise()
        RaiseEvent E1
    End Sub
End Class

Module Test
    WithEvents x As Raiser

    Sub E1Handler() Handles x.E1
        Console.WriteLine("Raised")
    End Sub

    Sub Main()
        x = New Raiser()
        x.Raise()
        x.Raise()
    End Sub
End Module
```

<span data-ttu-id="5529b-321">次のように出力されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-321">This will print out:</span></span>

```console
Raised
Raised
```

<span data-ttu-id="5529b-322">型は、その基本型によって提供されるすべてのイベントハンドラーを継承します。</span><span class="sxs-lookup"><span data-stu-id="5529b-322">A type inherits all event handlers provided by its base type.</span></span> <span data-ttu-id="5529b-323">派生型は、その基本型から継承するイベントマッピングを変更することはできませんが、イベントにハンドラーを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="5529b-323">A derived type cannot in any way alter the event mappings it inherits from its base types, but may add additional handlers to the event.</span></span>


### <a name="extension-methods"></a><span data-ttu-id="5529b-324">Extension のメソッド</span><span class="sxs-lookup"><span data-stu-id="5529b-324">Extension Methods</span></span>

<span data-ttu-id="5529b-325">メソッドは、*拡張メソッド*を使用して、型宣言の外部から型に追加できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-325">Methods can be added to types from outside of the type declaration using *extension methods*.</span></span> <span data-ttu-id="5529b-326">拡張メソッドは、`System.Runtime.CompilerServices.ExtensionAttribute` 属性が適用されたメソッドです。</span><span class="sxs-lookup"><span data-stu-id="5529b-326">Extension methods are methods with the `System.Runtime.CompilerServices.ExtensionAttribute` attribute applied to them.</span></span> <span data-ttu-id="5529b-327">これらは標準モジュールでのみ宣言でき、メソッドが拡張する型を指定するパラメーターが少なくとも1つ必要です。</span><span class="sxs-lookup"><span data-stu-id="5529b-327">They can only be declared in standard modules and must have at least one parameter, which specifies the type the method extends.</span></span> <span data-ttu-id="5529b-328">たとえば、次の拡張メソッドは `String`型を拡張します。</span><span class="sxs-lookup"><span data-stu-id="5529b-328">For example, the following extension method extends the type `String`:</span></span>

```vb
Imports System.Runtime.CompilerServices

Module StringExtensions
    <Extension> _
    Sub Print(s As String)
        Console.WriteLine(s)
    End Sub
End Module
```

<span data-ttu-id="5529b-329">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="5529b-329">__Note.__</span></span> <span data-ttu-id="5529b-330">Visual Basic では、拡張メソッドを標準モジュール内で宣言する必要がありますC#が、などの他の言語では、他の種類の型で宣言することができます。</span><span class="sxs-lookup"><span data-stu-id="5529b-330">Although Visual Basic requires extension methods to be declared in a standard module, other languages such as C# may allow them to be declared in other kinds of types.</span></span> <span data-ttu-id="5529b-331">ここに記載されている他の規則に従っている限り、含まれている型がオープンジェネリック型ではなく、インスタンス化できない場合、Visual Basic は拡張メソッドを認識します。</span><span class="sxs-lookup"><span data-stu-id="5529b-331">As long as the methods follow the other conventions outlined here and the containing type is not an open generic type and cannot be instantiated, Visual Basic will recognize the extension methods.</span></span>

<span data-ttu-id="5529b-332">拡張メソッドが呼び出されると、呼び出し先のインスタンスが最初のパラメーターに渡されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-332">When an extension method is invoked, the instance it is being invoked on is passed to the first parameter.</span></span> <span data-ttu-id="5529b-333">最初のパラメーターを `Optional` または `ParamArray`として宣言することはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-333">The first parameter cannot be declared `Optional` or `ParamArray`.</span></span> <span data-ttu-id="5529b-334">型パラメーターを含む任意の型は、拡張メソッドの最初のパラメーターとして表示できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-334">Any type, including a type parameter, can appear as the first parameter of an extension method.</span></span> <span data-ttu-id="5529b-335">たとえば、次のメソッドは、`Integer()`型、`System.Collections.Generic.IEnumerable(Of T)`を実装する任意の型、およびすべての型を拡張します。</span><span class="sxs-lookup"><span data-stu-id="5529b-335">For example, the following methods extend the types `Integer()`, any type that implements `System.Collections.Generic.IEnumerable(Of T)`, and any type at all:</span></span>

```vb
Imports System.Runtime.CompilerServices

Module Extensions
    <Extension> _
    Sub PrintArray(a() As Integer)
        ...
    End Sub

    <Extension> _
    Sub PrintList(Of T)(a As IEnumerable(Of T))
        ...
    End Sub

    <Extension> _
    Sub Print(Of T)(a As T)
        ...
    End Sub
End Module
```

<span data-ttu-id="5529b-336">前の例で示したように、インターフェイスは拡張できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-336">As the previous example shows, interfaces can be extended.</span></span> <span data-ttu-id="5529b-337">インターフェイス拡張メソッドは、メソッドの実装を提供します。したがって、拡張メソッドが定義されているインターフェイスを実装する型は、インターフェイスによって最初に宣言されたメンバーのみを実装します。</span><span class="sxs-lookup"><span data-stu-id="5529b-337">Interface extension methods supply the implementation of the method, so types that implement an interface that has extension methods defined on it still only implement the members originally declared by the interface.</span></span> <span data-ttu-id="5529b-338">例 :</span><span class="sxs-lookup"><span data-stu-id="5529b-338">For example:</span></span>

```vb
Imports System.Runtime.CompilerServices

Interface IAction
  Sub DoAction()
End Interface

Module IActionExtensions 
    <Extension> _
    Public Sub DoAnotherAction(i As IAction) 
        i.DoAction()
    End Sub
End Module

Class C
  Implements IAction

  Sub DoAction() Implements IAction.DoAction
    ...
  End Sub

  ' ERROR: Cannot implement extension method IAction.DoAnotherAction
  Sub DoAnotherAction() Implements IAction.DoAnotherAction
    ...
  End Sub
End Class
```

<span data-ttu-id="5529b-339">拡張メソッドは、型パラメーターに対して型制約を持つこともできます。また、非拡張ジェネリックメソッドと同様に、型引数を推論することもできます。</span><span class="sxs-lookup"><span data-stu-id="5529b-339">Extension methods can also have type constraints on their type parameters and, just as with non-extension generic methods, type argument can be inferred:</span></span>

```vb
Imports System.Runtime.CompilerServices

Module IEnumerableComparableExtensions
    <Extension> _
    Public Function Sort(Of T As IComparable(Of T))(i As IEnumerable(Of T)) _
        As IEnumerable(Of T)
        ...
    End Function
End Module
```

<span data-ttu-id="5529b-340">拡張メソッドには、拡張される型内の暗黙的なインスタンス式を使用してアクセスすることもできます。</span><span class="sxs-lookup"><span data-stu-id="5529b-340">Extension methods can also be accessed through implicit instance expressions within the type being extended:</span></span>

```vb
Imports System.Runtime.CompilerServices

Class C1
    Sub M1()
        Me.M2()
        M2()
    End Sub
End Class

Module C1Extensions
    <Extension>
    Sub M2(c As C1)
        ...
    End Sub
End Module
```

<span data-ttu-id="5529b-341">アクセシビリティのために、拡張メソッドは、宣言されている標準モジュールのメンバーとしても扱われます。これらのメソッドは、宣言コンテキストを使用することによって、拡張されているアクセス権を超える型のメンバーに対する追加のアクセス権を持っていません。</span><span class="sxs-lookup"><span data-stu-id="5529b-341">For the purposes of accessibility, extension methods are also treated as members of the standard module they are declared in -- they have no extra access to the members of the type they are extending beyond the access they have by virtue of their declaration context.</span></span>

<span data-ttu-id="5529b-342">拡張メソッドは、標準モジュールメソッドがスコープ内にある場合にのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-342">Extensions methods are only available when the standard module method is in scope.</span></span> <span data-ttu-id="5529b-343">そうしないと、元の型は拡張されていないように見えます。</span><span class="sxs-lookup"><span data-stu-id="5529b-343">Otherwise, the original type will not appear to have been extended.</span></span> <span data-ttu-id="5529b-344">例 :</span><span class="sxs-lookup"><span data-stu-id="5529b-344">For example:</span></span>

```vb
Imports System.Runtime.CompilerServices

Class C1
End Class

Namespace N1
    Module C1Extensions
        <Extension> _
        Sub M1(c As C1)
            ...
        End Sub
    End Module
End Namespace

Module Test
    Sub Main()
        Dim c As New C1()

        ' Error: c has no member named "M1"
        c.M1()
    End Sub
End Module
```

<span data-ttu-id="5529b-345">型の拡張メソッドだけが使用可能な場合、型を参照しても、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="5529b-345">Referring to a type when only an extension method on the type is available will still produce a compile-time error.</span></span>

<span data-ttu-id="5529b-346">拡張メソッドは、厳密に型指定された `For Each` パターンなど、メンバーがバインドされるすべてのコンテキストの型のメンバーであると見なされることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="5529b-346">It is important to note that extension methods are considered to be members of the type in all contexts where members are bound, such as the strongly-typed `For Each` pattern.</span></span> <span data-ttu-id="5529b-347">例 :</span><span class="sxs-lookup"><span data-stu-id="5529b-347">For example:</span></span>

```vb
Imports System.Runtime.CompilerServices

Class C1
End Class

Class C1Enumerator
    ReadOnly Property Current() As C1
        Get
            ...
        End Get
    End Property

    Function MoveNext() As Boolean
        ...
    End Function
End Class

Module C1Extensions
    <Extension> _
    Function GetEnumerator(c As C1) As C1Enumerator
        ...
    End Function
End Module

Module Test
    Sub Main()
        Dim c As New C1()

        ' Valid
        For Each o As Object In c
            ...
        Next o
    End Sub
End Module
```

<span data-ttu-id="5529b-348">拡張メソッドを参照するデリゲートを作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="5529b-348">Delegates can also be created that refer to extension methods.</span></span> <span data-ttu-id="5529b-349">したがって、コードは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="5529b-349">Thus, the code:</span></span>

```vb
Delegate Sub D1()

Module Test
    Sub Main()
        Dim s As String = "Hello, World!"
        Dim d As D1

        d = AddressOf s.Print
        d()
    End Sub
End Module
```

<span data-ttu-id="5529b-350">は、次の場合とほぼ同じです。</span><span class="sxs-lookup"><span data-stu-id="5529b-350">is roughly equivalent to:</span></span>

```vb
Delegate Sub D1()

Module Test
    Sub Main()
      Dim s As String = "Hello, World!"
      Dim d As D1

      d = CType([Delegate].CreateDelegate(GetType(D1), s, _
                GetType(StringExtensions).GetMethod("Print")), D1)
      d()
    End Sub
End Module
```

<span data-ttu-id="5529b-351">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="5529b-351">__Note.__</span></span> <span data-ttu-id="5529b-352">Visual Basic は、通常、メソッドが呼び出されているインスタンスが `Nothing`場合に `System.NullReferenceException` を発生させるインスタンスメソッド呼び出しにチェックを挿入します。</span><span class="sxs-lookup"><span data-stu-id="5529b-352">Visual Basic normally inserts a check on an instance method call that causes a `System.NullReferenceException` to occur if the instance the method is being invoked on is `Nothing`.</span></span> <span data-ttu-id="5529b-353">拡張メソッドの場合は、このチェックを挿入するための効率的な方法がないため、拡張メソッドは `Nothing`を明示的にチェックする必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-353">In the case of extension methods, there is no efficient way to insert this check, so extension methods will need to explicitly check for `Nothing`.</span></span> 

<span data-ttu-id="5529b-354">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="5529b-354">__Note.__</span></span> <span data-ttu-id="5529b-355">値型は、インターフェイスとして型指定されたパラメーターに `ByVal` 引数として渡されるときにボックス化されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-355">A value type will be boxed when being passed as a `ByVal` argument to a parameter typed as an interface.</span></span>  <span data-ttu-id="5529b-356">これは、拡張メソッドの副作用が、元のではなく、構造体のコピーに対して動作することを意味します。</span><span class="sxs-lookup"><span data-stu-id="5529b-356">This implies that side effects of the extension method will operate on a copy of the structure instead of the original.</span></span> <span data-ttu-id="5529b-357">言語では、拡張メソッドの最初の引数に制限はありませんが、拡張メソッドを使用して値型を拡張したり、値型を拡張したりするときには、最初のパラメーターが `ByRef` 渡され、元の値に対して副作用が動作するようにすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="5529b-357">While the language puts no restrictions on the first argument of an extension method, it is recommended that extension methods are not used to extend value types or that when extending value types, the first parameter is passed `ByRef` to ensure that side effects operate on the original value.</span></span>

### <a name="partial-methods"></a><span data-ttu-id="5529b-358">部分メソッド</span><span class="sxs-lookup"><span data-stu-id="5529b-358">Partial Methods</span></span>

<span data-ttu-id="5529b-359">*部分メソッド*は、メソッドの本体ではなく、シグネチャを指定するメソッドです。</span><span class="sxs-lookup"><span data-stu-id="5529b-359">A *partial method* is a method that specifies a signature but not the body of the method.</span></span> <span data-ttu-id="5529b-360">メソッドの本体は、同じ名前およびシグネチャを持つ別のメソッド宣言によって指定できます。この宣言は、通常、型の別の部分宣言に含まれています。</span><span class="sxs-lookup"><span data-stu-id="5529b-360">The body of the method can be supplied by another method declaration with the same name and signature, most likely in another partial declaration of the type.</span></span> <span data-ttu-id="5529b-361">例 :</span><span class="sxs-lookup"><span data-stu-id="5529b-361">For example:</span></span>

<span data-ttu-id="5529b-362">.vb:</span><span class="sxs-lookup"><span data-stu-id="5529b-362">a.vb:</span></span>

```vb
' Designer generated code
Public Partial Class MyForm
    Private Partial Sub ValidateControls()
    End Sub

    Public Sub New()
        ' Initialize controls
        ...

        ValidateControls()
    End Sub    
End Class
```

<span data-ttu-id="5529b-363">b. vb:</span><span class="sxs-lookup"><span data-stu-id="5529b-363">b.vb:</span></span>

```vb
Public Partial Class MyForm
    Public Sub ValidateControls()
        ' Validation logic goes here
        ...
    End Sub
End Class
```

<span data-ttu-id="5529b-364">この例では、クラスの部分宣言 `MyForm`、実装のない部分メソッド `ValidateControls` 宣言しています。</span><span class="sxs-lookup"><span data-stu-id="5529b-364">In this example, a partial declaration of the class `MyForm` declares a partial method `ValidateControls` with no implementation.</span></span> <span data-ttu-id="5529b-365">ファイルに本文が指定されていない場合でも、部分宣言のコンストラクターは部分メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="5529b-365">The constructor in the partial declaration calls the partial method, even though there is no body supplied in the file.</span></span> <span data-ttu-id="5529b-366">その他の `MyForm` の部分宣言は、メソッドの実装を提供します。</span><span class="sxs-lookup"><span data-stu-id="5529b-366">The other partial declaration of `MyForm` then supplies the implementation of the method.</span></span>

<span data-ttu-id="5529b-367">部分メソッドは、本文が指定されているかどうかに関係なく呼び出すことができます。メソッド本体が指定されていない場合、呼び出しは無視されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-367">Partial methods can be called regardless of whether a body has been supplied; if no method body is supplied, the call is ignored.</span></span> <span data-ttu-id="5529b-368">例 :</span><span class="sxs-lookup"><span data-stu-id="5529b-368">For example:</span></span>

```vb
Public Class C1
    Private Partial Sub M1()
    End Sub

    Public Sub New()
        ' Since no implementation is supplied, this call will not be made.
        M1()
    End Sub
End Class
```

<span data-ttu-id="5529b-369">無視される部分メソッド呼び出しに引数として渡されるすべての式は無視され、評価されません。</span><span class="sxs-lookup"><span data-stu-id="5529b-369">Any expressions that are passed in as arguments to a partial method call that is ignored are ignored also and not evaluated.</span></span> <span data-ttu-id="5529b-370">(__注:__</span><span class="sxs-lookup"><span data-stu-id="5529b-370">(__Note.__</span></span> <span data-ttu-id="5529b-371">つまり、部分メソッドは、2つの部分型に対して定義されている動作を提供する非常に効率的な方法です。部分メソッドは使用されない場合はコストがないためです)。</span><span class="sxs-lookup"><span data-stu-id="5529b-371">This means that partial methods are a very efficient way of providing behavior that is defined across two partial types, since the partial methods have no cost if they are not used.)</span></span>

<span data-ttu-id="5529b-372">部分メソッドの宣言は `Private` として宣言する必要があり、常に本体にステートメントがないサブルーチンである必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-372">The partial method declaration must be declared as `Private` and must always be a subroutine with no statements in its body.</span></span> <span data-ttu-id="5529b-373">部分メソッドは、それ自体がインターフェイスメソッドを実装することはできませんが、その本体を提供するメソッドは使用できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-373">Partial methods cannot themselves implement interface methods, although the method that supplies their body can.</span></span>

<span data-ttu-id="5529b-374">本文を部分メソッドに渡すことができるのは、1つのメソッドだけです。</span><span class="sxs-lookup"><span data-stu-id="5529b-374">Only one method can supply a body to a partial method.</span></span> <span data-ttu-id="5529b-375">部分メソッドに本文を指定するメソッドには、部分メソッドと同じシグネチャ、任意の型パラメーターに対する同じ制約、同じ宣言修飾子、および同じパラメーターと型パラメーター名が必要です。</span><span class="sxs-lookup"><span data-stu-id="5529b-375">A method supplying a body to a partial method must have the same signature as the partial method, the same constraints on any type parameters, the same declaration modifiers, and the same parameter and type parameter names.</span></span> <span data-ttu-id="5529b-376">部分メソッドの属性とその本体を提供するメソッドは、メソッドのパラメーターの属性と同様にマージされます。</span><span class="sxs-lookup"><span data-stu-id="5529b-376">Attributes on the partial method and the method that supplies its body are merged, as are any attributes on the methods' parameters.</span></span> <span data-ttu-id="5529b-377">同様に、メソッドが処理するイベントの一覧がマージされます。</span><span class="sxs-lookup"><span data-stu-id="5529b-377">Similarly, the list of events that the methods handle is merged.</span></span> <span data-ttu-id="5529b-378">例 :</span><span class="sxs-lookup"><span data-stu-id="5529b-378">For example:</span></span>

```vb
Class C1
    Event E1()
    Event E2()

    Private Partial Sub S() Handles Me.E1
    End Sub

    ' Handles both E1 and E2
    Private Sub S() Handles Me.E2
        ...
    End Sub
End Class
```

## <a name="constructors"></a><span data-ttu-id="5529b-379">コンストラクター</span><span class="sxs-lookup"><span data-stu-id="5529b-379">Constructors</span></span>

<span data-ttu-id="5529b-380">*コンストラクター*は、初期化の制御を可能にする特殊なメソッドです。</span><span class="sxs-lookup"><span data-stu-id="5529b-380">*Constructors* are special methods that allow control over initialization.</span></span> <span data-ttu-id="5529b-381">これらは、プログラムが開始した後、または型のインスタンスが作成されたときに実行されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-381">They are run after the program begins or when an instance of a type is created.</span></span> <span data-ttu-id="5529b-382">他のメンバーとは異なり、コンストラクターは継承されず、型の宣言空間に名前を導入しません。</span><span class="sxs-lookup"><span data-stu-id="5529b-382">Unlike other members, constructors are not inherited and do not introduce a name into a type's declaration space.</span></span> <span data-ttu-id="5529b-383">コンストラクターは、オブジェクト作成式または .NET Framework によってのみ呼び出すことができます。直接呼び出すことはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-383">Constructors may only be invoked by object-creation expressions or by the .NET Framework; they may never be directly invoked.</span></span>

<span data-ttu-id="5529b-384">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="5529b-384">__Note.__</span></span> <span data-ttu-id="5529b-385">コンストラクターには、サブルーチンが持つ行の配置に対して同じ制限があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-385">Constructors have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="5529b-386">開始ステートメント、終了ステートメント、およびブロックはすべて論理行の先頭に記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-386">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>

```antlr
ConstructorMemberDeclaration
    : Attributes? ConstructorModifier* 'Sub' 'New'
      ( OpenParenthesis ParameterList? CloseParenthesis )? LineTerminator
      Block?
      'End' 'Sub' StatementTerminator
    ;

ConstructorModifier
    : AccessModifier
    | 'Shared'
    ;
```

### <a name="instance-constructors"></a><span data-ttu-id="5529b-387">インスタンス コンストラクター</span><span class="sxs-lookup"><span data-stu-id="5529b-387">Instance Constructors</span></span>

<span data-ttu-id="5529b-388">インスタンス*コンストラクター*は、型のインスタンスを初期化し、インスタンスの作成時に .NET Framework によって実行されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-388">*Instance constructors* initialize instances of a type and are run by the .NET Framework when an instance is created.</span></span> <span data-ttu-id="5529b-389">コンストラクターのパラメーターリストは、メソッドのパラメーターリストと同じ規則に従います。</span><span class="sxs-lookup"><span data-stu-id="5529b-389">The parameter list of a constructor is subject to the same rules as the parameter list of a method.</span></span> <span data-ttu-id="5529b-390">インスタンスコンストラクターがオーバーロードされている可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-390">Instance constructors may be overloaded.</span></span>

<span data-ttu-id="5529b-391">参照型のすべてのコンストラクターは、別のコンストラクターを呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-391">All constructors in reference types must invoke another constructor.</span></span> <span data-ttu-id="5529b-392">呼び出しが明示的である場合は、コンストラクターのメソッド本体の最初のステートメントである必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-392">If the invocation is explicit, it must be the first statement in the constructor method body.</span></span> <span data-ttu-id="5529b-393">ステートメントでは、型のインスタンスコンストラクターの別の (たとえば、`Me.New(...)` または `MyClass.New(...)`) を呼び出すことができます。構造体でない場合は、型の基本型のインスタンスコンストラクター (`MyBase.New(...)`など) を呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="5529b-393">The statement can either invoke another of the type's instance constructors -- for example, `Me.New(...)` or `MyClass.New(...)` -- or if it is not a structure it can invoke an instance constructor of the type's base type -- for example, `MyBase.New(...)`.</span></span> <span data-ttu-id="5529b-394">コンストラクターがそれ自体を呼び出すことはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-394">It is invalid for a constructor to invoke itself.</span></span> <span data-ttu-id="5529b-395">コンストラクターが別のコンストラクターの呼び出しを省略した場合、`MyBase.New()` は暗黙的になります。</span><span class="sxs-lookup"><span data-stu-id="5529b-395">If a constructor omits a call to another constructor, `MyBase.New()` is implicit.</span></span> <span data-ttu-id="5529b-396">パラメーターなしの基本型コンストラクターがない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="5529b-396">If there is no parameterless base type constructor, a compile-time error occurs.</span></span> <span data-ttu-id="5529b-397">`Me` は基底クラスのコンストラクターへの呼び出しの後に構築されるとは見なされないため、コンストラクター呼び出しステートメントのパラメーターは、`Me`、`MyClass`、または `MyBase` を暗黙的または明示的に参照することはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-397">Because `Me` is not considered to be constructed until after the call to a base class constructor, the parameters to a constructor invocation statement cannot reference `Me`, `MyClass`, or `MyBase` implicitly or explicitly.</span></span>

<span data-ttu-id="5529b-398">コンストラクターの最初のステートメントが `MyBase.New(...)`の形式である場合、コンストラクターは、型で宣言されたインスタンス変数の変数初期化子によって指定された初期化を暗黙的に実行します。</span><span class="sxs-lookup"><span data-stu-id="5529b-398">When a constructor's first statement is of the form `MyBase.New(...)`, the constructor implicitly performs the initializations specified by the variable initializers of the instance variables declared in the type.</span></span> <span data-ttu-id="5529b-399">これは、直接の基本型コンストラクターを呼び出した直後に実行される代入のシーケンスに対応します。</span><span class="sxs-lookup"><span data-stu-id="5529b-399">This corresponds to a sequence of assignments that are executed immediately after invoking the direct base type constructor.</span></span> <span data-ttu-id="5529b-400">このような順序を使用すると、インスタンスへのアクセス権を持つステートメントが実行される前に、すべての基本インスタンス変数が変数初期化子によって初期化されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-400">Such ordering ensures that all base instance variables are initialized by their variable initializers before any statements that have access to the instance are executed.</span></span> <span data-ttu-id="5529b-401">例 :</span><span class="sxs-lookup"><span data-stu-id="5529b-401">For example:</span></span>

```vb
Class A
    Protected x As Integer = 1
End Class

Class B
    Inherits A

    Private y As Integer = x

    Public Sub New()
        Console.WriteLine("x = " & x & ", y = " & y)
    End Sub
End Class
```

<span data-ttu-id="5529b-402">`B`のインスタンスを作成するために `New B()` を使用すると、次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-402">When `New B()` is used to create an instance of `B`, the following output is produced:</span></span>

```console
x = 1, y = 1
```

<span data-ttu-id="5529b-403">`y` の値は、基本クラスのコンストラクターが呼び出された後に変数初期化子が実行されるため `1` です。</span><span class="sxs-lookup"><span data-stu-id="5529b-403">The value of `y` is `1` because the variable initializer is executed after the base class constructor is invoked.</span></span> <span data-ttu-id="5529b-404">変数初期化子は、型宣言に表示される順序に従って実行されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-404">Variable initializers are executed in the textual order they appear in the type declaration.</span></span>

<span data-ttu-id="5529b-405">型が `Private` コンストラクターのみを宣言している場合は、他の型を型から派生させたり、型のインスタンスを作成したりすることは一般的ではありません。唯一の例外は、型の中で入れ子にされた型です。</span><span class="sxs-lookup"><span data-stu-id="5529b-405">When a type declares only `Private` constructors, it is not possible in general for other types to derive from the type or create instances of the type; the only exception is types nested within the type.</span></span> <span data-ttu-id="5529b-406">`Private` コンストラクターは、`Shared` メンバーだけを含む型でよく使用されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-406">`Private` constructors are commonly used in types that contain only `Shared` members.</span></span>

<span data-ttu-id="5529b-407">型にインスタンスコンストラクターの宣言が含まれていない場合は、既定のコンストラクターが自動的に指定されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-407">If a type contains no instance constructor declarations, a default constructor is automatically provided.</span></span> <span data-ttu-id="5529b-408">既定のコンストラクターは、直接基本型のパラメーターなしのコンストラクターを呼び出すだけです。</span><span class="sxs-lookup"><span data-stu-id="5529b-408">The default constructor simply invokes the parameterless constructor of the direct base type.</span></span> <span data-ttu-id="5529b-409">直接の基本型にアクセス可能なパラメーターなしのコンストラクターがない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="5529b-409">If the direct base type does not have an accessible parameterless constructor, a compile-time error occurs.</span></span> <span data-ttu-id="5529b-410">既定のコンストラクターに対して宣言されているアクセス型は、型が `MustInherit`でない限り、`Public` ます。この場合、既定のコンストラクターは `Protected`ます。</span><span class="sxs-lookup"><span data-stu-id="5529b-410">The declared access type for the default constructor is `Public` unless the type is `MustInherit`, in which case the default constructor is `Protected`.</span></span>

<span data-ttu-id="5529b-411">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="5529b-411">__Note.__</span></span> <span data-ttu-id="5529b-412">`MustInherit` クラスを直接作成することはできないため、`MustInherit` 型の既定のコンストラクターに対する既定のアクセスは `Protected` ます。</span><span class="sxs-lookup"><span data-stu-id="5529b-412">The default access for a `MustInherit` type's default constructor is `Protected` because `MustInherit` classes cannot be created directly.</span></span> <span data-ttu-id="5529b-413">したがって、既定のコンストラクター `Public`するポイントはありません。</span><span class="sxs-lookup"><span data-stu-id="5529b-413">So there is no point in making the default constructor `Public`.</span></span>

<span data-ttu-id="5529b-414">次の例では、クラスにコンストラクター宣言が含まれていないため、既定のコンストラクターが用意されています。</span><span class="sxs-lookup"><span data-stu-id="5529b-414">In the following example a default constructor is provided because the class contains no constructor declarations:</span></span>

```vb
Class Message
    Dim sender As Object
    Dim text As String
End Class
```

<span data-ttu-id="5529b-415">したがって、この例は次のように正確に等価です。</span><span class="sxs-lookup"><span data-stu-id="5529b-415">Thus, the example is precisely equivalent to the following:</span></span>

```vb
Class Message
    Dim sender As Object
    Dim text As String

    Sub New()
    End Sub
End Class
```

<span data-ttu-id="5529b-416">属性 `Microsoft.VisualBasic.CompilerServices.DesignerGeneratedAttribute` でマークされたデザイナーで生成されたクラスに生成される既定のコンストラクターは、メソッドが存在する場合は、基本コンストラクターの呼び出しの後に、メソッド `Sub InitializeComponent()`呼び出します。</span><span class="sxs-lookup"><span data-stu-id="5529b-416">Default constructors that are emitted into a designer generated class marked with the attribute `Microsoft.VisualBasic.CompilerServices.DesignerGeneratedAttribute` will call the method `Sub InitializeComponent()`, if it exists, after the call to the base constructor.</span></span> <span data-ttu-id="5529b-417">(__注:__</span><span class="sxs-lookup"><span data-stu-id="5529b-417">(__Note.__</span></span> <span data-ttu-id="5529b-418">これにより、WinForms デザイナーによって作成されたファイルなど、デザイナーで生成されたファイルを使用して、デザイナーファイル内のコンストラクターを省略できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-418">This allows designer generated files, such as those created by the WinForms designer, to omit the constructor in the designer file.</span></span> <span data-ttu-id="5529b-419">これにより、プログラマが選択する場合は、プログラマ自身を指定することができます)。</span><span class="sxs-lookup"><span data-stu-id="5529b-419">This enables the programmer to specify it themselves, if they so choose.)</span></span>

### <a name="shared-constructors"></a><span data-ttu-id="5529b-420">共有コンストラクター</span><span class="sxs-lookup"><span data-stu-id="5529b-420">Shared Constructors</span></span>

<span data-ttu-id="5529b-421">*共有コンストラクター*は、型の共有変数を初期化します。これらは、プログラムの実行開始後、型のメンバーへの参照の前に実行されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-421">*Shared constructors* initialize a type's shared variables; they are run after the program begins executing, but before any references to a member of the type.</span></span> <span data-ttu-id="5529b-422">共有コンストラクターでは、標準モジュールに含まれていない限り、`Shared` 修飾子を指定します。この場合、`Shared` 修飾子は暗黙的に指定されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-422">A shared constructor specifies the `Shared` modifier, unless it is in a standard module in which case the `Shared` modifier is implied.</span></span>

<span data-ttu-id="5529b-423">インスタンスコンストラクターとは異なり、共有コンストラクターは暗黙的なパブリックアクセスを持ち、パラメーターを持たず、他のコンストラクターを呼び出すことができません。</span><span class="sxs-lookup"><span data-stu-id="5529b-423">Unlike instance constructors, shared constructors have implicit public access, have no parameters, and may not call other constructors.</span></span> <span data-ttu-id="5529b-424">共有コンストラクターの最初のステートメントの前に、共有コンストラクターは、型で宣言された共有変数の変数初期化子によって指定された初期化を暗黙的に実行します。</span><span class="sxs-lookup"><span data-stu-id="5529b-424">Before the first statement in a shared constructor, the shared constructor implicitly performs the initializations specified by the variable initializers of the shared variables declared in the type.</span></span> <span data-ttu-id="5529b-425">これは、コンストラクターへの入力時に直ちに実行される割り当てのシーケンスに対応します。</span><span class="sxs-lookup"><span data-stu-id="5529b-425">This corresponds to a sequence of assignments that are executed immediately upon entry to the constructor.</span></span> <span data-ttu-id="5529b-426">変数初期化子は、型宣言に表示される順序に従って実行されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-426">The variable initializers are executed in the textual order they appear in the type declaration.</span></span>

<span data-ttu-id="5529b-427">次の例は、共有変数を初期化する共有コンストラクターを持つ `Employee` クラスを示しています。</span><span class="sxs-lookup"><span data-stu-id="5529b-427">The following example shows an `Employee` class with a shared constructor that initializes a shared variable:</span></span>

```vb
Imports System.Data

Class Employee
    Private Shared ds As DataSet

    Shared Sub New()
        ds = New DataSet()
    End Sub

    Public Name As String
    Public Salary As Decimal
End Class
```

<span data-ttu-id="5529b-428">閉じられたジェネリック型ごとに個別の共有コンストラクターが存在します。</span><span class="sxs-lookup"><span data-stu-id="5529b-428">A separate shared constructor exists for each closed generic type.</span></span> <span data-ttu-id="5529b-429">共有コンストラクターは、閉じられた型ごとに1回だけ実行されるので、制約を介してコンパイル時にチェックできない型パラメーターに対して実行時チェックを適用するのに便利な場所です。</span><span class="sxs-lookup"><span data-stu-id="5529b-429">Because the shared constructor is executed exactly once for each closed type, it is a convenient place to enforce run-time checks on the type parameter that cannot be checked at compile-time via constraints.</span></span> <span data-ttu-id="5529b-430">たとえば、次の型では、共有コンストラクターを使用して、型パラメーターが `Integer` または `Double`ことを強制しています。</span><span class="sxs-lookup"><span data-stu-id="5529b-430">For example, the following type uses a shared constructor to enforce that the type parameter is `Integer` or `Double`:</span></span>

```vb
Class EnumHolder(Of T)
    Shared Sub New() 
        If Not GetType(T).IsEnum() Then
            Throw New ArgumentException("T must be an enumerated type.")
        End If
    End Sub
End Class
```

<span data-ttu-id="5529b-431">共有コンストラクターが実行される場合、厳密には実装に依存しますが、共有コンストラクターが明示的に定義されている場合は、次のような保証が提供されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-431">Exactly when shared constructors are run is mostly implementation dependent, though several guarantees are provided if a shared constructor is explicitly defined:</span></span>

* <span data-ttu-id="5529b-432">共有コンストラクターは、型の静的フィールドに最初にアクセスする前に実行されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-432">Shared constructors are run before the first access to any static field of the type.</span></span>

* <span data-ttu-id="5529b-433">共有コンストラクターは、型の静的メソッドの最初の呼び出しの前に実行されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-433">Shared constructors are run before the first invocation of any static method of the type.</span></span>

* <span data-ttu-id="5529b-434">共有コンストラクターは、その型のコンストラクターの最初の呼び出しの前に実行されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-434">Shared constructors are run before the first invocation of any constructor for the type.</span></span>

<span data-ttu-id="5529b-435">共有コンストラクターが共有初期化子に対して暗黙的に作成される場合、上記の保証は適用されません。</span><span class="sxs-lookup"><span data-stu-id="5529b-435">The above guarantees do not apply in the situation where a shared constructor is implicitly created for shared initializers.</span></span> <span data-ttu-id="5529b-436">次の例の出力は、読み込みの正確な順序が定義されていないため、不確定です。</span><span class="sxs-lookup"><span data-stu-id="5529b-436">The output from the following example is uncertain, because the exact ordering of loading and therefore of shared constructor execution is not defined:</span></span>

```vb
Module Test
    Sub Main()
        A.F()
        B.F()
    End Sub
End Module

Class A
    Shared Sub New()
        Console.WriteLine("Init A")
    End Sub

    Public Shared Sub F()
        Console.WriteLine("A.F")
    End Sub
End Class

Class B
    Shared Sub New()
        Console.WriteLine("Init B")
    End Sub

    Public Shared Sub F()
        Console.WriteLine("B.F")
    End Sub
End Class
```

<span data-ttu-id="5529b-437">出力は、次のいずれかになります。</span><span class="sxs-lookup"><span data-stu-id="5529b-437">The output could be either of the following:</span></span>

```console
Init A
A.F
Init B
B.F
```

<span data-ttu-id="5529b-438">、または</span><span class="sxs-lookup"><span data-stu-id="5529b-438">or</span></span>

```console
Init B
Init A
A.F
B.F
```

<span data-ttu-id="5529b-439">これに対し、次の例では、予測可能な出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-439">By contrast, the following example produces predictable output.</span></span> <span data-ttu-id="5529b-440">クラス `B` 派生クラスであっても、クラス `A` の `Shared` コンストラクターは実行されないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="5529b-440">Note that the `Shared` constructor for the class `A` never executes, even though class `B` derives from it:</span></span>

```vb
Module Test
    Sub Main()
        B.G()
    End Sub
End Module

Class A
    Shared Sub New()
        Console.WriteLine("Init A")
    End Sub
End Class

Class B
    Inherits A

    Shared Sub New()
        Console.WriteLine("Init B")
    End Sub

    Public Shared Sub G()
        Console.WriteLine("B.G")
    End Sub
End Class
```

<span data-ttu-id="5529b-441">出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="5529b-441">The output is:</span></span>

```console
Init B
B.G
```

<span data-ttu-id="5529b-442">また、次の例のように、変数初期化子を持つ `Shared` 変数を既定値の状態で確認できるようにする、循環依存関係を構築することもできます。</span><span class="sxs-lookup"><span data-stu-id="5529b-442">It is also possible to construct circular dependencies that allow `Shared` variables with variable initializers to be observed in their default value state, as in the following example:</span></span>

```vb
Class A
    Public Shared X As Integer = B.Y + 1
End Class

Class B
    Public Shared Y As Integer = A.X + 1

    Shared Sub Main()
        Console.WriteLine("X = " & A.X & ", Y = " & B.Y)
    End Sub
End Class
```

<span data-ttu-id="5529b-443">これにより、次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-443">This produces the output:</span></span>

```console
X = 1, Y = 2
```

<span data-ttu-id="5529b-444">`Main` メソッドを実行するために、システムはまずクラス `B`を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="5529b-444">To execute the `Main` method, the system first loads class `B`.</span></span> <span data-ttu-id="5529b-445">クラス `B` の `Shared` コンストラクターは `Y`の初期値を計算します。これにより、`A.X` の値が参照されるため、クラス `A` が再帰的に読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="5529b-445">The `Shared` constructor of class `B` proceeds to compute the initial value of `Y`, which recursively causes class `A` to be loaded because the value of `A.X` is referenced.</span></span> <span data-ttu-id="5529b-446">クラス `A` の `Shared` コンストラクターは `X`の初期値を計算します。これにより、`Y`の*既定*値 (0) がフェッチされます。</span><span class="sxs-lookup"><span data-stu-id="5529b-446">The `Shared` constructor of class `A` in turn proceeds to compute the initial value of `X`, and in doing so fetches the *default* value of `Y`, which is zero.</span></span> <span data-ttu-id="5529b-447">`A.X` は `1`に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-447">`A.X` is thus initialized to `1`.</span></span> <span data-ttu-id="5529b-448">次に、読み込み `A` の処理が完了し、`Y`の初期値の計算に戻り、その結果が `2`になります。</span><span class="sxs-lookup"><span data-stu-id="5529b-448">The process of loading `A` then completes, returning to the calculation of the initial value of `Y`, the result of which becomes `2`.</span></span>

<span data-ttu-id="5529b-449">`Main` メソッドがクラス `A`に配置されていた場合、例では次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-449">Had the `Main` method instead been located in class `A`, the example would have produced the following output:</span></span>

```console
X = 2, Y = 1
```

<span data-ttu-id="5529b-450">`Shared` 変数初期化子での循環参照は避けてください。通常、このような参照を含むクラスが読み込まれる順序を決定することはできないためです。</span><span class="sxs-lookup"><span data-stu-id="5529b-450">Avoid circular references in `Shared` variable initializers since it is generally impossible to determine the order in which classes containing such references are loaded.</span></span>

## <a name="events"></a><span data-ttu-id="5529b-451">イベント</span><span class="sxs-lookup"><span data-stu-id="5529b-451">Events</span></span>

<span data-ttu-id="5529b-452">イベントは、特定のイベントの発生をコードに通知するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-452">Events are used to notify code of a particular occurrence.</span></span> <span data-ttu-id="5529b-453">イベント宣言は、デリゲート型またはパラメーターリスト、および省略可能な `Implements` 句のいずれかの識別子で構成されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-453">An event declaration consists of an identifier, either a delegate type or a parameter list, and an optional `Implements` clause.</span></span>

```antlr
EventMemberDeclaration
    : RegularEventMemberDeclaration
    | CustomEventMemberDeclaration
    ;

RegularEventMemberDeclaration
    : Attributes? EventModifiers* 'Event'
      Identifier ParametersOrType ImplementsClause? StatementTerminator
    ;

InterfaceEventMemberDeclaration
    : Attributes? InterfaceEventModifiers* 'Event'
      Identifier ParametersOrType StatementTerminator
    ;

ParametersOrType
    : ( OpenParenthesis ParameterList? CloseParenthesis )?
    | 'As' NonArrayTypeName
    ;

EventModifiers
    : AccessModifier
    | 'Shadows'
    | 'Shared'
    ;

InterfaceEventModifiers
    : 'Shadows'
    ;
```

<span data-ttu-id="5529b-454">デリゲート型が指定されている場合、デリゲート型に戻り値の型を指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-454">If a delegate type is specified, the delegate type may not have a return type.</span></span> <span data-ttu-id="5529b-455">パラメーターリストが指定されている場合は、`Optional` パラメーターまたは `ParamArray` パラメーターが含まれていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-455">If a parameter list is specified, it may not contain `Optional` or `ParamArray` parameters.</span></span> <span data-ttu-id="5529b-456">パラメーターの型やデリゲート型のアクセシビリティドメインは、イベント自体のアクセシビリティドメインのスーパーセットと同じであるか、またはそのいずれかである必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-456">The accessibility domain of the parameter types and/or delegate type must be the same as, or a superset of, the accessibility domain of the event itself.</span></span> <span data-ttu-id="5529b-457">イベントは、`Shared` 修飾子を指定することによって共有できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-457">Events may be shared by specifying the `Shared` modifier.</span></span>

<span data-ttu-id="5529b-458">型の宣言空間に追加されたメンバー名に加えて、イベント宣言は、他のいくつかのメンバーを暗黙的に宣言します。</span><span class="sxs-lookup"><span data-stu-id="5529b-458">In addition to the member name added to the type's declaration space, an event declaration implicitly declares several other members.</span></span> <span data-ttu-id="5529b-459">`X`という名前のイベントを指定すると、次のメンバーが宣言領域に追加されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-459">Given an event named `X`, the following members are added to the declaration space:</span></span>

* <span data-ttu-id="5529b-460">宣言の形式がメソッド宣言の場合は、`XEventHandler` という名前の入れ子になったデリゲートクラスが導入されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-460">If the form of the declaration is a method declaration, a nested delegate class named `XEventHandler` is introduced.</span></span> <span data-ttu-id="5529b-461">入れ子になったデリゲートクラスは、メソッド宣言と一致し、イベントと同じアクセシビリティを持ちます。</span><span class="sxs-lookup"><span data-stu-id="5529b-461">The nested delegate class matches the method declaration and has the same accessibility as the event.</span></span> <span data-ttu-id="5529b-462">パラメーターリストの属性は、delegate クラスのパラメーターに適用されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-462">The attributes in the parameter list apply to the parameters of the delegate class.</span></span>

* <span data-ttu-id="5529b-463">`XEvent`という名前のデリゲートとして型指定された `Private` インスタンス変数。</span><span class="sxs-lookup"><span data-stu-id="5529b-463">A `Private` instance variable typed as the delegate, named `XEvent`.</span></span>

* <span data-ttu-id="5529b-464">`add_X` `remove_X` とという名前の2つのメソッドを呼び出し、オーバーライドまたはオーバーロードすることはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-464">Two methods named `add_X` and `remove_X` which cannot be invoked, overridden or overloaded.</span></span>

<span data-ttu-id="5529b-465">型が上記のいずれかの名前と一致する名前を宣言しようとすると、コンパイル時エラーが発生し、暗黙的な `add_X` と `remove_X` の宣言は名前のバインドのために無視されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-465">If a type attempts to declare a name that matches one of the above names, a compile-time error will result, and the implicit `add_X` and `remove_X` declarations are ignored for the purposes of name binding.</span></span> <span data-ttu-id="5529b-466">導入されたメンバーをオーバーライドまたはオーバーロードすることはできませんが、派生型でシャドウすることはできます。</span><span class="sxs-lookup"><span data-stu-id="5529b-466">It is not possible to override or overload any of the introduced members, although it is possible to shadow them in derived types.</span></span> <span data-ttu-id="5529b-467">たとえば、クラス宣言は</span><span class="sxs-lookup"><span data-stu-id="5529b-467">For example, the class declaration</span></span>

```vb
Class Raiser
    Public Event Constructed(i As Integer)
End Class
```

<span data-ttu-id="5529b-468">は、次の宣言と同じです。</span><span class="sxs-lookup"><span data-stu-id="5529b-468">is equivalent to the following declaration</span></span>

```vb
Class Raiser
    Public Delegate Sub ConstructedEventHandler(i As Integer)

    Protected ConstructedEvent As ConstructedEventHandler

    Public Sub add_Constructed(d As ConstructedEventHandler)
        ConstructedEvent = _
            CType( _
                [Delegate].Combine(ConstructedEvent, d), _
                    Raiser.ConstructedEventHandler)
    End Sub

    Public Sub remove_Constructed(d As ConstructedEventHandler)
        ConstructedEvent = _
            CType( _
                [Delegate].Remove(ConstructedEvent, d), _
                    Raiser.ConstructedEventHandler)
    End Sub
End Class
```

<span data-ttu-id="5529b-469">デリゲート型を指定せずにイベントを宣言することは、最も単純で最もコンパクトな構文ですが、各イベントに対して新しいデリゲート型を宣言するという欠点があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-469">Declaring an event without specifying a delegate type is the simplest and most compact syntax, but has the disadvantage of declaring a new delegate type for each event.</span></span> <span data-ttu-id="5529b-470">たとえば、次の例では、3つの非表示のデリゲート型が作成されていますが、3つのイベントすべてに同じパラメーターリストがあります。</span><span class="sxs-lookup"><span data-stu-id="5529b-470">For example, in the following example, three hidden delegate types are created, even though all three events have the same parameter list:</span></span>

```vb
Public Class Button
    Public Event Click(sender As Object, e As EventArgs)
    Public Event DoubleClick(sender As Object, e As EventArgs)
    Public Event RightClick(sender As Object, e As EventArgs)
End Class
```

<span data-ttu-id="5529b-471">次の例では、イベントが同じデリゲートを使用するだけで、`EventHandler`ます。</span><span class="sxs-lookup"><span data-stu-id="5529b-471">In the following example, the events simply use the same delegate, `EventHandler`:</span></span>

```vb
Public Delegate Sub EventHandler(sender As Object, e As EventArgs)

Public Class Button
    Public Event Click As EventHandler
    Public Event DoubleClick As EventHandler
    Public Event RightClick As EventHandler
End Class
```

<span data-ttu-id="5529b-472">イベントは、静的または動的の2つの方法のいずれかで処理できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-472">Events can be handled in one of two ways: statically or dynamically.</span></span> <span data-ttu-id="5529b-473">イベントを静的に処理するのは簡単であり、`WithEvents` の変数と `Handles` 句のみが必要です。</span><span class="sxs-lookup"><span data-stu-id="5529b-473">Statically handling events is simpler and only requires a `WithEvents` variable and a `Handles` clause.</span></span> <span data-ttu-id="5529b-474">次の例では、クラス `Form1` がオブジェクト `Button`のイベント `Click` を静的に処理します。</span><span class="sxs-lookup"><span data-stu-id="5529b-474">In the following example, class `Form1` statically handles the event `Click` of object `Button`:</span></span>

```vb
Public Class Form1
    Public WithEvents Button1 As New Button()

    Public Sub Button1_Click(sender As Object, e As EventArgs) _
           Handles Button1.Click
        Console.WriteLine("Button1 was clicked!")
    End Sub
End Class
```

<span data-ttu-id="5529b-475">イベントは、明示的に接続してコード内に接続解除する必要があるため、動的にイベントを処理することはより複雑になります。</span><span class="sxs-lookup"><span data-stu-id="5529b-475">Dynamically handling events is more complex because the event must be explicitly connected and disconnected to in code.</span></span> <span data-ttu-id="5529b-476">ステートメント `AddHandler` イベントのハンドラーを追加し、ステートメント `RemoveHandler` イベントのハンドラーを削除します。</span><span class="sxs-lookup"><span data-stu-id="5529b-476">The statement `AddHandler` adds a handler for an event, and the statement `RemoveHandler` removes a handler for an event.</span></span> <span data-ttu-id="5529b-477">次の例は、`Button1`の `Click` イベントのイベントハンドラーとして `Button1_Click` を追加するクラス `Form1` を示しています。</span><span class="sxs-lookup"><span data-stu-id="5529b-477">The next example shows a class `Form1` that adds `Button1_Click` as an event handler for `Button1`'s `Click` event:</span></span>

```vb
Public Class Form1
    Public Sub New()
        ' Add Button1_Click as an event handler for Button1's Click event.
        AddHandler Button1.Click, AddressOf Button1_Click
    End Sub 

    Private Button1 As Button = New Button()

    Sub Button1_Click(sender As Object, e As EventArgs)
        Console.WriteLine("Button1 was clicked!")
    End Sub

    Public Sub Disconnect()
        RemoveHandler Button1.Click, AddressOf Button1_Click
    End Sub 
End Class
```

<span data-ttu-id="5529b-478">メソッド `Disconnect`では、イベントハンドラーは削除されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-478">In method `Disconnect`, the event handler is removed.</span></span>


### <a name="custom-events"></a><span data-ttu-id="5529b-479">カスタムイベント</span><span class="sxs-lookup"><span data-stu-id="5529b-479">Custom Events</span></span>

<span data-ttu-id="5529b-480">前のセクションで説明したように、イベント宣言では、フィールド、`add_` メソッド、およびイベントハンドラーの追跡に使用する `remove_` メソッドを暗黙的に定義します。</span><span class="sxs-lookup"><span data-stu-id="5529b-480">As discussed in the previous section, event declarations implicitly define a field, an `add_` method, and a `remove_` method that are used to keep track of event handlers.</span></span> <span data-ttu-id="5529b-481">ただし、状況によっては、イベントハンドラーを追跡するためのカスタムコードを提供することが望ましい場合があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-481">In some situations, however, it may be desirable to provide custom code for tracking event handlers.</span></span> <span data-ttu-id="5529b-482">たとえば、クラスが、少数の処理のみを実行する40イベントを定義する場合、各イベントのハンドラーを追跡するために、40フィールドではなくハッシュテーブルを使用する方が効率的な場合があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-482">For example, if a class defines forty events of which only a few will ever be handled, using a hash table instead of forty fields to track the handlers for each event may be more efficient.</span></span> <span data-ttu-id="5529b-483">*カスタムイベント*を使用すると、`add_X` および `remove_X` メソッドを明示的に定義できます。これにより、イベントハンドラーのカスタムストレージが有効になります。</span><span class="sxs-lookup"><span data-stu-id="5529b-483">*Custom events* allow the `add_X` and `remove_X` methods to be defined explicitly, which enables custom storage for event handlers.</span></span>

<span data-ttu-id="5529b-484">カスタムイベントは、デリゲート型を指定するイベントが宣言されるのと同じ方法で宣言されます。ただし、キーワード `Custom` `Event` キーワードの前に記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-484">Custom events are declared in the same way that events that specify a delegate type are declared, with the exception that the keyword `Custom` must precede the `Event` keyword.</span></span> <span data-ttu-id="5529b-485">カスタムイベント宣言には、`AddHandler` 宣言、`RemoveHandler` 宣言、および `RaiseEvent` 宣言という3つの宣言が含まれています。</span><span class="sxs-lookup"><span data-stu-id="5529b-485">A custom event declaration contains three declarations: an `AddHandler` declaration, a `RemoveHandler` declaration and a `RaiseEvent` declaration.</span></span> <span data-ttu-id="5529b-486">どの宣言も修飾子を持つことはできませんが、属性を持つことはできます。</span><span class="sxs-lookup"><span data-stu-id="5529b-486">None of the declarations can have any modifiers, although they can have attributes.</span></span>

```antlr
CustomEventMemberDeclaration
    : Attributes? EventModifiers* 'Custom' 'Event'
      Identifier 'As' TypeName ImplementsClause? StatementTerminator
      EventAccessorDeclaration+
      'End' 'Event' StatementTerminator
    ;

EventAccessorDeclaration
    : AddHandlerDeclaration
    | RemoveHandlerDeclaration
    | RaiseEventDeclaration
    ;

AddHandlerDeclaration
    : Attributes? 'AddHandler'
      OpenParenthesis ParameterList CloseParenthesis LineTerminator
      Block?
      'End' 'AddHandler' StatementTerminator
    ;

RemoveHandlerDeclaration
    : Attributes? 'RemoveHandler'
      OpenParenthesis ParameterList CloseParenthesis LineTerminator
      Block?
      'End' 'RemoveHandler' StatementTerminator
    ;

RaiseEventDeclaration
    : Attributes? 'RaiseEvent'
      OpenParenthesis ParameterList CloseParenthesis LineTerminator
      Block?
      'End' 'RaiseEvent' StatementTerminator
    ;
```

<span data-ttu-id="5529b-487">例 :</span><span class="sxs-lookup"><span data-stu-id="5529b-487">For example:</span></span>

```vb
Class Test
    Private Handlers As EventHandler

    Public Custom Event TestEvent As EventHandler
        AddHandler(value As EventHandler)
            Handlers = CType([Delegate].Combine(Handlers, value), _
                EventHandler)
        End AddHandler

        RemoveHandler(value as EventHandler)
            Handlers = CType([Delegate].Remove(Handlers, value), _
                EventHandler)
        End RemoveHandler

        RaiseEvent(sender As Object, e As EventArgs)
            Dim TempHandlers As EventHandler = Handlers

            If TempHandlers IsNot Nothing Then
                TempHandlers(sender, e)
            End If
        End RaiseEvent
    End Event
End Class
```

<span data-ttu-id="5529b-488">`AddHandler` と `RemoveHandler` の宣言は、1つの `ByVal` パラメーターを受け取ります。このパラメーターは、イベントのデリゲート型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-488">The `AddHandler` and `RemoveHandler` declaration take one `ByVal` parameter, which must be of the delegate type of the event.</span></span> <span data-ttu-id="5529b-489">`AddHandler` または `RemoveHandler` ステートメントが実行される (または `Handles` 句によってイベントが自動的に処理される) と、対応する宣言が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-489">When an `AddHandler` or `RemoveHandler` statement is executed (or a `Handles` clause automatically handles an event), the corresponding declaration will be called.</span></span> <span data-ttu-id="5529b-490">`RaiseEvent` 宣言は、イベントデリゲートと同じパラメーターを受け取り、`RaiseEvent` ステートメントの実行時に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-490">The `RaiseEvent` declaration takes the same parameters as the event delegate and will be called when a `RaiseEvent` statement is executed.</span></span> <span data-ttu-id="5529b-491">すべての宣言を指定する必要があり、サブルーチンと見なされます。</span><span class="sxs-lookup"><span data-stu-id="5529b-491">All of the declarations must be provided and are considered to be subroutines.</span></span>

<span data-ttu-id="5529b-492">`AddHandler`、`RemoveHandler`、および `RaiseEvent` の宣言には、サブルーチンが持つ行の配置に対して同じ制限があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="5529b-492">Note that `AddHandler`, `RemoveHandler` and `RaiseEvent` declarations have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="5529b-493">開始ステートメント、終了ステートメント、およびブロックはすべて論理行の先頭に記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-493">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>

<span data-ttu-id="5529b-494">カスタムイベント宣言は、型の宣言空間に追加されたメンバー名に加えて、他のいくつかのメンバーを暗黙的に宣言します。</span><span class="sxs-lookup"><span data-stu-id="5529b-494">In addition to the member name added to the type's declaration space, a custom event declaration implicitly declares several other members.</span></span> <span data-ttu-id="5529b-495">`X`という名前のイベントを指定すると、次のメンバーが宣言領域に追加されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-495">Given an event named `X`, the following members are added to the declaration space:</span></span>

* <span data-ttu-id="5529b-496">`AddHandler` 宣言に対応する、`add_X`という名前のメソッド。</span><span class="sxs-lookup"><span data-stu-id="5529b-496">A method named `add_X`, corresponding to the `AddHandler` declaration.</span></span>

* <span data-ttu-id="5529b-497">`RemoveHandler` 宣言に対応する、`remove_X`という名前のメソッド。</span><span class="sxs-lookup"><span data-stu-id="5529b-497">A method named `remove_X`, corresponding to the `RemoveHandler` declaration.</span></span>

* <span data-ttu-id="5529b-498">`RaiseEvent` 宣言に対応する、`fire_X`という名前のメソッド。</span><span class="sxs-lookup"><span data-stu-id="5529b-498">A method named `fire_X`, corresponding to the `RaiseEvent` declaration.</span></span>

<span data-ttu-id="5529b-499">型が上記のいずれかの名前と一致する名前を宣言しようとすると、コンパイル時エラーが発生し、暗黙的な宣言はすべて名前のバインドのために無視されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-499">If a type attempts to declare a name that matches one of the above names, a compile-time error will result, and the implicit declarations are all ignored for the purposes of name binding.</span></span> <span data-ttu-id="5529b-500">導入されたメンバーをオーバーライドまたはオーバーロードすることはできませんが、派生型でシャドウすることはできます。</span><span class="sxs-lookup"><span data-stu-id="5529b-500">It is not possible to override or overload any of the introduced members, although it is possible to shadow them in derived types.</span></span>

<span data-ttu-id="5529b-501">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="5529b-501">__Note.__</span></span> <span data-ttu-id="5529b-502">`Custom` は予約語ではありません。</span><span class="sxs-lookup"><span data-stu-id="5529b-502">`Custom` is not a reserved word.</span></span>

#### <a name="custom-events-in-winrt-assemblies"></a><span data-ttu-id="5529b-503">WinRT アセンブリのカスタムイベント</span><span class="sxs-lookup"><span data-stu-id="5529b-503">Custom events in WinRT assemblies</span></span>

<span data-ttu-id="5529b-504">Microsoft Visual Basic 11.0 の場合、`/target:winmdobj`でコンパイルされたファイルで宣言されたイベント、またはそのようなファイル内のインターフェイスで宣言されたイベントは、多少異なる方法で処理されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-504">As of Microsoft Visual Basic 11.0, events declared in a file compiled with `/target:winmdobj`, or declared in an interface in such a file and then implemented elsewhere, are treated a little differently.</span></span>

* <span data-ttu-id="5529b-505">Winmd を構築するために使用される外部ツールでは、通常、`System.EventHandler(Of T)` や `System.TypedEventHandle(Of T, U)`などの特定のデリゲート型のみが許可され、他の型は許可されません。</span><span class="sxs-lookup"><span data-stu-id="5529b-505">External tools used to build the winmd will typically allow only certain delegate types such as `System.EventHandler(Of T)` or `System.TypedEventHandle(Of T, U)`, and will disallow others.</span></span>

* <span data-ttu-id="5529b-506">`XEvent` フィールドには `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationTokenTable(Of T)` 型があり、`T` はデリゲート型です。</span><span class="sxs-lookup"><span data-stu-id="5529b-506">The `XEvent` field has type `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationTokenTable(Of T)` where `T` is the delegate type.</span></span>

* <span data-ttu-id="5529b-507">AddHandler アクセサーは `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationToken`を返し、RemoveHandler アクセサーは同じ型の1つのパラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="5529b-507">The AddHandler accessor returns a `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationToken`, and the RemoveHandler accessor takes a single parameter of the same type.</span></span>

<span data-ttu-id="5529b-508">このようなカスタムイベントの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="5529b-508">Here is an example of such a custom event.</span></span>

```vb
Imports System.Runtime.InteropServices.WindowsRuntime

Public NotInheritable Class ClassInWinMD
    Private XEvent As EventRegistrationTokenTable(Of EventHandler(Of Integer))

    Public Custom Event X As EventHandler(Of Integer)
        AddHandler(handler As EventHandler(Of Integer))
            Return EventRegistrationTokenTable(Of EventHandler(Of Integer)).
                   GetOrCreateEventRegistrationTokenTable(XEvent).
                   AddEventHandler(handler)
        End AddHandler

        RemoveHandler(token As EventRegistrationToken)
            EventRegistrationTokenTable(Of EventHandler(Of Integer)).
                GetOrCreateEventRegistrationTokenTable(XEvent).
                RemoveEventHandler(token)
        End RemoveHandler

        RaiseEvent(sender As Object, i As Integer)
            Dim table = EventRegistrationTokenTable(Of EventHandler(Of Integer)).
                GetOrCreateEventRegistrationTokenTable(XEvent).
                InvocationList
            If table IsNot Nothing Then table(sender, i)
        End RaiseEvent
    End Event
End Class
```


## <a name="constants"></a><span data-ttu-id="5529b-509">定数</span><span class="sxs-lookup"><span data-stu-id="5529b-509">Constants</span></span>

<span data-ttu-id="5529b-510">*定数*は、型のメンバーである定数値です。</span><span class="sxs-lookup"><span data-stu-id="5529b-510">A *constant* is a constant value that is a member of a type.</span></span>

```antlr
ConstantMemberDeclaration
    : Attributes? ConstantModifier* 'Const' ConstantDeclarators StatementTerminator
    ;

ConstantModifier
    : AccessModifier
    | 'Shadows'
    ;

ConstantDeclarators
    : ConstantDeclarator ( Comma ConstantDeclarator )*
    ;

ConstantDeclarator
    : Identifier ( 'As' TypeName )? Equals ConstantExpression StatementTerminator
    ;
```

<span data-ttu-id="5529b-511">定数は暗黙的に共有されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-511">Constants are implicitly shared.</span></span> <span data-ttu-id="5529b-512">宣言に `As` 句が含まれている場合、句は、宣言によって導入されたメンバーの型を指定します。</span><span class="sxs-lookup"><span data-stu-id="5529b-512">If the declaration contains an `As` clause, the clause specifies the type of the member introduced by the declaration.</span></span> <span data-ttu-id="5529b-513">型を省略した場合、定数の型は推論されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-513">If the type is omitted then the type of the constant is inferred.</span></span> <span data-ttu-id="5529b-514">定数の型は、プリミティブ型または `Object`である必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-514">The type of a constant may only be a primitive type or `Object`.</span></span> <span data-ttu-id="5529b-515">定数が `Object` として型指定されていて、型文字がない場合、定数の実際の型は定数式の型になります。</span><span class="sxs-lookup"><span data-stu-id="5529b-515">If a constant is typed as `Object` and there is no type character, the real type of the constant will be the type of the constant expression.</span></span> <span data-ttu-id="5529b-516">それ以外の場合、定数の型は、定数の型文字の型になります。</span><span class="sxs-lookup"><span data-stu-id="5529b-516">Otherwise, the type of the constant is the type of the constant's type character.</span></span>

<span data-ttu-id="5529b-517">次の例は、2つのパブリック定数を持つ `Constants` という名前のクラスを示しています。</span><span class="sxs-lookup"><span data-stu-id="5529b-517">The following example shows a class named `Constants` that has two public constants:</span></span>

```vb
Class Constants
    Public Const A As Integer = 1
    Public Const B As Integer = A + 1
End Class
```

<span data-ttu-id="5529b-518">定数は、次の例のように、クラスを使用してアクセスできます。この例では、`Constants.A` と `Constants.B`の値を出力します。</span><span class="sxs-lookup"><span data-stu-id="5529b-518">Constants can be accessed through the class, as in the following example, which prints out the values of `Constants.A` and `Constants.B`.</span></span>

```vb
Module Test
    Sub Main()
        Console.WriteLine(Constants.A & ", " & Constants.B)
    End Sub 
End Module
```

<span data-ttu-id="5529b-519">複数の定数を宣言する定数宣言は、1つの定数の複数の宣言と同じです。</span><span class="sxs-lookup"><span data-stu-id="5529b-519">A constant declaration that declares multiple constants is equivalent to multiple declarations of single constants.</span></span> <span data-ttu-id="5529b-520">次の例では、1つの宣言ステートメントで3つの定数を宣言しています。</span><span class="sxs-lookup"><span data-stu-id="5529b-520">The following example declares three constants in one declaration statement.</span></span>

```vb
Class A
    Protected Const x As Integer = 1, y As Long = 2, z As Short = 3
End Class
```

<span data-ttu-id="5529b-521">この宣言は、次の場合と同じです。</span><span class="sxs-lookup"><span data-stu-id="5529b-521">This declaration is equivalent to the following:</span></span>

```vb
Class A
    Protected Const x As Integer = 1
    Protected Const y As Long = 2
    Protected Const z As Short = 3
End Class
```

<span data-ttu-id="5529b-522">定数の型のアクセシビリティドメインは、定数自体のアクセシビリティドメインのスーパーセットと同じであるか、またはそのスーパーセットと同じである必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-522">The accessibility domain of the type of the constant must be the same as or a superset of the accessibility domain of the constant itself.</span></span> <span data-ttu-id="5529b-523">定数式は、定数の型の値、または定数の型に暗黙的に変換できる型の値を生成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-523">The constant expression must yield a value of the constant's type or of a type that is implicitly convertible to the constant's type.</span></span> <span data-ttu-id="5529b-524">定数式を循環させることはできません。つまり、定数を定義することはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-524">The constant expression may not be circular; that is, a constant may not be defined in terms of itself.</span></span>

<span data-ttu-id="5529b-525">コンパイラは、定数宣言を適切な順序で自動的に評価します。</span><span class="sxs-lookup"><span data-stu-id="5529b-525">The compiler automatically evaluates the constant declarations in the appropriate order.</span></span> <span data-ttu-id="5529b-526">次の例では、コンパイラは最初に `Y`を評価し、次に `Z`、最後に `X`を評価して、それぞれ10、11、および12の値を生成します。</span><span class="sxs-lookup"><span data-stu-id="5529b-526">In the following example, the compiler first evaluates `Y`, then `Z`, and finally `X`, producing the values 10, 11, and 12, respectively.</span></span>

```vb
Class A
    Public Const X As Integer = B.Z + 1
    Public Const Y As Integer = 10
End Class

Class B
    Public Const Z As Integer = A.Y + 1
End Class
```

<span data-ttu-id="5529b-527">定数値のシンボル名が必要であるが、定数宣言で値の型が許可されていない場合、またはコンパイル時に定数式で値を計算できない場合は、代わりに読み取り専用の変数を使用できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-527">When a symbolic name for a constant value is desired, but the type of the value is not permitted in a constant declaration or when the value cannot be computed at compile time by a constant expression, a read-only variable may be used instead.</span></span>


## <a name="instance-and-shared-variables"></a><span data-ttu-id="5529b-528">インスタンスと共有変数</span><span class="sxs-lookup"><span data-stu-id="5529b-528">Instance and Shared Variables</span></span>

<span data-ttu-id="5529b-529">インスタンスまたは共有変数は、情報を格納できる型のメンバーです。</span><span class="sxs-lookup"><span data-stu-id="5529b-529">An instance or shared variable is a member of a type that can store information.</span></span>

```antlr
VariableMemberDeclaration
    : Attributes? VariableModifier+ VariableDeclarators StatementTerminator
    ;

VariableModifier
    : AccessModifier
    | 'Shadows'
    | 'Shared'
    | 'ReadOnly'
    | 'WithEvents'
    | 'Dim'
    ;

VariableDeclarators
    : VariableDeclarator ( Comma VariableDeclarator )*
    ;

VariableDeclarator
    : VariableIdentifiers 'As' ObjectCreationExpression
    | VariableIdentifiers ( 'As' TypeName )? ( Equals Expression )?
    ;

VariableIdentifiers
    : VariableIdentifier ( Comma VariableIdentifier )*
    ;

VariableIdentifier
    : Identifier IdentifierModifiers
    ;
```

<span data-ttu-id="5529b-530">修飾子が指定されていない場合は、`Dim` 修飾子を指定する必要がありますが、それ以外の場合は省略できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-530">The `Dim` modifier must be specified if no modifiers are specified, but may be omitted otherwise.</span></span> <span data-ttu-id="5529b-531">単一の変数宣言には、複数の変数宣言子を含めることができます。各変数宣言子は、新しいインスタンスまたは共有メンバーを導入します。</span><span class="sxs-lookup"><span data-stu-id="5529b-531">A single variable declaration may include multiple variable declarators; each variable declarator introduces a new instance or shared member.</span></span>

<span data-ttu-id="5529b-532">初期化子が指定されている場合は、変数宣言子で1つのインスタンスまたは共有変数のみを宣言できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-532">If an initializer is specified, only one instance or shared variable may be declared by the variable declarator:</span></span>

```vb
Class Test
    Dim a, b, c, d As Integer = 10  ' Invalid: multiple initialization
End Class
```

<span data-ttu-id="5529b-533">この制限は、オブジェクト初期化子には適用されません。</span><span class="sxs-lookup"><span data-stu-id="5529b-533">This restriction does not apply to object initializers:</span></span>

```vb
Class Test
    Dim a, b, c, d As New Collection() ' OK
End Class
```

<span data-ttu-id="5529b-534">`Shared` 修飾子で宣言された変数は*共有変数*です。</span><span class="sxs-lookup"><span data-stu-id="5529b-534">A variable declared with the `Shared` modifier is a *shared variable*.</span></span> <span data-ttu-id="5529b-535">共有変数は、作成される型のインスタンスの数に関係なく、ストレージの場所を1つだけ識別します。</span><span class="sxs-lookup"><span data-stu-id="5529b-535">A shared variable identifies exactly one storage location regardless of the number of instances of the type that are created.</span></span> <span data-ttu-id="5529b-536">共有変数は、プログラムの実行開始時に存在し、プログラムの終了時には存在しなくなります。</span><span class="sxs-lookup"><span data-stu-id="5529b-536">A shared variable comes into existence when a program begins executing, and ceases to exist when the program terminates.</span></span>

<span data-ttu-id="5529b-537">共有変数は、特定のクローズジェネリック型のインスタンス間でのみ共有されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-537">A shared variable is shared only among instances of a particular closed generic type.</span></span> <span data-ttu-id="5529b-538">たとえば、次のようなプログラムがあるとします。</span><span class="sxs-lookup"><span data-stu-id="5529b-538">For example, the program:</span></span>

```vb
Class C(Of V) 
    Shared InstanceCount As Integer = 0

    Public Sub New()  
        InstanceCount += 1 
    End Sub

    Public Shared ReadOnly Property Count() As Integer 
        Get
            Return InstanceCount
        End Get
    End Property
End Class

Class Application 
    Shared Sub Main() 
        Dim x1 As New C(Of Integer)()
        Console.WriteLine(C(Of Integer).Count)

        Dim x2 As New C(Of Double)() 
        Console.WriteLine(C(Of Integer).Count)

        Dim x3 As New C(Of Integer)() 
        Console.WriteLine(C(Of Integer).Count)
    End Sub
End Class
```

<span data-ttu-id="5529b-539">印刷する:</span><span class="sxs-lookup"><span data-stu-id="5529b-539">Prints out:</span></span>

```console
1
1
2
```

<span data-ttu-id="5529b-540">`Shared` 修飾子なしで宣言された変数は、*インスタンス変数*と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="5529b-540">A variable declared without the `Shared` modifier is called an *instance variable*.</span></span> <span data-ttu-id="5529b-541">クラスのすべてのインスタンスには、クラスのすべてのインスタンス変数の個別のコピーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="5529b-541">Every instance of a class contains a separate copy of all instance variables of the class.</span></span> <span data-ttu-id="5529b-542">参照型のインスタンス変数は、その型の新しいインスタンスが作成されるときに存在し、そのインスタンスへの参照がなく、`Finalize` メソッドが実行されたときに存在しなくなります。</span><span class="sxs-lookup"><span data-stu-id="5529b-542">An instance variable of a reference type comes into existence when a new instance of that type is created, and ceases to exist when there are no references to that instance and the `Finalize` method has executed.</span></span> <span data-ttu-id="5529b-543">値型のインスタンス変数は、そのインスタンスが属する変数と正確に同じ有効期間を持ちます。</span><span class="sxs-lookup"><span data-stu-id="5529b-543">An instance variable of a value type has exactly the same lifetime as the variable to which it belongs.</span></span> <span data-ttu-id="5529b-544">つまり、値型の変数が存在する場合、または存在しなくなった場合は、値型のインスタンス変数が発生します。</span><span class="sxs-lookup"><span data-stu-id="5529b-544">In other words, when a variable of a value type comes into existence or ceases to exist, so does the instance variable of the value type.</span></span>

<span data-ttu-id="5529b-545">宣言子に `As` 句が含まれている場合、句は、宣言によって導入されるメンバーの型を指定します。</span><span class="sxs-lookup"><span data-stu-id="5529b-545">If the declarator contains an `As` clause, the clause specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="5529b-546">型が省略され、厳密なセマンティクスが使用されている場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="5529b-546">If the type is omitted and strict semantics are being used, a compile-time error occurs.</span></span> <span data-ttu-id="5529b-547">それ以外の場合、メンバーの型は暗黙的に `Object` か、メンバーの型文字の型になります。</span><span class="sxs-lookup"><span data-stu-id="5529b-547">Otherwise the type of the members is implicitly `Object` or the type of the members' type character.</span></span>

<span data-ttu-id="5529b-548">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="5529b-548">__Note.__</span></span> <span data-ttu-id="5529b-549">構文にあいまいさはありません。宣言子が型を省略した場合、常に次の宣言子の型が使用されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-549">There is no ambiguity in the syntax: if a declarator omits a type, it will always use the type of a following declarator.</span></span>

<span data-ttu-id="5529b-550">インスタンスまたは共有変数の型または配列要素型のアクセシビリティドメインは、インスタンスまたは共有変数自体のアクセシビリティドメインのスーパーセットと同じであるか、またはそのスーパーセットと同じである必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-550">The accessibility domain of an instance or shared variable's type or array element type must be the same as or a superset of the accessibility domain of the instance or shared variable itself.</span></span>

<span data-ttu-id="5529b-551">次の例は、`redPart`、`greenPart`、および `bluePart`という名前の内部インスタンス変数を持つ `Color` クラスを示しています。</span><span class="sxs-lookup"><span data-stu-id="5529b-551">The following example shows a `Color` class that has internal instance variables named `redPart`, `greenPart`, and `bluePart`:</span></span>

```vb
Class Color
    Friend redPart As Short
    Friend bluePart As Short
    Friend greenPart As Short

    Public Sub New(red As Short, blue As Short, green As Short)
        redPart = red
        bluePart = blue
        greenPart = green
    End Sub
End Class
```


### <a name="read-only-variables"></a><span data-ttu-id="5529b-552">読み取り専用変数</span><span class="sxs-lookup"><span data-stu-id="5529b-552">Read-Only Variables</span></span>

<span data-ttu-id="5529b-553">インスタンスまたは共有変数の宣言に `ReadOnly` 修飾子が含まれている場合、宣言によって導入された変数への代入は、宣言の一部として、または同じクラスのコンストラクター内でのみ発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-553">When an instance or shared variable declaration includes a `ReadOnly` modifier, assignments to the variables introduced by the declaration may only occur as part of the declaration or in a constructor in the same class.</span></span> <span data-ttu-id="5529b-554">具体的には、読み取り専用インスタンスまたは共有変数への割り当ては、次の状況でのみ許可されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-554">Specifically, assignments to a read-only instance or shared variable are permitted only in the following situations:</span></span>

* <span data-ttu-id="5529b-555">(宣言に変数初期化子を含めることによって) インスタンスまたは共有変数を導入する変数宣言。</span><span class="sxs-lookup"><span data-stu-id="5529b-555">In the variable declaration that introduces the instance or shared variable (by including a variable initializer in the declaration).</span></span>

* <span data-ttu-id="5529b-556">インスタンス変数の場合は、変数宣言を含むクラスのインスタンスコンストラクター。</span><span class="sxs-lookup"><span data-stu-id="5529b-556">For an instance variable, in the instance constructors of the class that contains the variable declaration.</span></span> <span data-ttu-id="5529b-557">インスタンス変数にアクセスできるのは、非修飾の方法、または `Me` または `MyClass`を使用した場合のみです。</span><span class="sxs-lookup"><span data-stu-id="5529b-557">The instance variable can only be accessed in an unqualified manner or through `Me` or `MyClass`.</span></span>

* <span data-ttu-id="5529b-558">共有変数の場合は、共有変数宣言を含むクラスの共有コンストラクター。</span><span class="sxs-lookup"><span data-stu-id="5529b-558">For a shared variable, in the shared constructor of the class that contains the shared variable declaration.</span></span>

<span data-ttu-id="5529b-559">共有読み取り専用変数は、定数値のシンボル名が必要な場合、定数宣言で値の型が許可されていない場合、またはコンパイル時に定数式によって値を計算できない場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="5529b-559">A shared read-only variable is useful when a symbolic name for a constant value is desired, but when the type of the value is not permitted in a constant declaration, or when the value cannot be computed at compile time by a constant expression.</span></span>

<span data-ttu-id="5529b-560">最初のアプリケーションの例は次のようになります。色共有変数は、他のプログラムによって変更されないように `ReadOnly` として宣言されています。</span><span class="sxs-lookup"><span data-stu-id="5529b-560">An example of the first such application follows, in which color shared variables are declared `ReadOnly` to prevent them from being changed by other programs:</span></span>

```vb
Class Color
    Friend redPart As Short
    Friend bluePart As Short
    Friend greenPart As Short

    Public Sub New(red As Short, blue As Short, green As Short)
        redPart = red
        bluePart = blue
        greenPart = green
    End Sub 

    Public Shared ReadOnly Red As Color = New Color(&HFF, 0, 0)
    Public Shared ReadOnly Blue As Color = New Color(0, &HFF, 0)
    Public Shared ReadOnly Green As Color = New Color(0, 0, &HFF)
    Public Shared ReadOnly White As Color = New Color(&HFF, &HFF, &HFF)
End Class
```

<span data-ttu-id="5529b-561">定数と読み取り専用の共有変数のセマンティクスは異なります。</span><span class="sxs-lookup"><span data-stu-id="5529b-561">Constants and read-only shared variables have different semantics.</span></span> <span data-ttu-id="5529b-562">式が定数を参照する場合、定数の値はコンパイル時に取得されますが、式が読み取り専用の共有変数を参照している場合、共有変数の値は実行時まで取得されません。</span><span class="sxs-lookup"><span data-stu-id="5529b-562">When an expression references a constant, the value of the constant is obtained at compile time, but when an expression references a read-only shared variable, the value of the shared variable is not obtained until run time.</span></span> <span data-ttu-id="5529b-563">次のアプリケーションを考えてみます。このアプリケーションは、2つの異なるプログラムで構成されています。</span><span class="sxs-lookup"><span data-stu-id="5529b-563">Consider the following application, which consists of two separate programs.</span></span>

<span data-ttu-id="5529b-564">file1. vb:</span><span class="sxs-lookup"><span data-stu-id="5529b-564">file1.vb:</span></span>

```vb
Namespace Program1
    Public Class Utils
        Public Shared ReadOnly X As Integer = 1
    End Class
End Namespace
```

<span data-ttu-id="5529b-565">file2 .vb:</span><span class="sxs-lookup"><span data-stu-id="5529b-565">file2.vb:</span></span>

```vb
Namespace Program2
    Module Test
        Sub Main()
            Console.WriteLine(Program1.Utils.X)
        End Sub
    End Module
End Namespace
```

<span data-ttu-id="5529b-566">名前空間 `Program1` と `Program2` は、個別にコンパイルされる2つのプログラムを表します。</span><span class="sxs-lookup"><span data-stu-id="5529b-566">The namespaces `Program1` and `Program2` denote two programs that are compiled separately.</span></span> <span data-ttu-id="5529b-567">変数 `Program1.Utils.X` は `Shared ReadOnly`として宣言されているため、`Console.WriteLine` ステートメントによって出力される値はコンパイル時には認識されませんが、実行時に取得されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-567">Because variable `Program1.Utils.X` is declared as `Shared ReadOnly`, the value output by the `Console.WriteLine` statement is not known at compile time, but rather is obtained at run time.</span></span> <span data-ttu-id="5529b-568">したがって、`X` の値が変更され `Program1` 再コンパイルされると、`Program2` が再コンパイルされない場合でも、`Console.WriteLine` ステートメントによって新しい値が出力されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-568">Thus, if the value of `X` is changed and `Program1` is recompiled, the `Console.WriteLine` statement will output the new value even if `Program2` is not recompiled.</span></span> <span data-ttu-id="5529b-569">ただし、`X` が定数であった場合、`X` の値は `Program2` のコンパイル時に取得され、`Program2` が再コンパイルされるまで `Program1` の変更による影響を受けません。</span><span class="sxs-lookup"><span data-stu-id="5529b-569">However, if `X` had been a constant, the value of `X` would have been obtained at the time `Program2` was compiled, and would have remained unaffected by changes in `Program1` until `Program2` was recompiled.</span></span>

### <a name="withevents-variables"></a><span data-ttu-id="5529b-570">WithEvents 変数</span><span class="sxs-lookup"><span data-stu-id="5529b-570">WithEvents Variables</span></span>

<span data-ttu-id="5529b-571">型は、`WithEvents` 修飾子を使用してイベントを発生させるインスタンスまたは共有変数を宣言することで、インスタンスまたは共有変数のいずれかによって発生するイベントのセットを処理することを宣言できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-571">A type can declare that it handles some set of events raised by one of its instance or shared variables by declaring the instance or shared variable that raises the events with the `WithEvents` modifier.</span></span> <span data-ttu-id="5529b-572">例 :</span><span class="sxs-lookup"><span data-stu-id="5529b-572">For example:</span></span>

```vb
Class Raiser
    Public Event E1()

    Public Sub Raise()
        RaiseEvent E1
    End Sub
End Class

Module Test
    Private WithEvents x As Raiser

    Private Sub E1Handler() Handles x.E1
        Console.WriteLine("Raised")
    End Sub

    Public Sub Main()
        x = New Raiser()
    End Sub
End Module
```

<span data-ttu-id="5529b-573">この例では、メソッド `E1Handler` は、インスタンス変数 `x`に格納されて `Raiser` 型のインスタンスによって発生するイベント `E1` を処理します。</span><span class="sxs-lookup"><span data-stu-id="5529b-573">In this example, the method `E1Handler` handles the event `E1` that is raised by the instance of the type `Raiser` stored in the instance variable `x`.</span></span>

<span data-ttu-id="5529b-574">`WithEvents` 修飾子を指定すると、変数の名前が先頭のアンダースコアに変更され、イベントのフックと同じ名前のプロパティに置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="5529b-574">The `WithEvents` modifier causes the variable to be renamed with a leading underscore and replaced with a property of the same name that does the event hookup.</span></span> <span data-ttu-id="5529b-575">たとえば、変数の名前が `F`場合は、名前が `_F` に変更され、プロパティ `F` が暗黙的に宣言されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-575">For example, if the variable's name is `F`, it is renamed to `_F` and a property `F` is implicitly declared.</span></span> <span data-ttu-id="5529b-576">変数の新しい名前と別の宣言との間に競合がある場合は、コンパイル時エラーが報告されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-576">If there is a collision between the variable's new name and another declaration, a compile-time error will be reported.</span></span> <span data-ttu-id="5529b-577">変数に適用された属性は、名前が変更された変数に引き継がれます。</span><span class="sxs-lookup"><span data-stu-id="5529b-577">Any attributes applied to the variable are carried over to the renamed variable.</span></span>

<span data-ttu-id="5529b-578">`WithEvents` 宣言によって作成された暗黙的なプロパティは、フックを処理し、関連するイベントハンドラーをアンフック中します。</span><span class="sxs-lookup"><span data-stu-id="5529b-578">The implicit property created by a `WithEvents` declaration takes care of hooking and unhooking the relevant event handlers.</span></span> <span data-ttu-id="5529b-579">変数に値が割り当てられている場合、プロパティはまず、変数に現在あるインスタンスのイベントの `remove` メソッドを呼び出します (既存のイベントハンドラーがある場合はアンフック中)。</span><span class="sxs-lookup"><span data-stu-id="5529b-579">When a value is assigned to the variable, the property first calls the `remove` method for the event on the instance currently in the variable (unhooking the existing event handler, if any).</span></span> <span data-ttu-id="5529b-580">次に、割り当てが行われ、プロパティは、変数内の新しいインスタンスでイベントの `add` メソッドを呼び出します (新しいイベントハンドラーをフックします)。</span><span class="sxs-lookup"><span data-stu-id="5529b-580">Next the assignment is made, and the property calls the `add` method for the event on the new instance in the variable (hooking up the new event handler).</span></span> <span data-ttu-id="5529b-581">次のコードは、標準モジュール `Test`に対する上記のコードと同じです。</span><span class="sxs-lookup"><span data-stu-id="5529b-581">The following code is equivalent to the code above for the standard module `Test`:</span></span>

```vb
Module Test
    Private _x As Raiser

    Public Property x() As Raiser
        Get
            Return _x
        End Get

        Set (Value As Raiser)
            ' Unhook any existing handlers.
            If _x IsNot Nothing Then
                RemoveHandler _x.E1, AddressOf E1Handler
            End If

            ' Change value.
            _x = Value

            ' Hook-up new handlers.
            If _x IsNot Nothing Then
                AddHandler _x.E1, AddressOf E1Handler
            End If
        End Set
    End Property

    Sub E1Handler()
        Console.WriteLine("Raised")
    End Sub

    Sub Main()
        x = New Raiser()
    End Sub
End Module
```

<span data-ttu-id="5529b-582">変数が構造体として型指定されている場合、インスタンスまたは共有変数を `WithEvents` として宣言することは無効です。</span><span class="sxs-lookup"><span data-stu-id="5529b-582">It is not valid to declare an instance or shared variable as `WithEvents` if the variable is typed as a structure.</span></span> <span data-ttu-id="5529b-583">また、構造体で `WithEvents` を指定することはできません。また、`WithEvents` と `ReadOnly` を組み合わせることはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-583">In addition, `WithEvents` may not be specified in a structure, and `WithEvents` and `ReadOnly` cannot be combined.</span></span>

### <a name="variable-initializers"></a><span data-ttu-id="5529b-584">変数初期化子</span><span class="sxs-lookup"><span data-stu-id="5529b-584">Variable Initializers</span></span>

<span data-ttu-id="5529b-585">構造体のクラスおよびインスタンス変数宣言 (共有変数宣言ではありません) のインスタンスおよび共有変数宣言には、変数初期化子が含まれる場合があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-585">Instance and shared variable declarations in classes and instance variable declarations (but not shared variable declarations) in structures may include variable initializers.</span></span> <span data-ttu-id="5529b-586">`Shared` 変数の場合、変数初期化子は、プログラムの開始後、`Shared` 変数が最初に参照される前に実行される代入ステートメントに対応します。</span><span class="sxs-lookup"><span data-stu-id="5529b-586">For `Shared` variables, variable initializers correspond to assignment statements that are executed after the program begins, but before the `Shared` variable is first referenced.</span></span> <span data-ttu-id="5529b-587">インスタンス変数の場合、変数初期化子は、クラスのインスタンスが作成されるときに実行される代入ステートメントに対応します。</span><span class="sxs-lookup"><span data-stu-id="5529b-587">For instance variables, variable initializers correspond to assignment statements that are executed when an instance of the class is created.</span></span> <span data-ttu-id="5529b-588">パラメーターなしのコンストラクターは変更できないため、構造体にインスタンス変数初期化子を指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-588">Structures cannot have instance variable initializers because their parameterless constructors cannot be modified.</span></span>

<span data-ttu-id="5529b-589">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="5529b-589">Consider the following example:</span></span>

```vb
Class Test
    Public Shared x As Double = Math.Sqrt(2.0)
    Public i As Integer = 100
    Public s As String = "Hello"
End Class

Module TestModule
    Sub Main()
        Dim a As New Test()

        Console.WriteLine("x = " & Test.x & ", i = " & a.i & ", s = " & a.s)
    End Sub
End Module
```

<span data-ttu-id="5529b-590">この例では次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-590">The example produces the following output:</span></span>

```console
x = 1.4142135623731, i = 100, s = Hello
```

<span data-ttu-id="5529b-591">クラスが読み込まれたときに `x` への割り当てが発生し、クラスの新しいインスタンスが作成されるときに `i` および `s` の割り当てが発生します。</span><span class="sxs-lookup"><span data-stu-id="5529b-591">An assignment to `x` occurs when the class is loaded, and assignments to `i` and `s` occur when a new instance of the class is created.</span></span>

<span data-ttu-id="5529b-592">変数初期化子は、型のコンストラクターのブロックに自動的に挿入される代入ステートメントと考えると便利です。</span><span class="sxs-lookup"><span data-stu-id="5529b-592">It is useful to think of variable initializers as assignment statements that are automatically inserted in the block of the type's constructor.</span></span> <span data-ttu-id="5529b-593">次の例には、いくつかのインスタンス変数初期化子が含まれています。</span><span class="sxs-lookup"><span data-stu-id="5529b-593">The following example contains several instance variable initializers.</span></span>

```vb
Class A
    Private x As Integer = 1
    Private y As Integer = -1
    Private count As Integer

    Public Sub New()
        count = 0
    End Sub

    Public Sub New(n As Integer)
        count = n
    End Sub
End Class

Class B
    Inherits A

    Private sqrt2 As Double = Math.Sqrt(2.0)
    Private items As ArrayList = New ArrayList(100)
    Private max As Integer

    Public Sub New()
        Me.New(100)
        items.Add("default")
    End Sub

    Public Sub New(n As Integer)
        MyBase.New(n - 1)
        max = n
    End Sub
End Class
```

<span data-ttu-id="5529b-594">この例は、次に示すコードに対応しています。各コメントは、自動的に挿入されたステートメントを示しています。</span><span class="sxs-lookup"><span data-stu-id="5529b-594">The example corresponds to the code shown below, where each comment indicates an automatically inserted statement.</span></span>

```vb
Class A
    Private x, y, count As Integer

    Public Sub New()
        MyBase.New ' Invoke object() constructor.
        x = 1 ' This is a variable initializer.
        y = -1 ' This is a variable initializer.
        count = 0
    End Sub

    Public Sub New(n As Integer)
        MyBase.New ' Invoke object() constructor. 
        x = 1 ' This is a variable initializer.
        y = - 1 ' This is a variable initializer.
        count = n
    End Sub
End Class

Class B
    Inherits A

    Private sqrt2 As Double
    Private items As ArrayList
    Private max As Integer

    Public Sub New()
        Me.New(100) 
        items.Add("default")
    End Sub

    Public Sub New(n As Integer)
        MyBase.New(n - 1) 
        sqrt2 = Math.Sqrt(2.0) ' This is a variable initializer.
        items = New ArrayList(100) ' This is a variable initializer.
        max = n
    End Sub
End Class
```

<span data-ttu-id="5529b-595">変数初期化子が実行される前に、すべての変数がその型の既定値に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-595">All variables are initialized to the default value of their type before any variable initializers are executed.</span></span> <span data-ttu-id="5529b-596">例 :</span><span class="sxs-lookup"><span data-stu-id="5529b-596">For example:</span></span>

```vb
Class Test
    Public Shared b As Boolean
    Public i As Integer
End Class

Module TestModule
    Sub Main()
        Dim t As New Test()
        Console.WriteLine("b = " & Test.b & ", i = " & t.i)
    End Sub
End Module
```

<span data-ttu-id="5529b-597">クラスが読み込まれると `b` が既定値に自動的に初期化されるため、クラスのインスタンスが作成されるときに `i` が既定値に自動的に初期化されるため、前のコードでは次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-597">Because `b` is automatically initialized to its default value when the class is loaded and `i` is automatically initialized to its default value when an instance of the class is created, the preceding code produces the following output:</span></span>

```console
b = False, i = 0
```

<span data-ttu-id="5529b-598">各変数初期化子は、変数の型の値、または変数の型に暗黙的に変換できる型の値を生成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-598">Each variable initializer must yield a value of the variable's type or of a type that is implicitly convertible to the variable's type.</span></span> <span data-ttu-id="5529b-599">変数初期化子は、循環しているか、その後に初期化される変数を参照している可能性があります。この場合、参照先の変数の値は、初期化子のための既定値になります。</span><span class="sxs-lookup"><span data-stu-id="5529b-599">A variable initializer may be circular or refer to a variable that will be initialized after it, in which case the value of the referenced variable is its default value for the purposes of the initializer.</span></span> <span data-ttu-id="5529b-600">このような初期化子は、あいまい値です。</span><span class="sxs-lookup"><span data-stu-id="5529b-600">Such an initializer is of dubious value.</span></span>

<span data-ttu-id="5529b-601">変数初期化子には、標準初期化子、配列サイズ初期化子、およびオブジェクト初期化子の3つの形式があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-601">There are three forms of variable initializers: regular initializers, array-size initializers, and object initializers.</span></span> <span data-ttu-id="5529b-602">最初の2つの形式は、型名の後の等号の後に表示されます。後者の2つは宣言自体の一部です。</span><span class="sxs-lookup"><span data-stu-id="5529b-602">The first two forms appear after an equal sign that follows the type name, the latter two are part of the declaration itself.</span></span> <span data-ttu-id="5529b-603">特定の宣言で使用できる初期化子の形式は1つだけです。</span><span class="sxs-lookup"><span data-stu-id="5529b-603">Only one form of initializer may be used on any particular declaration.</span></span>

#### <a name="regular-initializers"></a><span data-ttu-id="5529b-604">標準初期化子</span><span class="sxs-lookup"><span data-stu-id="5529b-604">Regular Initializers</span></span>

<span data-ttu-id="5529b-605">*通常の初期化子*は、変数の型に暗黙的に変換できる式です。</span><span class="sxs-lookup"><span data-stu-id="5529b-605">A *regular initializer* is an expression that is implicitly convertible to the type of the variable.</span></span> <span data-ttu-id="5529b-606">これは、型名の後の等号の後に表示され、値として分類される必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-606">It appears after an equal sign that follows the type name and must be classified as a value.</span></span> <span data-ttu-id="5529b-607">例 :</span><span class="sxs-lookup"><span data-stu-id="5529b-607">For example:</span></span>

```vb
Module Test
    Dim x As Integer = 10
    Dim y As Integer = 20

    Sub Main()
        Console.WriteLine("x = " & x & ", y = " & y)
    End Sub
End Module
```

<span data-ttu-id="5529b-608">このプログラムでは、次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-608">This program produces the output:</span></span>

```console
x = 10, y = 20
```

<span data-ttu-id="5529b-609">変数宣言に通常の初期化子がある場合は、一度に1つの変数のみを宣言できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-609">If a variable declaration has a regular initializer, then only a single variable can be declared at a time.</span></span> <span data-ttu-id="5529b-610">例 :</span><span class="sxs-lookup"><span data-stu-id="5529b-610">For example:</span></span>

```vb
Module Test
    Sub Main()
        ' OK, only one variable declared at a time.
        Dim x As Integer = 10, y As Integer = 20

        ' Error: Can't initialize multiple variables at once.
        Dim a, b As Integer = 10
    End Sub
End Module
```

#### <a name="object-initializers"></a><span data-ttu-id="5529b-611">オブジェクト初期化子</span><span class="sxs-lookup"><span data-stu-id="5529b-611">Object Initializers</span></span>

<span data-ttu-id="5529b-612">*オブジェクト初期化子*は、型名の代わりにオブジェクト作成式を使用して指定します。</span><span class="sxs-lookup"><span data-stu-id="5529b-612">An *object initializer* is specified using an object creation expression in the place of the type name.</span></span> <span data-ttu-id="5529b-613">オブジェクト初期化子は、オブジェクト作成式の結果を変数に割り当てる標準初期化子に相当します。</span><span class="sxs-lookup"><span data-stu-id="5529b-613">An object initializer is equivalent to a regular initializer assigning the result of the object creation expression to the variable.</span></span> <span data-ttu-id="5529b-614">So</span><span class="sxs-lookup"><span data-stu-id="5529b-614">So</span></span>

```vb
Module TestModule
    Sub Main()
        Dim x As New Test(10)
    End Sub
End Module
```

<span data-ttu-id="5529b-615">上記の式は、次の式と同じです。</span><span class="sxs-lookup"><span data-stu-id="5529b-615">is equivalent to</span></span>

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test(10)
    End Sub
End Module
```

<span data-ttu-id="5529b-616">オブジェクト初期化子のかっこは、常にコンストラクターの引数リストとして解釈され、配列型修飾子としては解釈されません。</span><span class="sxs-lookup"><span data-stu-id="5529b-616">The parenthesis in an object initializer is always interpreted as the argument list for the constructor and never as array type modifiers.</span></span> <span data-ttu-id="5529b-617">オブジェクト初期化子を持つ変数名には、配列型修飾子または null 許容型修飾子を指定できません。</span><span class="sxs-lookup"><span data-stu-id="5529b-617">A variable name with an object initializer cannot have an array type modifier or a nullable type modifier.</span></span>

#### <a name="array-size-initializers"></a><span data-ttu-id="5529b-618">配列サイズ初期化子</span><span class="sxs-lookup"><span data-stu-id="5529b-618">Array-Size Initializers</span></span>

<span data-ttu-id="5529b-619">*配列サイズ初期化子*は、式によって示される次元の上限のセットを提供する変数の名前に対する修飾子です。</span><span class="sxs-lookup"><span data-stu-id="5529b-619">An *array-size initializer* is a modifier on the name of the variable that gives a set of dimension upper bounds denoted by expressions.</span></span>

```antlr
ArraySizeInitializationModifier
    : OpenParenthesis BoundList CloseParenthesis ArrayTypeModifiers?
    ;

BoundList
    : Bound ( Comma Bound )*
    ;

Bound
    : Expression
    | '0' 'To' Expression
    ;
```

<span data-ttu-id="5529b-620">上限式は、値として分類され、`Integer`に暗黙的に変換できる必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-620">The upper bound expressions must be classified as values and must be implicitly convertible to `Integer`.</span></span> <span data-ttu-id="5529b-621">上限のセットは、指定された上限を持つ配列作成式の変数初期化子に相当します。</span><span class="sxs-lookup"><span data-stu-id="5529b-621">The set of upper bounds is equivalent to a variable initializer of an array-creation expression with the given upper bounds.</span></span> <span data-ttu-id="5529b-622">配列型の次元数は、配列サイズ初期化子から推論されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-622">The number of dimensions of the array type is inferred from the array size initializer.</span></span> <span data-ttu-id="5529b-623">So</span><span class="sxs-lookup"><span data-stu-id="5529b-623">So</span></span>

```vb
Module Test
    Sub Main()
        Dim x(5, 10) As Integer
    End Sub
End Module
```

<span data-ttu-id="5529b-624">上記の式は、次の式と同じです。</span><span class="sxs-lookup"><span data-stu-id="5529b-624">is equivalent to</span></span>

```vb
Module Test
    Sub Main()
        Dim x As Integer(,) = New Integer(5, 10) {}
    End Sub
End Module
```

<span data-ttu-id="5529b-625">すべての上限は-1 以上である必要があり、すべてのディメンションには上限が指定されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-625">All upper bounds must be equal to or greater than -1, and all dimensions must have an upper bound specified.</span></span> <span data-ttu-id="5529b-626">初期化される配列の要素型がそれ自体が配列型である場合、配列型の修飾子は配列サイズ初期化子の右側になります。</span><span class="sxs-lookup"><span data-stu-id="5529b-626">If the element type of the array being initialized is itself an array type, the array-type modifiers go to the right of the array-size initializer.</span></span> <span data-ttu-id="5529b-627">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="5529b-627">For example</span></span>

```vb
Module Test
    Sub Main()
        Dim x(5,10)(,,) As Integer
    End Sub
End Module
```

<span data-ttu-id="5529b-628">型が `Integer`の3次元配列の2次元配列である `x` ローカル変数を宣言します。最初の次元の `0..5` の境界を持つ配列に初期化され、2番目の次元に `0..10` ます。</span><span class="sxs-lookup"><span data-stu-id="5529b-628">declares a local variable `x` whose type is a two-dimensional array of three-dimensional arrays of `Integer`, initialized to an array with bounds of `0..5` in the first dimension and `0..10` in the second dimension.</span></span> <span data-ttu-id="5529b-629">配列サイズ初期化子を使用して、型が配列の配列である変数の要素を初期化することはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-629">It is not possible to use an array size initializer to initialize the elements of a variable whose type is an array of arrays.</span></span>

<span data-ttu-id="5529b-630">配列サイズの初期化子を持つ変数宣言では、その型または標準の初期化子に配列型修飾子を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-630">A variable declaration with an array-size initializer cannot have an array type modifier on its type or a regular initializer.</span></span>


### <a name="systemmarshalbyrefobject-classes"></a><span data-ttu-id="5529b-631">MarshalByRefObject クラス</span><span class="sxs-lookup"><span data-stu-id="5529b-631">System.MarshalByRefObject Classes</span></span>

<span data-ttu-id="5529b-632">クラス `System.MarshalByRefObject` から派生するクラスは、(つまり、値によって) コピーするのではなく、プロキシを使用してコンテキスト境界を越えてマーシャリングされます (つまり、参照渡し)。</span><span class="sxs-lookup"><span data-stu-id="5529b-632">Classes that derive from the class `System.MarshalByRefObject` are marshaled across context boundaries using proxies (that is, by reference) rather than through copying (that is, by value).</span></span> <span data-ttu-id="5529b-633">つまり、このようなクラスのインスタンスは実際のインスタンスではなくてもかまいませんが、コンテキスト境界を越えて変数アクセスとメソッド呼び出しをマーシャリングするスタブにするだけで済みます。</span><span class="sxs-lookup"><span data-stu-id="5529b-633">This means that an instance of such a class may not be a true instance but instead may just be a stub that marshals variable accesses and method calls across a context boundary.</span></span>

<span data-ttu-id="5529b-634">そのため、このようなクラスで定義されている変数の格納場所への参照を作成することはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-634">As a result, it is not possible to create a reference to the storage location of variables defined on such classes.</span></span> <span data-ttu-id="5529b-635">つまり、`System.MarshalByRefObject` から派生したクラスとして型指定された変数を参照パラメーターに渡すことはできません。また、値型として型指定された変数のメソッドと変数にはアクセスできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-635">This means that variables typed as classes derived from `System.MarshalByRefObject` cannot be passed to reference parameters, and methods and variables of variables typed as value types may not be accessed.</span></span> <span data-ttu-id="5529b-636">代わりに、このようなクラスで定義されている変数はプロパティと同じように処理されます (制限はプロパティと同じであるため、Visual Basic ではプロパティとして扱われます)。</span><span class="sxs-lookup"><span data-stu-id="5529b-636">Instead, Visual Basic treats variables defined on such classes as if they were properties (since the restrictions are the same on properties).</span></span>

<span data-ttu-id="5529b-637">この規則には例外が1つあります。 `Me` は常にプロキシではなく実際のオブジェクトであることが保証されるため、`Me` で暗黙的または明示的に修飾されたメンバーは、上記の制限から除外されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-637">There is one exception to this rule: a member implicitly or explicitly qualified with `Me` is exempt from the above restrictions, because `Me` is always guaranteed to be an actual object, not a proxy.</span></span>

## <a name="properties"></a><span data-ttu-id="5529b-638">プロパティ</span><span class="sxs-lookup"><span data-stu-id="5529b-638">Properties</span></span>

<span data-ttu-id="5529b-639">*プロパティ*は、変数の自然な拡張です。どちらも、関連付けられた型を持つ名前付きのメンバーであり、変数およびプロパティにアクセスするための構文は同じです。</span><span class="sxs-lookup"><span data-stu-id="5529b-639">*Properties* are a natural extension of variables; both are named members with associated types, and the syntax for accessing variables and properties is the same.</span></span> <span data-ttu-id="5529b-640">ただし、変数とは異なり、プロパティはストレージの場所を意味しません。</span><span class="sxs-lookup"><span data-stu-id="5529b-640">Unlike variables, however, properties do not denote storage locations.</span></span> <span data-ttu-id="5529b-641">代わりに、プロパティに*アクセサー*があります。アクセサーは、値の読み取りまたは書き込みを行うために実行するステートメントを指定します。</span><span class="sxs-lookup"><span data-stu-id="5529b-641">Instead, properties have *accessors*, which specify the statements to execute in order to read or write their values.</span></span>

<span data-ttu-id="5529b-642">プロパティは、プロパティ宣言を使用して定義されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-642">Properties are defined with property declarations.</span></span> <span data-ttu-id="5529b-643">プロパティ宣言の最初の部分は、フィールド宣言に似ています。</span><span class="sxs-lookup"><span data-stu-id="5529b-643">The first part of a property declaration resembles a field declaration.</span></span> <span data-ttu-id="5529b-644">2番目の部分には、`Get` アクセサー、または `Set` アクセサーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="5529b-644">The second part includes a `Get` accessor and/or a `Set` accessor.</span></span>

```antlr
PropertyMemberDeclaration
    : RegularPropertyMemberDeclaration
    | MustOverridePropertyMemberDeclaration
    | AutoPropertyMemberDeclaration
    ;

PropertySignature
    : 'Property'
      Identifier ( OpenParenthesis ParameterList? CloseParenthesis )?
      ( 'As' Attributes? TypeName )?
    ;

RegularPropertyMemberDeclaration
    : Attributes? PropertyModifier* PropertySignature
      ImplementsClause? LineTerminator
      PropertyAccessorDeclaration+
      'End' 'Property' StatementTerminator
    ;

MustOverridePropertyMemberDeclaration
    : Attributes? MustOverridePropertyModifier+ PropertySignature
      ImplementsClause? StatementTerminator
    ;

AutoPropertyMemberDeclaration
    : Attributes? AutoPropertyModifier* 'Property' Identifier
      ( OpenParenthesis ParameterList? CloseParenthesis )?
      ( 'As' Attributes? TypeName )? ( Equals Expression )?
      ImplementsClause? LineTerminator
    | Attributes? AutoPropertyModifier* 'Property' Identifier
      ( OpenParenthesis ParameterList? CloseParenthesis )?
      'As' Attributes? 'New'
      ( NonArrayTypeName ( OpenParenthesis ArgumentList? CloseParenthesis )? )?
      ObjectCreationExpressionInitializer?
      ImplementsClause? LineTerminator
    ;

InterfacePropertyMemberDeclaration
    : Attributes? InterfacePropertyModifier* PropertySignature StatementTerminator
    ;

AutoPropertyModifier
    : AccessModifier
    | 'Shadows'
    | 'Shared'
    | 'Overridable'
    | 'NotOverridable'
    | 'Overrides'
    | 'Overloads'
    ;

PropertyModifier
    : AutoPropertyModifier
    | 'Default'
    | 'ReadOnly'
    | 'WriteOnly'
    | 'Iterator'
    ;

MustOverridePropertyModifier
    : PropertyModifier
    | 'MustOverride'
    ;

InterfacePropertyModifier
    : 'Shadows'
    | 'Overloads'
    | 'Default'
    | 'ReadOnly'
    | 'WriteOnly'
    ;

PropertyAccessorDeclaration
    : PropertyGetDeclaration
    | PropertySetDeclaration
    ;
```

<span data-ttu-id="5529b-645">次の例では、`Button` クラスは `Caption` プロパティを定義しています。</span><span class="sxs-lookup"><span data-stu-id="5529b-645">In the example below, the `Button` class defines a `Caption` property.</span></span>

```vb
Public Class Button
    Private captionValue As String

    Public Property Caption() As String
        Get
            Return captionValue
        End Get

        Set (Value As String)
            captionValue = value
            Repaint()
        End Set
    End Property

    ...
End Class
```

<span data-ttu-id="5529b-646">上記の `Button` クラスに基づいて、`Caption` プロパティの使用例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="5529b-646">Based on the `Button` class above, the following is an example of use of the `Caption` property:</span></span>

```vb
Dim okButton As Button = New Button()

okButton.Caption = "OK" ' Invokes Set accessor.
Dim s As String = okButton.Caption ' Invokes Get accessor.
```

<span data-ttu-id="5529b-647">ここでは、プロパティに値を割り当てることによって `Set` アクセサーが呼び出され、式のプロパティを参照することによって `Get` アクセサーが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-647">Here, the `Set` accessor is invoked by assigning a value to the property, and the `Get` accessor is invoked by referencing the property in an expression.</span></span>

<span data-ttu-id="5529b-648">プロパティに型が指定されておらず、厳密なセマンティクスが使用されている場合、コンパイル時エラーが発生します。それ以外の場合、プロパティの型は暗黙的に `Object` か、またはプロパティの型文字の型になります。</span><span class="sxs-lookup"><span data-stu-id="5529b-648">If no type is specified for a property and strict semantics are being used, a compile-time error occurs; otherwise the type of the property is implicitly `Object` or the type of the property's type character.</span></span> <span data-ttu-id="5529b-649">プロパティの宣言には、プロパティの値を取得する `Get` アクセサー、プロパティの値を格納する `Set` アクセサー、またはその両方を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="5529b-649">A property declaration may contain either a `Get` accessor, which retrieves the value of the property, a `Set` accessor, which stores the value of the property, or both.</span></span> <span data-ttu-id="5529b-650">プロパティはメソッドを暗黙的に宣言するので、メソッドと同じ修飾子を使用してプロパティを宣言できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-650">Because a property implicitly declares methods, a property may be declared with the same modifiers as a method.</span></span> <span data-ttu-id="5529b-651">プロパティがインターフェイスで定義されている場合、または `MustOverride` 修飾子を使用して定義されている場合は、プロパティ本体と `End` コンストラクトを省略する必要があります。それ以外の場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="5529b-651">If the property is defined in an interface or defined with the `MustOverride` modifier, the property body and the `End` construct must be omitted; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="5529b-652">インデックスパラメーターリストは、プロパティのシグネチャを作成するので、プロパティの型ではなく、インデックスパラメーターに対してプロパティがオーバーロードされる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-652">The index parameter list makes up the signature of the property, so properties may be overloaded on index parameters but not on the type of the property.</span></span> <span data-ttu-id="5529b-653">インデックスパラメーターリストは、通常のメソッドの場合と同じです。</span><span class="sxs-lookup"><span data-stu-id="5529b-653">The index parameter list is the same as for a regular method.</span></span> <span data-ttu-id="5529b-654">ただし、どのパラメーターも `ByRef` 修飾子を使用して変更することはできません。また、どちらのパラメーターも `Value` (`Set` アクセサーの暗黙的な値パラメーター用に予約されています) という名前にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-654">However, none of the parameters may be modified with the `ByRef` modifier and none of them may be named `Value` (which is reserved for the implicit value parameter in the `Set` accessor).</span></span>

<span data-ttu-id="5529b-655">プロパティは次のように宣言できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-655">A property may be declared as follows:</span></span>

* <span data-ttu-id="5529b-656">プロパティがプロパティ型修飾子を指定していない場合、プロパティは `Get` アクセサーと `Set` アクセサーの両方を持つ必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-656">If the property specifies no property type modifier, the property must have both a `Get` accessor and a `Set` accessor.</span></span> <span data-ttu-id="5529b-657">プロパティは、読み取り/書き込みプロパティと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="5529b-657">The property is said to be a read-write property.</span></span>

* <span data-ttu-id="5529b-658">プロパティが `ReadOnly` 修飾子を指定する場合、プロパティは `Get` アクセサーを持つ必要があり、`Set` アクセサーを持つことはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-658">If the property specifies the `ReadOnly` modifier, the property must have a `Get` accessor and may not have a `Set` accessor.</span></span> <span data-ttu-id="5529b-659">プロパティは、読み取り専用プロパティと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="5529b-659">The property is said to be read-only property.</span></span> <span data-ttu-id="5529b-660">読み取り専用プロパティが割り当てのターゲットである場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="5529b-660">It is a compile-time error for a read-only property to be the target of an assignment.</span></span>

* <span data-ttu-id="5529b-661">プロパティが `WriteOnly` 修飾子を指定する場合、プロパティは `Set` アクセサーを持つ必要があり、`Get` アクセサーを持つことはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-661">If the property specifies the `WriteOnly` modifier, the property must have a `Set` accessor and may not have a `Get` accessor.</span></span> <span data-ttu-id="5529b-662">プロパティは、書き込み専用プロパティと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="5529b-662">The property is said to be write-only property.</span></span> <span data-ttu-id="5529b-663">代入のターゲットまたはメソッドの引数として、式で書き込み専用のプロパティを参照する場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="5529b-663">It is a compile-time error to reference a write-only property in an expression except as the target of an assignment or as an argument to a method.</span></span>

<span data-ttu-id="5529b-664">プロパティの `Get` アクセサーと `Set` アクセサーは、個別のメンバーではなく、プロパティのアクセサーを個別に宣言することはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-664">The `Get` and `Set` accessors of a property are not distinct members, and it is not possible to declare the accessors of a property separately.</span></span> <span data-ttu-id="5529b-665">次の例では、1つの読み取り/書き込みプロパティを宣言しません。</span><span class="sxs-lookup"><span data-stu-id="5529b-665">The following example does not declare a single read-write property.</span></span> <span data-ttu-id="5529b-666">代わりに、同じ名前の2つのプロパティを宣言します。1つは読み取り専用で、もう1つは書き込み専用です。</span><span class="sxs-lookup"><span data-stu-id="5529b-666">Rather, it declares two properties with the same name, one read-only and one write-only:</span></span>

```vb
Class A
    Private nameValue As String

    ' Error, contains a duplicate member name.
    Public ReadOnly Property Name() As String 
        Get
            Return nameValue
        End Get
    End Property

    ' Error, contains a duplicate member name.
    Public WriteOnly Property Name() As String 
        Set (Value As String)
            nameValue = value
        End Set
    End Property
End Class
```

<span data-ttu-id="5529b-667">同じクラスで宣言された2つのメンバーは同じ名前を持つことができないため、この例ではコンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="5529b-667">Since two members declared in the same class cannot have the same name, the example causes a compile-time error.</span></span>

<span data-ttu-id="5529b-668">既定では、プロパティの `Get` と `Set` のアクセサーのアクセシビリティは、プロパティ自体のアクセシビリティと同じです。</span><span class="sxs-lookup"><span data-stu-id="5529b-668">By default, the accessibility of a property's `Get` and `Set` accessors is the same as the accessibility of the property itself.</span></span> <span data-ttu-id="5529b-669">ただし、`Get` アクセサーと `Set` アクセサーでは、プロパティとは別にアクセシビリティを指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="5529b-669">However, the `Get` and `Set` accessors can also specify accessibility separately from the property.</span></span> <span data-ttu-id="5529b-670">この場合、アクセサーのアクセシビリティは、プロパティのアクセシビリティよりも制限されている必要があり、1つのアクセサーだけがプロパティから異なるアクセシビリティレベルを持つことができます。</span><span class="sxs-lookup"><span data-stu-id="5529b-670">In that case, the accessibility of an accessor must be more restrictive than the accessibility of the property and only one accessor can have a different accessibility level from the property.</span></span> <span data-ttu-id="5529b-671">アクセスの種類は、次のように制限の厳しいものと見なされます。</span><span class="sxs-lookup"><span data-stu-id="5529b-671">Access types are considered more or less restrictive as follows:</span></span>

* <span data-ttu-id="5529b-672">`Private` は、`Public`、`Protected Friend`、`Protected`、または `Friend`よりも制限されています。</span><span class="sxs-lookup"><span data-stu-id="5529b-672">`Private` is more restrictive than `Public`, `Protected Friend`, `Protected`, or `Friend`.</span></span>

* <span data-ttu-id="5529b-673">`Friend` は `Protected Friend` または `Public`よりも制限されています。</span><span class="sxs-lookup"><span data-stu-id="5529b-673">`Friend` is more restrictive than `Protected Friend` or `Public`.</span></span>

* <span data-ttu-id="5529b-674">`Protected` は `Protected Friend` または `Public`よりも制限されています。</span><span class="sxs-lookup"><span data-stu-id="5529b-674">`Protected` is more restrictive than `Protected Friend` or `Public`.</span></span>

* <span data-ttu-id="5529b-675">`Protected Friend` は `Public`よりも制限が厳しいです。</span><span class="sxs-lookup"><span data-stu-id="5529b-675">`Protected Friend` is more restrictive than `Public`.</span></span>

<span data-ttu-id="5529b-676">プロパティのアクセサーのいずれかにアクセスできても、もう一方がアクセスできない場合、プロパティは読み取り専用または書き込み専用であるかのように扱われます。</span><span class="sxs-lookup"><span data-stu-id="5529b-676">When one of a property's accessors is accessible but the other one is not, the property is treated as if it was read-only or write-only.</span></span> <span data-ttu-id="5529b-677">例 :</span><span class="sxs-lookup"><span data-stu-id="5529b-677">For example:</span></span>

```vb
Class A
    Public Property P() As Integer
        Get
            ...
        End Get

        Private Set (Value As Integer)
            ...
        End Set
    End Property
End Class

Module Test
    Sub Main()
        Dim a As A = New A()

        ' Error: A.P is read-only in this context.
        a.P = 10
    End Sub
End Module
```

<span data-ttu-id="5529b-678">派生型がプロパティをシャドウする場合、派生プロパティは、読み取りと書き込みの両方について、シャドウされたプロパティを非表示にします。</span><span class="sxs-lookup"><span data-stu-id="5529b-678">When a derived type shadows a property, the derived property hides the shadowed property with respect to both reading and writing.</span></span> <span data-ttu-id="5529b-679">次の例では、`B` の `P` プロパティを使用して、読み取りと書き込みの両方について `A` の `P` プロパティを非表示にしています。</span><span class="sxs-lookup"><span data-stu-id="5529b-679">In the following example, the `P` property in `B` hides the `P` property in `A` with respect to both reading and writing:</span></span>

```vb
Class A
    Public WriteOnly Property P() As Integer
        Set (Value As Integer)
        End Set
    End Property
End Class

Class B
    Inherits A

    Public Shadows ReadOnly Property P() As Integer
       Get
       End Get
    End Property
End Class

Module Test
    Sub Main()
        Dim x As B = New B

        B.P = 10     ' Error, B.P is read-only.
    End Sub
End Module
```

<span data-ttu-id="5529b-680">戻り値の型またはパラメーターの型のアクセシビリティドメインは、またはプロパティ自体のアクセシビリティドメインのスーパーセットと同じである必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-680">The accessibility domain of the return type or parameter types must be the same as or a superset of the accessibility domain of the property itself.</span></span> <span data-ttu-id="5529b-681">プロパティは、1つの `Set` アクセサーと1つの `Get` アクセサーのみを持つことができます。</span><span class="sxs-lookup"><span data-stu-id="5529b-681">A property may only have one `Set` accessor and one `Get` accessor.</span></span>

<span data-ttu-id="5529b-682">宣言と呼び出しの構文の違いを除いて、`Overridable`、`NotOverridable`、`Overrides`、`MustOverride`、および `MustInherit` の各プロパティは、`Overridable`、`NotOverridable`、`Overrides`、`MustOverride`、`MustInherit` の各メソッドとまったく同様に動作します。</span><span class="sxs-lookup"><span data-stu-id="5529b-682">Except for differences in declaration and invocation syntax, `Overridable`, `NotOverridable`, `Overrides`, `MustOverride`, and `MustInherit` properties behave exactly like `Overridable`, `NotOverridable`, `Overrides`, `MustOverride`, and `MustInherit` methods.</span></span> <span data-ttu-id="5529b-683">プロパティがオーバーライドされた場合、オーバーライドするプロパティは同じ型 (読み取り/書き込み、読み取り専用、書き込み専用) である必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-683">When a property is overridden, the overriding property must be of the same type (read-write, read-only, write-only).</span></span> <span data-ttu-id="5529b-684">`Overridable` プロパティに `Private` アクセサーを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-684">An `Overridable` property cannot contain a `Private` accessor.</span></span>

<span data-ttu-id="5529b-685">次の例では、`X` は `Overridable` 読み取り専用プロパティ、`Y` は `Overridable` 読み取り/書き込みプロパティ、`Z` は `MustOverride` 読み取り/書き込みプロパティです。</span><span class="sxs-lookup"><span data-stu-id="5529b-685">In the following example `X` is an `Overridable` read-only property, `Y` is an `Overridable` read-write property, and `Z` is a `MustOverride` read-write property.</span></span>

```vb
MustInherit Class A
    Private _y As Integer

    Public Overridable ReadOnly Property X() As Integer
        Get
            Return 0
        End Get
    End Property

    Public Overridable Property Y() As Integer
        Get
            Return _y
         End Get
        Set (Value As Integer)
            _y = value
        End Set
    End Property

    Public MustOverride Property Z() As Integer
End Class
```

<span data-ttu-id="5529b-686">`Z` が `MustOverride`ので、含んでいるクラス `A` は `MustInherit`として宣言する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-686">Because `Z` is `MustOverride`, the containing class `A` must be declared `MustInherit`.</span></span>

<span data-ttu-id="5529b-687">これに対して、クラス `A` から派生するクラスを次に示します。</span><span class="sxs-lookup"><span data-stu-id="5529b-687">By contrast, a class that derives from class `A` is shown below:</span></span>

```vb
Class B
    Inherits A

    Private _z As Integer

    Public Overrides ReadOnly Property X() As Integer
        Get
            Return MyBase.X + 1
        End Get
    End Property

    Public Overrides Property Y() As Integer
        Get
            Return MyBase.Y
        End Get
        Set (Value As Integer)
            If value < 0 Then
                MyBase.Y = 0
            Else
                MyBase.Y = Value
            End If
        End Set
    End Property

    Public Overrides Property Z() As Integer
        Get
            Return _z
        End Get
        Set (Value As Integer)
            _z = Value
        End Set
    End Property
End Class
```

<span data-ttu-id="5529b-688">ここでは、`X`、`Y`、および `Z` プロパティの宣言によって、基本プロパティがオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="5529b-688">Here, the declarations of properties `X`,`Y`, and `Z` override the base properties.</span></span> <span data-ttu-id="5529b-689">各プロパティ宣言は、対応する継承されたプロパティのアクセシビリティ修飾子、型、および名前と完全に一致します。</span><span class="sxs-lookup"><span data-stu-id="5529b-689">Each property declaration exactly matches the accessibility modifiers, type, and name of the corresponding inherited property.</span></span> <span data-ttu-id="5529b-690">プロパティ `X` の `Get` アクセサーと、プロパティ `Y` の `Set` アクセサーは、`MyBase` キーワードを使用して、継承されたプロパティにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="5529b-690">The `Get` accessor of property `X` and the `Set` accessor of property `Y` use the `MyBase` keyword to access the inherited properties.</span></span> <span data-ttu-id="5529b-691">プロパティ `Z` の宣言は、`MustOverride` プロパティよりも優先されます。したがって、クラス `B`に未処理の `MustOverride` メンバーはなく、`B` は通常のクラスにすることができます。</span><span class="sxs-lookup"><span data-stu-id="5529b-691">The declaration of property `Z` overrides the `MustOverride` property -- thus, there are no outstanding `MustOverride` members in class `B`, and `B` is permitted to be a regular class.</span></span>

<span data-ttu-id="5529b-692">プロパティを使用して、リソースが最初に参照されるまで、リソースの初期化を遅延させることができます。</span><span class="sxs-lookup"><span data-stu-id="5529b-692">Properties can be used to delay initialization of a resource until the moment it is first referenced.</span></span> <span data-ttu-id="5529b-693">例 :</span><span class="sxs-lookup"><span data-stu-id="5529b-693">For example:</span></span>

```vb
Imports System.IO

Public Class ConsoleStreams
    Private Shared reader As TextReader
    Private Shared writer As TextWriter
    Private Shared errors As TextWriter

    Public Shared ReadOnly Property [In]() As TextReader
        Get
            If reader Is Nothing Then
                reader = Console.In
            End If
            Return reader
        End Get
    End Property

    Public Shared ReadOnly Property Out() As TextWriter
        Get
            If writer Is Nothing Then
                writer = Console.Out
            End If
            Return writer
        End Get
    End Property

    Public Shared ReadOnly Property [Error]() As TextWriter
        Get
            If errors Is Nothing Then
                errors = Console.Error
            End If
            Return errors
        End Get
    End Property
End Class
```

<span data-ttu-id="5529b-694">`ConsoleStreams` クラスには、標準入力、出力、およびエラーデバイスをそれぞれ表す、`In`、`Out`、および `Error`という3つのプロパティが含まれています。</span><span class="sxs-lookup"><span data-stu-id="5529b-694">The `ConsoleStreams` class contains three properties, `In`, `Out`, and `Error`, that represent the standard input, output, and error devices, respectively.</span></span> <span data-ttu-id="5529b-695">これらのメンバーをプロパティとして公開することによって、実際に使用されるまで、`ConsoleStreams` クラスは初期化を遅延させることができます。</span><span class="sxs-lookup"><span data-stu-id="5529b-695">By exposing these members as properties, the `ConsoleStreams` class can delay their initialization until they are actually used.</span></span> <span data-ttu-id="5529b-696">たとえば、最初に `Out` プロパティを参照したときに、`ConsoleStreams.Out.WriteLine("hello, world")`のように、出力デバイスの基になる `TextWriter` が初期化されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-696">For example, upon first referencing the `Out` property, as in `ConsoleStreams.Out.WriteLine("hello, world")`, the underlying `TextWriter` for the output device is initialized.</span></span> <span data-ttu-id="5529b-697">ただし、アプリケーションが `In` プロパティと `Error` プロパティを参照していない場合、それらのデバイスのオブジェクトは作成されません。</span><span class="sxs-lookup"><span data-stu-id="5529b-697">But if the application makes no reference to the `In` and `Error` properties, then no objects are created for those devices.</span></span>


### <a name="get-accessor-declarations"></a><span data-ttu-id="5529b-698">Get アクセサー宣言</span><span class="sxs-lookup"><span data-stu-id="5529b-698">Get Accessor Declarations</span></span>

<span data-ttu-id="5529b-699">`Get` アクセサー (getter) は、プロパティ `Get` 宣言を使用して宣言されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-699">A `Get` accessor (getter) is declared by using a property `Get` declaration.</span></span> <span data-ttu-id="5529b-700">プロパティ `Get` の宣言は、キーワード `Get` の後にステートメントブロックで構成されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-700">A property `Get` declaration consists of the keyword `Get` followed by a statement block.</span></span> <span data-ttu-id="5529b-701">`P`という名前のプロパティを指定した場合、`Get` のアクセサー宣言は、プロパティと同じ修飾子、型、およびパラメーターリストを持つ `get_P` 名前を持つメソッドを暗黙的に宣言します。</span><span class="sxs-lookup"><span data-stu-id="5529b-701">Given a property named `P`, a `Get` accessor declaration implicitly declares a method with the name `get_P` with the same modifiers, type, and parameter list as the property.</span></span> <span data-ttu-id="5529b-702">型にその名前の宣言が含まれている場合、コンパイル時エラーが発生しますが、暗黙的な宣言は名前のバインドのために無視されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-702">If the type contains a declaration with that name, a compile-time error results, but the implicit declaration is ignored for the purposes of name binding.</span></span>

<span data-ttu-id="5529b-703">特別なローカル変数は、プロパティと同じ名前の `Get` アクセサー本体の宣言領域で暗黙的に宣言され、プロパティの戻り値を表します。</span><span class="sxs-lookup"><span data-stu-id="5529b-703">A special local variable, which is implicitly declared in the `Get` accessor body's declaration space with the same name as the property, represents the return value of the property.</span></span> <span data-ttu-id="5529b-704">ローカル変数は、式で使用されるときに特別な名前解決セマンティクスを持ちます。</span><span class="sxs-lookup"><span data-stu-id="5529b-704">The local variable has special name resolution semantics when used in expressions.</span></span> <span data-ttu-id="5529b-705">呼び出し式など、メソッドグループとして分類された式を必要とするコンテキストでローカル変数が使用されている場合、その名前はローカル変数ではなく関数に解決されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-705">If the local variable is used in a context that expects an expression that is classified as a method group, such as an invocation expression, then the name resolves to the function rather than to the local variable.</span></span> <span data-ttu-id="5529b-706">例 :</span><span class="sxs-lookup"><span data-stu-id="5529b-706">For example:</span></span>

```vb
ReadOnly Property F(i As Integer) As Integer
    Get
        If i = 0 Then
            F = 1    ' Sets the return value.
        Else
            F = F(i - 1) ' Recursive call.
        End If
    End Get
End Property
```

<span data-ttu-id="5529b-707">かっこを使用すると、あいまいな状況が発生する可能性があります (`F` が1次元配列であるプロパティである `F(1)` など)。</span><span class="sxs-lookup"><span data-stu-id="5529b-707">The use of parentheses can cause ambiguous situations (such as `F(1)` where `F` is a property whose type is a one-dimensional array).</span></span> <span data-ttu-id="5529b-708">あいまいな状況では、名前がローカル変数ではなくプロパティに解決されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-708">In all ambiguous situations, the name resolves to the property rather than the local variable.</span></span> <span data-ttu-id="5529b-709">例 :</span><span class="sxs-lookup"><span data-stu-id="5529b-709">For example:</span></span>

```vb
ReadOnly Property F(i As Integer) As Integer()
    Get
        If i = 0 Then
            F = new Integer(2) { 1, 2, 3 }
        Else
            F = F(i - 1) ' Recursive call, not index.
        End If
    End Get
End Property
```

<span data-ttu-id="5529b-710">制御フローが `Get` アクセサー本体から出たときに、ローカル変数の値が呼び出し式に戻されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-710">When control flow leaves the `Get` accessor body, the value of the local variable is passed back to the invocation expression.</span></span> <span data-ttu-id="5529b-711">`Get` アクセサーを呼び出すことは、変数の値を読み取ることと概念的には同じであるため、次の例に示すように、`Get` アクセサーが観測可能な副作用を持つような不適切なプログラミングスタイルと見なされます。</span><span class="sxs-lookup"><span data-stu-id="5529b-711">Because invoking a `Get` accessor is conceptually equivalent to reading the value of a variable, it is considered bad programming style for `Get` accessors to have observable side effects, as illustrated in the following example:</span></span>

```vb
Class Counter
    Private Value As Integer

    Public ReadOnly Property NextValue() As Integer
        Get
            Value += 1
            Return Value
        End Get
    End Property
End Class
```

<span data-ttu-id="5529b-712">`NextValue` プロパティの値は、プロパティが以前にアクセスされた回数によって異なります。</span><span class="sxs-lookup"><span data-stu-id="5529b-712">The value of the `NextValue` property depends on the number of times the property has previously been accessed.</span></span> <span data-ttu-id="5529b-713">したがって、プロパティにアクセスすると、観測可能な副作用が生成されます。代わりに、プロパティをメソッドとして実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-713">Thus, accessing the property produces an observable side effect, and the property should instead be implemented as a method.</span></span>

<span data-ttu-id="5529b-714">`Get` アクセサーに対する "副作用なし" 規則は、`Get` アクセサーが常に変数に格納されている値を返すように記述されていることを意味するわけではありません。</span><span class="sxs-lookup"><span data-stu-id="5529b-714">The "no side effects" convention for `Get` accessors does not mean that `Get` accessors should always be written to simply return values stored in variables.</span></span> <span data-ttu-id="5529b-715">実際には、複数の変数にアクセスしたりメソッドを呼び出したりすることによって、多くの場合、`Get` アクセサーがプロパティの値を計算します。</span><span class="sxs-lookup"><span data-stu-id="5529b-715">Indeed, `Get` accessors often compute the value of a property by accessing multiple variables or invoking methods.</span></span> <span data-ttu-id="5529b-716">ただし、適切にデザインされた `Get` アクセサーは、オブジェクトの状態の監視可能な変更の原因となるアクションを実行しません。</span><span class="sxs-lookup"><span data-stu-id="5529b-716">However, a properly designed `Get` accessor performs no actions that cause observable changes in the state of the object.</span></span>

<span data-ttu-id="5529b-717">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="5529b-717">__Note.__</span></span> <span data-ttu-id="5529b-718">`Get` アクセサーには、サブルーチンが持つ行の配置に対して同じ制限があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-718">`Get` accessors have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="5529b-719">開始ステートメント、終了ステートメント、およびブロックはすべて論理行の先頭に記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-719">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>

```antlr
PropertyGetDeclaration
    : Attributes? AccessModifier? 'Get' LineTerminator
      Block?
      'End' 'Get' StatementTerminator
    ;
```

### <a name="set-accessor-declarations"></a><span data-ttu-id="5529b-720">アクセサー宣言の設定</span><span class="sxs-lookup"><span data-stu-id="5529b-720">Set Accessor Declarations</span></span>

<span data-ttu-id="5529b-721">`Set` accessor (セッター) は、プロパティセット宣言を使用して宣言されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-721">A `Set` accessor (setter) is declared by using a property set declaration.</span></span> <span data-ttu-id="5529b-722">プロパティセットの宣言は、キーワード `Set`、省略可能なパラメーターリスト、およびステートメントブロックで構成されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-722">A property set declaration consists of the keyword `Set`, an optional parameter list, and a statement block.</span></span> <span data-ttu-id="5529b-723">`P`という名前のプロパティを指定した場合、setter 宣言は、プロパティと同じ修飾子とパラメーターリストを持つ `set_P` 名前を持つメソッドを暗黙的に宣言します。</span><span class="sxs-lookup"><span data-stu-id="5529b-723">Given a property named `P`, a setter declaration implicitly declares a method with the name `set_P` with the same modifiers and parameter list as the property.</span></span> <span data-ttu-id="5529b-724">型にその名前の宣言が含まれている場合、コンパイル時エラーが発生しますが、暗黙的な宣言は名前のバインドのために無視されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-724">If the type contains a declaration with that name, a compile-time error results, but the implicit declaration is ignored for the purposes of name binding.</span></span>

<span data-ttu-id="5529b-725">パラメーターリストが指定されている場合は、1つのメンバーを持つ必要があります。また、そのメンバーには `ByVal`を除き、修飾子は使用できません。また、型はプロパティの型と同じである必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-725">If a parameter list is specified, it must have one member, that member must have no modifiers except `ByVal`, and its type must be the same as the type of the property.</span></span> <span data-ttu-id="5529b-726">パラメーターは、設定されるプロパティ値を表します。</span><span class="sxs-lookup"><span data-stu-id="5529b-726">The parameter represents the property value being set.</span></span> <span data-ttu-id="5529b-727">パラメーターを省略すると、`Value` という名前のパラメーターが暗黙的に宣言されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-727">If the parameter is omitted, a parameter named `Value` is implicitly declared.</span></span>

<span data-ttu-id="5529b-728">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="5529b-728">__Note.__</span></span> <span data-ttu-id="5529b-729">`Set` アクセサーには、サブルーチンが持つ行の配置に対して同じ制限があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-729">`Set` accessors have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="5529b-730">開始ステートメント、終了ステートメント、およびブロックはすべて論理行の先頭に記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-730">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>

```antlr
PropertySetDeclaration
    : Attributes? AccessModifier? 'Set'
      ( OpenParenthesis ParameterList? CloseParenthesis )? LineTerminator
      Block?
      'End' 'Set' StatementTerminator
    ;
```

### <a name="default-properties"></a><span data-ttu-id="5529b-731">既定のプロパティ</span><span class="sxs-lookup"><span data-stu-id="5529b-731">Default Properties</span></span>

<span data-ttu-id="5529b-732">修飾子 `Default` を指定するプロパティ。*既定のプロパティ*と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="5529b-732">A property that specifies the modifier `Default` is called a *default property*.</span></span> <span data-ttu-id="5529b-733">プロパティを許可する型には、インターフェイスを含む既定のプロパティが含まれる場合があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-733">Any type that allows properties may have a default property, including interfaces.</span></span> <span data-ttu-id="5529b-734">プロパティの名前を使用してインスタンスを修飾しなくても、既定のプロパティを参照できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-734">The default property may be referenced without having to qualify the instance with the name of the property.</span></span> <span data-ttu-id="5529b-735">したがって、クラスが指定されています。</span><span class="sxs-lookup"><span data-stu-id="5529b-735">Thus, given a class</span></span>

```vb
Class Test
    Public Default ReadOnly Property Item(i As Integer) As Integer
        Get
            Return i
        End Get
    End Property
End Class
```

<span data-ttu-id="5529b-736">コード</span><span class="sxs-lookup"><span data-stu-id="5529b-736">the code</span></span>

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test()
        Dim y As Integer

        y = x(10)
    End Sub
End Module
```

<span data-ttu-id="5529b-737">上記の式は、次の式と同じです。</span><span class="sxs-lookup"><span data-stu-id="5529b-737">is equivalent to</span></span>

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test()
        Dim y As Integer

        y = x.Item(10)
    End Sub
End Module
```

<span data-ttu-id="5529b-738">プロパティを `Default`として宣言すると、継承階層内のその名前でオーバーロードされたすべてのプロパティが `Default` 宣言されているかどうかにかかわらず、既定のプロパティになります。</span><span class="sxs-lookup"><span data-stu-id="5529b-738">Once a property is declared `Default`, all of the properties overloaded on that name in the inheritance hierarchy become the default property, whether they have been declared `Default` or not.</span></span> <span data-ttu-id="5529b-739">基底クラスが別の名前で既定のプロパティを宣言している場合に、派生クラスのプロパティ `Default` を宣言すると、`Shadows` や `Overrides`などの他の修飾子は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="5529b-739">Declaring a property `Default` in a derived class when the base class declared a default property by another name does not require any other modifiers such as `Shadows` or `Overrides`.</span></span> <span data-ttu-id="5529b-740">これは、既定のプロパティに id またはシグネチャがないため、シャドウまたはオーバーロードできないためです。</span><span class="sxs-lookup"><span data-stu-id="5529b-740">This is because the default property has no identity or signature and so cannot be shadowed or overloaded.</span></span> <span data-ttu-id="5529b-741">例 :</span><span class="sxs-lookup"><span data-stu-id="5529b-741">For example:</span></span>

```vb
Class Base
    Public ReadOnly Default Property Item(i As Integer) As Integer
        Get
            Console.WriteLine("Base = " & i)
        End Get
    End Property
End Class

Class Derived
    Inherits Base

    ' This hides Item, but does not change the default property.
    Public Shadows ReadOnly Property Item(i As Integer) As Integer
        Get
            Console.WriteLine("Derived = " & i)
        End Get
    End Property
End Class

Class MoreDerived
    Inherits Derived

    ' This declares a new default property, but not Item.
    ' This does not need to be declared Shadows
    Public ReadOnly Default Property Value(i As Integer) As Integer
        Get
            Console.WriteLine("MoreDerived = " & i)
        End Get
    End Property
End Class

Module Test
    Sub Main()
        Dim x As MoreDerived = New MoreDerived()
        Dim y As Integer
        Dim z As Derived = x

        y = x(10)        ' Calls MoreDerived.Value.
        y = x.Item(10)   ' Calls Derived.Item
        y = z(10)        ' Calls Base.Item
    End Sub
End Module
```

<span data-ttu-id="5529b-742">このプログラムでは、次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-742">This program will produce the output:</span></span>

```console
MoreDerived = 10
Derived = 10
Base = 10
```

<span data-ttu-id="5529b-743">型の中で宣言されたすべての既定のプロパティは、同じ名前である必要があります。わかりやすくするために、`Default` 修飾子を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-743">All default properties declared within a type must have the same name and, for clarity, must specify the `Default` modifier.</span></span> <span data-ttu-id="5529b-744">インデックスパラメーターが指定されていない既定のプロパティでは、含んでいるクラスのインスタンスが割り当てられるときにあいまいな状況が発生するため、既定のプロパティにはインデックスパラメーターが必要です。</span><span class="sxs-lookup"><span data-stu-id="5529b-744">Because a default property with no index parameters would cause an ambiguous situation when assigning instances of the containing class, default properties must have index parameters.</span></span> <span data-ttu-id="5529b-745">さらに、特定の名前でオーバーロードされた1つのプロパティに `Default` 修飾子が含まれる場合、その名前でオーバーロードされたすべてのプロパティでそれを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-745">Furthermore, if one property overloaded on a particular name includes the `Default` modifier, all properties overloaded on that name must specify it.</span></span> <span data-ttu-id="5529b-746">既定のプロパティを `Shared`にすることはできません。また、プロパティの少なくとも1つのアクセサーを `Private`することはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-746">Default properties may not be `Shared`, and at least one accessor of the property must not be `Private`.</span></span>

### <a name="automatically-implemented-properties"></a><span data-ttu-id="5529b-747">自動的に実装されたプロパティ</span><span class="sxs-lookup"><span data-stu-id="5529b-747">Automatically Implemented Properties</span></span>

<span data-ttu-id="5529b-748">プロパティが任意のアクセサーの宣言を省略した場合、プロパティがインターフェイスで宣言されていない場合、または `MustOverride`宣言されている場合を除き、プロパティの実装は自動的に指定されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-748">If a property omits declaration of any accessors, an implementation of the property will be automatically supplied unless the property is declared in an interface or is declared `MustOverride`.</span></span> <span data-ttu-id="5529b-749">引数を指定しない読み取り/書き込みプロパティのみが自動的に実装されます。それ以外の場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="5529b-749">Only read/write properties with no arguments can be automatically implemented; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="5529b-750">自動的に実装されたプロパティ `x`、もう1つのプロパティをオーバーライドする場合でも、プロパティと同じ型のプライベートローカル変数 `_x` が導入されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-750">An automatically implemented property `x`, even one overriding another property, introduces a private local variable `_x` with the same type as the property.</span></span> <span data-ttu-id="5529b-751">ローカル変数の名前と別の宣言との間に競合がある場合は、コンパイル時エラーが報告されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-751">If there is a collision between the local variable's name and another declaration, a compile-time error will be reported.</span></span> <span data-ttu-id="5529b-752">自動的に実装されるプロパティの `Get` アクセサーは、ローカルの値と、ローカルのの値を設定するプロパティの `Set` アクセサーを返します。</span><span class="sxs-lookup"><span data-stu-id="5529b-752">The automatically implemented property's `Get` accessor returns the value of the local and the property's `Set` accessor that sets the value of the local.</span></span> <span data-ttu-id="5529b-753">たとえば、次のように宣言します。</span><span class="sxs-lookup"><span data-stu-id="5529b-753">For example, the declaration:</span></span>

```vb
Public Property x() As Integer
```

<span data-ttu-id="5529b-754">は、次の場合とほぼ同じです。</span><span class="sxs-lookup"><span data-stu-id="5529b-754">is roughly equivalent to:</span></span>

```vb
Private _x As Integer
Public Property x() As Integer
    Get
        Return _x
    End Get
    Set (value As Integer)
        _x = value
    End Set
End Property
```

<span data-ttu-id="5529b-755">変数宣言と同様に、自動的に実装されるプロパティに初期化子を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="5529b-755">As with variable declarations, an automatically implemented property can include an initializer.</span></span> <span data-ttu-id="5529b-756">例 :</span><span class="sxs-lookup"><span data-stu-id="5529b-756">For example:</span></span>

```vb
Public Property x() As Integer = 10
Public Shared Property y() As New Customer() With { .Name = "Bob" }
```

<span data-ttu-id="5529b-757">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="5529b-757">__Note.__</span></span> <span data-ttu-id="5529b-758">自動的に実装されたプロパティが初期化されると、基になるフィールドではなく、プロパティによって初期化されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-758">When an automatically implemented property is initialized, it is initialized through the property, not the underlying field.</span></span> <span data-ttu-id="5529b-759">そのため、プロパティをオーバーライドすると、必要に応じて初期化を受け取ることができます。</span><span class="sxs-lookup"><span data-stu-id="5529b-759">This is so overriding properties can intercept the initialization if they need to.</span></span>

<span data-ttu-id="5529b-760">配列初期化子は、自動的に実装されるプロパティで使用できます。ただし、配列の範囲を明示的に指定する方法はありません。</span><span class="sxs-lookup"><span data-stu-id="5529b-760">Array initializers are allowed on automatically implemented properties, except that there is no way to specify the array bounds explicitly.</span></span>  <span data-ttu-id="5529b-761">例 :</span><span class="sxs-lookup"><span data-stu-id="5529b-761">For example:</span></span>

```vb
' Valid
Property x As Integer() = {1, 2, 3}
Property y As Integer(,) = {{1, 2, 3}, {12, 13, 14}, {11, 10, 9}}

' Invalid
Property x4(5) As Short
```

### <a name="iterator-properties"></a><span data-ttu-id="5529b-762">反復子のプロパティ</span><span class="sxs-lookup"><span data-stu-id="5529b-762">Iterator Properties</span></span>

<span data-ttu-id="5529b-763">*Iterator プロパティ*は、`Iterator` 修飾子を持つプロパティです。</span><span class="sxs-lookup"><span data-stu-id="5529b-763">An *iterator property* is a property with the `Iterator` modifier.</span></span> <span data-ttu-id="5529b-764">これは、反復子メソッド (セクション[反復子メソッド](statements.md#iterator-methods)) が使用されるのと同じ理由で使用されます。シーケンスを生成するための便利な方法として、`For Each` ステートメントで使用できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-764">It is used for the same reason an iterator method (Section [Iterator Methods](statements.md#iterator-methods)) is used -- as a convenient way to generate a sequence, one which can be consumed by the `For Each` statement.</span></span> <span data-ttu-id="5529b-765">Iterator プロパティの `Get` アクセサーは、iterator メソッドと同じように解釈されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-765">The `Get` accessor of an iterator property is interpreted in the same way as an iterator method.</span></span>

<span data-ttu-id="5529b-766">反復子プロパティは、明示的な `Get` アクセサーを持つ必要があり、その型は `IEnumerator`、`IEnumerable`、または一部の `IEnumerable(Of T)` に対して `IEnumerator(Of T)` または `T`である必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-766">An iterator property must have an explicit `Get` accessor, and its type must be `IEnumerator`, or `IEnumerable`, or `IEnumerator(Of T)` or `IEnumerable(Of T)` for some `T`.</span></span>

<span data-ttu-id="5529b-767">Iterator プロパティの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="5529b-767">Here is an example of an iterator property:</span></span>

```vb
Class Family
    Property Daughters As New List(Of String) From {"Beth", "Diane"}
    Property Sons As New List(Of String) From {"Abe", "Carl"}

    ReadOnly Iterator Property Children As IEnumerable(Of String)
        Get
            For Each name In Daughters : Yield name : Next
            For Each name In Sons : Yield name : Next
        End Get
    End Property
End Class

Module Module1
    Sub Main()
        Dim x As New Family
        For Each c In x.Children
            Console.WriteLine(c) ' prints Beth, Diane, Abe, Carl
        Next
    End Sub
End Module
```

## <a name="operators"></a><span data-ttu-id="5529b-768">演算子</span><span class="sxs-lookup"><span data-stu-id="5529b-768">Operators</span></span>

<span data-ttu-id="5529b-769">*演算子*は、含んでいるクラスの既存の Visual Basic 演算子の意味を定義するメソッドです。</span><span class="sxs-lookup"><span data-stu-id="5529b-769">*Operators* are methods that define the meaning of an existing Visual Basic operator for the containing class.</span></span> <span data-ttu-id="5529b-770">演算子が式のクラスに適用されると、演算子は、クラスで定義されている演算子メソッドの呼び出しにコンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="5529b-770">When the operator is applied to the class in an expression, the operator is compiled into a call to the operator method defined in the class.</span></span> <span data-ttu-id="5529b-771">クラスに対して演算子を定義することは、演算子の*オーバーロード*とも呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="5529b-771">Defining an operator for a class is also known as *overloading* the operator.</span></span>

```antlr
OperatorDeclaration
    : Attributes? OperatorModifier* 'Operator' OverloadableOperator
      OpenParenthesis ParameterList CloseParenthesis
      ( 'As' Attributes? TypeName )? LineTerminator
      Block?
      'End' 'Operator' StatementTerminator
    ;

OperatorModifier
    : 'Public' | 'Shared' | 'Overloads' | 'Shadows' | 'Widening' | 'Narrowing'
    ;

OverloadableOperator
    : '+' | '-' | '*' | '/' | '\\' | '&' | 'Like' | 'Mod' | 'And' | 'Or' | 'Xor'
    | '^' | '<' '<' | '>' '>' | '=' | '<' '>' | '>' | '<' | '>' '=' | '<' '='
    | 'Not' | 'IsTrue' | 'IsFalse' | 'CType'
    ;
```

<span data-ttu-id="5529b-772">既に存在する演算子をオーバーロードすることはできません。実際には、これは変換演算子に適用されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-772">It is not possible to overload an operator that already exists; in practice, this primarily applies to conversion operators.</span></span> <span data-ttu-id="5529b-773">たとえば、派生クラスから基底クラスへの変換をオーバーロードすることはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-773">For example, it is not possible to overload the conversion from a derived class to a base class:</span></span>

```vb
Class Base
End Class

Class Derived
    ' Cannot redefine conversion from Derived to Base,
    ' conversion will be ignored.
    Public Shared Widening Operator CType(s As Derived) As Base
        ...
    End Operator
End Class
```

<span data-ttu-id="5529b-774">演算子は、次の単語のような一般的な意味でオーバーロードすることもできます。</span><span class="sxs-lookup"><span data-stu-id="5529b-774">Operators can also be overloaded in the common sense of the word:</span></span>

```vb
Class Base
    Public Shared Widening Operator CType(b As Base) As Integer
        ...
    End Operator

    Public Shared Narrowing Operator CType(i As Integer) As Base
        ...
    End Operator
End Class
```

<span data-ttu-id="5529b-775">演算子宣言は、包含する型の宣言空間に明示的に名前を追加しません。ただし、これらのメソッドは、"op_" という文字で始まる対応するメソッドを暗黙的に宣言します。</span><span class="sxs-lookup"><span data-stu-id="5529b-775">Operator declarations do not explicitly add names to the containing type's declaration space; however they do implicitly declare a corresponding method starting with the characters "op_".</span></span> <span data-ttu-id="5529b-776">次のセクションでは、それぞれの演算子に対応するメソッド名を示します。</span><span class="sxs-lookup"><span data-stu-id="5529b-776">The following sections list the corresponding method names with each operator.</span></span>

<span data-ttu-id="5529b-777">定義可能な演算子には、単項演算子、二項演算子、および変換演算子の3つのクラスがあります。</span><span class="sxs-lookup"><span data-stu-id="5529b-777">There are three classes of operators that can be defined: unary operators, binary operators and conversion operators.</span></span> <span data-ttu-id="5529b-778">すべての演算子宣言は、特定の制限を共有します。</span><span class="sxs-lookup"><span data-stu-id="5529b-778">All operator declarations share certain restrictions:</span></span>

* <span data-ttu-id="5529b-779">演算子宣言は、常に `Public` および `Shared`である必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-779">Operator declarations must always be `Public` and `Shared`.</span></span> <span data-ttu-id="5529b-780">修飾子が想定されるコンテキストでは、`Public` 修飾子を省略できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-780">The `Public` modifier can be omitted in contexts where the modifier will be assumed.</span></span>

* <span data-ttu-id="5529b-781">演算子のパラメーターを `ByRef`、`Optional` または `ParamArray`として宣言することはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-781">The parameters of an operator cannot be declared `ByRef`, `Optional` or `ParamArray`.</span></span>

* <span data-ttu-id="5529b-782">少なくとも1つのオペランドまたは戻り値の型は、演算子を含む型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-782">The type of at least one of the operands or the return value must be the type that contains the operator.</span></span>

* <span data-ttu-id="5529b-783">演算子に対して定義された関数の戻り変数がありません。</span><span class="sxs-lookup"><span data-stu-id="5529b-783">There is no function return variable defined for operators.</span></span> <span data-ttu-id="5529b-784">したがって、演算子本体から値を返すには、`Return` ステートメントを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-784">Therefore, the `Return` statement must be used to return values from an operator body.</span></span>

<span data-ttu-id="5529b-785">これらの制限の唯一の例外は、null 許容値型に適用されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-785">The only exception to these restrictions applies to nullable value types.</span></span> <span data-ttu-id="5529b-786">Null 許容値型には実際の型定義がないため、値型では、型の null 許容バージョンに対してユーザー定義の演算子を宣言できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-786">Since nullable value types do not have an actual type definition, a value type can declare user-defined operators for the nullable version of the type.</span></span> <span data-ttu-id="5529b-787">型で特定のユーザー定義演算子を宣言できるかどうかを判断する場合、`?` の修飾子は、その宣言に含まれるすべての型から、有効性チェックの目的で最初に削除されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-787">When determining whether a type can declare a particular user-defined operator, the `?` modifiers are first dropped off of all of the types involved in the declaration for the purposes of validity checking.</span></span> <span data-ttu-id="5529b-788">この緩和は、`IsTrue` 演算子と `IsFalse` 演算子の戻り値の型には適用されません。`Boolean?`ではなく `Boolean`を返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-788">This relaxation does not apply to the return type of the `IsTrue` and `IsFalse` operators; they must still return `Boolean`, not `Boolean?`.</span></span>

<span data-ttu-id="5529b-789">演算子の優先順位と結合規則は、演算子宣言では変更できません。</span><span class="sxs-lookup"><span data-stu-id="5529b-789">The precedence and associativity of an operator cannot be modified by an operator declaration.</span></span>

<span data-ttu-id="5529b-790">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="5529b-790">__Note.__</span></span> <span data-ttu-id="5529b-791">演算子には、サブルーチンが持つ行の配置に対して同じ制限があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-791">Operators have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="5529b-792">開始ステートメント、終了ステートメント、およびブロックはすべて論理行の先頭に記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-792">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>


### <a name="unary-operators"></a><span data-ttu-id="5529b-793">単項演算子</span><span class="sxs-lookup"><span data-stu-id="5529b-793">Unary Operators</span></span>

<span data-ttu-id="5529b-794">次の単項演算子はオーバーロードできます。</span><span class="sxs-lookup"><span data-stu-id="5529b-794">The following unary operators can be overloaded:</span></span>

* <span data-ttu-id="5529b-795">単項プラス演算子 `+` (対応するメソッド: `op_UnaryPlus`)</span><span class="sxs-lookup"><span data-stu-id="5529b-795">The unary plus operator `+` (corresponding method: `op_UnaryPlus`)</span></span>

* <span data-ttu-id="5529b-796">単項マイナス演算子 `-` (対応するメソッド: `op_UnaryNegation`)</span><span class="sxs-lookup"><span data-stu-id="5529b-796">The unary minus operator `-` (corresponding method: `op_UnaryNegation`)</span></span>

* <span data-ttu-id="5529b-797">論理 `Not` 演算子 (対応するメソッド: `op_OnesComplement`)</span><span class="sxs-lookup"><span data-stu-id="5529b-797">The logical `Not` operator (corresponding method: `op_OnesComplement`)</span></span>

* <span data-ttu-id="5529b-798">`IsTrue` 演算子と `IsFalse` 演算子 (対応するメソッド: `op_True`、`op_False`)</span><span class="sxs-lookup"><span data-stu-id="5529b-798">The `IsTrue` and `IsFalse` operators (corresponding methods: `op_True`, `op_False`)</span></span>

<span data-ttu-id="5529b-799">オーバーロードされた単項演算子は、それを含む型の1つのパラメーターを受け取り、`Boolean`を返す必要がある `IsTrue` と `IsFalse`を除く、任意の型を返すことができます。</span><span class="sxs-lookup"><span data-stu-id="5529b-799">All overloaded unary operators must take a single parameter of the containing type and may return any type, except for `IsTrue` and `IsFalse`, which must return `Boolean`.</span></span> <span data-ttu-id="5529b-800">含んでいる型がジェネリック型の場合、型パラメーターは、それを含む型の型パラメーターと一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-800">If the containing type is a generic type, the type parameters must match the containing type's type parameters.</span></span> <span data-ttu-id="5529b-801">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="5529b-801">For example,</span></span>

```vb
Structure Complex
    ...

    Public Shared Operator +(v As Complex) As Complex
        Return v
    End Operator
End Structure
```

<span data-ttu-id="5529b-802">型が `IsTrue` または `IsFalse`のいずれかをオーバーロードする場合は、もう一方もオーバーロードする必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-802">If a type overloads one of `IsTrue` or `IsFalse`, then it must overload the other as well.</span></span> <span data-ttu-id="5529b-803">1つだけがオーバーロードされている場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="5529b-803">If only one is overloaded, a compile-time error results.</span></span>

<span data-ttu-id="5529b-804">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="5529b-804">__Note.__</span></span> <span data-ttu-id="5529b-805">`IsTrue` と `IsFalse` は予約語ではありません。</span><span class="sxs-lookup"><span data-stu-id="5529b-805">`IsTrue` and `IsFalse` are not reserved words.</span></span>

### <a name="binary-operators"></a><span data-ttu-id="5529b-806">二項演算子</span><span class="sxs-lookup"><span data-stu-id="5529b-806">Binary Operators</span></span>

<span data-ttu-id="5529b-807">次の二項演算子はオーバーロードできます。</span><span class="sxs-lookup"><span data-stu-id="5529b-807">The following binary operators can be overloaded:</span></span>

* <span data-ttu-id="5529b-808">加算 `+`、減算 `-`、乗算 `*`、除算 `/`、整数除算 `\`、剰余 `Mod` および指数演算子 (対応するメソッド: `^`、`op_Addition`、`op_Subtraction`、`op_Multiply`、`op_Division`、`op_IntegerDivision`、`op_Modulus`)`op_Exponent`</span><span class="sxs-lookup"><span data-stu-id="5529b-808">The addition `+`, subtraction `-`, multiplication `*`, division `/`, integral division `\`, modulo `Mod` and exponentiation `^` operators (corresponding method: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_IntegerDivision`, `op_Modulus`, `op_Exponent`)</span></span>

* <span data-ttu-id="5529b-809">関係演算子 `=`、`<>`、`<`、`>`、`<=`、`>=` (対応するメソッド: `op_Equality`、`op_Inequality`、`op_LessThan`、`op_GreaterThan`、`op_LessThanOrEqual`、`op_GreaterThanOrEqual`)。</span><span class="sxs-lookup"><span data-stu-id="5529b-809">The relational operators `=`, `<>`, `<`, `>`, `<=`, `>=` (corresponding methods: `op_Equality`, `op_Inequality`, `op_LessThan`, `op_GreaterThan`, `op_LessThanOrEqual`, `op_GreaterThanOrEqual`).</span></span> <span data-ttu-id="5529b-810">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="5529b-810">__Note.__</span></span> <span data-ttu-id="5529b-811">等値演算子はオーバーロードできますが、代入演算子 (代入ステートメントでのみ使用) をオーバーロードすることはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-811">While the equality operator can be overloaded, the assignment operator (used only in assignment statements) cannot be overloaded.</span></span>

* <span data-ttu-id="5529b-812">`Like` 演算子 (対応するメソッド: `op_Like`)</span><span class="sxs-lookup"><span data-stu-id="5529b-812">The `Like` operator (corresponding method: `op_Like`)</span></span>

* <span data-ttu-id="5529b-813">連結演算子 `&` (対応するメソッド: `op_Concatenate`)</span><span class="sxs-lookup"><span data-stu-id="5529b-813">The concatenation operator `&` (corresponding method: `op_Concatenate`)</span></span>

* <span data-ttu-id="5529b-814">論理 `And`、`Or`、および `Xor` 演算子 (対応するメソッド: `op_BitwiseAnd`、`op_BitwiseOr`、`op_ExclusiveOr`)</span><span class="sxs-lookup"><span data-stu-id="5529b-814">The logical `And`, `Or` and `Xor` operators (corresponding methods: `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`)</span></span>

* <span data-ttu-id="5529b-815">シフト演算子 `<<` および `>>` (対応するメソッド: `op_LeftShift`、`op_RightShift`)</span><span class="sxs-lookup"><span data-stu-id="5529b-815">The shift operators `<<` and `>>` (corresponding methods: `op_LeftShift`, `op_RightShift`)</span></span>

<span data-ttu-id="5529b-816">オーバーロードされた二項演算子はすべて、パラメーターの1つとして、含んでいる型を受け取る必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-816">All overloaded binary operators must take the containing type as one of the parameters.</span></span> <span data-ttu-id="5529b-817">含んでいる型がジェネリック型の場合、型パラメーターは、それを含む型の型パラメーターと一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-817">If the containing type is a generic type, the type parameters must match the containing type's type parameters.</span></span> <span data-ttu-id="5529b-818">シフト演算子は、最初のパラメーターが含まれている型であることを要求するように、この規則をさらに制限します。2番目のパラメーターは常に `Integer`型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-818">The shift operators further restrict this rule to require the first parameter to be of the containing type; the second parameter must always be of type `Integer`.</span></span>

<span data-ttu-id="5529b-819">次の二項演算子は、ペアで宣言する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-819">The following binary operators must be declared in pairs:</span></span>

* <span data-ttu-id="5529b-820">演算子 `=` and 演算子 `<>`</span><span class="sxs-lookup"><span data-stu-id="5529b-820">Operator `=` and operator `<>`</span></span>

* <span data-ttu-id="5529b-821">演算子 `>` and 演算子 `<`</span><span class="sxs-lookup"><span data-stu-id="5529b-821">Operator `>` and operator `<`</span></span>

* <span data-ttu-id="5529b-822">演算子 `>=` and 演算子 `<=`</span><span class="sxs-lookup"><span data-stu-id="5529b-822">Operator `>=` and operator `<=`</span></span>

<span data-ttu-id="5529b-823">ペアの1つが宣言されている場合、もう一方は、対応するパラメーターと戻り値の型で宣言されている必要があります。一致しない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="5529b-823">If one of the pair is declared, then the other must also be declared with matching parameter and return types, or a compile-time error will result.</span></span> <span data-ttu-id="5529b-824">(__注:__</span><span class="sxs-lookup"><span data-stu-id="5529b-824">(__Note.__</span></span> <span data-ttu-id="5529b-825">関係演算子のペア宣言を要求する目的は、オーバーロードされた演算子で少なくとも最小レベルの論理的な一貫性を確保することです。</span><span class="sxs-lookup"><span data-stu-id="5529b-825">The purpose of requiring paired declarations of relational operators is to try and ensure at least a minimum level of logical consistency in overloaded operators.)</span></span>

<span data-ttu-id="5529b-826">関係演算子とは異なり、除算演算子と整数除算演算子の両方をオーバーロードすることは、エラーではなく、避けることを強くお勧めします。</span><span class="sxs-lookup"><span data-stu-id="5529b-826">In contrast to the relational operators, overloading both the division and integral division operators is strongly discouraged, although not an error.</span></span> <span data-ttu-id="5529b-827">(__注:__</span><span class="sxs-lookup"><span data-stu-id="5529b-827">(__Note.__</span></span> <span data-ttu-id="5529b-828">一般に、2種類の除算は完全に区別されていなければなりません。除算をサポートする型は、整数 (この場合は `\`をサポートする必要があります)、またはそうではありません (この場合、`/`をサポートする必要があります)。</span><span class="sxs-lookup"><span data-stu-id="5529b-828">In general, the two types of division should be entirely distinct: a type that supports division is either integral (in which case it should support `\`) or not (in which case it should support `/`).</span></span> <span data-ttu-id="5529b-829">両方の演算子を定義するのにエラーが発生したと考えていましたが、これらの言語では、一般的には Visual Basic の2種類の除算を区別していないので、この方法を使用するのは安全だと思いますが、この方法は強くお勧めします)。</span><span class="sxs-lookup"><span data-stu-id="5529b-829">We considered making it an error to define both operators, but because their languages do not generally distinguish between two types of division the way Visual Basic does, we felt it was safest to allow the practice but strongly discourage it.)</span></span>

<span data-ttu-id="5529b-830">複合代入演算子を直接オーバーロードすることはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-830">Compound assignment operators cannot be overloaded directly.</span></span> <span data-ttu-id="5529b-831">代わりに、対応する二項演算子がオーバーロードされると、複合代入演算子はオーバーロードされた演算子を使用します。</span><span class="sxs-lookup"><span data-stu-id="5529b-831">Instead, when the corresponding binary operator is overloaded, the compound assignment operator will use the overloaded operator.</span></span> <span data-ttu-id="5529b-832">例 :</span><span class="sxs-lookup"><span data-stu-id="5529b-832">For example:</span></span>

```vb
Structure Complex
    ...

    Public Shared Operator +(x As Complex, y As Complex) _
        As Complex
        ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim c1, c2 As Complex
        ' Calls the overloaded + operator
        c1 += c2
    End Sub
End Module
```

### <a name="conversion-operators"></a><span data-ttu-id="5529b-833">変換演算子</span><span class="sxs-lookup"><span data-stu-id="5529b-833">Conversion Operators</span></span>

<span data-ttu-id="5529b-834">変換演算子は、型の間の新しい変換を定義します。</span><span class="sxs-lookup"><span data-stu-id="5529b-834">Conversion operators define new conversions between types.</span></span> <span data-ttu-id="5529b-835">これらの新しい変換は、*ユーザー定義の変換*と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="5529b-835">These new conversions are called *user-defined conversions*.</span></span> <span data-ttu-id="5529b-836">変換演算子は、変換演算子のパラメーター型で示される変換元の型を変換演算子の戻り値の型で指定されたターゲットの型に変換します。</span><span class="sxs-lookup"><span data-stu-id="5529b-836">A conversion operator converts from a source type, indicated by the parameter type of the conversion operator, to a target type, indicated by the return type of the conversion operator.</span></span> <span data-ttu-id="5529b-837">変換は、拡大または縮小として分類する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-837">Conversions must be classified as either widening or narrowing.</span></span> <span data-ttu-id="5529b-838">`Widening` キーワードを含む変換演算子の宣言では、ユーザー定義の拡大変換 (対応するメソッド: `op_Implicit`) が導入されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-838">A conversion operator declaration that includes the `Widening` keyword introduces a user-defined widening conversion (corresponding method: `op_Implicit`).</span></span> <span data-ttu-id="5529b-839">`Narrowing` キーワードを含む変換演算子の宣言では、ユーザー定義の縮小変換 (対応するメソッド: `op_Explicit`) が導入されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-839">A conversion operator declaration that includes the `Narrowing` keyword introduces a user-defined narrowing conversion (corresponding method: `op_Explicit`).</span></span>

<span data-ttu-id="5529b-840">一般に、ユーザー定義の拡大変換では、例外がスローされず、情報が失われることがないように設計する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-840">In general, user-defined widening conversions should be designed to never throw exceptions and never lose information.</span></span> <span data-ttu-id="5529b-841">ユーザー定義の変換によって例外が発生する可能性がある場合 (たとえば、ソース引数が範囲外の場合)、または情報が失われた場合 (上位ビットを破棄する場合など)、その変換は縮小変換として定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-841">If a user-defined conversion can cause exceptions (for example, because the source argument is out of range) or loss of information (such as discarding high-order bits), then that conversion should be defined as a narrowing conversion.</span></span> <span data-ttu-id="5529b-842">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="5529b-842">In the example:</span></span>

```vb
Structure Digit
    Dim value As Byte

    Public Sub New(value As Byte)
        if value < 0 OrElse value > 9 Then Throw New ArgumentException()
        Me.value = value
    End Sub

    Public Shared Widening Operator CType(d As Digit) As Byte
        Return d.value
    End Operator

    Public Shared Narrowing Operator CType(b As Byte) As Digit
        Return New Digit(b)
    End Operator
End Structure
```

<span data-ttu-id="5529b-843">`Digit` から `Byte` への変換は、例外がスローされたり情報が失われたりすることはないため、拡大変換ですが、`Digit` は `Byte`の有効な値のサブセットのみを表すことができるため、`Byte` から `Digit` への変換は縮小変換です。</span><span class="sxs-lookup"><span data-stu-id="5529b-843">the conversion from `Digit` to `Byte` is a widening conversion because it never throws exceptions or loses information, but the conversion from `Byte` to `Digit` is a narrowing conversion since `Digit` can only represent a subset of the possible values of a `Byte`.</span></span>

<span data-ttu-id="5529b-844">オーバーロードできる他のすべての型メンバーとは異なり、変換演算子のシグネチャには変換の対象の型が含まれます。</span><span class="sxs-lookup"><span data-stu-id="5529b-844">Unlike all other type members that can be overloaded, the signature of a conversion operator includes the target type of the conversion.</span></span> <span data-ttu-id="5529b-845">これは、戻り値の型がシグネチャに参加する唯一の型のメンバーです。</span><span class="sxs-lookup"><span data-stu-id="5529b-845">This is the only type member for which the return type participates in the signature.</span></span> <span data-ttu-id="5529b-846">ただし、変換演算子の拡大または縮小の分類は、演算子のシグネチャの一部ではありません。</span><span class="sxs-lookup"><span data-stu-id="5529b-846">The widening or narrowing classification of a conversion operator, however, is not part of the operator's signature.</span></span> <span data-ttu-id="5529b-847">したがって、クラスまたは構造体は、拡大変換演算子と、同じソースとターゲットの型を持つ縮小変換演算子の両方を宣言することはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-847">Thus, a class or structure cannot declare both a widening conversion operator and a narrowing conversion operator with the same source and target types.</span></span>

<span data-ttu-id="5529b-848">ユーザー定義の変換演算子は、含んでいる型との間で変換を行う必要があります。たとえば、クラス `C` では、`C` から `Integer` への変換と、`Integer` から `C`への変換を定義できますが、`Integer` から `Boolean`への変換は定義できません。</span><span class="sxs-lookup"><span data-stu-id="5529b-848">A user-defined conversion operator must convert either to or from the containing type -- for example, it is possible for a class `C` to define a conversion from `C` to `Integer` and from `Integer` to `C`, but not from `Integer` to `Boolean`.</span></span> <span data-ttu-id="5529b-849">含んでいる型がジェネリック型の場合、型パラメーターは、それを含む型の型パラメーターと一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5529b-849">If the containing type is a generic type, the type parameters must match the containing type's type parameters.</span></span> <span data-ttu-id="5529b-850">また、組み込みの (つまり、非ユーザー定義の) 変換を再定義することはできません。</span><span class="sxs-lookup"><span data-stu-id="5529b-850">Also, it is not possible to redefine an intrinsic (i.e. non-user-defined) conversion.</span></span> <span data-ttu-id="5529b-851">その結果、型は次のような変換を宣言できません。</span><span class="sxs-lookup"><span data-stu-id="5529b-851">As a result, a type cannot declare a conversion where:</span></span>

* <span data-ttu-id="5529b-852">変換元の型と変換先の型が同じです。</span><span class="sxs-lookup"><span data-stu-id="5529b-852">The source type and the destination type are the same.</span></span>

* <span data-ttu-id="5529b-853">変換元の型と変換先の型の両方が、変換演算子を定義する型ではありません。</span><span class="sxs-lookup"><span data-stu-id="5529b-853">Both the source type and the destination type are not the type that defines the conversion operator.</span></span>

* <span data-ttu-id="5529b-854">変換元の型または変換先の型がインターフェイス型です。</span><span class="sxs-lookup"><span data-stu-id="5529b-854">The source type or the destination type is an interface type.</span></span>

* <span data-ttu-id="5529b-855">変換元の型と変換先の型は、継承によって関連付けられます (`Object`を含む)。</span><span class="sxs-lookup"><span data-stu-id="5529b-855">The source type and destination types are related by inheritance (including `Object`).</span></span>

<span data-ttu-id="5529b-856">これらの規則の唯一の例外は、null 許容の値型に適用されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-856">The only exception to these rules applies to nullable value types.</span></span> <span data-ttu-id="5529b-857">Null 許容値型には実際の型定義がないため、値型では、型の null 許容バージョンのユーザー定義変換を宣言できます。</span><span class="sxs-lookup"><span data-stu-id="5529b-857">Since nullable value types do not have an actual type definition, a value type can declare user-defined conversions for the nullable version of the type.</span></span> <span data-ttu-id="5529b-858">型が特定のユーザー定義の変換を宣言できるかどうかを判断するときに、`?` の修飾子は、その宣言に含まれるすべての型から、有効性チェックの目的で最初に削除されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-858">When determining whether a type can declare a particular user-defined conversion, the `?` modifiers are first dropped off of all of the types involved in the declaration for the purposes of validity checking.</span></span> <span data-ttu-id="5529b-859">したがって、次の宣言は、`S` が `S` から `T`への変換を定義できるため、有効です。</span><span class="sxs-lookup"><span data-stu-id="5529b-859">Thus, the following declaration is valid because `S` can define a conversion from `S` to `T`:</span></span>

```vb
Structure T
    ...
End Structure

Structure S
    Public Shared Widening Operator CType(ByVal v As S?) As T
    ...
    End Operator
End Structure
```

<span data-ttu-id="5529b-860">ただし、構造体 `S` では `S` から `S`への変換を定義できないため、次の宣言は無効です。</span><span class="sxs-lookup"><span data-stu-id="5529b-860">The following declaration is not valid, however, because structure `S` cannot define a conversion from `S` to `S`:</span></span>

```vb
Structure S
    Public Shared Widening Operator CType(ByVal v As S) As S?
        ...
    End Operator
End Structure
```

### <a name="operator-mapping"></a><span data-ttu-id="5529b-861">演算子のマッピング</span><span class="sxs-lookup"><span data-stu-id="5529b-861">Operator Mapping</span></span>

<span data-ttu-id="5529b-862">でサポートされて Visual Basic いる一連の演算子は、.NET Framework にある他の言語の演算子のセットと完全に一致しない場合があるため、一部の演算子は定義または使用されているときに、他の演算子に特別にマップされます。</span><span class="sxs-lookup"><span data-stu-id="5529b-862">Because the set of operators that Visual Basic supports may not exactly match the set of operators that other languages on the .NET Framework, some operators are mapped specially onto other operators when being defined or used.</span></span> <span data-ttu-id="5529b-863">具体的な内容は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="5529b-863">Specifically:</span></span>

* <span data-ttu-id="5529b-864">整数除算演算子を定義すると、整数除算演算子を呼び出す通常の除算演算子 (他の言語からのみ使用可能) が自動的に定義されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-864">Defining an integral division operator will automatically define a regular division operator (usable only from other languages) that will call the integral division operator.</span></span>

* <span data-ttu-id="5529b-865">`Not`、`And`、および `Or` 演算子をオーバーロードすると、論理演算子とビット処理演算子を区別する他の言語の観点から、ビットごとの演算子のみがオーバーロードされます。</span><span class="sxs-lookup"><span data-stu-id="5529b-865">Overloading the `Not`, `And`, and `Or` operators will overload only the bitwise operator from the perspective of other languages that distinguish between logical and bitwise operators.</span></span>

* <span data-ttu-id="5529b-866">論理演算子とビット演算子 (つまり、`Not`、`And`、および `Or`に対して `op_LogicalNot`、`op_LogicalAnd`、および `op_LogicalOr` を使用する言語) を区別する言語の論理演算子だけをオーバーロードするクラスは、それぞれの論理演算子を Visual Basic 論理演算子にマップします。</span><span class="sxs-lookup"><span data-stu-id="5529b-866">A class that overloads only the logical operators in a language that distinguishes between logical and bitwise operators (i.e. a languages that uses `op_LogicalNot`, `op_LogicalAnd`, and `op_LogicalOr` for `Not`, `And`, and `Or`, respectively) will have their logical operators mapped onto the Visual Basic logical operators.</span></span> <span data-ttu-id="5529b-867">論理演算子とビット処理演算子の両方がオーバーロードされている場合は、ビットごとの演算子だけが使用されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-867">If both the logical and bitwise operators are overloaded, only the bitwise operators will be used.</span></span>

* <span data-ttu-id="5529b-868">`<<` 演算子と `>>` 演算子をオーバーロードすると、符号付きおよび符号なしのシフト演算子を区別する他の言語の観点から、符号付き演算子のみがオーバーロードされます。</span><span class="sxs-lookup"><span data-stu-id="5529b-868">Overloading the `<<` and `>>` operators will overload only the signed operators from the perspective of other languages that distinguish between signed and unsigned shift operators.</span></span>

* <span data-ttu-id="5529b-869">符号なしシフト演算子のみをオーバーロードするクラスでは、対応する Visual Basic シフト演算子にマップされた符号なしシフト演算子を持つことになります。</span><span class="sxs-lookup"><span data-stu-id="5529b-869">A class that overloads only an unsigned shift operator will have the unsigned shift operator mapped onto the corresponding Visual Basic shift operator.</span></span> <span data-ttu-id="5529b-870">符号なしおよび符号付きシフト演算子の両方がオーバーロードされている場合は、符号付きシフト演算子だけが使用されます。</span><span class="sxs-lookup"><span data-stu-id="5529b-870">If both an unsigned and signed shift operator is overloaded, only the signed shift operator will be used.</span></span>
