---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 08/06/2019
ms.author: erhopf
ms.openlocfilehash: 64ba910af24886c77404f4ca16e6963cf6efe25d
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/13/2019
ms.locfileid: "68968001"
---
## <a name="prerequisites"></a>前提条件

このクイック スタートでは以下が必要です。

* [Node 8.12.x 以降](https://nodejs.org/en/)
* Translator Text の Azure サブスクリプション キー

## <a name="create-a-project-and-import-required-modules"></a>プロジェクトの作成と必要なモジュールのインポート

任意の IDE またはエディターを使用して新しいプロジェクトを作成するか、`translate-text.js` という名前のファイルが含まれる新しいフォルダーをデスクトップ上に作成します。 次に、このコード スニペットをプロジェクト/ファイル内にコピーします。

```javascript
const request = require('request');
const uuidv4 = require('uuid/v4');
```

> [!NOTE]
> これらのモジュールを使用していない場合は、プログラムを実行する前にこれらをインストールする必要があります。 これらのパッケージをインストールするには、`npm install request uuidv4` を実行します。

これらのモジュールは、HTTP 要求を作成したり、`'X-ClientTraceId'` ヘッダーの一意識別子を作成したりする際に必要となります。

## <a name="set-the-subscription-key"></a>サブスクリプション キーの設定

このコードでは、環境変数 `TRANSLATOR_TEXT_KEY` から Translator Text のサブスクリプション キーが読み取られるよう試行されます。 環境変数を使い慣れていない場合は、`subscriptionKey` を文字列として設定し、条件ステートメントをコメント アウトすることができます。

このコードをプロジェクトにコピーします。

```javascript
/* Checks to see if the subscription key is available
as an environment variable. If you are setting your subscription key as a
string, then comment these lines out.

If you want to set your subscription key as a string, replace the value for
the Ocp-Apim-Subscription-Key header as a string. */
const subscriptionKey = process.env.TRANSLATOR_TEXT_KEY;
if (!subscriptionKey) {
  throw new Error('Environment variable for your subscription key is not set.')
};
```

## <a name="configure-the-request"></a>要求の構成

要求モジュールに用意されている `request()` メソッドには、HTTP メソッド、URL、要求パラメーター、ヘッダー、JSON 本文を `options` オブジェクトとして渡すことができます。 このコード スニペットで、実際の要求を構成してみましょう。

>[!NOTE]
> エンドポイント、ルート、および要求パラメーターの詳細については、「[Translator Text API 3.0: Transliterate](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-transliterate)」をご覧ください。

```javascript
let options = {
    method: 'POST',
    baseUrl: 'https://api.cognitive.microsofttranslator.com/',
    url: 'transliterate',
    qs: {
      'api-version': '3.0',
      'language': 'ja',
      'fromScript': 'jpan',
      'toScript': 'latn'
    },
    headers: {
      'Ocp-Apim-Subscription-Key': subscriptionKey,
      'Content-type': 'application/json',
      'X-ClientTraceId': uuidv4().toString()
    },
    body: [{
          'text': 'こんにちは'
    }],
    json: true,
};
```

要求を認証する最も簡単な方法は、このサンプルで使用している `Ocp-Apim-Subscription-Key` ヘッダーとしてサブスクリプション キーを渡すことです。 または、アクセス トークンのサブスクリプション キーを交換し、アクセス トークンを一緒に `Authorization` ヘッダーとして渡して要求を検証することもできます。

Cognitive Services のマルチサービス サブスクリプションを使用している場合は、要求のヘッダーに `Ocp-Apim-Subscription-Region` も含める必要があります。

詳細については、[認証](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication)に関するページをご覧ください。

## <a name="make-the-request-and-print-the-response"></a>要求の実行と応答の出力

次に、`request()` メソッドを使用して要求を作成します。 前のセクションで作成した `options` オブジェクトを第 1 引数として渡すと、修飾された JSON 応答が出力されます。

```javascript
request(options, function(err, res, body){
    console.log(JSON.stringify(body, null, 4));
});
```

>[!NOTE]
> このサンプルでは、`options` オブジェクト内で HTTP 要求を定義しています。 一方、要求モジュールでは、`.post` や `.get` などの便利なメソッドもサポートされています。 詳細については、[便利なメソッド](https://github.com/request/request#convenience-methods)に関するセクションを参照してください。

## <a name="put-it-all-together"></a>すべてをまとめた配置

これで、Translator Text API を呼び出して JSON 応答を返す簡単なプログラムが完成しました。 ここで、プログラムを実行してみましょう。

```console
node transliterate-text.js
```

作成したコードをサンプル コードと比較したい場合は、完全なサンプルを [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-NodeJS) から入手できます。

## <a name="sample-response"></a>応答のサンプル

```json
[
    {
        "script": "latn",
        "text": "konnichiwa"
    }
]
```

## <a name="clean-up-resources"></a>リソースのクリーンアップ

サブスクリプション キーをプログラムにハードコーディングしている場合は、このクイック スタートを終了するときにサブスクリプション キーを必ず削除してください。

## <a name="next-steps"></a>次の手順

API のリファレンスを見て、Translator Text API でできるすべてのことを理解してください。

> [!div class="nextstepaction"]
> [API リファレンス](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
