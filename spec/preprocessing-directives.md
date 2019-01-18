---
ms.openlocfilehash: 7ad305f4b85bce12f174511af5e5f0aa62a7cc0e
ms.sourcegitcommit: 6eca149bdc736113e0adb709212bd266c9503c33
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2019
ms.locfileid: "47426741"
---
# <a name="preprocessing-directives"></a><span data-ttu-id="b3349-101">プリプロセス ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="b3349-101">Preprocessing Directives</span></span>

<span data-ttu-id="b3349-102">ファイルが構文的分析されたソースの前処理のいくつかの種類が発生します。</span><span class="sxs-lookup"><span data-stu-id="b3349-102">Once a file has been lexically analyzed, several kinds of source preprocessing occur.</span></span> <span data-ttu-id="b3349-103">最も重要な条件付きコンパイルは、構文、文法で処理されるソースを決定します。-- 外部ソース ディレクティブと領域ディレクティブ--ディレクティブの他の 2 つの型では、ソースについてのメタ情報を提供するが、コンパイルに影響はありません。</span><span class="sxs-lookup"><span data-stu-id="b3349-103">The most important, conditional compilation, determines which source is processed by the syntactic grammar; two other types of directives -- external source directives and region directives -- provide meta-information about the source but have no effect on compilation.</span></span>

## <a name="conditional-compilation"></a><span data-ttu-id="b3349-104">条件付きコンパイル</span><span class="sxs-lookup"><span data-stu-id="b3349-104">Conditional Compilation</span></span>

<span data-ttu-id="b3349-105">条件付きコンパイルは、論理行のシーケンスは、実際のコードに変換するかどうかを制御します。</span><span class="sxs-lookup"><span data-stu-id="b3349-105">Conditional compilation controls whether sequences of logical lines are translated into actual code.</span></span> <span data-ttu-id="b3349-106">条件付きコンパイルの先頭には、すべての論理行は有効になっています。ただし、条件付きコンパイル ステートメントで行を囲む可能性があります選択的に無効になり、コンパイル プロセスの残りの部分では無視されます、ファイル内でこれらの行。</span><span class="sxs-lookup"><span data-stu-id="b3349-106">At the beginning of conditional compilation, all logical lines are enabled; however, enclosing lines in conditional compilation statements may selectively disable those lines within the file, causing them to be ignored during the rest of the compilation process.</span></span>

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


<span data-ttu-id="b3349-107">たとえば、プログラム</span><span class="sxs-lookup"><span data-stu-id="b3349-107">For example, the program</span></span>

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

<span data-ttu-id="b3349-108">プログラムとトークンの正確な同じシーケンスを生成します。</span><span class="sxs-lookup"><span data-stu-id="b3349-108">produces the exact same sequence of tokens as the program</span></span>

```vb
Class C
    Sub F()
    End Sub

    Sub I()
    End Sub
End Class
```

<span data-ttu-id="b3349-109">条件付きコンパイル ディレクティブで許可される定数式では、一般的な定数式のサブセットです。</span><span class="sxs-lookup"><span data-stu-id="b3349-109">The constant expressions allowed in conditional compilation directives are a subset of general constant expressions.</span></span>

<span data-ttu-id="b3349-110">プリプロセッサでは、すべてのトークンの前後に空白と明示的な行継続を使用できます。</span><span class="sxs-lookup"><span data-stu-id="b3349-110">The preprocessor allows whitespace and explicit line continuations before and after every token.</span></span>


### <a name="conditional-constant-directives"></a><span data-ttu-id="b3349-111">条件付き定数ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="b3349-111">Conditional Constant Directives</span></span>

<span data-ttu-id="b3349-112">一定の条件付きステートメントでは、ソース ファイルをスコープ別の条件付きコンパイルの宣言領域内に存在する定数を定義します。</span><span class="sxs-lookup"><span data-stu-id="b3349-112">Conditional constant statements define constants that exist in a separate conditional compilation declaration space scoped to the source file.</span></span>

```antlr
CCConstantDeclaration
    : '#' 'Const' Identifier '=' CCExpression LineTerminator
    ;
```

<span data-ttu-id="b3349-113">宣言領域は、条件付きコンパイル定数を明示的に宣言する必要はありません--条件付き定数を条件付きコンパイル ディレクティブで暗黙的に定義できる点で特殊です。</span><span class="sxs-lookup"><span data-stu-id="b3349-113">The declaration space is special in that no explicit declaration of conditional compilation constants is necessary -- conditional constants can be implicitly defined in a conditional compilation directive.</span></span>

<span data-ttu-id="b3349-114">条件付きコンパイル定数値を割り当てられている、前に値を持つ`Nothing`します。</span><span class="sxs-lookup"><span data-stu-id="b3349-114">Prior to being assigned a value, a conditional compilation constant has the value `Nothing`.</span></span> <span data-ttu-id="b3349-115">条件付きコンパイル定数には、定数式である必要があり、値が割り当てられたときに割り当てられている値の型が定数の型になります。</span><span class="sxs-lookup"><span data-stu-id="b3349-115">When a conditional compilation constant is assigned a value, which must be a constant expression, the type of the constant becomes the type of the value being assigned to it.</span></span> <span data-ttu-id="b3349-116">ソース ファイルでは、条件付きコンパイル定数を何度も再定義可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b3349-116">A conditional compilation constant may be redefined multiple times throughout a source file.</span></span>

<span data-ttu-id="b3349-117">たとえば、次のコードは、文字列のみを出力します。`about to print value`の値と`Test`します。</span><span class="sxs-lookup"><span data-stu-id="b3349-117">For example, the following code prints only the string `about to print value` and the value of `Test`.</span></span>

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

<span data-ttu-id="b3349-118">コンパイル環境でも、条件付き定数を条件付きコンパイルの宣言領域で定義します。</span><span class="sxs-lookup"><span data-stu-id="b3349-118">The compilation environment may also define conditional constants in a conditional compilation declaration space.</span></span>


### <a name="conditional-compilation-directives"></a><span data-ttu-id="b3349-119">条件付きコンパイル ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="b3349-119">Conditional Compilation Directives</span></span>

<span data-ttu-id="b3349-120">条件付きコンパイル ディレクティブは、条件付きコンパイルを制御します。</span><span class="sxs-lookup"><span data-stu-id="b3349-120">Conditional compilation directives control conditional compilation.</span></span>

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

<span data-ttu-id="b3349-121">条件付き定数には、定数式と条件付きコンパイル定数のみを参照できます。</span><span class="sxs-lookup"><span data-stu-id="b3349-121">Conditional constants can only reference constant expressions and conditional compilation constants.</span></span> <span data-ttu-id="b3349-122">各条件付きコンパイルを 1 つのグループ内の定数式が評価され、変換、`Boolean`に評価される式の最初から最後まで、条件付きのいずれかからテキストの順序で型`True`します。</span><span class="sxs-lookup"><span data-stu-id="b3349-122">Each of the constant expressions within a single conditional compilation group is evaluated and converted to the `Boolean` type in textual order from first to last until one of the conditional expressions evaluates to `True`.</span></span> <span data-ttu-id="b3349-123">式に変換できない場合`Boolean`コンパイル時エラーの結果。</span><span class="sxs-lookup"><span data-stu-id="b3349-123">If an expression is not convertible to `Boolean`, a compile-time error results.</span></span> <span data-ttu-id="b3349-124">制限の緩やかなセマンティクスとバイナリ文字列比較は常に使用いずれかに関係なく、条件付きコンパイル定数式を評価するときに`Option`ディレクティブまたはコンパイル環境の設定。</span><span class="sxs-lookup"><span data-stu-id="b3349-124">Permissive semantics and binary string comparisons are always used when evaluating conditional compilation constant expressions, regardless of any `Option` directives or compilation environment settings.</span></span>

<span data-ttu-id="b3349-125">ステートメントを含む、間に行を除く、入れ子になった条件付きコンパイル ディレクティブを含む、グループで囲まれたすべての行が無効になっている、`True`式とグループ、または、間の線の次の条件付きステートメント`Else`ステートメントと`End If`ステートメント場合、`Else`グループ内に表示するすべての式を評価および`False`。</span><span class="sxs-lookup"><span data-stu-id="b3349-125">All lines enclosed by the group, including nested conditional compilation directives, are disabled except for lines between the statement containing the `True` expression and the next conditional statement of the group, or lines between the `Else` statement and the `End If` statement if an `Else` appears in the group and all of the expressions evaluate to `False`.</span></span>

<span data-ttu-id="b3349-126">この例への呼び出しで`WriteToLog`で、`Trace`ために、条件付きコンパイル ディレクティブは処理されませんが周囲`Debug`に評価される条件付きコンパイル ディレクティブ`False`します。</span><span class="sxs-lookup"><span data-stu-id="b3349-126">In this example, the call to `WriteToLog` in the `Trace` conditional compilation directive is not processed because the surrounding `Debug` conditional compilation directive evaluates to `False`.</span></span>

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


## <a name="external-source-directives"></a><span data-ttu-id="b3349-127">外部ソース ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="b3349-127">External Source Directives</span></span>

<span data-ttu-id="b3349-128">ソース ファイルには、ソース行とソース外部のテキスト間のマッピングを示す外部ソース ディレクティブを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="b3349-128">A source file may include external source directives that indicate a mapping between source lines and text external to the source.</span></span>

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

<span data-ttu-id="b3349-129">外部ソース ディレクティブはコンパイルに影響を与えるありません、入れ子にすることがありますできません。</span><span class="sxs-lookup"><span data-stu-id="b3349-129">External source directives have no effect on compilation and may not be nested.</span></span> <span data-ttu-id="b3349-130">例:</span><span class="sxs-lookup"><span data-stu-id="b3349-130">For example:</span></span>

```vb
Module Test
    Sub Main()

#ExternalSource("c:\wwwroot\inetpub\test.aspx", 30)
        Console.WriteLine("In test.aspx")
#End ExternalSource

    End Sub
End Module
```


## <a name="region-directives"></a><span data-ttu-id="b3349-131">領域ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="b3349-131">Region Directives</span></span>

<span data-ttu-id="b3349-132">領域ディレクティブでは、ソース コードの行をグループ化が、コンパイル時に他の効果はありません。</span><span class="sxs-lookup"><span data-stu-id="b3349-132">Region directives group lines of source code but have no other effect on compilation.</span></span> <span data-ttu-id="b3349-133">グループ全体を折りたたまれていると、非表示または展開し、統合開発環境 (IDE) で、表示します。</span><span class="sxs-lookup"><span data-stu-id="b3349-133">The entire group can be collapsed and hidden, or expanded and viewed, in the integrated development environment (IDE).</span></span>

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

<span data-ttu-id="b3349-134">領域を入れ子にすることがあります。</span><span class="sxs-lookup"><span data-stu-id="b3349-134">Regions may be nested.</span></span> <span data-ttu-id="b3349-135">領域ディレクティブでは開始も、メソッド本体の内部を終了し、プログラムのブロック構造を考慮する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b3349-135">Region directives are special in that they can neither start nor terminate within a method body, and they must respect the block structure of the program.</span></span> <span data-ttu-id="b3349-136">例:</span><span class="sxs-lookup"><span data-stu-id="b3349-136">For example:</span></span>

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


## <a name="external-checksum-directives"></a><span data-ttu-id="b3349-137">外部チェックサム ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="b3349-137">External Checksum Directives</span></span>

<span data-ttu-id="b3349-138">ソース ファイルには、外部ソース ディレクティブで参照されているファイルのどのようなチェックサムを生成する必要を示す外部チェックサム ディレクティブを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="b3349-138">A source file may include an external checksum directive that indicates what checksum should be emitted for a file referenced in an external source directive.</span></span> <span data-ttu-id="b3349-139">他のすべての点で外部ソース ディレクティブのコンパイルに影響はあるありません。</span><span class="sxs-lookup"><span data-stu-id="b3349-139">In all other respects external source directives have no effect on compilation.</span></span>

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

<span data-ttu-id="b3349-140">外部チェックサム ディレクティブには、外部のファイルでは、ファイルおよびファイルのチェックサムに関連付けられているグローバル一意識別子 (GUID) のファイル名が含まれています。</span><span class="sxs-lookup"><span data-stu-id="b3349-140">An external checksum directive contains the filename of the external file, a globally unique identifier (GUID) associated with the file and the checksum for the file.</span></span> <span data-ttu-id="b3349-141">GUID は、フォーム「{xxxxxxxx xxxx-xxxx-。}」、x は 16 進数の文字列定数として指定されます。</span><span class="sxs-lookup"><span data-stu-id="b3349-141">The GUID is specified as a string constant of the form "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}", where x is a hexadecimal digit.</span></span> <span data-ttu-id="b3349-142">チェックサムは、x は 16 進数形式"xxxx..."の文字列定数として指定されます。</span><span class="sxs-lookup"><span data-stu-id="b3349-142">The checksum is specified as a string constant of the form "xxxx...", where x is a hexadecimal digit.</span></span> <span data-ttu-id="b3349-143">チェックサムの数字の数は偶数である必要があります。</span><span class="sxs-lookup"><span data-stu-id="b3349-143">The number of digits in a checksum must be an even number.</span></span>

<span data-ttu-id="b3349-144">外部ファイルには、関連付けられているすべての GUID とチェックサム値が正確に一致する複数の外部チェックサム ディレクティブがあります。</span><span class="sxs-lookup"><span data-stu-id="b3349-144">An external file may have multiple external checksum directives associated with it provided that all of the GUID and checksum values match exactly.</span></span> <span data-ttu-id="b3349-145">外部ファイルの名前には、コンパイルされるファイルの名前が一致すると、コンパイラのチェックサムの計算を優先して、チェックサムは無視されます。</span><span class="sxs-lookup"><span data-stu-id="b3349-145">If the name of the external file matches the name of a file being compiled, the checksum is ignored in favor of the compiler's checksum calculation.</span></span>

<span data-ttu-id="b3349-146">例:</span><span class="sxs-lookup"><span data-stu-id="b3349-146">For example:</span></span>

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

