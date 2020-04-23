---
ms.openlocfilehash: 54e674bedd587647436b859423ab0f14715eca2d
ms.sourcegitcommit: 19ec79a287fb79180b05a0ad20e8291e75fc63df
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2020
ms.locfileid: "82080604"
---
# <a name="documentation-comments"></a>ドキュメント コメント

ドキュメントコメントは、ソース内の特別な形式のコメントで、ソースが添付されているコードに関するドキュメントを作成するために分析できます。 ドキュメント コメントの基本的な形式は XML です。 ドキュメント コメントを含むコードをコンパイルする場合、コンパイラは、ソース内のドキュメント コメントの合計を表す XML ファイルを任意で出力できます。 この XML ファイルは、印刷またはオンラインのドキュメントを作成する他のツールで使用できます。

この章では、ドキュメントコメントと推奨 XML タグについて説明します。

## <a name="documentation-comment-format"></a>ドキュメント コメント形式

ドキュメントコメントは、3 つの単`'''`一引用符で始まる特別なコメントです。 これらは、クラス、デリゲート、インターフェイスなどの型メンバまたは型メンバ (フィールド、イベント、プロパティ、メソッドなど) の直前に記述する必要があります。 部分メソッド宣言に対するドキュメント コメントは、その本体を提供するメソッドのドキュメント コメント (存在する場合) に置き換えられます。 隣接するすべてのドキュメント コメントが追加され、1 つのドキュメント コメントが生成されます。 `'''`文字の後に空白文字がある場合、その空白文字は連結に含まれません。 次に例を示します。

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

ドキュメント コメントは、 に従ってhttps://www.w3.org/TR/REC-xmlXML の形式が正しい必要があります。 XML の形式が正しくない場合は警告が生成され、エラーが発生したことを示すコメントがドキュメント ファイルに含まれます。

開発者は独自のタグセットを自由に作成できますが、次のセクションで推奨されるセットを定義します。 推奨されるタグの一部には特別な意味があります。

* この`<param>`タグは、パラメータを記述するために使用されます。 `<param>`タグで指定するパラメーターが存在し、型メンバーのすべてのパラメーターがドキュメント コメントに記述されている必要があります。 いずれかの条件が真でない場合、コンパイラは警告を発行します。

* `cref` 属性は任意のタグにアタッチでき、コード要素への参照を提供します。 コード要素が存在する必要があります。コンパイル時に、コンパイラは名前をメンバーを表す ID 文字列に置き換えます。 コード要素が存在しない場合、コンパイラは警告を発行します。 `cref`属性で記述されている名前を検索する場合、コンパイラは、含`Imports`まれているソース ファイル内に記述されているステートメントを尊重します。

* タグ`<summary>`は、型またはメンバーに関する追加情報を表示するために、ドキュメント ビューアーで使用されます。

ドキュメント ファイルには、型とメンバーに関する完全な情報は提供されず、ドキュメント コメントに含まれている内容のみが提供されることに注意してください。 型またはメンバーに関する詳細情報を取得するには、ドキュメント ファイルを実際の型またはメンバーのリフレクションと組み合わせて使用する必要があります。

## <a name="recommended-tags"></a>推奨タグ

ドキュメント ジェネレータは、XML の規則に従って有効なタグを受け入れ、処理する必要があります。 次のタグによって、ユーザー ドキュメントで一般的に使用される機能が与えられます。

`<c>`コードに似るフォントでテキストを設定する

`<code>`コードに似るフォントで 1 行以上のソース コードまたはプログラム出力を設定します。

`<example>`例を示します。

`<exception>`メソッドがスローできる例外を識別します。

`<include>`外部 XML ドキュメントを含む

`<list>`リストまたはテーブルを作成します。

`<para>`テキストに構造を追加することを許可します。

`<param>`メソッドまたはコンストラクターのパラメーターを記述します。

`<paramref>`単語がパラメータ名であることを識別します。

`<permission>`メンバーのセキュリティアクセシビリティを文書化します。

`<remarks>`型について説明します。

`<returns>`メソッドの戻り値を記述します。

`<see>`リンクを指定します。

`<seealso>`参照項目を生成します。

`<summary>`型のメンバーについて説明します。

`<typeparam>`型パラメーターについて説明します。

`<value>`プロパティについて説明します。

### <a name="ltcgt"></a>&lt;c&gt;

このタグは、説明内のテキストの断片がコードブロックに使用されるようなフォントを使用することを指定します。 (実際のコード行の場合は`<code>`、を使用します。

__構文 :__

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

このタグは、1 行以上のソース コードまたはプログラム出力で固定幅フォントを使用することを指定します。 (小さなコードフラグメントの場合は`<c>`、.を使用してください)。

__構文 :__

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

このタグを使用すると、コメント内のコード例を使用して、要素の使用方法を示すことができます。 通常、これにはタグ`<code>`の使用も含まれます。

__構文 :__

```xml
<example>description</example>
```

__例:__

例については、「`<code>`」を参照してください。

### <a name="ltexceptiongt"></a>&lt;exception&gt;

このタグは、メソッドがスローできる例外を文書化する方法を提供します。

__構文 :__

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

このタグは、外部の整形式 XML ドキュメントからの情報を含めるために使用されます。 Xml ドキュメントに XPath 式を適用して、ドキュメントからどの XML をインクルードするかを指定します。 タグ`<include>`は、外部ドキュメントから選択した XML に置き換えられます。

__構文 :__

```xml
<include file="filename" path="xpath">
```

__例:__

ソース コードに次のような宣言が含まれている場合:

```vb
''' <include file="docs.xml" path="extra/class[@name="IntList"]/*" />
```

外部ファイル docs.xml には次の内容があります

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

その後、ソース コードに含まれている場合と同じドキュメントが出力されます。

```xml
''' <summary>
''' Contains a list of integers.
''' </summary>
```

### <a name="ltlistgt"></a>&lt;list&gt;

このタグは、項目のリストまたはテーブルを作成するために使用されます。 テーブルまたは定義リスト`<listheader>`の見出し行を定義するブロックを含めることができます。 (テーブルを定義する場合は、見出しに用語のエントリのみを指定する必要があります)。

リスト内の各項目はブロックで`<item>`指定されます。 定義リストを作成する場合は、用語と説明の両方を指定する必要があります。 ただし、表、箇条書きリスト、または番号付きリストの場合は、説明のみを指定する必要があります。

__構文 :__

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

このタグは、 や`<remarks>``<returns>`などの他のタグの内部で使用するためのもので、構造をテキストに追加できます。

__構文 :__

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

このタグは、メソッド、コンストラクター、またはインデックス付きプロパティのパラメーターを表します。

__構文 :__

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

このタグは、単語がパラメータであることを示します。 ドキュメント ファイルは、このパラメーターを何らかの方法でフォーマットするために処理できます。

__構文 :__

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

このタグは、メンバーのセキュリティアクセシビリティを示します。

__構文 :__

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

このタグは、型に関する概要情報を指定します。 (型`<summary>`のメンバーを記述するために使用します)。

__構文 :__

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

このタグは、メソッドの戻り値を表します。

__構文 :__

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

このタグを使用すると、テキスト内でリンクを指定できます。 (参照`<seealso>`セクションに表示されるテキストを指定するために使用します)。

__構文 :__

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

このタグは、[参照] セクションのエントリを生成します。 (テキスト`<see>`内からリンクを指定するために使用します。

__構文 :__

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

このタグは、型メンバーを表します。 (型`<remarks>`自体を記述するために使用します)。

__構文 :__

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

このタグは、型パラメーターを表します。

__構文 :__

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

このタグは、プロパティを記述します。

__構文 :__

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

ドキュメント ファイルを生成するときに、コンパイラは、ソース コード内の要素ごとに ID 文字列を生成し、その要素を一意に識別するドキュメント コメントでタグ付けします。 この ID 文字列は、外部ツールで使用して、コンパイル済みアセンブリ内のどの要素がドキュメント コメントに対応するかを識別できます。

ID 文字列は次のように生成されます。

文字列に空白は配置されません。

文字列の最初の部分は、1 文字の後にコロンを続けて、文書化されているメンバーの種類を識別します。 イベント (E)、フィールド (F)、コンストラクターと演算子 (M)、名前空間 (N)、プロパティ (P)、および型 (T) を含むメソッドの後に、対応する文字をかっこで囲んで、次の種類のメンバーが定義されます。 感嘆符 (!) は ID 文字列の生成中にエラーが発生したことを示し、残りの文字列はエラーに関する情報を提供します。

文字列の 2 番目の部分は、グローバル名前空間から始まる要素の完全修飾名です。 要素の名前、その外側の型、および名前空間はピリオドで区切られます。 項目自体の名前にピリオドがある場合は、シャープ記号 (#) に置き換えられます。 (この名前にこの文字を持つ要素は存在しないものと見なされます。型パラメーターを持つ型の名前は、後ろに戻り引用符 (') の後に型の型パラメーターの数を表す数値で終わります。 入れ子になった型は、それらを含む型の型パラメーターにアクセスできるため、入れ子になった型には、その型の型パラメーターが暗黙的に含まれ、その型は型パラメーターの合計にカウントされます。

引数を持つメソッドとプロパティの場合、引数リストはかっこで囲まれて次のようになります。 引数を持たない場合、括弧は省略されます。 引数はコンマで区切られます。 各引数のエンコーディングは、次のように CLI シグネチャと同じです。 `Integer`たとえば、、、、、、、、、`System.Int32``String``System.String``Object`などになります`System.Object`。 修飾子を`ByRef`持つ引数の型名の後には '@' が付きます。 修飾子または`ParamArray`修飾子`ByVal``Optional`を持つ引数には、特別な表記はありません。 配列である引数は、コンマの数`[lowerbound:size, ..., lowerbound:size]`がランク - 1 で、各次元の下限とサイズが分かっている場合は 10 進数で表されます。 下限またはサイズが指定されていない場合は、省略されます。 特定の次元で下限およびサイズが省略されている場合は、':' も省略されます。 配列の配列は、レベルごとに 1`[]`つの " " で表されます。

### <a name="id-string-examples"></a>ID 文字列の例

次の例は、VB コードのフラグメントと、ドキュメント コメントを持つ各ソース要素から生成された ID 文字列を示しています。

型は完全修飾名を使用して表されます。

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

プロパティ。

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

変換演算子には、末尾`~`に戻り値の型が続きます。

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

次の例は、クラスのソース`Point`コードを示しています。

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

上に示した class`Point`のソース コードを指定したときに生成される出力を次に示します。

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
