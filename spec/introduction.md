---
ms.openlocfilehash: 637b90ee9b164ebf6e152541de0aff6f7d791db1
ms.sourcegitcommit: 6eca149bdc736113e0adb709212bd266c9503c33
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2019
ms.locfileid: "47426733"
---
# <a name="introduction"></a>はじめに

Microsoft&reg; Visual Basic&reg;は Microsoft .NET Framework の高度なプログラミング言語のプログラミング言語。 習得が簡単に、わかりやすい言語にされていますが、そのも経験豊富なプログラマのニーズを満たすのに十分な強力です。 Visual Basic プログラミング言語では、英語、わかりやすくするため、Visual Basic コードの読みやすさを促進する次のような構文があります。 可能な場合は、省略形、頭字語、または特殊文字の代わりにわかりやすい単語または語句が使用されます。 余分なまたは不要な構文は通常許可しますが、必要ありません。

Visual Basic プログラミング言語には、厳密に型指定または緩く型指定された言語のいずれかを指定できます。 柔軟な型指定の型チェックまでプログラムが既に実行中の負担の大部分を延期します。 これには、だけでなく型チェックの変換が、メソッドの呼び出しの実行時まで遅延できるメソッドの呼び出しのバインドのことを意味が含まれます。 これは、機能は、プロトタイプや開発の速度の実行速度より重要なその他のプログラムを作成するときに便利です。 プログラミング言語にも厳密には、Visual Basic では、すべての型がコンパイル時チェックを実行し、メソッド呼び出しの実行時バインディングを使用できませんのセマンティクスを入力します。 これは、最大のパフォーマンスを保証し、型変換が正しいことを確認を役立ちます。 これは、機能は、正確性が重要と実行の速度で実稼働アプリケーションを構築するときに便利です。

このドキュメントでは、Visual Basic 言語について説明します。 これは言語のチュートリアルやリファレンス マニュアルではなく、完全な言語の説明をするためのものです。

## <a name="grammar-notation"></a>文法の表記

この仕様は、2 つの grammar をについて説明します: 字句文法と構文文法。 字句文法を文字形式のトークンを組み合わせる方法を定義します。構文の文法では、トークンを結合して、Visual Basic プログラムを形成する方法を定義します。 条件付きコンパイルなどの操作の前処理に使用されるいくつかのセカンダリの文法もあります。

この仕様で文法が ANTLR 形式で記述されたを参照してください-- http://www.antlr.org/します。

場合は、Visual Basic プログラムで重要ではありません。 わかりやすくするため、標準の大文字と小文字のすべての端末が与えられますが、大文字小文字の区別も一致します。 ASCII 文字セットの要素が印刷可能な端末は、対応する ASCII 文字で表されます。 Visual Basic も幅を区別しない、端末を照合するときに半角 Unicode に対応する、全体のトークンごとにのみ一致するように、全角 Unicode 文字を許可します。 トークンは、混合半角と全角文字が含まれている場合に一致しません。

改行とインデント設定は、読みやすくするために追加することがあり、運用環境の一部ではないです。

## <a name="compatibility"></a>互換性

プログラミング言語の重要な機能には、言語の異なるバージョン間の互換性です。 言語の新しいバージョンでは、以前のバージョン、言語のと同じコードを受け入れませんまたは解釈します。 以前のバージョンとは異なる、負担に配置できますプログラマに別の言語の 1 つのバージョンから自分のコードをアップグレードする場合。 そのため、言語のコンシューマーにとってのメリットが明確で膨大な性質のバージョン間の互換性を除く保持する必要があります。

次のポリシーは、バージョン間での Visual Basic 言語の変更を制御します。 このコンテキストで使用すると、用語言語自体、Visual Basic 言語の構文および意味側面だけを参照し、の一部として含まれているすべての .NET Framework クラスには含まれません、`Microsoft.VisualBasic`名前空間 (およびサブ名前空間)。 .NET Framework のすべてのクラスは、このドキュメントの範囲外の別のバージョン管理との互換性ポリシーが適用されます。

### <a name="kinds-of-compatibility-breaks"></a>互換性の種類

理想の世界では、互換性を既存のバージョンの Visual Basic と Visual Basic の将来のバージョン間で 100% となります。 ただし、場合があります、互換性の中断の必要性が、コストを上回る場合に、プログラマでかける場合があります。 このような状況は次のとおりです。

* *新しい警告です。* 新しい警告を導入しない、本質的に、互換性は break です。 ただし、多くの開発者は、「警告として扱うエラー」がオンでコンパイル、ため、余分な注意が必要警告を導入するときにします。

* *新しいキーワードです。* 新しい言語機能を導入するときに、新しいキーワードの概要が必要可能性がありますに。 合理的な努力はユーザーの識別子と競合の可能性を最小限に抑えるキーワードを選択して、既存のキーワードを使用して、適切な場所になります。 以前のバージョンからプロジェクトをアップグレードし、新しいキーワードをエスケープするヘルプが提供されます。

* *コンパイラのバグです。* コンパイラの動作は、言語の仕様に記述されている動作に対応していないが、文書化されている動作と一致するコンパイラの動作を修正する必要があります。

* *仕様のバグです。* 明らかに間違った言語仕様を変更するには、コンパイラは言語仕様が言語仕様と一致すると、コンパイラの動作が必要な可能性があります。 「明らかに間違った」という語句は、文書化されている動作が明確かつ明確な大半のユーザーが期待するカウンター実行され、ユーザーに対する高度に望ましくない動作を生成することを意味します。

* *仕様のあいまいさ。* 特定の状況で行われますが、どのような言語の仕様を説明する必要があり、コンパイラが一貫性のないか、(以前の時点から同じ定義を使用) が明らかに間違った方法で状況を処理する、確認、仕様とコンパイラの動作を修正する必要があります。 つまり、ときに、仕様は、ケース a、b、d および e の大文字と小文字の c での動作が省略され、コンパイラが正しく動作しない場合は、c 可能性があるケース c で行われ、一致するようにコンパイラの動作の変更内容を文書化するために必要な。 (なお、仕様があいまいな状況でどのようになるかと、コンパイラが明らかに不適切なコンパイラの動作が事実上の仕様ではない方法で動作します。)

* *コンパイル時エラーには、実行時エラーを行っています。* コードの 100% の (つまり、ユーザー コードが、明確なバグで) 実行時に失敗することが保証がある場合、状況をキャッチするコンパイル時エラーを追加することが望ましい場合があります。

* *仕様の省略します。* ときに言語仕様は特に許可または禁止する特定の状況と、コンパイラが (コンパイラの動作が明確に正しくない場合、これは仕様のバグでは、仕様ではない望ましくない方法で状況を処理します。省略)、仕様を明確にし、コンパイラの動作を変更するために必要な場合があります。 だけでなく、通常への影響分析は、この種の変更は、変更の影響は非常に最小限と見なされます、開発者にとってのメリットが非常に高い場合にさらに制限します。

* *新しい機能です。* 一般に、新機能の導入が変わらないようにする言語仕様の既存のパーツまたはコンパイラの既存の動作。 新しい機能の概要でが要求される既存の言語仕様を変更する場合への影響は非常に最小限になるし、機能の利点は、高い場合にのみは、このような互換性区切り実行することは妥当です。

* *セキュリティ。* 異常な状況は、セキュリティに関する注意事項を削除または変更すると、本質的に安全でないし、ユーザーのクリアのセキュリティ リスクをもたらす機能など、互換性の中断と生じます。

次のような場合は、互換性を導入するための許容可能な理由ではできません。

* *望ましくないまたは嘆かわしい怠慢の動作です。* 言語の設計やコンパイラ動作が適切では望ましくないか今にして思えば嘆かわしい怠慢と見なされますは旧バージョンとの互換性の理由ではありません。 、以下で説明、言語の廃止プロセスは、代わりに使用する必要があります。

* *ほかに何か。* それ以外の場合、コンパイラの動作は、下位互換性があります。

### <a name="impact-criteria"></a>影響の条件

互換性の中断は許容されるかどうかを検討するとき、いくつかの条件は、変更の影響の特定に使用されます。 影響も大きく、互換性を受け入れるためのほど、バーが中断されます。

条件は次のとおりです。

* 変更のスコープとは何ですか。 つまり、プログラムの数が影響を受ける可能性がありますか。 ユーザーの数が影響を受ける可能性がありますか。 変更によって影響を受けるコードを記述するは一般的な方法がありますか。

* 回避策に変更する前に、同じ動作にも存在しますか。

* 変更は明確な方法ですか。 ユーザーは何かが変更された場合、またはそれらのプログラムだけ実行されますが異なる即時のフィードバックを入手するでしょうか。

* 変更程度対処できますアップグレード中にしますか。 最適な精度で、変更が発生した状況を検索および変更を回避するコードを変更できるツールを記述することはできますか。

* 変更のコミュニティからのフィードバックとは何ですか。

### <a name="language-deprecation"></a>言語の廃止

時間の経過と共に言語やコンパイラの部分を非推奨になる可能性があります。 前述のように、このような非推奨の機能を削除する互換性を中断する余裕がないです。 代わりに、次の手順に従う必要があります。

1. バージョンに存在する機能を指定された*A* 、Visual Studio のフィードバックを非推奨の機能と、最終的な非推奨の意思決定が行われる前に指定された完全な通知のユーザー コミュニティから要請する必要があります。 非推奨となるプロセスを元に戻すまたはユーザー コミュニティからのフィードバックに基づいて任意の時点で破棄されたことがあります。

2. 完全なバージョン (ポイント リリースではなく) *B*の Visual Studio は、非推奨の使用状況を警告するコンパイラの警告と共にリリースする必要があります。 既定でオンにする必要がある警告およびオフにすることができます。 廃止された機能は、製品ドキュメントでは、web で明確に記述する必要があります。

3. 完全なバージョン*C*の Visual Studio は、コンパイラの警告をオフにすることはできませんと共にリリースする必要があります。

4. 完全なバージョン*D* Visual Studio の必要があります、その後で解放する非推奨のコンパイラの警告コンパイラ エラーに変換します。 リリース*D*リリースのメイン ストリーム サポート フェーズ (この記事の執筆時点は 5 年間) の終了後に発生する必要があります*A*します。

5. 最後に、バージョン*E*のコンパイラ エラーを削除する Visual Studio がリリースされる可能性があります。

この非推奨となるフレームワーク内で処理できない変更は許可されません。
