---
ms.openlocfilehash: 68237df3f66ba1cec661e6580c27bb5beedd6b9a
ms.sourcegitcommit: 6eca149bdc736113e0adb709212bd266c9503c33
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2019
ms.locfileid: "47426784"
---
# <a name="statements"></a><span data-ttu-id="9d4b6-101">ステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-101">Statements</span></span>

<span data-ttu-id="9d4b6-102">ステートメントは、実行可能コードを表します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-102">Statements represent executable code.</span></span>

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

<span data-ttu-id="9d4b6-103">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="9d4b6-103">__Note.__</span></span> <span data-ttu-id="9d4b6-104">Microsoft Visual Basic コンパイラは、キーワードまたは識別子で始まるステートメントを許可するだけです。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-104">The Microsoft Visual Basic Compiler only allows statements which start with a keyword or an identifier.</span></span> <span data-ttu-id="9d4b6-105">したがって、呼び出しステートメントでは、"`Call (Console).WriteLine`「許可されますが、呼び出しステートメント」`(Console).WriteLine`"はありません。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-105">Thus, for instance, the invocation statement "`Call (Console).WriteLine`" is allowed, but the invocation statement "`(Console).WriteLine`" is not.</span></span>

## <a name="control-flow"></a><span data-ttu-id="9d4b6-106">制御フロー</span><span class="sxs-lookup"><span data-stu-id="9d4b6-106">Control Flow</span></span>

<span data-ttu-id="9d4b6-107">*制御フロー*ステートメントと式が実行されるシーケンスです。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-107">*Control flow* is the sequence in which statements and expressions are executed.</span></span> <span data-ttu-id="9d4b6-108">実行順序は、特定のステートメントまたは式に依存します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-108">The order of execution depends on the particular statement or expression.</span></span>

<span data-ttu-id="9d4b6-109">たとえば、加算演算子を評価するときに (セクション[加算演算子](expressions.md#addition-operator))、左オペランドを評価すると、最初、右側のオペランドとし、演算子自体。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-109">For example, when evaluating an addition operator (Section [Addition Operator](expressions.md#addition-operator)), first the left operand is evaluated, then the right operand, and then the operator itself.</span></span> <span data-ttu-id="9d4b6-110">ブロックが実行される (セクション[ブロックとラベル](statements.md#blocks-and-labels)) によって、最初のサブステートメントを最初に実行し、ブロックのステートメントを 1 つずつです。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-110">Blocks are executed (Section [Blocks and Labels](statements.md#blocks-and-labels)) by first executing their first substatement, and then proceeding one by one through the statements of the block.</span></span>

<span data-ttu-id="9d4b6-111">この暗黙的な順序付けの概念は、*コントロール ポイント*、これは、次の操作が実行されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-111">Implicit in this ordering is the concept of a *control point*, which is the next operation to be executed.</span></span> <span data-ttu-id="9d4b6-112">作成というメソッドが呼び出されます (または「と呼ばれる」)、ときに、*インスタンス*メソッドの。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-112">When a method is invoked (or "called"), we say it creates an *instance* of the method.</span></span> <span data-ttu-id="9d4b6-113">メソッドのインスタンスは、独自のコピーのメソッドのパラメーターとローカル変数、および独自のコントロール ポイントで構成されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-113">A method instance consists of its own copy of the method's parameters and local variables, and its own control point.</span></span>

### <a name="regular-methods"></a><span data-ttu-id="9d4b6-114">通常のメソッド</span><span class="sxs-lookup"><span data-stu-id="9d4b6-114">Regular Methods</span></span>

<span data-ttu-id="9d4b6-115">通常のメソッドの例を次に示します</span><span class="sxs-lookup"><span data-stu-id="9d4b6-115">Here is an example of a regular method</span></span>

```vb
Function Test() As Integer
    Console.WriteLine("hello")
    Return 1
End Sub

Dim x = Test()    ' invokes the function, prints "hello", assigns 1 to x
```

<span data-ttu-id="9d4b6-116">通常のメソッドが呼び出されます</span><span class="sxs-lookup"><span data-stu-id="9d4b6-116">When a regular method is invoked,</span></span>

1. <span data-ttu-id="9d4b6-117">まず、メソッドのインスタンスは、その呼び出しに固有で作成されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-117">First an instance of the method is created specific to that invocation.</span></span> <span data-ttu-id="9d4b6-118">このインスタンスには、すべてのパラメーターのコピーと、メソッドのローカル変数が含まれています。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-118">This instance includes a copy of all parameters and local variables of the method.</span></span>
2. <span data-ttu-id="9d4b6-119">すべてのパラメーターは、指定された値、およびすべてのローカル変数の型の既定値に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-119">Then all of its parameters are initialized to the supplied values, and all of its local variables to the default values of their types.</span></span>
3. <span data-ttu-id="9d4b6-120">場合、 `Function`、暗黙のローカル変数も初期化と呼ばれる、*関数の戻り変数*名前が関数の名前、型が関数の戻り値の型と既定の初期値です型。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-120">In the case of a `Function`, an implicit local variable is also initialized called the *function return variable* whose name is the function's name, whose type is the return type of the function and whose initial value is the default of its type.</span></span>
4. <span data-ttu-id="9d4b6-121">メソッド インスタンスの管理ポイントは、メソッド本体の最初のステートメントで設定し、すぐにメソッド本体をそこから実行が開始されると (セクション[ブロックとラベル](statements.md#blocks-and-labels))。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-121">The method instance's control point is then set at the first statement of the method body, and the method body immediately starts to execute from there (Section [Blocks and Labels](statements.md#blocks-and-labels)).</span></span>

<span data-ttu-id="9d4b6-122">制御フローが終了したとき、メソッド本体通常どおりに到達を通じて、`End Function`または`End Sub`、末尾をマークするまたは明示的な`Return`または`Exit`メソッド インスタンスの呼び出し元ステートメントの制御フローを返します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-122">When control flow exits the method body normally - through reaching the `End Function` or `End Sub` that mark its end, or through an explicit `Return` or `Exit` statement - control flow returns to the caller of the method instance.</span></span> <span data-ttu-id="9d4b6-123">関数の戻り値の変数がある場合、呼び出しの結果、この変数の最終的な値です。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-123">If there is a function return variable, the result of the invocation is the final value of this variable.</span></span>

<span data-ttu-id="9d4b6-124">制御フローでは、ハンドルされない例外をメソッド本体を終了すると、その例外は、呼び出し元に伝達されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-124">When control flow exits the method body through an unhandled exception, that exception is propagated to the caller.</span></span>

<span data-ttu-id="9d4b6-125">制御フローが終了すると、後に、メソッドのインスタンスをライブ参照が不要になった存在します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-125">After control flow has exited, there are no longer any live references to the method instance.</span></span> <span data-ttu-id="9d4b6-126">メソッドのインスタンスにローカル変数またはパラメーターのコピーへの参照のみが保持されていると、ガベージ コレクションがある可能性があります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-126">If the method instance held the only references to its copy of local variables or parameters, then they may be garbage collected.</span></span>

### <a name="iterator-methods"></a><span data-ttu-id="9d4b6-127">反復子メソッド</span><span class="sxs-lookup"><span data-stu-id="9d4b6-127">Iterator Methods</span></span>

<span data-ttu-id="9d4b6-128">反復子メソッドがいずれかで使用できるシーケンスを生成する便利な方法として使用される、`For Each`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-128">Iterator methods are used as a convenient way to generate a sequence, one which can be consumed by the `For Each` statement.</span></span> <span data-ttu-id="9d4b6-129">反復子メソッドを使用して、`Yield`ステートメント (セクション[Yield ステートメント](statements.md#yield-statement)) のシーケンスの要素を提供します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-129">Iterator methods use the `Yield` statement (Section [Yield Statement](statements.md#yield-statement)) to provide elements of the sequence.</span></span> <span data-ttu-id="9d4b6-130">(なしで反復子メソッド`Yield`ステートメント、空のシーケンスが生成されます)。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-130">(An iterator method with no `Yield` statements will produce an empty sequence).</span></span> <span data-ttu-id="9d4b6-131">反復子メソッドの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-131">Here is an example of an iterator method:</span></span>

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

<span data-ttu-id="9d4b6-132">反復子メソッドが呼び出されると、戻り値の型は`IEnumerator(Of T)`、</span><span class="sxs-lookup"><span data-stu-id="9d4b6-132">When an iterator method is invoked whose return type is `IEnumerator(Of T)`,</span></span>

1. <span data-ttu-id="9d4b6-133">まず、反復子メソッドのインスタンスは、その呼び出しに固有で作成されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-133">First an instance of the iterator method is created specific to that invocation.</span></span> <span data-ttu-id="9d4b6-134">このインスタンスには、すべてのパラメーターのコピーと、メソッドのローカル変数が含まれています。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-134">This instance includes a copy of all parameters and local variables of the method.</span></span>
2. <span data-ttu-id="9d4b6-135">すべてのパラメーターは、指定された値、およびすべてのローカル変数の型の既定値に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-135">Then all of its parameters are initialized to the supplied values, and all of its local variables to the default values of their types.</span></span>
3. <span data-ttu-id="9d4b6-136">暗黙のローカル変数も初期化と呼ばれる、*反復子の現在の変数*、型が`T`し、初期値がその型の既定値。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-136">An implicit local variable is also initialized called the *iterator current variable*, whose type is `T` and whose initial value is the default of its type.</span></span>
4. <span data-ttu-id="9d4b6-137">メソッド インスタンスの管理ポイントは、メソッド本体の最初のステートメントで設定されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-137">The method instance's control point is then set at the first statement of the method body.</span></span>
5. <span data-ttu-id="9d4b6-138">*反復子オブジェクト*し、作成されると、このメソッドのインスタンスに関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-138">An *iterator object* is then created, associated with this method instance.</span></span> <span data-ttu-id="9d4b6-139">反復子オブジェクトは、宣言された戻り値の型を実装し、以下に示すように動作します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-139">The iterator object implements the declared return type and has behavior as described below.</span></span>
6. <span data-ttu-id="9d4b6-140">制御フローが再開し*直ちに*呼び出し元の呼び出しの結果は、反復子オブジェクトとします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-140">Control flow is then resumed *immediately* in the caller, and the result of the invocation is the iterator object.</span></span> <span data-ttu-id="9d4b6-141">この転送は、反復子メソッドのインスタンスを終了せずに行われ、最後を実行するハンドラーは発生しないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-141">Note that this transfer is done without exiting the iterator method instance, and does not cause finally handlers to execute.</span></span> <span data-ttu-id="9d4b6-142">メソッドのインスタンスが、反復子オブジェクトで参照されていると、ガベージ収集されません、反復子オブジェクトへのライブ参照が存在する限り、します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-142">The method instance is still referenced by the iterator object, and will not be garbage collected so long as there exists a live reference to the iterator object.</span></span>

<span data-ttu-id="9d4b6-143">ときに、反復子オブジェクトの`Current`プロパティにアクセスした、*現在変数*の呼び出しが返されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-143">When the iterator object's `Current` property is accessed, the *current variable* of the invocation is returned.</span></span>

<span data-ttu-id="9d4b6-144">ときに、反復子オブジェクトの`MoveNext`メソッドが呼び出される、呼び出しがメソッドの新しいインスタンスを作成できません。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-144">When the iterator object's `MoveNext` method is invoked, the invocation does not create a new method instance.</span></span> <span data-ttu-id="9d4b6-145">代わりに、既存のメソッドのインスタンスが使用されます (とその管理ポイントとローカル変数とパラメーター) の反復子メソッドが最初に呼び出されたときに作成されたインスタンス。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-145">Instead the existing method instance is used (and its control point and local variables and parameters) - the instance that was created when the iterator method was first invoked.</span></span> <span data-ttu-id="9d4b6-146">制御フローでは、そのメソッド インスタンスのコントロール ポイントで実行を再開し、通常どおり、iterator メソッドの本体を実行します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-146">Control flow resumes execution at the control point of that method instance, and proceeds through the body of the iterator method as normal.</span></span>

<span data-ttu-id="9d4b6-147">ときに、反復子オブジェクトの`Dispose`メソッドが呼び出される、もう一度既存のメソッドのインスタンスが使用されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-147">When the iterator object's `Dispose` method is invoked, again the existing method instance is used.</span></span> <span data-ttu-id="9d4b6-148">そのメソッド インスタンスのコントロール ポイントで実行フローが再開が、すぐに動作として、`Exit Function`ステートメントが次の操作。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-148">Control flow resumes at the control point of that method instance, but then immediately behaves as if an `Exit Function` statement were the next operation.</span></span>

<span data-ttu-id="9d4b6-149">上記の説明の呼び出しの動作の`MoveNext`または`Dispose`反復子オブジェクトのみの場合は、以前のすべてを適用の呼び出し`MoveNext`または`Dispose`その反復子オブジェクトに対して既にを返したものの呼び出し元。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-149">The above descriptions of behavior for invocation of `MoveNext` or `Dispose` on an iterator object only apply if all previous invocations of `MoveNext` or `Dispose` on that iterator object have already returned to their callers.</span></span> <span data-ttu-id="9d4b6-150">まだ、動作は定義されません。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-150">If they haven't, then the behavior is undefined.</span></span>

<span data-ttu-id="9d4b6-151">制御フローが終了したとき、反復子メソッドの本体普通に到達を通じて、 `End Function` 、末尾をマークするまたは明示的な`Return`または`Exit`--ステートメントにする必要がありますが行っての呼び出しのコンテキストで`MoveNext`または`Dispose` 、反復子メソッドのインスタンスを再開する、反復子オブジェクトに対して関数が使用している反復子メソッドが最初に呼び出されたときに作成されたメソッドのインスタンス。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-151">When control flow exits the iterator method body normally -- through reaching the `End Function` that mark its end, or through an explicit `Return` or `Exit` statement -- it must have done so in the context of an invocation of `MoveNext` or `Dispose` function on an iterator object to resume the iterator method instance, and it will have been using the method instance that was created when the iterator method was first invoked.</span></span> <span data-ttu-id="9d4b6-152">そのインスタンスの制御点のままになって、`End Function`ステートメント、および呼び出し元に制御フローが再開への呼び出しが再開された場合と`MoveNext`値`False`が呼び出し元に返されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-152">The control point of that instance is left at the `End Function` statement, and control flow resumes in the caller; and if it had been resumed by a call to `MoveNext` then the value `False` is returned to the caller.</span></span>

<span data-ttu-id="9d4b6-153">制御フローの呼び出しをもう一度値は、呼び出し元に未処理の例外をその例外を反復子メソッドの本体が伝達される終了時に`MoveNext`またはの`Dispose`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-153">When control flow exits the iterator method body through an unhandled exception, then the exception is propagated to the caller, which again will be either an invocation of `MoveNext` or of `Dispose`.</span></span>

<span data-ttu-id="9d4b6-154">その他の考えられる戻り値の型、反復子関数の場合</span><span class="sxs-lookup"><span data-stu-id="9d4b6-154">As for the other possible return types of an iterator function,</span></span>

* <span data-ttu-id="9d4b6-155">反復子メソッドが呼び出されると、戻り値の型は`IEnumerable(Of T)`一部`T`インスタンスを最初に作成 - 反復子メソッドのメソッドでは、すべてのパラメーターの--の呼び出しに特定指定された値で初期化されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-155">When an iterator method is invoked whose return type is `IEnumerable(Of T)` for some `T`, an instance is first created -- specific to that invocation of the iterator method -- of all parameters in the method, and they are initialized with the supplied values.</span></span> <span data-ttu-id="9d4b6-156">呼び出しの結果は、戻り値の型を実装するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-156">The result of the invocation is an an object which implements the return type.</span></span> <span data-ttu-id="9d4b6-157">このオブジェクトの必要があります`GetEnumerator`メソッドが呼び出されると、特定の呼び出しによってその - インスタンスを作成`GetEnumerator`--すべてのパラメーターと、メソッドのローカル変数の。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-157">Should this object's `GetEnumerator` method be called, it creates an instance -- specific to that invocation of `GetEnumerator` -- of all parameters and local variables in the method.</span></span> <span data-ttu-id="9d4b6-158">既に保存すると、値にパラメーターを初期化し、上記の反復子メソッドの場合とが続行されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-158">It initializes the parameters to the values already saved, and proceeds as for the iterator method above.</span></span>
* <span data-ttu-id="9d4b6-159">反復子メソッドが呼び出されると、戻り値の型は、非ジェネリック インターフェイス`IEnumerator`の動作はまったくとして`IEnumerator(Of Object)`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-159">When an iterator method is invoked whose return type is the non-generic interface `IEnumerator`, the behavior is exactly as for `IEnumerator(Of Object)`.</span></span>
* <span data-ttu-id="9d4b6-160">反復子メソッドが呼び出されると、戻り値の型は、非ジェネリック インターフェイス`IEnumerable`の動作はまったくとして`IEnumerable(Of Object)`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-160">When an iterator method is invoked whose return type is the non-generic interface `IEnumerable`, the behavior is exactly as for `IEnumerable(Of Object)`.</span></span>

### <a name="async-methods"></a><span data-ttu-id="9d4b6-161">非同期メソッド</span><span class="sxs-lookup"><span data-stu-id="9d4b6-161">Async Methods</span></span>

<span data-ttu-id="9d4b6-162">非同期メソッドは、たとえば、アプリケーションの UI をブロックすることがなく長時間実行処理を実行する便利な方法です。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-162">Async methods are a convenient way to do long-running work without for example blocking the UI of an application.</span></span> <span data-ttu-id="9d4b6-163">非同期は省略形で*非同期*-非同期メソッドの呼び出し元がその実行をすぐに、再開されますが、今後の後で、非同期メソッドのインスタンスの最終的な完了が発生することを意味します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-163">Async is short for *Asynchronous* - it means that the caller of the async method will resume its execution promptly, but the eventual completion of the async method's instance may happen at some later time in the future.</span></span> <span data-ttu-id="9d4b6-164">名前付け規則により非同期メソッドの前に、サフィックス"Async"。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-164">By convention async methods are named with the suffix "Async".</span></span>

```vb
Async Function TestAsync() As Task(Of String)
    Console.WriteLine("hello")
    Await Task.Delay(100)
    Return "world"
End Function

Dim t = TestAsync()         ' prints "hello"
Console.WriteLine(Await t)  ' prints "world"
```

<span data-ttu-id="9d4b6-165">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="9d4b6-165">__Note.__</span></span> <span data-ttu-id="9d4b6-166">非同期メソッドは*いない*バック グラウンド スレッドで実行します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-166">Async methods are *not* run on a background thread.</span></span> <span data-ttu-id="9d4b6-167">メソッドを介して自身を中断することができます代わりに、`Await`何らかのイベントへの応答で再開するには、演算子、およびスケジュール自体。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-167">Instead they allow a method to suspend itself through the `Await` operator, and schedule itself to be resumed in response to some event.</span></span>

<span data-ttu-id="9d4b6-168">非同期メソッドの呼び出し時に</span><span class="sxs-lookup"><span data-stu-id="9d4b6-168">When an async method is invoked</span></span>

1. <span data-ttu-id="9d4b6-169">まず、async メソッドのインスタンスは、その呼び出しに固有で作成されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-169">First an instance of the async method is created specific to that invocation.</span></span> <span data-ttu-id="9d4b6-170">このインスタンスには、すべてのパラメーターのコピーと、メソッドのローカル変数が含まれています。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-170">This instance includes a copy of all parameters and local variables of the method.</span></span>
2. <span data-ttu-id="9d4b6-171">すべてのパラメーターは、指定された値、およびすべてのローカル変数の型の既定値に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-171">Then all of its parameters are initialized to the supplied values, and all of its local variables to the default values of their types.</span></span>
3. <span data-ttu-id="9d4b6-172">非同期メソッドの戻り値の型の場合`Task(Of T)`のいくつか`T`、暗黙のローカル変数も初期化と呼ばれる、*タスク戻り変数*、型が`T`し、初期値が、既定値は`T`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-172">In the case of an async method with return type `Task(Of T)` for some `T`, an implicit local variable is also initialized called the *task return variable*, whose type is `T` and whose initial value is the default of `T`.</span></span>
4. <span data-ttu-id="9d4b6-173">非同期メソッドがある場合、`Function`で型を返す`Task`または`Task(Of T)`一部`T`、し、その型のオブジェクトに暗黙的に作成、現在の呼び出しに関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-173">If the async method is a `Function` with return type `Task` or `Task(Of T)` for some `T`, then an object of that type implicitly created, associated with the current invocation.</span></span> <span data-ttu-id="9d4b6-174">これと呼ばれますが、*非同期オブジェクト*非同期メソッドのインスタンスを実行することによって行われますが、今後の作業を表します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-174">This is called an *async object* and represents the future work that will be done by executing the instance of the async method.</span></span> <span data-ttu-id="9d4b6-175">この非同期メソッドのインスタンスの呼び出し元のコントロールが再開されると、呼び出し元はその呼び出しの結果としてこの非同期オブジェクトに表示されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-175">When control resumes in the caller of this async method instance, the caller will receive this async object as the result of its invocation.</span></span>
5. <span data-ttu-id="9d4b6-176">非同期メソッドの本体の最初のステートメントで設定しのインスタンスの制御点とすぐにそこからのメソッド本体の実行が開始される (セクション[ブロックとラベル](statements.md#blocks-and-labels))。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-176">The instance's control point is then set at the first statement of the async method body, and immediately starts to execute the method body from there (Section [Blocks and Labels](statements.md#blocks-and-labels)).</span></span>

<span data-ttu-id="9d4b6-177">__再開のデリゲートと現在の呼び出し元__</span><span class="sxs-lookup"><span data-stu-id="9d4b6-177">__Resumption Delegate and Current Caller__</span></span>

<span data-ttu-id="9d4b6-178">セクションで説明されている[Await 演算子](expressions.md#await-operator)の実行、`Await`式が別の場所を移動する制御フローのままメソッド インスタンスのコントロール ポイントを停止することです。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-178">As detailed in Section [Await Operator](expressions.md#await-operator), execution of an `Await` expression has the ability to suspend the method instance's control point leaving control flow to go elsewhere.</span></span> <span data-ttu-id="9d4b6-179">呼び出しからの同じインスタンスのコントロール ポイントでの制御フローを後で再開できます、*再開デリゲート*します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-179">Control flow can later resume at the same instance's control point through invocation of a *resumption delegate*.</span></span> <span data-ttu-id="9d4b6-180">この中断が非同期のメソッドを終了せずに行われ、最後を実行するハンドラーは発生しないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-180">Note that this suspension is done without exiting the async method, and does not cause finally handlers to execute.</span></span> <span data-ttu-id="9d4b6-181">メソッドのインスタンスが再開デリゲートで参照されていると、`Task`または`Task(Of T)`(あれば) が発生してはガベージ収集されませんデリゲートまたは結果のいずれかへのライブ参照が存在する限り、します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-181">The method instance is still referenced by both the resumption delegate and the `Task` or `Task(Of T)` result (if any), and will not be garbage collected so long as there exists a live reference to either delegate or result.</span></span>

<span data-ttu-id="9d4b6-182">ステートメントを想像することをお勧め`Dim x = Await WorkAsync()`次の構文の短縮形として約。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-182">It is helpful to imagine the statement `Dim x = Await WorkAsync()` approximately as syntactic shorthand for the following:</span></span>

```vb
Dim temp = WorkAsync().GetAwaiter()
If Not temp.IsCompleted Then
       temp.OnCompleted(resumptionDelegate)
       Return [task]
       CONT:   ' invocation of 'resumptionDelegate' will resume here
End If
Dim x = temp.GetResult()
```

<span data-ttu-id="9d4b6-183">次の場合、*現在の呼び出し元*メソッドのインスタンスは元の呼び出し元、または再開のデリゲートの呼び出し元のいずれかとして定義されますより新しい方します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-183">In the following, the *current caller* of the method instance is defined as either the original caller, or the caller of the resumption delegate, whichever is more recent.</span></span>

<span data-ttu-id="9d4b6-184">制御フローが到達を通じて--async メソッドの本体が終了したとき、`End Sub`または`End Function`、末尾をマークするまたは明示的な`Return`または`Exit`ステートメント、またはに設定されているインスタンスの管理ポイントにハンドルされない例外--メソッドの末尾。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-184">When control flow exits the async method body -- through reaching the `End Sub` or `End Function` that mark its end, or through an explicit `Return` or `Exit` statement, or through an unhandled exception -- the instance's control point is set to the end of the method.</span></span> <span data-ttu-id="9d4b6-185">動作は、非同期メソッドの戻り値の型に依存します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-185">Behavior then depends on the return type of the async method.</span></span>

* <span data-ttu-id="9d4b6-186">場合、`Async Function`で型を返す`Task`:</span><span class="sxs-lookup"><span data-stu-id="9d4b6-186">In the case of an `Async Function` with return type `Task`:</span></span>
  1. <span data-ttu-id="9d4b6-187">制御フローが終了し、ハンドルされない例外を通じて非同期オブジェクトの状態に設定されます`TaskStatus.Faulted`とその`Exception.InnerException`プロパティが、例外に設定 (を除く: などの特定の実装で定義された例外`OperationCanceledException`に変更`TaskStatus.Canceled`).</span><span class="sxs-lookup"><span data-stu-id="9d4b6-187">If control flow exits through an unhandled exception, then the async object's status is set to `TaskStatus.Faulted` and its `Exception.InnerException` property is set to the exception (except: certain implementation-defined exceptions such as `OperationCanceledException` change it to `TaskStatus.Canceled`).</span></span> <span data-ttu-id="9d4b6-188">現在の呼び出し元に制御フローを再開します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-188">Control flow resumes in the current caller.</span></span>
  2. <span data-ttu-id="9d4b6-189">非同期オブジェクトの状態に設定する場合は、`TaskStatus.Completed`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-189">Otherwise, the async object's status is set to `TaskStatus.Completed`.</span></span> <span data-ttu-id="9d4b6-190">現在の呼び出し元に制御フローを再開します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-190">Control flow resumes in the current caller.</span></span>

     <span data-ttu-id="9d4b6-191">(__に注意してください。__</span><span class="sxs-lookup"><span data-stu-id="9d4b6-191">(__Note.__</span></span> <span data-ttu-id="9d4b6-192">全体ポイント タスク、および非同期メソッドを興味深い点はタスクになるときに完了し、それを待機していたすべてのメソッドが実行再開デリゲートが現在、ブロックされていないつまりなる。)</span><span class="sxs-lookup"><span data-stu-id="9d4b6-192">The whole point of Task, and what makes async methods interesting, is that when a task becomes Completed then any methods that were awaiting it will presently have their resumption delegates executed, i.e. they will become unblocked.)</span></span>

* <span data-ttu-id="9d4b6-193">場合、`Async Function`型を返します`Task(Of T)`いくつかの`T`: としての動作は、非同期オブジェクトをケースでない例外を除く`Result`プロパティは、タスク戻り変数の最終的な値にも設定します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-193">In the case of an `Async Function` with return type `Task(Of T)` for some `T`: the behavior is as above, except that in non-exception cases the async object's `Result` property is also set to the final value of the task return variable.</span></span>

* <span data-ttu-id="9d4b6-194">場合、 `Async Sub`:</span><span class="sxs-lookup"><span data-stu-id="9d4b6-194">In the case of an `Async Sub`:</span></span>
  1. <span data-ttu-id="9d4b6-195">制御フローが終了し、ハンドルされない例外でその例外の場合は、いくつかの実装に固有の方法で、環境に反映されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-195">If control flow exits through an unhandled exception, then that exception is propagated to the environment in some implementation-specific manner.</span></span> <span data-ttu-id="9d4b6-196">現在の呼び出し元に制御フローを再開します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-196">Control flow resumes in the current caller.</span></span>
  2. <span data-ttu-id="9d4b6-197">それ以外の場合、制御フローは、現在の呼び出し元に単に再開します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-197">Otherwise, control flow simply resumes in the current caller.</span></span>

#### <a name="async-sub"></a><span data-ttu-id="9d4b6-198">Async Sub</span><span class="sxs-lookup"><span data-stu-id="9d4b6-198">Async Sub</span></span>

<span data-ttu-id="9d4b6-199">一部の Microsoft 固有の動作がある、`Async Sub`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-199">There is some Microsoft-specific behavior of an `Async Sub`.</span></span>

<span data-ttu-id="9d4b6-200">場合`SynchronizationContext.Current`は`Nothing`呼び出しの開始日、ハンドルされない例外、Async Sub から投稿されます Threadpool にします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-200">If `SynchronizationContext.Current` is `Nothing` at the start of the invocation, then any unhandled exceptions from an Async Sub will be posted to the Threadpool.</span></span>

<span data-ttu-id="9d4b6-201">場合`SynchronizationContext.Current`ない`Nothing`し、呼び出しの開始時`OperationStarted()`メソッドの開始前に、そのコンテキストで呼び出されると`OperationCompleted()`終了後にします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-201">If `SynchronizationContext.Current` is not `Nothing` at the start of the invocation, then `OperationStarted()` is invoked on that context before the start of the method and `OperationCompleted()` after the end.</span></span> <span data-ttu-id="9d4b6-202">さらに、ハンドルされない例外は、同期コンテキストで再スローに投稿されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-202">Additionally, any unhandled exceptions will be posted to be rethrown on the synchronization context.</span></span>

<span data-ttu-id="9d4b6-203">つまり、UI アプリケーションでの`Async Sub`UI スレッドで呼び出される、UI スレッド処理に失敗したすべての例外を再発行されるは。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-203">This means that in UI applications, for an `Async Sub` that is invoked on the UI thread, any exceptions it fails to handle will be reposted the UI thread.</span></span>

#### <a name="mutable-structures-in-async-and-iterator-methods"></a><span data-ttu-id="9d4b6-204">Async および反復子メソッドで変更可能な構造体</span><span class="sxs-lookup"><span data-stu-id="9d4b6-204">Mutable structures in async and iterator methods</span></span>

<span data-ttu-id="9d4b6-205">変更可能な構造一般し、見なされ不適切な手法は、非同期または反復子メソッドでサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-205">Mutable structures in general are considered bad practice, and they are not supported by async or iterator methods.</span></span> <span data-ttu-id="9d4b6-206">具体的には、構造体の非同期または反復子メソッドの呼び出しごとが暗黙的に動作を*コピー*の呼び出しの時点でコピーされた構造体。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-206">In particular, each invocation of an async or iterator method in a structure will implicitly operate on a *copy* of that structure that is copied at its moment of invocation.</span></span> <span data-ttu-id="9d4b6-207">たとえば、このため、</span><span class="sxs-lookup"><span data-stu-id="9d4b6-207">Thus, for example,</span></span>

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

### <a name="blocks-and-labels"></a><span data-ttu-id="9d4b6-208">ブロックとラベル</span><span class="sxs-lookup"><span data-stu-id="9d4b6-208">Blocks and Labels</span></span>

<span data-ttu-id="9d4b6-209">実行可能ステートメントのグループには、ステートメント ブロックは呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-209">A group of executable statements is called a statement block.</span></span> <span data-ttu-id="9d4b6-210">ステートメント ブロックの実行、ブロックの最初のステートメントから始まります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-210">Execution of a statement block begins with the first statement in the block.</span></span> <span data-ttu-id="9d4b6-211">ステートメントを実行すると、構文の順序で次のステートメントを実行すると、ステートメントが実行を別の場所を転送するか、例外が発生した場合を除き、します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-211">Once a statement has been executed, the next statement in lexical order is executed, unless a statement transfers execution elsewhere or an exception occurs.</span></span>

<span data-ttu-id="9d4b6-212">ステートメント ブロック内では、論理行のステートメントの除算はラベルの宣言ステートメントを除く意味ではありません。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-212">Within a statement block, the division of statements on logical lines is not significant with the exception of label declaration statements.</span></span> <span data-ttu-id="9d4b6-213">などの分岐ステートメントの対象として使用できるステートメント ブロック内で特定の位置を識別する識別子は、ラベルは`GoTo`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-213">A label is an identifier that identifies a particular position within the statement block that can be used as the target of a branch statement such as `GoTo`.</span></span>

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


<span data-ttu-id="9d4b6-214">ラベルの宣言ステートメントは論理行の先頭に表示され、識別子またはリテラル整数のいずれかをラベルとして使用することがあります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-214">Label declaration statements must appear at the beginning of a logical line and labels may be either an identifier or an integer literal.</span></span> <span data-ttu-id="9d4b6-215">ラベルの宣言ステートメントと呼び出しステートメントの両方が 1 つの識別子で構成できますので、ローカル行の先頭に単一の識別子は、常に、ラベルの宣言ステートメントと見なされます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-215">Because both label declaration statements and invocation statements can consist of a single identifier, a single identifier at the beginning of a local line is always considered a label declaration statement.</span></span> <span data-ttu-id="9d4b6-216">同じ論理行の後にステートメントがない場合でも、コロン、ラベルの宣言ステートメントの後に必ず必要があります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-216">Label declaration statements must always be followed by a colon, even if no statements follow on the same logical line.</span></span>

<span data-ttu-id="9d4b6-217">ラベルは、独自の宣言領域があるし、他の識別子に干渉することはできません。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-217">Labels have their own declaration space and do not interfere with other identifiers.</span></span> <span data-ttu-id="9d4b6-218">次の例が有効であり、名前の変数を使用して`x`パラメーターとラベルの両方。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-218">The following example is valid and uses the name variable `x` both as a parameter and as a label.</span></span>

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

<span data-ttu-id="9d4b6-219">ラベルのスコープは、それを含むメソッドの本体です。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-219">The scope of a label is the body of the method containing it.</span></span>

<span data-ttu-id="9d4b6-220">読みやすさ、複数サブステートメントに関連するステートメントの運用は、たとえ、サブステートメントがある場合それぞれ単独でラベル付けされた行にこの仕様では、1 つの運用環境として扱われます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-220">For the sake of readability, statement productions that involve multiple substatements are treated as a single production in this specification, even though the substatements may each be by themselves on a labeled line.</span></span>


### <a name="local-variables-and-parameters"></a><span data-ttu-id="9d4b6-221">ローカル変数とパラメーター</span><span class="sxs-lookup"><span data-stu-id="9d4b6-221">Local Variables and Parameters</span></span>

<span data-ttu-id="9d4b6-222">方法の詳細セクションの前に、メソッドのインスタンスを作成するときと、メソッドのローカル変数とパラメーターのコピーにします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-222">The preceding sections detail how and when method instances are created, and with them the copies of a method's local variables and parameters.</span></span> <span data-ttu-id="9d4b6-223">さらに、ループの本体が入力されるたびに新しいコピーが作成セクションで説明した、ループ内で宣言されている各ローカル変数の[Loop ステートメント](statements.md#loop-statements)メソッドのインスタンスが含まれています、ローカル変数のこのコピーにはではなく、前のコピーです。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-223">In addition, each time the body of a loop is entered, a new copy is made of each local variable declared inside that loop as described in Section [Loop Statements](statements.md#loop-statements), and the method instance now contains this copy of its local variable rather than the previous copy.</span></span>

<span data-ttu-id="9d4b6-224">すべてのローカル変数は、その型の既定値に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-224">All locals are initialized to their type's default value.</span></span> <span data-ttu-id="9d4b6-225">ローカル変数とパラメーターは、パブリックにアクセスでは常にします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-225">Local variables and parameters are always publicly accessible.</span></span> <span data-ttu-id="9d4b6-226">次の例に示すように、その宣言の前にあるテキストの位置にローカル変数を参照するとエラーにすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-226">It is an error to refer to a local variable in a textual position that precedes its declaration, as the following example illustrates:</span></span>

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

<span data-ttu-id="9d4b6-227">`F`上記のメソッドは、最初の割り当てを`i`具体的には、外側のスコープで宣言されているフィールドを参照しません。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-227">In the `F` method above, the first assignment to `i` specifically does not refer to the field declared in the outer scope.</span></span> <span data-ttu-id="9d4b6-228">代わりに、ローカル変数を参照でありエラー代入変数を宣言します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-228">Rather, it refers to the local variable, and it is in error because it textually precedes the declaration of the variable.</span></span> <span data-ttu-id="9d4b6-229">`G`メソッド、後続の変数宣言は同じローカル変数の宣言内で前の変数宣言で宣言されたローカル変数を参照します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-229">In the `G` method, a subsequent variable declaration refers to a local variable declared in an earlier variable declaration within the same local variable declaration.</span></span>

<span data-ttu-id="9d4b6-230">メソッド内の各ブロックは、ローカル変数の宣言領域を作成します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-230">Each block in a method creates a declaration space for local variables.</span></span> <span data-ttu-id="9d4b6-231">この宣言領域と、メソッドのパラメーター リストを使用して、メソッド本体のローカル変数宣言には、名前が導入されました。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-231">Names are introduced into this declaration space through local variable declarations in the method body and through the parameter list of the method, which introduces names into the outermost block's declaration space.</span></span> <span data-ttu-id="9d4b6-232">ブロックは入れ子による名前のシャドウは許可されません。 名前は再宣言では、可能性がありますいないブロックの名前を宣言したら。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-232">Blocks do not allow shadowing of names through nesting: once a name has been declared in a block, the name may not be redeclared in any nested blocks.</span></span>

<span data-ttu-id="9d4b6-233">したがって、次の例で、`F`と`G`ために、メソッドがエラーでは、名前`i`は外側のブロックで宣言されており、内側のブロックで再宣言することはできません。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-233">Thus, in the following example, the `F` and `G` methods are in error because the name `i` is declared in the outer block and cannot be redeclared in the inner block.</span></span> <span data-ttu-id="9d4b6-234">ただし、`H`と`I`メソッドは、有効なため、2 つ`i`の入れ子になっていない別のブロックで宣言されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-234">However, the `H` and `I` methods are valid because the two `i`'s are declared in separate non-nested blocks.</span></span>

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

<span data-ttu-id="9d4b6-235">メソッドは、関数が、特別なローカル変数は、関数の戻り値を表すメソッドと同じ名前のメソッド本体の宣言領域で暗黙的に宣言します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-235">When the method is a function, a special local variable is implicitly declared in the method body's declaration space with the same name as the method representing the return value of the function.</span></span> <span data-ttu-id="9d4b6-236">ローカルの変数は、式で使用する場合の特別な名前解決のセマンティクスを持っています。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-236">The local variable has special name resolution semantics when used in expressions.</span></span> <span data-ttu-id="9d4b6-237">Invocation 式など、メソッド グループとして分類される式が必要とするコンテキストでローカル変数が使用されるローカル変数ではなく、関数に名前が解決します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-237">If the local variable is used in a context that expects an expression classified as a method group, such as an invocation expression, then the name resolves to the function rather than to the local variable.</span></span> <span data-ttu-id="9d4b6-238">例:</span><span class="sxs-lookup"><span data-stu-id="9d4b6-238">For example:</span></span>

```vb
Function F(i As Integer) As Integer
    If i = 0 Then
        F = 1        ' Sets the return value.
    Else
        F = F(i - 1) ' Recursive call.
    End If
End Function
```

<span data-ttu-id="9d4b6-239">かっこを使用できるあいまいな状況が発生する (など`F(1)`ここで、`F`は、関数の戻り値の型は、1 次元配列です)。 すべてのあいまいな状況で、名前のローカル変数ではなく、関数に解決されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-239">The use of parentheses can cause ambiguous situations (such as `F(1)`, where `F` is a function whose return type is a one-dimensional array); in all ambiguous situations, the name resolves to the function rather than the local variable.</span></span> <span data-ttu-id="9d4b6-240">例:</span><span class="sxs-lookup"><span data-stu-id="9d4b6-240">For example:</span></span>

```vb
Function F(i As Integer) As Integer()
    If i = 0 Then
        F = new Integer(2) { 1, 2, 3 }
    Else
        F = F(i - 1) ' Recursive call, not an index.
    End If
End Function
```

<span data-ttu-id="9d4b6-241">制御フローがメソッド本体を離れると、ローカル変数の値は、invocation 式に渡されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-241">When control flow leaves the method body, the value of the local variable is passed back to the invocation expression.</span></span> <span data-ttu-id="9d4b6-242">メソッドがサブルーチンの場合は、このような暗黙のローカル変数がないと、コントロールは、単に、invocation 式を返します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-242">If the method is a subroutine, there is no such implicit local variable, and control simply returns to the invocation expression.</span></span>

## <a name="local-declaration-statements"></a><span data-ttu-id="9d4b6-243">ローカル宣言ステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-243">Local Declaration Statements</span></span>

<span data-ttu-id="9d4b6-244">ローカル宣言ステートメントでは、新しいローカル変数、ローカル定数、または静的変数を宣言します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-244">A local declaration statement declares a new local variable, local constant, or static variable.</span></span> <span data-ttu-id="9d4b6-245">*ローカル変数*と*ローカル定数*インスタンス変数とスコープをメソッドに定数に相当し、同じ方法で宣言します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-245">*Local variables* and *local constants* are equivalent to instance variables and constants scoped to the method and are declared in the same way.</span></span> <span data-ttu-id="9d4b6-246">*静的変数*のような`Shared`と変数宣言を使用して、`Static`修飾子。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-246">*Static variables* are similar to `Shared` variables and are declared using the `Static` modifier.</span></span>

```antlr
LocalDeclarationStatement
    : LocalModifier VariableDeclarators StatementTerminator
    ;

LocalModifier
    : 'Static' | 'Dim' | 'Const'
    ;
```

<span data-ttu-id="9d4b6-247">静的変数は、メソッドの呼び出し間でそれらの値を保持するローカル変数です。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-247">Static variables are locals that retain their value across invocations of the method.</span></span> <span data-ttu-id="9d4b6-248">非共有メソッド内で宣言された静的変数はインスタンスごと: メソッドを含む型の各インスタンスが静的変数の独自のコピー。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-248">Static variables declared within non-shared methods are per instance: each instance of the type that contains the method has its own copy of the static variable.</span></span> <span data-ttu-id="9d4b6-249">内で宣言された静的変数`Shared`メソッドは、1 つのタイプは、すべてのインスタンスの静的変数のコピーは 1 つだけです。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-249">Static variables declared within `Shared` methods are per type; there is only one copy of the static variable for all instances.</span></span> <span data-ttu-id="9d4b6-250">メソッドには、各エントリに、型の既定値には、ローカル変数が初期化される、中に、静的変数は、型または型のインスタンスが初期化されるときに、型の既定値にのみ初期化します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-250">While local variables are initialized to their type's default value upon each entry into the method, static variables are only initialized to their type's default value when the type or type instance is initialized.</span></span> <span data-ttu-id="9d4b6-251">構造体またはジェネリック メソッドで、静的変数を宣言しない場合があります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-251">Static variables may not be declared in structures or generic methods.</span></span>

<span data-ttu-id="9d4b6-252">常にローカル変数、ローカル定数、および静的変数は、パブリック アクセシビリティを持つし、アクセシビリティ修飾子を指定しない場合があります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-252">Local variables, local constants, and static variables always have public accessibility and may not specify accessibility modifiers.</span></span> <span data-ttu-id="9d4b6-253">ローカル宣言ステートメントで型が指定されていない場合、次の手順は、ローカル宣言の型を決定します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-253">If no type is specified on a local declaration statement, then the following steps determine the type of the local declaration:</span></span>

1. <span data-ttu-id="9d4b6-254">宣言型の文字がある場合、型の文字の型は、ローカル宣言の型になります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-254">If the declaration has a type character, the type of the type character is the type of the local declaration.</span></span>

2. <span data-ttu-id="9d4b6-255">ローカル宣言がローカルの定数の場合、またはローカル宣言は、ローカル変数初期化子を使用して、ローカル変数の型の推定が使用されている場合は、ローカル宣言の型は、初期化子の型から推論されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-255">If the local declaration is a local constant, or if the local declaration is a local variable with an initializer and local variable type inference is being used, the type of the local declaration is inferred from the type of the initializer.</span></span> <span data-ttu-id="9d4b6-256">初期化子は、ローカル宣言を参照している場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-256">If the initializer refers to the local declaration, a compile-time error occurs.</span></span> <span data-ttu-id="9d4b6-257">(ローカル定数は、初期化子を持つに必要ですが)。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-257">(Local constants are required to have initializers.)</span></span>

3. <span data-ttu-id="9d4b6-258">厳密な型が使用されていない場合、ローカル宣言ステートメントの種類は暗黙的に`Object`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-258">If strict semantics are not being used, the type of the local declaration statement is implicitly `Object`.</span></span>

4. <span data-ttu-id="9d4b6-259">それ以外の場合は、コンパイル時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-259">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="9d4b6-260">配列のサイズまたは配列の型修飾子があるローカル宣言ステートメントで型が指定されていない場合は、ローカル宣言の型は、指定したランクを持つ配列し、前の手順を使用して、配列の要素の型を決定します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-260">If no type is specified on a local declaration statement that has an array size or array type modifier, then the type of the local declaration is an array with the specified rank and the previous steps are used to determine the element type of the array.</span></span> <span data-ttu-id="9d4b6-261">ローカル変数の型推論を使用する場合、初期化子の種類はローカル宣言ステートメントと同じ配列形 (つまり配列型の修飾子) の配列型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-261">If local variable type inference is used, the type of the initializer must be an array type with the same array shape (i.e. array type modifiers) as the local declaration statement.</span></span> <span data-ttu-id="9d4b6-262">推論される要素の型が配列型を引き続きする可能性があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-262">Note that it is possible that the inferred element type may still be an array type.</span></span> <span data-ttu-id="9d4b6-263">例:</span><span class="sxs-lookup"><span data-stu-id="9d4b6-263">For example:</span></span>

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

<span data-ttu-id="9d4b6-264">ローカル宣言ステートメントで型が指定されていない場合、null 許容型修飾子を持つ場合は、null 許容型の値既に、ローカル宣言の型が推論される型または推論された型そのものの null 許容バージョン。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-264">If no type is specified on a local declaration statement that has a nullable type modifier, then the type of the local declaration is the nullable version of the inferred type or the inferred type itself if it a nullable value type already.</span></span>  <span data-ttu-id="9d4b6-265">推論された型が null 許容にすることができる値型でない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-265">If the inferred type is not a value type that can be made nullable, a compile-time error occurs.</span></span> <span data-ttu-id="9d4b6-266">Null 許容型修飾子と配列サイズまたは配列型修飾子の両方を配置しない型のローカル宣言ステートメント場合、は、null 許容型修飾子は配列の要素の型に適用すると見なされ、前の手順を使用して、eleme の決定nt の型。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-266">If both a nullable type modifier and an array size or array type modifier are placed on a local declaration statement with no type, then the nullable type modifier is considered to apply to the element type of the array and the previous steps are used to determine the element type.</span></span>

<span data-ttu-id="9d4b6-267">ローカル宣言ステートメントの変数の初期化子は、宣言のテキストの場所に配置する代入ステートメントと同じです。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-267">Variable initializers on local declaration statements are equivalent to assignment statements placed at the textual location of the declaration.</span></span> <span data-ttu-id="9d4b6-268">そのため、ローカル宣言ステートメントでは、実行が分岐、変数の初期化子は実行されません。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-268">Thus, if execution branches over the local declaration statement, the variable initializer is not executed.</span></span> <span data-ttu-id="9d4b6-269">変数の初期化子が実行されるローカル宣言ステートメントを複数回実行する場合と同じ回数。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-269">If the local declaration statement is executed more than once, the variable initializer is executed an equal number of times.</span></span> <span data-ttu-id="9d4b6-270">静的変数は、最初に初期化子を実行するだけです。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-270">Static variables only execute their initializer the first time.</span></span> <span data-ttu-id="9d4b6-271">静的変数の初期化中に例外が発生した場合、静的変数は、静的変数の型の既定値で初期化されたと見なされます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-271">If an exception occurs while initializing a static variable, the static variable is considered initialized with the default value of the static variable's type.</span></span>

<span data-ttu-id="9d4b6-272">次の例は、初期化子の使い方を示しています。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-272">The following example shows the use of initializers:</span></span>

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

<span data-ttu-id="9d4b6-273">このプログラムを出力します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-273">This program prints:</span></span>

```
Static variable x = 5
Static variable x = 6
Static variable x = 7
Local variable y = 8
Local variable y = 8
Local variable y = 8
```

<span data-ttu-id="9d4b6-274">静的ローカル変数の初期化子では、スレッド セーフであると例外に対して保護を使用の初期化中にです。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-274">Initializers on static locals are thread-safe and protected against exceptions during initialization.</span></span> <span data-ttu-id="9d4b6-275">静的ローカル初期化子で例外が発生した場合、静的ローカルは既定値を持つし、を初期化できません。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-275">If an exception occurs during a static local initializer, the static local will have its default value and not be initialized.</span></span> <span data-ttu-id="9d4b6-276">静的ローカル初期化子</span><span class="sxs-lookup"><span data-stu-id="9d4b6-276">A static local initializer</span></span>

```vb
Module Test
    Sub F()
        Static x As Integer = 5
    End Sub
End Module
```

<span data-ttu-id="9d4b6-277">上記の式は、次の式と同じです。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-277">is equivalent to</span></span>

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

<span data-ttu-id="9d4b6-278">ローカル変数、ローカル定数、および静的変数のスコープは、ステートメント、宣言されているをブロックします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-278">Local variables, local constants, and static variables are scoped to the statement block in which they are declared.</span></span> <span data-ttu-id="9d4b6-279">静的変数は、特別な全体のメソッド全体で 1 回に使用されるのみの名前も可能性があります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-279">Static variables are special in that their names may only be used once throughout the entire method.</span></span> <span data-ttu-id="9d4b6-280">たとえば、異なるブロック内にある場合でも、同じ名前の 2 つの静的変数の宣言を指定する有効ではありません。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-280">For example, it is not valid to specify two static variable declarations with the same name even if they are in different blocks.</span></span>


### <a name="implicit-local-declarations"></a><span data-ttu-id="9d4b6-281">暗黙のローカル宣言</span><span class="sxs-lookup"><span data-stu-id="9d4b6-281">Implicit Local Declarations</span></span>

<span data-ttu-id="9d4b6-282">ローカル宣言ステートメントでは、他のローカル変数宣言することも暗黙的を使用しています。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-282">In addition to local declaration statements, local variables can also be declared implicitly through use.</span></span> <span data-ttu-id="9d4b6-283">別のものに対して解決されない名前を使用する簡易名式では、その名前のローカル変数を宣言します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-283">A simple name expression that uses a name that does not resolve to something else declares a local variable by that name.</span></span> <span data-ttu-id="9d4b6-284">例:</span><span class="sxs-lookup"><span data-stu-id="9d4b6-284">For example:</span></span>

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

<span data-ttu-id="9d4b6-285">暗黙のローカル宣言は、変数として分類される式を受け入れることができる式のコンテキストでのみ発生します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-285">Implicit local declaration only occurs in expression contexts that can accept an expression classified as a variable.</span></span> <span data-ttu-id="9d4b6-286">このルールの例外をローカル変数は暗黙的に宣言できません関数の呼び出し式、式のインデックス作成、またはメンバー アクセス式の対象となっているときにします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-286">The exception to this rule is that a local variable may not be implicitly declared when it is the target of a function invocation expression, indexing expression, or a member access expression.</span></span>

<span data-ttu-id="9d4b6-287">暗黙的なローカル変数は、それを含むメソッドの先頭で宣言されているとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-287">Implicit locals are treated as if they are declared at the beginning of the containing method.</span></span> <span data-ttu-id="9d4b6-288">そのため、常にスコープが、メソッド全体の本文をラムダ式の内部で宣言されている場合でもです。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-288">Thus, they are always scoped to the entire method body, even if declared inside of a lambda expression.</span></span> <span data-ttu-id="9d4b6-289">コード例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-289">For example, the following code:</span></span>

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

<span data-ttu-id="9d4b6-290">値を出力`10`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-290">will print the value `10`.</span></span> <span data-ttu-id="9d4b6-291">暗黙的なローカル変数は、入力`Object`なしの型文字は、変数名に関連付けられました。 それ以外の場合、変数の型は、型の文字の型の場合。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-291">Implicit locals are typed as `Object` if no type character was attached to the variable name; otherwise the type of the variable is the type of the type character.</span></span> <span data-ttu-id="9d4b6-292">暗黙的なローカル変数では、ローカル変数の型の推論は使用されません。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-292">Local variable type inference is not used for implicit locals.</span></span>

<span data-ttu-id="9d4b6-293">コンパイル環境または明示的なローカル宣言が指定されている場合`Option Explicit`、すべてのローカル変数を明示的に宣言する必要があります、および暗黙的な変数宣言は許可されていません。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-293">If explicit local declaration is specified by the compilation environment or by `Option Explicit`, all local variables must be explicitly declared and implicit variable declaration is disallowed.</span></span>

## <a name="with-statement"></a><span data-ttu-id="9d4b6-294">ステートメントを使用</span><span class="sxs-lookup"><span data-stu-id="9d4b6-294">With Statement</span></span>

<span data-ttu-id="9d4b6-295">A`With`ステートメント、式を複数回指定することがなく、式のメンバーに複数の参照を使用できます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-295">A `With` statement allows multiple references to an expression's members without specifying the expression multiple times.</span></span>

```antlr
WithStatement
    : 'With' Expression StatementTerminator
      Block?
      'End' 'With' StatementTerminator
    ;
```

<span data-ttu-id="9d4b6-296">式は、値として分類する必要があり、ブロックへの入力時にのみ評価されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-296">The expression must be classified as a value and is evaluated once, upon entry into the block.</span></span> <span data-ttu-id="9d4b6-297">内で、`With`ステートメント ブロック、メンバー アクセス式またはピリオドまたは感嘆符で始まるディクショナリ アクセス式が評価される場合と、`With`式の前にします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-297">Within the `With` statement block, a member access expression or dictionary access expression starting with a period or an exclamation point is evaluated as if the `With` expression preceded it.</span></span> <span data-ttu-id="9d4b6-298">例:</span><span class="sxs-lookup"><span data-stu-id="9d4b6-298">For example:</span></span>

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

<span data-ttu-id="9d4b6-299">分岐することはできません、`With`ブロックの外側でステートメント ブロックから。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-299">It is invalid to branch into a `With` statement block from outside of the block.</span></span>


## <a name="synclock-statement"></a><span data-ttu-id="9d4b6-300">SyncLock ステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-300">SyncLock Statement</span></span>

<span data-ttu-id="9d4b6-301">A`SyncLock`ステートメント、式では、複数の実行スレッドは同時に同じステートメントを実行しないことを確実に同期するステートメントを使用できます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-301">A `SyncLock` statement allows statements to be synchronized on an expression, which ensures that multiple threads of execution do not execute the same statements at the same time.</span></span>

```antlr
SyncLockStatement
    : 'SyncLock' Expression StatementTerminator
      Block?
      'End' 'SyncLock' StatementTerminator
    ;
```

<span data-ttu-id="9d4b6-302">式は、値として分類する必要があり、ブロックへの入力時にのみ評価されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-302">The expression must be classified as a value and is evaluated once, upon entry to the block.</span></span> <span data-ttu-id="9d4b6-303">入力するときに、`SyncLock`ブロック、`Shared`メソッド`System.Threading.Monitor.Enter`ブロック式によって返されるオブジェクトの実行スレッドがある排他ロックするまで、指定された式で呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-303">When entering the `SyncLock` block, the `Shared` method `System.Threading.Monitor.Enter` is called on the specified expression, which blocks until the thread of execution has an exclusive lock on the object returned by the expression.</span></span> <span data-ttu-id="9d4b6-304">内の式の型を`SyncLock`ステートメントは参照型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-304">The type of the expression in a `SyncLock` statement must be a reference type.</span></span> <span data-ttu-id="9d4b6-305">例:</span><span class="sxs-lookup"><span data-stu-id="9d4b6-305">For example:</span></span>

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

<span data-ttu-id="9d4b6-306">クラスの特定のインスタンスで上記の例では同期`Test`させる、複数のスレッドの実行が追加または特定のインスタンスを一度に count 変数から減算できます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-306">The example above synchronizes on the specific instance of the class `Test` to ensure that no more than one thread of execution can add or subtract from the count variable at a time for a particular instance.</span></span>

<span data-ttu-id="9d4b6-307">`SyncLock`ブロックは暗黙的に含まれる、`Try`ステートメントが`Finally`呼び出しをブロック、`Shared`メソッド`System.Threading.Monitor.Exit`式。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-307">The `SyncLock` block is implicitly contained by a `Try` statement whose `Finally` block calls the `Shared` method `System.Threading.Monitor.Exit` on the expression.</span></span> <span data-ttu-id="9d4b6-308">これにより、例外がスローされた場合でも、ロックが解放されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-308">This ensures the lock is freed even when an exception is thrown.</span></span> <span data-ttu-id="9d4b6-309">その結果に分岐する有効ながない、 `SyncLock` 、ブロックの外側からブロックと`SyncLock`の目的でブロックが単一のステートメントとして扱われます`Resume`と`Resume Next`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-309">As a result, it is invalid to branch into a `SyncLock` block from outside of the block, and a `SyncLock` block is treated as a single statement for the purposes of `Resume` and `Resume Next`.</span></span> <span data-ttu-id="9d4b6-310">上記の例では、次のコードと同等です。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-310">The above example is equivalent to the following code:</span></span>

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


## <a name="event-statements"></a><span data-ttu-id="9d4b6-311">Event ステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-311">Event Statements</span></span>

<span data-ttu-id="9d4b6-312">`RaiseEvent`、 `AddHandler`、および`RemoveHandler`ステートメントは、イベントを発生させるし、動的にイベントを処理します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-312">The `RaiseEvent`, `AddHandler`, and `RemoveHandler` statements raise events and handle events dynamically.</span></span>

```antlr
EventStatement
    : RaiseEventStatement
    | AddHandlerStatement
    | RemoveHandlerStatement
    ;
```

### <a name="raiseevent-statement"></a><span data-ttu-id="9d4b6-313">RaiseEvent ステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-313">RaiseEvent Statement</span></span>

<span data-ttu-id="9d4b6-314">A`RaiseEvent`ステートメントが、特定のイベントが発生したイベント ハンドラーに通知します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-314">A `RaiseEvent` statement notifies event handlers that a particular event has occurred.</span></span>

```antlr
RaiseEventStatement
    : 'RaiseEvent' IdentifierOrKeyword
      ( OpenParenthesis ArgumentList? CloseParenthesis )? StatementTerminator
    ;
```

<span data-ttu-id="9d4b6-315">簡易名式、`RaiseEvent`でステートメントがメンバー参照として解釈されます`Me`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-315">The simple name expression in a `RaiseEvent` statement is interpreted as a member lookup on `Me`.</span></span> <span data-ttu-id="9d4b6-316">したがって、`RaiseEvent x`場合と同様に解釈`RaiseEvent Me.x`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-316">Thus, `RaiseEvent x` is interpreted as if it were `RaiseEvent Me.x`.</span></span> <span data-ttu-id="9d4b6-317">式の結果は、クラス自体で定義されているイベントのイベント アクセスとして分類する必要があります。基本型で定義されているイベントでは使用できません、`RaiseEvent`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-317">The result of the expression must be classified as an event access for an event defined in the class itself; events defined on base types cannot be used in a `RaiseEvent` statement.</span></span>

<span data-ttu-id="9d4b6-318">`RaiseEvent`への呼び出しとしてステートメントを処理、`Invoke`の存在する場合は、指定されたパラメーターを使用して、イベントのデリゲート メソッド。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-318">The `RaiseEvent` statement is processed as a call to the `Invoke` method of the event's delegate, using the supplied parameters, if any.</span></span> <span data-ttu-id="9d4b6-319">デリゲートの値が場合`Nothing`例外はスローされません。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-319">If the delegate's value is `Nothing`, no exception is thrown.</span></span> <span data-ttu-id="9d4b6-320">引数がない場合は、かっこは省略できます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-320">If there are no arguments, the parentheses may be omitted.</span></span> <span data-ttu-id="9d4b6-321">例:</span><span class="sxs-lookup"><span data-stu-id="9d4b6-321">For example:</span></span>

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

<span data-ttu-id="9d4b6-322">クラスは、`Raiser`は上記と同じです。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-322">The class `Raiser` above is equivalent to:</span></span>

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


### <a name="addhandler-and-removehandler-statements"></a><span data-ttu-id="9d4b6-323">AddHandler と RemoveHandler ステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-323">AddHandler and RemoveHandler Statements</span></span>

<span data-ttu-id="9d4b6-324">ほとんどのイベント ハンドラーが自動的にを通じてフックが`WithEvents`変数、必要がありますを動的に追加して、実行時にイベント ハンドラーを削除します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-324">Although most event handlers are automatically hooked up through `WithEvents` variables, it may be necessary to dynamically add and remove event handlers at run time.</span></span> <span data-ttu-id="9d4b6-325">`AddHandler` `RemoveHandler`ステートメントでは、これを実行します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-325">`AddHandler` and `RemoveHandler` statements do this.</span></span>

```antlr
AddHandlerStatement
    : 'AddHandler' Expression Comma Expression StatementTerminator
    ;

RemoveHandlerStatement
    : 'RemoveHandler' Expression Comma Expression StatementTerminator
    ;
```

<span data-ttu-id="9d4b6-326">各ステートメントは 2 つの引数を受け取ります。 最初の引数は、イベントへのアクセスに分類される式である必要がありますし、2 番目の引数が値として分類される式を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-326">Each statement takes two arguments: the first argument must be an expression that is classified as an event access and the second argument must be an expression that is classified as a value.</span></span> <span data-ttu-id="9d4b6-327">2 番目の引数の型は、イベントへのアクセスに関連付けられているデリゲート型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-327">The second argument's type must be the delegate type associated with the event access.</span></span> <span data-ttu-id="9d4b6-328">例:</span><span class="sxs-lookup"><span data-stu-id="9d4b6-328">For example:</span></span>

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

<span data-ttu-id="9d4b6-329">指定されたイベント`E,`、ステートメントを呼び出して、関連する`add_E`または`remove_E`メソッドを追加またはイベントのハンドラーとして、デリゲートを削除するインスタンス。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-329">Given an event `E,` the statement calls the relevant `add_E` or `remove_E` method on the instance to add or remove the delegate as a handler for the event.</span></span> <span data-ttu-id="9d4b6-330">したがって、上記のコードと同等です。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-330">Thus, the above code is equivalent to:</span></span>

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


## <a name="assignment-statements"></a><span data-ttu-id="9d4b6-331">代入ステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-331">Assignment Statements</span></span>

<span data-ttu-id="9d4b6-332">代入ステートメントは、式の値を変数に割り当てます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-332">An assignment statement assigns the value of an expression to a variable.</span></span> <span data-ttu-id="9d4b6-333">割り当ての種類をいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-333">There are several types of assignment.</span></span>

```antlr
AssignmentStatement
    : RegularAssignmentStatement
    | CompoundAssignmentStatement
    | MidAssignmentStatement
    ;
```

### <a name="regular-assignment-statements"></a><span data-ttu-id="9d4b6-334">通常の代入ステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-334">Regular Assignment Statements</span></span>

<span data-ttu-id="9d4b6-335">単純な代入ステートメントは、式の結果を変数に格納します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-335">A simple assignment statement stores the result of an expression in a variable.</span></span>

```antlr
RegularAssignmentStatement
    : Expression Equals Expression StatementTerminator
    ;
```

<span data-ttu-id="9d4b6-336">代入演算子の左側にある式は、中、代入演算子の右側にある式は、値として分類する必要があります、変数またはプロパティ アクセス、として分類する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-336">The expression on the left side of the assignment operator must be classified as a variable or a property access, while the expression on the right side of the assignment operator must be classified as a value.</span></span> <span data-ttu-id="9d4b6-337">式の型は、変数またはプロパティ アクセスの型に暗黙的に変換可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-337">The type of the expression must be implicitly convertible to the type of the variable or property access.</span></span>

<span data-ttu-id="9d4b6-338">割り当てられている変数が参照型の配列要素の場合は、式が配列要素の型と互換性があることを確認するランタイム チェックは実行されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-338">If the variable being assigned into is an array element of a reference type, a run-time check will be performed to ensure that the expression is compatible with the array-element type.</span></span> <span data-ttu-id="9d4b6-339">次の例では最後の割り当てと、`System.ArrayTypeMismatchException`がスローされるため、インスタンスの`ArrayList`の要素に格納することはできません、`String`配列。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-339">In the following example, the last assignment causes a `System.ArrayTypeMismatchException` to be thrown, because an instance of `ArrayList` cannot be stored in an element of a `String` array.</span></span>

```vb
Dim sa(10) As String
Dim oa As Object() = sa
oa(0) = Nothing         ' This is allowed.
oa(1) = "Hello"         ' This is allowed.
oa(2) = New ArrayList() ' System.ArrayTypeMismatchException is thrown.
```

<span data-ttu-id="9d4b6-340">変数は、代入演算子の左側にある式の場合、代入ステートメントは、変数に値を格納します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-340">If the expression on the left side of the assignment operator is classified as a variable, then the assignment statement stores the value in the variable.</span></span> <span data-ttu-id="9d4b6-341">かどうかには、式はプロパティ アクセス、として分類し、代入ステートメントの呼び出しにプロパティへのアクセスを有効に、`Set`値パラメーターの値を持つプロパティのアクセサー代わりに使用します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-341">If the expression is classified as a property access, then the assignment statement turns the property access into an invocation of the `Set` accessor of the property with the value substituted for the value parameter.</span></span> <span data-ttu-id="9d4b6-342">例:</span><span class="sxs-lookup"><span data-stu-id="9d4b6-342">For example:</span></span>

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

<span data-ttu-id="9d4b6-343">変数またはプロパティ アクセスの対象では、値の型として型指定されたが、変数として分類されない、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-343">If the target of the variable or property access is typed as a value type but not classified as a variable, a compile-time error occurs.</span></span> <span data-ttu-id="9d4b6-344">例:</span><span class="sxs-lookup"><span data-stu-id="9d4b6-344">For example:</span></span>

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

<span data-ttu-id="9d4b6-345">割り当てのセマンティクスは、変数または割り当てられるプロパティの種類によって異なりますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-345">Note that the semantics of the assignment depend on the type of the variable or property to which it is being assigned.</span></span> <span data-ttu-id="9d4b6-346">割り当て先の変数が値型の場合、割り当ては、変数に式の値をコピーします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-346">If the variable to which it is being assigned is a value type, the assignment copies the value of the expression into the variable.</span></span> <span data-ttu-id="9d4b6-347">割り当て先の変数が参照型の場合、割り当ては、変数に値自体ではなく、参照をコピーします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-347">If the variable to which it is being assigned is a reference type, the assignment copies the reference, not the value itself, into the variable.</span></span> <span data-ttu-id="9d4b6-348">変数の型がある場合`Object`割り当てのセマンティクスは値の型が値型または参照型を実行時にかどうかによって決まります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-348">If the type of the variable is `Object`, the assignment semantics are determined by whether the value's type is a value type or a reference type at run time.</span></span>


<span data-ttu-id="9d4b6-349">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="9d4b6-349">__Note.__</span></span> <span data-ttu-id="9d4b6-350">組み込み型などの`Integer`と`Date`、参照および値の割り当てのセマンティクスは、同じ種類は変更できないためです。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-350">For intrinsic types such as `Integer` and `Date`, reference and value assignment semantics are the same because the types are immutable.</span></span> <span data-ttu-id="9d4b6-351">その結果、言語は、自由にボックス化された組み込み型の参照の割り当てを使用して、最適化として。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-351">As a result, the language is free to use reference assignment on boxed intrinsic types as an optimization.</span></span> <span data-ttu-id="9d4b6-352">値の観点から、結果は同じです。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-352">From a value perspective, the result is the same.</span></span>

<span data-ttu-id="9d4b6-353">ため、equals 文字 (`=`) される両方の割り当てと等しいかどうかをあいまいさがある単純な代入と、呼び出しステートメントの間など`x = y.ToString()`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-353">Because the equals character (`=`) is used both for assignment and for equality, there is an ambiguity between a simple assignment and an invocation statement in situations such as `x = y.ToString()`.</span></span> <span data-ttu-id="9d4b6-354">このような場合は、代入ステートメントは等値演算子に優先します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-354">In all such cases, the assignment statement takes precedence over the equality operator.</span></span> <span data-ttu-id="9d4b6-355">つまり、例の式として解釈される`x = (y.ToString())`なく`(x = y).ToString()`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-355">This means that the example expression is interpreted as `x = (y.ToString())` rather than `(x = y).ToString()`.</span></span>


### <a name="compound-assignment-statements"></a><span data-ttu-id="9d4b6-356">複合代入ステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-356">Compound Assignment Statements</span></span>

<span data-ttu-id="9d4b6-357">A*複合代入ステートメント*形式の`V op= E`(場所`op`は有効な二項演算子)。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-357">A *compound assignment statement* takes the form `V op= E` (where `op` is a valid binary operator).</span></span>

```antlr
CompoundAssignmentStatement
    : Expression CompoundBinaryOperator LineTerminator? Expression StatementTerminator
    ;

CompoundBinaryOperator
    : '^' '=' | '*' '=' | '/' '=' | '\\' '=' | '+' '=' | '-' '='
    | '&' '=' | '<' '<' '=' | '>' '>' '='
    ;
```

<span data-ttu-id="9d4b6-358">代入演算子の左側にある式は、代入演算子の右側にある式は、値として分類する必要があります、変数またはプロパティ アクセスでは、として分類される必要があります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-358">The expression on the left side of the assignment operator must be classified as a variable or property access, while the expression on the right side of the assignment operator must be classified as a value.</span></span> <span data-ttu-id="9d4b6-359">複合代入ステートメントは、ステートメントと同じ`V = V op E`な相違点は、複合代入演算子の左側にある変数がのみである 1 回評価されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-359">The compound assignment statement is equivalent to the statement `V = V op E` with the difference that the variable on the left side of the compound assignment operator is only evaluated once.</span></span> <span data-ttu-id="9d4b6-360">次の例では、この違いを示しています。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-360">The following example demonstrates this difference:</span></span>

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

<span data-ttu-id="9d4b6-361">式`a(GetIndex())`、複合代入でそのため、コードを出力すると、だけの単純な代入で 2 回評価されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-361">The expression `a(GetIndex())` is evaluated twice for simple assignment but only once for compound assignment, so the code prints:</span></span>

```
Simple assignment
Getting index
Getting index
Compound assignment
Getting index
```


### <a name="mid-assignment-statement"></a><span data-ttu-id="9d4b6-362">Mid の代入ステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-362">Mid Assignment Statement</span></span>

<span data-ttu-id="9d4b6-363">A`Mid`代入ステートメントは、別の文字列に文字列を割り当てます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-363">A `Mid` assignment statement assigns a string into another string.</span></span> <span data-ttu-id="9d4b6-364">割り当ての左側にあるが、関数の呼び出しと同じ構文`Microsoft.VisualBasic.Strings.Mid`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-364">The left side of the assignment has the same syntax as a call to the function `Microsoft.VisualBasic.Strings.Mid`.</span></span>

```antlr
MidAssignmentStatement
    : 'Mid' '$'? OpenParenthesis Expression Comma Expression
      ( Comma Expression )? CloseParenthesis Equals Expression StatementTerminator
    ;
```

<span data-ttu-id="9d4b6-365">最初の引数が、割り当ての対象となるし、変数またはとの間に暗黙的に変換できる型を持つプロパティ アクセスに分類する必要があります`String`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-365">The first argument is the target of the assignment and must be classified as a variable or a property access whose type is implicitly convertible to and from `String`.</span></span> <span data-ttu-id="9d4b6-366">2 番目のパラメーターは、割り当てが、ターゲット文字列の開始し、暗黙的に変換できる型を持つ必要があります値として分類する必要がありますに対応する 1 から始まる開始位置`Integer`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-366">The second parameter is the 1-based start position that corresponds to where the assignment should begin in the target string and must be classified as a value whose type must be implicitly convertible to `Integer`.</span></span> <span data-ttu-id="9d4b6-367">省略可能な 3 番目のパラメーターは、右側にある文字の数に、対象の文字列に割り当てる値し、型が暗黙的に変換できる値として分類する必要があります`Integer`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-367">The optional third parameter is the number of characters from the right-side value to assign into the target string and must be classified as a value whose type is implicitly convertible to `Integer`.</span></span> <span data-ttu-id="9d4b6-368">右側にあるソース文字列は、型が暗黙的に変換できる値として分類する必要があります`String`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-368">The right side is the source string and must be classified as a value whose type is implicitly convertible to `String`.</span></span> <span data-ttu-id="9d4b6-369">右側にあるは、指定した場合、長さのパラメーターに切り捨てられ、開始位置から、左側にある文字列内の文字を置き換えます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-369">The right side is truncated to the length parameter, if specified, and replaces the characters in the left-side string, starting at the start position.</span></span> <span data-ttu-id="9d4b6-370">右側にある文字列に 3 番目のパラメーターよりも少ない文字が含まれている場合は、右側にある文字列の文字だけがコピーされます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-370">If the right side string contained fewer characters than the third parameter, only the characters from the right side string will be copied.</span></span>

<span data-ttu-id="9d4b6-371">次の例では、表示`ab123fg`:</span><span class="sxs-lookup"><span data-stu-id="9d4b6-371">The following example displays `ab123fg`:</span></span>

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

<span data-ttu-id="9d4b6-372">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="9d4b6-372">__Note.__</span></span> <span data-ttu-id="9d4b6-373">`Mid` 予約語ではありません。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-373">`Mid` is not a reserved word.</span></span>


## <a name="invocation-statements"></a><span data-ttu-id="9d4b6-374">呼び出しステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-374">Invocation Statements</span></span>

<span data-ttu-id="9d4b6-375">呼び出しステートメントが省略可能なキーワードに続くメソッドを呼び出す`Call`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-375">An invocation statement invokes a method preceded by the optional keyword `Call`.</span></span> <span data-ttu-id="9d4b6-376">呼び出しステートメントは、次に示すいくつかの違いを関数の呼び出し式と同じ方法で処理されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-376">The invocation statement is processed in the same way as the function invocation expression, with some differences noted below.</span></span> <span data-ttu-id="9d4b6-377">Invocation 式は、値、または void に分類する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-377">The invocation expression must be classified as a value or void.</span></span> <span data-ttu-id="9d4b6-378">Invocation 式の評価結果の任意の値は破棄されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-378">Any value resulting from the evaluation of the invocation expression is discarded.</span></span>

<span data-ttu-id="9d4b6-379">場合、`Call`キーワードを省略すると、その後、invocation 式は、識別子またはキーワードを使用またはを起動する必要があります`.`内で、`With`ブロックします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-379">If the `Call` keyword is omitted, then the invocation expression must start with an identifier or keyword, or with `.` inside a `With` block.</span></span> <span data-ttu-id="9d4b6-380">したがって、たとえば、"`Call 1.ToString()`"有効なステートメントですが、"`1.ToString()`"はありません。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-380">Thus, for instance, "`Call 1.ToString()`" is a valid statement but "`1.ToString()`" is not.</span></span> <span data-ttu-id="9d4b6-381">(いる式コンテキストでは、invocation 式も必要がありますで始まらない識別子に注意してください。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-381">(Note that in an expression context, invocation expressions also need not start with an identifier.</span></span> <span data-ttu-id="9d4b6-382">たとえば、"`Dim x = 1.ToString()`"有効なステートメントです)。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-382">For example, "`Dim x = 1.ToString()`" is a valid statement).</span></span>

<span data-ttu-id="9d4b6-383">呼び出しステートメントおよび呼び出し式にはもう 1 つの違いがある: かどうか、呼び出しステートメントには、引数リストが含まれていますし、呼び出しの引数リストとしてこの常に取得されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-383">There is another difference between the invocation statements and invocation expressions: if an invocation statement includes an argument list, then this is always taken as the argument list of the invocation.</span></span> <span data-ttu-id="9d4b6-384">次の例では、違いを示します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-384">The following example illustrates the difference:</span></span>

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

## <a name="conditional-statements"></a><span data-ttu-id="9d4b6-385">条件付きステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-385">Conditional Statements</span></span>

<span data-ttu-id="9d4b6-386">条件付きステートメントを使用すると、実行時に評価される式に基づいて、ステートメントの条件付きの実行。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-386">Conditional statements allow conditional execution of statements based on expressions evaluated at run time.</span></span>

```antlr
ConditionalStatement
    : IfStatement
    | SelectStatement
    ;
```

### <a name="ifthenelse-statements"></a><span data-ttu-id="9d4b6-387">もし。。。そうしたら。。。Else ステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-387">If...Then...Else Statements</span></span>

<span data-ttu-id="9d4b6-388">`If...Then...Else`ステートメントは、基本的な条件付きステートメント。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-388">An `If...Then...Else` statement is the basic conditional statement.</span></span>

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

<span data-ttu-id="9d4b6-389">内の各式、`If...Then...Else`ステートメントは、「」セクションのブール式をする必要があります[ブール式](expressions.md#boolean-expressions)します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-389">Each expression in an `If...Then...Else` statement must be a Boolean expression, as per Section [Boolean Expressions](expressions.md#boolean-expressions).</span></span> <span data-ttu-id="9d4b6-390">(注: ブール型を持つ式は必要ありません)。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-390">(Note: this does not require the expression to have Boolean type).</span></span> <span data-ttu-id="9d4b6-391">場合内の式、`If`ステートメントが true の場合、ステートメントがで囲まれた、`If`ブロックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-391">If the expression in the `If` statement is true, the statements enclosed by the `If` block are executed.</span></span> <span data-ttu-id="9d4b6-392">式が false の場合の`ElseIf`式が評価されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-392">If the expression is false, each of the `ElseIf` expressions is evaluated.</span></span> <span data-ttu-id="9d4b6-393">1 つの場合の`ElseIf`式が true に評価される、対応するブロックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-393">If one of the `ElseIf` expressions evaluates to true, the corresponding block is executed.</span></span> <span data-ttu-id="9d4b6-394">式を true として評価されていない場合は、`Else`ブロック、`Else`ブロックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-394">If no expression evaluates to true and there is an `Else` block, the `Else` block is executed.</span></span> <span data-ttu-id="9d4b6-395">実行がの末尾に移り、ブロックの実行が終了すると、`If...Then...Else`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-395">Once a block finishes executing, execution passes to the end of the `If...Then...Else` statement.</span></span>

<span data-ttu-id="9d4b6-396">行のバージョン、`If`ステートメントの場合に実行されるステートメントの 1 つのセットには、`If`式が`True`と省略可能なセット式が場合に実行されるステートメントの`False`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-396">The line version of the `If` statement has a single set of statements to be executed if the `If` expression is `True` and an optional set of statements to be executed if the expression is `False`.</span></span> <span data-ttu-id="9d4b6-397">例:</span><span class="sxs-lookup"><span data-stu-id="9d4b6-397">For example:</span></span>

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

<span data-ttu-id="9d4b6-398">場合は、行バージョン ステートメント バインドよりも厳密に小さい":"、およびその`Else`構文的に最も近いの前にバインドする`If`構文で許可されています。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-398">The line version of the If statement binds less tightly than ":", and its `Else` binds to the lexically nearest preceding `If` that is allowed by the syntax.</span></span> <span data-ttu-id="9d4b6-399">たとえば、次の 2 つのバージョンは同等です。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-399">For example, the following two versions are equivalent:</span></span>

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

<span data-ttu-id="9d4b6-400">行では、ラベルの宣言ステートメント以外のすべてのステートメントが許可されて`If`ブロック ステートメントを含むステートメント。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-400">All statements other than label declaration statements are allowed inside a line `If` statement, including block statements.</span></span> <span data-ttu-id="9d4b6-401">ただし、それらが LineTerminators として使用 StatementTerminators 複数行ラムダ式の中で除きます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-401">However, they may not use LineTerminators as StatementTerminators except inside multi-line lambda expressions.</span></span> <span data-ttu-id="9d4b6-402">例:</span><span class="sxs-lookup"><span data-stu-id="9d4b6-402">For example:</span></span>

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

### <a name="select-case-statements"></a><span data-ttu-id="9d4b6-403">Select Case ステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-403">Select Case Statements</span></span>

<span data-ttu-id="9d4b6-404">A`Select Case`ステートメント、式の値に基づいたステートメントを実行します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-404">A `Select Case` statement executes statements based on the value of an expression.</span></span>

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

<span data-ttu-id="9d4b6-405">式は、値として分類する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-405">The expression must be classified as a value.</span></span> <span data-ttu-id="9d4b6-406">ときに、`Select Case`ステートメントが実行される、`Select`式が最初に評価、`Case`ステートメント テキスト宣言の順序で評価されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-406">When a `Select Case` statement is executed, the `Select` expression is evaluated first, and the `Case` statements are then evaluated in order of textual declaration.</span></span> <span data-ttu-id="9d4b6-407">最初の`Case`ステートメントに評価される`True`のブロックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-407">The first `Case` statement that evaluates to `True` has its block executed.</span></span> <span data-ttu-id="9d4b6-408">ない場合は`Case`ステートメントの評価が`True`があると、`Case Else`ステートメントでは、そのブロックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-408">If no `Case` statement evaluates to `True` and there is a `Case Else` statement, that block is executed.</span></span> <span data-ttu-id="9d4b6-409">実行がの末尾に移り、ブロックの実行が完了すると、`Select`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-409">Once a block has finished executing, execution passes to the end of the `Select` statement.</span></span>

<span data-ttu-id="9d4b6-410">実行、`Case`ブロックは次の switch セクションに「フォールスルー」は許可されていません。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-410">Execution of a `Case` block is not permitted to "fall through" to the next switch section.</span></span> <span data-ttu-id="9d4b6-411">これにより、他の言語で発生するバグの一般的なクラスと、`Case`ステートメントの終了を誤って省略するとします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-411">This prevents a common class of bugs that occur in other languages when a `Case` terminating statement is accidentally omitted.</span></span> <span data-ttu-id="9d4b6-412">この動作を次の例に示します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-412">The following example illustrates this behavior:</span></span>

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

<span data-ttu-id="9d4b6-413">コードを出力します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-413">The code prints:</span></span>

```
x = 10
```

<span data-ttu-id="9d4b6-414">`Case 10`と`Case 20 - 10`同じの値の選択`Case 10`実行後に続くため`Case 20 - 10`テキスト。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-414">Although `Case 10` and `Case 20 - 10` select for the same value, `Case 10` is executed because it precedes `Case 20 - 10` textually.</span></span> <span data-ttu-id="9d4b6-415">ときに、[次へ]`Case`が後に達すると、実行が続行、`Select`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-415">When the next `Case` is reached, execution continues after the `Select` statement.</span></span>

<span data-ttu-id="9d4b6-416">A`Case`句は、2 つの形式をかかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-416">A `Case` clause may take two forms.</span></span> <span data-ttu-id="9d4b6-417">1 つの形式は、省略可能な`Is`キーワード、比較演算子、および式。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-417">One form is an optional `Is` keyword, a comparison operator, and an expression.</span></span> <span data-ttu-id="9d4b6-418">型に式を変換、`Select`式; 式がの型に暗黙的に変換できるかどうか、`Select`式、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-418">The expression is converted to the type of the `Select` expression; if the expression is not implicitly convertible to the type of the `Select` expression, a compile-time error occurs.</span></span> <span data-ttu-id="9d4b6-419">場合、`Select`式が*E*、比較演算子が*Op*、および`Case`式が*E1*、として大文字と小文字を評価*EOP E1*します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-419">If the `Select` expression is *E*, the comparison operator is *Op*, and the `Case` expression is *E1*, the case is evaluated as *E OP E1*.</span></span> <span data-ttu-id="9d4b6-420">演算子は、2 つの式の型に対して有効である必要があります。それ以外の場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-420">The operator must be valid for the types of the two expressions; otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="9d4b6-421">他の形式は、必要に応じて後に、キーワード、式`To`と 2 番目の式。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-421">The other form is an expression optionally followed by the keyword `To` and a second expression.</span></span> <span data-ttu-id="9d4b6-422">両方の式の型に変換されます、`Select`式; いずれかの式がの型に暗黙的に変換できるかどうか、`Select`式、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-422">Both expressions are converted to the type of the `Select` expression; if either expression is not implicitly convertible to the type of the `Select` expression, a compile-time error occurs.</span></span> <span data-ttu-id="9d4b6-423">場合、`Select`式が`E`、最初の`Case`式が`E1`、2 番目`Case`式が`E2`、`Case`が評価されるいずれか`E = E1`(場合ありません`E2`を指定) または`(E >= E1) And (E <= E2)`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-423">If the `Select` expression is `E`, the first `Case` expression is `E1`, and the second `Case` expression is `E2`, the `Case` is evaluated either as `E = E1` (if no `E2` is specified) or `(E >= E1) And (E <= E2)`.</span></span> <span data-ttu-id="9d4b6-424">演算子は、2 つの式の型に対して有効である必要があります。それ以外の場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-424">The operators must be valid for the types of the two expressions; otherwise a compile-time error occurs.</span></span>


## <a name="loop-statements"></a><span data-ttu-id="9d4b6-425">Loop ステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-425">Loop Statements</span></span>

<span data-ttu-id="9d4b6-426">Loop ステートメントは、本体で、ステートメントの実行を繰り返すを許可します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-426">Loop statements allow repeated execution of the statements in their body.</span></span>

```antlr
LoopStatement
    : WhileStatement
    | DoLoopStatement
    | ForStatement
    | ForEachStatement
    ;
```

<span data-ttu-id="9d4b6-427">ループの本体が入力されるたびに、変数の前の値に初期化され、その本体で宣言されたすべてのローカル変数の新しいコピーが行われました。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-427">Each time a loop body is entered, a fresh copy is made of all local variables declared in that body, initialized to the previous values of the variables.</span></span> <span data-ttu-id="9d4b6-428">ループ本体の内部変数への参照では、最近作成されたコピーを使用します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-428">Any reference to a variable within the loop body will use the most recently made copy.</span></span> <span data-ttu-id="9d4b6-429">このコード例に示します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-429">This code shows an example:</span></span>

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

<span data-ttu-id="9d4b6-430">コードでは、出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-430">The code produces the output:</span></span>

```
31    32    33
```

<span data-ttu-id="9d4b6-431">ループの本体が実行されたときに、変数のどちらのコピーが現在を使用します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-431">When the loop body is executed, it uses whichever copy of the variable is current.</span></span> <span data-ttu-id="9d4b6-432">たとえば、ステートメント`Dim y = x`の最新のコピーを指す`y`の元のコピーと`x`。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-432">For example, the statement  `Dim y = x` refers to the latest copy of `y` and the original copy of `x`.</span></span> <span data-ttu-id="9d4b6-433">変数のどちらのコピーが作成された時点の最新記憶ラムダが作成されるとします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-433">And when a lambda is created, it remembers whichever copy of a variable was current at the time it was created.</span></span> <span data-ttu-id="9d4b6-434">そのため、各ラムダが同じ共有のコピーを使用`x`の異なるコピーが`y`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-434">Therefore, each lambda uses the same shared copy of `x`, but a different copy of `y`.</span></span> <span data-ttu-id="9d4b6-435">プログラムの最後に、ラムダ、実行時にその共有のコピー`x`最終値 3 にようになりましたが、これらはすべてを参照してください。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-435">At the end of the program, when it executes the lambdas, that shared copy of `x` that they all refer to is now at its final value 3.</span></span>

<span data-ttu-id="9d4b6-436">ラムダまたは LINQ 式がない場合、しないループに入った新しいコピーが行われたことを認識することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-436">Note that if there are no lambdas or LINQ expressions, then it's impossible to know that a fresh copy is made on loop entry.</span></span> <span data-ttu-id="9d4b6-437">実際には、この場合、コピーを作成するコンパイラの最適化を回避できます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-437">Indeed, compiler optimizations will avoid making copies in this case.</span></span> <span data-ttu-id="9d4b6-438">正式にはないことに注意すぎる`GoTo`ラムダまたは LINQ 式を含むループにします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-438">Note too that it's illegal to `GoTo` into a loop that contains lambdas or LINQ expressions.</span></span>


### <a name="whileend-while-and-doloop-statements"></a><span data-ttu-id="9d4b6-439">しばらくしています.終了中に、操作を実行しています.Loop ステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-439">While...End While and Do...Loop Statements</span></span>

<span data-ttu-id="9d4b6-440">A`While`または`Do`ブール式に基づくステートメントのループをループします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-440">A `While` or `Do` loop statement loops based on a Boolean expression.</span></span>

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

<span data-ttu-id="9d4b6-441">A`While`ループ ステートメントのループを true に評価されるブール式であれば、`Do`ループ ステートメントより複雑な条件を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-441">A `While` loop statement loops as long as the Boolean expression evaluates to true; a `Do` loop statement may contain a more complex condition.</span></span> <span data-ttu-id="9d4b6-442">式は、後に配置することがあります、`Do`キーワード以降、`Loop`キーワードが両方の後です。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-442">An expression may be placed after the `Do` keyword or after the `Loop` keyword, but not after both.</span></span> <span data-ttu-id="9d4b6-443">セクションに従って、ブール式が評価される[ブール式](expressions.md#boolean-expressions)します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-443">The Boolean expression is evaluated as per Section [Boolean Expressions](expressions.md#boolean-expressions).</span></span> <span data-ttu-id="9d4b6-444">(注: ブール型を持つ式は必要ありません)。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-444">(Note: this does not require the expression to have Boolean type).</span></span> <span data-ttu-id="9d4b6-445">式をまったく; 指定せずに有効です。その場合は、ループは終了しません。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-445">It is also valid to specify no expression at all; in that case, the loop will never exit.</span></span> <span data-ttu-id="9d4b6-446">式が後に配置されている場合`Do`、各イテレーションでループ ブロックが実行される前に評価されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-446">If the expression is placed after `Do`, it will be evaluated before the loop block is executed on each iteration.</span></span> <span data-ttu-id="9d4b6-447">式が後に配置されている場合`Loop`、各イテレーションでループ ブロックが実行された後に評価されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-447">If the expression is placed after `Loop`, it will be evaluated after the loop block has executed on each iteration.</span></span> <span data-ttu-id="9d4b6-448">後の式を配置する`Loop`より後に配置の 1 つ以上ループが生成されますので`Do`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-448">Placing the expression after `Loop` will therefore generate one more loop than placement after `Do`.</span></span> <span data-ttu-id="9d4b6-449">次の例では、この動作を示します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-449">The following example demonstrates this behavior:</span></span>

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

<span data-ttu-id="9d4b6-450">コードでは、出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-450">The code produces the output:</span></span>

```
Second Loop
```

<span data-ttu-id="9d4b6-451">最初のループの場合は、ループが実行される前に、条件が評価されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-451">In the case of the first loop, the condition is evaluated before the loop executes.</span></span> <span data-ttu-id="9d4b6-452">2 つ目のループの場合、条件は、ループの実行後に実行されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-452">In the case of the second loop, the condition is executed after the loop executes.</span></span> <span data-ttu-id="9d4b6-453">条件式は、いずれかが付く必要があります、`While`キーワードまたは`Until`キーワード。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-453">The conditional expression must be prefixed with either a `While` keyword or an `Until` keyword.</span></span> <span data-ttu-id="9d4b6-454">前者に停止条件の評価が false の条件が true に評価された場合の後者の場合のループをします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-454">The former breaks the loop if the condition evaluates to false, the latter when the condition evaluates to true.</span></span>

<span data-ttu-id="9d4b6-455">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="9d4b6-455">__Note.__</span></span> <span data-ttu-id="9d4b6-456">`Until` 予約語ではありません。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-456">`Until` is not a reserved word.</span></span>


### <a name="fornext-statements"></a><span data-ttu-id="9d4b6-457">.次のステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-457">For...Next Statements</span></span>

<span data-ttu-id="9d4b6-458">A`For...Next`ステートメントのループの境界のセットに基づいています。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-458">A `For...Next` statement loops based on a set of bounds.</span></span> <span data-ttu-id="9d4b6-459">A`For`ステートメントは、ループ コントロール変数を下限の境界の式、上限式、および省略可能な手順の値式を指定します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-459">A `For` statement specifies a loop control variable, a lower bound expression, an upper bound expression, and an optional step value expression.</span></span> <span data-ttu-id="9d4b6-460">ループ コントロール変数は省略可能な識別子を使用するか後に指定された`As`句、または式。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-460">The loop control variable is specified either through an identifier followed by an optional `As` clause or an expression.</span></span>

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

<span data-ttu-id="9d4b6-461">これを特定、新しいローカル変数のいずれかループ制御変数によって、次の規則に従って参照`For...Next`ステートメント、または既存の変数または式にします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-461">As per the following rules, the loop control variable refers either to a new local variable specific to this `For...Next` statement, or to a pre-existing variable, or to an expression.</span></span>

* <span data-ttu-id="9d4b6-462">ループ コントロール変数が識別子の場合、`As`句で指定された型の場合は、新しいローカル変数を定義する識別子、`As`全体をスコープ句`For`ループします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-462">If the loop control variable is an identifier with an `As` clause, the identifier defines a new local variable of the type specified in the `As` clause, scoped to the entire `For` loop.</span></span>

* <span data-ttu-id="9d4b6-463">ループ コントロール変数がない識別子、`As`句では、その識別子は、まずを使用して解決単純な名前解決ルール (セクションを参照して[名前の単純式](expressions.md#simple-name-expressions))、後、例外をこの出現識別子自体は、暗黙的なローカル変数を作成する (セクション[暗黙のローカル宣言](statements.md#implicit-local-declarations))。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-463">If the loop control variable is an identifier without an `As` clause, then the identifier is first resolved using the simple name resolution rules (see Section [Simple Name Expressions](expressions.md#simple-name-expressions)), excepting that this occurrence of the identifier would not in and of itself cause an implicit local variable to be created (Section [Implicit Local Declarations](statements.md#implicit-local-declarations)).</span></span>

  * <span data-ttu-id="9d4b6-464">この解決が成功し、結果は変数として分類、ループ コントロール変数はその既存の変数です。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-464">If this resolution succeeds and the result is classified as a variable, then the loop control variable is that pre-existing variable.</span></span>

  * <span data-ttu-id="9d4b6-465">解決に失敗した場合、または解決が成功し、結果は、型として分類されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-465">If resolution fails, or if resolution succeeds and the result is classified as a type, then:</span></span>
    * <span data-ttu-id="9d4b6-466">ローカル変数の型の推定を使用している場合、識別子は、バインドから推論する型を持つ新しいローカル変数を定義します全体をスコープ式のステップと`For`ループです。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-466">if local variable type inference is being used, the identifier defines a new local variable whose type is inferred from the bound and step expressions, scoped to the entire `For` loop;</span></span>
    * <span data-ttu-id="9d4b6-467">ローカル変数の型の推定が使用されていないかどうかは暗黙のローカル宣言は、スコープのすべてのメソッドは、ローカルの暗黙的な変数を作成 (セクション[暗黙のローカル宣言](statements.md#implicit-local-declarations))、およびループ制御変数この既存の変数では; を参照します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-467">if local variable type inference is not being used but implicit local declaration is, then an implicit local variable is created whose scope is the entire method (Section [Implicit Local Declarations](statements.md#implicit-local-declarations)), and the loop control variable refers to this pre-existing variable;</span></span>
    * <span data-ttu-id="9d4b6-468">ローカル変数の型の推論も暗黙のローカル宣言を使用している場合は、エラーです。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-468">if neither local variable type inference nor implicit local declarations are used, it is an error.</span></span>

  * <span data-ttu-id="9d4b6-469">解像度は、型でも変数として分類されるものに成功すると、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-469">If resolution succeeds with something classified as neither a type nor a variable, it is an error.</span></span>

* <span data-ttu-id="9d4b6-470">ループ コントロール変数が式の場合は、式を変数として分類する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-470">If the loop control variable is an expression, the expression must be classified as a variable.</span></span>

<span data-ttu-id="9d4b6-471">もう 1 つの外側でループ コントロール変数は使用できません`For...Next`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-471">A loop control variable cannot be used by another enclosing `For...Next` statement.</span></span> <span data-ttu-id="9d4b6-472">ループ コントロール変数の型を`For`ステートメント、イテレーションの種類を決定しのいずれかを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-472">The type of the loop control variable of a `For` statement determines the type of the iteration and must be one of:</span></span>

* <span data-ttu-id="9d4b6-473">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`</span><span class="sxs-lookup"><span data-stu-id="9d4b6-473">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`</span></span>
* <span data-ttu-id="9d4b6-474">列挙型</span><span class="sxs-lookup"><span data-stu-id="9d4b6-474">An enumerated type</span></span>
* `Object`
* <span data-ttu-id="9d4b6-475">型`T`、次の演算子を持つ場所`B`ブール式で使用できる型には。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-475">A type `T` that has the following operators, where `B` is a type that can be used in a Boolean expression:</span></span>

```vb
Public Shared Operator >= (op1 As T, op2 As T) As B
Public Shared Operator <= (op1 As T, op2 As T) As B
Public Shared Operator - (op1 As T, op2 As T) As T
Public Shared Operator + (op1 As T, op2 As T) As T
```

<span data-ttu-id="9d4b6-476">バインドされていると、ステップ式は、ループ コントロール変数の型に暗黙的に変換できる必要があり、値として分類する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-476">The bound and step expressions must be implicitly convertible to the type of the loop control variable and must be classified as values.</span></span> <span data-ttu-id="9d4b6-477">コンパイル時に、下限の境界、上限、およびステップ式の型の間で最も広い種類を選択して、ループ コントロール変数の型が推論されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-477">At compile time, the type of the loop control variable is inferred by choosing the widest type among the lower bound, upper bound, and step expression types.</span></span> <span data-ttu-id="9d4b6-478">2 つの種類の間に拡大変換がない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-478">If there is no widening conversion between two of the types, then a compile-time error occurs.</span></span>

<span data-ttu-id="9d4b6-479">実行時に、ループ コントロール変数の型が場合`Object`イテレーションの型を推論し、2 つの例外は、コンパイル時と同じです。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-479">At run time, if the type of the loop control variable is `Object`, then the type of the iteration is inferred the same as at compile time, with two exceptions.</span></span> <span data-ttu-id="9d4b6-480">最初の場合、バインドされたステップ式が整数型のすべてが最も幅の広い型を持つありませんし、3 種類すべてを包含している最も幅の広い型が推論されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-480">First, if the bound and step expressions are all of integral types but have no widest type, then the widest type that encompasses all three types will be inferred.</span></span> <span data-ttu-id="9d4b6-481">2 番目にループ コントロール変数の型が推定された場合、 `String`、`Double`代わりに推論されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-481">And second, if the type of the loop control variable is inferred to be `String`, `Double` will be inferred instead.</span></span> <span data-ttu-id="9d4b6-482">式は、ループ コントロール型に変換できない場合、実行時にループ制御の種類を決定できない場合、または、`System.InvalidCastException`が発生します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-482">If, at run time, no loop control type can be determined or if any of the expressions cannot be converted to the loop control type, a `System.InvalidCastException` will occur.</span></span> <span data-ttu-id="9d4b6-483">ループの先頭にループ コントロールの種類を選択すると、同じ型がループ コントロール変数の値に加えられた変更に関係なく、イテレーション全体で使用されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-483">Once a loop control type has been chosen at the beginning of the loop, the same type will be used throughout the iteration, regardless of changes made to the value in the loop control variable.</span></span>

<span data-ttu-id="9d4b6-484">A`For`ステートメントは、対応するを閉じる必要があります`Next`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-484">A `For` statement must be closed by a matching `Next` statement.</span></span> <span data-ttu-id="9d4b6-485">A`Next`変数を指定しないステートメントに一致する最も内側のオープン`For`ステートメントでは中、`Next`ステートメントの 1 つまたは複数のループ コントロール変数は、左から右に、対応、`For`ループの各変数に一致します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-485">A `Next` statement without a variable matches the innermost open `For` statement, while a `Next` statement with one or more loop control variables will, from left to right, match the `For` loops that match each variable.</span></span> <span data-ttu-id="9d4b6-486">変数と一致する場合、`For`最も内側のループがその時点ではないループでは、コンパイル時エラーになります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-486">If a variable matches a `For` loop that is not the most nested loop at that point, a compile-time error results.</span></span>

<span data-ttu-id="9d4b6-487">ループの先頭には、3 つの式テキストの順序で評価して下限の境界の式は、ループ コントロール変数に割り当てられています。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-487">At the beginning of the loop, the three expressions are evaluated in textual order and the lower bound expression is assigned to the loop control variable.</span></span> <span data-ttu-id="9d4b6-488">リテラルは暗黙的に増分値を省略すると、 `1`, ループ コントロール変数の型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-488">If the step value is omitted, it is implicitly the literal `1`, converted to the type of the loop control variable.</span></span> <span data-ttu-id="9d4b6-489">3 つの式は、ループの先頭にのみ評価されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-489">The three expressions are only ever evaluated at the beginning of the loop.</span></span>

<span data-ttu-id="9d4b6-490">各ループの先頭が終点を超えるステップ式が負の場合、ステップ式が正の値、または終了点よりも小さい場合に、コントロール変数と比較されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-490">At the beginning of each loop, the control variable is compared to see if it is greater than the end point if the step expression is positive, or less than the end point if the step expression is negative.</span></span> <span data-ttu-id="9d4b6-491">その場合、`For`ループを終了します。 それ以外の場合、ループのブロックを実行します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-491">If it is, the `For` loop terminates; otherwise the loop block executes.</span></span> <span data-ttu-id="9d4b6-492">ループ コントロール変数がプリミティブ型ではない場合、比較演算子は、かどうかによって決まります式`step >= step - step`が true または false。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-492">If the loop control variable is not a primitive type, the comparison operator is determined by whether the expression `step >= step - step` is true or false.</span></span> <span data-ttu-id="9d4b6-493">`Next`ステートメントでは、ステップ値がコントロール変数に追加し、実行、ループの先頭に戻ります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-493">At the `Next` statement, the step value is added to the control variable and execution returns to the top of the loop.</span></span>

<span data-ttu-id="9d4b6-494">ループ コントロール変数の新しいコピーは*いない*ループ ブロックの各反復処理で作成します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-494">Note that a new copy of the loop control variable is *not* created on each iteration of the loop block.</span></span> <span data-ttu-id="9d4b6-495">この点で、`For`ステートメントとは異なります`For Each`(セクション[For次のステートメント](statements.md#for-eachnext-statements))。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-495">In this respect, the `For` statement differs from `For Each` (Section [For Each...Next Statements](statements.md#for-eachnext-statements)).</span></span>

<span data-ttu-id="9d4b6-496">分岐することはできません、`For`ループの外側のループ処理します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-496">It is not valid to branch into a `For` loop from outside the loop.</span></span>


### <a name="for-eachnext-statements"></a><span data-ttu-id="9d4b6-497">各.次のステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-497">For Each...Next Statements</span></span>

<span data-ttu-id="9d4b6-498">A`For Each...Next`ステートメントのループ式内の要素に基づいています。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-498">A `For Each...Next` statement loops based on the elements in an expression.</span></span> <span data-ttu-id="9d4b6-499">A`For Each`ステートメントは、ループ コントロール変数と列挙子の式を指定します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-499">A `For Each` statement specifies a loop control variable and an enumerator expression.</span></span> <span data-ttu-id="9d4b6-500">ループ コントロール変数は省略可能な識別子を使用するか後に指定された`As`句、または式。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-500">The loop control variable is specified either through an identifier followed by an optional `As` clause or an expression.</span></span>

```antlr
ForEachStatement
    : 'For' 'Each' LoopControlVariable 'In' LineTerminator? Expression StatementTerminator
      Block?
      ( 'Next' NextExpressionList? StatementTerminator )?
    ;
```

<span data-ttu-id="9d4b6-501">次のと同じ規則`For...Next`ステートメント (セクション[の..。次のステートメント](statements.md#fornext-statements))、ループ コントロール変数を指す特定新しいローカル変数にこのごとにしています.次のステートメント、または既存の変数または式にします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-501">Following the same rules as `For...Next` statements (Section [For...Next Statements](statements.md#fornext-statements)), the loop control variable refers either to a new local variable specific to this For Each...Next statement, or to a pre-existing variable, or to an expression.</span></span>

<span data-ttu-id="9d4b6-502">列挙子式は、値として分類する必要があり、その型はコレクション型である必要がありますまたは`Object`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-502">The enumerator expression must be classified as a value and its type must be a collection type or `Object`.</span></span> <span data-ttu-id="9d4b6-503">列挙子の式の型が場合`Object`、すべての処理が実行時まで延期されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-503">If the type of the enumerator expression is `Object`, then all processing is deferred until run-time.</span></span> <span data-ttu-id="9d4b6-504">それ以外の場合、変換は、コレクションの要素型、ループ コントロール変数の型に存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-504">Otherwise, a conversion must exist from the element type of the collection to the type of the loop control variable.</span></span>

<span data-ttu-id="9d4b6-505">もう 1 つの外側でループ コントロール変数は使用できません`For Each`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-505">The loop control variable cannot be used by another enclosing `For Each` statement.</span></span> <span data-ttu-id="9d4b6-506">A`For Each`ステートメントは、対応するを閉じる必要があります`Next`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-506">A `For Each` statement must be closed by a matching `Next` statement.</span></span> <span data-ttu-id="9d4b6-507">A`Next`ループ コントロール変数を指定しないステートメントに一致する最も内側のオープン`For Each`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-507">A `Next` statement without a loop control variable matches the innermost open `For Each`.</span></span> <span data-ttu-id="9d4b6-508">A`Next`ステートメントの 1 つまたは複数のループ コントロール変数は、左から右に、対応、`For Each`を同じループ コントロール変数を持つループします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-508">A `Next` statement with one or more loop control variables will, from left to right, match the `For Each` loops that have the same loop control variable.</span></span> <span data-ttu-id="9d4b6-509">変数と一致する場合、`For Each`最も内側のループがその時点ではないループでは、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-509">If a variable matches a `For Each` loop that is not the most nested loop at that point, a compile-time error occurs.</span></span>

<span data-ttu-id="9d4b6-510">型`C`と呼ばれます、*コレクション型*場合は、1 つの。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-510">A type `C` is said to be a *collection type* if one of:</span></span>

* <span data-ttu-id="9d4b6-511">次のすべてが該当します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-511">All of the following are true:</span></span>
  * <span data-ttu-id="9d4b6-512">`C` 共有のアクセス可能なインスタンスまたはシグネチャを持つ拡張メソッドが含まれる`GetEnumerator()`型を返す`E`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-512">`C` contains an accessible instance, shared or extension method with the signature `GetEnumerator()` that returns a type `E`.</span></span>
  * <span data-ttu-id="9d4b6-513">`E` 共有のアクセス可能なインスタンスまたはシグネチャを持つ拡張メソッドが含まれる`MoveNext()`と戻り値の型`Boolean`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-513">`E` contains an accessible instance, shared or extension method with the signature `MoveNext()` and the return type `Boolean`.</span></span>
  * <span data-ttu-id="9d4b6-514">`E` アクセス可能なインスタンスまたは名前付き共有のプロパティが含まれる`Current`getter を持ちます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-514">`E` contains an accessible instance or shared property named `Current` that has a getter.</span></span> <span data-ttu-id="9d4b6-515">このプロパティの型は、コレクション型の要素型です。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-515">The type of this property is the element type of the collection type.</span></span>

* <span data-ttu-id="9d4b6-516">インターフェイスを実装する`System.Collections.Generic.IEnumerable(Of T)`、その場合、コレクションの要素の型と見なされます`T`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-516">It implements the interface `System.Collections.Generic.IEnumerable(Of T)`, in which case the element type of the collection is considered to be `T`.</span></span>

* <span data-ttu-id="9d4b6-517">インターフェイスを実装する`System.Collections.IEnumerable`、その場合、コレクションの要素の型と見なされます`Object`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-517">It implements the interface `System.Collections.IEnumerable`, in which case the element type of the collection is considered to be `Object`.</span></span>

<span data-ttu-id="9d4b6-518">列挙できるクラスの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-518">Following is an example of a class that can be enumerated:</span></span>

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

<span data-ttu-id="9d4b6-519">ループが開始する前に、列挙子の式が評価されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-519">Before the loop begins, the enumerator expression is evaluated.</span></span> <span data-ttu-id="9d4b6-520">式の型は、設計パターンを満たさない場合、式にキャストされます`System.Collections.IEnumerable`または`System.Collections.Generic.IEnumerable(Of T)`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-520">If the type of the expression does not satisfy the design pattern, then the expression is cast to `System.Collections.IEnumerable` or `System.Collections.Generic.IEnumerable(Of T)`.</span></span> <span data-ttu-id="9d4b6-521">式の型は、ジェネリック インターフェイスを実装する場合は、ジェネリック インターフェイスは、コンパイル時に優先が実行時に、非ジェネリック インターフェイスをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-521">If the expression type implements the generic interface, the generic interface is preferred at compile-time but the non-generic interface is preferred at run-time.</span></span> <span data-ttu-id="9d4b6-522">式の型は、複数回ジェネリック インターフェイスを実装する場合、ステートメントがあいまいと見なされ、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-522">If the expression type implements the generic interface multiple times, the statement is considered ambiguous and a compile-time error occurs.</span></span>

<span data-ttu-id="9d4b6-523">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="9d4b6-523">__Note.__</span></span> <span data-ttu-id="9d4b6-524">インターフェイス メソッドに、すべての呼び出しで型パラメーターが含まれることを意味ジェネリック インターフェイスを選択するため、遅延バインドされた場合は、非ジェネリック インターフェイスが優先されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-524">The non-generic interface is preferred in the late bound case, because picking the generic interface would mean that all the calls to the interface methods would involve type parameters.</span></span> <span data-ttu-id="9d4b6-525">このようなすべての呼び出しにする必要がありますので、一致する実行時引数の型を把握することはできません、遅延バインディング呼び出しを使用します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-525">Since it is not possible to know the matching type arguments at run-time, all such calls would have to be made using late-bound calls.</span></span> <span data-ttu-id="9d4b6-526">これは、コンパイル時の呼び出しを使用して、非ジェネリック インターフェイスを呼び出すことがあるために、非ジェネリック インターフェイスを呼び出すよりも低速になります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-526">This would be slower than calling the non-generic interface because the non-generic interface could be called using compile-time calls.</span></span>

<span data-ttu-id="9d4b6-527">`GetEnumerator` 結果の値と戻り値で呼び出される関数の値を一時的に格納されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-527">`GetEnumerator` is called on the resulting value and the return value of the function is stored in a temporary.</span></span> <span data-ttu-id="9d4b6-528">その後、各反復処理の先頭に`MoveNext`が一時的なで呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-528">Then at the beginning of each iteration, `MoveNext` is called on the temporary.</span></span> <span data-ttu-id="9d4b6-529">返された場合`False`ループを終了します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-529">If it returns `False`, the loop terminates.</span></span> <span data-ttu-id="9d4b6-530">それ以外の場合、ループの各反復処理には、次のように実行されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-530">Otherwise, each iteration of the loop is executed as follows:</span></span>

1. <span data-ttu-id="9d4b6-531">ループ コントロール変数には、(既存の 1 つではなく) 新しいローカル変数が識別される、このローカル変数の新しいコピーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-531">If the loop control variable identified a new local variable (rather than a pre-existing one), then a fresh copy of this local variable is created.</span></span> <span data-ttu-id="9d4b6-532">現在のイテレーションがループ ブロック内のすべての参照はこのコピーを参照してください。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-532">For the current iteration, all references within the loop block will refer to this copy.</span></span>
2. <span data-ttu-id="9d4b6-533">`Current`プロパティを取得すると、(かどうか、変換が暗黙的または明示的な) に関係なくループ コントロール変数の型に強制変換、ループ コントロール変数に割り当てられているとします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-533">The `Current` property is retrieved, coerced to the type of the loop control variable (regardless of whether the conversion is implicit or explicit), and assigned to the loop control variable.</span></span>
3. <span data-ttu-id="9d4b6-534">ループのブロックを実行します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-534">The loop block executes.</span></span>

<span data-ttu-id="9d4b6-535">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="9d4b6-535">__Note.__</span></span> <span data-ttu-id="9d4b6-536">Version 10.0 および 11.0、言語の間における動作のわずかな変更があります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-536">There is a slight change in behavior between version 10.0 and 11.0 of the language.</span></span> <span data-ttu-id="9d4b6-537">新しいイテレーション変数が 11.0、前に*いない*ループの反復ごとに作成します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-537">Prior to 11.0, a fresh iteration variable was *not* created for each iteration of the loop.</span></span> <span data-ttu-id="9d4b6-538">この違いは、繰り返し変数がラムダ式、または、ループの後に呼び出され、LINQ 式によってキャプチャされた場合にのみ、観測可能なオブジェクトを示します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-538">This difference is observable only if the iteration variable is captured by a lambda or a LINQ expression which is then invoked after the loop:</span></span>

```vb
Dim lambdas As New List(Of Action)
For Each x In {1,2,3}
   lambdas.Add(Sub() Console.WriteLine(x)
Next
lambdas(0).Invoke()
lambdas(1).Invoke()
lambdas(2).Invoke()
```

<span data-ttu-id="9d4b6-539">Visual Basic 10.0、最高の コンパイル時に警告を生成および「3」印刷この 3 回です。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-539">Up to Visual Basic 10.0, this produced a warning at compile-time and printed "3" three times.</span></span> <span data-ttu-id="9d4b6-540">1 つの変数"x"は、ループのすべてのイテレーションで共有のみが、すべての 3 つのラムダ キャプチャ同じ"x"と、ラムダが実行された時間にし、保持数 3 ためにでした。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-540">That was because there was only a single variable "x" shared by all iterations of the loop, and all three lambdas captured the same "x", and by the time the lambdas were executed it then held the number 3.</span></span> <span data-ttu-id="9d4b6-541">Visual Basic の 11.0 の時点では、「1, 2, 3」が出力されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-541">As of Visual Basic 11.0, it prints "1, 2, 3".</span></span> <span data-ttu-id="9d4b6-542">各ラムダは、別の変数"x"をキャプチャするためです。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-542">That is because each lambda captures a different variable "x".</span></span>


<span data-ttu-id="9d4b6-543">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="9d4b6-543">__Note.__</span></span> <span data-ttu-id="9d4b6-544">イテレーションの現在の要素は、ステートメントの変換演算子を導入する便利な場所がないために、変換は明示的な場合でも、ループ コントロール変数の型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-544">The current element of the iteration is converted to the type of the loop control variable even if the conversion is explicit because there is no convenient place to introduce a conversion operator in the statement.</span></span> <span data-ttu-id="9d4b6-545">ここで廃止された型を使用する場合に特に問題になるこうして`System.Collections.ArrayList`その要素型であるため、`Object`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-545">This became particularly troublesome when working with the now-obsolete type `System.Collections.ArrayList`, because its element type is `Object`.</span></span> <span data-ttu-id="9d4b6-546">これが必要なキャストで優れた多くループは、理想的と考えたものでした。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-546">This would have required casts in a great many loops, something we felt was not ideal.</span></span> <span data-ttu-id="9d4b6-547">皮肉にも、ジェネリックは、厳密に型指定されたコレクションの作成を有効になって`System.Collections.Generic.List(Of T)`を加えた可能性のあるこの設計上のポイントを考え直すことが、互換性のために、これは変更できませんようになりました。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-547">Ironically, generics enabled the creation of a strongly-typed collection, `System.Collections.Generic.List(Of T)`, which might have made us rethink this design point, but for compatibility's sake, this cannot be changed now.</span></span>


<span data-ttu-id="9d4b6-548">ときに、`Next`ステートメントに達すると、実行をループの先頭に戻ります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-548">When the `Next` statement is reached, execution returns to the top of the loop.</span></span> <span data-ttu-id="9d4b6-549">後に変数が指定されている場合、`Next`キーワード、する必要がありますの後の最初の変数と同じ、`For Each`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-549">If a variable is specified after the `Next` keyword, it must be the same as the first variable after the `For Each`.</span></span> <span data-ttu-id="9d4b6-550">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-550">For example, consider the following code:</span></span>

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

<span data-ttu-id="9d4b6-551">これは、次のコードに相当します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-551">It is equivalent to the following code:</span></span>

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

<span data-ttu-id="9d4b6-552">場合、型`E`列挙子の実装`System.IDisposable`、呼び出すことによって、ループの終了時に、列挙子が破棄された後、`Dispose`メソッド。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-552">If the type `E` of the enumerator implements `System.IDisposable`, then the enumerator is disposed upon exiting the loop by calling the `Dispose` method.</span></span> <span data-ttu-id="9d4b6-553">これにより、列挙子によって保持されているリソースが解放されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-553">This ensures that resources held by the enumerator are released.</span></span> <span data-ttu-id="9d4b6-554">場合、メソッドを含む、`For Each`ステートメントは、非構造化エラー処理を使用していない、`For Each`にステートメントがラップされて、`Try`ステートメントを`Dispose`呼び出されるメソッド、`Finally`クリーンアップを確実に。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-554">If the method containing the `For Each` statement does not use unstructured error handling, then the `For Each` statement is wrapped in a `Try` statement with the `Dispose` method called in the `Finally` to ensure cleanup.</span></span>

<span data-ttu-id="9d4b6-555">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="9d4b6-555">__Note.__</span></span> <span data-ttu-id="9d4b6-556">`System.Array`型がコレクション型と型がから派生するすべての配列から`System.Array`で、配列型の式が許可されている、`For Each`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-556">The `System.Array` type is a collection type, and since all array types derive from `System.Array`, any array type expression is permitted in a `For Each` statement.</span></span> <span data-ttu-id="9d4b6-557">1 次元配列の場合、`For Each`ステートメントは、インデックス 0 から始まるインデックスの長さ - 1 で終わるインデックスの昇順に配列の要素を列挙します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-557">For single-dimensional arrays, the `For Each` statement enumerates the array elements in increasing index order, starting with index 0 and ending with index Length - 1.</span></span> <span data-ttu-id="9d4b6-558">多次元配列は、まず右端の次元のインデックスを増加します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-558">For multidimensional arrays, the indices of the rightmost dimension are increased first.</span></span>

<span data-ttu-id="9d4b6-559">たとえば、次のコードを出力`1 2 3 4`:</span><span class="sxs-lookup"><span data-stu-id="9d4b6-559">For example, the following code prints `1 2 3 4`:</span></span>

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

<span data-ttu-id="9d4b6-560">分岐することはできません、`For Each`からブロックの外側のステートメント ブロックです。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-560">It is not valid to branch into a `For Each` statement block from outside the block.</span></span>


## <a name="exception-handling-statements"></a><span data-ttu-id="9d4b6-561">例外処理ステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-561">Exception-Handling Statements</span></span>

<span data-ttu-id="9d4b6-562">Visual Basic では、構造化例外処理と非構造化例外処理をサポートします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-562">Visual Basic supports structured exception handling and unstructured exception handling.</span></span> <span data-ttu-id="9d4b6-563">メソッドでは、例外処理の 1 つだけのスタイルを使用する可能性がありますが、`Error`構造化例外処理でステートメントを使用することがあります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-563">Only one style of exception handling may be used in a method, but the `Error` statement may be used in structured exception handling.</span></span> <span data-ttu-id="9d4b6-564">メソッドは、例外処理の両方のスタイルを使用している場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-564">If a method uses both styles of exception handling, a compile-time error results.</span></span>

```antlr
ErrorHandlingStatement
    : StructuredErrorStatement
    | UnstructuredErrorStatement
    ;
```

### <a name="structured-exception-handling-statements"></a><span data-ttu-id="9d4b6-565">構造化例外処理ステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-565">Structured Exception-Handling Statements</span></span>

<span data-ttu-id="9d4b6-566">構造化例外処理は、特定の例外の処理を明示的なブロックを宣言することでエラーの処理の方法です。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-566">Structured exception handling is a method of handling errors by declaring explicit blocks within which certain exceptions will be handled.</span></span> <span data-ttu-id="9d4b6-567">構造化例外処理を行う、`Try`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-567">Structured exception handling is done through a `Try` statement.</span></span>

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


<span data-ttu-id="9d4b6-568">例:</span><span class="sxs-lookup"><span data-stu-id="9d4b6-568">For example:</span></span>

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

<span data-ttu-id="9d4b6-569">A`Try`ブロックの 3 種類のステートメントから成る: try ブロック、catch ブロックおよびブロックの最後にします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-569">A `Try` statement is made up of three kinds of blocks: try blocks, catch blocks, and finally blocks.</span></span> <span data-ttu-id="9d4b6-570">A *try ブロック*ステートメント ブロックに実行されるステートメントを含むです。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-570">A *try block* is a statement block that contains the statements to be executed.</span></span> <span data-ttu-id="9d4b6-571">A *catch ブロック*例外処理ステートメント ブロックです。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-571">A *catch block* is a statement block that handles an exception.</span></span> <span data-ttu-id="9d4b6-572">A *finally ブロック*ステートメント ブロックが実行されるステートメントを含む、`Try`例外が発生し、処理されたかどうかに関係なく、ステートメントが終了しました。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-572">A *finally block* is a statement block that contains statements to be run when the `Try` statement is exited, regardless of whether an exception has occurred and been handled.</span></span> <span data-ttu-id="9d4b6-573">A`Try`のみ含めることができる 1 つの try ブロックと 1 つ最後に、ステートメントを少なくとも 1 つの catch ブロックを含めることがまたは、finally ブロックする必要があります、ブロックします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-573">A `Try` statement, which can only contain one try block and one finally block, must contain at least one catch block or finally block.</span></span> <span data-ttu-id="9d4b6-574">Try ブロックに以外から、同じステートメント内の catch ブロック内で実行を明示的に転送することはしません。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-574">It is invalid to explicitly transfer execution into a try block except from within a catch block in the same statement.</span></span>


#### <a name="finally-blocks"></a><span data-ttu-id="9d4b6-575">Finally ブロック</span><span class="sxs-lookup"><span data-stu-id="9d4b6-575">Finally Blocks</span></span>

<span data-ttu-id="9d4b6-576">A`Finally`ブロックは常に実行がの一部を離れるときに実行、`Try`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-576">A `Finally` block is always executed when execution leaves any part of the `Try` statement.</span></span> <span data-ttu-id="9d4b6-577">実行する明示的なアクションは必要ありません、`Finally`ブロックは実行から離したときに、`Try`ステートメントでは、システムが自動的に実行、`Finally`をブロックし、その目的の宛先に実行を転送します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-577">No explicit action is required to execute the `Finally` block; when execution leaves the `Try` statement, the system will automatically execute the `Finally` block and then transfer execution to its intended destination.</span></span> <span data-ttu-id="9d4b6-578">`Finally`実行のままに関係なくブロックが実行される、`Try`ステートメント: の終わりまで、`Try`の終わりまでのブロックを`Catch`を通じて、ブロック、`Exit Try`ステートメントを使用して、 `GoTo`ステートメント、またはスローされた例外を処理しないことで。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-578">The `Finally` block is executed regardless of how execution leaves the `Try` statement: through the end of the `Try` block, through the end of a `Catch` block, through an `Exit Try` statement, through a `GoTo` statement, or by not handling a thrown exception.</span></span>

<span data-ttu-id="9d4b6-579">なお、 `Await` 、非同期メソッド内の式と`Yield`反復子メソッドでステートメント非同期または反復子メソッド インスタンスの中断およびいくつかその他のメソッドのインスタンスで再開する制御フローが発生することができます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-579">Note that the `Await` expression in an async method, and the `Yield` statement in an iterator method, can cause flow of control to suspend in the async or iterator method instance and resume in some other method instance.</span></span> <span data-ttu-id="9d4b6-580">ただし、この実行の中断だけでは、それぞれ async メソッドまたは iterator メソッド インスタンスを終了するには関与せず、ため発生しません`Finally`実行をブロックします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-580">However, this is merely a suspension of execution and does not involve exiting the respective async method or iterator method instance, and so does not cause `Finally` blocks to be executed.</span></span>

<span data-ttu-id="9d4b6-581">実行を明示的に転送することはできません、 `Finally` ; ブロックも転送の実行には無効です、`Finally`例外を除くブロックします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-581">It is invalid to explicitly transfer execution into a `Finally` block; it is also invalid to transfer execution out of a `Finally` block except through an exception.</span></span>

```antlr
FinallyStatement
    : 'Finally' StatementTerminator
      Block?
    ;
```

#### <a name="catch-blocks"></a><span data-ttu-id="9d4b6-582">catch ブロック</span><span class="sxs-lookup"><span data-stu-id="9d4b6-582">Catch Blocks</span></span>

<span data-ttu-id="9d4b6-583">処理中に例外が発生した場合、`Try`ブロックする場合に、各`Catch`ステートメントが例外を処理するかどうかのテキストの順序で調べられます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-583">If an exception occurs while processing the `Try` block, each `Catch` statement is examined in textual order to determine if it handles the exception.</span></span>

```antlr
CatchStatement
    : 'Catch' ( Identifier ( 'As' NonArrayTypeName )? )?
      ( 'When' BooleanExpression )? StatementTerminator
      Block?
    ;
```

<span data-ttu-id="9d4b6-584">指定された識別子を`Catch`句がスローされた例外を表します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-584">The identifier specified in a `Catch` clause represents the exception that has been thrown.</span></span> <span data-ttu-id="9d4b6-585">識別子が含まれている場合、`As`句では、その識別子は内で宣言すると見なされます、`Catch`ブロックのローカル宣言領域です。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-585">If the identifier contains an `As` clause, then the identifier is considered to be declared within the `Catch` block's local declaration space.</span></span> <span data-ttu-id="9d4b6-586">それ以外の場合、識別子が含まれているブロックで定義されたローカル変数 (静的変数ではなく) 場合があります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-586">Otherwise, the identifier must be a local variable (not a static variable) that was defined in a containing block.</span></span>

<span data-ttu-id="9d4b6-587">A`Catch`識別子のない句をキャッチ オールから派生した例外`System.Exception`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-587">A `Catch` clause with no identifier will catch all exceptions derived from `System.Exception`.</span></span> <span data-ttu-id="9d4b6-588">A`Catch`識別子を持つ句だけと同じ、または識別子の型から派生した型を持つ例外をキャッチします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-588">A `Catch` clause with an identifier will only catch exceptions whose types are the same as or derived from the type of the identifier.</span></span> <span data-ttu-id="9d4b6-589">型でなければなりません`System.Exception`から派生した型または`System.Exception`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-589">The type must be `System.Exception`, or a type derived from `System.Exception`.</span></span> <span data-ttu-id="9d4b6-590">派生した例外がキャッチされたときに`System.Exception`、例外オブジェクトへの参照が関数によって返されるオブジェクトに格納されている`Microsoft.VisualBasic.Information.Err`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-590">When an exception is caught that derives from `System.Exception`, a reference to the exception object is stored in the object returned by the function `Microsoft.VisualBasic.Information.Err`.</span></span>

<span data-ttu-id="9d4b6-591">A`Catch`句、`When`式として評価される場合は、句に例外はキャッチのみ`True`; 式の型はセクションに従ってブール式である必要があります[ブール式](expressions.md#boolean-expressions)します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-591">A `Catch` clause with a `When` clause will only catch exceptions when the expression evaluates to `True`; the type of the expression must be a Boolean expression as per Section [Boolean Expressions](expressions.md#boolean-expressions).</span></span> <span data-ttu-id="9d4b6-592">A`When`句は、例外の種類を確認後にだけ適用し、この例に示すように、次の式が例外を表す識別子を参照してください可能性があります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-592">A `When` clause is only applied after checking the type of the exception, and the expression may refer to the identifier representing the exception, as this example demonstrates:</span></span>

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

<span data-ttu-id="9d4b6-593">この例が出力されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-593">This example prints:</span></span>

```
Third handler
```

<span data-ttu-id="9d4b6-594">場合、`Catch`句は、例外を処理、転送の実行、`Catch`ブロックします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-594">If a `Catch` clause handles the exception, execution transfers to the `Catch` block.</span></span> <span data-ttu-id="9d4b6-595">最後に、`Catch`に続く最初のステートメントの実行の転送をブロックします。、`Try`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-595">At the end of the `Catch` block, execution transfers to the first statement following the `Try` statement.</span></span> <span data-ttu-id="9d4b6-596">`Try`ステートメント スローされた例外を処理しないが、`Catch`ブロックします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-596">The `Try` statement will not handle any exceptions thrown in a `Catch` block.</span></span> <span data-ttu-id="9d4b6-597">ない場合は`Catch`句は、システムによって決まります場所への転送の実行、例外を処理します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-597">If no `Catch` clause handles the exception, execution transfers to a location determined by the system.</span></span>

<span data-ttu-id="9d4b6-598">実行を明示的に転送することはできません、`Catch`ブロックします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-598">It is invalid to explicitly transfer execution into a `Catch` block.</span></span>

<span data-ttu-id="9d4b6-599">句が通常のフィルターでは、例外がスローする前に評価されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-599">The filters in When clauses are normally evaluated prior to the exception being thrown.</span></span> <span data-ttu-id="9d4b6-600">たとえば、次のコードが印刷されます「フィルター、最後に、Catch」。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-600">For instance, the following code will print "Filter, Finally, Catch".</span></span>

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

 <span data-ttu-id="9d4b6-601">ただし、非同期および反復子メソッドは、ブロックの外側のすべてのフィルターの前に実行するのにはその内部で、最後にすべて。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-601">However, Async and Iterator methods cause all finally blocks inside them to be executed prior to any filters outside.</span></span> <span data-ttu-id="9d4b6-602">たとえば、上記のコードがある場合`Async Sub Foo()`出力になりますし、「最後に、フィルター処理、キャッチ」。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-602">For instance, if the above code had `Async Sub Foo()`, then the output would be "Finally, Filter, Catch".</span></span>


#### <a name="throw-statement"></a><span data-ttu-id="9d4b6-603">Throw ステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-603">Throw Statement</span></span>

<span data-ttu-id="9d4b6-604">`Throw`ステートメントから派生した型のインスタンスによって表される例外を発生させる`System.Exception`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-604">The `Throw` statement raises an exception, which is represented by an instance of a type derived from `System.Exception`.</span></span>

```antlr
ThrowStatement
    : 'Throw' Expression? StatementTerminator
    ;
```

<span data-ttu-id="9d4b6-605">式が値として分類されないかから派生した型ではなく`System.Exception`コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-605">If the expression is not classified as a value or is not a type derived from `System.Exception`, then a compile-time error occurs.</span></span> <span data-ttu-id="9d4b6-606">実行時に、null 値に式が評価された場合、`System.NullReferenceException`代わりに例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-606">If the expression evaluates to a null value at run time, then a `System.NullReferenceException` exception is raised instead.</span></span>

<span data-ttu-id="9d4b6-607">A`Throw`ステートメントの catch ブロック内の式を省略できます、`Try`の介在がない限り、ステートメントが最後にブロックします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-607">A `Throw` statement may omit the expression within a catch block of a `Try` statement, as long as there is no intervening finally block.</span></span> <span data-ttu-id="9d4b6-608">その場合は、ステートメントでは、catch ブロック内で現在処理中の例外が再スローします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-608">In that case, the statement rethrows the exception currently being handled within the catch block.</span></span> <span data-ttu-id="9d4b6-609">例:</span><span class="sxs-lookup"><span data-stu-id="9d4b6-609">For example:</span></span>

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


### <a name="unstructured-exception-handling-statements"></a><span data-ttu-id="9d4b6-610">非構造化例外処理ステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-610">Unstructured Exception-Handling Statements</span></span>

<span data-ttu-id="9d4b6-611">非構造化例外処理は、例外が発生したときに分岐するステートメントを示すエラーを処理するメソッドです。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-611">Unstructured exception handling is a method of handling errors by indicating statements to branch to when an exception occurs.</span></span> <span data-ttu-id="9d4b6-612">3 つのステートメントを使用して非構造化例外処理を実装:`Error`ステートメントでは、`On Error`ステートメント、および`Resume`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-612">Unstructured exception handling is implemented using three statements: the `Error` statement, the `On Error` statement, and the `Resume` statement.</span></span>

```antlr
UnstructuredErrorStatement
    : ErrorStatement
    | OnErrorStatement
    | ResumeStatement
    ;
```

<span data-ttu-id="9d4b6-613">例:</span><span class="sxs-lookup"><span data-stu-id="9d4b6-613">For example:</span></span>

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

<span data-ttu-id="9d4b6-614">メソッドは、非構造化例外処理を使用する場合は、すべての例外をキャッチする場合、メソッド全体の 1 つの構造化例外ハンドラーが確立されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-614">When a method uses unstructured exception handling, a single structured exception handler is established for the entire method that catches all exceptions.</span></span> <span data-ttu-id="9d4b6-615">(をコンストラクター内でこのハンドラーは拡張されませんへの呼び出しへの呼び出しの上に注意してください`New`コンストラクターの先頭にします。)。メソッドからの最新の例外ハンドラーの場所と追跡がスローされた、最新の例外。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-615">(Note that in constructors this handler does not extend over the call to the call to `New` at the beginning of the constructor.) The method then keeps track of the most recent exception-handler location and the most recent exception that has been thrown.</span></span> <span data-ttu-id="9d4b6-616">メソッドへのエントリを例外ハンドラーの場所と、例外が両方設定`Nothing`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-616">At entry to the method, the exception-handler location and the exception are both set to `Nothing`.</span></span> <span data-ttu-id="9d4b6-617">例外オブジェクトへの参照が関数によって返されるオブジェクトに格納されている非構造化例外処理を使用するメソッドで例外がスローされると、`Microsoft.VisualBasic.Information.Err`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-617">When an exception is thrown in a method that uses unstructured exception handling, a reference to the exception object is stored in the object returned by the function `Microsoft.VisualBasic.Information.Err`.</span></span>

<span data-ttu-id="9d4b6-618">非構造化エラー処理ステートメントは、反復子または非同期のメソッドでは使用できません。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-618">Unstructured error handling statements are not allowed in iterator or async methods.</span></span>


#### <a name="error-statement"></a><span data-ttu-id="9d4b6-619">Error ステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-619">Error Statement</span></span>

<span data-ttu-id="9d4b6-620">`Error`ステートメントがスローされます、 `System.Exception` Visual Basic 6 の例外数を含む例外。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-620">An `Error` statement throws a `System.Exception` exception containing a Visual Basic 6 exception number.</span></span> <span data-ttu-id="9d4b6-621">値として式を分類する必要があり、その型に暗黙的に変換できる必要があります`Integer`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-621">The expression must be classified as a value and its type must be implicitly convertible to `Integer`.</span></span>

```antlr
ErrorStatement
    : 'Error' Expression StatementTerminator
    ;
```

#### <a name="on-error-statement"></a><span data-ttu-id="9d4b6-622">On Error ステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-622">On Error Statement</span></span>

<span data-ttu-id="9d4b6-623">`On Error`ステートメントは、最新の状態の例外処理を変更します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-623">An `On Error` statement modifies the most recent exception-handling state.</span></span>

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

<span data-ttu-id="9d4b6-624">4 つの方法のいずれかで使用することがあります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-624">It may be used in one of four ways:</span></span>

* <span data-ttu-id="9d4b6-625">`On Error GoTo -1` 最新の例外をリセット`Nothing`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-625">`On Error GoTo -1` resets the most recent exception to `Nothing`.</span></span>

* <span data-ttu-id="9d4b6-626">`On Error GoTo 0` 最新の例外ハンドラーの場所のリセット`Nothing`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-626">`On Error GoTo 0` resets the most recent exception-handler location to `Nothing`.</span></span>

* <span data-ttu-id="9d4b6-627">`On Error GoTo LabelName` 最新の例外ハンドラーの場所としては、ラベルを設定します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-627">`On Error GoTo LabelName` establishes the label as the most recent exception-handler location.</span></span> <span data-ttu-id="9d4b6-628">このステートメントは、ラムダ式、またはクエリ式を含むメソッドでは使用できません。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-628">This statement cannot be used in a method that contains a lambda or query expression.</span></span>

* <span data-ttu-id="9d4b6-629">`On Error Resume Next` 確立、`Resume Next`最新の例外ハンドラーの場所として動作します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-629">`On Error Resume Next` establishes the `Resume Next` behavior as the most recent exception-handler location.</span></span>


#### <a name="resume-statement"></a><span data-ttu-id="9d4b6-630">Resume ステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-630">Resume Statement</span></span>

<span data-ttu-id="9d4b6-631">A`Resume`ステートメントは、最新の例外の原因となったステートメントを実行を戻します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-631">A `Resume` statement returns execution to the statement that caused the most recent exception.</span></span>

```antlr
ResumeStatement
    : 'Resume' ResumeClause? StatementTerminator
    ;

ResumeClause
    : 'Next'
    | LabelName
    ;
```

<span data-ttu-id="9d4b6-632">場合、`Next`修飾子を指定すると、実行を最新の例外の原因となったステートメントの後に実行されたステートメントに戻ります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-632">If the `Next` modifier is specified, execution returns to the statement that would have been executed after the statement that caused the most recent exception.</span></span> <span data-ttu-id="9d4b6-633">ラベル名が指定されている場合、ラベルに実行を返します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-633">If a label name is specified, execution returns to the label.</span></span>

<span data-ttu-id="9d4b6-634">`SyncLock`ステートメントが含まれている暗黙の型の構造化エラー処理ブロック、`Resume`と`Resume Next`で発生する例外の特殊な動作がある`SyncLock`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-634">Because the `SyncLock` statement contains an implicit structured error-handling block, `Resume` and `Resume Next` have special behaviors for exceptions that occur in `SyncLock` statements.</span></span> <span data-ttu-id="9d4b6-635">`Resume` 先頭に、実行を返す、`SyncLock`ステートメントでは、中に`Resume Next`実行を次のステートメントに返す、`SyncLock`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-635">`Resume` returns execution to the beginning of the `SyncLock` statement, while `Resume Next` returns execution to the next statement following the `SyncLock` statement.</span></span> <span data-ttu-id="9d4b6-636">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-636">For example, consider the following code:</span></span>

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

<span data-ttu-id="9d4b6-637">次の結果を出力します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-637">It prints the following result.</span></span>

```
Before exception
Before exception
After SyncLock
```

<span data-ttu-id="9d4b6-638">最初のループ、`SyncLock`ステートメントでは、`Resume`の先頭に、実行を返す、`SyncLock`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-638">The first time through the `SyncLock` statement, `Resume` returns execution to the beginning of the `SyncLock` statement.</span></span> <span data-ttu-id="9d4b6-639">2 回目、`SyncLock`ステートメントでは、`Resume Next`の最後に実行を戻します、`SyncLock`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-639">The second time through the `SyncLock` statement, `Resume Next` returns execution to the end of the `SyncLock` statement.</span></span> <span data-ttu-id="9d4b6-640">`Resume` `Resume Next`内では使用できません、`SyncLock`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-640">`Resume` and `Resume Next` are not allowed within a `SyncLock` statement.</span></span>

<span data-ttu-id="9d4b6-641">どの場合も、ときに、`Resume`ステートメントが実行され、最新の例外に設定されている`Nothing`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-641">In all cases, when a `Resume` statement is executed, the most recent exception is set to `Nothing`.</span></span> <span data-ttu-id="9d4b6-642">場合、`Resume`ステートメントを実行する最新の例外ではないと、ステートメントが発生、 `System.Exception` Visual Basic のエラー番号を含む例外`20`(エラーを発生させずに再開する)。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-642">If a `Resume` statement is executed with no most recent exception, the statement raises a `System.Exception` exception containing the Visual Basic error number `20` (Resume without error).</span></span>


## <a name="branch-statements"></a><span data-ttu-id="9d4b6-643">分岐ステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-643">Branch Statements</span></span>

<span data-ttu-id="9d4b6-644">分岐ステートメントは、メソッドの実行の流れを変更します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-644">Branch statements modify the flow of execution in a method.</span></span> <span data-ttu-id="9d4b6-645">これには 6 つの分岐ステートメントがあります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-645">There are six branch statements:</span></span>

1. <span data-ttu-id="9d4b6-646">A`GoTo`ステートメントにより、メソッドで指定されたラベルに実行します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-646">A `GoTo` statement causes execution to transfer to the specified label in the method.</span></span> <span data-ttu-id="9d4b6-647">許可されていない`GoTo`に、 `Try`、 `Using`、 `SyncLock`、 `With`、`For`または`For Each`ブロックも、そのブロックのローカル変数がラムダ式、または LINQ 式でキャプチャされた場合、ループ ブロックにします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-647">It is not allowed to `GoTo` into a `Try`, `Using`, `SyncLock`, `With`, `For` or `For Each` block, nor into any loop block if a local variable of that block is captured in a lambda or LINQ expression.</span></span>
2. <span data-ttu-id="9d4b6-648">`Exit`ステートメントは、指定した種類のコンテナーのブロックのステートメントの終了後に次のステートメントに実行を転送します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-648">An `Exit` statement transfers execution to the next statement after the end of the immediately containing block statement of the specified kind.</span></span> <span data-ttu-id="9d4b6-649">かどうか、ブロックはメソッドのブロック、制御フローは、セクションで説明したように、メソッドを終了[制御フロー](statements.md#control-flow)します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-649">If the block is the method block, then control flow exits the method as described in Section [Control Flow](statements.md#control-flow).</span></span> <span data-ttu-id="9d4b6-650">場合、`Exit`ステートメントがコンパイル時エラーが発生したステートメントでは、指定されたブロックの種類に含まれていません。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-650">If the `Exit` statement is not contained within the kind of block specified in the statement, a compile-time error occurs.</span></span>
3. <span data-ttu-id="9d4b6-651">A`Continue`ステートメントは、指定した種類のコンテナーのブロック ループ ステートメントの最後に実行を転送します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-651">A `Continue` statement transfers execution to the end of the immediately containing block loop statement of the specified kind.</span></span> <span data-ttu-id="9d4b6-652">場合、`Continue`ステートメントがコンパイル時エラーが発生したステートメントでは、指定されたブロックの種類に含まれていません。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-652">If the `Continue` statement is not contained within the kind of block specified in the statement, a compile-time error occurs.</span></span>
4. <span data-ttu-id="9d4b6-653">A`Stop`ステートメントにより、デバッガーの例外を発生します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-653">A `Stop` statement causes a debugger exception to occur.</span></span>
5. <span data-ttu-id="9d4b6-654">`End`ステートメントは、プログラムを終了します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-654">An `End` statement terminates the program.</span></span> <span data-ttu-id="9d4b6-655">ファイナライザーは、シャット ダウンする前に実行されますが、現在の実行の finally ブロック`Try`ステートメントは実行されません。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-655">Finalizers are run before shutdown, but the finally blocks of any currently executing `Try` statements are not executed.</span></span> <span data-ttu-id="9d4b6-656">このステートメントを使用して、(たとえば、Dll など) 実行可能ファイルではないプログラムのことはできません。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-656">This statement may not be used in programs that are not executable (for example, DLLs).</span></span>
6. <span data-ttu-id="9d4b6-657">A`Return`式ステートメントは、`Exit Sub`または`Exit Function`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-657">A `Return` statement with no expression is equivalent to an `Exit Sub` or `Exit Function` statement.</span></span> <span data-ttu-id="9d4b6-658">A`Return`式を使用したステートメントが関数の場合は、通常のメソッドでのみ使用できますか、非同期メソッドで戻り値の型を持つ関数である`Task(Of T)`一部`T`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-658">A `Return` statement with an expression is only allowed in a regular method that is a function, or in an async method that is a function with return type `Task(Of T)` for some `T`.</span></span> <span data-ttu-id="9d4b6-659">暗黙的に変換される値として式を分類する必要があります、*関数の戻り変数*(通常のメソッド) の場合や、*タスク戻り変数*(非同期メソッド) の場合.</span><span class="sxs-lookup"><span data-stu-id="9d4b6-659">The expression must be classified as a value which is implicitly convertible to the *function return variable* (in the case of regular methods) or to the *task return variable* (in the case of async methods).</span></span> <span data-ttu-id="9d4b6-660">その動作は、その式を評価する戻り値の変数に格納し、暗黙的な実行`Exit Function`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-660">Its behavior is to evaluate its expression, then store it in the return variable, then execute an implicit `Exit Function` statement.</span></span>

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

## <a name="array-handling-statements"></a><span data-ttu-id="9d4b6-661">配列処理ステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-661">Array-Handling Statements</span></span>

<span data-ttu-id="9d4b6-662">2 つのステートメントは配列の操作を簡素化:`ReDim`ステートメントと`Erase`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-662">Two statements simplify working with arrays: `ReDim` statements and `Erase` statements.</span></span>

```antlr
ArrayHandlingStatement
    : RedimStatement
    | EraseStatement
    ;
```

### <a name="redim-statement"></a><span data-ttu-id="9d4b6-663">ReDim ステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-663">ReDim Statement</span></span>

<span data-ttu-id="9d4b6-664">A`ReDim`ステートメントは、新しい配列をインスタンス化します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-664">A `ReDim` statement instantiates new arrays.</span></span>

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

<span data-ttu-id="9d4b6-665">ステートメントの各句を変数またはプロパティ アクセスの型が配列型に分類する必要がありますまたは`Object`配列の境界のリストが続くとします。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-665">Each clause in the statement must be classified as a variable or a property access whose type is an array type or `Object`, and be followed by a list of array bounds.</span></span> <span data-ttu-id="9d4b6-666">境界の数は、変数の型と一致する必要があります。境界内の任意の数が許可されている`Object`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-666">The number of the bounds must be consistent with the type of the variable; any number of bounds is allowed for `Object`.</span></span> <span data-ttu-id="9d4b6-667">実行時に、配列は各式は、指定した境界内で右に左からのインスタンス化し、変数またはプロパティに割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-667">At run time, an array is instantiated for each expression from left to right with the specified bounds and then assigned to the variable or property.</span></span> <span data-ttu-id="9d4b6-668">変数の型が場合`Object`、ディメンションの数が指定すると、ディメンションの数、配列要素の型が`Object`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-668">If the variable type is `Object`, the number of dimensions is the number of dimensions specified, and the array element type is `Object`.</span></span> <span data-ttu-id="9d4b6-669">実行時にディメンションの指定した数値の変数またはプロパティと互換性がない場合は、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-669">If the given number of dimensions is incompatible with the variable or property at run time a compile-time error occurs.</span></span> <span data-ttu-id="9d4b6-670">例:</span><span class="sxs-lookup"><span data-stu-id="9d4b6-670">For example:</span></span>

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

<span data-ttu-id="9d4b6-671">場合、`Preserve`キーワードを指定し、式は、値として分類にもする必要があります、右端のものを除く各ディメンションの新しいサイズは、既存の配列のサイズと同じである必要があります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-671">If the `Preserve` keyword is specified, then the expressions must also be classifiable as a value, and the new size for each dimension except for the rightmost one must be the same as the size of the existing array.</span></span> <span data-ttu-id="9d4b6-672">既存の配列内の値は、新しい配列にコピーされます。 新しい配列が小さい場合は、既存の値は破棄されます。新しい配列が大きい場合は、余分な要素は、配列の要素型の既定値に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-672">The values in the existing array are copied into the new array: if the new array is smaller, the existing values are discarded; if the new array is bigger, the extra elements will be initialized to the default value of the element type of the array.</span></span> <span data-ttu-id="9d4b6-673">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-673">For example, consider the following code:</span></span>

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

<span data-ttu-id="9d4b6-674">次の結果が出力されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-674">It prints the following result:</span></span>

```
3, 0
```

<span data-ttu-id="9d4b6-675">既存の配列の参照では、null 値を実行時に、エラーは表示されません。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-675">If the existing array reference is a null value at run time, no error is given.</span></span> <span data-ttu-id="9d4b6-676">ディメンションのサイズが変更された場合は、最も右にあるディメンション以外、`System.ArrayTypeMismatchException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-676">Other than the rightmost dimension, if the size of a dimension changes, a `System.ArrayTypeMismatchException` will be thrown.</span></span>

<span data-ttu-id="9d4b6-677">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="9d4b6-677">__Note.__</span></span> <span data-ttu-id="9d4b6-678">`Preserve` 予約語ではありません。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-678">`Preserve` is not a reserved word.</span></span>


### <a name="erase-statement"></a><span data-ttu-id="9d4b6-679">Erase ステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-679">Erase Statement</span></span>

<span data-ttu-id="9d4b6-680">`Erase`ステートメントの各配列変数またはステートメントで指定したプロパティ設定`Nothing`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-680">An `Erase` statement sets each of the array variables or properties specified in the statement to `Nothing`.</span></span> <span data-ttu-id="9d4b6-681">ステートメント内の各式は、型が配列型の変数またはプロパティ アクセスとして分類する必要がありますまたは`Object`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-681">Each expression in the statement must be classified as a variable or property access whose type is an array type or `Object`.</span></span> <span data-ttu-id="9d4b6-682">例:</span><span class="sxs-lookup"><span data-stu-id="9d4b6-682">For example:</span></span>

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

## <a name="using-statement"></a><span data-ttu-id="9d4b6-683">Using ステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-683">Using statement</span></span>

<span data-ttu-id="9d4b6-684">型のインスタンスは、コレクションが実行され、インスタンスへのライブ参照が見つからないときに自動的に、ガベージ コレクターによって解放されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-684">Instances of types are automatically released by the garbage collector when a collection is run and no live references to the instance are found.</span></span> <span data-ttu-id="9d4b6-685">型は、特に役に立つと不足しているリソース (データベース接続、ファイル ハンドルなど) で保持している場合が、次のガベージ コレクションで使用できなくなった型の特定のインスタンスをクリーンアップするまで待機することが望ましい場合がありますできません。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-685">If a type holds on a particularly valuable and scarce resource (such as database connections or file handles), it may not be desirable to wait until the next garbage collection to clean up a particular instance of the type that is no longer in use.</span></span> <span data-ttu-id="9d4b6-686">コレクションの前にリソースを解放する最も簡単な方法を提供する型を実装可能性があります、`System.IDisposable`インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-686">To provide a lightweight way of releasing resources before a collection, a type may implement the `System.IDisposable` interface.</span></span> <span data-ttu-id="9d4b6-687">型を公開、`Dispose`メソッドをすぐに、そのために解放するためのリソースの強制的に呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-687">A type that does so exposes a `Dispose` method that can be called to force valuable resources to be released immediately, as such:</span></span>

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

<span data-ttu-id="9d4b6-688">`Using`ステートメントは、リソースの取得、一連のステートメントを実行して、リソースを破棄し、プロセスを自動化します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-688">The `Using` statement automates the process of acquiring a resource, executing a set of statements, and then disposing of the resource.</span></span> <span data-ttu-id="9d4b6-689">ステートメントには、2 つの形式を取りますリソースがローカル変数、ステートメントの一部として宣言されており、通常のローカル変数宣言ステートメントとして扱われますを 1 つは、。他のリソースは、式の結果です。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-689">The statement can take two forms: in one, the resource is a local variable declared as a part of the statement and treated as a regular local variable declaration statement; in the other, the resource is the result of an expression.</span></span>

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

<span data-ttu-id="9d4b6-690">かどうか、リソースがローカル変数宣言ステートメントは、ローカル変数宣言の型に暗黙的に変換できる型でなければなりません`System.IDisposable`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-690">If the resource is a local variable declaration statement then the type of the local variable declaration must be a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="9d4b6-691">宣言されたローカル変数は読み取り専用のスコープに、`Using`ステートメントをブロックし、初期化子を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-691">The declared local variables are read-only, scoped to the `Using` statement block and must include an initializer.</span></span> <span data-ttu-id="9d4b6-692">リソース式の結果であるかどうか、式の値として分類する必要がありますに暗黙的に変換できる型でなければなりません`System.IDisposable`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-692">If the resource is the result of an expression then the expression must be classified as a value and must be of a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="9d4b6-693">式は、ステートメントの先頭に 1 回だけ評価されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-693">The expression is only evaluated once, at the beginning of the statement.</span></span>

<span data-ttu-id="9d4b6-694">`Using`ブロックは暗黙的に含まれる、 `Try` finally ブロック ステートメントでメソッドを呼び出す`IDisposable.Dispose`リソース。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-694">The `Using` block is implicitly contained by a `Try` statement whose finally block calls the method `IDisposable.Dispose` on the resource.</span></span> <span data-ttu-id="9d4b6-695">これにより、例外がスローされた場合でも、リソースが破棄されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-695">This ensures the resource is disposed even when an exception is thrown.</span></span> <span data-ttu-id="9d4b6-696">その結果に分岐する有効ながない、 `Using` 、ブロックの外側からブロックと`Using`の目的でブロックが単一のステートメントとして扱われます`Resume`と`Resume Next`します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-696">As a result, it is invalid to branch into a `Using` block from outside of the block, and a `Using` block is treated as a single statement for the purposes of `Resume` and `Resume Next`.</span></span> <span data-ttu-id="9d4b6-697">リソースの場合`Nothing`、なしの呼び出しを`Dispose`されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-697">If the resource is `Nothing`, then no call to `Dispose` is made.</span></span> <span data-ttu-id="9d4b6-698">したがって、次の例を示します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-698">Thus, the example:</span></span>

```vb
Using f As C = New C()
    ...
End Using
```

<span data-ttu-id="9d4b6-699">上の式は、下の式と同等です。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-699">is equivalent to:</span></span>

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

<span data-ttu-id="9d4b6-700">A`Using`をローカル変数宣言ステートメントを持つステートメントは入れ子になっているのと同じですが、時に複数のリソースを取得する可能性があります`Using`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-700">A `Using` statement that has a local variable declaration statement may acquire multiple resources at a time, which is equivalent to nested `Using` statements.</span></span>  <span data-ttu-id="9d4b6-701">たとえば、`Using`形式のステートメント。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-701">For example, a `Using` statement of the form:</span></span>

```vb
Using r1 As R = New R(), r2 As R = New R()
    r1.F()
    r2.F()
End Using
```

<span data-ttu-id="9d4b6-702">上の式は、下の式と同等です。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-702">is equivalent to:</span></span>

```vb
Using r1 As R = New R()
    Using r2 As R = New R()
        r1.F()
        r2.F()
    End Using
End Using
```


## <a name="await-statement"></a><span data-ttu-id="9d4b6-703">Await ステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-703">Await Statement</span></span>

<span data-ttu-id="9d4b6-704">Await ステートメントが await 演算子の式と同じ構文 (セクション[Await 演算子](expressions.md#await-operator)) も、await 式を許可するメソッドにのみ使用できますが、await 演算子の式の動作と同じです。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-704">An await statement has the same syntax as an await operator expression (Section [Await Operator](expressions.md#await-operator)), is allowed only in methods that also allow await expressions, and has the same behavior as an await operator expression.</span></span>

<span data-ttu-id="9d4b6-705">ただし、値または void として分類される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-705">However, it may be classified as either a value or void.</span></span> <span data-ttu-id="9d4b6-706">Await 演算子の式の評価結果の任意の値は破棄されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-706">Any value resulting from evaluation of the await operator expression is discarded.</span></span>

```antlr
AwaitStatement
    : AwaitOperatorExpression StatementTerminator
    ;
```

## <a name="yield-statement"></a><span data-ttu-id="9d4b6-707">Yield ステートメント</span><span class="sxs-lookup"><span data-stu-id="9d4b6-707">Yield Statement</span></span>

<span data-ttu-id="9d4b6-708">Yield ステートメントは、セクションで説明されている反復子メソッドに関連する[反復子メソッド](statements.md#iterator-methods)します。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-708">Yield statements are related to iterator methods, which are described in Section [Iterator Methods](statements.md#iterator-methods).</span></span>

```antlr
YieldStatement
    : 'Yield' Expression StatementTerminator
    ;
```

<span data-ttu-id="9d4b6-709">`Yield` 予約語は、すぐ外側のメソッドまたはラムダ式が表示される場合、`Iterator`修飾子は、場合に、`Yield`を後に表示`Iterator`修飾子; は予約されている他の場所。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-709">`Yield` is a reserved word if the immediately enclosing method or lambda expression in which it appears has an `Iterator` modifier, and if the `Yield` appears after that `Iterator` modifier; it is unreserved elsewhere.</span></span> <span data-ttu-id="9d4b6-710">ないプリプロセッサ ディレクティブ内に確保できます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-710">It is also unreserved in preprocessor directives.</span></span> <span data-ttu-id="9d4b6-711">Yield ステートメントは予約語であるメソッドまたはラムダ式の本体でのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-711">The yield statement is only allowed in the body of a method or lambda expression where it is a reserved word.</span></span> <span data-ttu-id="9d4b6-712">すぐ外側のメソッドまたはラムダ内で yield ステートメントは内部の発生しません、`Catch`または`Finally`ブロック、または内部の本文を`SyncLock`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-712">Within the immediately enclosing method or lambda, the yield statement may not occur inside the body of a `Catch` or `Finally` block, nor inside the body of a `SyncLock` statement.</span></span>

<span data-ttu-id="9d4b6-713">Yield ステートメントは、1 つの式の値として分類する必要がありますの型がの型に暗黙的に変換できる、*反復子の現在の変数*(セクション[反復子メソッド](statements.md#iterator-methods)) の外側の反復子メソッド。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-713">The yield statement takes a single expression which must be classified as a value and whose type is implicitly convertible to the type of the *iterator current variable* (Section [Iterator Methods](statements.md#iterator-methods)) of its enclosing iterator method.</span></span>

<span data-ttu-id="9d4b6-714">制御フローに達するとでのみ、`Yield`ステートメントと、`MoveNext`反復子オブジェクトでメソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-714">Control flow only ever reaches a `Yield` statement when the `MoveNext` method is invoked on an iterator object.</span></span> <span data-ttu-id="9d4b6-715">(これは、反復子メソッドのインスタンスしかためには、そのステートメントを実行するため、`MoveNext`または`Dispose`; 反復子オブジェクトで呼び出されるメソッドと`Dispose`メソッドでのコードを実行してしか`Finally`ブロック、 `Yield`は許可されていません)。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-715">(This is because an iterator method instance only ever executes its statements due to the `MoveNext` or `Dispose` methods being called on an iterator object; and the `Dispose` method will only ever execute code in `Finally` blocks, where `Yield` is not allowed).</span></span>

<span data-ttu-id="9d4b6-716">ときに、`Yield`ステートメントが実行されると、その式が評価されに格納されている、*反復子の現在の変数*反復子メソッドのインスタンスがその反復子オブジェクトに関連付けられているのです。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-716">When a `Yield` statement is executed, its expression is evaluated and stored in the *iterator current variable* of the iterator method instance associated with that iterator object.</span></span> <span data-ttu-id="9d4b6-717">値`True`の呼び出し元に返される`MoveNext`、し、このインスタンスの制御点。 停止の次の呼び出しまで進める`MoveNext`反復子オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="9d4b6-717">The value `True` is returned to the invoker of `MoveNext`, and the control point of this instance stops advancing until the next invocation of `MoveNext` on the iterator object.</span></span>

