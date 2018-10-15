# <a name="preprocessing-directives"></a>プリプロセス ディレクティブ

ファイルが構文的分析されたソースの前処理のいくつかの種類が発生します。 最も重要な条件付きコンパイルは、構文、文法で処理されるソースを決定します。-- 外部ソース ディレクティブと領域ディレクティブ--ディレクティブの他の 2 つの型では、ソースについてのメタ情報を提供するが、コンパイルに影響はありません。

## <a name="conditional-compilation"></a>条件付きコンパイル

条件付きコンパイルは、論理行のシーケンスは、実際のコードに変換するかどうかを制御します。 条件付きコンパイルの先頭には、すべての論理行は有効になっています。ただし、条件付きコンパイル ステートメントで行を囲む可能性があります選択的に無効になり、コンパイル プロセスの残りの部分では無視されます、ファイル内でこれらの行。

```antlr
CCStart
    : CCStatement*
    ;

CCStatement
    : CCConstantDeclaration
    | CCIfGroup
    | LogicalLine
    ;

CCExpression
    : LiteralExpression
    | CCParenthesizedExpression
    | CCSimpleNameExpression
    | CCCastExpression
    | CCOperatorExpression
    | CCConditionalExpression
    ;

CCParenthesizedExpression
    : '(' CCExpression ')'
    ;

CCSimpleNameExpression
    : Identifier
    ;

CCCastExpression
    : 'DirectCast' '(' CCExpression ',' TypeName ')'
    | 'TryCast' '(' CCExpression ',' TypeName ')'
    | 'CType' '(' CCExpression ',' TypeName ')'
    | CastTarget '(' CCExpression ')'
    ;

CCOperatorExpression
    : CCUnaryOperator CCExpression
    | CCExpression CCBinaryOperator CCExpression
    ;

CCUnaryOperator
    : '+' | '-' | 'Not'
    ;

CCBinaryOperator
    : '+'     | '-'     | '*'   | '/'       | '\\'     | 'Mod' | '^' | '='
    | '<' '>' | '<'     | '>'   | '<' '='   | '>' '='  | '&'
    | 'And'   | 'Or'    | 'Xor' | 'AndAlso' | 'OrElse'
    | '<' '<' | '>' '>'
    ;

CCConditionalExpression
    : 'If' '(' CCExpression ',' CCExpression ',' CCExpression ')'
    | 'If' '(' CCExpression ',' CCExpression ')'
    ;
```


たとえば、プログラム

```vb
#Const A = True
#Const B = False

Class C

#If A Then
    Sub F()
    End Sub
#Else
    Sub G()
    End Sub
#End If

#If B Then
    Sub H()
    End Sub
#Else
    Sub I()
    End Sub
#End If

End Class
```

プログラムとトークンの正確な同じシーケンスを生成します。

```vb
Class C
    Sub F()
    End Sub

    Sub I()
    End Sub
End Class
```

条件付きコンパイル ディレクティブで許可される定数式では、一般的な定数式のサブセットです。

プリプロセッサでは、すべてのトークンの前後に空白と明示的な行継続を使用できます。


### <a name="conditional-constant-directives"></a>条件付き定数ディレクティブ

一定の条件付きステートメントでは、ソース ファイルをスコープ別の条件付きコンパイルの宣言領域内に存在する定数を定義します。

```antlr
CCConstantDeclaration
    : '#' 'Const' Identifier '=' CCExpression LineTerminator
    ;
```

宣言領域は、条件付きコンパイル定数を明示的に宣言する必要はありません--条件付き定数を条件付きコンパイル ディレクティブで暗黙的に定義できる点で特殊です。

条件付きコンパイル定数値を割り当てられている、前に値を持つ`Nothing`します。 条件付きコンパイル定数には、定数式である必要があり、値が割り当てられたときに割り当てられている値の型が定数の型になります。 ソース ファイルでは、条件付きコンパイル定数を何度も再定義可能性があります。

たとえば、次のコードは、文字列のみを出力します。`about to print value`の値と`Test`します。

```vb
Module M1
    Sub PrintValue(Test As Integer)

#Const DebugCode = True

#If DebugCode Then
        Console.WriteLine("about to print value")
#End If

#Const DebugCode = False

        Console.WriteLine(Test)

#If DebugCode Then
        Console.WriteLine("printed value")
#End If

    End Sub
End Module
```

コンパイル環境でも、条件付き定数を条件付きコンパイルの宣言領域で定義します。


### <a name="conditional-compilation-directives"></a>条件付きコンパイル ディレクティブ

条件付きコンパイル ディレクティブは、条件付きコンパイルを制御します。

```antlr
CCIfGroup
    : '#' 'If' CCExpression 'Then'? LineTerminator CCStatement*
    CCElseIfGroup* CCElseGroup? '#' 'End' 'If' LineTerminator
    ;

CCElseIfGroup
    : '#' ElseIf CCExpression 'Then'? LineTerminator CCStatement*
    ;

CCElseGroup
    : '#' 'Else' LineTerminator CCStatement*
    ;
```

条件付き定数には、定数式と条件付きコンパイル定数のみを参照できます。 各条件付きコンパイルを 1 つのグループ内の定数式が評価され、変換、`Boolean`に評価される式の最初から最後まで、条件付きのいずれかからテキストの順序で型`True`します。 式に変換できない場合`Boolean`コンパイル時エラーの結果。 制限の緩やかなセマンティクスとバイナリ文字列比較は常に使用いずれかに関係なく、条件付きコンパイル定数式を評価するときに`Option`ディレクティブまたはコンパイル環境の設定。

ステートメントを含む、間に行を除く、入れ子になった条件付きコンパイル ディレクティブを含む、グループで囲まれたすべての行が無効になっている、`True`式とグループ、または、間の線の次の条件付きステートメント`Else`ステートメントと`End If`ステートメント場合、`Else`グループ内に表示するすべての式を評価および`False`。

この例への呼び出しで`WriteToLog`で、`Trace`ために、条件付きコンパイル ディレクティブは処理されませんが周囲`Debug`に評価される条件付きコンパイル ディレクティブ`False`します。

```vb
#Const Debug = False   ' Debugging off
#Const Trace = True    ' Tracing on

Class PurchaseTransaction
    Sub Commit()

#If Debug Then
        CheckConsistency()
#If Trace Then
        WriteToLog(Me.ToString())
#End If
#End If
        ...
    End Sub
End Class
```


## <a name="external-source-directives"></a>外部ソース ディレクティブ

ソース ファイルには、ソース行とソース外部のテキスト間のマッピングを示す外部ソース ディレクティブを含めることができます。

```antlr
ESDStart
    : ExternalSourceStatement*
    ;

ExternalSourceStatement
    : ExternalSourceGroup
    | LogicalLine
    ;

ExternalSourceGroup
    : '#' 'ExternalSource' '(' StringLiteral ',' IntLiteral ')' LineTerminator
      LogicalLine* '#' 'End' 'ExternalSource' LineTerminator
    ;
```

外部ソース ディレクティブはコンパイルに影響を与えるありません、入れ子にすることがありますできません。 例えば:

```vb
Module Test
    Sub Main()

#ExternalSource("c:\wwwroot\inetpub\test.aspx", 30)
        Console.WriteLine("In test.aspx")
#End ExternalSource

    End Sub
End Module
```


## <a name="region-directives"></a>領域ディレクティブ

領域ディレクティブでは、ソース コードの行をグループ化が、コンパイル時に他の効果はありません。 グループ全体を折りたたまれていると、非表示または展開し、統合開発環境 (IDE) で、表示します。

```antlr
RegionStart
    : RegionStatement*
    ;

RegionStatement
    : RegionGroup
    | LogicalLine
    ;

RegionGroup
    : '#' 'Region' StringLiteral LineTerminator
      RegionStatement*
      '#' 'End' 'Region' LineTerminator
    ;
```

領域を入れ子にすることがあります。 領域ディレクティブでは開始も、メソッド本体の内部を終了し、プログラムのブロック構造を考慮する必要があります。 例えば:

```vb
Module Test
#Region "Startup code - do not edit"
    Sub Main()
    End Sub
#End Region

End Module


' Error due to Region directives breaking the block structure
Class C
#Region "Fred"
End Class
#End Region
```


## <a name="external-checksum-directives"></a>外部チェックサム ディレクティブ

ソース ファイルには、外部ソース ディレクティブで参照されているファイルのどのようなチェックサムを生成する必要を示す外部チェックサム ディレクティブを含めることができます。 他のすべての点で外部ソース ディレクティブのコンパイルに影響はあるありません。

```antlr
ExternalChecksumStart
    : ExternalChecksumStatement*
    ;

ExternalChecksumStatement
    : '#' 'ExternalChecksum' '('
      StringLiteral ',' StringLiteral ',' StringLiteral
      ')' LineTerminator
    ;
```

外部チェックサム ディレクティブには、外部のファイルでは、ファイルおよびファイルのチェックサムに関連付けられているグローバル一意識別子 (GUID) のファイル名が含まれています。 GUID は、フォーム「{xxxxxxxx xxxx-xxxx-。}」、x は 16 進数の文字列定数として指定されます。 チェックサムは、x は 16 進数形式"xxxx..."の文字列定数として指定されます。 チェックサムの数字の数は偶数である必要があります。

外部ファイルには、関連付けられているすべての GUID とチェックサム値が正確に一致する複数の外部チェックサム ディレクティブがあります。 外部ファイルの名前には、コンパイルされるファイルの名前が一致すると、コンパイラのチェックサムの計算を優先して、チェックサムは無視されます。

例えば:

```vb
#ExternalChecksum("c:\wwwroot\inetpub\test.aspx", _
    "{12345678-1234-1234-1234-123456789abc}", _
    "1a2b3c4e5f617239a49b9a9c0391849d34950f923fab9484")

Module Test
    Sub Main()

#ExternalSource("c:\wwwroot\inetpub\test.aspx", 30)
        Console.WriteLine("In test.aspx")
#End ExternalSource

    End Sub
End Module
```

