---
ms.openlocfilehash: 8599ffc0ad3313f4e7e42da220c22ffa7a37e56b
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306182"
---
# <a name="general-concepts"></a><span data-ttu-id="ac1cf-101">一般的な概念</span><span class="sxs-lookup"><span data-stu-id="ac1cf-101">General Concepts</span></span>

<span data-ttu-id="ac1cf-102">この章では、Microsoft Visual Basic 言語のセマンティクスを理解するために必要ないくつかの概念について説明します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-102">This chapter covers a number of concepts that are required to understand the semantics of the Microsoft Visual Basic language.</span></span> <span data-ttu-id="ac1cf-103">概念の多くは、プログラマーや C/C++プログラマーの Visual Basic を理解している必要がありますが、正確な定義は異なる場合があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-103">Many of the concepts should be familiar to Visual Basic programmers or C/C++ programmers, but their precise definitions may differ.</span></span>

## <a name="declarations"></a><span data-ttu-id="ac1cf-104">宣言</span><span class="sxs-lookup"><span data-stu-id="ac1cf-104">Declarations</span></span>

<span data-ttu-id="ac1cf-105">Visual Basic プログラムは、名前付きエンティティで構成されます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-105">A Visual Basic program is made up of named entities.</span></span> <span data-ttu-id="ac1cf-106">これらのエンティティは、*宣言*によって導入され、プログラムの "意味" を表します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-106">These entities are introduced through *declarations* and represent the "meaning" of the program.</span></span>

<span data-ttu-id="ac1cf-107">最上位レベルでは、*名前空間*は、入れ子になった名前空間や型など、他のエンティティを編成するエンティティです。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-107">At a top level, *namespaces* are entities that organize other entities, such as nested namespaces and types.</span></span> <span data-ttu-id="ac1cf-108">*型*は、値を記述し、実行可能コードを定義するエンティティです。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-108">*Types* are entities that describe values and define executable code.</span></span> <span data-ttu-id="ac1cf-109">型には、入れ子にされた型と型のメンバーを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-109">Types may contain nested types and type members.</span></span> <span data-ttu-id="ac1cf-110">*型のメンバー*は、定数、変数、メソッド、演算子、プロパティ、イベント、列挙値、およびコンストラクターです。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-110">*Type members* are constants, variables, methods, operators, properties, events, enumeration values, and constructors.</span></span>

<span data-ttu-id="ac1cf-111">他のエンティティを含むことができるエンティティは、*宣言空間*を定義します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-111">An entity that can contain other entities defines a *declaration space*.</span></span> <span data-ttu-id="ac1cf-112">エンティティは、宣言または継承によって宣言領域に導入されます。含まれている宣言領域は、エンティティの*宣言コンテキスト*と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-112">Entities are introduced into a declaration space either through declarations or inheritance; the containing declaration space is called the entities' *declaration context*.</span></span> <span data-ttu-id="ac1cf-113">宣言空間でエンティティを宣言すると、さらに入れ子になったエンティティ宣言を含めることができる新しい宣言空間が定義されます。したがって、プログラム内の宣言は、宣言スペースの階層を形成します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-113">Declaring an entity in a declaration space in turn defines a new declaration space that can contain further nested entity declarations; thus, the declarations in a program form a hierarchy of declaration spaces.</span></span>

<span data-ttu-id="ac1cf-114">オーバーロードされた型のメンバーの場合を除き、同じ種類の同じ名前を持つエンティティを同じ宣言コンテキストに導入する宣言では無効です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-114">Except in the case of overloaded type members, it is invalid for declarations to introduce identically named entities of the same kind into the same declaration context.</span></span> <span data-ttu-id="ac1cf-115">また、宣言空間には、同じ名前を持つ異なる種類のエンティティを含めることはできません。たとえば、宣言空間には、変数とメソッドを同じ名前で含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-115">Additionally, a declaration space may never contain different kinds of entities with the same name; for example, a declaration space may never contain a variable and a method by the same name.</span></span>

<span data-ttu-id="ac1cf-116">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="ac1cf-116">__Note.__</span></span> <span data-ttu-id="ac1cf-117">他の言語では、同じ名前を持つ異なる種類のエンティティを含む宣言空間を作成することができます (たとえば、言語で大文字と小文字が区別され、大文字と小文字に基づく宣言が異なる場合など)。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-117">It may be possible in other languages to create a declaration space that contains different kinds of entities with the same name (for example, if the language is case sensitive and allows different declarations based on casing).</span></span> <span data-ttu-id="ac1cf-118">そのような場合、最もアクセスしやすいエンティティは、その名前にバインドされていると見なされます。複数の種類のエンティティが最もアクセス可能な場合は、名前があいまいになります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-118">In that situation, the most accessible entity is considered bound to that name; if more than one type of entity is most accessible then the name is ambiguous.</span></span> <span data-ttu-id="ac1cf-119">`Public` は `Protected Friend` よりもアクセスが可能です。 `Protected Friend` の方が、`Protected` または @no__t よりもアクセスしやすく、`Protected` または `Friend` よりも @no__t にアクセスできるようになりました。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-119">`Public` is more accessible than `Protected Friend`, `Protected Friend` is more accessible than `Protected` or `Friend`, and `Protected` or `Friend` is more accessible than `Private`.</span></span>

<span data-ttu-id="ac1cf-120">名前空間の宣言空間は "open 終了" です。そのため、同じ完全修飾名を持つ2つの名前空間宣言は、同じ宣言領域に関与します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-120">The declaration space of a namespace is "open ended," so two namespace declarations with the same fully qualified name contribute to the same declaration space.</span></span> <span data-ttu-id="ac1cf-121">次の例では、2つの名前空間宣言が同じ宣言領域に関与しています。この場合、完全修飾名を持つ2つのクラスを宣言する `Data.Customer` と `Data.Order:` です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-121">In the example below, the two namespace declarations contribute to the same declaration space, in this case declaring two classes with the fully qualified names `Data.Customer` and `Data.Order:`</span></span>

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

<span data-ttu-id="ac1cf-122">2つの宣言は同じ宣言空間に関与するため、それぞれに同じ名前のクラスの宣言が含まれていると、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-122">Because the two declarations contribute to the same declaration space, a compile-time error would occur if each contained a declaration of a class with the same name.</span></span>

### <a name="overloading-and-signatures"></a><span data-ttu-id="ac1cf-123">オーバーロードとシグネチャ</span><span class="sxs-lookup"><span data-stu-id="ac1cf-123">Overloading and Signatures</span></span>

<span data-ttu-id="ac1cf-124">宣言空間で同じ種類の同じ名前を持つエンティティを宣言する唯一の方法は、*オーバーロード*を使用することです。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-124">The only way to declare identically named entities of the same kind in a declaration space is through *overloading*.</span></span> <span data-ttu-id="ac1cf-125">メソッド、演算子、インスタンスコンストラクター、およびプロパティのみがオーバーロードされる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-125">Only methods, operators, instance constructors, and properties may be overloaded.</span></span>

<span data-ttu-id="ac1cf-126">オーバーロードされた型のメンバーは、一意のシグネチャを持つ必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-126">Overloaded type members must possess unique signatures.</span></span> <span data-ttu-id="ac1cf-127">型のメンバーのシグネチャは、型パラメーターの数と、メンバーのパラメーターの数と型で構成されます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-127">The signature of a type member consists of the number of type parameters, and the number and types of the member's parameters.</span></span> <span data-ttu-id="ac1cf-128">変換演算子には、シグネチャ内の演算子の戻り値の型も含まれます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-128">Conversion operators also include the return type of the operator in the signature.</span></span>

<span data-ttu-id="ac1cf-129">次のはメンバーのシグネチャの一部ではないため、でオーバーロードすることはできません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-129">The following are not part of a member's signature, and hence cannot be overloaded on:</span></span>

* <span data-ttu-id="ac1cf-130">型のメンバーに対する修飾子 (たとえば、`Shared` または `Private`)。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-130">Modifiers to a type member (for example, `Shared` or `Private`).</span></span>

* <span data-ttu-id="ac1cf-131">パラメーターに修飾子を指定します (たとえば、`ByVal` または `ByRef`)。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-131">Modifiers to a parameter (for example, `ByVal` or `ByRef`).</span></span>

* <span data-ttu-id="ac1cf-132">パラメーターの名前。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-132">The names of the parameters.</span></span>

* <span data-ttu-id="ac1cf-133">メソッドまたは演算子の戻り値の型 (変換演算子を除く)、またはプロパティの要素型。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-133">The return type of a method or operator (except for conversion operators) or the element type of a property.</span></span>

* <span data-ttu-id="ac1cf-134">型パラメーターに対する制約。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-134">Constraints on a type parameter.</span></span>

<span data-ttu-id="ac1cf-135">次の例は、オーバーロードされたメソッド宣言のセットとそのシグネチャを示しています。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-135">The following example shows a set of overloaded method declarations along with their signatures.</span></span> <span data-ttu-id="ac1cf-136">いくつかのメソッド宣言のシグネチャが同じであるため、この宣言は有効ではありません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-136">This declaration would not be valid since several of the method declarations have identical signatures.</span></span>

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

<span data-ttu-id="ac1cf-137">指定された型引数に基づいて、同じシグネチャを持つメンバーを含む可能性のあるジェネリック型を定義することは有効です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-137">It is valid to define a generic type that may contain members with identical signatures based on the type arguments supplied.</span></span> <span data-ttu-id="ac1cf-138">オーバーロードの解決規則は、このようなオーバーロードを試すために使用されますが、あいまいさを解消できない場合もあります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-138">Overload resolution rules are used to try and disambiguate between such overloads, although there may be situations in which it is impossible to disambiguate.</span></span> <span data-ttu-id="ac1cf-139">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-139">For example:</span></span>

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

## <a name="scope"></a><span data-ttu-id="ac1cf-140">スコープ</span><span class="sxs-lookup"><span data-stu-id="ac1cf-140">Scope</span></span>

<span data-ttu-id="ac1cf-141">エンティティの名前の*スコープ*は、その名前を修飾なしで参照できるすべての宣言空間のセットです。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-141">The *scope* of an entity's name is the set of all declaration spaces within which it is possible to refer to that name without qualification.</span></span> <span data-ttu-id="ac1cf-142">一般に、エンティティの名前のスコープは、宣言コンテキスト全体です。ただし、エンティティの宣言には、同じ名前のエンティティの入れ子になった宣言が含まれている場合があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-142">In general, the scope of an entity's name is its entire declaration context; however, an entity's declaration may contain nested declarations of entities with the same name.</span></span> <span data-ttu-id="ac1cf-143">その場合、入れ子になったエンティティは、外側のエンティティを*シャドウ*したり非表示にしたりできます。また、シャドウされたエンティティへのアクセスは、限定によってのみ可能です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-143">In that case, the nested entity *shadows*, or hides, the outer entity, and access to the shadowed entity is only possible through qualification.</span></span>

<span data-ttu-id="ac1cf-144">入れ子によるシャドウ処理は、名前空間、または他の型で入れ子にされた型、および型メンバーの本体で入れ子になっている名前空間または型に発生します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-144">Shadowing through nesting occurs in namespaces or types nested within namespaces, in types nested within other types, and in the bodies of type members.</span></span> <span data-ttu-id="ac1cf-145">宣言の入れ子によるシャドウは、常に暗黙的に発生します。明示的な構文は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-145">Shadowing through the nesting of declarations always occurs implicitly; no explicit syntax is required.</span></span>

<span data-ttu-id="ac1cf-146">次の例では、`F` メソッド内で、インスタンス変数 `i` がローカル変数 `i` にシャドウされていますが、`G` メソッド内では、@no__t は引き続きインスタンス変数を参照しています。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-146">In the following example, within the `F` method, the instance variable `i` is shadowed by the local variable `i`, but within the `G` method, `i` still refers to the instance variable.</span></span>

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

<span data-ttu-id="ac1cf-147">内側のスコープ内の名前が外側のスコープ内の名前を非表示にすると、その名前のすべてのオーバーロードされた出現がシャドウされます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-147">When a name in an inner scope hides a name in an outer scope, it shadows all overloaded occurrences of that name.</span></span> <span data-ttu-id="ac1cf-148">次の @no__t 例では、`Inner` で宣言された `F(1)` が呼び出されます。これは、`F` のすべての外部オカレンスが内部宣言によって非表示になるためです。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-148">In the following example, the call `F(1)` invokes the `F` declared in `Inner` because all outer occurrences of `F` are hidden by the inner declaration.</span></span> <span data-ttu-id="ac1cf-149">同じ理由から、-0 @no__t の呼び出しはエラーになります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-149">For the same reason, the call `F("Hello")` is in error.</span></span>

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

## <a name="inheritance"></a><span data-ttu-id="ac1cf-150">継承</span><span class="sxs-lookup"><span data-stu-id="ac1cf-150">Inheritance</span></span>

<span data-ttu-id="ac1cf-151">継承関係とは、1つの型 (*派生*型) が別の型 (*基本*型) から派生したもので、派生型の宣言空間に、アクセス可能な非コンストラクター型メンバーと入れ子にされた型が暗黙的に含まれるようにするためです。その基本型。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-151">An inheritance relationship is one in which one type (the *derived* type) derives from another (the *base* type), such that the derived type's declaration space implicitly contains the accessible non-constructor type members and nested types of its base type.</span></span> <span data-ttu-id="ac1cf-152">次の例では、クラス `A` は `B` の基本クラスであり、`B` は `A` から派生しています。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-152">In the following example, class `A` is the base class of `B`, and `B` is derived from `A`.</span></span>

```vb
Class A
End Class

Class B
    Inherits A
End Class
```

<span data-ttu-id="ac1cf-153">@No__t-0 では基底クラスが明示的に指定されていないため、基本クラスは暗黙的に `Object` になります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-153">Since `A` does not explicitly specify a base class, its base class is implicitly `Object`.</span></span>

<span data-ttu-id="ac1cf-154">継承の重要な側面は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-154">The following are important aspects of inheritance:</span></span>

* <span data-ttu-id="ac1cf-155">継承は推移的です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-155">Inheritance is transitive.</span></span> <span data-ttu-id="ac1cf-156">型*c*が型*b*から派生し、型*b*が型*a*から派生している場合、型*c*は型*b*で宣言された型のメンバーと、型*a*で宣言された型のメンバーを継承します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-156">If type *C* is derived from type *B*, and type *B* is derived from type *A*, type *C* inherits the type members declared in type *B* as well as the type members declared in type *A*.</span></span>

* <span data-ttu-id="ac1cf-157">派生型は、その基本型を拡張しますが、限定することはできません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-157">A derived type extends, but cannot narrow, its base type.</span></span> <span data-ttu-id="ac1cf-158">派生型は新しい型のメンバーを追加でき、継承された型のメンバーをシャドウすることはできますが、継承された型のメンバーの定義を削除することはできません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-158">A derived type can add new type members, and it can shadow inherited type members, but it cannot remove the definition of an inherited type member.</span></span>

* <span data-ttu-id="ac1cf-159">型のインスタンスには、その基本型のすべての型メンバーが含まれているため、常に派生型からその基本型への変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-159">Because an instance of a type contains all of the type members of its base type, a conversion always exists from a derived type to its base type.</span></span>

* <span data-ttu-id="ac1cf-160">型 `Object` 以外のすべての型には基本型が必要です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-160">All types must have a base type, except for the type `Object`.</span></span> <span data-ttu-id="ac1cf-161">したがって、`Object` はすべての型の最終的な基本型であり、すべての型をこの型に変換できます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-161">Thus, `Object` is the ultimate base type of all types, and all types can be converted to it.</span></span>

* <span data-ttu-id="ac1cf-162">派生での循環は許可されていません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-162">Circularity in derivation is not permitted.</span></span> <span data-ttu-id="ac1cf-163">つまり、型 `B` が @no__t 型から派生している場合、型 `A` は、型 @no__t から直接または間接的に派生するエラーになります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-163">That is, when a type `B` derives from a type `A`, it is an error for type `A` to derive directly or indirectly from type `B`.</span></span>

* <span data-ttu-id="ac1cf-164">型は、その中に入れ子にされた型から直接的または間接的に派生することはできません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-164">A type may not directly or indirectly derive from a type nested within it.</span></span>

<span data-ttu-id="ac1cf-165">次の例では、クラスが循環的に依存しているため、コンパイル時エラーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-165">The following example produces a compile-time error because the classes circularly depend on each other.</span></span>

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

<span data-ttu-id="ac1cf-166">次の例では、`B` が、クラス `A` を通じて入れ子になった @no__t クラスから間接的に派生しているため、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-166">The following example also produces a compile-time error because `B` indirectly derives from its nested class `C` through class `A`.</span></span>

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

<span data-ttu-id="ac1cf-167">次の例では、クラス `A` はクラス `B` から派生しないため、エラーは生成されません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-167">The next example does not produce an error because class `A` does not derive from class `B`.</span></span>

```vb
Class A
    Class B
        Inherits A
    End Class 
End Class
```

### <a name="mustinherit-and-notinheritable-classes"></a><span data-ttu-id="ac1cf-168">MustInherit クラスと NotInheritable クラス</span><span class="sxs-lookup"><span data-stu-id="ac1cf-168">MustInherit and NotInheritable Classes</span></span>

<span data-ttu-id="ac1cf-169">@No__t 0 クラスは、基本型としてのみ動作できる不完全な型です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-169">A `MustInherit` class is an incomplete type that can act only as a base type.</span></span> <span data-ttu-id="ac1cf-170">@No__t 0 クラスはインスタンス化できないため、1つに対して `New` 演算子を使用するとエラーになります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-170">A `MustInherit` class cannot be instantiated, so it is an error to use the `New` operator on one.</span></span> <span data-ttu-id="ac1cf-171">@No__t-0 クラスの変数を宣言することは有効です。このような変数に割り当てることができるのは、-1 または @no__t クラスから派生したクラスの値 @no__t だけです。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-171">It is valid to declare variables of `MustInherit` classes; such variables can only be assigned `Nothing` or a value that is of a class derived from the `MustInherit` class.</span></span>

<span data-ttu-id="ac1cf-172">通常のクラスが @no__t 0 クラスから派生した場合、通常のクラスは継承されたすべての `MustOverride` メンバーをオーバーライドする必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-172">When a regular class is derived from a `MustInherit` class, the regular class must override all inherited `MustOverride` members.</span></span> <span data-ttu-id="ac1cf-173">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-173">For example:</span></span>

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

<span data-ttu-id="ac1cf-174">@No__t-0 クラス `A` では、@no__t の2つの @no__t メソッドが導入されています。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-174">The `MustInherit` class `A` introduces a `MustOverride` method `F`.</span></span> <span data-ttu-id="ac1cf-175">クラス `B` には、追加のメソッド `G` が導入されていますが、`F` の実装は提供されていません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-175">Class `B` introduces an additional method `G`, but does not provide an implementation of `F`.</span></span> <span data-ttu-id="ac1cf-176">クラス `B` も `MustInherit` と宣言されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-176">Class `B` must therefore also be declared `MustInherit`.</span></span> <span data-ttu-id="ac1cf-177">クラス `C` は `F` をオーバーライドし、実際の実装を提供します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-177">Class `C` overrides `F` and provides an actual implementation.</span></span> <span data-ttu-id="ac1cf-178">クラス `C` の未処理の @no__t 0 メンバーは存在しないため、`MustInherit` である必要はありません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-178">Since there are no outstanding `MustOverride` members in class `C`, it is not required to be `MustInherit`.</span></span>

<span data-ttu-id="ac1cf-179">@No__t 0 クラスは、別のクラスを派生させることができないクラスです。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-179">A `NotInheritable` class is a class from which another class cannot be derived.</span></span> <span data-ttu-id="ac1cf-180">@no__t 0 のクラスは、意図しない派生を防ぐために主に使用されます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-180">`NotInheritable` classes are primarily used to prevent unintended derivation.</span></span>

<span data-ttu-id="ac1cf-181">この例では、クラス `B` は、`NotInheritable` クラス `A` からの派生を試行するため、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-181">In this example, class `B` is in error because it attempts to derive from the `NotInheritable` class `A`.</span></span> <span data-ttu-id="ac1cf-182">クラスを `MustInherit` と `NotInheritable` の両方に設定することはできません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-182">A class cannot be marked both `MustInherit` and `NotInheritable`.</span></span>

```vb
NotInheritable Class A
End Class

Class B
    ' Error, a class cannot derive from a NotInheritable class.
    Inherits A
End Class
```

### <a name="interfaces-and-multiple-inheritance"></a><span data-ttu-id="ac1cf-183">インターフェイスと多重継承</span><span class="sxs-lookup"><span data-stu-id="ac1cf-183">Interfaces and Multiple Inheritance</span></span>

<span data-ttu-id="ac1cf-184">1つの基本型から派生した他の型とは異なり、インターフェイスは複数の基本インターフェイスから派生できます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-184">Unlike other types, which only derive from a single base type, an interface may derive from multiple base interfaces.</span></span> <span data-ttu-id="ac1cf-185">このため、インターフェイスは、異なる基本インターフェイスから同じ名前を持つ型メンバーを継承できます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-185">Because of this, an interface can inherit an identically named type member from different base interfaces.</span></span> <span data-ttu-id="ac1cf-186">このような場合、派生インターフェイスでは、多重継承された名前を使用できません。また、派生インターフェイスを使用してこれらの型のメンバーを参照すると、シグネチャやオーバーロードに関係なくコンパイル時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-186">In such a case, the multiply-inherited name is not available in the derived interface, and referring to any of those type members through the derived interface causes a compile-time error, regardless of signatures or overloading.</span></span> <span data-ttu-id="ac1cf-187">代わりに、競合する型メンバーは、基本インターフェイス名を使用して参照する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-187">Instead, conflicting type members must be referenced through a base interface name.</span></span>

<span data-ttu-id="ac1cf-188">次の例では、最初の2つのステートメントによってコンパイル時にエラーが発生します。これは、多重継承されたメンバー `Count` がインターフェイス `IListCounter` で使用できないためです。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-188">In the following example, the first two statements cause compile-time errors because the multiply-inherited member `Count` is not available in interface `IListCounter`:</span></span>

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

<span data-ttu-id="ac1cf-189">例に示すように、あいまいさを解決するには `x` を適切な基本インターフェイス型にキャストします。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-189">As illustrated by the example, the ambiguity is resolved by casting `x` to the appropriate base interface type.</span></span> <span data-ttu-id="ac1cf-190">このようなキャストには実行時のコストがありません。コンパイル時にインスタンスを弱い派生型として表示するだけで構成されます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-190">Such casts have no run-time costs; they merely consist of viewing the instance as a less-derived type at compile time.</span></span>

<span data-ttu-id="ac1cf-191">1つの型のメンバーが複数のパスを介して同じ基本インターフェイスから継承された場合、型のメンバーは1回だけ継承されているかのように扱われます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-191">When a single type member is inherited from the same base interface through multiple paths, the type member is treated as if it were only inherited once.</span></span> <span data-ttu-id="ac1cf-192">つまり、派生インターフェイスには、特定の基本インターフェイスから継承された各型メンバーのインスタンスが1つだけ含まれます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-192">In other words, the derived interface only contains one instance of each type member inherited from a particular base interface.</span></span> <span data-ttu-id="ac1cf-193">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-193">For example:</span></span>

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

<span data-ttu-id="ac1cf-194">継承階層を介して1つのパスで型のメンバー名がシャドウされている場合、その名前はすべてのパスでシャドウされます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-194">If a type member name is shadowed in one path through the inheritance hierarchy, then the name is shadowed in all paths.</span></span> <span data-ttu-id="ac1cf-195">次の例では、@no__t 0 のメンバーが `ILeft.F` メンバーによってシャドウされていますが、`IRight` ではシャドウされていません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-195">In the following example, the `IBase.F` member is shadowed by the `ILeft.F` member, but is not shadowed in `IRight`:</span></span>

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

<span data-ttu-id="ac1cf-196">@No__t-0 を選択すると、`IRight` をリードするアクセスパスで `IBase.F` がシャドウされていないように見えるにもかかわらず `ILeft.F` が選択されます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-196">The invocation `d.F(1)` selects `ILeft.F`, even though `IBase.F` appears to not be shadowed in the access path that leads through `IRight`.</span></span> <span data-ttu-id="ac1cf-197">@No__t-0 から `ILeft` までのアクセスパスは `IBase` のシャドウ `IBase.F` であるため、メンバーは、`IDerived` から `IRight` へのアクセスパスにもシャドウされます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-197">Because the access path from `IDerived` to `ILeft` to `IBase` shadows `IBase.F`, the member is also shadowed in the access path from `IDerived` to `IRight` to `IBase`.</span></span>

### <a name="shadowing"></a><span data-ttu-id="ac1cf-198">シャドウ</span><span class="sxs-lookup"><span data-stu-id="ac1cf-198">Shadowing</span></span>

<span data-ttu-id="ac1cf-199">派生型は、継承された型のメンバーの名前を、再宣言することによってシャドウします。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-199">A derived type shadows the name of an inherited type member by re-declaring it.</span></span> <span data-ttu-id="ac1cf-200">名前をシャドウすると、その名前を持つ継承された型のメンバーは削除されません。派生クラスでは、その名前を持つすべての継承された型メンバーを使用できなくなります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-200">Shadowing a name does not remove the inherited type members with that name; it merely makes all of the inherited type members with that name unavailable in the derived class.</span></span> <span data-ttu-id="ac1cf-201">シャドウ宣言には、任意の型のエンティティを指定できます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-201">The shadowing declaration may be any type of entity.</span></span>

<span data-ttu-id="ac1cf-202">オーバーロードできるエンティティは、2つの形式のシャドウのうちの1つを選択できます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-202">Entities than can be overloaded can choose one of two forms of shadowing.</span></span> <span data-ttu-id="ac1cf-203">*名前によるシャドウ*処理は、`Shadows` キーワードを使用して指定されます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-203">*Shadowing by name* is specified using the `Shadows` keyword.</span></span> <span data-ttu-id="ac1cf-204">名前で影を指定するエンティティは、すべてのオーバーロードを含め、基本クラスのその名前のすべてを非表示にします。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-204">An entity that shadows by name hides everything by that name in the base class, including all overloads.</span></span> <span data-ttu-id="ac1cf-205">*名前と署名によるシャドウ*処理は、`Overloads` キーワードを使用して指定されます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-205">*Shadowing by name and signature* is specified using the `Overloads` keyword.</span></span> <span data-ttu-id="ac1cf-206">名前とシグネチャでシャドウするエンティティは、エンティティと同じシグネチャを持つその名前のすべてを非表示にします。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-206">An entity that shadows by name and signature hides everything by that name with the same signature as the entity.</span></span> <span data-ttu-id="ac1cf-207">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-207">For example:</span></span>

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

<span data-ttu-id="ac1cf-208">名前とシグネチャによって @no__t 0 引数を使用してメソッドをシャドウすると、すべての拡張署名ではなく、個々の署名のみが隠蔽されます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-208">Shadowing a method with a `ParamArray` argument by name and signature hides only the individual signature, not all possible expanded signatures.</span></span> <span data-ttu-id="ac1cf-209">これは、シャドウするメソッドのシグネチャが、シャドウされたメソッドの展開されていないシグネチャと一致する場合にも当てはまります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-209">This is true even if the signature of the shadowing method matches the unexpanded signature of the shadowed method.</span></span> <span data-ttu-id="ac1cf-210">次のような例です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-210">The following example:</span></span>

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

<span data-ttu-id="ac1cf-211">`Derived.F` が展開されていない形式の `Base.F` と同じシグネチャを持つ場合でも、@no__t 0 を出力します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-211">prints `Base`, even though `Derived.F` has the same signature as the unexpanded form of `Base.F`.</span></span>

<span data-ttu-id="ac1cf-212">逆に、@no__t 0 引数を持つメソッドは、同じシグネチャを持つメソッドをシャドウするだけで、展開される可能性のあるすべてのシグネチャではありません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-212">Conversely, a method with a `ParamArray` argument only shadows methods with the same signature, not all possible expanded signatures.</span></span> <span data-ttu-id="ac1cf-213">次のような例です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-213">The following example:</span></span>

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

<span data-ttu-id="ac1cf-214">`Derived.F` に `Base.F` と同じシグネチャを持つ拡張フォームがある場合でも、@no__t 0 を出力します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-214">prints `Base`, even though `Derived.F` has an expanded form that has the same signature as `Base.F`.</span></span>

<span data-ttu-id="ac1cf-215">@No__t-0 または `Overloads` が指定されていないシャドウメソッドまたはプロパティは、メソッドまたはプロパティが `Overrides` で宣言されている場合は `Overloads` を、それ以外の場合は @no__t を想定します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-215">A shadowing method or property that does not specify `Shadows` or `Overloads` assumes `Overloads` if the method or property is declared `Overrides`, `Shadows` otherwise.</span></span> <span data-ttu-id="ac1cf-216">オーバーロードされたエンティティのセットの1つのメンバーが `Shadows` または `Overloads` のキーワードを指定している場合は、すべてを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-216">If one member of a set of overloaded entities specifies the `Shadows` or `Overloads` keyword, they all must specify it.</span></span> <span data-ttu-id="ac1cf-217">@No__t-0 および `Overloads` のキーワードを同時に指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-217">The `Shadows` and `Overloads` keywords cannot be specified at the same time.</span></span> <span data-ttu-id="ac1cf-218">@No__t-0 も `Overloads` も、標準モジュールでは指定できません。標準モジュールのメンバーは、`Object` から継承されたメンバーを暗黙的にシャドウします。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-218">Neither `Shadows` nor `Overloads` can be specified in a standard module; members in a standard module implicitly shadow members inherited from `Object`.</span></span>

<span data-ttu-id="ac1cf-219">インターフェイスの継承によって多重継承された型のメンバーの名前をシャドウすることができます (その結果、使用できなくなります)。したがって、派生インターフェイスで名前を使用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-219">It is valid to shadow the name of a type member that has been multiply-inherited through interface inheritance (and which is thereby unavailable), thus making the name available in the derived interface.</span></span>

<span data-ttu-id="ac1cf-220">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-220">For example:</span></span>

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

<span data-ttu-id="ac1cf-221">メソッドは継承されたメソッドをシャドウすることが許可されるため、クラスには、同じシグネチャを持つ複数の @no__t 0 メソッドを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-221">Because methods are allowed to shadow inherited methods, it is possible for a class to contain several `Overridable` methods with the same signature.</span></span> <span data-ttu-id="ac1cf-222">これは、最も派生したメソッドのみが表示されるため、あいまいさの問題はありません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-222">This does not present an ambiguity problem, since only the most-derived method is visible.</span></span> <span data-ttu-id="ac1cf-223">次の例では、`C` および `D` クラスに、同じシグネチャを持つ2つの @no__t 2 つのメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-223">In the following example, the `C` and `D` classes contain two `Overridable` methods with the same signature:</span></span>

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

<span data-ttu-id="ac1cf-224">ここでは @no__t 0 のメソッドが2つあります。1つはクラス `A`、もう1つはクラス `C` で導入されています。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-224">There are two `Overridable` methods here: one introduced by class `A` and the one introduced by class `C`.</span></span> <span data-ttu-id="ac1cf-225">クラス `C` によって導入されたメソッドは、クラス @no__t から継承されたメソッドを非表示にします。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-225">The method introduced by class `C` hides the method inherited from class `A`.</span></span> <span data-ttu-id="ac1cf-226">したがって、クラス @no__t の @no__t 0 宣言は、クラス `C` によって導入されたメソッドをオーバーライドします。クラス `D` では、クラス @no__t によって導入されたメソッドをオーバーライドすることはできません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-226">Thus, the `Overrides` declaration in class `D` overrides the method introduced by class `C`, and it is not possible for class `D` to override the method introduced by class `A`.</span></span> <span data-ttu-id="ac1cf-227">この例では、次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-227">The example produces the output:</span></span>

```console
B.F
B.F
D.F
D.F
```

<span data-ttu-id="ac1cf-228">メソッドが非表示になっていない派生型を使用して、クラス `D` のインスタンスにアクセスすることにより、非表示の `Overridable` メソッドを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-228">It is possible to invoke the hidden `Overridable` method by accessing an instance of class `D` through a less-derived type in which the method is not hidden.</span></span>

<span data-ttu-id="ac1cf-229">@No__t 0 のメソッドをシャドウすることは無効です。ほとんどの場合、クラスは使用できなくなります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-229">It is not valid to shadow a `MustOverride` method, because in most cases this would make the class unusable.</span></span> <span data-ttu-id="ac1cf-230">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-230">For example:</span></span>

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

<span data-ttu-id="ac1cf-231">この場合、`MustOverride` メソッド `Base.F` をオーバーライドするには、クラス `MoreDerived` が必要ですが、クラス @no__t-@no__t 3 の場合は-4 が使用できません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-231">In this case, the class `MoreDerived` is required to override the `MustOverride` method `Base.F`, but because the class `Derived` shadows `Base.F`, this is not possible.</span></span> <span data-ttu-id="ac1cf-232">@No__t-0 の有効な子孫を宣言することはできません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-232">There is no way to declare a valid descendent of `Derived`.</span></span>

<span data-ttu-id="ac1cf-233">外側のスコープから名前をシャドウする場合とは異なり、次の例のように、継承されたスコープからアクセス可能な名前をシャドウすると、警告が報告されます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-233">In contrast to shadowing a name from an outer scope, shadowing an accessible name from an inherited scope causes a warning to be reported, as in the following example:</span></span>

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

<span data-ttu-id="ac1cf-234">クラス `Derived` のメソッド `F` の宣言により、警告が報告されます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-234">The declaration of method `F` in class `Derived` causes a warning to be reported.</span></span> <span data-ttu-id="ac1cf-235">継承された名前のシャドウは、基本クラスの個別の進化を妨げるため、特にエラーではありません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-235">Shadowing an inherited name is specifically not an error, since that would preclude separate evolution of base classes.</span></span> <span data-ttu-id="ac1cf-236">たとえば、より新しいバージョンのクラス `Base` では、クラスの以前のバージョンには存在しないメソッド `F` が導入されたため、上記の状況が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-236">For example, the above situation might have come about because a later version of class `Base` introduced a method `F` that was not present in an earlier version of the class.</span></span> <span data-ttu-id="ac1cf-237">上記の状況でエラーが発生した場合、個別にバージョン管理されたクラスライブラリの基底クラス*に変更が*加えられると、派生クラスが無効になる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-237">Had the above situation been an error, *any* change made to a base class in a separately versioned class library could potentially cause derived classes to become invalid.</span></span>

<span data-ttu-id="ac1cf-238">継承された名前のシャドウによって発生した警告は、`Shadows` または `Overloads` 修飾子を使用して削除できます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-238">The warning caused by shadowing an inherited name can be eliminated through use of the `Shadows` or `Overloads` modifier:</span></span>

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

<span data-ttu-id="ac1cf-239">@No__t-0 修飾子は、継承されたメンバーをシャドウする目的を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-239">The `Shadows` modifier indicates the intention to shadow the inherited member.</span></span> <span data-ttu-id="ac1cf-240">シャドウする型のメンバー名がない場合、`Shadows` または `Overloads` 修飾子を指定してもエラーにはなりません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-240">It is not an error to specify the `Shadows` or `Overloads` modifier if there is no type member name to shadow.</span></span>

<span data-ttu-id="ac1cf-241">新しいメンバーの宣言は、次の例に示すように、新しいメンバーのスコープ内でのみ継承されるメンバーをシャドウします。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-241">A declaration of a new member shadows an inherited member only within the scope of the new member, as in the following example:</span></span>

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

<span data-ttu-id="ac1cf-242">上の例では、クラス `Derived` のメソッド `F` の宣言によって、クラス `Base` から継承されたメソッド `F` がシャドウされますが、クラス `Derived` の新しいメソッド @no__t は @no__t 6 にアクセスするため、そのスコープはクラス `MoreDerived` に拡張されません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-242">In the example above, the declaration of method `F` in class `Derived` shadows the method `F` that was inherited from class `Base`, but since the new method `F` in class `Derived` has `Private` access, its scope does not extend to class `MoreDerived`.</span></span> <span data-ttu-id="ac1cf-243">したがって、`MoreDerived.G` の `F()` は有効で、`Base.F` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-243">Thus, the call `F()` in `MoreDerived.G` is valid and will invoke `Base.F`.</span></span> <span data-ttu-id="ac1cf-244">オーバーロードされた型のメンバーの場合は、オーバーロードされた型のメンバーのセット全体が、シャドウの目的で最も制限のないアクセス権を持つものとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-244">In the case of overloaded type members, the entire set of overloaded type members is treated as if they all had the most permissive access for the purposes of shadowing.</span></span>

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

<span data-ttu-id="ac1cf-245">この例では、`Derived` の `F()` の宣言が @no__t 2 のアクセスで宣言されていますが、オーバーロードされた `F(Integer)` は @no__t 4 のアクセスで宣言されています。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-245">In this example, even though the declaration of `F()` in `Derived` is declared with `Private` access, the overloaded `F(Integer)` is declared with `Public` access.</span></span> <span data-ttu-id="ac1cf-246">このため、シャドウを行うために、`Derived` の @no__t の名前は、@no__t が2であるかのように処理されるため、両方のメソッドが `Base` でシャドウ `F` になります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-246">Therefore, for the purpose of shadowing, the name `F` in `Derived` is treated as if it was `Public`, so both methods shadow `F` in `Base`.</span></span>

## <a name="implementation"></a><span data-ttu-id="ac1cf-247">実装</span><span class="sxs-lookup"><span data-stu-id="ac1cf-247">Implementation</span></span>

<span data-ttu-id="ac1cf-248">*実装*関係は、型がインターフェイスを実装することを宣言し、型がインターフェイスのすべての型メンバーを実装する場合に存在します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-248">An *implementation* relationship exists when a type declares that it implements an interface and the type implements all the type members of the interface.</span></span> <span data-ttu-id="ac1cf-249">特定のインターフェイスを実装する型は、そのインターフェイスに変換できます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-249">A type that implements a particular interface is convertible to that interface.</span></span> <span data-ttu-id="ac1cf-250">インターフェイスをインスタンス化することはできませんが、インターフェイスの変数を宣言することは有効です。このような変数には、インターフェイスを実装するクラスの値のみを割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-250">Interfaces cannot be instantiated, but it is valid to declare variables of interfaces; such variables can only be assigned a value that is of a class that implements the interface.</span></span> <span data-ttu-id="ac1cf-251">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-251">For example:</span></span>

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

<span data-ttu-id="ac1cf-252">多重継承型メンバーを持つインターフェイスを実装する型は、実装されている派生インターフェイスから直接アクセスできない場合でも、これらのメソッドを実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-252">A type implementing an interface with multiply-inherited type members must still implement those methods, even though they cannot be accessed directly from the derived interface being implemented.</span></span> <span data-ttu-id="ac1cf-253">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-253">For example:</span></span>

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

<span data-ttu-id="ac1cf-254">@No__t-0 のクラスでも、実装されたインターフェイスのすべてのメンバーの実装を提供する必要があります。ただし、これらのメソッドを `MustOverride` として宣言することで、これらのメソッドの実装を遅らせることができます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-254">Even `MustInherit` classes must provide implementations of all the members of implemented interfaces; however, they can defer implementation of these methods by declaring them as `MustOverride`.</span></span> <span data-ttu-id="ac1cf-255">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-255">For example:</span></span>

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

<span data-ttu-id="ac1cf-256">型は、基本型が実装するインターフェイスを再実装することを選択できます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-256">A type may choose to re-implement an interface that its base type implements.</span></span> <span data-ttu-id="ac1cf-257">インターフェイスを再実装するには、型がインターフェイスを実装することを明示的に示す必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-257">To re-implement the interface, the type must explicitly state that it implements the interface.</span></span> <span data-ttu-id="ac1cf-258">インターフェイスを再実装する型は、インターフェイスのメンバーの一部だけを再実装することを選択できます。再実装されていないメンバーは、基本型の実装を引き続き使用します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-258">A type re-implementing an interface may choose to re-implement only some, but not all, of the members of the interface -- any members not re-implemented continue to use the base type's implementation.</span></span> <span data-ttu-id="ac1cf-259">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-259">For example:</span></span>

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

<span data-ttu-id="ac1cf-260">次の例では、が出力されます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-260">This example prints:</span></span>

```console
TestDerived.DerivedTest1
TestBase.Test2
```

<span data-ttu-id="ac1cf-261">派生型が派生型の基本型によって実装された基本インターフェイスを持つインターフェイスを実装する場合、派生型は、基本型によって実装されていないインターフェイスの型のメンバーのみを実装することを選択できます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-261">When a derived type implements an interface whose base interfaces are implemented by the derived type's base types, the derived type can choose to only implement the interface's type members that are not already implemented by the base types.</span></span> <span data-ttu-id="ac1cf-262">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-262">For example:</span></span>

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

<span data-ttu-id="ac1cf-263">インターフェイスメソッドは、基本型のオーバーライド可能なメソッドを使用して実装することもできます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-263">An interface method can also be implemented using an overridable method in a base type.</span></span> <span data-ttu-id="ac1cf-264">その場合、派生型は、オーバーライド可能なメソッドをオーバーライドし、インターフェイスの実装を変更することもできます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-264">In that case, a derived type may also override the overridable method and alter the implementation of the interface.</span></span> <span data-ttu-id="ac1cf-265">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-265">For example:</span></span>

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

### <a name="implementing-methods"></a><span data-ttu-id="ac1cf-266">メソッドの実装</span><span class="sxs-lookup"><span data-stu-id="ac1cf-266">Implementing Methods</span></span>

<span data-ttu-id="ac1cf-267">型は、`Implements` 句を持つメソッドを指定することによって、実装されたインターフェイスの型のメンバーを*実装*します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-267">A type *implements* a type member of an implemented interface by supplying a method with an `Implements` clause.</span></span> <span data-ttu-id="ac1cf-268">2つの型のメンバーは同じ数のパラメーターを持つ必要があります。パラメーターのすべての型と修飾子が一致している必要があります。これには、省略可能なパラメーターの既定値、戻り値の型、およびメソッドのパラメーターに対するすべての制約が一致している必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-268">The two type members must have the same number of parameters, all of the types and modifiers of the parameters must match, including the default value of optional parameters, the return type must match, and all of the constraints on method parameters must match.</span></span> <span data-ttu-id="ac1cf-269">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-269">For example:</span></span>

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

<span data-ttu-id="ac1cf-270">1つのメソッドが、上記の条件を満たしていれば、任意の数のインターフェイス型メンバーを実装できます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-270">A single method may implement any number of interface type members if they all meet the above criteria.</span></span> <span data-ttu-id="ac1cf-271">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-271">For example:</span></span>

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

<span data-ttu-id="ac1cf-272">ジェネリックインターフェイスでメソッドを実装する場合、実装するメソッドは、インターフェイスの型パラメーターに対応する型引数を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-272">When implementing a method in a generic interface, the implementing method must supply the type arguments that correspond to the interface's type parameters.</span></span> <span data-ttu-id="ac1cf-273">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-273">For example:</span></span>

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

<span data-ttu-id="ac1cf-274">ジェネリックインターフェイスは、型引数のセットに対して実装できない可能性があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-274">Note that it is possible that a generic interface may not be implementable for some set of type arguments.</span></span>

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

## <a name="polymorphism"></a><span data-ttu-id="ac1cf-275">ポリモーフィズム</span><span class="sxs-lookup"><span data-stu-id="ac1cf-275">Polymorphism</span></span>

<span data-ttu-id="ac1cf-276">*ポリモーフィズム*は、メソッドまたはプロパティの実装を変更する機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-276">*Polymorphism* provides the ability to vary the implementation of a method or property.</span></span> <span data-ttu-id="ac1cf-277">ポリモーフィズムでは、同じメソッドまたはプロパティは、それを呼び出すインスタンスの実行時の型に応じて、異なるアクションを実行できます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-277">With polymorphism, the same method or property can perform different actions depending on the run-time type of the instance that invokes it.</span></span> <span data-ttu-id="ac1cf-278">ポリモーフィックなメソッドまたはプロパティは、 *overridable*と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-278">Methods or properties that are polymorphic are called *overridable*.</span></span> <span data-ttu-id="ac1cf-279">これに対して、オーバーライド不可能なメソッドまたはプロパティの実装は不変です。実装は、メソッドまたはプロパティが宣言されているクラスのインスタンスまたは派生クラスのインスタンスで呼び出されているかどうかと同じです。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-279">By contrast, the implementation of a non-overridable method or property is invariant; the implementation is the same whether the method or property is invoked on an instance of the class in which it is declared or an instance of a derived class.</span></span> <span data-ttu-id="ac1cf-280">オーバーライドできないメソッドまたはプロパティが呼び出されると、インスタンスのコンパイル時の型が決定要因になります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-280">When a non-overridable method or property is invoked, the compile-time type of the instance is the determining factor.</span></span> <span data-ttu-id="ac1cf-281">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-281">For example:</span></span>

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

<span data-ttu-id="ac1cf-282">オーバーライド可能なメソッドは @no__t 0 にすることもできます。これは、メソッドの本体を提供せず、オーバーライドする必要があることを意味します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-282">An overridable method may also be `MustOverride`, which means that it provides no method body and must be overridden.</span></span> <span data-ttu-id="ac1cf-283">`MustOverride` メソッドは、`MustInherit` クラスでのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-283">`MustOverride` methods are only allowed in `MustInherit` classes.</span></span>

<span data-ttu-id="ac1cf-284">次の例では、クラス `Shape` によって、描画に使用できる幾何学図形オブジェクトの抽象概念が定義されています。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-284">In the following example, the class `Shape` defines the abstract notion of a geometrical shape object that can paint itself:</span></span>

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

<span data-ttu-id="ac1cf-285">@No__t-0 メソッドは、意味のある既定の実装がないため、-1 @no__t です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-285">The `Paint` method is `MustOverride` because there is no meaningful default implementation.</span></span> <span data-ttu-id="ac1cf-286">@No__t-0 および `Box` クラスは、具体的な `Shape` 実装です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-286">The `Ellipse` and `Box` classes are concrete `Shape` implementations.</span></span> <span data-ttu-id="ac1cf-287">これらのクラスは @no__t 0 ではないため、`Paint` メソッドをオーバーライドし、実際の実装を提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-287">Because these classes are not `MustInherit`, they are required to override the `Paint` method and provide an actual implementation.</span></span>

<span data-ttu-id="ac1cf-288">次の例に示すように、ベースアクセスで @no__t 0 メソッドを参照すると、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-288">It is an error for a base access to reference a `MustOverride` method, as the following example demonstrates:</span></span>

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

<span data-ttu-id="ac1cf-289">@No__t-1 メソッドを参照しているため、`MyBase.F()` 呼び出しに対してエラーが報告されます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-289">An error is reported for the `MyBase.F()` invocation because it references a `MustOverride` method.</span></span>

### <a name="overriding-methods"></a><span data-ttu-id="ac1cf-290">オーバーライド (メソッドを)</span><span class="sxs-lookup"><span data-stu-id="ac1cf-290">Overriding Methods</span></span>

<span data-ttu-id="ac1cf-291">型は、同じ名前およびシグネチャを持つメソッドを宣言し、`Overrides` 修飾子で宣言をマークすることによって、継承されたオーバーライド可能なメソッドを*オーバーライド*できます。メソッドのオーバーライドには、次に示す追加の要件があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-291">A type may *override* an inherited overridable method by declaring a method with the same name and , signature, and marking the declaration with the `Overrides` modifier.There are additional requirements on overriding methods, listed below.</span></span> <span data-ttu-id="ac1cf-292">@No__t-0 メソッド宣言によって新しいメソッドが導入されるのに対し、メソッドの継承された実装は、`Overrides` メソッドの宣言に置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-292">Whereas an `Overridable` method declaration introduces a new method, an `Overrides` method declaration replaces the inherited implementation of the method.</span></span>

<span data-ttu-id="ac1cf-293">オーバーライドするメソッドは、`NotOverridable` として宣言できます。これにより、派生型のメソッドをさらにオーバーライドできなくなります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-293">An overriding method may be declared `NotOverridable`, which prevents any further overriding of the method in derived types.</span></span> <span data-ttu-id="ac1cf-294">実際には、@no__t 0 のメソッドは、それ以降の派生クラスではオーバーライド不可能になります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-294">In effect, `NotOverridable` methods become non-overridable in any further derived classes.</span></span>

<span data-ttu-id="ac1cf-295">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-295">Consider the following example:</span></span>

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

<span data-ttu-id="ac1cf-296">この例では、クラス `B` は2つの @no__t のメソッドを提供します。1つは `NotOverridable` の修飾子を持ち、@no__t もう1つは @no__t ないメソッドです。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-296">In the example, class `B` provides two `Overrides` methods: a method `F` that has the `NotOverridable` modifier and a method `G` that does not.</span></span> <span data-ttu-id="ac1cf-297">@No__t-0 修飾子を使用すると、クラス `C` は、メソッド `F` をさらにオーバーライドできなくなります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-297">Use of the `NotOverridable` modifier prevents class `C` from further overriding method `F`.</span></span>

<span data-ttu-id="ac1cf-298">オーバーライドするメソッドは、オーバーライドするメソッドが-1 @no__t 宣言されていない場合でも、`MustOverride` として宣言できます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-298">An overriding method may also be declared `MustOverride`, even if the method that it is overriding is not declared `MustOverride`.</span></span> <span data-ttu-id="ac1cf-299">これを行うには、含んでいるクラスが-0 として @no__t 宣言されている必要があります。また、-1 @no__t 宣言されていないその他の派生クラスは、メソッドをオーバーライドする必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-299">This requires that the containing class be declared `MustInherit` and that any further derived classes that are not declared `MustInherit` must override the method.</span></span> <span data-ttu-id="ac1cf-300">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-300">For example:</span></span>

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

<span data-ttu-id="ac1cf-301">この例では、クラス `B` は、`MustOverride` メソッドを使用して `A.F` をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-301">In the example, class `B` overrides `A.F` with a `MustOverride` method.</span></span> <span data-ttu-id="ac1cf-302">これは、`B` から派生したすべてのクラスが `MustInherit` と宣言されている場合を除き、`F` をオーバーライドする必要があることを意味します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-302">This means that any classes derived from `B` will have to override `F`, unless they are declared `MustInherit` as well.</span></span>

<span data-ttu-id="ac1cf-303">次のすべてがオーバーライドメソッドに当てはまる場合を除き、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-303">A compile-time error occurs unless all of the following are true of an overriding method:</span></span>

* <span data-ttu-id="ac1cf-304">宣言コンテキストには、オーバーライドするメソッドと同じシグネチャと戻り値の型 (存在する場合) を持つ、アクセス可能な単一の継承メソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-304">The declaration context contains a single accessible inherited method with the same signature and return type (if any) as the overriding method.</span></span>
* <span data-ttu-id="ac1cf-305">オーバーライドされる継承メソッドはオーバーライド可能です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-305">The inherited method being overridden is overridable.</span></span> <span data-ttu-id="ac1cf-306">つまり、継承されたメソッドがオーバーライドされると、`Shared` または `NotOverridable` ではありません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-306">In other words, the inherited method being overridden is not `Shared` or `NotOverridable`.</span></span>
* <span data-ttu-id="ac1cf-307">宣言されるメソッドのアクセシビリティドメインは、オーバーライドされる継承メソッドのアクセシビリティドメインと同じです。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-307">The accessibility domain of the method being declared is the same as the accessibility domain of the inherited method being overridden.</span></span> <span data-ttu-id="ac1cf-308">例外が1つあります。他のメソッドが別のアセンブリ内にあり、オーバーライドする側のメソッドがに @no__t 2 のアクセス権を持っていない場合、`Protected` メソッドによって @no__t 0 メソッドがオーバーライドされる必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-308">There is one exception: a `Protected Friend` method must be overridden by a `Protected` method if the other method is in another assembly that the overriding method does not have `Friend` access to.</span></span>
* <span data-ttu-id="ac1cf-309">オーバーライドするメソッドのパラメーターは、省略可能なパラメーターに指定された値を含む `ByVal`、`ByRef`、`ParamArray,` および `Optional` の各修飾子の使用に関して、オーバーライドされたメソッドのパラメーターと一致します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-309">The parameters of the overriding method match the overridden method's parameters in regards to usage of the `ByVal`, `ByRef`, `ParamArray,` and `Optional` modifiers, including the values provided for optional parameters.</span></span>
* <span data-ttu-id="ac1cf-310">オーバーライドするメソッドの型パラメーターは、型制約に関してオーバーライドされたメソッドの型パラメーターと一致します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-310">The type parameters of the overriding method match the overridden method's type parameters in regards to type constraints.</span></span>

<span data-ttu-id="ac1cf-311">基本ジェネリック型のメソッドをオーバーライドする場合、オーバーライドする側のメソッドは、基本型のパラメーターに対応する型引数を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-311">When overriding a method in a base generic type, the overriding method must supply the type arguments that correspond to the base type parameters.</span></span> <span data-ttu-id="ac1cf-312">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-312">For example:</span></span>

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

<span data-ttu-id="ac1cf-313">ジェネリッククラスのオーバーライド可能なメソッドは、型引数のセットに対してオーバーライドできない可能性があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-313">Note that it is possible that an overridable method in a generic class may not be able to be overridden for some sets of type arguments.</span></span> <span data-ttu-id="ac1cf-314">メソッドが `MustOverride` として宣言されている場合は、一部の継承チェーンができないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-314">If the method is declared `MustOverride`, this means that some inheritance chains may not be possible.</span></span> <span data-ttu-id="ac1cf-315">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-315">For example:</span></span>

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

<span data-ttu-id="ac1cf-316">オーバーライド宣言は、次の例のように、ベースアクセスを使用してオーバーライドされた基本メソッドにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-316">An override declaration can access the overridden base method using a base access, as in the following example:</span></span>

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

この例では、クラス `Derived` の `MyBase.PrintVariables()` を呼び出すと、クラス `Base` で宣言された `PrintVariables` メソッドが呼び出されます。 基本アクセスは、オーバーライド可能な呼び出し機構を無効にし、単に基本メソッドをオーバーライド不可能なメソッドとして扱います。 <span data-ttu-id="ac1cf-319">@No__t 0 での呼び出しが-1 @no__t 書き込まれている場合、`Base` で宣言されているものではなく、`Derived` で宣言されている `PrintVariables` メソッドを再帰的に呼び出します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-319">Had the invocation in `Derived` been written `CType(Me, Base).PrintVariables()`, it would recursively invoke the `PrintVariables` method declared in `Derived`, not the one declared in `Base`.</span></span>

<span data-ttu-id="ac1cf-320">@No__t 0 修飾子を含む場合にのみ、メソッドが別のメソッドをオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-320">Only when it includes an `Overrides` modifier can a method override another method.</span></span> <span data-ttu-id="ac1cf-321">それ以外の場合、継承されたメソッドと同じシグネチャを持つメソッドは、次の例のように、継承されたメソッドを単純にします。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-321">In all other cases, a method with the same signature as an inherited method simply shadows the inherited method, as in the example below:</span></span>

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

<span data-ttu-id="ac1cf-322">この例では、クラス `Derived` のメソッド `F` は `Overrides` 修飾子を含まないため、クラス @no__t のメソッド `F` はオーバーライドされません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-322">In the example, the method `F` in class `Derived` does not include an `Overrides` modifier and therefore does not override method `F` in class `Base`.</span></span> <span data-ttu-id="ac1cf-323">代わりに、クラス `Derived` のメソッド `F` はクラス `Base` のメソッドをシャドウし、宣言には `Shadows` または `Overloads` 修飾子が含まれていないため、警告が報告されます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-323">Rather, method `F` in class `Derived` shadows the method in class `Base`, and a warning is reported because the declaration does not include a `Shadows` or `Overloads` modifier.</span></span>

<span data-ttu-id="ac1cf-324">次の例では、クラス `Derived` のメソッド `F` は、クラス `Base` から継承されたオーバーライド可能なメソッド @no__t をシャドウします。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-324">In the following example, method `F` in class `Derived` shadows the overridable method `F` inherited from class `Base`:</span></span>

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

<span data-ttu-id="ac1cf-325">クラス `Derived` の新しいメソッド `F` は @no__t 2 にアクセスしているため、そのスコープには `Derived` のクラス本体のみが含まれ、クラス @no__t には拡張されません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-325">Since the new method `F` in class `Derived` has `Private` access, its scope only includes the class body of `Derived` and does not extend to class `MoreDerived`.</span></span> <span data-ttu-id="ac1cf-326">このため、クラス `MoreDerived` のメソッド `F` の宣言では、クラス `Base` から継承されたメソッド @no__t をオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-326">The declaration of method `F` in class `MoreDerived` is therefore permitted to override the method `F` inherited from class `Base`.</span></span>

<span data-ttu-id="ac1cf-327">@No__t 0 のメソッドが呼び出されると、インスタンスの型に基づいて、インスタンスメソッドの最も派生した実装が呼び出されます。これは、呼び出しが基底クラスのメソッドに対して行われるか、派生クラスで行われるかには関係ありません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-327">When an `Overridable` method is invoked, the most derived implementation of the instance method is called, based on the type of the instance, regardless of whether the call is to the method in the base class or the derived class.</span></span> <span data-ttu-id="ac1cf-328">クラス @no__t @no__t に関して、@no__t 0 メソッドの最も派生する実装は、次のように決定されます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-328">The most derived implementation of an `Overridable` method `M` with respect to a class `R` is determined as follows:</span></span>

* <span data-ttu-id="ac1cf-329">@No__t-0 に `M` の導入 @no__t の宣言が含まれている場合、これは `M` の最も派生した実装です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-329">If `R` contains the introducing `Overridable` declaration of `M`, this is the most derived implementation of `M`.</span></span>

* <span data-ttu-id="ac1cf-330">それ以外の場合、`R` に `M` のオーバーライドが含まれていると、これが `M` の最も派生した実装になります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-330">Otherwise, if `R` contains an override of `M`, this is the most derived implementation of `M`.</span></span>

* <span data-ttu-id="ac1cf-331">それ以外の場合、`M` の最も派生する実装は、`R` の直接基底クラスの実装と同じです。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-331">Otherwise, the most derived implementation of `M` is the same as that of the direct base class of `R`.</span></span>

## <a name="accessibility"></a><span data-ttu-id="ac1cf-332">ユーザー補助</span><span class="sxs-lookup"><span data-stu-id="ac1cf-332">Accessibility</span></span>

<span data-ttu-id="ac1cf-333">宣言は、宣言するエンティティの*アクセシビリティ*を指定します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-333">A declaration specifies the *accessibility* of the entity it declares.</span></span> <span data-ttu-id="ac1cf-334">エンティティのアクセシビリティでは、エンティティの名前のスコープは変更されません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-334">An entity's accessibility does not change the scope of an entity's name.</span></span> <span data-ttu-id="ac1cf-335">宣言の*アクセシビリティドメイン*は、宣言されたエンティティにアクセスできるすべての宣言空間のセットです。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-335">The *accessibility domain* of a declaration is the set of all declaration spaces in which the declared entity is accessible.</span></span>

<span data-ttu-id="ac1cf-336">5つのアクセスの種類は `Public`、`Protected`、`Friend`、`Protected Friend`、および `Private` です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-336">The five access types are `Public`, `Protected`, `Friend`, `Protected Friend`, and `Private`.</span></span> <span data-ttu-id="ac1cf-337">`Public` は最も制限の厳しいアクセスの種類であり、他の4つの種類はすべて `Public` のサブセットです。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-337">`Public` is the most permissive access type, and the four other types are all subsets of `Public`.</span></span> <span data-ttu-id="ac1cf-338">最も制限の緩いアクセスの種類は 0 @no__t で、他の4つのアクセスの種類はすべて `Private` のスーパーセットになります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-338">The least permissive access type is `Private`, and the four other access types are all supersets of `Private`.</span></span>

```antlr
AccessModifier
    : 'Public'
    | 'Protected'
    | 'Friend'
    | 'Private'
    | 'Protected' 'Friend'
    ;
```

<span data-ttu-id="ac1cf-339">宣言のアクセスの種類は、オプションのアクセス修飾子を使用して指定します。これは、@no__t 0、`Protected`、`Friend`、`Private`、または `Protected` と `Friend` の組み合わせにすることができます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-339">The access type for a declaration is specified via an optional access modifier, which can be `Public`, `Protected`, `Friend`, `Private`, or the combination of `Protected` and `Friend`.</span></span> <span data-ttu-id="ac1cf-340">アクセス修飾子が指定されていない場合、既定のアクセスの種類は宣言コンテキストに依存します。許可されるアクセスの種類は、宣言のコンテキストにも依存します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-340">If no access modifier is specified, the default access type depends on the declaration context; the permitted access types also depend on the declaration context.</span></span>

* <span data-ttu-id="ac1cf-341">@No__t-0 修飾子を使用して宣言されたエンティティは、@no__t に1つのアクセス権を持ちます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-341">Entities declared with the `Public` modifier have `Public` access.</span></span> <span data-ttu-id="ac1cf-342">@No__t-0 エンティティの使用に関する制限はありません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-342">There are no restrictions on the use of `Public` entities.</span></span>

* <span data-ttu-id="ac1cf-343">@No__t-0 修飾子を使用して宣言されたエンティティは、@no__t に1つのアクセス権を持ちます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-343">Entities declared with the `Protected` modifier have `Protected` access.</span></span> <span data-ttu-id="ac1cf-344">`Protected` アクセスを指定できるのは、クラスのメンバー (通常の型メンバーと入れ子になったクラスの両方)、または標準モジュールおよび構造体の `Overridable` メンバー (定義上、`System.Object` または @no__t から継承する必要があります) のメンバーだけです。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-344">`Protected` access can only be specified on members of classes (both regular type members and nested classes) or on `Overridable` members of standard modules and structures (which must, by definition, be inherited from `System.Object` or `System.ValueType`).</span></span> <span data-ttu-id="ac1cf-345">メンバーがインスタンスメンバーでない場合、または派生クラスのインスタンスを介してアクセスが行われる場合は、派生クラスに @no__t 0 のメンバーにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-345">A `Protected` member is accessible to a derived class, provided that either the member is not an instance member, or the access takes place through an instance of the derived class.</span></span> <span data-ttu-id="ac1cf-346">`Protected` アクセスは `Friend` アクセスのスーパーセットではありません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-346">`Protected` access is not a superset of `Friend` access.</span></span>

* <span data-ttu-id="ac1cf-347">@No__t-0 修飾子を使用して宣言されたエンティティは、@no__t に1つのアクセス権を持ちます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-347">Entities declared with the `Friend` modifier have `Friend` access.</span></span> <span data-ttu-id="ac1cf-348">@No__t 0 のアクセス権を持つエンティティは、エンティティ宣言を含むプログラム内、または `System.Runtime.CompilerServices.InternalsVisibleToAttribute` 属性で `Friend` アクセスが与えられているすべてのアセンブリを含むプログラム内でのみアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-348">An entity with `Friend` access is accessible only within the program that contains the entity declaration or any assemblies that have been given `Friend` access through the `System.Runtime.CompilerServices.InternalsVisibleToAttribute` attribute.</span></span>

* <span data-ttu-id="ac1cf-349">@No__t-0 修飾子で宣言されたエンティティは、`Protected` と @no__t 2 のアクセスの和集合を持ちます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-349">Entities declared with the `Protected Friend` modifiers have the union of `Protected` and `Friend` access.</span></span>

* <span data-ttu-id="ac1cf-350">@No__t-0 修飾子を使用して宣言されたエンティティは、@no__t に1つのアクセス権を持ちます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-350">Entities declared with the `Private` modifier have `Private` access.</span></span> <span data-ttu-id="ac1cf-351">@No__t 0 エンティティは、入れ子になったエンティティも含め、その宣言コンテキスト内でのみアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-351">A `Private` entity is accessible only within its declaration context, including any nested entities.</span></span>

<span data-ttu-id="ac1cf-352">宣言のアクセシビリティは、宣言コンテキストのアクセシビリティに依存しません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-352">The accessibility in a declaration does not depend on the accessibility of the declaration context.</span></span> <span data-ttu-id="ac1cf-353">たとえば、`Private` アクセスで宣言された型には、@no__t が1つのアクセス権を持つ型のメンバーが含まれる場合があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-353">For example, a type declared with `Private` access may contain a type member with `Public` access.</span></span>

<span data-ttu-id="ac1cf-354">次のコードは、さまざまなアクセシビリティドメインを示しています。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-354">The following code demonstrates various accessibility domains:</span></span>

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

<span data-ttu-id="ac1cf-355">この例のクラスとメンバーには、次のアクセシビリティドメインがあります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-355">The classes and members in this example have the following accessibility domains:</span></span>

* <span data-ttu-id="ac1cf-356">@No__t-0 および `A.X` のアクセシビリティドメインは無制限です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-356">The accessibility domain of `A` and `A.X` is unlimited.</span></span>

* <span data-ttu-id="ac1cf-357">@No__t-0、`B`、`B.X`、`B.Y`、`B.C`、`B.C.X`、および `B.C.Y` のアクセシビリティドメインは、含まれているプログラムです。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-357">The accessibility domain of `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`, and `B.C.Y` is the containing program.</span></span>

* <span data-ttu-id="ac1cf-358">@No__t-0 のアクセシビリティドメインが `A.`</span><span class="sxs-lookup"><span data-stu-id="ac1cf-358">The accessibility domain of `A.Z` is `A.`</span></span>

* <span data-ttu-id="ac1cf-359">@No__t-0、`B.D`、`B.D.X`、および `B.D.Y` のアクセシビリティドメインは、`B.C` および @no__t を含む `B` です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-359">The accessibility domain of `B.Z`, `B.D`, `B.D.X`, and `B.D.Y` is `B`, including `B.C` and `B.D`.</span></span>

* <span data-ttu-id="ac1cf-360">@No__t-0 のアクセシビリティドメインは `B.C` です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-360">The accessibility domain of `B.C.Z` is `B.C`.</span></span>

* <span data-ttu-id="ac1cf-361">@No__t-0 のアクセシビリティドメインは `B.D` です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-361">The accessibility domain of `B.D.Z` is `B.D`.</span></span>

<span data-ttu-id="ac1cf-362">例に示すように、メンバーのアクセシビリティドメインは、それを含んでいる型のアクセシビリティドメインよりも大きくなることはありません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-362">As the example illustrates, the accessibility domain of a member is never larger than that of a containing type.</span></span> <span data-ttu-id="ac1cf-363">たとえば、すべての @no__t 0 のメンバー @no__t がアクセシビリティとして宣言されている場合でも、`A.X` は、包含する型によって制約されるアクセシビリティドメインを持ちます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-363">For example, even though all `X` members have `Public` declared accessibility, all but `A.X` have accessibility domains that are constrained by a containing type.</span></span>

<span data-ttu-id="ac1cf-364">関連付けられていない型が互いの保護されたメンバーにアクセスできないようにするには、@no__t 0 のインスタンスメンバーへのアクセスが派生型のインスタンスを経由する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-364">Access to `Protected` instance members must be through an instance of the derived type so that unrelated types cannot gain access to each other's protected members.</span></span> <span data-ttu-id="ac1cf-365">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-365">For example:</span></span>

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

<span data-ttu-id="ac1cf-366">上の例では、クラス `Guest` は、`Guest` のインスタンスで修飾されている場合にのみ、protected `Password` フィールドにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-366">In the above example, the class `Guest` only has access to the protected `Password` field if it is qualified with an instance of `Guest`.</span></span> <span data-ttu-id="ac1cf-367">これにより、`Guest` は `User` にキャストするだけで、`Employee` オブジェクトの `Password` フィールドにアクセスできなくなります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-367">This prevents `Guest` from gaining access to the `Password` field of an `Employee` object simply by casting it to `User`.</span></span>

<span data-ttu-id="ac1cf-368">ジェネリック型での @no__t 0 のメンバーアクセスのために、宣言コンテキストには型パラメーターが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-368">For the purposes of `Protected` member access in generic types, the declaration context includes type parameters.</span></span> <span data-ttu-id="ac1cf-369">これは、型引数の1つのセットを持つ派生型が、型引数の異なるセットを持つ派生型の @no__t 0 のメンバーにアクセスできないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-369">This means that a derived type with one set of type arguments does not have access to the `Protected` members of a derived type with a different set of type arguments.</span></span> <span data-ttu-id="ac1cf-370">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-370">For example:</span></span>

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

<span data-ttu-id="ac1cf-371">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="ac1cf-371">__Note.__</span></span> <span data-ttu-id="ac1cf-372">このC#言語 (および場合によっては他の言語) では、ジェネリック型は、指定された型引数に関係なく `Protected` のメンバーにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-372">The C# language (and possibly other languages) allows a generic type to access `Protected` members regardless of what type arguments are supplied.</span></span> <span data-ttu-id="ac1cf-373">@No__t 0 のメンバーを含むジェネリッククラスを設計する場合は、この点に留意する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-373">This should be kept in mind when designing generic classes that contain `Protected` members.</span></span>


### <a name="constituent-types"></a><span data-ttu-id="ac1cf-374">構成型</span><span class="sxs-lookup"><span data-stu-id="ac1cf-374">Constituent Types</span></span>

<span data-ttu-id="ac1cf-375">宣言の*構成型*は、宣言によって参照される型です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-375">The *constituent types* of a declaration are the types that are referenced by the declaration.</span></span> <span data-ttu-id="ac1cf-376">たとえば、定数の型、メソッドの戻り値の型、コンストラクターのパラメーターの型はすべて構成型です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-376">For example, the type of a constant, the return type of a method and the parameter types of a constructor are all constituent types.</span></span> <span data-ttu-id="ac1cf-377">宣言の構成型のアクセシビリティドメインは、または宣言自体のアクセシビリティドメインのスーパーセットと同じである必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-377">The accessibility domain of a constituent type of a declaration must be the same as or a superset of the accessibility domain of the declaration itself.</span></span> <span data-ttu-id="ac1cf-378">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-378">For example:</span></span>

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

## <a name="type-and-namespace-names"></a><span data-ttu-id="ac1cf-379">型と名前空間の名前</span><span class="sxs-lookup"><span data-stu-id="ac1cf-379">Type and Namespace Names</span></span>

<span data-ttu-id="ac1cf-380">多くの言語構成要素では、名前空間または型を指定する必要があります。これらは、名前空間または型の名前の修飾された形式を使用して指定できます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-380">Many language constructs require a namespace or type to be specified; these can be specified by using a qualified form of the namespace or type's name.</span></span> <span data-ttu-id="ac1cf-381">*修飾名*は、ピリオドで区切られた一連の識別子で構成されます。ピリオドの右側にある識別子は、期間の左側の識別子で指定された宣言空間で解決されます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-381">A *qualified name* consists of a series of identifiers separated by periods; the identifier on the right side of a period is resolved in the declaration space specified by the identifier on the left side of the period.</span></span>

<span data-ttu-id="ac1cf-382">名前空間または型の*完全修飾名*は、名前空間と型を含むすべての名前を含む修飾名です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-382">The *fully qualified name* of a namespace or type is a qualified name that contains the name of all containing namespaces and types.</span></span> <span data-ttu-id="ac1cf-383">つまり、名前空間または型の完全修飾名は `N.T` です。ここで `T` はエンティティの名前で、`N` はそのエンティティを含むエンティティの完全修飾名です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-383">In other words, the fully qualified name of a namespace or type is `N.T`, where `T` is the name of the entity and `N` is the fully qualified name of its containing entity.</span></span>

<span data-ttu-id="ac1cf-384">次の例では、いくつかの名前空間と型の宣言と、それに関連付けられた完全修飾名をインラインコメントに示しています。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-384">The example below shows several namespace and type declarations together with their associated fully qualified names in in-line comments.</span></span>

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

<span data-ttu-id="ac1cf-385">名前空間の X.y がソースコード内の2つの異なる場所で宣言されていることに注意してください。ただし、これらの2つの部分宣言は、クラス D とクラス E の両方を含む、X.y という名前空間を1つだけ構成します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-385">Observe that the namespace X.Y has been declared in two different locations in the source code, but these two partial declarations constitute just a single namespace called X.Y which contains both class D and class E.</span></span>

<span data-ttu-id="ac1cf-386">場合によっては、修飾名の先頭にキーワード `Global` が使用されることがあります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-386">In some situations, a qualified name may begin with the keyword `Global`.</span></span> <span data-ttu-id="ac1cf-387">キーワードは、名前のない最も外側の名前空間を表します。これは、宣言が外側の名前空間をシャドウする場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-387">The keyword represents the unnamed outermost namespace, which is useful in situations where a declaration shadows an enclosing namespace.</span></span> <span data-ttu-id="ac1cf-388">@No__t-0 キーワードを使用すると、そのような状況で最も外側の名前空間に "エスケープ" できます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-388">The `Global` keyword allows "escaping" out to the outermost namespace in that situation.</span></span> <span data-ttu-id="ac1cf-389">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-389">For example:</span></span>

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

<span data-ttu-id="ac1cf-390">上の例では、最初のメソッド呼び出しは無効です。識別子 `System` は、名前空間 `System` ではなく、クラス `System` にバインドされています。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-390">In the above example, the first method call is invalid because the identifier `System` binds to the class `System`, not the namespace `System`.</span></span> <span data-ttu-id="ac1cf-391">@No__t-0 名前空間にアクセスする唯一の方法は、`Global` を使用して、最も外側の名前空間をエスケープすることです。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-391">The only way to access the `System` namespace is to use `Global` to escape out to the outermost namespace.</span></span> <span data-ttu-id="ac1cf-392">`Global` は、`Imports` ステートメントまたは `Namespace` 宣言では使用できません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-392">`Global` cannot be used in an `Imports` statement or `Namespace` declaration.</span></span>

<span data-ttu-id="ac1cf-393">言語のキーワードと一致する型と名前空間が他の言語で導入される可能性があるため、Visual Basic は、ピリオドの後に続く限り、修飾名の一部としてキーワードを認識します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-393">Because other languages may introduce types and namespaces that match keywords in the language, Visual Basic recognizes keywords to be part of a qualified name as long as they follow a period.</span></span> <span data-ttu-id="ac1cf-394">このように使用されるキーワードは、識別子として扱われます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-394">Keywords used in this way are treated as identifiers.</span></span> <span data-ttu-id="ac1cf-395">たとえば、修飾識別子 `X.Default.Class` は有効な修飾識別子ですが、`Default.Class` は有効ではありません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-395">For example, the qualified identifier `X.Default.Class` is a valid qualified identifier, while `Default.Class` is not.</span></span>

### <a name="qualified-name-resolution-for-namespaces-and-types"></a><span data-ttu-id="ac1cf-396">名前空間と型の修飾名の解決</span><span class="sxs-lookup"><span data-stu-id="ac1cf-396">Qualified Name Resolution for namespaces and types</span></span>

<span data-ttu-id="ac1cf-397">@No__t-0 という形式の修飾された名前空間または型名を指定した場合、`R` は修飾名の右端の識別子、`A` は省略可能な型引数リストです。次の手順では、名前空間を決定する方法または修飾名を入力する方法について説明します。呼び</span><span class="sxs-lookup"><span data-stu-id="ac1cf-397">Given a qualified namespace or type name of the form `N.R(Of A)`, where `R` is the rightmost identifier in the qualified name and `A` is an optional type argument list, the following steps describe how to determine to which namespace or type the qualified name refers:</span></span>

1. <span data-ttu-id="ac1cf-398">修飾名または非修飾名前解決の規則を使用して `N` を解決します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-398">Resolve `N`, using the rules for either qualified or unqualified name resolution.</span></span>

2. <span data-ttu-id="ac1cf-399">@No__t-0 の解決が失敗した場合、または型パラメーターに解決された場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-399">If resolution of `N` fails, or resolves to a type parameter, a compile-time error occurs.</span></span>

3. <span data-ttu-id="ac1cf-400">それ以外の場合、`R` が N の名前空間の名前と一致し、型引数が指定されていないか、または `R` が型引数と同じ数の型パラメーターを持つ `N` のアクセス可能な型 (存在する場合) と一致する場合、修飾名はその名前空間または型を参照します.</span><span class="sxs-lookup"><span data-stu-id="ac1cf-400">Otherwise, if `R` matches the name of a namespace in N and no type arguments were supplied, or `R` matches an accessible type in `N` with the same number of type parameters as type arguments, if any, then the qualified name refers to that namespace or type.</span></span>

4. <span data-ttu-id="ac1cf-401">それ以外の場合、`N` に1つ以上の標準モジュールが含まれており、`R` が型引数と同じ数の型パラメーターを持つアクセス可能な型の名前 (存在する場合) が1つの標準モジュールで一致する場合、修飾名はその型を参照します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-401">Otherwise, if `N` contains one or more standard modules, and `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in exactly one standard module, then the qualified name refers to that type.</span></span> <span data-ttu-id="ac1cf-402">@No__t-0 が型引数と同じ数の型パラメーターを持つアクセス可能な型の名前と一致する場合 (存在する場合)、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-402">If `R` matches the name of accessible types with the same number of type parameters as type arguments, if any, in more than one standard module, a compile-time error occurs.</span></span>

5. <span data-ttu-id="ac1cf-403">それ以外の場合は、コンパイル時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-403">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="ac1cf-404">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="ac1cf-404">__Note.__</span></span> <span data-ttu-id="ac1cf-405">この解決プロセスの意味では、型のメンバーは名前空間または型名を解決するときに名前空間または型をシャドウしません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-405">An implication of this resolution process is that type members do not shadow namespaces or types when resolving namespace or type names.</span></span>

### <a name="unqualified-name-resolution-for-namespaces-and-types"></a><span data-ttu-id="ac1cf-406">名前空間と型の非修飾名前解決</span><span class="sxs-lookup"><span data-stu-id="ac1cf-406">Unqualified Name Resolution for namespaces and types</span></span>

<span data-ttu-id="ac1cf-407">修飾されていない名前 `R(Of A)` の場合、`A` は省略可能な型引数リストです。次の手順では、非修飾名が参照している名前空間または型を特定する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-407">Given an unqualified name `R(Of A)`, where `A` is an optional type argument list, the following steps describe how to determine to which namespace or type the unqualified name refers:</span></span>

1. <span data-ttu-id="ac1cf-408">R が現在のメソッドの型パラメーターの名前と一致し、型引数が指定されていない場合、非修飾名はその型パラメーターを参照します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-408">If R matches the name of a type parameter of the current method, and no type arguments were supplied, then the unqualified name refers to that type parameter.</span></span>

2.  <span data-ttu-id="ac1cf-409">名前参照を含む入れ子にされた型ごとに、最も内側の型から外側のに移動します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-409">For each nested type containing the name reference, starting from the innermost type and going to the outermost:</span></span>
    1. <span data-ttu-id="ac1cf-410">@No__t-0 が現在の型の型パラメーターの名前と一致し、型引数が指定されていない場合、非修飾名はその型パラメーターを参照します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-410">If `R` matches the name of a type parameter in the current type and no type arguments were supplied, then the unqualified name refers to that type parameter.</span></span>
    2. <span data-ttu-id="ac1cf-411">それ以外の場合、`R` が型引数と同じ数の型パラメーターを持つ、アクセス可能な入れ子にされた型の名前と一致すると、非修飾名はその型を参照します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-411">Otherwise, if `R` matches the name of an accessible nested type with the same number of type parameters as type arguments, if any, then the unqualified name refers to that type.</span></span>

3. <span data-ttu-id="ac1cf-412">名前参照を含む入れ子になった名前空間ごとに、最も内側の名前空間から始まり、最も外側の名前空間に移動します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-412">For each nested namespace containing the name reference, starting from the innermost namespace and going to the outermost namespace:</span></span>
    1. <span data-ttu-id="ac1cf-413">@No__t-0 が現在の名前空間の入れ子になった名前空間の名前と一致し、型引数リストが指定されていない場合、非修飾名はその入れ子になった名前空間を参照します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-413">If `R` matches the name of a nested namespace in the current namespace and no type argument list is supplied, then the unqualified name refers to that nested namespace.</span></span>
    2. <span data-ttu-id="ac1cf-414">それ以外の場合、`R` は、現在の名前空間で型引数と同じ数の型パラメーター (存在する場合) を持つアクセス可能な型の名前と一致する場合、非修飾名はその型を参照します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-414">Otherwise, if `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in the current namespace, then the unqualified name refers to that type.</span></span>
    3. <span data-ttu-id="ac1cf-415">それ以外の場合、名前空間にアクセス可能な標準モジュールが1つ以上含まれていて、`R` が型引数と同じ数の型パラメーター (存在する場合) と同じ数の型パラメーターを持つ、アクセス可能な入れ子にされた型の名前と一致する場合は、非修飾名は、その入れ子になった型を参照します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-415">Otherwise, if the namespace contains one or more accessible standard modules, and `R` matches the name of an accessible nested type with the same number of type parameters as type arguments, if any, in exactly one standard module, then the unqualified name refers to that nested type.</span></span> <span data-ttu-id="ac1cf-416">@No__t-0 が、1つの型引数と同じ数の型パラメーター (存在する場合) を持つ、アクセス可能な入れ子にされた型の名前と一致する場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-416">If `R` matches the name of accessible nested types with the same number of type parameters as type arguments, if any, in more than one standard module, a compile-time error occurs.</span></span>

4. <span data-ttu-id="ac1cf-417">ソースファイルに1つ以上のインポートエイリアスがあり、`R` がそのいずれかの名前と一致する場合、非修飾名はそのインポートエイリアスを参照します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-417">If the source file has one or more import aliases, and `R` matches the name of one of them, then the unqualified name refers to that import alias.</span></span> <span data-ttu-id="ac1cf-418">型引数リストが指定されている場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-418">If a type argument list is supplied, a compile-time error occurs.</span></span>

5. <span data-ttu-id="ac1cf-419">名前参照を含むソースファイルに1つ以上のインポートがある場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-419">If the source file containing the name reference has one or more imports:</span></span>
    1. <span data-ttu-id="ac1cf-420">@No__t-0 が型引数と同じ数の型パラメーターを持つアクセス可能な型の名前と一致する場合 (ある場合)、厳密に1つのインポートでは、非修飾名はその型を参照します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-420">If `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in exactly one import, then the unqualified name refers to that type.</span></span> <span data-ttu-id="ac1cf-421">@No__t-0 が型引数と同じ数の型パラメーターを持つアクセス可能な型の名前と一致する場合 (存在する場合)、複数のインポートとすべてが同じ型ではない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-421">If `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in more than one import and all are not the same type, a compile-time error occurs.</span></span>
    2. <span data-ttu-id="ac1cf-422">それ以外の場合、型引数リストが指定されておらず、`R` は、厳密に1つのインポートでアクセス可能な型を持つ名前空間の名前と一致する場合、非修飾名はその名前空間を参照します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-422">Otherwise, if no type argument list was supplied and `R` matches the name of a namespace with accessible types in exactly one import, then the unqualified name refers to that namespace.</span></span> <span data-ttu-id="ac1cf-423">型引数リストが指定されておらず、`R` が2つ以上のインポートでアクセス可能な型の名前空間の名前と一致し、すべてが同じ名前空間ではない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-423">If no type argument list was supplied and `R` matches the name of a namespace with accessible types in more than one import and all are not the same namespace, a compile-time error occurs.</span></span>
    3. <span data-ttu-id="ac1cf-424">それ以外の場合、インポートにアクセス可能な標準モジュールが1つ以上含まれていて、`R` が型引数と同じ数の型パラメーター (存在する場合) と同じ数の型パラメーターを持つ、アクセス可能な入れ子にされた型の名前と一致する場合、非修飾名はその型。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-424">Otherwise, if the imports contain one or more accessible standard modules, and `R` matches the name of an accessible nested type with the same number of type parameters as type arguments, if any, in exactly one standard module, then the unqualified name refers to that type.</span></span> <span data-ttu-id="ac1cf-425">@No__t-0 が、1つの型引数と同じ数の型パラメーター (存在する場合) を持つ、アクセス可能な入れ子にされた型の名前と一致する場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-425">If `R` matches the name of accessible nested types with the same number of type parameters as type arguments, if any, in more than one standard module, a compile-time error occurs.</span></span>

6. <span data-ttu-id="ac1cf-426">コンパイル環境で1つ以上のインポートエイリアスが定義されていて、`R` がそのいずれかの名前と一致する場合、非修飾名はそのインポートエイリアスを参照します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-426">If the compilation environment defines one or more import aliases, and `R` matches the name of one of them, then the unqualified name refers to that import alias.</span></span> <span data-ttu-id="ac1cf-427">型引数リストが指定されている場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-427">If a type argument list is supplied, a compile-time error occurs.</span></span>

7. <span data-ttu-id="ac1cf-428">コンパイル環境で1つ以上のインポートを定義する場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-428">If the compilation environment defines one or more imports:</span></span>
    1. <span data-ttu-id="ac1cf-429">@No__t-0 が型引数と同じ数の型パラメーターを持つアクセス可能な型の名前と一致する場合 (ある場合)、厳密に1つのインポートでは、非修飾名はその型を参照します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-429">If `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in exactly one import, then the unqualified name refers to that type.</span></span> <span data-ttu-id="ac1cf-430">@No__t-0 が型引数と同じ数の型パラメーターを持つアクセス可能な型の名前と一致する場合 (存在する場合)、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-430">If `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in more than one import, a compile-time error occurs.</span></span>
    2. <span data-ttu-id="ac1cf-431">それ以外の場合、型引数リストが指定されておらず、`R` は、厳密に1つのインポートでアクセス可能な型を持つ名前空間の名前と一致する場合、非修飾名はその名前空間を参照します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-431">Otherwise, if no type argument list was supplied and `R` matches the name of a namespace with accessible types in exactly one import, then the unqualified name refers to that namespace.</span></span> <span data-ttu-id="ac1cf-432">型引数リストが指定されておらず、`R` が2つ以上のインポートでアクセス可能な型の名前空間の名前と一致する場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-432">If no type argument list was supplied and `R` matches the name of a namespace with accessible types in more than one import, a compile-time error occurs.</span></span>
    3. <span data-ttu-id="ac1cf-433">それ以外の場合、インポートにアクセス可能な標準モジュールが1つ以上含まれていて、`R` が型引数と同じ数の型パラメーター (存在する場合) と同じ数の型パラメーターを持つ、アクセス可能な入れ子にされた型の名前と一致する場合、非修飾名はその型。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-433">Otherwise, if the imports contain one or more accessible standard modules, and `R` matches the name of an accessible nested type with the same number of type parameters as type arguments, if any, in exactly one standard module, then the unqualified name refers to that type.</span></span> <span data-ttu-id="ac1cf-434">@No__t-0 が、1つの型引数と同じ数の型パラメーター (存在する場合) を持つ、アクセス可能な入れ子にされた型の名前と一致する場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-434">If `R` matches the name of accessible nested types with the same number of type parameters as type arguments, if any, in more than one standard module, a compile-time error occurs.</span></span>

8. <span data-ttu-id="ac1cf-435">それ以外の場合は、コンパイル時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-435">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="ac1cf-436">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="ac1cf-436">__Note.__</span></span> <span data-ttu-id="ac1cf-437">この解決プロセスの意味では、型のメンバーは名前空間または型名を解決するときに名前空間または型をシャドウしません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-437">An implication of this resolution process is that type members do not shadow namespaces or types when resolving namespace or type names.</span></span>

<span data-ttu-id="ac1cf-438">通常、名前は特定の名前空間に1回だけ出現できます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-438">Normally, a name can only occur once in a particular namespace.</span></span> <span data-ttu-id="ac1cf-439">ただし、複数の .NET アセンブリにわたって名前空間を宣言できるため、2つのアセンブリが同じ完全修飾名を持つ型を定義する状況が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-439">However, because namespaces can be declared across multiple .NET assemblies, it is possible to have a situation where two assemblies define a type with the same fully qualified name.</span></span> <span data-ttu-id="ac1cf-440">この場合、ソースファイルの現在のセットで宣言されている型は、外部の .NET アセンブリで宣言されている型よりも優先されます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-440">In that case, a type declared in the current set of source files is preferred over a type declared in an external .NET assembly.</span></span> <span data-ttu-id="ac1cf-441">それ以外の場合、名前はあいまいであり、名前を明確に区別する方法はありません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-441">Otherwise, the name is ambiguous and there is no way to disambiguate the name.</span></span>

## <a name="variables"></a><span data-ttu-id="ac1cf-442">変数:</span><span class="sxs-lookup"><span data-stu-id="ac1cf-442">Variables</span></span>

<span data-ttu-id="ac1cf-443">*変数*は、格納場所を表します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-443">A *variable* represents a storage location.</span></span> <span data-ttu-id="ac1cf-444">すべての変数には、変数に格納できる値を決定する型があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-444">Every variable has a type that determines what values can be stored in the variable.</span></span> <span data-ttu-id="ac1cf-445">Visual Basic はタイプセーフな言語であるため、プログラム内のすべての変数には型があり、言語は変数に格納されている値が常に適切な型であることを保証します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-445">Because Visual Basic is a type-safe language, every variable in a program has a type and the language guarantees that values stored in variables are always of the appropriate type.</span></span> <span data-ttu-id="ac1cf-446">変数への参照を行う前に、変数は常にその型の既定値に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-446">Variables are always initialized to the default value of their type before any reference to the variable can be made.</span></span> <span data-ttu-id="ac1cf-447">初期化されていないメモリにアクセスすることはできません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-447">It is not possible to access uninitialized memory.</span></span>

## <a name="generic-types-and-methods"></a><span data-ttu-id="ac1cf-448">ジェネリック型とジェネリックメソッド</span><span class="sxs-lookup"><span data-stu-id="ac1cf-448">Generic Types and Methods</span></span>

<span data-ttu-id="ac1cf-449">型 (標準モジュールと列挙型を除く) とメソッドでは、型パラメーターを宣言できます。*型パラメーター*は、型のインスタンスが宣言されるか、メソッドが呼び出されるまで提供されない型です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-449">Types (except for standard modules and enumerated types) and methods can declare *type parameters*, which are types that will not be provided until an instance of the type is declared or the method is invoked.</span></span> <span data-ttu-id="ac1cf-450">型パラメーターを持つ型とメソッドは、それぞれ*ジェネリック型*および*ジェネリックメソッド*とも呼ばれます。これは、型またはメソッドを使用するコードによって提供される型に関する特定の知識がなくても、型またはメソッドが一般に記述されている必要があるためです。型またはメソッド。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-450">Types and methods with type parameters are also known as *generic types* and *generic methods*, respectively, because the type or method must be written generically, without specific knowledge of the types that will be supplied by code that uses the type or method.</span></span>

<span data-ttu-id="ac1cf-451">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="ac1cf-451">__Note.__</span></span> <span data-ttu-id="ac1cf-452">現時点では、メソッドとデリゲートはジェネリックにすることができますが、プロパティ、イベント、および演算子をジェネリックにすることはできません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-452">At this time, even though methods and delegates can be generic, properties, events and operators cannot be generic themselves.</span></span> <span data-ttu-id="ac1cf-453">ただし、包含クラスの型パラメーターを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-453">They may, however, use type parameters from the containing class.</span></span>

<span data-ttu-id="ac1cf-454">ジェネリック型またはジェネリックメソッドの観点からは、型パラメーターは、型またはメソッドが使用されたときに実際の型を格納するプレースホルダー型です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-454">From the perspective of the generic type or method, a type parameter is a placeholder type that will be filled in with an actual type when the type or method is used.</span></span> <span data-ttu-id="ac1cf-455">型引数は、型またはメソッドが使用されている点で、型またはメソッドの型パラメーターの代わりに使用されます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-455">Type arguments are substituted for the type parameters in the type or method at the point at which the type or method is used.</span></span> <span data-ttu-id="ac1cf-456">たとえば、ジェネリックスタッククラスは次のように実装できます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-456">For example, a generic stack class could be implemented as:</span></span>

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

<span data-ttu-id="ac1cf-457">@No__t-0 クラスを使用する宣言では、型パラメーター `ItemType` に型引数を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-457">Declarations that use the `Stack(Of ItemType)` class must supply a type argument for the type parameter `ItemType`.</span></span> <span data-ttu-id="ac1cf-458">この型は、クラス内で `ItemType` が使用されている場所に格納されます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-458">This type is then filled in wherever `ItemType` is used within the class:</span></span>

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

### <a name="type-parameters"></a><span data-ttu-id="ac1cf-459">型パラメーター</span><span class="sxs-lookup"><span data-stu-id="ac1cf-459">Type Parameters</span></span>

<span data-ttu-id="ac1cf-460">型パラメーターは、型またはメソッドの宣言で指定できます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-460">Type parameters may be supplied on type or method declarations.</span></span> <span data-ttu-id="ac1cf-461">各型パラメーターは識別子であり、構築された型またはメソッドを作成するために提供される型引数のプレースホルダーです。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-461">Each type parameter is an identifier which is a place-holder for a type argument that is supplied to create a constructed type or method.</span></span> <span data-ttu-id="ac1cf-462">これに対し、型引数は、ジェネリック型またはジェネリックメソッドが使用されている場合に、型パラメーターに置き換えられる実際の型です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-462">By contrast, a type argument is the actual type that is substituted for the type parameter when a generic type or method is used.</span></span>

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

<span data-ttu-id="ac1cf-463">型またはメソッドの宣言のそれぞれの型パラメーターは、その型またはメソッドの宣言空間で名前を定義します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-463">Each type parameter in a type or method declaration defines a name in the declaration space of that type or method.</span></span> <span data-ttu-id="ac1cf-464">そのため、別の型パラメーター、型メンバー、メソッドパラメーター、またはローカル変数と同じ名前を指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-464">Thus, it cannot have the same name as another type parameter, a type member, a method parameter, or a local variable.</span></span> <span data-ttu-id="ac1cf-465">型またはメソッドの型パラメーターのスコープは、型またはメソッド全体です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-465">The scope of a type parameter on a type or method is the entire type or method.</span></span> <span data-ttu-id="ac1cf-466">型パラメーターは型宣言全体にスコープが設定されているので、入れ子にされた型は外部型パラメーターを使用できます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-466">Because type parameters are scoped to the entire type declaration, nested types can use outer type parameters.</span></span> <span data-ttu-id="ac1cf-467">また、ジェネリック型内で入れ子にされた型にアクセスするときは、常に型パラメーターを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-467">This also means that type parameters must always be specified when accessing types nested inside generic types:</span></span>

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

<span data-ttu-id="ac1cf-468">クラスの他のメンバーとは異なり、型パラメーターは継承されません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-468">Unlike other members of a class, type parameters are not inherited.</span></span> <span data-ttu-id="ac1cf-469">型の型パラメーターは、単純な名前によってのみ参照できます。つまり、包含する型名で修飾することはできません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-469">Type parameters in a type can only be referred to by their simple name; in other words, they cannot be qualified with the containing type name.</span></span> <span data-ttu-id="ac1cf-470">無効なプログラミングスタイルですが、入れ子にされた型の型パラメーターは、外側の型で宣言されたメンバーまたは型パラメーターを隠ぺいすることができます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-470">Although it is bad programming style, the type parameters in a nested type can hide a member or type parameter declared in the outer type:</span></span>

```vb
Class Outer(Of T)
    Class Inner(Of T)
        Public t1 As T    ' Refers to Inner's T
    End Class
End Class
```

<span data-ttu-id="ac1cf-471">型およびメソッドは、型またはメソッドが宣言する型パラメーター (または*アリティ*) の数に基づいてオーバーロードできます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-471">Types and methods may be overloaded based on the number of type parameters (or *arity*) that the types or methods declare.</span></span> <span data-ttu-id="ac1cf-472">たとえば、次の宣言は有効です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-472">For example, the following declarations are legal:</span></span>

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

<span data-ttu-id="ac1cf-473">型の場合、オーバーロードは常に、指定された型引数の数と一致します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-473">In the case of types, overloads are always matched against the number of type arguments specified.</span></span> <span data-ttu-id="ac1cf-474">これは、ジェネリックと非ジェネリックの両方のクラスを同じプログラムで一緒に使用する場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-474">This is useful when using both generic and non-generic classes together in the same program:</span></span>

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

<span data-ttu-id="ac1cf-475">型パラメーターでオーバーロードされたメソッドの規則については、メソッドのオーバーロードの解決に関するセクションで説明します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-475">Rules for methods overloaded on type parameters are covered in the section on method overload resolution.</span></span>

<span data-ttu-id="ac1cf-476">包含宣言内では、型パラメーターは完全な型と見なされます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-476">Within the containing declaration, type parameters are considered full types.</span></span> <span data-ttu-id="ac1cf-477">型パラメーターは、さまざまな異なる実際の型引数を使用してインスタンス化できるため、次に示すように、型パラメーターには、他の型とは若干異なる操作と制限があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-477">Since a type parameter can be instantiated with many different actual type arguments, type parameters have slightly different operations and restrictions than other types as described below:</span></span>

* <span data-ttu-id="ac1cf-478">型パラメーターを直接使用して基底クラスまたはインターフェイスを宣言することはできません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-478">A type parameter cannot be used directly to declare a base class or interface.</span></span>

* <span data-ttu-id="ac1cf-479">型パラメーターに対するメンバー参照の規則は、型パラメーターに適用される制約によって異なります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-479">The rules for member lookup on type parameters depend on the constraints, if any, applied to the type parameter.</span></span>

* <span data-ttu-id="ac1cf-480">型パラメーターに使用できる変換は、型パラメーターに適用される制約によって異なります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-480">The available conversions for a type parameter depend on the constraints, if any, applied to the type parameters.</span></span>

* <span data-ttu-id="ac1cf-481">@No__t 0 の制約がない場合、型パラメーターによって表される型の値は、`Is` および @no__t を使用して `Nothing` と比較できます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-481">In the absence of a `Structure` constraint, a value with a type represented by a type parameter can be compared with `Nothing` using `Is` and `IsNot`.</span></span>

* <span data-ttu-id="ac1cf-482">型パラメーターが `New` または `Structure` の制約によって制約されている場合、`New` 式でのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-482">A type parameter can only be used in a `New` expression if the type parameter is constrained by a `New` or a `Structure` constraint.</span></span>

* <span data-ttu-id="ac1cf-483">型パラメーターは、`GetType` 式内の属性例外内の任意の場所で使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-483">A type parameter cannot be used anywhere within an attribute exception within a `GetType` expression.</span></span>

* <span data-ttu-id="ac1cf-484">型パラメーターは、他のジェネリック型およびパラメーターの型引数として使用できます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-484">Type parameters can be used as type arguments to other generic types and parameters.</span></span>

<span data-ttu-id="ac1cf-485">次の例は、`Stack(Of ItemType)` クラスを拡張するジェネリック型です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-485">The following example is a generic type that extends the `Stack(Of ItemType)` class:</span></span>

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

<span data-ttu-id="ac1cf-486">宣言で `MyStack` に型引数を指定すると、同じ型引数が `Stack` にも適用されます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-486">When a declaration supplies a type argument to `MyStack`, the same type argument will be applied to `Stack` as well.</span></span>

<span data-ttu-id="ac1cf-487">型として、型パラメーターは純粋にコンパイル時の構成要素です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-487">As a type, type parameters are purely a compile-time construct.</span></span> <span data-ttu-id="ac1cf-488">実行時に、各型パラメーターは、ジェネリック宣言に型引数を指定して指定されたランタイム型にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-488">At run-time, each type parameter is bound to a run-time type that was specified by supplying a type argument to the generic declaration.</span></span> <span data-ttu-id="ac1cf-489">したがって、型パラメーターで宣言された変数の型は、実行時に非ジェネリック型または特定の構築型になります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-489">Thus, the type of a variable declared with a type parameter will, at run-time, be a non-generic type or a specific constructed type.</span></span> <span data-ttu-id="ac1cf-490">型パラメーターを含むすべてのステートメントおよび式の実行時の実行では、そのパラメーターの型引数として指定された実際の型を使用します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-490">The run-time execution of all statements and expressions involving type parameters uses the actual type that was supplied as the type argument for that parameter.</span></span>


### <a name="type-constraints"></a><span data-ttu-id="ac1cf-491">型の制約</span><span class="sxs-lookup"><span data-stu-id="ac1cf-491">Type Constraints</span></span>

<span data-ttu-id="ac1cf-492">型引数は型システム内の任意の型にすることができるため、ジェネリック型またはジェネリックメソッドは型パラメーターについての想定を行うことができません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-492">Because a type argument can be any type in the type system, a generic type or method cannot make any assumptions about a type parameter.</span></span> <span data-ttu-id="ac1cf-493">したがって、型パラメーターのメンバーは、すべての型が @no__t から派生しているため、型のメンバーとして `Object` と見なされます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-493">Thus, the members of a type parameter are considered to be the members of the type `Object`, since all types derive from `Object`.</span></span>

<span data-ttu-id="ac1cf-494">@No__t-0 のようなコレクションの場合、この事実は特に重要な制限とは言えませんが、ジェネリック型が型引数として指定される型について想定することが必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-494">In the case of a collection like `Stack(Of ItemType)`, this fact may not be a particularly important restriction, but there may be cases where a generic type may wish to make an assumption about the types that will be supplied as type arguments.</span></span> <span data-ttu-id="ac1cf-495">型の*制約*は、型パラメーターとして指定できる型を制限する型パラメーターに配置できます。また、ジェネリック型またはメソッドが型パラメーターについてより多くのものを想定できるようにします。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-495">*Type constraints* can be placed on type parameters that restrict which types can be supplied as a type parameter and allow generic types or methods to assume more about type parameters.</span></span>

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

<span data-ttu-id="ac1cf-496">この例では、`DisposableStack(Of ItemType)` は、その型パラメーターを、インターフェイスを実装する型のみを `System.IDisposable` に制限します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-496">In this example, the `DisposableStack(Of ItemType)` constrains its type parameter to only types that implement the interface `System.IDisposable`.</span></span> <span data-ttu-id="ac1cf-497">その結果、キューに残っているオブジェクトを破棄する @no__t 0 のメソッドを実装できます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-497">As a result, it can implement a `Dispose` method that disposes any objects still left in the queue.</span></span>

<span data-ttu-id="ac1cf-498">型制約は、特別な制約の1つである必要があります (-0、`Structure`、または `New`)。または、次のいずれかの @no__t 型である必要があります: @no__t。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-498">A type constraint must be one of the special constraints `Class`, `Structure`, or `New`, or it must be a type `T` where:</span></span>

* <span data-ttu-id="ac1cf-499">`T` はクラス、インターフェイス、または型パラメーターです。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-499">`T` is a class, an interface, or a type parameter.</span></span>

* <span data-ttu-id="ac1cf-500">`T` が `NotInheritable` ではありません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-500">`T` is not `NotInheritable`.</span></span>

* <span data-ttu-id="ac1cf-501">`T` は、次の特殊な型 (`System.Array`、`System.Delegate`、`System.MulticastDelegate`、`System.Enum`、または `System.ValueType`) のいずれかではありません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-501">`T` is not one of, or a type inherited from one of, the following special types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span>

* <span data-ttu-id="ac1cf-502">`T` が `Object` ではありません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-502">`T` is not `Object`.</span></span> <span data-ttu-id="ac1cf-503">すべての型は @no__t 0 から派生するため、このような制約は、許可されている場合は効果がありません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-503">Since all types derive from `Object`, such a constraint would have no effect if it were permitted.</span></span>

* <span data-ttu-id="ac1cf-504">`T` は、宣言するジェネリック型またはメソッドと同じように、少なくともアクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-504">`T` must be at least as accessible as the generic type or method being declared.</span></span>

<span data-ttu-id="ac1cf-505">型制約を中かっこ (`{}`) で囲むことによって、1つの型パラメーターに複数の型制約を指定できます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-505">Multiple type constraints can be specified for a single type parameter by enclosing the type constraints in curly braces (`{}`)..</span></span> <span data-ttu-id="ac1cf-506">クラスには、特定の型パラメーターに対して1つの型制約のみを指定できます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-506">Only one type constraint for a given type parameter can be a class.</span></span> <span data-ttu-id="ac1cf-507">@No__t 0 の特殊な制約を名前付きクラスの制約または @no__t の特殊な制約と組み合わせると、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-507">It is an error to combine a `Structure` special constraint with a named class constraint or the `Class` special constraint.</span></span>

```vb
Class ControlFactory(Of T As {Control, New})
    ...
End Class
```

<span data-ttu-id="ac1cf-508">型制約は、含んでいる型、またはそれを含む型の型パラメーターを使用できます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-508">Type constraints can use the containing types or any of the containing types' type parameters.</span></span> <span data-ttu-id="ac1cf-509">次の例では、制約によって、指定された型引数が、それ自体を型引数として使用するジェネリックインターフェイスを実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-509">In the following example, the constraint requires that the type argument supplied implements a generic interface using itself as a type argument:</span></span>

```vb
Class Sorter(Of V As IComparable(Of V))
    ...
End Class
```

<span data-ttu-id="ac1cf-510">特殊な型制約 `Class` は、指定された型引数を任意の参照型に制限します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-510">The special type constraint `Class` constrains the supplied type argument to any reference type.</span></span>

<span data-ttu-id="ac1cf-511">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="ac1cf-511">__Note.__</span></span> <span data-ttu-id="ac1cf-512">特別な型制約 `Class` は、インターフェイスで満たすことができます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-512">The special type constraint `Class` can be satisfied by an interface.</span></span> <span data-ttu-id="ac1cf-513">構造体はインターフェイスを実装できます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-513">And a structure can implement an interface.</span></span> <span data-ttu-id="ac1cf-514">したがって、構造体 (`Class` の特殊な制約を満たしていない)、および "U" が実装するインターフェイス (@no__t 2 の特殊な制約を満たす) では、制約 `(Of T As U, U As Class)` になることがあります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-514">Therefore, the constraint `(Of T As U, U As Class)` might be satisfied with "T" a structure (which does not satisfy the `Class` special constraint), and "U" an interface that it implements (which does satisfy the `Class` special constraint).</span></span>

<span data-ttu-id="ac1cf-515">特殊な型制約 `Structure` は、指定された型引数を、`System.Nullable(Of T)` 以外の任意の値型に制限します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-515">The special type constraint `Structure` constrains the supplied type argument to any value type except `System.Nullable(Of T)`.</span></span>

<span data-ttu-id="ac1cf-516">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="ac1cf-516">__Note.__</span></span> <span data-ttu-id="ac1cf-517">構造体の制約では、型引数として `System.Nullable(Of T)` を指定することができないように、`System.Nullable(Of T)` は許可されません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-517">Structure constraints do not allow `System.Nullable(Of T)` so that it is not possible to supply `System.Nullable(Of T)` as a type argument to itself.</span></span>

<span data-ttu-id="ac1cf-518">特別な型制約 `New` では、指定された型引数にアクセス可能なパラメーターなしのコンストラクターが必要であり、`MustInherit` として宣言できない必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-518">The special type constraint `New` requires that the supplied type argument must have an accessible parameterless constructor and cannot be declared `MustInherit`.</span></span> <span data-ttu-id="ac1cf-519">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-519">For example:</span></span>

```vb
Class Factory(Of T As New)
    Function CreateInstance() As T
        Return New T()
    End Function
End Class
```

<span data-ttu-id="ac1cf-520">クラス型の制約では、指定された型引数が型であるか、それを継承している必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-520">A class type constraint requires that the supplied type argument must either be that type as or inherit from it.</span></span> <span data-ttu-id="ac1cf-521">インターフェイス型の制約では、指定された型引数にそのインターフェイスを実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-521">An interface type constraint requires that the supplied type argument must implement that interface.</span></span> <span data-ttu-id="ac1cf-522">型パラメーター制約では、指定された型引数が、対応する型パラメーターに指定されたすべての境界から派生するか、またはすべてを実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-522">A type parameter constraint requires that the supplied type argument must derive from or implement all of the bounds given for the matching type parameter.</span></span> <span data-ttu-id="ac1cf-523">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-523">For example:</span></span>

```vb
Class List(Of T)
    Sub AddRange(Of S As T)(collection As IEnumerable(Of S))
        ...
    End Sub
End Class
```

<span data-ttu-id="ac1cf-524">この例では、`AddRange` の型パラメーター `S` は、`List` の型パラメーター @no__t に制限されています。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-524">In this example, the type parameter `S` on `AddRange` is constrained to the type parameter `T` of `List`.</span></span> <span data-ttu-id="ac1cf-525">つまり、`List(Of Control)` は、`AddRange` の型パラメーターを `Control` から継承する任意の型に制限します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-525">This means that a `List(Of Control)` would constrain `AddRange`'s type parameter to any type that is or inherits from `Control`.</span></span>

<span data-ttu-id="ac1cf-526">型パラメーターの制約 `Of S As T` は、特別な制約 (`Class`、`Structure`、`New`) を除き、`T` のすべての制約を `S` に推移的に追加することによって解決されます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-526">A type parameter constraint `Of S As T` is resolved by transitively adding all of `T`'s constraints onto `S`, other than the special constraints (`Class`, `Structure`, `New`).</span></span> <span data-ttu-id="ac1cf-527">循環制約 (`Of S As T, T As S` など) を設定するとエラーになります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-527">It is an error to have circular constraints (e.g. `Of S As T, T As S`).</span></span> <span data-ttu-id="ac1cf-528">型パラメーターの制約を持つことはできませんが、それ自体には @no__t 0 の制約があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-528">It is an error to have a type parameter constraint which itself has the `Structure` constraint.</span></span> <span data-ttu-id="ac1cf-529">制約を追加した後、さまざまな特殊な状況が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-529">After adding constraints, it is possible that a number of special situations may occur:</span></span>

* <span data-ttu-id="ac1cf-530">複数のクラス制約が存在する場合、ほとんどの派生クラスは制約と見なされます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-530">If multiple class constraints exist, the most derived class is considered to be the constraint.</span></span> <span data-ttu-id="ac1cf-531">1つ以上のクラス制約に継承関係がない場合、制約は満たしであり、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-531">If one or more class constraints have no inheritance relationship, the constraint is unsatisfiable and it is an error.</span></span>

 * <span data-ttu-id="ac1cf-532">型パラメーターが、名前付きのクラス制約または `Class` の特殊な制約を持つ @no__t 0 の特殊な制約を結合する場合、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-532">If a type parameter combines a `Structure` special constraint with a named class constraint or the `Class` special constraint, it is an error.</span></span> <span data-ttu-id="ac1cf-533">クラスの制約は @no__t 0 にすることができます。この場合、その制約の派生型は受け入れられず、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-533">A class constraint may be `NotInheritable`, in which case no derived types of that constraint are accepted and it is an error.</span></span>

<span data-ttu-id="ac1cf-534">型には、のいずれか、またはから継承された型 (`System.Array`、`System.Delegate`、`System.MulticastDelegate`、`System.Enum`、または `System.ValueType`) を指定できます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-534">The type may be one of, or a type inherited from, the following special types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span> <span data-ttu-id="ac1cf-535">その場合は、型またはそれから継承された型のみが受け入れられます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-535">In that case, only the type, or a type inherited from it, is accepted.</span></span> <span data-ttu-id="ac1cf-536">これらの型のいずれかに制限されている型パラメーターでは、`DirectCast` 演算子で許可されている変換のみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-536">A type parameter constrained to one of these types can only use the conversions allowed by the `DirectCast` operator.</span></span> <span data-ttu-id="ac1cf-537">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-537">For example:</span></span>

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

<span data-ttu-id="ac1cf-538">また、上記のいずれかのリラクゼーションによって値型に制約される型パラメーターは、その値型で定義されているメソッドを呼び出すことはできません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-538">Additionally, a type parameter constrained to a value type due to one of the above relaxations cannot call any methods defined on that value type.</span></span> <span data-ttu-id="ac1cf-539">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-539">For example:</span></span>

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

<span data-ttu-id="ac1cf-540">置換後の制約が配列型として終了する場合は、共変の配列型も許可されます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-540">If the constraint, after substitution, ends up as an array type, any covariant array type is allowed as well.</span></span> <span data-ttu-id="ac1cf-541">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-541">For example:</span></span>

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

<span data-ttu-id="ac1cf-542">クラスまたはインターフェイスの制約を持つ型パラメーターは、そのクラスまたはインターフェイスの制約と同じメンバーを持つと見なされます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-542">A type parameter with a class or interface constraint is considered to have the same members as that class or interface constraint.</span></span> <span data-ttu-id="ac1cf-543">型パラメーターに複数の制約がある場合、型パラメーターは、制約のすべてのメンバーの和集合を持つと見なされます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-543">If a type parameter has multiple constraints, then the type parameter is considered to have the union of all the members of the constraints.</span></span> <span data-ttu-id="ac1cf-544">複数の制約に同じ名前のメンバーがある場合、メンバーは次の順序で非表示になります。クラスの制約は、インターフェイスの制約のメンバーを非表示にします。これにより、`Structure` の制約が指定されている場合に `System.ValueType` のメンバーが非表示になります。`Object` のメンバー。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-544">If there are members with the same name in more than one constraint, then members are hidden in the following order: the class constraint hides members in interface constraints, which hide members in `System.ValueType` (if `Structure` constraint is specified), which hides members in `Object`.</span></span> <span data-ttu-id="ac1cf-545">同じ名前のメンバーが複数のインターフェイスの制約に含まれている場合は、(複数のインターフェイスの継承のように) メンバーを使用できず、型パラメーターを目的のインターフェイスにキャストする必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-545">If a member with the same name appears in more than one interface constraint the member is unavailable (as in multiple interface inheritance) and the type parameter must be cast to the desired interface.</span></span> <span data-ttu-id="ac1cf-546">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-546">For example:</span></span>

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

<span data-ttu-id="ac1cf-547">型パラメーターを型引数として指定する場合は、型パラメーターが一致する型パラメーターの制約を満たしている必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-547">When supplying type parameters as type arguments, the type parameters must satisfy the constraints of the matching type parameters.</span></span>

```vb
Class Base(Of T As Class)
End Class

Class Derived(Of V)
    ' Error: V does not satisfy the constraints of T
    Inherits Base(Of V)
End Class
```

<span data-ttu-id="ac1cf-548">制約付き型パラメーターの値を使用して、制約で指定されたインスタンスメソッドを含むインスタンスメンバーにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-548">Values of a constrained type parameter can be used to access the instance members, including instance methods, specified in the constraint.</span></span>

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

### <a name="type-parameter-variance"></a><span data-ttu-id="ac1cf-549">型パラメーターの分散</span><span class="sxs-lookup"><span data-stu-id="ac1cf-549">Type Parameter Variance</span></span>

<span data-ttu-id="ac1cf-550">インターフェイスまたはデリゲート型の宣言の型パラメーターは、必要に応じて、*分散修飾子*を指定できます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-550">A type parameter in an interface or a delegate type declaration can optionally specify a *variance modifier*.</span></span> <span data-ttu-id="ac1cf-551">型パラメーターと変位修飾子を使用すると、インターフェイスまたはデリゲート型で型パラメーターを使用する方法が制限されますが、ジェネリックインターフェイスまたはデリゲート型を、バリアントと互換性のある型引数を持つ別のジェネリック型に変換することができます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-551">Type parameters with variance modifiers restrict how the type parameter can be used in the interface or delegate type but allow a generic interface or delegate type to be converted to another generic type with variant compatible type arguments.</span></span> <span data-ttu-id="ac1cf-552">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-552">For example:</span></span>

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

<span data-ttu-id="ac1cf-553">変位修飾子を持つ型パラメーターを持つジェネリックインターフェイスには、いくつかの制限があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-553">Generic interfaces that have type parameters with variance modifiers have several restrictions:</span></span>

* <span data-ttu-id="ac1cf-554">パラメーターリストを指定するイベント宣言を含めることはできません (ただし、カスタムイベント宣言や、デリゲート型を持つイベント宣言は許可されます)。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-554">They cannot contain an event declaration that specifies a parameter list (but a custom event declaration or an event declaration with a delegate type is allowed).</span></span>

* <span data-ttu-id="ac1cf-555">入れ子になったクラス、構造体、または列挙型を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-555">They cannot contain a nested class, structure, or enumerated type.</span></span>

<span data-ttu-id="ac1cf-556">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="ac1cf-556">__Note.__</span></span> <span data-ttu-id="ac1cf-557">これらの制限は、ジェネリック型で入れ子にされた型がその親のジェネリックパラメーターを暗黙的にコピーするという事実に起因します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-557">These restrictions are due to the fact that types nested in generic types implicitly copy the generic parameters of their parent.</span></span> <span data-ttu-id="ac1cf-558">クラス、構造体、または列挙型が入れ子になっている場合、それらの型の型パラメーターには変位修飾子を設定できません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-558">In the case of nested classes, structures, or enumerated types, those kinds of types cannot have variance modifiers on their type parameters.</span></span> <span data-ttu-id="ac1cf-559">パラメーターリストを持つイベント宣言の場合、生成された入れ子になったデリゲートクラスでは、@no__t 0 の位置 (パラメーター型) で使用されている型が実際には、`Out` の位置 (つまり、パラメーター型) で使用されていると、混乱を招く可能性があります。イベント)。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-559">In the case of an event declaration with a parameter list, the generated nested delegate class could have confusing errors when a type that appears to be used in an `In` position (i.e. a parameter type) is actually used in an `Out` position (i.e. the type of the event).</span></span>

<span data-ttu-id="ac1cf-560">Out 修飾子で宣言された型パラメーターは*共変*です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-560">A type parameter that is declared with the Out modifier is *covariant*.</span></span> <span data-ttu-id="ac1cf-561">非公式には、共変の型パラメーターは、出力位置 (インターフェイスまたはデリゲート型から返される値) でのみ使用できます。これは、入力位置では使用できません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-561">Informally, a covariant type parameter can only be used in an output position -- i.e. a value that is being returned from the interface or delegate type -- and cannot be used in an input position.</span></span> <span data-ttu-id="ac1cf-562">次の場合、型 `T` は*有効*であると見なされます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-562">A type `T` is considered to be *valid covariantly* if:</span></span>

* <span data-ttu-id="ac1cf-563">`T` はクラス、構造体、または列挙型です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-563">`T` is a class, structure, or enumerated type.</span></span>

* <span data-ttu-id="ac1cf-564">`T` は、非ジェネリックデリゲートまたはインターフェイス型です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-564">`T` is non-generic delegate or interface type.</span></span>

* <span data-ttu-id="ac1cf-565">`T` は、要素型が有効である配列型です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-565">`T` is an array type whose element type is valid covariantly.</span></span>

* <span data-ttu-id="ac1cf-566">`T` は、`Out` の型パラメーターとして宣言されていない型パラメーターです。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-566">`T` is a type parameter which was not declared as an `Out` type parameter.</span></span>

* <span data-ttu-id="ac1cf-567">`T` は、次のように、型引数 `X(Of P1,...,Pn)` を持つ、構築されたインターフェイスまたはデリゲート型 @no__t です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-567">`T` is a constructed interface or delegate type `X(Of P1,...,Pn)` with type arguments `A1,...,An` such that:</span></span>

  * <span data-ttu-id="ac1cf-568">@No__t-0 が Out 型パラメーターとして宣言されている場合、`Ai` は有効です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-568">If `Pi` was declared as an Out type parameter then `Ai` is valid covariantly.</span></span>

  * <span data-ttu-id="ac1cf-569">@No__t-0 が型パラメーターとして宣言されている場合、`Ai` は有効な contravariantly です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-569">If `Pi` was declared as an In type parameter then `Ai` is valid contravariantly.</span></span>

<span data-ttu-id="ac1cf-570">インターフェイスまたはデリゲート型では、次のものが有効である必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-570">The following must be valid covariantly in an interface or delegate type:</span></span>

* <span data-ttu-id="ac1cf-571">インターフェイスの基本インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-571">The base interface of an interface.</span></span>

* <span data-ttu-id="ac1cf-572">関数またはデリゲート型の戻り値の型。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-572">The return type of a function or the delegate type.</span></span>

* <span data-ttu-id="ac1cf-573">@No__t 0 のアクセサーがある場合のプロパティの型。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-573">The type of a property if there is a `Get` accessor.</span></span>

* <span data-ttu-id="ac1cf-574">@No__t-0 パラメーターの型。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-574">The type of any `ByRef` parameter.</span></span>

<span data-ttu-id="ac1cf-575">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-575">For example:</span></span>

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

<span data-ttu-id="ac1cf-576">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="ac1cf-576">__Note.__</span></span> <span data-ttu-id="ac1cf-577">`Out` は予約語ではありません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-577">`Out` is not a reserved word.</span></span>

<span data-ttu-id="ac1cf-578">In 修飾子で宣言された型パラメーターは*反変*です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-578">A type parameter that is declared with the In modifier is *contravariant*.</span></span> <span data-ttu-id="ac1cf-579">これに対して、反変の型パラメーターは、入力位置 (つまり、インターフェイスまたはデリゲート型に渡される値) でのみ使用でき、出力位置では使用できません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-579">Informally, a contravariant type parameter can only be used in an input position -- i.e. a value that is being passed in to the interface or delegate type -- and cannot be used in an output position.</span></span> <span data-ttu-id="ac1cf-580">次の場合、型 `T` は*有効な contravariantly*と見なされます。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-580">A type `T` is considered to be *valid contravariantly* if:</span></span>

* <span data-ttu-id="ac1cf-581">`T` はクラス、構造体、または列挙型です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-581">`T` is a class, structure, or enumerated type.</span></span>

* <span data-ttu-id="ac1cf-582">`T` は、非ジェネリックデリゲートまたはインターフェイス型です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-582">`T` is a non-generic delegate or interface type.</span></span>

* <span data-ttu-id="ac1cf-583">`T` は、要素型が有効な contravariantly である配列型です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-583">`T` is an array type whose element type is valid contravariantly.</span></span>

* <span data-ttu-id="ac1cf-584">`T` は、型パラメーターとして宣言されていない型パラメーターです。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-584">`T` is a type parameter which was not declared as an In type parameter.</span></span>

* <span data-ttu-id="ac1cf-585">`T` は、次のように、型引数 `X(Of P1,...,Pn)` を持つ、構築されたインターフェイスまたはデリゲート型 @no__t です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-585">`T` is a constructed interface or delegate type `X(Of P1,...,Pn)` with type arguments `A1,...,An` such that:</span></span>

  * <span data-ttu-id="ac1cf-586">@No__t-0 が `Out` の型パラメーターとして宣言されている場合、`Ai` は有効な contravariantly です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-586">If `Pi` was declared as an `Out` type parameter then `Ai` is valid contravariantly.</span></span>

  * <span data-ttu-id="ac1cf-587">@No__t-0 が `In` の型パラメーターとして宣言されている場合、`Ai` は有効です。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-587">If `Pi` was declared as an `In` type parameter then `Ai` is valid covariantly.</span></span>

<span data-ttu-id="ac1cf-588">次のは、インターフェイスまたはデリゲート型の有効な contravariantly である必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-588">The following must be valid contravariantly in an interface or delegate type:</span></span>

* <span data-ttu-id="ac1cf-589">パラメーターの型。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-589">The type of a parameter.</span></span>

* <span data-ttu-id="ac1cf-590">メソッド型パラメーターの型制約。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-590">A type constraint on a method type parameter.</span></span>

* <span data-ttu-id="ac1cf-591">@No__t 0 のアクセサーがある場合のプロパティの型。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-591">The type of a property if it has a `Set` accessor.</span></span>

* <span data-ttu-id="ac1cf-592">イベントの種類。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-592">The type of an event.</span></span>

<span data-ttu-id="ac1cf-593">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-593">For example:</span></span>

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

<span data-ttu-id="ac1cf-594">型が有効である必要がある場合 (@no__t 0 と `Set` の両方のアクセサーを持つプロパティや、`ByRef` のパラメーター)、バリアント型のパラメーターは使用できません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-594">In the case where a type must be valid be contravariantly and covariantly (such as a property with both a `Get` and `Set` accessor or a `ByRef` parameter), a variant type parameter cannot be used.</span></span>


<span data-ttu-id="ac1cf-595">同時変性と反変性により、"ダイヤモンドのあいまいさに関する問題" が増加します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-595">Co- and contra-variance give rise to a "diamond ambiguity problem".</span></span> <span data-ttu-id="ac1cf-596">次のコードがあるとします。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-596">Consider the following code:</span></span>

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

<span data-ttu-id="ac1cf-597">クラス `C` は、2つの方法で `IEnumerable(Of Object)` に変換できます。これには、`IEnumerable(Of String)` からの共変の変換と、`IEnumerable(Of Exception)` からの共変の変換を使用します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-597">The class `C` can be converted to `IEnumerable(Of Object)` in two ways, both through covariant conversion from `IEnumerable(Of String)` and through covariant conversion from `IEnumerable(Of Exception)`.</span></span> <span data-ttu-id="ac1cf-598">CLR では、2つのメソッドのどちらが `c.GetEnumerator()` によって呼び出されるかは指定されていません。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-598">The CLR does not specify which of the two methods will be called by `c.GetEnumerator()`.</span></span> <span data-ttu-id="ac1cf-599">一般に、共通のスーパータイプを持つ2つの異なるジェネリック引数を持つ共変のインターフェイスを実装するようにクラスが宣言されている場合 (この例では `String` と @no__t に共通のスーパータイプ `Object`)、またはクラスがを実装するように宣言されています。共通のサブタイプを持つ2つの異なるジェネリック引数を持つ反変のインターフェイスは、あいまいさが発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-599">In general, whenever a class is declared to implement a covariant interface with two different generic arguments that have a common supertype (e.g. in this case `String` and `Exception` have the common supertype `Object`), or a class is declared to implement a contravariant interface with two different generic arguments that have a common subtype, then ambiguity is likely to arise.</span></span> <span data-ttu-id="ac1cf-600">コンパイラは、このような宣言に対して警告を出します。</span><span class="sxs-lookup"><span data-stu-id="ac1cf-600">The compiler gives a warning on such declarations.</span></span>
