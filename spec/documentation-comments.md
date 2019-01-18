---
ms.openlocfilehash: 8a36506d9fce1605cf3758536f51782ea7680e84
ms.sourcegitcommit: 6eca149bdc736113e0adb709212bd266c9503c33
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2019
ms.locfileid: "47426726"
---
# <a name="documentation-comments"></a><span data-ttu-id="81687-101">ドキュメントのコメント</span><span class="sxs-lookup"><span data-stu-id="81687-101">Documentation Comments</span></span>

<span data-ttu-id="81687-102">ドキュメントのコメントは、特殊な形式のコメントは接続されているコードに関するドキュメントを生成するために分析できるソースです。</span><span class="sxs-lookup"><span data-stu-id="81687-102">Documentation comments are specially formatted comments in the source that can be analyzed to produce documentation about the code they are attached to.</span></span> <span data-ttu-id="81687-103">ドキュメントのコメントの基本的な形式は、XML です。</span><span class="sxs-lookup"><span data-stu-id="81687-103">The basic format for documentation comments is XML.</span></span> <span data-ttu-id="81687-104">ときにドキュメントのコメントを使用してコンパイル コードをコンパイラ可能性があります必要に応じて出力ソース ドキュメントのコメントの合計を表す XML ファイル。</span><span class="sxs-lookup"><span data-stu-id="81687-104">When the compiling code with documentation comments, the compiler may optionally emit an XML file that represents the sum total of the documentation comments in the source.</span></span> <span data-ttu-id="81687-105">この XML ファイルは、印刷物やオンライン ドキュメントを生成するために他のツールで使用できます。</span><span class="sxs-lookup"><span data-stu-id="81687-105">This XML file can then be used by other tools to produce printed or online documentation.</span></span>

<span data-ttu-id="81687-106">この章では、ドキュメントのコメントを記述し、XML タグを使用したドキュメントのコメントをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="81687-106">This chapter describes document comments and recommended XML tags to use with document comments.</span></span>

## <a name="documentation-comment-format"></a><span data-ttu-id="81687-107">ドキュメント コメント形式</span><span class="sxs-lookup"><span data-stu-id="81687-107">Documentation Comment Format</span></span>

<span data-ttu-id="81687-108">ドキュメント コメントは、特殊なコメントで始まる`'''`、3 つのマークで一重引用符。</span><span class="sxs-lookup"><span data-stu-id="81687-108">Document comments are special comments that begin with `'''`, three single quote marks.</span></span> <span data-ttu-id="81687-109">直前に、ドキュメント化 (クラス、デリゲート、またはインターフェイス) などの型または型のメンバー (フィールド、イベント、プロパティ、メソッドなど) を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="81687-109">They must immediately precede the type (such as a class, delegate, or interface) or type member (such as a field, event, property, or method) that they document.</span></span> <span data-ttu-id="81687-110">1 つを使用する必要がある場合、その本文を提供するメソッドのドキュメント コメントで部分メソッド宣言でドキュメントのコメントが置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="81687-110">A document comment on a partial method declaration will be replaced by the document comment on the method that supplies its body, if there is one.</span></span> <span data-ttu-id="81687-111">すべての隣接するドキュメントのコメントは、1 つのドキュメントのコメントを生成するために一緒に追加されます。</span><span class="sxs-lookup"><span data-stu-id="81687-111">All adjacent document comments are appended together to produce a single document comment.</span></span> <span data-ttu-id="81687-112">続く、空白文字がある場合、`'''`文字、連結したものにその空白文字は含まれません。</span><span class="sxs-lookup"><span data-stu-id="81687-112">If there is a whitespace character following the `'''` characters, then that whitespace character is not included in the concatenation.</span></span> <span data-ttu-id="81687-113">例:</span><span class="sxs-lookup"><span data-stu-id="81687-113">For example:</span></span>

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

<span data-ttu-id="81687-114">ドキュメントのコメントをする必要がありますが、整形式 XML に従って http://www.w3.org/TR/REC-xmlします。</span><span class="sxs-lookup"><span data-stu-id="81687-114">Documentation comments must be well formed XML according to http://www.w3.org/TR/REC-xml.</span></span> <span data-ttu-id="81687-115">XML の形式が正しくない場合、は、警告が生成され、ドキュメント ファイルは、エラーが発生したことを示すコメントが含まれます。</span><span class="sxs-lookup"><span data-stu-id="81687-115">If the XML is not well formed, a warning is generated and the documentation file will contain a comment saying that an error was encountered.</span></span>

<span data-ttu-id="81687-116">開発者は、独自のタグのセットを作成できますが、推奨される設定は、次のセクションで定義されます。</span><span class="sxs-lookup"><span data-stu-id="81687-116">Although developers are free to create their own set of tags, a recommended set is defined in the next section.</span></span> <span data-ttu-id="81687-117">推奨されるタグの一部には特別な意味があります。</span><span class="sxs-lookup"><span data-stu-id="81687-117">Some of the recommended tags have special meanings:</span></span>

* <span data-ttu-id="81687-118">`<param>`タグを使用して、パラメーターを説明します。</span><span class="sxs-lookup"><span data-stu-id="81687-118">The `<param>` tag is used to describe parameters.</span></span> <span data-ttu-id="81687-119">指定されたパラメーターを`<param>`タグが存在し、型のメンバーのすべてのパラメーターは、ドキュメント コメントで説明する必要があります。</span><span class="sxs-lookup"><span data-stu-id="81687-119">The parameter specified by a `<param>` tag must exist and all parameters of the type member must be described in the documentation comment.</span></span> <span data-ttu-id="81687-120">いずれかの条件が true でない場合、コンパイラは警告を発行します。</span><span class="sxs-lookup"><span data-stu-id="81687-120">If either condition is not true, the compiler issues a warning.</span></span>

* <span data-ttu-id="81687-121">`cref` 属性は任意のタグにアタッチでき、コード要素への参照を提供します。</span><span class="sxs-lookup"><span data-stu-id="81687-121">The `cref` attribute can be attached to any tag to provide a reference to a code element.</span></span> <span data-ttu-id="81687-122">コード要素が存在する必要があります。コンパイル時に、コンパイラは、名前をメンバーを表す ID 文字列に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="81687-122">The code element must exist; at compile-time the compiler replaces the name with the ID string representing the member.</span></span> <span data-ttu-id="81687-123">コード要素が存在しない場合、コンパイラは警告を発行します。</span><span class="sxs-lookup"><span data-stu-id="81687-123">If the code element does not exist, the compiler issues a warning.</span></span> <span data-ttu-id="81687-124">説明されている名前を検索するときに、`cref`属性は、コンパイラの点では`Imports`ステートメントを含むソース ファイル内に表示されます。</span><span class="sxs-lookup"><span data-stu-id="81687-124">When looking for a name described in a `cref` attribute, the compiler respects `Imports` statements that appear within the containing source file.</span></span>

* <span data-ttu-id="81687-125">`<summary>`タグは、型またはメンバーに関する情報を表示する、ドキュメント ビューアーで使用されます。</span><span class="sxs-lookup"><span data-stu-id="81687-125">The `<summary>` tag is intended to be used by a documentation viewer to display additional information about a type or member.</span></span>

<span data-ttu-id="81687-126">ドキュメント ファイルがのみに含まれるドキュメントのコメントの型とメンバーに関する完全な情報を提供しないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="81687-126">Note that the documentation file does not provide full information about a type and members, only what is contained in the document comments.</span></span> <span data-ttu-id="81687-127">型またはメンバーに関する詳細情報を表示するには、実際の型またはメンバーのリフレクションと組み合わせてドキュメント ファイルを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="81687-127">To get more information about a type or member, the documentation file must be used in conjunction with reflection on the actual type or member.</span></span>

## <a name="recommended-tags"></a><span data-ttu-id="81687-128">推奨されるタグ</span><span class="sxs-lookup"><span data-stu-id="81687-128">Recommended tags</span></span>

<span data-ttu-id="81687-129">ドキュメント ジェネレーターは、そのまま使用し、XML の規則に従って有効な任意のタグを処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="81687-129">The documentation generator must accept and process any tag that is valid according to the rules of XML.</span></span> <span data-ttu-id="81687-130">次のタグによって、ユーザー ドキュメントで一般的に使用される機能が与えられます。</span><span class="sxs-lookup"><span data-stu-id="81687-130">The following tags provide commonly used functionality in user documentation:</span></span>

<span data-ttu-id="81687-131">`<c>` コードのようなフォントのテキストを設定します。</span><span class="sxs-lookup"><span data-stu-id="81687-131">`<c>` Sets text in a code-like font</span></span>

<span data-ttu-id="81687-132">`<code>` コードのようなフォントで 1 つまたは複数の行のソース コードまたはプログラムの出力を設定します。</span><span class="sxs-lookup"><span data-stu-id="81687-132">`<code>` Sets one or more lines of source code or program output in a code-like font</span></span>

<span data-ttu-id="81687-133">`<example>` 例を示します</span><span class="sxs-lookup"><span data-stu-id="81687-133">`<example>` Indicates an example</span></span>

<span data-ttu-id="81687-134">`<exception>` メソッドがスローできる例外を識別します。</span><span class="sxs-lookup"><span data-stu-id="81687-134">`<exception>` Identifies the exceptions a method can throw</span></span>

<span data-ttu-id="81687-135">`<include>` 外部の XML ドキュメントが含まれています</span><span class="sxs-lookup"><span data-stu-id="81687-135">`<include>` Includes an external XML document</span></span>

<span data-ttu-id="81687-136">`<list>` リストまたはテーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="81687-136">`<list>` Creates a list or table</span></span>

<span data-ttu-id="81687-137">`<para>` により、テキストに追加される構造体</span><span class="sxs-lookup"><span data-stu-id="81687-137">`<para>` Permits structure to be added to text</span></span>

<span data-ttu-id="81687-138">`<param>` メソッドまたはコンス トラクターのパラメーターについて説明します</span><span class="sxs-lookup"><span data-stu-id="81687-138">`<param>` Describes a parameter for a method or constructor</span></span>

<span data-ttu-id="81687-139">`<paramref>` 単語がパラメーター名を識別します。</span><span class="sxs-lookup"><span data-stu-id="81687-139">`<paramref>` Identifies that a word is a parameter name</span></span>

<span data-ttu-id="81687-140">`<permission>` メンバーのセキュリティのユーザー補助を説明します。</span><span class="sxs-lookup"><span data-stu-id="81687-140">`<permission>` Documents the security accessibility of a member</span></span>

<span data-ttu-id="81687-141">`<remarks>` 型を記述します</span><span class="sxs-lookup"><span data-stu-id="81687-141">`<remarks>` Describes a type</span></span>

<span data-ttu-id="81687-142">`<returns>` メソッドの戻り値について説明します</span><span class="sxs-lookup"><span data-stu-id="81687-142">`<returns>` Describes the return value of a method</span></span>

<span data-ttu-id="81687-143">`<see>` リンクを指定します</span><span class="sxs-lookup"><span data-stu-id="81687-143">`<see>` Specifies a link</span></span>

<span data-ttu-id="81687-144">`<seealso>` 「参照」エントリを生成します。</span><span class="sxs-lookup"><span data-stu-id="81687-144">`<seealso>` Generates a See Also entry</span></span>

<span data-ttu-id="81687-145">`<summary>` 型のメンバーについて説明します</span><span class="sxs-lookup"><span data-stu-id="81687-145">`<summary>` Describes a member of a type</span></span>

<span data-ttu-id="81687-146">`<typeparam>` 型パラメーターについて説明します</span><span class="sxs-lookup"><span data-stu-id="81687-146">`<typeparam>` Describes a type parameter</span></span>

<span data-ttu-id="81687-147">`<value>` プロパティについて説明します</span><span class="sxs-lookup"><span data-stu-id="81687-147">`<value>` Describes a property</span></span>

### <a name="ltcgt"></a><span data-ttu-id="81687-148">&lt;c&gt;</span><span class="sxs-lookup"><span data-stu-id="81687-148">&lt;c&gt;</span></span>

<span data-ttu-id="81687-149">このタグは、説明内のテキストの一部がコードのブロックを使用するようなフォントを使用することを指定します。</span><span class="sxs-lookup"><span data-stu-id="81687-149">This tag specifies that a fragment of text within a description should use a font like that used for a block of code.</span></span> <span data-ttu-id="81687-150">(実際のコード行を使用して`<code>`)。</span><span class="sxs-lookup"><span data-stu-id="81687-150">(For lines of actual code, use `<code>`.)</span></span>

<span data-ttu-id="81687-151">__構文:__</span><span class="sxs-lookup"><span data-stu-id="81687-151">__Syntax:__</span></span>

```xml
<c>text to be set like code</c>
```

<span data-ttu-id="81687-152">__例:__</span><span class="sxs-lookup"><span data-stu-id="81687-152">__Example:__</span></span>

```vb
''' <remarks>
''' Class <c>Point</c> models a point in a two-dimensional plane.
''' </remarks>
Public Class Point 
End Class
```

### <a name="ltcodegt"></a><span data-ttu-id="81687-153">&lt;code&gt;</span><span class="sxs-lookup"><span data-stu-id="81687-153">&lt;code&gt;</span></span>

<span data-ttu-id="81687-154">このタグは、ソース コードまたはプログラムの出力の 1 つまたは複数の行が固定幅フォントを使用することを指定します。</span><span class="sxs-lookup"><span data-stu-id="81687-154">This tag specifies that one or more lines of source code or program output should use a fixed-width font.</span></span> <span data-ttu-id="81687-155">(小さなコードのフラグメントを使用して、 `<c>`)。</span><span class="sxs-lookup"><span data-stu-id="81687-155">(For small code fragments, use `<c>`.)</span></span>

<span data-ttu-id="81687-156">__構文:__</span><span class="sxs-lookup"><span data-stu-id="81687-156">__Syntax:__</span></span>

```xml
<code>source code or program output</code>
```

<span data-ttu-id="81687-157">__例:__</span><span class="sxs-lookup"><span data-stu-id="81687-157">__Example:__</span></span>

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

### <a name="ltexamplegt"></a><span data-ttu-id="81687-158">&lt;example&gt;</span><span class="sxs-lookup"><span data-stu-id="81687-158">&lt;example&gt;</span></span>

<span data-ttu-id="81687-159">このタグは、要素の使用方法を説明するコメント内のコード例を使用します。</span><span class="sxs-lookup"><span data-stu-id="81687-159">This tag allows example code within a comment to show how an element can be used.</span></span> <span data-ttu-id="81687-160">通常、このプロセスでは、タグの使用`<code>`もします。</span><span class="sxs-lookup"><span data-stu-id="81687-160">Ordinarily, this will involve use of the tag `<code>` as well.</span></span>

<span data-ttu-id="81687-161">__構文:__</span><span class="sxs-lookup"><span data-stu-id="81687-161">__Syntax:__</span></span>

```xml
<example>description</example>
```

<span data-ttu-id="81687-162">__例:__</span><span class="sxs-lookup"><span data-stu-id="81687-162">__Example:__</span></span>

<span data-ttu-id="81687-163">例については、「`<code>`」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="81687-163">See `<code>` for an example.</span></span>

### <a name="ltexceptiongt"></a><span data-ttu-id="81687-164">&lt;exception&gt;</span><span class="sxs-lookup"><span data-stu-id="81687-164">&lt;exception&gt;</span></span>

<span data-ttu-id="81687-165">このタグは、メソッドがスローできる例外を文書化する方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="81687-165">This tag provides a way to document the exceptions a method can throw.</span></span>

<span data-ttu-id="81687-166">__構文:__</span><span class="sxs-lookup"><span data-stu-id="81687-166">__Syntax:__</span></span>

```xml
<exception cref="member">description</exception>
```

<span data-ttu-id="81687-167">__例:__</span><span class="sxs-lookup"><span data-stu-id="81687-167">__Example:__</span></span>

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

### <a name="ltincludegt"></a><span data-ttu-id="81687-168">&lt;include&gt;</span><span class="sxs-lookup"><span data-stu-id="81687-168">&lt;include&gt;</span></span>

<span data-ttu-id="81687-169">このタグを使用して、外部の整形式 XML ドキュメントからの情報が含まれます。</span><span class="sxs-lookup"><span data-stu-id="81687-169">This tag is used to include information from an external well-formed XML document.</span></span> <span data-ttu-id="81687-170">XPath 式は、XML に追加する内容、ドキュメントからを指定する XML ドキュメントに適用されます。</span><span class="sxs-lookup"><span data-stu-id="81687-170">An XPath expression is applied to the XML document to specify what XML should be included from the document.</span></span> <span data-ttu-id="81687-171">`<include>`タグは、選択した外部のドキュメントから XML に置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="81687-171">The `<include>` tag is then replaced with the selected XML from the external document.</span></span>

<span data-ttu-id="81687-172">__構文:__</span><span class="sxs-lookup"><span data-stu-id="81687-172">__Syntax:__</span></span>

```xml
<include file="filename" path="xpath">
```

<span data-ttu-id="81687-173">__例:__</span><span class="sxs-lookup"><span data-stu-id="81687-173">__Example:__</span></span>

<span data-ttu-id="81687-174">場合は、ソース コードには、次のような宣言が含まれています。</span><span class="sxs-lookup"><span data-stu-id="81687-174">If the source code contained a declaration like the following:</span></span>

```vb
''' <include file="docs.xml" path="extra/class[@name="IntList"]/*" />
```

<span data-ttu-id="81687-175">外部ファイル docs.xml にあり、次の内容</span><span class="sxs-lookup"><span data-stu-id="81687-175">and the external file docs.xml had the following contents</span></span>

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

<span data-ttu-id="81687-176">同じドキュメントは、ソース コードが含まれている場合、出力を示します。</span><span class="sxs-lookup"><span data-stu-id="81687-176">then the same documentation is output as if the source code contained:</span></span>

```xml
''' <summary>
''' Contains a list of integers.
''' </summary>
```

### <a name="ltlistgt"></a><span data-ttu-id="81687-177">&lt;リスト&gt;</span><span class="sxs-lookup"><span data-stu-id="81687-177">&lt;list&gt;</span></span>

<span data-ttu-id="81687-178">このタグは、リストまたは項目のテーブルの作成に使用されます。</span><span class="sxs-lookup"><span data-stu-id="81687-178">This tag is used to create a list or table of items.</span></span> <span data-ttu-id="81687-179">`<listheader>`のテーブルまたは定義の一覧の見出し行を定義するブロック。</span><span class="sxs-lookup"><span data-stu-id="81687-179">It may contain a `<listheader>` block to define the heading row of either a table or definition list.</span></span> <span data-ttu-id="81687-180">(テーブルを定義するときに、見出しの用語のエントリのみを指定します。)</span><span class="sxs-lookup"><span data-stu-id="81687-180">(When defining a table, only an entry for term in the heading need be supplied.)</span></span>

<span data-ttu-id="81687-181">リスト内の各項目を指定した、`<item>`ブロックします。</span><span class="sxs-lookup"><span data-stu-id="81687-181">Each item in the list is specified with an `<item>` block.</span></span> <span data-ttu-id="81687-182">定義リストを作成するときに用語と説明の両方を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="81687-182">When creating a definition list, both term and description must be specified.</span></span> <span data-ttu-id="81687-183">ただし、テーブル、箇条書きまたは番号付きリストの説明のみを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="81687-183">However, for a table, bulleted list, or numbered list, only description need be specified.</span></span>

<span data-ttu-id="81687-184">__構文:__</span><span class="sxs-lookup"><span data-stu-id="81687-184">__Syntax:__</span></span>

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

<span data-ttu-id="81687-185">__例:__</span><span class="sxs-lookup"><span data-stu-id="81687-185">__Example:__</span></span>

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

### <a name="ltparagt"></a><span data-ttu-id="81687-186">&lt;para&gt;</span><span class="sxs-lookup"><span data-stu-id="81687-186">&lt;para&gt;</span></span>

<span data-ttu-id="81687-187">このタグは、その他のタグ内で使用できるよう`<remarks>`または`<returns>`、構造体をテキストに追加することができます。</span><span class="sxs-lookup"><span data-stu-id="81687-187">This tag is for use inside other tags, such as `<remarks>` or `<returns>`, and permits structure to be added to text.</span></span>

<span data-ttu-id="81687-188">__構文:__</span><span class="sxs-lookup"><span data-stu-id="81687-188">__Syntax:__</span></span>

```xml
<para>content</para>
```

<span data-ttu-id="81687-189">__例:__</span><span class="sxs-lookup"><span data-stu-id="81687-189">__Example:__</span></span>

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

### <a name="ltparamgt"></a><span data-ttu-id="81687-190">&lt;param&gt;</span><span class="sxs-lookup"><span data-stu-id="81687-190">&lt;param&gt;</span></span>

<span data-ttu-id="81687-191">このタグには、メソッド、コンス トラクター、またはインデックス付きプロパティのパラメーターについて説明します。</span><span class="sxs-lookup"><span data-stu-id="81687-191">This tag describes a parameter for a method, constructor, or indexed property.</span></span>

<span data-ttu-id="81687-192">__構文:__</span><span class="sxs-lookup"><span data-stu-id="81687-192">__Syntax:__</span></span>

```xml
<param name="name">description</param>
```

<span data-ttu-id="81687-193">__例:__</span><span class="sxs-lookup"><span data-stu-id="81687-193">__Example:__</span></span>

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

### <a name="ltparamrefgt"></a><span data-ttu-id="81687-194">&lt;paramref&gt;</span><span class="sxs-lookup"><span data-stu-id="81687-194">&lt;paramref&gt;</span></span>

<span data-ttu-id="81687-195">このタグは、単語がパラメーターであることを示します。</span><span class="sxs-lookup"><span data-stu-id="81687-195">This tag indicates that a word is a parameter.</span></span> <span data-ttu-id="81687-196">ドキュメント ファイルを処理することで、何らかの方法でこのパラメーターの書式を設定します。</span><span class="sxs-lookup"><span data-stu-id="81687-196">The documentation file can be processed to format this parameter in some distinct way.</span></span>

<span data-ttu-id="81687-197">__構文:__</span><span class="sxs-lookup"><span data-stu-id="81687-197">__Syntax:__</span></span>

```xml
<paramref name="name"/>
```

<span data-ttu-id="81687-198">__例:__</span><span class="sxs-lookup"><span data-stu-id="81687-198">__Example:__</span></span>

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

### <a name="ltpermissiongt"></a><span data-ttu-id="81687-199">&lt;permission&gt;</span><span class="sxs-lookup"><span data-stu-id="81687-199">&lt;permission&gt;</span></span>

<span data-ttu-id="81687-200">このタグは、メンバーのセキュリティのアクセシビリティをドキュメントします。</span><span class="sxs-lookup"><span data-stu-id="81687-200">This tag documents the security accessibility of a member</span></span>

<span data-ttu-id="81687-201">__構文:__</span><span class="sxs-lookup"><span data-stu-id="81687-201">__Syntax:__</span></span>

```xml
<permission cref="member">description</permission>
```

<span data-ttu-id="81687-202">__例:__</span><span class="sxs-lookup"><span data-stu-id="81687-202">__Example:__</span></span>

```vb
''' <permission cref="System.Security.PermissionSet">Everyone can
''' access this method.</permission>
Public Shared Sub Test()
End Sub
```

### <a name="ltremarksgt"></a><span data-ttu-id="81687-203">&lt;remarks&gt;</span><span class="sxs-lookup"><span data-stu-id="81687-203">&lt;remarks&gt;</span></span>

<span data-ttu-id="81687-204">このタグは、型に関する概要情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="81687-204">This tag specifies overview information about a type.</span></span> <span data-ttu-id="81687-205">(使用`<summary>`に型のメンバーを示しています)。</span><span class="sxs-lookup"><span data-stu-id="81687-205">(Use `<summary>` to describe the members of a type.)</span></span>

<span data-ttu-id="81687-206">__構文:__</span><span class="sxs-lookup"><span data-stu-id="81687-206">__Syntax:__</span></span>

```xml
<remarks>description</remarks>
```

<span data-ttu-id="81687-207">__例:__</span><span class="sxs-lookup"><span data-stu-id="81687-207">__Example:__</span></span>

```vb
''' <remarks>
''' Class <c>Point</c> models a point in a two-dimensional plane.
''' </remarks>
Public Class Point 
End Class
```

### <a name="ltreturnsgt"></a><span data-ttu-id="81687-208">&lt;returns&gt;</span><span class="sxs-lookup"><span data-stu-id="81687-208">&lt;returns&gt;</span></span>

<span data-ttu-id="81687-209">このタグには、メソッドの戻り値について説明します。</span><span class="sxs-lookup"><span data-stu-id="81687-209">This tag describes the return value of a method.</span></span>

<span data-ttu-id="81687-210">__構文:__</span><span class="sxs-lookup"><span data-stu-id="81687-210">__Syntax:__</span></span>

```xml
<returns>description</returns>
```

<span data-ttu-id="81687-211">__例:__</span><span class="sxs-lookup"><span data-stu-id="81687-211">__Example:__</span></span>

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

### <a name="ltseegt"></a><span data-ttu-id="81687-212">&lt;see&gt;</span><span class="sxs-lookup"><span data-stu-id="81687-212">&lt;see&gt;</span></span>

<span data-ttu-id="81687-213">このタグは、テキスト内で指定するためのリンクを使用します。</span><span class="sxs-lookup"><span data-stu-id="81687-213">This tag allows a link to be specified within text.</span></span> <span data-ttu-id="81687-214">(使用`<seealso>`「参照」セクションに表示するテキストを示します。)</span><span class="sxs-lookup"><span data-stu-id="81687-214">(Use `<seealso>` to indicate text that is to appear in a See Also section.)</span></span>

<span data-ttu-id="81687-215">__構文:__</span><span class="sxs-lookup"><span data-stu-id="81687-215">__Syntax:__</span></span>

```xml
<see cref="member"/>
```

<span data-ttu-id="81687-216">__例:__</span><span class="sxs-lookup"><span data-stu-id="81687-216">__Example:__</span></span>

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

### <a name="ltseealsogt"></a><span data-ttu-id="81687-217">&lt;seealso&gt;</span><span class="sxs-lookup"><span data-stu-id="81687-217">&lt;seealso&gt;</span></span>

<span data-ttu-id="81687-218">このタグは、「参照」セクションのエントリをを生成します。</span><span class="sxs-lookup"><span data-stu-id="81687-218">This tag generates an entry for the See Also section.</span></span> <span data-ttu-id="81687-219">(使用`<see>`からテキスト内のリンクを指定します)。</span><span class="sxs-lookup"><span data-stu-id="81687-219">(Use `<see>` to specify a link from within text.)</span></span>

<span data-ttu-id="81687-220">__構文:__</span><span class="sxs-lookup"><span data-stu-id="81687-220">__Syntax:__</span></span>

```xml
<seealso cref="member"/>
```

<span data-ttu-id="81687-221">__例:__</span><span class="sxs-lookup"><span data-stu-id="81687-221">__Example:__</span></span>

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

### <a name="ltsummarygt"></a><span data-ttu-id="81687-222">&lt;summary&gt;</span><span class="sxs-lookup"><span data-stu-id="81687-222">&lt;summary&gt;</span></span>

<span data-ttu-id="81687-223">このタグには、型のメンバーについて説明します。</span><span class="sxs-lookup"><span data-stu-id="81687-223">This tag describes a type member.</span></span> <span data-ttu-id="81687-224">(使用`<remarks>`型自体を記述する)。</span><span class="sxs-lookup"><span data-stu-id="81687-224">(Use `<remarks>` to describe a type itself.)</span></span>

<span data-ttu-id="81687-225">__構文:__</span><span class="sxs-lookup"><span data-stu-id="81687-225">__Syntax:__</span></span>

```xml
<summary>description</summary>
```

<span data-ttu-id="81687-226">__例:__</span><span class="sxs-lookup"><span data-stu-id="81687-226">__Example:__</span></span>

```vb
''' <summary>
''' This constructor initializes the new Point to (0,0).
''' </summary>
Public Sub New()
    Me.New(0,0)
End Sub
```

### <a name="lttypeparamgt"></a><span data-ttu-id="81687-227">&lt;typeparam&gt;</span><span class="sxs-lookup"><span data-stu-id="81687-227">&lt;typeparam&gt;</span></span>

<span data-ttu-id="81687-228">このタグには、型パラメーターについて説明します。</span><span class="sxs-lookup"><span data-stu-id="81687-228">This tag describes a type parameter.</span></span>

<span data-ttu-id="81687-229">__構文:__</span><span class="sxs-lookup"><span data-stu-id="81687-229">__Syntax:__</span></span>

```xml
<typeparam name="name">description</typeparam>
```

<span data-ttu-id="81687-230">__例:__</span><span class="sxs-lookup"><span data-stu-id="81687-230">__Example:__</span></span>

```vb
''' <typeparam name="T">
''' The base item type. Must implement IComparable.
''' </typeparam>
Public Class ItemManager(Of T As IComparable)
End Class
```

### <a name="ltvaluegt"></a><span data-ttu-id="81687-231">&lt;value&gt;</span><span class="sxs-lookup"><span data-stu-id="81687-231">&lt;value&gt;</span></span>

<span data-ttu-id="81687-232">このタグには、プロパティについて説明します。</span><span class="sxs-lookup"><span data-stu-id="81687-232">This tag describes a property.</span></span>

<span data-ttu-id="81687-233">__構文:__</span><span class="sxs-lookup"><span data-stu-id="81687-233">__Syntax:__</span></span>

```xml
<value>property description</value>
```

<span data-ttu-id="81687-234">__例:__</span><span class="sxs-lookup"><span data-stu-id="81687-234">__Example:__</span></span>

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

## <a name="id-strings"></a><span data-ttu-id="81687-235">ID 文字列</span><span class="sxs-lookup"><span data-stu-id="81687-235">ID Strings</span></span>

<span data-ttu-id="81687-236">ドキュメント ファイルを生成するときに、コンパイラは、一意に識別されているドキュメントのコメントとタグ付けされているソース コード内の各要素の ID 文字列を生成します。</span><span class="sxs-lookup"><span data-stu-id="81687-236">When generating the documentation file, the compiler generates an ID string for each element in the source code that is tagged with a documentation comment that uniquely identifies it.</span></span> <span data-ttu-id="81687-237">この ID 文字列は、ドキュメントのコメントに対応するコンパイル済みアセンブリ内のどの要素を識別するために、外部ツールで使用できます。</span><span class="sxs-lookup"><span data-stu-id="81687-237">This ID string can be used by external tools to identify which element in a compiled assembly corresponds to the document comment.</span></span>

<span data-ttu-id="81687-238">ID 文字列は、次のように生成されます。</span><span class="sxs-lookup"><span data-stu-id="81687-238">ID strings are generated as follows:</span></span>

<span data-ttu-id="81687-239">文字列に空白は配置されません。</span><span class="sxs-lookup"><span data-stu-id="81687-239">No white space is placed in the string.</span></span>

<span data-ttu-id="81687-240">文字列の最初の部分では、単一の文字の後にコロンを使用して、記述されているメンバーの種類を識別します。</span><span class="sxs-lookup"><span data-stu-id="81687-240">The first part of the string identifies the kind of member being documented, via a single character followed by a colon.</span></span> <span data-ttu-id="81687-241">その後にかっこで囲まれた対応する文字で、次の種類のメンバーを定義: イベント (E) フィールド (F) コンス トラクターと演算子 (M)、名前空間 (N)、(P) のプロパティと型 (T) を含むメソッド。</span><span class="sxs-lookup"><span data-stu-id="81687-241">The following kinds of members are defined, with the corresponding character in parenthesis after it: events (E), fields (F), methods including constructors and operators (M), namespaces (N), properties (P) and types (T).</span></span> <span data-ttu-id="81687-242">感嘆符 (!) は、ID の文字列を生成中にエラーが発生しました。 文字列の残りの部分は、エラーに関する情報を提供します。 を示します。</span><span class="sxs-lookup"><span data-stu-id="81687-242">An exclamation point (!) indicates an error occurred while generating the ID string, and the rest of the string provides information about the error.</span></span>

<span data-ttu-id="81687-243">文字列の 2 番目の部分は、グローバル名前空間、要素の完全修飾名です。</span><span class="sxs-lookup"><span data-stu-id="81687-243">The second part of the string is the fully qualified name of the element, starting at the global namespace.</span></span> <span data-ttu-id="81687-244">要素、それを囲む型、および名前空間の名前は、ピリオドで区切られます。</span><span class="sxs-lookup"><span data-stu-id="81687-244">The name of the element, its enclosing type(s), and namespace are separated by periods.</span></span> <span data-ttu-id="81687-245">項目自体の名前にピリオドがある場合は、置き換えられる、シャープ記号 (#)。</span><span class="sxs-lookup"><span data-stu-id="81687-245">If the name of the item itself has periods, they are replaced by the pound sign (#).</span></span> <span data-ttu-id="81687-246">(これと見なされます要素の名前にはこの文字がありません。)型パラメーターを持つ型の名前は、型の型パラメーターの数を表す数値を続けて、バッククォート (') で終わります。</span><span class="sxs-lookup"><span data-stu-id="81687-246">(It is assumed that no element has this character in its name.) The name of a type with type parameters ends with a backquote (\`) followed by a number that represents the number of type parameters on the type.</span></span> <span data-ttu-id="81687-247">それらが格納された型の型パラメーターに入れ子にされた型にアクセスし、入れ子にされた型が暗黙的にその親の型の型パラメーターを含めることがそれらの型がこのが型パラメーターの合計に反映させるための点に注意することが重要大文字にします。</span><span class="sxs-lookup"><span data-stu-id="81687-247">It is important to remember that because nested types have access to the type parameters of the types containing them, nested types implicitly contain the type parameters of their containing types, and those types are counted in their type parameter totals in this case.</span></span>

<span data-ttu-id="81687-248">メソッドとプロパティの引数は、引数には、次のように、かっこで囲まれているが一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="81687-248">For methods and properties with arguments, the argument list follows, enclosed in parentheses.</span></span> <span data-ttu-id="81687-249">引数を指定せずに、かっこを省略します。</span><span class="sxs-lookup"><span data-stu-id="81687-249">For those without arguments, the parentheses are omitted.</span></span> <span data-ttu-id="81687-250">引数はコンマで区切られます。</span><span class="sxs-lookup"><span data-stu-id="81687-250">The arguments are separated by commas.</span></span> <span data-ttu-id="81687-251">各引数のエンコーディング CLI シグネチャと同じとおりです。引数は、完全修飾名で表されます。</span><span class="sxs-lookup"><span data-stu-id="81687-251">The encoding of each argument is the same as a CLI signature, as follows: Arguments are represented by their fully qualified name.</span></span> <span data-ttu-id="81687-252">たとえば、`Integer`になります`System.Int32`、`String`なります`System.String`、`Object`になります`System.Object`など。</span><span class="sxs-lookup"><span data-stu-id="81687-252">For example, `Integer` becomes `System.Int32`, `String` becomes `System.String`, `Object` becomes `System.Object`, and so on.</span></span> <span data-ttu-id="81687-253">引数、`ByRef`修飾子が、' @'、型名の後です。</span><span class="sxs-lookup"><span data-stu-id="81687-253">Arguments having the `ByRef` modifier have a '@' following their type name.</span></span> <span data-ttu-id="81687-254">引数、 `ByVal`、`Optional`または`ParamArray`修飾子には特殊な表記はあるありません。</span><span class="sxs-lookup"><span data-stu-id="81687-254">Arguments having the `ByVal`, `Optional` or `ParamArray` modifier have no special notation.</span></span> <span data-ttu-id="81687-255">引数には、配列として表されます`[lowerbound:size, ..., lowerbound:size]`コンマの数がランク - 1 で、下限と各次元のサイズがわかっている場合が 10 進数で表されます。</span><span class="sxs-lookup"><span data-stu-id="81687-255">Arguments that are arrays are represented as `[lowerbound:size, ..., lowerbound:size]` where the number of commas is the rank - 1, and the lower bounds and size of each dimension, if known, are represented in decimal.</span></span> <span data-ttu-id="81687-256">下限またはサイズが指定されていない場合は省略されます。</span><span class="sxs-lookup"><span data-stu-id="81687-256">If a lower bound or size is not specified, it is omitted.</span></span> <span data-ttu-id="81687-257">特定の次元で下限およびサイズが省略されている場合は、':' も省略されます。</span><span class="sxs-lookup"><span data-stu-id="81687-257">If the lower bound and size for a particular dimension are omitted, the ':' is omitted as well.</span></span> <span data-ttu-id="81687-258">配列の配列は 1 つで表されます"`[]`"個々 のレベル。</span><span class="sxs-lookup"><span data-stu-id="81687-258">Arrays of arrays are represented by one "`[]`" per level.</span></span>

### <a name="id-string-examples"></a><span data-ttu-id="81687-259">ID 文字列の例</span><span class="sxs-lookup"><span data-stu-id="81687-259">ID string examples</span></span>

<span data-ttu-id="81687-260">次の例については、ドキュメントのコメントを持つことのできる各ソース要素から生成される ID 文字列と共に、VB コードのフラグメントを示します。</span><span class="sxs-lookup"><span data-stu-id="81687-260">The following examples each show a fragment of VB code, along with the ID string produced from each source element capable of having a documentation comment:</span></span>

<span data-ttu-id="81687-261">型は、完全修飾名で表されます。</span><span class="sxs-lookup"><span data-stu-id="81687-261">Types are represented using their fully qualified name.</span></span>

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

<span data-ttu-id="81687-262">フィールドは、完全修飾名で表されます。</span><span class="sxs-lookup"><span data-stu-id="81687-262">Fields are represented by their fully qualified name.</span></span>

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

<span data-ttu-id="81687-263">コンストラクター。</span><span class="sxs-lookup"><span data-stu-id="81687-263">Constructors.</span></span>

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

<span data-ttu-id="81687-264">メソッド。</span><span class="sxs-lookup"><span data-stu-id="81687-264">Methods.</span></span>

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

<span data-ttu-id="81687-265">プロパティ</span><span class="sxs-lookup"><span data-stu-id="81687-265">Properties.</span></span>

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

<span data-ttu-id="81687-266">イベント</span><span class="sxs-lookup"><span data-stu-id="81687-266">Events</span></span>   

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

<span data-ttu-id="81687-267">演算子。</span><span class="sxs-lookup"><span data-stu-id="81687-267">Operators.</span></span>

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

<span data-ttu-id="81687-268">変換演算子は、末尾が`~`後に、戻り値の型。</span><span class="sxs-lookup"><span data-stu-id="81687-268">Conversion operators have a trailing `~` followed by the return type.</span></span>

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

## <a name="documentation-comments-example"></a><span data-ttu-id="81687-269">ドキュメント コメントの例</span><span class="sxs-lookup"><span data-stu-id="81687-269">Documentation comments example</span></span>

<span data-ttu-id="81687-270">次の例のソース コードを示しています、`Point`クラス。</span><span class="sxs-lookup"><span data-stu-id="81687-270">The following example shows the source code of a `Point` class:</span></span>

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

<span data-ttu-id="81687-271">クラスのソース コードが指定されると生成される出力を次に示します`Point`、前述のようにします。</span><span class="sxs-lookup"><span data-stu-id="81687-271">Here is the output produced when given the source code for class `Point`, shown above:</span></span>

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
