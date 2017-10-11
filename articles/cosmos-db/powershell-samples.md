---
title: "Azure Cosmos DB Azure PowerShell örnekleri | Microsoft Docs"
description: "Azure PowerShell örnekleri - Azure Cosmos DB hesaplarını oluşturmak ve yönetmek amacıyla komut dosyaları."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: database
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 7698e03c0dc8d1c6d1e926f45e903a909bfd0c93
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="azure-powershell-samples-for-azure-cosmos-db"></a>Azure Cosmos DB Azure PowerShell örnekleri

Aşağıdaki tabloda Azure Cosmos DB için örnek Azure PowerShell betikleri bağlantılarını içerir.

| |  |
|---|---|
|**Bir Azure Cosmos DB hesabı oluşturma**||
|[Bir DocumentDB API hesabı oluşturun](scripts/create-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| DocumentDB API'si ile kullanmak için tek bir Azure Cosmos DB hesabı oluşturur. |
|**Azure Cosmos DB ölçeklendirme**||
|[Birden çok bölgeye Azure Cosmos DB hesabında çoğaltabilir ve yük devretme öncelikleri yapılandırın](scripts/scale-multiregion-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Belirtilen yük devretme önceliğe sahip birden çok bölgeleri genel hesap verilerini çoğaltır.|
|**Azure Cosmos DB güvenli**||
| [Hesap anahtarı alma](scripts/secure-get-account-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Birincil ve ikincil ana yazma anahtarları ve birincil ve ikincil salt okunur hesabı için anahtarları alır.|
| [MongoDB bağlantı dizesi Al](scripts/secure-mongo-connection-string-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | MongoDB uygulamanızı Azure Cosmos DB hesabınıza bağlanmak için bağlantı dizesini alır.|
|[Hesabı anahtarlarını yeniden oluştur](scripts/secure-regenerate-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Hesabı için asıl veya salt okunur anahtarı yeniden oluşturur.|
|[Bir güvenlik duvarı oluşturma](scripts/create-firewall-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Onaylanan bir dizi makineyi hesabına erişimi sınırlamak ve/veya Bulut Hizmetleri için bir gelen IP erişim denetimi ilkesi oluşturur.|
|**Yüksek kullanılabilirlik, olağanüstü durum kurtarma, yedekleme ve geri yükleme**||
|[Yük devretme ilkesi yapılandırma](scripts/ha-failover-policy-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Hesap çoğaltılır her bölgede yük devretme öncelik ayarlar.|
|||