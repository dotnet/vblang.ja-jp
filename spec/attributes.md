# <a name="attributes"></a><span data-ttu-id="ae363-101">属性</span><span class="sxs-lookup"><span data-stu-id="ae363-101">Attributes</span></span>

<span data-ttu-id="ae363-102">Visual Basic 言語により、プログラマは、宣言されているエンティティに関する情報を表す宣言に修飾子を指定できます。</span><span class="sxs-lookup"><span data-stu-id="ae363-102">The Visual Basic language enables the programmer to specify modifiers on declarations, which represent information about the entities being declared.</span></span> <span data-ttu-id="ae363-103">たとえば、修飾子を使用してクラスのメソッドを付加すること`Public`、 `Protected`、 `Friend`、 `Protected Friend`、または`Private`アクセシビリティを指定します。</span><span class="sxs-lookup"><span data-stu-id="ae363-103">For example, affixing a class method with the modifiers `Public`, `Protected`, `Friend`, `Protected Friend`, or `Private` specifies its accessibility.</span></span>

<span data-ttu-id="ae363-104">言語によって定義された修飾子、に加えて Visual Basic も作成を可能と呼ばれる新しい修飾子は、*属性*、新しいエンティティを宣言するときに使用するとします。</span><span class="sxs-lookup"><span data-stu-id="ae363-104">In addition to the modifiers defined by the language, Visual Basic also enables programmers to create new modifiers, called *attributes*, and to use them when declaring new entities.</span></span> <span data-ttu-id="ae363-105">属性クラスの宣言で定義されて、これらの新しい修飾子は使用してエンティティに割り当てられます*属性ブロック*します。</span><span class="sxs-lookup"><span data-stu-id="ae363-105">These new modifiers, which are defined through the declaration of attribute classes, are then assigned to entities through *attribute blocks*.</span></span>

<span data-ttu-id="ae363-106">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="ae363-106">__Note.__</span></span> <span data-ttu-id="ae363-107">属性は、.NET Framework のリフレクション Api を介して実行時に取得できます。</span><span class="sxs-lookup"><span data-stu-id="ae363-107">Attributes may be retrieved at run time through the .NET Framework's reflection APIs.</span></span> <span data-ttu-id="ae363-108">これらの Api は、この仕様の範囲外です。</span><span class="sxs-lookup"><span data-stu-id="ae363-108">These APIs are outside the scope of this specification.</span></span>

<span data-ttu-id="ae363-109">たとえば、フレームワークは、定義、`Help`次の例に示すように、ドキュメントへのプログラム要素からマッピングを提供するクラスおよびメソッドなどのプログラム要素に配置できる属性。</span><span class="sxs-lookup"><span data-stu-id="ae363-109">For instance, a framework might define a `Help` attribute that can be placed on program elements such as classes and methods to provide a mapping from program elements to documentation, as the following example demonstrates:</span></span>

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

<span data-ttu-id="ae363-110">例では、という名前の属性クラスを定義します`HelpAttribute`、または`Help`、略してを持つ 1 つの位置指定パラメーター (`UrlValue`) 1 つの名前付き引数と (`Topic`)。</span><span class="sxs-lookup"><span data-stu-id="ae363-110">The example defines an attribute class named `HelpAttribute`, or `Help` for short, that has one positional parameter (`UrlValue`) and one named argument (`Topic`).</span></span>

<span data-ttu-id="ae363-111">次の例では、属性のいくつかの用途を示します。</span><span class="sxs-lookup"><span data-stu-id="ae363-111">The next example shows several uses of the attribute:</span></span>

```vb
<Help("http://www.example.com/.../Class1.htm")> _
Public Class Class1
    <Help("http://www.example.com/.../Class1.htm", Topic:="F")> _
    Public Sub F()
    End Sub
End Class
```

<span data-ttu-id="ae363-112">次の例をかどうかを確認します`Class1`が、`Help`属性、および関連付けられた書き込み`Topic`と`Url`属性が存在する場合の値します。</span><span class="sxs-lookup"><span data-stu-id="ae363-112">The next example checks to see if `Class1` has a `Help` attribute, and writes out the associated `Topic` and `Url` values if the attribute is present.</span></span>

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

## <a name="attribute-classes"></a><span data-ttu-id="ae363-113">属性クラス</span><span class="sxs-lookup"><span data-stu-id="ae363-113">Attribute Classes</span></span>

<span data-ttu-id="ae363-114">*属性クラス*から派生した非ジェネリック クラスは、`System.Attribute`およびない`MustInherit`します。</span><span class="sxs-lookup"><span data-stu-id="ae363-114">An *attribute class* is a non-generic class that derives from `System.Attribute` and is not `MustInherit`.</span></span> <span data-ttu-id="ae363-115">属性クラスがあります、`System.AttributeUsage`属性はでは有効で、それが宣言では、使用することがあります複数回、および継承されているかどうかを宣言する属性。</span><span class="sxs-lookup"><span data-stu-id="ae363-115">The attribute class may have a `System.AttributeUsage` attribute that declares what the attribute is valid on, whether it may be used multiple times in a declaration, and whether it is inherited.</span></span> <span data-ttu-id="ae363-116">次の例は、という名前の属性クラスを定義します。`SimpleAttribute`クラス宣言でインターフェイスの宣言を配置できます。</span><span class="sxs-lookup"><span data-stu-id="ae363-116">The following example defines an attribute class named `SimpleAttribute` that can be placed on class declarations and interface declarations:</span></span>

```vb
<AttributeUsage(AttributeTargets.Class Or AttributeTargets.Interface)> _
Public Class SimpleAttribute
    Inherits System.Attribute
End Class
```

<span data-ttu-id="ae363-117">次の例は、いくつかの使用、`Simple`属性。</span><span class="sxs-lookup"><span data-stu-id="ae363-117">The next example shows a few uses of the `Simple` attribute.</span></span> <span data-ttu-id="ae363-118">属性クラスの名前が`SimpleAttribute`、この属性の使用方法が省略、`Attribute`に名前を短縮され、サフィックス`Simple`:</span><span class="sxs-lookup"><span data-stu-id="ae363-118">Although the attribute class is named `SimpleAttribute`, uses of this attribute may omit the `Attribute` suffix, thus shortening the name to `Simple`:</span></span>

```vb
<Simple> Class Class1
End Class

<Simple> Interface Interface1
End Interface
```

<span data-ttu-id="ae363-119">属性がない場合、 `System.AttributeUsage`、属性は、任意のターゲットに配置できますし、(に相当`AttributeTargets.All`)。</span><span class="sxs-lookup"><span data-stu-id="ae363-119">If the attribute lacks a `System.AttributeUsage`, then the attribute can be placed on any target (equivalent to `AttributeTargets.All`).</span></span> <span data-ttu-id="ae363-120">`System.AttributeUsage`属性は、変数の初期化子が`AllowMultiple`、特定の宣言の示された属性を複数回指定できるかどうかを指定します。</span><span class="sxs-lookup"><span data-stu-id="ae363-120">The `System.AttributeUsage` attribute has a variable initializer, `AllowMultiple`, which specifies whether the indicated attribute can be specified more than once for a given declaration.</span></span> <span data-ttu-id="ae363-121">場合`AllowMultiple`属性は`True`は、*属性クラスの複数の用途*宣言で複数回指定できます。</span><span class="sxs-lookup"><span data-stu-id="ae363-121">If `AllowMultiple` for an attribute is `True`, it is a *multiple-use attribute class*, and can be specified more than once on a declaration.</span></span> <span data-ttu-id="ae363-122">場合`AllowMultiple`属性は`False`または属性に指定されていない場合は、*単一使用属性クラス*、多くても 1 回宣言で指定できます。</span><span class="sxs-lookup"><span data-stu-id="ae363-122">If `AllowMultiple` for an attribute is `False` or unspecified for an attribute, it is a *single-use attribute class*, and can be specified at most once on a declaration.</span></span>

<span data-ttu-id="ae363-123">次の例は、という名前の属性を複数使用クラスを定義します`AuthorAttribute`:。</span><span class="sxs-lookup"><span data-stu-id="ae363-123">The following example defines a multiple-use attribute class named `AuthorAttribute`:</span></span>

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

<span data-ttu-id="ae363-124">2 つの用途をクラス宣言の例を示します、`Author`属性。</span><span class="sxs-lookup"><span data-stu-id="ae363-124">The example shows a class declaration with two uses of the `Author` attribute:</span></span>

```vb
<Author("Maria Hammond"), Author("Ramesh Meyyappan")> _
Class Class1
End Class
```

<span data-ttu-id="ae363-125">`System.AttributeUsage`属性が、パブリック インスタンス変数`Inherited`、基本型で指定する場合は、属性がこの基本型から派生した型でも継承されたかどうかを指定します。</span><span class="sxs-lookup"><span data-stu-id="ae363-125">The `System.AttributeUsage` attribute has a public instance variable, `Inherited`, that specifies whether the attribute, when specified on a base type, is also inherited by types that derive from this base type.</span></span> <span data-ttu-id="ae363-126">場合、`Inherited`パブリック インスタンス変数が初期化されていない、既定値は`False`使用されます。</span><span class="sxs-lookup"><span data-stu-id="ae363-126">If the `Inherited` public instance variable is not initialized, a default value of `False` is used.</span></span> <span data-ttu-id="ae363-127">プロパティおよびイベントは、プロパティおよびイベントによって定義されたメソッドが、属性を継承しないでください。</span><span class="sxs-lookup"><span data-stu-id="ae363-127">Properties and events do not inherit attributes, although the methods defined by properties and events do.</span></span> <span data-ttu-id="ae363-128">インターフェイスは、属性を継承しません。</span><span class="sxs-lookup"><span data-stu-id="ae363-128">Interfaces do not inherit attributes.</span></span>

<span data-ttu-id="ae363-129">単一使用属性が継承および派生型で指定された、派生型で指定されている属性は継承された属性より優先されます。</span><span class="sxs-lookup"><span data-stu-id="ae363-129">If a single-use attribute is both inherited and specified on a derived type, the attribute specified on the derived type overrides the inherited attribute.</span></span> <span data-ttu-id="ae363-130">場合は、複数の用途の属性が継承および派生型で指定されて、両方の属性は、派生型を指定します。</span><span class="sxs-lookup"><span data-stu-id="ae363-130">If a multiple-use attribute is both inherited and specified on a derived type, both attributes are specified on the derived type.</span></span> <span data-ttu-id="ae363-131">例えば:</span><span class="sxs-lookup"><span data-stu-id="ae363-131">For example:</span></span>

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

<span data-ttu-id="ae363-132">属性の位置指定パラメーターは、属性クラスのパブリック コンス トラクターのパラメーターによって定義されます。</span><span class="sxs-lookup"><span data-stu-id="ae363-132">The positional parameters of the attribute are defined by the parameters of the public constructors of the attribute class.</span></span> <span data-ttu-id="ae363-133">位置指定パラメーターである必要があります`ByVal`可能性がありますを指定しないと`ByRef`します。</span><span class="sxs-lookup"><span data-stu-id="ae363-133">Positional parameters must be `ByVal` and may not specify `ByRef`.</span></span> <span data-ttu-id="ae363-134">パブリック インスタンス変数とプロパティは、パブリックの読み取り/書き込みプロパティまたは属性クラスのインスタンス変数によって定義されます。</span><span class="sxs-lookup"><span data-stu-id="ae363-134">Public instance variables and properties are defined by public read-write properties or instance variables of the attribute class.</span></span> <span data-ttu-id="ae363-135">位置指定パラメーターとインスタンスのパブリック変数のプロパティで使用できる型では、属性の型に制限されます。</span><span class="sxs-lookup"><span data-stu-id="ae363-135">The types that can be used in positional parameters and public instance variables and properties are restricted to attribute types.</span></span> <span data-ttu-id="ae363-136">型が属性型で、次のいずれかに該当する場合です。</span><span class="sxs-lookup"><span data-stu-id="ae363-136">A type is an attribute type if it is one of the following:</span></span>

* <span data-ttu-id="ae363-137">任意のプリミティブ型を除く`Date`と`Decimal`します。</span><span class="sxs-lookup"><span data-stu-id="ae363-137">Any primitive type except for `Date` and `Decimal`.</span></span>

* <span data-ttu-id="ae363-138">型 `Object`。</span><span class="sxs-lookup"><span data-stu-id="ae363-138">The type `Object`.</span></span>

* <span data-ttu-id="ae363-139">型 `System.Type`。</span><span class="sxs-lookup"><span data-stu-id="ae363-139">The type `System.Type`.</span></span>

* <span data-ttu-id="ae363-140">列挙型、し、型では、入れ子になっている (あれば) が提供された`Public`アクセシビリティ。</span><span class="sxs-lookup"><span data-stu-id="ae363-140">An enumerated type, provided that it and the types in which it is nested (if any) have `Public` accessibility.</span></span>

* <span data-ttu-id="ae363-141">この一覧の前の型のいずれかの 1 次元配列。</span><span class="sxs-lookup"><span data-stu-id="ae363-141">A one-dimensional array of one of the previous types in this list.</span></span>

## <a name="attribute-blocks"></a><span data-ttu-id="ae363-142">属性ブロック</span><span class="sxs-lookup"><span data-stu-id="ae363-142">Attribute Blocks</span></span>

<span data-ttu-id="ae363-143">属性で指定*属性ブロック*します。</span><span class="sxs-lookup"><span data-stu-id="ae363-143">Attributes are specified in *attribute blocks*.</span></span> <span data-ttu-id="ae363-144">各属性ブロックは、山かっこ ("<>") で区切られた、属性ブロック内のコンマ区切りの一覧で、または複数の属性ブロックで、複数の属性を指定できます。</span><span class="sxs-lookup"><span data-stu-id="ae363-144">Each attribute block is delimited by angle brackets ("<>"), and multiple attributes can be specified in a comma-separated list within an attribute block or in multiple attribute blocks.</span></span> <span data-ttu-id="ae363-145">属性が指定されている順序が重要ではありません。</span><span class="sxs-lookup"><span data-stu-id="ae363-145">The order in which attributes are specified is not significant.</span></span> <span data-ttu-id="ae363-146">たとえば、属性ブロック`<A, B>`、 `<B, A>`、`<A> <B>`と`<B> <A>`はすべて同等です。</span><span class="sxs-lookup"><span data-stu-id="ae363-146">For example, the attribute blocks `<A, B>`, `<B, A>`, `<A> <B>` and `<B> <A>` are all equivalent.</span></span>

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

<span data-ttu-id="ae363-147">属性を指定できませんそうでない宣言のような属性ブロックでのサポート、および単一使用属性が複数回指定するされません。</span><span class="sxs-lookup"><span data-stu-id="ae363-147">An attribute may not be specified on a kind of declaration it does not support, and single-use attributes may not be specified more than once in an attribute block.</span></span> <span data-ttu-id="ae363-148">使用しようとするために、次の例によって両方のエラーが発生`HelpString`インターフェイスで`Interface1`の宣言で複数回と`Class1`します。</span><span class="sxs-lookup"><span data-stu-id="ae363-148">The example below causes errors both because it attempts to use `HelpString` on the interface `Interface1` and more than once on the declaration of `Class1`.</span></span>

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

<span data-ttu-id="ae363-149">属性は、省略可能な属性の修飾子、属性名、位置指定引数と変数/プロパティの初期化子のオプションのリストで構成されます。</span><span class="sxs-lookup"><span data-stu-id="ae363-149">An attribute consists of an optional attribute modifier, an attribute name, an optional list of positional arguments, and variable/property initializers.</span></span> <span data-ttu-id="ae363-150">パラメーターまたは初期化子がない場合、かっこは省略できます。</span><span class="sxs-lookup"><span data-stu-id="ae363-150">If there are no parameters or initializers, the parentheses may be omitted.</span></span> <span data-ttu-id="ae363-151">属性に、修飾子がある場合は、ソース ファイルの上部にある属性ブロックでがあります。</span><span class="sxs-lookup"><span data-stu-id="ae363-151">If an attribute has a modifier, it must be in an attribute block at the top of a source file.</span></span>

<span data-ttu-id="ae363-152">属性ブロック内の各属性を両方によってプレフィックス指定する必要がありますが、ソース ファイルにアセンブリまたはソース ファイルが含まれるモジュールの属性を指定するファイルの上部にある属性ブロックが含まれている場合、`Assembly`または`Module`修飾子およびコロンです。</span><span class="sxs-lookup"><span data-stu-id="ae363-152">If a source file contains an attribute block at the top of the file that specifies attributes for the assembly or module that will contain the source file, each attribute in the attribute block must be prefixed by both the `Assembly` or `Module` modifier and a colon.</span></span>


### <a name="attribute-names"></a><span data-ttu-id="ae363-153">属性名</span><span class="sxs-lookup"><span data-stu-id="ae363-153">Attribute Names</span></span>

<span data-ttu-id="ae363-154">属性の名前には、属性クラスを指定します。</span><span class="sxs-lookup"><span data-stu-id="ae363-154">The name of an attribute specifies an attribute class.</span></span> <span data-ttu-id="ae363-155">慣例により、属性クラスのサフィックスを持つ名前は`Attribute`します。</span><span class="sxs-lookup"><span data-stu-id="ae363-155">By convention, attribute classes are named with the suffix `Attribute`.</span></span> <span data-ttu-id="ae363-156">属性の使用は可能性があります含めるか、このサフィックスを省略します。</span><span class="sxs-lookup"><span data-stu-id="ae363-156">Uses of an attribute may either include or omit this suffix.</span></span> <span data-ttu-id="ae363-157">したがって、属性の識別子に対応する属性クラスの名前が識別子自体または修飾された識別子の連結と`Attribute`します。</span><span class="sxs-lookup"><span data-stu-id="ae363-157">Consequently the name of an attribute class that corresponds to an attribute identifier is either the identifier itself or the concatenation of the qualified identifier and `Attribute`.</span></span> <span data-ttu-id="ae363-158">コンパイラは、属性名を解決するときの追加`Attribute`名前にして検索します。</span><span class="sxs-lookup"><span data-stu-id="ae363-158">When the compiler resolves an attribute name, it appends `Attribute` to the name and tries the lookup.</span></span> <span data-ttu-id="ae363-159">検索に失敗した場合、コンパイラはサフィックスの検索を試みます。</span><span class="sxs-lookup"><span data-stu-id="ae363-159">If that lookup fails, the compiler tries the lookup without the suffix.</span></span> <span data-ttu-id="ae363-160">たとえば、属性クラスの使用`SimpleAttribute`省略可能性があります、`Attribute`に名前を短縮され、サフィックス`Simple`:</span><span class="sxs-lookup"><span data-stu-id="ae363-160">For example, uses of an attribute class `SimpleAttribute` may omit the `Attribute` suffix, thus shortening the name to `Simple`:</span></span>

```vb
<Simple> Class Class1
End Class

<Simple> Interface Interface1
End Interface
```

<span data-ttu-id="ae363-161">上記の例は、次の意味と同等です。</span><span class="sxs-lookup"><span data-stu-id="ae363-161">The example above is semantically equivalent to the following:</span></span>

```vb
<SimpleAttribute> Class Class1
End Class

<SimpleAttribute> Interface Interface1
End Interface
```

<span data-ttu-id="ae363-162">一般に、サフィックスを持つ属性が名前付き`Attribute`を推奨します。</span><span class="sxs-lookup"><span data-stu-id="ae363-162">In general, attributes named with the suffix `Attribute` are preferred.</span></span> <span data-ttu-id="ae363-163">次の例では、という名前の 2 つの属性クラス`T`と`T``Attribute`します。</span><span class="sxs-lookup"><span data-stu-id="ae363-163">The following example shows two attribute classes named `T` and `T``Attribute`.</span></span>

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

<span data-ttu-id="ae363-164">属性ブロック`<T>`および属性ブロック`<TAttribute>`という名前の属性クラスを参照してください`TAttribute`します。</span><span class="sxs-lookup"><span data-stu-id="ae363-164">Both the attribute block `<T>` and the attribute block `<TAttribute>` refer to the attribute class named `TAttribute`.</span></span> <span data-ttu-id="ae363-165">使用することはできません`T`クラスの宣言を削除するまで、属性として`TAttribute`します。</span><span class="sxs-lookup"><span data-stu-id="ae363-165">It is not possible to use `T` as an attribute until you remove the declaration for class `TAttribute`.</span></span>

### <a name="attribute-arguments"></a><span data-ttu-id="ae363-166">属性の引数</span><span class="sxs-lookup"><span data-stu-id="ae363-166">Attribute Arguments</span></span>

<span data-ttu-id="ae363-167">属性に引数が 2 つの形式をかかる場合があります:*位置指定引数*と*変数/プロパティの初期化子をインスタンス*します。</span><span class="sxs-lookup"><span data-stu-id="ae363-167">Arguments to an attribute may take two forms: *positional arguments* and *instance variable/property initializers*.</span></span> <span data-ttu-id="ae363-168">属性に位置引数には、インスタンス変数/プロパティの初期化子が前にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="ae363-168">Any positional arguments to the attribute must precede the instance variable/property initializers.</span></span> <span data-ttu-id="ae363-169">位置指定引数は、定数式である 1 次元配列作成式で構成されますまたは`GetType`式。</span><span class="sxs-lookup"><span data-stu-id="ae363-169">A positional argument consists of a constant expression, a one-dimensional array-creation expression or a `GetType` expression.</span></span> <span data-ttu-id="ae363-170">インスタンス変数/プロパティの初期化子の後にコロンと等号、定数式で終了しているキーワードに一致する識別子から成るまたは`GetType`式。</span><span class="sxs-lookup"><span data-stu-id="ae363-170">An instance variable/property initializer consists of an identifier, which can match keywords, followed by a colon and equal sign, and terminated by a constant expression or a `GetType` expression.</span></span>

<span data-ttu-id="ae363-171">属性クラスを使用して属性を指定された`T`、位置指定引数リスト`P`、およびインスタンス変数/プロパティの初期化子リスト`N`、次の手順は、引数が有効かどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="ae363-171">Given an attribute with attribute class `T`, positional argument list `P`, and instance variable/property initializer list `N`, these steps determine whether the arguments are valid:</span></span>

1. <span data-ttu-id="ae363-172">形式の式をコンパイルするためのコンパイル時の処理手順に従います`New T(P)`します。</span><span class="sxs-lookup"><span data-stu-id="ae363-172">Follow the compile-time processing steps for compiling an expression of the form `New T(P)`.</span></span> <span data-ttu-id="ae363-173">これは、コンパイル時エラーが発生またはでコンス トラクターが決定`T`引数リストに最も適したです。</span><span class="sxs-lookup"><span data-stu-id="ae363-173">This either results in a compile-time error or determines a constructor on `T` that is most applicable to the argument list.</span></span>

2. <span data-ttu-id="ae363-174">ステップ 1 で特定のコンス トラクターでは、属性の型ではないパラメーターを持ちまたは宣言のサイトにアクセスできない、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ae363-174">If the constructor determined in step 1 has parameters that are not attribute types or is inaccessible at the declaration site, a compile-time error occurs.</span></span>

3. <span data-ttu-id="ae363-175">変数/プロパティ初期化子のインスタンスそれぞれ`Arg`で`N`、`Name`インスタンス変数/プロパティの初期化子の識別子である`Arg`します。</span><span class="sxs-lookup"><span data-stu-id="ae363-175">For each instance variable/property initializer `Arg` in `N`, let `Name` be the identifier of the instance variable/property initializer `Arg`.</span></span> <span data-ttu-id="ae363-176">`Name` 識別する必要があります以外`Shared`、書き込み可能な`Public`変数またはパラメーターなしのプロパティをインスタンス`T`型が属性の型。</span><span class="sxs-lookup"><span data-stu-id="ae363-176">`Name` must identify a non-`Shared`, writeable, `Public` instance variable or parameterless property on `T` whose type is an attribute type.</span></span> <span data-ttu-id="ae363-177">場合`T`がそのようなインスタンスの変数またはプロパティ、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ae363-177">If `T` has no such instance variable or property, a compile-time error occurs.</span></span>

<span data-ttu-id="ae363-178">例えば:</span><span class="sxs-lookup"><span data-stu-id="ae363-178">For example:</span></span>

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

<span data-ttu-id="ae363-179">型パラメーターは、属性の引数に任意の場所で使用できません。</span><span class="sxs-lookup"><span data-stu-id="ae363-179">Type parameters cannot be used anywhere in attribute arguments.</span></span> <span data-ttu-id="ae363-180">ただし、構築された型を使用できます。</span><span class="sxs-lookup"><span data-stu-id="ae363-180">However, constructed types may be used:</span></span>

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