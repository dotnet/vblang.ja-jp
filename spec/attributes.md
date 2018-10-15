# <a name="attributes"></a>属性

Visual Basic 言語により、プログラマは、宣言されているエンティティに関する情報を表す宣言に修飾子を指定できます。 たとえば、修飾子を使用してクラスのメソッドを付加すること`Public`、 `Protected`、 `Friend`、 `Protected Friend`、または`Private`アクセシビリティを指定します。

言語によって定義された修飾子、に加えて Visual Basic も作成を可能と呼ばれる新しい修飾子は、*属性*、新しいエンティティを宣言するときに使用するとします。 属性クラスの宣言で定義されて、これらの新しい修飾子は使用してエンティティに割り当てられます*属性ブロック*します。

__注意してください。__ 属性は、.NET Framework のリフレクション Api を介して実行時に取得できます。 これらの Api は、この仕様の範囲外です。

たとえば、フレームワークは、定義、`Help`次の例に示すように、ドキュメントへのプログラム要素からマッピングを提供するクラスおよびメソッドなどのプログラム要素に配置できる属性。

```vb
<AttributeUsage(AttributeTargets.All)> _
Public Class HelpAttribute
    Inherits Attribute

    Public Sub New(urlValue As String)
        Me.UrlValue = urlValue
    End Sub

    Public Topic As String
    Private UrlValue As String

    Public ReadOnly Property Url() As String
        Get
            Return UrlValue
        End Get
    End Property
End Class
```

例では、という名前の属性クラスを定義します`HelpAttribute`、または`Help`、略してを持つ 1 つの位置指定パラメーター (`UrlValue`) 1 つの名前付き引数と (`Topic`)。

次の例では、属性のいくつかの用途を示します。

```vb
<Help("http://www.example.com/.../Class1.htm")> _
Public Class Class1
    <Help("http://www.example.com/.../Class1.htm", Topic:="F")> _
    Public Sub F()
    End Sub
End Class
```

次の例をかどうかを確認します`Class1`が、`Help`属性、および関連付けられた書き込み`Topic`と`Url`属性が存在する場合の値します。

```vb
Module Test
    Sub Main()
        Dim type As Type = GetType(Class1)
        Dim arr() As Object = _
            type.GetCustomAttributes(GetType(HelpAttribute), True)

        If arr.Length = 0 Then
            Console.WriteLine("Class1 has no Help attribute.")
        Else
            Dim ha As HelpAttribute = CType(arr(0), HelpAttribute)
            Console.WriteLine("Url = " & ha.Url & ", Topic = " & ha.Topic)
        End If
    End Sub
End Module
```

## <a name="attribute-classes"></a>属性クラス

*属性クラス*から派生した非ジェネリック クラスは、`System.Attribute`およびない`MustInherit`します。 属性クラスがあります、`System.AttributeUsage`属性はでは有効で、それが宣言では、使用することがあります複数回、および継承されているかどうかを宣言する属性。 次の例は、という名前の属性クラスを定義します。`SimpleAttribute`クラス宣言でインターフェイスの宣言を配置できます。

```vb
<AttributeUsage(AttributeTargets.Class Or AttributeTargets.Interface)> _
Public Class SimpleAttribute
    Inherits System.Attribute
End Class
```

次の例は、いくつかの使用、`Simple`属性。 属性クラスの名前が`SimpleAttribute`、この属性の使用方法が省略、`Attribute`に名前を短縮され、サフィックス`Simple`:

```vb
<Simple> Class Class1
End Class

<Simple> Interface Interface1
End Interface
```

属性がない場合、 `System.AttributeUsage`、属性は、任意のターゲットに配置できますし、(に相当`AttributeTargets.All`)。 `System.AttributeUsage`属性は、変数の初期化子が`AllowMultiple`、特定の宣言の示された属性を複数回指定できるかどうかを指定します。 場合`AllowMultiple`属性は`True`は、*属性クラスの複数の用途*宣言で複数回指定できます。 場合`AllowMultiple`属性は`False`または属性に指定されていない場合は、*単一使用属性クラス*、多くても 1 回宣言で指定できます。

次の例は、という名前の属性を複数使用クラスを定義します`AuthorAttribute`:。

```vb
<AttributeUsage(AttributeTargets.Class, AllowMultiple:=True)> _
Public Class AuthorAttribute
    Inherits System.Attribute

    Private _Value As String

    Public Sub New(value As String)
        Me._Value = value
    End Sub

    Public ReadOnly Property Value() As String
        Get
            Return _Value
        End Get
    End Property
End Class
```

2 つの用途をクラス宣言の例を示します、`Author`属性。

```vb
<Author("Maria Hammond"), Author("Ramesh Meyyappan")> _
Class Class1
End Class
```

`System.AttributeUsage`属性が、パブリック インスタンス変数`Inherited`、基本型で指定する場合は、属性がこの基本型から派生した型でも継承されたかどうかを指定します。 場合、`Inherited`パブリック インスタンス変数が初期化されていない、既定値は`False`使用されます。 プロパティおよびイベントは、プロパティおよびイベントによって定義されたメソッドが、属性を継承しないでください。 インターフェイスは、属性を継承しません。

単一使用属性が継承および派生型で指定された、派生型で指定されている属性は継承された属性より優先されます。 場合は、複数の用途の属性が継承および派生型で指定されて、両方の属性は、派生型を指定します。 例えば:

```vb
<AttributeUsage(AttributeTargets.Class, AllowMultiple:=True, _
                Inherited:=True) > _
Class MultiUseAttribute
    Inherits System.Attribute

    Public Sub New(value As Boolean)
    End Sub
End Class

<AttributeUsage(AttributeTargets.Class, Inherited:=True)> _
Class SingleUseAttribute
    Inherits Attribute

    Public Sub New(value As Boolean)
    End Sub
End Class

<SingleUse(True), MultiUse(True)> Class Base
End Class

' Derived has three attributes defined on it: SingleUse(False),
' MultiUse(True) and MultiUse(False)
<SingleUse(False), MultiUse(False)> _
Class Derived
    Inherits Base
End Class
```

属性の位置指定パラメーターは、属性クラスのパブリック コンス トラクターのパラメーターによって定義されます。 位置指定パラメーターである必要があります`ByVal`可能性がありますを指定しないと`ByRef`します。 パブリック インスタンス変数とプロパティは、パブリックの読み取り/書き込みプロパティまたは属性クラスのインスタンス変数によって定義されます。 位置指定パラメーターとインスタンスのパブリック変数のプロパティで使用できる型では、属性の型に制限されます。 型が属性型で、次のいずれかに該当する場合です。

* 任意のプリミティブ型を除く`Date`と`Decimal`します。

* 型 `Object`。

* 型 `System.Type`。

* 列挙型、し、型では、入れ子になっている (あれば) が提供された`Public`アクセシビリティ。

* この一覧の前の型のいずれかの 1 次元配列。

## <a name="attribute-blocks"></a>属性ブロック

属性で指定*属性ブロック*します。 各属性ブロックは、山かっこ ("<>") で区切られた、属性ブロック内のコンマ区切りの一覧で、または複数の属性ブロックで、複数の属性を指定できます。 属性が指定されている順序が重要ではありません。 たとえば、属性ブロック`<A, B>`、 `<B, A>`、`<A> <B>`と`<B> <A>`はすべて同等です。

```antlr
Attributes
    : AttributeBlock+
    ;

AttributeBlock
    : LineTerminator? '<' AttributeList LineTerminator? '>' LineTerminator?
    ;

AttributeList
    : Attribute ( Comma Attribute )*
    ;

Attribute
    : ( AttributeModifier ':' )? SimpleTypeName
    ( OpenParenthesis AttributeArguments? CloseParenthesis )?
    ;

AttributeModifier
    : 'Assembly' | 'Module'
    ;
```

属性を指定できませんそうでない宣言のような属性ブロックでのサポート、および単一使用属性が複数回指定するされません。 使用しようとするために、次の例によって両方のエラーが発生`HelpString`インターフェイスで`Interface1`の宣言で複数回と`Class1`します。

```vb
<AttributeUsage(AttributeTargets.Class)> _
Public Class HelpStringAttribute
    Inherits System.Attribute

    Private InternalValue As String

    Public Sub New(value As String)
        Me.InternalValue = value
    End Sub

    Public ReadOnly Property Value() As String
        Get
            Return InternalValue
        End Get
    End Property
End Class

' Error: HelpString only applies to classes.
<HelpString("Description of Interface1")> _
Interface Interface1
    Sub Sub1()
End Interface

' Error: HelpString is single-use.
<HelpString("Description of Class1"), _
    HelpString("Another description of Class1")> _
Public Class Class1
End Class
```

属性は、省略可能な属性の修飾子、属性名、位置指定引数と変数/プロパティの初期化子のオプションのリストで構成されます。 パラメーターまたは初期化子がない場合、かっこは省略できます。 属性に、修飾子がある場合は、ソース ファイルの上部にある属性ブロックでがあります。

属性ブロック内の各属性を両方によってプレフィックス指定する必要がありますが、ソース ファイルにアセンブリまたはソース ファイルが含まれるモジュールの属性を指定するファイルの上部にある属性ブロックが含まれている場合、`Assembly`または`Module`修飾子およびコロンです。


### <a name="attribute-names"></a>属性名

属性の名前には、属性クラスを指定します。 慣例により、属性クラスのサフィックスを持つ名前は`Attribute`します。 属性の使用は可能性があります含めるか、このサフィックスを省略します。 したがって、属性の識別子に対応する属性クラスの名前が識別子自体または修飾された識別子の連結と`Attribute`します。 コンパイラは、属性名を解決するときの追加`Attribute`名前にして検索します。 検索に失敗した場合、コンパイラはサフィックスの検索を試みます。 たとえば、属性クラスの使用`SimpleAttribute`省略可能性があります、`Attribute`に名前を短縮され、サフィックス`Simple`:

```vb
<Simple> Class Class1
End Class

<Simple> Interface Interface1
End Interface
```

上記の例は、次の意味と同等です。

```vb
<SimpleAttribute> Class Class1
End Class

<SimpleAttribute> Interface Interface1
End Interface
```

一般に、サフィックスを持つ属性が名前付き`Attribute`を推奨します。 次の例では、という名前の 2 つの属性クラス`T`と`T``Attribute`します。

```vb
<AttributeUsage(AttributeTargets.All)> _
Public Class T
    Inherits System.Attribute
End Class

<AttributeUsage(AttributeTargets.All)> _
Public Class TAttribute
    Inherits System.Attribute
End Class

' Refers to TAttribute.
<T> Class Class1 
End Class

' Refers to TAttribute.
<TAttribute> Class Class2 
End Class
```

属性ブロック`<T>`および属性ブロック`<TAttribute>`という名前の属性クラスを参照してください`TAttribute`します。 使用することはできません`T`クラスの宣言を削除するまで、属性として`TAttribute`します。

### <a name="attribute-arguments"></a>属性の引数

属性に引数が 2 つの形式をかかる場合があります:*位置指定引数*と*変数/プロパティの初期化子をインスタンス*します。 属性に位置引数には、インスタンス変数/プロパティの初期化子が前にする必要があります。 位置指定引数は、定数式である 1 次元配列作成式で構成されますまたは`GetType`式。 インスタンス変数/プロパティの初期化子の後にコロンと等号、定数式で終了しているキーワードに一致する識別子から成るまたは`GetType`式。

属性クラスを使用して属性を指定された`T`、位置指定引数リスト`P`、およびインスタンス変数/プロパティの初期化子リスト`N`、次の手順は、引数が有効かどうかを確認します。

1. 形式の式をコンパイルするためのコンパイル時の処理手順に従います`New T(P)`します。 これは、コンパイル時エラーが発生またはでコンス トラクターが決定`T`引数リストに最も適したです。

2. ステップ 1 で特定のコンス トラクターでは、属性の型ではないパラメーターを持ちまたは宣言のサイトにアクセスできない、コンパイル時エラーが発生します。

3. 変数/プロパティ初期化子のインスタンスそれぞれ`Arg`で`N`、`Name`インスタンス変数/プロパティの初期化子の識別子である`Arg`します。 `Name` 識別する必要があります以外`Shared`、書き込み可能な`Public`変数またはパラメーターなしのプロパティをインスタンス`T`型が属性の型。 場合`T`がそのようなインスタンスの変数またはプロパティ、コンパイル時エラーが発生します。

例えば:

```vb
<AttributeUsage(AttributeTargets.All)> _
Public Class GeneralAttribute
    Inherits Attribute

    Public Sub New(x As Integer)
    End Sub

    Public Sub New(x As Double)
    End Sub

    Public y As Type

    Public Property z As Integer
        Get
        End Get

        Set
        End Set
    End Property
End Class

' Calls the first constructor.
<General(10, z:=30, y:=GetType(Integer))> _
Class C1
End Class

' Calls the second constructor.
<General(10.5, z:=10)> _
Class C2
End Class
```

型パラメーターは、属性の引数に任意の場所で使用できません。 ただし、構築された型を使用できます。

```vb
<AttributeUsage(AttributeTargets.All)> _
Class A 
   Inherits System.Attribute 

   Public Sub New(t As Type)
   End Sub 
End Class

Class List(Of T) 
    ' Error: attribute argument cannot use type parameter
    <A(GetType(T))> Dim t1 As T 

    ' OK: closed type
    <A(GetType(List(Of Integer)))> Dim y As Integer
End Class
```


```antlr
AttributeArguments
    : AttributePositionalArgumentList
    | AttributePositionalArgumentList Comma VariablePropertyInitializerList
    | VariablePropertyInitializerList
    ;

AttributePositionalArgumentList
    : AttributeArgumentExpression? ( Comma AttributeArgumentExpression? )*
    ;

VariablePropertyInitializerList
    : VariablePropertyInitializer ( Comma VariablePropertyInitializer )*
    ;

VariablePropertyInitializer
    : IdentifierOrKeyword ColonEquals AttributeArgumentExpression
    ;

AttributeArgumentExpression
    : ConstantExpression
    | GetTypeExpression
    | ArrayExpression
    ;
```