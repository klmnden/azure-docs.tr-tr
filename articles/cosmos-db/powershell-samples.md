---
title: Azure Cosmos DB için Azure PowerShell Örnekleri | Microsoft Docs
description: Azure PowerShell Örnekleri - Azure Cosmos DB hesapları oluşturmanıza ve yönetmenize yardımcı olacak betikler.
services: cosmos-db
author: mimig1
manager: jhubbard
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: database
ms.date: 10/16/2017
ms.author: mimig
ms.openlocfilehash: f651a88f71e62518d0531a2d5cee5c1cd2bc5ce4
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="azure-powershell-samples-for-azure-cosmos-db"></a>Azure Cosmos DB için Azure PowerShell örnekleri

Aşağıdaki tablo, Azure Cosmos DB’ye yönelik örnek Azure PowerShell betiklerinin bağlantılarını içerir. Şu anda PowerShell aracılığıyla yalnızca Azure Cosmos DB hesap katmanını yönetebilirsiniz; veritabanları ve koleksiyonlar gibi diğer kaynaklar PowerShell aracılığıyla yönetilemez.

| |  |
|---|---|
|**Azure Cosmos DB hesabı oluşturma**||
|[SQL API hesabı oluşturma](scripts/create-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| SQL API ile kullanılacak tek bir Azure Cosmos DB hesabı oluşturur. |
|**Azure Cosmos DB’yi ölçeklendirme**||
|[Azure Cosmos DB hesabını birden çok bölgede çoğaltma ve yük devretme önceliklerini yapılandırma](scripts/scale-multiregion-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Hesap verilerini, belirtilmiş yük devretme önceliğine göre birden fazla bölgeye genel olarak çoğaltır.|
|**Güvenli Azure Cosmos DB**||
| [Hesap anahtarlarını alma](scripts/secure-get-account-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hesap için birincil ve ikincil ana yazma anahtarlarıyla birincil ve ikincil salt okunur anahtarları alır.|
| [MongoDB bağlantı dizesini alma](scripts/secure-mongo-connection-string-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | MongoDB uygulamanızı Azure Cosmos DB hesabınıza bağlamak için gerekli bağlantı dizesini alır.|
|[Hesap anahtarlarını yeniden oluşturma](scripts/secure-regenerate-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Hesap için ana veya salt okunur anahtarı yeniden oluşturur.|
|[Güvenlik duvarı oluşturma](scripts/create-firewall-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Onaylanmış bir makine kümesinden ve/veya bulut hizmetlerinden hesaba erişimi kısıtlayan bir gelen IP erişim denetimi ilkesi oluşturur.|
|**Yüksek kullanılabilirlik, olağanüstü durum kurtarma, yedekleme ve geri yükleme**||
|[Yük devretme ilkesini yapılandırma](scripts/ha-failover-policy-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Hesabın çoğaltıldığı her bölgenin yük devretme önceliğini ayarlar.|
|||
