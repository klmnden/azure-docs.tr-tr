---
title: Toplu içeri aktarma ve Azure Cosmos DB toplu Yürütücü kitaplığını kullanarak verileri güncelleştirme
description: Toplu işlemler Azure Cosmos DB'de toplu olarak içeri aktarma ve toplu güncelleştirme API'leri toplu Yürütücü kitaplığı tarafından sunulan.
author: tknandu
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: ramkris
ms.reviewer: sngun
ms.openlocfilehash: f37e7f508805dcdc20626dddf6d27768e4ab0b01
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65978678"
---
# <a name="azure-cosmos-db-bulk-executor-library-overview"></a>Azure Cosmos DB toplu Yürütücü kitaplığına genel bakış
 
Azure Cosmos DB, esnek ölçek artırma desteklemek için tasarlanmış bir hızlı, esnek ve Global olarak dağıtılmış veritabanı hizmetidir: 

* Büyük okuma ve yazma aktarım hızı (milyon işlem / saniye).  
* Büyük hacimlerde (terabayttan ya da daha da fazla yüzlerce) depolama tahmin edilebilir bir milisaniyeden kısa gecikme ile işlemsel ve operasyonel verileri.  

Toplu Yürütücü kitaplık bu devasa işlem hacmi ve depolama yararlanmanıza yardımcı olur. Toplu Yürütücü kitaplığı toplu gerçekleştirmenize olanak tanıyan Azure Cosmos DB'de işlemleri toplu olarak içeri aktarma ve toplu güncelleştirme API'leri. Daha fazla toplu Yürütücü kitaplığı aşağıdaki bölümlerde özellikleri hakkında. 

> [!NOTE] 
> Şu anda toplu Yürütücü kitaplığı içeri aktarma destekler ve güncelleştirme işlemleri ve bu kitaplık yalnızca Azure Cosmos DB SQL API ve Gremlin API hesapları tarafından desteklenir.
 
## <a name="key-features-of-the-bulk-executor-library"></a>Toplu Yürütücü Kitaplığı'nın temel özellikleri  
 
* Bu, bir kapsayıcıya ayrılan aktarım hızı saturate için gereken istemci tarafı işlem kaynakları önemli ölçüde azaltır. Verileri kullanarak toplu içeri aktarma API 10 başarır Yazar tek bir iş parçacıklı uygulamanın verileri paralel olarak istemci kapasitesini doldurmaktan sırasında makinenin CPU Yazar çok iş parçacıklı bir uygulamaya kıyasla daha fazla yazma üretimi zaman.  

* Hemen, verimli bir şekilde bunları kitaplıktan işleyerek isteği, istek zaman aşımı ve diğer geçici özel durumlar oran sınırlandırma işlemek için uygulama mantığı yazmak tedious görevlerini soyutlar.  

* Ölçeği genişletmek uygulamalar toplu işlemleri gerçekleştirmek için basitleştirilmiş bir mekanizma sağlar. Bir Azure sanal makinesinde çalışan bir tek toplu Yürütücü örneği 500 K RU/sn büyüktür kullanabilir ve tek tek istemci sanal makinelerine ek örnekleri ekleyerek daha yüksek bir aktarım hızı elde edebilirsiniz.  
 
* Bu veri bir terabayt birden çok içeri aktarma bir saat içinde bir yukarı ölçeklenme mimarisi kullanarak toplu olarak.  

* Azure Cosmos DB kapsayıcıları mevcut verileri güncelleştirme düzeltme ekleri olarak topluca ekleyebilirsiniz. 
 
## <a name="how-does-the-bulk-executor-operate"></a>Toplu Yürütücü nasıl çalışır? 

Toplu işlem almak veya belgeleri güncelleştirmek için toplu işlem varlık ile tetiklendiğinde, bunlar başlangıçta, Azure Cosmos DB bölüm anahtar aralığı karşılık gelen bir demet içinde karışık. Bölüm anahtar aralığı için karşılık gelen her bir demette, bunlar küçük toplu işler ve her kısa batch Yasası sunucu tarafında kaydedilen bir yükü olarak ayrılır. Toplu Yürütücü kitaplığı, bu mini-toplu işlerin hem içinde hem de bölüm anahtar aralığı arasında eş zamanlı yürütme için iyileştirmeleri sunmuştur. Aşağıdaki görüntüde, nasıl toplu Yürütücü veri farklı bölüm anahtarlarının toplu işlemleri gösterilmiştir:  

![Toplu Yürütücü mimarisi](./media/bulk-executor-overview/bulk-executor-architecture.png)

Toplu Yürütücü kitaplığı, bir koleksiyon için ayrılan aktarım hızı maximally kullanmaya emin olur. Bunu kullanan bir [AIMD stili Tıkanıklık denetimi mekanizmasını](https://tools.ietf.org/html/rfc5681) her Azure Cosmos DB için bölüm anahtar aralığı, oran sınırlandırma ve zaman aşımları verimli bir şekilde işlemek için. 

## <a name="next-steps"></a>Sonraki Adımlar 
  
* Toplu Yürütücü kitaplıkta kullanan örnek uygulamalar denemeye daha fazla bilgi edinmek [.NET](bulk-executor-dot-net.md) ve [Java](bulk-executor-java.md).  
* Toplu Yürütücü SDK bilgiler ve yayın notları kullanıma [.NET](sql-api-sdk-bulk-executor-dot-net.md) ve [Java](sql-api-sdk-bulk-executor-java.md).
* Toplu Yürütücü kitaplığı Cosmos DB Spark Connector'a tümleşiktir, daha fazla bilgi için bkz. [Azure Cosmos DB Spark Bağlayıcısı](spark-connector.md) makalesi.  
* Toplu Yürütücü kitaplığı yeni bir sürümüne de tümleştirilen [Azure Cosmos DB Bağlayıcısı](https://aka.ms/bulkexecutor-adf-v2) Azure Data Factory, verileri kopyalamak için.
