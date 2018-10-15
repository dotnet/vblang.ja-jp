# <a name="source-files-and-namespaces"></a>ソース ファイルと名前空間

Visual Basic プログラムは、1 つまたは複数のソース ファイルで構成されます。 すべてのソース ファイルがまとめて; 処理プログラムがコンパイルされると、そのため、ソース ファイルは、前方宣言必要はありません、循環形式で可能性があります、互いに依存することができます。 プログラム テキスト内の宣言の順序は、一般にあまり意味はありません。

ソース ファイルは、ステートメントのオプション、import ステートメント、および属性は、次の名前空間の本体のオプションのセットで構成されます。 属性は、いずれかがある各する必要があります、`Assembly`または`Module`修飾子は、.NET アセンブリ、またはコンパイルによって生成されたモジュールに適用されます。 ソース ファイルの本文は、グローバル名前空間、つまりソース ファイルの最上位レベルにあるすべての宣言がグローバル名前空間内に配置される、暗黙的な名前空間宣言として機能します。 例えば:

FileA.vb:

```vb
Class A
End Class
```

FileB.vb:

```vb
Class B
End Class
```

この場合、完全修飾名を持つ 2 つのクラスを宣言する、グローバル名前空間を 2 つのソース ファイルが投稿`A`と`B`します。 同じ宣言領域に 2 つのソース ファイルがドキュメントに投稿されているとエラーと同じ名前のメンバーの宣言をそれぞれに含まれる場合。

__注意してください。__ コンパイル環境は、ソース ファイルを暗黙的に配置する名前空間宣言をオーバーライドすることができます。

場合を除き、行終端記号またはコロンのいずれか、Visual Basic プログラム内のステートメントを終了できます。

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

## <a name="program-startup-and-termination"></a>プログラムの起動と終了

プログラムの起動は、実行環境がプログラムのと呼ばれる、指定されたメソッドを実行するときに発生します。*エントリ ポイント*します。 名前付き常にこのエントリ ポイント メソッド`Main`、共有する必要があります、ジェネリック型に含めることはできませんに async 修飾子を含めることはできません、シグネチャは次のいずれかが必要があります。

```vb
Sub Main()
Sub Main(args() As String)
Function Main() As Integer
Function Main(args() As String) As Integer
```

エントリ ポイント メソッドのアクセシビリティは関係ありません。 プログラムには、複数の適切なエントリ ポイントが含まれている場合、コンパイル環境は、エントリ ポイントとして 1 つを指定する必要があります。 それ以外の場合は、コンパイル時のエラーが発生します。 コンパイル環境は、1 つが存在しない場合、エントリ ポイント メソッドを作成もできます。

エントリ ポイントは、パラメーターを持つ場合、プログラムを開始、実行環境によって提供される引数には、文字列として表されるプログラムにコマンドライン引数が含まれています。 エントリ ポイントがある戻り値の型の場合`Integer`、実行環境に、プログラムの結果として、関数から返される値が返されます。

その他のすべての点では、エントリ ポイント メソッドは、他のメソッドと同様に動作します。 実行が、実行環境によって行われたエントリ ポイント メソッドの呼び出しを離れると、プログラムを終了します。

## <a name="compilation-options"></a>コンパイル オプション

ソース ファイルは、ソース コードを使用してコンパイル オプションを指定できます*オプション ステートメント*します。

```antlr
OptionStatement
    : OptionExplicitStatement
    | OptionStrictStatement
    | OptionCompareStatement
    | OptionInferStatement
    ;
```

`Option`ステートメントが、それが表示されたら、ソース ファイルとの種類ごとの 1 つだけにのみ適用されます`Option`ステートメントは、ソース ファイルになっている可能性があります。 例えば:

```vb
Option Strict On
Option Compare Text
Option Strict Off    ' Not allowed, Option Strict is already specified.
Option Compare Text  ' Not allowed, Option Compare is already specified.
```

次の 4 つのコンパイル オプションがあります。 厳密な型のセマンティクス、明示的な宣言のセマンティクス、比較のセマンティクス、およびローカル変数の型の推論のセマンティクスです。 ソース ファイルには特定が含まれていない場合`Option`ステートメントでは、次のコンパイル環境を決定する特定のセットのセマンティクスが使用されます。 5 番目のコンパイル オプションのコンパイル環境でのみ指定できる整数オーバーフローのチェックもあります。


### <a name="option-explicit-statement"></a>Option Explicit ステートメント

`Option Explicit`ステートメントは、ローカル変数が暗黙的に宣言されたかどうかを決定します。 キーワード`On`または`Off`が次のステートメントはどちらも指定しない場合、既定値は`On`します。 ファイル内のステートメントが指定されていない場合は、どちらを使用するコンパイル環境が決定します。

```antlr
OptionExplicitStatement
    : 'Option' 'Explicit' OnOff? StatementTerminator
    ;

OnOff
    : 'On' | 'Off'
    ;
```


__注意してください。__ `Explicit` `Off`予約された単語は使用されません。

```vb
Option Explicit Off

Module Test
    Sub Main()
        x = 5 ' Valid because Option Explicit is off.
    End Sub
End Module
```

この例では、ローカル変数で`x`に割り当てることで暗黙的に宣言されています。 `x` の型は `Object` です。


### <a name="option-strict-statement"></a>Option Strict Statement

`Option Strict`ステートメントを決定するかどうか変換と操作に`Object`は厳密なまたは制限の緩やかな型のセマンティクスと型として暗黙的に型指定するかどうかによって管理されます`Object`いない場合`As`句を指定します。 キーワードで、ステートメントの後にこと`On`または`Off`、どちらも指定されている場合、既定値は`On`します。 ファイル内のステートメントが指定されていない場合は、どちらを使用するコンパイル環境が決定します。

```antlr
OptionStrictStatement
    : 'Option' 'Strict' OnOff? StatementTerminator
    ;
```

__注意してください。__ `Strict` `Off`予約された単語は使用されません。

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

厳密な型は、次は許可されません。

* 明示的なキャスト演算子がない縮小変換します。

* 遅延バインディング。

* 型に対する操作`Object`以外`TypeOf`.`Is`、 `Is`、および`IsNot`します。

* 省略すると、`As`推論された型がない宣言内の句。


### <a name="option-compare-statement"></a>Option Compare ステートメント

`Option Compare`ステートメントは、文字列比較のセマンティクスを決定します。 文字列比較が実行するか (これで各文字のバイナリの Unicode 値と比較されます)、バイナリの比較またはテキストの比較 (で各文字の構文の意味と比較して、現在のカルチャを使用して) を使用します。 ファイル内のステートメントが指定されていない場合のコンパイル環境は、比較の種類が使用を制御します。

```antlr
OptionCompareStatement
    : 'Option' 'Compare' CompareOption StatementTerminator
    ;

CompareOption
    : 'Binary' | 'Text'
    ;
```

__注意してください。__ `Compare`、 `Binary`、および`Text`予約された単語は使用されません。

```vb
Option Compare Text

Module Test
    Sub Main()
        Console.WriteLine("a" = "A")    ' Prints True.
    End Sub
End Module
```

文字列比較を実行するこの例では、大文字小文字の違いを無視する文字列比較を使用します。 場合`Option Compare Binary`これが印刷し、指定されている`False`します。


### <a name="integer-overflow-checks"></a>整数オーバーフローのチェック

整数演算するチェックか、実行時にオーバーフロー状態をチェックされません。 オーバーフロー状態がチェックされ、整数演算がオーバーフローした場合、`System.OverflowException`例外がスローされます。 オーバーフロー状態がチェックされていない場合、整数演算のオーバーフロー時に例外はスローされません。 コンパイル環境では、このオプションがオンかオフかどうかを決定します。

### <a name="option-infer-statement"></a>Option Infer ステートメント

`Option Infer`ステートメントは、ローカルかどうかを決定します。 変数の宣言を持たず`As`句は、推論された型または使用がある`Object`します。 キーワードで、ステートメントの後にこと`On`または`Off`、どちらも指定されている場合、既定値は`On`します。 ファイル内のステートメントが指定されていない場合は、どちらを使用するコンパイル環境が決定します。

```antlr
OptionInferStatement
    : 'Option' 'Infer' OnOff? StatementTerminator
    ;
```

__注意してください。__ `Infer` `Off`予約された単語は使用されません。

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


## <a name="imports-statement"></a>Imports ステートメント

`Imports` ステートメントは、名の修飾せずに参照を使用できるように、ソース ファイルにエンティティの名前をインポートまたは XML の式で使用する名前空間をインポートします。

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

メンバー宣言を含むソース ファイル内で、`Imports`次の例に示すようにステートメントでは、指定した名前空間に含まれる型を直接参照できます。

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

ここでは、ソース ファイル内で、名前空間の型のメンバー`N1.N2`直接利用し、したがってクラス`N3.B`クラスから派生した`N1.N2.A`します。

`Imports` 後のステートメントを表示する必要があります`Option`ステートメントが任意の前に宣言を入力します。 コンパイル環境が暗黙的な定義も`Imports`ステートメント。

`Imports` ステートメントは、ソース ファイルで名前を使用できるようにしますが、グローバル名前空間の宣言領域内のあらゆるものを宣言しません。 インポートされた名前のスコープ、`Imports`ステートメントは、ソース ファイルに含まれる名前空間のメンバーの宣言に拡張します。 スコープ、`Imports`ステートメント具体的にが含まれない他`Imports`ステートメント、およびその他のソース ファイルが含まします。 `Imports` ステートメントは、互いには参照できません。

この例では、最後に`Imports`最初のインポート エイリアスに影響を受けないために、ステートメントがエラーでは。

```vb
Imports R1 = N1 ' OK.
Imports R2 = N1.N2 ' OK.
Imports R3 = R1.N2 ' Error: Can't refer to R1.

Namespace N1.N2
End Namespace
```

__注意してください。__ 表示される名前空間または型名`Imports`完全修飾している場合、ステートメントが常に扱われます。 名前空間または型名の一番左の識別子を常にグローバル名前空間に解決し、解像度の残りの部分は、通常の名前解決ルールに従って実行されます。 これは、このようなルールが適用される言語で唯一の場所この規則により、名前を完全に修飾から隠さことはできません。 規則がない場合は、グローバル名前空間の名前が特定のソース ファイルで非表示にできなくなる修飾の方法でその名前空間から任意の名前を指定することです。

この例で、`Imports`ステートメントは常には、グローバルに参照`System`名前空間、およびソース ファイル内のクラスではなく。

```vb
Imports System   ' Imports the namespace, not the class.

Class System
End Class
```


### <a name="import-aliases"></a>インポート エイリアス

*インポート エイリアス*名前空間または型のエイリアスを定義します。

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

ここでは、ソース ファイル内で、`A`の別名です`N1.N2.A`、クラスと`N3.B`クラスから派生した`N1.N2.A`します。 別名を作成して、同じ効果を取得できる`R`の`N1.N2`し、参照する`R.A`:

```vb
Imports R = N1.N2

Namespace N3
    Class B
        Inherits R.A
    End Class
End Namespace
```

インポート エイリアスの識別子は、グローバル名前空間 (だけでなく、グローバル名前空間宣言で、そのインポート エイリアスが定義されているソース ファイル) の宣言領域内で一意である必要があります、グローバル名前空間の名前で宣言されていない場合でも宣言領域です。

__注意してください。__ モジュール内の宣言は、コンテナーの宣言領域に名前を持ち込んでいません。 、つまりインポート エイリアスと同じ名前を指定するモジュールでの宣言に有効な場合でも、宣言の名前がコンテナーの宣言領域にアクセスできます。

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

メンバーが既にグローバル名前空間に含まれているここでは、`A`ので、その識別子を使用するインポート エイリアスがエラーになります。 同じ名前のエイリアスを宣言する、同じソース ファイルのインポート エイリアスを 2 つ以上のエラーでは同様にします。

インポート エイリアスは、任意の名前空間または型の別名を作成できます。 名前空間または型にエイリアスを使ってアクセスするには、名前空間または型をその宣言された名前を介してアクセスするとまったく同じ結果が得られます。

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

ここでは、名前`N1.N2.A`、 `R1.N2.A`、および`R2.A`の完全修飾名がクラスには等価であり、すべて参照`N1.N2.A`します。

インポートには、名前空間または別名を作成する型の正確な名前を指定します。 その名前空間または型の正確な完全修飾名があります: (インスタンスの派生クラスから基底クラスのメンバーへのアクセスを許可する) を修飾名前解決の通常の規則を使用しません。

型またはこれらのルールでは解決できない名前空間にインポート エイリアス ポイントし、import ステートメントは無視されます (または、コンパイラは警告)。

また、参照がオープン ジェネリック型にすることはできません--すべてのジェネリック型は指定するには、有効な型引数が必要し、すべての型引数は、上記のルールによって解決する必要があります。 ジェネリック型の任意の不正なバインディングでは、エラーになります。

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

ソース ファイル内の宣言は、インポート エイリアス名をシャドウすることができます。

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

前の例の参照を`R.A`の宣言で`B`ので、エラーが発生`R`を指す`N3.R`ではなく、`N1.N2`します。

インポート エイリアスは、特定のソース ファイル内で使用可能なエイリアスが、基になる宣言領域に新しいメンバーは含まれません。 つまり、インポート エイリアスは、推移的ではありませんではなくが発生するソース ファイルのみに影響します。

file1.vb:

```vb
Imports R = N1.N2

Namespace N1.N2
    Class A
    End Class
End Namespace
```

file2.vb:

```vb
Class B
    Inherits R.A ' Error, R unknown.
End Class
```

上記の例では、インポート エイリアスのスコープを導入するため、`R`が含まれるソース ファイル内の宣言にのみ拡張`R`2 番目のソース ファイルでは不明です。


### <a name="namespace-imports"></a>名前空間のインポート

A*名前空間インポート*名前空間または修飾なしで使用される型の各メンバーの識別子を許可する名前空間または型のメンバーのすべてをインポートします。 型の場合、名前空間のインポートはのみ、クラス名の修飾を必要とせず、型の共有メンバーへのアクセスを許可します。 具体的には、修飾なしで使用される列挙型のメンバーを使用します。

```antlr
MembersImportsClause
    : TypeName
    ;
```

例えば:

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

インポート エイリアスとは異なり、名前空間インポート制限はありませんをインポートし、名前空間と型を持つ識別子は、グローバル名前空間内で既に宣言されてインポートが名前にします。 インポート エイリアスとソース ファイル内の宣言では、通常のインポートでインポートされた名前が影付き。

次の例では、`A`を指す`N3.A`なく`N1.N2.A`メンバー宣言内で、`N3`名前空間。

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

1 つ以上のインポートされた名前空間には、同じ名前でメンバーが含まれます (とその名前がそれ以外の場合、インポート エイリアスまたは宣言でシャドウ)、その名前への参照はあいまいであり、コンパイル時エラーが発生します。

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

上記の例では、どちらも`N1`と`N2`、メンバーを含んで`A`します。 `N3`を参照する、両方をインポート`A`で`N3`コンパイル時エラーが発生します。 このような状況で、競合はへの参照の修飾を使用するか解決できる`A`、または特定を取得するインポート エイリアスを導入することで`A`、次の例。

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

のみ名前空間、クラス、構造体、列挙型、および標準的なモジュールをインポートすることがあります。


### <a name="xml-namespace-imports"></a>XML Namespace のインポート

*XML 名前空間インポート*名前空間または修飾されていない XML 表現がコンパイル単位内に含まれる既定の名前空間を定義します。

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

例えば:

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

既定の名前空間を含む、XML 名前空間は、インポートの特定のセットに対して 1 回だけ定義できます。 例えば:

```vb
Imports <xmlns:db="http://example.org/database-one">
' Error: namespace db is already defined
Imports <xmlns:db="http://example.org/database-two">
```


## <a name="namespaces"></a>名前空間

Visual Basic プログラムの名前空間を使用して構成します。 名前空間両方内部的には、プログラムを整理するだけでなくプログラム要素が他のプログラムを公開する方法を整理します。

他のエンティティとは異なり、名前空間は終わりし、同じプログラム内および同じ名前空間のメンバーの貢献をしている各宣言に、多くのプログラム間で複数回を宣言すること。 次の例では、2 つの名前空間宣言が完全修飾の名前を持つ 2 つのクラスを宣言する、同じ宣言領域に貢献`N1.N2.A`と`N1.N2.B`します。

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

2 つの宣言は、同じ宣言領域に追加、ため、各エラーには、同じ名前のメンバーの宣言が含まれているでしょう。

名なしおよびが入れ子になった名前空間と型は常に修飾なしでアクセスを持つグローバル名前空間があります。 グローバル名前空間で宣言された名前空間のメンバーのスコープは、プログラム全体のテキストです。 それ以外の場合、型または名前空間のスコープの完全修飾名は名前空間で宣言されている`N`が対応する名前空間の完全修飾名の先頭の各名前空間のプログラム テキスト`N`または`N`自体。 (既定では、特定の名前空間の宣言を配置する、コンパイラが選択できることに注意してください。 これは変更されません、名前のないグローバル名前空間がまだ残っているという事実です。)

この例では、クラスで`B`クラスをご覧`A`ため`B`の名前空間`N1.N2.N3`は概念的には、名前空間内で入れ子になった`N1.N2`します。

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

### <a name="namespace-declarations"></a>名前空間の宣言

名前空間宣言の 3 つの形式はあります。

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

キーワードから始まり、最初のフォーム`Namespace`に続けて相対名前空間の名前。 空間の相対名が修飾されている場合は、構文的修飾名では、各名前に対応する名前空間宣言内で入れ子になった場合、名前空間宣言が扱われます。 たとえば、次の 2 つの名前空間は意味的に同等です。

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

2 番目の形式は、キーワードで始まる`Namespace Global`します。 すべてのメンバー宣言は、グローバル名前なし名前空間 - コンパイル環境で提供されるすべての既定値に関係なくで構文的配置された場合、ように扱われます。 その他の任意の名前空間宣言内でこの形式の名前空間宣言が構文的入れ子にできません可能性があります。

3 番目の形式がキーワードで始まる`Namespace Global`修飾された識別子に続けて`N`します。 最初のフォームの名前空間宣言の場合と同様に扱われます"`Namespace N`"、グローバル名前なし名前空間 - コンパイル環境で提供されるすべての既定値に関係なくに格納された構文です。 その他の任意の名前空間宣言内でこの形式の名前空間宣言が構文的入れ子にできません可能性があります。

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

名前空間のメンバーを扱う場合、重要ではありません、特定のメンバーが宣言されています。 2 つのプログラムは、同じ名前空間で同じ名前のエンティティを定義する名前空間名を解決しようとすると、あいまいさエラーが発生します。

名前空間は本質的に`Public`ので、名前空間の宣言は、アクセス修飾子を含めることはできません。


### <a name="namespace-members"></a>Namespace メンバー

Namespace メンバーは名前空間宣言をして、宣言の型のみです。 型宣言があります`Public`または`Friend`アクセスします。 型の既定のアクセスは`Friend`アクセスします。

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
