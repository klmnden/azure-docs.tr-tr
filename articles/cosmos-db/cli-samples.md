---
title: "Azure Cosmos DB için Azure CLI örnek | Microsoft Docs"
description: "Azure CLI örnekler - Azure Cosmos DB hesapları, veritabanları, kapsayıcıları, bölgeler ve güvenlik duvarları oluşturun ve yönetin."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: database
ms.date: 11/29/2017
ms.author: mimig
ms.openlocfilehash: 432ffc80d602a9e4eaf83fba15f0e6ebabd13603
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="azure-cli-samples-for-azure-cosmos-db"></a>Azure Cosmos DB için Azure CLI örnekleri

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)] 

Aşağıdaki tabloda Azure Cosmos DB için örnek Azure CLI komutlar bağlantılarını içerir. Başvuru sayfaları tüm Azure Cosmos DB CLI komutları için de kullanılabilir [Azure CLI 2.0 başvurusu](https://docs.microsoft.com/cli/azure/cosmosdb).

| |  |
|---|---|
|**Azure Cosmos DB hesap, veritabanı ve kapsayıcıları oluşturma**||
|[Bir SQL API hesabı oluşturun](scripts/create-database-account-collections-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Bir tek Azure Cosmos DB API hesabını, veritabanını ve kapsayıcı kullanmak için SQL API ile oluşturur. |
| [MongoDB API hesabı oluşturma](scripts/create-mongodb-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Bir tek Azure Cosmos DB MongoDB API hesap, veritabanı ve koleksiyonu oluşturur. |
|**Azure Cosmos DB ölçeklendirme**||
| [Ölçek kapsayıcı işleme](scripts/scale-collection-throughput-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Bir kapsayıcı üzerinde sağlanan througput değiştirir.|
|[Birden çok bölgelerdeki Azure Cosmos DB veritabanı hesabı çoğaltabilir ve yük devretme öncelikleri yapılandırın](scripts/scale-multiregion-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Belirtilen yük devretme önceliğe sahip birden çok bölgeleri genel hesap verilerini çoğaltır.|
|**Azure Cosmos DB güvenli**||
| [Hesap anahtarı alma](scripts/secure-get-account-key-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Birincil ve ikincil ana yazma anahtarları ve birincil ve ikincil salt okunur hesabı için anahtarları alır.|
| [MongoDB bağlantı dizesi Al](scripts/secure-mongo-connection-string-cli.md?toc=%2fcli%2fazure%2ftoc.json) | MongoDB uygulamanızı Azure Cosmos DB hesabınıza bağlanmak için bağlantı dizesini alır.|
|[Hesabı anahtarlarını yeniden oluştur](scripts/secure-regenerate-key-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Hesabı için asıl veya salt okunur anahtarı yeniden oluşturur.|
|[Bir güvenlik duvarı oluşturma](scripts/create-firewall-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Onaylanan bir dizi makineyi hesabına erişimi sınırlamak ve/veya Bulut Hizmetleri için bir gelen IP erişim denetimi ilkesi oluşturur.|
|**Yüksek kullanılabilirlik, olağanüstü durum kurtarma, yedekleme ve geri yükleme**||
|[Yük devretme ilkesi yapılandırma](scripts/ha-failover-policy-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Hesap çoğaltılır her bölgede yük devretme öncelik ayarlar.|
|**Azure Cosmos DB kaynaklarına bağlanma**||
|[Bir web uygulaması Azure Cosmos Veritabanına bağlanın](../app-service/scripts/app-service-cli-app-service-documentdb.md?toc=%2fcli%2fazure%2ftoc.json)|Oluşturabilir ve bir Azure Cosmos DB veritabanı ve Azure web uygulaması bağlayabilirsiniz.|
|||
