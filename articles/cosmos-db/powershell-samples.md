---
title: Azure Cosmos DB için Azure PowerShell örnekleri
description: Azure PowerShell Örnekleri - Azure Cosmos DB hesapları oluşturmanıza ve yönetmenize yardımcı olacak betikler.
services: cosmos-db
author: SnehaGunda
tags: azure-service-management
ms.service: cosmos-db
ms.custom: mvc
ms.topic: sample
ms.date: 10/16/2017
ms.author: sngun
ms.openlocfilehash: c2bd1ac165e6b17d1bd9604fb1203ae47214278f
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52871039"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db"></a>Azure Cosmos DB için Azure PowerShell örnekleri

Aşağıdaki tablo, Azure Cosmos DB’ye yönelik örnek Azure PowerShell betiklerinin bağlantılarını içerir. Şu anda PowerShell aracılığıyla yalnızca Azure Cosmos DB hesabını yönetebilirsiniz; veritabanları ve kapsayıcılar gibi diğer kaynaklar PowerShell aracılığıyla yönetilemez.

| |  |
|---|---|
|**Azure Cosmos DB hesabı oluşturma**||
|[SQL API hesabı oluşturma](scripts/create-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| SQL API ile kullanılacak tek bir Azure Cosmos DB hesabı oluşturur. |
|[MongoDB API hesabı oluşturma](scripts/create-mongodb-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| MongoDB API ile kullanılacak tek bir Azure Cosmos DB hesabı oluşturur. |
|[Gremlin API hesabı oluşturma](scripts/create-graph-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Gremlin API ile kullanılacak tek bir Azure Cosmos DB hesabı oluşturur. |
|[Cassandra API hesabı oluşturma](scripts/create-and-configure-cassandra-database.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cassandra API ile kullanılacak tek bir Azure Cosmos DB hesabı oluşturur. |
|[Tablo API'si hesabı oluşturma](scripts/create-table-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Tablo API'si ile kullanılacak tek bir Azure Cosmos DB hesabı oluşturur. |
|**Azure Cosmos DB’yi ölçeklendirme**||
|[Azure Cosmos DB hesabını birden çok bölgede çoğaltma ve yük devretme önceliklerini yapılandırma](scripts/scale-multiregion-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Hesap verilerini, belirtilmiş yük devretme önceliğine göre birden fazla bölgeye genel olarak çoğaltır.|
|**Güvenli Azure Cosmos DB**||
| [Hesap anahtarlarını alma](scripts/secure-get-account-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hesap için birincil ve ikincil ana yazma anahtarlarıyla birincil ve ikincil salt okunur anahtarları alır.|
| [MongoDB bağlantı dizesini alma](scripts/secure-mongo-connection-string-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | MongoDB uygulamanızı Azure Cosmos DB hesabınıza bağlamak için gerekli bağlantı dizesini alır.|
|[Hesap anahtarlarını yeniden oluşturma](scripts/secure-regenerate-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Hesap için ana veya salt okunur anahtarı yeniden oluşturur.|
|[Güvenlik duvarı oluşturma](scripts/create-firewall-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Onaylanmış bir makine kümesinden ve/veya bulut hizmetlerinden hesaba erişimi kısıtlayan bir gelen IP erişim denetimi ilkesi oluşturur.|
|**Yüksek kullanılabilirlik, olağanüstü durum kurtarma, yedekleme ve geri yükleme**||
|[Yük devretme ilkesini yapılandırma](scripts/ha-failover-policy-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Hesabın çoğaltıldığı her bölgenin yük devretme önceliğini ayarlar.|
|||
