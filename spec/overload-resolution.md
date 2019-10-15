---
ms.openlocfilehash: a9499e51a67a9b311ae54410c001f8bd22c30d65
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306069"
---
# <a name="overloaded-method-resolution"></a><span data-ttu-id="c06e7-101">オーバーロードされたメソッドの解決</span><span class="sxs-lookup"><span data-stu-id="c06e7-101">Overloaded Method Resolution</span></span>

<span data-ttu-id="c06e7-102">実際には、オーバーロードの解決を決定するための規則は、指定された実際の引数に "最も近い" オーバーロードを見つけることを目的としています。</span><span class="sxs-lookup"><span data-stu-id="c06e7-102">In practice, the rules for determining overload resolution are intended to find the overload that is "closest" to the actual arguments supplied.</span></span> <span data-ttu-id="c06e7-103">パラメーターの型が引数の型と一致するメソッドがある場合、そのメソッドは明らかに最も近いものになります。</span><span class="sxs-lookup"><span data-stu-id="c06e7-103">If there is a method whose parameter types match the argument types, then that method is obviously the closest.</span></span> <span data-ttu-id="c06e7-104">そのようにしないと、1つのメソッドは、他のメソッドのパラメーター型よりも幅が狭く (または同じである)、他のメソッドよりも近くにあります。</span><span class="sxs-lookup"><span data-stu-id="c06e7-104">Barring that, one method is closer than another if all of its parameter types are narrower than (or the same as) the parameter types of the other method.</span></span> <span data-ttu-id="c06e7-105">どちらのパラメーターも他のパラメーターよりも狭い場合、では、どのメソッドが引数に近いかを判断する方法はありません。</span><span class="sxs-lookup"><span data-stu-id="c06e7-105">If neither method's parameters are narrower than the other, then there is no way for to determine which method is closer to the arguments.</span></span>

<span data-ttu-id="c06e7-106">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="c06e7-106">__Note.__</span></span> <span data-ttu-id="c06e7-107">オーバーロードの解決では、メソッドの予期される戻り値の型は考慮されません。</span><span class="sxs-lookup"><span data-stu-id="c06e7-107">Overload resolution does not take into account the expected return type of the method.</span></span> 

<span data-ttu-id="c06e7-108">また、名前付きパラメーターの構文が原因で、実際のパラメーターと仮パラメーターの順序が同じでない場合もあることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="c06e7-108">Also note that because of the named parameter syntax, the ordering of the actual and formal parameters may not be the same.</span></span>

<span data-ttu-id="c06e7-109">メソッドグループが指定されている場合、引数リストのグループ内の最も適切なメソッドは、次の手順を使用して決定されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-109">Given a method group, the most applicable method in the group for an argument list is determined using the following steps.</span></span> <span data-ttu-id="c06e7-110">特定のステップを適用した後に、セット内にメンバーが残っていない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-110">If, after applying a particular step, no members remain in the set, then a compile-time error occurs.</span></span> <span data-ttu-id="c06e7-111">セットに含まれるメンバーが1つだけの場合は、そのメンバーが最も適切なメンバーになります。</span><span class="sxs-lookup"><span data-stu-id="c06e7-111">If only one member remains in the set, then that member is the most applicable member.</span></span> <span data-ttu-id="c06e7-112">手順は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="c06e7-112">The steps are:</span></span>

1.  <span data-ttu-id="c06e7-113">まず、型引数が指定されていない場合は、型パラメーターを持つ任意のメソッドに型の推定を適用します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-113">First, if no type arguments have been supplied, apply type inference to any methods which have type parameters.</span></span> <span data-ttu-id="c06e7-114">メソッドに対して型の推定が成功すると、その特定のメソッドに対して推論される型引数が使用されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-114">If type inference succeeds for a method, then the inferred type arguments are used for that particular method.</span></span> <span data-ttu-id="c06e7-115">メソッドの型の推定が失敗した場合、そのメソッドはセットから除外されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-115">If type inference fails for a method, then that method is eliminated from the set.</span></span>

2.  <span data-ttu-id="c06e7-116">次に、アクセスできない、または適用されていないセットのすべてのメンバー[を引数リスト](overload-resolution.md#applicability-to-argument-list)に対して削除します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-116">Next, eliminate all members from the set that are inaccessible or not applicable (Section [Applicability To Argument List](overload-resolution.md#applicability-to-argument-list)) to the argument list</span></span>

3.  <span data-ttu-id="c06e7-117">次に、1つ以上の引数が @no__t 0 またはラムダ式である場合は、次のように各引数の*デリゲート緩和レベル*を計算します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-117">Next, if one or more arguments are `AddressOf` or lambda expressions, then calculate the *delegate relaxation levels* for each such argument as below.</span></span> <span data-ttu-id="c06e7-118">@No__t-0 の最も低い (最低) デリゲート緩和 level が `M` の最小デリゲート緩和レベルよりも悪い場合は、セットから `N` を削除します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-118">If the worst (lowest) delegate relaxation level in `N` is worse than the lowest delegate relaxation level in `M`, then eliminate `N` from the set.</span></span> <span data-ttu-id="c06e7-119">Delegate 緩和レベルは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="c06e7-119">The delegate relaxation levels are as follows:</span></span>

    31. <span data-ttu-id="c06e7-120">*緩和レベルを委任*するときにエラーが発生しました--`AddressOf` またはラムダをデリゲート型に変換できません。</span><span class="sxs-lookup"><span data-stu-id="c06e7-120">*Error delegate relaxation level* -- if the `AddressOf` or lambda cannot be converted to the delegate type.</span></span>
  
    32. <span data-ttu-id="c06e7-121">*戻り値の型またはパラメーターの縮小デリゲート緩和*--引数が `AddressOf`、または宣言された型のラムダで、戻り値の型からデリゲートの戻り値の型への変換が縮小されます。または、引数が標準のラムダで、その戻り式からデリゲートの戻り値の型への変換が縮小されている場合、または引数が非同期ラムダであり、デリゲートの戻り値の型が `Task(Of T)` で、その戻り式からの変換がの場合は、@no__ t-3 は縮小です。または、引数が反復子ラムダであり、デリゲートの戻り値の型が-4 または @no__t @no__t で、その yield オペランドから `T` への変換が縮小されている場合は。</span><span class="sxs-lookup"><span data-stu-id="c06e7-121">*Narrowing delegate relaxation of return type or parameters* -- if the argument is `AddressOf` or a lambda with a declared type and the conversion from its return type to the delegate return type is narrowing; or if the argument is a regular lambda and the conversion from any of its return expressions to the delegate return type is narrowing, or if the argument is an async lambda and the delegate return type is `Task(Of T)` and the conversion from any of its return expressions to `T` is narrowing; or if the argument is an iterator lambda and the delegate return type `IEnumerator(Of T)` or `IEnumerable(Of T)` and the conversion from any of its yield operands to `T` is narrowing.</span></span>

    33. <span data-ttu-id="c06e7-122">デリゲート*緩和をシグネチャなしでデリゲートするように拡張*しました--デリゲート型が `System.Delegate` または `System.MultiCastDelegate` または `System.Object`。</span><span class="sxs-lookup"><span data-stu-id="c06e7-122">*Widening delegate relaxation to delegate without signature* -- if delegate type is `System.Delegate` or `System.MultiCastDelegate` or `System.Object`.</span></span>

    34. <span data-ttu-id="c06e7-123">*Drop return または arguments delegate 緩和*--引数が `AddressOf`、または宣言された戻り値の型を持つラムダで、デリゲート型に戻り値の型がありません。引数が1つ以上の return 式を持つラムダであり、デリゲート型に戻り値の型がない場合は。引数が `AddressOf` またはラムダのパラメーターがなく、デリゲート型にパラメーターがある場合は。</span><span class="sxs-lookup"><span data-stu-id="c06e7-123">*Drop return or arguments delegate relaxation* -- if the argument is `AddressOf` or a lambda with a declared return type and the delegate type lacks a return type; or if the argument is a lambda with one or more return expressions and the delegate type lacks a return type; or if the argument is `AddressOf` or lambda with no parameters and the delegate type has parameters.</span></span>

    35. <span data-ttu-id="c06e7-124">*緩和戻り値の型の拡大デリゲートの*--引数が-1 または宣言 @no__t された戻り値の型を持つラムダで、戻り値の型からデリゲートの型への拡大変換がある場合は、または、引数が通常のラムダであり、すべての戻り式からデリゲートの戻り値の型への変換が、少なくとも1つの拡大を伴う、拡大または id である場合、または、引数が非同期ラムダであり、デリゲートが `Task(Of T)` または `Task` で、すべての戻り式から `T` @ no__t-5 に変換する場合は、それぞれ、少なくとも1つの拡大を含む拡大または id です。または、引数が反復子ラムダであり、デリゲートが `IEnumerator(Of T)` または `IEnumerable(Of T)` または `IEnumerator` または `AddressOf`0 であり、すべての戻り式から 1 @ no__t-12 @ no__t に変換する場合は、少なくとも1つの拡大があります。</span><span class="sxs-lookup"><span data-stu-id="c06e7-124">*Widening delegate relaxation of return type* -- if the argument is `AddressOf` or a lambda with a declared return type, and there is a widening conversion from its return type to that of the delegate; or if the argument is a regular lambda where the conversion from all return expressions to the delegate return type is widening or identity with at least one widening; or if the argument is an async lambda and the delegate is `Task(Of T)` or `Task` and the conversion from all return expressions to `T`/`Object` respectively is widening or identity with at least one widening; or if the argument is an iterator lambda and the delegate is `IEnumerator(Of T)` or `IEnumerable(Of T)` or `IEnumerator` or `IEnumerable` and the conversion from all return expressions to `T`/`Object` is widening or identity with at least one widening.</span></span>

    36. <span data-ttu-id="c06e7-125">*Id デリゲート緩和*--引数が `AddressOf` またはデリゲートと完全に一致するラムダ (パラメーターの拡大/縮小または削除を使用しない)、またはを返すまたはを返します。次に、セットの一部のメンバーが、引数のいずれかに適用される縮小変換を必要としない場合は、を実行するすべてのメンバーを削除します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-125">*Identity delegate relaxation* -- if the argument is an `AddressOf` or a lambda which matches the delegate exactly, with no widening or narrowing or dropping of parameters or returns or yields.Next, if some members of the set do not requiring narrowing conversions to be applicable to any of the arguments, then eliminate all members that do.</span></span> <span data-ttu-id="c06e7-126">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-126">For example:</span></span>

    ```vb
    Sub f(x As Object)
    End Sub

    Sub f(x As Short)
    End Sub

    Sub f(x As Short())
    End Sub

    f("5") ' picks the Object overload, since String->Short is narrowing
    f(5)   ' picks the Object overload, since Integer->Short is narrowing
    f({5}) ' picks the Object overload, since Integer->Short is narrowing
    f({})  ' a tie-breaker rule subsequent to [3] picks the Short() overload

    ```

4.  <span data-ttu-id="c06e7-127">次に、次のように縮小に基づいて削除を行います。</span><span class="sxs-lookup"><span data-stu-id="c06e7-127">Next, elimination is done based on narrowing as follows.</span></span> <span data-ttu-id="c06e7-128">(Option Strict がオンの場合、縮小が必要なすべてのメンバーは、既に適用されていないと判断されていることに注意してください (「[引数リストへの適用](overload-resolution.md#applicability-to-argument-list)」セクションを参照してください)。</span><span class="sxs-lookup"><span data-stu-id="c06e7-128">(Note that, if Option Strict is On, then all members that require narrowing have already been judged inapplicable (Section [Applicability To Argument List](overload-resolution.md#applicability-to-argument-list)) and removed by Step 2.)</span></span>

    41. <span data-ttu-id="c06e7-129">Set の一部のインスタンスメンバーで、引数の式の型が 0 @no__t である縮小変換のみが必要な場合は、他のすべてのメンバーを削除します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-129">If some instance members of the set only require narrowing conversions where the argument expression type is `Object`, then eliminate all other members.</span></span>
    42. <span data-ttu-id="c06e7-130">@No__t-0 からの縮小のみを必要とするメンバーがセットに複数含まれている場合、呼び出し先の式は遅延バインディングされたメソッドアクセスとして再分類されます (メソッドグループを含む型がインターフェイスの場合、または、適用可能なメンバーは拡張メンバーでした)。</span><span class="sxs-lookup"><span data-stu-id="c06e7-130">If the set contains more than one member which requires narrowing only from `Object`, then the invocation target expression is reclassified as a late-bound method access (and an error is given if the type containing the method group is an interface, or if any of the applicable members were extension members).</span></span>
    43. <span data-ttu-id="c06e7-131">数値リテラルからの縮小のみを必要とする候補がある場合は、次の手順で残りのすべての候補の中から最も限定的なものを選択します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-131">If there are any candidates that only require narrowing from numeric literals, then chose the most specific among all remaining candidates by the steps below.</span></span> <span data-ttu-id="c06e7-132">勝者が数値リテラルのみを縮小する必要がある場合は、オーバーロードの解決の結果として選択されます。それ以外の場合はエラーになります。</span><span class="sxs-lookup"><span data-stu-id="c06e7-132">If the winner requires only narrowing from numeric literals, then it is picked as the result of overload resolution; otherwise it is an error.</span></span>

    <span data-ttu-id="c06e7-133">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="c06e7-133">__Note.__</span></span> <span data-ttu-id="c06e7-134">この規則の根拠は、プログラムが弱い型指定の場合 (つまり、ほとんどまたはすべての変数が @no__t 0 として宣言されている場合)、`Object` からの多くの変換が縮小されると、オーバーロードの解決が困難になることがあります。</span><span class="sxs-lookup"><span data-stu-id="c06e7-134">The justification for this rule is that if a program is loosely-typed (that is, most or all variables are declared as `Object`), overload resolution can be difficult when many conversions from `Object` are narrowing.</span></span> <span data-ttu-id="c06e7-135">多くの状況でオーバーロードの解決が失敗する (メソッド呼び出しに引数を厳密に入力する必要がある) のではなく、呼び出しに適したオーバーロードされたメソッドが実行時まで遅延されることを解決します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-135">Rather than have the overload resolution fail in many situations (requiring strong typing of the arguments to the method call), resolution the appropriate overloaded method to call is deferred until run time.</span></span> <span data-ttu-id="c06e7-136">これにより、緩やかに型指定された呼び出しを、追加のキャストなしで成功させることができます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-136">This allows the loosely-typed call to succeed without additional casts.</span></span> <span data-ttu-id="c06e7-137">ただし、このような副作用は残念ですが、遅延バインディングの呼び出しを実行するには、呼び出しターゲットを `Object` にキャストする必要があります。</span><span class="sxs-lookup"><span data-stu-id="c06e7-137">An unfortunate side-effect of this, however, is that performing the late-bound call requires casting the call target to `Object`.</span></span> <span data-ttu-id="c06e7-138">構造体の値の場合、これは値が一時的なにボックス化される必要があることを意味します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-138">In the case of a structure value, this means that the value must be boxed to a temporary.</span></span> <span data-ttu-id="c06e7-139">メソッドが最終的に構造体のフィールドを変更しようとした場合、メソッドが返されると、この変更は失われます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-139">If the method eventually called tries to change a field of the structure, this change will be lost once the method returns.</span></span> <span data-ttu-id="c06e7-140">遅延バインディングは常にランタイムクラスまたは構造体型のメンバーに対して解決されるため、インターフェイスはこの特別な規則から除外されます。これは、実装するインターフェイスのメンバーとは名前が異なる場合があります。</span><span class="sxs-lookup"><span data-stu-id="c06e7-140">Interfaces are excluded from this special rule because late binding always resolves against the members of the runtime class or structure type, which may have different names than the members of the interfaces they implement.</span></span>

5.  <span data-ttu-id="c06e7-141">次に、縮小が不要なインスタンスメソッドがセットに残っている場合は、セットからすべての拡張メソッドを削除します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-141">Next, if any instance methods remain in the set which do not require narrowing, then eliminate all extension methods from the set.</span></span> <span data-ttu-id="c06e7-142">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-142">For example:</span></span>

    ```vb
    Imports System.Runtime.CompilerServices

    Class C3
        Sub M1(d As Integer)
        End Sub
    End Class

    Module C3Extensions
        <Extension> _
        Sub M1(c3 As C3, c As Long)
        End Sub

        <Extension> _
        Sub M1(c3 As C3, c As Short)
        End Sub
    End Module

    Module Test
        Sub Main()
            Dim c As New C3()
            Dim sVal As Short = 10
            Dim lVal As Long = 20

            ' Calls C3.M1, since C3.M1 is applicable.
            c.M1(sVal)

            ' Calls C3Extensions.M1 since C3.M1 requires a narrowing conversion
            c.M1(lVal)
        End Sub
    End Module
    ```

    <span data-ttu-id="c06e7-143">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="c06e7-143">__Note.__</span></span> <span data-ttu-id="c06e7-144">拡張メソッドは、(新しい拡張メソッドをスコープに取り込む可能性がある) インポートの追加によって既存のインスタンスメソッドに対する呼び出しが拡張メソッドに再バインドされないことを保証するために、適用可能なインスタンスメソッドがある場合は無視されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-144">Extension methods are ignored if there are applicable instance methods to guarantee that adding an import (that might bring new extension methods into scope) will not cause a call on an existing instance method to rebind to an extension method.</span></span> <span data-ttu-id="c06e7-145">一部の拡張メソッド (インターフェイスや型パラメーターで定義されたもの) の範囲が広いため、これは拡張メソッドにバインドするためのより安全な方法です。</span><span class="sxs-lookup"><span data-stu-id="c06e7-145">Given the broad scope of some extension methods (i.e. those defined on interfaces and/or type parameters), this is a safer approach to binding to extension methods.</span></span>

6.  <span data-ttu-id="c06e7-146">次に、セットの2つのメンバー `M` と `N` が指定されている場合は、引数リストで指定された `N` よりも `M` がより*具体的*に ([引数リストを指定したメンバー/型の特定の](overload-resolution.md#specificity-of-memberstypes-given-an-argument-list)部分)、set から @no__t 6 が削除されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-146">Next, if, given any two members of the set `M` and `N`, `M` is more *specific* (Section [Specificity of members/types given an argument list](overload-resolution.md#specificity-of-memberstypes-given-an-argument-list)) than `N` given the argument list, eliminate `N` from the set.</span></span> <span data-ttu-id="c06e7-147">セットに複数のメンバーが残っており、その他のメンバーが引数リストに等しくない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-147">If more than one member remains in the set and the remaining members are not equally specific given the argument list, a compile-time error results.</span></span>

7.  <span data-ttu-id="c06e7-148">それ以外の場合は、セットの2つのメンバー (`M` と `N`) を指定した場合、次の規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-148">Otherwise, given any two members of the set, `M` and `N`, apply the following tie-breaking rules, in order:</span></span>

    71. <span data-ttu-id="c06e7-149">@No__t-0 に ParamArray パラメーターが指定されておらず `N` がの場合、または両方の操作 @no__t で ParamArray パラメーターに渡される引数が `N` の場合よりも少なくなる場合は、set から @no__t 4 を削除します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-149">If `M` does not have a ParamArray parameter but `N` does, or if both do but `M` passes fewer arguments into the ParamArray parameter than `N` does, then eliminate `N` from the set.</span></span> <span data-ttu-id="c06e7-150">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-150">For example:</span></span>

        ```vb
        Module Test
            Sub F(a As Object, ParamArray b As Object())
                Console.WriteLine("F(Object, Object())")
            End Sub

            Sub F(a As Object, b As Object, ParamArray c As Object())
                Console.WriteLine("F(Object, Object, Object())")
            End Sub

           Sub G(Optional a As Object = Nothing)
              Console.WriteLine("G(Object)")
           End Sub

           Sub G(ParamArray a As Object())
              Console.WriteLine("G(Object())")
           End Sub    Sub Main()
                F(1)
                F(1, 2)
                F(1, 2, 3)
              G()
            End Sub
        End Module
        ```

        <span data-ttu-id="c06e7-151">上記の例では、次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-151">The above example produces the following output:</span></span>

        ```console
        F(Object, Object())
        F(Object, Object, Object())
        F(Object, Object, Object())
        G(Object)
        ```

        <span data-ttu-id="c06e7-152">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="c06e7-152">__Note.__</span></span> <span data-ttu-id="c06e7-153">クラスが paramarray パラメーターを使用してメソッドを宣言する場合、一部の拡張されたフォームも通常のメソッドとして含めることは珍しくありません。</span><span class="sxs-lookup"><span data-stu-id="c06e7-153">When a class declares a method with a paramarray parameter, it is not uncommon to also include some of the expanded forms as regular methods.</span></span> <span data-ttu-id="c06e7-154">これにより、paramarray パラメーターを使用して拡張された形式のメソッドが呼び出されたときに発生する配列インスタンスの割り当てを回避できます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-154">By doing so it is possible to avoid the allocation of an array instance that occurs when an expanded form of a method with a paramarray parameter is invoked.</span></span>

    72. <span data-ttu-id="c06e7-155">@No__t-0 が `N` よりも多くの派生型で定義されている場合は、セットから `N` を削除します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-155">If `M` is defined in a more derived type than `N`, eliminate `N` from the set.</span></span> <span data-ttu-id="c06e7-156">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-156">For example:</span></span>

        ```vb
        Class Base
            Sub F(Of T, U)(x As T, y As U)
            End Sub
        End Class

        Class Derived
            Inherits Base

            Overloads Sub F(Of T, U)(x As U, y As T)
            End Sub
        End Class

        Module Test
            Sub Main()
                Dim d As New Derived()

                ' Calls Derived.F
                d.F(10, 10)
            End Sub
        End Module
        ```

        <span data-ttu-id="c06e7-157">この規則は、拡張メソッドが定義されている型にも適用されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-157">This rule also applies to the types that extension methods are defined on.</span></span> <span data-ttu-id="c06e7-158">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-158">For example:</span></span>

        ```vb
        Imports System.Runtime.CompilerServices

        Class Base
        End Class

        Class Derived
            Inherits Base
        End Class

        Module BaseExt
            <Extension> _
            Sub M(b As Base, x As Integer)
            End Sub
        End Module

        Module DerivedExt
            <Extension> _
            Sub M(d As Derived, x As Integer)
            End Sub
        End Module

        Module Test
            Sub Main()
                Dim b As New Base()
                Dim d As New Derived()

                ' Calls BaseExt.M
                b.M(10)

                ' Calls DerivedExt.M 
                d.M(10)
            End Sub
        End Module
        ```

    73. <span data-ttu-id="c06e7-159">@No__t-0 および `N` が拡張メソッドであり、`M` のターゲット型がクラスまたは構造体であり、`N` のターゲット型がインターフェイスである場合は、セットから @no__t 4 を削除します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-159">If `M` and `N` are extension methods and the target type of `M` is a class or structure and the target type of `N` is an interface, eliminate `N` from the set.</span></span> <span data-ttu-id="c06e7-160">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-160">For example:</span></span>

        ```vb
        Imports System.Runtime.CompilerServices

        Interface I1
        End Interface

        Class C1
            Implements I1
        End Class

        Module Ext1
            <Extension> _
            Sub M(i As I1, x As Integer)
            End Sub
        End Module

        Module Ext2
            <Extension> _
            Sub M(c As C1, y As Integer)
            End Sub
        End Module

        Module Test
            Sub Main()
                Dim c As New C1()

                ' Calls Ext2.M, because Ext1.M is hidden since it extends
                ' an interface.
                c.M(10)

                ' Calls Ext1.M
                CType(c, I1).M(10)
            End Sub
        End Module
        ```

    74. <span data-ttu-id="c06e7-161">@No__t-0 および `N` が拡張メソッドであり、`M` と `N` のターゲット型が型パラメーターの置換後に同一であり、型パラメーターの置換の前に @no__t のターゲット型が型パラメーターを含まないが、ターゲットの型がである場合`N` は、@no__t の対象の型よりも少数の型パラメーターを持つため、セットから `N` を削除します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-161">If `M` and `N` are extension methods, and the target type of `M` and `N` are identical after type parameter substitution, and the target type of `M` before type parameter substitution does not contain type parameters but the target type of `N` does, then has fewer type parameters than the target type of `N`, eliminate `N` from the set.</span></span> <span data-ttu-id="c06e7-162">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-162">For example:</span></span>

        ```vb
        Imports System.Runtime.CompilerServices

        Module Module1
            Sub Main()
                Dim x As Integer = 1
                x.f(1) ' Calls first "f" extension method

                Dim y As New Dictionary(Of Integer, Integer)
                y.g(1) ' Ambiguity error
            End Sub

            <Extension()> Sub f(x As Integer, z As Integer)
            End Sub

            <Extension()> Sub f(Of T)(x As T, z As T)
            End Sub

            <Extension()> Sub g(Of T)(y As Dictionary(Of T, Integer), z As T)
            End Sub

            <Extension()> Sub g(Of T)(y As Dictionary(Of T, T), z As T)
            End Sub
        End Module
        ```

    75. <span data-ttu-id="c06e7-163">型引数が置換される前に、`M` が `N` よりも*少ないジェネリック*(セクション[Genericity](overload-resolution.md#genericity)) の場合は、セットから `N` を削除します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-163">Before type arguments have been substituted, if `M` is *less generic* (Section [Genericity](overload-resolution.md#genericity)) than `N`, eliminate `N` from the set.</span></span>

    76. <span data-ttu-id="c06e7-164">@No__t-0 が拡張メソッドではなく、`N` の場合は、セットから `N` を削除します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-164">If `M` is not an extension method and `N` is, eliminate `N` from the set.</span></span>

    77. <span data-ttu-id="c06e7-165">@No__t-0 および `N` が拡張メソッドであり、`N` (セクション[拡張メソッドのコレクション](expressions.md#extension-method-collection)) の前に `M` が見つかった場合は、セットから @no__t を削除します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-165">If `M` and `N` are extension methods and `M` was found before `N` (Section [Extension Method Collection](expressions.md#extension-method-collection)), eliminate `N` from the set.</span></span> <span data-ttu-id="c06e7-166">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-166">For example:</span></span>

        ```vb
        Imports System.Runtime.CompilerServices

        Class C1
        End Class

        Namespace N1
            Module N1C1Extensions
                <Extension> _
                Sub M1(c As C1, x As Integer)
                End Sub
            End Module
        End Namespace

        Namespace N1.N2
            Module N2C1Extensions
                <Extension> _
                Sub M1(c As C1, y As Integer)
                End Sub
            End Module
        End Namespace

        Namespace N1.N2.N3
            Module Test
                Sub Main()
                    Dim x As New C1()

                    ' Calls N2C1Extensions.M1
                    x.M1(10)
                End Sub
            End Module
        End Namespace
        ```

        <span data-ttu-id="c06e7-167">拡張メソッドが同じ手順で見つかった場合、これらの拡張メソッドはあいまいです。</span><span class="sxs-lookup"><span data-stu-id="c06e7-167">If the extension methods were found in the same step, then those extension methods are ambiguous.</span></span> <span data-ttu-id="c06e7-168">この呼び出しは、拡張メソッドを含む標準モジュールの名前を使用し、通常のメンバーであるかのように拡張メソッドを呼び出すことで、常に明確することができます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-168">The call may always be disambiguated using the name of the standard module containing the extension method and calling the extension method as if it was a regular member.</span></span> <span data-ttu-id="c06e7-169">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-169">For example:</span></span>

        ```vb
        Imports System.Runtime.CompilerServices

        Class C1
        End Class

        Module C1ExtA
            <Extension> _
            Sub M(c As C1)
            End Sub
        End Module

        Module C1ExtB
            <Extension> _
            Sub M(c As C1)
            End Sub
        End Module

        Module Main
            Sub Test()
                Dim c As New C1()

                C1.M()               ' Ambiguous between C1ExtA.M and BExtB.M
                C1ExtA.M(c)          ' Calls C1ExtA.M
                C1ExtB.M(c)          ' Calls C1ExtB.M
            End Sub
        End Module
        ```

    78. <span data-ttu-id="c06e7-170">@No__t-0 および `N` が型引数を生成するために必要な型の推論の両方である場合、`M` では、型引数のいずれかに対して最も優先される型を決定する必要 @no__t @no__t がありませんでした (つまり、1つの型に対して推論される型引数)。を設定します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-170">If `M` and `N` both required type inference to produce type arguments, and `M` did not require determining the dominant type for any of its type arguments (i.e. each the type arguments inferred to a single type), but `N` did, eliminate `N` from the set.</span></span>

        <span data-ttu-id="c06e7-171">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="c06e7-171">__Note.__</span></span> <span data-ttu-id="c06e7-172">この規則により、以前のバージョンで成功したオーバーロードの解決 (型引数に対して複数の型を推論するとエラーが発生する) が確実になり、同じ結果が生成されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-172">This rule ensures that overload resolution that succeeded in previous versions (where inferring multiple types for a type argument would cause an error), continue to produce the same results.</span></span>

    79. <span data-ttu-id="c06e7-173">@No__t-0 式からデリゲート作成式のターゲットを解決するためにオーバーロードの解決が行われていて、デリゲートと `M` の両方が関数である場合、`N` はサブルーチンであるため、セットから `N` を削除します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-173">If overload resolution is being done to resolve the target of a delegate-creation expression from an `AddressOf` expression, and both the delegate and `M` are functions while `N` is a subroutine, eliminate `N` from the set.</span></span> <span data-ttu-id="c06e7-174">同様に、デリゲートと @no__t 0 の両方がサブルーチンで、`N` が関数の場合は、セットから `N` を削除します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-174">Likewise, if both the delegate and `M` are subroutines, while `N` is a function, eliminate `N` from the set.</span></span>

    710. <span data-ttu-id="c06e7-175">@No__t-0 で、明示的な引数の代わりに省略可能なパラメーターの既定値が使用されなかったが、`N` だった場合は、セットから `N` を削除します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-175">If `M` did not use any optional parameter defaults in place of explicit arguments, but `N` did, then eliminate `N` from the set.</span></span>

    711. <span data-ttu-id="c06e7-176">型引数が置換される前に、`M` が `N` よりも*深さが多い genericity* (セクション[genericity](overload-resolution.md#genericity)) である場合は、セットから `N` を削除します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-176">Before type arguments have been substituted, if `M` has *greater depth of genericity* (Section [Genericity](overload-resolution.md#genericity)) than `N`, then eliminate `N` from the set.</span></span>

8. <span data-ttu-id="c06e7-177">それ以外の場合、呼び出しはあいまいになり、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-177">Otherwise, the call is ambiguous and a compile-time error occurs.</span></span>

#### <a name="specificity-of-memberstypes-given-an-argument-list"></a><span data-ttu-id="c06e7-178">引数リストを指定した場合のメンバー/型の特異性</span><span class="sxs-lookup"><span data-stu-id="c06e7-178">Specificity of members/types given an argument list</span></span>

<span data-ttu-id="c06e7-179">メンバー `M` は、引数リスト `A` が指定されている場合は、@no__t と*同じように扱われ*ます。これらのシグネチャが同じである場合、または `M` の各パラメーターの型が `N` の対応するパラメーターの型と同じである場合。</span><span class="sxs-lookup"><span data-stu-id="c06e7-179">A member `M` is considered *equally specific* as `N`, given an argument-list `A`, if their signatures are the same or if each parameter type in `M` is the same as the corresponding parameter type in `N`.</span></span>

<span data-ttu-id="c06e7-180">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="c06e7-180">__Note.__</span></span> <span data-ttu-id="c06e7-181">2つのメンバーは、拡張メソッドによって同じシグネチャを持つメソッドグループに終了できます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-181">Two members can end up in a method group with the same signature due to extension methods.</span></span> <span data-ttu-id="c06e7-182">2つのメンバーを同じように指定することもできますが、型パラメーターまたは paramarray 拡張のために同じシグネチャを持つことはできません。</span><span class="sxs-lookup"><span data-stu-id="c06e7-182">Two members can also be equally specific but not have the same signature due to type parameters or paramarray expansion.</span></span>

<span data-ttu-id="c06e7-183">メンバー `M` は、署名が異なる場合には @no__t よりも*具体的*であると見なされます。 @no__t 3 のパラメーターの型が少なくとも1つは、`N` のパラメーターの型よりも限定的であり、`N` のパラメーターの型がパラメーターよりも固有ではありません`M` を入力します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-183">A member `M` is considered *more specific* than `N` if their signatures are different and at least one parameter type in `M` is more specific than a parameter type in `N`, and no parameter type in `N` is more specific than a parameter type in `M`.</span></span> <span data-ttu-id="c06e7-184">次のいずれかの条件に該当する場合、-0 と `Nj` の2つの @no__t 引数に一致するパラメーターのペアが指定されている場合は、-3 @no__t の型は、`Nj` の型よりも*具体的*な値と見なされます (@no__t)。</span><span class="sxs-lookup"><span data-stu-id="c06e7-184">Given a pair of parameters `Mj` and `Nj` that matches an argument `Aj`, the type of `Mj` is considered *more specific* than the type of `Nj` if one of the following conditions is true:</span></span>

* <span data-ttu-id="c06e7-185">@No__t-0 の型から型 `Nj` への拡大変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-185">There exists a widening conversion from the type of `Mj` to the type `Nj`.</span></span> <span data-ttu-id="c06e7-186">(__注:__</span><span class="sxs-lookup"><span data-stu-id="c06e7-186">(__Note.__</span></span> <span data-ttu-id="c06e7-187">この場合、パラメーターの型は、実際の引数に関係なく比較されるため、定数式から数値型への拡大変換は、この場合は考慮されません。</span><span class="sxs-lookup"><span data-stu-id="c06e7-187">Because parameters types are being compared without regard to the actual argument in this case, the widening conversion from constant expressions to a numeric type the value fits into is not considered in this case.)</span></span>

* <span data-ttu-id="c06e7-188">`Aj` はリテラル `0`、`Mj` は数値型、`Nj` は列挙型です。</span><span class="sxs-lookup"><span data-stu-id="c06e7-188">`Aj` is the literal `0`, `Mj` is a numeric type and `Nj` is an enumerated type.</span></span> <span data-ttu-id="c06e7-189">(__注:__</span><span class="sxs-lookup"><span data-stu-id="c06e7-189">(__Note.__</span></span> <span data-ttu-id="c06e7-190">この規則は、リテラル `0` が任意の列挙型に拡大変換されるために必要です。</span><span class="sxs-lookup"><span data-stu-id="c06e7-190">This rule is necessary because the literal `0` widens to any enumerated type.</span></span> <span data-ttu-id="c06e7-191">列挙型は基になる型に拡大変換されるため、`0` のオーバーロードの解決では、既定で数値型に対する列挙型が優先されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-191">Since an enumerated type widens to its underlying type, this means that overload resolution on `0` will, by default, prefer enumerated types over numeric types.</span></span> <span data-ttu-id="c06e7-192">この動作が反するていたというフィードバックが多数寄せられています)。</span><span class="sxs-lookup"><span data-stu-id="c06e7-192">We received a lot of feedback that this behavior was counterintuitive.)</span></span>

* <span data-ttu-id="c06e7-193">`Mj` と `Nj` は両方とも数値型です。 @no__t-@no__t 2 は、リスト @no__t 4、`SByte`、@no__t 6、`UShort`、`Integer`、`UInteger`、0、1、2、3、4 のいずれかになります。</span><span class="sxs-lookup"><span data-stu-id="c06e7-193">`Mj` and `Nj` are both numeric types, and `Mj` comes earlier than `Nj` in the list `Byte`, `SByte`, `Short`, `UShort`, `Integer`, `UInteger`, `Long`, `ULong`, `Decimal`, `Single`, `Double`.</span></span> <span data-ttu-id="c06e7-194">(__注:__</span><span class="sxs-lookup"><span data-stu-id="c06e7-194">(__Note.__</span></span> <span data-ttu-id="c06e7-195">数値型に関する規則は便利です。これは、特定のサイズの符号付き数値型と符号なし数値型の間で縮小変換が行われるためです。</span><span class="sxs-lookup"><span data-stu-id="c06e7-195">The rule about the numeric types is useful because the signed and unsigned numeric types of a particular size only have narrowing conversions between them.</span></span> <span data-ttu-id="c06e7-196">上記の規則は、2つの型の間の相互関係を分割し、より "自然な" 数値型を優先します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-196">The above rule breaks the tie between the two types in favor of the more "natural" numeric type.</span></span> <span data-ttu-id="c06e7-197">これは、特定のサイズの符号付きおよび符号なしの数値型の両方に拡大変換される型 (両方に収まる数値リテラルなど) に対してオーバーロードの解決を実行する場合に特に重要です。</span><span class="sxs-lookup"><span data-stu-id="c06e7-197">This is particularly important when doing overload resolution on a type that widens to both the signed and unsigned numeric types of a particular size, for example, a numeric literal that fits into both.)</span></span>

* <span data-ttu-id="c06e7-198">`Mj` および `Nj` はデリゲート関数型であり、`Mj` の戻り値の型は、ラムダメソッドとして分類されている場合は `Nj` の戻り値の型よりも具体的で、`Mj` または `Nj` が @no__t の場合は、型の型引数 (比較する型の代わりに、デリゲート型) が使用されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-198">`Mj` and `Nj` are delegate function types and the return type of `Mj` is more specific than the return type of `Nj` If `Aj` is classified as a lambda method, and `Mj` or `Nj` is `System.Linq.Expressions.Expression(Of T)`, then the type argument of the type (assuming it is a delegate type) is substituted for the type being compared.</span></span>

* <span data-ttu-id="c06e7-199">`Mj` は `Aj` の型と同じであり、`Nj` はと同じではありません。</span><span class="sxs-lookup"><span data-stu-id="c06e7-199">`Mj` is identical to the type of `Aj`, and `Nj` is not.</span></span> <span data-ttu-id="c06e7-200">(__注:__</span><span class="sxs-lookup"><span data-stu-id="c06e7-200">(__Note.__</span></span> <span data-ttu-id="c06e7-201">前の規則はとは若干異なることに注意しC#てください。 C#では、戻り値の型を比較する前に、デリゲート関数型のパラメーターリストが同一である必要がありますが、Visual Basic はそうではありません)。</span><span class="sxs-lookup"><span data-stu-id="c06e7-201">It is interesting to note that the previous rule differs slightly from C#, in that C# requires that the delegate function types have identical parameter lists before comparing return types, while Visual Basic does not.)</span></span>

#### <a name="genericity"></a><span data-ttu-id="c06e7-202">Genericity</span><span class="sxs-lookup"><span data-stu-id="c06e7-202">Genericity</span></span>

<span data-ttu-id="c06e7-203">メンバー `M` は、次のように、メンバー `N` より*汎用的*ではないと判断されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-203">A member `M` is determined to be *less generic* than a member `N` as follows:</span></span>

1. <span data-ttu-id="c06e7-204">一致するパラメーターの各ペアについて、`Mj` および `Nj` の場合、`Mj` は、メソッドの型パラメーターに関しては `Nj` よりも少なく、または等しくありません。また、メソッドの型パラメーターに関して、少なくとも1つの `Mj` がジェネリックになりません。</span><span class="sxs-lookup"><span data-stu-id="c06e7-204">If, for each pair of matching parameters `Mj` and `Nj`, `Mj` is less or equally generic than `Nj` with respect to type parameters on the method, and at least one `Mj` is less generic with respect to type parameters on the method.</span></span>
2. <span data-ttu-id="c06e7-205">それ以外の場合、対応するパラメーターの各ペアに対して-0 および `Nj` の @no__t、`Mj` は、型の型パラメーターに関しては `Nj` よりも少なく、または同等であり、型の型パラメーターに関して少なくとも1つの `Mj` はジェネリックではありませんでは、`M` は `N` よりも一般的ではありません。</span><span class="sxs-lookup"><span data-stu-id="c06e7-205">Otherwise, if for each pair of matching parameters `Mj` and `Nj`, `Mj` is less or equally generic than `Nj` with respect to type parameters on the type, and at least one `Mj` is less generic with respect to type parameters on the type, then `M` is less generic than `N`.</span></span>

<span data-ttu-id="c06e7-206">パラメーター `M` は、パラメーターに対して同じように汎用的と見なされます。 @no__t パラメーターの型 `Mt` と @no__t の両方が型パラメーターを参照しているか、両方とも型パラメーターを参照していない場合は-1 です。</span><span class="sxs-lookup"><span data-stu-id="c06e7-206">A parameter `M` is considered to be equally generic to a parameter `N` if their types `Mt` and `Nt` both refer to type parameters or both don't refer to type parameters.</span></span> <span data-ttu-id="c06e7-207">`M` は、`Mt` が型パラメーターを参照しておらず、`Nt` である場合に `N` よりも汎用的ではないと見なされます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-207">`M` is considered to be less generic than `N` if `Mt` does not refer to a type parameter and `Nt` does.</span></span>

<span data-ttu-id="c06e7-208">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-208">For example:</span></span>

```vb
Class C1(Of T)
    Sub S1(Of U)(x As U, y As T)
    End Sub

    Sub S1(Of U)(x As U, y As U)
    End Sub

    Sub S2(x As Integer, y As T)
    End Sub

    Sub S2(x As T, y As T)
    End Sub
End Class

Module Test
    Sub Main()
        Dim x As C1(Of Integer) = New C1(Of Integer)

        x.S1(10, 10)    ' Calls S1(U, T)
        x.S2(10, 10)    ' Calls S2(Integer, T)
    End Sub
End Module
```

<span data-ttu-id="c06e7-209">カリー化中に修正された拡張メソッドの型パラメーターは、メソッドの型パラメーターではなく、型の型パラメーターと見なされます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-209">Extension method type parameters that were fixed during currying are considered type parameters on the type, not type parameters on the method.</span></span> <span data-ttu-id="c06e7-210">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-210">For example:</span></span>

```vb
Imports System.Runtime.CompilerServices

Module Ext1
    <Extension> _
    Sub M1(Of T, U)(x As T, y As U, z As U)
    End Sub
End Module

Module Ext2
    <Extension> _
    Sub M1(Of T, U)(x As T, y As U, z As T)
    End Sub
End Module

Module Test
    Sub Main()
        Dim i As Integer = 10

        i.M1(10, 10)
    End Sub
End Module
```

#### <a name="depth-of-genericity"></a><span data-ttu-id="c06e7-211">Genericity の深さ</span><span class="sxs-lookup"><span data-stu-id="c06e7-211">Depth of genericity</span></span>

<span data-ttu-id="c06e7-212">@No__t-3 と `Nj` の一致するパラメーターの各ペアに対して、`Mj`*の genericity の深さ*が @no__t の値以上で、少なくとも1つの @no__t が指定されている場合、メンバーの `M` は、@no__t メンバーよりも*詳細な genericity*を持つことになります。の深さは genericity です。</span><span class="sxs-lookup"><span data-stu-id="c06e7-212">A member `M` is determined to have *greater depth of genericity* than a member `N` if, for each pair of matching parameters  `Mj` and `Nj`, `Mj` has greater or equal *depth of genericity* than `Nj`, and at least one `Mj` is has greater depth of genericity.</span></span> <span data-ttu-id="c06e7-213">Genericity の深さは次のように定義されています。</span><span class="sxs-lookup"><span data-stu-id="c06e7-213">Depth of genericity is defined as follows:</span></span>

* <span data-ttu-id="c06e7-214">型パラメーター以外のものは、型パラメーターよりも genericity の深さが多くなります。</span><span class="sxs-lookup"><span data-stu-id="c06e7-214">Anything other than a type parameter has greater depth of genericity than a type parameter;</span></span>

* <span data-ttu-id="c06e7-215">少なくとも1つの型引数の深さが genericity で、型引数の深さが対応する型よりも少ない場合、構築された型は再帰的に (型引数の数が同じである) 他の構築された型と比べて genericity の深さが大きくなります。もう一方の引数。</span><span class="sxs-lookup"><span data-stu-id="c06e7-215">Recursively, a constructed type has greater depth of genericity than another constructed type (with the same number of type arguments) if at least one type argument has greater depth of genericity and no type argument has less depth than the corresponding type argument in the other.</span></span>

* <span data-ttu-id="c06e7-216">配列型は、1番目の要素の型が2番目の要素の型よりも genericity の深さを持つ場合、(次元数が同じ) 別の配列型よりも genericity の深さが大きくなります。</span><span class="sxs-lookup"><span data-stu-id="c06e7-216">An array type has greater depth of genericity than another array type (with the same number of dimensions) if the element type of the first has greater depth of genericity than the element type of the second.</span></span>

<span data-ttu-id="c06e7-217">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-217">For example:</span></span>

```vb
Module Test

    Sub f(Of T)(x As Task(Of T))
    End Sub

    Sub f(Of T)(x As T)
    End Sub

    Sub Main()
        Dim x As Task(Of Integer) = Nothing
        f(x)            ' Calls the first overload
    End Sub
End Module
```

### <a name="applicability-to-argument-list"></a><span data-ttu-id="c06e7-218">引数リストへの適用性</span><span class="sxs-lookup"><span data-stu-id="c06e7-218">Applicability To Argument List</span></span>

<span data-ttu-id="c06e7-219">引数リストを使用してメソッドを呼び出すことができる場合、メソッドは、型引数のセット、位置指定引数、および名前付き引数に*適用*されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-219">A method is *applicable* to a set of type arguments, positional arguments, and named arguments if the method can be invoked using the argument lists.</span></span> <span data-ttu-id="c06e7-220">引数リストは、次のようにパラメーターリストと照合されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-220">The argument lists are matched against the parameter lists as follows:</span></span>

1. <span data-ttu-id="c06e7-221">最初に、各位置引数をメソッドパラメーターの一覧に一致させます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-221">First, match each positional argument in order to the list of method parameters.</span></span> <span data-ttu-id="c06e7-222">パラメーターよりも位置指定引数が多く、最後のパラメーターが paramarray でない場合、メソッドは適用できません。</span><span class="sxs-lookup"><span data-stu-id="c06e7-222">If there are more positional arguments than parameters and the last parameter is not a paramarray, the method is not applicable.</span></span> <span data-ttu-id="c06e7-223">それ以外の場合、paramarray パラメーターは、位置引数の数と一致するように、paramarray 要素型のパラメーターを使用して展開されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-223">Otherwise, the paramarray parameter is expanded with parameters of the paramarray element type to match the number of positional arguments.</span></span> <span data-ttu-id="c06e7-224">Paramarray に入る位置引数を省略した場合、メソッドは適用されません。</span><span class="sxs-lookup"><span data-stu-id="c06e7-224">If a positional argument is omitted that would go into a paramarray, the method is not applicable.</span></span>
2. <span data-ttu-id="c06e7-225">次に、各名前付き引数を、指定した名前のパラメーターに一致させます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-225">Next, match each named argument to a parameter with the given name.</span></span> <span data-ttu-id="c06e7-226">名前付き引数のいずれかが一致しなかった場合、paramarray パラメーターに一致する場合、または既に別の位置指定引数または名前付き引数と一致した引数と一致する場合、メソッドは適用できません。</span><span class="sxs-lookup"><span data-stu-id="c06e7-226">If one of the named arguments fails to match, matches a paramarray parameter, or matches an argument already matched with another positional or named argument, the method is not applicable.</span></span>
3. <span data-ttu-id="c06e7-227">次に、型引数が指定されている場合は、型パラメーターリストと照合されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-227">Next, if type arguments have been specified, they are matched against the type parameter list .</span></span> <span data-ttu-id="c06e7-228">2つのリストに同じ数の要素がない場合、型引数リストが空でない限り、メソッドは適用されません。</span><span class="sxs-lookup"><span data-stu-id="c06e7-228">If the two lists do not have the same number of elements, the method is not applicable, unless the type argument list is empty.</span></span> <span data-ttu-id="c06e7-229">型引数リストが空の場合、型引数リストを試すために型の推定が使用されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-229">If the type argument list is empty, type inference is used to try and infer the type argument list.</span></span> <span data-ttu-id="c06e7-230">Type 推論が失敗した場合、メソッドは適用されません。</span><span class="sxs-lookup"><span data-stu-id="c06e7-230">If type inferencing fails, the method is not applicable.</span></span> <span data-ttu-id="c06e7-231">それ以外の場合、型引数はシグネチャの型パラメーターの代わりに格納されます。一致していないパラメーターが省略可能でない場合、メソッドは適用できません。</span><span class="sxs-lookup"><span data-stu-id="c06e7-231">Otherwise, the type arguments are filled in the place of the type parameters in the signature.If parameters that have not been matched are not optional, the method is not applicable.</span></span>
4. <span data-ttu-id="c06e7-232">引数式が一致するパラメーターの型に暗黙的に変換できない場合、メソッドは適用されません。</span><span class="sxs-lookup"><span data-stu-id="c06e7-232">If the argument expressions are not implicitly convertible to the types of the parameters they match, then the method is not applicable.</span></span>
5. <span data-ttu-id="c06e7-233">パラメーターが ByRef で、パラメーターの型から引数の型への暗黙的な変換がない場合、メソッドは適用されません。</span><span class="sxs-lookup"><span data-stu-id="c06e7-233">If a parameter is ByRef, and there is not an implicit conversion from the type of the parameter to the type of the argument, then the method is not applicable.</span></span>
6. <span data-ttu-id="c06e7-234">型引数がメソッドの制約に違反している場合 (手順3の推定型引数を含む)、メソッドは適用されません。</span><span class="sxs-lookup"><span data-stu-id="c06e7-234">If type arguments violate the method's constraints (including the inferred type arguments from Step 3), the method is not applicable.</span></span> <span data-ttu-id="c06e7-235">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-235">For example:</span></span>

```vb
Module Module1
    Sub Main()
        f(Of Integer)(New Exception)
        ' picks the first overload (narrowing),
        ' since the second overload (widening) violates constraints 
    End Sub

    Sub f(Of T)(x As IComparable)
    End Sub

    Sub f(Of T As Class)(x As Object)
    End Sub
End Module
```

<span data-ttu-id="c06e7-236">単一の引数式が paramarray パラメーターと一致し、引数式の型が paramarray パラメーターの型と paramarray 要素の型の両方に変換可能な場合、メソッドは展開されているフォームと展開されていない形式の両方に適用できます。2つの例外があります。</span><span class="sxs-lookup"><span data-stu-id="c06e7-236">If a single argument expression matches a paramarray parameter and the type of the argument expression is convertible to both the type of the paramarray parameter and the paramarray element type, the method is applicable in both its expanded and unexpanded forms, with two exceptions.</span></span> <span data-ttu-id="c06e7-237">引数式の型から paramarray 型への変換が縮小されている場合、メソッドは拡張された形式でのみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-237">If the conversion from the type of the argument expression to the paramarray type is narrowing, then the method is only applicable in its expanded form.</span></span> <span data-ttu-id="c06e7-238">引数式がリテラル `Nothing` の場合、メソッドは、その展開されていない形式でのみ適用できます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-238">If the argument expression is the literal `Nothing`, then the method is only applicable in its unexpanded form.</span></span> <span data-ttu-id="c06e7-239">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-239">For example:</span></span>

```vb
Module Test
    Sub F(ParamArray a As Object())
        Dim o As Object

        For Each o In a
            Console.Write(o.GetType().FullName)
            Console.Write(" ")
        Next o
        Console.WriteLine()
    End Sub

    Sub Main()
        Dim a As Object() = { 1, "Hello", 123.456 }
        Dim o As Object = a

        F(a)
        F(CType(a, Object))
        F(o)
        F(CType(o, Object()))
    End Sub
End Module
```

<span data-ttu-id="c06e7-240">上記の例では、次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-240">The above example produces the following output:</span></span>

```console
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

<span data-ttu-id="c06e7-241">@No__t-0 の最初と最後の呼び出しでは、引数の型からパラメーターの型 (両方とも `Object()`) への拡大変換が存在し、引数が通常の値のパラメーターとして渡されるため、`F` の通常の形式が適用されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-241">In the first and last invocations of `F`, the normal form of `F` is applicable because a widening conversion exists from the argument type to the parameter type (both are of type `Object()`), and the argument is passed as a regular value parameter.</span></span> <span data-ttu-id="c06e7-242">2番目と3番目の呼び出しでは、引数の型からパラメーターの型への拡大変換が存在しないため、通常の形式の `F` は適用されません (`Object` から `Object()` への変換は縮小)。</span><span class="sxs-lookup"><span data-stu-id="c06e7-242">In the second and third invocations, the normal form of `F` is not applicable because no widening conversion exists from the argument type to the parameter type (conversions from `Object` to `Object()` are narrowing).</span></span> <span data-ttu-id="c06e7-243">ただし、`F` の展開された形式が適用可能であり、1つの要素 @no__t 1 が呼び出しによって作成されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-243">However, the expanded form of `F` is applicable, and a one-element `Object()` is created by the invocation.</span></span> <span data-ttu-id="c06e7-244">配列の1つの要素が、指定された引数の値で初期化されています (それ自体が `Object()` への参照)。</span><span class="sxs-lookup"><span data-stu-id="c06e7-244">The single element of the array is initialized with the given argument value (which itself is a reference to an `Object()`).</span></span>

### <a name="passing-arguments-and-picking-arguments-for-optional-parameters"></a><span data-ttu-id="c06e7-245">渡す (引数を)、省略可能なパラメーターの引数を選択する</span><span class="sxs-lookup"><span data-stu-id="c06e7-245">Passing Arguments, and Picking Arguments for Optional Parameters</span></span>

<span data-ttu-id="c06e7-246">パラメーターが値パラメーターの場合は、一致する引数式を値として分類する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c06e7-246">If a parameter is a value parameter, the matching argument expression must be classified as a value.</span></span> <span data-ttu-id="c06e7-247">値は、パラメーターの型に変換され、実行時にパラメーターとして渡されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-247">The value is converted to the type of the parameter and passed in as the parameter at run time.</span></span> <span data-ttu-id="c06e7-248">パラメーターが参照パラメーターであり、一致する引数式が、パラメーターと同じ型を持つ変数として分類されている場合は、変数への参照が実行時にパラメーターとして渡されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-248">If the parameter is a reference parameter and the matching argument expression is classified as a variable whose type is the same as the parameter, then a reference to the variable is passed in as the parameter at run time.</span></span>

<span data-ttu-id="c06e7-249">それ以外の場合、一致する引数式が変数、値、またはプロパティアクセスとして分類されると、パラメーターの型の一時変数が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-249">Otherwise, if the matching argument expression is classified as a variable, value, or property access, then a temporary variable of the type of the parameter is allocated.</span></span> <span data-ttu-id="c06e7-250">実行時にメソッドが呼び出される前に、引数式が値として再分類され、パラメーターの型に変換され、一時変数に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-250">Before the method invocation at run time, the argument expression is reclassified as a value, converted to the type of the parameter, and assigned to the temporary variable.</span></span> <span data-ttu-id="c06e7-251">次に、一時変数への参照がパラメーターとして渡されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-251">Then a reference to the temporary variable is passed in as the parameter.</span></span> <span data-ttu-id="c06e7-252">メソッドの呼び出しが評価された後、引数式が変数またはプロパティアクセスとして分類されている場合、一時変数は変数式またはプロパティアクセス式に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-252">After the method invocation is evaluated, if the argument expression is classified as a variable or property access, the temporary variable is assigned to the variable expression or the property access expression.</span></span> <span data-ttu-id="c06e7-253">プロパティアクセス式に @no__t 0 のアクセサーがない場合、割り当ては実行されません。</span><span class="sxs-lookup"><span data-stu-id="c06e7-253">If the property access expression has no `Set` accessor, then the assignment is not performed.</span></span>

<span data-ttu-id="c06e7-254">引数が指定されていない省略可能なパラメーターの場合、コンパイラは、次に示すように引数を選択します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-254">For optional parameters where an argument has not been provided, the compiler picks arguments as described below.</span></span> <span data-ttu-id="c06e7-255">いずれの場合も、ジェネリック型の置換後にパラメーター型に対してテストされます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-255">In all cases it tests against the parameter type after generic type substitution.</span></span>

* <span data-ttu-id="c06e7-256">省略可能なパラメーターに `System.Runtime.CompilerServices.CallerLineNumber` という属性があり、呼び出しがソースコード内の場所からのものであり、その位置の行番号を表す数値リテラルにパラメーター型への組み込みの変換がある場合は、数値リテラルが使用されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-256">If the optional parameter has the attribute `System.Runtime.CompilerServices.CallerLineNumber`, and the invocation is from a location in source code, and a numeric literal representing that location's line number has an intrinsic conversion to the parameter type, then the numeric literal is used.</span></span> <span data-ttu-id="c06e7-257">呼び出しが複数行にまたがっている場合、使用する行の選択は実装に依存します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-257">If the invocation spans multiple lines, then the choice of which line to use is implementation-dependent.</span></span>

* <span data-ttu-id="c06e7-258">省略可能なパラメーターに `System.Runtime.CompilerServices.CallerFilePath` という属性があり、呼び出しがソースコード内の場所からのものであり、その場所のファイルパスを表す文字列リテラルにパラメーター型への組み込みの変換がある場合は、文字列リテラルが使用されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-258">If the optional parameter has the attribute `System.Runtime.CompilerServices.CallerFilePath`, and the invocation is from a location in source code, and a string literal representing that location's file path has an intrinsic conversion to the parameter type, then the string literal is used.</span></span> <span data-ttu-id="c06e7-259">ファイルパスの形式は実装に依存します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-259">The format of the file path is implementation-dependent.</span></span>

* <span data-ttu-id="c06e7-260">省略可能なパラメーターの属性が 0 @no__t で、呼び出しが型のメンバーの本体、またはその型のメンバーの任意の部分に適用されている属性に含まれていて、そのメンバー名を表す文字列リテラルがパラメーター型への組み込みの変換を持っている場合。では、文字列リテラルが使用されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-260">If the optional parameter has the attribute `System.Runtime.CompilerServices.CallerMemberName`, and the invocation is within the body of a type member or in an attribute applied to any part of that type member, and a string literal representing that member name has an intrinsic conversion to the parameter type, then the string literal is used.</span></span> <span data-ttu-id="c06e7-261">プロパティアクセサーまたはカスタムイベントハンドラーの一部である呼び出しの場合、使用されるメンバー名は、プロパティまたはイベント自体の名前になります。</span><span class="sxs-lookup"><span data-stu-id="c06e7-261">For invocations that are part of property accessors or custom event handlers, then the member name used is that of the property or event itself.</span></span> <span data-ttu-id="c06e7-262">演算子またはコンストラクターの一部である呼び出しの場合は、実装固有の名前が使用されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-262">For invocations that are part of an operator or constructor, then an implementation-specific name is used.</span></span>

<span data-ttu-id="c06e7-263">上記のいずれも当てはまらない場合は、省略可能なパラメーターの既定値が使用されます (または、既定値が指定されていない場合は `Nothing`)。</span><span class="sxs-lookup"><span data-stu-id="c06e7-263">If none of the above apply, then the optional parameter's default value is used (or `Nothing` if no default value is supplied).</span></span> <span data-ttu-id="c06e7-264">また、上記のいずれかが当てはまる場合は、実装に依存するオプションが使用されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-264">And if more than one of the above apply, then the choice of which to use is implementation-dependent.</span></span>

<span data-ttu-id="c06e7-265">@No__t-0 および `CallerFilePath` 属性は、ログ記録に便利です。</span><span class="sxs-lookup"><span data-stu-id="c06e7-265">The `CallerLineNumber` and `CallerFilePath` attributes are useful for logging.</span></span> <span data-ttu-id="c06e7-266">@No__t-0 は、`INotifyPropertyChanged` を実装する場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="c06e7-266">The `CallerMemberName` is useful for implementing `INotifyPropertyChanged`.</span></span> <span data-ttu-id="c06e7-267">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-267">Here are examples.</span></span>

```vb
Sub Log(msg As String,
        <CallerFilePath> Optional file As String = Nothing,
        <CallerLineNumber> Optional line As Integer? = Nothing)
    Console.WriteLine("{0}:{1} - {2}", file, line, msg)
End Sub

WriteOnly Property p As Integer
    Set(value As Integer)
        Notify(_p, value)
    End Set
End Property

Private _p As Integer

Sub Notify(Of T As IEquatable(Of T))(ByRef v1 As T, v2 As T,
        <CallerMemberName> Optional prop As String = Nothing)
    If v1 IsNot Nothing AndAlso v1.Equals(v2) Then Return
    If v1 Is Nothing AndAlso v2 Is Nothing Then Return
    v1 = v2
    RaiseEvent PropertyChanged(Me, New PropertyChangedEventArgs(prop))
End Sub
```

<span data-ttu-id="c06e7-268">Microsoft Visual Basic は、上記の省略可能なパラメーターに加えて、メタデータからインポートした場合 (つまり、DLL 参照から)、いくつかの追加のオプションパラメーターも認識します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-268">In addition to the optional parameters above, Microsoft Visual Basic also recognizes some additional optional parameters if they are imported from metadata (i.e. from a DLL reference):</span></span>

* <span data-ttu-id="c06e7-269">メタデータからインポートした場合は、パラメーターが省略可能であることを示すためにパラメーター `<Optional>` を Visual Basic も処理します。この方法では、省略可能なパラメーターを持つ宣言をインポートできますが、既定値は指定できません (ただし、これは表現できません)。`Optional` キーワードを使用します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-269">Upon importing from metadata, Visual Basic also treats the parameter `<Optional>` as indicative that the parameter is optional: in this way it is possible to import a declaration which has an optional parameter but no default value, even though this can't be expressed using the `Optional` keyword.</span></span>

* <span data-ttu-id="c06e7-270">省略可能なパラメーターに `Microsoft.VisualBasic.CompilerServices.OptionCompareAttribute` という属性があり、数値リテラル1または0がパラメーター型に変換されている場合、コンパイラは、`Option Compare Text` の場合はリテラル1、有効になっている @no__t 場合はリテラル0のいずれかを引数として使用します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-270">If the optional parameter has the attribute `Microsoft.VisualBasic.CompilerServices.OptionCompareAttribute`, and the numeric literal 1 or 0 has a conversion to the parameter type, then the compiler uses as argument either the literal 1 if `Option Compare Text` is in effect, or the literal 0 if `Optional Compare Binary` is in effect.</span></span>

* <span data-ttu-id="c06e7-271">省略可能なパラメーターに `System.Runtime.CompilerServices.IDispatchConstantAttribute` という属性があり、type が `Object` で、既定値が指定されていない場合、コンパイラは引数 `New System.Runtime.InteropServices.DispatchWrapper(Nothing)` を使用します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-271">If the optional parameter has the attribute `System.Runtime.CompilerServices.IDispatchConstantAttribute`, and it has type `Object`, and it does not specify a default value, then the compiler uses the argument `New System.Runtime.InteropServices.DispatchWrapper(Nothing)`.</span></span>

* <span data-ttu-id="c06e7-272">省略可能なパラメーターに `System.Runtime.CompilerServices.IUnknownConstantAttribute` という属性があり、type が `Object` で、既定値が指定されていない場合、コンパイラは引数 `New System.Runtime.InteropServices.UnknownWrapper(Nothing)` を使用します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-272">If the optional parameter has the attribute `System.Runtime.CompilerServices.IUnknownConstantAttribute`, and it has type `Object`, and it does not specify a default value, then the compiler uses the argument `New System.Runtime.InteropServices.UnknownWrapper(Nothing)`.</span></span>

* <span data-ttu-id="c06e7-273">省略可能なパラメーターの型が 0 @no__t で、既定値が指定されていない場合、コンパイラは引数 `System.Reflection.Missing.Value` を使用します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-273">If the optional parameter has type `Object`, and it does not specify a default value, then the compiler uses the argument `System.Reflection.Missing.Value`.</span></span>

### <a name="conditional-methods"></a><span data-ttu-id="c06e7-274">条件付きメソッド</span><span class="sxs-lookup"><span data-stu-id="c06e7-274">Conditional Methods</span></span>

<span data-ttu-id="c06e7-275">呼び出し式が参照するターゲットメソッドがインターフェイスのメンバーではないサブルーチンであり、メソッドに1つ以上の @no__t 0 属性がある場合、式の評価はそのポイントで定義されている条件付きコンパイル定数に依存しますソースファイル内。</span><span class="sxs-lookup"><span data-stu-id="c06e7-275">If the target method to which an invocation expression refers is a subroutine that is not a member of an interface and if the method has one or more `System.Diagnostics.ConditionalAttribute` attributes, evaluation of the expression depends on the conditional compilation constants defined at that point in the source file.</span></span> <span data-ttu-id="c06e7-276">属性の各インスタンスは、条件付きコンパイル定数に名前を指定する文字列を指定します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-276">Each instance of the attribute specifies a string, which names a conditional compilation constant.</span></span> <span data-ttu-id="c06e7-277">条件付きコンパイル定数は、条件付きコンパイルステートメントに含まれているかのように評価されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-277">Each conditional compilation constant is evaluated as if it were part of a conditional compilation statement.</span></span> <span data-ttu-id="c06e7-278">定数が `True` に評価される場合、式は実行時に正常に評価されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-278">If the constant evaluates to `True`, the expression is evaluated normally at run time.</span></span> <span data-ttu-id="c06e7-279">定数が `False` と評価された場合、式はまったく評価されません。</span><span class="sxs-lookup"><span data-stu-id="c06e7-279">If the constant evaluates to `False`, the expression is not evaluated at all.</span></span>

<span data-ttu-id="c06e7-280">属性を検索すると、オーバーライド可能なメソッドの最も派生した宣言がチェックされます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-280">When looking for the attribute, the most derived declaration of an overridable method is checked.</span></span>

<span data-ttu-id="c06e7-281">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="c06e7-281">__Note.__</span></span> <span data-ttu-id="c06e7-282">属性は関数またはインターフェイスメソッドでは無効であり、どちらの種類のメソッドでも指定されている場合は無視されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-282">The attribute is not valid on functions or interface methods and is ignored if specified on either kind of method.</span></span> <span data-ttu-id="c06e7-283">したがって、条件付きメソッドは、呼び出しステートメントでのみ表示されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-283">Thus, conditional methods will only appear in invocation statements.</span></span>

### <a name="type-argument-inference"></a><span data-ttu-id="c06e7-284">型引数の推論</span><span class="sxs-lookup"><span data-stu-id="c06e7-284">Type Argument Inference</span></span>

<span data-ttu-id="c06e7-285">型引数を指定せずに型パラメーターを持つメソッドが呼び出された場合、型引数の*推定*を使用して、呼び出しの型引数を試行して推論します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-285">When a method with type parameters is called without specifying type arguments, *type argument inference* is used to try and infer type arguments for the call.</span></span> <span data-ttu-id="c06e7-286">これにより、型引数を普通に推論できる場合に、型パラメーターを使用してメソッドを呼び出すために、より自然な構文を使用できます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-286">This allows a more natural syntax to be used for calling a method with type parameters when the type arguments can be trivially inferred.</span></span> <span data-ttu-id="c06e7-287">たとえば、次のメソッド宣言があるとします。</span><span class="sxs-lookup"><span data-stu-id="c06e7-287">For example, given the following method declaration:</span></span>

```vb
Module Util
    Function Choose(Of T)(b As Boolean, first As T, second As T) As T
        If b Then
            Return first
        Else
            Return second
        End If
    End Function
End Class
```

<span data-ttu-id="c06e7-288">型引数を明示的に指定しなくても、`Choose` メソッドを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-288">it is possible to invoke the `Choose` method without explicitly specifying a type argument:</span></span>

```vb
' calls Choose(Of Integer)
Dim i As Integer = Util.Choose(True, 5, 213)
' calls Choose(Of String)
Dim s As String = Util.Choose(False, "a", "b") 
```

<span data-ttu-id="c06e7-289">型引数の推定により、型引数 `Integer` および `String` は、メソッドの引数から決定されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-289">Through type argument inference, the type arguments `Integer` and `String` are determined from the arguments to the method.</span></span>

<span data-ttu-id="c06e7-290">型引数の推定は、ラムダメソッドまたは引数リスト内のメソッドポインターに対して式の再分類が実行される*前に*発生します。これら2種類の式を再分類するには、パラメーターの型がわかっている必要があるためです。</span><span class="sxs-lookup"><span data-stu-id="c06e7-290">Type argument inference occurs *before* expression reclassification is performed on lambda methods or method pointers in the argument list, since reclassification of those two kinds of expressions may require the type of the parameter to be known.</span></span>  <span data-ttu-id="c06e7-291">一連の引数 `A1,...,An`、一致するパラメーターのセット `P1,...,Pn`、メソッドの型 @no__t パラメーターのセット (-2) を指定すると、引数とメソッドの型パラメーターの間の依存関係は、次のように最初に収集されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-291">Given a set of arguments `A1,...,An`, a set of matching parameters `P1,...,Pn` and a set of method type parameters `T1,...,Tn`, the dependencies between the arguments and method type parameters are first collected as follows:</span></span>

* <span data-ttu-id="c06e7-292">@No__t-0 が `Nothing` のリテラルである場合、依存関係は生成されません。</span><span class="sxs-lookup"><span data-stu-id="c06e7-292">If `An` is the `Nothing` literal, no dependencies are generated.</span></span>

* <span data-ttu-id="c06e7-293">@No__t-0 がラムダメソッドで、@no__t の型が構築されたデリゲート型または `System.Linq.Expressions.Expression(Of T)` の場合、`T` は構築されたデリゲート型です。</span><span class="sxs-lookup"><span data-stu-id="c06e7-293">If `An` is a lambda method and the type of `Pn` is a constructed delegate type or `System.Linq.Expressions.Expression(Of T)`, where `T` is a constructed delegate type,</span></span>

* <span data-ttu-id="c06e7-294">ラムダメソッドのパラメーターの型が対応する @no__t パラメーターの型から推論され、パラメーターの型がメソッド型パラメーター `Tn` に依存している場合は、`An` が `Tn` に依存しています。</span><span class="sxs-lookup"><span data-stu-id="c06e7-294">If the type of a lambda method parameter will be inferred from the type of the corresponding parameter `Pn`, and the type of the parameter depends on a method type parameter `Tn`, then `An` has a dependency on `Tn`.</span></span>

* <span data-ttu-id="c06e7-295">ラムダメソッドのパラメーターの型が指定されていて、対応するパラメーターの型 `Pn` がメソッドの型パラメーター `Tn` の場合、`Tn` は `An` に依存しています。</span><span class="sxs-lookup"><span data-stu-id="c06e7-295">If the type of a lambda method parameter is specified and the type of the corresponding parameter `Pn` depends on a method type parameter `Tn`, then `Tn` has a dependency on `An`.</span></span>

* <span data-ttu-id="c06e7-296">@No__t-0 の戻り値の型が、メソッドの型パラメーター `Tn` に依存している場合、`Tn` は `An` に依存しています。</span><span class="sxs-lookup"><span data-stu-id="c06e7-296">If the return type of `Pn` depends on a method type parameter `Tn`, then `Tn` has a dependency on `An`.</span></span>

* <span data-ttu-id="c06e7-297">@No__t-0 がメソッドポインターで、@no__t の型が構築されたデリゲート型である場合は、</span><span class="sxs-lookup"><span data-stu-id="c06e7-297">If `An` is a method pointer and the type of `Pn` is a constructed delegate type,</span></span>

* <span data-ttu-id="c06e7-298">@No__t-0 の戻り値の型が、メソッドの型パラメーター `Tn` に依存している場合、`Tn` は `An` に依存しています。</span><span class="sxs-lookup"><span data-stu-id="c06e7-298">If the return type of `Pn` depends on a method type parameter `Tn`, then `Tn` has a dependency on `An`.</span></span>

* <span data-ttu-id="c06e7-299">@No__t-0 が構築された型で、@no__t の型がメソッドの型パラメーター `Tn` に依存している場合、`Tn` は @no__t に依存しています。</span><span class="sxs-lookup"><span data-stu-id="c06e7-299">If `Pn` is a constructed type and the type of `Pn` depends on a method type parameter `Tn`, then `Tn` has a dependency on `An`.</span></span>

* <span data-ttu-id="c06e7-300">それ以外の場合、依存関係は生成されません。</span><span class="sxs-lookup"><span data-stu-id="c06e7-300">Otherwise, no dependency is generated.</span></span>

<span data-ttu-id="c06e7-301">依存関係を収集した後、依存関係のない引数はすべて削除されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-301">After collecting dependencies, any arguments that have no dependencies are eliminated.</span></span> <span data-ttu-id="c06e7-302">メソッド型パラメーターに送信依存関係がない場合 (つまり、メソッド型パラメーターが引数に依存していない場合)、型の推定は失敗します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-302">If any method type parameters have no outgoing dependencies (i.e. the method type parameter does not depend on an argument), then type inference fails.</span></span> <span data-ttu-id="c06e7-303">それ以外の場合、残りの引数とメソッドの型パラメーターは、厳密に接続されたコンポーネントにグループ化されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-303">Otherwise, the remaining arguments and method type parameters are grouped into strongly connected components.</span></span> <span data-ttu-id="c06e7-304">厳密に接続されたコンポーネントは、引数とメソッド型パラメーターのセットであり、コンポーネント内の要素は、他の要素の依存関係を介して到達できます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-304">A strongly connected component is a set of arguments and method type parameters, where any element in the component is reachable via dependencies on other elements.</span></span>

<span data-ttu-id="c06e7-305">その後、厳密に接続されたコンポーネントは、位相的に並べ替えられ、次の順序で処理されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-305">The strongly connected components are then topologically sorted and processed in topological order:</span></span>

* <span data-ttu-id="c06e7-306">厳密に型指定されたコンポーネントに含まれる要素が1つだけの場合、</span><span class="sxs-lookup"><span data-stu-id="c06e7-306">If the strongly typed component contains only one element,</span></span>

  * <span data-ttu-id="c06e7-307">要素が既に完了としてマークされている場合は、スキップします。</span><span class="sxs-lookup"><span data-stu-id="c06e7-307">If the element has already been marked complete, skip it.</span></span>

  * <span data-ttu-id="c06e7-308">要素が引数の場合は、引数の型ヒントを、それに依存するメソッド型パラメーターに追加し、要素を完了としてマークします。</span><span class="sxs-lookup"><span data-stu-id="c06e7-308">If the element is an argument, then add type hints from the argument to the method type parameters that depend on it and mark the element as complete.</span></span> <span data-ttu-id="c06e7-309">引数が、推論された型を必要とするパラメーターを持つラムダメソッドである場合は、そのパラメーターの型に対して `Object` を推論します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-309">If the argument is a lambda method with parameters that still need inferred types, then infer `Object` for the types of those parameters.</span></span>

  * <span data-ttu-id="c06e7-310">要素がメソッド型パラメーターの場合、引数の型ヒントの中で最も優先度の高い型になるようにメソッド型パラメーターを推論し、要素を完了としてマークします。</span><span class="sxs-lookup"><span data-stu-id="c06e7-310">If the element is a method type parameter, then infer the method type parameter to be the dominant type among the argument type hints and mark the element as complete.</span></span> <span data-ttu-id="c06e7-311">型ヒントに配列要素の制限がある場合、指定された型の配列間で有効な変換だけが考慮されます (つまり、共変および組み込み配列の変換)。</span><span class="sxs-lookup"><span data-stu-id="c06e7-311">If a type hint has an array element restriction on it, then only conversions that are valid between arrays of the given type are considered (i.e. covariant and intrinsic array conversions).</span></span> <span data-ttu-id="c06e7-312">型ヒントにジェネリック引数の制限がある場合、id 変換だけが考慮されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-312">If a type hint has a generic argument restriction on it, then only identity conversions are considered.</span></span> <span data-ttu-id="c06e7-313">優先される型を選択できない場合、推定は失敗します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-313">If no dominant type can be chosen, inference fails.</span></span> <span data-ttu-id="c06e7-314">ラムダメソッドの引数の型がこのメソッドの型パラメーターに依存している場合、型はラムダメソッドに反映されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-314">If any lambda method argument types depend on this method type parameter, the type is propagated to the lambda method.</span></span>

* <span data-ttu-id="c06e7-315">厳密に型指定されたコンポーネントに複数の要素が含まれている場合、コンポーネントには循環参照が含まれます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-315">If the strongly typed component contains more than one element, then the component contains a cycle.</span></span>

  * <span data-ttu-id="c06e7-316">コンポーネント内の要素であるメソッド型パラメーターごとに、メソッドの型パラメーターが、完了とマークされていない引数に依存している場合は、その依存関係を、推論プロセスの終了時にチェックされるアサーションに変換します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-316">For each method type parameter that is an element in the component, if the method type parameter depends on an argument that is not marked complete, convert that dependency into an assertion that will be checked at the end of the inference process.</span></span>

  * <span data-ttu-id="c06e7-317">厳密に型指定されたコンポーネントが決定された時点で、推論プロセスを再起動します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-317">Restart the inference process at the point at which the strongly typed components were determined.</span></span>

<span data-ttu-id="c06e7-318">すべてのメソッドの型パラメーターに対して型の推定が成功すると、アサーションに変更された依存関係がすべてチェックされます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-318">If type inference succeeds for all of the method type parameters, then any dependencies that were changed into assertions are checked.</span></span> <span data-ttu-id="c06e7-319">引数の型がメソッド型パラメーターの推論された型に暗黙的に変換できる場合、アサーションは成功します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-319">An assertion succeeds if the type of the argument is implicitly convertible to the inferred type of the method type parameter.</span></span> <span data-ttu-id="c06e7-320">アサーションが失敗した場合、型引数の推定は失敗します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-320">If an assertion fails, then type argument inference fails.</span></span>

<span data-ttu-id="c06e7-321">引数の型が-1 の引数 `A` で、パラメーターの型 `Tp` の場合、パラメーター `P` の @no__t、型ヒントは次のように生成されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-321">Given an argument type `Ta` for an argument `A` and a parameter type `Tp` for a parameter `P`, type hints are generated as follows:</span></span>

* <span data-ttu-id="c06e7-322">@No__t-0 にメソッドの型パラメーターが含まれていない場合、ヒントは生成されません。</span><span class="sxs-lookup"><span data-stu-id="c06e7-322">If `Tp` does not involve any method type parameters then no hints are generated.</span></span>

* <span data-ttu-id="c06e7-323">@No__t-0 および `Ta` が同じランクの配列型である場合は、`Ta` と `Tp` を `Ta` および `Tp` の要素の型に置き換え、配列要素の制限でこのプロセスを再起動します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-323">If `Tp` and `Ta` are array types of the same rank, then replace `Ta` and `Tp` with the element types of `Ta` and `Tp` and restart this process with an array element restriction.</span></span>

* <span data-ttu-id="c06e7-324">@No__t-0 がメソッド型パラメーターの場合、`Ta` は、現在の制限を持つ型ヒントとして追加されます (存在する場合)。</span><span class="sxs-lookup"><span data-stu-id="c06e7-324">If `Tp` is a method type parameter, then `Ta` is added as a type hint with the current restriction, if any.</span></span>

* <span data-ttu-id="c06e7-325">@No__t-0 がラムダメソッドで、`Tp` が構築されたデリゲート型または `System.Linq.Expressions.Expression(Of T)` である場合 (`T` は構築されたデリゲート型)、各ラムダメソッドのパラメーターの型である-4 および対応するデリゲートパラメーターの型 `TD` は、`Ta` を @no__t に置き換えます。`TD` を使用して `Tp` を実行し、制限なくプロセスを再起動します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-325">If `A` is a lambda method and `Tp` is a constructed delegate type or `System.Linq.Expressions.Expression(Of T)`, where `T` is a constructed delegate type, for each lambda method parameter type `TL` and corresponding delegate parameter type `TD`, replace `Ta` with `TL` and `Tp` with `TD` and restart the process with no restriction.</span></span> <span data-ttu-id="c06e7-326">次に、`Ta` を、ラムダメソッドの戻り値の型とに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-326">Then, replace `Ta` with the return type of the lambda method and:</span></span>

  * <span data-ttu-id="c06e7-327">`A` が通常のラムダメソッドの場合は、`Tp` をデリゲート型の戻り値の型に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-327">if `A` is a regular lambda method, replace `Tp` with the return type of the delegate type;</span></span>
  * <span data-ttu-id="c06e7-328">`A` が非同期ラムダメソッドであり、デリゲート型の戻り値の型が `T` の `Task(Of T)` の場合は、`Tp` をその @no__t に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-328">if `A` is an async lambda method and the return type of the delegate type has form `Task(Of T)` for some `T`, replace `Tp` with that `T`;</span></span>
  * <span data-ttu-id="c06e7-329">`A` が反復子ラムダメソッドで @no__t あり、デリゲート型の戻り値の型の形式が `IEnumerator(Of T)` または `IEnumerable(Of T)` である場合は、@no__t を `T` に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-329">if `A` is an iterator lambda method and the return type of the delegate type has form `IEnumerator(Of T)` or `IEnumerable(Of T)` for some `T`, replace `Tp` with that `T`.</span></span>
  * <span data-ttu-id="c06e7-330">次に、制限なしでプロセスを再起動します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-330">Next, restart the process with no restriction.</span></span>

* <span data-ttu-id="c06e7-331">@No__t-0 がメソッドポインターで、`Tp` が構築されたデリゲート型である場合は、`Tp` のパラメーターの型を使用して、どのメソッドが `Tp` に最も適しているかを判断します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-331">If `A` is a method pointer and `Tp` is a constructed delegate type, use the parameter types of `Tp` to determine which method pointed is most applicable to `Tp`.</span></span> <span data-ttu-id="c06e7-332">最も適切なメソッドがある場合は、`Ta` をメソッドの戻り値の型に置き換え、`Tp` をデリゲート型の戻り値の型に置き換えて、制限なしでプロセスを再開します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-332">If there is a method that is most applicable, replace `Ta` with the return type of the method and `Tp` with the return type of the delegate type and restart the process with no restriction.</span></span>

* <span data-ttu-id="c06e7-333">それ以外の場合、`Tp` は構築された型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="c06e7-333">Otherwise, `Tp` must be a constructed type.</span></span> <span data-ttu-id="c06e7-334">@No__t-0、@no__t のジェネリック型-1、</span><span class="sxs-lookup"><span data-stu-id="c06e7-334">Given `TG`, the generic type of `Tp`,</span></span>

  * <span data-ttu-id="c06e7-335">@No__t-0 が `TG` の場合、@no__t から継承された場合、または @no__t 型を1回だけ実装している場合は、対応する型引数ごとに `Ta` と `Tpx` の @no__t を @no__t に置き換えてから @no__t を再起動します。汎用引数の制限で処理します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-335">If `Ta` is `TG`, inherits from `TG`, or implements the type `TG` exactly once, then for each matching type argument `Tax` from `Ta` and `Tpx` from `Tp`, replace `Ta` with `Tax` and `Tp` with `Tpx` and restart the process with a generic argument restriction.</span></span>

  * <span data-ttu-id="c06e7-336">それ以外の場合、ジェネリックメソッドの型の推定は失敗します。</span><span class="sxs-lookup"><span data-stu-id="c06e7-336">Otherwise, type inference fails for the generic method.</span></span>

<span data-ttu-id="c06e7-337">型の推定が成功しても、とでは、メソッドが適用可能であることが保証されます。</span><span class="sxs-lookup"><span data-stu-id="c06e7-337">The success of type inference does not, in and of itself, guarantee that the method is applicable.</span></span>
