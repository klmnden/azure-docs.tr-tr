---
title: Azure Cosmos DB için Azure PowerShell örnekleri
description: Azure PowerShell Örnekleri - Azure Cosmos DB hesapları oluşturmanıza ve yönetmenize yardımcı olacak betikler.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/16/2017
ms.author: sngun
ms.openlocfilehash: 3498ac6a2a4aaa1682d7b5bc5aae5383866d5bcd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60926184"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db"></a>Azure Cosmos DB için Azure PowerShell örnekleri

Aşağıdaki tablo, Azure Cosmos DB’ye yönelik örnek Azure PowerShell betiklerinin bağlantılarını içerir. Şu anda PowerShell aracılığıyla yalnızca Azure Cosmos DB hesabını yönetebilirsiniz; veritabanları ve kapsayıcılar gibi diğer kaynaklar PowerShell aracılığıyla yönetilemez.

| |  |
|---|---|
|**Azure Cosmos DB hesabı oluşturma**||
|[Oluşturma ve SQL API'si ile Cosmos hesabı yapılandırma](scripts/create-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| SQL API ile kullanılacak tek bir Azure Cosmos DB hesabı oluşturur. |
|[Oluşturma ve Cosmos hesabı, MongoDB için Azure Cosmos DB API'si ile yapılandırma](scripts/create-mongodb-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Tek bir Cosmos hesap için MongoDB API'si ile Azure Cosmos DB'nin oluşturur. |
|[Oluşturma ve Gremlin API'si ile Cosmos hesabı yapılandırma](scripts/create-graph-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Gremlin API ile kullanılacak tek bir Azure Cosmos DB hesabı oluşturur. |
|[Oluşturma ve Cassandra API'si ile Cosmos hesabı yapılandırma](scripts/create-and-configure-cassandra-database.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cassandra API ile kullanılacak tek bir Azure Cosmos DB hesabı oluşturur. |
|[Oluşturma ve tablo API'si ile Cosmos hesabı yapılandırma](scripts/create-table-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Tablo API'si ile kullanılacak tek bir Azure Cosmos DB hesabı oluşturur. |
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
