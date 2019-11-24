---
ms.openlocfilehash: e49e116e60e724bcd8f1148c8aad9d11dfc92e74
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306049"
---
# <a name="statements"></a>ステートメント

ステートメントは実行可能コードを表します。

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

__付箋.__ Microsoft Visual Basic Compiler では、キーワードまたは識別子で始まるステートメントのみを使用できます。 したがって、たとえば、呼び出しステートメント "`Call (Console).WriteLine`" は許可されていますが、呼び出しステートメント "`(Console).WriteLine`" は許可されていません。

## <a name="control-flow"></a>制御フロー

*制御フロー*は、ステートメントと式が実行される順序です。 実行順序は、特定のステートメントまたは式によって異なります。

たとえば、加算演算子 (セクション[加算演算子](expressions.md#addition-operator)) を評価する場合は、最初に左オペランドが評価され、次に右オペランド、次に演算子自体が評価されます。 まず最初のサブステートメントを実行し、次にブロックのステートメントを使用して1つずつ続行することによって、ブロックが実行されます (セクション[ブロックとラベル](statements.md#blocks-and-labels))。

この順序で暗黙的に使用されるのは、次に実行される操作である*制御ポイント*の概念です。 メソッドが呼び出されたとき (または "呼び出された")、メソッドの*インスタンス*を作成するとします。 メソッドインスタンスは、メソッドのパラメーターとローカル変数の独自のコピーと、独自の制御点で構成されます。

### <a name="regular-methods"></a>通常のメソッド

通常のメソッドの例を次に示します。

```vb
Function Test() As Integer
    Console.WriteLine("hello")
    Return 1
End Sub

Dim x = Test()    ' invokes the function, prints "hello", assigns 1 to x
```

通常のメソッドが呼び出されると、

1. 最初に、その呼び出しに固有のメソッドのインスタンスが作成されます。 このインスタンスには、メソッドのすべてのパラメーターとローカル変数のコピーが含まれています。
2. その後、すべてのパラメーターが、指定された値とそのすべてのローカル変数の型の既定値に初期化されます。
3. `Function`の場合、暗黙的なローカル変数も、関数の名前が関数の*戻り*値の型であり、その型が関数の戻り値の型であり、初期値がその型の既定値である関数の戻り値の変数と呼ばれます。
4. 次に、メソッドインスタンスの制御ポイントがメソッド本体の最初のステートメントで設定され、メソッドの本体がそこからすぐに実行を開始します (セクション[ブロックとラベル](statements.md#blocks-and-labels))。

制御フローがメソッド本体を正常に終了すると、その end をマークする `End Function` または `End Sub` に到達するか、明示的な `Return` または `Exit` ステートメント制御フローがメソッドインスタンスの呼び出し元に戻ります。 関数の戻り変数がある場合、呼び出しの結果はこの変数の最終的な値になります。

制御フローがハンドルされない例外によってメソッド本体を終了すると、その例外は呼び出し元に反映されます。

制御フローが終了した後は、メソッドインスタンスへのライブ参照がなくなります。 メソッドインスタンスがローカル変数またはパラメーターのコピーへの参照のみを保持している場合は、ガベージコレクションされる可能性があります。

### <a name="iterator-methods"></a>Iterator メソッド

反復子メソッドは、シーケンスを生成する便利な方法として使用されます。これは、`For Each` ステートメントで使用できます。 反復子メソッドは、`Yield` ステートメント (Section [Yield ステートメント](statements.md#yield-statement)) を使用してシーケンスの要素を指定します。 (`Yield` ステートメントのない反復子メソッドは、空のシーケンスを生成します)。 Iterator メソッドの例を次に示します。

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

戻り値の型が `IEnumerator(Of T)`である反復子メソッドが呼び出されると、

1. 最初に、その呼び出しに固有の反復子メソッドのインスタンスが作成されます。 このインスタンスには、メソッドのすべてのパラメーターとローカル変数のコピーが含まれています。
2. その後、すべてのパラメーターが、指定された値とそのすべてのローカル変数の型の既定値に初期化されます。
3. 暗黙的なローカル変数は、*反復子の現在の変数*とも呼ばれ、その型が `T` であり、初期値がその型の既定値であることを示します。
4. メソッドインスタンスの制御点は、メソッド本体の最初のステートメントで設定されます。
5. 次に、このメソッドインスタンスに関連付けられている*反復子オブジェクト*が作成されます。 反復子オブジェクトは、宣言された戻り値の型を実装し、以下に示す動作を持ちます。
6. 制御フローは、呼び出し元で*すぐ*に再開され、呼び出しの結果が反復子オブジェクトになります。 この転送は反復子メソッドのインスタンスを終了せずに実行され、finally ハンドラーは実行されないことに注意してください。 メソッドインスタンスは反復子オブジェクトによって参照されていますが、反復子オブジェクトへのライブ参照が存在する限り、ガベージコレクションされません。

反復子オブジェクトの `Current` プロパティにアクセスすると、呼び出しの*現在の変数*が返されます。

反復子オブジェクトの `MoveNext` メソッドが呼び出されると、呼び出しで新しいメソッドインスタンスは作成されません。 代わりに、既存のメソッドインスタンス (およびそのコントロールポイントとローカル変数およびパラメーター) が使用されます。これは、iterator メソッドが最初に呼び出されたときに作成されたインスタンスです。 制御フローは、そのメソッドインスタンスの制御点で実行を再開し、反復子メソッドの本体を通常どおりに進めます。

反復子オブジェクトの `Dispose` メソッドが呼び出されると、既存のメソッドインスタンスが使用されます。 制御フローは、そのメソッドインスタンスの制御ポイントで再開されますが、`Exit Function` のステートメントが次の操作であるかのように直ちに動作します。

反復子オブジェクトでの `MoveNext` または `Dispose` の呼び出しに関する上記の動作について説明するのは、その反復子オブジェクトに対する以前の `MoveNext` または `Dispose` のすべての呼び出しが既に呼び出し元に返されている場合のみです。 存在しない場合、動作は定義されていません。

制御フローが、終了をマークする `End Function` に到達するまで、または明示的な `Return` または `Exit` ステートメントを使用して反復子メソッド本体を終了する場合、反復子メソッドのインスタンスを再開するために、反復子オブジェクトに対する `MoveNext` または `Dispose` 関数の呼び出しのコンテキストで実行する必要があります そのインスタンスの制御点は `End Function` ステートメントのままになり、制御フローが呼び出し元で再開されます。また、`MoveNext` の呼び出しによって再開された場合は、`False` の値が呼び出し元に返されます。

制御フローがハンドルされない例外によって反復子メソッド本体を終了すると、例外が呼び出し元に反映されます。これは、`MoveNext` または `Dispose`の呼び出しのいずれかになります。

反復子関数のその他の考えられる戻り値の型と同様に、

* 一部の `T`に対して戻り値の型が `IEnumerable(Of T)` である反復子メソッドが呼び出されると、メソッド内のすべてのパラメーターの反復子メソッドの呼び出しに固有のインスタンスが最初に作成されます。このメソッドは、指定された値で初期化されます。 呼び出しの結果は、戻り値の型を実装するオブジェクトです。 このオブジェクトの `GetEnumerator` メソッドを呼び出すと、メソッド内のすべてのパラメーターとローカル変数の `GetEnumerator` の呼び出しに固有のインスタンスが作成されます。 このメソッドは、既に保存されている値に対してパラメーターを初期化し、上記の反復子メソッドの場合と同様に処理を続行します。
* 戻り値の型が非ジェネリックインターフェイス `IEnumerator`である反復子メソッドが呼び出されると、動作は `IEnumerator(Of Object)`の場合とまったく同じになります。
* 戻り値の型が非ジェネリックインターフェイス `IEnumerable`である反復子メソッドが呼び出されると、動作は `IEnumerable(Of Object)`の場合とまったく同じになります。

### <a name="async-methods"></a>非同期メソッド

非同期メソッドは、アプリケーションの UI をブロックするなど、長時間実行される作業を行うための便利な方法です。 *非同期は非同期*であるため、非同期メソッドの呼び出し元はすぐに実行を再開することを意味しますが、非同期メソッドのインスタンスの最終的な完了は、後で後で行われる可能性があります。 慣例により、非同期メソッドには "Async" というサフィックスが付いています。

```vb
Async Function TestAsync() As Task(Of String)
    Console.WriteLine("hello")
    Await Task.Delay(100)
    Return "world"
End Function

Dim t = TestAsync()         ' prints "hello"
Console.WriteLine(Await t)  ' prints "world"
```

__付箋.__ 非同期メソッドは、バックグラウンドスレッドでは実行され*ません*。 代わりに、メソッドが `Await` 演算子によって自身を中断し、何らかのイベントに応答して再開されるようにスケジュールすることができます。

非同期メソッドが呼び出されたとき

1. 最初に、その呼び出しに固有の非同期メソッドのインスタンスが作成されます。 このインスタンスには、メソッドのすべてのパラメーターとローカル変数のコピーが含まれています。
2. その後、すべてのパラメーターが、指定された値とそのすべてのローカル変数の型の既定値に初期化されます。
3. 一部の `T`に対して戻り値の型 `Task(Of T)` を持つ非同期メソッドの場合、暗黙的なローカル変数も*タスクの戻り変数*と呼ばれます。この変数の型は `T` であり、初期値は `T`の既定値です。
4. 非同期メソッドが、戻り値の型が `Task` または `T`の `Task(Of T)` の `Function` である場合は、現在の呼び出しに関連付けられた、その型のオブジェクトが暗黙的に作成されます。 これは、 *async オブジェクト*と呼ばれ、非同期メソッドのインスタンスを実行することによって実行される将来の作業を表します。 この非同期メソッドインスタンスの呼び出し元でコントロールが再開されると、呼び出し元はこの非同期オブジェクトを呼び出しの結果として受け取ります。
5. 次に、非同期のメソッド本体の最初のステートメントでインスタンスの制御ポイントを設定し、そこからメソッド本体の実行を直ちに開始します (セクション[ブロックとラベル](statements.md#blocks-and-labels))。

__再開デリゲートと現在の呼び出し元__

「 [Await 演算子](expressions.md#await-operator)」で詳しく説明したように、`Await` 式の実行では、メソッドインスタンスの制御ポイントを中断して他の場所に移動することができます。 制御フローは、*再開デリゲート*の呼び出しによって、後で同じインスタンスの制御ポイントで再開できます。 この中断は、非同期メソッドを終了せずに実行され、finally ハンドラーは実行されないことに注意してください。 メソッドインスタンスは、再開デリゲートと `Task` または `Task(Of T)` 結果 (存在する場合) の両方によって参照されていますが、デリゲートまたは結果へのライブ参照が存在する限り、ガベージコレクションされません。

ステートメント `Dim x = Await WorkAsync()`、次の構文の短縮形として考えてみてください。

```vb
Dim temp = WorkAsync().GetAwaiter()
If Not temp.IsCompleted Then
       temp.OnCompleted(resumptionDelegate)
       Return [task]
       CONT:   ' invocation of 'resumptionDelegate' will resume here
End If
Dim x = temp.GetResult()
```

次の例では、メソッドインスタンスの*現在の呼び出し*元が、元の呼び出し元、または再開デリゲートの呼び出し元として定義されています。

制御フローが非同期メソッド本体を終了すると、その end をマークする `End Sub` または `End Function` に到達するか、明示的な `Return` または `Exit` ステートメント、またはハンドルされない例外を使用して、インスタンスの制御ポイントがメソッドの最後に設定されます。 その後の動作は、非同期メソッドの戻り値の型によって異なります。

* 戻り値の型が `Task``Async Function` の場合は、次のようになります。
  1. 制御フローがハンドルされない例外によって終了した場合、非同期オブジェクトの状態が `TaskStatus.Faulted` に設定され、その `Exception.InnerException` プロパティが例外に設定されます (ただし、`OperationCanceledException` などの特定の実装定義の例外は `TaskStatus.Canceled`に変更されます)。 現在の呼び出し元で制御フローが再開されます。
  2. それ以外の場合は、非同期オブジェクトの状態が `TaskStatus.Completed`に設定されます。 現在の呼び出し元で制御フローが再開されます。

     (__注:__ タスクの中心となるのは、タスクが完了すると、タスクが完了すると、そのタスクを待機していたメソッドが、現在、再開デリゲートを実行している (つまり、ブロック解除される) ことです。

* 一部の `T`の戻り値の型が `Task(Of T)` の `Async Function` の場合: 動作は上記と同じですが、例外が発生しない場合でも、async オブジェクトの `Result` プロパティはタスクの戻り変数の最終的な値に設定される点が異なります。

* `Async Sub`の場合:
  1. 制御フローがハンドルされない例外によって終了した場合、その例外は実装固有の何らかの方法で環境に反映されます。 現在の呼び出し元で制御フローが再開されます。
  2. それ以外の場合、制御フローは、現在の呼び出し元で単に再開されます。

#### <a name="async-sub"></a>Async Sub

`Async Sub`には、Microsoft 固有の動作がいくつかあります。

呼び出しの開始時に `SynchronizationContext.Current` が `Nothing` 場合、非同期サブからの未処理の例外は Threadpool にポストされます。

呼び出しの開始時に `SynchronizationContext.Current` が `Nothing` されていない場合は、メソッドの開始前にそのコンテキストで `OperationStarted()` が呼び出され、終了後に `OperationCompleted()` ます。 また、未処理の例外は、同期コンテキストで再スローされるように投稿されます。

これは、ui アプリケーションで、ui スレッドで呼び出される `Async Sub` の場合、処理に失敗した例外は UI スレッドに reposted されることを意味します。

#### <a name="mutable-structures-in-async-and-iterator-methods"></a>Async および iterator メソッドの変更可能な構造体

通常、変更可能な構造体は不適切な方法と見なされ、async メソッドまたは iterator メソッドではサポートされません。 特に、構造体での async メソッドまたは iterator メソッドの呼び出しは、呼び出しの時点でコピーされるその構造の*コピー*を暗黙的に操作します。 このため、次のようになります。

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

### <a name="blocks-and-labels"></a>ブロックとラベル

実行可能なステートメントのグループは、ステートメントブロックと呼ばれます。 ステートメントブロックの実行は、ブロック内の最初のステートメントで開始されます。 ステートメントが実行されると、ステートメントが別の場所で実行を転送する場合や、例外が発生した場合を除き、次のステートメントが構文の順序で実行されます。

ステートメントブロック内では、論理行に対するステートメントの分割は、ラベル宣言ステートメントを除き、重要ではありません。 ラベルは、ステートメントブロック内の特定の位置を識別する識別子であり、`GoTo`などの分岐ステートメントの対象として使用できます。

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


ラベル宣言ステートメントは論理行の先頭に記述する必要があり、ラベルは識別子または整数リテラルのいずれかになります。 ラベル宣言ステートメントと呼び出しステートメントはどちらも1つの識別子で構成できるため、ローカル行の先頭にある単一の識別子は常にラベル宣言ステートメントと見なされます。 同じ論理行でステートメントが実行されていない場合でも、ラベル宣言ステートメントの後には常にコロンが必要です。

ラベルには独自の宣言領域があり、他の識別子に干渉することはありません。 次の例は有効で、name 変数をパラメーターとしても、ラベルとしても使用してい `x`。

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

ラベルのスコープは、それを含むメソッドの本体です。

読みやすくするために、複数のサブステートメントを含むステートメントの作成は、この仕様では1つの実稼働として扱われますが、それぞれがラベル付きの行でサブステートメントを単独で使用する場合もあります。


### <a name="local-variables-and-parameters"></a>ローカル変数とパラメーター

前のセクションでは、メソッドのインスタンスを作成する方法とタイミング、およびメソッドのローカル変数とパラメーターのコピーについて詳しく説明しました。 さらに、ループの本体が入力されるたびに、「 [Loop ステートメント](statements.md#loop-statements)」で説明されているように、ループ内で宣言された各ローカル変数の新しいコピーが作成されます。メソッドインスタンスには、前のコピーではなくローカル変数のコピーが含まれるようになりました。

すべてのローカル変数は、その型の既定値に初期化されます。 ローカル変数とパラメーターは、常にパブリックにアクセスできます。 次の例に示すように、宣言の前にあるテキスト位置でローカル変数を参照すると、エラーになります。

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

上記の `F` メソッドでは、`i` への最初の代入は、外側のスコープで宣言されたフィールドを参照しません。 代わりに、ローカル変数を参照しています。変数の宣言の前にあるため、エラーになります。 `G` メソッドでは、後続の変数宣言は、同じローカル変数宣言内の前の変数宣言で宣言されたローカル変数を参照します。

メソッド内の各ブロックは、ローカル変数の宣言空間を作成します。 名前は、メソッド本体のローカル変数宣言とメソッドのパラメーターリストを通じて、この宣言領域に導入されます。これにより、最も外側のブロックの宣言領域に名前が導入されます。 ブロックでは、入れ子によって名前のシャドウを許可しません。名前がブロック内で宣言されている場合、その名前は入れ子になったブロックで再宣言されない可能性があります。

したがって、次の例では、名前 `i` が外側のブロックで宣言されており、内部ブロックで再宣言できないため、`F` および `G` メソッドがエラーになります。 ただし、`H` メソッドと `I` メソッドは、2つの `i`が入れ子になっていない個別のブロックで宣言されているため有効です。

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

メソッドが関数の場合、関数の戻り値を表すメソッドと同じ名前のメソッド本体の宣言空間で、特別なローカル変数が暗黙的に宣言されます。 ローカル変数は、式で使用されるときに特別な名前解決セマンティクスを持ちます。 呼び出し式など、メソッドグループとして分類された式が必要なコンテキストでローカル変数が使用されている場合、その名前はローカル変数ではなく関数に解決されます。 例 :

```vb
Function F(i As Integer) As Integer
    If i = 0 Then
        F = 1        ' Sets the return value.
    Else
        F = F(i - 1) ' Recursive call.
    End If
End Function
```

かっこを使用すると、あいまいな状況 (`F(1)`など) が発生する可能性があります。 `F` は、戻り値の型が1次元配列である関数です。あいまいな状況では、名前がローカル変数ではなく関数に解決されます。 例 :

```vb
Function F(i As Integer) As Integer()
    If i = 0 Then
        F = new Integer(2) { 1, 2, 3 }
    Else
        F = F(i - 1) ' Recursive call, not an index.
    End If
End Function
```

制御フローがメソッド本体から出たときに、ローカル変数の値が呼び出し式に渡されます。 メソッドがサブルーチンの場合、そのような暗黙的なローカル変数は存在せず、制御は単に呼び出し式に戻ります。

## <a name="local-declaration-statements"></a>ローカル宣言ステートメント

ローカル宣言ステートメントは、新しいローカル変数、ローカル定数、または静的変数を宣言します。 *ローカル変数*と*ローカル定数*は、メソッドをスコープとするインスタンス変数および定数に相当し、同じ方法で宣言されます。 *静的変数*は `Shared` 変数に似ており、`Static` 修飾子を使用して宣言されます。

```antlr
LocalDeclarationStatement
    : LocalModifier VariableDeclarators StatementTerminator
    ;

LocalModifier
    : 'Static' | 'Dim' | 'Const'
    ;
```

静的変数は、メソッドの呼び出し間で値を保持するローカル変数です。 非共有メソッド内で宣言された静的変数は、インスタンスごとにあります。メソッドを含む型の各インスタンスには、静的変数の独自のコピーがあります。 `Shared` メソッド内で宣言された静的変数は、型ごとになります。すべてのインスタンスに対して静的変数のコピーは1つだけです。 ローカル変数は、メソッドへの各エントリの型の既定値に初期化されますが、静的変数は、型または型のインスタンスが初期化されるときに、その型の既定値にのみ初期化されます。 静的変数は、構造体またはジェネリックメソッドで宣言することはできません。

ローカル変数、ローカル定数、および静的変数は、常にパブリックアクセシビリティを持ち、アクセシビリティ修飾子を指定することはできません。 ローカル宣言ステートメントに型が指定されていない場合は、次の手順によってローカル宣言の型が決まります。

1. 宣言に型文字が含まれている場合、型文字の型はローカル宣言の型になります。

2. ローカル宣言がローカル定数である場合、またはローカル宣言が初期化子を持つローカル変数であり、ローカル変数型の推論が使用されている場合、ローカル宣言の型は初期化子の型から推論されます。 初期化子がローカル宣言を参照している場合、コンパイル時エラーが発生します。 (ローカル定数は初期化子を持つ必要があります)。

3. 厳密なセマンティクスが使用されていない場合、ローカル宣言ステートメントの型は暗黙的に `Object`ます。

4. それ以外の場合は、コンパイル時のエラーが発生します。

配列サイズまたは配列型修飾子を持つローカル宣言ステートメントに型が指定されていない場合、ローカル宣言の型は指定された順位の配列になり、前の手順を使用して配列の要素の型が決定されます。 ローカル変数型の推論を使用する場合、初期化子の型は、ローカル宣言ステートメントと同じ配列図形 (つまり配列型修飾子) を持つ配列型である必要があります。 推定される要素型は配列型である可能性があることに注意してください。 例 :

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

Null 許容型修飾子を持つローカル宣言ステートメントに型が指定されていない場合、ローカル宣言の型は、推論された型の null 許容バージョンか、既に null 許容値型である場合は推論された型自体になります。  推論された型が、null 値を許容できる値の型ではない場合、コンパイル時エラーが発生します。 Null 許容型修飾子と配列サイズまたは配列型修飾子の両方が型のないローカル宣言ステートメントに配置されている場合、null 許容型修飾子は配列の要素型に適用されると見なされ、前の手順を使用して要素が決定されます。nt 型。

ローカル宣言ステートメントの変数初期化子は、宣言のテキストの場所に配置される代入ステートメントと同じです。 したがって、実行がローカル宣言ステートメントで分岐した場合、変数初期化子は実行されません。 ローカル宣言ステートメントが複数回実行された場合、変数初期化子は同じ回数だけ実行されます。 静的変数は初期化子を最初に実行するだけです。 静的変数の初期化中に例外が発生した場合、静的変数は静的変数の型の既定値を使用して初期化されたと見なされます。

次の例は、初期化子の使用方法を示しています。

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

このプログラムの出力:

```console
Static variable x = 5
Static variable x = 6
Static variable x = 7
Local variable y = 8
Local variable y = 8
Local variable y = 8
```

静的ローカルの初期化子はスレッドセーフであり、初期化中に例外に対して保護されます。 静的ローカル初期化子の実行中に例外が発生した場合、静的ローカルは既定値を持ち、初期化されません。 静的ローカル初期化子

```vb
Module Test
    Sub F()
        Static x As Integer = 5
    End Sub
End Module
```

上記の式は、次の式と同じです。

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

ローカル変数、ローカル定数、および静的変数は、宣言されているステートメントブロックにスコープが設定されます。 静的変数は、メソッド全体で1回しか使用できないという意味で特別です。 たとえば、異なるブロックにある場合でも、同じ名前を持つ2つの静的変数宣言を指定することは無効です。


### <a name="implicit-local-declarations"></a>暗黙的なローカル宣言

ローカル宣言ステートメントに加えて、ローカル変数は、使用して暗黙的に宣言することもできます。 別の名前に解決されない名前を使用する単純な名前式は、その名前でローカル変数を宣言します。 例 :

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

暗黙的なローカル宣言は、変数として分類された式を受け入れることができる式のコンテキストでのみ発生します。 この規則の例外として、関数呼び出し式、インデックス式、またはメンバーアクセス式のターゲットである場合、ローカル変数を暗黙的に宣言することはできません。

暗黙的なローカルは、それを含んでいるメソッドの先頭で宣言されているかのように扱われます。 したがって、ラムダ式の内部で宣言されていても、常にメソッドの本体全体にスコープが設定されます。 コード例を次に示します。

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

は `10`値を出力します。 変数名に型文字がアタッチされていない場合、暗黙的なローカル変数は `Object` として型指定されます。それ以外の場合は、変数の型が型文字の型になります。 ローカル変数の型の推論は、暗黙的なローカルには使用されません。

明示的なローカル宣言がコンパイル環境または `Option Explicit`によって指定されている場合は、すべてのローカル変数を明示的に宣言する必要があり、暗黙的な変数宣言は許可されません。

## <a name="with-statement"></a>With ステートメント

`With` ステートメントでは、式を複数回指定することなく、式のメンバーへの複数の参照を使用できます。

```antlr
WithStatement
    : 'With' Expression StatementTerminator
      Block?
      'End' 'With' StatementTerminator
    ;
```

式は、値として分類される必要があり、ブロックに入ったときに1回だけ評価されます。 `With` ステートメントブロック内では、ピリオドまたは感嘆符で始まるメンバーアクセス式またはディクショナリアクセス式は、`With` 式の前にあるかのように評価されます。 例 :

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

ブロックの外部から `With` ステートメントブロックに分岐することはできません。


## <a name="synclock-statement"></a>SyncLock ステートメント

`SyncLock` ステートメントを使用すると、ステートメントを式で同期させることができます。これにより、複数の実行スレッドが同時に同じステートメントを実行することがなくなります。

```antlr
SyncLockStatement
    : 'SyncLock' Expression StatementTerminator
      Block?
      'End' 'SyncLock' StatementTerminator
    ;
```

式は、値として分類される必要があり、ブロックに入ると、1回だけ評価されます。 `SyncLock` ブロックを入力すると、指定された式に対して `Shared` メソッド `System.Threading.Monitor.Enter` が呼び出されます。この式は、実行のスレッドが、式によって返されたオブジェクトの排他ロックを解除するまでブロックします。 `SyncLock` ステートメント内の式の型は、参照型である必要があります。 例 :

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

上記の例では `Test` クラスの特定のインスタンスでを同期して、特定のインスタンスに対して一度にカウント変数に対して実行するスレッドが1つでも追加または削除できないようにしています。

`SyncLock` ブロックは、`Finally` ブロックが式で `System.Threading.Monitor.Exit` `Shared` メソッドを呼び出す `Try` ステートメントによって暗黙的に含まれます。 これにより、例外がスローされた場合でも、ロックが解放されます。 その結果、ブロックの外部から `SyncLock` ブロックに分岐することは無効になり、`SyncLock` ブロックは `Resume` と `Resume Next`のために1つのステートメントとして扱われます。 上の例は、次のコードに相当します。

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


## <a name="event-statements"></a>イベントステートメント

`RaiseEvent`、`AddHandler`、および `RemoveHandler` ステートメントは、イベントを発生させ、イベントを動的に処理します。

```antlr
EventStatement
    : RaiseEventStatement
    | AddHandlerStatement
    | RemoveHandlerStatement
    ;
```

### <a name="raiseevent-statement"></a>RaiseEvent ステートメント

`RaiseEvent` ステートメントは、特定のイベントが発生したことをイベントハンドラーに通知します。

```antlr
RaiseEventStatement
    : 'RaiseEvent' IdentifierOrKeyword
      ( OpenParenthesis ArgumentList? CloseParenthesis )? StatementTerminator
    ;
```

`RaiseEvent` ステートメントの単純な名前式は、`Me`でのメンバー参照として解釈されます。 したがって、`RaiseEvent x` は `RaiseEvent Me.x`されているかのように解釈されます。 式の結果は、クラス自体で定義されているイベントのイベントアクセスとして分類される必要があります。基本データ型で定義されたイベントは、`RaiseEvent` ステートメントでは使用できません。

`RaiseEvent` ステートメントは、指定されたパラメーター (存在する場合) を使用して、イベントのデリゲートの `Invoke` メソッドの呼び出しとして処理されます。 デリゲートの値が `Nothing`場合、例外はスローされません。 引数がない場合は、かっこを省略できます。 例 :

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

上記の `Raiser` クラスは、次の場合と同じです。

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


### <a name="addhandler-and-removehandler-statements"></a>AddHandler および RemoveHandler ステートメント

ほとんどのイベントハンドラーは `WithEvents` 変数によって自動的にフックされますが、実行時にイベントハンドラーを動的に追加したり削除したりすることが必要になる場合があります。 これは、`AddHandler` ステートメントと `RemoveHandler` ステートメントによって行われます。

```antlr
AddHandlerStatement
    : 'AddHandler' Expression Comma Expression StatementTerminator
    ;

RemoveHandlerStatement
    : 'RemoveHandler' Expression Comma Expression StatementTerminator
    ;
```

各ステートメントは2つの引数を取ります。1つ目の引数は、イベントアクセスとして分類される式である必要があり、2番目の引数は値として分類される式である必要があります。 2番目の引数の型は、イベントアクセスに関連付けられているデリゲート型である必要があります。 例 :

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

イベントが発生した場合 `E,` ステートメントは、インスタンスの関連する `add_E` または `remove_E` メソッドを呼び出して、デリゲートをイベントのハンドラーとして追加または削除します。 したがって、上記のコードは次と同じです。

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


## <a name="assignment-statements"></a>代入ステートメント

代入ステートメントによって、式の値が変数に代入されます。 割り当てにはいくつかの種類があります。

```antlr
AssignmentStatement
    : RegularAssignmentStatement
    | CompoundAssignmentStatement
    | MidAssignmentStatement
    ;
```

### <a name="regular-assignment-statements"></a>通常の代入ステートメント

単純な代入ステートメントでは、式の結果が変数に格納されます。

```antlr
RegularAssignmentStatement
    : Expression Equals Expression StatementTerminator
    ;
```

代入演算子の左辺の式は、変数またはプロパティアクセスとして分類する必要があります。また、代入演算子の右辺の式は、値として分類する必要があります。 式の型は、変数またはプロパティアクセスの型に暗黙的に変換できなければなりません。

に割り当てられている変数が参照型の配列要素である場合は、式が配列要素型と互換性があることを確認するためにランタイムチェックが実行されます。 次の例では、最後の割り当てによって `System.ArrayTypeMismatchException` がスローされます。これは、`ArrayList` のインスタンスを `String` 配列の要素に格納できないためです。

```vb
Dim sa(10) As String
Dim oa As Object() = sa
oa(0) = Nothing         ' This is allowed.
oa(1) = "Hello"         ' This is allowed.
oa(2) = New ArrayList() ' System.ArrayTypeMismatchException is thrown.
```

代入演算子の左辺の式が変数として分類されている場合、代入ステートメントは変数に値を格納します。 式がプロパティアクセスとして分類されている場合は、代入ステートメントによって、プロパティのアクセスが、value パラメーターの値に置き換えられたプロパティの `Set` アクセサーの呼び出しに変換されます。 例 :

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

変数またはプロパティアクセスの対象が値型として型指定されていても、変数として分類されていない場合は、コンパイル時エラーが発生します。 例 :

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

割り当てのセマンティクスは、割り当てられている変数またはプロパティの型によって異なります。 割り当てられている変数が値型である場合、代入によって式の値が変数にコピーされます。 割り当てられている変数が参照型の場合、代入は、値自体ではなく参照を変数にコピーします。 変数の型が `Object`場合、代入セマンティクスは、値の型が実行時に値型か参照型かによって決まります。


__付箋.__ `Integer` や `Date`などの組み込み型では、型が不変であるため、参照と値の代入のセマンティクスは同じです。 その結果、言語は、最適化として、ボックス化された組み込み型に対する参照割り当てを自由に使用できるようになります。 値の観点から見ると、結果は同じになります。

Equals 文字 (`=`) は代入と等価性の両方で使用されるため、単純な代入と、`x = y.ToString()`などの状況での呼び出しステートメントの間にあいまいさがあります。 このような場合、代入ステートメントは等値演算子よりも優先されます。 これは、例の式が `(x = y).ToString()`ではなく `x = (y.ToString())` として解釈されることを意味します。


### <a name="compound-assignment-statements"></a>複合代入ステートメント

*複合代入ステートメント*では、`V op= E` の形式を取ります (`op` は有効な二項演算子です)。

```antlr
CompoundAssignmentStatement
    : Expression CompoundBinaryOperator LineTerminator? Expression StatementTerminator
    ;

CompoundBinaryOperator
    : '^' '=' | '*' '=' | '/' '=' | '\\' '=' | '+' '=' | '-' '='
    | '&' '=' | '<' '<' '=' | '>' '>' '='
    ;
```

代入演算子の左辺の式は、変数またはプロパティアクセスとして分類する必要があります。また、代入演算子の右辺の式は、値として分類する必要があります。 複合代入ステートメントは、複合代入演算子の左辺の変数が1回だけ評価されるという点で、ステートメント `V = V op E` と同じです。 次の例は、この違いを示しています。

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

式 `a(GetIndex())` は単純な割り当てに対して2回評価されますが、複合代入では1回だけ評価されるので、コードは次のように出力されます。

```console
Simple assignment
Getting index
Getting index
Compound assignment
Getting index
```


### <a name="mid-assignment-statement"></a>Mid 代入ステートメント

`Mid` の代入ステートメントは、文字列を別の文字列に代入します。 代入の左側には、関数 `Microsoft.VisualBasic.Strings.Mid`の呼び出しと同じ構文があります。

```antlr
MidAssignmentStatement
    : 'Mid' '$'? OpenParenthesis Expression Comma Expression
      ( Comma Expression )? CloseParenthesis Equals Expression StatementTerminator
    ;
```

最初の引数は代入の対象であり、型が `String`との間で暗黙的に変換可能である変数またはプロパティアクセスとして分類される必要があります。 2番目のパラメーターは、対象の文字列内で代入を開始する位置に対応する1から始まる開始位置であり、`Integer`に暗黙的に変換できる型の値として分類する必要があります。 省略可能な3番目のパラメーターは、対象の文字列に代入する右側の値からの文字数であり、`Integer`に暗黙的に変換できる型を持つ値として分類する必要があります。 右側はソース文字列であり、`String`に暗黙的に変換できる型を持つ値として分類する必要があります。 右側は、指定されている場合は長さのパラメーターに切り捨てられ、左側の文字列の文字は開始位置から置き換えられます。 右側の文字列に3番目のパラメーターよりも多くの文字が含まれている場合は、右側の文字列の文字だけがコピーされます。

次の例では `ab123fg`が表示されます。

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

__付箋.__ `Mid` は予約語ではありません。


## <a name="invocation-statements"></a>呼び出しステートメント

呼び出しステートメントは、省略可能なキーワード `Call`の前にあるメソッドを呼び出します。 呼び出しステートメントは関数呼び出し式と同じ方法で処理されますが、次に示すいくつかの違いがあります。 呼び出し式は、値または void として分類される必要があります。 呼び出し式の評価の結果として得られる値はすべて破棄されます。

`Call` キーワードを省略した場合、呼び出し式は、識別子またはキーワード、または `With` ブロック内の `.` で始まる必要があります。 したがって、たとえば、"`Call 1.ToString()`" は有効なステートメントですが、"`1.ToString()`" は有効ではありません。 (式のコンテキストでは、呼び出し式も識別子で始まらない必要があることに注意してください。 たとえば、"`Dim x = 1.ToString()`" は有効なステートメントです)。

呼び出しステートメントと呼び出し式には別の違いがあります。呼び出しステートメントに引数リストが含まれている場合、これは常に呼び出しの引数リストとして取得されます。 次の例は、違いを示しています。

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

## <a name="conditional-statements"></a>条件付きステートメント

条件付きステートメントを使用すると、実行時に評価された式に基づいてステートメントを条件付きで実行できます。

```antlr
ConditionalStatement
    : IfStatement
    | SelectStatement
    ;
```

### <a name="ifthenelse-statements"></a>もし。。。そうしたら。。。Else ステートメント

`If...Then...Else` ステートメントは、基本的な条件付きステートメントです。

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

`If...Then...Else` ステートメント内の各式は、項[ブール式](expressions.md#boolean-expressions)に従ってブール式である必要があります。 (注: この場合、式はブール型である必要はありません)。 `If` ステートメント内の式が true の場合、`If` ブロックで囲まれたステートメントが実行されます。 式が false の場合、各 `ElseIf` 式が評価されます。 いずれかの `ElseIf` 式が true と評価されると、対応するブロックが実行されます。 式が true に評価されず、`Else` ブロックがある場合は、`Else` ブロックが実行されます。 ブロックの実行が完了すると、実行は `If...Then...Else` ステートメントの末尾に渡されます。

`If` ステートメントの行バージョンには、`If` 式が `True` 場合に実行されるステートメントのセットが1つあります。また、式が `False`場合は、省略可能なステートメントのセットが実行されます。 例 :

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

If ステートメントの行バージョンは、":" よりも密にバインドされており、その `Else` は、構文で許可されている、前に説明した前の `If` にバインドされます。 たとえば、次の2つのバージョンは同等です。

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

ラベル宣言ステートメント以外のすべてのステートメントは、ブロックステートメントを含む行 `If` ステートメントの内部で許可されます。 ただし、複数行のラムダ式の内部では、StatementTerminators として LineTerminators を使用することはできません。 例 :

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

### <a name="select-case-statements"></a>Select Case ステートメント

`Select Case` ステートメントは、式の値に基づいてステートメントを実行します。

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

式は、値として分類される必要があります。 `Select Case` ステートメントが実行されると、`Select` 式が最初に評価され、`Case` ステートメントがテキスト宣言の順序で評価されます。 `True` に評価される最初の `Case` ステートメントには、ブロックが実行されます。 `Case` ステートメントが `True` に評価されず、`Case Else` ステートメントがある場合は、そのブロックが実行されます。 ブロックの実行が完了すると、`Select` ステートメントの最後に実行が渡されます。

`Case` ブロックの実行は、次の switch セクションに "フォールスルー" することは許可されていません。 これにより、`Case` の終了ステートメントが誤って省略された場合に、他の言語で発生する一般的なバグクラスを回避できます。 この動作を次の例に示します。

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

コードは次のように出力されます。

```console
x = 10
```

`Case 10` と `Case 20 - 10` は同じ値に対して選択されますが、`Case 10` が実行されます。これは、`Case 20 - 10` の前にあるためです。 次の `Case` に到達すると、`Select` ステートメントの後で実行が続行されます。

`Case` 句には、2つの形式を使用できます。 1つの形式は、省略可能な `Is` キーワード、比較演算子、および式です。 式は `Select` 式の型に変換されます。式が `Select` 式の型に暗黙的に変換できない場合、コンパイル時エラーが発生します。 `Select` 式が*e*の場合、比較演算子は*Op*、`Case` 式は*E1*、case は*e Op E1*として評価されます。 演算子は、2つの式の型に対して有効である必要があります。それ以外の場合は、コンパイル時エラーが発生します。

もう1つの形式は、必要に応じて、キーワード `To` と2番目の式の後に続く式です。 両方の式は、`Select` 式の型に変換されます。いずれかの式が `Select` 式の型に暗黙的に変換できない場合、コンパイル時エラーが発生します。 `Select` 式が `E`の場合、最初の `Case` 式が `E1`、2番目の `Case` 式が `E2`の場合は、`Case` (`E = E1` が指定されていない場合) または `E2` として評価されます。`(E >= E1) And (E <= E2)` 演算子は、2つの式の型に対して有効である必要があります。それ以外の場合は、コンパイル時エラーが発生します。


## <a name="loop-statements"></a>Loop ステートメント

Loop ステートメントを使用すると、本体でステートメントを繰り返し実行できます。

```antlr
LoopStatement
    : WhileStatement
    | DoLoopStatement
    | ForStatement
    | ForEachStatement
    ;
```

ループ本体が入力されるたびに、その本体で宣言されたすべてのローカル変数の新しいコピーが作成され、変数の前の値に初期化されます。 ループ本体内の変数への参照は、最後に作成されたコピーを使用します。 このコードは例を示しています。

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

このコードでは、次の出力が生成されます。

```console
31    32    33
```

ループ本体が実行されると、変数のどのコピーが最新であるかが使用されます。 たとえば、ステートメント `Dim y = x` は、`y` の最新のコピーと `x`の元のコピーを参照します。 また、ラムダが作成されると、変数のコピーが作成された時点で最新だったことが記憶されます。 したがって、各ラムダは `x`の同じ共有コピーを使用しますが、`y`の異なるコピーを使用します。 プログラムの最後にラムダを実行すると、そのすべてが参照している `x` の共有コピーが最終的な値3になります。

ラムダ式または LINQ 式がない場合は、ループエントリに対して新しいコピーが作成されていることを認識できないことに注意してください。 実際には、コンパイラの最適化によって、この場合にコピーを作成することは避けられます。 また、ラムダ式または LINQ 式を含むループに `GoTo` するのは無効であることにも注意してください。


### <a name="whileend-while-and-doloop-statements"></a>しばらくお待ちください...しばらくしてから終了します...Loop ステートメント

`While` または `Do` loop ステートメントは、ブール式に基づいてループします。

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

ブール式が true と評価されている限り、`While` loop ステートメントはループします。`Do` loop ステートメントには、より複雑な条件を含めることができます。 式は、`Do` キーワードの後、または `Loop` キーワードの後に配置できますが、両方の後に配置することはできません。 ブール式は、セクションごとの[ブール式](expressions.md#boolean-expressions)として評価されます。 (注: この場合、式はブール型である必要はありません)。 式をまったく指定しないこともできます。この場合、ループは終了しません。 `Do`後に式を配置した場合は、反復処理のたびにループブロックが実行される前に評価されます。 `Loop`後に式が配置された場合は、各反復処理でループブロックが実行された後に評価されます。 したがって、`Loop` の後に式を配置すると、`Do`後の配置よりも1つ多くのループが生成されます。 次の例は、この動作を示しています。

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

このコードでは、次の出力が生成されます。

```console
Second Loop
```

最初のループの場合は、ループが実行される前に条件が評価されます。 2番目のループの場合は、ループの実行後に条件が実行されます。 条件式の先頭には、`While` キーワードまたは `Until` キーワードを付ける必要があります。 条件が false と評価された場合、前者はループを中断します。後者の場合、条件が true と評価されます。

__付箋.__ `Until` は予約語ではありません。


### <a name="fornext-statements"></a>...次のステートメント

`For...Next` ステートメントは、一連の境界に基づいてループします。 `For` ステートメントでは、ループコントロール変数、下限式、上限式、および省略可能なステップ値式を指定します。 ループコントロール変数は、識別子の後に省略可能な `As` 句または式を指定して指定します。

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

次の規則に従って、loop コントロール変数は、この `For...Next` ステートメント、または既存の変数または式に固有の新しいローカル変数を参照します。

* Loop コントロール変数が `As` 句を含む識別子の場合、識別子は、`As` 句で指定された型の新しいローカル変数を定義します。この変数の範囲は `For` ループ全体になります。

* Loop コントロール変数が `As` 句を含まない識別子である場合、識別子はまず単純な名前解決規則 (「 [Simple Name 式](expressions.md#simple-name-expressions)」を参照) を使用して解決されます。この識別子が違うではなく、それ自体が暗黙的なローカル変数を作成することになります (「[暗黙のローカル宣言](statements.md#implicit-local-declarations)」を参照してください)。

  * この解決が成功し、結果が変数として分類される場合、loop コントロール変数は既存の変数になります。

  * 解決が失敗した場合、または解決が成功し、結果が型として分類された場合は、次のようになります。
    * ローカル変数の型の推論が使用されている場合、識別子は、バインドされた式およびステップ式から推論される型を持つ新しいローカル変数を定義し、`For` ループ全体を対象とします。
    * ローカル変数の型の推定が使用されていないが、暗黙的なローカル宣言がの場合は、スコープがメソッド全体 (セクションの[暗黙のローカル宣言](statements.md#implicit-local-declarations)) であり、ループコントロール変数がこの既存の変数を参照する、暗黙的なローカル変数が作成されます。
    * ローカル変数の型の推論も暗黙的なローカル宣言も使用されていない場合は、エラーになります。

  * 型でも変数でもないように分類された解決が成功すると、エラーになります。

* Loop コントロール変数が式である場合は、式を変数として分類する必要があります。

ループコントロール変数は、それを囲む外側の `For...Next` ステートメントでは使用できません。 `For` ステートメントのループ制御変数の型によって、イテレーションの型が決定されます。次のいずれかである必要があります。

* `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`
* 列挙型
* `Object`
* 次の演算子を持つ型 `T`。 `B` は、ブール式で使用できる型です。

```vb
Public Shared Operator >= (op1 As T, op2 As T) As B
Public Shared Operator <= (op1 As T, op2 As T) As B
Public Shared Operator - (op1 As T, op2 As T) As T
Public Shared Operator + (op1 As T, op2 As T) As T
```

バインドおよびステップ式は、ループコントロール変数の型に暗黙的に変換できる必要があり、値として分類される必要があります。 コンパイル時に、ループコントロール変数の型は、下限、上限、およびステップ式の型の中で最も幅の広い型を選択することによって推論されます。 2つの型の間に拡大変換がない場合、コンパイル時エラーが発生します。

実行時に、ループコントロール変数の型が `Object`場合、反復処理の型はコンパイル時と同じように推論されますが、2つの例外があります。 最初に、バインドされた式とステップ式がすべて整数型であり、最も幅の広い型がない場合、3つすべての型を含む最も幅の広い型が推論されます。 次に、ループコントロール変数の型が `String`されると推論される場合は、代わりに `Double` が推論されます。 実行時に loop コントロール型を決定できない場合、またはいずれかの式を loop コントロール型に変換できない場合は、`System.InvalidCastException` が発生します。 ループの先頭でループコントロール型が選択されると、ループコントロール変数の値に加えられた変更に関係なく、同じ型がイテレーション全体で使用されます。

`For` ステートメントは、一致する `Next` ステートメントで閉じる必要があります。 変数のない `Next` ステートメントは最も内側にある open `For` ステートメントと一致しますが、1つ以上の loop コントロール変数を持つ `Next` ステートメントは左から右に、各変数に一致する `For` ループと一致します。 変数が、その時点で最も入れ子になっているループではない `For` ループと一致する場合、コンパイル時エラーが発生します。

ループの先頭では、3つの式がテキスト順に評価され、下限の式が loop コントロール変数に代入されます。 ステップ値を省略した場合は、暗黙的にリテラル `1`になり、ループコントロール変数の型に変換されます。 3つの式は、ループの先頭でのみ評価されます。

各ループの先頭では、コントロール変数が比較され、ステップ式が正の場合はエンドポイントより大きいか、またはステップ式が負の場合は終了点よりも小さいかどうかが確認されます。 の場合、`For` ループは終了します。それ以外の場合は、ループブロックが実行されます。 ループコントロール変数がプリミティブ型でない場合、比較演算子は `step >= step - step` 式が true か false かによって決まります。 `Next` ステートメントでは、ステップの値がコントロール変数に追加され、実行がループの先頭に戻ります。

Loop コントロール変数の新しいコピーは、ループブロックの反復処理のたびに作成され*ない*ことに注意してください。 この点で、`For` ステートメントは `For Each` ([各...次のステートメント](statements.md#for-eachnext-statements))。

ループの外側から `For` ループに分岐することは無効です。


### <a name="for-eachnext-statements"></a>各...次のステートメント

`For Each...Next` ステートメントは、式の要素に基づいてループします。 `For Each` ステートメントは、ループコントロール変数と列挙子式を指定します。 ループコントロール変数は、識別子の後に省略可能な `As` 句または式を指定して指定します。

```antlr
ForEachStatement
    : 'For' 'Each' LoopControlVariable 'In' LineTerminator? Expression StatementTerminator
      Block?
      ( 'Next' NextExpressionList? StatementTerminator )?
    ;
```

`For...Next` ステートメントと同じ規則に従っている (.. [.次のステートメント](statements.md#fornext-statements)) では、ループコントロール変数は、それぞれに固有の新しいローカル変数を参照します。次のステートメント、または既存の変数または式を指定します。

列挙子式は値として分類する必要があり、その型はコレクション型または `Object`である必要があります。 列挙子式の型が `Object`場合、すべての処理が実行時まで延期されます。 それ以外の場合は、コレクションの要素型からループコントロール変数の型への変換が存在している必要があります。

Loop コントロール変数は、それを囲む外側の `For Each` ステートメントでは使用できません。 `For Each` ステートメントは、一致する `Next` ステートメントで閉じる必要があります。 ループコントロール変数のない `Next` ステートメントは、最も内側にあるオープン `For Each`と一致します。 1つ以上の loop コントロール変数を持つ `Next` ステートメントは、左から右に、同じ loop コントロール変数を持つ `For Each` ループと一致します。 変数が、その時点で最も入れ子になっているループではない `For Each` ループと一致する場合、コンパイル時エラーが発生します。

型 `C` は、次のいずれかの場合に*コレクション型*と呼ばれます。

* 次のすべてが当てはまります。
  * `C` には、`E`型を返すシグネチャ `GetEnumerator()` を持つ、アクセス可能なインスタンス、共有、または拡張メソッドが含まれています。
  * `E` には、シグネチャ `MoveNext()` と戻り値の型 `Boolean`を持つ、アクセス可能なインスタンス、共有、または拡張メソッドが含まれています。
  * `E` には、getter を持つ `Current` という名前のアクセス可能なインスタンスまたは共有プロパティが含まれています。 このプロパティの型は、コレクション型の要素型です。

* インターフェイス `System.Collections.Generic.IEnumerable(Of T)`を実装します。この場合、コレクションの要素型は `T`と見なされます。

* インターフェイス `System.Collections.IEnumerable`を実装します。この場合、コレクションの要素型は `Object`と見なされます。

列挙できるクラスの例を次に示します。

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

ループが開始される前に、列挙子式が評価されます。 式の型がデザインパターンを満たしていない場合、式は `System.Collections.IEnumerable` または `System.Collections.Generic.IEnumerable(Of T)`にキャストされます。 式の型でジェネリックインターフェイスが実装されている場合、ジェネリックインターフェイスはコンパイル時に優先されますが、非ジェネリックインターフェイスは実行時に優先されます。 式の型でジェネリックインターフェイスが複数回実装されている場合、ステートメントはあいまいであると見なされ、コンパイル時エラーが発生します。

__付箋.__ 非ジェネリックインターフェイスは、ジェネリックインターフェイスを選択すると、インターフェイスメソッドのすべての呼び出しに型パラメーターが含まれるということを意味するため、遅延バインディングの場合に推奨されます。 実行時に一致する型引数を知ることはできないため、このような呼び出しはすべて遅延バインディング呼び出しを使用して行う必要があります。 非ジェネリックインターフェイスは、コンパイル時の呼び出しを使用して呼び出すことができるため、非ジェネリックインターフェイスを呼び出すよりも遅くなります。

結果の値に対して `GetEnumerator` が呼び出され、関数の戻り値は一時的なに格納されます。 その後、各イテレーションの開始時に、`MoveNext` が一時的に呼び出されます。 `False`が返された場合、ループは終了します。 それ以外の場合、ループの各反復は次のように実行されます。

1. ループコントロール変数が、既存の変数ではなく新しいローカル変数を識別した場合、このローカル変数の新しいコピーが作成されます。 現在の反復処理では、ループブロック内のすべての参照がこのコピーを参照します。
2. `Current` プロパティが取得され、ループコントロール変数の型に変換されます (変換が暗黙的か明示的かに関係なく)、ループコントロール変数に割り当てられます。
3. ループブロックが実行されます。

__付箋.__ 言語のバージョン10.0 と11.0 の間で動作が多少変更されています。 11.0 より前では、ループの反復ごとに新しい反復変数は作成され*ません*でした。 この違いは、反復変数がラムダまたはループの後に呼び出される LINQ 式によってキャプチャされた場合にのみ観測可能です。

```vb
Dim lambdas As New List(Of Action)
For Each x In {1,2,3}
   lambdas.Add(Sub() Console.WriteLine(x)
Next
lambdas(0).Invoke()
lambdas(1).Invoke()
lambdas(2).Invoke()
```

Visual Basic 10.0 までは、コンパイル時に警告が生成され、"3" が3回出力されました。 これは、ループのすべての反復処理によって共有される変数 "x" が1つだけであり、3つのラムダすべてが同じ "x" をキャプチャし、ラムダが実行されたときに、数値3を保持したためです。 Visual Basic 11.0 では、"1, 2, 3" が出力されます。 これは、各ラムダが異なる変数 "x" をキャプチャするためです。


__付箋.__ ステートメントに変換演算子を導入するための便利な場所がないため、変換が明示的である場合でも、反復処理の現在の要素はループコントロール変数の型に変換されます。 これは、現在廃止されている型 `System.Collections.ArrayList`を操作するときに特に厄介になりました。これは、その要素型が `Object`ためです。 これには、非常に多くのループで必要なキャストが含まれています。これは理想的ではありませんでした。 皮肉なことでは、厳密に型指定されたコレクションを作成できるようになりました。 `System.Collections.Generic.List(Of T)`はこのデザインポイントを再考する可能性がありますが、互換性を確保するためにこれを変更することはできません。


`Next` ステートメントに到達すると、実行がループの先頭に戻ります。 `Next` キーワードの後に変数を指定する場合は、`For Each`の後の最初の変数と同じである必要があります。 次に例を示します。

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

これは、次のコードと同じです。

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

列挙子の型 `E` が `System.IDisposable`を実装している場合は、`Dispose` メソッドを呼び出すことによって、ループの終了時に列挙子が破棄されます。 これにより、列挙子によって保持されているリソースが解放されます。 `For Each` ステートメントを含むメソッドが非構造化エラー処理を使用しない場合は、クリーンアップを確実にするために、`Finally` で `Dispose` メソッドを使用して `Try` ステートメントで `For Each` ステートメントをラップします。

__付箋.__ `System.Array` 型はコレクション型であり、すべての配列型が `System.Array`から派生しているため、`For Each` ステートメントで任意の配列型の式を使用できます。 1次元配列の場合、`For Each` のステートメントでは、インデックスの順序を大きくして配列の要素を列挙します。インデックスの順序は、インデックス0から始まり、インデックスの長さ-1 で終わります。 多次元配列の場合は、右端の次元のインデックスが先に増加します。

たとえば、次のコードでは `1 2 3 4`が出力されます。

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

ブロックの外側から `For Each` ステートメントブロックに分岐することは無効です。


## <a name="exception-handling-statements"></a>例外処理ステートメント

Visual Basic は、構造化例外処理と非構造化例外処理をサポートしています。 メソッドで使用できるのは1つのスタイルの例外処理のみですが、構造化例外処理では、`Error` ステートメントを使用できます。 メソッドが例外処理の両方のスタイルを使用する場合、コンパイル時エラーが発生します。

```antlr
ErrorHandlingStatement
    : StructuredErrorStatement
    | UnstructuredErrorStatement
    ;
```

### <a name="structured-exception-handling-statements"></a>構造化例外処理ステートメント

構造化例外処理は、特定の例外が処理される明示的なブロックを宣言することでエラーを処理する方法です。 構造化例外処理は、`Try` ステートメントを使用して行われます。

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


例 :

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

`Try` ステートメントは、try ブロック、catch ブロック、および finally ブロックの3種類のブロックで構成されます。 *Try ブロック*は、実行するステートメントを含むステートメントブロックです。 *Catch ブロック*は、例外を処理するステートメントブロックです。 *Finally ブロック*は、例外が発生して処理されたかどうかに関係なく、`Try` ステートメントが終了したときに実行されるステートメントを含むステートメントブロックです。 Try ブロックと finally ブロックを1つだけ含めることができる `Try` ステートメントには、少なくとも1つの catch ブロックまたは finally ブロックが含まれている必要があります。 同じステートメント内の catch ブロック内からを除き、try ブロックに明示的に実行を転送することは無効です。


#### <a name="finally-blocks"></a>Finally ブロック

`Finally` ブロックは、実行が `Try` ステートメントの任意の部分を離れると常に実行されます。 `Finally` ブロックを実行するために明示的な操作は必要ありません。実行時に `Try` ステートメントが実行されると、システムは自動的に `Finally` ブロックを実行し、実行を目的の宛先に転送します。 `Finally` ブロックは、実行によって `Try` ステートメントが終了する方法に関係なく実行されます。 `Try` `Catch` ブロックの最後まで、`Exit Try` ステートメントを使用して、またはスローされた例外を処理しないことによって、ステートメントを実行します。`GoTo`

Async メソッドの `Await` 式、および iterator メソッドの `Yield` ステートメントでは、async メソッドまたは iterator メソッドのインスタンスで制御のフローが中断され、他のメソッドインスタンスで再開される場合があることに注意してください。 ただし、これは実行の中断にすぎず、それぞれの非同期メソッドまたは反復子メソッドのインスタンスを終了する必要がないため、`Finally` ブロックが実行されません。

実行を明示的に `Finally` ブロックに転送するのは無効です。また、例外を除き、`Finally` ブロックの外で実行を転送することは無効です。

```antlr
FinallyStatement
    : 'Finally' StatementTerminator
      Block?
    ;
```

#### <a name="catch-blocks"></a>catch ブロック

`Try` ブロックの処理中に例外が発生した場合、各 `Catch` ステートメントは、例外を処理するかどうかを判断するためにテキスト順に検査されます。

```antlr
CatchStatement
    : 'Catch' ( Identifier ( 'As' NonArrayTypeName )? )?
      ( 'When' BooleanExpression )? StatementTerminator
      Block?
    ;
```

`Catch` 句で指定された識別子は、スローされた例外を表します。 識別子に `As` 句が含まれている場合、識別子は、`Catch` ブロックのローカル宣言領域内で宣言されていると見なされます。 それ以外の場合、識別子は、含んでいるブロックで定義されたローカル変数 (静的変数ではない) である必要があります。

識別子のない `Catch` 句は、`System.Exception`から派生したすべての例外をキャッチします。 識別子を持つ `Catch` 句でキャッチされるのは、識別子の型と同じか、またはその型から派生した型を持つ例外だけです。 型は `System.Exception`であるか、または `System.Exception`から派生した型である必要があります。 `System.Exception`から派生した例外がキャッチされると、例外オブジェクトへの参照は、関数 `Microsoft.VisualBasic.Information.Err`によって返されるオブジェクトに格納されます。

`When` 句を含む `Catch` 句は、式が `True`に評価された場合にのみ例外をキャッチします。式の型は、項[ブール式](expressions.md#boolean-expressions)に従ってブール式である必要があります。 `When` 句は、例外の型をチェックした後にのみ適用されます。この例で示すように、式は例外を表す識別子を参照する場合があります。

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

次の例では、が出力されます。

```console
Third handler
```

`Catch` 句が例外を処理する場合、実行は `Catch` ブロックに転送されます。 `Catch` ブロックの最後で、実行は `Try` ステートメントに続く最初のステートメントに転送されます。 `Try` ステートメントは、`Catch` ブロックでスローされた例外を処理しません。 `Catch` 句が例外を処理しない場合、実行はシステムによって決定された場所に転送されます。

実行を明示的に `Catch` ブロックに転送することは無効です。

When 句のフィルターは、通常、例外がスローされる前に評価されます。 たとえば、次のコードでは、"Filter, Finally, Catch" が出力されます。

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

 ただし、Async メソッドと Iterator メソッドでは、の内部のすべてのブロックが、以外のフィルターの前に実行されます。 たとえば、上記のコードが `Async Sub Foo()`た場合、出力は "Finally, Filter, Catch" になります。


#### <a name="throw-statement"></a>Throw ステートメント

`Throw` ステートメントは、`System.Exception`から派生した型のインスタンスによって表される例外を発生させます。

```antlr
ThrowStatement
    : 'Throw' Expression? StatementTerminator
    ;
```

式が値として分類されていない場合、または `System.Exception`から派生した型ではない場合、コンパイル時エラーが発生します。 実行時に式が null 値に評価された場合は、代わりに `System.NullReferenceException` 例外が発生します。

`Throw` ステートメントは、finally ブロックが介在しない限り、`Try` ステートメントの catch ブロック内の式を省略できます。 その場合、ステートメントは、catch ブロック内で現在処理されている例外を再スローします。 例 :

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


### <a name="unstructured-exception-handling-statements"></a>非構造化例外処理ステートメント

非構造化例外処理は、例外が発生したときに分岐するステートメントを示すことによってエラーを処理する方法です。 非構造化例外処理は、`Error` ステートメント、`On Error` ステートメント、および `Resume` ステートメントの3つのステートメントを使用して実装されます。

```antlr
UnstructuredErrorStatement
    : ErrorStatement
    | OnErrorStatement
    | ResumeStatement
    ;
```

例 :

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

メソッドが非構造化例外処理を使用する場合、すべての例外をキャッチするメソッド全体に対して1つの構造化例外ハンドラーが確立されます。 (コンストラクターでは、このハンドラーは、コンストラクターの先頭で `New` への呼び出しに対しては拡張されないことに注意してください)。メソッドは、最後に発生した例外ハンドラーの場所と、スローされた最新の例外を追跡します。 メソッドへの入力時には、例外ハンドラーの場所と例外の両方が `Nothing`に設定されます。 非構造化例外処理を使用するメソッドで例外がスローされると、例外オブジェクトへの参照は、関数 `Microsoft.VisualBasic.Information.Err`によって返されるオブジェクトに格納されます。

非構造化エラー処理ステートメントは、反復子または非同期メソッドでは使用できません。


#### <a name="error-statement"></a>Error ステートメント

`Error` ステートメントは、Visual Basic 6 個の例外番号を含む `System.Exception` 例外をスローします。 式は値として分類する必要があり、その型は `Integer`に暗黙的に変換可能である必要があります。

```antlr
ErrorStatement
    : 'Error' Expression StatementTerminator
    ;
```

#### <a name="on-error-statement"></a>On Error ステートメント

`On Error` ステートメントは、最新の例外処理状態を変更します。

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

次の4つの方法のいずれかで使用できます。

* `On Error GoTo -1` により、最新の例外が `Nothing`にリセットされます。

* `On Error GoTo 0` により、最新の例外ハンドラーの場所が `Nothing`にリセットされます。

* `On Error GoTo LabelName` は、最新の例外ハンドラーの場所としてラベルを設定します。 このステートメントは、ラムダ式またはクエリ式を含むメソッドでは使用できません。

* `On Error Resume Next` は、`Resume Next` の動作を最新の例外ハンドラーの場所として設定します。


#### <a name="resume-statement"></a>Resume ステートメント

`Resume` ステートメントは、最後の例外の原因となったステートメントに実行を返します。

```antlr
ResumeStatement
    : 'Resume' ResumeClause? StatementTerminator
    ;

ResumeClause
    : 'Next'
    | LabelName
    ;
```

`Next` 修飾子が指定されている場合、実行は、最新の例外の原因となったステートメントの後に実行されたステートメントに戻ります。 ラベル名が指定されている場合、実行はラベルに戻ります。

`SyncLock` ステートメントには暗黙的な構造化エラー処理ブロックが含まれているので、`Resume` と `Resume Next` には `SyncLock` ステートメントで発生する例外に対する特別な動作があります。 `Resume` は `SyncLock` ステートメントの先頭に実行を返し、`Resume Next` は `SyncLock` ステートメントに続く次のステートメントに実行を返します。 次に例を示します。

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

次の結果が出力されます。

```console
Before exception
Before exception
After SyncLock
```

最初に `SyncLock` ステートメントを実行すると、`Resume` は `SyncLock` ステートメントの先頭に実行を返します。 2回目に `SyncLock` ステートメントを実行すると、`Resume Next` は `SyncLock` ステートメントの最後まで実行を戻します。 `Resume` と `Resume Next` は `SyncLock` ステートメント内では許可されていません。

どのような場合でも、`Resume` ステートメントを実行すると、最新の例外が `Nothing`に設定されます。 最新の例外がない状態で `Resume` ステートメントを実行すると、ステートメントは Visual Basic エラー番号 `20` (エラーなしで再開) を含む `System.Exception` 例外を発生させます。


## <a name="branch-statements"></a>Branch ステートメント

分岐ステートメントは、メソッドの実行フローを変更します。 次の6つの分岐ステートメントがあります。

1. `GoTo` ステートメントを実行すると、メソッド内の指定したラベルに実行が転送されます。 このブロックのローカル変数がラムダ式または LINQ 式でキャプチャされている場合、`Try`、`Using`、`SyncLock`、`With`、`For`、または `For Each` ブロックに `GoTo` することはできません。
2. `Exit` ステートメントは、指定された種類のブロックステートメントの直後にあるステートメントの最後の次のステートメントに実行を転送します。 ブロックがメソッドブロックの場合、制御フローは「[制御フロー](statements.md#control-flow)」で説明されているようにメソッドを終了します。 ステートメントで指定されたブロックの種類に `Exit` ステートメントが含まれていない場合、コンパイル時エラーが発生します。
3. `Continue` ステートメントは、指定された種類のブロックループステートメントの最後まで実行を転送します。 ステートメントで指定されたブロックの種類に `Continue` ステートメントが含まれていない場合、コンパイル時エラーが発生します。
4. `Stop` ステートメントを実行すると、デバッガーの例外が発生します。
5. `End` ステートメントは、プログラムを終了します。 ファイナライザーはシャットダウンの前に実行されますが、現在実行中の `Try` ステートメントの最後のブロックは実行されません。 このステートメントは、実行可能ではないプログラム (Dll など) では使用できません。
6. 式が指定されていない `Return` ステートメントは、`Exit Sub` または `Exit Function` ステートメントと同じです。 式を含む `Return` ステートメントは、関数である通常のメソッド、または一部の `T`の戻り値の型 `Task(Of T)` を持つ関数である非同期メソッドでのみ使用できます。 式は、*関数の戻り変数*(通常のメソッドの場合) または*タスクの戻り変数*(非同期メソッドの場合) に暗黙的に変換できる値として分類する必要があります。 その動作は、式を評価し、戻り変数に格納した後、暗黙的な `Exit Function` ステートメントを実行することです。

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

## <a name="array-handling-statements"></a>配列処理ステートメント

2つのステートメントでは、配列の操作が簡単になります。 `ReDim` ステートメントと `Erase` ステートメントです。

```antlr
ArrayHandlingStatement
    : RedimStatement
    | EraseStatement
    ;
```

### <a name="redim-statement"></a>ReDim ステートメント

`ReDim` ステートメントは、新しい配列をインスタンス化します。

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

ステートメント内の各句は、変数またはプロパティアクセスとして分類される必要があります。型は配列型または `Object`であり、その後に配列の範囲のリストが続きます。 境界の数は、変数の型と一致している必要があります。`Object`には任意の数の境界を指定できます。 実行時に、配列は、式ごとに指定された境界を使用して左から右にインスタンス化され、変数またはプロパティに割り当てられます。 変数の型が `Object`の場合、次元の数は指定された次元の数になり、配列要素の型は `Object`になります。 指定された次元数が実行時に変数またはプロパティと互換性がない場合、コンパイル時エラーが発生します。 例 :

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

`Preserve` キーワードを指定する場合は、式も値として classifiable する必要があります。また、右端の次元以外の各次元の新しいサイズは、既存の配列のサイズと同じである必要があります。 既存の配列内の値が新しい配列にコピーされます。新しい配列が小さい場合、既存の値は破棄されます。新しい配列が大きい場合、追加の要素は配列の要素型の既定値に初期化されます。 次に例を示します。

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

次の結果が出力されます。

```console
3, 0
```

実行時に既存の配列参照が null 値の場合、エラーは発生しません。 右端のディメンション以外は、ディメンションのサイズが変更されると `System.ArrayTypeMismatchException` がスローされます。

__付箋.__ `Preserve` は予約語ではありません。


### <a name="erase-statement"></a>Erase ステートメント

`Erase` ステートメントは、ステートメントで指定された各配列変数またはプロパティを `Nothing`に設定します。 ステートメント内の各式は、型が配列型または `Object`である変数またはプロパティアクセスとして分類される必要があります。 例 :

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

## <a name="using-statement"></a>Using ステートメント

コレクションが実行され、インスタンスへのライブ参照が見つからない場合、型のインスタンスはガベージコレクターによって自動的に解放されます。 型が特に貴重なリソース (データベース接続やファイルハンドルなど) を保持している場合は、次のガベージコレクションが使用されなくなった型の特定のインスタンスをクリーンアップするまで待機することは望ましくありません。 コレクションの前にリソースを解放するための軽量な方法を提供するために、型は `System.IDisposable` インターフェイスを実装する場合があります。 これを行う型は、重要なリソースを直ちに解放するために呼び出すことができる `Dispose` メソッドを公開します。次にその例を示します。

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

`Using` ステートメントは、リソースを取得し、一連のステートメントを実行して、リソースを破棄するプロセスを自動化します。 このステートメントは2つの形式を取ることができます。1つは、リソースがステートメントの一部として宣言され、通常のローカル変数宣言ステートメントとして扱われるローカル変数です。もう1つのリソースは、式の結果です。

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

リソースがローカル変数宣言ステートメントである場合、ローカル変数宣言の型は、暗黙的に `System.IDisposable`に変換できる型である必要があります。 宣言されたローカル変数は読み取り専用であり、`Using` ステートメントブロックを対象としており、初期化子を含める必要があります。 リソースが式の結果である場合、式は値として分類される必要があり、`System.IDisposable`に暗黙的に変換できる型である必要があります。 式は、ステートメントの先頭で1回だけ評価されます。

`Using` ブロックは、finally ブロックがリソースのメソッド `IDisposable.Dispose` を呼び出す `Try` ステートメントによって暗黙的に含まれています。 これにより、例外がスローされた場合でも、リソースが確実に破棄されます。 その結果、ブロックの外部から `Using` ブロックに分岐することは無効になり、`Using` ブロックは `Resume` と `Resume Next`のために1つのステートメントとして扱われます。 リソースが `Nothing`場合、`Dispose` への呼び出しは行われません。 そのため、例を次に示します。

```vb
Using f As C = New C()
    ...
End Using
```

上の式は、下の式と同等です。

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

ローカル変数宣言ステートメントを持つ `Using` ステートメントは、同時に複数のリソースを取得できます。これは、入れ子になった `Using` ステートメントと同じです。  たとえば、次のような形式の `Using` ステートメントがあります。

```vb
Using r1 As R = New R(), r2 As R = New R()
    r1.F()
    r2.F()
End Using
```

上の式は、下の式と同等です。

```vb
Using r1 As R = New R()
    Using r2 As R = New R()
        r1.F()
        r2.F()
    End Using
End Using
```


## <a name="await-statement"></a>Await ステートメント

Await ステートメントは、await 演算子式 (Section [Await 演算子](expressions.md#await-operator)) と同じ構文を使用します。 await 式を許可するメソッドでのみ使用できます。また、await 演算子式と同じ動作をします。

ただし、値または void として分類される場合があります。 Await 演算子式の評価によって得られた値はすべて破棄されます。

```antlr
AwaitStatement
    : AwaitOperatorExpression StatementTerminator
    ;
```

## <a name="yield-statement"></a>Yield ステートメント

Yield ステートメントは、「 [Iterator メソッド](statements.md#iterator-methods)」で説明されている反復子メソッドに関連しています。

```antlr
YieldStatement
    : 'Yield' Expression StatementTerminator
    ;
```

`Yield` は、すぐ外側のメソッドまたはラムダ式が表示される場合、`Iterator` 修飾子を持ち、その `Iterator` 修飾子の後に `Yield` が表示される場合、予約語になります。他の場所では予約されていません。 プリプロセッサディレクティブでも予約されていません。 Yield ステートメントは、予約語であるメソッドまたはラムダ式の本体でのみ使用できます。 すぐ外側のメソッドまたはラムダ内では、yield ステートメントは、`Catch` または `Finally` ブロックの本体、または `SyncLock` ステートメントの本体の内部には記述できません。

Yield ステートメントは、1つの式を受け取ります。この式は、値として分類される必要があり、その型は、それを囲む反復[子メソッドの](statements.md#iterator-methods)*反復子の現在の変数*の型に暗黙的に変換できます。

制御フローは、反復子オブジェクトで `MoveNext` メソッドが呼び出された場合にのみ、`Yield` ステートメントに到達します。 (これは、反復子メソッドのインスタンスが、反復子オブジェクトで呼び出されている `MoveNext` または `Dispose` メソッドのためにステートメントを実行するだけであるためです。 `Dispose` メソッドでは、`Yield` が許可されていない `Finally` ブロックでのみコードが実行されます)。

`Yield` ステートメントを実行すると、その式が評価され、その反復子オブジェクトに関連付けられている iterator メソッドインスタンスの*反復子現在の変数*に格納されます。 `True` 値が `MoveNext`の呼び出し元に返されます。このインスタンスの制御ポイントは、iterator オブジェクトでの次の `MoveNext` の呼び出しまで進められなくなります。

