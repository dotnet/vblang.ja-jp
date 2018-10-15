### <a name="overloaded-method-resolution"></a><span data-ttu-id="ea348-101">オーバー ロードされたメソッドの解決</span><span class="sxs-lookup"><span data-stu-id="ea348-101">Overloaded Method Resolution</span></span>

<span data-ttu-id="ea348-102">実際には、オーバー ロードの解決を決定するルールは、オーバー ロードは実際に指定された引数に「最も近い」を検索する対象としています。</span><span class="sxs-lookup"><span data-stu-id="ea348-102">In practice, the rules for determining overload resolution are intended to find the overload that is "closest" to the actual arguments supplied.</span></span> <span data-ttu-id="ea348-103">パラメーター型、引数の型が一致メソッドの場合、そのメソッドが明らかに最も近いです。</span><span class="sxs-lookup"><span data-stu-id="ea348-103">If there is a method whose parameter types match the argument types, then that method is obviously the closest.</span></span> <span data-ttu-id="ea348-104">1 つのメソッドはより狭い幅はすべて、パラメーターの型の場合、別のよりも近い、なし機能 (またはと同じ) 他のメソッドのパラメーターの型。</span><span class="sxs-lookup"><span data-stu-id="ea348-104">Barring that, one method is closer than another if all of its parameter types are narrower than (or the same as) the parameter types of the other method.</span></span> <span data-ttu-id="ea348-105">どちらのメソッドのパラメーターが他よりも狭い場合は、しがないの引数に近い方法を決定します。</span><span class="sxs-lookup"><span data-stu-id="ea348-105">If neither method's parameters are narrower than the other, then there is no way for to determine which method is closer to the arguments.</span></span>

<span data-ttu-id="ea348-106">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="ea348-106">__Note.__</span></span> <span data-ttu-id="ea348-107">オーバー ロードの解決は考慮されませんメソッドの戻り値の型。</span><span class="sxs-lookup"><span data-stu-id="ea348-107">Overload resolution does not take into account the expected return type of the method.</span></span> 

<span data-ttu-id="ea348-108">また、名前付きパラメーターの構文が実際と仮パラメーターの順序付けできない可能性があります、同じに注意してください。</span><span class="sxs-lookup"><span data-stu-id="ea348-108">Also note that because of the named parameter syntax, the ordering of the actual and formal parameters may not be the same.</span></span>

<span data-ttu-id="ea348-109">メソッド グループを指定するには、次の手順を使用して、引数リストのグループに最適なメソッドが決まります。</span><span class="sxs-lookup"><span data-stu-id="ea348-109">Given a method group, the most applicable method in the group for an argument list is determined using the following steps.</span></span> <span data-ttu-id="ea348-110">、特定のステップを適用すると、メンバーが残っていない場合、セット内では、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ea348-110">If, after applying a particular step, no members remain in the set, then a compile-time error occurs.</span></span> <span data-ttu-id="ea348-111">1 つのメンバーは、セットに残っている場合のみそのメンバーは、最も適切なメンバーです。</span><span class="sxs-lookup"><span data-stu-id="ea348-111">If only one member remains in the set, then that member is the most applicable member.</span></span> <span data-ttu-id="ea348-112">手順は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="ea348-112">The steps are:</span></span>

1.  <span data-ttu-id="ea348-113">最初に、型引数が指定されていない場合は、型パラメーターを持つメソッドが存在する型の推定を適用します。</span><span class="sxs-lookup"><span data-stu-id="ea348-113">First, if no type arguments have been supplied, apply type inference to any methods which have type parameters.</span></span> <span data-ttu-id="ea348-114">メソッドの型の推論に成功した場合、推論された型引数は、特定のメソッドの使用されます。</span><span class="sxs-lookup"><span data-stu-id="ea348-114">If type inference succeeds for a method, then the inferred type arguments are used for that particular method.</span></span> <span data-ttu-id="ea348-115">メソッドの型の推定が失敗した場合、そのメソッドは、セットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="ea348-115">If type inference fails for a method, then that method is eliminated from the set.</span></span>

2.  <span data-ttu-id="ea348-116">次に、アクセスできないか該当なしであるセットからすべてのメンバーを排除 (セクション[引数リストへの適用性](overload-resolution.md#applicability-to-argument-list)) 引数リストへ</span><span class="sxs-lookup"><span data-stu-id="ea348-116">Next, eliminate all members from the set that are inaccessible or not applicable (Section [Applicability To Argument List](overload-resolution.md#applicability-to-argument-list)) to the argument list</span></span>

3.  <span data-ttu-id="ea348-117">次に、1 つまたは複数の引数がある場合`AddressOf`、ラムダ式を計算し、または、*果てるレベルの委任*次に示すように各このような引数。</span><span class="sxs-lookup"><span data-stu-id="ea348-117">Next, if one or more arguments are `AddressOf` or lambda expressions, then calculate the *delegate relaxation levels* for each such argument as below.</span></span> <span data-ttu-id="ea348-118">最悪の事態 (最低) の委任でくつろぎレベル場合`N`がの最下位のデリゲートを緩和したトークン レベルよりも悪い`M`から除外`N`セットから。</span><span class="sxs-lookup"><span data-stu-id="ea348-118">If the worst (lowest) delegate relaxation level in `N` is worse than the lowest delegate relaxation level in `M`, then eliminate `N` from the set.</span></span> <span data-ttu-id="ea348-119">デリゲートを緩和したトークンのレベルは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="ea348-119">The delegate relaxation levels are as follows:</span></span>

    31. <span data-ttu-id="ea348-120">*デリゲートを緩和したトークンのエラー レベル*場合--、`AddressOf`またはラムダをデリゲート型に変換できません。</span><span class="sxs-lookup"><span data-stu-id="ea348-120">*Error delegate relaxation level* -- if the `AddressOf` or lambda cannot be converted to the delegate type.</span></span>
  
    32. <span data-ttu-id="ea348-121">*デリゲートの戻り値の型またはパラメーターを緩和したトークンを縮小*引数の場合--`AddressOf`またはラムダ宣言された型と戻り値の型からデリゲートへの変換の戻り型が縮小します引数が正規のラムダの場合、または、。型の縮小または引数が非同期のラムダとデリゲートの戻り型は return 式のいずれかからデリゲートへの変換が返す`Task(Of T)`とその戻り値の式のいずれかから変換`T`は縮小; 場合引数が、反復子ラムダでは、デリゲートの戻り値の型と`IEnumerator(Of T)`または`IEnumerable(Of T)`に yield オペランドのいずれかからの変換と`T`は縮小します。</span><span class="sxs-lookup"><span data-stu-id="ea348-121">*Narrowing delegate relaxation of return type or parameters* -- if the argument is `AddressOf` or a lambda with a declared type and the conversion from its return type to the delegate return type is narrowing; or if the argument is a regular lambda and the conversion from any of its return expressions to the delegate return type is narrowing, or if the argument is an async lambda and the delegate return type is `Task(Of T)` and the conversion from any of its return expressions to `T` is narrowing; or if the argument is an iterator lambda and the delegate return type `IEnumerator(Of T)` or `IEnumerable(Of T)` and the conversion from any of its yield operands to `T` is narrowing.</span></span>

    33. <span data-ttu-id="ea348-122">*デリゲートを緩和したトークンに署名のない委任を拡大*デリゲート型の場合--`System.Delegate`または`System.MultiCastDelegate`または`System.Object`します。</span><span class="sxs-lookup"><span data-stu-id="ea348-122">*Widening delegate relaxation to delegate without signature* -- if delegate type is `System.Delegate` or `System.MultiCastDelegate` or `System.Object`.</span></span>

    34. <span data-ttu-id="ea348-123">*戻り値または引数をデリゲート緩和したトークンをドロップ*引数の場合--`AddressOf`ラムダ宣言された戻り値の型とデリゲート型と戻り値の型がないか、または引数が 1 つのラムダまたは式とデリゲート型を返す詳細戻り値の型が不足しています引数の場合、または`AddressOf`ラムダ パラメーターとデリゲート型を持つパラメーターまたはします。</span><span class="sxs-lookup"><span data-stu-id="ea348-123">*Drop return or arguments delegate relaxation* -- if the argument is `AddressOf` or a lambda with a declared return type and the delegate type lacks a return type; or if the argument is a lambda with one or more return expressions and the delegate type lacks a return type; or if the argument is `AddressOf` or lambda with no parameters and the delegate type has parameters.</span></span>

    35. <span data-ttu-id="ea348-124">*戻り値の型のデリゲートを緩和したトークンを拡大*引数の場合--`AddressOf`または宣言された戻り値の型のラムダは、デリゲートの戻り値の型から拡大変換と引数が正規のラムダの場合、または場所、すべての return 式からデリゲートの戻り値の型への変換は拡大変換または id を少なくとも 1 つの拡大します。引数が非同期のラムダでは、デリゲートが場合や`Task(Of T)`または`Task`とするすべての return 式から変換`T` / `Object`拡大変換または id を少なくとも 1 つの拡大; がそれぞれ場合引数が、反復子ラムダでは、デリゲートが`IEnumerator(Of T)`または`IEnumerable(Of T)`または`IEnumerator`または`IEnumerable`とするすべての return 式から変換`T` / `Object`は拡大または少なくとも 1 つの拡大を持つ id。</span><span class="sxs-lookup"><span data-stu-id="ea348-124">*Widening delegate relaxation of return type* -- if the argument is `AddressOf` or a lambda with a declared return type, and there is a widening conversion from its return type to that of the delegate; or if the argument is a regular lambda where the conversion from all return expressions to the delegate return type is widening or identity with at least one widening; or if the argument is an async lambda and the delegate is `Task(Of T)` or `Task` and the conversion from all return expressions to `T`/`Object` respectively is widening or identity with at least one widening; or if the argument is an iterator lambda and the delegate is `IEnumerator(Of T)` or `IEnumerable(Of T)` or `IEnumerator` or `IEnumerable` and the conversion from all return expressions to `T`/`Object` is widening or identity with at least one widening.</span></span>

    36. <span data-ttu-id="ea348-125">*デリゲート緩和の identity*引数の場合--、`AddressOf`なしで、デリゲートを正確に一致するラムダ式の拡大または縮小、またはパラメーターを返します。 または結果を削除していますか。次に、セットの一部のメンバーが、引数のいずれかに適用できる縮小変換を必要としない場合を実行するすべてのメンバーを除外します。</span><span class="sxs-lookup"><span data-stu-id="ea348-125">*Identity delegate relaxation* -- if the argument is an `AddressOf` or a lambda which matches the delegate exactly, with no widening or narrowing or dropping of parameters or returns or yields.Next, if some members of the set do not requiring narrowing conversions to be applicable to any of the arguments, then eliminate all members that do.</span></span> <span data-ttu-id="ea348-126">例えば:</span><span class="sxs-lookup"><span data-stu-id="ea348-126">For example:</span></span>

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

4.  <span data-ttu-id="ea348-127">次に、削除は、次のように縮小に基づいて行われます。</span><span class="sxs-lookup"><span data-stu-id="ea348-127">Next, elimination is done based on narrowing as follows.</span></span> <span data-ttu-id="ea348-128">(、Option Strict がオンの場合、縮小変換を必要とするすべてのメンバーが既に判定できないに注意してください (セクション[引数リストへの適用性](overload-resolution.md#applicability-to-argument-list))、手順 2. で削除します)。</span><span class="sxs-lookup"><span data-stu-id="ea348-128">(Note that, if Option Strict is On, then all members that require narrowing have already been judged inapplicable (Section [Applicability To Argument List](overload-resolution.md#applicability-to-argument-list)) and removed by Step 2.)</span></span>

    41. <span data-ttu-id="ea348-129">場合は、セットの一部のインスタンス メンバーが縮小変換は、引数式の型が必要なだけ`Object`、その他のすべてのメンバーを削除します。</span><span class="sxs-lookup"><span data-stu-id="ea348-129">If some instance members of the set only require narrowing conversions where the argument expression type is `Object`, then eliminate all other members.</span></span>
    42. <span data-ttu-id="ea348-130">のみに絞り込む必要があります 1 つ以上のメンバーがセットに含まれる場合`Object`、呼び出し対象の式が遅延バインディング メソッド アクセスとして再分類し (され、エラーは、メソッド グループを含む型がインターフェイス、またはのいずれか。該当するメンバーが拡張メンバー)。</span><span class="sxs-lookup"><span data-stu-id="ea348-130">If the set contains more than one member which requires narrowing only from `Object`, then the invocation target expression is reclassified as a late-bound method access (and an error is given if the type containing the method group is an interface, or if any of the applicable members were extension members).</span></span>
    43. <span data-ttu-id="ea348-131">のみ数値リテラルから縮小変換を必要とするすべての候補がある場合は、選択し残りのすべての応募者の間で最も具体的で次の手順。</span><span class="sxs-lookup"><span data-stu-id="ea348-131">If there are any candidates that only require narrowing from numeric literals, then chose the most specific among all remaining candidates by the steps below.</span></span> <span data-ttu-id="ea348-132">優勝者は、数値リテラルからのみに絞り込む必要がある場合に、選択したオーバー ロードの解決の結果としてそれ以外の場合、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="ea348-132">If the winner requires only narrowing from numeric literals, then it is picked as the result of overload resolution; otherwise it is an error.</span></span>

    <span data-ttu-id="ea348-133">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="ea348-133">__Note.__</span></span> <span data-ttu-id="ea348-134">この規則の妥当性は、プログラムが疎に型指定された場合 (としてのほとんどまたはすべての変数の宣言は、 `Object`)、オーバー ロードの解決は難しい場合に多くの変換から`Object`は縮小変換します。</span><span class="sxs-lookup"><span data-stu-id="ea348-134">The justification for this rule is that if a program is loosely-typed (that is, most or all variables are declared as `Object`), overload resolution can be difficult when many conversions from `Object` are narrowing.</span></span> <span data-ttu-id="ea348-135">(メソッド呼び出しの引数の厳密な型指定が必要)、多くの状況で、適切なオーバー ロードの解決が失敗するオーバー ロードの解決にはなくに呼び出すメソッドが実行時まで延期します。</span><span class="sxs-lookup"><span data-stu-id="ea348-135">Rather than have the overload resolution fail in many situations (requiring strong typing of the arguments to the method call), resolution the appropriate overloaded method to call is deferred until run time.</span></span> <span data-ttu-id="ea348-136">これにより、キャストなしで成功を柔軟に型指定された呼び出しができます。</span><span class="sxs-lookup"><span data-stu-id="ea348-136">This allows the loosely-typed call to succeed without additional casts.</span></span> <span data-ttu-id="ea348-137">影響を及ぼしますが、ただしはその実行の遅延バインディングによる呼び出しでは、呼び出しターゲットへのキャストが必要です`Object`します。</span><span class="sxs-lookup"><span data-stu-id="ea348-137">An unfortunate side-effect of this, however, is that performing the late-bound call requires casting the call target to `Object`.</span></span> <span data-ttu-id="ea348-138">場合は、構造体の値を一時的に値をボックス化する必要があります、つまりです。</span><span class="sxs-lookup"><span data-stu-id="ea348-138">In the case of a structure value, this means that the value must be boxed to a temporary.</span></span> <span data-ttu-id="ea348-139">最終的に呼び出されるメソッドが、構造体のフィールドを変更しようとすると場合、は、メソッドから戻ると変更が失われます。</span><span class="sxs-lookup"><span data-stu-id="ea348-139">If the method eventually called tries to change a field of the structure, this change will be lost once the method returns.</span></span> <span data-ttu-id="ea348-140">インターフェイスは、常に遅延バインディングに対して、型のメンバー、ランタイム クラスまたは構造体、異なる名前を実装するインターフェイスのメンバーがあります。 これにより解決されるため、この特別な規則から除外されます。</span><span class="sxs-lookup"><span data-stu-id="ea348-140">Interfaces are excluded from this special rule because late binding always resolves against the members of the runtime class or structure type, which may have different names than the members of the interfaces they implement.</span></span>

5.  <span data-ttu-id="ea348-141">次に、インスタンス メソッドは、縮小変換を必要としないセットに残っている場合はその、セットからすべての拡張メソッドを削除します。</span><span class="sxs-lookup"><span data-stu-id="ea348-141">Next, if any instance methods remain in the set which do not require narrowing, then eliminate all extension methods from the set.</span></span> <span data-ttu-id="ea348-142">例えば:</span><span class="sxs-lookup"><span data-stu-id="ea348-142">For example:</span></span>

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

    <span data-ttu-id="ea348-143">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="ea348-143">__Note.__</span></span> <span data-ttu-id="ea348-144">拡張メソッドには、こと (スコープに新しい拡張メソッドを含めることがあります) をインポートの追加は発生しませんの呼び出しで拡張メソッドを再バインドする既存のインスタンス メソッドを保証するために該当するインスタンス メソッドがある場合は無視されます。</span><span class="sxs-lookup"><span data-stu-id="ea348-144">Extension methods are ignored if there are applicable instance methods to guarantee that adding an import (that might bring new extension methods into scope) will not cause a call on an existing instance method to rebind to an extension method.</span></span> <span data-ttu-id="ea348-145">拡張メソッドにバインドするより安全なアプローチになります (インターフェイスまたは型パラメーターで定義されているものなど) 一部の拡張メソッドのさまざまなスコープを指定します。</span><span class="sxs-lookup"><span data-stu-id="ea348-145">Given the broad scope of some extension methods (i.e. those defined on interfaces and/or type parameters), this is a safer approach to binding to extension methods.</span></span>

6.  <span data-ttu-id="ea348-146">次に、if、2 つのメンバー、セットの`M`と`N`、`M`は*特定*(セクション[メンバー/種類を引数リストの特定の特異性](overload-resolution.md#specificity-of-memberstypes-given-an-argument-list))より`N`引数リストを指定するには、排除`N`セットから。</span><span class="sxs-lookup"><span data-stu-id="ea348-146">Next, if, given any two members of the set `M` and `N`, `M` is more *specific* (Section [Specificity of members/types given an argument list](overload-resolution.md#specificity-of-memberstypes-given-an-argument-list)) than `N` given the argument list, eliminate `N` from the set.</span></span> <span data-ttu-id="ea348-147">1 つ以上のメンバーがセットには残りのメンバーはなく、同じように引数リストを指定、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ea348-147">If more than one member remains in the set and the remaining members are not equally specific given the argument list, a compile-time error results.</span></span>

7.  <span data-ttu-id="ea348-148">それ以外の場合、2 つのメンバー、セットの指定された`M`と`N`順序で、次の対フスェケェルェニェヒ ルールを適用します。</span><span class="sxs-lookup"><span data-stu-id="ea348-148">Otherwise, given any two members of the set, `M` and `N`, apply the following tie-breaking rules, in order:</span></span>

    71. <span data-ttu-id="ea348-149">場合`M`ParamArray パラメーターはありませんが、 `N` 、または両方の操作を行いますが、`M`より ParamArray パラメーターに以下の引数を渡します`N`から除外は、`N`セットから。</span><span class="sxs-lookup"><span data-stu-id="ea348-149">If `M` does not have a ParamArray parameter but `N` does, or if both do but `M` passes fewer arguments into the ParamArray parameter than `N` does, then eliminate `N` from the set.</span></span> <span data-ttu-id="ea348-150">例えば:</span><span class="sxs-lookup"><span data-stu-id="ea348-150">For example:</span></span>

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

        <span data-ttu-id="ea348-151">上記の例では、次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="ea348-151">The above example produces the following output:</span></span>

        ```
        F(Object, Object())
        F(Object, Object, Object())
        F(Object, Object, Object())
        G(Object)
        ```

        <span data-ttu-id="ea348-152">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="ea348-152">__Note.__</span></span> <span data-ttu-id="ea348-153">クラスは、パラメーターを持つメソッドを宣言して、それはも通常のメソッドとして展開されたフォームの一部を含めるには珍しくありません。</span><span class="sxs-lookup"><span data-stu-id="ea348-153">When a class declares a method with a paramarray parameter, it is not uncommon to also include some of the expanded forms as regular methods.</span></span> <span data-ttu-id="ea348-154">により、配列の割り当てを回避することが、paramarray パラメーターを持つメソッドの拡張の形式のときに発生するインスタンスが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="ea348-154">By doing so it is possible to avoid the allocation of an array instance that occurs when an expanded form of a method with a paramarray parameter is invoked.</span></span>

    72. <span data-ttu-id="ea348-155">場合`M`よりも強い派生型で定義されて`N`、排除`N`セットから。</span><span class="sxs-lookup"><span data-stu-id="ea348-155">If `M` is defined in a more derived type than `N`, eliminate `N` from the set.</span></span> <span data-ttu-id="ea348-156">例えば:</span><span class="sxs-lookup"><span data-stu-id="ea348-156">For example:</span></span>

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

        <span data-ttu-id="ea348-157">このルールは、拡張メソッドで定義されている型にも適用されます。</span><span class="sxs-lookup"><span data-stu-id="ea348-157">This rule also applies to the types that extension methods are defined on.</span></span> <span data-ttu-id="ea348-158">例えば:</span><span class="sxs-lookup"><span data-stu-id="ea348-158">For example:</span></span>

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

    73. <span data-ttu-id="ea348-159">場合`M`と`N`拡張メソッドは、対象の型の`M`がクラスまたは構造体でありのターゲット型`N`インターフェイスで排除`N`セットから。</span><span class="sxs-lookup"><span data-stu-id="ea348-159">If `M` and `N` are extension methods and the target type of `M` is a class or structure and the target type of `N` is an interface, eliminate `N` from the set.</span></span> <span data-ttu-id="ea348-160">例えば:</span><span class="sxs-lookup"><span data-stu-id="ea348-160">For example:</span></span>

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

    74. <span data-ttu-id="ea348-161">場合`M`と`N`は拡張メソッドをおよびのターゲット型`M`と`N`が型パラメーターの代用とターゲットの後に同じ`M`パラメーターの置換が含まれていない型の前に型パラメーターのターゲット型`N`は、次のターゲット型よりも少ない型パラメーターを持ち`N`、排除`N`セットから。</span><span class="sxs-lookup"><span data-stu-id="ea348-161">If `M` and `N` are extension methods, and the target type of `M` and `N` are identical after type parameter substitution, and the target type of `M` before type parameter substitution does not contain type parameters but the target type of `N` does, then has fewer type parameters than the target type of `N`, eliminate `N` from the set.</span></span> <span data-ttu-id="ea348-162">例えば:</span><span class="sxs-lookup"><span data-stu-id="ea348-162">For example:</span></span>

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

    75. <span data-ttu-id="ea348-163">型の前に引数が置き換えられた場合、`M`は*ジェネリック以下*(セクション[汎用性](overload-resolution.md#genericity)) よりも`N`、排除`N`セットから。</span><span class="sxs-lookup"><span data-stu-id="ea348-163">Before type arguments have been substituted, if `M` is *less generic* (Section [Genericity](overload-resolution.md#genericity)) than `N`, eliminate `N` from the set.</span></span>

    76. <span data-ttu-id="ea348-164">場合`M`拡張メソッドでないと`N`排除、`N`セットから。</span><span class="sxs-lookup"><span data-stu-id="ea348-164">If `M` is not an extension method and `N` is, eliminate `N` from the set.</span></span>

    77. <span data-ttu-id="ea348-165">場合`M`と`N`拡張メソッドと`M`する前に見つかった`N`(セクション[拡張メソッドのコレクション](overload-resolution.md#extension-method-collection))、排除`N`セットから。</span><span class="sxs-lookup"><span data-stu-id="ea348-165">If `M` and `N` are extension methods and `M` was found before `N` (Section [Extension Method Collection](overload-resolution.md#extension-method-collection)), eliminate `N` from the set.</span></span> <span data-ttu-id="ea348-166">例えば:</span><span class="sxs-lookup"><span data-stu-id="ea348-166">For example:</span></span>

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

        <span data-ttu-id="ea348-167">拡張メソッドは、同じ手順で検出された場合は、それらの拡張メソッドはあいまいです。</span><span class="sxs-lookup"><span data-stu-id="ea348-167">If the extension methods were found in the same step, then those extension methods are ambiguous.</span></span> <span data-ttu-id="ea348-168">呼び出しは、拡張メソッドを格納していると、通常のメンバーの場合と、拡張メソッドを呼び出すことは、標準のモジュールの名前を使用して常に区別可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ea348-168">The call may always be disambiguated using the name of the standard module containing the extension method and calling the extension method as if it was a regular member.</span></span> <span data-ttu-id="ea348-169">例えば:</span><span class="sxs-lookup"><span data-stu-id="ea348-169">For example:</span></span>

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

    78. <span data-ttu-id="ea348-170">場合`M`と`N`両方に必要な型の引数を生成する型の推定と`M`が(つまり各型引数を1つの型を推論)、型引数のいずれかの主要な型を決定するを必要としない`N`排除、`N`セットから。</span><span class="sxs-lookup"><span data-stu-id="ea348-170">If `M` and `N` both required type inference to produce type arguments, and `M` did not require determining the dominant type for any of its type arguments (i.e. each the type arguments inferred to a single type), but `N` did, eliminate `N` from the set.</span></span>

        <span data-ttu-id="ea348-171">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="ea348-171">__Note.__</span></span> <span data-ttu-id="ea348-172">このルールは、そのオーバー ロードの解決 (で複数の型を型引数の推論はエラーが発生する) 以前のバージョンが成功したことを確認の場合、同じ結果に進みます。</span><span class="sxs-lookup"><span data-stu-id="ea348-172">This rule ensures that overload resolution that succeeded in previous versions (where inferring multiple types for a type argument would cause an error), continue to produce the same results.</span></span>

    79. <span data-ttu-id="ea348-173">デリゲート作成式のターゲットを解決するのにはオーバー ロードの解決が実行されている場合、`AddressOf`式と、両方のデリゲートと`M`中に関数`N`サブルーチン、排除`N`セットから。</span><span class="sxs-lookup"><span data-stu-id="ea348-173">If overload resolution is being done to resolve the target of a delegate-creation expression from an `AddressOf` expression, and both the delegate and `M` are functions while `N` is a subroutine, eliminate `N` from the set.</span></span> <span data-ttu-id="ea348-174">同様に場合、両方のデリゲートと`M`サブルーチン、中に`N`関数の場合は、排除`N`セットから。</span><span class="sxs-lookup"><span data-stu-id="ea348-174">Likewise, if both the delegate and `M` are subroutines, while `N` is a function, eliminate `N` from the set.</span></span>

    710. <span data-ttu-id="ea348-175">場合`M`明示的な引数は、代わりにすべて省略可能なパラメーターの既定値を使用していないが、`N`から除外して、`N`セットから。</span><span class="sxs-lookup"><span data-stu-id="ea348-175">If `M` did not use any optional parameter defaults in place of explicit arguments, but `N` did, then eliminate `N` from the set.</span></span>

    711. <span data-ttu-id="ea348-176">型の前に引数が置き換えられた場合、`M`が*汎用性のさらに詳しく*(セクション[汎用性](overload-resolution.md#genericity)) よりも`N`から除外`N`セットから。</span><span class="sxs-lookup"><span data-stu-id="ea348-176">Before type arguments have been substituted, if `M` has *greater depth of genericity* (Section [Genericity](overload-resolution.md#genericity)) than `N`, then eliminate `N` from the set.</span></span>

8. <span data-ttu-id="ea348-177">それ以外の場合、呼び出しがあいまいですし、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ea348-177">Otherwise, the call is ambiguous and a compile-time error occurs.</span></span>

#### <a name="specificity-of-memberstypes-given-an-argument-list"></a><span data-ttu-id="ea348-178">メンバー/種類を引数リストの特定の特異性</span><span class="sxs-lookup"><span data-stu-id="ea348-178">Specificity of members/types given an argument list</span></span>

<span data-ttu-id="ea348-179">メンバー`M`と見なされます*均等に特定*として`N`、引数リストを指定された`A`に各パラメーターを入力する場合、そのシグネチャが同じである場合または`M`対応すると同じですパラメーターの型`N`します。</span><span class="sxs-lookup"><span data-stu-id="ea348-179">A member `M` is considered *equally specific* as `N`, given an argument-list `A`, if their signatures are the same or if each parameter type in `M` is the same as the corresponding parameter type in `N`.</span></span>

<span data-ttu-id="ea348-180">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="ea348-180">__Note.__</span></span> <span data-ttu-id="ea348-181">2 つのメンバーは、拡張メソッドにより、同じシグネチャを持つメソッド グループで終了することができます。</span><span class="sxs-lookup"><span data-stu-id="ea348-181">Two members can end up in a method group with the same signature due to extension methods.</span></span> <span data-ttu-id="ea348-182">2 つのメンバーはできるも同じように特定するが、型パラメーターまたは paramarray 拡張により、同じシグネチャがありません。</span><span class="sxs-lookup"><span data-stu-id="ea348-182">Two members can also be equally specific but not have the same signature due to type parameters or paramarray expansion.</span></span>

<span data-ttu-id="ea348-183">メンバー`M`と見なされます*より具体的*よりも`N`かどうかそのシグネチャは異なるし、少なくとも 1 つのパラメーター入力`M`パラメーターの型よりも特定`N`、および noパラメーターの型`N`パラメーターの型よりも特定`M`します。</span><span class="sxs-lookup"><span data-stu-id="ea348-183">A member `M` is considered *more specific* than `N` if their signatures are different and at least one parameter type in `M` is more specific than a parameter type in `N`, and no parameter type in `N` is more specific than a parameter type in `M`.</span></span> <span data-ttu-id="ea348-184">2 つのパラメーターを指定された`Mj`と`Nj`引数と一致する`Aj`の型`Mj`と見なされます*より具体的*の型よりも`Nj`場合、次のいずれか条件が true:</span><span class="sxs-lookup"><span data-stu-id="ea348-184">Given a pair of parameters `Mj` and `Nj` that matches an argument `Aj`, the type of `Mj` is considered *more specific* than the type of `Nj` if one of the following conditions is true:</span></span>

* <span data-ttu-id="ea348-185">型から拡大変換が存在する`Mj`型に`Nj`します。</span><span class="sxs-lookup"><span data-stu-id="ea348-185">There exists a widening conversion from the type of `Mj` to the type `Nj`.</span></span> <span data-ttu-id="ea348-186">(__に注意してください。__</span><span class="sxs-lookup"><span data-stu-id="ea348-186">(__Note.__</span></span> <span data-ttu-id="ea348-187">パラメーターの型は、ここで、実引数に関係なく比較している、ため、拡大変換定数式から数値型に値が適合しないと見なされますここでします。)</span><span class="sxs-lookup"><span data-stu-id="ea348-187">Because parameters types are being compared without regard to the actual argument in this case, the widening conversion from constant expressions to a numeric type the value fits into is not considered in this case.)</span></span>

* <span data-ttu-id="ea348-188">`Aj` リテラルは、 `0`、`Mj`が数値型と`Nj`は列挙型。</span><span class="sxs-lookup"><span data-stu-id="ea348-188">`Aj` is the literal `0`, `Mj` is a numeric type and `Nj` is an enumerated type.</span></span> <span data-ttu-id="ea348-189">(__に注意してください。__</span><span class="sxs-lookup"><span data-stu-id="ea348-189">(__Note.__</span></span> <span data-ttu-id="ea348-190">このルールは、必要なため、リテラル`0`任意の列挙型に拡大変換されます。</span><span class="sxs-lookup"><span data-stu-id="ea348-190">This rule is necessary because the literal `0` widens to any enumerated type.</span></span> <span data-ttu-id="ea348-191">これにより、列挙型は、基になる型に拡大変換されます、ためにオーバー ロードの解決`0`は、既定よりも優先列挙型の数値型。</span><span class="sxs-lookup"><span data-stu-id="ea348-191">Since an enumerated type widens to its underlying type, this means that overload resolution on `0` will, by default, prefer enumerated types over numeric types.</span></span> <span data-ttu-id="ea348-192">受け取りました多くのフィードバックのこの動作が直感に反することです。)</span><span class="sxs-lookup"><span data-stu-id="ea348-192">We received a lot of feedback that this behavior was counterintuitive.)</span></span>

* <span data-ttu-id="ea348-193">`Mj` `Nj`は、両方の数値型と`Mj`ものよりも前`Nj`の一覧で`Byte`、 `SByte`、 `Short`、 `UShort`、 `Integer`、 `UInteger`、 `Long`、 `ULong`, `Decimal`, `Single`, `Double`.</span><span class="sxs-lookup"><span data-stu-id="ea348-193">`Mj` and `Nj` are both numeric types, and `Mj` comes earlier than `Nj` in the list `Byte`, `SByte`, `Short`, `UShort`, `Integer`, `UInteger`, `Long`, `ULong`, `Decimal`, `Single`, `Double`.</span></span> <span data-ttu-id="ea348-194">(__に注意してください。__</span><span class="sxs-lookup"><span data-stu-id="ea348-194">(__Note.__</span></span> <span data-ttu-id="ea348-195">数値型のルールは、特定のサイズの符号付きと符号なし数値型のみがある縮小変換に間ので便利です。</span><span class="sxs-lookup"><span data-stu-id="ea348-195">The rule about the numeric types is useful because the signed and unsigned numeric types of a particular size only have narrowing conversions between them.</span></span> <span data-ttu-id="ea348-196">上記のルールは、複数の「自然」数値型を優先して 2 つの型間のつながりを中断します。</span><span class="sxs-lookup"><span data-stu-id="ea348-196">The above rule breaks the tie between the two types in favor of the more "natural" numeric type.</span></span> <span data-ttu-id="ea348-197">これは特に重要ですたとえば、両方に適合する数値リテラルの両方、符号付きと符号なし数値の種類に特定のサイズの拡大型でオーバー ロードの解決を行う場合)。</span><span class="sxs-lookup"><span data-stu-id="ea348-197">This is particularly important when doing overload resolution on a type that widens to both the signed and unsigned numeric types of a particular size, for example, a numeric literal that fits into both.)</span></span>

* <span data-ttu-id="ea348-198">`Mj` `Nj`デリゲートを関数の型と戻り値の型は`Mj`の戻り値の型よりも特定`Nj`場合`Aj`メソッドのラムダに分類されますと`Mj`または`Nj`は`System.Linq.Expressions.Expression(Of T)`、し、比較される種類 (デリゲート型でを想定) 型の型引数に置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="ea348-198">`Mj` and `Nj` are delegate function types and the return type of `Mj` is more specific than the return type of `Nj` If `Aj` is classified as a lambda method, and `Mj` or `Nj` is `System.Linq.Expressions.Expression(Of T)`, then the type argument of the type (assuming it is a delegate type) is substituted for the type being compared.</span></span>

* <span data-ttu-id="ea348-199">`Mj` 型と同じ`Aj`、および`Nj`はありません。</span><span class="sxs-lookup"><span data-stu-id="ea348-199">`Mj` is identical to the type of `Aj`, and `Nj` is not.</span></span> <span data-ttu-id="ea348-200">(__に注意してください。__</span><span class="sxs-lookup"><span data-stu-id="ea348-200">(__Note.__</span></span> <span data-ttu-id="ea348-201">興味深いことに注意して以前のルールとは若干異なります (C#) でその c# をデリゲート型の関数は、Visual Basic ではないときに、戻り値の型を比較する前に同一のパラメーター リストを持っている必要があります。)</span><span class="sxs-lookup"><span data-stu-id="ea348-201">It is interesting to note that the previous rule differs slightly from C#, in that C# requires that the delegate function types have identical parameter lists before comparing return types, while Visual Basic does not.)</span></span>

#### <a name="genericity"></a><span data-ttu-id="ea348-202">汎用性</span><span class="sxs-lookup"><span data-stu-id="ea348-202">Genericity</span></span>

<span data-ttu-id="ea348-203">メンバー`M`判断されました*ジェネリック以下*メンバーよりも`N`次のようにします。</span><span class="sxs-lookup"><span data-stu-id="ea348-203">A member `M` is determined to be *less generic* than a member `N` as follows:</span></span>

1. <span data-ttu-id="ea348-204">一致するパラメーターの各ペアの場合、`Mj`と`Nj`、`Mj`未満かより均等にジェネリック`Nj`、メソッド、および少なくとも 1 つの型パラメーターに関して`Mj`は入力に関するあまり汎用メソッドのパラメーターです。</span><span class="sxs-lookup"><span data-stu-id="ea348-204">If, for each pair of matching parameters `Mj` and `Nj`, `Mj` is less or equally generic than `Nj` with respect to type parameters on the method, and at least one `Mj` is less generic with respect to type parameters on the method.</span></span>
2. <span data-ttu-id="ea348-205">一致するパラメーターの各ペアの場合はそれ以外の場合、`Mj`と`Nj`、`Mj`未満かより均等にジェネリック`Nj`、型、および少なくとも 1 つの型パラメーターに関して`Mj`は入力に関するあまり汎用パラメーター入力で、`M`がよりも小さいジェネリック`N`します。</span><span class="sxs-lookup"><span data-stu-id="ea348-205">Otherwise, if for each pair of matching parameters `Mj` and `Nj`, `Mj` is less or equally generic than `Nj` with respect to type parameters on the type, and at least one `Mj` is less generic with respect to type parameters on the type, then `M` is less generic than `N`.</span></span>

<span data-ttu-id="ea348-206">パラメーター`M`パラメーターに均等にジェネリックと見なされます`N`場合、型`Mt`と`Nt`両方参照パラメーターを入力するか、両方ない入力パラメーターを参照してください。</span><span class="sxs-lookup"><span data-stu-id="ea348-206">A parameter `M` is considered to be equally generic to a parameter `N` if their types `Mt` and `Nt` both refer to type parameters or both don't refer to type parameters.</span></span> <span data-ttu-id="ea348-207">`M` も小さいジェネリックと見なされます`N`場合`Mt`型パラメーターを参照していないと`Nt`は。</span><span class="sxs-lookup"><span data-stu-id="ea348-207">`M` is considered to be less generic than `N` if `Mt` does not refer to a type parameter and `Nt` does.</span></span>

<span data-ttu-id="ea348-208">例えば:</span><span class="sxs-lookup"><span data-stu-id="ea348-208">For example:</span></span>

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

<span data-ttu-id="ea348-209">カリー化中に修正されている拡張メソッドの型パラメーターは、型、メソッドでない型パラメーターの型パラメーターと見なされます。</span><span class="sxs-lookup"><span data-stu-id="ea348-209">Extension method type parameters that were fixed during currying are considered type parameters on the type, not type parameters on the method.</span></span> <span data-ttu-id="ea348-210">例えば:</span><span class="sxs-lookup"><span data-stu-id="ea348-210">For example:</span></span>

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

#### <a name="depth-of-genericity"></a><span data-ttu-id="ea348-211">汎用性の深さ</span><span class="sxs-lookup"><span data-stu-id="ea348-211">Depth of genericity</span></span>

<span data-ttu-id="ea348-212">メンバー`M`が判断されました*汎用性のさらに詳しく*メンバーよりも`N`場合、一致するパラメーターの各ペアの`Mj`と`Nj`、`Mj`が大きい以上*汎用性の深さ*よりも`Nj`、および少なくとも 1 つ`Mj`は汎用性の深さが大きいが。</span><span class="sxs-lookup"><span data-stu-id="ea348-212">A member `M` is determined to have *greater depth of genericity* than a member `N` if, for each pair of matching parameters  `Mj` and `Nj`, `Mj` has greater or equal *depth of genericity* than `Nj`, and at least one `Mj` is has greater depth of genericity.</span></span> <span data-ttu-id="ea348-213">汎用性の深さの定義は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="ea348-213">Depth of genericity is defined as follows:</span></span>

* <span data-ttu-id="ea348-214">型パラメーター以外のものが、汎用性の型パラメーターの場合よりもさらに詳しく</span><span class="sxs-lookup"><span data-stu-id="ea348-214">Anything other than a type parameter has greater depth of genericity than a type parameter;</span></span>

* <span data-ttu-id="ea348-215">再帰的に構築された型が (型引数の同じ番号) を持つ別の構築型よりもさらに詳しく汎用性の少なくとも 1 つの型引数が汎用性のさらに詳しく型引数に対応する型よりも少ないの深さがない場合もう一方の引数。</span><span class="sxs-lookup"><span data-stu-id="ea348-215">Recursively, a constructed type has greater depth of genericity than another constructed type (with the same number of type arguments) if at least one type argument has greater depth of genericity and no type argument has less depth than the corresponding type argument in the other.</span></span>

* <span data-ttu-id="ea348-216">配列型では、最初の要素の型がある 2 番目の要素の型よりもさらに詳しく汎用性の場合 (同じ次元数) で別の配列型よりも汎用性のさらに詳しくがあります。</span><span class="sxs-lookup"><span data-stu-id="ea348-216">An array type has greater depth of genericity than another array type (with the same number of dimensions) if the element type of the first has greater depth of genericity than the element type of the second.</span></span>

<span data-ttu-id="ea348-217">例えば:</span><span class="sxs-lookup"><span data-stu-id="ea348-217">For example:</span></span>

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

### <a name="applicability-to-argument-list"></a><span data-ttu-id="ea348-218">引数リストへの適用性</span><span class="sxs-lookup"><span data-stu-id="ea348-218">Applicability To Argument List</span></span>

<span data-ttu-id="ea348-219">メソッドは、*適用*型引数、引数の位置指定、および引数リストを使用するメソッドを呼び出すことができる場合、名前付き引数のセットにします。</span><span class="sxs-lookup"><span data-stu-id="ea348-219">A method is *applicable* to a set of type arguments, positional arguments, and named arguments if the method can be invoked using the argument lists.</span></span> <span data-ttu-id="ea348-220">次のように、引数リストのパラメーターが照合されますを示します。</span><span class="sxs-lookup"><span data-stu-id="ea348-220">The argument lists are matched against the parameter lists as follows:</span></span>

1. <span data-ttu-id="ea348-221">最初に、メソッドのパラメーターの一覧の順序で各位置指定引数を一致します。</span><span class="sxs-lookup"><span data-stu-id="ea348-221">First, match each positional argument in order to the list of method parameters.</span></span> <span data-ttu-id="ea348-222">位置指定引数のパラメーターよりもがあり、最後のパラメーターが、paramarray ではない場合、メソッドは適用されません。</span><span class="sxs-lookup"><span data-stu-id="ea348-222">If there are more positional arguments than parameters and the last parameter is not a paramarray, the method is not applicable.</span></span> <span data-ttu-id="ea348-223">それ以外の場合、paramarray パラメーターは位置指定引数の数と一致する paramarray 要素型のパラメーターを持つ展開されます。</span><span class="sxs-lookup"><span data-stu-id="ea348-223">Otherwise, the paramarray parameter is expanded with parameters of the paramarray element type to match the number of positional arguments.</span></span> <span data-ttu-id="ea348-224">位置指定引数を省略した場合は、paramarray に移動すると、メソッドは適用されません。</span><span class="sxs-lookup"><span data-stu-id="ea348-224">If a positional argument is omitted that would go into a paramarray, the method is not applicable.</span></span>
2. <span data-ttu-id="ea348-225">次に、各名前付き引数を指定した名前のパラメーターに一致します。</span><span class="sxs-lookup"><span data-stu-id="ea348-225">Next, match each named argument to a parameter with the given name.</span></span> <span data-ttu-id="ea348-226">Paramarray パラメーターと一致する、または別の位置または名前付き引数と既に一致、引数と一致する名前付き引数のいずれかに一致しない場合、メソッドは適用されません。</span><span class="sxs-lookup"><span data-stu-id="ea348-226">If one of the named arguments fails to match, matches a paramarray parameter, or matches an argument already matched with another positional or named argument, the method is not applicable.</span></span>
3. <span data-ttu-id="ea348-227">次に、型引数が指定されている場合は、型パラメーター リストとが照合されます。</span><span class="sxs-lookup"><span data-stu-id="ea348-227">Next, if type arguments have been specified, they are matched against the type parameter list .</span></span> <span data-ttu-id="ea348-228">2 つのリストが同じ数の要素を持たない場合、メソッドは、型引数リストが空でない限りと、適用されません。</span><span class="sxs-lookup"><span data-stu-id="ea348-228">If the two lists do not have the same number of elements, the method is not applicable, unless the type argument list is empty.</span></span> <span data-ttu-id="ea348-229">型引数リストが空の場合、型の推定は、型引数リストを推測してみてくださいに使用されます。</span><span class="sxs-lookup"><span data-stu-id="ea348-229">If the type argument list is empty, type inference is used to try and infer the type argument list.</span></span> <span data-ttu-id="ea348-230">型の推論が失敗した場合、メソッドは適用されません。</span><span class="sxs-lookup"><span data-stu-id="ea348-230">If type inferencing fails, the method is not applicable.</span></span> <span data-ttu-id="ea348-231">それ以外の場合、型引数は、シグネチャの型パラメーターの代わりに入力されます。対応していないパラメーターが省略可能でない場合、メソッドは適用されません。</span><span class="sxs-lookup"><span data-stu-id="ea348-231">Otherwise, the type arguments are filled in the place of the type parameters in the signature.If parameters that have not been matched are not optional, the method is not applicable.</span></span>
4. <span data-ttu-id="ea348-232">引数の式は、パラメーターの型に暗黙的に変換がない場合は、一致すると、し、メソッドは適用されません。</span><span class="sxs-lookup"><span data-stu-id="ea348-232">If the argument expressions are not implicitly convertible to the types of the parameters they match, then the method is not applicable.</span></span>
5. <span data-ttu-id="ea348-233">パラメーターが ByRef パラメーターの型から、引数の型への暗黙的な変換がない場合は、このメソッドは適用されません。</span><span class="sxs-lookup"><span data-stu-id="ea348-233">If a parameter is ByRef, and there is not an implicit conversion from the type of the parameter to the type of the argument, then the method is not applicable.</span></span>
6. <span data-ttu-id="ea348-234">(手順 3 から推論された型引数を含む)、メソッドの制約に違反すると、型引数がある場合、メソッドは適用されません。</span><span class="sxs-lookup"><span data-stu-id="ea348-234">If type arguments violate the method's constraints (including the inferred type arguments from Step 3), the method is not applicable.</span></span> <span data-ttu-id="ea348-235">例えば:</span><span class="sxs-lookup"><span data-stu-id="ea348-235">For example:</span></span>

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

<span data-ttu-id="ea348-236">メソッドがどちらの展開と展開されていない形式で、該当する 1 つの引数式が paramarray パラメーターと一致する引数式の型が paramarray パラメーターの型と paramarray 要素の型に変換できる場合は、2 つの例外。</span><span class="sxs-lookup"><span data-stu-id="ea348-236">If a single argument expression matches a paramarray parameter and the type of the argument expression is convertible to both the type of the paramarray parameter and the paramarray element type, the method is applicable in both its expanded and unexpanded forms, with two exceptions.</span></span> <span data-ttu-id="ea348-237">引数式の型から paramarray 型への変換を絞り込む場合、メソッドは拡張形式で適用可能なのみ。</span><span class="sxs-lookup"><span data-stu-id="ea348-237">If the conversion from the type of the argument expression to the paramarray type is narrowing, then the method is only applicable in its expanded form.</span></span> <span data-ttu-id="ea348-238">引数式が、リテラルの場合`Nothing`メソッドは、展開されていない形式で該当するだけです。</span><span class="sxs-lookup"><span data-stu-id="ea348-238">If the argument expression is the literal `Nothing`, then the method is only applicable in its unexpanded form.</span></span> <span data-ttu-id="ea348-239">例えば:</span><span class="sxs-lookup"><span data-stu-id="ea348-239">For example:</span></span>

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

<span data-ttu-id="ea348-240">上記の例では、次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="ea348-240">The above example produces the following output:</span></span>

```
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

<span data-ttu-id="ea348-241">最初と最後の呼び出しで`F`、通常の形式`F`はパラメーターの型に引数の型から拡大変換が存在するために適用 (型の両方が`Object()`)、通常の値として渡される引数、およびパラメーター。</span><span class="sxs-lookup"><span data-stu-id="ea348-241">In the first and last invocations of `F`, the normal form of `F` is applicable because a widening conversion exists from the argument type to the parameter type (both are of type `Object()`), and the argument is passed as a regular value parameter.</span></span> <span data-ttu-id="ea348-242">2 番目と 3 番目の呼び出し、通常の形式で`F`拡大変換が存在しないため、引数の型からパラメーターの型には適用されません (から変換`Object`に`Object()`縮小)。</span><span class="sxs-lookup"><span data-stu-id="ea348-242">In the second and third invocations, the normal form of `F` is not applicable because no widening conversion exists from the argument type to the parameter type (conversions from `Object` to `Object()` are narrowing).</span></span> <span data-ttu-id="ea348-243">ただし、拡張の形式の`F`該当する、1 つの要素は、`Object()`呼び出しによって作成されます。</span><span class="sxs-lookup"><span data-stu-id="ea348-243">However, the expanded form of `F` is applicable, and a one-element `Object()` is created by the invocation.</span></span> <span data-ttu-id="ea348-244">配列の 1 つの要素が指定された引数の値で初期化されます (それ自体がへの参照、 `Object()`)。</span><span class="sxs-lookup"><span data-stu-id="ea348-244">The single element of the array is initialized with the given argument value (which itself is a reference to an `Object()`).</span></span>

### <a name="passing-arguments-and-picking-arguments-for-optional-parameters"></a><span data-ttu-id="ea348-245">引数を渡すと、省略可能なパラメーターの引数を選択</span><span class="sxs-lookup"><span data-stu-id="ea348-245">Passing Arguments, and Picking Arguments for Optional Parameters</span></span>

<span data-ttu-id="ea348-246">パラメーターが値を持つパラメーターの場合は、一致する引数の式が値として分類する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ea348-246">If a parameter is a value parameter, the matching argument expression must be classified as a value.</span></span> <span data-ttu-id="ea348-247">値では、パラメーターの型に変換され、実行時に、パラメーターとして渡されました。</span><span class="sxs-lookup"><span data-stu-id="ea348-247">The value is converted to the type of the parameter and passed in as the parameter at run time.</span></span> <span data-ttu-id="ea348-248">このパラメーターは参照パラメーターと一致する引数式は、パラメーターと同じ型の変数として分類されます場合、変数への参照がパラメーターとして渡さ、実行時に。</span><span class="sxs-lookup"><span data-stu-id="ea348-248">If the parameter is a reference parameter and the matching argument expression is classified as a variable whose type is the same as the parameter, then a reference to the variable is passed in as the parameter at run time.</span></span>

<span data-ttu-id="ea348-249">それ以外の場合、一致する引数の式は、変数、値、またはプロパティへのアクセスに分類されるが場合、は、パラメーターの型の一時変数が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="ea348-249">Otherwise, if the matching argument expression is classified as a variable, value, or property access, then a temporary variable of the type of the parameter is allocated.</span></span> <span data-ttu-id="ea348-250">引数式が値として再分類は実行時にメソッドの呼び出しの前には、パラメーターの型に変換され、一時変数に割り当てられています。</span><span class="sxs-lookup"><span data-stu-id="ea348-250">Before the method invocation at run time, the argument expression is reclassified as a value, converted to the type of the parameter, and assigned to the temporary variable.</span></span> <span data-ttu-id="ea348-251">一時変数への参照は、パラメーターとしてに渡されます。</span><span class="sxs-lookup"><span data-stu-id="ea348-251">Then a reference to the temporary variable is passed in as the parameter.</span></span> <span data-ttu-id="ea348-252">メソッドの呼び出しを評価すると、引数式が変数またはプロパティ アクセスに分類される場合は、変数の式またはプロパティ アクセス式に一時変数が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="ea348-252">After the method invocation is evaluated, if the argument expression is classified as a variable or property access, the temporary variable is assigned to the variable expression or the property access expression.</span></span> <span data-ttu-id="ea348-253">プロパティ アクセス式がにない場合`Set`アクセサー、割り当ては実行されません。</span><span class="sxs-lookup"><span data-stu-id="ea348-253">If the property access expression has no `Set` accessor, then the assignment is not performed.</span></span>

<span data-ttu-id="ea348-254">省略可能なパラメーターが位置引数が指定されていない、コンパイラは、以下に示すように引数を取得します。</span><span class="sxs-lookup"><span data-stu-id="ea348-254">For optional parameters where an argument has not been provided, the compiler picks arguments as described below.</span></span> <span data-ttu-id="ea348-255">常にテスト パラメーターの型をジェネリック型の置換後にします。</span><span class="sxs-lookup"><span data-stu-id="ea348-255">In all cases it tests against the parameter type after generic type substitution.</span></span>

* <span data-ttu-id="ea348-256">省略可能なパラメーターに属性がある場合`System.Runtime.CompilerServices.CallerLineNumber`、および呼び出しは、ソース コード内の場所からはその場所の行番号を表す数値リテラルが、パラメーターの型への組み込みの変換し、数値リテラルを使用します。</span><span class="sxs-lookup"><span data-stu-id="ea348-256">If the optional parameter has the attribute `System.Runtime.CompilerServices.CallerLineNumber`, and the invocation is from a location in source code, and a numeric literal representing that location's line number has an intrinsic conversion to the parameter type, then the numeric literal is used.</span></span> <span data-ttu-id="ea348-257">呼び出しは、複数の行にまたがっている場合は、実装に依存はのどの行で使用するかを選択します。</span><span class="sxs-lookup"><span data-stu-id="ea348-257">If the invocation spans multiple lines, then the choice of which line to use is implementation-dependent.</span></span>

* <span data-ttu-id="ea348-258">省略可能なパラメーターに属性がある場合`System.Runtime.CompilerServices.CallerFilePath`、および呼び出しは、ソース コード内の場所からはその場所のファイル パスを表す文字列リテラルは、パラメーターの型への組み込みの変換し、文字列リテラルを使用します。</span><span class="sxs-lookup"><span data-stu-id="ea348-258">If the optional parameter has the attribute `System.Runtime.CompilerServices.CallerFilePath`, and the invocation is from a location in source code, and a string literal representing that location's file path has an intrinsic conversion to the parameter type, then the string literal is used.</span></span> <span data-ttu-id="ea348-259">ファイルのパスの形式は、実装によって異なります。</span><span class="sxs-lookup"><span data-stu-id="ea348-259">The format of the file path is implementation-dependent.</span></span>

* <span data-ttu-id="ea348-260">省略可能なパラメーターに属性がある場合`System.Runtime.CompilerServices.CallerMemberName`呼び出しが、その型のメンバーの任意の部分に適用する属性または型のメンバーの本文内と、そのメンバー名を表す文字列リテラルは、パラメーターへの組み込みの変換型、文字列リテラルを使用します。</span><span class="sxs-lookup"><span data-stu-id="ea348-260">If the optional parameter has the attribute `System.Runtime.CompilerServices.CallerMemberName`, and the invocation is within the body of a type member or in an attribute applied to any part of that type member, and a string literal representing that member name has an intrinsic conversion to the parameter type, then the string literal is used.</span></span> <span data-ttu-id="ea348-261">呼び出しではプロパティ アクセサーまたはカスタム イベント ハンドラーの一部であるし、メンバー名が使用されるプロパティまたはイベント自体の。</span><span class="sxs-lookup"><span data-stu-id="ea348-261">For invocations that are part of property accessors or custom event handlers, then the member name used is that of the property or event itself.</span></span> <span data-ttu-id="ea348-262">演算子またはコンス トラクターの一部である呼び出し、実装に固有の名前を使用します。</span><span class="sxs-lookup"><span data-stu-id="ea348-262">For invocations that are part of an operator or constructor, then an implementation-specific name is used.</span></span>

<span data-ttu-id="ea348-263">上記のいずれの場合は省略可能なパラメーターの既定値が使用 (または`Nothing`既定値が指定されていない場合)。</span><span class="sxs-lookup"><span data-stu-id="ea348-263">If none of the above apply, then the optional parameter's default value is used (or `Nothing` if no default value is supplied).</span></span> <span data-ttu-id="ea348-264">数より多い場合、上記のいずれかのどちらを使用する選択肢は実装に依存します。</span><span class="sxs-lookup"><span data-stu-id="ea348-264">And if more than one of the above apply, then the choice of which to use is implementation-dependent.</span></span>

<span data-ttu-id="ea348-265">`CallerLineNumber`と`CallerFilePath`属性は、ログ記録のために便利です。</span><span class="sxs-lookup"><span data-stu-id="ea348-265">The `CallerLineNumber` and `CallerFilePath` attributes are useful for logging.</span></span> <span data-ttu-id="ea348-266">`CallerMemberName`を実装するために役立ちます`INotifyPropertyChanged`します。</span><span class="sxs-lookup"><span data-stu-id="ea348-266">The `CallerMemberName` is useful for implementing `INotifyPropertyChanged`.</span></span> <span data-ttu-id="ea348-267">例を示します。</span><span class="sxs-lookup"><span data-stu-id="ea348-267">Here are examples.</span></span>

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

<span data-ttu-id="ea348-268">上記のオプション パラメーターに加えて Microsoft Visual Basic では、(つまり、DLL の参照) からのメタデータからインポートされた場合は、いくつか追加のオプション パラメーターも認識します。</span><span class="sxs-lookup"><span data-stu-id="ea348-268">In addition to the optional parameters above, Microsoft Visual Basic also recognizes some additional optional parameters if they are imported from metadata (i.e. from a DLL reference):</span></span>

* <span data-ttu-id="ea348-269">メタデータからインポートする時に Visual Basic も扱いますパラメーター`<Optional>`パラメーターは省略可能であると示しています場合でも、このことはできませんが、省略可能なパラメーターが既定値はありません、宣言をインポートすることは、この方法で。使用して表現、`Optional`キーワード。</span><span class="sxs-lookup"><span data-stu-id="ea348-269">Upon importing from metadata, Visual Basic also treats the parameter `<Optional>` as indicative that the parameter is optional: in this way it is possible to import a declaration which has an optional parameter but no default value, even though this can't be expressed using the `Optional` keyword.</span></span>

* <span data-ttu-id="ea348-270">省略可能なパラメーターに属性がある場合`Microsoft.VisualBasic.CompilerServices.OptionCompareAttribute`、数値リテラルの 1 または 0 がパラメーターの型に変換し、リテラルの 1 場合を引数として使用して、コンパイラ`Option Compare Text`特殊効果、またはリテラルの場合は 0 では、`Optional Compare Binary`が有効でします。</span><span class="sxs-lookup"><span data-stu-id="ea348-270">If the optional parameter has the attribute `Microsoft.VisualBasic.CompilerServices.OptionCompareAttribute`, and the numeric literal 1 or 0 has a conversion to the parameter type, then the compiler uses as argument either the literal 1 if `Option Compare Text` is in effect, or the literal 0 if `Optional Compare Binary` is in effect.</span></span>

* <span data-ttu-id="ea348-271">省略可能なパラメーターに属性がある場合`System.Runtime.CompilerServices.IDispatchConstantAttribute`、型があると`Object`、コンパイラは引数を使用し、既定値は指定しません`New System.Runtime.InteropServices.DispatchWrapper(Nothing)`します。</span><span class="sxs-lookup"><span data-stu-id="ea348-271">If the optional parameter has the attribute `System.Runtime.CompilerServices.IDispatchConstantAttribute`, and it has type `Object`, and it does not specify a default value, then the compiler uses the argument `New System.Runtime.InteropServices.DispatchWrapper(Nothing)`.</span></span>

* <span data-ttu-id="ea348-272">省略可能なパラメーターに属性がある場合`System.Runtime.CompilerServices.IUnknownConstantAttribute`、型があると`Object`、コンパイラは引数を使用し、既定値は指定しません`New System.Runtime.InteropServices.UnknownWrapper(Nothing)`します。</span><span class="sxs-lookup"><span data-stu-id="ea348-272">If the optional parameter has the attribute `System.Runtime.CompilerServices.IUnknownConstantAttribute`, and it has type `Object`, and it does not specify a default value, then the compiler uses the argument `New System.Runtime.InteropServices.UnknownWrapper(Nothing)`.</span></span>

* <span data-ttu-id="ea348-273">省略可能なパラメーターの型の場合`Object`、コンパイラは引数を使用し、既定値は指定しません`System.Reflection.Missing.Value`します。</span><span class="sxs-lookup"><span data-stu-id="ea348-273">If the optional parameter has type `Object`, and it does not specify a default value, then the compiler uses the argument `System.Reflection.Missing.Value`.</span></span>

### <a name="conditional-methods"></a><span data-ttu-id="ea348-274">条件付きメソッド</span><span class="sxs-lookup"><span data-stu-id="ea348-274">Conditional Methods</span></span>

<span data-ttu-id="ea348-275">Invocation 式が参照する対象のメソッドがインターフェイスのメンバーでないサブルーチンの場合と、メソッドが 1 つまたは複数`System.Diagnostics.ConditionalAttribute`属性、式の評価によって異なりますで定義した条件付きコンパイル定数ソース ファイルをポイントします。</span><span class="sxs-lookup"><span data-stu-id="ea348-275">If the target method to which an invocation expression refers is a subroutine that is not a member of an interface and if the method has one or more `System.Diagnostics.ConditionalAttribute` attributes, evaluation of the expression depends on the conditional compilation constants defined at that point in the source file.</span></span> <span data-ttu-id="ea348-276">属性の各インスタンスには、文字列は、条件付きコンパイル定数の名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="ea348-276">Each instance of the attribute specifies a string, which names a conditional compilation constant.</span></span> <span data-ttu-id="ea348-277">条件付きコンパイル ステートメントの一部の場合と同様に、各条件付きコンパイル定数が評価されます。</span><span class="sxs-lookup"><span data-stu-id="ea348-277">Each conditional compilation constant is evaluated as if it were part of a conditional compilation statement.</span></span> <span data-ttu-id="ea348-278">評価されると、定数`True`、通常実行時に式が評価されます。</span><span class="sxs-lookup"><span data-stu-id="ea348-278">If the constant evaluates to `True`, the expression is evaluated normally at run time.</span></span> <span data-ttu-id="ea348-279">評価されると、定数`False`式はすべての評価されません。</span><span class="sxs-lookup"><span data-stu-id="ea348-279">If the constant evaluates to `False`, the expression is not evaluated at all.</span></span>

<span data-ttu-id="ea348-280">属性を探すときに、オーバーライド可能なメソッドの最多派生宣言がチェックされます。</span><span class="sxs-lookup"><span data-stu-id="ea348-280">When looking for the attribute, the most derived declaration of an overridable method is checked.</span></span>

<span data-ttu-id="ea348-281">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="ea348-281">__Note.__</span></span> <span data-ttu-id="ea348-282">この属性は、関数またはインターフェイスのメソッドでは無効ですし、メソッドのいずれかの種類に指定されている場合は無視されます。</span><span class="sxs-lookup"><span data-stu-id="ea348-282">The attribute is not valid on functions or interface methods and is ignored if specified on either kind of method.</span></span> <span data-ttu-id="ea348-283">したがって、条件付きメソッドは、呼び出しステートメントでのみ表示されます。</span><span class="sxs-lookup"><span data-stu-id="ea348-283">Thus, conditional methods will only appear in invocation statements.</span></span>

### <a name="type-argument-inference"></a><span data-ttu-id="ea348-284">型引数の推論</span><span class="sxs-lookup"><span data-stu-id="ea348-284">Type Argument Inference</span></span>

<span data-ttu-id="ea348-285">型引数を指定せずに型パラメーターを持つメソッドが呼び出されたときに*入力引数を推定*し、呼び出しの引数の型を推測するために使用します。</span><span class="sxs-lookup"><span data-stu-id="ea348-285">When a method with type parameters is called without specifying type arguments, *type argument inference* is used to try and infer type arguments for the call.</span></span> <span data-ttu-id="ea348-286">これにより、型引数を普通に推論するときに、型パラメーターを持つメソッドを呼び出すために使用するより自然な構文です。</span><span class="sxs-lookup"><span data-stu-id="ea348-286">This allows a more natural syntax to be used for calling a method with type parameters when the type arguments can be trivially inferred.</span></span> <span data-ttu-id="ea348-287">たとえば、次のメソッド宣言を考えてみます。</span><span class="sxs-lookup"><span data-stu-id="ea348-287">For example, given the following method declaration:</span></span>

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

<span data-ttu-id="ea348-288">呼び出すことは、`Choose`型引数を明示的に指定しないでメソッド。</span><span class="sxs-lookup"><span data-stu-id="ea348-288">it is possible to invoke the `Choose` method without explicitly specifying a type argument:</span></span>

```vb
' calls Choose(Of Integer)
Dim i As Integer = Util.Choose(True, 5, 213)
' calls Choose(Of String)
Dim s As String = Util.Choose(False, "a", "b") 
```

<span data-ttu-id="ea348-289">型引数を推定、型引数を介して`Integer`と`String`メソッドに引数から決定されます。</span><span class="sxs-lookup"><span data-stu-id="ea348-289">Through type argument inference, the type arguments `Integer` and `String` are determined from the arguments to the method.</span></span>

<span data-ttu-id="ea348-290">入力引数の推論が発生した*する前に*これら 2 つの式の再分類の種類がかかる場合のため、ラムダ メソッドまたはメソッドのポインター引数リスト内で式の再分類は実行、既知であるパラメーターです。</span><span class="sxs-lookup"><span data-stu-id="ea348-290">Type argument inference occurs *before* expression reclassification is performed on lambda methods or method pointers in the argument list, since reclassification of those two kinds of expressions may require the type of the parameter to be known.</span></span>  <span data-ttu-id="ea348-291">与えられた一連の引数の`A1,...,An`、一連のパラメーターに一致する`P1,...,Pn`とメソッドのパラメーターの入力セット`T1,...,Tn`引数とメソッドの型パラメーター間の依存関係が次のように収集された最初。</span><span class="sxs-lookup"><span data-stu-id="ea348-291">Given a set of arguments `A1,...,An`, a set of matching parameters `P1,...,Pn` and a set of method type parameters `T1,...,Tn`, the dependencies between the arguments and method type parameters are first collected as follows:</span></span>

* <span data-ttu-id="ea348-292">場合`An`は、`Nothing`リテラルには、依存関係は生成されません。</span><span class="sxs-lookup"><span data-stu-id="ea348-292">If `An` is the `Nothing` literal, no dependencies are generated.</span></span>

* <span data-ttu-id="ea348-293">場合`An`ラムダ メソッドの型が`Pn`が構築されたデリゲート型または`System.Linq.Expressions.Expression(Of T)`ここで、`T`が構築されたデリゲート型では、</span><span class="sxs-lookup"><span data-stu-id="ea348-293">If `An` is a lambda method and the type of `Pn` is a constructed delegate type or `System.Linq.Expressions.Expression(Of T)`, where `T` is a constructed delegate type,</span></span>

* <span data-ttu-id="ea348-294">ラムダ メソッドのパラメーターの型は、対応するパラメーターの型から推論する場合`Pn`、パラメーターの型は、メソッド型パラメーターに依存して`Tn`、し`An`に依存している`Tn`します。</span><span class="sxs-lookup"><span data-stu-id="ea348-294">If the type of a lambda method parameter will be inferred from the type of the corresponding parameter `Pn`, and the type of the parameter depends on a method type parameter `Tn`, then `An` has a dependency on `Tn`.</span></span>

* <span data-ttu-id="ea348-295">ラムダ メソッドのパラメーターの型が指定されているかどうかと、対応するパラメーターの型`Pn`、メソッド型パラメーターに依存`Tn`、し`Tn`に依存している`An`します。</span><span class="sxs-lookup"><span data-stu-id="ea348-295">If the type of a lambda method parameter is specified and the type of the corresponding parameter `Pn` depends on a method type parameter `Tn`, then `Tn` has a dependency on `An`.</span></span>

* <span data-ttu-id="ea348-296">戻り値の型の場合`Pn`、メソッド型パラメーターに依存`Tn`、し`Tn`に依存している`An`します。</span><span class="sxs-lookup"><span data-stu-id="ea348-296">If the return type of `Pn` depends on a method type parameter `Tn`, then `Tn` has a dependency on `An`.</span></span>

* <span data-ttu-id="ea348-297">場合`An`メソッドのポインターの種類が`Pn`は、構築されたデリゲート型です。</span><span class="sxs-lookup"><span data-stu-id="ea348-297">If `An` is a method pointer and the type of `Pn` is a constructed delegate type,</span></span>

* <span data-ttu-id="ea348-298">戻り値の型の場合`Pn`、メソッド型パラメーターに依存`Tn`、し`Tn`に依存している`An`します。</span><span class="sxs-lookup"><span data-stu-id="ea348-298">If the return type of `Pn` depends on a method type parameter `Tn`, then `Tn` has a dependency on `An`.</span></span>

* <span data-ttu-id="ea348-299">場合`Pn`が構築された型の種類`Pn`、メソッド型パラメーターに依存`Tn`、し`Tn`に依存している`An`します。</span><span class="sxs-lookup"><span data-stu-id="ea348-299">If `Pn` is a constructed type and the type of `Pn` depends on a method type parameter `Tn`, then `Tn` has a dependency on `An`.</span></span>

* <span data-ttu-id="ea348-300">それ以外の場合、依存関係は生成されません。</span><span class="sxs-lookup"><span data-stu-id="ea348-300">Otherwise, no dependency is generated.</span></span>

<span data-ttu-id="ea348-301">依存関係を収集した後、依存関係を持たない任意の引数は除外されます。</span><span class="sxs-lookup"><span data-stu-id="ea348-301">After collecting dependencies, any arguments that have no dependencies are eliminated.</span></span> <span data-ttu-id="ea348-302">メソッド型パラメーターには、出力の依存関係 (つまり、メソッド型パラメーターに依存しない引数) がない、型の推論は失敗します。</span><span class="sxs-lookup"><span data-stu-id="ea348-302">If any method type parameters have no outgoing dependencies (i.e. the method type parameter does not depend on an argument), then type inference fails.</span></span> <span data-ttu-id="ea348-303">それ以外の場合、残りの引数とメソッドの型パラメーターは、厳密に接続されているコンポーネントにグループ化されます。</span><span class="sxs-lookup"><span data-stu-id="ea348-303">Otherwise, the remaining arguments and method type parameters are grouped into strongly connected components.</span></span> <span data-ttu-id="ea348-304">厳密に接続されているコンポーネントとは、コンポーネント内の任意の要素が他の要素への依存関係で到達可能な引数と、メソッド型パラメーターのセットです。</span><span class="sxs-lookup"><span data-stu-id="ea348-304">A strongly connected component is a set of arguments and method type parameters, where any element in the component is reachable via dependencies on other elements.</span></span>

<span data-ttu-id="ea348-305">厳密に接続されているコンポーネントの位相的並べ替えおよびトポロジの順序で処理します。</span><span class="sxs-lookup"><span data-stu-id="ea348-305">The strongly connected components are then topologically sorted and processed in topological order:</span></span>

* <span data-ttu-id="ea348-306">厳密に型指定されたコンポーネントには、1 つだけの要素が含まれている場合</span><span class="sxs-lookup"><span data-stu-id="ea348-306">If the strongly typed component contains only one element,</span></span>

  * <span data-ttu-id="ea348-307">要素が既にマークされている完全なスキップします。</span><span class="sxs-lookup"><span data-stu-id="ea348-307">If the element has already been marked complete, skip it.</span></span>

  * <span data-ttu-id="ea348-308">要素が、引数の場合は型ヒント引数からを依存していると、要素を完了としてマークするメソッドの型パラメーターを追加します。</span><span class="sxs-lookup"><span data-stu-id="ea348-308">If the element is an argument, then add type hints from the argument to the method type parameters that depend on it and mark the element as complete.</span></span> <span data-ttu-id="ea348-309">引数が型を推論する必要があるパラメーターを持つラムダ メソッドの場合、推論`Object`これらのパラメーターの型。</span><span class="sxs-lookup"><span data-stu-id="ea348-309">If the argument is a lambda method with parameters that still need inferred types, then infer `Object` for the types of those parameters.</span></span>

  * <span data-ttu-id="ea348-310">要素が、メソッド型パラメーターの場合は、引数の型ヒントの間での主要な型にして、要素を完了としてマークするメソッドの型パラメーターを推測します。</span><span class="sxs-lookup"><span data-stu-id="ea348-310">If the element is a method type parameter, then infer the method type parameter to be the dominant type among the argument type hints and mark the element as complete.</span></span> <span data-ttu-id="ea348-311">場合は、型ヒントでは、上に、配列要素の制限がある、指定された型の配列間で有効な変換のみは (つまり共変と組み込みの配列の変換) と見なされます。</span><span class="sxs-lookup"><span data-stu-id="ea348-311">If a type hint has an array element restriction on it, then only conversions that are valid between arrays of the given type are considered (i.e. covariant and intrinsic array conversions).</span></span> <span data-ttu-id="ea348-312">型ヒントは、汎用引数の制限を上にあるを場合は、恒等変換のみと見なされます。</span><span class="sxs-lookup"><span data-stu-id="ea348-312">If a type hint has a generic argument restriction on it, then only identity conversions are considered.</span></span> <span data-ttu-id="ea348-313">主要な型を選択しない場合は、推論が失敗します。</span><span class="sxs-lookup"><span data-stu-id="ea348-313">If no dominant type can be chosen, inference fails.</span></span> <span data-ttu-id="ea348-314">任意のラムダ メソッド引数の型は、このメソッドの型パラメーターに依存している場合、型がラムダ メソッドに反映されます。</span><span class="sxs-lookup"><span data-stu-id="ea348-314">If any lambda method argument types depend on this method type parameter, the type is propagated to the lambda method.</span></span>

* <span data-ttu-id="ea348-315">厳密に型指定されたコンポーネントには、複数の要素が含まれている場合、コンポーネントが循環しています。</span><span class="sxs-lookup"><span data-stu-id="ea348-315">If the strongly typed component contains more than one element, then the component contains a cycle.</span></span>

  * <span data-ttu-id="ea348-316">コンポーネント内の要素を各メソッドの型パラメーター、メソッドの型パラメーターは、完了マークされていない引数に依存している場合は、推論プロセスの最後にチェックされるアサーションにその依存関係を変換します。</span><span class="sxs-lookup"><span data-stu-id="ea348-316">For each method type parameter that is an element in the component, if the method type parameter depends on an argument that is not marked complete, convert that dependency into an assertion that will be checked at the end of the inference process.</span></span>

  * <span data-ttu-id="ea348-317">厳密に型指定されたコンポーネントの判断ポイントで推論プロセスを再起動します。</span><span class="sxs-lookup"><span data-stu-id="ea348-317">Restart the inference process at the point at which the strongly typed components were determined.</span></span>

<span data-ttu-id="ea348-318">すべてのメソッドの型パラメーターの型の推定が成功すると、アサーションに変更されたすべての依存関係がチェックされます。</span><span class="sxs-lookup"><span data-stu-id="ea348-318">If type inference succeeds for all of the method type parameters, then any dependencies that were changed into assertions are checked.</span></span> <span data-ttu-id="ea348-319">アサーションは、引数の型がメソッドの型パラメーターの推論された型に暗黙的に変換できる場合は成功します。</span><span class="sxs-lookup"><span data-stu-id="ea348-319">An assertion succeeds if the type of the argument is implicitly convertible to the inferred type of the method type parameter.</span></span> <span data-ttu-id="ea348-320">アサーションに失敗した場合は、型引数の推論が失敗します。</span><span class="sxs-lookup"><span data-stu-id="ea348-320">If an assertion fails, then type argument inference fails.</span></span>

<span data-ttu-id="ea348-321">引数の型指定された`Ta`引数`A`パラメーターの型と`Tp`パラメーターの`P`、型ヒントは次のように生成されます。</span><span class="sxs-lookup"><span data-stu-id="ea348-321">Given an argument type `Ta` for an argument `A` and a parameter type `Tp` for a parameter `P`, type hints are generated as follows:</span></span>

* <span data-ttu-id="ea348-322">場合`Tp`メソッド型パラメーターを含まない場合、ヒントは生成されません。</span><span class="sxs-lookup"><span data-stu-id="ea348-322">If `Tp` does not involve any method type parameters then no hints are generated.</span></span>

* <span data-ttu-id="ea348-323">場合`Tp`と`Ta`、同じランクの配列型は、置換`Ta`と`Tp`要素の種類の`Ta`と`Tp`に配列要素の制限は、このプロセスを再起動します。</span><span class="sxs-lookup"><span data-stu-id="ea348-323">If `Tp` and `Ta` are array types of the same rank, then replace `Ta` and `Tp` with the element types of `Ta` and `Tp` and restart this process with an array element restriction.</span></span>

* <span data-ttu-id="ea348-324">場合`Tp`し、メソッド型パラメーターは、`Ta`存在する場合は、現在の制限では、型ヒントとして追加します。</span><span class="sxs-lookup"><span data-stu-id="ea348-324">If `Tp` is a method type parameter, then `Ta` is added as a type hint with the current restriction, if any.</span></span>

* <span data-ttu-id="ea348-325">場合`A`ラムダ メソッドと`Tp`が構築されたデリゲート型または`System.Linq.Expressions.Expression(Of T)`ここで、`T`ラムダ メソッド パラメーターの種類ごとの構築されたデリゲート型は、`TL`とデリゲート パラメーター型に対応します。`TD`、置き換える`Ta`で`TL`と`Tp`で`TD`の制限なしでプロセスを再起動しています。</span><span class="sxs-lookup"><span data-stu-id="ea348-325">If `A` is a lambda method and `Tp` is a constructed delegate type or `System.Linq.Expressions.Expression(Of T)`, where `T` is a constructed delegate type, for each lambda method parameter type `TL` and corresponding delegate parameter type `TD`, replace `Ta` with `TL` and `Tp` with `TD` and restart the process with no restriction.</span></span> <span data-ttu-id="ea348-326">次に置き換えます。`Ta`ラムダ メソッドの戻り値の型と。</span><span class="sxs-lookup"><span data-stu-id="ea348-326">Then, replace `Ta` with the return type of the lambda method and:</span></span>

  * <span data-ttu-id="ea348-327">場合`A`正規のラムダ メソッドで置き換えます`Tp`デリゲート型の戻り値の型</span><span class="sxs-lookup"><span data-stu-id="ea348-327">if `A` is a regular lambda method, replace `Tp` with the return type of the delegate type;</span></span>
  * <span data-ttu-id="ea348-328">場合`A`非同期のラムダ メソッドであるし、デリゲート型の戻り値の型がフォーム`Task(Of T)`一部`T`、置き換える`Tp`を`T`;</span><span class="sxs-lookup"><span data-stu-id="ea348-328">if `A` is an async lambda method and the return type of the delegate type has form `Task(Of T)` for some `T`, replace `Tp` with that `T`;</span></span>
  * <span data-ttu-id="ea348-329">場合`A`反復子ラムダ メソッドは、デリゲート型の戻り値の型がフォーム`IEnumerator(Of T)`または`IEnumerable(Of T)`一部`T`、置換`Tp`を`T`します。</span><span class="sxs-lookup"><span data-stu-id="ea348-329">if `A` is an iterator lambda method and the return type of the delegate type has form `IEnumerator(Of T)` or `IEnumerable(Of T)` for some `T`, replace `Tp` with that `T`.</span></span>
  * <span data-ttu-id="ea348-330">次に、制限なしでプロセスを再起動します。</span><span class="sxs-lookup"><span data-stu-id="ea348-330">Next, restart the process with no restriction.</span></span>

* <span data-ttu-id="ea348-331">場合`A`メソッドのポインターと`Tp`は構築のパラメーターの型を使用してデリゲート型を`Tp`どの方法が示されるを決定するに最も適した`Tp`。</span><span class="sxs-lookup"><span data-stu-id="ea348-331">If `A` is a method pointer and `Tp` is a constructed delegate type, use the parameter types of `Tp` to determine which method pointed is most applicable to `Tp`.</span></span> <span data-ttu-id="ea348-332">最適なメソッドがある場合は、置き換える`Ta`メソッドの戻り値の型と`Tp`再起動プロセスの制限なしで、デリゲート型の戻り値の型。</span><span class="sxs-lookup"><span data-stu-id="ea348-332">If there is a method that is most applicable, replace `Ta` with the return type of the method and `Tp` with the return type of the delegate type and restart the process with no restriction.</span></span>

* <span data-ttu-id="ea348-333">それ以外の場合、`Tp`構築された型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="ea348-333">Otherwise, `Tp` must be a constructed type.</span></span> <span data-ttu-id="ea348-334">指定された`TG`のジェネリック型`Tp`、</span><span class="sxs-lookup"><span data-stu-id="ea348-334">Given `TG`, the generic type of `Tp`,</span></span>

  * <span data-ttu-id="ea348-335">場合`Ta`は`TG`から継承`TG`、型を実装または`TG`に 1 回だけの一致する型引数はそれぞれ、`Tax`から`Ta`と`Tpx`から`Tp`を置き換えます`Ta`で`Tax`と`Tp`で`Tpx`ジェネリック引数の制限を使用して、プロセスを再起動します。</span><span class="sxs-lookup"><span data-stu-id="ea348-335">If `Ta` is `TG`, inherits from `TG`, or implements the type `TG` exactly once, then for each matching type argument `Tax` from `Ta` and `Tpx` from `Tp`, replace `Ta` with `Tax` and `Tp` with `Tpx` and restart the process with a generic argument restriction.</span></span>

  * <span data-ttu-id="ea348-336">それ以外の場合、ジェネリック メソッドの型の推定が失敗します。</span><span class="sxs-lookup"><span data-stu-id="ea348-336">Otherwise, type inference fails for the generic method.</span></span>

<span data-ttu-id="ea348-337">型の推論の成功した場合でと、それ自体の保証されません、メソッドは、適用されます。</span><span class="sxs-lookup"><span data-stu-id="ea348-337">The success of type inference does not, in and of itself, guarantee that the method is applicable.</span></span>
