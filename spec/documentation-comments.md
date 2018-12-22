# <a name="documentation-comments"></a>ドキュメントのコメント

ドキュメントのコメントは、特殊な形式のコメントは接続されているコードに関するドキュメントを生成するために分析できるソースです。 ドキュメントのコメントの基本的な形式は、XML です。 ときにドキュメントのコメントを使用してコンパイル コードをコンパイラ可能性があります必要に応じて出力ソース ドキュメントのコメントの合計を表す XML ファイル。 この XML ファイルは、印刷物やオンライン ドキュメントを生成するために他のツールで使用できます。

この章では、ドキュメントのコメントを記述し、XML タグを使用したドキュメントのコメントをお勧めします。

## <a name="documentation-comment-format"></a>ドキュメント コメント形式

ドキュメント コメントは、特殊なコメントで始まる`'''`、3 つのマークで一重引用符。 直前に、ドキュメント化 (クラス、デリゲート、またはインターフェイス) などの型または型のメンバー (フィールド、イベント、プロパティ、メソッドなど) を指定する必要があります。 1 つを使用する必要がある場合、その本文を提供するメソッドのドキュメント コメントで部分メソッド宣言でドキュメントのコメントが置き換えられます。 すべての隣接するドキュメントのコメントは、1 つのドキュメントのコメントを生成するために一緒に追加されます。 続く、空白文字がある場合、`'''`文字、連結したものにその空白文字は含まれません。 例えば:

```vb
''' <remarks>
''' Class <c>Point</c> models a point in a two-dimensional plane.
''' </remarks>
Public Class Point 
   ''' <remarks>
   ''' Method <c>Draw</c> renders the point.
   ''' </remarks>
   Sub Draw()
   End Sub
End Class
```

ドキュメントのコメントをする必要がありますが、整形式 XML に従って http://www.w3.org/TR/REC-xmlします。 XML の形式が正しくない場合、は、警告が生成され、ドキュメント ファイルは、エラーが発生したことを示すコメントが含まれます。

開発者は、独自のタグのセットを作成できますが、推奨される設定は、次のセクションで定義されます。 推奨されるタグの一部には特別な意味があります。

* `<param>`タグを使用して、パラメーターを説明します。 指定されたパラメーターを`<param>`タグが存在し、型のメンバーのすべてのパラメーターは、ドキュメント コメントで説明する必要があります。 いずれかの条件が true でない場合、コンパイラは警告を発行します。

* `cref` 属性は任意のタグにアタッチでき、コード要素への参照を提供します。 コード要素が存在する必要があります。コンパイル時に、コンパイラは、名前をメンバーを表す ID 文字列に置き換えます。 コード要素が存在しない場合、コンパイラは警告を発行します。 説明されている名前を検索するときに、`cref`属性は、コンパイラの点では`Imports`ステートメントを含むソース ファイル内に表示されます。

* `<summary>`タグは、型またはメンバーに関する情報を表示する、ドキュメント ビューアーで使用されます。

ドキュメント ファイルがのみに含まれるドキュメントのコメントの型とメンバーに関する完全な情報を提供しないことに注意してください。 型またはメンバーに関する詳細情報を表示するには、実際の型またはメンバーのリフレクションと組み合わせてドキュメント ファイルを使用する必要があります。

## <a name="recommended-tags"></a>推奨されるタグ

ドキュメント ジェネレーターは、そのまま使用し、XML の規則に従って有効な任意のタグを処理する必要があります。 次のタグによって、ユーザー ドキュメントで一般的に使用される機能が与えられます。

`<c>` コードのようなフォントのテキストを設定します。

`<code>` コードのようなフォントで 1 つまたは複数の行のソース コードまたはプログラムの出力を設定します。

`<example>` 例を示します

`<exception>` メソッドがスローできる例外を識別します。

`<include>` 外部の XML ドキュメントが含まれています

`<list>` リストまたはテーブルを作成します。

`<para>` により、テキストに追加される構造体

`<param>` メソッドまたはコンス トラクターのパラメーターについて説明します

`<paramref>` 単語がパラメーター名を識別します。

`<permission>` メンバーのセキュリティのユーザー補助を説明します。

`<remarks>` 型を記述します

`<returns>` メソッドの戻り値について説明します

`<see>` リンクを指定します

`<seealso>` 「参照」エントリを生成します。

`<summary>` 型のメンバーについて説明します

`<typeparam>` 型パラメーターについて説明します

`<value>` プロパティについて説明します

### <a name="ltcgt"></a>&lt;c&gt;

このタグは、説明内のテキストの一部がコードのブロックを使用するようなフォントを使用することを指定します。 (実際のコード行を使用して`<code>`)。

__構文:__

```xml
<c>text to be set like code</c>
```

__例:__

```vb
''' <remarks>
''' Class <c>Point</c> models a point in a two-dimensional plane.
''' </remarks>
Public Class Point 
End Class
```

### <a name="ltcodegt"></a>&lt;code&gt;

このタグは、ソース コードまたはプログラムの出力の 1 つまたは複数の行が固定幅フォントを使用することを指定します。 (小さなコードのフラグメントを使用して、 `<c>`)。

__構文:__

```xml
<code>source code or program output</code>
```

__例:__

```vb
''' <summary>
''' This method changes the point's location by the given x- and 
''' y-offsets.
''' <example>
''' For example:
''' <code>
'''    Dim p As Point = New Point(3,5)
'''    p.Translate(-1,3)
''' </code>
''' results in <c>p</c>'s having the value (2,8).
''' </example>
''' </summary>
Public Sub Translate(x As Integer, y As Integer)
    Me.x += x
    Me.y += y
End Sub
```

### <a name="ltexamplegt"></a>&lt;example&gt;

このタグは、要素の使用方法を説明するコメント内のコード例を使用します。 通常、このプロセスでは、タグの使用`<code>`もします。

__構文:__

```xml
<example>description</example>
```

__例:__

例については、「`<code>`」を参照してください。

### <a name="ltexceptiongt"></a>&lt;exception&gt;

このタグは、メソッドがスローできる例外を文書化する方法を提供します。

__構文:__

```xml
<exception cref="member">description</exception>
```

__例:__

```vb
Public Module DataBaseOperations
    ''' <exception cref="MasterFileFormatCorruptException"></exception>
    ''' <exception cref="MasterFileLockedOpenException"></exception>
    Public Sub ReadRecord(flag As Integer)
        If Flag = 1 Then
            Throw New MasterFileFormatCorruptException()
        ElseIf Flag = 2 Then
            Throw New MasterFileLockedOpenException()
        End If
        ' ...
    End Sub
End Module
```

### <a name="ltincludegt"></a>&lt;include&gt;

このタグを使用して、外部の整形式 XML ドキュメントからの情報が含まれます。 XPath 式は、XML に追加する内容、ドキュメントからを指定する XML ドキュメントに適用されます。 `<include>`タグは、選択した外部のドキュメントから XML に置き換えられます。

__構文:__

```xml
<include file="filename" path="xpath">
```

__例:__

場合は、ソース コードには、次のような宣言が含まれています。

```vb
''' <include file="docs.xml" path="extra/class[@name="IntList"]/*" />
```

外部ファイル docs.xml にあり、次の内容

```xml
<?xml version="1.0"?>
<extra>
    <class name="IntList">
        <summary>
            Contains a list of integers.
        </summary>
    </class>
    <class name="StringList">
        <summary>
            Contains a list of strings.
        </summary>
    </class>
</extra>
```

同じドキュメントは、ソース コードが含まれている場合、出力を示します。

```xml
''' <summary>
''' Contains a list of integers.
''' </summary>
```

### <a name="ltlistgt"></a>&lt;リスト&gt;

このタグは、リストまたは項目のテーブルの作成に使用されます。 `<listheader>`のテーブルまたは定義の一覧の見出し行を定義するブロック。 (テーブルを定義するときに、見出しの用語のエントリのみを指定します。)

リスト内の各項目を指定した、`<item>`ブロックします。 定義リストを作成するときに用語と説明の両方を指定する必要があります。 ただし、テーブル、箇条書きまたは番号付きリストの説明のみを指定する必要があります。

__構文:__

```xml
<list type="bullet" | "number" | "table">
    <listheader>
        <term>term</term>
        <description>description</description>
    </listheader>
    <item>
        <term>term</term>
        <description>description</description>
    </item>
    ...
    <item>
        <term>term</term>
        <description>description</description>
    </item>
</list>
```

__例:__

```vb
Public Class TestClass
    ''' <remarks>
    ''' Here is an example of a bulleted list:
    ''' <list type="bullet">
    '''     <item>
    '''        <description>Item 1.</description>
    '''     </item>
    '''     <item>
    '''         <description>Item 2.</description>
    '''     </item>
    ''' </list>
    ''' </remarks>
    Public Shared Sub Main()
    End Sub
End Class
```

### <a name="ltparagt"></a>&lt;para&gt;

このタグは、その他のタグ内で使用できるよう`<remarks>`または`<returns>`、構造体をテキストに追加することができます。

__構文:__

```xml
<para>content</para>
```

__例:__

```vb
''' <summary>
''' This is the entry point of the Point class testing program.
''' <para>This program tests each method and operator, and
''' is intended to be run after any non-trvial maintenance has
''' been performed on the Point class.</para>
''' </summary>
Public Shared Sub Main()
End Sub
```

### <a name="ltparamgt"></a>&lt;param&gt;

このタグには、メソッド、コンス トラクター、またはインデックス付きプロパティのパラメーターについて説明します。

__構文:__

```xml
<param name="name">description</param>
```

__例:__

```vb
''' <summary>
''' This method changes the point's location to the given
''' coordinates.
''' </summary>
''' <param name="x"><c>x</c> is the new x-coordinate.</param>
''' <param name="y"><c>y</c> is the new y-coordinate.</param>
Public Sub Move(x As Integer, y As Integer)
    Me.x = x
    Me.y = y
End Sub
```

### <a name="ltparamrefgt"></a>&lt;paramref&gt;

このタグは、単語がパラメーターであることを示します。 ドキュメント ファイルを処理することで、何らかの方法でこのパラメーターの書式を設定します。

__構文:__

```xml
<paramref name="name"/>
```

__例:__

```vb
''' <summary>
''' This constructor initializes the new Point to
''' (<paramref name="x"/>,<paramref name="y"/>).
''' </summary>
''' <param name="x"><c>x</c> is the new Point's x-coordinate.</param>
''' <param name="y"><c>y</c> is the new Point's y-coordinate.</param>
Public Sub New(x As Integer, y As Integer)
    Me.x = x
    Me.y = y
End Sub
```

### <a name="ltpermissiongt"></a>&lt;permission&gt;

このタグは、メンバーのセキュリティのアクセシビリティをドキュメントします。

__構文:__

```xml
<permission cref="member">description</permission>
```

__例:__

```vb
''' <permission cref="System.Security.PermissionSet">Everyone can
''' access this method.</permission>
Public Shared Sub Test()
End Sub
```

### <a name="ltremarksgt"></a>&lt;remarks&gt;

このタグは、型に関する概要情報を指定します。 (使用`<summary>`に型のメンバーを示しています)。

__構文:__

```xml
<remarks>description</remarks>
```

__例:__

```vb
''' <remarks>
''' Class <c>Point</c> models a point in a two-dimensional plane.
''' </remarks>
Public Class Point 
End Class
```

### <a name="ltreturnsgt"></a>&lt;returns&gt;

このタグには、メソッドの戻り値について説明します。

__構文:__

```xml
<returns>description</returns>
```

__例:__

```vb
''' <summary>
''' Report a point's location as a string.
''' </summary>
''' <returns>
''' A string representing a point's location, in the form (x,y), without
''' any leading, training, or embedded whitespace.
''' </returns>
Public Overrides Function ToString() As String
    Return "(" & x & "," & y & ")"
End Sub
```

### <a name="ltseegt"></a>&lt;see&gt;

このタグは、テキスト内で指定するためのリンクを使用します。 (使用`<seealso>`「参照」セクションに表示するテキストを示します。)

__構文:__

```xml
<see cref="member"/>
```

__例:__

```vb
''' <summary>
''' This method changes the point's location to the given
''' coordinates.
''' </summary>
''' <see cref="Translate"/>
Public Sub Move(x As Integer, y As Integer)
    Me.x = x
    Me.y = y
End Sub

''' <summary>
''' This method changes the point's location by the given x- and
''' y-offsets.
''' </summary>
''' <see cref="Move"/>
Public Sub Translate(x As Integer, y As Integer)
    Me.x += x
    Me.y += y
End Sub
```

### <a name="ltseealsogt"></a>&lt;seealso&gt;

このタグは、「参照」セクションのエントリをを生成します。 (使用`<see>`からテキスト内のリンクを指定します)。

__構文:__

```xml
<seealso cref="member"/>
```

__例:__

```vb
''' <summary>
''' This method determines whether two Points have the same location.
''' </summary>
''' <seealso cref="operator=="/>
''' <seealso cref="operator!="/>
Public Overrides Function Equals(o As Object) As Boolean
    ' ...
End Function
```

### <a name="ltsummarygt"></a>&lt;summary&gt;

このタグには、型のメンバーについて説明します。 (使用`<remarks>`型自体を記述する)。

__構文:__

```xml
<summary>description</summary>
```

__例:__

```vb
''' <summary>
''' This constructor initializes the new Point to (0,0).
''' </summary>
Public Sub New()
    Me.New(0,0)
End Sub
```

### <a name="lttypeparamgt"></a>&lt;typeparam&gt;

このタグには、型パラメーターについて説明します。

__構文:__

```xml
<typeparam name="name">description</typeparam>
```

__例:__

```vb
''' <typeparam name="T">
''' The base item type. Must implement IComparable.
''' </typeparam>
Public Class ItemManager(Of T As IComparable)
End Class
```

### <a name="ltvaluegt"></a>&lt;value&gt;

このタグには、プロパティについて説明します。

__構文:__

```xml
<value>property description</value>
```

__例:__

```vb
''' <value>
''' Property <c>X</c> represents the point's x-coordinate.
''' </value>
Public Property X() As Integer
    Get
        Return _x
    End Get
    Set (Value As Integer)
        _x = Value
    End Set
End Property
```

## <a name="id-strings"></a>ID 文字列

ドキュメント ファイルを生成するときに、コンパイラは、一意に識別されているドキュメントのコメントとタグ付けされているソース コード内の各要素の ID 文字列を生成します。 この ID 文字列は、ドキュメントのコメントに対応するコンパイル済みアセンブリ内のどの要素を識別するために、外部ツールで使用できます。

ID 文字列は、次のように生成されます。

文字列に空白は配置されません。

文字列の最初の部分では、単一の文字の後にコロンを使用して、記述されているメンバーの種類を識別します。 その後にかっこで囲まれた対応する文字で、次の種類のメンバーを定義: イベント (E) フィールド (F) コンス トラクターと演算子 (M)、名前空間 (N)、(P) のプロパティと型 (T) を含むメソッド。 感嘆符 (!) は、ID の文字列を生成中にエラーが発生しました。 文字列の残りの部分は、エラーに関する情報を提供します。 を示します。

文字列の 2 番目の部分は、グローバル名前空間、要素の完全修飾名です。 要素、それを囲む型、および名前空間の名前は、ピリオドで区切られます。 項目自体の名前にピリオドがある場合は、置き換えられる、シャープ記号 (#)。 (これと見なされます要素の名前にはこの文字がありません。)型パラメーターを持つ型の名前は、型の型パラメーターの数を表す数値を続けて、バッククォート (') で終わります。 それらが格納された型の型パラメーターに入れ子にされた型にアクセスし、入れ子にされた型が暗黙的にその親の型の型パラメーターを含めることがそれらの型がこのが型パラメーターの合計に反映させるための点に注意することが重要大文字にします。

メソッドとプロパティの引数は、引数には、次のように、かっこで囲まれているが一覧表示します。 引数を指定せずに、かっこを省略します。 引数はコンマで区切られます。 各引数のエンコーディング CLI シグネチャと同じとおりです。引数は、完全修飾名で表されます。 たとえば、`Integer`になります`System.Int32`、`String`なります`System.String`、`Object`になります`System.Object`など。 引数、`ByRef`修飾子が、' @'、型名の後です。 引数、 `ByVal`、`Optional`または`ParamArray`修飾子には特殊な表記はあるありません。 引数には、配列として表されます`[lowerbound:size, ..., lowerbound:size]`コンマの数がランク - 1 で、下限と各次元のサイズがわかっている場合が 10 進数で表されます。 下限またはサイズが指定されていない場合は省略されます。 特定の次元で下限およびサイズが省略されている場合は、':' も省略されます。 配列の配列は 1 つで表されます"`[]`"個々 のレベル。

### <a name="id-string-examples"></a>ID 文字列の例

次の例については、ドキュメントのコメントを持つことのできる各ソース要素から生成される ID 文字列と共に、VB コードのフラグメントを示します。

型は、完全修飾名で表されます。

```vb
Enum Color
    Red
    Blue
    Green
End Enum

Namespace Acme
    Interface IProcess
    End Interface

    Structure ValueType
        ...
    End Structure

    Class Widget
        Public Class NestedClass
        End Class

        Public Interface IMenuItem
        End Interface

        Public Delegate Sub Del(i As Integer)

        Public Enum Direction
            North
            South
            East
            West
        End Enum
    End Class
End Namespace

"T:Color"
"T:Acme.IProcess"
"T:Acme.ValueType"
"T:Acme.Widget"
"T:Acme.Widget.NestedClass"
"T:Acme.Widget.IMenuItem"
"T:Acme.Widget.Del"
"T:Acme.Widget.Direction"
```

フィールドは、完全修飾名で表されます。

```vb
Namespace Acme
    Structure ValueType
        Private total As Integer
    End Structure

    Class Widget
        Public Class NestedClass
            Private value As Integer
        End Class

        Private message As String
        Private Shared defaultColor As Color
        Private Const PI As Double = 3.14159
        Protected ReadOnly monthlyAverage As Double
        Private array1() As Long
        Private array2(,) As Widget
    End Class
End Namespace

"F:Acme.ValueType.total"
"F:Acme.Widget.NestedClass.value"
"F:Acme.Widget.message"
"F:Acme.Widget.defaultColor"
"F:Acme.Widget.PI"
"F:Acme.Widget.monthlyAverage"
"F:Acme.Widget.array1"
"F:Acme.Widget.array2"
```

コンストラクター。

```vb
Namespace Acme
    Class Widget
        Shared Sub New()
        End Sub

        Public Sub New()
        End Sub

        Public Sub New(s As String)
        End Sub
    End Class
End Namespace

"M:Acme.Widget.#cctor"
"M:Acme.Widget.#ctor"
"M:Acme.Widget.#ctor(System.String)"
```

メソッド。

```vb
Namespace Acme
    Structure ValueType
        Public Sub M(i As Integer)
        End Sub
    End Structure

    Class Widget
        Public Class NestedClass
            Public Sub M(i As Integer)
            End Sub
        End Class

        Public Shared Sub M0()
        End Sub

        Public Sub M1(c As Char, ByRef f As Float, _
            ByRef v As ValueType)
        End Sub

        Public Sub M2(x1() As Short, x2(,) As Integer, _
            x3()() As Long)
        End Sub

        Public Sub M3(x3()() As Long, x4()(,,) As Widget)
        End Sub

        Public Sub M4(Optional i As Integer = 1)

        Public Sub M5(ParamArray args() As Object)
        End Sub
    End Class
End Namespace

"M:Acme.ValueType.M(System.Int32)"
"M:Acme.Widget.NestedClass.M(System.Int32)"
"M:Acme.Widget.M0"
"M:Acme.Widget.M1(System.Char,System.Single@,Acme.ValueType@)"
"M:Acme.Widget.M2(System.Int16[],System.Int32[0:,0:],System.Int64[][])"
"M:Acme.Widget.M3(System.Int64[][],Acme.Widget[0:,0:,0:][])"
"M:Acme.Widget.M4(System.Int32)"
"M:Acme.Widget.M5(System.Object[])"
```

プロパティ

```vb
Namespace Acme
    Class Widget
        Public Property Width() As Integer
            Get
            End Get
            Set (Value As Integer)
            End Set
        End Property

        Public Default Property Item(i As Integer) As Integer
            Get
            End Get
            Set (Value As Integer)
            End Set
        End Property

        Public Default Property Item(s As String, _
            i As Integer) As Integer
            Get
            End Get
            Set (Value As Integer)
            End Set
        End Property
    End Class
End Namespace

"P:Acme.Widget.Width"
"P:Acme.Widget.Item(System.Int32)"
"P:Acme.Widget.Item(System.String,System.Int32)"
```

イベント   

```vb
Namespace Acme
    Class Widget
        Public Event AnEvent As EventHandler
        Public Event AnotherEvent()
    End Class
End Namespace

"E:Acme.Widget.AnEvent"
"E:Acme.Widget.AnotherEvent"
```

演算子。

```vb
Namespace Acme
    Class Widget
        Public Shared Operator +(x As Widget) As Widget
        End Operator

        Public Shared Operator +(x1 As Widget, x2 As Widget) As Widget
        End Operator
    End Class
End Namespace

"M:Acme.Widget.op_UnaryPlus(Acme.Widget)"
"M:Acme.Widget.op_Addition(Acme.Widget,Acme.Widget)"
```

変換演算子は、末尾が`~`後に、戻り値の型。

```vb
Namespace Acme
    Class Widget
        Public Shared Narrowing Operator CType(x As Widget) As _
            Integer
        End Operator

        Public Shared Widening Operator CType(x As Widget) As Long
        End Operator
    End Class
End Namespace

"M:Acme.Widget.op_Explicit(Acme.Widget)~System.Int32"
"M:Acme.Widget.op_Implicit(Acme.Widget)~System.Int64"
```

## <a name="documentation-comments-example"></a>ドキュメント コメントの例

次の例のソース コードを示しています、`Point`クラス。

```vb
Namespace Graphics
    ''' <remarks>
    ''' Class <c>Point</c> models a point in a two-dimensional
    ''' plane.
    ''' </remarks>
    Public Class Point
        ''' <summary>
        ''' Instance variable <c>x</c> represents the point's x-coordinate.
        ''' </summary>
        Private _x As Integer

        ''' <summary>
        ''' Instance variable <c>y</c> represents the point's y-coordinate.
        ''' </summary>
        Private _y As Integer

        ''' <value>
        ''' Property <c>X</c> represents the point's x-coordinate.
        ''' </value>
        Public Property X() As Integer
            Get
                Return _x
            End Get
            Set(Value As Integer)
                _x = Value
            End Set
        End Property

        ''' <value>
        ''' Property <c>Y</c> represents the point's y-coordinate.
        ''' </value>
        Public Property Y() As Integer
            Get
                Return _y
            End Get
            Set(Value As Integer)
                _y = Value
            End Set
        End Property

        ''' <summary>
        ''' This constructor initializes the new Point to (0,0).
        ''' </summary>
        Public Sub New()
            Me.New(0, 0)
        End Sub

        ''' <summary>
        ''' This constructor initializes the new Point to
        ''' (<paramref name="x"/>,<paramref name="y"/>).
        ''' </summary>
        ''' <param name="x"><c>x</c> is the new Point's
        ''' x-coordinate.</param>
        ''' <param name="y"><c>y</c> is the new Point's
        ''' y-coordinate.</param>
        Public Sub New(x As Integer, y As Integer)
            Me.X = x
            Me.Y = y
        End Sub

        ''' <summary>
        ''' This method changes the point's location to the given
        ''' coordinates.
        ''' </summary>
        ''' <param name="x"><c>x</c> is the new x-coordinate.</param>
        ''' <param name="y"><c>y</c> is the new y-coordinate.</param>
        ''' <see cref="Translate"/>
        Public Sub Move(x As Integer, y As Integer)
            Me.X = x
            Me.Y = y
        End Sub

        ''' <summary>
        ''' This method changes the point's location by the given x- and
        ''' y-offsets.
        ''' <example>
        ''' For example:
        ''' <code>
        '''    Dim p As Point = New Point(3, 5)
        '''    p.Translate(-1, 3)
        ''' </code>
        ''' results in <c>p</c>'s having the value (2,8).
        ''' </example>
        ''' </summary>
        ''' <param name="x"><c>x</c> is the relative x-offset.</param>
        ''' <param name="y"><c>y</c> is the relative y-offset.</param>
        ''' <see cref="Move"/>
        Public Sub Translate(x As Integer, y As Integer)
            Me.X += x
            Me.Y += y
        End Sub

        ''' <summary>
        ''' This method determines whether two Points have the same
        ''' location.
        ''' </summary>
        ''' <param name="o"><c>o</c> is the object to be compared to the
        ''' current object.</param>
        ''' <returns>
        ''' True if the Points have the same location and they have the
        ''' exact same type; otherwise, false.
        ''' </returns>
        ''' <seealso cref="Operator op_Equality"/>
        ''' <seealso cref="Operator op_Inequality"/>
        Public Overrides Function Equals(o As Object) As Boolean
            If o Is Nothing Then
                Return False
            End If
            If o Is Me Then
                Return True
            End If
            If Me.GetType() Is o.GetType() Then
                Dim p As Point = CType(o, Point)
                Return (X = p.X) AndAlso (Y = p.Y)
            End If
            Return False
        End Function

        ''' <summary>
        ''' Report a point's location as a string.
        ''' </summary>
        ''' <returns>
        ''' A string representing a point's location, in the form
        ''' (x,y), without any leading, training, or embedded whitespace.
        ''' </returns>
        Public Overrides Function ToString() As String
            Return "(" & X & "," & Y & ")"
        End Function

        ''' <summary>
        ''' This operator determines whether two Points have the
        ''' same location.
        ''' </summary>
        ''' <param name="p1"><c>p1</c> is the first Point to be compared.
        ''' </param>
        ''' <param name="p2"><c>p2</c> is the second Point to be compared.
        ''' </param>
        ''' <returns>
        ''' True if the Points have the same location and they 
        ''' have the exact same type; otherwise, false.
        ''' </returns>
        ''' <seealso cref="Equals"/>
        ''' <seealso cref="op_Inequality"/>
        Public Shared Operator =(p1 As Point, p2 As Point) As Boolean
            If p1 Is Nothing OrElse p2 Is Nothing Then
                Return False
            End If
            If p1.GetType() Is p2.GetType() Then
                Return (p1.X = p2.X) AndAlso (p1.Y = p2.Y)
            End If
            Return False
        End Operator

        ''' <summary>
        ''' This operator determines whether two Points have the
        ''' same location.
        ''' </summary>
        ''' <param name="p1"><c>p1</c> is the first Point to be comapred.
        ''' </param>
        ''' <param name="p2"><c>p2</c> is the second Point to be compared.
        ''' </param>
        ''' <returns>
        ''' True if the Points do not have the same location and
        ''' the exact same type; otherwise, false.
        ''' </returns>
        ''' <seealso cref="Equals"/>
        ''' <seealso cref="op_Equality"/>
        Public Shared Operator <>(p1 As Point, p2 As Point) As Boolean
            Return Not p1 = p2
        End Operator

        ''' <summary>
        ''' This is the entry point of the Point class testing program.
        ''' <para>This program tests each method and operator, and
        ''' is intended to be run after any non-trvial maintenance has
        ''' been performed on the Point class.</para>
        ''' </summary>
        Public Shared Sub Main()
            ' class test code goes here
        End Sub
    End Class
End Namespace
```

クラスのソース コードが指定されると生成される出力を次に示します`Point`、前述のようにします。

```xml
<?xml version="1.0"?>
<doc>
    <assembly>
        <name>Point</name>
    </assembly>
    <members>
        <member name="T:Graphics.Point">
            <remarks>Class <c>Point</c> models a point in a
            two-dimensional plane. </remarks>
        </member>
        <member name="F:Graphics.Point.x">
            <summary>Instance variable <c>x</c> represents the point's
            x-coordinate.</summary>
        </member>
        <member name="F:Graphics.Point.y">
            <summary>Instance variable <c>y</c> represents the point's
            y-coordinate.</summary>
        </member>
        <member name="M:Graphics.Point.#ctor">
            <summary>This constructor initializes the new Point to
            (0,0).</summary>
        </member>
        <member name="M:Graphics.Point.#ctor(System.Int32,System.Int32)">
            <summary>This constructor initializes the new Point to
            (<paramref name="x"/>,<paramref name="y"/>).</summary>
            <param><c>x</c> is the new Point's x-coordinate.</param>
            <param><c>y</c> is the new Point's y-coordinate.</param>
        </member>
        <member name="M:Graphics.Point.Move(System.Int32,System.Int32)">
            <summary>This method changes the point's location to
            the given coordinates.</summary>
            <param><c>x</c> is the new x-coordinate.</param>
            <param><c>y</c> is the new y-coordinate.</param>
            <see cref=
            "M:Graphics.Point.Translate(System.Int32,System.Int32)"/>
        </member>
        <member name=
        "M:Graphics.Point.Translate(System.Int32,System.Int32)">
            <summary>This method changes the point's location by the given
            x- and y-offsets.
            <example>For example:
            <code>
            Point p = new Point(3,5);
            p.Translate(-1,3);
            </code>
            results in <c>p</c>'s having the value (2,8).
            </example>
            </summary>
            <param><c>x</c> is the relative x-offset.</param>
            <param><c>y</c> is the relative y-offset.</param>
            <see cref="M:Graphics.Point.Move(System.Int32,System.Int32)"/>
        </member>
        <member name="M:Graphics.Point.Equals(System.Object)">
            <summary>This method determines whether two Points have the
            same location.</summary>
            <param><c>o</c> is the object to be compared to the current
            object.</param>
            <returns>True if the Points have the same location and they
            have the exact same type; otherwise, false.</returns>
            <seealso cref=
            "M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)"
            />
            <seealso cref=
           "M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)"
            />
        </member>
        <member name="M:Graphics.Point.ToString">
            <summary>Report a point's location as a string.</summary>
            <returns>A string representing a point's location, in the form
            (x,y), without any leading, training, or embedded
            whitespace.</returns>
        </member>
        <member name=
        "M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)">
            <summary>This operator determines whether two Points have the
            same location.</summary>
            <param><c>p1</c> is the first Point to be compared.</param>
            <param><c>p2</c> is the second Point to be compared.</param>
            <returns>True if the Points have the same location and they
            have the exact same type; otherwise, false.</returns>
            <seealso cref="M:Graphics.Point.Equals(System.Object)"/>
            <seealso cref=
           "M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)"
            />
        </member>
        <member name=
        "M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)">
            <summary>This operator determines whether two Points have the
            same location.</summary>
            <param><c>p1</c> is the first Point to be compared.</param>
            <param><c>p2</c> is the second Point to be compared.</param>
            <returns>True if the Points do not have the same location and
            the exact same type; otherwise, false.</returns>
            <seealso cref="M:Graphics.Point.Equals(System.Object)"/>
            <seealso cref=
            "M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)"
            />
        </member>
        <member name="M:Graphics.Point.Main">
            <summary>This is the entry point of the Point class testing
            program.
            <para>This program tests each method and operator, and
            is intended to be run after any non-trvial maintenance has
            been performed on the Point class.</para>
            </summary>
        </member>
        <member name="P:Graphics.Point.X">
            <value>Property <c>X</c> represents the point's
            x-coordinate.</value>
        </member>
        <member name="P:Graphics.Point.Y">
            <value>Property <c>Y</c> represents the point's
            y-coordinate.</value>
        </member>
    </members>
</doc>
```
