---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 08/06/2019
ms.author: erhopf
ms.openlocfilehash: 5f0cf167a391cb14c70edd51832ec83cdda7bab6
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/13/2019
ms.locfileid: "68967969"
---
## <a name="prerequisites"></a>前提条件

* C# 7.1 以降
* [.NET SDK](https://www.microsoft.com/net/learn/dotnet/hello-world-tutorial)
* [Json.NET NuGet パッケージ](https://www.nuget.org/packages/Newtonsoft.Json/)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/)、[Visual Studio Code](https://code.visualstudio.com/download)、または任意のテキスト エディター
* Translator Text の Azure サブスクリプション キー

## <a name="create-a-net-core-project"></a>.NET Core プロジェクトを作成する

新しいコマンド プロンプト (またはターミナル セッション) を開き、次のコマンドを実行します。

```console
dotnet new console -o transliterate-sample
cd transliterate-sample
```

最初のコマンドでは、2 つのことを行っています。 新しい .NET コンソール アプリケーションを作成し、`transliterate-sample` という名前のディレクトリを作成します。 2 つ目のコマンドでは、目的のプロジェクトのディレクトリに変更しています。

次に、Json.Net をインストールする必要があります。 プロジェクトのディレクトリから、次を実行します。

```console
dotnet add package Newtonsoft.Json --version 11.0.2
```

## <a name="select-the-c-language-version"></a>C# 言語のバージョンを選択する

このクイック スタートでは、C# 7.1 以降が必要です。 プロジェクトに使用する C# のバージョンは、いくつかの方法で変更できます。 このガイドでは、`transliterate-sample.csproj` ファイルを調整する方法について説明します。 Visual Studio で言語を変更するなど、選択可能なすべての方法については、「[C# 言語のバージョンの選択](https://docs.microsoft.com/dotnet/csharp/language-reference/configure-language-version)」を参照してください。

対象のプロジェクトを開いて、`transliterate-sample.csproj` を開きます。 `LangVersion` は必ず 7.1 以降に設定してください。 該当する言語バージョンのプロパティ グループが存在しない場合は、次の行を追加します。

```xml
<PropertyGroup>
   <LangVersion>7.1</LangVersion>
</PropertyGroup>
```

## <a name="add-required-namespaces-to-your-project"></a>必要な名前空間をプロジェクトに追加する

先ほど実行した `dotnet new console` コマンドによって、プロジェクトが作成されています。これには `Program.cs` が含まれています。 このファイルに、アプリケーション コードを記述することになります。 `Program.cs` を開いて、既存の using ステートメントを置き換えます。 これらのステートメントによって、サンプル アプリのビルドと実行に必要なすべての型に確実にアクセスすることができます。

```csharp
using System;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
// Install Newtonsoft.Json with NuGet
using Newtonsoft.Json;
```

## <a name="create-classes-for-the-json-response"></a>JSON 応答用のクラスを作成する

次は、Translator Text API から返された JSON 応答を逆シリアル化するときに使用されるクラスを作成します。

```csharp
/// <summary>
/// The C# classes that represents the JSON returned by the Translator Text API.
/// </summary>
public class TransliterationResult
{
    public string Text { get; set; }
    public string Script { get; set; }
}
```

## <a name="create-a-function-to-transliterate-text"></a>テキストを表記変換するための関数を作成する

`Program` クラス内に、`TransliterateTextRequest()` という非同期関数を作成します。 この関数は、`subscriptionKey`、`host`、`route`、および `inputText` という 4 つの引数を受け取ります。

```csharp
static public async Task TransliterateTextRequest(string subscriptionKey, string host, string route, string inputText)
{
  /*
   * The code for your call to the translation service will be added to this
   * function in the next few sections.
   */
}
```

## <a name="serialize-the-translation-request"></a>変換要求をシリアル化する

次に、翻訳対象のテキストを含む JSON オブジェクトを作成してシリアル化する必要があります。 `body` には複数のオブジェクトを渡すことができる点に注目してください。

```csharp
object[] body = new object[] { new { Text = inputText } };
var requestBody = JsonConvert.SerializeObject(body);
```

## <a name="instantiate-the-client-and-make-a-request"></a>クライアントをインスタンス化して要求を行う

次の行によって `HttpClient` と `HttpRequestMessage` がインスタンス化されます。

```csharp
using (var client = new HttpClient())
using (var request = new HttpRequestMessage())
{
  // In the next few sections you'll add code to construct the request.
}
```

## <a name="construct-the-request-and-print-the-response"></a>要求を構成して応答を出力する

`HttpRequestMessage` 内で次のことを行います。

* HTTP メソッドを宣言する
* 要求 URI を構成する
* 要求本文 (シリアル化した JSON オブジェクト) を挿入する
* 必要なヘッダーを追加する
* 非同期要求を実行する
* 応答の出力

次のコードを `HttpRequestMessage` に追加します。

```csharp
// Build the request.
// Set the method to Post.
request.Method = HttpMethod.Post;
// Construct the URI and add headers.
request.RequestUri = new Uri(host + route);
request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
request.Headers.Add("Ocp-Apim-Subscription-Key", subscriptionKey);

// Send the request and get response.
HttpResponseMessage response = await client.SendAsync(request).ConfigureAwait(false);
// Read response as a string.
string result = await response.Content.ReadAsStringAsync();
// Deserialize the response using the classes created earlier.
TransliterationResult[] deserializedOutput = JsonConvert.DeserializeObject<TransliterationResult[]>(result);
// Iterate over the deserialized results.
foreach (TransliterationResult o in deserializedOutput)
{
    Console.WriteLine("Transliterated to {0} script: {1}", o.Script, o.Text);
}
```

Cognitive Services のマルチサービス サブスクリプションを使用している場合は、要求のパラメーターに `Ocp-Apim-Subscription-Region` も含める必要があります。 [マルチサービス サブスクリプションを使用した認証の詳細を参照してください](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication)。

## <a name="put-it-all-together"></a>すべてをまとめた配置

最後の手順は、`Main` 関数での `TransliterateTextRequest()` の呼び出しです。 このサンプルでは、日本語からラテン スクリプトへの表記変換を実行します。 `static void Main(string[] args)` を探してこのコードに置き換えます。

```csharp
static async Task Main(string[] args)
{
    // This is our main function.
    // Output languages are defined in the route.
    // For a complete list of options, see API reference.
    // https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-transliterate
    string subscriptionKey = "YOUR_TRANSLATOR_TEXT_KEY_GOES_HERE";
    string host = "https://api.cognitive.microsofttranslator.com";
    string route = "/transliterate?api-version=3.0&language=ja&fromScript=jpan&toScript=latn";
    string textToTransliterate = @"こんにちは";
    await TransliterateTextRequest(subscriptionKey, host, route, textToTransliterate);
}
```

`Main` では `subscriptionKey`、`host`、`route` が宣言され、`textToTransliterate` を表記変換するスクリプトがあることがわかります。

## <a name="run-the-sample-app"></a>サンプル アプリを実行する

以上で、サンプル アプリを実行する準備が整いました。 コマンド ライン (またはターミナル セッション) から、プロジェクト ディレクトリに移動して次のコマンドを実行します。

```console
dotnet run
```

## <a name="sample-response"></a>応答のサンプル

サンプルを実行すると、次のようにターミナルに出力されます。

```bash
Transliterated to latn script: Kon\'nichiwa
```

このメッセージは、次のように生の JSON からビルドされます。

```json
[
    {
        "script": "latn",
        "text": "konnichiwa"
    }
]
```

## <a name="clean-up-resources"></a>リソースのクリーンアップ

サブスクリプション キーなどの秘密情報は、サンプル アプリのソース コードからすべて確実に削除してください。

## <a name="next-steps"></a>次の手順

API のリファレンスを見て、Translator Text API でできるすべてのことを理解してください。

> [!div class="nextstepaction"]
> [API リファレンス](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
