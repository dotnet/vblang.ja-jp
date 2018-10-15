# <a name="expressions"></a><span data-ttu-id="4e5da-101">式</span><span class="sxs-lookup"><span data-stu-id="4e5da-101">Expressions</span></span>

<span data-ttu-id="4e5da-102">式は、値の計算を指定するか、変数または定数を指定する演算子とオペランドのシーケンスです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-102">An expression is a sequence of operators and operands that specifies a computation of a value, or that designates a variable or constant.</span></span> <span data-ttu-id="4e5da-103">この章では、構文、オペランドと演算子の評価と式の意味の順序を定義します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-103">This chapter defines the syntax, order of evaluation of operands and operators, and meaning of expressions.</span></span>

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

## <a name="expression-classifications"></a><span data-ttu-id="4e5da-104">式の分類</span><span class="sxs-lookup"><span data-stu-id="4e5da-104">Expression Classifications</span></span>

<span data-ttu-id="4e5da-105">すべての式は、次のいずれかとして分類されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-105">Every expression is classified as one of the following:</span></span>

* <span data-ttu-id="4e5da-106">*値。*</span><span class="sxs-lookup"><span data-stu-id="4e5da-106">*A value.*</span></span> <span data-ttu-id="4e5da-107">すべての値には、型が関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-107">Every value has an associated type.</span></span>

* <span data-ttu-id="4e5da-108">*変数です。*</span><span class="sxs-lookup"><span data-stu-id="4e5da-108">*A variable.*</span></span> <span data-ttu-id="4e5da-109">すべての変数は、関連付けられた型、変数の宣言型つまりを持っています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-109">Every variable has an associated type, namely the declared type of the variable.</span></span>

* <span data-ttu-id="4e5da-110">*名前空間。*</span><span class="sxs-lookup"><span data-stu-id="4e5da-110">*A namespace.*</span></span> <span data-ttu-id="4e5da-111">メンバー アクセスの左側にあることにのみこの分類の式。</span><span class="sxs-lookup"><span data-stu-id="4e5da-111">An expression with this classification can only appear as the left side of a member access.</span></span> <span data-ttu-id="4e5da-112">他のコンテキストでは、名前空間として分類される式は、コンパイル時エラーを発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-112">In any other context, an expression classified as a namespace causes a compile-time error.</span></span>

* <span data-ttu-id="4e5da-113">*型。*</span><span class="sxs-lookup"><span data-stu-id="4e5da-113">*A type.*</span></span> <span data-ttu-id="4e5da-114">メンバー アクセスの左側にあることにのみこの分類の式。</span><span class="sxs-lookup"><span data-stu-id="4e5da-114">An expression with this classification can only appear as the left side of a member access.</span></span> <span data-ttu-id="4e5da-115">他のコンテキストでは、型として分類される式は、コンパイル時エラーを発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-115">In any other context, an expression classified as a type causes a compile-time error.</span></span>

* <span data-ttu-id="4e5da-116">*メソッド グループ、* 同じ名前でオーバー ロードされたメソッドのセットです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-116">*A method group,* which is a set of methods overloaded on the same name.</span></span> <span data-ttu-id="4e5da-117">メソッド グループに関連付けられたターゲット式と関連付けられている型の引数リストがあります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-117">A method group may have an associated target expression and an associated type argument list.</span></span>

* <span data-ttu-id="4e5da-118">*メソッドのポインター、* メソッドの場所を表します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-118">*A method pointer,* which represents the location of a method.</span></span> <span data-ttu-id="4e5da-119">メソッドのポインターに関連付けられたターゲット式と関連付けられている型の引数リストがあります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-119">A method pointer may have an associated target expression and an associated type argument list.</span></span>

* <span data-ttu-id="4e5da-120">*メソッドのラムダ*匿名メソッドであります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-120">*A lambda method,* which is an anonymous method.</span></span>

* <span data-ttu-id="4e5da-121">*プロパティ グループでは、* プロパティが同じ名前でオーバー ロードのセットです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-121">*A property group,* which is a set of properties overloaded on the same name.</span></span> <span data-ttu-id="4e5da-122">プロパティ グループに関連付けられたターゲット式があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-122">A property group may have an associated target expression.</span></span>

* <span data-ttu-id="4e5da-123">*プロパティ アクセス。*</span><span class="sxs-lookup"><span data-stu-id="4e5da-123">*A property access.*</span></span> <span data-ttu-id="4e5da-124">すべてのプロパティ アクセスは、関連付けられた型、つまり、プロパティの型を持ちます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-124">Every property access has an associated type, namely the type of the property.</span></span> <span data-ttu-id="4e5da-125">プロパティ アクセスに関連付けられたターゲット式があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-125">A property access may have an associated target expression.</span></span>

* <span data-ttu-id="4e5da-126">*遅延バインディング アクセス、* を表すメソッドまたはプロパティのアクセスが実行時まで延期します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-126">*A late-bound access,* which represents a method or property access deferred until run-time.</span></span> <span data-ttu-id="4e5da-127">関連付けられたターゲット式と関連付けられている型の引数リスト、遅延バインディング アクセスがあります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-127">A late-bound access may have an associated target expression and an associated type argument list.</span></span> <span data-ttu-id="4e5da-128">遅延バインディング アクセスの種類は常に`Object`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-128">The type of a late-bound access is always `Object`.</span></span>

* <span data-ttu-id="4e5da-129">*イベントへのアクセス。*</span><span class="sxs-lookup"><span data-stu-id="4e5da-129">*An event access.*</span></span> <span data-ttu-id="4e5da-130">イベントのすべてのアクセスが、関連付けられた型、つまり、イベントの種類。</span><span class="sxs-lookup"><span data-stu-id="4e5da-130">Every event access has an associated type, namely the type of the event.</span></span> <span data-ttu-id="4e5da-131">イベントへのアクセスに関連付けられたターゲット式があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-131">An event access may have an associated target expression.</span></span> <span data-ttu-id="4e5da-132">イベントへのアクセスは、最初の引数として表示される可能性、 `RaiseEvent`、 `AddHandler`、および`RemoveHandler`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="4e5da-132">An event access may appear as the first argument of the `RaiseEvent`, `AddHandler`, and `RemoveHandler` statements.</span></span> <span data-ttu-id="4e5da-133">他のコンテキストでは、イベント アクセスとして分類される式は、コンパイル時エラーを発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-133">In any other context, an expression classified as an event access causes a compile-time error.</span></span>

* <span data-ttu-id="4e5da-134">*配列リテラル、* を表す型が決定されていない配列の初期値。</span><span class="sxs-lookup"><span data-stu-id="4e5da-134">*An array literal,* which represents the initial values of an array whose type has not yet been determined.</span></span>

* <span data-ttu-id="4e5da-135">*無効にします。*</span><span class="sxs-lookup"><span data-stu-id="4e5da-135">*Void.*</span></span> <span data-ttu-id="4e5da-136">これは、式がサブルーチン、または結果なしで await 演算子の式の呼び出しの場合に発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-136">This occurs when the expression is an invocation of a subroutine, or an await operator expression with no result.</span></span> <span data-ttu-id="4e5da-137">Void として分類される式は、呼び出しステートメントや、await ステートメントのコンテキストでのみです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-137">An expression classified as void is only valid in the context of an invocation statement or an await statement.</span></span>

* <span data-ttu-id="4e5da-138">*既定値です。*</span><span class="sxs-lookup"><span data-stu-id="4e5da-138">*A default value.*</span></span> <span data-ttu-id="4e5da-139">リテラルのみ`Nothing`この分類が生成されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-139">Only the literal `Nothing` produces this classification.</span></span>

<span data-ttu-id="4e5da-140">値または特定のコンテキストでのみ許可されている中間値として機能している式の他のカテゴリで、変数、式の最終的な結果は通常は。</span><span class="sxs-lookup"><span data-stu-id="4e5da-140">The final result of an expression is usually a value or a variable, with the other categories of expressions functioning as intermediate values that are only permitted in certain contexts.</span></span>

<span data-ttu-id="4e5da-141">ステートメントと式を式が特定の特性 (参照型、値の型、いくつかの種類などから派生されている) などの型を必要とする式の型が型パラメーターを使用できることに注意してください、制約が課される場合型パラメーターでは、このような特性を満たします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-141">Note that expressions whose type is a type parameter can be used in statements and expressions that require the type of an expression to have certain characteristics (such as being a reference type, value type, deriving from some type, etc.) if the constraints imposed on the type parameter satisfy those characteristics.</span></span>

### <a name="expression-reclassification"></a><span data-ttu-id="4e5da-142">式の再分類</span><span class="sxs-lookup"><span data-stu-id="4e5da-142">Expression Reclassification</span></span>

<span data-ttu-id="4e5da-143">通常、式は、式から別の分類が必要なコンテキストで使用されるが、コンパイル時エラーが発生する--などにリテラル値を代入しようとしています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-143">Normally, when an expression is used in a context that requires a classification different from that of the expression, a compile-time error occurs -- for example, attempting to assign a value to a literal.</span></span> <span data-ttu-id="4e5da-144">ただし、多くの場合は、式の分類のプロセスを変更すること*再分類*します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-144">However, in many cases it is possible to change an expression's classification through the process of *reclassification*.</span></span>

<span data-ttu-id="4e5da-145">再分類が成功するは、拡大または縮小として再分類が判定します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-145">If reclassification succeeds, then the reclassification is judged as widening or narrowing.</span></span> <span data-ttu-id="4e5da-146">明記されていない限りは、この一覧にすべての再分類を拡大します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-146">Unless otherwise noted, all the reclassifications in this list are widening.</span></span>

<span data-ttu-id="4e5da-147">次の式の型を再分類できます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-147">The following types of expressions can be reclassified:</span></span>

* <span data-ttu-id="4e5da-148">変数は、値として再分類できます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-148">A variable can be reclassified as a value.</span></span> <span data-ttu-id="4e5da-149">変数に格納された値がフェッチされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-149">The value stored in the variable is fetched.</span></span>

* <span data-ttu-id="4e5da-150">メソッド グループは、値として再分類できます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-150">A method group can be reclassified as a value.</span></span> <span data-ttu-id="4e5da-151">メソッド グループ式が関連付けられている対象の式と型パラメーターのリスト、および空のかっこを持つ invocation 式として解釈されます (つまり、`f`として解釈される`f()`と`f(Of Integer)`として解釈されます`f(Of Integer)()`).</span><span class="sxs-lookup"><span data-stu-id="4e5da-151">The method group expression is interpreted as an invocation expression with the associated target expression and type parameter list, and empty parentheses (that is, `f` is interpreted as `f()` and `f(Of Integer)` is interpreted as `f(Of Integer)()`).</span></span> <span data-ttu-id="4e5da-152">この再分類すると、さらに再分類される void として表現する可能性がありますが、</span><span class="sxs-lookup"><span data-stu-id="4e5da-152">This reclassification may result in the expression being further reclassified as void.</span></span>

* <span data-ttu-id="4e5da-153">メソッドのポインターは、値として再分類することができます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-153">A method pointer can be reclassified as a value.</span></span> <span data-ttu-id="4e5da-154">この分類は、対象の型がわかっている変換のコンテキストでのみ実行できます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-154">This reclassification can only occur in the context of a conversion where the target type is known.</span></span> <span data-ttu-id="4e5da-155">ポインターのメソッドは、関連付けられている型の引数リストで適切な型のデリゲートのインスタンス化の式に引数として解釈されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-155">The method pointer expression is interpreted as the argument to a delegate instantiation expression of the appropriate type with the associated type argument list.</span></span> <span data-ttu-id="4e5da-156">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-156">For example:</span></span>
    
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

* <span data-ttu-id="4e5da-157">Lambda メソッドは、値として再分類できます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-157">A lambda method can be reclassified as a value.</span></span> <span data-ttu-id="4e5da-158">対象の型がわかっている変換のコンテキストで、再分類する場合は、2 つの再分類のいずれかが発生できます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-158">If the reclassification occurs in the context of a conversion where the target type is known, then one of two reclassifications can occur:</span></span>
    
  1. <span data-ttu-id="4e5da-159">対象の型がデリゲート型の場合は、ラムダ メソッドは、適切な型のデリゲート構築式を引数として解釈されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-159">If the target type is a delegate type, the lambda method is interpreted as the argument to a delegate-construction expression of the appropriate type.</span></span>
    
  2. <span data-ttu-id="4e5da-160">ターゲット型がある場合`System.Linq.Expressions.Expression(Of T)`、および`T`ラムダ メソッドがデリゲート構築式で使用されているかのように解釈されますが、デリゲート型`T`し、式ツリーに変換されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-160">If the target type is `System.Linq.Expressions.Expression(Of T)`, and `T` is a delegate type, then the lambda method is interpreted as if it was being used in delegate-construction expression for `T` and then converted to an expression tree.</span></span>
    
  <span data-ttu-id="4e5da-161">デリゲートが ByRef パラメーターを持たない場合、非同期または反復子のラムダ メソッドをデリゲート構築式を引数として解釈のみ可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-161">An async or iterator lambda method may only be interpreted as the argument to a delegate-construction expression, if the delegate has no ByRef parameters.</span></span>
    
  <span data-ttu-id="4e5da-162">デリゲートのパラメーターの型のいずれかから対応するラムダ パラメーターの型への変換が縮小変換の場合は、再分類が縮小; として評価します。それ以外の場合は、拡大変換します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-162">If conversion from any of the delegate's parameter types to the corresponding lambda parameter types is a narrowing conversion, then the reclassification is judged as narrowing; otherwise it is widening.</span></span>
    
  <span data-ttu-id="4e5da-163">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-163">__Note.__</span></span> <span data-ttu-id="4e5da-164">ラムダ メソッドと式ツリー間の正確な変換は、コンパイラのバージョン間で解決されはこの仕様の範囲外です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-164">The exact translation between lambda methods and expression trees may not be fixed between versions of the compiler and is beyond the scope of this specification.</span></span> <span data-ttu-id="4e5da-165">Microsoft Visual Basic の 11.0 のすべてのラムダ式を次の制限に従い、式ツリーに変換可能性があります: (1) 1。</span><span class="sxs-lookup"><span data-stu-id="4e5da-165">For Microsoft Visual Basic 11.0, all lambda expressions may be converted to expression trees subject to the following restrictions: (1) 1.</span></span>  <span data-ttu-id="4e5da-166">ByRef パラメーターを指定せずに 1 行のラムダ式だけは、式ツリーに変換することがあります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-166">Only single-line lambda expressions without ByRef parameters may be converted to expression trees.</span></span> <span data-ttu-id="4e5da-167">単一行の`Sub`のみ呼び出しステートメント形式のラムダは式ツリーに変換可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-167">Of the single-line `Sub` lambdas, only invocation statements may be converted to expression trees.</span></span> <span data-ttu-id="4e5da-168">(2) は、匿名型の式はなど、後続のフィールドの初期化子を初期化するために以前のフィールド初期化子を使用する場合、式ツリーに変換できない`New With {.a=1, .b=.a}`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-168">(2) Anonymous type expressions cannot be converted to expression trees if an earlier field initializer is used to initialize a subsequent field initializer, e.g. `New With {.a=1, .b=.a}`.</span></span> <span data-ttu-id="4e5da-169">(3) は、オブジェクト初期化子式に変換できない式ツリーの現在のオブジェクトが初期化されるメンバーがフィールド初期化子のいずれかで使用する場合など`New C1 With {.a=1, .b=.Method1()}`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-169">(3) Object initializer expressions cannot be converted to expression trees if a member of the current object being initialized is used in one of the field initializers, e.g. `New C1 With {.a=1, .b=.Method1()}`.</span></span> <span data-ttu-id="4e5da-170">(要素の型を明示的に宣言する場合、4) 多次元配列作成式を式ツリーに変換のみできます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-170">(4) Multi-dimensional array creation expressions can only be converted to expression trees if they declare their element type explicitly.</span></span> <span data-ttu-id="4e5da-171">(5) 遅延バインディング式は、式ツリーに変換できません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-171">(5) Late-binding expressions cannot be converted to expression trees.</span></span> <span data-ttu-id="4e5da-172">(6) 変数またはフィールドは、invocation 式に ByRef を渡された場合、ByRef パラメーターと正確に同じ型ではありませんが、またはプロパティが ByRef を渡されるときに、通常の VB セマンティクスが ByRef を渡される引数のコピーを最終的な値はコピーされます。 変数またはフィールドやプロパティに戻す</span><span class="sxs-lookup"><span data-stu-id="4e5da-172">(6) When a variable or field is passed ByRef to an invocation expression but does not have exactly the same type as the ByRef parameter, or when a property is passed ByRef, normal VB semantics are that a copy of the argument is passed ByRef and its final value is then copied back into the variable or field or property.</span></span> <span data-ttu-id="4e5da-173">式のツリーでは、コピー バックアップは発生しません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-173">In expression trees, the copy-back does not happen.</span></span> <span data-ttu-id="4e5da-174">(入れ子になったラムダ式もに、これらの 7) すべての制限が適用されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-174">(7) All these restrictions apply to nested lambda expressions as well.</span></span>
    
  <span data-ttu-id="4e5da-175">対象の型が不明である場合、メソッドのラムダは、匿名のデリゲート型のラムダ メソッドの同じシグネチャを持つデリゲートのインスタンス化の式を引数として解釈されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-175">If the target type is not known, then the lambda method is interpreted as the argument to a delegate instantiation expression of an anonymous delegate type with the same signature of the lambda method.</span></span> <span data-ttu-id="4e5da-176">厳密な型が使用されているし、任意のパラメーターの型を省略すると、コンパイル時エラーが発生します。それ以外の場合、`Object`不足しているパラメーターの型の代わりに使用します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-176">If strict semantics are being used and the type of any of the parameters are omitted, a compile-time error occurs; otherwise, `Object` is substituted for any missing parameter type.</span></span> <span data-ttu-id="4e5da-177">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-177">For example:</span></span>
    
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

* <span data-ttu-id="4e5da-178">プロパティ アクセスには、プロパティ グループを再分類できます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-178">A property group can be reclassified as a property access.</span></span> <span data-ttu-id="4e5da-179">プロパティのグループ式が空のかっこインデックス式として解釈されます (つまり、`f`として解釈される`f()`)。</span><span class="sxs-lookup"><span data-stu-id="4e5da-179">The property group expression is interpreted as an index expression with empty parentheses (that is, `f` is interpreted as `f()`).</span></span>

* <span data-ttu-id="4e5da-180">プロパティ アクセスは、値として再分類できます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-180">A property access can be reclassified as a value.</span></span> <span data-ttu-id="4e5da-181">プロパティ アクセス式の invocation 式として解釈されます、`Get`プロパティのアクセサー。</span><span class="sxs-lookup"><span data-stu-id="4e5da-181">The property access expression is interpreted as an invocation expression of the `Get` accessor of the property.</span></span> <span data-ttu-id="4e5da-182">プロパティは、get アクセス操作子を持たない、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-182">If the property has no getter, then a compile-time error occurs.</span></span>

* <span data-ttu-id="4e5da-183">遅延バインディング アクセスは、遅延バインディング メソッドまたはプロパティの遅延バインディング アクセスとして再分類することができます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-183">A late-bound access can be reclassified as a late-bound method or late-bound property access.</span></span> <span data-ttu-id="4e5da-184">場合、遅延バインディング アクセス再分類できますメソッドへのアクセスとプロパティへのアクセスの両方では、再分類プロパティへのアクセスをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-184">In a situation where a late-bound access can be reclassified both as a method access and as a property access, reclassification to a property access is preferred.</span></span>

* <span data-ttu-id="4e5da-185">遅延バインディング アクセスは、値として再分類することができます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-185">A late-bound access can be reclassified as a value.</span></span>

* <span data-ttu-id="4e5da-186">配列リテラルは、値として再分類することができます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-186">An array literal can be reclassified as a value.</span></span> <span data-ttu-id="4e5da-187">値の型は、次のように決定されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-187">The type of the value is determined as follows:</span></span>

  1. <span data-ttu-id="4e5da-188">再分類が、ターゲット型がわかっているし、ターゲット型が配列型の変換のコンテキストで発生した場合、使用型の値として、配列リテラルが再分類します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-188">If the reclassification occurs in the context of a conversion where the target type is known and the target type is an array type, then the array literal is reclassified as a value of type T().</span></span> <span data-ttu-id="4e5da-189">対象の型は場合`System.Collections.Generic.IList(Of T)`、 `IReadOnlyList(Of T)`、 `ICollection(Of T)`、 `IReadOnlyCollection(Of T)`、または`IEnumerable(Of T)`、配列リテラルが型の値として再分類し、配列リテラルが、入れ子のレベルを 1 つ`T()`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-189">If the target type is `System.Collections.Generic.IList(Of T)`, `IReadOnlyList(Of T)`, `ICollection(Of T)`, `IReadOnlyCollection(Of T)`, or `IEnumerable(Of T)`, and the array literal has one level of nesting, then the array literal is reclassified as a value of type `T()`.</span></span>

  2. <span data-ttu-id="4e5da-190">それ以外の場合、配列リテラルが型が配列のランクと等しい入れ子のレベルと一緒に使用して、; 初期化子内の要素の主要な型によって決定された要素型の値に再分類します。主要な型を決定できない場合`Object`使用されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-190">Otherwise, the array literal is reclassified to a value whose type is an array of rank equal to the level of nesting is used, with element type determined by the dominant type of the elements in the initializer; if no dominant type can be determined, `Object` is used.</span></span> <span data-ttu-id="4e5da-191">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-191">For example:</span></span>

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

  <span data-ttu-id="4e5da-192">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-192">__Note.__</span></span> <span data-ttu-id="4e5da-193">バージョン 9.0 と言語のバージョン 10.0 の間の動作が若干変更があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-193">There is a slight change in behavior between version 9.0 and version 10.0 of the language.</span></span> <span data-ttu-id="4e5da-194">10.0 より前に、ローカル変数の型の推定によって影響されなかった配列要素の初期化子とその実行のようになりました。</span><span class="sxs-lookup"><span data-stu-id="4e5da-194">Prior to 10.0, array element initializers did not affect local variable type inference and now they do.</span></span> <span data-ttu-id="4e5da-195">したがって`Dim a() = { 1, 2, 3 }`あると推論された`Object()`の種類として`a`バージョン 9.0 の言語でと`Integer()`version 10.0 です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-195">So `Dim a() = { 1, 2, 3 }` would have inferred `Object()` as the type of `a` in version 9.0 of the language and `Integer()` in version 10.0.</span></span>

  <span data-ttu-id="4e5da-196">再分類し、配列配列作成式としてリテラルをとして再解釈します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-196">The reclassification then reinterprets the array literal as an array-creation expression.</span></span> <span data-ttu-id="4e5da-197">したがって、例。</span><span class="sxs-lookup"><span data-stu-id="4e5da-197">So the examples:</span></span>

  ```vb
  Dim x As Double = { 1, 2, 3, 4 }
  Dim y = { "a", "b" }
  ```

  <span data-ttu-id="4e5da-198">同等です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-198">are equivalent to:</span></span>

  ```vb
  Dim x As Double = New Double() { 1, 2, 3, 4 }
  Dim y = New String() { "a", "b" }
  ```

  <span data-ttu-id="4e5da-199">再分類が縮小要素式から配列の要素の型へのすべての変換は縮小; 場合として判定します。それ以外の場合として拡大変換と判断されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-199">The reclassification is judged as narrowing if any conversion from an element expression to the array element type is narrowing; otherwise it is judged as widening.</span></span>

* <span data-ttu-id="4e5da-200">既定値`Nothing`値として再分類できます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-200">The default value `Nothing` can be reclassified as a value.</span></span> <span data-ttu-id="4e5da-201">対象の型がわかっている場合は、結果は、対象の型の既定値です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-201">In a context where the target type is known, the result is the default value of the target type.</span></span> <span data-ttu-id="4e5da-202">対象の型が不明のコンテキストで、結果は型の null 値を`Object`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-202">In a context where the target type is not known, the result is a null value of type `Object`.</span></span>

<span data-ttu-id="4e5da-203">式の名前空間、型の式、イベントへのアクセス式、または void 式を再分類することはできません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-203">A namespace expression, type expression, event access expression, or void expression cannot be reclassified.</span></span> <span data-ttu-id="4e5da-204">複数の再分類は、同時に実行できます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-204">Multiple reclassifications can be done at the same time.</span></span> <span data-ttu-id="4e5da-205">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-205">For example:</span></span>

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

<span data-ttu-id="4e5da-206">この場合、プロパティはグループ式`P`が最初に、プロパティ アクセス プロパティ グループから再分類して、値プロパティ アクセスから分類し直されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-206">In this case, the property group expression `P` is first reclassified from a property group to a property access and then reclassified from a property access to a value.</span></span> <span data-ttu-id="4e5da-207">コンテキストで有効な分類に到達する再分類の最小数が実行されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-207">The fewest number of reclassifications are performed to reach a valid classification in the context.</span></span>

## <a name="constant-expressions"></a><span data-ttu-id="4e5da-208">定数式</span><span class="sxs-lookup"><span data-stu-id="4e5da-208">Constant Expressions</span></span>

<span data-ttu-id="4e5da-209">A*定数式*値はコンパイル時に完全に評価できる式です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-209">A *constant expression* is an expression whose value can be fully evaluated at compile time.</span></span>

```antlr
ConstantExpression
    : Expression
    ;
```

<span data-ttu-id="4e5da-210">定数式の型は、 `Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、 `ULong`、 `Long`、 `Char`、 `Single`、 `Double`、 `Decimal`、 `Date`、 `Boolean`、 `String`、 `Object`、または列挙型。</span><span class="sxs-lookup"><span data-stu-id="4e5da-210">The type of a constant expression can be `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Char`, `Single`, `Double`, `Decimal`, `Date`, `Boolean`, `String`, `Object`, or any enumeration type.</span></span> <span data-ttu-id="4e5da-211">次の構成要素は、定数式で許可されています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-211">The following constructs are permitted in constant expressions:</span></span>

* <span data-ttu-id="4e5da-212">リテラル (含む`Nothing`)。</span><span class="sxs-lookup"><span data-stu-id="4e5da-212">Literals (including `Nothing`).</span></span>

* <span data-ttu-id="4e5da-213">定数の型のメンバーまたはローカル変数を定数への参照。</span><span class="sxs-lookup"><span data-stu-id="4e5da-213">References to constant type members or constant locals.</span></span>

* <span data-ttu-id="4e5da-214">列挙型のメンバーへの参照。</span><span class="sxs-lookup"><span data-stu-id="4e5da-214">References to members of enumeration types.</span></span>

* <span data-ttu-id="4e5da-215">かっこで囲まれた部分式。</span><span class="sxs-lookup"><span data-stu-id="4e5da-215">Parenthesized subexpressions.</span></span>

* <span data-ttu-id="4e5da-216">強制型変換の式では、対象の型は、上記の型の 1 つ用意されています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-216">Coercion expressions, provided the target type is one of the types listed above.</span></span> <span data-ttu-id="4e5da-217">強制型変換との間`String`この規則の例外は、ために null 値でのみ使用できます`String`変換が実行環境の現在のカルチャで実行時に常に行われます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-217">Coercions to and from `String` are an exception to this rule and are only allowed on null values because `String` conversions are always done in the current culture of the execution environment at run time.</span></span> <span data-ttu-id="4e5da-218">強制型変換の定数式が組み込みの変換を使用できますのみに注意してください。</span><span class="sxs-lookup"><span data-stu-id="4e5da-218">Note that constant coercion expressions can only ever use intrinsic conversions.</span></span>

* <span data-ttu-id="4e5da-219">`+`、`-`と`Not`単項演算子、オペランドを指定して、結果は上記の型。</span><span class="sxs-lookup"><span data-stu-id="4e5da-219">The `+`, `-` and `Not` unary operators, provided the operand and result is of a type listed above.</span></span>

* <span data-ttu-id="4e5da-220">`+`、 `-`、 `*`、 `^`、 `Mod`、 `/`、 `\`、 `<<`、 `>>`、 `&`、 `And`、 `Or`、 `Xor`、 `AndAlso`、 `OrElse`、 `=`、 `<`、 `>`、 `<>`、 `<=`、および`=>`二項演算子は、各オペランドを指定して、結果は上記の型。</span><span class="sxs-lookup"><span data-stu-id="4e5da-220">The `+`, `-`, `*`, `^`, `Mod`, `/`, `\`, `<<`, `>>`, `&`, `And`, `Or`, `Xor`, `AndAlso`, `OrElse`, `=`, `<`, `>`, `<>`, `<=`, and `=>` binary operators, provided each operand and result is of a type listed above.</span></span>

* <span data-ttu-id="4e5da-221">条件演算子場合、各オペランドと結果が上に示した型を提供します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-221">The conditional operator If, provided each operand and result is of a type listed above.</span></span>

* <span data-ttu-id="4e5da-222">次のランタイム関数: `Microsoft.VisualBasic.Strings.ChrW`;`Microsoft.VisualBasic.Strings.Chr`定数の値は 0 ~ 128; 場合`Microsoft.VisualBasic.Strings.AscW`定数文字列が空でない場合`Microsoft.VisualBasic.Strings.Asc`定数文字列が空でない場合。</span><span class="sxs-lookup"><span data-stu-id="4e5da-222">The following run-time functions: `Microsoft.VisualBasic.Strings.ChrW`; `Microsoft.VisualBasic.Strings.Chr` if the constant value is between 0 and 128; `Microsoft.VisualBasic.Strings.AscW` if the constant string is not empty; `Microsoft.VisualBasic.Strings.Asc` if the constant string is not empty.</span></span>

<span data-ttu-id="4e5da-223">次の構成体は*いない*定数式で使用できます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-223">The following constructs are *not* permitted in constant expressions:</span></span>

* <span data-ttu-id="4e5da-224">暗黙的なバインドを通じて、`With`コンテキスト。</span><span class="sxs-lookup"><span data-stu-id="4e5da-224">Implicit binding through a `With` context.</span></span>

<span data-ttu-id="4e5da-225">整数型の定数式 (`ULong`、 `Long`、 `UInteger`、 `Integer`、 `UShort`、 `Short`、 `SByte`、または`Byte`) より狭いの整数型に暗黙的に変換できると型の定数式`Double`に暗黙的に変換できる`Single`定数式の値が変換先の型の範囲内に提供します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-225">Constant expressions of an integral type (`ULong`, `Long`, `UInteger`, `Integer`, `UShort`, `Short`, `SByte`, or `Byte`) can be implicitly converted to a narrower integral type, and constant expressions of type `Double` can be implicitly converted to `Single`, provided the value of the constant expression is within the range of the destination type.</span></span> <span data-ttu-id="4e5da-226">制限の少ないまたは strict セマンティクスが使用されているかどうかに関係なくこのような縮小変換が許可されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-226">These narrowing conversions are allowed regardless of whether permissive or strict semantics are being used.</span></span>


## <a name="late-bound-expressions"></a><span data-ttu-id="4e5da-227">遅延バインディング式</span><span class="sxs-lookup"><span data-stu-id="4e5da-227">Late-Bound Expressions</span></span>

<span data-ttu-id="4e5da-228">型の場合、メンバー アクセス式またはインデックスの式のターゲット`Object`式の処理は実行時まで遅延する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-228">When the target of a member access expression or index expression is of type `Object`, the processing of the expression may be deferred until run time.</span></span> <span data-ttu-id="4e5da-229">このように処理を遅延させることが呼び出されます*遅延バインディング*します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-229">Deferring processing this way is called *late binding*.</span></span> <span data-ttu-id="4e5da-230">遅延バインディングで`Object`で使用される変数を*型宣言不要な*メンバーのすべての解決は、変数の値の実際の実行時の型をに基づいて、方法です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-230">Late binding allows `Object` variables to be used in a *typeless* way, where all resolution of members is based on the actual run-time type of the value in the variable.</span></span> <span data-ttu-id="4e5da-231">コンパイル環境または厳密な型が指定されて場合`Option Strict`、遅延バインディングによってコンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-231">If strict semantics are specified by the compilation environment or by `Option Strict`, late binding causes a compile-time error.</span></span> <span data-ttu-id="4e5da-232">遅延バインディングを行うなどのオーバー ロードの解決の目的で非パブリック メンバーは無視されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-232">Non-public members are ignored when doing late-binding, including for the purposes of overload resolution.</span></span> <span data-ttu-id="4e5da-233">呼び出しまたはへのアクセスは、事前バインディングの場合とは異なりに注意してください、`Shared`メンバーの遅延バインディングを実行時に評価される呼び出しの対象となります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-233">Note that, unlike the early-bound case, invoking or accessing a `Shared` member late-bound will cause the invocation target to be evaluated at run time.</span></span> <span data-ttu-id="4e5da-234">Invocation 式で定義されているメンバーのかどうか、式には`System.Object`、遅延バインディングは行われません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-234">If the expression is an invocation expression for a member defined on `System.Object`, late binding will not take place.</span></span>

<span data-ttu-id="4e5da-235">一般に、遅延バインディング アクセスは解決実行時に、式の実際の実行時の型の識別子を参照しています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-235">In general, late-bound accesses are resolved at run time by looking up the identifier on the actual run-time type of the expression.</span></span> <span data-ttu-id="4e5da-236">実行時に、遅延バインディング メンバー検索が失敗した場合、`System.MissingMemberException`例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-236">If late-bound member lookup fails at run time, a `System.MissingMemberException` exception is thrown.</span></span> <span data-ttu-id="4e5da-237">遅延バインディング メンバー ルックアップが行われるため、関連付けられている対象の式の実行時の型、オフだけはありませんインターフェイスに、オブジェクトの実行時の型。</span><span class="sxs-lookup"><span data-stu-id="4e5da-237">Because late-bound member lookup is done solely off the run-time type of the associated target expression, an object's run-time type is never an interface.</span></span> <span data-ttu-id="4e5da-238">そのため、遅延バインディング メンバー アクセス式でインターフェイス メンバーにアクセスすることはできません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-238">Therefore, it is impossible to access interface members in a late-bound member access expression.</span></span>

<span data-ttu-id="4e5da-239">遅延バインディング メンバー アクセスへの引数は、メンバー アクセス式に表示される順序で評価されます。 パラメーターが、遅延バインディング メンバーで宣言されている順序されません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-239">The arguments to a late-bound member access are evaluated in the order they appear in the member access expression: not the order in which parameters are declared in the late-bound member.</span></span> <span data-ttu-id="4e5da-240">次の例は、この違いを示しています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-240">The following example illustrates this difference:</span></span>

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

<span data-ttu-id="4e5da-241">このコードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-241">This code displays:</span></span>

```
Early-bound: xy
Late-bound: yx
```

<span data-ttu-id="4e5da-242">引数の実行時の型で、遅延バインディングされたオーバー ロードの解決が行われるために、式がコンパイル時または実行時に評価されるかどうかに基づいて異なる結果を生成することです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-242">Because late-bound overload resolution is done on the run-time type of the arguments, it is possible that an expression might produce different results based on whether it is evaluated at compile time or run time.</span></span> <span data-ttu-id="4e5da-243">次の例は、この違いを示しています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-243">The following example illustrates this difference:</span></span>

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

<span data-ttu-id="4e5da-244">このコードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-244">This code displays:</span></span>

```
F(Base)
F(Derived)
```

## <a name="simple-expressions"></a><span data-ttu-id="4e5da-245">単純な式</span><span class="sxs-lookup"><span data-stu-id="4e5da-245">Simple Expressions</span></span>

<span data-ttu-id="4e5da-246">単純式は、リテラル、かっこで囲まれた式、インスタンスの式または簡易名式です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-246">Simple expressions are literals, parenthesized expressions, instance expressions, or simple name expressions.</span></span>

```antlr
SimpleExpression
    : LiteralExpression
    | ParenthesizedExpression
    | InstanceExpression
    | SimpleNameExpression
    | AddressOfExpression
    ;
```

### <a name="literal-expressions"></a><span data-ttu-id="4e5da-247">リテラル式</span><span class="sxs-lookup"><span data-stu-id="4e5da-247">Literal Expressions</span></span>

<span data-ttu-id="4e5da-248">リテラル式はリテラルで表される値に評価されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-248">Literal expressions evaluate to the value represented by the literal.</span></span> <span data-ttu-id="4e5da-249">リテラル式はリテラル以外の値として分類`Nothing`、これは既定値として分類します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-249">A literal expression is classified as a value, except for the literal `Nothing`, which is classified as a default value.</span></span>

```antlr
LiteralExpression
    : Literal
    ;
```

### <a name="parenthesized-expressions"></a><span data-ttu-id="4e5da-250">かっこで囲まれた式</span><span class="sxs-lookup"><span data-stu-id="4e5da-250">Parenthesized Expressions</span></span>

<span data-ttu-id="4e5da-251">かっこで囲まれた式は、かっこで囲まれた式で構成されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-251">A parenthesized expression consists of an expression enclosed in parentheses.</span></span> <span data-ttu-id="4e5da-252">かっこで囲まれた式は、値として分類され、値として、囲まれた式を分類する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-252">A parenthesized expression is classified as a value, and the enclosed expression must be classified as a value.</span></span> <span data-ttu-id="4e5da-253">かっこで囲まれた式は、かっこで囲まれた式の値に評価されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-253">A parenthesized expression evaluates to the value of the expression within the parentheses.</span></span>

```antlr
ParenthesizedExpression
    : OpenParenthesis Expression CloseParenthesis
    ;
```

### <a name="instance-expressions"></a><span data-ttu-id="4e5da-254">インスタンス式</span><span class="sxs-lookup"><span data-stu-id="4e5da-254">Instance Expressions</span></span>

<span data-ttu-id="4e5da-255">*インスタンス式*キーワード`Me`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-255">An *instance expression* is the keyword `Me`.</span></span> <span data-ttu-id="4e5da-256">非共有メソッド、コンス トラクター、またはプロパティ アクセサーの本体でのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-256">It may only be used within the body of a non-shared method, constructor, or property accessor.</span></span> <span data-ttu-id="4e5da-257">これは、値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-257">It is classified as a value.</span></span> <span data-ttu-id="4e5da-258">キーワード`Me`実行されているメソッドまたはプロパティのアクセサーを含む型のインスタンスを表します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-258">The keyword `Me` represents the instance of the type containing the method or property accessor being executed.</span></span> <span data-ttu-id="4e5da-259">コンス トラクターが明示的に別のコンス トラクターを呼び出す場合 (セクション[コンス トラクター](type-members.md#constructors))、`Me`そのコンス トラクターの呼び出し後までは使用できません、インスタンスがまだ作成していないためです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-259">If a constructor explicitly invokes another constructor (Section [Constructors](type-members.md#constructors)), `Me` cannot be used until after that constructor call, because the instance has not yet been constructed.</span></span>

```antlr
InstanceExpression
    : 'Me'
    ;
```

### <a name="simple-name-expressions"></a><span data-ttu-id="4e5da-260">簡易名式</span><span class="sxs-lookup"><span data-stu-id="4e5da-260">Simple Name Expressions</span></span>

<span data-ttu-id="4e5da-261">A*簡易名式*省略可能な型引数リストの後に 1 つの識別子で構成されています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-261">A *simple name expression* consists of a single identifier followed by an optional type argument list.</span></span>

```antlr
SimpleNameExpression
    : Identifier ( OpenParenthesis 'Of' TypeArgumentList CloseParenthesis )?
    ;
```

<span data-ttu-id="4e5da-262">名前が解決されており、以下の"単純な名前解決ルールで"に分類されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-262">The name is resolved and classified by the following "simple name resolution rules":</span></span>

1.  <span data-ttu-id="4e5da-263">すぐ外側から始まるブロックし、各それを囲む外側のブロック (ある場合)、進める識別子には、ローカル変数、静的変数、定数のローカルの名前が一致する場合、メソッドの型パラメーター、またはパラメーターの識別子を参照します一致するエンティティ。</span><span class="sxs-lookup"><span data-stu-id="4e5da-263">Starting with the immediately enclosing block and continuing with each enclosing outer block (if any), if the identifier matches the name of a local variable, static variable, constant local, method type parameter, or parameter, then the identifier refers to the matching entity.</span></span>

    <span data-ttu-id="4e5da-264">ローカル変数、静的変数、またはローカル定数識別子に一致して、型引数リストが指定されて、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-264">If the identifier matches a local variable, static variable, or constant local and a type argument list was provided, a compile-time error occurs.</span></span> <span data-ttu-id="4e5da-265">識別子、メソッド型パラメーターに一致する型引数リストが提供された場合は、一致が発生せず、解像度が続行されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-265">If the identifier matches a method type parameter and a type argument list was provided, no match occurs and resolution continues.</span></span> <span data-ttu-id="4e5da-266">一致するローカル変数が暗黙的な関数には、識別子には、ローカル変数が一致すると、または`Get`アクセサーは、ローカル変数を返すし、式は、呼び出しステートメントでは、invocation 式の一部または`AddressOf`し式一致が行われずの解像度が続行されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-266">If the identifier matches a local variable, the local variable matched is the implicit function or `Get` accessor return local variable, and the expression is part of an invocation expression, invocation statement, or an `AddressOf` expression, then no match occurs and resolution continues.</span></span>

    <span data-ttu-id="4e5da-267">式は、ローカル変数、静的変数、またはパラメーターである場合、変数として分類されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-267">The expression is classified as a variable if it is a local variable, static variable, or parameter.</span></span> <span data-ttu-id="4e5da-268">式は、メソッド型パラメーターである場合、型として分類されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-268">The expression is classified as a type if it is a method type parameter.</span></span> <span data-ttu-id="4e5da-269">式は、ローカル定数である場合、値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-269">The expression is classified as a value if it is a constant local.</span></span>

2.  <span data-ttu-id="4e5da-270">式を含む入れ子になった型ごとに、最も内側から開始し、参照型の識別子の場合に、最も外側に移動には、アクセス可能なメンバーとの一致が生成されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-270">For each nested type containing the expression, starting from the innermost and going to the outermost, if a lookup of the identifier in the type produces a match with an accessible member:</span></span>

    21. <span data-ttu-id="4e5da-271">一致する型のメンバーが型パラメーターの場合、結果は型として分類され、一致する型パラメーターは。</span><span class="sxs-lookup"><span data-stu-id="4e5da-271">If the matching type member is a type parameter, then the result is classified as a type and is the matching type parameter.</span></span> <span data-ttu-id="4e5da-272">型引数リストが提供されている場合は、一致が行われず、解決処理が続行します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-272">If a type argument list was provided, no match occurs and resolution continues.</span></span>
    22. <span data-ttu-id="4e5da-273">それ以外の場合、型は、すぐ外側の型と参照型の非共有メンバーを識別する場合、結果は、フォームのメンバー アクセスと同じ`Me.E(Of A)`ここで、`E`識別子と`A`型引数リストには、存在する場合。</span><span class="sxs-lookup"><span data-stu-id="4e5da-273">Otherwise, if the type is the immediately enclosing type and the lookup identifies a non-shared type member, then the result is the same as a member access of the form `Me.E(Of A)`, where `E` is the identifier and `A` is the type argument list, if any.</span></span>
    23. <span data-ttu-id="4e5da-274">それ以外の場合、結果はまったく同じ形式のメンバー アクセス`T.E(Of A)`ここで、`T`が一致するメンバーを含む型`E`識別子、および`A`型引数リストがある場合。</span><span class="sxs-lookup"><span data-stu-id="4e5da-274">Otherwise, the result is exactly the same as a member access of the form `T.E(Of A)`, where `T` is the type containing the matching member, `E` is the identifier, and `A` is the type argument list, if any.</span></span> <span data-ttu-id="4e5da-275">この場合、非共有メンバーを参照する識別子のエラーを勧めします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-275">In this case, it is an error for the identifier to refer to a non-shared member.</span></span>

3.  <span data-ttu-id="4e5da-276">入れ子になった名前空間ごとに、最も内側から開始し、最も外側の名前空間には、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="4e5da-276">For each nested namespace, starting from the innermost and going to the outermost namespace, do the following:</span></span>

    31. <span data-ttu-id="4e5da-277">名前空間では、指定した名前でアクセス可能な型が含まれていてがで指定される型引数リストで、存在する場合、識別子がその型を表してされ、型に分類が、同じ数の型パラメーターを持ちます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-277">If the namespace contains an accessible type with the given name and has the same number of type parameters as was supplied in the type argument list, if any, then the identifier refers to that type and is classified as a type.</span></span>
    32. <span data-ttu-id="4e5da-278">それ以外の場合、型引数リストが指定されていません、名前空間には、指定した名前の名前空間のメンバーが含まれている場合は、識別子、名前空間を参照し、名前空間として分類されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-278">Otherwise, if no type argument list was supplied and the namespace contains a namespace member with the given name, then the identifier refers to that namespace and is classified as a namespace.</span></span>
    33. <span data-ttu-id="4e5da-279">それ以外の場合、名前空間には、1 つまたは複数のアクセス可能な標準的なモジュールが含まれていますメンバー名の識別子の参照がただ 1 つの標準的なモジュールでアクセス可能な一致を生成、場合、結果は正確に形式のメンバー アクセスと同じ`M.E(Of A)`、。場所`M`一致するメンバーを含む標準のモジュールは、`E`識別子、および`A`型引数リストがある場合。</span><span class="sxs-lookup"><span data-stu-id="4e5da-279">Otherwise, if the namespace contains one or more accessible standard modules, and a member name lookup of the identifier produces an accessible match in exactly one standard module, then the result is exactly the same as a member access of the form `M.E(Of A)`, where `M` is the standard module containing the matching member, `E` is the identifier, and `A` is the type argument list, if any.</span></span> <span data-ttu-id="4e5da-280">識別子には、1 つ以上の標準的なモジュールでアクセス可能な型のメンバーが一致すると、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-280">If the identifier matches accessible type members in more than one standard module, a compile-time error occurs.</span></span>

4.  <span data-ttu-id="4e5da-281">ソース ファイルが 1 つまたは複数のインポート エイリアス、識別子のいずれかの名前に一致する場合は、識別子はその名前空間または型を参照します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-281">If the source file has one or more import aliases, and the identifier matches the name of one of them, then the identifier refers to that namespace or type.</span></span> <span data-ttu-id="4e5da-282">型引数リストを指定すると、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-282">If a type argument list is supplied, a compile-time error occurs.</span></span>

5. <span data-ttu-id="4e5da-283">名前参照を含むソース ファイルの 1 つまたは複数のインポート: 場合</span><span class="sxs-lookup"><span data-stu-id="4e5da-283">If the source file containing the name reference has one or more imports:</span></span>

    51. <span data-ttu-id="4e5da-284">1 つだけで、識別子と一致する場合、型引数リストが表示され、存在する場合、または型のメンバーでは、指定された型パラメーターの同じ番号を持つアクセス可能な型の名前をインポートし、識別子は、その型または型のメンバーを参照します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-284">If the identifier matches in exactly one import the name of an accessible type with the same number of type parameters as was supplied in the type argument list, if any, or a type member, then the identifier refers to that type or type member.</span></span> <span data-ttu-id="4e5da-285">識別子に一致する 1 つ以上のインポートの型パラメーターの同じ番号を持つアクセス可能な型の名前として存在する場合、型引数リストで指定されたか、アクセス可能な型、メンバー、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-285">If the identifier matches in more than one import the name of an accessible type with the same number of type parameters as was supplied in the type argument list, if any, or an accessible type member, a compile-time error occurs.</span></span>
    52. <span data-ttu-id="4e5da-286">それ以外の場合、型引数リストが指定されていません、識別子で正確に 1 つのインポートでアクセス可能な型を持つ名前空間の名前と一致する場合は、その名前空間を識別子が参照します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-286">Otherwise, if no type argument list was supplied and the identifier matches in exactly one import the name of a namespace with accessible types, then the identifier refers to that namespace.</span></span> <span data-ttu-id="4e5da-287">型引数リストが指定されていないと、識別子では、複数のインポートでアクセス可能な型を持つ名前空間の名前と一致する、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-287">If no type argument list was supplied and the identifier matches in more than one import the name of a namespace with accessible types, a compile-time error occurs.</span></span>
    53. <span data-ttu-id="4e5da-288">それ以外の場合、インポートには、1 つまたは複数のアクセス可能な標準的なモジュールが含まれ、識別子のメンバー名の参照がただ 1 つの標準的なモジュールでアクセス可能な一致を生成、結果は、フォームのメンバー アクセスと同じ`M.E(Of A)`ここで、`M`一致するメンバーを含む標準のモジュールは、`E`識別子、および`A`型引数リストがある場合。</span><span class="sxs-lookup"><span data-stu-id="4e5da-288">Otherwise, if the imports contain one or more accessible standard modules, and a member name lookup of the identifier produces an accessible match in exactly one standard module, then the result is exactly the same as a member access of the form `M.E(Of A)`, where `M` is the standard module containing the matching member, `E` is the identifier, and `A` is the type argument list, if any.</span></span> <span data-ttu-id="4e5da-289">識別子には、1 つ以上の標準的なモジュールでアクセス可能な型のメンバーが一致すると、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-289">If the identifier matches accessible type members in more than one standard module, a compile-time error occurs.</span></span>

6.  <span data-ttu-id="4e5da-290">コンパイル環境は、1 つまたは複数のインポート エイリアスを定義、識別子のいずれかの名前に一致する場合は、識別子はその名前空間または型を参照します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-290">If the compilation environment defines one or more import aliases, and the identifier matches the name of one of them, then the identifier refers to that namespace or type.</span></span> <span data-ttu-id="4e5da-291">型引数リストを指定すると、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-291">If a type argument list is supplied, a compile-time error occurs.</span></span>

7. <span data-ttu-id="4e5da-292">場合は、コンパイル環境では、1 つまたは複数のインポートを定義します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-292">If the compilation environment defines one or more imports:</span></span>

    71. <span data-ttu-id="4e5da-293">1 つだけで、識別子と一致する場合、型引数リストが表示され、存在する場合、または型のメンバーでは、指定された型パラメーターの同じ番号を持つアクセス可能な型の名前をインポートし、識別子は、その型または型のメンバーを参照します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-293">If the identifier matches in exactly one import the name of an accessible type with the same number of type parameters as was supplied in the type argument list, if any, or a type member, then the identifier refers to that type or type member.</span></span> <span data-ttu-id="4e5da-294">識別子に一致する 1 つ以上のインポートの型パラメーターの同じ番号を持つアクセス可能な型の名前として存在する場合、型引数リストで指定されたか、型のメンバーでは、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-294">If the identifier matches in more than one import the name of an accessible type with the same number of type parameters as was supplied in the type argument list, if any, or a type member, a compile-time error occurs.</span></span>
    72. <span data-ttu-id="4e5da-295">それ以外の場合、型引数リストが指定されていません、識別子で正確に 1 つのインポートでアクセス可能な型を持つ名前空間の名前と一致する場合は、その名前空間を識別子が参照します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-295">Otherwise, if no type argument list was supplied and the identifier matches in exactly one import the name of a namespace with accessible types, then the identifier refers to that namespace.</span></span> <span data-ttu-id="4e5da-296">型引数リストが指定されていないと、識別子では、複数のインポートでアクセス可能な型を持つ名前空間の名前と一致する、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-296">If no type argument list was supplied and the identifier matches in more than one import the name of a namespace with accessible types, a compile-time error occurs.</span></span>
    73. <span data-ttu-id="4e5da-297">それ以外の場合、インポートには、1 つまたは複数のアクセス可能な標準的なモジュールが含まれ、識別子のメンバー名の参照がただ 1 つの標準的なモジュールでアクセス可能な一致を生成、結果は、フォームのメンバー アクセスと同じ`M.E(Of A)`ここで、`M`一致するメンバーを含む標準のモジュールは、`E`識別子、および`A`型引数リストがある場合。</span><span class="sxs-lookup"><span data-stu-id="4e5da-297">Otherwise, if the imports contain one or more accessible standard modules, and a member name lookup of the identifier produces an accessible match in exactly one standard module, then the result is exactly the same as a member access of the form `M.E(Of A)`, where `M` is the standard module containing the matching member,  `E` is the identifier, and `A` is the type argument list, if any.</span></span> <span data-ttu-id="4e5da-298">識別子には、1 つ以上の標準的なモジュールでアクセス可能な型のメンバーが一致すると、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-298">If the identifier matches accessible type members in more than one standard module, a compile-time error occurs.</span></span>

8. <span data-ttu-id="4e5da-299">それ以外の場合、識別子によって指定された名前は定義されません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-299">Otherwise, the name given by the identifier is undefined.</span></span>

<span data-ttu-id="4e5da-300">定義されていない簡易名の式は、コンパイル時エラーです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-300">A simple name expression that is undefined is a compile-time error.</span></span>

<span data-ttu-id="4e5da-301">通常、名前できます 1 回だけでは、特定の名前空間。</span><span class="sxs-lookup"><span data-stu-id="4e5da-301">Normally, a name can only occur once in a particular namespace.</span></span> <span data-ttu-id="4e5da-302">ただし、ため、複数の .NET アセンブリ間で名前空間を宣言することができます、2 つのアセンブリが同じ完全修飾名を持つ型を定義する状況を含めることは可能です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-302">However, because namespaces can be declared across multiple .NET assemblies, it is possible to have a situation where two assemblies define a type with the same fully qualified name.</span></span> <span data-ttu-id="4e5da-303">その場合は、ソース ファイルの現在のセットで宣言された型は、外部 .NET アセンブリで宣言された型を優先します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-303">In that case, a type declared in the current set of source files is preferred over a type declared in an external .NET assembly.</span></span> <span data-ttu-id="4e5da-304">それ以外の場合、名前があいまいと名前のあいまいさを解消する方法はありません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-304">Otherwise, the name is ambiguous and there is no way to disambiguate the name.</span></span>


### <a name="addressof-expressions"></a><span data-ttu-id="4e5da-305">AddressOf 式</span><span class="sxs-lookup"><span data-stu-id="4e5da-305">AddressOf Expressions</span></span>

<span data-ttu-id="4e5da-306">`AddressOf`式はメソッドのポインターを生成するために使用します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-306">An `AddressOf` expression is used to produce a method pointer.</span></span> <span data-ttu-id="4e5da-307">式から成る、`AddressOf`キーワードと式をメソッド グループ、または遅延バインディング アクセスとして分類する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-307">The expression consists of the `AddressOf` keyword and an expression that must be classified as a method group or a late-bound access.</span></span> <span data-ttu-id="4e5da-308">メソッド グループは、コンス トラクターを参照できません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-308">The method group cannot refer to constructors.</span></span>

<span data-ttu-id="4e5da-309">結果は、メソッド グループとしての型引数リスト (ある場合) と同じ関連付けられている対象の式で、メソッドのポインターとして分類されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-309">The result is classified as a method pointer, with the same associated target expression and type argument list (if any) as the method group.</span></span>

```antlr
AddressOfExpression
    : 'AddressOf' Expression
    ;
```

## <a name="type-expressions"></a><span data-ttu-id="4e5da-310">型の式</span><span class="sxs-lookup"><span data-stu-id="4e5da-310">Type Expressions</span></span>

<span data-ttu-id="4e5da-311">A*式を入力*は、`GetType`式、`TypeOf...Is`式、`Is`式、または`GetXmlNamespace`式。</span><span class="sxs-lookup"><span data-stu-id="4e5da-311">A *type expression* is a `GetType` expression, a `TypeOf...Is` expression, an `Is` expression, or a `GetXmlNamespace` expression.</span></span>

```antlr
TypeExpression
    : GetTypeExpression
    | TypeOfIsExpression
    | IsExpression
    | GetXmlNamespaceExpression
    ;
```

### <a name="gettype-expressions"></a><span data-ttu-id="4e5da-312">GetType 式</span><span class="sxs-lookup"><span data-stu-id="4e5da-312">GetType Expressions</span></span>

<span data-ttu-id="4e5da-313">A`GetType`式から成るキーワード`GetType`と型の名前。</span><span class="sxs-lookup"><span data-stu-id="4e5da-313">A `GetType` expression consists of the keyword `GetType` and the name of a type.</span></span>

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

<span data-ttu-id="4e5da-314">A`GetType`式は、値として分類され、その値は、リフレクション (`System.Type`) を表すクラス、 *GetTypeTypeName*します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-314">A `GetType` expression is classified as a value, and its value is the reflection (`System.Type`) class that represents its *GetTypeTypeName*.</span></span> <span data-ttu-id="4e5da-315">場合、 *GetTypeTypeName* 、型パラメーターは、式を返す、`System.Type`実行時に、型パラメーターに対して指定された型引数に対応するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="4e5da-315">If the *GetTypeTypeName* is a type parameter, the expression will return the `System.Type` object that corresponds to the type argument supplied for the type parameter at run-time.</span></span>

<span data-ttu-id="4e5da-316">*GetTypeTypeName* 2 つの方法で特別な。</span><span class="sxs-lookup"><span data-stu-id="4e5da-316">The *GetTypeTypeName* is special in two ways:</span></span>

* <span data-ttu-id="4e5da-317">それが許容`System.Void`、この型の名前を参照する言語で唯一の場所。</span><span class="sxs-lookup"><span data-stu-id="4e5da-317">It is allowed to be `System.Void`, the only place in the language where this type name may be referenced.</span></span>

* <span data-ttu-id="4e5da-318">省略すると、型引数で構築されたジェネリック型である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-318">It may be a constructed generic type with the type arguments omitted.</span></span> <span data-ttu-id="4e5da-319">これにより、`GetType`を返す式、`System.Type`ジェネリック型パラメーター自体に対応するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="4e5da-319">This allows the `GetType` expression to return the `System.Type` object that corresponds to the generic type itself.</span></span>

<span data-ttu-id="4e5da-320">次の例で、`GetType`式。</span><span class="sxs-lookup"><span data-stu-id="4e5da-320">The following example demonstrates the `GetType` expression:</span></span>

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

<span data-ttu-id="4e5da-321">結果の出力は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-321">The resulting output is:</span></span>

```
Int32
Int32
String
Double[]
```


### <a name="typeofis-expressions"></a><span data-ttu-id="4e5da-322">TypeOf.式は、します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-322">TypeOf...Is Expressions</span></span>

<span data-ttu-id="4e5da-323">A`TypeOf...Is`式を使用して、実行時の型の値が指定された型と互換性のあるかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-323">A `TypeOf...Is` expression is used to check whether the run-time type of a value is compatible with a given type.</span></span> <span data-ttu-id="4e5da-324">最初のオペランドは値として分類する必要があります、再分類のラムダ メソッドにすることはできません、および参照型の制約のない型パラメーターの型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-324">The first operand must be classified as a value, cannot be a reclassified lambda method, and must be of a reference type or an unconstrained type parameter type.</span></span> <span data-ttu-id="4e5da-325">2 番目のオペランドは、型名である必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-325">The second operand must be a type name.</span></span> <span data-ttu-id="4e5da-326">値として分類があり、式の結果、`Boolean`値。</span><span class="sxs-lookup"><span data-stu-id="4e5da-326">The result of the expression is classified as a value and is a `Boolean` value.</span></span> <span data-ttu-id="4e5da-327">式の評価が`True`場合は、オペランドの実行時の型がある、id、既定、参照、配列、値の型または型に型パラメーター変換`False`それ以外の場合。</span><span class="sxs-lookup"><span data-stu-id="4e5da-327">The expression evaluates to `True` if the run-time type of the operand has an identity, default, reference, array, value type, or type parameter conversion to the type, `False` otherwise.</span></span> <span data-ttu-id="4e5da-328">式の型と特定の型の間の変換が存在しない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-328">A compile-time error occurs if no conversion exists between the type of the expression and the specific type.</span></span>

```antlr
TypeOfIsExpression
    : 'TypeOf' Expression 'Is' LineTerminator? TypeName
    ;
```

### <a name="is-expressions"></a><span data-ttu-id="4e5da-329">式は、します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-329">Is Expressions</span></span>

<span data-ttu-id="4e5da-330">`Is`または`IsNot`参照が等値比較を行う式を使用します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-330">An `Is` or `IsNot` expression is used to do a reference equality comparison.</span></span>

```antlr
IsExpression
    : Expression 'Is' LineTerminator? Expression
    | Expression 'IsNot' LineTerminator? Expression
    ;
```

<span data-ttu-id="4e5da-331">各式は、値として分類する必要があり、各式の型が参照型、制約のない型パラメーターの型、または null 許容値型にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-331">Each expression must be classified as a value and the type of each expression must be a reference type, an unconstrained type parameter type, or a nullable value type.</span></span> <span data-ttu-id="4e5da-332">1 つの式の型が、制約のない型パラメーターの型または null 許容値型の場合は、ただし、その他の式があります、リテラル`Nothing`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-332">If the type of one expression is an unconstrained type parameter type or nullable value type, however, the other expression must be the literal `Nothing`.</span></span>

<span data-ttu-id="4e5da-333">結果を値として分類され、として型指定された`Boolean`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-333">The result is classified as a value and is typed as `Boolean`.</span></span> <span data-ttu-id="4e5da-334">`Is`操作の評価に`True`両方の値が同じインスタンスを参照するか、両方の値が`Nothing`、または`False`それ以外の場合。</span><span class="sxs-lookup"><span data-stu-id="4e5da-334">An `Is` operation evaluates to `True` if both values refer to the same instance or both values are `Nothing`, or `False` otherwise.</span></span> <span data-ttu-id="4e5da-335">`IsNot`操作の評価に`False`両方の値が同じインスタンスを参照するか、両方の値が`Nothing`、または`True`それ以外の場合。</span><span class="sxs-lookup"><span data-stu-id="4e5da-335">An `IsNot` operation evaluates to `False` if both values refer to the same instance or both values are `Nothing`, or `True` otherwise.</span></span>


### <a name="getxmlnamespace-expressions"></a><span data-ttu-id="4e5da-336">GetXmlNamespace 式</span><span class="sxs-lookup"><span data-stu-id="4e5da-336">GetXmlNamespace Expressions</span></span>

<span data-ttu-id="4e5da-337">A`GetXmlNamespace`式から成るキーワード`GetXmlNamespace`とソース ファイルまたはコンパイル環境で宣言されている XML 名前空間の名前。</span><span class="sxs-lookup"><span data-stu-id="4e5da-337">A `GetXmlNamespace` expression consists of the keyword `GetXmlNamespace` and the name of an XML namespace declared by the source file or compilation environment.</span></span>

```antlr
GetXmlNamespaceExpression
    : 'GetXmlNamespace' OpenParenthesis XMLNamespaceName? CloseParenthesis
    ;
```

<span data-ttu-id="4e5da-338">`GetXmlNamespace`式は、値として分類され、その値のインスタンスである`System.Xml.Linq.XNamespace`を表す、 *XMLNamespaceName*します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-338">An `GetXmlNamespace` expression is classified as a value, and its value is an instance of `System.Xml.Linq.XNamespace` that represents the *XMLNamespaceName*.</span></span> <span data-ttu-id="4e5da-339">その型が使用できない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-339">If that type is not available, then a compile-time error will occur.</span></span>

<span data-ttu-id="4e5da-340">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-340">For example:</span></span>

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

<span data-ttu-id="4e5da-341">名前空間の名前の一部は、空白などの周囲の XML 規則が適用されますので、かっこ間にあるすべてと見なされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-341">Everything between the parentheses is considered part of the namespace name, so XML rules around things such as whitespace apply.</span></span> <span data-ttu-id="4e5da-342">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-342">For example:</span></span>

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

<span data-ttu-id="4e5da-343">XML 名前空間の式も省略できる場合、式が既定の XML 名前空間を表すオブジェクトを返します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-343">The XML namespace expression can also be omitted, in which case the expression returns the object that represents the default XML namespace.</span></span>


## <a name="member-access-expressions"></a><span data-ttu-id="4e5da-344">メンバー アクセス式</span><span class="sxs-lookup"><span data-stu-id="4e5da-344">Member Access Expressions</span></span>

<span data-ttu-id="4e5da-345">メンバー アクセス式は、エンティティのメンバーへのアクセスに使用されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-345">A member access expression is used to access a member of an entity.</span></span>

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

<span data-ttu-id="4e5da-346">形式のメンバー アクセス`E.I(Of A)`ここで、`E`式、非配列の型名、キーワード`Global`、または省略した場合と`I`は省略可能な型引数リストを持つ識別子`A`を評価および分類次のようにします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-346">A member access of the form `E.I(Of A)`, where `E` is an expression, a non-array type name, the keyword `Global`, or omitted and `I` is an identifier with an optional type argument list `A`, is evaluated and classified as follows:</span></span>

1. <span data-ttu-id="4e5da-347">場合`E`を省略すると、すぐに含むから式では、`With`ステートメントは、代わりに使用`E`され、メンバー アクセスが実行されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-347">If `E` is omitted, then the expression from the immediately containing `With` statement is substituted for `E` and the member access is performed.</span></span> <span data-ttu-id="4e5da-348">格納しているがない場合は`With`ステートメントでは、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-348">If there is no containing `With` statement, a compile-time error occurs.</span></span>

2. <span data-ttu-id="4e5da-349">場合`E`名前空間として分類されますまたは`E`キーワード`Global`、メンバー検索は指定した名前空間のコンテキストで実行し、します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-349">If `E` is classified as a namespace or `E` is the keyword `Global`, then the member lookup is done in the context of the specified namespace.</span></span> <span data-ttu-id="4e5da-350">場合`I`がで指定された型引数リストで、存在する場合、結果はそのメンバーは、同じ数の型パラメーターでその名前空間のアクセス可能なメンバーの名前。</span><span class="sxs-lookup"><span data-stu-id="4e5da-350">If `I` is the name of an accessible member of that namespace with the same number of type parameters as was supplied in the type argument list, if any, then the result is that member.</span></span> <span data-ttu-id="4e5da-351">結果は、名前空間または型によっては、メンバーとして分類されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-351">The result is classified as a namespace or a type depending on the member.</span></span> <span data-ttu-id="4e5da-352">それ以外の場合は、コンパイル時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-352">Otherwise, a compile-time error occurs.</span></span>

3. <span data-ttu-id="4e5da-353">場合`E`が、型または型として分類される式、メンバー検索は、指定した型のコンテキストで実行します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-353">If `E` is a type or an expression classified as a type, then the member lookup is done in the context of the specified type.</span></span> <span data-ttu-id="4e5da-354">場合`I`のアクセス可能なメンバーの名前を指定`E`、し`E.I`が評価され、次のように分類されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-354">If `I` is the name of an accessible member of `E`, then `E.I` is evaluated and classified as follows:</span></span>

    31. <span data-ttu-id="4e5da-355">場合`I`キーワード`New`と`E`コンパイル時エラーが発生し、列挙体ではありません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-355">If `I` is the keyword `New` and `E` is not an enumeration then a compile-time error occurs.</span></span>
    32. <span data-ttu-id="4e5da-356">場合`I`がで指定される型引数リストで、存在する場合、結果はその型の型パラメーターの同じ番号を持つ型を識別します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-356">If `I` identifies a type with the same number of type parameters as was supplied in the type argument list, if any, then the result is that type.</span></span>
    33. <span data-ttu-id="4e5da-357">場合`I`結果は、関連付けられている型の引数リストと関連付けられているターゲット式を指定せず、メソッド グループに 1 つまたは複数のメソッドを識別します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-357">If `I` identifies one or more methods, then the result is a method group with the associated type argument list and no associated target expression.</span></span>
    34. <span data-ttu-id="4e5da-358">場合`I`1 つまたは複数のプロパティと型を持たないを識別する引数リストが指定されて、結果は、プロパティ式のないグループに関連付けられたターゲット。</span><span class="sxs-lookup"><span data-stu-id="4e5da-358">If `I` identifies one or more properties and no type argument list was supplied, then the result is a property group with no associated target expression.</span></span>
    35. <span data-ttu-id="4e5da-359">場合`I`共有変数となしの型を識別する引数リストが指定されて、結果は、変数または値のいずれか。</span><span class="sxs-lookup"><span data-stu-id="4e5da-359">If `I` identifies a shared variable and no type argument list was supplied, then the result is either a variable or a value.</span></span> <span data-ttu-id="4e5da-360">かどうか、変数は読み取り専用と、変数が宣言されている型の共有のコンス トラクターの外部参照が発生した、結果は、共有変数の値`I`で`E`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-360">If the variable is read-only, and the reference occurs outside the shared constructor of the type in which the variable is declared, then the result is the value of the shared variable `I` in `E`.</span></span> <span data-ttu-id="4e5da-361">それ以外の場合、結果は、共有変数`I`で`E`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-361">Otherwise, the result is the shared variable `I` in `E`.</span></span>
    36. <span data-ttu-id="4e5da-362">場合`I`共有イベントとなしの型を識別する引数リストが指定されて、イベントに関連付けられたターゲット式を指定しないアクセスになります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-362">If `I` identifies a shared event and no type argument list was supplied, the result is an event access with no associated target expression.</span></span>
    37. <span data-ttu-id="4e5da-363">場合`I`定数となしの型を識別する引数リストを提供が、その結果は、その定数の値。</span><span class="sxs-lookup"><span data-stu-id="4e5da-363">If `I` identifies a constant and no type argument list was supplied, then the result is the value of that constant.</span></span>
    38. <span data-ttu-id="4e5da-364">場合`I`列挙体のメンバーと型を持たないを識別し、引数リストが指定されて、結果は、その列挙体メンバーの値。</span><span class="sxs-lookup"><span data-stu-id="4e5da-364">If `I` identifies an enumeration member and no type argument list was supplied, then the result is the value of that enumeration member.</span></span>
    39. <span data-ttu-id="4e5da-365">それ以外の場合、`E.I`無効なメンバー参照であり、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-365">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>

4. <span data-ttu-id="4e5da-366">場合`E`変数または、型が値として分類されます`T`、メンバー検索のコンテキストで実行し、`T`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-366">If `E` is classified as a variable or value, the type of which is `T`, then the member lookup is done in the context of `T`.</span></span> <span data-ttu-id="4e5da-367">場合`I`のアクセス可能なメンバーの名前を指定`T`、し`E.I`が評価され、次のように分類されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-367">If `I` is the name of an accessible member of `T`, then `E.I` is evaluated and classified as follows:</span></span>

    41. <span data-ttu-id="4e5da-368">場合`I`キーワード`New`、`E`は`Me`、 `MyBase`、または`MyClass`、型引数が指定されていない、結果はの型のインスタンスコンストラクターを表すメソッドグループ`E`の式が関連付けられているターゲット`E`と型引数の一覧がありません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-368">If `I` is the keyword `New`, `E` is  `Me`, `MyBase`, or `MyClass`, and no type arguments were supplied, then the result is a method group representing the instance constructors of the type of `E` with an associated target expression of `E` and no type argument list.</span></span> <span data-ttu-id="4e5da-369">それ以外の場合は、コンパイル時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-369">Otherwise, a compile-time error occurs.</span></span>
    42. <span data-ttu-id="4e5da-370">場合`I`場合は、拡張メソッドを含む 1 つまたは複数のメソッドを識別する`T`ない`Object`、結果は、関連付けられている型の引数リストとの関連付けられている対象の式は、メソッド グループ`E`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-370">If `I` identifies one or more methods, including extension methods if `T` is not `Object`, then the result is a method group with the associated type argument list and an associated target expression of `E`.</span></span>
    43. <span data-ttu-id="4e5da-371">場合`I`、結果の式が関連付けられているターゲット プロパティ グループは、1 つまたは複数のプロパティと引数が指定された種類がありませんを識別する`E`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-371">If `I` identifies one or more properties and no type arguments were supplied, then the result is a property group with an associated target expression of `E`.</span></span>
    44. <span data-ttu-id="4e5da-372">場合`I`結果は、変数または値のいずれか共有変数またはインスタンス変数と引数が指定されました、なしの型を識別します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-372">If `I` identifies a shared variable or an instance variable and no type arguments were supplied, then the result is either a variable or a value.</span></span> <span data-ttu-id="4e5da-373">変数は読み取り専用変数が宣言されている変数 (共有またはインスタンス) の種類に適したクラスのコンス トラクターの外部参照が発生し、かどうか、結果は、変数の値`I`で参照されるオブジェクトによって`E`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-373">If the variable is read-only, and the reference occurs outside a constructor of the class in which the variable is declared appropriate for the kind of variable (shared or instance), then the result is the value of the variable `I` in the object referenced by `E`.</span></span> <span data-ttu-id="4e5da-374">場合`T`、参照型では、結果は、変数、`I`によって参照されるオブジェクトで`E`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-374">If `T` is a reference type, then the result is the variable `I` in the object referenced by `E`.</span></span> <span data-ttu-id="4e5da-375">の場合`T`値型と式は、`E`変数として分類されると、結果は、変数は、それ以外の場合、結果は、値。</span><span class="sxs-lookup"><span data-stu-id="4e5da-375">Otherwise, if `T` is a value type and the expression `E` is classified as a variable, the result is a variable; otherwise the result is a value.</span></span>
    45. <span data-ttu-id="4e5da-376">場合`I`イベントとなしの型を識別する引数に指定された場合、結果は、イベントに関連付けられたターゲット式のアクセスは`E`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-376">If `I` identifies an event and no type arguments were supplied, the result is an event access with an associated target expression of `E`.</span></span>
    46. <span data-ttu-id="4e5da-377">場合`I`結果はその定数の値は、定数と引数が指定された種類がありませんを識別します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-377">If `I` identifies a constant and no type arguments were supplied, then the result is the value of that constant.</span></span>
    47. <span data-ttu-id="4e5da-378">場合`I`結果はその列挙体メンバーの値は、列挙体のメンバーと引数が指定された種類がありませんを識別します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-378">If `I` identifies an enumeration member and no type arguments were supplied, then the result is the value of that enumeration member.</span></span>
    48. <span data-ttu-id="4e5da-379">場合`T`は`Object`、結果は、関連付けられている型の引数リストと、関連付けられている対象の式の遅延バインディング アクセスとして分類される遅延バインディング メンバー ルックアップ`E`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-379">If `T` is `Object`, then the result is a late-bound member lookup classified as a late-bound access with the associated type argument list and an associated target expression of `E`.</span></span>

5. <span data-ttu-id="4e5da-380">それ以外の場合、`E.I`無効なメンバー参照であり、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-380">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>

<span data-ttu-id="4e5da-381">形式のメンバー アクセス`MyClass.I(Of A)`と等価`Me.I(Of A)`がすべてのメンバーにアクセスが、メンバーはオーバーライドできないかのように扱われます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-381">A member access of the form `MyClass.I(Of A)` is equivalent to `Me.I(Of A)`, but all members accessed on it are treated as if the members are non-overridable.</span></span> <span data-ttu-id="4e5da-382">このため、アクセスされるメンバーは、メンバーがアクセスされている値の実行時の型によっては影響ありません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-382">Thus, the member accessed will not be affected by the run-time type of the value on which the member is being accessed.</span></span>

<span data-ttu-id="4e5da-383">形式のメンバー アクセス`MyBase.I(Of A)`と等価`CType(Me, T).I(Of A)`場所`T`はメンバー アクセス式を含む型の直接の基本型です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-383">A member access of the form `MyBase.I(Of A)` is equivalent to `CType(Me, T).I(Of A)` where `T` is the direct base type of the type containing the member access expression.</span></span> <span data-ttu-id="4e5da-384">すべてのメソッド呼び出しは、呼び出されるメソッドはオーバーライドできないとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-384">All method invocations on it are treated as if the method being invoked is non-overridable.</span></span> <span data-ttu-id="4e5da-385">この形式のメンバー アクセスとも呼ばれますが、 *base アクセス*します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-385">This form of member access is also called a *base access*.</span></span>

<span data-ttu-id="4e5da-386">次の例でどのように`Me`、`MyBase`と`MyClass`関連。</span><span class="sxs-lookup"><span data-stu-id="4e5da-386">The following example demonstrates how `Me`, `MyBase` and `MyClass` relate:</span></span>

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

<span data-ttu-id="4e5da-387">このコードを出力します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-387">This code prints out:</span></span>

```
MoreDerived.F
Derived.F
Derived.F
```

<span data-ttu-id="4e5da-388">キーワードを使用して、メンバー アクセス式を開始する`Global`、キーワード表す最も外側の名前なし名前空間宣言が外側の名前空間をシャドウする場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-388">When a member access expression begins with the keyword `Global`, the keyword represents the outermost unnamed namespace, which is useful in situations where a declaration shadows an enclosing namespace.</span></span> <span data-ttu-id="4e5da-389">`Global`キーワードは、そのような状況で最も外側の名前空間を"エスケープ"処理を使用します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-389">The `Global` keyword allows "escaping" out to the outermost namespace in that situation.</span></span> <span data-ttu-id="4e5da-390">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-390">For example:</span></span>

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

<span data-ttu-id="4e5da-391">上記の例でも最初のメソッド呼び出しが無効ですので識別子`System`、クラスにバインド`System`、名前空間ではない`System`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-391">In the above example, the first method call is invalid because the identifier `System` binds to the class `System`, not the namespace `System`.</span></span> <span data-ttu-id="4e5da-392">アクセスする唯一の方法、`System`名前空間は、使用する`Global`最も外側の名前空間をエスケープします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-392">The only way to access the `System` namespace is to use `Global` to escape out to the outermost namespace.</span></span>

<span data-ttu-id="4e5da-393">期間の左側にある任意の式は不要であり、メンバー アクセスを実行しない限りは評価されない場合、アクセスされるメンバーを共有すると、遅延バインディング。</span><span class="sxs-lookup"><span data-stu-id="4e5da-393">If the member being accessed is shared, any expression on the left side of the period is superfluous and is not evaluated unless the member access is done late-bound.</span></span> <span data-ttu-id="4e5da-394">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-394">For example, consider the following code:</span></span>

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

<span data-ttu-id="4e5da-395">印刷`The value of F is: 10`ため関数`ReturnC`のインスタンスを指定するために呼び出す必要はありません`C`共有メンバーにアクセスする`F`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-395">It prints `The value of F is: 10` because the function `ReturnC` does not need to be called to provide an instance of `C` to access the shared member `F`.</span></span>


### <a name="identical-type-and-member-names"></a><span data-ttu-id="4e5da-396">同一の型およびメンバー名</span><span class="sxs-lookup"><span data-stu-id="4e5da-396">Identical Type and Member Names</span></span>

<span data-ttu-id="4e5da-397">名前のメンバーの種類と同じ名前を使用することは珍しくありません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-397">It is not uncommon to name members using the same name as their type.</span></span> <span data-ttu-id="4e5da-398">この場合、ただし、不都合な名前の非表示に発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-398">In that situation, however, inconvenient name hiding can occur:</span></span>

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

<span data-ttu-id="4e5da-399">前の例では、簡易名で`Color`で`DefaultColor`型の代わりに、インスタンス プロパティにバインドします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-399">In the previous example, the simple name `Color` in `DefaultColor` binds to the instance property instead of the type.</span></span> <span data-ttu-id="4e5da-400">インスタンス メンバーは、共有メンバーでは参照できません、ため、通常なりますエラー。</span><span class="sxs-lookup"><span data-stu-id="4e5da-400">Because an instance member cannot be referenced in a shared member, this would normally be an error.</span></span>

<span data-ttu-id="4e5da-401">ただし、特別な規則は、型へのアクセスをここでできます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-401">However, a special rule allows access to the type in this case.</span></span> <span data-ttu-id="4e5da-402">メンバー アクセス式の基本の式が簡易名、定数、フィールド、プロパティ、ローカル変数または型に同じ名前を持つパラメーターにバインドする場合は、メンバーまたは型のいずれか、基本式は参照できます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-402">If the base expression of a member access expression is a simple name and binds to a constant, field, property, local variable or parameter whose type has the same name, then the base expression can refer either to the member or the type.</span></span> <span data-ttu-id="4e5da-403">これは、ことができますいずれか 1 つからアクセスできるメンバーが同じであるために、曖昧になることはありません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-403">This can never result in ambiguity because the members that can be accessed off of either one are the same.</span></span>

### <a name="default-instances"></a><span data-ttu-id="4e5da-404">既定のインスタンス</span><span class="sxs-lookup"><span data-stu-id="4e5da-404">Default Instances</span></span>

<span data-ttu-id="4e5da-405">場合によっては、共通から派生したクラスは基底クラスは、通常、または常に 1 つのインスタンスのみがあります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-405">In some situations, classes derived from a common base class usually or always have only a single instance.</span></span> <span data-ttu-id="4e5da-406">たとえば、しかユーザー インターフェイスに表示するほとんどの windows では、いつでも、画面上に表示する 1 つのインスタンスがあります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-406">For example, most windows shown in a user interface only ever have one instance showing on the screen at any time.</span></span> <span data-ttu-id="4e5da-407">Visual Basic を自動的に生成するクラスのような作業を簡素化する*既定インスタンス*の各クラスに簡単に参照される、1 つのインスタンスを提供するクラス。</span><span class="sxs-lookup"><span data-stu-id="4e5da-407">To simplify working with these types of classes, Visual Basic can automatically generate *default instances* of the classes that provide a single, easily referenced instance for each class.</span></span>

<span data-ttu-id="4e5da-408">既定のインスタンスが常に作成、*ファミリ*1 つの特定の種類ではなく型のです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-408">Default instances are always created for a *family* of types rather than for one particular type.</span></span> <span data-ttu-id="4e5da-409">したがって Form1 フォームから派生したクラスの既定のインスタンスを作成する代わりには、既定のインスタンスはフォームから派生したすべてのクラスに対して作成されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-409">So instead of creating a default instance for a class Form1 that derives from Form, default instances are created for all classes derived from Form.</span></span> <span data-ttu-id="4e5da-410">これは、既定のインスタンスに特別にマークする個々 の基本クラスから派生したクラスがないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-410">This means that each individual class that derives from the base class does not have to be specially marked to have a default instance.</span></span>

<span data-ttu-id="4e5da-411">クラスの既定のインスタンスは、そのクラスの既定のインスタンスを返すコンパイラによって生成されたプロパティで表されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-411">The default instance of a class is represented by a compiler-generated property that returns the default instance of that class.</span></span> <span data-ttu-id="4e5da-412">クラスのメンバーとして生成されたプロパティと呼ばれる、*グループ クラス*の割り当てを管理して、特定の基底クラスから派生したすべてのクラスの既定のインスタンスを破棄します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-412">The property generated as a member of a class called the *group class* that manages allocating and destroying default instances for all classes derived from the particular base class.</span></span> <span data-ttu-id="4e5da-413">派生したすべてのクラスの既定のインスタンスのプロパティの例では、`Form`で収集される可能性があります、`MyForms`クラス。</span><span class="sxs-lookup"><span data-stu-id="4e5da-413">For example, all of the default instance properties of classes derived from `Form` may be collected in the `MyForms` class.</span></span> <span data-ttu-id="4e5da-414">グループ クラスのインスタンスが、式によって返されるかどうか`My.Forms`、次のコードが派生クラスの既定のインスタンスにアクセスし、`Form1`と`Form2`:</span><span class="sxs-lookup"><span data-stu-id="4e5da-414">If an instance of the group class is returned by the expression `My.Forms`, then the following code accesses the default instances of derived classes `Form1` and `Form2`:</span></span>

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

<span data-ttu-id="4e5da-415">既定のインスタンスは、それが最初に参照されるまでは作成されません。既に作成されていないかに設定されている場合に作成される既定のインスタンスの既定のインスタンスを表すプロパティをフェッチしています`Nothing`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-415">Default instances will not be created until the first reference to them; fetching the property representing the default instance causes the default instance to be created if it has not already been created or has been set to `Nothing`.</span></span> <span data-ttu-id="4e5da-416">既定のインスタンスの対象である場合、既定のインスタンスの存在をテストできるように、`Is`または`IsNot`オペレーターは、既定のインスタンスは作成されません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-416">To allow testing for the existence of a default instance, when a default instance is the target of an `Is` or `IsNot` operator, the default instance will not be created.</span></span> <span data-ttu-id="4e5da-417">したがって、既定のインスタンスがあるかどうかをテストすることは`Nothing`または既定のインスタンスを作成することがなく他のいくつか参照します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-417">Thus, it is possible to test whether a default instance is `Nothing` or some other reference without causing the default instance to be created.</span></span>

<span data-ttu-id="4e5da-418">既定のインスタンスは、簡単に既定のインスタンスがあるクラスの外部から既定のインスタンスを参照してください。</span><span class="sxs-lookup"><span data-stu-id="4e5da-418">Default instances are intended to make it easy to refer to the default instance from outside of the class that has the default instance.</span></span> <span data-ttu-id="4e5da-419">それを定義するクラス内での既定のインスタンスを使用すると、どのインスタンスで参照される、既定のインスタンスまたは現在のインスタンスつまり混乱が生じる場合があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-419">Using a default instance from within a class that defines it might cause confusion as to which instance is being referred to, i.e. the default instance or the current instance.</span></span> <span data-ttu-id="4e5da-420">たとえば、次のコードは値のみを変更します。 `x` 、既定のインスタンスの場合でも呼び出された別のインスタンスから。</span><span class="sxs-lookup"><span data-stu-id="4e5da-420">For example, the following code modifies only the value `x` in the default instance, even though it is being called from another instance.</span></span> <span data-ttu-id="4e5da-421">コードは、値を出力するため`5`の代わりに`10`:</span><span class="sxs-lookup"><span data-stu-id="4e5da-421">Thus the code would print the value `5` instead of `10`:</span></span>

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

<span data-ttu-id="4e5da-422">このような混乱を防ぐためには、既定のインスタンスから、インスタンス メソッド内で、既定のインスタンスの型を参照することはできません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-422">To prevent this kind of confusion, it is not valid to refer to a default instance from within an instance method of the default instance's type.</span></span>

#### <a name="default-instances-and-type-names"></a><span data-ttu-id="4e5da-423">既定のインスタンスと型名</span><span class="sxs-lookup"><span data-stu-id="4e5da-423">Default Instances and Type Names</span></span>

<span data-ttu-id="4e5da-424">既定のインスタンスは、その型の名前を使用して直接アクセスできる場合もあります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-424">A default instance may also be accessible directly through its type's name.</span></span> <span data-ttu-id="4e5da-425">場所の種類名では、式を使用できません、式のコンテキストでこの例では、`E`ここで、`E`既定のインスタンスを使用して、クラスの完全修飾名を表すに変更が`E'`ここで、`E'`を表します既定のインスタンスのプロパティをフェッチする式。</span><span class="sxs-lookup"><span data-stu-id="4e5da-425">In this case, in any expression context where the type name is not allowed the expression `E`, where `E` represents the fully qualified name of the class with a default instance, is changed to `E'`, where `E'` represents an expression that fetches the default instance property.</span></span> <span data-ttu-id="4e5da-426">たとえば、既定のインスタンスの派生クラスから`Form`次のコードは、前の例のコードに相当し、型の名前を既定のインスタンスへのアクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-426">For example, if default instances for classes derived from `Form` allow accessing the default instance through the type name, then the following code is equivalent to the code in the previous example:</span></span>

```vb
Module Main
    Sub Main()
        Form1.x = 10
        Console.WriteLine(Form2.y)
    End Sub
End Module
```

<span data-ttu-id="4e5da-427">つまり、型の名前からアクセス可能な既定のインスタンスは、型名を割り当てることもします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-427">This also means that a default instance that is accessible through its type's name is also assignable through the type name.</span></span> <span data-ttu-id="4e5da-428">たとえば、次のコードの既定のインスタンスを設定します`Form1`に`Nothing`:。</span><span class="sxs-lookup"><span data-stu-id="4e5da-428">For example, the following code sets the default instance of `Form1` to `Nothing`:</span></span>

```vb
Module Main
    Sub Main()
        Form1 = Nothing
    End Sub
End Module
```

<span data-ttu-id="4e5da-429">注意の意味`E.I`された`E`クラスを表しますと`I`表します共有メンバーは変更されません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-429">Note that the meaning of `E.I` were `E` represents a class and `I` represents a shared member does not change.</span></span> <span data-ttu-id="4e5da-430">まだ、このような式では、クラスのインスタンスから直接共有メンバーにアクセスしては既定のインスタンスを参照していません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-430">Such an expression still accesses the shared member directly off of the class instance and does not reference the default instance.</span></span>

#### <a name="group-classes"></a><span data-ttu-id="4e5da-431">クラスにグループ化</span><span class="sxs-lookup"><span data-stu-id="4e5da-431">Group Classes</span></span>

<span data-ttu-id="4e5da-432">`Microsoft.VisualBasic.MyGroupCollectionAttribute`属性の既定のインスタンスのファミリのグループのクラスを示します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-432">The `Microsoft.VisualBasic.MyGroupCollectionAttribute` attribute indicates the group class for a family of default instances.</span></span> <span data-ttu-id="4e5da-433">属性には、4 つのパラメーターがあります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-433">The attribute has four parameters:</span></span>

* <span data-ttu-id="4e5da-434">パラメーター`TypeToCollect`グループの基本クラスを指定します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-434">The parameter `TypeToCollect` specifies the base class for the group.</span></span> <span data-ttu-id="4e5da-435">すべてのインスタンス化可能なクラス (型パラメーター) に関係なく、この名前を持つ型から派生したオープン型のパラメーターを指定せずに自動的に既定のインスタンスになります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-435">All instantiable classes without open type parameters that derive from a type with this name (regardless of type parameters) will automatically have a default instance.</span></span>

* <span data-ttu-id="4e5da-436">パラメーター`CreateInstanceMethodName`既定インスタンスのプロパティの新しいインスタンスを作成するグループのクラスを呼び出す方法を指定します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-436">The parameter `CreateInstanceMethodName` specifies the method to call in the group class to create a new instance in a default instance property.</span></span>

* <span data-ttu-id="4e5da-437">パラメーター`DisposeInstanceMethodName`既定インスタンスのプロパティの値が割り当てられている場合、既定のインスタンスのプロパティを破棄するグループのクラスを呼び出す方法を指定します`Nothing`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-437">The parameter `DisposeInstanceMethodName` specifies the method to call in the group class to dispose of a default instance property if the default instance property is assigned the value `Nothing`.</span></span>

* <span data-ttu-id="4e5da-438">パラメーター`DefaultInstanceAlias`詳細式`E'`既定のインスタンスが、型名から直接アクセスできる場合は、クラス名の代わりにします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-438">The parameter `DefaultInstanceAlias` specifics the expression `E'` to substitute for the class name if the default instances are accessible directly through their type name.</span></span> <span data-ttu-id="4e5da-439">このパラメーターが場合`Nothing`または空の文字列の場合は、このグループの種類の既定のインスタンスは、型の名前を使用して直接アクセスできません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-439">If this parameter is `Nothing` or an empty string, default instances on this group type are not accessible directly through their type's name.</span></span> <span data-ttu-id="4e5da-440">(__に注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-440">(__Note.__</span></span> <span data-ttu-id="4e5da-441">Visual Basic 言語のすべての現在の実装で、`DefaultInstanceAlias`パラメーターは無視されます、コンパイラのコードでは可します)。</span><span class="sxs-lookup"><span data-stu-id="4e5da-441">In all current implementations of the Visual Basic language, the `DefaultInstanceAlias` parameter is ignored, except in compiler-provided code.)</span></span>

<span data-ttu-id="4e5da-442">複数の種類は、コンマを使用して最初の 3 つのパラメーターのメソッドや型の名前を分離することで、同じグループに収集できます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-442">Multiple types can be collected into the same group by separating the names of the types and methods in the first three parameters using commas.</span></span> <span data-ttu-id="4e5da-443">同じ数、各パラメーター内の項目の必要があり、リストの要素は順番に照合します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-443">There must be the same number of items in each parameter, and the list elements are matched in order.</span></span> <span data-ttu-id="4e5da-444">たとえば、次の属性宣言はから派生した型を収集します。 `C1`、`C2`または`C3`1 つのグループ。</span><span class="sxs-lookup"><span data-stu-id="4e5da-444">For example, the following attribute declaration collects types that derive from `C1`, `C2` or `C3` into a single group:</span></span>

```vb
<Microsoft.VisualBasic.MyGroupCollection("C1, C2, C3", _
    "CreateC1, CreateC2, CreateC3", _
    "DisposeC1, DisposeC2, DisposeC3", "My.Cs")>
Public NotInheritable Class MyCs
    ...
End Class
```

<span data-ttu-id="4e5da-445">Create メソッドのシグネチャは、の形式にする必要があります`Shared Function <Name>(Of T As {New, <Type>})(Instance Of T) As T`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-445">The signature of the create method must be of the form `Shared Function <Name>(Of T As {New, <Type>})(Instance Of T) As T`.</span></span> <span data-ttu-id="4e5da-446">Dispose メソッドは、の形式にする必要があります`Shared Sub <Name>(Of T As <Type>)(ByRef Instance Of T)`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-446">The dispose method must be of the form `Shared Sub <Name>(Of T As <Type>)(ByRef Instance Of T)`.</span></span> <span data-ttu-id="4e5da-447">したがって、前のセクションの例でグループ クラスを次のように宣言する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-447">Thus, the group class for the example in the preceding section could be declared as follows:</span></span>

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

<span data-ttu-id="4e5da-448">ソース ファイルには、派生クラスが宣言されている場合`Form1`、生成されるグループ クラスと等価になります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-448">If a source file declared a derived class `Form1`, the generated group class would be equivalent to:</span></span>

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

### <a name="extension-method-collection"></a><span data-ttu-id="4e5da-449">拡張メソッドのコレクション</span><span class="sxs-lookup"><span data-stu-id="4e5da-449">Extension Method Collection</span></span>

<span data-ttu-id="4e5da-450">拡張メソッド、メンバー アクセス式`E.I`名前を持つ拡張メソッドのすべての収集が収集した`I`現在のコンテキストで使用できます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-450">Extension methods for the member access expression `E.I` are collected by gathering all of the extension methods with the name `I` that are available in the current context:</span></span>

1. <span data-ttu-id="4e5da-451">最初に、式を含む各入れ子にされた型がオンになって、最も内側から開始し、最も外側にします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-451">First, each nested type containing the expression is checked, starting from the innermost and going to the outermost.</span></span>
2. <span data-ttu-id="4e5da-452">次に、入れ子になった各名前空間がオンになって、最も内側から開始し、最も外側の名前空間にします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-452">Then, each nested namespace is checked, starting from the innermost and going to the outermost namespace.</span></span>
3. <span data-ttu-id="4e5da-453">次に、ソース ファイルのインポートがチェックされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-453">Then, the imports in the source file are checked.</span></span>
4. <span data-ttu-id="4e5da-454">次に、コンパイル環境で定義されているインポートがチェックされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-454">Then, the imports defined by the compilation environment are checked.</span></span>

<span data-ttu-id="4e5da-455">拡張メソッドは、拡大ネイティブへの変換対象の式の型から最初のパラメーターの型の拡張メソッドがある場合にのみ収集されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-455">An extension method is collected only if there is a widening native conversion from the target expression type to the type of the first parameter of the extension method.</span></span> <span data-ttu-id="4e5da-456">定期的な簡易名式のバインディングとは異なり、検索の収集と*すべて*拡張メソッドはコレクションでは、拡張メソッドが見つかった場合は停止しません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-456">And unlike regular simple name expression binding, the search collects *all* extension methods; the collection does not stop when an extension method is found.</span></span> <span data-ttu-id="4e5da-457">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-457">For example:</span></span>

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

<span data-ttu-id="4e5da-458">この例でも`N2C1Extensions.M1`が前に見つかった`N1C1Extensions.M1`、両者は拡張メソッドとしてと見なされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-458">In this example, even though `N2C1Extensions.M1` is found before `N1C1Extensions.M1`, they both are considered as extension methods.</span></span> <span data-ttu-id="4e5da-459">収集されたすべての拡張メソッドと*カリー化*します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-459">Once all of the extension methods have been collected, they are then *curried*.</span></span> <span data-ttu-id="4e5da-460">カリー化拡張メソッドの呼び出しのターゲットを受け取るし、新しいメソッドのシグネチャ (指定されている) ために、削除する最初のパラメーターとその結果、拡張メソッドの呼び出しに適用されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-460">Currying takes the target of the extension method call and applies it to the extension method call, resulting in a new method signature with the first parameter removed (because it has been specified).</span></span> <span data-ttu-id="4e5da-461">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-461">For example:</span></span>

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

<span data-ttu-id="4e5da-462">上記の例では、適用したカリー化された結果で`v`に`Ext1.M`メソッドのシグネチャは、`Sub M(y As Integer)`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-462">In the above example, the curried result of applying `v` to `Ext1.M` is the method signature `Sub M(y As Integer)`.</span></span>

<span data-ttu-id="4e5da-463">拡張メソッドの最初のパラメーターを削除するだけでなくは最初のパラメーターの型の一部であるメソッドの型パラメーターの削除カリー化もします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-463">In addition to removing the first parameter of the extension method, currying also removes any method type parameters that are a part of the type of the first parameter.</span></span> <span data-ttu-id="4e5da-464">メソッド型パラメーターを持つ拡張メソッドをカリー化型の推定は、最初のパラメーターに適用され、結果は固定の推定される型パラメーターです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-464">When currying an extension method with method type parameter, type inference is applied to the first parameter and the result is fixed for any type parameters that are inferred.</span></span> <span data-ttu-id="4e5da-465">型の推定が失敗した場合、メソッドは無視されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-465">If type inference fails, the method is ignored.</span></span> <span data-ttu-id="4e5da-466">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-466">For example:</span></span>

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

<span data-ttu-id="4e5da-467">上記の例では、適用したカリー化された結果で`v`に`Ext1.M`メソッドのシグネチャは、`Sub M(Of U)(y As U)`ため、型パラメーター`T`カリー化の結果として推論され、は修正されています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-467">In the above example, the curried result of applying `v` to `Ext1.M` is the method signature `Sub M(Of U)(y As U)`, because the type parameter `T` is inferred as a result of the currying and is now fixed.</span></span> <span data-ttu-id="4e5da-468">ため、型パラメーター`U`オープンのパラメーターは、カリー化の一部として推論ができません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-468">Because the type parameter `U` was not inferred as a part of the currying, it remains an open parameter.</span></span> <span data-ttu-id="4e5da-469">同様に、ため、型パラメーター`T`適用の結果として推論される`v`に`Ext2.M`、パラメーターの型`y`として固定になります`Integer`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-469">Similarly, because the type parameter `T` is inferred as a result of applying `v` to `Ext2.M`, the type of parameter `y` becomes fixed as `Integer`.</span></span> <span data-ttu-id="4e5da-470">その他の種類を推論できません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-470">It will not be inferred to be any other type.</span></span> <span data-ttu-id="4e5da-471">署名を除くすべての制約をカリー化と`New`も制約が適用されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-471">When currying the signature, all constraints except for `New` constraints are also applied.</span></span> <span data-ttu-id="4e5da-472">制約が満たされていない、または、カリー化の一部としてしない推定された型に依存、拡張メソッドは無視されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-472">If the constraints are not satisfied, or depend on a type that was not inferred as a part of currying, the extension method is ignored.</span></span> <span data-ttu-id="4e5da-473">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-473">For example:</span></span>

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

<span data-ttu-id="4e5da-474">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-474">__Note.__</span></span> <span data-ttu-id="4e5da-475">拡張メソッドのカリー化を行うための主な理由の 1 つは、クエリ パターンのメソッドの引数を評価する前に、イテレーションの型を推論するクエリ式でできることです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-475">One of the main reasons for doing currying of extension methods is that it allows query expressions to infer the type of the iteration before evaluating the arguments to a query pattern method.</span></span> <span data-ttu-id="4e5da-476">ほとんどのクエリ パターンのメソッド、ラムダ式を必要とする型の推定自体を実行するためのクエリ式の評価プロセスかなりシンプルです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-476">Since most query pattern methods take lambda expressions, which require type inference themselves, this greatly simplifies the process of evaluating a query expression.</span></span>

<span data-ttu-id="4e5da-477">通常のインターフェイスの継承とは異なり互いに関連していない 2 つのインターフェイスを拡張する拡張メソッドは、同じカリー化されたシグネチャがあるない限り、使用可能なは。</span><span class="sxs-lookup"><span data-stu-id="4e5da-477">Unlike normal interface inheritance, extension methods that extend two interfaces that do not relate to one another are available, as long as they do not have the same curried signature:</span></span>

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

<span data-ttu-id="4e5da-478">最後に、遅延バインディングの実行時に、メソッドは考慮されませんが、その拡張機能に注意してください。</span><span class="sxs-lookup"><span data-stu-id="4e5da-478">Finally, it is important to remember that extension methods are not considered when doing late binding:</span></span>

```vb
Module Test
    Sub Main()
        Dim o As Object = ...

        ' Ignores extension methods
        o.M1()
    End Sub
End Module
```

## <a name="dictionary-member-access-expressions"></a><span data-ttu-id="4e5da-479">ディクショナリ メンバー アクセス式</span><span class="sxs-lookup"><span data-stu-id="4e5da-479">Dictionary Member Access Expressions</span></span>

<span data-ttu-id="4e5da-480">A*ディクショナリ メンバー アクセス式*コレクションのメンバーを検索するために使用します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-480">A *dictionary member access expression* is used to look up a member of a collection.</span></span> <span data-ttu-id="4e5da-481">ディクショナリ メンバー アクセスは、の形式をとります`E!I`ここで、`E`値として分類される式を指定し、`I`識別子です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-481">A dictionary member access takes the form of `E!I`, where `E` is an expression that is classified as a value and `I` is an identifier.</span></span>

```antlr
DictionaryAccessExpression
    : Expression? '!' IdentifierOrKeyword
    ;
```

<span data-ttu-id="4e5da-482">式の型は、1 つによってインデックス付けされた既定のプロパティをいる必要があります`String`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="4e5da-482">The type of the expression must have a default property indexed by a single `String` parameter.</span></span> <span data-ttu-id="4e5da-483">ディクショナリ メンバー アクセス式`E!I`式に変換されます`E.D("I")`ここで、`D`の既定のプロパティは、`E`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-483">The dictionary member access expression `E!I` is transformed into the expression `E.D("I")`, where `D` is the default property of `E`.</span></span> <span data-ttu-id="4e5da-484">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-484">For example:</span></span>

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

<span data-ttu-id="4e5da-485">感嘆符がない式では、すぐに含むから式で指定されている場合`With`ステートメントが使用されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-485">If an exclamation point is specified with no expression, the expression from the immediately containing `With` statement is assumed.</span></span> <span data-ttu-id="4e5da-486">格納しているがない場合は`With`ステートメントでは、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-486">If there is no containing `With` statement, a compile-time error occurs.</span></span>


## <a name="invocation-expressions"></a><span data-ttu-id="4e5da-487">Invocation 式</span><span class="sxs-lookup"><span data-stu-id="4e5da-487">Invocation Expressions</span></span>

<span data-ttu-id="4e5da-488">Invocation 式は、呼び出し対象と省略可能な引数リストで構成されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-488">An invocation expression consists of an invocation target and an optional argument list.</span></span>

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

<span data-ttu-id="4e5da-489">対象の式はメソッド グループ、またはデリゲート型の値として分類する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-489">The target expression must be classified as a method group or a value whose type is a delegate type.</span></span> <span data-ttu-id="4e5da-490">対象の式がデリゲート型の値としている場合、invocation 式のターゲットのメソッドのグループになります、`Invoke`デリゲートの型と、対象の式のメンバーは、メソッドの関連付けられている対象の式になりますグループ。</span><span class="sxs-lookup"><span data-stu-id="4e5da-490">If the target expression is a value whose type is a delegate type, then the target of the invocation expression becomes the method group for the `Invoke` member of the delegate type and the target expression becomes the associated target expression of the method group.</span></span>

<span data-ttu-id="4e5da-491">引数リストが 2 つのセクション: 位置指定引数と名前付き引数。</span><span class="sxs-lookup"><span data-stu-id="4e5da-491">An argument list has two sections: positional arguments and named arguments.</span></span> <span data-ttu-id="4e5da-492">*位置指定引数*式は、名前付き引数の前にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-492">*Positional arguments* are expressions and must precede any named arguments.</span></span> <span data-ttu-id="4e5da-493">*名前付き引数*開始後に、キーワードに一致する識別子を持つ`:=`と式。</span><span class="sxs-lookup"><span data-stu-id="4e5da-493">*Named arguments* start with an identifier that can match keywords, followed by `:=` and an expression.</span></span>

<span data-ttu-id="4e5da-494">メソッド グループには、1 つのアクセス可能なメソッドのみが含まれる場合、インスタンスと拡張機能の両方のメソッド、およびそのメソッドを含む引数を受け取らない関数は、メソッド グループは空の引数リストを持つ invocation 式として解釈され、結果は指定された引数リストで、invocation 式のターゲットとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-494">If the method group only contains one accessible method, including both instance and extension methods, and that method takes no arguments and is a function, then the method group is interpreted as an invocation expression with an empty argument list and the result is used as the target of an invocation expression with the provided argument list(s).</span></span> <span data-ttu-id="4e5da-495">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-495">For example:</span></span>

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

<span data-ttu-id="4e5da-496">それ以外の場合、オーバー ロードの解決は、指定した引数リストの最適な方法を選択するメソッドに適用されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-496">Otherwise, overload resolution is applied to the methods to pick the most applicable method for the given argument list(s).</span></span> <span data-ttu-id="4e5da-497">最適なメソッドが関数の場合は、invocation 式の結果は、関数の戻り値の型として型指定された値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-497">If the most applicable method is a function, then the result of the invocation expression is classified as a value typed as the return type of the function.</span></span> <span data-ttu-id="4e5da-498">最適なメソッドがサブルーチンの場合は、結果が void として分類されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-498">If the most applicable method is a subroutine, then the result is classified as void.</span></span> <span data-ttu-id="4e5da-499">最適なメソッドが本文がない部分メソッドの場合は、invocation 式は無視され、結果は void として分類されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-499">If the most applicable method is a partial method that has no body, then the invocation expression is ignored and the result is classified as void.</span></span>

<span data-ttu-id="4e5da-500">事前バインディングされた呼び出し式の引数は、対応するパラメーターが、対象のメソッドで宣言されている順序で評価されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-500">For an early-bound invocation expression, the arguments are evaluated in the order in which the corresponding parameters are declared in the target method.</span></span> <span data-ttu-id="4e5da-501">遅延バインディング メンバー アクセス式の場合、メンバー アクセス式で表示される順序で評価されます。 セクションを参照して[遅延バインディング式](expressions.md#late-bound-expressions)。</span><span class="sxs-lookup"><span data-stu-id="4e5da-501">For a late-bound member access expression, they are evaluated in the order in which they appear in the member access expression: see Section [Late-Bound Expressions](expressions.md#late-bound-expressions).</span></span>

## <a name="overloaded-method-resolution"></a><span data-ttu-id="4e5da-502">オーバー ロードされたメソッドの解決方法:</span><span class="sxs-lookup"><span data-stu-id="4e5da-502">Overloaded Method Resolution:</span></span>
<span data-ttu-id="4e5da-503">汎用性、引数リスト、渡す引数、および省略可能なパラメーター、条件付きのメソッドの引数のピッキングへの適用性を一覧表示し、入力引数を推定メンバー/型の引数が指定の特異性、オーバー ロードの解決: 」セクションを参照してください。[オーバー ロードの解決](overload-resolution.md)します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-503">For Overload Resolution, Specificity of members/types given an argument list, Genericity, Applicability to Argument List, Passing Arguments, and Picking Arguments for Optional Parameters, Conditional Methods, and Type Argument Inference: see Section [Overload Resolution](overload-resolution.md).</span></span>

## <a name="index-expressions"></a><span data-ttu-id="4e5da-504">インデックス式</span><span class="sxs-lookup"><span data-stu-id="4e5da-504">Index Expressions</span></span>

<span data-ttu-id="4e5da-505">*インデックス式*結果が配列の要素またはプロパティ アクセスにプロパティ グループを再分類します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-505">An *index expression* results in an array element or reclassifies a property group into a property access.</span></span> <span data-ttu-id="4e5da-506">順序、式、開きかっこを入力、インデックス引数リスト、および閉じかっこでインデックスの式で構成されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-506">An index expression consists of, in order, an expression, an opening parenthesis, an index argument list, and a closing parenthesis.</span></span>

```antlr
IndexExpression
    : Expression OpenParenthesis ArgumentList? CloseParenthesis
    ;
```

<span data-ttu-id="4e5da-507">インデックスの式のターゲットは、プロパティ グループまたは値のいずれかとして分類する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-507">The target of the index expression must be classified as either a property group or a value.</span></span> <span data-ttu-id="4e5da-508">インデックスの式は、次のように処理されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-508">An index expression is processed as follows:</span></span>

* <span data-ttu-id="4e5da-509">対象の式が値として分類される場合と、その型が配列型でない場合`Object`、または`System.Array`種類が既定のプロパティをいる必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-509">If the target expression is classified as a value and if its type is not an array type, `Object`, or `System.Array`, the type must have a default property.</span></span> <span data-ttu-id="4e5da-510">インデックスは、すべての型の既定のプロパティを表すプロパティ グループで実行されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-510">The index is performed on a property group that represents all of the default properties of the type.</span></span> <span data-ttu-id="4e5da-511">Visual Basic でのパラメーターなしの既定のプロパティの宣言ではありませんが、他の言語は、このようなプロパティを宣言することができます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-511">Although it is not valid to declare a parameterless default property in Visual Basic, other languages may allow declaring such a property.</span></span> <span data-ttu-id="4e5da-512">そのため、引数なしでのプロパティのインデックス作成が許可されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-512">Consequently, indexing a property with no arguments is allowed.</span></span>

* <span data-ttu-id="4e5da-513">式の結果配列型の値に、引数リストの引数の数が配列型のランクと同じである必要があり、名前付き引数を含まない場合があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-513">If the expression results in a value of an array type, the number of arguments in the argument list must be the same as the rank of the array type and may not include named arguments.</span></span> <span data-ttu-id="4e5da-514">インデックスのいずれかが無効の場合、実行時に、`System.IndexOutOfRangeException`例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-514">If any of the indexes are invalid at run time, a `System.IndexOutOfRangeException` exception is thrown.</span></span> <span data-ttu-id="4e5da-515">各式は、型に暗黙的に変換可能である必要があります`Integer`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-515">Each expression must be implicitly convertible to type `Integer`.</span></span> <span data-ttu-id="4e5da-516">インデックスの式の結果は、指定したインデックス位置にある変数し、変数として分類されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-516">The result of the index expression is the variable at the specified index and is classified as a variable.</span></span>

* <span data-ttu-id="4e5da-517">式は、プロパティ グループに分類は、プロパティの 1 つはインデックス引数のリストに適用できるかどうかを判断するオーバー ロードの解決が使用されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-517">If the expression is classified as a property group, overload resolution is used to determine whether one of the properties is applicable to the index argument list.</span></span> <span data-ttu-id="4e5da-518">プロパティ グループにのみを持つ 1 つのプロパティが含まれるかどうか、`Get`アクセサー、およびそのアクセサーには引数を受け取らないかどうかは、プロパティ グループは空の引数リストを持つインデックス式として解釈されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-518">If the property group only contains one property that has a `Get` accessor and if that accessor takes no arguments, then the property group is interpreted as an index expression with an empty argument list.</span></span> <span data-ttu-id="4e5da-519">結果は、現在のインデックスの式のターゲットとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-519">The result is used as the target of the current index expression.</span></span> <span data-ttu-id="4e5da-520">適用可能なプロパティがない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-520">If no properties are applicable, then a compile-time error occurs.</span></span> <span data-ttu-id="4e5da-521">それ以外の場合、式の結果には、プロパティ アクセスのプロパティ グループに関連付けられたターゲット式 (ある場合)。</span><span class="sxs-lookup"><span data-stu-id="4e5da-521">Otherwise, the expression results in a property access with the associated target expression (if any) of the property group.</span></span>

* <span data-ttu-id="4e5da-522">遅延バインディング プロパティ グループ、または型が値として式が分類される場合`Object`または`System.Array`インデックスの式の処理が実行時まで延期および遅延バインディングには、インデックスを作成します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-522">If the expression is classified as a late-bound property group or as a value whose type is `Object` or `System.Array`, the processing of the index expression is deferred until run time and the indexing is late-bound.</span></span> <span data-ttu-id="4e5da-523">として型指定された式の結果、遅延バインディング プロパティ アクセスで`Object`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-523">The expression results in a late-bound property access typed as `Object`.</span></span> <span data-ttu-id="4e5da-524">関連付けられている対象の式は、値である場合は、対象の式または、プロパティ グループの関連付けられている対象の式のいずれかです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-524">The associated target expression is either the target expression, if it is a value, or the associated target expression of the property group.</span></span> <span data-ttu-id="4e5da-525">実行時に、式は、次のように処理されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-525">At run time the expression is processed as follows:</span></span>

* <span data-ttu-id="4e5da-526">場合は、式は、遅延バインディング プロパティ グループに分類されます、式の可能性があります (メンバーが、インスタンスまたは共有変数の場合)、メソッド グループ、プロパティ グループ、または値の結果。</span><span class="sxs-lookup"><span data-stu-id="4e5da-526">If the expression is classified as a late-bound property group, the expression may result in a method group, a property group, or a value (if the member is an instance or shared variable).</span></span> <span data-ttu-id="4e5da-527">結果が、メソッド グループまたはプロパティのグループの場合は、オーバー ロードの解決は、引数の一覧については、適切な方法をグループに適用されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-527">If the result is a method group or property group, overload resolution is applied to the group to determine the correct method for the argument list.</span></span> <span data-ttu-id="4e5da-528">オーバー ロードの解決に失敗した場合、`System.Reflection.AmbiguousMatchException`例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-528">If overload resolution fails, a `System.Reflection.AmbiguousMatchException` exception is thrown.</span></span> <span data-ttu-id="4e5da-529">プロパティ アクセス、または呼び出しとして結果を処理し、結果が返されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-529">Then the result is processed either as a property access or as an invocation and the result is returned.</span></span> <span data-ttu-id="4e5da-530">サブルーチンの呼び出しがある場合、結果は、`Nothing`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-530">If the invocation is of a subroutine, the result is `Nothing`.</span></span>

* <span data-ttu-id="4e5da-531">対象の式の実行時の型が配列型または`System.Array`インデックスの式の結果は、指定したインデックス位置にある変数の値。</span><span class="sxs-lookup"><span data-stu-id="4e5da-531">If the run-time type of the target expression is an array type or `System.Array`, the result of the index expression is the value of the variable at the specified index.</span></span>

* <span data-ttu-id="4e5da-532">それ以外の場合、式の実行時の型は、既定のプロパティをいる必要があり、インデックスを表すすべての型の既定のプロパティのプロパティ グループで実行します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-532">Otherwise, the run-time type of the expression must have a default property and the index is performed on the property group that represents all of the default properties on the type.</span></span> <span data-ttu-id="4e5da-533">型の既定のプロパティがない、`System.MissingMemberException`例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-533">If the type has no default property, then a `System.MissingMemberException` exception is thrown.</span></span>


## <a name="new-expressions"></a><span data-ttu-id="4e5da-534">新しい式</span><span class="sxs-lookup"><span data-stu-id="4e5da-534">New Expressions</span></span>

<span data-ttu-id="4e5da-535">`New`演算子を使用して、型の新しいインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-535">The `New` operator is used to create new instances of types.</span></span> <span data-ttu-id="4e5da-536">次の 4 つの形式としては、`New`式。</span><span class="sxs-lookup"><span data-stu-id="4e5da-536">There are four forms of `New` expressions:</span></span>

* <span data-ttu-id="4e5da-537">オブジェクト作成式は、クラス型と値の型の新しいインスタンスを作成に使用されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-537">Object-creation expressions are used to create new instances of class types and value types.</span></span>

* <span data-ttu-id="4e5da-538">配列作成式は、配列型の新しいインスタンスを作成に使用されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-538">Array-creation expressions are used to create new instances of array types.</span></span>

* <span data-ttu-id="4e5da-539">型のデリゲートの新しいインスタンスを作成するデリゲート作成式 (これは、オブジェクト作成式から異なる構文がありません) が使用されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-539">Delegate-creation expressions (which do not have a distinct syntax from object-creation expressions) are used to create new instances of delegate types.</span></span>

* <span data-ttu-id="4e5da-540">匿名クラス型の新しいインスタンスを作成する匿名オブジェクト作成式が使用されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-540">Anonymous object-creation expressions are used to create new instances of anonymous class types.</span></span>

```antlr
NewExpression
    : ObjectCreationExpression
    | ArrayExpression
    | AnonymousObjectCreationExpression
    ;
```

<span data-ttu-id="4e5da-541">A`New`式は値として分類され、結果は、型の新しいインスタンス。</span><span class="sxs-lookup"><span data-stu-id="4e5da-541">A `New` expression is classified as a value and the result is the new instance of the type.</span></span>


### <a name="object-creation-expressions"></a><span data-ttu-id="4e5da-542">オブジェクト作成式</span><span class="sxs-lookup"><span data-stu-id="4e5da-542">Object-Creation Expressions</span></span>

<span data-ttu-id="4e5da-543">オブジェクト作成式を使用して、クラス型または構造型の新しいインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-543">An object-creation expression is used to create a new instance of a class type or a structure type.</span></span>

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

<span data-ttu-id="4e5da-544">オブジェクト作成式の型がクラス型、構造体型、または型パラメーターである必要があります、`New`制約することはできません、`MustInherit`クラス。</span><span class="sxs-lookup"><span data-stu-id="4e5da-544">The type of an object creation expression must be a class type, a structure type, or a type parameter with a `New` constraint and cannot be a `MustInherit` class.</span></span> <span data-ttu-id="4e5da-545">フォームのオブジェクト作成式が指定`New T(A)`ここで、`T`がクラス型または構造体の型と`A`、省略可能な引数リストは、オーバー ロードの解決の正しいコンス トラクターを決定します`T`を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-545">Given an object creation expression of the form `New T(A)`, where `T` is a class type or structure type and `A` is an optional argument list, overload resolution determines the correct constructor of `T` to call.</span></span> <span data-ttu-id="4e5da-546">型パラメーターで、`New`制約は、1 つのパラメーターなしのコンス トラクターを持つと見なされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-546">A type parameter with a `New` constraint is considered to have a single, parameterless constructor.</span></span> <span data-ttu-id="4e5da-547">呼び出し可能なコンス トラクターがない場合は、コンパイル時エラーが発生します。それ以外の場合、式の結果の新しいインスタンスの作成に`T`選択したコンス トラクターを使用します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-547">If no constructor is callable, a compile-time error occurs; otherwise the expression results in the creation of a new instance of `T` using the chosen constructor.</span></span> <span data-ttu-id="4e5da-548">引数がない場合は、かっこは省略できます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-548">If there are no arguments, the parentheses may be omitted.</span></span>

<span data-ttu-id="4e5da-549">インスタンスが割り当てられているインスタンスは、クラス型または値型かどうかによって異なります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-549">Where an instance is allocated depends on whether the instance is a class type or a value type.</span></span> <span data-ttu-id="4e5da-550">`New` クラス型のインスタンスは、スタックに直接値型の新しいインスタンスを作成中に、システム ヒープに作成されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-550">`New` instances of class types are created on the system heap, while new instances of value types are created directly on the stack.</span></span>

<span data-ttu-id="4e5da-551">必要に応じて、オブジェクト作成式は、コンス トラクターの引数の後にメンバーの初期化子の一覧を指定できます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-551">An object-creation expression can optionally specify a list of member initializers after the constructor arguments.</span></span> <span data-ttu-id="4e5da-552">これらのメンバー初期化子のキーワードが付いて`With`、初期化子リストは、のコンテキストでの場合と解釈されます、`With`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="4e5da-552">These member initializers are prefixed with the keyword `With`, and the initializer list is interpreted as if it was in the context of a `With` statement.</span></span> <span data-ttu-id="4e5da-553">たとえば、クラスを考えてみます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-553">For example, given the class:</span></span>

```vb
Class Customer
    Dim Name As String
    Dim Address As String
End Class
```

<span data-ttu-id="4e5da-554">コード:</span><span class="sxs-lookup"><span data-stu-id="4e5da-554">The code:</span></span>

```vb
Module Test
    Sub Main()
        Dim x As New Customer() With { .Name = "Bob Smith", _
            .Address = "123 Main St." }
    End Sub
End Module
```

<span data-ttu-id="4e5da-555">約と同等です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-555">Is roughly equivalent to:</span></span>

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

<span data-ttu-id="4e5da-556">各初期化子に割り当てるには、名前を指定する必要があり、名前にする必要があります以外`ReadOnly`; 構築される型のプロパティのインスタンス変数または構築される型がある場合に、メンバー アクセスが遅延バインディングできません`Object`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-556">Each initializer must specify a name to assign, and the name must be a non-`ReadOnly` instance variable or property of the type being constructed; the member access will not be late bound if the type being constructed is `Object`.</span></span> <span data-ttu-id="4e5da-557">初期化子を使用していない可能性があります、`Key`キーワード。</span><span class="sxs-lookup"><span data-stu-id="4e5da-557">Initializers may not use the `Key` keyword.</span></span> <span data-ttu-id="4e5da-558">型内の各メンバーは、1 回のみ初期化できます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-558">Each member in a type can only be initialized once.</span></span> <span data-ttu-id="4e5da-559">ただし、初期化子式では、相互に参照可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-559">The initializer expressions, however, may refer to each other.</span></span> <span data-ttu-id="4e5da-560">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-560">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim x As New Customer() With { .Name = "Bob Smith", _
            .Address = .Name & " St." }
    End Sub
End Module
```

<span data-ttu-id="4e5da-561">初期化子が左から右に割り当てられた、コンス トラクターが実行された後にインスタンス変数初期化子は、まだ初期化されていないメンバーを参照している場合は表示されるように値します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-561">The initializers are assigned left-to-right, so if an initializer refers to a member that has not been initialized yet, it will see whatever value the instance variable after the constructor ran:</span></span>

```vb
Module Test
    Sub Main()
        ' The value of Address will be " St." since Name has not been
        ' assigned yet.
        Dim x As New Customer() With { .Address = .Name & " St." }
    End Sub
End Module
```

<span data-ttu-id="4e5da-562">初期化子を入れ子になんだことができます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-562">Initializers can be nested:</span></span>

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

<span data-ttu-id="4e5da-563">作成される型がコレクション型とインスタンス メソッドの名前が場合`Add`(拡張メソッドと共有メソッド) を含むし、オブジェクト作成式を指定できます、キーワードでコレクション初期化子`From`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-563">If the type being created is a collection type and has an instance method named `Add` (including extension methods and shared methods), then the object-creation expression can specify a collection initializer prefixed by the keyword `From`.</span></span> <span data-ttu-id="4e5da-564">オブジェクト作成式には、メンバー初期化子とコレクション初期化子の両方を指定できません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-564">An object-creation expression cannot specify both a member initializer and a collection initializer.</span></span> <span data-ttu-id="4e5da-565">コレクション初期化子内の各要素がの呼び出しに引数として渡される、`Add`関数。</span><span class="sxs-lookup"><span data-stu-id="4e5da-565">Each element in the collection initializer is passed as an argument to an invocation of the `Add` function.</span></span> <span data-ttu-id="4e5da-566">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-566">For example:</span></span>

```vb
Dim list = New List(Of Integer)() From { 1, 2, 3, 4 }
```

<span data-ttu-id="4e5da-567">上の式は、下の式と同等です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-567">is equivalent to:</span></span>

```vb
Dim list = New List(Of Integer)()
list.Add(1)
list.Add(2)
list.Add(3)
```

<span data-ttu-id="4e5da-568">要素がそれ自体コレクション初期化子である場合は、サブ コレクション初期化子の各要素は、個々 の引数として渡されます、`Add`関数。</span><span class="sxs-lookup"><span data-stu-id="4e5da-568">If an element is a collection initializer itself, each element of the sub-collection initializer will be passed as an individual argument to the `Add` function.</span></span> <span data-ttu-id="4e5da-569">次: たとえば、</span><span class="sxs-lookup"><span data-stu-id="4e5da-569">For example, the following:</span></span>

```vb
Dim dict = Dictionary(Of Integer, String) From { { 1, "One" },{ 2, "Two" } }
```

<span data-ttu-id="4e5da-570">上の式は、下の式と同等です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-570">is equivalent to:</span></span>

```vb
Dim dict = New Dictionary(Of Integer, String)
dict.Add(1, "One")
dict.Add(2, "Two")
```

<span data-ttu-id="4e5da-571">この拡張は常に実行し、1 レベル深い; はしか行われますその後、サブの初期化子は配列リテラルと見なされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-571">This expansion is always done and is only ever done one level deep; after that, sub-initializers are considered array literals.</span></span> <span data-ttu-id="4e5da-572">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-572">For example:</span></span>

```vb
' Error: List(Of T) does not have an Add method that takes two parameters.
Dim list = New List(Of Integer())() From { { 1, 2 }, { 3, 4 } }

' OK, this initializes the dictionary with (Integer, Integer()) pairs.
Dim dict = New Dictionary(Of Integer, Integer())() From _
        { {  1, { 2, 3 } }, { 3, { 4, 5 } } }
```


### <a name="array-expressions"></a><span data-ttu-id="4e5da-573">配列式</span><span class="sxs-lookup"><span data-stu-id="4e5da-573">Array Expressions</span></span>

<span data-ttu-id="4e5da-574">配列式を使用して、配列型の新しいインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-574">An array expression is used to create a new instance of an array type.</span></span> <span data-ttu-id="4e5da-575">配列式の 2 種類があります。 配列作成式では、および配列リテラル。</span><span class="sxs-lookup"><span data-stu-id="4e5da-575">There are two types of array expressions: array creation expressions, and array literals.</span></span>

#### <a name="array-creation-expressions"></a><span data-ttu-id="4e5da-576">配列作成式</span><span class="sxs-lookup"><span data-stu-id="4e5da-576">Array creation expressions</span></span>

<span data-ttu-id="4e5da-577">配列サイズ初期化修飾子は、指定した場合、個々 の引数の配列のサイズの初期化の引数リストから削除することで、結果の配列型が派生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-577">If an array size initialization modifier is supplied, the resulting array type is derived by deleting each of the individual arguments from the array size initialization argument list.</span></span> <span data-ttu-id="4e5da-578">各引数の値は、新しく割り当てられた配列のインスタンスに対応する次元の上限を決定します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-578">The value of each argument determines the upper bound of the corresponding dimension in the newly allocated array instance.</span></span> <span data-ttu-id="4e5da-579">空のコレクション初期化子の式には、引数リストの各引数は、定数である必要があり、コレクション初期化子の式のリストで指定されたランクと次元の長さが一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-579">If the expression has a non-empty collection initializer, each argument in the argument list must be a constant, and the rank and dimension lengths specified by the expression list must match those of the collection initializer.</span></span>

```vb
Dim a() As Integer = New Integer(2) {}
Dim b() As Integer = New Integer(2) { 1, 2, 3 }
Dim c(,) As Integer = New Integer(1, 2) { { 1, 2, 3 } , { 4, 5, 6 } }

' Error, length/initializer mismatch.
Dim d() As Integer = New Integer(2) { 0, 1, 2, 3 }
```

<span data-ttu-id="4e5da-580">配列サイズ初期化修飾子が指定されていない場合、型名は、配列型である必要があり、コレクション初期化子が空にするか、含まれる指定された配列型のランクと入れ子のレベル数が同じ必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-580">If an array size initialization modifier is not supplied, then the type name must be an array type and the collection initializer must be empty or have the same number of levels of nesting as the rank of the specified array type.</span></span> <span data-ttu-id="4e5da-581">最も内側の入れ子レベル内の要素のすべての配列の要素の型に暗黙的に変換できる必要があり、値として分類する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-581">All of the elements in the innermost nesting level must be implicitly convertible to the element type of the array and must be classified as a value.</span></span> <span data-ttu-id="4e5da-582">入れ子になったコレクション初期化子のそれぞれの要素の数は、常に同じレベルの他のコレクションのサイズと一致である必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-582">The number of elements in each nested collection initializer must always be consistent with the size of the other collections at the same level.</span></span> <span data-ttu-id="4e5da-583">各次元の長さは、コレクション初期化子の対応する入れ子のレベルの各要素の数から推論されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-583">The individual dimension lengths are inferred from the number of elements in each of the corresponding nesting levels of the collection initializer.</span></span> <span data-ttu-id="4e5da-584">コレクション初期化子が空の場合は、各次元の長さは 0 です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-584">If the collection initializer is empty, the length of each dimension is zero.</span></span>

```vb
Dim e() As Integer = New Integer() { 1, 2, 3 }
Dim f(,) As Integer = New Integer(,) { { 1, 2, 3 } , { 4, 5, 6 } }

' Error: Inconsistent numbers of elements!
Dim g(,) As Integer = New Integer(,) { { 1, 2 }, { 4, 5, 6 } }

' Error: Inconsistent levels of nesting!
Dim h(,) As Integer = New Integer(,) { 1, 2, { 3, 4 } }
```

<span data-ttu-id="4e5da-585">コレクション初期化子の最も外側の入れ子レベルは、配列の一番左のディメンションに対応し、最も内側の入れ子レベルが最も右にあるディメンションに対応します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-585">The outermost nesting level of a collection initializer corresponds to the leftmost dimension of an array, and the innermost nesting level corresponds to the rightmost dimension.</span></span> <span data-ttu-id="4e5da-586">例:</span><span class="sxs-lookup"><span data-stu-id="4e5da-586">The example:</span></span>

```vb
Dim array As Integer(,) = _
    { { 0, 1 }, { 2, 3 }, { 4, 5 }, { 6, 7 }, { 8, 9 } }
```

<span data-ttu-id="4e5da-587">次のと同等です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-587">Is equivalent to the following:</span></span>

```vb
Dim array(4, 1) As Integer

array(0, 0) = 0: array(0, 1) = 1
array(1, 0) = 2: array(1, 1) = 3
array(2, 0) = 4: array(2, 1) = 5
array(3, 0) = 6: array(3, 1) = 7
array(4, 0) = 8: array(4, 1) = 9
```

<span data-ttu-id="4e5da-588">初期化される配列の次元が呼ばれるコレクション初期化子が空 (つまり、1 つを中かっこを含む) がない初期化子リストとの境界の場合は、空のコレクション初期化子は、指定したサイズの配列インスタンスを表します場所のすべての要素は、要素の型の既定値に初期化されています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-588">If the collection initializer is empty (that is, one that contains curly braces but no initializer list) and the bounds of the dimensions of the array being initialized are known, the empty collection initializer represents an array instance of the specified size where all the elements have been initialized to the element type's default value.</span></span> <span data-ttu-id="4e5da-589">初期化される配列の次元の境界が不明な場合、空のコレクション初期化子は、すべてのディメンションのサイズがゼロ配列インスタンスを表します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-589">If the bounds of the dimensions of the array being initialized are not known, the empty collection initializer represents an array instance in which all dimensions are size zero.</span></span>

<span data-ttu-id="4e5da-590">配列インスタンスのランクと各次元の長さは、インスタンスの有効期間にわたって定数。</span><span class="sxs-lookup"><span data-stu-id="4e5da-590">An array instance's rank and length of each dimension are constant for the entire lifetime of the instance.</span></span> <span data-ttu-id="4e5da-591">つまり、既存の配列インスタンスのランクを変更することはできませんも、そのディメンションのサイズを変更することはできます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-591">In other words, it is not possible to change the rank of an existing array instance, nor is it possible to resize its dimensions.</span></span>

#### <a name="array-literals"></a><span data-ttu-id="4e5da-592">配列リテラル</span><span class="sxs-lookup"><span data-stu-id="4e5da-592">Array Literals</span></span>

<span data-ttu-id="4e5da-593">配列リテラルでは、ある要素の型、ランク、および境界は、式のコンテキストとコレクション初期化子の組み合わせから推論される配列を表します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-593">An array literal denotes an array whose element type, rank, and bounds are inferred from a combination of the expression context and a collection initializer.</span></span> <span data-ttu-id="4e5da-594">これは、セクションで説明[式の再分類](expressions.md#expression-reclassification)します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-594">This is explained in Section [Expression Reclassification](expressions.md#expression-reclassification).</span></span>

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

<span data-ttu-id="4e5da-595">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-595">For example:</span></span>

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

<span data-ttu-id="4e5da-596">形式と、配列リテラル内のコレクション初期化子の要件はまったく同じと配列作成式で初期化子のコレクション。</span><span class="sxs-lookup"><span data-stu-id="4e5da-596">The format and requirements for the collection initializer in an array literal is exactly the same as that for the collection initializer in an array creation expression.</span></span>

<span data-ttu-id="4e5da-597">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-597">__Note.__</span></span> <span data-ttu-id="4e5da-598">自体は、配列リテラルが、配列を作成できません。代わりに、作成される配列を原因となる値には、式の再分類になります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-598">An array literal does not create the array in and of itself; instead, it is the reclassification of the expression into a value that causes the array to be created.</span></span> <span data-ttu-id="4e5da-599">変換では、`CType(new Integer() {1,2,3}, Short())`からの変換がないために、ことはできません`Integer()`に`Short()`; 式が、`CType({1,2,3},Short())`は、配列作成式に、配列リテラルを最初に再して分類ため可能です`New Short() {1,2,3}`.</span><span class="sxs-lookup"><span data-stu-id="4e5da-599">For instance, the conversion `CType(new Integer() {1,2,3}, Short())` is not possible because there is no conversion from `Integer()` to `Short()`; but the expression `CType({1,2,3},Short())` is possible because it first reclassifies the array literal into the array creation expression `New Short() {1,2,3}`.</span></span>


### <a name="delegate-creation-expressions"></a><span data-ttu-id="4e5da-600">デリゲート作成式</span><span class="sxs-lookup"><span data-stu-id="4e5da-600">Delegate-Creation Expressions</span></span>

<span data-ttu-id="4e5da-601">デリゲート作成式はデリゲート型の新しいインスタンスを作成するために使用します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-601">A delegate-creation expression is used to create a new instance of a delegate type.</span></span> <span data-ttu-id="4e5da-602">デリゲート作成式の引数は、メソッドのポインターまたはメソッドのラムダに分類される式である必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-602">The argument of a delegate-creation expression must be an expression classified as a method pointer or a lambda method.</span></span>

<span data-ttu-id="4e5da-603">引数がメソッドのポインターの場合、デリゲートのシグネチャに適用可能なメソッドのポインターによって参照されるメソッドのいずれかの必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-603">If the argument is a method pointer, one of the methods referenced by the method pointer must be applicable to the signature of the delegate type.</span></span> <span data-ttu-id="4e5da-604">メソッド`M`デリゲート型に適用できる`D`場合。</span><span class="sxs-lookup"><span data-stu-id="4e5da-604">A method `M` is applicable to a delegate type `D` if:</span></span>

* <span data-ttu-id="4e5da-605">`M` ない`Partial`本文が含まれるか。</span><span class="sxs-lookup"><span data-stu-id="4e5da-605">`M` is not `Partial` or has a body.</span></span>

* <span data-ttu-id="4e5da-606">両方`M`と`D`関数、または`D`サブルーチンです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-606">Both `M` and `D` are functions, or `D` is a subroutine.</span></span>

* <span data-ttu-id="4e5da-607">`M` `D`同じ数のパラメーターがあります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-607">`M` and `D` have the same number of parameters.</span></span>

* <span data-ttu-id="4e5da-608">パラメーターの型`M`の対応するパラメーターの型の型からの変換がある各`D`、およびその修飾子 (つまり`ByRef`、 `ByVal`) と一致します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-608">The parameter types of `M` each have a conversion from the type of the corresponding parameter type of `D`, and their modifiers (i.e. `ByRef`, `ByVal`) match.</span></span>

* <span data-ttu-id="4e5da-609">戻り値の型`M`の戻り値の型への変換がある存在する場合は、`D`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-609">The return type of `M`, if any, has a conversion to the return type of `D`.</span></span>

<span data-ttu-id="4e5da-610">メソッドのポインターは、遅延バインディング アクセスを参照する場合をデリゲート型と同じ数のパラメーターを持つ関数に遅延バインディング アクセスが使用されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-610">If the method pointer references a late-bound access, then the late-bound access is assumed to be to a function that has the same number of parameters as the delegate type.</span></span>

<span data-ttu-id="4e5da-611">厳密な型が使用されていないとは、メソッドのポインターによって参照される 1 つだけのメソッドがパラメーターを持たないとデリゲート型は、そのメソッドは、該当すると見なされますのファクトとパラメーターまたは戻り値により適用可能でない場合、re は単に無視されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-611">If strict semantics are not being used and there is only one method referenced by the method pointer, but it is not applicable due to the fact that it has no parameters and the delegate type does, then the method is considered applicable and the parameters or return value are simply ignored.</span></span> <span data-ttu-id="4e5da-612">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-612">For example:</span></span>

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

<span data-ttu-id="4e5da-613">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-613">__Note.__</span></span> <span data-ttu-id="4e5da-614">この緩和したトークンは厳密な型が拡張メソッドのために使用されていない場合にのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-614">This relaxation is only allowed when strict semantics are not being used because of extension methods.</span></span> <span data-ttu-id="4e5da-615">拡張メソッドは、通常のメソッドが適用できなかった場合にのみと見なされる、ために、インスタンス メソッドをデリゲート構築するためにパラメーターを持つ拡張メソッドを非表示にパラメーターのない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-615">Because extension methods are only considered if a regular method was not applicable, it is possible for an instance method with no parameters to hide an extension method with parameters for the purpose of delegate construction.</span></span>

<span data-ttu-id="4e5da-616">数より多い場合、メソッドのポインターによって参照される 1 つのメソッドは、デリゲート型に適用し、候補メソッド間での選択にオーバー ロードの解決を使用します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-616">If more than one method referenced by the method pointer is applicable to the delegate type, then overload resolution is used to pick between the candidate methods.</span></span> <span data-ttu-id="4e5da-617">デリゲートのパラメーターの型は、オーバー ロードの解決のための引数の型として使用されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-617">The types of the parameters to the delegate are used as the types of the arguments for the purposes of overload resolution.</span></span> <span data-ttu-id="4e5da-618">最適な候補の 1 つのメソッドがない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-618">If no one method candidate is most applicable, a compile-time error occurs.</span></span> <span data-ttu-id="4e5da-619">次の例では、ローカル変数は、2 番目に参照するデリゲートで初期化`Square`メソッドの複数のシグネチャと戻り値の型に適用可能なため、`DoubleFunc`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-619">In the following example, the local variable is initialized with a delegate that refers to the second `Square` method because that method is more applicable to the signature and return type of `DoubleFunc`.</span></span>

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

<span data-ttu-id="4e5da-620">2 番目の`Square`存在していないメソッドは、最初の`Square`メソッドが選択されています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-620">Had the second `Square` method not been present, the first `Square` method would have been chosen.</span></span> <span data-ttu-id="4e5da-621">コンパイル環境または厳密な型が指定されて場合`Option Strict`メソッドのポインターによって参照される最も固有のメソッドがデリゲート シグネチャより狭い幅の場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-621">If strict semantics are specified by the compilation environment or by `Option Strict`, then a compile-time error occurs if the most specific method referenced by the method pointer is narrower than the delegate signature.</span></span> <span data-ttu-id="4e5da-622">メソッド`M`と見なされますが、デリゲート型よりも幅の狭い`D`場合。</span><span class="sxs-lookup"><span data-stu-id="4e5da-622">A method `M` is considered narrower than a delegate type `D` if:</span></span>

* <span data-ttu-id="4e5da-623">パラメーターの型の`M`の対応するパラメーターの型への拡大変換が`D`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-623">A parameter type of `M` has a widening conversion to the corresponding parameter type of `D`.</span></span>

* <span data-ttu-id="4e5da-624">または、戻り値の型が存在する場合の`M`の戻り値の型への縮小変換が`D`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-624">Or, the return type, if any, of `M` has a narrowing conversion to the return type of `D`.</span></span>

<span data-ttu-id="4e5da-625">型引数がメソッドのポインターに関連付けられている場合は、同じ数の型引数を持つメソッドのみと見なされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-625">If type arguments are associated with the method pointer, only methods with the same number of type arguments are considered.</span></span> <span data-ttu-id="4e5da-626">型引数がメソッドのポインターに関連付けられていない場合は、型の推定は、ジェネリック メソッドの署名を照合するときに使用されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-626">If no type arguments are associated with the method pointer, type inference is used when matching signatures against a generic method.</span></span> <span data-ttu-id="4e5da-627">その他の通常の型推論とは異なり、デリゲートの戻り値の型が型引数を推論するときに使用されるがでも、戻り値の型は少なくともジェネリック オーバー ロードを決定する際に考慮されません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-627">Unlike other normal type inference, the return type of the delegate is used when inferring type arguments, but return types are still not considered when determining the least generic overload.</span></span> <span data-ttu-id="4e5da-628">次の例では、両方のデリゲート作成式に型引数を指定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-628">The following example shows both ways of supplying a type argument to a delegate-creation expression:</span></span>

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

<span data-ttu-id="4e5da-629">上記の例では、非ジェネリック デリゲート型がインスタンス化されたジェネリック メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-629">In the above example, a non-generic delegate type was instantiated using a generic method.</span></span> <span data-ttu-id="4e5da-630">ジェネリック メソッドを使用して構築されたデリゲート型のインスタンスを作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-630">It is also possible to create an instance of a constructed delegate type using a generic method.</span></span> <span data-ttu-id="4e5da-631">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-631">For example:</span></span>

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

<span data-ttu-id="4e5da-632">デリゲート作成式に引数がラムダ メソッドである場合は、ラムダ メソッドがデリゲート型のシグネチャに該当する場合があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-632">If the argument to the delegate-creation expression is a lambda method, the lambda method must be applicable to the signature of the delegate type.</span></span> <span data-ttu-id="4e5da-633">Lambda メソッド`L`デリゲート型に適用できる`D`場合。</span><span class="sxs-lookup"><span data-stu-id="4e5da-633">A lambda method `L` is applicable to a delegate type `D` if:</span></span>

* <span data-ttu-id="4e5da-634">場合`L`のパラメーターを持つ`D`が同じ数のパラメーター。</span><span class="sxs-lookup"><span data-stu-id="4e5da-634">If `L` has parameters, `D` has the same number of parameters.</span></span> <span data-ttu-id="4e5da-635">(場合`L`パラメーター、パラメーターを持たない`D`は無視されます)。</span><span class="sxs-lookup"><span data-stu-id="4e5da-635">(If `L` has no parameters, the parameters of `D` are ignored.)</span></span>

* <span data-ttu-id="4e5da-636">パラメーターの型`L`の対応するパラメーターの型の型への変換がある各`D`、およびその修飾子 (つまり`ByRef`、 `ByVal`) と一致します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-636">The parameter types of `L` each have a conversion to the type of the corresponding parameter type of `D`, and their modifiers (i.e. `ByRef`, `ByVal`) match.</span></span>

* <span data-ttu-id="4e5da-637">場合`D`関数の場合、戻り値の型は、`L`の戻り値の型への変換が`D`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-637">If `D` is a function, the return type of `L` has a conversion to the return type of `D`.</span></span> <span data-ttu-id="4e5da-638">(場合`D`の戻り値、サブルーチン`L`は無視されます)。</span><span class="sxs-lookup"><span data-stu-id="4e5da-638">(If `D` is a subroutine, the return value of `L` is ignored.)</span></span>

<span data-ttu-id="4e5da-639">パラメーターの型のパラメーターの場合`L`を省略するの対応するパラメーターの型`D`推論; 場合のパラメーターは、`L`配列または null 許容型名の修飾子が、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-639">If the parameter type of a parameter of `L` is omitted, then the type of the corresponding parameter in `D` is inferred; if the parameter of `L` has array or nullable name modifiers, a compile-time error results.</span></span> <span data-ttu-id="4e5da-640">すべてのパラメーターの型の 1 回`L`が使用可能なメソッドのラムダ式の型を推論します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-640">Once all of the parameter types of `L` are available, then the type of the expression in the lambda method is inferred.</span></span> <span data-ttu-id="4e5da-641">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-641">For example:</span></span>

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

<span data-ttu-id="4e5da-642">場合によっては、デリゲートのシグネチャが一致しないラムダ メソッドまたはメソッドのシグネチャ、.NET Framework がサポートしていませんデリゲートの作成ネイティブ。</span><span class="sxs-lookup"><span data-stu-id="4e5da-642">In some situations where delegate signature does not exactly match the lambda method or method signature, the .NET Framework may not support the delegate creation natively.</span></span> <span data-ttu-id="4e5da-643">そのような状況では、ラムダ メソッド式を使用して、2 つのメソッドを一致させる。</span><span class="sxs-lookup"><span data-stu-id="4e5da-643">In that situation, a lambda method expression is used to match the two methods.</span></span> <span data-ttu-id="4e5da-644">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-644">For example:</span></span>

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

<span data-ttu-id="4e5da-645">デリゲート作成式の結果は、メソッドのポインター式から (ある場合) に関連付けられたターゲット式と一致するメソッドを参照するデリゲート インスタンスです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-645">The result of a delegate-creation expression is a delegate instance that refers to the matching method with the associated target expression (if any) from the method pointer expression.</span></span> <span data-ttu-id="4e5da-646">値の型として、対象の式を入力すると、し、値の型がコピー システム ヒープにデリゲートが、ヒープ上のオブジェクトのメソッドをポイントできますのみためされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-646">If the target expression is typed as a value type, then the value type is copied onto the system heap because a delegate can only point to a method of an object on the heap.</span></span> <span data-ttu-id="4e5da-647">メソッドとデリゲートで参照するオブジェクトのまま、デリゲートの有効期間にわたって定数。</span><span class="sxs-lookup"><span data-stu-id="4e5da-647">The method and object to which a delegate refers remain constant for the entire lifetime of the delegate.</span></span> <span data-ttu-id="4e5da-648">つまり、作成した後、デリゲートのオブジェクトまたは複数のターゲットを変更することはできません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-648">In other words, it is not possible to change the target or object of a delegate after it has been created.</span></span>

### <a name="anonymous-object-creation-expressions"></a><span data-ttu-id="4e5da-649">匿名のオブジェクト作成式</span><span class="sxs-lookup"><span data-stu-id="4e5da-649">Anonymous Object-Creation Expressions</span></span>

<span data-ttu-id="4e5da-650">メンバー初期化子を含むオブジェクトの作成式を完全型名に省略できます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-650">An object-creation expression with member initializers can also omit the type name entirely.</span></span>

```antlr
AnonymousObjectCreationExpression
    : 'New' ObjectMemberInitializer
    ;
```

<span data-ttu-id="4e5da-651">その場合は、匿名型は、型と式の一部として初期化されるメンバーの名前に基づいて構築されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-651">In that case, an anonymous type is constructed based on the types and names of the members initialized as a part of the expression.</span></span> <span data-ttu-id="4e5da-652">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-652">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim Customer = New With { .Name = "John Smith", .Age = 34 }

        Console.WriteLine(Customer.Name)
    End Sub
End Module
```

<span data-ttu-id="4e5da-653">匿名のオブジェクト作成式によって作成された型は、名前を持たないから直接継承するクラス`Object`、メンバー初期化子リストに割り当てられているメンバーと同じ名前のプロパティのセットを持つとします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-653">The type created by an anonymous object-creation expression is a class that has no name, inherits directly from `Object`, and has a set of properties with the same name as the members assigned to in the member initializer list.</span></span> <span data-ttu-id="4e5da-654">ローカル変数の型の推論と同じ規則を使って、各プロパティの型が推論されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-654">The type of each property is inferred using the same rules as local variable type inference.</span></span> <span data-ttu-id="4e5da-655">生成された匿名型をオーバーライドも`ToString`、すべてのメンバーとその値の文字列表現を返します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-655">Generated anonymous types also override `ToString`, returning a string representation of all members and their values.</span></span> <span data-ttu-id="4e5da-656">(この文字列の正確な形式はこの仕様では扱いません) です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-656">(The exact format of this string is beyond the scope of this specification).</span></span>

<span data-ttu-id="4e5da-657">既定では、匿名の型によって生成されたプロパティは、読み取り/書き込みです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-657">By default, the properties generated by the anonymous type are read-write.</span></span> <span data-ttu-id="4e5da-658">使用して読み取り専用として匿名型のプロパティをマークすることは、`Key`修飾子。</span><span class="sxs-lookup"><span data-stu-id="4e5da-658">It is possible to mark an anonymous type property as read-only by using the `Key` modifier.</span></span> <span data-ttu-id="4e5da-659">`Key`修飾子は、匿名型を表す値を一意に識別するフィールドを使用できることを指定します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-659">The `Key` modifier specifies that the field can be used to uniquely identify the value the anonymous type represents.</span></span> <span data-ttu-id="4e5da-660">オーバーライドする匿名型が発生も読み取り専用プロパティを行う、だけでなく`Equals`と`GetHashCode`とインターフェイスを実装する`System.IEquatable(Of T)`(匿名型の入力`T`)。</span><span class="sxs-lookup"><span data-stu-id="4e5da-660">In addition to making the property read-only, it also causes the anonymous type to override `Equals` and  `GetHashCode` and to implement the interface `System.IEquatable(Of T)` (filling in the anonymous type for `T`).</span></span> <span data-ttu-id="4e5da-661">メンバーの定義は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-661">The members are defined as follows:</span></span>

<span data-ttu-id="4e5da-662">`Function Equals(obj As Object) As Boolean` `Function Equals(val As T) As Boolean` 、同じ型の 2 つのインスタンスの検証と各を比較することによって実装される`Key`を使用してメンバー`Object.Equals`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-662">`Function Equals(obj As Object) As Boolean` and `Function Equals(val As T) As Boolean` are implemented by validating that the two instances are of the same type and then comparing each `Key` member using `Object.Equals`.</span></span> <span data-ttu-id="4e5da-663">すべて`Key`メンバーが等しいか、`Equals`を返します`True`それ以外の場合、`Equals`返します`False`。</span><span class="sxs-lookup"><span data-stu-id="4e5da-663">If all `Key` members are equal, then `Equals` returns `True`, otherwise `Equals` returns `False`.</span></span>

<span data-ttu-id="4e5da-664">`Function GetHashCode() As Integer` 実装されているように場合に`Equals`しは、匿名型の 2 つのインスタンスは true、`GetHashCode`は同じ値を返します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-664">`Function GetHashCode() As Integer` is implemented such that that if `Equals` is true for two instances of the anonymous type, then `GetHashCode` will return the same value.</span></span> <span data-ttu-id="4e5da-665">ハッシュは開始シード値を持つとし、各`Key`メンバーは、順序が 31 をハッシュを乗算し、追加、`Key`メンバーのハッシュ値 (によって提供される`GetHashCode`) メンバーが参照型またはの値は、null許容値型ではない場合`Nothing`.</span><span class="sxs-lookup"><span data-stu-id="4e5da-665">The hash starts with a seed value and then, for each `Key` member, in order multiplies the hash by 31 and adds the `Key` member's hash value (provided by `GetHashCode`) if the member is not a reference type or nullable value type with the value of `Nothing`.</span></span>

<span data-ttu-id="4e5da-666">たとえば、ステートメントで作成した型:</span><span class="sxs-lookup"><span data-stu-id="4e5da-666">For example, the type created in the statement:</span></span>

```vb
Dim zipState = New With { Key .ZipCode = 98112, .State = "WA" }
```

<span data-ttu-id="4e5da-667">次のような約 (ただし、正確な実装が異なる場合があります) クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-667">creates a class that looks approximately like this (although exact implementation may vary):</span></span>

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

<span data-ttu-id="4e5da-668">匿名型が別の型のフィールドから作成されているような状況を簡素化するには、フィールド名は、次の場合、式から直接推論できます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-668">To simplify the situation where an anonymous type is created from the fields of another type, field names can be inferred directly from expressions in the following cases:</span></span>

* <span data-ttu-id="4e5da-669">簡易名式`x`名前を推測`x`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-669">A simple name expression `x` infers the name `x`.</span></span>

* <span data-ttu-id="4e5da-670">メンバー アクセス式`x.y`名前を推測`y`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-670">A member access expression `x.y` infers the name `y`.</span></span>

* <span data-ttu-id="4e5da-671">ディクショナリ lookup 式`x!y`名前を推測`y`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-671">A dictionary lookup expression `x!y` infers the name `y`.</span></span>

* <span data-ttu-id="4e5da-672">引数なしでの呼び出しまたはインデックス式`x()`名前を推測`x`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-672">An invocation or index expression with no arguments `x()` infers the name `x`.</span></span>

* <span data-ttu-id="4e5da-673">XML メンバー アクセス式`x.<y>`、 `x...<y>`、`x.@y`名前を推測`y`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-673">An XML member access expression `x.<y>`, `x...<y>`, `x.@y` infers the name `y`.</span></span>

* <span data-ttu-id="4e5da-674">XML メンバー アクセス式、メンバー アクセス式の対象となっている`x.<y>.z`名前を推測`z`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-674">An XML member access expression that is the target of a member access expression `x.<y>.z` infers the name `z`.</span></span>

* <span data-ttu-id="4e5da-675">引数なしでの呼び出しまたはインデックス式のターゲットである XML メンバー アクセス式`x.<y>.z()`名前を推測`z`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-675">An XML member access expression that is the target of an invocation or index expression with no arguments `x.<y>.z()` infers the name `z`.</span></span>

* <span data-ttu-id="4e5da-676">XML メンバー アクセス式を呼び出し、またはインデックス式の対象となっている`x.<y>(0)`名前を推測`y`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-676">An XML member access expression that is the target of an invocation or index expression `x.<y>(0)` infers the name `y`.</span></span>

<span data-ttu-id="4e5da-677">初期化子は、推論された名前の式の割り当てとして解釈されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-677">The initializer is interpreted as an assignment of the expression to the inferred name.</span></span> <span data-ttu-id="4e5da-678">たとえば、次の初期化子は同等です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-678">For example, the following initializers are equivalent:</span></span>

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

<span data-ttu-id="4e5da-679">など、型の既存のメンバーと競合するメンバーの名前が推論される場合`GetHashCode`コンパイル時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-679">If a member name is inferred that conflicts with an existing member of the type, such as `GetHashCode`, then a compile time error occurs.</span></span> <span data-ttu-id="4e5da-680">通常のメンバー初期化子とは異なり匿名オブジェクト作成式を許可しないメンバー初期化子の循環参照、または初期化前に、メンバーを参照してください。</span><span class="sxs-lookup"><span data-stu-id="4e5da-680">Unlike regular member initializers, anonymous object-creation expressions do not allow member initializers to have circular references, or to refer to a member before it has been initialized.</span></span> <span data-ttu-id="4e5da-681">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-681">For example:</span></span>

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

<span data-ttu-id="4e5da-682">匿名クラスの作成の 2 つの式は、同じメソッド内で発生し、プロパティの順序、プロパティ名、およびプロパティの型すべて一致 - 場合は、同一の結果として得られる図形--が生成する場合は両方を参照している同じ匿名クラス。</span><span class="sxs-lookup"><span data-stu-id="4e5da-682">If two anonymous class creation expressions occur within the same method and yield the same resulting shape -- if the property order, property names, and property types all match -- they will both refer to the same anonymous class.</span></span> <span data-ttu-id="4e5da-683">インスタンスまたは共有メンバー変数は初期化子でのメソッドのスコープは、変数を初期化するコンス トラクターです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-683">The method scope of an instance or shared member variable with an initializer is the constructor in which the variable is initialized.</span></span>

<span data-ttu-id="4e5da-684">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-684">__Note.__</span></span> <span data-ttu-id="4e5da-685">コンパイラは匿名を統一できます型さらに、このようなアセンブリ レベルでは、この時点で依存このことはできません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-685">It is possible that a compiler may choose to unify anonymous types further, such as at the assembly level, but this cannot be relied upon at this time.</span></span>


## <a name="cast-expressions"></a><span data-ttu-id="4e5da-686">キャスト式</span><span class="sxs-lookup"><span data-stu-id="4e5da-686">Cast Expressions</span></span>

<span data-ttu-id="4e5da-687">キャスト式は、式を指定した型を変換します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-687">A cast expression coerces an expression to a given type.</span></span> <span data-ttu-id="4e5da-688">特定のキャストのキーワードは、プリミティブ型に式を強制します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-688">Specific cast keywords coerce expressions into the primitive types.</span></span> <span data-ttu-id="4e5da-689">次の 3 つの一般的な cast キーワード`CType`、`TryCast`と`DirectCast`型に式を強制します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-689">Three general cast keywords, `CType`, `TryCast` and `DirectCast`, coerce an expression into a type.</span></span>

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

<span data-ttu-id="4e5da-690">`DirectCast` `TryCast`特殊な動作があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-690">`DirectCast` and `TryCast` have special behaviors.</span></span> <span data-ttu-id="4e5da-691">このため、ネイティブ変換のみサポートします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-691">Because of this, they only support native conversions.</span></span> <span data-ttu-id="4e5da-692">さらに、ターゲット型を`TryCast`式が値型にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-692">Additionally, the target type in a `TryCast` expression cannot be a value type.</span></span> <span data-ttu-id="4e5da-693">ユーザー定義変換演算子ときに考慮されません`DirectCast`または`TryCast`使用されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-693">User-defined conversion operators are not considered when `DirectCast` or `TryCast` is used.</span></span> <span data-ttu-id="4e5da-694">(__に注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-694">(__Note.__</span></span> <span data-ttu-id="4e5da-695">変換を設定する`DirectCast`と`TryCast`"ネイティブ CLR"変換を実装するためのサポートが制限されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-695">The conversion set that `DirectCast` and `TryCast` support are restricted because they implement "native CLR" conversions.</span></span> <span data-ttu-id="4e5da-696">目的は、`DirectCast`の目的は、中に、「ボックス化解除」命令の機能を提供する、 `TryCast` "isinst"命令の機能を提供することです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-696">The purpose of `DirectCast` is to provide the functionality of the "unbox" instruction, while the purpose of `TryCast` is to provide the functionality of the "isinst" instruction.</span></span> <span data-ttu-id="4e5da-697">手順については CLR 上に、マップ、ため、CLR によって直接サポートされている変換をサポートしているが損なわれる目的。)</span><span class="sxs-lookup"><span data-stu-id="4e5da-697">Since they map onto CLR instructions, supporting conversions not directly supported by the CLR would defeat the intended purpose.)</span></span>

<span data-ttu-id="4e5da-698">`DirectCast` として型指定される式に変換します`Object`とは異なる`CType`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-698">`DirectCast` converts expressions that are typed as `Object` differently than `CType`.</span></span> <span data-ttu-id="4e5da-699">型の式を変換するときに`Object`が実行時の型がプリミティブ値型では、`DirectCast`がスローされます、`System.InvalidCastException`例外指定の型でない式の実行時の型と同じ場合、または`System.NullReferenceException`場合、式評価される`Nothing`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-699">When converting an expression of type `Object` whose run-time type is a primitive value type, `DirectCast` throws a `System.InvalidCastException` exception if the specified type is not the same as the run-time type of the expression or a `System.NullReferenceException` if the expression evaluates to `Nothing`.</span></span> <span data-ttu-id="4e5da-700">(__に注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-700">(__Note.__</span></span> <span data-ttu-id="4e5da-701">前述のよう`DirectCast`CLR 命令に直接マップ「ボックス化解除」式の型が`Object`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-701">As noted above, `DirectCast` maps directly onto the CLR instruction "unbox" when the type of the expression is `Object`.</span></span> <span data-ttu-id="4e5da-702">これに対し、`CType`プリミティブ型間の変換をサポートできるように、変換するランタイム ヘルパーへの呼び出しに変換します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-702">In contrast, `CType` turns into a call to a runtime helper to do the conversion so that conversions between primitive types can be supported.</span></span> <span data-ttu-id="4e5da-703">場合と、`Object`式に変換されるプリミティブ値型と実際のインスタンスの一致の種類がターゲット型`DirectCast`よりも大幅に時間が短くなります`CType`)。</span><span class="sxs-lookup"><span data-stu-id="4e5da-703">In the case when an `Object` expression is being converted to a primitive value type and the type of the actual instance match the target type, `DirectCast` will be significantly faster than `CType`.)</span></span>

<span data-ttu-id="4e5da-704">`TryCast` 式に変換しますが、式は、対象の型に変換できない場合、例外はスローされません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-704">`TryCast` converts expressions but does not throw an exception if the expression cannot be converted to the target type.</span></span> <span data-ttu-id="4e5da-705">代わりに、`TryCast`なります`Nothing`式は、実行時に変換できない場合。</span><span class="sxs-lookup"><span data-stu-id="4e5da-705">Instead, `TryCast` will result in `Nothing` if the expression cannot be converted at runtime.</span></span> <span data-ttu-id="4e5da-706">(__に注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-706">(__Note.__</span></span> <span data-ttu-id="4e5da-707">前述のよう`TryCast`"isinst"CLR 命令に直接マップされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-707">As noted above, `TryCast` maps directly onto the CLR instruction "isinst".</span></span> <span data-ttu-id="4e5da-708">型チェックと 1 つの操作に変換を組み合わせることで`TryCast`これよりも低コストにことができます、`TypeOf ... Is`をクリックし、 `CType`)。</span><span class="sxs-lookup"><span data-stu-id="4e5da-708">By combining the type check and the conversion into a single operation, `TryCast` can be cheaper than doing a `TypeOf ... Is` and then a `CType`.)</span></span>

<span data-ttu-id="4e5da-709">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-709">For example:</span></span>

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

<span data-ttu-id="4e5da-710">指定した型に式の型から変換が存在しない、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-710">If no conversion exists from the type of the expression to the specified type, a compile-time error occurs.</span></span> <span data-ttu-id="4e5da-711">それ以外の場合、式は値として分類され、変換によって生成された値になります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-711">Otherwise, the expression is classified as a value and the result is the value produced by the conversion.</span></span>


## <a name="operator-expressions"></a><span data-ttu-id="4e5da-712">演算子式</span><span class="sxs-lookup"><span data-stu-id="4e5da-712">Operator Expressions</span></span>

<span data-ttu-id="4e5da-713">演算子の 2 種類があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-713">There are two kinds of operators.</span></span> <span data-ttu-id="4e5da-714">*単項演算子*1 つのオペランドを使用して、プレフィックス表記を使用して (たとえば、 `-x`)。</span><span class="sxs-lookup"><span data-stu-id="4e5da-714">*Unary operators* take one operand and use prefix notation (for example, `-x`).</span></span> <span data-ttu-id="4e5da-715">*二項演算子*2 つのオペランドを取るし、挿入辞表記を使用して (たとえば、 `x + y`)。</span><span class="sxs-lookup"><span data-stu-id="4e5da-715">*Binary operators* take two operands and use infix notation (for example, `x + y`).</span></span> <span data-ttu-id="4e5da-716">関係演算子を除く結果が常に`Boolean`、その型の結果、特定の種類に対して定義された演算子。</span><span class="sxs-lookup"><span data-stu-id="4e5da-716">With the exception of the relational operators, which always result in `Boolean`, an operator defined for a particular type results in that type.</span></span> <span data-ttu-id="4e5da-717">演算子のオペランドは値として常に分類する必要があります。演算子の式の結果は、値として分類されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-717">The operands to an operator must always be classified as a value; the result of an operator expression is classified as a value.</span></span>

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

### <a name="operator-precedence-and-associativity"></a><span data-ttu-id="4e5da-718">演算子の優先順位と結合規則</span><span class="sxs-lookup"><span data-stu-id="4e5da-718">Operator Precedence and Associativity</span></span>

<span data-ttu-id="4e5da-719">式には、複数のバイナリ演算子が含まれている場合、*優先順位*演算子の個々 のバイナリ演算子が評価される順序を制御します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-719">When an expression contains multiple binary operators, the *precedence* of the operators controls the order in which the individual binary operators are evaluated.</span></span> <span data-ttu-id="4e5da-720">たとえば、式 `x + y * z` の評価は `x + (y * z)` ですが、これは `*` 演算子が `+` 演算子より高い優先順位だからです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-720">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the `+` operator.</span></span> <span data-ttu-id="4e5da-721">次の表では、優先順位の降順で二項演算子を示します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-721">The following table lists the binary operators in descending order of precedence:</span></span>


| <span data-ttu-id="4e5da-722">__カテゴリ__</span><span class="sxs-lookup"><span data-stu-id="4e5da-722">__Category__</span></span>     | <span data-ttu-id="4e5da-723">__演算子__</span><span class="sxs-lookup"><span data-stu-id="4e5da-723">__Operators__</span></span>                                          | 
|------------------|--------------------------------------------------------|
| <span data-ttu-id="4e5da-724">1 次式</span><span class="sxs-lookup"><span data-stu-id="4e5da-724">Primary</span></span>          | <span data-ttu-id="4e5da-725">すべての非演算子式</span><span class="sxs-lookup"><span data-stu-id="4e5da-725">All non-operator expressions</span></span>                           |
| <span data-ttu-id="4e5da-726">Await </span><span class="sxs-lookup"><span data-stu-id="4e5da-726">Await</span></span>            | `Await`                                                |
| <span data-ttu-id="4e5da-727">指数演算</span><span class="sxs-lookup"><span data-stu-id="4e5da-727">Exponentiation</span></span>   | `^`                                                    |
| <span data-ttu-id="4e5da-728">単項マイナス符号</span><span class="sxs-lookup"><span data-stu-id="4e5da-728">Unary negation</span></span>   | <span data-ttu-id="4e5da-729">`+`, `-`</span><span class="sxs-lookup"><span data-stu-id="4e5da-729">`+`, `-`</span></span>                                               |
| <span data-ttu-id="4e5da-730">乗法</span><span class="sxs-lookup"><span data-stu-id="4e5da-730">Multiplicative</span></span>   | <span data-ttu-id="4e5da-731">`*`, `/`</span><span class="sxs-lookup"><span data-stu-id="4e5da-731">`*`, `/`</span></span>                                               |
| <span data-ttu-id="4e5da-732">整数除算</span><span class="sxs-lookup"><span data-stu-id="4e5da-732">Integer division</span></span> | `\`                                                    |
| <span data-ttu-id="4e5da-733">剰余</span><span class="sxs-lookup"><span data-stu-id="4e5da-733">Modulus</span></span>          | `Mod`                                                  |
| <span data-ttu-id="4e5da-734">加法</span><span class="sxs-lookup"><span data-stu-id="4e5da-734">Additive</span></span>         | <span data-ttu-id="4e5da-735">`+`, `-`</span><span class="sxs-lookup"><span data-stu-id="4e5da-735">`+`, `-`</span></span>                                               |
| <span data-ttu-id="4e5da-736">連結</span><span class="sxs-lookup"><span data-stu-id="4e5da-736">Concatenation</span></span>    | `&`                                                    |
| <span data-ttu-id="4e5da-737">シフト</span><span class="sxs-lookup"><span data-stu-id="4e5da-737">Shift</span></span>            | <span data-ttu-id="4e5da-738">`<<`, `>>`</span><span class="sxs-lookup"><span data-stu-id="4e5da-738">`<<`, `>>`</span></span>                                             |
| <span data-ttu-id="4e5da-739">関係</span><span class="sxs-lookup"><span data-stu-id="4e5da-739">Relational</span></span>       | <span data-ttu-id="4e5da-740">`=`, `<>`, `<`, `>`, `<=`, `>=`, `Like`, `Is`, `IsNot`</span><span class="sxs-lookup"><span data-stu-id="4e5da-740">`=`, `<>`, `<`, `>`, `<=`, `>=`, `Like`, `Is`, `IsNot`</span></span> |
| <span data-ttu-id="4e5da-741">論理 NOT</span><span class="sxs-lookup"><span data-stu-id="4e5da-741">Logical NOT</span></span>      | `Not`                                                  |
| <span data-ttu-id="4e5da-742">論理 AND</span><span class="sxs-lookup"><span data-stu-id="4e5da-742">Logical AND</span></span>      | <span data-ttu-id="4e5da-743">`And`, `AndAlso`</span><span class="sxs-lookup"><span data-stu-id="4e5da-743">`And`, `AndAlso`</span></span>                                       |
| <span data-ttu-id="4e5da-744">論理 OR</span><span class="sxs-lookup"><span data-stu-id="4e5da-744">Logical OR</span></span>       | <span data-ttu-id="4e5da-745">`Or`, `OrElse`</span><span class="sxs-lookup"><span data-stu-id="4e5da-745">`Or`, `OrElse`</span></span>                                         |
| <span data-ttu-id="4e5da-746">論理 XOR</span><span class="sxs-lookup"><span data-stu-id="4e5da-746">Logical XOR</span></span>      | `Xor`                                                  |

<span data-ttu-id="4e5da-747">式では、同じ優先順位が 2 つの演算子が含まれている場合、*結合規則*演算子の操作が実行される順序を制御します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-747">When an expression contains two operators with the same precedence, the *associativity* of the operators controls the order in which the operations are performed.</span></span> <span data-ttu-id="4e5da-748">すべてのバイナリ演算子は、左から右に操作を実行する意味左結合です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-748">All binary operators are left-associative, meaning that operations are performed from left to right.</span></span> <span data-ttu-id="4e5da-749">優先順位と結合規則は、かっこで囲まれた式を使用して制御できます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-749">Precedence and associativity can be controlled using parenthetical expressions.</span></span>

### <a name="object-operands"></a><span data-ttu-id="4e5da-750">オブジェクトのオペランド</span><span class="sxs-lookup"><span data-stu-id="4e5da-750">Object Operands</span></span>

<span data-ttu-id="4e5da-751">すべての演算子が型のオペランドをサポートする各演算子でサポートされている通常の型だけでなく`Object`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-751">In addition to the regular types supported by each operator, all operators support operands of type `Object`.</span></span> <span data-ttu-id="4e5da-752">適用される演算子`Object`オペランドは、上のメソッド呼び出しと同様に処理が`Object`値: である場合、コンパイル時の型ではなく、オペランドの実行時の型、有効性を判断、遅延バインディング メソッドの呼び出しを選択する場合があります操作の種類です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-752">Operators applied to `Object` operands are handled similarly to method calls made on `Object` values: a late-bound method call might be chosen, in which case the run-time type of the operands, rather than the compile-time type, determines the validity and type of the operation.</span></span> <span data-ttu-id="4e5da-753">コンパイル環境または厳密な型が指定されて場合`Option Strict`、型のオペランドの任意のオペレーター`Object`を除き、コンパイル時エラーが発生する、 `TypeOf...Is`、`Is`と`IsNot`演算子。</span><span class="sxs-lookup"><span data-stu-id="4e5da-753">If strict semantics are specified by the compilation environment or by `Option Strict`, any operators with operands of type `Object` cause a compile-time error, except for the `TypeOf...Is`, `Is` and `IsNot` operators.</span></span>

<span data-ttu-id="4e5da-754">演算子の解決操作に対して、遅延バインディングを実行する必要があると判断した場合、操作の結果はオペランドの実行時の型が演算子でサポートされている型の場合は、オペランドの型に演算子を適用した結果を示します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-754">When operator resolution determines that an operation should be performed late-bound, the outcome of the operation is the result of applying the operator to the operand types if the run-time types of the operands are types that are supported by the operator.</span></span> <span data-ttu-id="4e5da-755">値`Nothing`は二項演算式でもう一方のオペランドの型の既定値として扱われます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-755">The value `Nothing` is treated as the default value of the type of the other operand in a binary operator expression.</span></span> <span data-ttu-id="4e5da-756">単項演算子式の場合、または両方のオペランドが`Nothing`二項演算子の式では、操作の種類が`Integer`または演算子、演算子にならない場合の唯一の結果型`Integer`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-756">In a unary operator expression, or if both operands are `Nothing` in a binary operator expression, the type of the operation is `Integer` or the only result type of the operator, if the operator does not result in `Integer`.</span></span> <span data-ttu-id="4e5da-757">操作の結果が常に、キャストに戻す`Object`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-757">The result of the operation is always then cast back to `Object`.</span></span> <span data-ttu-id="4e5da-758">オペランドの型で有効な演算子がない場合、`System.InvalidCastException`例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-758">If the operand types have no valid operator, a `System.InvalidCastException` exception is thrown.</span></span> <span data-ttu-id="4e5da-759">実行時に変換が暗黙的または明示的なされるかどうかに関係なく行われます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-759">Conversions at run time are done without regard to whether they are implicit or explicit.</span></span>

<span data-ttu-id="4e5da-760">数値の二項演算の結果が生成されます (かどうかの整数オーバーフローのチェックが有効か無効) に関係なく、オーバーフロー例外、し、結果の型が昇格 [次へ] の幅の広い数値型に可能な場合。</span><span class="sxs-lookup"><span data-stu-id="4e5da-760">If the result of a numeric binary operation would produce an overflow exception (regardless of whether integer overflow checking is on or off), then the result type is promoted to the next wider numeric type, if possible.</span></span> <span data-ttu-id="4e5da-761">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-761">For example, consider the following code:</span></span>

```vb
Module Test
    Sub Main()
        Dim o As Object = CObj(CByte(2)) * CObj(CByte(255))

        Console.WriteLine(o.GetType().ToString() & " = " & o)
    End Sub
End Module
```

<span data-ttu-id="4e5da-762">次の結果が出力されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-762">It prints the following result:</span></span>

```
System.Int16 = 512
```

<span data-ttu-id="4e5da-763">幅の広い数値型が、数を保持するために使用できない場合、`System.OverflowException`例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-763">If no wider numeric type is available to hold the number, a `System.OverflowException` exception is thrown.</span></span>

### <a name="operator-resolution"></a><span data-ttu-id="4e5da-764">演算子の解決</span><span class="sxs-lookup"><span data-stu-id="4e5da-764">Operator Resolution</span></span>

<span data-ttu-id="4e5da-765">演算子の種類とオペランドのセットを指定するには、演算子の解決のオペランドを使用する演算子を決定します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-765">Given an operator type and a set of operands, operator resolution determines which operator to use for the operands.</span></span> <span data-ttu-id="4e5da-766">演算子を解決するときに、ユーザー定義演算子には、最初に、次の手順を使用して、考えられます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-766">When resolving operators, user-defined operators will be considered first, using the following steps:</span></span>

1. <span data-ttu-id="4e5da-767">最初に、すべての候補演算子収集されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-767">First, all of the candidate operators are collected.</span></span> <span data-ttu-id="4e5da-768">演算子の候補には、すべてのソースの種類で特定の演算子の種類のユーザー定義演算子、およびすべてのターゲットの型では、特定の種類のユーザー定義演算子です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-768">The candidate operators are all of the user-defined operators of the particular operator type in the source type and all of the user-defined operators of the particular type in the target type.</span></span> <span data-ttu-id="4e5da-769">ソースの種類と変換先の型は関連して、一般的な演算子は 1 回考慮のみ。</span><span class="sxs-lookup"><span data-stu-id="4e5da-769">If the source type and destination type are related, common operators are only considered once.</span></span>

2. <span data-ttu-id="4e5da-770">次に、オーバー ロードの解決は、最も固有の演算子を選択するには、演算子とオペランドに適用されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-770">Then, overload resolution is applied to the operators and operands to select the most specific operator.</span></span> <span data-ttu-id="4e5da-771">二項演算子は、の場合、遅延バインディング呼び出しにあります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-771">In the case of binary operators, this may result in a late-bound call.</span></span>

<span data-ttu-id="4e5da-772">型の演算子の候補を収集するときに`T?`、型の演算子`T`代わりに使用されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-772">When collecting the candidate operators for a type `T?`, the operators of type `T` are used instead.</span></span> <span data-ttu-id="4e5da-773">いずれかの`T`のユーザー定義された null 非許容値型のみに関連する演算子が無効になることもできます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-773">Any of `T`'s user-defined operators that involve only non-nullable value types are also lifted.</span></span> <span data-ttu-id="4e5da-774">リフトされた演算子を使用して、値の型の null 許容バージョン、例外での戻り値の型`IsTrue`と`IsFalse`(する必要があります`Boolean`)。</span><span class="sxs-lookup"><span data-stu-id="4e5da-774">A lifted operator uses the nullable version of any value types, with the exception the return types of `IsTrue` and `IsFalse` (which must be `Boolean`).</span></span> <span data-ttu-id="4e5da-775">リフトされた演算子がオペランドを null 非許容のバージョンに変換すると、評価し、ユーザー定義演算子を評価し、変換した結果が、null 許容バージョンを入力します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-775">Lifted operators are evaluated by converting the operands to their non-nullable version, then evaluating the user-defined operator and then converting the result type to its nullable version.</span></span> <span data-ttu-id="4e5da-776">イーサ オペランドが場合`Nothing`、式の結果の値は、`Nothing`結果型の null 許容バージョンとして型指定します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-776">If ether operand is `Nothing`, the result of the expression is a value of `Nothing` typed as the nullable version of the result type.</span></span> <span data-ttu-id="4e5da-777">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-777">For example:</span></span>

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

<span data-ttu-id="4e5da-778">演算子が二項演算子とオペランドの 1 つは、参照型、演算子が無効になることもが演算子の任意のバインディングには、エラーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-778">If the operator is a binary operator and one of the operands is reference type, the operator is also lifted, but any binding to the operator produces an error.</span></span> <span data-ttu-id="4e5da-779">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-779">For example:</span></span>

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

<span data-ttu-id="4e5da-780">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-780">__Note.__</span></span> <span data-ttu-id="4e5da-781">このルールは、ケースの場合は 2 種類の二項演算子の動作が変更は、将来のバージョンで参照型の null 値反映を追加するかどうかに考慮事項が発生したために存在します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-781">This rule exists because there has been consideration whether we wish to add null-propagating reference types in a future version, in which case the behavior in the case of binary operators between the two types would change.</span></span>

<span data-ttu-id="4e5da-782">変換とユーザー定義演算子は常に優先リフトされた演算子よりです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-782">As with conversions, user-defined operators are always preferred over lifted operators.</span></span>

<span data-ttu-id="4e5da-783">演算子はオーバー ロードを解決するときに Visual Basic で定義されているクラスとその他の言語で定義されている間の違いである可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-783">When resolving overloaded operators, there may be differences between classes defined in Visual Basic and those defined in other languages:</span></span>

* <span data-ttu-id="4e5da-784">他の言語で`Not`、 `And`、および`Or`の両方として、論理演算子とビットごとの演算子はオーバー ロードできます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-784">In other languages, `Not`, `And`, and `Or` may be overloaded both as logical operators and bitwise operators.</span></span> <span data-ttu-id="4e5da-785">外部アセンブリから、インポート時にいずれかの形式は、これらの演算子の有効なオーバー ロードとして承認されました。</span><span class="sxs-lookup"><span data-stu-id="4e5da-785">Upon import from an external assembly, either form is accepted as a valid overload for these operators.</span></span> <span data-ttu-id="4e5da-786">ただし、論理とビットごとの演算子を定義する型の場合、ビットごとの実装のみが考慮されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-786">However, for a type which defines both logical and bitwise operators, only the bitwise implementation will be considered.</span></span>

* <span data-ttu-id="4e5da-787">他の言語で`>>`と`<<`の両方として、演算子の符号付きと符号なしの演算子はオーバー ロードできます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-787">In other languages, `>>` and `<<` may be overloaded both as signed operators and unsigned operators.</span></span> <span data-ttu-id="4e5da-788">外部アセンブリから、インポート時に、いずれかの形式は有効なオーバー ロードとして受け入れられます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-788">Upon import from an external assembly, either form is accepted as a valid overload.</span></span> <span data-ttu-id="4e5da-789">ただし、符号付きと符号なしの両方の演算子を定義する型の場合、署名付きの実装のみが考慮されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-789">However, for a type which defines both signed and unsigned operators, only the signed implementation will be considered.</span></span>

* <span data-ttu-id="4e5da-790">ユーザー定義の演算子がオペランドに最も固有でない場合、組み込みの演算子を考慮します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-790">If no user-defined operator is most specific to the operands, then intrinsic operators will be considered.</span></span> <span data-ttu-id="4e5da-791">オペランドの組み込みの演算子が定義されていないと、いずれかのオペランドが Object 型の場合、オペレーターは解決されます遅延バインディング;それ以外の場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-791">If no intrinsic operator is defined for the operands and either operand has type Object then the operator will be resolved late-bound; otherwise,  a compile-time error results.</span></span>

<span data-ttu-id="4e5da-792">以前のバージョンの Visual Basic では、オブジェクト、型の 1 つのオペランドと該当するユーザー定義オペレーター、およびいない適用可能な組み込み演算子があった場合、エラーがあった。</span><span class="sxs-lookup"><span data-stu-id="4e5da-792">In prior versions of Visual Basic, if there was exactly one operand of type Object, and no applicable user-defined operators, and no applicable intrinsic operators, then it was an error.</span></span> <span data-ttu-id="4e5da-793">Visual Basic の 11 の時点では、遅延バインディングを解決にされています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-793">As of Visual Basic 11, it is now resolved late-bound.</span></span> <span data-ttu-id="4e5da-794">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-794">For example:</span></span>

```vb
Module Module1
  Sub Main()
      Dim p As Object = Nothing
      Dim U As New Uri("http://www.microsoft.com")
      Dim j = U * p  ' is now resolved late-bound
   End Sub
End Module
```

<span data-ttu-id="4e5da-795">型`T`を持つ組み込み演算子は、その同じ演算子も定義されています。`T?`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-795">A type `T` that has an intrinsic operator also defines that same operator for `T?`.</span></span> <span data-ttu-id="4e5da-796">演算子の結果`T?`場合と同じになります`T`点を除き、いずれかのオペランドが場合`Nothing`、演算子の結果になります`Nothing`(つまり、null 値が反映されます)。</span><span class="sxs-lookup"><span data-stu-id="4e5da-796">The result of the operator on `T?` will be the same as for `T`, except that if either operand is `Nothing`, the result of the operator will be `Nothing` (i.e. the null value is propagated).</span></span> <span data-ttu-id="4e5da-797">操作の型の解決のため、`?`が削除、設定されているオペランドから、操作の種類が決定されます、および`?`null 許容値型のいずれかのオペランドの場合、操作の種類に追加されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-797">For the purposes of resolving the type of an operation, the `?` is removed from any operands that have them, the type of the operation is determined, and a `?` is added to the type of the operation if any of the operands were nullable value types.</span></span> <span data-ttu-id="4e5da-798">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-798">For example:</span></span>

```vb
Dim v1? As Integer = 10
Dim v2 As Long = 20

' Type of operation will be Long?
Console.WriteLine(v1 + v2)
```

<span data-ttu-id="4e5da-799">各演算子には、組み込みの型に定義されていると実行される操作のオペランドの型を指定された型が一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-799">Each operator lists the intrinsic types it is defined for and the type of the operation performed given the operand types.</span></span> <span data-ttu-id="4e5da-800">組み込みの操作の種類の結果では、これらの一般的な規則に従います。</span><span class="sxs-lookup"><span data-stu-id="4e5da-800">The result of type of a intrinsic operation follows these general rules:</span></span>

* <span data-ttu-id="4e5da-801">すべてのオペランドが同じ型の型の演算子が定義されている場合は、変換は行われませんし、その型の演算子を使用します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-801">If all operands are of the same type, and the operator is defined for the type, then no conversion occurs and the operator for that type is used.</span></span>

* <span data-ttu-id="4e5da-802">演算子の型が定義されていないいずれかのオペランドは、次の手順を使用して変換されますされ、演算子は、新しい型に対して解決されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-802">Any operand whose type is not defined for the operator is converted using the following steps and the operator is resolved against the new types:</span></span>

  * <span data-ttu-id="4e5da-803">オペランドは、演算子とオペランドの両方で定義されている次の最も幅の広い型で暗黙的に変換に変換されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-803">The operand is converted to the next widest type that is defined for both the operator and the operand and to which it is implicitly convertible.</span></span>

  * <span data-ttu-id="4e5da-804">このような型がない場合は、演算子とオペランドの両方で定義されている次の最も狭い型で暗黙的に変換し、オペランドは変換されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-804">If there is no such type, then the operand is converted to the next narrowest type that is defined for both the operator and the operand and to which it is implicitly convertible.</span></span>

  * <span data-ttu-id="4e5da-805">このような型がないか、変換が発生することはできません、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-805">If there is no such type or the conversion cannot occur, a compile-time error occurs.</span></span>

* <span data-ttu-id="4e5da-806">オペランドを変換する場合は、オペランドの型とその型の演算子の幅が使用されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-806">Otherwise, the operands are converted to the wider of the operand types and the operator for that type is used.</span></span> <span data-ttu-id="4e5da-807">多くの演算子の種類より狭いオペランドの型を暗黙的に変換できません、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-807">If the narrower operand type cannot be implicitly converted to the wider operator type, a compile-time error occurs.</span></span>

<span data-ttu-id="4e5da-808">次の一般的な規則に関係なくただしがいくつかの特殊なケースが演算子の結果テーブルで呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-808">Despite these general rules, however, there are a number of special cases called out in the operator results tables.</span></span>

<span data-ttu-id="4e5da-809">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-809">__Note.__</span></span> <span data-ttu-id="4e5da-810">演算子の種類のテーブル上の理由から、書式設定する定義済みの名前を最初の 2 つの文字の省略形します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-810">For formatting reasons, the operator type tables abbreviate the predefined names to their first two characters.</span></span> <span data-ttu-id="4e5da-811">「を」 `Byte`、"UI"は`UInteger`、"st"`String`など。「エラー」指定されたオペランドの型に対して定義されている操作がないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-811">So "By" is `Byte`, "UI" is `UInteger`, "St" is `String`, etc. "Err" means that there is no operation defined for the given operand types.</span></span>

## <a name="arithmetic-operators"></a><span data-ttu-id="4e5da-812">算術演算子</span><span class="sxs-lookup"><span data-stu-id="4e5da-812">Arithmetic Operators</span></span>

<span data-ttu-id="4e5da-813">`*`、 `/`、 `\`、 `^`、 `Mod`、 `+`、および`-`演算子は、*算術演算子*します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-813">The `*`, `/`, `\`, `^`, `Mod`, `+`, and `-` operators are the *arithmetic operators*.</span></span>

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

<span data-ttu-id="4e5da-814">浮動小数点算術演算子は、操作の結果の型よりも高い精度で実行できます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-814">Floating-point arithmetic operations may be performed with higher precision than the result type of the operation.</span></span> <span data-ttu-id="4e5da-815">一部のハードウェア アーキテクチャでの大きい範囲とより有効桁数を持つ「拡張」または"long double"浮動小数点型のサポートなど、 `Double` 「」暗黙的にこの高精度型を使用してすべての浮動小数点演算を実行します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-815">For example, some hardware architectures support an "extended" or "long double" floating-point type with greater range and precision than the `Double` type, and implicitly perform all floating-point operations using this higher-precision type.</span></span> <span data-ttu-id="4e5da-816">パフォーマンス負荷精度の低い浮動小数点演算を実行するハードウェア アーキテクチャを行んだことができます。パフォーマンスと精度の両方を犠牲にする必要があるのではなく Visual Basic ではすべての浮動小数点演算に使用する高精度の型。</span><span class="sxs-lookup"><span data-stu-id="4e5da-816">Hardware architectures can be made to perform floating-point operations with less precision only at excessive cost in performance; rather than require an implementation to forfeit both performance and precision, Visual Basic allows the higher-precision type to be used for all floating-point operations.</span></span> <span data-ttu-id="4e5da-817">正確な結果を提供する以外はこのことはほとんどありません、測定可能な影響を与えます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-817">Other than delivering more precise results, this rarely has any measurable effects.</span></span> <span data-ttu-id="4e5da-818">ただし、フォームの式で`x * y / z`乗算が外部にある結果を生成します、`Double`範囲が、後続の除算に一時的な結果を表示、`Double`式であるという事実の範囲上の範囲で評価形式無限大ではなく生成する有限の結果が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-818">However, in expressions of the form `x * y / z`, where the multiplication produces a result that is outside the `Double` range, but the subsequent division brings the temporary result back into the `Double` range, the fact that the expression is evaluated in a higher-range format may cause a finite result to be produced instead of infinity.</span></span>


### <a name="unary-plus-operator"></a><span data-ttu-id="4e5da-819">単項プラス演算子</span><span class="sxs-lookup"><span data-stu-id="4e5da-819">Unary Plus Operator</span></span>

```antlr
UnaryPlusExpression
    : '+' Expression
    ;
```

<span data-ttu-id="4e5da-820">単項プラス演算子の定義は、 `Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、 `ULong`、 `Long`、 `Single`、 `Double`、および`Decimal`型.</span><span class="sxs-lookup"><span data-stu-id="4e5da-820">The unary plus operator is defined for the `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Single`, `Double`, and `Decimal` types.</span></span>

<span data-ttu-id="4e5da-821">__操作の種類:__</span><span class="sxs-lookup"><span data-stu-id="4e5da-821">__Operation Type:__</span></span>


| <span data-ttu-id="4e5da-822">__bo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-822">__Bo__</span></span> | <span data-ttu-id="4e5da-823">__SB__</span><span class="sxs-lookup"><span data-stu-id="4e5da-823">__SB__</span></span> | <span data-ttu-id="4e5da-824">__によって__</span><span class="sxs-lookup"><span data-stu-id="4e5da-824">__By__</span></span> | <span data-ttu-id="4e5da-825">__Sh__</span><span class="sxs-lookup"><span data-stu-id="4e5da-825">__Sh__</span></span> | <span data-ttu-id="4e5da-826">__米国__</span><span class="sxs-lookup"><span data-stu-id="4e5da-826">__US__</span></span> | <span data-ttu-id="4e5da-827">__In__</span><span class="sxs-lookup"><span data-stu-id="4e5da-827">__In__</span></span> | <span data-ttu-id="4e5da-828">__UI__</span><span class="sxs-lookup"><span data-stu-id="4e5da-828">__UI__</span></span> | <span data-ttu-id="4e5da-829">__Lo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-829">__Lo__</span></span> | <span data-ttu-id="4e5da-830">__UL__</span><span class="sxs-lookup"><span data-stu-id="4e5da-830">__UL__</span></span> | <span data-ttu-id="4e5da-831">__de__</span><span class="sxs-lookup"><span data-stu-id="4e5da-831">__De__</span></span> | <span data-ttu-id="4e5da-832">__si__</span><span class="sxs-lookup"><span data-stu-id="4e5da-832">__Si__</span></span> | <span data-ttu-id="4e5da-833">__Do__</span><span class="sxs-lookup"><span data-stu-id="4e5da-833">__Do__</span></span> | <span data-ttu-id="4e5da-834">__Da__</span><span class="sxs-lookup"><span data-stu-id="4e5da-834">__Da__</span></span>  | <span data-ttu-id="4e5da-835">__ch__</span><span class="sxs-lookup"><span data-stu-id="4e5da-835">__Ch__</span></span>  | <span data-ttu-id="4e5da-836">__st__</span><span class="sxs-lookup"><span data-stu-id="4e5da-836">__St__</span></span> | <span data-ttu-id="4e5da-837">__Ob__</span><span class="sxs-lookup"><span data-stu-id="4e5da-837">__Ob__</span></span> | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="4e5da-838">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-838">Sh</span></span> | <span data-ttu-id="4e5da-839">SB</span><span class="sxs-lookup"><span data-stu-id="4e5da-839">SB</span></span> | <span data-ttu-id="4e5da-840">By</span><span class="sxs-lookup"><span data-stu-id="4e5da-840">By</span></span> | <span data-ttu-id="4e5da-841">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-841">Sh</span></span> | <span data-ttu-id="4e5da-842">US</span><span class="sxs-lookup"><span data-stu-id="4e5da-842">US</span></span> | <span data-ttu-id="4e5da-843">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-843">In</span></span> | <span data-ttu-id="4e5da-844">UI</span><span class="sxs-lookup"><span data-stu-id="4e5da-844">UI</span></span> | <span data-ttu-id="4e5da-845">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-845">Lo</span></span> | <span data-ttu-id="4e5da-846">UL</span><span class="sxs-lookup"><span data-stu-id="4e5da-846">UL</span></span> | <span data-ttu-id="4e5da-847">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-847">De</span></span> | <span data-ttu-id="4e5da-848">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-848">Si</span></span> | <span data-ttu-id="4e5da-849">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-849">Do</span></span> | <span data-ttu-id="4e5da-850">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-850">Err</span></span> | <span data-ttu-id="4e5da-851">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-851">Err</span></span> | <span data-ttu-id="4e5da-852">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-852">Do</span></span> | <span data-ttu-id="4e5da-853">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-853">Ob</span></span> | 


### <a name="unary-minus-operator"></a><span data-ttu-id="4e5da-854">単項マイナス演算子</span><span class="sxs-lookup"><span data-stu-id="4e5da-854">Unary Minus Operator</span></span>

```antlr
UnaryMinusExpression
    : '-' Expression
    ;
```

<span data-ttu-id="4e5da-855">単項マイナス演算子は、次の種類に対して定義されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-855">The unary minus operator is defined for the following types:</span></span>

<span data-ttu-id="4e5da-856">`SByte`、 `Short`、 `Integer`、および `Long`。</span><span class="sxs-lookup"><span data-stu-id="4e5da-856">`SByte`, `Short`, `Integer`, and `Long`.</span></span> <span data-ttu-id="4e5da-857">結果は、0 からオペランドを減算して計算されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-857">The result is computed by subtracting the operand from zero.</span></span> <span data-ttu-id="4e5da-858">整数のオーバーフロー チェックが入っていて、オペランドの値が負の最大場合`SByte`、 `Short`、 `Integer`、または`Long`、`System.OverflowException`例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-858">If integer overflow checking is on and the value of the operand is the maximum negative `SByte`, `Short`, `Integer`, or `Long`, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="4e5da-859">それ以外の場合、オペランドの値が負の最大値の場合`SByte`、 `Short`、 `Integer`、または`Long`結果は同じ値をおよび、オーバーフローが報告されません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-859">Otherwise, if the value of the operand is the maximum negative `SByte`, `Short`, `Integer`, or `Long`, the result is that same value, and the overflow is not reported.</span></span>

<span data-ttu-id="4e5da-860">`Single` および `Double`。</span><span class="sxs-lookup"><span data-stu-id="4e5da-860">`Single` and `Double`.</span></span> <span data-ttu-id="4e5da-861">結果は、反転させて、その記号の値 0 と無限大オペランドの値です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-861">The result is the value of the operand with its sign inverted, including the values 0 and Infinity.</span></span> <span data-ttu-id="4e5da-862">オペランドが NaN の場合、結果も NaN にします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-862">If the operand is NaN, the result is also NaN.</span></span>

<span data-ttu-id="4e5da-863">`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="4e5da-863">`Decimal`.</span></span> <span data-ttu-id="4e5da-864">結果は、0 からオペランドを減算して計算されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-864">The result is computed by subtracting the operand from zero.</span></span>

<span data-ttu-id="4e5da-865">__操作の種類:__</span><span class="sxs-lookup"><span data-stu-id="4e5da-865">__Operation Type:__</span></span>

| <span data-ttu-id="4e5da-866">__bo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-866">__Bo__</span></span> | <span data-ttu-id="4e5da-867">__SB__</span><span class="sxs-lookup"><span data-stu-id="4e5da-867">__SB__</span></span> | <span data-ttu-id="4e5da-868">__によって__</span><span class="sxs-lookup"><span data-stu-id="4e5da-868">__By__</span></span> | <span data-ttu-id="4e5da-869">__Sh__</span><span class="sxs-lookup"><span data-stu-id="4e5da-869">__Sh__</span></span> | <span data-ttu-id="4e5da-870">__米国__</span><span class="sxs-lookup"><span data-stu-id="4e5da-870">__US__</span></span> | <span data-ttu-id="4e5da-871">__In__</span><span class="sxs-lookup"><span data-stu-id="4e5da-871">__In__</span></span> | <span data-ttu-id="4e5da-872">__UI__</span><span class="sxs-lookup"><span data-stu-id="4e5da-872">__UI__</span></span> | <span data-ttu-id="4e5da-873">__Lo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-873">__Lo__</span></span> | <span data-ttu-id="4e5da-874">__UL__</span><span class="sxs-lookup"><span data-stu-id="4e5da-874">__UL__</span></span> | <span data-ttu-id="4e5da-875">__de__</span><span class="sxs-lookup"><span data-stu-id="4e5da-875">__De__</span></span> | <span data-ttu-id="4e5da-876">__si__</span><span class="sxs-lookup"><span data-stu-id="4e5da-876">__Si__</span></span> | <span data-ttu-id="4e5da-877">__Do__</span><span class="sxs-lookup"><span data-stu-id="4e5da-877">__Do__</span></span> | <span data-ttu-id="4e5da-878">__Da__</span><span class="sxs-lookup"><span data-stu-id="4e5da-878">__Da__</span></span>  | <span data-ttu-id="4e5da-879">__ch__</span><span class="sxs-lookup"><span data-stu-id="4e5da-879">__Ch__</span></span>  | <span data-ttu-id="4e5da-880">__st__</span><span class="sxs-lookup"><span data-stu-id="4e5da-880">__St__</span></span> | <span data-ttu-id="4e5da-881">__Ob__</span><span class="sxs-lookup"><span data-stu-id="4e5da-881">__Ob__</span></span> | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="4e5da-882">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-882">Sh</span></span> | <span data-ttu-id="4e5da-883">SB</span><span class="sxs-lookup"><span data-stu-id="4e5da-883">SB</span></span> | <span data-ttu-id="4e5da-884">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-884">Sh</span></span> | <span data-ttu-id="4e5da-885">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-885">Sh</span></span> | <span data-ttu-id="4e5da-886">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-886">In</span></span> | <span data-ttu-id="4e5da-887">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-887">In</span></span> | <span data-ttu-id="4e5da-888">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-888">Lo</span></span> | <span data-ttu-id="4e5da-889">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-889">Lo</span></span> | <span data-ttu-id="4e5da-890">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-890">De</span></span> | <span data-ttu-id="4e5da-891">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-891">De</span></span> | <span data-ttu-id="4e5da-892">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-892">Si</span></span> | <span data-ttu-id="4e5da-893">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-893">Do</span></span> | <span data-ttu-id="4e5da-894">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-894">Err</span></span> | <span data-ttu-id="4e5da-895">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-895">Err</span></span> | <span data-ttu-id="4e5da-896">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-896">Do</span></span> | <span data-ttu-id="4e5da-897">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-897">Ob</span></span> | 


### <a name="addition-operator"></a><span data-ttu-id="4e5da-898">加算演算子</span><span class="sxs-lookup"><span data-stu-id="4e5da-898">Addition Operator</span></span>

<span data-ttu-id="4e5da-899">加算演算子は、2 つのオペランドの合計を計算します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-899">The addition operator computes the sum of the two operands.</span></span>

```antlr
AdditionOperatorExpression
    : Expression '+' LineTerminator? Expression
    ;
```

<span data-ttu-id="4e5da-900">加算演算子は、次の種類に対して定義されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-900">The addition operator is defined for the following types:</span></span>

* <span data-ttu-id="4e5da-901">`Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、および `Long`。</span><span class="sxs-lookup"><span data-stu-id="4e5da-901">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="4e5da-902">整数のオーバーフロー チェックが入っていて、結果の型の範囲外の合計が場合、`System.OverflowException`例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-902">If integer overflow checking is on and the sum is outside the range of the result type, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="4e5da-903">それ以外の場合、オーバーフローが報告していないと、結果の任意の大きな上位ビットが破棄されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-903">Otherwise, overflows are not reported, and any significant high-order bits of the result are discarded.</span></span>

* <span data-ttu-id="4e5da-904">`Single` および `Double`。</span><span class="sxs-lookup"><span data-stu-id="4e5da-904">`Single` and `Double`.</span></span> <span data-ttu-id="4e5da-905">合計は、IEEE 754 の演算の規則に従って計算されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-905">The sum is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="4e5da-906">`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="4e5da-906">`Decimal`.</span></span> <span data-ttu-id="4e5da-907">10 進数の形式で表すには、結果の値が大きすぎる場合、`System.OverflowException`例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-907">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="4e5da-908">結果の値が 10 進数形式で表す小さすぎる場合、結果は 0 です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-908">If the result value is too small to represent in the decimal format, the result is 0.</span></span>

* <span data-ttu-id="4e5da-909">`String`。</span><span class="sxs-lookup"><span data-stu-id="4e5da-909">`String`.</span></span> <span data-ttu-id="4e5da-910">2 つ`String`オペランドを連結します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-910">The two `String` operands are concatenated together.</span></span>

* <span data-ttu-id="4e5da-911">`Date`。</span><span class="sxs-lookup"><span data-stu-id="4e5da-911">`Date`.</span></span> <span data-ttu-id="4e5da-912">`System.DateTime`型オーバー ロードされた加算演算子を定義します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-912">The `System.DateTime` type defines overloaded addition operators.</span></span> <span data-ttu-id="4e5da-913">`System.DateTime`は、組み込みの等価`Date`型では、これらの演算子はできるも、`Date`型。</span><span class="sxs-lookup"><span data-stu-id="4e5da-913">Because `System.DateTime` is equivalent to the intrinsic `Date` type, these operators is also available on the `Date` type.</span></span>

<span data-ttu-id="4e5da-914">__操作の種類:__</span><span class="sxs-lookup"><span data-stu-id="4e5da-914">__Operation Type:__</span></span>

|        | <span data-ttu-id="4e5da-915">__bo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-915">__Bo__</span></span> | <span data-ttu-id="4e5da-916">__SB__</span><span class="sxs-lookup"><span data-stu-id="4e5da-916">__SB__</span></span> | <span data-ttu-id="4e5da-917">__によって__</span><span class="sxs-lookup"><span data-stu-id="4e5da-917">__By__</span></span> | <span data-ttu-id="4e5da-918">__Sh__</span><span class="sxs-lookup"><span data-stu-id="4e5da-918">__Sh__</span></span> | <span data-ttu-id="4e5da-919">__米国__</span><span class="sxs-lookup"><span data-stu-id="4e5da-919">__US__</span></span> | <span data-ttu-id="4e5da-920">__In__</span><span class="sxs-lookup"><span data-stu-id="4e5da-920">__In__</span></span> | <span data-ttu-id="4e5da-921">__UI__</span><span class="sxs-lookup"><span data-stu-id="4e5da-921">__UI__</span></span> | <span data-ttu-id="4e5da-922">__Lo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-922">__Lo__</span></span> | <span data-ttu-id="4e5da-923">__UL__</span><span class="sxs-lookup"><span data-stu-id="4e5da-923">__UL__</span></span> | <span data-ttu-id="4e5da-924">__de__</span><span class="sxs-lookup"><span data-stu-id="4e5da-924">__De__</span></span> | <span data-ttu-id="4e5da-925">__si__</span><span class="sxs-lookup"><span data-stu-id="4e5da-925">__Si__</span></span> | <span data-ttu-id="4e5da-926">__Do__</span><span class="sxs-lookup"><span data-stu-id="4e5da-926">__Do__</span></span> | <span data-ttu-id="4e5da-927">__Da__</span><span class="sxs-lookup"><span data-stu-id="4e5da-927">__Da__</span></span>  | <span data-ttu-id="4e5da-928">__ch__</span><span class="sxs-lookup"><span data-stu-id="4e5da-928">__Ch__</span></span>  | <span data-ttu-id="4e5da-929">__st__</span><span class="sxs-lookup"><span data-stu-id="4e5da-929">__St__</span></span> | <span data-ttu-id="4e5da-930">__Ob__</span><span class="sxs-lookup"><span data-stu-id="4e5da-930">__Ob__</span></span> | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="4e5da-931">__bo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-931">__Bo__</span></span> | <span data-ttu-id="4e5da-932">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-932">Sh</span></span> | <span data-ttu-id="4e5da-933">SB</span><span class="sxs-lookup"><span data-stu-id="4e5da-933">SB</span></span> | <span data-ttu-id="4e5da-934">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-934">Sh</span></span> | <span data-ttu-id="4e5da-935">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-935">Sh</span></span> | <span data-ttu-id="4e5da-936">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-936">In</span></span> | <span data-ttu-id="4e5da-937">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-937">In</span></span> | <span data-ttu-id="4e5da-938">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-938">Lo</span></span> | <span data-ttu-id="4e5da-939">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-939">Lo</span></span> | <span data-ttu-id="4e5da-940">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-940">De</span></span> | <span data-ttu-id="4e5da-941">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-941">De</span></span> | <span data-ttu-id="4e5da-942">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-942">Si</span></span> | <span data-ttu-id="4e5da-943">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-943">Do</span></span> | <span data-ttu-id="4e5da-944">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-944">Err</span></span> | <span data-ttu-id="4e5da-945">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-945">Err</span></span> | <span data-ttu-id="4e5da-946">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-946">Do</span></span> | <span data-ttu-id="4e5da-947">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-947">Ob</span></span> | 
| <span data-ttu-id="4e5da-948">__SB__</span><span class="sxs-lookup"><span data-stu-id="4e5da-948">__SB__</span></span> |    | <span data-ttu-id="4e5da-949">SB</span><span class="sxs-lookup"><span data-stu-id="4e5da-949">SB</span></span> | <span data-ttu-id="4e5da-950">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-950">Sh</span></span> | <span data-ttu-id="4e5da-951">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-951">Sh</span></span> | <span data-ttu-id="4e5da-952">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-952">In</span></span> | <span data-ttu-id="4e5da-953">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-953">In</span></span> | <span data-ttu-id="4e5da-954">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-954">Lo</span></span> | <span data-ttu-id="4e5da-955">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-955">Lo</span></span> | <span data-ttu-id="4e5da-956">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-956">De</span></span> | <span data-ttu-id="4e5da-957">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-957">De</span></span> | <span data-ttu-id="4e5da-958">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-958">Si</span></span> | <span data-ttu-id="4e5da-959">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-959">Do</span></span> | <span data-ttu-id="4e5da-960">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-960">Err</span></span> | <span data-ttu-id="4e5da-961">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-961">Err</span></span> | <span data-ttu-id="4e5da-962">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-962">Do</span></span> | <span data-ttu-id="4e5da-963">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-963">Ob</span></span> | 
| <span data-ttu-id="4e5da-964">__によって__</span><span class="sxs-lookup"><span data-stu-id="4e5da-964">__By__</span></span> |    |    | <span data-ttu-id="4e5da-965">By</span><span class="sxs-lookup"><span data-stu-id="4e5da-965">By</span></span> | <span data-ttu-id="4e5da-966">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-966">Sh</span></span> | <span data-ttu-id="4e5da-967">US</span><span class="sxs-lookup"><span data-stu-id="4e5da-967">US</span></span> | <span data-ttu-id="4e5da-968">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-968">In</span></span> | <span data-ttu-id="4e5da-969">UI</span><span class="sxs-lookup"><span data-stu-id="4e5da-969">UI</span></span> | <span data-ttu-id="4e5da-970">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-970">Lo</span></span> | <span data-ttu-id="4e5da-971">UL</span><span class="sxs-lookup"><span data-stu-id="4e5da-971">UL</span></span> | <span data-ttu-id="4e5da-972">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-972">De</span></span> | <span data-ttu-id="4e5da-973">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-973">Si</span></span> | <span data-ttu-id="4e5da-974">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-974">Do</span></span> | <span data-ttu-id="4e5da-975">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-975">Err</span></span> | <span data-ttu-id="4e5da-976">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-976">Err</span></span> | <span data-ttu-id="4e5da-977">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-977">Do</span></span> | <span data-ttu-id="4e5da-978">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-978">Ob</span></span> | 
| <span data-ttu-id="4e5da-979">__Sh__</span><span class="sxs-lookup"><span data-stu-id="4e5da-979">__Sh__</span></span> |    |    |    | <span data-ttu-id="4e5da-980">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-980">Sh</span></span> | <span data-ttu-id="4e5da-981">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-981">In</span></span> | <span data-ttu-id="4e5da-982">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-982">In</span></span> | <span data-ttu-id="4e5da-983">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-983">Lo</span></span> | <span data-ttu-id="4e5da-984">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-984">Lo</span></span> | <span data-ttu-id="4e5da-985">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-985">De</span></span> | <span data-ttu-id="4e5da-986">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-986">De</span></span> | <span data-ttu-id="4e5da-987">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-987">Si</span></span> | <span data-ttu-id="4e5da-988">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-988">Do</span></span> | <span data-ttu-id="4e5da-989">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-989">Err</span></span> | <span data-ttu-id="4e5da-990">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-990">Err</span></span> | <span data-ttu-id="4e5da-991">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-991">Do</span></span> | <span data-ttu-id="4e5da-992">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-992">Ob</span></span> | 
| <span data-ttu-id="4e5da-993">__米国__</span><span class="sxs-lookup"><span data-stu-id="4e5da-993">__US__</span></span> |    |    |    |    | <span data-ttu-id="4e5da-994">US</span><span class="sxs-lookup"><span data-stu-id="4e5da-994">US</span></span> | <span data-ttu-id="4e5da-995">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-995">In</span></span> | <span data-ttu-id="4e5da-996">UI</span><span class="sxs-lookup"><span data-stu-id="4e5da-996">UI</span></span> | <span data-ttu-id="4e5da-997">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-997">Lo</span></span> | <span data-ttu-id="4e5da-998">UL</span><span class="sxs-lookup"><span data-stu-id="4e5da-998">UL</span></span> | <span data-ttu-id="4e5da-999">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-999">De</span></span> | <span data-ttu-id="4e5da-1000">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1000">Si</span></span> | <span data-ttu-id="4e5da-1001">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1001">Do</span></span> | <span data-ttu-id="4e5da-1002">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1002">Err</span></span> | <span data-ttu-id="4e5da-1003">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1003">Err</span></span> | <span data-ttu-id="4e5da-1004">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1004">Do</span></span> | <span data-ttu-id="4e5da-1005">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1005">Ob</span></span> | 
| <span data-ttu-id="4e5da-1006">__In__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1006">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="4e5da-1007">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1007">In</span></span> | <span data-ttu-id="4e5da-1008">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1008">Lo</span></span> | <span data-ttu-id="4e5da-1009">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1009">Lo</span></span> | <span data-ttu-id="4e5da-1010">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1010">De</span></span> | <span data-ttu-id="4e5da-1011">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1011">De</span></span> | <span data-ttu-id="4e5da-1012">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1012">Si</span></span> | <span data-ttu-id="4e5da-1013">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1013">Do</span></span> | <span data-ttu-id="4e5da-1014">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1014">Err</span></span> | <span data-ttu-id="4e5da-1015">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1015">Err</span></span> | <span data-ttu-id="4e5da-1016">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1016">Do</span></span> | <span data-ttu-id="4e5da-1017">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1017">Ob</span></span> | 
| <span data-ttu-id="4e5da-1018">__UI__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1018">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="4e5da-1019">UI</span><span class="sxs-lookup"><span data-stu-id="4e5da-1019">UI</span></span> | <span data-ttu-id="4e5da-1020">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1020">Lo</span></span> | <span data-ttu-id="4e5da-1021">UL</span><span class="sxs-lookup"><span data-stu-id="4e5da-1021">UL</span></span> | <span data-ttu-id="4e5da-1022">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1022">De</span></span> | <span data-ttu-id="4e5da-1023">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1023">Si</span></span> | <span data-ttu-id="4e5da-1024">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1024">Do</span></span> | <span data-ttu-id="4e5da-1025">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1025">Err</span></span> | <span data-ttu-id="4e5da-1026">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1026">Err</span></span> | <span data-ttu-id="4e5da-1027">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1027">Do</span></span> | <span data-ttu-id="4e5da-1028">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1028">Ob</span></span> | 
| <span data-ttu-id="4e5da-1029">__Lo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1029">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1030">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1030">Lo</span></span> | <span data-ttu-id="4e5da-1031">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1031">De</span></span> | <span data-ttu-id="4e5da-1032">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1032">De</span></span> | <span data-ttu-id="4e5da-1033">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1033">Si</span></span> | <span data-ttu-id="4e5da-1034">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1034">Do</span></span> | <span data-ttu-id="4e5da-1035">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1035">Err</span></span> | <span data-ttu-id="4e5da-1036">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1036">Err</span></span> | <span data-ttu-id="4e5da-1037">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1037">Do</span></span> | <span data-ttu-id="4e5da-1038">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1038">Ob</span></span> | 
| <span data-ttu-id="4e5da-1039">__UL__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1039">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1040">UL</span><span class="sxs-lookup"><span data-stu-id="4e5da-1040">UL</span></span> | <span data-ttu-id="4e5da-1041">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1041">De</span></span> | <span data-ttu-id="4e5da-1042">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1042">Si</span></span> | <span data-ttu-id="4e5da-1043">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1043">Do</span></span> | <span data-ttu-id="4e5da-1044">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1044">Err</span></span> | <span data-ttu-id="4e5da-1045">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1045">Err</span></span> | <span data-ttu-id="4e5da-1046">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1046">Do</span></span> | <span data-ttu-id="4e5da-1047">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1047">Ob</span></span> | 
| <span data-ttu-id="4e5da-1048">__de__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1048">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1049">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1049">De</span></span> | <span data-ttu-id="4e5da-1050">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1050">Si</span></span> | <span data-ttu-id="4e5da-1051">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1051">Do</span></span> | <span data-ttu-id="4e5da-1052">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1052">Err</span></span> | <span data-ttu-id="4e5da-1053">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1053">Err</span></span> | <span data-ttu-id="4e5da-1054">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1054">Do</span></span> | <span data-ttu-id="4e5da-1055">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1055">Ob</span></span> | 
| <span data-ttu-id="4e5da-1056">__si__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1056">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1057">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1057">Si</span></span> | <span data-ttu-id="4e5da-1058">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1058">Do</span></span> | <span data-ttu-id="4e5da-1059">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1059">Err</span></span> | <span data-ttu-id="4e5da-1060">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1060">Err</span></span> | <span data-ttu-id="4e5da-1061">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1061">Do</span></span> | <span data-ttu-id="4e5da-1062">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1062">Ob</span></span> | 
| <span data-ttu-id="4e5da-1063">__Do__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1063">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1064">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1064">Do</span></span> | <span data-ttu-id="4e5da-1065">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1065">Err</span></span> | <span data-ttu-id="4e5da-1066">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1066">Err</span></span> | <span data-ttu-id="4e5da-1067">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1067">Do</span></span> | <span data-ttu-id="4e5da-1068">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1068">Ob</span></span> | 
| <span data-ttu-id="4e5da-1069">__Da__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1069">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1070">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-1070">St</span></span>  | <span data-ttu-id="4e5da-1071">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1071">Err</span></span> | <span data-ttu-id="4e5da-1072">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-1072">St</span></span> | <span data-ttu-id="4e5da-1073">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1073">Ob</span></span> | 
| <span data-ttu-id="4e5da-1074">__ch__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1074">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="4e5da-1075">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-1075">St</span></span>  | <span data-ttu-id="4e5da-1076">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-1076">St</span></span> | <span data-ttu-id="4e5da-1077">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1077">Ob</span></span> | 
| <span data-ttu-id="4e5da-1078">__st__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1078">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="4e5da-1079">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-1079">St</span></span> | <span data-ttu-id="4e5da-1080">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1080">Ob</span></span> | 
| <span data-ttu-id="4e5da-1081">__Ob__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1081">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |    | <span data-ttu-id="4e5da-1082">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1082">Ob</span></span> | 


### <a name="subtraction-operator"></a><span data-ttu-id="4e5da-1083">減算演算子</span><span class="sxs-lookup"><span data-stu-id="4e5da-1083">Subtraction Operator</span></span>

<span data-ttu-id="4e5da-1084">減算演算子は、最初のオペランドから 2 番目のオペランドを減算します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1084">The subtraction operator subtracts the second operand from the first operand.</span></span>

```antlr
SubtractionOperatorExpression
    : Expression '-' LineTerminator? Expression
    ;
```

<span data-ttu-id="4e5da-1085">減算演算子は、次の種類に対して定義されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1085">The subtraction operator is defined for the following types:</span></span>

* <span data-ttu-id="4e5da-1086">`Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、および `Long`。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1086">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="4e5da-1087">整数オーバーフローのチェックと違いは、結果の型の範囲外の場合、`System.OverflowException`例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1087">If integer overflow checking is on and the difference is outside the range of the result type, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="4e5da-1088">それ以外の場合、オーバーフローが報告していないと、結果の任意の大きな上位ビットが破棄されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1088">Otherwise, overflows are not reported, and any significant high-order bits of the result are discarded.</span></span>

* <span data-ttu-id="4e5da-1089">`Single` および `Double`。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1089">`Single` and `Double`.</span></span> <span data-ttu-id="4e5da-1090">違いは、IEEE 754 の演算の規則に従って計算されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1090">The difference is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="4e5da-1091">`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1091">`Decimal`.</span></span> <span data-ttu-id="4e5da-1092">10 進数の形式で表すには、結果の値が大きすぎる場合、`System.OverflowException`例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1092">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="4e5da-1093">結果の値が 10 進数形式で表す小さすぎる場合、結果は 0 です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1093">If the result value is too small to represent in the decimal format, the result is 0.</span></span>

* <span data-ttu-id="4e5da-1094">`Date`。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1094">`Date`.</span></span> <span data-ttu-id="4e5da-1095">`System.DateTime`型オーバー ロードされた減算演算子を定義します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1095">The `System.DateTime` type defines overloaded subtraction operators.</span></span> <span data-ttu-id="4e5da-1096">`System.DateTime`は、組み込みの等価`Date`型では、これらの演算子はできるも、`Date`型。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1096">Because `System.DateTime` is equivalent to the intrinsic `Date` type, these operators is also available on the `Date` type.</span></span>

<span data-ttu-id="4e5da-1097">__操作の種類:__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1097">__Operation Type:__</span></span>

|        | <span data-ttu-id="4e5da-1098">__bo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1098">__Bo__</span></span> | <span data-ttu-id="4e5da-1099">__SB__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1099">__SB__</span></span> | <span data-ttu-id="4e5da-1100">__によって__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1100">__By__</span></span> | <span data-ttu-id="4e5da-1101">__Sh__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1101">__Sh__</span></span> | <span data-ttu-id="4e5da-1102">__米国__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1102">__US__</span></span> | <span data-ttu-id="4e5da-1103">__In__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1103">__In__</span></span> | <span data-ttu-id="4e5da-1104">__UI__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1104">__UI__</span></span> | <span data-ttu-id="4e5da-1105">__Lo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1105">__Lo__</span></span> | <span data-ttu-id="4e5da-1106">__UL__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1106">__UL__</span></span> | <span data-ttu-id="4e5da-1107">__de__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1107">__De__</span></span> | <span data-ttu-id="4e5da-1108">__si__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1108">__Si__</span></span> | <span data-ttu-id="4e5da-1109">__Do__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1109">__Do__</span></span> | <span data-ttu-id="4e5da-1110">__Da__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1110">__Da__</span></span>  | <span data-ttu-id="4e5da-1111">__ch__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1111">__Ch__</span></span>  | <span data-ttu-id="4e5da-1112">__st__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1112">__St__</span></span> | <span data-ttu-id="4e5da-1113">__Ob__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1113">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="4e5da-1114">__bo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1114">__Bo__</span></span> | <span data-ttu-id="4e5da-1115">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-1115">Sh</span></span> | <span data-ttu-id="4e5da-1116">SB</span><span class="sxs-lookup"><span data-stu-id="4e5da-1116">SB</span></span> | <span data-ttu-id="4e5da-1117">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-1117">Sh</span></span> | <span data-ttu-id="4e5da-1118">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-1118">Sh</span></span> | <span data-ttu-id="4e5da-1119">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1119">In</span></span> | <span data-ttu-id="4e5da-1120">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1120">In</span></span> | <span data-ttu-id="4e5da-1121">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1121">Lo</span></span> | <span data-ttu-id="4e5da-1122">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1122">Lo</span></span> | <span data-ttu-id="4e5da-1123">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1123">De</span></span> | <span data-ttu-id="4e5da-1124">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1124">De</span></span> | <span data-ttu-id="4e5da-1125">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1125">Si</span></span> | <span data-ttu-id="4e5da-1126">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1126">Do</span></span> | <span data-ttu-id="4e5da-1127">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1127">Err</span></span> | <span data-ttu-id="4e5da-1128">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1128">Err</span></span> | <span data-ttu-id="4e5da-1129">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1129">Do</span></span>  | <span data-ttu-id="4e5da-1130">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1130">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1131">__SB__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1131">__SB__</span></span> |    | <span data-ttu-id="4e5da-1132">SB</span><span class="sxs-lookup"><span data-stu-id="4e5da-1132">SB</span></span> | <span data-ttu-id="4e5da-1133">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-1133">Sh</span></span> | <span data-ttu-id="4e5da-1134">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-1134">Sh</span></span> | <span data-ttu-id="4e5da-1135">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1135">In</span></span> | <span data-ttu-id="4e5da-1136">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1136">In</span></span> | <span data-ttu-id="4e5da-1137">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1137">Lo</span></span> | <span data-ttu-id="4e5da-1138">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1138">Lo</span></span> | <span data-ttu-id="4e5da-1139">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1139">De</span></span> | <span data-ttu-id="4e5da-1140">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1140">De</span></span> | <span data-ttu-id="4e5da-1141">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1141">Si</span></span> | <span data-ttu-id="4e5da-1142">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1142">Do</span></span> | <span data-ttu-id="4e5da-1143">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1143">Err</span></span> | <span data-ttu-id="4e5da-1144">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1144">Err</span></span> | <span data-ttu-id="4e5da-1145">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1145">Do</span></span>  | <span data-ttu-id="4e5da-1146">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1146">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1147">__によって__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1147">__By__</span></span> |    |    | <span data-ttu-id="4e5da-1148">By</span><span class="sxs-lookup"><span data-stu-id="4e5da-1148">By</span></span> | <span data-ttu-id="4e5da-1149">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-1149">Sh</span></span> | <span data-ttu-id="4e5da-1150">US</span><span class="sxs-lookup"><span data-stu-id="4e5da-1150">US</span></span> | <span data-ttu-id="4e5da-1151">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1151">In</span></span> | <span data-ttu-id="4e5da-1152">UI</span><span class="sxs-lookup"><span data-stu-id="4e5da-1152">UI</span></span> | <span data-ttu-id="4e5da-1153">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1153">Lo</span></span> | <span data-ttu-id="4e5da-1154">UL</span><span class="sxs-lookup"><span data-stu-id="4e5da-1154">UL</span></span> | <span data-ttu-id="4e5da-1155">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1155">De</span></span> | <span data-ttu-id="4e5da-1156">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1156">Si</span></span> | <span data-ttu-id="4e5da-1157">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1157">Do</span></span> | <span data-ttu-id="4e5da-1158">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1158">Err</span></span> | <span data-ttu-id="4e5da-1159">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1159">Err</span></span> | <span data-ttu-id="4e5da-1160">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1160">Do</span></span>  | <span data-ttu-id="4e5da-1161">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1161">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1162">__Sh__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1162">__Sh__</span></span> |    |    |    | <span data-ttu-id="4e5da-1163">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-1163">Sh</span></span> | <span data-ttu-id="4e5da-1164">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1164">In</span></span> | <span data-ttu-id="4e5da-1165">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1165">In</span></span> | <span data-ttu-id="4e5da-1166">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1166">Lo</span></span> | <span data-ttu-id="4e5da-1167">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1167">Lo</span></span> | <span data-ttu-id="4e5da-1168">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1168">De</span></span> | <span data-ttu-id="4e5da-1169">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1169">De</span></span> | <span data-ttu-id="4e5da-1170">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1170">Si</span></span> | <span data-ttu-id="4e5da-1171">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1171">Do</span></span> | <span data-ttu-id="4e5da-1172">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1172">Err</span></span> | <span data-ttu-id="4e5da-1173">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1173">Err</span></span> | <span data-ttu-id="4e5da-1174">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1174">Do</span></span>  | <span data-ttu-id="4e5da-1175">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1175">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1176">__米国__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1176">__US__</span></span> |    |    |    |    | <span data-ttu-id="4e5da-1177">US</span><span class="sxs-lookup"><span data-stu-id="4e5da-1177">US</span></span> | <span data-ttu-id="4e5da-1178">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1178">In</span></span> | <span data-ttu-id="4e5da-1179">UI</span><span class="sxs-lookup"><span data-stu-id="4e5da-1179">UI</span></span> | <span data-ttu-id="4e5da-1180">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1180">Lo</span></span> | <span data-ttu-id="4e5da-1181">UL</span><span class="sxs-lookup"><span data-stu-id="4e5da-1181">UL</span></span> | <span data-ttu-id="4e5da-1182">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1182">De</span></span> | <span data-ttu-id="4e5da-1183">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1183">Si</span></span> | <span data-ttu-id="4e5da-1184">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1184">Do</span></span> | <span data-ttu-id="4e5da-1185">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1185">Err</span></span> | <span data-ttu-id="4e5da-1186">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1186">Err</span></span> | <span data-ttu-id="4e5da-1187">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1187">Do</span></span>  | <span data-ttu-id="4e5da-1188">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1188">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1189">__In__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1189">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="4e5da-1190">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1190">In</span></span> | <span data-ttu-id="4e5da-1191">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1191">Lo</span></span> | <span data-ttu-id="4e5da-1192">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1192">Lo</span></span> | <span data-ttu-id="4e5da-1193">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1193">De</span></span> | <span data-ttu-id="4e5da-1194">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1194">De</span></span> | <span data-ttu-id="4e5da-1195">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1195">Si</span></span> | <span data-ttu-id="4e5da-1196">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1196">Do</span></span> | <span data-ttu-id="4e5da-1197">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1197">Err</span></span> | <span data-ttu-id="4e5da-1198">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1198">Err</span></span> | <span data-ttu-id="4e5da-1199">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1199">Do</span></span>  | <span data-ttu-id="4e5da-1200">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1200">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1201">__UI__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1201">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="4e5da-1202">UI</span><span class="sxs-lookup"><span data-stu-id="4e5da-1202">UI</span></span> | <span data-ttu-id="4e5da-1203">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1203">Lo</span></span> | <span data-ttu-id="4e5da-1204">UL</span><span class="sxs-lookup"><span data-stu-id="4e5da-1204">UL</span></span> | <span data-ttu-id="4e5da-1205">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1205">De</span></span> | <span data-ttu-id="4e5da-1206">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1206">Si</span></span> | <span data-ttu-id="4e5da-1207">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1207">Do</span></span> | <span data-ttu-id="4e5da-1208">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1208">Err</span></span> | <span data-ttu-id="4e5da-1209">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1209">Err</span></span> | <span data-ttu-id="4e5da-1210">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1210">Do</span></span>  | <span data-ttu-id="4e5da-1211">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1211">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1212">__Lo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1212">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1213">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1213">Lo</span></span> | <span data-ttu-id="4e5da-1214">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1214">De</span></span> | <span data-ttu-id="4e5da-1215">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1215">De</span></span> | <span data-ttu-id="4e5da-1216">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1216">Si</span></span> | <span data-ttu-id="4e5da-1217">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1217">Do</span></span> | <span data-ttu-id="4e5da-1218">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1218">Err</span></span> | <span data-ttu-id="4e5da-1219">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1219">Err</span></span> | <span data-ttu-id="4e5da-1220">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1220">Do</span></span>  | <span data-ttu-id="4e5da-1221">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1221">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1222">__UL__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1222">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1223">UL</span><span class="sxs-lookup"><span data-stu-id="4e5da-1223">UL</span></span> | <span data-ttu-id="4e5da-1224">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1224">De</span></span> | <span data-ttu-id="4e5da-1225">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1225">Si</span></span> | <span data-ttu-id="4e5da-1226">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1226">Do</span></span> | <span data-ttu-id="4e5da-1227">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1227">Err</span></span> | <span data-ttu-id="4e5da-1228">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1228">Err</span></span> | <span data-ttu-id="4e5da-1229">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1229">Do</span></span>  | <span data-ttu-id="4e5da-1230">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1230">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1231">__de__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1231">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1232">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1232">De</span></span> | <span data-ttu-id="4e5da-1233">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1233">Si</span></span> | <span data-ttu-id="4e5da-1234">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1234">Do</span></span> | <span data-ttu-id="4e5da-1235">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1235">Err</span></span> | <span data-ttu-id="4e5da-1236">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1236">Err</span></span> | <span data-ttu-id="4e5da-1237">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1237">Do</span></span>  | <span data-ttu-id="4e5da-1238">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1238">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1239">__si__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1239">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1240">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1240">Si</span></span> | <span data-ttu-id="4e5da-1241">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1241">Do</span></span> | <span data-ttu-id="4e5da-1242">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1242">Err</span></span> | <span data-ttu-id="4e5da-1243">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1243">Err</span></span> | <span data-ttu-id="4e5da-1244">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1244">Do</span></span>  | <span data-ttu-id="4e5da-1245">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1245">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1246">__Do__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1246">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1247">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1247">Do</span></span> | <span data-ttu-id="4e5da-1248">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1248">Err</span></span> | <span data-ttu-id="4e5da-1249">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1249">Err</span></span> | <span data-ttu-id="4e5da-1250">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1250">Do</span></span>  | <span data-ttu-id="4e5da-1251">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1251">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1252">__Da__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1252">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1253">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1253">Err</span></span> | <span data-ttu-id="4e5da-1254">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1254">Err</span></span> | <span data-ttu-id="4e5da-1255">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1255">Err</span></span> | <span data-ttu-id="4e5da-1256">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1256">Err</span></span> | 
| <span data-ttu-id="4e5da-1257">__ch__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1257">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="4e5da-1258">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1258">Err</span></span> | <span data-ttu-id="4e5da-1259">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1259">Err</span></span> | <span data-ttu-id="4e5da-1260">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1260">Err</span></span> | 
| <span data-ttu-id="4e5da-1261">__st__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1261">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="4e5da-1262">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1262">Do</span></span>  | <span data-ttu-id="4e5da-1263">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1263">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1264">__Ob__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1264">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="4e5da-1265">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1265">Ob</span></span>  | 


### <a name="multiplication-operator"></a><span data-ttu-id="4e5da-1266">乗算演算子</span><span class="sxs-lookup"><span data-stu-id="4e5da-1266">Multiplication Operator</span></span>

<span data-ttu-id="4e5da-1267">乗算演算子は、2 つのオペランドの積を計算します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1267">The multiplication operator computes the product of two operands.</span></span>

```antlr
MultiplicationOperatorExpression
    : Expression '*' LineTerminator? Expression
    ;
```

<span data-ttu-id="4e5da-1268">乗算演算子は、次の種類に対して定義されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1268">The multiplication operator is defined for the following types:</span></span>

* <span data-ttu-id="4e5da-1269">`Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、および `Long`。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1269">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="4e5da-1270">整数オーバーフローのチェックでは、製品は結果の型の範囲外の場合、`System.OverflowException`例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1270">If integer overflow checking is on and the product is outside the range of the result type, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="4e5da-1271">それ以外の場合、オーバーフローが報告していないと、結果の任意の大きな上位ビットが破棄されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1271">Otherwise, overflows are not reported, and any significant high-order bits of the result are discarded.</span></span>

* <span data-ttu-id="4e5da-1272">`Single` および `Double`。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1272">`Single` and `Double`.</span></span> <span data-ttu-id="4e5da-1273">製品は、IEEE 754 の演算の規則に従って計算されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1273">The product is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="4e5da-1274">`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1274">`Decimal`.</span></span> <span data-ttu-id="4e5da-1275">10 進数の形式で表すには、結果の値が大きすぎる場合、`System.OverflowException`例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1275">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="4e5da-1276">結果の値が 10 進数形式で表す小さすぎる場合、結果は 0 です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1276">If the result value is too small to represent in the decimal format, the result is 0.</span></span>

<span data-ttu-id="4e5da-1277">__操作の種類:__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1277">__Operation Type:__</span></span>

|        | <span data-ttu-id="4e5da-1278">__bo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1278">__Bo__</span></span> | <span data-ttu-id="4e5da-1279">__SB__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1279">__SB__</span></span> | <span data-ttu-id="4e5da-1280">__によって__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1280">__By__</span></span> | <span data-ttu-id="4e5da-1281">__Sh__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1281">__Sh__</span></span> | <span data-ttu-id="4e5da-1282">__米国__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1282">__US__</span></span> | <span data-ttu-id="4e5da-1283">__In__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1283">__In__</span></span> | <span data-ttu-id="4e5da-1284">__UI__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1284">__UI__</span></span> | <span data-ttu-id="4e5da-1285">__Lo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1285">__Lo__</span></span> | <span data-ttu-id="4e5da-1286">__UL__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1286">__UL__</span></span> | <span data-ttu-id="4e5da-1287">__de__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1287">__De__</span></span> | <span data-ttu-id="4e5da-1288">__si__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1288">__Si__</span></span> | <span data-ttu-id="4e5da-1289">__Do__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1289">__Do__</span></span> | <span data-ttu-id="4e5da-1290">__Da__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1290">__Da__</span></span>  | <span data-ttu-id="4e5da-1291">__ch__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1291">__Ch__</span></span>  | <span data-ttu-id="4e5da-1292">__st__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1292">__St__</span></span> | <span data-ttu-id="4e5da-1293">__Ob__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1293">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="4e5da-1294">__bo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1294">__Bo__</span></span> | <span data-ttu-id="4e5da-1295">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-1295">Sh</span></span> | <span data-ttu-id="4e5da-1296">SB</span><span class="sxs-lookup"><span data-stu-id="4e5da-1296">SB</span></span> | <span data-ttu-id="4e5da-1297">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-1297">Sh</span></span> | <span data-ttu-id="4e5da-1298">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-1298">Sh</span></span> | <span data-ttu-id="4e5da-1299">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1299">In</span></span> | <span data-ttu-id="4e5da-1300">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1300">In</span></span> | <span data-ttu-id="4e5da-1301">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1301">Lo</span></span> | <span data-ttu-id="4e5da-1302">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1302">Lo</span></span> | <span data-ttu-id="4e5da-1303">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1303">De</span></span> | <span data-ttu-id="4e5da-1304">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1304">De</span></span> | <span data-ttu-id="4e5da-1305">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1305">Si</span></span> | <span data-ttu-id="4e5da-1306">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1306">Do</span></span> | <span data-ttu-id="4e5da-1307">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1307">Err</span></span> | <span data-ttu-id="4e5da-1308">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1308">Err</span></span> | <span data-ttu-id="4e5da-1309">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1309">Do</span></span>  | <span data-ttu-id="4e5da-1310">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1310">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1311">__SB__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1311">__SB__</span></span> |    | <span data-ttu-id="4e5da-1312">SB</span><span class="sxs-lookup"><span data-stu-id="4e5da-1312">SB</span></span> | <span data-ttu-id="4e5da-1313">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-1313">Sh</span></span> | <span data-ttu-id="4e5da-1314">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-1314">Sh</span></span> | <span data-ttu-id="4e5da-1315">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1315">In</span></span> | <span data-ttu-id="4e5da-1316">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1316">In</span></span> | <span data-ttu-id="4e5da-1317">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1317">Lo</span></span> | <span data-ttu-id="4e5da-1318">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1318">Lo</span></span> | <span data-ttu-id="4e5da-1319">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1319">De</span></span> | <span data-ttu-id="4e5da-1320">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1320">De</span></span> | <span data-ttu-id="4e5da-1321">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1321">Si</span></span> | <span data-ttu-id="4e5da-1322">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1322">Do</span></span> | <span data-ttu-id="4e5da-1323">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1323">Err</span></span> | <span data-ttu-id="4e5da-1324">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1324">Err</span></span> | <span data-ttu-id="4e5da-1325">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1325">Do</span></span>  | <span data-ttu-id="4e5da-1326">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1326">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1327">__によって__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1327">__By__</span></span> |    |    | <span data-ttu-id="4e5da-1328">By</span><span class="sxs-lookup"><span data-stu-id="4e5da-1328">By</span></span> | <span data-ttu-id="4e5da-1329">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-1329">Sh</span></span> | <span data-ttu-id="4e5da-1330">US</span><span class="sxs-lookup"><span data-stu-id="4e5da-1330">US</span></span> | <span data-ttu-id="4e5da-1331">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1331">In</span></span> | <span data-ttu-id="4e5da-1332">UI</span><span class="sxs-lookup"><span data-stu-id="4e5da-1332">UI</span></span> | <span data-ttu-id="4e5da-1333">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1333">Lo</span></span> | <span data-ttu-id="4e5da-1334">UL</span><span class="sxs-lookup"><span data-stu-id="4e5da-1334">UL</span></span> | <span data-ttu-id="4e5da-1335">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1335">De</span></span> | <span data-ttu-id="4e5da-1336">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1336">Si</span></span> | <span data-ttu-id="4e5da-1337">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1337">Do</span></span> | <span data-ttu-id="4e5da-1338">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1338">Err</span></span> | <span data-ttu-id="4e5da-1339">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1339">Err</span></span> | <span data-ttu-id="4e5da-1340">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1340">Do</span></span>  | <span data-ttu-id="4e5da-1341">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1341">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1342">__Sh__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1342">__Sh__</span></span> |    |    |    | <span data-ttu-id="4e5da-1343">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-1343">Sh</span></span> | <span data-ttu-id="4e5da-1344">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1344">In</span></span> | <span data-ttu-id="4e5da-1345">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1345">In</span></span> | <span data-ttu-id="4e5da-1346">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1346">Lo</span></span> | <span data-ttu-id="4e5da-1347">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1347">Lo</span></span> | <span data-ttu-id="4e5da-1348">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1348">De</span></span> | <span data-ttu-id="4e5da-1349">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1349">De</span></span> | <span data-ttu-id="4e5da-1350">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1350">Si</span></span> | <span data-ttu-id="4e5da-1351">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1351">Do</span></span> | <span data-ttu-id="4e5da-1352">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1352">Err</span></span> | <span data-ttu-id="4e5da-1353">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1353">Err</span></span> | <span data-ttu-id="4e5da-1354">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1354">Do</span></span>  | <span data-ttu-id="4e5da-1355">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1355">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1356">__米国__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1356">__US__</span></span> |    |    |    |    | <span data-ttu-id="4e5da-1357">US</span><span class="sxs-lookup"><span data-stu-id="4e5da-1357">US</span></span> | <span data-ttu-id="4e5da-1358">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1358">In</span></span> | <span data-ttu-id="4e5da-1359">UI</span><span class="sxs-lookup"><span data-stu-id="4e5da-1359">UI</span></span> | <span data-ttu-id="4e5da-1360">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1360">Lo</span></span> | <span data-ttu-id="4e5da-1361">UL</span><span class="sxs-lookup"><span data-stu-id="4e5da-1361">UL</span></span> | <span data-ttu-id="4e5da-1362">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1362">De</span></span> | <span data-ttu-id="4e5da-1363">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1363">Si</span></span> | <span data-ttu-id="4e5da-1364">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1364">Do</span></span> | <span data-ttu-id="4e5da-1365">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1365">Err</span></span> | <span data-ttu-id="4e5da-1366">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1366">Err</span></span> | <span data-ttu-id="4e5da-1367">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1367">Do</span></span>  | <span data-ttu-id="4e5da-1368">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1368">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1369">__In__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1369">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="4e5da-1370">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1370">In</span></span> | <span data-ttu-id="4e5da-1371">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1371">Lo</span></span> | <span data-ttu-id="4e5da-1372">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1372">Lo</span></span> | <span data-ttu-id="4e5da-1373">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1373">De</span></span> | <span data-ttu-id="4e5da-1374">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1374">De</span></span> | <span data-ttu-id="4e5da-1375">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1375">Si</span></span> | <span data-ttu-id="4e5da-1376">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1376">Do</span></span> | <span data-ttu-id="4e5da-1377">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1377">Err</span></span> | <span data-ttu-id="4e5da-1378">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1378">Err</span></span> | <span data-ttu-id="4e5da-1379">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1379">Do</span></span>  | <span data-ttu-id="4e5da-1380">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1380">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1381">__UI__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1381">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="4e5da-1382">UI</span><span class="sxs-lookup"><span data-stu-id="4e5da-1382">UI</span></span> | <span data-ttu-id="4e5da-1383">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1383">Lo</span></span> | <span data-ttu-id="4e5da-1384">UL</span><span class="sxs-lookup"><span data-stu-id="4e5da-1384">UL</span></span> | <span data-ttu-id="4e5da-1385">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1385">De</span></span> | <span data-ttu-id="4e5da-1386">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1386">Si</span></span> | <span data-ttu-id="4e5da-1387">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1387">Do</span></span> | <span data-ttu-id="4e5da-1388">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1388">Err</span></span> | <span data-ttu-id="4e5da-1389">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1389">Err</span></span> | <span data-ttu-id="4e5da-1390">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1390">Do</span></span>  | <span data-ttu-id="4e5da-1391">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1391">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1392">__Lo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1392">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1393">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1393">Lo</span></span> | <span data-ttu-id="4e5da-1394">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1394">De</span></span> | <span data-ttu-id="4e5da-1395">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1395">De</span></span> | <span data-ttu-id="4e5da-1396">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1396">Si</span></span> | <span data-ttu-id="4e5da-1397">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1397">Do</span></span> | <span data-ttu-id="4e5da-1398">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1398">Err</span></span> | <span data-ttu-id="4e5da-1399">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1399">Err</span></span> | <span data-ttu-id="4e5da-1400">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1400">Do</span></span>  | <span data-ttu-id="4e5da-1401">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1401">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1402">__UL__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1402">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1403">UL</span><span class="sxs-lookup"><span data-stu-id="4e5da-1403">UL</span></span> | <span data-ttu-id="4e5da-1404">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1404">De</span></span> | <span data-ttu-id="4e5da-1405">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1405">Si</span></span> | <span data-ttu-id="4e5da-1406">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1406">Do</span></span> | <span data-ttu-id="4e5da-1407">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1407">Err</span></span> | <span data-ttu-id="4e5da-1408">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1408">Err</span></span> | <span data-ttu-id="4e5da-1409">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1409">Do</span></span>  | <span data-ttu-id="4e5da-1410">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1410">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1411">__de__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1411">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1412">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1412">De</span></span> | <span data-ttu-id="4e5da-1413">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1413">Si</span></span> | <span data-ttu-id="4e5da-1414">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1414">Do</span></span> | <span data-ttu-id="4e5da-1415">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1415">Err</span></span> | <span data-ttu-id="4e5da-1416">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1416">Err</span></span> | <span data-ttu-id="4e5da-1417">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1417">Do</span></span>  | <span data-ttu-id="4e5da-1418">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1418">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1419">__si__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1419">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1420">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1420">Si</span></span> | <span data-ttu-id="4e5da-1421">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1421">Do</span></span> | <span data-ttu-id="4e5da-1422">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1422">Err</span></span> | <span data-ttu-id="4e5da-1423">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1423">Err</span></span> | <span data-ttu-id="4e5da-1424">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1424">Do</span></span>  | <span data-ttu-id="4e5da-1425">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1425">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1426">__Do__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1426">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1427">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1427">Do</span></span> | <span data-ttu-id="4e5da-1428">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1428">Err</span></span> | <span data-ttu-id="4e5da-1429">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1429">Err</span></span> | <span data-ttu-id="4e5da-1430">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1430">Do</span></span>  | <span data-ttu-id="4e5da-1431">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1431">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1432">__Da__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1432">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1433">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1433">Err</span></span> | <span data-ttu-id="4e5da-1434">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1434">Err</span></span> | <span data-ttu-id="4e5da-1435">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1435">Err</span></span> | <span data-ttu-id="4e5da-1436">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1436">Err</span></span> | 
| <span data-ttu-id="4e5da-1437">__ch__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1437">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="4e5da-1438">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1438">Err</span></span> | <span data-ttu-id="4e5da-1439">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1439">Err</span></span> | <span data-ttu-id="4e5da-1440">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1440">Err</span></span> | 
| <span data-ttu-id="4e5da-1441">__st__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1441">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="4e5da-1442">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1442">Do</span></span>  | <span data-ttu-id="4e5da-1443">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1443">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1444">__Ob__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1444">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="4e5da-1445">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1445">Ob</span></span>  | 


### <a name="division-operators"></a><span data-ttu-id="4e5da-1446">除算演算子</span><span class="sxs-lookup"><span data-stu-id="4e5da-1446">Division Operators</span></span>

<span data-ttu-id="4e5da-1447">除算演算子は、2 つのオペランドの商を計算します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1447">Division operators compute the quotient of two operands.</span></span> <span data-ttu-id="4e5da-1448">2 つの除算演算子があります。 通常の (浮動小数点) 除算演算子および整数の除算演算子。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1448">There are two division operators: the regular (floating-point) division operator and the integer division operator.</span></span>

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

<span data-ttu-id="4e5da-1449">通常の除算演算子は、次の種類に対して定義されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1449">The regular division operator is defined for the following types:</span></span>

* <span data-ttu-id="4e5da-1450">`Single` および `Double`。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1450">`Single` and `Double`.</span></span> <span data-ttu-id="4e5da-1451">商は、IEEE 754 の演算の規則に従って計算されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1451">The quotient is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="4e5da-1452">`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1452">`Decimal`.</span></span> <span data-ttu-id="4e5da-1453">右側のオペランドの値が 0 の場合、`System.DivideByZeroException`例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1453">If the value of the right operand is zero, a `System.DivideByZeroException` exception is thrown.</span></span> <span data-ttu-id="4e5da-1454">10 進数の形式で表すには、結果の値が大きすぎる場合、`System.OverflowException`例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1454">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="4e5da-1455">結果の値が 10 進数形式で表す小さすぎる場合、結果は 0 です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1455">If the result value is too small to represent in the decimal format, the result is zero.</span></span> <span data-ttu-id="4e5da-1456">丸めを行う前に、結果の小数点以下桁数は、最も近いスケール優先の小数点以下桁数が、正確な結果と等しく、結果を保持します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1456">The scale of the result, before any rounding, is the closest scale to the preferred scale which will preserve a result equal to the exact result.</span></span>  <span data-ttu-id="4e5da-1457">推奨されるスケールは、2 番目のオペランドの小数点以下桁数以下の最初のオペランドの小数点以下桁数です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1457">The preferred scale is the scale of the first operand less the scale of the second operand.</span></span>

<span data-ttu-id="4e5da-1458">通常の演算子の解決規則、通常の除算などの型のオペランドの間で純粋に従って`Byte`、 `Short`、 `Integer`、および`Long`型に変換する両方のオペランドとなる`Decimal`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1458">According to normal operator resolution rules, regular division purely between operands of types such as `Byte`, `Short`, `Integer`, and `Long` would cause both operands to be converted to type `Decimal`.</span></span> <span data-ttu-id="4e5da-1459">ただし、どちらの種類がときに除算演算子での解決に演算子を行って`Decimal`、`Double`より狭い幅と見なされます`Decimal`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1459">However, when doing operator resolution on the division operator when neither type is `Decimal`, `Double` is considered narrower than `Decimal`.</span></span> <span data-ttu-id="4e5da-1460">ため、この規則に従った`Double`部門がよりも効率的です。`Decimal`除算します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1460">This convention is followed because `Double` division is more efficient than `Decimal` division.</span></span>

<span data-ttu-id="4e5da-1461">__操作の種類:__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1461">__Operation Type:__</span></span>

|        | <span data-ttu-id="4e5da-1462">__bo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1462">__Bo__</span></span> | <span data-ttu-id="4e5da-1463">__SB__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1463">__SB__</span></span> | <span data-ttu-id="4e5da-1464">__によって__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1464">__By__</span></span> | <span data-ttu-id="4e5da-1465">__Sh__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1465">__Sh__</span></span> | <span data-ttu-id="4e5da-1466">__米国__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1466">__US__</span></span> | <span data-ttu-id="4e5da-1467">__In__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1467">__In__</span></span> | <span data-ttu-id="4e5da-1468">__UI__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1468">__UI__</span></span> | <span data-ttu-id="4e5da-1469">__Lo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1469">__Lo__</span></span> | <span data-ttu-id="4e5da-1470">__UL__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1470">__UL__</span></span> | <span data-ttu-id="4e5da-1471">__de__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1471">__De__</span></span> | <span data-ttu-id="4e5da-1472">__si__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1472">__Si__</span></span> | <span data-ttu-id="4e5da-1473">__Do__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1473">__Do__</span></span> | <span data-ttu-id="4e5da-1474">__Da__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1474">__Da__</span></span>  | <span data-ttu-id="4e5da-1475">__ch__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1475">__Ch__</span></span>  | <span data-ttu-id="4e5da-1476">__st__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1476">__St__</span></span> | <span data-ttu-id="4e5da-1477">__Ob__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1477">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="4e5da-1478">__bo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1478">__Bo__</span></span> | <span data-ttu-id="4e5da-1479">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1479">Do</span></span> | <span data-ttu-id="4e5da-1480">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1480">Do</span></span> | <span data-ttu-id="4e5da-1481">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1481">Do</span></span> | <span data-ttu-id="4e5da-1482">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1482">Do</span></span> | <span data-ttu-id="4e5da-1483">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1483">Do</span></span> | <span data-ttu-id="4e5da-1484">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1484">Do</span></span> | <span data-ttu-id="4e5da-1485">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1485">Do</span></span> | <span data-ttu-id="4e5da-1486">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1486">Do</span></span> | <span data-ttu-id="4e5da-1487">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1487">Do</span></span> | <span data-ttu-id="4e5da-1488">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1488">De</span></span> | <span data-ttu-id="4e5da-1489">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1489">Si</span></span> | <span data-ttu-id="4e5da-1490">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1490">Do</span></span> | <span data-ttu-id="4e5da-1491">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1491">Err</span></span> | <span data-ttu-id="4e5da-1492">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1492">Err</span></span> | <span data-ttu-id="4e5da-1493">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1493">Do</span></span>  | <span data-ttu-id="4e5da-1494">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1494">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1495">__SB__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1495">__SB__</span></span> |    | <span data-ttu-id="4e5da-1496">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1496">Do</span></span> | <span data-ttu-id="4e5da-1497">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1497">Do</span></span> | <span data-ttu-id="4e5da-1498">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1498">Do</span></span> | <span data-ttu-id="4e5da-1499">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1499">Do</span></span> | <span data-ttu-id="4e5da-1500">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1500">Do</span></span> | <span data-ttu-id="4e5da-1501">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1501">Do</span></span> | <span data-ttu-id="4e5da-1502">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1502">Do</span></span> | <span data-ttu-id="4e5da-1503">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1503">Do</span></span> | <span data-ttu-id="4e5da-1504">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1504">De</span></span> | <span data-ttu-id="4e5da-1505">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1505">Si</span></span> | <span data-ttu-id="4e5da-1506">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1506">Do</span></span> | <span data-ttu-id="4e5da-1507">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1507">Err</span></span> | <span data-ttu-id="4e5da-1508">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1508">Err</span></span> | <span data-ttu-id="4e5da-1509">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1509">Do</span></span>  | <span data-ttu-id="4e5da-1510">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1510">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1511">__によって__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1511">__By__</span></span> |    |    | <span data-ttu-id="4e5da-1512">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1512">Do</span></span> | <span data-ttu-id="4e5da-1513">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1513">Do</span></span> | <span data-ttu-id="4e5da-1514">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1514">Do</span></span> | <span data-ttu-id="4e5da-1515">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1515">Do</span></span> | <span data-ttu-id="4e5da-1516">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1516">Do</span></span> | <span data-ttu-id="4e5da-1517">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1517">Do</span></span> | <span data-ttu-id="4e5da-1518">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1518">Do</span></span> | <span data-ttu-id="4e5da-1519">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1519">De</span></span> | <span data-ttu-id="4e5da-1520">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1520">Si</span></span> | <span data-ttu-id="4e5da-1521">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1521">Do</span></span> | <span data-ttu-id="4e5da-1522">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1522">Err</span></span> | <span data-ttu-id="4e5da-1523">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1523">Err</span></span> | <span data-ttu-id="4e5da-1524">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1524">Do</span></span>  | <span data-ttu-id="4e5da-1525">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1525">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1526">__Sh__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1526">__Sh__</span></span> |    |    |    | <span data-ttu-id="4e5da-1527">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1527">Do</span></span> | <span data-ttu-id="4e5da-1528">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1528">Do</span></span> | <span data-ttu-id="4e5da-1529">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1529">Do</span></span> | <span data-ttu-id="4e5da-1530">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1530">Do</span></span> | <span data-ttu-id="4e5da-1531">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1531">Do</span></span> | <span data-ttu-id="4e5da-1532">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1532">Do</span></span> | <span data-ttu-id="4e5da-1533">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1533">De</span></span> | <span data-ttu-id="4e5da-1534">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1534">Si</span></span> | <span data-ttu-id="4e5da-1535">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1535">Do</span></span> | <span data-ttu-id="4e5da-1536">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1536">Err</span></span> | <span data-ttu-id="4e5da-1537">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1537">Err</span></span> | <span data-ttu-id="4e5da-1538">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1538">Do</span></span>  | <span data-ttu-id="4e5da-1539">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1539">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1540">__米国__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1540">__US__</span></span> |    |    |    |    | <span data-ttu-id="4e5da-1541">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1541">Do</span></span> | <span data-ttu-id="4e5da-1542">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1542">Do</span></span> | <span data-ttu-id="4e5da-1543">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1543">Do</span></span> | <span data-ttu-id="4e5da-1544">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1544">Do</span></span> | <span data-ttu-id="4e5da-1545">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1545">Do</span></span> | <span data-ttu-id="4e5da-1546">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1546">De</span></span> | <span data-ttu-id="4e5da-1547">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1547">Si</span></span> | <span data-ttu-id="4e5da-1548">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1548">Do</span></span> | <span data-ttu-id="4e5da-1549">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1549">Err</span></span> | <span data-ttu-id="4e5da-1550">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1550">Err</span></span> | <span data-ttu-id="4e5da-1551">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1551">Do</span></span>  | <span data-ttu-id="4e5da-1552">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1552">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1553">__In__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1553">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="4e5da-1554">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1554">Do</span></span> | <span data-ttu-id="4e5da-1555">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1555">Do</span></span> | <span data-ttu-id="4e5da-1556">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1556">Do</span></span> | <span data-ttu-id="4e5da-1557">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1557">Do</span></span> | <span data-ttu-id="4e5da-1558">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1558">De</span></span> | <span data-ttu-id="4e5da-1559">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1559">Si</span></span> | <span data-ttu-id="4e5da-1560">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1560">Do</span></span> | <span data-ttu-id="4e5da-1561">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1561">Err</span></span> | <span data-ttu-id="4e5da-1562">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1562">Err</span></span> | <span data-ttu-id="4e5da-1563">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1563">Do</span></span>  | <span data-ttu-id="4e5da-1564">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1564">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1565">__UI__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1565">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="4e5da-1566">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1566">Do</span></span> | <span data-ttu-id="4e5da-1567">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1567">Do</span></span> | <span data-ttu-id="4e5da-1568">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1568">Do</span></span> | <span data-ttu-id="4e5da-1569">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1569">De</span></span> | <span data-ttu-id="4e5da-1570">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1570">Si</span></span> | <span data-ttu-id="4e5da-1571">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1571">Do</span></span> | <span data-ttu-id="4e5da-1572">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1572">Err</span></span> | <span data-ttu-id="4e5da-1573">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1573">Err</span></span> | <span data-ttu-id="4e5da-1574">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1574">Do</span></span>  | <span data-ttu-id="4e5da-1575">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1575">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1576">__Lo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1576">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1577">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1577">Do</span></span> | <span data-ttu-id="4e5da-1578">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1578">Do</span></span> | <span data-ttu-id="4e5da-1579">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1579">De</span></span> | <span data-ttu-id="4e5da-1580">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1580">Si</span></span> | <span data-ttu-id="4e5da-1581">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1581">Do</span></span> | <span data-ttu-id="4e5da-1582">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1582">Err</span></span> | <span data-ttu-id="4e5da-1583">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1583">Err</span></span> | <span data-ttu-id="4e5da-1584">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1584">Do</span></span>  | <span data-ttu-id="4e5da-1585">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1585">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1586">__UL__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1586">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1587">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1587">Do</span></span> | <span data-ttu-id="4e5da-1588">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1588">De</span></span> | <span data-ttu-id="4e5da-1589">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1589">Si</span></span> | <span data-ttu-id="4e5da-1590">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1590">Do</span></span> | <span data-ttu-id="4e5da-1591">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1591">Err</span></span> | <span data-ttu-id="4e5da-1592">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1592">Err</span></span> | <span data-ttu-id="4e5da-1593">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1593">Do</span></span>  | <span data-ttu-id="4e5da-1594">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1594">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1595">__de__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1595">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1596">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1596">De</span></span> | <span data-ttu-id="4e5da-1597">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1597">Si</span></span> | <span data-ttu-id="4e5da-1598">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1598">Do</span></span> | <span data-ttu-id="4e5da-1599">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1599">Err</span></span> | <span data-ttu-id="4e5da-1600">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1600">Err</span></span> | <span data-ttu-id="4e5da-1601">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1601">Do</span></span>  | <span data-ttu-id="4e5da-1602">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1602">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1603">__si__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1603">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1604">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1604">Si</span></span> | <span data-ttu-id="4e5da-1605">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1605">Do</span></span> | <span data-ttu-id="4e5da-1606">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1606">Err</span></span> | <span data-ttu-id="4e5da-1607">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1607">Err</span></span> | <span data-ttu-id="4e5da-1608">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1608">Do</span></span>  | <span data-ttu-id="4e5da-1609">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1609">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1610">__Do__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1610">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1611">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1611">Do</span></span> | <span data-ttu-id="4e5da-1612">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1612">Err</span></span> | <span data-ttu-id="4e5da-1613">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1613">Err</span></span> | <span data-ttu-id="4e5da-1614">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1614">Do</span></span>  | <span data-ttu-id="4e5da-1615">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1615">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1616">__Da__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1616">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1617">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1617">Err</span></span> | <span data-ttu-id="4e5da-1618">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1618">Err</span></span> | <span data-ttu-id="4e5da-1619">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1619">Err</span></span> | <span data-ttu-id="4e5da-1620">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1620">Err</span></span> | 
| <span data-ttu-id="4e5da-1621">__ch__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1621">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="4e5da-1622">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1622">Err</span></span> | <span data-ttu-id="4e5da-1623">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1623">Err</span></span> | <span data-ttu-id="4e5da-1624">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1624">Err</span></span> | 
| <span data-ttu-id="4e5da-1625">__st__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1625">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="4e5da-1626">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1626">Do</span></span>  | <span data-ttu-id="4e5da-1627">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1627">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1628">__Ob__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1628">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="4e5da-1629">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1629">Ob</span></span>  | 

<span data-ttu-id="4e5da-1630">整数の除算演算子が定義されて`Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、 `ULong`、および`Long`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1630">The integer division operator is defined for `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="4e5da-1631">右側のオペランドの値が 0 の場合、`System.DivideByZeroException`例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1631">If the value of the right operand is zero, a `System.DivideByZeroException` exception is thrown.</span></span> <span data-ttu-id="4e5da-1632">除算を 0 方向に結果を丸めます、結果の絶対値は、2 つのオペランドの商の絶対値より小さい最大整数。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1632">The division rounds the result towards zero, and the absolute value of the result is the largest possible integer that is less than the absolute value of the quotient of the two operands.</span></span> <span data-ttu-id="4e5da-1633">結果は、2 つのオペランドが同じの符号を持つと 0 または負の記号の反対側の 2 つのオペランドがときに 0 または正の値。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1633">The result is zero or positive when the two operands have the same sign, and zero or negative when the two operands have opposite signs.</span></span> <span data-ttu-id="4e5da-1634">場合は、左側のオペランドが負の最大`SByte`、 `Short`、 `Integer`、または`Long`、および右のオペランドが`-1`、オーバーフローが発生します。 場合、整数のオーバーフロー チェックを`System.OverflowException`例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1634">If the left operand is the maximum negative `SByte`, `Short`, `Integer`, or `Long`, and the right operand is `-1`, an overflow occurs; if integer overflow checking is on, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="4e5da-1635">それ以外の場合、オーバーフローが報告されないと、結果は、左側のオペランドの値では代わりにします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1635">Otherwise, the overflow is not reported and the result is instead the value of the left operand.</span></span>

<span data-ttu-id="4e5da-1636">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1636">__Note.__</span></span> <span data-ttu-id="4e5da-1637">符号なしの型の 2 つのオペランドは常に 0 または正の場合、結果は常に 0 または正の値。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1637">As the two operands for unsigned types will always be zero or positive, the result is always zero or positive.</span></span>  <span data-ttu-id="4e5da-1638">式の結果では、以下に、最大 2 つのオペランドには常のオーバーフローが発生することはできません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1638">As the result of the expression will always be less than or equal to the largest of the two operands, it is not possible for an overflow to occur.</span></span>  <span data-ttu-id="4e5da-1639">そのための 2 つの符号なし整数値で整数除算整数オーバーフローのチェックは実行されません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1639">As such integer overflow checking is not performed for integer divide with two unsigned integers.</span></span> <span data-ttu-id="4e5da-1640">左側のオペランドの型になります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1640">The result is the type as that of the left operand.</span></span>


<span data-ttu-id="4e5da-1641">__操作の種類:__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1641">__Operation Type:__</span></span>

|        | <span data-ttu-id="4e5da-1642">__bo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1642">__Bo__</span></span> | <span data-ttu-id="4e5da-1643">__SB__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1643">__SB__</span></span> | <span data-ttu-id="4e5da-1644">__によって__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1644">__By__</span></span> | <span data-ttu-id="4e5da-1645">__Sh__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1645">__Sh__</span></span> | <span data-ttu-id="4e5da-1646">__米国__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1646">__US__</span></span> | <span data-ttu-id="4e5da-1647">__In__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1647">__In__</span></span> | <span data-ttu-id="4e5da-1648">__UI__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1648">__UI__</span></span> | <span data-ttu-id="4e5da-1649">__Lo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1649">__Lo__</span></span> | <span data-ttu-id="4e5da-1650">__UL__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1650">__UL__</span></span> | <span data-ttu-id="4e5da-1651">__de__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1651">__De__</span></span> | <span data-ttu-id="4e5da-1652">__si__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1652">__Si__</span></span> | <span data-ttu-id="4e5da-1653">__Do__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1653">__Do__</span></span> | <span data-ttu-id="4e5da-1654">__Da__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1654">__Da__</span></span>  | <span data-ttu-id="4e5da-1655">__ch__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1655">__Ch__</span></span>  | <span data-ttu-id="4e5da-1656">__st__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1656">__St__</span></span> | <span data-ttu-id="4e5da-1657">__Ob__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1657">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="4e5da-1658">__bo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1658">__Bo__</span></span> | <span data-ttu-id="4e5da-1659">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-1659">Sh</span></span> | <span data-ttu-id="4e5da-1660">SB</span><span class="sxs-lookup"><span data-stu-id="4e5da-1660">SB</span></span> | <span data-ttu-id="4e5da-1661">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-1661">Sh</span></span> | <span data-ttu-id="4e5da-1662">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-1662">Sh</span></span> | <span data-ttu-id="4e5da-1663">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1663">In</span></span> | <span data-ttu-id="4e5da-1664">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1664">In</span></span> | <span data-ttu-id="4e5da-1665">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1665">Lo</span></span> | <span data-ttu-id="4e5da-1666">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1666">Lo</span></span> | <span data-ttu-id="4e5da-1667">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1667">Lo</span></span> | <span data-ttu-id="4e5da-1668">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1668">Lo</span></span> | <span data-ttu-id="4e5da-1669">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1669">Lo</span></span> | <span data-ttu-id="4e5da-1670">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1670">Lo</span></span> | <span data-ttu-id="4e5da-1671">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1671">Err</span></span> | <span data-ttu-id="4e5da-1672">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1672">Err</span></span> | <span data-ttu-id="4e5da-1673">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1673">Lo</span></span>  | <span data-ttu-id="4e5da-1674">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1674">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1675">__SB__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1675">__SB__</span></span> |    | <span data-ttu-id="4e5da-1676">SB</span><span class="sxs-lookup"><span data-stu-id="4e5da-1676">SB</span></span> | <span data-ttu-id="4e5da-1677">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-1677">Sh</span></span> | <span data-ttu-id="4e5da-1678">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-1678">Sh</span></span> | <span data-ttu-id="4e5da-1679">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1679">In</span></span> | <span data-ttu-id="4e5da-1680">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1680">In</span></span> | <span data-ttu-id="4e5da-1681">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1681">Lo</span></span> | <span data-ttu-id="4e5da-1682">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1682">Lo</span></span> | <span data-ttu-id="4e5da-1683">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1683">Lo</span></span> | <span data-ttu-id="4e5da-1684">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1684">Lo</span></span> | <span data-ttu-id="4e5da-1685">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1685">Lo</span></span> | <span data-ttu-id="4e5da-1686">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1686">Lo</span></span> | <span data-ttu-id="4e5da-1687">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1687">Err</span></span> | <span data-ttu-id="4e5da-1688">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1688">Err</span></span> | <span data-ttu-id="4e5da-1689">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1689">Lo</span></span>  | <span data-ttu-id="4e5da-1690">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1690">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1691">__によって__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1691">__By__</span></span> |    |    | <span data-ttu-id="4e5da-1692">By</span><span class="sxs-lookup"><span data-stu-id="4e5da-1692">By</span></span> | <span data-ttu-id="4e5da-1693">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-1693">Sh</span></span> | <span data-ttu-id="4e5da-1694">US</span><span class="sxs-lookup"><span data-stu-id="4e5da-1694">US</span></span> | <span data-ttu-id="4e5da-1695">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1695">In</span></span> | <span data-ttu-id="4e5da-1696">UI</span><span class="sxs-lookup"><span data-stu-id="4e5da-1696">UI</span></span> | <span data-ttu-id="4e5da-1697">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1697">Lo</span></span> | <span data-ttu-id="4e5da-1698">UL</span><span class="sxs-lookup"><span data-stu-id="4e5da-1698">UL</span></span> | <span data-ttu-id="4e5da-1699">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1699">Lo</span></span> | <span data-ttu-id="4e5da-1700">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1700">Lo</span></span> | <span data-ttu-id="4e5da-1701">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1701">Lo</span></span> | <span data-ttu-id="4e5da-1702">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1702">Err</span></span> | <span data-ttu-id="4e5da-1703">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1703">Err</span></span> | <span data-ttu-id="4e5da-1704">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1704">Lo</span></span>  | <span data-ttu-id="4e5da-1705">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1705">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1706">__Sh__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1706">__Sh__</span></span> |    |    |    | <span data-ttu-id="4e5da-1707">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-1707">Sh</span></span> | <span data-ttu-id="4e5da-1708">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1708">In</span></span> | <span data-ttu-id="4e5da-1709">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1709">In</span></span> | <span data-ttu-id="4e5da-1710">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1710">Lo</span></span> | <span data-ttu-id="4e5da-1711">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1711">Lo</span></span> | <span data-ttu-id="4e5da-1712">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1712">Lo</span></span> | <span data-ttu-id="4e5da-1713">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1713">Lo</span></span> | <span data-ttu-id="4e5da-1714">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1714">Lo</span></span> | <span data-ttu-id="4e5da-1715">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1715">Lo</span></span> | <span data-ttu-id="4e5da-1716">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1716">Err</span></span> | <span data-ttu-id="4e5da-1717">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1717">Err</span></span> | <span data-ttu-id="4e5da-1718">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1718">Lo</span></span>  | <span data-ttu-id="4e5da-1719">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1719">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1720">__米国__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1720">__US__</span></span> |    |    |    |    | <span data-ttu-id="4e5da-1721">US</span><span class="sxs-lookup"><span data-stu-id="4e5da-1721">US</span></span> | <span data-ttu-id="4e5da-1722">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1722">In</span></span> | <span data-ttu-id="4e5da-1723">UI</span><span class="sxs-lookup"><span data-stu-id="4e5da-1723">UI</span></span> | <span data-ttu-id="4e5da-1724">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1724">Lo</span></span> | <span data-ttu-id="4e5da-1725">UL</span><span class="sxs-lookup"><span data-stu-id="4e5da-1725">UL</span></span> | <span data-ttu-id="4e5da-1726">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1726">Lo</span></span> | <span data-ttu-id="4e5da-1727">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1727">Lo</span></span> | <span data-ttu-id="4e5da-1728">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1728">Lo</span></span> | <span data-ttu-id="4e5da-1729">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1729">Err</span></span> | <span data-ttu-id="4e5da-1730">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1730">Err</span></span> | <span data-ttu-id="4e5da-1731">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1731">Lo</span></span>  | <span data-ttu-id="4e5da-1732">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1732">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1733">__In__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1733">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="4e5da-1734">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1734">In</span></span> | <span data-ttu-id="4e5da-1735">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1735">Lo</span></span> | <span data-ttu-id="4e5da-1736">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1736">Lo</span></span> | <span data-ttu-id="4e5da-1737">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1737">Lo</span></span> | <span data-ttu-id="4e5da-1738">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1738">Lo</span></span> | <span data-ttu-id="4e5da-1739">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1739">Lo</span></span> | <span data-ttu-id="4e5da-1740">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1740">Lo</span></span> | <span data-ttu-id="4e5da-1741">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1741">Err</span></span> | <span data-ttu-id="4e5da-1742">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1742">Err</span></span> | <span data-ttu-id="4e5da-1743">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1743">Lo</span></span>  | <span data-ttu-id="4e5da-1744">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1744">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1745">__UI__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1745">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="4e5da-1746">UI</span><span class="sxs-lookup"><span data-stu-id="4e5da-1746">UI</span></span> | <span data-ttu-id="4e5da-1747">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1747">Lo</span></span> | <span data-ttu-id="4e5da-1748">UL</span><span class="sxs-lookup"><span data-stu-id="4e5da-1748">UL</span></span> | <span data-ttu-id="4e5da-1749">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1749">Lo</span></span> | <span data-ttu-id="4e5da-1750">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1750">Lo</span></span> | <span data-ttu-id="4e5da-1751">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1751">Lo</span></span> | <span data-ttu-id="4e5da-1752">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1752">Err</span></span> | <span data-ttu-id="4e5da-1753">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1753">Err</span></span> | <span data-ttu-id="4e5da-1754">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1754">Lo</span></span>  | <span data-ttu-id="4e5da-1755">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1755">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1756">__Lo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1756">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1757">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1757">Lo</span></span> | <span data-ttu-id="4e5da-1758">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1758">Lo</span></span> | <span data-ttu-id="4e5da-1759">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1759">Lo</span></span> | <span data-ttu-id="4e5da-1760">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1760">Lo</span></span> | <span data-ttu-id="4e5da-1761">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1761">Lo</span></span> | <span data-ttu-id="4e5da-1762">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1762">Err</span></span> | <span data-ttu-id="4e5da-1763">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1763">Err</span></span> | <span data-ttu-id="4e5da-1764">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1764">Lo</span></span>  | <span data-ttu-id="4e5da-1765">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1765">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1766">__UL__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1766">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1767">UL</span><span class="sxs-lookup"><span data-stu-id="4e5da-1767">UL</span></span> | <span data-ttu-id="4e5da-1768">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1768">Lo</span></span> | <span data-ttu-id="4e5da-1769">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1769">Lo</span></span> | <span data-ttu-id="4e5da-1770">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1770">Lo</span></span> | <span data-ttu-id="4e5da-1771">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1771">Err</span></span> | <span data-ttu-id="4e5da-1772">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1772">Err</span></span> | <span data-ttu-id="4e5da-1773">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1773">Lo</span></span>  | <span data-ttu-id="4e5da-1774">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1774">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1775">__de__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1775">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1776">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1776">Lo</span></span> | <span data-ttu-id="4e5da-1777">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1777">Lo</span></span> | <span data-ttu-id="4e5da-1778">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1778">Lo</span></span> | <span data-ttu-id="4e5da-1779">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1779">Err</span></span> | <span data-ttu-id="4e5da-1780">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1780">Err</span></span> | <span data-ttu-id="4e5da-1781">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1781">Lo</span></span>  | <span data-ttu-id="4e5da-1782">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1782">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1783">__si__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1783">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1784">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1784">Lo</span></span> | <span data-ttu-id="4e5da-1785">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1785">Lo</span></span> | <span data-ttu-id="4e5da-1786">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1786">Err</span></span> | <span data-ttu-id="4e5da-1787">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1787">Err</span></span> | <span data-ttu-id="4e5da-1788">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1788">Lo</span></span>  | <span data-ttu-id="4e5da-1789">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1789">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1790">__Do__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1790">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1791">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1791">Lo</span></span> | <span data-ttu-id="4e5da-1792">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1792">Err</span></span> | <span data-ttu-id="4e5da-1793">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1793">Err</span></span> | <span data-ttu-id="4e5da-1794">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1794">Lo</span></span>  | <span data-ttu-id="4e5da-1795">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1795">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1796">__Da__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1796">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1797">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1797">Err</span></span> | <span data-ttu-id="4e5da-1798">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1798">Err</span></span> | <span data-ttu-id="4e5da-1799">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1799">Err</span></span> | <span data-ttu-id="4e5da-1800">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1800">Err</span></span> | 
| <span data-ttu-id="4e5da-1801">__ch__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1801">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="4e5da-1802">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1802">Err</span></span> | <span data-ttu-id="4e5da-1803">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1803">Err</span></span> | <span data-ttu-id="4e5da-1804">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1804">Err</span></span> | 
| <span data-ttu-id="4e5da-1805">__st__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1805">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="4e5da-1806">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1806">Lo</span></span>  | <span data-ttu-id="4e5da-1807">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1807">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1808">__Ob__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1808">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="4e5da-1809">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1809">Ob</span></span>  | 


### <a name="mod-operator"></a><span data-ttu-id="4e5da-1810">Mod 演算子</span><span class="sxs-lookup"><span data-stu-id="4e5da-1810">Mod Operator</span></span>

<span data-ttu-id="4e5da-1811">`Mod` (剰余) 演算子には、2 つのオペランドの除算の剰余を計算します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1811">The `Mod` (modulo) operator computes the remainder of the division between two operands.</span></span>

```antlr
ModuloOperatorExpression
    : Expression 'Mod' LineTerminator? Expression
    ;
```

<span data-ttu-id="4e5da-1812">`Mod`演算子は、次の種類の定義します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1812">The `Mod` operator is defined for the following types:</span></span>

* <span data-ttu-id="4e5da-1813">`Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、`ULong`と`Long`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1813">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` and `Long`.</span></span> <span data-ttu-id="4e5da-1814">結果`x Mod y`によって値が生成される`x - (x \ y) * y`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1814">The result of `x Mod y` is the value produced by `x - (x \ y) * y`.</span></span> <span data-ttu-id="4e5da-1815">場合`y`0 の場合は、`System.DivideByZeroException`例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1815">If `y` is zero, a `System.DivideByZeroException` exception is thrown.</span></span> <span data-ttu-id="4e5da-1816">剰余演算子、オーバーフローを引き起こすことはありません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1816">The modulo operator never causes an overflow.</span></span>

* <span data-ttu-id="4e5da-1817">`Single` および `Double`。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1817">`Single` and `Double`.</span></span> <span data-ttu-id="4e5da-1818">残りの部分は、IEEE 754 の演算の規則に従って計算されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1818">The remainder is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="4e5da-1819">`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1819">`Decimal`.</span></span> <span data-ttu-id="4e5da-1820">右側のオペランドの値が 0 の場合、`System.DivideByZeroException`例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1820">If the value of the right operand is zero, a `System.DivideByZeroException` exception is thrown.</span></span> <span data-ttu-id="4e5da-1821">10 進数の形式で表すには、結果の値が大きすぎる場合、`System.OverflowException`例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1821">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="4e5da-1822">結果の値が 10 進数形式で表す小さすぎる場合、結果は 0 です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1822">If the result value is too small to represent in the decimal format, the result is zero.</span></span>

<span data-ttu-id="4e5da-1823">__操作の種類:__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1823">__Operation Type:__</span></span>

|        | <span data-ttu-id="4e5da-1824">__bo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1824">__Bo__</span></span> | <span data-ttu-id="4e5da-1825">__SB__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1825">__SB__</span></span> | <span data-ttu-id="4e5da-1826">__によって__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1826">__By__</span></span> | <span data-ttu-id="4e5da-1827">__Sh__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1827">__Sh__</span></span> | <span data-ttu-id="4e5da-1828">__米国__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1828">__US__</span></span> | <span data-ttu-id="4e5da-1829">__In__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1829">__In__</span></span> | <span data-ttu-id="4e5da-1830">__UI__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1830">__UI__</span></span> | <span data-ttu-id="4e5da-1831">__Lo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1831">__Lo__</span></span> | <span data-ttu-id="4e5da-1832">__UL__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1832">__UL__</span></span> | <span data-ttu-id="4e5da-1833">__de__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1833">__De__</span></span> | <span data-ttu-id="4e5da-1834">__si__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1834">__Si__</span></span> | <span data-ttu-id="4e5da-1835">__Do__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1835">__Do__</span></span> | <span data-ttu-id="4e5da-1836">__Da__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1836">__Da__</span></span>  | <span data-ttu-id="4e5da-1837">__ch__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1837">__Ch__</span></span>  | <span data-ttu-id="4e5da-1838">__st__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1838">__St__</span></span> | <span data-ttu-id="4e5da-1839">__Ob__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1839">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="4e5da-1840">__bo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1840">__Bo__</span></span> | <span data-ttu-id="4e5da-1841">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-1841">Sh</span></span> | <span data-ttu-id="4e5da-1842">SB</span><span class="sxs-lookup"><span data-stu-id="4e5da-1842">SB</span></span> | <span data-ttu-id="4e5da-1843">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-1843">Sh</span></span> | <span data-ttu-id="4e5da-1844">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-1844">Sh</span></span> | <span data-ttu-id="4e5da-1845">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1845">In</span></span> | <span data-ttu-id="4e5da-1846">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1846">In</span></span> | <span data-ttu-id="4e5da-1847">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1847">Lo</span></span> | <span data-ttu-id="4e5da-1848">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1848">Lo</span></span> | <span data-ttu-id="4e5da-1849">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1849">De</span></span> | <span data-ttu-id="4e5da-1850">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1850">De</span></span> | <span data-ttu-id="4e5da-1851">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1851">Si</span></span> | <span data-ttu-id="4e5da-1852">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1852">Do</span></span> | <span data-ttu-id="4e5da-1853">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1853">Err</span></span> | <span data-ttu-id="4e5da-1854">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1854">Err</span></span> | <span data-ttu-id="4e5da-1855">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1855">Do</span></span>  | <span data-ttu-id="4e5da-1856">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1856">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1857">__SB__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1857">__SB__</span></span> |    | <span data-ttu-id="4e5da-1858">SB</span><span class="sxs-lookup"><span data-stu-id="4e5da-1858">SB</span></span> | <span data-ttu-id="4e5da-1859">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-1859">Sh</span></span> | <span data-ttu-id="4e5da-1860">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-1860">Sh</span></span> | <span data-ttu-id="4e5da-1861">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1861">In</span></span> | <span data-ttu-id="4e5da-1862">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1862">In</span></span> | <span data-ttu-id="4e5da-1863">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1863">Lo</span></span> | <span data-ttu-id="4e5da-1864">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1864">Lo</span></span> | <span data-ttu-id="4e5da-1865">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1865">De</span></span> | <span data-ttu-id="4e5da-1866">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1866">De</span></span> | <span data-ttu-id="4e5da-1867">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1867">Si</span></span> | <span data-ttu-id="4e5da-1868">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1868">Do</span></span> | <span data-ttu-id="4e5da-1869">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1869">Err</span></span> | <span data-ttu-id="4e5da-1870">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1870">Err</span></span> | <span data-ttu-id="4e5da-1871">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1871">Do</span></span>  | <span data-ttu-id="4e5da-1872">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1872">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1873">__によって__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1873">__By__</span></span> |    |    | <span data-ttu-id="4e5da-1874">By</span><span class="sxs-lookup"><span data-stu-id="4e5da-1874">By</span></span> | <span data-ttu-id="4e5da-1875">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-1875">Sh</span></span> | <span data-ttu-id="4e5da-1876">US</span><span class="sxs-lookup"><span data-stu-id="4e5da-1876">US</span></span> | <span data-ttu-id="4e5da-1877">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1877">In</span></span> | <span data-ttu-id="4e5da-1878">UI</span><span class="sxs-lookup"><span data-stu-id="4e5da-1878">UI</span></span> | <span data-ttu-id="4e5da-1879">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1879">Lo</span></span> | <span data-ttu-id="4e5da-1880">UL</span><span class="sxs-lookup"><span data-stu-id="4e5da-1880">UL</span></span> | <span data-ttu-id="4e5da-1881">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1881">De</span></span> | <span data-ttu-id="4e5da-1882">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1882">Si</span></span> | <span data-ttu-id="4e5da-1883">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1883">Do</span></span> | <span data-ttu-id="4e5da-1884">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1884">Err</span></span> | <span data-ttu-id="4e5da-1885">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1885">Err</span></span> | <span data-ttu-id="4e5da-1886">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1886">Do</span></span>  | <span data-ttu-id="4e5da-1887">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1887">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1888">__Sh__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1888">__Sh__</span></span> |    |    |    | <span data-ttu-id="4e5da-1889">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-1889">Sh</span></span> | <span data-ttu-id="4e5da-1890">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1890">In</span></span> | <span data-ttu-id="4e5da-1891">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1891">In</span></span> | <span data-ttu-id="4e5da-1892">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1892">Lo</span></span> | <span data-ttu-id="4e5da-1893">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1893">Lo</span></span> | <span data-ttu-id="4e5da-1894">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1894">De</span></span> | <span data-ttu-id="4e5da-1895">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1895">De</span></span> | <span data-ttu-id="4e5da-1896">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1896">Si</span></span> | <span data-ttu-id="4e5da-1897">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1897">Do</span></span> | <span data-ttu-id="4e5da-1898">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1898">Err</span></span> | <span data-ttu-id="4e5da-1899">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1899">Err</span></span> | <span data-ttu-id="4e5da-1900">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1900">Do</span></span>  | <span data-ttu-id="4e5da-1901">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1901">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1902">__米国__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1902">__US__</span></span> |    |    |    |    | <span data-ttu-id="4e5da-1903">US</span><span class="sxs-lookup"><span data-stu-id="4e5da-1903">US</span></span> | <span data-ttu-id="4e5da-1904">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1904">In</span></span> | <span data-ttu-id="4e5da-1905">UI</span><span class="sxs-lookup"><span data-stu-id="4e5da-1905">UI</span></span> | <span data-ttu-id="4e5da-1906">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1906">Lo</span></span> | <span data-ttu-id="4e5da-1907">UL</span><span class="sxs-lookup"><span data-stu-id="4e5da-1907">UL</span></span> | <span data-ttu-id="4e5da-1908">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1908">De</span></span> | <span data-ttu-id="4e5da-1909">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1909">Si</span></span> | <span data-ttu-id="4e5da-1910">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1910">Do</span></span> | <span data-ttu-id="4e5da-1911">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1911">Err</span></span> | <span data-ttu-id="4e5da-1912">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1912">Err</span></span> | <span data-ttu-id="4e5da-1913">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1913">Do</span></span>  | <span data-ttu-id="4e5da-1914">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1914">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1915">__In__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1915">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="4e5da-1916">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-1916">In</span></span> | <span data-ttu-id="4e5da-1917">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1917">Lo</span></span> | <span data-ttu-id="4e5da-1918">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1918">Lo</span></span> | <span data-ttu-id="4e5da-1919">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1919">De</span></span> | <span data-ttu-id="4e5da-1920">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1920">De</span></span> | <span data-ttu-id="4e5da-1921">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1921">Si</span></span> | <span data-ttu-id="4e5da-1922">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1922">Do</span></span> | <span data-ttu-id="4e5da-1923">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1923">Err</span></span> | <span data-ttu-id="4e5da-1924">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1924">Err</span></span> | <span data-ttu-id="4e5da-1925">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1925">Do</span></span>  | <span data-ttu-id="4e5da-1926">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1926">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1927">__UI__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1927">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="4e5da-1928">UI</span><span class="sxs-lookup"><span data-stu-id="4e5da-1928">UI</span></span> | <span data-ttu-id="4e5da-1929">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1929">Lo</span></span> | <span data-ttu-id="4e5da-1930">UL</span><span class="sxs-lookup"><span data-stu-id="4e5da-1930">UL</span></span> | <span data-ttu-id="4e5da-1931">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1931">De</span></span> | <span data-ttu-id="4e5da-1932">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1932">Si</span></span> | <span data-ttu-id="4e5da-1933">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1933">Do</span></span> | <span data-ttu-id="4e5da-1934">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1934">Err</span></span> | <span data-ttu-id="4e5da-1935">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1935">Err</span></span> | <span data-ttu-id="4e5da-1936">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1936">Do</span></span>  | <span data-ttu-id="4e5da-1937">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1937">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1938">__Lo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1938">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1939">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-1939">Lo</span></span> | <span data-ttu-id="4e5da-1940">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1940">De</span></span> | <span data-ttu-id="4e5da-1941">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1941">De</span></span> | <span data-ttu-id="4e5da-1942">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1942">Si</span></span> | <span data-ttu-id="4e5da-1943">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1943">Do</span></span> | <span data-ttu-id="4e5da-1944">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1944">Err</span></span> | <span data-ttu-id="4e5da-1945">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1945">Err</span></span> | <span data-ttu-id="4e5da-1946">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1946">Do</span></span>  | <span data-ttu-id="4e5da-1947">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1947">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1948">__UL__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1948">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1949">UL</span><span class="sxs-lookup"><span data-stu-id="4e5da-1949">UL</span></span> | <span data-ttu-id="4e5da-1950">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1950">De</span></span> | <span data-ttu-id="4e5da-1951">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1951">Si</span></span> | <span data-ttu-id="4e5da-1952">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1952">Do</span></span> | <span data-ttu-id="4e5da-1953">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1953">Err</span></span> | <span data-ttu-id="4e5da-1954">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1954">Err</span></span> | <span data-ttu-id="4e5da-1955">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1955">Do</span></span>  | <span data-ttu-id="4e5da-1956">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1956">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1957">__de__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1957">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1958">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-1958">De</span></span> | <span data-ttu-id="4e5da-1959">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1959">Si</span></span> | <span data-ttu-id="4e5da-1960">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1960">Do</span></span> | <span data-ttu-id="4e5da-1961">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1961">Err</span></span> | <span data-ttu-id="4e5da-1962">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1962">Err</span></span> | <span data-ttu-id="4e5da-1963">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1963">Do</span></span>  | <span data-ttu-id="4e5da-1964">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1964">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1965">__si__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1965">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1966">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-1966">Si</span></span> | <span data-ttu-id="4e5da-1967">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1967">Do</span></span> | <span data-ttu-id="4e5da-1968">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1968">Err</span></span> | <span data-ttu-id="4e5da-1969">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1969">Err</span></span> | <span data-ttu-id="4e5da-1970">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1970">Do</span></span>  | <span data-ttu-id="4e5da-1971">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1971">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1972">__Do__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1972">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1973">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1973">Do</span></span> | <span data-ttu-id="4e5da-1974">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1974">Err</span></span> | <span data-ttu-id="4e5da-1975">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1975">Err</span></span> | <span data-ttu-id="4e5da-1976">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1976">Do</span></span>  | <span data-ttu-id="4e5da-1977">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1977">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1978">__Da__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1978">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-1979">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1979">Err</span></span> | <span data-ttu-id="4e5da-1980">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1980">Err</span></span> | <span data-ttu-id="4e5da-1981">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1981">Err</span></span> | <span data-ttu-id="4e5da-1982">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1982">Err</span></span> | 
| <span data-ttu-id="4e5da-1983">__ch__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1983">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="4e5da-1984">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1984">Err</span></span> | <span data-ttu-id="4e5da-1985">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1985">Err</span></span> | <span data-ttu-id="4e5da-1986">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-1986">Err</span></span> | 
| <span data-ttu-id="4e5da-1987">__st__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1987">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="4e5da-1988">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-1988">Do</span></span>  | <span data-ttu-id="4e5da-1989">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1989">Ob</span></span>  | 
| <span data-ttu-id="4e5da-1990">__Ob__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1990">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="4e5da-1991">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-1991">Ob</span></span>  | 


### <a name="exponentiation-operator"></a><span data-ttu-id="4e5da-1992">指数演算子</span><span class="sxs-lookup"><span data-stu-id="4e5da-1992">Exponentiation Operator</span></span>

<span data-ttu-id="4e5da-1993">指数演算子は、最初のオペランドが 2 番目のオペランドの累乗を計算します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1993">The exponentiation operator computes the first operand raised to the power of the second operand.</span></span>

```antlr
ExponentOperatorExpression
    : Expression '^' LineTerminator? Expression
    ;
```

<span data-ttu-id="4e5da-1994">型の累乗演算子が定義されている`Double`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1994">The exponentiation operator is defined for type `Double`.</span></span> <span data-ttu-id="4e5da-1995">値は、IEEE 754 の演算の規則に従って計算されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-1995">The value is computed according to the rules of IEEE 754 arithmetic.</span></span>

<span data-ttu-id="4e5da-1996">__操作の種類:__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1996">__Operation Type:__</span></span>

|        | <span data-ttu-id="4e5da-1997">__bo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1997">__Bo__</span></span> | <span data-ttu-id="4e5da-1998">__SB__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1998">__SB__</span></span> | <span data-ttu-id="4e5da-1999">__によって__</span><span class="sxs-lookup"><span data-stu-id="4e5da-1999">__By__</span></span> | <span data-ttu-id="4e5da-2000">__Sh__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2000">__Sh__</span></span> | <span data-ttu-id="4e5da-2001">__米国__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2001">__US__</span></span> | <span data-ttu-id="4e5da-2002">__In__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2002">__In__</span></span> | <span data-ttu-id="4e5da-2003">__UI__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2003">__UI__</span></span> | <span data-ttu-id="4e5da-2004">__Lo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2004">__Lo__</span></span> | <span data-ttu-id="4e5da-2005">__UL__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2005">__UL__</span></span> | <span data-ttu-id="4e5da-2006">__de__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2006">__De__</span></span> | <span data-ttu-id="4e5da-2007">__si__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2007">__Si__</span></span> | <span data-ttu-id="4e5da-2008">__Do__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2008">__Do__</span></span> | <span data-ttu-id="4e5da-2009">__Da__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2009">__Da__</span></span>  | <span data-ttu-id="4e5da-2010">__ch__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2010">__Ch__</span></span>  | <span data-ttu-id="4e5da-2011">__st__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2011">__St__</span></span> | <span data-ttu-id="4e5da-2012">__Ob__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2012">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="4e5da-2013">__bo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2013">__Bo__</span></span> | <span data-ttu-id="4e5da-2014">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2014">Do</span></span> | <span data-ttu-id="4e5da-2015">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2015">Do</span></span> | <span data-ttu-id="4e5da-2016">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2016">Do</span></span> | <span data-ttu-id="4e5da-2017">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2017">Do</span></span> | <span data-ttu-id="4e5da-2018">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2018">Do</span></span> | <span data-ttu-id="4e5da-2019">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2019">Do</span></span> | <span data-ttu-id="4e5da-2020">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2020">Do</span></span> | <span data-ttu-id="4e5da-2021">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2021">Do</span></span> | <span data-ttu-id="4e5da-2022">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2022">Do</span></span> | <span data-ttu-id="4e5da-2023">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2023">Do</span></span> | <span data-ttu-id="4e5da-2024">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2024">Do</span></span> | <span data-ttu-id="4e5da-2025">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2025">Do</span></span> | <span data-ttu-id="4e5da-2026">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2026">Err</span></span> | <span data-ttu-id="4e5da-2027">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2027">Err</span></span> | <span data-ttu-id="4e5da-2028">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2028">Do</span></span>  | <span data-ttu-id="4e5da-2029">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2029">Ob</span></span>  | 
| <span data-ttu-id="4e5da-2030">__SB__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2030">__SB__</span></span> |    | <span data-ttu-id="4e5da-2031">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2031">Do</span></span> | <span data-ttu-id="4e5da-2032">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2032">Do</span></span> | <span data-ttu-id="4e5da-2033">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2033">Do</span></span> | <span data-ttu-id="4e5da-2034">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2034">Do</span></span> | <span data-ttu-id="4e5da-2035">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2035">Do</span></span> | <span data-ttu-id="4e5da-2036">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2036">Do</span></span> | <span data-ttu-id="4e5da-2037">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2037">Do</span></span> | <span data-ttu-id="4e5da-2038">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2038">Do</span></span> | <span data-ttu-id="4e5da-2039">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2039">Do</span></span> | <span data-ttu-id="4e5da-2040">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2040">Do</span></span> | <span data-ttu-id="4e5da-2041">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2041">Do</span></span> | <span data-ttu-id="4e5da-2042">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2042">Err</span></span> | <span data-ttu-id="4e5da-2043">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2043">Err</span></span> | <span data-ttu-id="4e5da-2044">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2044">Do</span></span>  | <span data-ttu-id="4e5da-2045">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2045">Ob</span></span>  | 
| <span data-ttu-id="4e5da-2046">__によって__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2046">__By__</span></span> |    |    | <span data-ttu-id="4e5da-2047">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2047">Do</span></span> | <span data-ttu-id="4e5da-2048">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2048">Do</span></span> | <span data-ttu-id="4e5da-2049">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2049">Do</span></span> | <span data-ttu-id="4e5da-2050">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2050">Do</span></span> | <span data-ttu-id="4e5da-2051">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2051">Do</span></span> | <span data-ttu-id="4e5da-2052">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2052">Do</span></span> | <span data-ttu-id="4e5da-2053">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2053">Do</span></span> | <span data-ttu-id="4e5da-2054">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2054">Do</span></span> | <span data-ttu-id="4e5da-2055">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2055">Do</span></span> | <span data-ttu-id="4e5da-2056">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2056">Do</span></span> | <span data-ttu-id="4e5da-2057">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2057">Err</span></span> | <span data-ttu-id="4e5da-2058">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2058">Err</span></span> | <span data-ttu-id="4e5da-2059">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2059">Do</span></span>  | <span data-ttu-id="4e5da-2060">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2060">Ob</span></span>  | 
| <span data-ttu-id="4e5da-2061">__Sh__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2061">__Sh__</span></span> |    |    |    | <span data-ttu-id="4e5da-2062">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2062">Do</span></span> | <span data-ttu-id="4e5da-2063">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2063">Do</span></span> | <span data-ttu-id="4e5da-2064">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2064">Do</span></span> | <span data-ttu-id="4e5da-2065">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2065">Do</span></span> | <span data-ttu-id="4e5da-2066">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2066">Do</span></span> | <span data-ttu-id="4e5da-2067">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2067">Do</span></span> | <span data-ttu-id="4e5da-2068">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2068">Do</span></span> | <span data-ttu-id="4e5da-2069">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2069">Do</span></span> | <span data-ttu-id="4e5da-2070">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2070">Do</span></span> | <span data-ttu-id="4e5da-2071">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2071">Err</span></span> | <span data-ttu-id="4e5da-2072">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2072">Err</span></span> | <span data-ttu-id="4e5da-2073">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2073">Do</span></span>  | <span data-ttu-id="4e5da-2074">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2074">Ob</span></span>  | 
| <span data-ttu-id="4e5da-2075">__米国__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2075">__US__</span></span> |    |    |    |    | <span data-ttu-id="4e5da-2076">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2076">Do</span></span> | <span data-ttu-id="4e5da-2077">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2077">Do</span></span> | <span data-ttu-id="4e5da-2078">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2078">Do</span></span> | <span data-ttu-id="4e5da-2079">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2079">Do</span></span> | <span data-ttu-id="4e5da-2080">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2080">Do</span></span> | <span data-ttu-id="4e5da-2081">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2081">Do</span></span> | <span data-ttu-id="4e5da-2082">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2082">Do</span></span> | <span data-ttu-id="4e5da-2083">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2083">Do</span></span> | <span data-ttu-id="4e5da-2084">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2084">Err</span></span> | <span data-ttu-id="4e5da-2085">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2085">Err</span></span> | <span data-ttu-id="4e5da-2086">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2086">Do</span></span>  | <span data-ttu-id="4e5da-2087">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2087">Ob</span></span>  | 
| <span data-ttu-id="4e5da-2088">__In__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2088">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="4e5da-2089">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2089">Do</span></span> | <span data-ttu-id="4e5da-2090">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2090">Do</span></span> | <span data-ttu-id="4e5da-2091">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2091">Do</span></span> | <span data-ttu-id="4e5da-2092">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2092">Do</span></span> | <span data-ttu-id="4e5da-2093">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2093">Do</span></span> | <span data-ttu-id="4e5da-2094">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2094">Do</span></span> | <span data-ttu-id="4e5da-2095">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2095">Do</span></span> | <span data-ttu-id="4e5da-2096">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2096">Err</span></span> | <span data-ttu-id="4e5da-2097">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2097">Err</span></span> | <span data-ttu-id="4e5da-2098">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2098">Do</span></span>  | <span data-ttu-id="4e5da-2099">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2099">Ob</span></span>  | 
| <span data-ttu-id="4e5da-2100">__UI__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2100">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="4e5da-2101">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2101">Do</span></span> | <span data-ttu-id="4e5da-2102">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2102">Do</span></span> | <span data-ttu-id="4e5da-2103">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2103">Do</span></span> | <span data-ttu-id="4e5da-2104">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2104">Do</span></span> | <span data-ttu-id="4e5da-2105">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2105">Do</span></span> | <span data-ttu-id="4e5da-2106">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2106">Do</span></span> | <span data-ttu-id="4e5da-2107">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2107">Err</span></span> | <span data-ttu-id="4e5da-2108">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2108">Err</span></span> | <span data-ttu-id="4e5da-2109">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2109">Do</span></span>  | <span data-ttu-id="4e5da-2110">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2110">Ob</span></span>  | 
| <span data-ttu-id="4e5da-2111">__Lo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2111">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2112">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2112">Do</span></span> | <span data-ttu-id="4e5da-2113">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2113">Do</span></span> | <span data-ttu-id="4e5da-2114">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2114">Do</span></span> | <span data-ttu-id="4e5da-2115">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2115">Do</span></span> | <span data-ttu-id="4e5da-2116">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2116">Do</span></span> | <span data-ttu-id="4e5da-2117">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2117">Err</span></span> | <span data-ttu-id="4e5da-2118">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2118">Err</span></span> | <span data-ttu-id="4e5da-2119">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2119">Do</span></span>  | <span data-ttu-id="4e5da-2120">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2120">Ob</span></span>  | 
| <span data-ttu-id="4e5da-2121">__UL__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2121">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2122">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2122">Do</span></span> | <span data-ttu-id="4e5da-2123">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2123">Do</span></span> | <span data-ttu-id="4e5da-2124">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2124">Do</span></span> | <span data-ttu-id="4e5da-2125">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2125">Do</span></span> | <span data-ttu-id="4e5da-2126">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2126">Err</span></span> | <span data-ttu-id="4e5da-2127">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2127">Err</span></span> | <span data-ttu-id="4e5da-2128">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2128">Do</span></span>  | <span data-ttu-id="4e5da-2129">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2129">Ob</span></span>  | 
| <span data-ttu-id="4e5da-2130">__de__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2130">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2131">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2131">Do</span></span> | <span data-ttu-id="4e5da-2132">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2132">Do</span></span> | <span data-ttu-id="4e5da-2133">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2133">Do</span></span> | <span data-ttu-id="4e5da-2134">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2134">Err</span></span> | <span data-ttu-id="4e5da-2135">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2135">Err</span></span> | <span data-ttu-id="4e5da-2136">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2136">Do</span></span>  | <span data-ttu-id="4e5da-2137">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2137">Ob</span></span>  | 
| <span data-ttu-id="4e5da-2138">__si__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2138">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2139">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2139">Do</span></span> | <span data-ttu-id="4e5da-2140">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2140">Do</span></span> | <span data-ttu-id="4e5da-2141">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2141">Err</span></span> | <span data-ttu-id="4e5da-2142">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2142">Err</span></span> | <span data-ttu-id="4e5da-2143">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2143">Do</span></span>  | <span data-ttu-id="4e5da-2144">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2144">Ob</span></span>  | 
| <span data-ttu-id="4e5da-2145">__Do__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2145">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2146">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2146">Do</span></span> | <span data-ttu-id="4e5da-2147">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2147">Err</span></span> | <span data-ttu-id="4e5da-2148">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2148">Err</span></span> | <span data-ttu-id="4e5da-2149">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2149">Do</span></span>  | <span data-ttu-id="4e5da-2150">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2150">Ob</span></span>  | 
| <span data-ttu-id="4e5da-2151">__Da__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2151">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2152">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2152">Err</span></span> | <span data-ttu-id="4e5da-2153">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2153">Err</span></span> | <span data-ttu-id="4e5da-2154">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2154">Err</span></span> | <span data-ttu-id="4e5da-2155">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2155">Err</span></span> | 
| <span data-ttu-id="4e5da-2156">__ch__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2156">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="4e5da-2157">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2157">Err</span></span> | <span data-ttu-id="4e5da-2158">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2158">Err</span></span> | <span data-ttu-id="4e5da-2159">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2159">Err</span></span> | 
| <span data-ttu-id="4e5da-2160">__st__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2160">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="4e5da-2161">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2161">Do</span></span>  | <span data-ttu-id="4e5da-2162">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2162">Ob</span></span>  | 
| <span data-ttu-id="4e5da-2163">__Ob__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2163">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="4e5da-2164">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2164">Ob</span></span>  | 


## <a name="relational-operators"></a><span data-ttu-id="4e5da-2165">関係演算子</span><span class="sxs-lookup"><span data-stu-id="4e5da-2165">Relational Operators</span></span>

<span data-ttu-id="4e5da-2166">*関係演算子*他の 2 つの値を比較します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2166">The *relational operators* compare values to one other.</span></span> <span data-ttu-id="4e5da-2167">比較演算子は`=`、 `<>`、 `<`、 `>`、 `<=`、および`>=`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2167">The comparison operators are `=`, `<>`, `<`, `>`, `<=`, and `>=`.</span></span>

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

<span data-ttu-id="4e5da-2168">関係演算子のすべてになります、`Boolean`値。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2168">All of the relational operators result in a `Boolean` value.</span></span>

<span data-ttu-id="4e5da-2169">関係演算子では、次の一般的な意味があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2169">The relational operators have the following general meaning:</span></span>

* <span data-ttu-id="4e5da-2170">`=`演算子は 2 つのオペランドが等しいかどうかをテストします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2170">The `=` operator tests whether the two operands are equal.</span></span>

* <span data-ttu-id="4e5da-2171">`<>`演算子は 2 つのオペランドが等しくないかどうかをテストします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2171">The `<>` operator tests whether the two operands are not equal.</span></span>

* <span data-ttu-id="4e5da-2172">`<`演算子は、最初のオペランドが小さいかどうかをテストします。 2 番目のオペランドよりもします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2172">The `<` operator tests whether the first operand is less than the second operand.</span></span>

* <span data-ttu-id="4e5da-2173">`>`演算子は最初のオペランドが 2 番目のオペランドより大きいかどうかをテストします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2173">The `>` operator tests whether the first operand is greater than the second operand.</span></span>

* <span data-ttu-id="4e5da-2174">`<=`演算子は最初のオペランドが 2 番目のオペランド以下かどうかをテストします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2174">The `<=` operator tests whether the first operand is less than or equal to the second operand.</span></span>

* <span data-ttu-id="4e5da-2175">`>=`演算子は最初のオペランドが 2 番目のオペランド以上かどうかをテストします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2175">The `>=` operator tests whether the first operand is greater than or equal to the second operand.</span></span>

<span data-ttu-id="4e5da-2176">次の種類は、関係演算子が定義されています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2176">The relational operators are defined for the following types:</span></span>

* <span data-ttu-id="4e5da-2177">`Boolean`。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2177">`Boolean`.</span></span> <span data-ttu-id="4e5da-2178">演算子は、2 つのオペランドの真の値を比較します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2178">The operators compare the truth values of the two operands.</span></span> <span data-ttu-id="4e5da-2179">`True` あると見なされますより小さい`False`、その数値の値と一致します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2179">`True` is considered to be less than `False`, which matches with their numeric values.</span></span>

* <span data-ttu-id="4e5da-2180">`Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、および `Long`。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2180">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="4e5da-2181">演算子は、2 つの整数オペランドの数値を比較します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2181">The operators compare the numeric values of the two integral operands.</span></span>

* <span data-ttu-id="4e5da-2182">`Single` および `Double`。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2182">`Single` and `Double`.</span></span> <span data-ttu-id="4e5da-2183">演算子は、IEEE 754 標準の規則に従ってオペランドを比較します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2183">The operators compare the operands according to the rules of the IEEE 754 standard.</span></span>

* <span data-ttu-id="4e5da-2184">`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2184">`Decimal`.</span></span> <span data-ttu-id="4e5da-2185">演算子は、2 つの 10 進数のオペランドの数値を比較します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2185">The operators compare the numeric values of the two decimal operands.</span></span>

* <span data-ttu-id="4e5da-2186">`Date`。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2186">`Date`.</span></span> <span data-ttu-id="4e5da-2187">演算子は、2 つの日付/時刻値を比較した結果を返します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2187">The operators return the result of comparing the two date/time values.</span></span>

* <span data-ttu-id="4e5da-2188">`Char`。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2188">`Char`.</span></span> <span data-ttu-id="4e5da-2189">演算子は、2 つの Unicode 値を比較した結果を返します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2189">The operators return the result of comparing the two Unicode values.</span></span>

* <span data-ttu-id="4e5da-2190">`String`。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2190">`String`.</span></span> <span data-ttu-id="4e5da-2191">演算子は、二項比較またはテキスト比較を使用して 2 つの値を比較した結果を返します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2191">The operators return the result of comparing the two values using either a binary comparison or a text comparison.</span></span> <span data-ttu-id="4e5da-2192">使用する比較は、コンパイル環境によって決定されます、`Option Compare`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2192">The comparison used is determined by the compilation environment and the `Option Compare` statement.</span></span> <span data-ttu-id="4e5da-2193">バイナリ比較では、各文字列内の各文字の Unicode 数値が同じかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2193">A binary comparison determines whether the numeric Unicode value of each character in each string is the same.</span></span> <span data-ttu-id="4e5da-2194">文字比較では、.NET Framework で使用中の現在のカルチャに基づいて、Unicode テキスト比較します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2194">A text comparison does a Unicode text comparison based on the current culture in use on the .NET Framework.</span></span> <span data-ttu-id="4e5da-2195">文字列比較を行うと、null 値は文字列リテラルと同じ`""`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2195">When doing a string comparison, a null value is equivalent to the string literal `""`.</span></span>

<span data-ttu-id="4e5da-2196">__操作の種類:__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2196">__Operation Type:__</span></span>
        
|        | <span data-ttu-id="4e5da-2197">__bo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2197">__Bo__</span></span> | <span data-ttu-id="4e5da-2198">__SB__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2198">__SB__</span></span> | <span data-ttu-id="4e5da-2199">__によって__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2199">__By__</span></span> | <span data-ttu-id="4e5da-2200">__Sh__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2200">__Sh__</span></span> | <span data-ttu-id="4e5da-2201">__米国__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2201">__US__</span></span> | <span data-ttu-id="4e5da-2202">__In__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2202">__In__</span></span> | <span data-ttu-id="4e5da-2203">__UI__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2203">__UI__</span></span> | <span data-ttu-id="4e5da-2204">__Lo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2204">__Lo__</span></span> | <span data-ttu-id="4e5da-2205">__UL__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2205">__UL__</span></span> | <span data-ttu-id="4e5da-2206">__de__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2206">__De__</span></span> | <span data-ttu-id="4e5da-2207">__si__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2207">__Si__</span></span> | <span data-ttu-id="4e5da-2208">__Do__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2208">__Do__</span></span> | <span data-ttu-id="4e5da-2209">__Da__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2209">__Da__</span></span>  | <span data-ttu-id="4e5da-2210">__ch__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2210">__Ch__</span></span>  | <span data-ttu-id="4e5da-2211">__st__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2211">__St__</span></span> | <span data-ttu-id="4e5da-2212">__Ob__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2212">__Ob__</span></span> | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="4e5da-2213">__bo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2213">__Bo__</span></span> | <span data-ttu-id="4e5da-2214">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2214">Bo</span></span> | <span data-ttu-id="4e5da-2215">SB</span><span class="sxs-lookup"><span data-stu-id="4e5da-2215">SB</span></span> | <span data-ttu-id="4e5da-2216">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-2216">Sh</span></span> | <span data-ttu-id="4e5da-2217">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-2217">Sh</span></span> | <span data-ttu-id="4e5da-2218">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-2218">In</span></span> | <span data-ttu-id="4e5da-2219">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-2219">In</span></span> | <span data-ttu-id="4e5da-2220">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2220">Lo</span></span> | <span data-ttu-id="4e5da-2221">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2221">Lo</span></span> | <span data-ttu-id="4e5da-2222">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-2222">De</span></span> | <span data-ttu-id="4e5da-2223">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-2223">De</span></span> | <span data-ttu-id="4e5da-2224">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-2224">Si</span></span> | <span data-ttu-id="4e5da-2225">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2225">Do</span></span> | <span data-ttu-id="4e5da-2226">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2226">Err</span></span> | <span data-ttu-id="4e5da-2227">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2227">Err</span></span> | <span data-ttu-id="4e5da-2228">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2228">Bo</span></span> | <span data-ttu-id="4e5da-2229">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2229">Ob</span></span> | 
| <span data-ttu-id="4e5da-2230">__SB__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2230">__SB__</span></span> |    | <span data-ttu-id="4e5da-2231">SB</span><span class="sxs-lookup"><span data-stu-id="4e5da-2231">SB</span></span> | <span data-ttu-id="4e5da-2232">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-2232">Sh</span></span> | <span data-ttu-id="4e5da-2233">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-2233">Sh</span></span> | <span data-ttu-id="4e5da-2234">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-2234">In</span></span> | <span data-ttu-id="4e5da-2235">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-2235">In</span></span> | <span data-ttu-id="4e5da-2236">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2236">Lo</span></span> | <span data-ttu-id="4e5da-2237">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2237">Lo</span></span> | <span data-ttu-id="4e5da-2238">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-2238">De</span></span> | <span data-ttu-id="4e5da-2239">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-2239">De</span></span> | <span data-ttu-id="4e5da-2240">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-2240">Si</span></span> | <span data-ttu-id="4e5da-2241">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2241">Do</span></span> | <span data-ttu-id="4e5da-2242">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2242">Err</span></span> | <span data-ttu-id="4e5da-2243">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2243">Err</span></span> | <span data-ttu-id="4e5da-2244">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2244">Do</span></span> | <span data-ttu-id="4e5da-2245">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2245">Ob</span></span> | 
| <span data-ttu-id="4e5da-2246">__によって__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2246">__By__</span></span> |    |    | <span data-ttu-id="4e5da-2247">By</span><span class="sxs-lookup"><span data-stu-id="4e5da-2247">By</span></span> | <span data-ttu-id="4e5da-2248">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-2248">Sh</span></span> | <span data-ttu-id="4e5da-2249">US</span><span class="sxs-lookup"><span data-stu-id="4e5da-2249">US</span></span> | <span data-ttu-id="4e5da-2250">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-2250">In</span></span> | <span data-ttu-id="4e5da-2251">UI</span><span class="sxs-lookup"><span data-stu-id="4e5da-2251">UI</span></span> | <span data-ttu-id="4e5da-2252">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2252">Lo</span></span> | <span data-ttu-id="4e5da-2253">UL</span><span class="sxs-lookup"><span data-stu-id="4e5da-2253">UL</span></span> | <span data-ttu-id="4e5da-2254">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-2254">De</span></span> | <span data-ttu-id="4e5da-2255">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-2255">Si</span></span> | <span data-ttu-id="4e5da-2256">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2256">Do</span></span> | <span data-ttu-id="4e5da-2257">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2257">Err</span></span> | <span data-ttu-id="4e5da-2258">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2258">Err</span></span> | <span data-ttu-id="4e5da-2259">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2259">Do</span></span> | <span data-ttu-id="4e5da-2260">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2260">Ob</span></span> | 
| <span data-ttu-id="4e5da-2261">__Sh__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2261">__Sh__</span></span> |    |    |    | <span data-ttu-id="4e5da-2262">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-2262">Sh</span></span> | <span data-ttu-id="4e5da-2263">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-2263">In</span></span> | <span data-ttu-id="4e5da-2264">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-2264">In</span></span> | <span data-ttu-id="4e5da-2265">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2265">Lo</span></span> | <span data-ttu-id="4e5da-2266">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2266">Lo</span></span> | <span data-ttu-id="4e5da-2267">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-2267">De</span></span> | <span data-ttu-id="4e5da-2268">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-2268">De</span></span> | <span data-ttu-id="4e5da-2269">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-2269">Si</span></span> | <span data-ttu-id="4e5da-2270">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2270">Do</span></span> | <span data-ttu-id="4e5da-2271">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2271">Err</span></span> | <span data-ttu-id="4e5da-2272">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2272">Err</span></span> | <span data-ttu-id="4e5da-2273">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2273">Do</span></span> | <span data-ttu-id="4e5da-2274">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2274">Ob</span></span> | 
| <span data-ttu-id="4e5da-2275">__米国__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2275">__US__</span></span> |    |    |    |    | <span data-ttu-id="4e5da-2276">US</span><span class="sxs-lookup"><span data-stu-id="4e5da-2276">US</span></span> | <span data-ttu-id="4e5da-2277">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-2277">In</span></span> | <span data-ttu-id="4e5da-2278">UI</span><span class="sxs-lookup"><span data-stu-id="4e5da-2278">UI</span></span> | <span data-ttu-id="4e5da-2279">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2279">Lo</span></span> | <span data-ttu-id="4e5da-2280">UL</span><span class="sxs-lookup"><span data-stu-id="4e5da-2280">UL</span></span> | <span data-ttu-id="4e5da-2281">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-2281">De</span></span> | <span data-ttu-id="4e5da-2282">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-2282">Si</span></span> | <span data-ttu-id="4e5da-2283">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2283">Do</span></span> | <span data-ttu-id="4e5da-2284">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2284">Err</span></span> | <span data-ttu-id="4e5da-2285">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2285">Err</span></span> | <span data-ttu-id="4e5da-2286">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2286">Do</span></span> | <span data-ttu-id="4e5da-2287">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2287">Ob</span></span> | 
| <span data-ttu-id="4e5da-2288">__In__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2288">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="4e5da-2289">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-2289">In</span></span> | <span data-ttu-id="4e5da-2290">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2290">Lo</span></span> | <span data-ttu-id="4e5da-2291">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2291">Lo</span></span> | <span data-ttu-id="4e5da-2292">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-2292">De</span></span> | <span data-ttu-id="4e5da-2293">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-2293">De</span></span> | <span data-ttu-id="4e5da-2294">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-2294">Si</span></span> | <span data-ttu-id="4e5da-2295">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2295">Do</span></span> | <span data-ttu-id="4e5da-2296">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2296">Err</span></span> | <span data-ttu-id="4e5da-2297">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2297">Err</span></span> | <span data-ttu-id="4e5da-2298">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2298">Do</span></span> | <span data-ttu-id="4e5da-2299">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2299">Ob</span></span> | 
| <span data-ttu-id="4e5da-2300">__UI__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2300">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="4e5da-2301">UI</span><span class="sxs-lookup"><span data-stu-id="4e5da-2301">UI</span></span> | <span data-ttu-id="4e5da-2302">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2302">Lo</span></span> | <span data-ttu-id="4e5da-2303">UL</span><span class="sxs-lookup"><span data-stu-id="4e5da-2303">UL</span></span> | <span data-ttu-id="4e5da-2304">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-2304">De</span></span> | <span data-ttu-id="4e5da-2305">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-2305">Si</span></span> | <span data-ttu-id="4e5da-2306">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2306">Do</span></span> | <span data-ttu-id="4e5da-2307">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2307">Err</span></span> | <span data-ttu-id="4e5da-2308">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2308">Err</span></span> | <span data-ttu-id="4e5da-2309">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2309">Do</span></span> | <span data-ttu-id="4e5da-2310">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2310">Ob</span></span> | 
| <span data-ttu-id="4e5da-2311">__Lo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2311">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2312">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2312">Lo</span></span> | <span data-ttu-id="4e5da-2313">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-2313">De</span></span> | <span data-ttu-id="4e5da-2314">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-2314">De</span></span> | <span data-ttu-id="4e5da-2315">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-2315">Si</span></span> | <span data-ttu-id="4e5da-2316">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2316">Do</span></span> | <span data-ttu-id="4e5da-2317">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2317">Err</span></span> | <span data-ttu-id="4e5da-2318">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2318">Err</span></span> | <span data-ttu-id="4e5da-2319">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2319">Do</span></span> | <span data-ttu-id="4e5da-2320">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2320">Ob</span></span> | 
| <span data-ttu-id="4e5da-2321">__UL__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2321">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2322">UL</span><span class="sxs-lookup"><span data-stu-id="4e5da-2322">UL</span></span> | <span data-ttu-id="4e5da-2323">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-2323">De</span></span> | <span data-ttu-id="4e5da-2324">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-2324">Si</span></span> | <span data-ttu-id="4e5da-2325">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2325">Do</span></span> | <span data-ttu-id="4e5da-2326">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2326">Err</span></span> | <span data-ttu-id="4e5da-2327">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2327">Err</span></span> | <span data-ttu-id="4e5da-2328">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2328">Do</span></span> | <span data-ttu-id="4e5da-2329">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2329">Ob</span></span> | 
| <span data-ttu-id="4e5da-2330">__de__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2330">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2331">De</span><span class="sxs-lookup"><span data-stu-id="4e5da-2331">De</span></span> | <span data-ttu-id="4e5da-2332">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-2332">Si</span></span> | <span data-ttu-id="4e5da-2333">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2333">Do</span></span> | <span data-ttu-id="4e5da-2334">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2334">Err</span></span> | <span data-ttu-id="4e5da-2335">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2335">Err</span></span> | <span data-ttu-id="4e5da-2336">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2336">Do</span></span> | <span data-ttu-id="4e5da-2337">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2337">Ob</span></span> | 
| <span data-ttu-id="4e5da-2338">__si__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2338">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2339">Si</span><span class="sxs-lookup"><span data-stu-id="4e5da-2339">Si</span></span> | <span data-ttu-id="4e5da-2340">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2340">Do</span></span> | <span data-ttu-id="4e5da-2341">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2341">Err</span></span> | <span data-ttu-id="4e5da-2342">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2342">Err</span></span> | <span data-ttu-id="4e5da-2343">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2343">Do</span></span> | <span data-ttu-id="4e5da-2344">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2344">Ob</span></span> | 
| <span data-ttu-id="4e5da-2345">__Do__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2345">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2346">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2346">Do</span></span> | <span data-ttu-id="4e5da-2347">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2347">Err</span></span> | <span data-ttu-id="4e5da-2348">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2348">Err</span></span> | <span data-ttu-id="4e5da-2349">Do</span><span class="sxs-lookup"><span data-stu-id="4e5da-2349">Do</span></span> | <span data-ttu-id="4e5da-2350">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2350">Ob</span></span> | 
| <span data-ttu-id="4e5da-2351">__Da__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2351">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2352">Da</span><span class="sxs-lookup"><span data-stu-id="4e5da-2352">Da</span></span>  | <span data-ttu-id="4e5da-2353">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2353">Err</span></span> | <span data-ttu-id="4e5da-2354">Da</span><span class="sxs-lookup"><span data-stu-id="4e5da-2354">Da</span></span> | <span data-ttu-id="4e5da-2355">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2355">Ob</span></span> | 
| <span data-ttu-id="4e5da-2356">__ch__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2356">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="4e5da-2357">ch</span><span class="sxs-lookup"><span data-stu-id="4e5da-2357">Ch</span></span>  | <span data-ttu-id="4e5da-2358">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2358">St</span></span> | <span data-ttu-id="4e5da-2359">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2359">Ob</span></span> | 
| <span data-ttu-id="4e5da-2360">__st__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2360">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="4e5da-2361">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2361">St</span></span> | <span data-ttu-id="4e5da-2362">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2362">Ob</span></span> | 
| <span data-ttu-id="4e5da-2363">__Ob__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2363">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |    | <span data-ttu-id="4e5da-2364">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2364">Ob</span></span> | 


## <a name="like-operator"></a><span data-ttu-id="4e5da-2365">Like 演算子</span><span class="sxs-lookup"><span data-stu-id="4e5da-2365">Like Operator</span></span>

<span data-ttu-id="4e5da-2366">`Like`演算子が文字列に指定されたパターンが一致するかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2366">The `Like` operator determines whether a string matches a given pattern.</span></span>

```antlr
LikeOperatorExpression
    : Expression 'Like' LineTerminator? Expression
    ;
```

<span data-ttu-id="4e5da-2367">`Like`演算子が定義されて、`String`型。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2367">The `Like` operator is defined for the `String` type.</span></span> <span data-ttu-id="4e5da-2368">最初のオペランドが一致する文字列と 2 番目のオペランドは照合するパターンです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2368">The first operand is the string being matched, and the second operand is the pattern to match against.</span></span> <span data-ttu-id="4e5da-2369">パターンは Unicode 文字で構成をされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2369">The pattern is made up of Unicode characters.</span></span> <span data-ttu-id="4e5da-2370">次の文字シーケンスは、特別な意味を持ちます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2370">The following character sequences have special meanings:</span></span>

* <span data-ttu-id="4e5da-2371">文字`?`任意の 1 文字と一致します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2371">The character `?` matches any single character.</span></span>

* <span data-ttu-id="4e5da-2372">文字`*`0 個以上の文字と一致します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2372">The character `*` matches zero or more characters.</span></span>

* <span data-ttu-id="4e5da-2373">文字`#`1 つの数字 (0 ~ 9) と一致します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2373">The character `#` matches any single digit (0-9).</span></span>

* <span data-ttu-id="4e5da-2374">角かっこで囲まれた文字の一覧 (`[ab...]`) の一覧で任意の 1 文字と一致します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2374">A list of characters surrounded by brackets (`[ab...]`) matches any single character in the list.</span></span>

* <span data-ttu-id="4e5da-2375">文字の一覧が角かっこで囲まれているし、感嘆符が付いて (`[!ab...]`) 文字の一覧にない任意の 1 文字と一致します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2375">A list of characters surrounded by brackets and prefixed by an exclamation point (`[!ab...]`) matches any single character not in the character list.</span></span>

* <span data-ttu-id="4e5da-2376">ハイフンで区切られた一連の文字の 2 文字 (`-`) の範囲の Unicode 文字の最初の文字で始まり、2 番目の文字で終わるを指定します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2376">Two characters in a character list separated by a hyphen (`-`) specify a range of Unicode characters starting with the first character and ending with the second character.</span></span> <span data-ttu-id="4e5da-2377">2 番目の文字でない場合、最初の文字よりも並べ替え順序で後で、実行時例外が発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2377">If the second character is not later in the sort order than the first character, a run-time exception occurs.</span></span> <span data-ttu-id="4e5da-2378">先頭または末尾の文字の一覧に表示されるハイフンでは、それ自体を指定します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2378">A hyphen that appears at the beginning or end of a character list specifies itself.</span></span>

<span data-ttu-id="4e5da-2379">特殊文字の左角かっこの一致するように (`[`)、疑問符 (`?`)、シャープ記号 (`#`)、およびアスタリスク (`*`)、角かっこが囲む必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2379">To match the special characters left bracket (`[`), question mark (`?`), number sign (`#`), and asterisk (`*`), brackets must enclose them.</span></span> <span data-ttu-id="4e5da-2380">右の角かっこ (`]`) 自体には、一致するように、グループ内で使用できませんが、グループ外で個別の文字として使用することができます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2380">The right bracket (`]`) cannot be used within a group to match itself, but it can be used outside a group as an individual character.</span></span> <span data-ttu-id="4e5da-2381">文字シーケンス`[]`文字列リテラルと見なされます`""`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2381">The character sequence `[]` is considered to be the string literal `""`.</span></span> 

<span data-ttu-id="4e5da-2382">文字の比較と、文字リストの順序付けに使用する比較の種類に依存していることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2382">Note that character comparisons and ordering for character lists are dependent on the type of comparisons being used.</span></span> <span data-ttu-id="4e5da-2383">バイナリの比較を使用している場合文字の比較と順序付けが数値の Unicode 値に対するベースします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2383">If binary comparisons are being used, character comparisons and ordering are based on the numeric Unicode values.</span></span> <span data-ttu-id="4e5da-2384">テキスト比較を使用している場合、.NET Framework で使用されている現在のロケールの文字の比較と順序付けがベースします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2384">If text comparisons are being used, character comparisons and ordering are based on the current locale being used on the .NET Framework.</span></span>

<span data-ttu-id="4e5da-2385">言語によっては、特殊文字、アルファベット表す 2 つの文字、またはその逆です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2385">In some languages, special characters in the alphabet represent two separate characters and vice versa.</span></span> <span data-ttu-id="4e5da-2386">たとえば、複数の言語が文字を使用して`æ`の文字を表す`a`と`e`表示された場合に、文字の中に`^`と`O`の文字を表すために使用できます`Ô`.</span><span class="sxs-lookup"><span data-stu-id="4e5da-2386">For example, several languages use the character `æ` to represent the characters `a` and `e` when they appear together, while the characters `^` and `O` can be used to represent the character `Ô`.</span></span> <span data-ttu-id="4e5da-2387">テキスト比較を行うときに、`Like`演算子は、このような同等のカルチャを認識します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2387">When using text comparisons, the `Like` operator recognizes such cultural equivalences.</span></span> <span data-ttu-id="4e5da-2388">その場合は、パターンまたは文字列のいずれかで 1 つの特殊文字の発生は、その他の文字列で同等の 2 文字のシーケンスと一致します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2388">In that case, an occurrence of the single special character in either pattern or string matches the equivalent two-character sequence in the other string.</span></span> <span data-ttu-id="4e5da-2389">同様に、1 つの特殊文字角かっこで囲まれたパターンと一致と等価の 2 文字のシーケンスを文字列、またはその逆を (単独で、リスト、または範囲内)。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2389">Similarly, a single special character in pattern enclosed in brackets (by itself, in a list, or in a range) matches the equivalent two-character sequence in the string and vice versa.</span></span>

<span data-ttu-id="4e5da-2390">`Like`式の両方のオペランドが`Nothing`1 つのオペランドがへの組み込みの変換または`String`し、もう一方のオペランドが`Nothing`、`Nothing`は、空の文字列リテラルの場合と同様に扱われます`""`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2390">In a `Like` expression where both operands are `Nothing` or one operand has an intrinsic conversion to `String` and the other operand is `Nothing`, `Nothing` is treated as if it were the empty string literal `""`.</span></span>

<span data-ttu-id="4e5da-2391">__操作の種類:__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2391">__Operation Type:__</span></span>

|        | <span data-ttu-id="4e5da-2392">__bo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2392">__Bo__</span></span> | <span data-ttu-id="4e5da-2393">__SB__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2393">__SB__</span></span> | <span data-ttu-id="4e5da-2394">__によって__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2394">__By__</span></span> | <span data-ttu-id="4e5da-2395">__Sh__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2395">__Sh__</span></span> | <span data-ttu-id="4e5da-2396">__米国__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2396">__US__</span></span> | <span data-ttu-id="4e5da-2397">__In__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2397">__In__</span></span> | <span data-ttu-id="4e5da-2398">__UI__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2398">__UI__</span></span> | <span data-ttu-id="4e5da-2399">__Lo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2399">__Lo__</span></span> | <span data-ttu-id="4e5da-2400">__UL__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2400">__UL__</span></span> | <span data-ttu-id="4e5da-2401">__de__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2401">__De__</span></span> | <span data-ttu-id="4e5da-2402">__si__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2402">__Si__</span></span> | <span data-ttu-id="4e5da-2403">__Do__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2403">__Do__</span></span> | <span data-ttu-id="4e5da-2404">__Da__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2404">__Da__</span></span>  | <span data-ttu-id="4e5da-2405">__ch__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2405">__Ch__</span></span>  | <span data-ttu-id="4e5da-2406">__st__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2406">__St__</span></span> | <span data-ttu-id="4e5da-2407">__Ob__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2407">__Ob__</span></span> | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| <span data-ttu-id="4e5da-2408">__bo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2408">__Bo__</span></span> | <span data-ttu-id="4e5da-2409">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2409">St</span></span> | <span data-ttu-id="4e5da-2410">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2410">St</span></span> | <span data-ttu-id="4e5da-2411">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2411">St</span></span> | <span data-ttu-id="4e5da-2412">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2412">St</span></span> | <span data-ttu-id="4e5da-2413">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2413">St</span></span> | <span data-ttu-id="4e5da-2414">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2414">St</span></span> | <span data-ttu-id="4e5da-2415">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2415">St</span></span> | <span data-ttu-id="4e5da-2416">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2416">St</span></span> | <span data-ttu-id="4e5da-2417">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2417">St</span></span> | <span data-ttu-id="4e5da-2418">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2418">St</span></span> | <span data-ttu-id="4e5da-2419">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2419">St</span></span> | <span data-ttu-id="4e5da-2420">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2420">St</span></span> | <span data-ttu-id="4e5da-2421">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2421">St</span></span> | <span data-ttu-id="4e5da-2422">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2422">St</span></span> | <span data-ttu-id="4e5da-2423">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2423">St</span></span> | <span data-ttu-id="4e5da-2424">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2424">Ob</span></span> | 
| <span data-ttu-id="4e5da-2425">__SB__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2425">__SB__</span></span> |    | <span data-ttu-id="4e5da-2426">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2426">St</span></span> | <span data-ttu-id="4e5da-2427">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2427">St</span></span> | <span data-ttu-id="4e5da-2428">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2428">St</span></span> | <span data-ttu-id="4e5da-2429">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2429">St</span></span> | <span data-ttu-id="4e5da-2430">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2430">St</span></span> | <span data-ttu-id="4e5da-2431">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2431">St</span></span> | <span data-ttu-id="4e5da-2432">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2432">St</span></span> | <span data-ttu-id="4e5da-2433">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2433">St</span></span> | <span data-ttu-id="4e5da-2434">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2434">St</span></span> | <span data-ttu-id="4e5da-2435">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2435">St</span></span> | <span data-ttu-id="4e5da-2436">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2436">St</span></span> | <span data-ttu-id="4e5da-2437">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2437">St</span></span> | <span data-ttu-id="4e5da-2438">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2438">St</span></span> | <span data-ttu-id="4e5da-2439">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2439">St</span></span> | <span data-ttu-id="4e5da-2440">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2440">Ob</span></span> | 
| <span data-ttu-id="4e5da-2441">__によって__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2441">__By__</span></span> |    |    | <span data-ttu-id="4e5da-2442">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2442">St</span></span> | <span data-ttu-id="4e5da-2443">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2443">St</span></span> | <span data-ttu-id="4e5da-2444">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2444">St</span></span> | <span data-ttu-id="4e5da-2445">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2445">St</span></span> | <span data-ttu-id="4e5da-2446">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2446">St</span></span> | <span data-ttu-id="4e5da-2447">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2447">St</span></span> | <span data-ttu-id="4e5da-2448">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2448">St</span></span> | <span data-ttu-id="4e5da-2449">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2449">St</span></span> | <span data-ttu-id="4e5da-2450">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2450">St</span></span> | <span data-ttu-id="4e5da-2451">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2451">St</span></span> | <span data-ttu-id="4e5da-2452">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2452">St</span></span> | <span data-ttu-id="4e5da-2453">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2453">St</span></span> | <span data-ttu-id="4e5da-2454">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2454">St</span></span> | <span data-ttu-id="4e5da-2455">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2455">Ob</span></span> | 
| <span data-ttu-id="4e5da-2456">__Sh__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2456">__Sh__</span></span> |    |    |    | <span data-ttu-id="4e5da-2457">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2457">St</span></span> | <span data-ttu-id="4e5da-2458">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2458">St</span></span> | <span data-ttu-id="4e5da-2459">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2459">St</span></span> | <span data-ttu-id="4e5da-2460">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2460">St</span></span> | <span data-ttu-id="4e5da-2461">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2461">St</span></span> | <span data-ttu-id="4e5da-2462">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2462">St</span></span> | <span data-ttu-id="4e5da-2463">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2463">St</span></span> | <span data-ttu-id="4e5da-2464">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2464">St</span></span> | <span data-ttu-id="4e5da-2465">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2465">St</span></span> | <span data-ttu-id="4e5da-2466">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2466">St</span></span> | <span data-ttu-id="4e5da-2467">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2467">St</span></span> | <span data-ttu-id="4e5da-2468">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2468">St</span></span> | <span data-ttu-id="4e5da-2469">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2469">Ob</span></span> | 
| <span data-ttu-id="4e5da-2470">__米国__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2470">__US__</span></span> |    |    |    |    | <span data-ttu-id="4e5da-2471">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2471">St</span></span> | <span data-ttu-id="4e5da-2472">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2472">St</span></span> | <span data-ttu-id="4e5da-2473">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2473">St</span></span> | <span data-ttu-id="4e5da-2474">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2474">St</span></span> | <span data-ttu-id="4e5da-2475">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2475">St</span></span> | <span data-ttu-id="4e5da-2476">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2476">St</span></span> | <span data-ttu-id="4e5da-2477">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2477">St</span></span> | <span data-ttu-id="4e5da-2478">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2478">St</span></span> | <span data-ttu-id="4e5da-2479">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2479">St</span></span> | <span data-ttu-id="4e5da-2480">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2480">St</span></span> | <span data-ttu-id="4e5da-2481">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2481">St</span></span> | <span data-ttu-id="4e5da-2482">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2482">Ob</span></span> | 
| <span data-ttu-id="4e5da-2483">__In__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2483">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="4e5da-2484">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2484">St</span></span> | <span data-ttu-id="4e5da-2485">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2485">St</span></span> | <span data-ttu-id="4e5da-2486">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2486">St</span></span> | <span data-ttu-id="4e5da-2487">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2487">St</span></span> | <span data-ttu-id="4e5da-2488">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2488">St</span></span> | <span data-ttu-id="4e5da-2489">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2489">St</span></span> | <span data-ttu-id="4e5da-2490">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2490">St</span></span> | <span data-ttu-id="4e5da-2491">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2491">St</span></span> | <span data-ttu-id="4e5da-2492">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2492">St</span></span> | <span data-ttu-id="4e5da-2493">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2493">St</span></span> | <span data-ttu-id="4e5da-2494">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2494">Ob</span></span> | 
| <span data-ttu-id="4e5da-2495">__UI__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2495">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="4e5da-2496">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2496">St</span></span> | <span data-ttu-id="4e5da-2497">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2497">St</span></span> | <span data-ttu-id="4e5da-2498">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2498">St</span></span> | <span data-ttu-id="4e5da-2499">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2499">St</span></span> | <span data-ttu-id="4e5da-2500">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2500">St</span></span> | <span data-ttu-id="4e5da-2501">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2501">St</span></span> | <span data-ttu-id="4e5da-2502">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2502">St</span></span> | <span data-ttu-id="4e5da-2503">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2503">St</span></span> | <span data-ttu-id="4e5da-2504">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2504">St</span></span> | <span data-ttu-id="4e5da-2505">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2505">Ob</span></span> | 
| <span data-ttu-id="4e5da-2506">__Lo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2506">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2507">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2507">St</span></span> | <span data-ttu-id="4e5da-2508">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2508">St</span></span> | <span data-ttu-id="4e5da-2509">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2509">St</span></span> | <span data-ttu-id="4e5da-2510">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2510">St</span></span> | <span data-ttu-id="4e5da-2511">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2511">St</span></span> | <span data-ttu-id="4e5da-2512">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2512">St</span></span> | <span data-ttu-id="4e5da-2513">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2513">St</span></span> | <span data-ttu-id="4e5da-2514">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2514">St</span></span> | <span data-ttu-id="4e5da-2515">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2515">Ob</span></span> | 
| <span data-ttu-id="4e5da-2516">__UL__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2516">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2517">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2517">St</span></span> | <span data-ttu-id="4e5da-2518">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2518">St</span></span> | <span data-ttu-id="4e5da-2519">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2519">St</span></span> | <span data-ttu-id="4e5da-2520">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2520">St</span></span> | <span data-ttu-id="4e5da-2521">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2521">St</span></span> | <span data-ttu-id="4e5da-2522">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2522">St</span></span> | <span data-ttu-id="4e5da-2523">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2523">St</span></span> | <span data-ttu-id="4e5da-2524">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2524">Ob</span></span> | 
| <span data-ttu-id="4e5da-2525">__de__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2525">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2526">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2526">St</span></span> | <span data-ttu-id="4e5da-2527">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2527">St</span></span> | <span data-ttu-id="4e5da-2528">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2528">St</span></span> | <span data-ttu-id="4e5da-2529">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2529">St</span></span> | <span data-ttu-id="4e5da-2530">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2530">St</span></span> | <span data-ttu-id="4e5da-2531">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2531">St</span></span> | <span data-ttu-id="4e5da-2532">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2532">Ob</span></span> | 
| <span data-ttu-id="4e5da-2533">__si__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2533">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2534">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2534">St</span></span> | <span data-ttu-id="4e5da-2535">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2535">St</span></span> | <span data-ttu-id="4e5da-2536">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2536">St</span></span> | <span data-ttu-id="4e5da-2537">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2537">St</span></span> | <span data-ttu-id="4e5da-2538">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2538">St</span></span> | <span data-ttu-id="4e5da-2539">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2539">Ob</span></span> | 
| <span data-ttu-id="4e5da-2540">__Do__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2540">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2541">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2541">St</span></span> | <span data-ttu-id="4e5da-2542">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2542">St</span></span> | <span data-ttu-id="4e5da-2543">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2543">St</span></span> | <span data-ttu-id="4e5da-2544">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2544">St</span></span> | <span data-ttu-id="4e5da-2545">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2545">Ob</span></span> | 
| <span data-ttu-id="4e5da-2546">__Da__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2546">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2547">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2547">St</span></span> | <span data-ttu-id="4e5da-2548">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2548">St</span></span> | <span data-ttu-id="4e5da-2549">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2549">St</span></span> | <span data-ttu-id="4e5da-2550">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2550">Ob</span></span> | 
| <span data-ttu-id="4e5da-2551">__ch__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2551">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2552">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2552">St</span></span> | <span data-ttu-id="4e5da-2553">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2553">St</span></span> | <span data-ttu-id="4e5da-2554">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2554">Ob</span></span> | 
| <span data-ttu-id="4e5da-2555">__st__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2555">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2556">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2556">St</span></span> | <span data-ttu-id="4e5da-2557">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2557">Ob</span></span> | 
| <span data-ttu-id="4e5da-2558">__Ob__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2558">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2559">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2559">Ob</span></span> | 


## <a name="concatenation-operator"></a><span data-ttu-id="4e5da-2560">連結演算子</span><span class="sxs-lookup"><span data-stu-id="4e5da-2560">Concatenation Operator</span></span>

```antlr
ConcatenationOperatorExpression
    : Expression '&' LineTerminator? Expression
    ;
```

<span data-ttu-id="4e5da-2561">*連結演算子*に対してすべての組み込みの値の型の null 許容バージョンを含む、組み込み型が定義されています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2561">The *concatenation operator* is defined for all of the intrinsic types, including the nullable versions of the intrinsic value types.</span></span> <span data-ttu-id="4e5da-2562">上記で説明した型の間の連結用も定義されていると`System.DBNull`、として扱われる、`Nothing`文字列。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2562">It is also defined for concatenation between the types mentioned above and `System.DBNull`, which is treated as a `Nothing` string.</span></span> <span data-ttu-id="4e5da-2563">連結演算子では、すべてのオペランドに変換します`String`; へのすべての変換、式に`String`は厳密な型を使用するかどうかに関係なく、拡大変換と見なされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2563">The concatenation operator converts all of its operands to `String`; in the expression, all conversions to `String` are considered to be widening, regardless of whether strict semantics are used.</span></span> <span data-ttu-id="4e5da-2564">A`System.DBNull`リテラルに変換される値`Nothing`として型指定された`String`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2564">A `System.DBNull` value is converted to the literal `Nothing` typed as `String`.</span></span> <span data-ttu-id="4e5da-2565">値が null 許容値型`Nothing`リテラルに変換も`Nothing`として型指定された`String`実行時エラーがスローされるのではなく。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2565">A nullable value type whose value is `Nothing` is also converted to the literal `Nothing` typed as `String`, rather than throwing a run-time error.</span></span>

<span data-ttu-id="4e5da-2566">連結演算の結果を 2 つのオペランドが左から右への順序で連結した文字列にします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2566">A concatenation operation results in a string that is the concatenation of the two operands in order from left to right.</span></span> <span data-ttu-id="4e5da-2567">値`Nothing`は、空の文字列リテラルの場合と同様に扱われます`""`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2567">The value `Nothing` is treated as if it were the empty string literal `""`.</span></span>

<span data-ttu-id="4e5da-2568">__操作の種類:__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2568">__Operation Type:__</span></span>

|        | <span data-ttu-id="4e5da-2569">__bo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2569">__Bo__</span></span> | <span data-ttu-id="4e5da-2570">__SB__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2570">__SB__</span></span> | <span data-ttu-id="4e5da-2571">__によって__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2571">__By__</span></span> | <span data-ttu-id="4e5da-2572">__Sh__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2572">__Sh__</span></span> | <span data-ttu-id="4e5da-2573">__米国__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2573">__US__</span></span> | <span data-ttu-id="4e5da-2574">__In__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2574">__In__</span></span> | <span data-ttu-id="4e5da-2575">__UI__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2575">__UI__</span></span> | <span data-ttu-id="4e5da-2576">__Lo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2576">__Lo__</span></span> | <span data-ttu-id="4e5da-2577">__UL__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2577">__UL__</span></span> | <span data-ttu-id="4e5da-2578">__de__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2578">__De__</span></span> | <span data-ttu-id="4e5da-2579">__si__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2579">__Si__</span></span> | <span data-ttu-id="4e5da-2580">__Do__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2580">__Do__</span></span> | <span data-ttu-id="4e5da-2581">__Da__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2581">__Da__</span></span>  | <span data-ttu-id="4e5da-2582">__ch__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2582">__Ch__</span></span>  | <span data-ttu-id="4e5da-2583">__st__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2583">__St__</span></span> | <span data-ttu-id="4e5da-2584">__Ob__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2584">__Ob__</span></span> | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| <span data-ttu-id="4e5da-2585">__bo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2585">__Bo__</span></span> | <span data-ttu-id="4e5da-2586">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2586">St</span></span> | <span data-ttu-id="4e5da-2587">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2587">St</span></span> | <span data-ttu-id="4e5da-2588">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2588">St</span></span> | <span data-ttu-id="4e5da-2589">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2589">St</span></span> | <span data-ttu-id="4e5da-2590">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2590">St</span></span> | <span data-ttu-id="4e5da-2591">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2591">St</span></span> | <span data-ttu-id="4e5da-2592">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2592">St</span></span> | <span data-ttu-id="4e5da-2593">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2593">St</span></span> | <span data-ttu-id="4e5da-2594">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2594">St</span></span> | <span data-ttu-id="4e5da-2595">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2595">St</span></span> | <span data-ttu-id="4e5da-2596">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2596">St</span></span> | <span data-ttu-id="4e5da-2597">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2597">St</span></span> | <span data-ttu-id="4e5da-2598">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2598">St</span></span> | <span data-ttu-id="4e5da-2599">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2599">St</span></span> | <span data-ttu-id="4e5da-2600">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2600">St</span></span> | <span data-ttu-id="4e5da-2601">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2601">Ob</span></span> | 
| <span data-ttu-id="4e5da-2602">__SB__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2602">__SB__</span></span> |    | <span data-ttu-id="4e5da-2603">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2603">St</span></span> | <span data-ttu-id="4e5da-2604">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2604">St</span></span> | <span data-ttu-id="4e5da-2605">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2605">St</span></span> | <span data-ttu-id="4e5da-2606">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2606">St</span></span> | <span data-ttu-id="4e5da-2607">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2607">St</span></span> | <span data-ttu-id="4e5da-2608">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2608">St</span></span> | <span data-ttu-id="4e5da-2609">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2609">St</span></span> | <span data-ttu-id="4e5da-2610">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2610">St</span></span> | <span data-ttu-id="4e5da-2611">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2611">St</span></span> | <span data-ttu-id="4e5da-2612">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2612">St</span></span> | <span data-ttu-id="4e5da-2613">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2613">St</span></span> | <span data-ttu-id="4e5da-2614">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2614">St</span></span> | <span data-ttu-id="4e5da-2615">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2615">St</span></span> | <span data-ttu-id="4e5da-2616">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2616">St</span></span> | <span data-ttu-id="4e5da-2617">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2617">Ob</span></span> | 
| <span data-ttu-id="4e5da-2618">__によって__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2618">__By__</span></span> |    |    | <span data-ttu-id="4e5da-2619">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2619">St</span></span> | <span data-ttu-id="4e5da-2620">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2620">St</span></span> | <span data-ttu-id="4e5da-2621">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2621">St</span></span> | <span data-ttu-id="4e5da-2622">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2622">St</span></span> | <span data-ttu-id="4e5da-2623">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2623">St</span></span> | <span data-ttu-id="4e5da-2624">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2624">St</span></span> | <span data-ttu-id="4e5da-2625">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2625">St</span></span> | <span data-ttu-id="4e5da-2626">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2626">St</span></span> | <span data-ttu-id="4e5da-2627">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2627">St</span></span> | <span data-ttu-id="4e5da-2628">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2628">St</span></span> | <span data-ttu-id="4e5da-2629">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2629">St</span></span> | <span data-ttu-id="4e5da-2630">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2630">St</span></span> | <span data-ttu-id="4e5da-2631">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2631">St</span></span> | <span data-ttu-id="4e5da-2632">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2632">Ob</span></span> | 
| <span data-ttu-id="4e5da-2633">__Sh__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2633">__Sh__</span></span> |    |    |    | <span data-ttu-id="4e5da-2634">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2634">St</span></span> | <span data-ttu-id="4e5da-2635">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2635">St</span></span> | <span data-ttu-id="4e5da-2636">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2636">St</span></span> | <span data-ttu-id="4e5da-2637">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2637">St</span></span> | <span data-ttu-id="4e5da-2638">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2638">St</span></span> | <span data-ttu-id="4e5da-2639">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2639">St</span></span> | <span data-ttu-id="4e5da-2640">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2640">St</span></span> | <span data-ttu-id="4e5da-2641">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2641">St</span></span> | <span data-ttu-id="4e5da-2642">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2642">St</span></span> | <span data-ttu-id="4e5da-2643">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2643">St</span></span> | <span data-ttu-id="4e5da-2644">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2644">St</span></span> | <span data-ttu-id="4e5da-2645">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2645">St</span></span> | <span data-ttu-id="4e5da-2646">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2646">Ob</span></span> | 
| <span data-ttu-id="4e5da-2647">__米国__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2647">__US__</span></span> |    |    |    |    | <span data-ttu-id="4e5da-2648">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2648">St</span></span> | <span data-ttu-id="4e5da-2649">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2649">St</span></span> | <span data-ttu-id="4e5da-2650">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2650">St</span></span> | <span data-ttu-id="4e5da-2651">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2651">St</span></span> | <span data-ttu-id="4e5da-2652">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2652">St</span></span> | <span data-ttu-id="4e5da-2653">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2653">St</span></span> | <span data-ttu-id="4e5da-2654">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2654">St</span></span> | <span data-ttu-id="4e5da-2655">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2655">St</span></span> | <span data-ttu-id="4e5da-2656">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2656">St</span></span> | <span data-ttu-id="4e5da-2657">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2657">St</span></span> | <span data-ttu-id="4e5da-2658">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2658">St</span></span> | <span data-ttu-id="4e5da-2659">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2659">Ob</span></span> | 
| <span data-ttu-id="4e5da-2660">__In__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2660">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="4e5da-2661">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2661">St</span></span> | <span data-ttu-id="4e5da-2662">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2662">St</span></span> | <span data-ttu-id="4e5da-2663">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2663">St</span></span> | <span data-ttu-id="4e5da-2664">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2664">St</span></span> | <span data-ttu-id="4e5da-2665">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2665">St</span></span> | <span data-ttu-id="4e5da-2666">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2666">St</span></span> | <span data-ttu-id="4e5da-2667">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2667">St</span></span> | <span data-ttu-id="4e5da-2668">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2668">St</span></span> | <span data-ttu-id="4e5da-2669">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2669">St</span></span> | <span data-ttu-id="4e5da-2670">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2670">St</span></span> | <span data-ttu-id="4e5da-2671">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2671">Ob</span></span> | 
| <span data-ttu-id="4e5da-2672">__UI__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2672">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="4e5da-2673">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2673">St</span></span> | <span data-ttu-id="4e5da-2674">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2674">St</span></span> | <span data-ttu-id="4e5da-2675">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2675">St</span></span> | <span data-ttu-id="4e5da-2676">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2676">St</span></span> | <span data-ttu-id="4e5da-2677">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2677">St</span></span> | <span data-ttu-id="4e5da-2678">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2678">St</span></span> | <span data-ttu-id="4e5da-2679">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2679">St</span></span> | <span data-ttu-id="4e5da-2680">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2680">St</span></span> | <span data-ttu-id="4e5da-2681">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2681">St</span></span> | <span data-ttu-id="4e5da-2682">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2682">Ob</span></span> | 
| <span data-ttu-id="4e5da-2683">__Lo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2683">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2684">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2684">St</span></span> | <span data-ttu-id="4e5da-2685">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2685">St</span></span> | <span data-ttu-id="4e5da-2686">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2686">St</span></span> | <span data-ttu-id="4e5da-2687">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2687">St</span></span> | <span data-ttu-id="4e5da-2688">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2688">St</span></span> | <span data-ttu-id="4e5da-2689">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2689">St</span></span> | <span data-ttu-id="4e5da-2690">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2690">St</span></span> | <span data-ttu-id="4e5da-2691">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2691">St</span></span> | <span data-ttu-id="4e5da-2692">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2692">Ob</span></span> | 
| <span data-ttu-id="4e5da-2693">__UL__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2693">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2694">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2694">St</span></span> | <span data-ttu-id="4e5da-2695">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2695">St</span></span> | <span data-ttu-id="4e5da-2696">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2696">St</span></span> | <span data-ttu-id="4e5da-2697">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2697">St</span></span> | <span data-ttu-id="4e5da-2698">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2698">St</span></span> | <span data-ttu-id="4e5da-2699">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2699">St</span></span> | <span data-ttu-id="4e5da-2700">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2700">St</span></span> | <span data-ttu-id="4e5da-2701">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2701">Ob</span></span> | 
| <span data-ttu-id="4e5da-2702">__de__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2702">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2703">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2703">St</span></span> | <span data-ttu-id="4e5da-2704">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2704">St</span></span> | <span data-ttu-id="4e5da-2705">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2705">St</span></span> | <span data-ttu-id="4e5da-2706">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2706">St</span></span> | <span data-ttu-id="4e5da-2707">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2707">St</span></span> | <span data-ttu-id="4e5da-2708">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2708">St</span></span> | <span data-ttu-id="4e5da-2709">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2709">Ob</span></span> | 
| <span data-ttu-id="4e5da-2710">__si__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2710">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2711">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2711">St</span></span> | <span data-ttu-id="4e5da-2712">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2712">St</span></span> | <span data-ttu-id="4e5da-2713">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2713">St</span></span> | <span data-ttu-id="4e5da-2714">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2714">St</span></span> | <span data-ttu-id="4e5da-2715">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2715">St</span></span> | <span data-ttu-id="4e5da-2716">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2716">Ob</span></span> | 
| <span data-ttu-id="4e5da-2717">__Do__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2717">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2718">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2718">St</span></span> | <span data-ttu-id="4e5da-2719">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2719">St</span></span> | <span data-ttu-id="4e5da-2720">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2720">St</span></span> | <span data-ttu-id="4e5da-2721">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2721">St</span></span> | <span data-ttu-id="4e5da-2722">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2722">Ob</span></span> | 
| <span data-ttu-id="4e5da-2723">__Da__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2723">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2724">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2724">St</span></span> | <span data-ttu-id="4e5da-2725">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2725">St</span></span> | <span data-ttu-id="4e5da-2726">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2726">St</span></span> | <span data-ttu-id="4e5da-2727">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2727">Ob</span></span> | 
| <span data-ttu-id="4e5da-2728">__ch__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2728">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2729">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2729">St</span></span> | <span data-ttu-id="4e5da-2730">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2730">St</span></span> | <span data-ttu-id="4e5da-2731">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2731">Ob</span></span> | 
| <span data-ttu-id="4e5da-2732">__st__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2732">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2733">st</span><span class="sxs-lookup"><span data-stu-id="4e5da-2733">St</span></span> | <span data-ttu-id="4e5da-2734">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2734">Ob</span></span> | 
| <span data-ttu-id="4e5da-2735">__Ob__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2735">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2736">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2736">Ob</span></span> | 


## <a name="logical-operators"></a><span data-ttu-id="4e5da-2737">論理演算子</span><span class="sxs-lookup"><span data-stu-id="4e5da-2737">Logical Operators</span></span>

<span data-ttu-id="4e5da-2738">`And`、 `Not`、 `Or`、および`Xor`演算子は論理演算子と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2738">The `And`, `Not`, `Or`, and `Xor` operators are called the logical operators.</span></span>

```antlr
LogicalOperatorExpression
    : 'Not' Expression
    | Expression 'And' LineTerminator? Expression
    | Expression 'Or' LineTerminator? Expression
    | Expression 'Xor' LineTerminator? Expression
    ;
```

<span data-ttu-id="4e5da-2739">論理演算子は、次のように評価されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2739">The logical operators are evaluated as follows:</span></span>

* <span data-ttu-id="4e5da-2740">`Boolean`型。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2740">For the `Boolean` type:</span></span>

  * <span data-ttu-id="4e5da-2741">論理`And`2 つのオペランドに対して操作を実行します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2741">A logical `And` operation is performed on its two operands.</span></span>

  * <span data-ttu-id="4e5da-2742">論理`Not`オペランドに対して操作を実行します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2742">A logical `Not` operation is performed on its operand.</span></span>

  * <span data-ttu-id="4e5da-2743">論理`Or`2 つのオペランドに対して操作を実行します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2743">A logical `Or` operation is performed on its two operands.</span></span>

  * <span data-ttu-id="4e5da-2744">論理の排他-`Or` 2 つのオペランドに対して操作を実行します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2744">A logical exclusive-`Or` operation is performed on its two operands.</span></span>

* <span data-ttu-id="4e5da-2745">`Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、 `ULong`、 `Long`、すべての列挙型のバイナリ表現の各ビットに対して指定された操作が実行を2 つのオペランド。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2745">For `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, and all enumerated types, the specified operation is performed on each bit of the binary representation of the two operand(s):</span></span>

  * <span data-ttu-id="4e5da-2746">`And`: 結果のビットが 1 の場合、両方のビットが 1 になります。それ以外の場合、結果のビットは 0 です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2746">`And`: The result bit is 1 if both bits are 1; otherwise the result bit is 0.</span></span>

  * <span data-ttu-id="4e5da-2747">`Not`: 結果のビットが 1 のビットが 0 以外の場合それ以外の場合、結果のビットは 1 です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2747">`Not`: The result bit is 1 if the bit is 0; otherwise the result bit is 1.</span></span>

  * <span data-ttu-id="4e5da-2748">`Or`: 結果のビットが 1 のいずれかのビットが 1 以外の場合それ以外の場合、結果のビットは 0 です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2748">`Or`: The result bit is 1 if either bit is 1; otherwise the result bit is 0.</span></span>

  * <span data-ttu-id="4e5da-2749">`Xor`: 結果のビットが 1 の場合、いずれかのビットは 1 が両方のビットです。それ以外の場合、結果のビットは 0 です (つまり、1 `Xor` 0 = 1, 1 `Xor` 1 = 0)。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2749">`Xor`: The result bit is 1 if either bit is 1 but not both bits; otherwise the result bit is 0 (that is, 1 `Xor` 0 = 1, 1 `Xor` 1 = 0).</span></span>

* <span data-ttu-id="4e5da-2750">ときに、論理演算子`And`と`Or`の種類が無効になる`Boolean?`、ブール ロジックの 3 つの値として含まれるように拡張します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2750">When the logical operators `And` and `Or` are lifted for the type `Boolean?`, they are extended to encompass three-valued Boolean logic as such:</span></span>

  * <span data-ttu-id="4e5da-2751">`And` 両方のオペランドが true である場合は true に評価されます。false の場合は、オペランドの 1 つは false です。`Nothing`それ以外の場合。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2751">`And` evaluates to true if both operands are true; false if one of the operands is false; `Nothing` otherwise.</span></span>

  * <span data-ttu-id="4e5da-2752">`Or` どちらかのオペランドが true である場合は true に評価されます。false は両方のオペランドが false です。`Nothing`それ以外の場合。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2752">`Or` evaluates to true if either operand is true; false is both operands are false; `Nothing` otherwise.</span></span>

<span data-ttu-id="4e5da-2753">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-2753">For example:</span></span>

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

<span data-ttu-id="4e5da-2754">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2754">__Note.__</span></span> <span data-ttu-id="4e5da-2755">理想的には、論理演算子`And`と`Or`3 値ロジックを使用して、ブール式で使用できる任意の型のリフトが (実装する型つまり`IsTrue`と`IsFalse`)、同じ方法`AndAlso``OrElse`ブール式で使用できる任意の型の間でのショート サーキットします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2755">Ideally, the logical operators `And` and `Or` would be lifted using three-valued logic for any type that can be used in a Boolean expression (i.e. a type that implements `IsTrue` and `IsFalse`), in the same way that `AndAlso` and `OrElse` short circuit across any type that can be used in a Boolean expression.</span></span> <span data-ttu-id="4e5da-2756">残念ながら、3 つの値を持ち上げるにのみ適用`Boolean?`3 値ロジックを求めているユーザー定義の型が定義することで手動で行う必要がありますので、`And`と`Or`null 許容のバージョンの演算子。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2756">Unfortunately, three-valued lifting is only applied to `Boolean?`, so user-defined types that desire three-valued logic must do so manually by defining `And` and `Or` operators for their nullable version.</span></span>

<span data-ttu-id="4e5da-2757">オーバーフローは、これらの操作からではありません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2757">No overflows are possible from these operations.</span></span> <span data-ttu-id="4e5da-2758">列挙型の演算子は、列挙型の基になる型でビットごとの演算が、戻り値は列挙型です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2758">The enumerated type operators do the bitwise operation on the underlying type of the enumerated type, but the return value is the enumerated type.</span></span>

<span data-ttu-id="4e5da-2759">__ない操作の種類:__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2759">__Not Operation Type:__</span></span>

| <span data-ttu-id="4e5da-2760">__bo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2760">__Bo__</span></span> | <span data-ttu-id="4e5da-2761">__SB__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2761">__SB__</span></span> | <span data-ttu-id="4e5da-2762">__によって__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2762">__By__</span></span> | <span data-ttu-id="4e5da-2763">__Sh__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2763">__Sh__</span></span> | <span data-ttu-id="4e5da-2764">__米国__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2764">__US__</span></span> | <span data-ttu-id="4e5da-2765">__In__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2765">__In__</span></span> | <span data-ttu-id="4e5da-2766">__UI__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2766">__UI__</span></span> | <span data-ttu-id="4e5da-2767">__Lo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2767">__Lo__</span></span> | <span data-ttu-id="4e5da-2768">__UL__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2768">__UL__</span></span> | <span data-ttu-id="4e5da-2769">__de__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2769">__De__</span></span> | <span data-ttu-id="4e5da-2770">__si__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2770">__Si__</span></span> | <span data-ttu-id="4e5da-2771">__Do__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2771">__Do__</span></span> | <span data-ttu-id="4e5da-2772">__Da__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2772">__Da__</span></span>  | <span data-ttu-id="4e5da-2773">__ch__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2773">__Ch__</span></span>  | <span data-ttu-id="4e5da-2774">__st__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2774">__St__</span></span> | <span data-ttu-id="4e5da-2775">__Ob__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2775">__Ob__</span></span> | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="4e5da-2776">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2776">Bo</span></span> | <span data-ttu-id="4e5da-2777">SB</span><span class="sxs-lookup"><span data-stu-id="4e5da-2777">SB</span></span> | <span data-ttu-id="4e5da-2778">By</span><span class="sxs-lookup"><span data-stu-id="4e5da-2778">By</span></span> | <span data-ttu-id="4e5da-2779">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-2779">Sh</span></span> | <span data-ttu-id="4e5da-2780">US</span><span class="sxs-lookup"><span data-stu-id="4e5da-2780">US</span></span> | <span data-ttu-id="4e5da-2781">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-2781">In</span></span> | <span data-ttu-id="4e5da-2782">UI</span><span class="sxs-lookup"><span data-stu-id="4e5da-2782">UI</span></span> | <span data-ttu-id="4e5da-2783">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2783">Lo</span></span> | <span data-ttu-id="4e5da-2784">UL</span><span class="sxs-lookup"><span data-stu-id="4e5da-2784">UL</span></span> | <span data-ttu-id="4e5da-2785">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2785">Lo</span></span> | <span data-ttu-id="4e5da-2786">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2786">Lo</span></span> | <span data-ttu-id="4e5da-2787">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2787">Lo</span></span> | <span data-ttu-id="4e5da-2788">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2788">Err</span></span> | <span data-ttu-id="4e5da-2789">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2789">Err</span></span> | <span data-ttu-id="4e5da-2790">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2790">Lo</span></span> | <span data-ttu-id="4e5da-2791">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2791">Ob</span></span> | 

<span data-ttu-id="4e5da-2792">__または Xor 操作の種類。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2792">__And, Or, Xor Operation Type:__</span></span>

|        | <span data-ttu-id="4e5da-2793">__bo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2793">__Bo__</span></span> | <span data-ttu-id="4e5da-2794">__SB__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2794">__SB__</span></span> | <span data-ttu-id="4e5da-2795">__によって__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2795">__By__</span></span> | <span data-ttu-id="4e5da-2796">__Sh__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2796">__Sh__</span></span> | <span data-ttu-id="4e5da-2797">__米国__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2797">__US__</span></span> | <span data-ttu-id="4e5da-2798">__In__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2798">__In__</span></span> | <span data-ttu-id="4e5da-2799">__UI__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2799">__UI__</span></span> | <span data-ttu-id="4e5da-2800">__Lo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2800">__Lo__</span></span> | <span data-ttu-id="4e5da-2801">__UL__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2801">__UL__</span></span> | <span data-ttu-id="4e5da-2802">__de__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2802">__De__</span></span> | <span data-ttu-id="4e5da-2803">__si__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2803">__Si__</span></span> | <span data-ttu-id="4e5da-2804">__Do__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2804">__Do__</span></span> | <span data-ttu-id="4e5da-2805">__Da__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2805">__Da__</span></span>  | <span data-ttu-id="4e5da-2806">__ch__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2806">__Ch__</span></span>  | <span data-ttu-id="4e5da-2807">__st__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2807">__St__</span></span> | <span data-ttu-id="4e5da-2808">__Ob__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2808">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="4e5da-2809">__bo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2809">__Bo__</span></span> | <span data-ttu-id="4e5da-2810">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2810">Bo</span></span> | <span data-ttu-id="4e5da-2811">SB</span><span class="sxs-lookup"><span data-stu-id="4e5da-2811">SB</span></span> | <span data-ttu-id="4e5da-2812">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-2812">Sh</span></span> | <span data-ttu-id="4e5da-2813">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-2813">Sh</span></span> | <span data-ttu-id="4e5da-2814">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-2814">In</span></span> | <span data-ttu-id="4e5da-2815">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-2815">In</span></span> | <span data-ttu-id="4e5da-2816">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2816">Lo</span></span> | <span data-ttu-id="4e5da-2817">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2817">Lo</span></span> | <span data-ttu-id="4e5da-2818">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2818">Lo</span></span> | <span data-ttu-id="4e5da-2819">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2819">Lo</span></span> | <span data-ttu-id="4e5da-2820">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2820">Lo</span></span> | <span data-ttu-id="4e5da-2821">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2821">Lo</span></span> | <span data-ttu-id="4e5da-2822">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2822">Err</span></span> | <span data-ttu-id="4e5da-2823">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2823">Err</span></span> | <span data-ttu-id="4e5da-2824">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2824">Bo</span></span>  | <span data-ttu-id="4e5da-2825">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2825">Ob</span></span>  | 
| <span data-ttu-id="4e5da-2826">__SB__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2826">__SB__</span></span> |    | <span data-ttu-id="4e5da-2827">SB</span><span class="sxs-lookup"><span data-stu-id="4e5da-2827">SB</span></span> | <span data-ttu-id="4e5da-2828">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-2828">Sh</span></span> | <span data-ttu-id="4e5da-2829">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-2829">Sh</span></span> | <span data-ttu-id="4e5da-2830">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-2830">In</span></span> | <span data-ttu-id="4e5da-2831">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-2831">In</span></span> | <span data-ttu-id="4e5da-2832">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2832">Lo</span></span> | <span data-ttu-id="4e5da-2833">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2833">Lo</span></span> | <span data-ttu-id="4e5da-2834">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2834">Lo</span></span> | <span data-ttu-id="4e5da-2835">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2835">Lo</span></span> | <span data-ttu-id="4e5da-2836">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2836">Lo</span></span> | <span data-ttu-id="4e5da-2837">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2837">Lo</span></span> | <span data-ttu-id="4e5da-2838">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2838">Err</span></span> | <span data-ttu-id="4e5da-2839">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2839">Err</span></span> | <span data-ttu-id="4e5da-2840">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2840">Lo</span></span>  | <span data-ttu-id="4e5da-2841">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2841">Ob</span></span>  | 
| <span data-ttu-id="4e5da-2842">__によって__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2842">__By__</span></span> |    |    | <span data-ttu-id="4e5da-2843">By</span><span class="sxs-lookup"><span data-stu-id="4e5da-2843">By</span></span> | <span data-ttu-id="4e5da-2844">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-2844">Sh</span></span> | <span data-ttu-id="4e5da-2845">US</span><span class="sxs-lookup"><span data-stu-id="4e5da-2845">US</span></span> | <span data-ttu-id="4e5da-2846">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-2846">In</span></span> | <span data-ttu-id="4e5da-2847">UI</span><span class="sxs-lookup"><span data-stu-id="4e5da-2847">UI</span></span> | <span data-ttu-id="4e5da-2848">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2848">Lo</span></span> | <span data-ttu-id="4e5da-2849">UL</span><span class="sxs-lookup"><span data-stu-id="4e5da-2849">UL</span></span> | <span data-ttu-id="4e5da-2850">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2850">Lo</span></span> | <span data-ttu-id="4e5da-2851">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2851">Lo</span></span> | <span data-ttu-id="4e5da-2852">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2852">Lo</span></span> | <span data-ttu-id="4e5da-2853">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2853">Err</span></span> | <span data-ttu-id="4e5da-2854">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2854">Err</span></span> | <span data-ttu-id="4e5da-2855">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2855">Lo</span></span>  | <span data-ttu-id="4e5da-2856">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2856">Ob</span></span>  | 
| <span data-ttu-id="4e5da-2857">__Sh__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2857">__Sh__</span></span> |    |    |    | <span data-ttu-id="4e5da-2858">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-2858">Sh</span></span> | <span data-ttu-id="4e5da-2859">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-2859">In</span></span> | <span data-ttu-id="4e5da-2860">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-2860">In</span></span> | <span data-ttu-id="4e5da-2861">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2861">Lo</span></span> | <span data-ttu-id="4e5da-2862">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2862">Lo</span></span> | <span data-ttu-id="4e5da-2863">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2863">Lo</span></span> | <span data-ttu-id="4e5da-2864">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2864">Lo</span></span> | <span data-ttu-id="4e5da-2865">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2865">Lo</span></span> | <span data-ttu-id="4e5da-2866">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2866">Lo</span></span> | <span data-ttu-id="4e5da-2867">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2867">Err</span></span> | <span data-ttu-id="4e5da-2868">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2868">Err</span></span> | <span data-ttu-id="4e5da-2869">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2869">Lo</span></span>  | <span data-ttu-id="4e5da-2870">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2870">Ob</span></span>  | 
| <span data-ttu-id="4e5da-2871">__米国__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2871">__US__</span></span> |    |    |    |    | <span data-ttu-id="4e5da-2872">US</span><span class="sxs-lookup"><span data-stu-id="4e5da-2872">US</span></span> | <span data-ttu-id="4e5da-2873">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-2873">In</span></span> | <span data-ttu-id="4e5da-2874">UI</span><span class="sxs-lookup"><span data-stu-id="4e5da-2874">UI</span></span> | <span data-ttu-id="4e5da-2875">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2875">Lo</span></span> | <span data-ttu-id="4e5da-2876">UL</span><span class="sxs-lookup"><span data-stu-id="4e5da-2876">UL</span></span> | <span data-ttu-id="4e5da-2877">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2877">Lo</span></span> | <span data-ttu-id="4e5da-2878">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2878">Lo</span></span> | <span data-ttu-id="4e5da-2879">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2879">Lo</span></span> | <span data-ttu-id="4e5da-2880">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2880">Err</span></span> | <span data-ttu-id="4e5da-2881">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2881">Err</span></span> | <span data-ttu-id="4e5da-2882">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2882">Lo</span></span>  | <span data-ttu-id="4e5da-2883">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2883">Ob</span></span>  | 
| <span data-ttu-id="4e5da-2884">__In__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2884">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="4e5da-2885">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-2885">In</span></span> | <span data-ttu-id="4e5da-2886">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2886">Lo</span></span> | <span data-ttu-id="4e5da-2887">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2887">Lo</span></span> | <span data-ttu-id="4e5da-2888">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2888">Lo</span></span> | <span data-ttu-id="4e5da-2889">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2889">Lo</span></span> | <span data-ttu-id="4e5da-2890">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2890">Lo</span></span> | <span data-ttu-id="4e5da-2891">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2891">Lo</span></span> | <span data-ttu-id="4e5da-2892">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2892">Err</span></span> | <span data-ttu-id="4e5da-2893">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2893">Err</span></span> | <span data-ttu-id="4e5da-2894">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2894">Lo</span></span>  | <span data-ttu-id="4e5da-2895">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2895">Ob</span></span>  | 
| <span data-ttu-id="4e5da-2896">__UI__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2896">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="4e5da-2897">UI</span><span class="sxs-lookup"><span data-stu-id="4e5da-2897">UI</span></span> | <span data-ttu-id="4e5da-2898">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2898">Lo</span></span> | <span data-ttu-id="4e5da-2899">UL</span><span class="sxs-lookup"><span data-stu-id="4e5da-2899">UL</span></span> | <span data-ttu-id="4e5da-2900">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2900">Lo</span></span> | <span data-ttu-id="4e5da-2901">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2901">Lo</span></span> | <span data-ttu-id="4e5da-2902">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2902">Lo</span></span> | <span data-ttu-id="4e5da-2903">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2903">Err</span></span> | <span data-ttu-id="4e5da-2904">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2904">Err</span></span> | <span data-ttu-id="4e5da-2905">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2905">Lo</span></span>  | <span data-ttu-id="4e5da-2906">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2906">Ob</span></span>  | 
| <span data-ttu-id="4e5da-2907">__Lo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2907">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2908">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2908">Lo</span></span> | <span data-ttu-id="4e5da-2909">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2909">Lo</span></span> | <span data-ttu-id="4e5da-2910">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2910">Lo</span></span> | <span data-ttu-id="4e5da-2911">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2911">Lo</span></span> | <span data-ttu-id="4e5da-2912">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2912">Lo</span></span> | <span data-ttu-id="4e5da-2913">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2913">Err</span></span> | <span data-ttu-id="4e5da-2914">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2914">Err</span></span> | <span data-ttu-id="4e5da-2915">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2915">Lo</span></span>  | <span data-ttu-id="4e5da-2916">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2916">Ob</span></span>  | 
| <span data-ttu-id="4e5da-2917">__UL__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2917">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2918">UL</span><span class="sxs-lookup"><span data-stu-id="4e5da-2918">UL</span></span> | <span data-ttu-id="4e5da-2919">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2919">Lo</span></span> | <span data-ttu-id="4e5da-2920">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2920">Lo</span></span> | <span data-ttu-id="4e5da-2921">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2921">Lo</span></span> | <span data-ttu-id="4e5da-2922">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2922">Err</span></span> | <span data-ttu-id="4e5da-2923">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2923">Err</span></span> | <span data-ttu-id="4e5da-2924">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2924">Lo</span></span>  | <span data-ttu-id="4e5da-2925">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2925">Ob</span></span>  | 
| <span data-ttu-id="4e5da-2926">__de__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2926">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2927">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2927">Lo</span></span> | <span data-ttu-id="4e5da-2928">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2928">Lo</span></span> | <span data-ttu-id="4e5da-2929">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2929">Lo</span></span> | <span data-ttu-id="4e5da-2930">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2930">Err</span></span> | <span data-ttu-id="4e5da-2931">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2931">Err</span></span> | <span data-ttu-id="4e5da-2932">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2932">Lo</span></span>  | <span data-ttu-id="4e5da-2933">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2933">Ob</span></span>  | 
| <span data-ttu-id="4e5da-2934">__si__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2934">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2935">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2935">Lo</span></span> | <span data-ttu-id="4e5da-2936">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2936">Lo</span></span> | <span data-ttu-id="4e5da-2937">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2937">Err</span></span> | <span data-ttu-id="4e5da-2938">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2938">Err</span></span> | <span data-ttu-id="4e5da-2939">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2939">Lo</span></span>  | <span data-ttu-id="4e5da-2940">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2940">Ob</span></span>  | 
| <span data-ttu-id="4e5da-2941">__Do__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2941">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2942">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2942">Lo</span></span> | <span data-ttu-id="4e5da-2943">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2943">Err</span></span> | <span data-ttu-id="4e5da-2944">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2944">Err</span></span> | <span data-ttu-id="4e5da-2945">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2945">Lo</span></span>  | <span data-ttu-id="4e5da-2946">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2946">Ob</span></span>  | 
| <span data-ttu-id="4e5da-2947">__Da__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2947">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-2948">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2948">Err</span></span> | <span data-ttu-id="4e5da-2949">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2949">Err</span></span> | <span data-ttu-id="4e5da-2950">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2950">Err</span></span> | <span data-ttu-id="4e5da-2951">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2951">Err</span></span> | 
| <span data-ttu-id="4e5da-2952">__ch__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2952">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="4e5da-2953">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2953">Err</span></span> | <span data-ttu-id="4e5da-2954">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2954">Err</span></span> | <span data-ttu-id="4e5da-2955">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-2955">Err</span></span> | 
| <span data-ttu-id="4e5da-2956">__st__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2956">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="4e5da-2957">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2957">Lo</span></span>  | <span data-ttu-id="4e5da-2958">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2958">Ob</span></span>  | 
| <span data-ttu-id="4e5da-2959">__Ob__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2959">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="4e5da-2960">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-2960">Ob</span></span>  | 


### <a name="short-circuiting-logical-operators"></a><span data-ttu-id="4e5da-2961">ショート サーキット論理演算子</span><span class="sxs-lookup"><span data-stu-id="4e5da-2961">Short-circuiting Logical Operators</span></span>

<span data-ttu-id="4e5da-2962">`AndAlso`と`OrElse`演算子はショート サーキットのバージョンの`And`と`Or`論理演算子です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2962">The `AndAlso` and `OrElse` operators are the short-circuiting versions of the `And` and `Or` logical operators.</span></span>

```antlr
ShortCircuitLogicalOperatorExpression
    : Expression 'AndAlso' LineTerminator? Expression
    | Expression 'OrElse' LineTerminator? Expression
    ;
```

<span data-ttu-id="4e5da-2963">ショート サーキット動作、ため 2 番目のオペランドは評価されません実行時に最初のオペランドを評価した後、演算子の結果がわかっている場合。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2963">Because of their short circuiting behavior, the second operand is not evaluated at run time if the operator result is known after evaluating the first operand.</span></span>

<span data-ttu-id="4e5da-2964">ショート サーキット論理演算子は、次のように評価されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2964">The short-circuiting logical operators are evaluated as follows:</span></span>

* <span data-ttu-id="4e5da-2965">場合、最初のオペランドで、`AndAlso`操作の評価に`False`から True を返しますまたはその`IsFalse`演算子、式は、最初のオペランドを返します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2965">If the first operand in an `AndAlso` operation evaluates to `False` or returns True from its `IsFalse` operator, the expression returns its first operand.</span></span> <span data-ttu-id="4e5da-2966">それ以外の場合、2 番目のオペランドが評価され、論理`And`2 つの結果に対して操作を実行します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2966">Otherwise, the second operand is evaluated and a logical `And` operation is performed on the two results.</span></span>

* <span data-ttu-id="4e5da-2967">場合、最初のオペランドで、`OrElse`操作の評価に`True`から True を返しますまたはその`IsTrue`演算子、式は、最初のオペランドを返します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2967">If the first operand in an `OrElse` operation evaluates to `True` or returns True from its `IsTrue` operator, the expression returns its first operand.</span></span> <span data-ttu-id="4e5da-2968">それ以外の場合、2 番目のオペランドが評価され、論理`Or`その 2 つの結果に対して操作を実行します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2968">Otherwise, the second operand is evaluated and a logical `Or` operation is performed on its two results.</span></span>

<span data-ttu-id="4e5da-2969">`AndAlso`と`OrElse`型の演算子が定義されている`Boolean`、または任意の型の`T`次の演算子をオーバー ロードします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2969">The `AndAlso` and `OrElse` operators are defined for the type `Boolean`, or for any type `T` that overloads the following operators:</span></span>

```vb
Public Shared Operator IsTrue(op As T) As Boolean
Public Shared Operator IsFalse(op As T) As Boolean
```

<span data-ttu-id="4e5da-2970">対応するオーバー ロードと`And`または`Or`演算子。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2970">as well as overloading the corresponding `And` or `Or` operator:</span></span>

```vb
Public Shared Operator And(op1 As T, op2 As T) As T
Public Shared Operator Or(op1 As T, op2 As T) As T
```

<span data-ttu-id="4e5da-2971">評価するときに、`AndAlso`または`OrElse`演算子、最初のオペランドが 1 回だけ評価されると、2 番目のオペランドは、評価されず、1 回だけ評価します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2971">When evaluating the `AndAlso` or `OrElse` operators, the first operand is evaluated only once, and the second operand is either not evaluated or evaluated exactly once.</span></span> <span data-ttu-id="4e5da-2972">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2972">For example, consider the following code:</span></span>

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

<span data-ttu-id="4e5da-2973">次の結果が出力されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2973">It prints the following result:</span></span>

```
And: False True
Or: True False
AndAlso: False
OrElse: True
```

<span data-ttu-id="4e5da-2974">リフトの形式で、`AndAlso`と`OrElse`演算子、最初のオペランドが null 場合`Boolean?`、2 番目のオペランドが評価されますが、結果が null では常に`Boolean?`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-2974">In the lifted form of the `AndAlso` and `OrElse` operators, if the first operand was a null `Boolean?`, then the second operand is evaluated but the result is always a null `Boolean?`.</span></span>

<span data-ttu-id="4e5da-2975">__操作の種類:__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2975">__Operation Type:__</span></span>

|        | <span data-ttu-id="4e5da-2976">__bo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2976">__Bo__</span></span> | <span data-ttu-id="4e5da-2977">__SB__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2977">__SB__</span></span> | <span data-ttu-id="4e5da-2978">__によって__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2978">__By__</span></span> | <span data-ttu-id="4e5da-2979">__Sh__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2979">__Sh__</span></span> | <span data-ttu-id="4e5da-2980">__米国__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2980">__US__</span></span> | <span data-ttu-id="4e5da-2981">__In__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2981">__In__</span></span> | <span data-ttu-id="4e5da-2982">__UI__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2982">__UI__</span></span> | <span data-ttu-id="4e5da-2983">__Lo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2983">__Lo__</span></span> | <span data-ttu-id="4e5da-2984">__UL__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2984">__UL__</span></span> | <span data-ttu-id="4e5da-2985">__de__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2985">__De__</span></span> | <span data-ttu-id="4e5da-2986">__si__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2986">__Si__</span></span> | <span data-ttu-id="4e5da-2987">__Do__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2987">__Do__</span></span> | <span data-ttu-id="4e5da-2988">__Da__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2988">__Da__</span></span>  | <span data-ttu-id="4e5da-2989">__ch__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2989">__Ch__</span></span>  | <span data-ttu-id="4e5da-2990">__st__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2990">__St__</span></span> | <span data-ttu-id="4e5da-2991">__Ob__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2991">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="4e5da-2992">__bo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-2992">__Bo__</span></span> | <span data-ttu-id="4e5da-2993">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2993">Bo</span></span> | <span data-ttu-id="4e5da-2994">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2994">Bo</span></span> | <span data-ttu-id="4e5da-2995">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2995">Bo</span></span> | <span data-ttu-id="4e5da-2996">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2996">Bo</span></span> | <span data-ttu-id="4e5da-2997">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2997">Bo</span></span> | <span data-ttu-id="4e5da-2998">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2998">Bo</span></span> | <span data-ttu-id="4e5da-2999">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-2999">Bo</span></span> | <span data-ttu-id="4e5da-3000">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3000">Bo</span></span> | <span data-ttu-id="4e5da-3001">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3001">Bo</span></span> | <span data-ttu-id="4e5da-3002">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3002">Bo</span></span> | <span data-ttu-id="4e5da-3003">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3003">Bo</span></span> | <span data-ttu-id="4e5da-3004">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3004">Bo</span></span> | <span data-ttu-id="4e5da-3005">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3005">Err</span></span> | <span data-ttu-id="4e5da-3006">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3006">Err</span></span> | <span data-ttu-id="4e5da-3007">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3007">Bo</span></span>  | <span data-ttu-id="4e5da-3008">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-3008">Ob</span></span>  | 
| <span data-ttu-id="4e5da-3009">__SB__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3009">__SB__</span></span> |    | <span data-ttu-id="4e5da-3010">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3010">Bo</span></span> | <span data-ttu-id="4e5da-3011">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3011">Bo</span></span> | <span data-ttu-id="4e5da-3012">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3012">Bo</span></span> | <span data-ttu-id="4e5da-3013">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3013">Bo</span></span> | <span data-ttu-id="4e5da-3014">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3014">Bo</span></span> | <span data-ttu-id="4e5da-3015">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3015">Bo</span></span> | <span data-ttu-id="4e5da-3016">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3016">Bo</span></span> | <span data-ttu-id="4e5da-3017">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3017">Bo</span></span> | <span data-ttu-id="4e5da-3018">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3018">Bo</span></span> | <span data-ttu-id="4e5da-3019">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3019">Bo</span></span> | <span data-ttu-id="4e5da-3020">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3020">Bo</span></span> | <span data-ttu-id="4e5da-3021">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3021">Err</span></span> | <span data-ttu-id="4e5da-3022">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3022">Err</span></span> | <span data-ttu-id="4e5da-3023">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3023">Bo</span></span>  | <span data-ttu-id="4e5da-3024">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-3024">Ob</span></span>  | 
| <span data-ttu-id="4e5da-3025">__によって__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3025">__By__</span></span> |    |    | <span data-ttu-id="4e5da-3026">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3026">Bo</span></span> | <span data-ttu-id="4e5da-3027">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3027">Bo</span></span> | <span data-ttu-id="4e5da-3028">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3028">Bo</span></span> | <span data-ttu-id="4e5da-3029">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3029">Bo</span></span> | <span data-ttu-id="4e5da-3030">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3030">Bo</span></span> | <span data-ttu-id="4e5da-3031">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3031">Bo</span></span> | <span data-ttu-id="4e5da-3032">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3032">Bo</span></span> | <span data-ttu-id="4e5da-3033">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3033">Bo</span></span> | <span data-ttu-id="4e5da-3034">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3034">Bo</span></span> | <span data-ttu-id="4e5da-3035">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3035">Bo</span></span> | <span data-ttu-id="4e5da-3036">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3036">Err</span></span> | <span data-ttu-id="4e5da-3037">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3037">Err</span></span> | <span data-ttu-id="4e5da-3038">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3038">Bo</span></span>  | <span data-ttu-id="4e5da-3039">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-3039">Ob</span></span>  | 
| <span data-ttu-id="4e5da-3040">__Sh__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3040">__Sh__</span></span> |    |    |    | <span data-ttu-id="4e5da-3041">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3041">Bo</span></span> | <span data-ttu-id="4e5da-3042">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3042">Bo</span></span> | <span data-ttu-id="4e5da-3043">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3043">Bo</span></span> | <span data-ttu-id="4e5da-3044">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3044">Bo</span></span> | <span data-ttu-id="4e5da-3045">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3045">Bo</span></span> | <span data-ttu-id="4e5da-3046">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3046">Bo</span></span> | <span data-ttu-id="4e5da-3047">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3047">Bo</span></span> | <span data-ttu-id="4e5da-3048">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3048">Bo</span></span> | <span data-ttu-id="4e5da-3049">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3049">Bo</span></span> | <span data-ttu-id="4e5da-3050">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3050">Err</span></span> | <span data-ttu-id="4e5da-3051">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3051">Err</span></span> | <span data-ttu-id="4e5da-3052">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3052">Bo</span></span>  | <span data-ttu-id="4e5da-3053">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-3053">Ob</span></span>  | 
| <span data-ttu-id="4e5da-3054">__米国__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3054">__US__</span></span> |    |    |    |    | <span data-ttu-id="4e5da-3055">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3055">Bo</span></span> | <span data-ttu-id="4e5da-3056">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3056">Bo</span></span> | <span data-ttu-id="4e5da-3057">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3057">Bo</span></span> | <span data-ttu-id="4e5da-3058">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3058">Bo</span></span> | <span data-ttu-id="4e5da-3059">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3059">Bo</span></span> | <span data-ttu-id="4e5da-3060">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3060">Bo</span></span> | <span data-ttu-id="4e5da-3061">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3061">Bo</span></span> | <span data-ttu-id="4e5da-3062">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3062">Bo</span></span> | <span data-ttu-id="4e5da-3063">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3063">Err</span></span> | <span data-ttu-id="4e5da-3064">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3064">Err</span></span> | <span data-ttu-id="4e5da-3065">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3065">Bo</span></span>  | <span data-ttu-id="4e5da-3066">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-3066">Ob</span></span>  | 
| <span data-ttu-id="4e5da-3067">__In__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3067">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="4e5da-3068">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3068">Bo</span></span> | <span data-ttu-id="4e5da-3069">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3069">Bo</span></span> | <span data-ttu-id="4e5da-3070">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3070">Bo</span></span> | <span data-ttu-id="4e5da-3071">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3071">Bo</span></span> | <span data-ttu-id="4e5da-3072">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3072">Bo</span></span> | <span data-ttu-id="4e5da-3073">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3073">Bo</span></span> | <span data-ttu-id="4e5da-3074">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3074">Bo</span></span> | <span data-ttu-id="4e5da-3075">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3075">Err</span></span> | <span data-ttu-id="4e5da-3076">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3076">Err</span></span> | <span data-ttu-id="4e5da-3077">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3077">Bo</span></span>  | <span data-ttu-id="4e5da-3078">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-3078">Ob</span></span>  | 
| <span data-ttu-id="4e5da-3079">__UI__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3079">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="4e5da-3080">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3080">Bo</span></span> | <span data-ttu-id="4e5da-3081">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3081">Bo</span></span> | <span data-ttu-id="4e5da-3082">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3082">Bo</span></span> | <span data-ttu-id="4e5da-3083">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3083">Bo</span></span> | <span data-ttu-id="4e5da-3084">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3084">Bo</span></span> | <span data-ttu-id="4e5da-3085">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3085">Bo</span></span> | <span data-ttu-id="4e5da-3086">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3086">Err</span></span> | <span data-ttu-id="4e5da-3087">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3087">Err</span></span> | <span data-ttu-id="4e5da-3088">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3088">Bo</span></span>  | <span data-ttu-id="4e5da-3089">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-3089">Ob</span></span>  | 
| <span data-ttu-id="4e5da-3090">__Lo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3090">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-3091">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3091">Bo</span></span> | <span data-ttu-id="4e5da-3092">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3092">Bo</span></span> | <span data-ttu-id="4e5da-3093">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3093">Bo</span></span> | <span data-ttu-id="4e5da-3094">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3094">Bo</span></span> | <span data-ttu-id="4e5da-3095">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3095">Bo</span></span> | <span data-ttu-id="4e5da-3096">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3096">Err</span></span> | <span data-ttu-id="4e5da-3097">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3097">Err</span></span> | <span data-ttu-id="4e5da-3098">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3098">Bo</span></span>  | <span data-ttu-id="4e5da-3099">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-3099">Ob</span></span>  | 
| <span data-ttu-id="4e5da-3100">__UL__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3100">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-3101">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3101">Bo</span></span> | <span data-ttu-id="4e5da-3102">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3102">Bo</span></span> | <span data-ttu-id="4e5da-3103">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3103">Bo</span></span> | <span data-ttu-id="4e5da-3104">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3104">Bo</span></span> | <span data-ttu-id="4e5da-3105">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3105">Err</span></span> | <span data-ttu-id="4e5da-3106">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3106">Err</span></span> | <span data-ttu-id="4e5da-3107">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3107">Bo</span></span>  | <span data-ttu-id="4e5da-3108">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-3108">Ob</span></span>  | 
| <span data-ttu-id="4e5da-3109">__de__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3109">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-3110">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3110">Bo</span></span> | <span data-ttu-id="4e5da-3111">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3111">Bo</span></span> | <span data-ttu-id="4e5da-3112">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3112">Bo</span></span> | <span data-ttu-id="4e5da-3113">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3113">Err</span></span> | <span data-ttu-id="4e5da-3114">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3114">Err</span></span> | <span data-ttu-id="4e5da-3115">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3115">Bo</span></span>  | <span data-ttu-id="4e5da-3116">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-3116">Ob</span></span>  | 
| <span data-ttu-id="4e5da-3117">__si__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3117">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-3118">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3118">Bo</span></span> | <span data-ttu-id="4e5da-3119">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3119">Bo</span></span> | <span data-ttu-id="4e5da-3120">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3120">Err</span></span> | <span data-ttu-id="4e5da-3121">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3121">Err</span></span> | <span data-ttu-id="4e5da-3122">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3122">Bo</span></span>  | <span data-ttu-id="4e5da-3123">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-3123">Ob</span></span>  | 
| <span data-ttu-id="4e5da-3124">__Do__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3124">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-3125">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3125">Bo</span></span> | <span data-ttu-id="4e5da-3126">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3126">Err</span></span> | <span data-ttu-id="4e5da-3127">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3127">Err</span></span> | <span data-ttu-id="4e5da-3128">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3128">Bo</span></span>  | <span data-ttu-id="4e5da-3129">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-3129">Ob</span></span>  | 
| <span data-ttu-id="4e5da-3130">__Da__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3130">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="4e5da-3131">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3131">Err</span></span> | <span data-ttu-id="4e5da-3132">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3132">Err</span></span> | <span data-ttu-id="4e5da-3133">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3133">Err</span></span> | <span data-ttu-id="4e5da-3134">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3134">Err</span></span> | 
| <span data-ttu-id="4e5da-3135">__ch__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3135">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="4e5da-3136">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3136">Err</span></span> | <span data-ttu-id="4e5da-3137">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3137">Err</span></span> | <span data-ttu-id="4e5da-3138">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3138">Err</span></span> | 
| <span data-ttu-id="4e5da-3139">__st__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3139">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="4e5da-3140">bo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3140">Bo</span></span>  | <span data-ttu-id="4e5da-3141">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-3141">Ob</span></span>  | 
| <span data-ttu-id="4e5da-3142">__Ob__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3142">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="4e5da-3143">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-3143">Ob</span></span>  | 


## <a name="shift-operators"></a><span data-ttu-id="4e5da-3144">シフト演算子</span><span class="sxs-lookup"><span data-stu-id="4e5da-3144">Shift Operators</span></span>

<span data-ttu-id="4e5da-3145">二項演算子`<<`と`>>`ビット シフト演算を実行します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3145">The binary operators `<<` and `>>` perform bit shifting operations.</span></span>

```antlr
ShiftOperatorExpression
    : Expression '<' '<' LineTerminator? Expression
    | Expression '>' '>' LineTerminator? Expression
    ;
```

<span data-ttu-id="4e5da-3146">演算子が定義されている、 `Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、`ULong`と`Long`型。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3146">The operators are defined for the `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` and `Long` types.</span></span> <span data-ttu-id="4e5da-3147">他の二項演算子とは異なり、演算子は左のオペランドだけの単項演算子が場合と、シフト演算の結果の型が決まります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3147">Unlike the other binary operators, the result type of a shift operation is determined as if the operator was a unary operator with just the left operand.</span></span> <span data-ttu-id="4e5da-3148">右側のオペランドの型に暗黙的に変換できる必要があります`Integer`され、操作の結果の型を決定するのには使用されません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3148">The type of the right operand must be implicitly convertible to `Integer` and is not used in determining the result type of the operation.</span></span>

<span data-ttu-id="4e5da-3149">`<<`演算子は左シフト数で指定された位置の数のシフトするときは、最初のオペランドでビットを発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3149">The `<<` operator causes the bits in the first operand to be shifted left the number of places specified by the shift amount.</span></span> <span data-ttu-id="4e5da-3150">結果の型の範囲外の上位ビットは破棄され、下位の空いたビット位置はゼロで埋められます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3150">The high-order bits outside the range of the result type are discarded and the low-order vacated bit positions are zero-filled.</span></span>

<span data-ttu-id="4e5da-3151">`>>`演算子は、シフトするときは、最初のオペランドでビット右シフト数で指定された位置の数。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3151">The `>>` operator causes the bits in the first operand to be shifted right the number of places specified by the shift amount.</span></span> <span data-ttu-id="4e5da-3152">下位ビットは破棄され、高位の空いたビット位置は、左側のオペランドが正の場合は 0 または負の値のいずれかに設定されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3152">The low-order bits are discarded and the high-order vacated bit positions are set to zero if the left operand is positive or to one if negative.</span></span> <span data-ttu-id="4e5da-3153">左側のオペランドの型の場合`Byte`、 `UShort`、 `UInteger`、または`ULong`空いたビットはゼロで埋められます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3153">If the left operand is of type `Byte`, `UShort`, `UInteger`, or `ULong` the vacant high-order bits are zero-filled.</span></span>

<span data-ttu-id="4e5da-3154">シフト演算子では、2 番目のオペランドの量によっては、最初のオペランドの基になる表現のビットをシフトします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3154">The shift operators shift the bits of the underlying representation of the first operand by the amount of the second operand.</span></span> <span data-ttu-id="4e5da-3155">としてシフト数が計算されたかどうかは、2 番目のオペランドの値が最初のオペランドのビット数より大きいまたは負の値、`RightOperand And SizeMask`場所`SizeMask`は。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3155">If the value of the second operand is greater than the number of bits in the first operand, or is negative, then the shift amount is computed as `RightOperand And SizeMask` where `SizeMask` is:</span></span>

| <span data-ttu-id="4e5da-3156">__演算子の種類__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3156">__LeftOperand Type__</span></span>  | <span data-ttu-id="4e5da-3157">__SizeMask__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3157">__SizeMask__</span></span> | 
|-----------------------|--------------|
| <span data-ttu-id="4e5da-3158">`Byte`, `SByte`</span><span class="sxs-lookup"><span data-stu-id="4e5da-3158">`Byte`, `SByte`</span></span>       | <span data-ttu-id="4e5da-3159">7 (`&H7`)</span><span class="sxs-lookup"><span data-stu-id="4e5da-3159">7 (`&H7`)</span></span>    | 
| <span data-ttu-id="4e5da-3160">`UShort`, `Short`</span><span class="sxs-lookup"><span data-stu-id="4e5da-3160">`UShort`, `Short`</span></span>     | <span data-ttu-id="4e5da-3161">15 (`&HF`)</span><span class="sxs-lookup"><span data-stu-id="4e5da-3161">15 (`&HF`)</span></span>   | 
| <span data-ttu-id="4e5da-3162">`UInteger`, `Integer`</span><span class="sxs-lookup"><span data-stu-id="4e5da-3162">`UInteger`, `Integer`</span></span> | <span data-ttu-id="4e5da-3163">31 (`&H1F`)</span><span class="sxs-lookup"><span data-stu-id="4e5da-3163">31 (`&H1F`)</span></span>  | 
| <span data-ttu-id="4e5da-3164">`ULong`, `Long`</span><span class="sxs-lookup"><span data-stu-id="4e5da-3164">`ULong`, `Long`</span></span>       | <span data-ttu-id="4e5da-3165">63 (`&H3F`)</span><span class="sxs-lookup"><span data-stu-id="4e5da-3165">63 (`&H3F`)</span></span>  | 

<span data-ttu-id="4e5da-3166">シフト数が 0 の場合、操作の結果は最初のオペランドの値と同じです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3166">If the shift amount is zero, the result of the operation is identical to the value of the first operand.</span></span> <span data-ttu-id="4e5da-3167">オーバーフローは、これらの操作からではありません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3167">No overflows are possible from these operations.</span></span>

<span data-ttu-id="4e5da-3168">__操作の種類:__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3168">__Operation Type:__</span></span>


| <span data-ttu-id="4e5da-3169">__bo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3169">__Bo__</span></span> | <span data-ttu-id="4e5da-3170">__SB__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3170">__SB__</span></span> | <span data-ttu-id="4e5da-3171">__によって__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3171">__By__</span></span> | <span data-ttu-id="4e5da-3172">__Sh__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3172">__Sh__</span></span> | <span data-ttu-id="4e5da-3173">__米国__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3173">__US__</span></span> | <span data-ttu-id="4e5da-3174">__In__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3174">__In__</span></span> | <span data-ttu-id="4e5da-3175">__UI__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3175">__UI__</span></span> | <span data-ttu-id="4e5da-3176">__Lo__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3176">__Lo__</span></span> | <span data-ttu-id="4e5da-3177">__UL__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3177">__UL__</span></span> | <span data-ttu-id="4e5da-3178">__de__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3178">__De__</span></span> | <span data-ttu-id="4e5da-3179">__si__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3179">__Si__</span></span> | <span data-ttu-id="4e5da-3180">__Do__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3180">__Do__</span></span> | <span data-ttu-id="4e5da-3181">__Da__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3181">__Da__</span></span>  | <span data-ttu-id="4e5da-3182">__ch__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3182">__Ch__</span></span>  | <span data-ttu-id="4e5da-3183">__st__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3183">__St__</span></span> | <span data-ttu-id="4e5da-3184">__Ob__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3184">__Ob__</span></span> | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="4e5da-3185">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-3185">Sh</span></span> | <span data-ttu-id="4e5da-3186">SB</span><span class="sxs-lookup"><span data-stu-id="4e5da-3186">SB</span></span> | <span data-ttu-id="4e5da-3187">By</span><span class="sxs-lookup"><span data-stu-id="4e5da-3187">By</span></span> | <span data-ttu-id="4e5da-3188">Sh</span><span class="sxs-lookup"><span data-stu-id="4e5da-3188">Sh</span></span> | <span data-ttu-id="4e5da-3189">US</span><span class="sxs-lookup"><span data-stu-id="4e5da-3189">US</span></span> | <span data-ttu-id="4e5da-3190">イン</span><span class="sxs-lookup"><span data-stu-id="4e5da-3190">In</span></span> | <span data-ttu-id="4e5da-3191">UI</span><span class="sxs-lookup"><span data-stu-id="4e5da-3191">UI</span></span> | <span data-ttu-id="4e5da-3192">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3192">Lo</span></span> | <span data-ttu-id="4e5da-3193">UL</span><span class="sxs-lookup"><span data-stu-id="4e5da-3193">UL</span></span> | <span data-ttu-id="4e5da-3194">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3194">Lo</span></span> | <span data-ttu-id="4e5da-3195">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3195">Lo</span></span> | <span data-ttu-id="4e5da-3196">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3196">Lo</span></span> | <span data-ttu-id="4e5da-3197">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3197">Err</span></span> | <span data-ttu-id="4e5da-3198">Err</span><span class="sxs-lookup"><span data-stu-id="4e5da-3198">Err</span></span> | <span data-ttu-id="4e5da-3199">Lo</span><span class="sxs-lookup"><span data-stu-id="4e5da-3199">Lo</span></span> | <span data-ttu-id="4e5da-3200">Ob</span><span class="sxs-lookup"><span data-stu-id="4e5da-3200">Ob</span></span> | 


## <a name="boolean-expressions"></a><span data-ttu-id="4e5da-3201">Boolean 式</span><span class="sxs-lookup"><span data-stu-id="4e5da-3201">Boolean Expressions</span></span>

<span data-ttu-id="4e5da-3202">ブール式では、true の場合、または false の場合にテストできる式です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3202">A Boolean expression is an expression that can be tested to see if it is true or if it is false.</span></span>

```antlr
BooleanExpression
    : Expression
    ;
```

<span data-ttu-id="4e5da-3203">型`T`ブール式で使用できる場合は、優先順位で。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3203">A type `T` can be used in a Boolean expression if, in order of preference:</span></span>

* <span data-ttu-id="4e5da-3204">`T` が `Boolean` または `Boolean?` です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3204">`T` is `Boolean` or `Boolean?`</span></span>

* <span data-ttu-id="4e5da-3205">`T` 拡大変換するには `Boolean`</span><span class="sxs-lookup"><span data-stu-id="4e5da-3205">`T` has a widening conversion to `Boolean`</span></span>

* <span data-ttu-id="4e5da-3206">`T` 拡大変換するには `Boolean?`</span><span class="sxs-lookup"><span data-stu-id="4e5da-3206">`T` has a widening conversion to `Boolean?`</span></span>

* <span data-ttu-id="4e5da-3207">`T` 2 つの擬似演算子を定義します。`IsTrue`と`IsFalse`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3207">`T` defines two pseudo operators, `IsTrue` and `IsFalse`.</span></span>

* <span data-ttu-id="4e5da-3208">`T` 縮小変換が`Boolean?`からの変換を伴わない`Boolean`に`Boolean?`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3208">`T` has a narrowing conversion to `Boolean?` that does not involve a conversion from `Boolean` to `Boolean?`.</span></span>

* <span data-ttu-id="4e5da-3209">`T` 縮小変換が`Boolean`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3209">`T` has a narrowing conversion to `Boolean`.</span></span>

<span data-ttu-id="4e5da-3210">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3210">__Note.__</span></span> <span data-ttu-id="4e5da-3211">されている場合に興味深いは、`Option Strict`式への縮小変換は、オフ`Boolean`、コンパイル時エラーが、言語はそれでも選択せずに受け入れ、`IsTrue`演算子が存在する場合。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3211">It is interesting to note that if `Option Strict` is off, an expression that has a narrowing conversion to `Boolean` will be accepted without a compile-time error but the language will still prefer an `IsTrue` operator if it exists.</span></span> <span data-ttu-id="4e5da-3212">これは、ため`Option Strict`が、言語によって受け入れられないし、式の実際の意味を決して変更内容が変わるだけです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3212">This is because `Option Strict` only changes what is and isn't accepted by the language, and never changes the actual meaning of an expression.</span></span> <span data-ttu-id="4e5da-3213">したがって、`IsTrue`縮小変換では、上に関係なく常に優先されますが、`Option Strict`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3213">Thus, `IsTrue` has to always be preferred over a narrowing conversion, regardless of `Option Strict`.</span></span>

<span data-ttu-id="4e5da-3214">たとえば、次のクラスがへの拡大変換を定義しません`Boolean`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3214">For example, the following class does not define a widening conversion to `Boolean`.</span></span> <span data-ttu-id="4e5da-3215">その結果での使用、`If`ステートメントへの呼び出しがあると、`IsTrue`演算子。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3215">As a result, its use in the `If` statement causes a call to the `IsTrue` operator.</span></span>

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

<span data-ttu-id="4e5da-3216">ブール式として型指定されたかどうかに変換または`Boolean`または`Boolean?`、値の場合は true が`True`false、それ以外の場合。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3216">If a Boolean expression is typed as or converted to `Boolean` or `Boolean?`, then it is true if the value is `True` and false otherwise.</span></span>

<span data-ttu-id="4e5da-3217">それ以外の場合、ブール式を呼び出す、`IsTrue`演算子を返す`True`演算子は、返された場合`True`; それ以外の場合は false (呼び出すことはありませんが、`IsFalse`演算子)。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3217">Otherwise, a Boolean expression calls the `IsTrue` operator and returns `True` if the operator returned `True`; otherwise it is false (but never calls the `IsFalse` operator).</span></span>

<span data-ttu-id="4e5da-3218">次の例では、`Integer`への縮小変換が`Boolean`ため、null`Integer?`両方への縮小変換が`Boolean?`(null 値を生成`Boolean`) および`Boolean`(例外をスローする)。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3218">In the following example, `Integer` has a narrowing conversion to `Boolean`, so a null `Integer?` has a narrowing conversion to both `Boolean?` (yielding a null `Boolean`) and to `Boolean` (which throws an exception).</span></span> <span data-ttu-id="4e5da-3219">縮小変換`Boolean?`をお勧めなどの値"`i`"をブール値として式はそのため、`False`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3219">The narrowing conversion to `Boolean?` is preferred, and so the value of "`i`" as a Boolean expression is therefore `False`.</span></span>

```vb
Dim i As Integer? = Nothing
If i Then Console.WriteLine()
```


## <a name="lambda-expressions"></a><span data-ttu-id="4e5da-3220">ラムダ式</span><span class="sxs-lookup"><span data-stu-id="4e5da-3220">Lambda Expressions</span></span>

<span data-ttu-id="4e5da-3221">A*ラムダ式*と呼ばれる、匿名メソッドを定義、*メソッドのラムダ*します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3221">A *lambda expression* defines an anonymous method called a *lambda method*.</span></span> <span data-ttu-id="4e5da-3222">ラムダ メソッドでは、"line"メソッドを他のデリゲート型を取るメソッドに渡すやすいようにします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3222">Lambda methods make it easy to pass "in-line" methods to other methods that take delegate types.</span></span>

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

<span data-ttu-id="4e5da-3223">例:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3223">The example:</span></span>

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

<span data-ttu-id="4e5da-3224">表示されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3224">will print out:</span></span>

```
2 4 6 8
```

<span data-ttu-id="4e5da-3225">ラムダ式は、オプションの修飾子で始まる`Async`または`Iterator`キーワードと、その後`Function`または`Sub`およびパラメーターのリスト。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3225">A lambda expression begins with the optional modifiers `Async` or `Iterator`, followed by the keyword `Function` or `Sub` and a parameter list.</span></span> <span data-ttu-id="4e5da-3226">ラムダ式でパラメーターを宣言することはできません`Optional`または`ParamArray`と属性を持つことはできません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3226">Parameters in a lambda expression cannot be declared `Optional` or `ParamArray` and cannot have attributes.</span></span> <span data-ttu-id="4e5da-3227">通常のメソッドとは異なり、ラムダ メソッドのパラメーターの型を省略すると自動的に推論しません`Object`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3227">Unlike regular methods, omitting a parameter type for a lambda method does not automatically infer `Object`.</span></span> <span data-ttu-id="4e5da-3228">代わりに、ときに、ラムダ メソッドは、再分類、省略されたパラメーターの型と`ByRef`修飾子は、対象の型から推論されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3228">Instead, when a lambda method is reclassified, the omitted parameter types and `ByRef` modifiers are inferred from the target type.</span></span> <span data-ttu-id="4e5da-3229">前の例では、ラムダ式が書き込まれたとして`Function(x) x * 2`の型推論は`x`する`Integer`ラムダ メソッドのインスタンスを作成に使用されたときに、`IntFunc`デリゲート型。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3229">In the previous example, the lambda expression could have been written as `Function(x) x * 2`, and it would have inferred the type of `x` to be `Integer` when the lambda method was used to create an instance of the `IntFunc` delegate type.</span></span> <span data-ttu-id="4e5da-3230">ローカル変数の推定とは異なりラムダ メソッドのパラメーターは、型が省略されますが、配列または null 許容型名の修飾子がある場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3230">Unlike local variable inference, if a lambda method parameter omits a type but has an array or nullable name modifier, a compile-time error occurs.</span></span>

<span data-ttu-id="4e5da-3231">A__正規のラムダ式__で、どちらも`Async`も`Iterator`修飾子。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3231">A __regular lambda expression__ is one with neither `Async` nor `Iterator` modifiers.</span></span>

<span data-ttu-id="4e5da-3232">__反復子ラムダ式__で 1 つは、`Iterator`修飾子と no`Async`修飾子。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3232">An __iterator lambda expression__ is one with the `Iterator` modifier and no `Async` modifier.</span></span> <span data-ttu-id="4e5da-3233">関数があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3233">It must be a function.</span></span> <span data-ttu-id="4e5da-3234">値に、再分類するがときに、のみ再分類できます戻り値の型がデリゲート型の値に`IEnumerator`、または`IEnumerable`、または`IEnumerator(Of T)`または`IEnumerable(Of T)`一部`T`が ByRef パラメーターを持たないとします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3234">When it is reclassified to a value, it can only be reclassified to a value of delegate type whose return type is `IEnumerator`, or `IEnumerable`, or `IEnumerator(Of T)` or `IEnumerable(Of T)` for some `T`, and which has no ByRef parameters.</span></span>

<span data-ttu-id="4e5da-3235">__非同期ラムダ式__で 1 つは、`Async`修飾子と no`Iterator`修飾子。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3235">An __async lambda expression__ is one with the `Async` modifier and no `Iterator` modifier.</span></span> <span data-ttu-id="4e5da-3236">非同期サブルーチンのラムダは、ByRef パラメーターなしでサブ デリゲート型の値にのみ再分類可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3236">An async sub lambda may only be reclassified to a value of sub delegate type with no ByRef parameters.</span></span> <span data-ttu-id="4e5da-3237">非同期のラムダ関数が戻り値の型は関数デリゲート型の値に再分類する可能性がありますのみ`Task`または`Task(Of T)`一部`T`が ByRef パラメーターを持たないとします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3237">An async function lambda may only be reclassified to a value of function delegate type whose return type is `Task` or `Task(Of T)` for some `T`, and which has no ByRef parameters.</span></span>

<span data-ttu-id="4e5da-3238">単一行または複数行ラムダ式はできますか。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3238">Lambda expressions can either be single-line or multi-line.</span></span> <span data-ttu-id="4e5da-3239">単一行`Function`ラムダ式は、ラムダ メソッドから返される値を表す 1 つの式を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3239">Single-line `Function` lambda expressions contain a single expression that represents the value returned from the lambda method.</span></span> <span data-ttu-id="4e5da-3240">単一行`Sub`ラムダ式は、その閉じずに 1 つのステートメントを含めることが`StatementTerminator`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3240">Single-line `Sub` lambda expressions contain a single statement without its closing `StatementTerminator`.</span></span> <span data-ttu-id="4e5da-3241">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3241">For example:</span></span>

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

<span data-ttu-id="4e5da-3242">単一行のラムダは、その他のすべての式およびステートメントよりも小さい緊密にバインドを構築します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3242">Single-line lambda constructs bind less tightly than all other expressions and statements.</span></span> <span data-ttu-id="4e5da-3243">たとえば、このため、"`Function() x + 5`「は等価である」`Function() (x+5)"`なく"`(Function() x) + 5`"。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3243">Thus, for example, "`Function() x + 5`" is equivalent to "`Function() (x+5)"` rather than "`(Function() x) + 5`".</span></span> <span data-ttu-id="4e5da-3244">単一行、あいまいさを回避するために`Sub`Dim ステートメントまたはラベルの宣言ステートメントが、ラムダ式に含まれていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3244">To avoid ambiguity, a single-line `Sub` lambda expression may not contain a Dim statement or a label declaration statement.</span></span> <span data-ttu-id="4e5da-3245">また、単一行、かっこで囲まれている場合を除き、`Sub`ラムダ式がない直後、コロンで":"、メンバー アクセス演算子"."、ディクショナリ メンバー アクセス演算子"!"またはかっこ"(".</span><span class="sxs-lookup"><span data-stu-id="4e5da-3245">Also, unless it is enclosed in parentheses, a single-line `Sub` lambda expression may not be immediately followed by a colon ":", a member access operator ".", a dictionary member access operator "!" or an open parenthesis "(".</span></span> <span data-ttu-id="4e5da-3246">任意のブロックのステートメントがない場合があります (`With`、 `SyncLock, If...EndIf`、 `While`、 `For`、 `Do`、 `Using`) も`OnError`も`Resume`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3246">It may not contain any block statement (`With`, `SyncLock, If...EndIf`, `While`, `For`, `Do`, `Using`) nor `OnError` nor `Resume`.</span></span>

<span data-ttu-id="4e5da-3247">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3247">__Note.__</span></span> <span data-ttu-id="4e5da-3248">ラムダ式で`Function(i) x=i`、本文として解釈されます、*式*(テストするかどうか`x`と`i`が等しい)。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3248">In the lambda expression `Function(i) x=i`, the body is interpreted as an *expression* (which tests whether `x` and `i` are equal).</span></span> <span data-ttu-id="4e5da-3249">ラムダ式で`Sub(i) x=i`、本文は、ステートメントとして解釈されます (どの割り当てます`i`に`x`)。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3249">But in the lambda expression `Sub(i) x=i`, the body is interpreted as a statement (which assigns `i` to `x`).</span></span>

<span data-ttu-id="4e5da-3250">複数行ラムダ式がステートメント ブロックが含まれ、適切な終了する必要があります`End`ステートメント (つまり`End Function`または`End Sub`)。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3250">A multi-line lambda expression contains a statement block and must end with an appropriate `End` statement (i.e. `End Function` or `End Sub`).</span></span> <span data-ttu-id="4e5da-3251">通常のメソッドを複数行ラムダ メソッドのと同様`Function`または`Sub`ステートメントと`End`ステートメントは、独自の行に配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3251">As with regular methods, a multi-line lambda method's `Function` or `Sub` statement and `End` statements must be on their own lines.</span></span> <span data-ttu-id="4e5da-3252">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3252">For example:</span></span>

```vb
' Error: Function statement must be on its own line!
Dim x = Sub(x As Integer) : Console.WriteLine(x) : End Sub

' OK
Dim y = Sub(x As Integer)
               Console.WriteLine(x)
          End Sub
```

<span data-ttu-id="4e5da-3253">複数行`Function`ラムダ式が戻り値の型を宣言できますが、上の属性を配置することはできません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3253">Multi-line `Function` lambda expressions can declare a return type but cannot put attributes on it.</span></span> <span data-ttu-id="4e5da-3254">複数行の場合`Function`ラムダ式が戻り値の型を宣言しませんが、ラムダ式を使用し、その戻り値の型が使用されるコンテキストから戻り値の型を推論することができます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3254">If a multi-line `Function` lambda expression does not declare a return type but the return type can be inferred from the context in which the lambda expression is used , then that return type is used.</span></span> <span data-ttu-id="4e5da-3255">それ以外の場合、関数の戻り値の型は、次のように計算します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3255">Otherwise the return type of the function is calculated as follows:</span></span>

* <span data-ttu-id="4e5da-3256">通常のラムダ式では、戻り値の型はすべての式の主要な型、`Return`ステートメント ブロック内のステートメント。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3256">In a regular lambda expression, the return type is the dominant type of the expressions in all the `Return` statements in the statement block.</span></span>

* <span data-ttu-id="4e5da-3257">非同期ラムダ式では、戻り値の型は`Task(Of T)`場所`T`内のすべての式の主要な型には、`Return`ステートメント ブロック内のステートメント。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3257">In an async lambda expression, the return type is `Task(Of T)` where `T` is the dominant type of the expressions in all the `Return` statements in the statement block.</span></span>

* <span data-ttu-id="4e5da-3258">反復子ラムダ式では、戻り値の型が`IEnumerable(Of T)`場所`T`内のすべての式の主要な型には、`Yield`ステートメント ブロック内のステートメント。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3258">In an iterator lambda expression, the return type is `IEnumerable(Of T)` where `T` is the dominant type of the expressions in all the `Yield` statements in the statement block.</span></span>

<span data-ttu-id="4e5da-3259">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3259">For example:</span></span>

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

<span data-ttu-id="4e5da-3260">ある場合は常にない`Return`(それぞれ`Yield`) ステートメント、または主要な型、それらの間ではなく、厳密な型が使用されている、コンパイル時エラーが発生します。 それ以外の場合、主要な型は、暗黙的に`Object`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3260">In all cases, if there are no `Return` (respectively `Yield`) statements, or if there is no dominant type among them, and strict semantics are being used, a compile-time error occurs; otherwise the dominant type is implicitly `Object`.</span></span>

<span data-ttu-id="4e5da-3261">すべての戻り値の型が計算されることに注意してください。`Return`ステートメント、到達可能でない場合でも。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3261">Note that the return type is calculated from all `Return` statements, even if they are not reachable.</span></span> <span data-ttu-id="4e5da-3262">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3262">For example:</span></span>

```vb
' Return type is Double
Dim x = Function()
              Return 10
               Return 10.50
          End Function
```

<span data-ttu-id="4e5da-3263">変数の名前が存在しないために、暗黙の戻り変数はありません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3263">There is no implicit return variable, as there is no name for the variable.</span></span>

<span data-ttu-id="4e5da-3264">複数行ラムダ式の中でステートメント ブロックには、次の制限があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3264">The statement blocks inside multi-line lambda expressions have the following restrictions:</span></span>

* <span data-ttu-id="4e5da-3265">`On Error` `Resume`ですが、ステートメントは使用できません`Try`ステートメントを使用できます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3265">`On Error` and `Resume` statements are not allowed, although `Try` statements are allowed.</span></span>

* <span data-ttu-id="4e5da-3266">複数行ラムダ式では、静的ローカル変数を宣言することはできません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3266">Static locals cannot be declared in multi-line lambda expressions.</span></span>

* <span data-ttu-id="4e5da-3267">そこに通常の分岐の規則が適用されますが、複数行ラムダ式のステートメント ブロックの内外に分岐することはできません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3267">It is not possible to branch into or out of the statement block of a multi-line lambda expression, although the normal branching rules apply within it.</span></span> <span data-ttu-id="4e5da-3268">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3268">For example:</span></span>

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

<span data-ttu-id="4e5da-3269">ラムダ式は、含んでいる型で宣言された匿名メソッドとほぼ同じです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3269">A lambda expression is roughly equivalent to an anonymous method declared on the containing type.</span></span> <span data-ttu-id="4e5da-3270">最初の例では、約と同等です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3270">The initial example is roughly equivalent to:</span></span>

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


### <a name="closures"></a><span data-ttu-id="4e5da-3271">クロージャ</span><span class="sxs-lookup"><span data-stu-id="4e5da-3271">Closures</span></span>

<span data-ttu-id="4e5da-3272">ラムダ式では、ローカル変数またはパラメーターを含むメソッドとラムダ式で定義されているなど、スコープですべての変数へのアクセスがあります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3272">Lambda expressions have access to all of the variables in scope, including local variables or parameters defined in the containing method and lambda expressions.</span></span> <span data-ttu-id="4e5da-3273">ラムダ式は、ローカル変数またはパラメーターを参照しているときに、ラムダ式はクロージャに参照される変数をキャプチャします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3273">When a lambda expression refers to a local variable or parameter, the lambda expression captures the variable being referred to into a closure.</span></span> <span data-ttu-id="4e5da-3274">クロージャは、スタック上の代わりに、ヒープ上に存在するオブジェクトとは、変数をキャプチャしたら、すべての参照を変数には、クロージャにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3274">A closure is an object that lives on the heap instead of on the stack, and when a variable is captured, all references to the variable are redirected to the closure.</span></span> <span data-ttu-id="4e5da-3275">これにより、ラムダ式を含むメソッドが完了した後でも、ローカル変数とパラメーターを参照してください。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3275">This enables lambda expressions to continue to refer to local variables and parameters even after the containing method is complete.</span></span> <span data-ttu-id="4e5da-3276">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3276">For example:</span></span>

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

<span data-ttu-id="4e5da-3277">約と同等です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3277">is roughly equivalent to:</span></span>

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

<span data-ttu-id="4e5da-3278">ローカルの新しいコピーをキャプチャするクロージャ変数をブロックに入るたびに、宣言されたローカル変数が存在する場合、新しいコピーが以前のコピーの値で初期化されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3278">A closure captures a new copy of a local variable each time it enters the block in which the local variable is declared, but the new copy is initialized with the value of the previous copy, if any.</span></span> <span data-ttu-id="4e5da-3279">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3279">For example:</span></span>

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

<span data-ttu-id="4e5da-3280">プリント</span><span class="sxs-lookup"><span data-stu-id="4e5da-3280">prints</span></span>

```
1 2 3 4 5 6 7 8 9 10
```

<span data-ttu-id="4e5da-3281">代わりに</span><span class="sxs-lookup"><span data-stu-id="4e5da-3281">instead of</span></span>

```
9 9 9 9 9 9 9 9 9 9
```

<span data-ttu-id="4e5da-3282">許可されていないブロックを入力するときに初期化するため、クロージャ`GoTo`から、そのブロックの外部でのクロージャでのブロックにすることができますが`Resume`クロージャでのブロックにします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3282">Because closures have to be initialized when entering a block, it is not allowed to `GoTo` into a block with a closure from outside of that block, although it is allowed to `Resume` into a block with a closure.</span></span> <span data-ttu-id="4e5da-3283">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3283">For example:</span></span>

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

<span data-ttu-id="4e5da-3284">これらは、クロージャにキャプチャすることはできません、ためには、ラムダ式の内部で、次のことはできません表示されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3284">Because they cannot be captured into a closure, the following cannot appear inside of a lambda expression:</span></span>

* <span data-ttu-id="4e5da-3285">参照パラメーター。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3285">Reference parameters.</span></span>

* <span data-ttu-id="4e5da-3286">式のインスタンス (`Me`、 `MyClass`、 `MyBase`) 場合は、型`Me`クラスではありません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3286">Instance expressions (`Me`, `MyClass`, `MyBase`), if the type of `Me` is not a class.</span></span>

<span data-ttu-id="4e5da-3287">ラムダ式が式の一部である場合、匿名型の作成式のメンバー。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3287">The members of an anonymous type-creation expression, if the lambda expression is part of the expression.</span></span> <span data-ttu-id="4e5da-3288">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3288">For example:</span></span>

```vb
' Error: Lambda cannot refer to anonymous type field
Dim x = New With { .a = 12, .b = Function() .a }
```

<span data-ttu-id="4e5da-3289">`ReadOnly` インスタンス コンス トラクターのインスタンス変数または`ReadOnly`値以外のコンテキストで、変数が使用されている共有のコンス トラクター内の変数を共有します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3289">`ReadOnly` instance variables in instance constructors or `ReadOnly` shared variables in shared constructors where the variables are used in a non-value context.</span></span> <span data-ttu-id="4e5da-3290">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3290">For example:</span></span>

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

## <a name="query-expressions"></a><span data-ttu-id="4e5da-3291">クエリ式</span><span class="sxs-lookup"><span data-stu-id="4e5da-3291">Query Expressions</span></span>

<span data-ttu-id="4e5da-3292">A*クエリ式*一連が適用される式は、*クエリ演算子*の要素に、*クエリ可能な*コレクション。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3292">A *query expression* is an expression that applies a series of *query operators* to the elements of a *queryable* collection.</span></span> <span data-ttu-id="4e5da-3293">たとえば、次の式のコレクションを受け取り`Customer`オブジェクトし、ワシントン州内のすべての顧客の名前を返します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3293">For example, the following expression takes a collection of `Customer` objects and returns the names of all the customers in the state of Washington:</span></span>

```vb
Dim names = _
    From cust In Customers _
    Where cust.State = "WA" _
    Select cust.Name
```

<span data-ttu-id="4e5da-3294">クエリ式で始まらなければなりません、`From`または`Aggregate`演算子のクエリ演算子で終わります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3294">A query expression must start with a `From` or an `Aggregate` operator and can end with any query operator.</span></span> <span data-ttu-id="4e5da-3295">クエリ式の結果値として分類されます。式の結果型は、式内の最後のクエリ演算子の結果の型に依存します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3295">The result of a query expression is classified as a value; the result type of the expression depends on the result type of the last query operator in the expression.</span></span>

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

### <a name="range-variables"></a><span data-ttu-id="4e5da-3296">範囲変数</span><span class="sxs-lookup"><span data-stu-id="4e5da-3296">Range Variables</span></span>

<span data-ttu-id="4e5da-3297">いくつかのクエリ演算子という名前の変数の特殊な種類の導入を*範囲変数*します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3297">Some query operators introduce a special kind of variable called a *range variable*.</span></span> <span data-ttu-id="4e5da-3298">範囲変数が実際の変数です。代わりに、個々 の値は、クエリの評価中に、入力コレクションに対してこれら表します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3298">Range variables are not real variables; instead, they represent the individual values during the evaluation of the query over the input collections.</span></span>

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

<span data-ttu-id="4e5da-3299">範囲変数のスコープは、クエリ演算子の概要から、クエリ式の末尾に、またはクエリ演算子になど`Select`を非表示にします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3299">Range variables are scoped from the introducing query operator to the end of a query expression, or to a query operator such as `Select` that hides them.</span></span> <span data-ttu-id="4e5da-3300">たとえば、次のクエリで</span><span class="sxs-lookup"><span data-stu-id="4e5da-3300">For example, in the following query</span></span>

```vb
Dim waCusts = _
    From cust As Customer In Customers _
    Where cust.State = "WA"
```

<span data-ttu-id="4e5da-3301">`From`クエリ操作は、範囲変数を導入`cust`として型指定された`Customer`では、各顧客を表す、`Customers`コレクション。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3301">the `From` query operator introduces a range variable `cust` typed as `Customer` that represents each customer in the `Customers` collection.</span></span> <span data-ttu-id="4e5da-3302">次`Where`クエリ演算子は、範囲変数を参照`cust`結果のコレクションから個々 のユーザーをフィルター処理するかどうかを決定するフィルター式で。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3302">The following `Where` query operator then refers to the range variable `cust` in the filter expression to determine whether to filter an individual customer out of the resulting collection.</span></span>

<span data-ttu-id="4e5da-3303">範囲変数の 2 種類があります:*コレクションの範囲変数*と*式の範囲変数*します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3303">There are two types of range variables: *collection range variables* and *expression range variables*.</span></span> <span data-ttu-id="4e5da-3304">コレクションの範囲変数は、クエリ対象のコレクションの要素からその値を取得します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3304">Collection range variables take their values from the elements of the collections being queried.</span></span> <span data-ttu-id="4e5da-3305">コレクションの範囲変数宣言でコレクションの式は、型がクエリ可能な値として分類する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3305">The collection expression in a collection range variable declaration must be classified as a value whose type is queryable.</span></span> <span data-ttu-id="4e5da-3306">コレクションの範囲変数の型を省略すると、コレクションの式の要素の型を推論または`Object`コレクション式が要素の型を持たないかどうか (つまりのみを定義、`Cast`メソッド)。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3306">If the type of a collection range variable is omitted, it is inferred to be the element type of the collection expression, or `Object` if the collection expression does not have an element type (i.e. only defines a `Cast` method).</span></span> <span data-ttu-id="4e5da-3307">コレクション式がクエリ可能な場合 (つまり、コレクションの要素の型が推測できない)、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3307">If the collection expression is not queryable (i.e. the element type of the collection cannot be inferred), a compile-time error results.</span></span>

<span data-ttu-id="4e5da-3308">式の範囲変数は、範囲変数がその値がコレクションではなく、式で計算されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3308">An expression range variable is a range variable whose value is calculated by an expression rather than a collection.</span></span> <span data-ttu-id="4e5da-3309">次の例では、`Select`クエリ演算子という式の範囲変数が導入されています`cityState`2 つのフィールドから計算されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3309">In the following example, the `Select` query operator introduces an expression range variable named `cityState` calculated from two fields:</span></span>

```vb
Dim cityStates = _
    From cust As Customer In Customers _
    Select cityState = cust.City & "," & cust.State _
    Where cityState.Length() < 10
```

<span data-ttu-id="4e5da-3310">式の範囲変数は、疑わしい値のような変数があります。 別の範囲変数を参照する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3310">An expression range variable is not required to reference another range variable, although such a variable may be of dubious value.</span></span> <span data-ttu-id="4e5da-3311">式の範囲変数に割り当てられた式は、値として分類する必要があり、指定した場合、範囲変数の型に暗黙的に変換できる必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3311">The expression assigned to an expression range variable must be classified as a value and must be implicitly convertible to the type of the range variable, if given.</span></span>

<span data-ttu-id="4e5da-3312">Let 演算子でのみがあります式の範囲変数をその型を指定します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3312">Only in a Let operator may an expression range variable have its type specified.</span></span> <span data-ttu-id="4e5da-3313">その他の演算子でまたはその型が指定されていないかどうかは、ローカル変数の型推論を使用して、範囲変数の種類を決定します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3313">In other operators, or if its type is not specified, then local variable type inference is used to determine the type of the range variable.</span></span>

<span data-ttu-id="4e5da-3314">範囲変数は、シャドウする点では、ローカル変数の宣言に関する規則に従う必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3314">A range variable must follow the rules for declaring local variables in respect to shadowing.</span></span> <span data-ttu-id="4e5da-3315">そのため、範囲変数は、(具体的には、クエリ演算子には、スコープ内の現在の範囲変数がすべて非表示になります) しない限り、ローカル変数または外側のメソッドまたは別の範囲変数のパラメーターの名前を隠すことはできません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3315">Thus, a range variable cannot hide the name of a local variable or parameter in the enclosing method or another range variable (unless the query operator specifically hides all current range variables in scope).</span></span>


### <a name="queryable-types"></a><span data-ttu-id="4e5da-3316">クエリ可能型</span><span class="sxs-lookup"><span data-stu-id="4e5da-3316">Queryable Types</span></span>

<span data-ttu-id="4e5da-3317">クエリ式は、よく知られているコレクション型メソッドを呼び出すには、式を変換することによって実装されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3317">Query expressions are implemented by translating the expression into calls to well-known methods on a collection type.</span></span> <span data-ttu-id="4e5da-3318">これらの明確に定義されたメソッドは、クエリ可能なコレクションの要素の型と、コレクションで実行されるクエリ演算子の結果の型を定義します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3318">These well-defined methods define the element type of the queryable collection as well as the result types of query operators executed on the collection.</span></span> <span data-ttu-id="4e5da-3319">各クエリ演算子では、特定の変換は実装に依存するクエリ演算子に変換される一般に、メソッドまたはメソッドを指定します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3319">Each query operator specifies the method or methods that the query operator is generally translated into, although the specific translation is implementation dependent.</span></span> <span data-ttu-id="4e5da-3320">メソッドは、次のような一般的な形式を使用して、仕様で与えられます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3320">The methods are given in the specification using a general format that looks like:</span></span>

```vb
Function Select(selector As Func(Of T, R)) As CR
```

<span data-ttu-id="4e5da-3321">次のメソッドに適用されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3321">The following applies to the methods:</span></span>

* <span data-ttu-id="4e5da-3322">メソッドは、インスタンスまたはコレクション型の拡張機能のメンバーである必要があり、アクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3322">The method must be an instance or extension member of the collection type and must be accessible.</span></span>

* <span data-ttu-id="4e5da-3323">メソッドは、すべての型引数の推論することでは、汎用的な可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3323">The method may be generic, provided that is possible to infer all type arguments.</span></span>

* <span data-ttu-id="4e5da-3324">メソッドをオーバー ロードを決定するオーバー ロードの解決を使用する場合、使用する方法を正確にします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3324">The method may be overloaded, in which case overload resolution is used to determine the exactly method to use.</span></span>

* <span data-ttu-id="4e5da-3325">デリゲートの代わりに別のデリゲート型を使用できます`Func`入力すると、一致すると、戻り値の型を含む、同じシグネチャを持っていれば、`Func`型。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3325">Another delegate type may be used in place of the delegate `Func` type, provided that it has the same signature, including return type, as the matching `Func` type.</span></span>

* <span data-ttu-id="4e5da-3326">型`System.Linq.Expressions.Expression(Of D)`デリゲートの代わりに使用する場合があります`Func`る型`D`が一致すると、戻り値の型を含む、同じシグネチャを持つデリゲート型`Func`型。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3326">The type `System.Linq.Expressions.Expression(Of D)` may be used in place of the delegate `Func` type, provided that `D` is a delegate type that has the same signature, including return type, as the matching `Func` type.</span></span>

* <span data-ttu-id="4e5da-3327">型`T`入力コレクションの要素型を表します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3327">The type `T` represents the element type of the input collection.</span></span> <span data-ttu-id="4e5da-3328">同じ種類の入力の要素にクエリ可能なコレクション型のすべてのコレクション型によって定義されるメソッドが必要です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3328">All of the methods defined by a collection type must have the same input element type for the collection type to be queryable.</span></span>

* <span data-ttu-id="4e5da-3329">型`S`結合を実行するクエリ演算子の場合、2 番目の入力コレクションの要素型を表します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3329">The type `S` represents the element type of the second input collection in the case of query operators that perform joins.</span></span>

* <span data-ttu-id="4e5da-3330">型`K`をキーとして機能する範囲変数のセットを持つクエリ演算子の場合の主要な型を表します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3330">The type `K` represents a key type in the case of query operators that have a set of range variables that act as keys.</span></span>

* <span data-ttu-id="4e5da-3331">型`N`(ただし、ユーザー定義型と組み込みの数値型ではない可能性があります) は、数値型として使用する型を表します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3331">The type `N` represents a type that is used as a numeric type (although it could still be a user-defined type and not an intrinsic numeric type).</span></span>

* <span data-ttu-id="4e5da-3332">型`B`ブール式で使用できる型を表します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3332">The type `B` represents a type that can be used in a Boolean expression.</span></span>

* <span data-ttu-id="4e5da-3333">型`R`クエリ演算子は、結果のコレクションを生成した場合、結果のコレクションの要素型を表します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3333">The type `R` represents the element type of the result collection, if the query operator produces a result collection.</span></span> <span data-ttu-id="4e5da-3334">`R` クエリ演算子の終了時にスコープ内の範囲変数の数によって異なります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3334">`R` depends on the number of range variables in scope at the conclusion of the query operator.</span></span> <span data-ttu-id="4e5da-3335">かどうか、単一の範囲変数がスコープ内、し`R`範囲変数の型です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3335">If a single range variable is in scope, then `R` is the type of that range variable.</span></span> <span data-ttu-id="4e5da-3336">例</span><span class="sxs-lookup"><span data-stu-id="4e5da-3336">In the example</span></span>

  ```vb
  Dim custNames = From c In Customers
                  Select c.Name
  ```

  <span data-ttu-id="4e5da-3337">クエリの結果の要素型のコレクション型であるが`String`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3337">the result of the query will be a collection type with an element type of `String`.</span></span> <span data-ttu-id="4e5da-3338">は、スコープの複数の範囲変数場合、`R`範囲変数とスコープ内のすべてを含む匿名型は、`Key`フィールド。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3338">If multiple range variables are in scope, then `R` is an anonymous type that contains all of the range variables in scope as `Key` fields.</span></span> <span data-ttu-id="4e5da-3339">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3339">In the example:</span></span>

  ```vb
  Dim custNames = From c In Customers, o In c.Orders 
                  Select Name = c.Name, ProductName = o.ProductName
  ```

  <span data-ttu-id="4e5da-3340">クエリの結果をという名前の読み取り専用プロパティを持つ匿名型の要素型のコレクション型となります`Name`型の`String`という名前の読み取り専用プロパティと`ProductName`型の`String`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3340">the result of the query will be a collection type with an element type of an anonymous type with a read-only property named `Name` of type `String` and a read-only property named `ProductName` of type `String`.</span></span>

  <span data-ttu-id="4e5da-3341">クエリ式では、範囲変数を格納する生成された匿名型は*透明*ことを示す範囲変数の修飾なしで利用は常にします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3341">Within a query expression, anonymous types generated to contain range variables are *transparent*, which means that range variables are always available without qualification.</span></span> <span data-ttu-id="4e5da-3342">たとえば、前の例で範囲変数`c`と`o`で修飾なしでアクセスする可能性があります、`Select`入力コレクションの要素の型が、匿名型も、演算子のクエリを実行します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3342">For example, in the previous example the range variables `c` and `o` could be accessed without qualification in the `Select` query operator, even though the input collection's element type was an anonymous type.</span></span>

* <span data-ttu-id="4e5da-3343">型`CX`コレクション型、必ずしも、入力コレクション型、要素型がいくつかの型を表す`X`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3343">The type `CX` represents a collection type, not necessarily the input collection type, whose element type is some type `X`.</span></span>

<span data-ttu-id="4e5da-3344">クエリ可能なコレクション型には、優先順位で、次の条件のいずれかを満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3344">A queryable collection type must satisfy one of the following conditions, in order of preference:</span></span>

* <span data-ttu-id="4e5da-3345">準拠を定義する必要があります、`Select`メソッド。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3345">It must define a conforming `Select` method.</span></span>

* <span data-ttu-id="4e5da-3346">する必要がありますが、次のメソッドのいずれか</span><span class="sxs-lookup"><span data-stu-id="4e5da-3346">It must have have one of the following methods</span></span>

  ```vb
  Function AsEnumerable() As CT
  Function AsQueryable() As CT
  ```

  <span data-ttu-id="4e5da-3347">クエリ可能なコレクションを取得すると呼ばれることができます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3347">which can be called to obtain a queryable collection.</span></span> <span data-ttu-id="4e5da-3348">どちらの方法が指定されている場合`AsQueryable`が優先`AsEnumerable`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3348">If both methods are provided, `AsQueryable` is preferred over `AsEnumerable`.</span></span>

* <span data-ttu-id="4e5da-3349">メソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3349">It must have a method</span></span>

  ```vb
  Function Cast(Of T)() As CT
  ```

  <span data-ttu-id="4e5da-3350">クエリ可能なコレクションを生成するために、範囲変数の型を使用すると呼ばれることができます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3350">which can be called with the type of the range variable to produce a queryable collection.</span></span>

<span data-ttu-id="4e5da-3351">コレクションの要素の型を決定するが、実際のメソッド呼び出しとは無関係に発生するための特定のメソッドの適用性を特定できません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3351">Because determining the element type of a collection occurs independently of an actual method invocation, the applicability of specific methods cannot be determined.</span></span> <span data-ttu-id="4e5da-3352">したがって、よく知られているメソッドに一致するインスタンス メソッドがある場合は、コレクションの要素の型を決定する、ときに、よく知られているメソッドと一致する任意の拡張メソッドは無視されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3352">Thus, when determining the element type of a collection if there are instance methods that match well-known methods, then any extension methods that match well-known methods are ignored.</span></span>

<span data-ttu-id="4e5da-3353">クエリ演算子の変換は、クエリ演算子の式で行われる順序で発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3353">Query operator translation occurs in the order in which the query operators occur in the expression.</span></span> <span data-ttu-id="4e5da-3354">少なくともすべてのコレクション オブジェクトをサポートする必要がありますが、すべての必要なすべてのクエリ演算子メソッドを実装するコレクション オブジェクトに対する必要はありません、`Select`クエリ演算子。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3354">It is not necessary for a collection object to implement all of the methods needed by all the query operators, although every collection object must at least support the `Select` query operator.</span></span> <span data-ttu-id="4e5da-3355">必要なメソッドが存在しない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3355">If a needed method is not present, a compile-time error occurs.</span></span> <span data-ttu-id="4e5da-3356">よく知られているメソッドの名前をバインドするときに、非メソッドはシャドウのセマンティクスが引き続き適用されますがインターフェイスとバインドするには、拡張メソッドで複数の継承するために無視されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3356">When binding well-known method names, non-methods are ignored for the purpose of multiple inheritance in interfaces and extension method binding, although shadowing semantics still apply.</span></span> <span data-ttu-id="4e5da-3357">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3357">For example:</span></span>

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

### <a name="default-query-indexer"></a><span data-ttu-id="4e5da-3358">既定のクエリのインデクサー</span><span class="sxs-lookup"><span data-stu-id="4e5da-3358">Default Query Indexer</span></span>

<span data-ttu-id="4e5da-3359">要素型があるすべてのクエリ可能なコレクション型`T`まだ、既定値がないとプロパティは、次の一般的な形式の既定のプロパティを持つと見なされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3359">Every queryable collection type whose element type is `T` and does not already have a default property is considered to have a default property of the following general form:</span></span>

```vb
Public ReadOnly Default Property Item(index As Integer) As T
    Get
        Return Me.ElementAtOrDefault(index)
    End Get
End Property
```

<span data-ttu-id="4e5da-3360">既定のプロパティは、既定値プロパティ アクセスの構文を使用する場合にのみ参照できます。既定のプロパティは、名前によって参照することはできません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3360">The default property can only be referred to using the default property access syntax; the default property cannot be referred to by name.</span></span> <span data-ttu-id="4e5da-3361">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3361">For example:</span></span>

```vb
Dim customers As IEnumerable(Of Customer) = ...
Dim customerThree = customers(2)

' Error, no such property
Dim customerFour = customers.Item(4)
```

<span data-ttu-id="4e5da-3362">コレクション型がない場合、`ElementAtOrDefault`メンバー、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3362">If the collection type does not have an `ElementAtOrDefault` member, a compile-time error will occur.</span></span>

### <a name="from-query-operator"></a><span data-ttu-id="4e5da-3363">クエリ演算子から</span><span class="sxs-lookup"><span data-stu-id="4e5da-3363">From Query Operator</span></span>

<span data-ttu-id="4e5da-3364">`From`クエリ演算子には、クエリを実行するコレクションの個々 のメンバーを表すコレクションの範囲変数が導入されています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3364">The `From` query operator introduces a collection range variable that represents the individual members of a collection to be queried.</span></span>

```antlr
FromQueryOperator
    : LineTerminator? 'From' LineTerminator? CollectionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="4e5da-3365">たとえば、クエリ式。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3365">For example, the query expression:</span></span>

```vb
From c As Customer In Customers ...
```

<span data-ttu-id="4e5da-3366">相当する見なすことができます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3366">can be thought of as equivalent to</span></span>

```vb
For Each c As Customer In Customers
        ...
Next c
```

<span data-ttu-id="4e5da-3367">ときに、`From`クエリ演算子が複数のコレクションの範囲変数を宣言またはこの問題は`From`クエリ演算子がクエリ式で、コレクションの新しい各範囲変数は*クロス結合*の既存のセットに範囲変数。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3367">When a `From` query operator declares multiple collection range variables or is not the first `From` query operator in the query expression, each new collection range variable is *cross joined* to the existing set of range variables.</span></span> <span data-ttu-id="4e5da-3368">クエリが参加しているコレクション内のすべての要素のクロス積に対して評価されることになります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3368">The result is that the query is evaluated over the cross-product of all the elements in the joined collections.</span></span> <span data-ttu-id="4e5da-3369">たとえば、次のような式があるとします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3369">For example, the expression:</span></span>

```vb
From c In Customers _
From e In Employees _
...
```

<span data-ttu-id="4e5da-3370">相当する考えることができます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3370">can be thought of as equivalent to:</span></span>

```vb
For Each c In Customers
    For Each e In Employees
            ...
    Next e
Next c
```

<span data-ttu-id="4e5da-3371">まったく同じです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3371">and is exactly equivalent to:</span></span>

```vb
From c In Customers, e In Employees ...
```

<span data-ttu-id="4e5da-3372">前のクエリ演算子で導入された範囲変数は、後で使用できます`From`クエリ演算子。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3372">The range variables introduced in previous query operators can be used in a later `From` query operator.</span></span> <span data-ttu-id="4e5da-3373">たとえば、次のクエリ式で、2 つ目`From`クエリ演算子が最初の範囲変数の値を示します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3373">For example, in the following query expression the second `From` query operator refers to the value of the first range variable:</span></span>

```vb
From c As Customer In Customers _
From o As Order In c.Orders _
Select c.Name, o
```

<span data-ttu-id="4e5da-3374">内の複数の範囲変数、`From`演算子または複数のクエリ`From`クエリ演算子は、コレクション型には、次のメソッドの一方または両方が含まれている場合にのみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3374">Multiple range variables in a `From` query operator or multiple `From` query operators are only supported if the collection type contains one or both of the following methods:</span></span>

```vb
Function SelectMany(selector As Func(Of T, CR)) As CR
Function SelectMany(selector As Func(Of T, CS), _
                          resultsSelector As Func(Of T, S, R)) As CR
```

<span data-ttu-id="4e5da-3375">コード</span><span class="sxs-lookup"><span data-stu-id="4e5da-3375">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs, y In ys ...
```

<span data-ttu-id="4e5da-3376">一般に翻訳されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3376">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = _
    xs.SelectMany( _
        Function(x As Integer) ys, _
        Function(x As Integer, y As Integer) New With {x, y})...
```

<span data-ttu-id="4e5da-3377">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3377">__Note.__</span></span> <span data-ttu-id="4e5da-3378">`From` 予約語ではありません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3378">`From` is not a reserved word.</span></span>


### <a name="join-query-operator"></a><span data-ttu-id="4e5da-3379">Join クエリ演算子</span><span class="sxs-lookup"><span data-stu-id="4e5da-3379">Join Query Operator</span></span>

<span data-ttu-id="4e5da-3380">`Join`等値式に基づいてクエリ演算子を結合する要素を結合されている 1 つのコレクションを作成、新しいコレクションの範囲変数の既存の範囲変数。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3380">The `Join` query operator joins existing range variables with a new collection range variable, producing a single collection whose elements have been joined together based on an equality expression.</span></span>

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

<span data-ttu-id="4e5da-3381">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3381">For example:</span></span>

```vb
Dim customersAndOrders = _
    From cust In Customers _
    Join ord In Orders On cust.ID Equals ord.CustomerID
```

<span data-ttu-id="4e5da-3382">等しいかどうかの式は、正規表現の等価性の式よりも制限。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3382">The equality expression is more restricted than a regular equality expression:</span></span>

* <span data-ttu-id="4e5da-3383">両方の式は、値として分類する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3383">Both expressions must be classified as a value.</span></span>

* <span data-ttu-id="4e5da-3384">両方の式は、少なくとも 1 つの範囲変数を参照する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3384">Both expressions must reference at least one range variable.</span></span>

* <span data-ttu-id="4e5da-3385">範囲変数が、結合に宣言するは、クエリ演算子は、式のいずれかで参照する必要があり、表現する必要があります、その他の範囲変数を参照していないことです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3385">The range variable declared in the join query operator must be referenced by one of the expressions, and that expression must not reference any other range variables.</span></span>

<span data-ttu-id="4e5da-3386">2 つの式の型でない場合、まったく同じ型、</span><span class="sxs-lookup"><span data-stu-id="4e5da-3386">If the types of the two expressions are not the exact same type, then</span></span>

* <span data-ttu-id="4e5da-3387">等値演算子は 2 種類の定義、両方の式が、暗黙的に変換されない`Object`、両方の式をその型に変換します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3387">If the equality operator is defined for the two types, both expressions are implicitly convertible to it, and it is not `Object`, then convert both expressions to that type.</span></span>

* <span data-ttu-id="4e5da-3388">それ以外の場合、両方の式を暗黙的に変換する主要な型がある場合、両方の式を型に変換します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3388">Otherwise, if there is a dominant type that both expressions can be implicitly converted to, then convert both expressions to that type.</span></span>

* <span data-ttu-id="4e5da-3389">それ以外の場合は、コンパイル時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3389">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="4e5da-3390">ハッシュ値を使用して、式を比較 (つまり、呼び出すことによって`GetHashCode()`) 効率を高めるための等値演算子を使用するではなく。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3390">The expressions are compared using hash values (i.e. by calling `GetHashCode()`) rather than by using equality operators for efficiency.</span></span> <span data-ttu-id="4e5da-3391">A`Join`クエリ演算子は、複数の結合または等値演算子と同じ条件を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3391">A `Join` query operator may do multiple joins or equality conditions in the same operator.</span></span> <span data-ttu-id="4e5da-3392">A`Join`クエリ演算子がコレクション型には、メソッドが含まれている場合にのみサポートされています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3392">A `Join` query operator is only supported if the collection type contains a method:</span></span>

```vb
Function Join(inner As CS, _
                  outerSelector As Func(Of T, K), _
                  innerSelector As Func(Of S, K), _
                  resultSelector As Func(Of T, S, R)) As CR
```

<span data-ttu-id="4e5da-3393">コード</span><span class="sxs-lookup"><span data-stu-id="4e5da-3393">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs _
            Join y In ys On x Equals y _
            ...
```

<span data-ttu-id="4e5da-3394">一般に翻訳されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3394">is generally translated to</span></span>

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

<span data-ttu-id="4e5da-3395">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3395">__Note.__</span></span> <span data-ttu-id="4e5da-3396">`Join`、`On`と`Equals`予約された単語は使用されません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3396">`Join`, `On` and `Equals` are not reserved words.</span></span>


### <a name="let-query-operator"></a><span data-ttu-id="4e5da-3397">Let クエリ演算子</span><span class="sxs-lookup"><span data-stu-id="4e5da-3397">Let Query Operator</span></span>

<span data-ttu-id="4e5da-3398">`Let`クエリ演算子に式の範囲変数が導入されています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3398">The `Let` query operator introduces an expression range variable.</span></span> <span data-ttu-id="4e5da-3399">これにより、複数回以降のクエリ演算子で使用すると、中間の値を計算できます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3399">This allows calculating an intermediate value once that will be used multiple times in later query operators.</span></span>

```antlr
LetQueryOperator
    : LineTerminator? 'Let' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="4e5da-3400">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3400">For example:</span></span>

```vb
Dim taxedPrices = _
    From o In Orders _
    Let tax = o.Price * 0.088 _
    Where tax > 3.50 _
    Select o.Price, tax, total = o.Price + tax
```

<span data-ttu-id="4e5da-3401">相当する考えることができます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3401">can be thought of as equivalent to:</span></span>

```vb
For Each o In Orders
    Dim tax = o.Price * 0.088
    ...
Next o
```

<span data-ttu-id="4e5da-3402">A`Let`クエリ演算子がコレクション型には、メソッドが含まれている場合にのみサポートされています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3402">A `Let` query operator is only supported if the collection type contains a method:</span></span>

```vb
Function Select(selector As Func(Of T, R)) As CR
```

<span data-ttu-id="4e5da-3403">コード</span><span class="sxs-lookup"><span data-stu-id="4e5da-3403">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Let y = x * 10 _
            ...
```

<span data-ttu-id="4e5da-3404">一般に翻訳されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3404">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Select(Function(x As Integer) New With {x, .y = x * 10})...
```


### <a name="select-query-operator"></a><span data-ttu-id="4e5da-3405">クエリ演算子を選択します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3405">Select Query Operator</span></span>

<span data-ttu-id="4e5da-3406">`Select`クエリ演算子は似ています、`Let`クエリ演算子の式の範囲変数が導入されている、`Select`クエリ演算子には、それらを追加する代わりに、現在利用可能な範囲変数が非表示になります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3406">The `Select` query operator is like the `Let` query operator in that it introduces expression range variables; however, a `Select` query operator hides the currently available range variables instead of adding to them.</span></span> <span data-ttu-id="4e5da-3407">導入された式の範囲変数の型も、`Select`クエリ演算子は、ローカル変数の型の推論規則を使用して常に推論されます。 明示的な型を指定することはできませんと型を推論されない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3407">Also, the type of an expression range variable introduced by a `Select` query operator is always inferred using local variable type inference rules; an explicit type cannot be specified, and if no type can be inferred, a compile-time error occurs.</span></span>

```antlr
SelectQueryOperator
    : LineTerminator? 'Select' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="4e5da-3408">たとえば、次のクエリについて考えます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3408">For example, in the query:</span></span>

```vb
Dim smiths = _
    From cust In Customers _
    Select name = cust.name _
    Where name.EndsWith("Smith")
```

<span data-ttu-id="4e5da-3409">`Where`クエリ演算子のみへのアクセスを持つ、`name`で導入された範囲変数、`Select`演算子場合、`Where`参照しようとしていた演算子`cust`コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3409">the `Where` query operator only has access to the `name` range variable introduced by the `Select` operator; if the `Where` operator had tried to reference `cust`, a compile-time error would have occurred.</span></span>

<span data-ttu-id="4e5da-3410">範囲変数の名前を明示的に指定する代わりに、`Select`演算子は、範囲変数の名前を推論できる匿名型のオブジェクト作成式と同じ規則を使ってクエリ。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3410">Instead of explicitly specifying the names of the range variables, a `Select` query operator can infer the names of the range variables, using the same rules as anonymous type object creation expressions.</span></span> <span data-ttu-id="4e5da-3411">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3411">For example:</span></span>

```vb
Dim custAndOrderNames = _
      From cust In Customers, ord In cust.Orders _
      Select cust.name, ord.ProductName _
        Where name.EndsWith("Smith")
```

<span data-ttu-id="4e5da-3412">範囲変数の名前が指定されていない、名前が推測できない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3412">If the name of the range variable is not supplied and a name cannot be inferred, a compile-time error occurs.</span></span> <span data-ttu-id="4e5da-3413">場合、`Select`クエリ演算子には、1 つの式のみが含まれています、範囲変数の名前を推論することはできませんが、範囲変数は名前のないかどうかはエラーは発生しません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3413">If the `Select` query operator contains only a single expression, no error occurs if a name for that range variable cannot be inferred but the range variable is nameless.</span></span>  <span data-ttu-id="4e5da-3414">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3414">For example:</span></span>

```vb
Dim custAndOrderNames = _
      From cust In Customers, ord In cust.Orders _
      Select cust.Name & " bought " & ord.ProductName _
        Take 10
```

<span data-ttu-id="4e5da-3415">あいまいさがある場合、`Select`範囲変数と等値式に名前を割り当てることの間のクエリ演算子、名前の割り当てはお勧めします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3415">If there is an ambiguity in a `Select` query operator between assigning a name to a range variable and an equality expression, the name assignment is preferred.</span></span> <span data-ttu-id="4e5da-3416">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3416">For example:</span></span>

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

<span data-ttu-id="4e5da-3417">内の各式、`Select`クエリ演算子は、値として分類する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3417">Each expression in the `Select` query operator must be classified as a value.</span></span> <span data-ttu-id="4e5da-3418">A`Select`クエリ演算子はコレクション型には、メソッドが含まれている場合にのみサポートされています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3418">A `Select` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Select(selector As Func(Of T, R)) As CR
```

<span data-ttu-id="4e5da-3419">コード</span><span class="sxs-lookup"><span data-stu-id="4e5da-3419">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Select x, y = x * 10 _
            ...
```

<span data-ttu-id="4e5da-3420">一般に翻訳されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3420">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Select(Function(x As Integer) New With {x, .y = x * 10})...
```


### <a name="distinct-query-operator"></a><span data-ttu-id="4e5da-3421">Distinct クエリ演算子</span><span class="sxs-lookup"><span data-stu-id="4e5da-3421">Distinct Query Operator</span></span>

<span data-ttu-id="4e5da-3422">`Distinct`クエリ演算子と等しいかどうか要素型を比較することによって決定される個別の値にのみ、コレクション内の値を制限します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3422">The `Distinct` query operator restricts the values in a collection only to those with distinct values, as determined by comparing the element type for equality.</span></span>

```antlr
DistinctQueryOperator
    : LineTerminator? 'Distinct' LineTerminator?
    ;
```

<span data-ttu-id="4e5da-3423">たとえば、次のようなクエリがあるとします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3423">For example, the query:</span></span>

```vb
Dim distinctCustomerPrice = _
    From cust In Customers, ord In cust.Orders _
    Select cust.Name, ord.Price _
    Distinct
```

<span data-ttu-id="4e5da-3424">お客様は、同じ価格、複数の注文を持っている場合でも、各顧客の名前と順序の価格のペアリングを個別には 1 つの行を返しますのみは。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3424">will only return one row for each distinct pairing of customer name and order price, even if the customer has multiple orders with the same price.</span></span> <span data-ttu-id="4e5da-3425">A`Distinct`クエリ演算子はコレクション型には、メソッドが含まれている場合にのみサポートされています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3425">A `Distinct` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Distinct() As CT
```

<span data-ttu-id="4e5da-3426">コード</span><span class="sxs-lookup"><span data-stu-id="4e5da-3426">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Distinct _
            ...
```

<span data-ttu-id="4e5da-3427">一般に翻訳されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3427">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = xs.Distinct()...
```

<span data-ttu-id="4e5da-3428">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3428">__Note.__</span></span> <span data-ttu-id="4e5da-3429">`Distinct` 予約語ではありません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3429">`Distinct` is not a reserved word.</span></span>


### <a name="where-query-operator"></a><span data-ttu-id="4e5da-3430">Where クエリ演算子</span><span class="sxs-lookup"><span data-stu-id="4e5da-3430">Where Query Operator</span></span>

<span data-ttu-id="4e5da-3431">`Where`クエリ演算子が指定された条件を満たすコレクションに値を制限します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3431">The `Where` query operator restricts the values in a collection to those that satisfy a given condition.</span></span>

```antlr
WhereQueryOperator
    : LineTerminator? 'Where' LineTerminator? BooleanExpression
    ;
```

<span data-ttu-id="4e5da-3432">A`Where`クエリ演算子は、範囲変数の値のセットごとに評価されるブール式; 式の値が出力コレクションに値が表示されます、true の場合、それ以外の場合、値はスキップされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3432">A `Where` query operator takes a Boolean expression that is evaluated for each set of range variable values; if the value of the expression is true, then the values appear in the output collection, otherwise the values are skipped.</span></span> <span data-ttu-id="4e5da-3433">たとえば、クエリ式。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3433">For example, the query expression:</span></span>

```vb
From cust In Customers, ord In Orders _
Where cust.ID = ord.CustomerID _
...
```

<span data-ttu-id="4e5da-3434">入れ子になったループに相当する見なすことができます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3434">can be thought of as equivalent to the nested loop</span></span>

```vb
For Each cust In Customers
    For Each ord In Orders
            If cust.ID = ord.CustomerID Then
                ...
            End If
    Next ord
Next cust
```

<span data-ttu-id="4e5da-3435">A`Where`クエリ演算子はコレクション型には、メソッドが含まれている場合にのみサポートされています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3435">A `Where` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Where(predicate As Func(Of T, B)) As CT
```

<span data-ttu-id="4e5da-3436">コード</span><span class="sxs-lookup"><span data-stu-id="4e5da-3436">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Where x < 10 _
            ...
```

<span data-ttu-id="4e5da-3437">一般に翻訳されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3437">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Where(Function(x As Integer) x < 10)...
```

<span data-ttu-id="4e5da-3438">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3438">__Note.__</span></span> <span data-ttu-id="4e5da-3439">`Where` 予約語ではありません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3439">`Where` is not a reserved word.</span></span>


### <a name="partition-query-operators"></a><span data-ttu-id="4e5da-3440">パーティションのクエリ演算子</span><span class="sxs-lookup"><span data-stu-id="4e5da-3440">Partition Query Operators</span></span>

```antlr
PartitionQueryOperator
    : LineTerminator? 'Take' LineTerminator? Expression
    | LineTerminator? 'Take' 'While' LineTerminator? BooleanExpression
    | LineTerminator? 'Skip' LineTerminator? Expression
    | LineTerminator? 'Skip' 'While' LineTerminator? BooleanExpression
    ;
```

<span data-ttu-id="4e5da-3441">`Take`クエリ演算子の結果、最初に`n`コレクションの要素。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3441">The `Take` query operator results in the first `n` elements of a collection.</span></span> <span data-ttu-id="4e5da-3442">使用すると、`While`修飾子、`Take`最初の演算子の結果`n`ブール式を満たすコレクションの要素。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3442">When used with the `While` modifier, the `Take` operator results in the first `n` elements of a collection that satisfy a Boolean expression.</span></span> <span data-ttu-id="4e5da-3443">`Skip`演算子は、1 つ目をスキップします。`n`コレクションの要素をコレクションの残りの部分を返します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3443">The `Skip` operator skips the first `n` elements of a collection and then returns the remainder of the collection.</span></span>  <span data-ttu-id="4e5da-3444">組み合わせて使用すると、`While`修飾子、`Skip`演算子は、1 つ目をスキップします。`n`ブール式を満たすと、コレクションの残りの部分を返しますが、コレクションの要素。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3444">When used in conjunction with the `While` modifier, the `Skip` operator skips the first `n` elements of a collection that satisfy a Boolean expression and then returns the rest of the collection.</span></span> <span data-ttu-id="4e5da-3445">内の式を`Take`または`Skip`クエリ演算子は、値として分類する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3445">The expressions in a `Take` or `Skip` query operator must be classified as a value.</span></span>

<span data-ttu-id="4e5da-3446">A`Take`クエリ演算子はコレクション型には、メソッドが含まれている場合にのみサポートされています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3446">A `Take` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Take(count As N) As CT
```

<span data-ttu-id="4e5da-3447">A`Skip`クエリ演算子はコレクション型には、メソッドが含まれている場合にのみサポートされています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3447">A `Skip` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Skip(count As N) As CT
```

<span data-ttu-id="4e5da-3448">A`Take While`クエリ演算子はコレクション型には、メソッドが含まれている場合にのみサポートされています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3448">A `Take While` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function TakeWhile(predicate As Func(Of T, B)) As CT
```

<span data-ttu-id="4e5da-3449">A`Skip While`クエリ演算子はコレクション型には、メソッドが含まれている場合にのみサポートされています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3449">A `Skip While` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function SkipWhile(predicate As Func(Of T, B)) As CT
```

<span data-ttu-id="4e5da-3450">コード</span><span class="sxs-lookup"><span data-stu-id="4e5da-3450">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Skip 10 _
            Take 5 _
            Skip While x < 10 _
            Take While x > 5 _
            ...
```

<span data-ttu-id="4e5da-3451">一般に翻訳されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3451">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Skip(10). _
        Take(5). _
        SkipWhile(Function(x) x < 10). _
        TakeWhile(Function(x) x > 5)...
```

<span data-ttu-id="4e5da-3452">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3452">__Note.__</span></span> <span data-ttu-id="4e5da-3453">`Take` `Skip`予約された単語は使用されません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3453">`Take` and `Skip` are not reserved words.</span></span>


### <a name="order-by-query-operator"></a><span data-ttu-id="4e5da-3454">Order By クエリ演算子</span><span class="sxs-lookup"><span data-stu-id="4e5da-3454">Order By Query Operator</span></span>

<span data-ttu-id="4e5da-3455">`Order By`クエリ演算子は、範囲変数に表示される値を並べ替えます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3455">The `Order By` query operator orders the values that appear in the range variables.</span></span> 

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

<span data-ttu-id="4e5da-3456">`Order By`クエリ演算子は、反復変数の並べ替えに使用されるキーの値を指定する式。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3456">An `Order By` query operator takes expressions that specify the key values that should be used to order the iteration variables.</span></span> <span data-ttu-id="4e5da-3457">たとえば、次のクエリでは、製品の価格で並べ替えられますが返されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3457">For example, the following query returns products sorted by price:</span></span>

```vb
Dim productsByPrice = _
    From p In Products _
    Order By p.Price _
    Select p.Name
```

<span data-ttu-id="4e5da-3458">としてマークできる順序付け`Ascending`より大きな値は、前に小さい値を取得する場合または`Descending`値を小さくする前に大きい値を取得する場合。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3458">An ordering can be marked as `Ascending`, in which case smaller values come before larger values, or `Descending`, in which case larger values come before smaller values.</span></span> <span data-ttu-id="4e5da-3459">順序付けが指定されないかどうかの既定値は`Ascending`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3459">The default for an ordering if none is specified is `Ascending`.</span></span> <span data-ttu-id="4e5da-3460">たとえば、次のクエリには、最も負荷の高い製品に価格で並べ替えた製品が返されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3460">For example, the following query returns products sorted by price with the most expensive product first:</span></span>

```vb
Dim productsByPriceDesc = _
    From p In Products _
    Order By p.Price Descending _
    Select p.Name
```

<span data-ttu-id="4e5da-3461">`Order By`クエリ演算子は、順序付けの複数の式を指定できます、コレクションが入れ子になった方法で順序付けられた場合。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3461">The `Order By` query operator may specify multiple expressions for ordering, in which case the collection is ordered in a nested manner.</span></span> <span data-ttu-id="4e5da-3462">たとえば、次のクエリでは、市区町村内の各状態によってし、各都市の郵便番号/ZIP code によってし、状態でお客様が注文します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3462">For example, the following query orders customers by state, then by city within each state and then by ZIP code within each city:</span></span>

```vb
Dim customersByLocation = _
    From c In Customers _
    Order By c.State, c.City, c.ZIP _
    Select c.Name, c.State, c.City, c.ZIP
```

<span data-ttu-id="4e5da-3463">内の式、`Order By`クエリ演算子は、値として分類する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3463">The expressions in an `Order By` query operator must be classified as a value.</span></span> <span data-ttu-id="4e5da-3464">`Order By`コレクション型には、次のメソッドの一方または両方が含まれています。 場合にのみ、クエリ演算子がサポートされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3464">An `Order By` query operator is supported only if the collection type contains one or both of the following methods:</span></span>

```vb
Function OrderBy(keySelector As Func(Of T, K)) As CT
Function OrderByDescending(keySelector As Func(Of T, K)) As CT
```

<span data-ttu-id="4e5da-3465">戻り値の型`CT`必要があります、*順序付けられたコレクション*します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3465">The return type `CT` must be an *ordered collection*.</span></span> <span data-ttu-id="4e5da-3466">順序付きコレクションでは、メソッドの一方または両方を含むコレクション型を示します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3466">An ordered collection is a collection type that contains one or both of the methods:</span></span>

```vb
Function ThenBy(keySelector As Func(Of T, K)) As CT
Function ThenByDescending(keySelector As Func(Of T, K)) As CT
```

<span data-ttu-id="4e5da-3467">コード</span><span class="sxs-lookup"><span data-stu-id="4e5da-3467">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Order By x Ascending, x Mod 2 Descending _
            ...
```

<span data-ttu-id="4e5da-3468">一般に翻訳されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3468">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.OrderBy(Function(x) x).ThenByDescending(Function(x) x Mod 2)...
```

<span data-ttu-id="4e5da-3469">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3469">__Note.__</span></span> <span data-ttu-id="4e5da-3470">クエリ演算子は、特定のクエリ操作を実装するメソッドに構文を単にマップ、ため、順序の維持は言語によっては指定されていませんし、演算子自体の実装によって決定されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3470">Because query operators simply map syntax to methods that implement a particular query operation, order preservation is not dictated by the language and is determined by the implementation of the operator itself.</span></span> <span data-ttu-id="4e5da-3471">これは、機能は、ユーザー定義の数値型の加算演算子をオーバー ロードを実装可能性があります、追加のようなものを実行しない点でユーザー定義演算子によく似ています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3471">This is very similar to user-defined operators in that the implementation to overload the addition operator for a user-defined numeric type may not perform anything resembling an addition.</span></span> <span data-ttu-id="4e5da-3472">もちろん、予測可能性を保持するために実装するユーザーの期待に一致しないものは推奨されません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3472">Of course, to preserve predictability, implementing something that does not match user expectations is not recommended.</span></span>

<span data-ttu-id="4e5da-3473">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3473">__Note.__</span></span> <span data-ttu-id="4e5da-3474">`Order` `By`予約された単語は使用されません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3474">`Order` and `By` are not reserved words.</span></span>


### <a name="group-by-query-operator"></a><span data-ttu-id="4e5da-3475">Group By クエリ演算子</span><span class="sxs-lookup"><span data-stu-id="4e5da-3475">Group By Query Operator</span></span>

<span data-ttu-id="4e5da-3476">`Group By`クエリ演算子は、1 つまたは複数の式に基づくスコープ内の範囲変数をグループ化し、新しい範囲グループに基づいて変数が生成されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3476">The `Group By` query operator groups the range variables in scope based on one or more expressions, and then produces new range variables based on those groupings.</span></span>

```antlr
GroupByQueryOperator
    : LineTerminator? 'Group' ( LineTerminator? ExpressionRangeVariableDeclarationList )?
      LineTerminator? 'By' LineTerminator? ExpressionRangeVariableDeclarationList
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="4e5da-3477">たとえば、次のクエリはグループによってすべての顧客`State`、し、各グループの数、平均年齢を計算します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3477">For example, the following query groups all customers by `State`, and then computes the count and average age of each group:</span></span>

```vb
Dim averageAges = _
    From cust In Customers _
    Group By cust.State _
    Into Count(), Average(cust.Age)
```

<span data-ttu-id="4e5da-3478">`Group By`クエリ演算子は、3 つの句: 省略可能な`Group`句、`By`句、および`Into`句。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3478">The `Group By` query operator has three clauses: the optional `Group` clause, the `By` clause, and the `Into` clause.</span></span> <span data-ttu-id="4e5da-3479">`Group`句と同じ構文およびと効果には、`Select`で使用できる範囲変数のみに影響する点を除いて、演算子のクエリを実行、`Into`句および not、`By`句。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3479">The `Group` clause has the same syntax and effect as a `Select` query operator, except that it only affects the range variables available in the `Into` clause and not the `By` clause.</span></span> <span data-ttu-id="4e5da-3480">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3480">For example:</span></span>

```vb
Dim averageAges = _
    From cust In Customers _
    Group cust.Age By cust.State _
    Into Count(), Average(Age)
```

<span data-ttu-id="4e5da-3481">`By`句はグループ化操作でキーの値として使用されている範囲変数の式を宣言します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3481">The `By` clause declares expression range variables that are used as key values in the grouping operation.</span></span> <span data-ttu-id="4e5da-3482">`Into`句は集計を計算して形成されたグループのそれぞれの範囲変数の式の宣言を使用できます、`By`句。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3482">The `Into` clause allows the declaration of expression range variables that calculate aggregations over each of the groups formed by the `By` clause.</span></span> <span data-ttu-id="4e5da-3483">内で、`Into`句では、式の範囲変数のみ割り当てることができます、メソッドの呼び出しである式の*集計関数*します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3483">Within the `Into` clause, the expression range variable can only be assigned an expression which is a method invocation of an *aggregate function*.</span></span> <span data-ttu-id="4e5da-3484">集計関数は、次のいずれかのようになります (これは必ずしも元のコレクションの同じコレクション型) グループのコレクション型の関数を示します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3484">An aggregate function is a function on the collection type of the group (which may not necessarily be the same collection type of the original collection) which looks like either of the following methods:</span></span>

```vb
Function _name_() As _type_
Function _name_(selector As Func(Of T, R)) As R
```

<span data-ttu-id="4e5da-3485">集計関数がデリゲートの引数を受け取る場合、invocation 式は、値として分類する必要があります引数式を持つことができます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3485">If an aggregate function takes a delegate argument, then the invocation expression can have an argument expression that must be classified as a value.</span></span>  <span data-ttu-id="4e5da-3486">引数式のスコープ内の範囲変数を使用できます。集計関数の呼び出し内では、これらの範囲変数は、コレクション内の値のすべての形成中、グループ内の値を表します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3486">The argument expression can use the range variables that are in scope; within the call to an aggregate function, those range variables represent the values in the group being formed, not all of the values in the collection.</span></span> <span data-ttu-id="4e5da-3487">たとえば、このセクションでは、元の例で、`Average`関数がまとめて、顧客の年齢層のすべての顧客ではなく、状態ごとの平均を計算します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3487">For example, in the original example in this section the `Average` function calculates the average of the customers' ages per state rather than for all of the customers together.</span></span>

<span data-ttu-id="4e5da-3488">すべてのコレクション型を集計関数があると見なされます`Group`、定義されているパラメーターを受け取らない構成して、グループを返すだけです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3488">All collection types are considered to have the aggregate function `Group` defined on it, which takes no parameters and simply returns the group.</span></span> <span data-ttu-id="4e5da-3489">その他のコレクション型を提供する、標準の集計関数は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3489">Other standard aggregate functions that a collection type may provide are:</span></span>

<span data-ttu-id="4e5da-3490">`Count` `LongCount`、グループまたはブール式を満たすグループ内の要素の数の要素の数を返すをします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3490">`Count` and `LongCount`, which return the count of the elements in the group or the count of the elements in the group that satisfy a Boolean expression.</span></span> <span data-ttu-id="4e5da-3491">`Count` `LongCount`コレクション型を含むメソッドのいずれかの場合にのみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3491">`Count` and `LongCount` are supported only if the collection type contains one of the methods:</span></span>

```vb
Function Count() As N
Function Count(selector As Func(Of T, B)) As N
Function LongCount() As N
Function LongCount(selector As Func(Of T, B)) As N
```

<span data-ttu-id="4e5da-3492">`Sum`、グループ内のすべての要素間で式の合計を返します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3492">`Sum`, which returns the sum of an expression across all the elements in the group.</span></span> <span data-ttu-id="4e5da-3493">`Sum` コレクション型には、メソッドのいずれかが含まれている場合にのみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3493">`Sum` is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Sum() As N
Function Sum(selector As Func(Of T, N)) As N
```

<span data-ttu-id="4e5da-3494">`Min` これは、グループ内のすべての要素間で、式の最小値を返します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3494">`Min` which returns the minimum value of an expression across all the elements in the group.</span></span> <span data-ttu-id="4e5da-3495">`Min` コレクション型には、メソッドのいずれかが含まれている場合にのみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3495">`Min` is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Min() As N
Function Min(selector As Func(Of T, N)) As N
```

<span data-ttu-id="4e5da-3496">`Max`、式の最大値を、グループ内のすべての要素が返されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3496">`Max`, which returns the maximum value of an expression across all the elements in the group.</span></span> <span data-ttu-id="4e5da-3497">`Max` コレクション型には、メソッドのいずれかが含まれている場合にのみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3497">`Max` is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Max() As N
Function Max(selector As Func(Of T, N)) As N
```

<span data-ttu-id="4e5da-3498">`Average`、グループ内のすべての要素間で式の平均値が返されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3498">`Average`, which returns the average of an expression across all the elements in the group.</span></span> <span data-ttu-id="4e5da-3499">`Average` コレクション型には、メソッドのいずれかが含まれている場合にのみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3499">`Average` is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Average() As N
Function Average(selector As Func(Of T, N)) As N
```

<span data-ttu-id="4e5da-3500">`Any`、グループにメンバーが含まれているかどうかブール式がグループ内の任意の要素に対して true のかどうかを決定します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3500">`Any`, which determines whether a group contains members or if a Boolean expression is true for any element in the group.</span></span> <span data-ttu-id="4e5da-3501">`Any` ブール式で使用でき、コレクション型には、メソッドのいずれかが含まれている場合にのみサポートされている値が返されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3501">`Any` returns a value that can be used in a Boolean expression and is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Any() As B
Function Any(predicate As Func(Of T, B)) As B
```

<span data-ttu-id="4e5da-3502">`All`、ブール式がグループのすべての要素に対して true かどうかを決定します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3502">`All`, which determines whether a Boolean expression is true for all elements in the group.</span></span> <span data-ttu-id="4e5da-3503">`All` ブール式で使用でき、コレクション型には、メソッドが含まれている場合にのみサポートされている値が返されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3503">`All` returns a value that can be used in a Boolean expression and is supported only if the collection type contains a method:</span></span>

```vb
Function All(predicate As Func(Of T, B)) As B
```

<span data-ttu-id="4e5da-3504">後に、`Group By`クエリ演算子、以前のスコープ内の範囲変数は表示されず、および範囲変数がで導入された、`By`と`Into`句が使用可能な。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3504">After a `Group By` query operator, the range variables previously in scope are hidden, and the range variables introduced by the `By` and `Into` clauses are available.</span></span> <span data-ttu-id="4e5da-3505">A`Group By`クエリ演算子はコレクション型には、メソッドが含まれている場合にのみサポートされています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3505">A `Group By` query operator is supported only if the collection type contains the method:</span></span>

```vb
Function GroupBy(keySelector As Func(Of T, K), _
                      resultSelector As Func(Of K, CT, R)) As CR
```

<span data-ttu-id="4e5da-3506">範囲内の変数宣言、`Group`句がコレクション型には、メソッドが含まれている場合にのみサポートされています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3506">Range variable declarations in the `Group` clause are supported only if the collection type contains the method:</span></span>

```vb
Function GroupBy(keySelector As Func(Of T, K), _
                      elementSelector As Func(Of T, S), _
                      resultSelector As Func(Of K, CS, R)) As CR
```

<span data-ttu-id="4e5da-3507">コード</span><span class="sxs-lookup"><span data-stu-id="4e5da-3507">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Group y = x * 10, z = x / 10 By evenOdd = x Mod 2 _
            Into Sum(y), Average(z) _
            ...
```

<span data-ttu-id="4e5da-3508">一般に翻訳されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3508">is generally translated to</span></span>

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

<span data-ttu-id="4e5da-3509">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3509">__Note.__</span></span> <span data-ttu-id="4e5da-3510">`Group`、 `By`、および`Into`予約された単語は使用されません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3510">`Group`, `By`, and `Into` are not reserved words.</span></span>


### <a name="aggregate-query-operator"></a><span data-ttu-id="4e5da-3511">集計クエリ演算子</span><span class="sxs-lookup"><span data-stu-id="4e5da-3511">Aggregate Query Operator</span></span>

<span data-ttu-id="4e5da-3512">`Aggregate`クエリ演算子と同様の関数の実行、`Group By`オペレーターは、既に形成されたされたグループを集計することができる点が異なります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3512">The `Aggregate` query operator performs a similar function as the `Group By` operator, except it allows aggregating over groups that have already been formed.</span></span> <span data-ttu-id="4e5da-3513">グループが既に形成されたされているため、`Into`の句、`Aggregate`クエリ演算子には、スコープ内の範囲変数は非 (これにより、`Aggregate`ように、`Let`と`Group By`ように、 `Select`)。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3513">Because the group has already been formed, the `Into` clause of an `Aggregate` query operator does not hide the range variables in scope (in this way, `Aggregate` is more like a `Let`, and `Group By` is more like a `Select`).</span></span>

```antlr
AggregateQueryOperator
    : LineTerminator? 'Aggregate' LineTerminator? CollectionRangeVariableDeclaration QueryOperator*
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="4e5da-3514">たとえば、次のクエリは米国ワシントン州の顧客によって行われるすべての注文の合計、集計します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3514">For example, the following query aggregates the total of all the orders placed by customers in Washington:</span></span>

```vb
Dim orderTotals = _
    From cust In Customers _
    Where cust.State = "WA" _
    Aggregate order In cust.Orders _
    Into Sum(order.Total)
```

<span data-ttu-id="4e5da-3515">このクエリの結果は、コレクションの要素の入力がという名前のプロパティを持つ匿名型`cust`として型指定された`Customer`という名前のプロパティと`Sum`として型指定された`Integer`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3515">The result of this query is a collection whose element type is an anonymous type with a property named `cust` typed as `Customer` and a property named `Sum` typed as `Integer`.</span></span>

<span data-ttu-id="4e5da-3516">異なり`Group By`、追加のクエリ演算子の間に配置できる、`Aggregate`と`Into`句。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3516">Unlike `Group By`, additional query operators can be placed between the `Aggregate` and `Into` clauses.</span></span> <span data-ttu-id="4e5da-3517">間、`Aggregate`句との終了、`Into`句で宣言されているものも含め、スコープ内のすべての範囲変数、`Aggregate`句を使用できます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3517">Between an `Aggregate` clause and the end of the `Into` clause, all range variables in scope, including those declared by the `Aggregate` clause can be used.</span></span> <span data-ttu-id="4e5da-3518">たとえば、次クエリ集計配置のすべての注文の合計 2006 年に米国ワシントン州の顧客:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3518">For example, the following query aggregates the sum total of all the orders placed by customers in Washington before 2006:</span></span>

```vb
Dim orderTotals = _
    From cust In Customers _
    Where cust.State = "WA" _
    Aggregate order In cust.Orders _
    Where order.Date <= #01/01/2006# _
    Into Sum = Sum(order.Total)
```

<span data-ttu-id="4e5da-3519">`Aggregate`演算子がクエリ式を開始することもできます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3519">The `Aggregate` operator can also be used to start a query expression.</span></span> <span data-ttu-id="4e5da-3520">ここでは、クエリ式の結果値になります、1 つによって計算された、`Into`句。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3520">In this case, the result of the query expression will be the single value computed by the `Into` clause.</span></span> <span data-ttu-id="4e5da-3521">たとえば、次のクエリは、2006 年 1 月 1 日の前にすべての注文合計の合計を計算します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3521">For example, the following query calculates the sum of all the order totals before January 1st, 2006:</span></span>

```vb
Dim ordersTotal = _
    Aggregate order In Orders _
    Where order.Date <= #01/01/2006# _
    Into Sum(order.Total)
```

<span data-ttu-id="4e5da-3522">クエリの結果は、1 つ`Integer`値。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3522">The result of the query is a single `Integer` value.</span></span> <span data-ttu-id="4e5da-3523">`Aggregate`クエリ演算子は、です常に利用可能な (が集計関数する必要があるも有効な式を使用できます)。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3523">An `Aggregate` query operator is always available (although the aggregate function must be also be available for the expression to be valid).</span></span> <span data-ttu-id="4e5da-3524">コード</span><span class="sxs-lookup"><span data-stu-id="4e5da-3524">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    Aggregate x In xs _
    Where x < 5 _
    Into Sum()
```

<span data-ttu-id="4e5da-3525">一般に翻訳されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3525">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Where(Function(x) x < 5).Sum()
```

<span data-ttu-id="4e5da-3526">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3526">__Note.__</span></span> <span data-ttu-id="4e5da-3527">`Aggregate` `Into`予約された単語は使用されません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3527">`Aggregate` and `Into` are not reserved words.</span></span>


### <a name="group-join-query-operator"></a><span data-ttu-id="4e5da-3528">グループ結合のクエリ演算子</span><span class="sxs-lookup"><span data-stu-id="4e5da-3528">Group Join Query Operator</span></span>

<span data-ttu-id="4e5da-3529">`Group Join`クエリ演算子の関数を組み合わせて、`Join`と`Group By`に 1 つの演算子のクエリ演算子。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3529">The `Group Join` query operator combines the functions of the `Join` and `Group By` query operators into a single operator.</span></span> <span data-ttu-id="4e5da-3530">`Group Join` 一致する要素をまとめてグループ化から抽出したキーに基づいて 2 つのコレクションを結合するすべての結合の左側にある特定の要素に一致する結合の右側にある要素。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3530">`Group Join` joins two collections based on matching keys extracted from the elements, grouping together all of the elements on the right side of the join that match a particular element on the left side of the join.</span></span> <span data-ttu-id="4e5da-3531">そのため、演算子は、階層的な結果セットを生成します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3531">Thus, the operator produces a set of hierarchical results.</span></span>

```antlr
GroupJoinQueryOperator
    : LineTerminator? 'Group' 'Join' LineTerminator? CollectionRangeVariableDeclaration
      JoinOrGroupJoinQueryOperator? LineTerminator? 'On' LineTerminator? JoinConditionList
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="4e5da-3532">たとえば、次のクエリでは、単一の顧客の名前、すべての注文の合計量と、注文のすべてのグループにはが含まれている要素が生成されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3532">For example, the following query produces elements that contain a single customer's name, a group of all of their orders, and the total amount of all of those orders:</span></span>

```vb
Dim custsWithOrders = _
    From cust In Customers _
    Group Join order In Orders On cust.ID Equals order.CustomerID _ 
    Into Orders = Group, OrdersTotal = Sum(order.Total) _
    Select cust.Name, Orders, OrdersTotal
```

<span data-ttu-id="4e5da-3533">クエリの結果は、要素型が 3 つのプロパティを持つ匿名型コレクション:`Name`として型指定された、 `String`、`Orders`要素型がコレクションとして型指定された`Order`、および`OrdersTotal`として型指定された、`Integer`.</span><span class="sxs-lookup"><span data-stu-id="4e5da-3533">The result of the query is a collection whose element type is an anonymous type with three properties: `Name`, typed as `String`, `Orders` typed as a collection whose element type is `Order`, and `OrdersTotal`, typed as `Integer`.</span></span> <span data-ttu-id="4e5da-3534">A`Group Join`クエリ演算子はコレクション型には、メソッドが含まれている場合にのみサポートされています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3534">A `Group Join` query operator is supported only if the collection type contains the method:</span></span>

```vb
Function GroupJoin(inner As CS, _
                         outerSelector As Func(Of T, K), _
                         innerSelector As Func(Of S, K), _
                         resultSelector As Func(Of T, CS, R)) As CR
```

<span data-ttu-id="4e5da-3535">コード</span><span class="sxs-lookup"><span data-stu-id="4e5da-3535">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs _
            Group Join y in ys On x Equals y _
            Into g = Group _
            ...
```

<span data-ttu-id="4e5da-3536">一般に翻訳されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3536">is generally translated to</span></span>

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

<span data-ttu-id="4e5da-3537">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3537">__Note.__</span></span> <span data-ttu-id="4e5da-3538">`Group`、 `Join`、および`Into`予約された単語は使用されません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3538">`Group`, `Join`, and `Into` are not reserved words.</span></span>


## <a name="conditional-expressions"></a><span data-ttu-id="4e5da-3539">条件式</span><span class="sxs-lookup"><span data-stu-id="4e5da-3539">Conditional Expressions</span></span>

<span data-ttu-id="4e5da-3540">条件文`If`式は、式をテストし、値を返します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3540">A conditional `If` expression tests an expression and returns a value.</span></span>

```antlr
ConditionalExpression
    : 'If' OpenParenthesis BooleanExpression Comma Expression Comma Expression CloseParenthesis
    | 'If' OpenParenthesis Expression Comma Expression CloseParenthesis
    ;
```

<span data-ttu-id="4e5da-3541">異なり、`IIF`ランタイム関数、ただし、条件付きの式だけが評価に必要な場合は、そのオペランド。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3541">Unlike the `IIF` runtime function, however, a conditional expression only evaluates its operands if necessary.</span></span> <span data-ttu-id="4e5da-3542">したがって、例では、式の`If(c Is Nothing, c.Name, "Unknown")`例外をスローしない場合の値`c`は`Nothing`。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3542">Thus, for example, the expression `If(c Is Nothing, c.Name, "Unknown")` will not throw an exception if the value of `c` is `Nothing`.</span></span> <span data-ttu-id="4e5da-3543">条件式に 2 つの形式: 次の 3 つのオペランドを受け取る 2 つのオペランドと 1 つを受け取るいずれか。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3543">The conditional expression has two forms: one that takes two operands and one that takes three operands.</span></span>

<span data-ttu-id="4e5da-3544">3 つのオペランドが指定されている場合は、3 つすべての式は、値として分類する必要があり、最初のオペランドがブール式にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3544">If three operands are provided, all three expressions must be classified as values, and the first operand must be a Boolean expression.</span></span> <span data-ttu-id="4e5da-3545">結果は場合、式が true の場合、2 番目の式は、結果になります、演算子のそれ以外の場合、3 番目の式になります、演算子の結果。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3545">If the result is of the expression is true, then the second expression will be the result of the operator, otherwise the third expression will be the result of the operator.</span></span> <span data-ttu-id="4e5da-3546">式の結果型は、2 番目と 3 番目の式の型の間の主要な型です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3546">The result type of the expression is the dominant type between the types of the second and third expression.</span></span> <span data-ttu-id="4e5da-3547">主要な型がない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3547">If there is no dominant type, then a compile-time error occurs.</span></span>

<span data-ttu-id="4e5da-3548">2 つのオペランドが指定されている場合は、両方のオペランドの値として分類する必要があり、最初のオペランドが参照型または null 許容値型のいずれかにある必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3548">If two operands are provided, both of the operands must be classified as values, and the first operand must be either a reference type or a nullable value type.</span></span> <span data-ttu-id="4e5da-3549">式`If(x, y)`は評価されますが、式の場合と`If(x IsNot Nothing, x, y)`、2 つの例外。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3549">The expression `If(x, y)` is then evaluated as if was the expression `If(x IsNot Nothing, x, y)`, with two exceptions.</span></span> <span data-ttu-id="4e5da-3550">最初に、最初の式しか評価 1 回、2 つ目は、2 番目のオペランドの型が null 非許容値型と、最初のオペランドの型が場合、`?`の主要な型を決定するときに、最初のオペランドの型から削除します式の結果型。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3550">First, the first expression is only ever evaluated once, and second, if the second operand's type is a non-nullable value type and the first operand's type is, the `?` is removed from the type of the first operand when determining the dominant type for the result type of the expression.</span></span> <span data-ttu-id="4e5da-3551">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3551">For example:</span></span>

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

<span data-ttu-id="4e5da-3552">オペランドの場合は、式の両方の形式で`Nothing`、主要な型を決定するはその型は使用されません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3552">In both forms of the expression, if an operand is `Nothing`, its type is not used to determine the dominant type.</span></span> <span data-ttu-id="4e5da-3553">式の場合`If(<expression>, Nothing, Nothing)`、主要な型があると見なされます`Object`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3553">In the case of the expression `If(<expression>, Nothing, Nothing)`, the dominant type is considered to be `Object`.</span></span>


## <a name="xml-literal-expressions"></a><span data-ttu-id="4e5da-3554">XML リテラル式</span><span class="sxs-lookup"><span data-stu-id="4e5da-3554">XML Literal Expressions</span></span>

<span data-ttu-id="4e5da-3555">XML (eXtensible Markup Language) を表す XML リテラル式には 1.0 値。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3555">An XML literal expression represents an XML (eXtensible Markup Language) 1.0 value.</span></span>

```antlr
XMLLiteralExpression
    : XMLDocument
    | XMLElement
    | XMLProcessingInstruction
    | XMLComment
    | XMLCDATASection
    ;
```

<span data-ttu-id="4e5da-3556">XML リテラル式の結果は、型からの型の 1 つとして型指定された値、`System.Xml.Linq`名前空間。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3556">The result of an XML literal expression is a value typed as one of the types from the `System.Xml.Linq` namespace.</span></span> <span data-ttu-id="4e5da-3557">その名前空間の型が使用できない場合、XML リテラル式では、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3557">If the types in that namespace are not available, then an XML literal expression will cause a compile-time error.</span></span> <span data-ttu-id="4e5da-3558">値は、XML リテラル式から変換コンス トラクターの呼び出しを使用して生成されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3558">The values are generated through constructor calls translated from the XML literal expression.</span></span> <span data-ttu-id="4e5da-3559">たとえば、次のようなコードがあるとします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3559">For example, the code:</span></span>

```vb
Dim book As System.Xml.Linq.XElement = _
    <book title="My book"></book>
```

<span data-ttu-id="4e5da-3560">コードとほぼ同じです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3560">is roughly equivalent to the code:</span></span>

```vb
Dim book As System.Xml.Linq.XElement = _
    New System.Xml.Linq.XElement( _
        "book", _
        New System.Xml.Linq.XAttribute("title", "My book"))
```

<span data-ttu-id="4e5da-3561">XML リテラル式には、XML ドキュメント、XML 要素、XML 処理命令、XML コメント、または CDATA セクションのフォームを実行できます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3561">An XML literal expression can take the form of an XML document, an XML element, an XML processing instruction, an XML comment, or a CDATA section.</span></span>

<span data-ttu-id="4e5da-3562">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3562">__Note.__</span></span> <span data-ttu-id="4e5da-3563">この仕様のみを含む Visual Basic 言語の動作を記述する XML の説明は十分です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3563">This specification contains only enough of a description of XML to describe the behavior of the Visual Basic language.</span></span> <span data-ttu-id="4e5da-3564">XML の詳細についてで見つかります http://www.w3.org/TR/REC-xml/します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3564">More information on XML can be found at http://www.w3.org/TR/REC-xml/.</span></span>


### <a name="lexical-rules"></a><span data-ttu-id="4e5da-3565">語彙の規則</span><span class="sxs-lookup"><span data-stu-id="4e5da-3565">Lexical rules</span></span>

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

<span data-ttu-id="4e5da-3566">XML リテラル式では、通常の Visual Basic コードの語彙の規則ではなく、XML の語彙規則を使用して解釈されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3566">XML literal expressions are interpreted using the lexical rules of XML instead of the lexical rules of regular Visual Basic code.</span></span> <span data-ttu-id="4e5da-3567">ルールの 2 つのセットは、一般に、次の点で異なります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3567">The two sets of rules generally differ in the following ways:</span></span>

* <span data-ttu-id="4e5da-3568">空白は XML で重要です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3568">White space is significant in XML.</span></span> <span data-ttu-id="4e5da-3569">その結果、XML リテラル式の文法を明示的に表して空白が許可されています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3569">As a result, the grammar for XML literal expressions explicitly states where white space is allowed.</span></span> <span data-ttu-id="4e5da-3570">要素内の文字データのコンテキストで発生するタイミングを除く、空白は保持されません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3570">Whitespace is not preserved, except when it occurs in the context of character data within an element.</span></span> <span data-ttu-id="4e5da-3571">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3571">For example:</span></span>

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

* <span data-ttu-id="4e5da-3572">XML の行の末尾に空白文字は、XML 仕様に従って正規化されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3572">XML end-of-line whitespace is normalized according to the XML specification.</span></span>

* <span data-ttu-id="4e5da-3573">XML は大文字と小文字を区別します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3573">XML is case-sensitive.</span></span> <span data-ttu-id="4e5da-3574">キーワードに大文字小文字の区別を正確に一致する必要があります。 そう、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3574">Keywords must match casing exactly, or else a compile-time error will occur.</span></span>

* <span data-ttu-id="4e5da-3575">行ターミネータは、XML 内の空白と見なされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3575">Line terminators are considered white space in XML.</span></span> <span data-ttu-id="4e5da-3576">その結果、XML リテラル式で行継続文字は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3576">As a result, no line-continuation characters are needed in XML literal expressions.</span></span>

* <span data-ttu-id="4e5da-3577">XML では、全角文字は受け入れられません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3577">XML does not accept full-width characters.</span></span> <span data-ttu-id="4e5da-3578">全角文字を使用すると、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3578">If full-width characters are used, a compile-time error will occur.</span></span>


### <a name="embedded-expressions"></a><span data-ttu-id="4e5da-3579">埋め込み式</span><span class="sxs-lookup"><span data-stu-id="4e5da-3579">Embedded expressions</span></span>

<span data-ttu-id="4e5da-3580">XML リテラル式に含めることができます*埋め込み式*します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3580">XML literal expressions can contain *embedded expressions*.</span></span> <span data-ttu-id="4e5da-3581">埋め込み式は、Visual Basic の式が評価され、埋め込み式の位置にある 1 つまたは複数の値を入力するために使用します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3581">An embedded expression is a Visual Basic expression that is evaluated and used to fill in one or more values at the location of embedded expression.</span></span>

```antlr
XMLEmbeddedExpression
    : '<' '%' '=' LineTerminator? Expression LineTerminator? '%' '>'
    ;
```

<span data-ttu-id="4e5da-3582">たとえば、次のコードは、文字列を配置`John Smith`XML 要素の値として。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3582">For example, the following code places the string `John Smith` as the value of the XML element:</span></span>

```vb
Dim name as String = "John Smith"
Dim element As System.Xml.Linq.XElement = <customer><%= name %></customer>
```

<span data-ttu-id="4e5da-3583">さまざまなコンテキストで式を埋め込むことができます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3583">Expressions can be embedded in a number of contexts.</span></span> <span data-ttu-id="4e5da-3584">たとえば、次のコードは、という名前の要素を生成します`customer`:。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3584">For example, the following code produces an element named `customer`:</span></span>

```vb
Dim name As String = "customer"
Dim element As System.Xml.Linq.XElement = <<%= name %>>John Smith</>
```

<span data-ttu-id="4e5da-3585">埋め込み式を使用できる各コンテキストは、許容される型を指定します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3585">Each context where an embedded expression can be used specifies the types that will be accepted.</span></span> <span data-ttu-id="4e5da-3586">埋め込み式の式の一部のコンテキスト内では、これは、Visual Basic コードの通常の語彙の規則が引き続き適用、たとえば、行連結文字を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3586">When within the context of the expression part of an embedded expression, the normal lexical rules for Visual Basic code still apply so, for example, line continuations must be used:</span></span>

```vb
' Visual Basic expression uses line continuation, XML does not
Dim element As System.Xml.Linq.XElement = _
    <<%= name & _
          name %>>John 
                     Smith</>
```


### <a name="xml-documents"></a><span data-ttu-id="4e5da-3587">XML ドキュメント</span><span class="sxs-lookup"><span data-stu-id="4e5da-3587">XML Documents</span></span>

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

<span data-ttu-id="4e5da-3588">XML ドキュメントの結果として型指定された値に`System.Xml.Linq.XDocument`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3588">An XML document results in a value typed as `System.Xml.Linq.XDocument`.</span></span> <span data-ttu-id="4e5da-3589">XML 1.0 の仕様とは異なり、XML リテラル式の XML ドキュメントが XML ドキュメントのプロローグ; を指定する必要はXML ドキュメントのプロローグせず、XML リテラル式は、個別のエンティティとして解釈されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3589">Unlike the XML 1.0 specification, XML documents in XML literal expressions are required to specify the XML document prologue; XML literal expressions without the XML document prologue are interpreted as their individual entity.</span></span> <span data-ttu-id="4e5da-3590">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3590">For example:</span></span>

```vb
Dim doc As System.Xml.Linq.XDocument = _
    <?xml version="1.0"?>
    <?instruction?>
    <customer>Bob</>

Dim pi As System.Xml.Linq.XProcessingInstruction = _
    <?instruction?>
```

<span data-ttu-id="4e5da-3591">XML ドキュメントは、型を持つ、任意の型の埋め込み式を含めることができます。実行時に、ただし、オブジェクトする必要がありますの要件を満たす、`XDocument`コンス トラクターまたは実行時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3591">An XML document can contain an embedded expression whose type can be any type; at runtime, however, the object must satisfy the requirements of the `XDocument` constructor or a run-time error will occur.</span></span>

<span data-ttu-id="4e5da-3592">通常の XML とは異なり XML ドキュメントの式は Dtd (文書型宣言) をサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3592">Unlike regular XML, XML document expressions do not support DTDs (Document Type Declarations).</span></span> <span data-ttu-id="4e5da-3593">また、エンコーディングの属性では、指定した場合は無視されます Xml リテラル式のエンコードは常にソース ファイルのエンコーディングと同じであるためです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3593">Also, the encoding attribute, if supplied, will be ignored since the encoding of the Xml literal expression is always the same as the encoding of the source file itself.</span></span>

<span data-ttu-id="4e5da-3594">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3594">__Note.__</span></span> <span data-ttu-id="4e5da-3595">エンコーディング属性を無視すると、まだ有効な属性をソース コードの任意の有効な Xml 1.0 ドキュメントを含める機能を維持するためにです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3595">Although the encoding attribute is ignored, it is still valid attribute in order to maintain the ability to include any valid Xml 1.0 documents in source code.</span></span>


### <a name="xml-elements"></a><span data-ttu-id="4e5da-3596">XML 要素</span><span class="sxs-lookup"><span data-stu-id="4e5da-3596">XML Elements</span></span>

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

<span data-ttu-id="4e5da-3597">XML 要素の結果として型指定された値に`System.Xml.Linq.XElement`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3597">An XML element results in a value typed as `System.Xml.Linq.XElement`.</span></span> <span data-ttu-id="4e5da-3598">通常の XML とは異なり XML 要素は、終了タグの名前を省略でき、現在のほとんどの入れ子になった要素が閉じられます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3598">Unlike regular XML, XML elements can omit the name in the closing tag and the current most-nested element will be closed.</span></span> <span data-ttu-id="4e5da-3599">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3599">For example:</span></span>

```vb
Dim name = <name>Bob</>
```

<span data-ttu-id="4e5da-3600">として型指定された値で、結果の XML 要素内の宣言を属性`System.Xml.Linq.XAttribute`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3600">Attribute declarations in an XML element result in values typed as `System.Xml.Linq.XAttribute`.</span></span> <span data-ttu-id="4e5da-3601">属性の値は、XML 仕様に従って正規化されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3601">Attribute values are normalized according to the XML specification.</span></span> <span data-ttu-id="4e5da-3602">属性の値が`Nothing`属性は作成されません、属性値の式をチェックする必要はありませんので`Nothing`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3602">When the value of an attribute is `Nothing` the attribute will not be created, so the attribute value expression will not have to be checked for `Nothing`.</span></span> <span data-ttu-id="4e5da-3603">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3603">For example:</span></span>

```vb
Dim expr = Nothing

' Throws null argument exception
Dim direct = New System.Xml.Linq.XElement( _
    "Name", _
    New System.Xml.Linq.XAttribute("Length", expr))

' Doesn't throw exception, the result is <Name/>
Dim literal = <Name Length=<%= expr %>/>
```

<span data-ttu-id="4e5da-3604">XML 要素と属性は、次の場所に入れ子になった式を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3604">XML elements and attributes can contain nested expressions in the following places:</span></span>

<span data-ttu-id="4e5da-3605">場合、埋め込み式が暗黙的に変換できる型の値をある必要があります、要素の名前`System.Xml.Linq.XName`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3605">The name of the element, in which case the embedded expression must be a value of a type implicitly convertible to `System.Xml.Linq.XName`.</span></span> <span data-ttu-id="4e5da-3606">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3606">For example:</span></span>

```vb
Dim name = <<%= "name" %>>Bob</>
```

<span data-ttu-id="4e5da-3607">場合、埋め込み式が暗黙的に変換できる型の値をある必要があります、要素の属性の名前`System.Xml.Linq.XName`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3607">The name of an attribute of the element, in which case the embedded expression must be a value of a type implicitly convertible to `System.Xml.Linq.XName`.</span></span> <span data-ttu-id="4e5da-3608">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3608">For example:</span></span>

```vb
Dim name = <name <%= "length" %>="3">Bob</>
```

<span data-ttu-id="4e5da-3609">埋め込み式のケースが任意の型の値をすることができます、要素の属性の値。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3609">The value of an attribute of the element, in which case the embedded expression can be a value of any type.</span></span> <span data-ttu-id="4e5da-3610">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3610">For example:</span></span>

```vb
Dim name = <name length=<%= 3 %>>Bob</>
```

<span data-ttu-id="4e5da-3611">場合、埋め込み式が任意の型の値をすることができます、要素の属性です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3611">An attribute of the element, in which case the embedded expression can be a value of any type.</span></span> <span data-ttu-id="4e5da-3612">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3612">For example:</span></span>

```vb
Dim name = <name <%= new XAttribute("length", 3) %>>Bob</>
```

<span data-ttu-id="4e5da-3613">場合、埋め込み式が任意の型の値をすることができます、要素のコンテンツ。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3613">The content of the element, in which case the embedded expression can be a value of any type.</span></span> <span data-ttu-id="4e5da-3614">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3614">For example:</span></span>

```vb
Dim name = <name><%= "Bob" %></>
```

<span data-ttu-id="4e5da-3615">埋め込み式の型が場合`Object()`、配列に paramarray として渡されます、`XElement`コンス トラクター。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3615">If the type of the embedded expression is `Object()`, the array will be passed as a paramarray to the `XElement` constructor.</span></span>


### <a name="xml-namespaces"></a><span data-ttu-id="4e5da-3616">XML 名前空間</span><span class="sxs-lookup"><span data-stu-id="4e5da-3616">XML Namespaces</span></span>

<span data-ttu-id="4e5da-3617">XML 要素は、XML 1.0 名前空間の仕様で定義された XML 名前空間の宣言を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3617">XML elements can contain XML namespace declarations, as defined by the XML namespaces 1.0 specification.</span></span>

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

<span data-ttu-id="4e5da-3618">名前空間を定義するのには、制限`xml`と`xmlns`適用され、コンパイル時エラーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3618">The restrictions on defining the namespaces `xml` and `xmlns` are enforced and will produce compile-time errors.</span></span> <span data-ttu-id="4e5da-3619">XML 名前空間宣言は、値の埋め込み式を含めることはできません。指定された値は空の文字列リテラルである必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3619">XML namespace declarations cannot have an embedded expression for their value; the value supplied must be a non-empty string literal.</span></span> <span data-ttu-id="4e5da-3620">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3620">For example:</span></span>

```vb
' Declares a valid namespace
Dim customer = <db:customer xmlns:db="http://example.org/database">Bob</>

' Error: xmlns cannot be re-defined
Dim bad1 = <elem xmlns:xmlns="http://example.org/namespace"/>

' Error: cannot have an embedded expression
Dim bad2 = <elem xmlns:db=<%= "http://example.org/database" %>>Bob</>
```

<span data-ttu-id="4e5da-3621">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3621">__Note.__</span></span> <span data-ttu-id="4e5da-3622">この仕様のみを含む Visual Basic 言語の動作を記述する XML 名前空間の説明は十分です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3622">This specification contains only enough of a description of XML namespace to describe the behavior of the Visual Basic language.</span></span> <span data-ttu-id="4e5da-3623">XML 名前空間の詳細についてで見つかります http://www.w3.org/TR/REC-xml-names/します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3623">More information on XML namespaces can be found at http://www.w3.org/TR/REC-xml-names/.</span></span>

<span data-ttu-id="4e5da-3624">名前空間の名前を使用して XML 要素と属性名を修飾することができます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3624">XML element and attribute names can be qualified using namespace names.</span></span> <span data-ttu-id="4e5da-3625">名前空間を正規 XML に示すようにバインドは、ファイル レベルで宣言された任意の名前空間をインポートする例外では、コンパイルで宣言された名前空間インポートので囲まれた自体と、宣言の外側のコンテキストで宣言すると見なされます環境。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3625">Namespaces are bound as in regular XML, with the exception that any namespace imports declared at the file level are considered to be declared in a context enclosing the declaration, which is itself enclosed by any namespace imports declared by the compilation environment.</span></span> <span data-ttu-id="4e5da-3626">名前空間の名前が見つからない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3626">If a namespace name cannot be found, a compile-time error occurs.</span></span> <span data-ttu-id="4e5da-3627">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3627">For example:</span></span>

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

<span data-ttu-id="4e5da-3628">要素で宣言されている XML 名前空間は、埋め込み式の中で XML リテラルには適用されません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3628">XML namespaces declared in an element do not apply to XML literals inside embedded expressions.</span></span> <span data-ttu-id="4e5da-3629">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3629">For example:</span></span>

```vb
' Error: Namespace prefix 'db' is not declared
Dim customer = _
    <db:customer xmlns:db="http://example.org/database">
        <%= <db:customer>Bob</> %>
    </>
```

<span data-ttu-id="4e5da-3630">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3630">__Note.__</span></span> <span data-ttu-id="4e5da-3631">埋め込み式は関数呼び出しをなど、何でもできるためです。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3631">This is because the embedded expression can be anything, including a function call.</span></span> <span data-ttu-id="4e5da-3632">関数呼び出しには、XML リテラル式が含まれている、プログラマが適用または無視する XML 名前空間に期待するかどうかをオフすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3632">If the function call contained an XML literal expression, it is not clear whether programmers would expect the XML namespace to be applied or ignored.</span></span>


### <a name="xml-processing-instructions"></a><span data-ttu-id="4e5da-3633">XML 処理命令</span><span class="sxs-lookup"><span data-stu-id="4e5da-3633">XML Processing Instructions</span></span>

<span data-ttu-id="4e5da-3634">XML 処理命令の結果として型指定された値に`System.Xml.Linq.XProcessingInstruction`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3634">An XML processing instruction results in a value typed as `System.Xml.Linq.XProcessingInstruction`.</span></span> <span data-ttu-id="4e5da-3635">処理命令内で有効な構文は、XML 処理命令は、埋め込み式を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3635">XML processing instructions cannot contain embedded expressions, as they are valid syntax within the processing instruction.</span></span>

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

### <a name="xml-comments"></a><span data-ttu-id="4e5da-3636">XML コメント</span><span class="sxs-lookup"><span data-stu-id="4e5da-3636">XML Comments</span></span>

<span data-ttu-id="4e5da-3637">XML コメントの結果として型指定された値に`System.Xml.Linq.XComment`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3637">An XML comment results in a value typed as `System.Xml.Linq.XComment`.</span></span> <span data-ttu-id="4e5da-3638">XML コメントは、コメント内で有効な構文は、埋め込み式を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3638">XML comments cannot contain embedded expressions, as they are valid syntax within the comment.</span></span>

```antlr
XMLComment
    : '<' '!' '-' '-' XMLCommentCharacter* '-' '-' '>'
    ;

XMLCommentCharacter
    : '<Any XMLCharacter except dash (0x002D)>'
    | '-' '<Any XMLCharacter except dash (0x002D)>'
    ;
```

### <a name="cdata-sections"></a><span data-ttu-id="4e5da-3639">CDATA セクション</span><span class="sxs-lookup"><span data-stu-id="4e5da-3639">CDATA sections</span></span>

<span data-ttu-id="4e5da-3640">CDATA セクションの結果として型指定された値に`System.Xml.Linq.XCData`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3640">A CDATA section results in a value typed as `System.Xml.Linq.XCData`.</span></span> <span data-ttu-id="4e5da-3641">CDATA セクション内の有効な構文は、CDATA セクションは、埋め込み式を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3641">CDATA sections cannot contain embedded expressions, as they are valid syntax within the CDATA section.</span></span>

```antlr
XMLCDATASection
    : '<' '!' ( 'CDATA' '[' XMLCDATASectionString? ']' )? '>'
    ;

XMLCDATASectionString
    : '<Any XMLString that does not contain the string "]]>">'
    ;
```

## <a name="xml-member-access-expressions"></a><span data-ttu-id="4e5da-3642">XML メンバー アクセス式</span><span class="sxs-lookup"><span data-stu-id="4e5da-3642">XML Member Access Expressions</span></span>

<span data-ttu-id="4e5da-3643">XML メンバー アクセス式では、XML 値のメンバーにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3643">An XML member access expression accesses the members of an XML value.</span></span>

```antlr
XMLMemberAccessExpression
    : Expression '.' LineTerminator? '<' XMLQualifiedName '>'
    | Expression '.' LineTerminator? '@' LineTerminator? '<' XMLQualifiedName '>'
    | Expression '.' LineTerminator? '@' LineTerminator? IdentifierOrKeyword
    | Expression '.' '.' '.' LineTerminator? '<' XMLQualifiedName '>'
    ;
```

<span data-ttu-id="4e5da-3644">XML メンバー アクセス式の 3 つの種類があります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3644">There are three types of XML member access expressions:</span></span>

* <span data-ttu-id="4e5da-3645">*要素へのアクセス*XML 名が 1 つのドットに従います。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3645">*Element access*, in which an XML name follows a single dot.</span></span> <span data-ttu-id="4e5da-3646">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3646">For example:</span></span>

  ```vb
  Dim customer = _
      <customer>
          <name>Bob</>
      </>
  Dim customerName = customer.<name>.Value
  ```

  <span data-ttu-id="4e5da-3647">要素へのアクセスは、関数にマップします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3647">Element access maps to the function:</span></span>

  ```vb
  Function Elements(name As System.Xml.Linq.XName) As _
      System.Collections.Generic.IEnumerable(Of _
          System.Xml.Linq.XNode)
  ```

  <span data-ttu-id="4e5da-3648">このため、上記の例と同等です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3648">So the above example is equivalent to:</span></span>

  ```vb
  Dim customerName = customer.Elements("name").Value
  ```

* <span data-ttu-id="4e5da-3649">*属性アクセス*ドットに依存して、Visual Basic 識別子で、名前に依存してドット記号、または XML で、アット マーク。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3649">*Attribute access*, in which a Visual Basic identifier follows a dot and an at sign, or an XML name follows a dot and an at sign.</span></span> <span data-ttu-id="4e5da-3650">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3650">For example:</span></span>

  ```vb
  Dim customer = <customer age="30"/>
  Dim customerAge = customer.@age
  ```

  <span data-ttu-id="4e5da-3651">属性へのアクセスは、関数にマップされます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3651">Attribute access maps to the function:</span></span>

  ```vb
  Function AttributeValue(name As System.Xml.Linq.XName) as String
  ```

  <span data-ttu-id="4e5da-3652">このため、上記の例と同等です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3652">So the above example is equivalent to:</span></span>

  ```vb
  Dim customerAge = customer.AttributeValue("age")
  ```

  <span data-ttu-id="4e5da-3653">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3653">__Note.__</span></span> <span data-ttu-id="4e5da-3654">`AttributeValue`拡張メソッド (関連する拡張機能プロパティと`Value`) 任意のアセンブリで定義されていません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3654">The `AttributeValue` extension method (as well as the related extension property `Value`) is not currently defined in any assembly.</span></span> <span data-ttu-id="4e5da-3655">拡張メンバーが必要な場合は生成されているアセンブリで自動的に定義します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3655">If the extension members are needed, they are automatically defined in the assembly being produced.</span></span>

* <span data-ttu-id="4e5da-3656">*子孫アクセス*XML の名前が 3 つのドットに従います。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3656">*Descendents access*, in which an XML names follows three dots.</span></span> <span data-ttu-id="4e5da-3657">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3657">For example:</span></span>

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

  <span data-ttu-id="4e5da-3658">子孫へのアクセスは、関数にマップします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3658">Descendents access maps to the function:</span></span>

  ```vb
  Function Descendents(name As System.Xml.Linq.XName) As _
      System.Collections.Generic.IEnumerable(Of _
          System.Xml.Linq.XElement)
  ```

  <span data-ttu-id="4e5da-3659">このため、上記の例と同等です。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3659">So the above example is equivalent to:</span></span>

  ```vb
  Dim customers = company.Descendants("customer")
  ```

<span data-ttu-id="4e5da-3660">XML メンバー アクセス式の基本の式は、値を指定する必要があり、型でなければなりません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3660">The base expression of an XML member access expression must be a value and must be of the type:</span></span>

* <span data-ttu-id="4e5da-3661">要素または子孫にアクセスする場合`System.Xml.Linq.XContainer`または派生型、または`System.Collections.Generic.IEnumerable(Of T)`または派生型では、場所`T`は`System.Xml.Linq.XContainer`または派生型。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3661">If an element or descendents access,  `System.Xml.Linq.XContainer` or a derived type, or `System.Collections.Generic.IEnumerable(Of T)` or a derived type, where `T` is `System.Xml.Linq.XContainer` or a derived type.</span></span>

* <span data-ttu-id="4e5da-3662">場合、属性へのアクセス`System.Xml.Linq.XElement`または派生型、または`System.Collections.Generic.IEnumerable(Of T)`または派生型では、場所`T`は`System.Xml.Linq.XElement`または派生型。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3662">If an attribute access, `System.Xml.Linq.XElement` or a derived type, or `System.Collections.Generic.IEnumerable(Of T)` or a derived type, where `T` is `System.Xml.Linq.XElement` or a derived type.</span></span>

<span data-ttu-id="4e5da-3663">XML メンバー アクセス式の名前を空にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3663">Names in XML member access expressions cannot be empty.</span></span> <span data-ttu-id="4e5da-3664">名前空間で修飾、インポートによって定義された任意の名前空間を使用できます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3664">They can be namespace qualified, using any namespaces defined by imports.</span></span> <span data-ttu-id="4e5da-3665">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3665">For example:</span></span>

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

<span data-ttu-id="4e5da-3666">XML メンバー アクセス式で、山かっこと名の間または dot(s) 後は、空白が許可されていません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3666">Whitespace is not allowed after the dot(s) in an XML member access expression, or between the angle brackets and the name.</span></span> <span data-ttu-id="4e5da-3667">例えば:</span><span class="sxs-lookup"><span data-stu-id="4e5da-3667">For example:</span></span>

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

<span data-ttu-id="4e5da-3668">場合内の型、`System.Xml.Linq`名前空間は、使用し、XML メンバー アクセス式には、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3668">If the types in the `System.Xml.Linq` namespace are not available, then an XML member access expression will cause a compile-time error.</span></span>


## <a name="await-operator"></a><span data-ttu-id="4e5da-3669">Await 演算子</span><span class="sxs-lookup"><span data-stu-id="4e5da-3669">Await Operator</span></span>

<span data-ttu-id="4e5da-3670">Await 演算子がセクションで説明されている非同期メソッドに関連する[非同期メソッド](statements.md#async-methods)します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3670">The await operator is related to async methods, which are described in Section [Async Methods](statements.md#async-methods).</span></span>

```antlr
AwaitOperatorExpression
    : 'Await' Expression
    ;
```

<span data-ttu-id="4e5da-3671">`Await` 予約語は、すぐ外側のメソッドまたはラムダ式が表示される場合、`Async`修飾子は、場合に、`Await`を後に表示`Async`修飾子; は予約されている他の場所。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3671">`Await` is a reserved word if the immediately enclosing method or lambda expression in which it appears has an `Async` modifier, and if the `Await` appears after that `Async` modifier; it is unreserved elsewhere.</span></span> <span data-ttu-id="4e5da-3672">ないプリプロセッサ ディレクティブ内に確保できます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3672">It is also unreserved in preprocessor directives.</span></span> <span data-ttu-id="4e5da-3673">Await 演算子は予約語であるメソッドまたはラムダ式の本体でのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3673">The await operator is only allowed in the body of a method or lambda expressions where it is a reserved word.</span></span> <span data-ttu-id="4e5da-3674">すぐ外側のメソッドまたはラムダ内で await 式には発生しませんの本体内で、`Catch`または`Finally`ブロック、または内部の本文を`SyncLock`ステートメント、または内部クエリ式。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3674">Within the immediately enclosing method or lambda, an await expression may not occur inside the body of a `Catch` or `Finally` block, nor inside the body of a `SyncLock` statement, nor inside a query expression.</span></span>

<span data-ttu-id="4e5da-3675">Await 演算子は、1 つの式の値として分類する必要があります型を持つ必要があります、*待機可能物*型、または`Object`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3675">The await operator takes a single expression which must be classified as a value and whose type must be an *awaitable* type, or `Object`.</span></span> <span data-ttu-id="4e5da-3676">型が場合`Object`し、すべての処理が実行時まで延期されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3676">If its type is `Object` then all processing is deferred until run-time.</span></span> <span data-ttu-id="4e5da-3677">型`C`する場合は、次のすべてに該当する待機可能なことを言います。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3677">A type `C` is said to be awaitable if all of the following are true:</span></span>

* <span data-ttu-id="4e5da-3678">`C` アクセス可能なインスタンスまたは拡張メソッドがという名前を含む`GetAwaiter`引数がありませんが、いくつかの型を返す`E`;</span><span class="sxs-lookup"><span data-stu-id="4e5da-3678">`C` contains an accessible instance or extension method named `GetAwaiter` which has no arguments and which returns some type `E`;</span></span>

* <span data-ttu-id="4e5da-3679">`E` という名前の読み取り可能なインスタンスまたは拡張機能プロパティが含まれています`IsCompleted`引数をとらないし、; ブール値の型があります</span><span class="sxs-lookup"><span data-stu-id="4e5da-3679">`E` contains a readable instance or extension property named `IsCompleted` which takes no arguments and has type Boolean;</span></span>

* <span data-ttu-id="4e5da-3680">`E` アクセス可能なインスタンスまたは拡張メソッドがという名前を含む`GetResult`引数ですこれではありません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3680">`E` contains an accessible instance or extension method named `GetResult` which takes no arguments;</span></span>

* <span data-ttu-id="4e5da-3681">`E` いずれかを実装して`System.Runtime.CompilerServices.INotifyCompletion`または`ICriticalNotifyCompletion`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3681">`E` implements either `System.Runtime.CompilerServices.INotifyCompletion` or `ICriticalNotifyCompletion`.</span></span>

<span data-ttu-id="4e5da-3682">場合`GetResult`が、 `Sub`、await 式は void として分類されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3682">If `GetResult` was a `Sub`, then the await expression is classified as void.</span></span> <span data-ttu-id="4e5da-3683">それ以外の場合、await 式は値として分類され、その型は、戻り値の型、`GetResult`メソッド。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3683">Otherwise, the await expression is classified as a value and its type is the return type of the `GetResult` method.</span></span>

<span data-ttu-id="4e5da-3684">待機できるクラスの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3684">Here is an example of a class that can be awaited:</span></span>

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

<span data-ttu-id="4e5da-3685">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="4e5da-3685">__Note.__</span></span> <span data-ttu-id="4e5da-3686">ライブラリの作成者が同じで、継続のデリゲートが呼び出されてパターンに従うお勧めします`SynchronizationContext`としてその`OnCompleted`自体でが呼び出されました。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3686">Library authors are recommended to follow the pattern that they invoke the continuation delegate on the same `SynchronizationContext` as their `OnCompleted` was itself invoked on.</span></span> <span data-ttu-id="4e5da-3687">また、再開のデリゲートは実行されませんが同期的に内、`OnCompleted`スタック オーバーフローが発生するためのメソッド: 代わりに、デリゲートする必要がありますキューに置かれる後続の実行。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3687">Also, the resumption delegate should not be executed synchronously within the `OnCompleted` method since that can lead to stack overflow: instead, the delegate should be queued for subsequent execution.</span></span>

<span data-ttu-id="4e5da-3688">コントロール フローに達すると、`Await`演算子、動作は次のようにします。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3688">When control flow reaches an `Await` operator, behavior is as follows.</span></span>

1.  <span data-ttu-id="4e5da-3689">`GetAwaiter` Await のオペランドのメソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3689">The `GetAwaiter` method of the await operand is invoked.</span></span> <span data-ttu-id="4e5da-3690">この呼び出しの結果と呼ばれる、 *awaiter*します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3690">The result of this invocation is termed the *awaiter*.</span></span>

2.  <span data-ttu-id="4e5da-3691">Awaiter の`IsCompleted`プロパティを取得します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3691">The awaiter's `IsCompleted` property is retrieved.</span></span> <span data-ttu-id="4e5da-3692">結果が true の場合。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3692">If the result is true then:</span></span>

    21. <span data-ttu-id="4e5da-3693">`GetResult` Awaiter のメソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3693">The `GetResult` method of the awaiter is invoked.</span></span> <span data-ttu-id="4e5da-3694">場合`GetResult`が関数の場合、await 式の値は、この関数の戻り値。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3694">If `GetResult` was a function, then the value of the await expression is the return value of this function.</span></span>

3.  <span data-ttu-id="4e5da-3695">IsCompleted プロパティが true でない場合。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3695">If the IsCompleted property isn't true then:</span></span>

    31. <span data-ttu-id="4e5da-3696">いずれか`ICriticalNotifyCompletion.UnsafeOnCompleted`awaiter で呼び出される (場合 awaiter の型`E`実装`ICriticalNotifyCompletion`) または`INotifyCompletion.OnCompleted`(それ以外の場合)。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3696">Either `ICriticalNotifyCompletion.UnsafeOnCompleted` is invoked on the awaiter (if the awaiter's type `E` implements `ICriticalNotifyCompletion`) or `INotifyCompletion.OnCompleted` (otherwise).</span></span> <span data-ttu-id="4e5da-3697">どちらの場合でも、パス、*再開デリゲート*非同期メソッドの現在のインスタンスに関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3697">In both cases it passes a *resumption delegate* associated with the current instance of the async method.</span></span>

    32. <span data-ttu-id="4e5da-3698">現在の非同期メソッドのインスタンスの制御点は中断しています。 でフローが再開、*現在の呼び出し元*(セクションで定義されている[非同期メソッド](statements.md#async-methods))。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3698">The control point of the current async method instance is suspended, and control flow resumes in the *current caller* (defined in Section [Async Methods](statements.md#async-methods)).</span></span>

    33. <span data-ttu-id="4e5da-3699">後で再開デリゲートが呼び出される場合</span><span class="sxs-lookup"><span data-stu-id="4e5da-3699">If later the resumption delegate is invoked,</span></span>

        331. <span data-ttu-id="4e5da-3700">再開のデリゲートがまず復元`System.Threading.Thread.CurrentThread.ExecutionContext`された時点を`OnCompleted`が呼び出されました</span><span class="sxs-lookup"><span data-stu-id="4e5da-3700">the resumption delegate first restores `System.Threading.Thread.CurrentThread.ExecutionContext` to what it was at the time `OnCompleted` was called,</span></span>
        332. <span data-ttu-id="4e5da-3701">非同期メソッドのインスタンスの管理ポイントでの制御フローを再開し、(を参照してください[非同期メソッド](statements.md#async-methods))、</span><span class="sxs-lookup"><span data-stu-id="4e5da-3701">then it resumes control flow at the control point of the async method instance (see Section [Async Methods](statements.md#async-methods)),</span></span>
        333. <span data-ttu-id="4e5da-3702">呼び出す場合に、 `GetResult` 2.1 上記のように、awaiter のメソッド。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3702">where it calls the `GetResult` method of the awaiter, as in 2.1 above.</span></span>

<span data-ttu-id="4e5da-3703">Await オペランドの型がオブジェクトの場合、この動作は、実行時まで延期されます。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3703">If the await operand has type Object, then this behavior is deferred until runtime:</span></span>

- <span data-ttu-id="4e5da-3704">手順 1 には呼び出し元の GetAwaiter(); 引数なしで、省略可能なパラメーターを受け取るインスタンス メソッドに実行時にバインド可能性がありますので。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3704">Step 1 is accomplished by calling GetAwaiter() with no arguments; it may therefore bind at runtime to instance methods which take optional parameters.</span></span>
- <span data-ttu-id="4e5da-3705">引数なしで IsCompleted() プロパティを取得することによって、ブール値への組み込みの変換を試みることで、手順 2 が実現します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3705">Step 2 is accomplished by retrieving the IsCompleted() property with no arguments, and by attempting an intrinsic conversion to Boolean.</span></span>
- <span data-ttu-id="4e5da-3706">手順 3. a にはしようとすると、 `TryCast(awaiter, ICriticalNotifyCompletion)`、これが失敗した場合と`DirectCast(awaiter, INotifyCompletion)`します。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3706">Step 3.a is accomplished by attempting `TryCast(awaiter, ICriticalNotifyCompletion)`, and if this fails then `DirectCast(awaiter, INotifyCompletion)`.</span></span>

<span data-ttu-id="4e5da-3707">3. a に渡される再開デリゲートは 1 回のみ呼び出すことがあります。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3707">The resumption delegate passed in 3.a may only be invoked once.</span></span> <span data-ttu-id="4e5da-3708">複数回呼び出された場合、動作は定義されません。</span><span class="sxs-lookup"><span data-stu-id="4e5da-3708">If it is invoked more than once, the behavior is undefined.</span></span>
