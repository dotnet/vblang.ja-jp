---
ms.openlocfilehash: 8f250f8cee957fa60e7970ace250e8d7ce145fd7
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306169"
---
# <a name="expressions"></a><span data-ttu-id="8ad58-101">式</span><span class="sxs-lookup"><span data-stu-id="8ad58-101">Expressions</span></span>

<span data-ttu-id="8ad58-102">式は、値の計算を指定する、または変数または定数を指定する演算子とオペランドのシーケンスです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-102">An expression is a sequence of operators and operands that specifies a computation of a value, or that designates a variable or constant.</span></span> <span data-ttu-id="8ad58-103">この章では、構文、オペランドと演算子の評価順序、および式の意味を定義します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-103">This chapter defines the syntax, order of evaluation of operands and operators, and meaning of expressions.</span></span>

```antlr
Expression
    : SimpleExpression
    | TypeExpression
    | MemberAccessExpression
    | DictionaryAccessExpression
    | InvocationExpression
    | IndexExpression
    | NewExpression
    | CastExpression
    | OperatorExpression
    | ConditionalExpression
    | LambdaExpression
    | QueryExpression
    | XMLLiteralExpression
    | XMLMemberAccessExpression
    ;
```

## <a name="expression-classifications"></a><span data-ttu-id="8ad58-104">式の分類</span><span class="sxs-lookup"><span data-stu-id="8ad58-104">Expression Classifications</span></span>

<span data-ttu-id="8ad58-105">すべての式は、次のいずれかに分類されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-105">Every expression is classified as one of the following:</span></span>

* <span data-ttu-id="8ad58-106">*値。*</span><span class="sxs-lookup"><span data-stu-id="8ad58-106">*A value.*</span></span> <span data-ttu-id="8ad58-107">すべての値には、型が関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-107">Every value has an associated type.</span></span>

* <span data-ttu-id="8ad58-108">*変数。*</span><span class="sxs-lookup"><span data-stu-id="8ad58-108">*A variable.*</span></span> <span data-ttu-id="8ad58-109">すべての変数には、関連付けられた型、つまり変数の宣言型があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-109">Every variable has an associated type, namely the declared type of the variable.</span></span>

* <span data-ttu-id="8ad58-110">*名前空間。*</span><span class="sxs-lookup"><span data-stu-id="8ad58-110">*A namespace.*</span></span> <span data-ttu-id="8ad58-111">この分類の式は、メンバーアクセスの左側としてのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-111">An expression with this classification can only appear as the left side of a member access.</span></span> <span data-ttu-id="8ad58-112">その他のコンテキストでは、名前空間として分類された式によってコンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-112">In any other context, an expression classified as a namespace causes a compile-time error.</span></span>

* <span data-ttu-id="8ad58-113">*型。*</span><span class="sxs-lookup"><span data-stu-id="8ad58-113">*A type.*</span></span> <span data-ttu-id="8ad58-114">この分類の式は、メンバーアクセスの左側としてのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-114">An expression with this classification can only appear as the left side of a member access.</span></span> <span data-ttu-id="8ad58-115">その他のコンテキストでは、型として分類された式によってコンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-115">In any other context, an expression classified as a type causes a compile-time error.</span></span>

* <span data-ttu-id="8ad58-116">*メソッドグループ*。同じ名前でオーバーロードされた一連のメソッドです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-116">*A method group,* which is a set of methods overloaded on the same name.</span></span> <span data-ttu-id="8ad58-117">メソッドグループには、関連付けられたターゲット式と、関連付けられた型引数リストを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-117">A method group may have an associated target expression and an associated type argument list.</span></span>

* <span data-ttu-id="8ad58-118">メソッドの場所を表す*メソッドポインター* 。</span><span class="sxs-lookup"><span data-stu-id="8ad58-118">*A method pointer,* which represents the location of a method.</span></span> <span data-ttu-id="8ad58-119">メソッドポインターには、関連付けられたターゲット式と、関連する型引数リストを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-119">A method pointer may have an associated target expression and an associated type argument list.</span></span>

* <span data-ttu-id="8ad58-120">*ラムダメソッド (* 匿名メソッド)。</span><span class="sxs-lookup"><span data-stu-id="8ad58-120">*A lambda method,* which is an anonymous method.</span></span>

* <span data-ttu-id="8ad58-121">*プロパティグループ*。同じ名前でオーバーロードされた一連のプロパティです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-121">*A property group,* which is a set of properties overloaded on the same name.</span></span> <span data-ttu-id="8ad58-122">プロパティグループには、関連付けられたターゲット式を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-122">A property group may have an associated target expression.</span></span>

* <span data-ttu-id="8ad58-123">*プロパティアクセス。*</span><span class="sxs-lookup"><span data-stu-id="8ad58-123">*A property access.*</span></span> <span data-ttu-id="8ad58-124">すべてのプロパティアクセスには、関連付けられた型、つまりプロパティの型があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-124">Every property access has an associated type, namely the type of the property.</span></span> <span data-ttu-id="8ad58-125">プロパティアクセスには、関連付けられたターゲット式を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-125">A property access may have an associated target expression.</span></span>

* <span data-ttu-id="8ad58-126">*遅延バインディングアクセス*。実行時まで遅延されるメソッドまたはプロパティアクセスを表します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-126">*A late-bound access,* which represents a method or property access deferred until run-time.</span></span> <span data-ttu-id="8ad58-127">遅延バインディングアクセスには、関連付けられたターゲット式と、関連付けられた型引数リストが含まれる場合があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-127">A late-bound access may have an associated target expression and an associated type argument list.</span></span> <span data-ttu-id="8ad58-128">遅延バインディングアクセスの種類は常に `Object` です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-128">The type of a late-bound access is always `Object`.</span></span>

* <span data-ttu-id="8ad58-129">*イベントアクセス。*</span><span class="sxs-lookup"><span data-stu-id="8ad58-129">*An event access.*</span></span> <span data-ttu-id="8ad58-130">すべてのイベントアクセスには、関連付けられた型、つまりイベントの種類があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-130">Every event access has an associated type, namely the type of the event.</span></span> <span data-ttu-id="8ad58-131">イベントアクセスには、関連付けられたターゲット式を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-131">An event access may have an associated target expression.</span></span> <span data-ttu-id="8ad58-132">イベントアクセスは、`RaiseEvent`、`AddHandler`、および `RemoveHandler` ステートメントの最初の引数として表示される場合があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-132">An event access may appear as the first argument of the `RaiseEvent`, `AddHandler`, and `RemoveHandler` statements.</span></span> <span data-ttu-id="8ad58-133">その他のコンテキストでは、イベントアクセスとして分類された式によってコンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-133">In any other context, an expression classified as an event access causes a compile-time error.</span></span>

* <span data-ttu-id="8ad58-134">*配列リテラル*。型がまだ決定されていない配列の初期値を表します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-134">*An array literal,* which represents the initial values of an array whose type has not yet been determined.</span></span>

* <span data-ttu-id="8ad58-135">*無効化.*</span><span class="sxs-lookup"><span data-stu-id="8ad58-135">*Void.*</span></span> <span data-ttu-id="8ad58-136">このエラーは、式がサブルーチンの呼び出し、または結果のない await 演算子式の場合に発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-136">This occurs when the expression is an invocation of a subroutine, or an await operator expression with no result.</span></span> <span data-ttu-id="8ad58-137">Void として分類される式は、呼び出しステートメントまたは await ステートメントのコンテキストでのみ有効です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-137">An expression classified as void is only valid in the context of an invocation statement or an await statement.</span></span>

* <span data-ttu-id="8ad58-138">*既定値。*</span><span class="sxs-lookup"><span data-stu-id="8ad58-138">*A default value.*</span></span> <span data-ttu-id="8ad58-139">この分類を生成するのは、リテラル `Nothing` だけです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-139">Only the literal `Nothing` produces this classification.</span></span>

<span data-ttu-id="8ad58-140">式の最終的な結果は、通常、値または変数です。他のカテゴリの式は、特定のコンテキストでのみ許可される中間値として機能します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-140">The final result of an expression is usually a value or a variable, with the other categories of expressions functioning as intermediate values that are only permitted in certain contexts.</span></span>

<span data-ttu-id="8ad58-141">型パラメーターを持つ式は、式の型に特定の特性 (参照型、値型、何らかの型からの派生など) を指定する必要がある場合に、制約が適用されている場合に使用できます。型パラメーターでは、これらの特性を満たしています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-141">Note that expressions whose type is a type parameter can be used in statements and expressions that require the type of an expression to have certain characteristics (such as being a reference type, value type, deriving from some type, etc.) if the constraints imposed on the type parameter satisfy those characteristics.</span></span>

### <a name="expression-reclassification"></a><span data-ttu-id="8ad58-142">式の再分類</span><span class="sxs-lookup"><span data-stu-id="8ad58-142">Expression Reclassification</span></span>

<span data-ttu-id="8ad58-143">通常、式が式とは異なる分類を必要とするコンテキストで使用されると、コンパイル時エラーが発生します。たとえば、リテラルに値を割り当てようとした場合などです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-143">Normally, when an expression is used in a context that requires a classification different from that of the expression, a compile-time error occurs -- for example, attempting to assign a value to a literal.</span></span> <span data-ttu-id="8ad58-144">ただし、多くの場合、再*分類*のプロセスを通じて、式の分類を変更することができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-144">However, in many cases it is possible to change an expression's classification through the process of *reclassification*.</span></span>

<span data-ttu-id="8ad58-145">再分類が成功した場合、再分類は拡大または縮小として判断されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-145">If reclassification succeeds, then the reclassification is judged as widening or narrowing.</span></span> <span data-ttu-id="8ad58-146">特に明記されていない限り、この一覧のすべての再分類は拡大されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-146">Unless otherwise noted, all the reclassifications in this list are widening.</span></span>

<span data-ttu-id="8ad58-147">次の種類の式を再分類できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-147">The following types of expressions can be reclassified:</span></span>

* <span data-ttu-id="8ad58-148">変数は、値として再分類できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-148">A variable can be reclassified as a value.</span></span> <span data-ttu-id="8ad58-149">変数に格納された値がフェッチされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-149">The value stored in the variable is fetched.</span></span>

* <span data-ttu-id="8ad58-150">メソッドグループは、値として再分類できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-150">A method group can be reclassified as a value.</span></span> <span data-ttu-id="8ad58-151">メソッドグループ式は、関連するターゲット式と型パラメーターリストを持つ呼び出し式として解釈されます。また、空のかっこ (つまり、@no__t 0 は `f()` と解釈され、`f(Of Integer)` は `f(Of Integer)()` と解釈されます)。</span><span class="sxs-lookup"><span data-stu-id="8ad58-151">The method group expression is interpreted as an invocation expression with the associated target expression and type parameter list, and empty parentheses (that is, `f` is interpreted as `f()` and `f(Of Integer)` is interpreted as `f(Of Integer)()`).</span></span> <span data-ttu-id="8ad58-152">この再分類により、式がさらに void として再分類される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-152">This reclassification may result in the expression being further reclassified as void.</span></span>

* <span data-ttu-id="8ad58-153">メソッドポインターは、値として再分類できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-153">A method pointer can be reclassified as a value.</span></span> <span data-ttu-id="8ad58-154">この再分類は、対象の型がわかっている変換のコンテキストでのみ発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-154">This reclassification can only occur in the context of a conversion where the target type is known.</span></span> <span data-ttu-id="8ad58-155">メソッドポインター式は、関連付けられた型引数リストを使用して、適切な型のデリゲートインスタンス化式への引数として解釈されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-155">The method pointer expression is interpreted as the argument to a delegate instantiation expression of the appropriate type with the associated type argument list.</span></span> <span data-ttu-id="8ad58-156">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-156">For example:</span></span>
    
    ```vb
    Delegate Sub D(i As Integer)
    
    Module Test
        Sub F(i As Integer)
        End Sub
    
        Sub Main()
            Dim del As D
    
            ' The next two lines are equivalent.
            del = AddressOf F
            del = New D(AddressOf F)
        End Sub
    End Module
    ```

* <span data-ttu-id="8ad58-157">ラムダメソッドは、値として再分類できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-157">A lambda method can be reclassified as a value.</span></span> <span data-ttu-id="8ad58-158">ターゲットの型がわかっている変換のコンテキストで再分類が行われた場合、次の2つの再分類のいずれかが発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-158">If the reclassification occurs in the context of a conversion where the target type is known, then one of two reclassifications can occur:</span></span>
    
  1. <span data-ttu-id="8ad58-159">対象の型がデリゲート型の場合、ラムダメソッドは、適切な型のデリゲートコンストラクション式の引数として解釈されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-159">If the target type is a delegate type, the lambda method is interpreted as the argument to a delegate-construction expression of the appropriate type.</span></span>
    
  2. <span data-ttu-id="8ad58-160">対象の型が `System.Linq.Expressions.Expression(Of T)` で `T` がデリゲート型である場合、ラムダメソッドは @no__t 2 のデリゲート型構築式で使用されているかのように解釈され、式ツリーに変換されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-160">If the target type is `System.Linq.Expressions.Expression(Of T)`, and `T` is a delegate type, then the lambda method is interpreted as if it was being used in delegate-construction expression for `T` and then converted to an expression tree.</span></span>
    
  <span data-ttu-id="8ad58-161">デリゲートに ByRef パラメーターがない場合、非同期または反復子のラムダメソッドは、デリゲートの構築式の引数としてのみ解釈できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-161">An async or iterator lambda method may only be interpreted as the argument to a delegate-construction expression, if the delegate has no ByRef parameters.</span></span>
    
  <span data-ttu-id="8ad58-162">デリゲートのいずれかのパラメーター型から対応するラムダパラメーターの型への変換が縮小変換である場合、再分類は縮小として判断されます。それ以外の場合は、拡大されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-162">If conversion from any of the delegate's parameter types to the corresponding lambda parameter types is a narrowing conversion, then the reclassification is judged as narrowing; otherwise it is widening.</span></span>
    
  <span data-ttu-id="8ad58-163">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="8ad58-163">__Note.__</span></span> <span data-ttu-id="8ad58-164">ラムダメソッドと式ツリー間の正確な変換は、コンパイラのバージョン間では修正されない場合があり、この仕様の範囲を超えています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-164">The exact translation between lambda methods and expression trees may not be fixed between versions of the compiler and is beyond the scope of this specification.</span></span> <span data-ttu-id="8ad58-165">Microsoft Visual Basic 11.0 では、次の制限に従って、すべてのラムダ式を式ツリーに変換できます。(1) 1。</span><span class="sxs-lookup"><span data-stu-id="8ad58-165">For Microsoft Visual Basic 11.0, all lambda expressions may be converted to expression trees subject to the following restrictions: (1) 1.</span></span>  <span data-ttu-id="8ad58-166">式ツリーに変換できるのは、ByRef パラメーターを持たない単一行のラムダ式だけです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-166">Only single-line lambda expressions without ByRef parameters may be converted to expression trees.</span></span> <span data-ttu-id="8ad58-167">単一行の @no__t 0 のラムダの場合、式ツリーに変換できるのは呼び出しステートメントだけです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-167">Of the single-line `Sub` lambdas, only invocation statements may be converted to expression trees.</span></span> <span data-ttu-id="8ad58-168">(2) 前のフィールド初期化子を使用して後続のフィールド初期化子を初期化する場合、匿名型式を式ツリーに変換することはできません (`New With {.a=1, .b=.a}` など)。</span><span class="sxs-lookup"><span data-stu-id="8ad58-168">(2) Anonymous type expressions cannot be converted to expression trees if an earlier field initializer is used to initialize a subsequent field initializer, e.g. `New With {.a=1, .b=.a}`.</span></span> <span data-ttu-id="8ad58-169">(3) 初期化中の現在のオブジェクトのメンバーがフィールド初期化子の1つ (`New C1 With {.a=1, .b=.Method1()}` など) で使用されている場合、オブジェクト初期化子式を式ツリーに変換することはできません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-169">(3) Object initializer expressions cannot be converted to expression trees if a member of the current object being initialized is used in one of the field initializers, e.g. `New C1 With {.a=1, .b=.Method1()}`.</span></span> <span data-ttu-id="8ad58-170">(4) 多次元配列作成式は、要素の型を明示的に宣言している場合にのみ、式ツリーに変換できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-170">(4) Multi-dimensional array creation expressions can only be converted to expression trees if they declare their element type explicitly.</span></span> <span data-ttu-id="8ad58-171">(5) 遅延バインディング式を式ツリーに変換することはできません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-171">(5) Late-binding expressions cannot be converted to expression trees.</span></span> <span data-ttu-id="8ad58-172">(6) 変数またはフィールドを呼び出し式に渡すが、ByRef パラメーターとまったく同じ型がない場合、またはプロパティが byref で渡される場合、通常の VB セマンティクスとして、引数のコピーが ByRef で渡され、その最終的な値がコピーされます。 変数、フィールド、またはプロパティに戻ります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-172">(6) When a variable or field is passed ByRef to an invocation expression but does not have exactly the same type as the ByRef parameter, or when a property is passed ByRef, normal VB semantics are that a copy of the argument is passed ByRef and its final value is then copied back into the variable or field or property.</span></span> <span data-ttu-id="8ad58-173">式ツリーでは、コピーバックは行われません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-173">In expression trees, the copy-back does not happen.</span></span> <span data-ttu-id="8ad58-174">(7) これらすべての制限は、入れ子になったラムダ式にも適用されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-174">(7) All these restrictions apply to nested lambda expressions as well.</span></span>
    
  <span data-ttu-id="8ad58-175">対象の型が不明な場合、ラムダメソッドは、ラムダメソッドの同じシグネチャを持つ匿名デリゲート型のデリゲートインスタンス化式への引数として解釈されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-175">If the target type is not known, then the lambda method is interpreted as the argument to a delegate instantiation expression of an anonymous delegate type with the same signature of the lambda method.</span></span> <span data-ttu-id="8ad58-176">厳密なセマンティクスが使用されていて、いずれかのパラメーターの型が省略されている場合、コンパイル時エラーが発生します。それ以外の場合は、不足しているパラメーターの型に対して `Object` が置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-176">If strict semantics are being used and the type of any of the parameters are omitted, a compile-time error occurs; otherwise, `Object` is substituted for any missing parameter type.</span></span> <span data-ttu-id="8ad58-177">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-177">For example:</span></span>
    
  ```vb
  Module Test
      Sub Main()
          ' Type of x will be equivalent to Func(Of Object, Object, Object)
          Dim x = Function(a, b) a + b
  
          ' Type of y will be equivalent to Action(Of Object, Object)
          Dim y = Sub(a, b) Console.WriteLine(a + b)
      End Sub
  End Module
  ```

* <span data-ttu-id="8ad58-178">プロパティグループは、プロパティアクセスとして再分類できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-178">A property group can be reclassified as a property access.</span></span> <span data-ttu-id="8ad58-179">プロパティグループ式は、空のかっこを含むインデックス式として解釈されます (つまり、@no__t 0 は `f()` と解釈されます)。</span><span class="sxs-lookup"><span data-stu-id="8ad58-179">The property group expression is interpreted as an index expression with empty parentheses (that is, `f` is interpreted as `f()`).</span></span>

* <span data-ttu-id="8ad58-180">プロパティ アクセスは、値として再分類できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-180">A property access can be reclassified as a value.</span></span> <span data-ttu-id="8ad58-181">プロパティアクセス式は、プロパティの @no__t 0 アクセサーの呼び出し式として解釈されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-181">The property access expression is interpreted as an invocation expression of the `Get` accessor of the property.</span></span> <span data-ttu-id="8ad58-182">プロパティに getter がない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-182">If the property has no getter, then a compile-time error occurs.</span></span>

* <span data-ttu-id="8ad58-183">遅延バインディングアクセスは、遅延バインディングされたメソッドまたは遅延バインディングプロパティアクセスとして再分類できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-183">A late-bound access can be reclassified as a late-bound method or late-bound property access.</span></span> <span data-ttu-id="8ad58-184">遅延バインディングアクセスをメソッドアクセスとして、またプロパティアクセスとして再分類できる状況では、プロパティアクセスへの再分類が推奨されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-184">In a situation where a late-bound access can be reclassified both as a method access and as a property access, reclassification to a property access is preferred.</span></span>

* <span data-ttu-id="8ad58-185">遅延バインディングされたアクセスは、値として再分類できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-185">A late-bound access can be reclassified as a value.</span></span>

* <span data-ttu-id="8ad58-186">配列リテラルは、値として再分類できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-186">An array literal can be reclassified as a value.</span></span> <span data-ttu-id="8ad58-187">値の型は次のように決定されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-187">The type of the value is determined as follows:</span></span>

  1. <span data-ttu-id="8ad58-188">対象の型が認識され、ターゲットの型が配列型である変換のコンテキストで再分類が行われた場合、配列リテラルは T () 型の値として再分類されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-188">If the reclassification occurs in the context of a conversion where the target type is known and the target type is an array type, then the array literal is reclassified as a value of type T().</span></span> <span data-ttu-id="8ad58-189">ターゲットの型が `System.Collections.Generic.IList(Of T)`、`IReadOnlyList(Of T)`、`ICollection(Of T)`、`IReadOnlyCollection(Of T)`、または `IEnumerable(Of T)` で、配列リテラルに1つの入れ子レベルがある場合、配列リテラルは `T()` 型の値として再分類されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-189">If the target type is `System.Collections.Generic.IList(Of T)`, `IReadOnlyList(Of T)`, `ICollection(Of T)`, `IReadOnlyCollection(Of T)`, or `IEnumerable(Of T)`, and the array literal has one level of nesting, then the array literal is reclassified as a value of type `T()`.</span></span>

  2. <span data-ttu-id="8ad58-190">それ以外の場合、配列リテラルは、入れ子のレベルと等しいランクの配列である型を持つ値に再分類されます。要素型は、初期化子の要素の中で最も優先度の高い型によって決定されます。優先される型を特定できない場合は、`Object` が使用されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-190">Otherwise, the array literal is reclassified to a value whose type is an array of rank equal to the level of nesting is used, with element type determined by the dominant type of the elements in the initializer; if no dominant type can be determined, `Object` is used.</span></span> <span data-ttu-id="8ad58-191">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-191">For example:</span></span>

     ```vb
     ' x Is GetType(Double(,,))
     Dim x = { { { 1, 2.0 }, { 3, 4 } }, { { 5, 6 }, { 7, 8 } } }.GetType()
        
     ' y Is GetType(Integer())
     Dim y = { 1, 2, 3 }.GetType()
        
     ' z Is GetType(Object())
     Dim z = { 1, "2" }.GetType()
        
     ' Error: Inconsistent nesting
     Dim a = { { 10 }, { 20, 30 } }.GetType()
     ```

  <span data-ttu-id="8ad58-192">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="8ad58-192">__Note.__</span></span> <span data-ttu-id="8ad58-193">バージョン9.0 とバージョン10.0 の言語の動作が若干変更されています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-193">There is a slight change in behavior between version 9.0 and version 10.0 of the language.</span></span> <span data-ttu-id="8ad58-194">10.0 より前では、配列要素初期化子はローカル変数型の推定に影響を与えませんでした。</span><span class="sxs-lookup"><span data-stu-id="8ad58-194">Prior to 10.0, array element initializers did not affect local variable type inference and now they do.</span></span> <span data-ttu-id="8ad58-195">そのため `Dim a() = { 1, 2, 3 }` は、言語のバージョン9.0 で `a` の型として `Object()` を推定し、バージョン10.0 では `Integer()` になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-195">So `Dim a() = { 1, 2, 3 }` would have inferred `Object()` as the type of `a` in version 9.0 of the language and `Integer()` in version 10.0.</span></span>

  <span data-ttu-id="8ad58-196">その後、再分類は配列作成式として配列リテラルを再解釈します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-196">The reclassification then reinterprets the array literal as an array-creation expression.</span></span> <span data-ttu-id="8ad58-197">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-197">So the examples:</span></span>

  ```vb
  Dim x As Double = { 1, 2, 3, 4 }
  Dim y = { "a", "b" }
  ```

  <span data-ttu-id="8ad58-198">は次と同じです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-198">are equivalent to:</span></span>

  ```vb
  Dim x As Double = New Double() { 1, 2, 3, 4 }
  Dim y = New String() { "a", "b" }
  ```

  <span data-ttu-id="8ad58-199">要素式から配列要素型への変換が縮小されている場合、再分類は縮小として判断されます。それ以外の場合は、拡大として判断されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-199">The reclassification is judged as narrowing if any conversion from an element expression to the array element type is narrowing; otherwise it is judged as widening.</span></span>

* <span data-ttu-id="8ad58-200">既定値 `Nothing` は、値として再分類できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-200">The default value `Nothing` can be reclassified as a value.</span></span> <span data-ttu-id="8ad58-201">ターゲット型がわかっているコンテキストでは、結果は対象の型の既定値になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-201">In a context where the target type is known, the result is the default value of the target type.</span></span> <span data-ttu-id="8ad58-202">対象の型が不明なコンテキストでは、結果は `Object` 型の null 値になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-202">In a context where the target type is not known, the result is a null value of type `Object`.</span></span>

<span data-ttu-id="8ad58-203">名前空間式、型式、イベントアクセス式、または void 式を再分類することはできません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-203">A namespace expression, type expression, event access expression, or void expression cannot be reclassified.</span></span> <span data-ttu-id="8ad58-204">複数の再分類を同時に実行できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-204">Multiple reclassifications can be done at the same time.</span></span> <span data-ttu-id="8ad58-205">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-205">For example:</span></span>

```vb
Module Test
    Sub F(i As Integer)
    End Sub

    ReadOnly Property P() As Integer
        Get
        End Get
    End Sub

    Sub Main()
        F(P)
    End Property
End Module
```

<span data-ttu-id="8ad58-206">この場合、プロパティグループ式 `P` は、まずプロパティグループからプロパティアクセスに再分類し、次にプロパティアクセスから値に再分類します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-206">In this case, the property group expression `P` is first reclassified from a property group to a property access and then reclassified from a property access to a value.</span></span> <span data-ttu-id="8ad58-207">コンテキスト内の有効な分類に至るまで、最も少ない再分類の数が実行されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-207">The fewest number of reclassifications are performed to reach a valid classification in the context.</span></span>

## <a name="constant-expressions"></a><span data-ttu-id="8ad58-208">定数式</span><span class="sxs-lookup"><span data-stu-id="8ad58-208">Constant Expressions</span></span>

<span data-ttu-id="8ad58-209">*定数式*は、コンパイル時に値を完全に評価できる式です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-209">A *constant expression* is an expression whose value can be fully evaluated at compile time.</span></span>

```antlr
ConstantExpression
    : Expression
    ;
```

<span data-ttu-id="8ad58-210">定数式の型は、0、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`、`Char`、`Single`、3、4、5、または任意の列挙型に @no__t することができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-210">The type of a constant expression can be `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Char`, `Single`, `Double`, `Decimal`, `Date`, `Boolean`, `String`, `Object`, or any enumeration type.</span></span> <span data-ttu-id="8ad58-211">定数式では、次の構造体を使用できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-211">The following constructs are permitted in constant expressions:</span></span>

* <span data-ttu-id="8ad58-212">リテラル (@no__t 0 を含む)。</span><span class="sxs-lookup"><span data-stu-id="8ad58-212">Literals (including `Nothing`).</span></span>

* <span data-ttu-id="8ad58-213">定数型のメンバーまたは定数のローカル変数への参照。</span><span class="sxs-lookup"><span data-stu-id="8ad58-213">References to constant type members or constant locals.</span></span>

* <span data-ttu-id="8ad58-214">列挙型のメンバーへの参照。</span><span class="sxs-lookup"><span data-stu-id="8ad58-214">References to members of enumeration types.</span></span>

* <span data-ttu-id="8ad58-215">かっこで囲まれた部分式。</span><span class="sxs-lookup"><span data-stu-id="8ad58-215">Parenthesized subexpressions.</span></span>

* <span data-ttu-id="8ad58-216">変換式は、対象の型が上記のいずれかの型である場合に指定します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-216">Coercion expressions, provided the target type is one of the types listed above.</span></span> <span data-ttu-id="8ad58-217">@No__t-0 との間の強制変換はこの規則の例外であり、`String` の変換は実行時に実行環境の現在のカルチャで常に実行されるため、null 値でのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-217">Coercions to and from `String` are an exception to this rule and are only allowed on null values because `String` conversions are always done in the current culture of the execution environment at run time.</span></span> <span data-ttu-id="8ad58-218">定数強制変換式では、組み込み変換のみを使用できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-218">Note that constant coercion expressions can only ever use intrinsic conversions.</span></span>

* <span data-ttu-id="8ad58-219">@No__t-0、`-`、`Not` の単項演算子は、オペランドと結果が上記の型になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-219">The `+`, `-` and `Not` unary operators, provided the operand and result is of a type listed above.</span></span>

* <span data-ttu-id="8ad58-220">@No__t-0、`-`、`*`、`^`、`Mod`、`/`、`\`、`<<`、`>>`、`&`、@no__t、1、2、3、4、5、6、7、8、9、0 の2項演算子各オペランドと結果は、上に示した型になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-220">The `+`, `-`, `*`, `^`, `Mod`, `/`, `\`, `<<`, `>>`, `&`, `And`, `Or`, `Xor`, `AndAlso`, `OrElse`, `=`, `<`, `>`, `<>`, `<=`, and `=>` binary operators, provided each operand and result is of a type listed above.</span></span>

* <span data-ttu-id="8ad58-221">条件演算子は、各オペランドと結果が、上に示した型である場合に指定します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-221">The conditional operator If, provided each operand and result is of a type listed above.</span></span>

* <span data-ttu-id="8ad58-222">次の実行時関数: `Microsoft.VisualBasic.Strings.ChrW`;定数値が 0 ~ 128 の場合は-1 を @no__t します。定数文字列が空でない場合は、`Microsoft.VisualBasic.Strings.AscW`定数文字列が空でない場合は、-3 を @no__t ます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-222">The following run-time functions: `Microsoft.VisualBasic.Strings.ChrW`; `Microsoft.VisualBasic.Strings.Chr` if the constant value is between 0 and 128; `Microsoft.VisualBasic.Strings.AscW` if the constant string is not empty; `Microsoft.VisualBasic.Strings.Asc` if the constant string is not empty.</span></span>

<span data-ttu-id="8ad58-223">定数式では、次のコンストラクトは使用でき*ません*。</span><span class="sxs-lookup"><span data-stu-id="8ad58-223">The following constructs are *not* permitted in constant expressions:</span></span>

* <span data-ttu-id="8ad58-224">@No__t 0 コンテキストを使用した暗黙的なバインド。</span><span class="sxs-lookup"><span data-stu-id="8ad58-224">Implicit binding through a `With` context.</span></span>

<span data-ttu-id="8ad58-225">整数型の定数式 (`ULong`、`Long`、`UInteger`、`Integer`、`UShort`、`Short`、`SByte`、または @no__t) は、より狭い整数型に暗黙的に変換できます。また、型 @no__t の定数式を暗黙的にに変換することもでき `Single`。定数式の値が変換先の型の範囲内にあることを示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-225">Constant expressions of an integral type (`ULong`, `Long`, `UInteger`, `Integer`, `UShort`, `Short`, `SByte`, or `Byte`) can be implicitly converted to a narrower integral type, and constant expressions of type `Double` can be implicitly converted to `Single`, provided the value of the constant expression is within the range of the destination type.</span></span> <span data-ttu-id="8ad58-226">これらの縮小変換は、寛容または厳格なセマンティクスが使用されているかどうかにかかわらず許可されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-226">These narrowing conversions are allowed regardless of whether permissive or strict semantics are being used.</span></span>


## <a name="late-bound-expressions"></a><span data-ttu-id="8ad58-227">遅延バインディング式</span><span class="sxs-lookup"><span data-stu-id="8ad58-227">Late-Bound Expressions</span></span>

<span data-ttu-id="8ad58-228">メンバーアクセス式またはインデックス式の対象が型 `Object` の場合、実行時まで式の処理が遅延される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-228">When the target of a member access expression or index expression is of type `Object`, the processing of the expression may be deferred until run time.</span></span> <span data-ttu-id="8ad58-229">このように処理を延期すると、*遅延バインディング*と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-229">Deferring processing this way is called *late binding*.</span></span> <span data-ttu-id="8ad58-230">遅延バインディングでは、@no__t 0 の変数を型指定されてい*ない方法で*使用できます。この場合、メンバーのすべての解決は、変数の値の実際の実行時の型に基づいています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-230">Late binding allows `Object` variables to be used in a *typeless* way, where all resolution of members is based on the actual run-time type of the value in the variable.</span></span> <span data-ttu-id="8ad58-231">厳密なセマンティクスがコンパイル環境で指定される場合、または @no__t 0 によって指定される場合は、遅延バインディングによってコンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-231">If strict semantics are specified by the compilation environment or by `Option Strict`, late binding causes a compile-time error.</span></span> <span data-ttu-id="8ad58-232">非パブリックメンバーは、オーバーロードの解決のために、遅延バインディングを行うときに無視されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-232">Non-public members are ignored when doing late-binding, including for the purposes of overload resolution.</span></span> <span data-ttu-id="8ad58-233">事前バインディングされたケースとは異なり、`Shared` メンバーの遅延バインディングを呼び出したり、アクセスしたりすると、実行時に呼び出しターゲットが評価されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8ad58-233">Note that, unlike the early-bound case, invoking or accessing a `Shared` member late-bound will cause the invocation target to be evaluated at run time.</span></span><span data-ttu-id="8ad58-234"> 式が `System.Object` で定義されたメンバーの呼び出し式である場合、遅延バインディングは行われません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-234"> If the expression is an invocation expression for a member defined on `System.Object`, late binding will not take place.</span></span>

<span data-ttu-id="8ad58-235">一般に、遅延バインディングのアクセスは、式の実際の実行時の型で識別子を検索することによって、実行時に解決されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-235">In general, late-bound accesses are resolved at run time by looking up the identifier on the actual run-time type of the expression.</span></span> <span data-ttu-id="8ad58-236">実行時に遅延バインディングメンバー参照が失敗した場合は、@no__t 0 の例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-236">If late-bound member lookup fails at run time, a `System.MissingMemberException` exception is thrown.</span></span> <span data-ttu-id="8ad58-237">遅延バインディングされたメンバーの参照は、関連付けられているターゲット式の実行時の型からのみ行われるため、オブジェクトの実行時の型はインターフェイスではありません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-237">Because late-bound member lookup is done solely off the run-time type of the associated target expression, an object's run-time type is never an interface.</span></span> <span data-ttu-id="8ad58-238">したがって、遅延バインディングされたメンバーアクセス式でインターフェイスメンバーにアクセスすることはできません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-238">Therefore, it is impossible to access interface members in a late-bound member access expression.</span></span>

<span data-ttu-id="8ad58-239">遅延バインディングメンバーアクセスの引数は、メンバーアクセス式に出現する順序で評価されます。これは、遅延バインディングされたメンバーでパラメーターが宣言されている順序ではありません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-239">The arguments to a late-bound member access are evaluated in the order they appear in the member access expression: not the order in which parameters are declared in the late-bound member.</span></span> <span data-ttu-id="8ad58-240">次の例は、この違いを示しています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-240">The following example illustrates this difference:</span></span>

```vb
Class C
    Public Sub f(ByVal x As Integer, ByVal y As Integer)
    End Sub
End Class

Module Module1
    Sub Main()
        Console.Write("Early-bound: ")
        Dim c As C = New C
        c.f(y:=t("y"), x:=t("x"))

        Console.Write(vbCrLf & "Late-bound: ")
        Dim o As Object = New C
        o.f(y:=t("y"), x:=t("x"))
    End Sub

    Function t(ByVal s As String) As Integer
        Console.Write(s)
        Return 0
    End Function
End Module
```

<span data-ttu-id="8ad58-241">次のコードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-241">This code displays:</span></span>

```console
Early-bound: xy
Late-bound: yx
```

<span data-ttu-id="8ad58-242">遅延バインディングされたオーバーロードの解決は、引数の実行時の型に対して行われるため、コンパイル時または実行時のどちらで評価されるかに基づいて、式が異なる結果を生成する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-242">Because late-bound overload resolution is done on the run-time type of the arguments, it is possible that an expression might produce different results based on whether it is evaluated at compile time or run time.</span></span> <span data-ttu-id="8ad58-243">次の例は、この違いを示しています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-243">The following example illustrates this difference:</span></span>

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class

Module Test
    Sub F(b As Base)
        Console.WriteLine("F(Base)")
    End Sub

    Sub F(d As Derived)
        Console.WriteLine("F(Derived)")
    End Sub

    Sub Main()
        Dim b As Base = New Derived()
        Dim o As Object = b

        F(b)
        F(o)
    End Sub
End Module
```

<span data-ttu-id="8ad58-244">次のコードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-244">This code displays:</span></span>

```console
F(Base)
F(Derived)
```

## <a name="simple-expressions"></a><span data-ttu-id="8ad58-245">単純式</span><span class="sxs-lookup"><span data-stu-id="8ad58-245">Simple Expressions</span></span>

<span data-ttu-id="8ad58-246">単純式は、リテラル、かっこで囲まれた式、インスタンス式、または単純な名前式です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-246">Simple expressions are literals, parenthesized expressions, instance expressions, or simple name expressions.</span></span>

```antlr
SimpleExpression
    : LiteralExpression
    | ParenthesizedExpression
    | InstanceExpression
    | SimpleNameExpression
    | AddressOfExpression
    ;
```

### <a name="literal-expressions"></a><span data-ttu-id="8ad58-247">リテラル式</span><span class="sxs-lookup"><span data-stu-id="8ad58-247">Literal Expressions</span></span>

<span data-ttu-id="8ad58-248">リテラル式は、リテラルによって表される値に評価されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-248">Literal expressions evaluate to the value represented by the literal.</span></span> <span data-ttu-id="8ad58-249">リテラル式は、値として分類されます。ただし、リテラル `Nothing` は既定値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-249">A literal expression is classified as a value, except for the literal `Nothing`, which is classified as a default value.</span></span>

```antlr
LiteralExpression
    : Literal
    ;
```

### <a name="parenthesized-expressions"></a><span data-ttu-id="8ad58-250">かっこで囲まれる式</span><span class="sxs-lookup"><span data-stu-id="8ad58-250">Parenthesized Expressions</span></span>

<span data-ttu-id="8ad58-251">かっこで囲まれた式は、かっこで囲まれた式で構成されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-251">A parenthesized expression consists of an expression enclosed in parentheses.</span></span> <span data-ttu-id="8ad58-252">かっこで囲まれた式は値として分類され、囲まれた式は値として分類される必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-252">A parenthesized expression is classified as a value, and the enclosed expression must be classified as a value.</span></span> <span data-ttu-id="8ad58-253">かっこで囲まれた式は、かっこ内の式の値に評価されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-253">A parenthesized expression evaluates to the value of the expression within the parentheses.</span></span>

```antlr
ParenthesizedExpression
    : OpenParenthesis Expression CloseParenthesis
    ;
```

### <a name="instance-expressions"></a><span data-ttu-id="8ad58-254">インスタンス式</span><span class="sxs-lookup"><span data-stu-id="8ad58-254">Instance Expressions</span></span>

<span data-ttu-id="8ad58-255">*インスタンス式*は、キーワード `Me` です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-255">An *instance expression* is the keyword `Me`.</span></span> <span data-ttu-id="8ad58-256">これは、共有されていないメソッド、コンストラクター、またはプロパティアクセサーの本体内でのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-256">It may only be used within the body of a non-shared method, constructor, or property accessor.</span></span> <span data-ttu-id="8ad58-257">値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-257">It is classified as a value.</span></span> <span data-ttu-id="8ad58-258">キーワード `Me` は、実行されるメソッドまたはプロパティアクセサーを格納している型のインスタンスを表します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-258">The keyword `Me` represents the instance of the type containing the method or property accessor being executed.</span></span> <span data-ttu-id="8ad58-259">コンストラクターが別のコンストラクター (セクション[コンストラクター](type-members.md#constructors)) を明示的に呼び出す場合、そのコンストラクターが呼び出されるまでは、インスタンスがまだ構築されていないため、`Me` を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-259">If a constructor explicitly invokes another constructor (Section [Constructors](type-members.md#constructors)), `Me` cannot be used until after that constructor call, because the instance has not yet been constructed.</span></span>

```antlr
InstanceExpression
    : 'Me'
    ;
```

### <a name="simple-name-expressions"></a><span data-ttu-id="8ad58-260">単純な名前の式</span><span class="sxs-lookup"><span data-stu-id="8ad58-260">Simple Name Expressions</span></span>

<span data-ttu-id="8ad58-261">*単純な名前の式*は、単一の識別子の後に省略可能な型引数リストを指定して構成されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-261">A *simple name expression* consists of a single identifier followed by an optional type argument list.</span></span>

```antlr
SimpleNameExpression
    : Identifier ( OpenParenthesis 'Of' TypeArgumentList CloseParenthesis )?
    ;
```

<span data-ttu-id="8ad58-262">名前は解決され、次の "単純な名前解決規則" によって分類されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-262">The name is resolved and classified by the following "simple name resolution rules":</span></span>

1.  <span data-ttu-id="8ad58-263">すぐ外側のブロックから開始し、外側の外側の各ブロック (存在する場合) を続けて、識別子がローカル変数、静的変数、定数のローカル、メソッドの型パラメーター、またはパラメーターの名前と一致する場合、識別子はを参照します。一致するエンティティ。</span><span class="sxs-lookup"><span data-stu-id="8ad58-263">Starting with the immediately enclosing block and continuing with each enclosing outer block (if any), if the identifier matches the name of a local variable, static variable, constant local, method type parameter, or parameter, then the identifier refers to the matching entity.</span></span>

    <span data-ttu-id="8ad58-264">識別子がローカル変数、静的変数、または定数 local に一致し、型引数リストが指定されている場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-264">If the identifier matches a local variable, static variable, or constant local and a type argument list was provided, a compile-time error occurs.</span></span> <span data-ttu-id="8ad58-265">識別子がメソッド型パラメーターと一致し、型引数リストが指定されている場合、一致は発生せず、解決は続行されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-265">If the identifier matches a method type parameter and a type argument list was provided, no match occurs and resolution continues.</span></span> <span data-ttu-id="8ad58-266">識別子がローカル変数と一致する場合、一致したローカル変数は暗黙的な関数または `Get` アクセサーはローカル変数を返し、式は呼び出し式、呼び出しステートメント、または `AddressOf` 式の一部になり、次に一致しません。が発生し、解決が続行されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-266">If the identifier matches a local variable, the local variable matched is the implicit function or `Get` accessor return local variable, and the expression is part of an invocation expression, invocation statement, or an `AddressOf` expression, then no match occurs and resolution continues.</span></span>

    <span data-ttu-id="8ad58-267">式は、ローカル変数、静的変数、またはパラメーターである場合、変数として分類されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-267">The expression is classified as a variable if it is a local variable, static variable, or parameter.</span></span> <span data-ttu-id="8ad58-268">式は、メソッド型パラメーターの場合、型として分類されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-268">The expression is classified as a type if it is a method type parameter.</span></span> <span data-ttu-id="8ad58-269">定数がローカルである場合、式は値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-269">The expression is classified as a value if it is a constant local.</span></span>

2.  <span data-ttu-id="8ad58-270">式を含む入れ子にされた型ごとに、最も内側から外側に向かっています。型の識別子を参照すると、アクセス可能なメンバーとの一致が生成されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-270">For each nested type containing the expression, starting from the innermost and going to the outermost, if a lookup of the identifier in the type produces a match with an accessible member:</span></span>

    21. <span data-ttu-id="8ad58-271">一致する型のメンバーが型パラメーターの場合、結果は型として分類され、対応する型パラメーターになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-271">If the matching type member is a type parameter, then the result is classified as a type and is the matching type parameter.</span></span> <span data-ttu-id="8ad58-272">型引数リストが指定された場合、一致は発生せず、解決は続行されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-272">If a type argument list was provided, no match occurs and resolution continues.</span></span>
    22. <span data-ttu-id="8ad58-273">それ以外の場合、型がすぐ外側の型であり、参照によって共有されていない型のメンバーが識別されると、結果は `Me.E(Of A)` という形式のメンバーアクセスと同じになります。ここで `E` は識別子、`A` は型引数リストです。(存在する場合)。</span><span class="sxs-lookup"><span data-stu-id="8ad58-273">Otherwise, if the type is the immediately enclosing type and the lookup identifies a non-shared type member, then the result is the same as a member access of the form `Me.E(Of A)`, where `E` is the identifier and `A` is the type argument list, if any.</span></span>
    23. <span data-ttu-id="8ad58-274">それ以外の場合、結果は `T.E(Of A)` という形式のメンバーアクセスとまったく同じです。ここで `T` は一致するメンバーを含む型、`E` は @no__t 識別子、は型引数リスト (存在する場合) です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-274">Otherwise, the result is exactly the same as a member access of the form `T.E(Of A)`, where `T` is the type containing the matching member, `E` is the identifier, and `A` is the type argument list, if any.</span></span> <span data-ttu-id="8ad58-275">この場合、識別子が非共有メンバーを参照しているとエラーになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-275">In this case, it is an error for the identifier to refer to a non-shared member.</span></span>

3.  <span data-ttu-id="8ad58-276">入れ子になった名前空間ごとに、最も内側から最も外側の名前空間に移動し、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-276">For each nested namespace, starting from the innermost and going to the outermost namespace, do the following:</span></span>

    31. <span data-ttu-id="8ad58-277">名前空間に、指定した名前のアクセス可能な型が含まれていて、型引数リストで指定されたものと同じ数の型パラメーターを持っている場合、識別子はその型を参照し、型として分類されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-277">If the namespace contains an accessible type with the given name and has the same number of type parameters as was supplied in the type argument list, if any, then the identifier refers to that type and is classified as a type.</span></span>
    32. <span data-ttu-id="8ad58-278">それ以外の場合、型引数リストが指定されておらず、名前空間に指定された名前の名前空間メンバーが含まれている場合、識別子はその名前空間を参照し、名前空間として分類されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-278">Otherwise, if no type argument list was supplied and the namespace contains a namespace member with the given name, then the identifier refers to that namespace and is classified as a namespace.</span></span>
    33. <span data-ttu-id="8ad58-279">それ以外の場合、名前空間にアクセス可能な標準モジュールが1つ以上含まれており、その識別子のメンバー名の参照によって1つの標準モジュールでアクセス可能な一致が生成されると、結果は `M.E(Of A)` の形式のメンバーアクセスとまったく同じになり @no__t は、対応するメンバーが含まれている標準モジュールで、@no__t 2 は識別子、`A` は型引数リスト (存在する場合) です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-279">Otherwise, if the namespace contains one or more accessible standard modules, and a member name lookup of the identifier produces an accessible match in exactly one standard module, then the result is exactly the same as a member access of the form `M.E(Of A)`, where `M` is the standard module containing the matching member, `E` is the identifier, and `A` is the type argument list, if any.</span></span> <span data-ttu-id="8ad58-280">識別子が複数の標準モジュールのアクセス可能な型メンバーと一致する場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-280">If the identifier matches accessible type members in more than one standard module, a compile-time error occurs.</span></span>

4.  <span data-ttu-id="8ad58-281">ソースファイルに1つ以上のインポートエイリアスがあり、識別子がいずれか1つの名前と一致する場合、識別子はその名前空間または型を参照します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-281">If the source file has one or more import aliases, and the identifier matches the name of one of them, then the identifier refers to that namespace or type.</span></span> <span data-ttu-id="8ad58-282">型引数リストが指定されている場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-282">If a type argument list is supplied, a compile-time error occurs.</span></span>

5. <span data-ttu-id="8ad58-283">名前参照を含むソースファイルに1つ以上のインポートがある場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-283">If the source file containing the name reference has one or more imports:</span></span>

    51. <span data-ttu-id="8ad58-284">識別子が、型引数リストで指定されたものと同じ数の型パラメーターを持つアクセス可能な型の名前 (存在する場合) または型のメンバーである場合、識別子はその型または型のメンバーを参照します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-284">If the identifier matches in exactly one import the name of an accessible type with the same number of type parameters as was supplied in the type argument list, if any, or a type member, then the identifier refers to that type or type member.</span></span> <span data-ttu-id="8ad58-285">識別子が複数のインポートで一致する場合、型引数リストで指定された型パラメーターの数と同じ数の型パラメーターを持つアクセス可能な型の名前 (存在する場合)、またはアクセス可能な型のメンバーである場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-285">If the identifier matches in more than one import the name of an accessible type with the same number of type parameters as was supplied in the type argument list, if any, or an accessible type member, a compile-time error occurs.</span></span>
    52. <span data-ttu-id="8ad58-286">それ以外の場合、型引数リストが指定されておらず、識別子がアクセス可能な型の名前空間の名前を厳密に1つのインポートで一致する場合、識別子はその名前空間を参照します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-286">Otherwise, if no type argument list was supplied and the identifier matches in exactly one import the name of a namespace with accessible types, then the identifier refers to that namespace.</span></span> <span data-ttu-id="8ad58-287">型引数リストが指定されておらず、識別子がアクセス可能な型の名前空間の名前を複数のインポートで一致する場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-287">If no type argument list was supplied and the identifier matches in more than one import the name of a namespace with accessible types, a compile-time error occurs.</span></span>
    53. <span data-ttu-id="8ad58-288">それ以外の場合、インポートにアクセス可能な標準モジュールが1つ以上含まれており、その識別子のメンバー名参照によって1つの標準モジュールでアクセス可能な一致が生成された場合、結果は `M.E(Of A)` の形式のメンバーアクセスとまったく同じになり `M` は、一致するメンバーを含む標準モジュールであり、@no__t 2 は識別子、`A` は型引数リスト (存在する場合) です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-288">Otherwise, if the imports contain one or more accessible standard modules, and a member name lookup of the identifier produces an accessible match in exactly one standard module, then the result is exactly the same as a member access of the form `M.E(Of A)`, where `M` is the standard module containing the matching member, `E` is the identifier, and `A` is the type argument list, if any.</span></span> <span data-ttu-id="8ad58-289">識別子が複数の標準モジュールのアクセス可能な型メンバーと一致する場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-289">If the identifier matches accessible type members in more than one standard module, a compile-time error occurs.</span></span>

6.  <span data-ttu-id="8ad58-290">コンパイル環境で1つ以上のインポートエイリアスが定義されており、その識別子がいずれか1つの名前と一致する場合、識別子はその名前空間または型を参照します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-290">If the compilation environment defines one or more import aliases, and the identifier matches the name of one of them, then the identifier refers to that namespace or type.</span></span> <span data-ttu-id="8ad58-291">型引数リストが指定されている場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-291">If a type argument list is supplied, a compile-time error occurs.</span></span>

7. <span data-ttu-id="8ad58-292">コンパイル環境で1つ以上のインポートを定義する場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-292">If the compilation environment defines one or more imports:</span></span>

    71. <span data-ttu-id="8ad58-293">識別子が、型引数リストで指定されたものと同じ数の型パラメーターを持つアクセス可能な型の名前 (存在する場合) または型のメンバーである場合、識別子はその型または型のメンバーを参照します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-293">If the identifier matches in exactly one import the name of an accessible type with the same number of type parameters as was supplied in the type argument list, if any, or a type member, then the identifier refers to that type or type member.</span></span> <span data-ttu-id="8ad58-294">識別子が複数のインポートで一致する場合は、型引数リストで指定されたものと同じ数の型パラメーターを持つアクセス可能な型の名前 (存在する場合)、または型のメンバーである場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-294">If the identifier matches in more than one import the name of an accessible type with the same number of type parameters as was supplied in the type argument list, if any, or a type member, a compile-time error occurs.</span></span>
    72. <span data-ttu-id="8ad58-295">それ以外の場合、型引数リストが指定されておらず、識別子がアクセス可能な型の名前空間の名前を厳密に1つのインポートで一致する場合、識別子はその名前空間を参照します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-295">Otherwise, if no type argument list was supplied and the identifier matches in exactly one import the name of a namespace with accessible types, then the identifier refers to that namespace.</span></span> <span data-ttu-id="8ad58-296">型引数リストが指定されておらず、識別子がアクセス可能な型の名前空間の名前を複数のインポートで一致する場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-296">If no type argument list was supplied and the identifier matches in more than one import the name of a namespace with accessible types, a compile-time error occurs.</span></span>
    73. <span data-ttu-id="8ad58-297">それ以外の場合、インポートにアクセス可能な標準モジュールが1つ以上含まれており、その識別子のメンバー名参照によって1つの標準モジュールでアクセス可能な一致が生成された場合、結果は `M.E(Of A)` の形式のメンバーアクセスとまったく同じになり `M` は、一致するメンバーを含む標準モジュールであり、@no__t 2 は識別子、`A` は型引数リスト (存在する場合) です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-297">Otherwise, if the imports contain one or more accessible standard modules, and a member name lookup of the identifier produces an accessible match in exactly one standard module, then the result is exactly the same as a member access of the form `M.E(Of A)`, where `M` is the standard module containing the matching member,  `E` is the identifier, and `A` is the type argument list, if any.</span></span> <span data-ttu-id="8ad58-298">識別子が複数の標準モジュールのアクセス可能な型メンバーと一致する場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-298">If the identifier matches accessible type members in more than one standard module, a compile-time error occurs.</span></span>

8. <span data-ttu-id="8ad58-299">それ以外の場合、識別子によって指定された名前は定義されません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-299">Otherwise, the name given by the identifier is undefined.</span></span>

<span data-ttu-id="8ad58-300">未定義の単純な名前式は、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-300">A simple name expression that is undefined is a compile-time error.</span></span>

<span data-ttu-id="8ad58-301">通常、名前は特定の名前空間に1回だけ出現できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-301">Normally, a name can only occur once in a particular namespace.</span></span> <span data-ttu-id="8ad58-302">ただし、複数の .NET アセンブリにわたって名前空間を宣言できるため、2つのアセンブリが同じ完全修飾名を持つ型を定義する状況が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-302">However, because namespaces can be declared across multiple .NET assemblies, it is possible to have a situation where two assemblies define a type with the same fully qualified name.</span></span> <span data-ttu-id="8ad58-303">この場合、ソースファイルの現在のセットで宣言されている型は、外部の .NET アセンブリで宣言されている型よりも優先されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-303">In that case, a type declared in the current set of source files is preferred over a type declared in an external .NET assembly.</span></span> <span data-ttu-id="8ad58-304">それ以外の場合、名前はあいまいであり、名前を明確に区別する方法はありません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-304">Otherwise, the name is ambiguous and there is no way to disambiguate the name.</span></span>


### <a name="addressof-expressions"></a><span data-ttu-id="8ad58-305">AddressOf 式</span><span class="sxs-lookup"><span data-stu-id="8ad58-305">AddressOf Expressions</span></span>

<span data-ttu-id="8ad58-306">メソッドポインターを生成するには、@no__t 0 の式を使用します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-306">An `AddressOf` expression is used to produce a method pointer.</span></span> <span data-ttu-id="8ad58-307">式は、@no__t 0 キーワードと、メソッドグループまたは遅延バインディングアクセスとして分類する必要がある式で構成されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-307">The expression consists of the `AddressOf` keyword and an expression that must be classified as a method group or a late-bound access.</span></span> <span data-ttu-id="8ad58-308">メソッドグループはコンストラクターを参照できません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-308">The method group cannot refer to constructors.</span></span>

<span data-ttu-id="8ad58-309">結果はメソッドポインターとして分類され、メソッドグループと同じ関連するターゲット式と型引数リスト (存在する場合) があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-309">The result is classified as a method pointer, with the same associated target expression and type argument list (if any) as the method group.</span></span>

```antlr
AddressOfExpression
    : 'AddressOf' Expression
    ;
```

## <a name="type-expressions"></a><span data-ttu-id="8ad58-310">型の式</span><span class="sxs-lookup"><span data-stu-id="8ad58-310">Type Expressions</span></span>

<span data-ttu-id="8ad58-311">*型の式*は、@no__t 1 の式、@no__t 2 の式、`Is` の式、または `GetXmlNamespace` 式です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-311">A *type expression* is a `GetType` expression, a `TypeOf...Is` expression, an `Is` expression, or a `GetXmlNamespace` expression.</span></span>

```antlr
TypeExpression
    : GetTypeExpression
    | TypeOfIsExpression
    | IsExpression
    | GetXmlNamespaceExpression
    ;
```

### <a name="gettype-expressions"></a><span data-ttu-id="8ad58-312">GetType 式</span><span class="sxs-lookup"><span data-stu-id="8ad58-312">GetType Expressions</span></span>

<span data-ttu-id="8ad58-313">@No__t 0 の式は、キーワード `GetType`、型の名前で構成されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-313">A `GetType` expression consists of the keyword `GetType` and the name of a type.</span></span>

```antlr
GetTypeExpression
    : 'GetType' OpenParenthesis GetTypeTypeName CloseParenthesis
    ;

GetTypeTypeName
    : TypeName
    | QualifiedOpenTypeName
    ;

QualifiedOpenTypeName
    : Identifier TypeArityList? (Period IdentifierOrKeyword TypeArityList?)*
    | 'Global' Period IdentifierOrKeyword TypeArityList?
      (Period IdentifierOrKeyword TypeArityList?)*
    ;

TypeArityList
    : OpenParenthesis 'Of' CommaList? CloseParenthesis
    ;

CommaList
    : Comma Comma*
    ;
```

<span data-ttu-id="8ad58-314">@No__t 0 の式は値として分類され、その値は*Gettypetypename*を表すリフレクション (`System.Type`) クラスです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-314">A `GetType` expression is classified as a value, and its value is the reflection (`System.Type`) class that represents its *GetTypeTypeName*.</span></span> <span data-ttu-id="8ad58-315">*Gettypetypename*が型パラメーターの場合、式は、実行時に型パラメーターに指定された型引数に対応する `System.Type` オブジェクトを返します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-315">If the *GetTypeTypeName* is a type parameter, the expression will return the `System.Type` object that corresponds to the type argument supplied for the type parameter at run-time.</span></span>

<span data-ttu-id="8ad58-316">*Gettypetypename*は、次の2つの方法で特別です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-316">The *GetTypeTypeName* is special in two ways:</span></span>

* <span data-ttu-id="8ad58-317">この型名が参照される言語の唯一の場所である @no__t 0 にすることができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-317">It is allowed to be `System.Void`, the only place in the language where this type name may be referenced.</span></span>

* <span data-ttu-id="8ad58-318">型引数を省略して構築されたジェネリック型である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-318">It may be a constructed generic type with the type arguments omitted.</span></span> <span data-ttu-id="8ad58-319">これにより、`GetType` 式は、ジェネリック型自体に対応する @no__t 1 オブジェクトを返すことができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-319">This allows the `GetType` expression to return the `System.Type` object that corresponds to the generic type itself.</span></span>

<span data-ttu-id="8ad58-320">次の例は、@no__t 0 の式を示しています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-320">The following example demonstrates the `GetType` expression:</span></span>

```vb
Module Test
    Sub Main()
        Dim t As Type() = { GetType(Integer), GetType(System.Int32), _
            GetType(String), GetType(Double()) }
        Dim i As Integer

        For i = 0 To t.Length - 1
            Console.WriteLine(t(i).Name)
        Next i
    End Sub
End Module
```

<span data-ttu-id="8ad58-321">結果の出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-321">The resulting output is:</span></span>

```console
Int32
Int32
String
Double[]
```


### <a name="typeofis-expressions"></a><span data-ttu-id="8ad58-322">TypeOf...Is 式</span><span class="sxs-lookup"><span data-stu-id="8ad58-322">TypeOf...Is Expressions</span></span>

<span data-ttu-id="8ad58-323">値の実行時の型が特定の型と互換性があるかどうかを確認するには、`TypeOf...Is` 式を使用します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-323">A `TypeOf...Is` expression is used to check whether the run-time type of a value is compatible with a given type.</span></span> <span data-ttu-id="8ad58-324">最初のオペランドは、値として分類する必要があります。また、再分類するラムダメソッドにすることはできません。また、参照型または制約のない型パラメーター型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-324">The first operand must be classified as a value, cannot be a reclassified lambda method, and must be of a reference type or an unconstrained type parameter type.</span></span> <span data-ttu-id="8ad58-325">2番目のオペランドは型名である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-325">The second operand must be a type name.</span></span> <span data-ttu-id="8ad58-326">式の結果は値として分類され、@no__t 0 の値になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-326">The result of the expression is classified as a value and is a `Boolean` value.</span></span> <span data-ttu-id="8ad58-327">オペランドのランタイム型に id、既定値、参照、配列、値型、または型パラメーターから型への変換がある場合、式は `True` に評価されます。それ以外の場合は、`False` になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-327">The expression evaluates to `True` if the run-time type of the operand has an identity, default, reference, array, value type, or type parameter conversion to the type, `False` otherwise.</span></span> <span data-ttu-id="8ad58-328">式の型と特定の型の間に変換が存在しない場合、コンパイル時にエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-328">A compile-time error occurs if no conversion exists between the type of the expression and the specific type.</span></span>

```antlr
TypeOfIsExpression
    : 'TypeOf' Expression 'Is' LineTerminator? TypeName
    ;
```

### <a name="is-expressions"></a><span data-ttu-id="8ad58-329">Is 式</span><span class="sxs-lookup"><span data-stu-id="8ad58-329">Is Expressions</span></span>

<span data-ttu-id="8ad58-330">参照の等価性の比較を実行するには、@no__t 0 または `IsNot` 式を使用します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-330">An `Is` or `IsNot` expression is used to do a reference equality comparison.</span></span>

```antlr
IsExpression
    : Expression 'Is' LineTerminator? Expression
    | Expression 'IsNot' LineTerminator? Expression
    ;
```

<span data-ttu-id="8ad58-331">各式は値として分類する必要があり、各式の型は参照型、制約のない型パラメーター型、または null 許容値型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-331">Each expression must be classified as a value and the type of each expression must be a reference type, an unconstrained type parameter type, or a nullable value type.</span></span> <span data-ttu-id="8ad58-332">一方の式の型が制約のない型パラメーター型または null 許容値型である場合は、もう一方の式がリテラル `Nothing` である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-332">If the type of one expression is an unconstrained type parameter type or nullable value type, however, the other expression must be the literal `Nothing`.</span></span>

<span data-ttu-id="8ad58-333">結果は値として分類され、`Boolean` として型指定されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-333">The result is classified as a value and is typed as `Boolean`.</span></span> <span data-ttu-id="8ad58-334">両方の値が同じインスタンスを参照している場合、または両方の値が `Nothing` @no__t の場合、またはそれ以外の場合は、@no__t 0 演算が `True` と評価されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-334">An `Is` operation evaluates to `True` if both values refer to the same instance or both values are `Nothing`, or `False` otherwise.</span></span> <span data-ttu-id="8ad58-335">両方の値が同じインスタンスを参照している場合、または両方の値が `Nothing` @no__t の場合、またはそれ以外の場合は、@no__t 0 演算が `False` と評価されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-335">An `IsNot` operation evaluates to `False` if both values refer to the same instance or both values are `Nothing`, or `True` otherwise.</span></span>


### <a name="getxmlnamespace-expressions"></a><span data-ttu-id="8ad58-336">GetXmlNamespace 式</span><span class="sxs-lookup"><span data-stu-id="8ad58-336">GetXmlNamespace Expressions</span></span>

<span data-ttu-id="8ad58-337">@No__t 0 の式は、キーワード `GetXmlNamespace` と、ソースファイルまたはコンパイル環境によって宣言された XML 名前空間の名前で構成されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-337">A `GetXmlNamespace` expression consists of the keyword `GetXmlNamespace` and the name of an XML namespace declared by the source file or compilation environment.</span></span>

```antlr
GetXmlNamespaceExpression
    : 'GetXmlNamespace' OpenParenthesis XMLNamespaceName? CloseParenthesis
    ;
```

<span data-ttu-id="8ad58-338">@No__t 0 の式は値として分類され、その値は*XMLNamespaceName*を表す `System.Xml.Linq.XNamespace` のインスタンスになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-338">An `GetXmlNamespace` expression is classified as a value, and its value is an instance of `System.Xml.Linq.XNamespace` that represents the *XMLNamespaceName*.</span></span> <span data-ttu-id="8ad58-339">その型が使用できない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-339">If that type is not available, then a compile-time error will occur.</span></span>

<span data-ttu-id="8ad58-340">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-340">For example:</span></span>

```vb
Imports <xmlns:db="http://example.org/database">

Module Test
    Sub Main()
        Dim db = GetXmlNamespace(db)

        ' The following are equivalent
        Dim customer1 = _
            New System.Xml.Linq.XElement(db + "customer", "Bob")
        Dim customer2 = <db:customer>Bob</>
    End Sub
End Module
```

<span data-ttu-id="8ad58-341">かっこ内のすべての要素は名前空間名の一部と見なされるため、空白文字などの XML ルールが適用されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-341">Everything between the parentheses is considered part of the namespace name, so XML rules around things such as whitespace apply.</span></span> <span data-ttu-id="8ad58-342">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-342">For example:</span></span>

```vb
Imports <xmlns:db-ns="http://example.org/database">

Module Test
    Sub Main()

        ' Error, XML name expected
        Dim db1 = GetXmlNamespace( db-ns )

        ' Error, ')' expected
        Dim db2 = GetXmlNamespace(db _
            )

        ' OK.
        Dim db3 = GetXmlNamespace(db-ns)
    End Sub
End Module
```

<span data-ttu-id="8ad58-343">XML 名前空間式を省略することもできます。この場合、式は、既定の XML 名前空間を表すオブジェクトを返します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-343">The XML namespace expression can also be omitted, in which case the expression returns the object that represents the default XML namespace.</span></span>


## <a name="member-access-expressions"></a><span data-ttu-id="8ad58-344">メンバー アクセス式</span><span class="sxs-lookup"><span data-stu-id="8ad58-344">Member Access Expressions</span></span>

<span data-ttu-id="8ad58-345">メンバーアクセス式は、エンティティのメンバーにアクセスするために使用されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-345">A member access expression is used to access a member of an entity.</span></span>

```antlr
MemberAccessExpression
    : MemberAccessBase? Period IdentifierOrKeyword
      ( OpenParenthesis 'Of' TypeArgumentList CloseParenthesis )?
    ;

MemberAccessBase
    : Expression
    | NonArrayTypeName
    | 'Global'
    | 'MyClass'
    | 'MyBase'
    ;
```

<span data-ttu-id="8ad58-346">@No__t-0 の形式のメンバーアクセス。ここで `E` は式、非配列型の名前、キーワード `Global`、または省略されています。また、`I` は、省略可能な型引数リスト @no__t を持つ識別子で、次のように評価および分類されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-346">A member access of the form `E.I(Of A)`, where `E` is an expression, a non-array type name, the keyword `Global`, or omitted and `I` is an identifier with an optional type argument list `A`, is evaluated and classified as follows:</span></span>

1. <span data-ttu-id="8ad58-347">@No__t-0 を省略した場合、直ちに `With` ステートメントを含む式が `E` に置き換えられ、メンバーアクセスが実行されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-347">If `E` is omitted, then the expression from the immediately containing `With` statement is substituted for `E` and the member access is performed.</span></span> <span data-ttu-id="8ad58-348">@No__t-0 ステートメントが含まれていない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-348">If there is no containing `With` statement, a compile-time error occurs.</span></span>

2. <span data-ttu-id="8ad58-349">@No__t-0 が名前空間として分類されているか `E` がキーワード `Global` の場合、メンバー参照は指定された名前空間のコンテキストで実行されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-349">If `E` is classified as a namespace or `E` is the keyword `Global`, then the member lookup is done in the context of the specified namespace.</span></span> <span data-ttu-id="8ad58-350">@No__t-0 は、型引数リストで指定されたものと同じ数の型パラメーターを持つ、その名前空間のアクセス可能なメンバーの名前である場合、結果はそのメンバーになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-350">If `I` is the name of an accessible member of that namespace with the same number of type parameters as was supplied in the type argument list, if any, then the result is that member.</span></span> <span data-ttu-id="8ad58-351">結果は、メンバーに応じて名前空間または型として分類されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-351">The result is classified as a namespace or a type depending on the member.</span></span> <span data-ttu-id="8ad58-352">それ以外の場合は、コンパイル時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-352">Otherwise, a compile-time error occurs.</span></span>

3. <span data-ttu-id="8ad58-353">@No__t-0 が型または型として分類された式の場合、メンバー参照は、指定された型のコンテキストで実行されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-353">If `E` is a type or an expression classified as a type, then the member lookup is done in the context of the specified type.</span></span> <span data-ttu-id="8ad58-354">@No__t-0 が `E` のアクセス可能なメンバーの名前である場合、`E.I` が評価され、次のように分類されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-354">If `I` is the name of an accessible member of `E`, then `E.I` is evaluated and classified as follows:</span></span>

    31. <span data-ttu-id="8ad58-355">@No__t-0 がキーワード `New` で、`E` が列挙型ではない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-355">If `I` is the keyword `New` and `E` is not an enumeration then a compile-time error occurs.</span></span>
    32. <span data-ttu-id="8ad58-356">@No__t-0 の場合、型引数リストで指定された型パラメーターの数と同じ数の型パラメーターがある場合は、その型が返されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-356">If `I` identifies a type with the same number of type parameters as was supplied in the type argument list, if any, then the result is that type.</span></span>
    33. <span data-ttu-id="8ad58-357">@No__t-0 が1つ以上のメソッドを識別する場合、結果は、関連付けられた型引数リストを持ち、関連付けられたターゲット式がないメソッドグループになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-357">If `I` identifies one or more methods, then the result is a method group with the associated type argument list and no associated target expression.</span></span>
    34. <span data-ttu-id="8ad58-358">@No__t-0 が1つ以上のプロパティを識別し、型引数リストが指定されていない場合、結果はターゲット式が関連付けられていないプロパティグループになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-358">If `I` identifies one or more properties and no type argument list was supplied, then the result is a property group with no associated target expression.</span></span>
    35. <span data-ttu-id="8ad58-359">@No__t-0 が共有変数を識別し、型引数リストが指定されていない場合、結果は変数または値になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-359">If `I` identifies a shared variable and no type argument list was supplied, then the result is either a variable or a value.</span></span> <span data-ttu-id="8ad58-360">変数が読み取り専用で、参照が変数が宣言されている型の共有コンストラクターの外部で行われた場合、結果は `E` の共有変数 `I` になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-360">If the variable is read-only, and the reference occurs outside the shared constructor of the type in which the variable is declared, then the result is the value of the shared variable `I` in `E`.</span></span> <span data-ttu-id="8ad58-361">それ以外の場合、結果は `E` の共有変数 `I` になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-361">Otherwise, the result is the shared variable `I` in `E`.</span></span>
    36. <span data-ttu-id="8ad58-362">@No__t-0 が共有イベントを識別し、型引数リストが指定されていない場合、結果はターゲット式が関連付けられていないイベントアクセスになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-362">If `I` identifies a shared event and no type argument list was supplied, the result is an event access with no associated target expression.</span></span>
    37. <span data-ttu-id="8ad58-363">@No__t-0 が定数を識別し、型引数リストが指定されていない場合、結果はその定数の値になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-363">If `I` identifies a constant and no type argument list was supplied, then the result is the value of that constant.</span></span>
    38. <span data-ttu-id="8ad58-364">@No__t-0 が列挙型のメンバーを識別し、型引数リストが指定されていない場合、結果はその列挙メンバーの値になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-364">If `I` identifies an enumeration member and no type argument list was supplied, then the result is the value of that enumeration member.</span></span>
    39. <span data-ttu-id="8ad58-365">それ以外の場合、`E.I` は無効なメンバー参照であり、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-365">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>

4. <span data-ttu-id="8ad58-366">@No__t-0 が変数または値として分類されている場合、その型が-1 @no__t の場合、メンバー参照は `T` のコンテキストで実行されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-366">If `E` is classified as a variable or value, the type of which is `T`, then the member lookup is done in the context of `T`.</span></span> <span data-ttu-id="8ad58-367">@No__t-0 が `T` のアクセス可能なメンバーの名前である場合、`E.I` が評価され、次のように分類されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-367">If `I` is the name of an accessible member of `T`, then `E.I` is evaluated and classified as follows:</span></span>

    41. <span data-ttu-id="8ad58-368">@No__t-0 がキーワード `New`、`E` が `Me`、`MyBase`、または `MyClass` で、型引数が指定されていない場合、結果は @no__t に関連付けられたターゲット式を持つ @no__t の型のインスタンスコンストラクターを表すメソッドグループになります。型引数リストがありません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-368">If `I` is the keyword `New`, `E` is  `Me`, `MyBase`, or `MyClass`, and no type arguments were supplied, then the result is a method group representing the instance constructors of the type of `E` with an associated target expression of `E` and no type argument list.</span></span> <span data-ttu-id="8ad58-369">それ以外の場合は、コンパイル時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-369">Otherwise, a compile-time error occurs.</span></span>
    42. <span data-ttu-id="8ad58-370">@No__t-0 が1つ以上のメソッドを識別する場合 (`T` が @no__t ではない場合)、結果は、関連付けられた型引数リストと、関連付けられている対象式 `E` を持つメソッドグループになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-370">If `I` identifies one or more methods, including extension methods if `T` is not `Object`, then the result is a method group with the associated type argument list and an associated target expression of `E`.</span></span>
    43. <span data-ttu-id="8ad58-371">@No__t-0 が1つ以上のプロパティを識別し、型引数が指定されなかった場合、結果は `E` の対象式が関連付けられたプロパティグループになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-371">If `I` identifies one or more properties and no type arguments were supplied, then the result is a property group with an associated target expression of `E`.</span></span>
    44. <span data-ttu-id="8ad58-372">@No__t-0 が共有変数またはインスタンス変数を識別し、型引数が指定されていない場合、結果は変数または値のいずれかになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-372">If `I` identifies a shared variable or an instance variable and no type arguments were supplied, then the result is either a variable or a value.</span></span> <span data-ttu-id="8ad58-373">変数が読み取り専用で、変数が変数の種類 (shared または instance) に対して適切に宣言されているクラスのコンストラクターの外側で参照が発生した場合、結果はによって参照されるオブジェクトの変数 `I` になり `E`。</span><span class="sxs-lookup"><span data-stu-id="8ad58-373">If the variable is read-only, and the reference occurs outside a constructor of the class in which the variable is declared appropriate for the kind of variable (shared or instance), then the result is the value of the variable `I` in the object referenced by `E`.</span></span> <span data-ttu-id="8ad58-374">@No__t-0 が参照型の場合、結果は `E` によって参照されるオブジェクトの変数 `I` になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-374">If `T` is a reference type, then the result is the variable `I` in the object referenced by `E`.</span></span> <span data-ttu-id="8ad58-375">それ以外の場合、`T` が値の型で、式 `E` が変数として分類されると、結果は変数になります。それ以外の場合、結果は値になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-375">Otherwise, if `T` is a value type and the expression `E` is classified as a variable, the result is a variable; otherwise the result is a value.</span></span>
    45. <span data-ttu-id="8ad58-376">@No__t-0 によってイベントが識別され、型引数が指定されなかった場合、結果は `E` の関連付けられたターゲット式を使用したイベントアクセスになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-376">If `I` identifies an event and no type arguments were supplied, the result is an event access with an associated target expression of `E`.</span></span>
    46. <span data-ttu-id="8ad58-377">@No__t-0 が定数を識別し、型引数が指定されなかった場合、結果はその定数の値になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-377">If `I` identifies a constant and no type arguments were supplied, then the result is the value of that constant.</span></span>
    47. <span data-ttu-id="8ad58-378">@No__t-0 が列挙体のメンバーを識別し、型引数が指定されなかった場合、結果はその列挙体のメンバーの値になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-378">If `I` identifies an enumeration member and no type arguments were supplied, then the result is the value of that enumeration member.</span></span>
    48. <span data-ttu-id="8ad58-379">@No__t-0 @no__t が-1 の場合、結果は、関連付けられている型引数リストと、`E` の関連付けられたターゲット式を使用して、遅延バインディングアクセスとして分類された遅延バインディングメンバー参照になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-379">If `T` is `Object`, then the result is a late-bound member lookup classified as a late-bound access with the associated type argument list and an associated target expression of `E`.</span></span>

5. <span data-ttu-id="8ad58-380">それ以外の場合、`E.I` は無効なメンバー参照であり、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-380">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>

<span data-ttu-id="8ad58-381">@No__t-0 という形式のメンバーアクセスは `Me.I(Of A)` と同じですが、アクセスされるすべてのメンバーは、メンバーがオーバーライド不可能であるかのように扱われます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-381">A member access of the form `MyClass.I(Of A)` is equivalent to `Me.I(Of A)`, but all members accessed on it are treated as if the members are non-overridable.</span></span> <span data-ttu-id="8ad58-382">したがって、アクセスされるメンバーは、メンバーがアクセスされている値の実行時の型の影響を受けません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-382">Thus, the member accessed will not be affected by the run-time type of the value on which the member is being accessed.</span></span>

<span data-ttu-id="8ad58-383">@No__t-0 という形式のメンバーアクセスは、`CType(Me, T).I(Of A)` と同じです。 `T` は、メンバーアクセス式を含む型の直接の基本型です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-383">A member access of the form `MyBase.I(Of A)` is equivalent to `CType(Me, T).I(Of A)` where `T` is the direct base type of the type containing the member access expression.</span></span> <span data-ttu-id="8ad58-384">メソッド呼び出しは、呼び出されるメソッドがオーバーライド不可能であるかのように処理されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-384">All method invocations on it are treated as if the method being invoked is non-overridable.</span></span> <span data-ttu-id="8ad58-385">この形式のメンバーアクセスは、*基本アクセス*とも呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-385">This form of member access is also called a *base access*.</span></span>

<span data-ttu-id="8ad58-386">次の例は、`Me`、`MyBase`、`MyClass` がどのように関連しているかを示しています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-386">The following example demonstrates how `Me`, `MyBase` and `MyClass` relate:</span></span>

```vb
Class Base
    Public Overridable Sub F()
        Console.WriteLine("Base.F")
    End Sub
End Class

Class Derived
    Inherits Base

    Public Overrides Sub F()
        Console.WriteLine("Derived.F")
    End Sub

    Public Sub G()
        MyClass.F()
    End Sub
End Class

Class MoreDerived
    Inherits Derived

    Public Overrides Sub F()
        Console.WriteLine("MoreDerived.F")
    End Sub

    Public Sub H()
        MyBase.F()
    End Sub
End Class

Module Test
    Sub Main()
        Dim x As MoreDerived = new MoreDerived()

        x.F()
        x.G()
        x.H()
    End Sub

End Module
```

<span data-ttu-id="8ad58-387">このコードは次の出力を出力します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-387">This code prints out:</span></span>

```console
MoreDerived.F
Derived.F
Derived.F
```

<span data-ttu-id="8ad58-388">メンバーアクセス式がキーワード `Global` で始まる場合、キーワードは最も外側の名前空間を表します。これは、宣言が外側の名前空間をシャドウする場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-388">When a member access expression begins with the keyword `Global`, the keyword represents the outermost unnamed namespace, which is useful in situations where a declaration shadows an enclosing namespace.</span></span> <span data-ttu-id="8ad58-389">@No__t-0 キーワードを使用すると、そのような状況で最も外側の名前空間に "エスケープ" できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-389">The `Global` keyword allows "escaping" out to the outermost namespace in that situation.</span></span> <span data-ttu-id="8ad58-390">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-390">For example:</span></span>

```vb
Class System
End Class

Module Test
    Sub Main()
        ' Error: Class System does not contain Console
        System.Console.WriteLine("Hello, world!") 


        ' Legal, binds to System in outermost namespace
        Global.System.Console.WriteLine("Hello, world!") 
    End Sub
End Module
```

<span data-ttu-id="8ad58-391">上の例では、最初のメソッド呼び出しは無効です。識別子 `System` は、名前空間 `System` ではなく、クラス `System` にバインドされています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-391">In the above example, the first method call is invalid because the identifier `System` binds to the class `System`, not the namespace `System`.</span></span> <span data-ttu-id="8ad58-392">@No__t-0 名前空間にアクセスする唯一の方法は、`Global` を使用して、最も外側の名前空間をエスケープすることです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-392">The only way to access the `System` namespace is to use `Global` to escape out to the outermost namespace.</span></span>

<span data-ttu-id="8ad58-393">アクセスされているメンバーが共有されている場合、期間の左側の式は不要であり、メンバーアクセスが遅延バインディングされない限り評価されません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-393">If the member being accessed is shared, any expression on the left side of the period is superfluous and is not evaluated unless the member access is done late-bound.</span></span> <span data-ttu-id="8ad58-394">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-394">For example, consider the following code:</span></span>

```vb
Class C
    Public Shared F As Integer = 10
End Class

Module Test
    Public Function ReturnC() As C
        Console.WriteLine("Returning a new instance of C.")
        Return New C()
    End Function

    Public Sub Main()
        Console.WriteLine("The value of F is: " & ReturnC().F)
    End Sub
End Module
```

<span data-ttu-id="8ad58-395">@No__t-0 を出力します。これは、共有メンバー @no__t 3 にアクセスするために `C` のインスタンスを提供するために、関数 `ReturnC` を呼び出す必要がないためです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-395">It prints `The value of F is: 10` because the function `ReturnC` does not need to be called to provide an instance of `C` to access the shared member `F`.</span></span>


### <a name="identical-type-and-member-names"></a><span data-ttu-id="8ad58-396">同じ型とメンバー名</span><span class="sxs-lookup"><span data-stu-id="8ad58-396">Identical Type and Member Names</span></span>

<span data-ttu-id="8ad58-397">型と同じ名前を使用してメンバーの名前を指定することは珍しくありません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-397">It is not uncommon to name members using the same name as their type.</span></span> <span data-ttu-id="8ad58-398">ただし、このような状況では、わかりにくい名前の隠ぺいが発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-398">In that situation, however, inconvenient name hiding can occur:</span></span>

```vb
Enum Color
    Red
    Green
    Yellow
End Enum

Class Test
    ReadOnly Property Color() As Color
        Get
            Return Color.Red
        End Get
    End Property

    Shared Function DefaultColor() As Color
        Return Color.Green    ' Binds to the instance property!
    End Function
End Class
```

<span data-ttu-id="8ad58-399">前の例では、`DefaultColor` の簡易名 `Color` は、型ではなくインスタンスプロパティにバインドされています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-399">In the previous example, the simple name `Color` in `DefaultColor` binds to the instance property instead of the type.</span></span> <span data-ttu-id="8ad58-400">インスタンスメンバーは共有メンバーでは参照できないため、通常はエラーになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-400">Because an instance member cannot be referenced in a shared member, this would normally be an error.</span></span>

<span data-ttu-id="8ad58-401">ただし、特別な規則によって、この場合は型にアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-401">However, a special rule allows access to the type in this case.</span></span> <span data-ttu-id="8ad58-402">メンバーアクセス式の基本式が単純な名前で、型が同じである定数、フィールド、プロパティ、ローカル変数、またはパラメーターにバインドする場合、基本式はメンバーまたは型のいずれかを参照できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-402">If the base expression of a member access expression is a simple name and binds to a constant, field, property, local variable or parameter whose type has the same name, then the base expression can refer either to the member or the type.</span></span> <span data-ttu-id="8ad58-403">どちらか一方からアクセスできるメンバーが同じであるため、あいまいさが生じることはありません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-403">This can never result in ambiguity because the members that can be accessed off of either one are the same.</span></span>

### <a name="default-instances"></a><span data-ttu-id="8ad58-404">既定のインスタンス</span><span class="sxs-lookup"><span data-stu-id="8ad58-404">Default Instances</span></span>

<span data-ttu-id="8ad58-405">場合によっては、共通の基底クラスから派生したクラスは、通常、インスタンスを1つだけ持ちます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-405">In some situations, classes derived from a common base class usually or always have only a single instance.</span></span> <span data-ttu-id="8ad58-406">たとえば、ユーザーインターフェイスに表示されるほとんどのウィンドウでは、常に1つのインスタンスが画面上に表示されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-406">For example, most windows shown in a user interface only ever have one instance showing on the screen at any time.</span></span> <span data-ttu-id="8ad58-407">これらの種類のクラスを簡単に操作できるように、Visual Basic では、クラスの*既定のインスタンス*を自動的に生成できます。これらのクラスは、各クラスに対して簡単に参照できる単一のインスタンスを提供します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-407">To simplify working with these types of classes, Visual Basic can automatically generate *default instances* of the classes that provide a single, easily referenced instance for each class.</span></span>

<span data-ttu-id="8ad58-408">既定のインスタンスは、特定の1つの型ではなく、型の*ファミリ*に対して常に作成されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-408">Default instances are always created for a *family* of types rather than for one particular type.</span></span> <span data-ttu-id="8ad58-409">そのため、フォームから派生したクラス Form1 の既定のインスタンスを作成する代わりに、フォームから派生したすべてのクラスに対して既定のインスタンスが作成されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-409">So instead of creating a default instance for a class Form1 that derives from Form, default instances are created for all classes derived from Form.</span></span> <span data-ttu-id="8ad58-410">これは、基本クラスから派生した個々のクラスが、既定のインスタンスを持つように特別にマークする必要がないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-410">This means that each individual class that derives from the base class does not have to be specially marked to have a default instance.</span></span>

<span data-ttu-id="8ad58-411">クラスの既定のインスタンスは、そのクラスの既定のインスタンスを返すコンパイラによって生成されるプロパティによって表されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-411">The default instance of a class is represented by a compiler-generated property that returns the default instance of that class.</span></span> <span data-ttu-id="8ad58-412">特定の基本クラスから派生したすべてのクラスの既定のインスタンスの割り当てと破棄を管理する*グループクラス*と呼ばれるクラスのメンバーとして生成されたプロパティ。</span><span class="sxs-lookup"><span data-stu-id="8ad58-412">The property generated as a member of a class called the *group class* that manages allocating and destroying default instances for all classes derived from the particular base class.</span></span> <span data-ttu-id="8ad58-413">たとえば、`Form` から派生したクラスの既定のインスタンスプロパティはすべて、`MyForms` クラスで収集できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-413">For example, all of the default instance properties of classes derived from `Form` may be collected in the `MyForms` class.</span></span> <span data-ttu-id="8ad58-414">Group クラスのインスタンスが式によって返される場合 `My.Forms`,、次のコードは、派生クラスの既定のインスタンスにアクセスする-1 と `Form2` @no__t します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-414">If an instance of the group class is returned by the expression `My.Forms`, then the following code accesses the default instances of derived classes `Form1` and `Form2`:</span></span>

```vb
Class Form1
    Inherits Form
    Public x As Integer
End Class

Class Form2
    Inherits Form
    Public y As Integer
End Class

Module Main
    Sub Main()
        My.Forms.Form1.x = 10
        Console.WriteLine(My.Forms.Form2.y)
    End Sub
End Module
```

<span data-ttu-id="8ad58-415">既定のインスタンスは、そのインスタンスへの最初の参照までは作成されません。既定のインスタンスを表すプロパティを取得すると、既定のインスタンスがまだ作成されていない場合、または `Nothing` に設定されている場合は、既定のインスタンスが作成されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-415">Default instances will not be created until the first reference to them; fetching the property representing the default instance causes the default instance to be created if it has not already been created or has been set to `Nothing`.</span></span> <span data-ttu-id="8ad58-416">既定のインスタンスが存在するかどうかのテストを許可するために、既定のインスタンスが `Is` または `IsNot` 演算子のターゲットである場合、既定のインスタンスは作成されません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-416">To allow testing for the existence of a default instance, when a default instance is the target of an `Is` or `IsNot` operator, the default instance will not be created.</span></span> <span data-ttu-id="8ad58-417">したがって、既定のインスタンスが作成されないように、既定のインスタンスが @no__t 0 またはその他の参照であるかどうかをテストできます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-417">Thus, it is possible to test whether a default instance is `Nothing` or some other reference without causing the default instance to be created.</span></span>

<span data-ttu-id="8ad58-418">既定のインスタンスは、既定のインスタンスを持つクラスの外部から既定のインスタンスを簡単に参照できるようにすることを目的としています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-418">Default instances are intended to make it easy to refer to the default instance from outside of the class that has the default instance.</span></span> <span data-ttu-id="8ad58-419">を定義するクラス内から既定のインスタンスを使用すると、参照されているインスタンス (つまり、既定のインスタンスまたは現在のインスタンス) との混同が生じる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-419">Using a default instance from within a class that defines it might cause confusion as to which instance is being referred to, i.e. the default instance or the current instance.</span></span> <span data-ttu-id="8ad58-420">たとえば、次のコードでは、別のインスタンスから呼び出されている場合でも、既定のインスタンスの値 `x` のみを変更します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-420">For example, the following code modifies only the value `x` in the default instance, even though it is being called from another instance.</span></span> <span data-ttu-id="8ad58-421">したがって、コードは `10` ではなく 0 @no__t 値を出力します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-421">Thus the code would print the value `5` instead of `10`:</span></span>

```vb
Class Form1
    Inherits Form

    Public x As Integer = 5

    Public Sub ChangeX()
        Form1.x = 10
    End Sub
End Class

Module Main
    Sub Main()
        Dim f As Form1 = New Form1()
        f.ChangeX()
        Console.WriteLine(f.x)
    End Sub
End Module
```

<span data-ttu-id="8ad58-422">このような混乱を防ぐために、既定のインスタンスの型のインスタンスメソッド内から既定のインスタンスを参照することは無効です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-422">To prevent this kind of confusion, it is not valid to refer to a default instance from within an instance method of the default instance's type.</span></span>

#### <a name="default-instances-and-type-names"></a><span data-ttu-id="8ad58-423">既定のインスタンスと型名</span><span class="sxs-lookup"><span data-stu-id="8ad58-423">Default Instances and Type Names</span></span>

<span data-ttu-id="8ad58-424">既定のインスタンスには、その型の名前を使用して直接アクセスすることもできます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-424">A default instance may also be accessible directly through its type's name.</span></span> <span data-ttu-id="8ad58-425">この場合、型名が許可されていない式のコンテキストでは、式 `E` は、`E` は既定のインスタンスを持つクラスの完全修飾名を表し、は `E'` に変更されます。ここで、`E'` は、をフェッチする式を表します。既定のインスタンスプロパティ。</span><span class="sxs-lookup"><span data-stu-id="8ad58-425">In this case, in any expression context where the type name is not allowed the expression `E`, where `E` represents the fully qualified name of the class with a default instance, is changed to `E'`, where `E'` represents an expression that fetches the default instance property.</span></span> <span data-ttu-id="8ad58-426">たとえば、`Form` から派生したクラスの既定のインスタンスで、型名を使用して既定のインスタンスにアクセスできる場合、次のコードは前の例のコードに相当します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-426">For example, if default instances for classes derived from `Form` allow accessing the default instance through the type name, then the following code is equivalent to the code in the previous example:</span></span>

```vb
Module Main
    Sub Main()
        Form1.x = 10
        Console.WriteLine(Form2.y)
    End Sub
End Module
```

<span data-ttu-id="8ad58-427">これはまた、型の名前を使用してアクセスできる既定のインスタンスも、型名を使用して割り当てることができることを意味します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-427">This also means that a default instance that is accessible through its type's name is also assignable through the type name.</span></span> <span data-ttu-id="8ad58-428">たとえば、次のコードでは、`Form1` の既定のインスタンスを `Nothing` に設定しています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-428">For example, the following code sets the default instance of `Form1` to `Nothing`:</span></span>

```vb
Module Main
    Sub Main()
        Form1 = Nothing
    End Sub
End Module
```

<span data-ttu-id="8ad58-429">@No__t-0 の意味は、-1 @no__t がクラスを表し、`I` が共有メンバーを変更しないことを表していることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8ad58-429">Note that the meaning of `E.I` were `E` represents a class and `I` represents a shared member does not change.</span></span> <span data-ttu-id="8ad58-430">このような式は、引き続きクラスインスタンスから直接共有メンバーにアクセスし、既定のインスタンスを参照しません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-430">Such an expression still accesses the shared member directly off of the class instance and does not reference the default instance.</span></span>

#### <a name="group-classes"></a><span data-ttu-id="8ad58-431">グループクラス</span><span class="sxs-lookup"><span data-stu-id="8ad58-431">Group Classes</span></span>

<span data-ttu-id="8ad58-432">@No__t-0 属性は、既定のインスタンスのファミリのグループクラスを示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-432">The `Microsoft.VisualBasic.MyGroupCollectionAttribute` attribute indicates the group class for a family of default instances.</span></span> <span data-ttu-id="8ad58-433">属性には、次の4つのパラメーターがあります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-433">The attribute has four parameters:</span></span>

* <span data-ttu-id="8ad58-434">パラメーター `TypeToCollect` は、グループの基本クラスを指定します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-434">The parameter `TypeToCollect` specifies the base class for the group.</span></span> <span data-ttu-id="8ad58-435">(型パラメーターに関係なく) この名前を持つ型から派生するオープン型パラメーターを持たないインスタンス化可能なクラスはすべて、自動的に既定のインスタンスを持ちます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-435">All instantiable classes without open type parameters that derive from a type with this name (regardless of type parameters) will automatically have a default instance.</span></span>

* <span data-ttu-id="8ad58-436">パラメーター `CreateInstanceMethodName` は、既定のインスタンスプロパティに新しいインスタンスを作成するために、group クラスで呼び出すメソッドを指定します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-436">The parameter `CreateInstanceMethodName` specifies the method to call in the group class to create a new instance in a default instance property.</span></span>

* <span data-ttu-id="8ad58-437">パラメーター `DisposeInstanceMethodName` は、既定のインスタンスプロパティに値 `Nothing` が割り当てられている場合に、既定のインスタンスプロパティを破棄するために、group クラスで呼び出すメソッドを指定します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-437">The parameter `DisposeInstanceMethodName` specifies the method to call in the group class to dispose of a default instance property if the default instance property is assigned the value `Nothing`.</span></span>

* <span data-ttu-id="8ad58-438">パラメーター @no__t-@no__t 0 に指定すると、既定のインスタンスに型名を使用して直接アクセスできる場合は、クラス名の代わりに-1 が指定されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-438">The parameter `DefaultInstanceAlias` specifics the expression `E'` to substitute for the class name if the default instances are accessible directly through their type name.</span></span> <span data-ttu-id="8ad58-439">このパラメーターが @no__t 0 または空の文字列の場合、このグループの種類の既定のインスタンスには、その型の名前を使用して直接アクセスすることはできません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-439">If this parameter is `Nothing` or an empty string, default instances on this group type are not accessible directly through their type's name.</span></span> <span data-ttu-id="8ad58-440">(__注:__</span><span class="sxs-lookup"><span data-stu-id="8ad58-440">(__Note.__</span></span> <span data-ttu-id="8ad58-441">Visual Basic 言語の現在のすべての実装では、コンパイラが提供するコードを除き、`DefaultInstanceAlias` パラメーターは無視されます)。</span><span class="sxs-lookup"><span data-stu-id="8ad58-441">In all current implementations of the Visual Basic language, the `DefaultInstanceAlias` parameter is ignored, except in compiler-provided code.)</span></span>

<span data-ttu-id="8ad58-442">最初の3つのパラメーターの型とメソッドの名前をコンマで区切ることで、同じグループに複数の型を集めることができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-442">Multiple types can be collected into the same group by separating the names of the types and methods in the first three parameters using commas.</span></span> <span data-ttu-id="8ad58-443">各パラメーターには同じ数の項目が必要です。また、リストの要素は順番に一致します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-443">There must be the same number of items in each parameter, and the list elements are matched in order.</span></span> <span data-ttu-id="8ad58-444">たとえば、次の属性の宣言は、`C1`、`C2`、または @no__t から派生した型を1つのグループに収集します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-444">For example, the following attribute declaration collects types that derive from `C1`, `C2` or `C3` into a single group:</span></span>

```vb
<Microsoft.VisualBasic.MyGroupCollection("C1, C2, C3", _
    "CreateC1, CreateC2, CreateC3", _
    "DisposeC1, DisposeC2, DisposeC3", "My.Cs")>
Public NotInheritable Class MyCs
    ...
End Class
```

<span data-ttu-id="8ad58-445">Create メソッドのシグネチャは、`Shared Function <Name>(Of T As {New, <Type>})(Instance Of T) As T` の形式にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-445">The signature of the create method must be of the form `Shared Function <Name>(Of T As {New, <Type>})(Instance Of T) As T`.</span></span> <span data-ttu-id="8ad58-446">Dispose メソッドは、`Shared Sub <Name>(Of T As <Type>)(ByRef Instance Of T)` の形式にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-446">The dispose method must be of the form `Shared Sub <Name>(Of T As <Type>)(ByRef Instance Of T)`.</span></span> <span data-ttu-id="8ad58-447">したがって、前のセクションの例の group クラスは次のように宣言できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-447">Thus, the group class for the example in the preceding section could be declared as follows:</span></span>

```vb
<Microsoft.VisualBasic.MyGroupCollection("Form", "Create", _
    "Dispose", "My.Forms")> _
Public NotInheritable Class MyForms
    Private Shared Function Create(Of T As {New, Form}) _
        (Instance As T) As T
        If Instance Is Nothing Then
            Return New T()
        Else
            Return Instance
        End If
    End Function

    Private Shared Sub Dispose(Of T As Form)(ByRef Instance As T)
        Instance.Close()
        Instance = Nothing
    End Sub
End Class
```

<span data-ttu-id="8ad58-448">派生クラスを宣言したソースファイル `Form1` の場合、生成された group クラスは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-448">If a source file declared a derived class `Form1`, the generated group class would be equivalent to:</span></span>

```vb
<Microsoft.VisualBasic.MyGroupCollection("Form", "Create", _
    "Dispose", "My.Forms")> _
Public NotInheritable Class MyForms
    Private Shared Function Create(Of T As {New, Form}) _
        (Instance As T) As T
        If Instance Is Nothing Then
            Return New T()
        Else
            Return Instance
        End If
    End Function

    Private Shared Sub Dispose(Of T As Form)(ByRef Instance As T)
        Instance.Close()
        Instance = Nothing
    End Sub

    Private m_Form1 As Form1

    Public Property Form1() As Form1
        Get
            Return Create(m_Form1)
        End Get
        Set (Value As Form1)
            If Value IsNot Nothing AndAlso Value IsNot m_Form1 Then
                Throw New ArgumentException( _
                    "Property can only be set to Nothing.")
            End If
            Dispose(m_Form1)
        End Set
    End Property
End Class
```

### <a name="extension-method-collection"></a><span data-ttu-id="8ad58-449">拡張メソッドのコレクション</span><span class="sxs-lookup"><span data-stu-id="8ad58-449">Extension Method Collection</span></span>

<span data-ttu-id="8ad58-450">メンバーアクセス式の拡張メソッド `E.I` は、現在のコンテキストで使用可能な名前 `I` を持つすべての拡張メソッドを収集することによって収集されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-450">Extension methods for the member access expression `E.I` are collected by gathering all of the extension methods with the name `I` that are available in the current context:</span></span>

1. <span data-ttu-id="8ad58-451">まず、式を含む入れ子になった各型がチェックされ、最も内側から外側に向かっています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-451">First, each nested type containing the expression is checked, starting from the innermost and going to the outermost.</span></span>
2. <span data-ttu-id="8ad58-452">次に、入れ子になった各名前空間が、最も内側から最も外側の名前空間に向かってチェックされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-452">Then, each nested namespace is checked, starting from the innermost and going to the outermost namespace.</span></span>
3. <span data-ttu-id="8ad58-453">次に、ソースファイルのインポートが確認されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-453">Then, the imports in the source file are checked.</span></span>
4. <span data-ttu-id="8ad58-454">次に、コンパイル環境で定義されているインポートがチェックされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-454">Then, the imports defined by the compilation environment are checked.</span></span>

<span data-ttu-id="8ad58-455">拡張メソッドは、対象の式の型から拡張メソッドの最初のパラメーターの型への拡張ネイティブ変換がある場合にのみ収集されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-455">An extension method is collected only if there is a widening native conversion from the target expression type to the type of the first parameter of the extension method.</span></span> <span data-ttu-id="8ad58-456">通常の簡易名式のバインドとは異なり、検索では*すべて*の拡張メソッドが収集されます。拡張メソッドが見つかった場合、コレクションは停止しません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-456">And unlike regular simple name expression binding, the search collects *all* extension methods; the collection does not stop when an extension method is found.</span></span> <span data-ttu-id="8ad58-457">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-457">For example:</span></span>

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
        Sub M1(c As C1, y As Double)
        End Sub
    End Module
End Namespace

Namespace N1.N2.N3
    Module Test
        Sub Main()
            Dim x As New C1()

            ' Calls N1C1Extensions.M1
            x.M1(10)
        End Sub
    End Module
End Namespace
```

<span data-ttu-id="8ad58-458">この例では、-1 @no__t 前に `N2C1Extensions.M1` が検出された場合でも、両方とも拡張メソッドと見なされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-458">In this example, even though `N2C1Extensions.M1` is found before `N1C1Extensions.M1`, they both are considered as extension methods.</span></span> <span data-ttu-id="8ad58-459">すべての拡張メソッドが収集されると、そのメソッドは*カリー*化されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-459">Once all of the extension methods have been collected, they are then *curried*.</span></span> <span data-ttu-id="8ad58-460">カリー化は、拡張メソッド呼び出しのターゲットを取得し、拡張メソッド呼び出しに適用します。これにより、最初のパラメーター (指定されているため) が削除された新しいメソッドシグネチャが生成されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-460">Currying takes the target of the extension method call and applies it to the extension method call, resulting in a new method signature with the first parameter removed (because it has been specified).</span></span> <span data-ttu-id="8ad58-461">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-461">For example:</span></span>

```vb
Imports System.Runtime.CompilerServices

Module Ext1
    <Extension> _
    Sub M(x As Integer, y As Integer)
    End Sub
End Module

Module Ext2
    <Extension> _
    Sub M(x As Integer, y As Double)
    End Sub
End Module

Module Main
    Sub Test()
        Dim v As Integer = 10

        ' The curried method signatures considered are:
        '        Ext1.M(y As Integer)
        '        Ext2.M(y As Double)
        v.M(10)
    End Sub
End Module
```

<span data-ttu-id="8ad58-462">上の例では、`v` を `Ext1.M` に適用したカリー化された結果が、メソッドシグネチャ `Sub M(y As Integer)` です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-462">In the above example, the curried result of applying `v` to `Ext1.M` is the method signature `Sub M(y As Integer)`.</span></span>

<span data-ttu-id="8ad58-463">カリー化は、拡張メソッドの最初のパラメーターを削除するだけでなく、最初のパラメーターの型の一部であるメソッド型パラメーターもすべて削除します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-463">In addition to removing the first parameter of the extension method, currying also removes any method type parameters that are a part of the type of the first parameter.</span></span> <span data-ttu-id="8ad58-464">カリー化がメソッド型パラメーターを使用して拡張メソッドを実行すると、最初のパラメーターに型の推定が適用され、推論されるすべての型パラメーターに対して結果が固定されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-464">When currying an extension method with method type parameter, type inference is applied to the first parameter and the result is fixed for any type parameters that are inferred.</span></span> <span data-ttu-id="8ad58-465">型の推定が失敗した場合、メソッドは無視されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-465">If type inference fails, the method is ignored.</span></span> <span data-ttu-id="8ad58-466">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-466">For example:</span></span>

```vb
Imports System.Runtime.CompilerServices

Module Ext1
    <Extension> _
    Sub M(Of T, U)(x As T, y As U)
    End Sub
End Module

Module Ext2
    <Extension> _
    Sub M(Of T)(x As T, y As T)
    End Sub
End Module

Module Main
    Sub Test()
        Dim v As Integer = 10

        ' The curried method signatures considered are:
        '        Ext1.M(Of U)(y As U)
        '        Ext2.M(y As Integer)
        v.M(10)
    End Sub
End Module
```

<span data-ttu-id="8ad58-467">上の例では、`v` を `Ext1.M` に適用したカリー化された結果は、メソッドシグネチャ @no__t です。これは、型パラメーター `T` がカリー化の結果として推論され、現在は修正されているためです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-467">In the above example, the curried result of applying `v` to `Ext1.M` is the method signature `Sub M(Of U)(y As U)`, because the type parameter `T` is inferred as a result of the currying and is now fixed.</span></span> <span data-ttu-id="8ad58-468">型パラメーター `U` は、カリー化の一部として推論されなかったため、開いているパラメーターのままです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-468">Because the type parameter `U` was not inferred as a part of the currying, it remains an open parameter.</span></span> <span data-ttu-id="8ad58-469">同様に、型パラメーター `T` は `v` を `Ext2.M` に適用した結果として推論されるため、パラメーター `y` の型は `Integer` として固定されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-469">Similarly, because the type parameter `T` is inferred as a result of applying `v` to `Ext2.M`, the type of parameter `y` becomes fixed as `Integer`.</span></span> <span data-ttu-id="8ad58-470">他の型として推論されることはありません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-470">It will not be inferred to be any other type.</span></span> <span data-ttu-id="8ad58-471">署名をカリー化すると、`New` 制約を除くすべての制約も適用されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-471">When currying the signature, all constraints except for `New` constraints are also applied.</span></span> <span data-ttu-id="8ad58-472">制約が満たされない場合、またはカリー化の一部として推論されなかった型に依存している場合、拡張メソッドは無視されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-472">If the constraints are not satisfied, or depend on a type that was not inferred as a part of currying, the extension method is ignored.</span></span> <span data-ttu-id="8ad58-473">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-473">For example:</span></span>

```vb
Imports System.Runtime.CompilerServices

Module Ext1
    <Extension> _
    Sub M1(Of T As Structure)(x As T, y As Integer)
    End Sub

    <Extension> _
    Sub M2(Of T As U, U)(x As T, y As U)
    End Sub
End Module

Module Main
    Sub Test()
        Dim s As String = "abc"

        ' Error: String does not satisfy the Structure constraint
        s.M1(10)

        ' Error: T depends on U, which cannot be inferred
        s.M2(10)
    End Sub
End Module
```

<span data-ttu-id="8ad58-474">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="8ad58-474">__Note.__</span></span> <span data-ttu-id="8ad58-475">カリー化の拡張メソッドを実行する主な理由の1つは、クエリパターンメソッドの引数を評価する前に、クエリ式で反復処理の型を推論できることです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-475">One of the main reasons for doing currying of extension methods is that it allows query expressions to infer the type of the iteration before evaluating the arguments to a query pattern method.</span></span> <span data-ttu-id="8ad58-476">ほとんどのクエリパターンメソッドでは、型の推定を必要とするラムダ式が使用されるため、クエリ式を評価するプロセスが非常に簡単になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-476">Since most query pattern methods take lambda expressions, which require type inference themselves, this greatly simplifies the process of evaluating a query expression.</span></span>

<span data-ttu-id="8ad58-477">通常のインターフェイスの継承とは異なり、相互に関連しない2つのインターフェイスを拡張する拡張メソッドは、同じカリー化シグネチャを持たない限り使用できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-477">Unlike normal interface inheritance, extension methods that extend two interfaces that do not relate to one another are available, as long as they do not have the same curried signature:</span></span>

```vb
Imports System.Runtime.CompilerServices

Interface I1
End Interface

Interface I2
End Interface

Class C1
    Implements I1, I2
End Class

Module I1Ext
    <Extension> _
    Sub M1(i As I1, x As Integer)
    End Sub

    <Extension> _
    Sub M2(i As I1, x As Integer)
    End Sub
End Module

Module I2Ext
    <Extension> _
    Sub M1(i As I2, x As Integer)
    End Sub

    <Extension> _
    Sub M2(I As I2, x As Double)
    End Sub
End Module

Module Main
    Sub Test()
        Dim c As New C1()

        ' Error: M is ambiguous between I1Ext.M1 and I2Ext.M1.
        c.M1(10)

        ' Calls I1Ext.M2
        c.M2(10)
    End Sub
End Module
```

<span data-ttu-id="8ad58-478">最後に、遅延バインディングを実行する場合、拡張メソッドは考慮されないことに注意することが重要です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-478">Finally, it is important to remember that extension methods are not considered when doing late binding:</span></span>

```vb
Module Test
    Sub Main()
        Dim o As Object = ...

        ' Ignores extension methods
        o.M1()
    End Sub
End Module
```

## <a name="dictionary-member-access-expressions"></a><span data-ttu-id="8ad58-479">ディクショナリメンバーアクセス式</span><span class="sxs-lookup"><span data-stu-id="8ad58-479">Dictionary Member Access Expressions</span></span>

<span data-ttu-id="8ad58-480">*ディクショナリメンバーアクセス式*は、コレクションのメンバーを検索するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-480">A *dictionary member access expression* is used to look up a member of a collection.</span></span> <span data-ttu-id="8ad58-481">ディクショナリメンバーへのアクセスには `E!I` の形式を使用します。ここで `E` は値として分類される式で、`I` は識別子です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-481">A dictionary member access takes the form of `E!I`, where `E` is an expression that is classified as a value and `I` is an identifier.</span></span>

```antlr
DictionaryAccessExpression
    : Expression? '!' IdentifierOrKeyword
    ;
```

<span data-ttu-id="8ad58-482">式の型には、単一の `String` パラメーターによってインデックス付けされた既定のプロパティが必要です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-482">The type of the expression must have a default property indexed by a single `String` parameter.</span></span> <span data-ttu-id="8ad58-483">ディクショナリメンバーアクセス式 `E!I` は、式 `E.D("I")` に変換されます。 `D` は `E` の既定のプロパティです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-483">The dictionary member access expression `E!I` is transformed into the expression `E.D("I")`, where `D` is the default property of `E`.</span></span> <span data-ttu-id="8ad58-484">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-484">For example:</span></span>

```vb
Class Keys
    Public ReadOnly Default Property Item(s As String) As Integer
        Get
            Return 10
        End Get
    End Property 
End Class

Module Test
    Sub Main()
        Dim x As Keys = new Keys()
        Dim y As Integer
        ' The two statements are equivalent.
        y = x!abc
        y = x("abc")
    End Sub
End Module
```

<span data-ttu-id="8ad58-485">感嘆符が式なしで指定されている場合は、直ちに `With` ステートメントを含む式が使用されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-485">If an exclamation point is specified with no expression, the expression from the immediately containing `With` statement is assumed.</span></span> <span data-ttu-id="8ad58-486">@No__t-0 ステートメントが含まれていない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-486">If there is no containing `With` statement, a compile-time error occurs.</span></span>


## <a name="invocation-expressions"></a><span data-ttu-id="8ad58-487">Invocation 式</span><span class="sxs-lookup"><span data-stu-id="8ad58-487">Invocation Expressions</span></span>

<span data-ttu-id="8ad58-488">呼び出し式は、呼び出し先と省略可能な引数リストで構成されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-488">An invocation expression consists of an invocation target and an optional argument list.</span></span>

```antlr
InvocationExpression
    : Expression ( OpenParenthesis ArgumentList? CloseParenthesis )?
    ;

ArgumentList
    : PositionalArgumentList
    | PositionalArgumentList Comma NamedArgumentList
    | NamedArgumentList
    ;

PositionalArgumentList
    : Expression? ( Comma Expression? )*
    ;

NamedArgumentList
    : IdentifierOrKeyword ColonEquals Expression
      ( Comma IdentifierOrKeyword ColonEquals Expression )*
    ;
```

<span data-ttu-id="8ad58-489">ターゲット式は、メソッドグループ、または型がデリゲート型である値として分類される必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-489">The target expression must be classified as a method group or a value whose type is a delegate type.</span></span> <span data-ttu-id="8ad58-490">対象の式が型がデリゲート型である値の場合、呼び出し式のターゲットはデリゲート型の `Invoke` メンバーのメソッドグループになり、対象の式がメソッドグループの関連付けられたターゲット式になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-490">If the target expression is a value whose type is a delegate type, then the target of the invocation expression becomes the method group for the `Invoke` member of the delegate type and the target expression becomes the associated target expression of the method group.</span></span>

<span data-ttu-id="8ad58-491">引数リストには、位置引数と名前付き引数の2つのセクションがあります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-491">An argument list has two sections: positional arguments and named arguments.</span></span> <span data-ttu-id="8ad58-492">*位置指定引数*は式であり、名前付き引数の前に指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-492">*Positional arguments* are expressions and must precede any named arguments.</span></span> <span data-ttu-id="8ad58-493">*名前付き引数*は、キーワードに一致できる識別子で始まり、その後に `:=` と式が続きます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-493">*Named arguments* start with an identifier that can match keywords, followed by `:=` and an expression.</span></span>

<span data-ttu-id="8ad58-494">メソッドグループにインスタンスと拡張メソッドの両方を含むアクセス可能なメソッドが1つだけ含まれており、そのメソッドが引数を取らず、関数である場合、メソッドグループは、空の引数リストを持つ呼び出し式として解釈され、結果はになります。指定された引数リストを持つ呼び出し式のターゲットとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-494">If the method group only contains one accessible method, including both instance and extension methods, and that method takes no arguments and is a function, then the method group is interpreted as an invocation expression with an empty argument list and the result is used as the target of an invocation expression with the provided argument list(s).</span></span> <span data-ttu-id="8ad58-495">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-495">For example:</span></span>

```vb
Class C1
    Function M1() As Integer()
        Return New Integer() { 1, 2, 3 }
    End Sub
End Class

Module Test
    Sub Main()
        Dim c As New C1()

        ' Prints 3
        Console.WriteLine(c.M1(2))
    End Sub
End Module
```

<span data-ttu-id="8ad58-496">それ以外の場合は、オーバーロードの解決がメソッドに適用され、指定された引数リストに対して最も適切なメソッドが選択されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-496">Otherwise, overload resolution is applied to the methods to pick the most applicable method for the given argument list(s).</span></span> <span data-ttu-id="8ad58-497">最も適切なメソッドが関数の場合、呼び出し式の結果は、関数の戻り値の型として型指定された値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-497">If the most applicable method is a function, then the result of the invocation expression is classified as a value typed as the return type of the function.</span></span> <span data-ttu-id="8ad58-498">最も適切なメソッドがサブルーチンの場合、結果は void として分類されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-498">If the most applicable method is a subroutine, then the result is classified as void.</span></span> <span data-ttu-id="8ad58-499">最も適切なメソッドが、本体のない部分メソッドである場合、呼び出し式は無視され、結果は void として分類されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-499">If the most applicable method is a partial method that has no body, then the invocation expression is ignored and the result is classified as void.</span></span>

<span data-ttu-id="8ad58-500">事前バインディングされた呼び出し式の場合、引数は、対応するパラメーターがターゲットメソッドで宣言されている順序で評価されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-500">For an early-bound invocation expression, the arguments are evaluated in the order in which the corresponding parameters are declared in the target method.</span></span> <span data-ttu-id="8ad58-501">遅延バインディングされたメンバーアクセス式では、メンバーアクセス式に出現する順序で評価されます。「遅延バインディング[式](expressions.md#late-bound-expressions)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8ad58-501">For a late-bound member access expression, they are evaluated in the order in which they appear in the member access expression: see Section [Late-Bound Expressions](expressions.md#late-bound-expressions).</span></span>

## <a name="overloaded-method-resolution"></a><span data-ttu-id="8ad58-502">オーバーロードされたメソッドの解決:</span><span class="sxs-lookup"><span data-stu-id="8ad58-502">Overloaded Method Resolution:</span></span>
<span data-ttu-id="8ad58-503">オーバーロードの解決については、引数リストを指定したメンバー/型の特異性、Genericity、引数リストへの適用性、引数の引き渡し、省略可能なパラメーターの引数の指定、条件付きメソッド、および型引数の推定については、「」を参照してください[。オーバーロードの解決](overload-resolution.md)。</span><span class="sxs-lookup"><span data-stu-id="8ad58-503">For Overload Resolution, Specificity of members/types given an argument list, Genericity, Applicability to Argument List, Passing Arguments, and Picking Arguments for Optional Parameters, Conditional Methods, and Type Argument Inference: see Section [Overload Resolution](overload-resolution.md).</span></span>

## <a name="index-expressions"></a><span data-ttu-id="8ad58-504">インデックス式</span><span class="sxs-lookup"><span data-stu-id="8ad58-504">Index Expressions</span></span>

<span data-ttu-id="8ad58-505">*インデックス式*は、配列要素を生成するか、プロパティグループをプロパティアクセスに再分類します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-505">An *index expression* results in an array element or reclassifies a property group into a property access.</span></span> <span data-ttu-id="8ad58-506">インデックス式は、式、左かっこ、インデックス引数リスト、および閉じかっこで構成されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-506">An index expression consists of, in order, an expression, an opening parenthesis, an index argument list, and a closing parenthesis.</span></span>

```antlr
IndexExpression
    : Expression OpenParenthesis ArgumentList? CloseParenthesis
    ;
```

<span data-ttu-id="8ad58-507">インデックス式のターゲットは、プロパティグループまたは値のいずれかとして分類される必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-507">The target of the index expression must be classified as either a property group or a value.</span></span> <span data-ttu-id="8ad58-508">インデックス式は、次のように処理されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-508">An index expression is processed as follows:</span></span>

* <span data-ttu-id="8ad58-509">ターゲット式が値として分類され、その型が配列型でない場合、`Object` または `System.Array` の場合、型には既定のプロパティが必要です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-509">If the target expression is classified as a value and if its type is not an array type, `Object`, or `System.Array`, the type must have a default property.</span></span> <span data-ttu-id="8ad58-510">インデックスは、型のすべての既定のプロパティを表すプロパティグループで実行されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-510">The index is performed on a property group that represents all of the default properties of the type.</span></span> <span data-ttu-id="8ad58-511">Visual Basic でパラメーターなしの既定のプロパティを宣言することは無効ですが、他の言語ではこのようなプロパティを宣言できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-511">Although it is not valid to declare a parameterless default property in Visual Basic, other languages may allow declaring such a property.</span></span> <span data-ttu-id="8ad58-512">そのため、引数を指定せずにプロパティのインデックスを作成することはできます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-512">Consequently, indexing a property with no arguments is allowed.</span></span>

* <span data-ttu-id="8ad58-513">式の結果が配列型の値である場合、引数リスト内の引数の数は配列型のランクと同じである必要があり、名前付き引数を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-513">If the expression results in a value of an array type, the number of arguments in the argument list must be the same as the rank of the array type and may not include named arguments.</span></span> <span data-ttu-id="8ad58-514">実行時に無効なインデックスがある場合は、@no__t 0 の例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-514">If any of the indexes are invalid at run time, a `System.IndexOutOfRangeException` exception is thrown.</span></span> <span data-ttu-id="8ad58-515">各式は、型 `Integer` に暗黙的に変換できる必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-515">Each expression must be implicitly convertible to type `Integer`.</span></span> <span data-ttu-id="8ad58-516">インデックス式の結果は、指定されたインデックス位置にある変数で、変数として分類されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-516">The result of the index expression is the variable at the specified index and is classified as a variable.</span></span>

* <span data-ttu-id="8ad58-517">式がプロパティグループとして分類されている場合、オーバーロードの解決は、プロパティのいずれかがインデックス引数リストに適用可能かどうかを判断するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-517">If the expression is classified as a property group, overload resolution is used to determine whether one of the properties is applicable to the index argument list.</span></span> <span data-ttu-id="8ad58-518">プロパティグループに @no__t 0 のアクセサーを持つ1つのプロパティのみが含まれていて、そのアクセサーが引数をとらない場合、プロパティグループは空の引数リストを持つインデックス式として解釈されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-518">If the property group only contains one property that has a `Get` accessor and if that accessor takes no arguments, then the property group is interpreted as an index expression with an empty argument list.</span></span> <span data-ttu-id="8ad58-519">結果は、現在のインデックス式のターゲットとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-519">The result is used as the target of the current index expression.</span></span> <span data-ttu-id="8ad58-520">適用可能なプロパティがない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-520">If no properties are applicable, then a compile-time error occurs.</span></span> <span data-ttu-id="8ad58-521">それ以外の場合、式は、プロパティグループの関連付けられた対象の式 (存在する場合) を使用して、プロパティにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="8ad58-521">Otherwise, the expression results in a property access with the associated target expression (if any) of the property group.</span></span>

* <span data-ttu-id="8ad58-522">式が遅延バインディングされたプロパティグループ、または型が `Object` または `System.Array` である値として分類される場合、インデックス式の処理は実行時まで遅延され、インデックス作成は遅延バインディングされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-522">If the expression is classified as a late-bound property group or as a value whose type is `Object` or `System.Array`, the processing of the index expression is deferred until run time and the indexing is late-bound.</span></span> <span data-ttu-id="8ad58-523">式は、`Object` として型指定された遅延バインディングプロパティのアクセスを実行します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-523">The expression results in a late-bound property access typed as `Object`.</span></span> <span data-ttu-id="8ad58-524">関連するターゲット式は、対象の式 (値の場合)、またはプロパティグループの関連付けられた対象式のいずれかです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-524">The associated target expression is either the target expression, if it is a value, or the associated target expression of the property group.</span></span> <span data-ttu-id="8ad58-525">実行時に、式は次のように処理されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-525">At run time the expression is processed as follows:</span></span>

* <span data-ttu-id="8ad58-526">式が遅延バインディングされたプロパティグループとして分類されている場合、式は、メソッドグループ、プロパティグループ、または値 (メンバーがインスタンスまたは共有変数の場合) になることがあります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-526">If the expression is classified as a late-bound property group, the expression may result in a method group, a property group, or a value (if the member is an instance or shared variable).</span></span> <span data-ttu-id="8ad58-527">結果がメソッドグループまたはプロパティグループの場合は、オーバーロードの解決がグループに適用され、引数リストの正しいメソッドが決定されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-527">If the result is a method group or property group, overload resolution is applied to the group to determine the correct method for the argument list.</span></span> <span data-ttu-id="8ad58-528">オーバーロードの解決に失敗した場合は、@no__t 0 の例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-528">If overload resolution fails, a `System.Reflection.AmbiguousMatchException` exception is thrown.</span></span> <span data-ttu-id="8ad58-529">その後、結果はプロパティアクセスまたは呼び出しとして処理され、結果が返されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-529">Then the result is processed either as a property access or as an invocation and the result is returned.</span></span> <span data-ttu-id="8ad58-530">呼び出しがサブルーチンの場合、結果は `Nothing` になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-530">If the invocation is of a subroutine, the result is `Nothing`.</span></span>

* <span data-ttu-id="8ad58-531">対象の式の実行時の型が配列型または `System.Array` の場合、インデックス式の結果は、指定されたインデックス位置にある変数の値になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-531">If the run-time type of the target expression is an array type or `System.Array`, the result of the index expression is the value of the variable at the specified index.</span></span>

* <span data-ttu-id="8ad58-532">それ以外の場合、式の実行時の型には既定のプロパティが必要です。また、インデックスは、型のすべての既定のプロパティを表すプロパティグループで実行されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-532">Otherwise, the run-time type of the expression must have a default property and the index is performed on the property group that represents all of the default properties on the type.</span></span> <span data-ttu-id="8ad58-533">型に既定のプロパティがない場合は、`System.MissingMemberException` 例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-533">If the type has no default property, then a `System.MissingMemberException` exception is thrown.</span></span>


## <a name="new-expressions"></a><span data-ttu-id="8ad58-534">新しい式</span><span class="sxs-lookup"><span data-stu-id="8ad58-534">New Expressions</span></span>

<span data-ttu-id="8ad58-535">@No__t-0 演算子は、型の新しいインスタンスを作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-535">The `New` operator is used to create new instances of types.</span></span> <span data-ttu-id="8ad58-536">@No__t 0 の式には、次の4つの形式があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-536">There are four forms of `New` expressions:</span></span>

* <span data-ttu-id="8ad58-537">オブジェクト作成式は、クラス型と値型の新しいインスタンスを作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-537">Object-creation expressions are used to create new instances of class types and value types.</span></span>

* <span data-ttu-id="8ad58-538">配列作成式は、配列型の新しいインスタンスを作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-538">Array-creation expressions are used to create new instances of array types.</span></span>

* <span data-ttu-id="8ad58-539">デリゲート作成式 (オブジェクト作成式とは異なる構文を持たない) は、デリゲート型の新しいインスタンスを作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-539">Delegate-creation expressions (which do not have a distinct syntax from object-creation expressions) are used to create new instances of delegate types.</span></span>

* <span data-ttu-id="8ad58-540">匿名のオブジェクト作成式は、匿名クラス型の新しいインスタンスを作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-540">Anonymous object-creation expressions are used to create new instances of anonymous class types.</span></span>

```antlr
NewExpression
    : ObjectCreationExpression
    | ArrayExpression
    | AnonymousObjectCreationExpression
    ;
```

<span data-ttu-id="8ad58-541">@No__t-0 式は値として分類され、結果は型の新しいインスタンスになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-541">A `New` expression is classified as a value and the result is the new instance of the type.</span></span>


### <a name="object-creation-expressions"></a><span data-ttu-id="8ad58-542">オブジェクト作成式</span><span class="sxs-lookup"><span data-stu-id="8ad58-542">Object-Creation Expressions</span></span>

<span data-ttu-id="8ad58-543">オブジェクト作成式は、クラス型または構造体型の新しいインスタンスを作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-543">An object-creation expression is used to create a new instance of a class type or a structure type.</span></span>

```antlr
ObjectCreationExpression
    : 'New' NonArrayTypeName ( OpenParenthesis ArgumentList? CloseParenthesis )?
      ObjectCreationExpressionInitializer?
    ;

ObjectCreationExpressionInitializer
    : ObjectMemberInitializer
    | ObjectCollectionInitializer
    ;

ObjectMemberInitializer
    : 'With' OpenCurlyBrace FieldInitializerList CloseCurlyBrace
    ;

FieldInitializerList
    : FieldInitializer ( Comma FieldInitializer )*
    ;

FieldInitializer
    : 'Key'? ('.' IdentifierOrKeyword Equals )? Expression
    ;

ObjectCollectionInitializer
    : 'From' CollectionInitializer
    ;

CollectionInitializer
    : OpenCurlyBrace CollectionElementList? CloseCurlyBrace
    ;

CollectionElementList
    : CollectionElement ( Comma CollectionElement )*
    ;

CollectionElement
    : Expression
    | CollectionInitializer
    ;
```

<span data-ttu-id="8ad58-544">オブジェクト作成式の型は、クラス型、構造体型、または @no__t 0 制約を持つ型パラメーターである必要があり、`MustInherit` クラスにすることはできません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-544">The type of an object creation expression must be a class type, a structure type, or a type parameter with a `New` constraint and cannot be a `MustInherit` class.</span></span> <span data-ttu-id="8ad58-545">@No__t-0 という形式のオブジェクト作成式が指定されている場合、`T` はクラス型または構造体型であり、`A` は省略可能な引数リストです。オーバーロードの解決では、呼び出し対象の `T` の正しいコンストラクターを決定します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-545">Given an object creation expression of the form `New T(A)`, where `T` is a class type or structure type and `A` is an optional argument list, overload resolution determines the correct constructor of `T` to call.</span></span> <span data-ttu-id="8ad58-546">@No__t 0 の制約のある型パラメーターは、1つのパラメーターなしのコンストラクターを持つと見なされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-546">A type parameter with a `New` constraint is considered to have a single, parameterless constructor.</span></span> <span data-ttu-id="8ad58-547">呼び出し可能なコンストラクターがない場合、コンパイル時エラーが発生します。それ以外の場合、式は、選択したコンストラクターを使用して `T` の新しいインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-547">If no constructor is callable, a compile-time error occurs; otherwise the expression results in the creation of a new instance of `T` using the chosen constructor.</span></span> <span data-ttu-id="8ad58-548">引数がない場合は、かっこを省略できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-548">If there are no arguments, the parentheses may be omitted.</span></span>

<span data-ttu-id="8ad58-549">インスタンスが割り当てられる場所は、インスタンスがクラス型であるか値型であるかによって異なります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-549">Where an instance is allocated depends on whether the instance is a class type or a value type.</span></span> <span data-ttu-id="8ad58-550">`New` クラス型のインスタンスはシステムヒープ上に作成されますが、値型の新しいインスタンスはスタックに直接作成されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-550">`New` instances of class types are created on the system heap, while new instances of value types are created directly on the stack.</span></span>

<span data-ttu-id="8ad58-551">オブジェクト作成式では、必要に応じて、コンストラクター引数の後にメンバー初期化子のリストを指定できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-551">An object-creation expression can optionally specify a list of member initializers after the constructor arguments.</span></span> <span data-ttu-id="8ad58-552">これらのメンバー初期化子には、キーワード `With` がプレフィックスとして付けられます。また、初期化子リストは、`With` ステートメントのコンテキスト内にあるかのように解釈されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-552">These member initializers are prefixed with the keyword `With`, and the initializer list is interpreted as if it was in the context of a `With` statement.</span></span> <span data-ttu-id="8ad58-553">たとえば、次のようなクラスがあるとします。</span><span class="sxs-lookup"><span data-stu-id="8ad58-553">For example, given the class:</span></span>

```vb
Class Customer
    Dim Name As String
    Dim Address As String
End Class
```

<span data-ttu-id="8ad58-554">コード:</span><span class="sxs-lookup"><span data-stu-id="8ad58-554">The code:</span></span>

```vb
Module Test
    Sub Main()
        Dim x As New Customer() With { .Name = "Bob Smith", _
            .Address = "123 Main St." }
    End Sub
End Module
```

<span data-ttu-id="8ad58-555">は、次の場合とほぼ同じです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-555">Is roughly equivalent to:</span></span>

```vb
Module Test
    Sub Main()
        Dim x, _t1 As Customer

        _t1 = New Customer()
        With _t1
            .Name = "Bob Smith"
            .Address = "123 Main St."
        End With

        x = _t1
    End Sub
End Module
```

<span data-ttu-id="8ad58-556">各初期化子は割り当てる名前を指定する必要があり、名前は、@no__t 0 以外のインスタンス変数、または構築される型のプロパティである必要があります。構築される型が-1 @no__t 場合、メンバーアクセスは遅延バインディングされません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-556">Each initializer must specify a name to assign, and the name must be a non-`ReadOnly` instance variable or property of the type being constructed; the member access will not be late bound if the type being constructed is `Object`.</span></span> <span data-ttu-id="8ad58-557">初期化子は `Key` キーワードを使用できません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-557">Initializers may not use the `Key` keyword.</span></span> <span data-ttu-id="8ad58-558">型の各メンバーは、1回だけ初期化できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-558">Each member in a type can only be initialized once.</span></span> <span data-ttu-id="8ad58-559">ただし、初期化子式は相互に参照できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-559">The initializer expressions, however, may refer to each other.</span></span> <span data-ttu-id="8ad58-560">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-560">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim x As New Customer() With { .Name = "Bob Smith", _
            .Address = .Name & " St." }
    End Sub
End Module
```

<span data-ttu-id="8ad58-561">初期化子は左から右に割り当てられます。したがって、初期化子がまだ初期化されていないメンバーを参照している場合は、コンストラクターの実行後にインスタンス変数の値がすべて表示されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-561">The initializers are assigned left-to-right, so if an initializer refers to a member that has not been initialized yet, it will see whatever value the instance variable after the constructor ran:</span></span>

```vb
Module Test
    Sub Main()
        ' The value of Address will be " St." since Name has not been
        ' assigned yet.
        Dim x As New Customer() With { .Address = .Name & " St." }
    End Sub
End Module
```

<span data-ttu-id="8ad58-562">初期化子は入れ子にすることができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-562">Initializers can be nested:</span></span>

```vb
Class Customer
    Dim Name As String
    Dim Address As Address
    Dim Age As Integer
End Class

Class Address
    Dim Street As String
    Dim City As String
    Dim State As String
    Dim ZIP As String
End Class

Module Test
    Sub Main()
        Dim c As New Customer() With { _
            .Name = "John Smith", _
            .Address = New Address() With { _
                .Street = "23 Main St.", _
                .City = "Peoria", _
                .State = "IL", _
                .ZIP = "13934" }, _
            .Age = 34 }
    End Sub
End Module
```

<span data-ttu-id="8ad58-563">作成される型がコレクション型であり、`Add` という名前のインスタンスメソッド (拡張メソッドと共有メソッドを含む) を持っている場合、オブジェクト作成式では、キーワード `From` で始まるコレクション初期化子を指定できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-563">If the type being created is a collection type and has an instance method named `Add` (including extension methods and shared methods), then the object-creation expression can specify a collection initializer prefixed by the keyword `From`.</span></span> <span data-ttu-id="8ad58-564">オブジェクト作成式では、メンバー初期化子とコレクション初期化子の両方を指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-564">An object-creation expression cannot specify both a member initializer and a collection initializer.</span></span> <span data-ttu-id="8ad58-565">コレクション初期化子内の各要素は、`Add` 関数の呼び出しに引数として渡されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-565">Each element in the collection initializer is passed as an argument to an invocation of the `Add` function.</span></span> <span data-ttu-id="8ad58-566">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-566">For example:</span></span>

```vb
Dim list = New List(Of Integer)() From { 1, 2, 3, 4 }
```

<span data-ttu-id="8ad58-567">上の式は、下の式と同等です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-567">is equivalent to:</span></span>

```vb
Dim list = New List(Of Integer)()
list.Add(1)
list.Add(2)
list.Add(3)
```

<span data-ttu-id="8ad58-568">要素がコレクション初期化子の場合、サブコレクション初期化子の各要素は、個別の引数として @no__t 0 関数に渡されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-568">If an element is a collection initializer itself, each element of the sub-collection initializer will be passed as an individual argument to the `Add` function.</span></span> <span data-ttu-id="8ad58-569">たとえば、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-569">For example, the following:</span></span>

```vb
Dim dict = Dictionary(Of Integer, String) From { { 1, "One" },{ 2, "Two" } }
```

<span data-ttu-id="8ad58-570">上の式は、下の式と同等です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-570">is equivalent to:</span></span>

```vb
Dim dict = New Dictionary(Of Integer, String)
dict.Add(1, "One")
dict.Add(2, "Two")
```

<span data-ttu-id="8ad58-571">この拡張は常に実行され、1つのレベルの深さに対してのみ実行されます。その後、サブ初期化子は配列リテラルと見なされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-571">This expansion is always done and is only ever done one level deep; after that, sub-initializers are considered array literals.</span></span> <span data-ttu-id="8ad58-572">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-572">For example:</span></span>

```vb
' Error: List(Of T) does not have an Add method that takes two parameters.
Dim list = New List(Of Integer())() From { { 1, 2 }, { 3, 4 } }

' OK, this initializes the dictionary with (Integer, Integer()) pairs.
Dim dict = New Dictionary(Of Integer, Integer())() From _
        { {  1, { 2, 3 } }, { 3, { 4, 5 } } }
```


### <a name="array-expressions"></a><span data-ttu-id="8ad58-573">配列式</span><span class="sxs-lookup"><span data-stu-id="8ad58-573">Array Expressions</span></span>

<span data-ttu-id="8ad58-574">配列式は、配列型の新しいインスタンスを作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-574">An array expression is used to create a new instance of an array type.</span></span> <span data-ttu-id="8ad58-575">配列式には、配列作成式と配列リテラルの2種類があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-575">There are two types of array expressions: array creation expressions, and array literals.</span></span>

#### <a name="array-creation-expressions"></a><span data-ttu-id="8ad58-576">配列作成式</span><span class="sxs-lookup"><span data-stu-id="8ad58-576">Array creation expressions</span></span>

<span data-ttu-id="8ad58-577">配列サイズ初期化修飾子が指定されている場合、結果の配列型は、配列サイズ初期化引数リストから個々の引数をそれぞれ削除することによって派生されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-577">If an array size initialization modifier is supplied, the resulting array type is derived by deleting each of the individual arguments from the array size initialization argument list.</span></span> <span data-ttu-id="8ad58-578">各引数の値によって、新しく割り当てられた配列インスタンス内の対応する次元の上限が決まります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-578">The value of each argument determines the upper bound of the corresponding dimension in the newly allocated array instance.</span></span> <span data-ttu-id="8ad58-579">式に空でないコレクション初期化子がある場合、引数リストの各引数は定数である必要があります。また、式リストで指定された順位と次元の長さは、コレクション初期化子のものと一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-579">If the expression has a non-empty collection initializer, each argument in the argument list must be a constant, and the rank and dimension lengths specified by the expression list must match those of the collection initializer.</span></span>

```vb
Dim a() As Integer = New Integer(2) {}
Dim b() As Integer = New Integer(2) { 1, 2, 3 }
Dim c(,) As Integer = New Integer(1, 2) { { 1, 2, 3 } , { 4, 5, 6 } }

' Error, length/initializer mismatch.
Dim d() As Integer = New Integer(2) { 0, 1, 2, 3 }
```

<span data-ttu-id="8ad58-580">配列サイズ初期化修飾子が指定されていない場合、型名は配列型である必要があり、コレクション初期化子は空であるか、または指定された配列型のランクと同じ数の入れ子レベルを持つ必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-580">If an array size initialization modifier is not supplied, then the type name must be an array type and the collection initializer must be empty or have the same number of levels of nesting as the rank of the specified array type.</span></span> <span data-ttu-id="8ad58-581">最も内側の入れ子レベルにあるすべての要素は、配列の要素型に暗黙的に変換できる必要があり、値として分類される必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-581">All of the elements in the innermost nesting level must be implicitly convertible to the element type of the array and must be classified as a value.</span></span> <span data-ttu-id="8ad58-582">入れ子になった各コレクション初期化子の要素の数は、同じレベルの他のコレクションのサイズと常に一致している必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-582">The number of elements in each nested collection initializer must always be consistent with the size of the other collections at the same level.</span></span> <span data-ttu-id="8ad58-583">個々の次元の長さは、コレクション初期化子のそれぞれの入れ子レベルに含まれる要素の数から推論されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-583">The individual dimension lengths are inferred from the number of elements in each of the corresponding nesting levels of the collection initializer.</span></span> <span data-ttu-id="8ad58-584">コレクション初期化子が空の場合、各次元の長さは0になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-584">If the collection initializer is empty, the length of each dimension is zero.</span></span>

```vb
Dim e() As Integer = New Integer() { 1, 2, 3 }
Dim f(,) As Integer = New Integer(,) { { 1, 2, 3 } , { 4, 5, 6 } }

' Error: Inconsistent numbers of elements!
Dim g(,) As Integer = New Integer(,) { { 1, 2 }, { 4, 5, 6 } }

' Error: Inconsistent levels of nesting!
Dim h(,) As Integer = New Integer(,) { 1, 2, { 3, 4 } }
```

<span data-ttu-id="8ad58-585">コレクション初期化子の最も外側の入れ子レベルは、配列の左端の次元に対応し、最も内側の入れ子レベルが右端の次元に対応します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-585">The outermost nesting level of a collection initializer corresponds to the leftmost dimension of an array, and the innermost nesting level corresponds to the rightmost dimension.</span></span> <span data-ttu-id="8ad58-586">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-586">The example:</span></span>

```vb
Dim array As Integer(,) = _
    { { 0, 1 }, { 2, 3 }, { 4, 5 }, { 6, 7 }, { 8, 9 } }
```

<span data-ttu-id="8ad58-587">は、次の場合と同じです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-587">Is equivalent to the following:</span></span>

```vb
Dim array(4, 1) As Integer

array(0, 0) = 0: array(0, 1) = 1
array(1, 0) = 2: array(1, 1) = 3
array(2, 0) = 4: array(2, 1) = 5
array(3, 0) = 6: array(3, 1) = 7
array(4, 0) = 8: array(4, 1) = 9
```

<span data-ttu-id="8ad58-588">コレクション初期化子が空の場合 (つまり、中かっこが含まれているが初期化子リストがない場合)、初期化される配列の次元の境界がわかっていれば、空のコレクション初期化子は、指定されたサイズの配列インスタンスを表します。すべての要素が要素型の既定値に初期化されています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-588">If the collection initializer is empty (that is, one that contains curly braces but no initializer list) and the bounds of the dimensions of the array being initialized are known, the empty collection initializer represents an array instance of the specified size where all the elements have been initialized to the element type's default value.</span></span> <span data-ttu-id="8ad58-589">初期化されている配列の次元の境界が不明な場合、空のコレクション初期化子は、すべての次元のサイズがゼロである配列インスタンスを表します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-589">If the bounds of the dimensions of the array being initialized are not known, the empty collection initializer represents an array instance in which all dimensions are size zero.</span></span>

<span data-ttu-id="8ad58-590">各次元の配列インスタンスのランクと長さは、インスタンスの有効期間全体にわたって一定です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-590">An array instance's rank and length of each dimension are constant for the entire lifetime of the instance.</span></span> <span data-ttu-id="8ad58-591">つまり、既存の配列インスタンスのランクを変更することはできません。また、次元のサイズを変更することもできません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-591">In other words, it is not possible to change the rank of an existing array instance, nor is it possible to resize its dimensions.</span></span>

#### <a name="array-literals"></a><span data-ttu-id="8ad58-592">配列リテラル</span><span class="sxs-lookup"><span data-stu-id="8ad58-592">Array Literals</span></span>

<span data-ttu-id="8ad58-593">配列リテラルは、式コンテキストとコレクション初期化子の組み合わせから推論される要素型、ランク、および境界を持つ配列を表します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-593">An array literal denotes an array whose element type, rank, and bounds are inferred from a combination of the expression context and a collection initializer.</span></span> <span data-ttu-id="8ad58-594">これについては、「式の再[分類](expressions.md#expression-reclassification)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8ad58-594">This is explained in Section [Expression Reclassification](expressions.md#expression-reclassification).</span></span>

```antlr
ArrayExpression
    : ArrayCreationExpression
    | ArrayLiteralExpression
    ;

ArrayCreationExpression
    : 'New' NonArrayTypeName ArrayNameModifier CollectionInitializer
    ;

ArrayLiteralExpression
    : CollectionInitializer
    ;
```

<span data-ttu-id="8ad58-595">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-595">For example:</span></span>

```vb
' array of integers
Dim a = {1, 2, 3}

' array of shorts
Dim b = {1S, 2S, 3S}

' array of shorts whose type is taken from the context
Dim c As Short() = {1, 2, 3}

' array of type Integer(,)
Dim d = {{1, 0}, {0, 1}}

' jagged array of rank ()()
Dim e = {({1, 0}), ({0, 1})}

' error: inconsistent rank
Dim f = {{1}, {2, 3}}

' error: inconsistent rank
Dim g = {1, {2}}
```

<span data-ttu-id="8ad58-596">配列リテラルのコレクション初期化子の形式と要件は、配列作成式のコレクション初期化子とまったく同じです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-596">The format and requirements for the collection initializer in an array literal is exactly the same as that for the collection initializer in an array creation expression.</span></span>

<span data-ttu-id="8ad58-597">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="8ad58-597">__Note.__</span></span> <span data-ttu-id="8ad58-598">配列リテラルでは、配列はで作成されません。代わりに、式を値に再分類して、配列が作成されるようにします。</span><span class="sxs-lookup"><span data-stu-id="8ad58-598">An array literal does not create the array in and of itself; instead, it is the reclassification of the expression into a value that causes the array to be created.</span></span> <span data-ttu-id="8ad58-599">たとえば、`Integer()` から `Short()` への変換が行われていないため、`CType(new Integer() {1,2,3}, Short())` は変換できません。ただし、@no__t 式は、配列のリテラルを配列作成式 `New Short() {1,2,3}` に最初に分類するため、使用できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-599">For instance, the conversion `CType(new Integer() {1,2,3}, Short())` is not possible because there is no conversion from `Integer()` to `Short()`; but the expression `CType({1,2,3},Short())` is possible because it first reclassifies the array literal into the array creation expression `New Short() {1,2,3}`.</span></span>


### <a name="delegate-creation-expressions"></a><span data-ttu-id="8ad58-600">デリゲート作成式</span><span class="sxs-lookup"><span data-stu-id="8ad58-600">Delegate-Creation Expressions</span></span>

<span data-ttu-id="8ad58-601">デリゲート作成式は、デリゲート型の新しいインスタンスを作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-601">A delegate-creation expression is used to create a new instance of a delegate type.</span></span> <span data-ttu-id="8ad58-602">デリゲート作成式の引数は、メソッドポインターまたはラムダメソッドとして分類される式である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-602">The argument of a delegate-creation expression must be an expression classified as a method pointer or a lambda method.</span></span>

<span data-ttu-id="8ad58-603">引数がメソッドポインターの場合は、メソッドポインターによって参照されるメソッドの1つが、デリゲート型のシグネチャに適用可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-603">If the argument is a method pointer, one of the methods referenced by the method pointer must be applicable to the signature of the delegate type.</span></span> <span data-ttu-id="8ad58-604">次の場合、メソッド `M` はデリゲート型 @no__t に適用できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-604">A method `M` is applicable to a delegate type `D` if:</span></span>

* <span data-ttu-id="8ad58-605">`M` が-1 でないか、または本文を含んで @no__t います。</span><span class="sxs-lookup"><span data-stu-id="8ad58-605">`M` is not `Partial` or has a body.</span></span>

* <span data-ttu-id="8ad58-606">@No__t-0 と `D` の両方が関数であるか、または `D` がサブルーチンです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-606">Both `M` and `D` are functions, or `D` is a subroutine.</span></span>

* <span data-ttu-id="8ad58-607">`M` および `D` のパラメーターの数が同じです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-607">`M` and `D` have the same number of parameters.</span></span>

* <span data-ttu-id="8ad58-608">@No__t 0 のパラメーターの型はそれぞれ、`D` の対応するパラメーターの型からの変換と、その修飾子 (`ByRef`、`ByVal`) が一致します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-608">The parameter types of `M` each have a conversion from the type of the corresponding parameter type of `D`, and their modifiers (i.e. `ByRef`, `ByVal`) match.</span></span>

* <span data-ttu-id="8ad58-609">戻り値の型 `M` (存在する場合) は、戻り値の型 `D` に変換されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-609">The return type of `M`, if any, has a conversion to the return type of `D`.</span></span>

<span data-ttu-id="8ad58-610">メソッドポインターが遅延バインディングアクセスを参照する場合、遅延バインディングアクセスは、デリゲート型と同じ数のパラメーターを持つ関数と見なされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-610">If the method pointer references a late-bound access, then the late-bound access is assumed to be to a function that has the same number of parameters as the delegate type.</span></span>

<span data-ttu-id="8ad58-611">厳密なセマンティクスが使用されておらず、メソッドポインターによって参照されているメソッドが1つだけである場合、パラメーターを持たず、デリゲート型であることが原因で適用できない場合は、メソッドが適用可能であると見なされ、パラメーターまたは戻り値 a が返されます。単に無視されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-611">If strict semantics are not being used and there is only one method referenced by the method pointer, but it is not applicable due to the fact that it has no parameters and the delegate type does, then the method is considered applicable and the parameters or return value are simply ignored.</span></span> <span data-ttu-id="8ad58-612">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-612">For example:</span></span>

```vb
Delegate Sub F(x As Integer)

Module Test
    Sub M()
    End Sub

    Sub Main()
        ' Valid
        Dim x As F = AddressOf M
    End Sub
End Module
```

<span data-ttu-id="8ad58-613">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="8ad58-613">__Note.__</span></span> <span data-ttu-id="8ad58-614">この緩和は、拡張メソッドのために厳密なセマンティクスが使用されていない場合にのみ許可されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-614">This relaxation is only allowed when strict semantics are not being used because of extension methods.</span></span> <span data-ttu-id="8ad58-615">拡張メソッドは、通常のメソッドが適用されていない場合にのみ考慮されるため、デリゲートの構築のためにパラメーターを使用して拡張メソッドを非表示にするパラメーターを指定せずにインスタンスメソッドを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-615">Because extension methods are only considered if a regular method was not applicable, it is possible for an instance method with no parameters to hide an extension method with parameters for the purpose of delegate construction.</span></span>

<span data-ttu-id="8ad58-616">メソッドポインターによって参照される複数のメソッドがデリゲート型に適用可能な場合は、オーバーロードの解決を使用して候補のメソッドの中から選択します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-616">If more than one method referenced by the method pointer is applicable to the delegate type, then overload resolution is used to pick between the candidate methods.</span></span> <span data-ttu-id="8ad58-617">デリゲートに対するパラメーターの型は、オーバーロードの解決のために引数の型として使用されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-617">The types of the parameters to the delegate are used as the types of the arguments for the purposes of overload resolution.</span></span> <span data-ttu-id="8ad58-618">1つのメソッド候補が最も適していない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-618">If no one method candidate is most applicable, a compile-time error occurs.</span></span> <span data-ttu-id="8ad58-619">次の例では、ローカル変数は、2番目の `Square` メソッドを参照するデリゲートを使用して初期化されています。これは、そのメソッドが @no__t のシグネチャと戻り値の型に適用可能であるためです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-619">In the following example, the local variable is initialized with a delegate that refers to the second `Square` method because that method is more applicable to the signature and return type of `DoubleFunc`.</span></span>

```vb
Delegate Function DoubleFunc(x As Double) As Double

Module Test
    Function Square(x As Single) As Single
        Return x * x
    End Function 

    Function Square(x As Double) As Double
        Return x * x
    End Function

    Sub Main()
        Dim a As New DoubleFunc(AddressOf Square)
    End Sub
End Module
```

<span data-ttu-id="8ad58-620">2番目の `Square` メソッドが存在しませんでした。最初の `Square` メソッドが選択されています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-620">Had the second `Square` method not been present, the first `Square` method would have been chosen.</span></span> <span data-ttu-id="8ad58-621">厳密なセマンティクスがコンパイル環境で指定されている場合、または `Option Strict` の場合、メソッドポインターによって参照される最も具体的なメソッドがデリゲートシグネチャよりも狭い場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-621">If strict semantics are specified by the compilation environment or by `Option Strict`, then a compile-time error occurs if the most specific method referenced by the method pointer is narrower than the delegate signature.</span></span> <span data-ttu-id="8ad58-622">次の場合、メソッド `M` はデリゲート型よりも狭くなります `D` です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-622">A method `M` is considered narrower than a delegate type `D` if:</span></span>

* <span data-ttu-id="8ad58-623">@No__t-0 のパラメーターの型には、対応するパラメーターの型 `D` への拡大変換が含まれています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-623">A parameter type of `M` has a widening conversion to the corresponding parameter type of `D`.</span></span>

* <span data-ttu-id="8ad58-624">または、戻り値の型 (存在する場合) がある場合は、戻り値の型 `D` の縮小変換が @no__t ます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-624">Or, the return type, if any, of `M` has a narrowing conversion to the return type of `D`.</span></span>

<span data-ttu-id="8ad58-625">型引数がメソッドポインターに関連付けられている場合、同じ数の型引数を持つメソッドだけが考慮されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-625">If type arguments are associated with the method pointer, only methods with the same number of type arguments are considered.</span></span> <span data-ttu-id="8ad58-626">型引数がメソッドポインターに関連付けられていない場合は、ジェネリックメソッドに対して署名を照合するときに、型の推定が使用されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-626">If no type arguments are associated with the method pointer, type inference is used when matching signatures against a generic method.</span></span> <span data-ttu-id="8ad58-627">他の通常の型の推論とは異なり、デリゲートの戻り値の型は型引数を推論するときに使用されますが、最も一般的でないオーバーロードを決定するときに戻り値の型は考慮されません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-627">Unlike other normal type inference, the return type of the delegate is used when inferring type arguments, but return types are still not considered when determining the least generic overload.</span></span> <span data-ttu-id="8ad58-628">次の例では、デリゲート作成式に型引数を指定する両方の方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-628">The following example shows both ways of supplying a type argument to a delegate-creation expression:</span></span>

```vb
Delegate Function D(s As String, i As Integer) As Integer
Delegate Function E() As Integer

Module Test
    Public Function F(Of T)(s As String, t1 As T) As T
    End Function

    Public Function G(Of T)() As T
    End Function

    Sub Main()
        Dim d1 As D = AddressOf f(Of Integer)    ' OK, type arg explicit
        Dim d2 As D = AddressOf f                ' OK, type arg inferred

        Dim e1 As E = AddressOf g(Of Integer)    ' OK, type arg explicit
        Dim e2 As E = AddressOf g                ' OK, infer from return
  End Sub
End Module
```

<span data-ttu-id="8ad58-629">上記の例では、ジェネリックメソッドを使用して非ジェネリックデリゲート型がインスタンス化されています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-629">In the above example, a non-generic delegate type was instantiated using a generic method.</span></span> <span data-ttu-id="8ad58-630">ジェネリックメソッドを使用して、構築されたデリゲート型のインスタンスを作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-630">It is also possible to create an instance of a constructed delegate type using a generic method.</span></span> <span data-ttu-id="8ad58-631">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-631">For example:</span></span>

```vb
Delegate Function Predicate(Of U)(u1 As U, u2 As U) As Boolean

Module Test
    Function Compare(Of T)(t1 As List(of T), t2 As List(of T)) As Boolean
        ...
    End Function

    Sub Main()
        Dim p As Predicate(Of List(Of Integer))
        p = AddressOf Compare(Of Integer)
    End Sub
End Module
```

<span data-ttu-id="8ad58-632">デリゲート作成式の引数がラムダメソッドの場合は、ラムダメソッドがデリゲート型のシグネチャに適用可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-632">If the argument to the delegate-creation expression is a lambda method, the lambda method must be applicable to the signature of the delegate type.</span></span> <span data-ttu-id="8ad58-633">次の場合は、ラムダメソッド `L` がデリゲート型に適用されます-1 @no__t。</span><span class="sxs-lookup"><span data-stu-id="8ad58-633">A lambda method `L` is applicable to a delegate type `D` if:</span></span>

* <span data-ttu-id="8ad58-634">@No__t-0 にパラメーターがある場合、`D` のパラメーターの数は同じになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-634">If `L` has parameters, `D` has the same number of parameters.</span></span> <span data-ttu-id="8ad58-635">(@No__t-0 にパラメーターがない場合、`D` のパラメーターは無視されます)。</span><span class="sxs-lookup"><span data-stu-id="8ad58-635">(If `L` has no parameters, the parameters of `D` are ignored.)</span></span>

* <span data-ttu-id="8ad58-636">@No__t 0 のパラメーターの型はそれぞれ、`D` の対応するパラメーターの型に変換され、その修飾子 (`ByRef`、`ByVal`) が一致します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-636">The parameter types of `L` each have a conversion to the type of the corresponding parameter type of `D`, and their modifiers (i.e. `ByRef`, `ByVal`) match.</span></span>

* <span data-ttu-id="8ad58-637">@No__t-0 が関数の場合、@no__t の戻り値の型は `D` の戻り値の型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-637">If `D` is a function, the return type of `L` has a conversion to the return type of `D`.</span></span> <span data-ttu-id="8ad58-638">(@No__t-0 がサブルーチンの場合、`L` の戻り値は無視されます)。</span><span class="sxs-lookup"><span data-stu-id="8ad58-638">(If `D` is a subroutine, the return value of `L` is ignored.)</span></span>

<span data-ttu-id="8ad58-639">@No__t-0 のパラメーターの型が省略されている場合は、`D` の対応するパラメーターの型が推論されます。`L` のパラメーターに配列または null 許容の名前修飾子がある場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-639">If the parameter type of a parameter of `L` is omitted, then the type of the corresponding parameter in `D` is inferred; if the parameter of `L` has array or nullable name modifiers, a compile-time error results.</span></span> <span data-ttu-id="8ad58-640">@No__t-0 のすべてのパラメーターの型を使用できるようになると、ラムダメソッド内の式の型が推論されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-640">Once all of the parameter types of `L` are available, then the type of the expression in the lambda method is inferred.</span></span> <span data-ttu-id="8ad58-641">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-641">For example:</span></span>

```vb
Delegate Function F(x As Integer, y As Long) As Long

Module Test
    Sub Main()
        ' b inferred to Integer, c and return type inferred to Long
        Dim a As F = Function(b, c) b + c

        ' e and return type inferred to Integer, f inferred to Long
        Dim d As F = Function(e, f) e + CInt(f)
    End Sub
End Module
```

<span data-ttu-id="8ad58-642">デリゲートシグネチャがラムダメソッドまたはメソッドシグネチャと完全に一致しない場合、.NET Framework はデリゲートの作成をネイティブでサポートしていないことがあります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-642">In some situations where delegate signature does not exactly match the lambda method or method signature, the .NET Framework may not support the delegate creation natively.</span></span> <span data-ttu-id="8ad58-643">そのような場合は、ラムダメソッド式を使用して2つのメソッドを照合します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-643">In that situation, a lambda method expression is used to match the two methods.</span></span> <span data-ttu-id="8ad58-644">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-644">For example:</span></span>

```vb
Delegate Function IntFunc(x As Integer) As Integer

Module Test
    Function SquareString(x As String) As String
        Return CInt(x) * CInt(x)
    End Function 

    Sub Main()
        ' The following two lines are equivalent
        Dim a As New IntFunc(AddressOf SquareString)
        Dim b As New IntFunc( _
            Function(x As Integer) CInt(SquareString(CStr(x))))
    End Sub
End Module
```

<span data-ttu-id="8ad58-645">デリゲート作成式の結果は、メソッドポインター式から、関連付けられている対象式 (存在する場合) と一致するメソッドを参照するデリゲートインスタンスです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-645">The result of a delegate-creation expression is a delegate instance that refers to the matching method with the associated target expression (if any) from the method pointer expression.</span></span> <span data-ttu-id="8ad58-646">ターゲット式が値型として型指定されている場合、デリゲートはヒープ上のオブジェクトのメソッドを指すことしかできないため、値型はシステムヒープにコピーされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-646">If the target expression is typed as a value type, then the value type is copied onto the system heap because a delegate can only point to a method of an object on the heap.</span></span> <span data-ttu-id="8ad58-647">デリゲートが参照するメソッドとオブジェクトは、デリゲートの有効期間全体にわたって定数を保持します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-647">The method and object to which a delegate refers remain constant for the entire lifetime of the delegate.</span></span> <span data-ttu-id="8ad58-648">つまり、デリゲートを作成した後に、そのデリゲートのターゲットまたはオブジェクトを変更することはできません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-648">In other words, it is not possible to change the target or object of a delegate after it has been created.</span></span>

### <a name="anonymous-object-creation-expressions"></a><span data-ttu-id="8ad58-649">匿名オブジェクト作成式</span><span class="sxs-lookup"><span data-stu-id="8ad58-649">Anonymous Object-Creation Expressions</span></span>

<span data-ttu-id="8ad58-650">メンバー初期化子を持つオブジェクト作成式では、型名を完全に省略することもできます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-650">An object-creation expression with member initializers can also omit the type name entirely.</span></span>

```antlr
AnonymousObjectCreationExpression
    : 'New' ObjectMemberInitializer
    ;
```

<span data-ttu-id="8ad58-651">その場合、匿名型は、式の一部として初期化されたメンバーの型と名前に基づいて作成されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-651">In that case, an anonymous type is constructed based on the types and names of the members initialized as a part of the expression.</span></span> <span data-ttu-id="8ad58-652">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-652">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim Customer = New With { .Name = "John Smith", .Age = 34 }

        Console.WriteLine(Customer.Name)
    End Sub
End Module
```

<span data-ttu-id="8ad58-653">匿名オブジェクト作成式によって作成される型は、名前のないクラスであり、@no__t から直接継承されます。また、メンバーの初期化子リスト内のに割り当てられているメンバーと同じ名前のプロパティのセットがあります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-653">The type created by an anonymous object-creation expression is a class that has no name, inherits directly from `Object`, and has a set of properties with the same name as the members assigned to in the member initializer list.</span></span> <span data-ttu-id="8ad58-654">各プロパティの型は、ローカル変数の型の推論と同じ規則を使用して推論されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-654">The type of each property is inferred using the same rules as local variable type inference.</span></span> <span data-ttu-id="8ad58-655">生成された匿名型も `ToString` をオーバーライドし、すべてのメンバーとその値の文字列形式を返します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-655">Generated anonymous types also override `ToString`, returning a string representation of all members and their values.</span></span> <span data-ttu-id="8ad58-656">(この文字列の正確な形式は、この仕様の範囲を超えています)。</span><span class="sxs-lookup"><span data-stu-id="8ad58-656">(The exact format of this string is beyond the scope of this specification).</span></span>

<span data-ttu-id="8ad58-657">既定では、匿名型によって生成されるプロパティは、読み取り/書き込み可能です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-657">By default, the properties generated by the anonymous type are read-write.</span></span> <span data-ttu-id="8ad58-658">@No__t-0 修飾子を使用して、匿名型プロパティを読み取り専用としてマークすることができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-658">It is possible to mark an anonymous type property as read-only by using the `Key` modifier.</span></span> <span data-ttu-id="8ad58-659">@No__t-0 修飾子は、匿名型が表す値を一意に識別するためにフィールドを使用できることを指定します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-659">The `Key` modifier specifies that the field can be used to uniquely identify the value the anonymous type represents.</span></span> <span data-ttu-id="8ad58-660">プロパティを読み取り専用にするだけでなく、匿名型によって `Equals` と @no__t がオーバーライドされ、インターフェイス `System.IEquatable(Of T)` が実装されます (`T` の匿名型を入力します)。</span><span class="sxs-lookup"><span data-stu-id="8ad58-660">In addition to making the property read-only, it also causes the anonymous type to override `Equals` and  `GetHashCode` and to implement the interface `System.IEquatable(Of T)` (filling in the anonymous type for `T`).</span></span> <span data-ttu-id="8ad58-661">メンバーは次のように定義されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-661">The members are defined as follows:</span></span>

<span data-ttu-id="8ad58-662">`Function Equals(obj As Object) As Boolean` および `Function Equals(val As T) As Boolean` は、2つのインスタンスが同じ型であることを検証し、`Object.Equals` を使用して各 `Key` メンバーを比較することによって実装されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-662">`Function Equals(obj As Object) As Boolean` and `Function Equals(val As T) As Boolean` are implemented by validating that the two instances are of the same type and then comparing each `Key` member using `Object.Equals`.</span></span> <span data-ttu-id="8ad58-663">すべての `Key` メンバーが等しい場合、`Equals` は `True` を返します。それ @no__t 以外の場合は、-@no__t 3 を返します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-663">If all `Key` members are equal, then `Equals` returns `True`, otherwise `Equals` returns `False`.</span></span>

<span data-ttu-id="8ad58-664">`Function GetHashCode() As Integer` が実装されている場合、匿名型の2つのインスタンスに対して `Equals` が true の場合、`GetHashCode` は同じ値を返します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-664">`Function GetHashCode() As Integer` is implemented such that that if `Equals` is true for two instances of the anonymous type, then `GetHashCode` will return the same value.</span></span> <span data-ttu-id="8ad58-665">ハッシュはシード値で始まり、`Key` の各メンバーについて、ハッシュを31で乗算し、メンバーが参照型ではない場合は `Key` メンバーのハッシュ値 (`GetHashCode`) を加算し、`Nothing` の値を持つ null 許容値型ではなくなります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-665">The hash starts with a seed value and then, for each `Key` member, in order multiplies the hash by 31 and adds the `Key` member's hash value (provided by `GetHashCode`) if the member is not a reference type or nullable value type with the value of `Nothing`.</span></span>

<span data-ttu-id="8ad58-666">たとえば、ステートメントで作成された型は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-666">For example, the type created in the statement:</span></span>

```vb
Dim zipState = New With { Key .ZipCode = 98112, .State = "WA" }
```

<span data-ttu-id="8ad58-667">次のようなクラスを作成します (厳密な実装は異なる場合もあります)。</span><span class="sxs-lookup"><span data-stu-id="8ad58-667">creates a class that looks approximately like this (although exact implementation may vary):</span></span>

```vb
Friend NotInheritable Class $Anonymous1
    Implements IEquatable(Of $Anonymous1)

    Private ReadOnly _zipCode As Integer
    Private _state As String

    Public Sub New(zipCode As Integer, state As String)
        _zipCode = zipcode
        _state = state
    End Sub

    Public ReadOnly Property ZipCode As Integer
        Get
            Return _zipCode
        End Get
    End Property

    Public Property State As String
        Get
            Return _state
        End Get
        Set (value As Integer)
            _state = value
        End Set
    End Property

    Public Overrides Function Equals(obj As Object) As Boolean
        Dim val As $Anonymous1 = TryCast(obj, $Anonymous1)
        Return Equals(val)
    End Function

    Public Overloads Function Equals(val As $Anonymous1) As Boolean _
        Implements IEquatable(Of $Anonymous1).Equals

        If val Is Nothing Then 
            Return False
        End If

        If Not Object.Equals(_zipCode, val._zipCode) Then 
            Return False
        End If

        Return True
    End Function

    Public Overrides Function GetHashCode() As Integer
        Dim hash As Integer = 0

        hash = hash Xor _zipCode.GetHashCode()

        Return hash
    End Function

    Public Overrides Function ToString() As String
        Return "{ Key .ZipCode = " & _zipCode & ", .State = " & _state & " }"
    End Function
End Class
```

<span data-ttu-id="8ad58-668">匿名型が別の型のフィールドから作成される状況を簡単にするために、次の場合に式からフィールド名を直接推論できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-668">To simplify the situation where an anonymous type is created from the fields of another type, field names can be inferred directly from expressions in the following cases:</span></span>

* <span data-ttu-id="8ad58-669">単純な名前の式 `x` は、名前 `x` を推測します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-669">A simple name expression `x` infers the name `x`.</span></span>

* <span data-ttu-id="8ad58-670">メンバーアクセス式 `x.y` は、名前 `y` を推測します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-670">A member access expression `x.y` infers the name `y`.</span></span>

* <span data-ttu-id="8ad58-671">ディクショナリ参照式 `x!y` は、名前 `y` を推測します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-671">A dictionary lookup expression `x!y` infers the name `y`.</span></span>

* <span data-ttu-id="8ad58-672">引数のない呼び出しまたはインデックス式 `x()` は名前 `x` を推測します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-672">An invocation or index expression with no arguments `x()` infers the name `x`.</span></span>

* <span data-ttu-id="8ad58-673">XML メンバーアクセス式 `x.<y>`、`x...<y>`、`x.@y` は、名前 `y` を推測します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-673">An XML member access expression `x.<y>`, `x...<y>`, `x.@y` infers the name `y`.</span></span>

* <span data-ttu-id="8ad58-674">メンバーアクセス式のターゲットである XML メンバーアクセス式 `x.<y>.z` は、名前 `z` を推測します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-674">An XML member access expression that is the target of a member access expression `x.<y>.z` infers the name `z`.</span></span>

* <span data-ttu-id="8ad58-675">引数のない呼び出しまたはインデックス式のターゲットである XML メンバーアクセス式 `x.<y>.z()` は名前 `z` を推測します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-675">An XML member access expression that is the target of an invocation or index expression with no arguments `x.<y>.z()` infers the name `z`.</span></span>

* <span data-ttu-id="8ad58-676">呼び出しまたはインデックス式のターゲットである XML メンバーアクセス式 `x.<y>(0)` は、名前 `y` を推測します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-676">An XML member access expression that is the target of an invocation or index expression `x.<y>(0)` infers the name `y`.</span></span>

<span data-ttu-id="8ad58-677">初期化子は、推定名への式の代入として解釈されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-677">The initializer is interpreted as an assignment of the expression to the inferred name.</span></span> <span data-ttu-id="8ad58-678">たとえば、次の初期化子は同等です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-678">For example, the following initializers are equivalent:</span></span>

```vb
Class Address
    Public Street As String
    Public City As String
    Public State As String
    Public ZIP As String
End Class

Class C1
    Sub Test(a As Address)
        Dim cityState1 = New With { .City = a.City, .State = a.State }
        Dim cityState2 = New With { a.City, a.State }
    End Sub
End Class
```

<span data-ttu-id="8ad58-679">@No__t-0 など、型の既存のメンバーと競合するメンバー名が推論されると、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-679">If a member name is inferred that conflicts with an existing member of the type, such as `GetHashCode`, then a compile time error occurs.</span></span> <span data-ttu-id="8ad58-680">通常のメンバー初期化子とは異なり、匿名のオブジェクト作成式では、メンバー初期化子が循環参照を持つことを許可したり、メンバーを初期化する前に参照したりすることはできません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-680">Unlike regular member initializers, anonymous object-creation expressions do not allow member initializers to have circular references, or to refer to a member before it has been initialized.</span></span> <span data-ttu-id="8ad58-681">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-681">For example:</span></span>

```vb
Module Test
    Sub Main()
        ' Error: Circular references
        Dim x = New With { .a = .b, .b = .a }

        ' Error: Referring to .b before it has been assigned to
        Dim y = New With { .a = .b, .b = 10 }

        ' Error: Referring to .a before it has been assigned to
        Dim z = New With { .a = .a }
    End Sub
End Module
```

<span data-ttu-id="8ad58-682">2つの匿名クラス作成式が同じメソッド内で発生し、結果として得られる同じ構造を生成する場合、プロパティの順序、プロパティ名、およびプロパティの型がすべて一致すると、両方とも同じ匿名クラスを参照します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-682">If two anonymous class creation expressions occur within the same method and yield the same resulting shape -- if the property order, property names, and property types all match -- they will both refer to the same anonymous class.</span></span> <span data-ttu-id="8ad58-683">初期化子を使用したインスタンスまたは共有メンバー変数のメソッドスコープは、変数が初期化されるコンストラクターです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-683">The method scope of an instance or shared member variable with an initializer is the constructor in which the variable is initialized.</span></span>

<span data-ttu-id="8ad58-684">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="8ad58-684">__Note.__</span></span> <span data-ttu-id="8ad58-685">コンパイラは、アセンブリレベルなどで、匿名型をさらに統合することができますが、現時点ではこのような型に依存することはできません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-685">It is possible that a compiler may choose to unify anonymous types further, such as at the assembly level, but this cannot be relied upon at this time.</span></span>


## <a name="cast-expressions"></a><span data-ttu-id="8ad58-686">キャスト式</span><span class="sxs-lookup"><span data-stu-id="8ad58-686">Cast Expressions</span></span>

<span data-ttu-id="8ad58-687">キャスト式は、式を指定された型に強制的に変換します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-687">A cast expression coerces an expression to a given type.</span></span> <span data-ttu-id="8ad58-688">特定の cast キーワードは、プリミティブ型に式を強制的に変換します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-688">Specific cast keywords coerce expressions into the primitive types.</span></span> <span data-ttu-id="8ad58-689">3つの一般的なキャストキーワード (@no__t 0、`TryCast`、`DirectCast`) は、式を型に強制します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-689">Three general cast keywords, `CType`, `TryCast` and `DirectCast`, coerce an expression into a type.</span></span>

```antlr
CastExpression
    : 'DirectCast' OpenParenthesis Expression Comma TypeName CloseParenthesis
    | 'TryCast' OpenParenthesis Expression Comma TypeName CloseParenthesis
    | 'CType' OpenParenthesis Expression Comma TypeName CloseParenthesis
    | CastTarget OpenParenthesis Expression CloseParenthesis
    ;

CastTarget
    : 'CBool' | 'CByte' | 'CChar'  | 'CDate'  | 'CDec' | 'CDbl' | 'CInt'
    | 'CLng'  | 'CObj'  | 'CSByte' | 'CShort' | 'CSng' | 'CStr' | 'CUInt'
    | 'CULng' | 'CUShort'
    ;
```

<span data-ttu-id="8ad58-690">`DirectCast` および `TryCast` には特殊な動作があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-690">`DirectCast` and `TryCast` have special behaviors.</span></span> <span data-ttu-id="8ad58-691">このため、ネイティブ変換のみがサポートされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-691">Because of this, they only support native conversions.</span></span> <span data-ttu-id="8ad58-692">また、`TryCast` 式の対象の型を値型にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-692">Additionally, the target type in a `TryCast` expression cannot be a value type.</span></span> <span data-ttu-id="8ad58-693">@No__t-0 または `TryCast` が使用されている場合、ユーザー定義の変換演算子は考慮されません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-693">User-defined conversion operators are not considered when `DirectCast` or `TryCast` is used.</span></span> <span data-ttu-id="8ad58-694">(__注:__</span><span class="sxs-lookup"><span data-stu-id="8ad58-694">(__Note.__</span></span> <span data-ttu-id="8ad58-695">-0 および `TryCast` のサポート @no__t する変換セットは、"ネイティブ CLR" 変換を実装するため、制限されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-695">The conversion set that `DirectCast` and `TryCast` support are restricted because they implement "native CLR" conversions.</span></span> <span data-ttu-id="8ad58-696">@No__t-0 の目的は、"ボックス化解除" 命令の機能を提供することです。一方、`TryCast` の目的は、"isinst" 命令の機能を提供することです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-696">The purpose of `DirectCast` is to provide the functionality of the "unbox" instruction, while the purpose of `TryCast` is to provide the functionality of the "isinst" instruction.</span></span> <span data-ttu-id="8ad58-697">Clr 命令にマップされるため、CLR によって直接サポートされていない変換をサポートすると、意図した目的が損なわれます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-697">Since they map onto CLR instructions, supporting conversions not directly supported by the CLR would defeat the intended purpose.)</span></span>

<span data-ttu-id="8ad58-698">`DirectCast` @no__t として型指定された式を `CType` とは異なる方法で変換します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-698">`DirectCast` converts expressions that are typed as `Object` differently than `CType`.</span></span> <span data-ttu-id="8ad58-699">@No__t-0 型の式を変換するときに、実行時の型がプリミティブ値型である場合、`DirectCast` は、指定された型が式の実行時の型と異なる場合は `System.InvalidCastException` の例外をスローし、式が @no__t に評価される場合は `System.NullReferenceException` をスローします。</span><span class="sxs-lookup"><span data-stu-id="8ad58-699">When converting an expression of type `Object` whose run-time type is a primitive value type, `DirectCast` throws a `System.InvalidCastException` exception if the specified type is not the same as the run-time type of the expression or a `System.NullReferenceException` if the expression evaluates to `Nothing`.</span></span> <span data-ttu-id="8ad58-700">(__注:__</span><span class="sxs-lookup"><span data-stu-id="8ad58-700">(__Note.__</span></span> <span data-ttu-id="8ad58-701">前述のように、式の型が-1 @no__t 場合、`DirectCast` は CLR 命令に直接マップされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-701">As noted above, `DirectCast` maps directly onto the CLR instruction "unbox" when the type of the expression is `Object`.</span></span> <span data-ttu-id="8ad58-702">これに対し、`CType` は、プリミティブ型間の変換がサポートされるように、ランタイムヘルパーを呼び出して変換を実行します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-702">In contrast, `CType` turns into a call to a runtime helper to do the conversion so that conversions between primitive types can be supported.</span></span> <span data-ttu-id="8ad58-703">@No__t 0 式がプリミティブ値型に変換され、実際のインスタンスの型がターゲット型と一致する場合、`DirectCast` は `CType` より大幅に高速になります)。</span><span class="sxs-lookup"><span data-stu-id="8ad58-703">In the case when an `Object` expression is being converted to a primitive value type and the type of the actual instance match the target type, `DirectCast` will be significantly faster than `CType`.)</span></span>

<span data-ttu-id="8ad58-704">`TryCast` は式を変換しますが、式を対象の型に変換できない場合は例外をスローしません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-704">`TryCast` converts expressions but does not throw an exception if the expression cannot be converted to the target type.</span></span> <span data-ttu-id="8ad58-705">代わりに、`TryCast` を指定すると、実行時に式を変換できない場合は `Nothing` になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-705">Instead, `TryCast` will result in `Nothing` if the expression cannot be converted at runtime.</span></span> <span data-ttu-id="8ad58-706">(__注:__</span><span class="sxs-lookup"><span data-stu-id="8ad58-706">(__Note.__</span></span> <span data-ttu-id="8ad58-707">前述のように、`TryCast` は CLR 命令 "isinst" に直接マップされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-707">As noted above, `TryCast` maps directly onto the CLR instruction "isinst".</span></span> <span data-ttu-id="8ad58-708">型チェックと変換を組み合わせて1つの操作を行うことにより、`TryCast` は、`TypeOf ... Is` と `CType` を実行するよりも低コストになります)。</span><span class="sxs-lookup"><span data-stu-id="8ad58-708">By combining the type check and the conversion into a single operation, `TryCast` can be cheaper than doing a `TypeOf ... Is` and then a `CType`.)</span></span>

<span data-ttu-id="8ad58-709">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-709">For example:</span></span>

```vb
Interface ITest
    Sub Test()
End Interface

Module Test
    Sub Convert(o As Object)
        Dim i As ITest = TryCast(o, ITest)

        If i IsNot Nothing Then
            i.Test()
        End If
    End Sub
End Module
```

<span data-ttu-id="8ad58-710">式の型から指定された型への変換が存在しない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-710">If no conversion exists from the type of the expression to the specified type, a compile-time error occurs.</span></span> <span data-ttu-id="8ad58-711">それ以外の場合、式は値として分類され、結果は変換によって生成される値になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-711">Otherwise, the expression is classified as a value and the result is the value produced by the conversion.</span></span>


## <a name="operator-expressions"></a><span data-ttu-id="8ad58-712">演算子式</span><span class="sxs-lookup"><span data-stu-id="8ad58-712">Operator Expressions</span></span>

<span data-ttu-id="8ad58-713">演算子には、次の2種類があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-713">There are two kinds of operators.</span></span> <span data-ttu-id="8ad58-714">*単項演算子*は1つのオペランドを受け取り、プレフィックス表記を使用します (たとえば、`-x`)。</span><span class="sxs-lookup"><span data-stu-id="8ad58-714">*Unary operators* take one operand and use prefix notation (for example, `-x`).</span></span> <span data-ttu-id="8ad58-715">*二項演算子*は2つのオペランドを受け取り、挿入辞表記を使用します (たとえば、`x + y`)。</span><span class="sxs-lookup"><span data-stu-id="8ad58-715">*Binary operators* take two operands and use infix notation (for example, `x + y`).</span></span> <span data-ttu-id="8ad58-716">常に @no__t 0 になる関係演算子を除き、特定の型に対して定義された演算子はその型になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-716">With the exception of the relational operators, which always result in `Boolean`, an operator defined for a particular type results in that type.</span></span> <span data-ttu-id="8ad58-717">演算子のオペランドは、常に値として分類される必要があります。演算子式の結果は、値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-717">The operands to an operator must always be classified as a value; the result of an operator expression is classified as a value.</span></span>

```antlr
OperatorExpression
    : ArithmeticOperatorExpression
    | RelationalOperatorExpression
    | LikeOperatorExpression
    | ConcatenationOperatorExpression
    | ShortCircuitLogicalOperatorExpression
    | LogicalOperatorExpression
    | ShiftOperatorExpression
    | AwaitOperatorExpression
    ;
```

### <a name="operator-precedence-and-associativity"></a><span data-ttu-id="8ad58-718">演算子の優先順位と結合規則</span><span class="sxs-lookup"><span data-stu-id="8ad58-718">Operator Precedence and Associativity</span></span>

<span data-ttu-id="8ad58-719">式に複数の二項演算子が含まれている場合、演算子の*優先順位*によって、個々の二項演算子の評価順序が制御されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-719">When an expression contains multiple binary operators, the *precedence* of the operators controls the order in which the individual binary operators are evaluated.</span></span> <span data-ttu-id="8ad58-720">たとえば、式 `x + y * z` の評価は `x + (y * z)` ですが、これは `*` 演算子が `+` 演算子より高い優先順位だからです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-720">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the `+` operator.</span></span> <span data-ttu-id="8ad58-721">次の表は、二項演算子を優先順位の降順で示しています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-721">The following table lists the binary operators in descending order of precedence:</span></span>


| <span data-ttu-id="8ad58-722">__カテゴリ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-722">__Category__</span></span>     | <span data-ttu-id="8ad58-723">__演算子__</span><span class="sxs-lookup"><span data-stu-id="8ad58-723">__Operators__</span></span>                                          | 
|------------------|--------------------------------------------------------|
| <span data-ttu-id="8ad58-724">1 次式</span><span class="sxs-lookup"><span data-stu-id="8ad58-724">Primary</span></span>          | <span data-ttu-id="8ad58-725">すべての非演算子式</span><span class="sxs-lookup"><span data-stu-id="8ad58-725">All non-operator expressions</span></span>                           |
| <span data-ttu-id="8ad58-726">Await</span><span class="sxs-lookup"><span data-stu-id="8ad58-726">Await</span></span>            | `Await`                                                |
| <span data-ttu-id="8ad58-727">累乗</span><span class="sxs-lookup"><span data-stu-id="8ad58-727">Exponentiation</span></span>   | `^`                                                    |
| <span data-ttu-id="8ad58-728">単項マイナス符号</span><span class="sxs-lookup"><span data-stu-id="8ad58-728">Unary negation</span></span>   | <span data-ttu-id="8ad58-729">`+`, `-`</span><span class="sxs-lookup"><span data-stu-id="8ad58-729">`+`, `-`</span></span>                                               |
| <span data-ttu-id="8ad58-730">乗法</span><span class="sxs-lookup"><span data-stu-id="8ad58-730">Multiplicative</span></span>   | <span data-ttu-id="8ad58-731">`*`, `/`</span><span class="sxs-lookup"><span data-stu-id="8ad58-731">`*`, `/`</span></span>                                               |
| <span data-ttu-id="8ad58-732">整数の除算</span><span class="sxs-lookup"><span data-stu-id="8ad58-732">Integer division</span></span> | `\`                                                    |
| <span data-ttu-id="8ad58-733">剰余</span><span class="sxs-lookup"><span data-stu-id="8ad58-733">Modulus</span></span>          | `Mod`                                                  |
| <span data-ttu-id="8ad58-734">加法</span><span class="sxs-lookup"><span data-stu-id="8ad58-734">Additive</span></span>         | <span data-ttu-id="8ad58-735">`+`, `-`</span><span class="sxs-lookup"><span data-stu-id="8ad58-735">`+`, `-`</span></span>                                               |
| <span data-ttu-id="8ad58-736">連結</span><span class="sxs-lookup"><span data-stu-id="8ad58-736">Concatenation</span></span>    | `&`                                                    |
| <span data-ttu-id="8ad58-737">Shift</span><span class="sxs-lookup"><span data-stu-id="8ad58-737">Shift</span></span>            | <span data-ttu-id="8ad58-738">`<<`, `>>`</span><span class="sxs-lookup"><span data-stu-id="8ad58-738">`<<`, `>>`</span></span>                                             |
| <span data-ttu-id="8ad58-739">関係</span><span class="sxs-lookup"><span data-stu-id="8ad58-739">Relational</span></span>       | <span data-ttu-id="8ad58-740">`=`, `<>`, `<`, `>`, `<=`, `>=`, `Like`, `Is`, `IsNot`</span><span class="sxs-lookup"><span data-stu-id="8ad58-740">`=`, `<>`, `<`, `>`, `<=`, `>=`, `Like`, `Is`, `IsNot`</span></span> |
| <span data-ttu-id="8ad58-741">論理 NOT</span><span class="sxs-lookup"><span data-stu-id="8ad58-741">Logical NOT</span></span>      | `Not`                                                  |
| <span data-ttu-id="8ad58-742">論理 AND</span><span class="sxs-lookup"><span data-stu-id="8ad58-742">Logical AND</span></span>      | <span data-ttu-id="8ad58-743">`And`, `AndAlso`</span><span class="sxs-lookup"><span data-stu-id="8ad58-743">`And`, `AndAlso`</span></span>                                       |
| <span data-ttu-id="8ad58-744">論理 OR</span><span class="sxs-lookup"><span data-stu-id="8ad58-744">Logical OR</span></span>       | <span data-ttu-id="8ad58-745">`Or`, `OrElse`</span><span class="sxs-lookup"><span data-stu-id="8ad58-745">`Or`, `OrElse`</span></span>                                         |
| <span data-ttu-id="8ad58-746">論理 XOR</span><span class="sxs-lookup"><span data-stu-id="8ad58-746">Logical XOR</span></span>      | `Xor`                                                  |

<span data-ttu-id="8ad58-747">同じ優先順位を持つ2つの演算子が式に含まれている場合、演算子の*結合規則*によって、操作の実行順序が制御されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-747">When an expression contains two operators with the same precedence, the *associativity* of the operators controls the order in which the operations are performed.</span></span> <span data-ttu-id="8ad58-748">すべての二項演算子は左から右へと結合されます。つまり、操作は左から右に実行されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-748">All binary operators are left-associative, meaning that operations are performed from left to right.</span></span> <span data-ttu-id="8ad58-749">優先順位と結合規則は、かっこで囲まれた式を使用して制御できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-749">Precedence and associativity can be controlled using parenthetical expressions.</span></span>

### <a name="object-operands"></a><span data-ttu-id="8ad58-750">オブジェクトのオペランド</span><span class="sxs-lookup"><span data-stu-id="8ad58-750">Object Operands</span></span>

<span data-ttu-id="8ad58-751">各演算子でサポートされている通常の型に加えて、すべての演算子は `Object` 型のオペランドをサポートします。</span><span class="sxs-lookup"><span data-stu-id="8ad58-751">In addition to the regular types supported by each operator, all operators support operands of type `Object`.</span></span> <span data-ttu-id="8ad58-752">@No__t 0 のオペランドに適用される演算子は、`Object` の値に対して行われるメソッド呼び出しと同様に処理されます。遅延バインディングのメソッド呼び出しが選択される場合があります。この場合、コンパイル時の型ではなく、オペランドの実行時の型によって、運用.</span><span class="sxs-lookup"><span data-stu-id="8ad58-752">Operators applied to `Object` operands are handled similarly to method calls made on `Object` values: a late-bound method call might be chosen, in which case the run-time type of the operands, rather than the compile-time type, determines the validity and type of the operation.</span></span> <span data-ttu-id="8ad58-753">厳密なセマンティクスがコンパイル環境または @no__t 0 によって指定されている場合、型 `Object` のオペランドを持つ演算子は、`TypeOf...Is`、`Is`、および `IsNot` の各演算子を除き、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-753">If strict semantics are specified by the compilation environment or by `Option Strict`, any operators with operands of type `Object` cause a compile-time error, except for the `TypeOf...Is`, `Is` and `IsNot` operators.</span></span>

<span data-ttu-id="8ad58-754">演算子の解決によって、操作を遅延バインディングで実行する必要があると判断された場合、演算の結果は、オペランドのランタイム型が演算子でサポートされている型である場合、オペランドの型に演算子を適用した結果になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-754">When operator resolution determines that an operation should be performed late-bound, the outcome of the operation is the result of applying the operator to the operand types if the run-time types of the operands are types that are supported by the operator.</span></span> <span data-ttu-id="8ad58-755">値 `Nothing` は、二項演算子式の他のオペランドの型の既定値として処理されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-755">The value `Nothing` is treated as the default value of the type of the other operand in a binary operator expression.</span></span> <span data-ttu-id="8ad58-756">単項演算子式の場合、または両方のオペランドが二項演算子式で `Nothing` の場合、演算の型は `Integer` または演算子の唯一の結果型です (演算子によって `Integer` が生成されない場合)。</span><span class="sxs-lookup"><span data-stu-id="8ad58-756">In a unary operator expression, or if both operands are `Nothing` in a binary operator expression, the type of the operation is `Integer` or the only result type of the operator, if the operator does not result in `Integer`.</span></span> <span data-ttu-id="8ad58-757">操作の結果は、常に `Object` にキャストされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-757">The result of the operation is always then cast back to `Object`.</span></span> <span data-ttu-id="8ad58-758">オペランドの型に有効な演算子がない場合は、@no__t 0 の例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-758">If the operand types have no valid operator, a `System.InvalidCastException` exception is thrown.</span></span> <span data-ttu-id="8ad58-759">実行時の変換は、暗黙的なものでも明示的であるかに関係なく実行されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-759">Conversions at run time are done without regard to whether they are implicit or explicit.</span></span>

<span data-ttu-id="8ad58-760">数値二項演算の結果としてオーバーフロー例外が発生する場合 (整数オーバーフローチェックがオンかオフかに関係なく)、結果型は可能であれば、次に大きい数値型に上位変換されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-760">If the result of a numeric binary operation would produce an overflow exception (regardless of whether integer overflow checking is on or off), then the result type is promoted to the next wider numeric type, if possible.</span></span> <span data-ttu-id="8ad58-761">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-761">For example, consider the following code:</span></span>

```vb
Module Test
    Sub Main()
        Dim o As Object = CObj(CByte(2)) * CObj(CByte(255))

        Console.WriteLine(o.GetType().ToString() & " = " & o)
    End Sub
End Module
```

<span data-ttu-id="8ad58-762">次の結果が出力されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-762">It prints the following result:</span></span>

```console
System.Int16 = 512
```

<span data-ttu-id="8ad58-763">数値の範囲を超える数値型を使用できない場合は、@no__t 0 の例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-763">If no wider numeric type is available to hold the number, a `System.OverflowException` exception is thrown.</span></span>

### <a name="operator-resolution"></a><span data-ttu-id="8ad58-764">演算子の解決</span><span class="sxs-lookup"><span data-stu-id="8ad58-764">Operator Resolution</span></span>

<span data-ttu-id="8ad58-765">演算子の種類とオペランドのセットを指定すると、演算子の解決によって、オペランドに使用する演算子が決まります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-765">Given an operator type and a set of operands, operator resolution determines which operator to use for the operands.</span></span> <span data-ttu-id="8ad58-766">演算子を解決する際、次の手順に従って、ユーザー定義の演算子が最初に考慮されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-766">When resolving operators, user-defined operators will be considered first, using the following steps:</span></span>

1. <span data-ttu-id="8ad58-767">まず、候補となる演算子がすべて収集されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-767">First, all of the candidate operators are collected.</span></span> <span data-ttu-id="8ad58-768">候補演算子は、ソース型の特定の演算子型のすべてのユーザー定義演算子と、対象の型の特定の型のすべてのユーザー定義演算子です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-768">The candidate operators are all of the user-defined operators of the particular operator type in the source type and all of the user-defined operators of the particular type in the target type.</span></span> <span data-ttu-id="8ad58-769">変換元の型と変換先の型が関連付けられている場合、共通の演算子は1回だけ考慮されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-769">If the source type and destination type are related, common operators are only considered once.</span></span>

2. <span data-ttu-id="8ad58-770">次に、演算子とオペランドにオーバーロードの解決を適用して、最も限定的な演算子を選択します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-770">Then, overload resolution is applied to the operators and operands to select the most specific operator.</span></span> <span data-ttu-id="8ad58-771">バイナリ演算子の場合は、遅延バインディングの呼び出しが発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-771">In the case of binary operators, this may result in a late-bound call.</span></span>

<span data-ttu-id="8ad58-772">型の候補演算子 `T?` を収集する場合は、代わりに型 @no__t の演算子が使用されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-772">When collecting the candidate operators for a type `T?`, the operators of type `T` are used instead.</span></span> <span data-ttu-id="8ad58-773">Null 非許容の値型のみを含む、@no__t 0 のユーザー定義演算子も、すべてリフトされています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-773">Any of `T`'s user-defined operators that involve only non-nullable value types are also lifted.</span></span> <span data-ttu-id="8ad58-774">リフトされた演算子は、任意の値型の null 許容バージョンを使用しますが、例外として `IsTrue` の戻り値の型と `IsFalse` (`Boolean` である必要があります) です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-774">A lifted operator uses the nullable version of any value types, with the exception the return types of `IsTrue` and `IsFalse` (which must be `Boolean`).</span></span> <span data-ttu-id="8ad58-775">リフトされた演算子は、オペランドを null 非許容バージョンに変換し、ユーザー定義の演算子を評価した後、結果の型を null 許容バージョンに変換することによって評価されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-775">Lifted operators are evaluated by converting the operands to their non-nullable version, then evaluating the user-defined operator and then converting the result type to its nullable version.</span></span> <span data-ttu-id="8ad58-776">イーサオペランドが 0 @no__t の場合、式の結果は、結果型の null 許容バージョンとして型指定された `Nothing` の値になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-776">If ether operand is `Nothing`, the result of the expression is a value of `Nothing` typed as the nullable version of the result type.</span></span> <span data-ttu-id="8ad58-777">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-777">For example:</span></span>

```vb
Structure T
    ...
End Structure

Structure S
    Public Shared Operator +(ByVal op1 As S, ByVal op2 As T) As T
        ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim x As S?
        Dim y, z As T?

        ' Valid, as S + T = T is lifted to S? + T? = T?
        z = x + y 
    End Sub
End Module
```

<span data-ttu-id="8ad58-778">演算子が二項演算子で、いずれかのオペランドが参照型の場合は、演算子もリフトされますが、演算子へのバインドではエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-778">If the operator is a binary operator and one of the operands is reference type, the operator is also lifted, but any binding to the operator produces an error.</span></span> <span data-ttu-id="8ad58-779">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-779">For example:</span></span>

```vb
Structure S1
    Public F1 As Integer

    Public Shared Operator +(left As S1, right As String) As S1
       ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim a? As S1
        Dim s As String
        
        ' Error: '+' is not defined for S1? and String
        a = a + s
    End Sub
End Module
```

<span data-ttu-id="8ad58-780">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="8ad58-780">__Note.__</span></span> <span data-ttu-id="8ad58-781">この規則は、将来のバージョンで null を反映する参照型を追加するかどうかを検討しているために存在します。この場合、2つの型の間で二項演算子を使用した場合の動作は変更されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-781">This rule exists because there has been consideration whether we wish to add null-propagating reference types in a future version, in which case the behavior in the case of binary operators between the two types would change.</span></span>

<span data-ttu-id="8ad58-782">変換と同様に、ユーザー定義の演算子は、リフトされた演算子よりも常に優先されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-782">As with conversions, user-defined operators are always preferred over lifted operators.</span></span>

<span data-ttu-id="8ad58-783">オーバーロードされた演算子を解決する場合、Visual Basic で定義されているクラスと、他の言語で定義されているクラスに違いがある可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-783">When resolving overloaded operators, there may be differences between classes defined in Visual Basic and those defined in other languages:</span></span>

* <span data-ttu-id="8ad58-784">他の言語では、`Not`、`And`、`Or` は、論理演算子とビット処理演算子の両方としてオーバーロードされる場合があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-784">In other languages, `Not`, `And`, and `Or` may be overloaded both as logical operators and bitwise operators.</span></span> <span data-ttu-id="8ad58-785">外部アセンブリからインポートした場合、どちらの形式も、これらの演算子の有効なオーバーロードとして受け入れられます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-785">Upon import from an external assembly, either form is accepted as a valid overload for these operators.</span></span> <span data-ttu-id="8ad58-786">ただし、論理演算子とビット処理演算子の両方を定義する型の場合は、ビットごとの実装だけが考慮されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-786">However, for a type which defines both logical and bitwise operators, only the bitwise implementation will be considered.</span></span>

* <span data-ttu-id="8ad58-787">他の言語では、`>>` および `<<` は、符号付き演算子と符号なし演算子の両方としてオーバーロードされる場合があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-787">In other languages, `>>` and `<<` may be overloaded both as signed operators and unsigned operators.</span></span> <span data-ttu-id="8ad58-788">外部アセンブリからのインポート時に、いずれかの形式が有効なオーバーロードとして受け入れられます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-788">Upon import from an external assembly, either form is accepted as a valid overload.</span></span> <span data-ttu-id="8ad58-789">ただし、符号付きと符号なしの両方の演算子を定義する型では、署名付きの実装のみが考慮されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-789">However, for a type which defines both signed and unsigned operators, only the signed implementation will be considered.</span></span>

* <span data-ttu-id="8ad58-790">ユーザー定義演算子がオペランドに最も固有でない場合は、組み込み演算子が考慮されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-790">If no user-defined operator is most specific to the operands, then intrinsic operators will be considered.</span></span> <span data-ttu-id="8ad58-791">オペランドに対して組み込み演算子が定義されておらず、いずれかのオペランドに Object 型オブジェクトがある場合、演算子は遅延バインディングされます。それ以外の場合は、コンパイル時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-791">If no intrinsic operator is defined for the operands and either operand has type Object then the operator will be resolved late-bound; otherwise,  a compile-time error results.</span></span>

<span data-ttu-id="8ad58-792">以前のバージョンの Visual Basic では、Object 型のオペランドが1つだけあり、適用可能なユーザー定義演算子がない場合は、それがエラーになりました。</span><span class="sxs-lookup"><span data-stu-id="8ad58-792">In prior versions of Visual Basic, if there was exactly one operand of type Object, and no applicable user-defined operators, and no applicable intrinsic operators, then it was an error.</span></span> <span data-ttu-id="8ad58-793">Visual Basic 11 の時点で、遅延バインディングが解決されました。</span><span class="sxs-lookup"><span data-stu-id="8ad58-793">As of Visual Basic 11, it is now resolved late-bound.</span></span> <span data-ttu-id="8ad58-794">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-794">For example:</span></span>

```vb
Module Module1
  Sub Main()
      Dim p As Object = Nothing
      Dim U As New Uri("http://www.microsoft.com")
      Dim j = U * p  ' is now resolved late-bound
   End Sub
End Module
```

<span data-ttu-id="8ad58-795">組み込み演算子を持つ型 `T` も `T?` に対して同じ演算子を定義します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-795">A type `T` that has an intrinsic operator also defines that same operator for `T?`.</span></span> <span data-ttu-id="8ad58-796">@No__t-0 に対する演算子の結果は、`T` の場合と同じになります。ただし、いずれかのオペランドが `Nothing` の場合、演算子の結果は `Nothing` になります (つまり、null 値が反映されます)。</span><span class="sxs-lookup"><span data-stu-id="8ad58-796">The result of the operator on `T?` will be the same as for `T`, except that if either operand is `Nothing`, the result of the operator will be `Nothing` (i.e. the null value is propagated).</span></span> <span data-ttu-id="8ad58-797">操作の型を解決するために、`?` は、それを持つすべてのオペランドから削除され、演算の型が決定されます。いずれかのオペランドが null 許容の値型である場合は、操作の型に `?` が追加されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-797">For the purposes of resolving the type of an operation, the `?` is removed from any operands that have them, the type of the operation is determined, and a `?` is added to the type of the operation if any of the operands were nullable value types.</span></span> <span data-ttu-id="8ad58-798">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-798">For example:</span></span>

```vb
Dim v1? As Integer = 10
Dim v2 As Long = 20

' Type of operation will be Long?
Console.WriteLine(v1 + v2)
```

<span data-ttu-id="8ad58-799">各演算子は、定義されている組み込み型と、オペランド型を指定して実行された操作の型を一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-799">Each operator lists the intrinsic types it is defined for and the type of the operation performed given the operand types.</span></span> <span data-ttu-id="8ad58-800">組み込み操作の型の結果は、次の一般的な規則に従います。</span><span class="sxs-lookup"><span data-stu-id="8ad58-800">The result of type of a intrinsic operation follows these general rules:</span></span>

* <span data-ttu-id="8ad58-801">すべてのオペランドが同じ型であり、演算子が型に対して定義されている場合、変換は行われず、その型の演算子が使用されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-801">If all operands are of the same type, and the operator is defined for the type, then no conversion occurs and the operator for that type is used.</span></span>

* <span data-ttu-id="8ad58-802">演算子に対して型が定義されていないオペランドは、次の手順を使用して変換され、演算子は新しい型に対して解決されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-802">Any operand whose type is not defined for the operator is converted using the following steps and the operator is resolved against the new types:</span></span>

  * <span data-ttu-id="8ad58-803">オペランドは、演算子とオペランドの両方に対して定義され、暗黙的に変換可能な、次に最も幅の広い型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-803">The operand is converted to the next widest type that is defined for both the operator and the operand and to which it is implicitly convertible.</span></span>

  * <span data-ttu-id="8ad58-804">このような型が存在しない場合、オペランドは演算子とオペランドの両方に対して定義され、暗黙的に変換可能な次の型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-804">If there is no such type, then the operand is converted to the next narrowest type that is defined for both the operator and the operand and to which it is implicitly convertible.</span></span>

  * <span data-ttu-id="8ad58-805">そのような型がない場合、または変換を実行できない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-805">If there is no such type or the conversion cannot occur, a compile-time error occurs.</span></span>

* <span data-ttu-id="8ad58-806">それ以外の場合は、オペランドがより広いオペランド型に変換され、その型の演算子が使用されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-806">Otherwise, the operands are converted to the wider of the operand types and the operator for that type is used.</span></span> <span data-ttu-id="8ad58-807">幅の狭いオペランドの型を、より広い演算子の型に暗黙的に変換できない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-807">If the narrower operand type cannot be implicitly converted to the wider operator type, a compile-time error occurs.</span></span>

<span data-ttu-id="8ad58-808">ただし、これらの一般的な規則にかかわらず、演算子の結果テーブルにはいくつかの特殊なケースがあります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-808">Despite these general rules, however, there are a number of special cases called out in the operator results tables.</span></span>

<span data-ttu-id="8ad58-809">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="8ad58-809">__Note.__</span></span> <span data-ttu-id="8ad58-810">書式設定の理由により、演算子の種類の表では、定義済みの名前が最初の2文字に省略されています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-810">For formatting reasons, the operator type tables abbreviate the predefined names to their first two characters.</span></span> <span data-ttu-id="8ad58-811">したがって、"By" は @no__t 0、"UI" は `UInteger`、"St" は `String` などです。"Err" は、指定されたオペランド型に対して操作が定義されていないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-811">So "By" is `Byte`, "UI" is `UInteger`, "St" is `String`, etc. "Err" means that there is no operation defined for the given operand types.</span></span>

## <a name="arithmetic-operators"></a><span data-ttu-id="8ad58-812">算術演算子</span><span class="sxs-lookup"><span data-stu-id="8ad58-812">Arithmetic Operators</span></span>

<span data-ttu-id="8ad58-813">@No__t-0、`/`、`\`、`^`、`Mod`、`+`、および `-` 演算子は*算術演算子*です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-813">The `*`, `/`, `\`, `^`, `Mod`, `+`, and `-` operators are the *arithmetic operators*.</span></span>

```antlr
ArithmeticOperatorExpression
    : UnaryPlusExpression
    | UnaryMinusExpression
    | AdditionOperatorExpression
    | SubtractionOperatorExpression
    | MultiplicationOperatorExpression
    | DivisionOperatorExpression
    | ModuloOperatorExpression
    | ExponentOperatorExpression
    ;
```

<span data-ttu-id="8ad58-814">浮動小数点算術演算は、演算の結果の型よりも高い精度で実行される場合があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-814">Floating-point arithmetic operations may be performed with higher precision than the result type of the operation.</span></span> <span data-ttu-id="8ad58-815">たとえば、一部のハードウェアアーキテクチャでは、"extended" または "long double" 浮動小数点型がサポートされています。これは、@no__t 0 型よりも範囲と有効桁数が大きく、この高い精度の型を使用してすべての浮動小数点演算を暗黙的に実行します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-815">For example, some hardware architectures support an "extended" or "long double" floating-point type with greater range and precision than the `Double` type, and implicitly perform all floating-point operations using this higher-precision type.</span></span> <span data-ttu-id="8ad58-816">低精度で浮動小数点演算を実行するためにハードウェアアーキテクチャを作成できますが、パフォーマンスが過剰に低下します。Visual Basic では、パフォーマンスと精度の両方をプランに実装する必要がなく、すべての浮動小数点演算に対して高い精度の型を使用できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-816">Hardware architectures can be made to perform floating-point operations with less precision only at excessive cost in performance; rather than require an implementation to forfeit both performance and precision, Visual Basic allows the higher-precision type to be used for all floating-point operations.</span></span> <span data-ttu-id="8ad58-817">より正確な結果を提供する以外にも、測定可能な効果はほとんどありません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-817">Other than delivering more precise results, this rarely has any measurable effects.</span></span> <span data-ttu-id="8ad58-818">ただし、`x * y / z` という形式の式では、乗算によって `Double` の範囲外の結果が生成されますが、その後の除算では、一時的な結果が @no__t 2 の範囲に戻されます。これは、式がで評価されるという事実です。範囲の広い形式では、無限大ではなく、有限の結果が生成される場合があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-818">However, in expressions of the form `x * y / z`, where the multiplication produces a result that is outside the `Double` range, but the subsequent division brings the temporary result back into the `Double` range, the fact that the expression is evaluated in a higher-range format may cause a finite result to be produced instead of infinity.</span></span>


### <a name="unary-plus-operator"></a><span data-ttu-id="8ad58-819">単項プラス演算子</span><span class="sxs-lookup"><span data-stu-id="8ad58-819">Unary Plus Operator</span></span>

```antlr
UnaryPlusExpression
    : '+' Expression
    ;
```

<span data-ttu-id="8ad58-820">単項プラス演算子は @no__t 0、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`、`Single`、`Double`、および 0 の各型に対して定義されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-820">The unary plus operator is defined for the `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Single`, `Double`, and `Decimal` types.</span></span>

<span data-ttu-id="8ad58-821">__操作の種類:__</span><span class="sxs-lookup"><span data-stu-id="8ad58-821">__Operation Type:__</span></span>


| <span data-ttu-id="8ad58-822">__コンボ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-822">__Bo__</span></span> | <span data-ttu-id="8ad58-823">__SB__</span><span class="sxs-lookup"><span data-stu-id="8ad58-823">__SB__</span></span> | <span data-ttu-id="8ad58-824">__は__</span><span class="sxs-lookup"><span data-stu-id="8ad58-824">__By__</span></span> | <span data-ttu-id="8ad58-825">__悪夢__</span><span class="sxs-lookup"><span data-stu-id="8ad58-825">__Sh__</span></span> | <span data-ttu-id="8ad58-826">__米国__</span><span class="sxs-lookup"><span data-stu-id="8ad58-826">__US__</span></span> | <span data-ttu-id="8ad58-827">__In__</span><span class="sxs-lookup"><span data-stu-id="8ad58-827">__In__</span></span> | <span data-ttu-id="8ad58-828">__UI__</span><span class="sxs-lookup"><span data-stu-id="8ad58-828">__UI__</span></span> | <span data-ttu-id="8ad58-829">__Lo__</span><span class="sxs-lookup"><span data-stu-id="8ad58-829">__Lo__</span></span> | <span data-ttu-id="8ad58-830">__UL__</span><span class="sxs-lookup"><span data-stu-id="8ad58-830">__UL__</span></span> | <span data-ttu-id="8ad58-831">__解放__</span><span class="sxs-lookup"><span data-stu-id="8ad58-831">__De__</span></span> | <span data-ttu-id="8ad58-832">__Si__</span><span class="sxs-lookup"><span data-stu-id="8ad58-832">__Si__</span></span> | <span data-ttu-id="8ad58-833">__Do__</span><span class="sxs-lookup"><span data-stu-id="8ad58-833">__Do__</span></span> | <span data-ttu-id="8ad58-834">__オーディオ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-834">__Da__</span></span>  | <span data-ttu-id="8ad58-835">__ハーフ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-835">__Ch__</span></span>  | <span data-ttu-id="8ad58-836">__&__</span><span class="sxs-lookup"><span data-stu-id="8ad58-836">__St__</span></span> | <span data-ttu-id="8ad58-837">__Ob__</span><span class="sxs-lookup"><span data-stu-id="8ad58-837">__Ob__</span></span> | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="8ad58-838">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-838">Sh</span></span> | <span data-ttu-id="8ad58-839">SB</span><span class="sxs-lookup"><span data-stu-id="8ad58-839">SB</span></span> | <span data-ttu-id="8ad58-840">By</span><span class="sxs-lookup"><span data-stu-id="8ad58-840">By</span></span> | <span data-ttu-id="8ad58-841">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-841">Sh</span></span> | <span data-ttu-id="8ad58-842">US</span><span class="sxs-lookup"><span data-stu-id="8ad58-842">US</span></span> | <span data-ttu-id="8ad58-843">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-843">In</span></span> | <span data-ttu-id="8ad58-844">UI</span><span class="sxs-lookup"><span data-stu-id="8ad58-844">UI</span></span> | <span data-ttu-id="8ad58-845">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-845">Lo</span></span> | <span data-ttu-id="8ad58-846">UL</span><span class="sxs-lookup"><span data-stu-id="8ad58-846">UL</span></span> | <span data-ttu-id="8ad58-847">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-847">De</span></span> | <span data-ttu-id="8ad58-848">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-848">Si</span></span> | <span data-ttu-id="8ad58-849">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-849">Do</span></span> | <span data-ttu-id="8ad58-850">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-850">Err</span></span> | <span data-ttu-id="8ad58-851">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-851">Err</span></span> | <span data-ttu-id="8ad58-852">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-852">Do</span></span> | <span data-ttu-id="8ad58-853">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-853">Ob</span></span> | 


### <a name="unary-minus-operator"></a><span data-ttu-id="8ad58-854">単項マイナス演算子</span><span class="sxs-lookup"><span data-stu-id="8ad58-854">Unary Minus Operator</span></span>

```antlr
UnaryMinusExpression
    : '-' Expression
    ;
```

<span data-ttu-id="8ad58-855">単項マイナス演算子は、次の型に対して定義されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-855">The unary minus operator is defined for the following types:</span></span>

<span data-ttu-id="8ad58-856">`SByte`、 `Short`、 `Integer`、および `Long`。</span><span class="sxs-lookup"><span data-stu-id="8ad58-856">`SByte`, `Short`, `Integer`, and `Long`.</span></span> <span data-ttu-id="8ad58-857">結果は、オペランドを0から減算することによって計算されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-857">The result is computed by subtracting the operand from zero.</span></span> <span data-ttu-id="8ad58-858">整数のオーバーフローチェックがオンで、オペランドの値が負の最大 `SByte`、`Short`、`Integer`、または `Long` の場合は、@no__t 4 の例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-858">If integer overflow checking is on and the value of the operand is the maximum negative `SByte`, `Short`, `Integer`, or `Long`, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="8ad58-859">それ以外の場合、オペランドの値が負の最大 `SByte`、`Short`、`Integer`、または `Long` の場合、結果は同じ値になり、オーバーフローは報告されません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-859">Otherwise, if the value of the operand is the maximum negative `SByte`, `Short`, `Integer`, or `Long`, the result is that same value, and the overflow is not reported.</span></span>

<span data-ttu-id="8ad58-860">`Single` および `Double`。</span><span class="sxs-lookup"><span data-stu-id="8ad58-860">`Single` and `Double`.</span></span> <span data-ttu-id="8ad58-861">結果は、符号を反転したオペランドの値 (0 と無限大を含む) です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-861">The result is the value of the operand with its sign inverted, including the values 0 and Infinity.</span></span> <span data-ttu-id="8ad58-862">オペランドが NaN の場合、結果は NaN にもなります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-862">If the operand is NaN, the result is also NaN.</span></span>

<span data-ttu-id="8ad58-863">`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="8ad58-863">`Decimal`.</span></span> <span data-ttu-id="8ad58-864">結果は、オペランドを0から減算することによって計算されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-864">The result is computed by subtracting the operand from zero.</span></span>

<span data-ttu-id="8ad58-865">__操作の種類:__</span><span class="sxs-lookup"><span data-stu-id="8ad58-865">__Operation Type:__</span></span>

| <span data-ttu-id="8ad58-866">__コンボ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-866">__Bo__</span></span> | <span data-ttu-id="8ad58-867">__SB__</span><span class="sxs-lookup"><span data-stu-id="8ad58-867">__SB__</span></span> | <span data-ttu-id="8ad58-868">__は__</span><span class="sxs-lookup"><span data-stu-id="8ad58-868">__By__</span></span> | <span data-ttu-id="8ad58-869">__悪夢__</span><span class="sxs-lookup"><span data-stu-id="8ad58-869">__Sh__</span></span> | <span data-ttu-id="8ad58-870">__米国__</span><span class="sxs-lookup"><span data-stu-id="8ad58-870">__US__</span></span> | <span data-ttu-id="8ad58-871">__In__</span><span class="sxs-lookup"><span data-stu-id="8ad58-871">__In__</span></span> | <span data-ttu-id="8ad58-872">__UI__</span><span class="sxs-lookup"><span data-stu-id="8ad58-872">__UI__</span></span> | <span data-ttu-id="8ad58-873">__Lo__</span><span class="sxs-lookup"><span data-stu-id="8ad58-873">__Lo__</span></span> | <span data-ttu-id="8ad58-874">__UL__</span><span class="sxs-lookup"><span data-stu-id="8ad58-874">__UL__</span></span> | <span data-ttu-id="8ad58-875">__解放__</span><span class="sxs-lookup"><span data-stu-id="8ad58-875">__De__</span></span> | <span data-ttu-id="8ad58-876">__Si__</span><span class="sxs-lookup"><span data-stu-id="8ad58-876">__Si__</span></span> | <span data-ttu-id="8ad58-877">__Do__</span><span class="sxs-lookup"><span data-stu-id="8ad58-877">__Do__</span></span> | <span data-ttu-id="8ad58-878">__オーディオ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-878">__Da__</span></span>  | <span data-ttu-id="8ad58-879">__ハーフ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-879">__Ch__</span></span>  | <span data-ttu-id="8ad58-880">__&__</span><span class="sxs-lookup"><span data-stu-id="8ad58-880">__St__</span></span> | <span data-ttu-id="8ad58-881">__Ob__</span><span class="sxs-lookup"><span data-stu-id="8ad58-881">__Ob__</span></span> | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="8ad58-882">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-882">Sh</span></span> | <span data-ttu-id="8ad58-883">SB</span><span class="sxs-lookup"><span data-stu-id="8ad58-883">SB</span></span> | <span data-ttu-id="8ad58-884">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-884">Sh</span></span> | <span data-ttu-id="8ad58-885">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-885">Sh</span></span> | <span data-ttu-id="8ad58-886">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-886">In</span></span> | <span data-ttu-id="8ad58-887">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-887">In</span></span> | <span data-ttu-id="8ad58-888">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-888">Lo</span></span> | <span data-ttu-id="8ad58-889">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-889">Lo</span></span> | <span data-ttu-id="8ad58-890">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-890">De</span></span> | <span data-ttu-id="8ad58-891">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-891">De</span></span> | <span data-ttu-id="8ad58-892">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-892">Si</span></span> | <span data-ttu-id="8ad58-893">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-893">Do</span></span> | <span data-ttu-id="8ad58-894">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-894">Err</span></span> | <span data-ttu-id="8ad58-895">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-895">Err</span></span> | <span data-ttu-id="8ad58-896">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-896">Do</span></span> | <span data-ttu-id="8ad58-897">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-897">Ob</span></span> | 


### <a name="addition-operator"></a><span data-ttu-id="8ad58-898">加算演算子</span><span class="sxs-lookup"><span data-stu-id="8ad58-898">Addition Operator</span></span>

<span data-ttu-id="8ad58-899">加算演算子は、2つのオペランドの合計を計算します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-899">The addition operator computes the sum of the two operands.</span></span>

```antlr
AdditionOperatorExpression
    : Expression '+' LineTerminator? Expression
    ;
```

<span data-ttu-id="8ad58-900">加算演算子は、次の型に対して定義されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-900">The addition operator is defined for the following types:</span></span>

* <span data-ttu-id="8ad58-901">`Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、および `Long`。</span><span class="sxs-lookup"><span data-stu-id="8ad58-901">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="8ad58-902">整数のオーバーフローチェックがオンで、合計が結果の型の範囲外である場合は、@no__t 0 の例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-902">If integer overflow checking is on and the sum is outside the range of the result type, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="8ad58-903">それ以外の場合、オーバーフローは報告されず、結果の重要な上位ビットはすべて破棄されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-903">Otherwise, overflows are not reported, and any significant high-order bits of the result are discarded.</span></span>

* <span data-ttu-id="8ad58-904">`Single` および `Double`。</span><span class="sxs-lookup"><span data-stu-id="8ad58-904">`Single` and `Double`.</span></span> <span data-ttu-id="8ad58-905">合計は、IEEE 754 算術のルールに従って計算されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-905">The sum is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="8ad58-906">`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="8ad58-906">`Decimal`.</span></span> <span data-ttu-id="8ad58-907">結果の値が大きすぎて decimal 形式で表現できない場合は、@no__t 0 の例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-907">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="8ad58-908">小数点以下の値が小さすぎる場合、結果は0になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-908">If the result value is too small to represent in the decimal format, the result is 0.</span></span>

* <span data-ttu-id="8ad58-909">`String`。</span><span class="sxs-lookup"><span data-stu-id="8ad58-909">`String`.</span></span> <span data-ttu-id="8ad58-910">2つの @no__t 0 のオペランドが連結されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-910">The two `String` operands are concatenated together.</span></span>

* <span data-ttu-id="8ad58-911">`Date`。</span><span class="sxs-lookup"><span data-stu-id="8ad58-911">`Date`.</span></span> <span data-ttu-id="8ad58-912">@No__t-0 型は、オーバーロードされた加算演算子を定義します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-912">The `System.DateTime` type defines overloaded addition operators.</span></span> <span data-ttu-id="8ad58-913">@No__t-0 は組み込みの `Date` 型と同じであるため、これらの演算子は `Date` 型でも使用できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-913">Because `System.DateTime` is equivalent to the intrinsic `Date` type, these operators is also available on the `Date` type.</span></span>

<span data-ttu-id="8ad58-914">__操作の種類:__</span><span class="sxs-lookup"><span data-stu-id="8ad58-914">__Operation Type:__</span></span>

|        | <span data-ttu-id="8ad58-915">__コンボ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-915">__Bo__</span></span> | <span data-ttu-id="8ad58-916">__SB__</span><span class="sxs-lookup"><span data-stu-id="8ad58-916">__SB__</span></span> | <span data-ttu-id="8ad58-917">__は__</span><span class="sxs-lookup"><span data-stu-id="8ad58-917">__By__</span></span> | <span data-ttu-id="8ad58-918">__悪夢__</span><span class="sxs-lookup"><span data-stu-id="8ad58-918">__Sh__</span></span> | <span data-ttu-id="8ad58-919">__米国__</span><span class="sxs-lookup"><span data-stu-id="8ad58-919">__US__</span></span> | <span data-ttu-id="8ad58-920">__In__</span><span class="sxs-lookup"><span data-stu-id="8ad58-920">__In__</span></span> | <span data-ttu-id="8ad58-921">__UI__</span><span class="sxs-lookup"><span data-stu-id="8ad58-921">__UI__</span></span> | <span data-ttu-id="8ad58-922">__Lo__</span><span class="sxs-lookup"><span data-stu-id="8ad58-922">__Lo__</span></span> | <span data-ttu-id="8ad58-923">__UL__</span><span class="sxs-lookup"><span data-stu-id="8ad58-923">__UL__</span></span> | <span data-ttu-id="8ad58-924">__解放__</span><span class="sxs-lookup"><span data-stu-id="8ad58-924">__De__</span></span> | <span data-ttu-id="8ad58-925">__Si__</span><span class="sxs-lookup"><span data-stu-id="8ad58-925">__Si__</span></span> | <span data-ttu-id="8ad58-926">__Do__</span><span class="sxs-lookup"><span data-stu-id="8ad58-926">__Do__</span></span> | <span data-ttu-id="8ad58-927">__オーディオ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-927">__Da__</span></span>  | <span data-ttu-id="8ad58-928">__ハーフ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-928">__Ch__</span></span>  | <span data-ttu-id="8ad58-929">__&__</span><span class="sxs-lookup"><span data-stu-id="8ad58-929">__St__</span></span> | <span data-ttu-id="8ad58-930">__Ob__</span><span class="sxs-lookup"><span data-stu-id="8ad58-930">__Ob__</span></span> | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="8ad58-931">__コンボ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-931">__Bo__</span></span> | <span data-ttu-id="8ad58-932">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-932">Sh</span></span> | <span data-ttu-id="8ad58-933">SB</span><span class="sxs-lookup"><span data-stu-id="8ad58-933">SB</span></span> | <span data-ttu-id="8ad58-934">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-934">Sh</span></span> | <span data-ttu-id="8ad58-935">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-935">Sh</span></span> | <span data-ttu-id="8ad58-936">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-936">In</span></span> | <span data-ttu-id="8ad58-937">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-937">In</span></span> | <span data-ttu-id="8ad58-938">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-938">Lo</span></span> | <span data-ttu-id="8ad58-939">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-939">Lo</span></span> | <span data-ttu-id="8ad58-940">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-940">De</span></span> | <span data-ttu-id="8ad58-941">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-941">De</span></span> | <span data-ttu-id="8ad58-942">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-942">Si</span></span> | <span data-ttu-id="8ad58-943">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-943">Do</span></span> | <span data-ttu-id="8ad58-944">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-944">Err</span></span> | <span data-ttu-id="8ad58-945">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-945">Err</span></span> | <span data-ttu-id="8ad58-946">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-946">Do</span></span> | <span data-ttu-id="8ad58-947">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-947">Ob</span></span> | 
| <span data-ttu-id="8ad58-948">__SB__</span><span class="sxs-lookup"><span data-stu-id="8ad58-948">__SB__</span></span> |    | <span data-ttu-id="8ad58-949">SB</span><span class="sxs-lookup"><span data-stu-id="8ad58-949">SB</span></span> | <span data-ttu-id="8ad58-950">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-950">Sh</span></span> | <span data-ttu-id="8ad58-951">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-951">Sh</span></span> | <span data-ttu-id="8ad58-952">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-952">In</span></span> | <span data-ttu-id="8ad58-953">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-953">In</span></span> | <span data-ttu-id="8ad58-954">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-954">Lo</span></span> | <span data-ttu-id="8ad58-955">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-955">Lo</span></span> | <span data-ttu-id="8ad58-956">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-956">De</span></span> | <span data-ttu-id="8ad58-957">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-957">De</span></span> | <span data-ttu-id="8ad58-958">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-958">Si</span></span> | <span data-ttu-id="8ad58-959">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-959">Do</span></span> | <span data-ttu-id="8ad58-960">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-960">Err</span></span> | <span data-ttu-id="8ad58-961">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-961">Err</span></span> | <span data-ttu-id="8ad58-962">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-962">Do</span></span> | <span data-ttu-id="8ad58-963">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-963">Ob</span></span> | 
| <span data-ttu-id="8ad58-964">__は__</span><span class="sxs-lookup"><span data-stu-id="8ad58-964">__By__</span></span> |    |    | <span data-ttu-id="8ad58-965">By</span><span class="sxs-lookup"><span data-stu-id="8ad58-965">By</span></span> | <span data-ttu-id="8ad58-966">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-966">Sh</span></span> | <span data-ttu-id="8ad58-967">US</span><span class="sxs-lookup"><span data-stu-id="8ad58-967">US</span></span> | <span data-ttu-id="8ad58-968">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-968">In</span></span> | <span data-ttu-id="8ad58-969">UI</span><span class="sxs-lookup"><span data-stu-id="8ad58-969">UI</span></span> | <span data-ttu-id="8ad58-970">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-970">Lo</span></span> | <span data-ttu-id="8ad58-971">UL</span><span class="sxs-lookup"><span data-stu-id="8ad58-971">UL</span></span> | <span data-ttu-id="8ad58-972">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-972">De</span></span> | <span data-ttu-id="8ad58-973">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-973">Si</span></span> | <span data-ttu-id="8ad58-974">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-974">Do</span></span> | <span data-ttu-id="8ad58-975">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-975">Err</span></span> | <span data-ttu-id="8ad58-976">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-976">Err</span></span> | <span data-ttu-id="8ad58-977">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-977">Do</span></span> | <span data-ttu-id="8ad58-978">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-978">Ob</span></span> | 
| <span data-ttu-id="8ad58-979">__悪夢__</span><span class="sxs-lookup"><span data-stu-id="8ad58-979">__Sh__</span></span> |    |    |    | <span data-ttu-id="8ad58-980">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-980">Sh</span></span> | <span data-ttu-id="8ad58-981">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-981">In</span></span> | <span data-ttu-id="8ad58-982">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-982">In</span></span> | <span data-ttu-id="8ad58-983">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-983">Lo</span></span> | <span data-ttu-id="8ad58-984">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-984">Lo</span></span> | <span data-ttu-id="8ad58-985">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-985">De</span></span> | <span data-ttu-id="8ad58-986">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-986">De</span></span> | <span data-ttu-id="8ad58-987">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-987">Si</span></span> | <span data-ttu-id="8ad58-988">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-988">Do</span></span> | <span data-ttu-id="8ad58-989">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-989">Err</span></span> | <span data-ttu-id="8ad58-990">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-990">Err</span></span> | <span data-ttu-id="8ad58-991">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-991">Do</span></span> | <span data-ttu-id="8ad58-992">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-992">Ob</span></span> | 
| <span data-ttu-id="8ad58-993">__米国__</span><span class="sxs-lookup"><span data-stu-id="8ad58-993">__US__</span></span> |    |    |    |    | <span data-ttu-id="8ad58-994">US</span><span class="sxs-lookup"><span data-stu-id="8ad58-994">US</span></span> | <span data-ttu-id="8ad58-995">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-995">In</span></span> | <span data-ttu-id="8ad58-996">UI</span><span class="sxs-lookup"><span data-stu-id="8ad58-996">UI</span></span> | <span data-ttu-id="8ad58-997">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-997">Lo</span></span> | <span data-ttu-id="8ad58-998">UL</span><span class="sxs-lookup"><span data-stu-id="8ad58-998">UL</span></span> | <span data-ttu-id="8ad58-999">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-999">De</span></span> | <span data-ttu-id="8ad58-1000">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1000">Si</span></span> | <span data-ttu-id="8ad58-1001">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1001">Do</span></span> | <span data-ttu-id="8ad58-1002">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1002">Err</span></span> | <span data-ttu-id="8ad58-1003">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1003">Err</span></span> | <span data-ttu-id="8ad58-1004">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1004">Do</span></span> | <span data-ttu-id="8ad58-1005">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1005">Ob</span></span> | 
| <span data-ttu-id="8ad58-1006">__In__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1006">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="8ad58-1007">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1007">In</span></span> | <span data-ttu-id="8ad58-1008">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1008">Lo</span></span> | <span data-ttu-id="8ad58-1009">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1009">Lo</span></span> | <span data-ttu-id="8ad58-1010">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1010">De</span></span> | <span data-ttu-id="8ad58-1011">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1011">De</span></span> | <span data-ttu-id="8ad58-1012">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1012">Si</span></span> | <span data-ttu-id="8ad58-1013">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1013">Do</span></span> | <span data-ttu-id="8ad58-1014">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1014">Err</span></span> | <span data-ttu-id="8ad58-1015">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1015">Err</span></span> | <span data-ttu-id="8ad58-1016">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1016">Do</span></span> | <span data-ttu-id="8ad58-1017">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1017">Ob</span></span> | 
| <span data-ttu-id="8ad58-1018">__UI__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1018">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="8ad58-1019">UI</span><span class="sxs-lookup"><span data-stu-id="8ad58-1019">UI</span></span> | <span data-ttu-id="8ad58-1020">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1020">Lo</span></span> | <span data-ttu-id="8ad58-1021">UL</span><span class="sxs-lookup"><span data-stu-id="8ad58-1021">UL</span></span> | <span data-ttu-id="8ad58-1022">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1022">De</span></span> | <span data-ttu-id="8ad58-1023">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1023">Si</span></span> | <span data-ttu-id="8ad58-1024">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1024">Do</span></span> | <span data-ttu-id="8ad58-1025">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1025">Err</span></span> | <span data-ttu-id="8ad58-1026">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1026">Err</span></span> | <span data-ttu-id="8ad58-1027">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1027">Do</span></span> | <span data-ttu-id="8ad58-1028">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1028">Ob</span></span> | 
| <span data-ttu-id="8ad58-1029">__Lo__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1029">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1030">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1030">Lo</span></span> | <span data-ttu-id="8ad58-1031">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1031">De</span></span> | <span data-ttu-id="8ad58-1032">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1032">De</span></span> | <span data-ttu-id="8ad58-1033">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1033">Si</span></span> | <span data-ttu-id="8ad58-1034">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1034">Do</span></span> | <span data-ttu-id="8ad58-1035">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1035">Err</span></span> | <span data-ttu-id="8ad58-1036">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1036">Err</span></span> | <span data-ttu-id="8ad58-1037">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1037">Do</span></span> | <span data-ttu-id="8ad58-1038">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1038">Ob</span></span> | 
| <span data-ttu-id="8ad58-1039">__UL__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1039">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1040">UL</span><span class="sxs-lookup"><span data-stu-id="8ad58-1040">UL</span></span> | <span data-ttu-id="8ad58-1041">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1041">De</span></span> | <span data-ttu-id="8ad58-1042">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1042">Si</span></span> | <span data-ttu-id="8ad58-1043">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1043">Do</span></span> | <span data-ttu-id="8ad58-1044">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1044">Err</span></span> | <span data-ttu-id="8ad58-1045">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1045">Err</span></span> | <span data-ttu-id="8ad58-1046">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1046">Do</span></span> | <span data-ttu-id="8ad58-1047">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1047">Ob</span></span> | 
| <span data-ttu-id="8ad58-1048">__解放__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1048">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1049">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1049">De</span></span> | <span data-ttu-id="8ad58-1050">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1050">Si</span></span> | <span data-ttu-id="8ad58-1051">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1051">Do</span></span> | <span data-ttu-id="8ad58-1052">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1052">Err</span></span> | <span data-ttu-id="8ad58-1053">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1053">Err</span></span> | <span data-ttu-id="8ad58-1054">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1054">Do</span></span> | <span data-ttu-id="8ad58-1055">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1055">Ob</span></span> | 
| <span data-ttu-id="8ad58-1056">__Si__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1056">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1057">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1057">Si</span></span> | <span data-ttu-id="8ad58-1058">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1058">Do</span></span> | <span data-ttu-id="8ad58-1059">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1059">Err</span></span> | <span data-ttu-id="8ad58-1060">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1060">Err</span></span> | <span data-ttu-id="8ad58-1061">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1061">Do</span></span> | <span data-ttu-id="8ad58-1062">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1062">Ob</span></span> | 
| <span data-ttu-id="8ad58-1063">__Do__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1063">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1064">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1064">Do</span></span> | <span data-ttu-id="8ad58-1065">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1065">Err</span></span> | <span data-ttu-id="8ad58-1066">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1066">Err</span></span> | <span data-ttu-id="8ad58-1067">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1067">Do</span></span> | <span data-ttu-id="8ad58-1068">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1068">Ob</span></span> | 
| <span data-ttu-id="8ad58-1069">__オーディオ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1069">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1070">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-1070">St</span></span>  | <span data-ttu-id="8ad58-1071">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1071">Err</span></span> | <span data-ttu-id="8ad58-1072">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-1072">St</span></span> | <span data-ttu-id="8ad58-1073">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1073">Ob</span></span> | 
| <span data-ttu-id="8ad58-1074">__ハーフ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1074">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="8ad58-1075">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-1075">St</span></span>  | <span data-ttu-id="8ad58-1076">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-1076">St</span></span> | <span data-ttu-id="8ad58-1077">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1077">Ob</span></span> | 
| <span data-ttu-id="8ad58-1078">__&__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1078">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="8ad58-1079">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-1079">St</span></span> | <span data-ttu-id="8ad58-1080">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1080">Ob</span></span> | 
| <span data-ttu-id="8ad58-1081">__Ob__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1081">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |    | <span data-ttu-id="8ad58-1082">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1082">Ob</span></span> | 


### <a name="subtraction-operator"></a><span data-ttu-id="8ad58-1083">減算演算子</span><span class="sxs-lookup"><span data-stu-id="8ad58-1083">Subtraction Operator</span></span>

<span data-ttu-id="8ad58-1084">減算演算子は、最初のオペランドから2番目のオペランドを減算します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1084">The subtraction operator subtracts the second operand from the first operand.</span></span>

```antlr
SubtractionOperatorExpression
    : Expression '-' LineTerminator? Expression
    ;
```

<span data-ttu-id="8ad58-1085">減算演算子は、次の型に対して定義されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1085">The subtraction operator is defined for the following types:</span></span>

* <span data-ttu-id="8ad58-1086">`Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、および `Long`。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1086">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="8ad58-1087">整数のオーバーフローチェックがオンで、その差が結果の型の範囲外である場合は、@no__t 0 の例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1087">If integer overflow checking is on and the difference is outside the range of the result type, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="8ad58-1088">それ以外の場合、オーバーフローは報告されず、結果の重要な上位ビットはすべて破棄されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1088">Otherwise, overflows are not reported, and any significant high-order bits of the result are discarded.</span></span>

* <span data-ttu-id="8ad58-1089">`Single` および `Double`。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1089">`Single` and `Double`.</span></span> <span data-ttu-id="8ad58-1090">この違いは、IEEE 754 算術のルールに従って計算されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1090">The difference is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="8ad58-1091">`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1091">`Decimal`.</span></span> <span data-ttu-id="8ad58-1092">結果の値が大きすぎて decimal 形式で表現できない場合は、@no__t 0 の例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1092">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="8ad58-1093">小数点以下の値が小さすぎる場合、結果は0になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1093">If the result value is too small to represent in the decimal format, the result is 0.</span></span>

* <span data-ttu-id="8ad58-1094">`Date`。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1094">`Date`.</span></span> <span data-ttu-id="8ad58-1095">@No__t-0 型は、オーバーロードされた減算演算子を定義します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1095">The `System.DateTime` type defines overloaded subtraction operators.</span></span> <span data-ttu-id="8ad58-1096">@No__t-0 は組み込みの `Date` 型と同じであるため、これらの演算子は `Date` 型でも使用できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1096">Because `System.DateTime` is equivalent to the intrinsic `Date` type, these operators is also available on the `Date` type.</span></span>

<span data-ttu-id="8ad58-1097">__操作の種類:__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1097">__Operation Type:__</span></span>

|        | <span data-ttu-id="8ad58-1098">__コンボ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1098">__Bo__</span></span> | <span data-ttu-id="8ad58-1099">__SB__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1099">__SB__</span></span> | <span data-ttu-id="8ad58-1100">__は__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1100">__By__</span></span> | <span data-ttu-id="8ad58-1101">__悪夢__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1101">__Sh__</span></span> | <span data-ttu-id="8ad58-1102">__米国__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1102">__US__</span></span> | <span data-ttu-id="8ad58-1103">__In__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1103">__In__</span></span> | <span data-ttu-id="8ad58-1104">__UI__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1104">__UI__</span></span> | <span data-ttu-id="8ad58-1105">__Lo__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1105">__Lo__</span></span> | <span data-ttu-id="8ad58-1106">__UL__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1106">__UL__</span></span> | <span data-ttu-id="8ad58-1107">__解放__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1107">__De__</span></span> | <span data-ttu-id="8ad58-1108">__Si__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1108">__Si__</span></span> | <span data-ttu-id="8ad58-1109">__Do__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1109">__Do__</span></span> | <span data-ttu-id="8ad58-1110">__オーディオ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1110">__Da__</span></span>  | <span data-ttu-id="8ad58-1111">__ハーフ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1111">__Ch__</span></span>  | <span data-ttu-id="8ad58-1112">__&__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1112">__St__</span></span> | <span data-ttu-id="8ad58-1113">__Ob__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1113">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="8ad58-1114">__コンボ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1114">__Bo__</span></span> | <span data-ttu-id="8ad58-1115">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-1115">Sh</span></span> | <span data-ttu-id="8ad58-1116">SB</span><span class="sxs-lookup"><span data-stu-id="8ad58-1116">SB</span></span> | <span data-ttu-id="8ad58-1117">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-1117">Sh</span></span> | <span data-ttu-id="8ad58-1118">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-1118">Sh</span></span> | <span data-ttu-id="8ad58-1119">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1119">In</span></span> | <span data-ttu-id="8ad58-1120">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1120">In</span></span> | <span data-ttu-id="8ad58-1121">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1121">Lo</span></span> | <span data-ttu-id="8ad58-1122">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1122">Lo</span></span> | <span data-ttu-id="8ad58-1123">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1123">De</span></span> | <span data-ttu-id="8ad58-1124">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1124">De</span></span> | <span data-ttu-id="8ad58-1125">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1125">Si</span></span> | <span data-ttu-id="8ad58-1126">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1126">Do</span></span> | <span data-ttu-id="8ad58-1127">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1127">Err</span></span> | <span data-ttu-id="8ad58-1128">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1128">Err</span></span> | <span data-ttu-id="8ad58-1129">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1129">Do</span></span>  | <span data-ttu-id="8ad58-1130">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1130">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1131">__SB__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1131">__SB__</span></span> |    | <span data-ttu-id="8ad58-1132">SB</span><span class="sxs-lookup"><span data-stu-id="8ad58-1132">SB</span></span> | <span data-ttu-id="8ad58-1133">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-1133">Sh</span></span> | <span data-ttu-id="8ad58-1134">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-1134">Sh</span></span> | <span data-ttu-id="8ad58-1135">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1135">In</span></span> | <span data-ttu-id="8ad58-1136">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1136">In</span></span> | <span data-ttu-id="8ad58-1137">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1137">Lo</span></span> | <span data-ttu-id="8ad58-1138">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1138">Lo</span></span> | <span data-ttu-id="8ad58-1139">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1139">De</span></span> | <span data-ttu-id="8ad58-1140">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1140">De</span></span> | <span data-ttu-id="8ad58-1141">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1141">Si</span></span> | <span data-ttu-id="8ad58-1142">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1142">Do</span></span> | <span data-ttu-id="8ad58-1143">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1143">Err</span></span> | <span data-ttu-id="8ad58-1144">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1144">Err</span></span> | <span data-ttu-id="8ad58-1145">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1145">Do</span></span>  | <span data-ttu-id="8ad58-1146">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1146">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1147">__は__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1147">__By__</span></span> |    |    | <span data-ttu-id="8ad58-1148">By</span><span class="sxs-lookup"><span data-stu-id="8ad58-1148">By</span></span> | <span data-ttu-id="8ad58-1149">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-1149">Sh</span></span> | <span data-ttu-id="8ad58-1150">US</span><span class="sxs-lookup"><span data-stu-id="8ad58-1150">US</span></span> | <span data-ttu-id="8ad58-1151">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1151">In</span></span> | <span data-ttu-id="8ad58-1152">UI</span><span class="sxs-lookup"><span data-stu-id="8ad58-1152">UI</span></span> | <span data-ttu-id="8ad58-1153">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1153">Lo</span></span> | <span data-ttu-id="8ad58-1154">UL</span><span class="sxs-lookup"><span data-stu-id="8ad58-1154">UL</span></span> | <span data-ttu-id="8ad58-1155">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1155">De</span></span> | <span data-ttu-id="8ad58-1156">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1156">Si</span></span> | <span data-ttu-id="8ad58-1157">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1157">Do</span></span> | <span data-ttu-id="8ad58-1158">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1158">Err</span></span> | <span data-ttu-id="8ad58-1159">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1159">Err</span></span> | <span data-ttu-id="8ad58-1160">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1160">Do</span></span>  | <span data-ttu-id="8ad58-1161">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1161">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1162">__悪夢__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1162">__Sh__</span></span> |    |    |    | <span data-ttu-id="8ad58-1163">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-1163">Sh</span></span> | <span data-ttu-id="8ad58-1164">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1164">In</span></span> | <span data-ttu-id="8ad58-1165">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1165">In</span></span> | <span data-ttu-id="8ad58-1166">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1166">Lo</span></span> | <span data-ttu-id="8ad58-1167">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1167">Lo</span></span> | <span data-ttu-id="8ad58-1168">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1168">De</span></span> | <span data-ttu-id="8ad58-1169">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1169">De</span></span> | <span data-ttu-id="8ad58-1170">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1170">Si</span></span> | <span data-ttu-id="8ad58-1171">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1171">Do</span></span> | <span data-ttu-id="8ad58-1172">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1172">Err</span></span> | <span data-ttu-id="8ad58-1173">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1173">Err</span></span> | <span data-ttu-id="8ad58-1174">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1174">Do</span></span>  | <span data-ttu-id="8ad58-1175">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1175">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1176">__米国__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1176">__US__</span></span> |    |    |    |    | <span data-ttu-id="8ad58-1177">US</span><span class="sxs-lookup"><span data-stu-id="8ad58-1177">US</span></span> | <span data-ttu-id="8ad58-1178">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1178">In</span></span> | <span data-ttu-id="8ad58-1179">UI</span><span class="sxs-lookup"><span data-stu-id="8ad58-1179">UI</span></span> | <span data-ttu-id="8ad58-1180">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1180">Lo</span></span> | <span data-ttu-id="8ad58-1181">UL</span><span class="sxs-lookup"><span data-stu-id="8ad58-1181">UL</span></span> | <span data-ttu-id="8ad58-1182">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1182">De</span></span> | <span data-ttu-id="8ad58-1183">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1183">Si</span></span> | <span data-ttu-id="8ad58-1184">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1184">Do</span></span> | <span data-ttu-id="8ad58-1185">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1185">Err</span></span> | <span data-ttu-id="8ad58-1186">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1186">Err</span></span> | <span data-ttu-id="8ad58-1187">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1187">Do</span></span>  | <span data-ttu-id="8ad58-1188">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1188">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1189">__In__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1189">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="8ad58-1190">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1190">In</span></span> | <span data-ttu-id="8ad58-1191">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1191">Lo</span></span> | <span data-ttu-id="8ad58-1192">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1192">Lo</span></span> | <span data-ttu-id="8ad58-1193">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1193">De</span></span> | <span data-ttu-id="8ad58-1194">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1194">De</span></span> | <span data-ttu-id="8ad58-1195">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1195">Si</span></span> | <span data-ttu-id="8ad58-1196">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1196">Do</span></span> | <span data-ttu-id="8ad58-1197">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1197">Err</span></span> | <span data-ttu-id="8ad58-1198">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1198">Err</span></span> | <span data-ttu-id="8ad58-1199">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1199">Do</span></span>  | <span data-ttu-id="8ad58-1200">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1200">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1201">__UI__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1201">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="8ad58-1202">UI</span><span class="sxs-lookup"><span data-stu-id="8ad58-1202">UI</span></span> | <span data-ttu-id="8ad58-1203">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1203">Lo</span></span> | <span data-ttu-id="8ad58-1204">UL</span><span class="sxs-lookup"><span data-stu-id="8ad58-1204">UL</span></span> | <span data-ttu-id="8ad58-1205">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1205">De</span></span> | <span data-ttu-id="8ad58-1206">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1206">Si</span></span> | <span data-ttu-id="8ad58-1207">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1207">Do</span></span> | <span data-ttu-id="8ad58-1208">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1208">Err</span></span> | <span data-ttu-id="8ad58-1209">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1209">Err</span></span> | <span data-ttu-id="8ad58-1210">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1210">Do</span></span>  | <span data-ttu-id="8ad58-1211">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1211">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1212">__Lo__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1212">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1213">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1213">Lo</span></span> | <span data-ttu-id="8ad58-1214">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1214">De</span></span> | <span data-ttu-id="8ad58-1215">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1215">De</span></span> | <span data-ttu-id="8ad58-1216">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1216">Si</span></span> | <span data-ttu-id="8ad58-1217">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1217">Do</span></span> | <span data-ttu-id="8ad58-1218">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1218">Err</span></span> | <span data-ttu-id="8ad58-1219">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1219">Err</span></span> | <span data-ttu-id="8ad58-1220">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1220">Do</span></span>  | <span data-ttu-id="8ad58-1221">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1221">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1222">__UL__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1222">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1223">UL</span><span class="sxs-lookup"><span data-stu-id="8ad58-1223">UL</span></span> | <span data-ttu-id="8ad58-1224">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1224">De</span></span> | <span data-ttu-id="8ad58-1225">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1225">Si</span></span> | <span data-ttu-id="8ad58-1226">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1226">Do</span></span> | <span data-ttu-id="8ad58-1227">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1227">Err</span></span> | <span data-ttu-id="8ad58-1228">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1228">Err</span></span> | <span data-ttu-id="8ad58-1229">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1229">Do</span></span>  | <span data-ttu-id="8ad58-1230">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1230">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1231">__解放__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1231">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1232">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1232">De</span></span> | <span data-ttu-id="8ad58-1233">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1233">Si</span></span> | <span data-ttu-id="8ad58-1234">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1234">Do</span></span> | <span data-ttu-id="8ad58-1235">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1235">Err</span></span> | <span data-ttu-id="8ad58-1236">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1236">Err</span></span> | <span data-ttu-id="8ad58-1237">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1237">Do</span></span>  | <span data-ttu-id="8ad58-1238">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1238">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1239">__Si__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1239">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1240">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1240">Si</span></span> | <span data-ttu-id="8ad58-1241">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1241">Do</span></span> | <span data-ttu-id="8ad58-1242">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1242">Err</span></span> | <span data-ttu-id="8ad58-1243">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1243">Err</span></span> | <span data-ttu-id="8ad58-1244">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1244">Do</span></span>  | <span data-ttu-id="8ad58-1245">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1245">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1246">__Do__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1246">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1247">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1247">Do</span></span> | <span data-ttu-id="8ad58-1248">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1248">Err</span></span> | <span data-ttu-id="8ad58-1249">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1249">Err</span></span> | <span data-ttu-id="8ad58-1250">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1250">Do</span></span>  | <span data-ttu-id="8ad58-1251">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1251">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1252">__オーディオ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1252">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1253">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1253">Err</span></span> | <span data-ttu-id="8ad58-1254">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1254">Err</span></span> | <span data-ttu-id="8ad58-1255">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1255">Err</span></span> | <span data-ttu-id="8ad58-1256">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1256">Err</span></span> | 
| <span data-ttu-id="8ad58-1257">__ハーフ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1257">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="8ad58-1258">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1258">Err</span></span> | <span data-ttu-id="8ad58-1259">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1259">Err</span></span> | <span data-ttu-id="8ad58-1260">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1260">Err</span></span> | 
| <span data-ttu-id="8ad58-1261">__&__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1261">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="8ad58-1262">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1262">Do</span></span>  | <span data-ttu-id="8ad58-1263">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1263">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1264">__Ob__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1264">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="8ad58-1265">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1265">Ob</span></span>  | 


### <a name="multiplication-operator"></a><span data-ttu-id="8ad58-1266">乗算演算子</span><span class="sxs-lookup"><span data-stu-id="8ad58-1266">Multiplication Operator</span></span>

<span data-ttu-id="8ad58-1267">乗算演算子は、2つのオペランドの積を計算します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1267">The multiplication operator computes the product of two operands.</span></span>

```antlr
MultiplicationOperatorExpression
    : Expression '*' LineTerminator? Expression
    ;
```

<span data-ttu-id="8ad58-1268">乗算演算子は、次の型に対して定義されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1268">The multiplication operator is defined for the following types:</span></span>

* <span data-ttu-id="8ad58-1269">`Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、および `Long`。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1269">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="8ad58-1270">整数のオーバーフローチェックがオンで、製品が結果の型の範囲外にある場合は、@no__t 0 の例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1270">If integer overflow checking is on and the product is outside the range of the result type, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="8ad58-1271">それ以外の場合、オーバーフローは報告されず、結果の重要な上位ビットはすべて破棄されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1271">Otherwise, overflows are not reported, and any significant high-order bits of the result are discarded.</span></span>

* <span data-ttu-id="8ad58-1272">`Single` および `Double`。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1272">`Single` and `Double`.</span></span> <span data-ttu-id="8ad58-1273">この製品は、IEEE 754 算術のルールに従って計算されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1273">The product is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="8ad58-1274">`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1274">`Decimal`.</span></span> <span data-ttu-id="8ad58-1275">結果の値が大きすぎて decimal 形式で表現できない場合は、@no__t 0 の例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1275">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="8ad58-1276">小数点以下の値が小さすぎる場合、結果は0になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1276">If the result value is too small to represent in the decimal format, the result is 0.</span></span>

<span data-ttu-id="8ad58-1277">__操作の種類:__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1277">__Operation Type:__</span></span>

|        | <span data-ttu-id="8ad58-1278">__コンボ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1278">__Bo__</span></span> | <span data-ttu-id="8ad58-1279">__SB__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1279">__SB__</span></span> | <span data-ttu-id="8ad58-1280">__は__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1280">__By__</span></span> | <span data-ttu-id="8ad58-1281">__悪夢__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1281">__Sh__</span></span> | <span data-ttu-id="8ad58-1282">__米国__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1282">__US__</span></span> | <span data-ttu-id="8ad58-1283">__In__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1283">__In__</span></span> | <span data-ttu-id="8ad58-1284">__UI__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1284">__UI__</span></span> | <span data-ttu-id="8ad58-1285">__Lo__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1285">__Lo__</span></span> | <span data-ttu-id="8ad58-1286">__UL__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1286">__UL__</span></span> | <span data-ttu-id="8ad58-1287">__解放__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1287">__De__</span></span> | <span data-ttu-id="8ad58-1288">__Si__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1288">__Si__</span></span> | <span data-ttu-id="8ad58-1289">__Do__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1289">__Do__</span></span> | <span data-ttu-id="8ad58-1290">__オーディオ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1290">__Da__</span></span>  | <span data-ttu-id="8ad58-1291">__ハーフ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1291">__Ch__</span></span>  | <span data-ttu-id="8ad58-1292">__&__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1292">__St__</span></span> | <span data-ttu-id="8ad58-1293">__Ob__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1293">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="8ad58-1294">__コンボ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1294">__Bo__</span></span> | <span data-ttu-id="8ad58-1295">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-1295">Sh</span></span> | <span data-ttu-id="8ad58-1296">SB</span><span class="sxs-lookup"><span data-stu-id="8ad58-1296">SB</span></span> | <span data-ttu-id="8ad58-1297">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-1297">Sh</span></span> | <span data-ttu-id="8ad58-1298">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-1298">Sh</span></span> | <span data-ttu-id="8ad58-1299">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1299">In</span></span> | <span data-ttu-id="8ad58-1300">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1300">In</span></span> | <span data-ttu-id="8ad58-1301">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1301">Lo</span></span> | <span data-ttu-id="8ad58-1302">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1302">Lo</span></span> | <span data-ttu-id="8ad58-1303">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1303">De</span></span> | <span data-ttu-id="8ad58-1304">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1304">De</span></span> | <span data-ttu-id="8ad58-1305">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1305">Si</span></span> | <span data-ttu-id="8ad58-1306">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1306">Do</span></span> | <span data-ttu-id="8ad58-1307">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1307">Err</span></span> | <span data-ttu-id="8ad58-1308">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1308">Err</span></span> | <span data-ttu-id="8ad58-1309">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1309">Do</span></span>  | <span data-ttu-id="8ad58-1310">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1310">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1311">__SB__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1311">__SB__</span></span> |    | <span data-ttu-id="8ad58-1312">SB</span><span class="sxs-lookup"><span data-stu-id="8ad58-1312">SB</span></span> | <span data-ttu-id="8ad58-1313">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-1313">Sh</span></span> | <span data-ttu-id="8ad58-1314">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-1314">Sh</span></span> | <span data-ttu-id="8ad58-1315">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1315">In</span></span> | <span data-ttu-id="8ad58-1316">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1316">In</span></span> | <span data-ttu-id="8ad58-1317">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1317">Lo</span></span> | <span data-ttu-id="8ad58-1318">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1318">Lo</span></span> | <span data-ttu-id="8ad58-1319">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1319">De</span></span> | <span data-ttu-id="8ad58-1320">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1320">De</span></span> | <span data-ttu-id="8ad58-1321">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1321">Si</span></span> | <span data-ttu-id="8ad58-1322">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1322">Do</span></span> | <span data-ttu-id="8ad58-1323">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1323">Err</span></span> | <span data-ttu-id="8ad58-1324">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1324">Err</span></span> | <span data-ttu-id="8ad58-1325">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1325">Do</span></span>  | <span data-ttu-id="8ad58-1326">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1326">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1327">__は__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1327">__By__</span></span> |    |    | <span data-ttu-id="8ad58-1328">By</span><span class="sxs-lookup"><span data-stu-id="8ad58-1328">By</span></span> | <span data-ttu-id="8ad58-1329">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-1329">Sh</span></span> | <span data-ttu-id="8ad58-1330">US</span><span class="sxs-lookup"><span data-stu-id="8ad58-1330">US</span></span> | <span data-ttu-id="8ad58-1331">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1331">In</span></span> | <span data-ttu-id="8ad58-1332">UI</span><span class="sxs-lookup"><span data-stu-id="8ad58-1332">UI</span></span> | <span data-ttu-id="8ad58-1333">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1333">Lo</span></span> | <span data-ttu-id="8ad58-1334">UL</span><span class="sxs-lookup"><span data-stu-id="8ad58-1334">UL</span></span> | <span data-ttu-id="8ad58-1335">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1335">De</span></span> | <span data-ttu-id="8ad58-1336">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1336">Si</span></span> | <span data-ttu-id="8ad58-1337">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1337">Do</span></span> | <span data-ttu-id="8ad58-1338">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1338">Err</span></span> | <span data-ttu-id="8ad58-1339">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1339">Err</span></span> | <span data-ttu-id="8ad58-1340">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1340">Do</span></span>  | <span data-ttu-id="8ad58-1341">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1341">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1342">__悪夢__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1342">__Sh__</span></span> |    |    |    | <span data-ttu-id="8ad58-1343">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-1343">Sh</span></span> | <span data-ttu-id="8ad58-1344">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1344">In</span></span> | <span data-ttu-id="8ad58-1345">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1345">In</span></span> | <span data-ttu-id="8ad58-1346">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1346">Lo</span></span> | <span data-ttu-id="8ad58-1347">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1347">Lo</span></span> | <span data-ttu-id="8ad58-1348">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1348">De</span></span> | <span data-ttu-id="8ad58-1349">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1349">De</span></span> | <span data-ttu-id="8ad58-1350">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1350">Si</span></span> | <span data-ttu-id="8ad58-1351">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1351">Do</span></span> | <span data-ttu-id="8ad58-1352">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1352">Err</span></span> | <span data-ttu-id="8ad58-1353">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1353">Err</span></span> | <span data-ttu-id="8ad58-1354">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1354">Do</span></span>  | <span data-ttu-id="8ad58-1355">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1355">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1356">__米国__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1356">__US__</span></span> |    |    |    |    | <span data-ttu-id="8ad58-1357">US</span><span class="sxs-lookup"><span data-stu-id="8ad58-1357">US</span></span> | <span data-ttu-id="8ad58-1358">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1358">In</span></span> | <span data-ttu-id="8ad58-1359">UI</span><span class="sxs-lookup"><span data-stu-id="8ad58-1359">UI</span></span> | <span data-ttu-id="8ad58-1360">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1360">Lo</span></span> | <span data-ttu-id="8ad58-1361">UL</span><span class="sxs-lookup"><span data-stu-id="8ad58-1361">UL</span></span> | <span data-ttu-id="8ad58-1362">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1362">De</span></span> | <span data-ttu-id="8ad58-1363">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1363">Si</span></span> | <span data-ttu-id="8ad58-1364">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1364">Do</span></span> | <span data-ttu-id="8ad58-1365">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1365">Err</span></span> | <span data-ttu-id="8ad58-1366">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1366">Err</span></span> | <span data-ttu-id="8ad58-1367">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1367">Do</span></span>  | <span data-ttu-id="8ad58-1368">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1368">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1369">__In__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1369">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="8ad58-1370">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1370">In</span></span> | <span data-ttu-id="8ad58-1371">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1371">Lo</span></span> | <span data-ttu-id="8ad58-1372">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1372">Lo</span></span> | <span data-ttu-id="8ad58-1373">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1373">De</span></span> | <span data-ttu-id="8ad58-1374">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1374">De</span></span> | <span data-ttu-id="8ad58-1375">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1375">Si</span></span> | <span data-ttu-id="8ad58-1376">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1376">Do</span></span> | <span data-ttu-id="8ad58-1377">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1377">Err</span></span> | <span data-ttu-id="8ad58-1378">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1378">Err</span></span> | <span data-ttu-id="8ad58-1379">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1379">Do</span></span>  | <span data-ttu-id="8ad58-1380">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1380">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1381">__UI__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1381">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="8ad58-1382">UI</span><span class="sxs-lookup"><span data-stu-id="8ad58-1382">UI</span></span> | <span data-ttu-id="8ad58-1383">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1383">Lo</span></span> | <span data-ttu-id="8ad58-1384">UL</span><span class="sxs-lookup"><span data-stu-id="8ad58-1384">UL</span></span> | <span data-ttu-id="8ad58-1385">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1385">De</span></span> | <span data-ttu-id="8ad58-1386">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1386">Si</span></span> | <span data-ttu-id="8ad58-1387">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1387">Do</span></span> | <span data-ttu-id="8ad58-1388">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1388">Err</span></span> | <span data-ttu-id="8ad58-1389">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1389">Err</span></span> | <span data-ttu-id="8ad58-1390">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1390">Do</span></span>  | <span data-ttu-id="8ad58-1391">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1391">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1392">__Lo__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1392">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1393">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1393">Lo</span></span> | <span data-ttu-id="8ad58-1394">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1394">De</span></span> | <span data-ttu-id="8ad58-1395">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1395">De</span></span> | <span data-ttu-id="8ad58-1396">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1396">Si</span></span> | <span data-ttu-id="8ad58-1397">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1397">Do</span></span> | <span data-ttu-id="8ad58-1398">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1398">Err</span></span> | <span data-ttu-id="8ad58-1399">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1399">Err</span></span> | <span data-ttu-id="8ad58-1400">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1400">Do</span></span>  | <span data-ttu-id="8ad58-1401">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1401">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1402">__UL__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1402">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1403">UL</span><span class="sxs-lookup"><span data-stu-id="8ad58-1403">UL</span></span> | <span data-ttu-id="8ad58-1404">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1404">De</span></span> | <span data-ttu-id="8ad58-1405">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1405">Si</span></span> | <span data-ttu-id="8ad58-1406">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1406">Do</span></span> | <span data-ttu-id="8ad58-1407">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1407">Err</span></span> | <span data-ttu-id="8ad58-1408">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1408">Err</span></span> | <span data-ttu-id="8ad58-1409">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1409">Do</span></span>  | <span data-ttu-id="8ad58-1410">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1410">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1411">__解放__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1411">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1412">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1412">De</span></span> | <span data-ttu-id="8ad58-1413">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1413">Si</span></span> | <span data-ttu-id="8ad58-1414">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1414">Do</span></span> | <span data-ttu-id="8ad58-1415">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1415">Err</span></span> | <span data-ttu-id="8ad58-1416">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1416">Err</span></span> | <span data-ttu-id="8ad58-1417">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1417">Do</span></span>  | <span data-ttu-id="8ad58-1418">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1418">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1419">__Si__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1419">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1420">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1420">Si</span></span> | <span data-ttu-id="8ad58-1421">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1421">Do</span></span> | <span data-ttu-id="8ad58-1422">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1422">Err</span></span> | <span data-ttu-id="8ad58-1423">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1423">Err</span></span> | <span data-ttu-id="8ad58-1424">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1424">Do</span></span>  | <span data-ttu-id="8ad58-1425">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1425">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1426">__Do__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1426">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1427">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1427">Do</span></span> | <span data-ttu-id="8ad58-1428">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1428">Err</span></span> | <span data-ttu-id="8ad58-1429">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1429">Err</span></span> | <span data-ttu-id="8ad58-1430">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1430">Do</span></span>  | <span data-ttu-id="8ad58-1431">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1431">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1432">__オーディオ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1432">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1433">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1433">Err</span></span> | <span data-ttu-id="8ad58-1434">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1434">Err</span></span> | <span data-ttu-id="8ad58-1435">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1435">Err</span></span> | <span data-ttu-id="8ad58-1436">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1436">Err</span></span> | 
| <span data-ttu-id="8ad58-1437">__ハーフ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1437">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="8ad58-1438">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1438">Err</span></span> | <span data-ttu-id="8ad58-1439">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1439">Err</span></span> | <span data-ttu-id="8ad58-1440">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1440">Err</span></span> | 
| <span data-ttu-id="8ad58-1441">__&__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1441">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="8ad58-1442">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1442">Do</span></span>  | <span data-ttu-id="8ad58-1443">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1443">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1444">__Ob__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1444">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="8ad58-1445">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1445">Ob</span></span>  | 


### <a name="division-operators"></a><span data-ttu-id="8ad58-1446">除算演算子</span><span class="sxs-lookup"><span data-stu-id="8ad58-1446">Division Operators</span></span>

<span data-ttu-id="8ad58-1447">除算演算子は、2つのオペランドの商を計算します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1447">Division operators compute the quotient of two operands.</span></span> <span data-ttu-id="8ad58-1448">除算演算子には、標準 (浮動小数点) 除算演算子と整数除算演算子の2つがあります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1448">There are two division operators: the regular (floating-point) division operator and the integer division operator.</span></span>

```antlr
DivisionOperatorExpression
    : FPDivisionOperatorExpression
    | IntegerDivisionOperatorExpression
    ;

FPDivisionOperatorExpression
    : Expression '/' LineTerminator? Expression
    ;

IntegerDivisionOperatorExpression
    : Expression '\\' LineTerminator? Expression
    ;
```

<span data-ttu-id="8ad58-1449">通常の除算演算子は、次の型に対して定義されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1449">The regular division operator is defined for the following types:</span></span>

* <span data-ttu-id="8ad58-1450">`Single` および `Double`。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1450">`Single` and `Double`.</span></span> <span data-ttu-id="8ad58-1451">商は、IEEE 754 算術のルールに従って計算されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1451">The quotient is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="8ad58-1452">`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1452">`Decimal`.</span></span> <span data-ttu-id="8ad58-1453">右オペランドの値が0の場合は、@no__t 0 の例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1453">If the value of the right operand is zero, a `System.DivideByZeroException` exception is thrown.</span></span> <span data-ttu-id="8ad58-1454">結果の値が大きすぎて decimal 形式で表現できない場合は、@no__t 0 の例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1454">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="8ad58-1455">小数点以下の値が小さすぎる場合、結果は0になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1455">If the result value is too small to represent in the decimal format, the result is zero.</span></span> <span data-ttu-id="8ad58-1456">結果の小数点以下の小数点以下桁数は、最適なスケールに最も近いスケールになり、結果が正確な結果と同じになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1456">The scale of the result, before any rounding, is the closest scale to the preferred scale which will preserve a result equal to the exact result.</span></span>  <span data-ttu-id="8ad58-1457">優先する小数点以下桁数は、2番目のオペランドの小数点以下桁数よりも小さい1番目のオペランドの小数点以下桁数です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1457">The preferred scale is the scale of the first operand less the scale of the second operand.</span></span>

<span data-ttu-id="8ad58-1458">通常の演算子の解決規則に従って、`Byte`、`Short`、`Integer`、`Long` などの型のオペランドの間に純粋に分割されると、両方のオペランドが型 `Decimal` に変換されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1458">According to normal operator resolution rules, regular division purely between operands of types such as `Byte`, `Short`, `Integer`, and `Long` would cause both operands to be converted to type `Decimal`.</span></span> <span data-ttu-id="8ad58-1459">ただし、いずれの型も `Decimal` でない場合に除算演算子に対して演算子の解決を実行すると、`Double` は `Decimal` よりも狭く見なされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1459">However, when doing operator resolution on the division operator when neither type is `Decimal`, `Double` is considered narrower than `Decimal`.</span></span> <span data-ttu-id="8ad58-1460">この規則に従うのは、`Double` 除算が `Decimal` 除算よりも効率的であるためです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1460">This convention is followed because `Double` division is more efficient than `Decimal` division.</span></span>

<span data-ttu-id="8ad58-1461">__操作の種類:__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1461">__Operation Type:__</span></span>

|        | <span data-ttu-id="8ad58-1462">__コンボ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1462">__Bo__</span></span> | <span data-ttu-id="8ad58-1463">__SB__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1463">__SB__</span></span> | <span data-ttu-id="8ad58-1464">__は__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1464">__By__</span></span> | <span data-ttu-id="8ad58-1465">__悪夢__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1465">__Sh__</span></span> | <span data-ttu-id="8ad58-1466">__米国__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1466">__US__</span></span> | <span data-ttu-id="8ad58-1467">__In__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1467">__In__</span></span> | <span data-ttu-id="8ad58-1468">__UI__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1468">__UI__</span></span> | <span data-ttu-id="8ad58-1469">__Lo__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1469">__Lo__</span></span> | <span data-ttu-id="8ad58-1470">__UL__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1470">__UL__</span></span> | <span data-ttu-id="8ad58-1471">__解放__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1471">__De__</span></span> | <span data-ttu-id="8ad58-1472">__Si__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1472">__Si__</span></span> | <span data-ttu-id="8ad58-1473">__Do__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1473">__Do__</span></span> | <span data-ttu-id="8ad58-1474">__オーディオ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1474">__Da__</span></span>  | <span data-ttu-id="8ad58-1475">__ハーフ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1475">__Ch__</span></span>  | <span data-ttu-id="8ad58-1476">__&__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1476">__St__</span></span> | <span data-ttu-id="8ad58-1477">__Ob__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1477">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="8ad58-1478">__コンボ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1478">__Bo__</span></span> | <span data-ttu-id="8ad58-1479">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1479">Do</span></span> | <span data-ttu-id="8ad58-1480">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1480">Do</span></span> | <span data-ttu-id="8ad58-1481">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1481">Do</span></span> | <span data-ttu-id="8ad58-1482">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1482">Do</span></span> | <span data-ttu-id="8ad58-1483">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1483">Do</span></span> | <span data-ttu-id="8ad58-1484">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1484">Do</span></span> | <span data-ttu-id="8ad58-1485">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1485">Do</span></span> | <span data-ttu-id="8ad58-1486">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1486">Do</span></span> | <span data-ttu-id="8ad58-1487">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1487">Do</span></span> | <span data-ttu-id="8ad58-1488">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1488">De</span></span> | <span data-ttu-id="8ad58-1489">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1489">Si</span></span> | <span data-ttu-id="8ad58-1490">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1490">Do</span></span> | <span data-ttu-id="8ad58-1491">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1491">Err</span></span> | <span data-ttu-id="8ad58-1492">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1492">Err</span></span> | <span data-ttu-id="8ad58-1493">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1493">Do</span></span>  | <span data-ttu-id="8ad58-1494">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1494">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1495">__SB__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1495">__SB__</span></span> |    | <span data-ttu-id="8ad58-1496">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1496">Do</span></span> | <span data-ttu-id="8ad58-1497">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1497">Do</span></span> | <span data-ttu-id="8ad58-1498">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1498">Do</span></span> | <span data-ttu-id="8ad58-1499">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1499">Do</span></span> | <span data-ttu-id="8ad58-1500">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1500">Do</span></span> | <span data-ttu-id="8ad58-1501">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1501">Do</span></span> | <span data-ttu-id="8ad58-1502">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1502">Do</span></span> | <span data-ttu-id="8ad58-1503">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1503">Do</span></span> | <span data-ttu-id="8ad58-1504">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1504">De</span></span> | <span data-ttu-id="8ad58-1505">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1505">Si</span></span> | <span data-ttu-id="8ad58-1506">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1506">Do</span></span> | <span data-ttu-id="8ad58-1507">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1507">Err</span></span> | <span data-ttu-id="8ad58-1508">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1508">Err</span></span> | <span data-ttu-id="8ad58-1509">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1509">Do</span></span>  | <span data-ttu-id="8ad58-1510">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1510">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1511">__は__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1511">__By__</span></span> |    |    | <span data-ttu-id="8ad58-1512">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1512">Do</span></span> | <span data-ttu-id="8ad58-1513">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1513">Do</span></span> | <span data-ttu-id="8ad58-1514">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1514">Do</span></span> | <span data-ttu-id="8ad58-1515">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1515">Do</span></span> | <span data-ttu-id="8ad58-1516">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1516">Do</span></span> | <span data-ttu-id="8ad58-1517">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1517">Do</span></span> | <span data-ttu-id="8ad58-1518">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1518">Do</span></span> | <span data-ttu-id="8ad58-1519">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1519">De</span></span> | <span data-ttu-id="8ad58-1520">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1520">Si</span></span> | <span data-ttu-id="8ad58-1521">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1521">Do</span></span> | <span data-ttu-id="8ad58-1522">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1522">Err</span></span> | <span data-ttu-id="8ad58-1523">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1523">Err</span></span> | <span data-ttu-id="8ad58-1524">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1524">Do</span></span>  | <span data-ttu-id="8ad58-1525">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1525">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1526">__悪夢__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1526">__Sh__</span></span> |    |    |    | <span data-ttu-id="8ad58-1527">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1527">Do</span></span> | <span data-ttu-id="8ad58-1528">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1528">Do</span></span> | <span data-ttu-id="8ad58-1529">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1529">Do</span></span> | <span data-ttu-id="8ad58-1530">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1530">Do</span></span> | <span data-ttu-id="8ad58-1531">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1531">Do</span></span> | <span data-ttu-id="8ad58-1532">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1532">Do</span></span> | <span data-ttu-id="8ad58-1533">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1533">De</span></span> | <span data-ttu-id="8ad58-1534">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1534">Si</span></span> | <span data-ttu-id="8ad58-1535">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1535">Do</span></span> | <span data-ttu-id="8ad58-1536">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1536">Err</span></span> | <span data-ttu-id="8ad58-1537">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1537">Err</span></span> | <span data-ttu-id="8ad58-1538">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1538">Do</span></span>  | <span data-ttu-id="8ad58-1539">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1539">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1540">__米国__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1540">__US__</span></span> |    |    |    |    | <span data-ttu-id="8ad58-1541">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1541">Do</span></span> | <span data-ttu-id="8ad58-1542">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1542">Do</span></span> | <span data-ttu-id="8ad58-1543">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1543">Do</span></span> | <span data-ttu-id="8ad58-1544">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1544">Do</span></span> | <span data-ttu-id="8ad58-1545">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1545">Do</span></span> | <span data-ttu-id="8ad58-1546">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1546">De</span></span> | <span data-ttu-id="8ad58-1547">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1547">Si</span></span> | <span data-ttu-id="8ad58-1548">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1548">Do</span></span> | <span data-ttu-id="8ad58-1549">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1549">Err</span></span> | <span data-ttu-id="8ad58-1550">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1550">Err</span></span> | <span data-ttu-id="8ad58-1551">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1551">Do</span></span>  | <span data-ttu-id="8ad58-1552">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1552">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1553">__In__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1553">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="8ad58-1554">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1554">Do</span></span> | <span data-ttu-id="8ad58-1555">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1555">Do</span></span> | <span data-ttu-id="8ad58-1556">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1556">Do</span></span> | <span data-ttu-id="8ad58-1557">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1557">Do</span></span> | <span data-ttu-id="8ad58-1558">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1558">De</span></span> | <span data-ttu-id="8ad58-1559">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1559">Si</span></span> | <span data-ttu-id="8ad58-1560">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1560">Do</span></span> | <span data-ttu-id="8ad58-1561">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1561">Err</span></span> | <span data-ttu-id="8ad58-1562">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1562">Err</span></span> | <span data-ttu-id="8ad58-1563">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1563">Do</span></span>  | <span data-ttu-id="8ad58-1564">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1564">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1565">__UI__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1565">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="8ad58-1566">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1566">Do</span></span> | <span data-ttu-id="8ad58-1567">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1567">Do</span></span> | <span data-ttu-id="8ad58-1568">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1568">Do</span></span> | <span data-ttu-id="8ad58-1569">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1569">De</span></span> | <span data-ttu-id="8ad58-1570">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1570">Si</span></span> | <span data-ttu-id="8ad58-1571">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1571">Do</span></span> | <span data-ttu-id="8ad58-1572">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1572">Err</span></span> | <span data-ttu-id="8ad58-1573">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1573">Err</span></span> | <span data-ttu-id="8ad58-1574">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1574">Do</span></span>  | <span data-ttu-id="8ad58-1575">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1575">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1576">__Lo__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1576">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1577">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1577">Do</span></span> | <span data-ttu-id="8ad58-1578">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1578">Do</span></span> | <span data-ttu-id="8ad58-1579">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1579">De</span></span> | <span data-ttu-id="8ad58-1580">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1580">Si</span></span> | <span data-ttu-id="8ad58-1581">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1581">Do</span></span> | <span data-ttu-id="8ad58-1582">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1582">Err</span></span> | <span data-ttu-id="8ad58-1583">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1583">Err</span></span> | <span data-ttu-id="8ad58-1584">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1584">Do</span></span>  | <span data-ttu-id="8ad58-1585">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1585">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1586">__UL__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1586">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1587">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1587">Do</span></span> | <span data-ttu-id="8ad58-1588">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1588">De</span></span> | <span data-ttu-id="8ad58-1589">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1589">Si</span></span> | <span data-ttu-id="8ad58-1590">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1590">Do</span></span> | <span data-ttu-id="8ad58-1591">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1591">Err</span></span> | <span data-ttu-id="8ad58-1592">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1592">Err</span></span> | <span data-ttu-id="8ad58-1593">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1593">Do</span></span>  | <span data-ttu-id="8ad58-1594">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1594">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1595">__解放__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1595">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1596">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1596">De</span></span> | <span data-ttu-id="8ad58-1597">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1597">Si</span></span> | <span data-ttu-id="8ad58-1598">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1598">Do</span></span> | <span data-ttu-id="8ad58-1599">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1599">Err</span></span> | <span data-ttu-id="8ad58-1600">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1600">Err</span></span> | <span data-ttu-id="8ad58-1601">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1601">Do</span></span>  | <span data-ttu-id="8ad58-1602">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1602">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1603">__Si__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1603">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1604">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1604">Si</span></span> | <span data-ttu-id="8ad58-1605">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1605">Do</span></span> | <span data-ttu-id="8ad58-1606">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1606">Err</span></span> | <span data-ttu-id="8ad58-1607">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1607">Err</span></span> | <span data-ttu-id="8ad58-1608">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1608">Do</span></span>  | <span data-ttu-id="8ad58-1609">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1609">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1610">__Do__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1610">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1611">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1611">Do</span></span> | <span data-ttu-id="8ad58-1612">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1612">Err</span></span> | <span data-ttu-id="8ad58-1613">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1613">Err</span></span> | <span data-ttu-id="8ad58-1614">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1614">Do</span></span>  | <span data-ttu-id="8ad58-1615">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1615">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1616">__オーディオ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1616">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1617">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1617">Err</span></span> | <span data-ttu-id="8ad58-1618">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1618">Err</span></span> | <span data-ttu-id="8ad58-1619">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1619">Err</span></span> | <span data-ttu-id="8ad58-1620">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1620">Err</span></span> | 
| <span data-ttu-id="8ad58-1621">__ハーフ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1621">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="8ad58-1622">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1622">Err</span></span> | <span data-ttu-id="8ad58-1623">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1623">Err</span></span> | <span data-ttu-id="8ad58-1624">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1624">Err</span></span> | 
| <span data-ttu-id="8ad58-1625">__&__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1625">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="8ad58-1626">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1626">Do</span></span>  | <span data-ttu-id="8ad58-1627">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1627">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1628">__Ob__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1628">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="8ad58-1629">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1629">Ob</span></span>  | 

<span data-ttu-id="8ad58-1630">整数除算演算子は `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、および @no__t に対して定義されています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1630">The integer division operator is defined for `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="8ad58-1631">右オペランドの値が0の場合は、@no__t 0 の例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1631">If the value of the right operand is zero, a `System.DivideByZeroException` exception is thrown.</span></span> <span data-ttu-id="8ad58-1632">除算は、結果を0方向に丸め、結果の絶対値は、2つのオペランドの商の絶対値より小さい最大の整数になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1632">The division rounds the result towards zero, and the absolute value of the result is the largest possible integer that is less than the absolute value of the quotient of the two operands.</span></span> <span data-ttu-id="8ad58-1633">2つのオペランドの符号が同じ場合、結果は0または正になります。2つのオペランドの符号が逆の場合は、0または負の値になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1633">The result is zero or positive when the two operands have the same sign, and zero or negative when the two operands have opposite signs.</span></span> <span data-ttu-id="8ad58-1634">左側のオペランドが最大値 `SByte`、`Short`、`Integer`、または `Long` で、右オペランドが 4 @no__t の場合は、オーバーフローが発生します。整数オーバーフローのチェックがオンになっている場合は、@no__t 5 の例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1634">If the left operand is the maximum negative `SByte`, `Short`, `Integer`, or `Long`, and the right operand is `-1`, an overflow occurs; if integer overflow checking is on, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="8ad58-1635">それ以外の場合、オーバーフローは報告されず、結果は左オペランドの値になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1635">Otherwise, the overflow is not reported and the result is instead the value of the left operand.</span></span>

<span data-ttu-id="8ad58-1636">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1636">__Note.__</span></span> <span data-ttu-id="8ad58-1637">符号なしの型の2つのオペランドは常に0または正の値になるため、結果は常に0または正になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1637">As the two operands for unsigned types will always be zero or positive, the result is always zero or positive.</span></span>  <span data-ttu-id="8ad58-1638">式の結果は常に2つのオペランドのうちの最大値以下であるため、オーバーフローが発生する可能性はありません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1638">As the result of the expression will always be less than or equal to the largest of the two operands, it is not possible for an overflow to occur.</span></span>  <span data-ttu-id="8ad58-1639">そのため、2つの符号なし整数を使用した整数除算では、このような整数オーバーフローチェックは実行されません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1639">As such integer overflow checking is not performed for integer divide with two unsigned integers.</span></span> <span data-ttu-id="8ad58-1640">結果は、左オペランドの型と同じになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1640">The result is the type as that of the left operand.</span></span>


<span data-ttu-id="8ad58-1641">__操作の種類:__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1641">__Operation Type:__</span></span>

|        | <span data-ttu-id="8ad58-1642">__コンボ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1642">__Bo__</span></span> | <span data-ttu-id="8ad58-1643">__SB__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1643">__SB__</span></span> | <span data-ttu-id="8ad58-1644">__は__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1644">__By__</span></span> | <span data-ttu-id="8ad58-1645">__悪夢__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1645">__Sh__</span></span> | <span data-ttu-id="8ad58-1646">__米国__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1646">__US__</span></span> | <span data-ttu-id="8ad58-1647">__In__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1647">__In__</span></span> | <span data-ttu-id="8ad58-1648">__UI__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1648">__UI__</span></span> | <span data-ttu-id="8ad58-1649">__Lo__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1649">__Lo__</span></span> | <span data-ttu-id="8ad58-1650">__UL__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1650">__UL__</span></span> | <span data-ttu-id="8ad58-1651">__解放__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1651">__De__</span></span> | <span data-ttu-id="8ad58-1652">__Si__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1652">__Si__</span></span> | <span data-ttu-id="8ad58-1653">__Do__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1653">__Do__</span></span> | <span data-ttu-id="8ad58-1654">__オーディオ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1654">__Da__</span></span>  | <span data-ttu-id="8ad58-1655">__ハーフ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1655">__Ch__</span></span>  | <span data-ttu-id="8ad58-1656">__&__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1656">__St__</span></span> | <span data-ttu-id="8ad58-1657">__Ob__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1657">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="8ad58-1658">__コンボ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1658">__Bo__</span></span> | <span data-ttu-id="8ad58-1659">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-1659">Sh</span></span> | <span data-ttu-id="8ad58-1660">SB</span><span class="sxs-lookup"><span data-stu-id="8ad58-1660">SB</span></span> | <span data-ttu-id="8ad58-1661">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-1661">Sh</span></span> | <span data-ttu-id="8ad58-1662">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-1662">Sh</span></span> | <span data-ttu-id="8ad58-1663">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1663">In</span></span> | <span data-ttu-id="8ad58-1664">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1664">In</span></span> | <span data-ttu-id="8ad58-1665">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1665">Lo</span></span> | <span data-ttu-id="8ad58-1666">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1666">Lo</span></span> | <span data-ttu-id="8ad58-1667">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1667">Lo</span></span> | <span data-ttu-id="8ad58-1668">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1668">Lo</span></span> | <span data-ttu-id="8ad58-1669">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1669">Lo</span></span> | <span data-ttu-id="8ad58-1670">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1670">Lo</span></span> | <span data-ttu-id="8ad58-1671">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1671">Err</span></span> | <span data-ttu-id="8ad58-1672">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1672">Err</span></span> | <span data-ttu-id="8ad58-1673">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1673">Lo</span></span>  | <span data-ttu-id="8ad58-1674">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1674">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1675">__SB__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1675">__SB__</span></span> |    | <span data-ttu-id="8ad58-1676">SB</span><span class="sxs-lookup"><span data-stu-id="8ad58-1676">SB</span></span> | <span data-ttu-id="8ad58-1677">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-1677">Sh</span></span> | <span data-ttu-id="8ad58-1678">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-1678">Sh</span></span> | <span data-ttu-id="8ad58-1679">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1679">In</span></span> | <span data-ttu-id="8ad58-1680">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1680">In</span></span> | <span data-ttu-id="8ad58-1681">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1681">Lo</span></span> | <span data-ttu-id="8ad58-1682">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1682">Lo</span></span> | <span data-ttu-id="8ad58-1683">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1683">Lo</span></span> | <span data-ttu-id="8ad58-1684">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1684">Lo</span></span> | <span data-ttu-id="8ad58-1685">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1685">Lo</span></span> | <span data-ttu-id="8ad58-1686">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1686">Lo</span></span> | <span data-ttu-id="8ad58-1687">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1687">Err</span></span> | <span data-ttu-id="8ad58-1688">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1688">Err</span></span> | <span data-ttu-id="8ad58-1689">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1689">Lo</span></span>  | <span data-ttu-id="8ad58-1690">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1690">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1691">__は__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1691">__By__</span></span> |    |    | <span data-ttu-id="8ad58-1692">By</span><span class="sxs-lookup"><span data-stu-id="8ad58-1692">By</span></span> | <span data-ttu-id="8ad58-1693">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-1693">Sh</span></span> | <span data-ttu-id="8ad58-1694">US</span><span class="sxs-lookup"><span data-stu-id="8ad58-1694">US</span></span> | <span data-ttu-id="8ad58-1695">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1695">In</span></span> | <span data-ttu-id="8ad58-1696">UI</span><span class="sxs-lookup"><span data-stu-id="8ad58-1696">UI</span></span> | <span data-ttu-id="8ad58-1697">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1697">Lo</span></span> | <span data-ttu-id="8ad58-1698">UL</span><span class="sxs-lookup"><span data-stu-id="8ad58-1698">UL</span></span> | <span data-ttu-id="8ad58-1699">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1699">Lo</span></span> | <span data-ttu-id="8ad58-1700">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1700">Lo</span></span> | <span data-ttu-id="8ad58-1701">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1701">Lo</span></span> | <span data-ttu-id="8ad58-1702">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1702">Err</span></span> | <span data-ttu-id="8ad58-1703">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1703">Err</span></span> | <span data-ttu-id="8ad58-1704">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1704">Lo</span></span>  | <span data-ttu-id="8ad58-1705">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1705">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1706">__悪夢__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1706">__Sh__</span></span> |    |    |    | <span data-ttu-id="8ad58-1707">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-1707">Sh</span></span> | <span data-ttu-id="8ad58-1708">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1708">In</span></span> | <span data-ttu-id="8ad58-1709">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1709">In</span></span> | <span data-ttu-id="8ad58-1710">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1710">Lo</span></span> | <span data-ttu-id="8ad58-1711">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1711">Lo</span></span> | <span data-ttu-id="8ad58-1712">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1712">Lo</span></span> | <span data-ttu-id="8ad58-1713">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1713">Lo</span></span> | <span data-ttu-id="8ad58-1714">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1714">Lo</span></span> | <span data-ttu-id="8ad58-1715">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1715">Lo</span></span> | <span data-ttu-id="8ad58-1716">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1716">Err</span></span> | <span data-ttu-id="8ad58-1717">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1717">Err</span></span> | <span data-ttu-id="8ad58-1718">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1718">Lo</span></span>  | <span data-ttu-id="8ad58-1719">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1719">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1720">__米国__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1720">__US__</span></span> |    |    |    |    | <span data-ttu-id="8ad58-1721">US</span><span class="sxs-lookup"><span data-stu-id="8ad58-1721">US</span></span> | <span data-ttu-id="8ad58-1722">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1722">In</span></span> | <span data-ttu-id="8ad58-1723">UI</span><span class="sxs-lookup"><span data-stu-id="8ad58-1723">UI</span></span> | <span data-ttu-id="8ad58-1724">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1724">Lo</span></span> | <span data-ttu-id="8ad58-1725">UL</span><span class="sxs-lookup"><span data-stu-id="8ad58-1725">UL</span></span> | <span data-ttu-id="8ad58-1726">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1726">Lo</span></span> | <span data-ttu-id="8ad58-1727">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1727">Lo</span></span> | <span data-ttu-id="8ad58-1728">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1728">Lo</span></span> | <span data-ttu-id="8ad58-1729">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1729">Err</span></span> | <span data-ttu-id="8ad58-1730">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1730">Err</span></span> | <span data-ttu-id="8ad58-1731">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1731">Lo</span></span>  | <span data-ttu-id="8ad58-1732">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1732">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1733">__In__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1733">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="8ad58-1734">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1734">In</span></span> | <span data-ttu-id="8ad58-1735">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1735">Lo</span></span> | <span data-ttu-id="8ad58-1736">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1736">Lo</span></span> | <span data-ttu-id="8ad58-1737">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1737">Lo</span></span> | <span data-ttu-id="8ad58-1738">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1738">Lo</span></span> | <span data-ttu-id="8ad58-1739">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1739">Lo</span></span> | <span data-ttu-id="8ad58-1740">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1740">Lo</span></span> | <span data-ttu-id="8ad58-1741">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1741">Err</span></span> | <span data-ttu-id="8ad58-1742">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1742">Err</span></span> | <span data-ttu-id="8ad58-1743">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1743">Lo</span></span>  | <span data-ttu-id="8ad58-1744">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1744">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1745">__UI__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1745">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="8ad58-1746">UI</span><span class="sxs-lookup"><span data-stu-id="8ad58-1746">UI</span></span> | <span data-ttu-id="8ad58-1747">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1747">Lo</span></span> | <span data-ttu-id="8ad58-1748">UL</span><span class="sxs-lookup"><span data-stu-id="8ad58-1748">UL</span></span> | <span data-ttu-id="8ad58-1749">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1749">Lo</span></span> | <span data-ttu-id="8ad58-1750">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1750">Lo</span></span> | <span data-ttu-id="8ad58-1751">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1751">Lo</span></span> | <span data-ttu-id="8ad58-1752">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1752">Err</span></span> | <span data-ttu-id="8ad58-1753">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1753">Err</span></span> | <span data-ttu-id="8ad58-1754">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1754">Lo</span></span>  | <span data-ttu-id="8ad58-1755">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1755">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1756">__Lo__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1756">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1757">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1757">Lo</span></span> | <span data-ttu-id="8ad58-1758">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1758">Lo</span></span> | <span data-ttu-id="8ad58-1759">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1759">Lo</span></span> | <span data-ttu-id="8ad58-1760">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1760">Lo</span></span> | <span data-ttu-id="8ad58-1761">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1761">Lo</span></span> | <span data-ttu-id="8ad58-1762">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1762">Err</span></span> | <span data-ttu-id="8ad58-1763">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1763">Err</span></span> | <span data-ttu-id="8ad58-1764">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1764">Lo</span></span>  | <span data-ttu-id="8ad58-1765">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1765">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1766">__UL__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1766">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1767">UL</span><span class="sxs-lookup"><span data-stu-id="8ad58-1767">UL</span></span> | <span data-ttu-id="8ad58-1768">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1768">Lo</span></span> | <span data-ttu-id="8ad58-1769">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1769">Lo</span></span> | <span data-ttu-id="8ad58-1770">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1770">Lo</span></span> | <span data-ttu-id="8ad58-1771">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1771">Err</span></span> | <span data-ttu-id="8ad58-1772">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1772">Err</span></span> | <span data-ttu-id="8ad58-1773">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1773">Lo</span></span>  | <span data-ttu-id="8ad58-1774">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1774">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1775">__解放__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1775">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1776">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1776">Lo</span></span> | <span data-ttu-id="8ad58-1777">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1777">Lo</span></span> | <span data-ttu-id="8ad58-1778">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1778">Lo</span></span> | <span data-ttu-id="8ad58-1779">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1779">Err</span></span> | <span data-ttu-id="8ad58-1780">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1780">Err</span></span> | <span data-ttu-id="8ad58-1781">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1781">Lo</span></span>  | <span data-ttu-id="8ad58-1782">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1782">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1783">__Si__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1783">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1784">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1784">Lo</span></span> | <span data-ttu-id="8ad58-1785">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1785">Lo</span></span> | <span data-ttu-id="8ad58-1786">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1786">Err</span></span> | <span data-ttu-id="8ad58-1787">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1787">Err</span></span> | <span data-ttu-id="8ad58-1788">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1788">Lo</span></span>  | <span data-ttu-id="8ad58-1789">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1789">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1790">__Do__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1790">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1791">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1791">Lo</span></span> | <span data-ttu-id="8ad58-1792">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1792">Err</span></span> | <span data-ttu-id="8ad58-1793">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1793">Err</span></span> | <span data-ttu-id="8ad58-1794">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1794">Lo</span></span>  | <span data-ttu-id="8ad58-1795">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1795">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1796">__オーディオ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1796">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1797">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1797">Err</span></span> | <span data-ttu-id="8ad58-1798">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1798">Err</span></span> | <span data-ttu-id="8ad58-1799">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1799">Err</span></span> | <span data-ttu-id="8ad58-1800">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1800">Err</span></span> | 
| <span data-ttu-id="8ad58-1801">__ハーフ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1801">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="8ad58-1802">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1802">Err</span></span> | <span data-ttu-id="8ad58-1803">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1803">Err</span></span> | <span data-ttu-id="8ad58-1804">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1804">Err</span></span> | 
| <span data-ttu-id="8ad58-1805">__&__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1805">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="8ad58-1806">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1806">Lo</span></span>  | <span data-ttu-id="8ad58-1807">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1807">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1808">__Ob__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1808">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="8ad58-1809">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1809">Ob</span></span>  | 


### <a name="mod-operator"></a><span data-ttu-id="8ad58-1810">Mod 演算子</span><span class="sxs-lookup"><span data-stu-id="8ad58-1810">Mod Operator</span></span>

<span data-ttu-id="8ad58-1811">@No__t-0 (剰余) 演算子は、2つのオペランド間の除算の剰余を計算します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1811">The `Mod` (modulo) operator computes the remainder of the division between two operands.</span></span>

```antlr
ModuloOperatorExpression
    : Expression 'Mod' LineTerminator? Expression
    ;
```

<span data-ttu-id="8ad58-1812">@No__t-0 演算子は、次の型に対して定義されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1812">The `Mod` operator is defined for the following types:</span></span>

* <span data-ttu-id="8ad58-1813">`Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、および `Long`。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1813">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` and `Long`.</span></span> <span data-ttu-id="8ad58-1814">@No__t-0 の結果は `x - (x \ y) * y` によって生成される値になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1814">The result of `x Mod y` is the value produced by `x - (x \ y) * y`.</span></span> <span data-ttu-id="8ad58-1815">@No__t-0 が0の場合は、`System.DivideByZeroException` の例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1815">If `y` is zero, a `System.DivideByZeroException` exception is thrown.</span></span> <span data-ttu-id="8ad58-1816">剰余演算子はオーバーフローを発生させません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1816">The modulo operator never causes an overflow.</span></span>

* <span data-ttu-id="8ad58-1817">`Single` および `Double`。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1817">`Single` and `Double`.</span></span> <span data-ttu-id="8ad58-1818">剰余は、IEEE 754 算術のルールに従って計算されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1818">The remainder is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="8ad58-1819">`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1819">`Decimal`.</span></span> <span data-ttu-id="8ad58-1820">右オペランドの値が0の場合は、@no__t 0 の例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1820">If the value of the right operand is zero, a `System.DivideByZeroException` exception is thrown.</span></span> <span data-ttu-id="8ad58-1821">結果の値が大きすぎて decimal 形式で表現できない場合は、@no__t 0 の例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1821">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="8ad58-1822">小数点以下の値が小さすぎる場合、結果は0になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1822">If the result value is too small to represent in the decimal format, the result is zero.</span></span>

<span data-ttu-id="8ad58-1823">__操作の種類:__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1823">__Operation Type:__</span></span>

|        | <span data-ttu-id="8ad58-1824">__コンボ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1824">__Bo__</span></span> | <span data-ttu-id="8ad58-1825">__SB__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1825">__SB__</span></span> | <span data-ttu-id="8ad58-1826">__は__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1826">__By__</span></span> | <span data-ttu-id="8ad58-1827">__悪夢__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1827">__Sh__</span></span> | <span data-ttu-id="8ad58-1828">__米国__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1828">__US__</span></span> | <span data-ttu-id="8ad58-1829">__In__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1829">__In__</span></span> | <span data-ttu-id="8ad58-1830">__UI__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1830">__UI__</span></span> | <span data-ttu-id="8ad58-1831">__Lo__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1831">__Lo__</span></span> | <span data-ttu-id="8ad58-1832">__UL__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1832">__UL__</span></span> | <span data-ttu-id="8ad58-1833">__解放__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1833">__De__</span></span> | <span data-ttu-id="8ad58-1834">__Si__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1834">__Si__</span></span> | <span data-ttu-id="8ad58-1835">__Do__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1835">__Do__</span></span> | <span data-ttu-id="8ad58-1836">__オーディオ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1836">__Da__</span></span>  | <span data-ttu-id="8ad58-1837">__ハーフ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1837">__Ch__</span></span>  | <span data-ttu-id="8ad58-1838">__&__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1838">__St__</span></span> | <span data-ttu-id="8ad58-1839">__Ob__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1839">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="8ad58-1840">__コンボ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1840">__Bo__</span></span> | <span data-ttu-id="8ad58-1841">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-1841">Sh</span></span> | <span data-ttu-id="8ad58-1842">SB</span><span class="sxs-lookup"><span data-stu-id="8ad58-1842">SB</span></span> | <span data-ttu-id="8ad58-1843">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-1843">Sh</span></span> | <span data-ttu-id="8ad58-1844">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-1844">Sh</span></span> | <span data-ttu-id="8ad58-1845">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1845">In</span></span> | <span data-ttu-id="8ad58-1846">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1846">In</span></span> | <span data-ttu-id="8ad58-1847">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1847">Lo</span></span> | <span data-ttu-id="8ad58-1848">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1848">Lo</span></span> | <span data-ttu-id="8ad58-1849">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1849">De</span></span> | <span data-ttu-id="8ad58-1850">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1850">De</span></span> | <span data-ttu-id="8ad58-1851">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1851">Si</span></span> | <span data-ttu-id="8ad58-1852">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1852">Do</span></span> | <span data-ttu-id="8ad58-1853">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1853">Err</span></span> | <span data-ttu-id="8ad58-1854">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1854">Err</span></span> | <span data-ttu-id="8ad58-1855">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1855">Do</span></span>  | <span data-ttu-id="8ad58-1856">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1856">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1857">__SB__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1857">__SB__</span></span> |    | <span data-ttu-id="8ad58-1858">SB</span><span class="sxs-lookup"><span data-stu-id="8ad58-1858">SB</span></span> | <span data-ttu-id="8ad58-1859">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-1859">Sh</span></span> | <span data-ttu-id="8ad58-1860">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-1860">Sh</span></span> | <span data-ttu-id="8ad58-1861">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1861">In</span></span> | <span data-ttu-id="8ad58-1862">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1862">In</span></span> | <span data-ttu-id="8ad58-1863">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1863">Lo</span></span> | <span data-ttu-id="8ad58-1864">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1864">Lo</span></span> | <span data-ttu-id="8ad58-1865">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1865">De</span></span> | <span data-ttu-id="8ad58-1866">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1866">De</span></span> | <span data-ttu-id="8ad58-1867">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1867">Si</span></span> | <span data-ttu-id="8ad58-1868">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1868">Do</span></span> | <span data-ttu-id="8ad58-1869">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1869">Err</span></span> | <span data-ttu-id="8ad58-1870">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1870">Err</span></span> | <span data-ttu-id="8ad58-1871">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1871">Do</span></span>  | <span data-ttu-id="8ad58-1872">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1872">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1873">__は__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1873">__By__</span></span> |    |    | <span data-ttu-id="8ad58-1874">By</span><span class="sxs-lookup"><span data-stu-id="8ad58-1874">By</span></span> | <span data-ttu-id="8ad58-1875">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-1875">Sh</span></span> | <span data-ttu-id="8ad58-1876">US</span><span class="sxs-lookup"><span data-stu-id="8ad58-1876">US</span></span> | <span data-ttu-id="8ad58-1877">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1877">In</span></span> | <span data-ttu-id="8ad58-1878">UI</span><span class="sxs-lookup"><span data-stu-id="8ad58-1878">UI</span></span> | <span data-ttu-id="8ad58-1879">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1879">Lo</span></span> | <span data-ttu-id="8ad58-1880">UL</span><span class="sxs-lookup"><span data-stu-id="8ad58-1880">UL</span></span> | <span data-ttu-id="8ad58-1881">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1881">De</span></span> | <span data-ttu-id="8ad58-1882">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1882">Si</span></span> | <span data-ttu-id="8ad58-1883">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1883">Do</span></span> | <span data-ttu-id="8ad58-1884">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1884">Err</span></span> | <span data-ttu-id="8ad58-1885">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1885">Err</span></span> | <span data-ttu-id="8ad58-1886">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1886">Do</span></span>  | <span data-ttu-id="8ad58-1887">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1887">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1888">__悪夢__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1888">__Sh__</span></span> |    |    |    | <span data-ttu-id="8ad58-1889">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-1889">Sh</span></span> | <span data-ttu-id="8ad58-1890">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1890">In</span></span> | <span data-ttu-id="8ad58-1891">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1891">In</span></span> | <span data-ttu-id="8ad58-1892">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1892">Lo</span></span> | <span data-ttu-id="8ad58-1893">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1893">Lo</span></span> | <span data-ttu-id="8ad58-1894">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1894">De</span></span> | <span data-ttu-id="8ad58-1895">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1895">De</span></span> | <span data-ttu-id="8ad58-1896">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1896">Si</span></span> | <span data-ttu-id="8ad58-1897">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1897">Do</span></span> | <span data-ttu-id="8ad58-1898">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1898">Err</span></span> | <span data-ttu-id="8ad58-1899">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1899">Err</span></span> | <span data-ttu-id="8ad58-1900">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1900">Do</span></span>  | <span data-ttu-id="8ad58-1901">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1901">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1902">__米国__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1902">__US__</span></span> |    |    |    |    | <span data-ttu-id="8ad58-1903">US</span><span class="sxs-lookup"><span data-stu-id="8ad58-1903">US</span></span> | <span data-ttu-id="8ad58-1904">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1904">In</span></span> | <span data-ttu-id="8ad58-1905">UI</span><span class="sxs-lookup"><span data-stu-id="8ad58-1905">UI</span></span> | <span data-ttu-id="8ad58-1906">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1906">Lo</span></span> | <span data-ttu-id="8ad58-1907">UL</span><span class="sxs-lookup"><span data-stu-id="8ad58-1907">UL</span></span> | <span data-ttu-id="8ad58-1908">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1908">De</span></span> | <span data-ttu-id="8ad58-1909">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1909">Si</span></span> | <span data-ttu-id="8ad58-1910">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1910">Do</span></span> | <span data-ttu-id="8ad58-1911">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1911">Err</span></span> | <span data-ttu-id="8ad58-1912">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1912">Err</span></span> | <span data-ttu-id="8ad58-1913">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1913">Do</span></span>  | <span data-ttu-id="8ad58-1914">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1914">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1915">__In__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1915">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="8ad58-1916">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-1916">In</span></span> | <span data-ttu-id="8ad58-1917">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1917">Lo</span></span> | <span data-ttu-id="8ad58-1918">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1918">Lo</span></span> | <span data-ttu-id="8ad58-1919">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1919">De</span></span> | <span data-ttu-id="8ad58-1920">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1920">De</span></span> | <span data-ttu-id="8ad58-1921">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1921">Si</span></span> | <span data-ttu-id="8ad58-1922">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1922">Do</span></span> | <span data-ttu-id="8ad58-1923">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1923">Err</span></span> | <span data-ttu-id="8ad58-1924">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1924">Err</span></span> | <span data-ttu-id="8ad58-1925">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1925">Do</span></span>  | <span data-ttu-id="8ad58-1926">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1926">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1927">__UI__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1927">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="8ad58-1928">UI</span><span class="sxs-lookup"><span data-stu-id="8ad58-1928">UI</span></span> | <span data-ttu-id="8ad58-1929">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1929">Lo</span></span> | <span data-ttu-id="8ad58-1930">UL</span><span class="sxs-lookup"><span data-stu-id="8ad58-1930">UL</span></span> | <span data-ttu-id="8ad58-1931">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1931">De</span></span> | <span data-ttu-id="8ad58-1932">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1932">Si</span></span> | <span data-ttu-id="8ad58-1933">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1933">Do</span></span> | <span data-ttu-id="8ad58-1934">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1934">Err</span></span> | <span data-ttu-id="8ad58-1935">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1935">Err</span></span> | <span data-ttu-id="8ad58-1936">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1936">Do</span></span>  | <span data-ttu-id="8ad58-1937">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1937">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1938">__Lo__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1938">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1939">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-1939">Lo</span></span> | <span data-ttu-id="8ad58-1940">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1940">De</span></span> | <span data-ttu-id="8ad58-1941">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1941">De</span></span> | <span data-ttu-id="8ad58-1942">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1942">Si</span></span> | <span data-ttu-id="8ad58-1943">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1943">Do</span></span> | <span data-ttu-id="8ad58-1944">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1944">Err</span></span> | <span data-ttu-id="8ad58-1945">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1945">Err</span></span> | <span data-ttu-id="8ad58-1946">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1946">Do</span></span>  | <span data-ttu-id="8ad58-1947">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1947">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1948">__UL__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1948">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1949">UL</span><span class="sxs-lookup"><span data-stu-id="8ad58-1949">UL</span></span> | <span data-ttu-id="8ad58-1950">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1950">De</span></span> | <span data-ttu-id="8ad58-1951">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1951">Si</span></span> | <span data-ttu-id="8ad58-1952">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1952">Do</span></span> | <span data-ttu-id="8ad58-1953">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1953">Err</span></span> | <span data-ttu-id="8ad58-1954">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1954">Err</span></span> | <span data-ttu-id="8ad58-1955">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1955">Do</span></span>  | <span data-ttu-id="8ad58-1956">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1956">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1957">__解放__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1957">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1958">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-1958">De</span></span> | <span data-ttu-id="8ad58-1959">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1959">Si</span></span> | <span data-ttu-id="8ad58-1960">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1960">Do</span></span> | <span data-ttu-id="8ad58-1961">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1961">Err</span></span> | <span data-ttu-id="8ad58-1962">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1962">Err</span></span> | <span data-ttu-id="8ad58-1963">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1963">Do</span></span>  | <span data-ttu-id="8ad58-1964">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1964">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1965">__Si__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1965">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1966">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-1966">Si</span></span> | <span data-ttu-id="8ad58-1967">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1967">Do</span></span> | <span data-ttu-id="8ad58-1968">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1968">Err</span></span> | <span data-ttu-id="8ad58-1969">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1969">Err</span></span> | <span data-ttu-id="8ad58-1970">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1970">Do</span></span>  | <span data-ttu-id="8ad58-1971">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1971">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1972">__Do__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1972">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1973">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1973">Do</span></span> | <span data-ttu-id="8ad58-1974">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1974">Err</span></span> | <span data-ttu-id="8ad58-1975">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1975">Err</span></span> | <span data-ttu-id="8ad58-1976">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1976">Do</span></span>  | <span data-ttu-id="8ad58-1977">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1977">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1978">__オーディオ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1978">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-1979">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1979">Err</span></span> | <span data-ttu-id="8ad58-1980">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1980">Err</span></span> | <span data-ttu-id="8ad58-1981">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1981">Err</span></span> | <span data-ttu-id="8ad58-1982">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1982">Err</span></span> | 
| <span data-ttu-id="8ad58-1983">__ハーフ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1983">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="8ad58-1984">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1984">Err</span></span> | <span data-ttu-id="8ad58-1985">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1985">Err</span></span> | <span data-ttu-id="8ad58-1986">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-1986">Err</span></span> | 
| <span data-ttu-id="8ad58-1987">__&__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1987">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="8ad58-1988">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-1988">Do</span></span>  | <span data-ttu-id="8ad58-1989">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1989">Ob</span></span>  | 
| <span data-ttu-id="8ad58-1990">__Ob__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1990">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="8ad58-1991">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-1991">Ob</span></span>  | 


### <a name="exponentiation-operator"></a><span data-ttu-id="8ad58-1992">指数演算子</span><span class="sxs-lookup"><span data-stu-id="8ad58-1992">Exponentiation Operator</span></span>

<span data-ttu-id="8ad58-1993">累乗演算子は、2番目のオペランドの累乗に累乗された最初のオペランドを計算します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1993">The exponentiation operator computes the first operand raised to the power of the second operand.</span></span>

```antlr
ExponentOperatorExpression
    : Expression '^' LineTerminator? Expression
    ;
```

<span data-ttu-id="8ad58-1994">指数演算子が型 `Double` に対して定義されています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1994">The exponentiation operator is defined for type `Double`.</span></span> <span data-ttu-id="8ad58-1995">この値は、IEEE 754 算術の規則に従って計算されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-1995">The value is computed according to the rules of IEEE 754 arithmetic.</span></span>

<span data-ttu-id="8ad58-1996">__操作の種類:__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1996">__Operation Type:__</span></span>

|        | <span data-ttu-id="8ad58-1997">__コンボ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1997">__Bo__</span></span> | <span data-ttu-id="8ad58-1998">__SB__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1998">__SB__</span></span> | <span data-ttu-id="8ad58-1999">__は__</span><span class="sxs-lookup"><span data-stu-id="8ad58-1999">__By__</span></span> | <span data-ttu-id="8ad58-2000">__悪夢__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2000">__Sh__</span></span> | <span data-ttu-id="8ad58-2001">__米国__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2001">__US__</span></span> | <span data-ttu-id="8ad58-2002">__In__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2002">__In__</span></span> | <span data-ttu-id="8ad58-2003">__UI__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2003">__UI__</span></span> | <span data-ttu-id="8ad58-2004">__Lo__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2004">__Lo__</span></span> | <span data-ttu-id="8ad58-2005">__UL__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2005">__UL__</span></span> | <span data-ttu-id="8ad58-2006">__解放__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2006">__De__</span></span> | <span data-ttu-id="8ad58-2007">__Si__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2007">__Si__</span></span> | <span data-ttu-id="8ad58-2008">__Do__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2008">__Do__</span></span> | <span data-ttu-id="8ad58-2009">__オーディオ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2009">__Da__</span></span>  | <span data-ttu-id="8ad58-2010">__ハーフ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2010">__Ch__</span></span>  | <span data-ttu-id="8ad58-2011">__&__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2011">__St__</span></span> | <span data-ttu-id="8ad58-2012">__Ob__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2012">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="8ad58-2013">__コンボ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2013">__Bo__</span></span> | <span data-ttu-id="8ad58-2014">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2014">Do</span></span> | <span data-ttu-id="8ad58-2015">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2015">Do</span></span> | <span data-ttu-id="8ad58-2016">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2016">Do</span></span> | <span data-ttu-id="8ad58-2017">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2017">Do</span></span> | <span data-ttu-id="8ad58-2018">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2018">Do</span></span> | <span data-ttu-id="8ad58-2019">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2019">Do</span></span> | <span data-ttu-id="8ad58-2020">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2020">Do</span></span> | <span data-ttu-id="8ad58-2021">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2021">Do</span></span> | <span data-ttu-id="8ad58-2022">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2022">Do</span></span> | <span data-ttu-id="8ad58-2023">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2023">Do</span></span> | <span data-ttu-id="8ad58-2024">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2024">Do</span></span> | <span data-ttu-id="8ad58-2025">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2025">Do</span></span> | <span data-ttu-id="8ad58-2026">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2026">Err</span></span> | <span data-ttu-id="8ad58-2027">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2027">Err</span></span> | <span data-ttu-id="8ad58-2028">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2028">Do</span></span>  | <span data-ttu-id="8ad58-2029">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2029">Ob</span></span>  | 
| <span data-ttu-id="8ad58-2030">__SB__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2030">__SB__</span></span> |    | <span data-ttu-id="8ad58-2031">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2031">Do</span></span> | <span data-ttu-id="8ad58-2032">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2032">Do</span></span> | <span data-ttu-id="8ad58-2033">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2033">Do</span></span> | <span data-ttu-id="8ad58-2034">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2034">Do</span></span> | <span data-ttu-id="8ad58-2035">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2035">Do</span></span> | <span data-ttu-id="8ad58-2036">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2036">Do</span></span> | <span data-ttu-id="8ad58-2037">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2037">Do</span></span> | <span data-ttu-id="8ad58-2038">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2038">Do</span></span> | <span data-ttu-id="8ad58-2039">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2039">Do</span></span> | <span data-ttu-id="8ad58-2040">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2040">Do</span></span> | <span data-ttu-id="8ad58-2041">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2041">Do</span></span> | <span data-ttu-id="8ad58-2042">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2042">Err</span></span> | <span data-ttu-id="8ad58-2043">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2043">Err</span></span> | <span data-ttu-id="8ad58-2044">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2044">Do</span></span>  | <span data-ttu-id="8ad58-2045">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2045">Ob</span></span>  | 
| <span data-ttu-id="8ad58-2046">__は__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2046">__By__</span></span> |    |    | <span data-ttu-id="8ad58-2047">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2047">Do</span></span> | <span data-ttu-id="8ad58-2048">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2048">Do</span></span> | <span data-ttu-id="8ad58-2049">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2049">Do</span></span> | <span data-ttu-id="8ad58-2050">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2050">Do</span></span> | <span data-ttu-id="8ad58-2051">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2051">Do</span></span> | <span data-ttu-id="8ad58-2052">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2052">Do</span></span> | <span data-ttu-id="8ad58-2053">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2053">Do</span></span> | <span data-ttu-id="8ad58-2054">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2054">Do</span></span> | <span data-ttu-id="8ad58-2055">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2055">Do</span></span> | <span data-ttu-id="8ad58-2056">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2056">Do</span></span> | <span data-ttu-id="8ad58-2057">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2057">Err</span></span> | <span data-ttu-id="8ad58-2058">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2058">Err</span></span> | <span data-ttu-id="8ad58-2059">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2059">Do</span></span>  | <span data-ttu-id="8ad58-2060">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2060">Ob</span></span>  | 
| <span data-ttu-id="8ad58-2061">__悪夢__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2061">__Sh__</span></span> |    |    |    | <span data-ttu-id="8ad58-2062">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2062">Do</span></span> | <span data-ttu-id="8ad58-2063">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2063">Do</span></span> | <span data-ttu-id="8ad58-2064">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2064">Do</span></span> | <span data-ttu-id="8ad58-2065">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2065">Do</span></span> | <span data-ttu-id="8ad58-2066">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2066">Do</span></span> | <span data-ttu-id="8ad58-2067">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2067">Do</span></span> | <span data-ttu-id="8ad58-2068">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2068">Do</span></span> | <span data-ttu-id="8ad58-2069">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2069">Do</span></span> | <span data-ttu-id="8ad58-2070">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2070">Do</span></span> | <span data-ttu-id="8ad58-2071">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2071">Err</span></span> | <span data-ttu-id="8ad58-2072">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2072">Err</span></span> | <span data-ttu-id="8ad58-2073">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2073">Do</span></span>  | <span data-ttu-id="8ad58-2074">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2074">Ob</span></span>  | 
| <span data-ttu-id="8ad58-2075">__米国__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2075">__US__</span></span> |    |    |    |    | <span data-ttu-id="8ad58-2076">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2076">Do</span></span> | <span data-ttu-id="8ad58-2077">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2077">Do</span></span> | <span data-ttu-id="8ad58-2078">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2078">Do</span></span> | <span data-ttu-id="8ad58-2079">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2079">Do</span></span> | <span data-ttu-id="8ad58-2080">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2080">Do</span></span> | <span data-ttu-id="8ad58-2081">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2081">Do</span></span> | <span data-ttu-id="8ad58-2082">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2082">Do</span></span> | <span data-ttu-id="8ad58-2083">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2083">Do</span></span> | <span data-ttu-id="8ad58-2084">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2084">Err</span></span> | <span data-ttu-id="8ad58-2085">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2085">Err</span></span> | <span data-ttu-id="8ad58-2086">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2086">Do</span></span>  | <span data-ttu-id="8ad58-2087">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2087">Ob</span></span>  | 
| <span data-ttu-id="8ad58-2088">__In__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2088">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="8ad58-2089">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2089">Do</span></span> | <span data-ttu-id="8ad58-2090">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2090">Do</span></span> | <span data-ttu-id="8ad58-2091">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2091">Do</span></span> | <span data-ttu-id="8ad58-2092">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2092">Do</span></span> | <span data-ttu-id="8ad58-2093">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2093">Do</span></span> | <span data-ttu-id="8ad58-2094">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2094">Do</span></span> | <span data-ttu-id="8ad58-2095">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2095">Do</span></span> | <span data-ttu-id="8ad58-2096">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2096">Err</span></span> | <span data-ttu-id="8ad58-2097">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2097">Err</span></span> | <span data-ttu-id="8ad58-2098">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2098">Do</span></span>  | <span data-ttu-id="8ad58-2099">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2099">Ob</span></span>  | 
| <span data-ttu-id="8ad58-2100">__UI__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2100">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="8ad58-2101">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2101">Do</span></span> | <span data-ttu-id="8ad58-2102">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2102">Do</span></span> | <span data-ttu-id="8ad58-2103">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2103">Do</span></span> | <span data-ttu-id="8ad58-2104">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2104">Do</span></span> | <span data-ttu-id="8ad58-2105">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2105">Do</span></span> | <span data-ttu-id="8ad58-2106">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2106">Do</span></span> | <span data-ttu-id="8ad58-2107">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2107">Err</span></span> | <span data-ttu-id="8ad58-2108">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2108">Err</span></span> | <span data-ttu-id="8ad58-2109">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2109">Do</span></span>  | <span data-ttu-id="8ad58-2110">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2110">Ob</span></span>  | 
| <span data-ttu-id="8ad58-2111">__Lo__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2111">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2112">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2112">Do</span></span> | <span data-ttu-id="8ad58-2113">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2113">Do</span></span> | <span data-ttu-id="8ad58-2114">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2114">Do</span></span> | <span data-ttu-id="8ad58-2115">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2115">Do</span></span> | <span data-ttu-id="8ad58-2116">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2116">Do</span></span> | <span data-ttu-id="8ad58-2117">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2117">Err</span></span> | <span data-ttu-id="8ad58-2118">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2118">Err</span></span> | <span data-ttu-id="8ad58-2119">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2119">Do</span></span>  | <span data-ttu-id="8ad58-2120">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2120">Ob</span></span>  | 
| <span data-ttu-id="8ad58-2121">__UL__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2121">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2122">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2122">Do</span></span> | <span data-ttu-id="8ad58-2123">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2123">Do</span></span> | <span data-ttu-id="8ad58-2124">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2124">Do</span></span> | <span data-ttu-id="8ad58-2125">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2125">Do</span></span> | <span data-ttu-id="8ad58-2126">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2126">Err</span></span> | <span data-ttu-id="8ad58-2127">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2127">Err</span></span> | <span data-ttu-id="8ad58-2128">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2128">Do</span></span>  | <span data-ttu-id="8ad58-2129">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2129">Ob</span></span>  | 
| <span data-ttu-id="8ad58-2130">__解放__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2130">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2131">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2131">Do</span></span> | <span data-ttu-id="8ad58-2132">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2132">Do</span></span> | <span data-ttu-id="8ad58-2133">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2133">Do</span></span> | <span data-ttu-id="8ad58-2134">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2134">Err</span></span> | <span data-ttu-id="8ad58-2135">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2135">Err</span></span> | <span data-ttu-id="8ad58-2136">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2136">Do</span></span>  | <span data-ttu-id="8ad58-2137">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2137">Ob</span></span>  | 
| <span data-ttu-id="8ad58-2138">__Si__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2138">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2139">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2139">Do</span></span> | <span data-ttu-id="8ad58-2140">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2140">Do</span></span> | <span data-ttu-id="8ad58-2141">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2141">Err</span></span> | <span data-ttu-id="8ad58-2142">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2142">Err</span></span> | <span data-ttu-id="8ad58-2143">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2143">Do</span></span>  | <span data-ttu-id="8ad58-2144">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2144">Ob</span></span>  | 
| <span data-ttu-id="8ad58-2145">__Do__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2145">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2146">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2146">Do</span></span> | <span data-ttu-id="8ad58-2147">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2147">Err</span></span> | <span data-ttu-id="8ad58-2148">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2148">Err</span></span> | <span data-ttu-id="8ad58-2149">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2149">Do</span></span>  | <span data-ttu-id="8ad58-2150">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2150">Ob</span></span>  | 
| <span data-ttu-id="8ad58-2151">__オーディオ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2151">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2152">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2152">Err</span></span> | <span data-ttu-id="8ad58-2153">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2153">Err</span></span> | <span data-ttu-id="8ad58-2154">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2154">Err</span></span> | <span data-ttu-id="8ad58-2155">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2155">Err</span></span> | 
| <span data-ttu-id="8ad58-2156">__ハーフ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2156">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="8ad58-2157">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2157">Err</span></span> | <span data-ttu-id="8ad58-2158">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2158">Err</span></span> | <span data-ttu-id="8ad58-2159">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2159">Err</span></span> | 
| <span data-ttu-id="8ad58-2160">__&__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2160">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="8ad58-2161">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2161">Do</span></span>  | <span data-ttu-id="8ad58-2162">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2162">Ob</span></span>  | 
| <span data-ttu-id="8ad58-2163">__Ob__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2163">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="8ad58-2164">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2164">Ob</span></span>  | 


## <a name="relational-operators"></a><span data-ttu-id="8ad58-2165">関係演算子</span><span class="sxs-lookup"><span data-stu-id="8ad58-2165">Relational Operators</span></span>

<span data-ttu-id="8ad58-2166">*関係演算子*は、値を互いに比較します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2166">The *relational operators* compare values to one other.</span></span> <span data-ttu-id="8ad58-2167">比較演算子は @no__t 0、`<>`、`<`、`>`、`<=`、および `>=` です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2167">The comparison operators are `=`, `<>`, `<`, `>`, `<=`, and `>=`.</span></span>

```antlr
RelationalOperatorExpression
    : Expression '=' LineTerminator? Expression
    | Expression '<' '>' LineTerminator? Expression
    | Expression '<' LineTerminator? Expression
    | Expression '>' LineTerminator? Expression
    | Expression '<' '=' LineTerminator? Expression
    | Expression '>' '=' LineTerminator? Expression
    ;
```

<span data-ttu-id="8ad58-2168">すべての関係演算子は、@no__t 0 の値になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2168">All of the relational operators result in a `Boolean` value.</span></span>

<span data-ttu-id="8ad58-2169">関係演算子には、次の一般的な意味があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2169">The relational operators have the following general meaning:</span></span>

* <span data-ttu-id="8ad58-2170">@No__t-0 演算子は、2つのオペランドが等しいかどうかをテストします。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2170">The `=` operator tests whether the two operands are equal.</span></span>

* <span data-ttu-id="8ad58-2171">@No__t-0 演算子は、2つのオペランドが等しくないかどうかをテストします。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2171">The `<>` operator tests whether the two operands are not equal.</span></span>

* <span data-ttu-id="8ad58-2172">@No__t-0 演算子は、最初のオペランドが2番目のオペランドより小さいかどうかをテストします。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2172">The `<` operator tests whether the first operand is less than the second operand.</span></span>

* <span data-ttu-id="8ad58-2173">@No__t-0 演算子は、最初のオペランドが2番目のオペランドより大きいかどうかをテストします。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2173">The `>` operator tests whether the first operand is greater than the second operand.</span></span>

* <span data-ttu-id="8ad58-2174">@No__t-0 演算子は、最初のオペランドが2番目のオペランド以下かどうかをテストします。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2174">The `<=` operator tests whether the first operand is less than or equal to the second operand.</span></span>

* <span data-ttu-id="8ad58-2175">@No__t-0 演算子は、最初のオペランドが2番目のオペランド以上であるかどうかをテストします。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2175">The `>=` operator tests whether the first operand is greater than or equal to the second operand.</span></span>

<span data-ttu-id="8ad58-2176">関係演算子は、次の型に対して定義されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2176">The relational operators are defined for the following types:</span></span>

* <span data-ttu-id="8ad58-2177">`Boolean`。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2177">`Boolean`.</span></span> <span data-ttu-id="8ad58-2178">演算子は、2つのオペランドの真の値を比較します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2178">The operators compare the truth values of the two operands.</span></span> <span data-ttu-id="8ad58-2179">`True` は、数値と一致する `False` より小さいと見なされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2179">`True` is considered to be less than `False`, which matches with their numeric values.</span></span>

* <span data-ttu-id="8ad58-2180">`Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、および `Long`。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2180">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="8ad58-2181">演算子は、2つの整数オペランドの数値を比較します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2181">The operators compare the numeric values of the two integral operands.</span></span>

* <span data-ttu-id="8ad58-2182">`Single` および `Double`。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2182">`Single` and `Double`.</span></span> <span data-ttu-id="8ad58-2183">演算子は、IEEE 754 標準の規則に従って、オペランドを比較します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2183">The operators compare the operands according to the rules of the IEEE 754 standard.</span></span>

* <span data-ttu-id="8ad58-2184">`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2184">`Decimal`.</span></span> <span data-ttu-id="8ad58-2185">演算子は、2つの10進オペランドの数値を比較します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2185">The operators compare the numeric values of the two decimal operands.</span></span>

* <span data-ttu-id="8ad58-2186">`Date`。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2186">`Date`.</span></span> <span data-ttu-id="8ad58-2187">これらの演算子は、2つの日付/時刻値を比較した結果を返します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2187">The operators return the result of comparing the two date/time values.</span></span>

* <span data-ttu-id="8ad58-2188">`Char`。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2188">`Char`.</span></span> <span data-ttu-id="8ad58-2189">これらの演算子は、2つの Unicode 値を比較した結果を返します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2189">The operators return the result of comparing the two Unicode values.</span></span>

* <span data-ttu-id="8ad58-2190">`String`。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2190">`String`.</span></span> <span data-ttu-id="8ad58-2191">これらの演算子は、2つの値を比較した結果を、バイナリ比較とテキスト比較のどちらかを使用して返します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2191">The operators return the result of comparing the two values using either a binary comparison or a text comparison.</span></span> <span data-ttu-id="8ad58-2192">使用される比較は、コンパイル環境と `Option Compare` ステートメントによって決まります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2192">The comparison used is determined by the compilation environment and the `Option Compare` statement.</span></span> <span data-ttu-id="8ad58-2193">バイナリ比較では、各文字列の各文字の Unicode 値の数値が同じであるかどうかが判断されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2193">A binary comparison determines whether the numeric Unicode value of each character in each string is the same.</span></span> <span data-ttu-id="8ad58-2194">テキスト比較では、.NET Framework で使用されている現在のカルチャに基づいて、Unicode テキストの比較が行われます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2194">A text comparison does a Unicode text comparison based on the current culture in use on the .NET Framework.</span></span> <span data-ttu-id="8ad58-2195">文字列比較を実行する場合、null 値は、文字列リテラル `""` に相当します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2195">When doing a string comparison, a null value is equivalent to the string literal `""`.</span></span>

<span data-ttu-id="8ad58-2196">__操作の種類:__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2196">__Operation Type:__</span></span>
        
|        | <span data-ttu-id="8ad58-2197">__コンボ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2197">__Bo__</span></span> | <span data-ttu-id="8ad58-2198">__SB__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2198">__SB__</span></span> | <span data-ttu-id="8ad58-2199">__は__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2199">__By__</span></span> | <span data-ttu-id="8ad58-2200">__悪夢__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2200">__Sh__</span></span> | <span data-ttu-id="8ad58-2201">__米国__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2201">__US__</span></span> | <span data-ttu-id="8ad58-2202">__In__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2202">__In__</span></span> | <span data-ttu-id="8ad58-2203">__UI__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2203">__UI__</span></span> | <span data-ttu-id="8ad58-2204">__Lo__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2204">__Lo__</span></span> | <span data-ttu-id="8ad58-2205">__UL__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2205">__UL__</span></span> | <span data-ttu-id="8ad58-2206">__解放__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2206">__De__</span></span> | <span data-ttu-id="8ad58-2207">__Si__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2207">__Si__</span></span> | <span data-ttu-id="8ad58-2208">__Do__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2208">__Do__</span></span> | <span data-ttu-id="8ad58-2209">__オーディオ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2209">__Da__</span></span>  | <span data-ttu-id="8ad58-2210">__ハーフ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2210">__Ch__</span></span>  | <span data-ttu-id="8ad58-2211">__&__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2211">__St__</span></span> | <span data-ttu-id="8ad58-2212">__Ob__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2212">__Ob__</span></span> | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="8ad58-2213">__コンボ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2213">__Bo__</span></span> | <span data-ttu-id="8ad58-2214">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-2214">Bo</span></span> | <span data-ttu-id="8ad58-2215">SB</span><span class="sxs-lookup"><span data-stu-id="8ad58-2215">SB</span></span> | <span data-ttu-id="8ad58-2216">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-2216">Sh</span></span> | <span data-ttu-id="8ad58-2217">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-2217">Sh</span></span> | <span data-ttu-id="8ad58-2218">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-2218">In</span></span> | <span data-ttu-id="8ad58-2219">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-2219">In</span></span> | <span data-ttu-id="8ad58-2220">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2220">Lo</span></span> | <span data-ttu-id="8ad58-2221">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2221">Lo</span></span> | <span data-ttu-id="8ad58-2222">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-2222">De</span></span> | <span data-ttu-id="8ad58-2223">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-2223">De</span></span> | <span data-ttu-id="8ad58-2224">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-2224">Si</span></span> | <span data-ttu-id="8ad58-2225">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2225">Do</span></span> | <span data-ttu-id="8ad58-2226">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2226">Err</span></span> | <span data-ttu-id="8ad58-2227">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2227">Err</span></span> | <span data-ttu-id="8ad58-2228">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-2228">Bo</span></span> | <span data-ttu-id="8ad58-2229">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2229">Ob</span></span> | 
| <span data-ttu-id="8ad58-2230">__SB__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2230">__SB__</span></span> |    | <span data-ttu-id="8ad58-2231">SB</span><span class="sxs-lookup"><span data-stu-id="8ad58-2231">SB</span></span> | <span data-ttu-id="8ad58-2232">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-2232">Sh</span></span> | <span data-ttu-id="8ad58-2233">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-2233">Sh</span></span> | <span data-ttu-id="8ad58-2234">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-2234">In</span></span> | <span data-ttu-id="8ad58-2235">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-2235">In</span></span> | <span data-ttu-id="8ad58-2236">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2236">Lo</span></span> | <span data-ttu-id="8ad58-2237">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2237">Lo</span></span> | <span data-ttu-id="8ad58-2238">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-2238">De</span></span> | <span data-ttu-id="8ad58-2239">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-2239">De</span></span> | <span data-ttu-id="8ad58-2240">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-2240">Si</span></span> | <span data-ttu-id="8ad58-2241">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2241">Do</span></span> | <span data-ttu-id="8ad58-2242">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2242">Err</span></span> | <span data-ttu-id="8ad58-2243">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2243">Err</span></span> | <span data-ttu-id="8ad58-2244">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2244">Do</span></span> | <span data-ttu-id="8ad58-2245">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2245">Ob</span></span> | 
| <span data-ttu-id="8ad58-2246">__は__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2246">__By__</span></span> |    |    | <span data-ttu-id="8ad58-2247">By</span><span class="sxs-lookup"><span data-stu-id="8ad58-2247">By</span></span> | <span data-ttu-id="8ad58-2248">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-2248">Sh</span></span> | <span data-ttu-id="8ad58-2249">US</span><span class="sxs-lookup"><span data-stu-id="8ad58-2249">US</span></span> | <span data-ttu-id="8ad58-2250">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-2250">In</span></span> | <span data-ttu-id="8ad58-2251">UI</span><span class="sxs-lookup"><span data-stu-id="8ad58-2251">UI</span></span> | <span data-ttu-id="8ad58-2252">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2252">Lo</span></span> | <span data-ttu-id="8ad58-2253">UL</span><span class="sxs-lookup"><span data-stu-id="8ad58-2253">UL</span></span> | <span data-ttu-id="8ad58-2254">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-2254">De</span></span> | <span data-ttu-id="8ad58-2255">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-2255">Si</span></span> | <span data-ttu-id="8ad58-2256">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2256">Do</span></span> | <span data-ttu-id="8ad58-2257">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2257">Err</span></span> | <span data-ttu-id="8ad58-2258">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2258">Err</span></span> | <span data-ttu-id="8ad58-2259">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2259">Do</span></span> | <span data-ttu-id="8ad58-2260">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2260">Ob</span></span> | 
| <span data-ttu-id="8ad58-2261">__悪夢__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2261">__Sh__</span></span> |    |    |    | <span data-ttu-id="8ad58-2262">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-2262">Sh</span></span> | <span data-ttu-id="8ad58-2263">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-2263">In</span></span> | <span data-ttu-id="8ad58-2264">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-2264">In</span></span> | <span data-ttu-id="8ad58-2265">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2265">Lo</span></span> | <span data-ttu-id="8ad58-2266">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2266">Lo</span></span> | <span data-ttu-id="8ad58-2267">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-2267">De</span></span> | <span data-ttu-id="8ad58-2268">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-2268">De</span></span> | <span data-ttu-id="8ad58-2269">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-2269">Si</span></span> | <span data-ttu-id="8ad58-2270">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2270">Do</span></span> | <span data-ttu-id="8ad58-2271">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2271">Err</span></span> | <span data-ttu-id="8ad58-2272">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2272">Err</span></span> | <span data-ttu-id="8ad58-2273">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2273">Do</span></span> | <span data-ttu-id="8ad58-2274">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2274">Ob</span></span> | 
| <span data-ttu-id="8ad58-2275">__米国__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2275">__US__</span></span> |    |    |    |    | <span data-ttu-id="8ad58-2276">US</span><span class="sxs-lookup"><span data-stu-id="8ad58-2276">US</span></span> | <span data-ttu-id="8ad58-2277">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-2277">In</span></span> | <span data-ttu-id="8ad58-2278">UI</span><span class="sxs-lookup"><span data-stu-id="8ad58-2278">UI</span></span> | <span data-ttu-id="8ad58-2279">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2279">Lo</span></span> | <span data-ttu-id="8ad58-2280">UL</span><span class="sxs-lookup"><span data-stu-id="8ad58-2280">UL</span></span> | <span data-ttu-id="8ad58-2281">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-2281">De</span></span> | <span data-ttu-id="8ad58-2282">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-2282">Si</span></span> | <span data-ttu-id="8ad58-2283">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2283">Do</span></span> | <span data-ttu-id="8ad58-2284">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2284">Err</span></span> | <span data-ttu-id="8ad58-2285">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2285">Err</span></span> | <span data-ttu-id="8ad58-2286">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2286">Do</span></span> | <span data-ttu-id="8ad58-2287">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2287">Ob</span></span> | 
| <span data-ttu-id="8ad58-2288">__In__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2288">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="8ad58-2289">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-2289">In</span></span> | <span data-ttu-id="8ad58-2290">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2290">Lo</span></span> | <span data-ttu-id="8ad58-2291">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2291">Lo</span></span> | <span data-ttu-id="8ad58-2292">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-2292">De</span></span> | <span data-ttu-id="8ad58-2293">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-2293">De</span></span> | <span data-ttu-id="8ad58-2294">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-2294">Si</span></span> | <span data-ttu-id="8ad58-2295">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2295">Do</span></span> | <span data-ttu-id="8ad58-2296">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2296">Err</span></span> | <span data-ttu-id="8ad58-2297">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2297">Err</span></span> | <span data-ttu-id="8ad58-2298">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2298">Do</span></span> | <span data-ttu-id="8ad58-2299">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2299">Ob</span></span> | 
| <span data-ttu-id="8ad58-2300">__UI__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2300">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="8ad58-2301">UI</span><span class="sxs-lookup"><span data-stu-id="8ad58-2301">UI</span></span> | <span data-ttu-id="8ad58-2302">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2302">Lo</span></span> | <span data-ttu-id="8ad58-2303">UL</span><span class="sxs-lookup"><span data-stu-id="8ad58-2303">UL</span></span> | <span data-ttu-id="8ad58-2304">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-2304">De</span></span> | <span data-ttu-id="8ad58-2305">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-2305">Si</span></span> | <span data-ttu-id="8ad58-2306">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2306">Do</span></span> | <span data-ttu-id="8ad58-2307">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2307">Err</span></span> | <span data-ttu-id="8ad58-2308">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2308">Err</span></span> | <span data-ttu-id="8ad58-2309">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2309">Do</span></span> | <span data-ttu-id="8ad58-2310">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2310">Ob</span></span> | 
| <span data-ttu-id="8ad58-2311">__Lo__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2311">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2312">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2312">Lo</span></span> | <span data-ttu-id="8ad58-2313">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-2313">De</span></span> | <span data-ttu-id="8ad58-2314">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-2314">De</span></span> | <span data-ttu-id="8ad58-2315">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-2315">Si</span></span> | <span data-ttu-id="8ad58-2316">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2316">Do</span></span> | <span data-ttu-id="8ad58-2317">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2317">Err</span></span> | <span data-ttu-id="8ad58-2318">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2318">Err</span></span> | <span data-ttu-id="8ad58-2319">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2319">Do</span></span> | <span data-ttu-id="8ad58-2320">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2320">Ob</span></span> | 
| <span data-ttu-id="8ad58-2321">__UL__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2321">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2322">UL</span><span class="sxs-lookup"><span data-stu-id="8ad58-2322">UL</span></span> | <span data-ttu-id="8ad58-2323">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-2323">De</span></span> | <span data-ttu-id="8ad58-2324">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-2324">Si</span></span> | <span data-ttu-id="8ad58-2325">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2325">Do</span></span> | <span data-ttu-id="8ad58-2326">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2326">Err</span></span> | <span data-ttu-id="8ad58-2327">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2327">Err</span></span> | <span data-ttu-id="8ad58-2328">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2328">Do</span></span> | <span data-ttu-id="8ad58-2329">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2329">Ob</span></span> | 
| <span data-ttu-id="8ad58-2330">__解放__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2330">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2331">De</span><span class="sxs-lookup"><span data-stu-id="8ad58-2331">De</span></span> | <span data-ttu-id="8ad58-2332">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-2332">Si</span></span> | <span data-ttu-id="8ad58-2333">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2333">Do</span></span> | <span data-ttu-id="8ad58-2334">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2334">Err</span></span> | <span data-ttu-id="8ad58-2335">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2335">Err</span></span> | <span data-ttu-id="8ad58-2336">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2336">Do</span></span> | <span data-ttu-id="8ad58-2337">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2337">Ob</span></span> | 
| <span data-ttu-id="8ad58-2338">__Si__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2338">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2339">Si</span><span class="sxs-lookup"><span data-stu-id="8ad58-2339">Si</span></span> | <span data-ttu-id="8ad58-2340">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2340">Do</span></span> | <span data-ttu-id="8ad58-2341">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2341">Err</span></span> | <span data-ttu-id="8ad58-2342">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2342">Err</span></span> | <span data-ttu-id="8ad58-2343">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2343">Do</span></span> | <span data-ttu-id="8ad58-2344">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2344">Ob</span></span> | 
| <span data-ttu-id="8ad58-2345">__Do__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2345">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2346">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2346">Do</span></span> | <span data-ttu-id="8ad58-2347">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2347">Err</span></span> | <span data-ttu-id="8ad58-2348">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2348">Err</span></span> | <span data-ttu-id="8ad58-2349">Do</span><span class="sxs-lookup"><span data-stu-id="8ad58-2349">Do</span></span> | <span data-ttu-id="8ad58-2350">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2350">Ob</span></span> | 
| <span data-ttu-id="8ad58-2351">__オーディオ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2351">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2352">オーディオ</span><span class="sxs-lookup"><span data-stu-id="8ad58-2352">Da</span></span>  | <span data-ttu-id="8ad58-2353">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2353">Err</span></span> | <span data-ttu-id="8ad58-2354">オーディオ</span><span class="sxs-lookup"><span data-stu-id="8ad58-2354">Da</span></span> | <span data-ttu-id="8ad58-2355">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2355">Ob</span></span> | 
| <span data-ttu-id="8ad58-2356">__ハーフ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2356">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="8ad58-2357">ハーフ</span><span class="sxs-lookup"><span data-stu-id="8ad58-2357">Ch</span></span>  | <span data-ttu-id="8ad58-2358">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2358">St</span></span> | <span data-ttu-id="8ad58-2359">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2359">Ob</span></span> | 
| <span data-ttu-id="8ad58-2360">__&__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2360">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="8ad58-2361">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2361">St</span></span> | <span data-ttu-id="8ad58-2362">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2362">Ob</span></span> | 
| <span data-ttu-id="8ad58-2363">__Ob__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2363">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |    | <span data-ttu-id="8ad58-2364">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2364">Ob</span></span> | 


## <a name="like-operator"></a><span data-ttu-id="8ad58-2365">Like 演算子</span><span class="sxs-lookup"><span data-stu-id="8ad58-2365">Like Operator</span></span>

<span data-ttu-id="8ad58-2366">@No__t-0 演算子は、文字列が特定のパターンに一致するかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2366">The `Like` operator determines whether a string matches a given pattern.</span></span>

```antlr
LikeOperatorExpression
    : Expression 'Like' LineTerminator? Expression
    ;
```

<span data-ttu-id="8ad58-2367">@No__t-0 演算子が `String` 型に対して定義されています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2367">The `Like` operator is defined for the `String` type.</span></span> <span data-ttu-id="8ad58-2368">最初のオペランドは照合される文字列で、2番目のオペランドは照合するパターンです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2368">The first operand is the string being matched, and the second operand is the pattern to match against.</span></span> <span data-ttu-id="8ad58-2369">このパターンは、Unicode 文字で構成されています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2369">The pattern is made up of Unicode characters.</span></span> <span data-ttu-id="8ad58-2370">次の文字シーケンスには特別な意味があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2370">The following character sequences have special meanings:</span></span>

* <span data-ttu-id="8ad58-2371">文字 `?` は任意の1文字と一致します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2371">The character `?` matches any single character.</span></span>

* <span data-ttu-id="8ad58-2372">文字 `*` は、0個以上の文字と一致します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2372">The character `*` matches zero or more characters.</span></span>

* <span data-ttu-id="8ad58-2373">文字 `#` は任意の1桁 (0-9) に一致します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2373">The character `#` matches any single digit (0-9).</span></span>

* <span data-ttu-id="8ad58-2374">角かっこ (`[ab...]`) で囲まれた文字の一覧は、リスト内の任意の1文字と一致します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2374">A list of characters surrounded by brackets (`[ab...]`) matches any single character in the list.</span></span>

* <span data-ttu-id="8ad58-2375">角かっこで囲まれ、感嘆符 (@no__t 0) で始まる文字のリストは、文字リストに含まれていない任意の1文字と一致します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2375">A list of characters surrounded by brackets and prefixed by an exclamation point (`[!ab...]`) matches any single character not in the character list.</span></span>

* <span data-ttu-id="8ad58-2376">ハイフン (`-`) で区切られた文字リスト内の2つの文字は、最初の文字で始まり、2番目の文字で終わる Unicode 文字の範囲を指定します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2376">Two characters in a character list separated by a hyphen (`-`) specify a range of Unicode characters starting with the first character and ending with the second character.</span></span> <span data-ttu-id="8ad58-2377">2番目の文字が、最初の文字より後の並べ替え順序ではない場合、実行時例外が発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2377">If the second character is not later in the sort order than the first character, a run-time exception occurs.</span></span> <span data-ttu-id="8ad58-2378">文字リストの先頭または末尾にあるハイフンは、それ自体を指定します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2378">A hyphen that appears at the beginning or end of a character list specifies itself.</span></span>

<span data-ttu-id="8ad58-2379">特殊文字の左角かっこ (`[`)、疑問符 (`?`)、番号記号 (`#`)、アスタリスク (`*`) を一致させるには、角かっこで囲む必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2379">To match the special characters left bracket (`[`), question mark (`?`), number sign (`#`), and asterisk (`*`), brackets must enclose them.</span></span> <span data-ttu-id="8ad58-2380">右角かっこ (`]`) は、それ自体に一致するグループ内では使用できませんが、グループの外側で個別の文字として使用することはできます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2380">The right bracket (`]`) cannot be used within a group to match itself, but it can be used outside a group as an individual character.</span></span> <span data-ttu-id="8ad58-2381">@No__t-0 という文字シーケンスは、文字列リテラル `""` と見なされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2381">The character sequence `[]` is considered to be the string literal `""`.</span></span> 

<span data-ttu-id="8ad58-2382">文字の比較と文字リストの順序付けは、使用される比較の種類によって異なります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2382">Note that character comparisons and ordering for character lists are dependent on the type of comparisons being used.</span></span> <span data-ttu-id="8ad58-2383">バイナリ比較が使用されている場合、文字の比較と順序付けは、数値の Unicode 値に基づいて行われます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2383">If binary comparisons are being used, character comparisons and ordering are based on the numeric Unicode values.</span></span> <span data-ttu-id="8ad58-2384">テキスト比較が使用されている場合、文字の比較と順序付けは、.NET Framework で現在使用されているロケールに基づいて行われます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2384">If text comparisons are being used, character comparisons and ordering are based on the current locale being used on the .NET Framework.</span></span>

<span data-ttu-id="8ad58-2385">言語によっては、アルファベットの特殊文字は2つの異なる文字を表し、その逆も同様です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2385">In some languages, special characters in the alphabet represent two separate characters and vice versa.</span></span> <span data-ttu-id="8ad58-2386">たとえば、複数の言語では、文字 `æ` を使用して文字 `a` と `e` が一緒に表示されます。一方、文字 `^` と @no__t は、文字 @no__t を表すために使用できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2386">For example, several languages use the character `æ` to represent the characters `a` and `e` when they appear together, while the characters `^` and `O` can be used to represent the character `Ô`.</span></span> <span data-ttu-id="8ad58-2387">テキスト比較を使用する場合、@no__t 0 演算子は、このようなカルチャのとっを認識します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2387">When using text comparisons, the `Like` operator recognizes such cultural equivalences.</span></span> <span data-ttu-id="8ad58-2388">その場合、pattern または string 内の1つの特殊文字が、他の文字列の等価の2文字シーケンスと一致します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2388">In that case, an occurrence of the single special character in either pattern or string matches the equivalent two-character sequence in the other string.</span></span> <span data-ttu-id="8ad58-2389">同様に、角かっこで囲まれたパターン内の1つの特殊文字 (単独、リスト、または範囲内) は、文字列内の等価の2文字シーケンスと一致します。また、その逆も同様です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2389">Similarly, a single special character in pattern enclosed in brackets (by itself, in a list, or in a range) matches the equivalent two-character sequence in the string and vice versa.</span></span>

<span data-ttu-id="8ad58-2390">両方のオペランドが `Nothing` または1つのオペランドが `String` への組み込みの変換であり、もう一方のオペランド @no__t が-3 である @no__t 0 の式では、@no__t は空の文字列リテラルとして処理されます `""`。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2390">In a `Like` expression where both operands are `Nothing` or one operand has an intrinsic conversion to `String` and the other operand is `Nothing`, `Nothing` is treated as if it were the empty string literal `""`.</span></span>

<span data-ttu-id="8ad58-2391">__操作の種類:__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2391">__Operation Type:__</span></span>

|        | <span data-ttu-id="8ad58-2392">__コンボ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2392">__Bo__</span></span> | <span data-ttu-id="8ad58-2393">__SB__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2393">__SB__</span></span> | <span data-ttu-id="8ad58-2394">__は__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2394">__By__</span></span> | <span data-ttu-id="8ad58-2395">__悪夢__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2395">__Sh__</span></span> | <span data-ttu-id="8ad58-2396">__米国__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2396">__US__</span></span> | <span data-ttu-id="8ad58-2397">__In__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2397">__In__</span></span> | <span data-ttu-id="8ad58-2398">__UI__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2398">__UI__</span></span> | <span data-ttu-id="8ad58-2399">__Lo__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2399">__Lo__</span></span> | <span data-ttu-id="8ad58-2400">__UL__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2400">__UL__</span></span> | <span data-ttu-id="8ad58-2401">__解放__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2401">__De__</span></span> | <span data-ttu-id="8ad58-2402">__Si__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2402">__Si__</span></span> | <span data-ttu-id="8ad58-2403">__Do__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2403">__Do__</span></span> | <span data-ttu-id="8ad58-2404">__オーディオ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2404">__Da__</span></span>  | <span data-ttu-id="8ad58-2405">__ハーフ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2405">__Ch__</span></span>  | <span data-ttu-id="8ad58-2406">__&__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2406">__St__</span></span> | <span data-ttu-id="8ad58-2407">__Ob__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2407">__Ob__</span></span> | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| <span data-ttu-id="8ad58-2408">__コンボ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2408">__Bo__</span></span> | <span data-ttu-id="8ad58-2409">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2409">St</span></span> | <span data-ttu-id="8ad58-2410">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2410">St</span></span> | <span data-ttu-id="8ad58-2411">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2411">St</span></span> | <span data-ttu-id="8ad58-2412">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2412">St</span></span> | <span data-ttu-id="8ad58-2413">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2413">St</span></span> | <span data-ttu-id="8ad58-2414">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2414">St</span></span> | <span data-ttu-id="8ad58-2415">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2415">St</span></span> | <span data-ttu-id="8ad58-2416">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2416">St</span></span> | <span data-ttu-id="8ad58-2417">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2417">St</span></span> | <span data-ttu-id="8ad58-2418">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2418">St</span></span> | <span data-ttu-id="8ad58-2419">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2419">St</span></span> | <span data-ttu-id="8ad58-2420">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2420">St</span></span> | <span data-ttu-id="8ad58-2421">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2421">St</span></span> | <span data-ttu-id="8ad58-2422">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2422">St</span></span> | <span data-ttu-id="8ad58-2423">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2423">St</span></span> | <span data-ttu-id="8ad58-2424">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2424">Ob</span></span> | 
| <span data-ttu-id="8ad58-2425">__SB__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2425">__SB__</span></span> |    | <span data-ttu-id="8ad58-2426">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2426">St</span></span> | <span data-ttu-id="8ad58-2427">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2427">St</span></span> | <span data-ttu-id="8ad58-2428">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2428">St</span></span> | <span data-ttu-id="8ad58-2429">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2429">St</span></span> | <span data-ttu-id="8ad58-2430">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2430">St</span></span> | <span data-ttu-id="8ad58-2431">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2431">St</span></span> | <span data-ttu-id="8ad58-2432">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2432">St</span></span> | <span data-ttu-id="8ad58-2433">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2433">St</span></span> | <span data-ttu-id="8ad58-2434">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2434">St</span></span> | <span data-ttu-id="8ad58-2435">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2435">St</span></span> | <span data-ttu-id="8ad58-2436">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2436">St</span></span> | <span data-ttu-id="8ad58-2437">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2437">St</span></span> | <span data-ttu-id="8ad58-2438">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2438">St</span></span> | <span data-ttu-id="8ad58-2439">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2439">St</span></span> | <span data-ttu-id="8ad58-2440">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2440">Ob</span></span> | 
| <span data-ttu-id="8ad58-2441">__は__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2441">__By__</span></span> |    |    | <span data-ttu-id="8ad58-2442">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2442">St</span></span> | <span data-ttu-id="8ad58-2443">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2443">St</span></span> | <span data-ttu-id="8ad58-2444">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2444">St</span></span> | <span data-ttu-id="8ad58-2445">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2445">St</span></span> | <span data-ttu-id="8ad58-2446">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2446">St</span></span> | <span data-ttu-id="8ad58-2447">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2447">St</span></span> | <span data-ttu-id="8ad58-2448">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2448">St</span></span> | <span data-ttu-id="8ad58-2449">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2449">St</span></span> | <span data-ttu-id="8ad58-2450">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2450">St</span></span> | <span data-ttu-id="8ad58-2451">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2451">St</span></span> | <span data-ttu-id="8ad58-2452">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2452">St</span></span> | <span data-ttu-id="8ad58-2453">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2453">St</span></span> | <span data-ttu-id="8ad58-2454">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2454">St</span></span> | <span data-ttu-id="8ad58-2455">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2455">Ob</span></span> | 
| <span data-ttu-id="8ad58-2456">__悪夢__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2456">__Sh__</span></span> |    |    |    | <span data-ttu-id="8ad58-2457">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2457">St</span></span> | <span data-ttu-id="8ad58-2458">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2458">St</span></span> | <span data-ttu-id="8ad58-2459">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2459">St</span></span> | <span data-ttu-id="8ad58-2460">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2460">St</span></span> | <span data-ttu-id="8ad58-2461">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2461">St</span></span> | <span data-ttu-id="8ad58-2462">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2462">St</span></span> | <span data-ttu-id="8ad58-2463">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2463">St</span></span> | <span data-ttu-id="8ad58-2464">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2464">St</span></span> | <span data-ttu-id="8ad58-2465">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2465">St</span></span> | <span data-ttu-id="8ad58-2466">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2466">St</span></span> | <span data-ttu-id="8ad58-2467">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2467">St</span></span> | <span data-ttu-id="8ad58-2468">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2468">St</span></span> | <span data-ttu-id="8ad58-2469">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2469">Ob</span></span> | 
| <span data-ttu-id="8ad58-2470">__米国__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2470">__US__</span></span> |    |    |    |    | <span data-ttu-id="8ad58-2471">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2471">St</span></span> | <span data-ttu-id="8ad58-2472">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2472">St</span></span> | <span data-ttu-id="8ad58-2473">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2473">St</span></span> | <span data-ttu-id="8ad58-2474">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2474">St</span></span> | <span data-ttu-id="8ad58-2475">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2475">St</span></span> | <span data-ttu-id="8ad58-2476">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2476">St</span></span> | <span data-ttu-id="8ad58-2477">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2477">St</span></span> | <span data-ttu-id="8ad58-2478">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2478">St</span></span> | <span data-ttu-id="8ad58-2479">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2479">St</span></span> | <span data-ttu-id="8ad58-2480">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2480">St</span></span> | <span data-ttu-id="8ad58-2481">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2481">St</span></span> | <span data-ttu-id="8ad58-2482">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2482">Ob</span></span> | 
| <span data-ttu-id="8ad58-2483">__In__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2483">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="8ad58-2484">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2484">St</span></span> | <span data-ttu-id="8ad58-2485">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2485">St</span></span> | <span data-ttu-id="8ad58-2486">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2486">St</span></span> | <span data-ttu-id="8ad58-2487">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2487">St</span></span> | <span data-ttu-id="8ad58-2488">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2488">St</span></span> | <span data-ttu-id="8ad58-2489">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2489">St</span></span> | <span data-ttu-id="8ad58-2490">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2490">St</span></span> | <span data-ttu-id="8ad58-2491">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2491">St</span></span> | <span data-ttu-id="8ad58-2492">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2492">St</span></span> | <span data-ttu-id="8ad58-2493">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2493">St</span></span> | <span data-ttu-id="8ad58-2494">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2494">Ob</span></span> | 
| <span data-ttu-id="8ad58-2495">__UI__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2495">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="8ad58-2496">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2496">St</span></span> | <span data-ttu-id="8ad58-2497">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2497">St</span></span> | <span data-ttu-id="8ad58-2498">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2498">St</span></span> | <span data-ttu-id="8ad58-2499">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2499">St</span></span> | <span data-ttu-id="8ad58-2500">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2500">St</span></span> | <span data-ttu-id="8ad58-2501">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2501">St</span></span> | <span data-ttu-id="8ad58-2502">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2502">St</span></span> | <span data-ttu-id="8ad58-2503">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2503">St</span></span> | <span data-ttu-id="8ad58-2504">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2504">St</span></span> | <span data-ttu-id="8ad58-2505">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2505">Ob</span></span> | 
| <span data-ttu-id="8ad58-2506">__Lo__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2506">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2507">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2507">St</span></span> | <span data-ttu-id="8ad58-2508">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2508">St</span></span> | <span data-ttu-id="8ad58-2509">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2509">St</span></span> | <span data-ttu-id="8ad58-2510">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2510">St</span></span> | <span data-ttu-id="8ad58-2511">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2511">St</span></span> | <span data-ttu-id="8ad58-2512">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2512">St</span></span> | <span data-ttu-id="8ad58-2513">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2513">St</span></span> | <span data-ttu-id="8ad58-2514">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2514">St</span></span> | <span data-ttu-id="8ad58-2515">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2515">Ob</span></span> | 
| <span data-ttu-id="8ad58-2516">__UL__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2516">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2517">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2517">St</span></span> | <span data-ttu-id="8ad58-2518">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2518">St</span></span> | <span data-ttu-id="8ad58-2519">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2519">St</span></span> | <span data-ttu-id="8ad58-2520">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2520">St</span></span> | <span data-ttu-id="8ad58-2521">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2521">St</span></span> | <span data-ttu-id="8ad58-2522">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2522">St</span></span> | <span data-ttu-id="8ad58-2523">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2523">St</span></span> | <span data-ttu-id="8ad58-2524">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2524">Ob</span></span> | 
| <span data-ttu-id="8ad58-2525">__解放__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2525">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2526">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2526">St</span></span> | <span data-ttu-id="8ad58-2527">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2527">St</span></span> | <span data-ttu-id="8ad58-2528">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2528">St</span></span> | <span data-ttu-id="8ad58-2529">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2529">St</span></span> | <span data-ttu-id="8ad58-2530">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2530">St</span></span> | <span data-ttu-id="8ad58-2531">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2531">St</span></span> | <span data-ttu-id="8ad58-2532">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2532">Ob</span></span> | 
| <span data-ttu-id="8ad58-2533">__Si__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2533">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2534">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2534">St</span></span> | <span data-ttu-id="8ad58-2535">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2535">St</span></span> | <span data-ttu-id="8ad58-2536">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2536">St</span></span> | <span data-ttu-id="8ad58-2537">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2537">St</span></span> | <span data-ttu-id="8ad58-2538">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2538">St</span></span> | <span data-ttu-id="8ad58-2539">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2539">Ob</span></span> | 
| <span data-ttu-id="8ad58-2540">__Do__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2540">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2541">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2541">St</span></span> | <span data-ttu-id="8ad58-2542">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2542">St</span></span> | <span data-ttu-id="8ad58-2543">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2543">St</span></span> | <span data-ttu-id="8ad58-2544">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2544">St</span></span> | <span data-ttu-id="8ad58-2545">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2545">Ob</span></span> | 
| <span data-ttu-id="8ad58-2546">__オーディオ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2546">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2547">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2547">St</span></span> | <span data-ttu-id="8ad58-2548">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2548">St</span></span> | <span data-ttu-id="8ad58-2549">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2549">St</span></span> | <span data-ttu-id="8ad58-2550">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2550">Ob</span></span> | 
| <span data-ttu-id="8ad58-2551">__ハーフ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2551">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2552">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2552">St</span></span> | <span data-ttu-id="8ad58-2553">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2553">St</span></span> | <span data-ttu-id="8ad58-2554">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2554">Ob</span></span> | 
| <span data-ttu-id="8ad58-2555">__&__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2555">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2556">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2556">St</span></span> | <span data-ttu-id="8ad58-2557">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2557">Ob</span></span> | 
| <span data-ttu-id="8ad58-2558">__Ob__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2558">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2559">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2559">Ob</span></span> | 


## <a name="concatenation-operator"></a><span data-ttu-id="8ad58-2560">連結演算子</span><span class="sxs-lookup"><span data-stu-id="8ad58-2560">Concatenation Operator</span></span>

```antlr
ConcatenationOperatorExpression
    : Expression '&' LineTerminator? Expression
    ;
```

<span data-ttu-id="8ad58-2561">*連結演算子*は、組み込み値型の null 許容バージョンを含む、すべての組み込み型に対して定義されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2561">The *concatenation operator* is defined for all of the intrinsic types, including the nullable versions of the intrinsic value types.</span></span> <span data-ttu-id="8ad58-2562">また、前述の型と `System.DBNull` の連結にも定義されています。これは、@no__t 1 の文字列として扱われます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2562">It is also defined for concatenation between the types mentioned above and `System.DBNull`, which is treated as a `Nothing` string.</span></span> <span data-ttu-id="8ad58-2563">連結演算子は、そのすべてのオペランドを `String` に変換します。式では、厳密なセマンティクスが使用されているかどうかにかかわらず、`String` への変換はすべて、拡大されていると見なされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2563">The concatenation operator converts all of its operands to `String`; in the expression, all conversions to `String` are considered to be widening, regardless of whether strict semantics are used.</span></span> <span data-ttu-id="8ad58-2564">@No__t-0 の値は、`String` として型指定されたリテラル @no__t に変換されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2564">A `System.DBNull` value is converted to the literal `Nothing` typed as `String`.</span></span> <span data-ttu-id="8ad58-2565">値が @no__t 0 の null 許容値型は、実行時エラーをスローするのではなく、`String` として型指定されたリテラル @no__t にも変換されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2565">A nullable value type whose value is `Nothing` is also converted to the literal `Nothing` typed as `String`, rather than throwing a run-time error.</span></span>

<span data-ttu-id="8ad58-2566">連結演算では、2つのオペランドを左から右に連結した文字列が生成されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2566">A concatenation operation results in a string that is the concatenation of the two operands in order from left to right.</span></span> <span data-ttu-id="8ad58-2567">値 `Nothing` は、空の文字列リテラル `""` として扱われます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2567">The value `Nothing` is treated as if it were the empty string literal `""`.</span></span>

<span data-ttu-id="8ad58-2568">__操作の種類:__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2568">__Operation Type:__</span></span>

|        | <span data-ttu-id="8ad58-2569">__コンボ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2569">__Bo__</span></span> | <span data-ttu-id="8ad58-2570">__SB__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2570">__SB__</span></span> | <span data-ttu-id="8ad58-2571">__は__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2571">__By__</span></span> | <span data-ttu-id="8ad58-2572">__悪夢__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2572">__Sh__</span></span> | <span data-ttu-id="8ad58-2573">__米国__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2573">__US__</span></span> | <span data-ttu-id="8ad58-2574">__In__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2574">__In__</span></span> | <span data-ttu-id="8ad58-2575">__UI__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2575">__UI__</span></span> | <span data-ttu-id="8ad58-2576">__Lo__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2576">__Lo__</span></span> | <span data-ttu-id="8ad58-2577">__UL__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2577">__UL__</span></span> | <span data-ttu-id="8ad58-2578">__解放__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2578">__De__</span></span> | <span data-ttu-id="8ad58-2579">__Si__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2579">__Si__</span></span> | <span data-ttu-id="8ad58-2580">__Do__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2580">__Do__</span></span> | <span data-ttu-id="8ad58-2581">__オーディオ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2581">__Da__</span></span>  | <span data-ttu-id="8ad58-2582">__ハーフ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2582">__Ch__</span></span>  | <span data-ttu-id="8ad58-2583">__&__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2583">__St__</span></span> | <span data-ttu-id="8ad58-2584">__Ob__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2584">__Ob__</span></span> | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| <span data-ttu-id="8ad58-2585">__コンボ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2585">__Bo__</span></span> | <span data-ttu-id="8ad58-2586">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2586">St</span></span> | <span data-ttu-id="8ad58-2587">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2587">St</span></span> | <span data-ttu-id="8ad58-2588">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2588">St</span></span> | <span data-ttu-id="8ad58-2589">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2589">St</span></span> | <span data-ttu-id="8ad58-2590">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2590">St</span></span> | <span data-ttu-id="8ad58-2591">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2591">St</span></span> | <span data-ttu-id="8ad58-2592">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2592">St</span></span> | <span data-ttu-id="8ad58-2593">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2593">St</span></span> | <span data-ttu-id="8ad58-2594">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2594">St</span></span> | <span data-ttu-id="8ad58-2595">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2595">St</span></span> | <span data-ttu-id="8ad58-2596">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2596">St</span></span> | <span data-ttu-id="8ad58-2597">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2597">St</span></span> | <span data-ttu-id="8ad58-2598">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2598">St</span></span> | <span data-ttu-id="8ad58-2599">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2599">St</span></span> | <span data-ttu-id="8ad58-2600">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2600">St</span></span> | <span data-ttu-id="8ad58-2601">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2601">Ob</span></span> | 
| <span data-ttu-id="8ad58-2602">__SB__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2602">__SB__</span></span> |    | <span data-ttu-id="8ad58-2603">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2603">St</span></span> | <span data-ttu-id="8ad58-2604">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2604">St</span></span> | <span data-ttu-id="8ad58-2605">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2605">St</span></span> | <span data-ttu-id="8ad58-2606">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2606">St</span></span> | <span data-ttu-id="8ad58-2607">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2607">St</span></span> | <span data-ttu-id="8ad58-2608">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2608">St</span></span> | <span data-ttu-id="8ad58-2609">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2609">St</span></span> | <span data-ttu-id="8ad58-2610">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2610">St</span></span> | <span data-ttu-id="8ad58-2611">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2611">St</span></span> | <span data-ttu-id="8ad58-2612">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2612">St</span></span> | <span data-ttu-id="8ad58-2613">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2613">St</span></span> | <span data-ttu-id="8ad58-2614">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2614">St</span></span> | <span data-ttu-id="8ad58-2615">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2615">St</span></span> | <span data-ttu-id="8ad58-2616">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2616">St</span></span> | <span data-ttu-id="8ad58-2617">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2617">Ob</span></span> | 
| <span data-ttu-id="8ad58-2618">__は__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2618">__By__</span></span> |    |    | <span data-ttu-id="8ad58-2619">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2619">St</span></span> | <span data-ttu-id="8ad58-2620">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2620">St</span></span> | <span data-ttu-id="8ad58-2621">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2621">St</span></span> | <span data-ttu-id="8ad58-2622">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2622">St</span></span> | <span data-ttu-id="8ad58-2623">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2623">St</span></span> | <span data-ttu-id="8ad58-2624">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2624">St</span></span> | <span data-ttu-id="8ad58-2625">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2625">St</span></span> | <span data-ttu-id="8ad58-2626">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2626">St</span></span> | <span data-ttu-id="8ad58-2627">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2627">St</span></span> | <span data-ttu-id="8ad58-2628">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2628">St</span></span> | <span data-ttu-id="8ad58-2629">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2629">St</span></span> | <span data-ttu-id="8ad58-2630">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2630">St</span></span> | <span data-ttu-id="8ad58-2631">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2631">St</span></span> | <span data-ttu-id="8ad58-2632">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2632">Ob</span></span> | 
| <span data-ttu-id="8ad58-2633">__悪夢__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2633">__Sh__</span></span> |    |    |    | <span data-ttu-id="8ad58-2634">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2634">St</span></span> | <span data-ttu-id="8ad58-2635">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2635">St</span></span> | <span data-ttu-id="8ad58-2636">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2636">St</span></span> | <span data-ttu-id="8ad58-2637">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2637">St</span></span> | <span data-ttu-id="8ad58-2638">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2638">St</span></span> | <span data-ttu-id="8ad58-2639">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2639">St</span></span> | <span data-ttu-id="8ad58-2640">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2640">St</span></span> | <span data-ttu-id="8ad58-2641">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2641">St</span></span> | <span data-ttu-id="8ad58-2642">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2642">St</span></span> | <span data-ttu-id="8ad58-2643">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2643">St</span></span> | <span data-ttu-id="8ad58-2644">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2644">St</span></span> | <span data-ttu-id="8ad58-2645">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2645">St</span></span> | <span data-ttu-id="8ad58-2646">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2646">Ob</span></span> | 
| <span data-ttu-id="8ad58-2647">__米国__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2647">__US__</span></span> |    |    |    |    | <span data-ttu-id="8ad58-2648">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2648">St</span></span> | <span data-ttu-id="8ad58-2649">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2649">St</span></span> | <span data-ttu-id="8ad58-2650">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2650">St</span></span> | <span data-ttu-id="8ad58-2651">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2651">St</span></span> | <span data-ttu-id="8ad58-2652">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2652">St</span></span> | <span data-ttu-id="8ad58-2653">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2653">St</span></span> | <span data-ttu-id="8ad58-2654">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2654">St</span></span> | <span data-ttu-id="8ad58-2655">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2655">St</span></span> | <span data-ttu-id="8ad58-2656">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2656">St</span></span> | <span data-ttu-id="8ad58-2657">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2657">St</span></span> | <span data-ttu-id="8ad58-2658">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2658">St</span></span> | <span data-ttu-id="8ad58-2659">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2659">Ob</span></span> | 
| <span data-ttu-id="8ad58-2660">__In__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2660">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="8ad58-2661">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2661">St</span></span> | <span data-ttu-id="8ad58-2662">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2662">St</span></span> | <span data-ttu-id="8ad58-2663">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2663">St</span></span> | <span data-ttu-id="8ad58-2664">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2664">St</span></span> | <span data-ttu-id="8ad58-2665">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2665">St</span></span> | <span data-ttu-id="8ad58-2666">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2666">St</span></span> | <span data-ttu-id="8ad58-2667">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2667">St</span></span> | <span data-ttu-id="8ad58-2668">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2668">St</span></span> | <span data-ttu-id="8ad58-2669">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2669">St</span></span> | <span data-ttu-id="8ad58-2670">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2670">St</span></span> | <span data-ttu-id="8ad58-2671">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2671">Ob</span></span> | 
| <span data-ttu-id="8ad58-2672">__UI__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2672">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="8ad58-2673">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2673">St</span></span> | <span data-ttu-id="8ad58-2674">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2674">St</span></span> | <span data-ttu-id="8ad58-2675">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2675">St</span></span> | <span data-ttu-id="8ad58-2676">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2676">St</span></span> | <span data-ttu-id="8ad58-2677">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2677">St</span></span> | <span data-ttu-id="8ad58-2678">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2678">St</span></span> | <span data-ttu-id="8ad58-2679">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2679">St</span></span> | <span data-ttu-id="8ad58-2680">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2680">St</span></span> | <span data-ttu-id="8ad58-2681">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2681">St</span></span> | <span data-ttu-id="8ad58-2682">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2682">Ob</span></span> | 
| <span data-ttu-id="8ad58-2683">__Lo__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2683">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2684">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2684">St</span></span> | <span data-ttu-id="8ad58-2685">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2685">St</span></span> | <span data-ttu-id="8ad58-2686">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2686">St</span></span> | <span data-ttu-id="8ad58-2687">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2687">St</span></span> | <span data-ttu-id="8ad58-2688">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2688">St</span></span> | <span data-ttu-id="8ad58-2689">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2689">St</span></span> | <span data-ttu-id="8ad58-2690">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2690">St</span></span> | <span data-ttu-id="8ad58-2691">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2691">St</span></span> | <span data-ttu-id="8ad58-2692">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2692">Ob</span></span> | 
| <span data-ttu-id="8ad58-2693">__UL__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2693">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2694">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2694">St</span></span> | <span data-ttu-id="8ad58-2695">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2695">St</span></span> | <span data-ttu-id="8ad58-2696">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2696">St</span></span> | <span data-ttu-id="8ad58-2697">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2697">St</span></span> | <span data-ttu-id="8ad58-2698">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2698">St</span></span> | <span data-ttu-id="8ad58-2699">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2699">St</span></span> | <span data-ttu-id="8ad58-2700">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2700">St</span></span> | <span data-ttu-id="8ad58-2701">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2701">Ob</span></span> | 
| <span data-ttu-id="8ad58-2702">__解放__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2702">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2703">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2703">St</span></span> | <span data-ttu-id="8ad58-2704">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2704">St</span></span> | <span data-ttu-id="8ad58-2705">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2705">St</span></span> | <span data-ttu-id="8ad58-2706">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2706">St</span></span> | <span data-ttu-id="8ad58-2707">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2707">St</span></span> | <span data-ttu-id="8ad58-2708">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2708">St</span></span> | <span data-ttu-id="8ad58-2709">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2709">Ob</span></span> | 
| <span data-ttu-id="8ad58-2710">__Si__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2710">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2711">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2711">St</span></span> | <span data-ttu-id="8ad58-2712">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2712">St</span></span> | <span data-ttu-id="8ad58-2713">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2713">St</span></span> | <span data-ttu-id="8ad58-2714">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2714">St</span></span> | <span data-ttu-id="8ad58-2715">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2715">St</span></span> | <span data-ttu-id="8ad58-2716">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2716">Ob</span></span> | 
| <span data-ttu-id="8ad58-2717">__Do__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2717">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2718">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2718">St</span></span> | <span data-ttu-id="8ad58-2719">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2719">St</span></span> | <span data-ttu-id="8ad58-2720">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2720">St</span></span> | <span data-ttu-id="8ad58-2721">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2721">St</span></span> | <span data-ttu-id="8ad58-2722">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2722">Ob</span></span> | 
| <span data-ttu-id="8ad58-2723">__オーディオ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2723">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2724">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2724">St</span></span> | <span data-ttu-id="8ad58-2725">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2725">St</span></span> | <span data-ttu-id="8ad58-2726">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2726">St</span></span> | <span data-ttu-id="8ad58-2727">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2727">Ob</span></span> | 
| <span data-ttu-id="8ad58-2728">__ハーフ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2728">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2729">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2729">St</span></span> | <span data-ttu-id="8ad58-2730">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2730">St</span></span> | <span data-ttu-id="8ad58-2731">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2731">Ob</span></span> | 
| <span data-ttu-id="8ad58-2732">__&__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2732">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2733">&</span><span class="sxs-lookup"><span data-stu-id="8ad58-2733">St</span></span> | <span data-ttu-id="8ad58-2734">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2734">Ob</span></span> | 
| <span data-ttu-id="8ad58-2735">__Ob__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2735">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2736">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2736">Ob</span></span> | 


## <a name="logical-operators"></a><span data-ttu-id="8ad58-2737">論理演算子</span><span class="sxs-lookup"><span data-stu-id="8ad58-2737">Logical Operators</span></span>

<span data-ttu-id="8ad58-2738">@No__t-0、`Not`、`Or`、および `Xor` の各演算子は、論理演算子と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2738">The `And`, `Not`, `Or`, and `Xor` operators are called the logical operators.</span></span>

```antlr
LogicalOperatorExpression
    : 'Not' Expression
    | Expression 'And' LineTerminator? Expression
    | Expression 'Or' LineTerminator? Expression
    | Expression 'Xor' LineTerminator? Expression
    ;
```

<span data-ttu-id="8ad58-2739">論理演算子は次のように評価されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2739">The logical operators are evaluated as follows:</span></span>

* <span data-ttu-id="8ad58-2740">@No__t-0 型の場合:</span><span class="sxs-lookup"><span data-stu-id="8ad58-2740">For the `Boolean` type:</span></span>

  * <span data-ttu-id="8ad58-2741">2つのオペランドに対して論理 @no__t 0 操作が実行されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2741">A logical `And` operation is performed on its two operands.</span></span>

  * <span data-ttu-id="8ad58-2742">オペランドに対して論理 @no__t 0 演算が実行されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2742">A logical `Not` operation is performed on its operand.</span></span>

  * <span data-ttu-id="8ad58-2743">2つのオペランドに対して論理 @no__t 0 操作が実行されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2743">A logical `Or` operation is performed on its two operands.</span></span>

  * <span data-ttu-id="8ad58-2744">2つのオペランドに対して、論理的な排他的 `Or` 演算が実行されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2744">A logical exclusive-`Or` operation is performed on its two operands.</span></span>

* <span data-ttu-id="8ad58-2745">@No__t 0、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、@no__t、およびすべての列挙型の場合、指定された操作は、2つのオペランドのバイナリ表現の各ビットに対して実行されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2745">For `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, and all enumerated types, the specified operation is performed on each bit of the binary representation of the two operand(s):</span></span>

  * <span data-ttu-id="8ad58-2746">`And`:両方のビットが1の場合、結果のビットは1になります。それ以外の場合、結果のビットは0になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2746">`And`: The result bit is 1 if both bits are 1; otherwise the result bit is 0.</span></span>

  * <span data-ttu-id="8ad58-2747">`Not`:ビットが0の場合、結果のビットは1になります。それ以外の場合、結果のビットは1になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2747">`Not`: The result bit is 1 if the bit is 0; otherwise the result bit is 1.</span></span>

  * <span data-ttu-id="8ad58-2748">`Or`:いずれかのビットが1の場合、結果のビットは1になります。それ以外の場合、結果のビットは0になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2748">`Or`: The result bit is 1 if either bit is 1; otherwise the result bit is 0.</span></span>

  * <span data-ttu-id="8ad58-2749">`Xor`:いずれかのビットが1で、両方のビットが1ではない場合、結果のビットは1になります。それ以外の場合、結果のビットは 0 (つまり、1 `Xor` 0 = 1、1 `Xor` 1 = 0) になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2749">`Xor`: The result bit is 1 if either bit is 1 but not both bits; otherwise the result bit is 0 (that is, 1 `Xor` 0 = 1, 1 `Xor` 1 = 0).</span></span>

* <span data-ttu-id="8ad58-2750">論理演算子 `And` および `Or` が型 `Boolean?` でリフトされている場合、次のように3つの値を持つブール型のロジックが含まれるように拡張されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2750">When the logical operators `And` and `Or` are lifted for the type `Boolean?`, they are extended to encompass three-valued Boolean logic as such:</span></span>

  * <span data-ttu-id="8ad58-2751">`And` は、両方のオペランドが true の場合、true に評価されます。オペランドのいずれかが false の場合は false。それ以外の場合は `Nothing`。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2751">`And` evaluates to true if both operands are true; false if one of the operands is false; `Nothing` otherwise.</span></span>

  * <span data-ttu-id="8ad58-2752">`Or` のいずれかのオペランドが true の場合、true に評価されます。false は両方とも false です。それ以外の場合は `Nothing`。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2752">`Or` evaluates to true if either operand is true; false is both operands are false; `Nothing` otherwise.</span></span>

<span data-ttu-id="8ad58-2753">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2753">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim x?, y? As Boolean

        x = Nothing
        y = True 

        If x Or y Then
            ' Will execute
        End If
    End Sub
End Module
```

<span data-ttu-id="8ad58-2754">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2754">__Note.__</span></span> <span data-ttu-id="8ad58-2755">理想的には、ブール式 (たとえば、`IsTrue` と `IsFalse` を実装する型) で使用できる任意の型に対して3つの値のロジックを使用して、論理演算子 `And` および @no__t をリフトします。 @no__t これは、任意のブール式で使用できる型。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2755">Ideally, the logical operators `And` and `Or` would be lifted using three-valued logic for any type that can be used in a Boolean expression (i.e. a type that implements `IsTrue` and `IsFalse`), in the same way that `AndAlso` and `OrElse` short circuit across any type that can be used in a Boolean expression.</span></span> <span data-ttu-id="8ad58-2756">残念ながら、3つの値を持つリフトは `Boolean?` にのみ適用されるため、3つの値を持つロジックを必要とするユーザー定義型では、null 許容バージョンに対して `And` および `Or` 演算子を定義することで、手動で行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2756">Unfortunately, three-valued lifting is only applied to `Boolean?`, so user-defined types that desire three-valued logic must do so manually by defining `And` and `Or` operators for their nullable version.</span></span>

<span data-ttu-id="8ad58-2757">これらの操作によってオーバーフローが発生することはありません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2757">No overflows are possible from these operations.</span></span> <span data-ttu-id="8ad58-2758">列挙型の演算子は、列挙型の基になる型に対してビットごとの演算を実行しますが、戻り値は列挙型です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2758">The enumerated type operators do the bitwise operation on the underlying type of the enumerated type, but the return value is the enumerated type.</span></span>

<span data-ttu-id="8ad58-2759">__操作の種類がありません:__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2759">__Not Operation Type:__</span></span>

| <span data-ttu-id="8ad58-2760">__コンボ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2760">__Bo__</span></span> | <span data-ttu-id="8ad58-2761">__SB__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2761">__SB__</span></span> | <span data-ttu-id="8ad58-2762">__は__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2762">__By__</span></span> | <span data-ttu-id="8ad58-2763">__悪夢__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2763">__Sh__</span></span> | <span data-ttu-id="8ad58-2764">__米国__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2764">__US__</span></span> | <span data-ttu-id="8ad58-2765">__In__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2765">__In__</span></span> | <span data-ttu-id="8ad58-2766">__UI__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2766">__UI__</span></span> | <span data-ttu-id="8ad58-2767">__Lo__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2767">__Lo__</span></span> | <span data-ttu-id="8ad58-2768">__UL__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2768">__UL__</span></span> | <span data-ttu-id="8ad58-2769">__解放__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2769">__De__</span></span> | <span data-ttu-id="8ad58-2770">__Si__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2770">__Si__</span></span> | <span data-ttu-id="8ad58-2771">__Do__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2771">__Do__</span></span> | <span data-ttu-id="8ad58-2772">__オーディオ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2772">__Da__</span></span>  | <span data-ttu-id="8ad58-2773">__ハーフ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2773">__Ch__</span></span>  | <span data-ttu-id="8ad58-2774">__&__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2774">__St__</span></span> | <span data-ttu-id="8ad58-2775">__Ob__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2775">__Ob__</span></span> | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="8ad58-2776">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-2776">Bo</span></span> | <span data-ttu-id="8ad58-2777">SB</span><span class="sxs-lookup"><span data-stu-id="8ad58-2777">SB</span></span> | <span data-ttu-id="8ad58-2778">By</span><span class="sxs-lookup"><span data-stu-id="8ad58-2778">By</span></span> | <span data-ttu-id="8ad58-2779">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-2779">Sh</span></span> | <span data-ttu-id="8ad58-2780">US</span><span class="sxs-lookup"><span data-stu-id="8ad58-2780">US</span></span> | <span data-ttu-id="8ad58-2781">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-2781">In</span></span> | <span data-ttu-id="8ad58-2782">UI</span><span class="sxs-lookup"><span data-stu-id="8ad58-2782">UI</span></span> | <span data-ttu-id="8ad58-2783">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2783">Lo</span></span> | <span data-ttu-id="8ad58-2784">UL</span><span class="sxs-lookup"><span data-stu-id="8ad58-2784">UL</span></span> | <span data-ttu-id="8ad58-2785">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2785">Lo</span></span> | <span data-ttu-id="8ad58-2786">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2786">Lo</span></span> | <span data-ttu-id="8ad58-2787">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2787">Lo</span></span> | <span data-ttu-id="8ad58-2788">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2788">Err</span></span> | <span data-ttu-id="8ad58-2789">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2789">Err</span></span> | <span data-ttu-id="8ad58-2790">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2790">Lo</span></span> | <span data-ttu-id="8ad58-2791">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2791">Ob</span></span> | 

<span data-ttu-id="8ad58-2792">__And、Or、Xor 演算型:__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2792">__And, Or, Xor Operation Type:__</span></span>

|        | <span data-ttu-id="8ad58-2793">__コンボ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2793">__Bo__</span></span> | <span data-ttu-id="8ad58-2794">__SB__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2794">__SB__</span></span> | <span data-ttu-id="8ad58-2795">__は__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2795">__By__</span></span> | <span data-ttu-id="8ad58-2796">__悪夢__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2796">__Sh__</span></span> | <span data-ttu-id="8ad58-2797">__米国__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2797">__US__</span></span> | <span data-ttu-id="8ad58-2798">__In__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2798">__In__</span></span> | <span data-ttu-id="8ad58-2799">__UI__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2799">__UI__</span></span> | <span data-ttu-id="8ad58-2800">__Lo__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2800">__Lo__</span></span> | <span data-ttu-id="8ad58-2801">__UL__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2801">__UL__</span></span> | <span data-ttu-id="8ad58-2802">__解放__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2802">__De__</span></span> | <span data-ttu-id="8ad58-2803">__Si__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2803">__Si__</span></span> | <span data-ttu-id="8ad58-2804">__Do__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2804">__Do__</span></span> | <span data-ttu-id="8ad58-2805">__オーディオ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2805">__Da__</span></span>  | <span data-ttu-id="8ad58-2806">__ハーフ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2806">__Ch__</span></span>  | <span data-ttu-id="8ad58-2807">__&__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2807">__St__</span></span> | <span data-ttu-id="8ad58-2808">__Ob__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2808">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="8ad58-2809">__コンボ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2809">__Bo__</span></span> | <span data-ttu-id="8ad58-2810">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-2810">Bo</span></span> | <span data-ttu-id="8ad58-2811">SB</span><span class="sxs-lookup"><span data-stu-id="8ad58-2811">SB</span></span> | <span data-ttu-id="8ad58-2812">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-2812">Sh</span></span> | <span data-ttu-id="8ad58-2813">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-2813">Sh</span></span> | <span data-ttu-id="8ad58-2814">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-2814">In</span></span> | <span data-ttu-id="8ad58-2815">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-2815">In</span></span> | <span data-ttu-id="8ad58-2816">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2816">Lo</span></span> | <span data-ttu-id="8ad58-2817">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2817">Lo</span></span> | <span data-ttu-id="8ad58-2818">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2818">Lo</span></span> | <span data-ttu-id="8ad58-2819">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2819">Lo</span></span> | <span data-ttu-id="8ad58-2820">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2820">Lo</span></span> | <span data-ttu-id="8ad58-2821">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2821">Lo</span></span> | <span data-ttu-id="8ad58-2822">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2822">Err</span></span> | <span data-ttu-id="8ad58-2823">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2823">Err</span></span> | <span data-ttu-id="8ad58-2824">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-2824">Bo</span></span>  | <span data-ttu-id="8ad58-2825">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2825">Ob</span></span>  | 
| <span data-ttu-id="8ad58-2826">__SB__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2826">__SB__</span></span> |    | <span data-ttu-id="8ad58-2827">SB</span><span class="sxs-lookup"><span data-stu-id="8ad58-2827">SB</span></span> | <span data-ttu-id="8ad58-2828">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-2828">Sh</span></span> | <span data-ttu-id="8ad58-2829">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-2829">Sh</span></span> | <span data-ttu-id="8ad58-2830">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-2830">In</span></span> | <span data-ttu-id="8ad58-2831">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-2831">In</span></span> | <span data-ttu-id="8ad58-2832">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2832">Lo</span></span> | <span data-ttu-id="8ad58-2833">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2833">Lo</span></span> | <span data-ttu-id="8ad58-2834">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2834">Lo</span></span> | <span data-ttu-id="8ad58-2835">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2835">Lo</span></span> | <span data-ttu-id="8ad58-2836">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2836">Lo</span></span> | <span data-ttu-id="8ad58-2837">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2837">Lo</span></span> | <span data-ttu-id="8ad58-2838">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2838">Err</span></span> | <span data-ttu-id="8ad58-2839">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2839">Err</span></span> | <span data-ttu-id="8ad58-2840">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2840">Lo</span></span>  | <span data-ttu-id="8ad58-2841">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2841">Ob</span></span>  | 
| <span data-ttu-id="8ad58-2842">__は__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2842">__By__</span></span> |    |    | <span data-ttu-id="8ad58-2843">By</span><span class="sxs-lookup"><span data-stu-id="8ad58-2843">By</span></span> | <span data-ttu-id="8ad58-2844">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-2844">Sh</span></span> | <span data-ttu-id="8ad58-2845">US</span><span class="sxs-lookup"><span data-stu-id="8ad58-2845">US</span></span> | <span data-ttu-id="8ad58-2846">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-2846">In</span></span> | <span data-ttu-id="8ad58-2847">UI</span><span class="sxs-lookup"><span data-stu-id="8ad58-2847">UI</span></span> | <span data-ttu-id="8ad58-2848">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2848">Lo</span></span> | <span data-ttu-id="8ad58-2849">UL</span><span class="sxs-lookup"><span data-stu-id="8ad58-2849">UL</span></span> | <span data-ttu-id="8ad58-2850">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2850">Lo</span></span> | <span data-ttu-id="8ad58-2851">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2851">Lo</span></span> | <span data-ttu-id="8ad58-2852">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2852">Lo</span></span> | <span data-ttu-id="8ad58-2853">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2853">Err</span></span> | <span data-ttu-id="8ad58-2854">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2854">Err</span></span> | <span data-ttu-id="8ad58-2855">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2855">Lo</span></span>  | <span data-ttu-id="8ad58-2856">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2856">Ob</span></span>  | 
| <span data-ttu-id="8ad58-2857">__悪夢__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2857">__Sh__</span></span> |    |    |    | <span data-ttu-id="8ad58-2858">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-2858">Sh</span></span> | <span data-ttu-id="8ad58-2859">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-2859">In</span></span> | <span data-ttu-id="8ad58-2860">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-2860">In</span></span> | <span data-ttu-id="8ad58-2861">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2861">Lo</span></span> | <span data-ttu-id="8ad58-2862">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2862">Lo</span></span> | <span data-ttu-id="8ad58-2863">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2863">Lo</span></span> | <span data-ttu-id="8ad58-2864">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2864">Lo</span></span> | <span data-ttu-id="8ad58-2865">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2865">Lo</span></span> | <span data-ttu-id="8ad58-2866">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2866">Lo</span></span> | <span data-ttu-id="8ad58-2867">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2867">Err</span></span> | <span data-ttu-id="8ad58-2868">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2868">Err</span></span> | <span data-ttu-id="8ad58-2869">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2869">Lo</span></span>  | <span data-ttu-id="8ad58-2870">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2870">Ob</span></span>  | 
| <span data-ttu-id="8ad58-2871">__米国__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2871">__US__</span></span> |    |    |    |    | <span data-ttu-id="8ad58-2872">US</span><span class="sxs-lookup"><span data-stu-id="8ad58-2872">US</span></span> | <span data-ttu-id="8ad58-2873">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-2873">In</span></span> | <span data-ttu-id="8ad58-2874">UI</span><span class="sxs-lookup"><span data-stu-id="8ad58-2874">UI</span></span> | <span data-ttu-id="8ad58-2875">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2875">Lo</span></span> | <span data-ttu-id="8ad58-2876">UL</span><span class="sxs-lookup"><span data-stu-id="8ad58-2876">UL</span></span> | <span data-ttu-id="8ad58-2877">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2877">Lo</span></span> | <span data-ttu-id="8ad58-2878">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2878">Lo</span></span> | <span data-ttu-id="8ad58-2879">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2879">Lo</span></span> | <span data-ttu-id="8ad58-2880">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2880">Err</span></span> | <span data-ttu-id="8ad58-2881">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2881">Err</span></span> | <span data-ttu-id="8ad58-2882">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2882">Lo</span></span>  | <span data-ttu-id="8ad58-2883">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2883">Ob</span></span>  | 
| <span data-ttu-id="8ad58-2884">__In__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2884">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="8ad58-2885">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-2885">In</span></span> | <span data-ttu-id="8ad58-2886">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2886">Lo</span></span> | <span data-ttu-id="8ad58-2887">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2887">Lo</span></span> | <span data-ttu-id="8ad58-2888">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2888">Lo</span></span> | <span data-ttu-id="8ad58-2889">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2889">Lo</span></span> | <span data-ttu-id="8ad58-2890">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2890">Lo</span></span> | <span data-ttu-id="8ad58-2891">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2891">Lo</span></span> | <span data-ttu-id="8ad58-2892">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2892">Err</span></span> | <span data-ttu-id="8ad58-2893">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2893">Err</span></span> | <span data-ttu-id="8ad58-2894">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2894">Lo</span></span>  | <span data-ttu-id="8ad58-2895">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2895">Ob</span></span>  | 
| <span data-ttu-id="8ad58-2896">__UI__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2896">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="8ad58-2897">UI</span><span class="sxs-lookup"><span data-stu-id="8ad58-2897">UI</span></span> | <span data-ttu-id="8ad58-2898">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2898">Lo</span></span> | <span data-ttu-id="8ad58-2899">UL</span><span class="sxs-lookup"><span data-stu-id="8ad58-2899">UL</span></span> | <span data-ttu-id="8ad58-2900">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2900">Lo</span></span> | <span data-ttu-id="8ad58-2901">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2901">Lo</span></span> | <span data-ttu-id="8ad58-2902">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2902">Lo</span></span> | <span data-ttu-id="8ad58-2903">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2903">Err</span></span> | <span data-ttu-id="8ad58-2904">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2904">Err</span></span> | <span data-ttu-id="8ad58-2905">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2905">Lo</span></span>  | <span data-ttu-id="8ad58-2906">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2906">Ob</span></span>  | 
| <span data-ttu-id="8ad58-2907">__Lo__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2907">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2908">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2908">Lo</span></span> | <span data-ttu-id="8ad58-2909">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2909">Lo</span></span> | <span data-ttu-id="8ad58-2910">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2910">Lo</span></span> | <span data-ttu-id="8ad58-2911">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2911">Lo</span></span> | <span data-ttu-id="8ad58-2912">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2912">Lo</span></span> | <span data-ttu-id="8ad58-2913">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2913">Err</span></span> | <span data-ttu-id="8ad58-2914">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2914">Err</span></span> | <span data-ttu-id="8ad58-2915">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2915">Lo</span></span>  | <span data-ttu-id="8ad58-2916">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2916">Ob</span></span>  | 
| <span data-ttu-id="8ad58-2917">__UL__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2917">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2918">UL</span><span class="sxs-lookup"><span data-stu-id="8ad58-2918">UL</span></span> | <span data-ttu-id="8ad58-2919">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2919">Lo</span></span> | <span data-ttu-id="8ad58-2920">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2920">Lo</span></span> | <span data-ttu-id="8ad58-2921">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2921">Lo</span></span> | <span data-ttu-id="8ad58-2922">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2922">Err</span></span> | <span data-ttu-id="8ad58-2923">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2923">Err</span></span> | <span data-ttu-id="8ad58-2924">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2924">Lo</span></span>  | <span data-ttu-id="8ad58-2925">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2925">Ob</span></span>  | 
| <span data-ttu-id="8ad58-2926">__解放__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2926">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2927">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2927">Lo</span></span> | <span data-ttu-id="8ad58-2928">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2928">Lo</span></span> | <span data-ttu-id="8ad58-2929">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2929">Lo</span></span> | <span data-ttu-id="8ad58-2930">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2930">Err</span></span> | <span data-ttu-id="8ad58-2931">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2931">Err</span></span> | <span data-ttu-id="8ad58-2932">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2932">Lo</span></span>  | <span data-ttu-id="8ad58-2933">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2933">Ob</span></span>  | 
| <span data-ttu-id="8ad58-2934">__Si__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2934">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2935">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2935">Lo</span></span> | <span data-ttu-id="8ad58-2936">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2936">Lo</span></span> | <span data-ttu-id="8ad58-2937">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2937">Err</span></span> | <span data-ttu-id="8ad58-2938">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2938">Err</span></span> | <span data-ttu-id="8ad58-2939">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2939">Lo</span></span>  | <span data-ttu-id="8ad58-2940">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2940">Ob</span></span>  | 
| <span data-ttu-id="8ad58-2941">__Do__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2941">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2942">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2942">Lo</span></span> | <span data-ttu-id="8ad58-2943">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2943">Err</span></span> | <span data-ttu-id="8ad58-2944">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2944">Err</span></span> | <span data-ttu-id="8ad58-2945">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2945">Lo</span></span>  | <span data-ttu-id="8ad58-2946">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2946">Ob</span></span>  | 
| <span data-ttu-id="8ad58-2947">__オーディオ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2947">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-2948">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2948">Err</span></span> | <span data-ttu-id="8ad58-2949">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2949">Err</span></span> | <span data-ttu-id="8ad58-2950">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2950">Err</span></span> | <span data-ttu-id="8ad58-2951">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2951">Err</span></span> | 
| <span data-ttu-id="8ad58-2952">__ハーフ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2952">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="8ad58-2953">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2953">Err</span></span> | <span data-ttu-id="8ad58-2954">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2954">Err</span></span> | <span data-ttu-id="8ad58-2955">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-2955">Err</span></span> | 
| <span data-ttu-id="8ad58-2956">__&__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2956">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="8ad58-2957">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-2957">Lo</span></span>  | <span data-ttu-id="8ad58-2958">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2958">Ob</span></span>  | 
| <span data-ttu-id="8ad58-2959">__Ob__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2959">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="8ad58-2960">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-2960">Ob</span></span>  | 


### <a name="short-circuiting-logical-operators"></a><span data-ttu-id="8ad58-2961">ショートサーキット論理演算子</span><span class="sxs-lookup"><span data-stu-id="8ad58-2961">Short-circuiting Logical Operators</span></span>

<span data-ttu-id="8ad58-2962">@No__t-0 および `OrElse` 演算子は、`And` および `Or` 論理演算子のショートサーキットバージョンです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2962">The `AndAlso` and `OrElse` operators are the short-circuiting versions of the `And` and `Or` logical operators.</span></span>

```antlr
ShortCircuitLogicalOperatorExpression
    : Expression 'AndAlso' LineTerminator? Expression
    | Expression 'OrElse' LineTerminator? Expression
    ;
```

<span data-ttu-id="8ad58-2963">2番目のオペランドは、最初のオペランドを評価した後に演算子の結果がわかっている場合に、実行時に評価されません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2963">Because of their short circuiting behavior, the second operand is not evaluated at run time if the operator result is known after evaluating the first operand.</span></span>

<span data-ttu-id="8ad58-2964">ショートサーキット論理演算子は、次のように評価されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2964">The short-circuiting logical operators are evaluated as follows:</span></span>

* <span data-ttu-id="8ad58-2965">@No__t 0 演算の最初のオペランドが `False` に評価された場合、または `IsFalse` 演算子から True が返された場合、式は最初のオペランドを返します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2965">If the first operand in an `AndAlso` operation evaluates to `False` or returns True from its `IsFalse` operator, the expression returns its first operand.</span></span> <span data-ttu-id="8ad58-2966">それ以外の場合は、2番目のオペランドが評価され、2つの結果に対して論理 @no__t 0 演算が実行されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2966">Otherwise, the second operand is evaluated and a logical `And` operation is performed on the two results.</span></span>

* <span data-ttu-id="8ad58-2967">@No__t 0 演算の最初のオペランドが `True` に評価された場合、または `IsTrue` 演算子から True が返された場合、式は最初のオペランドを返します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2967">If the first operand in an `OrElse` operation evaluates to `True` or returns True from its `IsTrue` operator, the expression returns its first operand.</span></span> <span data-ttu-id="8ad58-2968">それ以外の場合は、2番目のオペランドが評価され、2つの結果に対して論理 @no__t 0 演算が実行されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2968">Otherwise, the second operand is evaluated and a logical `Or` operation is performed on its two results.</span></span>

<span data-ttu-id="8ad58-2969">@No__t-0 および `OrElse` 演算子は、型 `Boolean`、または次の演算子をオーバーロードする型 `T` に対して定義されています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2969">The `AndAlso` and `OrElse` operators are defined for the type `Boolean`, or for any type `T` that overloads the following operators:</span></span>

```vb
Public Shared Operator IsTrue(op As T) As Boolean
Public Shared Operator IsFalse(op As T) As Boolean
```

<span data-ttu-id="8ad58-2970">また、対応する `And` または `Or` 演算子をオーバーロードすることもできます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2970">as well as overloading the corresponding `And` or `Or` operator:</span></span>

```vb
Public Shared Operator And(op1 As T, op2 As T) As T
Public Shared Operator Or(op1 As T, op2 As T) As T
```

<span data-ttu-id="8ad58-2971">@No__t-0 または `OrElse` の演算子を評価する場合、最初のオペランドは1回だけ評価され、2番目のオペランドは評価も評価もされません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2971">When evaluating the `AndAlso` or `OrElse` operators, the first operand is evaluated only once, and the second operand is either not evaluated or evaluated exactly once.</span></span> <span data-ttu-id="8ad58-2972">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2972">For example, consider the following code:</span></span>

```vb
Module Test
    Function TrueValue() As Boolean
        Console.Write(" True")
        Return True
    End Function

    Function FalseValue() As Boolean
        Console.Write(" False")
        Return False
    End Function

    Sub Main()
        Console.Write("And:")
        If FalseValue() And TrueValue() Then
        End If
        Console.WriteLine()

        Console.Write("Or:")
        If TrueValue() Or FalseValue() Then
        End If
        Console.WriteLine()

        Console.Write("AndAlso:")
        If FalseValue() AndAlso TrueValue() Then
        End If
        Console.WriteLine()

        Console.Write("OrElse:")
        If TrueValue() OrElse FalseValue() Then
        End If
        Console.WriteLine()
    End Sub
End Module
```

<span data-ttu-id="8ad58-2973">次の結果が出力されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2973">It prints the following result:</span></span>

```console
And: False True
Or: True False
AndAlso: False
OrElse: True
```

<span data-ttu-id="8ad58-2974">@No__t-0 および `OrElse` 演算子のリフトされた形式では、最初のオペランドが null `Boolean?` の場合、2番目のオペランドが評価されますが、結果は常に null `Boolean?` になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-2974">In the lifted form of the `AndAlso` and `OrElse` operators, if the first operand was a null `Boolean?`, then the second operand is evaluated but the result is always a null `Boolean?`.</span></span>

<span data-ttu-id="8ad58-2975">__操作の種類:__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2975">__Operation Type:__</span></span>

|        | <span data-ttu-id="8ad58-2976">__コンボ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2976">__Bo__</span></span> | <span data-ttu-id="8ad58-2977">__SB__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2977">__SB__</span></span> | <span data-ttu-id="8ad58-2978">__は__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2978">__By__</span></span> | <span data-ttu-id="8ad58-2979">__悪夢__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2979">__Sh__</span></span> | <span data-ttu-id="8ad58-2980">__米国__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2980">__US__</span></span> | <span data-ttu-id="8ad58-2981">__In__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2981">__In__</span></span> | <span data-ttu-id="8ad58-2982">__UI__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2982">__UI__</span></span> | <span data-ttu-id="8ad58-2983">__Lo__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2983">__Lo__</span></span> | <span data-ttu-id="8ad58-2984">__UL__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2984">__UL__</span></span> | <span data-ttu-id="8ad58-2985">__解放__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2985">__De__</span></span> | <span data-ttu-id="8ad58-2986">__Si__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2986">__Si__</span></span> | <span data-ttu-id="8ad58-2987">__Do__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2987">__Do__</span></span> | <span data-ttu-id="8ad58-2988">__オーディオ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2988">__Da__</span></span>  | <span data-ttu-id="8ad58-2989">__ハーフ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2989">__Ch__</span></span>  | <span data-ttu-id="8ad58-2990">__&__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2990">__St__</span></span> | <span data-ttu-id="8ad58-2991">__Ob__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2991">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="8ad58-2992">__コンボ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-2992">__Bo__</span></span> | <span data-ttu-id="8ad58-2993">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-2993">Bo</span></span> | <span data-ttu-id="8ad58-2994">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-2994">Bo</span></span> | <span data-ttu-id="8ad58-2995">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-2995">Bo</span></span> | <span data-ttu-id="8ad58-2996">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-2996">Bo</span></span> | <span data-ttu-id="8ad58-2997">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-2997">Bo</span></span> | <span data-ttu-id="8ad58-2998">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-2998">Bo</span></span> | <span data-ttu-id="8ad58-2999">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-2999">Bo</span></span> | <span data-ttu-id="8ad58-3000">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3000">Bo</span></span> | <span data-ttu-id="8ad58-3001">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3001">Bo</span></span> | <span data-ttu-id="8ad58-3002">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3002">Bo</span></span> | <span data-ttu-id="8ad58-3003">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3003">Bo</span></span> | <span data-ttu-id="8ad58-3004">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3004">Bo</span></span> | <span data-ttu-id="8ad58-3005">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3005">Err</span></span> | <span data-ttu-id="8ad58-3006">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3006">Err</span></span> | <span data-ttu-id="8ad58-3007">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3007">Bo</span></span>  | <span data-ttu-id="8ad58-3008">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-3008">Ob</span></span>  | 
| <span data-ttu-id="8ad58-3009">__SB__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3009">__SB__</span></span> |    | <span data-ttu-id="8ad58-3010">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3010">Bo</span></span> | <span data-ttu-id="8ad58-3011">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3011">Bo</span></span> | <span data-ttu-id="8ad58-3012">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3012">Bo</span></span> | <span data-ttu-id="8ad58-3013">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3013">Bo</span></span> | <span data-ttu-id="8ad58-3014">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3014">Bo</span></span> | <span data-ttu-id="8ad58-3015">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3015">Bo</span></span> | <span data-ttu-id="8ad58-3016">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3016">Bo</span></span> | <span data-ttu-id="8ad58-3017">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3017">Bo</span></span> | <span data-ttu-id="8ad58-3018">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3018">Bo</span></span> | <span data-ttu-id="8ad58-3019">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3019">Bo</span></span> | <span data-ttu-id="8ad58-3020">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3020">Bo</span></span> | <span data-ttu-id="8ad58-3021">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3021">Err</span></span> | <span data-ttu-id="8ad58-3022">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3022">Err</span></span> | <span data-ttu-id="8ad58-3023">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3023">Bo</span></span>  | <span data-ttu-id="8ad58-3024">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-3024">Ob</span></span>  | 
| <span data-ttu-id="8ad58-3025">__は__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3025">__By__</span></span> |    |    | <span data-ttu-id="8ad58-3026">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3026">Bo</span></span> | <span data-ttu-id="8ad58-3027">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3027">Bo</span></span> | <span data-ttu-id="8ad58-3028">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3028">Bo</span></span> | <span data-ttu-id="8ad58-3029">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3029">Bo</span></span> | <span data-ttu-id="8ad58-3030">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3030">Bo</span></span> | <span data-ttu-id="8ad58-3031">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3031">Bo</span></span> | <span data-ttu-id="8ad58-3032">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3032">Bo</span></span> | <span data-ttu-id="8ad58-3033">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3033">Bo</span></span> | <span data-ttu-id="8ad58-3034">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3034">Bo</span></span> | <span data-ttu-id="8ad58-3035">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3035">Bo</span></span> | <span data-ttu-id="8ad58-3036">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3036">Err</span></span> | <span data-ttu-id="8ad58-3037">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3037">Err</span></span> | <span data-ttu-id="8ad58-3038">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3038">Bo</span></span>  | <span data-ttu-id="8ad58-3039">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-3039">Ob</span></span>  | 
| <span data-ttu-id="8ad58-3040">__悪夢__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3040">__Sh__</span></span> |    |    |    | <span data-ttu-id="8ad58-3041">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3041">Bo</span></span> | <span data-ttu-id="8ad58-3042">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3042">Bo</span></span> | <span data-ttu-id="8ad58-3043">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3043">Bo</span></span> | <span data-ttu-id="8ad58-3044">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3044">Bo</span></span> | <span data-ttu-id="8ad58-3045">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3045">Bo</span></span> | <span data-ttu-id="8ad58-3046">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3046">Bo</span></span> | <span data-ttu-id="8ad58-3047">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3047">Bo</span></span> | <span data-ttu-id="8ad58-3048">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3048">Bo</span></span> | <span data-ttu-id="8ad58-3049">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3049">Bo</span></span> | <span data-ttu-id="8ad58-3050">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3050">Err</span></span> | <span data-ttu-id="8ad58-3051">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3051">Err</span></span> | <span data-ttu-id="8ad58-3052">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3052">Bo</span></span>  | <span data-ttu-id="8ad58-3053">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-3053">Ob</span></span>  | 
| <span data-ttu-id="8ad58-3054">__米国__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3054">__US__</span></span> |    |    |    |    | <span data-ttu-id="8ad58-3055">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3055">Bo</span></span> | <span data-ttu-id="8ad58-3056">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3056">Bo</span></span> | <span data-ttu-id="8ad58-3057">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3057">Bo</span></span> | <span data-ttu-id="8ad58-3058">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3058">Bo</span></span> | <span data-ttu-id="8ad58-3059">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3059">Bo</span></span> | <span data-ttu-id="8ad58-3060">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3060">Bo</span></span> | <span data-ttu-id="8ad58-3061">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3061">Bo</span></span> | <span data-ttu-id="8ad58-3062">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3062">Bo</span></span> | <span data-ttu-id="8ad58-3063">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3063">Err</span></span> | <span data-ttu-id="8ad58-3064">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3064">Err</span></span> | <span data-ttu-id="8ad58-3065">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3065">Bo</span></span>  | <span data-ttu-id="8ad58-3066">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-3066">Ob</span></span>  | 
| <span data-ttu-id="8ad58-3067">__In__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3067">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="8ad58-3068">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3068">Bo</span></span> | <span data-ttu-id="8ad58-3069">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3069">Bo</span></span> | <span data-ttu-id="8ad58-3070">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3070">Bo</span></span> | <span data-ttu-id="8ad58-3071">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3071">Bo</span></span> | <span data-ttu-id="8ad58-3072">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3072">Bo</span></span> | <span data-ttu-id="8ad58-3073">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3073">Bo</span></span> | <span data-ttu-id="8ad58-3074">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3074">Bo</span></span> | <span data-ttu-id="8ad58-3075">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3075">Err</span></span> | <span data-ttu-id="8ad58-3076">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3076">Err</span></span> | <span data-ttu-id="8ad58-3077">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3077">Bo</span></span>  | <span data-ttu-id="8ad58-3078">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-3078">Ob</span></span>  | 
| <span data-ttu-id="8ad58-3079">__UI__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3079">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="8ad58-3080">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3080">Bo</span></span> | <span data-ttu-id="8ad58-3081">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3081">Bo</span></span> | <span data-ttu-id="8ad58-3082">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3082">Bo</span></span> | <span data-ttu-id="8ad58-3083">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3083">Bo</span></span> | <span data-ttu-id="8ad58-3084">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3084">Bo</span></span> | <span data-ttu-id="8ad58-3085">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3085">Bo</span></span> | <span data-ttu-id="8ad58-3086">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3086">Err</span></span> | <span data-ttu-id="8ad58-3087">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3087">Err</span></span> | <span data-ttu-id="8ad58-3088">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3088">Bo</span></span>  | <span data-ttu-id="8ad58-3089">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-3089">Ob</span></span>  | 
| <span data-ttu-id="8ad58-3090">__Lo__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3090">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-3091">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3091">Bo</span></span> | <span data-ttu-id="8ad58-3092">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3092">Bo</span></span> | <span data-ttu-id="8ad58-3093">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3093">Bo</span></span> | <span data-ttu-id="8ad58-3094">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3094">Bo</span></span> | <span data-ttu-id="8ad58-3095">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3095">Bo</span></span> | <span data-ttu-id="8ad58-3096">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3096">Err</span></span> | <span data-ttu-id="8ad58-3097">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3097">Err</span></span> | <span data-ttu-id="8ad58-3098">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3098">Bo</span></span>  | <span data-ttu-id="8ad58-3099">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-3099">Ob</span></span>  | 
| <span data-ttu-id="8ad58-3100">__UL__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3100">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-3101">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3101">Bo</span></span> | <span data-ttu-id="8ad58-3102">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3102">Bo</span></span> | <span data-ttu-id="8ad58-3103">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3103">Bo</span></span> | <span data-ttu-id="8ad58-3104">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3104">Bo</span></span> | <span data-ttu-id="8ad58-3105">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3105">Err</span></span> | <span data-ttu-id="8ad58-3106">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3106">Err</span></span> | <span data-ttu-id="8ad58-3107">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3107">Bo</span></span>  | <span data-ttu-id="8ad58-3108">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-3108">Ob</span></span>  | 
| <span data-ttu-id="8ad58-3109">__解放__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3109">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-3110">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3110">Bo</span></span> | <span data-ttu-id="8ad58-3111">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3111">Bo</span></span> | <span data-ttu-id="8ad58-3112">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3112">Bo</span></span> | <span data-ttu-id="8ad58-3113">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3113">Err</span></span> | <span data-ttu-id="8ad58-3114">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3114">Err</span></span> | <span data-ttu-id="8ad58-3115">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3115">Bo</span></span>  | <span data-ttu-id="8ad58-3116">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-3116">Ob</span></span>  | 
| <span data-ttu-id="8ad58-3117">__Si__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3117">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-3118">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3118">Bo</span></span> | <span data-ttu-id="8ad58-3119">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3119">Bo</span></span> | <span data-ttu-id="8ad58-3120">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3120">Err</span></span> | <span data-ttu-id="8ad58-3121">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3121">Err</span></span> | <span data-ttu-id="8ad58-3122">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3122">Bo</span></span>  | <span data-ttu-id="8ad58-3123">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-3123">Ob</span></span>  | 
| <span data-ttu-id="8ad58-3124">__Do__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3124">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-3125">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3125">Bo</span></span> | <span data-ttu-id="8ad58-3126">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3126">Err</span></span> | <span data-ttu-id="8ad58-3127">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3127">Err</span></span> | <span data-ttu-id="8ad58-3128">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3128">Bo</span></span>  | <span data-ttu-id="8ad58-3129">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-3129">Ob</span></span>  | 
| <span data-ttu-id="8ad58-3130">__オーディオ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3130">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="8ad58-3131">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3131">Err</span></span> | <span data-ttu-id="8ad58-3132">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3132">Err</span></span> | <span data-ttu-id="8ad58-3133">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3133">Err</span></span> | <span data-ttu-id="8ad58-3134">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3134">Err</span></span> | 
| <span data-ttu-id="8ad58-3135">__ハーフ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3135">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="8ad58-3136">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3136">Err</span></span> | <span data-ttu-id="8ad58-3137">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3137">Err</span></span> | <span data-ttu-id="8ad58-3138">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3138">Err</span></span> | 
| <span data-ttu-id="8ad58-3139">__&__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3139">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="8ad58-3140">コンボ</span><span class="sxs-lookup"><span data-stu-id="8ad58-3140">Bo</span></span>  | <span data-ttu-id="8ad58-3141">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-3141">Ob</span></span>  | 
| <span data-ttu-id="8ad58-3142">__Ob__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3142">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="8ad58-3143">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-3143">Ob</span></span>  | 


## <a name="shift-operators"></a><span data-ttu-id="8ad58-3144">シフト演算子</span><span class="sxs-lookup"><span data-stu-id="8ad58-3144">Shift Operators</span></span>

<span data-ttu-id="8ad58-3145">二項演算子 `<<` および `>>` は、ビットシフト演算を実行します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3145">The binary operators `<<` and `>>` perform bit shifting operations.</span></span>

```antlr
ShiftOperatorExpression
    : Expression '<' '<' LineTerminator? Expression
    | Expression '>' '>' LineTerminator? Expression
    ;
```

<span data-ttu-id="8ad58-3146">これらの演算子は、@no__t 0、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、および @no__t 7 の各型に対して定義されています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3146">The operators are defined for the `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` and `Long` types.</span></span> <span data-ttu-id="8ad58-3147">他の二項演算子とは異なり、シフト演算の結果の型は、演算子が左側のオペランドだけを持つ単項演算子であったかのように決定されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3147">Unlike the other binary operators, the result type of a shift operation is determined as if the operator was a unary operator with just the left operand.</span></span> <span data-ttu-id="8ad58-3148">右オペランドの型は `Integer` に暗黙的に変換できる必要があり、操作の結果の型を決定するときには使用されません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3148">The type of the right operand must be implicitly convertible to `Integer` and is not used in determining the result type of the operation.</span></span>

<span data-ttu-id="8ad58-3149">@No__t-0 演算子を使用すると、最初のオペランドのビットがシフトの量によって指定された桁数から左にシフトされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3149">The `<<` operator causes the bits in the first operand to be shifted left the number of places specified by the shift amount.</span></span> <span data-ttu-id="8ad58-3150">結果型の範囲外の上位ビットは破棄され、下位空いているビット位置はゼロで埋められます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3150">The high-order bits outside the range of the result type are discarded and the low-order vacated bit positions are zero-filled.</span></span>

<span data-ttu-id="8ad58-3151">@No__t-0 演算子を使用すると、最初のオペランドのビットが、シフト量で指定された場所の数だけ右にシフトされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3151">The `>>` operator causes the bits in the first operand to be shifted right the number of places specified by the shift amount.</span></span> <span data-ttu-id="8ad58-3152">下位ビットは破棄され、左オペランドが正の場合は0に、負の場合は1に設定されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3152">The low-order bits are discarded and the high-order vacated bit positions are set to zero if the left operand is positive or to one if negative.</span></span> <span data-ttu-id="8ad58-3153">左オペランドの型が `Byte`、`UShort`、`UInteger`、または `ULong` の場合、空席の上位ビットはゼロで埋められます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3153">If the left operand is of type `Byte`, `UShort`, `UInteger`, or `ULong` the vacant high-order bits are zero-filled.</span></span>

<span data-ttu-id="8ad58-3154">シフト演算子は、最初のオペランドの基になる表現のビットを2番目のオペランドの量でシフトします。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3154">The shift operators shift the bits of the underlying representation of the first operand by the amount of the second operand.</span></span> <span data-ttu-id="8ad58-3155">2番目のオペランドの値が1番目のオペランドのビット数より大きい場合、または負の値の場合、シフトの量は @no__t 0 として計算されます。 `SizeMask` は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3155">If the value of the second operand is greater than the number of bits in the first operand, or is negative, then the shift amount is computed as `RightOperand And SizeMask` where `SizeMask` is:</span></span>

| <span data-ttu-id="8ad58-3156">__LeftOperand 型__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3156">__LeftOperand Type__</span></span>  | <span data-ttu-id="8ad58-3157">__SizeMask__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3157">__SizeMask__</span></span> | 
|-----------------------|--------------|
| <span data-ttu-id="8ad58-3158">`Byte`, `SByte`</span><span class="sxs-lookup"><span data-stu-id="8ad58-3158">`Byte`, `SByte`</span></span>       | <span data-ttu-id="8ad58-3159">7 (`&H7`)</span><span class="sxs-lookup"><span data-stu-id="8ad58-3159">7 (`&H7`)</span></span>    | 
| <span data-ttu-id="8ad58-3160">`UShort`, `Short`</span><span class="sxs-lookup"><span data-stu-id="8ad58-3160">`UShort`, `Short`</span></span>     | <span data-ttu-id="8ad58-3161">15 (`&HF`)</span><span class="sxs-lookup"><span data-stu-id="8ad58-3161">15 (`&HF`)</span></span>   | 
| <span data-ttu-id="8ad58-3162">`UInteger`, `Integer`</span><span class="sxs-lookup"><span data-stu-id="8ad58-3162">`UInteger`, `Integer`</span></span> | <span data-ttu-id="8ad58-3163">31 (`&H1F`)</span><span class="sxs-lookup"><span data-stu-id="8ad58-3163">31 (`&H1F`)</span></span>  | 
| <span data-ttu-id="8ad58-3164">`ULong`, `Long`</span><span class="sxs-lookup"><span data-stu-id="8ad58-3164">`ULong`, `Long`</span></span>       | <span data-ttu-id="8ad58-3165">63 (`&H3F`)</span><span class="sxs-lookup"><span data-stu-id="8ad58-3165">63 (`&H3F`)</span></span>  | 

<span data-ttu-id="8ad58-3166">シフトの量がゼロの場合、演算の結果は最初のオペランドの値と同じになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3166">If the shift amount is zero, the result of the operation is identical to the value of the first operand.</span></span> <span data-ttu-id="8ad58-3167">これらの操作によってオーバーフローが発生することはありません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3167">No overflows are possible from these operations.</span></span>

<span data-ttu-id="8ad58-3168">__操作の種類:__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3168">__Operation Type:__</span></span>


| <span data-ttu-id="8ad58-3169">__コンボ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3169">__Bo__</span></span> | <span data-ttu-id="8ad58-3170">__SB__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3170">__SB__</span></span> | <span data-ttu-id="8ad58-3171">__は__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3171">__By__</span></span> | <span data-ttu-id="8ad58-3172">__悪夢__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3172">__Sh__</span></span> | <span data-ttu-id="8ad58-3173">__米国__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3173">__US__</span></span> | <span data-ttu-id="8ad58-3174">__In__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3174">__In__</span></span> | <span data-ttu-id="8ad58-3175">__UI__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3175">__UI__</span></span> | <span data-ttu-id="8ad58-3176">__Lo__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3176">__Lo__</span></span> | <span data-ttu-id="8ad58-3177">__UL__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3177">__UL__</span></span> | <span data-ttu-id="8ad58-3178">__解放__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3178">__De__</span></span> | <span data-ttu-id="8ad58-3179">__Si__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3179">__Si__</span></span> | <span data-ttu-id="8ad58-3180">__Do__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3180">__Do__</span></span> | <span data-ttu-id="8ad58-3181">__オーディオ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3181">__Da__</span></span>  | <span data-ttu-id="8ad58-3182">__ハーフ__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3182">__Ch__</span></span>  | <span data-ttu-id="8ad58-3183">__&__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3183">__St__</span></span> | <span data-ttu-id="8ad58-3184">__Ob__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3184">__Ob__</span></span> | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="8ad58-3185">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-3185">Sh</span></span> | <span data-ttu-id="8ad58-3186">SB</span><span class="sxs-lookup"><span data-stu-id="8ad58-3186">SB</span></span> | <span data-ttu-id="8ad58-3187">By</span><span class="sxs-lookup"><span data-stu-id="8ad58-3187">By</span></span> | <span data-ttu-id="8ad58-3188">悪夢</span><span class="sxs-lookup"><span data-stu-id="8ad58-3188">Sh</span></span> | <span data-ttu-id="8ad58-3189">US</span><span class="sxs-lookup"><span data-stu-id="8ad58-3189">US</span></span> | <span data-ttu-id="8ad58-3190">In</span><span class="sxs-lookup"><span data-stu-id="8ad58-3190">In</span></span> | <span data-ttu-id="8ad58-3191">UI</span><span class="sxs-lookup"><span data-stu-id="8ad58-3191">UI</span></span> | <span data-ttu-id="8ad58-3192">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-3192">Lo</span></span> | <span data-ttu-id="8ad58-3193">UL</span><span class="sxs-lookup"><span data-stu-id="8ad58-3193">UL</span></span> | <span data-ttu-id="8ad58-3194">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-3194">Lo</span></span> | <span data-ttu-id="8ad58-3195">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-3195">Lo</span></span> | <span data-ttu-id="8ad58-3196">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-3196">Lo</span></span> | <span data-ttu-id="8ad58-3197">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3197">Err</span></span> | <span data-ttu-id="8ad58-3198">Err</span><span class="sxs-lookup"><span data-stu-id="8ad58-3198">Err</span></span> | <span data-ttu-id="8ad58-3199">Lo</span><span class="sxs-lookup"><span data-stu-id="8ad58-3199">Lo</span></span> | <span data-ttu-id="8ad58-3200">Ob</span><span class="sxs-lookup"><span data-stu-id="8ad58-3200">Ob</span></span> | 


## <a name="boolean-expressions"></a><span data-ttu-id="8ad58-3201">Boolean 式</span><span class="sxs-lookup"><span data-stu-id="8ad58-3201">Boolean Expressions</span></span>

<span data-ttu-id="8ad58-3202">ブール式は、true であるか false であるかを確認するためにテストできる式です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3202">A Boolean expression is an expression that can be tested to see if it is true or if it is false.</span></span>

```antlr
BooleanExpression
    : Expression
    ;
```

<span data-ttu-id="8ad58-3203">ブール式では、次のような優先順位で型 `T` を使用できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3203">A type `T` can be used in a Boolean expression if, in order of preference:</span></span>

* <span data-ttu-id="8ad58-3204">`T` が `Boolean` または `Boolean?` です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3204">`T` is `Boolean` or `Boolean?`</span></span>

* <span data-ttu-id="8ad58-3205">`T` は `Boolean` への拡大変換を行っています</span><span class="sxs-lookup"><span data-stu-id="8ad58-3205">`T` has a widening conversion to `Boolean`</span></span>

* <span data-ttu-id="8ad58-3206">`T` は `Boolean?` への拡大変換を行っています</span><span class="sxs-lookup"><span data-stu-id="8ad58-3206">`T` has a widening conversion to `Boolean?`</span></span>

* <span data-ttu-id="8ad58-3207">`T` は、2つの擬似演算子 (`IsTrue` と `IsFalse`) を定義します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3207">`T` defines two pseudo operators, `IsTrue` and `IsFalse`.</span></span>

* <span data-ttu-id="8ad58-3208">`T` には、`Boolean` から `Boolean?` への変換が関係しない `Boolean?` への縮小変換が含まれています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3208">`T` has a narrowing conversion to `Boolean?` that does not involve a conversion from `Boolean` to `Boolean?`.</span></span>

* <span data-ttu-id="8ad58-3209">`T` は `Boolean` に縮小変換します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3209">`T` has a narrowing conversion to `Boolean`.</span></span>

<span data-ttu-id="8ad58-3210">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3210">__Note.__</span></span> <span data-ttu-id="8ad58-3211">@No__t-0 がオフの場合、`Boolean` への縮小変換を含む式は、コンパイル時エラーが発生することなく受け入れられますが、言語では、存在する場合は `IsTrue` 演算子が優先されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3211">It is interesting to note that if `Option Strict` is off, an expression that has a narrowing conversion to `Boolean` will be accepted without a compile-time error but the language will still prefer an `IsTrue` operator if it exists.</span></span> <span data-ttu-id="8ad58-3212">これは、`Option Strict` は、その言語で受け入れられていないものだけを変更し、式の実際の意味を変更しないためです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3212">This is because `Option Strict` only changes what is and isn't accepted by the language, and never changes the actual meaning of an expression.</span></span> <span data-ttu-id="8ad58-3213">したがって、`Option Strict` に関係なく、`IsTrue` は常に縮小変換よりも優先される必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3213">Thus, `IsTrue` has to always be preferred over a narrowing conversion, regardless of `Option Strict`.</span></span>

<span data-ttu-id="8ad58-3214">たとえば、次のクラスは `Boolean` への拡大変換を定義していません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3214">For example, the following class does not define a widening conversion to `Boolean`.</span></span> <span data-ttu-id="8ad58-3215">その結果、`If` ステートメントで使用すると、`IsTrue` 演算子が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3215">As a result, its use in the `If` statement causes a call to the `IsTrue` operator.</span></span>

```vb
Class MyBool
    Public Shared Widening Operator CType(b As Boolean) As MyBool
        ...
    End Operator

    Public Shared Narrowing Operator CType(b As MyBool) As Boolean
        ...
    End Operator

    Public Shared Operator IsTrue(b As MyBool) As Boolean
        ...
    End Operator

    Public Shared Operator IsFalse(b As MyBool) As Boolean
        ...
    End Operator
End Class

Module Test
    Sub Main()
        Dim b As New MyBool

        If b Then Console.WriteLine("True")
    End Sub
End Module
```

<span data-ttu-id="8ad58-3216">ブール式がとして型指定された場合、または `Boolean` または `Boolean?` に変換された場合は、値が `True` および false の場合は true になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3216">If a Boolean expression is typed as or converted to `Boolean` or `Boolean?`, then it is true if the value is `True` and false otherwise.</span></span>

<span data-ttu-id="8ad58-3217">それ以外の場合、ブール式は `IsTrue` 演算子を呼び出し、演算子が `True`; を返した場合は `True` を返します。それ以外の場合は false になります (ただし、`IsFalse` 演算子は呼び出されません)。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3217">Otherwise, a Boolean expression calls the `IsTrue` operator and returns `True` if the operator returned `True`; otherwise it is false (but never calls the `IsFalse` operator).</span></span>

<span data-ttu-id="8ad58-3218">次の例では、`Integer` に `Boolean` への縮小変換が含まれているため、null `Integer?` では `Boolean?` (null @no__t) と `Boolean` (例外がスローされます) の両方に縮小変換されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3218">In the following example, `Integer` has a narrowing conversion to `Boolean`, so a null `Integer?` has a narrowing conversion to both `Boolean?` (yielding a null `Boolean`) and to `Boolean` (which throws an exception).</span></span> <span data-ttu-id="8ad58-3219">@No__t-0 への縮小変換が優先されるため、ブール式として "`i`" の値が `False` になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3219">The narrowing conversion to `Boolean?` is preferred, and so the value of "`i`" as a Boolean expression is therefore `False`.</span></span>

```vb
Dim i As Integer? = Nothing
If i Then Console.WriteLine()
```


## <a name="lambda-expressions"></a><span data-ttu-id="8ad58-3220">ラムダ式</span><span class="sxs-lookup"><span data-stu-id="8ad58-3220">Lambda Expressions</span></span>

<span data-ttu-id="8ad58-3221">*ラムダ式*は、*ラムダメソッド*と呼ばれる匿名メソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3221">A *lambda expression* defines an anonymous method called a *lambda method*.</span></span> <span data-ttu-id="8ad58-3222">ラムダメソッドを使用すると、デリゲート型を受け取る他のメソッドに "インライン" メソッドを簡単に渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3222">Lambda methods make it easy to pass "in-line" methods to other methods that take delegate types.</span></span>

```antlr
LambdaExpression
    : SingleLineLambda
    | MultiLineLambda
    ;

SingleLineLambda
    : LambdaModifier* 'Function' ( OpenParenthesis ParameterList? CloseParenthesis )? Expression
    | 'Sub' ( OpenParenthesis ParameterList? CloseParenthesis )? Statement
    ;

MultiLineLambda
    : MultiLineFunctionLambda
    | MultiLineSubLambda
    ;

MultiLineFunctionLambda
    : LambdaModifier? 'Function' ( OpenParenthesis ParameterList? CloseParenthesis )? ( 'As' TypeName )? LineTerminator
      Block
      'End' 'Function'
    ;

MultiLineSubLambda
    : LambdaModifier? 'Sub' ( OpenParenthesis ParameterList? CloseParenthesis )? LineTerminator
      Block
      'End' 'Sub'
    ;

LambdaModifier
    : 'Async' | 'Iterator'
    ;
```

<span data-ttu-id="8ad58-3223">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3223">The example:</span></span>

```vb
Module Test
    Delegate Function IntFunc(x As Integer) As Integer

    Sub Apply(a() As Integer, func As IntFunc)
        For index As Integer = 0 To a.Length - 1
            a(index) = func(a(index))
        Next index
    End Sub

    Sub Main()
        Dim a() As Integer = { 1, 2, 3, 4 }

        Apply(a, Function(x As Integer) x * 2)

        For Each value In a
            Console.Write(value & " ")
        Next value
    End Sub
End Module
```

<span data-ttu-id="8ad58-3224">次のように出力されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3224">will print out:</span></span>

```console
2 4 6 8
```

<span data-ttu-id="8ad58-3225">ラムダ式は、オプションの修飾子 `Async` または `Iterator` で始まり、その後にキーワード `Function` または `Sub` とパラメーターリストが続きます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3225">A lambda expression begins with the optional modifiers `Async` or `Iterator`, followed by the keyword `Function` or `Sub` and a parameter list.</span></span> <span data-ttu-id="8ad58-3226">ラムダ式のパラメーターを `Optional` または `ParamArray` に宣言することはできません。また、属性を指定することもできません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3226">Parameters in a lambda expression cannot be declared `Optional` or `ParamArray` and cannot have attributes.</span></span> <span data-ttu-id="8ad58-3227">通常のメソッドとは異なり、ラムダメソッドのパラメーター型を省略しても、`Object` は自動的に推論されません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3227">Unlike regular methods, omitting a parameter type for a lambda method does not automatically infer `Object`.</span></span> <span data-ttu-id="8ad58-3228">代わりに、ラムダメソッドが再分類されると、省略されたパラメーターの型と @no__t 0 の修飾子が対象の型から推論されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3228">Instead, when a lambda method is reclassified, the omitted parameter types and `ByRef` modifiers are inferred from the target type.</span></span> <span data-ttu-id="8ad58-3229">前の例では、ラムダ式は `Function(x) x * 2` として記述されている可能性があり、ラムダメソッドを使用して `IntFunc` デリゲート型のインスタンスを作成すると、`x` の型が `Integer` であると推論されました。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3229">In the previous example, the lambda expression could have been written as `Function(x) x * 2`, and it would have inferred the type of `x` to be `Integer` when the lambda method was used to create an instance of the `IntFunc` delegate type.</span></span> <span data-ttu-id="8ad58-3230">ローカル変数の推論とは異なり、ラムダメソッドのパラメーターが型を省略しても、配列または null 許容の名前修飾子がある場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3230">Unlike local variable inference, if a lambda method parameter omits a type but has an array or nullable name modifier, a compile-time error occurs.</span></span>

<span data-ttu-id="8ad58-3231">__通常のラムダ式__は、`Async` でも @no__t でもありません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3231">A __regular lambda expression__ is one with neither `Async` nor `Iterator` modifiers.</span></span>

<span data-ttu-id="8ad58-3232">__反復子ラムダ式__は、`Iterator` 修飾子を持ち、`Async` 修飾子を指定していません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3232">An __iterator lambda expression__ is one with the `Iterator` modifier and no `Async` modifier.</span></span> <span data-ttu-id="8ad58-3233">関数である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3233">It must be a function.</span></span> <span data-ttu-id="8ad58-3234">値に再分類されると、戻り値の型が @no__t 0、または `IEnumerable` であるデリゲート型の値にのみ再分類できます。また、一部の @no__t では `IEnumerator(Of T)` または `IEnumerable(Of T)` であり、ByRef パラメーターはありません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3234">When it is reclassified to a value, it can only be reclassified to a value of delegate type whose return type is `IEnumerator`, or `IEnumerable`, or `IEnumerator(Of T)` or `IEnumerable(Of T)` for some `T`, and which has no ByRef parameters.</span></span>

<span data-ttu-id="8ad58-3235">__非同期ラムダ式__は、`Async` 修飾子を持ち、`Iterator` 修飾子を指定していません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3235">An __async lambda expression__ is one with the `Async` modifier and no `Iterator` modifier.</span></span> <span data-ttu-id="8ad58-3236">非同期サブラムダは、ByRef パラメーターを持たないサブデリゲート型の値にのみ再分類できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3236">An async sub lambda may only be reclassified to a value of sub delegate type with no ByRef parameters.</span></span> <span data-ttu-id="8ad58-3237">非同期関数ラムダは、戻り値の型が @no__t 0 であるか、または `T` の `Task(Of T)` で、ByRef パラメーターを持たない関数デリゲート型の値にのみ再分類できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3237">An async function lambda may only be reclassified to a value of function delegate type whose return type is `Task` or `Task(Of T)` for some `T`, and which has no ByRef parameters.</span></span>

<span data-ttu-id="8ad58-3238">ラムダ式は、単一行または複数行にすることができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3238">Lambda expressions can either be single-line or multi-line.</span></span> <span data-ttu-id="8ad58-3239">単一行 `Function` ラムダ式には、ラムダメソッドから返された値を表す単一の式が含まれます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3239">Single-line `Function` lambda expressions contain a single expression that represents the value returned from the lambda method.</span></span> <span data-ttu-id="8ad58-3240">単一行 `Sub` ラムダ式には、1つのステートメントが含まれています。その場合、`StatementTerminator` は終わりません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3240">Single-line `Sub` lambda expressions contain a single statement without its closing `StatementTerminator`.</span></span> <span data-ttu-id="8ad58-3241">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3241">For example:</span></span>

```vb
Module Test
    Sub Do(a() As Integer, action As Action(Of Integer))
        For index As Integer = 0 To a.Length - 1
            action(a(index))
        Next index
    End Sub

    Sub Main()
        Dim a() As Integer = { 1, 2, 3, 4 }

        Do(a, Sub(x As Integer) Console.WriteLine(x))
    End Sub
End Module
```

<span data-ttu-id="8ad58-3242">単一行のラムダ構造は、他のすべての式およびステートメントよりも密にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3242">Single-line lambda constructs bind less tightly than all other expressions and statements.</span></span> <span data-ttu-id="8ad58-3243">したがって、たとえば、"`Function() x + 5`" は、"`(Function() x) + 5`" ではなく "`Function() (x+5)"`" に相当します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3243">Thus, for example, "`Function() x + 5`" is equivalent to "`Function() (x+5)"` rather than "`(Function() x) + 5`".</span></span> <span data-ttu-id="8ad58-3244">あいまいさを避けるために、単一行の `Sub` ラムダ式には、Dim ステートメントまたはラベル宣言ステートメントを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3244">To avoid ambiguity, a single-line `Sub` lambda expression may not contain a Dim statement or a label declaration statement.</span></span> <span data-ttu-id="8ad58-3245">また、かっこで囲まれていない限り、単一行の `Sub` ラムダ式は、直後にコロン ":"、メンバーアクセス演算子 "."、ディクショナリメンバーアクセス演算子 "!"、または始めかっこ "(" を指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3245">Also, unless it is enclosed in parentheses, a single-line `Sub` lambda expression may not be immediately followed by a colon ":", a member access operator ".", a dictionary member access operator "!" or an open parenthesis "(".</span></span> <span data-ttu-id="8ad58-3246">これには、block ステートメント (`With`、`SyncLock, If...EndIf`、`While`、`For`、`Do`、`Using`) と `OnError` は含まれません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3246">It may not contain any block statement (`With`, `SyncLock, If...EndIf`, `While`, `For`, `Do`, `Using`) nor `OnError` nor `Resume`.</span></span>

<span data-ttu-id="8ad58-3247">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3247">__Note.__</span></span> <span data-ttu-id="8ad58-3248">ラムダ式 `Function(i) x=i` の場合、本体は*式*として解釈されます (`x` と @no__t が等しいかどうかをテストします)。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3248">In the lambda expression `Function(i) x=i`, the body is interpreted as an *expression* (which tests whether `x` and `i` are equal).</span></span> <span data-ttu-id="8ad58-3249">ただし、ラムダ式 `Sub(i) x=i` の場合、本文はステートメントとして解釈されます (`i` は `x` に割り当てられます)。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3249">But in the lambda expression `Sub(i) x=i`, the body is interpreted as a statement (which assigns `i` to `x`).</span></span>

<span data-ttu-id="8ad58-3250">複数行のラムダ式には、ステートメントブロックが含まれており、適切な @no__t 0 ステートメント (つまり `End Function` または `End Sub`) で終わる必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3250">A multi-line lambda expression contains a statement block and must end with an appropriate `End` statement (i.e. `End Function` or `End Sub`).</span></span> <span data-ttu-id="8ad58-3251">通常のメソッドと同様に、複数行のラムダメソッドの `Function` または @no__t 1 つのステートメントと @no__t 2 つのステートメントをそれぞれの行に記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3251">As with regular methods, a multi-line lambda method's `Function` or `Sub` statement and `End` statements must be on their own lines.</span></span> <span data-ttu-id="8ad58-3252">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3252">For example:</span></span>

```vb
' Error: Function statement must be on its own line!
Dim x = Sub(x As Integer) : Console.WriteLine(x) : End Sub

' OK
Dim y = Sub(x As Integer)
               Console.WriteLine(x)
          End Sub
```

<span data-ttu-id="8ad58-3253">複数行の `Function` ラムダ式は戻り値の型を宣言できますが、属性を配置することはできません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3253">Multi-line `Function` lambda expressions can declare a return type but cannot put attributes on it.</span></span> <span data-ttu-id="8ad58-3254">複数行の `Function` ラムダ式で戻り値の型が宣言されていないが、戻り値の型をラムダ式が使用されているコンテキストから推論できる場合、その戻り値の型が使用されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3254">If a multi-line `Function` lambda expression does not declare a return type but the return type can be inferred from the context in which the lambda expression is used , then that return type is used.</span></span> <span data-ttu-id="8ad58-3255">それ以外の場合、関数の戻り値の型は次のように計算されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3255">Otherwise the return type of the function is calculated as follows:</span></span>

* <span data-ttu-id="8ad58-3256">通常のラムダ式では、戻り値の型は、ステートメントブロック内のすべての `Return` ステートメント内の式の中で最も優先的な型になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3256">In a regular lambda expression, the return type is the dominant type of the expressions in all the `Return` statements in the statement block.</span></span>

* <span data-ttu-id="8ad58-3257">非同期ラムダ式では、戻り値の型は 0 @no__t。ここで `T` は、ステートメントブロック内のすべての `Return` ステートメント内の式の中で最も優先される型です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3257">In an async lambda expression, the return type is `Task(Of T)` where `T` is the dominant type of the expressions in all the `Return` statements in the statement block.</span></span>

* <span data-ttu-id="8ad58-3258">反復子ラムダ式では、戻り値の型は 0 @no__t。ここで `T` は、ステートメントブロック内のすべての `Yield` ステートメント内の式の中で最も優先される型です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3258">In an iterator lambda expression, the return type is `IEnumerable(Of T)` where `T` is the dominant type of the expressions in all the `Yield` statements in the statement block.</span></span>

<span data-ttu-id="8ad58-3259">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3259">For example:</span></span>

```vb
Function f(min As Integer, max As Integer) As IEnumerable(Of Integer)
    If min > max Then Throw New ArgumentException()
    Dim x = Iterator Function()
                  For i = min To max
                    Yield i
                Next
               End Function

    ' infers x to be a delegate with return type IEnumerable(Of Integer)
    Return x()
End Function
```

<span data-ttu-id="8ad58-3260">どのような場合でも、`Return` (それぞれ `Yield`) ステートメントが存在しない場合、または優先度の高い型が存在せず、厳密なセマンティクスが使用されている場合は、コンパイル時エラーが発生します。それ以外の場合、優先される型は暗黙的に `Object` です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3260">In all cases, if there are no `Return` (respectively `Yield`) statements, or if there is no dominant type among them, and strict semantics are being used, a compile-time error occurs; otherwise the dominant type is implicitly `Object`.</span></span>

<span data-ttu-id="8ad58-3261">戻り値の型は、到達できない場合でも、すべての `Return` ステートメントから計算されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3261">Note that the return type is calculated from all `Return` statements, even if they are not reachable.</span></span> <span data-ttu-id="8ad58-3262">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3262">For example:</span></span>

```vb
' Return type is Double
Dim x = Function()
              Return 10
               Return 10.50
          End Function
```

<span data-ttu-id="8ad58-3263">変数の名前が存在しないため、暗黙的な戻り値の変数はありません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3263">There is no implicit return variable, as there is no name for the variable.</span></span>

<span data-ttu-id="8ad58-3264">複数行のラムダ式内のステートメントブロックには、次の制限があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3264">The statement blocks inside multi-line lambda expressions have the following restrictions:</span></span>

* <span data-ttu-id="8ad58-3265">`On Error` および `Resume` ステートメントは許可されていませんが、`Try` ステートメントは許可されています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3265">`On Error` and `Resume` statements are not allowed, although `Try` statements are allowed.</span></span>

* <span data-ttu-id="8ad58-3266">複数行のラムダ式では、静的ローカル変数を宣言できません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3266">Static locals cannot be declared in multi-line lambda expressions.</span></span>

* <span data-ttu-id="8ad58-3267">複数行のラムダ式のステートメントブロックに分岐することはできませんが、通常の分岐規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3267">It is not possible to branch into or out of the statement block of a multi-line lambda expression, although the normal branching rules apply within it.</span></span> <span data-ttu-id="8ad58-3268">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3268">For example:</span></span>

  ```vb
  Label1:
  Dim x = Sub()
                 ' Error: Cannot branch out
                 GoTo Label1

                 ' OK: Wholly within the lamba.
                 GoTo Label2:
            Label2:
            End Sub

  ' Error: Cannot branch in
  GoTo Label2
  ```

<span data-ttu-id="8ad58-3269">ラムダ式は、含んでいる型で宣言された匿名メソッドとほぼ同じです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3269">A lambda expression is roughly equivalent to an anonymous method declared on the containing type.</span></span> <span data-ttu-id="8ad58-3270">最初の例は次のようにほぼ同じです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3270">The initial example is roughly equivalent to:</span></span>

```vb
Module Test
    Delegate Function IntFunc(x As Integer) As Integer

    Sub Apply(a() As Integer, func As IntFunc)
        For index As Integer = 0 To a.Length - 1
            a(index) = func(a(index))
        Next index
    End Sub

    Function $Lambda1(x As Integer) As Integer
        Return x * 2
    End Function

    Sub Main()
        Dim a() As Integer = { 1, 2, 3, 4 }

        Apply(a, AddressOf $Lambda1)

        For Each value In a
            Console.Write(value & " ")
        Next value
    End Sub
End Module
```


### <a name="closures"></a><span data-ttu-id="8ad58-3271">休業</span><span class="sxs-lookup"><span data-stu-id="8ad58-3271">Closures</span></span>

<span data-ttu-id="8ad58-3272">ラムダ式は、スコープ内のすべての変数にアクセスできます。これには、外側のメソッドとラムダ式で定義されているローカル変数やパラメーターも含まれます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3272">Lambda expressions have access to all of the variables in scope, including local variables or parameters defined in the containing method and lambda expressions.</span></span> <span data-ttu-id="8ad58-3273">ラムダ式がローカル変数またはパラメーターを参照する場合、ラムダ式は、参照先の変数をクロージャにキャプチャします。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3273">When a lambda expression refers to a local variable or parameter, the lambda expression captures the variable being referred to into a closure.</span></span> <span data-ttu-id="8ad58-3274">クロージャは、スタックではなくヒープ上に存在し、変数がキャプチャされると、その変数へのすべての参照がクロージャにリダイレクトされるオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3274">A closure is an object that lives on the heap instead of on the stack, and when a variable is captured, all references to the variable are redirected to the closure.</span></span> <span data-ttu-id="8ad58-3275">これにより、ラムダ式は、外側のメソッドが完了した後でも、ローカル変数とパラメーターを引き続き参照できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3275">This enables lambda expressions to continue to refer to local variables and parameters even after the containing method is complete.</span></span> <span data-ttu-id="8ad58-3276">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3276">For example:</span></span>

```vb
Module Test
    Delegate Function D() As Integer

    Function M() As D
        Dim x As Integer = 10
        Return Function() x
    End Function

    Sub Main()
        Dim y As D = M()

        ' Prints 10
        Console.WriteLine(y())
    End Sub
End Module
```

<span data-ttu-id="8ad58-3277">は、次の場合とほぼ同じです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3277">is roughly equivalent to:</span></span>

```vb
Module Test
    Delegate Function D() As Integer

    Class $Closure1
        Public x As Integer

        Function $Lambda1() As Integer
            Return x
        End Function
    End Class

    Function M() As D
        Dim c As New $Closure1()
        c.x = 10
        Return AddressOf c.$Lambda1
    End Function

    Sub Main()
        Dim y As D = M()

        ' Prints 10
        Console.WriteLine(y())
    End Sub
End Module
```

<span data-ttu-id="8ad58-3278">クロージャは、ローカル変数が宣言されているブロックに入るたびにローカル変数の新しいコピーをキャプチャしますが、新しいコピーは、前のコピーの値 (存在する場合) を使用して初期化されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3278">A closure captures a new copy of a local variable each time it enters the block in which the local variable is declared, but the new copy is initialized with the value of the previous copy, if any.</span></span> <span data-ttu-id="8ad58-3279">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3279">For example:</span></span>

```vb
Module Test
    Delegate Function D() As Integer

    Function M() As D()
        Dim a(9) As D

        For i As Integer = 0 To 9
            Dim x
            a(i) = Function() x
            x += 1
        Next i

        Return a
    End Function

    Sub Main()
        Dim y() As D = M()

        For i As Integer = 0 To 9
            Console.Write(y(i)() & " ")
        Next i
    End Sub
End Module
```

<span data-ttu-id="8ad58-3280">出力</span><span class="sxs-lookup"><span data-stu-id="8ad58-3280">prints</span></span>

```console
1 2 3 4 5 6 7 8 9 10
```

<span data-ttu-id="8ad58-3281">代わりに</span><span class="sxs-lookup"><span data-stu-id="8ad58-3281">instead of</span></span>

```console
9 9 9 9 9 9 9 9 9 9
```

<span data-ttu-id="8ad58-3282">クロージャはブロックを入力するときに初期化する必要があるため、そのブロックの外部からクロージャを持つブロックには0を @no__t ことはできませんが、クロージャを持つブロックには-1 を @no__t ことができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3282">Because closures have to be initialized when entering a block, it is not allowed to `GoTo` into a block with a closure from outside of that block, although it is allowed to `Resume` into a block with a closure.</span></span> <span data-ttu-id="8ad58-3283">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3283">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim a = 10

        If a = 10 Then
L1:
            Dim x = Function() a

            ' Valid, source is within block
            GoTo L2
L2:
        End If

        ' ERROR: target is inside block with closure
        GoTo L1
    End Sub
End Module
```

<span data-ttu-id="8ad58-3284">クロージャにキャプチャすることはできないため、ラムダ式の内部に次の文字列を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3284">Because they cannot be captured into a closure, the following cannot appear inside of a lambda expression:</span></span>

* <span data-ttu-id="8ad58-3285">参照パラメーター。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3285">Reference parameters.</span></span>

* <span data-ttu-id="8ad58-3286">@No__t-3 の型がクラスでない場合は、インスタンス式 (`Me`、`MyClass`、`MyBase`)。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3286">Instance expressions (`Me`, `MyClass`, `MyBase`), if the type of `Me` is not a class.</span></span>

<span data-ttu-id="8ad58-3287">ラムダ式が式の一部である場合は、匿名型作成式のメンバー。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3287">The members of an anonymous type-creation expression, if the lambda expression is part of the expression.</span></span> <span data-ttu-id="8ad58-3288">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3288">For example:</span></span>

```vb
' Error: Lambda cannot refer to anonymous type field
Dim x = New With { .a = 12, .b = Function() .a }
```

<span data-ttu-id="8ad58-3289">インスタンスコンストラクター内のインスタンス変数、または値のないコンテキストで変数が使用されている共有コンストラクター内の @no__t 共有変数の @no__t 0 です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3289">`ReadOnly` instance variables in instance constructors or `ReadOnly` shared variables in shared constructors where the variables are used in a non-value context.</span></span> <span data-ttu-id="8ad58-3290">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3290">For example:</span></span>

```vb
Class C1
    ReadOnly F1 As Integer

    Sub New()
        ' Valid, doesn't modify F1
        Dim x = Function() F1

        ' Error, tries to modify F1
        Dim f = Function() ModifyValue(F1)
    End Sub

    Sub ModifyValue(ByRef x As Integer)
    End Sub
End Class
```

## <a name="query-expressions"></a><span data-ttu-id="8ad58-3291">クエリ式</span><span class="sxs-lookup"><span data-stu-id="8ad58-3291">Query Expressions</span></span>

<span data-ttu-id="8ad58-3292">*クエリ式*は、クエリ可能*なコレクションの*要素に一連の*クエリ演算子*を適用する式です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3292">A *query expression* is an expression that applies a series of *query operators* to the elements of a *queryable* collection.</span></span> <span data-ttu-id="8ad58-3293">たとえば、次の式では、@no__t 0 のオブジェクトのコレクションを受け取り、ワシントン州のすべての顧客の名前を返します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3293">For example, the following expression takes a collection of `Customer` objects and returns the names of all the customers in the state of Washington:</span></span>

```vb
Dim names = _
    From cust In Customers _
    Where cust.State = "WA" _
    Select cust.Name
```

<span data-ttu-id="8ad58-3294">クエリ式は、@no__t 0 または `Aggregate` 演算子で始まり、任意のクエリ演算子で終わることができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3294">A query expression must start with a `From` or an `Aggregate` operator and can end with any query operator.</span></span> <span data-ttu-id="8ad58-3295">クエリ式の結果は、値として分類されます。式の結果型は、式の最後のクエリ演算子の結果の型によって異なります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3295">The result of a query expression is classified as a value; the result type of the expression depends on the result type of the last query operator in the expression.</span></span>

```antlr
QueryExpression
    : FromOrAggregateQueryOperator QueryOperator*
    ;

FromOrAggregateQueryOperator
    : FromQueryOperator
    | AggregateQueryOperator
    ;

QueryOperator
    : FromQueryOperator
    | AggregateQueryOperator
    | SelectQueryOperator
    | DistinctQueryOperator
    | WhereQueryOperator
    | OrderByQueryOperator
    | PartitionQueryOperator
    | LetQueryOperator
    | GroupByQueryOperator
    | JoinOrGroupJoinQueryOperator
    ;

JoinOrGroupJoinQueryOperator
    : JoinQueryOperator
    | GroupJoinQueryOperator
    ;
```

### <a name="range-variables"></a><span data-ttu-id="8ad58-3296">範囲変数</span><span class="sxs-lookup"><span data-stu-id="8ad58-3296">Range Variables</span></span>

<span data-ttu-id="8ad58-3297">一部のクエリ演算子は、*範囲変数*と呼ばれる特殊な種類の変数を導入します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3297">Some query operators introduce a special kind of variable called a *range variable*.</span></span> <span data-ttu-id="8ad58-3298">範囲変数は実際の変数ではありません。代わりに、入力コレクションに対するクエリの評価時に個々の値を表します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3298">Range variables are not real variables; instead, they represent the individual values during the evaluation of the query over the input collections.</span></span>

```antlr
CollectionRangeVariableDeclarationList
    : CollectionRangeVariableDeclaration ( Comma CollectionRangeVariableDeclaration )*
    ;

CollectionRangeVariableDeclaration
    : Identifier ( 'As' TypeName )? 'In' LineTerminator? Expression
    ;

ExpressionRangeVariableDeclarationList
    : ExpressionRangeVariableDeclaration ( Comma ExpressionRangeVariableDeclaration )*
    ;

ExpressionRangeVariableDeclaration
    : Identifier ( 'As' TypeName )? Equals Expression
    ;
```

<span data-ttu-id="8ad58-3299">範囲変数は、クエリ演算子の導入からクエリ式の末尾まで、またはクエリ演算子 (非表示にする `Select` など) にスコープが設定されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3299">Range variables are scoped from the introducing query operator to the end of a query expression, or to a query operator such as `Select` that hides them.</span></span> <span data-ttu-id="8ad58-3300">たとえば、次のクエリでは、</span><span class="sxs-lookup"><span data-stu-id="8ad58-3300">For example, in the following query</span></span>

```vb
Dim waCusts = _
    From cust As Customer In Customers _
    Where cust.State = "WA"
```

<span data-ttu-id="8ad58-3301">`From` クエリ演算子では、`Customers` コレクション内の各顧客を表す `Customer` として指定された範囲変数 `cust` を導入します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3301">the `From` query operator introduces a range variable `cust` typed as `Customer` that represents each customer in the `Customers` collection.</span></span> <span data-ttu-id="8ad58-3302">次の `Where` クエリ演算子は、フィルター式の範囲変数 `cust` を参照して、結果として得られるコレクションから個々の顧客をフィルター処理するかどうかを決定します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3302">The following `Where` query operator then refers to the range variable `cust` in the filter expression to determine whether to filter an individual customer out of the resulting collection.</span></span>

<span data-ttu-id="8ad58-3303">範囲変数には、*コレクション範囲*変数と*式範囲変数*の2種類があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3303">There are two types of range variables: *collection range variables* and *expression range variables*.</span></span> <span data-ttu-id="8ad58-3304">コレクション範囲変数は、クエリ対象のコレクションの要素から値を取得します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3304">Collection range variables take their values from the elements of the collections being queried.</span></span> <span data-ttu-id="8ad58-3305">コレクション範囲変数宣言内のコレクション式は、型がクエリ可能である値として分類する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3305">The collection expression in a collection range variable declaration must be classified as a value whose type is queryable.</span></span> <span data-ttu-id="8ad58-3306">コレクション範囲変数の型が省略されている場合は、コレクション式の要素型であると推論されます。または、コレクション式に要素型がない場合は `Object` (つまり、`Cast` のメソッドのみを定義します) を指定します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3306">If the type of a collection range variable is omitted, it is inferred to be the element type of the collection expression, or `Object` if the collection expression does not have an element type (i.e. only defines a `Cast` method).</span></span> <span data-ttu-id="8ad58-3307">コレクション式がクエリ可能でない場合 (つまり、コレクションの要素型を推論できない場合)、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3307">If the collection expression is not queryable (i.e. the element type of the collection cannot be inferred), a compile-time error results.</span></span>

<span data-ttu-id="8ad58-3308">式の範囲変数は、コレクションではなく、式によって計算される値を持つ範囲変数です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3308">An expression range variable is a range variable whose value is calculated by an expression rather than a collection.</span></span> <span data-ttu-id="8ad58-3309">次の例では、`Select` クエリ演算子によって、2つのフィールドから計算された `cityState` という式の範囲変数が導入されています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3309">In the following example, the `Select` query operator introduces an expression range variable named `cityState` calculated from two fields:</span></span>

```vb
Dim cityStates = _
    From cust As Customer In Customers _
    Select cityState = cust.City & "," & cust.State _
    Where cityState.Length() < 10
```

<span data-ttu-id="8ad58-3310">式の範囲変数は、別の範囲変数を参照するためには必要ありませんが、このような変数はあいまい値にすることができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3310">An expression range variable is not required to reference another range variable, although such a variable may be of dubious value.</span></span> <span data-ttu-id="8ad58-3311">式の範囲変数に割り当てられた式は、値として分類される必要があります。また、指定されている場合は、範囲変数の型に暗黙的に変換可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3311">The expression assigned to an expression range variable must be classified as a value and must be implicitly convertible to the type of the range variable, if given.</span></span>

<span data-ttu-id="8ad58-3312">Let 演算子内でのみ、式の範囲変数の型を指定できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3312">Only in a Let operator may an expression range variable have its type specified.</span></span> <span data-ttu-id="8ad58-3313">他の演算子では、または型が指定されていない場合は、範囲変数の型を決定するためにローカル変数型の推定が使用されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3313">In other operators, or if its type is not specified, then local variable type inference is used to determine the type of the range variable.</span></span>

<span data-ttu-id="8ad58-3314">範囲変数は、シャドウ処理に関してローカル変数を宣言するための規則に従う必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3314">A range variable must follow the rules for declaring local variables in respect to shadowing.</span></span> <span data-ttu-id="8ad58-3315">したがって、範囲変数は、外側のメソッドまたは別の範囲変数のローカル変数またはパラメーターの名前を非表示にすることはできません (クエリ演算子によってスコープ内の現在の範囲変数がすべて非表示にされている場合を除きます)。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3315">Thus, a range variable cannot hide the name of a local variable or parameter in the enclosing method or another range variable (unless the query operator specifically hides all current range variables in scope).</span></span>


### <a name="queryable-types"></a><span data-ttu-id="8ad58-3316">クエリ可能な型</span><span class="sxs-lookup"><span data-stu-id="8ad58-3316">Queryable Types</span></span>

<span data-ttu-id="8ad58-3317">クエリ式は、式をコレクション型の既知のメソッドの呼び出しに変換することによって実装されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3317">Query expressions are implemented by translating the expression into calls to well-known methods on a collection type.</span></span> <span data-ttu-id="8ad58-3318">これらの適切に定義されたメソッドは、クエリ可能なコレクションの要素型と、コレクションに対して実行されるクエリ演算子の結果の型を定義します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3318">These well-defined methods define the element type of the queryable collection as well as the result types of query operators executed on the collection.</span></span> <span data-ttu-id="8ad58-3319">各クエリ演算子は、クエリ演算子が一般的に変換されるメソッドを指定します。ただし、特定の変換は実装に依存します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3319">Each query operator specifies the method or methods that the query operator is generally translated into, although the specific translation is implementation dependent.</span></span> <span data-ttu-id="8ad58-3320">メソッドは、次のような一般的な形式を使用して仕様で指定されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3320">The methods are given in the specification using a general format that looks like:</span></span>

```vb
Function Select(selector As Func(Of T, R)) As CR
```

<span data-ttu-id="8ad58-3321">メソッドには次のものが適用されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3321">The following applies to the methods:</span></span>

* <span data-ttu-id="8ad58-3322">メソッドは、コレクション型のインスタンスまたは拡張メンバーであり、アクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3322">The method must be an instance or extension member of the collection type and must be accessible.</span></span>

* <span data-ttu-id="8ad58-3323">メソッドは、すべての型引数を推論できる場合に、ジェネリックである可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3323">The method may be generic, provided that is possible to infer all type arguments.</span></span>

* <span data-ttu-id="8ad58-3324">メソッドはオーバーロードされる場合があります。この場合、オーバーロードの解決を使用して、使用するメソッドを正確に決定します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3324">The method may be overloaded, in which case overload resolution is used to determine the exactly method to use.</span></span>

* <span data-ttu-id="8ad58-3325">同じシグネチャ (戻り値の型を含む) が一致する `Func` 型として指定されている場合、デリゲート `Func` 型の代わりに別のデリゲート型を使用できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3325">Another delegate type may be used in place of the delegate `Func` type, provided that it has the same signature, including return type, as the matching `Func` type.</span></span>

* <span data-ttu-id="8ad58-3326">@No__t-2 は、対応する `Func` 型と同じシグネチャを持つデリゲート型 (戻り値の型を含む) である場合、@no__t デリゲートの代わりに `System.Linq.Expressions.Expression(Of D)` 型を使用できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3326">The type `System.Linq.Expressions.Expression(Of D)` may be used in place of the delegate `Func` type, provided that `D` is a delegate type that has the same signature, including return type, as the matching `Func` type.</span></span>

* <span data-ttu-id="8ad58-3327">型 `T` は入力コレクションの要素型を表します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3327">The type `T` represents the element type of the input collection.</span></span> <span data-ttu-id="8ad58-3328">コレクション型によって定義されるすべてのメソッドは、コレクション型がクエリ可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3328">All of the methods defined by a collection type must have the same input element type for the collection type to be queryable.</span></span>

* <span data-ttu-id="8ad58-3329">型 `S` は、結合を実行するクエリ演算子の場合の2番目の入力コレクションの要素の型を表します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3329">The type `S` represents the element type of the second input collection in the case of query operators that perform joins.</span></span>

* <span data-ttu-id="8ad58-3330">型 `K` は、キーとして機能する一連の範囲変数を持つクエリ演算子の場合、キーの種類を表します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3330">The type `K` represents a key type in the case of query operators that have a set of range variables that act as keys.</span></span>

* <span data-ttu-id="8ad58-3331">型 `N` は、数値型として使用される型を表します (ただし、これはユーザー定義型であり、組み込みの数値型ではない場合もあります)。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3331">The type `N` represents a type that is used as a numeric type (although it could still be a user-defined type and not an intrinsic numeric type).</span></span>

* <span data-ttu-id="8ad58-3332">@No__t-0 型は、ブール式で使用できる型を表します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3332">The type `B` represents a type that can be used in a Boolean expression.</span></span>

* <span data-ttu-id="8ad58-3333">クエリ演算子によって結果コレクションが生成される場合、型 `R` は結果コレクションの要素型を表します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3333">The type `R` represents the element type of the result collection, if the query operator produces a result collection.</span></span> <span data-ttu-id="8ad58-3334">`R` は、クエリ演算子の最後にスコープ内の範囲変数の数に依存します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3334">`R` depends on the number of range variables in scope at the conclusion of the query operator.</span></span> <span data-ttu-id="8ad58-3335">1つの範囲変数がスコープ内にある場合は、`R` がその範囲変数の型になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3335">If a single range variable is in scope, then `R` is the type of that range variable.</span></span> <span data-ttu-id="8ad58-3336">この例では、</span><span class="sxs-lookup"><span data-stu-id="8ad58-3336">In the example</span></span>

  ```vb
  Dim custNames = From c In Customers
                  Select c.Name
  ```

  <span data-ttu-id="8ad58-3337">クエリの結果は、要素の型が `String` のコレクション型になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3337">the result of the query will be a collection type with an element type of `String`.</span></span> <span data-ttu-id="8ad58-3338">複数の範囲変数がスコープ内にある場合、`R` は、スコープ内のすべての範囲変数を `Key` フィールドとして含む匿名型になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3338">If multiple range variables are in scope, then `R` is an anonymous type that contains all of the range variables in scope as `Key` fields.</span></span> <span data-ttu-id="8ad58-3339">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3339">In the example:</span></span>

  ```vb
  Dim custNames = From c In Customers, o In c.Orders 
                  Select Name = c.Name, ProductName = o.ProductName
  ```

  <span data-ttu-id="8ad58-3340">クエリの結果は、匿名型の要素型を持つコレクション型であり、型 `String` の `Name` という名前の読み取り専用プロパティと、型 `String` の `ProductName` という名前の読み取り専用プロパティが設定されています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3340">the result of the query will be a collection type with an element type of an anonymous type with a read-only property named `Name` of type `String` and a read-only property named `ProductName` of type `String`.</span></span>

  <span data-ttu-id="8ad58-3341">クエリ式内では、範囲変数を含むように生成された匿名型は*透過的*であり、範囲変数は常に修飾なしで使用できることを意味します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3341">Within a query expression, anonymous types generated to contain range variables are *transparent*, which means that range variables are always available without qualification.</span></span> <span data-ttu-id="8ad58-3342">たとえば、前の例では、入力コレクションの要素の型が匿名型であったとしても、範囲変数 `c` および `o` は、`Select` クエリ演算子で修飾なしでアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3342">For example, in the previous example the range variables `c` and `o` could be accessed without qualification in the `Select` query operator, even though the input collection's element type was an anonymous type.</span></span>

* <span data-ttu-id="8ad58-3343">型 `CX` はコレクション型を表します。必ずしも入力コレクション型ではなく、要素型が-1 @no__t 型であることを示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3343">The type `CX` represents a collection type, not necessarily the input collection type, whose element type is some type `X`.</span></span>

<span data-ttu-id="8ad58-3344">クエリ可能なコレクション型は、次のいずれかの条件を優先順に満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3344">A queryable collection type must satisfy one of the following conditions, in order of preference:</span></span>

* <span data-ttu-id="8ad58-3345">@No__t-0 に準拠したメソッドを定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3345">It must define a conforming `Select` method.</span></span>

* <span data-ttu-id="8ad58-3346">次のいずれかのメソッドが含まれている必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3346">It must have have one of the following methods</span></span>

  ```vb
  Function AsEnumerable() As CT
  Function AsQueryable() As CT
  ```

  <span data-ttu-id="8ad58-3347">クエリ可能なコレクションを取得するために呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3347">which can be called to obtain a queryable collection.</span></span> <span data-ttu-id="8ad58-3348">両方の方法が指定されている場合は、`AsQueryable` が `AsEnumerable` よりも優先されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3348">If both methods are provided, `AsQueryable` is preferred over `AsEnumerable`.</span></span>

* <span data-ttu-id="8ad58-3349">メソッドが必要です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3349">It must have a method</span></span>

  ```vb
  Function Cast(Of T)() As CT
  ```

  <span data-ttu-id="8ad58-3350">クエリ可能なコレクションを生成するために、範囲変数の型を使用して呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3350">which can be called with the type of the range variable to produce a queryable collection.</span></span>

<span data-ttu-id="8ad58-3351">コレクションの要素の型は、実際のメソッドの呼び出しとは独立して判断されるため、特定のメソッドの適用性を判断することはできません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3351">Because determining the element type of a collection occurs independently of an actual method invocation, the applicability of specific methods cannot be determined.</span></span> <span data-ttu-id="8ad58-3352">したがって、既知のメソッドに一致するインスタンスメソッドがある場合、コレクションの要素の型を決定すると、既知のメソッドに一致する拡張メソッドは無視されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3352">Thus, when determining the element type of a collection if there are instance methods that match well-known methods, then any extension methods that match well-known methods are ignored.</span></span>

<span data-ttu-id="8ad58-3353">クエリ演算子の変換は、クエリ演算子が式で使用されている順序で行われます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3353">Query operator translation occurs in the order in which the query operators occur in the expression.</span></span> <span data-ttu-id="8ad58-3354">コレクションオブジェクトは、すべてのクエリ演算子で必要なすべてのメソッドを実装する必要はありませんが、すべてのコレクションオブジェクトが少なくとも `Select` クエリ演算子をサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3354">It is not necessary for a collection object to implement all of the methods needed by all the query operators, although every collection object must at least support the `Select` query operator.</span></span> <span data-ttu-id="8ad58-3355">必要なメソッドが存在しない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3355">If a needed method is not present, a compile-time error occurs.</span></span> <span data-ttu-id="8ad58-3356">よく知られているメソッド名をバインドする場合、非メソッドは、インターフェイスおよび拡張メソッドのバインディングでの多重継承のために無視されます。ただし、シャドウセマンティクスは引き続き適用されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3356">When binding well-known method names, non-methods are ignored for the purpose of multiple inheritance in interfaces and extension method binding, although shadowing semantics still apply.</span></span> <span data-ttu-id="8ad58-3357">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3357">For example:</span></span>

```vb
Class Q1
    Public Function [Select](selector As Func(Of Integer, Integer)) As Q1
    End Function
End Class

Class Q2
    Inherits Q1

    Public [Select] As Integer
End Class

Module Test
    Sub Main()
        Dim qs As New Q2()

        ' Error: Q2.Select still hides Q1.Select
        Dim zs = From q In qs Select q
    End Sub
End Module
```

### <a name="default-query-indexer"></a><span data-ttu-id="8ad58-3358">既定のクエリインデクサー</span><span class="sxs-lookup"><span data-stu-id="8ad58-3358">Default Query Indexer</span></span>

<span data-ttu-id="8ad58-3359">要素型が 0 @no__t で、既定のプロパティを持たないすべてのクエリ可能なコレクション型は、次の一般的な形式の既定のプロパティを持つと見なされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3359">Every queryable collection type whose element type is `T` and does not already have a default property is considered to have a default property of the following general form:</span></span>

```vb
Public ReadOnly Default Property Item(index As Integer) As T
    Get
        Return Me.ElementAtOrDefault(index)
    End Get
End Property
```

<span data-ttu-id="8ad58-3360">既定のプロパティは、既定のプロパティアクセスの構文を使用してのみ参照できます。既定のプロパティを名前で参照することはできません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3360">The default property can only be referred to using the default property access syntax; the default property cannot be referred to by name.</span></span> <span data-ttu-id="8ad58-3361">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3361">For example:</span></span>

```vb
Dim customers As IEnumerable(Of Customer) = ...
Dim customerThree = customers(2)

' Error, no such property
Dim customerFour = customers.Item(4)
```

<span data-ttu-id="8ad58-3362">コレクション型に @no__t 0 のメンバーがない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3362">If the collection type does not have an `ElementAtOrDefault` member, a compile-time error will occur.</span></span>

### <a name="from-query-operator"></a><span data-ttu-id="8ad58-3363">From クエリ演算子</span><span class="sxs-lookup"><span data-stu-id="8ad58-3363">From Query Operator</span></span>

<span data-ttu-id="8ad58-3364">@No__t-0 クエリ演算子は、クエリ対象のコレクションの個々のメンバーを表すコレクション範囲変数を導入します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3364">The `From` query operator introduces a collection range variable that represents the individual members of a collection to be queried.</span></span>

```antlr
FromQueryOperator
    : LineTerminator? 'From' LineTerminator? CollectionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="8ad58-3365">たとえば、クエリ式は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3365">For example, the query expression:</span></span>

```vb
From c As Customer In Customers ...
```

<span data-ttu-id="8ad58-3366">はと同じであると考えることができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3366">can be thought of as equivalent to</span></span>

```vb
For Each c As Customer In Customers
        ...
Next c
```

<span data-ttu-id="8ad58-3367">@No__t 0 のクエリ演算子で複数のコレクション範囲変数が宣言されている場合、またはクエリ式の最初の `From` クエリ演算子ではない場合、新しいコレクション範囲変数は、既存の一連の範囲変数に*クロス結合*されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3367">When a `From` query operator declares multiple collection range variables or is not the first `From` query operator in the query expression, each new collection range variable is *cross joined* to the existing set of range variables.</span></span> <span data-ttu-id="8ad58-3368">結果として、結合されたコレクション内のすべての要素のクロス積に対してクエリが評価されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3368">The result is that the query is evaluated over the cross-product of all the elements in the joined collections.</span></span> <span data-ttu-id="8ad58-3369">たとえば、式は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3369">For example, the expression:</span></span>

```vb
From c In Customers _
From e In Employees _
...
```

<span data-ttu-id="8ad58-3370">次の場合と同じであると考えることができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3370">can be thought of as equivalent to:</span></span>

```vb
For Each c In Customers
    For Each e In Employees
            ...
    Next e
Next c
```

<span data-ttu-id="8ad58-3371">とは、次とまったく同じです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3371">and is exactly equivalent to:</span></span>

```vb
From c In Customers, e In Employees ...
```

<span data-ttu-id="8ad58-3372">前のクエリ演算子で導入された範囲変数は、後の `From` クエリ演算子で使用できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3372">The range variables introduced in previous query operators can be used in a later `From` query operator.</span></span> <span data-ttu-id="8ad58-3373">たとえば、次のクエリ式では、2番目の `From` クエリ演算子は、最初の範囲変数の値を参照します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3373">For example, in the following query expression the second `From` query operator refers to the value of the first range variable:</span></span>

```vb
From c As Customer In Customers _
From o As Order In c.Orders _
Select c.Name, o
```

<span data-ttu-id="8ad58-3374">@No__t-0 クエリ演算子または複数の `From` クエリ演算子内の複数の範囲変数は、コレクション型に次のメソッドのいずれかまたは両方が含まれている場合にのみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3374">Multiple range variables in a `From` query operator or multiple `From` query operators are only supported if the collection type contains one or both of the following methods:</span></span>

```vb
Function SelectMany(selector As Func(Of T, CR)) As CR
Function SelectMany(selector As Func(Of T, CS), _
                          resultsSelector As Func(Of T, S, R)) As CR
```

<span data-ttu-id="8ad58-3375">コード</span><span class="sxs-lookup"><span data-stu-id="8ad58-3375">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs, y In ys ...
```

<span data-ttu-id="8ad58-3376">通常はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3376">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = _
    xs.SelectMany( _
        Function(x As Integer) ys, _
        Function(x As Integer, y As Integer) New With {x, y})...
```

<span data-ttu-id="8ad58-3377">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3377">__Note.__</span></span> <span data-ttu-id="8ad58-3378">`From` は予約語ではありません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3378">`From` is not a reserved word.</span></span>


### <a name="join-query-operator"></a><span data-ttu-id="8ad58-3379">Join クエリ演算子</span><span class="sxs-lookup"><span data-stu-id="8ad58-3379">Join Query Operator</span></span>

<span data-ttu-id="8ad58-3380">@No__t-0 クエリ演算子は、既存の範囲変数を新しいコレクション範囲変数と結合し、等値式に基づいて要素が結合された単一のコレクションを生成します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3380">The `Join` query operator joins existing range variables with a new collection range variable, producing a single collection whose elements have been joined together based on an equality expression.</span></span>

```antlr
JoinQueryOperator
    : LineTerminator? 'Join' LineTerminator? CollectionRangeVariableDeclaration
      JoinOrGroupJoinQueryOperator? LineTerminator? 'On' LineTerminator? JoinConditionList
    ;

JoinConditionList
    : JoinCondition ( 'And' LineTerminator? JoinCondition )*
    ;

JoinCondition
    : Expression 'Equals' LineTerminator? Expression
    ;
```

<span data-ttu-id="8ad58-3381">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3381">For example:</span></span>

```vb
Dim customersAndOrders = _
    From cust In Customers _
    Join ord In Orders On cust.ID Equals ord.CustomerID
```

<span data-ttu-id="8ad58-3382">等値式は、通常の等値式よりも制限されています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3382">The equality expression is more restricted than a regular equality expression:</span></span>

* <span data-ttu-id="8ad58-3383">両方の式が値として分類される必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3383">Both expressions must be classified as a value.</span></span>

* <span data-ttu-id="8ad58-3384">両方の式が少なくとも1つの範囲変数を参照している必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3384">Both expressions must reference at least one range variable.</span></span>

* <span data-ttu-id="8ad58-3385">Join クエリ演算子で宣言された範囲変数は、いずれかの式で参照されている必要があり、その式は他の範囲変数を参照することはできません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3385">The range variable declared in the join query operator must be referenced by one of the expressions, and that expression must not reference any other range variables.</span></span>

<span data-ttu-id="8ad58-3386">2つの式の型がまったく同じ型でない場合は、</span><span class="sxs-lookup"><span data-stu-id="8ad58-3386">If the types of the two expressions are not the exact same type, then</span></span>

* <span data-ttu-id="8ad58-3387">等値演算子が2つの型に対して定義されている場合、両方の式が暗黙的に変換可能であり、@no__t 0 ではありません。その後、両方の式をその型に変換します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3387">If the equality operator is defined for the two types, both expressions are implicitly convertible to it, and it is not `Object`, then convert both expressions to that type.</span></span>

* <span data-ttu-id="8ad58-3388">それ以外の場合、両方の式を暗黙的にに変換できる、優先される型がある場合は、両方の式をその型に変換します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3388">Otherwise, if there is a dominant type that both expressions can be implicitly converted to, then convert both expressions to that type.</span></span>

* <span data-ttu-id="8ad58-3389">それ以外の場合は、コンパイル時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3389">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="8ad58-3390">式は、効率的に等値演算子を使用するのではなく、ハッシュ値 (つまり `GetHashCode()` を呼び出すことによって) を使用して比較されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3390">The expressions are compared using hash values (i.e. by calling `GetHashCode()`) rather than by using equality operators for efficiency.</span></span> <span data-ttu-id="8ad58-3391">@No__t 0 クエリ演算子は、同じ演算子で複数の結合または等値条件を実行できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3391">A `Join` query operator may do multiple joins or equality conditions in the same operator.</span></span> <span data-ttu-id="8ad58-3392">@No__t-0 クエリ演算子は、コレクション型にメソッドが含まれている場合にのみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3392">A `Join` query operator is only supported if the collection type contains a method:</span></span>

```vb
Function Join(inner As CS, _
                  outerSelector As Func(Of T, K), _
                  innerSelector As Func(Of S, K), _
                  resultSelector As Func(Of T, S, R)) As CR
```

<span data-ttu-id="8ad58-3393">コード</span><span class="sxs-lookup"><span data-stu-id="8ad58-3393">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs _
            Join y In ys On x Equals y _
            ...
```

<span data-ttu-id="8ad58-3394">通常はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3394">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = _
    xs.Join( _
        ys, _
        Function(x As Integer) x, _
        Function(y As Integer) y, _
        Function(x As Integer, y As Integer) New With {x, y})...
```

<span data-ttu-id="8ad58-3395">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3395">__Note.__</span></span> <span data-ttu-id="8ad58-3396">`Join`、`On`、`Equals` は予約語ではありません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3396">`Join`, `On` and `Equals` are not reserved words.</span></span>


### <a name="let-query-operator"></a><span data-ttu-id="8ad58-3397">Let クエリ演算子</span><span class="sxs-lookup"><span data-stu-id="8ad58-3397">Let Query Operator</span></span>

<span data-ttu-id="8ad58-3398">@No__t-0 クエリ演算子は、式の範囲変数を導入します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3398">The `Let` query operator introduces an expression range variable.</span></span> <span data-ttu-id="8ad58-3399">これにより、後のクエリ演算子で複数回使用される中間値を1回計算できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3399">This allows calculating an intermediate value once that will be used multiple times in later query operators.</span></span>

```antlr
LetQueryOperator
    : LineTerminator? 'Let' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="8ad58-3400">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3400">For example:</span></span>

```vb
Dim taxedPrices = _
    From o In Orders _
    Let tax = o.Price * 0.088 _
    Where tax > 3.50 _
    Select o.Price, tax, total = o.Price + tax
```

<span data-ttu-id="8ad58-3401">次の場合と同じであると考えることができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3401">can be thought of as equivalent to:</span></span>

```vb
For Each o In Orders
    Dim tax = o.Price * 0.088
    ...
Next o
```

<span data-ttu-id="8ad58-3402">@No__t-0 クエリ演算子は、コレクション型にメソッドが含まれている場合にのみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3402">A `Let` query operator is only supported if the collection type contains a method:</span></span>

```vb
Function Select(selector As Func(Of T, R)) As CR
```

<span data-ttu-id="8ad58-3403">コード</span><span class="sxs-lookup"><span data-stu-id="8ad58-3403">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Let y = x * 10 _
            ...
```

<span data-ttu-id="8ad58-3404">通常はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3404">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Select(Function(x As Integer) New With {x, .y = x * 10})...
```


### <a name="select-query-operator"></a><span data-ttu-id="8ad58-3405">Select クエリ演算子</span><span class="sxs-lookup"><span data-stu-id="8ad58-3405">Select Query Operator</span></span>

<span data-ttu-id="8ad58-3406">@No__t-0 クエリ演算子は、式の範囲変数を導入するというの `Let` クエリ演算子に似ています。ただし、`Select` クエリ演算子は、現在使用できる範囲変数を追加するのではなく、非表示にします。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3406">The `Select` query operator is like the `Let` query operator in that it introduces expression range variables; however, a `Select` query operator hides the currently available range variables instead of adding to them.</span></span> <span data-ttu-id="8ad58-3407">また、`Select` クエリ演算子によって導入される式範囲変数の型は、常にローカル変数型の推論規則を使用して推論されます。明示的な型を指定することはできません。また、推論できる型がない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3407">Also, the type of an expression range variable introduced by a `Select` query operator is always inferred using local variable type inference rules; an explicit type cannot be specified, and if no type can be inferred, a compile-time error occurs.</span></span>

```antlr
SelectQueryOperator
    : LineTerminator? 'Select' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="8ad58-3408">たとえば、次のクエリについて考えます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3408">For example, in the query:</span></span>

```vb
Dim smiths = _
    From cust In Customers _
    Select name = cust.name _
    Where name.EndsWith("Smith")
```

<span data-ttu-id="8ad58-3409">`Where` クエリ演算子は、`Select` 演算子によって導入された `name` 範囲変数にのみアクセスできます。`Where` オペレーターが `cust` を参照しようとした場合、コンパイル時エラーが発生した可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3409">the `Where` query operator only has access to the `name` range variable introduced by the `Select` operator; if the `Where` operator had tried to reference `cust`, a compile-time error would have occurred.</span></span>

<span data-ttu-id="8ad58-3410">範囲変数の名前を明示的に指定する代わりに、`Select` クエリ演算子は、匿名型オブジェクト作成式と同じ規則を使用して、範囲変数の名前を推論できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3410">Instead of explicitly specifying the names of the range variables, a `Select` query operator can infer the names of the range variables, using the same rules as anonymous type object creation expressions.</span></span> <span data-ttu-id="8ad58-3411">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3411">For example:</span></span>

```vb
Dim custAndOrderNames = _
      From cust In Customers, ord In cust.Orders _
      Select cust.name, ord.ProductName _
        Where name.EndsWith("Smith")
```

<span data-ttu-id="8ad58-3412">範囲変数の名前が指定されておらず、名前が推論できない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3412">If the name of the range variable is not supplied and a name cannot be inferred, a compile-time error occurs.</span></span> <span data-ttu-id="8ad58-3413">@No__t-0 クエリ演算子に1つの式だけが含まれている場合、その範囲変数の名前を推論できないが、範囲変数には名前が付いていない場合、エラーは発生しません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3413">If the `Select` query operator contains only a single expression, no error occurs if a name for that range variable cannot be inferred but the range variable is nameless.</span></span>  <span data-ttu-id="8ad58-3414">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3414">For example:</span></span>

```vb
Dim custAndOrderNames = _
      From cust In Customers, ord In cust.Orders _
      Select cust.Name & " bought " & ord.ProductName _
        Take 10
```

<span data-ttu-id="8ad58-3415">範囲変数と等値式に名前を割り当てる間に、`Select` クエリ演算子にあいまいさがある場合は、名前の割り当てが優先されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3415">If there is an ambiguity in a `Select` query operator between assigning a name to a range variable and an equality expression, the name assignment is preferred.</span></span> <span data-ttu-id="8ad58-3416">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3416">For example:</span></span>

```vb
Dim badCustNames = _
      From c In Customers _
        Let name = "John Smith" _
      Select name = c.Name ' Creates a range variable named "name"


Dim goodCustNames = _
      From c In Customers _
        Let name = "John Smith" _
      Select match = (name = c.Name)
```

<span data-ttu-id="8ad58-3417">@No__t-0 クエリ演算子の各式は、値として分類される必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3417">Each expression in the `Select` query operator must be classified as a value.</span></span> <span data-ttu-id="8ad58-3418">@No__t-0 クエリ演算子は、コレクション型にメソッドが含まれている場合にのみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3418">A `Select` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Select(selector As Func(Of T, R)) As CR
```

<span data-ttu-id="8ad58-3419">コード</span><span class="sxs-lookup"><span data-stu-id="8ad58-3419">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Select x, y = x * 10 _
            ...
```

<span data-ttu-id="8ad58-3420">通常はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3420">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Select(Function(x As Integer) New With {x, .y = x * 10})...
```


### <a name="distinct-query-operator"></a><span data-ttu-id="8ad58-3421">Distinct クエリ演算子</span><span class="sxs-lookup"><span data-stu-id="8ad58-3421">Distinct Query Operator</span></span>

<span data-ttu-id="8ad58-3422">@No__t-0 クエリ演算子は、要素の型が等しいかどうかを比較することによって、コレクション内の値を個別の値を持つ値のみに制限します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3422">The `Distinct` query operator restricts the values in a collection only to those with distinct values, as determined by comparing the element type for equality.</span></span>

```antlr
DistinctQueryOperator
    : LineTerminator? 'Distinct' LineTerminator?
    ;
```

<span data-ttu-id="8ad58-3423">たとえば、クエリは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3423">For example, the query:</span></span>

```vb
Dim distinctCustomerPrice = _
    From cust In Customers, ord In cust.Orders _
    Select cust.Name, ord.Price _
    Distinct
```

<span data-ttu-id="8ad58-3424">は、顧客名と注文価格の個別の組み合わせごとに1つの行のみを返します。これは、顧客が同じ価格を持つ複数の注文を持っている場合でも同様です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3424">will only return one row for each distinct pairing of customer name and order price, even if the customer has multiple orders with the same price.</span></span> <span data-ttu-id="8ad58-3425">@No__t-0 クエリ演算子は、コレクション型にメソッドが含まれている場合にのみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3425">A `Distinct` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Distinct() As CT
```

<span data-ttu-id="8ad58-3426">コード</span><span class="sxs-lookup"><span data-stu-id="8ad58-3426">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Distinct _
            ...
```

<span data-ttu-id="8ad58-3427">通常はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3427">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = xs.Distinct()...
```

<span data-ttu-id="8ad58-3428">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3428">__Note.__</span></span> <span data-ttu-id="8ad58-3429">`Distinct` は予約語ではありません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3429">`Distinct` is not a reserved word.</span></span>


### <a name="where-query-operator"></a><span data-ttu-id="8ad58-3430">Where クエリ演算子</span><span class="sxs-lookup"><span data-stu-id="8ad58-3430">Where Query Operator</span></span>

<span data-ttu-id="8ad58-3431">@No__t-0 クエリ演算子は、コレクション内の値を、特定の条件を満たす値に制限します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3431">The `Where` query operator restricts the values in a collection to those that satisfy a given condition.</span></span>

```antlr
WhereQueryOperator
    : LineTerminator? 'Where' LineTerminator? BooleanExpression
    ;
```

<span data-ttu-id="8ad58-3432">@No__t-0 クエリ演算子は、範囲変数値のセットごとに評価されるブール式を受け取ります。式の値が true の場合、値は出力コレクションに表示されます。それ以外の場合は、値がスキップされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3432">A `Where` query operator takes a Boolean expression that is evaluated for each set of range variable values; if the value of the expression is true, then the values appear in the output collection, otherwise the values are skipped.</span></span> <span data-ttu-id="8ad58-3433">たとえば、クエリ式は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3433">For example, the query expression:</span></span>

```vb
From cust In Customers, ord In Orders _
Where cust.ID = ord.CustomerID _
...
```

<span data-ttu-id="8ad58-3434">は、入れ子になったループと同じであると考えることができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3434">can be thought of as equivalent to the nested loop</span></span>

```vb
For Each cust In Customers
    For Each ord In Orders
            If cust.ID = ord.CustomerID Then
                ...
            End If
    Next ord
Next cust
```

<span data-ttu-id="8ad58-3435">@No__t-0 クエリ演算子は、コレクション型にメソッドが含まれている場合にのみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3435">A `Where` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Where(predicate As Func(Of T, B)) As CT
```

<span data-ttu-id="8ad58-3436">コード</span><span class="sxs-lookup"><span data-stu-id="8ad58-3436">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Where x < 10 _
            ...
```

<span data-ttu-id="8ad58-3437">通常はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3437">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Where(Function(x As Integer) x < 10)...
```

<span data-ttu-id="8ad58-3438">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3438">__Note.__</span></span> <span data-ttu-id="8ad58-3439">`Where` は予約語ではありません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3439">`Where` is not a reserved word.</span></span>


### <a name="partition-query-operators"></a><span data-ttu-id="8ad58-3440">パーティションクエリ演算子</span><span class="sxs-lookup"><span data-stu-id="8ad58-3440">Partition Query Operators</span></span>

```antlr
PartitionQueryOperator
    : LineTerminator? 'Take' LineTerminator? Expression
    | LineTerminator? 'Take' 'While' LineTerminator? BooleanExpression
    | LineTerminator? 'Skip' LineTerminator? Expression
    | LineTerminator? 'Skip' 'While' LineTerminator? BooleanExpression
    ;
```

<span data-ttu-id="8ad58-3441">@No__t-0 クエリ演算子は、コレクションの最初の @no__t 要素になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3441">The `Take` query operator results in the first `n` elements of a collection.</span></span> <span data-ttu-id="8ad58-3442">@No__t-0 修飾子と共に使用すると、`Take` 演算子は、ブール式を満たすコレクションの最初の @no__t 2 要素になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3442">When used with the `While` modifier, the `Take` operator results in the first `n` elements of a collection that satisfy a Boolean expression.</span></span> <span data-ttu-id="8ad58-3443">@No__t-0 演算子は、コレクションの最初の @no__t 要素をスキップし、コレクションの残りの部分を返します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3443">The `Skip` operator skips the first `n` elements of a collection and then returns the remainder of the collection.</span></span>  <span data-ttu-id="8ad58-3444">@No__t-0 修飾子と組み合わせて使用すると、`Skip` 演算子は、ブール式を満たすコレクションの最初の `n` 要素をスキップし、コレクションの残りの部分を返します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3444">When used in conjunction with the `While` modifier, the `Skip` operator skips the first `n` elements of a collection that satisfy a Boolean expression and then returns the rest of the collection.</span></span> <span data-ttu-id="8ad58-3445">@No__t-0 または `Skip` クエリ演算子内の式は、値として分類される必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3445">The expressions in a `Take` or `Skip` query operator must be classified as a value.</span></span>

<span data-ttu-id="8ad58-3446">@No__t-0 クエリ演算子は、コレクション型にメソッドが含まれている場合にのみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3446">A `Take` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Take(count As N) As CT
```

<span data-ttu-id="8ad58-3447">@No__t-0 クエリ演算子は、コレクション型にメソッドが含まれている場合にのみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3447">A `Skip` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Skip(count As N) As CT
```

<span data-ttu-id="8ad58-3448">@No__t-0 クエリ演算子は、コレクション型にメソッドが含まれている場合にのみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3448">A `Take While` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function TakeWhile(predicate As Func(Of T, B)) As CT
```

<span data-ttu-id="8ad58-3449">@No__t-0 クエリ演算子は、コレクション型にメソッドが含まれている場合にのみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3449">A `Skip While` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function SkipWhile(predicate As Func(Of T, B)) As CT
```

<span data-ttu-id="8ad58-3450">コード</span><span class="sxs-lookup"><span data-stu-id="8ad58-3450">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Skip 10 _
            Take 5 _
            Skip While x < 10 _
            Take While x > 5 _
            ...
```

<span data-ttu-id="8ad58-3451">通常はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3451">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Skip(10). _
        Take(5). _
        SkipWhile(Function(x) x < 10). _
        TakeWhile(Function(x) x > 5)...
```

<span data-ttu-id="8ad58-3452">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3452">__Note.__</span></span> <span data-ttu-id="8ad58-3453">`Take` および `Skip` は予約語ではありません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3453">`Take` and `Skip` are not reserved words.</span></span>


### <a name="order-by-query-operator"></a><span data-ttu-id="8ad58-3454">Order By クエリ演算子</span><span class="sxs-lookup"><span data-stu-id="8ad58-3454">Order By Query Operator</span></span>

<span data-ttu-id="8ad58-3455">@No__t-0 クエリ演算子は、範囲変数に表示される値を並べ替えます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3455">The `Order By` query operator orders the values that appear in the range variables.</span></span> 

```antlr
OrderByQueryOperator
    : LineTerminator? 'Order' 'By' LineTerminator? OrderExpressionList
    ;

OrderExpressionList
    : OrderExpression ( Comma OrderExpression )*
    ;

OrderExpression
    : Expression Ordering?
    ;

Ordering
    : 'Ascending' | 'Descending'
    ;
```

<span data-ttu-id="8ad58-3456">@No__t 0 クエリ演算子は、反復変数の順序付けに使用するキー値を指定する式を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3456">An `Order By` query operator takes expressions that specify the key values that should be used to order the iteration variables.</span></span> <span data-ttu-id="8ad58-3457">たとえば、次のクエリでは、価格で並べ替えられた製品が返されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3457">For example, the following query returns products sorted by price:</span></span>

```vb
Dim productsByPrice = _
    From p In Products _
    Order By p.Price _
    Select p.Name
```

<span data-ttu-id="8ad58-3458">順序付けは `Ascending` としてマークできます。この場合、小さい値の方が大きい値の前になるか、または `Descending` になります。この場合、大きな値は小さい方の値よりも前になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3458">An ordering can be marked as `Ascending`, in which case smaller values come before larger values, or `Descending`, in which case larger values come before smaller values.</span></span> <span data-ttu-id="8ad58-3459">順序が指定されていない場合の既定値は `Ascending` です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3459">The default for an ordering if none is specified is `Ascending`.</span></span> <span data-ttu-id="8ad58-3460">たとえば、次のクエリでは、最もコストの高い製品を最初に価格で並べ替えた製品が返されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3460">For example, the following query returns products sorted by price with the most expensive product first:</span></span>

```vb
Dim productsByPriceDesc = _
    From p In Products _
    Order By p.Price Descending _
    Select p.Name
```

<span data-ttu-id="8ad58-3461">@No__t-0 クエリ演算子では、順序付けのために複数の式を指定できます。この場合、コレクションは入れ子になった方法で並べ替えられます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3461">The `Order By` query operator may specify multiple expressions for ordering, in which case the collection is ordered in a nested manner.</span></span> <span data-ttu-id="8ad58-3462">たとえば、次のクエリでは、州別に顧客を注文した後、州ごとに市区町村別に、各都市内の郵便番号によって顧客を並べ替えます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3462">For example, the following query orders customers by state, then by city within each state and then by ZIP code within each city:</span></span>

```vb
Dim customersByLocation = _
    From c In Customers _
    Order By c.State, c.City, c.ZIP _
    Select c.Name, c.State, c.City, c.ZIP
```

<span data-ttu-id="8ad58-3463">@No__t-0 クエリ演算子内の式は、値として分類される必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3463">The expressions in an `Order By` query operator must be classified as a value.</span></span> <span data-ttu-id="8ad58-3464">@No__t-0 クエリ演算子は、コレクション型に次のメソッドのいずれかまたは両方が含まれている場合にのみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3464">An `Order By` query operator is supported only if the collection type contains one or both of the following methods:</span></span>

```vb
Function OrderBy(keySelector As Func(Of T, K)) As CT
Function OrderByDescending(keySelector As Func(Of T, K)) As CT
```

<span data-ttu-id="8ad58-3465">戻り値の型 `CT` は*順序付け*られたコレクションである必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3465">The return type `CT` must be an *ordered collection*.</span></span> <span data-ttu-id="8ad58-3466">順序付けられたコレクションは、次のいずれかまたは両方のメソッドを含むコレクション型です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3466">An ordered collection is a collection type that contains one or both of the methods:</span></span>

```vb
Function ThenBy(keySelector As Func(Of T, K)) As CT
Function ThenByDescending(keySelector As Func(Of T, K)) As CT
```

<span data-ttu-id="8ad58-3467">コード</span><span class="sxs-lookup"><span data-stu-id="8ad58-3467">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Order By x Ascending, x Mod 2 Descending _
            ...
```

<span data-ttu-id="8ad58-3468">通常はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3468">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.OrderBy(Function(x) x).ThenByDescending(Function(x) x Mod 2)...
```

<span data-ttu-id="8ad58-3469">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3469">__Note.__</span></span> <span data-ttu-id="8ad58-3470">クエリ演算子は、特定のクエリ操作を実装するメソッドに構文をマップするだけなので、順序の保持は言語によって決まりません。また、演算子自体の実装によって決定されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3470">Because query operators simply map syntax to methods that implement a particular query operation, order preservation is not dictated by the language and is determined by the implementation of the operator itself.</span></span> <span data-ttu-id="8ad58-3471">これはユーザー定義の演算子とよく似ています。これは、ユーザー定義の数値型に対して加算演算子をオーバーロードする実装によって、加算に似た処理が行われない場合があるという点です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3471">This is very similar to user-defined operators in that the implementation to overload the addition operator for a user-defined numeric type may not perform anything resembling an addition.</span></span> <span data-ttu-id="8ad58-3472">もちろん、予測可能性を維持するために、ユーザーの期待に一致しないものを実装することは推奨されません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3472">Of course, to preserve predictability, implementing something that does not match user expectations is not recommended.</span></span>

<span data-ttu-id="8ad58-3473">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3473">__Note.__</span></span> <span data-ttu-id="8ad58-3474">`Order` および `By` は予約語ではありません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3474">`Order` and `By` are not reserved words.</span></span>


### <a name="group-by-query-operator"></a><span data-ttu-id="8ad58-3475">Group By クエリ演算子</span><span class="sxs-lookup"><span data-stu-id="8ad58-3475">Group By Query Operator</span></span>

<span data-ttu-id="8ad58-3476">@No__t-0 クエリ演算子は、1つまたは複数の式に基づいてスコープ内の範囲変数をグループ化し、それらのグループに基づいて新しい範囲変数を生成します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3476">The `Group By` query operator groups the range variables in scope based on one or more expressions, and then produces new range variables based on those groupings.</span></span>

```antlr
GroupByQueryOperator
    : LineTerminator? 'Group' ( LineTerminator? ExpressionRangeVariableDeclarationList )?
      LineTerminator? 'By' LineTerminator? ExpressionRangeVariableDeclarationList
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="8ad58-3477">たとえば、次のクエリでは、すべての顧客を `State` でグループ化し、各グループのカウントと平均経過時間を計算します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3477">For example, the following query groups all customers by `State`, and then computes the count and average age of each group:</span></span>

```vb
Dim averageAges = _
    From cust In Customers _
    Group By cust.State _
    Into Count(), Average(cust.Age)
```

<span data-ttu-id="8ad58-3478">@No__t-0 クエリ演算子には、省略可能な `Group` 句、`By` 句、および `Into` 句の3つの句があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3478">The `Group By` query operator has three clauses: the optional `Group` clause, the `By` clause, and the `Into` clause.</span></span> <span data-ttu-id="8ad58-3479">@No__t-0 句は、`Select` クエリ演算子と同じ構文と効果がありますが、`By` 句ではなく `Into` 句で使用できる範囲変数にのみ影響します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3479">The `Group` clause has the same syntax and effect as a `Select` query operator, except that it only affects the range variables available in the `Into` clause and not the `By` clause.</span></span> <span data-ttu-id="8ad58-3480">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3480">For example:</span></span>

```vb
Dim averageAges = _
    From cust In Customers _
    Group cust.Age By cust.State _
    Into Count(), Average(Age)
```

<span data-ttu-id="8ad58-3481">@No__t-0 句は、グループ化操作でキー値として使用される式の範囲変数を宣言します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3481">The `By` clause declares expression range variables that are used as key values in the grouping operation.</span></span> <span data-ttu-id="8ad58-3482">@No__t-0 句を使用すると、`By` 句で形成される各グループに対して集計を計算する式範囲変数を宣言できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3482">The `Into` clause allows the declaration of expression range variables that calculate aggregations over each of the groups formed by the `By` clause.</span></span> <span data-ttu-id="8ad58-3483">@No__t-0 句内では、式の範囲変数には、*集計関数*のメソッド呼び出しである式のみを割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3483">Within the `Into` clause, the expression range variable can only be assigned an expression which is a method invocation of an *aggregate function*.</span></span> <span data-ttu-id="8ad58-3484">集計関数は、次のいずれかのメソッドのような、グループのコレクション型に対する関数です (元のコレクションと同じコレクション型であるとは限りません)。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3484">An aggregate function is a function on the collection type of the group (which may not necessarily be the same collection type of the original collection) which looks like either of the following methods:</span></span>

```vb
Function _name_() As _type_
Function _name_(selector As Func(Of T, R)) As R
```

<span data-ttu-id="8ad58-3485">集計関数がデリゲート引数を受け取る場合、呼び出し式は、値として分類される必要がある引数式を持つことができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3485">If an aggregate function takes a delegate argument, then the invocation expression can have an argument expression that must be classified as a value.</span></span>  <span data-ttu-id="8ad58-3486">引数式では、スコープ内の範囲変数を使用できます。集計関数の呼び出し内では、これらの範囲変数は、コレクション内のすべての値ではなく、形成されているグループ内の値を表します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3486">The argument expression can use the range variables that are in scope; within the call to an aggregate function, those range variables represent the values in the group being formed, not all of the values in the collection.</span></span> <span data-ttu-id="8ad58-3487">たとえば、このセクションの元の例では、`Average` 関数は、すべての顧客に対してではなく、顧客の年齢ごとの平均を計算します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3487">For example, in the original example in this section the `Average` function calculates the average of the customers' ages per state rather than for all of the customers together.</span></span>

<span data-ttu-id="8ad58-3488">すべてのコレクション型には、集計関数が定義されていると見なされます。この関数は、パラメーターを取らず、単にグループを返します。 @no__t。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3488">All collection types are considered to have the aggregate function `Group` defined on it, which takes no parameters and simply returns the group.</span></span> <span data-ttu-id="8ad58-3489">コレクション型によって提供されるその他の標準の集計関数は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3489">Other standard aggregate functions that a collection type may provide are:</span></span>

<span data-ttu-id="8ad58-3490">`Count` および `LongCount` で、グループ内の要素の数またはブール式を満たすグループ内の要素の数を返します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3490">`Count` and `LongCount`, which return the count of the elements in the group or the count of the elements in the group that satisfy a Boolean expression.</span></span> <span data-ttu-id="8ad58-3491">`Count` および `LongCount` は、コレクション型にメソッドが1つ含まれている場合にのみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3491">`Count` and `LongCount` are supported only if the collection type contains one of the methods:</span></span>

```vb
Function Count() As N
Function Count(selector As Func(Of T, B)) As N
Function LongCount() As N
Function LongCount(selector As Func(Of T, B)) As N
```

<span data-ttu-id="8ad58-3492">`Sum`。グループ内のすべての要素にわたる式の合計を返します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3492">`Sum`, which returns the sum of an expression across all the elements in the group.</span></span> <span data-ttu-id="8ad58-3493">`Sum` がサポートされるのは、コレクション型にメソッドが1つ含まれている場合のみです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3493">`Sum` is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Sum() As N
Function Sum(selector As Func(Of T, N)) As N
```

<span data-ttu-id="8ad58-3494">`Min` を指定すると、グループ内のすべての要素にわたる式の最小値が返されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3494">`Min` which returns the minimum value of an expression across all the elements in the group.</span></span> <span data-ttu-id="8ad58-3495">`Min` がサポートされるのは、コレクション型にメソッドが1つ含まれている場合のみです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3495">`Min` is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Min() As N
Function Min(selector As Func(Of T, N)) As N
```

<span data-ttu-id="8ad58-3496">`Max`。グループ内のすべての要素にわたる式の最大値を返します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3496">`Max`, which returns the maximum value of an expression across all the elements in the group.</span></span> <span data-ttu-id="8ad58-3497">`Max` がサポートされるのは、コレクション型にメソッドが1つ含まれている場合のみです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3497">`Max` is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Max() As N
Function Max(selector As Func(Of T, N)) As N
```

<span data-ttu-id="8ad58-3498">`Average`。グループ内のすべての要素にわたる式の平均を返します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3498">`Average`, which returns the average of an expression across all the elements in the group.</span></span> <span data-ttu-id="8ad58-3499">`Average` がサポートされるのは、コレクション型にメソッドが1つ含まれている場合のみです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3499">`Average` is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Average() As N
Function Average(selector As Func(Of T, N)) As N
```

<span data-ttu-id="8ad58-3500">`Any`。グループにメンバーが含まれているかどうか、またはグループ内の任意の要素に対してブール式が true であるかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3500">`Any`, which determines whether a group contains members or if a Boolean expression is true for any element in the group.</span></span> <span data-ttu-id="8ad58-3501">`Any` は、ブール式で使用できる値を返します。この値は、コレクション型にメソッドが1つ含まれている場合にのみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3501">`Any` returns a value that can be used in a Boolean expression and is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Any() As B
Function Any(predicate As Func(Of T, B)) As B
```

<span data-ttu-id="8ad58-3502">`All`。グループ内のすべての要素に対してブール式が true かどうかを決定します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3502">`All`, which determines whether a Boolean expression is true for all elements in the group.</span></span> <span data-ttu-id="8ad58-3503">`All` は、ブール式で使用できる値を返します。この値は、コレクション型にメソッドが含まれている場合にのみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3503">`All` returns a value that can be used in a Boolean expression and is supported only if the collection type contains a method:</span></span>

```vb
Function All(predicate As Func(Of T, B)) As B
```

<span data-ttu-id="8ad58-3504">@No__t 0 のクエリ演算子の後に、スコープ内の以前の範囲変数は非表示になり、`By` および `Into` の各句によって導入された範囲変数が使用可能になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3504">After a `Group By` query operator, the range variables previously in scope are hidden, and the range variables introduced by the `By` and `Into` clauses are available.</span></span> <span data-ttu-id="8ad58-3505">@No__t-0 クエリ演算子は、コレクション型にメソッドが含まれている場合にのみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3505">A `Group By` query operator is supported only if the collection type contains the method:</span></span>

```vb
Function GroupBy(keySelector As Func(Of T, K), _
                      resultSelector As Func(Of K, CT, R)) As CR
```

<span data-ttu-id="8ad58-3506">@No__t-0 句の範囲変数宣言は、コレクション型にメソッドが含まれている場合にのみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3506">Range variable declarations in the `Group` clause are supported only if the collection type contains the method:</span></span>

```vb
Function GroupBy(keySelector As Func(Of T, K), _
                      elementSelector As Func(Of T, S), _
                      resultSelector As Func(Of K, CS, R)) As CR
```

<span data-ttu-id="8ad58-3507">コード</span><span class="sxs-lookup"><span data-stu-id="8ad58-3507">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Group y = x * 10, z = x / 10 By evenOdd = x Mod 2 _
            Into Sum(y), Average(z) _
            ...
```

<span data-ttu-id="8ad58-3508">通常はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3508">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.GroupBy( _
        Function(x As Integer) x Mod 2, _
        Function(x As Integer) New With {.y = x * 10, .z = x / 10}, _
        Function(evenOdd, group) New With { _
            evenOdd, _
            .Sum = group.Sum(Function(e) e.y), _
            .Average = group.Average(Function(e) e.z)})...
```

<span data-ttu-id="8ad58-3509">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3509">__Note.__</span></span> <span data-ttu-id="8ad58-3510">`Group`、`By`、`Into` は予約語ではありません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3510">`Group`, `By`, and `Into` are not reserved words.</span></span>


### <a name="aggregate-query-operator"></a><span data-ttu-id="8ad58-3511">集計クエリ演算子</span><span class="sxs-lookup"><span data-stu-id="8ad58-3511">Aggregate Query Operator</span></span>

<span data-ttu-id="8ad58-3512">@No__t-0 クエリ演算子は、`Group By` 演算子と同様の関数を実行します。ただし、既に形成されているグループを集計することはできます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3512">The `Aggregate` query operator performs a similar function as the `Group By` operator, except it allows aggregating over groups that have already been formed.</span></span> <span data-ttu-id="8ad58-3513">グループは既に形成されているため、`Aggregate` クエリ演算子の `Into` 句はスコープ内の範囲変数を非表示にしません (この方法では、`Aggregate` は `Let` のようになり、@no__t 4 は `Select` に似ています)。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3513">Because the group has already been formed, the `Into` clause of an `Aggregate` query operator does not hide the range variables in scope (in this way, `Aggregate` is more like a `Let`, and `Group By` is more like a `Select`).</span></span>

```antlr
AggregateQueryOperator
    : LineTerminator? 'Aggregate' LineTerminator? CollectionRangeVariableDeclaration QueryOperator*
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="8ad58-3514">たとえば、次のクエリは、ワシントン州の顧客によって行われたすべての注文の合計を集計します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3514">For example, the following query aggregates the total of all the orders placed by customers in Washington:</span></span>

```vb
Dim orderTotals = _
    From cust In Customers _
    Where cust.State = "WA" _
    Aggregate order In cust.Orders _
    Into Sum(order.Total)
```

<span data-ttu-id="8ad58-3515">このクエリの結果は、`Customer` として型指定された `cust` という名前のプロパティと、`Integer` として型指定された `Sum` という名前のプロパティを持つ匿名型を持つコレクションです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3515">The result of this query is a collection whose element type is an anonymous type with a property named `cust` typed as `Customer` and a property named `Sum` typed as `Integer`.</span></span>

<span data-ttu-id="8ad58-3516">@No__t-0 とは異なり、`Aggregate` 句と `Into` 句の間に追加のクエリ演算子を配置できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3516">Unlike `Group By`, additional query operators can be placed between the `Aggregate` and `Into` clauses.</span></span> <span data-ttu-id="8ad58-3517">@No__t-0 句と `Into` 句の末尾の間に、スコープ内のすべての範囲変数を指定します。これには、`Aggregate` 句で宣言された変数も使用できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3517">Between an `Aggregate` clause and the end of the `Into` clause, all range variables in scope, including those declared by the `Aggregate` clause can be used.</span></span> <span data-ttu-id="8ad58-3518">たとえば、次のクエリでは、2006より前に、ワシントン州の顧客によって行われたすべての注文の合計を集計しています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3518">For example, the following query aggregates the sum total of all the orders placed by customers in Washington before 2006:</span></span>

```vb
Dim orderTotals = _
    From cust In Customers _
    Where cust.State = "WA" _
    Aggregate order In cust.Orders _
    Where order.Date <= #01/01/2006# _
    Into Sum = Sum(order.Total)
```

<span data-ttu-id="8ad58-3519">@No__t-0 演算子を使用してクエリ式を開始することもできます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3519">The `Aggregate` operator can also be used to start a query expression.</span></span> <span data-ttu-id="8ad58-3520">この場合、クエリ式の結果は、`Into` 句によって計算された単一の値になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3520">In this case, the result of the query expression will be the single value computed by the `Into` clause.</span></span> <span data-ttu-id="8ad58-3521">たとえば、次のクエリでは、2006年1月1日より前のすべての注文合計の合計が計算されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3521">For example, the following query calculates the sum of all the order totals before January 1st, 2006:</span></span>

```vb
Dim ordersTotal = _
    Aggregate order In Orders _
    Where order.Date <= #01/01/2006# _
    Into Sum(order.Total)
```

<span data-ttu-id="8ad58-3522">クエリの結果は、単一の @no__t 0 の値になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3522">The result of the query is a single `Integer` value.</span></span> <span data-ttu-id="8ad58-3523">@No__t-0 クエリ演算子は常に使用できます (ただし、式を有効にするには集計関数も使用できる必要があります)。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3523">An `Aggregate` query operator is always available (although the aggregate function must be also be available for the expression to be valid).</span></span> <span data-ttu-id="8ad58-3524">コード</span><span class="sxs-lookup"><span data-stu-id="8ad58-3524">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    Aggregate x In xs _
    Where x < 5 _
    Into Sum()
```

<span data-ttu-id="8ad58-3525">通常はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3525">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Where(Function(x) x < 5).Sum()
```

<span data-ttu-id="8ad58-3526">__メモ。__  `Aggregate`</span><span class="sxs-lookup"><span data-stu-id="8ad58-3526">__Note.__ `Aggregate`</span></span> <span data-ttu-id="8ad58-3527">と `Into` は予約語ではありません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3527">and `Into` are not reserved words.</span></span>


### <a name="group-join-query-operator"></a><span data-ttu-id="8ad58-3528">Group Join クエリ演算子</span><span class="sxs-lookup"><span data-stu-id="8ad58-3528">Group Join Query Operator</span></span>

<span data-ttu-id="8ad58-3529">@No__t-0 クエリ演算子は、`Join` および `Group By` クエリ演算子の関数を1つの演算子に結合します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3529">The `Group Join` query operator combines the functions of the `Join` and `Group By` query operators into a single operator.</span></span> <span data-ttu-id="8ad58-3530">`Group Join` は、要素から抽出された一致するキーに基づいて2つのコレクションを結合し、結合の左側の特定の要素に一致する結合の右側にあるすべての要素をグループ化します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3530">`Group Join` joins two collections based on matching keys extracted from the elements, grouping together all of the elements on the right side of the join that match a particular element on the left side of the join.</span></span> <span data-ttu-id="8ad58-3531">したがって、演算子は一連の階層的な結果を生成します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3531">Thus, the operator produces a set of hierarchical results.</span></span>

```antlr
GroupJoinQueryOperator
    : LineTerminator? 'Group' 'Join' LineTerminator? CollectionRangeVariableDeclaration
      JoinOrGroupJoinQueryOperator? LineTerminator? 'On' LineTerminator? JoinConditionList
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="8ad58-3532">たとえば、次のクエリでは、1人の顧客の名前、すべての注文のグループ、およびそれらすべての注文の合計金額を含む要素が生成されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3532">For example, the following query produces elements that contain a single customer's name, a group of all of their orders, and the total amount of all of those orders:</span></span>

```vb
Dim custsWithOrders = _
    From cust In Customers _
    Group Join order In Orders On cust.ID Equals order.CustomerID _ 
    Into Orders = Group, OrdersTotal = Sum(order.Total) _
    Select cust.Name, Orders, OrdersTotal
```

<span data-ttu-id="8ad58-3533">このクエリの結果は、3つのプロパティを持つ匿名型である要素型を持つコレクションです。 @no__t 0 の場合は、`String` として型指定され、`Orders` は要素の型が @no__t 3 で、@no__t が `OrdersTotal` であるコレクションとして型指定されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3533">The result of the query is a collection whose element type is an anonymous type with three properties: `Name`, typed as `String`, `Orders` typed as a collection whose element type is `Order`, and `OrdersTotal`, typed as `Integer`.</span></span> <span data-ttu-id="8ad58-3534">@No__t-0 クエリ演算子は、コレクション型にメソッドが含まれている場合にのみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3534">A `Group Join` query operator is supported only if the collection type contains the method:</span></span>

```vb
Function GroupJoin(inner As CS, _
                         outerSelector As Func(Of T, K), _
                         innerSelector As Func(Of S, K), _
                         resultSelector As Func(Of T, CS, R)) As CR
```

<span data-ttu-id="8ad58-3535">コード</span><span class="sxs-lookup"><span data-stu-id="8ad58-3535">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs _
            Group Join y in ys On x Equals y _
            Into g = Group _
            ...
```

<span data-ttu-id="8ad58-3536">通常はに変換されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3536">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = _
    xs.GroupJoin( _
        ys, _
        Function(x As Integer) x, _
        Function(y As Integer) y, _
        Function(x, group) New With {x, .g = group})...
```

<span data-ttu-id="8ad58-3537">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3537">__Note.__</span></span> <span data-ttu-id="8ad58-3538">`Group`、`Join`、`Into` は予約語ではありません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3538">`Group`, `Join`, and `Into` are not reserved words.</span></span>


## <a name="conditional-expressions"></a><span data-ttu-id="8ad58-3539">条件式</span><span class="sxs-lookup"><span data-stu-id="8ad58-3539">Conditional Expressions</span></span>

<span data-ttu-id="8ad58-3540">条件付き `If` 式は、式をテストして値を返します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3540">A conditional `If` expression tests an expression and returns a value.</span></span>

```antlr
ConditionalExpression
    : 'If' OpenParenthesis BooleanExpression Comma Expression Comma Expression CloseParenthesis
    | 'If' OpenParenthesis Expression Comma Expression CloseParenthesis
    ;
```

<span data-ttu-id="8ad58-3541">ただし、`IIF` ランタイム関数とは異なり、条件式は必要に応じてそのオペランドだけを評価します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3541">Unlike the `IIF` runtime function, however, a conditional expression only evaluates its operands if necessary.</span></span> <span data-ttu-id="8ad58-3542">したがって、たとえば、`c` の値が `Nothing` の場合、式 `If(c Is Nothing, c.Name, "Unknown")` は例外をスローしません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3542">Thus, for example, the expression `If(c Is Nothing, c.Name, "Unknown")` will not throw an exception if the value of `c` is `Nothing`.</span></span> <span data-ttu-id="8ad58-3543">条件式には2つの形式があります。1つは2つのオペランドを受け取り、もう1つは3つのオペランドを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3543">The conditional expression has two forms: one that takes two operands and one that takes three operands.</span></span>

<span data-ttu-id="8ad58-3544">3つのオペランドが指定されている場合、3つの式はすべて値として分類する必要があり、最初のオペランドはブール式である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3544">If three operands are provided, all three expressions must be classified as values, and the first operand must be a Boolean expression.</span></span> <span data-ttu-id="8ad58-3545">式の結果が true の場合、2番目の式は演算子の結果になります。それ以外の場合は、3番目の式が演算子の結果になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3545">If the result is of the expression is true, then the second expression will be the result of the operator, otherwise the third expression will be the result of the operator.</span></span> <span data-ttu-id="8ad58-3546">式の結果型は、2番目と3番目の式の型の間で最も優先度の高い型になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3546">The result type of the expression is the dominant type between the types of the second and third expression.</span></span> <span data-ttu-id="8ad58-3547">優先度の高い型がない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3547">If there is no dominant type, then a compile-time error occurs.</span></span>

<span data-ttu-id="8ad58-3548">2つのオペランドを指定する場合は、両方のオペランドが値として分類され、1番目のオペランドが参照型または null 許容値型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3548">If two operands are provided, both of the operands must be classified as values, and the first operand must be either a reference type or a nullable value type.</span></span> <span data-ttu-id="8ad58-3549">式 `If(x, y)` は、次の2つの例外を除き、式が-1 @no__t のように評価されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3549">The expression `If(x, y)` is then evaluated as if was the expression `If(x IsNot Nothing, x, y)`, with two exceptions.</span></span> <span data-ttu-id="8ad58-3550">最初の式は1回だけ評価されます。2番目のオペランドの型が null 非許容の値型で、最初のオペランドの型がの場合は、結果の優先度の高い型を決定するときに、最初のオペランドの型から `?` が削除されます。式の型。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3550">First, the first expression is only ever evaluated once, and second, if the second operand's type is a non-nullable value type and the first operand's type is, the `?` is removed from the type of the first operand when determining the dominant type for the result type of the expression.</span></span> <span data-ttu-id="8ad58-3551">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3551">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim x?, y As Integer
        Dim a?, b As Long

        a = If(x, a)        ' Result type: Long?
        y = If(x, 0)        ' Result type: Integer
    End Sub
End Module
```

<span data-ttu-id="8ad58-3552">式の両方の形式で、オペランドが 0 @no__t 場合は、その型を使用して、優先される型を決定しません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3552">In both forms of the expression, if an operand is `Nothing`, its type is not used to determine the dominant type.</span></span> <span data-ttu-id="8ad58-3553">式 `If(<expression>, Nothing, Nothing)` の場合、優先される型は `Object` と見なされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3553">In the case of the expression `If(<expression>, Nothing, Nothing)`, the dominant type is considered to be `Object`.</span></span>


## <a name="xml-literal-expressions"></a><span data-ttu-id="8ad58-3554">XML リテラル式</span><span class="sxs-lookup"><span data-stu-id="8ad58-3554">XML Literal Expressions</span></span>

<span data-ttu-id="8ad58-3555">XML リテラル式は、XML (拡張マークアップ言語) 1.0 値を表します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3555">An XML literal expression represents an XML (eXtensible Markup Language) 1.0 value.</span></span>

```antlr
XMLLiteralExpression
    : XMLDocument
    | XMLElement
    | XMLProcessingInstruction
    | XMLComment
    | XMLCDATASection
    ;
```

<span data-ttu-id="8ad58-3556">XML リテラル式の結果は、`System.Xml.Linq` 名前空間のいずれかの型として型指定された値になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3556">The result of an XML literal expression is a value typed as one of the types from the `System.Xml.Linq` namespace.</span></span> <span data-ttu-id="8ad58-3557">その名前空間の型が使用できない場合は、XML リテラル式によってコンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3557">If the types in that namespace are not available, then an XML literal expression will cause a compile-time error.</span></span> <span data-ttu-id="8ad58-3558">値は、XML リテラル式から変換されたコンストラクター呼び出しによって生成されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3558">The values are generated through constructor calls translated from the XML literal expression.</span></span> <span data-ttu-id="8ad58-3559">たとえば、コードは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3559">For example, the code:</span></span>

```vb
Dim book As System.Xml.Linq.XElement = _
    <book title="My book"></book>
```

<span data-ttu-id="8ad58-3560">は、次のコードとほぼ同じです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3560">is roughly equivalent to the code:</span></span>

```vb
Dim book As System.Xml.Linq.XElement = _
    New System.Xml.Linq.XElement( _
        "book", _
        New System.Xml.Linq.XAttribute("title", "My book"))
```

<span data-ttu-id="8ad58-3561">Xml リテラル式は、xml ドキュメント、XML 要素、XML 処理命令、XML コメント、または CDATA セクションの形式を取ることができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3561">An XML literal expression can take the form of an XML document, an XML element, an XML processing instruction, an XML comment, or a CDATA section.</span></span>

<span data-ttu-id="8ad58-3562">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3562">__Note.__</span></span> <span data-ttu-id="8ad58-3563">この仕様には、Visual Basic 言語の動作を説明するのに十分な XML 記述が含まれています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3563">This specification contains only enough of a description of XML to describe the behavior of the Visual Basic language.</span></span> <span data-ttu-id="8ad58-3564">XML の詳細については、@no__t 0 を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3564">More information on XML can be found at http://www.w3.org/TR/REC-xml/.</span></span>


### <a name="lexical-rules"></a><span data-ttu-id="8ad58-3565">字句規則</span><span class="sxs-lookup"><span data-stu-id="8ad58-3565">Lexical rules</span></span>

```antlr
XMLCharacter
    : '<Unicode tab character (0x0009)>'
    | '<Unicode linefeed character (0x000A)>'
    | '<Unicode carriage return character (0x000D)>'
    | '<Unicode characters 0x0020 - 0xD7FF>'
    | '<Unicode characters 0xE000 - 0xFFFD>'
    | '<Unicode characters 0x10000 - 0x10FFFF>'
    ;

XMLString
    : XMLCharacter+
    ;

XMLWhitespace
    : XMLWhitespaceCharacter+
    ;

XMLWhitespaceCharacter
    : '<Unicode carriage return character (0x000D)>'
    | '<Unicode linefeed character (0x000A)>'
    | '<Unicode space character (0x0020)>'
    | '<Unicode tab character (0x0009)>'
    ;

XMLNameCharacter
    : XMLLetter
    | XMLDigit
    | '.'
    | '-'
    | '_'
    | ':'
    | XMLCombiningCharacter
    | XMLExtender
    ;

XMLNameStartCharacter
    : XMLLetter
    | '_'
    | ':'
    ;

XMLName
    : XMLNameStartCharacter XMLNameCharacter*
    ;

XMLLetter
    : '<Unicode character as defined in the Letter production of the XML 1.0 specification>'
    ;

XMLDigit
    : '<Unicode character as defined in the Digit production of the XML 1.0 specification>'
    ;

XMLCombiningCharacter
    : '<Unicode character as defined in the CombiningChar production of the XML 1.0 specification>'
    ;

XMLExtender
    : '<Unicode character as defined in the Extender production of the XML 1.0 specification>'
    ;
```

<span data-ttu-id="8ad58-3566">XML リテラル式は、通常の Visual Basic コードの字句規則ではなく、XML の字句規則を使用して解釈されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3566">XML literal expressions are interpreted using the lexical rules of XML instead of the lexical rules of regular Visual Basic code.</span></span> <span data-ttu-id="8ad58-3567">2つの規則セットは、一般に次の点で異なります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3567">The two sets of rules generally differ in the following ways:</span></span>

* <span data-ttu-id="8ad58-3568">XML では、空白文字が重要です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3568">White space is significant in XML.</span></span> <span data-ttu-id="8ad58-3569">その結果、XML リテラル式の文法では、空白が許可されている場所が明示的に示されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3569">As a result, the grammar for XML literal expressions explicitly states where white space is allowed.</span></span> <span data-ttu-id="8ad58-3570">要素内の文字データのコンテキストで発生する場合を除き、空白は保持されません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3570">Whitespace is not preserved, except when it occurs in the context of character data within an element.</span></span> <span data-ttu-id="8ad58-3571">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3571">For example:</span></span>

  ```vb
  ' The following element preserves no whitespace
  Dim e1 = _
      <customer>
          <name>Bob</>
      </>

  ' The following element preserves all of the whitespace
  Dim e2 = _
      <customer>
          Bob
      </>
  ```

* <span data-ttu-id="8ad58-3572">XML の行末の空白文字は、XML 仕様に従って正規化されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3572">XML end-of-line whitespace is normalized according to the XML specification.</span></span>

* <span data-ttu-id="8ad58-3573">XML は大文字と小文字を区別します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3573">XML is case-sensitive.</span></span> <span data-ttu-id="8ad58-3574">キーワードは大文字と小文字を正確に一致させる必要があります。そうしないと、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3574">Keywords must match casing exactly, or else a compile-time error will occur.</span></span>

* <span data-ttu-id="8ad58-3575">行ターミネータは、XML の空白文字と見なされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3575">Line terminators are considered white space in XML.</span></span> <span data-ttu-id="8ad58-3576">そのため、XML リテラル式には行連結文字は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3576">As a result, no line-continuation characters are needed in XML literal expressions.</span></span>

* <span data-ttu-id="8ad58-3577">XML では、全角文字は使用できません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3577">XML does not accept full-width characters.</span></span> <span data-ttu-id="8ad58-3578">全角文字が使用されている場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3578">If full-width characters are used, a compile-time error will occur.</span></span>


### <a name="embedded-expressions"></a><span data-ttu-id="8ad58-3579">埋め込み式</span><span class="sxs-lookup"><span data-stu-id="8ad58-3579">Embedded expressions</span></span>

<span data-ttu-id="8ad58-3580">XML リテラル式には、*埋め込み式*を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3580">XML literal expressions can contain *embedded expressions*.</span></span> <span data-ttu-id="8ad58-3581">埋め込み式は Visual Basic 式で、評価され、埋め込み式の場所に1つ以上の値を入力するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3581">An embedded expression is a Visual Basic expression that is evaluated and used to fill in one or more values at the location of embedded expression.</span></span>

```antlr
XMLEmbeddedExpression
    : '<' '%' '=' LineTerminator? Expression LineTerminator? '%' '>'
    ;
```

<span data-ttu-id="8ad58-3582">たとえば、次のコードでは、XML 要素の値として文字列 `John Smith` に配置します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3582">For example, the following code places the string `John Smith` as the value of the XML element:</span></span>

```vb
Dim name as String = "John Smith"
Dim element As System.Xml.Linq.XElement = <customer><%= name %></customer>
```

<span data-ttu-id="8ad58-3583">式は、さまざまなコンテキストに埋め込むことができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3583">Expressions can be embedded in a number of contexts.</span></span> <span data-ttu-id="8ad58-3584">たとえば、次のコードでは `customer` という名前の要素が生成されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3584">For example, the following code produces an element named `customer`:</span></span>

```vb
Dim name As String = "customer"
Dim element As System.Xml.Linq.XElement = <<%= name %>>John Smith</>
```

<span data-ttu-id="8ad58-3585">埋め込み式を使用できる各コンテキストは、受け入れられる型を指定します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3585">Each context where an embedded expression can be used specifies the types that will be accepted.</span></span> <span data-ttu-id="8ad58-3586">埋め込み式の式部分のコンテキスト内では、Visual Basic コードの通常の構文規則が引き続き適用されます。たとえば、行の継続を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3586">When within the context of the expression part of an embedded expression, the normal lexical rules for Visual Basic code still apply so, for example, line continuations must be used:</span></span>

```vb
' Visual Basic expression uses line continuation, XML does not
Dim element As System.Xml.Linq.XElement = _
    <<%= name & _
          name %>>John 
                     Smith</>
```


### <a name="xml-documents"></a><span data-ttu-id="8ad58-3587">XML ドキュメント</span><span class="sxs-lookup"><span data-stu-id="8ad58-3587">XML Documents</span></span>

```antlr
XMLDocument
    : XMLDocumentPrologue XMLMisc* XMLDocumentBody XMLMisc*
    ;

XMLDocumentPrologue
    : '<' '?' 'xml' XMLVersion XMLEncoding? XMLStandalone? XMLWhitespace? '?' '>'
    ;

XMLVersion
    : XMLWhitespace 'version' XMLWhitespace? '=' XMLWhitespace? XMLVersionNumberValue
    ;

XMLVersionNumberValue
    : SingleQuoteCharacter '1' '.' '0' SingleQuoteCharacter
    | DoubleQuoteCharacter '1' '.' '0' DoubleQuoteCharacter
    ;

XMLEncoding
    : XMLWhitespace 'encoding' XMLWhitespace? '=' XMLWhitespace? XMLEncodingNameValue
    ;

XMLEncodingNameValue
    : SingleQuoteCharacter XMLEncodingName SingleQuoteCharacter
    | DoubleQuoteCharacter XMLEncodingName DoubleQuoteCharacter
    ;

XMLEncodingName
    : XMLLatinAlphaCharacter XMLEncodingNameCharacter*
    ;

XMLEncodingNameCharacter
    : XMLUnderscoreCharacter
    | XMLLatinAlphaCharacter
    | XMLNumericCharacter
    | XMLPeriodCharacter
    | XMLDashCharacter
    ;

XMLLatinAlphaCharacter
    : '<Unicode Latin alphabetic character (0x0041-0x005a, 0x0061-0x007a)>'
    ;

XMLNumericCharacter
    : '<Unicode digit character (0x0030-0x0039)>'
    ;

XMLHexNumericCharacter
    : XMLNumericCharacter
    | '<Unicode Latin hex alphabetic character (0x0041-0x0046, 0x0061-0x0066)>'
    ;

XMLPeriodCharacter
    : '<Unicode period character (0x002e)>'
    ;

XMLUnderscoreCharacter
    : '<Unicode underscore character (0x005f)>'
    ;

XMLDashCharacter
    : '<Unicode dash character (0x002d)>'
    ;

XMLStandalone
    : XMLWhitespace 'standalone' XMLWhitespace? '=' XMLWhitespace? XMLYesNoValue
    ;

XMLYesNoValue
    : SingleQuoteCharacter XMLYesNo SingleQuoteCharacter
    | DoubleQuoteCharacter XMLYesNo DoubleQuoteCharacter
    ;

XMLYesNo
    : 'yes'
    | 'no'
    ;

XMLMisc
    : XMLComment
    | XMLProcessingInstruction
    | XMLWhitespace
    ;

XMLDocumentBody
    : XMLElement
    | XMLEmbeddedExpression
    ;
```

<span data-ttu-id="8ad58-3588">XML ドキュメントでは、`System.Xml.Linq.XDocument` として型指定された値が返されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3588">An XML document results in a value typed as `System.Xml.Linq.XDocument`.</span></span> <span data-ttu-id="8ad58-3589">Xml 1.0 仕様とは異なり、xml ドキュメントプロローグを指定するには xml リテラル式の XML ドキュメントが必要です。XML ドキュメントプロローグのない XML リテラル式は、個々のエンティティとして解釈されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3589">Unlike the XML 1.0 specification, XML documents in XML literal expressions are required to specify the XML document prologue; XML literal expressions without the XML document prologue are interpreted as their individual entity.</span></span> <span data-ttu-id="8ad58-3590">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3590">For example:</span></span>

```vb
Dim doc As System.Xml.Linq.XDocument = _
    <?xml version="1.0"?>
    <?instruction?>
    <customer>Bob</>

Dim pi As System.Xml.Linq.XProcessingInstruction = _
    <?instruction?>
```

<span data-ttu-id="8ad58-3591">XML ドキュメントには、任意の型を持つことができる埋め込み式を含めることができます。ただし、実行時には、オブジェクトが `XDocument` コンストラクターの要件を満たしている必要があります。指定されていない場合は、実行時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3591">An XML document can contain an embedded expression whose type can be any type; at runtime, however, the object must satisfy the requirements of the `XDocument` constructor or a run-time error will occur.</span></span>

<span data-ttu-id="8ad58-3592">通常の XML とは異なり、XML ドキュメント式では Dtd (ドキュメント型宣言) はサポートされません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3592">Unlike regular XML, XML document expressions do not support DTDs (Document Type Declarations).</span></span> <span data-ttu-id="8ad58-3593">また、指定した場合、encoding 属性は無視されます。これは、Xml リテラル式のエンコーディングがソースファイル自体のエンコーディングと常に同じであるためです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3593">Also, the encoding attribute, if supplied, will be ignored since the encoding of the Xml literal expression is always the same as the encoding of the source file itself.</span></span>

<span data-ttu-id="8ad58-3594">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3594">__Note.__</span></span> <span data-ttu-id="8ad58-3595">Encoding 属性は無視されますが、有効な属性であるため、有効な Xml 1.0 ドキュメントをソースコードに含めることができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3595">Although the encoding attribute is ignored, it is still valid attribute in order to maintain the ability to include any valid Xml 1.0 documents in source code.</span></span>


### <a name="xml-elements"></a><span data-ttu-id="8ad58-3596">XML 要素</span><span class="sxs-lookup"><span data-stu-id="8ad58-3596">XML Elements</span></span>

```antlr
XMLElement
    : XMLEmptyElement
    | XMLElementStart XMLContent XMLElementEnd
    ;

XMLEmptyElement
    : '<' XMLQualifiedNameOrExpression XMLAttribute* XMLWhitespace? '/' '>'
    ;

XMLElementStart
    : '<' XMLQualifiedNameOrExpression XMLAttribute* XMLWhitespace? '>'
    ;

XMLElementEnd
    : '<' '/' '>'
    | '<' '/' XMLQualifiedName XMLWhitespace? '>'
    ;

XMLContent
    : XMLCharacterData? ( XMLNestedContent XMLCharacterData? )+
    ;

XMLCharacterData
    : '<Any XMLCharacterDataString that does not contain the string "]]>">'
    ;

XMLCharacterDataString
    : '<Any Unicode character except < or &>'+
    ;

XMLNestedContent
    : XMLElement
    | XMLReference
    | XMLCDATASection
    | XMLProcessingInstruction
    | XMLComment
    | XMLEmbeddedExpression
    ;

XMLAttribute
    : XMLWhitespace XMLAttributeName XMLWhitespace? '=' XMLWhitespace? XMLAttributeValue
    | XMLWhitespace XMLEmbeddedExpression
    ;

XMLAttributeName
    : XMLQualifiedNameOrExpression
    | XMLNamespaceAttributeName
    ;

XMLAttributeValue
    : DoubleQuoteCharacter XMLAttributeDoubleQuoteValueCharacter* DoubleQuoteCharacter
    | SingleQuoteCharacter XMLAttributeSingleQuoteValueCharacter* SingleQuoteCharacter
    | XMLEmbeddedExpression
    ;

XMLAttributeDoubleQuoteValueCharacter
    : '<Any XMLCharacter except <, &, or DoubleQuoteCharacter>'
    | XMLReference
    ;

XMLAttributeSingleQuoteValueCharacter
    : '<Any XMLCharacter except <, &, or SingleQuoteCharacter>'
    | XMLReference
    ;

XMLReference
    : XMLEntityReference
    | XMLCharacterReference
    ;

XMLEntityReference
    : '&' XMLEntityName ';'
    ;

XMLEntityName
    : 'lt' | 'gt' | 'amp' | 'apos' | 'quot'
    ;

XMLCharacterReference
    : '&' '#' XMLNumericCharacter+ ';'
    | '&' '#' 'x' XMLHexNumericCharacter+ ';'
    ;
```

<span data-ttu-id="8ad58-3597">XML 要素は、`System.Xml.Linq.XElement` として型指定された値になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3597">An XML element results in a value typed as `System.Xml.Linq.XElement`.</span></span> <span data-ttu-id="8ad58-3598">通常の XML とは異なり、XML 要素は終了タグ内の名前を省略し、現在最も入れ子になっている要素を閉じることができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3598">Unlike regular XML, XML elements can omit the name in the closing tag and the current most-nested element will be closed.</span></span> <span data-ttu-id="8ad58-3599">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3599">For example:</span></span>

```vb
Dim name = <name>Bob</>
```

<span data-ttu-id="8ad58-3600">XML 要素の属性宣言は、`System.Xml.Linq.XAttribute` として型指定された値になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3600">Attribute declarations in an XML element result in values typed as `System.Xml.Linq.XAttribute`.</span></span> <span data-ttu-id="8ad58-3601">属性値は、XML 仕様に従って正規化されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3601">Attribute values are normalized according to the XML specification.</span></span> <span data-ttu-id="8ad58-3602">属性の値が @no__t 0 の場合、属性は作成されないため、`Nothing` の属性値式をチェックする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3602">When the value of an attribute is `Nothing` the attribute will not be created, so the attribute value expression will not have to be checked for `Nothing`.</span></span> <span data-ttu-id="8ad58-3603">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3603">For example:</span></span>

```vb
Dim expr = Nothing

' Throws null argument exception
Dim direct = New System.Xml.Linq.XElement( _
    "Name", _
    New System.Xml.Linq.XAttribute("Length", expr))

' Doesn't throw exception, the result is <Name/>
Dim literal = <Name Length=<%= expr %>/>
```

<span data-ttu-id="8ad58-3604">XML 要素と属性には、次の場所で入れ子になった式を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3604">XML elements and attributes can contain nested expressions in the following places:</span></span>

<span data-ttu-id="8ad58-3605">要素の名前。この場合、埋め込み式は `System.Xml.Linq.XName` に暗黙的に変換できる型の値である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3605">The name of the element, in which case the embedded expression must be a value of a type implicitly convertible to `System.Xml.Linq.XName`.</span></span> <span data-ttu-id="8ad58-3606">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3606">For example:</span></span>

```vb
Dim name = <<%= "name" %>>Bob</>
```

<span data-ttu-id="8ad58-3607">要素の属性の名前。この場合、埋め込み式は `System.Xml.Linq.XName` に暗黙的に変換できる型の値である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3607">The name of an attribute of the element, in which case the embedded expression must be a value of a type implicitly convertible to `System.Xml.Linq.XName`.</span></span> <span data-ttu-id="8ad58-3608">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3608">For example:</span></span>

```vb
Dim name = <name <%= "length" %>="3">Bob</>
```

<span data-ttu-id="8ad58-3609">要素の属性の値。この場合、埋め込み式には任意の型の値を指定できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3609">The value of an attribute of the element, in which case the embedded expression can be a value of any type.</span></span> <span data-ttu-id="8ad58-3610">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3610">For example:</span></span>

```vb
Dim name = <name length=<%= 3 %>>Bob</>
```

<span data-ttu-id="8ad58-3611">要素の属性。この場合、埋め込み式には任意の型の値を指定できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3611">An attribute of the element, in which case the embedded expression can be a value of any type.</span></span> <span data-ttu-id="8ad58-3612">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3612">For example:</span></span>

```vb
Dim name = <name <%= new XAttribute("length", 3) %>>Bob</>
```

<span data-ttu-id="8ad58-3613">要素の内容。この場合、埋め込み式には任意の型の値を指定できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3613">The content of the element, in which case the embedded expression can be a value of any type.</span></span> <span data-ttu-id="8ad58-3614">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3614">For example:</span></span>

```vb
Dim name = <name><%= "Bob" %></>
```

<span data-ttu-id="8ad58-3615">埋め込み式の型が `Object()` の場合、配列は paramarray として `XElement` コンストラクターに渡されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3615">If the type of the embedded expression is `Object()`, the array will be passed as a paramarray to the `XElement` constructor.</span></span>


### <a name="xml-namespaces"></a><span data-ttu-id="8ad58-3616">XML 名前空間</span><span class="sxs-lookup"><span data-stu-id="8ad58-3616">XML Namespaces</span></span>

<span data-ttu-id="8ad58-3617">Xml 要素には、xml 名前空間1.0 仕様で定義されている XML 名前空間宣言を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3617">XML elements can contain XML namespace declarations, as defined by the XML namespaces 1.0 specification.</span></span>

```antlr
XMLNamespaceAttributeName
    : XMLPrefixedNamespaceAttributeName
    | XMLDefaultNamespaceAttributeName
    ;

XMLPrefixedNamespaceAttributeName
    : 'xmlns' ':' XMLNamespaceName
    ;

XMLDefaultNamespaceAttributeName
    : 'xmlns'
    ;

XMLNamespaceName
    : XMLNamespaceNameStartCharacter XMLNamespaceNameCharacter*
    ;

XMLNamespaceNameStartCharacter
    : '<Any XMLNameCharacter except :>'
    ;

XMLNamespaceNameCharacter
    : XMLLetter
    | '_'
    ;

XMLQualifiedNameOrExpression
    : XMLQualifiedName
    | XMLEmbeddedExpression
    ;

XMLQualifiedName
    : XMLPrefixedName
    | XMLUnprefixedName
    ;

XMLPrefixedName
    : XMLNamespaceName ':' XMLNamespaceName
    ;

XMLUnprefixedName
    : XMLNamespaceName
    ;
```

<span data-ttu-id="8ad58-3618">@No__t-0 および `xmlns` という名前空間の定義に関する制限が適用され、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3618">The restrictions on defining the namespaces `xml` and `xmlns` are enforced and will produce compile-time errors.</span></span> <span data-ttu-id="8ad58-3619">XML 名前空間の宣言では、その値に対して埋め込み式を使用することはできません。指定された値は、空でない文字列リテラルである必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3619">XML namespace declarations cannot have an embedded expression for their value; the value supplied must be a non-empty string literal.</span></span> <span data-ttu-id="8ad58-3620">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3620">For example:</span></span>

```vb
' Declares a valid namespace
Dim customer = <db:customer xmlns:db="http://example.org/database">Bob</>

' Error: xmlns cannot be re-defined
Dim bad1 = <elem xmlns:xmlns="http://example.org/namespace"/>

' Error: cannot have an embedded expression
Dim bad2 = <elem xmlns:db=<%= "http://example.org/database" %>>Bob</>
```

<span data-ttu-id="8ad58-3621">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3621">__Note.__</span></span> <span data-ttu-id="8ad58-3622">この仕様には、Visual Basic 言語の動作を記述するのに十分な XML 名前空間の説明が含まれています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3622">This specification contains only enough of a description of XML namespace to describe the behavior of the Visual Basic language.</span></span> <span data-ttu-id="8ad58-3623">XML 名前空間の詳細については、@no__t 0 を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3623">More information on XML namespaces can be found at http://www.w3.org/TR/REC-xml-names/.</span></span>

<span data-ttu-id="8ad58-3624">XML 要素名と属性名は、名前空間名を使用して修飾できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3624">XML element and attribute names can be qualified using namespace names.</span></span> <span data-ttu-id="8ad58-3625">名前空間は、通常の XML のようにバインドされます。ただし、ファイルレベルで宣言された名前空間インポートは、その宣言を囲むコンテキストで宣言されていると見なされます。これは、コンパイルによって宣言された名前空間インポートによって囲まれています。environment.</span><span class="sxs-lookup"><span data-stu-id="8ad58-3625">Namespaces are bound as in regular XML, with the exception that any namespace imports declared at the file level are considered to be declared in a context enclosing the declaration, which is itself enclosed by any namespace imports declared by the compilation environment.</span></span> <span data-ttu-id="8ad58-3626">名前空間の名前が見つからない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3626">If a namespace name cannot be found, a compile-time error occurs.</span></span> <span data-ttu-id="8ad58-3627">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3627">For example:</span></span>

```vb
Imports System.Xml.Linq
Imports <xmlns:db="http://example.org/database">

Module Test
    Sub Main()
        ' Binds to the imported namespace above.
        Dim c1 = <db:customer>Bob</>

        ' Binds to the namespace declaration in the element
        Dim c2 = _
            <db:customer xmlns:db="http://example.org/database-other">Mary</>

        ' Binds to the inner namespace declaration
        Dim c3 = _
            <database xmlns:db="http://example.org/database-one">
                <db:customer xmlns:db="http://example.org/database-two">Joe</>
            </>

        ' Error: namespace db2 cannot be found
        Dim c4 = _
            <db2:customer>Jim</>
    End Sub
End Module
```

<span data-ttu-id="8ad58-3628">要素で宣言された XML 名前空間は、埋め込み式内の XML リテラルには適用されません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3628">XML namespaces declared in an element do not apply to XML literals inside embedded expressions.</span></span> <span data-ttu-id="8ad58-3629">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3629">For example:</span></span>

```vb
' Error: Namespace prefix 'db' is not declared
Dim customer = _
    <db:customer xmlns:db="http://example.org/database">
        <%= <db:customer>Bob</> %>
    </>
```

<span data-ttu-id="8ad58-3630">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3630">__Note.__</span></span> <span data-ttu-id="8ad58-3631">これは、埋め込み式が関数呼び出しを含む任意のものになる可能性があるためです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3631">This is because the embedded expression can be anything, including a function call.</span></span> <span data-ttu-id="8ad58-3632">関数呼び出しに XML リテラル式が含まれている場合は、XML 名前空間を適用するか無視するかをプログラマが想定するかどうかが明確ではありません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3632">If the function call contained an XML literal expression, it is not clear whether programmers would expect the XML namespace to be applied or ignored.</span></span>


### <a name="xml-processing-instructions"></a><span data-ttu-id="8ad58-3633">XML 処理命令</span><span class="sxs-lookup"><span data-stu-id="8ad58-3633">XML Processing Instructions</span></span>

<span data-ttu-id="8ad58-3634">XML 処理命令は、`System.Xml.Linq.XProcessingInstruction` として型指定された値になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3634">An XML processing instruction results in a value typed as `System.Xml.Linq.XProcessingInstruction`.</span></span> <span data-ttu-id="8ad58-3635">XML 処理命令は、処理命令内の有効な構文であるため、埋め込み式を含むことはできません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3635">XML processing instructions cannot contain embedded expressions, as they are valid syntax within the processing instruction.</span></span>

```antlr
XMLProcessingInstruction
    : '<' '?' XMLProcessingTarget ( XMLWhitespace XMLProcessingValue? )? '?' '>'
    ;

XMLProcessingTarget
    : '<Any XMLName except a casing permutation of the string "xml">'
    ;

XMLProcessingValue
    : '<Any XMLString that does not contain a question-mark followed by ">">'
    ;
```

### <a name="xml-comments"></a><span data-ttu-id="8ad58-3636">XML コメント</span><span class="sxs-lookup"><span data-stu-id="8ad58-3636">XML Comments</span></span>

<span data-ttu-id="8ad58-3637">XML コメントは、`System.Xml.Linq.XComment` として型指定された値になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3637">An XML comment results in a value typed as `System.Xml.Linq.XComment`.</span></span> <span data-ttu-id="8ad58-3638">XML コメントは、コメント内の有効な構文であるため、埋め込み式を含むことはできません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3638">XML comments cannot contain embedded expressions, as they are valid syntax within the comment.</span></span>

```antlr
XMLComment
    : '<' '!' '-' '-' XMLCommentCharacter* '-' '-' '>'
    ;

XMLCommentCharacter
    : '<Any XMLCharacter except dash (0x002D)>'
    | '-' '<Any XMLCharacter except dash (0x002D)>'
    ;
```

### <a name="cdata-sections"></a><span data-ttu-id="8ad58-3639">CDATA セクション</span><span class="sxs-lookup"><span data-stu-id="8ad58-3639">CDATA sections</span></span>

<span data-ttu-id="8ad58-3640">CDATA セクションは、`System.Xml.Linq.XCData` として型指定された値になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3640">A CDATA section results in a value typed as `System.Xml.Linq.XCData`.</span></span> <span data-ttu-id="8ad58-3641">Cdata セクションには、CDATA セクション内の有効な構文であるため、埋め込み式を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3641">CDATA sections cannot contain embedded expressions, as they are valid syntax within the CDATA section.</span></span>

```antlr
XMLCDATASection
    : '<' '!' ( 'CDATA' '[' XMLCDATASectionString? ']' )? '>'
    ;

XMLCDATASectionString
    : '<Any XMLString that does not contain the string "]]>">'
    ;
```

## <a name="xml-member-access-expressions"></a><span data-ttu-id="8ad58-3642">XML メンバーアクセス式</span><span class="sxs-lookup"><span data-stu-id="8ad58-3642">XML Member Access Expressions</span></span>

<span data-ttu-id="8ad58-3643">Xml メンバーアクセス式は、XML 値のメンバーにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3643">An XML member access expression accesses the members of an XML value.</span></span>

```antlr
XMLMemberAccessExpression
    : Expression '.' LineTerminator? '<' XMLQualifiedName '>'
    | Expression '.' LineTerminator? '@' LineTerminator? '<' XMLQualifiedName '>'
    | Expression '.' LineTerminator? '@' LineTerminator? IdentifierOrKeyword
    | Expression '.' '.' '.' LineTerminator? '<' XMLQualifiedName '>'
    ;
```

<span data-ttu-id="8ad58-3644">XML メンバーアクセス式には、次の3種類があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3644">There are three types of XML member access expressions:</span></span>

* <span data-ttu-id="8ad58-3645">*要素アクセス*。 XML 名は1つのドットに従います。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3645">*Element access*, in which an XML name follows a single dot.</span></span> <span data-ttu-id="8ad58-3646">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3646">For example:</span></span>

  ```vb
  Dim customer = _
      <customer>
          <name>Bob</>
      </>
  Dim customerName = customer.<name>.Value
  ```

  <span data-ttu-id="8ad58-3647">要素アクセスは、次の関数にマップされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3647">Element access maps to the function:</span></span>

  ```vb
  Function Elements(name As System.Xml.Linq.XName) As _
      System.Collections.Generic.IEnumerable(Of _
          System.Xml.Linq.XNode)
  ```

  <span data-ttu-id="8ad58-3648">上の例は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3648">So the above example is equivalent to:</span></span>

  ```vb
  Dim customerName = customer.Elements("name").Value
  ```

* <span data-ttu-id="8ad58-3649">*属性アクセス*。 Visual Basic 識別子がドットとアットマークの後に続く場合、または XML 名がドットとアットマークの後に続く場合。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3649">*Attribute access*, in which a Visual Basic identifier follows a dot and an at sign, or an XML name follows a dot and an at sign.</span></span> <span data-ttu-id="8ad58-3650">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3650">For example:</span></span>

  ```vb
  Dim customer = <customer age="30"/>
  Dim customerAge = customer.@age
  ```

  <span data-ttu-id="8ad58-3651">属性へのアクセスは、次の関数にマップされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3651">Attribute access maps to the function:</span></span>

  ```vb
  Function AttributeValue(name As System.Xml.Linq.XName) as String
  ```

  <span data-ttu-id="8ad58-3652">上の例は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3652">So the above example is equivalent to:</span></span>

  ```vb
  Dim customerAge = customer.AttributeValue("age")
  ```

  <span data-ttu-id="8ad58-3653">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3653">__Note.__</span></span> <span data-ttu-id="8ad58-3654">@No__t-0 拡張メソッド (および関連する拡張機能プロパティ `Value`) は、現在、どのアセンブリでも定義されていません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3654">The `AttributeValue` extension method (as well as the related extension property `Value`) is not currently defined in any assembly.</span></span> <span data-ttu-id="8ad58-3655">拡張機能のメンバーが必要な場合は、生成されるアセンブリに自動的に定義されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3655">If the extension members are needed, they are automatically defined in the assembly being produced.</span></span>

* <span data-ttu-id="8ad58-3656">*子孫アクセス*。 XML 名が3つのドットの後に続きます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3656">*Descendents access*, in which an XML names follows three dots.</span></span> <span data-ttu-id="8ad58-3657">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3657">For example:</span></span>

  ```vb
  Dim company = _
      <company>
          <customers>
              <customer>Bob</>
              <customer>Mary</>
              <customer>Joe</>
          </>
      </>
  Dim customers = company...<customer>
  ```

  <span data-ttu-id="8ad58-3658">子孫アクセスは、次の関数にマップされます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3658">Descendents access maps to the function:</span></span>

  ```vb
  Function Descendents(name As System.Xml.Linq.XName) As _
      System.Collections.Generic.IEnumerable(Of _
          System.Xml.Linq.XElement)
  ```

  <span data-ttu-id="8ad58-3659">上の例は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3659">So the above example is equivalent to:</span></span>

  ```vb
  Dim customers = company.Descendants("customer")
  ```

<span data-ttu-id="8ad58-3660">XML メンバーアクセス式の基本式は、値である必要があり、型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3660">The base expression of an XML member access expression must be a value and must be of the type:</span></span>

* <span data-ttu-id="8ad58-3661">要素または子孫がにアクセスする場合は、`System.Xml.Linq.XContainer` または派生型、または `System.Collections.Generic.IEnumerable(Of T)` または派生型。 `T` は `System.Xml.Linq.XContainer` または派生型です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3661">If an element or descendents access,  `System.Xml.Linq.XContainer` or a derived type, or `System.Collections.Generic.IEnumerable(Of T)` or a derived type, where `T` is `System.Xml.Linq.XContainer` or a derived type.</span></span>

* <span data-ttu-id="8ad58-3662">属性にアクセスする場合は、@no__t 0 または派生型、または `System.Collections.Generic.IEnumerable(Of T)` または派生型のいずれかを指定します。 `T` は `System.Xml.Linq.XElement` または派生型です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3662">If an attribute access, `System.Xml.Linq.XElement` or a derived type, or `System.Collections.Generic.IEnumerable(Of T)` or a derived type, where `T` is `System.Xml.Linq.XElement` or a derived type.</span></span>

<span data-ttu-id="8ad58-3663">XML メンバーアクセス式の名前を空にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3663">Names in XML member access expressions cannot be empty.</span></span> <span data-ttu-id="8ad58-3664">インポートで定義された名前空間を使用して、名前空間を修飾することができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3664">They can be namespace qualified, using any namespaces defined by imports.</span></span> <span data-ttu-id="8ad58-3665">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3665">For example:</span></span>

```vb
Imports <xmlns:db="http://example.org/database">

Module Test
    Sub Main()
        Dim customer = _
            <db:customer>
                <db:name>Bob</>
            </>
        Dim name = customer.<db:name>
    End Sub
End Module
```

<span data-ttu-id="8ad58-3666">XML メンバーアクセス式のドットの後、または山かっこと名前の間に空白を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3666">Whitespace is not allowed after the dot(s) in an XML member access expression, or between the angle brackets and the name.</span></span> <span data-ttu-id="8ad58-3667">以下に例を示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3667">For example:</span></span>

```vb
Dim customer = _
    <customer age="30">
        <name>Bob</>
    </>
' All the following are error cases
Dim age = customer.@ age
Dim name = customer.< name >
Dim names = customer...< name >
```

<span data-ttu-id="8ad58-3668">@No__t-0 名前空間の型が使用できない場合は、XML メンバーアクセス式によってコンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3668">If the types in the `System.Xml.Linq` namespace are not available, then an XML member access expression will cause a compile-time error.</span></span>


## <a name="await-operator"></a><span data-ttu-id="8ad58-3669">Await 演算子</span><span class="sxs-lookup"><span data-stu-id="8ad58-3669">Await Operator</span></span>

<span data-ttu-id="8ad58-3670">Await 演算子は、「[非同期メソッド](statements.md#async-methods)」で説明されている非同期メソッドに関連しています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3670">The await operator is related to async methods, which are described in Section [Async Methods](statements.md#async-methods).</span></span>

```antlr
AwaitOperatorExpression
    : 'Await' Expression
    ;
```

<span data-ttu-id="8ad58-3671">`Await` は、すぐ外側のメソッドまたはラムダ式が表示されている場合に、その単語が `Async` 修飾子を持ち、`Await` が `Async` 修飾子の後に続く場合は、予約語です。他の場所では予約されていません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3671">`Await` is a reserved word if the immediately enclosing method or lambda expression in which it appears has an `Async` modifier, and if the `Await` appears after that `Async` modifier; it is unreserved elsewhere.</span></span> <span data-ttu-id="8ad58-3672">プリプロセッサディレクティブでも予約されていません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3672">It is also unreserved in preprocessor directives.</span></span> <span data-ttu-id="8ad58-3673">Await 演算子は、予約語であるメソッドまたはラムダ式の本体でのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3673">The await operator is only allowed in the body of a method or lambda expressions where it is a reserved word.</span></span> <span data-ttu-id="8ad58-3674">すぐ外側のメソッドまたはラムダ内では、await 式は、`Catch` または `Finally` ブロックの本体内、または `SyncLock` ステートメントの本体の内部、またはクエリ式の内部には出現しません。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3674">Within the immediately enclosing method or lambda, an await expression may not occur inside the body of a `Catch` or `Finally` block, nor inside the body of a `SyncLock` statement, nor inside a query expression.</span></span>

<span data-ttu-id="8ad58-3675">Await 演算子は、値として分類される必要があり、型が*待機可能*型であるか、または `Object` である必要がある単一の式を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3675">The await operator takes a single expression which must be classified as a value and whose type must be an *awaitable* type, or `Object`.</span></span> <span data-ttu-id="8ad58-3676">その型が `Object` の場合、すべての処理が実行時まで延期されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3676">If its type is `Object` then all processing is deferred until run-time.</span></span> <span data-ttu-id="8ad58-3677">次のすべての条件に該当する場合は、型 `C` と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3677">A type `C` is said to be awaitable if all of the following are true:</span></span>

* <span data-ttu-id="8ad58-3678">`C` には、引数を持たず、@no__t 型を返す `GetAwaiter` という名前のアクセス可能なインスタンスまたは拡張メソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3678">`C` contains an accessible instance or extension method named `GetAwaiter` which has no arguments and which returns some type `E`;</span></span>

* <span data-ttu-id="8ad58-3679">`E` には、読み取り可能なインスタンスまたは拡張プロパティが含まれています。このプロパティは、引数を取らず、ブール型の @no__t です。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3679">`E` contains a readable instance or extension property named `IsCompleted` which takes no arguments and has type Boolean;</span></span>

* <span data-ttu-id="8ad58-3680">`E` には、引数を受け取らない `GetResult` という名前のアクセス可能なインスタンスまたは拡張メソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3680">`E` contains an accessible instance or extension method named `GetResult` which takes no arguments;</span></span>

* <span data-ttu-id="8ad58-3681">`E` `System.Runtime.CompilerServices.INotifyCompletion` または `ICriticalNotifyCompletion` のいずれかを実装します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3681">`E` implements either `System.Runtime.CompilerServices.INotifyCompletion` or `ICriticalNotifyCompletion`.</span></span>

<span data-ttu-id="8ad58-3682">@No__t-0 が `Sub` の場合、await 式は void として分類されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3682">If `GetResult` was a `Sub`, then the await expression is classified as void.</span></span> <span data-ttu-id="8ad58-3683">それ以外の場合、await 式は値として分類され、型は `GetResult` メソッドの戻り値の型になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3683">Otherwise, the await expression is classified as a value and its type is the return type of the `GetResult` method.</span></span>

<span data-ttu-id="8ad58-3684">待機できるクラスの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3684">Here is an example of a class that can be awaited:</span></span>

```vb
Class MyTask(Of T)
    Function GetAwaiter() As MyTaskAwaiter(Of T)
        Return New MyTaskAwaiter With {.m_Task = Me}
    End Function

    ...
End Class

Structure MyTaskAwaiter(Of T)
    Implements INotifyCompletion

    Friend m_Task As MyTask(Of T)

    ReadOnly Property IsCompleted As Boolean
        Get
            Return m_Task.IsCompleted
        End Get
    End Property

    Sub OnCompleted(r As Action) Implements INotifyCompletion.OnCompleted
        ' r is the "resumptionDelegate"
        Dim sc = SynchronizationContext.Current
        If sc Is Nothing Then
            m_Task.ContinueWith(Sub() r())
        Else
            m_Task.ContinueWith(Sub() sc.Post(Sub() r(), Nothing))
        End If
    End Sub

    Function GetResult() As T
        If m_Task.IsCanceled Then Throw New TaskCanceledException(m_Task)
        If m_Task.IsFaulted Then Throw m_Task.Exception.InnerException
        Return m_Task.Result
    End Function
End Structure
```

<span data-ttu-id="8ad58-3685">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="8ad58-3685">__Note.__</span></span> <span data-ttu-id="8ad58-3686">ライブラリの作成者は、同じ @no__t で継続デリゲートを呼び出すパターンに従うことをお勧めします。これは、`OnCompleted` がそれ自体で呼び出された場合と同じです。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3686">Library authors are recommended to follow the pattern that they invoke the continuation delegate on the same `SynchronizationContext` as their `OnCompleted` was itself invoked on.</span></span> <span data-ttu-id="8ad58-3687">また、再開デリゲートは、stack overflow につながる可能性があるため、`OnCompleted` メソッド内で同期的に実行することはできません。代わりに、デリゲートを後続の実行のためにキューに配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3687">Also, the resumption delegate should not be executed synchronously within the `OnCompleted` method since that can lead to stack overflow: instead, the delegate should be queued for subsequent execution.</span></span>

<span data-ttu-id="8ad58-3688">制御フローが @no__t 0 演算子に到達すると、動作は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3688">When control flow reaches an `Await` operator, behavior is as follows.</span></span>

1.  <span data-ttu-id="8ad58-3689">Await オペランドの `GetAwaiter` メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3689">The `GetAwaiter` method of the await operand is invoked.</span></span> <span data-ttu-id="8ad58-3690">この呼び出しの結果は、 *awaiter*と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3690">The result of this invocation is termed the *awaiter*.</span></span>

2.  <span data-ttu-id="8ad58-3691">Awaiter の `IsCompleted` プロパティが取得されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3691">The awaiter's `IsCompleted` property is retrieved.</span></span> <span data-ttu-id="8ad58-3692">結果が true の場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3692">If the result is true then:</span></span>

    21. <span data-ttu-id="8ad58-3693">Awaiter の `GetResult` メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3693">The `GetResult` method of the awaiter is invoked.</span></span> <span data-ttu-id="8ad58-3694">@No__t-0 が関数の場合、await 式の値はこの関数の戻り値になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3694">If `GetResult` was a function, then the value of the await expression is the return value of this function.</span></span>

3.  <span data-ttu-id="8ad58-3695">IsCompleted プロパティが true でない場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3695">If the IsCompleted property isn't true then:</span></span>

    31. <span data-ttu-id="8ad58-3696">@No__t-0 は、awaiter に対して呼び出されます (awaiter の @no__t 型が `ICriticalNotifyCompletion` を実装している場合)。または `INotifyCompletion.OnCompleted` を実装している場合は。それ以外の場合は。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3696">Either `ICriticalNotifyCompletion.UnsafeOnCompleted` is invoked on the awaiter (if the awaiter's type `E` implements `ICriticalNotifyCompletion`) or `INotifyCompletion.OnCompleted` (otherwise).</span></span> <span data-ttu-id="8ad58-3697">どちらの場合も、非同期メソッドの現在のインスタンスに関連付けられている*再開デリゲート*を渡します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3697">In both cases it passes a *resumption delegate* associated with the current instance of the async method.</span></span>

    32. <span data-ttu-id="8ad58-3698">現在の非同期メソッドインスタンスの制御点は中断され、*現在の呼び出し元*で制御フローが再開されます (セクションの[非同期メソッド](statements.md#async-methods)で定義されています)。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3698">The control point of the current async method instance is suspended, and control flow resumes in the *current caller* (defined in Section [Async Methods](statements.md#async-methods)).</span></span>

    33. <span data-ttu-id="8ad58-3699">後で再開デリゲートが呼び出された場合、</span><span class="sxs-lookup"><span data-stu-id="8ad58-3699">If later the resumption delegate is invoked,</span></span>

        331. <span data-ttu-id="8ad58-3700">再開デリゲートは、最初に `System.Threading.Thread.CurrentThread.ExecutionContext` が呼び出されたとき `OnCompleted` を呼び出しました。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3700">the resumption delegate first restores `System.Threading.Thread.CurrentThread.ExecutionContext` to what it was at the time `OnCompleted` was called,</span></span>
        332. <span data-ttu-id="8ad58-3701">次に、非同期メソッドインスタンスの制御ポイントで制御フローを再開します (「[非同期メソッド](statements.md#async-methods)」を参照)。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3701">then it resumes control flow at the control point of the async method instance (see Section [Async Methods](statements.md#async-methods)),</span></span>
        333. <span data-ttu-id="8ad58-3702">ここでは、2.1 のように、awaiter の `GetResult` メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3702">where it calls the `GetResult` method of the awaiter, as in 2.1 above.</span></span>

<span data-ttu-id="8ad58-3703">Await オペランドに型オブジェクトがある場合、この動作は実行時まで遅延されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3703">If the await operand has type Object, then this behavior is deferred until runtime:</span></span>

- <span data-ttu-id="8ad58-3704">手順1は、引数を指定せずに GetAwaiter () を呼び出すことで実現されます。そのため、実行時に、オプションのパラメーターを受け取るインスタンスメソッドにバインドできます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3704">Step 1 is accomplished by calling GetAwaiter() with no arguments; it may therefore bind at runtime to instance methods which take optional parameters.</span></span>
- <span data-ttu-id="8ad58-3705">手順2は、引数を指定せずに IsCompleted () プロパティを取得し、ブール型への組み込みの変換を試行することで実現されます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3705">Step 2 is accomplished by retrieving the IsCompleted() property with no arguments, and by attempting an intrinsic conversion to Boolean.</span></span>
- <span data-ttu-id="8ad58-3706">手順 3. を実行すると `TryCast(awaiter, ICriticalNotifyCompletion)` を試行し、失敗した場合は-1 を @no__t ます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3706">Step 3.a is accomplished by attempting `TryCast(awaiter, ICriticalNotifyCompletion)`, and if this fails then `DirectCast(awaiter, INotifyCompletion)`.</span></span>

<span data-ttu-id="8ad58-3707">3\. で渡される再開デリゲート。1回だけ呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3707">The resumption delegate passed in 3.a may only be invoked once.</span></span> <span data-ttu-id="8ad58-3708">2回以上呼び出された場合、動作は未定義になります。</span><span class="sxs-lookup"><span data-stu-id="8ad58-3708">If it is invoked more than once, the behavior is undefined.</span></span>

