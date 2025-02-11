---
title: VMware Solution by CloudSimple - Azure 仮想マシンの概要
description: CloudSimple 仮想マシンとその利点について学習します。
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 0f4967bbd12107bf6a04cb80537d4425c75c5f46
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/06/2019
ms.locfileid: "68812499"
---
# <a name="cloudsimple-virtual-machines-overview"></a>CloudSimple 仮想マシンの概要

CloudSimple を使用すると、Azure portal から VMware VM を管理できます。  クラスターまたは vSphere クラスターのリソース プールは、サブスクリプションにマップすることによって Azure を介して管理されます。  CloudSimple 仮想マシンを使用すると、Azure portal で VMware VM のセルフサービス管理を実現できます。  

Azure から CloudSimple VM を作成するには、VM テンプレートがお使いのプライベート クラウド vCenter に存在している必要があります。  テンプレートは、オペレーティング システムとアプリケーションをカスタマイズするために使用されます。  テンプレート VM は、企業のセキュリティ ポリシーを満たすように強化することができます。  テンプレートを使用して VM を作成し、セルフサービス モデルを使用して Azure portal から VM を使用することができます。

## <a name="benefits"></a>メリット

Azure portal から利用できる CloudSimple 仮想マシンは、ユーザーが VMware 仮想マシンを作成および管理するためのセルフサービス メカニズムを提供します。

* プライベート クラウド vCenter に CloudSimple VM を作成する
* VM プロパティを管理する
  * ディスクを追加/削除する
  * NIC を追加/削除する
* CloudSimple VM の電源操作
  * 電源オンと電源オフ
  * VM をリセットする
* VM を削除する

## <a name="next-steps"></a>次の手順

* [Azure で VMware VM を使用する](quickstart-create-vmware-virtual-machine.md)方法を学習する
* [Azure サブスクリプションをマップする](https://docs.azure.cloudsimple.com/azure-subscription-mapping/)方法を学習する