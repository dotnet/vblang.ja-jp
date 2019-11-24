---
ms.openlocfilehash: e49e116e60e724bcd8f1148c8aad9d11dfc92e74
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306049"
---
# <a name="statements"></a><span data-ttu-id="eb747-101">ステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-101">Statements</span></span>

<span data-ttu-id="eb747-102">ステートメントは実行可能コードを表します。</span><span class="sxs-lookup"><span data-stu-id="eb747-102">Statements represent executable code.</span></span>

```antlr
Statement
    : LabelDeclarationStatement
    | LocalDeclarationStatement
    | WithStatement
    | SyncLockStatement
    | EventStatement
    | AssignmentStatement
    | InvocationStatement
    | ConditionalStatement
    | LoopStatement
    | ErrorHandlingStatement
    | BranchStatement
    | ArrayHandlingStatement
    | UsingStatement
    | AwaitStatement
    | YieldStatement
    ;
```

<span data-ttu-id="eb747-103">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="eb747-103">__Note.__</span></span> <span data-ttu-id="eb747-104">Microsoft Visual Basic Compiler では、キーワードまたは識別子で始まるステートメントのみを使用できます。</span><span class="sxs-lookup"><span data-stu-id="eb747-104">The Microsoft Visual Basic Compiler only allows statements which start with a keyword or an identifier.</span></span> <span data-ttu-id="eb747-105">したがって、たとえば、呼び出しステートメント "`Call (Console).WriteLine`" は許可されていますが、呼び出しステートメント "`(Console).WriteLine`" は許可されていません。</span><span class="sxs-lookup"><span data-stu-id="eb747-105">Thus, for instance, the invocation statement "`Call (Console).WriteLine`" is allowed, but the invocation statement "`(Console).WriteLine`" is not.</span></span>

## <a name="control-flow"></a><span data-ttu-id="eb747-106">制御フロー</span><span class="sxs-lookup"><span data-stu-id="eb747-106">Control Flow</span></span>

<span data-ttu-id="eb747-107">*制御フロー*は、ステートメントと式が実行される順序です。</span><span class="sxs-lookup"><span data-stu-id="eb747-107">*Control flow* is the sequence in which statements and expressions are executed.</span></span> <span data-ttu-id="eb747-108">実行順序は、特定のステートメントまたは式によって異なります。</span><span class="sxs-lookup"><span data-stu-id="eb747-108">The order of execution depends on the particular statement or expression.</span></span>

<span data-ttu-id="eb747-109">たとえば、加算演算子 (セクション[加算演算子](expressions.md#addition-operator)) を評価する場合は、最初に左オペランドが評価され、次に右オペランド、次に演算子自体が評価されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-109">For example, when evaluating an addition operator (Section [Addition Operator](expressions.md#addition-operator)), first the left operand is evaluated, then the right operand, and then the operator itself.</span></span> <span data-ttu-id="eb747-110">まず最初のサブステートメントを実行し、次にブロックのステートメントを使用して1つずつ続行することによって、ブロックが実行されます (セクション[ブロックとラベル](statements.md#blocks-and-labels))。</span><span class="sxs-lookup"><span data-stu-id="eb747-110">Blocks are executed (Section [Blocks and Labels](statements.md#blocks-and-labels)) by first executing their first substatement, and then proceeding one by one through the statements of the block.</span></span>

<span data-ttu-id="eb747-111">この順序で暗黙的に使用されるのは、次に実行される操作である*制御ポイント*の概念です。</span><span class="sxs-lookup"><span data-stu-id="eb747-111">Implicit in this ordering is the concept of a *control point*, which is the next operation to be executed.</span></span> <span data-ttu-id="eb747-112">メソッドが呼び出されたとき (または "呼び出された")、メソッドの*インスタンス*を作成するとします。</span><span class="sxs-lookup"><span data-stu-id="eb747-112">When a method is invoked (or "called"), we say it creates an *instance* of the method.</span></span> <span data-ttu-id="eb747-113">メソッドインスタンスは、メソッドのパラメーターとローカル変数の独自のコピーと、独自の制御点で構成されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-113">A method instance consists of its own copy of the method's parameters and local variables, and its own control point.</span></span>

### <a name="regular-methods"></a><span data-ttu-id="eb747-114">通常のメソッド</span><span class="sxs-lookup"><span data-stu-id="eb747-114">Regular Methods</span></span>

<span data-ttu-id="eb747-115">通常のメソッドの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="eb747-115">Here is an example of a regular method</span></span>

```vb
Function Test() As Integer
    Console.WriteLine("hello")
    Return 1
End Sub

Dim x = Test()    ' invokes the function, prints "hello", assigns 1 to x
```

<span data-ttu-id="eb747-116">通常のメソッドが呼び出されると、</span><span class="sxs-lookup"><span data-stu-id="eb747-116">When a regular method is invoked,</span></span>

1. <span data-ttu-id="eb747-117">最初に、その呼び出しに固有のメソッドのインスタンスが作成されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-117">First an instance of the method is created specific to that invocation.</span></span> <span data-ttu-id="eb747-118">このインスタンスには、メソッドのすべてのパラメーターとローカル変数のコピーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="eb747-118">This instance includes a copy of all parameters and local variables of the method.</span></span>
2. <span data-ttu-id="eb747-119">その後、すべてのパラメーターが、指定された値とそのすべてのローカル変数の型の既定値に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-119">Then all of its parameters are initialized to the supplied values, and all of its local variables to the default values of their types.</span></span>
3. <span data-ttu-id="eb747-120">`Function`の場合、暗黙的なローカル変数も、関数の名前が関数の*戻り*値の型であり、その型が関数の戻り値の型であり、初期値がその型の既定値である関数の戻り値の変数と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="eb747-120">In the case of a `Function`, an implicit local variable is also initialized called the *function return variable* whose name is the function's name, whose type is the return type of the function and whose initial value is the default of its type.</span></span>
4. <span data-ttu-id="eb747-121">次に、メソッドインスタンスの制御ポイントがメソッド本体の最初のステートメントで設定され、メソッドの本体がそこからすぐに実行を開始します (セクション[ブロックとラベル](statements.md#blocks-and-labels))。</span><span class="sxs-lookup"><span data-stu-id="eb747-121">The method instance's control point is then set at the first statement of the method body, and the method body immediately starts to execute from there (Section [Blocks and Labels](statements.md#blocks-and-labels)).</span></span>

<span data-ttu-id="eb747-122">制御フローがメソッド本体を正常に終了すると、その end をマークする `End Function` または `End Sub` に到達するか、明示的な `Return` または `Exit` ステートメント制御フローがメソッドインスタンスの呼び出し元に戻ります。</span><span class="sxs-lookup"><span data-stu-id="eb747-122">When control flow exits the method body normally - through reaching the `End Function` or `End Sub` that mark its end, or through an explicit `Return` or `Exit` statement - control flow returns to the caller of the method instance.</span></span> <span data-ttu-id="eb747-123">関数の戻り変数がある場合、呼び出しの結果はこの変数の最終的な値になります。</span><span class="sxs-lookup"><span data-stu-id="eb747-123">If there is a function return variable, the result of the invocation is the final value of this variable.</span></span>

<span data-ttu-id="eb747-124">制御フローがハンドルされない例外によってメソッド本体を終了すると、その例外は呼び出し元に反映されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-124">When control flow exits the method body through an unhandled exception, that exception is propagated to the caller.</span></span>

<span data-ttu-id="eb747-125">制御フローが終了した後は、メソッドインスタンスへのライブ参照がなくなります。</span><span class="sxs-lookup"><span data-stu-id="eb747-125">After control flow has exited, there are no longer any live references to the method instance.</span></span> <span data-ttu-id="eb747-126">メソッドインスタンスがローカル変数またはパラメーターのコピーへの参照のみを保持している場合は、ガベージコレクションされる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-126">If the method instance held the only references to its copy of local variables or parameters, then they may be garbage collected.</span></span>

### <a name="iterator-methods"></a><span data-ttu-id="eb747-127">Iterator メソッド</span><span class="sxs-lookup"><span data-stu-id="eb747-127">Iterator Methods</span></span>

<span data-ttu-id="eb747-128">反復子メソッドは、シーケンスを生成する便利な方法として使用されます。これは、`For Each` ステートメントで使用できます。</span><span class="sxs-lookup"><span data-stu-id="eb747-128">Iterator methods are used as a convenient way to generate a sequence, one which can be consumed by the `For Each` statement.</span></span> <span data-ttu-id="eb747-129">反復子メソッドは、`Yield` ステートメント (Section [Yield ステートメント](statements.md#yield-statement)) を使用してシーケンスの要素を指定します。</span><span class="sxs-lookup"><span data-stu-id="eb747-129">Iterator methods use the `Yield` statement (Section [Yield Statement](statements.md#yield-statement)) to provide elements of the sequence.</span></span> <span data-ttu-id="eb747-130">(`Yield` ステートメントのない反復子メソッドは、空のシーケンスを生成します)。</span><span class="sxs-lookup"><span data-stu-id="eb747-130">(An iterator method with no `Yield` statements will produce an empty sequence).</span></span> <span data-ttu-id="eb747-131">Iterator メソッドの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="eb747-131">Here is an example of an iterator method:</span></span>

```vb
Iterator Function Test() As IEnumerable(Of Integer)
    Console.WriteLine("hello")
    Yield 1
    Yield 2
End Function

Dim en = Test()
For Each x In en          ' prints "hello" before the first x
    Console.WriteLine(x)  ' prints "1" and then "2"
Next
```

<span data-ttu-id="eb747-132">戻り値の型が `IEnumerator(Of T)`である反復子メソッドが呼び出されると、</span><span class="sxs-lookup"><span data-stu-id="eb747-132">When an iterator method is invoked whose return type is `IEnumerator(Of T)`,</span></span>

1. <span data-ttu-id="eb747-133">最初に、その呼び出しに固有の反復子メソッドのインスタンスが作成されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-133">First an instance of the iterator method is created specific to that invocation.</span></span> <span data-ttu-id="eb747-134">このインスタンスには、メソッドのすべてのパラメーターとローカル変数のコピーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="eb747-134">This instance includes a copy of all parameters and local variables of the method.</span></span>
2. <span data-ttu-id="eb747-135">その後、すべてのパラメーターが、指定された値とそのすべてのローカル変数の型の既定値に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-135">Then all of its parameters are initialized to the supplied values, and all of its local variables to the default values of their types.</span></span>
3. <span data-ttu-id="eb747-136">暗黙的なローカル変数は、*反復子の現在の変数*とも呼ばれ、その型が `T` であり、初期値がその型の既定値であることを示します。</span><span class="sxs-lookup"><span data-stu-id="eb747-136">An implicit local variable is also initialized called the *iterator current variable*, whose type is `T` and whose initial value is the default of its type.</span></span>
4. <span data-ttu-id="eb747-137">メソッドインスタンスの制御点は、メソッド本体の最初のステートメントで設定されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-137">The method instance's control point is then set at the first statement of the method body.</span></span>
5. <span data-ttu-id="eb747-138">次に、このメソッドインスタンスに関連付けられている*反復子オブジェクト*が作成されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-138">An *iterator object* is then created, associated with this method instance.</span></span> <span data-ttu-id="eb747-139">反復子オブジェクトは、宣言された戻り値の型を実装し、以下に示す動作を持ちます。</span><span class="sxs-lookup"><span data-stu-id="eb747-139">The iterator object implements the declared return type and has behavior as described below.</span></span>
6. <span data-ttu-id="eb747-140">制御フローは、呼び出し元で*すぐ*に再開され、呼び出しの結果が反復子オブジェクトになります。</span><span class="sxs-lookup"><span data-stu-id="eb747-140">Control flow is then resumed *immediately* in the caller, and the result of the invocation is the iterator object.</span></span> <span data-ttu-id="eb747-141">この転送は反復子メソッドのインスタンスを終了せずに実行され、finally ハンドラーは実行されないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="eb747-141">Note that this transfer is done without exiting the iterator method instance, and does not cause finally handlers to execute.</span></span> <span data-ttu-id="eb747-142">メソッドインスタンスは反復子オブジェクトによって参照されていますが、反復子オブジェクトへのライブ参照が存在する限り、ガベージコレクションされません。</span><span class="sxs-lookup"><span data-stu-id="eb747-142">The method instance is still referenced by the iterator object, and will not be garbage collected so long as there exists a live reference to the iterator object.</span></span>

<span data-ttu-id="eb747-143">反復子オブジェクトの `Current` プロパティにアクセスすると、呼び出しの*現在の変数*が返されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-143">When the iterator object's `Current` property is accessed, the *current variable* of the invocation is returned.</span></span>

<span data-ttu-id="eb747-144">反復子オブジェクトの `MoveNext` メソッドが呼び出されると、呼び出しで新しいメソッドインスタンスは作成されません。</span><span class="sxs-lookup"><span data-stu-id="eb747-144">When the iterator object's `MoveNext` method is invoked, the invocation does not create a new method instance.</span></span> <span data-ttu-id="eb747-145">代わりに、既存のメソッドインスタンス (およびそのコントロールポイントとローカル変数およびパラメーター) が使用されます。これは、iterator メソッドが最初に呼び出されたときに作成されたインスタンスです。</span><span class="sxs-lookup"><span data-stu-id="eb747-145">Instead the existing method instance is used (and its control point and local variables and parameters) - the instance that was created when the iterator method was first invoked.</span></span> <span data-ttu-id="eb747-146">制御フローは、そのメソッドインスタンスの制御点で実行を再開し、反復子メソッドの本体を通常どおりに進めます。</span><span class="sxs-lookup"><span data-stu-id="eb747-146">Control flow resumes execution at the control point of that method instance, and proceeds through the body of the iterator method as normal.</span></span>

<span data-ttu-id="eb747-147">反復子オブジェクトの `Dispose` メソッドが呼び出されると、既存のメソッドインスタンスが使用されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-147">When the iterator object's `Dispose` method is invoked, again the existing method instance is used.</span></span> <span data-ttu-id="eb747-148">制御フローは、そのメソッドインスタンスの制御ポイントで再開されますが、`Exit Function` のステートメントが次の操作であるかのように直ちに動作します。</span><span class="sxs-lookup"><span data-stu-id="eb747-148">Control flow resumes at the control point of that method instance, but then immediately behaves as if an `Exit Function` statement were the next operation.</span></span>

<span data-ttu-id="eb747-149">反復子オブジェクトでの `MoveNext` または `Dispose` の呼び出しに関する上記の動作について説明するのは、その反復子オブジェクトに対する以前の `MoveNext` または `Dispose` のすべての呼び出しが既に呼び出し元に返されている場合のみです。</span><span class="sxs-lookup"><span data-stu-id="eb747-149">The above descriptions of behavior for invocation of `MoveNext` or `Dispose` on an iterator object only apply if all previous invocations of `MoveNext` or `Dispose` on that iterator object have already returned to their callers.</span></span> <span data-ttu-id="eb747-150">存在しない場合、動作は定義されていません。</span><span class="sxs-lookup"><span data-stu-id="eb747-150">If they haven't, then the behavior is undefined.</span></span>

<span data-ttu-id="eb747-151">制御フローが、終了をマークする `End Function` に到達するまで、または明示的な `Return` または `Exit` ステートメントを使用して反復子メソッド本体を終了する場合、反復子メソッドのインスタンスを再開するために、反復子オブジェクトに対する `MoveNext` または `Dispose` 関数の呼び出しのコンテキストで実行する必要があります</span><span class="sxs-lookup"><span data-stu-id="eb747-151">When control flow exits the iterator method body normally -- through reaching the `End Function` that mark its end, or through an explicit `Return` or `Exit` statement -- it must have done so in the context of an invocation of `MoveNext` or `Dispose` function on an iterator object to resume the iterator method instance, and it will have been using the method instance that was created when the iterator method was first invoked.</span></span> <span data-ttu-id="eb747-152">そのインスタンスの制御点は `End Function` ステートメントのままになり、制御フローが呼び出し元で再開されます。また、`MoveNext` の呼び出しによって再開された場合は、`False` の値が呼び出し元に返されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-152">The control point of that instance is left at the `End Function` statement, and control flow resumes in the caller; and if it had been resumed by a call to `MoveNext` then the value `False` is returned to the caller.</span></span>

<span data-ttu-id="eb747-153">制御フローがハンドルされない例外によって反復子メソッド本体を終了すると、例外が呼び出し元に反映されます。これは、`MoveNext` または `Dispose`の呼び出しのいずれかになります。</span><span class="sxs-lookup"><span data-stu-id="eb747-153">When control flow exits the iterator method body through an unhandled exception, then the exception is propagated to the caller, which again will be either an invocation of `MoveNext` or of `Dispose`.</span></span>

<span data-ttu-id="eb747-154">反復子関数のその他の考えられる戻り値の型と同様に、</span><span class="sxs-lookup"><span data-stu-id="eb747-154">As for the other possible return types of an iterator function,</span></span>

* <span data-ttu-id="eb747-155">一部の `T`に対して戻り値の型が `IEnumerable(Of T)` である反復子メソッドが呼び出されると、メソッド内のすべてのパラメーターの反復子メソッドの呼び出しに固有のインスタンスが最初に作成されます。このメソッドは、指定された値で初期化されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-155">When an iterator method is invoked whose return type is `IEnumerable(Of T)` for some `T`, an instance is first created -- specific to that invocation of the iterator method -- of all parameters in the method, and they are initialized with the supplied values.</span></span> <span data-ttu-id="eb747-156">呼び出しの結果は、戻り値の型を実装するオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="eb747-156">The result of the invocation is an an object which implements the return type.</span></span> <span data-ttu-id="eb747-157">このオブジェクトの `GetEnumerator` メソッドを呼び出すと、メソッド内のすべてのパラメーターとローカル変数の `GetEnumerator` の呼び出しに固有のインスタンスが作成されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-157">Should this object's `GetEnumerator` method be called, it creates an instance -- specific to that invocation of `GetEnumerator` -- of all parameters and local variables in the method.</span></span> <span data-ttu-id="eb747-158">このメソッドは、既に保存されている値に対してパラメーターを初期化し、上記の反復子メソッドの場合と同様に処理を続行します。</span><span class="sxs-lookup"><span data-stu-id="eb747-158">It initializes the parameters to the values already saved, and proceeds as for the iterator method above.</span></span>
* <span data-ttu-id="eb747-159">戻り値の型が非ジェネリックインターフェイス `IEnumerator`である反復子メソッドが呼び出されると、動作は `IEnumerator(Of Object)`の場合とまったく同じになります。</span><span class="sxs-lookup"><span data-stu-id="eb747-159">When an iterator method is invoked whose return type is the non-generic interface `IEnumerator`, the behavior is exactly as for `IEnumerator(Of Object)`.</span></span>
* <span data-ttu-id="eb747-160">戻り値の型が非ジェネリックインターフェイス `IEnumerable`である反復子メソッドが呼び出されると、動作は `IEnumerable(Of Object)`の場合とまったく同じになります。</span><span class="sxs-lookup"><span data-stu-id="eb747-160">When an iterator method is invoked whose return type is the non-generic interface `IEnumerable`, the behavior is exactly as for `IEnumerable(Of Object)`.</span></span>

### <a name="async-methods"></a><span data-ttu-id="eb747-161">非同期メソッド</span><span class="sxs-lookup"><span data-stu-id="eb747-161">Async Methods</span></span>

<span data-ttu-id="eb747-162">非同期メソッドは、アプリケーションの UI をブロックするなど、長時間実行される作業を行うための便利な方法です。</span><span class="sxs-lookup"><span data-stu-id="eb747-162">Async methods are a convenient way to do long-running work without for example blocking the UI of an application.</span></span> <span data-ttu-id="eb747-163">*非同期は非同期*であるため、非同期メソッドの呼び出し元はすぐに実行を再開することを意味しますが、非同期メソッドのインスタンスの最終的な完了は、後で後で行われる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-163">Async is short for *Asynchronous* - it means that the caller of the async method will resume its execution promptly, but the eventual completion of the async method's instance may happen at some later time in the future.</span></span> <span data-ttu-id="eb747-164">慣例により、非同期メソッドには "Async" というサフィックスが付いています。</span><span class="sxs-lookup"><span data-stu-id="eb747-164">By convention async methods are named with the suffix "Async".</span></span>

```vb
Async Function TestAsync() As Task(Of String)
    Console.WriteLine("hello")
    Await Task.Delay(100)
    Return "world"
End Function

Dim t = TestAsync()         ' prints "hello"
Console.WriteLine(Await t)  ' prints "world"
```

<span data-ttu-id="eb747-165">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="eb747-165">__Note.__</span></span> <span data-ttu-id="eb747-166">非同期メソッドは、バックグラウンドスレッドでは実行され*ません*。</span><span class="sxs-lookup"><span data-stu-id="eb747-166">Async methods are *not* run on a background thread.</span></span> <span data-ttu-id="eb747-167">代わりに、メソッドが `Await` 演算子によって自身を中断し、何らかのイベントに応答して再開されるようにスケジュールすることができます。</span><span class="sxs-lookup"><span data-stu-id="eb747-167">Instead they allow a method to suspend itself through the `Await` operator, and schedule itself to be resumed in response to some event.</span></span>

<span data-ttu-id="eb747-168">非同期メソッドが呼び出されたとき</span><span class="sxs-lookup"><span data-stu-id="eb747-168">When an async method is invoked</span></span>

1. <span data-ttu-id="eb747-169">最初に、その呼び出しに固有の非同期メソッドのインスタンスが作成されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-169">First an instance of the async method is created specific to that invocation.</span></span> <span data-ttu-id="eb747-170">このインスタンスには、メソッドのすべてのパラメーターとローカル変数のコピーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="eb747-170">This instance includes a copy of all parameters and local variables of the method.</span></span>
2. <span data-ttu-id="eb747-171">その後、すべてのパラメーターが、指定された値とそのすべてのローカル変数の型の既定値に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-171">Then all of its parameters are initialized to the supplied values, and all of its local variables to the default values of their types.</span></span>
3. <span data-ttu-id="eb747-172">一部の `T`に対して戻り値の型 `Task(Of T)` を持つ非同期メソッドの場合、暗黙的なローカル変数も*タスクの戻り変数*と呼ばれます。この変数の型は `T` であり、初期値は `T`の既定値です。</span><span class="sxs-lookup"><span data-stu-id="eb747-172">In the case of an async method with return type `Task(Of T)` for some `T`, an implicit local variable is also initialized called the *task return variable*, whose type is `T` and whose initial value is the default of `T`.</span></span>
4. <span data-ttu-id="eb747-173">非同期メソッドが、戻り値の型が `Task` または `T`の `Task(Of T)` の `Function` である場合は、現在の呼び出しに関連付けられた、その型のオブジェクトが暗黙的に作成されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-173">If the async method is a `Function` with return type `Task` or `Task(Of T)` for some `T`, then an object of that type implicitly created, associated with the current invocation.</span></span> <span data-ttu-id="eb747-174">これは、 *async オブジェクト*と呼ばれ、非同期メソッドのインスタンスを実行することによって実行される将来の作業を表します。</span><span class="sxs-lookup"><span data-stu-id="eb747-174">This is called an *async object* and represents the future work that will be done by executing the instance of the async method.</span></span> <span data-ttu-id="eb747-175">この非同期メソッドインスタンスの呼び出し元でコントロールが再開されると、呼び出し元はこの非同期オブジェクトを呼び出しの結果として受け取ります。</span><span class="sxs-lookup"><span data-stu-id="eb747-175">When control resumes in the caller of this async method instance, the caller will receive this async object as the result of its invocation.</span></span>
5. <span data-ttu-id="eb747-176">次に、非同期のメソッド本体の最初のステートメントでインスタンスの制御ポイントを設定し、そこからメソッド本体の実行を直ちに開始します (セクション[ブロックとラベル](statements.md#blocks-and-labels))。</span><span class="sxs-lookup"><span data-stu-id="eb747-176">The instance's control point is then set at the first statement of the async method body, and immediately starts to execute the method body from there (Section [Blocks and Labels](statements.md#blocks-and-labels)).</span></span>

<span data-ttu-id="eb747-177">__再開デリゲートと現在の呼び出し元__</span><span class="sxs-lookup"><span data-stu-id="eb747-177">__Resumption Delegate and Current Caller__</span></span>

<span data-ttu-id="eb747-178">「 [Await 演算子](expressions.md#await-operator)」で詳しく説明したように、`Await` 式の実行では、メソッドインスタンスの制御ポイントを中断して他の場所に移動することができます。</span><span class="sxs-lookup"><span data-stu-id="eb747-178">As detailed in Section [Await Operator](expressions.md#await-operator), execution of an `Await` expression has the ability to suspend the method instance's control point leaving control flow to go elsewhere.</span></span> <span data-ttu-id="eb747-179">制御フローは、*再開デリゲート*の呼び出しによって、後で同じインスタンスの制御ポイントで再開できます。</span><span class="sxs-lookup"><span data-stu-id="eb747-179">Control flow can later resume at the same instance's control point through invocation of a *resumption delegate*.</span></span> <span data-ttu-id="eb747-180">この中断は、非同期メソッドを終了せずに実行され、finally ハンドラーは実行されないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="eb747-180">Note that this suspension is done without exiting the async method, and does not cause finally handlers to execute.</span></span> <span data-ttu-id="eb747-181">メソッドインスタンスは、再開デリゲートと `Task` または `Task(Of T)` 結果 (存在する場合) の両方によって参照されていますが、デリゲートまたは結果へのライブ参照が存在する限り、ガベージコレクションされません。</span><span class="sxs-lookup"><span data-stu-id="eb747-181">The method instance is still referenced by both the resumption delegate and the `Task` or `Task(Of T)` result (if any), and will not be garbage collected so long as there exists a live reference to either delegate or result.</span></span>

<span data-ttu-id="eb747-182">ステートメント `Dim x = Await WorkAsync()`、次の構文の短縮形として考えてみてください。</span><span class="sxs-lookup"><span data-stu-id="eb747-182">It is helpful to imagine the statement `Dim x = Await WorkAsync()` approximately as syntactic shorthand for the following:</span></span>

```vb
Dim temp = WorkAsync().GetAwaiter()
If Not temp.IsCompleted Then
       temp.OnCompleted(resumptionDelegate)
       Return [task]
       CONT:   ' invocation of 'resumptionDelegate' will resume here
End If
Dim x = temp.GetResult()
```

<span data-ttu-id="eb747-183">次の例では、メソッドインスタンスの*現在の呼び出し*元が、元の呼び出し元、または再開デリゲートの呼び出し元として定義されています。</span><span class="sxs-lookup"><span data-stu-id="eb747-183">In the following, the *current caller* of the method instance is defined as either the original caller, or the caller of the resumption delegate, whichever is more recent.</span></span>

<span data-ttu-id="eb747-184">制御フローが非同期メソッド本体を終了すると、その end をマークする `End Sub` または `End Function` に到達するか、明示的な `Return` または `Exit` ステートメント、またはハンドルされない例外を使用して、インスタンスの制御ポイントがメソッドの最後に設定されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-184">When control flow exits the async method body -- through reaching the `End Sub` or `End Function` that mark its end, or through an explicit `Return` or `Exit` statement, or through an unhandled exception -- the instance's control point is set to the end of the method.</span></span> <span data-ttu-id="eb747-185">その後の動作は、非同期メソッドの戻り値の型によって異なります。</span><span class="sxs-lookup"><span data-stu-id="eb747-185">Behavior then depends on the return type of the async method.</span></span>

* <span data-ttu-id="eb747-186">戻り値の型が `Task``Async Function` の場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="eb747-186">In the case of an `Async Function` with return type `Task`:</span></span>
  1. <span data-ttu-id="eb747-187">制御フローがハンドルされない例外によって終了した場合、非同期オブジェクトの状態が `TaskStatus.Faulted` に設定され、その `Exception.InnerException` プロパティが例外に設定されます (ただし、`OperationCanceledException` などの特定の実装定義の例外は `TaskStatus.Canceled`に変更されます)。</span><span class="sxs-lookup"><span data-stu-id="eb747-187">If control flow exits through an unhandled exception, then the async object's status is set to `TaskStatus.Faulted` and its `Exception.InnerException` property is set to the exception (except: certain implementation-defined exceptions such as `OperationCanceledException` change it to `TaskStatus.Canceled`).</span></span> <span data-ttu-id="eb747-188">現在の呼び出し元で制御フローが再開されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-188">Control flow resumes in the current caller.</span></span>
  2. <span data-ttu-id="eb747-189">それ以外の場合は、非同期オブジェクトの状態が `TaskStatus.Completed`に設定されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-189">Otherwise, the async object's status is set to `TaskStatus.Completed`.</span></span> <span data-ttu-id="eb747-190">現在の呼び出し元で制御フローが再開されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-190">Control flow resumes in the current caller.</span></span>

     <span data-ttu-id="eb747-191">(__注:__</span><span class="sxs-lookup"><span data-stu-id="eb747-191">(__Note.__</span></span> <span data-ttu-id="eb747-192">タスクの中心となるのは、タスクが完了すると、タスクが完了すると、そのタスクを待機していたメソッドが、現在、再開デリゲートを実行している (つまり、ブロック解除される) ことです。</span><span class="sxs-lookup"><span data-stu-id="eb747-192">The whole point of Task, and what makes async methods interesting, is that when a task becomes Completed then any methods that were awaiting it will presently have their resumption delegates executed, i.e. they will become unblocked.)</span></span>

* <span data-ttu-id="eb747-193">一部の `T`の戻り値の型が `Task(Of T)` の `Async Function` の場合: 動作は上記と同じですが、例外が発生しない場合でも、async オブジェクトの `Result` プロパティはタスクの戻り変数の最終的な値に設定される点が異なります。</span><span class="sxs-lookup"><span data-stu-id="eb747-193">In the case of an `Async Function` with return type `Task(Of T)` for some `T`: the behavior is as above, except that in non-exception cases the async object's `Result` property is also set to the final value of the task return variable.</span></span>

* <span data-ttu-id="eb747-194">`Async Sub`の場合:</span><span class="sxs-lookup"><span data-stu-id="eb747-194">In the case of an `Async Sub`:</span></span>
  1. <span data-ttu-id="eb747-195">制御フローがハンドルされない例外によって終了した場合、その例外は実装固有の何らかの方法で環境に反映されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-195">If control flow exits through an unhandled exception, then that exception is propagated to the environment in some implementation-specific manner.</span></span> <span data-ttu-id="eb747-196">現在の呼び出し元で制御フローが再開されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-196">Control flow resumes in the current caller.</span></span>
  2. <span data-ttu-id="eb747-197">それ以外の場合、制御フローは、現在の呼び出し元で単に再開されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-197">Otherwise, control flow simply resumes in the current caller.</span></span>

#### <a name="async-sub"></a><span data-ttu-id="eb747-198">Async Sub</span><span class="sxs-lookup"><span data-stu-id="eb747-198">Async Sub</span></span>

<span data-ttu-id="eb747-199">`Async Sub`には、Microsoft 固有の動作がいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="eb747-199">There is some Microsoft-specific behavior of an `Async Sub`.</span></span>

<span data-ttu-id="eb747-200">呼び出しの開始時に `SynchronizationContext.Current` が `Nothing` 場合、非同期サブからの未処理の例外は Threadpool にポストされます。</span><span class="sxs-lookup"><span data-stu-id="eb747-200">If `SynchronizationContext.Current` is `Nothing` at the start of the invocation, then any unhandled exceptions from an Async Sub will be posted to the Threadpool.</span></span>

<span data-ttu-id="eb747-201">呼び出しの開始時に `SynchronizationContext.Current` が `Nothing` されていない場合は、メソッドの開始前にそのコンテキストで `OperationStarted()` が呼び出され、終了後に `OperationCompleted()` ます。</span><span class="sxs-lookup"><span data-stu-id="eb747-201">If `SynchronizationContext.Current` is not `Nothing` at the start of the invocation, then `OperationStarted()` is invoked on that context before the start of the method and `OperationCompleted()` after the end.</span></span> <span data-ttu-id="eb747-202">また、未処理の例外は、同期コンテキストで再スローされるように投稿されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-202">Additionally, any unhandled exceptions will be posted to be rethrown on the synchronization context.</span></span>

<span data-ttu-id="eb747-203">これは、ui アプリケーションで、ui スレッドで呼び出される `Async Sub` の場合、処理に失敗した例外は UI スレッドに reposted されることを意味します。</span><span class="sxs-lookup"><span data-stu-id="eb747-203">This means that in UI applications, for an `Async Sub` that is invoked on the UI thread, any exceptions it fails to handle will be reposted the UI thread.</span></span>

#### <a name="mutable-structures-in-async-and-iterator-methods"></a><span data-ttu-id="eb747-204">Async および iterator メソッドの変更可能な構造体</span><span class="sxs-lookup"><span data-stu-id="eb747-204">Mutable structures in async and iterator methods</span></span>

<span data-ttu-id="eb747-205">通常、変更可能な構造体は不適切な方法と見なされ、async メソッドまたは iterator メソッドではサポートされません。</span><span class="sxs-lookup"><span data-stu-id="eb747-205">Mutable structures in general are considered bad practice, and they are not supported by async or iterator methods.</span></span> <span data-ttu-id="eb747-206">特に、構造体での async メソッドまたは iterator メソッドの呼び出しは、呼び出しの時点でコピーされるその構造の*コピー*を暗黙的に操作します。</span><span class="sxs-lookup"><span data-stu-id="eb747-206">In particular, each invocation of an async or iterator method in a structure will implicitly operate on a *copy* of that structure that is copied at its moment of invocation.</span></span> <span data-ttu-id="eb747-207">このため、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="eb747-207">Thus, for example,</span></span>

```vb
Structure S
       Dim x As Integer
       Async Sub Mutate()
           x = 2
       End Sub
End Structure

Dim s As New S With {.x = 1}
s.Mutate()
Console.WriteLine(s.x)   ' prints "1"
```

### <a name="blocks-and-labels"></a><span data-ttu-id="eb747-208">ブロックとラベル</span><span class="sxs-lookup"><span data-stu-id="eb747-208">Blocks and Labels</span></span>

<span data-ttu-id="eb747-209">実行可能なステートメントのグループは、ステートメントブロックと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="eb747-209">A group of executable statements is called a statement block.</span></span> <span data-ttu-id="eb747-210">ステートメントブロックの実行は、ブロック内の最初のステートメントで開始されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-210">Execution of a statement block begins with the first statement in the block.</span></span> <span data-ttu-id="eb747-211">ステートメントが実行されると、ステートメントが別の場所で実行を転送する場合や、例外が発生した場合を除き、次のステートメントが構文の順序で実行されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-211">Once a statement has been executed, the next statement in lexical order is executed, unless a statement transfers execution elsewhere or an exception occurs.</span></span>

<span data-ttu-id="eb747-212">ステートメントブロック内では、論理行に対するステートメントの分割は、ラベル宣言ステートメントを除き、重要ではありません。</span><span class="sxs-lookup"><span data-stu-id="eb747-212">Within a statement block, the division of statements on logical lines is not significant with the exception of label declaration statements.</span></span> <span data-ttu-id="eb747-213">ラベルは、ステートメントブロック内の特定の位置を識別する識別子であり、`GoTo`などの分岐ステートメントの対象として使用できます。</span><span class="sxs-lookup"><span data-stu-id="eb747-213">A label is an identifier that identifies a particular position within the statement block that can be used as the target of a branch statement such as `GoTo`.</span></span>

```antlr
Block
    : Statements*
    ;

LabelDeclarationStatement
    : LabelName ':'
    ;

LabelName
    : Identifier
    | IntLiteral
    ;

Statements
    : Statement? ( ':' Statement? )*
    ;
```


<span data-ttu-id="eb747-214">ラベル宣言ステートメントは論理行の先頭に記述する必要があり、ラベルは識別子または整数リテラルのいずれかになります。</span><span class="sxs-lookup"><span data-stu-id="eb747-214">Label declaration statements must appear at the beginning of a logical line and labels may be either an identifier or an integer literal.</span></span> <span data-ttu-id="eb747-215">ラベル宣言ステートメントと呼び出しステートメントはどちらも1つの識別子で構成できるため、ローカル行の先頭にある単一の識別子は常にラベル宣言ステートメントと見なされます。</span><span class="sxs-lookup"><span data-stu-id="eb747-215">Because both label declaration statements and invocation statements can consist of a single identifier, a single identifier at the beginning of a local line is always considered a label declaration statement.</span></span> <span data-ttu-id="eb747-216">同じ論理行でステートメントが実行されていない場合でも、ラベル宣言ステートメントの後には常にコロンが必要です。</span><span class="sxs-lookup"><span data-stu-id="eb747-216">Label declaration statements must always be followed by a colon, even if no statements follow on the same logical line.</span></span>

<span data-ttu-id="eb747-217">ラベルには独自の宣言領域があり、他の識別子に干渉することはありません。</span><span class="sxs-lookup"><span data-stu-id="eb747-217">Labels have their own declaration space and do not interfere with other identifiers.</span></span> <span data-ttu-id="eb747-218">次の例は有効で、name 変数をパラメーターとしても、ラベルとしても使用してい `x`。</span><span class="sxs-lookup"><span data-stu-id="eb747-218">The following example is valid and uses the name variable `x` both as a parameter and as a label.</span></span>

```vb
Function F(x As Integer) As Integer
    If x >= 0 Then
        GoTo x
    End If
    x = -x
x: 
    Return x
End Function
```

<span data-ttu-id="eb747-219">ラベルのスコープは、それを含むメソッドの本体です。</span><span class="sxs-lookup"><span data-stu-id="eb747-219">The scope of a label is the body of the method containing it.</span></span>

<span data-ttu-id="eb747-220">読みやすくするために、複数のサブステートメントを含むステートメントの作成は、この仕様では1つの実稼働として扱われますが、それぞれがラベル付きの行でサブステートメントを単独で使用する場合もあります。</span><span class="sxs-lookup"><span data-stu-id="eb747-220">For the sake of readability, statement productions that involve multiple substatements are treated as a single production in this specification, even though the substatements may each be by themselves on a labeled line.</span></span>


### <a name="local-variables-and-parameters"></a><span data-ttu-id="eb747-221">ローカル変数とパラメーター</span><span class="sxs-lookup"><span data-stu-id="eb747-221">Local Variables and Parameters</span></span>

<span data-ttu-id="eb747-222">前のセクションでは、メソッドのインスタンスを作成する方法とタイミング、およびメソッドのローカル変数とパラメーターのコピーについて詳しく説明しました。</span><span class="sxs-lookup"><span data-stu-id="eb747-222">The preceding sections detail how and when method instances are created, and with them the copies of a method's local variables and parameters.</span></span> <span data-ttu-id="eb747-223">さらに、ループの本体が入力されるたびに、「 [Loop ステートメント](statements.md#loop-statements)」で説明されているように、ループ内で宣言された各ローカル変数の新しいコピーが作成されます。メソッドインスタンスには、前のコピーではなくローカル変数のコピーが含まれるようになりました。</span><span class="sxs-lookup"><span data-stu-id="eb747-223">In addition, each time the body of a loop is entered, a new copy is made of each local variable declared inside that loop as described in Section [Loop Statements](statements.md#loop-statements), and the method instance now contains this copy of its local variable rather than the previous copy.</span></span>

<span data-ttu-id="eb747-224">すべてのローカル変数は、その型の既定値に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-224">All locals are initialized to their type's default value.</span></span> <span data-ttu-id="eb747-225">ローカル変数とパラメーターは、常にパブリックにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="eb747-225">Local variables and parameters are always publicly accessible.</span></span> <span data-ttu-id="eb747-226">次の例に示すように、宣言の前にあるテキスト位置でローカル変数を参照すると、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="eb747-226">It is an error to refer to a local variable in a textual position that precedes its declaration, as the following example illustrates:</span></span>

```vb
Class A
    Private i As Integer = 0

    Sub F()
        i = 1
        Dim i As Integer       ' Error, use precedes declaration.
        i = 2
    End Sub

    Sub G()
        Dim a As Integer = 1
        Dim b As Integer = a   ' This is valid.
    End Sub
End Class
```

<span data-ttu-id="eb747-227">上記の `F` メソッドでは、`i` への最初の代入は、外側のスコープで宣言されたフィールドを参照しません。</span><span class="sxs-lookup"><span data-stu-id="eb747-227">In the `F` method above, the first assignment to `i` specifically does not refer to the field declared in the outer scope.</span></span> <span data-ttu-id="eb747-228">代わりに、ローカル変数を参照しています。変数の宣言の前にあるため、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="eb747-228">Rather, it refers to the local variable, and it is in error because it textually precedes the declaration of the variable.</span></span> <span data-ttu-id="eb747-229">`G` メソッドでは、後続の変数宣言は、同じローカル変数宣言内の前の変数宣言で宣言されたローカル変数を参照します。</span><span class="sxs-lookup"><span data-stu-id="eb747-229">In the `G` method, a subsequent variable declaration refers to a local variable declared in an earlier variable declaration within the same local variable declaration.</span></span>

<span data-ttu-id="eb747-230">メソッド内の各ブロックは、ローカル変数の宣言空間を作成します。</span><span class="sxs-lookup"><span data-stu-id="eb747-230">Each block in a method creates a declaration space for local variables.</span></span> <span data-ttu-id="eb747-231">名前は、メソッド本体のローカル変数宣言とメソッドのパラメーターリストを通じて、この宣言領域に導入されます。これにより、最も外側のブロックの宣言領域に名前が導入されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-231">Names are introduced into this declaration space through local variable declarations in the method body and through the parameter list of the method, which introduces names into the outermost block's declaration space.</span></span> <span data-ttu-id="eb747-232">ブロックでは、入れ子によって名前のシャドウを許可しません。名前がブロック内で宣言されている場合、その名前は入れ子になったブロックで再宣言されない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-232">Blocks do not allow shadowing of names through nesting: once a name has been declared in a block, the name may not be redeclared in any nested blocks.</span></span>

<span data-ttu-id="eb747-233">したがって、次の例では、名前 `i` が外側のブロックで宣言されており、内部ブロックで再宣言できないため、`F` および `G` メソッドがエラーになります。</span><span class="sxs-lookup"><span data-stu-id="eb747-233">Thus, in the following example, the `F` and `G` methods are in error because the name `i` is declared in the outer block and cannot be redeclared in the inner block.</span></span> <span data-ttu-id="eb747-234">ただし、`H` メソッドと `I` メソッドは、2つの `i`が入れ子になっていない個別のブロックで宣言されているため有効です。</span><span class="sxs-lookup"><span data-stu-id="eb747-234">However, the `H` and `I` methods are valid because the two `i`'s are declared in separate non-nested blocks.</span></span>

```vb
Class A
    Sub F()
        Dim i As Integer = 0
        If True Then
               Dim i As Integer = 1
        End If
    End Sub

    Sub G()
        If True Then
            Dim i As Integer = 0
        End If
        Dim i As Integer = 1
    End Sub 

    Sub H()
        If True Then
            Dim i As Integer = 0
        End If
        If True Then
            Dim i As Integer = 1
        End If
    End Sub

    Sub I() 
        For i As Integer = 0 To 9
            H()
        Next i

        For i As Integer = 0 To 9
            H()
        Next i
    End Sub 
End Class
```

<span data-ttu-id="eb747-235">メソッドが関数の場合、関数の戻り値を表すメソッドと同じ名前のメソッド本体の宣言空間で、特別なローカル変数が暗黙的に宣言されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-235">When the method is a function, a special local variable is implicitly declared in the method body's declaration space with the same name as the method representing the return value of the function.</span></span> <span data-ttu-id="eb747-236">ローカル変数は、式で使用されるときに特別な名前解決セマンティクスを持ちます。</span><span class="sxs-lookup"><span data-stu-id="eb747-236">The local variable has special name resolution semantics when used in expressions.</span></span> <span data-ttu-id="eb747-237">呼び出し式など、メソッドグループとして分類された式が必要なコンテキストでローカル変数が使用されている場合、その名前はローカル変数ではなく関数に解決されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-237">If the local variable is used in a context that expects an expression classified as a method group, such as an invocation expression, then the name resolves to the function rather than to the local variable.</span></span> <span data-ttu-id="eb747-238">例 :</span><span class="sxs-lookup"><span data-stu-id="eb747-238">For example:</span></span>

```vb
Function F(i As Integer) As Integer
    If i = 0 Then
        F = 1        ' Sets the return value.
    Else
        F = F(i - 1) ' Recursive call.
    End If
End Function
```

<span data-ttu-id="eb747-239">かっこを使用すると、あいまいな状況 (`F(1)`など) が発生する可能性があります。 `F` は、戻り値の型が1次元配列である関数です。あいまいな状況では、名前がローカル変数ではなく関数に解決されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-239">The use of parentheses can cause ambiguous situations (such as `F(1)`, where `F` is a function whose return type is a one-dimensional array); in all ambiguous situations, the name resolves to the function rather than the local variable.</span></span> <span data-ttu-id="eb747-240">例 :</span><span class="sxs-lookup"><span data-stu-id="eb747-240">For example:</span></span>

```vb
Function F(i As Integer) As Integer()
    If i = 0 Then
        F = new Integer(2) { 1, 2, 3 }
    Else
        F = F(i - 1) ' Recursive call, not an index.
    End If
End Function
```

<span data-ttu-id="eb747-241">制御フローがメソッド本体から出たときに、ローカル変数の値が呼び出し式に渡されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-241">When control flow leaves the method body, the value of the local variable is passed back to the invocation expression.</span></span> <span data-ttu-id="eb747-242">メソッドがサブルーチンの場合、そのような暗黙的なローカル変数は存在せず、制御は単に呼び出し式に戻ります。</span><span class="sxs-lookup"><span data-stu-id="eb747-242">If the method is a subroutine, there is no such implicit local variable, and control simply returns to the invocation expression.</span></span>

## <a name="local-declaration-statements"></a><span data-ttu-id="eb747-243">ローカル宣言ステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-243">Local Declaration Statements</span></span>

<span data-ttu-id="eb747-244">ローカル宣言ステートメントは、新しいローカル変数、ローカル定数、または静的変数を宣言します。</span><span class="sxs-lookup"><span data-stu-id="eb747-244">A local declaration statement declares a new local variable, local constant, or static variable.</span></span> <span data-ttu-id="eb747-245">*ローカル変数*と*ローカル定数*は、メソッドをスコープとするインスタンス変数および定数に相当し、同じ方法で宣言されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-245">*Local variables* and *local constants* are equivalent to instance variables and constants scoped to the method and are declared in the same way.</span></span> <span data-ttu-id="eb747-246">*静的変数*は `Shared` 変数に似ており、`Static` 修飾子を使用して宣言されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-246">*Static variables* are similar to `Shared` variables and are declared using the `Static` modifier.</span></span>

```antlr
LocalDeclarationStatement
    : LocalModifier VariableDeclarators StatementTerminator
    ;

LocalModifier
    : 'Static' | 'Dim' | 'Const'
    ;
```

<span data-ttu-id="eb747-247">静的変数は、メソッドの呼び出し間で値を保持するローカル変数です。</span><span class="sxs-lookup"><span data-stu-id="eb747-247">Static variables are locals that retain their value across invocations of the method.</span></span> <span data-ttu-id="eb747-248">非共有メソッド内で宣言された静的変数は、インスタンスごとにあります。メソッドを含む型の各インスタンスには、静的変数の独自のコピーがあります。</span><span class="sxs-lookup"><span data-stu-id="eb747-248">Static variables declared within non-shared methods are per instance: each instance of the type that contains the method has its own copy of the static variable.</span></span> <span data-ttu-id="eb747-249">`Shared` メソッド内で宣言された静的変数は、型ごとになります。すべてのインスタンスに対して静的変数のコピーは1つだけです。</span><span class="sxs-lookup"><span data-stu-id="eb747-249">Static variables declared within `Shared` methods are per type; there is only one copy of the static variable for all instances.</span></span> <span data-ttu-id="eb747-250">ローカル変数は、メソッドへの各エントリの型の既定値に初期化されますが、静的変数は、型または型のインスタンスが初期化されるときに、その型の既定値にのみ初期化されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-250">While local variables are initialized to their type's default value upon each entry into the method, static variables are only initialized to their type's default value when the type or type instance is initialized.</span></span> <span data-ttu-id="eb747-251">静的変数は、構造体またはジェネリックメソッドで宣言することはできません。</span><span class="sxs-lookup"><span data-stu-id="eb747-251">Static variables may not be declared in structures or generic methods.</span></span>

<span data-ttu-id="eb747-252">ローカル変数、ローカル定数、および静的変数は、常にパブリックアクセシビリティを持ち、アクセシビリティ修飾子を指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="eb747-252">Local variables, local constants, and static variables always have public accessibility and may not specify accessibility modifiers.</span></span> <span data-ttu-id="eb747-253">ローカル宣言ステートメントに型が指定されていない場合は、次の手順によってローカル宣言の型が決まります。</span><span class="sxs-lookup"><span data-stu-id="eb747-253">If no type is specified on a local declaration statement, then the following steps determine the type of the local declaration:</span></span>

1. <span data-ttu-id="eb747-254">宣言に型文字が含まれている場合、型文字の型はローカル宣言の型になります。</span><span class="sxs-lookup"><span data-stu-id="eb747-254">If the declaration has a type character, the type of the type character is the type of the local declaration.</span></span>

2. <span data-ttu-id="eb747-255">ローカル宣言がローカル定数である場合、またはローカル宣言が初期化子を持つローカル変数であり、ローカル変数型の推論が使用されている場合、ローカル宣言の型は初期化子の型から推論されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-255">If the local declaration is a local constant, or if the local declaration is a local variable with an initializer and local variable type inference is being used, the type of the local declaration is inferred from the type of the initializer.</span></span> <span data-ttu-id="eb747-256">初期化子がローカル宣言を参照している場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="eb747-256">If the initializer refers to the local declaration, a compile-time error occurs.</span></span> <span data-ttu-id="eb747-257">(ローカル定数は初期化子を持つ必要があります)。</span><span class="sxs-lookup"><span data-stu-id="eb747-257">(Local constants are required to have initializers.)</span></span>

3. <span data-ttu-id="eb747-258">厳密なセマンティクスが使用されていない場合、ローカル宣言ステートメントの型は暗黙的に `Object`ます。</span><span class="sxs-lookup"><span data-stu-id="eb747-258">If strict semantics are not being used, the type of the local declaration statement is implicitly `Object`.</span></span>

4. <span data-ttu-id="eb747-259">それ以外の場合は、コンパイル時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="eb747-259">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="eb747-260">配列サイズまたは配列型修飾子を持つローカル宣言ステートメントに型が指定されていない場合、ローカル宣言の型は指定された順位の配列になり、前の手順を使用して配列の要素の型が決定されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-260">If no type is specified on a local declaration statement that has an array size or array type modifier, then the type of the local declaration is an array with the specified rank and the previous steps are used to determine the element type of the array.</span></span> <span data-ttu-id="eb747-261">ローカル変数型の推論を使用する場合、初期化子の型は、ローカル宣言ステートメントと同じ配列図形 (つまり配列型修飾子) を持つ配列型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-261">If local variable type inference is used, the type of the initializer must be an array type with the same array shape (i.e. array type modifiers) as the local declaration statement.</span></span> <span data-ttu-id="eb747-262">推定される要素型は配列型である可能性があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="eb747-262">Note that it is possible that the inferred element type may still be an array type.</span></span> <span data-ttu-id="eb747-263">例 :</span><span class="sxs-lookup"><span data-stu-id="eb747-263">For example:</span></span>

```vb
Option Infer On

Module Test
    Sub Main()
        ' Error: initializer is not an array type
        Dim x() = 1

        ' Type is Integer()
        Dim y() = New Integer() {}

        ' Type is Integer()()
        Dim z() = New Integer()() {}

        ' Type is Integer()()()

        Dim a()() = New Integer()()() {}

        ' Error: initializer does not have same array shape
        Dim b()() = New Integer(,)() {}
    End Sub
End Module
```

<span data-ttu-id="eb747-264">Null 許容型修飾子を持つローカル宣言ステートメントに型が指定されていない場合、ローカル宣言の型は、推論された型の null 許容バージョンか、既に null 許容値型である場合は推論された型自体になります。</span><span class="sxs-lookup"><span data-stu-id="eb747-264">If no type is specified on a local declaration statement that has a nullable type modifier, then the type of the local declaration is the nullable version of the inferred type or the inferred type itself if it a nullable value type already.</span></span>  <span data-ttu-id="eb747-265">推論された型が、null 値を許容できる値の型ではない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="eb747-265">If the inferred type is not a value type that can be made nullable, a compile-time error occurs.</span></span> <span data-ttu-id="eb747-266">Null 許容型修飾子と配列サイズまたは配列型修飾子の両方が型のないローカル宣言ステートメントに配置されている場合、null 許容型修飾子は配列の要素型に適用されると見なされ、前の手順を使用して要素が決定されます。nt 型。</span><span class="sxs-lookup"><span data-stu-id="eb747-266">If both a nullable type modifier and an array size or array type modifier are placed on a local declaration statement with no type, then the nullable type modifier is considered to apply to the element type of the array and the previous steps are used to determine the element type.</span></span>

<span data-ttu-id="eb747-267">ローカル宣言ステートメントの変数初期化子は、宣言のテキストの場所に配置される代入ステートメントと同じです。</span><span class="sxs-lookup"><span data-stu-id="eb747-267">Variable initializers on local declaration statements are equivalent to assignment statements placed at the textual location of the declaration.</span></span> <span data-ttu-id="eb747-268">したがって、実行がローカル宣言ステートメントで分岐した場合、変数初期化子は実行されません。</span><span class="sxs-lookup"><span data-stu-id="eb747-268">Thus, if execution branches over the local declaration statement, the variable initializer is not executed.</span></span> <span data-ttu-id="eb747-269">ローカル宣言ステートメントが複数回実行された場合、変数初期化子は同じ回数だけ実行されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-269">If the local declaration statement is executed more than once, the variable initializer is executed an equal number of times.</span></span> <span data-ttu-id="eb747-270">静的変数は初期化子を最初に実行するだけです。</span><span class="sxs-lookup"><span data-stu-id="eb747-270">Static variables only execute their initializer the first time.</span></span> <span data-ttu-id="eb747-271">静的変数の初期化中に例外が発生した場合、静的変数は静的変数の型の既定値を使用して初期化されたと見なされます。</span><span class="sxs-lookup"><span data-stu-id="eb747-271">If an exception occurs while initializing a static variable, the static variable is considered initialized with the default value of the static variable's type.</span></span>

<span data-ttu-id="eb747-272">次の例は、初期化子の使用方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="eb747-272">The following example shows the use of initializers:</span></span>

```vb
Module Test
    Sub F()
        Static x As Integer = 5

        Console.WriteLine("Static variable x = " & x)
        x += 1
    End Sub

    Sub Main()
        Dim i As Integer

        For i = 1 to 3
            F()
        Next i

        i = 3
label:
        Dim y As Integer = 8

        If i > 0 Then
            Console.WriteLine("Local variable y = " & y)
            y -= 1
            i -= 1
            GoTo label
        End If
    End Sub
End Module
```

<span data-ttu-id="eb747-273">このプログラムの出力:</span><span class="sxs-lookup"><span data-stu-id="eb747-273">This program prints:</span></span>

```console
Static variable x = 5
Static variable x = 6
Static variable x = 7
Local variable y = 8
Local variable y = 8
Local variable y = 8
```

<span data-ttu-id="eb747-274">静的ローカルの初期化子はスレッドセーフであり、初期化中に例外に対して保護されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-274">Initializers on static locals are thread-safe and protected against exceptions during initialization.</span></span> <span data-ttu-id="eb747-275">静的ローカル初期化子の実行中に例外が発生した場合、静的ローカルは既定値を持ち、初期化されません。</span><span class="sxs-lookup"><span data-stu-id="eb747-275">If an exception occurs during a static local initializer, the static local will have its default value and not be initialized.</span></span> <span data-ttu-id="eb747-276">静的ローカル初期化子</span><span class="sxs-lookup"><span data-stu-id="eb747-276">A static local initializer</span></span>

```vb
Module Test
    Sub F()
        Static x As Integer = 5
    End Sub
End Module
```

<span data-ttu-id="eb747-277">上記の式は、次の式と同じです。</span><span class="sxs-lookup"><span data-stu-id="eb747-277">is equivalent to</span></span>

```vb
Imports System.Threading
Imports Microsoft.VisualBasic.CompilerServices

Module Test
    Class InitFlag
        Public State As Short
    End Class

    Private xInitFlag As InitFlag = New InitFlag()

    Sub F()
        Dim x As Integer

        If xInitFlag.State <> 1 Then
            Monitor.Enter(xInitFlag)
            Try
                If xInitFlag.State = 0 Then
                    xInitFlag.State = 2
                    x = 5
                Else If xInitFlag.State = 2 Then
                    Throw New IncompleteInitialization()
                End If
            Finally
                xInitFlag.State = 1
                Monitor.Exit(xInitFlag)
            End Try
        End If
    End Sub
End Module
```

<span data-ttu-id="eb747-278">ローカル変数、ローカル定数、および静的変数は、宣言されているステートメントブロックにスコープが設定されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-278">Local variables, local constants, and static variables are scoped to the statement block in which they are declared.</span></span> <span data-ttu-id="eb747-279">静的変数は、メソッド全体で1回しか使用できないという意味で特別です。</span><span class="sxs-lookup"><span data-stu-id="eb747-279">Static variables are special in that their names may only be used once throughout the entire method.</span></span> <span data-ttu-id="eb747-280">たとえば、異なるブロックにある場合でも、同じ名前を持つ2つの静的変数宣言を指定することは無効です。</span><span class="sxs-lookup"><span data-stu-id="eb747-280">For example, it is not valid to specify two static variable declarations with the same name even if they are in different blocks.</span></span>


### <a name="implicit-local-declarations"></a><span data-ttu-id="eb747-281">暗黙的なローカル宣言</span><span class="sxs-lookup"><span data-stu-id="eb747-281">Implicit Local Declarations</span></span>

<span data-ttu-id="eb747-282">ローカル宣言ステートメントに加えて、ローカル変数は、使用して暗黙的に宣言することもできます。</span><span class="sxs-lookup"><span data-stu-id="eb747-282">In addition to local declaration statements, local variables can also be declared implicitly through use.</span></span> <span data-ttu-id="eb747-283">別の名前に解決されない名前を使用する単純な名前式は、その名前でローカル変数を宣言します。</span><span class="sxs-lookup"><span data-stu-id="eb747-283">A simple name expression that uses a name that does not resolve to something else declares a local variable by that name.</span></span> <span data-ttu-id="eb747-284">例 :</span><span class="sxs-lookup"><span data-stu-id="eb747-284">For example:</span></span>

```vb
Option Explicit Off

Module Test
    Sub Main()
        x = 10
        y = 20
        Console.WriteLine(x + y)
    End Sub
End Module
```

<span data-ttu-id="eb747-285">暗黙的なローカル宣言は、変数として分類された式を受け入れることができる式のコンテキストでのみ発生します。</span><span class="sxs-lookup"><span data-stu-id="eb747-285">Implicit local declaration only occurs in expression contexts that can accept an expression classified as a variable.</span></span> <span data-ttu-id="eb747-286">この規則の例外として、関数呼び出し式、インデックス式、またはメンバーアクセス式のターゲットである場合、ローカル変数を暗黙的に宣言することはできません。</span><span class="sxs-lookup"><span data-stu-id="eb747-286">The exception to this rule is that a local variable may not be implicitly declared when it is the target of a function invocation expression, indexing expression, or a member access expression.</span></span>

<span data-ttu-id="eb747-287">暗黙的なローカルは、それを含んでいるメソッドの先頭で宣言されているかのように扱われます。</span><span class="sxs-lookup"><span data-stu-id="eb747-287">Implicit locals are treated as if they are declared at the beginning of the containing method.</span></span> <span data-ttu-id="eb747-288">したがって、ラムダ式の内部で宣言されていても、常にメソッドの本体全体にスコープが設定されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-288">Thus, they are always scoped to the entire method body, even if declared inside of a lambda expression.</span></span> <span data-ttu-id="eb747-289">コード例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="eb747-289">For example, the following code:</span></span>

```vb
Option Explicit Off 

Module Test
    Sub Main()
        Dim x = Sub()
                    a = 10
                End Sub
        Dim y = Sub()
                    Console.WriteLine(a)
                End Sub

        x()
        y()
    End Sub
End Module
```

<span data-ttu-id="eb747-290">は `10`値を出力します。</span><span class="sxs-lookup"><span data-stu-id="eb747-290">will print the value `10`.</span></span> <span data-ttu-id="eb747-291">変数名に型文字がアタッチされていない場合、暗黙的なローカル変数は `Object` として型指定されます。それ以外の場合は、変数の型が型文字の型になります。</span><span class="sxs-lookup"><span data-stu-id="eb747-291">Implicit locals are typed as `Object` if no type character was attached to the variable name; otherwise the type of the variable is the type of the type character.</span></span> <span data-ttu-id="eb747-292">ローカル変数の型の推論は、暗黙的なローカルには使用されません。</span><span class="sxs-lookup"><span data-stu-id="eb747-292">Local variable type inference is not used for implicit locals.</span></span>

<span data-ttu-id="eb747-293">明示的なローカル宣言がコンパイル環境または `Option Explicit`によって指定されている場合は、すべてのローカル変数を明示的に宣言する必要があり、暗黙的な変数宣言は許可されません。</span><span class="sxs-lookup"><span data-stu-id="eb747-293">If explicit local declaration is specified by the compilation environment or by `Option Explicit`, all local variables must be explicitly declared and implicit variable declaration is disallowed.</span></span>

## <a name="with-statement"></a><span data-ttu-id="eb747-294">With ステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-294">With Statement</span></span>

<span data-ttu-id="eb747-295">`With` ステートメントでは、式を複数回指定することなく、式のメンバーへの複数の参照を使用できます。</span><span class="sxs-lookup"><span data-stu-id="eb747-295">A `With` statement allows multiple references to an expression's members without specifying the expression multiple times.</span></span>

```antlr
WithStatement
    : 'With' Expression StatementTerminator
      Block?
      'End' 'With' StatementTerminator
    ;
```

<span data-ttu-id="eb747-296">式は、値として分類される必要があり、ブロックに入ったときに1回だけ評価されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-296">The expression must be classified as a value and is evaluated once, upon entry into the block.</span></span> <span data-ttu-id="eb747-297">`With` ステートメントブロック内では、ピリオドまたは感嘆符で始まるメンバーアクセス式またはディクショナリアクセス式は、`With` 式の前にあるかのように評価されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-297">Within the `With` statement block, a member access expression or dictionary access expression starting with a period or an exclamation point is evaluated as if the `With` expression preceded it.</span></span> <span data-ttu-id="eb747-298">例 :</span><span class="sxs-lookup"><span data-stu-id="eb747-298">For example:</span></span>

```vb
Structure Test
    Public x As Integer

    Function F() As Integer
        Return 10
    End Sub
End Structure

Module TestModule
    Sub Main()
        Dim y As Test

        With y
            .x = 10
            Console.WriteLine(.x)
            .x = .F()
        End With
    End Sub
End Module
```

<span data-ttu-id="eb747-299">ブロックの外部から `With` ステートメントブロックに分岐することはできません。</span><span class="sxs-lookup"><span data-stu-id="eb747-299">It is invalid to branch into a `With` statement block from outside of the block.</span></span>


## <a name="synclock-statement"></a><span data-ttu-id="eb747-300">SyncLock ステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-300">SyncLock Statement</span></span>

<span data-ttu-id="eb747-301">`SyncLock` ステートメントを使用すると、ステートメントを式で同期させることができます。これにより、複数の実行スレッドが同時に同じステートメントを実行することがなくなります。</span><span class="sxs-lookup"><span data-stu-id="eb747-301">A `SyncLock` statement allows statements to be synchronized on an expression, which ensures that multiple threads of execution do not execute the same statements at the same time.</span></span>

```antlr
SyncLockStatement
    : 'SyncLock' Expression StatementTerminator
      Block?
      'End' 'SyncLock' StatementTerminator
    ;
```

<span data-ttu-id="eb747-302">式は、値として分類される必要があり、ブロックに入ると、1回だけ評価されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-302">The expression must be classified as a value and is evaluated once, upon entry to the block.</span></span> <span data-ttu-id="eb747-303">`SyncLock` ブロックを入力すると、指定された式に対して `Shared` メソッド `System.Threading.Monitor.Enter` が呼び出されます。この式は、実行のスレッドが、式によって返されたオブジェクトの排他ロックを解除するまでブロックします。</span><span class="sxs-lookup"><span data-stu-id="eb747-303">When entering the `SyncLock` block, the `Shared` method `System.Threading.Monitor.Enter` is called on the specified expression, which blocks until the thread of execution has an exclusive lock on the object returned by the expression.</span></span> <span data-ttu-id="eb747-304">`SyncLock` ステートメント内の式の型は、参照型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-304">The type of the expression in a `SyncLock` statement must be a reference type.</span></span> <span data-ttu-id="eb747-305">例 :</span><span class="sxs-lookup"><span data-stu-id="eb747-305">For example:</span></span>

```vb
Class Test
    Private count As Integer = 0

    Public Function Add() As Integer
        SyncLock Me
            count += 1
            Add = count
        End SyncLock
    End Function

    Public Function Subtract() As Integer
        SyncLock Me
            count -= 1
            Subtract = count
        End SyncLock
    End Function
End Class
```

<span data-ttu-id="eb747-306">上記の例では `Test` クラスの特定のインスタンスでを同期して、特定のインスタンスに対して一度にカウント変数に対して実行するスレッドが1つでも追加または削除できないようにしています。</span><span class="sxs-lookup"><span data-stu-id="eb747-306">The example above synchronizes on the specific instance of the class `Test` to ensure that no more than one thread of execution can add or subtract from the count variable at a time for a particular instance.</span></span>

<span data-ttu-id="eb747-307">`SyncLock` ブロックは、`Finally` ブロックが式で `System.Threading.Monitor.Exit` `Shared` メソッドを呼び出す `Try` ステートメントによって暗黙的に含まれます。</span><span class="sxs-lookup"><span data-stu-id="eb747-307">The `SyncLock` block is implicitly contained by a `Try` statement whose `Finally` block calls the `Shared` method `System.Threading.Monitor.Exit` on the expression.</span></span> <span data-ttu-id="eb747-308">これにより、例外がスローされた場合でも、ロックが解放されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-308">This ensures the lock is freed even when an exception is thrown.</span></span> <span data-ttu-id="eb747-309">その結果、ブロックの外部から `SyncLock` ブロックに分岐することは無効になり、`SyncLock` ブロックは `Resume` と `Resume Next`のために1つのステートメントとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="eb747-309">As a result, it is invalid to branch into a `SyncLock` block from outside of the block, and a `SyncLock` block is treated as a single statement for the purposes of `Resume` and `Resume Next`.</span></span> <span data-ttu-id="eb747-310">上の例は、次のコードに相当します。</span><span class="sxs-lookup"><span data-stu-id="eb747-310">The above example is equivalent to the following code:</span></span>

```vb
Class Test
    Private count As Integer = 0

    Public Function Add() As Integer
        Try
            System.Threading.Monitor.Enter(Me)

            count += 1
            Add = count
        Finally
            System.Threading.Monitor.Exit(Me)
        End Try
    End Function

    Public Function Subtract() As Integer
        Try
            System.Threading.Monitor.Enter(Me)

            count -= 1
            Subtract = count
        Finally
            System.Threading.Monitor.Exit(Me)
        End Try
    End Function
End Class
```


## <a name="event-statements"></a><span data-ttu-id="eb747-311">イベントステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-311">Event Statements</span></span>

<span data-ttu-id="eb747-312">`RaiseEvent`、`AddHandler`、および `RemoveHandler` ステートメントは、イベントを発生させ、イベントを動的に処理します。</span><span class="sxs-lookup"><span data-stu-id="eb747-312">The `RaiseEvent`, `AddHandler`, and `RemoveHandler` statements raise events and handle events dynamically.</span></span>

```antlr
EventStatement
    : RaiseEventStatement
    | AddHandlerStatement
    | RemoveHandlerStatement
    ;
```

### <a name="raiseevent-statement"></a><span data-ttu-id="eb747-313">RaiseEvent ステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-313">RaiseEvent Statement</span></span>

<span data-ttu-id="eb747-314">`RaiseEvent` ステートメントは、特定のイベントが発生したことをイベントハンドラーに通知します。</span><span class="sxs-lookup"><span data-stu-id="eb747-314">A `RaiseEvent` statement notifies event handlers that a particular event has occurred.</span></span>

```antlr
RaiseEventStatement
    : 'RaiseEvent' IdentifierOrKeyword
      ( OpenParenthesis ArgumentList? CloseParenthesis )? StatementTerminator
    ;
```

<span data-ttu-id="eb747-315">`RaiseEvent` ステートメントの単純な名前式は、`Me`でのメンバー参照として解釈されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-315">The simple name expression in a `RaiseEvent` statement is interpreted as a member lookup on `Me`.</span></span> <span data-ttu-id="eb747-316">したがって、`RaiseEvent x` は `RaiseEvent Me.x`されているかのように解釈されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-316">Thus, `RaiseEvent x` is interpreted as if it were `RaiseEvent Me.x`.</span></span> <span data-ttu-id="eb747-317">式の結果は、クラス自体で定義されているイベントのイベントアクセスとして分類される必要があります。基本データ型で定義されたイベントは、`RaiseEvent` ステートメントでは使用できません。</span><span class="sxs-lookup"><span data-stu-id="eb747-317">The result of the expression must be classified as an event access for an event defined in the class itself; events defined on base types cannot be used in a `RaiseEvent` statement.</span></span>

<span data-ttu-id="eb747-318">`RaiseEvent` ステートメントは、指定されたパラメーター (存在する場合) を使用して、イベントのデリゲートの `Invoke` メソッドの呼び出しとして処理されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-318">The `RaiseEvent` statement is processed as a call to the `Invoke` method of the event's delegate, using the supplied parameters, if any.</span></span> <span data-ttu-id="eb747-319">デリゲートの値が `Nothing`場合、例外はスローされません。</span><span class="sxs-lookup"><span data-stu-id="eb747-319">If the delegate's value is `Nothing`, no exception is thrown.</span></span> <span data-ttu-id="eb747-320">引数がない場合は、かっこを省略できます。</span><span class="sxs-lookup"><span data-stu-id="eb747-320">If there are no arguments, the parentheses may be omitted.</span></span> <span data-ttu-id="eb747-321">例 :</span><span class="sxs-lookup"><span data-stu-id="eb747-321">For example:</span></span>

```vb
Class Raiser
    Public Event E1(Count As Integer)

    Public Sub Raise()
        Static RaiseCount As Integer = 0

        RaiseCount += 1
        RaiseEvent E1(RaiseCount)
    End Sub
End Class

Module Test
    Private WithEvents x As Raiser

    Private Sub E1Handler(Count As Integer) Handles x.E1
        Console.WriteLine("Raise #" & Count)
    End Sub

    Public Sub Main()
        x = New Raiser
        x.Raise()        ' Prints "Raise #1".
        x.Raise()        ' Prints "Raise #2".
        x.Raise()        ' Prints "Raise #3".
    End Sub
End Module
```

<span data-ttu-id="eb747-322">上記の `Raiser` クラスは、次の場合と同じです。</span><span class="sxs-lookup"><span data-stu-id="eb747-322">The class `Raiser` above is equivalent to:</span></span>

```vb
Class Raiser
    Public Event E1(Count As Integer)

    Public Sub Raise()
        Static RaiseCount As Integer = 0
        Dim TemporaryDelegate As E1EventHandler

        RaiseCount += 1

        ' Use a temporary to avoid a race condition.
        TemporaryDelegate = E1Event
        If Not TemporaryDelegate Is Nothing Then
            TemporaryDelegate.Invoke(RaiseCount)
        End If
    End Sub
End Class
```


### <a name="addhandler-and-removehandler-statements"></a><span data-ttu-id="eb747-323">AddHandler および RemoveHandler ステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-323">AddHandler and RemoveHandler Statements</span></span>

<span data-ttu-id="eb747-324">ほとんどのイベントハンドラーは `WithEvents` 変数によって自動的にフックされますが、実行時にイベントハンドラーを動的に追加したり削除したりすることが必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-324">Although most event handlers are automatically hooked up through `WithEvents` variables, it may be necessary to dynamically add and remove event handlers at run time.</span></span> <span data-ttu-id="eb747-325">これは、`AddHandler` ステートメントと `RemoveHandler` ステートメントによって行われます。</span><span class="sxs-lookup"><span data-stu-id="eb747-325">`AddHandler` and `RemoveHandler` statements do this.</span></span>

```antlr
AddHandlerStatement
    : 'AddHandler' Expression Comma Expression StatementTerminator
    ;

RemoveHandlerStatement
    : 'RemoveHandler' Expression Comma Expression StatementTerminator
    ;
```

<span data-ttu-id="eb747-326">各ステートメントは2つの引数を取ります。1つ目の引数は、イベントアクセスとして分類される式である必要があり、2番目の引数は値として分類される式である必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-326">Each statement takes two arguments: the first argument must be an expression that is classified as an event access and the second argument must be an expression that is classified as a value.</span></span> <span data-ttu-id="eb747-327">2番目の引数の型は、イベントアクセスに関連付けられているデリゲート型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-327">The second argument's type must be the delegate type associated with the event access.</span></span> <span data-ttu-id="eb747-328">例 :</span><span class="sxs-lookup"><span data-stu-id="eb747-328">For example:</span></span>

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

<span data-ttu-id="eb747-329">イベントが発生した場合 `E,` ステートメントは、インスタンスの関連する `add_E` または `remove_E` メソッドを呼び出して、デリゲートをイベントのハンドラーとして追加または削除します。</span><span class="sxs-lookup"><span data-stu-id="eb747-329">Given an event `E,` the statement calls the relevant `add_E` or `remove_E` method on the instance to add or remove the delegate as a handler for the event.</span></span> <span data-ttu-id="eb747-330">したがって、上記のコードは次と同じです。</span><span class="sxs-lookup"><span data-stu-id="eb747-330">Thus, the above code is equivalent to:</span></span>

```vb
Public Class Form1
    Public Sub New()
        Button1.add_Click(AddressOf Button1_Click)
    End Sub 

    Private Button1 As Button = New Button()

    Sub Button1_Click(sender As Object, e As EventArgs)
        Console.WriteLine("Button1 was clicked!")
    End Sub

    Public Sub Disconnect()
        Button1.remove_Click(AddressOf Button1_Click)
    End Sub
End Class
```


## <a name="assignment-statements"></a><span data-ttu-id="eb747-331">代入ステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-331">Assignment Statements</span></span>

<span data-ttu-id="eb747-332">代入ステートメントによって、式の値が変数に代入されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-332">An assignment statement assigns the value of an expression to a variable.</span></span> <span data-ttu-id="eb747-333">割り当てにはいくつかの種類があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-333">There are several types of assignment.</span></span>

```antlr
AssignmentStatement
    : RegularAssignmentStatement
    | CompoundAssignmentStatement
    | MidAssignmentStatement
    ;
```

### <a name="regular-assignment-statements"></a><span data-ttu-id="eb747-334">通常の代入ステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-334">Regular Assignment Statements</span></span>

<span data-ttu-id="eb747-335">単純な代入ステートメントでは、式の結果が変数に格納されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-335">A simple assignment statement stores the result of an expression in a variable.</span></span>

```antlr
RegularAssignmentStatement
    : Expression Equals Expression StatementTerminator
    ;
```

<span data-ttu-id="eb747-336">代入演算子の左辺の式は、変数またはプロパティアクセスとして分類する必要があります。また、代入演算子の右辺の式は、値として分類する必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-336">The expression on the left side of the assignment operator must be classified as a variable or a property access, while the expression on the right side of the assignment operator must be classified as a value.</span></span> <span data-ttu-id="eb747-337">式の型は、変数またはプロパティアクセスの型に暗黙的に変換できなければなりません。</span><span class="sxs-lookup"><span data-stu-id="eb747-337">The type of the expression must be implicitly convertible to the type of the variable or property access.</span></span>

<span data-ttu-id="eb747-338">に割り当てられている変数が参照型の配列要素である場合は、式が配列要素型と互換性があることを確認するためにランタイムチェックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-338">If the variable being assigned into is an array element of a reference type, a run-time check will be performed to ensure that the expression is compatible with the array-element type.</span></span> <span data-ttu-id="eb747-339">次の例では、最後の割り当てによって `System.ArrayTypeMismatchException` がスローされます。これは、`ArrayList` のインスタンスを `String` 配列の要素に格納できないためです。</span><span class="sxs-lookup"><span data-stu-id="eb747-339">In the following example, the last assignment causes a `System.ArrayTypeMismatchException` to be thrown, because an instance of `ArrayList` cannot be stored in an element of a `String` array.</span></span>

```vb
Dim sa(10) As String
Dim oa As Object() = sa
oa(0) = Nothing         ' This is allowed.
oa(1) = "Hello"         ' This is allowed.
oa(2) = New ArrayList() ' System.ArrayTypeMismatchException is thrown.
```

<span data-ttu-id="eb747-340">代入演算子の左辺の式が変数として分類されている場合、代入ステートメントは変数に値を格納します。</span><span class="sxs-lookup"><span data-stu-id="eb747-340">If the expression on the left side of the assignment operator is classified as a variable, then the assignment statement stores the value in the variable.</span></span> <span data-ttu-id="eb747-341">式がプロパティアクセスとして分類されている場合は、代入ステートメントによって、プロパティのアクセスが、value パラメーターの値に置き換えられたプロパティの `Set` アクセサーの呼び出しに変換されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-341">If the expression is classified as a property access, then the assignment statement turns the property access into an invocation of the `Set` accessor of the property with the value substituted for the value parameter.</span></span> <span data-ttu-id="eb747-342">例 :</span><span class="sxs-lookup"><span data-stu-id="eb747-342">For example:</span></span>

```vb
Module Test
    Private PValue As Integer

    Public Property P As Integer
        Get
            Return PValue
        End Get

        Set (Value As Integer)
            PValue = Value
        End Set
    End Property

    Sub Main()
        ' The following two lines are equivalent.
        P = 10
        set_P(10)
    End Sub
End Module
```

<span data-ttu-id="eb747-343">変数またはプロパティアクセスの対象が値型として型指定されていても、変数として分類されていない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="eb747-343">If the target of the variable or property access is typed as a value type but not classified as a variable, a compile-time error occurs.</span></span> <span data-ttu-id="eb747-344">例 :</span><span class="sxs-lookup"><span data-stu-id="eb747-344">For example:</span></span>

```vb
Structure S
    Public F As Integer
End Structure

Class C
    Private PValue As S

    Public Property P As S
        Get
            Return PValue
        End Get

        Set (Value As S)
            PValue = Value
        End Set
    End Property
End Class

Module Test
    Sub Main()
        Dim ct As C = New C()
        Dim rt As Object = new C()

        ' Compile-time error: ct.P not classified as variable.
        ct.P.F = 10

        ' Run-time exception.
        rt.P.F = 10
    End Sub
End Module
```

<span data-ttu-id="eb747-345">割り当てのセマンティクスは、割り当てられている変数またはプロパティの型によって異なります。</span><span class="sxs-lookup"><span data-stu-id="eb747-345">Note that the semantics of the assignment depend on the type of the variable or property to which it is being assigned.</span></span> <span data-ttu-id="eb747-346">割り当てられている変数が値型である場合、代入によって式の値が変数にコピーされます。</span><span class="sxs-lookup"><span data-stu-id="eb747-346">If the variable to which it is being assigned is a value type, the assignment copies the value of the expression into the variable.</span></span> <span data-ttu-id="eb747-347">割り当てられている変数が参照型の場合、代入は、値自体ではなく参照を変数にコピーします。</span><span class="sxs-lookup"><span data-stu-id="eb747-347">If the variable to which it is being assigned is a reference type, the assignment copies the reference, not the value itself, into the variable.</span></span> <span data-ttu-id="eb747-348">変数の型が `Object`場合、代入セマンティクスは、値の型が実行時に値型か参照型かによって決まります。</span><span class="sxs-lookup"><span data-stu-id="eb747-348">If the type of the variable is `Object`, the assignment semantics are determined by whether the value's type is a value type or a reference type at run time.</span></span>


<span data-ttu-id="eb747-349">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="eb747-349">__Note.__</span></span> <span data-ttu-id="eb747-350">`Integer` や `Date`などの組み込み型では、型が不変であるため、参照と値の代入のセマンティクスは同じです。</span><span class="sxs-lookup"><span data-stu-id="eb747-350">For intrinsic types such as `Integer` and `Date`, reference and value assignment semantics are the same because the types are immutable.</span></span> <span data-ttu-id="eb747-351">その結果、言語は、最適化として、ボックス化された組み込み型に対する参照割り当てを自由に使用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="eb747-351">As a result, the language is free to use reference assignment on boxed intrinsic types as an optimization.</span></span> <span data-ttu-id="eb747-352">値の観点から見ると、結果は同じになります。</span><span class="sxs-lookup"><span data-stu-id="eb747-352">From a value perspective, the result is the same.</span></span>

<span data-ttu-id="eb747-353">Equals 文字 (`=`) は代入と等価性の両方で使用されるため、単純な代入と、`x = y.ToString()`などの状況での呼び出しステートメントの間にあいまいさがあります。</span><span class="sxs-lookup"><span data-stu-id="eb747-353">Because the equals character (`=`) is used both for assignment and for equality, there is an ambiguity between a simple assignment and an invocation statement in situations such as `x = y.ToString()`.</span></span> <span data-ttu-id="eb747-354">このような場合、代入ステートメントは等値演算子よりも優先されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-354">In all such cases, the assignment statement takes precedence over the equality operator.</span></span> <span data-ttu-id="eb747-355">これは、例の式が `(x = y).ToString()`ではなく `x = (y.ToString())` として解釈されることを意味します。</span><span class="sxs-lookup"><span data-stu-id="eb747-355">This means that the example expression is interpreted as `x = (y.ToString())` rather than `(x = y).ToString()`.</span></span>


### <a name="compound-assignment-statements"></a><span data-ttu-id="eb747-356">複合代入ステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-356">Compound Assignment Statements</span></span>

<span data-ttu-id="eb747-357">*複合代入ステートメント*では、`V op= E` の形式を取ります (`op` は有効な二項演算子です)。</span><span class="sxs-lookup"><span data-stu-id="eb747-357">A *compound assignment statement* takes the form `V op= E` (where `op` is a valid binary operator).</span></span>

```antlr
CompoundAssignmentStatement
    : Expression CompoundBinaryOperator LineTerminator? Expression StatementTerminator
    ;

CompoundBinaryOperator
    : '^' '=' | '*' '=' | '/' '=' | '\\' '=' | '+' '=' | '-' '='
    | '&' '=' | '<' '<' '=' | '>' '>' '='
    ;
```

<span data-ttu-id="eb747-358">代入演算子の左辺の式は、変数またはプロパティアクセスとして分類する必要があります。また、代入演算子の右辺の式は、値として分類する必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-358">The expression on the left side of the assignment operator must be classified as a variable or property access, while the expression on the right side of the assignment operator must be classified as a value.</span></span> <span data-ttu-id="eb747-359">複合代入ステートメントは、複合代入演算子の左辺の変数が1回だけ評価されるという点で、ステートメント `V = V op E` と同じです。</span><span class="sxs-lookup"><span data-stu-id="eb747-359">The compound assignment statement is equivalent to the statement `V = V op E` with the difference that the variable on the left side of the compound assignment operator is only evaluated once.</span></span> <span data-ttu-id="eb747-360">次の例は、この違いを示しています。</span><span class="sxs-lookup"><span data-stu-id="eb747-360">The following example demonstrates this difference:</span></span>

```vb
Module Test
    Function GetIndex() As Integer
        Console.WriteLine("Getting index")
        Return 1
    End Function

    Sub Main()
        Dim a(2) As Integer

        Console.WriteLine("Simple assignment")
        a(GetIndex()) = a(GetIndex()) + 1

        Console.WriteLine("Compound assignment")
        a(GetIndex()) += 1
    End Sub
End Module
```

<span data-ttu-id="eb747-361">式 `a(GetIndex())` は単純な割り当てに対して2回評価されますが、複合代入では1回だけ評価されるので、コードは次のように出力されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-361">The expression `a(GetIndex())` is evaluated twice for simple assignment but only once for compound assignment, so the code prints:</span></span>

```console
Simple assignment
Getting index
Getting index
Compound assignment
Getting index
```


### <a name="mid-assignment-statement"></a><span data-ttu-id="eb747-362">Mid 代入ステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-362">Mid Assignment Statement</span></span>

<span data-ttu-id="eb747-363">`Mid` の代入ステートメントは、文字列を別の文字列に代入します。</span><span class="sxs-lookup"><span data-stu-id="eb747-363">A `Mid` assignment statement assigns a string into another string.</span></span> <span data-ttu-id="eb747-364">代入の左側には、関数 `Microsoft.VisualBasic.Strings.Mid`の呼び出しと同じ構文があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-364">The left side of the assignment has the same syntax as a call to the function `Microsoft.VisualBasic.Strings.Mid`.</span></span>

```antlr
MidAssignmentStatement
    : 'Mid' '$'? OpenParenthesis Expression Comma Expression
      ( Comma Expression )? CloseParenthesis Equals Expression StatementTerminator
    ;
```

<span data-ttu-id="eb747-365">最初の引数は代入の対象であり、型が `String`との間で暗黙的に変換可能である変数またはプロパティアクセスとして分類される必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-365">The first argument is the target of the assignment and must be classified as a variable or a property access whose type is implicitly convertible to and from `String`.</span></span> <span data-ttu-id="eb747-366">2番目のパラメーターは、対象の文字列内で代入を開始する位置に対応する1から始まる開始位置であり、`Integer`に暗黙的に変換できる型の値として分類する必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-366">The second parameter is the 1-based start position that corresponds to where the assignment should begin in the target string and must be classified as a value whose type must be implicitly convertible to `Integer`.</span></span> <span data-ttu-id="eb747-367">省略可能な3番目のパラメーターは、対象の文字列に代入する右側の値からの文字数であり、`Integer`に暗黙的に変換できる型を持つ値として分類する必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-367">The optional third parameter is the number of characters from the right-side value to assign into the target string and must be classified as a value whose type is implicitly convertible to `Integer`.</span></span> <span data-ttu-id="eb747-368">右側はソース文字列であり、`String`に暗黙的に変換できる型を持つ値として分類する必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-368">The right side is the source string and must be classified as a value whose type is implicitly convertible to `String`.</span></span> <span data-ttu-id="eb747-369">右側は、指定されている場合は長さのパラメーターに切り捨てられ、左側の文字列の文字は開始位置から置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="eb747-369">The right side is truncated to the length parameter, if specified, and replaces the characters in the left-side string, starting at the start position.</span></span> <span data-ttu-id="eb747-370">右側の文字列に3番目のパラメーターよりも多くの文字が含まれている場合は、右側の文字列の文字だけがコピーされます。</span><span class="sxs-lookup"><span data-stu-id="eb747-370">If the right side string contained fewer characters than the third parameter, only the characters from the right side string will be copied.</span></span>

<span data-ttu-id="eb747-371">次の例では `ab123fg`が表示されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-371">The following example displays `ab123fg`:</span></span>

```vb
Module Test
    Sub Main()
        Dim s1 As String = "abcdefg"
        Dim s2 As String = "1234567"

        Mid$(s1, 3, 3) = s2
        Console.WriteLine(s1)
    End Sub
End Module
```

<span data-ttu-id="eb747-372">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="eb747-372">__Note.__</span></span> <span data-ttu-id="eb747-373">`Mid` は予約語ではありません。</span><span class="sxs-lookup"><span data-stu-id="eb747-373">`Mid` is not a reserved word.</span></span>


## <a name="invocation-statements"></a><span data-ttu-id="eb747-374">呼び出しステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-374">Invocation Statements</span></span>

<span data-ttu-id="eb747-375">呼び出しステートメントは、省略可能なキーワード `Call`の前にあるメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="eb747-375">An invocation statement invokes a method preceded by the optional keyword `Call`.</span></span> <span data-ttu-id="eb747-376">呼び出しステートメントは関数呼び出し式と同じ方法で処理されますが、次に示すいくつかの違いがあります。</span><span class="sxs-lookup"><span data-stu-id="eb747-376">The invocation statement is processed in the same way as the function invocation expression, with some differences noted below.</span></span> <span data-ttu-id="eb747-377">呼び出し式は、値または void として分類される必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-377">The invocation expression must be classified as a value or void.</span></span> <span data-ttu-id="eb747-378">呼び出し式の評価の結果として得られる値はすべて破棄されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-378">Any value resulting from the evaluation of the invocation expression is discarded.</span></span>

<span data-ttu-id="eb747-379">`Call` キーワードを省略した場合、呼び出し式は、識別子またはキーワード、または `With` ブロック内の `.` で始まる必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-379">If the `Call` keyword is omitted, then the invocation expression must start with an identifier or keyword, or with `.` inside a `With` block.</span></span> <span data-ttu-id="eb747-380">したがって、たとえば、"`Call 1.ToString()`" は有効なステートメントですが、"`1.ToString()`" は有効ではありません。</span><span class="sxs-lookup"><span data-stu-id="eb747-380">Thus, for instance, "`Call 1.ToString()`" is a valid statement but "`1.ToString()`" is not.</span></span> <span data-ttu-id="eb747-381">(式のコンテキストでは、呼び出し式も識別子で始まらない必要があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="eb747-381">(Note that in an expression context, invocation expressions also need not start with an identifier.</span></span> <span data-ttu-id="eb747-382">たとえば、"`Dim x = 1.ToString()`" は有効なステートメントです)。</span><span class="sxs-lookup"><span data-stu-id="eb747-382">For example, "`Dim x = 1.ToString()`" is a valid statement).</span></span>

<span data-ttu-id="eb747-383">呼び出しステートメントと呼び出し式には別の違いがあります。呼び出しステートメントに引数リストが含まれている場合、これは常に呼び出しの引数リストとして取得されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-383">There is another difference between the invocation statements and invocation expressions: if an invocation statement includes an argument list, then this is always taken as the argument list of the invocation.</span></span> <span data-ttu-id="eb747-384">次の例は、違いを示しています。</span><span class="sxs-lookup"><span data-stu-id="eb747-384">The following example illustrates the difference:</span></span>

```vb
Module Test
    Sub Main()
        Call {Function() 15}(0)
        ' error: (0) is taken as argument list, but array is not invokable

        Call ({Function() 15}(0))
        ' valid, since the invocation statement has no argument list

        Dim x = {Function() 15}(0)
        ' valid as an expression, since (0) is taken as an array-indexing

        Call f("a")
        ' error: ("a") is taken as argument list to the invocation of f

        Call f()("a")
        ' valid, since () is the argument list for the invocation of f

        Dim y = f("a")
        ' valid as an expression, since f("a") is interpreted as f()("a")
    End Sub

    Sub f() As Func(Of String,String)
        Return Function(x) x
    End Sub
End Module
```

```antlr
InvocationStatement
    : 'Call'? InvocationExpression StatementTerminator
    ;
```

## <a name="conditional-statements"></a><span data-ttu-id="eb747-385">条件付きステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-385">Conditional Statements</span></span>

<span data-ttu-id="eb747-386">条件付きステートメントを使用すると、実行時に評価された式に基づいてステートメントを条件付きで実行できます。</span><span class="sxs-lookup"><span data-stu-id="eb747-386">Conditional statements allow conditional execution of statements based on expressions evaluated at run time.</span></span>

```antlr
ConditionalStatement
    : IfStatement
    | SelectStatement
    ;
```

### <a name="ifthenelse-statements"></a><span data-ttu-id="eb747-387">もし。。。そうしたら。。。Else ステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-387">If...Then...Else Statements</span></span>

<span data-ttu-id="eb747-388">`If...Then...Else` ステートメントは、基本的な条件付きステートメントです。</span><span class="sxs-lookup"><span data-stu-id="eb747-388">An `If...Then...Else` statement is the basic conditional statement.</span></span>

```antlr
IfStatement
    : BlockIfStatement
    | LineIfThenStatement
    ;

BlockIfStatement
    : 'If' BooleanExpression 'Then'? StatementTerminator
      Block?
      ElseIfStatement*
      ElseStatement?
      'End' 'If' StatementTerminator
    ;

ElseIfStatement
    : ElseIf BooleanExpression 'Then'? StatementTerminator
      Block?
    ;

ElseStatement
    : 'Else' StatementTerminator
      Block?
    ;

LineIfThenStatement
    : 'If' BooleanExpression 'Then' Statements ( 'Else' Statements )? StatementTerminator
    ;
    
ElseIf      
    : 'ElseIf'      
    | 'Else' 'If'   
   ;
```

<span data-ttu-id="eb747-389">`If...Then...Else` ステートメント内の各式は、項[ブール式](expressions.md#boolean-expressions)に従ってブール式である必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-389">Each expression in an `If...Then...Else` statement must be a Boolean expression, as per Section [Boolean Expressions](expressions.md#boolean-expressions).</span></span> <span data-ttu-id="eb747-390">(注: この場合、式はブール型である必要はありません)。</span><span class="sxs-lookup"><span data-stu-id="eb747-390">(Note: this does not require the expression to have Boolean type).</span></span> <span data-ttu-id="eb747-391">`If` ステートメント内の式が true の場合、`If` ブロックで囲まれたステートメントが実行されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-391">If the expression in the `If` statement is true, the statements enclosed by the `If` block are executed.</span></span> <span data-ttu-id="eb747-392">式が false の場合、各 `ElseIf` 式が評価されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-392">If the expression is false, each of the `ElseIf` expressions is evaluated.</span></span> <span data-ttu-id="eb747-393">いずれかの `ElseIf` 式が true と評価されると、対応するブロックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-393">If one of the `ElseIf` expressions evaluates to true, the corresponding block is executed.</span></span> <span data-ttu-id="eb747-394">式が true に評価されず、`Else` ブロックがある場合は、`Else` ブロックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-394">If no expression evaluates to true and there is an `Else` block, the `Else` block is executed.</span></span> <span data-ttu-id="eb747-395">ブロックの実行が完了すると、実行は `If...Then...Else` ステートメントの末尾に渡されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-395">Once a block finishes executing, execution passes to the end of the `If...Then...Else` statement.</span></span>

<span data-ttu-id="eb747-396">`If` ステートメントの行バージョンには、`If` 式が `True` 場合に実行されるステートメントのセットが1つあります。また、式が `False`場合は、省略可能なステートメントのセットが実行されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-396">The line version of the `If` statement has a single set of statements to be executed if the `If` expression is `True` and an optional set of statements to be executed if the expression is `False`.</span></span> <span data-ttu-id="eb747-397">例 :</span><span class="sxs-lookup"><span data-stu-id="eb747-397">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim a As Integer = 10
        Dim b As Integer = 20

        ' Block If statement.
        If a < b Then
            a = b
        Else
            b = a
        End If

        ' Line If statement
        If a < b Then a = b Else b = a
    End Sub
End Module
```

<span data-ttu-id="eb747-398">If ステートメントの行バージョンは、":" よりも密にバインドされており、その `Else` は、構文で許可されている、前に説明した前の `If` にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="eb747-398">The line version of the If statement binds less tightly than ":", and its `Else` binds to the lexically nearest preceding `If` that is allowed by the syntax.</span></span> <span data-ttu-id="eb747-399">たとえば、次の2つのバージョンは同等です。</span><span class="sxs-lookup"><span data-stu-id="eb747-399">For example, the following two versions are equivalent:</span></span>

```vb
If True Then _
If True Then Console.WriteLine("a") Else Console.WriteLine("b") _
Else Console.WriteLine("c") : Console.WriteLine("d")

If True Then
    If True Then
        Console.WriteLine("a")
    Else
        Console.WriteLine("b")
    End If
    Console.WriteLine("c") : Console.WriteLine("d")
End If
```

<span data-ttu-id="eb747-400">ラベル宣言ステートメント以外のすべてのステートメントは、ブロックステートメントを含む行 `If` ステートメントの内部で許可されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-400">All statements other than label declaration statements are allowed inside a line `If` statement, including block statements.</span></span> <span data-ttu-id="eb747-401">ただし、複数行のラムダ式の内部では、StatementTerminators として LineTerminators を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="eb747-401">However, they may not use LineTerminators as StatementTerminators except inside multi-line lambda expressions.</span></span> <span data-ttu-id="eb747-402">例 :</span><span class="sxs-lookup"><span data-stu-id="eb747-402">For example:</span></span>

```vb
' Allowed, since it uses : instead of LineTerminator to separate statements
If b Then With New String("a"(0),5) : Console.WriteLine(.Length) : End With

' Disallowed, since it uses a LineTerminator
If b then With New String("a"(0), 5)
              Console.WriteLine(.Length)
          End With

' Allowed, since it only uses LineTerminator inside a multi-line lambda
If b Then Call Sub()
                   Console.WriteLine("a")
               End Sub.Invoke()
```

### <a name="select-case-statements"></a><span data-ttu-id="eb747-403">Select Case ステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-403">Select Case Statements</span></span>

<span data-ttu-id="eb747-404">`Select Case` ステートメントは、式の値に基づいてステートメントを実行します。</span><span class="sxs-lookup"><span data-stu-id="eb747-404">A `Select Case` statement executes statements based on the value of an expression.</span></span>

```antlr
SelectStatement
    : 'Select' 'Case'? Expression StatementTerminator
      CaseStatement*
      CaseElseStatement?
      'End' 'Select' StatementTerminator
    ;

CaseStatement
    : 'Case' CaseClauses StatementTerminator
      Block?
    ;

CaseClauses
    : CaseClause ( Comma CaseClause )*
    ;

CaseClause
    : ( 'Is' LineTerminator? )? ComparisonOperator LineTerminator? Expression
    | Expression ( 'To' Expression )?
    ;

ComparisonOperator
    : '=' | '<' '>' | '<' | '>' | '>' '=' | '<' '='
    ;

CaseElseStatement
    : 'Case' 'Else' StatementTerminator
      Block?
    ;
```

<span data-ttu-id="eb747-405">式は、値として分類される必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-405">The expression must be classified as a value.</span></span> <span data-ttu-id="eb747-406">`Select Case` ステートメントが実行されると、`Select` 式が最初に評価され、`Case` ステートメントがテキスト宣言の順序で評価されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-406">When a `Select Case` statement is executed, the `Select` expression is evaluated first, and the `Case` statements are then evaluated in order of textual declaration.</span></span> <span data-ttu-id="eb747-407">`True` に評価される最初の `Case` ステートメントには、ブロックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-407">The first `Case` statement that evaluates to `True` has its block executed.</span></span> <span data-ttu-id="eb747-408">`Case` ステートメントが `True` に評価されず、`Case Else` ステートメントがある場合は、そのブロックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-408">If no `Case` statement evaluates to `True` and there is a `Case Else` statement, that block is executed.</span></span> <span data-ttu-id="eb747-409">ブロックの実行が完了すると、`Select` ステートメントの最後に実行が渡されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-409">Once a block has finished executing, execution passes to the end of the `Select` statement.</span></span>

<span data-ttu-id="eb747-410">`Case` ブロックの実行は、次の switch セクションに "フォールスルー" することは許可されていません。</span><span class="sxs-lookup"><span data-stu-id="eb747-410">Execution of a `Case` block is not permitted to "fall through" to the next switch section.</span></span> <span data-ttu-id="eb747-411">これにより、`Case` の終了ステートメントが誤って省略された場合に、他の言語で発生する一般的なバグクラスを回避できます。</span><span class="sxs-lookup"><span data-stu-id="eb747-411">This prevents a common class of bugs that occur in other languages when a `Case` terminating statement is accidentally omitted.</span></span> <span data-ttu-id="eb747-412">この動作を次の例に示します。</span><span class="sxs-lookup"><span data-stu-id="eb747-412">The following example illustrates this behavior:</span></span>

```vb
Module Test
    Sub Main()
        Dim x As Integer = 10

        Select Case x
            Case 5
                Console.WriteLine("x = 5")
            Case 10
                Console.WriteLine("x = 10")
            Case 20 - 10
                Console.WriteLine("x = 20 - 10")
            Case 30
                Console.WriteLine("x = 30")
        End Select
    End Sub
End Module
```

<span data-ttu-id="eb747-413">コードは次のように出力されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-413">The code prints:</span></span>

```console
x = 10
```

<span data-ttu-id="eb747-414">`Case 10` と `Case 20 - 10` は同じ値に対して選択されますが、`Case 10` が実行されます。これは、`Case 20 - 10` の前にあるためです。</span><span class="sxs-lookup"><span data-stu-id="eb747-414">Although `Case 10` and `Case 20 - 10` select for the same value, `Case 10` is executed because it precedes `Case 20 - 10` textually.</span></span> <span data-ttu-id="eb747-415">次の `Case` に到達すると、`Select` ステートメントの後で実行が続行されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-415">When the next `Case` is reached, execution continues after the `Select` statement.</span></span>

<span data-ttu-id="eb747-416">`Case` 句には、2つの形式を使用できます。</span><span class="sxs-lookup"><span data-stu-id="eb747-416">A `Case` clause may take two forms.</span></span> <span data-ttu-id="eb747-417">1つの形式は、省略可能な `Is` キーワード、比較演算子、および式です。</span><span class="sxs-lookup"><span data-stu-id="eb747-417">One form is an optional `Is` keyword, a comparison operator, and an expression.</span></span> <span data-ttu-id="eb747-418">式は `Select` 式の型に変換されます。式が `Select` 式の型に暗黙的に変換できない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="eb747-418">The expression is converted to the type of the `Select` expression; if the expression is not implicitly convertible to the type of the `Select` expression, a compile-time error occurs.</span></span> <span data-ttu-id="eb747-419">`Select` 式が*e*の場合、比較演算子は*Op*、`Case` 式は*E1*、case は*e Op E1*として評価されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-419">If the `Select` expression is *E*, the comparison operator is *Op*, and the `Case` expression is *E1*, the case is evaluated as *E OP E1*.</span></span> <span data-ttu-id="eb747-420">演算子は、2つの式の型に対して有効である必要があります。それ以外の場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="eb747-420">The operator must be valid for the types of the two expressions; otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="eb747-421">もう1つの形式は、必要に応じて、キーワード `To` と2番目の式の後に続く式です。</span><span class="sxs-lookup"><span data-stu-id="eb747-421">The other form is an expression optionally followed by the keyword `To` and a second expression.</span></span> <span data-ttu-id="eb747-422">両方の式は、`Select` 式の型に変換されます。いずれかの式が `Select` 式の型に暗黙的に変換できない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="eb747-422">Both expressions are converted to the type of the `Select` expression; if either expression is not implicitly convertible to the type of the `Select` expression, a compile-time error occurs.</span></span> <span data-ttu-id="eb747-423">`Select` 式が `E`の場合、最初の `Case` 式が `E1`、2番目の `Case` 式が `E2`の場合は、`Case` (`E = E1` が指定されていない場合) または `E2` として評価されます。`(E >= E1) And (E <= E2)`</span><span class="sxs-lookup"><span data-stu-id="eb747-423">If the `Select` expression is `E`, the first `Case` expression is `E1`, and the second `Case` expression is `E2`, the `Case` is evaluated either as `E = E1` (if no `E2` is specified) or `(E >= E1) And (E <= E2)`.</span></span> <span data-ttu-id="eb747-424">演算子は、2つの式の型に対して有効である必要があります。それ以外の場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="eb747-424">The operators must be valid for the types of the two expressions; otherwise a compile-time error occurs.</span></span>


## <a name="loop-statements"></a><span data-ttu-id="eb747-425">Loop ステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-425">Loop Statements</span></span>

<span data-ttu-id="eb747-426">Loop ステートメントを使用すると、本体でステートメントを繰り返し実行できます。</span><span class="sxs-lookup"><span data-stu-id="eb747-426">Loop statements allow repeated execution of the statements in their body.</span></span>

```antlr
LoopStatement
    : WhileStatement
    | DoLoopStatement
    | ForStatement
    | ForEachStatement
    ;
```

<span data-ttu-id="eb747-427">ループ本体が入力されるたびに、その本体で宣言されたすべてのローカル変数の新しいコピーが作成され、変数の前の値に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-427">Each time a loop body is entered, a fresh copy is made of all local variables declared in that body, initialized to the previous values of the variables.</span></span> <span data-ttu-id="eb747-428">ループ本体内の変数への参照は、最後に作成されたコピーを使用します。</span><span class="sxs-lookup"><span data-stu-id="eb747-428">Any reference to a variable within the loop body will use the most recently made copy.</span></span> <span data-ttu-id="eb747-429">このコードは例を示しています。</span><span class="sxs-lookup"><span data-stu-id="eb747-429">This code shows an example:</span></span>

```vb
Module Test
    Sub Main()
        Dim lambdas As New List(Of Action)
        Dim x = 1

        For i = 1 To 3
            x = i
            Dim y = x
            lambdas.Add(Sub() Console.WriteLine(x & y))
        Next

        For Each lambda In lambdas
            lambda()
        Next
    End Sub
End Module
```

<span data-ttu-id="eb747-430">このコードでは、次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-430">The code produces the output:</span></span>

```console
31    32    33
```

<span data-ttu-id="eb747-431">ループ本体が実行されると、変数のどのコピーが最新であるかが使用されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-431">When the loop body is executed, it uses whichever copy of the variable is current.</span></span> <span data-ttu-id="eb747-432">たとえば、ステートメント `Dim y = x` は、`y` の最新のコピーと `x`の元のコピーを参照します。</span><span class="sxs-lookup"><span data-stu-id="eb747-432">For example, the statement  `Dim y = x` refers to the latest copy of `y` and the original copy of `x`.</span></span> <span data-ttu-id="eb747-433">また、ラムダが作成されると、変数のコピーが作成された時点で最新だったことが記憶されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-433">And when a lambda is created, it remembers whichever copy of a variable was current at the time it was created.</span></span> <span data-ttu-id="eb747-434">したがって、各ラムダは `x`の同じ共有コピーを使用しますが、`y`の異なるコピーを使用します。</span><span class="sxs-lookup"><span data-stu-id="eb747-434">Therefore, each lambda uses the same shared copy of `x`, but a different copy of `y`.</span></span> <span data-ttu-id="eb747-435">プログラムの最後にラムダを実行すると、そのすべてが参照している `x` の共有コピーが最終的な値3になります。</span><span class="sxs-lookup"><span data-stu-id="eb747-435">At the end of the program, when it executes the lambdas, that shared copy of `x` that they all refer to is now at its final value 3.</span></span>

<span data-ttu-id="eb747-436">ラムダ式または LINQ 式がない場合は、ループエントリに対して新しいコピーが作成されていることを認識できないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="eb747-436">Note that if there are no lambdas or LINQ expressions, then it's impossible to know that a fresh copy is made on loop entry.</span></span> <span data-ttu-id="eb747-437">実際には、コンパイラの最適化によって、この場合にコピーを作成することは避けられます。</span><span class="sxs-lookup"><span data-stu-id="eb747-437">Indeed, compiler optimizations will avoid making copies in this case.</span></span> <span data-ttu-id="eb747-438">また、ラムダ式または LINQ 式を含むループに `GoTo` するのは無効であることにも注意してください。</span><span class="sxs-lookup"><span data-stu-id="eb747-438">Note too that it's illegal to `GoTo` into a loop that contains lambdas or LINQ expressions.</span></span>


### <a name="whileend-while-and-doloop-statements"></a><span data-ttu-id="eb747-439">しばらくお待ちください...しばらくしてから終了します...Loop ステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-439">While...End While and Do...Loop Statements</span></span>

<span data-ttu-id="eb747-440">`While` または `Do` loop ステートメントは、ブール式に基づいてループします。</span><span class="sxs-lookup"><span data-stu-id="eb747-440">A `While` or `Do` loop statement loops based on a Boolean expression.</span></span>

```antlr
WhileStatement
    : 'While' BooleanExpression StatementTerminator
      Block?
      'End' 'While' StatementTerminator
    ;

DoLoopStatement
    : DoTopLoopStatement
    | DoBottomLoopStatement
    ;

DoTopLoopStatement
    : 'Do' ( WhileOrUntil BooleanExpression )? StatementTerminator
      Block?
      'Loop' StatementTerminator
    ;

DoBottomLoopStatement
    : 'Do' StatementTerminator
      Block?
      'Loop' WhileOrUntil BooleanExpression StatementTerminator
    ;

WhileOrUntil
    : 'While' | 'Until'
    ;
```

<span data-ttu-id="eb747-441">ブール式が true と評価されている限り、`While` loop ステートメントはループします。`Do` loop ステートメントには、より複雑な条件を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="eb747-441">A `While` loop statement loops as long as the Boolean expression evaluates to true; a `Do` loop statement may contain a more complex condition.</span></span> <span data-ttu-id="eb747-442">式は、`Do` キーワードの後、または `Loop` キーワードの後に配置できますが、両方の後に配置することはできません。</span><span class="sxs-lookup"><span data-stu-id="eb747-442">An expression may be placed after the `Do` keyword or after the `Loop` keyword, but not after both.</span></span> <span data-ttu-id="eb747-443">ブール式は、セクションごとの[ブール式](expressions.md#boolean-expressions)として評価されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-443">The Boolean expression is evaluated as per Section [Boolean Expressions](expressions.md#boolean-expressions).</span></span> <span data-ttu-id="eb747-444">(注: この場合、式はブール型である必要はありません)。</span><span class="sxs-lookup"><span data-stu-id="eb747-444">(Note: this does not require the expression to have Boolean type).</span></span> <span data-ttu-id="eb747-445">式をまったく指定しないこともできます。この場合、ループは終了しません。</span><span class="sxs-lookup"><span data-stu-id="eb747-445">It is also valid to specify no expression at all; in that case, the loop will never exit.</span></span> <span data-ttu-id="eb747-446">`Do`後に式を配置した場合は、反復処理のたびにループブロックが実行される前に評価されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-446">If the expression is placed after `Do`, it will be evaluated before the loop block is executed on each iteration.</span></span> <span data-ttu-id="eb747-447">`Loop`後に式が配置された場合は、各反復処理でループブロックが実行された後に評価されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-447">If the expression is placed after `Loop`, it will be evaluated after the loop block has executed on each iteration.</span></span> <span data-ttu-id="eb747-448">したがって、`Loop` の後に式を配置すると、`Do`後の配置よりも1つ多くのループが生成されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-448">Placing the expression after `Loop` will therefore generate one more loop than placement after `Do`.</span></span> <span data-ttu-id="eb747-449">次の例は、この動作を示しています。</span><span class="sxs-lookup"><span data-stu-id="eb747-449">The following example demonstrates this behavior:</span></span>

```vb
Module Test
    Sub Main()
        Dim x As Integer

        x = 3
        Do While x = 1
            Console.WriteLine("First loop")
        Loop

        Do
            Console.WriteLine("Second loop")
        Loop While x = 1
    End Sub
End Module
```

<span data-ttu-id="eb747-450">このコードでは、次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-450">The code produces the output:</span></span>

```console
Second Loop
```

<span data-ttu-id="eb747-451">最初のループの場合は、ループが実行される前に条件が評価されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-451">In the case of the first loop, the condition is evaluated before the loop executes.</span></span> <span data-ttu-id="eb747-452">2番目のループの場合は、ループの実行後に条件が実行されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-452">In the case of the second loop, the condition is executed after the loop executes.</span></span> <span data-ttu-id="eb747-453">条件式の先頭には、`While` キーワードまたは `Until` キーワードを付ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-453">The conditional expression must be prefixed with either a `While` keyword or an `Until` keyword.</span></span> <span data-ttu-id="eb747-454">条件が false と評価された場合、前者はループを中断します。後者の場合、条件が true と評価されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-454">The former breaks the loop if the condition evaluates to false, the latter when the condition evaluates to true.</span></span>

<span data-ttu-id="eb747-455">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="eb747-455">__Note.__</span></span> <span data-ttu-id="eb747-456">`Until` は予約語ではありません。</span><span class="sxs-lookup"><span data-stu-id="eb747-456">`Until` is not a reserved word.</span></span>


### <a name="fornext-statements"></a><span data-ttu-id="eb747-457">...次のステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-457">For...Next Statements</span></span>

<span data-ttu-id="eb747-458">`For...Next` ステートメントは、一連の境界に基づいてループします。</span><span class="sxs-lookup"><span data-stu-id="eb747-458">A `For...Next` statement loops based on a set of bounds.</span></span> <span data-ttu-id="eb747-459">`For` ステートメントでは、ループコントロール変数、下限式、上限式、および省略可能なステップ値式を指定します。</span><span class="sxs-lookup"><span data-stu-id="eb747-459">A `For` statement specifies a loop control variable, a lower bound expression, an upper bound expression, and an optional step value expression.</span></span> <span data-ttu-id="eb747-460">ループコントロール変数は、識別子の後に省略可能な `As` 句または式を指定して指定します。</span><span class="sxs-lookup"><span data-stu-id="eb747-460">The loop control variable is specified either through an identifier followed by an optional `As` clause or an expression.</span></span>

```antlr
ForStatement
    : 'For' LoopControlVariable Equals Expression 'To' Expression
      ( 'Step' Expression )? StatementTerminator
      Block?
      ( 'Next' NextExpressionList? StatementTerminator )?
    ;

LoopControlVariable
    : Identifier ( IdentifierModifiers 'As' TypeName )?
    | Expression
    ;

NextExpressionList
    : Expression ( Comma Expression )*
    ;
```

<span data-ttu-id="eb747-461">次の規則に従って、loop コントロール変数は、この `For...Next` ステートメント、または既存の変数または式に固有の新しいローカル変数を参照します。</span><span class="sxs-lookup"><span data-stu-id="eb747-461">As per the following rules, the loop control variable refers either to a new local variable specific to this `For...Next` statement, or to a pre-existing variable, or to an expression.</span></span>

* <span data-ttu-id="eb747-462">Loop コントロール変数が `As` 句を含む識別子の場合、識別子は、`As` 句で指定された型の新しいローカル変数を定義します。この変数の範囲は `For` ループ全体になります。</span><span class="sxs-lookup"><span data-stu-id="eb747-462">If the loop control variable is an identifier with an `As` clause, the identifier defines a new local variable of the type specified in the `As` clause, scoped to the entire `For` loop.</span></span>

* <span data-ttu-id="eb747-463">Loop コントロール変数が `As` 句を含まない識別子である場合、識別子はまず単純な名前解決規則 (「 [Simple Name 式](expressions.md#simple-name-expressions)」を参照) を使用して解決されます。この識別子が違うではなく、それ自体が暗黙的なローカル変数を作成することになります (「[暗黙のローカル宣言](statements.md#implicit-local-declarations)」を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="eb747-463">If the loop control variable is an identifier without an `As` clause, then the identifier is first resolved using the simple name resolution rules (see Section [Simple Name Expressions](expressions.md#simple-name-expressions)), excepting that this occurrence of the identifier would not in and of itself cause an implicit local variable to be created (Section [Implicit Local Declarations](statements.md#implicit-local-declarations)).</span></span>

  * <span data-ttu-id="eb747-464">この解決が成功し、結果が変数として分類される場合、loop コントロール変数は既存の変数になります。</span><span class="sxs-lookup"><span data-stu-id="eb747-464">If this resolution succeeds and the result is classified as a variable, then the loop control variable is that pre-existing variable.</span></span>

  * <span data-ttu-id="eb747-465">解決が失敗した場合、または解決が成功し、結果が型として分類された場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="eb747-465">If resolution fails, or if resolution succeeds and the result is classified as a type, then:</span></span>
    * <span data-ttu-id="eb747-466">ローカル変数の型の推論が使用されている場合、識別子は、バインドされた式およびステップ式から推論される型を持つ新しいローカル変数を定義し、`For` ループ全体を対象とします。</span><span class="sxs-lookup"><span data-stu-id="eb747-466">if local variable type inference is being used, the identifier defines a new local variable whose type is inferred from the bound and step expressions, scoped to the entire `For` loop;</span></span>
    * <span data-ttu-id="eb747-467">ローカル変数の型の推定が使用されていないが、暗黙的なローカル宣言がの場合は、スコープがメソッド全体 (セクションの[暗黙のローカル宣言](statements.md#implicit-local-declarations)) であり、ループコントロール変数がこの既存の変数を参照する、暗黙的なローカル変数が作成されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-467">if local variable type inference is not being used but implicit local declaration is, then an implicit local variable is created whose scope is the entire method (Section [Implicit Local Declarations](statements.md#implicit-local-declarations)), and the loop control variable refers to this pre-existing variable;</span></span>
    * <span data-ttu-id="eb747-468">ローカル変数の型の推論も暗黙的なローカル宣言も使用されていない場合は、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="eb747-468">if neither local variable type inference nor implicit local declarations are used, it is an error.</span></span>

  * <span data-ttu-id="eb747-469">型でも変数でもないように分類された解決が成功すると、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="eb747-469">If resolution succeeds with something classified as neither a type nor a variable, it is an error.</span></span>

* <span data-ttu-id="eb747-470">Loop コントロール変数が式である場合は、式を変数として分類する必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-470">If the loop control variable is an expression, the expression must be classified as a variable.</span></span>

<span data-ttu-id="eb747-471">ループコントロール変数は、それを囲む外側の `For...Next` ステートメントでは使用できません。</span><span class="sxs-lookup"><span data-stu-id="eb747-471">A loop control variable cannot be used by another enclosing `For...Next` statement.</span></span> <span data-ttu-id="eb747-472">`For` ステートメントのループ制御変数の型によって、イテレーションの型が決定されます。次のいずれかである必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-472">The type of the loop control variable of a `For` statement determines the type of the iteration and must be one of:</span></span>

* <span data-ttu-id="eb747-473">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`</span><span class="sxs-lookup"><span data-stu-id="eb747-473">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`</span></span>
* <span data-ttu-id="eb747-474">列挙型</span><span class="sxs-lookup"><span data-stu-id="eb747-474">An enumerated type</span></span>
* `Object`
* <span data-ttu-id="eb747-475">次の演算子を持つ型 `T`。 `B` は、ブール式で使用できる型です。</span><span class="sxs-lookup"><span data-stu-id="eb747-475">A type `T` that has the following operators, where `B` is a type that can be used in a Boolean expression:</span></span>

```vb
Public Shared Operator >= (op1 As T, op2 As T) As B
Public Shared Operator <= (op1 As T, op2 As T) As B
Public Shared Operator - (op1 As T, op2 As T) As T
Public Shared Operator + (op1 As T, op2 As T) As T
```

<span data-ttu-id="eb747-476">バインドおよびステップ式は、ループコントロール変数の型に暗黙的に変換できる必要があり、値として分類される必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-476">The bound and step expressions must be implicitly convertible to the type of the loop control variable and must be classified as values.</span></span> <span data-ttu-id="eb747-477">コンパイル時に、ループコントロール変数の型は、下限、上限、およびステップ式の型の中で最も幅の広い型を選択することによって推論されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-477">At compile time, the type of the loop control variable is inferred by choosing the widest type among the lower bound, upper bound, and step expression types.</span></span> <span data-ttu-id="eb747-478">2つの型の間に拡大変換がない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="eb747-478">If there is no widening conversion between two of the types, then a compile-time error occurs.</span></span>

<span data-ttu-id="eb747-479">実行時に、ループコントロール変数の型が `Object`場合、反復処理の型はコンパイル時と同じように推論されますが、2つの例外があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-479">At run time, if the type of the loop control variable is `Object`, then the type of the iteration is inferred the same as at compile time, with two exceptions.</span></span> <span data-ttu-id="eb747-480">最初に、バインドされた式とステップ式がすべて整数型であり、最も幅の広い型がない場合、3つすべての型を含む最も幅の広い型が推論されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-480">First, if the bound and step expressions are all of integral types but have no widest type, then the widest type that encompasses all three types will be inferred.</span></span> <span data-ttu-id="eb747-481">次に、ループコントロール変数の型が `String`されると推論される場合は、代わりに `Double` が推論されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-481">And second, if the type of the loop control variable is inferred to be `String`, `Double` will be inferred instead.</span></span> <span data-ttu-id="eb747-482">実行時に loop コントロール型を決定できない場合、またはいずれかの式を loop コントロール型に変換できない場合は、`System.InvalidCastException` が発生します。</span><span class="sxs-lookup"><span data-stu-id="eb747-482">If, at run time, no loop control type can be determined or if any of the expressions cannot be converted to the loop control type, a `System.InvalidCastException` will occur.</span></span> <span data-ttu-id="eb747-483">ループの先頭でループコントロール型が選択されると、ループコントロール変数の値に加えられた変更に関係なく、同じ型がイテレーション全体で使用されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-483">Once a loop control type has been chosen at the beginning of the loop, the same type will be used throughout the iteration, regardless of changes made to the value in the loop control variable.</span></span>

<span data-ttu-id="eb747-484">`For` ステートメントは、一致する `Next` ステートメントで閉じる必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-484">A `For` statement must be closed by a matching `Next` statement.</span></span> <span data-ttu-id="eb747-485">変数のない `Next` ステートメントは最も内側にある open `For` ステートメントと一致しますが、1つ以上の loop コントロール変数を持つ `Next` ステートメントは左から右に、各変数に一致する `For` ループと一致します。</span><span class="sxs-lookup"><span data-stu-id="eb747-485">A `Next` statement without a variable matches the innermost open `For` statement, while a `Next` statement with one or more loop control variables will, from left to right, match the `For` loops that match each variable.</span></span> <span data-ttu-id="eb747-486">変数が、その時点で最も入れ子になっているループではない `For` ループと一致する場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="eb747-486">If a variable matches a `For` loop that is not the most nested loop at that point, a compile-time error results.</span></span>

<span data-ttu-id="eb747-487">ループの先頭では、3つの式がテキスト順に評価され、下限の式が loop コントロール変数に代入されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-487">At the beginning of the loop, the three expressions are evaluated in textual order and the lower bound expression is assigned to the loop control variable.</span></span> <span data-ttu-id="eb747-488">ステップ値を省略した場合は、暗黙的にリテラル `1`になり、ループコントロール変数の型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-488">If the step value is omitted, it is implicitly the literal `1`, converted to the type of the loop control variable.</span></span> <span data-ttu-id="eb747-489">3つの式は、ループの先頭でのみ評価されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-489">The three expressions are only ever evaluated at the beginning of the loop.</span></span>

<span data-ttu-id="eb747-490">各ループの先頭では、コントロール変数が比較され、ステップ式が正の場合はエンドポイントより大きいか、またはステップ式が負の場合は終了点よりも小さいかどうかが確認されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-490">At the beginning of each loop, the control variable is compared to see if it is greater than the end point if the step expression is positive, or less than the end point if the step expression is negative.</span></span> <span data-ttu-id="eb747-491">の場合、`For` ループは終了します。それ以外の場合は、ループブロックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-491">If it is, the `For` loop terminates; otherwise the loop block executes.</span></span> <span data-ttu-id="eb747-492">ループコントロール変数がプリミティブ型でない場合、比較演算子は `step >= step - step` 式が true か false かによって決まります。</span><span class="sxs-lookup"><span data-stu-id="eb747-492">If the loop control variable is not a primitive type, the comparison operator is determined by whether the expression `step >= step - step` is true or false.</span></span> <span data-ttu-id="eb747-493">`Next` ステートメントでは、ステップの値がコントロール変数に追加され、実行がループの先頭に戻ります。</span><span class="sxs-lookup"><span data-stu-id="eb747-493">At the `Next` statement, the step value is added to the control variable and execution returns to the top of the loop.</span></span>

<span data-ttu-id="eb747-494">Loop コントロール変数の新しいコピーは、ループブロックの反復処理のたびに作成され*ない*ことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="eb747-494">Note that a new copy of the loop control variable is *not* created on each iteration of the loop block.</span></span> <span data-ttu-id="eb747-495">この点で、`For` ステートメントは `For Each` ([各...次のステートメント](statements.md#for-eachnext-statements))。</span><span class="sxs-lookup"><span data-stu-id="eb747-495">In this respect, the `For` statement differs from `For Each` (Section [For Each...Next Statements](statements.md#for-eachnext-statements)).</span></span>

<span data-ttu-id="eb747-496">ループの外側から `For` ループに分岐することは無効です。</span><span class="sxs-lookup"><span data-stu-id="eb747-496">It is not valid to branch into a `For` loop from outside the loop.</span></span>


### <a name="for-eachnext-statements"></a><span data-ttu-id="eb747-497">各...次のステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-497">For Each...Next Statements</span></span>

<span data-ttu-id="eb747-498">`For Each...Next` ステートメントは、式の要素に基づいてループします。</span><span class="sxs-lookup"><span data-stu-id="eb747-498">A `For Each...Next` statement loops based on the elements in an expression.</span></span> <span data-ttu-id="eb747-499">`For Each` ステートメントは、ループコントロール変数と列挙子式を指定します。</span><span class="sxs-lookup"><span data-stu-id="eb747-499">A `For Each` statement specifies a loop control variable and an enumerator expression.</span></span> <span data-ttu-id="eb747-500">ループコントロール変数は、識別子の後に省略可能な `As` 句または式を指定して指定します。</span><span class="sxs-lookup"><span data-stu-id="eb747-500">The loop control variable is specified either through an identifier followed by an optional `As` clause or an expression.</span></span>

```antlr
ForEachStatement
    : 'For' 'Each' LoopControlVariable 'In' LineTerminator? Expression StatementTerminator
      Block?
      ( 'Next' NextExpressionList? StatementTerminator )?
    ;
```

<span data-ttu-id="eb747-501">`For...Next` ステートメントと同じ規則に従っている (.. [.次のステートメント](statements.md#fornext-statements)) では、ループコントロール変数は、それぞれに固有の新しいローカル変数を参照します。次のステートメント、または既存の変数または式を指定します。</span><span class="sxs-lookup"><span data-stu-id="eb747-501">Following the same rules as `For...Next` statements (Section [For...Next Statements](statements.md#fornext-statements)), the loop control variable refers either to a new local variable specific to this For Each...Next statement, or to a pre-existing variable, or to an expression.</span></span>

<span data-ttu-id="eb747-502">列挙子式は値として分類する必要があり、その型はコレクション型または `Object`である必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-502">The enumerator expression must be classified as a value and its type must be a collection type or `Object`.</span></span> <span data-ttu-id="eb747-503">列挙子式の型が `Object`場合、すべての処理が実行時まで延期されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-503">If the type of the enumerator expression is `Object`, then all processing is deferred until run-time.</span></span> <span data-ttu-id="eb747-504">それ以外の場合は、コレクションの要素型からループコントロール変数の型への変換が存在している必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-504">Otherwise, a conversion must exist from the element type of the collection to the type of the loop control variable.</span></span>

<span data-ttu-id="eb747-505">Loop コントロール変数は、それを囲む外側の `For Each` ステートメントでは使用できません。</span><span class="sxs-lookup"><span data-stu-id="eb747-505">The loop control variable cannot be used by another enclosing `For Each` statement.</span></span> <span data-ttu-id="eb747-506">`For Each` ステートメントは、一致する `Next` ステートメントで閉じる必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-506">A `For Each` statement must be closed by a matching `Next` statement.</span></span> <span data-ttu-id="eb747-507">ループコントロール変数のない `Next` ステートメントは、最も内側にあるオープン `For Each`と一致します。</span><span class="sxs-lookup"><span data-stu-id="eb747-507">A `Next` statement without a loop control variable matches the innermost open `For Each`.</span></span> <span data-ttu-id="eb747-508">1つ以上の loop コントロール変数を持つ `Next` ステートメントは、左から右に、同じ loop コントロール変数を持つ `For Each` ループと一致します。</span><span class="sxs-lookup"><span data-stu-id="eb747-508">A `Next` statement with one or more loop control variables will, from left to right, match the `For Each` loops that have the same loop control variable.</span></span> <span data-ttu-id="eb747-509">変数が、その時点で最も入れ子になっているループではない `For Each` ループと一致する場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="eb747-509">If a variable matches a `For Each` loop that is not the most nested loop at that point, a compile-time error occurs.</span></span>

<span data-ttu-id="eb747-510">型 `C` は、次のいずれかの場合に*コレクション型*と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="eb747-510">A type `C` is said to be a *collection type* if one of:</span></span>

* <span data-ttu-id="eb747-511">次のすべてが当てはまります。</span><span class="sxs-lookup"><span data-stu-id="eb747-511">All of the following are true:</span></span>
  * <span data-ttu-id="eb747-512">`C` には、`E`型を返すシグネチャ `GetEnumerator()` を持つ、アクセス可能なインスタンス、共有、または拡張メソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="eb747-512">`C` contains an accessible instance, shared or extension method with the signature `GetEnumerator()` that returns a type `E`.</span></span>
  * <span data-ttu-id="eb747-513">`E` には、シグネチャ `MoveNext()` と戻り値の型 `Boolean`を持つ、アクセス可能なインスタンス、共有、または拡張メソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="eb747-513">`E` contains an accessible instance, shared or extension method with the signature `MoveNext()` and the return type `Boolean`.</span></span>
  * <span data-ttu-id="eb747-514">`E` には、getter を持つ `Current` という名前のアクセス可能なインスタンスまたは共有プロパティが含まれています。</span><span class="sxs-lookup"><span data-stu-id="eb747-514">`E` contains an accessible instance or shared property named `Current` that has a getter.</span></span> <span data-ttu-id="eb747-515">このプロパティの型は、コレクション型の要素型です。</span><span class="sxs-lookup"><span data-stu-id="eb747-515">The type of this property is the element type of the collection type.</span></span>

* <span data-ttu-id="eb747-516">インターフェイス `System.Collections.Generic.IEnumerable(Of T)`を実装します。この場合、コレクションの要素型は `T`と見なされます。</span><span class="sxs-lookup"><span data-stu-id="eb747-516">It implements the interface `System.Collections.Generic.IEnumerable(Of T)`, in which case the element type of the collection is considered to be `T`.</span></span>

* <span data-ttu-id="eb747-517">インターフェイス `System.Collections.IEnumerable`を実装します。この場合、コレクションの要素型は `Object`と見なされます。</span><span class="sxs-lookup"><span data-stu-id="eb747-517">It implements the interface `System.Collections.IEnumerable`, in which case the element type of the collection is considered to be `Object`.</span></span>

<span data-ttu-id="eb747-518">列挙できるクラスの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="eb747-518">Following is an example of a class that can be enumerated:</span></span>

```vb
Public Class IntegerCollection
    Private integers(10) As Integer

    Public Class IntegerCollectionEnumerator
        Private collection As IntegerCollection
        Private index As Integer = -1

        Friend Sub New(c As IntegerCollection)
            collection = c
        End Sub

        Public Function MoveNext() As Boolean
            index += 1

            Return index <= 10
        End Function

        Public ReadOnly Property Current As Integer
            Get
                If index < 0 OrElse index > 10 Then
                    Throw New System.InvalidOperationException()
                End If

                Return collection.integers(index)
            End Get
        End Property
    End Class

    Public Sub New()
        Dim i As Integer

        For i = 0 To 10
            integers(i) = I
        Next i
    End Sub

    Public Function GetEnumerator() As IntegerCollectionEnumerator
        Return New IntegerCollectionEnumerator(Me)
    End Function
End Class
```

<span data-ttu-id="eb747-519">ループが開始される前に、列挙子式が評価されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-519">Before the loop begins, the enumerator expression is evaluated.</span></span> <span data-ttu-id="eb747-520">式の型がデザインパターンを満たしていない場合、式は `System.Collections.IEnumerable` または `System.Collections.Generic.IEnumerable(Of T)`にキャストされます。</span><span class="sxs-lookup"><span data-stu-id="eb747-520">If the type of the expression does not satisfy the design pattern, then the expression is cast to `System.Collections.IEnumerable` or `System.Collections.Generic.IEnumerable(Of T)`.</span></span> <span data-ttu-id="eb747-521">式の型でジェネリックインターフェイスが実装されている場合、ジェネリックインターフェイスはコンパイル時に優先されますが、非ジェネリックインターフェイスは実行時に優先されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-521">If the expression type implements the generic interface, the generic interface is preferred at compile-time but the non-generic interface is preferred at run-time.</span></span> <span data-ttu-id="eb747-522">式の型でジェネリックインターフェイスが複数回実装されている場合、ステートメントはあいまいであると見なされ、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="eb747-522">If the expression type implements the generic interface multiple times, the statement is considered ambiguous and a compile-time error occurs.</span></span>

<span data-ttu-id="eb747-523">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="eb747-523">__Note.__</span></span> <span data-ttu-id="eb747-524">非ジェネリックインターフェイスは、ジェネリックインターフェイスを選択すると、インターフェイスメソッドのすべての呼び出しに型パラメーターが含まれるということを意味するため、遅延バインディングの場合に推奨されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-524">The non-generic interface is preferred in the late bound case, because picking the generic interface would mean that all the calls to the interface methods would involve type parameters.</span></span> <span data-ttu-id="eb747-525">実行時に一致する型引数を知ることはできないため、このような呼び出しはすべて遅延バインディング呼び出しを使用して行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-525">Since it is not possible to know the matching type arguments at run-time, all such calls would have to be made using late-bound calls.</span></span> <span data-ttu-id="eb747-526">非ジェネリックインターフェイスは、コンパイル時の呼び出しを使用して呼び出すことができるため、非ジェネリックインターフェイスを呼び出すよりも遅くなります。</span><span class="sxs-lookup"><span data-stu-id="eb747-526">This would be slower than calling the non-generic interface because the non-generic interface could be called using compile-time calls.</span></span>

<span data-ttu-id="eb747-527">結果の値に対して `GetEnumerator` が呼び出され、関数の戻り値は一時的なに格納されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-527">`GetEnumerator` is called on the resulting value and the return value of the function is stored in a temporary.</span></span> <span data-ttu-id="eb747-528">その後、各イテレーションの開始時に、`MoveNext` が一時的に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-528">Then at the beginning of each iteration, `MoveNext` is called on the temporary.</span></span> <span data-ttu-id="eb747-529">`False`が返された場合、ループは終了します。</span><span class="sxs-lookup"><span data-stu-id="eb747-529">If it returns `False`, the loop terminates.</span></span> <span data-ttu-id="eb747-530">それ以外の場合、ループの各反復は次のように実行されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-530">Otherwise, each iteration of the loop is executed as follows:</span></span>

1. <span data-ttu-id="eb747-531">ループコントロール変数が、既存の変数ではなく新しいローカル変数を識別した場合、このローカル変数の新しいコピーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-531">If the loop control variable identified a new local variable (rather than a pre-existing one), then a fresh copy of this local variable is created.</span></span> <span data-ttu-id="eb747-532">現在の反復処理では、ループブロック内のすべての参照がこのコピーを参照します。</span><span class="sxs-lookup"><span data-stu-id="eb747-532">For the current iteration, all references within the loop block will refer to this copy.</span></span>
2. <span data-ttu-id="eb747-533">`Current` プロパティが取得され、ループコントロール変数の型に変換されます (変換が暗黙的か明示的かに関係なく)、ループコントロール変数に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="eb747-533">The `Current` property is retrieved, coerced to the type of the loop control variable (regardless of whether the conversion is implicit or explicit), and assigned to the loop control variable.</span></span>
3. <span data-ttu-id="eb747-534">ループブロックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-534">The loop block executes.</span></span>

<span data-ttu-id="eb747-535">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="eb747-535">__Note.__</span></span> <span data-ttu-id="eb747-536">言語のバージョン10.0 と11.0 の間で動作が多少変更されています。</span><span class="sxs-lookup"><span data-stu-id="eb747-536">There is a slight change in behavior between version 10.0 and 11.0 of the language.</span></span> <span data-ttu-id="eb747-537">11.0 より前では、ループの反復ごとに新しい反復変数は作成され*ません*でした。</span><span class="sxs-lookup"><span data-stu-id="eb747-537">Prior to 11.0, a fresh iteration variable was *not* created for each iteration of the loop.</span></span> <span data-ttu-id="eb747-538">この違いは、反復変数がラムダまたはループの後に呼び出される LINQ 式によってキャプチャされた場合にのみ観測可能です。</span><span class="sxs-lookup"><span data-stu-id="eb747-538">This difference is observable only if the iteration variable is captured by a lambda or a LINQ expression which is then invoked after the loop:</span></span>

```vb
Dim lambdas As New List(Of Action)
For Each x In {1,2,3}
   lambdas.Add(Sub() Console.WriteLine(x)
Next
lambdas(0).Invoke()
lambdas(1).Invoke()
lambdas(2).Invoke()
```

<span data-ttu-id="eb747-539">Visual Basic 10.0 までは、コンパイル時に警告が生成され、"3" が3回出力されました。</span><span class="sxs-lookup"><span data-stu-id="eb747-539">Up to Visual Basic 10.0, this produced a warning at compile-time and printed "3" three times.</span></span> <span data-ttu-id="eb747-540">これは、ループのすべての反復処理によって共有される変数 "x" が1つだけであり、3つのラムダすべてが同じ "x" をキャプチャし、ラムダが実行されたときに、数値3を保持したためです。</span><span class="sxs-lookup"><span data-stu-id="eb747-540">That was because there was only a single variable "x" shared by all iterations of the loop, and all three lambdas captured the same "x", and by the time the lambdas were executed it then held the number 3.</span></span> <span data-ttu-id="eb747-541">Visual Basic 11.0 では、"1, 2, 3" が出力されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-541">As of Visual Basic 11.0, it prints "1, 2, 3".</span></span> <span data-ttu-id="eb747-542">これは、各ラムダが異なる変数 "x" をキャプチャするためです。</span><span class="sxs-lookup"><span data-stu-id="eb747-542">That is because each lambda captures a different variable "x".</span></span>


<span data-ttu-id="eb747-543">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="eb747-543">__Note.__</span></span> <span data-ttu-id="eb747-544">ステートメントに変換演算子を導入するための便利な場所がないため、変換が明示的である場合でも、反復処理の現在の要素はループコントロール変数の型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-544">The current element of the iteration is converted to the type of the loop control variable even if the conversion is explicit because there is no convenient place to introduce a conversion operator in the statement.</span></span> <span data-ttu-id="eb747-545">これは、現在廃止されている型 `System.Collections.ArrayList`を操作するときに特に厄介になりました。これは、その要素型が `Object`ためです。</span><span class="sxs-lookup"><span data-stu-id="eb747-545">This became particularly troublesome when working with the now-obsolete type `System.Collections.ArrayList`, because its element type is `Object`.</span></span> <span data-ttu-id="eb747-546">これには、非常に多くのループで必要なキャストが含まれています。これは理想的ではありませんでした。</span><span class="sxs-lookup"><span data-stu-id="eb747-546">This would have required casts in a great many loops, something we felt was not ideal.</span></span> <span data-ttu-id="eb747-547">皮肉なことでは、厳密に型指定されたコレクションを作成できるようになりました。 `System.Collections.Generic.List(Of T)`はこのデザインポイントを再考する可能性がありますが、互換性を確保するためにこれを変更することはできません。</span><span class="sxs-lookup"><span data-stu-id="eb747-547">Ironically, generics enabled the creation of a strongly-typed collection, `System.Collections.Generic.List(Of T)`, which might have made us rethink this design point, but for compatibility's sake, this cannot be changed now.</span></span>


<span data-ttu-id="eb747-548">`Next` ステートメントに到達すると、実行がループの先頭に戻ります。</span><span class="sxs-lookup"><span data-stu-id="eb747-548">When the `Next` statement is reached, execution returns to the top of the loop.</span></span> <span data-ttu-id="eb747-549">`Next` キーワードの後に変数を指定する場合は、`For Each`の後の最初の変数と同じである必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-549">If a variable is specified after the `Next` keyword, it must be the same as the first variable after the `For Each`.</span></span> <span data-ttu-id="eb747-550">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="eb747-550">For example, consider the following code:</span></span>

```vb
Module Test
    Sub Main()
        Dim i As Integer
        Dim c As IntegerCollection = New IntegerCollection()

        For Each i In c
            Console.WriteLine(i)
        Next i
    End Sub
End Module
```

<span data-ttu-id="eb747-551">これは、次のコードと同じです。</span><span class="sxs-lookup"><span data-stu-id="eb747-551">It is equivalent to the following code:</span></span>

```vb
Module Test
    Sub Main()
        Dim i As Integer
        Dim c As IntegerCollection = New IntegerCollection()

        Dim e As IntegerCollection.IntegerCollectionEnumerator

        e = c.GetEnumerator()
        While e.MoveNext()
            i = e.Current

            Console.WriteLine(i)
        End While
    End Sub
End Module
```

<span data-ttu-id="eb747-552">列挙子の型 `E` が `System.IDisposable`を実装している場合は、`Dispose` メソッドを呼び出すことによって、ループの終了時に列挙子が破棄されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-552">If the type `E` of the enumerator implements `System.IDisposable`, then the enumerator is disposed upon exiting the loop by calling the `Dispose` method.</span></span> <span data-ttu-id="eb747-553">これにより、列挙子によって保持されているリソースが解放されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-553">This ensures that resources held by the enumerator are released.</span></span> <span data-ttu-id="eb747-554">`For Each` ステートメントを含むメソッドが非構造化エラー処理を使用しない場合は、クリーンアップを確実にするために、`Finally` で `Dispose` メソッドを使用して `Try` ステートメントで `For Each` ステートメントをラップします。</span><span class="sxs-lookup"><span data-stu-id="eb747-554">If the method containing the `For Each` statement does not use unstructured error handling, then the `For Each` statement is wrapped in a `Try` statement with the `Dispose` method called in the `Finally` to ensure cleanup.</span></span>

<span data-ttu-id="eb747-555">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="eb747-555">__Note.__</span></span> <span data-ttu-id="eb747-556">`System.Array` 型はコレクション型であり、すべての配列型が `System.Array`から派生しているため、`For Each` ステートメントで任意の配列型の式を使用できます。</span><span class="sxs-lookup"><span data-stu-id="eb747-556">The `System.Array` type is a collection type, and since all array types derive from `System.Array`, any array type expression is permitted in a `For Each` statement.</span></span> <span data-ttu-id="eb747-557">1次元配列の場合、`For Each` のステートメントでは、インデックスの順序を大きくして配列の要素を列挙します。インデックスの順序は、インデックス0から始まり、インデックスの長さ-1 で終わります。</span><span class="sxs-lookup"><span data-stu-id="eb747-557">For single-dimensional arrays, the `For Each` statement enumerates the array elements in increasing index order, starting with index 0 and ending with index Length - 1.</span></span> <span data-ttu-id="eb747-558">多次元配列の場合は、右端の次元のインデックスが先に増加します。</span><span class="sxs-lookup"><span data-stu-id="eb747-558">For multidimensional arrays, the indices of the rightmost dimension are increased first.</span></span>

<span data-ttu-id="eb747-559">たとえば、次のコードでは `1 2 3 4`が出力されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-559">For example, the following code prints `1 2 3 4`:</span></span>

```vb
Module Test
    Sub Main()
        Dim x(,) As Integer = { { 1, 2 }, { 3, 4 } }
        Dim i As Integer

        For Each i In x
            Console.Write(i & " ")
        Next i
    End Sub
End Module
```

<span data-ttu-id="eb747-560">ブロックの外側から `For Each` ステートメントブロックに分岐することは無効です。</span><span class="sxs-lookup"><span data-stu-id="eb747-560">It is not valid to branch into a `For Each` statement block from outside the block.</span></span>


## <a name="exception-handling-statements"></a><span data-ttu-id="eb747-561">例外処理ステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-561">Exception-Handling Statements</span></span>

<span data-ttu-id="eb747-562">Visual Basic は、構造化例外処理と非構造化例外処理をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="eb747-562">Visual Basic supports structured exception handling and unstructured exception handling.</span></span> <span data-ttu-id="eb747-563">メソッドで使用できるのは1つのスタイルの例外処理のみですが、構造化例外処理では、`Error` ステートメントを使用できます。</span><span class="sxs-lookup"><span data-stu-id="eb747-563">Only one style of exception handling may be used in a method, but the `Error` statement may be used in structured exception handling.</span></span> <span data-ttu-id="eb747-564">メソッドが例外処理の両方のスタイルを使用する場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="eb747-564">If a method uses both styles of exception handling, a compile-time error results.</span></span>

```antlr
ErrorHandlingStatement
    : StructuredErrorStatement
    | UnstructuredErrorStatement
    ;
```

### <a name="structured-exception-handling-statements"></a><span data-ttu-id="eb747-565">構造化例外処理ステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-565">Structured Exception-Handling Statements</span></span>

<span data-ttu-id="eb747-566">構造化例外処理は、特定の例外が処理される明示的なブロックを宣言することでエラーを処理する方法です。</span><span class="sxs-lookup"><span data-stu-id="eb747-566">Structured exception handling is a method of handling errors by declaring explicit blocks within which certain exceptions will be handled.</span></span> <span data-ttu-id="eb747-567">構造化例外処理は、`Try` ステートメントを使用して行われます。</span><span class="sxs-lookup"><span data-stu-id="eb747-567">Structured exception handling is done through a `Try` statement.</span></span>

```antlr
StructuredErrorStatement
    : ThrowStatement
    | TryStatement
    ;

TryStatement
    : 'Try' StatementTerminator
      Block?
      CatchStatement*
      FinallyStatement?
      'End' 'Try' StatementTerminator
    ;
```


<span data-ttu-id="eb747-568">例 :</span><span class="sxs-lookup"><span data-stu-id="eb747-568">For example:</span></span>

```vb
Module Test
    Sub ThrowException()
        Throw New Exception()
    End Sub

    Sub Main()
        Try
            ThrowException()
        Catch e As Exception
            Console.WriteLine("Caught exception!")
        Finally
            Console.WriteLine("Exiting try.")
        End Try
    End Sub
End Module
```

<span data-ttu-id="eb747-569">`Try` ステートメントは、try ブロック、catch ブロック、および finally ブロックの3種類のブロックで構成されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-569">A `Try` statement is made up of three kinds of blocks: try blocks, catch blocks, and finally blocks.</span></span> <span data-ttu-id="eb747-570">*Try ブロック*は、実行するステートメントを含むステートメントブロックです。</span><span class="sxs-lookup"><span data-stu-id="eb747-570">A *try block* is a statement block that contains the statements to be executed.</span></span> <span data-ttu-id="eb747-571">*Catch ブロック*は、例外を処理するステートメントブロックです。</span><span class="sxs-lookup"><span data-stu-id="eb747-571">A *catch block* is a statement block that handles an exception.</span></span> <span data-ttu-id="eb747-572">*Finally ブロック*は、例外が発生して処理されたかどうかに関係なく、`Try` ステートメントが終了したときに実行されるステートメントを含むステートメントブロックです。</span><span class="sxs-lookup"><span data-stu-id="eb747-572">A *finally block* is a statement block that contains statements to be run when the `Try` statement is exited, regardless of whether an exception has occurred and been handled.</span></span> <span data-ttu-id="eb747-573">Try ブロックと finally ブロックを1つだけ含めることができる `Try` ステートメントには、少なくとも1つの catch ブロックまたは finally ブロックが含まれている必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-573">A `Try` statement, which can only contain one try block and one finally block, must contain at least one catch block or finally block.</span></span> <span data-ttu-id="eb747-574">同じステートメント内の catch ブロック内からを除き、try ブロックに明示的に実行を転送することは無効です。</span><span class="sxs-lookup"><span data-stu-id="eb747-574">It is invalid to explicitly transfer execution into a try block except from within a catch block in the same statement.</span></span>


#### <a name="finally-blocks"></a><span data-ttu-id="eb747-575">Finally ブロック</span><span class="sxs-lookup"><span data-stu-id="eb747-575">Finally Blocks</span></span>

<span data-ttu-id="eb747-576">`Finally` ブロックは、実行が `Try` ステートメントの任意の部分を離れると常に実行されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-576">A `Finally` block is always executed when execution leaves any part of the `Try` statement.</span></span> <span data-ttu-id="eb747-577">`Finally` ブロックを実行するために明示的な操作は必要ありません。実行時に `Try` ステートメントが実行されると、システムは自動的に `Finally` ブロックを実行し、実行を目的の宛先に転送します。</span><span class="sxs-lookup"><span data-stu-id="eb747-577">No explicit action is required to execute the `Finally` block; when execution leaves the `Try` statement, the system will automatically execute the `Finally` block and then transfer execution to its intended destination.</span></span> <span data-ttu-id="eb747-578">`Finally` ブロックは、実行によって `Try` ステートメントが終了する方法に関係なく実行されます。 `Try` `Catch` ブロックの最後まで、`Exit Try` ステートメントを使用して、またはスローされた例外を処理しないことによって、ステートメントを実行します。`GoTo`</span><span class="sxs-lookup"><span data-stu-id="eb747-578">The `Finally` block is executed regardless of how execution leaves the `Try` statement: through the end of the `Try` block, through the end of a `Catch` block, through an `Exit Try` statement, through a `GoTo` statement, or by not handling a thrown exception.</span></span>

<span data-ttu-id="eb747-579">Async メソッドの `Await` 式、および iterator メソッドの `Yield` ステートメントでは、async メソッドまたは iterator メソッドのインスタンスで制御のフローが中断され、他のメソッドインスタンスで再開される場合があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="eb747-579">Note that the `Await` expression in an async method, and the `Yield` statement in an iterator method, can cause flow of control to suspend in the async or iterator method instance and resume in some other method instance.</span></span> <span data-ttu-id="eb747-580">ただし、これは実行の中断にすぎず、それぞれの非同期メソッドまたは反復子メソッドのインスタンスを終了する必要がないため、`Finally` ブロックが実行されません。</span><span class="sxs-lookup"><span data-stu-id="eb747-580">However, this is merely a suspension of execution and does not involve exiting the respective async method or iterator method instance, and so does not cause `Finally` blocks to be executed.</span></span>

<span data-ttu-id="eb747-581">実行を明示的に `Finally` ブロックに転送するのは無効です。また、例外を除き、`Finally` ブロックの外で実行を転送することは無効です。</span><span class="sxs-lookup"><span data-stu-id="eb747-581">It is invalid to explicitly transfer execution into a `Finally` block; it is also invalid to transfer execution out of a `Finally` block except through an exception.</span></span>

```antlr
FinallyStatement
    : 'Finally' StatementTerminator
      Block?
    ;
```

#### <a name="catch-blocks"></a><span data-ttu-id="eb747-582">catch ブロック</span><span class="sxs-lookup"><span data-stu-id="eb747-582">Catch Blocks</span></span>

<span data-ttu-id="eb747-583">`Try` ブロックの処理中に例外が発生した場合、各 `Catch` ステートメントは、例外を処理するかどうかを判断するためにテキスト順に検査されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-583">If an exception occurs while processing the `Try` block, each `Catch` statement is examined in textual order to determine if it handles the exception.</span></span>

```antlr
CatchStatement
    : 'Catch' ( Identifier ( 'As' NonArrayTypeName )? )?
      ( 'When' BooleanExpression )? StatementTerminator
      Block?
    ;
```

<span data-ttu-id="eb747-584">`Catch` 句で指定された識別子は、スローされた例外を表します。</span><span class="sxs-lookup"><span data-stu-id="eb747-584">The identifier specified in a `Catch` clause represents the exception that has been thrown.</span></span> <span data-ttu-id="eb747-585">識別子に `As` 句が含まれている場合、識別子は、`Catch` ブロックのローカル宣言領域内で宣言されていると見なされます。</span><span class="sxs-lookup"><span data-stu-id="eb747-585">If the identifier contains an `As` clause, then the identifier is considered to be declared within the `Catch` block's local declaration space.</span></span> <span data-ttu-id="eb747-586">それ以外の場合、識別子は、含んでいるブロックで定義されたローカル変数 (静的変数ではない) である必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-586">Otherwise, the identifier must be a local variable (not a static variable) that was defined in a containing block.</span></span>

<span data-ttu-id="eb747-587">識別子のない `Catch` 句は、`System.Exception`から派生したすべての例外をキャッチします。</span><span class="sxs-lookup"><span data-stu-id="eb747-587">A `Catch` clause with no identifier will catch all exceptions derived from `System.Exception`.</span></span> <span data-ttu-id="eb747-588">識別子を持つ `Catch` 句でキャッチされるのは、識別子の型と同じか、またはその型から派生した型を持つ例外だけです。</span><span class="sxs-lookup"><span data-stu-id="eb747-588">A `Catch` clause with an identifier will only catch exceptions whose types are the same as or derived from the type of the identifier.</span></span> <span data-ttu-id="eb747-589">型は `System.Exception`であるか、または `System.Exception`から派生した型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-589">The type must be `System.Exception`, or a type derived from `System.Exception`.</span></span> <span data-ttu-id="eb747-590">`System.Exception`から派生した例外がキャッチされると、例外オブジェクトへの参照は、関数 `Microsoft.VisualBasic.Information.Err`によって返されるオブジェクトに格納されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-590">When an exception is caught that derives from `System.Exception`, a reference to the exception object is stored in the object returned by the function `Microsoft.VisualBasic.Information.Err`.</span></span>

<span data-ttu-id="eb747-591">`When` 句を含む `Catch` 句は、式が `True`に評価された場合にのみ例外をキャッチします。式の型は、項[ブール式](expressions.md#boolean-expressions)に従ってブール式である必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-591">A `Catch` clause with a `When` clause will only catch exceptions when the expression evaluates to `True`; the type of the expression must be a Boolean expression as per Section [Boolean Expressions](expressions.md#boolean-expressions).</span></span> <span data-ttu-id="eb747-592">`When` 句は、例外の型をチェックした後にのみ適用されます。この例で示すように、式は例外を表す識別子を参照する場合があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-592">A `When` clause is only applied after checking the type of the exception, and the expression may refer to the identifier representing the exception, as this example demonstrates:</span></span>

```vb
Module Test
    Sub Main()
        Dim i As Integer = 5

        Try
            Throw New ArgumentException()
        Catch e As OverflowException When i = 5
            Console.WriteLine("First handler")
        Catch e As ArgumentException When i = 4
            Console.WriteLine("Second handler")
        Catch When i = 5
            Console.WriteLine("Third handler")
        End Try

    End Sub
End Module
```

<span data-ttu-id="eb747-593">次の例では、が出力されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-593">This example prints:</span></span>

```console
Third handler
```

<span data-ttu-id="eb747-594">`Catch` 句が例外を処理する場合、実行は `Catch` ブロックに転送されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-594">If a `Catch` clause handles the exception, execution transfers to the `Catch` block.</span></span> <span data-ttu-id="eb747-595">`Catch` ブロックの最後で、実行は `Try` ステートメントに続く最初のステートメントに転送されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-595">At the end of the `Catch` block, execution transfers to the first statement following the `Try` statement.</span></span> <span data-ttu-id="eb747-596">`Try` ステートメントは、`Catch` ブロックでスローされた例外を処理しません。</span><span class="sxs-lookup"><span data-stu-id="eb747-596">The `Try` statement will not handle any exceptions thrown in a `Catch` block.</span></span> <span data-ttu-id="eb747-597">`Catch` 句が例外を処理しない場合、実行はシステムによって決定された場所に転送されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-597">If no `Catch` clause handles the exception, execution transfers to a location determined by the system.</span></span>

<span data-ttu-id="eb747-598">実行を明示的に `Catch` ブロックに転送することは無効です。</span><span class="sxs-lookup"><span data-stu-id="eb747-598">It is invalid to explicitly transfer execution into a `Catch` block.</span></span>

<span data-ttu-id="eb747-599">When 句のフィルターは、通常、例外がスローされる前に評価されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-599">The filters in When clauses are normally evaluated prior to the exception being thrown.</span></span> <span data-ttu-id="eb747-600">たとえば、次のコードでは、"Filter, Finally, Catch" が出力されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-600">For instance, the following code will print "Filter, Finally, Catch".</span></span>

```vb
Sub Main()
   Try
       Foo()
   Catch ex As Exception When F()
       Console.WriteLine("Catch")
   End Try
End Sub

Sub Foo()
    Try
        Throw New Exception
    Finally
        Console.WriteLine("Finally")
    End Try
End Sub

Function F() As Boolean
    Console.WriteLine("Filter")
    Return True
End Function
```

 <span data-ttu-id="eb747-601">ただし、Async メソッドと Iterator メソッドでは、の内部のすべてのブロックが、以外のフィルターの前に実行されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-601">However, Async and Iterator methods cause all finally blocks inside them to be executed prior to any filters outside.</span></span> <span data-ttu-id="eb747-602">たとえば、上記のコードが `Async Sub Foo()`た場合、出力は "Finally, Filter, Catch" になります。</span><span class="sxs-lookup"><span data-stu-id="eb747-602">For instance, if the above code had `Async Sub Foo()`, then the output would be "Finally, Filter, Catch".</span></span>


#### <a name="throw-statement"></a><span data-ttu-id="eb747-603">Throw ステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-603">Throw Statement</span></span>

<span data-ttu-id="eb747-604">`Throw` ステートメントは、`System.Exception`から派生した型のインスタンスによって表される例外を発生させます。</span><span class="sxs-lookup"><span data-stu-id="eb747-604">The `Throw` statement raises an exception, which is represented by an instance of a type derived from `System.Exception`.</span></span>

```antlr
ThrowStatement
    : 'Throw' Expression? StatementTerminator
    ;
```

<span data-ttu-id="eb747-605">式が値として分類されていない場合、または `System.Exception`から派生した型ではない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="eb747-605">If the expression is not classified as a value or is not a type derived from `System.Exception`, then a compile-time error occurs.</span></span> <span data-ttu-id="eb747-606">実行時に式が null 値に評価された場合は、代わりに `System.NullReferenceException` 例外が発生します。</span><span class="sxs-lookup"><span data-stu-id="eb747-606">If the expression evaluates to a null value at run time, then a `System.NullReferenceException` exception is raised instead.</span></span>

<span data-ttu-id="eb747-607">`Throw` ステートメントは、finally ブロックが介在しない限り、`Try` ステートメントの catch ブロック内の式を省略できます。</span><span class="sxs-lookup"><span data-stu-id="eb747-607">A `Throw` statement may omit the expression within a catch block of a `Try` statement, as long as there is no intervening finally block.</span></span> <span data-ttu-id="eb747-608">その場合、ステートメントは、catch ブロック内で現在処理されている例外を再スローします。</span><span class="sxs-lookup"><span data-stu-id="eb747-608">In that case, the statement rethrows the exception currently being handled within the catch block.</span></span> <span data-ttu-id="eb747-609">例 :</span><span class="sxs-lookup"><span data-stu-id="eb747-609">For example:</span></span>

```vb
Sub Test(x As Integer)
    Try
        Throw New Exception()
    Catch
        If x = 0 Then
            Throw    ' OK, rethrows exception from above.
        Else
            Try
                If x = 1 Then
                    Throw   ' OK, rethrows exception from above.
                End If
            Finally
                Throw    ' Invalid, inside of a Finally.
            End Try
        End If
    End Try
End Sub
```


### <a name="unstructured-exception-handling-statements"></a><span data-ttu-id="eb747-610">非構造化例外処理ステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-610">Unstructured Exception-Handling Statements</span></span>

<span data-ttu-id="eb747-611">非構造化例外処理は、例外が発生したときに分岐するステートメントを示すことによってエラーを処理する方法です。</span><span class="sxs-lookup"><span data-stu-id="eb747-611">Unstructured exception handling is a method of handling errors by indicating statements to branch to when an exception occurs.</span></span> <span data-ttu-id="eb747-612">非構造化例外処理は、`Error` ステートメント、`On Error` ステートメント、および `Resume` ステートメントの3つのステートメントを使用して実装されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-612">Unstructured exception handling is implemented using three statements: the `Error` statement, the `On Error` statement, and the `Resume` statement.</span></span>

```antlr
UnstructuredErrorStatement
    : ErrorStatement
    | OnErrorStatement
    | ResumeStatement
    ;
```

<span data-ttu-id="eb747-613">例 :</span><span class="sxs-lookup"><span data-stu-id="eb747-613">For example:</span></span>

```vb
Module Test
    Sub ThrowException()
        Error 5
    End Sub

    Sub Main()
        On Error GoTo GotException

        ThrowException()
        Exit Sub

GotException:
        Console.WriteLine("Caught exception!")
        Resume Next
    End Sub
End Module
```

<span data-ttu-id="eb747-614">メソッドが非構造化例外処理を使用する場合、すべての例外をキャッチするメソッド全体に対して1つの構造化例外ハンドラーが確立されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-614">When a method uses unstructured exception handling, a single structured exception handler is established for the entire method that catches all exceptions.</span></span> <span data-ttu-id="eb747-615">(コンストラクターでは、このハンドラーは、コンストラクターの先頭で `New` への呼び出しに対しては拡張されないことに注意してください)。メソッドは、最後に発生した例外ハンドラーの場所と、スローされた最新の例外を追跡します。</span><span class="sxs-lookup"><span data-stu-id="eb747-615">(Note that in constructors this handler does not extend over the call to the call to `New` at the beginning of the constructor.) The method then keeps track of the most recent exception-handler location and the most recent exception that has been thrown.</span></span> <span data-ttu-id="eb747-616">メソッドへの入力時には、例外ハンドラーの場所と例外の両方が `Nothing`に設定されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-616">At entry to the method, the exception-handler location and the exception are both set to `Nothing`.</span></span> <span data-ttu-id="eb747-617">非構造化例外処理を使用するメソッドで例外がスローされると、例外オブジェクトへの参照は、関数 `Microsoft.VisualBasic.Information.Err`によって返されるオブジェクトに格納されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-617">When an exception is thrown in a method that uses unstructured exception handling, a reference to the exception object is stored in the object returned by the function `Microsoft.VisualBasic.Information.Err`.</span></span>

<span data-ttu-id="eb747-618">非構造化エラー処理ステートメントは、反復子または非同期メソッドでは使用できません。</span><span class="sxs-lookup"><span data-stu-id="eb747-618">Unstructured error handling statements are not allowed in iterator or async methods.</span></span>


#### <a name="error-statement"></a><span data-ttu-id="eb747-619">Error ステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-619">Error Statement</span></span>

<span data-ttu-id="eb747-620">`Error` ステートメントは、Visual Basic 6 個の例外番号を含む `System.Exception` 例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="eb747-620">An `Error` statement throws a `System.Exception` exception containing a Visual Basic 6 exception number.</span></span> <span data-ttu-id="eb747-621">式は値として分類する必要があり、その型は `Integer`に暗黙的に変換可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-621">The expression must be classified as a value and its type must be implicitly convertible to `Integer`.</span></span>

```antlr
ErrorStatement
    : 'Error' Expression StatementTerminator
    ;
```

#### <a name="on-error-statement"></a><span data-ttu-id="eb747-622">On Error ステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-622">On Error Statement</span></span>

<span data-ttu-id="eb747-623">`On Error` ステートメントは、最新の例外処理状態を変更します。</span><span class="sxs-lookup"><span data-stu-id="eb747-623">An `On Error` statement modifies the most recent exception-handling state.</span></span>

```antlr
OnErrorStatement
    : 'On' 'Error' ErrorClause StatementTerminator
    ;

ErrorClause
    : 'GoTo' '-' '1'
    | 'GoTo' '0'
    | GoToStatement
    | 'Resume' 'Next'
    ;
```

<span data-ttu-id="eb747-624">次の4つの方法のいずれかで使用できます。</span><span class="sxs-lookup"><span data-stu-id="eb747-624">It may be used in one of four ways:</span></span>

* <span data-ttu-id="eb747-625">`On Error GoTo -1` により、最新の例外が `Nothing`にリセットされます。</span><span class="sxs-lookup"><span data-stu-id="eb747-625">`On Error GoTo -1` resets the most recent exception to `Nothing`.</span></span>

* <span data-ttu-id="eb747-626">`On Error GoTo 0` により、最新の例外ハンドラーの場所が `Nothing`にリセットされます。</span><span class="sxs-lookup"><span data-stu-id="eb747-626">`On Error GoTo 0` resets the most recent exception-handler location to `Nothing`.</span></span>

* <span data-ttu-id="eb747-627">`On Error GoTo LabelName` は、最新の例外ハンドラーの場所としてラベルを設定します。</span><span class="sxs-lookup"><span data-stu-id="eb747-627">`On Error GoTo LabelName` establishes the label as the most recent exception-handler location.</span></span> <span data-ttu-id="eb747-628">このステートメントは、ラムダ式またはクエリ式を含むメソッドでは使用できません。</span><span class="sxs-lookup"><span data-stu-id="eb747-628">This statement cannot be used in a method that contains a lambda or query expression.</span></span>

* <span data-ttu-id="eb747-629">`On Error Resume Next` は、`Resume Next` の動作を最新の例外ハンドラーの場所として設定します。</span><span class="sxs-lookup"><span data-stu-id="eb747-629">`On Error Resume Next` establishes the `Resume Next` behavior as the most recent exception-handler location.</span></span>


#### <a name="resume-statement"></a><span data-ttu-id="eb747-630">Resume ステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-630">Resume Statement</span></span>

<span data-ttu-id="eb747-631">`Resume` ステートメントは、最後の例外の原因となったステートメントに実行を返します。</span><span class="sxs-lookup"><span data-stu-id="eb747-631">A `Resume` statement returns execution to the statement that caused the most recent exception.</span></span>

```antlr
ResumeStatement
    : 'Resume' ResumeClause? StatementTerminator
    ;

ResumeClause
    : 'Next'
    | LabelName
    ;
```

<span data-ttu-id="eb747-632">`Next` 修飾子が指定されている場合、実行は、最新の例外の原因となったステートメントの後に実行されたステートメントに戻ります。</span><span class="sxs-lookup"><span data-stu-id="eb747-632">If the `Next` modifier is specified, execution returns to the statement that would have been executed after the statement that caused the most recent exception.</span></span> <span data-ttu-id="eb747-633">ラベル名が指定されている場合、実行はラベルに戻ります。</span><span class="sxs-lookup"><span data-stu-id="eb747-633">If a label name is specified, execution returns to the label.</span></span>

<span data-ttu-id="eb747-634">`SyncLock` ステートメントには暗黙的な構造化エラー処理ブロックが含まれているので、`Resume` と `Resume Next` には `SyncLock` ステートメントで発生する例外に対する特別な動作があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-634">Because the `SyncLock` statement contains an implicit structured error-handling block, `Resume` and `Resume Next` have special behaviors for exceptions that occur in `SyncLock` statements.</span></span> <span data-ttu-id="eb747-635">`Resume` は `SyncLock` ステートメントの先頭に実行を返し、`Resume Next` は `SyncLock` ステートメントに続く次のステートメントに実行を返します。</span><span class="sxs-lookup"><span data-stu-id="eb747-635">`Resume` returns execution to the beginning of the `SyncLock` statement, while `Resume Next` returns execution to the next statement following the `SyncLock` statement.</span></span> <span data-ttu-id="eb747-636">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="eb747-636">For example, consider the following code:</span></span>

```vb
Class LockClass
End Class

Module Test
    Sub Main()
        Dim FirstTime As Boolean = True
        Dim Lock As LockClass = New LockClass()

        On Error GoTo Handler

        SyncLock Lock
            Console.WriteLine("Before exception")
            Throw New Exception()
            Console.WriteLine("After exception")
        End SyncLock

        Console.WriteLine("After SyncLock")
        Exit Sub

Handler:
        If FirstTime Then
            FirstTime = False
            Resume
        Else
            Resume Next
        End If
    End Sub
End Module
```

<span data-ttu-id="eb747-637">次の結果が出力されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-637">It prints the following result.</span></span>

```console
Before exception
Before exception
After SyncLock
```

<span data-ttu-id="eb747-638">最初に `SyncLock` ステートメントを実行すると、`Resume` は `SyncLock` ステートメントの先頭に実行を返します。</span><span class="sxs-lookup"><span data-stu-id="eb747-638">The first time through the `SyncLock` statement, `Resume` returns execution to the beginning of the `SyncLock` statement.</span></span> <span data-ttu-id="eb747-639">2回目に `SyncLock` ステートメントを実行すると、`Resume Next` は `SyncLock` ステートメントの最後まで実行を戻します。</span><span class="sxs-lookup"><span data-stu-id="eb747-639">The second time through the `SyncLock` statement, `Resume Next` returns execution to the end of the `SyncLock` statement.</span></span> <span data-ttu-id="eb747-640">`Resume` と `Resume Next` は `SyncLock` ステートメント内では許可されていません。</span><span class="sxs-lookup"><span data-stu-id="eb747-640">`Resume` and `Resume Next` are not allowed within a `SyncLock` statement.</span></span>

<span data-ttu-id="eb747-641">どのような場合でも、`Resume` ステートメントを実行すると、最新の例外が `Nothing`に設定されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-641">In all cases, when a `Resume` statement is executed, the most recent exception is set to `Nothing`.</span></span> <span data-ttu-id="eb747-642">最新の例外がない状態で `Resume` ステートメントを実行すると、ステートメントは Visual Basic エラー番号 `20` (エラーなしで再開) を含む `System.Exception` 例外を発生させます。</span><span class="sxs-lookup"><span data-stu-id="eb747-642">If a `Resume` statement is executed with no most recent exception, the statement raises a `System.Exception` exception containing the Visual Basic error number `20` (Resume without error).</span></span>


## <a name="branch-statements"></a><span data-ttu-id="eb747-643">Branch ステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-643">Branch Statements</span></span>

<span data-ttu-id="eb747-644">分岐ステートメントは、メソッドの実行フローを変更します。</span><span class="sxs-lookup"><span data-stu-id="eb747-644">Branch statements modify the flow of execution in a method.</span></span> <span data-ttu-id="eb747-645">次の6つの分岐ステートメントがあります。</span><span class="sxs-lookup"><span data-stu-id="eb747-645">There are six branch statements:</span></span>

1. <span data-ttu-id="eb747-646">`GoTo` ステートメントを実行すると、メソッド内の指定したラベルに実行が転送されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-646">A `GoTo` statement causes execution to transfer to the specified label in the method.</span></span> <span data-ttu-id="eb747-647">このブロックのローカル変数がラムダ式または LINQ 式でキャプチャされている場合、`Try`、`Using`、`SyncLock`、`With`、`For`、または `For Each` ブロックに `GoTo` することはできません。</span><span class="sxs-lookup"><span data-stu-id="eb747-647">It is not allowed to `GoTo` into a `Try`, `Using`, `SyncLock`, `With`, `For` or `For Each` block, nor into any loop block if a local variable of that block is captured in a lambda or LINQ expression.</span></span>
2. <span data-ttu-id="eb747-648">`Exit` ステートメントは、指定された種類のブロックステートメントの直後にあるステートメントの最後の次のステートメントに実行を転送します。</span><span class="sxs-lookup"><span data-stu-id="eb747-648">An `Exit` statement transfers execution to the next statement after the end of the immediately containing block statement of the specified kind.</span></span> <span data-ttu-id="eb747-649">ブロックがメソッドブロックの場合、制御フローは「[制御フロー](statements.md#control-flow)」で説明されているようにメソッドを終了します。</span><span class="sxs-lookup"><span data-stu-id="eb747-649">If the block is the method block, then control flow exits the method as described in Section [Control Flow](statements.md#control-flow).</span></span> <span data-ttu-id="eb747-650">ステートメントで指定されたブロックの種類に `Exit` ステートメントが含まれていない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="eb747-650">If the `Exit` statement is not contained within the kind of block specified in the statement, a compile-time error occurs.</span></span>
3. <span data-ttu-id="eb747-651">`Continue` ステートメントは、指定された種類のブロックループステートメントの最後まで実行を転送します。</span><span class="sxs-lookup"><span data-stu-id="eb747-651">A `Continue` statement transfers execution to the end of the immediately containing block loop statement of the specified kind.</span></span> <span data-ttu-id="eb747-652">ステートメントで指定されたブロックの種類に `Continue` ステートメントが含まれていない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="eb747-652">If the `Continue` statement is not contained within the kind of block specified in the statement, a compile-time error occurs.</span></span>
4. <span data-ttu-id="eb747-653">`Stop` ステートメントを実行すると、デバッガーの例外が発生します。</span><span class="sxs-lookup"><span data-stu-id="eb747-653">A `Stop` statement causes a debugger exception to occur.</span></span>
5. <span data-ttu-id="eb747-654">`End` ステートメントは、プログラムを終了します。</span><span class="sxs-lookup"><span data-stu-id="eb747-654">An `End` statement terminates the program.</span></span> <span data-ttu-id="eb747-655">ファイナライザーはシャットダウンの前に実行されますが、現在実行中の `Try` ステートメントの最後のブロックは実行されません。</span><span class="sxs-lookup"><span data-stu-id="eb747-655">Finalizers are run before shutdown, but the finally blocks of any currently executing `Try` statements are not executed.</span></span> <span data-ttu-id="eb747-656">このステートメントは、実行可能ではないプログラム (Dll など) では使用できません。</span><span class="sxs-lookup"><span data-stu-id="eb747-656">This statement may not be used in programs that are not executable (for example, DLLs).</span></span>
6. <span data-ttu-id="eb747-657">式が指定されていない `Return` ステートメントは、`Exit Sub` または `Exit Function` ステートメントと同じです。</span><span class="sxs-lookup"><span data-stu-id="eb747-657">A `Return` statement with no expression is equivalent to an `Exit Sub` or `Exit Function` statement.</span></span> <span data-ttu-id="eb747-658">式を含む `Return` ステートメントは、関数である通常のメソッド、または一部の `T`の戻り値の型 `Task(Of T)` を持つ関数である非同期メソッドでのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="eb747-658">A `Return` statement with an expression is only allowed in a regular method that is a function, or in an async method that is a function with return type `Task(Of T)` for some `T`.</span></span> <span data-ttu-id="eb747-659">式は、*関数の戻り変数*(通常のメソッドの場合) または*タスクの戻り変数*(非同期メソッドの場合) に暗黙的に変換できる値として分類する必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-659">The expression must be classified as a value which is implicitly convertible to the *function return variable* (in the case of regular methods) or to the *task return variable* (in the case of async methods).</span></span> <span data-ttu-id="eb747-660">その動作は、式を評価し、戻り変数に格納した後、暗黙的な `Exit Function` ステートメントを実行することです。</span><span class="sxs-lookup"><span data-stu-id="eb747-660">Its behavior is to evaluate its expression, then store it in the return variable, then execute an implicit `Exit Function` statement.</span></span>

```antlr
BranchStatement
    : GoToStatement
    | ExitStatement
    | ContinueStatement
    | StopStatement
    | EndStatement
    | ReturnStatement
    ;

GoToStatement
    : 'GoTo' LabelName StatementTerminator
    ;

ExitStatement
    : 'Exit' ExitKind StatementTerminator
    ;

ExitKind
    : 'Do' | 'For' | 'While' | 'Select' | 'Sub' | 'Function' | 'Property' | 'Try'
    ;

ContinueStatement
    : 'Continue' ContinueKind StatementTerminator
    ;

ContinueKind
    : 'Do' | 'For' | 'While'
    ;

StopStatement
    : 'Stop' StatementTerminator
    ;

EndStatement
    : 'End' StatementTerminator
    ;

ReturnStatement
    : 'Return' Expression? StatementTerminator
    ;
```

## <a name="array-handling-statements"></a><span data-ttu-id="eb747-661">配列処理ステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-661">Array-Handling Statements</span></span>

<span data-ttu-id="eb747-662">2つのステートメントでは、配列の操作が簡単になります。 `ReDim` ステートメントと `Erase` ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="eb747-662">Two statements simplify working with arrays: `ReDim` statements and `Erase` statements.</span></span>

```antlr
ArrayHandlingStatement
    : RedimStatement
    | EraseStatement
    ;
```

### <a name="redim-statement"></a><span data-ttu-id="eb747-663">ReDim ステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-663">ReDim Statement</span></span>

<span data-ttu-id="eb747-664">`ReDim` ステートメントは、新しい配列をインスタンス化します。</span><span class="sxs-lookup"><span data-stu-id="eb747-664">A `ReDim` statement instantiates new arrays.</span></span>

```antlr
RedimStatement
    : 'ReDim' 'Preserve'? RedimClauses StatementTerminator
    ;

RedimClauses
    : RedimClause ( Comma RedimClause )*
    ;

RedimClause
    : Expression ArraySizeInitializationModifier
    ;
```

<span data-ttu-id="eb747-665">ステートメント内の各句は、変数またはプロパティアクセスとして分類される必要があります。型は配列型または `Object`であり、その後に配列の範囲のリストが続きます。</span><span class="sxs-lookup"><span data-stu-id="eb747-665">Each clause in the statement must be classified as a variable or a property access whose type is an array type or `Object`, and be followed by a list of array bounds.</span></span> <span data-ttu-id="eb747-666">境界の数は、変数の型と一致している必要があります。`Object`には任意の数の境界を指定できます。</span><span class="sxs-lookup"><span data-stu-id="eb747-666">The number of the bounds must be consistent with the type of the variable; any number of bounds is allowed for `Object`.</span></span> <span data-ttu-id="eb747-667">実行時に、配列は、式ごとに指定された境界を使用して左から右にインスタンス化され、変数またはプロパティに割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="eb747-667">At run time, an array is instantiated for each expression from left to right with the specified bounds and then assigned to the variable or property.</span></span> <span data-ttu-id="eb747-668">変数の型が `Object`の場合、次元の数は指定された次元の数になり、配列要素の型は `Object`になります。</span><span class="sxs-lookup"><span data-stu-id="eb747-668">If the variable type is `Object`, the number of dimensions is the number of dimensions specified, and the array element type is `Object`.</span></span> <span data-ttu-id="eb747-669">指定された次元数が実行時に変数またはプロパティと互換性がない場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="eb747-669">If the given number of dimensions is incompatible with the variable or property at run time a compile-time error occurs.</span></span> <span data-ttu-id="eb747-670">例 :</span><span class="sxs-lookup"><span data-stu-id="eb747-670">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim o As Object
        Dim b() As Byte
        Dim i(,) As Integer

        ' The next two statements are equivalent.
        ReDim o(10,30)
        o = New Object(10, 30) {}

        ' The next two statements are equivalent.
        ReDim b(10)
        b = New Byte(10) {}

        ' Error: Incorrect number of dimensions.
        ReDim i(10, 30, 40)
    End Sub
End Module
```

<span data-ttu-id="eb747-671">`Preserve` キーワードを指定する場合は、式も値として classifiable する必要があります。また、右端の次元以外の各次元の新しいサイズは、既存の配列のサイズと同じである必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-671">If the `Preserve` keyword is specified, then the expressions must also be classifiable as a value, and the new size for each dimension except for the rightmost one must be the same as the size of the existing array.</span></span> <span data-ttu-id="eb747-672">既存の配列内の値が新しい配列にコピーされます。新しい配列が小さい場合、既存の値は破棄されます。新しい配列が大きい場合、追加の要素は配列の要素型の既定値に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-672">The values in the existing array are copied into the new array: if the new array is smaller, the existing values are discarded; if the new array is bigger, the extra elements will be initialized to the default value of the element type of the array.</span></span> <span data-ttu-id="eb747-673">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="eb747-673">For example, consider the following code:</span></span>

```vb
Module Test
    Sub Main()
        Dim x(5, 5) As Integer

        x(3, 3) = 3

        ReDim Preserve x(5, 6)
        Console.WriteLine(x(3, 3) & ", " & x(3, 6))
    End Sub
End Module
```

<span data-ttu-id="eb747-674">次の結果が出力されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-674">It prints the following result:</span></span>

```console
3, 0
```

<span data-ttu-id="eb747-675">実行時に既存の配列参照が null 値の場合、エラーは発生しません。</span><span class="sxs-lookup"><span data-stu-id="eb747-675">If the existing array reference is a null value at run time, no error is given.</span></span> <span data-ttu-id="eb747-676">右端のディメンション以外は、ディメンションのサイズが変更されると `System.ArrayTypeMismatchException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="eb747-676">Other than the rightmost dimension, if the size of a dimension changes, a `System.ArrayTypeMismatchException` will be thrown.</span></span>

<span data-ttu-id="eb747-677">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="eb747-677">__Note.__</span></span> <span data-ttu-id="eb747-678">`Preserve` は予約語ではありません。</span><span class="sxs-lookup"><span data-stu-id="eb747-678">`Preserve` is not a reserved word.</span></span>


### <a name="erase-statement"></a><span data-ttu-id="eb747-679">Erase ステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-679">Erase Statement</span></span>

<span data-ttu-id="eb747-680">`Erase` ステートメントは、ステートメントで指定された各配列変数またはプロパティを `Nothing`に設定します。</span><span class="sxs-lookup"><span data-stu-id="eb747-680">An `Erase` statement sets each of the array variables or properties specified in the statement to `Nothing`.</span></span> <span data-ttu-id="eb747-681">ステートメント内の各式は、型が配列型または `Object`である変数またはプロパティアクセスとして分類される必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-681">Each expression in the statement must be classified as a variable or property access whose type is an array type or `Object`.</span></span> <span data-ttu-id="eb747-682">例 :</span><span class="sxs-lookup"><span data-stu-id="eb747-682">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim x() As Integer = New Integer(5) {}

        ' The following two statements are equivalent.
        Erase x
        x = Nothing
    End Sub
End Module
```

```antlr
EraseStatement
    : 'Erase' EraseExpressions StatementTerminator
    ;

EraseExpressions
    : Expression ( Comma Expression )*
    ;
```

## <a name="using-statement"></a><span data-ttu-id="eb747-683">Using ステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-683">Using statement</span></span>

<span data-ttu-id="eb747-684">コレクションが実行され、インスタンスへのライブ参照が見つからない場合、型のインスタンスはガベージコレクターによって自動的に解放されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-684">Instances of types are automatically released by the garbage collector when a collection is run and no live references to the instance are found.</span></span> <span data-ttu-id="eb747-685">型が特に貴重なリソース (データベース接続やファイルハンドルなど) を保持している場合は、次のガベージコレクションが使用されなくなった型の特定のインスタンスをクリーンアップするまで待機することは望ましくありません。</span><span class="sxs-lookup"><span data-stu-id="eb747-685">If a type holds on a particularly valuable and scarce resource (such as database connections or file handles), it may not be desirable to wait until the next garbage collection to clean up a particular instance of the type that is no longer in use.</span></span> <span data-ttu-id="eb747-686">コレクションの前にリソースを解放するための軽量な方法を提供するために、型は `System.IDisposable` インターフェイスを実装する場合があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-686">To provide a lightweight way of releasing resources before a collection, a type may implement the `System.IDisposable` interface.</span></span> <span data-ttu-id="eb747-687">これを行う型は、重要なリソースを直ちに解放するために呼び出すことができる `Dispose` メソッドを公開します。次にその例を示します。</span><span class="sxs-lookup"><span data-stu-id="eb747-687">A type that does so exposes a `Dispose` method that can be called to force valuable resources to be released immediately, as such:</span></span>

```vb
Module Test
    Sub Main()
        Dim x As DBConnection = New DBConnection("...")

        ' Do some work
        ...

        x.Dispose()        ' Free the connection
    End Sub
End Module
```

<span data-ttu-id="eb747-688">`Using` ステートメントは、リソースを取得し、一連のステートメントを実行して、リソースを破棄するプロセスを自動化します。</span><span class="sxs-lookup"><span data-stu-id="eb747-688">The `Using` statement automates the process of acquiring a resource, executing a set of statements, and then disposing of the resource.</span></span> <span data-ttu-id="eb747-689">このステートメントは2つの形式を取ることができます。1つは、リソースがステートメントの一部として宣言され、通常のローカル変数宣言ステートメントとして扱われるローカル変数です。もう1つのリソースは、式の結果です。</span><span class="sxs-lookup"><span data-stu-id="eb747-689">The statement can take two forms: in one, the resource is a local variable declared as a part of the statement and treated as a regular local variable declaration statement; in the other, the resource is the result of an expression.</span></span>

```antlr
UsingStatement
    : 'Using' UsingResources StatementTerminator
      Block?
      'End' 'Using' StatementTerminator
    ;

UsingResources
    : VariableDeclarators
    | Expression
    ;
```

<span data-ttu-id="eb747-690">リソースがローカル変数宣言ステートメントである場合、ローカル変数宣言の型は、暗黙的に `System.IDisposable`に変換できる型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-690">If the resource is a local variable declaration statement then the type of the local variable declaration must be a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="eb747-691">宣言されたローカル変数は読み取り専用であり、`Using` ステートメントブロックを対象としており、初期化子を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-691">The declared local variables are read-only, scoped to the `Using` statement block and must include an initializer.</span></span> <span data-ttu-id="eb747-692">リソースが式の結果である場合、式は値として分類される必要があり、`System.IDisposable`に暗黙的に変換できる型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-692">If the resource is the result of an expression then the expression must be classified as a value and must be of a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="eb747-693">式は、ステートメントの先頭で1回だけ評価されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-693">The expression is only evaluated once, at the beginning of the statement.</span></span>

<span data-ttu-id="eb747-694">`Using` ブロックは、finally ブロックがリソースのメソッド `IDisposable.Dispose` を呼び出す `Try` ステートメントによって暗黙的に含まれています。</span><span class="sxs-lookup"><span data-stu-id="eb747-694">The `Using` block is implicitly contained by a `Try` statement whose finally block calls the method `IDisposable.Dispose` on the resource.</span></span> <span data-ttu-id="eb747-695">これにより、例外がスローされた場合でも、リソースが確実に破棄されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-695">This ensures the resource is disposed even when an exception is thrown.</span></span> <span data-ttu-id="eb747-696">その結果、ブロックの外部から `Using` ブロックに分岐することは無効になり、`Using` ブロックは `Resume` と `Resume Next`のために1つのステートメントとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="eb747-696">As a result, it is invalid to branch into a `Using` block from outside of the block, and a `Using` block is treated as a single statement for the purposes of `Resume` and `Resume Next`.</span></span> <span data-ttu-id="eb747-697">リソースが `Nothing`場合、`Dispose` への呼び出しは行われません。</span><span class="sxs-lookup"><span data-stu-id="eb747-697">If the resource is `Nothing`, then no call to `Dispose` is made.</span></span> <span data-ttu-id="eb747-698">そのため、例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="eb747-698">Thus, the example:</span></span>

```vb
Using f As C = New C()
    ...
End Using
```

<span data-ttu-id="eb747-699">上の式は、下の式と同等です。</span><span class="sxs-lookup"><span data-stu-id="eb747-699">is equivalent to:</span></span>

```vb
Dim f As C = New C()
Try
    ...
Finally
    If f IsNot Nothing Then
        f.Dispose()
    End If
End Try
```

<span data-ttu-id="eb747-700">ローカル変数宣言ステートメントを持つ `Using` ステートメントは、同時に複数のリソースを取得できます。これは、入れ子になった `Using` ステートメントと同じです。</span><span class="sxs-lookup"><span data-stu-id="eb747-700">A `Using` statement that has a local variable declaration statement may acquire multiple resources at a time, which is equivalent to nested `Using` statements.</span></span>  <span data-ttu-id="eb747-701">たとえば、次のような形式の `Using` ステートメントがあります。</span><span class="sxs-lookup"><span data-stu-id="eb747-701">For example, a `Using` statement of the form:</span></span>

```vb
Using r1 As R = New R(), r2 As R = New R()
    r1.F()
    r2.F()
End Using
```

<span data-ttu-id="eb747-702">上の式は、下の式と同等です。</span><span class="sxs-lookup"><span data-stu-id="eb747-702">is equivalent to:</span></span>

```vb
Using r1 As R = New R()
    Using r2 As R = New R()
        r1.F()
        r2.F()
    End Using
End Using
```


## <a name="await-statement"></a><span data-ttu-id="eb747-703">Await ステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-703">Await Statement</span></span>

<span data-ttu-id="eb747-704">Await ステートメントは、await 演算子式 (Section [Await 演算子](expressions.md#await-operator)) と同じ構文を使用します。 await 式を許可するメソッドでのみ使用できます。また、await 演算子式と同じ動作をします。</span><span class="sxs-lookup"><span data-stu-id="eb747-704">An await statement has the same syntax as an await operator expression (Section [Await Operator](expressions.md#await-operator)), is allowed only in methods that also allow await expressions, and has the same behavior as an await operator expression.</span></span>

<span data-ttu-id="eb747-705">ただし、値または void として分類される場合があります。</span><span class="sxs-lookup"><span data-stu-id="eb747-705">However, it may be classified as either a value or void.</span></span> <span data-ttu-id="eb747-706">Await 演算子式の評価によって得られた値はすべて破棄されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-706">Any value resulting from evaluation of the await operator expression is discarded.</span></span>

```antlr
AwaitStatement
    : AwaitOperatorExpression StatementTerminator
    ;
```

## <a name="yield-statement"></a><span data-ttu-id="eb747-707">Yield ステートメント</span><span class="sxs-lookup"><span data-stu-id="eb747-707">Yield Statement</span></span>

<span data-ttu-id="eb747-708">Yield ステートメントは、「 [Iterator メソッド](statements.md#iterator-methods)」で説明されている反復子メソッドに関連しています。</span><span class="sxs-lookup"><span data-stu-id="eb747-708">Yield statements are related to iterator methods, which are described in Section [Iterator Methods](statements.md#iterator-methods).</span></span>

```antlr
YieldStatement
    : 'Yield' Expression StatementTerminator
    ;
```

<span data-ttu-id="eb747-709">`Yield` は、すぐ外側のメソッドまたはラムダ式が表示される場合、`Iterator` 修飾子を持ち、その `Iterator` 修飾子の後に `Yield` が表示される場合、予約語になります。他の場所では予約されていません。</span><span class="sxs-lookup"><span data-stu-id="eb747-709">`Yield` is a reserved word if the immediately enclosing method or lambda expression in which it appears has an `Iterator` modifier, and if the `Yield` appears after that `Iterator` modifier; it is unreserved elsewhere.</span></span> <span data-ttu-id="eb747-710">プリプロセッサディレクティブでも予約されていません。</span><span class="sxs-lookup"><span data-stu-id="eb747-710">It is also unreserved in preprocessor directives.</span></span> <span data-ttu-id="eb747-711">Yield ステートメントは、予約語であるメソッドまたはラムダ式の本体でのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="eb747-711">The yield statement is only allowed in the body of a method or lambda expression where it is a reserved word.</span></span> <span data-ttu-id="eb747-712">すぐ外側のメソッドまたはラムダ内では、yield ステートメントは、`Catch` または `Finally` ブロックの本体、または `SyncLock` ステートメントの本体の内部には記述できません。</span><span class="sxs-lookup"><span data-stu-id="eb747-712">Within the immediately enclosing method or lambda, the yield statement may not occur inside the body of a `Catch` or `Finally` block, nor inside the body of a `SyncLock` statement.</span></span>

<span data-ttu-id="eb747-713">Yield ステートメントは、1つの式を受け取ります。この式は、値として分類される必要があり、その型は、それを囲む反復[子メソッドの](statements.md#iterator-methods)*反復子の現在の変数*の型に暗黙的に変換できます。</span><span class="sxs-lookup"><span data-stu-id="eb747-713">The yield statement takes a single expression which must be classified as a value and whose type is implicitly convertible to the type of the *iterator current variable* (Section [Iterator Methods](statements.md#iterator-methods)) of its enclosing iterator method.</span></span>

<span data-ttu-id="eb747-714">制御フローは、反復子オブジェクトで `MoveNext` メソッドが呼び出された場合にのみ、`Yield` ステートメントに到達します。</span><span class="sxs-lookup"><span data-stu-id="eb747-714">Control flow only ever reaches a `Yield` statement when the `MoveNext` method is invoked on an iterator object.</span></span> <span data-ttu-id="eb747-715">(これは、反復子メソッドのインスタンスが、反復子オブジェクトで呼び出されている `MoveNext` または `Dispose` メソッドのためにステートメントを実行するだけであるためです。 `Dispose` メソッドでは、`Yield` が許可されていない `Finally` ブロックでのみコードが実行されます)。</span><span class="sxs-lookup"><span data-stu-id="eb747-715">(This is because an iterator method instance only ever executes its statements due to the `MoveNext` or `Dispose` methods being called on an iterator object; and the `Dispose` method will only ever execute code in `Finally` blocks, where `Yield` is not allowed).</span></span>

<span data-ttu-id="eb747-716">`Yield` ステートメントを実行すると、その式が評価され、その反復子オブジェクトに関連付けられている iterator メソッドインスタンスの*反復子現在の変数*に格納されます。</span><span class="sxs-lookup"><span data-stu-id="eb747-716">When a `Yield` statement is executed, its expression is evaluated and stored in the *iterator current variable* of the iterator method instance associated with that iterator object.</span></span> <span data-ttu-id="eb747-717">`True` 値が `MoveNext`の呼び出し元に返されます。このインスタンスの制御ポイントは、iterator オブジェクトでの次の `MoveNext` の呼び出しまで進められなくなります。</span><span class="sxs-lookup"><span data-stu-id="eb747-717">The value `True` is returned to the invoker of `MoveNext`, and the control point of this instance stops advancing until the next invocation of `MoveNext` on the iterator object.</span></span>

