---
title: Azure Container Instances の実行 - Text Analytics
titleSuffix: Azure Cognitive Services
description: Text Analytics コンテナーを感情分析イメージと一緒に Azure Container Instance にデプロイし、それを Web ブラウザーでテストします。
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.author: dapine
ms.openlocfilehash: 9ef529c9d505e5b305602c80a8dbef906f52269c
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68552530"
---
# <a name="deploy-a-sentiment-analysis-container-to-azure-container-instances"></a>Azure Container Instances への感情分析コンテナーのデプロイ

Cognitive Services [Text Analytics](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers) コンテナーを感情分析イメージと一緒に Azure [Container Instances](https://docs.microsoft.com/azure/container-instances/) にデプロイする方法について説明します。 この手順では、Text Analytics リソースの作成、関連付けられる感情分析イメージの作成、およびこの 2 つのオーケストレーションをブラウザーから実行する機能について例示します。 コンテナーを使用することで、開発者の関心をインフラストラクチャの管理から切り離し、アプリケーション開発に専念させることができます。

## <a name="prerequisites"></a>前提条件

* Azure サブスクリプションを使用してください。 Azure サブスクリプションをお持ちでない場合は、開始する前に [無料アカウント](https://azure.microsoft.com/free/) を作成してください。

[!INCLUDE [Create a Cognitive Services Text Analytics resource](../includes/create-text-analytics-resource.md)]

[!INCLUDE [Create a Text Analytics Containers on Azure Container Instances](../../containers/includes/create-container-instances-resource.md)]

[!INCLUDE [Verify the Sentiment Analysis container instance](../includes/verify-sentiment-analysis-container.md)]

## <a name="next-steps"></a>次の手順 

* さらに [Cognitive Services コンテナー](../../cognitive-services-container-support.md)を使用する
* [Text Analytics 接続済みサービス](../vs-text-connected-service.md)を使用する
