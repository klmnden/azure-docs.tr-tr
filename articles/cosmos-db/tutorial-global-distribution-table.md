---
title: Tablo API’si için Azure Cosmos DB genel dağıtım öğreticisi | Microsoft Docs
description: Tablo API’sini kullanarak Azure Cosmos DB genel dağıtımını ayarlamayı öğrenin.
services: cosmos-db
keywords: genel dağıtım, Tablo
documentationcenter: ''
author: SnehaGunda
manager: kfile
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/13/2017
ms.author: sngun
ms.custom: mvc
ms.openlocfilehash: f877baa33d94dad07250da9a10209555dbca65c9
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-table-api"></a>Tablo API’sini kullanarak Azure Cosmos DB genel dağıtımını ayarlama

Bu makale aşağıdaki görevleri kapsar: 

> [!div class="checklist"]
> * Azure portalını kullanarak genel dağıtımı yapılandırma
> * [Tablo API’sini](table-introduction.md) kullanarak genel dağıtımı yapılandırma

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-table-api"></a>Tablo API’sini kullanarak tercih edilen bir bölgeye bağlanma

[Genel dağıtımdan](distribute-data-globally.md) yararlanmak için istemci uygulamaları, belge işlemlerini gerçekleştirmek için kullanılacak bölgelerin tercihe göre sıralanmış listesini belirtebilir. [TableConnectionPolicy.PreferredLocations](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.table.tableconnectionpolicy.preferredlocations?view=azure-dotnet#Microsoft_Azure_CosmosDB_Table_TableConnectionPolicy_PreferredLocations) özelliği ayarlanarak bu yapılabilir. Azure Cosmos DB Tablo API’si SDK’sı, hesap yapılandırmasına, geçerli bölgesel kullanılabilirliğe ve sağlanan tercih listesine göre iletişim kurmak için en iyi uç noktayı seçer.

PreferredLocations, okuma için tercih edilen (çok girişli) konumların virgülle ayrılmış listesini içermelidir. Her istemci düşük gecikme okumaları için tercih edilen sırayla bu bölgelerin bir alt kümesini belirtebilir. Bölgeler [görünen adları](https://msdn.microsoft.com/library/azure/gg441293.aspx) kullanılarak adlandırılmalıdır, örneğin `West US`.

Tüm okuma işlemleri, PreferredLocations listesindeki ilk kullanılabilir bölgeye gönderilir. İstek başarısız olursa, istemci listedeki sonraki bölgeyi dener ve bu şekilde devam eder.

SDK, PreferredLocations listesinde belirtilen bölgelerden okuma işlemi yapmaya çalışır. Bu nedenle, örneğin, Veritabanı Hesabı dört bölgede kullanılabiliyorsa ancak istemci, PreferredLocations için yalnızca iki yazma bölgesi belirtiyorsa, yük devretme durumunda bile yazma bölgesinden okuma işlemi yapılmaz.

SDK, tüm yazma işlemlerini otomatik olarak geçerli yazma bölgesine gönderir.

PreferredLocations özelliği ayarlanmazsa, tüm istekler geçerli yazma bölgesinden işlenir.

Hepsi bu kadar. Böylece bu öğretici tamamlanmış olur. [Azure Cosmos DB’deki tutarlılık düzeyleri](consistency-levels.md) bölümünü okuyarak genel olarak çoğaltılan hesabınızın tutarlılığının nasıl yönetileceğini öğrenebilirsiniz. Ayrıca genel veritabanı çoğaltmasının Azure Cosmos DB’de nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Azure Cosmos DB ile verileri genel olarak dağıtma](distribute-data-globally.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdakileri yaptınız:

> [!div class="checklist"]
> * Azure portalını kullanarak genel dağıtımı yapılandırma
> * Azure Cosmos DB Tablo API’lerini kullanarak genel dağıtımı yapılandırma

