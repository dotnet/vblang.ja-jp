# <a name="type-members"></a><span data-ttu-id="b36aa-101">型のメンバー</span><span class="sxs-lookup"><span data-stu-id="b36aa-101">Type Members</span></span>

<span data-ttu-id="b36aa-102">型のメンバーは、記憶域の場所と実行可能コードを定義します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-102">Type members define storage locations and executable code.</span></span> <span data-ttu-id="b36aa-103">メソッド、コンス トラクター、イベント、定数、変数、およびプロパティがあることができます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-103">They can be methods, constructors, events, constants, variables, and properties.</span></span>

## <a name="interface-method-implementation"></a><span data-ttu-id="b36aa-104">インターフェイス メソッドの実装</span><span class="sxs-lookup"><span data-stu-id="b36aa-104">Interface Method Implementation</span></span>

<span data-ttu-id="b36aa-105">メソッド、イベント、およびプロパティは、インターフェイス メンバーを実装できます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-105">Methods, events, and properties can implement interface members.</span></span> <span data-ttu-id="b36aa-106">インターフェイス メンバーを実装するメンバーの宣言を指定します、`Implements`キーワードと 1 つまたは複数のインターフェイス メンバーを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-106">To implement an interface member, a member declaration specifies the `Implements` keyword and lists one or more interface members.</span></span>

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

<span data-ttu-id="b36aa-107">インターフェイス メンバーを実装するメソッドとプロパティは、暗黙的に`NotOverridable`として宣言されている場合を除き、 `MustOverride`、 `Overridable`、または別のメンバーをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="b36aa-107">Methods and properties that implement interface members are implicitly `NotOverridable` unless declared to be `MustOverride`, `Overridable`, or overriding another member.</span></span> <span data-ttu-id="b36aa-108">エラーにするインターフェイス メンバーを実装するメンバーには`Shared`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-108">It is an error for a member implementing an interface member to be `Shared`.</span></span> <span data-ttu-id="b36aa-109">メンバーのアクセシビリティはインターフェイス メンバーを実装する機能に影響を与えません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-109">A member's accessibility has no effect on its ability to implement interface members.</span></span>

<span data-ttu-id="b36aa-110">インターフェイスの実装を有効にするには、包含する型の実装のリストは互換性のあるメンバーを含むインターフェイスを名前する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-110">For an interface implementation to be valid, the implements list of the containing type must name an interface that contains a compatible member.</span></span> <span data-ttu-id="b36aa-111">互換性のあるメンバーは、署名には、実装するメンバーのシグネチャが一致する 1 つです。</span><span class="sxs-lookup"><span data-stu-id="b36aa-111">A compatible member is one whose signature matches the signature of the implementing member.</span></span> <span data-ttu-id="b36aa-112">ジェネリック インターフェイスが実装されている場合、Implements 句で指定された型引数に置き換えられます、シグネチャに互換性をチェックするときに。</span><span class="sxs-lookup"><span data-stu-id="b36aa-112">If a generic interface is being implemented, then the type argument supplied in the Implements clause is substituted into the signature when checking compatibility.</span></span> <span data-ttu-id="b36aa-113">例えば:</span><span class="sxs-lookup"><span data-stu-id="b36aa-113">For example:</span></span>

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

<span data-ttu-id="b36aa-114">イベント宣言を使用している場合は、デリゲート型がインターフェイスのイベントを実装する、互換性のあるイベントは、基になるデリゲート型が同じ型。</span><span class="sxs-lookup"><span data-stu-id="b36aa-114">If an event declared using a delegate type is implementing an interface event, then a compatible event is one whose underlying delegate type is the same type.</span></span> <span data-ttu-id="b36aa-115">それ以外の場合、イベントは、実装しているインターフェイスのイベントのデリゲート型を使用します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-115">Otherwise, the event uses the delegate type from the interface event it is implementing.</span></span> <span data-ttu-id="b36aa-116">このようなイベントは、複数のインターフェイス イベントを実装するインターフェイスのすべてのイベントが同じ基になるデリゲート型に必要です。</span><span class="sxs-lookup"><span data-stu-id="b36aa-116">If such an event implements multiple interface events, all the interface events must have the same underlying delegate type.</span></span> <span data-ttu-id="b36aa-117">例えば:</span><span class="sxs-lookup"><span data-stu-id="b36aa-117">For example:</span></span>

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

<span data-ttu-id="b36aa-118">インターフェイス メンバーの実装のリストを指定するには、型の名前、ピリオド、および識別子を使用します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-118">An interface member in the implements list is specified using a type name, a period, and an identifier.</span></span> <span data-ttu-id="b36aa-119">実装リスト内のインターフェイスまたは基本インターフェイスの実装の一覧で、インターフェイス型名である必要があります、識別子は、指定したインターフェイスのメンバーである必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-119">The type name must be an interface in the implements list or a base interface of an interface in the implements list, and the identifier must be a member of the specified interface.</span></span> <span data-ttu-id="b36aa-120">1 つのメンバーは、1 つ以上の一致するインターフェイス メンバーを実装できます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-120">A single member can implement more than one matching interface member.</span></span>

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

<span data-ttu-id="b36aa-121">実装されているインターフェイスのメンバーが使用できない場合すべて明示的に実装されたインターフェイスで複数のインターフェイスの継承により、実装しているメンバーは、メンバーは、使用する基本インターフェイスを明示的に参照する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-121">If the interface member being implemented is unavailable in all explicitly implemented interfaces because of multiple interface inheritance, the implementing member must explicitly reference a base interface on which the member is available.</span></span> <span data-ttu-id="b36aa-122">たとえば場合、`I1`と`I2`、メンバーを含んで`M`と`I3`から継承`I1`と`I2`、実装する型`I3`実装は`I1.M`と`I2.M`。</span><span class="sxs-lookup"><span data-stu-id="b36aa-122">For example, if `I1` and `I2` contain a member `M`, and `I3` inherits from `I1` and `I2`, a type implementing `I3` will implement `I1.M` and `I2.M`.</span></span> <span data-ttu-id="b36aa-123">場合は、インターフェイスの影は多重継承されたメンバーを実装する型が継承されたメンバーとそれをシャドウするメンバーを実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-123">If an interface shadows multiply inherited members, an implementing type will have to implement the inherited members and the member(s) shadowing them.</span></span>

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

<span data-ttu-id="b36aa-124">インターフェイス メンバーの親インターフェイスを実装する場合は、ジェネリック型引数を入力と同じように実装されるインターフェイスを提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-124">If the containing interface of the interface member be implemented is generic, the same type arguments as the interface being implements must be supplied.</span></span> <span data-ttu-id="b36aa-125">例えば:</span><span class="sxs-lookup"><span data-stu-id="b36aa-125">For example:</span></span>

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


## <a name="methods"></a><span data-ttu-id="b36aa-126">メソッド</span><span class="sxs-lookup"><span data-stu-id="b36aa-126">Methods</span></span>

<span data-ttu-id="b36aa-127">メソッドには、プログラムの実行可能なステートメントが含まれます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-127">Methods contain the executable statements of a program.</span></span>

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

<span data-ttu-id="b36aa-128">メソッドで、オプション パラメーターと省略可能な戻り値のリストがあるは、共有またはは共有されません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-128">Methods, which have an optional list of parameters and an optional return value, are either shared or not shared.</span></span> <span data-ttu-id="b36aa-129">共有メソッドは、クラスまたはクラスのインスタンスを介してアクセスします。</span><span class="sxs-lookup"><span data-stu-id="b36aa-129">Shared methods are accessed through the class or instances of the class.</span></span> <span data-ttu-id="b36aa-130">インスタンスのメソッドとも呼ばれます。 非共有メソッドは、クラスのインスタンスを通じてアクセスします。</span><span class="sxs-lookup"><span data-stu-id="b36aa-130">Non-shared methods, also called instance methods, are accessed through instances of the class.</span></span> <span data-ttu-id="b36aa-131">次の例では、クラス`Stack`いくつかの共有メソッドを持つ (`Clone`と`Flip`)、いくつかのインスタンス メソッドと (`Push`、 `Pop`、および`ToString`)。</span><span class="sxs-lookup"><span data-stu-id="b36aa-131">The following example shows a class `Stack` that has several shared methods (`Clone` and `Flip`), and several instance methods (`Push`, `Pop`, and `ToString`):</span></span>

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

<span data-ttu-id="b36aa-132">メソッド オーバー ロードできます、つまり、一意の署名がある限り、複数のメソッドに同じ名前を持つことがあります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-132">Methods can be overloaded, which means that multiple methods may have the same name so long as they have unique signatures.</span></span> <span data-ttu-id="b36aa-133">メソッドのシグネチャは、そのパラメーターの型と数で構成されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-133">The signature of a method consists of the number and types of its parameters.</span></span> <span data-ttu-id="b36aa-134">メソッドのシグネチャ具体的には含まれません、戻り値の型またはパラメーター修飾子 (省略可能)、ByRef ParamArray など。</span><span class="sxs-lookup"><span data-stu-id="b36aa-134">The signature of a method specifically does not include the return type or parameter modifiers such as Optional, ByRef or ParamArray.</span></span> <span data-ttu-id="b36aa-135">次の例では、さまざまなオーバー ロード クラスを示します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-135">The following example shows a class with a number of overloads:</span></span>

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

<span data-ttu-id="b36aa-136">プログラムの出力は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="b36aa-136">The output of the program is:</span></span>

```vb
F()
F(Integer)
F(Object)
F(Integer, Integer)
F(Integer())
G(String)
G(String, Optional String)
```

<span data-ttu-id="b36aa-137">省略可能なパラメーターでのみ異なるオーバー ロードは、ライブラリの「バージョン管理」で使用できます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-137">Overloads that differ only in optional parameters can be used for "versioning" of libraries.</span></span> <span data-ttu-id="b36aa-138">たとえば、ライブラリの v1 には、省略可能なパラメーターを持つ関数が含まれます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-138">For instance, v1 of a library might include a function with optional parameters:</span></span>

```vb
Sub fopen(fileName As String, Optional accessMode as Integer = 0)
```

<span data-ttu-id="b36aa-139">もう 1 つの省略可能なパラメーター"password"を追加する場合、ライブラリの v2 と (つまり、v1 を対象とするために使用するアプリケーションを再コンパイルすることができます) のソース互換性を損なうことがなく、(そのために使用するアプリケーションのバイナリの互換性を損なうことがなく、これを実行したいです。リファレンス v1 と参照できます再コンパイルせず v2)。</span><span class="sxs-lookup"><span data-stu-id="b36aa-139">Then v2 of the library wants to add another optional parameter "password", and it wants to do so without breaking source compatibility (so applications that used to target v1 can be recompiled), and without breaking binary compatibility (so applications that used to reference v1 can now reference v2 without recompilation).</span></span> <span data-ttu-id="b36aa-140">これは、v2 の見た目は。</span><span class="sxs-lookup"><span data-stu-id="b36aa-140">This is how v2 will look:</span></span>

```vb
Sub fopen(file As String, mode as Integer)
Sub fopen(file As String, Optional mode as Integer = 0, Optional pword As String = "")
```

<span data-ttu-id="b36aa-141">パブリック API では省略可能なパラメーターが CLS に準拠していないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="b36aa-141">Note that optional parameters in a public API are not CLS-compliant.</span></span> <span data-ttu-id="b36aa-142">ただし、それらで使用できる、少なくとも Visual Basic および C# 4 とF#します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-142">However, they can be consumed at least by Visual Basic and C#4 and F#.</span></span>



### <a name="regular-async-and-iterator-method-declarations"></a><span data-ttu-id="b36aa-143">通常、非同期および反復子メソッドの宣言</span><span class="sxs-lookup"><span data-stu-id="b36aa-143">Regular, Async and Iterator Method Declarations</span></span>

<span data-ttu-id="b36aa-144">メソッドの 2 種類があります:*サブルーチン*、値を返さないと*関数*を実行します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-144">There are two types of methods: *subroutines*, which do not return values, and *functions*, which do.</span></span> <span data-ttu-id="b36aa-145">本文と`End`メソッドがインターフェイスで定義されている、または場合、メソッドの構文を省略することがありますのみ、`MustOverride`修飾子。</span><span class="sxs-lookup"><span data-stu-id="b36aa-145">The body and `End` construct of a method may only be omitted if the method is defined in an interface or has the `MustOverride` modifier.</span></span> <span data-ttu-id="b36aa-146">関数の戻り値の型が指定されていないと、厳密な型が使用されている、コンパイル時エラーが発生します。それ以外の場合、型は暗黙的に`Object`またはメソッドの型の文字の種類。</span><span class="sxs-lookup"><span data-stu-id="b36aa-146">If no return type is specified on a function and strict semantics are being used, a compile-time error occurs; otherwise the type is implicitly `Object` or the type of the method's type character.</span></span> <span data-ttu-id="b36aa-147">戻り値の型とメソッドのパラメーターの型のアクセシビリティ ドメインは、メソッド自体のアクセシビリティ ドメインのスーパー セットであるかと同じである必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-147">The accessibility domain of the return type and parameter types of a method must be the same as or a superset of the accessibility domain of the method itself.</span></span>

<span data-ttu-id="b36aa-148">A__正規メソッド__で、どちらも`Async`も`Iterator`修飾子。</span><span class="sxs-lookup"><span data-stu-id="b36aa-148">A __regular method__ is one with neither `Async` nor `Iterator` modifiers.</span></span> <span data-ttu-id="b36aa-149">サブルーチンまたは関数があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-149">It may be a subroutine or a function.</span></span> <span data-ttu-id="b36aa-150">セクション[通常のメソッド](statements.md#regular-methods)正規表現メソッドが呼び出されたときの動作について詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-150">Section [Regular Methods](statements.md#regular-methods) details what happens when a regular method is invoked.</span></span>

<span data-ttu-id="b36aa-151">__反復子メソッド__で 1 つは、`Iterator`修飾子と no`Async`修飾子。</span><span class="sxs-lookup"><span data-stu-id="b36aa-151">An __iterator method__ is one with the `Iterator` modifier and no `Async` modifier.</span></span> <span data-ttu-id="b36aa-152">関数の場合、する必要があり、その戻り値の型である必要があります`IEnumerator`、 `IEnumerable`、または`IEnumerator(Of T)`または`IEnumerable(Of T)`一部`T`、されが必要ない`ByRef`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="b36aa-152">It must be a function, and its return type must be `IEnumerator`, `IEnumerable`, or `IEnumerator(Of T)` or `IEnumerable(Of T)` for some `T`, and it must have no `ByRef` parameters.</span></span> <span data-ttu-id="b36aa-153">セクション[反復子メソッド](statements.md#iterator-methods)反復子メソッドが呼び出されたときの動作について詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-153">Section [Iterator Methods](statements.md#iterator-methods) details what happens when an iterator method is invoked.</span></span>

<span data-ttu-id="b36aa-154">__Async メソッド__で 1 つ、`Async`修飾子と no`Iterator`修飾子。</span><span class="sxs-lookup"><span data-stu-id="b36aa-154">An __async method__ is one with the `Async` modifier and no `Iterator` modifier.</span></span> <span data-ttu-id="b36aa-155">サブルーチン、または戻り値の型を持つ関数のいずれかでなければなりません`Task`または`Task(Of T)`一部`T`、no が必要と`ByRef`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="b36aa-155">It must be either a subroutine, or a function with return type `Task` or `Task(Of T)` for some `T`, and must have no `ByRef` parameters.</span></span> <span data-ttu-id="b36aa-156">セクション[非同期メソッド](statements.md#async-methods)非同期メソッドが呼び出されたときの動作について詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-156">Section [Async Methods](statements.md#async-methods) details what happens when an async method is invoked.</span></span>

<span data-ttu-id="b36aa-157">メソッドでないメソッドのこれらの 3 種類のいずれかの場合は、コンパイル時エラーを勧めします。</span><span class="sxs-lookup"><span data-stu-id="b36aa-157">It is a compile-time error if a method is not one of these three kinds of method.</span></span>

<span data-ttu-id="b36aa-158">サブルーチンと関数の宣言では論理行の先頭には、先頭と end ステートメントの各開始する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-158">Subroutine and function declarations are special in that their beginning and end statements must each start at the beginning of a logical line.</span></span> <span data-ttu-id="b36aa-159">以外の本文ではさらに、`MustOverride`サブルーチンまたは関数の宣言が論理行の先頭から開始する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-159">Additionally, the body of a non-`MustOverride` subroutine or function declaration must start at the beginning of a logical line.</span></span> <span data-ttu-id="b36aa-160">例えば:</span><span class="sxs-lookup"><span data-stu-id="b36aa-160">For example:</span></span>

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


### <a name="external-method-declarations"></a><span data-ttu-id="b36aa-161">外部メソッドの宣言</span><span class="sxs-lookup"><span data-stu-id="b36aa-161">External Method Declarations</span></span>

<span data-ttu-id="b36aa-162">外部メソッドの宣言には、実装がプログラムの外部に提供される新しいメソッドが導入されています。</span><span class="sxs-lookup"><span data-stu-id="b36aa-162">An external method declaration introduces a new method whose implementation is provided external to the program.</span></span>

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

<span data-ttu-id="b36aa-163">外部メソッドの宣言は実際の実装を提供しないため、メソッド本体を持たないまたは`End`を構築します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-163">Because an external method declaration provides no actual implementation, it has no method body or `End` construct.</span></span> <span data-ttu-id="b36aa-164">外部メソッドが暗黙的に共有、可能性がありますいない型のパラメーターを持つと可能性がありますいないイベントを処理またはインターフェイス メンバーを実装します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-164">External methods are implicitly shared, may not have type parameters, and may not handle events or implement interface members.</span></span> <span data-ttu-id="b36aa-165">関数の戻り値の型が指定されていないと、厳密な型が使用されている、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-165">If no return type is specified on a function and strict semantics are being used, a compile-time error occurs.</span></span> <span data-ttu-id="b36aa-166">それ以外の場合、型は暗黙的に`Object`またはメソッドの型の文字の種類。</span><span class="sxs-lookup"><span data-stu-id="b36aa-166">Otherwise the type is implicitly `Object` or the type of the method's type character.</span></span> <span data-ttu-id="b36aa-167">外部メソッドのパラメーターの型、戻り値の型のアクセシビリティ ドメインと同じか、外部メソッド自体のアクセシビリティ ドメインのスーパー セットがあります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-167">The accessibility domain of the return type and parameter types of an external method must be the same as or a superset of the accessibility domain of the external method itself.</span></span>

<span data-ttu-id="b36aa-168">外部メソッドの宣言のライブラリの句では、メソッドを実装する外部のファイルの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-168">The library clause of an external method declaration specifies the name of the external file that implements the method.</span></span> <span data-ttu-id="b36aa-169">オプションのエイリアス句は、数値の序数を指定する文字列 (プレフィックス、`#`文字) または外部ファイル内のメソッドの名前。</span><span class="sxs-lookup"><span data-stu-id="b36aa-169">The optional alias clause is a string that specifies the numeric ordinal (prefixed by a `#` character) or name of the method in the external file.</span></span> <span data-ttu-id="b36aa-170">単一の文字セットの修飾子を指定すると、外部メソッドの呼び出し中に文字列をマーシャ リングするために使用する文字セットが決まります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-170">A single-character set modifier may also be specified, which governs the character set used to marshal strings during a call to the external method.</span></span> <span data-ttu-id="b36aa-171">`Unicode`修飾子を Unicode 値では、すべての文字列をマーシャ リング、`Ansi`修飾子を ANSI 値のすべての文字列をマーシャ リングと`Auto`修飾子、メソッドの名前に基づく .NET Framework の規則に従って文字列をマーシャ リングまたは指定した場合の別名です。</span><span class="sxs-lookup"><span data-stu-id="b36aa-171">The `Unicode` modifier marshals all strings to Unicode values, the `Ansi` modifier marshals all strings to ANSI values, and the `Auto` modifier marshals the strings according to .NET Framework rules based on the name of the method, or the alias name if specified.</span></span> <span data-ttu-id="b36aa-172">修飾子が指定されていない場合、既定値は`Ansi`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-172">If no modifier is specified, the default is `Ansi`.</span></span>

<span data-ttu-id="b36aa-173">場合`Ansi`または`Unicode`が指定されている場合、外部ファイルを変更していない場合、メソッド名が検索されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-173">If `Ansi` or `Unicode` is specified, then the method name is looked up in the external file with no modification.</span></span> <span data-ttu-id="b36aa-174">場合`Auto`が指定されたメソッド名の検索は、プラットフォームによって異なります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-174">If `Auto` is specified, then method name lookup depends on the platform.</span></span> <span data-ttu-id="b36aa-175">プラットフォームは、ANSI (たとえば、Windows 95、Windows 98、Windows ME) と見なされます場合、メソッド名が検索されます変更していない場合。</span><span class="sxs-lookup"><span data-stu-id="b36aa-175">If the platform is considered to be ANSI (for example, Windows 95, Windows 98, Windows ME), then the method name is looked up with no modification.</span></span> <span data-ttu-id="b36aa-176">ルックアップに失敗した場合、`A`が追加されます、参照を再実行します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-176">If the lookup fails, an `A` is appended and the lookup tried again.</span></span> <span data-ttu-id="b36aa-177">プラットフォームは Unicode (たとえば、Windows NT、Windows 2000、Windows XP の場合) と見なされる場合、`W`が追加され、名前が検索されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-177">If the platform is considered to be Unicode (for example, Windows NT, Windows 2000, Windows XP), then a `W` is appended and the name is looked up.</span></span> <span data-ttu-id="b36aa-178">検索がなしでもう一度しようとした場合は、参照に失敗、`W`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-178">If the lookup fails, the lookup is tried again without the `W`.</span></span> <span data-ttu-id="b36aa-179">例えば:</span><span class="sxs-lookup"><span data-stu-id="b36aa-179">For example:</span></span>

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

<span data-ttu-id="b36aa-180">外部メソッドに渡されるデータ型は、1 つの例外には、.NET Framework データ マーシャ リング規則に従ってマーシャ リングされます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-180">Data types being passed to external methods are marshaled according to the .NET Framework data marshalling conventions with one exception.</span></span> <span data-ttu-id="b36aa-181">値によって渡される変数の文字列 (つまり、 `ByVal x As String`) OLE オートメーション BSTR 型には、マーシャ リングと文字列引数には、外部メソッドに BSTR に加えられた変更が反映します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-181">String variables that are passed by value (that is, `ByVal x As String`) are marshaled to the OLE Automation BSTR type, and changes made to the BSTR in the external method are reflected back in the string argument.</span></span> <span data-ttu-id="b36aa-182">ため、これは、型`String`外部メソッドが、変更可能であり、この特殊なマーシャ リング動作を模倣します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-182">This is because the type `String` in external methods is mutable, and this special marshalling mimics that behavior.</span></span> <span data-ttu-id="b36aa-183">参照によって渡されるパラメーターの文字列 (つまり`ByRef x As String`) は、OLE オートメーション BSTR 型へのポインターとしてマーシャ リングします。</span><span class="sxs-lookup"><span data-stu-id="b36aa-183">String parameters that are passed by reference (i.e. `ByRef x As String`) are marshaled as a pointer to the OLE Automation BSTR type.</span></span> <span data-ttu-id="b36aa-184">指定することによってこれらの特殊な動作をオーバーライドすることができます、`System.Runtime.InteropServices.MarshalAsAttribute`パラメーターの属性。</span><span class="sxs-lookup"><span data-stu-id="b36aa-184">It is possible to override these special behaviors by specifying the `System.Runtime.InteropServices.MarshalAsAttribute` attribute on the parameter.</span></span>

<span data-ttu-id="b36aa-185">この例では、外部メソッドの使用を示しています。</span><span class="sxs-lookup"><span data-stu-id="b36aa-185">The example demonstrates use of external methods:</span></span>

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


### <a name="overridable-methods"></a><span data-ttu-id="b36aa-186">オーバーライド可能なメソッド</span><span class="sxs-lookup"><span data-stu-id="b36aa-186">Overridable Methods</span></span>

<span data-ttu-id="b36aa-187">`Overridable`修飾子は、メソッドがオーバーライドできることを示します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-187">The `Overridable` modifier indicates that a method is overridable.</span></span> <span data-ttu-id="b36aa-188">`Overrides`修飾子は、メソッドが、同じシグネチャを持つ基本型のオーバーライド可能なメソッドをオーバーライドすることを示します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-188">The `Overrides` modifier indicates that a method overrides a base-type overridable method that has the same signature.</span></span> <span data-ttu-id="b36aa-189">`NotOverridable`修飾子は、オーバーライド可能なメソッドをさらにオーバーライドできないことを示します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-189">The `NotOverridable` modifier indicates that an overridable method cannot be further overridden.</span></span> <span data-ttu-id="b36aa-190">`MustOverride`修飾子は、派生クラスでメソッドをオーバーライドする必要があることを示します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-190">The `MustOverride` modifier indicates that a method must be overridden in derived classes.</span></span>

<span data-ttu-id="b36aa-191">これらの修飾子の特定の組み合わせが無効です。</span><span class="sxs-lookup"><span data-stu-id="b36aa-191">Certain combinations of these modifiers are not valid:</span></span>

* <span data-ttu-id="b36aa-192">`Overridable` `NotOverridable`は相互に排他的と組み合わせることはできません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-192">`Overridable` and `NotOverridable` are mutually exclusive and cannot be combined.</span></span>

* <span data-ttu-id="b36aa-193">`MustOverride` 意味`Overridable`(および指定できないように) と併用することはできません`NotOverridable`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-193">`MustOverride` implies `Overridable` (and so cannot specify it) and cannot be combined with `NotOverridable`.</span></span>

* <span data-ttu-id="b36aa-194">`NotOverridable` 組み合わせて使用できない`Overridable`または`MustOverride`と併用する必要があります`Overrides`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-194">`NotOverridable` cannot be combined with `Overridable` or `MustOverride` and must be combined with `Overrides`.</span></span>

* <span data-ttu-id="b36aa-195">`Overrides` 意味`Overridable`(および指定できないように) と併用することはできません`MustOverride`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-195">`Overrides` implies `Overridable` (and so cannot specify it) and cannot be combined with `MustOverride`.</span></span>

<span data-ttu-id="b36aa-196">オーバーライド可能なメソッドで追加の制限もあります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-196">There are also additional restrictions on overridable methods:</span></span>

* <span data-ttu-id="b36aa-197">A`MustOverride`メソッドは、メソッド本体を含まない場合がありますまたは`End`構築、もう 1 つのメソッドをオーバーライドしませんおよびにのみ出現`MustInherit`クラス。</span><span class="sxs-lookup"><span data-stu-id="b36aa-197">A `MustOverride` method may not include a method body or an `End` construct, may not override another method, and may only appear in `MustInherit` classes.</span></span>

* <span data-ttu-id="b36aa-198">メソッドが指定されている場合`Overrides`を上書きするコンパイル時エラーが発生したに一致する基本メソッドはありません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-198">If a method specifies `Overrides` and there is no matching base method to override, a compile-time error occurs.</span></span> <span data-ttu-id="b36aa-199">オーバーライドするメソッドを指定しない場合があります`Shadows`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-199">An overriding method may not specify `Shadows`.</span></span>

* <span data-ttu-id="b36aa-200">メソッドは、オーバーライドするメソッドのアクセシビリティ ドメインでオーバーライドされるメソッドのアクセシビリティ ドメインと等しくない場合、別のメソッドをオーバーライドしない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-200">A method may not override another method if the overriding method's accessibility domain is not equal to the accessibility domain of the method being overridden.</span></span> <span data-ttu-id="b36aa-201">1 つの例外、メソッドをオーバーライドすることです、`Protected Friend`メソッドがない別のアセンブリで`Friend`へのアクセスを指定する必要があります`Protected`(いない`Protected Friend`)。</span><span class="sxs-lookup"><span data-stu-id="b36aa-201">The one exception is that a method overriding a `Protected Friend` method in another assembly that does not have `Friend` access must specify `Protected` (not `Protected Friend`).</span></span>

* <span data-ttu-id="b36aa-202">`Private` メソッドができない可能性があります`Overridable`、 `NotOverridable`、または`MustOverride`、やその他のメソッドをオーバーライドする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-202">`Private` methods may not be `Overridable`, `NotOverridable`, or `MustOverride`, nor may they override other methods.</span></span>

* <span data-ttu-id="b36aa-203">メソッドで`NotInheritable`クラスが宣言されていない`Overridable`または`MustOverride`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-203">Methods in `NotInheritable` classes may not be declared `Overridable` or `MustOverride`.</span></span>

<span data-ttu-id="b36aa-204">次の例は、オーバーライド可能なとできる方法の相違点を示しています。</span><span class="sxs-lookup"><span data-stu-id="b36aa-204">The following example illustrates the differences between overridable and nonoverridable methods:</span></span>

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

<span data-ttu-id="b36aa-205">クラスの例では、`Base`メソッドが導入されています`F`と`Overridable`メソッド`G`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-205">In the example, class `Base` introduces a method `F` and an `Overridable` method `G`.</span></span> <span data-ttu-id="b36aa-206">クラスは、`Derived`新しい方法が導入されました`F`、そのため、継承されたシャドウ`F`、継承されたメソッドもオーバーライドと`G`。</span><span class="sxs-lookup"><span data-stu-id="b36aa-206">The class `Derived` introduces a new method `F`, thus shadowing the inherited `F`, and also overrides the inherited method `G`.</span></span> <span data-ttu-id="b36aa-207">この例では次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-207">The example produces the following output:</span></span>

```
Base.F
Derived.F
Derived.G
Derived.G
```

<span data-ttu-id="b36aa-208">注意ステートメント`b.G()`呼び出す`Derived.G`ではなく、`Base.G`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-208">Notice that the statement `b.G()` invokes `Derived.G`, not `Base.G`.</span></span> <span data-ttu-id="b36aa-209">これは、実行時の型のインスタンスのためです (これは`Derived`) インスタンスのコンパイル時の型ではなく (これは`Base`) を呼び出す実際のメソッドの実装を決定します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-209">This is because the run-time type of the instance (which is `Derived`) rather than the compile-time type of the instance (which is `Base`) determines the actual method implementation to invoke.</span></span>

### <a name="shared-methods"></a><span data-ttu-id="b36aa-210">共有メソッド</span><span class="sxs-lookup"><span data-stu-id="b36aa-210">Shared Methods</span></span>

<span data-ttu-id="b36aa-211">`Shared`修飾子は、メソッドは、ことを示します、*メソッドを共有*します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-211">The `Shared` modifier indicates a method is a *shared method*.</span></span> <span data-ttu-id="b36aa-212">共有メソッドでは、型の特定のインスタンスで動作せず、型の特定のインスタンスではなく、型から直接呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-212">A shared method does not operate on a specific instance of a type and may be invoked directly from a type rather than through a particular instance of a type.</span></span> <span data-ttu-id="b36aa-213">ただし、共有メソッドを修飾するためにインスタンスを使用するには、有効です。</span><span class="sxs-lookup"><span data-stu-id="b36aa-213">It is valid, however, to use an instance to qualify a shared method.</span></span> <span data-ttu-id="b36aa-214">参照することはできません`Me`、 `MyClass`、または`MyBase`共有メソッドで。</span><span class="sxs-lookup"><span data-stu-id="b36aa-214">It is invalid to refer to `Me`, `MyClass`, or `MyBase` in a shared method.</span></span> <span data-ttu-id="b36aa-215">共有メソッドができない可能性があります`Overridable`、 `NotOverridable`、または`MustOverride`、およびメソッドを無効にする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-215">Shared methods may not be `Overridable`, `NotOverridable`, or `MustOverride`, and they may not override methods.</span></span> <span data-ttu-id="b36aa-216">標準的なモジュールとインターフェイスで定義されているメソッドは指定できません`Shared`暗黙的にいるため、`Shared`既にします。</span><span class="sxs-lookup"><span data-stu-id="b36aa-216">Methods defined in standard modules and interfaces may not specify `Shared`, because they are implicitly `Shared` already.</span></span>

<span data-ttu-id="b36aa-217">構造体またはクラスなしで宣言されたメソッド、`Shared`修飾子は、*インスタンス メソッド*します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-217">A method declared in a structure or class without a `Shared` modifier is an *instance method*.</span></span> <span data-ttu-id="b36aa-218">インスタンス メソッドは、指定された型のインスタンスで動作します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-218">An instance method operates on a given instance of a type.</span></span> <span data-ttu-id="b36aa-219">インスタンス メソッド、型のインスタンスを介して呼び出すことができますのみと可能性がありますからインスタンスを参照してください、`Me`式。</span><span class="sxs-lookup"><span data-stu-id="b36aa-219">Instance methods can only be invoked through an instance of a type and may refer to the instance through the `Me` expression.</span></span>

<span data-ttu-id="b36aa-220">次の例は、共有へのアクセスとインスタンス メンバーのルールを示しています。</span><span class="sxs-lookup"><span data-stu-id="b36aa-220">The following example illustrates the rules for accessing shared and instance members:</span></span>

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

<span data-ttu-id="b36aa-221">メソッド`F`インスタンス関数のメンバーでは、識別子を両方のインスタンス メンバーと共有メンバーへのアクセスに使用でくことを示します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-221">Method `F` shows that in an instance function member, an identifier can be used to access both instance members and shared members.</span></span> <span data-ttu-id="b36aa-222">メソッド`G`共有関数メンバーの識別子を使ってインスタンス メンバーにアクセスするとエラーをあることを示します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-222">Method `G` shows that in a shared function member, it is an error to access an instance member through an identifier.</span></span> <span data-ttu-id="b36aa-223">メソッド`Main`こと、メンバー アクセス式でインスタンス、インスタンス メンバーにアクセスする必要がありますが、型またはインスタンスを通じてアクセスできる共有メンバーを示しています。</span><span class="sxs-lookup"><span data-stu-id="b36aa-223">Method `Main` shows that in a member access expression, instance members must be accessed through instances, but shared members can be accessed through types or instances.</span></span>

### <a name="method-parameters"></a><span data-ttu-id="b36aa-224">メソッドのパラメーター</span><span class="sxs-lookup"><span data-stu-id="b36aa-224">Method Parameters</span></span>

<span data-ttu-id="b36aa-225">A*パラメーター*におよび、メソッドからの情報を渡すために使用できる変数です。</span><span class="sxs-lookup"><span data-stu-id="b36aa-225">A *parameter* is a variable that can be used to pass information into and out of a method.</span></span> <span data-ttu-id="b36aa-226">メソッドのパラメーターは、コンマで区切られた 1 つまたは複数のパラメーターで構成されるメソッドのパラメーター リストで宣言されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-226">Parameters of a method are declared by the method's parameter list, which consists of one or more parameters separated by commas.</span></span>

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

<span data-ttu-id="b36aa-227">パラメーターの型を指定しないと、厳密な型が使用される、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-227">If no type is specified for a parameter and strict semantics are used, a compile-time error occurs.</span></span> <span data-ttu-id="b36aa-228">既定の型がそれ以外の場合は`Object`またはパラメーターの型の文字の種類。</span><span class="sxs-lookup"><span data-stu-id="b36aa-228">Otherwise the default type is `Object` or the type of the parameter's type character.</span></span> <span data-ttu-id="b36aa-229">1 つのパラメーターが含まれている場合、寛容であっても、`As`句では、すべてのパラメーターの種類を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-229">Even under permissive semantics, if one parameter includes an `As` clause, all parameters must specify types.</span></span>

<span data-ttu-id="b36aa-230">パラメーターは修飾子によってパラメーター値、参照、省略可能なまたは paramarray パラメーターとして指定された`ByVal`、 `ByRef`、 `Optional`、および`ParamArray`、それぞれします。</span><span class="sxs-lookup"><span data-stu-id="b36aa-230">Parameters are specified as value, reference, optional, or paramarray parameters by the modifiers `ByVal`, `ByRef`, `Optional`, and `ParamArray`, respectively.</span></span> <span data-ttu-id="b36aa-231">指定されていないパラメーター`ByRef`または`ByVal`の既定値は`ByVal`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-231">A parameter that does not specify `ByRef` or `ByVal` defaults to `ByVal`.</span></span>

<span data-ttu-id="b36aa-232">パラメーターの名前は、メソッドの本文全体のスコープし、は常にパブリックにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="b36aa-232">Parameter names are scoped to the entire body of the method and are always publicly accessible.</span></span> <span data-ttu-id="b36aa-233">メソッドの呼び出しのパラメーター、その呼び出しに固有のコピーが作成し、呼び出しの引数リストが値または変数参照を新しく作成されたパラメーターを割り当てます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-233">A method invocation creates a copy, specific to that invocation, of the parameters, and the argument list of the invocation assigns values or variable references to the newly created parameters.</span></span> <span data-ttu-id="b36aa-234">外部メソッドの宣言とデリゲートの宣言には、本文があるありません、ため、重複するパラメーター名はパラメーター リストで許可されているが、推奨されません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-234">Because external method declarations and delegate declarations have no body, duplicate parameter names are allowed in parameter lists, but discouraged.</span></span>

<span data-ttu-id="b36aa-235">識別子の名前が null 許容修飾子後にこと`?`、null 値を許可し、これが配列で配列名の修飾子を示すためにもします。</span><span class="sxs-lookup"><span data-stu-id="b36aa-235">The identifier may be followed by the nullable name modifier `?` to indicate that it is nullable, and also by array name modifiers to indicate that it is an array.</span></span> <span data-ttu-id="b36aa-236">組み合わせることができます、例:"`ByVal x?() As Integer`"。</span><span class="sxs-lookup"><span data-stu-id="b36aa-236">They may be combined, e.g. "`ByVal x?() As Integer`".</span></span> <span data-ttu-id="b36aa-237">明示的な配列の境界を使用することはできません。また、名前が null 許容修飾子が存在する場合、`As`句が存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-237">It is not allowed to use explicit array bounds; also, if the nullable name modifier is present then an `As` clause must be present.</span></span>


#### <a name="value-parameters"></a><span data-ttu-id="b36aa-238">値パラメーター</span><span class="sxs-lookup"><span data-stu-id="b36aa-238">Value Parameters</span></span>

<span data-ttu-id="b36aa-239">A*値パラメーター*明示的な宣言が`ByVal`修飾子。</span><span class="sxs-lookup"><span data-stu-id="b36aa-239">A *value parameter* is declared with an explicit `ByVal` modifier.</span></span> <span data-ttu-id="b36aa-240">場合、`ByVal`修飾子を使用すると、`ByRef`修飾子が指定されていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-240">If the `ByVal` modifier is used, the `ByRef` modifier may not be specified.</span></span> <span data-ttu-id="b36aa-241">値を持つパラメーターが存在すると、呼び出しで指定された引数の値を持つパラメーターが属するで初期化されるメンバーの呼び出しに渡されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-241">A value parameter comes into existence with the invocation of the member the parameter belongs to, and is initialized with the value of the argument given in the invocation.</span></span> <span data-ttu-id="b36aa-242">値を持つパラメーター、メンバーの戻り時に存在しなくなります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-242">A value parameter ceases to exist upon return of the member.</span></span>

<span data-ttu-id="b36aa-243">値を持つパラメーターに新しい値を割り当てるメソッドが許可されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-243">A method is permitted to assign new values to a value parameter.</span></span> <span data-ttu-id="b36aa-244">このような割り当てでは、値パラメーターによって表されるローカル ストレージの場所のみに影響します。実際にメソッドの呼び出しで指定された引数に影響を与えるありません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-244">Such assignments only affect the local storage location represented by the value parameter; they have no effect on the actual argument given in the method invocation.</span></span>

<span data-ttu-id="b36aa-245">値を持つパラメーターは、引数の値が、メソッドに渡され、元の引数のパラメーターの変更に影響しないときに使用されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-245">A value parameter is used when the value of an argument is passed into a method, and modifications of the parameter do not impact the original argument.</span></span> <span data-ttu-id="b36aa-246">値を持つパラメーターは、いずれかの対応する引数の変数とは異なりますが、独自の変数を指します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-246">A value parameter refers to its own variable, one that is distinct from the variable of the corresponding argument.</span></span> <span data-ttu-id="b36aa-247">この変数は、対応する引数の値をコピーして初期化されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-247">This variable is initialized by copying the value of the corresponding argument.</span></span> <span data-ttu-id="b36aa-248">次の例は、メソッドを示しています`F`という名前の値を持つパラメーターを持つ`p`:。</span><span class="sxs-lookup"><span data-stu-id="b36aa-248">The following example shows a method `F` that has a value parameter named `p`:</span></span>

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

<span data-ttu-id="b36aa-249">例では、生成、次の出力も、value パラメーター`p`が変更されました。</span><span class="sxs-lookup"><span data-stu-id="b36aa-249">The example produces the following output, even though the value parameter `p` is modified:</span></span>

```
pre: a = 1
p = 1
post: a = 1
```

#### <a name="reference-parameters"></a><span data-ttu-id="b36aa-250">参照パラメーター</span><span class="sxs-lookup"><span data-stu-id="b36aa-250">Reference Parameters</span></span>

<span data-ttu-id="b36aa-251">参照はで宣言されたパラメーターを`ByRef`修飾子。</span><span class="sxs-lookup"><span data-stu-id="b36aa-251">A reference parameter is a parameter declared with a `ByRef` modifier.</span></span> <span data-ttu-id="b36aa-252">場合、`ByRef`修飾子が指定された、`ByVal`修飾子を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-252">If the `ByRef` modifier is specified, the `ByVal` modifier may not be used.</span></span> <span data-ttu-id="b36aa-253">参照パラメーターは、新しい記憶域の場所を作成できません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-253">A reference parameter does not create a new storage location.</span></span> <span data-ttu-id="b36aa-254">代わりに、参照パラメーターは、メソッドまたはコンス トラクターの呼び出しで引数として指定された変数を表します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-254">Instead, a reference parameter represents the variable given as the argument in the method or constructor invocation.</span></span> <span data-ttu-id="b36aa-255">概念的には、参照パラメーターの値は常に、基になる変数と同じです。</span><span class="sxs-lookup"><span data-stu-id="b36aa-255">Conceptually, the value of a reference parameter is always the same as the underlying variable.</span></span>

<span data-ttu-id="b36aa-256">参照パラメーターの動作として 2 つのモードで*エイリアス*または*コピー-バックアップ コピーで。*</span><span class="sxs-lookup"><span data-stu-id="b36aa-256">Reference parameters act in two modes, either as *aliases* or through *copy-in copy-back.*</span></span>

<span data-ttu-id="b36aa-257">__エイリアスです。__</span><span class="sxs-lookup"><span data-stu-id="b36aa-257">__Aliases.__</span></span> <span data-ttu-id="b36aa-258">パラメーターが呼び出し元が提供する引数のエイリアスとして機能する場合に、参照パラメーターが使用されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-258">A reference parameter is used when the parameter acts as an alias for a caller-provided argument.</span></span> <span data-ttu-id="b36aa-259">参照パラメーター自体は、変数を定義しませんが、対応する引数の変数を参照します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-259">A reference parameter does not itself define a variable, but rather refers to the variable of the corresponding argument.</span></span> <span data-ttu-id="b36aa-260">直接とすぐに参照パラメーターの変更、対応する引数に影響します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-260">Modifications of a reference parameter directly and immediately impact the corresponding argument.</span></span> <span data-ttu-id="b36aa-261">次の例は、メソッドを示しています。`Swap`を持つ 2 つの参照パラメーター。</span><span class="sxs-lookup"><span data-stu-id="b36aa-261">The following example shows a method `Swap` that has two reference parameters:</span></span>

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

<span data-ttu-id="b36aa-262">プログラムの出力は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="b36aa-262">The output of the program is:</span></span>

```
pre: x = 1, y = 2
post: x = 2, y = 1
```

<span data-ttu-id="b36aa-263">メソッドの呼び出しの`Swap`クラスで`Main`、`a`表します`x,`と`b`表します`y`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-263">For the invocation of method `Swap` in class `Main`, `a` represents `x,` and `b` represents `y`.</span></span> <span data-ttu-id="b36aa-264">このため、呼び出しは、の値を交換`x`と`y`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-264">Thus, the invocation has the effect of swapping the values of `x` and `y`.</span></span>

<span data-ttu-id="b36aa-265">参照パラメーターをとるメソッドでは複数の名前が同じ記憶域の場所を表すことができます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-265">In a method that takes reference parameters, it is possible for multiple names to represent the same storage location:</span></span>

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

<span data-ttu-id="b36aa-266">メソッドの呼び出しの例では`F`で`G`への参照を渡す`s`両方の`a`と`b`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-266">In the example the invocation of method `F` in `G` passes a reference to `s` for both `a` and `b`.</span></span> <span data-ttu-id="b36aa-267">したがって、その呼び出しでは、名前の`s`、 `a`、および`b`すべて、同じストレージの場所を参照し、すべての 3 つの割り当ては、インスタンス変数を変更`s`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-267">Thus, for that invocation, the names `s`, `a`, and `b` all refer to the same storage location, and the three assignments all modify the instance variable `s`.</span></span>

<span data-ttu-id="b36aa-268">__コピーのコピー-バックします。__</span><span class="sxs-lookup"><span data-stu-id="b36aa-268">__Copy-in copy-back.__</span></span> <span data-ttu-id="b36aa-269">参照パラメーターに渡される変数の型が参照パラメーターの型と互換性がない場合または非変数 (例: プロパティ) が参照パラメーターに引数として渡された場合や、呼び出しが遅延バインディングされた一時的な場合変数が割り当てられ、参照パラメーターに渡されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-269">If the type of the variable being passed to a reference parameter is not compatible with the reference parameter's type, or if a non-variable (e.g. a property) is passed as an argument to a reference parameter, or if the invocation is late-bound, then a temporary variable is allocated and passed to the reference parameter.</span></span> <span data-ttu-id="b36aa-270">渡される値は、メソッドが呼び出されにコピー元の変数 (存在する場合とが書き込み可能な場合など)、メソッドが戻るときの前に、この一時変数にコピーされます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-270">The value being passed in will be copied into this temporary variable before the method is invoked and will be copied back to the original variable (if there is one and if it's writable) when the method returns.</span></span> <span data-ttu-id="b36aa-271">したがって、参照パラメーターは、含めることはできませんとは限りません渡される変数の正確な記憶域への参照、メソッドを終了するまで、参照パラメーターへの変更を変数に反映されません可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-271">Thus, a reference parameter may not necessarily contain a reference to the exact storage of the variable being passed in, and any changes to the reference parameter may not be reflected in the variable until the method exits.</span></span> <span data-ttu-id="b36aa-272">例えば:</span><span class="sxs-lookup"><span data-stu-id="b36aa-272">For example:</span></span>

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

<span data-ttu-id="b36aa-273">最初の呼び出しの場合`F`、一時変数が作成されると、プロパティの値`G`に割り当てられに渡される`F`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-273">In the case of the first invocation of `F`, a temporary variable is created and the value of the property `G` is assigned to it and passed into `F`.</span></span> <span data-ttu-id="b36aa-274">戻り時に`F`、一時変数に値がのプロパティに割り当てられている`G`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-274">Upon return from `F`, the value in the temporary variable is assigned back to the property of `G`.</span></span> <span data-ttu-id="b36aa-275">もう 1 つの一時変数を作成する 2 番目のケースの値と`d`に割り当てられに渡される`F`。</span><span class="sxs-lookup"><span data-stu-id="b36aa-275">In the second case, another temporary variable is created and the value of `d` is assigned to it and passed into `F`.</span></span> <span data-ttu-id="b36aa-276">戻るときに`F`、一時変数に値をキャストして、変数の型に戻す`Derived`とに割り当てられている`d`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-276">When returning from `F`, the value in the temporary variable is cast back to the type of the variable, `Derived`, and assigned to `d`.</span></span> <span data-ttu-id="b36aa-277">値から返されたにキャストできない`Derived`、実行時に例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-277">Since the value being passed back cannot be cast to `Derived`, an exception is thrown at run time.</span></span>

#### <a name="optional-parameters"></a><span data-ttu-id="b36aa-278">省略可能なパラメーター</span><span class="sxs-lookup"><span data-stu-id="b36aa-278">Optional Parameters</span></span>

<span data-ttu-id="b36aa-279">省略可能なパラメーターが宣言された、`Optional`修飾子。</span><span class="sxs-lookup"><span data-stu-id="b36aa-279">An optional parameter is declared with the `Optional` modifier.</span></span> <span data-ttu-id="b36aa-280">仮パラメーター リストで、省略可能なパラメーターを後続のパラメーターがありますオプションも;指定しない、`Optional`修飾子は、次のパラメーターでは、コンパイル時エラーをトリガーします。</span><span class="sxs-lookup"><span data-stu-id="b36aa-280">Parameters that follow an optional parameter in the formal parameter list must be optional as well; failure to specify the `Optional` modifier on the following parameters will trigger a compile-time error.</span></span> <span data-ttu-id="b36aa-281">いくつかのオプションのパラメーターが null 許容型を入力`T?`または非許容型`T`定数式を指定する必要があります`e`引数が指定されていない場合、既定値として使用します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-281">An optional parameter of some type nullable type `T?` or non-nullable type `T` must specify a constant expression `e` to be used as a default value if no argument is specified.</span></span> <span data-ttu-id="b36aa-282">場合`e`に評価される`Nothing`オブジェクト、既定値の型の*パラメーターの型*パラメーターの既定値として使用されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-282">If `e` evaluates to `Nothing` of type Object, then the default value of the *parameter type* will be used as the default for the parameter.</span></span> <span data-ttu-id="b36aa-283">それ以外の場合、`CType(e, T)`定数式である必要があり、パラメーターの既定値として取得されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-283">Otherwise, `CType(e, T)` must be a constant expression and it is taken as the default for the parameter.</span></span>

<span data-ttu-id="b36aa-284">省略可能なパラメーターは、パラメーター初期化子が有効である場合だけです。</span><span class="sxs-lookup"><span data-stu-id="b36aa-284">Optional parameters are the only situation in which an initializer on a parameter is valid.</span></span> <span data-ttu-id="b36aa-285">初期化は、常に、メソッド本体内ではなく、invocation 式の一部として実行します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-285">The initialization is always done as a part of the invocation expression, not within the method body itself.</span></span>

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

<span data-ttu-id="b36aa-286">プログラムの出力は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="b36aa-286">The output of the program is:</span></span>

```
x = 10, y = 20
x = 30, y = 40
```

<span data-ttu-id="b36aa-287">デリゲートまたはイベントの宣言にもラムダ式には、省略可能なパラメーターを指定できません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-287">Optional parameters may not be specified in delegate or event declarations, nor in lambda expressions.</span></span>

#### <a name="paramarray-parameters"></a><span data-ttu-id="b36aa-288">ParamArray パラメーター</span><span class="sxs-lookup"><span data-stu-id="b36aa-288">ParamArray Parameters</span></span>

<span data-ttu-id="b36aa-289">`ParamArray` 宣言されたパラメーターは、`ParamArray`修飾子。</span><span class="sxs-lookup"><span data-stu-id="b36aa-289">`ParamArray` parameters are declared with the `ParamArray` modifier.</span></span> <span data-ttu-id="b36aa-290">場合、`ParamArray`修飾子が存在する、`ByVal`修飾子を指定する必要があります、およびその他のパラメーターを使用しない可能性があります、`ParamArray`修飾子。</span><span class="sxs-lookup"><span data-stu-id="b36aa-290">If the `ParamArray` modifier is present, the `ByVal` modifier must be specified, and no other parameter may use the `ParamArray` modifier.</span></span> <span data-ttu-id="b36aa-291">`ParamArray`パラメーターの型は 1 次元配列である必要があり、パラメーター リストの最後のパラメーターがあります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-291">The `ParamArray` parameter's type must be a one-dimensional array, and it must be the last parameter in the parameter list.</span></span>

<span data-ttu-id="b36aa-292">A`ParamArray`パラメーターの型のパラメーターの不確定の数を表します、`ParamArray`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-292">A `ParamArray` parameter represents an indeterminate number of parameters of the type of the `ParamArray`.</span></span> <span data-ttu-id="b36aa-293">メソッド自体内で、`ParamArray`パラメーターが宣言された型として扱われ、特別な意味を持ちません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-293">Within the method itself, a `ParamArray` parameter is treated as its declared type and has no special semantics.</span></span> <span data-ttu-id="b36aa-294">A`ParamArray`パラメーターは暗黙的に省略可能で、既定値は空の型の 1 次元配列の`ParamArray`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-294">A `ParamArray` parameter is implicitly optional, with a default value of an empty one-dimensional array of the type of the `ParamArray`.</span></span>

<span data-ttu-id="b36aa-295">A`ParamArray`メソッドの呼び出しで 2 つの方法のいずれかで指定する引数を許可します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-295">A `ParamArray` permits arguments to be specified in one of two ways in a method invocation:</span></span>

* <span data-ttu-id="b36aa-296">指定された引数を`ParamArray`を拡張する型の 1 つの式を指定できます、`ParamArray`型。</span><span class="sxs-lookup"><span data-stu-id="b36aa-296">The argument given for a `ParamArray` can be a single expression of a type that widens to the `ParamArray` type.</span></span> <span data-ttu-id="b36aa-297">ここで、`ParamArray`値パラメーターとまったく同じように機能します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-297">In this case, the `ParamArray` acts precisely like a value parameter.</span></span>

* <span data-ttu-id="b36aa-298">または、呼び出しがの 0 個以上の引数を指定できます、`ParamArray`各引数の要素の型に暗黙的に変換できる型の式は、`ParamArray`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-298">Alternatively, the invocation can specify zero or more arguments for the `ParamArray`, where each argument is an expression of a type that is implicitly convertible to the element type of the `ParamArray`.</span></span> <span data-ttu-id="b36aa-299">この場合、呼び出しがのインスタンスを作成、`ParamArray`引数の数、配列の要素が指定した引数の値を持つインスタンスを初期化します使用して、新しく作成された配列のインスタンスとして、実際に対応する長さを持つ型引数。</span><span class="sxs-lookup"><span data-stu-id="b36aa-299">In this case, the invocation creates an instance of the `ParamArray` type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="b36aa-300">を除き、呼び出しで可変個の引数を許可、`ParamArray`次の例に示すように、同じ型の値を持つパラメーターとまったく同じです。</span><span class="sxs-lookup"><span data-stu-id="b36aa-300">Except for allowing a variable number of arguments in an invocation, a `ParamArray` is precisely equivalent to a value parameter of the same type, as the following example illustrates.</span></span>

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

<span data-ttu-id="b36aa-301">この例には、出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-301">The example produces the output</span></span>

```
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

<span data-ttu-id="b36aa-302">最初の呼び出し`F`配列を渡すだけ`a`値パラメーターとして。</span><span class="sxs-lookup"><span data-stu-id="b36aa-302">The first invocation of `F` simply passes the array `a` as a value parameter.</span></span> <span data-ttu-id="b36aa-303">2 番目の呼び出しの`F`自動的に指定された要素値を持つ 4 つの要素の配列を作成し、その配列インスタンス値を持つパラメーターとして渡されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-303">The second invocation of `F` automatically creates a four-element array with the given element values and passes that array instance as a value parameter.</span></span> <span data-ttu-id="b36aa-304">3 つ目の呼び出しでは同様に、`F`要素が 0 の配列を作成し、値パラメーターとしてそのインスタンスを渡します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-304">Likewise, the third invocation of `F` creates a zero-element array and passes that instance as a value parameter.</span></span> <span data-ttu-id="b36aa-305">2 番目と 3 番目の呼び出しは、書き込みを正確に同じです。</span><span class="sxs-lookup"><span data-stu-id="b36aa-305">The second and third invocations are precisely equivalent to writing:</span></span>

```vb
F(New Integer() {10, 20, 30, 40})
F(New Integer() {})
```

<span data-ttu-id="b36aa-306">`ParamArray` デリゲートまたはイベント宣言で、パラメーターを指定しない場合があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-306">`ParamArray` parameters may not be specified in delegate or event declarations.</span></span>

### <a name="event-handling"></a><span data-ttu-id="b36aa-307">イベント処理</span><span class="sxs-lookup"><span data-stu-id="b36aa-307">Event Handling</span></span>

<span data-ttu-id="b36aa-308">メソッドは、インスタンスまたは共有変数にオブジェクトによって発生したイベントを宣言によって処理できます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-308">Methods can declaratively handle events raised by objects in instance or shared variables.</span></span> <span data-ttu-id="b36aa-309">イベントを処理するメソッドの宣言を指定します、`Handles`キーワードと 1 つまたは複数のイベントを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-309">To handle events, a method declaration specifies the `Handles` keyword and lists one or more events.</span></span>

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

<span data-ttu-id="b36aa-310">内のイベント、`Handles`リストはピリオドで区切られた 2 つの識別子によって指定します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-310">An event in the `Handles` list is specified by two identifiers separated by a period:</span></span>

* <span data-ttu-id="b36aa-311">インスタンスまたは共有の変数を指定するコンテナーの型の最初の識別子がある必要があります、`WithEvents`修飾子または`MyBase`または`MyClass`または`Me`キーワードです。 それ以外の場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-311">The first identifier must be an instance or shared variable in the containing type that specifies the `WithEvents` modifier or the `MyBase` or `MyClass` or `Me` keyword; otherwise, a compile-time error occurs.</span></span> <span data-ttu-id="b36aa-312">この変数には、このメソッドによって処理されるイベントを発生させるオブジェクトが含まれています。</span><span class="sxs-lookup"><span data-stu-id="b36aa-312">This variable contains the object that will raise the events handled by this method.</span></span>

* <span data-ttu-id="b36aa-313">2 番目の識別子では、最初の識別子の型のメンバーを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-313">The second identifier must specify a member of the type of the first identifier.</span></span> <span data-ttu-id="b36aa-314">メンバーは、イベントにする必要があり、共有することがあります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-314">The member must be an event, and may be shared.</span></span> <span data-ttu-id="b36aa-315">最初の識別子で共有変数を指定する場合、イベントを共有する必要があります、またはエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-315">If a shared variable is specified for the first identifier, then the event must be shared, or an error results.</span></span>

<span data-ttu-id="b36aa-316">ハンドラー メソッド`M`イベントの有効なイベント ハンドラーと見なされます`E`場合、ステートメント`AddHandler E, AddressOf M`も有効になります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-316">A handler method `M` is considered a valid event handler for an event `E` if the statement `AddHandler E, AddressOf M` would also be valid.</span></span> <span data-ttu-id="b36aa-317">異なり、`AddHandler`ステートメントでは、ただし、明示的なイベント ハンドラーはか厳密な型が使用されているかどうかに関係なく引数なしでメソッドを持つイベントの処理を許可します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-317">Unlike an `AddHandler` statement, however, explicit event handlers allow handling an event with a method with no arguments regardless of whether strict semantics are being used or not:</span></span>

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

<span data-ttu-id="b36aa-318">1 つのメンバーが複数の一致するイベントを処理し、複数のメソッドが 1 つのイベントを処理します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-318">A single member can handle multiple matching events, and multiple methods may handle a single event.</span></span> <span data-ttu-id="b36aa-319">メソッドのアクセシビリティは、イベントを処理する機能に影響を与えません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-319">A method's accessibility has no effect on its ability to handle events.</span></span> <span data-ttu-id="b36aa-320">次の例では、メソッドがイベントを処理する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="b36aa-320">The following example shows how a method can handle events:</span></span>

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

<span data-ttu-id="b36aa-321">これが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-321">This will print out:</span></span>

```
Raised
Raised
```

<span data-ttu-id="b36aa-322">型は、その基本型によって提供されるすべてのイベント ハンドラーを継承します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-322">A type inherits all event handlers provided by its base type.</span></span> <span data-ttu-id="b36aa-323">派生型変更できません。 任意の方法でイベント マッピングは、基本型から継承しますが、イベントに追加のハンドラーを追加することがあります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-323">A derived type cannot in any way alter the event mappings it inherits from its base types, but may add additional handlers to the event.</span></span>


### <a name="extension-methods"></a><span data-ttu-id="b36aa-324">拡張メソッド</span><span class="sxs-lookup"><span data-stu-id="b36aa-324">Extension Methods</span></span>

<span data-ttu-id="b36aa-325">メソッドは、型宣言を使用して外部からの型に追加できる*拡張メソッド*します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-325">Methods can be added to types from outside of the type declaration using *extension methods*.</span></span> <span data-ttu-id="b36aa-326">拡張メソッドは、メソッド、`System.Runtime.CompilerServices.ExtensionAttribute`属性を適用します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-326">Extension methods are methods with the `System.Runtime.CompilerServices.ExtensionAttribute` attribute applied to them.</span></span> <span data-ttu-id="b36aa-327">標準的なモジュールでのみ宣言できますされ、少なくとも 1 つのパラメーター、型、メソッドによって拡張を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-327">They can only be declared in standard modules and must have at least one parameter, which specifies the type the method extends.</span></span> <span data-ttu-id="b36aa-328">たとえば、次の拡張メソッドが型を拡張`String`:</span><span class="sxs-lookup"><span data-stu-id="b36aa-328">For example, the following extension method extends the type `String`:</span></span>

```vb
Imports System.Runtime.CompilerServices

Module StringExtensions
    <Extension> _
    Sub Print(s As String)
        Console.WriteLine(s)
    End Sub
End Module
```

<span data-ttu-id="b36aa-329">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="b36aa-329">__Note.__</span></span> <span data-ttu-id="b36aa-330">Visual Basic では、標準のモジュールで宣言する拡張メソッドが必要ですが、c# などの他の言語は、その他の種類の型で宣言することでことができます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-330">Although Visual Basic requires extension methods to be declared in a standard module, other languages such as C# may allow them to be declared in other kinds of types.</span></span> <span data-ttu-id="b36aa-331">メソッドは、ここで説明した他の規則と包含に従う限り、型がオープン ジェネリック型でないし、インスタンス化することはできません、Visual Basic で拡張メソッドを認識します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-331">As long as the methods follow the other conventions outlined here and the containing type is not an open generic type and cannot be instantiated, Visual Basic will recognize the extension methods.</span></span>

<span data-ttu-id="b36aa-332">拡張メソッドが呼び出されたときで呼び出されたインスタンスは、最初のパラメーターに渡されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-332">When an extension method is invoked, the instance it is being invoked on is passed to the first parameter.</span></span> <span data-ttu-id="b36aa-333">最初のパラメーターを宣言することはできません`Optional`または`ParamArray`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-333">The first parameter cannot be declared `Optional` or `ParamArray`.</span></span> <span data-ttu-id="b36aa-334">型パラメーターを含む、任意の型は、拡張メソッドの最初のパラメーターとして表示できます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-334">Any type, including a type parameter, can appear as the first parameter of an extension method.</span></span> <span data-ttu-id="b36aa-335">次のメソッドが型を拡張するなど、 `Integer()`、任意の型を実装する`System.Collections.Generic.IEnumerable(Of T)`、任意のすべての型と。</span><span class="sxs-lookup"><span data-stu-id="b36aa-335">For example, the following methods extend the types `Integer()`, any type that implements `System.Collections.Generic.IEnumerable(Of T)`, and any type at all:</span></span>

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

<span data-ttu-id="b36aa-336">前の例に示すように、インターフェイスを拡張できます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-336">As the previous example shows, interfaces can be extended.</span></span> <span data-ttu-id="b36aa-337">インターフェイスの拡張メソッドをのみに対して定義された拡張メソッドを持つインターフェイスを実装する型、インターフェイスによって最初に宣言されたメンバーを実装するために、メソッドの実装を指定します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-337">Interface extension methods supply the implementation of the method, so types that implement an interface that has extension methods defined on it still only implement the members originally declared by the interface.</span></span> <span data-ttu-id="b36aa-338">例えば:</span><span class="sxs-lookup"><span data-stu-id="b36aa-338">For example:</span></span>

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

<span data-ttu-id="b36aa-339">拡張メソッドは、型パラメーターの型の制約をこともできと同様、拡張機能の非ジェネリック メソッドの引数の型推論できます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-339">Extension methods can also have type constraints on their type parameters and, just as with non-extension generic methods, type argument can be inferred:</span></span>

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

<span data-ttu-id="b36aa-340">拡張メソッドは、拡張される型内の式の暗黙のインスタンスからもアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-340">Extension methods can also be accessed through implicit instance expressions within the type being extended:</span></span>

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

<span data-ttu-id="b36aa-341">ユーザー補助のために、拡張メソッドは、コンテキストの宣言によりユーザーが持つアクセスを超えて拡張型のメンバーに余分なアクセス権があるありません--で宣言された標準のモジュールのメンバーとしても扱われます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-341">For the purposes of accessibility, extension methods are also treated as members of the standard module they are declared in -- they have no extra access to the members of the type they are extending beyond the access they have by virtue of their declaration context.</span></span>

<span data-ttu-id="b36aa-342">拡張メソッドは、標準のモジュールのメソッドがスコープの場合のみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-342">Extensions methods are only available when the standard module method is in scope.</span></span> <span data-ttu-id="b36aa-343">それ以外の場合、元の型は、拡張されているには表示されません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-343">Otherwise, the original type will not appear to have been extended.</span></span> <span data-ttu-id="b36aa-344">例えば:</span><span class="sxs-lookup"><span data-stu-id="b36aa-344">For example:</span></span>

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

<span data-ttu-id="b36aa-345">型の拡張メソッドのみが使用可能な場合は、型を参照すると、コンパイル時エラーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-345">Referring to a type when only an extension method on the type is available will still produce a compile-time error.</span></span>

<span data-ttu-id="b36aa-346">拡張メソッドが厳密に型指定など、メンバーのバインドをすべてのコンテキストでの型のメンバーであると見なされることを確認することが重要`For Each`パターン。</span><span class="sxs-lookup"><span data-stu-id="b36aa-346">It is important to note that extension methods are considered to be members of the type in all contexts where members are bound, such as the strongly-typed `For Each` pattern.</span></span> <span data-ttu-id="b36aa-347">例えば:</span><span class="sxs-lookup"><span data-stu-id="b36aa-347">For example:</span></span>

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

<span data-ttu-id="b36aa-348">デリゲートは拡張メソッドを参照するも作成されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-348">Delegates can also be created that refer to extension methods.</span></span> <span data-ttu-id="b36aa-349">したがって、次のようなコードがあると:</span><span class="sxs-lookup"><span data-stu-id="b36aa-349">Thus, the code:</span></span>

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

<span data-ttu-id="b36aa-350">約と同等です。</span><span class="sxs-lookup"><span data-stu-id="b36aa-350">is roughly equivalent to:</span></span>

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

<span data-ttu-id="b36aa-351">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="b36aa-351">__Note.__</span></span> <span data-ttu-id="b36aa-352">Visual Basic が通常の原因となるインスタンス メソッドの呼び出しのチェックを挿入、`System.NullReferenceException`メソッドを呼び出す対象のインスタンスが場合に発生する`Nothing`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-352">Visual Basic normally inserts a check on an instance method call that causes a `System.NullReferenceException` to occur if the instance the method is being invoked on is `Nothing`.</span></span> <span data-ttu-id="b36aa-353">拡張機能のメソッドの場合の拡張メソッドが明示的にチェックする必要がありますので、このチェックを挿入する効率的な方法はありません`Nothing`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-353">In the case of extension methods, there is no efficient way to insert this check, so extension methods will need to explicitly check for `Nothing`.</span></span> 

<span data-ttu-id="b36aa-354">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="b36aa-354">__Note.__</span></span> <span data-ttu-id="b36aa-355">として渡されるときに、値の型をボックス化されたが、`ByVal`インターフェイスとして型指定されたパラメーターに引数。</span><span class="sxs-lookup"><span data-stu-id="b36aa-355">A value type will be boxed when being passed as a `ByVal` argument to a parameter typed as an interface.</span></span>  <span data-ttu-id="b36aa-356">つまり、元の代わりに、構造のコピーで、拡張メソッドの副作用が動作することです。</span><span class="sxs-lookup"><span data-stu-id="b36aa-356">This implies that side effects of the extension method will operate on a copy of the structure instead of the original.</span></span> <span data-ttu-id="b36aa-357">拡張メソッドの最初の引数には、言語の制限はありません、お勧め値の型を拡張する拡張メソッドを使用しない、または値の型を拡張するには、最初のパラメーターが渡される`ByRef`側ことを確認するには効果は、元の値で動作します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-357">While the language puts no restrictions on the first argument of an extension method, it is recommended that extension methods are not used to extend value types or that when extending value types, the first parameter is passed `ByRef` to ensure that side effects operate on the original value.</span></span>

### <a name="partial-methods"></a><span data-ttu-id="b36aa-358">部分メソッド</span><span class="sxs-lookup"><span data-stu-id="b36aa-358">Partial Methods</span></span>

<span data-ttu-id="b36aa-359">A*部分メソッド*は署名が、メソッドの本体ではないを指定する方法です。</span><span class="sxs-lookup"><span data-stu-id="b36aa-359">A *partial method* is a method that specifies a signature but not the body of the method.</span></span> <span data-ttu-id="b36aa-360">メソッドの本体は、同じ名前とシグネチャ、型の別の部分宣言で最も可能性の高い別のメソッド宣言で指定できます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-360">The body of the method can be supplied by another method declaration with the same name and signature, most likely in another partial declaration of the type.</span></span> <span data-ttu-id="b36aa-361">例えば:</span><span class="sxs-lookup"><span data-stu-id="b36aa-361">For example:</span></span>

<span data-ttu-id="b36aa-362">a.vb:</span><span class="sxs-lookup"><span data-stu-id="b36aa-362">a.vb:</span></span>

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

<span data-ttu-id="b36aa-363">b.vb:</span><span class="sxs-lookup"><span data-stu-id="b36aa-363">b.vb:</span></span>

```vb
Public Partial Class MyForm
    Public Sub ValidateControls()
        ' Validation logic goes here
        ...
    End Sub
End Class
```

<span data-ttu-id="b36aa-364">この例では、クラスの部分宣言で`MyForm`部分メソッドを宣言`ValidateControls`実装のないです。</span><span class="sxs-lookup"><span data-stu-id="b36aa-364">In this example, a partial declaration of the class `MyForm` declares a partial method `ValidateControls` with no implementation.</span></span> <span data-ttu-id="b36aa-365">部分的な宣言にコンス トラクターは、ファイルで指定された本文がない場合でも、部分のメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-365">The constructor in the partial declaration calls the partial method, even though there is no body supplied in the file.</span></span> <span data-ttu-id="b36aa-366">その他の部分宣言`MyForm`メソッドの実装を提供します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-366">The other partial declaration of `MyForm` then supplies the implementation of the method.</span></span>

<span data-ttu-id="b36aa-367">本文が指定されているかどうかを; に関係なく、部分メソッドを呼び出すことができます。メソッド本体が指定されていない場合、呼び出しは無視されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-367">Partial methods can be called regardless of whether a body has been supplied; if no method body is supplied, the call is ignored.</span></span> <span data-ttu-id="b36aa-368">例えば:</span><span class="sxs-lookup"><span data-stu-id="b36aa-368">For example:</span></span>

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

<span data-ttu-id="b36aa-369">無視される部分メソッドの呼び出しに引数として渡される任意の式も無視され、評価されません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-369">Any expressions that are passed in as arguments to a partial method call that is ignored are ignored also and not evaluated.</span></span> <span data-ttu-id="b36aa-370">(__に注意してください。__</span><span class="sxs-lookup"><span data-stu-id="b36aa-370">(__Note.__</span></span> <span data-ttu-id="b36aa-371">つまり、部分メソッドは部分メソッドにはコストがあるない場合に使用されませんので、2 つの部分型の間で定義されている動作を提供するため、非常に効率的にできます。)</span><span class="sxs-lookup"><span data-stu-id="b36aa-371">This means that partial methods are a very efficient way of providing behavior that is defined across two partial types, since the partial methods have no cost if they are not used.)</span></span>

<span data-ttu-id="b36aa-372">部分メソッド宣言として宣言する必要があります`Private`ありませんステートメント本体内にサブルーチンを常にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-372">The partial method declaration must be declared as `Private` and must always be a subroutine with no statements in its body.</span></span> <span data-ttu-id="b36aa-373">部分メソッド自体を実装できませんインターフェイスのメソッド、メソッドの本文を提供することができます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-373">Partial methods cannot themselves implement interface methods, although the method that supplies their body can.</span></span>

<span data-ttu-id="b36aa-374">1 つのメソッドは、本文の部分メソッドを指定できます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-374">Only one method can supply a body to a partial method.</span></span> <span data-ttu-id="b36aa-375">本文の部分メソッドを提供するメソッドは、部分メソッドとして同じシグネチャでの型パラメーター、同じの宣言修飾子と同じパラメーターと型パラメーター名と同じ制約が必要です。</span><span class="sxs-lookup"><span data-stu-id="b36aa-375">A method supplying a body to a partial method must have the same signature as the partial method, the same constraints on any type parameters, the same declaration modifiers, and the same parameter and type parameter names.</span></span> <span data-ttu-id="b36aa-376">部分メソッドおよびメソッドの本体を提供する属性のメソッドのパラメーターに任意の属性は、マージされます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-376">Attributes on the partial method and the method that supplies its body are merged, as are any attributes on the methods' parameters.</span></span> <span data-ttu-id="b36aa-377">同様に、メソッドを処理するイベントの一覧に結合されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-377">Similarly, the list of events that the methods handle is merged.</span></span> <span data-ttu-id="b36aa-378">例えば:</span><span class="sxs-lookup"><span data-stu-id="b36aa-378">For example:</span></span>

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

## <a name="constructors"></a><span data-ttu-id="b36aa-379">コンストラクター</span><span class="sxs-lookup"><span data-stu-id="b36aa-379">Constructors</span></span>

<span data-ttu-id="b36aa-380">*コンス トラクター*初期化を制御できる特殊なメソッドです。</span><span class="sxs-lookup"><span data-stu-id="b36aa-380">*Constructors* are special methods that allow control over initialization.</span></span> <span data-ttu-id="b36aa-381">プログラムを開始した後、または型のインスタンスが作成されるときに実行されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-381">They are run after the program begins or when an instance of a type is created.</span></span> <span data-ttu-id="b36aa-382">他のメンバーとは異なり、コンス トラクターは継承されず、型の宣言領域に名前を導入しません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-382">Unlike other members, constructors are not inherited and do not introduce a name into a type's declaration space.</span></span> <span data-ttu-id="b36aa-383">コンス トラクターは、オブジェクト作成式または、.NET Framework でのみ呼び出すことがあります。これらが直接呼び出されません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-383">Constructors may only be invoked by object-creation expressions or by the .NET Framework; they may never be directly invoked.</span></span>

<span data-ttu-id="b36aa-384">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="b36aa-384">__Note.__</span></span> <span data-ttu-id="b36aa-385">コンス トラクターでは、サブルーチンが行の配置で同じ制限があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-385">Constructors have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="b36aa-386">最初のステートメント、end ステートメント、およびブロックする必要がありますすべて表示されますは論理行の先頭。</span><span class="sxs-lookup"><span data-stu-id="b36aa-386">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>

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

### <a name="instance-constructors"></a><span data-ttu-id="b36aa-387">インスタンス コンストラクター</span><span class="sxs-lookup"><span data-stu-id="b36aa-387">Instance Constructors</span></span>

<span data-ttu-id="b36aa-388">*インスタンス コンス トラクター*型のインスタンスを初期化し、インスタンスが作成されるときに、.NET Framework で実行されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-388">*Instance constructors* initialize instances of a type and are run by the .NET Framework when an instance is created.</span></span> <span data-ttu-id="b36aa-389">コンス トラクターのパラメーター リストは、メソッドのパラメーター リストと同じ規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-389">The parameter list of a constructor is subject to the same rules as the parameter list of a method.</span></span> <span data-ttu-id="b36aa-390">インスタンス コンス トラクターはオーバー ロードすることがあります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-390">Instance constructors may be overloaded.</span></span>

<span data-ttu-id="b36aa-391">参照型のすべてのコンス トラクターは、別のコンス トラクターを呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-391">All constructors in reference types must invoke another constructor.</span></span> <span data-ttu-id="b36aa-392">呼び出しが明示的な場合、コンス トラクター メソッドの本体の最初のステートメントがあります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-392">If the invocation is explicit, it must be the first statement in the constructor method body.</span></span> <span data-ttu-id="b36aa-393">ステートメント呼び出すことができますか、別の型のインスタンスのコンス トラクター--など`Me.New(...)`または`MyClass.New(...)`--または構造体でない場合は、型の基本型のインスタンス コンス トラクターを呼び出すこともできます - たとえば、`MyBase.New(...)`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-393">The statement can either invoke another of the type's instance constructors -- for example, `Me.New(...)` or `MyClass.New(...)` -- or if it is not a structure it can invoke an instance constructor of the type's base type -- for example, `MyBase.New(...)`.</span></span> <span data-ttu-id="b36aa-394">それ自体を呼び出すコンス トラクターは無効です。</span><span class="sxs-lookup"><span data-stu-id="b36aa-394">It is invalid for a constructor to invoke itself.</span></span> <span data-ttu-id="b36aa-395">コンス トラクターは、別のコンス トラクターの呼び出しを付ける場合`MyBase.New()`は暗黙の型。</span><span class="sxs-lookup"><span data-stu-id="b36aa-395">If a constructor omits a call to another constructor, `MyBase.New()` is implicit.</span></span> <span data-ttu-id="b36aa-396">パラメーターなしの基本型のコンス トラクターがない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-396">If there is no parameterless base type constructor, a compile-time error occurs.</span></span> <span data-ttu-id="b36aa-397">`Me`コンス トラクターの呼び出しステートメントのパラメーターを参照できません、基本クラス コンス トラクターの呼び出しの後までに構築されているとは見なされません`Me`、 `MyClass`、または`MyBase`暗黙的または明示的にします。</span><span class="sxs-lookup"><span data-stu-id="b36aa-397">Because `Me` is not considered to be constructed until after the call to a base class constructor, the parameters to a constructor invocation statement cannot reference `Me`, `MyClass`, or `MyBase` implicitly or explicitly.</span></span>

<span data-ttu-id="b36aa-398">コンス トラクターの最初のステートメントの場合は、フォームの`MyBase.New(...)`、コンス トラクターが暗黙的に型で宣言されているインスタンス変数の変数の初期化子によって指定された初期化を実行します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-398">When a constructor's first statement is of the form `MyBase.New(...)`, the constructor implicitly performs the initializations specified by the variable initializers of the instance variables declared in the type.</span></span> <span data-ttu-id="b36aa-399">これは、直接の基本型のコンス トラクターの呼び出し後にすぐに実行される割り当てのシーケンスに対応します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-399">This corresponds to a sequence of assignments that are executed immediately after invoking the direct base type constructor.</span></span> <span data-ttu-id="b36aa-400">この順序により、基本のインスタンスのすべての変数は、インスタンスへのアクセス権を持つすべてのステートメントが実行される前に、変数の初期化子によって初期化されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-400">Such ordering ensures that all base instance variables are initialized by their variable initializers before any statements that have access to the instance are executed.</span></span> <span data-ttu-id="b36aa-401">例えば:</span><span class="sxs-lookup"><span data-stu-id="b36aa-401">For example:</span></span>

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

<span data-ttu-id="b36aa-402">ときに`New B()`のインスタンスを作成するために使用`B`、次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-402">When `New B()` is used to create an instance of `B`, the following output is produced:</span></span>

```
x = 1, y = 1
```

<span data-ttu-id="b36aa-403">値`y`は`1`変数初期化子は、基底クラスのコンス トラクターが呼び出された後に実行されるためです。</span><span class="sxs-lookup"><span data-stu-id="b36aa-403">The value of `y` is `1` because the variable initializer is executed after the base class constructor is invoked.</span></span> <span data-ttu-id="b36aa-404">変数の初期化子は、型宣言に表示されるテキストの順序で実行されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-404">Variable initializers are executed in the textual order they appear in the type declaration.</span></span>

<span data-ttu-id="b36aa-405">型宣言がのみ`Private`、コンス トラクターはありません。 可能な一般型から派生または型のインスタンスを作成するには、その他の種類の唯一の例外は、型内で入れ子にされた型。</span><span class="sxs-lookup"><span data-stu-id="b36aa-405">When a type declares only `Private` constructors, it is not possible in general for other types to derive from the type or create instances of the type; the only exception is types nested within the type.</span></span> <span data-ttu-id="b36aa-406">`Private` コンス トラクターは、のみを含む型でよく使用される`Shared`メンバー。</span><span class="sxs-lookup"><span data-stu-id="b36aa-406">`Private` constructors are commonly used in types that contain only `Shared` members.</span></span>

<span data-ttu-id="b36aa-407">型にインスタンス コンス トラクターの宣言が含まれていない場合は、既定のコンス トラクターが自動的に提供します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-407">If a type contains no instance constructor declarations, a default constructor is automatically provided.</span></span> <span data-ttu-id="b36aa-408">既定のコンス トラクターは、直接の基本型のパラメーターなしのコンス トラクターを呼び出すだけです。</span><span class="sxs-lookup"><span data-stu-id="b36aa-408">The default constructor simply invokes the parameterless constructor of the direct base type.</span></span> <span data-ttu-id="b36aa-409">直接の基本型が、アクセス可能なパラメーターなしのコンス トラクターを持たない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-409">If the direct base type does not have an accessible parameterless constructor, a compile-time error occurs.</span></span> <span data-ttu-id="b36aa-410">既定のコンス トラクターの宣言されたアクセスの種類は`Public`型でない限り、 `MustInherit`、この場合、既定のコンス トラクターは`Protected`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-410">The declared access type for the default constructor is `Public` unless the type is `MustInherit`, in which case the default constructor is `Protected`.</span></span>

<span data-ttu-id="b36aa-411">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="b36aa-411">__Note.__</span></span> <span data-ttu-id="b36aa-412">既定のアクセス、`MustInherit`型の既定のコンス トラクターは`Protected`ため`MustInherit`クラスを直接作成することはできません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-412">The default access for a `MustInherit` type's default constructor is `Protected` because `MustInherit` classes cannot be created directly.</span></span> <span data-ttu-id="b36aa-413">既定のコンス トラクターを行う意味がないように`Public`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-413">So there is no point in making the default constructor `Public`.</span></span>

<span data-ttu-id="b36aa-414">次の例では、クラスにコンス トラクターの宣言が含まれていないため、既定のコンス トラクターが用意されています。</span><span class="sxs-lookup"><span data-stu-id="b36aa-414">In the following example a default constructor is provided because the class contains no constructor declarations:</span></span>

```vb
Class Message
    Dim sender As Object
    Dim text As String
End Class
```

<span data-ttu-id="b36aa-415">そのため、例は、次とまったく同じです。</span><span class="sxs-lookup"><span data-stu-id="b36aa-415">Thus, the example is precisely equivalent to the following:</span></span>

```vb
Class Message
    Dim sender As Object
    Dim text As String

    Sub New()
    End Sub
End Class
```

<span data-ttu-id="b36aa-416">デザイナーに生成される既定のコンス トラクターには、属性でマークされたクラスが生成される`Microsoft.VisualBasic.CompilerServices.DesignerGeneratedAttribute`メソッドを呼び出して`Sub InitializeComponent()`基底コンス トラクターの呼び出し後、ユーザーが存在します場合。</span><span class="sxs-lookup"><span data-stu-id="b36aa-416">Default constructors that are emitted into a designer generated class marked with the attribute `Microsoft.VisualBasic.CompilerServices.DesignerGeneratedAttribute` will call the method `Sub InitializeComponent()`, if it exists, after the call to the base constructor.</span></span> <span data-ttu-id="b36aa-417">(__に注意してください。__</span><span class="sxs-lookup"><span data-stu-id="b36aa-417">(__Note.__</span></span> <span data-ttu-id="b36aa-418">これにより、デザイナー ファイルにコンス トラクターを省略する場合、WinForms デザイナーによって作成されたものなどのデザイナー生成されたファイルができます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-418">This allows designer generated files, such as those created by the WinForms designer, to omit the constructor in the designer file.</span></span> <span data-ttu-id="b36aa-419">これにより、プログラマは自分で指定を希望する場合。)</span><span class="sxs-lookup"><span data-stu-id="b36aa-419">This enables the programmer to specify it themselves, if they so choose.)</span></span>

### <a name="shared-constructors"></a><span data-ttu-id="b36aa-420">共有のコンス トラクター</span><span class="sxs-lookup"><span data-stu-id="b36aa-420">Shared Constructors</span></span>

<span data-ttu-id="b36aa-421">*コンス トラクターを共有*共有変数の型を初期化は、プログラムが実行を開始した後は、型のメンバーへの参照の前に実行されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-421">*Shared constructors* initialize a type's shared variables; they are run after the program begins executing, but before any references to a member of the type.</span></span> <span data-ttu-id="b36aa-422">共有のコンス トラクターを指定します、`Shared`修飾子は、後者標準モジュールである場合を除き、`Shared`修飾子が暗黙的に指定します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-422">A shared constructor specifies the `Shared` modifier, unless it is in a standard module in which case the `Shared` modifier is implied.</span></span>

<span data-ttu-id="b36aa-423">インスタンス コンス トラクターとは異なりは、共有コンス トラクターは暗黙的なパブリック アクセスのパラメーターを指定しないと、他のコンス トラクターを呼び出すことができません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-423">Unlike instance constructors, shared constructors have implicit public access, have no parameters, and may not call other constructors.</span></span> <span data-ttu-id="b36aa-424">共有のコンス トラクターの最初のステートメントの前に、共有のコンス トラクターは暗黙的に型で宣言された共有変数の変数の初期化子によって指定された初期化を実行します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-424">Before the first statement in a shared constructor, the shared constructor implicitly performs the initializations specified by the variable initializers of the shared variables declared in the type.</span></span> <span data-ttu-id="b36aa-425">これは、一連のコンス トラクターに入ったときにすぐに実行される割り当てに対応します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-425">This corresponds to a sequence of assignments that are executed immediately upon entry to the constructor.</span></span> <span data-ttu-id="b36aa-426">変数の初期化子は、型宣言に表示されるテキストの順序で実行されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-426">The variable initializers are executed in the textual order they appear in the type declaration.</span></span>

<span data-ttu-id="b36aa-427">次の例は、`Employee`共有共有変数を初期化するコンス トラクターを持つクラス。</span><span class="sxs-lookup"><span data-stu-id="b36aa-427">The following example shows an `Employee` class with a shared constructor that initializes a shared variable:</span></span>

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

<span data-ttu-id="b36aa-428">各クローズ ジェネリック型の別の共有のコンス トラクターが存在します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-428">A separate shared constructor exists for each closed generic type.</span></span> <span data-ttu-id="b36aa-429">共有のコンス トラクターは、実行時のチェック制約を使用してコンパイル時にチェックすることはできませんが、型パラメーターに適用する便利な場所がクローズドの種類ごとに 1 回だけ実行されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-429">Because the shared constructor is executed exactly once for each closed type, it is a convenient place to enforce run-time checks on the type parameter that cannot be checked at compile-time via constraints.</span></span> <span data-ttu-id="b36aa-430">次の種類は共有コンス トラクターを使用して、型パラメーターがあることを強制するなど`Integer`または`Double`:</span><span class="sxs-lookup"><span data-stu-id="b36aa-430">For example, the following type uses a shared constructor to enforce that the type parameter is `Integer` or `Double`:</span></span>

```vb
Class EnumHolder(Of T)
    Shared Sub New() 
        If Not GetType(T).IsEnum() Then
            Throw New ArgumentException("T must be an enumerated type.")
        End If
    End Sub
End Class
```

<span data-ttu-id="b36aa-431">共有コンス トラクターが明示的に定義されている場合に、いくつかの保証が提供されますが、実装によっては、ほとんどの場合は、共有コンス トラクターが実行正確に。</span><span class="sxs-lookup"><span data-stu-id="b36aa-431">Exactly when shared constructors are run is mostly implementation dependent, though several guarantees are provided if a shared constructor is explicitly defined:</span></span>

* <span data-ttu-id="b36aa-432">共有のコンス トラクターは、型の任意の静的フィールドに初めてアクセスする前に実行されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-432">Shared constructors are run before the first access to any static field of the type.</span></span>

* <span data-ttu-id="b36aa-433">共有のコンス トラクターは、型の任意の静的メソッドの最初の呼び出しの前に実行されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-433">Shared constructors are run before the first invocation of any static method of the type.</span></span>

* <span data-ttu-id="b36aa-434">共有のコンス トラクターは、型のどのコンス トラクターの最初の呼び出しの前に実行されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-434">Shared constructors are run before the first invocation of any constructor for the type.</span></span>

<span data-ttu-id="b36aa-435">上記の保証は、共有のコンス トラクターが共有の初期化子を暗黙的に作成されているような状況では適用されません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-435">The above guarantees do not apply in the situation where a shared constructor is implicitly created for shared initializers.</span></span> <span data-ttu-id="b36aa-436">次の例からの出力では、ため、読み込みの正確な順序付けし、そのため共有コンス トラクターの実行が定義されていません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-436">The output from the following example is uncertain, because the exact ordering of loading and therefore of shared constructor execution is not defined:</span></span>

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

<span data-ttu-id="b36aa-437">出力は、次のいずれかになります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-437">The output could be either of the following:</span></span>

```
Init A
A.F
Init B
B.F
```

<span data-ttu-id="b36aa-438">または</span><span class="sxs-lookup"><span data-stu-id="b36aa-438">or</span></span>

```
Init B
Init A
A.F
B.F
```

<span data-ttu-id="b36aa-439">これに対し、次の例では、予測可能な出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-439">By contrast, the following example produces predictable output.</span></span> <span data-ttu-id="b36aa-440">なお、`Shared`クラスのコンス トラクター`A`は実行されません、たとえクラス`B`から派生します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-440">Note that the `Shared` constructor for the class `A` never executes, even though class `B` derives from it:</span></span>

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

<span data-ttu-id="b36aa-441">出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-441">The output is:</span></span>

```
Init B
B.G
```

<span data-ttu-id="b36aa-442">循環依存関係を構築することも`Shared`は既定で見られる変数初期化子と変数値の次の例のように、状態。</span><span class="sxs-lookup"><span data-stu-id="b36aa-442">It is also possible to construct circular dependencies that allow `Shared` variables with variable initializers to be observed in their default value state, as in the following example:</span></span>

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

<span data-ttu-id="b36aa-443">これには、出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-443">This produces the output:</span></span>

```
X = 1, Y = 2
```

<span data-ttu-id="b36aa-444">実行する、`Main`メソッド、システムは、まず、クラスを読み込みます`B`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-444">To execute the `Main` method, the system first loads class `B`.</span></span> <span data-ttu-id="b36aa-445">`Shared`クラスのコンス トラクター`B`の初期値を計算に進みます`Y`と、クラスを再帰的に`A`を読み込むための値`A.X`参照されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-445">The `Shared` constructor of class `B` proceeds to compute the initial value of `Y`, which recursively causes class `A` to be loaded because the value of `A.X` is referenced.</span></span> <span data-ttu-id="b36aa-446">`Shared`クラスのコンス トラクター`A`の初期値を計算する続いて`X`、フェッチはそうすることで、*既定*の値`Y`ゼロであります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-446">The `Shared` constructor of class `A` in turn proceeds to compute the initial value of `X`, and in doing so fetches the *default* value of `Y`, which is zero.</span></span> <span data-ttu-id="b36aa-447">`A.X` したがってに初期化`1`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-447">`A.X` is thus initialized to `1`.</span></span> <span data-ttu-id="b36aa-448">読み込むプロセス`A`しが完了するの初期値の計算に返す`Y`がの結果になります`2`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-448">The process of loading `A` then completes, returning to the calculation of the initial value of `Y`, the result of which becomes `2`.</span></span>

<span data-ttu-id="b36aa-449">`Main`メソッドが代わりに、見つからないクラスで`A`例に、次の出力が生成しました。</span><span class="sxs-lookup"><span data-stu-id="b36aa-449">Had the `Main` method instead been located in class `A`, the example would have produced the following output:</span></span>

```
X = 2, Y = 1
```

<span data-ttu-id="b36aa-450">循環参照を回避`Shared`変数初期化子から、一般に、クラスでこのような参照を含む読み込まれる順序を決定することはできません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-450">Avoid circular references in `Shared` variable initializers since it is generally impossible to determine the order in which classes containing such references are loaded.</span></span>

## <a name="events"></a><span data-ttu-id="b36aa-451">イベント</span><span class="sxs-lookup"><span data-stu-id="b36aa-451">Events</span></span>

<span data-ttu-id="b36aa-452">イベントは、特定のオカレンスをコードに通知に使用されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-452">Events are used to notify code of a particular occurrence.</span></span> <span data-ttu-id="b36aa-453">イベントの宣言から成る識別子、デリゲート型またはパラメーター リストのいずれかと、省略可能な`Implements`句。</span><span class="sxs-lookup"><span data-stu-id="b36aa-453">An event declaration consists of an identifier, either a delegate type or a parameter list, and an optional `Implements` clause.</span></span>

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

<span data-ttu-id="b36aa-454">デリゲート型が指定されている場合、デリゲート型は戻り値の型をしないがあります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-454">If a delegate type is specified, the delegate type may not have a return type.</span></span> <span data-ttu-id="b36aa-455">パラメーター リストが指定されている場合に含めることはできません`Optional`または`ParamArray`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="b36aa-455">If a parameter list is specified, it may not contain `Optional` or `ParamArray` parameters.</span></span> <span data-ttu-id="b36aa-456">パラメーターの型やデリゲート型のアクセシビリティ ドメインは、として、同じまたは、イベント自体のアクセシビリティ ドメインのスーパー セットである必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-456">The accessibility domain of the parameter types and/or delegate type must be the same as, or a superset of, the accessibility domain of the event itself.</span></span> <span data-ttu-id="b36aa-457">イベントを指定することで共有する可能性があります、`Shared`修飾子。</span><span class="sxs-lookup"><span data-stu-id="b36aa-457">Events may be shared by specifying the `Shared` modifier.</span></span>

<span data-ttu-id="b36aa-458">型の宣言領域に追加されたメンバー名だけでなく、イベント宣言では、他のいくつかのメンバーが暗黙的に宣言します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-458">In addition to the member name added to the type's declaration space, an event declaration implicitly declares several other members.</span></span> <span data-ttu-id="b36aa-459">指定された名前付きイベント`X`、次のメンバー宣言領域に追加されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-459">Given an event named `X`, the following members are added to the declaration space:</span></span>

* <span data-ttu-id="b36aa-460">入れ子になったデリゲート クラス宣言の形式がメソッドの宣言である場合は、名前付き`XEventHandler`が導入されました。</span><span class="sxs-lookup"><span data-stu-id="b36aa-460">If the form of the declaration is a method declaration, a nested delegate class named `XEventHandler` is introduced.</span></span> <span data-ttu-id="b36aa-461">入れ子になったデリゲート クラスは、メソッドの宣言と一致して、イベントとして同じアクセシビリティを持ちます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-461">The nested delegate class matches the method declaration and has the same accessibility as the event.</span></span> <span data-ttu-id="b36aa-462">デリゲート クラスのパラメーターにパラメーター リスト内の属性が適用されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-462">The attributes in the parameter list apply to the parameters of the delegate class.</span></span>

* <span data-ttu-id="b36aa-463">A`Private`という名前のデリゲートとして型指定されたインスタンス変数`XEvent`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-463">A `Private` instance variable typed as the delegate, named `XEvent`.</span></span>

* <span data-ttu-id="b36aa-464">という名前の 2 つのメソッド`add_X`と`remove_X`することはできませんが呼び出される、オーバーライドまたはオーバー ロードします。</span><span class="sxs-lookup"><span data-stu-id="b36aa-464">Two methods named `add_X` and `remove_X` which cannot be invoked, overridden or overloaded.</span></span>

<span data-ttu-id="b36aa-465">型が上記の名のいずれかに一致する名前を宣言しようとすると、コンパイル時エラーが発生、および、暗黙的な`add_X`と`remove_X`宣言は名前のバインドのため無視されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-465">If a type attempts to declare a name that matches one of the above names, a compile-time error will result, and the implicit `add_X` and `remove_X` declarations are ignored for the purposes of name binding.</span></span> <span data-ttu-id="b36aa-466">派生型でそれらをシャドウすることはできますが、オーバーライドまたは導入のメンバーのオーバー ロードすることはできません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-466">It is not possible to override or overload any of the introduced members, although it is possible to shadow them in derived types.</span></span> <span data-ttu-id="b36aa-467">たとえば、クラス宣言</span><span class="sxs-lookup"><span data-stu-id="b36aa-467">For example, the class declaration</span></span>

```vb
Class Raiser
    Public Event Constructed(i As Integer)
End Class
```

<span data-ttu-id="b36aa-468">次の宣言と同じです。</span><span class="sxs-lookup"><span data-stu-id="b36aa-468">is equivalent to the following declaration</span></span>

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

<span data-ttu-id="b36aa-469">デリゲート型を指定せずに、イベントを宣言する最も簡単かつコンパクトな構文ですが、イベントごとに新しいデリゲート型を宣言するというデメリットがあります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-469">Declaring an event without specifying a delegate type is the simplest and most compact syntax, but has the disadvantage of declaring a new delegate type for each event.</span></span> <span data-ttu-id="b36aa-470">たとえば、次の例では、次の 3 つの非表示のデリゲート型が作成されます、3 つすべてのイベントは、同じパラメーター リストを持っていなくて。</span><span class="sxs-lookup"><span data-stu-id="b36aa-470">For example, in the following example, three hidden delegate types are created, even though all three events have the same parameter list:</span></span>

```vb
Public Class Button
    Public Event Click(sender As Object, e As EventArgs)
    Public Event DoubleClick(sender As Object, e As EventArgs)
    Public Event RightClick(sender As Object, e As EventArgs)
End Class
```

<span data-ttu-id="b36aa-471">次の例では、イベントだけを使用して、同じデリゲート`EventHandler`:</span><span class="sxs-lookup"><span data-stu-id="b36aa-471">In the following example, the events simply use the same delegate, `EventHandler`:</span></span>

```vb
Public Delegate Sub EventHandler(sender As Object, e As EventArgs)

Public Class Button
    Public Event Click As EventHandler
    Public Event DoubleClick As EventHandler
    Public Event RightClick As EventHandler
End Class
```

<span data-ttu-id="b36aa-472">2 つの方法のいずれかでイベントを処理できます。 静的または動的にします。</span><span class="sxs-lookup"><span data-stu-id="b36aa-472">Events can be handled in one of two ways: statically or dynamically.</span></span> <span data-ttu-id="b36aa-473">静的にイベントを処理方が簡単ですし、必要なだけです、`WithEvents`変数と`Handles`句。</span><span class="sxs-lookup"><span data-stu-id="b36aa-473">Statically handling events is simpler and only requires a `WithEvents` variable and a `Handles` clause.</span></span> <span data-ttu-id="b36aa-474">次の例では、クラス`Form1`静的にイベントを処理`Click`オブジェクトの`Button`:</span><span class="sxs-lookup"><span data-stu-id="b36aa-474">In the following example, class `Form1` statically handles the event `Click` of object `Button`:</span></span>

```vb
Public Class Form1
    Public WithEvents Button1 As New Button()

    Public Sub Button1_Click(sender As Object, e As EventArgs) _
           Handles Button1.Click
        Console.WriteLine("Button1 was clicked!")
    End Sub
End Class
```

<span data-ttu-id="b36aa-475">動的なイベント処理は、明示的な接続し、コードを切断する必要があります、イベントに複雑です。</span><span class="sxs-lookup"><span data-stu-id="b36aa-475">Dynamically handling events is more complex because the event must be explicitly connected and disconnected to in code.</span></span> <span data-ttu-id="b36aa-476">ステートメント`AddHandler`ハンドラー イベント、およびステートメントを追加`RemoveHandler`イベントのハンドラーを削除します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-476">The statement `AddHandler` adds a handler for an event, and the statement `RemoveHandler` removes a handler for an event.</span></span> <span data-ttu-id="b36aa-477">次の例では、クラス`Form1`を追加する`Button1_Click`のイベント ハンドラーとして`Button1`の`Click`イベント。</span><span class="sxs-lookup"><span data-stu-id="b36aa-477">The next example shows a class `Form1` that adds `Button1_Click` as an event handler for `Button1`'s `Click` event:</span></span>

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

<span data-ttu-id="b36aa-478">メソッドで`Disconnect`、イベント ハンドラーが削除されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-478">In method `Disconnect`, the event handler is removed.</span></span>


### <a name="custom-events"></a><span data-ttu-id="b36aa-479">カスタム イベント</span><span class="sxs-lookup"><span data-stu-id="b36aa-479">Custom Events</span></span>

<span data-ttu-id="b36aa-480">イベントの宣言が暗黙的にフィールドを定義して、前のセクションで説明した、よう、`add_`メソッド、および`remove_`イベント ハンドラーの追跡に使用されるメソッド。</span><span class="sxs-lookup"><span data-stu-id="b36aa-480">As discussed in the previous section, event declarations implicitly define a field, an `add_` method, and a `remove_` method that are used to keep track of event handlers.</span></span> <span data-ttu-id="b36aa-481">場合によっては、ただし、かもしれませんイベント ハンドラーを追跡するためのカスタム コードを提供することが望ましい。</span><span class="sxs-lookup"><span data-stu-id="b36aa-481">In some situations, however, it may be desirable to provide custom code for tracking event handlers.</span></span> <span data-ttu-id="b36aa-482">たとえば、うち少数のみこれまで、処理する 40 個のフィールドではなくハッシュ テーブルを使用して、追跡する 40 個のイベントを定義するクラスの各イベント ハンドラーがより効率的な可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-482">For example, if a class defines forty events of which only a few will ever be handled, using a hash table instead of forty fields to track the handlers for each event may be more efficient.</span></span> <span data-ttu-id="b36aa-483">*カスタム イベント*を許可する、`add_X`と`remove_X`イベント ハンドラー用のカスタム ストレージを使用するメソッドを明示的に定義できます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-483">*Custom events* allow the `add_X` and `remove_X` methods to be defined explicitly, which enables custom storage for event handlers.</span></span>

<span data-ttu-id="b36aa-484">カスタム イベントは、例外を使用して、デリゲート型を指定するイベントが宣言ことと同じ方法で宣言されているキーワード`Custom`前に指定する必要があります、`Event`キーワード。</span><span class="sxs-lookup"><span data-stu-id="b36aa-484">Custom events are declared in the same way that events that specify a delegate type are declared, with the exception that the keyword `Custom` must precede the `Event` keyword.</span></span> <span data-ttu-id="b36aa-485">カスタム イベント宣言には、3 つの宣言が含まれています。`AddHandler`宣言、`RemoveHandler`宣言と`RaiseEvent`宣言します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-485">A custom event declaration contains three declarations: an `AddHandler` declaration, a `RemoveHandler` declaration and a `RaiseEvent` declaration.</span></span> <span data-ttu-id="b36aa-486">属性があることができますが、任意の修飾子、宣言のいずれもことができます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-486">None of the declarations can have any modifiers, although they can have attributes.</span></span>

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

<span data-ttu-id="b36aa-487">例えば:</span><span class="sxs-lookup"><span data-stu-id="b36aa-487">For example:</span></span>

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

<span data-ttu-id="b36aa-488">`AddHandler`と`RemoveHandler`宣言は、いずれか`ByVal`パラメーターで、イベントのデリゲート型でなければなりません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-488">The `AddHandler` and `RemoveHandler` declaration take one `ByVal` parameter, which must be of the delegate type of the event.</span></span> <span data-ttu-id="b36aa-489">ときに、`AddHandler`または`RemoveHandler`ステートメントが実行される (または`Handles`句が自動的にイベントを処理)、対応する宣言が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-489">When an `AddHandler` or `RemoveHandler` statement is executed (or a `Handles` clause automatically handles an event), the corresponding declaration will be called.</span></span> <span data-ttu-id="b36aa-490">`RaiseEvent`宣言は、イベントのデリゲートと同じパラメーターを受け取るし、呼び出されるときに、`RaiseEvent`ステートメントが実行されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-490">The `RaiseEvent` declaration takes the same parameters as the event delegate and will be called when a `RaiseEvent` statement is executed.</span></span> <span data-ttu-id="b36aa-491">すべての宣言する必要がありますを指定して、サブルーチンと見なされます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-491">All of the declarations must be provided and are considered to be subroutines.</span></span>

<span data-ttu-id="b36aa-492">なお`AddHandler`、`RemoveHandler`と`RaiseEvent`宣言サブルーチンが行の配置で同じ制限があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-492">Note that `AddHandler`, `RemoveHandler` and `RaiseEvent` declarations have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="b36aa-493">最初のステートメント、end ステートメント、およびブロックする必要がありますすべて表示されますは論理行の先頭。</span><span class="sxs-lookup"><span data-stu-id="b36aa-493">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>

<span data-ttu-id="b36aa-494">型の宣言領域に追加されたメンバー名だけでなく、カスタム イベント宣言では、他のいくつかのメンバーが暗黙的に宣言します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-494">In addition to the member name added to the type's declaration space, a custom event declaration implicitly declares several other members.</span></span> <span data-ttu-id="b36aa-495">指定された名前付きイベント`X`、次のメンバー宣言領域に追加されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-495">Given an event named `X`, the following members are added to the declaration space:</span></span>

* <span data-ttu-id="b36aa-496">という名前のメソッド`add_X`に対応する、`AddHandler`宣言します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-496">A method named `add_X`, corresponding to the `AddHandler` declaration.</span></span>

* <span data-ttu-id="b36aa-497">という名前のメソッド`remove_X`に対応する、`RemoveHandler`宣言します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-497">A method named `remove_X`, corresponding to the `RemoveHandler` declaration.</span></span>

* <span data-ttu-id="b36aa-498">という名前のメソッド`fire_X`に対応する、`RaiseEvent`宣言します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-498">A method named `fire_X`, corresponding to the `RaiseEvent` declaration.</span></span>

<span data-ttu-id="b36aa-499">型が上記の名のいずれかに一致する名前を宣言しようとすると、コンパイル時エラーが発生し、名前のバインドのため、暗黙的な宣言すべて無視されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-499">If a type attempts to declare a name that matches one of the above names, a compile-time error will result, and the implicit declarations are all ignored for the purposes of name binding.</span></span> <span data-ttu-id="b36aa-500">派生型でそれらをシャドウすることはできますが、オーバーライドまたは導入のメンバーのオーバー ロードすることはできません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-500">It is not possible to override or overload any of the introduced members, although it is possible to shadow them in derived types.</span></span>

<span data-ttu-id="b36aa-501">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="b36aa-501">__Note.__</span></span> <span data-ttu-id="b36aa-502">`Custom` 予約語ではありません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-502">`Custom` is not a reserved word.</span></span>

#### <a name="custom-events-in-winrt-assemblies"></a><span data-ttu-id="b36aa-503">WinRT のアセンブリのカスタム イベント</span><span class="sxs-lookup"><span data-stu-id="b36aa-503">Custom events in WinRT assemblies</span></span>

<span data-ttu-id="b36aa-504">Microsoft Visual Basic の 11.0 の時点でコンパイルされたファイルで宣言されたイベント`/target:winmdobj`、またはファイル内のインターフェイスで宣言されている、別の場所では、実装しは少し異なる方法で扱われます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-504">As of Microsoft Visual Basic 11.0, events declared in a file compiled with `/target:winmdobj`, or declared in an interface in such a file and then implemented elsewhere, are treated a little differently.</span></span>

* <span data-ttu-id="b36aa-505">許可など特定のデリゲート型のみは通常、winmd を作成するため、外部ツール`System.EventHandler(Of T)`または`System.TypedEventHandle(Of T, U)`、および他のユーザーは禁止されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-505">External tools used to build the winmd will typically allow only certain delegate types such as `System.EventHandler(Of T)` or `System.TypedEventHandle(Of T, U)`, and will disallow others.</span></span>

* <span data-ttu-id="b36aa-506">`XEvent`フィールドの型が`System.Runtime.InteropServices.WindowsRuntime.EventRegistrationTokenTable(Of T)`場所`T`はデリゲート型です。</span><span class="sxs-lookup"><span data-stu-id="b36aa-506">The `XEvent` field has type `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationTokenTable(Of T)` where `T` is the delegate type.</span></span>

* <span data-ttu-id="b36aa-507">AddHandler アクセサーを返します、 `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationToken`、RemoveHandler アクセサーは、同じ型の 1 つのパラメーターを受け取るとします。</span><span class="sxs-lookup"><span data-stu-id="b36aa-507">The AddHandler accessor returns a `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationToken`, and the RemoveHandler accessor takes a single parameter of the same type.</span></span>

<span data-ttu-id="b36aa-508">このようなカスタム イベントの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-508">Here is an example of such a custom event.</span></span>

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


## <a name="constants"></a><span data-ttu-id="b36aa-509">定数</span><span class="sxs-lookup"><span data-stu-id="b36aa-509">Constants</span></span>

<span data-ttu-id="b36aa-510">A*定数*型のメンバーである定数値です。</span><span class="sxs-lookup"><span data-stu-id="b36aa-510">A *constant* is a constant value that is a member of a type.</span></span>

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

<span data-ttu-id="b36aa-511">定数は、暗黙的に共有されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-511">Constants are implicitly shared.</span></span> <span data-ttu-id="b36aa-512">宣言が含まれている場合、`As`句では、句は、宣言によって導入されるメンバーの種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-512">If the declaration contains an `As` clause, the clause specifies the type of the member introduced by the declaration.</span></span> <span data-ttu-id="b36aa-513">型を省略した場合、定数の型が推論されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-513">If the type is omitted then the type of the constant is inferred.</span></span> <span data-ttu-id="b36aa-514">定数の型がプリミティブ型のみありますまたは`Object`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-514">The type of a constant may only be a primitive type or `Object`.</span></span> <span data-ttu-id="b36aa-515">として型指定された定数場合`Object`型文字がないとは、定数の実際の型の定数式の型になります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-515">If a constant is typed as `Object` and there is no type character, the real type of the constant will be the type of the constant expression.</span></span> <span data-ttu-id="b36aa-516">それ以外の場合、定数の型は、定数の型の文字の種類です。</span><span class="sxs-lookup"><span data-stu-id="b36aa-516">Otherwise, the type of the constant is the type of the constant's type character.</span></span>

<span data-ttu-id="b36aa-517">次の例は、という名前のクラスを示しています。`Constants`を持つ 2 つのパブリック定数。</span><span class="sxs-lookup"><span data-stu-id="b36aa-517">The following example shows a class named `Constants` that has two public constants:</span></span>

```vb
Class Constants
    Public Const A As Integer = 1
    Public Const B As Integer = A + 1
End Class
```

<span data-ttu-id="b36aa-518">定数の値を出力する次の例のように、クラスを介してアクセスできる`Constants.A`と`Constants.B`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-518">Constants can be accessed through the class, as in the following example, which prints out the values of `Constants.A` and `Constants.B`.</span></span>

```vb
Module Test
    Sub Main()
        Console.WriteLine(Constants.A & ", " & Constants.B)
    End Sub 
End Module
```

<span data-ttu-id="b36aa-519">複数の定数を宣言する定数宣言では、単一の定数の複数の宣言と同じです。</span><span class="sxs-lookup"><span data-stu-id="b36aa-519">A constant declaration that declares multiple constants is equivalent to multiple declarations of single constants.</span></span> <span data-ttu-id="b36aa-520">次の例では、3 つの定数の 1 つの宣言ステートメントで宣言します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-520">The following example declares three constants in one declaration statement.</span></span>

```vb
Class A
    Protected Const x As Integer = 1, y As Long = 2, z As Short = 3
End Class
```

<span data-ttu-id="b36aa-521">この宣言は、次のと同等です。</span><span class="sxs-lookup"><span data-stu-id="b36aa-521">This declaration is equivalent to the following:</span></span>

```vb
Class A
    Protected Const x As Integer = 1
    Protected Const y As Long = 2
    Protected Const z As Short = 3
End Class
```

<span data-ttu-id="b36aa-522">定数の型のアクセシビリティ ドメインは、定数自体のアクセシビリティ ドメインのスーパー セットであるかと同じである必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-522">The accessibility domain of the type of the constant must be the same as or a superset of the accessibility domain of the constant itself.</span></span> <span data-ttu-id="b36aa-523">定数式では、定数の型に暗黙的に変換できる型、または定数の型の値を生成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-523">The constant expression must yield a value of the constant's type or of a type that is implicitly convertible to the constant's type.</span></span> <span data-ttu-id="b36aa-524">定数式に循環; できない可能性があります。つまり、定数はそれ自体の観点から定義できません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-524">The constant expression may not be circular; that is, a constant may not be defined in terms of itself.</span></span>

<span data-ttu-id="b36aa-525">コンパイラは、自動的に適切な順序で定数の宣言を評価します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-525">The compiler automatically evaluates the constant declarations in the appropriate order.</span></span> <span data-ttu-id="b36aa-526">コンパイラの最初の評価では、次の例では、 `Y`、し`Z`、最後に`X`、それぞれ 10、11、および 12 ですが、値を生成します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-526">In the following example, the compiler first evaluates `Y`, then `Z`, and finally `X`, producing the values 10, 11, and 12, respectively.</span></span>

```vb
Class A
    Public Const X As Integer = B.Z + 1
    Public Const Y As Integer = 10
End Class

Class B
    Public Const Z As Integer = A.Y + 1
End Class
```

<span data-ttu-id="b36aa-527">定数値のシンボル名が必要な場合は値の型が定数の宣言またはときに、値で計算できない場合コンパイル時に定数式で許可されていません、読み取り専用変数を代わりに使用可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-527">When a symbolic name for a constant value is desired, but the type of the value is not permitted in a constant declaration or when the value cannot be computed at compile time by a constant expression, a read-only variable may be used instead.</span></span>


## <a name="instance-and-shared-variables"></a><span data-ttu-id="b36aa-528">インスタンスおよび共有変数</span><span class="sxs-lookup"><span data-stu-id="b36aa-528">Instance and Shared Variables</span></span>

<span data-ttu-id="b36aa-529">インスタンスまたは共有変数は、情報を格納できる型のメンバーです。</span><span class="sxs-lookup"><span data-stu-id="b36aa-529">An instance or shared variable is a member of a type that can store information.</span></span>

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

<span data-ttu-id="b36aa-530">`Dim`修飾子がある必要があります指定なしを指定するがそれ以外の場合は省略できます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-530">The `Dim` modifier must be specified if no modifiers are specified, but may be omitted otherwise.</span></span> <span data-ttu-id="b36aa-531">1 つの変数宣言は、複数の変数宣言子を含めることができます。各変数の宣言子は、新しいインスタンスまたは共有メンバーについて説明します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-531">A single variable declaration may include multiple variable declarators; each variable declarator introduces a new instance or shared member.</span></span>

<span data-ttu-id="b36aa-532">初期化子が指定されている場合のみに 1 つのインスタンスまたは共有変数は、変数の宣言子で宣言できます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-532">If an initializer is specified, only one instance or shared variable may be declared by the variable declarator:</span></span>

```vb
Class Test
    Dim a, b, c, d As Integer = 10  ' Invalid: multiple initialization
End Class
```

<span data-ttu-id="b36aa-533">この制限は、オブジェクト初期化子には適用されません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-533">This restriction does not apply to object initializers:</span></span>

```vb
Class Test
    Dim a, b, c, d As New Collection() ' OK
End Class
```

<span data-ttu-id="b36aa-534">宣言された変数、`Shared`修飾子は、*共有変数*します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-534">A variable declared with the `Shared` modifier is a *shared variable*.</span></span> <span data-ttu-id="b36aa-535">共有変数は、作成される型のインスタンスの数に関係なく正確に 1 つのストレージの場所を識別します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-535">A shared variable identifies exactly one storage location regardless of the number of instances of the type that are created.</span></span> <span data-ttu-id="b36aa-536">共有変数プログラムが実行を開始されには、プログラムの終了時に存在しなくなります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-536">A shared variable comes into existence when a program begins executing, and ceases to exist when the program terminates.</span></span>

<span data-ttu-id="b36aa-537">共有変数は、特定のクローズ ジェネリック型のインスタンス間でのみ共有されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-537">A shared variable is shared only among instances of a particular closed generic type.</span></span> <span data-ttu-id="b36aa-538">たとえば、プログラムします。</span><span class="sxs-lookup"><span data-stu-id="b36aa-538">For example, the program:</span></span>

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

<span data-ttu-id="b36aa-539">印刷します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-539">Prints out:</span></span>

```
1
1
2
```

<span data-ttu-id="b36aa-540">なしで宣言された変数、`Shared`修飾子と呼ばれる、*インスタンス変数*します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-540">A variable declared without the `Shared` modifier is called an *instance variable*.</span></span> <span data-ttu-id="b36aa-541">クラスのすべてのインスタンスには、クラスのすべてのインスタンス変数の個別のコピーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="b36aa-541">Every instance of a class contains a separate copy of all instance variables of the class.</span></span> <span data-ttu-id="b36aa-542">参照型のインスタンス変数は、型が作成され、そのインスタンスへの参照がない場合に存在しなくなるときの新しいインスタンスが存在することには、`Finalize`メソッドが実行します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-542">An instance variable of a reference type comes into existence when a new instance of that type is created, and ceases to exist when there are no references to that instance and the `Finalize` method has executed.</span></span> <span data-ttu-id="b36aa-543">値型のインスタンス変数が、正確に同じ有効期間が所属する変数とします。</span><span class="sxs-lookup"><span data-stu-id="b36aa-543">An instance variable of a value type has exactly the same lifetime as the variable to which it belongs.</span></span> <span data-ttu-id="b36aa-544">つまり、値型の変数が存在することになるか存在しなくなりますときも、値型のインスタンス変数。</span><span class="sxs-lookup"><span data-stu-id="b36aa-544">In other words, when a variable of a value type comes into existence or ceases to exist, so does the instance variable of the value type.</span></span>

<span data-ttu-id="b36aa-545">宣言子が含まれている場合、`As`句では、句は、宣言によって導入されるメンバーの種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-545">If the declarator contains an `As` clause, the clause specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="b36aa-546">型を省略すると、厳密な型が使用される、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-546">If the type is omitted and strict semantics are being used, a compile-time error occurs.</span></span> <span data-ttu-id="b36aa-547">それ以外の場合、メンバーの型は、暗黙的に`Object`またはメンバーの種類の文字の種類。</span><span class="sxs-lookup"><span data-stu-id="b36aa-547">Otherwise the type of the members is implicitly `Object` or the type of the members' type character.</span></span>

<span data-ttu-id="b36aa-548">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="b36aa-548">__Note.__</span></span> <span data-ttu-id="b36aa-549">構文のあいまいさはありません。 宣言子は、型を省略、場合常に次の宣言子の型が使用されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-549">There is no ambiguity in the syntax: if a declarator omits a type, it will always use the type of a following declarator.</span></span>

<span data-ttu-id="b36aa-550">インスタンスまたは共有変数の型または配列要素の型のアクセシビリティ ドメインは、インスタンスまたは共有変数自体のアクセシビリティ ドメインのスーパー セットであるかと同じである必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-550">The accessibility domain of an instance or shared variable's type or array element type must be the same as or a superset of the accessibility domain of the instance or shared variable itself.</span></span>

<span data-ttu-id="b36aa-551">次の例は、`Color`という名前の内部インスタンス変数を持つクラスである`redPart`、 `greenPart`、および`bluePart`:</span><span class="sxs-lookup"><span data-stu-id="b36aa-551">The following example shows a `Color` class that has internal instance variables named `redPart`, `greenPart`, and `bluePart`:</span></span>

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


### <a name="read-only-variables"></a><span data-ttu-id="b36aa-552">読み取り専用変数</span><span class="sxs-lookup"><span data-stu-id="b36aa-552">Read-Only Variables</span></span>

<span data-ttu-id="b36aa-553">インスタンスまたは共有変数の宣言が含まれる場合、`ReadOnly`修飾子、宣言によって導入された変数への代入のみ発生する可能性または同じクラスのコンス トラクターの宣言の一部として。</span><span class="sxs-lookup"><span data-stu-id="b36aa-553">When an instance or shared variable declaration includes a `ReadOnly` modifier, assignments to the variables introduced by the declaration may only occur as part of the declaration or in a constructor in the same class.</span></span> <span data-ttu-id="b36aa-554">具体的には、読み取り専用インスタンスまたは共有変数への代入は、次の状況でのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-554">Specifically, assignments to a read-only instance or shared variable are permitted only in the following situations:</span></span>

* <span data-ttu-id="b36aa-555">(宣言では、変数の初期化子を含む) でインスタンスまたは共有変数を導入する、変数宣言します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-555">In the variable declaration that introduces the instance or shared variable (by including a variable initializer in the declaration).</span></span>

* <span data-ttu-id="b36aa-556">インスタンス変数、変数の宣言を含むクラスのインスタンス コンス トラクター内。</span><span class="sxs-lookup"><span data-stu-id="b36aa-556">For an instance variable, in the instance constructors of the class that contains the variable declaration.</span></span> <span data-ttu-id="b36aa-557">インスタンス変数は、修飾されていない方法で、またはを介してのみアクセスできます`Me`または`MyClass`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-557">The instance variable can only be accessed in an unqualified manner or through `Me` or `MyClass`.</span></span>

* <span data-ttu-id="b36aa-558">共有変数の場合は、共有変数宣言を含むクラスの共有のコンス トラクター。</span><span class="sxs-lookup"><span data-stu-id="b36aa-558">For a shared variable, in the shared constructor of the class that contains the shared variable declaration.</span></span>

<span data-ttu-id="b36aa-559">共有の読み取り専用変数は便利な定数値のシンボル名が必要な場合が値の型が定数の宣言で許可されていないときや定数式がコンパイル時に値を計算することはできません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-559">A shared read-only variable is useful when a symbolic name for a constant value is desired, but when the type of the value is not permitted in a constant declaration, or when the value cannot be computed at compile time by a constant expression.</span></span>

<span data-ttu-id="b36aa-560">最初の例、共有変数が宣言されている色でこのようなアプリケーション次`ReadOnly`を他のプログラムで変更されるを防ぐために。</span><span class="sxs-lookup"><span data-stu-id="b36aa-560">An example of the first such application follows, in which color shared variables are declared `ReadOnly` to prevent them from being changed by other programs:</span></span>

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

<span data-ttu-id="b36aa-561">定数と読み取り専用共有変数は、異なるセマンティクスを持ちます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-561">Constants and read-only shared variables have different semantics.</span></span> <span data-ttu-id="b36aa-562">定数の値が、コンパイル時に取得した式は、定数を参照する場合は、式では、読み取り専用共有変数を参照する場合、共有変数の値は、実行時までが得られない。</span><span class="sxs-lookup"><span data-stu-id="b36aa-562">When an expression references a constant, the value of the constant is obtained at compile time, but when an expression references a read-only shared variable, the value of the shared variable is not obtained until run time.</span></span> <span data-ttu-id="b36aa-563">次のようなアプリケーションの 2 つのプログラムで構成されるを検討してください。</span><span class="sxs-lookup"><span data-stu-id="b36aa-563">Consider the following application, which consists of two separate programs.</span></span>

<span data-ttu-id="b36aa-564">file1.vb:</span><span class="sxs-lookup"><span data-stu-id="b36aa-564">file1.vb:</span></span>

```vb
Namespace Program1
    Public Class Utils
        Public Shared ReadOnly X As Integer = 1
    End Class
End Namespace
```

<span data-ttu-id="b36aa-565">file2.vb:</span><span class="sxs-lookup"><span data-stu-id="b36aa-565">file2.vb:</span></span>

```vb
Namespace Program2
    Module Test
        Sub Main()
            Console.WriteLine(Program1.Utils.X)
        End Sub
    End Module
End Namespace
```

<span data-ttu-id="b36aa-566">名前空間`Program1`と`Program2`とは別にコンパイルされる 2 つのプログラムを示します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-566">The namespaces `Program1` and `Program2` denote two programs that are compiled separately.</span></span> <span data-ttu-id="b36aa-567">変数`Program1.Utils.X`として宣言されている`Shared ReadOnly`、値の出力によって、`Console.WriteLine`ステートメントがコンパイル時に、不明はではなく、実行時に取得されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-567">Because variable `Program1.Utils.X` is declared as `Shared ReadOnly`, the value output by the `Console.WriteLine` statement is not known at compile time, but rather is obtained at run time.</span></span> <span data-ttu-id="b36aa-568">したがって場合の値`X`が変更されたと`Program1`が再コンパイルされる、`Console.WriteLine`ステートメントの出力は、新しい値いて`Program2`は再コンパイルされません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-568">Thus, if the value of `X` is changed and `Program1` is recompiled, the `Console.WriteLine` statement will output the new value even if `Program2` is not recompiled.</span></span> <span data-ttu-id="b36aa-569">ただし場合、`X`定数の値をされていた`X`時に得られる`Program2`コンパイルされ、は上記の変更による影響を受ける`Program1`まで`Program2`再コンパイルされました。</span><span class="sxs-lookup"><span data-stu-id="b36aa-569">However, if `X` had been a constant, the value of `X` would have been obtained at the time `Program2` was compiled, and would have remained unaffected by changes in `Program1` until `Program2` was recompiled.</span></span>

### <a name="withevents-variables"></a><span data-ttu-id="b36aa-570">WithEvents 変数</span><span class="sxs-lookup"><span data-stu-id="b36aa-570">WithEvents Variables</span></span>

<span data-ttu-id="b36aa-571">インスタンスまたは共有変数を宣言することでそのインスタンスまたは共有変数のいずれかで発生するとイベントを発生させるイベントのセットを処理する型を宣言できます、`WithEvents`修飾子。</span><span class="sxs-lookup"><span data-stu-id="b36aa-571">A type can declare that it handles some set of events raised by one of its instance or shared variables by declaring the instance or shared variable that raises the events with the `WithEvents` modifier.</span></span> <span data-ttu-id="b36aa-572">例えば:</span><span class="sxs-lookup"><span data-stu-id="b36aa-572">For example:</span></span>

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

<span data-ttu-id="b36aa-573">この例では、メソッドで`E1Handler`イベントを処理します`E1`型のインスタンスによって発生する`Raiser`インスタンス変数に格納されている`x`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-573">In this example, the method `E1Handler` handles the event `E1` that is raised by the instance of the type `Raiser` stored in the instance variable `x`.</span></span>

<span data-ttu-id="b36aa-574">`WithEvents`修飾子により、先頭にアンダー スコアで名前を変更し、イベント フックアップを同じ名前のプロパティと置き換える変数。</span><span class="sxs-lookup"><span data-stu-id="b36aa-574">The `WithEvents` modifier causes the variable to be renamed with a leading underscore and replaced with a property of the same name that does the event hookup.</span></span> <span data-ttu-id="b36aa-575">たとえば、変数の名前は`F`、名前に変更`_F`プロパティと、`F`が暗黙的に宣言します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-575">For example, if the variable's name is `F`, it is renamed to `_F` and a property `F` is implicitly declared.</span></span> <span data-ttu-id="b36aa-576">変数の新しい名前と別の宣言間で競合がある場合は、コンパイル時エラーが報告されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-576">If there is a collision between the variable's new name and another declaration, a compile-time error will be reported.</span></span> <span data-ttu-id="b36aa-577">変数に適用されるすべての属性が、名前が変更された変数に適用されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-577">Any attributes applied to the variable are carried over to the renamed variable.</span></span>

<span data-ttu-id="b36aa-578">によって作成される暗黙的なプロパティ、`WithEvents`フックと関連するイベント ハンドラーはこれをアンフックの宣言を行います。</span><span class="sxs-lookup"><span data-stu-id="b36aa-578">The implicit property created by a `WithEvents` declaration takes care of hooking and unhooking the relevant event handlers.</span></span> <span data-ttu-id="b36aa-579">プロパティが最初に呼び出し、変数に値が割り当てられると、 `remove` (存在する場合、既存のイベント ハンドラーはこれをアンフック) 変数の現在のインスタンスでは、イベントのメソッド。</span><span class="sxs-lookup"><span data-stu-id="b36aa-579">When a value is assigned to the variable, the property first calls the `remove` method for the event on the instance currently in the variable (unhooking the existing event handler, if any).</span></span> <span data-ttu-id="b36aa-580">次に、割り当てが行われたと、プロパティを呼び出し、 `add` (新しいイベント ハンドラーのフック) 変数の新しいインスタンスでイベントのメソッド。</span><span class="sxs-lookup"><span data-stu-id="b36aa-580">Next the assignment is made, and the property calls the `add` method for the event on the new instance in the variable (hooking up the new event handler).</span></span> <span data-ttu-id="b36aa-581">次のコードは、標準のモジュールは、上記のコードに相当`Test`:</span><span class="sxs-lookup"><span data-stu-id="b36aa-581">The following code is equivalent to the code above for the standard module `Test`:</span></span>

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

<span data-ttu-id="b36aa-582">インスタンスまたは共有変数として宣言することはできません`WithEvents`変数は、構造体として型指定された場合。</span><span class="sxs-lookup"><span data-stu-id="b36aa-582">It is not valid to declare an instance or shared variable as `WithEvents` if the variable is typed as a structure.</span></span> <span data-ttu-id="b36aa-583">さらに、`WithEvents`構造体で指定することはできませんと`WithEvents`と`ReadOnly`組み合わせることはできません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-583">In addition, `WithEvents` may not be specified in a structure, and `WithEvents` and `ReadOnly` cannot be combined.</span></span>

### <a name="variable-initializers"></a><span data-ttu-id="b36aa-584">変数の初期化子</span><span class="sxs-lookup"><span data-stu-id="b36aa-584">Variable Initializers</span></span>

<span data-ttu-id="b36aa-585">インスタンスおよび共有変数の宣言では、クラスとインスタンスの変数宣言 (は共有されていない変数の宣言) 構造体では、変数の初期化子を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-585">Instance and shared variable declarations in classes and instance variable declarations (but not shared variable declarations) in structures may include variable initializers.</span></span> <span data-ttu-id="b36aa-586">`Shared`する前に、プログラムの開始後に実行されるステートメントを代入する変数、変数初期化子の対応、`Shared`変数の最初の参照します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-586">For `Shared` variables, variable initializers correspond to assignment statements that are executed after the program begins, but before the `Shared` variable is first referenced.</span></span> <span data-ttu-id="b36aa-587">インスタンス変数、変数初期化子に対応、クラスのインスタンスが作成されるときに実行されるステートメントを代入します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-587">For instance variables, variable initializers correspond to assignment statements that are executed when an instance of the class is created.</span></span> <span data-ttu-id="b36aa-588">構造体は、パラメーターなしのコンス トラクターを変更できないためインスタンス変数初期化子を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-588">Structures cannot have instance variable initializers because their parameterless constructors cannot be modified.</span></span>

<span data-ttu-id="b36aa-589">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-589">Consider the following example:</span></span>

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

<span data-ttu-id="b36aa-590">この例では次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-590">The example produces the following output:</span></span>

```
x = 1.4142135623731, i = 100, s = Hello
```

<span data-ttu-id="b36aa-591">代入`x`クラスが読み込まれるときに発生しますへと割り当て`i`と`s`クラスの新しいインスタンスが作成されたときに発生します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-591">An assignment to `x` occurs when the class is loaded, and assignments to `i` and `s` occur when a new instance of the class is created.</span></span>

<span data-ttu-id="b36aa-592">変数の初期化子の型のコンス トラクターのブロックに自動的に挿入する代入ステートメントと考えると便利です。</span><span class="sxs-lookup"><span data-stu-id="b36aa-592">It is useful to think of variable initializers as assignment statements that are automatically inserted in the block of the type's constructor.</span></span> <span data-ttu-id="b36aa-593">次の例には、いくつかのインスタンス変数初期化子が含まれています。</span><span class="sxs-lookup"><span data-stu-id="b36aa-593">The following example contains several instance variable initializers.</span></span>

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

<span data-ttu-id="b36aa-594">例は、次のコードに対応各コメントが、自動的に挿入されたステートメントを示します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-594">The example corresponds to the code shown below, where each comment indicates an automatically inserted statement.</span></span>

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

<span data-ttu-id="b36aa-595">すべての変数は、任意の変数の初期化子が実行される前に、その型の既定値に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-595">All variables are initialized to the default value of their type before any variable initializers are executed.</span></span> <span data-ttu-id="b36aa-596">例えば:</span><span class="sxs-lookup"><span data-stu-id="b36aa-596">For example:</span></span>

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

<span data-ttu-id="b36aa-597">`b`クラスが読み込まれるときに、既定値に自動的に初期化と`i`が自動的に、クラスのインスタンスが作成されたときに、既定値に初期化、上記のコードは、次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-597">Because `b` is automatically initialized to its default value when the class is loaded and `i` is automatically initialized to its default value when an instance of the class is created, the preceding code produces the following output:</span></span>

```
b = False, i = 0
```

<span data-ttu-id="b36aa-598">各変数の初期化子は、変数の型または変数の型に暗黙的に変換できる型の値を生成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-598">Each variable initializer must yield a value of the variable's type or of a type that is implicitly convertible to the variable's type.</span></span> <span data-ttu-id="b36aa-599">変数の初期化子は、循環する可能性があります。 または参照先の変数の場合、値をその既定値を初期化子の目的では、その後初期化される変数を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b36aa-599">A variable initializer may be circular or refer to a variable that will be initialized after it, in which case the value of the referenced variable is its default value for the purposes of the initializer.</span></span> <span data-ttu-id="b36aa-600">このような初期化子では、疑わしい値です。</span><span class="sxs-lookup"><span data-stu-id="b36aa-600">Such an initializer is of dubious value.</span></span>

<span data-ttu-id="b36aa-601">変数の初期化子の 3 つの形式があります。 通常の初期化子、配列のサイズの初期化子とオブジェクト初期化子。</span><span class="sxs-lookup"><span data-stu-id="b36aa-601">There are three forms of variable initializers: regular initializers, array-size initializers, and object initializers.</span></span> <span data-ttu-id="b36aa-602">型名に続く等号の後に最初の 2 つのフォームが表示される、後の 2 つは、宣言自体の一部です。</span><span class="sxs-lookup"><span data-stu-id="b36aa-602">The first two forms appear after an equal sign that follows the type name, the latter two are part of the declaration itself.</span></span> <span data-ttu-id="b36aa-603">初期化子の 1 つだけの形式は、任意の特定の宣言で使用可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-603">Only one form of initializer may be used on any particular declaration.</span></span>

#### <a name="regular-initializers"></a><span data-ttu-id="b36aa-604">通常の初期化子</span><span class="sxs-lookup"><span data-stu-id="b36aa-604">Regular Initializers</span></span>

<span data-ttu-id="b36aa-605">A*通常の初期化子*変数の型に暗黙的に変換される式を指定します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-605">A *regular initializer* is an expression that is implicitly convertible to the type of the variable.</span></span> <span data-ttu-id="b36aa-606">型名に依存し、値として分類する必要があります、等号の後に表示されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-606">It appears after an equal sign that follows the type name and must be classified as a value.</span></span> <span data-ttu-id="b36aa-607">例えば:</span><span class="sxs-lookup"><span data-stu-id="b36aa-607">For example:</span></span>

```vb
Module Test
    Dim x As Integer = 10
    Dim y As Integer = 20

    Sub Main()
        Console.WriteLine("x = " & x & ", y = " & y)
    End Sub
End Module
```

<span data-ttu-id="b36aa-608">このプログラムでは、出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-608">This program produces the output:</span></span>

```vb
x = 10, y = 20
```

<span data-ttu-id="b36aa-609">変数の宣言に通常の初期化子がある場合、一度に 1 つの変数を宣言できます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-609">If a variable declaration has a regular initializer, then only a single variable can be declared at a time.</span></span> <span data-ttu-id="b36aa-610">例えば:</span><span class="sxs-lookup"><span data-stu-id="b36aa-610">For example:</span></span>

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

#### <a name="object-initializers"></a><span data-ttu-id="b36aa-611">オブジェクト初期化子</span><span class="sxs-lookup"><span data-stu-id="b36aa-611">Object Initializers</span></span>

<span data-ttu-id="b36aa-612">*オブジェクト初期化子*型名の代わりに、オブジェクトの作成式を使用して指定されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-612">An *object initializer* is specified using an object creation expression in the place of the type name.</span></span> <span data-ttu-id="b36aa-613">オブジェクト初期化子は、通常の初期化子オブジェクトの作成式の結果を変数に代入と同じです。</span><span class="sxs-lookup"><span data-stu-id="b36aa-613">An object initializer is equivalent to a regular initializer assigning the result of the object creation expression to the variable.</span></span> <span data-ttu-id="b36aa-614">So</span><span class="sxs-lookup"><span data-stu-id="b36aa-614">So</span></span>

```vb
Module TestModule
    Sub Main()
        Dim x As New Test(10)
    End Sub
End Module
```

<span data-ttu-id="b36aa-615">上記の式は、次の式と同じです。</span><span class="sxs-lookup"><span data-stu-id="b36aa-615">is equivalent to</span></span>

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test(10)
    End Sub
End Module
```

<span data-ttu-id="b36aa-616">オブジェクト初期化子内のかっこは、コンス トラクターの引数リストとは配列型修飾子としてことはありませんは常に解釈されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-616">The parenthesis in an object initializer is always interpreted as the argument list for the constructor and never as array type modifiers.</span></span> <span data-ttu-id="b36aa-617">オブジェクト初期化子と変数名には、配列型修飾子または null 許容型修飾子を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-617">A variable name with an object initializer cannot have an array type modifier or a nullable type modifier.</span></span>

#### <a name="array-size-initializers"></a><span data-ttu-id="b36aa-618">配列のサイズの初期化子</span><span class="sxs-lookup"><span data-stu-id="b36aa-618">Array-Size Initializers</span></span>

<span data-ttu-id="b36aa-619">*配列サイズの初期化子*は式で表されるディメンションの上限値のセットを提供する変数の名前の修飾子です。</span><span class="sxs-lookup"><span data-stu-id="b36aa-619">An *array-size initializer* is a modifier on the name of the variable that gives a set of dimension upper bounds denoted by expressions.</span></span>

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

<span data-ttu-id="b36aa-620">上限値式の値として分類する必要があり、暗黙的に変換できる必要があります`Integer`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-620">The upper bound expressions must be classified as values and must be implicitly convertible to `Integer`.</span></span> <span data-ttu-id="b36aa-621">上限の設定は、上限を指定した配列作成式の変数の初期化子と同じです。</span><span class="sxs-lookup"><span data-stu-id="b36aa-621">The set of upper bounds is equivalent to a variable initializer of an array-creation expression with the given upper bounds.</span></span> <span data-ttu-id="b36aa-622">配列型の次元数は、サイズの配列初期化子から推測されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-622">The number of dimensions of the array type is inferred from the array size initializer.</span></span> <span data-ttu-id="b36aa-623">So</span><span class="sxs-lookup"><span data-stu-id="b36aa-623">So</span></span>

```vb
Module Test
    Sub Main()
        Dim x(5, 10) As Integer
    End Sub
End Module
```

<span data-ttu-id="b36aa-624">上記の式は、次の式と同じです。</span><span class="sxs-lookup"><span data-stu-id="b36aa-624">is equivalent to</span></span>

```vb
Module Test
    Sub Main()
        Dim x As Integer(,) = New Integer(5, 10) {}
    End Sub
End Module
```

<span data-ttu-id="b36aa-625">すべての上限が-1 以上にする必要があり、すべてのディメンションは上限が指定されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-625">All upper bounds must be equal to or greater than -1, and all dimensions must have an upper bound specified.</span></span> <span data-ttu-id="b36aa-626">初期化される配列の要素型が配列型の場合、配列型修飾子は配列のサイズの初期化子の右に移動します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-626">If the element type of the array being initialized is itself an array type, the array-type modifiers go to the right of the array-size initializer.</span></span> <span data-ttu-id="b36aa-627">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-627">For example</span></span>

```vb
Module Test
    Sub Main()
        Dim x(5,10)(,,) As Integer
    End Sub
End Module
```

<span data-ttu-id="b36aa-628">ローカル変数を宣言`x`型がの 3 次元の配列の 2 次元配列`Integer`の境界を含む配列に初期化された`0..5`の最初の次元と`0..10`2 番目の次元。</span><span class="sxs-lookup"><span data-stu-id="b36aa-628">declares a local variable `x` whose type is a two-dimensional array of three-dimensional arrays of `Integer`, initialized to an array with bounds of `0..5` in the first dimension and `0..10` in the second dimension.</span></span> <span data-ttu-id="b36aa-629">変数の型は、配列の配列の要素を初期化するために配列サイズの初期化子を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-629">It is not possible to use an array size initializer to initialize the elements of a variable whose type is an array of arrays.</span></span>

<span data-ttu-id="b36aa-630">配列サイズの初期化子と変数宣言では、その型または通常の初期化子に配列の型修飾子を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-630">A variable declaration with an array-size initializer cannot have an array type modifier on its type or a regular initializer.</span></span>


### <a name="systemmarshalbyrefobject-classes"></a><span data-ttu-id="b36aa-631">System.MarshalByRefObject クラス</span><span class="sxs-lookup"><span data-stu-id="b36aa-631">System.MarshalByRefObject Classes</span></span>

<span data-ttu-id="b36aa-632">クラスから派生するクラス`System.MarshalByRefObject`はプロキシを使用してコンテキストの境界を越えてマーシャ リング (は、参照渡しで) コピーではなく (つまり、値によって)。</span><span class="sxs-lookup"><span data-stu-id="b36aa-632">Classes that derive from the class `System.MarshalByRefObject` are marshaled across context boundaries using proxies (that is, by reference) rather than through copying (that is, by value).</span></span> <span data-ttu-id="b36aa-633">つまり、このようなクラスのインスタンスは true。 インスタンスができない可能性がありますが、代わりに単なる変数をマーシャ リングするスタブにアクセスし、コンテキストの境界を越えてメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-633">This means that an instance of such a class may not be a true instance but instead may just be a stub that marshals variable accesses and method calls across a context boundary.</span></span>

<span data-ttu-id="b36aa-634">その結果、このようなクラスで定義された変数の格納場所への参照を作成することはできません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-634">As a result, it is not possible to create a reference to the storage location of variables defined on such classes.</span></span> <span data-ttu-id="b36aa-635">派生したクラスが変数に型指定されたつまり`System.MarshalByRefObject`パラメーター、およびメソッドと変数の値の型にはアクセスできない型の変数の参照を渡すことができません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-635">This means that variables typed as classes derived from `System.MarshalByRefObject` cannot be passed to reference parameters, and methods and variables of variables typed as value types may not be accessed.</span></span> <span data-ttu-id="b36aa-636">代わりに、Visual Basic では、変数の場合と同様のプロパティ (制限は、同じプロパティでは) ために、このようなクラスで定義されているが扱われます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-636">Instead, Visual Basic treats variables defined on such classes as if they were properties (since the restrictions are the same on properties).</span></span>

<span data-ttu-id="b36aa-637">このルールを 1 つの例外があります: メンバー暗黙的または明示的に修飾`Me`上記の制限から除外されてため`Me`常に、プロキシではない実際のオブジェクトにすることが保証されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-637">There is one exception to this rule: a member implicitly or explicitly qualified with `Me` is exempt from the above restrictions, because `Me` is always guaranteed to be an actual object, not a proxy.</span></span>

## <a name="properties"></a><span data-ttu-id="b36aa-638">プロパティ</span><span class="sxs-lookup"><span data-stu-id="b36aa-638">Properties</span></span>

<span data-ttu-id="b36aa-639">*プロパティ*変数の自然な拡張機能は、関連付けられた型を持つメンバーを両方とも; と変数とプロパティにアクセスするための構文は同じです。</span><span class="sxs-lookup"><span data-stu-id="b36aa-639">*Properties* are a natural extension of variables; both are named members with associated types, and the syntax for accessing variables and properties is the same.</span></span> <span data-ttu-id="b36aa-640">変数と違って、ただし、プロパティを表しません記憶域の場所。</span><span class="sxs-lookup"><span data-stu-id="b36aa-640">Unlike variables, however, properties do not denote storage locations.</span></span> <span data-ttu-id="b36aa-641">プロパティの代わりに、ある*アクセサー*、読み取り、またはその値を記述するために実行するステートメントを指定します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-641">Instead, properties have *accessors*, which specify the statements to execute in order to read or write their values.</span></span>

<span data-ttu-id="b36aa-642">プロパティは、プロパティ宣言で定義されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-642">Properties are defined with property declarations.</span></span> <span data-ttu-id="b36aa-643">プロパティ宣言の最初の部分では、フィールド宣言に似ています。</span><span class="sxs-lookup"><span data-stu-id="b36aa-643">The first part of a property declaration resembles a field declaration.</span></span> <span data-ttu-id="b36aa-644">2 番目の部分が含まれています、`Get`アクセサーおよび`Set`アクセサー。</span><span class="sxs-lookup"><span data-stu-id="b36aa-644">The second part includes a `Get` accessor and/or a `Set` accessor.</span></span>

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

<span data-ttu-id="b36aa-645">次の例で、`Button`クラスを定義、`Caption`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="b36aa-645">In the example below, the `Button` class defines a `Caption` property.</span></span>

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

<span data-ttu-id="b36aa-646">に基づいて、`Button`上記のクラス、例を次の使用、`Caption`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="b36aa-646">Based on the `Button` class above, the following is an example of use of the `Caption` property:</span></span>

```vb
Dim okButton As Button = New Button()

okButton.Caption = "OK" ' Invokes Set accessor.
Dim s As String = okButton.Caption ' Invokes Get accessor.
```

<span data-ttu-id="b36aa-647">ここでは、`Set`アクセサーがプロパティの値を割り当てることによって呼び出されると、`Get`アクセサーは、式でプロパティを参照することによって呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-647">Here, the `Set` accessor is invoked by assigning a value to the property, and the `Get` accessor is invoked by referencing the property in an expression.</span></span>

<span data-ttu-id="b36aa-648">プロパティの型を指定しないと、厳密な型が使用されている、コンパイル時エラーが発生します。それ以外の場合、プロパティの型は、暗黙的に`Object`またはプロパティの型の文字の種類。</span><span class="sxs-lookup"><span data-stu-id="b36aa-648">If no type is specified for a property and strict semantics are being used, a compile-time error occurs; otherwise the type of the property is implicitly `Object` or the type of the property's type character.</span></span> <span data-ttu-id="b36aa-649">プロパティ宣言は、いずれかを含めることができます、 `Get` 、プロパティの値を取得するには、アクセサーを`Set`アクセサーで、プロパティ、またはその両方の値を格納します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-649">A property declaration may contain either a `Get` accessor, which retrieves the value of the property, a `Set` accessor, which stores the value of the property, or both.</span></span> <span data-ttu-id="b36aa-650">プロパティは、メソッドを暗黙的に宣言するため、メソッドとして同じ修飾子を持つプロパティを宣言することがあります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-650">Because a property implicitly declares methods, a property may be declared with the same modifiers as a method.</span></span> <span data-ttu-id="b36aa-651">プロパティがインターフェイスで定義されているかで定義されているかどうか、`MustOverride`修飾子、プロパティ本体、`End`構文を省略する必要があります。 それ以外の場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-651">If the property is defined in an interface or defined with the `MustOverride` modifier, the property body and the `End` construct must be omitted; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="b36aa-652">インデックスのパラメーター リストは、インデックス パラメーターのプロパティの型ではなく、プロパティがオーバー ロードするため、プロパティの署名構成します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-652">The index parameter list makes up the signature of the property, so properties may be overloaded on index parameters but not on the type of the property.</span></span> <span data-ttu-id="b36aa-653">インデックスのパラメーター リストは、通常のメソッドと同じです。</span><span class="sxs-lookup"><span data-stu-id="b36aa-653">The index parameter list is the same as for a regular method.</span></span> <span data-ttu-id="b36aa-654">ただし、パラメーターがない修正が可能で、`ByRef`修飾子とそれらのいずれが付けられて`Value`(で暗黙の値パラメーター用に予約されている、`Set`アクセサー)。</span><span class="sxs-lookup"><span data-stu-id="b36aa-654">However, none of the parameters may be modified with the `ByRef` modifier and none of them may be named `Value` (which is reserved for the implicit value parameter in the `Set` accessor).</span></span>

<span data-ttu-id="b36aa-655">プロパティは、次のように宣言できます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-655">A property may be declared as follows:</span></span>

* <span data-ttu-id="b36aa-656">プロパティが両方が必要プロパティがプロパティ型修飾子を指定しない場合、`Get`アクセサーと`Set`アクセサー。</span><span class="sxs-lookup"><span data-stu-id="b36aa-656">If the property specifies no property type modifier, the property must have both a `Get` accessor and a `Set` accessor.</span></span> <span data-ttu-id="b36aa-657">プロパティを読み取り/書き込みプロパティであるといいます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-657">The property is said to be a read-write property.</span></span>

* <span data-ttu-id="b36aa-658">プロパティが指定されている場合、`ReadOnly`修飾子は、プロパティには、`Get`アクセサーがないと、`Set`アクセサー。</span><span class="sxs-lookup"><span data-stu-id="b36aa-658">If the property specifies the `ReadOnly` modifier, the property must have a `Get` accessor and may not have a `Set` accessor.</span></span> <span data-ttu-id="b36aa-659">読み取り専用プロパティ、プロパティと言います。</span><span class="sxs-lookup"><span data-stu-id="b36aa-659">The property is said to be read-only property.</span></span> <span data-ttu-id="b36aa-660">割り当ての対象となる読み取り専用プロパティのコンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-660">It is a compile-time error for a read-only property to be the target of an assignment.</span></span>

* <span data-ttu-id="b36aa-661">プロパティが指定されている場合、`WriteOnly`修飾子は、プロパティには、`Set`アクセサーがないと、`Get`アクセサー。</span><span class="sxs-lookup"><span data-stu-id="b36aa-661">If the property specifies the `WriteOnly` modifier, the property must have a `Set` accessor and may not have a `Get` accessor.</span></span> <span data-ttu-id="b36aa-662">プロパティを書き込み専用プロパティであるといいます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-662">The property is said to be write-only property.</span></span> <span data-ttu-id="b36aa-663">割り当ての対象として、またはメソッドに引数として場合を除き、式での書き込み専用プロパティを参照すると、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-663">It is a compile-time error to reference a write-only property in an expression except as the target of an assignment or as an argument to a method.</span></span>

<span data-ttu-id="b36aa-664">`Get`と`Set`プロパティのアクセサーが個別のメンバーでないし、プロパティのアクセサーを個別に宣言することはできません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-664">The `Get` and `Set` accessors of a property are not distinct members, and it is not possible to declare the accessors of a property separately.</span></span> <span data-ttu-id="b36aa-665">次の例では、1 つの読み取り/書き込みプロパティを宣言していません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-665">The following example does not declare a single read-write property.</span></span> <span data-ttu-id="b36aa-666">読み取り専用のいずれか、同じ名前の 2 つのプロパティを宣言ではなく、書き込み専用の 1 つ。</span><span class="sxs-lookup"><span data-stu-id="b36aa-666">Rather, it declares two properties with the same name, one read-only and one write-only:</span></span>

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

<span data-ttu-id="b36aa-667">同じクラスで宣言されている 2 つのメンバーは、同じ名前を持つことはできません、ため例では、によってコンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-667">Since two members declared in the same class cannot have the same name, the example causes a compile-time error.</span></span>

<span data-ttu-id="b36aa-668">既定のプロパティのアクセシビリティ`Get`と`Set`アクセサーは、プロパティ自体のアクセシビリティと同じです。</span><span class="sxs-lookup"><span data-stu-id="b36aa-668">By default, the accessibility of a property's `Get` and `Set` accessors is the same as the accessibility of the property itself.</span></span> <span data-ttu-id="b36aa-669">ただし、`Get`と`Set`アクセサーがプロパティから個別にアクセシビリティを指定できますも。</span><span class="sxs-lookup"><span data-stu-id="b36aa-669">However, the `Get` and `Set` accessors can also specify accessibility separately from the property.</span></span> <span data-ttu-id="b36aa-670">その場合は、アクセサーのアクセシビリティはプロパティのアクセシビリティよりもより制限の厳しいである必要があり、1 つだけのアクセサーがプロパティから別のアクセシビリティ レベルを持つことができます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-670">In that case, the accessibility of an accessor must be more restrictive than the accessibility of the property and only one accessor can have a different accessibility level from the property.</span></span> <span data-ttu-id="b36aa-671">アクセスの種類は、次のように制限を調整に考えられます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-671">Access types are considered more or less restrictive as follows:</span></span>

* <span data-ttu-id="b36aa-672">`Private` 限定的な`Public`、 `Protected Friend`、 `Protected`、または`Friend`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-672">`Private` is more restrictive than `Public`, `Protected Friend`, `Protected`, or `Friend`.</span></span>

* <span data-ttu-id="b36aa-673">`Friend` 限定的な`Protected Friend`または`Public`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-673">`Friend` is more restrictive than `Protected Friend` or `Public`.</span></span>

* <span data-ttu-id="b36aa-674">`Protected` 限定的な`Protected Friend`または`Public`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-674">`Protected` is more restrictive than `Protected Friend` or `Public`.</span></span>

* <span data-ttu-id="b36aa-675">`Protected Friend` 限定的な`Public`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-675">`Protected Friend` is more restrictive than `Public`.</span></span>

<span data-ttu-id="b36aa-676">プロパティのアクセサーのいずれかがアクセスできるが、もう 1 つが、読み取り専用または書き込み専用の場合と、プロパティが扱われます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-676">When one of a property's accessors is accessible but the other one is not, the property is treated as if it was read-only or write-only.</span></span> <span data-ttu-id="b36aa-677">例えば:</span><span class="sxs-lookup"><span data-stu-id="b36aa-677">For example:</span></span>

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

<span data-ttu-id="b36aa-678">派生型では、プロパティをシャドウする場合に派生プロパティの読み取りと書き込みの両方に対してシャドウされたプロパティを非表示にします。</span><span class="sxs-lookup"><span data-stu-id="b36aa-678">When a derived type shadows a property, the derived property hides the shadowed property with respect to both reading and writing.</span></span> <span data-ttu-id="b36aa-679">次の例では、`P`プロパティ`B`を非表示に、`P`プロパティ`A`の読み取りと書き込みの両方に関して。</span><span class="sxs-lookup"><span data-stu-id="b36aa-679">In the following example, the `P` property in `B` hides the `P` property in `A` with respect to both reading and writing:</span></span>

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

<span data-ttu-id="b36aa-680">戻り値の型やパラメーターの型のアクセシビリティ ドメインは、プロパティ自体のアクセシビリティ ドメインのスーパー セットであるかと同じである必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-680">The accessibility domain of the return type or parameter types must be the same as or a superset of the accessibility domain of the property itself.</span></span> <span data-ttu-id="b36aa-681">プロパティが 1 つを持つだけ`Set`アクセサーと 1 つ`Get`アクセサー。</span><span class="sxs-lookup"><span data-stu-id="b36aa-681">A property may only have one `Set` accessor and one `Get` accessor.</span></span>

<span data-ttu-id="b36aa-682">宣言と呼び出しの構文での相違点を除いて`Overridable`、 `NotOverridable`、 `Overrides`、 `MustOverride`、および`MustInherit`プロパティの動作と同様に`Overridable`、 `NotOverridable`、 `Overrides`、 `MustOverride`、および`MustInherit`メソッド。</span><span class="sxs-lookup"><span data-stu-id="b36aa-682">Except for differences in declaration and invocation syntax, `Overridable`, `NotOverridable`, `Overrides`, `MustOverride`, and `MustInherit` properties behave exactly like `Overridable`, `NotOverridable`, `Overrides`, `MustOverride`, and `MustInherit` methods.</span></span> <span data-ttu-id="b36aa-683">プロパティがオーバーライドされると、同じ種類 (読み取り/書き込み、書き込み専用、読み取り専用) のオーバーライドするプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-683">When a property is overridden, the overriding property must be of the same type (read-write, read-only, write-only).</span></span> <span data-ttu-id="b36aa-684">`Overridable`プロパティを含めることはできません、`Private`アクセサー。</span><span class="sxs-lookup"><span data-stu-id="b36aa-684">An `Overridable` property cannot contain a `Private` accessor.</span></span>

<span data-ttu-id="b36aa-685">次の例で`X`は、`Overridable`読み取り専用のプロパティ`Y`は、`Overridable`読み取り/書き込みプロパティ、および`Z`は、`MustOverride`読み取り/書き込みプロパティ。</span><span class="sxs-lookup"><span data-stu-id="b36aa-685">In the following example `X` is an `Overridable` read-only property, `Y` is an `Overridable` read-write property, and `Z` is a `MustOverride` read-write property.</span></span>

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

<span data-ttu-id="b36aa-686">`Z`は`MustOverride`、クラスを含む`A`として宣言されなければなりません`MustInherit`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-686">Because `Z` is `MustOverride`, the containing class `A` must be declared `MustInherit`.</span></span>

<span data-ttu-id="b36aa-687">これに対し、クラスから派生したクラス`A`を次に示します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-687">By contrast, a class that derives from class `A` is shown below:</span></span>

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

<span data-ttu-id="b36aa-688">ここでは、プロパティの宣言`X`、`Y`、および`Z`基本プロパティを上書きします。</span><span class="sxs-lookup"><span data-stu-id="b36aa-688">Here, the declarations of properties `X`,`Y`, and `Z` override the base properties.</span></span> <span data-ttu-id="b36aa-689">各プロパティの宣言は、アクセシビリティ修飾子、型、および対応する継承されたプロパティの名前を正確に一致します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-689">Each property declaration exactly matches the accessibility modifiers, type, and name of the corresponding inherited property.</span></span> <span data-ttu-id="b36aa-690">`Get`プロパティのアクセサー`X`と`Set`プロパティのアクセサー`Y`を使用して、`MyBase`継承されたプロパティにアクセスするキーワード。</span><span class="sxs-lookup"><span data-stu-id="b36aa-690">The `Get` accessor of property `X` and the `Set` accessor of property `Y` use the `MyBase` keyword to access the inherited properties.</span></span> <span data-ttu-id="b36aa-691">プロパティの宣言`Z`よりも優先されます、`MustOverride`プロパティ - そのため、いいえ未処理ありますは`MustOverride`クラスのメンバー`B`と`B`正規クラスにすることが許可されています。</span><span class="sxs-lookup"><span data-stu-id="b36aa-691">The declaration of property `Z` overrides the `MustOverride` property -- thus, there are no outstanding `MustOverride` members in class `B`, and `B` is permitted to be a regular class.</span></span>

<span data-ttu-id="b36aa-692">最初に参照される時点まで、リソースの初期化を遅延プロパティを使用できます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-692">Properties can be used to delay initialization of a resource until the moment it is first referenced.</span></span> <span data-ttu-id="b36aa-693">例えば:</span><span class="sxs-lookup"><span data-stu-id="b36aa-693">For example:</span></span>

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

<span data-ttu-id="b36aa-694">`ConsoleStreams`クラスには、3 つのプロパティが含まれています。 `In`、 `Out`、および`Error`、標準入力、出力、およびデバイスのエラーをそれぞれ表すです。</span><span class="sxs-lookup"><span data-stu-id="b36aa-694">The `ConsoleStreams` class contains three properties, `In`, `Out`, and `Error`, that represent the standard input, output, and error devices, respectively.</span></span> <span data-ttu-id="b36aa-695">これらのメンバーのプロパティとして公開することで、`ConsoleStreams`クラスは実際に使用されるまで初期化を遅らせることができます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-695">By exposing these members as properties, the `ConsoleStreams` class can delay their initialization until they are actually used.</span></span> <span data-ttu-id="b36aa-696">最初に参照する時に、たとえば、`Out`に示すように、プロパティ`ConsoleStreams.Out.WriteLine("hello, world")`、基になる`TextWriter`出力デバイスが初期化されるのです。</span><span class="sxs-lookup"><span data-stu-id="b36aa-696">For example, upon first referencing the `Out` property, as in `ConsoleStreams.Out.WriteLine("hello, world")`, the underlying `TextWriter` for the output device is initialized.</span></span> <span data-ttu-id="b36aa-697">アプリケーションへの参照がない場合は、`In`と`Error`それらのデバイス プロパティ、そのオブジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-697">But if the application makes no reference to the `In` and `Error` properties, then no objects are created for those devices.</span></span>


### <a name="get-accessor-declarations"></a><span data-ttu-id="b36aa-698">アクセサーの宣言を取得します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-698">Get Accessor Declarations</span></span>

<span data-ttu-id="b36aa-699">A`Get`アクセサー (ゲッター) が、プロパティを使用して宣言されている`Get`宣言します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-699">A `Get` accessor (getter) is declared by using a property `Get` declaration.</span></span> <span data-ttu-id="b36aa-700">プロパティ`Get`宣言キーワードから成る`Get`にステートメント ブロックを続けます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-700">A property `Get` declaration consists of the keyword `Get` followed by a statement block.</span></span> <span data-ttu-id="b36aa-701">という名前のプロパティを指定された`P`、`Get`アクセサー宣言は、名前を持つメソッドを暗黙的に宣言`get_P`同じ修飾子、型、およびパラメーター リストのプロパティとして使用します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-701">Given a property named `P`, a `Get` accessor declaration implicitly declares a method with the name `get_P` with the same modifiers, type, and parameter list as the property.</span></span> <span data-ttu-id="b36aa-702">型に同じ名前の宣言が含まれている場合、コンパイル時エラーが発生しますが、暗黙の宣言は名前のバインドのため無視されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-702">If the type contains a declaration with that name, a compile-time error results, but the implicit declaration is ignored for the purposes of name binding.</span></span>

<span data-ttu-id="b36aa-703">暗黙的に宣言されている特別なローカル変数、`Get`プロパティと同じ名前でアクセサー本体の宣言領域は、プロパティの戻り値を表します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-703">A special local variable, which is implicitly declared in the `Get` accessor body's declaration space with the same name as the property, represents the return value of the property.</span></span> <span data-ttu-id="b36aa-704">ローカルの変数は、式で使用する場合の特別な名前解決のセマンティクスを持っています。</span><span class="sxs-lookup"><span data-stu-id="b36aa-704">The local variable has special name resolution semantics when used in expressions.</span></span> <span data-ttu-id="b36aa-705">Invocation 式など、メソッド グループとして分類される式が必要とするコンテキストでローカル変数が使用されるローカル変数ではなく、関数に名前が解決します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-705">If the local variable is used in a context that expects an expression that is classified as a method group, such as an invocation expression, then the name resolves to the function rather than to the local variable.</span></span> <span data-ttu-id="b36aa-706">例えば:</span><span class="sxs-lookup"><span data-stu-id="b36aa-706">For example:</span></span>

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

<span data-ttu-id="b36aa-707">かっこを使用してあいまいな状況が発生することができます (など`F(1)`場所`F`は 1 次元配列の型を持つプロパティです)。</span><span class="sxs-lookup"><span data-stu-id="b36aa-707">The use of parentheses can cause ambiguous situations (such as `F(1)` where `F` is a property whose type is a one-dimensional array).</span></span> <span data-ttu-id="b36aa-708">すべてのあいまいな状況では、名前は、ローカル変数ではなく、プロパティに解決されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-708">In all ambiguous situations, the name resolves to the property rather than the local variable.</span></span> <span data-ttu-id="b36aa-709">例えば:</span><span class="sxs-lookup"><span data-stu-id="b36aa-709">For example:</span></span>

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

<span data-ttu-id="b36aa-710">コントロール フロー リーフ、`Get`アクセサーの本体のローカル変数の値は呼び出しの式に渡されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-710">When control flow leaves the `Get` accessor body, the value of the local variable is passed back to the invocation expression.</span></span> <span data-ttu-id="b36aa-711">呼び出しているため、`Get`アクセサーは概念的には、変数の値を読み取るのと同じですが、プログラミング スタイルは不適切と見なされます`Get`アクセサーが副作用を持つ次の例に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="b36aa-711">Because invoking a `Get` accessor is conceptually equivalent to reading the value of a variable, it is considered bad programming style for `Get` accessors to have observable side effects, as illustrated in the following example:</span></span>

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

<span data-ttu-id="b36aa-712">値、`NextValue`プロパティは、プロパティにアクセスした回数によって異なります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-712">The value of the `NextValue` property depends on the number of times the property has previously been accessed.</span></span> <span data-ttu-id="b36aa-713">そのため、監視可能な副作用を生成するプロパティにアクセスして、プロパティは、メソッドとして代わりに実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-713">Thus, accessing the property produces an observable side effect, and the property should instead be implemented as a method.</span></span>

<span data-ttu-id="b36aa-714">「副作用なし」の規則`Get`アクセサーがあることを意味`Get`を単に変数に格納されている値を返すアクセサーを記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-714">The "no side effects" convention for `Get` accessors does not mean that `Get` accessors should always be written to simply return values stored in variables.</span></span> <span data-ttu-id="b36aa-715">実際、`Get`アクセサーは多くの場合、複数の変数へのアクセスやメソッドの呼び出しによってプロパティの値を計算します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-715">Indeed, `Get` accessors often compute the value of a property by accessing multiple variables or invoking methods.</span></span> <span data-ttu-id="b36aa-716">ただし、適切にデザインされた`Get`アクセサーは、オブジェクトの状態の監視可能な変更を引き起こす操作を行いません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-716">However, a properly designed `Get` accessor performs no actions that cause observable changes in the state of the object.</span></span>

<span data-ttu-id="b36aa-717">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="b36aa-717">__Note.__</span></span> <span data-ttu-id="b36aa-718">`Get` アクセサーには、サブルーチンが行の配置で同じ制限があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-718">`Get` accessors have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="b36aa-719">最初のステートメント、end ステートメント、およびブロックする必要がありますすべて表示されますは論理行の先頭。</span><span class="sxs-lookup"><span data-stu-id="b36aa-719">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>

```antlr
PropertyGetDeclaration
    : Attributes? AccessModifier? 'Get' LineTerminator
      Block?
      'End' 'Get' StatementTerminator
    ;
```

### <a name="set-accessor-declarations"></a><span data-ttu-id="b36aa-720">Set アクセサーの宣言</span><span class="sxs-lookup"><span data-stu-id="b36aa-720">Set Accessor Declarations</span></span>

<span data-ttu-id="b36aa-721">A`Set`アクセサー (セッター) は、プロパティの設定宣言を使用して宣言されています。</span><span class="sxs-lookup"><span data-stu-id="b36aa-721">A `Set` accessor (setter) is declared by using a property set declaration.</span></span> <span data-ttu-id="b36aa-722">プロパティの設定宣言キーワードから成る`Set`、オプションのパラメーター リストと、ステートメント ブロックです。</span><span class="sxs-lookup"><span data-stu-id="b36aa-722">A property set declaration consists of the keyword `Set`, an optional parameter list, and a statement block.</span></span> <span data-ttu-id="b36aa-723">という名前のプロパティを指定された`P`、set アクセス操作子の宣言が暗黙的に名前を持つメソッドを宣言します`set_P`同じ修飾子およびプロパティとパラメーターの一覧を使用します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-723">Given a property named `P`, a setter declaration implicitly declares a method with the name `set_P` with the same modifiers and parameter list as the property.</span></span> <span data-ttu-id="b36aa-724">型に同じ名前の宣言が含まれている場合、コンパイル時エラーが発生しますが、暗黙の宣言は名前のバインドのため無視されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-724">If the type contains a declaration with that name, a compile-time error results, but the implicit declaration is ignored for the purposes of name binding.</span></span>

<span data-ttu-id="b36aa-725">パラメーター リストが指定されている場合がありますいずれかのメンバー、そのメンバーが修飾子を除く`ByVal`、その型は、プロパティの型と同じである必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-725">If a parameter list is specified, it must have one member, that member must have no modifiers except `ByVal`, and its type must be the same as the type of the property.</span></span> <span data-ttu-id="b36aa-726">パラメーターが設定されているプロパティ値を表します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-726">The parameter represents the property value being set.</span></span> <span data-ttu-id="b36aa-727">パラメーターが名前付きパラメーターを省略すると、`Value`が暗黙的に宣言します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-727">If the parameter is omitted, a parameter named `Value` is implicitly declared.</span></span>

<span data-ttu-id="b36aa-728">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="b36aa-728">__Note.__</span></span> <span data-ttu-id="b36aa-729">`Set` アクセサーには、サブルーチンが行の配置で同じ制限があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-729">`Set` accessors have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="b36aa-730">最初のステートメント、end ステートメント、およびブロックする必要がありますすべて表示されますは論理行の先頭。</span><span class="sxs-lookup"><span data-stu-id="b36aa-730">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>

```antlr
PropertySetDeclaration
    : Attributes? AccessModifier? 'Set'
      ( OpenParenthesis ParameterList? CloseParenthesis )? LineTerminator
      Block?
      'End' 'Set' StatementTerminator
    ;
```

### <a name="default-properties"></a><span data-ttu-id="b36aa-731">既定のプロパティ</span><span class="sxs-lookup"><span data-stu-id="b36aa-731">Default Properties</span></span>

<span data-ttu-id="b36aa-732">修飾子を指定するプロパティ`Default`と呼ばれますが、*プロパティの既定*します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-732">A property that specifies the modifier `Default` is called a *default property*.</span></span> <span data-ttu-id="b36aa-733">プロパティを利用できる任意の型は、インターフェイスなど、既定のプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-733">Any type that allows properties may have a default property, including interfaces.</span></span> <span data-ttu-id="b36aa-734">プロパティの名前を持つインスタンスを修飾することがなく、既定のプロパティを参照できます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-734">The default property may be referenced without having to qualify the instance with the name of the property.</span></span> <span data-ttu-id="b36aa-735">したがって、クラスを指定</span><span class="sxs-lookup"><span data-stu-id="b36aa-735">Thus, given a class</span></span>

```vb
Class Test
    Public Default ReadOnly Property Item(i As Integer) As Integer
        Get
            Return i
        End Get
    End Property
End Class
```

<span data-ttu-id="b36aa-736">コード</span><span class="sxs-lookup"><span data-stu-id="b36aa-736">the code</span></span>

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test()
        Dim y As Integer

        y = x(10)
    End Sub
End Module
```

<span data-ttu-id="b36aa-737">上記の式は、次の式と同じです。</span><span class="sxs-lookup"><span data-stu-id="b36aa-737">is equivalent to</span></span>

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test()
        Dim y As Integer

        y = x.Item(10)
    End Sub
End Module
```

<span data-ttu-id="b36aa-738">プロパティが宣言される`Default`、宣言されていないかどうか、既定のプロパティがすべてのプロパティの継承階層内でその名前でオーバー ロードになる`Default`かどうか。</span><span class="sxs-lookup"><span data-stu-id="b36aa-738">Once a property is declared `Default`, all of the properties overloaded on that name in the inheritance hierarchy become the default property, whether they have been declared `Default` or not.</span></span> <span data-ttu-id="b36aa-739">プロパティの宣言`Default`基底クラスが別の名前で、既定のプロパティは、その他の修飾子などを宣言するときに、派生クラスで`Shadows`または`Overrides`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-739">Declaring a property `Default` in a derived class when the base class declared a default property by another name does not require any other modifiers such as `Shadows` or `Overrides`.</span></span> <span data-ttu-id="b36aa-740">これは、既定のプロパティには、id または署名があるないため、そのため、影付きまたはオーバー ロードできません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-740">This is because the default property has no identity or signature and so cannot be shadowed or overloaded.</span></span> <span data-ttu-id="b36aa-741">例えば:</span><span class="sxs-lookup"><span data-stu-id="b36aa-741">For example:</span></span>

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

<span data-ttu-id="b36aa-742">このプログラムの出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-742">This program will produce the output:</span></span>

```
MoreDerived = 10
Derived = 10
Base = 10
```

<span data-ttu-id="b36aa-743">型の中で宣言されているすべての既定のプロパティは、同じ名前を指定する必要があり、わかりやすくするために、指定する必要があります、`Default`修飾子。</span><span class="sxs-lookup"><span data-stu-id="b36aa-743">All default properties declared within a type must have the same name and, for clarity, must specify the `Default` modifier.</span></span> <span data-ttu-id="b36aa-744">外側のクラスのインスタンスを割り当てるときに、インデックス パラメーターのない既定のプロパティがあいまいな状況になる、ため既定のプロパティはインデックス パラメーターを持つ必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-744">Because a default property with no index parameters would cause an ambiguous situation when assigning instances of the containing class, default properties must have index parameters.</span></span> <span data-ttu-id="b36aa-745">さらで 1 つのプロパティがオーバー ロードの場合、特定の名前が含まれます。、`Default`修飾子は、その名前でオーバー ロードされたすべてのプロパティに指定してください。</span><span class="sxs-lookup"><span data-stu-id="b36aa-745">Furthermore, if one property overloaded on a particular name includes the `Default` modifier, all properties overloaded on that name must specify it.</span></span> <span data-ttu-id="b36aa-746">既定のプロパティができない可能性があります`Shared`、プロパティの少なくとも 1 つのアクセサーがすることはできませんと`Private`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-746">Default properties may not be `Shared`, and at least one accessor of the property must not be `Private`.</span></span>

### <a name="automatically-implemented-properties"></a><span data-ttu-id="b36aa-747">自動実装プロパティ</span><span class="sxs-lookup"><span data-stu-id="b36aa-747">Automatically Implemented Properties</span></span>

<span data-ttu-id="b36aa-748">アクセサーの宣言を省略すると、プロパティ場合、プロパティの実装が自動的を指定するプロパティがインターフェイスで宣言されているかが宣言されている場合を除き、`MustOverride`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-748">If a property omits declaration of any accessors, an implementation of the property will be automatically supplied unless the property is declared in an interface or is declared `MustOverride`.</span></span> <span data-ttu-id="b36aa-749">引数なしで読み取り/書き込みプロパティのみを自動的に実装できます。それ以外の場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-749">Only read/write properties with no arguments can be automatically implemented; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="b36aa-750">自動的に実装されたプロパティを`x`、別のプロパティをオーバーライドする 1 つでも、プライベートのローカル変数が導入されています`_x`プロパティと同じ型を持つ。</span><span class="sxs-lookup"><span data-stu-id="b36aa-750">An automatically implemented property `x`, even one overriding another property, introduces a private local variable `_x` with the same type as the property.</span></span> <span data-ttu-id="b36aa-751">ローカル変数の名前と別の宣言間で競合がある場合は、コンパイル時エラーが報告されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-751">If there is a collision between the local variable's name and another declaration, a compile-time error will be reported.</span></span> <span data-ttu-id="b36aa-752">自動的に実装されたプロパティの`Get`アクセサーには、ローカル プロパティの値が返されます`Set`ローカルの値を設定するアクセサーを指定します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-752">The automatically implemented property's `Get` accessor returns the value of the local and the property's `Set` accessor that sets the value of the local.</span></span> <span data-ttu-id="b36aa-753">たとえば、次のような宣言があるとします。</span><span class="sxs-lookup"><span data-stu-id="b36aa-753">For example, the declaration:</span></span>

```vb
Public Property x() As Integer
```

<span data-ttu-id="b36aa-754">約と同等です。</span><span class="sxs-lookup"><span data-stu-id="b36aa-754">is roughly equivalent to:</span></span>

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

<span data-ttu-id="b36aa-755">変数の宣言と同様、自動的に実装されたプロパティは、初期化子を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-755">As with variable declarations, an automatically implemented property can include an initializer.</span></span> <span data-ttu-id="b36aa-756">例えば:</span><span class="sxs-lookup"><span data-stu-id="b36aa-756">For example:</span></span>

```vb
Public Property x() As Integer = 10
Public Shared Property y() As New Customer() With { .Name = "Bob" }
```

<span data-ttu-id="b36aa-757">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="b36aa-757">__Note.__</span></span> <span data-ttu-id="b36aa-758">自動的に実装されたプロパティが初期化されると、基になるフィールドではないプロパティで初期化されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-758">When an automatically implemented property is initialized, it is initialized through the property, not the underlying field.</span></span> <span data-ttu-id="b36aa-759">これは、このようにする必要がある場合、初期化をインターセプト プロパティをオーバーライドすることができます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-759">This is so overriding properties can intercept the initialization if they need to.</span></span>

<span data-ttu-id="b36aa-760">配列初期化子は、配列の境界を明示的に指定する方法はありませんが、自動的に実装されたプロパティで許可されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-760">Array initializers are allowed on automatically implemented properties, except that there is no way to specify the array bounds explicitly.</span></span>  <span data-ttu-id="b36aa-761">例えば:</span><span class="sxs-lookup"><span data-stu-id="b36aa-761">For example:</span></span>

```vb
' Valid
Property x As Integer() = {1, 2, 3}
Property y As Integer(,) = {{1, 2, 3}, {12, 13, 14}, {11, 10, 9}}

' Invalid
Property x4(5) As Short
```

### <a name="iterator-properties"></a><span data-ttu-id="b36aa-762">反復子のプロパティ</span><span class="sxs-lookup"><span data-stu-id="b36aa-762">Iterator Properties</span></span>

<span data-ttu-id="b36aa-763">*反復子プロパティ*プロパティ、`Iterator`修飾子。</span><span class="sxs-lookup"><span data-stu-id="b36aa-763">An *iterator property* is a property with the `Iterator` modifier.</span></span> <span data-ttu-id="b36aa-764">使用されてと同じ理由で反復子メソッド (セクション[反復子メソッド](statements.md#iterator-methods))、シーケンスを生成する便利な方法として--使用で使用できる 1 つ、`For Each`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="b36aa-764">It is used for the same reason an iterator method (Section [Iterator Methods](statements.md#iterator-methods)) is used -- as a convenient way to generate a sequence, one which can be consumed by the `For Each` statement.</span></span> <span data-ttu-id="b36aa-765">`Get`反復子のプロパティのアクセサーが反復子メソッドと同じ方法で解釈されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-765">The `Get` accessor of an iterator property is interpreted in the same way as an iterator method.</span></span>

<span data-ttu-id="b36aa-766">反復子のプロパティを明示的にいる必要があります`Get`アクセサー、およびその型である必要があります`IEnumerator`、または`IEnumerable`、または`IEnumerator(Of T)`または`IEnumerable(Of T)`一部`T`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-766">An iterator property must have an explicit `Get` accessor, and its type must be `IEnumerator`, or `IEnumerable`, or `IEnumerator(Of T)` or `IEnumerable(Of T)` for some `T`.</span></span>

<span data-ttu-id="b36aa-767">反復子のプロパティの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-767">Here is an example of an iterator property:</span></span>

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

## <a name="operators"></a><span data-ttu-id="b36aa-768">演算子</span><span class="sxs-lookup"><span data-stu-id="b36aa-768">Operators</span></span>

<span data-ttu-id="b36aa-769">*演算子*方法は、含んでいるクラスの既存の Visual Basic の演算子の意味を定義します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-769">*Operators* are methods that define the meaning of an existing Visual Basic operator for the containing class.</span></span> <span data-ttu-id="b36aa-770">演算子が式内のクラスに適用されるときに、演算子は、クラスで定義されている演算子メソッドの呼び出しにコンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-770">When the operator is applied to the class in an expression, the operator is compiled into a call to the operator method defined in the class.</span></span> <span data-ttu-id="b36aa-771">演算子を定義するクラスとも呼ばれます*オーバー ロード*演算子。</span><span class="sxs-lookup"><span data-stu-id="b36aa-771">Defining an operator for a class is also known as *overloading* the operator.</span></span>

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

<span data-ttu-id="b36aa-772">既に存在する; 演算子をオーバー ロードすることはできません。実際には、これが変換演算子に当てはまります主になりました。</span><span class="sxs-lookup"><span data-stu-id="b36aa-772">It is not possible to overload an operator that already exists; in practice, this primarily applies to conversion operators.</span></span> <span data-ttu-id="b36aa-773">たとえば、派生クラスから基底クラスへの変換をオーバー ロードすることはできません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-773">For example, it is not possible to overload the conversion from a derived class to a base class:</span></span>

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

<span data-ttu-id="b36aa-774">演算子は、単語の一般的な意味でもオーバー ロードできます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-774">Operators can also be overloaded in the common sense of the word:</span></span>

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

<span data-ttu-id="b36aa-775">演算子の宣言明示的に追加しない名前に含まれる型の宣言領域です。ただし、文字"op_"で始まる対応するメソッドを暗黙的に宣言が操作を行います。</span><span class="sxs-lookup"><span data-stu-id="b36aa-775">Operator declarations do not explicitly add names to the containing type's declaration space; however they do implicitly declare a corresponding method starting with the characters "op_".</span></span> <span data-ttu-id="b36aa-776">次のセクションでは、各演算子に対応するメソッド名を一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-776">The following sections list the corresponding method names with each operator.</span></span>

<span data-ttu-id="b36aa-777">定義可能な演算子の 3 つのクラスがある: 単項演算子、二項演算子、および変換演算子。</span><span class="sxs-lookup"><span data-stu-id="b36aa-777">There are three classes of operators that can be defined: unary operators, binary operators and conversion operators.</span></span> <span data-ttu-id="b36aa-778">すべての演算子の宣言では、特定の制限を共有します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-778">All operator declarations share certain restrictions:</span></span>

* <span data-ttu-id="b36aa-779">演算子の宣言は常にあります`Public`と`Shared`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-779">Operator declarations must always be `Public` and `Shared`.</span></span> <span data-ttu-id="b36aa-780">`Public`修飾子と想定のコンテキストで修飾子を省略できます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-780">The `Public` modifier can be omitted in contexts where the modifier will be assumed.</span></span>

* <span data-ttu-id="b36aa-781">演算子のパラメーターを宣言することはできません`ByRef`、`Optional`または`ParamArray`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-781">The parameters of an operator cannot be declared `ByRef`, `Optional` or `ParamArray`.</span></span>

* <span data-ttu-id="b36aa-782">少なくとも 1 つのオペランドまたは戻り値の型は、演算子が含まれる型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-782">The type of at least one of the operands or the return value must be the type that contains the operator.</span></span>

* <span data-ttu-id="b36aa-783">演算子に対して定義されている関数の戻り変数はありません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-783">There is no function return variable defined for operators.</span></span> <span data-ttu-id="b36aa-784">そのため、`Return`演算子の本文から値を返すステートメントを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-784">Therefore, the `Return` statement must be used to return values from an operator body.</span></span>

<span data-ttu-id="b36aa-785">これらの制限の唯一の例外は、null 許容値型に適用されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-785">The only exception to these restrictions applies to nullable value types.</span></span> <span data-ttu-id="b36aa-786">Null 許容値型は、実際の型定義を持っていないために、値型は、ユーザー定義演算子の型の null 許容バージョンを宣言できます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-786">Since nullable value types do not have an actual type definition, a value type can declare user-defined operators for the nullable version of the type.</span></span> <span data-ttu-id="b36aa-787">特定のユーザー定義演算子に型を宣言できるかどうかを決定するときに、`?`修飾子がからすべての宣言に含まれる型の有効性チェックの目的で最初に削除されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-787">When determining whether a type can declare a particular user-defined operator, the `?` modifiers are first dropped off of all of the types involved in the declaration for the purposes of validity checking.</span></span> <span data-ttu-id="b36aa-788">この緩和したトークンは、戻り値の型には適用されません、`IsTrue`と`IsFalse`演算子も返す必要がある`Boolean`ではなく、`Boolean?`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-788">This relaxation does not apply to the return type of the `IsTrue` and `IsFalse` operators; they must still return `Boolean`, not `Boolean?`.</span></span>

<span data-ttu-id="b36aa-789">演算子の宣言では、優先順位と演算子の結合を変更できません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-789">The precedence and associativity of an operator cannot be modified by an operator declaration.</span></span>

<span data-ttu-id="b36aa-790">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="b36aa-790">__Note.__</span></span> <span data-ttu-id="b36aa-791">演算子では、サブルーチンが行の配置で同じ制限があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-791">Operators have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="b36aa-792">最初のステートメント、end ステートメント、およびブロックする必要がありますすべて表示されますは論理行の先頭。</span><span class="sxs-lookup"><span data-stu-id="b36aa-792">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>


### <a name="unary-operators"></a><span data-ttu-id="b36aa-793">単項演算子</span><span class="sxs-lookup"><span data-stu-id="b36aa-793">Unary Operators</span></span>

<span data-ttu-id="b36aa-794">次のような単項演算子をオーバー ロードできます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-794">The following unary operators can be overloaded:</span></span>

* <span data-ttu-id="b36aa-795">単項プラス演算子`+`(対応するメソッド: `op_UnaryPlus`)</span><span class="sxs-lookup"><span data-stu-id="b36aa-795">The unary plus operator `+` (corresponding method: `op_UnaryPlus`)</span></span>

* <span data-ttu-id="b36aa-796">単項マイナス演算子`-`(対応するメソッド: `op_UnaryNegation`)</span><span class="sxs-lookup"><span data-stu-id="b36aa-796">The unary minus operator `-` (corresponding method: `op_UnaryNegation`)</span></span>

* <span data-ttu-id="b36aa-797">論理`Not`演算子 (対応するメソッド: `op_OnesComplement`)</span><span class="sxs-lookup"><span data-stu-id="b36aa-797">The logical `Not` operator (corresponding method: `op_OnesComplement`)</span></span>

* <span data-ttu-id="b36aa-798">`IsTrue`と`IsFalse`演算子 (対応するメソッド: `op_True`、 `op_False`)</span><span class="sxs-lookup"><span data-stu-id="b36aa-798">The `IsTrue` and `IsFalse` operators (corresponding methods: `op_True`, `op_False`)</span></span>

<span data-ttu-id="b36aa-799">すべてのオーバー ロードされた単項演算子は、包含する型の 1 つのパラメーターを受け取る必要があり、を除き、任意の型を返す可能性があります`IsTrue`と`IsFalse`を返す必要があります`Boolean`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-799">All overloaded unary operators must take a single parameter of the containing type and may return any type, except for `IsTrue` and `IsFalse`, which must return `Boolean`.</span></span> <span data-ttu-id="b36aa-800">含んでいる型がジェネリック型の場合は、型パラメーターは、含む型の型パラメーターと一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-800">If the containing type is a generic type, the type parameters must match the containing type's type parameters.</span></span> <span data-ttu-id="b36aa-801">例えば以下のようにします。</span><span class="sxs-lookup"><span data-stu-id="b36aa-801">For example,</span></span>

```vb
Structure Complex
    ...

    Public Shared Operator +(v As Complex) As Complex
        Return v
    End Operator
End Structure
```

<span data-ttu-id="b36aa-802">型のオーバー ロードのいずれかの場合`IsTrue`または`IsFalse`、その他の同様にオーバー ロードする必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-802">If a type overloads one of `IsTrue` or `IsFalse`, then it must overload the other as well.</span></span> <span data-ttu-id="b36aa-803">1 つはオーバー ロードだけの場合、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-803">If only one is overloaded, a compile-time error results.</span></span>

<span data-ttu-id="b36aa-804">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="b36aa-804">__Note.__</span></span> <span data-ttu-id="b36aa-805">`IsTrue` `IsFalse`予約された単語は使用されません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-805">`IsTrue` and `IsFalse` are not reserved words.</span></span>

### <a name="binary-operators"></a><span data-ttu-id="b36aa-806">二項演算子</span><span class="sxs-lookup"><span data-stu-id="b36aa-806">Binary Operators</span></span>

<span data-ttu-id="b36aa-807">次の二項演算子をオーバー ロードできます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-807">The following binary operators can be overloaded:</span></span>

* <span data-ttu-id="b36aa-808">追加`+`、減算`-`、乗算`*`、除算`/`、整数の除算`\`、剰余`Mod`と指数演算`^`演算子 (対応するメソッド。`op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_IntegerDivision`, `op_Modulus`, `op_Exponent`)</span><span class="sxs-lookup"><span data-stu-id="b36aa-808">The addition `+`, subtraction `-`, multiplication `*`, division `/`, integral division `\`, modulo `Mod` and exponentiation `^` operators (corresponding method: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_IntegerDivision`, `op_Modulus`, `op_Exponent`)</span></span>

* <span data-ttu-id="b36aa-809">関係演算子`=`、 `<>`、 `<`、 `>`、 `<=`、 `>=` (対応するメソッド: `op_Equality`、 `op_Inequality`、 `op_LessThan`、 `op_GreaterThan`、 `op_LessThanOrEqual`, `op_GreaterThanOrEqual`).</span><span class="sxs-lookup"><span data-stu-id="b36aa-809">The relational operators `=`, `<>`, `<`, `>`, `<=`, `>=` (corresponding methods: `op_Equality`, `op_Inequality`, `op_LessThan`, `op_GreaterThan`, `op_LessThanOrEqual`, `op_GreaterThanOrEqual`).</span></span> <span data-ttu-id="b36aa-810">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="b36aa-810">__Note.__</span></span> <span data-ttu-id="b36aa-811">等値演算子をオーバー ロードできる、中には、(代入ステートメントでのみ使用)、代入演算子はオーバー ロードできません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-811">While the equality operator can be overloaded, the assignment operator (used only in assignment statements) cannot be overloaded.</span></span>

* <span data-ttu-id="b36aa-812">`Like`演算子 (対応するメソッド: `op_Like`)</span><span class="sxs-lookup"><span data-stu-id="b36aa-812">The `Like` operator (corresponding method: `op_Like`)</span></span>

* <span data-ttu-id="b36aa-813">連結演算子`&`(対応するメソッド: `op_Concatenate`)</span><span class="sxs-lookup"><span data-stu-id="b36aa-813">The concatenation operator `&` (corresponding method: `op_Concatenate`)</span></span>

* <span data-ttu-id="b36aa-814">論理`And`、`Or`と`Xor`演算子 (対応するメソッド: `op_BitwiseAnd`、 `op_BitwiseOr`、 `op_ExclusiveOr`)</span><span class="sxs-lookup"><span data-stu-id="b36aa-814">The logical `And`, `Or` and `Xor` operators (corresponding methods: `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`)</span></span>

* <span data-ttu-id="b36aa-815">シフト演算子`<<`と`>>`(対応するメソッド: `op_LeftShift`、 `op_RightShift`)</span><span class="sxs-lookup"><span data-stu-id="b36aa-815">The shift operators `<<` and `>>` (corresponding methods: `op_LeftShift`, `op_RightShift`)</span></span>

<span data-ttu-id="b36aa-816">すべてのオーバー ロードされた二項演算子は、パラメーターの 1 つとして含んでいる型を取得する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-816">All overloaded binary operators must take the containing type as one of the parameters.</span></span> <span data-ttu-id="b36aa-817">含んでいる型がジェネリック型の場合は、型パラメーターは、含む型の型パラメーターと一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-817">If the containing type is a generic type, the type parameters must match the containing type's type parameters.</span></span> <span data-ttu-id="b36aa-818">シフト演算子をさらに制限を包含する型の最初のパラメーターを要求するには、この規則型の 2 番目のパラメーターが必ず`Integer`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-818">The shift operators further restrict this rule to require the first parameter to be of the containing type; the second parameter must always be of type `Integer`.</span></span>

<span data-ttu-id="b36aa-819">次の二項演算子は、ペアで宣言する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-819">The following binary operators must be declared in pairs:</span></span>

* <span data-ttu-id="b36aa-820">演算子`=`と演算子 `<>`</span><span class="sxs-lookup"><span data-stu-id="b36aa-820">Operator `=` and operator `<>`</span></span>

* <span data-ttu-id="b36aa-821">演算子`>`と演算子 `<`</span><span class="sxs-lookup"><span data-stu-id="b36aa-821">Operator `>` and operator `<`</span></span>

* <span data-ttu-id="b36aa-822">演算子`>=`と演算子 `<=`</span><span class="sxs-lookup"><span data-stu-id="b36aa-822">Operator `>=` and operator `<=`</span></span>

<span data-ttu-id="b36aa-823">ペアの 1 つが宣言されている場合、この属性はパラメーターと戻り値の型が一致する、もう一方も宣言する必要があります。 またはコンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-823">If one of the pair is declared, then the other must also be declared with matching parameter and return types, or a compile-time error will result.</span></span> <span data-ttu-id="b36aa-824">(__に注意してください。__</span><span class="sxs-lookup"><span data-stu-id="b36aa-824">(__Note.__</span></span> <span data-ttu-id="b36aa-825">関係演算子のペアの宣言を要求する場合の目的は、少なくとも最小レベルのオーバー ロードされた演算子で論理的な一貫性を確保する)。</span><span class="sxs-lookup"><span data-stu-id="b36aa-825">The purpose of requiring paired declarations of relational operators is to try and ensure at least a minimum level of logical consistency in overloaded operators.)</span></span>

<span data-ttu-id="b36aa-826">関係演算子とは異なり、除算と整数の除算演算子をオーバー ロードはお勧めがエラーではありません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-826">In contrast to the relational operators, overloading both the division and integral division operators is strongly discouraged, although not an error.</span></span> <span data-ttu-id="b36aa-827">(__に注意してください。__</span><span class="sxs-lookup"><span data-stu-id="b36aa-827">(__Note.__</span></span> <span data-ttu-id="b36aa-828">一般に、除算の 2 種類が完全に別にする必要があります。 除算をサポートする型が整数 (後者をサポートすること`\`) かどうか (後者をサポートすること`/`)。</span><span class="sxs-lookup"><span data-stu-id="b36aa-828">In general, the two types of division should be entirely distinct: a type that supports division is either integral (in which case it should support `\`) or not (in which case it should support `/`).</span></span> <span data-ttu-id="b36aa-829">両方の演算子を定義するとエラーを行うことを検討しましたが、実習を許可するが、厳密にこれを防ぐために最も安全な方法だと考えたため、言語は、一般的には 2 種類の方法は Visual Basic では、除算を区別しません、)。</span><span class="sxs-lookup"><span data-stu-id="b36aa-829">We considered making it an error to define both operators, but because their languages do not generally distinguish between two types of division the way Visual Basic does, we felt it was safest to allow the practice but strongly discourage it.)</span></span>

<span data-ttu-id="b36aa-830">複合代入演算子は直接オーバー ロードすることはできません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-830">Compound assignment operators cannot be overloaded directly.</span></span> <span data-ttu-id="b36aa-831">代わりに、対応する二項演算子をオーバー ロードするときに、複合代入演算子はオーバー ロードされた演算子を使用します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-831">Instead, when the corresponding binary operator is overloaded, the compound assignment operator will use the overloaded operator.</span></span> <span data-ttu-id="b36aa-832">例えば:</span><span class="sxs-lookup"><span data-stu-id="b36aa-832">For example:</span></span>

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

### <a name="conversion-operators"></a><span data-ttu-id="b36aa-833">変換演算子</span><span class="sxs-lookup"><span data-stu-id="b36aa-833">Conversion Operators</span></span>

<span data-ttu-id="b36aa-834">変換演算子は、型の間で新しい変換を定義します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-834">Conversion operators define new conversions between types.</span></span> <span data-ttu-id="b36aa-835">これらの新しい変換と呼びます*ユーザー定義の変換*します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-835">These new conversions are called *user-defined conversions*.</span></span> <span data-ttu-id="b36aa-836">変換演算子は、戻り値の型変換演算子によって示される、ターゲットの型に、変換演算子のパラメーターの型によって示される、ソースの種類からに変換します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-836">A conversion operator converts from a source type, indicated by the parameter type of the conversion operator, to a target type, indicated by the return type of the conversion operator.</span></span> <span data-ttu-id="b36aa-837">変換は、拡大または縮小のいずれかに分類する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-837">Conversions must be classified as either widening or narrowing.</span></span> <span data-ttu-id="b36aa-838">変換演算子の宣言を含む、`Widening`キーワードにはユーザー定義の拡大変換が導入されています (対応するメソッド: `op_Implicit`)。</span><span class="sxs-lookup"><span data-stu-id="b36aa-838">A conversion operator declaration that includes the `Widening` keyword introduces a user-defined widening conversion (corresponding method: `op_Implicit`).</span></span> <span data-ttu-id="b36aa-839">変換演算子の宣言を含む、`Narrowing`キーワードには、ユーザー定義の縮小変換が導入されています (対応するメソッド: `op_Explicit`)。</span><span class="sxs-lookup"><span data-stu-id="b36aa-839">A conversion operator declaration that includes the `Narrowing` keyword introduces a user-defined narrowing conversion (corresponding method: `op_Explicit`).</span></span>

<span data-ttu-id="b36aa-840">一般に、例外をスローしないと、情報が失われることをユーザー定義の拡大変換を設計する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-840">In general, user-defined widening conversions should be designed to never throw exceptions and never lose information.</span></span> <span data-ttu-id="b36aa-841">(たとえば、元の引数が範囲外です) ために、ユーザー定義の変換の例外が発生する場合はその変換 (上位ビットが破棄) などの情報の損失は縮小変換として定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-841">If a user-defined conversion can cause exceptions (for example, because the source argument is out of range) or loss of information (such as discarding high-order bits), then that conversion should be defined as a narrowing conversion.</span></span> <span data-ttu-id="b36aa-842">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-842">In the example:</span></span>

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

<span data-ttu-id="b36aa-843">変換`Digit`に`Byte`拡大変換は、決してに例外がスローまたはについてがからの変換を失ったため`Byte`に`Digit`から縮小変換は、`Digit`のみ表すことができます、可能な値のサブセットを`Byte`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-843">the conversion from `Digit` to `Byte` is a widening conversion because it never throws exceptions or loses information, but the conversion from `Byte` to `Digit` is a narrowing conversion since `Digit` can only represent a subset of the possible values of a `Byte`.</span></span>

<span data-ttu-id="b36aa-844">すべての他の型のメンバー オーバー ロードできるとは異なりは、変換演算子のシグネチャには、変換のターゲット型が含まれています。</span><span class="sxs-lookup"><span data-stu-id="b36aa-844">Unlike all other type members that can be overloaded, the signature of a conversion operator includes the target type of the conversion.</span></span> <span data-ttu-id="b36aa-845">これは、対象の戻り値の型は、署名に参加しているだけの型のメンバーです。</span><span class="sxs-lookup"><span data-stu-id="b36aa-845">This is the only type member for which the return type participates in the signature.</span></span> <span data-ttu-id="b36aa-846">拡大または縮小、変換演算子の分類、演算子のシグネチャの一部はできませんただし、します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-846">The widening or narrowing classification of a conversion operator, however, is not part of the operator's signature.</span></span> <span data-ttu-id="b36aa-847">したがって、クラスまたは構造体は拡大変換演算子と同じソースとターゲット型の縮小変換演算子の両方に宣言できません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-847">Thus, a class or structure cannot declare both a widening conversion operator and a narrowing conversion operator with the same source and target types.</span></span>

<span data-ttu-id="b36aa-848">ユーザー定義変換演算子にするか、それを含む型から変換する必要があります--、たとえば、クラスのことは`C`から変換を定義する`C`に`Integer`との間`Integer`に`C`からはできません`Integer`に`Boolean`します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-848">A user-defined conversion operator must convert either to or from the containing type -- for example, it is possible for a class `C` to define a conversion from `C` to `Integer` and from `Integer` to `C`, but not from `Integer` to `Boolean`.</span></span> <span data-ttu-id="b36aa-849">含んでいる型がジェネリック型の場合は、型パラメーターは、含む型の型パラメーターと一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-849">If the containing type is a generic type, the type parameters must match the containing type's type parameters.</span></span> <span data-ttu-id="b36aa-850">また、組み込みの (つまり非ユーザー定義) の変換を再定義することはできません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-850">Also, it is not possible to redefine an intrinsic (i.e. non-user-defined) conversion.</span></span> <span data-ttu-id="b36aa-851">その結果、型が変換を宣言できません場所。</span><span class="sxs-lookup"><span data-stu-id="b36aa-851">As a result, a type cannot declare a conversion where:</span></span>

* <span data-ttu-id="b36aa-852">ソースの種類と変換先の型は、同じです。</span><span class="sxs-lookup"><span data-stu-id="b36aa-852">The source type and the destination type are the same.</span></span>

* <span data-ttu-id="b36aa-853">ソースの種類と変換先の型の両方が変換演算子を定義する型ではありません。</span><span class="sxs-lookup"><span data-stu-id="b36aa-853">Both the source type and the destination type are not the type that defines the conversion operator.</span></span>

* <span data-ttu-id="b36aa-854">ソースの種類または変換先の型がインターフェイス型です。</span><span class="sxs-lookup"><span data-stu-id="b36aa-854">The source type or the destination type is an interface type.</span></span>

* <span data-ttu-id="b36aa-855">ソースの種類と変換先の型が継承によって関連付けられる (を含む`Object`)。</span><span class="sxs-lookup"><span data-stu-id="b36aa-855">The source type and destination types are related by inheritance (including `Object`).</span></span>

<span data-ttu-id="b36aa-856">これらの規則の唯一の例外は、null 許容値型に適用されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-856">The only exception to these rules applies to nullable value types.</span></span> <span data-ttu-id="b36aa-857">Null 許容値型は、実際の型定義を持っていないために、値型は、null 許容型のバージョンについては、ユーザー定義の変換を宣言できます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-857">Since nullable value types do not have an actual type definition, a value type can declare user-defined conversions for the nullable version of the type.</span></span> <span data-ttu-id="b36aa-858">型が特定のユーザー定義変換を宣言するかどうかを決定するときに、`?`修飾子がからすべての宣言に含まれる型の有効性チェックの目的で最初に削除されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-858">When determining whether a type can declare a particular user-defined conversion, the `?` modifiers are first dropped off of all of the types involved in the declaration for the purposes of validity checking.</span></span> <span data-ttu-id="b36aa-859">したがって、次の宣言は有効なため、`S`からの変換を定義できます`S`に`T`:</span><span class="sxs-lookup"><span data-stu-id="b36aa-859">Thus, the following declaration is valid because `S` can define a conversion from `S` to `T`:</span></span>

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

<span data-ttu-id="b36aa-860">次の宣言が無効です、ただし、構造体`S`からの変換を定義することはできません`S`に`S`:</span><span class="sxs-lookup"><span data-stu-id="b36aa-860">The following declaration is not valid, however, because structure `S` cannot define a conversion from `S` to `S`:</span></span>

```vb
Structure S
    Public Shared Widening Operator CType(ByVal v As S) As S?
        ...
    End Operator
End Structure
```

### <a name="operator-mapping"></a><span data-ttu-id="b36aa-861">演算子のマッピング</span><span class="sxs-lookup"><span data-stu-id="b36aa-861">Operator Mapping</span></span>

<span data-ttu-id="b36aa-862">Visual Basic をサポートする演算子のセットは、演算子のセットが .NET Framework の他の言語を一致も一致しない可能性があります、ため、いくつかの演算子が定義されている、または使用する場合は、その他の演算子に特別にマップされます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-862">Because the set of operators that Visual Basic supports may not exactly match the set of operators that other languages on the .NET Framework, some operators are mapped specially onto other operators when being defined or used.</span></span> <span data-ttu-id="b36aa-863">具体的には、次のように使用します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-863">Specifically:</span></span>

* <span data-ttu-id="b36aa-864">整数除算演算子を定義すると、通常の除算演算子 (その他の言語からのみ使用可能) 整数の除算演算子を呼び出すことは自動的に定義します。</span><span class="sxs-lookup"><span data-stu-id="b36aa-864">Defining an integral division operator will automatically define a regular division operator (usable only from other languages) that will call the integral division operator.</span></span>

* <span data-ttu-id="b36aa-865">オーバー ロード、 `Not`、 `And`、および`Or`演算子が論理/ビット処理演算子の区別を他の言語の観点からのビットごとの演算子をオーバー ロードします。</span><span class="sxs-lookup"><span data-stu-id="b36aa-865">Overloading the `Not`, `And`, and `Or` operators will overload only the bitwise operator from the perspective of other languages that distinguish between logical and bitwise operators.</span></span>

* <span data-ttu-id="b36aa-866">論理/ビット処理演算子を識別するための言語でのみ論理演算子をオーバー ロードするクラス (を使用する言語つまり`op_LogicalNot`、 `op_LogicalAnd`、および`op_LogicalOr`の`Not`、 `And`、および`Or`、それぞれ)、論理演算子を Visual Basic の論理演算子にマップする必要があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-866">A class that overloads only the logical operators in a language that distinguishes between logical and bitwise operators (i.e. a languages that uses `op_LogicalNot`, `op_LogicalAnd`, and `op_LogicalOr` for `Not`, `And`, and `Or`, respectively) will have their logical operators mapped onto the Visual Basic logical operators.</span></span> <span data-ttu-id="b36aa-867">論理/ビット処理演算子の両方がオーバー ロードする場合は、ビットごとの演算子のみが使用されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-867">If both the logical and bitwise operators are overloaded, only the bitwise operators will be used.</span></span>

* <span data-ttu-id="b36aa-868">オーバー ロード、`<<`と`>>`演算子が符号付きと符号なしのシフト演算子の区別を他の言語の観点から符号付きの演算子のみをオーバー ロードします。</span><span class="sxs-lookup"><span data-stu-id="b36aa-868">Overloading the `<<` and `>>` operators will overload only the signed operators from the perspective of other languages that distinguish between signed and unsigned shift operators.</span></span>

* <span data-ttu-id="b36aa-869">符号なしのシフト演算子のみをオーバー ロードするクラスには、対応する Visual Basic のシフト演算子にマップされる符号なしのシフト演算子があります。</span><span class="sxs-lookup"><span data-stu-id="b36aa-869">A class that overloads only an unsigned shift operator will have the unsigned shift operator mapped onto the corresponding Visual Basic shift operator.</span></span> <span data-ttu-id="b36aa-870">両方を符号なしと符号付きのシフト演算子はオーバー ロードする場合は、署名された shift 演算子のみが使用されます。</span><span class="sxs-lookup"><span data-stu-id="b36aa-870">If both an unsigned and signed shift operator is overloaded, only the signed shift operator will be used.</span></span>
