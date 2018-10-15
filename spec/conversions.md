# <a name="conversions"></a><span data-ttu-id="40113-101">変換</span><span class="sxs-lookup"><span data-stu-id="40113-101">Conversions</span></span>

<span data-ttu-id="40113-102">変換は、別の 1 つの型から値を変更するプロセスです。</span><span class="sxs-lookup"><span data-stu-id="40113-102">Conversion is the process of changing a value from one type to another.</span></span> <span data-ttu-id="40113-103">たとえば、型の値`Integer`型の値に変換できる`Double`、または型の値`Derived`型の値に変換できる`Base`されている場合、`Base`と`Derived`は、両方のクラスおよび`Derived`継承`Base`します。</span><span class="sxs-lookup"><span data-stu-id="40113-103">For example, a value of type `Integer` can be converted to a value of type `Double`, or a value of type `Derived` can be converted to a value of type `Base`, assuming that `Base` and `Derived` are both classes and `Derived` inherits from `Base`.</span></span> <span data-ttu-id="40113-104">変換 (例のように、後者)、変更するには値自体は必要としないまたは (前の例では) のように値の表現に大幅な変更を必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="40113-104">Conversions may not require the value itself to change (as in the latter example), or they may require significant changes in the value representation (as in the former example).</span></span>

<span data-ttu-id="40113-105">変換がある拡張または縮小します。</span><span class="sxs-lookup"><span data-stu-id="40113-105">Conversions may either be widening or narrowing.</span></span> <span data-ttu-id="40113-106">A*拡大変換*型から値ドメインが以上の容量を元の型の値のドメインよりも大きいにない場合、別の型に変換します。</span><span class="sxs-lookup"><span data-stu-id="40113-106">A *widening conversion* is a conversion from a type to another type whose value domain is at least as big, if not bigger, than the original type's value domain.</span></span> <span data-ttu-id="40113-107">拡大変換が失敗しないはずです。</span><span class="sxs-lookup"><span data-stu-id="40113-107">Widening conversions should never fail.</span></span> <span data-ttu-id="40113-108">A*縮小変換*は、型から値ドメインがドメインの元の型よりも小さい値か、十分に関係のないその余分な注意が必要場合に別の型への変換で変換を行う (例では、変換するときに`Integer`に`String`)。</span><span class="sxs-lookup"><span data-stu-id="40113-108">A *narrowing conversion* is a conversion from a type to another type whose value domain is either smaller than the original type's value domain or sufficiently unrelated that extra care must be taken when doing the conversion (for example, when converting from `Integer` to `String`).</span></span> <span data-ttu-id="40113-109">縮小変換で、情報の損失を伴う場合がありますは失敗します。</span><span class="sxs-lookup"><span data-stu-id="40113-109">Narrowing conversions, which may entail loss of information, can fail.</span></span>

<span data-ttu-id="40113-110">Id 変換 (つまりからへの変換、型自体) と既定値の変換 (つまりからの変換`Nothing`) すべての種類に対して定義されています。</span><span class="sxs-lookup"><span data-stu-id="40113-110">The identity conversion (i.e. a conversion from a type to itself) and default value conversion (i.e. a conversion from `Nothing`) are defined for all types.</span></span>

## <a name="implicit-and-explicit-conversions"></a><span data-ttu-id="40113-111">暗黙の型変換と明示的な型変換</span><span class="sxs-lookup"><span data-stu-id="40113-111">Implicit and Explicit Conversions</span></span>

<span data-ttu-id="40113-112">変換には、いずれかを指定できる*暗黙的な*または*明示的な*します。</span><span class="sxs-lookup"><span data-stu-id="40113-112">Conversions can be either *implicit* or *explicit*.</span></span> <span data-ttu-id="40113-113">任意の特殊な構文を使用しないで、暗黙的な変換が行われます。</span><span class="sxs-lookup"><span data-stu-id="40113-113">Implicit conversions occur without any special syntax.</span></span> <span data-ttu-id="40113-114">暗黙的な変換の例を次に、`Integer`値を`Long`値。</span><span class="sxs-lookup"><span data-stu-id="40113-114">The following is an example of implicit conversion of an `Integer` value to a `Long` value:</span></span>

```vb
Module Test
    Sub Main()
        Dim intValue As Integer = 123
        Dim longValue As Long = intValue

        Console.WriteLine(intValue & " = " & longValue)
    End Sub
End Module
```

明示的な変換には、その一方で、キャスト演算子が必要です。 キャスト演算子を使用せずに、明示的な変換を行うとすると、コンパイル時エラーが発生します。 <span data-ttu-id="40113-117">次の例では、変換する明示的な変換を使用して、`Long`値を`Integer`値。</span><span class="sxs-lookup"><span data-stu-id="40113-117">The following example uses an explicit conversion to convert a `Long` value to an `Integer` value.</span></span>

```vb
Module Test
    Sub Main()
        Dim longValue As Long = 134
        Dim intValue As Integer = CInt(longValue)

        Console.WriteLine(longValue & " = " & intValue)
    End Sub
End Module
```

暗黙的な変換のセットは、コンパイル環境によって異なります、`Option Strict`ステートメント。 厳密な型を使用しているのみ拡大変換が暗黙的に発生します。 <span data-ttu-id="40113-120">制限の緩やかなセマンティクスを使用しているすべての拡大と縮小変換 (つまり、すべての変換) が暗黙的に発生します。</span><span class="sxs-lookup"><span data-stu-id="40113-120">If permissive semantics are being used, all widening and narrowing conversions (in other words, all conversions) may occur implicitly.</span></span>

## <a name="boolean-conversions"></a><span data-ttu-id="40113-121">ブール型変換</span><span class="sxs-lookup"><span data-stu-id="40113-121">Boolean Conversions</span></span>

<span data-ttu-id="40113-122">`Boolean` 、数値型ではないが列挙型の場合と同様に、数値型との変換を縮小します。</span><span class="sxs-lookup"><span data-stu-id="40113-122">Although `Boolean` is not a numeric type, it does have narrowing conversions to and from the numeric types as if it were an enumerated type.</span></span> <span data-ttu-id="40113-123">リテラル`True`リテラルに変換`255`の`Byte`、`65535`の`UShort`、`4294967295`の`UInteger`、`18446744073709551615`の`ULong`と式に`-1`の`SByte`、 `Short`、 `Integer`、 `Long`、 `Decimal`、 `Single`、および`Double`します。</span><span class="sxs-lookup"><span data-stu-id="40113-123">The literal `True` converts to the literal `255` for `Byte`, `65535` for `UShort`, `4294967295` for `UInteger`, `18446744073709551615` for `ULong`, and to the expression `-1` for `SByte`, `Short`, `Integer`, `Long`, `Decimal`, `Single`, and `Double`.</span></span> <span data-ttu-id="40113-124">リテラル`False`リテラルに変換`0`します。</span><span class="sxs-lookup"><span data-stu-id="40113-124">The literal `False` converts to the literal `0`.</span></span> <span data-ttu-id="40113-125">ゼロの数値リテラルに変換`False`します。</span><span class="sxs-lookup"><span data-stu-id="40113-125">A zero numeric value converts to the literal `False`.</span></span> <span data-ttu-id="40113-126">その他のすべての数値の値が、リテラルに変換`True`します。</span><span class="sxs-lookup"><span data-stu-id="40113-126">All other numeric values convert to the literal `True`.</span></span>

<span data-ttu-id="40113-127">ブール値から変換する文字列への縮小変換がある`System.Boolean.TrueString`または`System.Boolean.FalseString`します。</span><span class="sxs-lookup"><span data-stu-id="40113-127">There is a narrowing conversion from Boolean to String, converting to either `System.Boolean.TrueString` or `System.Boolean.FalseString`.</span></span> <span data-ttu-id="40113-128">縮小変換も`String`に`Boolean`: 文字列が等しい場合`TrueString`または`FalseString`(現在のカルチャで大文字小文字) の適切な値が使用し、; として文字列を解析しようとするそれ以外の場合、数値型 (16 進数または 8 進数の場合に可能であれば、それ以外の場合、浮動小数点数と)、上記の規則を使用してそれ以外の場合にスロー`System.InvalidCastException`します。</span><span class="sxs-lookup"><span data-stu-id="40113-128">There is also a narrowing conversion from `String` to `Boolean`: if the string was equal to `TrueString` or `FalseString` (in the current culture, case-insensitively) then it uses the appropriate value; otherwise it attempts to parse the string as a numeric type (in hex or octal if possible, otherwise as a float) and uses the above rules; otherwise it throws `System.InvalidCastException`.</span></span>

## <a name="numeric-conversions"></a><span data-ttu-id="40113-129">数値変換</span><span class="sxs-lookup"><span data-stu-id="40113-129">Numeric Conversions</span></span>

<span data-ttu-id="40113-130">数値変換は、型の間存在`Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、 `ULong`、 `Long`、 `Decimal`、`Single`と`Double`、すべての種類を列挙します。</span><span class="sxs-lookup"><span data-stu-id="40113-130">Numeric conversions exist between the types `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single` and `Double`, and all enumerated types.</span></span> <span data-ttu-id="40113-131">変換されるときに、列挙型は、場合、基になる型と同様に扱われます。</span><span class="sxs-lookup"><span data-stu-id="40113-131">When being converted, enumerated types are treated as if they were their underlying types.</span></span> <span data-ttu-id="40113-132">列挙型への変換、変換元の値では、列挙型で定義されている値のセットに準拠する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="40113-132">When converting to an enumerated type, the source value is not required to conform to the set of values defined in the enumerated type.</span></span> <span data-ttu-id="40113-133">例えば:</span><span class="sxs-lookup"><span data-stu-id="40113-133">For example:</span></span>

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

<span data-ttu-id="40113-134">数値変換が実行時に、次のように処理されます。</span><span class="sxs-lookup"><span data-stu-id="40113-134">Numeric conversions are processed at run-time as follows:</span></span>

* <span data-ttu-id="40113-135">数値型からより広い数値型への変換では、値は単に幅の広い型に変換します。</span><span class="sxs-lookup"><span data-stu-id="40113-135">For a conversion from a numeric type to a wider numeric type, the value is simply converted to the wider type.</span></span> <span data-ttu-id="40113-136">変換`UInteger`、 `Integer`、 `ULong`、 `Long`、または`Decimal`に`Single`または`Double`に丸められますが、最も近い`Single`または`Double`値。</span><span class="sxs-lookup"><span data-stu-id="40113-136">Conversions from `UInteger`, `Integer`, `ULong`, `Long`, or `Decimal` to `Single` or `Double` are rounded to the nearest `Single` or `Double` value.</span></span> <span data-ttu-id="40113-137">この変換は、精度の損失を引き起こす可能性があります、その 1 桁の損失は発生しません。</span><span class="sxs-lookup"><span data-stu-id="40113-137">While this conversion may cause a loss of precision, it will never cause a loss of magnitude.</span></span>

* <span data-ttu-id="40113-138">整数型から別の整数型との間の変換の`Single`、 `Double`、または`Decimal`、整数型に結果がで整数のオーバーフロー チェックがかどうかが異なります。</span><span class="sxs-lookup"><span data-stu-id="40113-138">For a conversion from an integral type to another integral type, or from `Single`, `Double`, or `Decimal` to an integral type, the result depends on whether integer overflow checking is on:</span></span>

  <span data-ttu-id="40113-139">*整数のオーバーフローがチェックされる: 場合*</span><span class="sxs-lookup"><span data-stu-id="40113-139">*If integer overflow is being checked:*</span></span>

  * <span data-ttu-id="40113-140">ソースが整数型の場合は、ソース引数が変換先の型の範囲内にある場合、変換が成功します。</span><span class="sxs-lookup"><span data-stu-id="40113-140">If the source is an integral type, the conversion succeeds if the source argument is within the range of the destination type.</span></span> <span data-ttu-id="40113-141">変換がスローされます、`System.OverflowException`ソース引数が変換先の型の範囲外にある場合は例外です。</span><span class="sxs-lookup"><span data-stu-id="40113-141">The conversion throws a `System.OverflowException` exception if the source argument is outside the range of the destination type.</span></span>

  * <span data-ttu-id="40113-142">ソースの場合`Single`、 `Double`、または`Decimal`ソース値は、最も近い整数値の上下に丸められますが、この整数値の変換の結果になります。</span><span class="sxs-lookup"><span data-stu-id="40113-142">If the source is `Single`, `Double`, or `Decimal`, the source value is rounded up or down to the nearest integral value, and this integral value becomes the result of the conversion.</span></span> <span data-ttu-id="40113-143">ソース値が同じように 2 つの整数値に近い値は、最下位ビットが 0 である値に丸められます。</span><span class="sxs-lookup"><span data-stu-id="40113-143">If the source value is equally close to two integral values, the value is rounded to the value that has an even number in the least significant digit position.</span></span> <span data-ttu-id="40113-144">結果の整数値が変換先の型の範囲外の場合、`System.OverflowException`例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="40113-144">If the resulting integral value is outside the range of the destination type, a `System.OverflowException` exception is thrown.</span></span>

  <span data-ttu-id="40113-145">*整数のオーバーフローがチェックされない: 場合*</span><span class="sxs-lookup"><span data-stu-id="40113-145">*If integer overflow is not being checked:*</span></span>

  * <span data-ttu-id="40113-146">ソースが整数型の場合は、変換は常に成功し、だけで構成される元の値の最上位ビットを破棄します。</span><span class="sxs-lookup"><span data-stu-id="40113-146">If the source is an integral type, the conversion always succeeds and simply consists of discarding the most significant bits of the source value.</span></span>

  * <span data-ttu-id="40113-147">ソースの場合`Single`、 `Double`、または`Decimal`変換は常に成功し、元の値を最も近い整数値に丸め処理を行うだけで構成されます。</span><span class="sxs-lookup"><span data-stu-id="40113-147">If the source is `Single`, `Double`, or `Decimal`, the conversion always succeeds and simply consists of rounding the source value towards the nearest integral value.</span></span> <span data-ttu-id="40113-148">ソース値が同じように 2 つの整数値に近い場合、値は常に偶数の最下位ビットが 0 である値に丸められます。</span><span class="sxs-lookup"><span data-stu-id="40113-148">If the source value is equally close to two integral values, the value is always rounded to the value that has an even number in the least significant digit position.</span></span>

* <span data-ttu-id="40113-149">変換の`Double`に`Single`、`Double`値に丸められます、最も近い`Single`値。</span><span class="sxs-lookup"><span data-stu-id="40113-149">For a conversion from `Double` to `Single`, the `Double` value is rounded to the nearest `Single` value.</span></span> <span data-ttu-id="40113-150">場合、`Double`として表す値が小さすぎる、`Single`結果が正のゼロまたは負のゼロ。</span><span class="sxs-lookup"><span data-stu-id="40113-150">If the `Double` value is too small to represent as a `Single`, the result becomes positive zero or negative zero.</span></span> <span data-ttu-id="40113-151">場合、`Double`として表す値が大きすぎて、`Single`結果は正の無限大または負の無限大になります。</span><span class="sxs-lookup"><span data-stu-id="40113-151">If the `Double` value is too large to represent as a `Single`, the result becomes positive infinity or negative infinity.</span></span> <span data-ttu-id="40113-152">場合、`Double`値は`NaN`、結果も`NaN`します。</span><span class="sxs-lookup"><span data-stu-id="40113-152">If the `Double` value is `NaN`, the result is also `NaN`.</span></span>

* <span data-ttu-id="40113-153">変換の`Single`または`Double`に`Decimal`、ソース値に変換されます`Decimal`表現し、必要な場合、28 小数点後近似値に丸められます。</span><span class="sxs-lookup"><span data-stu-id="40113-153">For a conversion from `Single` or `Double` to `Decimal`, the source value is converted to `Decimal` representation and rounded to the nearest number after the 28th decimal place if required.</span></span> <span data-ttu-id="40113-154">ソース値が小さすぎてとして表すかどうか、`Decimal`結果はゼロになります。</span><span class="sxs-lookup"><span data-stu-id="40113-154">If the source value is too small to represent as a `Decimal`, the result becomes zero.</span></span> <span data-ttu-id="40113-155">元の値が場合`NaN`、無限大、または大きすぎてとして表す、 `Decimal`、`System.OverflowException`例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="40113-155">If the source value is `NaN`, infinity, or too large to represent as a `Decimal`, a `System.OverflowException` exception is thrown.</span></span>

* <span data-ttu-id="40113-156">変換の`Double`に`Single`、`Double`値に丸められます、最も近い`Single`値。</span><span class="sxs-lookup"><span data-stu-id="40113-156">For a conversion from `Double` to `Single`, the `Double` value is rounded to the nearest `Single` value.</span></span> <span data-ttu-id="40113-157">場合、`Double`として表す値が小さすぎる、`Single`結果が正のゼロまたは負のゼロ。</span><span class="sxs-lookup"><span data-stu-id="40113-157">If the `Double` value is too small to represent as a `Single`, the result becomes positive zero or negative zero.</span></span> <span data-ttu-id="40113-158">場合、`Double`として表す値が大きすぎて、`Single`結果は正の無限大または負の無限大になります。</span><span class="sxs-lookup"><span data-stu-id="40113-158">If the `Double` value is too large to represent as a `Single`, the result becomes positive infinity or negative infinity.</span></span> <span data-ttu-id="40113-159">場合、`Double`値は`NaN`、結果も`NaN`します。</span><span class="sxs-lookup"><span data-stu-id="40113-159">If the `Double` value is `NaN`, the result is also `NaN`.</span></span>

## <a name="reference-conversions"></a><span data-ttu-id="40113-160">参照変換</span><span class="sxs-lookup"><span data-stu-id="40113-160">Reference Conversions</span></span>

<span data-ttu-id="40113-161">参照型は、基本型の場合は、その逆に変換可能性があります。</span><span class="sxs-lookup"><span data-stu-id="40113-161">Reference types may be converted to a base type, and vice versa.</span></span> <span data-ttu-id="40113-162">変換される値が null 値を派生型自体、またはより強い派生型の場合、基本型からより強い派生型への変換は実行時にのみ成功します。</span><span class="sxs-lookup"><span data-stu-id="40113-162">Conversions from a base type to a more derived type only succeed at run time if the value being converted is a null value, the derived type itself, or a more derived type.</span></span>

<span data-ttu-id="40113-163">クラスとインターフェイスの型は、任意のインターフェイス型との間にキャストできます。</span><span class="sxs-lookup"><span data-stu-id="40113-163">Class and interface types can be cast to and from any interface type.</span></span> <span data-ttu-id="40113-164">関係する実際の型が継承または実装の関係がある場合、型とインターフェイスの型の間の変換は実行時にのみ成功します。</span><span class="sxs-lookup"><span data-stu-id="40113-164">Conversions between a type and an interface type only succeed at run time if the actual types involved have an inheritance or implementation relationship.</span></span> <span data-ttu-id="40113-165">インターフェイス型がから派生した型のインスタンスを常に格納されるため`Object`との間、インターフェイス型をキャストすることができますも必ず`Object`します。</span><span class="sxs-lookup"><span data-stu-id="40113-165">Because an interface type will always contain an instance of a type that derives from `Object`, an interface type can also always be cast to and from `Object`.</span></span>

<span data-ttu-id="40113-166">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="40113-166">__Note.__</span></span> <span data-ttu-id="40113-167">変換するとエラーには、`NotInheritable`クラスと COM クラスを表すクラスのインターフェイスの実装まで不明な可能性があるためにを実装していないインターフェイスの実行時間。</span><span class="sxs-lookup"><span data-stu-id="40113-167">It is not an error to convert a `NotInheritable` classes to and from interfaces that it does not implement because classes that represent COM classes may have interface implementations that are not known until run time.</span></span> 

<span data-ttu-id="40113-168">実行時に、参照変換が失敗した場合、`System.InvalidCastException`例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="40113-168">If a reference conversion fails at run time, a `System.InvalidCastException` exception is thrown.</span></span>

### <a name="reference-variance-conversions"></a><span data-ttu-id="40113-169">参照の差異の変換</span><span class="sxs-lookup"><span data-stu-id="40113-169">Reference Variance Conversions</span></span>

<span data-ttu-id="40113-170">ジェネリック インターフェイスまたはデリゲートには、互換性のあるバリアント型の間の変換を許可するバリアント型パラメーターがある場合があります。</span><span class="sxs-lookup"><span data-stu-id="40113-170">Generic interfaces or delegates may have variant type parameters that allow conversions between compatible variants of the type.</span></span> <span data-ttu-id="40113-171">そのため、実行時に、クラス型またはインターフェイス型からバリアント インターフェイス型から継承または実装と互換性のあるインターフェイス型への変換は成功します。</span><span class="sxs-lookup"><span data-stu-id="40113-171">Therefore, at runtime a conversion from a class type or an interface type to an interface type that is variant compatible with an interface type it inherits from or implements will succeed.</span></span> <span data-ttu-id="40113-172">同様に、デリゲート型、および互換性のあるバリアントからデリゲートの型にキャストすることができます。</span><span class="sxs-lookup"><span data-stu-id="40113-172">Similarly, delegate types can be cast to and from variant compatible delegate types.</span></span> <span data-ttu-id="40113-173">たとえば、デリゲート型</span><span class="sxs-lookup"><span data-stu-id="40113-173">For example, the delegate type</span></span>

```vb
Delegate Function F(Of In A, Out R)(a As A) As R
```

<span data-ttu-id="40113-174">変換を許可するよう`F(Of Object, Integer)`に`F(Of String, Integer)`します。</span><span class="sxs-lookup"><span data-stu-id="40113-174">would allow a conversion from `F(Of Object, Integer)` to `F(Of String, Integer)`.</span></span> <span data-ttu-id="40113-175">つまり、デリゲート`F`受け取り`Object`デリゲートとして安全に使用できる可能性があります`F`受け取り`String`します。</span><span class="sxs-lookup"><span data-stu-id="40113-175">That is, a delegate `F` which takes `Object` may be safely used as a delegate `F` which takes `String`.</span></span> <span data-ttu-id="40113-176">デリゲートが呼び出されたときに、対象のメソッドは、オブジェクトが想定され、文字列は、オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="40113-176">When the delegate is invoked, the target method will be expecting an object, and a string is an object.</span></span>

<span data-ttu-id="40113-177">ジェネリック デリゲートまたはインターフェイス型`S(Of S1,...,Sn)`言います*バリアント型の互換性のある*ジェネリック インターフェイスまたはデリゲート型で`T(Of T1,...,Tn)`場合。</span><span class="sxs-lookup"><span data-stu-id="40113-177">A generic delegate or interface type `S(Of S1,...,Sn)` is said to be *variant compatible* with a generic interface or delegate type `T(Of T1,...,Tn)` if:</span></span>

* <span data-ttu-id="40113-178">`S` `T`同じジェネリック型から構築された両方`U(Of U1,...,Un)`します。</span><span class="sxs-lookup"><span data-stu-id="40113-178">`S` and `T` are both constructed from the same generic type `U(Of U1,...,Un)`.</span></span>

* <span data-ttu-id="40113-179">型パラメーターごと`Ux`:</span><span class="sxs-lookup"><span data-stu-id="40113-179">For each type parameter `Ux`:</span></span>

  * <span data-ttu-id="40113-180">なし、分散型のパラメーターで宣言された場合`Sx`と`Tx`同じ型でなければなりません。</span><span class="sxs-lookup"><span data-stu-id="40113-180">If the type parameter was declared without variance then `Sx` and `Tx` must be the same type.</span></span>

  * <span data-ttu-id="40113-181">型パラメーターが宣言されていた場合`In`、拡大 id、既定値、参照、配列、または型がある必要がありますし、パラメーターの変換から`Sx`に`Tx`します。</span><span class="sxs-lookup"><span data-stu-id="40113-181">If the type parameter was declared `In` then there must be a widening identity, default, reference, array, or type parameter conversion from `Sx` to `Tx`.</span></span>

  * <span data-ttu-id="40113-182">型パラメーターが宣言されていた場合`Out`、拡大 id、既定値、参照、配列、または型がある必要がありますし、パラメーターの変換から`Tx`に`Sx`します。</span><span class="sxs-lookup"><span data-stu-id="40113-182">If the type parameter was declared `Out` then there must be a widening identity, default, reference, array, or type parameter conversion from `Tx` to `Sx`.</span></span>

<span data-ttu-id="40113-183">クラスは、バリアント 1 つ以上の互換性のあるインターフェイスを実装している場合は、バリアント型パラメーターを持つジェネリック インターフェイスをクラスから変換するときに非 variant の変換がない場合、変換はあいまいです。</span><span class="sxs-lookup"><span data-stu-id="40113-183">When converting from a class to a generic interface with variant type parameters, if the class implements more than one variant compatible interface the conversion is ambiguous if there is not a non-variant conversion.</span></span> <span data-ttu-id="40113-184">例えば:</span><span class="sxs-lookup"><span data-stu-id="40113-184">For example:</span></span>

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

### <a name="anonymous-delegate-conversions"></a><span data-ttu-id="40113-185">匿名デリゲートの変換</span><span class="sxs-lookup"><span data-stu-id="40113-185">Anonymous Delegate Conversions</span></span>

<span data-ttu-id="40113-186">ラムダ メソッドとして分類される式がコンテキスト内の値として再分類とターゲットの型がない (たとえば、 `Dim x = Function(a As Integer, b As Integer) a + b`)、結果として得られる式の型、匿名デリゲートの型は、対象の型がデリゲート型ではない、またはラムダ メソッドのシグネチャと等価です。</span><span class="sxs-lookup"><span data-stu-id="40113-186">When an expression classified as a lambda method is reclassified as a value in a context where there is no target type (for example, `Dim x = Function(a As Integer, b As Integer) a + b`), or where the target type is not a delegate type, the type of the resulting expression is an anonymous delegate type equivalent to the signature of the lambda method.</span></span> <span data-ttu-id="40113-187">この匿名のデリゲート型が互換性のあるデリゲート型への変換: 互換性のあるデリゲート型がデリゲート型と匿名デリゲート型のデリゲート作成式を使用して作成できる`Invoke`メソッドをパラメーターとして。</span><span class="sxs-lookup"><span data-stu-id="40113-187">This anonymous delegate type has a conversion to any compatible delegate type: a compatible delegate type is any delegate type that can be created using a delegate creation expression with the anonymous delegate type's `Invoke` method as a parameter.</span></span> <span data-ttu-id="40113-188">例えば:</span><span class="sxs-lookup"><span data-stu-id="40113-188">For example:</span></span>

```vb
' Anonymous delegate type similar to Func(Of Object, Object, Object)
Dim x = Function(x, y) x + y

' OK because delegate type is compatible
Dim y As Func(Of Integer, Integer, Integer) = x
```

<span data-ttu-id="40113-189">なお、種類`System.Delegate`と`System.MulticastDelegate`自体ではないと見なされますデリゲート型 (でもすべてのデリゲート型は、そこから継承)。</span><span class="sxs-lookup"><span data-stu-id="40113-189">Note that the types `System.Delegate` and `System.MulticastDelegate` are not themselves considered delegate types (even though all delegate types inherit from them).</span></span> <span data-ttu-id="40113-190">また、互換性のあるデリゲート型への匿名デリゲートの型から変換が参照変換ではないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="40113-190">Also, note that the conversion from anonymous delegate type to a compatible delegate type is not a reference conversion.</span></span>

## <a name="array-conversions"></a><span data-ttu-id="40113-191">配列の変換</span><span class="sxs-lookup"><span data-stu-id="40113-191">Array Conversions</span></span>

<span data-ttu-id="40113-192">だけでなく、実際には参照型であることにより、配列で定義されている、変換は、いくつかの特別な変換は、配列に存在します。</span><span class="sxs-lookup"><span data-stu-id="40113-192">Besides the conversions that are defined on arrays by virtue of the fact that they are reference types, several special conversions exist for arrays.</span></span>

<span data-ttu-id="40113-193">2 つの型`A`と`B`場合、両方の参照型または型パラメーターの値の型であるとわかっていない場合および`A`参照、配列、または入力パラメーターの変換に含まれています`B`の配列からの変換が存在します型`A`型の配列を`B`同じランクを持つ。</span><span class="sxs-lookup"><span data-stu-id="40113-193">For any two types `A` and `B`, if they are both reference types or type parameters not known to be value types, and if `A` has a reference, array, or type parameter conversion to `B`, a conversion exists from an array of type `A` to an array of type `B` with the same rank.</span></span> <span data-ttu-id="40113-194">このリレーションシップと呼ばれる*配列の共変性*します。</span><span class="sxs-lookup"><span data-stu-id="40113-194">This relationship is known as *array covariance*.</span></span> <span data-ttu-id="40113-195">配列の共変性であることを意味要素型が配列の要素`B`要素型が配列の要素が実際にあります`A`、その両方`A`と`B`は参照型とその`B`には、参照変換または配列に変換する`A`します。</span><span class="sxs-lookup"><span data-stu-id="40113-195">Array covariance in particular means that an element of an array whose element type is `B` may actually be an element of an array whose element type is `A`, provided that both `A` and `B` are reference types and that `B` has a reference conversion or array conversion to `A`.</span></span> <span data-ttu-id="40113-196">次の例の 2 番目の呼び出しで`F`により、`System.ArrayTypeMismatchException`が、実際の要素の入力のためにスローされる例外`b`は`String`ではなく、 `Object`:</span><span class="sxs-lookup"><span data-stu-id="40113-196">In the following example, the second invocation of `F` causes a `System.ArrayTypeMismatchException` exception to be thrown because the actual element type of `b` is `String`, not `Object`:</span></span>

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

<span data-ttu-id="40113-197">配列の共変性により参照型の配列の要素への代入には、により許可されている型の配列の要素に割り当てられている値が実際には、実行時のチェックが含まれます。</span><span class="sxs-lookup"><span data-stu-id="40113-197">Because of array covariance, assignments to elements of reference type arrays include a run-time check that ensures that the value being assigned to the array element is actually of a permitted type.</span></span>

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

<span data-ttu-id="40113-198">この例への代入で`array(i)`メソッドで`Fill`変数によって参照されるオブジェクトに、ランタイム チェックを暗黙的に含まれます`value`いずれかです`Nothing`またはと互換性がある型のインスタンス、配列の実際の要素型`array`します。</span><span class="sxs-lookup"><span data-stu-id="40113-198">In this example, the assignment to `array(i)` in method `Fill` implicitly includes a run-time check that ensures that the object referenced by the variable `value` is either `Nothing` or an instance of a type that is compatible with the actual element type of array `array`.</span></span> <span data-ttu-id="40113-199">メソッドで`Main`、メソッドの最初の 2 つの呼び出し`Fill`失敗すると、3 つ目の呼び出しの原因が、`System.ArrayTypeMismatchException`への最初の割り当てを実行したときにスローされる例外`array(i)`。</span><span class="sxs-lookup"><span data-stu-id="40113-199">In method `Main`, the first two invocations of method `Fill` succeed, but the third invocation causes a `System.ArrayTypeMismatchException` exception to be thrown upon executing the first assignment to `array(i)`.</span></span> <span data-ttu-id="40113-200">例外が発生するため、`Integer`で格納することはできません、`String`配列。</span><span class="sxs-lookup"><span data-stu-id="40113-200">The exception occurs because an `Integer` cannot be stored in a `String` array.</span></span>

<span data-ttu-id="40113-201">配列の要素型のいずれかが、実行時に値型であることがわかりました型を持つ型パラメーターの場合、`System.InvalidCastException`例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="40113-201">If one of the array element types is a type parameter whose type turns out to be a value type at runtime, a `System.InvalidCastException` exception will be thrown.</span></span> <span data-ttu-id="40113-202">例えば:</span><span class="sxs-lookup"><span data-stu-id="40113-202">For example:</span></span>

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

<span data-ttu-id="40113-203">変換は、列挙型の配列の間も存在し、列挙型の配列の基になる型または同じの基になる型を持つ別の列挙型の配列、配列は同じランクを持っています。</span><span class="sxs-lookup"><span data-stu-id="40113-203">Conversions also exist between an array of an enumerated type and an array of the enumerated type's underlying type or an array of another enumerated type with the same underlying type, provided the arrays have the same rank.</span></span>

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

<span data-ttu-id="40113-204">この例では、配列の`Color`の配列との間に変換`Byte`、`Color`の基になる型。</span><span class="sxs-lookup"><span data-stu-id="40113-204">In this example, an array of `Color` is converted to and from an array of `Byte`, `Color`'s underlying type.</span></span> <span data-ttu-id="40113-205">配列への変換`Integer`、ただしはエラーになりますので`Integer`の基になる型ではない`Color`します。</span><span class="sxs-lookup"><span data-stu-id="40113-205">The conversion to an array of `Integer`, however, will be an error because `Integer` is not the underlying type of `Color`.</span></span>

<span data-ttu-id="40113-206">型のランク 1 の配列`A()`も、コレクション インターフェイス型への配列変換`IList(Of B)`、 `IReadOnlyList(Of B)`、 `ICollection(Of B)`、`IReadOnlyCollection(Of B)`と`IEnumerable(Of B)`で見つかった`System.Collections.Generic`次のいずれかが当てはまる場合に限り。</span><span class="sxs-lookup"><span data-stu-id="40113-206">A rank-1 array of type `A()` also has an array conversion to the collection interface types `IList(Of B)`, `IReadOnlyList(Of B)`, `ICollection(Of B)`, `IReadOnlyCollection(Of B)` and `IEnumerable(Of B)` found in `System.Collections.Generic`, so long as one of the following is true:</span></span>

- <span data-ttu-id="40113-207">`A` `B`は両方の参照型または型パラメーターの値の型があるとわかっていないと`A`に拡大参照、配列または型パラメーター変換`B`; または</span><span class="sxs-lookup"><span data-stu-id="40113-207">`A` and `B` are both reference types or type parameters not known to be value types; and `A` has a widening reference, array or type parameter conversion to `B`; or</span></span>
- <span data-ttu-id="40113-208">`A` `B`は、同じ基になる型の両方の列挙型、または</span><span class="sxs-lookup"><span data-stu-id="40113-208">`A` and `B` are both enumerated types of the same underlying type; or</span></span>
- <span data-ttu-id="40113-209">いずれかの`A`と`B`は、列挙型であり、もう一方が基になる型。</span><span class="sxs-lookup"><span data-stu-id="40113-209">one of `A` and `B` is an enumerated type, and the other is its underlying type.</span></span>

<span data-ttu-id="40113-210">任意のランクを持つタイプ A のすべての配列にも、非ジェネリック コレクション インターフェイス型への配列変換`IList`、`ICollection`と`IEnumerable`で見つかった`System.Collections`します。</span><span class="sxs-lookup"><span data-stu-id="40113-210">Any array of type A with any rank also has an array conversion to the non-generic collection interface types `IList`, `ICollection` and `IEnumerable` found in `System.Collections`.</span></span>

<span data-ttu-id="40113-211">使用して、結果として得られるインターフェイスを反復処理することは`For Each`、または呼び出すことによって、`GetEnumerator`メソッドを直接します。</span><span class="sxs-lookup"><span data-stu-id="40113-211">It is possible to iterate over the resulting interfaces using `For Each`, or through invoking the `GetEnumerator` methods directly.</span></span> <span data-ttu-id="40113-212">配列のランク 1 の場合のジェネリック、または非ジェネリックの形式を変換`IList`または`ICollection`インデックスを使用して要素を取得することもできます。</span><span class="sxs-lookup"><span data-stu-id="40113-212">In the case of rank-1 arrays converted generic or non-generic forms of `IList` or `ICollection`, it is also possible to get elements by index.</span></span> <span data-ttu-id="40113-213">ジェネリック、または非ジェネリックの形式に変換ランク 1 の配列の場合`IList`インデックスを使用して要素を設定することも、配列の共変性、同じランタイムを対象とは、前述の操作を確認します。</span><span class="sxs-lookup"><span data-stu-id="40113-213">In the case of rank-1 arrays converted to generic or non-generic forms of `IList`, it is also possible to set elements by index, subject to the same runtime array covariance checks as described above.</span></span> <span data-ttu-id="40113-214">その他のすべてのインターフェイス メソッドの動作は VB 言語仕様で定義されていません基になるランタイムの責任です。</span><span class="sxs-lookup"><span data-stu-id="40113-214">The behavior of all other interface methods is undefined by the VB language specification; it is up to the underlying runtime.</span></span>

## <a name="value-type-conversions"></a><span data-ttu-id="40113-215">値型の変換</span><span class="sxs-lookup"><span data-stu-id="40113-215">Value Type Conversions</span></span>

<span data-ttu-id="40113-216">値型の値をそのベース参照型またはインターフェイス型と呼ばれるプロセスを実装するのいずれかに変換できる*ボックス化*します。</span><span class="sxs-lookup"><span data-stu-id="40113-216">A value type value can be converted to one of its base reference types or an interface type that it implements through a process called *boxing*.</span></span> <span data-ttu-id="40113-217">値型の値がボックス化するとき、値は、.NET Framework ヒープ上に存在する場所の場所からコピーされます。</span><span class="sxs-lookup"><span data-stu-id="40113-217">When a value type value is boxed, the value is copied from the location where it lives onto the .NET Framework heap.</span></span> <span data-ttu-id="40113-218">ヒープ上のこの場所への参照は返され、参照型の変数に格納できます。</span><span class="sxs-lookup"><span data-stu-id="40113-218">A reference to this location on the heap is then returned and can be stored in a reference type variable.</span></span> <span data-ttu-id="40113-219">この参照と呼ばれることも、*手書き*値型のインスタンス。</span><span class="sxs-lookup"><span data-stu-id="40113-219">This reference is also referred to as a *boxed* instance of the value type.</span></span> <span data-ttu-id="40113-220">ボックス化されたインスタンスには、値型ではなく、参照型と同じセマンティクスがあります。</span><span class="sxs-lookup"><span data-stu-id="40113-220">The boxed instance has the same semantics as a reference type instead of a value type.</span></span>

<span data-ttu-id="40113-221">ボックス化された値の型と呼ばれるプロセスを介して元の値型に変換できる*ボックス化解除*します。</span><span class="sxs-lookup"><span data-stu-id="40113-221">Boxed value types can be converted back to their original value type through a process called *unboxing*.</span></span> <span data-ttu-id="40113-222">ボックス化された値の型がボックス化された値はヒープから変数の場所にコピーします。</span><span class="sxs-lookup"><span data-stu-id="40113-222">When a boxed value type is unboxed, the value is copied from the heap into a variable location.</span></span> <span data-ttu-id="40113-223">その時点から、値型の場合と動作します。</span><span class="sxs-lookup"><span data-stu-id="40113-223">From that point on, it behaves as if it was a value type.</span></span> <span data-ttu-id="40113-224">値の型をボックス化解除、ときに、値には、null 値または値型のインスタンスです。</span><span class="sxs-lookup"><span data-stu-id="40113-224">When unboxing a value type, the value must be a null value or an instance of the value type.</span></span> <span data-ttu-id="40113-225">それ以外の場合、`System.InvalidCastException`例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="40113-225">Otherwise a `System.InvalidCastException` exception is thrown.</span></span> <span data-ttu-id="40113-226">値が列挙型のインスタンスの場合は、その値もボックス化解除できます列挙型の基になる型または別の列挙型を持つ同じ基になる型。</span><span class="sxs-lookup"><span data-stu-id="40113-226">If the value is an instance of an enumerated type, that value can also be unboxed to the enumerated type's underlying type or another enumerated type that has the same underlying type.</span></span> <span data-ttu-id="40113-227">Null 値は、リテラルの場合と同様に扱われますが`Nothing`します。</span><span class="sxs-lookup"><span data-stu-id="40113-227">A null value is treated as if it were the literal `Nothing`.</span></span>

<span data-ttu-id="40113-228">Null 許容型をサポートする型、値の型に値`System.Nullable(Of T)`実行特別にする場合、ボックス化とボックス化解除が扱われます。</span><span class="sxs-lookup"><span data-stu-id="40113-228">To support nullable value types well, the value type `System.Nullable(Of T)` is treated specially when doing boxing and unboxing.</span></span> <span data-ttu-id="40113-229">型の値をボックス化`Nullable(Of T)`結果の型のボックス化された値`T`場合、値の`HasValue`プロパティが`True`かの値を`Nothing`場合、値の`HasValue`プロパティが`False`します。</span><span class="sxs-lookup"><span data-stu-id="40113-229">Boxing a value of type `Nullable(Of T)` results in a boxed value of type `T` if the value's `HasValue` property is `True` or a value of `Nothing` if the value's `HasValue` property is `False`.</span></span> <span data-ttu-id="40113-230">型の値をボックス化解除`T`に`Nullable(Of T)`のインスタンスで`Nullable(Of T)`が`Value`プロパティは、ボックス化された値と持つ`HasValue`プロパティは`True`します。</span><span class="sxs-lookup"><span data-stu-id="40113-230">Unboxing a value of type `T` to `Nullable(Of T)` results in an instance of `Nullable(Of T)` whose `Value` property is the boxed value and whose `HasValue` property is `True`.</span></span> <span data-ttu-id="40113-231">値`Nothing`にボックス化解除することができます`Nullable(Of T)`は`T`され、結果を値に持つ`HasValue`プロパティは`False`。</span><span class="sxs-lookup"><span data-stu-id="40113-231">The value `Nothing` can be unboxed to `Nullable(Of T)` for any `T` and results in a value whose `HasValue` property is `False`.</span></span> <span data-ttu-id="40113-232">ボックス化された値の型が参照のように動作するため、型が同じ値に複数の参照を作成することです。</span><span class="sxs-lookup"><span data-stu-id="40113-232">Because boxed value types behave like reference types, it is possible to create multiple references to the same value.</span></span> <span data-ttu-id="40113-233">プリミティブ型と列挙型では、これは関連しませんそれらの型のインスタンスが*不変*します。</span><span class="sxs-lookup"><span data-stu-id="40113-233">For the primitive types and enumerated types, this is irrelevant because instances of those types are *immutable*.</span></span> <span data-ttu-id="40113-234">つまり、同じ値に複数の参照があるという事実を観察することはできませんので、それらの型のボックス化されたインスタンスを変更することはできません。</span><span class="sxs-lookup"><span data-stu-id="40113-234">That is, it is not possible to modify a boxed instance of those types, so it is not possible to observe the fact that there are multiple references to the same value.</span></span>

<span data-ttu-id="40113-235">構造体、その一方で、可能性があります変更可能なインスタンス変数にアクセスできる場合、またはそのメソッドまたはプロパティは、そのインスタンス変数を変更する場合。</span><span class="sxs-lookup"><span data-stu-id="40113-235">Structures, on the other hand, may be mutable if its instance variables are accessible or if its methods or properties modify its instance variables.</span></span> <span data-ttu-id="40113-236">ボックス化された構造体への参照を 1 つを使用して構造を変更する場合、ボックス化された構造に対するすべての参照の間で変更が表示されます。</span><span class="sxs-lookup"><span data-stu-id="40113-236">If one reference to a boxed structure is used to modify the structure, then all references to the boxed structure will see the change.</span></span> <span data-ttu-id="40113-237">この結果は、予期できない可能性があります、ため場合、値として型指定された`Object`1 つの場所から別のボックス化された値型は使用するだけでそれらの参照がコピーではなく、ヒープ上に自動的に複製にコピーされます。</span><span class="sxs-lookup"><span data-stu-id="40113-237">Because this result may be unexpected, when a value typed as `Object` is copied from one location to another boxed value types will automatically be cloned on the heap instead of merely having their references copied.</span></span> <span data-ttu-id="40113-238">例えば:</span><span class="sxs-lookup"><span data-stu-id="40113-238">For example:</span></span>

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

<span data-ttu-id="40113-239">プログラムの出力は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="40113-239">The output of the program is:</span></span>

```
Values: 0, 123
Refs: 123, 123
```

<span data-ttu-id="40113-240">ローカル変数のフィールドに割り当て`val2`ローカル変数のフィールドには影響しません`val1`ためと、ボックス化された`Struct1`に割り当てられた`val2`値のコピーが行われました。</span><span class="sxs-lookup"><span data-stu-id="40113-240">The assignment to the field of the local variable `val2` does not impact the field of the local variable `val1` because when the boxed `Struct1` was assigned to `val2`, a copy of the value was made.</span></span> <span data-ttu-id="40113-241">これに対して、割り当て`ref2.Value = 123`オブジェクトに影響を両方`ref1`と`ref2`参照。</span><span class="sxs-lookup"><span data-stu-id="40113-241">In contrast, the assignment `ref2.Value = 123` affects the object that both `ref1` and `ref2` references.</span></span>

<span data-ttu-id="40113-242">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="40113-242">__Note.__</span></span> <span data-ttu-id="40113-243">として型指定されたボックス化された構造体の構造のコピーが完了しない`System.ValueType`オフの遅延をバインドすることはできませんので`System.ValueType`します。</span><span class="sxs-lookup"><span data-stu-id="40113-243">Structure copying is not done for boxed structures typed as `System.ValueType` because it is not possible to late bind off of `System.ValueType`.</span></span>

<span data-ttu-id="40113-244">型は代入時にコピーする値をボックス化のルールに 1 つの例外があります。</span><span class="sxs-lookup"><span data-stu-id="40113-244">There is one exception to the rule that boxed value types will be copied on assignment.</span></span> <span data-ttu-id="40113-245">ボックス化された値の型参照が別の型で格納されている場合、内部の参照はコピーされません。</span><span class="sxs-lookup"><span data-stu-id="40113-245">If a boxed value type reference is stored within another type, the inner reference will not be copied.</span></span> <span data-ttu-id="40113-246">例えば:</span><span class="sxs-lookup"><span data-stu-id="40113-246">For example:</span></span>

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

<span data-ttu-id="40113-247">プログラムの出力は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="40113-247">The output of the program is:</span></span>

```
Values: 123, 123
```

<span data-ttu-id="40113-248">これは、値をコピーするときに内部のボックス化された値がコピーされないためにです。</span><span class="sxs-lookup"><span data-stu-id="40113-248">This is because the inner boxed value is not copied when the value is copied.</span></span> <span data-ttu-id="40113-249">したがって、どちらも`val1.Value`と`val2.Value`同じボックス化された値型への参照します。</span><span class="sxs-lookup"><span data-stu-id="40113-249">Thus, both `val1.Value` and `val2.Value` have a reference to the same boxed value type.</span></span>

<span data-ttu-id="40113-250">__注意してください。__</span><span class="sxs-lookup"><span data-stu-id="40113-250">__Note.__</span></span> <span data-ttu-id="40113-251">内部のボックス化された値の型はコピーされません実際には、.NET の制限の入力型の値のたびにすべての内部のボックス化された値型がコピーされたことを確認する - システム`Object`がコピーされた非常にコスト高になります。</span><span class="sxs-lookup"><span data-stu-id="40113-251">The fact that inner boxed value types are not copied is a limitation of the .NET type system -- to ensure that all inner boxed value types were copied whenever a value of type `Object` was copied would be prohibitively expensive.</span></span>

<span data-ttu-id="40113-252">既に説明した、ボックス化された値と型のみボックス化解除できますを元の型。</span><span class="sxs-lookup"><span data-stu-id="40113-252">As previously described, boxed value types can only be unboxed to their original type.</span></span> <span data-ttu-id="40113-253">ボックス化されたプリミティブ型、ただしは処理として型指定されたときに特別に`Object`します。</span><span class="sxs-lookup"><span data-stu-id="40113-253">Boxed primitive types, however, are treated specially when typed as `Object`.</span></span> <span data-ttu-id="40113-254">これらへの変換を持っている他のプリミティブ型に変換できます。</span><span class="sxs-lookup"><span data-stu-id="40113-254">They can be converted to any other primitive type that they have a conversion to.</span></span> <span data-ttu-id="40113-255">例えば:</span><span class="sxs-lookup"><span data-stu-id="40113-255">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim o As Object = 5
        Dim b As Byte = CByte(o)  ' Legal
        Console.WriteLine(b) ' Prints 5
    End Sub
End Module
```

<span data-ttu-id="40113-256">通常、ボックス化`Integer`値`5`にボックス化解除がない、`Byte`変数。</span><span class="sxs-lookup"><span data-stu-id="40113-256">Normally, the boxed `Integer` value `5` could not be unboxed into a `Byte` variable.</span></span> <span data-ttu-id="40113-257">ただし、ため`Integer`と`Byte`プリミティブ型であり、変換、変換は許可します。</span><span class="sxs-lookup"><span data-stu-id="40113-257">However, because `Integer` and `Byte` are primitive types and have a conversion, the conversion is allowed.</span></span>

<span data-ttu-id="40113-258">値の型に変換するインターフェイスはジェネリック引数がインターフェイスに制限するものと異なる場合に重要です。</span><span class="sxs-lookup"><span data-stu-id="40113-258">It is important to note that converting a value type to an interface is different than a generic argument constrained to an interface.</span></span> <span data-ttu-id="40113-259">制約付きの型パラメーターでインターフェイス メンバーにアクセスするときに (またはメソッドの呼び出しで`Object`)、値型はインターフェイスに変換され、インターフェイス メンバーにアクセスするときのようにボックス化が発生しません。</span><span class="sxs-lookup"><span data-stu-id="40113-259">When accessing interface members on a constrained type parameter (or calling methods on `Object`), boxing does not occur as it does when a value type is converted to an interface and an interface member is accessed.</span></span> <span data-ttu-id="40113-260">たとえば、インターフェイス`ICounter`メソッドを含む`Increment`値の変更に使用できます。</span><span class="sxs-lookup"><span data-stu-id="40113-260">For example, suppose an interface `ICounter` contains a method `Increment` which can be used to modify a value.</span></span> <span data-ttu-id="40113-261">場合`ICounter`制約の実装として提供される、`Increment`メソッドを呼び出すと、変数への参照を`Increment`で、ボックス化されたコピーではなくが呼び出されました。</span><span class="sxs-lookup"><span data-stu-id="40113-261">If `ICounter` is used as a constraint, the implementation of the `Increment` method is called with a reference to the variable that `Increment` was called on, not a boxed copy:</span></span>

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

<span data-ttu-id="40113-262">最初の呼び出し`Increment`変数に値を変更します`x`します。</span><span class="sxs-lookup"><span data-stu-id="40113-262">The first call to `Increment` modifies the value in the variable `x`.</span></span> <span data-ttu-id="40113-263">これは、2 番目の呼び出しに相当しない`Increment`、ボックス化されたコピーの値が変更されます`x`します。</span><span class="sxs-lookup"><span data-stu-id="40113-263">This is not equivalent to the second call to `Increment`, which modifies the value in a boxed copy of `x`.</span></span> <span data-ttu-id="40113-264">そのため、プログラムの出力は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="40113-264">Thus, the output of the program is:</span></span>

```
0
1
1
```

### <a name="nullable-value-type-conversions"></a><span data-ttu-id="40113-265">Null 許容値型の変換</span><span class="sxs-lookup"><span data-stu-id="40113-265">Nullable Value Type Conversions</span></span>

<span data-ttu-id="40113-266">値型`T`、型の null 許容バージョンとの間に変換できる`T?`します。</span><span class="sxs-lookup"><span data-stu-id="40113-266">A value type `T` can convert to and from the nullable version of the type, `T?`.</span></span> <span data-ttu-id="40113-267">変換`T?`に`T`がスローされます、`System.InvalidOperationException`変換される値がある場合は例外`Nothing`します。</span><span class="sxs-lookup"><span data-stu-id="40113-267">The conversion from `T?` to `T` throws a `System.InvalidOperationException` exception if the value being converted is `Nothing`.</span></span> <span data-ttu-id="40113-268">また、`T?`型への変換が`S`場合`T`への組み込みの変換が`S`します。</span><span class="sxs-lookup"><span data-stu-id="40113-268">Also, `T?` has a conversion to a type `S` if `T` has an intrinsic conversion to `S`.</span></span> <span data-ttu-id="40113-269">場合`S`間に、次の組み込み変換が存在し、値の型は、`T?`と`S?`:</span><span class="sxs-lookup"><span data-stu-id="40113-269">And if `S` is a value type, then the following intrinsic conversions exist between `T?` and `S?`:</span></span>

* <span data-ttu-id="40113-270">同じ分類 (縮小または拡大) からの変換`T?`に`S?`します。</span><span class="sxs-lookup"><span data-stu-id="40113-270">A conversion of the same classification (narrowing or widening) from `T?` to `S?`.</span></span>

* <span data-ttu-id="40113-271">同じ分類 (縮小または拡大) からの変換`T`に`S?`します。</span><span class="sxs-lookup"><span data-stu-id="40113-271">A conversion of the same classification (narrowing or widening) from `T` to `S?`.</span></span>

* <span data-ttu-id="40113-272">縮小変換から`S?`に`T`します。</span><span class="sxs-lookup"><span data-stu-id="40113-272">A narrowing conversion from `S?` to `T`.</span></span>

<span data-ttu-id="40113-273">組み込みの拡大変換が存在するなど、`Integer?`に`Long?`から組み込みの拡大変換が存在するので、`Integer`に`Long`:</span><span class="sxs-lookup"><span data-stu-id="40113-273">For example, an intrinsic widening conversion exists from `Integer?` to `Long?` because an intrinsic widening conversion exists from `Integer` to `Long`:</span></span>

```vb
Dim i As Integer? = 10
Dim l As Long? = i
```

<span data-ttu-id="40113-274">変換するときに`T?`に`S?`場合は、値`T?`は`Nothing`の値`S?`なります`Nothing`します。</span><span class="sxs-lookup"><span data-stu-id="40113-274">When converting from `T?` to `S?`, if the value of `T?` is `Nothing`, then the value of `S?` will be `Nothing`.</span></span> <span data-ttu-id="40113-275">変換するときに`S?`に`T`または`T?`に`S`場合は、値`T?`または`S?`は`Nothing`、`System.InvalidCastException`例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="40113-275">When converting from `S?` to `T` or `T?` to `S`, if the value of `T?` or `S?` is `Nothing`, a `System.InvalidCastException` exception will be thrown.</span></span>

<span data-ttu-id="40113-276">基になる型の動作のため`System.Nullable(Of T)`ときに null 許容値型、`T?`は、結果は型のボックス化された値をボックス化、 `T`、型のボックス化された値ではなく`T?`します。</span><span class="sxs-lookup"><span data-stu-id="40113-276">Because of the behavior of the underlying type `System.Nullable(Of T)`, when a nullable value type `T?` is boxed, the result is a boxed value of type `T`, not a boxed value of type `T?`.</span></span> <span data-ttu-id="40113-277">逆に、null 許容値型にボックス化解除と`T?`で、値が折り返される`System.Nullable(Of T)`、および`Nothing`型の null 値をボックス化されます`T?`します。</span><span class="sxs-lookup"><span data-stu-id="40113-277">And, conversely, when unboxing to a nullable value type `T?`, the value will be wrapped by `System.Nullable(Of T)`, and `Nothing` will be unboxed to a null value of type `T?`.</span></span> <span data-ttu-id="40113-278">例えば:</span><span class="sxs-lookup"><span data-stu-id="40113-278">For example:</span></span>

```vb
Dim i1? As Integer = Nothing
Dim o1 As Object = i1

Console.WriteLine(o1 Is Nothing)                    ' Will print True
o1 = 10
i1 = CType(o1, Integer?)
Console.WriteLine(i1)                               ' Will print 10
```

<span data-ttu-id="40113-279">この動作の副作用は、null 許容の値が入力`T?`すべてのインターフェイスを実装するために表示される`T`ボックス化される型、インターフェイスに値型に変換する必要があるため、します。</span><span class="sxs-lookup"><span data-stu-id="40113-279">A side effect of this behavior is that a nullable value type `T?` appears to implement all of the interfaces of `T`, because converting a value type to an interface requires the type to be boxed.</span></span> <span data-ttu-id="40113-280">その結果、`T?`はすべてのインターフェイスに変換する`T`に変換可能です。</span><span class="sxs-lookup"><span data-stu-id="40113-280">As a result, `T?` is convertible to all the interfaces that `T` is convertible to.</span></span> <span data-ttu-id="40113-281">ただし、null 許容の値が入力することが重要`T?`のインターフェイスを実際には実装しません`T`ジェネリック制約チェックやリフレクションのためです。</span><span class="sxs-lookup"><span data-stu-id="40113-281">It is important to note, however, that a nullable value type `T?` does not actually implement the interfaces of `T` for the purposes of generic constraint checking or reflection.</span></span> <span data-ttu-id="40113-282">例えば:</span><span class="sxs-lookup"><span data-stu-id="40113-282">For example:</span></span>

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

## <a name="string-conversions"></a><span data-ttu-id="40113-283">文字列の変換</span><span class="sxs-lookup"><span data-stu-id="40113-283">String Conversions</span></span>

<span data-ttu-id="40113-284">変換する`Char`に`String`が最初の文字は文字の値の文字列にします。</span><span class="sxs-lookup"><span data-stu-id="40113-284">Converting `Char` into `String` results in a string whose first character is the character value.</span></span> <span data-ttu-id="40113-285">変換する`String`に`Char`値が文字列の最初の文字を文字になります。</span><span class="sxs-lookup"><span data-stu-id="40113-285">Converting `String` into `Char` results in a character whose value is the first character of the string.</span></span> <span data-ttu-id="40113-286">配列に変換する`Char`に`String`文字が、配列の要素の文字列にします。</span><span class="sxs-lookup"><span data-stu-id="40113-286">Converting an array of `Char` into `String` results in a string whose characters are the elements of the array.</span></span> <span data-ttu-id="40113-287">変換する`String`の配列に`Char`要素は、文字列の文字の文字の配列になります。</span><span class="sxs-lookup"><span data-stu-id="40113-287">Converting `String` into an array of `Char` results in an array of characters whose elements are the characters of the string.</span></span>

<span data-ttu-id="40113-288">間の正確な変換`String`と`Boolean`、 `Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、 `ULong`、 `Long`、 `Decimal`、 `Single`、 `Double`、 `Date`、およびその逆の場合、この仕様の範囲を超えていて、実装は、1 つの詳細を除く依存します。</span><span class="sxs-lookup"><span data-stu-id="40113-288">The exact conversions between `String` and `Boolean`, `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`, `Date`, and vice versa, are beyond the scope of this specification and are implementation dependent with the exception of one detail.</span></span> <span data-ttu-id="40113-289">文字列の変換には、実行時環境の現在のカルチャが常に検討してください。</span><span class="sxs-lookup"><span data-stu-id="40113-289">String conversions always consider the current culture of the run-time environment.</span></span> <span data-ttu-id="40113-290">そのため、実行時に実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="40113-290">As such, they must be performed at run time.</span></span>

## <a name="widening-conversions"></a><span data-ttu-id="40113-291">拡大変換</span><span class="sxs-lookup"><span data-stu-id="40113-291">Widening Conversions</span></span>

<span data-ttu-id="40113-292">拡大変換では、オーバーフローありませんが、精度の損失を伴う場合があります。</span><span class="sxs-lookup"><span data-stu-id="40113-292">Widening conversions never overflow but may entail a loss of precision.</span></span> <span data-ttu-id="40113-293">次の変換は拡大変換。</span><span class="sxs-lookup"><span data-stu-id="40113-293">The following conversions are widening conversions:</span></span>

<span data-ttu-id="40113-294">__Identity/既定の変換__</span><span class="sxs-lookup"><span data-stu-id="40113-294">__Identity/Default conversions__</span></span>

* <span data-ttu-id="40113-295">それ自体の型。</span><span class="sxs-lookup"><span data-stu-id="40113-295">From a type to itself.</span></span>

* <span data-ttu-id="40113-296">匿名デリゲートの型から同じシグネチャを持つ任意のデリゲート型へのラムダ メソッドの再分類に対して生成されます。</span><span class="sxs-lookup"><span data-stu-id="40113-296">From an anonymous delegate type generated for a lambda method reclassification to any delegate type with an identical signature.</span></span>

* <span data-ttu-id="40113-297">リテラルから`Nothing`型にします。</span><span class="sxs-lookup"><span data-stu-id="40113-297">From the literal `Nothing` to a type.</span></span>

<span data-ttu-id="40113-298">__数値変換__</span><span class="sxs-lookup"><span data-stu-id="40113-298">__Numeric conversions__</span></span>

* <span data-ttu-id="40113-299">`Byte`に`UShort`、 `Short`、 `UInteger`、 `Integer`、 `ULong`、 `Long`、 `Decimal`、 `Single`、または`Double`します。</span><span class="sxs-lookup"><span data-stu-id="40113-299">From `Byte` to `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="40113-300">`SByte`に`Short`、 `Integer`、 `Long`、 `Decimal`、 `Single`、または`Double`します。</span><span class="sxs-lookup"><span data-stu-id="40113-300">From `SByte` to `Short`, `Integer`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="40113-301">`UShort`に`UInteger`、 `Integer`、 `ULong`、 `Long`、 `Decimal`、 `Single`、または`Double`します。</span><span class="sxs-lookup"><span data-stu-id="40113-301">From `UShort` to `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="40113-302">`Short`に`Integer`、 `Long`、 `Decimal`、`Single`または`Double`します。</span><span class="sxs-lookup"><span data-stu-id="40113-302">From `Short` to `Integer`, `Long`, `Decimal`, `Single` or `Double`.</span></span>

* <span data-ttu-id="40113-303">`UInteger`に`ULong`、 `Long`、 `Decimal`、 `Single`、または`Double`します。</span><span class="sxs-lookup"><span data-stu-id="40113-303">From `UInteger` to `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="40113-304">`Integer`に`Long`、 `Decimal`、`Single`または`Double`します。</span><span class="sxs-lookup"><span data-stu-id="40113-304">From `Integer` to `Long`, `Decimal`, `Single` or `Double`.</span></span>

* <span data-ttu-id="40113-305">`ULong`に`Decimal`、 `Single`、または`Double`します。</span><span class="sxs-lookup"><span data-stu-id="40113-305">From `ULong` to `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="40113-306">`Long`に`Decimal`、`Single`または`Double`します。</span><span class="sxs-lookup"><span data-stu-id="40113-306">From `Long` to `Decimal`, `Single` or `Double`.</span></span>

* <span data-ttu-id="40113-307">`Decimal`に`Single`または`Double`します。</span><span class="sxs-lookup"><span data-stu-id="40113-307">From `Decimal` to `Single` or `Double`.</span></span>

* <span data-ttu-id="40113-308">`Single`に`Double`します。</span><span class="sxs-lookup"><span data-stu-id="40113-308">From `Single` to `Double`.</span></span>

* <span data-ttu-id="40113-309">リテラルから`0`列挙型にします。</span><span class="sxs-lookup"><span data-stu-id="40113-309">From the literal `0` to an enumerated type.</span></span> <span data-ttu-id="40113-310">(__に注意してください。__</span><span class="sxs-lookup"><span data-stu-id="40113-310">(__Note.__</span></span> <span data-ttu-id="40113-311">変換`0`テスト フラグを簡略化するには、任意の列挙型に拡大変換します。</span><span class="sxs-lookup"><span data-stu-id="40113-311">The conversion from `0` to any enumerated type is widening to simplify testing flags.</span></span> <span data-ttu-id="40113-312">場合など、`Values`値を持つ列挙型は、 `One`、変数をテストできる`v`型の`Values`ことを示す`(v And Values.One) = 0`)。</span><span class="sxs-lookup"><span data-stu-id="40113-312">For example, if `Values` is an enumerated type with a value `One`, you could test a variable `v` of type `Values` by saying `(v And Values.One) = 0`.)</span></span>

* <span data-ttu-id="40113-313">基になる数値型または数値型、列挙型からへの拡大変換がその基になる数値を入力します。</span><span class="sxs-lookup"><span data-stu-id="40113-313">From an enumerated type to its underlying numeric type, or to a numeric type that its underlying numeric type has a widening conversion to.</span></span>

* <span data-ttu-id="40113-314">型の定数式から`ULong`、 `Long`、 `UInteger`、 `Integer`、 `UShort`、 `Short`、 `Byte`、または`SByte`より狭い型に指定の定数式の値内では、変換先の型の範囲です。</span><span class="sxs-lookup"><span data-stu-id="40113-314">From a constant expression of type `ULong`, `Long`, `UInteger`, `Integer`, `UShort`, `Short`, `Byte`, or `SByte` to a narrower type, provided the value of the constant expression is within the range of the destination type.</span></span> <span data-ttu-id="40113-315">(__に注意してください。__</span><span class="sxs-lookup"><span data-stu-id="40113-315">(__Note.__</span></span> <span data-ttu-id="40113-316">変換`UInteger`または`Integer`に`Single`、`ULong`または`Long`に`Single`または`Double`、または`Decimal`に`Single`または`Double`可能性がありますが、精度の損失を発生させることはありませんが1 桁の損失が発生します。</span><span class="sxs-lookup"><span data-stu-id="40113-316">Conversions from `UInteger` or `Integer` to `Single`, `ULong` or `Long` to `Single` or `Double`, or `Decimal` to `Single` or `Double` may cause a loss of precision, but will never cause a loss of magnitude.</span></span> <span data-ttu-id="40113-317">数値変換、拡大情報は失われません。)</span><span class="sxs-lookup"><span data-stu-id="40113-317">The other widening numeric conversions never lose any information.)</span></span>

<span data-ttu-id="40113-318">__参照変換__</span><span class="sxs-lookup"><span data-stu-id="40113-318">__Reference conversions__</span></span>

* <span data-ttu-id="40113-319">基本データ型への参照型。</span><span class="sxs-lookup"><span data-stu-id="40113-319">From a reference type to a base type.</span></span>

* <span data-ttu-id="40113-320">インターフェイス型への参照型から型が、インターフェイスまたはバリアントの互換性のあるインターフェイスを実装するを指定します。</span><span class="sxs-lookup"><span data-stu-id="40113-320">From a reference type to an interface type, provided that the type implements the interface or a variant compatible interface.</span></span>

* <span data-ttu-id="40113-321">対象のインターフェイス型から`Object`します。</span><span class="sxs-lookup"><span data-stu-id="40113-321">From an interface type to `Object`.</span></span>

* <span data-ttu-id="40113-322">バリアントの互換性のあるインターフェイス型にインターフェイスの型。</span><span class="sxs-lookup"><span data-stu-id="40113-322">From an interface type to a variant compatible interface type.</span></span>

* <span data-ttu-id="40113-323">互換性のあるバリアントにデリゲート型のデリゲートの型にします。</span><span class="sxs-lookup"><span data-stu-id="40113-323">From a delegate type to a variant compatible delegate type.</span></span> <span data-ttu-id="40113-324">(__に注意してください。__</span><span class="sxs-lookup"><span data-stu-id="40113-324">(__Note.__</span></span> <span data-ttu-id="40113-325">その他の多くの参照変換はこれらの規則によって暗黙的に指定します。</span><span class="sxs-lookup"><span data-stu-id="40113-325">Many other reference conversions are implied by these rules.</span></span> <span data-ttu-id="40113-326">たとえば、匿名のデリゲートは、参照型から継承する`System.MulticastDelegate`; 配列型は参照型から継承する`System.Array`; 匿名型は参照型から継承する`System.Object`)。</span><span class="sxs-lookup"><span data-stu-id="40113-326">For example, anonymous delegates are reference types that inherit from `System.MulticastDelegate`; array types are reference types that inherit from `System.Array`; anonymous types are reference types that inherit from `System.Object`.)</span></span>

<span data-ttu-id="40113-327">__匿名デリゲートの変換__</span><span class="sxs-lookup"><span data-stu-id="40113-327">__Anonymous Delegate conversions__</span></span>

* <span data-ttu-id="40113-328">匿名からより多くのデリゲート型にラムダ メソッドの再分類に対して生成された型を委任します。</span><span class="sxs-lookup"><span data-stu-id="40113-328">From an anonymous delegate type generated for a lambda method reclassification to any wider delegate type.</span></span>

<span data-ttu-id="40113-329">__配列変換__</span><span class="sxs-lookup"><span data-stu-id="40113-329">__Array conversions__</span></span>

* <span data-ttu-id="40113-330">配列型から`S`要素型が`Se`が配列型に`T`要素型が`Te`true は、次のすべての提供。</span><span class="sxs-lookup"><span data-stu-id="40113-330">From an array type `S` with an element type `Se` to an array type `T` with an element type `Te`, provided all of the following are true:</span></span>

  * <span data-ttu-id="40113-331">`S` `T`要素の型のみが異なります。</span><span class="sxs-lookup"><span data-stu-id="40113-331">`S` and `T` differ only in element type.</span></span>

  * <span data-ttu-id="40113-332">両方`Se`と`Te`は参照型に既知の型パラメーターまたは参照型。</span><span class="sxs-lookup"><span data-stu-id="40113-332">Both `Se` and `Te` are reference types or are type parameters known to be a reference type.</span></span>

  * <span data-ttu-id="40113-333">拡大の参照、配列、または型からパラメーターの変換が存在する`Se`に`Te`します。</span><span class="sxs-lookup"><span data-stu-id="40113-333">A widening reference, array, or type parameter conversion exists from `Se` to `Te`.</span></span>

* <span data-ttu-id="40113-334">配列型から`S`列挙された要素型が`Se`が配列型に`T`要素型が`Te`true は、次のすべての提供。</span><span class="sxs-lookup"><span data-stu-id="40113-334">From an array type `S` with an enumerated element type `Se` to an array type `T` with an element type `Te`, provided all of the following are true:</span></span>

  * <span data-ttu-id="40113-335">`S` `T`要素の型のみが異なります。</span><span class="sxs-lookup"><span data-stu-id="40113-335">`S` and `T` differ only in element type.</span></span>

  * <span data-ttu-id="40113-336">`Te` 基になる型は、`Se`します。</span><span class="sxs-lookup"><span data-stu-id="40113-336">`Te` is the underlying type of `Se`.</span></span>

* <span data-ttu-id="40113-337">配列型から`S`列挙された要素型のランク 1 の`Se`を`System.Collections.Generic.IList(Of Te)`、 `IReadOnlyList(Of Te)`、 `ICollection(Of Te)`、 `IReadOnlyCollection(Of Te)`、および`IEnumerable(Of Te)`、指定された、次のいずれかが true:</span><span class="sxs-lookup"><span data-stu-id="40113-337">From an array type `S` of rank 1 with an enumerated element type `Se`, to `System.Collections.Generic.IList(Of Te)`, `IReadOnlyList(Of Te)`, `ICollection(Of Te)`, `IReadOnlyCollection(Of Te)`, and `IEnumerable(Of Te)`, provided one of the following is true:</span></span>

  * <span data-ttu-id="40113-338">両方`Se`と`Te`は参照型または型パラメーターは参照である型、および拡大の参照が配列、またはから型パラメーター変換が存在する既知の`Se`に`Te`; または</span><span class="sxs-lookup"><span data-stu-id="40113-338">Both `Se` and `Te` are reference types or are type parameters known to be a reference type, and a widening reference, array, or type parameter conversion exists from `Se` to `Te`; or</span></span>

  * <span data-ttu-id="40113-339">`Te` 基になる型は、 `Se`; または</span><span class="sxs-lookup"><span data-stu-id="40113-339">`Te` is the underlying type of `Se`; or</span></span>

  * <span data-ttu-id="40113-340">`Te` は `Se` と同じ</span><span class="sxs-lookup"><span data-stu-id="40113-340">`Te` is identical to `Se`</span></span>

<span data-ttu-id="40113-341">__値型の変換__</span><span class="sxs-lookup"><span data-stu-id="40113-341">__Value Type conversions__</span></span>

* <span data-ttu-id="40113-342">値の型を基本データ型。</span><span class="sxs-lookup"><span data-stu-id="40113-342">From a value type to a base type.</span></span>

* <span data-ttu-id="40113-343">型が実装するインターフェイス型に値型。</span><span class="sxs-lookup"><span data-stu-id="40113-343">From a value type to an interface type that the type implements.</span></span>

<span data-ttu-id="40113-344">__Null 許容の値の型変換__</span><span class="sxs-lookup"><span data-stu-id="40113-344">__Nullable Value Type conversions__</span></span>

* <span data-ttu-id="40113-345">型から`T`型に`T?`します。</span><span class="sxs-lookup"><span data-stu-id="40113-345">From a type `T` to the type `T?`.</span></span>

* <span data-ttu-id="40113-346">型から`T?`型`S?`型から拡大変換がある場合、`T`型に`S`します。</span><span class="sxs-lookup"><span data-stu-id="40113-346">From a type `T?` to a type `S?`, where there is a widening conversion from the type `T` to the type `S`.</span></span>

* <span data-ttu-id="40113-347">型から`T`型`S?`型から拡大変換がある場合、`T`型に`S`します。</span><span class="sxs-lookup"><span data-stu-id="40113-347">From a type `T` to a type `S?`, where there is a widening conversion from the type `T` to the type `S`.</span></span>

* <span data-ttu-id="40113-348">型から`T?`インターフェイス型を型`T`を実装します。</span><span class="sxs-lookup"><span data-stu-id="40113-348">From a type `T?` to an interface type that the type `T` implements.</span></span>

<span data-ttu-id="40113-349">__文字列の変換__</span><span class="sxs-lookup"><span data-stu-id="40113-349">__String conversions__</span></span>

* <span data-ttu-id="40113-350">`Char`に`String`します。</span><span class="sxs-lookup"><span data-stu-id="40113-350">From `Char` to `String`.</span></span>

* <span data-ttu-id="40113-351">`Char()`に`String`します。</span><span class="sxs-lookup"><span data-stu-id="40113-351">From `Char()` to `String`.</span></span>

<span data-ttu-id="40113-352">__型パラメーター変換__</span><span class="sxs-lookup"><span data-stu-id="40113-352">__Type Parameter conversions__</span></span>

* <span data-ttu-id="40113-353">型パラメーターから`Object`します。</span><span class="sxs-lookup"><span data-stu-id="40113-353">From a type parameter to `Object`.</span></span>

* <span data-ttu-id="40113-354">インターフェイス型制約またはインターフェイス型の制約と互換性のあるインターフェイス バリアント型パラメーター。</span><span class="sxs-lookup"><span data-stu-id="40113-354">From a type parameter to an interface type constraint or any interface variant compatible with an interface type constraint.</span></span>

* <span data-ttu-id="40113-355">クラスの制約によって実装されるインターフェイスの型パラメーター。</span><span class="sxs-lookup"><span data-stu-id="40113-355">From a type parameter to an interface implemented by a class constraint.</span></span>

* <span data-ttu-id="40113-356">クラスの制約によって実装されるインターフェイスと互換性のあるインターフェイス バリアント型を型パラメーター。</span><span class="sxs-lookup"><span data-stu-id="40113-356">From a type parameter to an interface variant compatible with an interface implemented by a class constraint.</span></span>

* <span data-ttu-id="40113-357">クラスの制約、または、クラス制約の基本型の型パラメーター。</span><span class="sxs-lookup"><span data-stu-id="40113-357">From a type parameter to a class constraint, or a base type of the class constraint.</span></span>

* <span data-ttu-id="40113-358">型パラメーターから`T`型パラメーター制約に`Tx`、または何も`Tx`に拡大変換します。</span><span class="sxs-lookup"><span data-stu-id="40113-358">From a type parameter `T` to a type parameter constraint `Tx`, or anything `Tx` has a widening conversion to.</span></span>

## <a name="narrowing-conversions"></a><span data-ttu-id="40113-359">縮小変換</span><span class="sxs-lookup"><span data-stu-id="40113-359">Narrowing Conversions</span></span>

<span data-ttu-id="40113-360">縮小変換では、常に成功する変換、情報が失われる可能性があることがわかっている変換および変換を縮小表記するだけの価値を十分にさまざまな種類のドメイン間で。</span><span class="sxs-lookup"><span data-stu-id="40113-360">Narrowing conversions are conversions that cannot be proved to always succeed, conversions that are known to possibly lose information, and conversions across domains of types sufficiently different to merit narrowing notation.</span></span> <span data-ttu-id="40113-361">次の変換は縮小変換として分類されます。</span><span class="sxs-lookup"><span data-stu-id="40113-361">The following conversions are classified as narrowing conversions:</span></span>

<span data-ttu-id="40113-362">__ブール型変換__</span><span class="sxs-lookup"><span data-stu-id="40113-362">__Boolean conversions__</span></span>

* <span data-ttu-id="40113-363">`Boolean`に`Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、 `ULong`、 `Long`、 `Decimal`、 `Single`、または`Double`します。</span><span class="sxs-lookup"><span data-stu-id="40113-363">From `Boolean` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="40113-364">`Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、 `ULong`、 `Long`、 `Decimal`、 `Single`、または`Double`に`Boolean`します。</span><span class="sxs-lookup"><span data-stu-id="40113-364">From `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double` to `Boolean`.</span></span>

<span data-ttu-id="40113-365">__数値変換__</span><span class="sxs-lookup"><span data-stu-id="40113-365">__Numeric conversions__</span></span>

* <span data-ttu-id="40113-366">`Byte`に`SByte`します。</span><span class="sxs-lookup"><span data-stu-id="40113-366">From `Byte` to `SByte`.</span></span>

* <span data-ttu-id="40113-367">`SByte`に`Byte`、 `UShort`、 `UInteger`、または`ULong`します。</span><span class="sxs-lookup"><span data-stu-id="40113-367">From `SByte` to `Byte`, `UShort`, `UInteger`, or `ULong`.</span></span>

* <span data-ttu-id="40113-368">`UShort`に`Byte`、 `SByte`、または`Short`します。</span><span class="sxs-lookup"><span data-stu-id="40113-368">From `UShort` to `Byte`, `SByte`, or `Short`.</span></span>

* <span data-ttu-id="40113-369">`Short`に`Byte`、 `SByte`、 `UShort`、 `UInteger`、または`ULong`します。</span><span class="sxs-lookup"><span data-stu-id="40113-369">From `Short` to `Byte`, `SByte`, `UShort`, `UInteger`, or `ULong`.</span></span>

* <span data-ttu-id="40113-370">`UInteger`に`Byte`、 `SByte`、 `UShort`、 `Short`、または`Integer`します。</span><span class="sxs-lookup"><span data-stu-id="40113-370">From `UInteger` to `Byte`, `SByte`, `UShort`, `Short`, or `Integer`.</span></span>

* <span data-ttu-id="40113-371">`Integer`に`Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、または`ULong`します。</span><span class="sxs-lookup"><span data-stu-id="40113-371">From `Integer` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, or `ULong`.</span></span>

* <span data-ttu-id="40113-372">`ULong`に`Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、または`Long`します。</span><span class="sxs-lookup"><span data-stu-id="40113-372">From `ULong` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, or `Long`.</span></span>

* <span data-ttu-id="40113-373">`Long`に`Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、または`ULong`します。</span><span class="sxs-lookup"><span data-stu-id="40113-373">From `Long` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, or `ULong`.</span></span>

* <span data-ttu-id="40113-374">`Decimal`に`Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、 `ULong`、または`Long`します。</span><span class="sxs-lookup"><span data-stu-id="40113-374">From `Decimal` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, or `Long`.</span></span>

* <span data-ttu-id="40113-375">`Single`に`Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、 `ULong`、 `Long`、または`Decimal`します。</span><span class="sxs-lookup"><span data-stu-id="40113-375">From `Single` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, or `Decimal`.</span></span>

* <span data-ttu-id="40113-376">`Double`に`Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、 `ULong`、 `Long`、 `Decimal`、または`Single`します。</span><span class="sxs-lookup"><span data-stu-id="40113-376">From `Double` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, or `Single`.</span></span>

* <span data-ttu-id="40113-377">列挙型の数値型から</span><span class="sxs-lookup"><span data-stu-id="40113-377">From a numeric type to an enumerated type.</span></span>

* <span data-ttu-id="40113-378">数値型の列挙型から基になる数値型への縮小変換が。</span><span class="sxs-lookup"><span data-stu-id="40113-378">From an enumerated type to a numeric type its underlying numeric type has a narrowing conversion to.</span></span>

* <span data-ttu-id="40113-379">別の列挙型の列挙型。</span><span class="sxs-lookup"><span data-stu-id="40113-379">From an enumerated type to another enumerated type.</span></span>

<span data-ttu-id="40113-380">__参照変換__</span><span class="sxs-lookup"><span data-stu-id="40113-380">__Reference conversions__</span></span>

* <span data-ttu-id="40113-381">より強い派生型への参照型。</span><span class="sxs-lookup"><span data-stu-id="40113-381">From a reference type to a more derived type.</span></span>

* <span data-ttu-id="40113-382">インターフェイス型に、クラス型からクラス型がインターフェイス型またはそれと互換性のあるインターフェイス型バリアントを実装しませんを提供します。</span><span class="sxs-lookup"><span data-stu-id="40113-382">From a class type to an interface type, provided the class type does not implement the interface type or an interface type variant compatible with it.</span></span>

* <span data-ttu-id="40113-383">クラス型へのインターフェイス型。</span><span class="sxs-lookup"><span data-stu-id="40113-383">From an interface type to a class type.</span></span>

* <span data-ttu-id="40113-384">インターフェイスからされている別のインターフェイス型には 2 つの型の間の継承関係がないと、variant と互換性のあるないを指定します。</span><span class="sxs-lookup"><span data-stu-id="40113-384">From an interface type to another interface type, provided there is no inheritance relationship between the two types and provided they are not variant compatible.</span></span>

<span data-ttu-id="40113-385">__匿名デリゲートの変換__</span><span class="sxs-lookup"><span data-stu-id="40113-385">__Anonymous Delegate conversions__</span></span>

* <span data-ttu-id="40113-386">より狭いデリゲート型へのラムダ メソッドの再分類に対して生成される匿名デリゲートの型。</span><span class="sxs-lookup"><span data-stu-id="40113-386">From an anonymous delegate type generated for a lambda method reclassification to any narrower delegate type.</span></span>

<span data-ttu-id="40113-387">__配列変換__</span><span class="sxs-lookup"><span data-stu-id="40113-387">__Array conversions__</span></span>

* <span data-ttu-id="40113-388">配列型から`S`要素型が`Se`、配列型に`T`要素型が`Te`、その次のすべてに該当します。</span><span class="sxs-lookup"><span data-stu-id="40113-388">From an array type `S` with an element type `Se`, to an array type `T` with an element type `Te`, provided that all of the following are true:</span></span>

  * <span data-ttu-id="40113-389">`S` `T`要素の型のみが異なります。</span><span class="sxs-lookup"><span data-stu-id="40113-389">`S` and `T` differ only in element type.</span></span>
  * <span data-ttu-id="40113-390">両方`Se`と`Te`は参照型または型パラメーターに認識されていない値型であります。</span><span class="sxs-lookup"><span data-stu-id="40113-390">Both `Se` and `Te` are reference types or are type parameters not known to be value types.</span></span>
  * <span data-ttu-id="40113-391">縮小参照、配列、または型パラメーター変換が存在する`Se`に`Te`します。</span><span class="sxs-lookup"><span data-stu-id="40113-391">A narrowing reference, array, or type parameter conversion exists from `Se` to `Te`.</span></span>

* <span data-ttu-id="40113-392">配列型から`S`要素型が`Se`が配列型に`T`列挙された要素型が`Te`true は、次のすべての提供。</span><span class="sxs-lookup"><span data-stu-id="40113-392">From an array type `S` with an element type `Se` to an array type `T` with an enumerated element type `Te`, provided all of the following are true:</span></span>

  * <span data-ttu-id="40113-393">`S` `T`要素の型のみが異なります。</span><span class="sxs-lookup"><span data-stu-id="40113-393">`S` and `T` differ only in element type.</span></span>
  * <span data-ttu-id="40113-394">`Se` 基になる型は、 `Te` 、か、同じ基になる型を共有する両方のさまざまな列挙型。</span><span class="sxs-lookup"><span data-stu-id="40113-394">`Se` is the underlying type of `Te` , or they are both different enumerated types that share the same underlying type.</span></span>

* <span data-ttu-id="40113-395">配列型から`S`列挙された要素型のランク 1 の`Se`を`IList(Of Te)`、 `IReadOnlyList(Of Te)`、 `ICollection(Of Te)`、`IReadOnlyCollection(Of Te)`と`IEnumerable(Of Te)`、指定された、次のいずれかが true:</span><span class="sxs-lookup"><span data-stu-id="40113-395">From an array type `S` of rank 1 with an enumerated element type `Se`, to `IList(Of Te)`, `IReadOnlyList(Of Te)`, `ICollection(Of Te)`, `IReadOnlyCollection(Of Te)` and `IEnumerable(Of Te)`, provided one of the following is true:</span></span>

  * <span data-ttu-id="40113-396">両方`Se`と`Te`は参照型やは参照型では、既知の型パラメーターから縮小参照、配列、または型パラメーター変換が存在する`Se`に`Te`; または</span><span class="sxs-lookup"><span data-stu-id="40113-396">Both `Se` and `Te` are reference types or are type parameters known to be a reference type, and a narrowing reference, array, or type parameter conversion exists from `Se` to `Te`; or</span></span>
  * <span data-ttu-id="40113-397">`Se` 基になる型は、 `Te`、か、同じ基になる型を共有する両方のさまざまな列挙型。</span><span class="sxs-lookup"><span data-stu-id="40113-397">`Se` is the underlying type of `Te`, or they are both different enumerated types that share the same underlying type.</span></span>

<span data-ttu-id="40113-398">__値型の変換__</span><span class="sxs-lookup"><span data-stu-id="40113-398">__Value type conversions__</span></span>

* <span data-ttu-id="40113-399">派生値型への参照型。</span><span class="sxs-lookup"><span data-stu-id="40113-399">From a reference type to a more derived value type.</span></span>

* <span data-ttu-id="40113-400">インターフェイスの型を値型から値の型がインターフェイス型を実装を提供します。</span><span class="sxs-lookup"><span data-stu-id="40113-400">From an interface type to a value type, provided the value type implements the interface type.</span></span>

<span data-ttu-id="40113-401">__Null 許容の値の型変換__</span><span class="sxs-lookup"><span data-stu-id="40113-401">__Nullable Value Type conversions__</span></span>

* <span data-ttu-id="40113-402">型から`T?`型`T`します。</span><span class="sxs-lookup"><span data-stu-id="40113-402">From a type `T?` to a type `T`.</span></span>

* <span data-ttu-id="40113-403">型から`T?`型`S?`型から縮小変換がある場合、`T`型に`S`します。</span><span class="sxs-lookup"><span data-stu-id="40113-403">From a type `T?` to a type `S?`, where there is a narrowing conversion from the type `T` to the type `S`.</span></span>

* <span data-ttu-id="40113-404">型から`T`型`S?`型から縮小変換がある場合、`T`型に`S`します。</span><span class="sxs-lookup"><span data-stu-id="40113-404">From a type `T` to a type `S?`, where there is a narrowing conversion from the type `T` to the type `S`.</span></span>

* <span data-ttu-id="40113-405">型から`S?`型`T`型からの変換がある場合、`S`型に`T`します。</span><span class="sxs-lookup"><span data-stu-id="40113-405">From a type `S?` to a type `T`, where there is a conversion from the type `S` to the type `T`.</span></span>

<span data-ttu-id="40113-406">__文字列の変換__</span><span class="sxs-lookup"><span data-stu-id="40113-406">__String conversions__</span></span>

* <span data-ttu-id="40113-407">`String`に`Char`します。</span><span class="sxs-lookup"><span data-stu-id="40113-407">From `String` to `Char`.</span></span>

* <span data-ttu-id="40113-408">`String`に`Char()`します。</span><span class="sxs-lookup"><span data-stu-id="40113-408">From `String` to `Char()`.</span></span>

* <span data-ttu-id="40113-409">`String`に`Boolean`との間`Boolean`に`String`します。</span><span class="sxs-lookup"><span data-stu-id="40113-409">From `String` to `Boolean` and from `Boolean` to `String`.</span></span>

* <span data-ttu-id="40113-410">間の変換`String`と`Byte`、 `SByte`、 `UShort`、 `Short`、 `UInteger`、 `Integer`、 `ULong`、 `Long`、 `Decimal`、 `Single`、または`Double`します。</span><span class="sxs-lookup"><span data-stu-id="40113-410">Conversions between `String` and `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="40113-411">`String`に`Date`との間`Date`に`String`します。</span><span class="sxs-lookup"><span data-stu-id="40113-411">From `String` to `Date` and from `Date` to `String`.</span></span>

<span data-ttu-id="40113-412">__型パラメーター変換__</span><span class="sxs-lookup"><span data-stu-id="40113-412">__Type Parameter conversions__</span></span>

* <span data-ttu-id="40113-413">`Object`型パラメーターにします。</span><span class="sxs-lookup"><span data-stu-id="40113-413">From `Object` to a type parameter.</span></span>

* <span data-ttu-id="40113-414">型から、インターフェイス型、型パラメーターを指定するパラメーターがないそのインターフェイスに制約付きまたはそのインターフェイスを実装するクラスに制限は。</span><span class="sxs-lookup"><span data-stu-id="40113-414">From a type parameter to an interface type, provided the type parameter is not constrained to that interface or constrained to a class that implements that interface.</span></span>

* <span data-ttu-id="40113-415">型パラメーターをインターフェイスの型。</span><span class="sxs-lookup"><span data-stu-id="40113-415">From an interface type to a type parameter.</span></span>

* <span data-ttu-id="40113-416">クラス制約の派生型に型パラメーター。</span><span class="sxs-lookup"><span data-stu-id="40113-416">From a type parameter to a derived type of a class constraint.</span></span>

* <span data-ttu-id="40113-417">型パラメーターから`T`型パラメーターの制約に`Tx`に縮小変換します。</span><span class="sxs-lookup"><span data-stu-id="40113-417">From a type parameter `T` to anything a type parameter constraint `Tx` has a narrowing conversion to.</span></span>

## <a name="type-parameter-conversions"></a><span data-ttu-id="40113-418">型パラメーター変換</span><span class="sxs-lookup"><span data-stu-id="40113-418">Type Parameter Conversions</span></span>

<span data-ttu-id="40113-419">型パラメーターの変換は、配置、いずれかである場合、制約によって決定されます。</span><span class="sxs-lookup"><span data-stu-id="40113-419">Type parameters' conversions are determined by the constraints, if any, put on them.</span></span> <span data-ttu-id="40113-420">型パラメーター`T`と常に、それ自体に変換できる`Object`、およびとの間の任意のインターフェイス型。</span><span class="sxs-lookup"><span data-stu-id="40113-420">A type parameter `T` can always be converted to itself, to and from `Object`, and to and from any interface type.</span></span> <span data-ttu-id="40113-421">型をされた場合に注意してください`T`ランタイムからの変換に値型では、`T`に`Object`インターフェイス型はボックス化変換および変換されます`Object`またはインターフェイス型`T`なボックス化解除されます変換を行います。</span><span class="sxs-lookup"><span data-stu-id="40113-421">Note that if the type `T` is a value type at run-time, converting from `T` to `Object` or an interface type will be a boxing conversion and converting from `Object` or an interface type to `T` will be an unboxing conversion.</span></span> <span data-ttu-id="40113-422">クラスの制約を指定された型パラメーター`C`には、型パラメーターの追加の変換を定義します`C`とその基本クラスでは、その逆です。</span><span class="sxs-lookup"><span data-stu-id="40113-422">A type parameter with a class constraint `C` defines additional conversions from the type parameter to `C` and its base classes, and vice versa.</span></span> <span data-ttu-id="40113-423">型パラメーター`T`型パラメーター制約`Tx`への変換を定義します。`Tx`および`Tx`に変換します。</span><span class="sxs-lookup"><span data-stu-id="40113-423">A type parameter `T` with a type parameter constraint `Tx` defines a conversion to `Tx` and anything `Tx` converts to.</span></span>

<span data-ttu-id="40113-424">配列要素型は、型パラメーターはインターフェイスの制約で`I`が配列要素型が同じ配列の共変変換`I`、その型パラメーターがあります、`Class`クラスの制約 (または参照の配列要素の型のみがあるため共変)。</span><span class="sxs-lookup"><span data-stu-id="40113-424">An array whose element type is a type parameter with an interface constraint `I` has the same covariant array conversions as an array whose element type is `I`, provided that the type parameter also has a `Class` or class constraint (since only reference array element types can be covariant).</span></span> <span data-ttu-id="40113-425">配列要素型は、型パラメーター、クラスの制約で`C`が配列要素型が同じ配列の共変変換`C`します。</span><span class="sxs-lookup"><span data-stu-id="40113-425">An array whose element type is a type parameter with a class constraint `C` has the same covariant array conversions as an array whose element type is `C`.</span></span>

<span data-ttu-id="40113-426">上記の変換規則は制約のない型パラメーターからインターフェイス以外の型への変換許可されていません驚くべきことになる可能性もあります。</span><span class="sxs-lookup"><span data-stu-id="40113-426">The above conversions rules do not permit conversions from unconstrained type parameters to non-interface types, which may be surprising.</span></span> <span data-ttu-id="40113-427">この理由は、このような変換のセマンティクスについて混乱を避けるためです。</span><span class="sxs-lookup"><span data-stu-id="40113-427">The reason for this is to prevent confusion about the semantics of such conversions.</span></span> <span data-ttu-id="40113-428">たとえば、次のような宣言があるとします。</span><span class="sxs-lookup"><span data-stu-id="40113-428">For example, consider the following declaration:</span></span>

```vb
Class X(Of T)
    Public Shared Function F(t As T) As Long 
        Return CLng(t)    ' Error, explicit conversion not permitted
    End Function
End Class
```

<span data-ttu-id="40113-429">場合の変換`T`に`Integer`簡単に期待される、許可されたを`X(Of Integer).F(7)`は返します`7L`。</span><span class="sxs-lookup"><span data-stu-id="40113-429">If the conversion of `T` to `Integer` were permitted, one might easily expect that `X(Of Integer).F(7)` would return `7L`.</span></span> <span data-ttu-id="40113-430">ただし、設定されませんでしたが、数値変換には、コンパイル時に数値型がわかっている場合にのみと見なされるためです。</span><span class="sxs-lookup"><span data-stu-id="40113-430">However, it would not, because numeric conversions are only considered when the types are known to be numeric at compile time.</span></span> <span data-ttu-id="40113-431">セマンティクスを作成するには明確で上記の例では、代わりに記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="40113-431">In order to make the semantics clear, the above example must instead be written:</span></span>

```vb
Class X(Of T)
    Public Shared Function F(t As T) As Long
        Return CLng(CObj(t))    ' OK, conversions permitted
    End Function
End Class
```

## <a name="user-defined-conversions"></a><span data-ttu-id="40113-432">ユーザー定義変換</span><span class="sxs-lookup"><span data-stu-id="40113-432">User-Defined Conversions</span></span>

<span data-ttu-id="40113-433">*組み込み変換*変換中に (つまり表示されているこの仕様で)、言語によって定義されている*ユーザー定義の変換*オーバー ロードによって定義されます、`CType`演算子。</span><span class="sxs-lookup"><span data-stu-id="40113-433">*Intrinsic conversions* are conversions defined by the language (i.e. listed in this specification), while *user-defined conversions* are defined by overloading the `CType` operator.</span></span> <span data-ttu-id="40113-434">適用可能な組み込み変換がない場合は、型の間で変換するときに、ユーザー定義の変換が考慮されます。</span><span class="sxs-lookup"><span data-stu-id="40113-434">When converting between types, if no intrinsic conversions are applicable then user-defined conversions will be considered.</span></span> <span data-ttu-id="40113-435">ユーザー定義の変換がある場合*最も具体的*ソースとターゲットの種類では、そのユーザー定義の変換が使用されます。</span><span class="sxs-lookup"><span data-stu-id="40113-435">If there is a user-defined conversion that is *most specific* for the source and target types, then the user-defined conversion will be used.</span></span> <span data-ttu-id="40113-436">それ以外の場合、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="40113-436">Otherwise, a compile-time error results.</span></span> <span data-ttu-id="40113-437">最も固有の変換がオペランドがソースの種類に「最も近い」には、結果の型がターゲットの型に「最も近い」です。</span><span class="sxs-lookup"><span data-stu-id="40113-437">The most specific conversion is the one whose operand is "closest" to the source type and whose result type is "closest" to the target type.</span></span> <span data-ttu-id="40113-438">使用するどのようなユーザー定義の変換を決定するには、最も固有の拡大変換が使用されます。拡大しない場合は、変換は最も具体的に最も固有の縮小変換が使用されます。</span><span class="sxs-lookup"><span data-stu-id="40113-438">When determining what user-defined conversion to use, the most specific widening conversion will be used; if no widening conversion is most specific, the most specific narrowing conversion will be used.</span></span> <span data-ttu-id="40113-439">最も関連の縮小変換は、変換が定義されているし、し、コンパイル時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="40113-439">If there is no most specific narrowing conversion, then the conversion is undefined and a compile-time error occurs.</span></span>

<span data-ttu-id="40113-440">次のセクションでは、最も固有の変換を決定する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="40113-440">The following sections cover how the most specific conversions are determined.</span></span> <span data-ttu-id="40113-441">次の用語を使用します。</span><span class="sxs-lookup"><span data-stu-id="40113-441">They use the following terms:</span></span>

<span data-ttu-id="40113-442">型から変換が存在する場合は、組み込みの拡大`A`型`B`、どちらの場合と`A`も`B`は、インターフェイス、`A`は*包含*によって`B`、`B` *包含*`A`します。</span><span class="sxs-lookup"><span data-stu-id="40113-442">If an intrinsic widening conversion exists from a type `A` to a type `B`, and if neither `A` nor `B` are interfaces, then `A` is *encompassed* by `B`, and `B` *encompasses* `A`.</span></span>

<span data-ttu-id="40113-443">*最も外側にある*型のセット内の型は、セット内の他のすべての型を包含している 1 つの型。</span><span class="sxs-lookup"><span data-stu-id="40113-443">The *most encompassing* type in a set of types is the one type that encompasses all other types in the set.</span></span> <span data-ttu-id="40113-444">その他のすべての型を包含する 1 つの型の場合セットに最も外側の型がありません。</span><span class="sxs-lookup"><span data-stu-id="40113-444">If no single type encompasses all other types, then the set has no most encompassing type.</span></span> <span data-ttu-id="40113-445">直感的な用語では、最も外側の型は、set - の他の種類ごとに拡大変換を介して変換できる 1 つの型の「最大」の型です。</span><span class="sxs-lookup"><span data-stu-id="40113-445">In intuitive terms, the most encompassing type is the "largest" type in the set -- the one type to which each of the other types can be converted through a widening conversion.</span></span>

<span data-ttu-id="40113-446">*最も内側の*一連の型の型が、セット内の他のすべての種類に包含されている 1 つの型。</span><span class="sxs-lookup"><span data-stu-id="40113-446">The *most encompassed* type in a set of types is the one type that is encompassed by all other types in the set.</span></span> <span data-ttu-id="40113-447">他のすべての型によって 1 つの型が含まれていない場合、セットが最も内側の型。</span><span class="sxs-lookup"><span data-stu-id="40113-447">If no single type is encompassed by all other types, then the set has no most encompassed type.</span></span> <span data-ttu-id="40113-448">直感的な用語では、最も内側の型は、set - の縮小変換を他の種類ごとに変換できる 1 つの型の「最小」の型です。</span><span class="sxs-lookup"><span data-stu-id="40113-448">In intuitive terms, the most encompassed type is the "smallest" type in the set -- the one type that can be converted to each of the other types through a narrowing conversion.</span></span>

<span data-ttu-id="40113-449">型の候補ユーザー定義の変換を収集するときに`T?`で定義されたユーザー定義の変換演算子`T`代わりに使用されます。</span><span class="sxs-lookup"><span data-stu-id="40113-449">When collecting the candidate user-defined conversions for a type `T?`, the user-defined conversion operators defined by `T` are used instead.</span></span> <span data-ttu-id="40113-450">変換される型が null 許容値型では、次のいずれかでも場合`T`のユーザー定義変換演算子を伴う null 非許容値型のみが無効になります。</span><span class="sxs-lookup"><span data-stu-id="40113-450">If the type being converted to is also a nullable value type, then any of `T`'s user-defined conversions operators that involve only non-nullable value types are lifted.</span></span> <span data-ttu-id="40113-451">変換演算子`T`に`S`からの変換にリフトされた`T?`に`S?`変換によって評価されると`T?`に`T`必要に応じて、ユーザー定義の変換を評価し、演算子を`T`に`S`変換してみます`S`に`S?`のために必要な場合は、します。</span><span class="sxs-lookup"><span data-stu-id="40113-451">A conversion operator from `T` to `S` is lifted to be a conversion from `T?` to `S?` and is evaluated by converting `T?` to `T`, if necessary, then evaluating the user-defined conversion operator from `T` to `S` and then converting `S` to `S?`, if necessary.</span></span> <span data-ttu-id="40113-452">変換される値が場合`Nothing`、リフトの変換演算子がの値に直接変換するただし、`Nothing`として型指定された`S?`します。</span><span class="sxs-lookup"><span data-stu-id="40113-452">If the value being converted is `Nothing`, however, a lifted conversion operator converts directly into a value of `Nothing` typed as `S?`.</span></span> <span data-ttu-id="40113-453">例えば:</span><span class="sxs-lookup"><span data-stu-id="40113-453">For example:</span></span>

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

<span data-ttu-id="40113-454">変換では、解決するユーザー定義されたときに変換演算子はリフト変換演算子より優先常にします。</span><span class="sxs-lookup"><span data-stu-id="40113-454">When resolving conversions, user-defined conversions operators are always preferred over lifted conversion operators.</span></span> <span data-ttu-id="40113-455">例えば:</span><span class="sxs-lookup"><span data-stu-id="40113-455">For example:</span></span>

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

<span data-ttu-id="40113-456">実行時に、ユーザー定義の変換の評価に最大 3 つの手順を含むことができます。</span><span class="sxs-lookup"><span data-stu-id="40113-456">At run-time, evaluating a user-defined conversion can involve up to three steps:</span></span>

1. <span data-ttu-id="40113-457">最初に、値は、必要に応じて、組み込みの変換を使用して、オペランドの型に元の型から変換されます。</span><span class="sxs-lookup"><span data-stu-id="40113-457">First, the value is converted from the source type to the operand type using an intrinsic conversion, if necessary.</span></span>

2. <span data-ttu-id="40113-458">次に、ユーザー定義の変換が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="40113-458">Then, the user-defined conversion is invoked.</span></span>

3. <span data-ttu-id="40113-459">最後に、ユーザー定義の変換の結果は、必要に応じて、組み込みの変換を使用して、ターゲット型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="40113-459">Finally, the result of the user-defined conversion is converted to the target type using an intrinsic conversion, if necessary.</span></span>

<span data-ttu-id="40113-460">ユーザー定義の変換の評価は 1 つ以上のユーザー定義変換演算子が含まれることはないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="40113-460">It is important to note that evaluation of a user-defined conversion will never involve more than one user-defined conversion operator.</span></span>

### <a name="most-specific-widening-conversion"></a><span data-ttu-id="40113-461">最も固有の拡大変換</span><span class="sxs-lookup"><span data-stu-id="40113-461">Most Specific Widening Conversion</span></span>

<span data-ttu-id="40113-462">最も固有ユーザー定義拡大変換演算子の 2 種類の間を決定するには、次の手順を使用して実現されます。</span><span class="sxs-lookup"><span data-stu-id="40113-462">Determining the most specific user-defined widening conversion operator between two types is accomplished using the following steps:</span></span>

1. <span data-ttu-id="40113-463">最初に、すべての候補の変換演算子が収集されます。</span><span class="sxs-lookup"><span data-stu-id="40113-463">First, all of the candidate conversion operators are collected.</span></span> <span data-ttu-id="40113-464">変換演算子の候補は、ソースの種類のユーザー定義の拡大変換演算子のすべてとすべてのターゲットの型でユーザー定義の拡大変換演算子。</span><span class="sxs-lookup"><span data-stu-id="40113-464">The candidate conversion operators are all of the user-defined widening conversion operators in the source type and all of the user-defined widening conversion operators in the target type.</span></span>

2. <span data-ttu-id="40113-465">次に、すべての非該当変換演算子は、セットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="40113-465">Then, all non-applicable conversion operators are removed from the set.</span></span> <span data-ttu-id="40113-466">変換演算子は、演算子があります。 組み込み拡大変換元の型からオペランドの型へと、組み込み拡大への変換演算子、演算子の結果から対象の型がある場合、ソースの種類とターゲットの種類に適用されます。</span><span class="sxs-lookup"><span data-stu-id="40113-466">A conversion operator is applicable to a source type and target type if there is an intrinsic widening conversion operator from the source type to the operand type and there is an intrinsic widening conversion operator from the result of the operator to the target type.</span></span> <span data-ttu-id="40113-467">適用可能な変換演算子がない場合は、し、固有がない最も拡大変換します。</span><span class="sxs-lookup"><span data-stu-id="40113-467">If there are no applicable conversion operators, then there is no most specific widening conversion.</span></span>

3. <span data-ttu-id="40113-468">次に、適用できる変換演算子の特定のソースの種類が決定されます。</span><span class="sxs-lookup"><span data-stu-id="40113-468">Then, the most specific source type of the applicable conversion operators is determined:</span></span>

   * <span data-ttu-id="40113-469">変換演算子のいずれかの変換元の型から直接場合、元の型が特定のソースの種類です。</span><span class="sxs-lookup"><span data-stu-id="40113-469">If any of the conversion operators convert directly from the source type, then the source type is the most specific source type.</span></span>

   * <span data-ttu-id="40113-470">それ以外の場合、最も固有のソースの種類は、変換演算子のソースの種類の結合されたセット内の最も内側の型。</span><span class="sxs-lookup"><span data-stu-id="40113-470">Otherwise, the most specific source type is the most encompassed type in the combined set of source types of the conversion operators.</span></span> <span data-ttu-id="40113-471">場合に最も内側のない型が見つからない、最も関連の変換を拡大し、します。</span><span class="sxs-lookup"><span data-stu-id="40113-471">If no most encompassed type can be found, then there is no most specific widening conversion.</span></span>

4. <span data-ttu-id="40113-472">次に、適用できる変換演算子の特定のターゲット型が決定されます。</span><span class="sxs-lookup"><span data-stu-id="40113-472">Then, the most specific target type of the applicable conversion operators is determined:</span></span>

   * <span data-ttu-id="40113-473">場合は、ターゲット型に直接変換演算子のいずれかの変換、対象の型は最も固有のターゲット型です。</span><span class="sxs-lookup"><span data-stu-id="40113-473">If any of the conversion operators convert directly to the target type, then the target type is the most specific target type.</span></span>

   * <span data-ttu-id="40113-474">それ以外の場合、最も固有のターゲット型は、対象の型の変換演算子の結合されたセット内の最も外側の型です。</span><span class="sxs-lookup"><span data-stu-id="40113-474">Otherwise, the most specific target type is the most encompassing type in the combined set of target types of the conversion operators.</span></span> <span data-ttu-id="40113-475">最も外側の型が見つからない場合、固有がない最も拡大変換します。</span><span class="sxs-lookup"><span data-stu-id="40113-475">If no most encompassing type can be found, then there is no most specific widening conversion.</span></span>

5. <span data-ttu-id="40113-476">次に、正確に 1 つの変換演算子は、特定のソースの種類から最も固有のターゲット型に変換、した場合、これが最も固有の変換演算子。</span><span class="sxs-lookup"><span data-stu-id="40113-476">Then, if exactly one conversion operator converts from the most specific source type to the most specific target type, then this is the most specific conversion operator.</span></span> <span data-ttu-id="40113-477">このような 1 つ以上のオペレーターが存在する場合に最も固有の拡大変換はあるありません。</span><span class="sxs-lookup"><span data-stu-id="40113-477">If more than one such operator exists, then there is no most specific widening conversion.</span></span>

### <a name="most-specific-narrowing-conversion"></a><span data-ttu-id="40113-478">最も固有の縮小変換</span><span class="sxs-lookup"><span data-stu-id="40113-478">Most Specific Narrowing Conversion</span></span>

<span data-ttu-id="40113-479">最も固有ユーザー定義縮小変換演算子の 2 種類の間を決定するには、次の手順を使用して実現されます。</span><span class="sxs-lookup"><span data-stu-id="40113-479">Determining the most specific user-defined narrowing conversion operator between two types is accomplished using the following steps:</span></span>

1. <span data-ttu-id="40113-480">最初に、すべての候補の変換演算子が収集されます。</span><span class="sxs-lookup"><span data-stu-id="40113-480">First, all of the candidate conversion operators are collected.</span></span> <span data-ttu-id="40113-481">変換演算子の候補は、ソースの種類のユーザー定義の変換演算子のすべてとすべてのターゲットの型で、ユーザー定義の変換演算子。</span><span class="sxs-lookup"><span data-stu-id="40113-481">The candidate conversion operators are all of the user-defined conversion operators in the source type and all of the user-defined conversion operators in the target type.</span></span>

2. <span data-ttu-id="40113-482">次に、すべての非該当変換演算子は、セットから削除されます。</span><span class="sxs-lookup"><span data-stu-id="40113-482">Then, all non-applicable conversion operators are removed from the set.</span></span> <span data-ttu-id="40113-483">変換演算子は、ソースの種類からオペランドの型への組み込みの変換演算子があるし、演算子の結果から対象の型への組み込みの変換演算子がある場合、ソースの種類とターゲットの種類に適用されます。</span><span class="sxs-lookup"><span data-stu-id="40113-483">A conversion operator is applicable to a source type and target type if there is an intrinsic conversion operator from the source type to the operand type and there is an intrinsic conversion operator from the result of the operator to the target type.</span></span> <span data-ttu-id="40113-484">適用可能な変換演算子がない場合は、最も固有の縮小変換はあるありません。</span><span class="sxs-lookup"><span data-stu-id="40113-484">If there are no applicable conversion operators, then there is no most specific narrowing conversion.</span></span>

3. <span data-ttu-id="40113-485">次に、適用できる変換演算子の特定のソースの種類が決定されます。</span><span class="sxs-lookup"><span data-stu-id="40113-485">Then, the most specific source type of the applicable conversion operators is determined:</span></span>

   * <span data-ttu-id="40113-486">変換演算子のいずれかの変換元の型から直接場合、元の型が特定のソースの種類です。</span><span class="sxs-lookup"><span data-stu-id="40113-486">If any of the conversion operators convert directly from the source type, then the source type is the most specific source type.</span></span>

   * <span data-ttu-id="40113-487">それ以外の場合、変換演算子のいずれかの変換元の型を包含する型から場合、これらの変換演算子のソースの種類の結合されたセット内の最も内側の型は、特定のソースの種類にします。</span><span class="sxs-lookup"><span data-stu-id="40113-487">Otherwise, if any of the conversion operators convert from types that encompass the source type, then the most specific source type is the most encompassed type in the combined set of source types of those conversion operators.</span></span> <span data-ttu-id="40113-488">場合に最も内側のない型が見つからない、最も固有の縮小変換はありません。</span><span class="sxs-lookup"><span data-stu-id="40113-488">If no most encompassed type can be found, then there is no most specific narrowing conversion.</span></span>

   * <span data-ttu-id="40113-489">それ以外の場合、特定のソースの種類は、変換演算子のソースの種類の結合されたセット内の最も外側の型。</span><span class="sxs-lookup"><span data-stu-id="40113-489">Otherwise, the most specific source type is the most encompassing type in the combined set of source types of the conversion operators.</span></span> <span data-ttu-id="40113-490">最も外側の型が見つからない場合に最も固有の縮小変換はあるありません。</span><span class="sxs-lookup"><span data-stu-id="40113-490">If no most encompassing type can be found, then there is no most specific narrowing conversion.</span></span>

4. <span data-ttu-id="40113-491">次に、適用できる変換演算子の特定のターゲット型が決定されます。</span><span class="sxs-lookup"><span data-stu-id="40113-491">Then, the most specific target type of the applicable conversion operators is determined:</span></span>

   * <span data-ttu-id="40113-492">場合は、ターゲット型に直接変換演算子のいずれかの変換、対象の型は最も固有のターゲット型です。</span><span class="sxs-lookup"><span data-stu-id="40113-492">If any of the conversion operators convert directly to the target type, then the target type is the most specific target type.</span></span>

   * <span data-ttu-id="40113-493">それ以外の場合、対象の型によって包含する型に変換、変換演算子のいずれかの場合これらの変換演算子のソースの種類の結合されたセット内の最も外側の型が特定のターゲット型にします。</span><span class="sxs-lookup"><span data-stu-id="40113-493">Otherwise, if any of the conversion operators convert to types that are encompassed by the target type, then the most specific target type is the most encompassing type in the combined set of source types of those conversion operators.</span></span> <span data-ttu-id="40113-494">最も外側の型が見つからない場合に最も固有の縮小変換はあるありません。</span><span class="sxs-lookup"><span data-stu-id="40113-494">If no most encompassing type can be found, then there is no most specific narrowing conversion.</span></span>

   * <span data-ttu-id="40113-495">それ以外の場合、最も固有のターゲット型は、対象の型の変換演算子の結合されたセット内の最も内側の型。</span><span class="sxs-lookup"><span data-stu-id="40113-495">Otherwise, the most specific target type is the most encompassed type in the combined set of target types of the conversion operators.</span></span> <span data-ttu-id="40113-496">場合に最も内側のない型が見つからない、最も固有の縮小変換はありません。</span><span class="sxs-lookup"><span data-stu-id="40113-496">If no most encompassed type can be found, then there is no most specific narrowing conversion.</span></span>

5. <span data-ttu-id="40113-497">次に、正確に 1 つの変換演算子は、特定のソースの種類から最も固有のターゲット型に変換、した場合、これが最も固有の変換演算子。</span><span class="sxs-lookup"><span data-stu-id="40113-497">Then, if exactly one conversion operator converts from the most specific source type to the most specific target type, then this is the most specific conversion operator.</span></span> <span data-ttu-id="40113-498">このような 1 つ以上のオペレーターが存在する場合に最も固有の縮小変換はあるありません。</span><span class="sxs-lookup"><span data-stu-id="40113-498">If more than one such operator exists, then there is no most specific narrowing conversion.</span></span>

## <a name="native-conversions"></a><span data-ttu-id="40113-499">ネイティブ変換</span><span class="sxs-lookup"><span data-stu-id="40113-499">Native Conversions</span></span>

<span data-ttu-id="40113-500">変換のいくつかに分類されます*ネイティブ変換*ネイティブ .NET Framework でサポートされているためです。</span><span class="sxs-lookup"><span data-stu-id="40113-500">Several of the conversions are classified as *native conversions* because they are supported natively by the .NET Framework.</span></span> <span data-ttu-id="40113-501">これらの変換ですが、使用して最適化することができます、`DirectCast`と`TryCast`変換の演算子とその他の特殊な動作です。</span><span class="sxs-lookup"><span data-stu-id="40113-501">These conversions are ones that can be optimized through the use of the `DirectCast` and `TryCast` conversion operators, as well as other special behaviors.</span></span> <span data-ttu-id="40113-502">分類は、ネイティブの変換と変換: 恒等変換、既定の変換、参照変換、配列の変換、値型の変換、または型パラメーター変換します。</span><span class="sxs-lookup"><span data-stu-id="40113-502">The conversions classified as native conversions are: identity conversions, default conversions, reference conversions, array conversions, value type conversions, and type parameter conversions.</span></span>

## <a name="dominant-type"></a><span data-ttu-id="40113-503">主要な型</span><span class="sxs-lookup"><span data-stu-id="40113-503">Dominant Type</span></span>

<span data-ttu-id="40113-504">型を指定する必要がある多くの場合、型の推定を決定するなどの状況で、*優先型*のセット。</span><span class="sxs-lookup"><span data-stu-id="40113-504">Given a set of types, it is often necessary in situations such as type inference to determine the *dominant type* of the set.</span></span> <span data-ttu-id="40113-505">型のセットの主要な型は、最初にその他の 1 つまたは複数の型への暗黙的な変換がない任意の型を削除することによって決定されます。</span><span class="sxs-lookup"><span data-stu-id="40113-505">The dominant type of a set of types is determined by first removing any types that one or more other types do not have an implicit conversion to.</span></span> <span data-ttu-id="40113-506">ない場合型左この時点で、主要な型はありません。</span><span class="sxs-lookup"><span data-stu-id="40113-506">If there are no types left at this point, there is no dominant type.</span></span> <span data-ttu-id="40113-507">主要な型が、最も内側の残りの種類のします。</span><span class="sxs-lookup"><span data-stu-id="40113-507">The dominant type is then the most encompassed of the remaining types.</span></span> <span data-ttu-id="40113-508">1 つ以上の型がある場合は、最も内側の主要な型はありません。</span><span class="sxs-lookup"><span data-stu-id="40113-508">If there is more than one type that is most encompassed, then there is no dominant type.</span></span>
