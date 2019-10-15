---
ms.openlocfilehash: 3d5c1e90283b6d6ec8cdeccd35e32c78f997cc27
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306041"
---
# <a name="type-members"></a>型のメンバー

型のメンバーは、ストレージの場所と実行可能コードを定義します。 メソッド、コンストラクター、イベント、定数、変数、およびプロパティを指定できます。

## <a name="interface-method-implementation"></a>インターフェイスメソッドの実装

メソッド、イベント、およびプロパティは、インターフェイスメンバーを実装できます。 インターフェイスメンバーを実装するには、メンバー宣言によって @no__t 0 キーワードが指定され、1つ以上のインターフェイスメンバーが一覧表示されます。

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

インターフェイスメンバーを実装するメソッドとプロパティは、`MustOverride`、`Overridable`、または別のメンバーをオーバーライドするように宣言されている場合を除き、暗黙的に @no__t 0 になります。 インターフェイスメンバーを実装するメンバーが @no__t 0 になると、エラーになります。 メンバーのアクセシビリティは、インターフェイスメンバーを実装する機能には影響しません。

インターフェイスの実装を有効にするには、含んでいる型の実装リストが、互換性のあるメンバーを含むインターフェイスに名前を指定する必要があります。 互換性のあるメンバーとは、実装するメンバーのシグネチャと一致するシグネチャを持つメンバーのことです。 ジェネリックインターフェイスが実装されている場合は、Implements 句に指定された型引数が、互換性をチェックするときにシグネチャに置き換えられます。 以下に例を示します。

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

デリゲート型を使用して宣言されたイベントがインターフェイスイベントを実装している場合、互換性のあるイベントとは、基になるデリゲート型が同じ型であるイベントのことです。 それ以外の場合、イベントは、実装しているインターフェイスイベントのデリゲート型を使用します。 このようなイベントに複数のインターフェイスイベントが実装されている場合、すべてのインターフェイスイベントの基になるデリゲート型が同じである必要があります。 以下に例を示します。

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

Implements リストのインターフェイスメンバーは、型名、ピリオド、および識別子を使用して指定されます。 型名は、implements リスト内のインターフェイス、または implements リスト内のインターフェイスの基本インターフェイスである必要があります。また、識別子は、指定されたインターフェイスのメンバーである必要があります。 1つのメンバーは、複数の一致するインターフェイスメンバーを実装できます。

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

実装するインターフェイスメンバーが、複数のインターフェイスの継承によって明示的に実装されたすべてのインターフェイスで使用できない場合は、そのメンバーが使用可能な基本インターフェイスを実装するメンバーが明示的に参照する必要があります。 たとえば、`I1` および `I2` にメンバー `M` が含まれており、`I3` が `I1` および `I2` から継承されている場合、`I3` を実装する型は `I1.M` と `I2.M` を実装します。 インターフェイスのシャドウが継承されたメンバーを乗算する場合、実装する型は、継承されたメンバーとそのメンバーをシャドウするメンバーを実装する必要があります。

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

インターフェイスメンバーのコンテナーインターフェイスを実装する場合は、実装するインターフェイスと同じ型引数を指定する必要があります。 以下に例を示します。

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

メソッドには、プログラムの実行可能なステートメントが含まれています。

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

省略可能なパラメーターのリストと戻り値 (省略可能) を持つメソッドは、共有されているか、共有されていません。 共有メソッドには、クラスまたはクラスのインスタンスを使用してアクセスします。 非共有メソッド (インスタンスメソッドとも呼ばれます) には、クラスのインスタンスを介してアクセスします。 次の例は、複数の共有メソッド (`Clone` と `Flip`) を持ち、いくつかのインスタンスメソッド (`Push`、`Pop`、および `ToString`) を持つ @no__t 0 のクラスを示しています。

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

メソッドはオーバーロードすることができます。つまり、複数のメソッドが一意のシグネチャを持つ限り、同じ名前を持つことができます。 メソッドのシグネチャは、パラメーターの数と型で構成されます。 メソッドのシグネチャには、戻り値の型やパラメーター修飾子 (省略可能、ByRef、ParamArray など) は含まれません。 次の例は、多数のオーバーロードを持つクラスを示しています。

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

プログラムの出力は次のようになります。

```console
F()
F(Integer)
F(Object)
F(Integer, Integer)
F(Integer())
G(String)
G(String, Optional String)
```

省略可能なパラメーターのみが異なるオーバーロードは、ライブラリの "バージョン管理" に使用できます。 たとえば、ライブラリの v1 には、省略可能なパラメーターを持つ関数が含まれる場合があります。

```vb
Sub fopen(fileName As String, Optional accessMode as Integer = 0)
```

次に、ライブラリの v2 は、もう1つの省略可能なパラメーター "password" を追加する必要があります。この場合、ソースの互換性を損なうことなく (つまり、v1 をターゲットとするアプリケーションを再コンパイルできます)、バイナリ互換性を損なうことなく、参照 v1 は、再コンパイルなしで v2 を参照できるようになりました)。 V2 は次のようになります。

```vb
Sub fopen(file As String, mode as Integer)
Sub fopen(file As String, Optional mode as Integer = 0, Optional pword As String = "")
```

パブリック API の省略可能なパラメーターは CLS に準拠していないことに注意してください。 ただし、少なくとも Visual Basic と C# 4 およびF#で使用できます。



### <a name="regular-async-and-iterator-method-declarations"></a>標準、非同期、および反復子メソッドの宣言

2種類のメソッドがあります。値を返さない*サブルーチン*と、関数を実行する*関数*です。 メソッドがインターフェイスで定義されているか、または @no__t 修飾子が指定されている場合にのみ、メソッドの本体および `End` コンストラクトを省略できます。 関数に戻り値の型が指定されておらず、厳密なセマンティクスが使用されている場合、コンパイル時エラーが発生します。それ以外の場合、型は暗黙的に `Object` またはメソッドの型文字の型になります。 メソッドの戻り値の型とパラメーターの型のアクセシビリティドメインは、またはメソッド自体のアクセシビリティドメインのスーパーセットと同じである必要があります。

__通常のメソッド__は、`Async` でも @no__t でもありません。 これは、サブルーチンまたは関数である可能性があります。 セクションの[通常のメソッド](statements.md#regular-methods)では、通常のメソッドが呼び出されたときの動作について詳しく説明します。

__Iterator メソッド__は、`Iterator` 修飾子を持ち、`Async` 修飾子はありません。 これは関数である必要があり、戻り値の型は `IEnumerator`、`IEnumerable`、または `IEnumerator(Of T)` または `T` のいずれかである必要があります。また、@no__t 5 のパラメーターを指定することはできません。 「[反復子メソッド](statements.md#iterator-methods)」セクションでは、反復子メソッドが呼び出されたときの動作について詳しく説明します。

__非同期メソッド__は、`Async` 修飾子を持ち、`Iterator` 修飾子はありません。 これは、サブルーチンであるか、または戻り値の型を持つ関数 `Task` であるか、または `T` の `Task(Of T)` である必要があり、@no__t 3 個のパラメーターを持つことはできません。 「[非同期メソッド](statements.md#async-methods)」セクションでは、非同期メソッドが呼び出されたときの動作について詳しく説明します。

メソッドがこれら3種類のメソッドのいずれでもない場合、コンパイル時エラーになります。

サブルーチンと関数の宣言は、先頭と末尾のステートメントが論理行の先頭から開始する必要があることに特に意味があります。 また、@no__t 0 以外のサブルーチンまたは関数宣言の本体は、論理行の先頭から始める必要があります。 以下に例を示します。

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

外部メソッドの宣言には、プログラムの外部で実装が提供される新しいメソッドが導入されています。

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

外部メソッドの宣言には実際の実装がないため、メソッド本体または @no__t 0 のコンストラクトはありません。 外部メソッドは暗黙的に共有され、型パラメーターを持つことはできません。また、イベントを処理したり、インターフェイスメンバーを実装したりすることはできません。 関数に戻り値の型が指定されておらず、厳密なセマンティクスが使用されている場合、コンパイル時エラーが発生します。 それ以外の場合、型は暗黙的に `Object` またはメソッドの型文字の型になります。 外部メソッドの戻り値の型とパラメーターの型のアクセシビリティドメインは、外部メソッド自体のアクセシビリティドメインのスーパーセットと同じである必要があります。

外部メソッドの宣言の library 句は、メソッドを実装する外部ファイルの名前を指定します。 省略可能な alias 句は、数値の序数 (プレフィックスの @no__t 0 文字) または外部ファイル内のメソッドの名前を指定する文字列です。 1文字セット修飾子を指定することもできます。これは、外部メソッドの呼び出し中に文字列をマーシャリングするために使用される文字セットを制御します。 @No__t-0 修飾子は、すべての文字列を Unicode 値にマーシャリングし、`Ansi` 修飾子はすべての文字列を ANSI 値にマーシャリングします。また、`Auto` 修飾子は、メソッドの名前に基づいて .NET Framework 規則に従って文字列をマーシャリングします。または、指定されている場合はエイリアス名をマーシャリングします。 修飾子が指定されていない場合、既定値は `Ansi` になります。

@No__t-0 または `Unicode` が指定されている場合、メソッド名は外部ファイルでは変更されずに検索されます。 @No__t-0 が指定されている場合、メソッド名の参照はプラットフォームに依存します。 プラットフォームが ANSI (たとえば、Windows 95、Windows 98、Windows ME) と見なされた場合、メソッド名は変更されずに検索されます。 参照が失敗した場合は、`A` が追加され、参照が再試行されます。 プラットフォームが Unicode (Windows NT、Windows 2000、Windows XP など) と見なされた場合は、`W` が追加され、名前が検索されます。 参照が失敗した場合は、`W` なしで参照が再試行されます。 以下に例を示します。

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

外部メソッドに渡されるデータ型は、.NET Framework のデータマーシャリング規則に従って、1つの例外でマーシャリングされます。 値によって渡される文字列変数 (つまり、@no__t 0) は OLE オートメーション BSTR 型にマーシャリングされ、外部メソッドで BSTR に加えられた変更は文字列引数に戻されます。 これは、外部メソッドの型 `String` が変更可能であるためです。この特別なマーシャリングは、その動作を模倣しています。 参照渡しで渡される文字列パラメーター (つまり @no__t 0) は、OLE オートメーション BSTR 型へのポインターとしてマーシャリングされます。 パラメーターに `System.Runtime.InteropServices.MarshalAsAttribute` 属性を指定することで、これらの特殊な動作をオーバーライドすることができます。

外部メソッドの使用例を次に示します。

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

@No__t 0 修飾子は、メソッドがオーバーライド可能であることを示します。 @No__t 0 修飾子は、メソッドが、同じシグネチャを持つ基本型のオーバーライド可能なメソッドをオーバーライドすることを示します。 @No__t 0 修飾子は、オーバーライド可能なメソッドをさらにオーバーライドできないことを示します。 @No__t 0 修飾子は、派生クラスでメソッドをオーバーライドする必要があることを示します。

これらの修飾子の特定の組み合わせは無効です。

* `Overridable` および `NotOverridable` は相互に排他的であるため、組み合わせることはできません。

* `MustOverride` は `Overridable` を意味します (そのため、指定できないため)。 `NotOverridable` と組み合わせることはできません。

* `NotOverridable` は、`Overridable` または `MustOverride` と組み合わせることはできません。また、`Overrides` と組み合わせる必要があります。

* `Overrides` は `Overridable` を意味します (そのため、指定できないため)。 `MustOverride` と組み合わせることはできません。

また、オーバーライド可能なメソッドにも追加の制限があります。

* @No__t 0 のメソッドは、メソッド本体または `End` コンストラクトを含むことはできません。また、別のメソッドをオーバーライドすることはできません。また、`MustInherit` クラスでのみ表示される可能性があります。

* メソッドで @no__t 0 が指定されていて、オーバーライドする対応する基本メソッドが存在しない場合、コンパイル時エラーが発生します。 オーバーライドするメソッドで `Shadows` を指定することはできません。

* オーバーライドするメソッドのアクセシビリティドメインがオーバーライドされるメソッドのアクセシビリティドメインと等しくない場合、メソッドは別のメソッドをオーバーライドすることはできません。 1つの例外として、`Friend` のアクセス権を持たない別のアセンブリ内の @no__t 0 メソッドをオーバーライドするメソッドは `Protected` (`Protected Friend` ではなく) を指定する必要があります。

* `Private` のメソッドを `Overridable`、`NotOverridable`、または `MustOverride` にすることはできません。また、他のメソッドをオーバーライドすることもできません。

* @No__t 0 クラスのメソッドは、-1 または `MustOverride` @no__t 宣言できません。

次の例は、overridable メソッドと overridable 以外のメソッドの違いを示しています。

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

この例では、クラス `Base` によって、メソッド `F`、@no__t 2 のメソッド `G` が導入されています。 クラス `Derived` では、新しいメソッド `F` が導入されており、継承された `F` をシャドウしています。また、継承されたメソッド `G` もオーバーライドします。 この例では次の出力が生成されます。

```console
Base.F
Derived.F
Derived.G
Derived.G
```

ステートメント `b.G()` は、`Base.G` ではなく `Derived.G` を呼び出します。 これは、インスタンス (`Base`) のコンパイル時の型ではなく、インスタンスのランタイム型 (`Derived`) が、呼び出す実際のメソッドの実装を決定するためです。

### <a name="shared-methods"></a>共有メソッド

@No__t-0 修飾子は、メソッドが*共有メソッド*であることを示します。 共有メソッドは、型の特定のインスタンスでは動作せず、型の特定のインスタンスを使用するのではなく、型から直接呼び出すことができます。 ただし、インスタンスを使用して共有メソッドを修飾することは有効です。 共有メソッドで `Me`、`MyClass`、または `MyBase` を参照することはできません。 共有メソッドは、@no__t 0、`NotOverridable`、`MustOverride` のいずれでもない可能性があり、メソッドをオーバーライドすることはできません。 標準モジュールおよびインターフェイスで定義されているメソッドは、暗黙的に `Shared` であるため `Shared` を指定することはできません。

構造体またはクラスで @no__t 0 修飾子のないメソッドが宣言されていますが、*インスタンスメソッド*です。 インスタンスメソッドは、型の特定のインスタンスに対して動作します。 インスタンスメソッドは、型のインスタンスを介してのみ呼び出すことができ、`Me` 式を通じてインスタンスを参照することができます。

次の例は、共有メンバーとインスタンスメンバーにアクセスするための規則を示しています。

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

メソッド @no__t 0 は、インスタンス関数メンバーで、インスタンスメンバーと共有メンバーの両方にアクセスするために識別子を使用できることを示しています。 メソッド `G` は、共有関数メンバーで、識別子を使用してインスタンスメンバーにアクセスするときにエラーになることを示します。 メソッド @no__t 0 は、メンバーアクセス式でインスタンスメンバーにはインスタンスを介してアクセスする必要があることを示していますが、共有メンバーは、型またはインスタンスを使用してアクセスできます。

### <a name="method-parameters"></a>メソッドのパラメーター

*パラメーター*は、メソッドとの間で情報を渡すために使用できる変数です。 メソッドのパラメーターは、メソッドのパラメーターリストによって宣言されます。これは、コンマで区切られた1つ以上のパラメーターで構成されます。

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

パラメーターに型が指定されておらず、厳密なセマンティクスが使用されている場合は、コンパイル時エラーが発生します。 それ以外の場合、既定の型は @no__t 0 またはパラメーターの型文字の型になります。 寛容なセマンティクスでも、1つのパラメーターに `As` 句が含まれている場合、すべてのパラメーターで型を指定する必要があります。

パラメーターは、値、参照、省略可能、または paramarray パラメーターとして、`ByVal`、`ByRef`、`Optional`、および `ParamArray` の修飾子で指定されます。 @No__t-0 または `ByVal` が指定されていないパラメーターは、既定で `ByVal` に設定されます。

パラメーター名は、メソッドの本体全体にスコープが設定され、常にパブリックにアクセスできます。 メソッドの呼び出しでは、その呼び出しに固有のコピーがパラメーターによって作成され、呼び出しの引数リストによって、新しく作成されたパラメーターに値または変数参照が割り当てられます。 外部メソッドの宣言とデリゲート宣言には本体がないため、パラメーターリストで重複するパラメーター名を使用できますが、これは推奨されません。

識別子の後に null 許容の名前修飾子を指定すると、null 値が許容されることを示す場合は 0 @no__t、配列であることを示す場合は配列の名前修飾子によって識別されます。 たとえば、"`ByVal x?() As Integer`" のように組み合わせることができます。 明示的な配列の境界を使用することはできません。また、nullable name 修飾子が指定されている場合は、`As` 句が存在する必要があります。


#### <a name="value-parameters"></a>値パラメーター

*値パラメーター*は、明示的な `ByVal` 修飾子を使用して宣言されています。 @No__t-0 修飾子が使用されている場合、`ByRef` 修飾子を指定することはできません。 値パラメーターは、パラメーターが属しているメンバーの呼び出しと共に存在し、呼び出しで指定された引数の値で初期化されます。 値パラメーターは、メンバーが返されたときに存在しなくなります。

メソッドは、値パラメーターに新しい値を割り当てることができます。 このような割り当ては、値パラメーターによって表されるローカルストレージの場所にのみ影響します。メソッドの呼び出しで指定された実際の引数には影響しません。

値パラメーターは、引数の値がメソッドに渡されるときに使用されます。パラメーターを変更しても、元の引数には影響しません。 値パラメーターは、対応する引数の変数とは別の変数である、独自の変数を参照します。 この変数は、対応する引数の値をコピーすることによって初期化されます。 次の例は、`p` という名前の値パラメーターを持つメソッド `F` を示しています。

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

この例では、値パラメーター `p` が変更されていても、次の出力が生成されます。

```console
pre: a = 1
p = 1
post: a = 1
```

#### <a name="reference-parameters"></a>参照パラメーター

参照パラメーターは、@no__t 0 修飾子で宣言されたパラメーターです。 @No__t-0 修飾子が指定されている場合、`ByVal` 修飾子は使用できません。 参照パラメーターでは、新しいストレージの場所は作成されません。 代わりに、参照パラメーターは、メソッドまたはコンストラクターの呼び出しで引数として指定された変数を表します。 概念的には、参照パラメーターの値は、基になる変数と常に同じです。

参照パラメーターは、*エイリアス*として、または*コピーインコピーバック*を使用して、2つのモードで動作します。

__エイリアス.__ 参照パラメーターは、パラメーターが、呼び出し元が指定した引数のエイリアスとして機能する場合に使用されます。 参照パラメーターは、それ自体は変数を定義するのではなく、対応する引数の変数を参照します。 参照パラメーターを直接変更し、対応する引数に直ちに影響を与えます。 次の例は、2つの参照パラメーターを持つメソッド `Swap` を示しています。

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

プログラムの出力は次のようになります。

```console
pre: x = 1, y = 2
post: x = 2, y = 1
```

クラス `Main` のメソッド `Swap` を呼び出す場合、`a` は `x,` を表し、@no__t は `y` を表します。 このため、呼び出しは `x` と `y` の値を交換した場合の効果があります。

参照パラメーターを受け取るメソッドでは、複数の名前が同じストレージの場所を表すことができます。

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

この例では、`G` のメソッド `F` は、`a` と `b` の両方に対して `s` への参照を渡します。 このため、この呼び出しでは、`s`、`a`、`b` の名前はすべて同じストレージの場所を参照し、3つの割り当てはすべてインスタンス変数 `s` を変更します。

__コピーインコピーバック。__ 参照パラメーターに渡される変数の型が参照パラメーターの型と互換性がない場合、または非変数 (プロパティなど) が参照パラメーターに引数として渡された場合、または呼び出しが遅延バインディングの場合は、一時的な変数が割り当てられ、参照パラメーターに渡されます。 渡された値は、メソッドが呼び出される前にこの一時変数にコピーされ、メソッドから制御が戻ったときに、元の変数 (存在する場合は、書き込み可能な場合) にコピーされます。 このため、参照パラメーターには、渡される変数の正確な格納場所への参照が含まれるとは限りません。また、メソッドが終了するまで、参照パラメーターに対する変更は変数に反映されない可能性があります。 以下に例を示します。

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

@No__t-0 の最初の呼び出しの場合は、一時変数が作成され、プロパティ `G` の値が割り当てられ `F` に渡されます。 @No__t-0 から戻ると、一時変数の値が `G` のプロパティに割り当てられます。 2番目のケースでは、別の一時変数が作成され、`d` の値が割り当てられ `F` に渡されます。 @No__t-0 から戻ると、一時変数の値が変数の型にキャストされ、`Derived` に戻り、`d` に割り当てられます。 返される値は `Derived` にキャストできないため、実行時に例外がスローされます。

#### <a name="optional-parameters"></a>省略可能なパラメーター

省略可能なパラメーターが @no__t 0 修飾子で宣言されています。 仮パラメーターリスト内の省略可能なパラメーターの後に続くパラメーターは、省略可能である必要があります。次のパラメーターに `Optional` 修飾子を指定しないと、コンパイル時エラーが発生します。 Null 許容型 `T?` または null 非許容 @no__t 型の省略可能なパラメーターです。引数が指定されていない場合は、既定値として使用する定数式 `e` を指定する必要があります。 @No__t-0 が Object 型の `Nothing` と評価された場合、パラメーターの*型*の既定値がパラメーターの既定値として使用されます。 それ以外の場合、`CType(e, T)` は定数式である必要があり、パラメーターの既定値として取得されます。

パラメーターの初期化子が有効である唯一の状況は、省略可能なパラメーターです。 初期化は常に、メソッド本体内ではなく、呼び出し式の一部として実行されます。

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

プログラムの出力は次のようになります。

```console
x = 10, y = 20
x = 30, y = 40
```

省略可能なパラメーターは、デリゲートまたはイベント宣言では指定できません。また、ラムダ式でも指定できません。

#### <a name="paramarray-parameters"></a>ParamArray パラメーター

`ParamArray` パラメーターは、`ParamArray` 修飾子を使用して宣言されます。 @No__t-0 修飾子が存在する場合は、`ByVal` 修飾子を指定する必要があり、他のパラメーターでは `ParamArray` 修飾子を使用できません。 @No__t-0 パラメーターの型は1次元配列でなければなりません。また、パラメーターリストの最後のパラメーターである必要があります。

@No__t-0 パラメーターは、`ParamArray` の型のパラメーターの不確定数を表します。 メソッド自体では、@no__t 0 のパラメーターは宣言された型として扱われ、特別なセマンティクスはありません。 @No__t-0 パラメーターは暗黙的に省略可能であり、@no__t の型の空の1次元配列の既定値が指定されています。

@No__t 0 は、メソッドの呼び出しで次の2つの方法のいずれかで引数を指定できます。

* @No__t-0 に指定された引数は、`ParamArray` 型に拡大変換される型の1つの式にすることができます。 この場合、@no__t 0 は、値パラメーターとまったく同じように動作します。

* また、呼び出しでは、0個以上の引数を `ParamArray` に指定できます。各引数は、@no__t の要素型に暗黙的に変換できる型の式です。 この場合、呼び出しは、引数の数に対応する長さを持つ @no__t 0 型のインスタンスを作成し、指定された引数値を使用して配列インスタンスの要素を初期化し、新しく作成された配列インスタンスを実際のとして使用します。引数.

呼び出しで可変個の引数を許可する場合を除き、次の例に示すように、@no__t 0 は同じ型の値パラメーターとまったく同じ意味を持ちます。

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

この例では、出力が生成されます。

```console
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

@No__t-0 の最初の呼び出しでは、配列 @no__t 値パラメーターとして1つだけ渡されます。 @No__t-0 の2回目の呼び出しでは、指定された要素値を持つ4要素配列が自動的に作成され、その配列インスタンスが値パラメーターとして渡されます。 同様に、`F` の3回目の呼び出しでは、ゼロ要素の配列を作成し、そのインスタンスを値パラメーターとして渡します。 2番目と3番目の呼び出しは、書き込みとまったく同じです。

```vb
F(New Integer() {10, 20, 30, 40})
F(New Integer() {})
```

`ParamArray` パラメーターをデリゲートまたはイベント宣言で指定することはできません。

### <a name="event-handling"></a>イベント処理

メソッドは、インスタンスまたは共有変数のオブジェクトによって生成されたイベントを宣言によって処理できます。 イベントを処理するために、メソッドの宣言は @no__t 0 のキーワードを指定し、1つ以上のイベントを一覧表示します。

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

@No__t-0 リストのイベントは、ピリオドで区切られた2つの識別子によって指定されます。

* 最初の識別子は、@no__t 0 修飾子、または `MyBase` または `MyClass` または `Me` キーワードを指定する、含んでいる型のインスタンスまたは共有変数である必要があります。それ以外の場合は、コンパイル時エラーが発生します。 この変数には、このメソッドによって処理されるイベントを発生させるオブジェクトが格納されます。

* 2番目の識別子では、最初の識別子の型のメンバーを指定する必要があります。 メンバーはイベントである必要があり、共有されている可能性があります。 最初の識別子に共有変数が指定されている場合は、イベントを共有するか、エラーの結果を生成する必要があります。

ハンドラーメソッド `M` は、イベントの有効なイベントハンドラーと見なされます。-2 のステートメント @no__t も有効である場合は-1 @no__t ます。 ただし、@no__t 0 のステートメントとは異なり、明示的なイベントハンドラーを使用すると、厳密なセマンティクスが使用されているかどうかに関係なく、引数のないメソッドでイベントを処理できます。

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

1つのメンバーが複数の一致するイベントを処理でき、複数のメソッドが1つのイベントを処理する場合があります。 メソッドのアクセシビリティは、イベントを処理する機能には影響しません。 次の例は、メソッドがイベントを処理する方法を示しています。

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

次のように出力されます。

```console
Raised
Raised
```

型は、その基本型によって提供されるすべてのイベントハンドラーを継承します。 派生型は、その基本型から継承するイベントマッピングを変更することはできませんが、イベントにハンドラーを追加することができます。


### <a name="extension-methods"></a>拡張メソッド

メソッドは、*拡張メソッド*を使用して、型宣言の外部から型に追加できます。 拡張メソッドは、@no__t 0 属性が適用されたメソッドです。 これらは標準モジュールでのみ宣言でき、メソッドが拡張する型を指定するパラメーターが少なくとも1つ必要です。 たとえば、次の拡張メソッドは、型 `String` に拡張します。

```vb
Imports System.Runtime.CompilerServices

Module StringExtensions
    <Extension> _
    Sub Print(s As String)
        Console.WriteLine(s)
    End Sub
End Module
```

__付箋.__ Visual Basic では、拡張メソッドを標準モジュール内で宣言する必要がありますC#が、などの他の言語では、他の種類の型で宣言することができます。 ここに記載されている他の規則に従っている限り、含まれている型がオープンジェネリック型ではなく、インスタンス化できない場合、Visual Basic は拡張メソッドを認識します。

拡張メソッドが呼び出されると、呼び出し先のインスタンスが最初のパラメーターに渡されます。 最初のパラメーターを `Optional` または `ParamArray` に宣言することはできません。 型パラメーターを含む任意の型は、拡張メソッドの最初のパラメーターとして表示できます。 たとえば、次のメソッドは、型 `Integer()`、`System.Collections.Generic.IEnumerable(Of T)` を実装するすべての型、およびすべての型を拡張します。

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

前の例で示したように、インターフェイスは拡張できます。 インターフェイス拡張メソッドは、メソッドの実装を提供します。したがって、拡張メソッドが定義されているインターフェイスを実装する型は、インターフェイスによって最初に宣言されたメンバーのみを実装します。 以下に例を示します。

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

拡張メソッドは、型パラメーターに対して型制約を持つこともできます。また、非拡張ジェネリックメソッドと同様に、型引数を推論することもできます。

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

拡張メソッドには、拡張される型内の暗黙的なインスタンス式を使用してアクセスすることもできます。

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

アクセシビリティのために、拡張メソッドは、宣言されている標準モジュールのメンバーとしても扱われます。これらのメソッドは、宣言コンテキストを使用することによって、拡張されているアクセス権を超える型のメンバーに対する追加のアクセス権を持っていません。

拡張メソッドは、標準モジュールメソッドがスコープ内にある場合にのみ使用できます。 そうしないと、元の型は拡張されていないように見えます。 以下に例を示します。

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

型の拡張メソッドだけが使用可能な場合、型を参照しても、コンパイル時エラーが発生します。

拡張メソッドは、厳密に型指定された `For Each` パターンなど、メンバーがバインドされるすべてのコンテキストの型のメンバーであると見なされることに注意してください。 以下に例を示します。

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

拡張メソッドを参照するデリゲートを作成することもできます。 したがって、コードは次のようになります。

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

は、次の場合とほぼ同じです。

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

__付箋.__ Visual Basic は、通常、メソッドが呼び出されているインスタンスが `Nothing` である場合に `System.NullReferenceException` を発生させるインスタンスメソッド呼び出しにチェックを挿入します。 拡張メソッドの場合は、このチェックを挿入するための効率的な方法がないため、拡張メソッドは `Nothing` を明示的にチェックする必要があります。 

__付箋.__ 値型は、インターフェイスとして型指定されたパラメーターに @no__t 0 引数として渡されると、ボックス化されます。  これは、拡張メソッドの副作用が、元のではなく、構造体のコピーに対して動作することを意味します。 この言語では、拡張メソッドの最初の引数に制限はありませんが、拡張メソッドを使用して値型を拡張することはできません。また、値型を拡張する場合は、最初のパラメーターを 0 @no__t 渡して、副作用を保証することをお勧めします。元の値を操作します。

### <a name="partial-methods"></a>部分メソッド

*部分メソッド*は、メソッドの本体ではなく、シグネチャを指定するメソッドです。 メソッドの本体は、同じ名前およびシグネチャを持つ別のメソッド宣言によって指定できます。この宣言は、通常、型の別の部分宣言に含まれています。 以下に例を示します。

.vb:

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

b. vb:

```vb
Public Partial Class MyForm
    Public Sub ValidateControls()
        ' Validation logic goes here
        ...
    End Sub
End Class
```

この例では、クラス `MyForm` の部分宣言は、実装のない部分メソッド `ValidateControls` を宣言します。 ファイルに本文が指定されていない場合でも、部分宣言のコンストラクターは部分メソッドを呼び出します。 @No__t-0 のその他の部分宣言は、メソッドの実装を提供します。

部分メソッドは、本文が指定されているかどうかに関係なく呼び出すことができます。メソッド本体が指定されていない場合、呼び出しは無視されます。 以下に例を示します。

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

無視される部分メソッド呼び出しに引数として渡されるすべての式は無視され、評価されません。 (__注:__ つまり、部分メソッドは、2つの部分型に対して定義されている動作を提供する非常に効率的な方法です。部分メソッドは使用されない場合はコストがないためです)。

部分メソッドの宣言は @no__t 0 として宣言する必要があり、常に本体にステートメントがないサブルーチンである必要があります。 部分メソッドは、それ自体がインターフェイスメソッドを実装することはできませんが、その本体を提供するメソッドは使用できます。

本文を部分メソッドに渡すことができるのは、1つのメソッドだけです。 部分メソッドに本文を指定するメソッドには、部分メソッドと同じシグネチャ、任意の型パラメーターに対する同じ制約、同じ宣言修飾子、および同じパラメーターと型パラメーター名が必要です。 部分メソッドの属性とその本体を提供するメソッドは、メソッドのパラメーターの属性と同様にマージされます。 同様に、メソッドが処理するイベントの一覧がマージされます。 以下に例を示します。

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

*コンストラクター*は、初期化の制御を可能にする特殊なメソッドです。 これらは、プログラムが開始した後、または型のインスタンスが作成されたときに実行されます。 他のメンバーとは異なり、コンストラクターは継承されず、型の宣言空間に名前を導入しません。 コンストラクターは、オブジェクト作成式または .NET Framework によってのみ呼び出すことができます。直接呼び出すことはできません。

__付箋.__ コンストラクターには、サブルーチンが持つ行の配置に対して同じ制限があります。 開始ステートメント、終了ステートメント、およびブロックはすべて論理行の先頭に記述する必要があります。

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

インスタンス*コンストラクター*は、型のインスタンスを初期化し、インスタンスの作成時に .NET Framework によって実行されます。 コンストラクターのパラメーターリストは、メソッドのパラメーターリストと同じ規則に従います。 インスタンスコンストラクターがオーバーロードされている可能性があります。

参照型のすべてのコンストラクターは、別のコンストラクターを呼び出す必要があります。 呼び出しが明示的である場合は、コンストラクターのメソッド本体の最初のステートメントである必要があります。 ステートメントは、型のインスタンスコンストラクターの別の (たとえば、`Me.New(...)` または `MyClass.New(...)`) を呼び出すことができます。または、構造体でない場合は、型の基本型のインスタンスコンストラクター (たとえば、`MyBase.New(...)`) を呼び出すことができます。 コンストラクターがそれ自体を呼び出すことはできません。 コンストラクターが別のコンストラクターの呼び出しを省略した場合、`MyBase.New()` は暗黙的になります。 パラメーターなしの基本型コンストラクターがない場合、コンパイル時エラーが発生します。 @No__t-0 は、基底クラスのコンストラクターへの呼び出しの後に構築されるとは見なされないため、コンストラクター呼び出しステートメントのパラメーターは、暗黙的または明示的に `Me`、`MyClass`、または @no__t を参照できません。

コンストラクターの最初のステートメントが `MyBase.New(...)` の形式である場合、コンストラクターは、型で宣言されたインスタンス変数の変数初期化子によって指定された初期化を暗黙的に実行します。 これは、直接の基本型コンストラクターを呼び出した直後に実行される代入のシーケンスに対応します。 このような順序を使用すると、インスタンスへのアクセス権を持つステートメントが実行される前に、すべての基本インスタンス変数が変数初期化子によって初期化されます。 以下に例を示します。

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

@No__t-0 を使用して `B` のインスタンスを作成すると、次の出力が生成されます。

```console
x = 1, y = 1
```

@No__t-0 の値は、基本クラスのコンストラクターが呼び出された後に変数初期化子が実行されるため、-1 @no__t ます。 変数初期化子は、型宣言に表示される順序に従って実行されます。

型が @no__t のコンストラクターのみを宣言している場合、通常、他の型が型から派生したり、型のインスタンスを作成したりすることはできません。唯一の例外は、型の中で入れ子にされた型です。 @no__t 0 のコンストラクターは、`Shared` のメンバーのみを含む型でよく使用されます。

型にインスタンスコンストラクターの宣言が含まれていない場合は、既定のコンストラクターが自動的に指定されます。 既定のコンストラクターは、直接基本型のパラメーターなしのコンストラクターを呼び出すだけです。 直接の基本型にアクセス可能なパラメーターなしのコンストラクターがない場合、コンパイル時エラーが発生します。 既定のコンストラクターに対して宣言されているアクセス型は、型 @no__t が-1 でない限り、0 @no__t になります。この場合、既定のコンストラクターは `Protected` です。

__付箋.__ @No__t-2 クラスを直接作成することはできないため、`MustInherit` 型の既定のコンストラクターに対する既定のアクセスは-1 @no__t ます。 したがって、既定のコンストラクターを作成しても、@no__t はありません。

次の例では、クラスにコンストラクター宣言が含まれていないため、既定のコンストラクターが用意されています。

```vb
Class Message
    Dim sender As Object
    Dim text As String
End Class
```

したがって、この例は次のように正確に等価です。

```vb
Class Message
    Dim sender As Object
    Dim text As String

    Sub New()
    End Sub
End Class
```

属性でマークされた、デザイナーによって生成されたクラスに生成される既定のコンストラクター `Microsoft.VisualBasic.CompilerServices.DesignerGeneratedAttribute` は、基本コンストラクターの呼び出しの後に、メソッド `Sub InitializeComponent()` (存在する場合) を呼び出します。 (__注:__ これにより、WinForms デザイナーによって作成されたファイルなど、デザイナーで生成されたファイルを使用して、デザイナーファイル内のコンストラクターを省略できます。 これにより、プログラマが選択する場合は、プログラマ自身を指定することができます)。

### <a name="shared-constructors"></a>共有コンストラクター

*共有コンストラクター*は、型の共有変数を初期化します。これらは、プログラムの実行開始後、型のメンバーへの参照の前に実行されます。 共有コンストラクターは @no__t 0 修飾子を指定します。ただし、標準モジュールに含まれていない場合は、`Shared` 修飾子が暗黙的に使用されます。

インスタンスコンストラクターとは異なり、共有コンストラクターは暗黙的なパブリックアクセスを持ち、パラメーターを持たず、他のコンストラクターを呼び出すことができません。 共有コンストラクターの最初のステートメントの前に、共有コンストラクターは、型で宣言された共有変数の変数初期化子によって指定された初期化を暗黙的に実行します。 これは、コンストラクターへの入力時に直ちに実行される割り当てのシーケンスに対応します。 変数初期化子は、型宣言に表示される順序に従って実行されます。

次の例は、共有変数を初期化する共有コンストラクターを持つ @no__t 0 クラスを示しています。

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

閉じられたジェネリック型ごとに個別の共有コンストラクターが存在します。 共有コンストラクターは、閉じられた型ごとに1回だけ実行されるので、制約を介してコンパイル時にチェックできない型パラメーターに対して実行時チェックを適用するのに便利な場所です。 たとえば、次の型では、共有コンストラクターを使用して、型パラメーターが `Integer` または `Double` であることを強制しています。

```vb
Class EnumHolder(Of T)
    Shared Sub New() 
        If Not GetType(T).IsEnum() Then
            Throw New ArgumentException("T must be an enumerated type.")
        End If
    End Sub
End Class
```

共有コンストラクターが実行される場合、厳密には実装に依存しますが、共有コンストラクターが明示的に定義されている場合は、次のような保証が提供されます。

* 共有コンストラクターは、型の静的フィールドに最初にアクセスする前に実行されます。

* 共有コンストラクターは、型の静的メソッドの最初の呼び出しの前に実行されます。

* 共有コンストラクターは、その型のコンストラクターの最初の呼び出しの前に実行されます。

共有コンストラクターが共有初期化子に対して暗黙的に作成される場合、上記の保証は適用されません。 次の例の出力は、読み込みの正確な順序が定義されていないため、不確定です。

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

```console
Init A
A.F
Init B
B.F
```

または

```console
Init B
Init A
A.F
B.F
```

これに対し、次の例では、予測可能な出力が生成されます。 クラス @no__t から派生していても、クラス `A` の @no__t コンストラクターは実行されないことに注意してください。

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

```console
Init B
B.G
```

また、次の例のように、変数初期化子を持つ @no__t 0 の変数を既定値の状態で確認できるようにする、循環依存関係を構築することもできます。

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

これにより、次の出力が生成されます。

```console
X = 1, Y = 2
```

@No__t-0 メソッドを実行するために、システムは最初にクラス `B` を読み込みます。 クラス `B` の @no__t 0 コンストラクターは `Y` の初期値を計算します。これにより、@no__t の値が参照されるため、クラス `A` が再帰的に読み込まれます。 クラス `A` の @no__t 0 のコンストラクターは、`X` の初期値の計算を続行します。これにより、では、*既定*値である `Y` (0) がフェッチされます。 `A.X` は `1` に初期化されます。 @No__t-0 の読み込み処理が完了し、`Y` の初期値の計算に戻ります。結果は `2` になります。

@No__t 0 のメソッドがクラス `A` にありました。この例では、次の出力が生成されています。

```console
X = 2, Y = 1
```

@No__t 0 変数初期化子での循環参照は避けてください。通常、このような参照を含むクラスが読み込まれる順序を決定することはできないためです。

## <a name="events"></a>イベント

イベントは、特定のイベントの発生をコードに通知するために使用されます。 イベント宣言は、デリゲート型またはパラメーターリスト、およびオプションの `Implements` 句のいずれかの識別子で構成されます。

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

デリゲート型が指定されている場合、デリゲート型に戻り値の型を指定することはできません。 パラメーターリストが指定されている場合は、`Optional` または `ParamArray` パラメーターを含めることはできません。 パラメーターの型やデリゲート型のアクセシビリティドメインは、イベント自体のアクセシビリティドメインのスーパーセットと同じであるか、またはそのいずれかである必要があります。 @No__t-0 修飾子を指定することで、イベントを共有できます。

型の宣言空間に追加されたメンバー名に加えて、イベント宣言は、他のいくつかのメンバーを暗黙的に宣言します。 @No__t-0 という名前のイベントを指定すると、次のメンバーが宣言領域に追加されます。

* 宣言の形式がメソッド宣言の場合は、`XEventHandler` という名前の入れ子になったデリゲートクラスが導入されます。 入れ子になったデリゲートクラスは、メソッド宣言と一致し、イベントと同じアクセシビリティを持ちます。 パラメーターリストの属性は、delegate クラスのパラメーターに適用されます。

* @No__t-1 という名前の、デリゲートとして型指定された @no__t 0 インスタンス変数。

* @No__t-0 および `remove_X` という名前の2つのメソッドを呼び出し、オーバーライド、またはオーバーロードすることはできません。

型が上記のいずれかの名前と一致する名前を宣言しようとすると、コンパイル時エラーが発生し、暗黙的な `add_X` および `remove_X` 宣言は名前のバインドのために無視されます。 導入されたメンバーをオーバーライドまたはオーバーロードすることはできませんが、派生型でシャドウすることはできます。 たとえば、クラス宣言は

```vb
Class Raiser
    Public Event Constructed(i As Integer)
End Class
```

は、次の宣言と同じです。

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

デリゲート型を指定せずにイベントを宣言することは、最も単純で最もコンパクトな構文ですが、各イベントに対して新しいデリゲート型を宣言するという欠点があります。 たとえば、次の例では、3つの非表示のデリゲート型が作成されていますが、3つのイベントすべてに同じパラメーターリストがあります。

```vb
Public Class Button
    Public Event Click(sender As Object, e As EventArgs)
    Public Event DoubleClick(sender As Object, e As EventArgs)
    Public Event RightClick(sender As Object, e As EventArgs)
End Class
```

次の例では、イベントが同じデリゲートを使用するだけで、`EventHandler` です。

```vb
Public Delegate Sub EventHandler(sender As Object, e As EventArgs)

Public Class Button
    Public Event Click As EventHandler
    Public Event DoubleClick As EventHandler
    Public Event RightClick As EventHandler
End Class
```

イベントは、静的または動的の2つの方法のいずれかで処理できます。 イベントを静的に処理するのは簡単で、@no__t 0 の変数と `Handles` の句だけが必要です。 次の例では、クラス `Form1` がオブジェクト `Button` のイベント @no__t 静的に処理します。

```vb
Public Class Form1
    Public WithEvents Button1 As New Button()

    Public Sub Button1_Click(sender As Object, e As EventArgs) _
           Handles Button1.Click
        Console.WriteLine("Button1 was clicked!")
    End Sub
End Class
```

イベントは、明示的に接続してコード内に接続解除する必要があるため、動的にイベントを処理することはより複雑になります。 ステートメント `AddHandler` はイベントのハンドラーを追加し、ステートメント `RemoveHandler` はイベントのハンドラーを削除します。 次の例は、`Button1` の `Click` イベントのイベントハンドラーとして `Button1_Click` を追加するクラス `Form1` を示しています。

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

メソッド `Disconnect` の場合、イベントハンドラーは削除されます。


### <a name="custom-events"></a>カスタムイベント

前のセクションで説明したように、イベント宣言では、フィールド、@no__t 0 のメソッド、およびイベントハンドラーを追跡するために使用される @no__t 1 つのメソッドを暗黙的に定義します。 ただし、状況によっては、イベントハンドラーを追跡するためのカスタムコードを提供することが望ましい場合があります。 たとえば、クラスが、少数の処理のみを実行する40イベントを定義する場合、各イベントのハンドラーを追跡するために、40フィールドではなくハッシュテーブルを使用する方が効率的な場合があります。 *カスタムイベント*を使用すると `add_X` メソッドと @no__t メソッドを明示的に定義できます。これにより、イベントハンドラーのカスタムストレージが有効になります。

カスタムイベントは、デリゲート型を指定するイベントが宣言されるのと同じ方法で宣言されます。ただし、キーワード `Custom` は `Event` キーワードの前に記述する必要があります。 カスタムイベント宣言には、@no__t 0 宣言、@no__t 1 宣言、および `RaiseEvent` 宣言の3つの宣言が含まれています。 どの宣言も修飾子を持つことはできませんが、属性を持つことはできます。

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

以下に例を示します。

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

@No__t-0 および `RemoveHandler` 宣言は1つの `ByVal` パラメーターを受け取ります。これは、イベントのデリゲート型である必要があります。 @No__t-0 または `RemoveHandler` ステートメントが実行される (または、`Handles` 句が自動的にイベントを処理する) と、対応する宣言が呼び出されます。 @No__t-0 宣言は、イベントデリゲートと同じパラメーターを受け取り、@no__t 1 ステートメントの実行時に呼び出されます。 すべての宣言を指定する必要があり、サブルーチンと見なされます。

@No__t-0、`RemoveHandler`、および `RaiseEvent` 宣言では、サブルーチンに含まれる行の配置に対して同じ制限が設定されていることに注意してください。 開始ステートメント、終了ステートメント、およびブロックはすべて論理行の先頭に記述する必要があります。

カスタムイベント宣言は、型の宣言空間に追加されたメンバー名に加えて、他のいくつかのメンバーを暗黙的に宣言します。 @No__t-0 という名前のイベントを指定すると、次のメンバーが宣言領域に追加されます。

* @No__t-1 宣言に対応する `add_X` という名前のメソッド。

* @No__t-1 宣言に対応する `remove_X` という名前のメソッド。

* @No__t-1 宣言に対応する `fire_X` という名前のメソッド。

型が上記のいずれかの名前と一致する名前を宣言しようとすると、コンパイル時エラーが発生し、暗黙的な宣言はすべて名前のバインドのために無視されます。 導入されたメンバーをオーバーライドまたはオーバーロードすることはできませんが、派生型でシャドウすることはできます。

__付箋.__ `Custom` は予約語ではありません。

#### <a name="custom-events-in-winrt-assemblies"></a>WinRT アセンブリのカスタムイベント

Microsoft Visual Basic 11.0 の場合、`/target:winmdobj` でコンパイルされたファイルで宣言されたイベント、またはそのようなファイル内のインターフェイスで宣言されたイベントは、多少異なる方法で処理されます。

* Winmd を構築するために使用される外部ツールでは、通常、`System.EventHandler(Of T)` や `System.TypedEventHandle(Of T, U)` などの特定のデリゲート型のみが許可され、他の型は許可されません。

* @No__t-0 フィールドには、型 `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationTokenTable(Of T)` が指定されています。 `T` はデリゲート型です。

* AddHandler アクセサーは @no__t 0 を返し、RemoveHandler アクセサーは同じ型の1つのパラメーターを受け取ります。

このようなカスタムイベントの例を次に示します。

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

*定数*は、型のメンバーである定数値です。

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

定数は暗黙的に共有されます。 宣言に `As` 句が含まれている場合、句は、宣言によって導入されたメンバーの型を指定します。 型を省略した場合、定数の型は推論されます。 定数の型には、プリミティブ型または `Object` のみを指定できます。 定数が `Object` として型指定され、型文字がない場合、定数の実際の型は定数式の型になります。 それ以外の場合、定数の型は、定数の型文字の型になります。

次の例は、2つのパブリック定数を持つ `Constants` という名前のクラスを示しています。

```vb
Class Constants
    Public Const A As Integer = 1
    Public Const B As Integer = A + 1
End Class
```

定数は、次の例のように、クラスを使用してアクセスできます。この例では、`Constants.A` および `Constants.B` の値を出力します。

```vb
Module Test
    Sub Main()
        Console.WriteLine(Constants.A & ", " & Constants.B)
    End Sub 
End Module
```

複数の定数を宣言する定数宣言は、1つの定数の複数の宣言と同じです。 次の例では、1つの宣言ステートメントで3つの定数を宣言しています。

```vb
Class A
    Protected Const x As Integer = 1, y As Long = 2, z As Short = 3
End Class
```

この宣言は、次の場合と同じです。

```vb
Class A
    Protected Const x As Integer = 1
    Protected Const y As Long = 2
    Protected Const z As Short = 3
End Class
```

定数の型のアクセシビリティドメインは、定数自体のアクセシビリティドメインのスーパーセットと同じであるか、またはそのスーパーセットと同じである必要があります。 定数式は、定数の型の値、または定数の型に暗黙的に変換できる型の値を生成する必要があります。 定数式を循環させることはできません。つまり、定数を定義することはできません。

コンパイラは、定数宣言を適切な順序で自動的に評価します。 次の例では、コンパイラは最初に `Y`、次に `Z`、最後に `X` を評価し、それぞれ10、11、および12の値を生成します。

```vb
Class A
    Public Const X As Integer = B.Z + 1
    Public Const Y As Integer = 10
End Class

Class B
    Public Const Z As Integer = A.Y + 1
End Class
```

定数値のシンボル名が必要であるが、定数宣言で値の型が許可されていない場合、またはコンパイル時に定数式で値を計算できない場合は、代わりに読み取り専用の変数を使用できます。


## <a name="instance-and-shared-variables"></a>インスタンスと共有変数

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

修飾子が指定されていない場合は、`Dim` 修飾子を指定する必要がありますが、それ以外の場合は省略できます。 単一の変数宣言には、複数の変数宣言子を含めることができます。各変数宣言子は、新しいインスタンスまたは共有メンバーを導入します。

初期化子が指定されている場合は、変数宣言子で1つのインスタンスまたは共有変数のみを宣言できます。

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

@No__t-0 修飾子を使用して宣言された変数は*共有変数*です。 共有変数は、作成される型のインスタンスの数に関係なく、ストレージの場所を1つだけ識別します。 共有変数は、プログラムの実行開始時に存在し、プログラムの終了時には存在しなくなります。

共有変数は、特定のクローズジェネリック型のインスタンス間でのみ共有されます。 たとえば、次のようなプログラムがあるとします。

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

印刷する:

```console
1
1
2
```

@No__t 0 修飾子を指定せずに宣言された変数は、*インスタンス変数*と呼ばれます。 クラスのすべてのインスタンスには、クラスのすべてのインスタンス変数の個別のコピーが含まれています。 参照型のインスタンス変数は、その型の新しいインスタンスが作成されたときに存在し、そのインスタンスへの参照がなく、`Finalize` メソッドが実行されたときに存在しなくなります。 値型のインスタンス変数は、そのインスタンスが属する変数と正確に同じ有効期間を持ちます。 つまり、値型の変数が存在する場合、または存在しなくなった場合は、値型のインスタンス変数が発生します。

宣言子に `As` 句が含まれている場合、句は、宣言によって導入されたメンバーの型を指定します。 型が省略され、厳密なセマンティクスが使用されている場合、コンパイル時エラーが発生します。 それ以外の場合は、メンバーの型が暗黙的に `Object` またはメンバーの型文字の型になります。

__付箋.__ 構文にあいまいさはありません。宣言子が型を省略した場合、常に次の宣言子の型が使用されます。

インスタンスまたは共有変数の型または配列要素型のアクセシビリティドメインは、インスタンスまたは共有変数自体のアクセシビリティドメインのスーパーセットと同じであるか、またはそのスーパーセットと同じである必要があります。

次の例は、`redPart`、`greenPart`、および @no__t という名前の内部インスタンス変数を持つ @no__t 0 クラスを示しています。

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

インスタンスまたは共有変数の宣言に @no__t 0 修飾子が含まれている場合、宣言によって導入された変数への代入は、宣言の一部として、または同じクラスのコンストラクター内でのみ発生する可能性があります。 具体的には、読み取り専用インスタンスまたは共有変数への割り当ては、次の状況でのみ許可されます。

* (宣言に変数初期化子を含めることによって) インスタンスまたは共有変数を導入する変数宣言。

* インスタンス変数の場合は、変数宣言を含むクラスのインスタンスコンストラクター。 インスタンス変数にアクセスできるのは、非修飾の方法、または `Me` または `MyClass` です。

* 共有変数の場合は、共有変数宣言を含むクラスの共有コンストラクター。

共有読み取り専用変数は、定数値のシンボル名が必要な場合、定数宣言で値の型が許可されていない場合、またはコンパイル時に定数式によって値を計算できない場合に便利です。

最初のアプリケーションの例は次のようになります。色共有変数は、他のプログラムによって変更されないように `ReadOnly` として宣言されます。

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

定数と読み取り専用の共有変数のセマンティクスは異なります。 式が定数を参照する場合、定数の値はコンパイル時に取得されますが、式が読み取り専用の共有変数を参照している場合、共有変数の値は実行時まで取得されません。 次のアプリケーションを考えてみます。このアプリケーションは、2つの異なるプログラムで構成されています。

file1. vb:

```vb
Namespace Program1
    Public Class Utils
        Public Shared ReadOnly X As Integer = 1
    End Class
End Namespace
```

file2 .vb:

```vb
Namespace Program2
    Module Test
        Sub Main()
            Console.WriteLine(Program1.Utils.X)
        End Sub
    End Module
End Namespace
```

@No__t-0 および `Program2` という名前空間は、個別にコンパイルされた2つのプログラムを表します。 変数 `Program1.Utils.X` は `Shared ReadOnly` として宣言されているため、`Console.WriteLine` ステートメントによって出力される値はコンパイル時には認識されませんが、実行時に取得されます。 したがって、`X` の値が変更され `Program1` が再コンパイルされた場合、`Program2` が再コンパイルされない場合でも、`Console.WriteLine` ステートメントによって新しい値が出力されます。 ただし、`X` が定数である場合、`Program2` がコンパイルされた時点で `X` の値が取得され、@no__t が再コンパイルされるまで `Program1` の変更による影響を受けません。

### <a name="withevents-variables"></a>WithEvents 変数

型は、イベントを発生させたインスタンスまたは共有変数を @no__t 0 修飾子で宣言することによって、インスタンスまたは共有変数のいずれかによって発生するイベントのセットを処理することを宣言できます。 以下に例を示します。

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

この例では、メソッド `E1Handler` は、インスタンス @no__t 変数に格納されている型 `Raiser` のインスタンスによって発生するイベント `E1` を処理します。

@No__t-0 修飾子を指定すると、変数の名前は先頭にアンダースコアが付き、イベントフックと同じ名前のプロパティに置き換えられます。 たとえば、変数の名前が `F` の場合、`_F` に名前が変更され、プロパティ `F` が暗黙的に宣言されます。 変数の新しい名前と別の宣言との間に競合がある場合は、コンパイル時エラーが報告されます。 変数に適用された属性は、名前が変更された変数に引き継がれます。

@No__t 0 宣言によって作成された暗黙的なプロパティは、フックを処理し、関連するイベントハンドラーをアンフック中します。 変数に値が割り当てられている場合、プロパティはまず、変数に現在あるインスタンスのイベントに対して `remove` メソッドを呼び出します (既存のイベントハンドラーがある場合はアンフック中)。 次に、割り当てが行われ、プロパティは変数 (新しいイベントハンドラーをフックする) の新しいインスタンスのイベントに対して `add` メソッドを呼び出します。 次のコードは、標準モジュールの上記のコードと同じ `Test` です。

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

変数が構造体として型指定されている場合、インスタンスまたは共有変数を `WithEvents` として宣言することは無効です。 また、構造体で `WithEvents` を指定することはできません。 `WithEvents` と `ReadOnly` を組み合わせることはできません。

### <a name="variable-initializers"></a>変数初期化子

構造体のクラスおよびインスタンス変数宣言 (共有変数宣言ではありません) のインスタンスおよび共有変数宣言には、変数初期化子が含まれる場合があります。 @No__t 0 変数の場合、変数初期化子は、プログラムの開始後に実行される代入ステートメントに対応しますが、最初に @no__t の変数が参照される前に実行されます。 インスタンス変数の場合、変数初期化子は、クラスのインスタンスが作成されるときに実行される代入ステートメントに対応します。 パラメーターなしのコンストラクターは変更できないため、構造体にインスタンス変数初期化子を指定することはできません。

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

```console
x = 1.4142135623731, i = 100, s = Hello
```

@No__t-0 への代入は、クラスが読み込まれるときに発生します。クラスの新しいインスタンスが作成されると、`i` と `s` への割り当てが発生します。

変数初期化子は、型のコンストラクターのブロックに自動的に挿入される代入ステートメントと考えると便利です。 次の例には、いくつかのインスタンス変数初期化子が含まれています。

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

この例は、次に示すコードに対応しています。各コメントは、自動的に挿入されたステートメントを示しています。

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

変数初期化子が実行される前に、すべての変数がその型の既定値に初期化されます。 以下に例を示します。

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

クラスの読み込み時には `b` が自動的に既定値に初期化されるため、クラスのインスタンスが作成されると `i` は自動的に既定値に初期化されます。これにより、前のコードでは次の出力が生成されます。

```console
b = False, i = 0
```

各変数初期化子は、変数の型の値、または変数の型に暗黙的に変換できる型の値を生成する必要があります。 変数初期化子は、循環しているか、その後に初期化される変数を参照している可能性があります。この場合、参照先の変数の値は、初期化子のための既定値になります。 このような初期化子は、あいまい値です。

変数初期化子には、標準初期化子、配列サイズ初期化子、およびオブジェクト初期化子の3つの形式があります。 最初の2つの形式は、型名の後の等号の後に表示されます。後者の2つは宣言自体の一部です。 特定の宣言で使用できる初期化子の形式は1つだけです。

#### <a name="regular-initializers"></a>標準初期化子

*通常の初期化子*は、変数の型に暗黙的に変換できる式です。 これは、型名の後の等号の後に表示され、値として分類される必要があります。 以下に例を示します。

```vb
Module Test
    Dim x As Integer = 10
    Dim y As Integer = 20

    Sub Main()
        Console.WriteLine("x = " & x & ", y = " & y)
    End Sub
End Module
```

このプログラムでは、次の出力が生成されます。

```console
x = 10, y = 20
```

変数宣言に通常の初期化子がある場合は、一度に1つの変数のみを宣言できます。 以下に例を示します。

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

*オブジェクト初期化子*は、型名の代わりにオブジェクト作成式を使用して指定します。 オブジェクト初期化子は、オブジェクト作成式の結果を変数に割り当てる標準初期化子に相当します。 So

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

オブジェクト初期化子のかっこは、常にコンストラクターの引数リストとして解釈され、配列型修飾子としては解釈されません。 オブジェクト初期化子を持つ変数名には、配列型修飾子または null 許容型修飾子を指定できません。

#### <a name="array-size-initializers"></a>配列サイズ初期化子

*配列サイズ初期化子*は、式によって示される次元の上限のセットを提供する変数の名前に対する修飾子です。

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

上限式は値として分類する必要があり、`Integer` に暗黙的に変換できる必要があります。 上限のセットは、指定された上限を持つ配列作成式の変数初期化子に相当します。 配列型の次元数は、配列サイズ初期化子から推論されます。 So

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

すべての上限は-1 以上である必要があり、すべてのディメンションには上限が指定されている必要があります。 初期化される配列の要素型がそれ自体が配列型である場合、配列型の修飾子は配列サイズ初期化子の右側になります。 次に例を示します。

```vb
Module Test
    Sub Main()
        Dim x(5,10)(,,) As Integer
    End Sub
End Module
```

ローカル変数 @no__t 0 を宣言します。これは、型が `Integer` の3次元配列の2次元配列であり、1番目の次元に `0.5` の境界を持つ配列に初期化され、2番目の次元で `0.10` であることを示します。 配列サイズ初期化子を使用して、型が配列の配列である変数の要素を初期化することはできません。

配列サイズの初期化子を持つ変数宣言では、その型または標準の初期化子に配列型修飾子を含めることはできません。


### <a name="systemmarshalbyrefobject-classes"></a>MarshalByRefObject クラス

クラス `System.MarshalByRefObject` から派生したクラスは、(つまり、値渡しで) コピーするのではなく、プロキシを使用してコンテキスト境界を越えてマーシャリングされます (つまり、参照渡し)。 つまり、このようなクラスのインスタンスは実際のインスタンスではなくてもかまいませんが、コンテキスト境界を越えて変数アクセスとメソッド呼び出しをマーシャリングするスタブにするだけで済みます。

そのため、このようなクラスで定義されている変数の格納場所への参照を作成することはできません。 つまり、`System.MarshalByRefObject` から派生したクラスとして型指定された変数を参照パラメーターに渡すことはできません。また、値型として型指定された変数のメソッドと変数にはアクセスできません。 代わりに、このようなクラスで定義されている変数はプロパティと同じように処理されます (制限はプロパティと同じであるため、Visual Basic ではプロパティとして扱われます)。

この規則には例外が1つあります。 `Me` は常にプロキシではなく実際のオブジェクトであることが保証されるため、@no__t 0 で暗黙的または明示的に修飾されたメンバーは、上記の制限から除外されます。

## <a name="properties"></a>[プロパティ]

*プロパティ*は、変数の自然な拡張です。どちらも、関連付けられた型を持つ名前付きのメンバーであり、変数およびプロパティにアクセスするための構文は同じです。 ただし、変数とは異なり、プロパティはストレージの場所を意味しません。 代わりに、プロパティに*アクセサー*があります。アクセサーは、値の読み取りまたは書き込みを行うために実行するステートメントを指定します。

プロパティは、プロパティ宣言を使用して定義されます。 プロパティ宣言の最初の部分は、フィールド宣言に似ています。 2番目の部分には、@no__t 0 アクセサーまたは `Set` アクセサーが含まれます。

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

次の例では、`Button` クラスが `Caption` プロパティを定義しています。

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

上記の @no__t 0 クラスに基づいて、`Caption` プロパティの使用例を次に示します。

```vb
Dim okButton As Button = New Button()

okButton.Caption = "OK" ' Invokes Set accessor.
Dim s As String = okButton.Caption ' Invokes Get accessor.
```

ここでは、プロパティに値を割り当てることによって @no__t 0 アクセサーが呼び出され、`Get` アクセサーは、式のプロパティを参照することによって呼び出されます。

プロパティに型が指定されておらず、厳密なセマンティクスが使用されている場合、コンパイル時エラーが発生します。それ以外の場合、プロパティの型は暗黙的に `Object` またはプロパティの型文字の型になります。 プロパティの宣言には、プロパティの値を取得する @no__t 0 のアクセサー、またはプロパティの値を格納する `Set` のアクセサー、またはその両方を含めることができます。 プロパティはメソッドを暗黙的に宣言するので、メソッドと同じ修飾子を使用してプロパティを宣言できます。 プロパティがインターフェイスで定義されている場合、または @no__t 0 修飾子を使用して定義されている場合は、プロパティ本体と `End` コンストラクトを省略する必要があります。それ以外の場合は、コンパイル時エラーが発生します。

インデックスパラメーターリストは、プロパティのシグネチャを作成するので、プロパティの型ではなく、インデックスパラメーターに対してプロパティがオーバーロードされる可能性があります。 インデックスパラメーターリストは、通常のメソッドの場合と同じです。 ただし、どのパラメーターも @no__t 0 修飾子を使用して変更することはできません。また、どちらのパラメーターも `Value` (`Set` のアクセサーの暗黙的な値パラメーター用に予約されています) という名前にすることはできません。

プロパティは次のように宣言できます。

* プロパティがプロパティ型修飾子を指定していない場合、プロパティには @no__t 0 アクセサーと `Set` アクセサーの両方が含まれている必要があります。 プロパティは、読み取り/書き込みプロパティと呼ばれます。

* プロパティで `ReadOnly` 修飾子が指定されている場合、プロパティは @no__t のアクセサーを持つ必要があり、@no__t 2 のアクセサーを持つことはできません。 プロパティは、読み取り専用プロパティと呼ばれます。 読み取り専用プロパティが割り当てのターゲットである場合、コンパイル時エラーになります。

* プロパティで `WriteOnly` 修飾子が指定されている場合、プロパティは @no__t のアクセサーを持つ必要があり、@no__t 2 のアクセサーを持つことはできません。 プロパティは、書き込み専用プロパティと呼ばれます。 代入のターゲットまたはメソッドの引数として、式で書き込み専用のプロパティを参照する場合、コンパイル時エラーになります。

プロパティの @no__t 0 および `Set` アクセサーは個別のメンバーではないため、プロパティのアクセサーを個別に宣言することはできません。 次の例では、1つの読み取り/書き込みプロパティを宣言しません。 代わりに、同じ名前の2つのプロパティを宣言します。1つは読み取り専用で、もう1つは書き込み専用です。

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

同じクラスで宣言された2つのメンバーは同じ名前を持つことができないため、この例ではコンパイル時エラーが発生します。

既定では、プロパティの `Get` および `Set` アクセサーのアクセシビリティは、プロパティ自体のアクセシビリティと同じです。 ただし、`Get` および `Set` アクセサーは、プロパティとは別にアクセシビリティを指定することもできます。 この場合、アクセサーのアクセシビリティは、プロパティのアクセシビリティよりも制限されている必要があり、1つのアクセサーだけがプロパティから異なるアクセシビリティレベルを持つことができます。 アクセスの種類は、次のように制限の厳しいものと見なされます。

* `Private` は、`Public`、`Protected Friend`、`Protected`、または @no__t よりも制限されています。

* `Friend` は、`Protected Friend` または `Public` よりも制限されています。

* `Protected` は、`Protected Friend` または `Public` よりも制限されています。

* `Protected Friend` は `Public` よりも制限されています。

プロパティのアクセサーのいずれかにアクセスできても、もう一方がアクセスできない場合、プロパティは読み取り専用または書き込み専用であるかのように扱われます。 以下に例を示します。

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

派生型がプロパティをシャドウする場合、派生プロパティは、読み取りと書き込みの両方について、シャドウされたプロパティを非表示にします。 次の例では、`B` の `P` プロパティは、読み取りと書き込みの両方に対して `A` の `P` プロパティを非表示にします。

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

戻り値の型またはパラメーターの型のアクセシビリティドメインは、またはプロパティ自体のアクセシビリティドメインのスーパーセットと同じである必要があります。 プロパティには、1つの @no__t アクセサーと1つの @no__t 1 つのアクセサーのみを含めることができます。

宣言と呼び出しの構文の相違点を除いて、@no__t 0、`NotOverridable`、`Overrides`、`MustOverride`、および @no__t の各プロパティは、@no__t、@no__t、@no__t の各メソッドとまったく同じように動作します。 プロパティがオーバーライドされた場合、オーバーライドするプロパティは同じ型 (読み取り/書き込み、読み取り専用、書き込み専用) である必要があります。 @No__t 0 のプロパティには、`Private` アクセサーを含めることはできません。

次の @no__t 例では、-0 は @no__t 1 の読み取り専用プロパティで、`Y` は `Overridable` の読み取り/書き込みプロパティで、`Z` は `MustOverride` の読み取り/書き込みプロパティです。

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

@No__t-0 は `MustOverride` であるため、含んでいるクラス `A` は `MustInherit` として宣言する必要があります。

これに対して、クラス `A` から派生したクラスを次に示します。

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

ここでは、プロパティの宣言 `X`、`Y`、および `Z` によって基本プロパティがオーバーライドされます。 各プロパティ宣言は、対応する継承されたプロパティのアクセシビリティ修飾子、型、および名前と完全に一致します。 プロパティ `X` の @no__t アクセサーと、プロパティ `Y` の `Set` アクセサーは、`MyBase` キーワードを使用して、継承されたプロパティにアクセスします。 プロパティ `Z` の宣言は、`MustOverride` プロパティをオーバーライドします。したがって、クラス `B` の未処理の @no__t メンバーはなく、@no__t は通常のクラスにすることが許可されています。

プロパティを使用して、リソースが最初に参照されるまで、リソースの初期化を遅延させることができます。 以下に例を示します。

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

@No__t-0 クラスには、標準入力、出力、およびエラーデバイスをそれぞれ表す、`In`、`Out`、および `Error` の3つのプロパティが含まれています。 これらのメンバーをプロパティとして公開することにより、@no__t 0 クラスは実際に使用されるまで初期化を遅延させることができます。 たとえば、最初に `Out` プロパティを参照したときに、`ConsoleStreams.Out.WriteLine("hello, world")` のように、出力デバイスの基になる `TextWriter` が初期化されます。 ただし、アプリケーションが `In` および `Error` プロパティを参照していない場合、それらのデバイスのオブジェクトは作成されません。


### <a name="get-accessor-declarations"></a>Get アクセサー宣言

@No__t-0 アクセサー (getter) は、プロパティ `Get` 宣言を使用して宣言されています。 プロパティ `Get` の宣言は、キーワード `Get` の後にステートメントブロックを続けたもので構成されます。 @No__t-0 という名前のプロパティが指定されている場合、`Get` アクセサー宣言は、プロパティと同じ修飾子、型、およびパラメーターリストを持つ `get_P` という名前のメソッドを暗黙的に宣言します。 型にその名前の宣言が含まれている場合、コンパイル時エラーが発生しますが、暗黙的な宣言は名前のバインドのために無視されます。

特別なローカル変数は、プロパティと同じ名前の `Get` アクセサー本体の宣言領域で暗黙的に宣言され、プロパティの戻り値を表します。 ローカル変数は、式で使用されるときに特別な名前解決セマンティクスを持ちます。 呼び出し式など、メソッドグループとして分類された式を必要とするコンテキストでローカル変数が使用されている場合、その名前はローカル変数ではなく関数に解決されます。 以下に例を示します。

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

かっこを使用すると、あいまいな状況が発生する可能性があります。たとえば、@no__t `F` は、1次元配列の型を持つプロパティです。 あいまいな状況では、名前がローカル変数ではなくプロパティに解決されます。 以下に例を示します。

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

制御フローが @no__t 0 のアクセサー本体から出たときに、ローカル変数の値が呼び出し式に戻されます。 @No__t-0 アクセサーを呼び出すことは、変数の値を読み取ることと概念的には同じであるため、次の例に示すように、`Get` アクセサーが観測可能な副作用を持つように、不適切なプログラミングスタイルと見なされます。

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

@No__t-0 プロパティの値は、プロパティが以前にアクセスされた回数によって異なります。 したがって、プロパティにアクセスすると、観測可能な副作用が生成されます。代わりに、プロパティをメソッドとして実装する必要があります。

@No__t 0 のアクセサーに対する "副作用なし" 規則は、`Get` アクセサーが常に変数に格納されている値を返すように記述されていることを意味しません。 実際には、複数の変数にアクセスしたりメソッドを呼び出したりすることによって、多くの場合、@no__t 0 のアクセサーはプロパティの値を計算します。 ただし、適切に設計された `Get` アクセサーでは、オブジェクトの状態に対して監視可能な変更を引き起こすアクションは実行されません。

__付箋.__ `Get` アクセサーには、サブルーチンが持つ行の配置に対して同じ制限があります。 開始ステートメント、終了ステートメント、およびブロックはすべて論理行の先頭に記述する必要があります。

```antlr
PropertyGetDeclaration
    : Attributes? AccessModifier? 'Get' LineTerminator
      Block?
      'End' 'Get' StatementTerminator
    ;
```

### <a name="set-accessor-declarations"></a>アクセサー宣言の設定

@No__t-0 アクセサー (setter) は、プロパティセット宣言を使用して宣言されます。 プロパティセットの宣言は、キーワード `Set`、省略可能なパラメーターリスト、およびステートメントブロックで構成されます。 @No__t-0 という名前のプロパティを指定した場合、setter 宣言は、プロパティと同じ修飾子とパラメーターリストを持つ `set_P` という名前のメソッドを暗黙的に宣言します。 型にその名前の宣言が含まれている場合、コンパイル時エラーが発生しますが、暗黙的な宣言は名前のバインドのために無視されます。

パラメーターリストが指定されている場合は、1つのメンバーを持つ必要があります。また、メンバーは `ByVal` 以外の修飾子を持つ必要があり、その型はプロパティの型と同じである必要があります。 パラメーターは、設定されるプロパティ値を表します。 パラメーターを省略した場合、`Value` という名前のパラメーターが暗黙的に宣言されます。

__付箋.__ `Set` アクセサーには、サブルーチンが持つ行の配置に対して同じ制限があります。 開始ステートメント、終了ステートメント、およびブロックはすべて論理行の先頭に記述する必要があります。

```antlr
PropertySetDeclaration
    : Attributes? AccessModifier? 'Set'
      ( OpenParenthesis ParameterList? CloseParenthesis )? LineTerminator
      Block?
      'End' 'Set' StatementTerminator
    ;
```

### <a name="default-properties"></a>既定のプロパティ

修飾子 @no__t 0 を指定するプロパティは、*既定のプロパティ*と呼ばれます。 プロパティを許可する型には、インターフェイスを含む既定のプロパティが含まれる場合があります。 プロパティの名前を使用してインスタンスを修飾しなくても、既定のプロパティを参照できます。 したがって、クラスが指定されています。

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

プロパティが `Default` として宣言されると、継承階層内のその名前でオーバーロードされたすべてのプロパティが、`Default` で宣言されているかどうかにかかわらず、既定のプロパティになります。 基底クラスで別の名前によって既定のプロパティが宣言されている場合に、派生クラスのプロパティ `Default` に宣言すると、`Shadows` や `Overrides` などの他の修飾子は必要ありません。 これは、既定のプロパティに id またはシグネチャがないため、シャドウまたはオーバーロードできないためです。 以下に例を示します。

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

このプログラムでは、次の出力が生成されます。

```console
MoreDerived = 10
Derived = 10
Base = 10
```

型の中で宣言されたすべての既定のプロパティは、同じ名前である必要があります。わかりやすくするために、@no__t 0 修飾子を指定する必要があります。 インデックスパラメーターが指定されていない既定のプロパティでは、含んでいるクラスのインスタンスが割り当てられるときにあいまいな状況が発生するため、既定のプロパティにはインデックスパラメーターが必要です。 さらに、特定の名前でオーバーロードされた1つのプロパティに @no__t 0 修飾子が含まれる場合は、その名前でオーバーロードされたすべてのプロパティでそれを指定する必要があります。 既定のプロパティを @no__t 0 にすることはできません。また、プロパティの少なくとも1つのアクセサーを `Private` にすることはできません。

### <a name="automatically-implemented-properties"></a>自動的に実装されたプロパティ

プロパティが任意のアクセサーの宣言を省略した場合、プロパティがインターフェイスで宣言されていないか、または `MustOverride` として宣言されていない限り、プロパティの実装は自動的に指定されます。 引数を指定しない読み取り/書き込みプロパティのみが自動的に実装されます。それ以外の場合は、コンパイル時エラーが発生します。

自動的に実装されるプロパティ `x` で、別のプロパティをオーバーライドする場合でも、プロパティと同じ型のプライベートローカル変数 `_x` が導入されます。 ローカル変数の名前と別の宣言との間に競合がある場合は、コンパイル時エラーが報告されます。 自動的に実装されたプロパティの `Get` アクセサーは、ローカルの値と、ローカルの値を設定するプロパティの `Set` アクセサーを返します。 たとえば、次のように宣言します。

```vb
Public Property x() As Integer
```

は、次の場合とほぼ同じです。

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

変数宣言と同様に、自動的に実装されるプロパティに初期化子を含めることができます。 以下に例を示します。

```vb
Public Property x() As Integer = 10
Public Shared Property y() As New Customer() With { .Name = "Bob" }
```

__付箋.__ 自動的に実装されたプロパティが初期化されると、基になるフィールドではなく、プロパティによって初期化されます。 そのため、プロパティをオーバーライドすると、必要に応じて初期化を受け取ることができます。

配列初期化子は、自動的に実装されるプロパティで使用できます。ただし、配列の範囲を明示的に指定する方法はありません。  以下に例を示します。

```vb
' Valid
Property x As Integer() = {1, 2, 3}
Property y As Integer(,) = {{1, 2, 3}, {12, 13, 14}, {11, 10, 9}}

' Invalid
Property x4(5) As Short
```

### <a name="iterator-properties"></a>反復子のプロパティ

*Iterator プロパティ*は、`Iterator` 修飾子を持つプロパティです。 これは、反復子メソッド (セクション[反復子メソッド](statements.md#iterator-methods)) が使用されるのと同じ理由で使用されます。シーケンスを生成するための便利な方法として、`For Each` ステートメントで使用できます。 Iterator プロパティの @no__t 0 のアクセサーは、iterator メソッドと同じように解釈されます。

Iterator プロパティには、明示的な @no__t 0 アクセサーを指定する必要があります。また、その型は、`IEnumerator`、`IEnumerable`、また @no__t は `T` のいずれかの `IEnumerable(Of T)` である必要があります。

Iterator プロパティの例を次に示します。

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

*演算子*は、含んでいるクラスの既存の Visual Basic 演算子の意味を定義するメソッドです。 演算子が式のクラスに適用されると、演算子は、クラスで定義されている演算子メソッドの呼び出しにコンパイルされます。 クラスに対して演算子を定義することは、演算子の*オーバーロード*とも呼ばれます。

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

既に存在する演算子をオーバーロードすることはできません。実際には、これは変換演算子に適用されます。 たとえば、派生クラスから基底クラスへの変換をオーバーロードすることはできません。

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

演算子は、次の単語のような一般的な意味でオーバーロードすることもできます。

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

演算子宣言は、包含する型の宣言空間に明示的に名前を追加しません。ただし、これらのメソッドは、"op_" という文字で始まる対応するメソッドを暗黙的に宣言します。 次のセクションでは、それぞれの演算子に対応するメソッド名を示します。

定義可能な演算子には、単項演算子、二項演算子、および変換演算子の3つのクラスがあります。 すべての演算子宣言は、特定の制限を共有します。

* 演算子の宣言は、常に 0 @no__t、`Shared` である必要があります。 修飾子が想定されるコンテキストでは、@no__t 0 修飾子を省略できます。

* 演算子のパラメーターを `ByRef`、`Optional`、`ParamArray` のいずれかに宣言することはできません。

* 少なくとも1つのオペランドまたは戻り値の型は、演算子を含む型である必要があります。

* 演算子に対して定義された関数の戻り変数がありません。 したがって、演算子本体から値を返すには、`Return` ステートメントを使用する必要があります。

これらの制限の唯一の例外は、null 許容値型に適用されます。 Null 許容値型には実際の型定義がないため、値型では、型の null 許容バージョンに対してユーザー定義の演算子を宣言できます。 型が特定のユーザー定義の演算子を宣言できるかどうかを判断するときに、検証の目的で、宣言に含まれるすべての型から @no__t 0 修飾子が最初に削除されます。 この緩和は、`IsTrue` および `IsFalse` 演算子の戻り値の型には適用されません。`Boolean?` ではなく `Boolean` を返す必要があります。

演算子の優先順位と結合規則は、演算子宣言では変更できません。

__付箋.__ 演算子には、サブルーチンが持つ行の配置に対して同じ制限があります。 開始ステートメント、終了ステートメント、およびブロックはすべて論理行の先頭に記述する必要があります。


### <a name="unary-operators"></a>単項演算子

次の単項演算子はオーバーロードできます。

* 単項プラス演算子 `+` (対応するメソッド: `op_UnaryPlus`)

* 単項マイナス演算子 `-` (対応するメソッド: `op_UnaryNegation`)

* 論理 `Not` 演算子 (対応するメソッド: `op_OnesComplement`)

* @No__t-0 および `IsFalse` 演算子 (対応するメソッド: `op_True`、`op_False`)

オーバーロードされた単項演算子は、それを含んでいる型の1つのパラメーターを受け取り、`Boolean` を返す必要がある `IsTrue` および `IsFalse` を除く、任意の型を返すことができます。 含んでいる型がジェネリック型の場合、型パラメーターは、それを含む型の型パラメーターと一致する必要があります。 例を次に示します。

```vb
Structure Complex
    ...

    Public Shared Operator +(v As Complex) As Complex
        Return v
    End Operator
End Structure
```

型が `IsTrue` または `IsFalse` のいずれかをオーバーロードする場合は、もう一方もオーバーロードする必要があります。 1つだけがオーバーロードされている場合は、コンパイル時エラーが発生します。

__付箋.__ `IsTrue` および `IsFalse` は予約語ではありません。

### <a name="binary-operators"></a>二項演算子

次の二項演算子はオーバーロードできます。

* 加算 `+`、減算 `-`、乗算 `*`、除算 `/`、整数除算 `\`、モジュロ `Mod`、および指数 `^` 演算子 (対応するメソッド: `op_Addition`、`op_Subtraction`、`op_Multiply`、0、1、2、3)

* 関係演算子 `=`、`<>`、`<`、`>`、`<=`、`>=` (対応するメソッド: @no__t 6、`op_Inequality`、`op_LessThan`、`op_GreaterThan`、0、1)。 __付箋.__ 等値演算子はオーバーロードできますが、代入演算子 (代入ステートメントでのみ使用) をオーバーロードすることはできません。

* @No__t-0 演算子 (対応するメソッド: `op_Like`)

* 連結演算子 `&` (対応するメソッド: `op_Concatenate`)

* 論理 `And`、`Or`、および `Xor` 演算子 (対応するメソッド: `op_BitwiseAnd`、`op_BitwiseOr`、`op_ExclusiveOr`)

* シフト演算子 `<<` および `>>` (対応するメソッド: `op_LeftShift`、`op_RightShift`)

オーバーロードされた二項演算子はすべて、パラメーターの1つとして、含んでいる型を受け取る必要があります。 含んでいる型がジェネリック型の場合、型パラメーターは、それを含む型の型パラメーターと一致する必要があります。 シフト演算子は、最初のパラメーターが含まれている型であることを要求するように、この規則をさらに制限します。2番目のパラメーターは、常に `Integer` 型である必要があります。

次の二項演算子は、ペアで宣言する必要があります。

* Operator `=` および operator `<>`

* Operator `>` および operator `<`

* Operator `>=` および operator `<=`

ペアの1つが宣言されている場合、もう一方は、対応するパラメーターと戻り値の型で宣言されている必要があります。一致しない場合、コンパイル時エラーが発生します。 (__注:__ 関係演算子のペア宣言を要求する目的は、オーバーロードされた演算子で少なくとも最小レベルの論理的な一貫性を確保することです。

関係演算子とは異なり、除算演算子と整数除算演算子の両方をオーバーロードすることは、エラーではなく、避けることを強くお勧めします。 (__注:__ 一般に、2種類の除算は完全に区別されていなければなりません。除算をサポートする型は、整数 (この場合は `\` をサポートする必要があります) であるか、そうではありません (この場合は `/` をサポートする必要があります)。 両方の演算子を定義するのにエラーが発生したと考えていましたが、これらの言語では、一般的には Visual Basic の2種類の除算を区別していないので、この方法を使用するのは安全だと思いますが、この方法は強くお勧めします)。

複合代入演算子を直接オーバーロードすることはできません。 代わりに、対応する二項演算子がオーバーロードされると、複合代入演算子はオーバーロードされた演算子を使用します。 以下に例を示します。

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

変換演算子は、型の間の新しい変換を定義します。 これらの新しい変換は、*ユーザー定義の変換*と呼ばれます。 変換演算子は、変換演算子のパラメーター型で示される変換元の型を変換演算子の戻り値の型で指定されたターゲットの型に変換します。 変換は、拡大または縮小として分類する必要があります。 @No__t-0 キーワードを含む変換演算子の宣言では、ユーザー定義の拡大変換 (対応するメソッド: `op_Implicit`) が導入されます。 @No__t-0 キーワードを含む変換演算子の宣言では、ユーザー定義の縮小変換 (対応するメソッド: `op_Explicit`) が導入されます。

一般に、ユーザー定義の拡大変換では、例外がスローされず、情報が失われることがないように設計する必要があります。 ユーザー定義の変換によって例外が発生する可能性がある場合 (たとえば、ソース引数が範囲外の場合)、または情報が失われた場合 (上位ビットを破棄する場合など)、その変換は縮小変換として定義する必要があります。 次に例を示します。

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

`Digit` から `Byte` への変換は、例外がスローされたり情報が失われたりすることはないため、拡大変換になります。ただし、`Digit` は、の有効な値のサブセットのみを表すことができるため、`Byte` から `Digit` への変換は縮小変換です。`Byte`。

オーバーロードできる他のすべての型メンバーとは異なり、変換演算子のシグネチャには変換の対象の型が含まれます。 これは、戻り値の型がシグネチャに参加する唯一の型のメンバーです。 ただし、変換演算子の拡大または縮小の分類は、演算子のシグネチャの一部ではありません。 したがって、クラスまたは構造体は、拡大変換演算子と、同じソースとターゲットの型を持つ縮小変換演算子の両方を宣言することはできません。

ユーザー定義の変換演算子は、含んでいる型との間で変換を行う必要があります。たとえば、クラス `C` は、`C` から `Integer` への変換と、`Integer` から `C` への変換を定義することができますが、`Integer` から `Boolean` への変換は実行できません。 含んでいる型がジェネリック型の場合、型パラメーターは、それを含む型の型パラメーターと一致する必要があります。 また、組み込みの (つまり、非ユーザー定義の) 変換を再定義することはできません。 その結果、型は次のような変換を宣言できません。

* 変換元の型と変換先の型が同じです。

* 変換元の型と変換先の型の両方が、変換演算子を定義する型ではありません。

* 変換元の型または変換先の型がインターフェイス型です。

* 変換元の型と変換先の型は、継承によって関連付けられます (@no__t 0 を含む)。

これらの規則の唯一の例外は、null 許容の値型に適用されます。 Null 許容値型には実際の型定義がないため、値型では、型の null 許容バージョンのユーザー定義変換を宣言できます。 型で特定のユーザー定義の変換を宣言できるかどうかを判断するときに、`?` 修飾子は、有効性チェックの目的で、宣言に含まれるすべての型から最初に削除されます。 したがって、次の宣言は、`S` が `S` から `T` への変換を定義できるため、有効です。

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

ただし、次の宣言は有効ではありません。構造 `S` は、`S` から `S` への変換を定義できません。

```vb
Structure S
    Public Shared Widening Operator CType(ByVal v As S) As S?
        ...
    End Operator
End Structure
```

### <a name="operator-mapping"></a>演算子のマッピング

でサポートされて Visual Basic いる一連の演算子は、.NET Framework にある他の言語の演算子のセットと完全に一致しない場合があるため、一部の演算子は定義または使用されているときに、他の演算子に特別にマップされます。 具体的な内容は次のとおりです。

* 整数除算演算子を定義すると、整数除算演算子を呼び出す通常の除算演算子 (他の言語からのみ使用可能) が自動的に定義されます。

* @No__t-0、`And`、および `Or` 演算子をオーバーロードすると、論理演算子とビット処理演算子を区別する他の言語の観点から、ビットごとの演算子のみがオーバーロードされます。

* 論理演算子とビット演算子 (つまり、`op_LogicalNot`、`op_LogicalAnd` を使用する言語、および `Not`、`And`、`Or` の `op_LogicalOr`) を区別する論理演算子のみをオーバーロードするクラスは、それぞれの論理演算子を持つVisual Basic 論理演算子にマップされた演算子。 論理演算子とビット処理演算子の両方がオーバーロードされている場合は、ビットごとの演算子だけが使用されます。

* @No__t-0 および `>>` 演算子をオーバーロードすると、符号付きおよび符号なしのシフト演算子を区別する他の言語の観点から、符号付き演算子だけがオーバーロードされます。

* 符号なしシフト演算子のみをオーバーロードするクラスでは、対応する Visual Basic シフト演算子にマップされた符号なしシフト演算子を持つことになります。 符号なしおよび符号付きシフト演算子の両方がオーバーロードされている場合は、符号付きシフト演算子だけが使用されます。
