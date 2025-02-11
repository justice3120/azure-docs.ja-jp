---
title: 変更、アプリのトレーニング、Node.js - LUIS
titleSuffix: Azure Cognitive Services
description: この Node.js クイック スタートでは、Home Automation アプリに発話の例を追加してアプリをトレーニングします。
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 05/29/2019
ms.author: diberry
ms.openlocfilehash: 6f0b19c1ba8d4a72ced19e74a3807c3962989e5d
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68560575"
---
# <a name="quickstart-change-model-using-nodejs"></a>クイック スタート: Node.js を使ってモデルを変更する

[!INCLUDE [Quickstart prerequisites for changing model](../../../includes/cognitive-services-luis-qs-change-model-intro-para.md)]

## <a name="prerequisites"></a>前提条件

[!INCLUDE [Quickstart prerequisites for changing model](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* 最新の [**Node.js**](https://nodejs.org/en/download/) と NPM。
* この記事の NPM の依存関係: [**request**](https://www.npmjs.com/package/request)、[**request-promise**](https://www.npmjs.com/package/request-promise)、[**fs-extra**](https://www.npmjs.com/package/fs-extra)。  
* [Visual Studio Code](https://code.visualstudio.com/)。

[!INCLUDE [Code is available in Azure-Samples GitHub repo](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>発話の例の JSON ファイル

[!INCLUDE [Quickstart explanation of example utterance JSON file](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]

## <a name="create-quickstart-code"></a>クイック スタート コードの作成 

`add-utterances.js` という名前のファイルに NPM 依存関係を追加します。

   [!code-javascript[NPM Dependencies](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=8-11 "NPM Dependencies")]

LUIS 定数をファイルに追加します。 次のコードをコピーし、実際のオーサリング キー、アプリケーション ID、およびバージョン ID に変更します。

   [!code-javascript[LUIS key and IDs](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=13-22 "LUIS key and IDs")]

発話を含むアップロード ファイルの名前と場所を追加します。 

   [!code-javascript[Add upload file](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=24-26 "Add upload file")]

`addUtterance` 関数のメソッドとオブジェクトを追加します。

   [!code-javascript[Add configuration information for adding utterance](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=28-67 "Add configuration information for adding utterance")]

`train` 関数のメソッドとオブジェクトを追加します。

   [!code-javascript[Add configuration information for training LUIS](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=69-101 "Add configuration information for training LUIS")]

HTTP 呼び出しを送受信する関数 `sendUtteranceToApi` を追加します。 

   [!code-javascript[Send the HTTP request](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=103-119 "Send the HTTP request")]

アクションを選択する **main** コードを追加します。

   [!code-javascript[Main function](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=121-143 "Main function")]

### <a name="install-dependencies"></a>依存関係をインストールする

次のテキストを含んだ `package.json` ファイルを作成します。

   [!code-json[Package.json](~/samples-luis/documentation-samples/quickstarts/change-model/node/package.json "Package.json")]

package.json があるディレクトリのコマンド ラインから、NPM (`npm install`) を実行して依存関係をインストールします。

## <a name="run-code"></a>コードの実行

Node.js を使用してコマンド ラインからアプリケーションを実行します。

`npm start` を呼び出すと、発話が追加され、トレーニングが実行されて、その状態が返されます。

```console
> npm start 
```

このコマンドラインで、発話の追加 API を呼び出した結果が表示されます。 

[!INCLUDE [Quickstart response from API calls](../../../includes/cognitive-services-luis-qs-change-model-json-results.md)]


## <a name="clean-up-resources"></a>リソースのクリーンアップ

このクイック スタートで作成したファイルは、完了後、すべて削除してください。 

## <a name="next-steps"></a>次の手順
> [!div class="nextstepaction"] 
> [プログラムで LUIS アプリを構築する](luis-tutorial-node-import-utterances-csv.md)
