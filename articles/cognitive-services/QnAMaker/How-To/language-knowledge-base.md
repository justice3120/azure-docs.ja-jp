---
title: 英語以外のナレッジ ベース - QnA Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker は、さまざまな言語のナレッジ ベース コンテンツをサポートしています。 ただし、1 つの言語につき 1 つ QnA Maker サービスを確保する必要があります。 特定の QnA Maker サービスの言語は、そのサービスを対象に作成された 1 つ目のナレッジ ベースによって設定されます。
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 01/14/2019
ms.author: diberry
ms.openlocfilehash: 26792246267ced6d9fff50fe4fea11cc8d280d6a
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/13/2019
ms.locfileid: "68966688"
---
# <a name="language-support-of-knowledge-base-content-for-qna-maker"></a>QnA Maker のナレッジ ベース コンテンツの言語サポート
QnA Maker は、さまざまな言語のナレッジ ベース コンテンツをサポートしています。 ただし、1 つの言語につき 1 つ QnA Maker サービスを確保する必要があります。 特定の QnA Maker サービスの言語は、そのサービスを対象に作成された 1 つ目のナレッジ ベースによって設定されます。 サポートされている言語の一覧については、[こちら](../Overview/languages-supported.md)を参照してください。

言語は、抽出の対象となるデータ ソースのコンテンツから自動的に認識されます。 言語が正しく設定されていることは、新しい QnA Maker サービスを作成して、そこに新しいナレッジ ベースを作成した後で確認できます。

1. [Azure Portal](https://portal.azure.com/) に移動します。

2. **[リソース グループ]** を選択し、QnA Maker サービスがデプロイされているリソース グループに移動して、 **[Azure Search]** リソースを選択します。

    ![Azure Search リソースを選択](../media/qnamaker-how-to-language-kb/select-azsearch.png)

3. **[testkb]** インデックスを選択します。 この Azure Search インデックスは、常に最初に作成されるインデックスであり、そのサービス内の全ナレッジ ベースの保存済みコンテンツを含んでいます。 

    ![テスト ナレッジ ベースを選択](../media/qnamaker-how-to-language-kb/select-testkb.png)

4. testkb の詳細が表示されている **[フィールド]** セクションを選択します。

    ![[フィールド] を選択](../media/qnamaker-how-to-language-kb/selectfields.png)

5. **[アナライザー]** チェック ボックスをオンにして、言語の詳細を表示します。

    ![[アナライザー] を選択](../media/qnamaker-how-to-language-kb/select-analyzer.png)

6. アナライザーが特定の言語に設定されていることがわかります。 この言語は、ナレッジ ベースの作成手順で自動的に検出されたものです。 リソースの作成後に、この言語を変更することはできません。

    ![選択されたアナライザー](../media/qnamaker-how-to-language-kb/selected-analyzer.png)

## <a name="next-steps"></a>次の手順

> [!div class="nextstepaction"]
> [Azure Bot Service による QnA ボットの作成](../Tutorials/create-qna-bot.md)
