---
title: Azure Functions のエラー処理に関するガイダンス | Microsoft Docs
description: 関数の実行時に発生するエラーの処理に関する一般的なガイダンスと、バインド固有のエラーのトピックへのリンクを提供します。
services: functions
cloud: ''
documentationcenter: ''
author: craigshoemaker
manager: gwallace
ms.assetid: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 02/01/2018
ms.author: cshoe
ms.openlocfilehash: bc0ffec1bfe55ee81573feffc58e1adb11421eec
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68667442"
---
# <a name="azure-functions-error-handling"></a>Azure Functions のエラー処理

このトピックでは、関数の実行時に発生するエラーの処理に関する一般的なガイダンスを提供します。 さらに、発生する可能性があるバインド固有のエラーについて説明されているトピックへのリンクを提供します。 

## <a name="handling-errors-in-functions"></a>関数のエラーの処理
[!INCLUDE [bindings errors intro](../../includes/functions-bindings-errors-intro.md)]

 
## <a name="binding-error-codes"></a>エラー コードのバインド

Azure サービスと統合する際、基盤となるサービスの API からのエラーが発生する場合があります。 これらのサービスのエラー コード ドキュメントへのリンクは、次のトリガーおよびバインド リファレンス トピックの「**例外とリターン コード**」セクションで見つかります。

+ [Azure Cosmos DB](functions-bindings-cosmosdb.md#exceptions-and-return-codes)

+ [Blob Storage](functions-bindings-storage-blob.md#exceptions-and-return-codes)

+ [Event Hubs](functions-bindings-event-hubs.md#exceptions-and-return-codes)

+ [Notification Hubs](functions-bindings-notification-hubs.md#exceptions-and-return-codes)

+ [Queue Storage](functions-bindings-storage-queue.md#exceptions-and-return-codes)

+ [Service Bus](functions-bindings-service-bus.md#exceptions-and-return-codes)

+ [Table Storage](functions-bindings-storage-table.md#exceptions-and-return-codes)
