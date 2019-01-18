---
ms.openlocfilehash: 2f14161222741beb7505f5674b230f1881828b5d
ms.sourcegitcommit: 6eca149bdc736113e0adb709212bd266c9503c33
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2019
ms.locfileid: "47426744"
---
# <a name="lexical-grammar"></a>字句文法

Visual Basic プログラムのコンパイルでは、最初、レキシカル トークンの順序付けされたセットに Unicode 文字の生のストリームを変換する必要があります。 Visual Basic 言語は自由な形式ではないため、トークンのセットが一連の論理行にさらに分割します。 A*論理行*ストリームの末尾には、行連結文字または修飾されていない次の行終端記号をストリームの開始または行の終端からのスパン。

__注意してください。__ 言語のバージョン 9.0 での XML リテラル式の導入に伴い、Visual Basic はなくなりました個別字句文法という意味で Visual Basic では構文のコンテキストに関係なくコードをトークン化することができます。 これは、原因という事実に XML と Visual Basic のさまざまな語彙の規則があるし、特定の時点に使用中の語彙の規則のセットがその時点でどのような構文構造が処理されている異なります。 この仕様では、標準の Visual Basic コードの語彙の規則のガイドとしてこの字句文法セクションが保持されます。

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

## <a name="characters-and-lines"></a>文字と行

Visual Basic プログラムは、Unicode 文字のセットの文字で構成されます。

```antlr
Character:
    '<Any Unicode character except a LineTerminator>'
    ;
```

### <a name="line-terminators"></a>行ターミネータ

Unicode 行区切り文字は、論理的な行を区切ります。

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

### <a name="line-continuation"></a>行の連結

A*行継続*(空白) を除くテキスト行の最後の文字として 1 つのアンダー スコア文字の直前にある少なくとも 1 つの空白文字で構成されます。 行の連結は、論理行にまたがる 1 つ以上の物理行を使用できます。 行連結文字として扱われます、空白がない場合でもです。

```antlr
LineContinuation
    : WhiteSpace '_' WhiteSpace* LineTerminator
    ;
```

次のプログラムは、いくつかの行連結文字を示しています。

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

いくつかの場所の構文文法では、*暗黙的な行継続*します。 行終端記号が発生したとき

* コンマの後に (`,`) 開きかっこ (`(`)、中かっこを開きます (`{`)、または埋め込み式を開く (`<%=`)

* メンバー修飾子の後 (`.`または`.@`または`...`)、その何かが修飾されている (つまりが使用されていない暗黙的な`With`コンテキスト)

* かっこの前に (`)`)、中かっこを閉じます (`}`)、または閉じる埋め込み式 (`%>`)

* 小後-より (`<`) 属性コンテキスト

* 前に、大きい-より (`>`) 属性コンテキスト

* 後に、大きい-より (`>`) 非ファイル レベルの属性のコンテキストで

* クエリ演算子の前後に (`Where`、 `Order`、`Select`など)。

* 二項演算子の後 (`+`、 `-`、 `/`、`*`など) 式のコンテキストで

* 代入演算子の後 (`=`、 `:=`、 `+=`、`-=`など) 任意のコンテキストでします。

行終端記号が、行連結文字として扱われます。

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

たとえば、前の例としてを記述もできます。

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

暗黙的な行継続は、指定したトークンの前後に直接しか推論されます。 それらはない、行連結文字の前後に推論されます。 例:

```vb
Dim y = 10
' Error: Expression expected for assignment to x
Dim x = _

y
```

条件付きコンパイルのコンテキストでは行連結文字を推論できません。 (__に注意してください。__ コンパイルされない条件付きコンパイル ブロック内のテキストを構文的に有効にする必要はありませんので、この最後の制限が必要です。 そのため、ブロック内のテキストが誤って"によって取得されるまで"、条件付きコンパイル ステートメント言語が、将来の拡張を取得するように特に。)


### <a name="white-space"></a>空白文字

*空白*トークンを区切る場合にのみ機能し、それ以外の場合は無視されます。 空白文字のみを含む論理行は無視されます。 (__に注意してください。__
行ターミネータは考慮されません空白文字。)

```antlr
WhiteSpace
    : '<Unicode class Zs>'
    | '<Unicode Tab 0x0009>'
    ;
```

### <a name="comments"></a>コメント

A*コメント*が単一引用符文字またはキーワードで始まる`REM`します。 単一引用符文字が ASCII の単一引用符文字、Unicode の左側、一重引用符の文字または Unicode 権限には、単一引用符文字。 ソースの行にコメントがどこからでも開始でき、物理的な行末までがコメントを終了します。 コンパイラは、コメントの先頭と行終端記号の間の文字を無視します。 その結果、コメントは、行継続を使用して複数行の間で拡張することはできません。

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

## <a name="identifiers"></a>識別子

*識別子*名前を指定します。 Visual Basic 識別子は、Unicode Standard Annex 15 は、1 つの例外に準拠している: 識別子はアンダー スコア (コネクタ) で始まる可能性があります。 識別子はアンダー スコアで始まっている場合、行の連結から区別するために他の少なくとも 1 つの有効な識別子文字を含めることが必要があります。

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

標準識別子が、キーワードに一致しないが、エスケープされた識別子または型の文字を識別子のことができます。 *識別子をエスケープ*の角かっこで区切られた識別子です。 キーワードに一致する可能性がありますが、型文字がない可能性がありますとする点を除いて、標準識別子と同じ規則識別子に従ってをエスケープします。

この例は、という名前のクラスを定義します`class`という名前の共有メソッドで`shared`という名前のパラメーターを受け取る`boolean`メソッドを呼び出します。

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

識別子は、2 つの識別子を大文字と小文字が異なる場合、同じ識別子であると見なされるように大文字小文字を区別します。 (__に注意してください。__ 識別子を比較するときに、Unicode の標準的な 1 対 1 大文字と小文字マッピングが使用し、ロケール固有の大文字小文字マップは無視されます。)


### <a name="type-characters"></a>型文字

A*文字入力*先行する識別子の種類を表します。 型文字は、識別子の一部とは見なされません。

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

型文字は、宣言自体で指定された型と一致する必要があります、宣言には、型文字が含まれている場合それ以外の場合、コンパイル時エラーが発生します。 宣言型が省略されている場合 (が指定されていない場合など、`As`句)、型文字は、宣言の型として暗黙的にします。

空白文字が識別子とその型の文字の間到達しない可能性があります。 型文字がない`Byte`、 `SByte`、 `UShort`、 `Short`、`UInteger`または`ULong`、適切な文字の不足が原因です。

概念的には、型 (たとえば、名前空間の名前) がありません識別子に、または型の文字の型と一致しない型を持つ識別子には、型文字を付加すると、コンパイル時エラーが発生します。

次の例は、型の文字の使用を示しています。

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

型文字`!`特別な問題が示されることで、型文字、および言語での区切り記号として使用できます。 あいまいさを削除する、`!`文字がそれに続く文字が識別子を開始できない限り、型文字。 可能な場合、`!`文字が区切り記号で型文字ではありません。


## <a name="keywords"></a>キーワード

A*キーワード*言語コンストラクトで特別な意味のある単語します。 すべてのキーワードは、言語によって予約されており、識別子をエスケープしない限りは識別子として使用することはできません。 (__に注意してください。__ `EndIf`、 `GoSub`、 `Let`、 `Variant`、および`Wend`Visual Basic では使用されなくがキーワードとして保持されます。)

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

## <a name="literals"></a>リテラル

A*リテラル*型の特定の値のテキスト表現です。 リテラル型には、ブール値、整数、浮動小数点、文字列、文字、および日付が含まれます。

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

### <a name="boolean-literals"></a>ブール型リテラル

`True` `False`のリテラルは、`Boolean`はそれぞれ、true と false の状態にマップする型。

```antlr
BooleanLiteral
    : 'True' | 'False'
    ;
```

### <a name="integer-literals"></a>整数リテラル

整数リテラルには、10 進数 (基数 10)、16 進数 (基数 16)、または 8 進数 (基数 8) を指定します。 10 進数の整数リテラルは、10 進数字 (0 ~ 9) の文字列です。 16 進数リテラルは、`&H`続けて 16 進数の数字 (0 ~ 9、A ~ F) の文字列。 8 進数のリテラルは、`&O`続けて 8 進数の数字 (0 ~ 7) の文字列。 10 進リテラルは、8 進数と 16 進数のリテラル、整数リテラルのバイナリ値を表しますが、直接、整数リテラルの 10 進値を表します (したがって、 `&H8000S` -32768、オーバーフロー エラーではありません)。

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

リテラルの型は、値、または次の型文字によって決定されます。 範囲の値の型文字が指定されていない場合、`Integer`型は、入力`Integer`; の範囲外の値`Integer`として入力された`Long`します。 整数リテラルの型がリテラル、整数を保持するために十分なサイズである場合、コンパイル エラーが発生します。 (__に注意してください。__ 型文字がない`Byte`が最も自然な文字になるので`B`、16 進数リテラルで有効な文字である)。


### <a name="floating-point-literals"></a>浮動小数点リテラル

浮動小数点リテラルは、整数リテラルの後に省略可能な 10 進数のポイント (ピリオドの ASCII 文字) と仮数と省略可能な基本 10 の指数部です。 既定では、浮動小数点リテラルは、型の`Double`します。 場合、 `Single`、 `Double`、または`Decimal`型文字を指定すると、その型のリテラルは、します。 浮動小数点リテラルの型が浮動小数点リテラルを保持するサイズが不十分である場合、コンパイル エラーが発生します。

__注意してください。__ いることに注意が、`Decimal`データ型は、後続の 0 値をエンコードできます。 仕様にで 0 が末尾かどうかについてのコメントはありません、`Decimal`リテラルはコンパイラによって受け入れられる必要があります。

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

### <a name="string-literals"></a>文字列リテラル。

文字列リテラルとは、0 個以上の Unicode のシーケンスの先頭文字し、ASCII 二重引用符文字で終わる、Unicode の左側二重引用符の文字または Unicode 右二重引用符文字です。 、文字列内では、2 つの二重引用符文字のシーケンスは、文字列内の二重引用符を表すエスケープ シーケンスです。

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

文字列定数は、`String`型。

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

コンパイラを許可して、文字列定数式をリテラル文字列に置き換えます。 各文字列リテラルは、新しい文字列インスタンスで必ずしも発生するされません。 バイナリ比較セマンティクスを使用して文字列の等値演算子に従って等しい 2 つ以上の文字列リテラルは、同じプログラム内で表示されたら、これらの文字列リテラルは可能性があります、同じ文字列インスタンスを参照してください。 たとえば、次のプログラムの出力を返す可能性があります`True`のため、2 つのリテラルは、同じ文字列インスタンスを参照してください。

```vb
Module Test
    Sub Main()
        Dim a As Object = "he" & "llo"
        Dim b As Object = "hello"
        Console.WriteLine(a Is b)
    End Sub
End Module
```


### <a name="character-literals"></a>文字リテラル

文字リテラルの 1 つの Unicode 文字を表す、`Char`型。 2 つの二重引用符文字は、二重引用符の文字を表すエスケープ シーケンスです。

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


### <a name="date-literals"></a>日付リテラル

日付リテラルが、特定の時点での値で表された時刻を表す、`Date`型。

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

リテラルには、日付と時刻、日付だけ、または時刻だけの両方を指定できます。 日付の値を省略した場合、構成のグレゴリオ暦カレンダーで 1 年 1 月 1 日と見なされます。 時刻の値を省略すると、12時 00分: 00 AM が使われます。

日付値の年の値の解釈で問題を避けるためには、年の値は 2 桁の数字にすることはできません。 最初の世紀 AD/CE の日付を表現する場合、先行ゼロを指定する必要があります。

24 時間の値または; 12 時間値を使用時間の値を指定することがあります。時刻の値を省略する、`AM`または`PM`は 24 時間の値であると見なされます。 時間の値が、分、リテラルを省略するかどうかは`0`既定で使用されます。 時間の値がリテラル (秒) を省略するかどうかは`0`既定で使用されます。 分と秒の両方を省略した場合、し`AM`または`PM`指定する必要があります。 範囲外に指定された日付値が、`Date`入力すると、コンパイル時エラーが発生します。

次の例には、いくつかの日付リテラルが含まれています。

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


### <a name="nothing"></a>Nothing

`Nothing` 特別なリテラルです。型がないと、型パラメーターを含む、型システムですべての型に変換可能です。 特定の型に変換するときは、その型の既定値に相当します。

```antlr
Nothing
    : 'Nothing'
    ;
```

## <a name="separators"></a>[区切り記号]

次の ASCII 文字には、区切り記号がされます。

```antlr
Separator
    : '(' | ')' | '{' | '}' | '!' | '#' | ',' | '.' | ':' | '?'
    ;
```

## <a name="operator-characters"></a>演算子文字

次の ASCII 文字または文字のシーケンス演算子を表します。

```antlr
Operator
    : '&' | '*' | '+' | '-' | '/' | '\\' | '^' | '<' | '=' | '>'
    ;
```

