# <a name="lexical-grammar"></a><span data-ttu-id="a862d-101">字句文法</span><span class="sxs-lookup"><span data-stu-id="a862d-101">Lexical Grammar</span></span>

<span data-ttu-id="a862d-102">Visual Basic プログラムのコンパイルでは、最初、レキシカル トークンの順序付けされたセットに Unicode 文字の生のストリームを変換する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a862d-102">Compilation of a Visual Basic program first involves translating the raw stream of Unicode characters into an ordered set of lexical tokens.</span></span> <span data-ttu-id="a862d-103">Visual Basic 言語は自由な形式ではないため、トークンのセットが一連の論理行にさらに分割します。</span><span class="sxs-lookup"><span data-stu-id="a862d-103">Because the Visual Basic language is not free-format, the set of tokens is then further divided into a series of logical lines.</span></span> <span data-ttu-id="a862d-104">A*論理行*ストリームの末尾には、行連結文字または修飾されていない次の行終端記号をストリームの開始または行の終端からのスパン。</span><span class="sxs-lookup"><span data-stu-id="a862d-104">A *logical line* spans from either the start of the stream or a line terminator through to the next line terminator that is not preceded by a line continuation or through to the end of the stream.</span></span>

<span data-ttu-id="a862d-105">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="a862d-105">__Note.__</span></span> <span data-ttu-id="a862d-106">言語のバージョン 9.0 での XML リテラル式の導入に伴い、Visual Basic はなくなりました個別字句文法という意味で Visual Basic では構文のコンテキストに関係なくコードをトークン化することができます。</span><span class="sxs-lookup"><span data-stu-id="a862d-106">With the introduction of XML literal expressions in version 9.0 of the language, Visual Basic no longer has a distinct lexical grammar in the sense that Visual Basic code can be tokenized without regard to the syntactic context.</span></span> <span data-ttu-id="a862d-107">これは、原因という事実に XML と Visual Basic のさまざまな語彙の規則があるし、特定の時点に使用中の語彙の規則のセットがその時点でどのような構文構造が処理されている異なります。</span><span class="sxs-lookup"><span data-stu-id="a862d-107">This is due to the fact that XML and Visual Basic have different lexical rules and the set of lexical rules in use at any particular time depends on what syntactic construct is being processed at that moment.</span></span> <span data-ttu-id="a862d-108">この仕様では、標準の Visual Basic コードの語彙の規則のガイドとしてこの字句文法セクションが保持されます。</span><span class="sxs-lookup"><span data-stu-id="a862d-108">This specification retains this lexical grammar section as a guide to the lexical rules of regular Visual Basic code.</span></span>

```antlr
LogicalLineStart
    : LogicalLine*
    ;

LogicalLine
    : LogicalLineElement* Comment? LineTerminator
    ;

LogicalLineElement
    : WhiteSpace
    | LineContinuation
    | Token
    ;

Token
    : Identifier
    | Keyword
    | Literal
    | Separator
    | Operator
    ;
```

## <a name="characters-and-lines"></a><span data-ttu-id="a862d-109">文字と行</span><span class="sxs-lookup"><span data-stu-id="a862d-109">Characters and Lines</span></span>

<span data-ttu-id="a862d-110">Visual Basic プログラムは、Unicode 文字のセットの文字で構成されます。</span><span class="sxs-lookup"><span data-stu-id="a862d-110">Visual Basic programs are composed of characters from the Unicode character set.</span></span>

```antlr
Character:
    '<Any Unicode character except a LineTerminator>'
    ;
```

### <a name="line-terminators"></a><span data-ttu-id="a862d-111">行ターミネータ</span><span class="sxs-lookup"><span data-stu-id="a862d-111">Line Terminators</span></span>

<span data-ttu-id="a862d-112">Unicode 行区切り文字は、論理的な行を区切ります。</span><span class="sxs-lookup"><span data-stu-id="a862d-112">Unicode line break characters separate logical lines.</span></span>

```antlr
LineTerminator
    : '<Unicode 0x00D>'
    | '<Unicode 0x00A>'
    | '<CR>'
    | '<LF>'
    | '<Unicode 0x2028>'
    | '<Unicode 0x2029>'
    ;
```

### <a name="line-continuation"></a><span data-ttu-id="a862d-113">行の連結</span><span class="sxs-lookup"><span data-stu-id="a862d-113">Line Continuation</span></span>

<span data-ttu-id="a862d-114">A*行継続*(空白) を除くテキスト行の最後の文字として 1 つのアンダー スコア文字の直前にある少なくとも 1 つの空白文字で構成されます。</span><span class="sxs-lookup"><span data-stu-id="a862d-114">A *line continuation* consists of at least one white-space character that immediately precedes a single underscore character as the last character (other than white space) in a text line.</span></span> <span data-ttu-id="a862d-115">行の連結は、論理行にまたがる 1 つ以上の物理行を使用できます。</span><span class="sxs-lookup"><span data-stu-id="a862d-115">A line continuation allows a logical line to span more than one physical line.</span></span> <span data-ttu-id="a862d-116">行連結文字として扱われます、空白がない場合でもです。</span><span class="sxs-lookup"><span data-stu-id="a862d-116">Line continuations are treated as if they were white space, even though they are not.</span></span>

```antlr
LineContinuation
    : WhiteSpace '_' WhiteSpace* LineTerminator
    ;
```

<span data-ttu-id="a862d-117">次のプログラムは、いくつかの行連結文字を示しています。</span><span class="sxs-lookup"><span data-stu-id="a862d-117">The following program shows some line continuations:</span></span>

```vb
Module Test
    Sub Print( _
        Param1 As Integer, _
        Param2 As Integer )

        If (Param1 < Param2) Or _
            (Param1 > Param2) Then
            Console.WriteLine("Not equal")
        End If
    End Function
End Module
```

<span data-ttu-id="a862d-118">いくつかの場所の構文文法では、*暗黙的な行継続*します。</span><span class="sxs-lookup"><span data-stu-id="a862d-118">Some places in the syntactic grammar allow for *implicit line continuations*.</span></span> <span data-ttu-id="a862d-119">行終端記号が発生したとき</span><span class="sxs-lookup"><span data-stu-id="a862d-119">When a line terminator is encountered</span></span>

* <span data-ttu-id="a862d-120">コンマの後に (`,`) 開きかっこ (`(`)、中かっこを開きます (`{`)、または埋め込み式を開く (`<%=`)</span><span class="sxs-lookup"><span data-stu-id="a862d-120">after a comma (`,`), open parenthesis (`(`), open curly brace (`{`), or open embedded expression (`<%=`)</span></span>

* <span data-ttu-id="a862d-121">メンバー修飾子の後 (`.`または`.@`または`...`)、その何かが修飾されている (つまりが使用されていない暗黙的な`With`コンテキスト)</span><span class="sxs-lookup"><span data-stu-id="a862d-121">after a member qualifier (`.` or `.@` or `...`), provided that something is being qualified (i.e. is not using an implicit `With` context)</span></span>

* <span data-ttu-id="a862d-122">かっこの前に (`)`)、中かっこを閉じます (`}`)、または閉じる埋め込み式 (`%>`)</span><span class="sxs-lookup"><span data-stu-id="a862d-122">before a close parenthesis (`)`), close curly brace (`}`), or close embedded expression (`%>`)</span></span>

* <span data-ttu-id="a862d-123">小後-より (`<`) 属性コンテキスト</span><span class="sxs-lookup"><span data-stu-id="a862d-123">after a less-than (`<`) in an attribute context</span></span>

* <span data-ttu-id="a862d-124">前に、大きい-より (`>`) 属性コンテキスト</span><span class="sxs-lookup"><span data-stu-id="a862d-124">before a greater-than (`>`) in an attribute context</span></span>

* <span data-ttu-id="a862d-125">後に、大きい-より (`>`) 非ファイル レベルの属性のコンテキストで</span><span class="sxs-lookup"><span data-stu-id="a862d-125">after a greater-than (`>`) in a non-file-level attribute context</span></span>

* <span data-ttu-id="a862d-126">クエリ演算子の前後に (`Where`、 `Order`、`Select`など)。</span><span class="sxs-lookup"><span data-stu-id="a862d-126">before and after query operators (`Where`, `Order`, `Select`, etc.)</span></span>

* <span data-ttu-id="a862d-127">二項演算子の後 (`+`、 `-`、 `/`、`*`など) 式のコンテキストで</span><span class="sxs-lookup"><span data-stu-id="a862d-127">after binary operators (`+`, `-`, `/`, `*`, etc.) in an expression context</span></span>

* <span data-ttu-id="a862d-128">代入演算子の後 (`=`、 `:=`、 `+=`、`-=`など) 任意のコンテキストでします。</span><span class="sxs-lookup"><span data-stu-id="a862d-128">after assignment operators (`=`, `:=`, `+=`, `-=`, etc.) in any context.</span></span>

<span data-ttu-id="a862d-129">行終端記号が、行連結文字として扱われます。</span><span class="sxs-lookup"><span data-stu-id="a862d-129">the line terminator is treated as if it was a line continuation.</span></span>

```antlr
Comma
    : ',' LineTerminator?
    ;

Period
    : '.' LineTerminator?
    ;

OpenParenthesis
    : '(' LineTerminator?
    ;

CloseParenthesis
    : LineTerminator? ')'
    ;

OpenCurlyBrace
    : '{' LineTerminator?
    ;

CloseCurlyBrace
    : LineTerminator? '}'
    ;

Equals
    : '=' LineTerminator?
    ;

ColonEquals
    : ':' '=' LineTerminator?
    ;
```

<span data-ttu-id="a862d-130">たとえば、前の例としてを記述もできます。</span><span class="sxs-lookup"><span data-stu-id="a862d-130">For example, the previous example could also be written as:</span></span>

```vb
Module Test
    Sub Print(
        Param1 As Integer,
        Param2 As Integer)

        If (Param1 < Param2) Or
            (Param1 > Param2) Then
            Console.WriteLine("Not equal")
        End If
    End Function
End Module
```

<span data-ttu-id="a862d-131">暗黙的な行継続は、指定したトークンの前後に直接しか推論されます。</span><span class="sxs-lookup"><span data-stu-id="a862d-131">Implicit line continuations will only ever be inferred directly before or after the specified token.</span></span> <span data-ttu-id="a862d-132">それらはない、行連結文字の前後に推論されます。</span><span class="sxs-lookup"><span data-stu-id="a862d-132">They will not be inferred before or after a line continuation.</span></span> <span data-ttu-id="a862d-133">例えば:</span><span class="sxs-lookup"><span data-stu-id="a862d-133">For example:</span></span>

```vb
Dim y = 10
' Error: Expression expected for assignment to x
Dim x = _

y
```

<span data-ttu-id="a862d-134">条件付きコンパイルのコンテキストでは行連結文字を推論できません。</span><span class="sxs-lookup"><span data-stu-id="a862d-134">Line continuations will not be inferred in conditional compilation contexts.</span></span> <span data-ttu-id="a862d-135">(__に注意してください。__</span><span class="sxs-lookup"><span data-stu-id="a862d-135">(__Note.__</span></span> <span data-ttu-id="a862d-136">コンパイルされない条件付きコンパイル ブロック内のテキストを構文的に有効にする必要はありませんので、この最後の制限が必要です。</span><span class="sxs-lookup"><span data-stu-id="a862d-136">This last restriction is required because text in conditional compilation blocks that are not compiled do not have to be syntactically valid.</span></span> <span data-ttu-id="a862d-137">そのため、ブロック内のテキストが誤って"によって取得されるまで"、条件付きコンパイル ステートメント言語が、将来の拡張を取得するように特に。)</span><span class="sxs-lookup"><span data-stu-id="a862d-137">Thus, text in the block might accidentally get "picked up" by the conditional compilation statement, especially as the language gets extended in the future.)</span></span>


### <a name="white-space"></a><span data-ttu-id="a862d-138">空白文字</span><span class="sxs-lookup"><span data-stu-id="a862d-138">White Space</span></span>

<span data-ttu-id="a862d-139">*空白*トークンを区切る場合にのみ機能し、それ以外の場合は無視されます。</span><span class="sxs-lookup"><span data-stu-id="a862d-139">*White space* serves only to separate tokens and is otherwise ignored.</span></span> <span data-ttu-id="a862d-140">空白文字のみを含む論理行は無視されます。</span><span class="sxs-lookup"><span data-stu-id="a862d-140">Logical lines containing only white space are ignored.</span></span> <span data-ttu-id="a862d-141">(__に注意してください。__</span><span class="sxs-lookup"><span data-stu-id="a862d-141">(__Note.__</span></span>
<span data-ttu-id="a862d-142">行ターミネータは考慮されません空白文字。)</span><span class="sxs-lookup"><span data-stu-id="a862d-142">Line terminators are not considered white space.)</span></span>

```antlr
WhiteSpace
    : '<Unicode class Zs>'
    | '<Unicode Tab 0x0009>'
    ;
```

### <a name="comments"></a><span data-ttu-id="a862d-143">コメント</span><span class="sxs-lookup"><span data-stu-id="a862d-143">Comments</span></span>

<span data-ttu-id="a862d-144">A*コメント*が単一引用符文字またはキーワードで始まる`REM`します。</span><span class="sxs-lookup"><span data-stu-id="a862d-144">A *comment* begins with a single-quote character or the keyword `REM`.</span></span> <span data-ttu-id="a862d-145">単一引用符文字が ASCII の単一引用符文字、Unicode の左側、一重引用符の文字または Unicode 権限には、単一引用符文字。</span><span class="sxs-lookup"><span data-stu-id="a862d-145">A single-quote character is either an ASCII single-quote character, a Unicode left single-quote character, or a Unicode right single-quote character.</span></span> <span data-ttu-id="a862d-146">ソースの行にコメントがどこからでも開始でき、物理的な行末までがコメントを終了します。</span><span class="sxs-lookup"><span data-stu-id="a862d-146">Comments can begin anywhere on a source line, and the end of the physical line ends the comment.</span></span> <span data-ttu-id="a862d-147">コンパイラは、コメントの先頭と行終端記号の間の文字を無視します。</span><span class="sxs-lookup"><span data-stu-id="a862d-147">The compiler ignores the characters between the beginning of the comment and the line terminator.</span></span> <span data-ttu-id="a862d-148">その結果、コメントは、行継続を使用して複数行の間で拡張することはできません。</span><span class="sxs-lookup"><span data-stu-id="a862d-148">Consequently, comments cannot extend across multiple lines by using line continuations.</span></span>

```antlr
Comment
    : CommentMarker Character*
    ;

CommentMarker
    : SingleQuoteCharacter
    | 'REM'
    ;

SingleQuoteCharacter
    : '\''
    | '<Unicode 0x2018>'
    | '<Unicode 0x2019>'
    ;
```

## <a name="identifiers"></a><span data-ttu-id="a862d-149">識別子</span><span class="sxs-lookup"><span data-stu-id="a862d-149">Identifiers</span></span>

<span data-ttu-id="a862d-150">*識別子*名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="a862d-150">An *identifier* is a name.</span></span> <span data-ttu-id="a862d-151">Visual Basic 識別子は、Unicode Standard Annex 15 は、1 つの例外に準拠している: 識別子はアンダー スコア (コネクタ) で始まる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a862d-151">Visual Basic identifiers conform to the Unicode Standard Annex 15 with one exception: identifiers may begin with an underscore (connector) character.</span></span> <span data-ttu-id="a862d-152">識別子はアンダー スコアで始まっている場合、行の連結から区別するために他の少なくとも 1 つの有効な識別子文字を含めることが必要があります。</span><span class="sxs-lookup"><span data-stu-id="a862d-152">If an identifier begins with an underscore, it must contain at least one other valid identifier character to disambiguate it from a line continuation.</span></span>

```antlr
Identifier
    : NonEscapedIdentifier TypeCharacter?
    | Keyword TypeCharacter
    | EscapedIdentifier
    ;

NonEscapedIdentifier
    : '<Any IdentifierName but not Keyword>'
    ;

EscapedIdentifier
    : '[' IdentifierName ']'
    ;

IdentifierName
    : IdentifierStart IdentifierCharacter*
    ;

IdentifierStart
    : AlphaCharacter
    | UnderscoreCharacter IdentifierCharacter
    ;

IdentifierCharacter
    : UnderscoreCharacter
    | AlphaCharacter
    | NumericCharacter
    | CombiningCharacter
    | FormattingCharacter
    ;

AlphaCharacter
    : '<Unicode classes Lu,Ll,Lt,Lm,Lo,Nl>'
    ;

NumericCharacter
    : '<Unicode decimal digit class Nd>'
    ;

CombiningCharacter
    : '<Unicode combining character classes Mn, Mc>'
    ;

FormattingCharacter
    : '<Unicode formatting character class Cf>'
    ;

UnderscoreCharacter
    : '<Unicode connection character class Pc>'
    ;

IdentifierOrKeyword
    : Identifier
    | Keyword
    ;
```

<span data-ttu-id="a862d-153">標準識別子が、キーワードに一致しないが、エスケープされた識別子または型の文字を識別子のことができます。</span><span class="sxs-lookup"><span data-stu-id="a862d-153">Regular identifiers may not match keywords, but escaped identifiers or identifiers with a type character can.</span></span> <span data-ttu-id="a862d-154">*識別子をエスケープ*の角かっこで区切られた識別子です。</span><span class="sxs-lookup"><span data-stu-id="a862d-154">An *escaped identifier* is an identifier delimited by square brackets.</span></span> <span data-ttu-id="a862d-155">キーワードに一致する可能性がありますが、型文字がない可能性がありますとする点を除いて、標準識別子と同じ規則識別子に従ってをエスケープします。</span><span class="sxs-lookup"><span data-stu-id="a862d-155">Escaped identifiers follow the same rules as regular identifiers except that they may match keywords and may not have type characters.</span></span>

<span data-ttu-id="a862d-156">この例は、という名前のクラスを定義します`class`という名前の共有メソッドで`shared`という名前のパラメーターを受け取る`boolean`メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="a862d-156">This example defines a class named `class` with a shared method named `shared` that takes a parameter named `boolean` and then calls the method.</span></span>

```vb
Class [class]
    Shared Sub [shared]([boolean] As Boolean)
        If [boolean] Then
            Console.WriteLine("true")
        Else
            Console.WriteLine("false")
        End If
    End Sub
End Class

Module [module]
    Sub Main()
        [class].[shared](True)
    End Sub
End Module
```

<span data-ttu-id="a862d-157">識別子は、2 つの識別子を大文字と小文字が異なる場合、同じ識別子であると見なされるように大文字小文字を区別します。</span><span class="sxs-lookup"><span data-stu-id="a862d-157">Identifiers are case insensitive, so two identifiers are considered to be the same identifier if they differ only in case.</span></span> <span data-ttu-id="a862d-158">(__に注意してください。__</span><span class="sxs-lookup"><span data-stu-id="a862d-158">(__Note.__</span></span> <span data-ttu-id="a862d-159">識別子を比較するときに、Unicode の標準的な 1 対 1 大文字と小文字マッピングが使用し、ロケール固有の大文字小文字マップは無視されます。)</span><span class="sxs-lookup"><span data-stu-id="a862d-159">The Unicode Standard one-to-one case mappings are used when comparing identifiers and any locale-specific case mappings are ignored.)</span></span>


### <a name="type-characters"></a><span data-ttu-id="a862d-160">型文字</span><span class="sxs-lookup"><span data-stu-id="a862d-160">Type Characters</span></span>

<span data-ttu-id="a862d-161">A*文字入力*先行する識別子の種類を表します。</span><span class="sxs-lookup"><span data-stu-id="a862d-161">A *type character* denotes the type of the preceding identifier.</span></span> <span data-ttu-id="a862d-162">型文字は、識別子の一部とは見なされません。</span><span class="sxs-lookup"><span data-stu-id="a862d-162">The type character is not considered part of the identifier.</span></span>

```antlr
TypeCharacter
    : IntegerTypeCharacter
    | LongTypeCharacter
    | DecimalTypeCharacter
    | SingleTypeCharacter
    | DoubleTypeCharacter
    | StringTypeCharacter
    ;

IntegerTypeCharacter
    : '%'
    ;

LongTypeCharacter
    : '&'
    ;

DecimalTypeCharacter
    : '@'
    ;

SingleTypeCharacter
    : '!'
    ;

DoubleTypeCharacter
    : '#'
    ;

StringTypeCharacter
    : '$'
    ;
```

<span data-ttu-id="a862d-163">型文字は、宣言自体で指定された型と一致する必要があります、宣言には、型文字が含まれている場合それ以外の場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a862d-163">If a declaration includes a type character, the type character must agree with the type specified in the declaration itself; otherwise, a compile-time error occurs.</span></span> <span data-ttu-id="a862d-164">宣言型が省略されている場合 (が指定されていない場合など、`As`句)、型文字は、宣言の型として暗黙的にします。</span><span class="sxs-lookup"><span data-stu-id="a862d-164">If the declaration omits the type (for example, if it does not specify an `As` clause), the type character is implicitly substituted as the type of the declaration.</span></span>

<span data-ttu-id="a862d-165">空白文字が識別子とその型の文字の間到達しない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a862d-165">No white space may come between an identifier and its type character.</span></span> <span data-ttu-id="a862d-166">型文字がない`Byte`、 `SByte`、 `UShort`、 `Short`、`UInteger`または`ULong`、適切な文字の不足が原因です。</span><span class="sxs-lookup"><span data-stu-id="a862d-166">There are no type characters for `Byte`, `SByte`, `UShort`, `Short`, `UInteger` or `ULong`, due to a lack of suitable characters.</span></span>

<span data-ttu-id="a862d-167">概念的には、型 (たとえば、名前空間の名前) がありません識別子に、または型の文字の型と一致しない型を持つ識別子には、型文字を付加すると、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a862d-167">Appending a type character to an identifier that conceptually does not have a type (for example, a namespace name) or to an identifier whose type disagrees with the type of the type character causes a compile-time error.</span></span>

<span data-ttu-id="a862d-168">次の例は、型の文字の使用を示しています。</span><span class="sxs-lookup"><span data-stu-id="a862d-168">The following example shows the use of type characters:</span></span>

```vb
' The follow line will cause an error: standard modules have no type.
Module Test1#
End Module

Module Test2

    ' This function takes a Long parameter and returns a String.
    Function Func$(Param&)

        ' The following line causes an error because the type character
        ' conflicts with the declared type of Func and Param.
        Func# = CStr(Param@)

        ' The following line is valid.
        Func$ = CStr(Param&)
    End Function
End Module
```

<span data-ttu-id="a862d-169">型文字`!`特別な問題が示されることで、型文字、および言語での区切り記号として使用できます。</span><span class="sxs-lookup"><span data-stu-id="a862d-169">The type character `!` presents a special problem in that it can be used both as a type character and as a separator in the language.</span></span> <span data-ttu-id="a862d-170">あいまいさを削除する、`!`文字がそれに続く文字が識別子を開始できない限り、型文字。</span><span class="sxs-lookup"><span data-stu-id="a862d-170">To remove ambiguity, a `!` character is a type character as long as the character that follows it cannot start an identifier.</span></span> <span data-ttu-id="a862d-171">可能な場合、`!`文字が区切り記号で型文字ではありません。</span><span class="sxs-lookup"><span data-stu-id="a862d-171">If it can, then the `!` character is a separator, not a type character.</span></span>


## <a name="keywords"></a><span data-ttu-id="a862d-172">キーワード</span><span class="sxs-lookup"><span data-stu-id="a862d-172">Keywords</span></span>

<span data-ttu-id="a862d-173">A*キーワード*言語コンストラクトで特別な意味のある単語します。</span><span class="sxs-lookup"><span data-stu-id="a862d-173">A *keyword* is a word that has special meaning in a language construct.</span></span> <span data-ttu-id="a862d-174">すべてのキーワードは、言語によって予約されており、識別子をエスケープしない限りは識別子として使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="a862d-174">All keywords are reserved by the language and may not be used as identifiers unless the identifiers are escaped.</span></span> <span data-ttu-id="a862d-175">(__に注意してください。__</span><span class="sxs-lookup"><span data-stu-id="a862d-175">(__Note.__</span></span> <span data-ttu-id="a862d-176">`EndIf`、 `GoSub`、 `Let`、 `Variant`、および`Wend`Visual Basic では使用されなくがキーワードとして保持されます。)</span><span class="sxs-lookup"><span data-stu-id="a862d-176">`EndIf`, `GoSub`, `Let`, `Variant`, and `Wend` are retained as keywords, although they are no longer used in Visual Basic.)</span></span>

```antlr
Keyword
    : 'AddHandler'      | 'AddressOf'      | 'Alias'       | 'And'
    | 'AndAlso'         | 'As'             | 'Boolean'     | 'ByRef'
    | 'Byte'            | 'ByVal'          | 'Call'        | 'Case'        
    | 'Catch'           | 'CBool'          | 'CByte'       | 'CChar'       
    | 'CDate'           | 'CDbl'           | 'CDec'        | 'Char'        
    | 'CInt'            | 'Class'          | 'CLng'        | 'CObj'        
    | 'Const'           | 'Continue'       | 'CSByte'      | 'CShort'      
    | 'CSng'            | 'CStr'           | 'CType'       | 'CUInt'       
    | 'CULng'           | 'CUShort'        | 'Date'        | 'Decimal'     
    | 'Declare'         | 'Default'        | 'Delegate'    | 'Dim'         
    | 'DirectCast'      | 'Do'             | 'Double'      | 'Each'        
    | 'Else'            | 'ElseIf'         | 'End'         | 'EndIf'       
    | 'Enum'            | 'Erase'          | 'Error'       | 'Event'       
    | 'Exit'            | 'False'          | 'Finally'     | 'For'         
    | 'Friend'          | 'Function'       | 'Get'         | 'GetType'     
    | 'GetXmlNamespace' | 'Global'         | 'GoSub'       | 'GoTo'        
    | 'Handles'         | 'If'             | 'Implements'  | 'Imports'     
    | 'In'              | 'Inherits'       | 'Integer'     | 'Interface'   
    | 'Is'              | 'IsNot'          | 'Let'         | 'Lib'         
    | 'Like'            | 'Long'           | 'Loop'        | 'Me'          
    | 'Mod'             | 'Module'         | 'MustInherit' | 'MustOverride'
    | 'MyBase'          | 'MyClass'        | 'Namespace'   | 'Narrowing'   
    | 'New'             | 'Next'           | 'Not'         | 'Nothing'     
    | 'NotInheritable'  | 'NotOverridable' | 'Object'      | 'Of'          
    | 'On'              | 'Operator'       | 'Option'      | 'Optional'    
    | 'Or'              | 'OrElse'         | 'Overloads'   | 'Overridable' 
    | 'Overrides'       | 'ParamArray'     | 'Partial'     | 'Private'     
    | 'Property'        | 'Protected'      | 'Public'      | 'RaiseEvent'  
    | 'ReadOnly'        | 'ReDim'          | 'REM'         | 'RemoveHandler'
    | 'Resume'          | 'Return'         | 'SByte'       | 'Select'      
    | 'Set'             | 'Shadows'        | 'Shared'      | 'Short'       
    | 'Single'          | 'Static'         | 'Step'        | 'Stop'        
    | 'String'          | 'Structure'      | 'Sub'         | 'SyncLock'    
    | 'Then'            | 'Throw'          | 'To'          | 'True'        
    | 'Try'             | 'TryCast'        | 'TypeOf'      | 'UInteger'    
    | 'ULong'           | 'UShort'         | 'Using'       | 'Variant'     
    | 'Wend'            | 'When'           | 'While'       | 'Widening'    
    | 'With'            | 'WithEvents'     | 'WriteOnly'   | 'Xor'         
    ;
```

## <a name="literals"></a><span data-ttu-id="a862d-177">リテラル</span><span class="sxs-lookup"><span data-stu-id="a862d-177">Literals</span></span>

<span data-ttu-id="a862d-178">A*リテラル*型の特定の値のテキスト表現です。</span><span class="sxs-lookup"><span data-stu-id="a862d-178">A *literal* is a textual representation of a particular value of a type.</span></span> <span data-ttu-id="a862d-179">リテラル型には、ブール値、整数、浮動小数点、文字列、文字、および日付が含まれます。</span><span class="sxs-lookup"><span data-stu-id="a862d-179">Literal types include Boolean, integer, floating point, string, character, and date.</span></span>

```antlr
Literal
    : BooleanLiteral
    | IntegerLiteral
    | FloatingPointLiteral
    | StringLiteral
    | CharacterLiteral
    | DateLiteral
    | Nothing
    ;
```

### <a name="boolean-literals"></a><span data-ttu-id="a862d-180">ブール型リテラル</span><span class="sxs-lookup"><span data-stu-id="a862d-180">Boolean Literals</span></span>

<span data-ttu-id="a862d-181">`True` `False`のリテラルは、`Boolean`はそれぞれ、true と false の状態にマップする型。</span><span class="sxs-lookup"><span data-stu-id="a862d-181">`True` and `False` are literals of the `Boolean` type that map to the true and false state, respectively.</span></span>

```antlr
BooleanLiteral
    : 'True' | 'False'
    ;
```

### <a name="integer-literals"></a><span data-ttu-id="a862d-182">整数リテラル</span><span class="sxs-lookup"><span data-stu-id="a862d-182">Integer Literals</span></span>

<span data-ttu-id="a862d-183">整数リテラルには、10 進数 (基数 10)、16 進数 (基数 16)、または 8 進数 (基数 8) を指定します。</span><span class="sxs-lookup"><span data-stu-id="a862d-183">Integer literals can be decimal (base 10), hexadecimal (base 16), or octal (base 8).</span></span> <span data-ttu-id="a862d-184">10 進数の整数リテラルは、10 進数字 (0 ~ 9) の文字列です。</span><span class="sxs-lookup"><span data-stu-id="a862d-184">A decimal integer literal is a string of decimal digits (0-9).</span></span> <span data-ttu-id="a862d-185">16 進数リテラルは、`&H`続けて 16 進数の数字 (0 ~ 9、A ~ F) の文字列。</span><span class="sxs-lookup"><span data-stu-id="a862d-185">A hexadecimal literal is `&H` followed by a string of hexadecimal digits (0-9, A-F).</span></span> <span data-ttu-id="a862d-186">8 進数のリテラルは、`&O`続けて 8 進数の数字 (0 ~ 7) の文字列。</span><span class="sxs-lookup"><span data-stu-id="a862d-186">An octal literal is `&O` followed by a string of octal digits (0-7).</span></span> <span data-ttu-id="a862d-187">10 進リテラルは、8 進数と 16 進数のリテラル、整数リテラルのバイナリ値を表しますが、直接、整数リテラルの 10 進値を表します (したがって、 `&H8000S` -32768、オーバーフロー エラーではありません)。</span><span class="sxs-lookup"><span data-stu-id="a862d-187">Decimal literals directly represent the decimal value of the integral literal, whereas octal and hexadecimal literals represent the binary value of the integer literal (thus, `&H8000S` is -32768, not an overflow error).</span></span>

```antlr
IntegerLiteral
    : IntegralLiteralValue IntegralTypeCharacter?
    ;

IntegralLiteralValue
    : IntLiteral
    | HexLiteral
    | OctalLiteral
    ;

IntegralTypeCharacter
    : ShortCharacter
    | UnsignedShortCharacter
    | IntegerCharacter
    | UnsignedIntegerCharacter
    | LongCharacter
    | UnsignedLongCharacter
    | IntegerTypeCharacter
    | LongTypeCharacter
    ;

ShortCharacter
    : 'S'
    ;

UnsignedShortCharacter
    : 'US'
    ;

IntegerCharacter
    : 'I'
    ;

UnsignedIntegerCharacter
    : 'UI'
    ;

LongCharacter
    : 'L'
    ;

UnsignedLongCharacter
    : 'UL'
    ;

IntLiteral
    : Digit+
    ;

HexLiteral
    : '&' 'H' HexDigit+
    ;

OctalLiteral
    : '&' 'O' OctalDigit+
    ;

Digit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'
    ;

HexDigit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'
    | 'A' | 'B' | 'C' | 'D' | 'E' | 'F'
    ;

OctalDigit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7'
    ;
```

<span data-ttu-id="a862d-188">リテラルの型は、値、または次の型文字によって決定されます。</span><span class="sxs-lookup"><span data-stu-id="a862d-188">The type of a literal is determined by its value or by the following type character.</span></span> <span data-ttu-id="a862d-189">範囲の値の型文字が指定されていない場合、`Integer`型は、入力`Integer`; の範囲外の値`Integer`として入力された`Long`します。</span><span class="sxs-lookup"><span data-stu-id="a862d-189">If no type character is specified, values in the range of the `Integer` type are typed as `Integer`; values outside the range for `Integer` are typed as `Long`.</span></span> <span data-ttu-id="a862d-190">整数リテラルの型がリテラル、整数を保持するために十分なサイズである場合、コンパイル エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a862d-190">If an integer literal's type is of insufficient size to hold the integer literal, a compile-time error results.</span></span> <span data-ttu-id="a862d-191">(__に注意してください。__</span><span class="sxs-lookup"><span data-stu-id="a862d-191">(__Note.__</span></span> <span data-ttu-id="a862d-192">型文字がない`Byte`が最も自然な文字になるので`B`、16 進数リテラルで有効な文字である)。</span><span class="sxs-lookup"><span data-stu-id="a862d-192">There isn't a type character for `Byte` because the most natural character would be `B`, which is a legal character in a hexadecimal literal.)</span></span>


### <a name="floating-point-literals"></a><span data-ttu-id="a862d-193">浮動小数点リテラル</span><span class="sxs-lookup"><span data-stu-id="a862d-193">Floating-Point Literals</span></span>

<span data-ttu-id="a862d-194">浮動小数点リテラルは、整数リテラルの後に省略可能な 10 進数のポイント (ピリオドの ASCII 文字) と仮数と省略可能な基本 10 の指数部です。</span><span class="sxs-lookup"><span data-stu-id="a862d-194">A floating-point literal is an integer literal followed by an optional decimal point (the ASCII period character) and mantissa, and an optional base 10 exponent.</span></span> <span data-ttu-id="a862d-195">既定では、浮動小数点リテラルは、型の`Double`します。</span><span class="sxs-lookup"><span data-stu-id="a862d-195">By default, a floating-point literal is of type `Double`.</span></span> <span data-ttu-id="a862d-196">場合、 `Single`、 `Double`、または`Decimal`型文字を指定すると、その型のリテラルは、します。</span><span class="sxs-lookup"><span data-stu-id="a862d-196">If the `Single`, `Double`, or `Decimal` type character is specified, the literal is of that type.</span></span> <span data-ttu-id="a862d-197">浮動小数点リテラルの型が浮動小数点リテラルを保持するサイズが不十分である場合、コンパイル エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a862d-197">If a floating-point literal's type is of insufficient size to hold the floating-point literal, a compile-time error results.</span></span>

<span data-ttu-id="a862d-198">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="a862d-198">__Note.__</span></span> <span data-ttu-id="a862d-199">いることに注意が、`Decimal`データ型は、後続の 0 値をエンコードできます。</span><span class="sxs-lookup"><span data-stu-id="a862d-199">It is worth noting that the `Decimal` data type can encode trailing zeros in a value.</span></span> <span data-ttu-id="a862d-200">仕様にで 0 が末尾かどうかについてのコメントはありません、`Decimal`リテラルはコンパイラによって受け入れられる必要があります。</span><span class="sxs-lookup"><span data-stu-id="a862d-200">The specification currently makes no comment about whether trailing zeros in a `Decimal` literal should be honored by a compiler.</span></span>

```antlr
FloatingPointLiteral
    : FloatingPointLiteralValue FloatingPointTypeCharacter?
    | IntLiteral FloatingPointTypeCharacter
    ;

FloatingPointTypeCharacter
    : SingleCharacter
    | DoubleCharacter
    | DecimalCharacter
    | SingleTypeCharacter
    | DoubleTypeCharacter
    | DecimalTypeCharacter
    ;

SingleCharacter
    : 'F'
    ;

DoubleCharacter
    : 'R'
    ;

DecimalCharacter
    : 'D'
    ;

FloatingPointLiteralValue
    : IntLiteral '.' IntLiteral Exponent?
    | '.' IntLiteral Exponent?
    | IntLiteral Exponent
    ;

Exponent
    : 'E' Sign? IntLiteral
    ;

Sign
    : '+'
    | '-'
    ;
```

### <a name="string-literals"></a><span data-ttu-id="a862d-201">文字列リテラル。</span><span class="sxs-lookup"><span data-stu-id="a862d-201">String Literals</span></span>

<span data-ttu-id="a862d-202">文字列リテラルとは、0 個以上の Unicode のシーケンスの先頭文字し、ASCII 二重引用符文字で終わる、Unicode の左側二重引用符の文字または Unicode 右二重引用符文字です。</span><span class="sxs-lookup"><span data-stu-id="a862d-202">A string literal is a sequence of zero or more Unicode characters beginning and ending with an ASCII double-quote character, a Unicode left double-quote character, or a Unicode right double-quote character.</span></span> <span data-ttu-id="a862d-203">、文字列内では、2 つの二重引用符文字のシーケンスは、文字列内の二重引用符を表すエスケープ シーケンスです。</span><span class="sxs-lookup"><span data-stu-id="a862d-203">Within a string, a sequence of two double-quote characters is an escape sequence representing a double quote in the string.</span></span>

```antlr
StringLiteral
    : DoubleQuoteCharacter StringCharacter* DoubleQuoteCharacter
    ;

DoubleQuoteCharacter
    : '"'
    | '<unicode left double-quote 0x201c>'
    | '<unicode right double-quote 0x201D>'
    ;

StringCharacter
    : '<Any character except DoubleQuoteCharacter>'
    | DoubleQuoteCharacter DoubleQuoteCharacter
    ;
```

<span data-ttu-id="a862d-204">文字列定数は、`String`型。</span><span class="sxs-lookup"><span data-stu-id="a862d-204">A string constant is of the `String` type.</span></span>

```vb
Module Test
    Sub Main()

        ' This prints out: ".
        Console.WriteLine("""")

        ' This prints out: a"b.
        Console.WriteLine("a""b")

        ' This causes a compile error due to mismatched double-quotes.
        Console.WriteLine("a"b")
    End Sub
End Module
```

<span data-ttu-id="a862d-205">コンパイラを許可して、文字列定数式をリテラル文字列に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a862d-205">The compiler is allowed to replace a constant string expression with a string literal.</span></span> <span data-ttu-id="a862d-206">各文字列リテラルは、新しい文字列インスタンスで必ずしも発生するされません。</span><span class="sxs-lookup"><span data-stu-id="a862d-206">Each string literal does not necessarily result in a new string instance.</span></span> <span data-ttu-id="a862d-207">バイナリ比較セマンティクスを使用して文字列の等値演算子に従って等しい 2 つ以上の文字列リテラルは、同じプログラム内で表示されたら、これらの文字列リテラルは可能性があります、同じ文字列インスタンスを参照してください。</span><span class="sxs-lookup"><span data-stu-id="a862d-207">When two or more string literals that are equivalent according to the string equality operator using binary comparison semantics appear in the same program, these string literals may refer to the same string instance.</span></span> <span data-ttu-id="a862d-208">たとえば、次のプログラムの出力を返す可能性があります`True`のため、2 つのリテラルは、同じ文字列インスタンスを参照してください。</span><span class="sxs-lookup"><span data-stu-id="a862d-208">For instance, the output of the following program may return `True` because the two literals may refer to the same string instance.</span></span>

```vb
Module Test
    Sub Main()
        Dim a As Object = "he" & "llo"
        Dim b As Object = "hello"
        Console.WriteLine(a Is b)
    End Sub
End Module
```


### <a name="character-literals"></a><span data-ttu-id="a862d-209">文字リテラル</span><span class="sxs-lookup"><span data-stu-id="a862d-209">Character Literals</span></span>

<span data-ttu-id="a862d-210">文字リテラルの 1 つの Unicode 文字を表す、`Char`型。</span><span class="sxs-lookup"><span data-stu-id="a862d-210">A character literal represents a single Unicode character of the `Char` type.</span></span> <span data-ttu-id="a862d-211">2 つの二重引用符文字は、二重引用符の文字を表すエスケープ シーケンスです。</span><span class="sxs-lookup"><span data-stu-id="a862d-211">Two double-quote characters is an escape sequence representing the double-quote character.</span></span>

```antlr
CharacterLiteral
    : DoubleQuoteCharacter StringCharacter DoubleQuoteCharacter 'C'
    ;
```


```vb
Module Test
    Sub Main()

        ' This prints out: a.
        Console.WriteLine("a"c)

        ' This prints out: ".
        Console.WriteLine(""""c)
    End Sub
End Module
```


### <a name="date-literals"></a><span data-ttu-id="a862d-212">日付リテラル</span><span class="sxs-lookup"><span data-stu-id="a862d-212">Date Literals</span></span>

<span data-ttu-id="a862d-213">日付リテラルが、特定の時点での値で表された時刻を表す、`Date`型。</span><span class="sxs-lookup"><span data-stu-id="a862d-213">A date literal represents a particular moment in time expressed as a value of the `Date` type.</span></span>

```antlr
DateLiteral
    : '#' WhiteSpace* DateOrTime WhiteSpace* '#'
    ;

DateOrTime
    : DateValue WhiteSpace+ TimeValue
    | DateValue
    | TimeValue
    ;

DateValue
    : MonthValue '/' DayValue '/' YearValue
    | MonthValue '-' DayValue '-' YearValue
    ;

TimeValue
    : HourValue ':' MinuteValue ( ':' SecondValue )? WhiteSpace* AMPM?
    | HourValue WhiteSpace* AMPM
    ;

MonthValue
    : IntLiteral
    ;

DayValue
    : IntLiteral
    ;

YearValue
    : IntLiteral
    ;

HourValue
    : IntLiteral
    ;

MinuteValue
    : IntLiteral
    ;

SecondValue
    : IntLiteral
    ;

AMPM
    : 'AM' | 'PM'
    ;    
```

<span data-ttu-id="a862d-214">リテラルには、日付と時刻、日付だけ、または時刻だけの両方を指定できます。</span><span class="sxs-lookup"><span data-stu-id="a862d-214">The literal may specify both a date and a time, just a date, or just a time.</span></span> <span data-ttu-id="a862d-215">日付の値を省略した場合、構成のグレゴリオ暦カレンダーで 1 年 1 月 1 日と見なされます。</span><span class="sxs-lookup"><span data-stu-id="a862d-215">If the date value is omitted, then January 1 of the year 1 in the Gregorian calendar is assumed.</span></span> <span data-ttu-id="a862d-216">時刻の値を省略すると、12時 00分: 00 AM が使われます。</span><span class="sxs-lookup"><span data-stu-id="a862d-216">If the time value is omitted, then 12:00:00 AM is assumed.</span></span>

<span data-ttu-id="a862d-217">日付値の年の値の解釈で問題を避けるためには、年の値は 2 桁の数字にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="a862d-217">To avoid problems with interpreting the year value in a date value, the year value cannot be two digits.</span></span> <span data-ttu-id="a862d-218">最初の世紀 AD/CE の日付を表現する場合、先行ゼロを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a862d-218">When expressing a date in the first century AD/CE, leading zeros must be specified.</span></span>

<span data-ttu-id="a862d-219">24 時間の値または; 12 時間値を使用時間の値を指定することがあります。時刻の値を省略する、`AM`または`PM`は 24 時間の値であると見なされます。</span><span class="sxs-lookup"><span data-stu-id="a862d-219">A time value may be specified either using a 24-hour value or a 12-hour value; time values that omit an `AM` or `PM` are assumed to be 24-hour values.</span></span> <span data-ttu-id="a862d-220">時間の値が、分、リテラルを省略するかどうかは`0`既定で使用されます。</span><span class="sxs-lookup"><span data-stu-id="a862d-220">If a time value omits the minutes, the literal `0` is used by default.</span></span> <span data-ttu-id="a862d-221">時間の値がリテラル (秒) を省略するかどうかは`0`既定で使用されます。</span><span class="sxs-lookup"><span data-stu-id="a862d-221">If a time value omits the seconds, the literal `0` is used by default.</span></span> <span data-ttu-id="a862d-222">分と秒の両方を省略した場合、し`AM`または`PM`指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a862d-222">If both minutes and second are omitted, then `AM` or `PM` must be specified.</span></span> <span data-ttu-id="a862d-223">範囲外に指定された日付値が、`Date`入力すると、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a862d-223">If the date value specified is outside the range of the `Date` type, a compile-time error occurs.</span></span>

<span data-ttu-id="a862d-224">次の例には、いくつかの日付リテラルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="a862d-224">The following example contains several date literals.</span></span>

```vb
Dim d As Date

d = # 8/23/1970 3:45:39AM #
d = # 8/23/1970 #              ' Date value: 8/23/1970 12:00:00AM.
d = # 3:45:39AM #              ' Date value: 1/1/1 3:45:39AM.
d = # 3:45:39 #                ' Date value: 1/1/1 3:45:39AM.
d = # 13:45:39 #               ' Date value: 1/1/1 1:45:39PM.
d = # 1AM #                    ' Date value: 1/1/1 1:00:00AM.
d = # 13:45:39PM #             ' This date value is not valid.
```


### <a name="nothing"></a><span data-ttu-id="a862d-225">Nothing</span><span class="sxs-lookup"><span data-stu-id="a862d-225">Nothing</span></span>

<span data-ttu-id="a862d-226">`Nothing` 特別なリテラルです。型がないと、型パラメーターを含む、型システムですべての型に変換可能です。</span><span class="sxs-lookup"><span data-stu-id="a862d-226">`Nothing` is a special literal; it does not have a type and is convertible to all types in the type system, including type parameters.</span></span> <span data-ttu-id="a862d-227">特定の型に変換するときは、その型の既定値に相当します。</span><span class="sxs-lookup"><span data-stu-id="a862d-227">When converted to a particular type, it is the equivalent of the default value of that type.</span></span>

```antlr
Nothing
    : 'Nothing'
    ;
```

## <a name="separators"></a><span data-ttu-id="a862d-228">[区切り記号]</span><span class="sxs-lookup"><span data-stu-id="a862d-228">Separators</span></span>

<span data-ttu-id="a862d-229">次の ASCII 文字には、区切り記号がされます。</span><span class="sxs-lookup"><span data-stu-id="a862d-229">The following ASCII characters are separators:</span></span>

```antlr
Separator
    : '(' | ')' | '{' | '}' | '!' | '#' | ',' | '.' | ':' | '?'
    ;
```

## <a name="operator-characters"></a><span data-ttu-id="a862d-230">演算子文字</span><span class="sxs-lookup"><span data-stu-id="a862d-230">Operator Characters</span></span>

<span data-ttu-id="a862d-231">次の ASCII 文字または文字のシーケンス演算子を表します。</span><span class="sxs-lookup"><span data-stu-id="a862d-231">The following ASCII characters or character sequences denote operators:</span></span>

```antlr
Operator
    : '&' | '*' | '+' | '-' | '/' | '\\' | '^' | '<' | '=' | '>'
    ;
```

