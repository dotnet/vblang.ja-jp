# <a name="general-concepts"></a><span data-ttu-id="0cf9a-101">一般的な概念</span><span class="sxs-lookup"><span data-stu-id="0cf9a-101">General Concepts</span></span>

<span data-ttu-id="0cf9a-102">この章では、さまざまな Microsoft Visual Basic 言語のセマンティクスを理解するために必要な概念について説明します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-102">This chapter covers a number of concepts that are required to understand the semantics of the Microsoft Visual Basic language.</span></span> <span data-ttu-id="0cf9a-103">Visual Basic プログラマまたは C と C++ のプログラマにとって馴染み深い概念の多くする必要がありますが、厳密な定義が異なる場合があります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-103">Many of the concepts should be familiar to Visual Basic programmers or C/C++ programmers, but their precise definitions may differ.</span></span>

## <a name="declarations"></a><span data-ttu-id="0cf9a-104">宣言</span><span class="sxs-lookup"><span data-stu-id="0cf9a-104">Declarations</span></span>

<span data-ttu-id="0cf9a-105">Visual Basic プログラムは名前付きエンティティで構成をされます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-105">A Visual Basic program is made up of named entities.</span></span> <span data-ttu-id="0cf9a-106">これらのエンティティがで導入された*宣言*プログラムの「意味」を表します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-106">These entities are introduced through *declarations* and represent the "meaning" of the program.</span></span>

<span data-ttu-id="0cf9a-107">最上位レベルの場合で*名前空間*は入れ子になった名前空間と型などの他のエンティティを整理するエンティティです。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-107">At a top level, *namespaces* are entities that organize other entities, such as nested namespaces and types.</span></span> <span data-ttu-id="0cf9a-108">*型*は値を記述し、実行可能コードを定義するエンティティです。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-108">*Types* are entities that describe values and define executable code.</span></span> <span data-ttu-id="0cf9a-109">型は、入れ子にされた型を含めることがよぶ型のメンバー場合があります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-109">Types may contain nested types and type members.</span></span> <span data-ttu-id="0cf9a-110">*メンバーの入力*定数、変数、メソッド、演算子、プロパティ、イベント、列挙値、およびコンス トラクター。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-110">*Type members* are constants, variables, methods, operators, properties, events, enumeration values, and constructors.</span></span>

<span data-ttu-id="0cf9a-111">その他のエンティティを含むことのできるエンティティの定義、*宣言領域*します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-111">An entity that can contain other entities defines a *declaration space*.</span></span> <span data-ttu-id="0cf9a-112">宣言または継承を通じていずれかの宣言領域にエンティティが導入されます。エンティティのコンテナーの宣言領域と呼びます*宣言コンテキスト*します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-112">Entities are introduced into a declaration space either through declarations or inheritance; the containing declaration space is called the entities' *declaration context*.</span></span> <span data-ttu-id="0cf9a-113">さらに入れ子になったエンティティ宣言を含むことのできる新しい宣言領域を定義する宣言領域内のエンティティをさらに宣言します。そのため、プログラム内の宣言は、宣言のスペースの階層を形成します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-113">Declaring an entity in a declaration space in turn defines a new declaration space that can contain further nested entity declarations; thus, the declarations in a program form a hierarchy of declaration spaces.</span></span>

<span data-ttu-id="0cf9a-114">除くをオーバー ロードされた型のメンバーの場合は同じ宣言のコンテキストに同じ種類の同じ名前のエンティティを導入する宣言に対して無効です。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-114">Except in the case of overloaded type members, it is invalid for declarations to introduce identically named entities of the same kind into the same declaration context.</span></span> <span data-ttu-id="0cf9a-115">さらに、宣言領域で可能性があります。 同じ名前のエンティティのさまざまな種類を含めることはありません。たとえば、宣言領域ことはありませんがあります変数とメソッド、同じ名前で。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-115">Additionally, a declaration space may never contain different kinds of entities with the same name; for example, a declaration space may never contain a variable and a method by the same name.</span></span>

<span data-ttu-id="0cf9a-116">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="0cf9a-116">__Note.__</span></span> <span data-ttu-id="0cf9a-117">他の言語 (たとえば、可能な場合、言語大文字と小文字は大文字小文字の区別に基づいてさまざまな宣言) の種類の異なる同じ名前のエンティティを含む宣言領域を作成する可能性がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-117">It may be possible in other languages to create a declaration space that contains different kinds of entities with the same name (for example, if the language is case sensitive and allows different declarations based on casing).</span></span> <span data-ttu-id="0cf9a-118">そのような状況で最もアクセスしやすいエンティティはその名前にバインドされたと見なされますエンティティの 1 つ以上の型が最もアクセスしやすい場合、名前があいまいです。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-118">In that situation, the most accessible entity is considered bound to that name; if more than one type of entity is most accessible then the name is ambiguous.</span></span> <span data-ttu-id="0cf9a-119">`Public` 方がより使いやすく`Protected Friend`、`Protected Friend`方がより使いやすく`Protected`または`Friend`、および`Protected`または`Friend`方がより使いやすく`Private`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-119">`Public` is more accessible than `Protected Friend`, `Protected Friend` is more accessible than `Protected` or `Friend`, and `Protected` or `Friend` is more accessible than `Private`.</span></span>

<span data-ttu-id="0cf9a-120">名前空間の宣言領域は、「開いている終了した場合は、」同じ完全修飾名で 2 つの名前空間宣言が同じ宣言領域に貢献するようにします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-120">The declaration space of a namespace is "open ended," so two namespace declarations with the same fully qualified name contribute to the same declaration space.</span></span> <span data-ttu-id="0cf9a-121">次の例で、2 つの名前空間宣言はここで完全修飾の名前を持つ 2 つのクラスを宣言する、同じ宣言領域に貢献`Data.Customer`と `Data.Order:`</span><span class="sxs-lookup"><span data-stu-id="0cf9a-121">In the example below, the two namespace declarations contribute to the same declaration space, in this case declaring two classes with the fully qualified names `Data.Customer` and `Data.Order:`</span></span>

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

<span data-ttu-id="0cf9a-122">同じ宣言領域に 2 つの宣言がドキュメントに投稿するため、宣言と同じ名前のクラスのそれぞれに含まれる場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-122">Because the two declarations contribute to the same declaration space, a compile-time error would occur if each contained a declaration of a class with the same name.</span></span>

### <a name="overloading-and-signatures"></a><span data-ttu-id="0cf9a-123">オーバー ロードと署名</span><span class="sxs-lookup"><span data-stu-id="0cf9a-123">Overloading and Signatures</span></span>

<span data-ttu-id="0cf9a-124">宣言領域内の同じ名前を持つエンティティの種類を宣言する唯一の方法は*オーバー ロード*します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-124">The only way to declare identically named entities of the same kind in a declaration space is through *overloading*.</span></span> <span data-ttu-id="0cf9a-125">メソッド、演算子、インスタンス コンス トラクターとプロパティのみがオーバー ロードします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-125">Only methods, operators, instance constructors, and properties may be overloaded.</span></span>

<span data-ttu-id="0cf9a-126">オーバー ロードされた型のメンバーは、一意のシグネチャを持つ必要があります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-126">Overloaded type members must possess unique signatures.</span></span> <span data-ttu-id="0cf9a-127">型のメンバーのシグネチャは、メンバーのパラメーターの型パラメーター、および数の数と種類で構成されます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-127">The signature of a type member consists of the number of type parameters, and the number and types of the member's parameters.</span></span> <span data-ttu-id="0cf9a-128">変換演算子には、シグネチャに、演算子の戻り値の型も含まれます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-128">Conversion operators also include the return type of the operator in the signature.</span></span>

<span data-ttu-id="0cf9a-129">次のメンバーのシグネチャの一部ではないとそのオーバー ロードできませんで。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-129">The following are not part of a member's signature, and hence cannot be overloaded on:</span></span>

* <span data-ttu-id="0cf9a-130">修飾子を型のメンバー (たとえば、`Shared`または`Private`)。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-130">Modifiers to a type member (for example, `Shared` or `Private`).</span></span>

* <span data-ttu-id="0cf9a-131">パラメーター修飾子 (たとえば、`ByVal`または`ByRef`)。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-131">Modifiers to a parameter (for example, `ByVal` or `ByRef`).</span></span>

* <span data-ttu-id="0cf9a-132">パラメーターの名前。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-132">The names of the parameters.</span></span>

* <span data-ttu-id="0cf9a-133">メソッドまたは (変換演算子) を除く演算子またはプロパティの要素の型の戻り値の型。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-133">The return type of a method or operator (except for conversion operators) or the element type of a property.</span></span>

* <span data-ttu-id="0cf9a-134">型パラメーターに対する制約。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-134">Constraints on a type parameter.</span></span>

<span data-ttu-id="0cf9a-135">次の例では、一連のオーバー ロードされたメソッドの宣言とそのシグネチャを示します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-135">The following example shows a set of overloaded method declarations along with their signatures.</span></span> <span data-ttu-id="0cf9a-136">この宣言は、同じシグネチャがあるいくつかのメソッド宣言のため、有効でないです。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-136">This declaration would not be valid since several of the method declarations have identical signatures.</span></span>

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

<span data-ttu-id="0cf9a-137">指定された型引数に基づいて、同じシグネチャを持つメンバーを含む可能性のあるジェネリック型定義に有効です。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-137">It is valid to define a generic type that may contain members with identical signatures based on the type arguments supplied.</span></span> <span data-ttu-id="0cf9a-138">オーバー ロード解決規則は、することはできませんを明確にする場合がありますが、このようなオーバー ロードでは、あいまいさを解消してみてくださいに使用されます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-138">Overload resolution rules are used to try and disambiguate between such overloads, although there may be situations in which it is impossible to disambiguate.</span></span> <span data-ttu-id="0cf9a-139">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-139">For example:</span></span>

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

## <a name="scope"></a><span data-ttu-id="0cf9a-140">スコープ</span><span class="sxs-lookup"><span data-stu-id="0cf9a-140">Scope</span></span>

<span data-ttu-id="0cf9a-141">*スコープ*のエンティティの名前は修飾なしには、その名前を参照することを内のすべての宣言スペースのセット。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-141">The *scope* of an entity's name is the set of all declaration spaces within which it is possible to refer to that name without qualification.</span></span> <span data-ttu-id="0cf9a-142">一般に、エンティティの名前のスコープには、宣言全体のコンテキストただし、エンティティの宣言では、同じ名前のエンティティの入れ子になった宣言を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-142">In general, the scope of an entity's name is its entire declaration context; however, an entity's declaration may contain nested declarations of entities with the same name.</span></span> <span data-ttu-id="0cf9a-143">その場合、入れ子になった entity *shadows*、または非表示に、外部のエンティティ、およびシャドウされたエンティティへのアクセスが修飾を通してのみです。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-143">In that case, the nested entity *shadows*, or hides, the outer entity, and access to the shadowed entity is only possible through qualification.</span></span>

<span data-ttu-id="0cf9a-144">入れ子によるシャドウするには、名前空間または名前空間内で入れ子になった型で、他の種類内で入れ子にされた型と型のメンバーの本文に発生します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-144">Shadowing through nesting occurs in namespaces or types nested within namespaces, in types nested within other types, and in the bodies of type members.</span></span> <span data-ttu-id="0cf9a-145">宣言の入れ子は常にによるシャドウ; 暗黙的に発生します明示的な構文は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-145">Shadowing through the nesting of declarations always occurs implicitly; no explicit syntax is required.</span></span>

<span data-ttu-id="0cf9a-146">次の例では、内で、`F`メソッドは、インスタンス変数`i`ローカル変数でシャドウが`i`が内、`G`メソッド、`i`インスタンス変数を参照します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-146">In the following example, within the `F` method, the instance variable `i` is shadowed by the local variable `i`, but within the `G` method, `i` still refers to the instance variable.</span></span>

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

<span data-ttu-id="0cf9a-147">内側のスコープに名前が外側のスコープでの名前を非表示にする場合は、その名前のすべてのオーバー ロードされた出現箇所をシャドウします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-147">When a name in an inner scope hides a name in an outer scope, it shadows all overloaded occurrences of that name.</span></span> <span data-ttu-id="0cf9a-148">次の例では、呼び出し`F(1)`呼び出す、`F`で宣言されている`Inner`ため、外部出現するすべての`F`内部宣言では表示されません。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-148">In the following example, the call `F(1)` invokes the `F` declared in `Inner` because all outer occurrences of `F` are hidden by the inner declaration.</span></span> <span data-ttu-id="0cf9a-149">同じ理由、呼び出しから`F("Hello")`エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-149">For the same reason, the call `F("Hello")` is in error.</span></span>

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

## <a name="inheritance"></a><span data-ttu-id="0cf9a-150">継承</span><span class="sxs-lookup"><span data-stu-id="0cf9a-150">Inheritance</span></span>

<span data-ttu-id="0cf9a-151">継承リレーションシップは 1 種類のいずれか (、*派生*型) から別の派生 (、*基本*型)、派生型の宣言領域を暗黙的に含む、アクセスできるように、コンス トラクターではない型のメンバーとその基本型の入れ子にされた型。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-151">An inheritance relationship is one in which one type (the *derived* type) derives from another (the *base* type), such that the derived type's declaration space implicitly contains the accessible non-constructor type members and nested types of its base type.</span></span> <span data-ttu-id="0cf9a-152">次の例では、クラス`A`の基本クラスは、 `B`、および`B`から派生`A`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-152">In the following example, class `A` is the base class of `B`, and `B` is derived from `A`.</span></span>

```vb
Class A
End Class

Class B
    Inherits A
End Class
```

<span data-ttu-id="0cf9a-153">`A`は基底クラスを明示的に指定する、その基底クラスが暗黙的に`Object`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-153">Since `A` does not explicitly specify a base class, its base class is implicitly `Object`.</span></span>

<span data-ttu-id="0cf9a-154">以下は、継承の重要な側面です。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-154">The following are important aspects of inheritance:</span></span>

* <span data-ttu-id="0cf9a-155">継承は推移的です。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-155">Inheritance is transitive.</span></span> <span data-ttu-id="0cf9a-156">場合型*C*型から派生*B*、および種類*B*型から派生*A*、型*C*継承、入力の型で宣言されたメンバー *B*型で宣言された型のメンバーも*A*します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-156">If type *C* is derived from type *B*, and type *B* is derived from type *A*, type *C* inherits the type members declared in type *B* as well as the type members declared in type *A*.</span></span>

* <span data-ttu-id="0cf9a-157">派生型では、拡張しますが、絞り込むことはできません、その基本型。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-157">A derived type extends, but cannot narrow, its base type.</span></span> <span data-ttu-id="0cf9a-158">派生型が型の新しいメンバーを追加できますと継承した型のメンバーをシャドウすることができますが、継承した型のメンバーの定義を削除できません。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-158">A derived type can add new type members, and it can shadow inherited type members, but it cannot remove the definition of an inherited type member.</span></span>

* <span data-ttu-id="0cf9a-159">型のインスタンスには、その基本型の型のメンバーのすべてが含まれている、ためには、派生型から変換を常にその基本型が存在します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-159">Because an instance of a type contains all of the type members of its base type, a conversion always exists from a derived type to its base type.</span></span>

* <span data-ttu-id="0cf9a-160">すべての種類は、基本型を型を除くを設定する必要があります`Object`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-160">All types must have a base type, except for the type `Object`.</span></span> <span data-ttu-id="0cf9a-161">したがって、`Object`は、すべての型の究極の基本型ですし、すべての型に変換できます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-161">Thus, `Object` is the ultimate base type of all types, and all types can be converted to it.</span></span>

* <span data-ttu-id="0cf9a-162">派生での循環は許可されていません。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-162">Circularity in derivation is not permitted.</span></span> <span data-ttu-id="0cf9a-163">つまり、ときに、型`B`型から派生`A`、エラーの種類には`A`型から直接または間接的に派生する`B`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-163">That is, when a type `B` derives from a type `A`, it is an error for type `A` to derive directly or indirectly from type `B`.</span></span>

* <span data-ttu-id="0cf9a-164">型が直接または間接的にから派生していません内に入れ子にされた型。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-164">A type may not directly or indirectly derive from a type nested within it.</span></span>

<span data-ttu-id="0cf9a-165">次の例では、クラスは、互いに循環依存しているために、コンパイル時エラーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-165">The following example produces a compile-time error because the classes circularly depend on each other.</span></span>

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

<span data-ttu-id="0cf9a-166">次の例は、ためも、コンパイル時エラーを生成`B`、入れ子になったクラスから直接派生`C`クラスを通じて`A`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-166">The following example also produces a compile-time error because `B` indirectly derives from its nested class `C` through class `A`.</span></span>

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

<span data-ttu-id="0cf9a-167">に、次の例でエラーが発生しないクラス`A`クラスから派生していない`B`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-167">The next example does not produce an error because class `A` does not derive from class `B`.</span></span>

```vb
Class A
    Class B
        Inherits A
    End Class 
End Class
```

### <a name="mustinherit-and-notinheritable-classes"></a><span data-ttu-id="0cf9a-168">MustInherit と NotInheritable クラス</span><span class="sxs-lookup"><span data-stu-id="0cf9a-168">MustInherit and NotInheritable Classes</span></span>

<span data-ttu-id="0cf9a-169">A`MustInherit`クラスが基本型としてのみ機能する不完全な型。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-169">A `MustInherit` class is an incomplete type that can act only as a base type.</span></span> <span data-ttu-id="0cf9a-170">A`MustInherit`クラス、インスタンス化できないため、使用するとエラー、`New`演算子に対して 1 つ。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-170">A `MustInherit` class cannot be instantiated, so it is an error to use the `New` operator on one.</span></span> <span data-ttu-id="0cf9a-171">変数を宣言することは`MustInherit`クラスです。 このような変数を割り当てることできますのみ`Nothing`またはから派生したクラスのある値、`MustInherit`クラス。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-171">It is valid to declare variables of `MustInherit` classes; such variables can only be assigned `Nothing` or a value that is of a class derived from the `MustInherit` class.</span></span>

<span data-ttu-id="0cf9a-172">通常のクラスを派生する場合、`MustInherit`クラスでは、通常のクラスをオーバーライドする必要がありますすべて継承された`MustOverride`メンバー。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-172">When a regular class is derived from a `MustInherit` class, the regular class must override all inherited `MustOverride` members.</span></span> <span data-ttu-id="0cf9a-173">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-173">For example:</span></span>

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

<span data-ttu-id="0cf9a-174">`MustInherit`クラス`A`が導入されています、`MustOverride`メソッド`F`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-174">The `MustInherit` class `A` introduces a `MustOverride` method `F`.</span></span> <span data-ttu-id="0cf9a-175">クラス`B`その他の方法が導入されました`G`の実装は提供されません`F`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-175">Class `B` introduces an additional method `G`, but does not provide an implementation of `F`.</span></span> <span data-ttu-id="0cf9a-176">クラス`B`も宣言する必要がありますので`MustInherit`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-176">Class `B` must therefore also be declared `MustInherit`.</span></span> <span data-ttu-id="0cf9a-177">クラス`C`オーバーライド`F`し、実際の実装を提供します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-177">Class `C` overrides `F` and provides an actual implementation.</span></span> <span data-ttu-id="0cf9a-178">いいえ未処理があるので`MustOverride`クラスでメンバー `C`、する必要はありません`MustInherit`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-178">Since there are no outstanding `MustOverride` members in class `C`, it is not required to be `MustInherit`.</span></span>

<span data-ttu-id="0cf9a-179">A`NotInheritable`クラスは、クラスが別のクラスを派生できません。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-179">A `NotInheritable` class is a class from which another class cannot be derived.</span></span> <span data-ttu-id="0cf9a-180">`NotInheritable` クラスは、意図しない派生を防ぐために主に使用されます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-180">`NotInheritable` classes are primarily used to prevent unintended derivation.</span></span>

<span data-ttu-id="0cf9a-181">この例では、クラス`B`しようとするから派生するために、エラーでは、`NotInheritable`クラス`A`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-181">In this example, class `B` is in error because it attempts to derive from the `NotInheritable` class `A`.</span></span> <span data-ttu-id="0cf9a-182">両方のクラスをマークできません`MustInherit`と`NotInheritable`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-182">A class cannot be marked both `MustInherit` and `NotInheritable`.</span></span>

```vb
NotInheritable Class A
End Class

Class B
    ' Error, a class cannot derive from a NotInheritable class.
    Inherits A
End Class
```

### <a name="interfaces-and-multiple-inheritance"></a><span data-ttu-id="0cf9a-183">インターフェイスと多重継承</span><span class="sxs-lookup"><span data-stu-id="0cf9a-183">Interfaces and Multiple Inheritance</span></span>

<span data-ttu-id="0cf9a-184">単一の基本型からのみ派生するには、他の型とは異なり、インターフェイスは、複数の基底インターフェイスから派生可能性があります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-184">Unlike other types, which only derive from a single base type, an interface may derive from multiple base interfaces.</span></span> <span data-ttu-id="0cf9a-185">このため、インターフェイスは、別の基底インターフェイスから、同じ名前を持つ型のメンバーを継承できます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-185">Because of this, an interface can inherit an identically named type member from different base interfaces.</span></span> <span data-ttu-id="0cf9a-186">このような場合は、派生インターフェイスでは多重継承の名前は利用できず、派生インターフェイスを通じてこれらの型メンバーのいずれかを参照するによって署名やオーバー ロードに関係なく、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-186">In such a case, the multiply-inherited name is not available in the derived interface, and referring to any of those type members through the derived interface causes a compile-time error, regardless of signatures or overloading.</span></span> <span data-ttu-id="0cf9a-187">代わりに、競合する型のメンバーは、基底インターフェイス名を使用して参照する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-187">Instead, conflicting type members must be referenced through a base interface name.</span></span>

<span data-ttu-id="0cf9a-188">ため、次の例では、最初の 2 つのステートメントがコンパイル時エラーを発生多重継承メンバー`Count`インターフェイスでは利用できません`IListCounter`:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-188">In the following example, the first two statements cause compile-time errors because the multiply-inherited member `Count` is not available in interface `IListCounter`:</span></span>

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

<span data-ttu-id="0cf9a-189">キャストすることで、あいまいさが解決されるように、例に示すように、`x`基底インターフェイスの適切な型にします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-189">As illustrated by the example, the ambiguity is resolved by casting `x` to the appropriate base interface type.</span></span> <span data-ttu-id="0cf9a-190">このようなキャストが実行時のコストはあるありません。単なる階層の派生型としてコンパイル時に、インスタンスを表示するので構成されます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-190">Such casts have no run-time costs; they merely consist of viewing the instance as a less-derived type at compile time.</span></span>

<span data-ttu-id="0cf9a-191">1 つの型のメンバーが複数のパスを通じて同じ基底インターフェイスから継承されると、型のメンバーは 1 回のみ継承がいるかのように扱われます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-191">When a single type member is inherited from the same base interface through multiple paths, the type member is treated as if it were only inherited once.</span></span> <span data-ttu-id="0cf9a-192">つまり、派生インターフェイスには、特定の基底インターフェイスから継承された各型のメンバーの 1 つのインスタンスにはのみが含まれます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-192">In other words, the derived interface only contains one instance of each type member inherited from a particular base interface.</span></span> <span data-ttu-id="0cf9a-193">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-193">For example:</span></span>

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

<span data-ttu-id="0cf9a-194">型メンバーの名前が継承階層の 1 つのパスでシャドウされている場合は、すべてのパスに名前が影付き。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-194">If a type member name is shadowed in one path through the inheritance hierarchy, then the name is shadowed in all paths.</span></span> <span data-ttu-id="0cf9a-195">次の例では、`IBase.F`によってメンバーが影付き、`ILeft.F`のメンバーでシャドウはありませんが、 `IRight`:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-195">In the following example, the `IBase.F` member is shadowed by the `ILeft.F` member, but is not shadowed in `IRight`:</span></span>

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

<span data-ttu-id="0cf9a-196">呼び出し`d.F(1)`選択`ILeft.F`場合でも、`IBase.F`を通じて潜在顧客のアクセス パスでシャドウされませんが、`IRight`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-196">The invocation `d.F(1)` selects `ILeft.F`, even though `IBase.F` appears to not be shadowed in the access path that leads through `IRight`.</span></span> <span data-ttu-id="0cf9a-197">からのアクセス パス`IDerived`に`ILeft`に`IBase`shadows`IBase.F`からのアクセス パスで、メンバーが影付きも`IDerived`に`IRight`に`IBase`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-197">Because the access path from `IDerived` to `ILeft` to `IBase` shadows `IBase.F`, the member is also shadowed in the access path from `IDerived` to `IRight` to `IBase`.</span></span>

### <a name="shadowing"></a><span data-ttu-id="0cf9a-198">シャドウ</span><span class="sxs-lookup"><span data-stu-id="0cf9a-198">Shadowing</span></span>

<span data-ttu-id="0cf9a-199">派生型では、再宣言することにより、継承された型のメンバーの名前をシャドウします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-199">A derived type shadows the name of an inherited type member by re-declaring it.</span></span> <span data-ttu-id="0cf9a-200">名前をシャドウします。 その名前を持つ継承された型のメンバーは削除されません。単に、その名前を持つ継承された型のメンバーの派生クラスでは使用できません。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-200">Shadowing a name does not remove the inherited type members with that name; it merely makes all of the inherited type members with that name unavailable in the derived class.</span></span> <span data-ttu-id="0cf9a-201">シャドウの宣言には、任意の種類のエンティティの可能性があります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-201">The shadowing declaration may be any type of entity.</span></span>

<span data-ttu-id="0cf9a-202">オーバー ロード可能なエンティティには、シャドウの 2 つの形式のいずれかを選択できます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-202">Entities than can be overloaded can choose one of two forms of shadowing.</span></span> <span data-ttu-id="0cf9a-203">*名前によるシャドウ*を使用して指定は、`Shadows`キーワード。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-203">*Shadowing by name* is specified using the `Shadows` keyword.</span></span> <span data-ttu-id="0cf9a-204">名前でシャドウするエンティティ名ですべてのオーバー ロードを含む、基本クラスに非表示にします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-204">An entity that shadows by name hides everything by that name in the base class, including all overloads.</span></span> <span data-ttu-id="0cf9a-205">*名前とシグネチャによるシャドウ*を使用して指定は、`Overloads`キーワード。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-205">*Shadowing by name and signature* is specified using the `Overloads` keyword.</span></span> <span data-ttu-id="0cf9a-206">名前とシグネチャでシャドウするエンティティは、その名前のエンティティとして同じシグネチャを持つすべてを隠ぺいします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-206">An entity that shadows by name and signature hides everything by that name with the same signature as the entity.</span></span> <span data-ttu-id="0cf9a-207">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-207">For example:</span></span>

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

<span data-ttu-id="0cf9a-208">持つメソッドをシャドウする`ParamArray`個別の署名、展開された署名のないすべての可能な引数を名前とシグネチャを非表示にします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-208">Shadowing a method with a `ParamArray` argument by name and signature hides only the individual signature, not all possible expanded signatures.</span></span> <span data-ttu-id="0cf9a-209">これは、シャドウ メソッドのシグネチャが展開されていないシャドウされたメソッドのシグネチャと一致する場合でも当てはまります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-209">This is true even if the signature of the shadowing method matches the unexpanded signature of the shadowed method.</span></span> <span data-ttu-id="0cf9a-210">次の例では:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-210">The following example:</span></span>

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

<span data-ttu-id="0cf9a-211">印刷`Base`場合でも、`Derived.F`の展開されていない形式と同じシグネチャを持つ`Base.F`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-211">prints `Base`, even though `Derived.F` has the same signature as the unexpanded form of `Base.F`.</span></span>

<span data-ttu-id="0cf9a-212">逆に、使用してメソッドを`ParamArray`引数のみ可能なすべての拡張されたシグネチャ、同じシグネチャを持つメソッドをシャドウします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-212">Conversely, a method with a `ParamArray` argument only shadows methods with the same signature, not all possible expanded signatures.</span></span> <span data-ttu-id="0cf9a-213">次の例では:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-213">The following example:</span></span>

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

<span data-ttu-id="0cf9a-214">印刷`Base`場合でも、`Derived.F`として同じシグネチャを持つ拡張の形式が`Base.F`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-214">prints `Base`, even though `Derived.F` has an expanded form that has the same signature as `Base.F`.</span></span>

<span data-ttu-id="0cf9a-215">シャドウ メソッドまたはプロパティが指定されていない`Shadows`または`Overloads`前提としています`Overloads`メソッドまたはプロパティが宣言されている場合`Overrides`、`Shadows`それ以外の場合。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-215">A shadowing method or property that does not specify `Shadows` or `Overloads` assumes `Overloads` if the method or property is declared `Overrides`, `Shadows` otherwise.</span></span> <span data-ttu-id="0cf9a-216">一連のオーバー ロードされたエンティティの 1 つのメンバーを指定しているかどうか、`Shadows`または`Overloads`キーワードをすべて指定する必要あります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-216">If one member of a set of overloaded entities specifies the `Shadows` or `Overloads` keyword, they all must specify it.</span></span> <span data-ttu-id="0cf9a-217">`Shadows`と`Overloads`キーワードを同時に指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-217">The `Shadows` and `Overloads` keywords cannot be specified at the same time.</span></span> <span data-ttu-id="0cf9a-218">どちらも`Shadows`も`Overloads`標準モジュールで指定することができます。 標準的なモジュールのメンバーが暗黙的にから継承されたメンバーをシャドウ`Object`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-218">Neither `Shadows` nor `Overloads` can be specified in a standard module; members in a standard module implicitly shadow members inherited from `Object`.</span></span>

<span data-ttu-id="0cf9a-219">インターフェイスの継承によって多重継承された (および、それによって使用できない)、型のメンバーの名前をシャドウすることはそのため、名前使用できるようにする派生インターフェイスでします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-219">It is valid to shadow the name of a type member that has been multiply-inherited through interface inheritance (and which is thereby unavailable), thus making the name available in the derived interface.</span></span>

<span data-ttu-id="0cf9a-220">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-220">For example:</span></span>

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

<span data-ttu-id="0cf9a-221">シャドウの継承されたメソッドには、メソッドが許可されている、ため、クラスにいくつか含まれてはことが`Overridable`同じシグネチャを持つメソッド。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-221">Because methods are allowed to shadow inherited methods, it is possible for a class to contain several `Overridable` methods with the same signature.</span></span> <span data-ttu-id="0cf9a-222">これはありません、あいまいさの問題、最も多く派生されたメソッドのみが表示されるためです。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-222">This does not present an ambiguity problem, since only the most-derived method is visible.</span></span> <span data-ttu-id="0cf9a-223">次の例では、`C`と`D`クラスを含む 2 つ`Overridable`同じシグネチャを持つメソッド。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-223">In the following example, the `C` and `D` classes contain two `Overridable` methods with the same signature:</span></span>

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

<span data-ttu-id="0cf9a-224">2 つ`Overridable`メソッド: クラスによって導入された 1 つ`A`クラスによって導入された、1 つ`C`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-224">There are two `Overridable` methods here: one introduced by class `A` and the one introduced by class `C`.</span></span> <span data-ttu-id="0cf9a-225">クラスによって導入されたメソッド`C`クラスから継承されたメソッドを非表示に`A`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-225">The method introduced by class `C` hides the method inherited from class `A`.</span></span> <span data-ttu-id="0cf9a-226">つまり、`Overrides`クラスの宣言で`D`クラスによって導入されたメソッドをオーバーライド`C`、クラスのことはできませんと`D`クラスによって導入されたメソッドをオーバーライドする`A`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-226">Thus, the `Overrides` declaration in class `D` overrides the method introduced by class `C`, and it is not possible for class `D` to override the method introduced by class `A`.</span></span> <span data-ttu-id="0cf9a-227">この例では、出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-227">The example produces the output:</span></span>

```
B.F
B.F
D.F
D.F
```

<span data-ttu-id="0cf9a-228">非表示を起動することは`Overridable`クラスのインスタンスにアクセスしてメソッド`D`メソッドが非表示しない階層の派生型を使って。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-228">It is possible to invoke the hidden `Overridable` method by accessing an instance of class `D` through a less-derived type in which the method is not hidden.</span></span>

<span data-ttu-id="0cf9a-229">シャドウすることはできません、`MustOverride`メソッド、ほとんどの場合そうクラスは、使用できないためです。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-229">It is not valid to shadow a `MustOverride` method, because in most cases this would make the class unusable.</span></span> <span data-ttu-id="0cf9a-230">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-230">For example:</span></span>

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

<span data-ttu-id="0cf9a-231">ここでは、クラス`MoreDerived`をオーバーライドするために必要な`MustOverride`メソッド`Base.F`、だクラス`Derived`shadows `Base.F`、このことはできません。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-231">In this case, the class `MoreDerived` is required to override the `MustOverride` method `Base.F`, but because the class `Derived` shadows `Base.F`, this is not possible.</span></span> <span data-ttu-id="0cf9a-232">有効なを宣言する方法はありませんの子孫`Derived`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-232">There is no way to declare a valid descendent of `Derived`.</span></span>

<span data-ttu-id="0cf9a-233">外側のスコープから名前をシャドウ処理とは対照的継承したスコープからをアクセス可能な名前をシャドウすると、次の例のように、報告されることを警告が発生します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-233">In contrast to shadowing a name from an outer scope, shadowing an accessible name from an inherited scope causes a warning to be reported, as in the following example:</span></span>

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

<span data-ttu-id="0cf9a-234">メソッドの宣言`F`クラスで`Derived`により警告が報告されます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-234">The declaration of method `F` in class `Derived` causes a warning to be reported.</span></span> <span data-ttu-id="0cf9a-235">継承された名前をシャドウ具体的にはないエラーが、基底クラスの個別の進化を来たすためです。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-235">Shadowing an inherited name is specifically not an error, since that would preclude separate evolution of base classes.</span></span> <span data-ttu-id="0cf9a-236">たとえば、上記のような状況に発生していますのでクラスの以降のバージョン`Base`メソッドを導入`F`クラスの以前のバージョンにするはありません。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-236">For example, the above situation might have come about because a later version of class `Base` introduced a method `F` that was not present in an earlier version of the class.</span></span> <span data-ttu-id="0cf9a-237">上記のような状況で、エラーをされていた*任意*派生クラスを無効になりますが、個別にバージョン管理されたクラス ライブラリで基底クラスに加えられた変更可能性です。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-237">Had the above situation been an error, *any* change made to a base class in a separately versioned class library could potentially cause derived classes to become invalid.</span></span>

<span data-ttu-id="0cf9a-238">使用して、継承された名前をシャドウ処理に起因する警告を取り除くことができます、`Shadows`または`Overloads`修飾子。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-238">The warning caused by shadowing an inherited name can be eliminated through use of the `Shadows` or `Overloads` modifier:</span></span>

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

<span data-ttu-id="0cf9a-239">`Shadows`修飾子は、継承されたメンバーをシャドウすることを指定します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-239">The `Shadows` modifier indicates the intention to shadow the inherited member.</span></span> <span data-ttu-id="0cf9a-240">指定するとエラーには、`Shadows`または`Overloads`修飾子をシャドウする型のメンバー名がない場合。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-240">It is not an error to specify the `Shadows` or `Overloads` modifier if there is no type member name to shadow.</span></span>

<span data-ttu-id="0cf9a-241">新しいメンバーの宣言では、次の例のように、新しいメンバーのスコープ内でのみ、継承されたメンバーをシャドウします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-241">A declaration of a new member shadows an inherited member only within the scope of the new member, as in the following example:</span></span>

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

<span data-ttu-id="0cf9a-242">メソッドの宣言の前の例では`F`クラスで`Derived`メソッドをシャドウ`F`クラスから継承されている`Base`、以降、新しいメソッドが、`F`クラスで`Derived`が`Private`アクセス、そのスコープはクラスに拡張されません`MoreDerived`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-242">In the example above, the declaration of method `F` in class `Derived` shadows the method `F` that was inherited from class `Base`, but since the new method `F` in class `Derived` has `Private` access, its scope does not extend to class `MoreDerived`.</span></span> <span data-ttu-id="0cf9a-243">したがって、呼び出し`F()`で`MoreDerived.G`が有効では呼び出す`Base.F`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-243">Thus, the call `F()` in `MoreDerived.G` is valid and will invoke `Base.F`.</span></span> <span data-ttu-id="0cf9a-244">をオーバー ロードされた型のメンバーの場合は、オーバー ロードされた型のメンバーのセット全体はシャドウのための最も制限の少ないアクセス権を持ってこれらはすべてかのように扱われます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-244">In the case of overloaded type members, the entire set of overloaded type members is treated as if they all had the most permissive access for the purposes of shadowing.</span></span>

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

<span data-ttu-id="0cf9a-245">この例でもの宣言`F()`で`Derived`で宣言されて`Private`アクセス、オーバー ロードされた`F(Integer)`で宣言されて`Public`アクセスします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-245">In this example, even though the declaration of `F()` in `Derived` is declared with `Private` access, the overloaded `F(Integer)` is declared with `Public` access.</span></span> <span data-ttu-id="0cf9a-246">名前をシャドウするために、そのため、`F`で`Derived`だとして扱われます`Public`、ため両方のメソッドのシャドウ`F`で`Base`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-246">Therefore, for the purpose of shadowing, the name `F` in `Derived` is treated as if it was `Public`, so both methods shadow `F` in `Base`.</span></span>

## <a name="implementation"></a><span data-ttu-id="0cf9a-247">実装</span><span class="sxs-lookup"><span data-stu-id="0cf9a-247">Implementation</span></span>

<span data-ttu-id="0cf9a-248">*実装*インターフェイスを実装して、型がインターフェイスのすべての型のメンバーを実装する型で宣言されている場合、リレーションシップが存在します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-248">An *implementation* relationship exists when a type declares that it implements an interface and the type implements all the type members of the interface.</span></span> <span data-ttu-id="0cf9a-249">特定のインターフェイスを実装する型は、そのインターフェイスに変換できます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-249">A type that implements a particular interface is convertible to that interface.</span></span> <span data-ttu-id="0cf9a-250">インターフェイスをインスタンス化することはできませんが、インターフェイスの変数を宣言することはこのような変数はできるインターフェイスを実装するクラスのある値のみ割り当てること。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-250">Interfaces cannot be instantiated, but it is valid to declare variables of interfaces; such variables can only be assigned a value that is of a class that implements the interface.</span></span> <span data-ttu-id="0cf9a-251">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-251">For example:</span></span>

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

<span data-ttu-id="0cf9a-252">多重継承の種類のメンバーを持つインターフェイスを実装する型は、実装される派生インターフェイスから直接アクセスできない場合でも、これらのメソッドを実装もする必要があります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-252">A type implementing an interface with multiply-inherited type members must still implement those methods, even though they cannot be accessed directly from the derived interface being implemented.</span></span> <span data-ttu-id="0cf9a-253">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-253">For example:</span></span>

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

<span data-ttu-id="0cf9a-254">でも`MustInherit`クラスで実装されたインターフェイスのすべてのメンバーの実装を提供する必要があります。 これらのメソッドの実装を延期として宣言することで、ただし、`MustOverride`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-254">Even `MustInherit` classes must provide implementations of all the members of implemented interfaces; however, they can defer implementation of these methods by declaring them as `MustOverride`.</span></span> <span data-ttu-id="0cf9a-255">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-255">For example:</span></span>

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

<span data-ttu-id="0cf9a-256">型は、その基本型を実装するインターフェイスを再実装するために選択できます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-256">A type may choose to re-implement an interface that its base type implements.</span></span> <span data-ttu-id="0cf9a-257">インターフェイスを再実装するには、型する必要があります明示的にインターフェイスを実装すること。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-257">To re-implement the interface, the type must explicitly state that it implements the interface.</span></span> <span data-ttu-id="0cf9a-258">インターフェイスを再実装する型が一部のみを再実装を選択できますがない再実装されるメンバー - インターフェイスのメンバーの基本型の実装を使用します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-258">A type re-implementing an interface may choose to re-implement only some, but not all, of the members of the interface -- any members not re-implemented continue to use the base type's implementation.</span></span> <span data-ttu-id="0cf9a-259">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-259">For example:</span></span>

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

<span data-ttu-id="0cf9a-260">この例が出力されます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-260">This example prints:</span></span>

```
TestDerived.DerivedTest1
TestBase.Test2
```

<span data-ttu-id="0cf9a-261">派生型では、基本インターフェイスが派生型の基本型によって実装されるインターフェイスを実装するときにのみ、基本型で既に実装されていないインターフェイスの型のメンバーを実装する派生型を選択できます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-261">When a derived type implements an interface whose base interfaces are implemented by the derived type's base types, the derived type can choose to only implement the interface's type members that are not already implemented by the base types.</span></span> <span data-ttu-id="0cf9a-262">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-262">For example:</span></span>

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

<span data-ttu-id="0cf9a-263">基本データ型のオーバーライド可能なメソッドを使用してインターフェイス メソッドを実装することもできます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-263">An interface method can also be implemented using an overridable method in a base type.</span></span> <span data-ttu-id="0cf9a-264">その場合は、派生型がオーバーライド可能なメソッドも、およびインターフェイスの実装を変更する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-264">In that case, a derived type may also override the overridable method and alter the implementation of the interface.</span></span> <span data-ttu-id="0cf9a-265">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-265">For example:</span></span>

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

### <a name="implementing-methods"></a><span data-ttu-id="0cf9a-266">メソッドを実装します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-266">Implementing Methods</span></span>

<span data-ttu-id="0cf9a-267">型*実装*メソッドを指定することによって実装されたインターフェイスの型のメンバー、`Implements`句。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-267">A type *implements* a type member of an implemented interface by supplying a method with an `Implements` clause.</span></span> <span data-ttu-id="0cf9a-268">2 つの型のメンバーは同じ数のパラメーターが必要、すべての型およびパラメーターの修飾子が一致している省略可能なパラメーターの既定値を含む、戻り値の型が一致する必要があります、およびすべてのメソッド パラメーターに対する制約と一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-268">The two type members must have the same number of parameters, all of the types and modifiers of the parameters must match, including the default value of optional parameters, the return type must match, and all of the constraints on method parameters must match.</span></span> <span data-ttu-id="0cf9a-269">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-269">For example:</span></span>

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

<span data-ttu-id="0cf9a-270">1 つのメソッドは、上記の条件を満たす場合、任意の数のインターフェイス型のメンバーを実装できます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-270">A single method may implement any number of interface type members if they all meet the above criteria.</span></span> <span data-ttu-id="0cf9a-271">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-271">For example:</span></span>

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

<span data-ttu-id="0cf9a-272">メソッドをジェネリック インターフェイスを実装する場合、メソッドの実装は、インターフェイスの型パラメーターに対応する型の引数を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-272">When implementing a method in a generic interface, the implementing method must supply the type arguments that correspond to the interface's type parameters.</span></span> <span data-ttu-id="0cf9a-273">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-273">For example:</span></span>

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

<span data-ttu-id="0cf9a-274">これは、ジェネリック インターフェイスがいくつかの一連の型引数に実装可能ないない可能性がありますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-274">Note that it is possible that a generic interface may not be implementable for some set of type arguments.</span></span>

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

## <a name="polymorphism"></a><span data-ttu-id="0cf9a-275">ポリモーフィズム</span><span class="sxs-lookup"><span data-stu-id="0cf9a-275">Polymorphism</span></span>

<span data-ttu-id="0cf9a-276">*ポリモーフィズム*メソッドまたはプロパティの実装を変更する機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-276">*Polymorphism* provides the ability to vary the implementation of a method or property.</span></span> <span data-ttu-id="0cf9a-277">ポリモーフィズムと同じメソッドまたはプロパティを呼び出すインスタンスの実行時の型に応じて異なるアクションを実行できます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-277">With polymorphism, the same method or property can perform different actions depending on the run-time type of the instance that invokes it.</span></span> <span data-ttu-id="0cf9a-278">メソッドまたはポリモーフィックなプロパティが呼び出されます*オーバーライド可能な*します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-278">Methods or properties that are polymorphic are called *overridable*.</span></span> <span data-ttu-id="0cf9a-279">これに対し、オーバーライドできないメソッドまたはプロパティの実装が不変です。メソッドまたはプロパティがクラスのインスタンスで呼び出されるかどうか、実装は同じ宣言されているか、派生クラスのインスタンス。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-279">By contrast, the implementation of a non-overridable method or property is invariant; the implementation is the same whether the method or property is invoked on an instance of the class in which it is declared or an instance of a derived class.</span></span> <span data-ttu-id="0cf9a-280">オーバーライドできないメソッドまたはプロパティが呼び出されると、インスタンスのコンパイル時の型が決定要因にします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-280">When a non-overridable method or property is invoked, the compile-time type of the instance is the determining factor.</span></span> <span data-ttu-id="0cf9a-281">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-281">For example:</span></span>

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

<span data-ttu-id="0cf9a-282">オーバーライド可能なメソッドがありますも`MustOverride`、つまり、メソッド本体を提供しないとオーバーライドする必要があります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-282">An overridable method may also be `MustOverride`, which means that it provides no method body and must be overridden.</span></span> <span data-ttu-id="0cf9a-283">`MustOverride` メソッドでのみ許可`MustInherit`クラス。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-283">`MustOverride` methods are only allowed in `MustInherit` classes.</span></span>

<span data-ttu-id="0cf9a-284">次の例では、クラスで`Shape`自体を描画できる幾何学的な図形オブジェクトの抽象概念を定義します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-284">In the following example, the class `Shape` defines the abstract notion of a geometrical shape object that can paint itself:</span></span>

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

<span data-ttu-id="0cf9a-285">`Paint`メソッドは`MustOverride`意味のある既定の実装がないためです。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-285">The `Paint` method is `MustOverride` because there is no meaningful default implementation.</span></span> <span data-ttu-id="0cf9a-286">`Ellipse`と`Box`クラスは具象`Shape`実装します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-286">The `Ellipse` and `Box` classes are concrete `Shape` implementations.</span></span> <span data-ttu-id="0cf9a-287">これらのクラスがないため、 `MustInherit`、オーバーライドする必要が、`Paint`メソッドと、実際の実装を提供します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-287">Because these classes are not `MustInherit`, they are required to override the `Paint` method and provide an actual implementation.</span></span>

<span data-ttu-id="0cf9a-288">エラーを参照するベースのアクセスには、`MustOverride`メソッドでは、次の例として示します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-288">It is an error for a base access to reference a `MustOverride` method, as the following example demonstrates:</span></span>

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

<span data-ttu-id="0cf9a-289">エラーは、`MyBase.F()`呼び出し参照しているため、`MustOverride`メソッド。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-289">An error is reported for the `MyBase.F()` invocation because it references a `MustOverride` method.</span></span>

### <a name="overriding-methods"></a><span data-ttu-id="0cf9a-290">メソッドのオーバーライド</span><span class="sxs-lookup"><span data-stu-id="0cf9a-290">Overriding Methods</span></span>

<span data-ttu-id="0cf9a-291">型が*オーバーライド*、継承されたオーバーライド可能なメソッドを使用して宣言をマークすること、同じ名前と、シグネチャを持つメソッドを宣言することによって、`Overrides`修飾子。以下に示すメソッドのオーバーライドの追加要件があります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-291">A type may *override* an inherited overridable method by declaring a method with the same name and , signature, and marking the declaration with the `Overrides` modifier.There are additional requirements on overriding methods, listed below.</span></span> <span data-ttu-id="0cf9a-292">一方、`Overridable`メソッドの宣言には、新しいメソッドが導入されています、`Overrides`メソッドの宣言には、継承されたメソッドの実装が置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-292">Whereas an `Overridable` method declaration introduces a new method, an `Overrides` method declaration replaces the inherited implementation of the method.</span></span>

<span data-ttu-id="0cf9a-293">オーバーライドするメソッドを宣言することも`NotOverridable`、派生型でメソッドのさらにオーバーライドを防ぐことができます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-293">An overriding method may be declared `NotOverridable`, which prevents any further overriding of the method in derived types.</span></span> <span data-ttu-id="0cf9a-294">実際には、`NotOverridable`派生クラスのメソッドのオーバーライドをこれ以上になります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-294">In effect, `NotOverridable` methods become non-overridable in any further derived classes.</span></span>

<span data-ttu-id="0cf9a-295">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-295">Consider the following example:</span></span>

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

<span data-ttu-id="0cf9a-296">クラスの例では、`B`は、2`Overrides`メソッド: メソッド`F`を持つ、`NotOverridable`修飾子とメソッド`G`をしません。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-296">In the example, class `B` provides two `Overrides` methods: a method `F` that has the `NotOverridable` modifier and a method `G` that does not.</span></span> <span data-ttu-id="0cf9a-297">使用、`NotOverridable`修飾子がクラス`C`からさらにメソッドをオーバーライドする`F`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-297">Use of the `NotOverridable` modifier prevents class `C` from further overriding method `F`.</span></span>

<span data-ttu-id="0cf9a-298">オーバーライドするメソッドを宣言することも`MustOverride`はオーバーライドするメソッドが宣言されていない場合でも、`MustOverride`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-298">An overriding method may also be declared `MustOverride`, even if the method that it is overriding is not declared `MustOverride`.</span></span> <span data-ttu-id="0cf9a-299">外側のクラスを宣言する必要があります`MustInherit`さらに派生した任意のクラスが宣言されていないと`MustInherit`メソッドをオーバーライドする必要があります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-299">This requires that the containing class be declared `MustInherit` and that any further derived classes that are not declared `MustInherit` must override the method.</span></span> <span data-ttu-id="0cf9a-300">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-300">For example:</span></span>

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

<span data-ttu-id="0cf9a-301">クラスの例では、`B`オーバーライド`A.F`で、`MustOverride`メソッド。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-301">In the example, class `B` overrides `A.F` with a `MustOverride` method.</span></span> <span data-ttu-id="0cf9a-302">派生するクラス、つまり`B`をオーバーライドする必要があります`F`宣言されている場合を除き、`MustInherit`もします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-302">This means that any classes derived from `B` will have to override `F`, unless they are declared `MustInherit` as well.</span></span>

<span data-ttu-id="0cf9a-303">コンパイル時エラーは、次のすべてがオーバーライドするメソッドの場合は true でない場合に発生します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-303">A compile-time error occurs unless all of the following are true of an overriding method:</span></span>

* <span data-ttu-id="0cf9a-304">宣言コンテキストには、オーバーライドするメソッドとしてアクセス可能な継承されたメソッドと同じシグネチャと戻り値の型 (存在する場合) が含まれています。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-304">The declaration context contains a single accessible inherited method with the same signature and return type (if any) as the overriding method.</span></span>
* <span data-ttu-id="0cf9a-305">無効にされている継承されたメソッドはオーバーライド可能です。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-305">The inherited method being overridden is overridable.</span></span> <span data-ttu-id="0cf9a-306">つまり、オーバーライドされている継承されたメソッドが`Shared`または`NotOverridable`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-306">In other words, the inherited method being overridden is not `Shared` or `NotOverridable`.</span></span>
* <span data-ttu-id="0cf9a-307">宣言されているメソッドのアクセシビリティ ドメインは、無効にされている継承されたメソッドのアクセシビリティ ドメインと同じです。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-307">The accessibility domain of the method being declared is the same as the accessibility domain of the inherited method being overridden.</span></span> <span data-ttu-id="0cf9a-308">1 つの例外:`Protected Friend`でメソッドをオーバーライドする必要があります、`Protected`メソッドの場合、もう一方のメソッドをオーバーライドするメソッドがない別のアセンブリ内`Friend`へのアクセスします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-308">There is one exception: a `Protected Friend` method must be overridden by a `Protected` method if the other method is in another assembly that the overriding method does not have `Friend` access to.</span></span>
* <span data-ttu-id="0cf9a-309">オーバーライドするメソッドのパラメーターに一致の使用状況において、オーバーライドされたメソッドのパラメーター、 `ByVal`、 `ByRef`、`ParamArray,`と`Optional`省略可能なパラメーターの値を含む修飾子。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-309">The parameters of the overriding method match the overridden method's parameters in regards to usage of the `ByVal`, `ByRef`, `ParamArray,` and `Optional` modifiers, including the values provided for optional parameters.</span></span>
* <span data-ttu-id="0cf9a-310">オーバーライド元のメソッドの型パラメーターでは、制約の型において、オーバーライドされたメソッドの型パラメーターと一致します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-310">The type parameters of the overriding method match the overridden method's type parameters in regards to type constraints.</span></span>

<span data-ttu-id="0cf9a-311">基本のジェネリック型のメソッドをオーバーライドするときにオーバーライドするメソッドは基本データ型のパラメーターに対応する型引数を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-311">When overriding a method in a base generic type, the overriding method must supply the type arguments that correspond to the base type parameters.</span></span> <span data-ttu-id="0cf9a-312">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-312">For example:</span></span>

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

<span data-ttu-id="0cf9a-313">これは、ジェネリック クラスでオーバーライド可能なメソッドが型引数のいくつかの設定をオーバーライドすることがない可能性がありますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-313">Note that it is possible that an overridable method in a generic class may not be able to be overridden for some sets of type arguments.</span></span> <span data-ttu-id="0cf9a-314">メソッドが宣言されている場合`MustOverride`、つまり、あるいくつかの継承チェーンが使用できない場合があります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-314">If the method is declared `MustOverride`, this means that some inheritance chains may not be possible.</span></span> <span data-ttu-id="0cf9a-315">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-315">For example:</span></span>

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

<span data-ttu-id="0cf9a-316">上書きの宣言は、次の例のように、基本のアクセスを使用して、オーバーライドされる基本メソッドにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-316">An override declaration can access the overridden base method using a base access, as in the following example:</span></span>

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

呼び出しの例で`MyBase.PrintVariables()`クラスで`Derived`呼び出す、`PrintVariables`メソッドがクラスで宣言された`Base`します。 基本アクセスでは、オーバーライド可能な呼び出しの機構を無効にし、単に基本のメソッドをオーバーライドできないメソッドとして扱われます。 <span data-ttu-id="0cf9a-319">呼び出す必要がある`Derived`されて書き込まれる`CType(Me, Base).PrintVariables()`、再帰的に存在するかを呼び出す、`PrintVariables`メソッドで宣言`Derived`で宣言されている 1 つではない`Base`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-319">Had the invocation in `Derived` been written `CType(Me, Base).PrintVariables()`, it would recursively invoke the `PrintVariables` method declared in `Derived`, not the one declared in `Base`.</span></span>

<span data-ttu-id="0cf9a-320">のみが含まれる場合、`Overrides`修飾子は、メソッドは、別のメソッドをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-320">Only when it includes an `Overrides` modifier can a method override another method.</span></span> <span data-ttu-id="0cf9a-321">その他のすべてのケースでは、継承されたメソッドとして同じシグネチャを持つメソッドは単に次の例のように、継承されたメソッドをシャドウします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-321">In all other cases, a method with the same signature as an inherited method simply shadows the inherited method, as in the example below:</span></span>

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

<span data-ttu-id="0cf9a-322">メソッドの例で`F`クラスで`Derived`は含まれません、`Overrides`修飾子とメソッドをオーバーライドしません`F`クラスで`Base`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-322">In the example, the method `F` in class `Derived` does not include an `Overrides` modifier and therefore does not override method `F` in class `Base`.</span></span> <span data-ttu-id="0cf9a-323">メソッドではなく、`F`クラスで`Derived`クラスのメソッドをシャドウ`Base`、宣言が含まれていないために、警告が報告されると、`Shadows`または`Overloads`修飾子。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-323">Rather, method `F` in class `Derived` shadows the method in class `Base`, and a warning is reported because the declaration does not include a `Shadows` or `Overloads` modifier.</span></span>

<span data-ttu-id="0cf9a-324">次の例では、メソッドで`F`クラスで`Derived`オーバーライド可能なメソッドをシャドウ`F`クラスから継承された`Base`:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-324">In the following example, method `F` in class `Derived` shadows the overridable method `F` inherited from class `Base`:</span></span>

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

<span data-ttu-id="0cf9a-325">新しいメソッドから`F`クラスで`Derived`が`Private`アクセスにはのみが含まれますのクラス本体にはスコープには`Derived`クラスには拡張されず`MoreDerived`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-325">Since the new method `F` in class `Derived` has `Private` access, its scope only includes the class body of `Derived` and does not extend to class `MoreDerived`.</span></span> <span data-ttu-id="0cf9a-326">メソッドの宣言`F`クラスで`MoreDerived`メソッドをオーバーライドするために許可されて`F`クラスから継承された`Base`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-326">The declaration of method `F` in class `MoreDerived` is therefore permitted to override the method `F` inherited from class `Base`.</span></span>

<span data-ttu-id="0cf9a-327">ときに、`Overridable`メソッドが呼び出される場合、基底クラスまたは派生クラスでメソッド呼び出しではかどうかに関係なく、インスタンスの型に基づいて、インスタンス メソッドの最多派生実装が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-327">When an `Overridable` method is invoked, the most derived implementation of the instance method is called, based on the type of the instance, regardless of whether the call is to the method in the base class or the derived class.</span></span> <span data-ttu-id="0cf9a-328">最派生実装、`Overridable`メソッド`M`クラスに関して`R`は次のように決定されます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-328">The most derived implementation of an `Overridable` method `M` with respect to a class `R` is determined as follows:</span></span>

* <span data-ttu-id="0cf9a-329">場合`R`、導入が含まれています。`Overridable`の宣言`M`、これは、最派生実装の`M`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-329">If `R` contains the introducing `Overridable` declaration of `M`, this is the most derived implementation of `M`.</span></span>

* <span data-ttu-id="0cf9a-330">の場合`R`のオーバーライドを含む`M`、これは、最派生実装の`M`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-330">Otherwise, if `R` contains an override of `M`, this is the most derived implementation of `M`.</span></span>

* <span data-ttu-id="0cf9a-331">それ以外の場合の実装の派生を最大限に`M`との直接基底クラスの同じ`R`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-331">Otherwise, the most derived implementation of `M` is the same as that of the direct base class of `R`.</span></span>

## <a name="accessibility"></a><span data-ttu-id="0cf9a-332">ユーザー補助</span><span class="sxs-lookup"><span data-stu-id="0cf9a-332">Accessibility</span></span>

<span data-ttu-id="0cf9a-333">宣言を指定します、*アクセシビリティ*のエンティティを宣言します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-333">A declaration specifies the *accessibility* of the entity it declares.</span></span> <span data-ttu-id="0cf9a-334">エンティティのユーザー補助機能では、エンティティの名前のスコープは変更されません。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-334">An entity's accessibility does not change the scope of an entity's name.</span></span> <span data-ttu-id="0cf9a-335">*アクセシビリティ ドメイン*の宣言をすべての宣言空間の宣言されたエンティティがアクセス可能なセット。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-335">The *accessibility domain* of a declaration is the set of all declaration spaces in which the declared entity is accessible.</span></span>

<span data-ttu-id="0cf9a-336">5 つのアクセスの種類は`Public`、 `Protected`、 `Friend`、 `Protected Friend`、および`Private`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-336">The five access types are `Public`, `Protected`, `Friend`, `Protected Friend`, and `Private`.</span></span> <span data-ttu-id="0cf9a-337">`Public` 最も制限の少ないアクセスの種類とその他の 4 つの型はすべてのサブセットの`Public`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-337">`Public` is the most permissive access type, and the four other types are all subsets of `Public`.</span></span> <span data-ttu-id="0cf9a-338">最も制限の多いアクセスの種類は`Private`、およびその他の 4 つのアクセス型はのすべてのスーパセットです`Private`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-338">The least permissive access type is `Private`, and the four other access types are all supersets of `Private`.</span></span>

```antlr
AccessModifier
    : 'Public'
    | 'Protected'
    | 'Friend'
    | 'Private'
    | 'Protected' 'Friend'
    ;
```

<span data-ttu-id="0cf9a-339">可能性がある、省略可能なアクセス修飾子で宣言用のアクセスの種類を指定した`Public`、 `Protected`、 `Friend`、 `Private`、または組み合わせた`Protected`と`Friend`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-339">The access type for a declaration is specified via an optional access modifier, which can be `Public`, `Protected`, `Friend`, `Private`, or the combination of `Protected` and `Friend`.</span></span> <span data-ttu-id="0cf9a-340">既定のアクセスの種類によって異なります、宣言のコンテキストへのアクセス修飾子が指定されていない場合許可されているアクセスの種類は、宣言コンテキストにも依存します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-340">If no access modifier is specified, the default access type depends on the declaration context; the permitted access types also depend on the declaration context.</span></span>

* <span data-ttu-id="0cf9a-341">宣言されたエンティティ、`Public`修飾子が`Public`アクセスします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-341">Entities declared with the `Public` modifier have `Public` access.</span></span> <span data-ttu-id="0cf9a-342">使用に関する制約がない`Public`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-342">There are no restrictions on the use of `Public` entities.</span></span>

* <span data-ttu-id="0cf9a-343">宣言されたエンティティ、`Protected`修飾子が`Protected`アクセスします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-343">Entities declared with the `Protected` modifier have `Protected` access.</span></span> <span data-ttu-id="0cf9a-344">`Protected` アクセスは、クラス (正規型のメンバーと入れ子になったクラスの両方) のまたはのメンバーでのみ指定できます`Overridable`標準的なモジュールと構造体のメンバー (から継承する必要があります、定義上、`System.Object`または`System.ValueType`)。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-344">`Protected` access can only be specified on members of classes (both regular type members and nested classes) or on `Overridable` members of standard modules and structures (which must, by definition, be inherited from `System.Object` or `System.ValueType`).</span></span> <span data-ttu-id="0cf9a-345">A`Protected`メンバーは、そのメンバーは、インスタンス メンバーはありません。 または、派生クラスのインスタンスを通して、アクセスが行わ、派生クラスにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-345">A `Protected` member is accessible to a derived class, provided that either the member is not an instance member, or the access takes place through an instance of the derived class.</span></span> <span data-ttu-id="0cf9a-346">`Protected` アクセスのスーパー セットでない`Friend`アクセスします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-346">`Protected` access is not a superset of `Friend` access.</span></span>

* <span data-ttu-id="0cf9a-347">宣言されたエンティティ、`Friend`修飾子が`Friend`アクセスします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-347">Entities declared with the `Friend` modifier have `Friend` access.</span></span> <span data-ttu-id="0cf9a-348">持つエンティティ`Friend`アクセスは、エンティティ宣言または指定されているすべてのアセンブリを含むプログラム内でのみアクセス可能な`Friend`経由でアクセス、`System.Runtime.CompilerServices.InternalsVisibleToAttribute`属性。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-348">An entity with `Friend` access is accessible only within the program that contains the entity declaration or any assemblies that have been given `Friend` access through the `System.Runtime.CompilerServices.InternalsVisibleToAttribute` attribute.</span></span>

* <span data-ttu-id="0cf9a-349">宣言されたエンティティ、`Protected Friend`修飾子の和集合がある`Protected`と`Friend`アクセスします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-349">Entities declared with the `Protected Friend` modifiers have the union of `Protected` and `Friend` access.</span></span>

* <span data-ttu-id="0cf9a-350">宣言されたエンティティ、`Private`修飾子が`Private`アクセスします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-350">Entities declared with the `Private` modifier have `Private` access.</span></span> <span data-ttu-id="0cf9a-351">A`Private`エンティティは、入れ子になったエンティティを含め、その宣言コンテキスト内でのみアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-351">A `Private` entity is accessible only within its declaration context, including any nested entities.</span></span>

<span data-ttu-id="0cf9a-352">宣言内のアクセシビリティは宣言コンテキストのアクセシビリティに依存しません。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-352">The accessibility in a declaration does not depend on the accessibility of the declaration context.</span></span> <span data-ttu-id="0cf9a-353">宣言された型の例では、`Private`へのアクセスを持つ型のメンバーを含めることができます`Public`アクセスします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-353">For example, a type declared with `Private` access may contain a type member with `Public` access.</span></span>

<span data-ttu-id="0cf9a-354">次のコードでは、さまざまなアクセシビリティ ドメインを示します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-354">The following code demonstrates various accessibility domains:</span></span>

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

<span data-ttu-id="0cf9a-355">クラスとメンバーがこの例では、次のアクセシビリティ ドメインがあります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-355">The classes and members in this example have the following accessibility domains:</span></span>

* <span data-ttu-id="0cf9a-356">アクセシビリティ ドメイン`A`と`A.X`に制限はありません。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-356">The accessibility domain of `A` and `A.X` is unlimited.</span></span>

* <span data-ttu-id="0cf9a-357">アクセシビリティ ドメイン`A.Y`、 `B`、 `B.X`、 `B.Y`、 `B.C`、 `B.C.X`、および`B.C.Y`は含んでいるプログラムです。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-357">The accessibility domain of `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`, and `B.C.Y` is the containing program.</span></span>

* <span data-ttu-id="0cf9a-358">アクセシビリティ ドメイン`A.Z`は `A.`</span><span class="sxs-lookup"><span data-stu-id="0cf9a-358">The accessibility domain of `A.Z` is `A.`</span></span>

* <span data-ttu-id="0cf9a-359">アクセシビリティ ドメイン`B.Z`、 `B.D`、 `B.D.X`、および`B.D.Y`は`B`など、`B.C`と`B.D`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-359">The accessibility domain of `B.Z`, `B.D`, `B.D.X`, and `B.D.Y` is `B`, including `B.C` and `B.D`.</span></span>

* <span data-ttu-id="0cf9a-360">アクセシビリティ ドメイン`B.C.Z`は`B.C`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-360">The accessibility domain of `B.C.Z` is `B.C`.</span></span>

* <span data-ttu-id="0cf9a-361">アクセシビリティ ドメイン`B.D.Z`は`B.D`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-361">The accessibility domain of `B.D.Z` is `B.D`.</span></span>

<span data-ttu-id="0cf9a-362">例に示すように、メンバーのアクセシビリティ ドメインを含んでいる型よりも大きいことはありません。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-362">As the example illustrates, the accessibility domain of a member is never larger than that of a containing type.</span></span> <span data-ttu-id="0cf9a-363">たとえば、場合でもすべて`X`メンバーが`Public`宣言されたアクセシビリティ、以外のすべて`A.X`アクセシビリティ ドメインは、それを含む型によって制限されます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-363">For example, even though all `X` members have `Public` declared accessibility, all but `A.X` have accessibility domains that are constrained by a containing type.</span></span>

<span data-ttu-id="0cf9a-364">アクセスを`Protected`がプロテクト メンバーのインスタンスの関係のない型は互いにアクセスできないように、派生型のインスタンスでメンバーがある必要があります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-364">Access to `Protected` instance members must be through an instance of the derived type so that unrelated types cannot gain access to each other's protected members.</span></span> <span data-ttu-id="0cf9a-365">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-365">For example:</span></span>

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

<span data-ttu-id="0cf9a-366">上記の例では、クラスで`Guest`だけに、保護されたアクセス`Password`フィールドのインスタンスで修飾される場合`Guest`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-366">In the above example, the class `Guest` only has access to the protected `Password` field if it is qualified with an instance of `Guest`.</span></span> <span data-ttu-id="0cf9a-367">これにより、`Guest`へアクセスできない、`Password`のフィールド、`Employee`オブジェクトにキャストするだけで`User`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-367">This prevents `Guest` from gaining access to the `Password` field of an `Employee` object simply by casting it to `User`.</span></span>

<span data-ttu-id="0cf9a-368">目的で`Protected`メンバー アクセス宣言のコンテキストにはで、ジェネリック型には型パラメーターが含まれています。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-368">For the purposes of `Protected` member access in generic types, the declaration context includes type parameters.</span></span> <span data-ttu-id="0cf9a-369">つまり、型引数の 1 つのセットを持つ派生型にへのアクセスがないこと、`Protected`異なる一連の型引数を持つ派生型のメンバー。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-369">This means that a derived type with one set of type arguments does not have access to the `Protected` members of a derived type with a different set of type arguments.</span></span> <span data-ttu-id="0cf9a-370">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-370">For example:</span></span>

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

<span data-ttu-id="0cf9a-371">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="0cf9a-371">__Note.__</span></span> <span data-ttu-id="0cf9a-372">C# 言語 (と可能性があるその他の言語) にアクセスするジェネリック型は、`Protected`にかかわらず、どのような型引数が指定されたメンバー。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-372">The C# language (and possibly other languages) allows a generic type to access `Protected` members regardless of what type arguments are supplied.</span></span> <span data-ttu-id="0cf9a-373">これを含むジェネリック クラスを設計する際に注意してくださいで維持するか`Protected`メンバー。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-373">This should be kept in mind when designing generic classes that contain `Protected` members.</span></span>


### <a name="constituent-types"></a><span data-ttu-id="0cf9a-374">構成要素型</span><span class="sxs-lookup"><span data-stu-id="0cf9a-374">Constituent Types</span></span>

<span data-ttu-id="0cf9a-375">*構成要素型*の宣言は、宣言によって参照される型。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-375">The *constituent types* of a declaration are the types that are referenced by the declaration.</span></span> <span data-ttu-id="0cf9a-376">たとえば、定数、メソッドの戻り値の型コンス トラクターのパラメーターの型の型は、すべての構成要素の型です。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-376">For example, the type of a constant, the return type of a method and the parameter types of a constructor are all constituent types.</span></span> <span data-ttu-id="0cf9a-377">宣言の構成要素の型のアクセシビリティ ドメインは、宣言自体のアクセシビリティ ドメインのスーパー セットであるかと同じである必要があります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-377">The accessibility domain of a constituent type of a declaration must be the same as or a superset of the accessibility domain of the declaration itself.</span></span> <span data-ttu-id="0cf9a-378">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-378">For example:</span></span>

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

## <a name="type-and-namespace-names"></a><span data-ttu-id="0cf9a-379">型と Namespace 名</span><span class="sxs-lookup"><span data-stu-id="0cf9a-379">Type and Namespace Names</span></span>

<span data-ttu-id="0cf9a-380">多くの言語構成要素は、名前空間または型指定が必要です。これらは、名前空間または型の名前の修飾形式を使用して指定できます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-380">Many language constructs require a namespace or type to be specified; these can be specified by using a qualified form of the namespace or type's name.</span></span> <span data-ttu-id="0cf9a-381">A*修飾名*から成るピリオドで区切られた一連の識別子。 期間の左側にある、識別子によって指定された宣言領域の右側にある期間の識別子を解決します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-381">A *qualified name* consists of a series of identifiers separated by periods; the identifier on the right side of a period is resolved in the declaration space specified by the identifier on the left side of the period.</span></span>

<span data-ttu-id="0cf9a-382">*完全修飾名*名前空間または型のすべての名前を含む修飾名を含む名前空間と型。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-382">The *fully qualified name* of a namespace or type is a qualified name that contains the name of all containing namespaces and types.</span></span> <span data-ttu-id="0cf9a-383">名前空間または型の完全修飾名は、つまり、`N.T`ここで、`T`エンティティの名前を指定し、`N`はそれを含むエンティティの完全修飾名です。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-383">In other words, the fully qualified name of a namespace or type is `N.T`, where `T` is the name of the entity and `N` is the fully qualified name of its containing entity.</span></span>

<span data-ttu-id="0cf9a-384">次の例では、行のコメントで、関連付けられている完全修飾名と名前空間と型の宣言がいくつかを示します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-384">The example below shows several namespace and type declarations together with their associated fully qualified names in in-line comments.</span></span>

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

<span data-ttu-id="0cf9a-385">名前空間 X.Y は、ソース コード内の 2 つの異なる場所で宣言されていますが、これら 2 つの部分宣言がクラス D および E. クラスの両方を含む X.Y と呼ばれる単一の名前空間だけを構成することを確認します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-385">Observe that the namespace X.Y has been declared in two different locations in the source code, but these two partial declarations constitute just a single namespace called X.Y which contains both class D and class E.</span></span>

<span data-ttu-id="0cf9a-386">場合によっては、修飾名がキーワードを使用して開始可能性があります`Global`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-386">In some situations, a qualified name may begin with the keyword `Global`.</span></span> <span data-ttu-id="0cf9a-387">キーワードは、これは、宣言が外側の名前空間をシャドウする場合に便利です無名の最も外側にある名前空間を表します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-387">The keyword represents the unnamed outermost namespace, which is useful in situations where a declaration shadows an enclosing namespace.</span></span> <span data-ttu-id="0cf9a-388">`Global`キーワードは、そのような状況で最も外側の名前空間を"エスケープ"処理を使用します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-388">The `Global` keyword allows "escaping" out to the outermost namespace in that situation.</span></span> <span data-ttu-id="0cf9a-389">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-389">For example:</span></span>

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

<span data-ttu-id="0cf9a-390">上記の例でも最初のメソッド呼び出しが無効ですので識別子`System`、クラスにバインド`System`、名前空間ではない`System`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-390">In the above example, the first method call is invalid because the identifier `System` binds to the class `System`, not the namespace `System`.</span></span> <span data-ttu-id="0cf9a-391">アクセスする唯一の方法、`System`名前空間は、使用する`Global`最も外側の名前空間をエスケープします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-391">The only way to access the `System` namespace is to use `Global` to escape out to the outermost namespace.</span></span> <span data-ttu-id="0cf9a-392">`Global` 使用することはできません、`Imports`ステートメントまたは`Namespace`宣言します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-392">`Global` cannot be used in an `Imports` statement or `Namespace` declaration.</span></span>

<span data-ttu-id="0cf9a-393">他の言語には、型と言語のキーワードに一致する名前空間を生じる可能性があります、ために、Visual Basic は、一定期間に従っていれば、修飾名の一部であるキーワードを認識します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-393">Because other languages may introduce types and namespaces that match keywords in the language, Visual Basic recognizes keywords to be part of a qualified name as long as they follow a period.</span></span> <span data-ttu-id="0cf9a-394">この方法で使用されるキーワードは、識別子として扱われます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-394">Keywords used in this way are treated as identifiers.</span></span> <span data-ttu-id="0cf9a-395">修飾された識別子など、`X.Default.Class`は有効な修飾された識別子では、中に`Default.Class`はありません。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-395">For example, the qualified identifier `X.Default.Class` is a valid qualified identifier, while `Default.Class` is not.</span></span>

### <a name="qualified-name-resolution-for-namespaces-and-types"></a><span data-ttu-id="0cf9a-396">名前空間と型の修飾の名前解決</span><span class="sxs-lookup"><span data-stu-id="0cf9a-396">Qualified Name Resolution for namespaces and types</span></span>

<span data-ttu-id="0cf9a-397">フォームの修飾の名前空間または型の名前を指定`N.R(Of A)`ここで、`R`は修飾名の右端の識別子と`A`は省略可能な型引数リストでは、次の手順は、どの名前空間を決定する方法を説明または修飾名は参照の種類:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-397">Given a qualified namespace or type name of the form `N.R(Of A)`, where `R` is the rightmost identifier in the qualified name and `A` is an optional type argument list, the following steps describe how to determine to which namespace or type the qualified name refers:</span></span>

1. <span data-ttu-id="0cf9a-398">解決`N`のいずれかの修飾付きまたは修飾されていない名前解決規則を使用します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-398">Resolve `N`, using the rules for either qualified or unqualified name resolution.</span></span>

2. <span data-ttu-id="0cf9a-399">場合の解決`N`失敗した場合、または、コンパイル時エラーが発生した、型パラメーターに解決されます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-399">If resolution of `N` fails, or resolves to a type parameter, a compile-time error occurs.</span></span>

3. <span data-ttu-id="0cf9a-400">の場合`R`N と型引数なしで名前空間の名前が指定された一致または`R`でアクセス可能な型と一致する`N`型引数として型パラメーターの同じ番号、存在する場合、修飾名を参照その名前空間または型。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-400">Otherwise, if `R` matches the name of a namespace in N and no type arguments were supplied, or `R` matches an accessible type in `N` with the same number of type parameters as type arguments, if any, then the qualified name refers to that namespace or type.</span></span>

4. <span data-ttu-id="0cf9a-401">の場合`N`1 つまたは複数の標準的なモジュールが含まれていますと`R`をいずれか 1 つの標準のモジュール修飾名で参照された場合、型引数として型パラメーターの同じ番号を持つアクセス可能な型の名前に一致入力します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-401">Otherwise, if `N` contains one or more standard modules, and `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in exactly one standard module, then the qualified name refers to that type.</span></span> <span data-ttu-id="0cf9a-402">場合`R`いずれかで 1 つ以上の標準のモジュール、コンパイル時エラーが発生した場合、型引数として型パラメーターの同じ番号を持つアクセス可能な型の名前に一致します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-402">If `R` matches the name of accessible types with the same number of type parameters as type arguments, if any, in more than one standard module, a compile-time error occurs.</span></span>

5. <span data-ttu-id="0cf9a-403">それ以外の場合は、コンパイル時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-403">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="0cf9a-404">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="0cf9a-404">__Note.__</span></span> <span data-ttu-id="0cf9a-405">この解決プロセスの影響がある型のメンバーをシャドウしません名前空間または型名前空間または型の名前を解決するときにします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-405">An implication of this resolution process is that type members do not shadow namespaces or types when resolving namespace or type names.</span></span>

### <a name="unqualified-name-resolution-for-namespaces-and-types"></a><span data-ttu-id="0cf9a-406">名前空間と型の非修飾の名前解決</span><span class="sxs-lookup"><span data-stu-id="0cf9a-406">Unqualified Name Resolution for namespaces and types</span></span>

<span data-ttu-id="0cf9a-407">非修飾名を指定`R(Of A)`ここで、`A`は省略可能な型引数リストでは、次の手順を参照する名前空間または型修飾されていない名前を確認する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-407">Given an unqualified name `R(Of A)`, where `A` is an optional type argument list, the following steps describe how to determine to which namespace or type the unqualified name refers:</span></span>

1. <span data-ttu-id="0cf9a-408">R が現在のメソッドの型パラメーターの名前と一致する型引数が指定されていない場合は、非修飾名はその型パラメーターを参照します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-408">If R matches the name of a type parameter of the current method, and no type arguments were supplied, then the unqualified name refers to that type parameter.</span></span>

2.  <span data-ttu-id="0cf9a-409">それぞれの入れ子にされた型名の参照を格納している最も内側の型から開始し、最も外側に。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-409">For each nested type containing the name reference, starting from the innermost type and going to the outermost:</span></span>
    21. <span data-ttu-id="0cf9a-410">場合`R`非修飾名は、その型パラメーターを参照し、現在の型でない型の引数が指定された型パラメーターの名前に一致します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-410">If `R` matches the name of a type parameter in the current type and no type arguments were supplied, then the unqualified name refers to that type parameter.</span></span>
    22. <span data-ttu-id="0cf9a-411">の場合`R`のアクセス可能な名前には、同じ数の型が入れ子になったの一致が存在する場合に、型の引数としてパラメーターを入力し、非修飾名は、その型を参照します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-411">Otherwise, if `R` matches the name of an accessible nested type with the same number of type parameters as type arguments, if any, then the unqualified name refers to that type.</span></span>

3. <span data-ttu-id="0cf9a-412">入れ子になった名前空間ごとに、最も内側の名前空間から開始し、最も外側の名前空間には、名前の参照を含みます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-412">For each nested namespace containing the name reference, starting from the innermost namespace and going to the outermost namespace:</span></span>
    31. <span data-ttu-id="0cf9a-413">場合`R`非修飾名がその入れ子になった名前空間を参照し、現在の名前空間となしの型引数リスト内の入れ子になった名前空間の名前が指定した一致。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-413">If `R` matches the name of a nested namespace in the current namespace and no type argument list is supplied, then the unqualified name refers to that nested namespace.</span></span>
    32. <span data-ttu-id="0cf9a-414">の場合`R`いずれかで現在の名前空間では、非修飾名がその型を表している場合、型引数として型パラメーターの同じ番号を持つアクセス可能な型の名前に一致します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-414">Otherwise, if `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in the current namespace, then the unqualified name refers to that type.</span></span>
    33. <span data-ttu-id="0cf9a-415">それ以外の場合、名前空間には、1 つまたは複数のアクセス可能な標準的なモジュールが含まれている場合と`R`し、修飾されていない、ただ 1 つの標準的なモジュールに存在する場合、型引数として型パラメーターの同じ番号を持つアクセス可能な入れ子になった型の名前に一致名前は、その入れ子にされた型を参照します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-415">Otherwise, if the namespace contains one or more accessible standard modules, and `R` matches the name of an accessible nested type with the same number of type parameters as type arguments, if any, in exactly one standard module, then the unqualified name refers to that nested type.</span></span> <span data-ttu-id="0cf9a-416">場合`R`いずれかで 1 つ以上の標準のモジュール、コンパイル時エラーが発生した場合、型引数として型パラメーターの同じ番号を持つアクセス可能な入れ子になった型の名前に一致します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-416">If `R` matches the name of accessible nested types with the same number of type parameters as type arguments, if any, in more than one standard module, a compile-time error occurs.</span></span>

4. <span data-ttu-id="0cf9a-417">ソース ファイルが 1 つまたは複数のインポート エイリアス、および`R`非修飾名は、そのインポート エイリアスを参照し、それらのいずれかの名前に一致します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-417">If the source file has one or more import aliases, and `R` matches the name of one of them, then the unqualified name refers to that import alias.</span></span> <span data-ttu-id="0cf9a-418">型引数リストを指定すると、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-418">If a type argument list is supplied, a compile-time error occurs.</span></span>

5. <span data-ttu-id="0cf9a-419">名前参照を含むソース ファイルの 1 つまたは複数のインポート: 場合</span><span class="sxs-lookup"><span data-stu-id="0cf9a-419">If the source file containing the name reference has one or more imports:</span></span>
    51. <span data-ttu-id="0cf9a-420">場合`R`いずれかで 1 つだけインポートは、非修飾名がその型を表している場合、型引数として型パラメーターの同じ番号を持つアクセス可能な型の名前に一致します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-420">If `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in exactly one import, then the unqualified name refers to that type.</span></span> <span data-ttu-id="0cf9a-421">場合`R`いずれか 1 つ以上のインポートとすべてがない場合、同じ型で、コンパイル時エラーが発生した型の引数として型パラメーターの同じ番号を持つアクセス可能な型の名前に一致します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-421">If `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in more than one import and all are not the same type, a compile-time error occurs.</span></span>
    52. <span data-ttu-id="0cf9a-422">それ以外の場合、型引数リストが指定されていない場合、`R`非修飾名は、その名前空間を参照し、1 つだけのインポートでアクセス可能な型で、名前空間の名前に一致します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-422">Otherwise, if no type argument list was supplied and `R` matches the name of a namespace with accessible types in exactly one import, then the unqualified name refers to that namespace.</span></span> <span data-ttu-id="0cf9a-423">型引数リストが指定されていない場合、`R`一致するすべての 1 つ以上のインポートでアクセス可能な型と名前空間の名前は、同じ名前空間、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-423">If no type argument list was supplied and `R` matches the name of a namespace with accessible types in more than one import and all are not the same namespace, a compile-time error occurs.</span></span>
    53. <span data-ttu-id="0cf9a-424">それ以外の場合、インポートには、1 つまたは複数のアクセス可能な標準的なモジュールが含まれている場合と`R`し、1 つの標準のモジュールに存在する場合、型引数として型パラメーターの同じ番号を持つアクセス可能な入れ子になった型の名前、非修飾名と一致します。その型を参照します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-424">Otherwise, if the imports contain one or more accessible standard modules, and `R` matches the name of an accessible nested type with the same number of type parameters as type arguments, if any, in exactly one standard module, then the unqualified name refers to that type.</span></span> <span data-ttu-id="0cf9a-425">場合`R`いずれかで 1 つ以上の標準のモジュール、コンパイル時エラーが発生した場合、型引数として型パラメーターの同じ番号を持つアクセス可能な入れ子になった型の名前に一致します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-425">If `R` matches the name of accessible nested types with the same number of type parameters as type arguments, if any, in more than one standard module, a compile-time error occurs.</span></span>

6. <span data-ttu-id="0cf9a-426">コンパイル環境に 1 つまたは複数のインポート エイリアスが定義されている場合と`R`非修飾名は、そのインポート エイリアスを参照し、それらのいずれかの名前に一致します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-426">If the compilation environment defines one or more import aliases, and `R` matches the name of one of them, then the unqualified name refers to that import alias.</span></span> <span data-ttu-id="0cf9a-427">型引数リストを指定すると、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-427">If a type argument list is supplied, a compile-time error occurs.</span></span>

7. <span data-ttu-id="0cf9a-428">場合は、コンパイル環境では、1 つまたは複数のインポートを定義します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-428">If the compilation environment defines one or more imports:</span></span>
    71. <span data-ttu-id="0cf9a-429">場合`R`いずれかで 1 つだけインポートは、非修飾名がその型を表している場合、型引数として型パラメーターの同じ番号を持つアクセス可能な型の名前に一致します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-429">If `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in exactly one import, then the unqualified name refers to that type.</span></span> <span data-ttu-id="0cf9a-430">場合`R`いずれか 1 つ以上のインポート、コンパイル時エラーが発生した場合、型引数として型パラメーターの同じ番号を持つアクセス可能な型の名前に一致します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-430">If `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in more than one import, a compile-time error occurs.</span></span>
    72. <span data-ttu-id="0cf9a-431">それ以外の場合、型引数リストが指定されていない場合、`R`非修飾名は、その名前空間を参照し、1 つだけのインポートでアクセス可能な型で、名前空間の名前に一致します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-431">Otherwise, if no type argument list was supplied and `R` matches the name of a namespace with accessible types in exactly one import, then the unqualified name refers to that namespace.</span></span> <span data-ttu-id="0cf9a-432">型引数リストが指定されていない場合、`R`コンパイル時エラーが発生した 1 つ以上のインポートでアクセス可能な型と名前空間の名前に一致します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-432">If no type argument list was supplied and `R` matches the name of a namespace with accessible types in more than one import, a compile-time error occurs.</span></span>
    73. <span data-ttu-id="0cf9a-433">それ以外の場合、インポートには、1 つまたは複数のアクセス可能な標準的なモジュールが含まれている場合と`R`し、1 つの標準のモジュールに存在する場合、型引数として型パラメーターの同じ番号を持つアクセス可能な入れ子になった型の名前、非修飾名と一致します。その型を参照します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-433">Otherwise, if the imports contain one or more accessible standard modules, and `R` matches the name of an accessible nested type with the same number of type parameters as type arguments, if any, in exactly one standard module, then the unqualified name refers to that type.</span></span> <span data-ttu-id="0cf9a-434">場合`R`いずれかで 1 つ以上の標準のモジュール、コンパイル時エラーが発生した場合、型引数として型パラメーターの同じ番号を持つアクセス可能な入れ子になった型の名前に一致します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-434">If `R` matches the name of accessible nested types with the same number of type parameters as type arguments, if any, in more than one standard module, a compile-time error occurs.</span></span>

8. <span data-ttu-id="0cf9a-435">それ以外の場合は、コンパイル時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-435">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="0cf9a-436">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="0cf9a-436">__Note.__</span></span> <span data-ttu-id="0cf9a-437">この解決プロセスの影響がある型のメンバーをシャドウしません名前空間または型名前空間または型の名前を解決するときにします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-437">An implication of this resolution process is that type members do not shadow namespaces or types when resolving namespace or type names.</span></span>

<span data-ttu-id="0cf9a-438">通常、名前できます 1 回だけでは、特定の名前空間。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-438">Normally, a name can only occur once in a particular namespace.</span></span> <span data-ttu-id="0cf9a-439">ただし、ため、複数の .NET アセンブリ間で名前空間を宣言することができます、2 つのアセンブリが同じ完全修飾名を持つ型を定義する状況を含めることは可能です。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-439">However, because namespaces can be declared across multiple .NET assemblies, it is possible to have a situation where two assemblies define a type with the same fully qualified name.</span></span> <span data-ttu-id="0cf9a-440">その場合は、ソース ファイルの現在のセットで宣言された型は、外部 .NET アセンブリで宣言された型を優先します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-440">In that case, a type declared in the current set of source files is preferred over a type declared in an external .NET assembly.</span></span> <span data-ttu-id="0cf9a-441">それ以外の場合、名前があいまいと名前のあいまいさを解消する方法はありません。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-441">Otherwise, the name is ambiguous and there is no way to disambiguate the name.</span></span>

## <a name="variables"></a><span data-ttu-id="0cf9a-442">変数</span><span class="sxs-lookup"><span data-stu-id="0cf9a-442">Variables</span></span>

<span data-ttu-id="0cf9a-443">A*変数*記憶域の場所を表します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-443">A *variable* represents a storage location.</span></span> <span data-ttu-id="0cf9a-444">すべての変数ができる値を指定する型の変数に格納します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-444">Every variable has a type that determines what values can be stored in the variable.</span></span> <span data-ttu-id="0cf9a-445">Visual Basic はタイプ セーフな言語、プログラムのすべての変数に型があり、言語に値が格納されていることを保証するため、適切な型の変数は常に、します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-445">Because Visual Basic is a type-safe language, every variable in a program has a type and the language guarantees that values stored in variables are always of the appropriate type.</span></span> <span data-ttu-id="0cf9a-446">変数は、変数への参照を行う前に常にその型の既定値に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-446">Variables are always initialized to the default value of their type before any reference to the variable can be made.</span></span> <span data-ttu-id="0cf9a-447">初期化されていないメモリにアクセスすることはできません。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-447">It is not possible to access uninitialized memory.</span></span>

## <a name="generic-types-and-methods"></a><span data-ttu-id="0cf9a-448">ジェネリック型とメソッド</span><span class="sxs-lookup"><span data-stu-id="0cf9a-448">Generic Types and Methods</span></span>

<span data-ttu-id="0cf9a-449">(標準的なモジュール型および列挙型) を除く型およびメソッドを宣言できます*パラメーター入力*、宣言されていない型のインスタンスまで提供される型である、またはメソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-449">Types (except for standard modules and enumerated types) and methods can declare *type parameters*, which are types that will not be provided until an instance of the type is declared or the method is invoked.</span></span> <span data-ttu-id="0cf9a-450">型と型パラメーターを持つメソッドとも呼ばれますが*ジェネリック型*と*ジェネリック メソッド*、それぞれ、型またはメソッドは、特定の知識のない一般的に、記述する必要がありますので、型またはメソッドを使用するコードによって指定される型。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-450">Types and methods with type parameters are also known as *generic types* and *generic methods*, respectively, because the type or method must be written generically, without specific knowledge of the types that will be supplied by code that uses the type or method.</span></span>

<span data-ttu-id="0cf9a-451">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="0cf9a-451">__Note.__</span></span> <span data-ttu-id="0cf9a-452">この時点でメソッドとデリゲートがジェネリックであることができる場合でもプロパティ、イベント、および演算子することはできませんジェネリック自体。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-452">At this time, even though methods and delegates can be generic, properties, events and operators cannot be generic themselves.</span></span> <span data-ttu-id="0cf9a-453">外側のクラスからの型パラメーターが使用される可能性があります、ただし、します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-453">They may, however, use type parameters from the containing class.</span></span>

<span data-ttu-id="0cf9a-454">ジェネリック型またはメソッドの観点からは、型パラメーターは、型またはメソッドを使用する場合に、実際の型を使用して入力がプレース ホルダーの型です。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-454">From the perspective of the generic type or method, a type parameter is a placeholder type that will be filled in with an actual type when the type or method is used.</span></span> <span data-ttu-id="0cf9a-455">型または型またはメソッドを使用する時点でメソッドの型パラメーターに対して型引数が置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-455">Type arguments are substituted for the type parameters in the type or method at the point at which the type or method is used.</span></span> <span data-ttu-id="0cf9a-456">たとえば、としてジェネリック スタック クラスを実装できます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-456">For example, a generic stack class could be implemented as:</span></span>

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

<span data-ttu-id="0cf9a-457">使用して、宣言、`Stack(Of ItemType)`クラスは、型パラメーターに対して型引数を指定する必要があります`ItemType`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-457">Declarations that use the `Stack(Of ItemType)` class must supply a type argument for the type parameter `ItemType`.</span></span> <span data-ttu-id="0cf9a-458">この型が入力し、どこにも`ItemType`クラス内で使用されます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-458">This type is then filled in wherever `ItemType` is used within the class:</span></span>

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

### <a name="type-parameters"></a><span data-ttu-id="0cf9a-459">型パラメーター</span><span class="sxs-lookup"><span data-stu-id="0cf9a-459">Type Parameters</span></span>

<span data-ttu-id="0cf9a-460">型またはメソッドの宣言では、型パラメーターを指定することがあります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-460">Type parameters may be supplied on type or method declarations.</span></span> <span data-ttu-id="0cf9a-461">それぞれの型パラメーターでは、作成、構築された型またはメソッドに渡される型引数のプレース ホルダーである識別子です。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-461">Each type parameter is an identifier which is a place-holder for a type argument that is supplied to create a constructed type or method.</span></span> <span data-ttu-id="0cf9a-462">これに対し、型引数は、ジェネリック型またはメソッドを使用する場合、型パラメーターを置き換える実際の型です。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-462">By contrast, a type argument is the actual type that is substituted for the type parameter when a generic type or method is used.</span></span>

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

<span data-ttu-id="0cf9a-463">型またはメソッドの宣言で型パラメーターごとでは、その型またはメソッドの宣言領域の名前を定義します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-463">Each type parameter in a type or method declaration defines a name in the declaration space of that type or method.</span></span> <span data-ttu-id="0cf9a-464">したがってが別の型パラメーター、型のメンバー、メソッドのパラメーターまたはローカル変数と同じ名前を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-464">Thus, it cannot have the same name as another type parameter, a type member, a method parameter, or a local variable.</span></span> <span data-ttu-id="0cf9a-465">型またはメソッドに型パラメーターのスコープは、型全体またはメソッドです。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-465">The scope of a type parameter on a type or method is the entire type or method.</span></span> <span data-ttu-id="0cf9a-466">型パラメーター、全体の型宣言のスコープは、ので、入れ子にされた型は、外側の型パラメーターを使用できます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-466">Because type parameters are scoped to the entire type declaration, nested types can use outer type parameters.</span></span> <span data-ttu-id="0cf9a-467">つまり型へのアクセス、ジェネリック型の内部で入れ子になったときに型パラメーターを指定常にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-467">This also means that type parameters must always be specified when accessing types nested inside generic types:</span></span>

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

<span data-ttu-id="0cf9a-468">クラスの他のメンバーとは異なり、型パラメーターは継承されません。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-468">Unlike other members of a class, type parameters are not inherited.</span></span> <span data-ttu-id="0cf9a-469">簡単な名前を指定して型の型パラメーターを参照することができますのみつまり、親の型名で修飾することはできません。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-469">Type parameters in a type can only be referred to by their simple name; in other words, they cannot be qualified with the containing type name.</span></span> <span data-ttu-id="0cf9a-470">不適切なプログラミング スタイルでは、入れ子にされた型の型パラメーター メンバーを非表示にしたり、外側の型で宣言されたパラメーターを入力します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-470">Although it is bad programming style, the type parameters in a nested type can hide a member or type parameter declared in the outer type:</span></span>

```vb
Class Outer(Of T)
    Class Inner(Of T)
        Public t1 As T    ' Refers to Inner's T
    End Class
End Class
```

<span data-ttu-id="0cf9a-471">型およびメソッドがオーバー ロードする型パラメーターの数に基づく (または*アリティ*) 型またはメソッドを宣言します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-471">Types and methods may be overloaded based on the number of type parameters (or *arity*) that the types or methods declare.</span></span> <span data-ttu-id="0cf9a-472">たとえば、次の宣言は有効です。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-472">For example, the following declarations are legal:</span></span>

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

<span data-ttu-id="0cf9a-473">型の場合、オーバー ロードは常に、指定された型引数の数と照らし合わせて照合します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-473">In the case of types, overloads are always matched against the number of type arguments specified.</span></span> <span data-ttu-id="0cf9a-474">これは、機能は、同じプログラム内で、ジェネリックと非ジェネリック クラスを共に使用する場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-474">This is useful when using both generic and non-generic classes together in the same program:</span></span>

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

<span data-ttu-id="0cf9a-475">メソッド オーバー ロードの解決に関するセクションでは、メソッド型パラメーターでオーバー ロードの規則がについて説明します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-475">Rules for methods overloaded on type parameters are covered in the section on method overload resolution.</span></span>

<span data-ttu-id="0cf9a-476">コンテナーの宣言内で型パラメーターは完全な型と見なされます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-476">Within the containing declaration, type parameters are considered full types.</span></span> <span data-ttu-id="0cf9a-477">型パラメーターは、多くのさまざまな実際の型引数でインスタンス化することができます、ため若干異なる操作と次のように他の型よりも制限がある型パラメーター。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-477">Since a type parameter can be instantiated with many different actual type arguments, type parameters have slightly different operations and restrictions than other types as described below:</span></span>

* <span data-ttu-id="0cf9a-478">型パラメーターを使用して、直接基底クラスまたはインターフェイスを宣言することはできません。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-478">A type parameter cannot be used directly to declare a base class or interface.</span></span>

* <span data-ttu-id="0cf9a-479">パラメーターは、存在する場合、制約とは異なります。 型のメンバー検索の規則は、型パラメーターに適用されます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-479">The rules for member lookup on type parameters depend on the constraints, if any, applied to the type parameter.</span></span>

* <span data-ttu-id="0cf9a-480">使用可能な変換型パラメーターは、存在する場合、制約に依存しているは、型パラメーターに適用されます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-480">The available conversions for a type parameter depend on the constraints, if any, applied to the type parameters.</span></span>

* <span data-ttu-id="0cf9a-481">ない場合、`Structure`制約、型パラメーターによって表される型の値と比較できる`Nothing`を使用して`Is`と`IsNot`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-481">In the absence of a `Structure` constraint, a value with a type represented by a type parameter can be compared with `Nothing` using `Is` and `IsNot`.</span></span>

* <span data-ttu-id="0cf9a-482">型パラメーターでのみ使用できます、`New`式で型パラメーターの制約がある場合、`New`または`Structure`制約。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-482">A type parameter can only be used in a `New` expression if the type parameter is constrained by a `New` or a `Structure` constraint.</span></span>

* <span data-ttu-id="0cf9a-483">内で属性例外内で任意の場所は型パラメーターを使用することはできません、`GetType`式。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-483">A type parameter cannot be used anywhere within an attribute exception within a `GetType` expression.</span></span>

* <span data-ttu-id="0cf9a-484">型パラメーターは、その他のジェネリック型とパラメーターの型引数として使用できます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-484">Type parameters can be used as type arguments to other generic types and parameters.</span></span>

<span data-ttu-id="0cf9a-485">次の例では、ジェネリック型を拡張するは、`Stack(Of ItemType)`クラス。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-485">The following example is a generic type that extends the `Stack(Of ItemType)` class:</span></span>

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

<span data-ttu-id="0cf9a-486">宣言がへの型引数を提供するときに`MyStack`、同じ型の引数に適用される`Stack`もします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-486">When a declaration supplies a type argument to `MyStack`, the same type argument will be applied to `Stack` as well.</span></span>

<span data-ttu-id="0cf9a-487">型パラメーター、型では、純粋なコンパイル時の構成要素があります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-487">As a type, type parameters are purely a compile-time construct.</span></span> <span data-ttu-id="0cf9a-488">実行時に、それぞれの型パラメーターは、ジェネリック宣言する型引数を指定することによって指定された実行時の型にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-488">At run-time, each type parameter is bound to a run-time type that was specified by supplying a type argument to the generic declaration.</span></span> <span data-ttu-id="0cf9a-489">したがって、型パラメーターで宣言された変数の型は、実行時には、非ジェネリック型または構築された特定の型。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-489">Thus, the type of a variable declared with a type parameter will, at run-time, be a non-generic type or a specific constructed type.</span></span> <span data-ttu-id="0cf9a-490">すべての型パラメーターを含む式とステートメントの実行時の実行は、そのパラメーターの型引数として渡された実際の型を使用します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-490">The run-time execution of all statements and expressions involving type parameters uses the actual type that was supplied as the type argument for that parameter.</span></span>


### <a name="type-constraints"></a><span data-ttu-id="0cf9a-491">型の制約</span><span class="sxs-lookup"><span data-stu-id="0cf9a-491">Type Constraints</span></span>

<span data-ttu-id="0cf9a-492">型引数を指定できますが、任意の型、型システムであるため、ジェネリック型またはメソッドは型パラメーターに関してどのような想定をことはできません。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-492">Because a type argument can be any type in the type system, a generic type or method cannot make any assumptions about a type parameter.</span></span> <span data-ttu-id="0cf9a-493">型パラメーターのメンバーが型のメンバーであると見なされるため、`Object`からすべての型を派生させるため、`Object`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-493">Thus, the members of a type parameter are considered to be the members of the type `Object`, since all types derive from `Object`.</span></span>

<span data-ttu-id="0cf9a-494">ようなコレクションの場合`Stack(Of ItemType)`とジェネリック型はするが型引数として提供する型を想定する場合もありますが、この事実に特に重要な制限事項では、できない場合があります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-494">In the case of a collection like `Stack(Of ItemType)`, this fact may not be a particularly important restriction, but there may be cases where a generic type may wish to make an assumption about the types that will be supplied as type arguments.</span></span> <span data-ttu-id="0cf9a-495">*制約入力*どの型が型パラメーターとして指定でき、ジェネリック型またはメソッドの詳細については、型パラメーターを想定することを許可するが制限されている型パラメーターに配置できます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-495">*Type constraints* can be placed on type parameters that restrict which types can be supplied as a type parameter and allow generic types or methods to assume more about type parameters.</span></span>

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

<span data-ttu-id="0cf9a-496">この例で、`DisposableStack(Of ItemType)`インターフェイスを実装する型のみをその型パラメーター制約`System.IDisposable`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-496">In this example, the `DisposableStack(Of ItemType)` constrains its type parameter to only types that implement the interface `System.IDisposable`.</span></span> <span data-ttu-id="0cf9a-497">実装できるため、`Dispose`キューのままにメソッドを任意のオブジェクトを破棄します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-497">As a result, it can implement a `Dispose` method that disposes any objects still left in the queue.</span></span>

<span data-ttu-id="0cf9a-498">型制約は特殊な制約のいずれかを指定する必要があります`Class`、 `Structure`、または`New`、型がありますまたは`T`場所。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-498">A type constraint must be one of the special constraints `Class`, `Structure`, or `New`, or it must be a type `T` where:</span></span>

* <span data-ttu-id="0cf9a-499">`T` クラス、インターフェイス、または型パラメーターです。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-499">`T` is a class, an interface, or a type parameter.</span></span>

* <span data-ttu-id="0cf9a-500">`T` が `NotInheritable` ではありません。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-500">`T` is not `NotInheritable`.</span></span>

* <span data-ttu-id="0cf9a-501">`T` 、のいずれかでがないか、次の特殊な種類のいずれかから継承された型: `System.Array`、 `System.Delegate`、 `System.MulticastDelegate`、 `System.Enum`、または`System.ValueType`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-501">`T` is not one of, or a type inherited from one of, the following special types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span>

* <span data-ttu-id="0cf9a-502">`T` が `Object` ではありません。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-502">`T` is not `Object`.</span></span> <span data-ttu-id="0cf9a-503">すべての型から派生するので`Object`、このような制約は効果がありませんが、許可された場合。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-503">Since all types derive from `Object`, such a constraint would have no effect if it were permitted.</span></span>

* <span data-ttu-id="0cf9a-504">`T` ジェネリック型またはメソッドが宣言されていると、少なくともアクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-504">`T` must be at least as accessible as the generic type or method being declared.</span></span>

<span data-ttu-id="0cf9a-505">型の制約を中かっこで囲むことで、1 つの型パラメーターの型の複数の制約を指定できます (`{}`).</span><span class="sxs-lookup"><span data-stu-id="0cf9a-505">Multiple type constraints can be specified for a single type parameter by enclosing the type constraints in curly braces (`{}`)..</span></span> <span data-ttu-id="0cf9a-506">特定の型パラメーターの 1 つだけの型制約はクラスであることができます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-506">Only one type constraint for a given type parameter can be a class.</span></span> <span data-ttu-id="0cf9a-507">結合するとエラーには、`Structure`クラスの名前付き制約を持つ特殊な制約、または`Class`特殊な制約。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-507">It is an error to combine a `Structure` special constraint with a named class constraint or the `Class` special constraint.</span></span>

```vb
Class ControlFactory(Of T As {Control, New})
    ...
End Class
```

<span data-ttu-id="0cf9a-508">型の制約には、含む型または含んでいる型の型パラメーターのいずれかを使用できます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-508">Type constraints can use the containing types or any of the containing types' type parameters.</span></span> <span data-ttu-id="0cf9a-509">次の例では、制約は、指定された型引数が型引数として使用してジェネリック インターフェイスを実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-509">In the following example, the constraint requires that the type argument supplied implements a generic interface using itself as a type argument:</span></span>

```vb
Class Sorter(Of V As IComparable(Of V))
    ...
End Class
```

<span data-ttu-id="0cf9a-510">特殊な制約`Class`任意の参照型に指定された型引数を制限します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-510">The special type constraint `Class` constrains the supplied type argument to any reference type.</span></span>

<span data-ttu-id="0cf9a-511">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="0cf9a-511">__Note.__</span></span> <span data-ttu-id="0cf9a-512">特殊な制約`Class`インターフェイスによって満たすことができます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-512">The special type constraint `Class` can be satisfied by an interface.</span></span> <span data-ttu-id="0cf9a-513">構造体はインターフェイスを実装できます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-513">And a structure can implement an interface.</span></span> <span data-ttu-id="0cf9a-514">制約ではそのため、 `(Of T As U, U As Class)` "T"構造体で満足する可能性があります (これを満たさない、`Class`特殊な制約)、"u"を実装するインターフェイス (が満たしている、`Class`特殊な制約)。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-514">Therefore, the constraint `(Of T As U, U As Class)` might be satisfied with "T" a structure (which does not satisfy the `Class` special constraint), and "U" an interface that it implements (which does satisfy the `Class` special constraint).</span></span>

<span data-ttu-id="0cf9a-515">特殊な制約`Structure`以外の値の型に指定された型引数の制約`System.Nullable(Of T)`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-515">The special type constraint `Structure` constrains the supplied type argument to any value type except `System.Nullable(Of T)`.</span></span>

<span data-ttu-id="0cf9a-516">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="0cf9a-516">__Note.__</span></span> <span data-ttu-id="0cf9a-517">構造体の制約は許可しない`System.Nullable(Of T)`を指定することはできませんように`System.Nullable(Of T)`自体への型引数として。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-517">Structure constraints do not allow `System.Nullable(Of T)` so that it is not possible to supply `System.Nullable(Of T)` as a type argument to itself.</span></span>

<span data-ttu-id="0cf9a-518">特殊な制約`New`として宣言できません。 指定された型引数、アクセス可能なパラメーターなしのコンス トラクターが必要する必要があります`MustInherit`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-518">The special type constraint `New` requires that the supplied type argument must have an accessible parameterless constructor and cannot be declared `MustInherit`.</span></span> <span data-ttu-id="0cf9a-519">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-519">For example:</span></span>

```vb
Class Factory(Of T As New)
    Function CreateInstance() As T
        Return New T()
    End Function
End Class
```

<span data-ttu-id="0cf9a-520">クラス型制約は、指定された型引数する必要があります、として入力する、またはそれを継承するかが必要です。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-520">A class type constraint requires that the supplied type argument must either be that type as or inherit from it.</span></span> <span data-ttu-id="0cf9a-521">インターフェイスの型制約は、指定された型引数がそのインターフェイスを実装する必要がありますが必要です。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-521">An interface type constraint requires that the supplied type argument must implement that interface.</span></span> <span data-ttu-id="0cf9a-522">型パラメーターの制約は、指定された型引数する必要がありますから派生または一致する型パラメーターに指定された境界内のすべてを実装している必要があります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-522">A type parameter constraint requires that the supplied type argument must derive from or implement all of the bounds given for the matching type parameter.</span></span> <span data-ttu-id="0cf9a-523">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-523">For example:</span></span>

```vb
Class List(Of T)
    Sub AddRange(Of S As T)(collection As IEnumerable(Of S))
        ...
    End Sub
End Class
```

<span data-ttu-id="0cf9a-524">この例では、型パラメーターで`S`で`AddRange`型パラメーターに制約される`T`の`List`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-524">In this example, the type parameter `S` on `AddRange` is constrained to the type parameter `T` of `List`.</span></span> <span data-ttu-id="0cf9a-525">つまり、`List(Of Control)`制限は`AddRange`の入力パラメーターはかから継承する任意の型を`Control`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-525">This means that a `List(Of Control)` would constrain `AddRange`'s type parameter to any type that is or inherits from `Control`.</span></span>

<span data-ttu-id="0cf9a-526">型パラメーター制約`Of S As T`が推移的にすべての追加することによって解決`T`の制約に`S`、その他の特殊な制約よりも (`Class`、 `Structure`、 `New`)。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-526">A type parameter constraint `Of S As T` is resolved by transitively adding all of `T`'s constraints onto `S`, other than the special constraints (`Class`, `Structure`, `New`).</span></span> <span data-ttu-id="0cf9a-527">エラーに循環制約があることになります (例: `Of S As T, T As S`)。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-527">It is an error to have circular constraints (e.g. `Of S As T, T As S`).</span></span> <span data-ttu-id="0cf9a-528">それ自体が、エラー、型パラメーター制約には、`Structure`制約。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-528">It is an error to have a type parameter constraint which itself has the `Structure` constraint.</span></span> <span data-ttu-id="0cf9a-529">制約を追加した後は、さまざまな特別な状況が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-529">After adding constraints, it is possible that a number of special situations may occur:</span></span>

* <span data-ttu-id="0cf9a-530">複数のクラスの制約が存在しない場合、最派生クラスは、制約と見なされます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-530">If multiple class constraints exist, the most derived class is considered to be the constraint.</span></span> <span data-ttu-id="0cf9a-531">1 つまたは複数のクラスの制約には、継承関係がない、制約が満たされていないと、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-531">If one or more class constraints have no inheritance relationship, the constraint is unsatisfiable and it is an error.</span></span>

 * <span data-ttu-id="0cf9a-532">型パラメーターを結合する場合、`Structure`クラスの名前付き制約を持つ特殊な制約、または`Class`特殊な制約は、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-532">If a type parameter combines a `Structure` special constraint with a named class constraint or the `Class` special constraint, it is an error.</span></span> <span data-ttu-id="0cf9a-533">クラスの制約があります`NotInheritable`、この場合その制約の派生型は受け入れられませんされ、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-533">A class constraint may be `NotInheritable`, in which case no derived types of that constraint are accepted and it is an error.</span></span>

<span data-ttu-id="0cf9a-534">種類がありますのいずれかまたは型が、次の特殊な型から継承: `System.Array`、 `System.Delegate`、 `System.MulticastDelegate`、 `System.Enum`、または`System.ValueType`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-534">The type may be one of, or a type inherited from, the following special types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span> <span data-ttu-id="0cf9a-535">その場合、型のみ、またはそれから継承された型が受け入れられます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-535">In that case, only the type, or a type inherited from it, is accepted.</span></span> <span data-ttu-id="0cf9a-536">これらの型のいずれかに制限されている型パラメーターのみで許可されている変換を使用できます、`DirectCast`演算子。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-536">A type parameter constrained to one of these types can only use the conversions allowed by the `DirectCast` operator.</span></span> <span data-ttu-id="0cf9a-537">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-537">For example:</span></span>

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

<span data-ttu-id="0cf9a-538">さらに、上記リラクゼーションのいずれかの値の型を制約付きの型パラメーターは、その値の型で定義されているすべてのメソッドを呼び出すことはできません。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-538">Additionally, a type parameter constrained to a value type due to one of the above relaxations cannot call any methods defined on that value type.</span></span> <span data-ttu-id="0cf9a-539">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-539">For example:</span></span>

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

<span data-ttu-id="0cf9a-540">制約、置換では、後に最終的に、配列型として、任意の配列の共変の型も許可されます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-540">If the constraint, after substitution, ends up as an array type, any covariant array type is allowed as well.</span></span> <span data-ttu-id="0cf9a-541">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-541">For example:</span></span>

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

<span data-ttu-id="0cf9a-542">クラスまたはインターフェイス制約と型パラメーターは、そのクラスまたはインターフェイスの制約と同じメンバーを持つと見なされます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-542">A type parameter with a class or interface constraint is considered to have the same members as that class or interface constraint.</span></span> <span data-ttu-id="0cf9a-543">型パラメーターに複数の制約がある場合、型パラメーターと見なされます、制約のすべてのメンバーの和集合。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-543">If a type parameter has multiple constraints, then the type parameter is considered to have the union of all the members of the constraints.</span></span> <span data-ttu-id="0cf9a-544">1 つ以上の制約で同じ名前のメンバーがあるかどうかは、次の順序でメンバーを非表示: クラスの制約内のメンバーを非表示にするインターフェイスの制約は、内のメンバーを非表示に`System.ValueType`(場合`Structure`制約を指定)、内のメンバーが隠ぺいされます`Object`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-544">If there are members with the same name in more than one constraint, then members are hidden in the following order: the class constraint hides members in interface constraints, which hide members in `System.ValueType` (if `Structure` constraint is specified), which hides members in `Object`.</span></span> <span data-ttu-id="0cf9a-545">1 つ以上のインターフェイス制約で同じ名前のメンバーが表示された場合、メンバーは (複数のインターフェイスの継承) のように使用できませんし、型パラメーターは、必要なインターフェイスにキャストする必要があります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-545">If a member with the same name appears in more than one interface constraint the member is unavailable (as in multiple interface inheritance) and the type parameter must be cast to the desired interface.</span></span> <span data-ttu-id="0cf9a-546">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-546">For example:</span></span>

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

<span data-ttu-id="0cf9a-547">型引数として型パラメーターを指定するとき、型パラメーターは、一致する型パラメーターの制約を満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-547">When supplying type parameters as type arguments, the type parameters must satisfy the constraints of the matching type parameters.</span></span>

```vb
Class Base(Of T As Class)
End Class

Class Derived(Of V)
    ' Error: V does not satisfy the constraints of T
    Inherits Base(Of V)
End Class
```

<span data-ttu-id="0cf9a-548">制約付きの型パラメーターの値は、制約で指定された、インスタンス メソッドを含む、インスタンス メンバーへのアクセスに使用できます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-548">Values of a constrained type parameter can be used to access the instance members, including instance methods, specified in the constraint.</span></span>

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

### <a name="type-parameter-variance"></a><span data-ttu-id="0cf9a-549">型パラメーターの分散</span><span class="sxs-lookup"><span data-stu-id="0cf9a-549">Type Parameter Variance</span></span>

<span data-ttu-id="0cf9a-550">インターフェイスまたはデリゲートの型宣言の型パラメーターを指定できます必要に応じて、*分散修飾子*します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-550">A type parameter in an interface or a delegate type declaration can optionally specify a *variance modifier*.</span></span> <span data-ttu-id="0cf9a-551">分散修飾子のある型パラメーターは、型パラメーターがインターフェイスまたはデリゲート型で使用できますがバリアント互換型引数を持つ別のジェネリック型に変換するジェネリック インターフェイスまたはデリゲート型を許可する方法を制限します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-551">Type parameters with variance modifiers restrict how the type parameter can be used in the interface or delegate type but allow a generic interface or delegate type to be converted to another generic type with variant compatible type arguments.</span></span> <span data-ttu-id="0cf9a-552">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-552">For example:</span></span>

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

<span data-ttu-id="0cf9a-553">分散修飾子を持つ型のパラメーターを持つジェネリック インターフェイスでは、いくつかの制限があります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-553">Generic interfaces that have type parameters with variance modifiers have several restrictions:</span></span>

* <span data-ttu-id="0cf9a-554">パラメーター リストを指定するイベントの宣言を含めることはできません (ただし、カスタム イベント宣言またはイベントのデリゲート型で宣言が許可されている)。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-554">They cannot contain an event declaration that specifies a parameter list (but a custom event declaration or an event declaration with a delegate type is allowed).</span></span>

* <span data-ttu-id="0cf9a-555">入れ子になったクラス、構造体、または列挙型を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-555">They cannot contain a nested class, structure, or enumerated type.</span></span>

<span data-ttu-id="0cf9a-556">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="0cf9a-556">__Note.__</span></span> <span data-ttu-id="0cf9a-557">これらの制限は、入れ子になったジェネリック型に暗黙的に型の親のジェネリック パラメーターをコピーするという事実が原因には。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-557">These restrictions are due to the fact that types nested in generic types implicitly copy the generic parameters of their parent.</span></span> <span data-ttu-id="0cf9a-558">入れ子になったクラス、構造体、または列挙型の場合は、これらの型、型パラメーターの分散修飾子ことはできません。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-558">In the case of nested classes, structures, or enumerated types, those kinds of types cannot have variance modifiers on their type parameters.</span></span> <span data-ttu-id="0cf9a-559">生成された入れ子になったデリゲート クラスの場合は、イベントは、パラメーター リストで宣言して、エラー、型で使用される表示されるときに混乱を招くことができます、`In`で位置 (パラメーターの型など) が実際に使用する`Out`位置 (つまり、イベントの種類)。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-559">In the case of an event declaration with a parameter list, the generated nested delegate class could have confusing errors when a type that appears to be used in an `In` position (i.e. a parameter type) is actually used in an `Out` position (i.e. the type of the event).</span></span>

<span data-ttu-id="0cf9a-560">Out 修飾子で宣言された型パラメーターが*共変*します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-560">A type parameter that is declared with the Out modifier is *covariant*.</span></span> <span data-ttu-id="0cf9a-561">非公式には、共変の型パラメーターは出力位置 - つまり、インターフェイスまたはデリゲートの型から返される値--でのみ使用でき、入力の位置では使用できません。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-561">Informally, a covariant type parameter can only be used in an output position -- i.e. a value that is being returned from the interface or delegate type -- and cannot be used in an input position.</span></span> <span data-ttu-id="0cf9a-562">型`T`と見なされます*有効 covariantly*場合。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-562">A type `T` is considered to be *valid covariantly* if:</span></span>

* <span data-ttu-id="0cf9a-563">`T` クラス、構造体、または列挙型。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-563">`T` is a class, structure, or enumerated type.</span></span>

* <span data-ttu-id="0cf9a-564">`T` 非ジェネリック デリゲートまたはインターフェイス型です。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-564">`T` is non-generic delegate or interface type.</span></span>

* <span data-ttu-id="0cf9a-565">`T` 要素型が covariantly 有効な配列型です。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-565">`T` is an array type whose element type is valid covariantly.</span></span>

* <span data-ttu-id="0cf9a-566">`T` 型パラメーターとして宣言されませんでしたが、`Out`パラメーターを入力します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-566">`T` is a type parameter which was not declared as an `Out` type parameter.</span></span>

* <span data-ttu-id="0cf9a-567">`T` 構築されたインターフェイスまたはデリゲートの型は、`X(Of P1,...,Pn)`型引数を持つ`A1,...,An`ように。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-567">`T` is a constructed interface or delegate type `X(Of P1,...,Pn)` with type arguments `A1,...,An` such that:</span></span>

  * <span data-ttu-id="0cf9a-568">場合`Pi`し Out 型パラメーターとして宣言された`Ai`covariantly は有効です。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-568">If `Pi` was declared as an Out type parameter then `Ai` is valid covariantly.</span></span>

  * <span data-ttu-id="0cf9a-569">場合`Pi`し In 型パラメーターとして宣言された`Ai`変に有効な機能です。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-569">If `Pi` was declared as an In type parameter then `Ai` is valid contravariantly.</span></span>

<span data-ttu-id="0cf9a-570">次の covariantly インターフェイスまたはデリゲート型で有効な必要があります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-570">The following must be valid covariantly in an interface or delegate type:</span></span>

* <span data-ttu-id="0cf9a-571">インターフェイスの基本インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-571">The base interface of an interface.</span></span>

* <span data-ttu-id="0cf9a-572">関数の戻り値の型またはデリゲートの型。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-572">The return type of a function or the delegate type.</span></span>

* <span data-ttu-id="0cf9a-573">存在する場合のプロパティの型を`Get`アクセサー。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-573">The type of a property if there is a `Get` accessor.</span></span>

* <span data-ttu-id="0cf9a-574">いずれかの種類`ByRef`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-574">The type of any `ByRef` parameter.</span></span>

<span data-ttu-id="0cf9a-575">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-575">For example:</span></span>

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

<span data-ttu-id="0cf9a-576">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="0cf9a-576">__Note.__</span></span> <span data-ttu-id="0cf9a-577">`Out` 予約語ではありません。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-577">`Out` is not a reserved word.</span></span>

<span data-ttu-id="0cf9a-578">修飾子で宣言された型パラメーターが*反変*します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-578">A type parameter that is declared with the In modifier is *contravariant*.</span></span> <span data-ttu-id="0cf9a-579">非公式、反変の型パラメーターは入力の位置 - で、インターフェイスまたはデリゲート型に渡される値つまり--でのみ使用でき、出力位置では使用できません。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-579">Informally, a contravariant type parameter can only be used in an input position -- i.e. a value that is being passed in to the interface or delegate type -- and cannot be used in an output position.</span></span> <span data-ttu-id="0cf9a-580">型`T`と見なされます*変に有効な機能*場合。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-580">A type `T` is considered to be *valid contravariantly* if:</span></span>

* <span data-ttu-id="0cf9a-581">`T` クラス、構造体、または列挙型。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-581">`T` is a class, structure, or enumerated type.</span></span>

* <span data-ttu-id="0cf9a-582">`T` 非ジェネリック デリゲートまたはインターフェイス型です。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-582">`T` is a non-generic delegate or interface type.</span></span>

* <span data-ttu-id="0cf9a-583">`T` 要素型を持つ変に有効な機能は、配列型です。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-583">`T` is an array type whose element type is valid contravariantly.</span></span>

* <span data-ttu-id="0cf9a-584">`T` In 型パラメーターとして宣言されませんでしたが、型パラメーターです。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-584">`T` is a type parameter which was not declared as an In type parameter.</span></span>

* <span data-ttu-id="0cf9a-585">`T` 構築されたインターフェイスまたはデリゲートの型は、`X(Of P1,...,Pn)`型引数を持つ`A1,...,An`ように。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-585">`T` is a constructed interface or delegate type `X(Of P1,...,Pn)` with type arguments `A1,...,An` such that:</span></span>

  * <span data-ttu-id="0cf9a-586">場合`Pi`として宣言されている、`Out`パラメーターを入力し、`Ai`変に有効な機能です。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-586">If `Pi` was declared as an `Out` type parameter then `Ai` is valid contravariantly.</span></span>

  * <span data-ttu-id="0cf9a-587">場合`Pi`として宣言されている、`In`パラメーターを入力し、 `Ai` covariantly は有効です。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-587">If `Pi` was declared as an `In` type parameter then `Ai` is valid covariantly.</span></span>

<span data-ttu-id="0cf9a-588">次のインターフェイスまたはデリゲート型で有効な変に機能があります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-588">The following must be valid contravariantly in an interface or delegate type:</span></span>

* <span data-ttu-id="0cf9a-589">パラメーターの型。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-589">The type of a parameter.</span></span>

* <span data-ttu-id="0cf9a-590">型の制約をメソッド型パラメーター。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-590">A type constraint on a method type parameter.</span></span>

* <span data-ttu-id="0cf9a-591">ある場合、プロパティの型を`Set`アクセサー。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-591">The type of a property if it has a `Set` accessor.</span></span>

* <span data-ttu-id="0cf9a-592">イベントの種類。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-592">The type of an event.</span></span>

<span data-ttu-id="0cf9a-593">例えば:</span><span class="sxs-lookup"><span data-stu-id="0cf9a-593">For example:</span></span>

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

<span data-ttu-id="0cf9a-594">場合は、型が有効なある変に機能と covariantly (両方を持つプロパティなど、`Get`と`Set`アクセサーまたは`ByRef`パラメーター)、バリアント型パラメーターを使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-594">In the case where a type must be valid be contravariantly and covariantly (such as a property with both a `Get` and `Set` accessor or a `ByRef` parameter), a variant type parameter cannot be used.</span></span>


<span data-ttu-id="0cf9a-595">Co および反変性は、「ダイヤモンドあいまいさが問題」に上昇を付与します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-595">Co- and contra-variance give rise to a "diamond ambiguity problem".</span></span> <span data-ttu-id="0cf9a-596">次のコードがあるとします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-596">Consider the following code:</span></span>

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

<span data-ttu-id="0cf9a-597">クラスは、`C`に変換できる`IEnumerable(Of Object)`両方からの共変性の変換を 2 つの方法で`IEnumerable(Of String)`と共変性の変換から`IEnumerable(Of Exception)`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-597">The class `C` can be converted to `IEnumerable(Of Object)` in two ways, both through covariant conversion from `IEnumerable(Of String)` and through covariant conversion from `IEnumerable(Of Exception)`.</span></span> <span data-ttu-id="0cf9a-598">CLR でによって呼び出される 2 つの方法が指定されていない`c.GetEnumerator()`します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-598">The CLR does not specify which of the two methods will be called by `c.GetEnumerator()`.</span></span> <span data-ttu-id="0cf9a-599">共通のスーパー タイプを持つ 2 つの異なるジェネリック引数の共変のインターフェイスを実装するクラスが宣言されたときに一般に、(この場合の例:`String`と`Exception`共通のスーパー タイプがある`Object`)、またはするクラスが宣言されています。2 つの異なるジェネリック引数を持つ一般的なサブタイプの場合は、反変のインターフェイスを実装し、あいまいさが発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-599">In general, whenever a class is declared to implement a covariant interface with two different generic arguments that have a common supertype (e.g. in this case `String` and `Exception` have the common supertype `Object`), or a class is declared to implement a contravariant interface with two different generic arguments that have a common subtype, then ambiguity is likely to arise.</span></span> <span data-ttu-id="0cf9a-600">コンパイラは、このような宣言に関する警告を提供します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-600">The compiler gives a warning on such declarations.</span></span>
