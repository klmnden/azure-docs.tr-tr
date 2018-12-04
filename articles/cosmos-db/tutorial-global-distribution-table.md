---
title: Tablo API'si için Azure Cosmos DB genel dağıtım Öğreticisi
description: Tablo API’sini kullanarak Azure Cosmos DB genel dağıtımını ayarlamayı öğrenin.
services: cosmos-db
keywords: genel dağıtım, Tablo
author: SnehaGunda
ms.author: sngun
ms.service: cosmos-db
ms.component: cosmosdb-table
ms.topic: tutorial
ms.custom: mvc
ms.date: 12/13/2017
ms.openlocfilehash: 92ae0c3dc07c6dada80e081c65484e4f6bf4f947
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52844787"
---
# <a name="set-up-azure-cosmos-db-global-distribution-using-the-table-api"></a>Tablo API’sini kullanarak Azure Cosmos DB genel dağıtımını ayarlama

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

