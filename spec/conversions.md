---
ms.openlocfilehash: 7aef52145a71bff1d489772e81eb786a9dbd23d1
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306191"
---
# <a name="conversions"></a><span data-ttu-id="24e55-101">変換</span><span class="sxs-lookup"><span data-stu-id="24e55-101">Conversions</span></span>

<span data-ttu-id="24e55-102">変換とは、ある型から別の型に値を変更する処理のことです。</span><span class="sxs-lookup"><span data-stu-id="24e55-102">Conversion is the process of changing a value from one type to another.</span></span> <span data-ttu-id="24e55-103">たとえば、`Integer` 型の値を `Double`型の値に変換したり、`Derived` 型の値を `Base`型の値に変換したりすることができます。これは `Base` と `Derived` が両方ともクラスであり、`Derived` から継承されていることを前提としています。`Base`</span><span class="sxs-lookup"><span data-stu-id="24e55-103">For example, a value of type `Integer` can be converted to a value of type `Double`, or a value of type `Derived` can be converted to a value of type `Base`, assuming that `Base` and `Derived` are both classes and `Derived` inherits from `Base`.</span></span> <span data-ttu-id="24e55-104">変換では、値自体を変更する必要がない場合があります (後者の例のように)。または、(前の例のように) 値表現に大きな変更が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="24e55-104">Conversions may not require the value itself to change (as in the latter example), or they may require significant changes in the value representation (as in the former example).</span></span>

<span data-ttu-id="24e55-105">変換は、拡大または縮小することができます。</span><span class="sxs-lookup"><span data-stu-id="24e55-105">Conversions may either be widening or narrowing.</span></span> <span data-ttu-id="24e55-106">*拡大変換*とは、ある型から別の型への変換のことです。この型は、値ドメインが元の型の値ドメインよりも大きい場合は、少なくとも大きくなります。</span><span class="sxs-lookup"><span data-stu-id="24e55-106">A *widening conversion* is a conversion from a type to another type whose value domain is at least as big, if not bigger, than the original type's value domain.</span></span> <span data-ttu-id="24e55-107">拡大変換は失敗しません。</span><span class="sxs-lookup"><span data-stu-id="24e55-107">Widening conversions should never fail.</span></span> <span data-ttu-id="24e55-108">*縮小変換*とは、ある型から別の型への変換のことです。この型は、値ドメインが元の型の値ドメインより小さいか、または変換を実行するときに余分な注意が必要である (たとえば、`Integer` から `String`に変換する場合など)。</span><span class="sxs-lookup"><span data-stu-id="24e55-108">A *narrowing conversion* is a conversion from a type to another type whose value domain is either smaller than the original type's value domain or sufficiently unrelated that extra care must be taken when doing the conversion (for example, when converting from `Integer` to `String`).</span></span> <span data-ttu-id="24e55-109">情報の損失を伴う可能性がある縮小変換は失敗する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="24e55-109">Narrowing conversions, which may entail loss of information, can fail.</span></span>

<span data-ttu-id="24e55-110">Id 変換 (型からそれ自体への変換) と既定値の変換 (つまり、`Nothing`からの変換) は、すべての型に対して定義されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-110">The identity conversion (i.e. a conversion from a type to itself) and default value conversion (i.e. a conversion from `Nothing`) are defined for all types.</span></span>

## <a name="implicit-and-explicit-conversions"></a><span data-ttu-id="24e55-111">暗黙の型変換と明示的な型変換</span><span class="sxs-lookup"><span data-stu-id="24e55-111">Implicit and Explicit Conversions</span></span>

<span data-ttu-id="24e55-112">変換は、*暗黙的*または*明示的*に行うことができます。</span><span class="sxs-lookup"><span data-stu-id="24e55-112">Conversions can be either *implicit* or *explicit*.</span></span> <span data-ttu-id="24e55-113">暗黙の型変換は、特別な構文を使用せずに発生します。</span><span class="sxs-lookup"><span data-stu-id="24e55-113">Implicit conversions occur without any special syntax.</span></span> <span data-ttu-id="24e55-114">`Integer` 値から `Long` 値への暗黙的な変換の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="24e55-114">The following is an example of implicit conversion of an `Integer` value to a `Long` value:</span></span>

```vb
Module Test
    Sub Main()
        Dim intValue As Integer = 123
        Dim longValue As Long = intValue

        Console.WriteLine(intValue & " = " & longValue)
    End Sub
End Module
```

一方、明示的な変換では、キャスト演算子が必要です。 Cast 演算子を使用せずに値に対して明示的な変換を実行しようとすると、コンパイル時エラーが発生します。 <span data-ttu-id="24e55-117">次の例では、明示的な変換を使用して、`Long` 値を `Integer` 値に変換します。</span><span class="sxs-lookup"><span data-stu-id="24e55-117">The following example uses an explicit conversion to convert a `Long` value to an `Integer` value.</span></span>

```vb
Module Test
    Sub Main()
        Dim longValue As Long = 134
        Dim intValue As Integer = CInt(longValue)

        Console.WriteLine(longValue & " = " & intValue)
    End Sub
End Module
```

暗黙的な変換のセットは、コンパイル環境と `Option Strict` ステートメントによって異なります。 厳密なセマンティクスが使用されている場合、暗黙的に拡張変換のみが発生する可能性があります。 <span data-ttu-id="24e55-120">寛容なセマンティクスが使用されている場合、すべての拡大変換および縮小変換 (つまり、すべての変換) が暗黙的に発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="24e55-120">If permissive semantics are being used, all widening and narrowing conversions (in other words, all conversions) may occur implicitly.</span></span>

## <a name="boolean-conversions"></a><span data-ttu-id="24e55-121">ブール型変換</span><span class="sxs-lookup"><span data-stu-id="24e55-121">Boolean Conversions</span></span>

<span data-ttu-id="24e55-122">`Boolean` は数値型ではありませんが、列挙型であるかのように、数値型との間で縮小変換が行われます。</span><span class="sxs-lookup"><span data-stu-id="24e55-122">Although `Boolean` is not a numeric type, it does have narrowing conversions to and from the numeric types as if it were an enumerated type.</span></span> <span data-ttu-id="24e55-123">リテラル `True` は、`Byte`の場合はリテラル `255` に変換されます。 `UShort`の場合は `65535`、`4294967295` の場合は `UInteger`、`18446744073709551615` の場合は `ULong`、`-1`、`SByte`、`Short`、`Integer`、`Long`、`Decimal`の場合は式 `Single`に変換されます。`Double`</span><span class="sxs-lookup"><span data-stu-id="24e55-123">The literal `True` converts to the literal `255` for `Byte`, `65535` for `UShort`, `4294967295` for `UInteger`, `18446744073709551615` for `ULong`, and to the expression `-1` for `SByte`, `Short`, `Integer`, `Long`, `Decimal`, `Single`, and `Double`.</span></span> <span data-ttu-id="24e55-124">リテラル `False` は、リテラル `0`に変換されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-124">The literal `False` converts to the literal `0`.</span></span> <span data-ttu-id="24e55-125">数値が0の場合は、リテラル `False`に変換されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-125">A zero numeric value converts to the literal `False`.</span></span> <span data-ttu-id="24e55-126">その他のすべての数値は、リテラル `True`に変換されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-126">All other numeric values convert to the literal `True`.</span></span>

<span data-ttu-id="24e55-127">ブール値から文字列への縮小変換では、`System.Boolean.TrueString` または `System.Boolean.FalseString`のいずれかに変換されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-127">There is a narrowing conversion from Boolean to String, converting to either `System.Boolean.TrueString` or `System.Boolean.FalseString`.</span></span> <span data-ttu-id="24e55-128">`String` から `Boolean`への縮小変換もあります。文字列が `TrueString` または `FalseString` (現在のカルチャでは区別) と等しい場合は、適切な値が使用されます。それ以外の場合は、文字列を数値型として解析しようとします (可能な場合は16進数または8進数、それ以外の場合は float)。それ以外の場合は、`System.InvalidCastException`をスローします。</span><span class="sxs-lookup"><span data-stu-id="24e55-128">There is also a narrowing conversion from `String` to `Boolean`: if the string was equal to `TrueString` or `FalseString` (in the current culture, case-insensitively) then it uses the appropriate value; otherwise it attempts to parse the string as a numeric type (in hex or octal if possible, otherwise as a float) and uses the above rules; otherwise it throws `System.InvalidCastException`.</span></span>

## <a name="numeric-conversions"></a><span data-ttu-id="24e55-129">数値変換</span><span class="sxs-lookup"><span data-stu-id="24e55-129">Numeric Conversions</span></span>

<span data-ttu-id="24e55-130">型 `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`、`Decimal`、`Single`、`Double`、およびすべての列挙型の間には数値変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="24e55-130">Numeric conversions exist between the types `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single` and `Double`, and all enumerated types.</span></span> <span data-ttu-id="24e55-131">変換されるとき、列挙型は基になる型として扱われます。</span><span class="sxs-lookup"><span data-stu-id="24e55-131">When being converted, enumerated types are treated as if they were their underlying types.</span></span> <span data-ttu-id="24e55-132">列挙型に変換する場合、ソース値は列挙型で定義されている値のセットに準拠している必要はありません。</span><span class="sxs-lookup"><span data-stu-id="24e55-132">When converting to an enumerated type, the source value is not required to conform to the set of values defined in the enumerated type.</span></span> <span data-ttu-id="24e55-133">例 :</span><span class="sxs-lookup"><span data-stu-id="24e55-133">For example:</span></span>

```vb
Enum Values
    One
    Two
    Three
End Enum

Module Test
    Sub Main()
        Dim x As Integer = 5

        ' OK, even though there is no enumerated value for 5.
        Dim y As Values = CType(x, Values)
    End Sub
End Module
```

<span data-ttu-id="24e55-134">数値変換は、次のように実行時に処理されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-134">Numeric conversions are processed at run-time as follows:</span></span>

* <span data-ttu-id="24e55-135">数値型からより広い数値型への変換では、値は単純により広い型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-135">For a conversion from a numeric type to a wider numeric type, the value is simply converted to the wider type.</span></span> <span data-ttu-id="24e55-136">`UInteger`、`Integer`、`ULong`、`Long`、または `Decimal` から `Single` または `Double` への変換は、最も近い `Single` または `Double` 値に丸められます。</span><span class="sxs-lookup"><span data-stu-id="24e55-136">Conversions from `UInteger`, `Integer`, `ULong`, `Long`, or `Decimal` to `Single` or `Double` are rounded to the nearest `Single` or `Double` value.</span></span> <span data-ttu-id="24e55-137">この変換によって有効桁数が失われる可能性がありますが、マグニチュードが失われることはありません。</span><span class="sxs-lookup"><span data-stu-id="24e55-137">While this conversion may cause a loss of precision, it will never cause a loss of magnitude.</span></span>

* <span data-ttu-id="24e55-138">整数型から別の整数型への変換、または `Single`、`Double`、または `Decimal` から整数型への変換の結果は、整数のオーバーフローチェックがオンかどうかによって異なります。</span><span class="sxs-lookup"><span data-stu-id="24e55-138">For a conversion from an integral type to another integral type, or from `Single`, `Double`, or `Decimal` to an integral type, the result depends on whether integer overflow checking is on:</span></span>

  <span data-ttu-id="24e55-139">*整数オーバーフローを確認する場合は、次のようになります。*</span><span class="sxs-lookup"><span data-stu-id="24e55-139">*If integer overflow is being checked:*</span></span>

  * <span data-ttu-id="24e55-140">ソースが整数型の場合、変換元の引数が変換先の型の範囲内にある場合、変換は成功します。</span><span class="sxs-lookup"><span data-stu-id="24e55-140">If the source is an integral type, the conversion succeeds if the source argument is within the range of the destination type.</span></span> <span data-ttu-id="24e55-141">変換元の引数が変換先の型の範囲外の場合、変換は `System.OverflowException` 例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="24e55-141">The conversion throws a `System.OverflowException` exception if the source argument is outside the range of the destination type.</span></span>

  * <span data-ttu-id="24e55-142">ソースが `Single`、`Double`、または `Decimal`の場合、ソース値は最も近い整数値に切り上げられます。この整数値は変換の結果になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-142">If the source is `Single`, `Double`, or `Decimal`, the source value is rounded up or down to the nearest integral value, and this integral value becomes the result of the conversion.</span></span> <span data-ttu-id="24e55-143">ソース値が2つの整数値に均等に近い場合、値は、最下位桁の位置にある偶数を持つ値に丸められます。</span><span class="sxs-lookup"><span data-stu-id="24e55-143">If the source value is equally close to two integral values, the value is rounded to the value that has an even number in the least significant digit position.</span></span> <span data-ttu-id="24e55-144">結果の整数値が変換先の型の範囲外になると、`System.OverflowException` 例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="24e55-144">If the resulting integral value is outside the range of the destination type, a `System.OverflowException` exception is thrown.</span></span>

  <span data-ttu-id="24e55-145">*整数オーバーフローがチェックされていない場合は、次のようになります。*</span><span class="sxs-lookup"><span data-stu-id="24e55-145">*If integer overflow is not being checked:*</span></span>

  * <span data-ttu-id="24e55-146">ソースが整数型の場合、変換は常に成功し、ソース値の最上位ビットを破棄するだけで構成されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-146">If the source is an integral type, the conversion always succeeds and simply consists of discarding the most significant bits of the source value.</span></span>

  * <span data-ttu-id="24e55-147">ソースが `Single`、`Double`、または `Decimal`の場合、変換は常に成功し、最も近い整数値に対するソース値の丸め処理が行われます。</span><span class="sxs-lookup"><span data-stu-id="24e55-147">If the source is `Single`, `Double`, or `Decimal`, the conversion always succeeds and simply consists of rounding the source value towards the nearest integral value.</span></span> <span data-ttu-id="24e55-148">ソース値が2つの整数値に均等に近い場合、値は常に、最下位桁の位置にある偶数の値に丸められます。</span><span class="sxs-lookup"><span data-stu-id="24e55-148">If the source value is equally close to two integral values, the value is always rounded to the value that has an even number in the least significant digit position.</span></span>

* <span data-ttu-id="24e55-149">`Double` から `Single`への変換では、`Double` 値は最も近い `Single` 値に丸められます。</span><span class="sxs-lookup"><span data-stu-id="24e55-149">For a conversion from `Double` to `Single`, the `Double` value is rounded to the nearest `Single` value.</span></span> <span data-ttu-id="24e55-150">`Double` 値が小さすぎて `Single`として表現できない場合、結果は正のゼロまたは負のゼロになります。</span><span class="sxs-lookup"><span data-stu-id="24e55-150">If the `Double` value is too small to represent as a `Single`, the result becomes positive zero or negative zero.</span></span> <span data-ttu-id="24e55-151">`Double` 値が大きすぎて `Single`として表現できない場合、結果は正の無限大または負の無限大になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-151">If the `Double` value is too large to represent as a `Single`, the result becomes positive infinity or negative infinity.</span></span> <span data-ttu-id="24e55-152">`Double` 値が `NaN`場合は、結果も `NaN`ます。</span><span class="sxs-lookup"><span data-stu-id="24e55-152">If the `Double` value is `NaN`, the result is also `NaN`.</span></span>

* <span data-ttu-id="24e55-153">`Single` または `Double` から `Decimal`への変換では、変換元の値が `Decimal` 表現に変換され、必要に応じて28桁の小数点以下の最も近い数値に丸められます。</span><span class="sxs-lookup"><span data-stu-id="24e55-153">For a conversion from `Single` or `Double` to `Decimal`, the source value is converted to `Decimal` representation and rounded to the nearest number after the 28th decimal place if required.</span></span> <span data-ttu-id="24e55-154">ソース値が小さすぎて `Decimal`として表現できない場合、結果は0になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-154">If the source value is too small to represent as a `Decimal`, the result becomes zero.</span></span> <span data-ttu-id="24e55-155">ソース値が `NaN`、無限大、または大きすぎて `Decimal`として表現できない場合は、`System.OverflowException` 例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="24e55-155">If the source value is `NaN`, infinity, or too large to represent as a `Decimal`, a `System.OverflowException` exception is thrown.</span></span>

* <span data-ttu-id="24e55-156">`Double` から `Single`への変換では、`Double` 値は最も近い `Single` 値に丸められます。</span><span class="sxs-lookup"><span data-stu-id="24e55-156">For a conversion from `Double` to `Single`, the `Double` value is rounded to the nearest `Single` value.</span></span> <span data-ttu-id="24e55-157">`Double` 値が小さすぎて `Single`として表現できない場合、結果は正のゼロまたは負のゼロになります。</span><span class="sxs-lookup"><span data-stu-id="24e55-157">If the `Double` value is too small to represent as a `Single`, the result becomes positive zero or negative zero.</span></span> <span data-ttu-id="24e55-158">`Double` 値が大きすぎて `Single`として表現できない場合、結果は正の無限大または負の無限大になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-158">If the `Double` value is too large to represent as a `Single`, the result becomes positive infinity or negative infinity.</span></span> <span data-ttu-id="24e55-159">`Double` 値が `NaN`場合は、結果も `NaN`ます。</span><span class="sxs-lookup"><span data-stu-id="24e55-159">If the `Double` value is `NaN`, the result is also `NaN`.</span></span>

## <a name="reference-conversions"></a><span data-ttu-id="24e55-160">参照変換</span><span class="sxs-lookup"><span data-stu-id="24e55-160">Reference Conversions</span></span>

<span data-ttu-id="24e55-161">参照型は基本型に変換できますが、その逆も可能です。</span><span class="sxs-lookup"><span data-stu-id="24e55-161">Reference types may be converted to a base type, and vice versa.</span></span> <span data-ttu-id="24e55-162">変換される値が null 値、派生型自体、またはより派生型である場合、基本型からより派生型への変換は実行時にのみ成功します。</span><span class="sxs-lookup"><span data-stu-id="24e55-162">Conversions from a base type to a more derived type only succeed at run time if the value being converted is a null value, the derived type itself, or a more derived type.</span></span>

<span data-ttu-id="24e55-163">クラス型とインターフェイス型は、任意のインターフェイス型との間でキャストできます。</span><span class="sxs-lookup"><span data-stu-id="24e55-163">Class and interface types can be cast to and from any interface type.</span></span> <span data-ttu-id="24e55-164">型とインターフェイス型の間の変換は、関連する実際の型が継承または実装関係を持つ場合にのみ、実行時に成功します。</span><span class="sxs-lookup"><span data-stu-id="24e55-164">Conversions between a type and an interface type only succeed at run time if the actual types involved have an inheritance or implementation relationship.</span></span> <span data-ttu-id="24e55-165">インターフェイス型には常に `Object`から派生した型のインスタンスが含まれているので、インターフェイス型は常に `Object`との間でキャストできます。</span><span class="sxs-lookup"><span data-stu-id="24e55-165">Because an interface type will always contain an instance of a type that derives from `Object`, an interface type can also always be cast to and from `Object`.</span></span>

<span data-ttu-id="24e55-166">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="24e55-166">__Note.__</span></span> <span data-ttu-id="24e55-167">COM クラスを表すクラスには、実行時までわからないインターフェイスの実装が含まれている可能性があるため、`NotInheritable` クラスを、実装していないインターフェイスとの間で変換することはできません。</span><span class="sxs-lookup"><span data-stu-id="24e55-167">It is not an error to convert a `NotInheritable` classes to and from interfaces that it does not implement because classes that represent COM classes may have interface implementations that are not known until run time.</span></span> 

<span data-ttu-id="24e55-168">実行時に参照変換が失敗した場合は、`System.InvalidCastException` 例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="24e55-168">If a reference conversion fails at run time, a `System.InvalidCastException` exception is thrown.</span></span>

### <a name="reference-variance-conversions"></a><span data-ttu-id="24e55-169">参照の分散変換</span><span class="sxs-lookup"><span data-stu-id="24e55-169">Reference Variance Conversions</span></span>

<span data-ttu-id="24e55-170">ジェネリックインターフェイスまたはデリゲートは、型の互換性のあるバリアント間の変換を可能にするバリアント型パラメーターを持つことができます。</span><span class="sxs-lookup"><span data-stu-id="24e55-170">Generic interfaces or delegates may have variant type parameters that allow conversions between compatible variants of the type.</span></span> <span data-ttu-id="24e55-171">したがって、実行時に、クラス型またはインターフェイス型から、派生したインターフェイス型またはを実装するインターフェイス型との互換性を持つインターフェイス型への変換は成功します。</span><span class="sxs-lookup"><span data-stu-id="24e55-171">Therefore, at runtime a conversion from a class type or an interface type to an interface type that is variant compatible with an interface type it inherits from or implements will succeed.</span></span> <span data-ttu-id="24e55-172">同様に、デリゲート型は、バリアント互換のデリゲート型との間でキャストできます。</span><span class="sxs-lookup"><span data-stu-id="24e55-172">Similarly, delegate types can be cast to and from variant compatible delegate types.</span></span> <span data-ttu-id="24e55-173">たとえば、デリゲート型</span><span class="sxs-lookup"><span data-stu-id="24e55-173">For example, the delegate type</span></span>

```vb
Delegate Function F(Of In A, Out R)(a As A) As R
```

<span data-ttu-id="24e55-174">`F(Of Object, Integer)` から `F(Of String, Integer)`への変換を許可します。</span><span class="sxs-lookup"><span data-stu-id="24e55-174">would allow a conversion from `F(Of Object, Integer)` to `F(Of String, Integer)`.</span></span> <span data-ttu-id="24e55-175">つまり、`Object` を受け取るデリゲート `F` は、`String`を受け取るデリゲート `F` として安全に使用される場合があります。</span><span class="sxs-lookup"><span data-stu-id="24e55-175">That is, a delegate `F` which takes `Object` may be safely used as a delegate `F` which takes `String`.</span></span> <span data-ttu-id="24e55-176">デリゲートが呼び出されると、ターゲットメソッドはオブジェクトを想定し、文字列はオブジェクトになります。</span><span class="sxs-lookup"><span data-stu-id="24e55-176">When the delegate is invoked, the target method will be expecting an object, and a string is an object.</span></span>

<span data-ttu-id="24e55-177">一般的なデリゲートまたはインターフェイス型 `S(Of S1,...,Sn)` は、次の場合に `T(Of T1,...,Tn)` ジェネリックインターフェイスまたはデリゲート型との*バリアント互換*と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="24e55-177">A generic delegate or interface type `S(Of S1,...,Sn)` is said to be *variant compatible* with a generic interface or delegate type `T(Of T1,...,Tn)` if:</span></span>

* <span data-ttu-id="24e55-178">`S` と `T` は、両方とも同じジェネリック型 `U(Of U1,...,Un)`から構築されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-178">`S` and `T` are both constructed from the same generic type `U(Of U1,...,Un)`.</span></span>

* <span data-ttu-id="24e55-179">`Ux`型パラメーターごとに次のように入力します。</span><span class="sxs-lookup"><span data-stu-id="24e55-179">For each type parameter `Ux`:</span></span>

  * <span data-ttu-id="24e55-180">型パラメーターが variance なしで宣言されている場合、`Sx` と `Tx` は同じ型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="24e55-180">If the type parameter was declared without variance then `Sx` and `Tx` must be the same type.</span></span>

  * <span data-ttu-id="24e55-181">型パラメーターが `In` 宣言されている場合は、`Sx` から `Tx`に拡張 id、既定値、参照、配列、または型パラメーターを変換する必要があります。</span><span class="sxs-lookup"><span data-stu-id="24e55-181">If the type parameter was declared `In` then there must be a widening identity, default, reference, array, or type parameter conversion from `Sx` to `Tx`.</span></span>

  * <span data-ttu-id="24e55-182">型パラメーターが `Out` 宣言されている場合は、`Tx` から `Sx`に拡張 id、既定値、参照、配列、または型パラメーターを変換する必要があります。</span><span class="sxs-lookup"><span data-stu-id="24e55-182">If the type parameter was declared `Out` then there must be a widening identity, default, reference, array, or type parameter conversion from `Tx` to `Sx`.</span></span>

<span data-ttu-id="24e55-183">クラスからバリアント型パラメーターを持つジェネリックインターフェイスに変換するときに、クラスが複数のバリアント互換インターフェイスを実装する場合、バリアント以外の変換がない場合、変換はあいまいです。</span><span class="sxs-lookup"><span data-stu-id="24e55-183">When converting from a class to a generic interface with variant type parameters, if the class implements more than one variant compatible interface the conversion is ambiguous if there is not a non-variant conversion.</span></span> <span data-ttu-id="24e55-184">例 :</span><span class="sxs-lookup"><span data-stu-id="24e55-184">For example:</span></span>

```vb
Class Base
End Class

Class Derived1
    Inherits Base
End Class

Class Derived2
    Inherits Base
End Class

Class OneAndTwo
    Implements IEnumerable(Of Derived1)
    Implements IEnumerable(Of Derived2)
End Class

Class BaseAndOneAndTwo
    Implements IEnumerable(Of Base)
    Implements IEnumerable(Of Derived1)
    Implements IEnumerable(Of Derived2)
End Class

Module Test
    Sub Main()
        ' Error: conversion is ambiguous
        Dim x As IEnumerable(Of Base) = New OneAndTwo()

        ' OK, will pick up the direct implementation of IEnumerable(Of Base)
        Dim y as IEnumerable(Of Base) = New BaseAndOneAndTwo()
    End Sub
End Module
```

### <a name="anonymous-delegate-conversions"></a><span data-ttu-id="24e55-185">匿名デリゲートの変換</span><span class="sxs-lookup"><span data-stu-id="24e55-185">Anonymous Delegate Conversions</span></span>

<span data-ttu-id="24e55-186">ラムダメソッドとして分類された式が、対象の型が存在しないコンテキスト (`Dim x = Function(a As Integer, b As Integer) a + b`など) の値として再分類される場合、またはターゲット型がデリゲート型ではない場合、結果として得られる式の型は、ラムダメソッドのシグネチャと等価な匿名デリゲート型になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-186">When an expression classified as a lambda method is reclassified as a value in a context where there is no target type (for example, `Dim x = Function(a As Integer, b As Integer) a + b`), or where the target type is not a delegate type, the type of the resulting expression is an anonymous delegate type equivalent to the signature of the lambda method.</span></span> <span data-ttu-id="24e55-187">この匿名デリゲート型には、互換性のある任意のデリゲート型への変換が含まれています。互換性のあるデリゲート型は、匿名デリゲート型の `Invoke` メソッドをパラメーターとして持つデリゲート作成式を使用して作成できる任意のデリゲート型です。</span><span class="sxs-lookup"><span data-stu-id="24e55-187">This anonymous delegate type has a conversion to any compatible delegate type: a compatible delegate type is any delegate type that can be created using a delegate creation expression with the anonymous delegate type's `Invoke` method as a parameter.</span></span> <span data-ttu-id="24e55-188">例 :</span><span class="sxs-lookup"><span data-stu-id="24e55-188">For example:</span></span>

```vb
' Anonymous delegate type similar to Func(Of Object, Object, Object)
Dim x = Function(x, y) x + y

' OK because delegate type is compatible
Dim y As Func(Of Integer, Integer, Integer) = x
```

<span data-ttu-id="24e55-189">型 `System.Delegate` と `System.MulticastDelegate` は、デリゲート型とは異なります (すべてのデリゲート型が継承していますが)。</span><span class="sxs-lookup"><span data-stu-id="24e55-189">Note that the types `System.Delegate` and `System.MulticastDelegate` are not themselves considered delegate types (even though all delegate types inherit from them).</span></span> <span data-ttu-id="24e55-190">また、匿名デリゲート型から互換性のあるデリゲート型への変換は、参照変換ではないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="24e55-190">Also, note that the conversion from anonymous delegate type to a compatible delegate type is not a reference conversion.</span></span>

## <a name="array-conversions"></a><span data-ttu-id="24e55-191">配列変換</span><span class="sxs-lookup"><span data-stu-id="24e55-191">Array Conversions</span></span>

<span data-ttu-id="24e55-192">配列に定義されている変換に加えて、参照型であるという事実によって、配列に対していくつかの特殊な変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="24e55-192">Besides the conversions that are defined on arrays by virtue of the fact that they are reference types, several special conversions exist for arrays.</span></span>

<span data-ttu-id="24e55-193">`A` と `B`の2つの型では、それらが参照型であるか、値型として知られていない型パラメーターの両方であり、`A` に参照、配列、または型パラメーターの `B`への変換がある場合、`A` 型の配列から、同じ順位を持つ `B` 型の配列への変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="24e55-193">For any two types `A` and `B`, if they are both reference types or type parameters not known to be value types, and if `A` has a reference, array, or type parameter conversion to `B`, a conversion exists from an array of type `A` to an array of type `B` with the same rank.</span></span> <span data-ttu-id="24e55-194">このリレーションシップは、*配列の共変性*と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="24e55-194">This relationship is known as *array covariance*.</span></span> <span data-ttu-id="24e55-195">配列の共変性は、要素型が `B` の配列の要素が、実際には要素型が `A`配列の要素であることを意味します。これは、`A` と `B` の両方が参照型であり、`B` に参照変換または配列変換が含まれていることを意味します。`A`</span><span class="sxs-lookup"><span data-stu-id="24e55-195">Array covariance in particular means that an element of an array whose element type is `B` may actually be an element of an array whose element type is `A`, provided that both `A` and `B` are reference types and that `B` has a reference conversion or array conversion to `A`.</span></span> <span data-ttu-id="24e55-196">次の例では、`F` の2回目の呼び出しによって `System.ArrayTypeMismatchException` 例外がスローされます。これは、`b` の実際の要素の型が `String`であり、`Object`ではないためです。</span><span class="sxs-lookup"><span data-stu-id="24e55-196">In the following example, the second invocation of `F` causes a `System.ArrayTypeMismatchException` exception to be thrown because the actual element type of `b` is `String`, not `Object`:</span></span>

```vb
Module Test
    Sub F(ByRef x As Object)
    End Sub

    Sub Main()
        Dim a(10) As Object
        Dim b() As Object = New String(10) {}
        F(a(0)) ' OK.
        F(b(1)) ' Not allowed: System.ArrayTypeMismatchException.
   End Sub
End Module
```

<span data-ttu-id="24e55-197">配列の共変性により、参照型の配列の要素への代入には、配列要素に割り当てられている値が実際に許可されている型であることを保証するランタイムチェックが含まれます。</span><span class="sxs-lookup"><span data-stu-id="24e55-197">Because of array covariance, assignments to elements of reference type arrays include a run-time check that ensures that the value being assigned to the array element is actually of a permitted type.</span></span>

```vb
Module Test
    Sub Fill(array() As Object, index As Integer, count As Integer, _
            value As Object)
        Dim i As Integer

        For i = index To (index + count) - 1
            array(i) = value
        Next i
    End Sub

    Sub Main()
        Dim strings(100) As String

        Fill(strings, 0, 101, "Undefined")
        Fill(strings, 0, 10, Nothing)
        Fill(strings, 91, 10, 0)
    End Sub
End Module
```

<span data-ttu-id="24e55-198">この例では、メソッド `Fill` に `array(i)` するための割り当てには、変数 `value` によって参照されるオブジェクトが `Nothing` であるか、配列 `array`の実際の要素型と互換性のある型のインスタンスであることを保証するランタイムチェックが暗黙的に含まれています。</span><span class="sxs-lookup"><span data-stu-id="24e55-198">In this example, the assignment to `array(i)` in method `Fill` implicitly includes a run-time check that ensures that the object referenced by the variable `value` is either `Nothing` or an instance of a type that is compatible with the actual element type of array `array`.</span></span> <span data-ttu-id="24e55-199">メソッド `Main`では、`Fill` メソッドの最初の2回の呼び出しは成功しますが、3回目の呼び出しでは、`array(i)`への最初の割り当てを実行したときに `System.ArrayTypeMismatchException` 例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="24e55-199">In method `Main`, the first two invocations of method `Fill` succeed, but the third invocation causes a `System.ArrayTypeMismatchException` exception to be thrown upon executing the first assignment to `array(i)`.</span></span> <span data-ttu-id="24e55-200">この例外は、`Integer` を `String` 配列に格納できないために発生します。</span><span class="sxs-lookup"><span data-stu-id="24e55-200">The exception occurs because an `Integer` cannot be stored in a `String` array.</span></span>

<span data-ttu-id="24e55-201">配列要素の型の1つが、実行時に型が値型である型パラメーターである場合は、`System.InvalidCastException` 例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="24e55-201">If one of the array element types is a type parameter whose type turns out to be a value type at runtime, a `System.InvalidCastException` exception will be thrown.</span></span> <span data-ttu-id="24e55-202">例 :</span><span class="sxs-lookup"><span data-stu-id="24e55-202">For example:</span></span>

```vb
Module Test
    Sub F(Of T As U, U)(x() As T)
        Dim y() As U = x
    End Sub

    Sub Main()
        ' F will throw an exception because Integer() cannot be
        ' converted to Object()
        F(New Integer() { 1, 2, 3 })
    End Sub
End Module
```

<span data-ttu-id="24e55-203">また、配列のランクが同じ場合は、列挙型の配列と、列挙型の基になる型の配列または基になる型が同じ別の列挙型の配列の間にも、変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="24e55-203">Conversions also exist between an array of an enumerated type and an array of the enumerated type's underlying type or an array of another enumerated type with the same underlying type, provided the arrays have the same rank.</span></span>

```vb
Enum Color As Byte
    Red
    Green
    Blue
End Enum

Module Test
    Sub Main()
        Dim a(10) As Color
        Dim b() As Integer
        Dim c() As Byte

        b = a    ' Error: Integer is not the underlying type of Color
        c = a    ' OK
        a = c    ' OK
    End Sub
End Module
```

<span data-ttu-id="24e55-204">この例では、`Color` の配列は、`Byte`の配列、または `Color`の基になる型との間で変換されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-204">In this example, an array of `Color` is converted to and from an array of `Byte`, `Color`'s underlying type.</span></span> <span data-ttu-id="24e55-205">ただし、`Integer` が `Color`の基になる型ではないため、`Integer`の配列への変換はエラーになります。</span><span class="sxs-lookup"><span data-stu-id="24e55-205">The conversion to an array of `Integer`, however, will be an error because `Integer` is not the underlying type of `Color`.</span></span>

<span data-ttu-id="24e55-206">`A()` 型のランク1配列には、次のいずれかの条件を満たしている限り、`IList(Of B)`、`IReadOnlyList(Of B)`、`ICollection(Of B)`、`IReadOnlyCollection(Of B)`、`IEnumerable(Of B)` のコレクションインターフェイス型への配列変換も含まれます。`System.Collections.Generic`</span><span class="sxs-lookup"><span data-stu-id="24e55-206">A rank-1 array of type `A()` also has an array conversion to the collection interface types `IList(Of B)`, `IReadOnlyList(Of B)`, `ICollection(Of B)`, `IReadOnlyCollection(Of B)` and `IEnumerable(Of B)` found in `System.Collections.Generic`, so long as one of the following is true:</span></span>

- <span data-ttu-id="24e55-207">`A` と `B` は、値型として知られていない参照型または型パラメーターの両方を持ちます。および `A` には、`B`への拡大参照、配列、または型パラメーターの変換があります。もしくは</span><span class="sxs-lookup"><span data-stu-id="24e55-207">`A` and `B` are both reference types or type parameters not known to be value types; and `A` has a widening reference, array or type parameter conversion to `B`; or</span></span>
- <span data-ttu-id="24e55-208">`A` と `B` は両方とも同じ基になる型の列挙型です。もしくは</span><span class="sxs-lookup"><span data-stu-id="24e55-208">`A` and `B` are both enumerated types of the same underlying type; or</span></span>
- <span data-ttu-id="24e55-209">`A` と `B` のいずれかが列挙型であり、もう1つは基になる型です。</span><span class="sxs-lookup"><span data-stu-id="24e55-209">one of `A` and `B` is an enumerated type, and the other is its underlying type.</span></span>

<span data-ttu-id="24e55-210">任意のランクを持つ A 型の配列には、`System.Collections`で検出された非ジェネリックコレクションインターフェイス型 `IList`、`ICollection`、および `IEnumerable` への配列変換もあります。</span><span class="sxs-lookup"><span data-stu-id="24e55-210">Any array of type A with any rank also has an array conversion to the non-generic collection interface types `IList`, `ICollection` and `IEnumerable` found in `System.Collections`.</span></span>

<span data-ttu-id="24e55-211">`For Each`を使用するか、`GetEnumerator` メソッドを直接呼び出すことによって、結果のインターフェイスを反復処理することができます。</span><span class="sxs-lookup"><span data-stu-id="24e55-211">It is possible to iterate over the resulting interfaces using `For Each`, or through invoking the `GetEnumerator` methods directly.</span></span> <span data-ttu-id="24e55-212">ランク1の配列で `IList` または `ICollection`の汎用形式または非ジェネリック形式が変換された場合は、インデックスを使用して要素を取得することもできます。</span><span class="sxs-lookup"><span data-stu-id="24e55-212">In the case of rank-1 arrays converted generic or non-generic forms of `IList` or `ICollection`, it is also possible to get elements by index.</span></span> <span data-ttu-id="24e55-213">`IList`のジェネリックまたは非ジェネリック形式に変換されたランク1の配列の場合は、前に説明したのと同じランタイム配列の共変性のチェックに従って、インデックスを使用して要素を設定することもできます。</span><span class="sxs-lookup"><span data-stu-id="24e55-213">In the case of rank-1 arrays converted to generic or non-generic forms of `IList`, it is also possible to set elements by index, subject to the same runtime array covariance checks as described above.</span></span> <span data-ttu-id="24e55-214">他のすべてのインターフェイスメソッドの動作は、VB 言語仕様によって定義されていません。これは、基になるランタイムによって作成されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-214">The behavior of all other interface methods is undefined by the VB language specification; it is up to the underlying runtime.</span></span>

## <a name="value-type-conversions"></a><span data-ttu-id="24e55-215">値型の変換</span><span class="sxs-lookup"><span data-stu-id="24e55-215">Value Type Conversions</span></span>

<span data-ttu-id="24e55-216">値型の値は、その基本参照型または*ボックス*化と呼ばれるプロセスによって実装されるインターフェイス型のいずれかに変換できます。</span><span class="sxs-lookup"><span data-stu-id="24e55-216">A value type value can be converted to one of its base reference types or an interface type that it implements through a process called *boxing*.</span></span> <span data-ttu-id="24e55-217">値型の値がボックス化されている場合、値は .NET Framework ヒープ上に存在する場所からコピーされます。</span><span class="sxs-lookup"><span data-stu-id="24e55-217">When a value type value is boxed, the value is copied from the location where it lives onto the .NET Framework heap.</span></span> <span data-ttu-id="24e55-218">次に、ヒープ上のこの場所への参照が返され、参照型の変数に格納できます。</span><span class="sxs-lookup"><span data-stu-id="24e55-218">A reference to this location on the heap is then returned and can be stored in a reference type variable.</span></span> <span data-ttu-id="24e55-219">この参照は、値型の*ボックス*化されたインスタンスとも呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="24e55-219">This reference is also referred to as a *boxed* instance of the value type.</span></span> <span data-ttu-id="24e55-220">ボックス化されたインスタンスは、値型ではなく、参照型と同じセマンティクスを持ちます。</span><span class="sxs-lookup"><span data-stu-id="24e55-220">The boxed instance has the same semantics as a reference type instead of a value type.</span></span>

<span data-ttu-id="24e55-221">ボックス化された値型は、*ボックス化解除*と呼ばれるプロセスを使用して、元の値型に変換できます。</span><span class="sxs-lookup"><span data-stu-id="24e55-221">Boxed value types can be converted back to their original value type through a process called *unboxing*.</span></span> <span data-ttu-id="24e55-222">ボックス化された値型がボックス化解除されると、値がヒープから変数の場所にコピーされます。</span><span class="sxs-lookup"><span data-stu-id="24e55-222">When a boxed value type is unboxed, the value is copied from the heap into a variable location.</span></span> <span data-ttu-id="24e55-223">その時点から、値型であるかのように動作します。</span><span class="sxs-lookup"><span data-stu-id="24e55-223">From that point on, it behaves as if it was a value type.</span></span> <span data-ttu-id="24e55-224">値型のボックス化を解除するときは、値が null 値または値型のインスタンスである必要があります。</span><span class="sxs-lookup"><span data-stu-id="24e55-224">When unboxing a value type, the value must be a null value or an instance of the value type.</span></span> <span data-ttu-id="24e55-225">それ以外の場合は、`System.InvalidCastException` 例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="24e55-225">Otherwise a `System.InvalidCastException` exception is thrown.</span></span> <span data-ttu-id="24e55-226">値が列挙型のインスタンスである場合、その値は列挙型の基になる型または基になる型が同じ別の列挙型にボックス化解除することもできます。</span><span class="sxs-lookup"><span data-stu-id="24e55-226">If the value is an instance of an enumerated type, that value can also be unboxed to the enumerated type's underlying type or another enumerated type that has the same underlying type.</span></span> <span data-ttu-id="24e55-227">Null 値は、リテラル `Nothing`として扱われます。</span><span class="sxs-lookup"><span data-stu-id="24e55-227">A null value is treated as if it were the literal `Nothing`.</span></span>

<span data-ttu-id="24e55-228">Null 許容値型を適切にサポートするために、ボックス化とボックス化解除を行うときに、値型 `System.Nullable(Of T)` が特別に扱われます。</span><span class="sxs-lookup"><span data-stu-id="24e55-228">To support nullable value types well, the value type `System.Nullable(Of T)` is treated specially when doing boxing and unboxing.</span></span> <span data-ttu-id="24e55-229">`Nullable(Of T)` 型の値をボックス化すると `T` 型のボックス化された値になります。値の `HasValue` プロパティが `True` の場合、または値の `Nothing` プロパティが `HasValue` の場合は `False`の値になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-229">Boxing a value of type `Nullable(Of T)` results in a boxed value of type `T` if the value's `HasValue` property is `True` or a value of `Nothing` if the value's `HasValue` property is `False`.</span></span> <span data-ttu-id="24e55-230">`T` 型の値のボックス化を解除すると、`Value` プロパティがボックス化された値であり、`HasValue` プロパティが `True`である `Nullable(Of T)` インスタンスの `Nullable(Of T)` 結果になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-230">Unboxing a value of type `T` to `Nullable(Of T)` results in an instance of `Nullable(Of T)` whose `Value` property is the boxed value and whose `HasValue` property is `True`.</span></span> <span data-ttu-id="24e55-231">`Nothing` 値は、任意の `T` に対して `Nullable(Of T)` するようにボックス化を解除することができ、結果は `HasValue` プロパティが `False`の値になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-231">The value `Nothing` can be unboxed to `Nullable(Of T)` for any `T` and results in a value whose `HasValue` property is `False`.</span></span> <span data-ttu-id="24e55-232">ボックス化された値型は参照型と同じように動作するため、同じ値への複数の参照を作成することができます。</span><span class="sxs-lookup"><span data-stu-id="24e55-232">Because boxed value types behave like reference types, it is possible to create multiple references to the same value.</span></span> <span data-ttu-id="24e55-233">プリミティブ型と列挙型では、これらの型のインスタンスが*不変*であるため、これは関係ありません。</span><span class="sxs-lookup"><span data-stu-id="24e55-233">For the primitive types and enumerated types, this is irrelevant because instances of those types are *immutable*.</span></span> <span data-ttu-id="24e55-234">つまり、これらの型のボックス化されたインスタンスを変更することはできないため、同じ値への複数の参照があるという事実を観察することはできません。</span><span class="sxs-lookup"><span data-stu-id="24e55-234">That is, it is not possible to modify a boxed instance of those types, so it is not possible to observe the fact that there are multiple references to the same value.</span></span>

<span data-ttu-id="24e55-235">一方、構造体は、そのインスタンス変数がアクセス可能である場合、またはそのメソッドやプロパティがインスタンス変数を変更する場合に、変更可能な場合があります。</span><span class="sxs-lookup"><span data-stu-id="24e55-235">Structures, on the other hand, may be mutable if its instance variables are accessible or if its methods or properties modify its instance variables.</span></span> <span data-ttu-id="24e55-236">ボックス化された構造体への1つの参照を使用して構造体を変更すると、ボックス化された構造体へのすべての参照に変更が表示されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-236">If one reference to a boxed structure is used to modify the structure, then all references to the boxed structure will see the change.</span></span> <span data-ttu-id="24e55-237">この結果は予期しない結果になる可能性があるため、`Object` として型指定された値がある場所から別の場所にコピーされると、参照がコピーされるだけでなく、ヒープ上で自動的に複製されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-237">Because this result may be unexpected, when a value typed as `Object` is copied from one location to another boxed value types will automatically be cloned on the heap instead of merely having their references copied.</span></span> <span data-ttu-id="24e55-238">例 :</span><span class="sxs-lookup"><span data-stu-id="24e55-238">For example:</span></span>

```vb
Class Class1
    Public Value As Integer = 0
End Class

Structure Struct1
    Public Value As Integer
End Structure

Module Test
    Sub Main()
        Dim val1 As Object = New Struct1()
        Dim val2 As Object = val1

        val2.Value = 123

        Dim ref1 As Object = New Class1()
        Dim ref2 As Object = ref1

        ref2.Value = 123

        Console.WriteLine("Values: " & val1.Value & ", " & val2.Value)
        Console.WriteLine("Refs: " & ref1.Value & ", " & ref2.Value)
    End Sub
End Module
```

<span data-ttu-id="24e55-239">プログラムの出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="24e55-239">The output of the program is:</span></span>

```console
Values: 0, 123
Refs: 123, 123
```

<span data-ttu-id="24e55-240">ローカル変数 `val2` のフィールドへの割り当ては、ローカル `val1` 変数のフィールドには影響しません。これは、ボックス化された `Struct1` が `val2`に割り当てられたときに、値のコピーが作成されたためです。</span><span class="sxs-lookup"><span data-stu-id="24e55-240">The assignment to the field of the local variable `val2` does not impact the field of the local variable `val1` because when the boxed `Struct1` was assigned to `val2`, a copy of the value was made.</span></span> <span data-ttu-id="24e55-241">これに対して、割り当て `ref2.Value = 123` は、`ref1` と `ref2` の両方が参照するオブジェクトに影響します。</span><span class="sxs-lookup"><span data-stu-id="24e55-241">In contrast, the assignment `ref2.Value = 123` affects the object that both `ref1` and `ref2` references.</span></span>

<span data-ttu-id="24e55-242">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="24e55-242">__Note.__</span></span> <span data-ttu-id="24e55-243">`System.ValueType`から遅延バインドできないため、`System.ValueType` として入力されたボックス化された構造体の場合、構造のコピーは実行されません。</span><span class="sxs-lookup"><span data-stu-id="24e55-243">Structure copying is not done for boxed structures typed as `System.ValueType` because it is not possible to late bind off of `System.ValueType`.</span></span>

<span data-ttu-id="24e55-244">ルールには、ボックス化された値の型が割り当て時にコピーされるという例外が1つあります。</span><span class="sxs-lookup"><span data-stu-id="24e55-244">There is one exception to the rule that boxed value types will be copied on assignment.</span></span> <span data-ttu-id="24e55-245">ボックス化された値型参照が別の型に格納されている場合、内部参照はコピーされません。</span><span class="sxs-lookup"><span data-stu-id="24e55-245">If a boxed value type reference is stored within another type, the inner reference will not be copied.</span></span> <span data-ttu-id="24e55-246">例 :</span><span class="sxs-lookup"><span data-stu-id="24e55-246">For example:</span></span>

```vb
Structure Struct1
    Public Value As Object
End Structure

Module Test
    Sub Main()
        Dim val1 As Struct1
        Dim val2 As Struct1

        val1.Value = New Struct1()
        val1.Value.Value = 10

        val2 = val1
        val2.Value.Value = 123
        Console.WriteLine("Values: " & val1.Value.Value & ", " & _
            val2.Value.Value)
    End Sub
End Module
```

<span data-ttu-id="24e55-247">プログラムの出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="24e55-247">The output of the program is:</span></span>

```console
Values: 123, 123
```

<span data-ttu-id="24e55-248">これは、値をコピーするときに、ボックス化された内部の値がコピーされないためです。</span><span class="sxs-lookup"><span data-stu-id="24e55-248">This is because the inner boxed value is not copied when the value is copied.</span></span> <span data-ttu-id="24e55-249">したがって、`val1.Value` と `val2.Value` の両方に、同じボックス化された値型への参照があります。</span><span class="sxs-lookup"><span data-stu-id="24e55-249">Thus, both `val1.Value` and `val2.Value` have a reference to the same boxed value type.</span></span>

<span data-ttu-id="24e55-250">__付箋.__</span><span class="sxs-lookup"><span data-stu-id="24e55-250">__Note.__</span></span> <span data-ttu-id="24e55-251">内部のボックス化された値型がコピーされないという事実は、.NET 型システムの制限であり、`Object` コピーされた型の値が非常に高価になるたびに、すべての内部のボックス化された値型がコピーされるようにします。</span><span class="sxs-lookup"><span data-stu-id="24e55-251">The fact that inner boxed value types are not copied is a limitation of the .NET type system -- to ensure that all inner boxed value types were copied whenever a value of type `Object` was copied would be prohibitively expensive.</span></span>

<span data-ttu-id="24e55-252">既に説明したように、ボックス化された値型は、元の型にのみボックス化を解除できます。</span><span class="sxs-lookup"><span data-stu-id="24e55-252">As previously described, boxed value types can only be unboxed to their original type.</span></span> <span data-ttu-id="24e55-253">ただし、ボックス化されたプリミティブ型は、`Object`として型指定されると、特別に扱われます。</span><span class="sxs-lookup"><span data-stu-id="24e55-253">Boxed primitive types, however, are treated specially when typed as `Object`.</span></span> <span data-ttu-id="24e55-254">これらは、変換先の他のプリミティブ型に変換できます。</span><span class="sxs-lookup"><span data-stu-id="24e55-254">They can be converted to any other primitive type that they have a conversion to.</span></span> <span data-ttu-id="24e55-255">例 :</span><span class="sxs-lookup"><span data-stu-id="24e55-255">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim o As Object = 5
        Dim b As Byte = CByte(o)  ' Legal
        Console.WriteLine(b) ' Prints 5
    End Sub
End Module
```

<span data-ttu-id="24e55-256">通常、ボックス化された `Integer` 値 `5` を `Byte` 変数にボックス化解除できませんでした。</span><span class="sxs-lookup"><span data-stu-id="24e55-256">Normally, the boxed `Integer` value `5` could not be unboxed into a `Byte` variable.</span></span> <span data-ttu-id="24e55-257">ただし、`Integer` と `Byte` はプリミティブ型であり、変換があるため、変換は許可されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-257">However, because `Integer` and `Byte` are primitive types and have a conversion, the conversion is allowed.</span></span>

<span data-ttu-id="24e55-258">値型をインターフェイスに変換することは、インターフェイスに制約される汎用引数とは異なることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="24e55-258">It is important to note that converting a value type to an interface is different than a generic argument constrained to an interface.</span></span> <span data-ttu-id="24e55-259">制約された型パラメーターのインターフェイスメンバーにアクセスする (または `Object`のメソッドを呼び出す) 場合、ボックス化は、値型がインターフェイスに変換され、インターフェイスメンバーにアクセスするときと同様に発生します。</span><span class="sxs-lookup"><span data-stu-id="24e55-259">When accessing interface members on a constrained type parameter (or calling methods on `Object`), boxing does not occur as it does when a value type is converted to an interface and an interface member is accessed.</span></span> <span data-ttu-id="24e55-260">たとえば、インターフェイス `ICounter` に、値の変更に使用できるメソッド `Increment` が含まれているとします。</span><span class="sxs-lookup"><span data-stu-id="24e55-260">For example, suppose an interface `ICounter` contains a method `Increment` which can be used to modify a value.</span></span> <span data-ttu-id="24e55-261">`ICounter` が制約として使用されている場合、`Increment` メソッドの実装は、ボックス化されたコピーではなく、`Increment` が呼び出された変数への参照を使用して呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-261">If `ICounter` is used as a constraint, the implementation of the `Increment` method is called with a reference to the variable that `Increment` was called on, not a boxed copy:</span></span>

```vb
Interface ICounter
    Sub Increment()
    ReadOnly Property Value() As Integer
End Interface

Structure Counter
    Implements ICounter

    Dim _value As Integer

    Property Value() As Integer Implements ICounter.Value
        Get
            Return _value
        End Get
    End Property

    Sub Increment() Implements ICounter.Increment
       value += 1
    End Sub
End Structure

Module Test
      Sub Test(Of T As ICounter)(x As T)
         Console.WriteLine(x.value)
         x.Increment()                     ' Modify x
         Console.WriteLine(x.value)
         CType(x, ICounter).Increment()    ' Modify boxed copy of x
         Console.WriteLine(x.value)
      End Sub

      Sub Main()
         Dim x As Counter
         Test(x)
      End Sub
End Module
```

<span data-ttu-id="24e55-262">`Increment` の最初の呼び出しでは `x`変数の値が変更されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-262">The first call to `Increment` modifies the value in the variable `x`.</span></span> <span data-ttu-id="24e55-263">これは、`x`のボックス化されたコピーの値を変更する `Increment`への2回目の呼び出しとは同じではありません。</span><span class="sxs-lookup"><span data-stu-id="24e55-263">This is not equivalent to the second call to `Increment`, which modifies the value in a boxed copy of `x`.</span></span> <span data-ttu-id="24e55-264">このため、プログラムの出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="24e55-264">Thus, the output of the program is:</span></span>

```console
0
1
1
```

### <a name="nullable-value-type-conversions"></a><span data-ttu-id="24e55-265">Null 許容値型の変換</span><span class="sxs-lookup"><span data-stu-id="24e55-265">Nullable Value Type Conversions</span></span>

<span data-ttu-id="24e55-266">`T` 値型は、型の null 許容バージョンとの間で変換を行うことができます `T?`です。</span><span class="sxs-lookup"><span data-stu-id="24e55-266">A value type `T` can convert to and from the nullable version of the type, `T?`.</span></span> <span data-ttu-id="24e55-267">変換される値が `Nothing`場合、`T?` から `T` への変換は `System.InvalidOperationException` 例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="24e55-267">The conversion from `T?` to `T` throws a `System.InvalidOperationException` exception if the value being converted is `Nothing`.</span></span> <span data-ttu-id="24e55-268">また、`T` に `S`への組み込みの変換がある場合、`T?` は型 `S` に変換されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-268">Also, `T?` has a conversion to a type `S` if `T` has an intrinsic conversion to `S`.</span></span> <span data-ttu-id="24e55-269">`S` が値型の場合、`T?` と `S?`の間には次の組み込み変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="24e55-269">And if `S` is a value type, then the following intrinsic conversions exist between `T?` and `S?`:</span></span>

* <span data-ttu-id="24e55-270">`T?` から `S?`への同じ分類 (縮小または拡大) の変換。</span><span class="sxs-lookup"><span data-stu-id="24e55-270">A conversion of the same classification (narrowing or widening) from `T?` to `S?`.</span></span>

* <span data-ttu-id="24e55-271">`T` から `S?`への同じ分類 (縮小または拡大) の変換。</span><span class="sxs-lookup"><span data-stu-id="24e55-271">A conversion of the same classification (narrowing or widening) from `T` to `S?`.</span></span>

* <span data-ttu-id="24e55-272">`S?` から `T`への縮小変換。</span><span class="sxs-lookup"><span data-stu-id="24e55-272">A narrowing conversion from `S?` to `T`.</span></span>

<span data-ttu-id="24e55-273">たとえば、組み込みの拡大変換は `Integer?` から `Long?` に存在します。これは、組み込みの拡大変換が `Integer` から `Long`に存在するためです。</span><span class="sxs-lookup"><span data-stu-id="24e55-273">For example, an intrinsic widening conversion exists from `Integer?` to `Long?` because an intrinsic widening conversion exists from `Integer` to `Long`:</span></span>

```vb
Dim i As Integer? = 10
Dim l As Long? = i
```

<span data-ttu-id="24e55-274">`T?` から `S?`に変換する場合、`T?` の値が `Nothing`の場合、`S?` の値は `Nothing`になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-274">When converting from `T?` to `S?`, if the value of `T?` is `Nothing`, then the value of `S?` will be `Nothing`.</span></span> <span data-ttu-id="24e55-275">`S?` から `T` または `T?` から `S`に変換する場合、`T?` または `S?` の値が `Nothing`の場合は、`System.InvalidCastException` 例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="24e55-275">When converting from `S?` to `T` or `T?` to `S`, if the value of `T?` or `S?` is `Nothing`, a `System.InvalidCastException` exception will be thrown.</span></span>

<span data-ttu-id="24e55-276">基になる型 `System.Nullable(Of T)`の動作により、null 許容型の値型 `T?` がボックス化された場合、結果は `T?`型のボックス化された値ではなく、`T`型のボックス化された値になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-276">Because of the behavior of the underlying type `System.Nullable(Of T)`, when a nullable value type `T?` is boxed, the result is a boxed value of type `T`, not a boxed value of type `T?`.</span></span> <span data-ttu-id="24e55-277">逆に、null 許容値型 `T?`にボックス化を解除すると、値は `System.Nullable(Of T)`によってラップされ、`Nothing` は `T?`型の null 値にボックス化解除されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-277">And, conversely, when unboxing to a nullable value type `T?`, the value will be wrapped by `System.Nullable(Of T)`, and `Nothing` will be unboxed to a null value of type `T?`.</span></span> <span data-ttu-id="24e55-278">例 :</span><span class="sxs-lookup"><span data-stu-id="24e55-278">For example:</span></span>

```vb
Dim i1? As Integer = Nothing
Dim o1 As Object = i1

Console.WriteLine(o1 Is Nothing)                    ' Will print True
o1 = 10
i1 = CType(o1, Integer?)
Console.WriteLine(i1)                               ' Will print 10
```

<span data-ttu-id="24e55-279">この動作の副作用として、`T`のすべてのインターフェイスを実装するために null 許容型 `T?` が表示されます。これは、値型をインターフェイスに変換するには、型をボックス化する必要があるためです。</span><span class="sxs-lookup"><span data-stu-id="24e55-279">A side effect of this behavior is that a nullable value type `T?` appears to implement all of the interfaces of `T`, because converting a value type to an interface requires the type to be boxed.</span></span> <span data-ttu-id="24e55-280">その結果、`T?` は、`T` 変換可能なすべてのインターフェイスに変換できます。</span><span class="sxs-lookup"><span data-stu-id="24e55-280">As a result, `T?` is convertible to all the interfaces that `T` is convertible to.</span></span> <span data-ttu-id="24e55-281">ただし、null 許容型 `T?` では、ジェネリック制約チェックまたはリフレクションのために、実際には `T` のインターフェイスが実装されていないことに注意する必要があります。</span><span class="sxs-lookup"><span data-stu-id="24e55-281">It is important to note, however, that a nullable value type `T?` does not actually implement the interfaces of `T` for the purposes of generic constraint checking or reflection.</span></span> <span data-ttu-id="24e55-282">例 :</span><span class="sxs-lookup"><span data-stu-id="24e55-282">For example:</span></span>

```vb
Interface I1
End Interface

Structure T1
    Implements I1
    ...
End Structure

Module Test
    Sub M1(Of T As I1)(ByVal x As T)
    End Sub

    Sub Main()
        Dim x? As T1 = Nothing
        Dim y As I1 = x                ' Valid
        M1(x)                          ' Error: x? does not satisfy I1 constraint
    End Sub
End Module
```

## <a name="string-conversions"></a><span data-ttu-id="24e55-283">文字列の変換</span><span class="sxs-lookup"><span data-stu-id="24e55-283">String Conversions</span></span>

<span data-ttu-id="24e55-284">`Char` を `String` に変換すると、最初の文字が文字値である文字列が生成されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-284">Converting `Char` into `String` results in a string whose first character is the character value.</span></span> <span data-ttu-id="24e55-285">`String` を `Char` に変換すると、文字列の最初の文字が値である文字が返されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-285">Converting `String` into `Char` results in a character whose value is the first character of the string.</span></span> <span data-ttu-id="24e55-286">`Char` の配列を `String` に変換すると、配列の要素を文字として持つ文字列が生成されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-286">Converting an array of `Char` into `String` results in a string whose characters are the elements of the array.</span></span> <span data-ttu-id="24e55-287">`String` を `Char` の配列に変換すると、文字列の文字を要素として持つ文字の配列が生成されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-287">Converting `String` into an array of `Char` results in an array of characters whose elements are the characters of the string.</span></span>

<span data-ttu-id="24e55-288">`String` と `Boolean`、`Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`、`Decimal`、`Single`、`Double`、`Date`、およびその逆の変換は、この仕様の範囲を超えており、1つの詳細を除き、実装に依存します。</span><span class="sxs-lookup"><span data-stu-id="24e55-288">The exact conversions between `String` and `Boolean`, `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`, `Date`, and vice versa, are beyond the scope of this specification and are implementation dependent with the exception of one detail.</span></span> <span data-ttu-id="24e55-289">文字列変換では、常に実行時環境の現在のカルチャが考慮されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-289">String conversions always consider the current culture of the run-time environment.</span></span> <span data-ttu-id="24e55-290">そのため、実行時に実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="24e55-290">As such, they must be performed at run time.</span></span>

## <a name="widening-conversions"></a><span data-ttu-id="24e55-291">拡大変換</span><span class="sxs-lookup"><span data-stu-id="24e55-291">Widening Conversions</span></span>

<span data-ttu-id="24e55-292">拡大変換ではオーバーフローは発生しませんが、精度が失われる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="24e55-292">Widening conversions never overflow but may entail a loss of precision.</span></span> <span data-ttu-id="24e55-293">次の変換は、拡大変換です。</span><span class="sxs-lookup"><span data-stu-id="24e55-293">The following conversions are widening conversions:</span></span>

<span data-ttu-id="24e55-294">__Id/既定の変換__</span><span class="sxs-lookup"><span data-stu-id="24e55-294">__Identity/Default conversions__</span></span>

* <span data-ttu-id="24e55-295">型からそれ自体へ。</span><span class="sxs-lookup"><span data-stu-id="24e55-295">From a type to itself.</span></span>

* <span data-ttu-id="24e55-296">同じシグネチャを持つ任意のデリゲート型に対してラムダメソッドを再分類するために生成された匿名デリゲート型から。</span><span class="sxs-lookup"><span data-stu-id="24e55-296">From an anonymous delegate type generated for a lambda method reclassification to any delegate type with an identical signature.</span></span>

* <span data-ttu-id="24e55-297">リテラルから型への `Nothing`。</span><span class="sxs-lookup"><span data-stu-id="24e55-297">From the literal `Nothing` to a type.</span></span>

<span data-ttu-id="24e55-298">__数値変換__</span><span class="sxs-lookup"><span data-stu-id="24e55-298">__Numeric conversions__</span></span>

* <span data-ttu-id="24e55-299">`Byte` から `UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`、`Decimal`、`Single`、`Double`になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-299">From `Byte` to `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="24e55-300">`SByte` から `Short`、`Integer`、`Long`、`Decimal`、`Single`、`Double`になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-300">From `SByte` to `Short`, `Integer`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="24e55-301">`UShort` から `UInteger`、`Integer`、`ULong`、`Long`、`Decimal`、`Single`、`Double`になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-301">From `UShort` to `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="24e55-302">`Short` から `Integer`、`Long`、`Decimal`、`Single` または `Double`です。</span><span class="sxs-lookup"><span data-stu-id="24e55-302">From `Short` to `Integer`, `Long`, `Decimal`, `Single` or `Double`.</span></span>

* <span data-ttu-id="24e55-303">`UInteger` から `ULong`、`Long`、`Decimal`、`Single`、`Double`になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-303">From `UInteger` to `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="24e55-304">`Integer` から `Long`、`Decimal`、`Single` または `Double`ます。</span><span class="sxs-lookup"><span data-stu-id="24e55-304">From `Integer` to `Long`, `Decimal`, `Single` or `Double`.</span></span>

* <span data-ttu-id="24e55-305">`ULong` から `Decimal`、`Single`、または `Double`。</span><span class="sxs-lookup"><span data-stu-id="24e55-305">From `ULong` to `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="24e55-306">`Long` から `Decimal`、`Single` または `Double`します。</span><span class="sxs-lookup"><span data-stu-id="24e55-306">From `Long` to `Decimal`, `Single` or `Double`.</span></span>

* <span data-ttu-id="24e55-307">`Decimal` から `Single` または `Double`ます。</span><span class="sxs-lookup"><span data-stu-id="24e55-307">From `Decimal` to `Single` or `Double`.</span></span>

* <span data-ttu-id="24e55-308">`Single` から `Double`にします。</span><span class="sxs-lookup"><span data-stu-id="24e55-308">From `Single` to `Double`.</span></span>

* <span data-ttu-id="24e55-309">リテラルから列挙型への `0`。</span><span class="sxs-lookup"><span data-stu-id="24e55-309">From the literal `0` to an enumerated type.</span></span> <span data-ttu-id="24e55-310">(__注:__</span><span class="sxs-lookup"><span data-stu-id="24e55-310">(__Note.__</span></span> <span data-ttu-id="24e55-311">`0` から任意の列挙型への変換は、テストフラグを簡略化するために拡大されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-311">The conversion from `0` to any enumerated type is widening to simplify testing flags.</span></span> <span data-ttu-id="24e55-312">たとえば、`Values` が `One`値を持つ列挙型である場合、`(v And Values.One) = 0`を指定して `Values` 型の変数 `v` をテストできます。)</span><span class="sxs-lookup"><span data-stu-id="24e55-312">For example, if `Values` is an enumerated type with a value `One`, you could test a variable `v` of type `Values` by saying `(v And Values.One) = 0`.)</span></span>

* <span data-ttu-id="24e55-313">列挙型から基になる数値型、または基になる数値型からへの拡大変換がある数値型。</span><span class="sxs-lookup"><span data-stu-id="24e55-313">From an enumerated type to its underlying numeric type, or to a numeric type that its underlying numeric type has a widening conversion to.</span></span>

* <span data-ttu-id="24e55-314">定数式の値が変換先の型の範囲内にある場合、`ULong`型の定数式、`Long`、`UInteger`、`Integer`、`UShort`、`Short`、`Byte`、または `SByte` をより狭い型に変換します。</span><span class="sxs-lookup"><span data-stu-id="24e55-314">From a constant expression of type `ULong`, `Long`, `UInteger`, `Integer`, `UShort`, `Short`, `Byte`, or `SByte` to a narrower type, provided the value of the constant expression is within the range of the destination type.</span></span> <span data-ttu-id="24e55-315">(__注:__</span><span class="sxs-lookup"><span data-stu-id="24e55-315">(__Note.__</span></span> <span data-ttu-id="24e55-316">`UInteger` または `Integer` から `Single`、`ULong` または `Long` に変換して `Single` または `Double`に変換したり、`Decimal` または `Single` に変換したりすると、精度が失われる可能性がありますが、マグニチュードが失われることはありません。`Double`</span><span class="sxs-lookup"><span data-stu-id="24e55-316">Conversions from `UInteger` or `Integer` to `Single`, `ULong` or `Long` to `Single` or `Double`, or `Decimal` to `Single` or `Double` may cause a loss of precision, but will never cause a loss of magnitude.</span></span> <span data-ttu-id="24e55-317">その他の拡大数値変換では、情報が失われることはありません)。</span><span class="sxs-lookup"><span data-stu-id="24e55-317">The other widening numeric conversions never lose any information.)</span></span>

<span data-ttu-id="24e55-318">__参照変換__</span><span class="sxs-lookup"><span data-stu-id="24e55-318">__Reference conversions__</span></span>

* <span data-ttu-id="24e55-319">参照型から基本型への。</span><span class="sxs-lookup"><span data-stu-id="24e55-319">From a reference type to a base type.</span></span>

* <span data-ttu-id="24e55-320">型がインターフェイスまたは variant 互換インターフェイスを実装している場合は、参照型からインターフェイス型への。</span><span class="sxs-lookup"><span data-stu-id="24e55-320">From a reference type to an interface type, provided that the type implements the interface or a variant compatible interface.</span></span>

* <span data-ttu-id="24e55-321">インターフェイス型から `Object`にします。</span><span class="sxs-lookup"><span data-stu-id="24e55-321">From an interface type to `Object`.</span></span>

* <span data-ttu-id="24e55-322">インターフェイス型から variant 互換のインターフェイス型へ。</span><span class="sxs-lookup"><span data-stu-id="24e55-322">From an interface type to a variant compatible interface type.</span></span>

* <span data-ttu-id="24e55-323">デリゲート型から variant 互換のデリゲート型へ。</span><span class="sxs-lookup"><span data-stu-id="24e55-323">From a delegate type to a variant compatible delegate type.</span></span> <span data-ttu-id="24e55-324">(__注:__</span><span class="sxs-lookup"><span data-stu-id="24e55-324">(__Note.__</span></span> <span data-ttu-id="24e55-325">その他の多くの参照変換は、これらの規則によって暗黙的に示されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-325">Many other reference conversions are implied by these rules.</span></span> <span data-ttu-id="24e55-326">たとえば、匿名デリゲートは `System.MulticastDelegate`から継承する参照型です。配列型は `System.Array`から継承する参照型です。匿名型は、`System.Object`から継承する参照型です)。</span><span class="sxs-lookup"><span data-stu-id="24e55-326">For example, anonymous delegates are reference types that inherit from `System.MulticastDelegate`; array types are reference types that inherit from `System.Array`; anonymous types are reference types that inherit from `System.Object`.)</span></span>

<span data-ttu-id="24e55-327">__匿名デリゲートの変換__</span><span class="sxs-lookup"><span data-stu-id="24e55-327">__Anonymous Delegate conversions__</span></span>

* <span data-ttu-id="24e55-328">ラムダメソッドをより広範なデリゲート型に再分類するために生成された匿名デリゲート型から。</span><span class="sxs-lookup"><span data-stu-id="24e55-328">From an anonymous delegate type generated for a lambda method reclassification to any wider delegate type.</span></span>

<span data-ttu-id="24e55-329">__配列の変換__</span><span class="sxs-lookup"><span data-stu-id="24e55-329">__Array conversions__</span></span>

* <span data-ttu-id="24e55-330">要素型を `Se` 持つ `S` 配列型から、要素型 `Te`を持つ `T` 配列型には、次のすべてが true であることが条件となります。</span><span class="sxs-lookup"><span data-stu-id="24e55-330">From an array type `S` with an element type `Se` to an array type `T` with an element type `Te`, provided all of the following are true:</span></span>

  * <span data-ttu-id="24e55-331">`S` と `T` は要素の型のみが異なります。</span><span class="sxs-lookup"><span data-stu-id="24e55-331">`S` and `T` differ only in element type.</span></span>

  * <span data-ttu-id="24e55-332">`Se` と `Te` はどちらも参照型であるか、参照型であると認識される型パラメーターです。</span><span class="sxs-lookup"><span data-stu-id="24e55-332">Both `Se` and `Te` are reference types or are type parameters known to be a reference type.</span></span>

  * <span data-ttu-id="24e55-333">拡大参照、配列、または型パラメーターの変換が `Se` から `Te`に存在します。</span><span class="sxs-lookup"><span data-stu-id="24e55-333">A widening reference, array, or type parameter conversion exists from `Se` to `Te`.</span></span>

* <span data-ttu-id="24e55-334">配列型から `S` 列挙された要素型を使用して、次のすべての条件を満たしている場合は、要素型 `Te`で `T` 配列型に `Se` します。</span><span class="sxs-lookup"><span data-stu-id="24e55-334">From an array type `S` with an enumerated element type `Se` to an array type `T` with an element type `Te`, provided all of the following are true:</span></span>

  * <span data-ttu-id="24e55-335">`S` と `T` は要素の型のみが異なります。</span><span class="sxs-lookup"><span data-stu-id="24e55-335">`S` and `T` differ only in element type.</span></span>

  * <span data-ttu-id="24e55-336">`Te` は `Se`の基になる型です。</span><span class="sxs-lookup"><span data-stu-id="24e55-336">`Te` is the underlying type of `Se`.</span></span>

* <span data-ttu-id="24e55-337">次のいずれかの条件に該当する場合は、ランク1の `S` 配列型から、列挙された要素型 `Se`、`System.Collections.Generic.IList(Of Te)`、`IReadOnlyList(Of Te)`、`ICollection(Of Te)`、`IReadOnlyCollection(Of Te)`、および `IEnumerable(Of Te)`を使用します。</span><span class="sxs-lookup"><span data-stu-id="24e55-337">From an array type `S` of rank 1 with an enumerated element type `Se`, to `System.Collections.Generic.IList(Of Te)`, `IReadOnlyList(Of Te)`, `ICollection(Of Te)`, `IReadOnlyCollection(Of Te)`, and `IEnumerable(Of Te)`, provided one of the following is true:</span></span>

  * <span data-ttu-id="24e55-338">`Se` と `Te` はどちらも参照型であるか、参照型であると認識される型パラメーターであり、拡大参照、配列、または型パラメーターの変換は `Se` から `Te`に存在します。もしくは</span><span class="sxs-lookup"><span data-stu-id="24e55-338">Both `Se` and `Te` are reference types or are type parameters known to be a reference type, and a widening reference, array, or type parameter conversion exists from `Se` to `Te`; or</span></span>

  * <span data-ttu-id="24e55-339">`Te` は `Se`の基になる型です。もしくは</span><span class="sxs-lookup"><span data-stu-id="24e55-339">`Te` is the underlying type of `Se`; or</span></span>

  * <span data-ttu-id="24e55-340">`Te` は `Se` と同じ</span><span class="sxs-lookup"><span data-stu-id="24e55-340">`Te` is identical to `Se`</span></span>

<span data-ttu-id="24e55-341">__値型の変換__</span><span class="sxs-lookup"><span data-stu-id="24e55-341">__Value Type conversions__</span></span>

* <span data-ttu-id="24e55-342">値型から基本型への。</span><span class="sxs-lookup"><span data-stu-id="24e55-342">From a value type to a base type.</span></span>

* <span data-ttu-id="24e55-343">値型から、型が実装するインターフェイス型になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-343">From a value type to an interface type that the type implements.</span></span>

<span data-ttu-id="24e55-344">__Null 許容値型の変換__</span><span class="sxs-lookup"><span data-stu-id="24e55-344">__Nullable Value Type conversions__</span></span>

* <span data-ttu-id="24e55-345">型から `T?`型に `T` ます。</span><span class="sxs-lookup"><span data-stu-id="24e55-345">From a type `T` to the type `T?`.</span></span>

* <span data-ttu-id="24e55-346">型から `S?`型に `T?` ます。 `T` 型から `S`型への拡大変換があります。</span><span class="sxs-lookup"><span data-stu-id="24e55-346">From a type `T?` to a type `S?`, where there is a widening conversion from the type `T` to the type `S`.</span></span>

* <span data-ttu-id="24e55-347">型から `S?`型に `T` ます。 `T` 型から `S`型への拡大変換があります。</span><span class="sxs-lookup"><span data-stu-id="24e55-347">From a type `T` to a type `S?`, where there is a widening conversion from the type `T` to the type `S`.</span></span>

* <span data-ttu-id="24e55-348">型から、型 `T` が実装するインターフェイス型に `T?` ます。</span><span class="sxs-lookup"><span data-stu-id="24e55-348">From a type `T?` to an interface type that the type `T` implements.</span></span>

<span data-ttu-id="24e55-349">__文字列変換__</span><span class="sxs-lookup"><span data-stu-id="24e55-349">__String conversions__</span></span>

* <span data-ttu-id="24e55-350">`Char` から `String`にします。</span><span class="sxs-lookup"><span data-stu-id="24e55-350">From `Char` to `String`.</span></span>

* <span data-ttu-id="24e55-351">`Char()` から `String`にします。</span><span class="sxs-lookup"><span data-stu-id="24e55-351">From `Char()` to `String`.</span></span>

<span data-ttu-id="24e55-352">__型パラメーターの変換__</span><span class="sxs-lookup"><span data-stu-id="24e55-352">__Type Parameter conversions__</span></span>

* <span data-ttu-id="24e55-353">型パラメーターから `Object`にします。</span><span class="sxs-lookup"><span data-stu-id="24e55-353">From a type parameter to `Object`.</span></span>

* <span data-ttu-id="24e55-354">型パラメーターからインターフェイス型制約、またはインターフェイス型制約と互換性のあるインターフェイスバリアント。</span><span class="sxs-lookup"><span data-stu-id="24e55-354">From a type parameter to an interface type constraint or any interface variant compatible with an interface type constraint.</span></span>

* <span data-ttu-id="24e55-355">型パラメーターから、クラス制約によって実装されるインターフェイスへの。</span><span class="sxs-lookup"><span data-stu-id="24e55-355">From a type parameter to an interface implemented by a class constraint.</span></span>

* <span data-ttu-id="24e55-356">型パラメーターから、クラス制約によって実装されるインターフェイスと互換性のあるインターフェイスバリアントへの通信。</span><span class="sxs-lookup"><span data-stu-id="24e55-356">From a type parameter to an interface variant compatible with an interface implemented by a class constraint.</span></span>

* <span data-ttu-id="24e55-357">型パラメーターからクラス制約への、またはクラス制約の基本型。</span><span class="sxs-lookup"><span data-stu-id="24e55-357">From a type parameter to a class constraint, or a base type of the class constraint.</span></span>

* <span data-ttu-id="24e55-358">型パラメーターから `T` 型パラメーターの制約 `Tx`、またはすべての `Tx` に対してへの拡大変換が含まれています。</span><span class="sxs-lookup"><span data-stu-id="24e55-358">From a type parameter `T` to a type parameter constraint `Tx`, or anything `Tx` has a widening conversion to.</span></span>

## <a name="narrowing-conversions"></a><span data-ttu-id="24e55-359">縮小変換</span><span class="sxs-lookup"><span data-stu-id="24e55-359">Narrowing Conversions</span></span>

<span data-ttu-id="24e55-360">縮小変換は、常に成功するとは限りません。情報が失われる可能性がある変換、型のドメイン間での変換は、縮小表記という意味で十分に異なる変換です。</span><span class="sxs-lookup"><span data-stu-id="24e55-360">Narrowing conversions are conversions that cannot be proved to always succeed, conversions that are known to possibly lose information, and conversions across domains of types sufficiently different to merit narrowing notation.</span></span> <span data-ttu-id="24e55-361">次の変換は縮小変換として分類されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-361">The following conversions are classified as narrowing conversions:</span></span>

<span data-ttu-id="24e55-362">__ブール型変換__</span><span class="sxs-lookup"><span data-stu-id="24e55-362">__Boolean conversions__</span></span>

* <span data-ttu-id="24e55-363">`Boolean` から `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`、`Decimal`、`Single`、`Double`になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-363">From `Boolean` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="24e55-364">`Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`、`Decimal`、`Single`、`Double` を `Boolean`します。</span><span class="sxs-lookup"><span data-stu-id="24e55-364">From `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double` to `Boolean`.</span></span>

<span data-ttu-id="24e55-365">__数値変換__</span><span class="sxs-lookup"><span data-stu-id="24e55-365">__Numeric conversions__</span></span>

* <span data-ttu-id="24e55-366">`Byte` から `SByte`にします。</span><span class="sxs-lookup"><span data-stu-id="24e55-366">From `Byte` to `SByte`.</span></span>

* <span data-ttu-id="24e55-367">`SByte` から `Byte`、`UShort`、`UInteger`、または `ULong`になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-367">From `SByte` to `Byte`, `UShort`, `UInteger`, or `ULong`.</span></span>

* <span data-ttu-id="24e55-368">`UShort` から `Byte`、`SByte`、または `Short`。</span><span class="sxs-lookup"><span data-stu-id="24e55-368">From `UShort` to `Byte`, `SByte`, or `Short`.</span></span>

* <span data-ttu-id="24e55-369">`Short` から `Byte`、`SByte`、`UShort`、`UInteger`、`ULong`になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-369">From `Short` to `Byte`, `SByte`, `UShort`, `UInteger`, or `ULong`.</span></span>

* <span data-ttu-id="24e55-370">`UInteger` から `Byte`、`SByte`、`UShort`、`Short`、`Integer`になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-370">From `UInteger` to `Byte`, `SByte`, `UShort`, `Short`, or `Integer`.</span></span>

* <span data-ttu-id="24e55-371">`Integer` から `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`ULong`になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-371">From `Integer` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, or `ULong`.</span></span>

* <span data-ttu-id="24e55-372">`ULong` から `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`Long`になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-372">From `ULong` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, or `Long`.</span></span>

* <span data-ttu-id="24e55-373">`Long` から `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-373">From `Long` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, or `ULong`.</span></span>

* <span data-ttu-id="24e55-374">`Decimal` から `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-374">From `Decimal` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, or `Long`.</span></span>

* <span data-ttu-id="24e55-375">`Single` から `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`、`Decimal`になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-375">From `Single` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, or `Decimal`.</span></span>

* <span data-ttu-id="24e55-376">`Double` から `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`、`Decimal`、`Single`になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-376">From `Double` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, or `Single`.</span></span>

* <span data-ttu-id="24e55-377">数値型から列挙型への。</span><span class="sxs-lookup"><span data-stu-id="24e55-377">From a numeric type to an enumerated type.</span></span>

* <span data-ttu-id="24e55-378">列挙型から数値型への変換では、基になる数値型がに縮小変換されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-378">From an enumerated type to a numeric type its underlying numeric type has a narrowing conversion to.</span></span>

* <span data-ttu-id="24e55-379">列挙型から別の列挙型へ。</span><span class="sxs-lookup"><span data-stu-id="24e55-379">From an enumerated type to another enumerated type.</span></span>

<span data-ttu-id="24e55-380">__参照変換__</span><span class="sxs-lookup"><span data-stu-id="24e55-380">__Reference conversions__</span></span>

* <span data-ttu-id="24e55-381">参照型からより派生した型への。</span><span class="sxs-lookup"><span data-stu-id="24e55-381">From a reference type to a more derived type.</span></span>

* <span data-ttu-id="24e55-382">クラス型がインターフェイス型またはそれと互換性のあるインターフェイス型バリアントを実装していない場合、クラス型からインターフェイス型になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-382">From a class type to an interface type, provided the class type does not implement the interface type or an interface type variant compatible with it.</span></span>

* <span data-ttu-id="24e55-383">インターフェイス型からクラス型へ。</span><span class="sxs-lookup"><span data-stu-id="24e55-383">From an interface type to a class type.</span></span>

* <span data-ttu-id="24e55-384">2つの型の間に継承関係がなく、バリアントとの互換性がない場合は、インターフェイス型から別のインターフェイス型にします。</span><span class="sxs-lookup"><span data-stu-id="24e55-384">From an interface type to another interface type, provided there is no inheritance relationship between the two types and provided they are not variant compatible.</span></span>

<span data-ttu-id="24e55-385">__匿名デリゲートの変換__</span><span class="sxs-lookup"><span data-stu-id="24e55-385">__Anonymous Delegate conversions__</span></span>

* <span data-ttu-id="24e55-386">任意の幅の狭いデリゲート型に再分類するラムダメソッドに対して生成された匿名デリゲート型から。</span><span class="sxs-lookup"><span data-stu-id="24e55-386">From an anonymous delegate type generated for a lambda method reclassification to any narrower delegate type.</span></span>

<span data-ttu-id="24e55-387">__配列の変換__</span><span class="sxs-lookup"><span data-stu-id="24e55-387">__Array conversions__</span></span>

* <span data-ttu-id="24e55-388">次のすべての条件に該当する場合は、要素型が `Se`である `S` 配列型から、要素型が `Te`の配列型に `T` ます。</span><span class="sxs-lookup"><span data-stu-id="24e55-388">From an array type `S` with an element type `Se`, to an array type `T` with an element type `Te`, provided that all of the following are true:</span></span>

  * <span data-ttu-id="24e55-389">`S` と `T` は要素の型のみが異なります。</span><span class="sxs-lookup"><span data-stu-id="24e55-389">`S` and `T` differ only in element type.</span></span>
  * <span data-ttu-id="24e55-390">`Se` と `Te` はどちらも参照型であるか、または値型として知られていない型パラメーターです。</span><span class="sxs-lookup"><span data-stu-id="24e55-390">Both `Se` and `Te` are reference types or are type parameters not known to be value types.</span></span>
  * <span data-ttu-id="24e55-391">`Se` から `Te`への縮小参照、配列、または型パラメーターの変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="24e55-391">A narrowing reference, array, or type parameter conversion exists from `Se` to `Te`.</span></span>

* <span data-ttu-id="24e55-392">次のすべての条件に該当する場合は、要素型を持つ `S` 配列型から、列挙された要素型 `Te`を持つ `T` 配列型に `Se` ます。</span><span class="sxs-lookup"><span data-stu-id="24e55-392">From an array type `S` with an element type `Se` to an array type `T` with an enumerated element type `Te`, provided all of the following are true:</span></span>

  * <span data-ttu-id="24e55-393">`S` と `T` は要素の型のみが異なります。</span><span class="sxs-lookup"><span data-stu-id="24e55-393">`S` and `T` differ only in element type.</span></span>
  * <span data-ttu-id="24e55-394">`Se` は `Te` の基になる型であるか、または基になる型が同じである列挙型が異なる場合があります。</span><span class="sxs-lookup"><span data-stu-id="24e55-394">`Se` is the underlying type of `Te` , or they are both different enumerated types that share the same underlying type.</span></span>

* <span data-ttu-id="24e55-395">次のいずれかに該当する場合は、ランク1の `S` 配列型から、列挙された要素型 `Se`、`IList(Of Te)`、`IReadOnlyList(Of Te)`、`ICollection(Of Te)`、`IReadOnlyCollection(Of Te)`、および `IEnumerable(Of Te)`になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-395">From an array type `S` of rank 1 with an enumerated element type `Se`, to `IList(Of Te)`, `IReadOnlyList(Of Te)`, `ICollection(Of Te)`, `IReadOnlyCollection(Of Te)` and `IEnumerable(Of Te)`, provided one of the following is true:</span></span>

  * <span data-ttu-id="24e55-396">`Se` と `Te` はどちらも参照型であるか、参照型であると認識される型パラメーターであり、縮小参照、配列、または型パラメーターの変換は `Se` から `Te`に存在します。もしくは</span><span class="sxs-lookup"><span data-stu-id="24e55-396">Both `Se` and `Te` are reference types or are type parameters known to be a reference type, and a narrowing reference, array, or type parameter conversion exists from `Se` to `Te`; or</span></span>
  * <span data-ttu-id="24e55-397">`Se` は `Te`の基になる型であるか、または基になる型が同じである列挙型が異なる場合があります。</span><span class="sxs-lookup"><span data-stu-id="24e55-397">`Se` is the underlying type of `Te`, or they are both different enumerated types that share the same underlying type.</span></span>

<span data-ttu-id="24e55-398">__値型の変換__</span><span class="sxs-lookup"><span data-stu-id="24e55-398">__Value type conversions__</span></span>

* <span data-ttu-id="24e55-399">参照型からより多くの派生値型へ。</span><span class="sxs-lookup"><span data-stu-id="24e55-399">From a reference type to a more derived value type.</span></span>

* <span data-ttu-id="24e55-400">値型がインターフェイス型を実装している場合は、インターフェイス型から値型になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-400">From an interface type to a value type, provided the value type implements the interface type.</span></span>

<span data-ttu-id="24e55-401">__Null 許容値型の変換__</span><span class="sxs-lookup"><span data-stu-id="24e55-401">__Nullable Value Type conversions__</span></span>

* <span data-ttu-id="24e55-402">型から `T`型に `T?` ます。</span><span class="sxs-lookup"><span data-stu-id="24e55-402">From a type `T?` to a type `T`.</span></span>

* <span data-ttu-id="24e55-403">型から `S?`型に `T?` します。 `T` 型から `S`型への縮小変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="24e55-403">From a type `T?` to a type `S?`, where there is a narrowing conversion from the type `T` to the type `S`.</span></span>

* <span data-ttu-id="24e55-404">型から `S?`型に `T` します。 `T` 型から `S`型への縮小変換が存在します。</span><span class="sxs-lookup"><span data-stu-id="24e55-404">From a type `T` to a type `S?`, where there is a narrowing conversion from the type `T` to the type `S`.</span></span>

* <span data-ttu-id="24e55-405">型から `T`型に `S?` ます。 `S` 型から `T`型への変換があります。</span><span class="sxs-lookup"><span data-stu-id="24e55-405">From a type `S?` to a type `T`, where there is a conversion from the type `S` to the type `T`.</span></span>

<span data-ttu-id="24e55-406">__文字列変換__</span><span class="sxs-lookup"><span data-stu-id="24e55-406">__String conversions__</span></span>

* <span data-ttu-id="24e55-407">`String` から `Char`にします。</span><span class="sxs-lookup"><span data-stu-id="24e55-407">From `String` to `Char`.</span></span>

* <span data-ttu-id="24e55-408">`String` から `Char()`にします。</span><span class="sxs-lookup"><span data-stu-id="24e55-408">From `String` to `Char()`.</span></span>

* <span data-ttu-id="24e55-409">`String` から `Boolean`、`Boolean` から `String`にします。</span><span class="sxs-lookup"><span data-stu-id="24e55-409">From `String` to `Boolean` and from `Boolean` to `String`.</span></span>

* <span data-ttu-id="24e55-410">`String` と `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`、`Decimal`、`Single`、`Double`間の変換。</span><span class="sxs-lookup"><span data-stu-id="24e55-410">Conversions between `String` and `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="24e55-411">`String` から `Date`、`Date` から `String`にします。</span><span class="sxs-lookup"><span data-stu-id="24e55-411">From `String` to `Date` and from `Date` to `String`.</span></span>

<span data-ttu-id="24e55-412">__型パラメーターの変換__</span><span class="sxs-lookup"><span data-stu-id="24e55-412">__Type Parameter conversions__</span></span>

* <span data-ttu-id="24e55-413">`Object` から型パラメーターへ。</span><span class="sxs-lookup"><span data-stu-id="24e55-413">From `Object` to a type parameter.</span></span>

* <span data-ttu-id="24e55-414">型パラメーターがインターフェイスに制約されていない場合、または、そのインターフェイスを実装するクラスに制約されている場合、型パラメーターからインターフェイス型になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-414">From a type parameter to an interface type, provided the type parameter is not constrained to that interface or constrained to a class that implements that interface.</span></span>

* <span data-ttu-id="24e55-415">インターフェイス型から型パラメーターへ。</span><span class="sxs-lookup"><span data-stu-id="24e55-415">From an interface type to a type parameter.</span></span>

* <span data-ttu-id="24e55-416">型パラメーターから、クラス制約の派生型にします。</span><span class="sxs-lookup"><span data-stu-id="24e55-416">From a type parameter to a derived type of a class constraint.</span></span>

* <span data-ttu-id="24e55-417">型パラメーターから `T` 型パラメーターの制約には、への縮小変換が `Tx` れます。</span><span class="sxs-lookup"><span data-stu-id="24e55-417">From a type parameter `T` to anything a type parameter constraint `Tx` has a narrowing conversion to.</span></span>

## <a name="type-parameter-conversions"></a><span data-ttu-id="24e55-418">型パラメーターの変換</span><span class="sxs-lookup"><span data-stu-id="24e55-418">Type Parameter Conversions</span></span>

<span data-ttu-id="24e55-419">型パラメーターの変換は、制約 (存在する場合) によって決定されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-419">Type parameters' conversions are determined by the constraints, if any, put on them.</span></span> <span data-ttu-id="24e55-420">型パラメーター `T` は、任意のインターフェイス型に対して常に、または `Object`との間でいつでも変換できます。</span><span class="sxs-lookup"><span data-stu-id="24e55-420">A type parameter `T` can always be converted to itself, to and from `Object`, and to and from any interface type.</span></span> <span data-ttu-id="24e55-421">型 `T` が実行時に値型である場合、`T` から `Object` またはインターフェイス型への変換はボックス化変換であり、`Object` またはインターフェイス型から `T` への変換はボックス化変換となります。</span><span class="sxs-lookup"><span data-stu-id="24e55-421">Note that if the type `T` is a value type at run-time, converting from `T` to `Object` or an interface type will be a boxing conversion and converting from `Object` or an interface type to `T` will be an unboxing conversion.</span></span> <span data-ttu-id="24e55-422">`C` クラス制約を持つ型パラメーターは、型パラメーターから `C` とその基本クラスへの追加の変換、およびその逆の変換を定義します。</span><span class="sxs-lookup"><span data-stu-id="24e55-422">A type parameter with a class constraint `C` defines additional conversions from the type parameter to `C` and its base classes, and vice versa.</span></span> <span data-ttu-id="24e55-423">型パラメーターの制約を持つ `T` 型パラメーターは、`Tx` への変換と、に変換 `Tx` すべてを定義 `Tx` ます。</span><span class="sxs-lookup"><span data-stu-id="24e55-423">A type parameter `T` with a type parameter constraint `Tx` defines a conversion to `Tx` and anything `Tx` converts to.</span></span>

<span data-ttu-id="24e55-424">要素型がインターフェイス制約を持つ型パラメーターである配列は、型パラメーターにも `Class` またはクラスの制約 (参照配列要素型のみを共変にすることができるため) を持つ `I`配列と同じ共変配列変換を `I` します。</span><span class="sxs-lookup"><span data-stu-id="24e55-424">An array whose element type is a type parameter with an interface constraint `I` has the same covariant array conversions as an array whose element type is `I`, provided that the type parameter also has a `Class` or class constraint (since only reference array element types can be covariant).</span></span> <span data-ttu-id="24e55-425">要素型がクラス制約を持つ型パラメーターである配列は、要素型が `C`配列と同じ共変配列変換を `C` ます。</span><span class="sxs-lookup"><span data-stu-id="24e55-425">An array whose element type is a type parameter with a class constraint `C` has the same covariant array conversions as an array whose element type is `C`.</span></span>

<span data-ttu-id="24e55-426">上記の変換規則では、制約のない型パラメーターから非インターフェイス型への変換は許可されていません。</span><span class="sxs-lookup"><span data-stu-id="24e55-426">The above conversions rules do not permit conversions from unconstrained type parameters to non-interface types, which may be surprising.</span></span> <span data-ttu-id="24e55-427">その理由は、このような変換のセマンティクスについて混乱が生じないようにするためです。</span><span class="sxs-lookup"><span data-stu-id="24e55-427">The reason for this is to prevent confusion about the semantics of such conversions.</span></span> <span data-ttu-id="24e55-428">たとえば、次のような宣言があるとします。</span><span class="sxs-lookup"><span data-stu-id="24e55-428">For example, consider the following declaration:</span></span>

```vb
Class X(Of T)
    Public Shared Function F(t As T) As Long 
        Return CLng(t)    ' Error, explicit conversion not permitted
    End Function
End Class
```

<span data-ttu-id="24e55-429">`T` から `Integer` への変換が許可されている場合は、`X(Of Integer).F(7)` が `7L`を返すことが簡単であると考えられます。</span><span class="sxs-lookup"><span data-stu-id="24e55-429">If the conversion of `T` to `Integer` were permitted, one might easily expect that `X(Of Integer).F(7)` would return `7L`.</span></span> <span data-ttu-id="24e55-430">ただし、数値変換はコンパイル時に型が数値であることがわかっている場合にのみ考慮されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-430">However, it would not, because numeric conversions are only considered when the types are known to be numeric at compile time.</span></span> <span data-ttu-id="24e55-431">このようなセマンティクスを明確にするために、上記の例を記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="24e55-431">In order to make the semantics clear, the above example must instead be written:</span></span>

```vb
Class X(Of T)
    Public Shared Function F(t As T) As Long
        Return CLng(CObj(t))    ' OK, conversions permitted
    End Function
End Class
```

## <a name="user-defined-conversions"></a><span data-ttu-id="24e55-432">ユーザー定義の変換</span><span class="sxs-lookup"><span data-stu-id="24e55-432">User-Defined Conversions</span></span>

<span data-ttu-id="24e55-433">*組み込み変換*は、言語によって定義された変換 (この仕様ではリスト) であり、*ユーザー定義の変換*は `CType` 演算子のオーバーロードによって定義されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-433">*Intrinsic conversions* are conversions defined by the language (i.e. listed in this specification), while *user-defined conversions* are defined by overloading the `CType` operator.</span></span> <span data-ttu-id="24e55-434">型の間で変換を行う場合、組み込みの変換が適用されない場合は、ユーザー定義の変換が考慮されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-434">When converting between types, if no intrinsic conversions are applicable then user-defined conversions will be considered.</span></span> <span data-ttu-id="24e55-435">ソースとターゲットの型に*最も固有*なユーザー定義の変換がある場合は、ユーザー定義の変換が使用されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-435">If there is a user-defined conversion that is *most specific* for the source and target types, then the user-defined conversion will be used.</span></span> <span data-ttu-id="24e55-436">それ以外の場合は、コンパイル時のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="24e55-436">Otherwise, a compile-time error results.</span></span> <span data-ttu-id="24e55-437">最も具体的な変換は、変換元の型に "最も近い" オペランドがあり、結果の型がターゲットの型に "最も近い" である変換です。</span><span class="sxs-lookup"><span data-stu-id="24e55-437">The most specific conversion is the one whose operand is "closest" to the source type and whose result type is "closest" to the target type.</span></span> <span data-ttu-id="24e55-438">使用するユーザー定義の変換を決定するときに、最も具体的な拡大変換が使用されます。拡大変換が最も限定的でない場合は、最も限定的な縮小変換が使用されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-438">When determining what user-defined conversion to use, the most specific widening conversion will be used; if no widening conversion is most specific, the most specific narrowing conversion will be used.</span></span> <span data-ttu-id="24e55-439">特定の縮小変換がない場合、変換は定義されず、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="24e55-439">If there is no most specific narrowing conversion, then the conversion is undefined and a compile-time error occurs.</span></span>

<span data-ttu-id="24e55-440">以下のセクションでは、最も具体的な変換がどのように決定されるかについて説明します。</span><span class="sxs-lookup"><span data-stu-id="24e55-440">The following sections cover how the most specific conversions are determined.</span></span> <span data-ttu-id="24e55-441">次の用語を使用します。</span><span class="sxs-lookup"><span data-stu-id="24e55-441">They use the following terms:</span></span>

<span data-ttu-id="24e55-442">型 `A` から `B`型に組み込みの拡大変換が存在し、`A` も `B` もインターフェイスではない場合、 *`A` は `B`* に含まれ、`B` は `A`を*含み*ます。</span><span class="sxs-lookup"><span data-stu-id="24e55-442">If an intrinsic widening conversion exists from a type `A` to a type `B`, and if neither `A` nor `B` are interfaces, then `A` is *encompassed* by `B`, and `B` *encompasses* `A`.</span></span>

<span data-ttu-id="24e55-443">型のセットの中で*最も外側*にある型は、セット内の他のすべての型を含む1つの型です。</span><span class="sxs-lookup"><span data-stu-id="24e55-443">The *most encompassing* type in a set of types is the one type that encompasses all other types in the set.</span></span> <span data-ttu-id="24e55-444">1つの型に他のすべての型が含まれていない場合、そのセットには最も外側の型がありません。</span><span class="sxs-lookup"><span data-stu-id="24e55-444">If no single type encompasses all other types, then the set has no most encompassing type.</span></span> <span data-ttu-id="24e55-445">直感的に言うと、最も外側の型は、セット内の "最大" 型です。これは、他の型を拡大変換することによって変換できる1つの型です。</span><span class="sxs-lookup"><span data-stu-id="24e55-445">In intuitive terms, the most encompassing type is the "largest" type in the set -- the one type to which each of the other types can be converted through a widening conversion.</span></span>

<span data-ttu-id="24e55-446">型のセットの中で*最も内側*にある型は、セット内の他のすべての型に包含されている型です。</span><span class="sxs-lookup"><span data-stu-id="24e55-446">The *most encompassed* type in a set of types is the one type that is encompassed by all other types in the set.</span></span> <span data-ttu-id="24e55-447">他のすべての型に包含されている型がない場合は、そのセットに最も包含されていない型はありません。</span><span class="sxs-lookup"><span data-stu-id="24e55-447">If no single type is encompassed by all other types, then the set has no most encompassed type.</span></span> <span data-ttu-id="24e55-448">直感的に言うと、最も包含されている型は、セット内の "最小" 型です。これは、縮小変換を通じて他の型に変換できる1つの型です。</span><span class="sxs-lookup"><span data-stu-id="24e55-448">In intuitive terms, the most encompassed type is the "smallest" type in the set -- the one type that can be converted to each of the other types through a narrowing conversion.</span></span>

<span data-ttu-id="24e55-449">`T?`型のユーザー定義候補の候補を収集する場合、`T` で定義されたユーザー定義の変換演算子が代わりに使用されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-449">When collecting the candidate user-defined conversions for a type `T?`, the user-defined conversion operators defined by `T` are used instead.</span></span> <span data-ttu-id="24e55-450">変換先の型が null 許容の値型でもある場合、null 非許容の値型のみを含む、`T`のユーザー定義変換演算子はすべてリフトされます。</span><span class="sxs-lookup"><span data-stu-id="24e55-450">If the type being converted to is also a nullable value type, then any of `T`'s user-defined conversions operators that involve only non-nullable value types are lifted.</span></span> <span data-ttu-id="24e55-451">`T` から `S` への変換演算子は、`T?` から `S?` への変換として解除され、必要に応じて `T?` から `T`に変換し、`T` を `S` に変換することによって評価されます。`S``S?`</span><span class="sxs-lookup"><span data-stu-id="24e55-451">A conversion operator from `T` to `S` is lifted to be a conversion from `T?` to `S?` and is evaluated by converting `T?` to `T`, if necessary, then evaluating the user-defined conversion operator from `T` to `S` and then converting `S` to `S?`, if necessary.</span></span> <span data-ttu-id="24e55-452">ただし、変換される値が `Nothing`場合、リフトされた変換演算子は `S?`として型指定された `Nothing` の値に直接変換します。</span><span class="sxs-lookup"><span data-stu-id="24e55-452">If the value being converted is `Nothing`, however, a lifted conversion operator converts directly into a value of `Nothing` typed as `S?`.</span></span> <span data-ttu-id="24e55-453">例 :</span><span class="sxs-lookup"><span data-stu-id="24e55-453">For example:</span></span>

```vb
Structure S
    ...
End Structure

Structure T
    Public Shared Widening Operator CType(ByVal v As T) As S
        ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim x As T?
        Dim y As S?

        y = x                ' Legal: y is still null
        x = New T()
        y = x                ' Legal: Converts from T to S
    End Sub
End Module
```

<span data-ttu-id="24e55-454">変換を解決する際には、リフトされた変換演算子よりもユーザー定義の変換演算子を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="24e55-454">When resolving conversions, user-defined conversions operators are always preferred over lifted conversion operators.</span></span> <span data-ttu-id="24e55-455">例 :</span><span class="sxs-lookup"><span data-stu-id="24e55-455">For example:</span></span>

```vb
Structure S
    ...
End Structure

Structure T
    Public Shared Widening Operator CType(ByVal v As T) As S
        ...
    End Operator

    Public Shared Widening Operator CType(ByVal v As T?) As S?
        ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim x As T?
        Dim y As S?

        y = x                ' Calls user-defined conversion, not lifted conversion
    End Sub
End Module
```

<span data-ttu-id="24e55-456">実行時にユーザー定義の変換を評価するには、次の3つの手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="24e55-456">At run-time, evaluating a user-defined conversion can involve up to three steps:</span></span>

1. <span data-ttu-id="24e55-457">まず、必要に応じて、組み込みの変換を使用して、変換元の型からオペランドの型に値を変換します。</span><span class="sxs-lookup"><span data-stu-id="24e55-457">First, the value is converted from the source type to the operand type using an intrinsic conversion, if necessary.</span></span>

2. <span data-ttu-id="24e55-458">次に、ユーザー定義の変換が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-458">Then, the user-defined conversion is invoked.</span></span>

3. <span data-ttu-id="24e55-459">最後に、必要に応じて、組み込みの変換を使用して、ユーザー定義の変換の結果を対象の型に変換します。</span><span class="sxs-lookup"><span data-stu-id="24e55-459">Finally, the result of the user-defined conversion is converted to the target type using an intrinsic conversion, if necessary.</span></span>

<span data-ttu-id="24e55-460">ユーザー定義の変換の評価では、複数のユーザー定義変換演算子が含まれないことに注意する必要があります。</span><span class="sxs-lookup"><span data-stu-id="24e55-460">It is important to note that evaluation of a user-defined conversion will never involve more than one user-defined conversion operator.</span></span>

### <a name="most-specific-widening-conversion"></a><span data-ttu-id="24e55-461">最も限定的な拡大変換</span><span class="sxs-lookup"><span data-stu-id="24e55-461">Most Specific Widening Conversion</span></span>

<span data-ttu-id="24e55-462">次の手順を使用して、2つの型の間で、ユーザー定義の拡大変換演算子のうち最も限定的なものを決定します。</span><span class="sxs-lookup"><span data-stu-id="24e55-462">Determining the most specific user-defined widening conversion operator between two types is accomplished using the following steps:</span></span>

1. <span data-ttu-id="24e55-463">まず、候補となる変換演算子がすべて収集されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-463">First, all of the candidate conversion operators are collected.</span></span> <span data-ttu-id="24e55-464">候補の変換演算子は、ソース型のすべてのユーザー定義の拡大変換演算子と、対象の型のすべてのユーザー定義の拡大変換演算子です。</span><span class="sxs-lookup"><span data-stu-id="24e55-464">The candidate conversion operators are all of the user-defined widening conversion operators in the source type and all of the user-defined widening conversion operators in the target type.</span></span>

2. <span data-ttu-id="24e55-465">次に、適用できないすべての変換演算子がセットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-465">Then, all non-applicable conversion operators are removed from the set.</span></span> <span data-ttu-id="24e55-466">変換演算子は、ソース型からオペランド型への組み込みの拡大変換演算子が存在し、演算子の結果からターゲット型への組み込みの拡大変換演算子が存在する場合に、ソースの型とターゲットの型に適用されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-466">A conversion operator is applicable to a source type and target type if there is an intrinsic widening conversion operator from the source type to the operand type and there is an intrinsic widening conversion operator from the result of the operator to the target type.</span></span> <span data-ttu-id="24e55-467">適用可能な変換演算子がない場合、特定の拡大変換は行われません。</span><span class="sxs-lookup"><span data-stu-id="24e55-467">If there are no applicable conversion operators, then there is no most specific widening conversion.</span></span>

3. <span data-ttu-id="24e55-468">次に、適用可能な変換演算子の最も限定的なソースの種類が決定されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-468">Then, the most specific source type of the applicable conversion operators is determined:</span></span>

   * <span data-ttu-id="24e55-469">変換演算子のいずれかが変換元の型から直接変換する場合、変換元の型は最も限定的なソースの種類です。</span><span class="sxs-lookup"><span data-stu-id="24e55-469">If any of the conversion operators convert directly from the source type, then the source type is the most specific source type.</span></span>

   * <span data-ttu-id="24e55-470">それ以外の場合、最も限定的なソースの種類は、変換演算子のソース型の組み合わせで最も包含される型です。</span><span class="sxs-lookup"><span data-stu-id="24e55-470">Otherwise, the most specific source type is the most encompassed type in the combined set of source types of the conversion operators.</span></span> <span data-ttu-id="24e55-471">包含されている型の中に最も多くの包含型が見つからない場合は、特に特定の拡大変換はありません。</span><span class="sxs-lookup"><span data-stu-id="24e55-471">If no most encompassed type can be found, then there is no most specific widening conversion.</span></span>

4. <span data-ttu-id="24e55-472">次に、適用可能な変換演算子の最も限定的なターゲットの種類を決定します。</span><span class="sxs-lookup"><span data-stu-id="24e55-472">Then, the most specific target type of the applicable conversion operators is determined:</span></span>

   * <span data-ttu-id="24e55-473">変換演算子のいずれかがターゲット型に直接変換される場合、対象の型は最も限定的なターゲット型です。</span><span class="sxs-lookup"><span data-stu-id="24e55-473">If any of the conversion operators convert directly to the target type, then the target type is the most specific target type.</span></span>

   * <span data-ttu-id="24e55-474">それ以外の場合、最も限定的なターゲット型は、変換演算子の対象の型の組み合わせで最も外側にある型になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-474">Otherwise, the most specific target type is the most encompassing type in the combined set of target types of the conversion operators.</span></span> <span data-ttu-id="24e55-475">最も外側の型が見つからない場合、特定の拡大変換は行われません。</span><span class="sxs-lookup"><span data-stu-id="24e55-475">If no most encompassing type can be found, then there is no most specific widening conversion.</span></span>

5. <span data-ttu-id="24e55-476">その後、ただ1つの変換演算子が最も具体的な変換元の型から最も限定的なターゲット型に変換する場合は、これが最も具体的な変換演算子です。</span><span class="sxs-lookup"><span data-stu-id="24e55-476">Then, if exactly one conversion operator converts from the most specific source type to the most specific target type, then this is the most specific conversion operator.</span></span> <span data-ttu-id="24e55-477">このような演算子が複数存在する場合、特定の拡大変換は行われません。</span><span class="sxs-lookup"><span data-stu-id="24e55-477">If more than one such operator exists, then there is no most specific widening conversion.</span></span>

### <a name="most-specific-narrowing-conversion"></a><span data-ttu-id="24e55-478">最も限定的な縮小変換</span><span class="sxs-lookup"><span data-stu-id="24e55-478">Most Specific Narrowing Conversion</span></span>

<span data-ttu-id="24e55-479">次の手順に従って、2つの型の間で最も限定的なユーザー定義の縮小変換演算子を決定します。</span><span class="sxs-lookup"><span data-stu-id="24e55-479">Determining the most specific user-defined narrowing conversion operator between two types is accomplished using the following steps:</span></span>

1. <span data-ttu-id="24e55-480">まず、候補となる変換演算子がすべて収集されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-480">First, all of the candidate conversion operators are collected.</span></span> <span data-ttu-id="24e55-481">候補の変換演算子は、変換元の型のすべてのユーザー定義変換演算子と、対象の型のすべてのユーザー定義変換演算子です。</span><span class="sxs-lookup"><span data-stu-id="24e55-481">The candidate conversion operators are all of the user-defined conversion operators in the source type and all of the user-defined conversion operators in the target type.</span></span>

2. <span data-ttu-id="24e55-482">次に、適用できないすべての変換演算子がセットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-482">Then, all non-applicable conversion operators are removed from the set.</span></span> <span data-ttu-id="24e55-483">変換演算子は、ソース型からオペランド型への組み込みの変換演算子が存在し、演算子の結果からターゲット型への組み込みの変換演算子がある場合に、ソース型およびターゲット型に適用されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-483">A conversion operator is applicable to a source type and target type if there is an intrinsic conversion operator from the source type to the operand type and there is an intrinsic conversion operator from the result of the operator to the target type.</span></span> <span data-ttu-id="24e55-484">適用可能な変換演算子がない場合、特定の縮小変換は行われません。</span><span class="sxs-lookup"><span data-stu-id="24e55-484">If there are no applicable conversion operators, then there is no most specific narrowing conversion.</span></span>

3. <span data-ttu-id="24e55-485">次に、適用可能な変換演算子の最も限定的なソースの種類が決定されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-485">Then, the most specific source type of the applicable conversion operators is determined:</span></span>

   * <span data-ttu-id="24e55-486">変換演算子のいずれかが変換元の型から直接変換する場合、変換元の型は最も限定的なソースの種類です。</span><span class="sxs-lookup"><span data-stu-id="24e55-486">If any of the conversion operators convert directly from the source type, then the source type is the most specific source type.</span></span>

   * <span data-ttu-id="24e55-487">それ以外の場合、変換演算子のいずれかが、変換元の型を含む型から変換する場合、最も限定的なソース型は、それらの変換演算子のソース型の組み合わせに含まれる最も包含された型になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-487">Otherwise, if any of the conversion operators convert from types that encompass the source type, then the most specific source type is the most encompassed type in the combined set of source types of those conversion operators.</span></span> <span data-ttu-id="24e55-488">包含されていない型がほとんど見つからない場合、特定の縮小変換は行われません。</span><span class="sxs-lookup"><span data-stu-id="24e55-488">If no most encompassed type can be found, then there is no most specific narrowing conversion.</span></span>

   * <span data-ttu-id="24e55-489">それ以外の場合、最も限定的なソースの種類は、変換演算子のソース型の組み合わせで最も外側にある型になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-489">Otherwise, the most specific source type is the most encompassing type in the combined set of source types of the conversion operators.</span></span> <span data-ttu-id="24e55-490">最も外側の型が見つからない場合、特定の縮小変換は行われません。</span><span class="sxs-lookup"><span data-stu-id="24e55-490">If no most encompassing type can be found, then there is no most specific narrowing conversion.</span></span>

4. <span data-ttu-id="24e55-491">次に、適用可能な変換演算子の最も限定的なターゲットの種類を決定します。</span><span class="sxs-lookup"><span data-stu-id="24e55-491">Then, the most specific target type of the applicable conversion operators is determined:</span></span>

   * <span data-ttu-id="24e55-492">変換演算子のいずれかがターゲット型に直接変換される場合、対象の型は最も限定的なターゲット型です。</span><span class="sxs-lookup"><span data-stu-id="24e55-492">If any of the conversion operators convert directly to the target type, then the target type is the most specific target type.</span></span>

   * <span data-ttu-id="24e55-493">それ以外の場合、変換演算子のいずれかが、対象の型によって包含されている型に変換される場合、最も限定的なターゲット型は、それらの変換演算子のソース型の組み合わせで最も外側にある型になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-493">Otherwise, if any of the conversion operators convert to types that are encompassed by the target type, then the most specific target type is the most encompassing type in the combined set of source types of those conversion operators.</span></span> <span data-ttu-id="24e55-494">最も外側の型が見つからない場合、特定の縮小変換は行われません。</span><span class="sxs-lookup"><span data-stu-id="24e55-494">If no most encompassing type can be found, then there is no most specific narrowing conversion.</span></span>

   * <span data-ttu-id="24e55-495">それ以外の場合、最も限定的なターゲット型は、変換演算子の対象の型の組み合わせで最も包含される型です。</span><span class="sxs-lookup"><span data-stu-id="24e55-495">Otherwise, the most specific target type is the most encompassed type in the combined set of target types of the conversion operators.</span></span> <span data-ttu-id="24e55-496">包含されていない型がほとんど見つからない場合、特定の縮小変換は行われません。</span><span class="sxs-lookup"><span data-stu-id="24e55-496">If no most encompassed type can be found, then there is no most specific narrowing conversion.</span></span>

5. <span data-ttu-id="24e55-497">その後、ただ1つの変換演算子が最も具体的な変換元の型から最も限定的なターゲット型に変換する場合は、これが最も具体的な変換演算子です。</span><span class="sxs-lookup"><span data-stu-id="24e55-497">Then, if exactly one conversion operator converts from the most specific source type to the most specific target type, then this is the most specific conversion operator.</span></span> <span data-ttu-id="24e55-498">このような演算子が複数存在する場合、特定の縮小変換は行われません。</span><span class="sxs-lookup"><span data-stu-id="24e55-498">If more than one such operator exists, then there is no most specific narrowing conversion.</span></span>

## <a name="native-conversions"></a><span data-ttu-id="24e55-499">ネイティブ変換</span><span class="sxs-lookup"><span data-stu-id="24e55-499">Native Conversions</span></span>

<span data-ttu-id="24e55-500">一部の変換は、.NET Framework によってネイティブにサポートされているため、*ネイティブ変換*として分類されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-500">Several of the conversions are classified as *native conversions* because they are supported natively by the .NET Framework.</span></span> <span data-ttu-id="24e55-501">これらの変換は、`DirectCast` および `TryCast` 変換演算子とその他の特殊な動作を使用することによって最適化できるものです。</span><span class="sxs-lookup"><span data-stu-id="24e55-501">These conversions are ones that can be optimized through the use of the `DirectCast` and `TryCast` conversion operators, as well as other special behaviors.</span></span> <span data-ttu-id="24e55-502">ネイティブ変換として分類される変換は、id 変換、既定の変換、参照変換、配列の変換、値型の変換、および型パラメーターの変換です。</span><span class="sxs-lookup"><span data-stu-id="24e55-502">The conversions classified as native conversions are: identity conversions, default conversions, reference conversions, array conversions, value type conversions, and type parameter conversions.</span></span>

## <a name="dominant-type"></a><span data-ttu-id="24e55-503">優先する型</span><span class="sxs-lookup"><span data-stu-id="24e55-503">Dominant Type</span></span>

<span data-ttu-id="24e55-504">型のセットを指定した場合、通常は、型の推定などの状況で、セットの優先度の*高い型*を判断する必要があります。</span><span class="sxs-lookup"><span data-stu-id="24e55-504">Given a set of types, it is often necessary in situations such as type inference to determine the *dominant type* of the set.</span></span> <span data-ttu-id="24e55-505">型のセットの優先される型は、1つ以上の他の型が暗黙的に変換されていない型を最初に削除することによって決定されます。</span><span class="sxs-lookup"><span data-stu-id="24e55-505">The dominant type of a set of types is determined by first removing any types that one or more other types do not have an implicit conversion to.</span></span> <span data-ttu-id="24e55-506">この時点で型がない場合は、優先度の高い型はありません。</span><span class="sxs-lookup"><span data-stu-id="24e55-506">If there are no types left at this point, there is no dominant type.</span></span> <span data-ttu-id="24e55-507">最も優先される型は、残りの型の最も内側の型になります。</span><span class="sxs-lookup"><span data-stu-id="24e55-507">The dominant type is then the most encompassed of the remaining types.</span></span> <span data-ttu-id="24e55-508">最も包含されている型が複数ある場合は、優先度の高い型はありません。</span><span class="sxs-lookup"><span data-stu-id="24e55-508">If there is more than one type that is most encompassed, then there is no dominant type.</span></span>
