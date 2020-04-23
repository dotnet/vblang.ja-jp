---
ms.openlocfilehash: 54e674bedd587647436b859423ab0f14715eca2d
ms.sourcegitcommit: 19ec79a287fb79180b05a0ad20e8291e75fc63df
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2020
ms.locfileid: "82080604"
---
# <a name="documentation-comments"></a><span data-ttu-id="9c2ab-101">ドキュメント コメント</span><span class="sxs-lookup"><span data-stu-id="9c2ab-101">Documentation Comments</span></span>

<span data-ttu-id="9c2ab-102">ドキュメントコメントは、ソース内の特別な形式のコメントで、ソースが添付されているコードに関するドキュメントを作成するために分析できます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-102">Documentation comments are specially formatted comments in the source that can be analyzed to produce documentation about the code they are attached to.</span></span> <span data-ttu-id="9c2ab-103">ドキュメント コメントの基本的な形式は XML です。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-103">The basic format for documentation comments is XML.</span></span> <span data-ttu-id="9c2ab-104">ドキュメント コメントを含むコードをコンパイルする場合、コンパイラは、ソース内のドキュメント コメントの合計を表す XML ファイルを任意で出力できます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-104">When the compiling code with documentation comments, the compiler may optionally emit an XML file that represents the sum total of the documentation comments in the source.</span></span> <span data-ttu-id="9c2ab-105">この XML ファイルは、印刷またはオンラインのドキュメントを作成する他のツールで使用できます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-105">This XML file can then be used by other tools to produce printed or online documentation.</span></span>

<span data-ttu-id="9c2ab-106">この章では、ドキュメントコメントと推奨 XML タグについて説明します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-106">This chapter describes document comments and recommended XML tags to use with document comments.</span></span>

## <a name="documentation-comment-format"></a><span data-ttu-id="9c2ab-107">ドキュメント コメント形式</span><span class="sxs-lookup"><span data-stu-id="9c2ab-107">Documentation Comment Format</span></span>

<span data-ttu-id="9c2ab-108">ドキュメントコメントは、3 つの単`'''`一引用符で始まる特別なコメントです。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-108">Document comments are special comments that begin with `'''`, three single quote marks.</span></span> <span data-ttu-id="9c2ab-109">これらは、クラス、デリゲート、インターフェイスなどの型メンバまたは型メンバ (フィールド、イベント、プロパティ、メソッドなど) の直前に記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-109">They must immediately precede the type (such as a class, delegate, or interface) or type member (such as a field, event, property, or method) that they document.</span></span> <span data-ttu-id="9c2ab-110">部分メソッド宣言に対するドキュメント コメントは、その本体を提供するメソッドのドキュメント コメント (存在する場合) に置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-110">A document comment on a partial method declaration will be replaced by the document comment on the method that supplies its body, if there is one.</span></span> <span data-ttu-id="9c2ab-111">隣接するすべてのドキュメント コメントが追加され、1 つのドキュメント コメントが生成されます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-111">All adjacent document comments are appended together to produce a single document comment.</span></span> <span data-ttu-id="9c2ab-112">`'''`文字の後に空白文字がある場合、その空白文字は連結に含まれません。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-112">If there is a whitespace character following the `'''` characters, then that whitespace character is not included in the concatenation.</span></span> <span data-ttu-id="9c2ab-113">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-113">For example:</span></span>

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

<span data-ttu-id="9c2ab-114">ドキュメント コメントは、 に従ってhttps://www.w3.org/TR/REC-xmlXML の形式が正しい必要があります。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-114">Documentation comments must be well formed XML according to https://www.w3.org/TR/REC-xml.</span></span> <span data-ttu-id="9c2ab-115">XML の形式が正しくない場合は警告が生成され、エラーが発生したことを示すコメントがドキュメント ファイルに含まれます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-115">If the XML is not well formed, a warning is generated and the documentation file will contain a comment saying that an error was encountered.</span></span>

<span data-ttu-id="9c2ab-116">開発者は独自のタグセットを自由に作成できますが、次のセクションで推奨されるセットを定義します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-116">Although developers are free to create their own set of tags, a recommended set is defined in the next section.</span></span> <span data-ttu-id="9c2ab-117">推奨されるタグの一部には特別な意味があります。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-117">Some of the recommended tags have special meanings:</span></span>

* <span data-ttu-id="9c2ab-118">この`<param>`タグは、パラメータを記述するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-118">The `<param>` tag is used to describe parameters.</span></span> <span data-ttu-id="9c2ab-119">`<param>`タグで指定するパラメーターが存在し、型メンバーのすべてのパラメーターがドキュメント コメントに記述されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-119">The parameter specified by a `<param>` tag must exist and all parameters of the type member must be described in the documentation comment.</span></span> <span data-ttu-id="9c2ab-120">いずれかの条件が真でない場合、コンパイラは警告を発行します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-120">If either condition is not true, the compiler issues a warning.</span></span>

* <span data-ttu-id="9c2ab-121">`cref` 属性は任意のタグにアタッチでき、コード要素への参照を提供します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-121">The `cref` attribute can be attached to any tag to provide a reference to a code element.</span></span> <span data-ttu-id="9c2ab-122">コード要素が存在する必要があります。コンパイル時に、コンパイラは名前をメンバーを表す ID 文字列に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-122">The code element must exist; at compile-time the compiler replaces the name with the ID string representing the member.</span></span> <span data-ttu-id="9c2ab-123">コード要素が存在しない場合、コンパイラは警告を発行します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-123">If the code element does not exist, the compiler issues a warning.</span></span> <span data-ttu-id="9c2ab-124">`cref`属性で記述されている名前を検索する場合、コンパイラは、含`Imports`まれているソース ファイル内に記述されているステートメントを尊重します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-124">When looking for a name described in a `cref` attribute, the compiler respects `Imports` statements that appear within the containing source file.</span></span>

* <span data-ttu-id="9c2ab-125">タグ`<summary>`は、型またはメンバーに関する追加情報を表示するために、ドキュメント ビューアーで使用されます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-125">The `<summary>` tag is intended to be used by a documentation viewer to display additional information about a type or member.</span></span>

<span data-ttu-id="9c2ab-126">ドキュメント ファイルには、型とメンバーに関する完全な情報は提供されず、ドキュメント コメントに含まれている内容のみが提供されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-126">Note that the documentation file does not provide full information about a type and members, only what is contained in the document comments.</span></span> <span data-ttu-id="9c2ab-127">型またはメンバーに関する詳細情報を取得するには、ドキュメント ファイルを実際の型またはメンバーのリフレクションと組み合わせて使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-127">To get more information about a type or member, the documentation file must be used in conjunction with reflection on the actual type or member.</span></span>

## <a name="recommended-tags"></a><span data-ttu-id="9c2ab-128">推奨タグ</span><span class="sxs-lookup"><span data-stu-id="9c2ab-128">Recommended tags</span></span>

<span data-ttu-id="9c2ab-129">ドキュメント ジェネレータは、XML の規則に従って有効なタグを受け入れ、処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-129">The documentation generator must accept and process any tag that is valid according to the rules of XML.</span></span> <span data-ttu-id="9c2ab-130">次のタグによって、ユーザー ドキュメントで一般的に使用される機能が与えられます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-130">The following tags provide commonly used functionality in user documentation:</span></span>

<span data-ttu-id="9c2ab-131">`<c>`コードに似るフォントでテキストを設定する</span><span class="sxs-lookup"><span data-stu-id="9c2ab-131">`<c>` Sets text in a code-like font</span></span>

<span data-ttu-id="9c2ab-132">`<code>`コードに似るフォントで 1 行以上のソース コードまたはプログラム出力を設定します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-132">`<code>` Sets one or more lines of source code or program output in a code-like font</span></span>

<span data-ttu-id="9c2ab-133">`<example>`例を示します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-133">`<example>` Indicates an example</span></span>

<span data-ttu-id="9c2ab-134">`<exception>`メソッドがスローできる例外を識別します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-134">`<exception>` Identifies the exceptions a method can throw</span></span>

<span data-ttu-id="9c2ab-135">`<include>`外部 XML ドキュメントを含む</span><span class="sxs-lookup"><span data-stu-id="9c2ab-135">`<include>` Includes an external XML document</span></span>

<span data-ttu-id="9c2ab-136">`<list>`リストまたはテーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-136">`<list>` Creates a list or table</span></span>

<span data-ttu-id="9c2ab-137">`<para>`テキストに構造を追加することを許可します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-137">`<para>` Permits structure to be added to text</span></span>

<span data-ttu-id="9c2ab-138">`<param>`メソッドまたはコンストラクターのパラメーターを記述します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-138">`<param>` Describes a parameter for a method or constructor</span></span>

<span data-ttu-id="9c2ab-139">`<paramref>`単語がパラメータ名であることを識別します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-139">`<paramref>` Identifies that a word is a parameter name</span></span>

<span data-ttu-id="9c2ab-140">`<permission>`メンバーのセキュリティアクセシビリティを文書化します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-140">`<permission>` Documents the security accessibility of a member</span></span>

<span data-ttu-id="9c2ab-141">`<remarks>`型について説明します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-141">`<remarks>` Describes a type</span></span>

<span data-ttu-id="9c2ab-142">`<returns>`メソッドの戻り値を記述します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-142">`<returns>` Describes the return value of a method</span></span>

<span data-ttu-id="9c2ab-143">`<see>`リンクを指定します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-143">`<see>` Specifies a link</span></span>

<span data-ttu-id="9c2ab-144">`<seealso>`参照項目を生成します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-144">`<seealso>` Generates a See Also entry</span></span>

<span data-ttu-id="9c2ab-145">`<summary>`型のメンバーについて説明します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-145">`<summary>` Describes a member of a type</span></span>

<span data-ttu-id="9c2ab-146">`<typeparam>`型パラメーターについて説明します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-146">`<typeparam>` Describes a type parameter</span></span>

<span data-ttu-id="9c2ab-147">`<value>`プロパティについて説明します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-147">`<value>` Describes a property</span></span>

### <a name="ltcgt"></a><span data-ttu-id="9c2ab-148">&lt;c&gt;</span><span class="sxs-lookup"><span data-stu-id="9c2ab-148">&lt;c&gt;</span></span>

<span data-ttu-id="9c2ab-149">このタグは、説明内のテキストの断片がコードブロックに使用されるようなフォントを使用することを指定します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-149">This tag specifies that a fragment of text within a description should use a font like that used for a block of code.</span></span> <span data-ttu-id="9c2ab-150">(実際のコード行の場合は`<code>`、を使用します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-150">(For lines of actual code, use `<code>`.)</span></span>

<span data-ttu-id="9c2ab-151">__構文 :__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-151">__Syntax:__</span></span>

```xml
<c>text to be set like code</c>
```

<span data-ttu-id="9c2ab-152">__例:__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-152">__Example:__</span></span>

```vb
''' <remarks>
''' Class <c>Point</c> models a point in a two-dimensional plane.
''' </remarks>
Public Class Point 
End Class
```

### <a name="ltcodegt"></a><span data-ttu-id="9c2ab-153">&lt;code&gt;</span><span class="sxs-lookup"><span data-stu-id="9c2ab-153">&lt;code&gt;</span></span>

<span data-ttu-id="9c2ab-154">このタグは、1 行以上のソース コードまたはプログラム出力で固定幅フォントを使用することを指定します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-154">This tag specifies that one or more lines of source code or program output should use a fixed-width font.</span></span> <span data-ttu-id="9c2ab-155">(小さなコードフラグメントの場合は`<c>`、.を使用してください)。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-155">(For small code fragments, use `<c>`.)</span></span>

<span data-ttu-id="9c2ab-156">__構文 :__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-156">__Syntax:__</span></span>

```xml
<code>source code or program output</code>
```

<span data-ttu-id="9c2ab-157">__例:__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-157">__Example:__</span></span>

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

### <a name="ltexamplegt"></a><span data-ttu-id="9c2ab-158">&lt;example&gt;</span><span class="sxs-lookup"><span data-stu-id="9c2ab-158">&lt;example&gt;</span></span>

<span data-ttu-id="9c2ab-159">このタグを使用すると、コメント内のコード例を使用して、要素の使用方法を示すことができます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-159">This tag allows example code within a comment to show how an element can be used.</span></span> <span data-ttu-id="9c2ab-160">通常、これにはタグ`<code>`の使用も含まれます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-160">Ordinarily, this will involve use of the tag `<code>` as well.</span></span>

<span data-ttu-id="9c2ab-161">__構文 :__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-161">__Syntax:__</span></span>

```xml
<example>description</example>
```

<span data-ttu-id="9c2ab-162">__例:__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-162">__Example:__</span></span>

<span data-ttu-id="9c2ab-163">例については、「`<code>`」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-163">See `<code>` for an example.</span></span>

### <a name="ltexceptiongt"></a><span data-ttu-id="9c2ab-164">&lt;exception&gt;</span><span class="sxs-lookup"><span data-stu-id="9c2ab-164">&lt;exception&gt;</span></span>

<span data-ttu-id="9c2ab-165">このタグは、メソッドがスローできる例外を文書化する方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-165">This tag provides a way to document the exceptions a method can throw.</span></span>

<span data-ttu-id="9c2ab-166">__構文 :__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-166">__Syntax:__</span></span>

```xml
<exception cref="member">description</exception>
```

<span data-ttu-id="9c2ab-167">__例:__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-167">__Example:__</span></span>

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

### <a name="ltincludegt"></a><span data-ttu-id="9c2ab-168">&lt;include&gt;</span><span class="sxs-lookup"><span data-stu-id="9c2ab-168">&lt;include&gt;</span></span>

<span data-ttu-id="9c2ab-169">このタグは、外部の整形式 XML ドキュメントからの情報を含めるために使用されます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-169">This tag is used to include information from an external well-formed XML document.</span></span> <span data-ttu-id="9c2ab-170">Xml ドキュメントに XPath 式を適用して、ドキュメントからどの XML をインクルードするかを指定します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-170">An XPath expression is applied to the XML document to specify what XML should be included from the document.</span></span> <span data-ttu-id="9c2ab-171">タグ`<include>`は、外部ドキュメントから選択した XML に置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-171">The `<include>` tag is then replaced with the selected XML from the external document.</span></span>

<span data-ttu-id="9c2ab-172">__構文 :__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-172">__Syntax:__</span></span>

```xml
<include file="filename" path="xpath">
```

<span data-ttu-id="9c2ab-173">__例:__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-173">__Example:__</span></span>

<span data-ttu-id="9c2ab-174">ソース コードに次のような宣言が含まれている場合:</span><span class="sxs-lookup"><span data-stu-id="9c2ab-174">If the source code contained a declaration like the following:</span></span>

```vb
''' <include file="docs.xml" path="extra/class[@name="IntList"]/*" />
```

<span data-ttu-id="9c2ab-175">外部ファイル docs.xml には次の内容があります</span><span class="sxs-lookup"><span data-stu-id="9c2ab-175">and the external file docs.xml had the following contents</span></span>

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

<span data-ttu-id="9c2ab-176">その後、ソース コードに含まれている場合と同じドキュメントが出力されます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-176">then the same documentation is output as if the source code contained:</span></span>

```xml
''' <summary>
''' Contains a list of integers.
''' </summary>
```

### <a name="ltlistgt"></a><span data-ttu-id="9c2ab-177">&lt;list&gt;</span><span class="sxs-lookup"><span data-stu-id="9c2ab-177">&lt;list&gt;</span></span>

<span data-ttu-id="9c2ab-178">このタグは、項目のリストまたはテーブルを作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-178">This tag is used to create a list or table of items.</span></span> <span data-ttu-id="9c2ab-179">テーブルまたは定義リスト`<listheader>`の見出し行を定義するブロックを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-179">It may contain a `<listheader>` block to define the heading row of either a table or definition list.</span></span> <span data-ttu-id="9c2ab-180">(テーブルを定義する場合は、見出しに用語のエントリのみを指定する必要があります)。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-180">(When defining a table, only an entry for term in the heading need be supplied.)</span></span>

<span data-ttu-id="9c2ab-181">リスト内の各項目はブロックで`<item>`指定されます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-181">Each item in the list is specified with an `<item>` block.</span></span> <span data-ttu-id="9c2ab-182">定義リストを作成する場合は、用語と説明の両方を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-182">When creating a definition list, both term and description must be specified.</span></span> <span data-ttu-id="9c2ab-183">ただし、表、箇条書きリスト、または番号付きリストの場合は、説明のみを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-183">However, for a table, bulleted list, or numbered list, only description need be specified.</span></span>

<span data-ttu-id="9c2ab-184">__構文 :__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-184">__Syntax:__</span></span>

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

<span data-ttu-id="9c2ab-185">__例:__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-185">__Example:__</span></span>

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

### <a name="ltparagt"></a><span data-ttu-id="9c2ab-186">&lt;para&gt;</span><span class="sxs-lookup"><span data-stu-id="9c2ab-186">&lt;para&gt;</span></span>

<span data-ttu-id="9c2ab-187">このタグは、 や`<remarks>``<returns>`などの他のタグの内部で使用するためのもので、構造をテキストに追加できます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-187">This tag is for use inside other tags, such as `<remarks>` or `<returns>`, and permits structure to be added to text.</span></span>

<span data-ttu-id="9c2ab-188">__構文 :__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-188">__Syntax:__</span></span>

```xml
<para>content</para>
```

<span data-ttu-id="9c2ab-189">__例:__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-189">__Example:__</span></span>

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

### <a name="ltparamgt"></a><span data-ttu-id="9c2ab-190">&lt;param&gt;</span><span class="sxs-lookup"><span data-stu-id="9c2ab-190">&lt;param&gt;</span></span>

<span data-ttu-id="9c2ab-191">このタグは、メソッド、コンストラクター、またはインデックス付きプロパティのパラメーターを表します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-191">This tag describes a parameter for a method, constructor, or indexed property.</span></span>

<span data-ttu-id="9c2ab-192">__構文 :__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-192">__Syntax:__</span></span>

```xml
<param name="name">description</param>
```

<span data-ttu-id="9c2ab-193">__例:__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-193">__Example:__</span></span>

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

### <a name="ltparamrefgt"></a><span data-ttu-id="9c2ab-194">&lt;paramref&gt;</span><span class="sxs-lookup"><span data-stu-id="9c2ab-194">&lt;paramref&gt;</span></span>

<span data-ttu-id="9c2ab-195">このタグは、単語がパラメータであることを示します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-195">This tag indicates that a word is a parameter.</span></span> <span data-ttu-id="9c2ab-196">ドキュメント ファイルは、このパラメーターを何らかの方法でフォーマットするために処理できます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-196">The documentation file can be processed to format this parameter in some distinct way.</span></span>

<span data-ttu-id="9c2ab-197">__構文 :__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-197">__Syntax:__</span></span>

```xml
<paramref name="name"/>
```

<span data-ttu-id="9c2ab-198">__例:__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-198">__Example:__</span></span>

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

### <a name="ltpermissiongt"></a><span data-ttu-id="9c2ab-199">&lt;permission&gt;</span><span class="sxs-lookup"><span data-stu-id="9c2ab-199">&lt;permission&gt;</span></span>

<span data-ttu-id="9c2ab-200">このタグは、メンバーのセキュリティアクセシビリティを示します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-200">This tag documents the security accessibility of a member</span></span>

<span data-ttu-id="9c2ab-201">__構文 :__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-201">__Syntax:__</span></span>

```xml
<permission cref="member">description</permission>
```

<span data-ttu-id="9c2ab-202">__例:__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-202">__Example:__</span></span>

```vb
''' <permission cref="System.Security.PermissionSet">Everyone can
''' access this method.</permission>
Public Shared Sub Test()
End Sub
```

### <a name="ltremarksgt"></a><span data-ttu-id="9c2ab-203">&lt;remarks&gt;</span><span class="sxs-lookup"><span data-stu-id="9c2ab-203">&lt;remarks&gt;</span></span>

<span data-ttu-id="9c2ab-204">このタグは、型に関する概要情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-204">This tag specifies overview information about a type.</span></span> <span data-ttu-id="9c2ab-205">(型`<summary>`のメンバーを記述するために使用します)。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-205">(Use `<summary>` to describe the members of a type.)</span></span>

<span data-ttu-id="9c2ab-206">__構文 :__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-206">__Syntax:__</span></span>

```xml
<remarks>description</remarks>
```

<span data-ttu-id="9c2ab-207">__例:__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-207">__Example:__</span></span>

```vb
''' <remarks>
''' Class <c>Point</c> models a point in a two-dimensional plane.
''' </remarks>
Public Class Point 
End Class
```

### <a name="ltreturnsgt"></a><span data-ttu-id="9c2ab-208">&lt;returns&gt;</span><span class="sxs-lookup"><span data-stu-id="9c2ab-208">&lt;returns&gt;</span></span>

<span data-ttu-id="9c2ab-209">このタグは、メソッドの戻り値を表します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-209">This tag describes the return value of a method.</span></span>

<span data-ttu-id="9c2ab-210">__構文 :__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-210">__Syntax:__</span></span>

```xml
<returns>description</returns>
```

<span data-ttu-id="9c2ab-211">__例:__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-211">__Example:__</span></span>

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

### <a name="ltseegt"></a><span data-ttu-id="9c2ab-212">&lt;see&gt;</span><span class="sxs-lookup"><span data-stu-id="9c2ab-212">&lt;see&gt;</span></span>

<span data-ttu-id="9c2ab-213">このタグを使用すると、テキスト内でリンクを指定できます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-213">This tag allows a link to be specified within text.</span></span> <span data-ttu-id="9c2ab-214">(参照`<seealso>`セクションに表示されるテキストを指定するために使用します)。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-214">(Use `<seealso>` to indicate text that is to appear in a See Also section.)</span></span>

<span data-ttu-id="9c2ab-215">__構文 :__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-215">__Syntax:__</span></span>

```xml
<see cref="member"/>
```

<span data-ttu-id="9c2ab-216">__例:__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-216">__Example:__</span></span>

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

### <a name="ltseealsogt"></a><span data-ttu-id="9c2ab-217">&lt;seealso&gt;</span><span class="sxs-lookup"><span data-stu-id="9c2ab-217">&lt;seealso&gt;</span></span>

<span data-ttu-id="9c2ab-218">このタグは、[参照] セクションのエントリを生成します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-218">This tag generates an entry for the See Also section.</span></span> <span data-ttu-id="9c2ab-219">(テキスト`<see>`内からリンクを指定するために使用します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-219">(Use `<see>` to specify a link from within text.)</span></span>

<span data-ttu-id="9c2ab-220">__構文 :__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-220">__Syntax:__</span></span>

```xml
<seealso cref="member"/>
```

<span data-ttu-id="9c2ab-221">__例:__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-221">__Example:__</span></span>

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

### <a name="ltsummarygt"></a><span data-ttu-id="9c2ab-222">&lt;summary&gt;</span><span class="sxs-lookup"><span data-stu-id="9c2ab-222">&lt;summary&gt;</span></span>

<span data-ttu-id="9c2ab-223">このタグは、型メンバーを表します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-223">This tag describes a type member.</span></span> <span data-ttu-id="9c2ab-224">(型`<remarks>`自体を記述するために使用します)。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-224">(Use `<remarks>` to describe a type itself.)</span></span>

<span data-ttu-id="9c2ab-225">__構文 :__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-225">__Syntax:__</span></span>

```xml
<summary>description</summary>
```

<span data-ttu-id="9c2ab-226">__例:__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-226">__Example:__</span></span>

```vb
''' <summary>
''' This constructor initializes the new Point to (0,0).
''' </summary>
Public Sub New()
    Me.New(0,0)
End Sub
```

### <a name="lttypeparamgt"></a><span data-ttu-id="9c2ab-227">&lt;typeparam&gt;</span><span class="sxs-lookup"><span data-stu-id="9c2ab-227">&lt;typeparam&gt;</span></span>

<span data-ttu-id="9c2ab-228">このタグは、型パラメーターを表します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-228">This tag describes a type parameter.</span></span>

<span data-ttu-id="9c2ab-229">__構文 :__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-229">__Syntax:__</span></span>

```xml
<typeparam name="name">description</typeparam>
```

<span data-ttu-id="9c2ab-230">__例:__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-230">__Example:__</span></span>

```vb
''' <typeparam name="T">
''' The base item type. Must implement IComparable.
''' </typeparam>
Public Class ItemManager(Of T As IComparable)
End Class
```

### <a name="ltvaluegt"></a><span data-ttu-id="9c2ab-231">&lt;value&gt;</span><span class="sxs-lookup"><span data-stu-id="9c2ab-231">&lt;value&gt;</span></span>

<span data-ttu-id="9c2ab-232">このタグは、プロパティを記述します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-232">This tag describes a property.</span></span>

<span data-ttu-id="9c2ab-233">__構文 :__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-233">__Syntax:__</span></span>

```xml
<value>property description</value>
```

<span data-ttu-id="9c2ab-234">__例:__</span><span class="sxs-lookup"><span data-stu-id="9c2ab-234">__Example:__</span></span>

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

## <a name="id-strings"></a><span data-ttu-id="9c2ab-235">ID 文字列</span><span class="sxs-lookup"><span data-stu-id="9c2ab-235">ID Strings</span></span>

<span data-ttu-id="9c2ab-236">ドキュメント ファイルを生成するときに、コンパイラは、ソース コード内の要素ごとに ID 文字列を生成し、その要素を一意に識別するドキュメント コメントでタグ付けします。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-236">When generating the documentation file, the compiler generates an ID string for each element in the source code that is tagged with a documentation comment that uniquely identifies it.</span></span> <span data-ttu-id="9c2ab-237">この ID 文字列は、外部ツールで使用して、コンパイル済みアセンブリ内のどの要素がドキュメント コメントに対応するかを識別できます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-237">This ID string can be used by external tools to identify which element in a compiled assembly corresponds to the document comment.</span></span>

<span data-ttu-id="9c2ab-238">ID 文字列は次のように生成されます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-238">ID strings are generated as follows:</span></span>

<span data-ttu-id="9c2ab-239">文字列に空白は配置されません。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-239">No white space is placed in the string.</span></span>

<span data-ttu-id="9c2ab-240">文字列の最初の部分は、1 文字の後にコロンを続けて、文書化されているメンバーの種類を識別します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-240">The first part of the string identifies the kind of member being documented, via a single character followed by a colon.</span></span> <span data-ttu-id="9c2ab-241">イベント (E)、フィールド (F)、コンストラクターと演算子 (M)、名前空間 (N)、プロパティ (P)、および型 (T) を含むメソッドの後に、対応する文字をかっこで囲んで、次の種類のメンバーが定義されます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-241">The following kinds of members are defined, with the corresponding character in parenthesis after it: events (E), fields (F), methods including constructors and operators (M), namespaces (N), properties (P) and types (T).</span></span> <span data-ttu-id="9c2ab-242">感嘆符 (!) は ID 文字列の生成中にエラーが発生したことを示し、残りの文字列はエラーに関する情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-242">An exclamation point (!) indicates an error occurred while generating the ID string, and the rest of the string provides information about the error.</span></span>

<span data-ttu-id="9c2ab-243">文字列の 2 番目の部分は、グローバル名前空間から始まる要素の完全修飾名です。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-243">The second part of the string is the fully qualified name of the element, starting at the global namespace.</span></span> <span data-ttu-id="9c2ab-244">要素の名前、その外側の型、および名前空間はピリオドで区切られます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-244">The name of the element, its enclosing type(s), and namespace are separated by periods.</span></span> <span data-ttu-id="9c2ab-245">項目自体の名前にピリオドがある場合は、シャープ記号 (#) に置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-245">If the name of the item itself has periods, they are replaced by the pound sign (#).</span></span> <span data-ttu-id="9c2ab-246">(この名前にこの文字を持つ要素は存在しないものと見なされます。型パラメーターを持つ型の名前は、後ろに戻り引用符 (') の後に型の型パラメーターの数を表す数値で終わります。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-246">(It is assumed that no element has this character in its name.) The name of a type with type parameters ends with a backquote (\`) followed by a number that represents the number of type parameters on the type.</span></span> <span data-ttu-id="9c2ab-247">入れ子になった型は、それらを含む型の型パラメーターにアクセスできるため、入れ子になった型には、その型の型パラメーターが暗黙的に含まれ、その型は型パラメーターの合計にカウントされます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-247">It is important to remember that because nested types have access to the type parameters of the types containing them, nested types implicitly contain the type parameters of their containing types, and those types are counted in their type parameter totals in this case.</span></span>

<span data-ttu-id="9c2ab-248">引数を持つメソッドとプロパティの場合、引数リストはかっこで囲まれて次のようになります。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-248">For methods and properties with arguments, the argument list follows, enclosed in parentheses.</span></span> <span data-ttu-id="9c2ab-249">引数を持たない場合、括弧は省略されます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-249">For those without arguments, the parentheses are omitted.</span></span> <span data-ttu-id="9c2ab-250">引数はコンマで区切られます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-250">The arguments are separated by commas.</span></span> <span data-ttu-id="9c2ab-251">各引数のエンコーディングは、次のように CLI シグネチャと同じです。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-251">The encoding of each argument is the same as a CLI signature, as follows: Arguments are represented by their fully qualified name.</span></span> <span data-ttu-id="9c2ab-252">`Integer`たとえば、、、、、、、、、`System.Int32``String``System.String``Object`などになります`System.Object`。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-252">For example, `Integer` becomes `System.Int32`, `String` becomes `System.String`, `Object` becomes `System.Object`, and so on.</span></span> <span data-ttu-id="9c2ab-253">修飾子を`ByRef`持つ引数の型名の後には '@' が付きます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-253">Arguments having the `ByRef` modifier have a '@' following their type name.</span></span> <span data-ttu-id="9c2ab-254">修飾子または`ParamArray`修飾子`ByVal``Optional`を持つ引数には、特別な表記はありません。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-254">Arguments having the `ByVal`, `Optional` or `ParamArray` modifier have no special notation.</span></span> <span data-ttu-id="9c2ab-255">配列である引数は、コンマの数`[lowerbound:size, ..., lowerbound:size]`がランク - 1 で、各次元の下限とサイズが分かっている場合は 10 進数で表されます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-255">Arguments that are arrays are represented as `[lowerbound:size, ..., lowerbound:size]` where the number of commas is the rank - 1, and the lower bounds and size of each dimension, if known, are represented in decimal.</span></span> <span data-ttu-id="9c2ab-256">下限またはサイズが指定されていない場合は、省略されます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-256">If a lower bound or size is not specified, it is omitted.</span></span> <span data-ttu-id="9c2ab-257">特定の次元で下限およびサイズが省略されている場合は、':' も省略されます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-257">If the lower bound and size for a particular dimension are omitted, the ':' is omitted as well.</span></span> <span data-ttu-id="9c2ab-258">配列の配列は、レベルごとに 1`[]`つの " " で表されます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-258">Arrays of arrays are represented by one "`[]`" per level.</span></span>

### <a name="id-string-examples"></a><span data-ttu-id="9c2ab-259">ID 文字列の例</span><span class="sxs-lookup"><span data-stu-id="9c2ab-259">ID string examples</span></span>

<span data-ttu-id="9c2ab-260">次の例は、VB コードのフラグメントと、ドキュメント コメントを持つ各ソース要素から生成された ID 文字列を示しています。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-260">The following examples each show a fragment of VB code, along with the ID string produced from each source element capable of having a documentation comment:</span></span>

<span data-ttu-id="9c2ab-261">型は完全修飾名を使用して表されます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-261">Types are represented using their fully qualified name.</span></span>

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

<span data-ttu-id="9c2ab-262">フィールドは、完全修飾名で表されます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-262">Fields are represented by their fully qualified name.</span></span>

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

<span data-ttu-id="9c2ab-263">コンストラクター。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-263">Constructors.</span></span>

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

<span data-ttu-id="9c2ab-264">メソッド。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-264">Methods.</span></span>

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

<span data-ttu-id="9c2ab-265">プロパティ。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-265">Properties.</span></span>

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

<span data-ttu-id="9c2ab-266">イベント</span><span class="sxs-lookup"><span data-stu-id="9c2ab-266">Events</span></span>   

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

<span data-ttu-id="9c2ab-267">演算子。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-267">Operators.</span></span>

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

<span data-ttu-id="9c2ab-268">変換演算子には、末尾`~`に戻り値の型が続きます。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-268">Conversion operators have a trailing `~` followed by the return type.</span></span>

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

## <a name="documentation-comments-example"></a><span data-ttu-id="9c2ab-269">ドキュメント コメントの例</span><span class="sxs-lookup"><span data-stu-id="9c2ab-269">Documentation comments example</span></span>

<span data-ttu-id="9c2ab-270">次の例は、クラスのソース`Point`コードを示しています。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-270">The following example shows the source code of a `Point` class:</span></span>

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

<span data-ttu-id="9c2ab-271">上に示した class`Point`のソース コードを指定したときに生成される出力を次に示します。</span><span class="sxs-lookup"><span data-stu-id="9c2ab-271">Here is the output produced when given the source code for class `Point`, shown above:</span></span>

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
