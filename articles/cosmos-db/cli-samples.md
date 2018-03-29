---
title: Azure Cosmos DB için Azure CLI Örnekleri | Microsoft Docs
description: Azure CLI Örnekleri - Azure Cosmos DB hesapları, veritabanları, kapsayıcıları, bölgeleri ve güvenlik duvarları oluşturun ve yönetin.
services: cosmos-db
author: mimig1
manager: jhubbard
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: database
ms.date: 11/29/2017
ms.author: mimig
ms.openlocfilehash: 5a649279385da882687c50edd53e16f5ebaed163
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="azure-cli-samples-for-azure-cosmos-db"></a>Azure Cosmos DB için Azure CLI örnekleri

Aşağıdaki tablo, Azure Cosmos DB’ye yönelik örnek Azure CLI betiklerinin bağlantılarını içerir. Tüm Azure Cosmos DB CLI komutlarına ait başvuru sayfalarına [Azure CLI 2.0 Başvurusu](https://docs.microsoft.com/cli/azure/cosmosdb) sayfasından erişilebilir.

| |  |
|---|---|
|**Azure Cosmos DB hesabı, veritabanı ve kapsayıcıları oluşturma**||
|[SQL API hesabı oluşturma](scripts/create-database-account-collections-cli.md?toc=%2fcli%2fazure%2ftoc.json)| SQL API ile kullanılmak üzere tek bir Azure Cosmos DB API hesabı, veritabanı ve kapsayıcısı oluşturur. |
| [MongoDB API hesabı oluşturma](scripts/create-mongodb-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Tek bir Azure Cosmos DB MongoDB API hesabı, veritabanı ve koleksiyonu oluşturur. |
|**Azure Cosmos DB’yi ölçeklendirme**||
| [Kapsayıcının aktarım hızını ölçeklendirme](scripts/scale-collection-throughput-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Kapsayıcıda sağlanan aktarım hızını değiştirir.|
|[Azure Cosmos DB veritabanı hesabını birden çok bölgede çoğaltma ve yük devretme önceliklerini yapılandırma](scripts/scale-multiregion-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Hesap verilerini, belirtilmiş yük devretme önceliğine göre birden fazla bölgeye genel olarak çoğaltır.|
|**Güvenli Azure Cosmos DB**||
| [Hesap anahtarlarını alma](scripts/secure-get-account-key-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Hesap için birincil ve ikincil ana yazma anahtarlarıyla birincil ve ikincil salt okunur anahtarları alır.|
| [MongoDB bağlantı dizesini alma](scripts/secure-mongo-connection-string-cli.md?toc=%2fcli%2fazure%2ftoc.json) | MongoDB uygulamanızı Azure Cosmos DB hesabınıza bağlamak için gerekli bağlantı dizesini alır.|
|[Hesap anahtarlarını yeniden oluşturma](scripts/secure-regenerate-key-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Hesap için ana veya salt okunur anahtarı yeniden oluşturur.|
|[Güvenlik duvarı oluşturma](scripts/create-firewall-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Onaylanmış bir makine kümesinden ve/veya bulut hizmetlerinden hesaba erişimi kısıtlayan bir gelen IP erişim denetimi ilkesi oluşturur.|
|**Yüksek kullanılabilirlik, olağanüstü durum kurtarma, yedekleme ve geri yükleme**||
|[Yük devretme ilkesini yapılandırma](scripts/ha-failover-policy-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Hesabın çoğaltıldığı her bölgenin yük devretme önceliğini ayarlar.|
|**Azure Cosmos DB’yi kaynaklara bağlama**||
|[Azure Cosmos DB’ye bir web uygulaması bağlama](../app-service/scripts/app-service-cli-app-service-documentdb.md?toc=%2fcli%2fazure%2ftoc.json)|Azure Cosmos DB veritabanı ve Azure web uygulaması oluşturun ve bağlayın.|
|||
