---
title: Azure IoT Hub デバイス ツインの使用 (.NET/.NET) | Microsoft Docs
description: Azure IoT Hub デバイス ツインを使用してタグを追加し、IoT Hub クエリを使用する方法。 Azure IoT device SDK for .NET を使用して、シミュレートされたデバイス アプリを実装し、Azure IoT service SDK for .NET を使用して、タグを追加し、IoT Hub クエリを実行するサービス アプリを実装します。
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 05/15/2017
ms.author: robinsh
ms.openlocfilehash: 9bf34fd48c3a4a9a9672ac162f63dcce118b2c0a
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68668168"
---
# <a name="get-started-with-device-twins-net"></a>デバイス ツインの使用 (.NET)
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

このチュートリアルの最後には、次の .NET コンソール アプリが完成します。

* **CreateDeviceIdentity**: .NET アプリ。デバイス ID と関連付けられているセキュリティ キーを作成し、シミュレーション対象デバイス アプリを接続します。

* **AddTagsAndQuery**: .NET バックエンド アプリ。タグを追加してデバイス ツインのクエリを実行します。

* **ReportConnectivity**: .NET デバイス アプリ。以前作成したデバイス ID を使用して IoT hub に接続するデバイスをシミュレートし、接続の状態を報告します。

> [!NOTE]
> デバイス アプリとバックエンド アプリの両方をビルドするために使用できる Azure IoT SDK については、記事「[Azure IoT SDK](iot-hub-devguide-sdks.md)」を参照してください。
> 

このチュートリアルを完了するには、以下が必要です。

* 見ることができます。
* アクティブな Azure アカウントアカウントがない場合、Azure 試用版にサインアップして、最大 10 件の無料 Mobile Apps を入手できます。 (アカウントがない場合は、[無料アカウント](https://azure.microsoft.com/pricing/free-trial/) を数分で作成できます)。

## <a name="create-an-iot-hub"></a>IoT Hub の作成

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>IoT ハブに新しいデバイスを登録する

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="get-the-iot-hub-connection-string"></a>IoT ハブ接続文字列を取得する

[!INCLUDE [iot-hub-howto-twin-shared-access-policy-text](../../includes/iot-hub-howto-twin-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-custom-connection-string](../../includes/iot-hub-include-find-custom-connection-string.md)]

## <a name="create-the-service-app"></a>デバイス アプリを作成する

このセクションでは、**myDeviceId** に関連付けられたデバイス ツインに場所のメタデータを追加する .NET コンソール アプリを (C# を使用) 作成します。 その後、米国にあるデバイスで、携帯ネットワーク接続を報告しているものを選択して、IoT Hub に格納されているデバイス ツインに対してクエリを実行します。

1. Visual Studio で、 **[コンソール アプリケーション]** プロジェクト テンプレートを使用し、Visual C# Windows クラシック デスクトップ プロジェクトを現在のソリューションに追加します。 プロジェクトに **AddTagsAndQuery** という名前を付けます。
   
    ![New Visual C# Windows Classic Desktop project](./media/iot-hub-csharp-csharp-twin-getstarted/createnetapp.png)

2. ソリューション エクスプローラーで **AddTagsAndQuery** プロジェクトを右クリックし、 **[NuGet パッケージの管理...]** をクリックします。

3. **[NuGet パッケージ マネージャー]** ウィンドウで **[参照]** を選択し、**Microsoft.Azure.Devices** を検索します。 **[インストール]** を選択して、**Microsoft.Azure.Devices** パッケージをインストールし、使用条件に同意します。 この手順により、パッケージのダウンロードとインストールが実行され、[Azure IoT service SDK](https://www.nuget.org/packages/Microsoft.Azure.Devices/) NuGet パッケージへの参照とその依存関係が追加されます。
   
    ![NuGet Package Manager window](./media/iot-hub-csharp-csharp-twin-getstarted/servicesdknuget.png)

4. **Program.cs** ファイルの先頭に次の `using` ステートメントを追加します。

    ```csharp  
    using Microsoft.Azure.Devices;
    ```

5. **Program** クラスに次のフィールドを追加します。 プレースホルダーの値を、先ほど「[IoT ハブ接続文字列を取得する](#get-the-iot-hub-connection-string)」でコピーしておいた IoT Hub 接続文字列に置き換えます。

    ```csharp  
    static RegistryManager registryManager;
    static string connectionString = "{iot hub connection string}";
    ```

6. **Program** クラスに次のメソッドを追加します。

    ```csharp  
    public static async Task AddTagsAndQuery()
    {
        var twin = await registryManager.GetTwinAsync("myDeviceId");
        var patch =
            @"{
                tags: {
                    location: {
                        region: 'US',
                        plant: 'Redmond43'
                    }
                }
            }";
        await registryManager.UpdateTwinAsync(twin.DeviceId, patch, twin.ETag);
   
        var query = registryManager.CreateQuery(
          "SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
        var twinsInRedmond43 = await query.GetNextAsTwinAsync();
        Console.WriteLine("Devices in Redmond43: {0}", 
          string.Join(", ", twinsInRedmond43.Select(t => t.DeviceId)));
   
        query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
        var twinsInRedmond43UsingCellular = await query.GetNextAsTwinAsync();
        Console.WriteLine("Devices in Redmond43 using cellular network: {0}", 
          string.Join(", ", twinsInRedmond43UsingCellular.Select(t => t.DeviceId)));
    }
    ```
   
    **RegistryManager** クラスに、サービスからデバイス ツインとやりとりするのに必要なすべてのメソッドが表示されます。 前のコードでは、まず **registryManager** オブジェクトを初期化し、**myDeviceId** のデバイス ツインを取得して、最後にタグを目的の位置情報で更新します。
   
    更新後、2 つのクエリを実行します。1 番目のクエリでは、**Redmond43** 工場にあるデバイスのデバイス ツインのみを選択し、2 番目のクエリでは携帯ネットワーク経由で接続しているデバイスのみを選択します。
   
    前のコードでは、**query** オブジェクトの作成時に返されるドキュメントの最大数を指定します。 **query** オブジェクトには、**GetNextAsTwinAsync** メソッドを複数回呼び出してすべての結果を取得する際に使用できる **HasMoreResults** のブール型プロパティが含まれます。 **GetNextAsJson** というメソッドは、集計クエリの結果など、デバイス ツインではない結果に使用できます。

7. 最後に、**Main** メソッドに次の行を追加します。

    ```csharp  
    registryManager = RegistryManager.CreateFromConnectionString(connectionString);
    AddTagsAndQuery().Wait();
    Console.WriteLine("Press Enter to exit.");
    Console.ReadLine();
    ```

8. ソリューション エクスプローラーで、 **[スタートアップ プロジェクトの設定...]** を開き、**AddTagsAndQuery** プロジェクトの **[アクション]** が **[開始]** になっていることを確認します。 ソリューションをビルドします。

9. **AddTagsAndQuery** プロジェクトを右クリックし、 **[デバッグ]** を選択してから、 **[新しいインスタンスを開始]** を選択して、このアプリケーションを実行します。 **Redmond43** にあるすべてのデバイスを照会するクエリの結果には、1 件のデバイスが表示され、携帯ネットワークを使用するデバイスに絞り込んだ結果には 0 件のデバイスが表示されます。
   
    ![ウィンドウに表示されたクエリの結果](./media/iot-hub-csharp-csharp-twin-getstarted/addtagapp.png)

次のセクションでは、接続情報を報告し、前のセクションのクエリの結果を変更するデバイス アプリを作成します。

## <a name="create-the-device-app"></a>デバイス アプリを作成する

このセクションでは、**myDeviceId** としてハブに接続し、報告されるプロパティに携帯ネットワークを使用しているという情報を含めるよう更新する .NET コンソール アプリを作成します。

1. Visual Studio で、 **[コンソール アプリケーション]** プロジェクト テンプレートを使用し、Visual C# Windows クラシック デスクトップ プロジェクトを現在のソリューションに追加します。 プロジェクトに **ReportConnectivity** という名前をつけます。
   
    ![New Visual C# Windows Classic device app](./media/iot-hub-csharp-csharp-twin-getstarted/createdeviceapp.png)
    
2. ソリューション エクスプローラーで **ReportConnectivity** プロジェクトを右クリックし、 **[NuGet パッケージの管理...]** をクリックします。

3. **[NuGet パッケージ マネージャー]** ウィンドウで **[参照]** を選択し、**Microsoft.Azure.Devices.Client** を検索します。 **[インストール]** を選択して、**Microsoft.Azure.Devices.Client** パッケージをインストールし、使用条件に同意します。 この手順により、パッケージのダウンロードとインストールが実行され、[Azure IoT device SDK](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/) NuGet パッケージへの参照とその依存関係が追加されます。
   
    ![NuGet Package Manager window Client app](./media/iot-hub-csharp-csharp-twin-getstarted/clientsdknuget.png)

4. **Program.cs** ファイルの先頭に次の `using` ステートメントを追加します。

    ```csharp  
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    using Newtonsoft.Json;
    ```

5. **Program** クラスに次のフィールドを追加します。 プレースホルダーの値は、前のセクションで記したデバイス接続文字列に置き換えてください。

    ```csharp  
    static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;
      DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
    static DeviceClient Client = null;
    ```

6. **Program** クラスに次のメソッドを追加します。

    ```csharp
    public static async void InitClient()
    {
        try
        {
            Console.WriteLine("Connecting to hub");
            Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, 
              TransportType.Mqtt);
            Console.WriteLine("Retrieving twin");
            await Client.GetTwinAsync();
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }
    }
    ```

    **Client** オブジェクトに、デバイスからデバイス ツインとやりとりするのに必要なすべてのメソッドが表示されます。 上記のコードは**クライアント**オブジェクトを初期化し、次に **myDeviceId** のデバイス ツインを取得します。

7. **Program** クラスに次のメソッドを追加します。

    ```csharp  
    public static async void ReportConnectivity()
    {
        try
        {
            Console.WriteLine("Sending connectivity data as reported property");
            
            TwinCollection reportedProperties, connectivity;
            reportedProperties = new TwinCollection();
            connectivity = new TwinCollection();
            connectivity["type"] = "cellular";
            reportedProperties["connectivity"] = connectivity;
            await Client.UpdateReportedPropertiesAsync(reportedProperties);
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }
    }
    ```

   上記のコードは、**myDeviceId**の報告対象プロパティを接続情報で更新します。

8. 最後に、**Main** メソッドに次の行を追加します。

    ```csharp
    try
    {
        InitClient();
        ReportConnectivity();
    }
    catch (Exception ex)
    {
        Console.WriteLine();
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    Console.WriteLine("Press Enter to exit.");
    Console.ReadLine();
    ```

9. ソリューション エクスプローラーで、 **[スタートアップ プロジェクトの設定...]** を開き、**ReportConnectivity** プロジェクトの **[アクション]** が **[開始]** になっていることを確認します。 ソリューションをビルドします。

10. **ReportConnectivity** プロジェクトを右クリックし、 **[デバッグ]** を選択してから、 **[新しいインスタンスを開始]** を選択して、このアプリケーションを実行します。 ツインの情報を取得し、次に接続性が*報告対象プロパティ*として送信される様子が見られます。
   
    ![デバイス アプリを実行して接続性を報告](./media/iot-hub-csharp-csharp-twin-getstarted/rundeviceapp.png)
       
11. これで、デバイスが接続情報を報告したため、両方のクエリで表示されるようになります。 .NET **AddTagsAndQuery** アプリを実行して、クエリをもう一度実行します。 今回は、**myDeviceId** が両方のクエリ結果に表示されるはずです。
   
    ![デバイスの接続性が正常に報告される](./media/iot-hub-csharp-csharp-twin-getstarted/tagappsuccess.png)

## <a name="next-steps"></a>次の手順

このチュートリアルでは、Azure Portal で新しい IoT Hub を構成し、IoT Hub の ID レジストリにデバイス ID を作成しました。 バックエンド アプリからデバイスのメタデータをタグとして追加し、シミュレート対象デバイス アプリでデバイス ツインのデバイスの接続情報を報告するよう記述しました。 さらに、SQL に似た IoT Hub クエリ言語を使用してこの情報を照会する方法も学習しました。

詳細については、次のリソースをご覧ください。

* デバイスからテレメトリを送信する: [デバイスから IoT ハブにテレメトリを送信する方法](quickstart-send-telemetry-dotnet.md)のチュートリアル、

* デバイス ツインの必要なプロパティを使用してデバイスを構成する: [必要なプロパティを使用してデバイスを構成する](tutorial-device-twins.md)方法のチュートリアル、

* デバイスを対話形式で制御する (ユーザー制御アプリからファンをオンにするなど): [ダイレクト メソッドの使用](quickstart-control-device-dotnet.md)に関するチュートリアル。
