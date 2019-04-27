---
title: Azure Cosmos DB için Azure CLI örnekleri
description: Azure CLI Örnekleri - Azure Cosmos DB hesapları, veritabanları, kapsayıcıları, bölgeleri ve güvenlik duvarları oluşturun ve yönetin.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/26/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: a6348024d4e84c27610f1294f916cca9a851b6b9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60892634"
---
# <a name="azure-cli-samples-for-azure-cosmos-db"></a>Azure Cosmos DB için Azure CLI örnekleri

Aşağıdaki tablo, Azure Cosmos DB’ye yönelik örnek Azure CLI betiklerinin bağlantılarını içerir. Tüm Azure Cosmos DB CLI komutlarına ait başvuru sayfalarına [Azure CLI Başvurusu](/cli/azure/cosmosdb) sayfasından erişilebilir.

| |  |
|---|---|
|**Azure Cosmos DB hesabı, veritabanı ve kapsayıcıları oluşturma**||
| [SQL API'sini kullanarak Azure Cosmos DB hesabı oluşturma](scripts/create-database-account-collections-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Bir tek Azure Cosmos DB hesabı, veritabanı ve kapsayıcı oluşturur. |
| [Cosmos DB'nin MongoDB kullanarak bir Azure Cosmos DB hesabı oluşturma](scripts/create-mongodb-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Tek bir Azure Cosmos DB hesabı, veritabanı ve koleksiyonu oluşturur. |
| [Gremlin API kullanarak Azure Cosmos DB hesabı oluşturma](scripts/create-gremlin-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Bir tek Azure Cosmos DB hesabını, veritabanını ve grafiği oluşturur. |
| [Cassandra API kullanarak Azure Cosmos DB hesabı oluşturma](scripts/create-cassandra-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Tek bir Azure Cosmos DB hesabı ve veritabanı oluşturur. |
| [Tablo API'sini kullanarak Azure Cosmos DB hesabı oluşturma](scripts/create-table-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Bir tek Azure Cosmos DB hesabı, veritabanı ve tablo oluşturur. |
|**Azure Cosmos DB’yi ölçeklendirme**||
| [Kapsayıcının aktarım hızını ölçeklendirme](scripts/scale-collection-throughput-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Kapsayıcıda sağlanan aktarım hızını değiştirir.|
| [Azure Cosmos DB veritabanı hesabını birden çok bölgede çoğaltma ve yük devretme önceliklerini yapılandırma](scripts/scale-multiregion-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Hesap verilerini, belirtilmiş yük devretme önceliğine göre birden fazla bölgeye genel olarak çoğaltır.|
|**Güvenli Azure Cosmos DB**||
| [Hesap anahtarlarını alma](scripts/secure-get-account-key-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Hesap için birincil ve ikincil ana yazma anahtarlarıyla birincil ve ikincil salt okunur anahtarları alır.|
| [MongoDB için Azure Cosmos DB API'si ile yapılandırılan Cosmos hesabı için bağlantı dizesini alın](scripts/secure-mongo-connection-string-cli.md?toc=%2fcli%2fazure%2ftoc.json) | MongoDB uygulamasını Azure Cosmos DB hesabınıza bağlamak için bağlantı dizesini alır.|
| [Hesap anahtarlarını yeniden oluşturma](scripts/secure-regenerate-key-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Hesap anahtarlarını yeniden oluşturun.|
| [Güvenlik duvarı oluşturma](scripts/create-firewall-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Onaylanmış bir makine kümesinden ve/veya bulut hizmetlerinden hesaba erişimi kısıtlayan bir gelen IP erişim denetimi ilkesi oluşturun.|
|**Yüksek kullanılabilirlik, olağanüstü durum kurtarma, yedekleme ve geri yükleme**||
| [Yük devretme ilkesini yapılandırma](scripts/ha-failover-policy-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Hesabın çoğaltıldığı her bölgenin yük devretme önceliğini ayarlar.|
|**Azure Cosmos DB’yi kaynaklara bağlama**||
| [Azure Cosmos DB’ye bir web uygulaması bağlama](../app-service/scripts/cli-connect-to-documentdb.md?toc=%2fcli%2fazure%2ftoc.json)|Azure Cosmos DB veritabanı ve Azure web uygulaması oluşturun ve bağlayın.|
|||
