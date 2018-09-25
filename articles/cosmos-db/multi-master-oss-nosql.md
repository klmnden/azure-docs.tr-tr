---
title: Azure Cosmos DB çok yöneticili açık kaynak NoSQL veritabanları ile kullanma
description: ''
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: ea3a16cf6b5aa4e46ad401e59aacb4d936a7429a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46963909"
---
# <a name="using-azure-cosmos-db-multi-master-with-open-source-nosql-databases"></a>Azure Cosmos DB çok yöneticili açık kaynak NoSQL veritabanları ile kullanma

Azure Cosmos DB çok yöneticili destek Cosmos DB tarafından desteklenen tüm açık kaynak NoSQL teklifleri için kullanılabilir olan yerel, sunucu tarafı bir uygulamadır. Ayrıca tüm tarafından erişilebilir durumda Cosmos DB SDK'ları desteklenir.

Azure Cosmos DB, bu açık kaynaklı bir NoSQL veritabanları için çok yöneticili desteği sunmak dünyanın ilk hizmettir.

|Model  |Destek  |
|---------|---------|
|MongoDB  | Etkin-Etkin  |
|Graf  | Etkin-Etkin |
|Cassandra  | Etkin-Etkin   |

## <a name="use-mongodb-clients-with-multi-master"></a>Çok yöneticili MongoDB istemcileri kullanın

Çok yöneticili özellik, diğer Azure Cosmos DB API'leri için etkin şekilde MongoDB API'si hesapları için etkinleştirildi. MongoDB API hesabı için çok yöneticili etkinleştirdikten sonra her bir istemci uygulaması örneğini, tercih edilen bir bölgeye okuma ve yazma işlemleri için seçebilirsiniz. MongoDB sürücü açısından bakıldığında, tercih edilen bir bölgeye çoğaltma birincil kümesi istemciye görünür. Bu şekilde, her bölgeye dağıtılmış veritabanınızın ayarlamak birincil çoğaltma olarak hareket eder. Azure Cosmos DB çok yöneticili yazma gecikmeleri, Global olarak dağıtılmış bir MongoDB uygulamaları için önemli ölçüde azaltan sağlar. 

### <a name="set-the-primary-region"></a>Birincil bölge ayarlayın

Her bir MongoDB istemcisi örneği ekleyerek birincil bölge seçebilirsiniz `@<preferred_primary_region_name>` "uygulama adı" alanına MongoDB istemci sürücü tarafından kabul edildi. Çoğu sürücüleri Bu bağlantı dizesinde gibi kabul edin:

`mongodb://fabrikam:KEY@fabrikam.documents.azure.com:10255/?ssl=true&replicaSet=globaldb&appname=@West US`

Bağlandıktan sonra aşağıdakilerden birini aşağıdaki durumlarda oluşabilir:

* Ardından bir yazma bölgesi tercih edilen bölge adı varsa, Azure Cosmos DB, birincil bölge seçer.

* Tercih edilen bölge adı sağlanmazsa, Azure Cosmos DB varsayılan bölge birincil bölgeye seçer.

* Tercih edilen bölge adı sağlanırsa ancak veritabanı hesabı için bir yazma bölgesi olarak kullanılabilir değilse, Azure Cosmos DB birincil bölgede kullanılabilir olan en yakın yazma bölgesi seçin.

NodeJS sürücüsünü konağa ilk sorun Yazar her zaman olacak gibi belirli sürücüleri ilk bağlantı dizesi belirtilen. Bu tür sürücüler için yazma uygulama adına ek olarak tercih edilen bir bölgeye yönlendirilir emin olmak için bölge adı eklemek için bağlantı dizesi DNS adını değiştirmeniz gerekir. Bölge adı boşluklar olmadan belirttiğinizden emin olun. Örneğin:

`mongodb://fabrikam:KEY@fabrikam-westus.documents.azure.com:10255/?ssl=true&replicaSet=globaldb&appname=@West US`

### <a name="conflict-resolution-mode"></a>Çakışma çözümleme modu

Çakışma çözümleme için Azure Cosmos DB MongoDB API hesabı her zaman moddur son yazıcı-yazma kabul bölgesel sunucu zaman damgası kullanarak WINS,.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Cosmos DB MongoDB API hesaplar için çok yöneticili destek hakkında bilgi edindiniz. Ardından, aşağıdaki kaynakları arayın:

* [Azure Cosmos DB hesapları için çok yöneticili etkinleştirme](enable-multi-master.md)

* [Azure Cosmos DB'de çakışma çözümü anlama](multi-master-conflict-resolution.md)
