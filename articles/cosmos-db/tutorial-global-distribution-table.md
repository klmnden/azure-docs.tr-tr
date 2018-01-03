---
title: "Tablo API için Azure Cosmos DB genel dağıtım Öğreticisi | Microsoft Docs"
description: "Tablo API kullanarak Azure Cosmos DB genel dağıtım Kurulum öğrenin."
services: cosmos-db
keywords: "genel dağıtım, tablo"
documentationcenter: 
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/13/2017
ms.author: mimig
ms.custom: mvc
ms.openlocfilehash: 40c0bfe913e1396194de00cf6fa1d1ff823b1d0e
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2017
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-table-api"></a>Tablo API kullanarak Azure Cosmos DB genel dağıtım ayarlama

Bu makalede aşağıdaki görevleri içerir: 

> [!div class="checklist"]
> * Azure portalını kullanarak genel dağıtım yapılandırma
> * Genel dağıtım kullanarak yapılandırma [tablo API](table-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-table-api"></a>Tablo API kullanarak bir tercih edilen bölge bağlanma

Anlamıyla yararlanabilmek için [genel dağıtım](distribute-data-globally.md), istemci uygulamaları, belge işlemlerini gerçekleştirmek için kullanılacak bölgelerin sıralı tercih listesi belirtebilirsiniz. Bu ayar yapılabilir [TableConnectionPolicy.PreferredLocations](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.table.tableconnectionpolicy.preferredlocations?view=azure-dotnet#Microsoft_Azure_CosmosDB_Table_TableConnectionPolicy_PreferredLocations) özelliği. Azure Cosmos DB tablo API SDK'sı ile iletişim kurmak için en iyi uç noktası hesabı yapılandırması, geçerli bölge kullanılabilirliği ve sağlanan tercih listesi göre seçer.

PreferredLocations okuma için tercih edilen (çok girişli) konumları, virgülle ayrılmış listesini içermelidir. Her bir istemci örnek bir alt kümesini Bu bölgeler düşük gecikme süresi okuma tercih edilen sırayı belirtebilirsiniz. Bölgeleri kullanma şeklinde adlandırılmalıdır kendi [görünen adları](https://msdn.microsoft.com/library/azure/gg441293.aspx), örneğin, `West US`.

Tüm okuma PreferredLocations listedeki ilk kullanılabilir bölge gönderilir. İstek başarısız olursa, istemci listeyi sonraki bölgeyi başarısız ve benzeri.

SDK, belirtilen PreferredLocations bölgelerden okumaya çalışır. Bu nedenle, örneğin, veritabanı hesabı üç bölgelerde kullanılabilir, ancak PreferredLocations için iki yazma olmayan bölgeleri yalnızca istemci belirtir, sonra okuma yazma bölge, yük devretme durumunda bile dışında sunulacak.

SDK, geçerli yazma bölgesine tüm yazma işlemlerini otomatik olarak gönderir.

PreferredLocations özellik ayarlanmamışsa, tüm istekleri geçerli yazma bölgesinden sunulacak.

Bu, bu öğreticinin tamamlanan kadar. Genel olarak çoğaltılmış hesabınızı tutarlılığını okuyarak yönetmek nasıl öğrenebilirsiniz [Azure Cosmos veritabanı tutarlılık düzeylerini](consistency-levels.md). Ve Azure Cosmos DB'de nasıl genel veritabanı çoğaltma hakkında daha fazla bilgi çalıştığı için bkz: [Azure Cosmos DB genel verilerle dağıtmak](distribute-data-globally.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdakileri yaptığınızdan:

> [!div class="checklist"]
> * Azure portalını kullanarak genel dağıtım yapılandırma
> * Azure Cosmos DB tablo API'lerini kullanarak genel dağıtım yapılandırma

