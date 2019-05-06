---
ms.openlocfilehash: 68237df3f66ba1cec661e6580c27bb5beedd6b9a
ms.sourcegitcommit: 6eca149bdc736113e0adb709212bd266c9503c33
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2019
ms.locfileid: "47426784"
---
# <a name="statements"></a>ステートメント

ステートメントは、実行可能コードを表します。

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

__注意してください。__ Microsoft Visual Basic コンパイラは、キーワードまたは識別子で始まるステートメントを許可するだけです。 したがって、呼び出しステートメントでは、"`Call (Console).WriteLine`「許可されますが、呼び出しステートメント」`(Console).WriteLine`"はありません。

## <a name="control-flow"></a>制御フロー

*制御フロー*ステートメントと式が実行されるシーケンスです。 実行順序は、特定のステートメントまたは式に依存します。

たとえば、加算演算子を評価するときに (セクション[加算演算子](expressions.md#addition-operator))、左オペランドを評価すると、最初、右側のオペランドとし、演算子自体。 ブロックが実行される (セクション[ブロックとラベル](statements.md#blocks-and-labels)) によって、最初のサブステートメントを最初に実行し、ブロックのステートメントを 1 つずつです。

この暗黙的な順序付けの概念は、*コントロール ポイント*、これは、次の操作が実行されます。 作成というメソッドが呼び出されます (または「と呼ばれる」)、ときに、*インスタンス*メソッドの。 メソッドのインスタンスは、独自のコピーのメソッドのパラメーターとローカル変数、および独自のコントロール ポイントで構成されます。

### <a name="regular-methods"></a>通常のメソッド

通常のメソッドの例を次に示します

```vb
Function Test() As Integer
    Console.WriteLine("hello")
    Return 1
End Sub

Dim x = Test()    ' invokes the function, prints "hello", assigns 1 to x
```

通常のメソッドが呼び出されます

1. まず、メソッドのインスタンスは、その呼び出しに固有で作成されます。 このインスタンスには、すべてのパラメーターのコピーと、メソッドのローカル変数が含まれています。
2. すべてのパラメーターは、指定された値、およびすべてのローカル変数の型の既定値に初期化されます。
3. 場合、 `Function`、暗黙のローカル変数も初期化と呼ばれる、*関数の戻り変数*名前が関数の名前、型が関数の戻り値の型と既定の初期値です型。
4. メソッド インスタンスの管理ポイントは、メソッド本体の最初のステートメントで設定し、すぐにメソッド本体をそこから実行が開始されると (セクション[ブロックとラベル](statements.md#blocks-and-labels))。

制御フローが終了したとき、メソッド本体通常どおりに到達を通じて、`End Function`または`End Sub`、末尾をマークするまたは明示的な`Return`または`Exit`メソッド インスタンスの呼び出し元ステートメントの制御フローを返します。 関数の戻り値の変数がある場合、呼び出しの結果、この変数の最終的な値です。

制御フローでは、ハンドルされない例外をメソッド本体を終了すると、その例外は、呼び出し元に伝達されます。

制御フローが終了すると、後に、メソッドのインスタンスをライブ参照が不要になった存在します。 メソッドのインスタンスにローカル変数またはパラメーターのコピーへの参照のみが保持されていると、ガベージ コレクションがある可能性があります。

### <a name="iterator-methods"></a>反復子メソッド

反復子メソッドがいずれかで使用できるシーケンスを生成する便利な方法として使用される、`For Each`ステートメント。 反復子メソッドを使用して、`Yield`ステートメント (セクション[Yield ステートメント](statements.md#yield-statement)) のシーケンスの要素を提供します。 (なしで反復子メソッド`Yield`ステートメント、空のシーケンスが生成されます)。 反復子メソッドの例を次に示します。

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

反復子メソッドが呼び出されると、戻り値の型は`IEnumerator(Of T)`、

1. まず、反復子メソッドのインスタンスは、その呼び出しに固有で作成されます。 このインスタンスには、すべてのパラメーターのコピーと、メソッドのローカル変数が含まれています。
2. すべてのパラメーターは、指定された値、およびすべてのローカル変数の型の既定値に初期化されます。
3. 暗黙のローカル変数も初期化と呼ばれる、*反復子の現在の変数*、型が`T`し、初期値がその型の既定値。
4. メソッド インスタンスの管理ポイントは、メソッド本体の最初のステートメントで設定されます。
5. *反復子オブジェクト*し、作成されると、このメソッドのインスタンスに関連付けられています。 反復子オブジェクトは、宣言された戻り値の型を実装し、以下に示すように動作します。
6. 制御フローが再開し*直ちに*呼び出し元の呼び出しの結果は、反復子オブジェクトとします。 この転送は、反復子メソッドのインスタンスを終了せずに行われ、最後を実行するハンドラーは発生しないことに注意してください。 メソッドのインスタンスが、反復子オブジェクトで参照されていると、ガベージ収集されません、反復子オブジェクトへのライブ参照が存在する限り、します。

ときに、反復子オブジェクトの`Current`プロパティにアクセスした、*現在変数*の呼び出しが返されます。

ときに、反復子オブジェクトの`MoveNext`メソッドが呼び出される、呼び出しがメソッドの新しいインスタンスを作成できません。 代わりに、既存のメソッドのインスタンスが使用されます (とその管理ポイントとローカル変数とパラメーター) の反復子メソッドが最初に呼び出されたときに作成されたインスタンス。 制御フローでは、そのメソッド インスタンスのコントロール ポイントで実行を再開し、通常どおり、iterator メソッドの本体を実行します。

ときに、反復子オブジェクトの`Dispose`メソッドが呼び出される、もう一度既存のメソッドのインスタンスが使用されます。 そのメソッド インスタンスのコントロール ポイントで実行フローが再開が、すぐに動作として、`Exit Function`ステートメントが次の操作。

上記の説明の呼び出しの動作の`MoveNext`または`Dispose`反復子オブジェクトのみの場合は、以前のすべてを適用の呼び出し`MoveNext`または`Dispose`その反復子オブジェクトに対して既にを返したものの呼び出し元。 まだ、動作は定義されません。

制御フローが終了したとき、反復子メソッドの本体普通に到達を通じて、 `End Function` 、末尾をマークするまたは明示的な`Return`または`Exit`--ステートメントにする必要がありますが行っての呼び出しのコンテキストで`MoveNext`または`Dispose` 、反復子メソッドのインスタンスを再開する、反復子オブジェクトに対して関数が使用している反復子メソッドが最初に呼び出されたときに作成されたメソッドのインスタンス。 そのインスタンスの制御点のままになって、`End Function`ステートメント、および呼び出し元に制御フローが再開への呼び出しが再開された場合と`MoveNext`値`False`が呼び出し元に返されます。

制御フローの呼び出しをもう一度値は、呼び出し元に未処理の例外をその例外を反復子メソッドの本体が伝達される終了時に`MoveNext`またはの`Dispose`します。

その他の考えられる戻り値の型、反復子関数の場合

* 反復子メソッドが呼び出されると、戻り値の型は`IEnumerable(Of T)`一部`T`インスタンスを最初に作成 - 反復子メソッドのメソッドでは、すべてのパラメーターの--の呼び出しに特定指定された値で初期化されます。 呼び出しの結果は、戻り値の型を実装するオブジェクト。 このオブジェクトの必要があります`GetEnumerator`メソッドが呼び出されると、特定の呼び出しによってその - インスタンスを作成`GetEnumerator`--すべてのパラメーターと、メソッドのローカル変数の。 既に保存すると、値にパラメーターを初期化し、上記の反復子メソッドの場合とが続行されます。
* 反復子メソッドが呼び出されると、戻り値の型は、非ジェネリック インターフェイス`IEnumerator`の動作はまったくとして`IEnumerator(Of Object)`します。
* 反復子メソッドが呼び出されると、戻り値の型は、非ジェネリック インターフェイス`IEnumerable`の動作はまったくとして`IEnumerable(Of Object)`します。

### <a name="async-methods"></a>非同期メソッド

非同期メソッドは、たとえば、アプリケーションの UI をブロックすることがなく長時間実行処理を実行する便利な方法です。 非同期は省略形で*非同期*-非同期メソッドの呼び出し元がその実行をすぐに、再開されますが、今後の後で、非同期メソッドのインスタンスの最終的な完了が発生することを意味します。 名前付け規則により非同期メソッドの前に、サフィックス"Async"。

```vb
Async Function TestAsync() As Task(Of String)
    Console.WriteLine("hello")
    Await Task.Delay(100)
    Return "world"
End Function

Dim t = TestAsync()         ' prints "hello"
Console.WriteLine(Await t)  ' prints "world"
```

__注意してください。__ 非同期メソッドは*いない*バック グラウンド スレッドで実行します。 メソッドを介して自身を中断することができます代わりに、`Await`何らかのイベントへの応答で再開するには、演算子、およびスケジュール自体。

非同期メソッドの呼び出し時に

1. まず、async メソッドのインスタンスは、その呼び出しに固有で作成されます。 このインスタンスには、すべてのパラメーターのコピーと、メソッドのローカル変数が含まれています。
2. すべてのパラメーターは、指定された値、およびすべてのローカル変数の型の既定値に初期化されます。
3. 非同期メソッドの戻り値の型の場合`Task(Of T)`のいくつか`T`、暗黙のローカル変数も初期化と呼ばれる、*タスク戻り変数*、型が`T`し、初期値が、既定値は`T`します。
4. 非同期メソッドがある場合、`Function`で型を返す`Task`または`Task(Of T)`一部`T`、し、その型のオブジェクトに暗黙的に作成、現在の呼び出しに関連付けられています。 これと呼ばれますが、*非同期オブジェクト*非同期メソッドのインスタンスを実行することによって行われますが、今後の作業を表します。 この非同期メソッドのインスタンスの呼び出し元のコントロールが再開されると、呼び出し元はその呼び出しの結果としてこの非同期オブジェクトに表示されます。
5. 非同期メソッドの本体の最初のステートメントで設定しのインスタンスの制御点とすぐにそこからのメソッド本体の実行が開始される (セクション[ブロックとラベル](statements.md#blocks-and-labels))。

__再開のデリゲートと現在の呼び出し元__

セクションで説明されている[Await 演算子](expressions.md#await-operator)の実行、`Await`式が別の場所を移動する制御フローのままメソッド インスタンスのコントロール ポイントを停止することです。 呼び出しからの同じインスタンスのコントロール ポイントでの制御フローを後で再開できます、*再開デリゲート*します。 この中断が非同期のメソッドを終了せずに行われ、最後を実行するハンドラーは発生しないことに注意してください。 メソッドのインスタンスが再開デリゲートで参照されていると、`Task`または`Task(Of T)`(あれば) が発生してはガベージ収集されませんデリゲートまたは結果のいずれかへのライブ参照が存在する限り、します。

ステートメントを想像することをお勧め`Dim x = Await WorkAsync()`次の構文の短縮形として約。

```vb
Dim temp = WorkAsync().GetAwaiter()
If Not temp.IsCompleted Then
       temp.OnCompleted(resumptionDelegate)
       Return [task]
       CONT:   ' invocation of 'resumptionDelegate' will resume here
End If
Dim x = temp.GetResult()
```

次の場合、*現在の呼び出し元*メソッドのインスタンスは元の呼び出し元、または再開のデリゲートの呼び出し元のいずれかとして定義されますより新しい方します。

制御フローが到達を通じて--async メソッドの本体が終了したとき、`End Sub`または`End Function`、末尾をマークするまたは明示的な`Return`または`Exit`ステートメント、またはに設定されているインスタンスの管理ポイントにハンドルされない例外--メソッドの末尾。 動作は、非同期メソッドの戻り値の型に依存します。

* 場合、`Async Function`で型を返す`Task`:
  1. 制御フローが終了し、ハンドルされない例外を通じて非同期オブジェクトの状態に設定されます`TaskStatus.Faulted`とその`Exception.InnerException`プロパティが、例外に設定 (を除く: などの特定の実装で定義された例外`OperationCanceledException`に変更`TaskStatus.Canceled`). 現在の呼び出し元に制御フローを再開します。
  2. 非同期オブジェクトの状態に設定する場合は、`TaskStatus.Completed`します。 現在の呼び出し元に制御フローを再開します。

     (__に注意してください。__ 全体ポイント タスク、および非同期メソッドを興味深い点はタスクになるときに完了し、それを待機していたすべてのメソッドが実行再開デリゲートが現在、ブロックされていないつまりなる。)

* 場合、`Async Function`型を返します`Task(Of T)`いくつかの`T`: としての動作は、非同期オブジェクトをケースでない例外を除く`Result`プロパティは、タスク戻り変数の最終的な値にも設定します。

* 場合、 `Async Sub`:
  1. 制御フローが終了し、ハンドルされない例外でその例外の場合は、いくつかの実装に固有の方法で、環境に反映されます。 現在の呼び出し元に制御フローを再開します。
  2. それ以外の場合、制御フローは、現在の呼び出し元に単に再開します。

#### <a name="async-sub"></a>Async Sub

一部の Microsoft 固有の動作がある、`Async Sub`します。

場合`SynchronizationContext.Current`は`Nothing`呼び出しの開始日、ハンドルされない例外、Async Sub から投稿されます Threadpool にします。

場合`SynchronizationContext.Current`ない`Nothing`し、呼び出しの開始時`OperationStarted()`メソッドの開始前に、そのコンテキストで呼び出されると`OperationCompleted()`終了後にします。 さらに、ハンドルされない例外は、同期コンテキストで再スローに投稿されます。

つまり、UI アプリケーションでの`Async Sub`UI スレッドで呼び出される、UI スレッド処理に失敗したすべての例外を再発行されるは。

#### <a name="mutable-structures-in-async-and-iterator-methods"></a>Async および反復子メソッドで変更可能な構造体

変更可能な構造一般し、見なされ不適切な手法は、非同期または反復子メソッドでサポートされていません。 具体的には、構造体の非同期または反復子メソッドの呼び出しごとが暗黙的に動作を*コピー*の呼び出しの時点でコピーされた構造体。 たとえば、このため、

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

実行可能ステートメントのグループには、ステートメント ブロックは呼び出されます。 ステートメント ブロックの実行、ブロックの最初のステートメントから始まります。 ステートメントを実行すると、構文の順序で次のステートメントを実行すると、ステートメントが実行を別の場所を転送するか、例外が発生した場合を除き、します。

ステートメント ブロック内では、論理行のステートメントの除算はラベルの宣言ステートメントを除く意味ではありません。 などの分岐ステートメントの対象として使用できるステートメント ブロック内で特定の位置を識別する識別子は、ラベルは`GoTo`します。

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


ラベルの宣言ステートメントは論理行の先頭に表示され、識別子またはリテラル整数のいずれかをラベルとして使用することがあります。 ラベルの宣言ステートメントと呼び出しステートメントの両方が 1 つの識別子で構成できますので、ローカル行の先頭に単一の識別子は、常に、ラベルの宣言ステートメントと見なされます。 同じ論理行の後にステートメントがない場合でも、コロン、ラベルの宣言ステートメントの後に必ず必要があります。

ラベルは、独自の宣言領域があるし、他の識別子に干渉することはできません。 次の例が有効であり、名前の変数を使用して`x`パラメーターとラベルの両方。

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

読みやすさ、複数サブステートメントに関連するステートメントの運用は、たとえ、サブステートメントがある場合それぞれ単独でラベル付けされた行にこの仕様では、1 つの運用環境として扱われます。


### <a name="local-variables-and-parameters"></a>ローカル変数とパラメーター

方法の詳細セクションの前に、メソッドのインスタンスを作成するときと、メソッドのローカル変数とパラメーターのコピーにします。 さらに、ループの本体が入力されるたびに新しいコピーが作成セクションで説明した、ループ内で宣言されている各ローカル変数の[Loop ステートメント](statements.md#loop-statements)メソッドのインスタンスが含まれています、ローカル変数のこのコピーにはではなく、前のコピーです。

すべてのローカル変数は、その型の既定値に初期化されます。 ローカル変数とパラメーターは、パブリックにアクセスでは常にします。 次の例に示すように、その宣言の前にあるテキストの位置にローカル変数を参照するとエラーにすることをお勧めします。

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

`F`上記のメソッドは、最初の割り当てを`i`具体的には、外側のスコープで宣言されているフィールドを参照しません。 代わりに、ローカル変数を参照でありエラー代入変数を宣言します。 `G`メソッド、後続の変数宣言は同じローカル変数の宣言内で前の変数宣言で宣言されたローカル変数を参照します。

メソッド内の各ブロックは、ローカル変数の宣言領域を作成します。 この宣言領域と、メソッドのパラメーター リストを使用して、メソッド本体のローカル変数宣言には、名前が導入されました。 ブロックは入れ子による名前のシャドウは許可されません。 名前は再宣言では、可能性がありますいないブロックの名前を宣言したら。

したがって、次の例で、`F`と`G`ために、メソッドがエラーでは、名前`i`は外側のブロックで宣言されており、内側のブロックで再宣言することはできません。 ただし、`H`と`I`メソッドは、有効なため、2 つ`i`の入れ子になっていない別のブロックで宣言されます。

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

メソッドは、関数が、特別なローカル変数は、関数の戻り値を表すメソッドと同じ名前のメソッド本体の宣言領域で暗黙的に宣言します。 ローカルの変数は、式で使用する場合の特別な名前解決のセマンティクスを持っています。 Invocation 式など、メソッド グループとして分類される式が必要とするコンテキストでローカル変数が使用されるローカル変数ではなく、関数に名前が解決します。 例:

```vb
Function F(i As Integer) As Integer
    If i = 0 Then
        F = 1        ' Sets the return value.
    Else
        F = F(i - 1) ' Recursive call.
    End If
End Function
```

かっこを使用できるあいまいな状況が発生する (など`F(1)`ここで、`F`は、関数の戻り値の型は、1 次元配列です)。 すべてのあいまいな状況で、名前のローカル変数ではなく、関数に解決されます。 例:

```vb
Function F(i As Integer) As Integer()
    If i = 0 Then
        F = new Integer(2) { 1, 2, 3 }
    Else
        F = F(i - 1) ' Recursive call, not an index.
    End If
End Function
```

制御フローがメソッド本体を離れると、ローカル変数の値は、invocation 式に渡されます。 メソッドがサブルーチンの場合は、このような暗黙のローカル変数がないと、コントロールは、単に、invocation 式を返します。

## <a name="local-declaration-statements"></a>ローカル宣言ステートメント

ローカル宣言ステートメントでは、新しいローカル変数、ローカル定数、または静的変数を宣言します。 *ローカル変数*と*ローカル定数*インスタンス変数とスコープをメソッドに定数に相当し、同じ方法で宣言します。 *静的変数*のような`Shared`と変数宣言を使用して、`Static`修飾子。

```antlr
LocalDeclarationStatement
    : LocalModifier VariableDeclarators StatementTerminator
    ;

LocalModifier
    : 'Static' | 'Dim' | 'Const'
    ;
```

静的変数は、メソッドの呼び出し間でそれらの値を保持するローカル変数です。 非共有メソッド内で宣言された静的変数はインスタンスごと: メソッドを含む型の各インスタンスが静的変数の独自のコピー。 内で宣言された静的変数`Shared`メソッドは、1 つのタイプは、すべてのインスタンスの静的変数のコピーは 1 つだけです。 メソッドには、各エントリに、型の既定値には、ローカル変数が初期化される、中に、静的変数は、型または型のインスタンスが初期化されるときに、型の既定値にのみ初期化します。 構造体またはジェネリック メソッドで、静的変数を宣言しない場合があります。

常にローカル変数、ローカル定数、および静的変数は、パブリック アクセシビリティを持つし、アクセシビリティ修飾子を指定しない場合があります。 ローカル宣言ステートメントで型が指定されていない場合、次の手順は、ローカル宣言の型を決定します。

1. 宣言型の文字がある場合、型の文字の型は、ローカル宣言の型になります。

2. ローカル宣言がローカルの定数の場合、またはローカル宣言は、ローカル変数初期化子を使用して、ローカル変数の型の推定が使用されている場合は、ローカル宣言の型は、初期化子の型から推論されます。 初期化子は、ローカル宣言を参照している場合、コンパイル時エラーが発生します。 (ローカル定数は、初期化子を持つに必要ですが)。

3. 厳密な型が使用されていない場合、ローカル宣言ステートメントの種類は暗黙的に`Object`します。

4. それ以外の場合は、コンパイル時のエラーが発生します。

配列のサイズまたは配列の型修飾子があるローカル宣言ステートメントで型が指定されていない場合は、ローカル宣言の型は、指定したランクを持つ配列し、前の手順を使用して、配列の要素の型を決定します。 ローカル変数の型推論を使用する場合、初期化子の種類はローカル宣言ステートメントと同じ配列形 (つまり配列型の修飾子) の配列型である必要があります。 推論される要素の型が配列型を引き続きする可能性があることに注意してください。 例:

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

ローカル宣言ステートメントで型が指定されていない場合、null 許容型修飾子を持つ場合は、null 許容型の値既に、ローカル宣言の型が推論される型または推論された型そのものの null 許容バージョン。  推論された型が null 許容にすることができる値型でない場合は、コンパイル時エラーが発生します。 Null 許容型修飾子と配列サイズまたは配列型修飾子の両方を配置しない型のローカル宣言ステートメント場合、は、null 許容型修飾子は配列の要素の型に適用すると見なされ、前の手順を使用して、eleme の決定nt の型。

ローカル宣言ステートメントの変数の初期化子は、宣言のテキストの場所に配置する代入ステートメントと同じです。 そのため、ローカル宣言ステートメントでは、実行が分岐、変数の初期化子は実行されません。 変数の初期化子が実行されるローカル宣言ステートメントを複数回実行する場合と同じ回数。 静的変数は、最初に初期化子を実行するだけです。 静的変数の初期化中に例外が発生した場合、静的変数は、静的変数の型の既定値で初期化されたと見なされます。

次の例は、初期化子の使い方を示しています。

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

このプログラムを出力します。

```
Static variable x = 5
Static variable x = 6
Static variable x = 7
Local variable y = 8
Local variable y = 8
Local variable y = 8
```

静的ローカル変数の初期化子では、スレッド セーフであると例外に対して保護を使用の初期化中にです。 静的ローカル初期化子で例外が発生した場合、静的ローカルは既定値を持つし、を初期化できません。 静的ローカル初期化子

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

ローカル変数、ローカル定数、および静的変数のスコープは、ステートメント、宣言されているをブロックします。 静的変数は、特別な全体のメソッド全体で 1 回に使用されるのみの名前も可能性があります。 たとえば、異なるブロック内にある場合でも、同じ名前の 2 つの静的変数の宣言を指定する有効ではありません。


### <a name="implicit-local-declarations"></a>暗黙のローカル宣言

ローカル宣言ステートメントでは、他のローカル変数宣言することも暗黙的を使用しています。 別のものに対して解決されない名前を使用する簡易名式では、その名前のローカル変数を宣言します。 例:

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

暗黙のローカル宣言は、変数として分類される式を受け入れることができる式のコンテキストでのみ発生します。 このルールの例外をローカル変数は暗黙的に宣言できません関数の呼び出し式、式のインデックス作成、またはメンバー アクセス式の対象となっているときにします。

暗黙的なローカル変数は、それを含むメソッドの先頭で宣言されているとして扱われます。 そのため、常にスコープが、メソッド全体の本文をラムダ式の内部で宣言されている場合でもです。 コード例を次に示します。

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

値を出力`10`します。 暗黙的なローカル変数は、入力`Object`なしの型文字は、変数名に関連付けられました。 それ以外の場合、変数の型は、型の文字の型の場合。 暗黙的なローカル変数では、ローカル変数の型の推論は使用されません。

コンパイル環境または明示的なローカル宣言が指定されている場合`Option Explicit`、すべてのローカル変数を明示的に宣言する必要があります、および暗黙的な変数宣言は許可されていません。

## <a name="with-statement"></a>ステートメントを使用

A`With`ステートメント、式を複数回指定することがなく、式のメンバーに複数の参照を使用できます。

```antlr
WithStatement
    : 'With' Expression StatementTerminator
      Block?
      'End' 'With' StatementTerminator
    ;
```

式は、値として分類する必要があり、ブロックへの入力時にのみ評価されます。 内で、`With`ステートメント ブロック、メンバー アクセス式またはピリオドまたは感嘆符で始まるディクショナリ アクセス式が評価される場合と、`With`式の前にします。 例:

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

分岐することはできません、`With`ブロックの外側でステートメント ブロックから。


## <a name="synclock-statement"></a>SyncLock ステートメント

A`SyncLock`ステートメント、式では、複数の実行スレッドは同時に同じステートメントを実行しないことを確実に同期するステートメントを使用できます。

```antlr
SyncLockStatement
    : 'SyncLock' Expression StatementTerminator
      Block?
      'End' 'SyncLock' StatementTerminator
    ;
```

式は、値として分類する必要があり、ブロックへの入力時にのみ評価されます。 入力するときに、`SyncLock`ブロック、`Shared`メソッド`System.Threading.Monitor.Enter`ブロック式によって返されるオブジェクトの実行スレッドがある排他ロックするまで、指定された式で呼び出されます。 内の式の型を`SyncLock`ステートメントは参照型である必要があります。 例:

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

クラスの特定のインスタンスで上記の例では同期`Test`させる、複数のスレッドの実行が追加または特定のインスタンスを一度に count 変数から減算できます。

`SyncLock`ブロックは暗黙的に含まれる、`Try`ステートメントが`Finally`呼び出しをブロック、`Shared`メソッド`System.Threading.Monitor.Exit`式。 これにより、例外がスローされた場合でも、ロックが解放されます。 その結果に分岐する有効ながない、 `SyncLock` 、ブロックの外側からブロックと`SyncLock`の目的でブロックが単一のステートメントとして扱われます`Resume`と`Resume Next`します。 上記の例では、次のコードと同等です。

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


## <a name="event-statements"></a>Event ステートメント

`RaiseEvent`、 `AddHandler`、および`RemoveHandler`ステートメントは、イベントを発生させるし、動的にイベントを処理します。

```antlr
EventStatement
    : RaiseEventStatement
    | AddHandlerStatement
    | RemoveHandlerStatement
    ;
```

### <a name="raiseevent-statement"></a>RaiseEvent ステートメント

A`RaiseEvent`ステートメントが、特定のイベントが発生したイベント ハンドラーに通知します。

```antlr
RaiseEventStatement
    : 'RaiseEvent' IdentifierOrKeyword
      ( OpenParenthesis ArgumentList? CloseParenthesis )? StatementTerminator
    ;
```

簡易名式、`RaiseEvent`でステートメントがメンバー参照として解釈されます`Me`します。 したがって、`RaiseEvent x`場合と同様に解釈`RaiseEvent Me.x`します。 式の結果は、クラス自体で定義されているイベントのイベント アクセスとして分類する必要があります。基本型で定義されているイベントでは使用できません、`RaiseEvent`ステートメント。

`RaiseEvent`への呼び出しとしてステートメントを処理、`Invoke`の存在する場合は、指定されたパラメーターを使用して、イベントのデリゲート メソッド。 デリゲートの値が場合`Nothing`例外はスローされません。 引数がない場合は、かっこは省略できます。 例:

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

クラスは、`Raiser`は上記と同じです。

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


### <a name="addhandler-and-removehandler-statements"></a>AddHandler と RemoveHandler ステートメント

ほとんどのイベント ハンドラーが自動的にを通じてフックが`WithEvents`変数、必要がありますを動的に追加して、実行時にイベント ハンドラーを削除します。 `AddHandler` `RemoveHandler`ステートメントでは、これを実行します。

```antlr
AddHandlerStatement
    : 'AddHandler' Expression Comma Expression StatementTerminator
    ;

RemoveHandlerStatement
    : 'RemoveHandler' Expression Comma Expression StatementTerminator
    ;
```

各ステートメントは 2 つの引数を受け取ります。 最初の引数は、イベントへのアクセスに分類される式である必要がありますし、2 番目の引数が値として分類される式を指定する必要があります。 2 番目の引数の型は、イベントへのアクセスに関連付けられているデリゲート型である必要があります。 例:

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

指定されたイベント`E,`、ステートメントを呼び出して、関連する`add_E`または`remove_E`メソッドを追加またはイベントのハンドラーとして、デリゲートを削除するインスタンス。 したがって、上記のコードと同等です。

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

代入ステートメントは、式の値を変数に割り当てます。 割り当ての種類をいくつかあります。

```antlr
AssignmentStatement
    : RegularAssignmentStatement
    | CompoundAssignmentStatement
    | MidAssignmentStatement
    ;
```

### <a name="regular-assignment-statements"></a>通常の代入ステートメント

単純な代入ステートメントは、式の結果を変数に格納します。

```antlr
RegularAssignmentStatement
    : Expression Equals Expression StatementTerminator
    ;
```

代入演算子の左側にある式は、中、代入演算子の右側にある式は、値として分類する必要があります、変数またはプロパティ アクセス、として分類する必要があります。 式の型は、変数またはプロパティ アクセスの型に暗黙的に変換可能である必要があります。

割り当てられている変数が参照型の配列要素の場合は、式が配列要素の型と互換性があることを確認するランタイム チェックは実行されます。 次の例では最後の割り当てと、`System.ArrayTypeMismatchException`がスローされるため、インスタンスの`ArrayList`の要素に格納することはできません、`String`配列。

```vb
Dim sa(10) As String
Dim oa As Object() = sa
oa(0) = Nothing         ' This is allowed.
oa(1) = "Hello"         ' This is allowed.
oa(2) = New ArrayList() ' System.ArrayTypeMismatchException is thrown.
```

変数は、代入演算子の左側にある式の場合、代入ステートメントは、変数に値を格納します。 かどうかには、式はプロパティ アクセス、として分類し、代入ステートメントの呼び出しにプロパティへのアクセスを有効に、`Set`値パラメーターの値を持つプロパティのアクセサー代わりに使用します。 例:

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

変数またはプロパティ アクセスの対象では、値の型として型指定されたが、変数として分類されない、コンパイル時エラーが発生します。 例:

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

割り当てのセマンティクスは、変数または割り当てられるプロパティの種類によって異なりますに注意してください。 割り当て先の変数が値型の場合、割り当ては、変数に式の値をコピーします。 割り当て先の変数が参照型の場合、割り当ては、変数に値自体ではなく、参照をコピーします。 変数の型がある場合`Object`割り当てのセマンティクスは値の型が値型または参照型を実行時にかどうかによって決まります。


__注意してください。__ 組み込み型などの`Integer`と`Date`、参照および値の割り当てのセマンティクスは、同じ種類は変更できないためです。 その結果、言語は、自由にボックス化された組み込み型の参照の割り当てを使用して、最適化として。 値の観点から、結果は同じです。

ため、equals 文字 (`=`) される両方の割り当てと等しいかどうかをあいまいさがある単純な代入と、呼び出しステートメントの間など`x = y.ToString()`します。 このような場合は、代入ステートメントは等値演算子に優先します。 つまり、例の式として解釈される`x = (y.ToString())`なく`(x = y).ToString()`します。


### <a name="compound-assignment-statements"></a>複合代入ステートメント

A*複合代入ステートメント*形式の`V op= E`(場所`op`は有効な二項演算子)。

```antlr
CompoundAssignmentStatement
    : Expression CompoundBinaryOperator LineTerminator? Expression StatementTerminator
    ;

CompoundBinaryOperator
    : '^' '=' | '*' '=' | '/' '=' | '\\' '=' | '+' '=' | '-' '='
    | '&' '=' | '<' '<' '=' | '>' '>' '='
    ;
```

代入演算子の左側にある式は、代入演算子の右側にある式は、値として分類する必要があります、変数またはプロパティ アクセスでは、として分類される必要があります。 複合代入ステートメントは、ステートメントと同じ`V = V op E`な相違点は、複合代入演算子の左側にある変数がのみである 1 回評価されます。 次の例では、この違いを示しています。

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

式`a(GetIndex())`、複合代入でそのため、コードを出力すると、だけの単純な代入で 2 回評価されます。

```
Simple assignment
Getting index
Getting index
Compound assignment
Getting index
```


### <a name="mid-assignment-statement"></a>Mid の代入ステートメント

A`Mid`代入ステートメントは、別の文字列に文字列を割り当てます。 割り当ての左側にあるが、関数の呼び出しと同じ構文`Microsoft.VisualBasic.Strings.Mid`します。

```antlr
MidAssignmentStatement
    : 'Mid' '$'? OpenParenthesis Expression Comma Expression
      ( Comma Expression )? CloseParenthesis Equals Expression StatementTerminator
    ;
```

最初の引数が、割り当ての対象となるし、変数またはとの間に暗黙的に変換できる型を持つプロパティ アクセスに分類する必要があります`String`します。 2 番目のパラメーターは、割り当てが、ターゲット文字列の開始し、暗黙的に変換できる型を持つ必要があります値として分類する必要がありますに対応する 1 から始まる開始位置`Integer`します。 省略可能な 3 番目のパラメーターは、右側にある文字の数に、対象の文字列に割り当てる値し、型が暗黙的に変換できる値として分類する必要があります`Integer`します。 右側にあるソース文字列は、型が暗黙的に変換できる値として分類する必要があります`String`します。 右側にあるは、指定した場合、長さのパラメーターに切り捨てられ、開始位置から、左側にある文字列内の文字を置き換えます。 右側にある文字列に 3 番目のパラメーターよりも少ない文字が含まれている場合は、右側にある文字列の文字だけがコピーされます。

次の例では、表示`ab123fg`:

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

__注意してください。__ `Mid` 予約語ではありません。


## <a name="invocation-statements"></a>呼び出しステートメント

呼び出しステートメントが省略可能なキーワードに続くメソッドを呼び出す`Call`します。 呼び出しステートメントは、次に示すいくつかの違いを関数の呼び出し式と同じ方法で処理されます。 Invocation 式は、値、または void に分類する必要があります。 Invocation 式の評価結果の任意の値は破棄されます。

場合、`Call`キーワードを省略すると、その後、invocation 式は、識別子またはキーワードを使用またはを起動する必要があります`.`内で、`With`ブロックします。 したがって、たとえば、"`Call 1.ToString()`"有効なステートメントですが、"`1.ToString()`"はありません。 (いる式コンテキストでは、invocation 式も必要がありますで始まらない識別子に注意してください。 たとえば、"`Dim x = 1.ToString()`"有効なステートメントです)。

呼び出しステートメントおよび呼び出し式にはもう 1 つの違いがある: かどうか、呼び出しステートメントには、引数リストが含まれていますし、呼び出しの引数リストとしてこの常に取得されます。 次の例では、違いを示します。

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

条件付きステートメントを使用すると、実行時に評価される式に基づいて、ステートメントの条件付きの実行。

```antlr
ConditionalStatement
    : IfStatement
    | SelectStatement
    ;
```

### <a name="ifthenelse-statements"></a>もし。。。そうしたら。。。Else ステートメント

`If...Then...Else`ステートメントは、基本的な条件付きステートメント。

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

内の各式、`If...Then...Else`ステートメントは、「」セクションのブール式をする必要があります[ブール式](expressions.md#boolean-expressions)します。 (注: ブール型を持つ式は必要ありません)。 場合内の式、`If`ステートメントが true の場合、ステートメントがで囲まれた、`If`ブロックが実行されます。 式が false の場合の`ElseIf`式が評価されます。 1 つの場合の`ElseIf`式が true に評価される、対応するブロックが実行されます。 式を true として評価されていない場合は、`Else`ブロック、`Else`ブロックが実行されます。 実行がの末尾に移り、ブロックの実行が終了すると、`If...Then...Else`ステートメント。

行のバージョン、`If`ステートメントの場合に実行されるステートメントの 1 つのセットには、`If`式が`True`と省略可能なセット式が場合に実行されるステートメントの`False`します。 例:

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

場合は、行バージョン ステートメント バインドよりも厳密に小さい":"、およびその`Else`構文的に最も近いの前にバインドする`If`構文で許可されています。 たとえば、次の 2 つのバージョンは同等です。

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

行では、ラベルの宣言ステートメント以外のすべてのステートメントが許可されて`If`ブロック ステートメントを含むステートメント。 ただし、それらが LineTerminators として使用 StatementTerminators 複数行ラムダ式の中で除きます。 例:

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

A`Select Case`ステートメント、式の値に基づいたステートメントを実行します。

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

式は、値として分類する必要があります。 ときに、`Select Case`ステートメントが実行される、`Select`式が最初に評価、`Case`ステートメント テキスト宣言の順序で評価されます。 最初の`Case`ステートメントに評価される`True`のブロックが実行されます。 ない場合は`Case`ステートメントの評価が`True`があると、`Case Else`ステートメントでは、そのブロックが実行されます。 実行がの末尾に移り、ブロックの実行が完了すると、`Select`ステートメント。

実行、`Case`ブロックは次の switch セクションに「フォールスルー」は許可されていません。 これにより、他の言語で発生するバグの一般的なクラスと、`Case`ステートメントの終了を誤って省略するとします。 この動作を次の例に示します。

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

コードを出力します。

```
x = 10
```

`Case 10`と`Case 20 - 10`同じの値の選択`Case 10`実行後に続くため`Case 20 - 10`テキスト。 ときに、[次へ]`Case`が後に達すると、実行が続行、`Select`ステートメント。

A`Case`句は、2 つの形式をかかる場合があります。 1 つの形式は、省略可能な`Is`キーワード、比較演算子、および式。 型に式を変換、`Select`式; 式がの型に暗黙的に変換できるかどうか、`Select`式、コンパイル時エラーが発生します。 場合、`Select`式が*E*、比較演算子が*Op*、および`Case`式が*E1*、として大文字と小文字を評価*EOP E1*します。 演算子は、2 つの式の型に対して有効である必要があります。それ以外の場合、コンパイル時エラーが発生します。

他の形式は、必要に応じて後に、キーワード、式`To`と 2 番目の式。 両方の式の型に変換されます、`Select`式; いずれかの式がの型に暗黙的に変換できるかどうか、`Select`式、コンパイル時エラーが発生します。 場合、`Select`式が`E`、最初の`Case`式が`E1`、2 番目`Case`式が`E2`、`Case`が評価されるいずれか`E = E1`(場合ありません`E2`を指定) または`(E >= E1) And (E <= E2)`します。 演算子は、2 つの式の型に対して有効である必要があります。それ以外の場合、コンパイル時エラーが発生します。


## <a name="loop-statements"></a>Loop ステートメント

Loop ステートメントは、本体で、ステートメントの実行を繰り返すを許可します。

```antlr
LoopStatement
    : WhileStatement
    | DoLoopStatement
    | ForStatement
    | ForEachStatement
    ;
```

ループの本体が入力されるたびに、変数の前の値に初期化され、その本体で宣言されたすべてのローカル変数の新しいコピーが行われました。 ループ本体の内部変数への参照では、最近作成されたコピーを使用します。 このコード例に示します。

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

コードでは、出力が生成されます。

```
31    32    33
```

ループの本体が実行されたときに、変数のどちらのコピーが現在を使用します。 たとえば、ステートメント`Dim y = x`の最新のコピーを指す`y`の元のコピーと`x`。 変数のどちらのコピーが作成された時点の最新記憶ラムダが作成されるとします。 そのため、各ラムダが同じ共有のコピーを使用`x`の異なるコピーが`y`します。 プログラムの最後に、ラムダ、実行時にその共有のコピー`x`最終値 3 にようになりましたが、これらはすべてを参照してください。

ラムダまたは LINQ 式がない場合、しないループに入った新しいコピーが行われたことを認識することに注意してください。 実際には、この場合、コピーを作成するコンパイラの最適化を回避できます。 正式にはないことに注意すぎる`GoTo`ラムダまたは LINQ 式を含むループにします。


### <a name="whileend-while-and-doloop-statements"></a>しばらくしています.終了中に、操作を実行しています.Loop ステートメント

A`While`または`Do`ブール式に基づくステートメントのループをループします。

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

A`While`ループ ステートメントのループを true に評価されるブール式であれば、`Do`ループ ステートメントより複雑な条件を含めることができます。 式は、後に配置することがあります、`Do`キーワード以降、`Loop`キーワードが両方の後です。 セクションに従って、ブール式が評価される[ブール式](expressions.md#boolean-expressions)します。 (注: ブール型を持つ式は必要ありません)。 式をまったく; 指定せずに有効です。その場合は、ループは終了しません。 式が後に配置されている場合`Do`、各イテレーションでループ ブロックが実行される前に評価されます。 式が後に配置されている場合`Loop`、各イテレーションでループ ブロックが実行された後に評価されます。 後の式を配置する`Loop`より後に配置の 1 つ以上ループが生成されますので`Do`します。 次の例では、この動作を示します。

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

コードでは、出力が生成されます。

```
Second Loop
```

最初のループの場合は、ループが実行される前に、条件が評価されます。 2 つ目のループの場合、条件は、ループの実行後に実行されます。 条件式は、いずれかが付く必要があります、`While`キーワードまたは`Until`キーワード。 前者に停止条件の評価が false の条件が true に評価された場合の後者の場合のループをします。

__注意してください。__ `Until` 予約語ではありません。


### <a name="fornext-statements"></a>.次のステートメント

A`For...Next`ステートメントのループの境界のセットに基づいています。 A`For`ステートメントは、ループ コントロール変数を下限の境界の式、上限式、および省略可能な手順の値式を指定します。 ループ コントロール変数は省略可能な識別子を使用するか後に指定された`As`句、または式。

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

これを特定、新しいローカル変数のいずれかループ制御変数によって、次の規則に従って参照`For...Next`ステートメント、または既存の変数または式にします。

* ループ コントロール変数が識別子の場合、`As`句で指定された型の場合は、新しいローカル変数を定義する識別子、`As`全体をスコープ句`For`ループします。

* ループ コントロール変数がない識別子、`As`句では、その識別子は、まずを使用して解決単純な名前解決ルール (セクションを参照して[名前の単純式](expressions.md#simple-name-expressions))、後、例外をこの出現識別子自体は、暗黙的なローカル変数を作成する (セクション[暗黙のローカル宣言](statements.md#implicit-local-declarations))。

  * この解決が成功し、結果は変数として分類、ループ コントロール変数はその既存の変数です。

  * 解決に失敗した場合、または解決が成功し、結果は、型として分類されます。
    * ローカル変数の型の推定を使用している場合、識別子は、バインドから推論する型を持つ新しいローカル変数を定義します全体をスコープ式のステップと`For`ループです。
    * ローカル変数の型の推定が使用されていないかどうかは暗黙のローカル宣言は、スコープのすべてのメソッドは、ローカルの暗黙的な変数を作成 (セクション[暗黙のローカル宣言](statements.md#implicit-local-declarations))、およびループ制御変数この既存の変数では; を参照します。
    * ローカル変数の型の推論も暗黙のローカル宣言を使用している場合は、エラーです。

  * 解像度は、型でも変数として分類されるものに成功すると、エラーになります。

* ループ コントロール変数が式の場合は、式を変数として分類する必要があります。

もう 1 つの外側でループ コントロール変数は使用できません`For...Next`ステートメント。 ループ コントロール変数の型を`For`ステートメント、イテレーションの種類を決定しのいずれかを指定する必要があります。

* `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`
* 列挙型
* `Object`
* 型`T`、次の演算子を持つ場所`B`ブール式で使用できる型には。

```vb
Public Shared Operator >= (op1 As T, op2 As T) As B
Public Shared Operator <= (op1 As T, op2 As T) As B
Public Shared Operator - (op1 As T, op2 As T) As T
Public Shared Operator + (op1 As T, op2 As T) As T
```

バインドされていると、ステップ式は、ループ コントロール変数の型に暗黙的に変換できる必要があり、値として分類する必要があります。 コンパイル時に、下限の境界、上限、およびステップ式の型の間で最も広い種類を選択して、ループ コントロール変数の型が推論されます。 2 つの種類の間に拡大変換がない場合は、コンパイル時エラーが発生します。

実行時に、ループ コントロール変数の型が場合`Object`イテレーションの型を推論し、2 つの例外は、コンパイル時と同じです。 最初の場合、バインドされたステップ式が整数型のすべてが最も幅の広い型を持つありませんし、3 種類すべてを包含している最も幅の広い型が推論されます。 2 番目にループ コントロール変数の型が推定された場合、 `String`、`Double`代わりに推論されます。 式は、ループ コントロール型に変換できない場合、実行時にループ制御の種類を決定できない場合、または、`System.InvalidCastException`が発生します。 ループの先頭にループ コントロールの種類を選択すると、同じ型がループ コントロール変数の値に加えられた変更に関係なく、イテレーション全体で使用されます。

A`For`ステートメントは、対応するを閉じる必要があります`Next`ステートメント。 A`Next`変数を指定しないステートメントに一致する最も内側のオープン`For`ステートメントでは中、`Next`ステートメントの 1 つまたは複数のループ コントロール変数は、左から右に、対応、`For`ループの各変数に一致します。 変数と一致する場合、`For`最も内側のループがその時点ではないループでは、コンパイル時エラーになります。

ループの先頭には、3 つの式テキストの順序で評価して下限の境界の式は、ループ コントロール変数に割り当てられています。 リテラルは暗黙的に増分値を省略すると、 `1`, ループ コントロール変数の型に変換されます。 3 つの式は、ループの先頭にのみ評価されます。

各ループの先頭が終点を超えるステップ式が負の場合、ステップ式が正の値、または終了点よりも小さい場合に、コントロール変数と比較されます。 その場合、`For`ループを終了します。 それ以外の場合、ループのブロックを実行します。 ループ コントロール変数がプリミティブ型ではない場合、比較演算子は、かどうかによって決まります式`step >= step - step`が true または false。 `Next`ステートメントでは、ステップ値がコントロール変数に追加し、実行、ループの先頭に戻ります。

ループ コントロール変数の新しいコピーは*いない*ループ ブロックの各反復処理で作成します。 この点で、`For`ステートメントとは異なります`For Each`(セクション[For次のステートメント](statements.md#for-eachnext-statements))。

分岐することはできません、`For`ループの外側のループ処理します。


### <a name="for-eachnext-statements"></a>各.次のステートメント

A`For Each...Next`ステートメントのループ式内の要素に基づいています。 A`For Each`ステートメントは、ループ コントロール変数と列挙子の式を指定します。 ループ コントロール変数は省略可能な識別子を使用するか後に指定された`As`句、または式。

```antlr
ForEachStatement
    : 'For' 'Each' LoopControlVariable 'In' LineTerminator? Expression StatementTerminator
      Block?
      ( 'Next' NextExpressionList? StatementTerminator )?
    ;
```

次のと同じ規則`For...Next`ステートメント (セクション[の..。次のステートメント](statements.md#fornext-statements))、ループ コントロール変数を指す特定新しいローカル変数にこのごとにしています.次のステートメント、または既存の変数または式にします。

列挙子式は、値として分類する必要があり、その型はコレクション型である必要がありますまたは`Object`します。 列挙子の式の型が場合`Object`、すべての処理が実行時まで延期されます。 それ以外の場合、変換は、コレクションの要素型、ループ コントロール変数の型に存在する必要があります。

もう 1 つの外側でループ コントロール変数は使用できません`For Each`ステートメント。 A`For Each`ステートメントは、対応するを閉じる必要があります`Next`ステートメント。 A`Next`ループ コントロール変数を指定しないステートメントに一致する最も内側のオープン`For Each`します。 A`Next`ステートメントの 1 つまたは複数のループ コントロール変数は、左から右に、対応、`For Each`を同じループ コントロール変数を持つループします。 変数と一致する場合、`For Each`最も内側のループがその時点ではないループでは、コンパイル時エラーが発生します。

型`C`と呼ばれます、*コレクション型*場合は、1 つの。

* 次のすべてが該当します。
  * `C` 共有のアクセス可能なインスタンスまたはシグネチャを持つ拡張メソッドが含まれる`GetEnumerator()`型を返す`E`します。
  * `E` 共有のアクセス可能なインスタンスまたはシグネチャを持つ拡張メソッドが含まれる`MoveNext()`と戻り値の型`Boolean`します。
  * `E` アクセス可能なインスタンスまたは名前付き共有のプロパティが含まれる`Current`getter を持ちます。 このプロパティの型は、コレクション型の要素型です。

* インターフェイスを実装する`System.Collections.Generic.IEnumerable(Of T)`、その場合、コレクションの要素の型と見なされます`T`します。

* インターフェイスを実装する`System.Collections.IEnumerable`、その場合、コレクションの要素の型と見なされます`Object`します。

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

ループが開始する前に、列挙子の式が評価されます。 式の型は、設計パターンを満たさない場合、式にキャストされます`System.Collections.IEnumerable`または`System.Collections.Generic.IEnumerable(Of T)`します。 式の型は、ジェネリック インターフェイスを実装する場合は、ジェネリック インターフェイスは、コンパイル時に優先が実行時に、非ジェネリック インターフェイスをお勧めします。 式の型は、複数回ジェネリック インターフェイスを実装する場合、ステートメントがあいまいと見なされ、コンパイル時エラーが発生します。

__注意してください。__ インターフェイス メソッドに、すべての呼び出しで型パラメーターが含まれることを意味ジェネリック インターフェイスを選択するため、遅延バインドされた場合は、非ジェネリック インターフェイスが優先されます。 このようなすべての呼び出しにする必要がありますので、一致する実行時引数の型を把握することはできません、遅延バインディング呼び出しを使用します。 これは、コンパイル時の呼び出しを使用して、非ジェネリック インターフェイスを呼び出すことがあるために、非ジェネリック インターフェイスを呼び出すよりも低速になります。

`GetEnumerator` 結果の値と戻り値で呼び出される関数の値を一時的に格納されます。 その後、各反復処理の先頭に`MoveNext`が一時的なで呼び出されます。 返された場合`False`ループを終了します。 それ以外の場合、ループの各反復処理には、次のように実行されます。

1. ループ コントロール変数には、(既存の 1 つではなく) 新しいローカル変数が識別される、このローカル変数の新しいコピーが作成されます。 現在のイテレーションがループ ブロック内のすべての参照はこのコピーを参照してください。
2. `Current`プロパティを取得すると、(かどうか、変換が暗黙的または明示的な) に関係なくループ コントロール変数の型に強制変換、ループ コントロール変数に割り当てられているとします。
3. ループのブロックを実行します。

__注意してください。__ Version 10.0 および 11.0、言語の間における動作のわずかな変更があります。 新しいイテレーション変数が 11.0、前に*いない*ループの反復ごとに作成します。 この違いは、繰り返し変数がラムダ式、または、ループの後に呼び出され、LINQ 式によってキャプチャされた場合にのみ、観測可能なオブジェクトを示します。

```vb
Dim lambdas As New List(Of Action)
For Each x In {1,2,3}
   lambdas.Add(Sub() Console.WriteLine(x)
Next
lambdas(0).Invoke()
lambdas(1).Invoke()
lambdas(2).Invoke()
```

Visual Basic 10.0、最高の コンパイル時に警告を生成および「3」印刷この 3 回です。 1 つの変数"x"は、ループのすべてのイテレーションで共有のみが、すべての 3 つのラムダ キャプチャ同じ"x"と、ラムダが実行された時間にし、保持数 3 ためにでした。 Visual Basic の 11.0 の時点では、「1, 2, 3」が出力されます。 各ラムダは、別の変数"x"をキャプチャするためです。


__注意してください。__ イテレーションの現在の要素は、ステートメントの変換演算子を導入する便利な場所がないために、変換は明示的な場合でも、ループ コントロール変数の型に変換されます。 ここで廃止された型を使用する場合に特に問題になるこうして`System.Collections.ArrayList`その要素型であるため、`Object`します。 これが必要なキャストで優れた多くループは、理想的と考えたものでした。 皮肉にも、ジェネリックは、厳密に型指定されたコレクションの作成を有効になって`System.Collections.Generic.List(Of T)`を加えた可能性のあるこの設計上のポイントを考え直すことが、互換性のために、これは変更できませんようになりました。


ときに、`Next`ステートメントに達すると、実行をループの先頭に戻ります。 後に変数が指定されている場合、`Next`キーワード、する必要がありますの後の最初の変数と同じ、`For Each`します。 次に例を示します。

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

これは、次のコードに相当します。

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

場合、型`E`列挙子の実装`System.IDisposable`、呼び出すことによって、ループの終了時に、列挙子が破棄された後、`Dispose`メソッド。 これにより、列挙子によって保持されているリソースが解放されます。 場合、メソッドを含む、`For Each`ステートメントは、非構造化エラー処理を使用していない、`For Each`にステートメントがラップされて、`Try`ステートメントを`Dispose`呼び出されるメソッド、`Finally`クリーンアップを確実に。

__注意してください。__ `System.Array`型がコレクション型と型がから派生するすべての配列から`System.Array`で、配列型の式が許可されている、`For Each`ステートメント。 1 次元配列の場合、`For Each`ステートメントは、インデックス 0 から始まるインデックスの長さ - 1 で終わるインデックスの昇順に配列の要素を列挙します。 多次元配列は、まず右端の次元のインデックスを増加します。

たとえば、次のコードを出力`1 2 3 4`:

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

分岐することはできません、`For Each`からブロックの外側のステートメント ブロックです。


## <a name="exception-handling-statements"></a>例外処理ステートメント

Visual Basic では、構造化例外処理と非構造化例外処理をサポートします。 メソッドでは、例外処理の 1 つだけのスタイルを使用する可能性がありますが、`Error`構造化例外処理でステートメントを使用することがあります。 メソッドは、例外処理の両方のスタイルを使用している場合は、コンパイル時エラーが発生します。

```antlr
ErrorHandlingStatement
    : StructuredErrorStatement
    | UnstructuredErrorStatement
    ;
```

### <a name="structured-exception-handling-statements"></a>構造化例外処理ステートメント

構造化例外処理は、特定の例外の処理を明示的なブロックを宣言することでエラーの処理の方法です。 構造化例外処理を行う、`Try`ステートメント。

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


例:

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

A`Try`ブロックの 3 種類のステートメントから成る: try ブロック、catch ブロックおよびブロックの最後にします。 A *try ブロック*ステートメント ブロックに実行されるステートメントを含むです。 A *catch ブロック*例外処理ステートメント ブロックです。 A *finally ブロック*ステートメント ブロックが実行されるステートメントを含む、`Try`例外が発生し、処理されたかどうかに関係なく、ステートメントが終了しました。 A`Try`のみ含めることができる 1 つの try ブロックと 1 つ最後に、ステートメントを少なくとも 1 つの catch ブロックを含めることがまたは、finally ブロックする必要があります、ブロックします。 Try ブロックに以外から、同じステートメント内の catch ブロック内で実行を明示的に転送することはしません。


#### <a name="finally-blocks"></a>Finally ブロック

A`Finally`ブロックは常に実行がの一部を離れるときに実行、`Try`ステートメント。 実行する明示的なアクションは必要ありません、`Finally`ブロックは実行から離したときに、`Try`ステートメントでは、システムが自動的に実行、`Finally`をブロックし、その目的の宛先に実行を転送します。 `Finally`実行のままに関係なくブロックが実行される、`Try`ステートメント: の終わりまで、`Try`の終わりまでのブロックを`Catch`を通じて、ブロック、`Exit Try`ステートメントを使用して、 `GoTo`ステートメント、またはスローされた例外を処理しないことで。

なお、 `Await` 、非同期メソッド内の式と`Yield`反復子メソッドでステートメント非同期または反復子メソッド インスタンスの中断およびいくつかその他のメソッドのインスタンスで再開する制御フローが発生することができます。 ただし、この実行の中断だけでは、それぞれ async メソッドまたは iterator メソッド インスタンスを終了するには関与せず、ため発生しません`Finally`実行をブロックします。

実行を明示的に転送することはできません、 `Finally` ; ブロックも転送の実行には無効です、`Finally`例外を除くブロックします。

```antlr
FinallyStatement
    : 'Finally' StatementTerminator
      Block?
    ;
```

#### <a name="catch-blocks"></a>catch ブロック

処理中に例外が発生した場合、`Try`ブロックする場合に、各`Catch`ステートメントが例外を処理するかどうかのテキストの順序で調べられます。

```antlr
CatchStatement
    : 'Catch' ( Identifier ( 'As' NonArrayTypeName )? )?
      ( 'When' BooleanExpression )? StatementTerminator
      Block?
    ;
```

指定された識別子を`Catch`句がスローされた例外を表します。 識別子が含まれている場合、`As`句では、その識別子は内で宣言すると見なされます、`Catch`ブロックのローカル宣言領域です。 それ以外の場合、識別子が含まれているブロックで定義されたローカル変数 (静的変数ではなく) 場合があります。

A`Catch`識別子のない句をキャッチ オールから派生した例外`System.Exception`します。 A`Catch`識別子を持つ句だけと同じ、または識別子の型から派生した型を持つ例外をキャッチします。 型でなければなりません`System.Exception`から派生した型または`System.Exception`します。 派生した例外がキャッチされたときに`System.Exception`、例外オブジェクトへの参照が関数によって返されるオブジェクトに格納されている`Microsoft.VisualBasic.Information.Err`します。

A`Catch`句、`When`式として評価される場合は、句に例外はキャッチのみ`True`; 式の型はセクションに従ってブール式である必要があります[ブール式](expressions.md#boolean-expressions)します。 A`When`句は、例外の種類を確認後にだけ適用し、この例に示すように、次の式が例外を表す識別子を参照してください可能性があります。

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

この例が出力されます。

```
Third handler
```

場合、`Catch`句は、例外を処理、転送の実行、`Catch`ブロックします。 最後に、`Catch`に続く最初のステートメントの実行の転送をブロックします。、`Try`ステートメント。 `Try`ステートメント スローされた例外を処理しないが、`Catch`ブロックします。 ない場合は`Catch`句は、システムによって決まります場所への転送の実行、例外を処理します。

実行を明示的に転送することはできません、`Catch`ブロックします。

句が通常のフィルターでは、例外がスローする前に評価されます。 たとえば、次のコードが印刷されます「フィルター、最後に、Catch」。

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

 ただし、非同期および反復子メソッドは、ブロックの外側のすべてのフィルターの前に実行するのにはその内部で、最後にすべて。 たとえば、上記のコードがある場合`Async Sub Foo()`出力になりますし、「最後に、フィルター処理、キャッチ」。


#### <a name="throw-statement"></a>Throw ステートメント

`Throw`ステートメントから派生した型のインスタンスによって表される例外を発生させる`System.Exception`します。

```antlr
ThrowStatement
    : 'Throw' Expression? StatementTerminator
    ;
```

式が値として分類されないかから派生した型ではなく`System.Exception`コンパイル時エラーが発生します。 実行時に、null 値に式が評価された場合、`System.NullReferenceException`代わりに例外がスローされます。

A`Throw`ステートメントの catch ブロック内の式を省略できます、`Try`の介在がない限り、ステートメントが最後にブロックします。 その場合は、ステートメントでは、catch ブロック内で現在処理中の例外が再スローします。 例:

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

非構造化例外処理は、例外が発生したときに分岐するステートメントを示すエラーを処理するメソッドです。 3 つのステートメントを使用して非構造化例外処理を実装:`Error`ステートメントでは、`On Error`ステートメント、および`Resume`ステートメント。

```antlr
UnstructuredErrorStatement
    : ErrorStatement
    | OnErrorStatement
    | ResumeStatement
    ;
```

例:

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

メソッドは、非構造化例外処理を使用する場合は、すべての例外をキャッチする場合、メソッド全体の 1 つの構造化例外ハンドラーが確立されます。 (をコンストラクター内でこのハンドラーは拡張されませんへの呼び出しへの呼び出しの上に注意してください`New`コンストラクターの先頭にします。)。メソッドからの最新の例外ハンドラーの場所と追跡がスローされた、最新の例外。 メソッドへのエントリを例外ハンドラーの場所と、例外が両方設定`Nothing`します。 例外オブジェクトへの参照が関数によって返されるオブジェクトに格納されている非構造化例外処理を使用するメソッドで例外がスローされると、`Microsoft.VisualBasic.Information.Err`します。

非構造化エラー処理ステートメントは、反復子または非同期のメソッドでは使用できません。


#### <a name="error-statement"></a>Error ステートメント

`Error`ステートメントがスローされます、 `System.Exception` Visual Basic 6 の例外数を含む例外。 値として式を分類する必要があり、その型に暗黙的に変換できる必要があります`Integer`します。

```antlr
ErrorStatement
    : 'Error' Expression StatementTerminator
    ;
```

#### <a name="on-error-statement"></a>On Error ステートメント

`On Error`ステートメントは、最新の状態の例外処理を変更します。

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

4 つの方法のいずれかで使用することがあります。

* `On Error GoTo -1` 最新の例外をリセット`Nothing`します。

* `On Error GoTo 0` 最新の例外ハンドラーの場所のリセット`Nothing`します。

* `On Error GoTo LabelName` 最新の例外ハンドラーの場所としては、ラベルを設定します。 このステートメントは、ラムダ式、またはクエリ式を含むメソッドでは使用できません。

* `On Error Resume Next` 確立、`Resume Next`最新の例外ハンドラーの場所として動作します。


#### <a name="resume-statement"></a>Resume ステートメント

A`Resume`ステートメントは、最新の例外の原因となったステートメントを実行を戻します。

```antlr
ResumeStatement
    : 'Resume' ResumeClause? StatementTerminator
    ;

ResumeClause
    : 'Next'
    | LabelName
    ;
```

場合、`Next`修飾子を指定すると、実行を最新の例外の原因となったステートメントの後に実行されたステートメントに戻ります。 ラベル名が指定されている場合、ラベルに実行を返します。

`SyncLock`ステートメントが含まれている暗黙の型の構造化エラー処理ブロック、`Resume`と`Resume Next`で発生する例外の特殊な動作がある`SyncLock`ステートメント。 `Resume` 先頭に、実行を返す、`SyncLock`ステートメントでは、中に`Resume Next`実行を次のステートメントに返す、`SyncLock`ステートメント。 次に例を示します。

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

次の結果を出力します。

```
Before exception
Before exception
After SyncLock
```

最初のループ、`SyncLock`ステートメントでは、`Resume`の先頭に、実行を返す、`SyncLock`ステートメント。 2 回目、`SyncLock`ステートメントでは、`Resume Next`の最後に実行を戻します、`SyncLock`ステートメント。 `Resume` `Resume Next`内では使用できません、`SyncLock`ステートメント。

どの場合も、ときに、`Resume`ステートメントが実行され、最新の例外に設定されている`Nothing`します。 場合、`Resume`ステートメントを実行する最新の例外ではないと、ステートメントが発生、 `System.Exception` Visual Basic のエラー番号を含む例外`20`(エラーを発生させずに再開する)。


## <a name="branch-statements"></a>分岐ステートメント

分岐ステートメントは、メソッドの実行の流れを変更します。 これには 6 つの分岐ステートメントがあります。

1. A`GoTo`ステートメントにより、メソッドで指定されたラベルに実行します。 許可されていない`GoTo`に、 `Try`、 `Using`、 `SyncLock`、 `With`、`For`または`For Each`ブロックも、そのブロックのローカル変数がラムダ式、または LINQ 式でキャプチャされた場合、ループ ブロックにします。
2. `Exit`ステートメントは、指定した種類のコンテナーのブロックのステートメントの終了後に次のステートメントに実行を転送します。 かどうか、ブロックはメソッドのブロック、制御フローは、セクションで説明したように、メソッドを終了[制御フロー](statements.md#control-flow)します。 場合、`Exit`ステートメントがコンパイル時エラーが発生したステートメントでは、指定されたブロックの種類に含まれていません。
3. A`Continue`ステートメントは、指定した種類のコンテナーのブロック ループ ステートメントの最後に実行を転送します。 場合、`Continue`ステートメントがコンパイル時エラーが発生したステートメントでは、指定されたブロックの種類に含まれていません。
4. A`Stop`ステートメントにより、デバッガーの例外を発生します。
5. `End`ステートメントは、プログラムを終了します。 ファイナライザーは、シャット ダウンする前に実行されますが、現在の実行の finally ブロック`Try`ステートメントは実行されません。 このステートメントを使用して、(たとえば、Dll など) 実行可能ファイルではないプログラムのことはできません。
6. A`Return`式ステートメントは、`Exit Sub`または`Exit Function`ステートメント。 A`Return`式を使用したステートメントが関数の場合は、通常のメソッドでのみ使用できますか、非同期メソッドで戻り値の型を持つ関数である`Task(Of T)`一部`T`します。 暗黙的に変換される値として式を分類する必要があります、*関数の戻り変数*(通常のメソッド) の場合や、*タスク戻り変数*(非同期メソッド) の場合. その動作は、その式を評価する戻り値の変数に格納し、暗黙的な実行`Exit Function`ステートメント。

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

2 つのステートメントは配列の操作を簡素化:`ReDim`ステートメントと`Erase`ステートメント。

```antlr
ArrayHandlingStatement
    : RedimStatement
    | EraseStatement
    ;
```

### <a name="redim-statement"></a>ReDim ステートメント

A`ReDim`ステートメントは、新しい配列をインスタンス化します。

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

ステートメントの各句を変数またはプロパティ アクセスの型が配列型に分類する必要がありますまたは`Object`配列の境界のリストが続くとします。 境界の数は、変数の型と一致する必要があります。境界内の任意の数が許可されている`Object`します。 実行時に、配列は各式は、指定した境界内で右に左からのインスタンス化し、変数またはプロパティに割り当てられます。 変数の型が場合`Object`、ディメンションの数が指定すると、ディメンションの数、配列要素の型が`Object`します。 実行時にディメンションの指定した数値の変数またはプロパティと互換性がない場合は、コンパイル時エラーが発生します。 例:

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

場合、`Preserve`キーワードを指定し、式は、値として分類にもする必要があります、右端のものを除く各ディメンションの新しいサイズは、既存の配列のサイズと同じである必要があります。 既存の配列内の値は、新しい配列にコピーされます。 新しい配列が小さい場合は、既存の値は破棄されます。新しい配列が大きい場合は、余分な要素は、配列の要素型の既定値に初期化されます。 次に例を示します。

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

```
3, 0
```

既存の配列の参照では、null 値を実行時に、エラーは表示されません。 ディメンションのサイズが変更された場合は、最も右にあるディメンション以外、`System.ArrayTypeMismatchException`がスローされます。

__注意してください。__ `Preserve` 予約語ではありません。


### <a name="erase-statement"></a>Erase ステートメント

`Erase`ステートメントの各配列変数またはステートメントで指定したプロパティ設定`Nothing`します。 ステートメント内の各式は、型が配列型の変数またはプロパティ アクセスとして分類する必要がありますまたは`Object`します。 例:

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

型のインスタンスは、コレクションが実行され、インスタンスへのライブ参照が見つからないときに自動的に、ガベージ コレクターによって解放されます。 型は、特に役に立つと不足しているリソース (データベース接続、ファイル ハンドルなど) で保持している場合が、次のガベージ コレクションで使用できなくなった型の特定のインスタンスをクリーンアップするまで待機することが望ましい場合がありますできません。 コレクションの前にリソースを解放する最も簡単な方法を提供する型を実装可能性があります、`System.IDisposable`インターフェイス。 型を公開、`Dispose`メソッドをすぐに、そのために解放するためのリソースの強制的に呼び出すことができます。

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

`Using`ステートメントは、リソースの取得、一連のステートメントを実行して、リソースを破棄し、プロセスを自動化します。 ステートメントには、2 つの形式を取りますリソースがローカル変数、ステートメントの一部として宣言されており、通常のローカル変数宣言ステートメントとして扱われますを 1 つは、。他のリソースは、式の結果です。

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

かどうか、リソースがローカル変数宣言ステートメントは、ローカル変数宣言の型に暗黙的に変換できる型でなければなりません`System.IDisposable`します。 宣言されたローカル変数は読み取り専用のスコープに、`Using`ステートメントをブロックし、初期化子を含める必要があります。 リソース式の結果であるかどうか、式の値として分類する必要がありますに暗黙的に変換できる型でなければなりません`System.IDisposable`します。 式は、ステートメントの先頭に 1 回だけ評価されます。

`Using`ブロックは暗黙的に含まれる、 `Try` finally ブロック ステートメントでメソッドを呼び出す`IDisposable.Dispose`リソース。 これにより、例外がスローされた場合でも、リソースが破棄されます。 その結果に分岐する有効ながない、 `Using` 、ブロックの外側からブロックと`Using`の目的でブロックが単一のステートメントとして扱われます`Resume`と`Resume Next`します。 リソースの場合`Nothing`、なしの呼び出しを`Dispose`されます。 したがって、次の例を示します。

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

A`Using`をローカル変数宣言ステートメントを持つステートメントは入れ子になっているのと同じですが、時に複数のリソースを取得する可能性があります`Using`ステートメント。  たとえば、`Using`形式のステートメント。

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

Await ステートメントが await 演算子の式と同じ構文 (セクション[Await 演算子](expressions.md#await-operator)) も、await 式を許可するメソッドにのみ使用できますが、await 演算子の式の動作と同じです。

ただし、値または void として分類される可能性があります。 Await 演算子の式の評価結果の任意の値は破棄されます。

```antlr
AwaitStatement
    : AwaitOperatorExpression StatementTerminator
    ;
```

## <a name="yield-statement"></a>Yield ステートメント

Yield ステートメントは、セクションで説明されている反復子メソッドに関連する[反復子メソッド](statements.md#iterator-methods)します。

```antlr
YieldStatement
    : 'Yield' Expression StatementTerminator
    ;
```

`Yield` 予約語は、すぐ外側のメソッドまたはラムダ式が表示される場合、`Iterator`修飾子は、場合に、`Yield`を後に表示`Iterator`修飾子; は予約されている他の場所。 ないプリプロセッサ ディレクティブ内に確保できます。 Yield ステートメントは予約語であるメソッドまたはラムダ式の本体でのみ使用できます。 すぐ外側のメソッドまたはラムダ内で yield ステートメントは内部の発生しません、`Catch`または`Finally`ブロック、または内部の本文を`SyncLock`ステートメント。

Yield ステートメントは、1 つの式の値として分類する必要がありますの型がの型に暗黙的に変換できる、*反復子の現在の変数*(セクション[反復子メソッド](statements.md#iterator-methods)) の外側の反復子メソッド。

制御フローに達するとでのみ、`Yield`ステートメントと、`MoveNext`反復子オブジェクトでメソッドが呼び出されます。 (これは、反復子メソッドのインスタンスしかためには、そのステートメントを実行するため、`MoveNext`または`Dispose`; 反復子オブジェクトで呼び出されるメソッドと`Dispose`メソッドでのコードを実行してしか`Finally`ブロック、 `Yield`は許可されていません)。

ときに、`Yield`ステートメントが実行されると、その式が評価されに格納されている、*反復子の現在の変数*反復子メソッドのインスタンスがその反復子オブジェクトに関連付けられているのです。 値`True`の呼び出し元に返される`MoveNext`、し、このインスタンスの制御点。 停止の次の呼び出しまで進める`MoveNext`反復子オブジェクト。

