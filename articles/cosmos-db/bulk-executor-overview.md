---
title: Azure Cosmos DB toplu Yürütücü kitaplığına genel bakış | Microsoft Docs
description: Kitaplık ve mimarisinin kullanmanın yararları Azure Cosmos DB toplu Yürütücü Kitaplığı hakkında bilgi edinin.
keywords: Java toplu Yürütücü
services: cosmos-db
author: tknandu
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: ramkris
ms.openlocfilehash: 893888904b6afc583c3c20b94d08eb3255a98cad
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36304433"
---
# <a name="azure-cosmos-db-bulk-executor-library-overview"></a>Azure Cosmos DB toplu Yürütücü kitaplığına genel bakış
 
Azure Cosmos DB üzere özellikler esnek ölçeklendirme desteklemek üzere tasarlanmış bir hızlı, esnek ve genel olarak dağıtılmış veritabanı hizmetidir.: 

* Büyük okuma ve kapatma (saniye başına milyonlarca) yazma.  
* Yüksek hacim rakamlarına (yüzlerce terabayt ya da daha da) depolama tahmin edilebilir milisaniyelik gecikme süresi ile işlem ve işletimsel verileri.  

Toplu Yürütücü kitaplığı, bu çok büyük verim ve depolama yararlanan yardımcı olur, toplu Yürütücü kitaplığı toplu işlemleri yapmanıza olanak tanır Azure Cosmos DB işlemlerinde toplu içeri aktarma ve toplu güncelleştirme API'leri. Daha fazla bilgiyi aşağıdaki bölümlerdeki toplu Yürütücü kitaplığı özellikleri hakkında. 

> [!NOTE] 
> Şu anda içeri aktarma toplu Yürütücü kitaplığı destekler ve bu kitaplığa ve güncelleştirme işlemleri yalnızca Azure Cosmos DB SQL API hesapları tarafından desteklenir. Bkz: [.NET](sql-api-sdk-bulk-executor-dot-net.md) ve [Java](sql-api-sdk-bulk-executor-java.md) kitaplığına herhangi bir güncelleştirme için sürüm notları.
 
## <a name="key-features-of-the-bulk-executor-library"></a>Toplu Yürütücü kitaplığın temel özellikleri  
 
* Ayrıca bir kapsayıcıya ayrılmış işleme saturate için gerekli olan istemci tarafı işlem kaynaklarını önemli ölçüde azaltır. Verileri kullanarak toplu içeri aktarma API'si 10 başarır yazan tek iş parçacıklı uygulama verileri paralel olarak istemci kapasitesini doldurmaktan sırasında makinenin CPU Yazar çok iş parçacıklı bir uygulamaya kıyasla daha büyük yazma üretimi zaman.  

* Hemen verimli bir şekilde bunları kitaplığın içinden işleyerek istek azaltma, istek zaman aşımları ve diğer geçici özel durumları işlemek için uygulama mantığını yazma can sıkıcı görevleri de soyutlar.  

* Ölçeği genişletme uygulamalar toplu işlemleri gerçekleştirmek için basitleştirilmiş bir mekanizma sağlar. Bir Azure VM üzerinde çalışan tek toplu Yürütücü örneği 500 K RU/s büyük tüketebileceği ve sanal makineleri tek tek istemcide ek örnekleri ekleyerek daha büyük verimi elde edebilirsiniz.  
 
* Bunu terabayt verilerin birden çok alma bir saat içinde genişleme mimarisi kullanarak toplu düzenleyebilirsiniz.  

* Güncelleştirme mevcut verileri Azure Cosmos DB kapsayıcılarında düzeltme ekleri olarak toplu düzenleyebilirsiniz. 
 
## <a name="how-does-the-bulk-executor-operate"></a>Toplu Yürütücü nasıl çalışır? 

Toplu işlem varlık ile içe aktarmak ya da belgeleri güncelleştirmek için bir toplu işlemin tetiklendiğinde bunların başlangıçta kendi Azure Cosmos DB bölüm anahtarı aralığının karşılık gelen demet içine karışık. Bir bölüm anahtarı aralığının karşılık gelen her demet içinde bunlar mini toplu ve her kısa toplu act halinde sunucu tarafında kaydedilen bir yükü olarak bölünür. Toplu Yürütücü kitaplığı iyileştirmeler bu mini-toplu hem içinde hem de bölüm anahtar aralıklarına üzerinden eşzamanlı yürütülmesi için yerleşik. Aşağıdaki görüntüde nasıl toplu Yürütücü veri farklı bölüm anahtarlarının toplu işlemleri gösterilmektedir:  

![Toplu Yürütücü mimarisi](./media/bulk-executor-overview/bulk-executor-architecture.png)

Toplu Yürütücü kitaplığı bir koleksiyona ayrılmış işleme maximally kullanmaya emin olur. Kullandığı bir [AIMD stili Tıkanıklık denetimi mekanizmasını](https://tools.ietf.org/html/rfc5681) her Azure Cosmos DB azaltma verimli bir şekilde işlemek için anahtar aralık ve zaman aşımlarına bölüm. 

## <a name="next-steps"></a>Sonraki Adımlar 
  
* Toplu Yürütücü Kitaplığı'nda kullanan örnek uygulamaları çıkışı deneyerek daha fazla bilgi edinin [.NET](bulk-executor-dot-net.md) ve [Java](bulk-executor-java.md).  
* Toplu Yürütücü SDK bilgileri ve sürüm notları kullanıma [.NET](sql-api-sdk-bulk-executor-dot-net.md) ve [Java](sql-api-sdk-bulk-executor-java.md).
* Toplu Yürütücü kitaplığı Cosmos DB Spark bağlayıcısına tümleşik, daha fazla bilgi için bkz: [Azure Cosmos DB Spark bağlayıcı](spark-connector.md) makalesi.  
* Toplu Yürütücü kitaplığı da yeni bir sürümüne tümleşik [Azure Cosmos DB bağlayıcı](https://aka.ms/bulkexecutor-adf-v2) verileri kopyalamak Azure Data Factory için.
