---
title: Azure Cosmos DB için Azure CLI örnekleri
description: Azure CLI Örnekleri - Azure Cosmos DB hesapları, veritabanları, kapsayıcıları, bölgeleri ve güvenlik duvarları oluşturun ve yönetin.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/26/2018
ms.author: mjbrown
ms.openlocfilehash: 001561c1eb0932d0a00f612f7b52c79e12202c53
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2018
ms.locfileid: "53545048"
---
# <a name="azure-cli-samples-for-azure-cosmos-db"></a>Azure Cosmos DB için Azure CLI örnekleri

Aşağıdaki tablo, Azure Cosmos DB’ye yönelik örnek Azure CLI betiklerinin bağlantılarını içerir. Tüm Azure Cosmos DB CLI komutlarına ait başvuru sayfalarına [Azure CLI Başvurusu](/cli/azure/cosmosdb) sayfasından erişilebilir.

| |  |
|---|---|
|**Azure Cosmos DB hesabı, veritabanı ve kapsayıcıları oluşturma**||
| [Bir Azure Cosmos DB hesabı oluşturun: SQL API'si](scripts/create-database-account-collections-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Tek bir Azure Cosmos DB SQL API hesabı, veritabanı ve kapsayıcısı oluşturur. |
| [Bir Azure Cosmos DB hesabı oluşturun: MongoDB API'si](scripts/create-mongodb-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Tek bir Azure Cosmos DB API hesabı, MongoDB, bir veritabanı ve koleksiyonu oluşturur. |
| [Bir Azure Cosmos DB hesabı oluşturun: Gremlin API'si](scripts/create-gremlin-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Tek bir Azure Cosmos DB Gremlin API hesabı, veritabanı ve grafı oluşturur. |
| [Bir Azure Cosmos DB hesabı oluşturun: Cassandra API'si](scripts/create-cassandra-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Tek bir Azure Cosmos DB Cassandra API hesabı ve veritabanı oluşturur. |
| [Bir Azure Cosmos DB hesabı oluşturun: Tablo API'si](scripts/create-table-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Tek bir Azure Cosmos DB Tablo API'si hesabı, veritabanı ve tablosu oluşturur. |
|**Azure Cosmos DB’yi ölçeklendirme**||
| [Kapsayıcının aktarım hızını ölçeklendirme](scripts/scale-collection-throughput-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Kapsayıcıda sağlanan aktarım hızını değiştirir.|
| [Azure Cosmos DB veritabanı hesabını birden çok bölgede çoğaltma ve yük devretme önceliklerini yapılandırma](scripts/scale-multiregion-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Hesap verilerini, belirtilmiş yük devretme önceliğine göre birden fazla bölgeye genel olarak çoğaltır.|
|**Güvenli Azure Cosmos DB**||
| [Hesap anahtarlarını alma](scripts/secure-get-account-key-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Hesap için birincil ve ikincil ana yazma anahtarlarıyla birincil ve ikincil salt okunur anahtarları alır.|
| [MongoDB için Azure Cosmos DB API bağlantı dizesini alın](scripts/secure-mongo-connection-string-cli.md?toc=%2fcli%2fazure%2ftoc.json) | MongoDB uygulamasını Azure Cosmos DB hesabınıza bağlamak için bağlantı dizesini alır.|
| [Hesap anahtarlarını yeniden oluşturma](scripts/secure-regenerate-key-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Hesap anahtarlarını yeniden oluşturun.|
| [Güvenlik duvarı oluşturma](scripts/create-firewall-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Onaylanmış bir makine kümesinden ve/veya bulut hizmetlerinden hesaba erişimi kısıtlayan bir gelen IP erişim denetimi ilkesi oluşturun.|
|**Yüksek kullanılabilirlik, olağanüstü durum kurtarma, yedekleme ve geri yükleme**||
| [Yük devretme ilkesini yapılandırma](scripts/ha-failover-policy-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Hesabın çoğaltıldığı her bölgenin yük devretme önceliğini ayarlar.|
|**Azure Cosmos DB’yi kaynaklara bağlama**||
| [Azure Cosmos DB’ye bir web uygulaması bağlama](../app-service/scripts/app-service-cli-app-service-documentdb.md?toc=%2fcli%2fazure%2ftoc.json)|Azure Cosmos DB veritabanı ve Azure web uygulaması oluşturun ve bağlayın.|
|||
