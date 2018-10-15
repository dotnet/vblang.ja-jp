# <a name="source-files-and-namespaces"></a><span data-ttu-id="08fd6-101">ソース ファイルと名前空間</span><span class="sxs-lookup"><span data-stu-id="08fd6-101">Source Files and Namespaces</span></span>

<span data-ttu-id="08fd6-102">Visual Basic プログラムは、1 つまたは複数のソース ファイルで構成されます。</span><span class="sxs-lookup"><span data-stu-id="08fd6-102">A Visual Basic program consists of one or more source files.</span></span> <span data-ttu-id="08fd6-103">すべてのソース ファイルがまとめて; 処理プログラムがコンパイルされると、そのため、ソース ファイルは、前方宣言必要はありません、循環形式で可能性があります、互いに依存することができます。</span><span class="sxs-lookup"><span data-stu-id="08fd6-103">When a program is compiled, all of the source files are processed together; thus, source files can depend on each other, possibly in a circular fashion, without any forward-declaration requirement.</span></span> <span data-ttu-id="08fd6-104">プログラム テキスト内の宣言の順序は、一般にあまり意味はありません。</span><span class="sxs-lookup"><span data-stu-id="08fd6-104">The textual order of declarations in the program text is generally of no significance.</span></span>

<span data-ttu-id="08fd6-105">ソース ファイルは、ステートメントのオプション、import ステートメント、および属性は、次の名前空間の本体のオプションのセットで構成されます。</span><span class="sxs-lookup"><span data-stu-id="08fd6-105">A source file consists of an optional set of option statements, import statements, and attributes, which are followed by a namespace body.</span></span> <span data-ttu-id="08fd6-106">属性は、いずれかがある各する必要があります、`Assembly`または`Module`修飾子は、.NET アセンブリ、またはコンパイルによって生成されたモジュールに適用されます。</span><span class="sxs-lookup"><span data-stu-id="08fd6-106">The attributes, which must each have either the `Assembly` or `Module` modifier, apply to the .NET assembly or module produced by the compilation.</span></span> <span data-ttu-id="08fd6-107">ソース ファイルの本文は、グローバル名前空間、つまりソース ファイルの最上位レベルにあるすべての宣言がグローバル名前空間内に配置される、暗黙的な名前空間宣言として機能します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-107">The body of the source file functions as an implicit namespace declaration for the global namespace, meaning that all declarations at the top level of a source file are placed in the global namespace.</span></span> <span data-ttu-id="08fd6-108">例えば:</span><span class="sxs-lookup"><span data-stu-id="08fd6-108">For example:</span></span>

<span data-ttu-id="08fd6-109">FileA.vb:</span><span class="sxs-lookup"><span data-stu-id="08fd6-109">FileA.vb:</span></span>

```vb
Class A
End Class
```

<span data-ttu-id="08fd6-110">FileB.vb:</span><span class="sxs-lookup"><span data-stu-id="08fd6-110">FileB.vb:</span></span>

```vb
Class B
End Class
```

<span data-ttu-id="08fd6-111">この場合、完全修飾名を持つ 2 つのクラスを宣言する、グローバル名前空間を 2 つのソース ファイルが投稿`A`と`B`します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-111">The two source files contribute to the global namespace, in this case declaring two classes with the fully qualified names `A` and `B`.</span></span> <span data-ttu-id="08fd6-112">同じ宣言領域に 2 つのソース ファイルがドキュメントに投稿されているとエラーと同じ名前のメンバーの宣言をそれぞれに含まれる場合。</span><span class="sxs-lookup"><span data-stu-id="08fd6-112">Because the two source files contribute to the same declaration space, it would have been an error if each contained a declaration of a member with the same name.</span></span>

<span data-ttu-id="08fd6-113">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="08fd6-113">__Note.__</span></span> <span data-ttu-id="08fd6-114">コンパイル環境は、ソース ファイルを暗黙的に配置する名前空間宣言をオーバーライドすることができます。</span><span class="sxs-lookup"><span data-stu-id="08fd6-114">The compilation environment may override the namespace declarations into which a source file is implicitly placed.</span></span>

<span data-ttu-id="08fd6-115">場合を除き、行終端記号またはコロンのいずれか、Visual Basic プログラム内のステートメントを終了できます。</span><span class="sxs-lookup"><span data-stu-id="08fd6-115">Except where noted, statements within a Visual Basic program can be terminated either by a line terminator or by a colon.</span></span>

```antlr
Start
    : OptionStatement* ImportsStatement* AttributesStatement* NamespaceMemberDeclaration*
    ;

StatementTerminator
    : LineTerminator
    | ':'
    ;

AttributesStatement
    : Attributes StatementTerminator
    ;
```

## <a name="program-startup-and-termination"></a><span data-ttu-id="08fd6-116">プログラムの起動と終了</span><span class="sxs-lookup"><span data-stu-id="08fd6-116">Program Startup and Termination</span></span>

<span data-ttu-id="08fd6-117">プログラムの起動は、実行環境がプログラムのと呼ばれる、指定されたメソッドを実行するときに発生します。*エントリ ポイント*します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-117">Program startup occurs when the execution environment executes a designated method, which is referred to as the program's *entry point*.</span></span> <span data-ttu-id="08fd6-118">名前付き常にこのエントリ ポイント メソッド`Main`、共有する必要があります、ジェネリック型に含めることはできませんに async 修飾子を含めることはできません、シグネチャは次のいずれかが必要があります。</span><span class="sxs-lookup"><span data-stu-id="08fd6-118">This entry point method, which must always be named `Main`, must be shared, cannot be contained in a generic type, cannot have the async modifier, and must have one of the following signatures:</span></span>

```vb
Sub Main()
Sub Main(args() As String)
Function Main() As Integer
Function Main(args() As String) As Integer
```

<span data-ttu-id="08fd6-119">エントリ ポイント メソッドのアクセシビリティは関係ありません。</span><span class="sxs-lookup"><span data-stu-id="08fd6-119">The accessibility of the entry point method is irrelevant.</span></span> <span data-ttu-id="08fd6-120">プログラムには、複数の適切なエントリ ポイントが含まれている場合、コンパイル環境は、エントリ ポイントとして 1 つを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="08fd6-120">If a program contains more than one suitable entry point, the compilation environment must designate one as the entry point.</span></span> <span data-ttu-id="08fd6-121">それ以外の場合は、コンパイル時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-121">Otherwise, a compile-time error occurs.</span></span> <span data-ttu-id="08fd6-122">コンパイル環境は、1 つが存在しない場合、エントリ ポイント メソッドを作成もできます。</span><span class="sxs-lookup"><span data-stu-id="08fd6-122">The compilation environment may also create an entry point method if one does not exist.</span></span>

<span data-ttu-id="08fd6-123">エントリ ポイントは、パラメーターを持つ場合、プログラムを開始、実行環境によって提供される引数には、文字列として表されるプログラムにコマンドライン引数が含まれています。</span><span class="sxs-lookup"><span data-stu-id="08fd6-123">When a program begins, if the entry point has a parameter, the argument supplied by the execution environment contains the command-line arguments to the program represented as strings.</span></span> <span data-ttu-id="08fd6-124">エントリ ポイントがある戻り値の型の場合`Integer`、実行環境に、プログラムの結果として、関数から返される値が返されます。</span><span class="sxs-lookup"><span data-stu-id="08fd6-124">If the entry point has a return type of `Integer`, then the value returned from the function is returned to the execution environment as the result of the program.</span></span>

<span data-ttu-id="08fd6-125">その他のすべての点では、エントリ ポイント メソッドは、他のメソッドと同様に動作します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-125">In all other respects, entry point methods behave in the same manner as other methods.</span></span> <span data-ttu-id="08fd6-126">実行が、実行環境によって行われたエントリ ポイント メソッドの呼び出しを離れると、プログラムを終了します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-126">When execution leaves the invocation of the entry point method made by the execution environment, the program terminates.</span></span>

## <a name="compilation-options"></a><span data-ttu-id="08fd6-127">コンパイル オプション</span><span class="sxs-lookup"><span data-stu-id="08fd6-127">Compilation Options</span></span>

<span data-ttu-id="08fd6-128">ソース ファイルは、ソース コードを使用してコンパイル オプションを指定できます*オプション ステートメント*します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-128">A source file can specify compilation options in the source code using *option statements*.</span></span>

```antlr
OptionStatement
    : OptionExplicitStatement
    | OptionStrictStatement
    | OptionCompareStatement
    | OptionInferStatement
    ;
```

<span data-ttu-id="08fd6-129">`Option`ステートメントが、それが表示されたら、ソース ファイルとの種類ごとの 1 つだけにのみ適用されます`Option`ステートメントは、ソース ファイルになっている可能性があります。</span><span class="sxs-lookup"><span data-stu-id="08fd6-129">An `Option` statement applies only to the source file in which it appears, and only one of each type of `Option` statement may appear in a source file.</span></span> <span data-ttu-id="08fd6-130">例えば:</span><span class="sxs-lookup"><span data-stu-id="08fd6-130">For example:</span></span>

```vb
Option Strict On
Option Compare Text
Option Strict Off    ' Not allowed, Option Strict is already specified.
Option Compare Text  ' Not allowed, Option Compare is already specified.
```

<span data-ttu-id="08fd6-131">次の 4 つのコンパイル オプションがあります。 厳密な型のセマンティクス、明示的な宣言のセマンティクス、比較のセマンティクス、およびローカル変数の型の推論のセマンティクスです。</span><span class="sxs-lookup"><span data-stu-id="08fd6-131">There are four compilation options: strict type semantics, explicit declaration semantics, comparison semantics, and local variable type inference semantics.</span></span> <span data-ttu-id="08fd6-132">ソース ファイルには特定が含まれていない場合`Option`ステートメントでは、次のコンパイル環境を決定する特定のセットのセマンティクスが使用されます。</span><span class="sxs-lookup"><span data-stu-id="08fd6-132">If a source file does not include a particular `Option` statement, then the compilation environment determines which particular set of semantics will be used.</span></span> <span data-ttu-id="08fd6-133">5 番目のコンパイル オプションのコンパイル環境でのみ指定できる整数オーバーフローのチェックもあります。</span><span class="sxs-lookup"><span data-stu-id="08fd6-133">There is also a fifth compilation option, integer overflow checks, which can only be specified through the compilation environment.</span></span>


### <a name="option-explicit-statement"></a><span data-ttu-id="08fd6-134">Option Explicit ステートメント</span><span class="sxs-lookup"><span data-stu-id="08fd6-134">Option Explicit Statement</span></span>

<span data-ttu-id="08fd6-135">`Option Explicit`ステートメントは、ローカル変数が暗黙的に宣言されたかどうかを決定します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-135">The `Option Explicit` statement determines whether local variables may be implicitly declared.</span></span> <span data-ttu-id="08fd6-136">キーワード`On`または`Off`が次のステートメントはどちらも指定しない場合、既定値は`On`します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-136">The keywords `On` or `Off` may follow the statement; if neither is specified, the default is `On`.</span></span> <span data-ttu-id="08fd6-137">ファイル内のステートメントが指定されていない場合は、どちらを使用するコンパイル環境が決定します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-137">If no statement is specified in a file, the compilation environment determines which will be used.</span></span>

```antlr
OptionExplicitStatement
    : 'Option' 'Explicit' OnOff? StatementTerminator
    ;

OnOff
    : 'On' | 'Off'
    ;
```


<span data-ttu-id="08fd6-138">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="08fd6-138">__Note.__</span></span> <span data-ttu-id="08fd6-139">`Explicit` `Off`予約された単語は使用されません。</span><span class="sxs-lookup"><span data-stu-id="08fd6-139">`Explicit` and `Off` are not reserved words.</span></span>

```vb
Option Explicit Off

Module Test
    Sub Main()
        x = 5 ' Valid because Option Explicit is off.
    End Sub
End Module
```

<span data-ttu-id="08fd6-140">この例では、ローカル変数で`x`に割り当てることで暗黙的に宣言されています。</span><span class="sxs-lookup"><span data-stu-id="08fd6-140">In this example, the local variable `x` is implicitly declared by assigning to it.</span></span> <span data-ttu-id="08fd6-141">`x` の型は `Object` です。</span><span class="sxs-lookup"><span data-stu-id="08fd6-141">The type of `x` is `Object`.</span></span>


### <a name="option-strict-statement"></a><span data-ttu-id="08fd6-142">Option Strict Statement</span><span class="sxs-lookup"><span data-stu-id="08fd6-142">Option Strict Statement</span></span>

<span data-ttu-id="08fd6-143">`Option Strict`ステートメントを決定するかどうか変換と操作に`Object`は厳密なまたは制限の緩やかな型のセマンティクスと型として暗黙的に型指定するかどうかによって管理されます`Object`いない場合`As`句を指定します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-143">The `Option Strict` statement determines whether conversions and operations on `Object` are governed by strict or permissive type semantics and whether types are implicitly typed as `Object` if no `As` clause is specified.</span></span> <span data-ttu-id="08fd6-144">キーワードで、ステートメントの後にこと`On`または`Off`、どちらも指定されている場合、既定値は`On`します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-144">The statement may be followed by the keywords `On` or `Off`; if neither is specified, the default is `On`.</span></span> <span data-ttu-id="08fd6-145">ファイル内のステートメントが指定されていない場合は、どちらを使用するコンパイル環境が決定します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-145">If no statement is specified in a file, the compilation environment determines which will be used.</span></span>

```antlr
OptionStrictStatement
    : 'Option' 'Strict' OnOff? StatementTerminator
    ;
```

<span data-ttu-id="08fd6-146">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="08fd6-146">__Note.__</span></span> <span data-ttu-id="08fd6-147">`Strict` `Off`予約された単語は使用されません。</span><span class="sxs-lookup"><span data-stu-id="08fd6-147">`Strict` and `Off` are not reserved words.</span></span>

```vb
Option Strict On

Module Test
    Sub Main()
        Dim x ' Error, no type specified.
        Dim o As Object
        Dim b As Byte = o ' Error, narrowing conversion.

        o.F() ' Error, late binding disallowed.
        o = o + 1 ' Error, addition is not defined on Object.
    End Sub
End Module
```

<span data-ttu-id="08fd6-148">厳密な型は、次は許可されません。</span><span class="sxs-lookup"><span data-stu-id="08fd6-148">Under strict semantics, the following are disallowed:</span></span>

* <span data-ttu-id="08fd6-149">明示的なキャスト演算子がない縮小変換します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-149">Narrowing conversions without an explicit cast operator.</span></span>

* <span data-ttu-id="08fd6-150">遅延バインディング。</span><span class="sxs-lookup"><span data-stu-id="08fd6-150">Late binding.</span></span>

* <span data-ttu-id="08fd6-151">型に対する操作`Object`以外`TypeOf`.`Is`、 `Is`、および`IsNot`します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-151">Operations on type `Object` other than `TypeOf`...`Is`, `Is`, and `IsNot`.</span></span>

* <span data-ttu-id="08fd6-152">省略すると、`As`推論された型がない宣言内の句。</span><span class="sxs-lookup"><span data-stu-id="08fd6-152">Omitting the `As` clause in a declaration that does not have an inferred type.</span></span>


### <a name="option-compare-statement"></a><span data-ttu-id="08fd6-153">Option Compare ステートメント</span><span class="sxs-lookup"><span data-stu-id="08fd6-153">Option Compare Statement</span></span>

<span data-ttu-id="08fd6-154">`Option Compare`ステートメントは、文字列比較のセマンティクスを決定します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-154">The `Option Compare` statement determines the semantics of string comparisons.</span></span> <span data-ttu-id="08fd6-155">文字列比較が実行するか (これで各文字のバイナリの Unicode 値と比較されます)、バイナリの比較またはテキストの比較 (で各文字の構文の意味と比較して、現在のカルチャを使用して) を使用します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-155">String comparisons are carried out either using binary comparisons (in which the binary Unicode value of each character is compared) or text comparisons (in which the lexical meaning of each character is compared using the current culture).</span></span> <span data-ttu-id="08fd6-156">ファイル内のステートメントが指定されていない場合のコンパイル環境は、比較の種類が使用を制御します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-156">If no statement is specified in a file, the compilation environment controls which type of comparison will be used.</span></span>

```antlr
OptionCompareStatement
    : 'Option' 'Compare' CompareOption StatementTerminator
    ;

CompareOption
    : 'Binary' | 'Text'
    ;
```

<span data-ttu-id="08fd6-157">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="08fd6-157">__Note.__</span></span> <span data-ttu-id="08fd6-158">`Compare`、 `Binary`、および`Text`予約された単語は使用されません。</span><span class="sxs-lookup"><span data-stu-id="08fd6-158">`Compare`, `Binary`, and `Text` are not reserved words.</span></span>

```vb
Option Compare Text

Module Test
    Sub Main()
        Console.WriteLine("a" = "A")    ' Prints True.
    End Sub
End Module
```

<span data-ttu-id="08fd6-159">文字列比較を実行するこの例では、大文字小文字の違いを無視する文字列比較を使用します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-159">In this case, the string comparison is done using a text comparison that ignores case differences.</span></span> <span data-ttu-id="08fd6-160">場合`Option Compare Binary`これが印刷し、指定されている`False`します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-160">If `Option Compare Binary` had been specified, then this would have printed `False`.</span></span>


### <a name="integer-overflow-checks"></a><span data-ttu-id="08fd6-161">整数オーバーフローのチェック</span><span class="sxs-lookup"><span data-stu-id="08fd6-161">Integer Overflow Checks</span></span>

<span data-ttu-id="08fd6-162">整数演算するチェックか、実行時にオーバーフロー状態をチェックされません。</span><span class="sxs-lookup"><span data-stu-id="08fd6-162">Integer operations can either be checked or not checked for overflow conditions at run time.</span></span> <span data-ttu-id="08fd6-163">オーバーフロー状態がチェックされ、整数演算がオーバーフローした場合、`System.OverflowException`例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="08fd6-163">If overflow conditions are checked and an integer operation overflows, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="08fd6-164">オーバーフロー状態がチェックされていない場合、整数演算のオーバーフロー時に例外はスローされません。</span><span class="sxs-lookup"><span data-stu-id="08fd6-164">If overflow conditions are not checked, integer operation overflows do not throw an exception.</span></span> <span data-ttu-id="08fd6-165">コンパイル環境では、このオプションがオンかオフかどうかを決定します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-165">The compilation environment determines whether this option is on or off.</span></span>

### <a name="option-infer-statement"></a><span data-ttu-id="08fd6-166">Option Infer ステートメント</span><span class="sxs-lookup"><span data-stu-id="08fd6-166">Option Infer Statement</span></span>

<span data-ttu-id="08fd6-167">`Option Infer`ステートメントは、ローカルかどうかを決定します。 変数の宣言を持たず`As`句は、推論された型または使用がある`Object`します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-167">The `Option Infer` statement determines whether local variable declarations that have no `As` clause have an inferred type or use `Object`.</span></span> <span data-ttu-id="08fd6-168">キーワードで、ステートメントの後にこと`On`または`Off`、どちらも指定されている場合、既定値は`On`します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-168">The statement may be followed by the keywords `On` or `Off`; if neither is specified, the default is `On`.</span></span> <span data-ttu-id="08fd6-169">ファイル内のステートメントが指定されていない場合は、どちらを使用するコンパイル環境が決定します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-169">If no statement is specified in a file, the compilation environment determines which will be used.</span></span>

```antlr
OptionInferStatement
    : 'Option' 'Infer' OnOff? StatementTerminator
    ;
```

<span data-ttu-id="08fd6-170">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="08fd6-170">__Note.__</span></span> <span data-ttu-id="08fd6-171">`Infer` `Off`予約された単語は使用されません。</span><span class="sxs-lookup"><span data-stu-id="08fd6-171">`Infer` and `Off` are not reserved words.</span></span>

```vb
Option Infer On

Module Test
    Sub Main()
        ' The type of x is Integer
        Dim x = 10

        ' The type of y is String
        Dim y = "abc"
    End Sub
End Module
```


## <a name="imports-statement"></a><span data-ttu-id="08fd6-172">Imports ステートメント</span><span class="sxs-lookup"><span data-stu-id="08fd6-172">Imports Statement</span></span>

<span data-ttu-id="08fd6-173">`Imports` ステートメントは、名の修飾せずに参照を使用できるように、ソース ファイルにエンティティの名前をインポートまたは XML の式で使用する名前空間をインポートします。</span><span class="sxs-lookup"><span data-stu-id="08fd6-173">`Imports` statements import the names of entities into a source file, allowing the names to be referenced without qualification, or import a namespace for use in XML expressions.</span></span>

```antlr
ImportsStatement
    : 'Imports' ImportsClauses StatementTerminator
    ;

ImportsClauses
    : ImportsClause ( Comma ImportsClause )*
    ;

ImportsClause
    : AliasImportsClause
    | MembersImportsClause
    | XMLNamespaceImportsClause
    ;
```

<span data-ttu-id="08fd6-174">メンバー宣言を含むソース ファイル内で、`Imports`次の例に示すようにステートメントでは、指定した名前空間に含まれる型を直接参照できます。</span><span class="sxs-lookup"><span data-stu-id="08fd6-174">Within member declarations in a source file that contains an `Imports` statement, the types contained in the given namespace can be referenced directly, as seen in the following example:</span></span>

```vb
Imports N1.N2

Namespace N1.N2
    Class A
    End Class
End Namespace 

Namespace N3
    Class B
        Inherits A
    End Class
End Namespace
```

<span data-ttu-id="08fd6-175">ここでは、ソース ファイル内で、名前空間の型のメンバー`N1.N2`直接利用し、したがってクラス`N3.B`クラスから派生した`N1.N2.A`します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-175">Here, within the source file, the type members of namespace `N1.N2` are directly available, and thus class `N3.B` derives from class `N1.N2.A`.</span></span>

<span data-ttu-id="08fd6-176">`Imports` 後のステートメントを表示する必要があります`Option`ステートメントが任意の前に宣言を入力します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-176">`Imports` statements must appear after any `Option` statements but before any type declarations.</span></span> <span data-ttu-id="08fd6-177">コンパイル環境が暗黙的な定義も`Imports`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="08fd6-177">The compilation environment may also define implicit `Imports` statements.</span></span>

<span data-ttu-id="08fd6-178">`Imports` ステートメントは、ソース ファイルで名前を使用できるようにしますが、グローバル名前空間の宣言領域内のあらゆるものを宣言しません。</span><span class="sxs-lookup"><span data-stu-id="08fd6-178">`Imports` statements make names available in a source file, but do not declare anything in the global namespace's declaration space.</span></span> <span data-ttu-id="08fd6-179">インポートされた名前のスコープ、`Imports`ステートメントは、ソース ファイルに含まれる名前空間のメンバーの宣言に拡張します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-179">The scope of the names imported by an `Imports` statement extends over the namespace member declarations contained in the source file.</span></span> <span data-ttu-id="08fd6-180">スコープ、`Imports`ステートメント具体的にが含まれない他`Imports`ステートメント、およびその他のソース ファイルが含まします。</span><span class="sxs-lookup"><span data-stu-id="08fd6-180">The scope of an `Imports` statement specifically does not include other `Imports` statements, nor does it include other source files.</span></span> <span data-ttu-id="08fd6-181">`Imports` ステートメントは、互いには参照できません。</span><span class="sxs-lookup"><span data-stu-id="08fd6-181">`Imports` statements may not refer to one another.</span></span>

<span data-ttu-id="08fd6-182">この例では、最後に`Imports`最初のインポート エイリアスに影響を受けないために、ステートメントがエラーでは。</span><span class="sxs-lookup"><span data-stu-id="08fd6-182">In this example, the last `Imports` statement is in error because it is not affected by the first import alias.</span></span>

```vb
Imports R1 = N1 ' OK.
Imports R2 = N1.N2 ' OK.
Imports R3 = R1.N2 ' Error: Can't refer to R1.

Namespace N1.N2
End Namespace
```

<span data-ttu-id="08fd6-183">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="08fd6-183">__Note.__</span></span> <span data-ttu-id="08fd6-184">表示される名前空間または型名`Imports`完全修飾している場合、ステートメントが常に扱われます。</span><span class="sxs-lookup"><span data-stu-id="08fd6-184">The namespace or type names that appear in `Imports` statements are always treated as if they are fully qualified.</span></span> <span data-ttu-id="08fd6-185">名前空間または型名の一番左の識別子を常にグローバル名前空間に解決し、解像度の残りの部分は、通常の名前解決ルールに従って実行されます。</span><span class="sxs-lookup"><span data-stu-id="08fd6-185">That is, the leftmost identifier in a namespace or type name always resolves in the global namespace and the rest of the resolution proceeds according to normal name resolution rules.</span></span> <span data-ttu-id="08fd6-186">これは、このようなルールが適用される言語で唯一の場所この規則により、名前を完全に修飾から隠さことはできません。</span><span class="sxs-lookup"><span data-stu-id="08fd6-186">This is the only place in the language that applies such a rule; the rule ensures that a name cannot be completely hidden from qualification.</span></span> <span data-ttu-id="08fd6-187">規則がない場合は、グローバル名前空間の名前が特定のソース ファイルで非表示にできなくなる修飾の方法でその名前空間から任意の名前を指定することです。</span><span class="sxs-lookup"><span data-stu-id="08fd6-187">Without the rule, if a name in the global namespace were hidden in a particular source file, it would be impossible to specify any names from that namespace in a qualified way.</span></span>

<span data-ttu-id="08fd6-188">この例で、`Imports`ステートメントは常には、グローバルに参照`System`名前空間、およびソース ファイル内のクラスではなく。</span><span class="sxs-lookup"><span data-stu-id="08fd6-188">In this example, the `Imports` statement always refers to the global `System` namespace, and not the class in the source file.</span></span>

```vb
Imports System   ' Imports the namespace, not the class.

Class System
End Class
```


### <a name="import-aliases"></a><span data-ttu-id="08fd6-189">インポート エイリアス</span><span class="sxs-lookup"><span data-stu-id="08fd6-189">Import Aliases</span></span>

<span data-ttu-id="08fd6-190">*インポート エイリアス*名前空間または型のエイリアスを定義します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-190">An *import alias* defines an alias for a namespace or type.</span></span>

```antlr
AliasImportsClause
    : Identifier Equals TypeName
    ;
```

```vb
Imports A = N1.N2.A

Namespace N1.N2
    Class A
    End Class
End Namespace

Namespace N3
    Class B
        Inherits A
    End Class
End Namespace
```

<span data-ttu-id="08fd6-191">ここでは、ソース ファイル内で、`A`の別名です`N1.N2.A`、クラスと`N3.B`クラスから派生した`N1.N2.A`します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-191">Here, within the source file, `A` is an alias for `N1.N2.A`, and thus class `N3.B` derives from class `N1.N2.A`.</span></span> <span data-ttu-id="08fd6-192">別名を作成して、同じ効果を取得できる`R`の`N1.N2`し、参照する`R.A`:</span><span class="sxs-lookup"><span data-stu-id="08fd6-192">The same effect can be obtained by creating an alias `R` for `N1.N2` and then referencing `R.A`:</span></span>

```vb
Imports R = N1.N2

Namespace N3
    Class B
        Inherits R.A
    End Class
End Namespace
```

<span data-ttu-id="08fd6-193">インポート エイリアスの識別子は、グローバル名前空間 (だけでなく、グローバル名前空間宣言で、そのインポート エイリアスが定義されているソース ファイル) の宣言領域内で一意である必要があります、グローバル名前空間の名前で宣言されていない場合でも宣言領域です。</span><span class="sxs-lookup"><span data-stu-id="08fd6-193">The identifier of an import alias must be unique within the declaration space of the global namespace (not just the global namespace declaration in the source file in which the import alias is defined), even though it does not declare a name in the global namespace's declaration space.</span></span>

<span data-ttu-id="08fd6-194">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="08fd6-194">__Note.__</span></span> <span data-ttu-id="08fd6-195">モジュール内の宣言は、コンテナーの宣言領域に名前を持ち込んでいません。</span><span class="sxs-lookup"><span data-stu-id="08fd6-195">Declarations in a module do not introduce names into the containing declaration space.</span></span> <span data-ttu-id="08fd6-196">、つまりインポート エイリアスと同じ名前を指定するモジュールでの宣言に有効な場合でも、宣言の名前がコンテナーの宣言領域にアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="08fd6-196">Thus, it is valid for a declaration in a module to have the same name as an import alias, even though the declaration's name will be accessible in the containing declaration space.</span></span>

```vb
' Error: Alias A conflicts with typename A
Imports A = N3.A

Class A
End Class

Namespace N3
    Class A
    End Class
End Namespace
```

<span data-ttu-id="08fd6-197">メンバーが既にグローバル名前空間に含まれているここでは、`A`ので、その識別子を使用するインポート エイリアスがエラーになります。</span><span class="sxs-lookup"><span data-stu-id="08fd6-197">Here, the global namespace already contains a member `A`, so it is an error for an import alias to use that identifier.</span></span> <span data-ttu-id="08fd6-198">同じ名前のエイリアスを宣言する、同じソース ファイルのインポート エイリアスを 2 つ以上のエラーでは同様にします。</span><span class="sxs-lookup"><span data-stu-id="08fd6-198">It is likewise an error for two or more import aliases in the same source file to declare aliases by the same name.</span></span>

<span data-ttu-id="08fd6-199">インポート エイリアスは、任意の名前空間または型の別名を作成できます。</span><span class="sxs-lookup"><span data-stu-id="08fd6-199">An import alias can create an alias for any namespace or type.</span></span> <span data-ttu-id="08fd6-200">名前空間または型にエイリアスを使ってアクセスするには、名前空間または型をその宣言された名前を介してアクセスするとまったく同じ結果が得られます。</span><span class="sxs-lookup"><span data-stu-id="08fd6-200">Accessing a namespace or type through an alias yields exactly the same result as accessing the namespace or type through its declared name.</span></span>

```vb
Imports R1 = N1
Imports R2 = N1.N2

Namespace N1.N2
    Class A
    End Class
End Namespace

Namespace N3
    Class B
        Private a As N1.N2.A
        Private b As R1.N2.A
        Private c As R2.A
    End Class
End Namespace
```

<span data-ttu-id="08fd6-201">ここでは、名前`N1.N2.A`、 `R1.N2.A`、および`R2.A`の完全修飾名がクラスには等価であり、すべて参照`N1.N2.A`します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-201">Here, the names `N1.N2.A`, `R1.N2.A`, and `R2.A` are equivalent, and all refer to the class whose fully qualified name is `N1.N2.A`.</span></span>

<span data-ttu-id="08fd6-202">インポートには、名前空間または別名を作成する型の正確な名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-202">The import specifies the exact name of the namespace or type to which it is creating an alias.</span></span> <span data-ttu-id="08fd6-203">その名前空間または型の正確な完全修飾名があります: (インスタンスの派生クラスから基底クラスのメンバーへのアクセスを許可する) を修飾名前解決の通常の規則を使用しません。</span><span class="sxs-lookup"><span data-stu-id="08fd6-203">This must be the exact fully qualified name of that namespace or type: it does not use the normal rules for qualified name resolution (which for instance allow access to the members of a base class through a derived class).</span></span>

<span data-ttu-id="08fd6-204">型またはこれらのルールでは解決できない名前空間にインポート エイリアス ポイントし、import ステートメントは無視されます (または、コンパイラは警告)。</span><span class="sxs-lookup"><span data-stu-id="08fd6-204">If an import alias points to a type or namespace which cannot be resolved by these rules, then the import statement is ignored (and the compiler gives a warning).</span></span>

<span data-ttu-id="08fd6-205">また、参照がオープン ジェネリック型にすることはできません--すべてのジェネリック型は指定するには、有効な型引数が必要し、すべての型引数は、上記のルールによって解決する必要があります。</span><span class="sxs-lookup"><span data-stu-id="08fd6-205">Also, the reference cannot be to an open generic type -- all generic types must have valid type arguments supplied, and all type arguments must be resolvable by the rules above.</span></span> <span data-ttu-id="08fd6-206">ジェネリック型の任意の不正なバインディングでは、エラーになります。</span><span class="sxs-lookup"><span data-stu-id="08fd6-206">Any incorrect binding of a generic type is an error.</span></span>

```vb
Imports A = G              ' error: since G is an open generic type
Imports B = G(Of Integer)  ' okay
Imports C = Derived.Nested ' warning: Derived.Nested isn't itself a type
Imports D = G(Of Derived.Nested) ' error: Derived.Nested isn't found

Class G(Of T) : End Class

Class Base
    Class Nested : End Class
End Class

Class Derived : Inherits Base
End Class

Module Module1
    Sub Main()
        Dim x As C               ' error: "C" wasn't succesfully defined
        Dim y As Derived.Nested  ' okay
    End Sub
End Module
```

<span data-ttu-id="08fd6-207">ソース ファイル内の宣言は、インポート エイリアス名をシャドウすることができます。</span><span class="sxs-lookup"><span data-stu-id="08fd6-207">Declarations in the source file may shadow the import alias name.</span></span>

```vb
Imports R = N1.N2

Namespace N1.N2
    Class A
    End Class
End Namespace

Namespace N3
    Class R
    End Class

    Class B
        Inherits R.A  ' Error, R has no member A
    End Class
End Namespace
```

<span data-ttu-id="08fd6-208">前の例の参照を`R.A`の宣言で`B`ので、エラーが発生`R`を指す`N3.R`ではなく、`N1.N2`します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-208">In the preceding example the reference to `R.A` in the declaration of `B` causes an error because `R` refers to `N3.R`, not `N1.N2`.</span></span>

<span data-ttu-id="08fd6-209">インポート エイリアスは、特定のソース ファイル内で使用可能なエイリアスが、基になる宣言領域に新しいメンバーは含まれません。</span><span class="sxs-lookup"><span data-stu-id="08fd6-209">An import alias makes an alias available within a particular source file, but it does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="08fd6-210">つまり、インポート エイリアスは、推移的ではありませんではなくが発生するソース ファイルのみに影響します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-210">In other words, an import alias is not transitive, but rather affects only the source file in which it occurs.</span></span>

<span data-ttu-id="08fd6-211">file1.vb:</span><span class="sxs-lookup"><span data-stu-id="08fd6-211">File1.vb:</span></span>

```vb
Imports R = N1.N2

Namespace N1.N2
    Class A
    End Class
End Namespace
```

<span data-ttu-id="08fd6-212">file2.vb:</span><span class="sxs-lookup"><span data-stu-id="08fd6-212">File2.vb:</span></span>

```vb
Class B
    Inherits R.A ' Error, R unknown.
End Class
```

<span data-ttu-id="08fd6-213">上記の例では、インポート エイリアスのスコープを導入するため、`R`が含まれるソース ファイル内の宣言にのみ拡張`R`2 番目のソース ファイルでは不明です。</span><span class="sxs-lookup"><span data-stu-id="08fd6-213">In the above example, because the scope of the import alias that introduces `R` only extends to declarations in the source file in which it is contained, `R` is unknown in the second source file.</span></span>


### <a name="namespace-imports"></a><span data-ttu-id="08fd6-214">名前空間のインポート</span><span class="sxs-lookup"><span data-stu-id="08fd6-214">Namespace Imports</span></span>

<span data-ttu-id="08fd6-215">A*名前空間インポート*名前空間または修飾なしで使用される型の各メンバーの識別子を許可する名前空間または型のメンバーのすべてをインポートします。</span><span class="sxs-lookup"><span data-stu-id="08fd6-215">A *namespace import* imports all of the members of a namespace or type, allowing the identifier of each member of the namespace or type to be used without qualification.</span></span> <span data-ttu-id="08fd6-216">型の場合、名前空間のインポートはのみ、クラス名の修飾を必要とせず、型の共有メンバーへのアクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-216">In the case of types, a namespace import only allows access to the shared members of the type without requiring qualification of the class name.</span></span> <span data-ttu-id="08fd6-217">具体的には、修飾なしで使用される列挙型のメンバーを使用します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-217">In particular, it allows the members of enumerated types to be used without qualification.</span></span>

```antlr
MembersImportsClause
    : TypeName
    ;
```

<span data-ttu-id="08fd6-218">例えば:</span><span class="sxs-lookup"><span data-stu-id="08fd6-218">For example:</span></span>

```vb
Imports Colors

Enum Colors
    Red
    Green
    Blue
End Enum

Module M1
    Sub Main()
        Dim c As Colors = Red
    End Sub
End Module
```

<span data-ttu-id="08fd6-219">インポート エイリアスとは異なり、名前空間インポート制限はありませんをインポートし、名前空間と型を持つ識別子は、グローバル名前空間内で既に宣言されてインポートが名前にします。</span><span class="sxs-lookup"><span data-stu-id="08fd6-219">Unlike an import alias, a namespace import has no restrictions on the names it imports and may import namespaces and types whose identifiers are already declared within the global namespace.</span></span> <span data-ttu-id="08fd6-220">インポート エイリアスとソース ファイル内の宣言では、通常のインポートでインポートされた名前が影付き。</span><span class="sxs-lookup"><span data-stu-id="08fd6-220">The names imported by a regular import are shadowed by import aliases and declarations in the source file.</span></span>

<span data-ttu-id="08fd6-221">次の例では、`A`を指す`N3.A`なく`N1.N2.A`メンバー宣言内で、`N3`名前空間。</span><span class="sxs-lookup"><span data-stu-id="08fd6-221">In the following example, `A` refers to `N3.A` rather than `N1.N2.A` within member declarations in the `N3` namespace.</span></span>

```vb
Imports N1.N2

Namespace N1.N2
    Class A
    End Class
End Namespace

Namespace N3
    Class A
    End Class

    Class B
        Inherits A
    End Class
End Namespace
```

<span data-ttu-id="08fd6-222">1 つ以上のインポートされた名前空間には、同じ名前でメンバーが含まれます (とその名前がそれ以外の場合、インポート エイリアスまたは宣言でシャドウ)、その名前への参照はあいまいであり、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-222">When more than one imported namespace contains members by the same name (and that name is not otherwise shadowed by an import alias or declaration), a reference to that name is ambiguous and causes a compile-time error.</span></span>

```vb
Imports N1
Imports N2

Namespace N1
    Class A
    End Class
End Namespace 

Namespace N2
    Class A
    End Class
End Namespace 

Namespace N3
    Class B
        Inherits A ' Error, A is ambiguous.
    End Class
End Namespace
```

<span data-ttu-id="08fd6-223">上記の例では、どちらも`N1`と`N2`、メンバーを含んで`A`します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-223">In the above example, both `N1` and `N2` contain a member `A`.</span></span> <span data-ttu-id="08fd6-224">`N3`を参照する、両方をインポート`A`で`N3`コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-224">Because `N3` imports both, referencing `A` in `N3` causes a compile-time error.</span></span> <span data-ttu-id="08fd6-225">このような状況で、競合はへの参照の修飾を使用するか解決できる`A`、または特定を取得するインポート エイリアスを導入することで`A`、次の例。</span><span class="sxs-lookup"><span data-stu-id="08fd6-225">In this situation, the conflict can be resolved either through qualification of references to `A`, or by introducing an import alias that picks a particular `A`, as in the following example:</span></span>

```vb
Imports N1
Imports N2
Imports A = N1.A

Namespace N3
    Class B
        Inherits A ' A means N1.A.
    End Class
End Namespace
```

<span data-ttu-id="08fd6-226">のみ名前空間、クラス、構造体、列挙型、および標準的なモジュールをインポートすることがあります。</span><span class="sxs-lookup"><span data-stu-id="08fd6-226">Only namespaces, classes, structures, enumerated types, and standard modules may be imported.</span></span>


### <a name="xml-namespace-imports"></a><span data-ttu-id="08fd6-227">XML Namespace のインポート</span><span class="sxs-lookup"><span data-stu-id="08fd6-227">XML Namespace Imports</span></span>

<span data-ttu-id="08fd6-228">*XML 名前空間インポート*名前空間または修飾されていない XML 表現がコンパイル単位内に含まれる既定の名前空間を定義します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-228">An *XML namespace import* defines a namespace or the default namespace for unqualified XML expressions contained within the compilation unit.</span></span>

```antlr
XMLNamespaceImportsClause
    : '<' XMLNamespaceAttributeName XMLWhitespace? Equals XMLWhitespace?
      XMLNamespaceValue '>'
    ;

XMLNamespaceValue
    : DoubleQuoteCharacter XMLAttributeDoubleQuoteValueCharacter* DoubleQuoteCharacter
    | SingleQuoteCharacter XMLAttributeSingleQuoteValueCharacter* SingleQuoteCharacter
    ;
```

<span data-ttu-id="08fd6-229">例えば:</span><span class="sxs-lookup"><span data-stu-id="08fd6-229">For example:</span></span>

```vb
Imports <xmlns:db="http://example.org/database">

Module Test
    Sub Main()
        ' db namespace is "http://example.org/database"
        Dim x = <db:customer><db:Name>Bob</></>

        Console.WriteLine(x.<db:Name>)
    End Sub
End Module
```

<span data-ttu-id="08fd6-230">既定の名前空間を含む、XML 名前空間は、インポートの特定のセットに対して 1 回だけ定義できます。</span><span class="sxs-lookup"><span data-stu-id="08fd6-230">An XML namespace, including the default namespace, can only be defined once for a particular set of imports.</span></span> <span data-ttu-id="08fd6-231">例えば:</span><span class="sxs-lookup"><span data-stu-id="08fd6-231">For example:</span></span>

```vb
Imports <xmlns:db="http://example.org/database-one">
' Error: namespace db is already defined
Imports <xmlns:db="http://example.org/database-two">
```


## <a name="namespaces"></a><span data-ttu-id="08fd6-232">名前空間</span><span class="sxs-lookup"><span data-stu-id="08fd6-232">Namespaces</span></span>

<span data-ttu-id="08fd6-233">Visual Basic プログラムの名前空間を使用して構成します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-233">Visual Basic programs are organized using namespaces.</span></span> <span data-ttu-id="08fd6-234">名前空間両方内部的には、プログラムを整理するだけでなくプログラム要素が他のプログラムを公開する方法を整理します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-234">Namespaces both internally organize a program as well as organize the way program elements are exposed to other programs.</span></span>

<span data-ttu-id="08fd6-235">他のエンティティとは異なり、名前空間は終わりし、同じプログラム内および同じ名前空間のメンバーの貢献をしている各宣言に、多くのプログラム間で複数回を宣言すること。</span><span class="sxs-lookup"><span data-stu-id="08fd6-235">Unlike other entities, namespaces are open-ended, and may be declared multiple times within the same program and across many programs, with each declaration contributing members to the same namespace.</span></span> <span data-ttu-id="08fd6-236">次の例では、2 つの名前空間宣言が完全修飾の名前を持つ 2 つのクラスを宣言する、同じ宣言領域に貢献`N1.N2.A`と`N1.N2.B`します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-236">In the following example, the two namespace declarations contribute to the same declaration space, declaring two classes with the fully qualified names `N1.N2.A` and `N1.N2.B`.</span></span>

```vb
Namespace N1.N2
    Class A
    End Class
End Namespace

Namespace N1.N2
    Class B
    End Class
End Namespace
```

<span data-ttu-id="08fd6-237">2 つの宣言は、同じ宣言領域に追加、ため、各エラーには、同じ名前のメンバーの宣言が含まれているでしょう。</span><span class="sxs-lookup"><span data-stu-id="08fd6-237">Because the two declarations contribute to the same declaration space, it would be an error if each contained a declaration of a member with the same name.</span></span>

<span data-ttu-id="08fd6-238">名なしおよびが入れ子になった名前空間と型は常に修飾なしでアクセスを持つグローバル名前空間があります。</span><span class="sxs-lookup"><span data-stu-id="08fd6-238">There is a global namespace that has no name and whose nested namespaces and types can always be accessed without qualification.</span></span> <span data-ttu-id="08fd6-239">グローバル名前空間で宣言された名前空間のメンバーのスコープは、プログラム全体のテキストです。</span><span class="sxs-lookup"><span data-stu-id="08fd6-239">The scope of a namespace member declared in the global namespace is the entire program text.</span></span> <span data-ttu-id="08fd6-240">それ以外の場合、型または名前空間のスコープの完全修飾名は名前空間で宣言されている`N`が対応する名前空間の完全修飾名の先頭の各名前空間のプログラム テキスト`N`または`N`自体。</span><span class="sxs-lookup"><span data-stu-id="08fd6-240">Otherwise, the scope of a type or namespace declared in a namespace whose fully qualified name is `N` is the program text of each namespace whose corresponding namespace's fully qualified name begins with `N` or is `N` itself.</span></span> <span data-ttu-id="08fd6-241">(既定では、特定の名前空間の宣言を配置する、コンパイラが選択できることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="08fd6-241">(Note that a compiler can choose to put declarations in a particular namespace by default.</span></span> <span data-ttu-id="08fd6-242">これは変更されません、名前のないグローバル名前空間がまだ残っているという事実です。)</span><span class="sxs-lookup"><span data-stu-id="08fd6-242">This does not alter the fact that there is still a global, unnamed namespace.)</span></span>

<span data-ttu-id="08fd6-243">この例では、クラスで`B`クラスをご覧`A`ため`B`の名前空間`N1.N2.N3`は概念的には、名前空間内で入れ子になった`N1.N2`します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-243">In this example, the class `B` can see the class `A` because `B`'s namespace `N1.N2.N3` is conceptually nested within the namespace `N1.N2`.</span></span>

```vb
Namespace N1.N2
    Class A
    End Class
End Namespace

Namespace N1.N2.N3
    Class B
        Inherits A
    End Class
End Namespace
```

### <a name="namespace-declarations"></a><span data-ttu-id="08fd6-244">名前空間の宣言</span><span class="sxs-lookup"><span data-stu-id="08fd6-244">Namespace Declarations</span></span>

<span data-ttu-id="08fd6-245">名前空間宣言の 3 つの形式はあります。</span><span class="sxs-lookup"><span data-stu-id="08fd6-245">There are three forms of namespace declaration.</span></span>

```antlr
NamespaceDeclaration
    : 'Namespace' NamespaceName StatementTerminator
      NamespaceMemberDeclaration*
      'End' 'Namespace' StatementTerminator
    ;

NamespaceName
    : RelativeNamespaceName
    | 'Global'
    | 'Global' '.' RelativeNamespaceName
    ;

RelativeNamespaceName
    : Identifier ( Period IdentifierOrKeyword )*
    ;
```

<span data-ttu-id="08fd6-246">キーワードから始まり、最初のフォーム`Namespace`に続けて相対名前空間の名前。</span><span class="sxs-lookup"><span data-stu-id="08fd6-246">The first form starts with the keyword `Namespace` followed by a relative namespace name.</span></span> <span data-ttu-id="08fd6-247">空間の相対名が修飾されている場合は、構文的修飾名では、各名前に対応する名前空間宣言内で入れ子になった場合、名前空間宣言が扱われます。</span><span class="sxs-lookup"><span data-stu-id="08fd6-247">If the relative namespace name is qualified, the namespace declaration is treated as if it is lexically nested within namespace declarations corresponding to each name in the qualified name.</span></span> <span data-ttu-id="08fd6-248">たとえば、次の 2 つの名前空間は意味的に同等です。</span><span class="sxs-lookup"><span data-stu-id="08fd6-248">For example, the following two namespaces are semantically equivalent:</span></span>

```vb
Namespace N1.N2
    Class A
    End Class

    Class B
    End Class
End Namespace 

Namespace N1
    Namespace N2
        Class A
        End Class

        Class B
        End Class
    End Namespace
End Namespace
```

<span data-ttu-id="08fd6-249">2 番目の形式は、キーワードで始まる`Namespace Global`します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-249">The second form starts with the keywords `Namespace Global`.</span></span> <span data-ttu-id="08fd6-250">すべてのメンバー宣言は、グローバル名前なし名前空間 - コンパイル環境で提供されるすべての既定値に関係なくで構文的配置された場合、ように扱われます。</span><span class="sxs-lookup"><span data-stu-id="08fd6-250">It is treated as if all its member declarations were lexically placed in the global unnamed namespace -- regardless of any defaults provided by the compilation environment.</span></span> <span data-ttu-id="08fd6-251">その他の任意の名前空間宣言内でこの形式の名前空間宣言が構文的入れ子にできません可能性があります。</span><span class="sxs-lookup"><span data-stu-id="08fd6-251">This form of namespace declaration may not be lexically nested within any other namespace declaration.</span></span>

<span data-ttu-id="08fd6-252">3 番目の形式がキーワードで始まる`Namespace Global`修飾された識別子に続けて`N`します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-252">The third form starts with the keywords `Namespace Global` followed by a qualified identifier `N`.</span></span> <span data-ttu-id="08fd6-253">最初のフォームの名前空間宣言の場合と同様に扱われます"`Namespace N`"、グローバル名前なし名前空間 - コンパイル環境で提供されるすべての既定値に関係なくに格納された構文です。</span><span class="sxs-lookup"><span data-stu-id="08fd6-253">It is treated as if it were a namespace declaration of the first form "`Namespace N`" that was lexically placed in the global unnamed namespace -- regardless of any defaults provided by the compilation environment.</span></span> <span data-ttu-id="08fd6-254">その他の任意の名前空間宣言内でこの形式の名前空間宣言が構文的入れ子にできません可能性があります。</span><span class="sxs-lookup"><span data-stu-id="08fd6-254">This form of namespace declaration may not be lexically nested within any other namespace declaration.</span></span>

```vb
Namespace Global       ' Puts N1.A in the global namespace
    Namespace N1
        Class A
        End Class
    End Namespace
End Namespace

Namespace Global.N1    ' Equivalent to the above
    Class A
    End Class
End Namespace

Namespace N1           ' May or may not be equivalent to the above,
    Class A            ' depending on defaults provided by the
    End Class          ' compilation environment
End Namespace
```

<span data-ttu-id="08fd6-255">名前空間のメンバーを扱う場合、重要ではありません、特定のメンバーが宣言されています。</span><span class="sxs-lookup"><span data-stu-id="08fd6-255">When dealing with the members of a namespace, it is not important where a particular member is declared.</span></span> <span data-ttu-id="08fd6-256">2 つのプログラムは、同じ名前空間で同じ名前のエンティティを定義する名前空間名を解決しようとすると、あいまいさエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="08fd6-256">If two programs define an entity with the same name in the same namespace, attempting to resolve the name in the namespace causes an ambiguity error.</span></span>

<span data-ttu-id="08fd6-257">名前空間は本質的に`Public`ので、名前空間の宣言は、アクセス修飾子を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="08fd6-257">Namespaces are by definition `Public`, so a namespace declaration cannot include any access modifiers.</span></span>


### <a name="namespace-members"></a><span data-ttu-id="08fd6-258">Namespace メンバー</span><span class="sxs-lookup"><span data-stu-id="08fd6-258">Namespace Members</span></span>

<span data-ttu-id="08fd6-259">Namespace メンバーは名前空間宣言をして、宣言の型のみです。</span><span class="sxs-lookup"><span data-stu-id="08fd6-259">Namespace members can only be namespace declarations and type declarations.</span></span> <span data-ttu-id="08fd6-260">型宣言があります`Public`または`Friend`アクセスします。</span><span class="sxs-lookup"><span data-stu-id="08fd6-260">Type declarations may have `Public` or `Friend` access.</span></span> <span data-ttu-id="08fd6-261">型の既定のアクセスは`Friend`アクセスします。</span><span class="sxs-lookup"><span data-stu-id="08fd6-261">The default access for types is `Friend` access.</span></span>

```antlr
NamespaceMemberDeclaration
    : NamespaceDeclaration
    | TypeDeclaration
    ;

TypeDeclaration
    : ModuleDeclaration
    | NonModuleDeclaration
    ;

NonModuleDeclaration
    : EnumDeclaration
    | StructureDeclaration
    | InterfaceDeclaration
    | ClassDeclaration
    | DelegateDeclaration
    ;
```
