---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 08/06/2019
ms.author: erhopf
ms.openlocfilehash: efb314b788f7718bef1dbf1588c87ba133229552
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/13/2019
ms.locfileid: "68968233"
---
## <a name="prerequisites"></a>前提条件

このクイック スタートでは以下が必要です。

* [Go](https://golang.org/doc/install)
* Translator Text の Azure サブスクリプション キー

## <a name="create-a-project-and-import-required-modules"></a>プロジェクトの作成と必要なモジュールのインポート

普段使用している IDE またはエディターで、新しい Go プロジェクトを作成します。 次に、このコード スニペットをプロジェクトの `detect-language.go` という名前のファイルにコピーします。

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "log"
    "net/http"
    "net/url"
    "os"
)
```

## <a name="create-the-main-function"></a>main 関数を作成する

このサンプルでは、環境変数 `TRANSLATOR_TEXT_KEY` から Translator Text のサブスクリプション キーが読み取られるよう試行されます。 環境変数を使い慣れていない場合は、`subscriptionKey` を文字列として設定し、条件ステートメントをコメント アウトすることができます。

このコードをプロジェクトにコピーします。

```go
func main() {
    /*
     * Read your subscription key from an env variable.
     * Please note: You can replace this code block with
     * var subscriptionKey = "YOUR_SUBSCRIPTION_KEY" if you don't
     * want to use env variables. If so, be sure to delete the "os" import.
     */
    subscriptionKey := os.Getenv("TRANSLATOR_TEXT_KEY")
    if subscriptionKey == "" {
       log.Fatal("Environment variable TRANSLATOR_TEXT_KEY is not set.")
    }
    /*
     * This calls our detect function, which we'll
     * create in the next section. It takes a single argument,
     * the subscription key.
     */
    detect(subscriptionKey)
}
```

## <a name="create-a-function-to-detect-the-text-language"></a>テキストの言語を検出するための関数を作成する

テキストの言語を検出する関数を作成しましょう。 この関数では、単一の引数である Translator Text サブスクリプション キーを使用します。

```go
func detect(subscriptionKey string) {
    /*  
     * In the next few sections, we'll add code to this
     * function to make a request and handle the response.
     */
}
```

次に、URL を構築しましょう。 URL は、`Parse()` メソッドと `Query()` メソッドを使用して構築されます。

このコードを `detect` 関数内にコピーします。

```go
// Build the request URL. See: https://golang.org/pkg/net/url/#example_URL_Parse
u, _ := url.Parse("https://api.cognitive.microsofttranslator.com/detect?api-version=3.0")
q := u.Query()
u.RawQuery = q.Encode()
```

>[!NOTE]
> エンドポイント、ルート、および要求パラメーターの詳細については、「[Translator Text API 3.0: Detect](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-detect)」をご覧ください。

## <a name="create-a-struct-for-your-request-body"></a>要求本文の構造体を作成する

次に、要求本文の匿名の構造体を作成し、`json.Marshal()` を使用して JSON としてエンコードします。 そのコードを `detect` 関数に追加します。

```go
// Create an anonymous struct for your request body and encode it to JSON
body := []struct {
    Text string
}{
    {Text: "Salve, Mondo!"},
}
b, _ := json.Marshal(body)
```

## <a name="build-the-request"></a>要求を作成する

要求本文を JSON としてエンコードしたので、ご自分の POST 要求を作成し、Translator Text API を呼び出すことができます。

```go
// Build the HTTP POST request
req, err := http.NewRequest("POST", u.String(), bytes.NewBuffer(b))
if err != nil {
    log.Fatal(err)
}
// Add required headers
req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
req.Header.Add("Content-Type", "application/json")

// Call the Translator Text API
res, err := http.DefaultClient.Do(req)
if err != nil {
    log.Fatal(err)
}
```

Cognitive Services のマルチサービス サブスクリプションを使用している場合は、要求のパラメーターに `Ocp-Apim-Subscription-Region` も含める必要があります。 [マルチサービス サブスクリプションを使用した認証の詳細を参照してください](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication)。

## <a name="handle-and-print-the-response"></a>応答を処理して出力する

次のコードを `detect` 関数に追加して、JSON 応答をデコードした後、結果を書式設定して出力します。

```go
// Decode the JSON response
var result interface{}
if err := json.NewDecoder(res.Body).Decode(&result); err != nil {
    log.Fatal(err)
}
// Format and print the response to terminal
prettyJSON, _ := json.MarshalIndent(result, "", "  ")
fmt.Printf("%s\n", prettyJSON)
```

## <a name="put-it-all-together"></a>すべてをまとめた配置

これで、Translator Text API を呼び出して JSON 応答を返す簡単なプログラムが完成しました。 ここで、プログラムを実行してみましょう。

```console
go run detect-language.go
```

作成したコードをサンプル コードと比較したい場合は、完全なサンプルを [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Go) から入手できます。

## <a name="sample-response"></a>応答のサンプル

国/地域の省略形は、こちらの[言語一覧](https://docs.microsoft.com/azure/cognitive-services/translator/language-support)で確認してください。

```json
[
  {
    "alternatives": [
      {
        "isTranslationSupported": true,
        "isTransliterationSupported": false,
        "language": "pt",
        "score": 1
      },
      {
        "isTranslationSupported": true,
        "isTransliterationSupported": false,
        "language": "en",
        "score": 1
      }
    ],
    "isTranslationSupported": true,
    "isTransliterationSupported": false,
    "language": "it",
    "score": 1
  }
]
```

## <a name="next-steps"></a>次の手順

API のリファレンスを見て、Translator Text API でできるすべてのことを理解してください。

> [!div class="nextstepaction"]
> [API リファレンス](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
