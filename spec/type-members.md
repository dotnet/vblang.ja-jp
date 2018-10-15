# <a name="type-members"></a>型のメンバー

型のメンバーは、記憶域の場所と実行可能コードを定義します。 メソッド、コンス トラクター、イベント、定数、変数、およびプロパティがあることができます。

## <a name="interface-method-implementation"></a>インターフェイス メソッドの実装

メソッド、イベント、およびプロパティは、インターフェイス メンバーを実装できます。 インターフェイス メンバーを実装するメンバーの宣言を指定します、`Implements`キーワードと 1 つまたは複数のインターフェイス メンバーを一覧表示します。

```antlr
ImplementsClause
    : ( 'Implements' ImplementsList )?
    ;

ImplementsList
    : InterfaceMemberSpecifier ( Comma InterfaceMemberSpecifier )*
    ;

InterfaceMemberSpecifier
    : NonArrayTypeName Period IdentifierOrKeyword
    ;
```

インターフェイス メンバーを実装するメソッドとプロパティは、暗黙的に`NotOverridable`として宣言されている場合を除き、 `MustOverride`、 `Overridable`、または別のメンバーをオーバーライドします。 エラーにするインターフェイス メンバーを実装するメンバーには`Shared`します。 メンバーのアクセシビリティはインターフェイス メンバーを実装する機能に影響を与えません。

インターフェイスの実装を有効にするには、包含する型の実装のリストは互換性のあるメンバーを含むインターフェイスを名前する必要があります。 互換性のあるメンバーは、署名には、実装するメンバーのシグネチャが一致する 1 つです。 ジェネリック インターフェイスが実装されている場合、Implements 句で指定された型引数に置き換えられます、シグネチャに互換性をチェックするときに。 例えば:

```vb
Interface I1(Of T)
    Sub F(x As T)
End Interface

Class C1
    Implements I1(Of Integer)

    Sub F(x As Integer) Implements I1(Of Integer).F
    End Sub
End Class

Class C2(Of U)
    Implements I1(Of U)

    Sub F(x As U) Implements I1(Of U).F
    End Sub
End Class
```

イベント宣言を使用している場合は、デリゲート型がインターフェイスのイベントを実装する、互換性のあるイベントは、基になるデリゲート型が同じ型。 それ以外の場合、イベントは、実装しているインターフェイスのイベントのデリゲート型を使用します。 このようなイベントは、複数のインターフェイス イベントを実装するインターフェイスのすべてのイベントが同じ基になるデリゲート型に必要です。 例えば:

```vb
Interface ClickEvents
    Event LeftClick(x As Integer, y As Integer)
    Event RightClick(x As Integer, y As Integer)
End Interface

Class Button
    Implements ClickEvents

    ' OK. Signatures match, delegate type = ClickEvents.LeftClickHandler.
    Event LeftClick(x As Integer, y As Integer) _
        Implements ClickEvents.LeftClick

    ' OK. Signatures match, delegate type = ClickEvents.RightClickHandler.
    Event RightClick(x As Integer, y As Integer) _
        Implements ClickEvents.RightClick
End Class

Class Label
    Implements ClickEvents

    ' Error. Signatures match, but can't be both delegate types.
    Event Click(x As Integer, y As Integer) _
        Implements ClickEvents.LeftClick, ClickEvents.RightClick
End Class
```

インターフェイス メンバーの実装のリストを指定するには、型の名前、ピリオド、および識別子を使用します。 実装リスト内のインターフェイスまたは基本インターフェイスの実装の一覧で、インターフェイス型名である必要があります、識別子は、指定したインターフェイスのメンバーである必要があります。 1 つのメンバーは、1 つ以上の一致するインターフェイス メンバーを実装できます。

```vb
Interface ILeft
    Sub F()
End Interface

Interface IRight
    Sub F()
End Interface

Class Test
    Implements ILeft, IRight

    Sub F() Implements ILeft.F, IRight.F
    End Sub
End Class
```

実装されているインターフェイスのメンバーが使用できない場合すべて明示的に実装されたインターフェイスで複数のインターフェイスの継承により、実装しているメンバーは、メンバーは、使用する基本インターフェイスを明示的に参照する必要があります。 たとえば場合、`I1`と`I2`、メンバーを含んで`M`と`I3`から継承`I1`と`I2`、実装する型`I3`実装は`I1.M`と`I2.M`。 場合は、インターフェイスの影は多重継承されたメンバーを実装する型が継承されたメンバーとそれをシャドウするメンバーを実装する必要があります。

```vb
Interface ILeft
    Sub F()
End Interface

Interface IRight
    Sub F()
End Interface

Interface ILeftRight
    Inherits ILeft, IRight

    Shadows Sub F()
End Interface

Class Test
    Implements ILeftRight

    Sub LeftF() Implements ILeft.F
    End Sub

    Sub RightF() Implements IRight.F
    End Sub

    Sub LeftRightF() Implements ILeftRight.F
    End Sub
End Class
```

インターフェイス メンバーの親インターフェイスを実装する場合は、ジェネリック型引数を入力と同じように実装されるインターフェイスを提供する必要があります。 例えば:

```vb
Interface I1(Of T)
    Function F() As T
End Interface

Class C1
    Implements I1(Of Integer)
    Implements I1(Of Double)

    Function F1() As Integer Implements I1(Of Integer).F
    End Function

    Function F2() As Double Implements I1(Of Double).F
    End Function

    ' Error: I1(Of String) is not implemented by C1
    Function F3() As String Implements I1(Of String).F
    End Function
End Class

Class C2(Of U)
    Implements I1(Of U)

    Function F() As U Implements I1(Of U).F
    End Function
End Class
```


## <a name="methods"></a>メソッド

メソッドには、プログラムの実行可能なステートメントが含まれます。

```antlr
MethodMemberDeclaration
    : MethodDeclaration
    | ExternalMethodDeclaration
    ;

InterfaceMethodMemberDeclaration
    : InterfaceMethodDeclaration
    ;

MethodDeclaration
    : SubDeclaration
    | MustOverrideSubDeclaration
    | FunctionDeclaration
    | MustOverrideFunctionDeclaration
    ;

InterfaceMethodDeclaration
    : InterfaceSubDeclaration
    | InterfaceFunctionDeclaration
    ;

SubSignature
    : 'Sub' Identifier TypeParameterList?
      ( OpenParenthesis ParameterList? CloseParenthesis )?
    ;

FunctionSignature
    : 'Function' Identifier TypeParameterList?
      ( OpenParenthesis ParameterList? CloseParenthesis )?
      ( 'As' Attributes? TypeName )?
    ;

SubDeclaration
    : Attributes? ProcedureModifier* SubSignature
      HandlesOrImplements? LineTerminator
      Block
      'End' 'Sub' StatementTerminator
    ;

MustOverrideSubDeclaration
    : Attributes? MustOverrideProcedureModifier+ SubSignature
      HandlesOrImplements? StatementTerminator
    ;

InterfaceSubDeclaration
    : Attributes? InterfaceProcedureModifier* SubSignature StatementTerminator
    ;

FunctionDeclaration
    : Attributes? ProcedureModifier* FunctionSignature
      HandlesOrImplements? LineTerminator
      Block
      'End' 'Function' StatementTerminator
    ;

MustOverrideFunctionDeclaration
    : Attributes? MustOverrideProcedureModifier+ FunctionSignature
      HandlesOrImplements? StatementTerminator
    ;

InterfaceFunctionDeclaration
    : Attributes? InterfaceProcedureModifier* FunctionSignature StatementTerminator
    ;

ProcedureModifier
    : AccessModifier | 'Shadows' | 'Shared' | 'Overridable' | 'NotOverridable' | 'Overrides'
    | 'Overloads' | 'Partial' | 'Iterator' | 'Async'
    ;

MustOverrideProcedureModifier
    : ProcedureModifier
    | 'MustOverride'
    ;

InterfaceProcedureModifier
    : 'Shadows' | 'Overloads'
    ;

HandlesOrImplements
    : HandlesClause
    | ImplementsClause
    ;
```

メソッドで、オプション パラメーターと省略可能な戻り値のリストがあるは、共有またはは共有されません。 共有メソッドは、クラスまたはクラスのインスタンスを介してアクセスします。 インスタンスのメソッドとも呼ばれます。 非共有メソッドは、クラスのインスタンスを通じてアクセスします。 次の例では、クラス`Stack`いくつかの共有メソッドを持つ (`Clone`と`Flip`)、いくつかのインスタンス メソッドと (`Push`、 `Pop`、および`ToString`)。

```vb
Public Class Stack
    Public Shared Function Clone(s As Stack) As Stack
        ...
    End Function

    Public Shared Function Flip(s As Stack) As Stack
        ...
    End Function

    Public Function Pop() As Object
        ...
    End Function

    Public Sub Push(o As Object)
        ...
    End Sub 

    Public Overrides Function ToString() As String
        ...
    End Function 
End Class 

Module Test
    Sub Main()
        Dim s As Stack = New Stack()
        Dim i As Integer

        While i < 10
            s.Push(i)
        End While

        Dim flipped As Stack = Stack.Flip(s)
        Dim cloned As Stack = Stack.Clone(s)

        Console.WriteLine("Original stack: " & s.ToString())
        Console.WriteLine("Flipped stack: " & flipped.ToString())
        Console.WriteLine("Cloned stack: " & cloned.ToString())
    End Sub
End Module
```

メソッド オーバー ロードできます、つまり、一意の署名がある限り、複数のメソッドに同じ名前を持つことがあります。 メソッドのシグネチャは、そのパラメーターの型と数で構成されます。 メソッドのシグネチャ具体的には含まれません、戻り値の型またはパラメーター修飾子 (省略可能)、ByRef ParamArray など。 次の例では、さまざまなオーバー ロード クラスを示します。

```vb
Module Test
    Sub F()
        Console.WriteLine("F()")
    End Sub 

    Sub F(o As Object)
        Console.WriteLine("F(Object)")
    End Sub

    Sub F(value As Integer)
        Console.WriteLine("F(Integer)")
    End Sub 

    Sub F(a As Integer, b As Integer)
        Console.WriteLine("F(Integer, Integer)")
    End Sub 

    Sub F(values() As Integer)
        Console.WriteLine("F(Integer())")
    End Sub 

    Sub G(s As String, Optional s2 As String = 5)
        Console.WriteLine("G(String, Optional String")
    End Sub

    Sub G(s As String)
        Console.WriteLine("G(String)")
    End Sub


    Sub Main()
        F()
        F(1)
        F(CType(1, Object))
        F(1, 2)
        F(New Integer() { 1, 2, 3 })
        G("hello")
        G("hello", "world")
    End Sub
End Module
```

プログラムの出力は次のとおりです。

```vb
F()
F(Integer)
F(Object)
F(Integer, Integer)
F(Integer())
G(String)
G(String, Optional String)
```

省略可能なパラメーターでのみ異なるオーバー ロードは、ライブラリの「バージョン管理」で使用できます。 たとえば、ライブラリの v1 には、省略可能なパラメーターを持つ関数が含まれます。

```vb
Sub fopen(fileName As String, Optional accessMode as Integer = 0)
```

もう 1 つの省略可能なパラメーター"password"を追加する場合、ライブラリの v2 と (つまり、v1 を対象とするために使用するアプリケーションを再コンパイルすることができます) のソース互換性を損なうことがなく、(そのために使用するアプリケーションのバイナリの互換性を損なうことがなく、これを実行したいです。リファレンス v1 と参照できます再コンパイルせず v2)。 これは、v2 の見た目は。

```vb
Sub fopen(file As String, mode as Integer)
Sub fopen(file As String, Optional mode as Integer = 0, Optional pword As String = "")
```

パブリック API では省略可能なパラメーターが CLS に準拠していないことに注意してください。 ただし、それらで使用できる、少なくとも Visual Basic、C# 4 と f#。



### <a name="regular-async-and-iterator-method-declarations"></a>通常、非同期および反復子メソッドの宣言

メソッドの 2 種類があります:*サブルーチン*、値を返さないと*関数*を実行します。 本文と`End`メソッドがインターフェイスで定義されている、または場合、メソッドの構文を省略することがありますのみ、`MustOverride`修飾子。 関数の戻り値の型が指定されていないと、厳密な型が使用されている、コンパイル時エラーが発生します。それ以外の場合、型は暗黙的に`Object`またはメソッドの型の文字の種類。 戻り値の型とメソッドのパラメーターの型のアクセシビリティ ドメインは、メソッド自体のアクセシビリティ ドメインのスーパー セットであるかと同じである必要があります。

A__正規メソッド__で、どちらも`Async`も`Iterator`修飾子。 サブルーチンまたは関数があります。 セクション[通常のメソッド](statements.md#regular-methods)正規表現メソッドが呼び出されたときの動作について詳しく説明します。

__反復子メソッド__で 1 つは、`Iterator`修飾子と no`Async`修飾子。 関数の場合、する必要があり、その戻り値の型である必要があります`IEnumerator`、 `IEnumerable`、または`IEnumerator(Of T)`または`IEnumerable(Of T)`一部`T`、されが必要ない`ByRef`パラメーター。 セクション[反復子メソッド](statements.md#iterator-methods)反復子メソッドが呼び出されたときの動作について詳しく説明します。

__Async メソッド__で 1 つ、`Async`修飾子と no`Iterator`修飾子。 サブルーチン、または戻り値の型を持つ関数のいずれかでなければなりません`Task`または`Task(Of T)`一部`T`、no が必要と`ByRef`パラメーター。 セクション[非同期メソッド](statements.md#async-methods)非同期メソッドが呼び出されたときの動作について詳しく説明します。

メソッドでないメソッドのこれらの 3 種類のいずれかの場合は、コンパイル時エラーを勧めします。

サブルーチンと関数の宣言では論理行の先頭には、先頭と end ステートメントの各開始する必要があります。 以外の本文ではさらに、`MustOverride`サブルーチンまたは関数の宣言が論理行の先頭から開始する必要があります。 例えば:

```vb
Module Test
    ' Illegal: Subroutine doesn't start the line
    Public x As Integer : Sub F() : End Sub

    ' Illegal: First statement doesn't start the line
    Sub G() : Console.WriteLine("G")
    End Sub

    ' Illegal: End Sub doesn't start the line
    Sub H() : End Sub
End Module
```


### <a name="external-method-declarations"></a>外部メソッドの宣言

外部メソッドの宣言には、実装がプログラムの外部に提供される新しいメソッドが導入されています。

```antlr
ExternalMethodDeclaration
    : ExternalSubDeclaration
    | ExternalFunctionDeclaration
    ;

ExternalSubDeclaration
    : Attributes? ExternalMethodModifier* 'Declare' CharsetModifier? 'Sub'
      Identifier LibraryClause AliasClause?
      ( OpenParenthesis ParameterList? CloseParenthesis )? StatementTerminator
    ;

ExternalFunctionDeclaration
    : Attributes? ExternalMethodModifier* 'Declare' CharsetModifier? 'Function'
      Identifier LibraryClause AliasClause?
      ( OpenParenthesis ParameterList? CloseParenthesis )?
      ( 'As' Attributes? TypeName )?
      StatementTerminator
    ;

ExternalMethodModifier
    : AccessModifier
    | 'Shadows'
    | 'Overloads'
    ;

CharsetModifier
    : 'Ansi' | 'Unicode' | 'Auto'
    ;

LibraryClause
    : 'Lib' StringLiteral
    ;

AliasClause
    : 'Alias' StringLiteral
    ;
```

外部メソッドの宣言は実際の実装を提供しないため、メソッド本体を持たないまたは`End`を構築します。 外部メソッドが暗黙的に共有、可能性がありますいない型のパラメーターを持つと可能性がありますいないイベントを処理またはインターフェイス メンバーを実装します。 関数の戻り値の型が指定されていないと、厳密な型が使用されている、コンパイル時エラーが発生します。 それ以外の場合、型は暗黙的に`Object`またはメソッドの型の文字の種類。 外部メソッドのパラメーターの型、戻り値の型のアクセシビリティ ドメインと同じか、外部メソッド自体のアクセシビリティ ドメインのスーパー セットがあります。

外部メソッドの宣言のライブラリの句では、メソッドを実装する外部のファイルの名前を指定します。 オプションのエイリアス句は、数値の序数を指定する文字列 (プレフィックス、`#`文字) または外部ファイル内のメソッドの名前。 単一の文字セットの修飾子を指定すると、外部メソッドの呼び出し中に文字列をマーシャ リングするために使用する文字セットが決まります。 `Unicode`修飾子を Unicode 値では、すべての文字列をマーシャ リング、`Ansi`修飾子を ANSI 値のすべての文字列をマーシャ リングと`Auto`修飾子、メソッドの名前に基づく .NET Framework の規則に従って文字列をマーシャ リングまたは指定した場合の別名です。 修飾子が指定されていない場合、既定値は`Ansi`します。

場合`Ansi`または`Unicode`が指定されている場合、外部ファイルを変更していない場合、メソッド名が検索されます。 場合`Auto`が指定されたメソッド名の検索は、プラットフォームによって異なります。 プラットフォームは、ANSI (たとえば、Windows 95、Windows 98、Windows ME) と見なされます場合、メソッド名が検索されます変更していない場合。 ルックアップに失敗した場合、`A`が追加されます、参照を再実行します。 プラットフォームは Unicode (たとえば、Windows NT、Windows 2000、Windows XP の場合) と見なされる場合、`W`が追加され、名前が検索されます。 検索がなしでもう一度しようとした場合は、参照に失敗、`W`します。 例えば:

```vb
Module Test
    ' All platforms bind to "ExternSub".
    Declare Ansi Sub ExternSub Lib "ExternDLL" ()

    ' All platforms bind to "ExternSub".
    Declare Unicode Sub ExternSub Lib "ExternDLL" ()

    ' ANSI platforms: bind to "ExternSub" then "ExternSubA".
    ' Unicode platforms: bind to "ExternSubW" then "ExternSub".
    Declare Auto Sub ExternSub Lib "ExternDLL" ()
End Module
```

外部メソッドに渡されるデータ型は、1 つの例外には、.NET Framework データ マーシャ リング規則に従ってマーシャ リングされます。 値によって渡される変数の文字列 (つまり、 `ByVal x As String`) OLE オートメーション BSTR 型には、マーシャ リングと文字列引数には、外部メソッドに BSTR に加えられた変更が反映します。 ため、これは、型`String`外部メソッドが、変更可能であり、この特殊なマーシャ リング動作を模倣します。 参照によって渡されるパラメーターの文字列 (つまり`ByRef x As String`) は、OLE オートメーション BSTR 型へのポインターとしてマーシャ リングします。 指定することによってこれらの特殊な動作をオーバーライドすることができます、`System.Runtime.InteropServices.MarshalAsAttribute`パラメーターの属性。

この例では、外部メソッドの使用を示しています。

```vb
Class Path
    Declare Function CreateDirectory Lib "kernel32" ( _
        Name As String, sa As SecurityAttributes) As Boolean
    Declare Function RemoveDirectory Lib "kernel32" ( _
        Name As String) As Boolean
    Declare Function GetCurrentDirectory Lib "kernel32" ( _
        BufSize As Integer, Buf As String) As Integer
    Declare Function SetCurrentDirectory Lib "kernel32" ( _
        Name As String) As Boolean
End Class
```


### <a name="overridable-methods"></a>オーバーライド可能なメソッド

`Overridable`修飾子は、メソッドがオーバーライドできることを示します。 `Overrides`修飾子は、メソッドが、同じシグネチャを持つ基本型のオーバーライド可能なメソッドをオーバーライドすることを示します。 `NotOverridable`修飾子は、オーバーライド可能なメソッドをさらにオーバーライドできないことを示します。 `MustOverride`修飾子は、派生クラスでメソッドをオーバーライドする必要があることを示します。

これらの修飾子の特定の組み合わせが無効です。

* `Overridable` `NotOverridable`は相互に排他的と組み合わせることはできません。

* `MustOverride` 意味`Overridable`(および指定できないように) と併用することはできません`NotOverridable`します。

* `NotOverridable` 組み合わせて使用できない`Overridable`または`MustOverride`と併用する必要があります`Overrides`します。

* `Overrides` 意味`Overridable`(および指定できないように) と併用することはできません`MustOverride`します。

オーバーライド可能なメソッドで追加の制限もあります。

* A`MustOverride`メソッドは、メソッド本体を含まない場合がありますまたは`End`構築、もう 1 つのメソッドをオーバーライドしませんおよびにのみ出現`MustInherit`クラス。

* メソッドが指定されている場合`Overrides`を上書きするコンパイル時エラーが発生したに一致する基本メソッドはありません。 オーバーライドするメソッドを指定しない場合があります`Shadows`します。

* メソッドは、オーバーライドするメソッドのアクセシビリティ ドメインでオーバーライドされるメソッドのアクセシビリティ ドメインと等しくない場合、別のメソッドをオーバーライドしない可能性があります。 1 つの例外、メソッドをオーバーライドすることです、`Protected Friend`メソッドがない別のアセンブリで`Friend`へのアクセスを指定する必要があります`Protected`(いない`Protected Friend`)。

* `Private` メソッドができない可能性があります`Overridable`、 `NotOverridable`、または`MustOverride`、やその他のメソッドをオーバーライドする可能性があります。

* メソッドで`NotInheritable`クラスが宣言されていない`Overridable`または`MustOverride`します。

次の例は、オーバーライド可能なとできる方法の相違点を示しています。

```vb
Class Base
    Public Sub F()
        Console.WriteLine("Base.F")
    End Sub

    Public Overridable Sub G()
        Console.WriteLine("Base.G")
    End Sub
End Class

Class Derived
    Inherits Base

    Public Shadows Sub F()
        Console.WriteLine("Derived.F")
    End Sub

    Public Overrides Sub G()
        Console.WriteLine("Derived.G")
    End Sub
End Class

Module Test
    Sub Main()
        Dim d As Derived = New Derived()
        Dim b As Base = d

        b.F()
        d.F()
        b.G()
        d.G()
    End Sub
End Module
```

クラスの例では、`Base`メソッドが導入されています`F`と`Overridable`メソッド`G`します。 クラスは、`Derived`新しい方法が導入されました`F`、そのため、継承されたシャドウ`F`、継承されたメソッドもオーバーライドと`G`。 この例では次の出力が生成されます。

```
Base.F
Derived.F
Derived.G
Derived.G
```

注意ステートメント`b.G()`呼び出す`Derived.G`ではなく、`Base.G`します。 これは、実行時の型のインスタンスのためです (これは`Derived`) インスタンスのコンパイル時の型ではなく (これは`Base`) を呼び出す実際のメソッドの実装を決定します。

### <a name="shared-methods"></a>共有メソッド

`Shared`修飾子は、メソッドは、ことを示します、*メソッドを共有*します。 共有メソッドでは、型の特定のインスタンスで動作せず、型の特定のインスタンスではなく、型から直接呼び出すことができます。 ただし、共有メソッドを修飾するためにインスタンスを使用するには、有効です。 参照することはできません`Me`、 `MyClass`、または`MyBase`共有メソッドで。 共有メソッドができない可能性があります`Overridable`、 `NotOverridable`、または`MustOverride`、およびメソッドを無効にする可能性があります。 標準的なモジュールとインターフェイスで定義されているメソッドは指定できません`Shared`暗黙的にいるため、`Shared`既にします。

構造体またはクラスなしで宣言されたメソッド、`Shared`修飾子は、*インスタンス メソッド*します。 インスタンス メソッドは、指定された型のインスタンスで動作します。 インスタンス メソッド、型のインスタンスを介して呼び出すことができますのみと可能性がありますからインスタンスを参照してください、`Me`式。

次の例は、共有へのアクセスとインスタンス メンバーのルールを示しています。

```vb
Class Test
    Private x As Integer
    Private Shared y As Integer

    Sub F()
        x = 1 ' Ok, same as Me.x = 1.
        y = 1 ' Ok, same as Test.y = 1.
    End Sub

    Shared Sub G()
        x = 1 ' Error, cannot access Me.x.
        y = 1 ' Ok, same as Test.y = 1.
    End Sub

    Shared Sub Main()
        Dim t As Test = New Test()

        t.x = 1 ' Ok.
        t.y = 1 ' Ok.
        Test.x = 1 ' Error, cannot access instance member through type.
        Test.y = 1 ' Ok.
    End Sub
End Class
```

メソッド`F`インスタンス関数のメンバーでは、識別子を両方のインスタンス メンバーと共有メンバーへのアクセスに使用でくことを示します。 メソッド`G`共有関数メンバーの識別子を使ってインスタンス メンバーにアクセスするとエラーをあることを示します。 メソッド`Main`こと、メンバー アクセス式でインスタンス、インスタンス メンバーにアクセスする必要がありますが、型またはインスタンスを通じてアクセスできる共有メンバーを示しています。

### <a name="method-parameters"></a>メソッドのパラメーター

A*パラメーター*におよび、メソッドからの情報を渡すために使用できる変数です。 メソッドのパラメーターは、コンマで区切られた 1 つまたは複数のパラメーターで構成されるメソッドのパラメーター リストで宣言されます。

```antlr
ParameterList
    : Parameter ( Comma Parameter )*
    ;

Parameter
    : Attributes? ParameterModifier* ParameterIdentifier ( 'As' TypeName )?
      ( Equals ConstantExpression )?
    ;

ParameterModifier
    : 'ByVal' | 'ByRef' | 'Optional' | 'ParamArray'
    ;

ParameterIdentifier
    : Identifier IdentifierModifiers
    ;
```

パラメーターの型を指定しないと、厳密な型が使用される、コンパイル時エラーが発生します。 既定の型がそれ以外の場合は`Object`またはパラメーターの型の文字の種類。 1 つのパラメーターが含まれている場合、寛容であっても、`As`句では、すべてのパラメーターの種類を指定する必要があります。

パラメーターは修飾子によってパラメーター値、参照、省略可能なまたは paramarray パラメーターとして指定された`ByVal`、 `ByRef`、 `Optional`、および`ParamArray`、それぞれします。 指定されていないパラメーター`ByRef`または`ByVal`の既定値は`ByVal`します。

パラメーターの名前は、メソッドの本文全体のスコープし、は常にパブリックにアクセスします。 メソッドの呼び出しのパラメーター、その呼び出しに固有のコピーが作成し、呼び出しの引数リストが値または変数参照を新しく作成されたパラメーターを割り当てます。 外部メソッドの宣言とデリゲートの宣言には、本文があるありません、ため、重複するパラメーター名はパラメーター リストで許可されているが、推奨されません。

識別子の名前が null 許容修飾子後にこと`?`、null 値を許可し、これが配列で配列名の修飾子を示すためにもします。 組み合わせることができます、例:"`ByVal x?() As Integer`"。 明示的な配列の境界を使用することはできません。また、名前が null 許容修飾子が存在する場合、`As`句が存在する必要があります。


#### <a name="value-parameters"></a>値パラメーター

A*値パラメーター*明示的な宣言が`ByVal`修飾子。 場合、`ByVal`修飾子を使用すると、`ByRef`修飾子が指定されていない可能性があります。 値を持つパラメーターが存在すると、呼び出しで指定された引数の値を持つパラメーターが属するで初期化されるメンバーの呼び出しに渡されます。 値を持つパラメーター、メンバーの戻り時に存在しなくなります。

値を持つパラメーターに新しい値を割り当てるメソッドが許可されます。 このような割り当てでは、値パラメーターによって表されるローカル ストレージの場所のみに影響します。実際にメソッドの呼び出しで指定された引数に影響を与えるありません。

値を持つパラメーターは、引数の値が、メソッドに渡され、元の引数のパラメーターの変更に影響しないときに使用されます。 値を持つパラメーターは、いずれかの対応する引数の変数とは異なりますが、独自の変数を指します。 この変数は、対応する引数の値をコピーして初期化されます。 次の例は、メソッドを示しています`F`という名前の値を持つパラメーターを持つ`p`:。

```vb
Module Test
    Sub F(p As Integer)
        Console.WriteLine("p = " & p)
        p += 1
    End Sub 

    Sub Main()
        Dim a As Integer = 1

        Console.WriteLine("pre: a = " & a)
        F(a)
        Console.WriteLine("post: a = " & a)
    End Sub
End Module
```

例では、生成、次の出力も、value パラメーター`p`が変更されました。

```
pre: a = 1
p = 1
post: a = 1
```

#### <a name="reference-parameters"></a>参照パラメーター

参照はで宣言されたパラメーターを`ByRef`修飾子。 場合、`ByRef`修飾子が指定された、`ByVal`修飾子を使用することはできません。 参照パラメーターは、新しい記憶域の場所を作成できません。 代わりに、参照パラメーターは、メソッドまたはコンス トラクターの呼び出しで引数として指定された変数を表します。 概念的には、参照パラメーターの値は常に、基になる変数と同じです。

参照パラメーターの動作として 2 つのモードで*エイリアス*または*コピー-バックアップ コピーで。*

__エイリアスです。__ パラメーターが呼び出し元が提供する引数のエイリアスとして機能する場合に、参照パラメーターが使用されます。 参照パラメーター自体は、変数を定義しませんが、対応する引数の変数を参照します。 直接とすぐに参照パラメーターの変更、対応する引数に影響します。 次の例は、メソッドを示しています。`Swap`を持つ 2 つの参照パラメーター。

```vb
Module Test
    Sub Swap(ByRef a As Integer, ByRef b As Integer)
        Dim t As Integer = a
        a = b
        b = t
    End Sub 

    Sub Main()
        Dim x As Integer = 1
        Dim y As Integer = 2

        Console.WriteLine("pre: x = " & x & ", y = " & y)
        Swap(x, y)
        Console.WriteLine("post: x = " & x & ", y = " & y)
    End Sub 
End Module
```

プログラムの出力は次のとおりです。

```
pre: x = 1, y = 2
post: x = 2, y = 1
```

メソッドの呼び出しの`Swap`クラスで`Main`、`a`表します`x,`と`b`表します`y`します。 このため、呼び出しは、の値を交換`x`と`y`します。

参照パラメーターをとるメソッドでは複数の名前が同じ記憶域の場所を表すことができます。

```vb
Module Test
    Private s As String

    Sub F(ByRef a As String, ByRef b As String)
        s = "One"
        a = "Two"
        b = "Three"
    End Sub

    Sub G()
        F(s, s)
    End Sub
End Module
```

メソッドの呼び出しの例では`F`で`G`への参照を渡す`s`両方の`a`と`b`します。 したがって、その呼び出しでは、名前の`s`、 `a`、および`b`すべて、同じストレージの場所を参照し、すべての 3 つの割り当ては、インスタンス変数を変更`s`します。

__コピーのコピー-バックします。__ 参照パラメーターに渡される変数の型が参照パラメーターの型と互換性がない場合または非変数 (例: プロパティ) が参照パラメーターに引数として渡された場合や、呼び出しが遅延バインディングされた一時的な場合変数が割り当てられ、参照パラメーターに渡されます。 渡される値は、メソッドが呼び出されにコピー元の変数 (存在する場合とが書き込み可能な場合など)、メソッドが戻るときの前に、この一時変数にコピーされます。 したがって、参照パラメーターは、含めることはできませんとは限りません渡される変数の正確な記憶域への参照、メソッドを終了するまで、参照パラメーターへの変更を変数に反映されません可能性があります。 例えば:

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class

Module Test
    Sub F(ByRef b As Base)
        b = New Base()
    End Sub

    Property G() As Base
        Get
        End Get
        Set
        End Set
    End Property

    Sub Main()
        Dim d As Derived

        F(G)   ' OK.
        F(d)   ' Throws System.InvalidCastException after F returns.
    End Sub
End Module
```

最初の呼び出しの場合`F`、一時変数が作成されると、プロパティの値`G`に割り当てられに渡される`F`します。 戻り時に`F`、一時変数に値がのプロパティに割り当てられている`G`します。 もう 1 つの一時変数を作成する 2 番目のケースの値と`d`に割り当てられに渡される`F`。 戻るときに`F`、一時変数に値をキャストして、変数の型に戻す`Derived`とに割り当てられている`d`します。 値から返されたにキャストできない`Derived`、実行時に例外がスローされます。

#### <a name="optional-parameters"></a>省略可能なパラメーター

省略可能なパラメーターが宣言された、`Optional`修飾子。 仮パラメーター リストで、省略可能なパラメーターを後続のパラメーターがありますオプションも;指定しない、`Optional`修飾子は、次のパラメーターでは、コンパイル時エラーをトリガーします。 いくつかのオプションのパラメーターが null 許容型を入力`T?`または非許容型`T`定数式を指定する必要があります`e`引数が指定されていない場合、既定値として使用します。 場合`e`に評価される`Nothing`オブジェクト、既定値の型の*パラメーターの型*パラメーターの既定値として使用されます。 それ以外の場合、`CType(e, T)`定数式である必要があり、パラメーターの既定値として取得されます。

省略可能なパラメーターは、パラメーター初期化子が有効である場合だけです。 初期化は、常に、メソッド本体内ではなく、invocation 式の一部として実行します。

```vb
Module Test
    Sub F(x As Integer, Optional y As Integer = 20)
        Console.WriteLine("x = " & x & ", y = " & y)
    End Sub

    Sub Main()
        F(10)
        F(30,40)
    End Sub
End Module
```

プログラムの出力は次のとおりです。

```
x = 10, y = 20
x = 30, y = 40
```

デリゲートまたはイベントの宣言にもラムダ式には、省略可能なパラメーターを指定できません。

#### <a name="paramarray-parameters"></a>ParamArray パラメーター

`ParamArray` 宣言されたパラメーターは、`ParamArray`修飾子。 場合、`ParamArray`修飾子が存在する、`ByVal`修飾子を指定する必要があります、およびその他のパラメーターを使用しない可能性があります、`ParamArray`修飾子。 `ParamArray`パラメーターの型は 1 次元配列である必要があり、パラメーター リストの最後のパラメーターがあります。

A`ParamArray`パラメーターの型のパラメーターの不確定の数を表します、`ParamArray`します。 メソッド自体内で、`ParamArray`パラメーターが宣言された型として扱われ、特別な意味を持ちません。 A`ParamArray`パラメーターは暗黙的に省略可能で、既定値は空の型の 1 次元配列の`ParamArray`します。

A`ParamArray`メソッドの呼び出しで 2 つの方法のいずれかで指定する引数を許可します。

* 指定された引数を`ParamArray`を拡張する型の 1 つの式を指定できます、`ParamArray`型。 ここで、`ParamArray`値パラメーターとまったく同じように機能します。

* または、呼び出しがの 0 個以上の引数を指定できます、`ParamArray`各引数の要素の型に暗黙的に変換できる型の式は、`ParamArray`します。 この場合、呼び出しがのインスタンスを作成、`ParamArray`引数の数、配列の要素が指定した引数の値を持つインスタンスを初期化します使用して、新しく作成された配列のインスタンスとして、実際に対応する長さを持つ型引数。

を除き、呼び出しで可変個の引数を許可、`ParamArray`次の例に示すように、同じ型の値を持つパラメーターとまったく同じです。

```vb
Module Test
    Sub F(ParamArray args() As Integer)
        Dim i As Integer

        Console.Write("Array contains " & args.Length & " elements:")
        For Each i In args
            Console.Write(" " & i)
        Next i
        Console.WriteLine()
    End Sub

    Sub Main()
        Dim a As Integer() = { 1, 2, 3 }

        F(a)
        F(10, 20, 30, 40)
        F()
    End Sub
End Module
```

この例には、出力が生成されます。

```
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

最初の呼び出し`F`配列を渡すだけ`a`値パラメーターとして。 2 番目の呼び出しの`F`自動的に指定された要素値を持つ 4 つの要素の配列を作成し、その配列インスタンス値を持つパラメーターとして渡されます。 3 つ目の呼び出しでは同様に、`F`要素が 0 の配列を作成し、値パラメーターとしてそのインスタンスを渡します。 2 番目と 3 番目の呼び出しは、書き込みを正確に同じです。

```vb
F(New Integer() {10, 20, 30, 40})
F(New Integer() {})
```

`ParamArray` デリゲートまたはイベント宣言で、パラメーターを指定しない場合があります。

### <a name="event-handling"></a>イベント処理

メソッドは、インスタンスまたは共有変数にオブジェクトによって発生したイベントを宣言によって処理できます。 イベントを処理するメソッドの宣言を指定します、`Handles`キーワードと 1 つまたは複数のイベントを一覧表示します。

```antlr
HandlesClause
    : ( 'Handles' EventHandlesList )?
    ;

EventHandlesList
    : EventMemberSpecifier ( Comma EventMemberSpecifier )*
    ;

EventMemberSpecifier
    : Identifier Period IdentifierOrKeyword
    | 'MyBase' Period IdentifierOrKeyword
    | 'MyClass' Period IdentifierOrKeyword
    | 'Me' Period IdentifierOrKeyword
    ;
```

内のイベント、`Handles`リストはピリオドで区切られた 2 つの識別子によって指定します。

* インスタンスまたは共有の変数を指定するコンテナーの型の最初の識別子がある必要があります、`WithEvents`修飾子または`MyBase`または`MyClass`または`Me`キーワードです。 それ以外の場合、コンパイル時エラーが発生します。 この変数には、このメソッドによって処理されるイベントを発生させるオブジェクトが含まれています。

* 2 番目の識別子では、最初の識別子の型のメンバーを指定する必要があります。 メンバーは、イベントにする必要があり、共有することがあります。 最初の識別子で共有変数を指定する場合、イベントを共有する必要があります、またはエラーが発生します。

ハンドラー メソッド`M`イベントの有効なイベント ハンドラーと見なされます`E`場合、ステートメント`AddHandler E, AddressOf M`も有効になります。 異なり、`AddHandler`ステートメントでは、ただし、明示的なイベント ハンドラーはか厳密な型が使用されているかどうかに関係なく引数なしでメソッドを持つイベントの処理を許可します。

```vb
Option Strict On

Class C1
    Event E(x As Integer)
End Class

Class C2
    withEvents C1 As New C1()

    ' Valid
    Sub M1() Handles C1.E
    End Sub

    Sub M2()
        ' Invalid
        AddHandler C1.E, AddressOf M1
    End Sub
End Class
```

1 つのメンバーが複数の一致するイベントを処理し、複数のメソッドが 1 つのイベントを処理します。 メソッドのアクセシビリティは、イベントを処理する機能に影響を与えません。 次の例では、メソッドがイベントを処理する方法を示しています。

```vb
Class Raiser
    Event E1()

    Sub Raise()
        RaiseEvent E1
    End Sub
End Class

Module Test
    WithEvents x As Raiser

    Sub E1Handler() Handles x.E1
        Console.WriteLine("Raised")
    End Sub

    Sub Main()
        x = New Raiser()
        x.Raise()
        x.Raise()
    End Sub
End Module
```

これが表示されます。

```
Raised
Raised
```

型は、その基本型によって提供されるすべてのイベント ハンドラーを継承します。 派生型変更できません。 任意の方法でイベント マッピングは、基本型から継承しますが、イベントに追加のハンドラーを追加することがあります。


### <a name="extension-methods"></a>拡張メソッド

メソッドは、型宣言を使用して外部からの型に追加できる*拡張メソッド*します。 拡張メソッドは、メソッド、`System.Runtime.CompilerServices.ExtensionAttribute`属性を適用します。 標準的なモジュールでのみ宣言できますされ、少なくとも 1 つのパラメーター、型、メソッドによって拡張を指定する必要があります。 たとえば、次の拡張メソッドが型を拡張`String`:

```vb
Imports System.Runtime.CompilerServices

Module StringExtensions
    <Extension> _
    Sub Print(s As String)
        Console.WriteLine(s)
    End Sub
End Module
```

__注意してください。__ Visual Basic では、標準のモジュールで宣言する拡張メソッドが必要ですが、c# などの他の言語は、その他の種類の型で宣言することでことができます。 メソッドは、ここで説明した他の規則と包含に従う限り、型がオープン ジェネリック型でないし、インスタンス化することはできません、Visual Basic で拡張メソッドを認識します。

拡張メソッドが呼び出されたときで呼び出されたインスタンスは、最初のパラメーターに渡されます。 最初のパラメーターを宣言することはできません`Optional`または`ParamArray`します。 型パラメーターを含む、任意の型は、拡張メソッドの最初のパラメーターとして表示できます。 次のメソッドが型を拡張するなど、 `Integer()`、任意の型を実装する`System.Collections.Generic.IEnumerable(Of T)`、任意のすべての型と。

```vb
Imports System.Runtime.CompilerServices

Module Extensions
    <Extension> _
    Sub PrintArray(a() As Integer)
        ...
    End Sub

    <Extension> _
    Sub PrintList(Of T)(a As IEnumerable(Of T))
        ...
    End Sub

    <Extension> _
    Sub Print(Of T)(a As T)
        ...
    End Sub
End Module
```

前の例に示すように、インターフェイスを拡張できます。 インターフェイスの拡張メソッドをのみに対して定義された拡張メソッドを持つインターフェイスを実装する型、インターフェイスによって最初に宣言されたメンバーを実装するために、メソッドの実装を指定します。 例えば:

```vb
Imports System.Runtime.CompilerServices

Interface IAction
  Sub DoAction()
End Interface

Module IActionExtensions 
    <Extension> _
    Public Sub DoAnotherAction(i As IAction) 
        i.DoAction()
    End Sub
End Module

Class C
  Implements IAction

  Sub DoAction() Implements IAction.DoAction
    ...
  End Sub

  ' ERROR: Cannot implement extension method IAction.DoAnotherAction
  Sub DoAnotherAction() Implements IAction.DoAnotherAction
    ...
  End Sub
End Class
```

拡張メソッドは、型パラメーターの型の制約をこともできと同様、拡張機能の非ジェネリック メソッドの引数の型推論できます。

```vb
Imports System.Runtime.CompilerServices

Module IEnumerableComparableExtensions
    <Extension> _
    Public Function Sort(Of T As IComparable(Of T))(i As IEnumerable(Of T)) _
        As IEnumerable(Of T)
        ...
    End Function
End Module
```

拡張メソッドは、拡張される型内の式の暗黙のインスタンスからもアクセスできます。

```vb
Imports System.Runtime.CompilerServices

Class C1
    Sub M1()
        Me.M2()
        M2()
    End Sub
End Class

Module C1Extensions
    <Extension>
    Sub M2(c As C1)
        ...
    End Sub
End Module
```

ユーザー補助のために、拡張メソッドは、コンテキストの宣言によりユーザーが持つアクセスを超えて拡張型のメンバーに余分なアクセス権があるありません--で宣言された標準のモジュールのメンバーとしても扱われます。

拡張メソッドは、標準のモジュールのメソッドがスコープの場合のみ使用できます。 それ以外の場合、元の型は、拡張されているには表示されません。 例えば:

```vb
Imports System.Runtime.CompilerServices

Class C1
End Class

Namespace N1
    Module C1Extensions
        <Extension> _
        Sub M1(c As C1)
            ...
        End Sub
    End Module
End Namespace

Module Test
    Sub Main()
        Dim c As New C1()

        ' Error: c has no member named "M1"
        c.M1()
    End Sub
End Module
```

型の拡張メソッドのみが使用可能な場合は、型を参照すると、コンパイル時エラーが生成されます。

拡張メソッドが厳密に型指定など、メンバーのバインドをすべてのコンテキストでの型のメンバーであると見なされることを確認することが重要`For Each`パターン。 例えば:

```vb
Imports System.Runtime.CompilerServices

Class C1
End Class

Class C1Enumerator
    ReadOnly Property Current() As C1
        Get
            ...
        End Get
    End Property

    Function MoveNext() As Boolean
        ...
    End Function
End Class

Module C1Extensions
    <Extension> _
    Function GetEnumerator(c As C1) As C1Enumerator
        ...
    End Function
End Module

Module Test
    Sub Main()
        Dim c As New C1()

        ' Valid
        For Each o As Object In c
            ...
        Next o
    End Sub
End Module
```

デリゲートは拡張メソッドを参照するも作成されます。 したがって、次のようなコードがあると:

```vb
Delegate Sub D1()

Module Test
    Sub Main()
        Dim s As String = "Hello, World!"
        Dim d As D1

        d = AddressOf s.Print
        d()
    End Sub
End Module
```

約と同等です。

```vb
Delegate Sub D1()

Module Test
    Sub Main()
      Dim s As String = "Hello, World!"
      Dim d As D1

      d = CType([Delegate].CreateDelegate(GetType(D1), s, _
                GetType(StringExtensions).GetMethod("Print")), D1)
      d()
    End Sub
End Module
```

__注意してください。__ Visual Basic が通常の原因となるインスタンス メソッドの呼び出しのチェックを挿入、`System.NullReferenceException`メソッドを呼び出す対象のインスタンスが場合に発生する`Nothing`します。 拡張機能のメソッドの場合の拡張メソッドが明示的にチェックする必要がありますので、このチェックを挿入する効率的な方法はありません`Nothing`します。 

__注意してください。__ として渡されるときに、値の型をボックス化されたが、`ByVal`インターフェイスとして型指定されたパラメーターに引数。  つまり、元の代わりに、構造のコピーで、拡張メソッドの副作用が動作することです。 拡張メソッドの最初の引数には、言語の制限はありません、お勧め値の型を拡張する拡張メソッドを使用しない、または値の型を拡張するには、最初のパラメーターが渡される`ByRef`側ことを確認するには効果は、元の値で動作します。

### <a name="partial-methods"></a>部分メソッド

A*部分メソッド*は署名が、メソッドの本体ではないを指定する方法です。 メソッドの本体は、同じ名前とシグネチャ、型の別の部分宣言で最も可能性の高い別のメソッド宣言で指定できます。 例えば:

a.vb:

```vb
' Designer generated code
Public Partial Class MyForm
    Private Partial Sub ValidateControls()
    End Sub

    Public Sub New()
        ' Initialize controls
        ...

        ValidateControls()
    End Sub    
End Class
```

b.vb:

```vb
Public Partial Class MyForm
    Public Sub ValidateControls()
        ' Validation logic goes here
        ...
    End Sub
End Class
```

この例では、クラスの部分宣言で`MyForm`部分メソッドを宣言`ValidateControls`実装のないです。 部分的な宣言にコンス トラクターは、ファイルで指定された本文がない場合でも、部分のメソッドを呼び出します。 その他の部分宣言`MyForm`メソッドの実装を提供します。

本文が指定されているかどうかを; に関係なく、部分メソッドを呼び出すことができます。メソッド本体が指定されていない場合、呼び出しは無視されます。 例えば:

```vb
Public Class C1
    Private Partial Sub M1()
    End Sub

    Public Sub New()
        ' Since no implementation is supplied, this call will not be made.
        M1()
    End Sub
End Class
```

無視される部分メソッドの呼び出しに引数として渡される任意の式も無視され、評価されません。 (__に注意してください。__ つまり、部分メソッドは部分メソッドにはコストがあるない場合に使用されませんので、2 つの部分型の間で定義されている動作を提供するため、非常に効率的にできます。)

部分メソッド宣言として宣言する必要があります`Private`ありませんステートメント本体内にサブルーチンを常にする必要があります。 部分メソッド自体を実装できませんインターフェイスのメソッド、メソッドの本文を提供することができます。

1 つのメソッドは、本文の部分メソッドを指定できます。 本文の部分メソッドを提供するメソッドは、部分メソッドとして同じシグネチャでの型パラメーター、同じの宣言修飾子と同じパラメーターと型パラメーター名と同じ制約が必要です。 部分メソッドおよびメソッドの本体を提供する属性のメソッドのパラメーターに任意の属性は、マージされます。 同様に、メソッドを処理するイベントの一覧に結合されます。 例えば:

```vb
Class C1
    Event E1()
    Event E2()

    Private Partial Sub S() Handles Me.E1
    End Sub

    ' Handles both E1 and E2
    Private Sub S() Handles Me.E2
        ...
    End Sub
End Class
```

## <a name="constructors"></a>コンストラクター

*コンス トラクター*初期化を制御できる特殊なメソッドです。 プログラムを開始した後、または型のインスタンスが作成されるときに実行されます。 他のメンバーとは異なり、コンス トラクターは継承されず、型の宣言領域に名前を導入しません。 コンス トラクターは、オブジェクト作成式または、.NET Framework でのみ呼び出すことがあります。これらが直接呼び出されません。

__注意してください。__ コンス トラクターでは、サブルーチンが行の配置で同じ制限があります。 最初のステートメント、end ステートメント、およびブロックする必要がありますすべて表示されますは論理行の先頭。

```antlr
ConstructorMemberDeclaration
    : Attributes? ConstructorModifier* 'Sub' 'New'
      ( OpenParenthesis ParameterList? CloseParenthesis )? LineTerminator
      Block?
      'End' 'Sub' StatementTerminator
    ;

ConstructorModifier
    : AccessModifier
    | 'Shared'
    ;
```

### <a name="instance-constructors"></a>インスタンス コンストラクター

*インスタンス コンス トラクター*型のインスタンスを初期化し、インスタンスが作成されるときに、.NET Framework で実行されます。 コンス トラクターのパラメーター リストは、メソッドのパラメーター リストと同じ規則が適用されます。 インスタンス コンス トラクターはオーバー ロードすることがあります。

参照型のすべてのコンス トラクターは、別のコンス トラクターを呼び出す必要があります。 呼び出しが明示的な場合、コンス トラクター メソッドの本体の最初のステートメントがあります。 ステートメント呼び出すことができますか、別の型のインスタンスのコンス トラクター--など`Me.New(...)`または`MyClass.New(...)`--または構造体でない場合は、型の基本型のインスタンス コンス トラクターを呼び出すこともできます - たとえば、`MyBase.New(...)`します。 それ自体を呼び出すコンス トラクターは無効です。 コンス トラクターは、別のコンス トラクターの呼び出しを付ける場合`MyBase.New()`は暗黙の型。 パラメーターなしの基本型のコンス トラクターがない場合は、コンパイル時エラーが発生します。 `Me`コンス トラクターの呼び出しステートメントのパラメーターを参照できません、基本クラス コンス トラクターの呼び出しの後までに構築されているとは見なされません`Me`、 `MyClass`、または`MyBase`暗黙的または明示的にします。

コンス トラクターの最初のステートメントの場合は、フォームの`MyBase.New(...)`、コンス トラクターが暗黙的に型で宣言されているインスタンス変数の変数の初期化子によって指定された初期化を実行します。 これは、直接の基本型のコンス トラクターの呼び出し後にすぐに実行される割り当てのシーケンスに対応します。 この順序により、基本のインスタンスのすべての変数は、インスタンスへのアクセス権を持つすべてのステートメントが実行される前に、変数の初期化子によって初期化されます。 例えば:

```vb
Class A
    Protected x As Integer = 1
End Class

Class B
    Inherits A

    Private y As Integer = x

    Public Sub New()
        Console.WriteLine("x = " & x & ", y = " & y)
    End Sub
End Class
```

ときに`New B()`のインスタンスを作成するために使用`B`、次の出力が生成されます。

```
x = 1, y = 1
```

値`y`は`1`変数初期化子は、基底クラスのコンス トラクターが呼び出された後に実行されるためです。 変数の初期化子は、型宣言に表示されるテキストの順序で実行されます。

型宣言がのみ`Private`、コンス トラクターはありません。 可能な一般型から派生または型のインスタンスを作成するには、その他の種類の唯一の例外は、型内で入れ子にされた型。 `Private` コンス トラクターは、のみを含む型でよく使用される`Shared`メンバー。

型にインスタンス コンス トラクターの宣言が含まれていない場合は、既定のコンス トラクターが自動的に提供します。 既定のコンス トラクターは、直接の基本型のパラメーターなしのコンス トラクターを呼び出すだけです。 直接の基本型が、アクセス可能なパラメーターなしのコンス トラクターを持たない場合、コンパイル時エラーが発生します。 既定のコンス トラクターの宣言されたアクセスの種類は`Public`型でない限り、 `MustInherit`、この場合、既定のコンス トラクターは`Protected`します。

__注意してください。__ 既定のアクセス、`MustInherit`型の既定のコンス トラクターは`Protected`ため`MustInherit`クラスを直接作成することはできません。 既定のコンス トラクターを行う意味がないように`Public`します。

次の例では、クラスにコンス トラクターの宣言が含まれていないため、既定のコンス トラクターが用意されています。

```vb
Class Message
    Dim sender As Object
    Dim text As String
End Class
```

そのため、例は、次とまったく同じです。

```vb
Class Message
    Dim sender As Object
    Dim text As String

    Sub New()
    End Sub
End Class
```

デザイナーに生成される既定のコンス トラクターには、属性でマークされたクラスが生成される`Microsoft.VisualBasic.CompilerServices.DesignerGeneratedAttribute`メソッドを呼び出して`Sub InitializeComponent()`基底コンス トラクターの呼び出し後、ユーザーが存在します場合。 (__に注意してください。__ これにより、デザイナー ファイルにコンス トラクターを省略する場合、WinForms デザイナーによって作成されたものなどのデザイナー生成されたファイルができます。 これにより、プログラマは自分で指定を希望する場合。)

### <a name="shared-constructors"></a>共有のコンス トラクター

*コンス トラクターを共有*共有変数の型を初期化は、プログラムが実行を開始した後は、型のメンバーへの参照の前に実行されます。 共有のコンス トラクターを指定します、`Shared`修飾子は、後者標準モジュールである場合を除き、`Shared`修飾子が暗黙的に指定します。

インスタンス コンス トラクターとは異なりは、共有コンス トラクターは暗黙的なパブリック アクセスのパラメーターを指定しないと、他のコンス トラクターを呼び出すことができません。 共有のコンス トラクターの最初のステートメントの前に、共有のコンス トラクターは暗黙的に型で宣言された共有変数の変数の初期化子によって指定された初期化を実行します。 これは、一連のコンス トラクターに入ったときにすぐに実行される割り当てに対応します。 変数の初期化子は、型宣言に表示されるテキストの順序で実行されます。

次の例は、`Employee`共有共有変数を初期化するコンス トラクターを持つクラス。

```vb
Imports System.Data

Class Employee
    Private Shared ds As DataSet

    Shared Sub New()
        ds = New DataSet()
    End Sub

    Public Name As String
    Public Salary As Decimal
End Class
```

各クローズ ジェネリック型の別の共有のコンス トラクターが存在します。 共有のコンス トラクターは、実行時のチェック制約を使用してコンパイル時にチェックすることはできませんが、型パラメーターに適用する便利な場所がクローズドの種類ごとに 1 回だけ実行されます。 次の種類は共有コンス トラクターを使用して、型パラメーターがあることを強制するなど`Integer`または`Double`:

```vb
Class EnumHolder(Of T)
    Shared Sub New() 
        If Not GetType(T).IsEnum() Then
            Throw New ArgumentException("T must be an enumerated type.")
        End If
    End Sub
End Class
```

共有コンス トラクターが明示的に定義されている場合に、いくつかの保証が提供されますが、実装によっては、ほとんどの場合は、共有コンス トラクターが実行正確に。

* 共有のコンス トラクターは、型の任意の静的フィールドに初めてアクセスする前に実行されます。

* 共有のコンス トラクターは、型の任意の静的メソッドの最初の呼び出しの前に実行されます。

* 共有のコンス トラクターは、型のどのコンス トラクターの最初の呼び出しの前に実行されます。

上記の保証は、共有のコンス トラクターが共有の初期化子を暗黙的に作成されているような状況では適用されません。 次の例からの出力では、ため、読み込みの正確な順序付けし、そのため共有コンス トラクターの実行が定義されていません。

```vb
Module Test
    Sub Main()
        A.F()
        B.F()
    End Sub
End Module

Class A
    Shared Sub New()
        Console.WriteLine("Init A")
    End Sub

    Public Shared Sub F()
        Console.WriteLine("A.F")
    End Sub
End Class

Class B
    Shared Sub New()
        Console.WriteLine("Init B")
    End Sub

    Public Shared Sub F()
        Console.WriteLine("B.F")
    End Sub
End Class
```

出力は、次のいずれかになります。

```
Init A
A.F
Init B
B.F
```

または

```
Init B
Init A
A.F
B.F
```

これに対し、次の例では、予測可能な出力が生成されます。 なお、`Shared`クラスのコンス トラクター`A`は実行されません、たとえクラス`B`から派生します。

```vb
Module Test
    Sub Main()
        B.G()
    End Sub
End Module

Class A
    Shared Sub New()
        Console.WriteLine("Init A")
    End Sub
End Class

Class B
    Inherits A

    Shared Sub New()
        Console.WriteLine("Init B")
    End Sub

    Public Shared Sub G()
        Console.WriteLine("B.G")
    End Sub
End Class
```

出力は次のようになります。

```
Init B
B.G
```

循環依存関係を構築することも`Shared`は既定で見られる変数初期化子と変数値の次の例のように、状態。

```vb
Class A
    Public Shared X As Integer = B.Y + 1
End Class

Class B
    Public Shared Y As Integer = A.X + 1

    Shared Sub Main()
        Console.WriteLine("X = " & A.X & ", Y = " & B.Y)
    End Sub
End Class
```

これには、出力が生成されます。

```
X = 1, Y = 2
```

実行する、`Main`メソッド、システムは、まず、クラスを読み込みます`B`します。 `Shared`クラスのコンス トラクター`B`の初期値を計算に進みます`Y`と、クラスを再帰的に`A`を読み込むための値`A.X`参照されます。 `Shared`クラスのコンス トラクター`A`の初期値を計算する続いて`X`、フェッチはそうすることで、*既定*の値`Y`ゼロであります。 `A.X` したがってに初期化`1`します。 読み込むプロセス`A`しが完了するの初期値の計算に返す`Y`がの結果になります`2`します。

`Main`メソッドが代わりに、見つからないクラスで`A`例に、次の出力が生成しました。

```
X = 2, Y = 1
```

循環参照を回避`Shared`変数初期化子から、一般に、クラスでこのような参照を含む読み込まれる順序を決定することはできません。

## <a name="events"></a>イベント

イベントは、特定のオカレンスをコードに通知に使用されます。 イベントの宣言から成る識別子、デリゲート型またはパラメーター リストのいずれかと、省略可能な`Implements`句。

```antlr
EventMemberDeclaration
    : RegularEventMemberDeclaration
    | CustomEventMemberDeclaration
    ;

RegularEventMemberDeclaration
    : Attributes? EventModifiers* 'Event'
      Identifier ParametersOrType ImplementsClause? StatementTerminator
    ;

InterfaceEventMemberDeclaration
    : Attributes? InterfaceEventModifiers* 'Event'
      Identifier ParametersOrType StatementTerminator
    ;

ParametersOrType
    : ( OpenParenthesis ParameterList? CloseParenthesis )?
    | 'As' NonArrayTypeName
    ;

EventModifiers
    : AccessModifier
    | 'Shadows'
    | 'Shared'
    ;

InterfaceEventModifiers
    : 'Shadows'
    ;
```

デリゲート型が指定されている場合、デリゲート型は戻り値の型をしないがあります。 パラメーター リストが指定されている場合に含めることはできません`Optional`または`ParamArray`パラメーター。 パラメーターの型やデリゲート型のアクセシビリティ ドメインは、として、同じまたは、イベント自体のアクセシビリティ ドメインのスーパー セットである必要があります。 イベントを指定することで共有する可能性があります、`Shared`修飾子。

型の宣言領域に追加されたメンバー名だけでなく、イベント宣言では、他のいくつかのメンバーが暗黙的に宣言します。 指定された名前付きイベント`X`、次のメンバー宣言領域に追加されます。

* 入れ子になったデリゲート クラス宣言の形式がメソッドの宣言である場合は、名前付き`XEventHandler`が導入されました。 入れ子になったデリゲート クラスは、メソッドの宣言と一致して、イベントとして同じアクセシビリティを持ちます。 デリゲート クラスのパラメーターにパラメーター リスト内の属性が適用されます。

* A`Private`という名前のデリゲートとして型指定されたインスタンス変数`XEvent`します。

* という名前の 2 つのメソッド`add_X`と`remove_X`することはできませんが呼び出される、オーバーライドまたはオーバー ロードします。

型が上記の名のいずれかに一致する名前を宣言しようとすると、コンパイル時エラーが発生、および、暗黙的な`add_X`と`remove_X`宣言は名前のバインドのため無視されます。 派生型でそれらをシャドウすることはできますが、オーバーライドまたは導入のメンバーのオーバー ロードすることはできません。 たとえば、クラス宣言

```vb
Class Raiser
    Public Event Constructed(i As Integer)
End Class
```

次の宣言と同じです。

```vb
Class Raiser
    Public Delegate Sub ConstructedEventHandler(i As Integer)

    Protected ConstructedEvent As ConstructedEventHandler

    Public Sub add_Constructed(d As ConstructedEventHandler)
        ConstructedEvent = _
            CType( _
                [Delegate].Combine(ConstructedEvent, d), _
                    Raiser.ConstructedEventHandler)
    End Sub

    Public Sub remove_Constructed(d As ConstructedEventHandler)
        ConstructedEvent = _
            CType( _
                [Delegate].Remove(ConstructedEvent, d), _
                    Raiser.ConstructedEventHandler)
    End Sub
End Class
```

デリゲート型を指定せずに、イベントを宣言する最も簡単かつコンパクトな構文ですが、イベントごとに新しいデリゲート型を宣言するというデメリットがあります。 たとえば、次の例では、次の 3 つの非表示のデリゲート型が作成されます、3 つすべてのイベントは、同じパラメーター リストを持っていなくて。

```vb
Public Class Button
    Public Event Click(sender As Object, e As EventArgs)
    Public Event DoubleClick(sender As Object, e As EventArgs)
    Public Event RightClick(sender As Object, e As EventArgs)
End Class
```

次の例では、イベントだけを使用して、同じデリゲート`EventHandler`:

```vb
Public Delegate Sub EventHandler(sender As Object, e As EventArgs)

Public Class Button
    Public Event Click As EventHandler
    Public Event DoubleClick As EventHandler
    Public Event RightClick As EventHandler
End Class
```

2 つの方法のいずれかでイベントを処理できます。 静的または動的にします。 静的にイベントを処理方が簡単ですし、必要なだけです、`WithEvents`変数と`Handles`句。 次の例では、クラス`Form1`静的にイベントを処理`Click`オブジェクトの`Button`:

```vb
Public Class Form1
    Public WithEvents Button1 As New Button()

    Public Sub Button1_Click(sender As Object, e As EventArgs) _
           Handles Button1.Click
        Console.WriteLine("Button1 was clicked!")
    End Sub
End Class
```

動的なイベント処理は、明示的な接続し、コードを切断する必要があります、イベントに複雑です。 ステートメント`AddHandler`ハンドラー イベント、およびステートメントを追加`RemoveHandler`イベントのハンドラーを削除します。 次の例では、クラス`Form1`を追加する`Button1_Click`のイベント ハンドラーとして`Button1`の`Click`イベント。

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

メソッドで`Disconnect`、イベント ハンドラーが削除されます。


### <a name="custom-events"></a>カスタム イベント

イベントの宣言が暗黙的にフィールドを定義して、前のセクションで説明した、よう、`add_`メソッド、および`remove_`イベント ハンドラーの追跡に使用されるメソッド。 場合によっては、ただし、かもしれませんイベント ハンドラーを追跡するためのカスタム コードを提供することが望ましい。 たとえば、うち少数のみこれまで、処理する 40 個のフィールドではなくハッシュ テーブルを使用して、追跡する 40 個のイベントを定義するクラスの各イベント ハンドラーがより効率的な可能性があります。 *カスタム イベント*を許可する、`add_X`と`remove_X`イベント ハンドラー用のカスタム ストレージを使用するメソッドを明示的に定義できます。

カスタム イベントは、例外を使用して、デリゲート型を指定するイベントが宣言ことと同じ方法で宣言されているキーワード`Custom`前に指定する必要があります、`Event`キーワード。 カスタム イベント宣言には、3 つの宣言が含まれています。`AddHandler`宣言、`RemoveHandler`宣言と`RaiseEvent`宣言します。 属性があることができますが、任意の修飾子、宣言のいずれもことができます。

```antlr
CustomEventMemberDeclaration
    : Attributes? EventModifiers* 'Custom' 'Event'
      Identifier 'As' TypeName ImplementsClause? StatementTerminator
      EventAccessorDeclaration+
      'End' 'Event' StatementTerminator
    ;

EventAccessorDeclaration
    : AddHandlerDeclaration
    | RemoveHandlerDeclaration
    | RaiseEventDeclaration
    ;

AddHandlerDeclaration
    : Attributes? 'AddHandler'
      OpenParenthesis ParameterList CloseParenthesis LineTerminator
      Block?
      'End' 'AddHandler' StatementTerminator
    ;

RemoveHandlerDeclaration
    : Attributes? 'RemoveHandler'
      OpenParenthesis ParameterList CloseParenthesis LineTerminator
      Block?
      'End' 'RemoveHandler' StatementTerminator
    ;

RaiseEventDeclaration
    : Attributes? 'RaiseEvent'
      OpenParenthesis ParameterList CloseParenthesis LineTerminator
      Block?
      'End' 'RaiseEvent' StatementTerminator
    ;
```

例えば:

```vb
Class Test
    Private Handlers As EventHandler

    Public Custom Event TestEvent As EventHandler
        AddHandler(value As EventHandler)
            Handlers = CType([Delegate].Combine(Handlers, value), _
                EventHandler)
        End AddHandler

        RemoveHandler(value as EventHandler)
            Handlers = CType([Delegate].Remove(Handlers, value), _
                EventHandler)
        End RemoveHandler

        RaiseEvent(sender As Object, e As EventArgs)
            Dim TempHandlers As EventHandler = Handlers

            If TempHandlers IsNot Nothing Then
                TempHandlers(sender, e)
            End If
        End RaiseEvent
    End Event
End Class
```

`AddHandler`と`RemoveHandler`宣言は、いずれか`ByVal`パラメーターで、イベントのデリゲート型でなければなりません。 ときに、`AddHandler`または`RemoveHandler`ステートメントが実行される (または`Handles`句が自動的にイベントを処理)、対応する宣言が呼び出されます。 `RaiseEvent`宣言は、イベントのデリゲートと同じパラメーターを受け取るし、呼び出されるときに、`RaiseEvent`ステートメントが実行されます。 すべての宣言する必要がありますを指定して、サブルーチンと見なされます。

なお`AddHandler`、`RemoveHandler`と`RaiseEvent`宣言サブルーチンが行の配置で同じ制限があります。 最初のステートメント、end ステートメント、およびブロックする必要がありますすべて表示されますは論理行の先頭。

型の宣言領域に追加されたメンバー名だけでなく、カスタム イベント宣言では、他のいくつかのメンバーが暗黙的に宣言します。 指定された名前付きイベント`X`、次のメンバー宣言領域に追加されます。

* という名前のメソッド`add_X`に対応する、`AddHandler`宣言します。

* という名前のメソッド`remove_X`に対応する、`RemoveHandler`宣言します。

* という名前のメソッド`fire_X`に対応する、`RaiseEvent`宣言します。

型が上記の名のいずれかに一致する名前を宣言しようとすると、コンパイル時エラーが発生し、名前のバインドのため、暗黙的な宣言すべて無視されます。 派生型でそれらをシャドウすることはできますが、オーバーライドまたは導入のメンバーのオーバー ロードすることはできません。

__注意してください。__ `Custom` 予約語ではありません。

#### <a name="custom-events-in-winrt-assemblies"></a>WinRT のアセンブリのカスタム イベント

Microsoft Visual Basic の 11.0 の時点でコンパイルされたファイルで宣言されたイベント`/target:winmdobj`、またはファイル内のインターフェイスで宣言されている、別の場所では、実装しは少し異なる方法で扱われます。

* 許可など特定のデリゲート型のみは通常、winmd を作成するため、外部ツール`System.EventHandler(Of T)`または`System.TypedEventHandle(Of T, U)`、および他のユーザーは禁止されます。

* `XEvent`フィールドの型が`System.Runtime.InteropServices.WindowsRuntime.EventRegistrationTokenTable(Of T)`場所`T`はデリゲート型です。

* AddHandler アクセサーを返します、 `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationToken`、RemoveHandler アクセサーは、同じ型の 1 つのパラメーターを受け取るとします。

このようなカスタム イベントの例を次に示します。

```vb
Imports System.Runtime.InteropServices.WindowsRuntime

Public NotInheritable Class ClassInWinMD
    Private XEvent As EventRegistrationTokenTable(Of EventHandler(Of Integer))

    Public Custom Event X As EventHandler(Of Integer)
        AddHandler(handler As EventHandler(Of Integer))
            Return EventRegistrationTokenTable(Of EventHandler(Of Integer)).
                   GetOrCreateEventRegistrationTokenTable(XEvent).
                   AddEventHandler(handler)
        End AddHandler

        RemoveHandler(token As EventRegistrationToken)
            EventRegistrationTokenTable(Of EventHandler(Of Integer)).
                GetOrCreateEventRegistrationTokenTable(XEvent).
                RemoveEventHandler(token)
        End RemoveHandler

        RaiseEvent(sender As Object, i As Integer)
            Dim table = EventRegistrationTokenTable(Of EventHandler(Of Integer)).
                GetOrCreateEventRegistrationTokenTable(XEvent).
                InvocationList
            If table IsNot Nothing Then table(sender, i)
        End RaiseEvent
    End Event
End Class
```


## <a name="constants"></a>定数

A*定数*型のメンバーである定数値です。

```antlr
ConstantMemberDeclaration
    : Attributes? ConstantModifier* 'Const' ConstantDeclarators StatementTerminator
    ;

ConstantModifier
    : AccessModifier
    | 'Shadows'
    ;

ConstantDeclarators
    : ConstantDeclarator ( Comma ConstantDeclarator )*
    ;

ConstantDeclarator
    : Identifier ( 'As' TypeName )? Equals ConstantExpression StatementTerminator
    ;
```

定数は、暗黙的に共有されます。 宣言が含まれている場合、`As`句では、句は、宣言によって導入されるメンバーの種類を指定します。 型を省略した場合、定数の型が推論されます。 定数の型がプリミティブ型のみありますまたは`Object`します。 として型指定された定数場合`Object`型文字がないとは、定数の実際の型の定数式の型になります。 それ以外の場合、定数の型は、定数の型の文字の種類です。

次の例は、という名前のクラスを示しています。`Constants`を持つ 2 つのパブリック定数。

```vb
Class Constants
    Public Const A As Integer = 1
    Public Const B As Integer = A + 1
End Class
```

定数の値を出力する次の例のように、クラスを介してアクセスできる`Constants.A`と`Constants.B`します。

```vb
Module Test
    Sub Main()
        Console.WriteLine(Constants.A & ", " & Constants.B)
    End Sub 
End Module
```

複数の定数を宣言する定数宣言では、単一の定数の複数の宣言と同じです。 次の例では、3 つの定数の 1 つの宣言ステートメントで宣言します。

```vb
Class A
    Protected Const x As Integer = 1, y As Long = 2, z As Short = 3
End Class
```

この宣言は、次のと同等です。

```vb
Class A
    Protected Const x As Integer = 1
    Protected Const y As Long = 2
    Protected Const z As Short = 3
End Class
```

定数の型のアクセシビリティ ドメインは、定数自体のアクセシビリティ ドメインのスーパー セットであるかと同じである必要があります。 定数式では、定数の型に暗黙的に変換できる型、または定数の型の値を生成する必要があります。 定数式に循環; できない可能性があります。つまり、定数はそれ自体の観点から定義できません。

コンパイラは、自動的に適切な順序で定数の宣言を評価します。 コンパイラの最初の評価では、次の例では、 `Y`、し`Z`、最後に`X`、それぞれ 10、11、および 12 ですが、値を生成します。

```vb
Class A
    Public Const X As Integer = B.Z + 1
    Public Const Y As Integer = 10
End Class

Class B
    Public Const Z As Integer = A.Y + 1
End Class
```

定数値のシンボル名が必要な場合は値の型が定数の宣言またはときに、値で計算できない場合コンパイル時に定数式で許可されていません、読み取り専用変数を代わりに使用可能性があります。


## <a name="instance-and-shared-variables"></a>インスタンスおよび共有変数

インスタンスまたは共有変数は、情報を格納できる型のメンバーです。

```antlr
VariableMemberDeclaration
    : Attributes? VariableModifier+ VariableDeclarators StatementTerminator
    ;

VariableModifier
    : AccessModifier
    | 'Shadows'
    | 'Shared'
    | 'ReadOnly'
    | 'WithEvents'
    | 'Dim'
    ;

VariableDeclarators
    : VariableDeclarator ( Comma VariableDeclarator )*
    ;

VariableDeclarator
    : VariableIdentifiers 'As' ObjectCreationExpression
    | VariableIdentifiers ( 'As' TypeName )? ( Equals Expression )?
    ;

VariableIdentifiers
    : VariableIdentifier ( Comma VariableIdentifier )*
    ;

VariableIdentifier
    : Identifier IdentifierModifiers
    ;
```

`Dim`修飾子がある必要があります指定なしを指定するがそれ以外の場合は省略できます。 1 つの変数宣言は、複数の変数宣言子を含めることができます。各変数の宣言子は、新しいインスタンスまたは共有メンバーについて説明します。

初期化子が指定されている場合のみに 1 つのインスタンスまたは共有変数は、変数の宣言子で宣言できます。

```vb
Class Test
    Dim a, b, c, d As Integer = 10  ' Invalid: multiple initialization
End Class
```

この制限は、オブジェクト初期化子には適用されません。

```vb
Class Test
    Dim a, b, c, d As New Collection() ' OK
End Class
```

宣言された変数、`Shared`修飾子は、*共有変数*します。 共有変数は、作成される型のインスタンスの数に関係なく正確に 1 つのストレージの場所を識別します。 共有変数プログラムが実行を開始されには、プログラムの終了時に存在しなくなります。

共有変数は、特定のクローズ ジェネリック型のインスタンス間でのみ共有されます。 たとえば、プログラムします。

```vb
Class C(Of V) 
    Shared InstanceCount As Integer = 0

    Public Sub New()  
        InstanceCount += 1 
    End Sub

    Public Shared ReadOnly Property Count() As Integer 
        Get
            Return InstanceCount
        End Get
    End Property
End Class

Class Application 
    Shared Sub Main() 
        Dim x1 As New C(Of Integer)()
        Console.WriteLine(C(Of Integer).Count)

        Dim x2 As New C(Of Double)() 
        Console.WriteLine(C(Of Integer).Count)

        Dim x3 As New C(Of Integer)() 
        Console.WriteLine(C(Of Integer).Count)
    End Sub
End Class
```

印刷します。

```
1
1
2
```

なしで宣言された変数、`Shared`修飾子と呼ばれる、*インスタンス変数*します。 クラスのすべてのインスタンスには、クラスのすべてのインスタンス変数の個別のコピーが含まれています。 参照型のインスタンス変数は、型が作成され、そのインスタンスへの参照がない場合に存在しなくなるときの新しいインスタンスが存在することには、`Finalize`メソッドが実行します。 値型のインスタンス変数が、正確に同じ有効期間が所属する変数とします。 つまり、値型の変数が存在することになるか存在しなくなりますときも、値型のインスタンス変数。

宣言子が含まれている場合、`As`句では、句は、宣言によって導入されるメンバーの種類を指定します。 型を省略すると、厳密な型が使用される、コンパイル時エラーが発生します。 それ以外の場合、メンバーの型は、暗黙的に`Object`またはメンバーの種類の文字の種類。

__注意してください。__ 構文のあいまいさはありません。 宣言子は、型を省略、場合常に次の宣言子の型が使用されます。

インスタンスまたは共有変数の型または配列要素の型のアクセシビリティ ドメインは、インスタンスまたは共有変数自体のアクセシビリティ ドメインのスーパー セットであるかと同じである必要があります。

次の例は、`Color`という名前の内部インスタンス変数を持つクラスである`redPart`、 `greenPart`、および`bluePart`:

```vb
Class Color
    Friend redPart As Short
    Friend bluePart As Short
    Friend greenPart As Short

    Public Sub New(red As Short, blue As Short, green As Short)
        redPart = red
        bluePart = blue
        greenPart = green
    End Sub
End Class
```


### <a name="read-only-variables"></a>読み取り専用変数

インスタンスまたは共有変数の宣言が含まれる場合、`ReadOnly`修飾子、宣言によって導入された変数への代入のみ発生する可能性または同じクラスのコンス トラクターの宣言の一部として。 具体的には、読み取り専用インスタンスまたは共有変数への代入は、次の状況でのみ使用できます。

* (宣言では、変数の初期化子を含む) でインスタンスまたは共有変数を導入する、変数宣言します。

* インスタンス変数、変数の宣言を含むクラスのインスタンス コンス トラクター内。 インスタンス変数は、修飾されていない方法で、またはを介してのみアクセスできます`Me`または`MyClass`します。

* 共有変数の場合は、共有変数宣言を含むクラスの共有のコンス トラクター。

共有の読み取り専用変数は便利な定数値のシンボル名が必要な場合が値の型が定数の宣言で許可されていないときや定数式がコンパイル時に値を計算することはできません。

最初の例、共有変数が宣言されている色でこのようなアプリケーション次`ReadOnly`を他のプログラムで変更されるを防ぐために。

```vb
Class Color
    Friend redPart As Short
    Friend bluePart As Short
    Friend greenPart As Short

    Public Sub New(red As Short, blue As Short, green As Short)
        redPart = red
        bluePart = blue
        greenPart = green
    End Sub 

    Public Shared ReadOnly Red As Color = New Color(&HFF, 0, 0)
    Public Shared ReadOnly Blue As Color = New Color(0, &HFF, 0)
    Public Shared ReadOnly Green As Color = New Color(0, 0, &HFF)
    Public Shared ReadOnly White As Color = New Color(&HFF, &HFF, &HFF)
End Class
```

定数と読み取り専用共有変数は、異なるセマンティクスを持ちます。 定数の値が、コンパイル時に取得した式は、定数を参照する場合は、式では、読み取り専用共有変数を参照する場合、共有変数の値は、実行時までが得られない。 次のようなアプリケーションの 2 つのプログラムで構成されるを検討してください。

file1.vb:

```vb
Namespace Program1
    Public Class Utils
        Public Shared ReadOnly X As Integer = 1
    End Class
End Namespace
```

file2.vb:

```vb
Namespace Program2
    Module Test
        Sub Main()
            Console.WriteLine(Program1.Utils.X)
        End Sub
    End Module
End Namespace
```

名前空間`Program1`と`Program2`とは別にコンパイルされる 2 つのプログラムを示します。 変数`Program1.Utils.X`として宣言されている`Shared ReadOnly`、値の出力によって、`Console.WriteLine`ステートメントがコンパイル時に、不明はではなく、実行時に取得されます。 したがって場合の値`X`が変更されたと`Program1`が再コンパイルされる、`Console.WriteLine`ステートメントの出力は、新しい値いて`Program2`は再コンパイルされません。 ただし場合、`X`定数の値をされていた`X`時に得られる`Program2`コンパイルされ、は上記の変更による影響を受ける`Program1`まで`Program2`再コンパイルされました。

### <a name="withevents-variables"></a>WithEvents 変数

インスタンスまたは共有変数を宣言することでそのインスタンスまたは共有変数のいずれかで発生するとイベントを発生させるイベントのセットを処理する型を宣言できます、`WithEvents`修飾子。 例えば:

```vb
Class Raiser
    Public Event E1()

    Public Sub Raise()
        RaiseEvent E1
    End Sub
End Class

Module Test
    Private WithEvents x As Raiser

    Private Sub E1Handler() Handles x.E1
        Console.WriteLine("Raised")
    End Sub

    Public Sub Main()
        x = New Raiser()
    End Sub
End Module
```

この例では、メソッドで`E1Handler`イベントを処理します`E1`型のインスタンスによって発生する`Raiser`インスタンス変数に格納されている`x`します。

`WithEvents`修飾子により、先頭にアンダー スコアで名前を変更し、イベント フックアップを同じ名前のプロパティと置き換える変数。 たとえば、変数の名前は`F`、名前に変更`_F`プロパティと、`F`が暗黙的に宣言します。 変数の新しい名前と別の宣言間で競合がある場合は、コンパイル時エラーが報告されます。 変数に適用されるすべての属性が、名前が変更された変数に適用されます。

によって作成される暗黙的なプロパティ、`WithEvents`フックと関連するイベント ハンドラーはこれをアンフックの宣言を行います。 プロパティが最初に呼び出し、変数に値が割り当てられると、 `remove` (存在する場合、既存のイベント ハンドラーはこれをアンフック) 変数の現在のインスタンスでは、イベントのメソッド。 次に、割り当てが行われたと、プロパティを呼び出し、 `add` (新しいイベント ハンドラーのフック) 変数の新しいインスタンスでイベントのメソッド。 次のコードは、標準のモジュールは、上記のコードに相当`Test`:

```vb
Module Test
    Private _x As Raiser

    Public Property x() As Raiser
        Get
            Return _x
        End Get

        Set (Value As Raiser)
            ' Unhook any existing handlers.
            If _x IsNot Nothing Then
                RemoveHandler _x.E1, AddressOf E1Handler
            End If

            ' Change value.
            _x = Value

            ' Hook-up new handlers.
            If _x IsNot Nothing Then
                AddHandler _x.E1, AddressOf E1Handler
            End If
        End Set
    End Property

    Sub E1Handler()
        Console.WriteLine("Raised")
    End Sub

    Sub Main()
        x = New Raiser()
    End Sub
End Module
```

インスタンスまたは共有変数として宣言することはできません`WithEvents`変数は、構造体として型指定された場合。 さらに、`WithEvents`構造体で指定することはできませんと`WithEvents`と`ReadOnly`組み合わせることはできません。

### <a name="variable-initializers"></a>変数の初期化子

インスタンスおよび共有変数の宣言では、クラスとインスタンスの変数宣言 (は共有されていない変数の宣言) 構造体では、変数の初期化子を含めることができます。 `Shared`する前に、プログラムの開始後に実行されるステートメントを代入する変数、変数初期化子の対応、`Shared`変数の最初の参照します。 インスタンス変数、変数初期化子に対応、クラスのインスタンスが作成されるときに実行されるステートメントを代入します。 構造体は、パラメーターなしのコンス トラクターを変更できないためインスタンス変数初期化子を含めることはできません。

次に例を示します。

```vb
Class Test
    Public Shared x As Double = Math.Sqrt(2.0)
    Public i As Integer = 100
    Public s As String = "Hello"
End Class

Module TestModule
    Sub Main()
        Dim a As New Test()

        Console.WriteLine("x = " & Test.x & ", i = " & a.i & ", s = " & a.s)
    End Sub
End Module
```

この例では次の出力が生成されます。

```
x = 1.4142135623731, i = 100, s = Hello
```

代入`x`クラスが読み込まれるときに発生しますへと割り当て`i`と`s`クラスの新しいインスタンスが作成されたときに発生します。

変数の初期化子の型のコンス トラクターのブロックに自動的に挿入する代入ステートメントと考えると便利です。 次の例には、いくつかのインスタンス変数初期化子が含まれています。

```vb
Class A
    Private x As Integer = 1
    Private y As Integer = -1
    Private count As Integer

    Public Sub New()
        count = 0
    End Sub

    Public Sub New(n As Integer)
        count = n
    End Sub
End Class

Class B
    Inherits A

    Private sqrt2 As Double = Math.Sqrt(2.0)
    Private items As ArrayList = New ArrayList(100)
    Private max As Integer

    Public Sub New()
        Me.New(100)
        items.Add("default")
    End Sub

    Public Sub New(n As Integer)
        MyBase.New(n - 1)
        max = n
    End Sub
End Class
```

例は、次のコードに対応各コメントが、自動的に挿入されたステートメントを示します。

```vb
Class A
    Private x, y, count As Integer

    Public Sub New()
        MyBase.New ' Invoke object() constructor.
        x = 1 ' This is a variable initializer.
        y = -1 ' This is a variable initializer.
        count = 0
    End Sub

    Public Sub New(n As Integer)
        MyBase.New ' Invoke object() constructor. 
        x = 1 ' This is a variable initializer.
        y = - 1 ' This is a variable initializer.
        count = n
    End Sub
End Class

Class B
    Inherits A

    Private sqrt2 As Double
    Private items As ArrayList
    Private max As Integer

    Public Sub New()
        Me.New(100) 
        items.Add("default")
    End Sub

    Public Sub New(n As Integer)
        MyBase.New(n - 1) 
        sqrt2 = Math.Sqrt(2.0) ' This is a variable initializer.
        items = New ArrayList(100) ' This is a variable initializer.
        max = n
    End Sub
End Class
```

すべての変数は、任意の変数の初期化子が実行される前に、その型の既定値に初期化されます。 例えば:

```vb
Class Test
    Public Shared b As Boolean
    Public i As Integer
End Class

Module TestModule
    Sub Main()
        Dim t As New Test()
        Console.WriteLine("b = " & Test.b & ", i = " & t.i)
    End Sub
End Module
```

`b`クラスが読み込まれるときに、既定値に自動的に初期化と`i`が自動的に、クラスのインスタンスが作成されたときに、既定値に初期化、上記のコードは、次の出力が生成されます。

```
b = False, i = 0
```

各変数の初期化子は、変数の型または変数の型に暗黙的に変換できる型の値を生成する必要があります。 変数の初期化子は、循環する可能性があります。 または参照先の変数の場合、値をその既定値を初期化子の目的では、その後初期化される変数を参照してください。 このような初期化子では、疑わしい値です。

変数の初期化子の 3 つの形式があります。 通常の初期化子、配列のサイズの初期化子とオブジェクト初期化子。 型名に続く等号の後に最初の 2 つのフォームが表示される、後の 2 つは、宣言自体の一部です。 初期化子の 1 つだけの形式は、任意の特定の宣言で使用可能性があります。

#### <a name="regular-initializers"></a>通常の初期化子

A*通常の初期化子*変数の型に暗黙的に変換される式を指定します。 型名に依存し、値として分類する必要があります、等号の後に表示されます。 例えば:

```vb
Module Test
    Dim x As Integer = 10
    Dim y As Integer = 20

    Sub Main()
        Console.WriteLine("x = " & x & ", y = " & y)
    End Sub
End Module
```

このプログラムでは、出力が生成されます。

```vb
x = 10, y = 20
```

変数の宣言に通常の初期化子がある場合、一度に 1 つの変数を宣言できます。 例えば:

```vb
Module Test
    Sub Main()
        ' OK, only one variable declared at a time.
        Dim x As Integer = 10, y As Integer = 20

        ' Error: Can't initialize multiple variables at once.
        Dim a, b As Integer = 10
    End Sub
End Module
```

#### <a name="object-initializers"></a>オブジェクト初期化子

*オブジェクト初期化子*型名の代わりに、オブジェクトの作成式を使用して指定されます。 オブジェクト初期化子は、通常の初期化子オブジェクトの作成式の結果を変数に代入と同じです。 So

```vb
Module TestModule
    Sub Main()
        Dim x As New Test(10)
    End Sub
End Module
```

上記の式は、次の式と同じです。

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test(10)
    End Sub
End Module
```

オブジェクト初期化子内のかっこは、コンス トラクターの引数リストとは配列型修飾子としてことはありませんは常に解釈されます。 オブジェクト初期化子と変数名には、配列型修飾子または null 許容型修飾子を含めることはできません。

#### <a name="array-size-initializers"></a>配列のサイズの初期化子

*配列サイズの初期化子*は式で表されるディメンションの上限値のセットを提供する変数の名前の修飾子です。

```antlr
ArraySizeInitializationModifier
    : OpenParenthesis BoundList CloseParenthesis ArrayTypeModifiers?
    ;

BoundList
    : Bound ( Comma Bound )*
    ;

Bound
    : Expression
    | '0' 'To' Expression
    ;
```

上限値式の値として分類する必要があり、暗黙的に変換できる必要があります`Integer`します。 上限の設定は、上限を指定した配列作成式の変数の初期化子と同じです。 配列型の次元数は、サイズの配列初期化子から推測されます。 So

```vb
Module Test
    Sub Main()
        Dim x(5, 10) As Integer
    End Sub
End Module
```

上記の式は、次の式と同じです。

```vb
Module Test
    Sub Main()
        Dim x As Integer(,) = New Integer(5, 10) {}
    End Sub
End Module
```

すべての上限が-1 以上にする必要があり、すべてのディメンションは上限が指定されている必要があります。 初期化される配列の要素型が配列型の場合、配列型修飾子は配列のサイズの初期化子の右に移動します。 次に例を示します。

```vb
Module Test
    Sub Main()
        Dim x(5,10)(,,) As Integer
    End Sub
End Module
```

ローカル変数を宣言`x`型がの 3 次元の配列の 2 次元配列`Integer`の境界を含む配列に初期化された`0..5`の最初の次元と`0..10`2 番目の次元。 変数の型は、配列の配列の要素を初期化するために配列サイズの初期化子を使用することはできません。

配列サイズの初期化子と変数宣言では、その型または通常の初期化子に配列の型修飾子を含めることはできません。


### <a name="systemmarshalbyrefobject-classes"></a>System.MarshalByRefObject クラス

クラスから派生するクラス`System.MarshalByRefObject`はプロキシを使用してコンテキストの境界を越えてマーシャ リング (は、参照渡しで) コピーではなく (つまり、値によって)。 つまり、このようなクラスのインスタンスは true。 インスタンスができない可能性がありますが、代わりに単なる変数をマーシャ リングするスタブにアクセスし、コンテキストの境界を越えてメソッドを呼び出します。

その結果、このようなクラスで定義された変数の格納場所への参照を作成することはできません。 派生したクラスが変数に型指定されたつまり`System.MarshalByRefObject`パラメーター、およびメソッドと変数の値の型にはアクセスできない型の変数の参照を渡すことができません。 代わりに、Visual Basic では、変数の場合と同様のプロパティ (制限は、同じプロパティでは) ために、このようなクラスで定義されているが扱われます。

このルールを 1 つの例外があります: メンバー暗黙的または明示的に修飾`Me`上記の制限から除外されてため`Me`常に、プロキシではない実際のオブジェクトにすることが保証されます。

## <a name="properties"></a>プロパティ

*プロパティ*変数の自然な拡張機能は、関連付けられた型を持つメンバーを両方とも; と変数とプロパティにアクセスするための構文は同じです。 変数と違って、ただし、プロパティを表しません記憶域の場所。 プロパティの代わりに、ある*アクセサー*、読み取り、またはその値を記述するために実行するステートメントを指定します。

プロパティは、プロパティ宣言で定義されます。 プロパティ宣言の最初の部分では、フィールド宣言に似ています。 2 番目の部分が含まれています、`Get`アクセサーおよび`Set`アクセサー。

```antlr
PropertyMemberDeclaration
    : RegularPropertyMemberDeclaration
    | MustOverridePropertyMemberDeclaration
    | AutoPropertyMemberDeclaration
    ;

PropertySignature
    : 'Property'
      Identifier ( OpenParenthesis ParameterList? CloseParenthesis )?
      ( 'As' Attributes? TypeName )?
    ;

RegularPropertyMemberDeclaration
    : Attributes? PropertyModifier* PropertySignature
      ImplementsClause? LineTerminator
      PropertyAccessorDeclaration+
      'End' 'Property' StatementTerminator
    ;

MustOverridePropertyMemberDeclaration
    : Attributes? MustOverridePropertyModifier+ PropertySignature
      ImplementsClause? StatementTerminator
    ;

AutoPropertyMemberDeclaration
    : Attributes? AutoPropertyModifier* 'Property' Identifier
      ( OpenParenthesis ParameterList? CloseParenthesis )?
      ( 'As' Attributes? TypeName )? ( Equals Expression )?
      ImplementsClause? LineTerminator
    | Attributes? AutoPropertyModifier* 'Property' Identifier
      ( OpenParenthesis ParameterList? CloseParenthesis )?
      'As' Attributes? 'New'
      ( NonArrayTypeName ( OpenParenthesis ArgumentList? CloseParenthesis )? )?
      ObjectCreationExpressionInitializer?
      ImplementsClause? LineTerminator
    ;

InterfacePropertyMemberDeclaration
    : Attributes? InterfacePropertyModifier* PropertySignature StatementTerminator
    ;

AutoPropertyModifier
    : AccessModifier
    | 'Shadows'
    | 'Shared'
    | 'Overridable'
    | 'NotOverridable'
    | 'Overrides'
    | 'Overloads'
    ;

PropertyModifier
    : AutoPropertyModifier
    | 'Default'
    | 'ReadOnly'
    | 'WriteOnly'
    | 'Iterator'
    ;

MustOverridePropertyModifier
    : PropertyModifier
    | 'MustOverride'
    ;

InterfacePropertyModifier
    : 'Shadows'
    | 'Overloads'
    | 'Default'
    | 'ReadOnly'
    | 'WriteOnly'
    ;

PropertyAccessorDeclaration
    : PropertyGetDeclaration
    | PropertySetDeclaration
    ;
```

次の例で、`Button`クラスを定義、`Caption`プロパティ。

```vb
Public Class Button
    Private captionValue As String

    Public Property Caption() As String
        Get
            Return captionValue
        End Get

        Set (Value As String)
            captionValue = value
            Repaint()
        End Set
    End Property

    ...
End Class
```

に基づいて、`Button`上記のクラス、例を次の使用、`Caption`プロパティ。

```vb
Dim okButton As Button = New Button()

okButton.Caption = "OK" ' Invokes Set accessor.
Dim s As String = okButton.Caption ' Invokes Get accessor.
```

ここでは、`Set`アクセサーがプロパティの値を割り当てることによって呼び出されると、`Get`アクセサーは、式でプロパティを参照することによって呼び出されます。

プロパティの型を指定しないと、厳密な型が使用されている、コンパイル時エラーが発生します。それ以外の場合、プロパティの型は、暗黙的に`Object`またはプロパティの型の文字の種類。 プロパティ宣言は、いずれかを含めることができます、 `Get` 、プロパティの値を取得するには、アクセサーを`Set`アクセサーで、プロパティ、またはその両方の値を格納します。 プロパティは、メソッドを暗黙的に宣言するため、メソッドとして同じ修飾子を持つプロパティを宣言することがあります。 プロパティがインターフェイスで定義されているかで定義されているかどうか、`MustOverride`修飾子、プロパティ本体、`End`構文を省略する必要があります。 それ以外の場合、コンパイル時エラーが発生します。

インデックスのパラメーター リストは、インデックス パラメーターのプロパティの型ではなく、プロパティがオーバー ロードするため、プロパティの署名構成します。 インデックスのパラメーター リストは、通常のメソッドと同じです。 ただし、パラメーターがない修正が可能で、`ByRef`修飾子とそれらのいずれが付けられて`Value`(で暗黙の値パラメーター用に予約されている、`Set`アクセサー)。

プロパティは、次のように宣言できます。

* プロパティが両方が必要プロパティがプロパティ型修飾子を指定しない場合、`Get`アクセサーと`Set`アクセサー。 プロパティを読み取り/書き込みプロパティであるといいます。

* プロパティが指定されている場合、`ReadOnly`修飾子は、プロパティには、`Get`アクセサーがないと、`Set`アクセサー。 読み取り専用プロパティ、プロパティと言います。 割り当ての対象となる読み取り専用プロパティのコンパイル時エラーになります。

* プロパティが指定されている場合、`WriteOnly`修飾子は、プロパティには、`Set`アクセサーがないと、`Get`アクセサー。 プロパティを書き込み専用プロパティであるといいます。 割り当ての対象として、またはメソッドに引数として場合を除き、式での書き込み専用プロパティを参照すると、コンパイル時エラーになります。

`Get`と`Set`プロパティのアクセサーが個別のメンバーでないし、プロパティのアクセサーを個別に宣言することはできません。 次の例では、1 つの読み取り/書き込みプロパティを宣言していません。 読み取り専用のいずれか、同じ名前の 2 つのプロパティを宣言ではなく、書き込み専用の 1 つ。

```vb
Class A
    Private nameValue As String

    ' Error, contains a duplicate member name.
    Public ReadOnly Property Name() As String 
        Get
            Return nameValue
        End Get
    End Property

    ' Error, contains a duplicate member name.
    Public WriteOnly Property Name() As String 
        Set (Value As String)
            nameValue = value
        End Set
    End Property
End Class
```

同じクラスで宣言されている 2 つのメンバーは、同じ名前を持つことはできません、ため例では、によってコンパイル時エラーが発生します。

既定のプロパティのアクセシビリティ`Get`と`Set`アクセサーは、プロパティ自体のアクセシビリティと同じです。 ただし、`Get`と`Set`アクセサーがプロパティから個別にアクセシビリティを指定できますも。 その場合は、アクセサーのアクセシビリティはプロパティのアクセシビリティよりもより制限の厳しいである必要があり、1 つだけのアクセサーがプロパティから別のアクセシビリティ レベルを持つことができます。 アクセスの種類は、次のように制限を調整に考えられます。

* `Private` 限定的な`Public`、 `Protected Friend`、 `Protected`、または`Friend`します。

* `Friend` 限定的な`Protected Friend`または`Public`します。

* `Protected` 限定的な`Protected Friend`または`Public`します。

* `Protected Friend` 限定的な`Public`します。

プロパティのアクセサーのいずれかがアクセスできるが、もう 1 つが、読み取り専用または書き込み専用の場合と、プロパティが扱われます。 例えば:

```vb
Class A
    Public Property P() As Integer
        Get
            ...
        End Get

        Private Set (Value As Integer)
            ...
        End Set
    End Property
End Class

Module Test
    Sub Main()
        Dim a As A = New A()

        ' Error: A.P is read-only in this context.
        a.P = 10
    End Sub
End Module
```

派生型では、プロパティをシャドウする場合に派生プロパティの読み取りと書き込みの両方に対してシャドウされたプロパティを非表示にします。 次の例では、`P`プロパティ`B`を非表示に、`P`プロパティ`A`の読み取りと書き込みの両方に関して。

```vb
Class A
    Public WriteOnly Property P() As Integer
        Set (Value As Integer)
        End Set
    End Property
End Class

Class B
    Inherits A

    Public Shadows ReadOnly Property P() As Integer
       Get
       End Get
    End Property
End Class

Module Test
    Sub Main()
        Dim x As B = New B

        B.P = 10     ' Error, B.P is read-only.
    End Sub
End Module
```

戻り値の型やパラメーターの型のアクセシビリティ ドメインは、プロパティ自体のアクセシビリティ ドメインのスーパー セットであるかと同じである必要があります。 プロパティが 1 つを持つだけ`Set`アクセサーと 1 つ`Get`アクセサー。

宣言と呼び出しの構文での相違点を除いて`Overridable`、 `NotOverridable`、 `Overrides`、 `MustOverride`、および`MustInherit`プロパティの動作と同様に`Overridable`、 `NotOverridable`、 `Overrides`、 `MustOverride`、および`MustInherit`メソッド。 プロパティがオーバーライドされると、同じ種類 (読み取り/書き込み、書き込み専用、読み取り専用) のオーバーライドするプロパティがあります。 `Overridable`プロパティを含めることはできません、`Private`アクセサー。

次の例で`X`は、`Overridable`読み取り専用のプロパティ`Y`は、`Overridable`読み取り/書き込みプロパティ、および`Z`は、`MustOverride`読み取り/書き込みプロパティ。

```vb
MustInherit Class A
    Private _y As Integer

    Public Overridable ReadOnly Property X() As Integer
        Get
            Return 0
        End Get
    End Property

    Public Overridable Property Y() As Integer
        Get
            Return _y
         End Get
        Set (Value As Integer)
            _y = value
        End Set
    End Property

    Public MustOverride Property Z() As Integer
End Class
```

`Z`は`MustOverride`、クラスを含む`A`として宣言されなければなりません`MustInherit`します。

これに対し、クラスから派生したクラス`A`を次に示します。

```vb
Class B
    Inherits A

    Private _z As Integer

    Public Overrides ReadOnly Property X() As Integer
        Get
            Return MyBase.X + 1
        End Get
    End Property

    Public Overrides Property Y() As Integer
        Get
            Return MyBase.Y
        End Get
        Set (Value As Integer)
            If value < 0 Then
                MyBase.Y = 0
            Else
                MyBase.Y = Value
            End If
        End Set
    End Property

    Public Overrides Property Z() As Integer
        Get
            Return _z
        End Get
        Set (Value As Integer)
            _z = Value
        End Set
    End Property
End Class
```

ここでは、プロパティの宣言`X`、`Y`、および`Z`基本プロパティを上書きします。 各プロパティの宣言は、アクセシビリティ修飾子、型、および対応する継承されたプロパティの名前を正確に一致します。 `Get`プロパティのアクセサー`X`と`Set`プロパティのアクセサー`Y`を使用して、`MyBase`継承されたプロパティにアクセスするキーワード。 プロパティの宣言`Z`よりも優先されます、`MustOverride`プロパティ - そのため、いいえ未処理ありますは`MustOverride`クラスのメンバー`B`と`B`正規クラスにすることが許可されています。

最初に参照される時点まで、リソースの初期化を遅延プロパティを使用できます。 例えば:

```vb
Imports System.IO

Public Class ConsoleStreams
    Private Shared reader As TextReader
    Private Shared writer As TextWriter
    Private Shared errors As TextWriter

    Public Shared ReadOnly Property [In]() As TextReader
        Get
            If reader Is Nothing Then
                reader = Console.In
            End If
            Return reader
        End Get
    End Property

    Public Shared ReadOnly Property Out() As TextWriter
        Get
            If writer Is Nothing Then
                writer = Console.Out
            End If
            Return writer
        End Get
    End Property

    Public Shared ReadOnly Property [Error]() As TextWriter
        Get
            If errors Is Nothing Then
                errors = Console.Error
            End If
            Return errors
        End Get
    End Property
End Class
```

`ConsoleStreams`クラスには、3 つのプロパティが含まれています。 `In`、 `Out`、および`Error`、標準入力、出力、およびデバイスのエラーをそれぞれ表すです。 これらのメンバーのプロパティとして公開することで、`ConsoleStreams`クラスは実際に使用されるまで初期化を遅らせることができます。 最初に参照する時に、たとえば、`Out`に示すように、プロパティ`ConsoleStreams.Out.WriteLine("hello, world")`、基になる`TextWriter`出力デバイスが初期化されるのです。 アプリケーションへの参照がない場合は、`In`と`Error`それらのデバイス プロパティ、そのオブジェクトが作成されます。


### <a name="get-accessor-declarations"></a>アクセサーの宣言を取得します。

A`Get`アクセサー (ゲッター) が、プロパティを使用して宣言されている`Get`宣言します。 プロパティ`Get`宣言キーワードから成る`Get`にステートメント ブロックを続けます。 という名前のプロパティを指定された`P`、`Get`アクセサー宣言は、名前を持つメソッドを暗黙的に宣言`get_P`同じ修飾子、型、およびパラメーター リストのプロパティとして使用します。 型に同じ名前の宣言が含まれている場合、コンパイル時エラーが発生しますが、暗黙の宣言は名前のバインドのため無視されます。

暗黙的に宣言されている特別なローカル変数、`Get`プロパティと同じ名前でアクセサー本体の宣言領域は、プロパティの戻り値を表します。 ローカルの変数は、式で使用する場合の特別な名前解決のセマンティクスを持っています。 Invocation 式など、メソッド グループとして分類される式が必要とするコンテキストでローカル変数が使用されるローカル変数ではなく、関数に名前が解決します。 例えば:

```vb
ReadOnly Property F(i As Integer) As Integer
    Get
        If i = 0 Then
            F = 1    ' Sets the return value.
        Else
            F = F(i - 1) ' Recursive call.
        End If
    End Get
End Property
```

かっこを使用してあいまいな状況が発生することができます (など`F(1)`場所`F`は 1 次元配列の型を持つプロパティです)。 すべてのあいまいな状況では、名前は、ローカル変数ではなく、プロパティに解決されます。 例えば:

```vb
ReadOnly Property F(i As Integer) As Integer()
    Get
        If i = 0 Then
            F = new Integer(2) { 1, 2, 3 }
        Else
            F = F(i - 1) ' Recursive call, not index.
        End If
    End Get
End Property
```

コントロール フロー リーフ、`Get`アクセサーの本体のローカル変数の値は呼び出しの式に渡されます。 呼び出しているため、`Get`アクセサーは概念的には、変数の値を読み取るのと同じですが、プログラミング スタイルは不適切と見なされます`Get`アクセサーが副作用を持つ次の例に示すようにします。

```vb
Class Counter
    Private Value As Integer

    Public ReadOnly Property NextValue() As Integer
        Get
            Value += 1
            Return Value
        End Get
    End Property
End Class
```

値、`NextValue`プロパティは、プロパティにアクセスした回数によって異なります。 そのため、監視可能な副作用を生成するプロパティにアクセスして、プロパティは、メソッドとして代わりに実装する必要があります。

「副作用なし」の規則`Get`アクセサーがあることを意味`Get`を単に変数に格納されている値を返すアクセサーを記述する必要があります。 実際、`Get`アクセサーは多くの場合、複数の変数へのアクセスやメソッドの呼び出しによってプロパティの値を計算します。 ただし、適切にデザインされた`Get`アクセサーは、オブジェクトの状態の監視可能な変更を引き起こす操作を行いません。

__注意してください。__ `Get` アクセサーには、サブルーチンが行の配置で同じ制限があります。 最初のステートメント、end ステートメント、およびブロックする必要がありますすべて表示されますは論理行の先頭。

```antlr
PropertyGetDeclaration
    : Attributes? AccessModifier? 'Get' LineTerminator
      Block?
      'End' 'Get' StatementTerminator
    ;
```

### <a name="set-accessor-declarations"></a>Set アクセサーの宣言

A`Set`アクセサー (セッター) は、プロパティの設定宣言を使用して宣言されています。 プロパティの設定宣言キーワードから成る`Set`、オプションのパラメーター リストと、ステートメント ブロックです。 という名前のプロパティを指定された`P`、set アクセス操作子の宣言が暗黙的に名前を持つメソッドを宣言します`set_P`同じ修飾子およびプロパティとパラメーターの一覧を使用します。 型に同じ名前の宣言が含まれている場合、コンパイル時エラーが発生しますが、暗黙の宣言は名前のバインドのため無視されます。

パラメーター リストが指定されている場合がありますいずれかのメンバー、そのメンバーが修飾子を除く`ByVal`、その型は、プロパティの型と同じである必要があります。 パラメーターが設定されているプロパティ値を表します。 パラメーターが名前付きパラメーターを省略すると、`Value`が暗黙的に宣言します。

__注意してください。__ `Set` アクセサーには、サブルーチンが行の配置で同じ制限があります。 最初のステートメント、end ステートメント、およびブロックする必要がありますすべて表示されますは論理行の先頭。

```antlr
PropertySetDeclaration
    : Attributes? AccessModifier? 'Set'
      ( OpenParenthesis ParameterList? CloseParenthesis )? LineTerminator
      Block?
      'End' 'Set' StatementTerminator
    ;
```

### <a name="default-properties"></a>既定のプロパティ

修飾子を指定するプロパティ`Default`と呼ばれますが、*プロパティの既定*します。 プロパティを利用できる任意の型は、インターフェイスなど、既定のプロパティがあります。 プロパティの名前を持つインスタンスを修飾することがなく、既定のプロパティを参照できます。 したがって、クラスを指定

```vb
Class Test
    Public Default ReadOnly Property Item(i As Integer) As Integer
        Get
            Return i
        End Get
    End Property
End Class
```

コード

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test()
        Dim y As Integer

        y = x(10)
    End Sub
End Module
```

上記の式は、次の式と同じです。

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test()
        Dim y As Integer

        y = x.Item(10)
    End Sub
End Module
```

プロパティが宣言される`Default`、宣言されていないかどうか、既定のプロパティがすべてのプロパティの継承階層内でその名前でオーバー ロードになる`Default`かどうか。 プロパティの宣言`Default`基底クラスが別の名前で、既定のプロパティは、その他の修飾子などを宣言するときに、派生クラスで`Shadows`または`Overrides`します。 これは、既定のプロパティには、id または署名があるないため、そのため、影付きまたはオーバー ロードできません。 例えば:

```vb
Class Base
    Public ReadOnly Default Property Item(i As Integer) As Integer
        Get
            Console.WriteLine("Base = " & i)
        End Get
    End Property
End Class

Class Derived
    Inherits Base

    ' This hides Item, but does not change the default property.
    Public Shadows ReadOnly Property Item(i As Integer) As Integer
        Get
            Console.WriteLine("Derived = " & i)
        End Get
    End Property
End Class

Class MoreDerived
    Inherits Derived

    ' This declares a new default property, but not Item.
    ' This does not need to be declared Shadows
    Public ReadOnly Default Property Value(i As Integer) As Integer
        Get
            Console.WriteLine("MoreDerived = " & i)
        End Get
    End Property
End Class

Module Test
    Sub Main()
        Dim x As MoreDerived = New MoreDerived()
        Dim y As Integer
        Dim z As Derived = x

        y = x(10)        ' Calls MoreDerived.Value.
        y = x.Item(10)   ' Calls Derived.Item
        y = z(10)        ' Calls Base.Item
    End Sub
End Module
```

このプログラムの出力が生成されます。

```
MoreDerived = 10
Derived = 10
Base = 10
```

型の中で宣言されているすべての既定のプロパティは、同じ名前を指定する必要があり、わかりやすくするために、指定する必要があります、`Default`修飾子。 外側のクラスのインスタンスを割り当てるときに、インデックス パラメーターのない既定のプロパティがあいまいな状況になる、ため既定のプロパティはインデックス パラメーターを持つ必要があります。 さらで 1 つのプロパティがオーバー ロードの場合、特定の名前が含まれます。、`Default`修飾子は、その名前でオーバー ロードされたすべてのプロパティに指定してください。 既定のプロパティができない可能性があります`Shared`、プロパティの少なくとも 1 つのアクセサーがすることはできませんと`Private`します。

### <a name="automatically-implemented-properties"></a>自動実装プロパティ

アクセサーの宣言を省略すると、プロパティ場合、プロパティの実装が自動的を指定するプロパティがインターフェイスで宣言されているかが宣言されている場合を除き、`MustOverride`します。 引数なしで読み取り/書き込みプロパティのみを自動的に実装できます。それ以外の場合、コンパイル時エラーが発生します。

自動的に実装されたプロパティを`x`、別のプロパティをオーバーライドする 1 つでも、プライベートのローカル変数が導入されています`_x`プロパティと同じ型を持つ。 ローカル変数の名前と別の宣言間で競合がある場合は、コンパイル時エラーが報告されます。 自動的に実装されたプロパティの`Get`アクセサーには、ローカル プロパティの値が返されます`Set`ローカルの値を設定するアクセサーを指定します。 たとえば、次のような宣言があるとします。

```vb
Public Property x() As Integer
```

約と同等です。

```vb
Private _x As Integer
Public Property x() As Integer
    Get
        Return _x
    End Get
    Set (value As Integer)
        _x = value
    End Set
End Property
```

変数の宣言と同様、自動的に実装されたプロパティは、初期化子を含めることができます。 例えば:

```vb
Public Property x() As Integer = 10
Public Shared Property y() As New Customer() With { .Name = "Bob" }
```

__注意してください。__ 自動的に実装されたプロパティが初期化されると、基になるフィールドではないプロパティで初期化されます。 これは、このようにする必要がある場合、初期化をインターセプト プロパティをオーバーライドすることができます。

配列初期化子は、配列の境界を明示的に指定する方法はありませんが、自動的に実装されたプロパティで許可されます。  例えば:

```vb
' Valid
Property x As Integer() = {1, 2, 3}
Property y As Integer(,) = {{1, 2, 3}, {12, 13, 14}, {11, 10, 9}}

' Invalid
Property x4(5) As Short
```

### <a name="iterator-properties"></a>反復子のプロパティ

*反復子プロパティ*プロパティ、`Iterator`修飾子。 使用されてと同じ理由で反復子メソッド (セクション[反復子メソッド](statements.md#iterator-methods))、シーケンスを生成する便利な方法として--使用で使用できる 1 つ、`For Each`ステートメント。 `Get`反復子のプロパティのアクセサーが反復子メソッドと同じ方法で解釈されます。

反復子のプロパティを明示的にいる必要があります`Get`アクセサー、およびその型である必要があります`IEnumerator`、または`IEnumerable`、または`IEnumerator(Of T)`または`IEnumerable(Of T)`一部`T`します。

反復子のプロパティの例を次に示します。

```vb
Class Family
    Property Daughters As New List(Of String) From {"Beth", "Diane"}
    Property Sons As New List(Of String) From {"Abe", "Carl"}

    ReadOnly Iterator Property Children As IEnumerable(Of String)
        Get
            For Each name In Daughters : Yield name : Next
            For Each name In Sons : Yield name : Next
        End Get
    End Property
End Class

Module Module1
    Sub Main()
        Dim x As New Family
        For Each c In x.Children
            Console.WriteLine(c) ' prints Beth, Diane, Abe, Carl
        Next
    End Sub
End Module
```

## <a name="operators"></a>演算子

*演算子*方法は、含んでいるクラスの既存の Visual Basic の演算子の意味を定義します。 演算子が式内のクラスに適用されるときに、演算子は、クラスで定義されている演算子メソッドの呼び出しにコンパイルされます。 演算子を定義するクラスとも呼ばれます*オーバー ロード*演算子。

```antlr
OperatorDeclaration
    : Attributes? OperatorModifier* 'Operator' OverloadableOperator
      OpenParenthesis ParameterList CloseParenthesis
      ( 'As' Attributes? TypeName )? LineTerminator
      Block?
      'End' 'Operator' StatementTerminator
    ;

OperatorModifier
    : 'Public' | 'Shared' | 'Overloads' | 'Shadows' | 'Widening' | 'Narrowing'
    ;

OverloadableOperator
    : '+' | '-' | '*' | '/' | '\\' | '&' | 'Like' | 'Mod' | 'And' | 'Or' | 'Xor'
    | '^' | '<' '<' | '>' '>' | '=' | '<' '>' | '>' | '<' | '>' '=' | '<' '='
    | 'Not' | 'IsTrue' | 'IsFalse' | 'CType'
    ;
```

既に存在する; 演算子をオーバー ロードすることはできません。実際には、これが変換演算子に当てはまります主になりました。 たとえば、派生クラスから基底クラスへの変換をオーバー ロードすることはできません。

```vb
Class Base
End Class

Class Derived
    ' Cannot redefine conversion from Derived to Base,
    ' conversion will be ignored.
    Public Shared Widening Operator CType(s As Derived) As Base
        ...
    End Operator
End Class
```

演算子は、単語の一般的な意味でもオーバー ロードできます。

```vb
Class Base
    Public Shared Widening Operator CType(b As Base) As Integer
        ...
    End Operator

    Public Shared Narrowing Operator CType(i As Integer) As Base
        ...
    End Operator
End Class
```

演算子の宣言明示的に追加しない名前に含まれる型の宣言領域です。ただし、文字"op_"で始まる対応するメソッドを暗黙的に宣言が操作を行います。 次のセクションでは、各演算子に対応するメソッド名を一覧表示します。

定義可能な演算子の 3 つのクラスがある: 単項演算子、二項演算子、および変換演算子。 すべての演算子の宣言では、特定の制限を共有します。

* 演算子の宣言は常にあります`Public`と`Shared`します。 `Public`修飾子と想定のコンテキストで修飾子を省略できます。

* 演算子のパラメーターを宣言することはできません`ByRef`、`Optional`または`ParamArray`します。

* 少なくとも 1 つのオペランドまたは戻り値の型は、演算子が含まれる型である必要があります。

* 演算子に対して定義されている関数の戻り変数はありません。 そのため、`Return`演算子の本文から値を返すステートメントを使用する必要があります。

これらの制限の唯一の例外は、null 許容値型に適用されます。 Null 許容値型は、実際の型定義を持っていないために、値型は、ユーザー定義演算子の型の null 許容バージョンを宣言できます。 特定のユーザー定義演算子に型を宣言できるかどうかを決定するときに、`?`修飾子がからすべての宣言に含まれる型の有効性チェックの目的で最初に削除されます。 この緩和したトークンは、戻り値の型には適用されません、`IsTrue`と`IsFalse`演算子も返す必要がある`Boolean`ではなく、`Boolean?`します。

演算子の宣言では、優先順位と演算子の結合を変更できません。

__注意してください。__ 演算子では、サブルーチンが行の配置で同じ制限があります。 最初のステートメント、end ステートメント、およびブロックする必要がありますすべて表示されますは論理行の先頭。


### <a name="unary-operators"></a>単項演算子

次のような単項演算子をオーバー ロードできます。

* 単項プラス演算子`+`(対応するメソッド: `op_UnaryPlus`)

* 単項マイナス演算子`-`(対応するメソッド: `op_UnaryNegation`)

* 論理`Not`演算子 (対応するメソッド: `op_OnesComplement`)

* `IsTrue`と`IsFalse`演算子 (対応するメソッド: `op_True`、 `op_False`)

すべてのオーバー ロードされた単項演算子は、包含する型の 1 つのパラメーターを受け取る必要があり、を除き、任意の型を返す可能性があります`IsTrue`と`IsFalse`を返す必要があります`Boolean`します。 含んでいる型がジェネリック型の場合は、型パラメーターは、含む型の型パラメーターと一致する必要があります。 たとえば、オブジェクトに適用された

```vb
Structure Complex
    ...

    Public Shared Operator +(v As Complex) As Complex
        Return v
    End Operator
End Structure
```

型のオーバー ロードのいずれかの場合`IsTrue`または`IsFalse`、その他の同様にオーバー ロードする必要があります。 1 つはオーバー ロードだけの場合、コンパイル時エラーになります。

__注意してください。__ `IsTrue` `IsFalse`予約された単語は使用されません。

### <a name="binary-operators"></a>二項演算子

次の二項演算子をオーバー ロードできます。

* 追加`+`、減算`-`、乗算`*`、除算`/`、整数の除算`\`、剰余`Mod`と指数演算`^`演算子 (対応するメソッド。`op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_IntegerDivision`, `op_Modulus`, `op_Exponent`)

* 関係演算子`=`、 `<>`、 `<`、 `>`、 `<=`、 `>=` (対応するメソッド: `op_Equality`、 `op_Inequality`、 `op_LessThan`、 `op_GreaterThan`、 `op_LessThanOrEqual`, `op_GreaterThanOrEqual`). __注意してください。__ 等値演算子をオーバー ロードできる、中には、(代入ステートメントでのみ使用)、代入演算子はオーバー ロードできません。

* `Like`演算子 (対応するメソッド: `op_Like`)

* 連結演算子`&`(対応するメソッド: `op_Concatenate`)

* 論理`And`、`Or`と`Xor`演算子 (対応するメソッド: `op_BitwiseAnd`、 `op_BitwiseOr`、 `op_ExclusiveOr`)

* シフト演算子`<<`と`>>`(対応するメソッド: `op_LeftShift`、 `op_RightShift`)

すべてのオーバー ロードされた二項演算子は、パラメーターの 1 つとして含んでいる型を取得する必要があります。 含んでいる型がジェネリック型の場合は、型パラメーターは、含む型の型パラメーターと一致する必要があります。 シフト演算子をさらに制限を包含する型の最初のパラメーターを要求するには、この規則型の 2 番目のパラメーターが必ず`Integer`します。

次の二項演算子は、ペアで宣言する必要があります。

* 演算子`=`と演算子 `<>`

* 演算子`>`と演算子 `<`

* 演算子`>=`と演算子 `<=`

ペアの 1 つが宣言されている場合、この属性はパラメーターと戻り値の型が一致する、もう一方も宣言する必要があります。 またはコンパイル時エラーが発生します。 (__に注意してください。__ 関係演算子のペアの宣言を要求する場合の目的は、少なくとも最小レベルのオーバー ロードされた演算子で論理的な一貫性を確保する)。

関係演算子とは異なり、除算と整数の除算演算子をオーバー ロードはお勧めがエラーではありません。 (__に注意してください。__ 一般に、除算の 2 種類が完全に別にする必要があります。 除算をサポートする型が整数 (後者をサポートすること`\`) かどうか (後者をサポートすること`/`)。 両方の演算子を定義するとエラーを行うことを検討しましたが、実習を許可するが、厳密にこれを防ぐために最も安全な方法だと考えたため、言語は、一般的には 2 種類の方法は Visual Basic では、除算を区別しません、)。

複合代入演算子は直接オーバー ロードすることはできません。 代わりに、対応する二項演算子をオーバー ロードするときに、複合代入演算子はオーバー ロードされた演算子を使用します。 例えば:

```vb
Structure Complex
    ...

    Public Shared Operator +(x As Complex, y As Complex) _
        As Complex
        ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim c1, c2 As Complex
        ' Calls the overloaded + operator
        c1 += c2
    End Sub
End Module
```

### <a name="conversion-operators"></a>変換演算子

変換演算子は、型の間で新しい変換を定義します。 これらの新しい変換と呼びます*ユーザー定義の変換*します。 変換演算子は、戻り値の型変換演算子によって示される、ターゲットの型に、変換演算子のパラメーターの型によって示される、ソースの種類からに変換します。 変換は、拡大または縮小のいずれかに分類する必要があります。 変換演算子の宣言を含む、`Widening`キーワードにはユーザー定義の拡大変換が導入されています (対応するメソッド: `op_Implicit`)。 変換演算子の宣言を含む、`Narrowing`キーワードには、ユーザー定義の縮小変換が導入されています (対応するメソッド: `op_Explicit`)。

一般に、例外をスローしないと、情報が失われることをユーザー定義の拡大変換を設計する必要があります。 (たとえば、元の引数が範囲外です) ために、ユーザー定義の変換の例外が発生する場合はその変換 (上位ビットが破棄) などの情報の損失は縮小変換として定義する必要があります。 次に例を示します。

```vb
Structure Digit
    Dim value As Byte

    Public Sub New(value As Byte)
        if value < 0 OrElse value > 9 Then Throw New ArgumentException()
        Me.value = value
    End Sub

    Public Shared Widening Operator CType(d As Digit) As Byte
        Return d.value
    End Operator

    Public Shared Narrowing Operator CType(b As Byte) As Digit
        Return New Digit(b)
    End Operator
End Structure
```

変換`Digit`に`Byte`拡大変換は、決してに例外がスローまたはについてがからの変換を失ったため`Byte`に`Digit`から縮小変換は、`Digit`のみ表すことができます、可能な値のサブセットを`Byte`します。

すべての他の型のメンバー オーバー ロードできるとは異なりは、変換演算子のシグネチャには、変換のターゲット型が含まれています。 これは、対象の戻り値の型は、署名に参加しているだけの型のメンバーです。 拡大または縮小、変換演算子の分類、演算子のシグネチャの一部はできませんただし、します。 したがって、クラスまたは構造体は拡大変換演算子と同じソースとターゲット型の縮小変換演算子の両方に宣言できません。

ユーザー定義変換演算子にするか、それを含む型から変換する必要があります--、たとえば、クラスのことは`C`から変換を定義する`C`に`Integer`との間`Integer`に`C`からはできません`Integer`に`Boolean`します。 含んでいる型がジェネリック型の場合は、型パラメーターは、含む型の型パラメーターと一致する必要があります。 また、組み込みの (つまり非ユーザー定義) の変換を再定義することはできません。 その結果、型が変換を宣言できません場所。

* ソースの種類と変換先の型は、同じです。

* ソースの種類と変換先の型の両方が変換演算子を定義する型ではありません。

* ソースの種類または変換先の型がインターフェイス型です。

* ソースの種類と変換先の型が継承によって関連付けられる (を含む`Object`)。

これらの規則の唯一の例外は、null 許容値型に適用されます。 Null 許容値型は、実際の型定義を持っていないために、値型は、null 許容型のバージョンについては、ユーザー定義の変換を宣言できます。 型が特定のユーザー定義変換を宣言するかどうかを決定するときに、`?`修飾子がからすべての宣言に含まれる型の有効性チェックの目的で最初に削除されます。 したがって、次の宣言は有効なため、`S`からの変換を定義できます`S`に`T`:

```vb
Structure T
    ...
End Structure

Structure S
    Public Shared Widening Operator CType(ByVal v As S?) As T
    ...
    End Operator
End Structure
```

次の宣言が無効です、ただし、構造体`S`からの変換を定義することはできません`S`に`S`:

```vb
Structure S
    Public Shared Widening Operator CType(ByVal v As S) As S?
        ...
    End Operator
End Structure
```

### <a name="operator-mapping"></a>演算子のマッピング

Visual Basic をサポートする演算子のセットは、演算子のセットが .NET Framework の他の言語を一致も一致しない可能性があります、ため、いくつかの演算子が定義されている、または使用する場合は、その他の演算子に特別にマップされます。 具体的には、次のように使用します。

* 整数除算演算子を定義すると、通常の除算演算子 (その他の言語からのみ使用可能) 整数の除算演算子を呼び出すことは自動的に定義します。

* オーバー ロード、 `Not`、 `And`、および`Or`演算子が論理/ビット処理演算子の区別を他の言語の観点からのビットごとの演算子をオーバー ロードします。

* 論理/ビット処理演算子を識別するための言語でのみ論理演算子をオーバー ロードするクラス (を使用する言語つまり`op_LogicalNot`、 `op_LogicalAnd`、および`op_LogicalOr`の`Not`、 `And`、および`Or`、それぞれ)、論理演算子を Visual Basic の論理演算子にマップする必要があります。 論理/ビット処理演算子の両方がオーバー ロードする場合は、ビットごとの演算子のみが使用されます。

* オーバー ロード、`<<`と`>>`演算子が符号付きと符号なしのシフト演算子の区別を他の言語の観点から符号付きの演算子のみをオーバー ロードします。

* 符号なしのシフト演算子のみをオーバー ロードするクラスには、対応する Visual Basic のシフト演算子にマップされる符号なしのシフト演算子があります。 両方を符号なしと符号付きのシフト演算子はオーバー ロードする場合は、署名された shift 演算子のみが使用されます。
