---
title: Azure Cosmos DB'de bölgesel yük devretme | Microsoft Docs
description: Azure Cosmos DB ile nasıl el ile ve otomatik yük devretme çalıştığı hakkında bilgi edinin.
services: cosmos-db
author: kanshiG
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/27/2018
ms.author: govindk
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 697be3a1eb07b2f2650f3dd94fd835b9431aec6b
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2018
ms.locfileid: "42057611"
---
# <a name="automatic-regional-failover-for-business-continuity-in-azure-cosmos-db"></a>İş sürekliliği için Azure Cosmos DB, otomatik bölgesel yük devretme
Azure Cosmos DB basitleştirir genel dağılımını veri sunarak, tümüyle yönetilen [çoklu bölge veritabanı hesapları](distribute-data-globally.md) tutarlılık, kullanılabilirlik ve performans, karşılık gelen tüm ile NET bir denge sağlar garanti eder. Cosmos DB hesapları, tek basamağı ms gecikme, yüksek kullanılabilirlik sunmak [iyi tanımlanmış tutarlılık düzeyi](consistency-levels.md), çok girişli API'ler ile şeffaf bölgesel yük devretme ve aktarım hızını ve depolamayı esnek bir şekilde ölçeklendirme olanağı Dünya. 

Cosmos DB açık hem de destekler ve ilke temelli yük devretmeleri, hataları durumunda uçtan uca sistem davranışını denetlemek için izin verir. Bu makalede, şu konuları:

* El ile yük devretmeleri Cosmos DB'de nasıl çalışır?
* Nasıl Cosmos DB'de otomatik yük devretme iş ve bir veri ne gider aşağı center?
* Nasıl el ile yük devretme işlemleri uygulama mimarilerinde kullanabilir miyim?

Bölgesel yük devretme dahil olmak üzere küresel dağıtım özelliklerin Azure Cosmos DB Program Yöneticisi Manager Andrew Liu tarafından bu videoda bölgesel yük devretme işlemleri hakkında bilgi edinebilirsiniz.

>[!VIDEO https://www.youtube.com/embed/1D06yjTVxt8]
>

## <a id="ConfigureMultiRegionApplications"></a>Çok bölgeli uygulamaları yapılandırma
Biz yük devretme modu ayrıntılarına geçmeden önce çok bölgeli kullanılabilirlik avantajlarından yararlanın ve bölgesel yük devretme karşılaşıldığında dayanıklı olması için bir uygulama nasıl yapılandırabileceğiniz en arayın.

* İlk olarak, uygulamanızın birden çok bölgede dağıtma
* Uygulamanızın dağıtıldığı her bölgedeki düşük gecikme süreli erişim sağlamak için karşılık gelen yapılandırma [tercih edilen bölge listesi](https://msdn.microsoft.com/library/microsoft.azure.documents.client.connectionpolicy.preferredlocations.aspx#P:Microsoft.Azure.Documents.Client.ConnectionPolicy.PreferredLocations) desteklenen Sdk'lardan birini aracılığıyla her bölge için.

Aşağıdaki kod parçacığı, çok bölgeli bir uygulama başlatma işlemi gösterilmektedir. Burada, Azure Cosmos DB hesabı `contoso.documents.azure.com` iki bölgeleri ile - Batı ABD ve Kuzey Avrupa yapılandırılır. 

* Uygulama (örneğin Azure uygulama hizmetleri kullanarak) Batı ABD bölgesinde dağıtılır 
* İle yapılandırılmış `West US` düşük gecikme süresi için birinci tercih edilen bölge olarak okur.
* İle yapılandırılmış `North Europe` olarak ikinci tercih edilen bir bölgeye (için bölgesel hatalar sırasında yüksek kullanılabilirlik)

SQL API'si, bu yapılandırma aşağıdaki kod parçacığı gibi görünür:

```cs
ConnectionPolicy usConnectionPolicy = new ConnectionPolicy 
{ 
    ConnectionMode = ConnectionMode.Direct,
    ConnectionProtocol = Protocol.Tcp
};

usConnectionPolicy.PreferredLocations.Add(LocationNames.WestUS);
usConnectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope);

DocumentClient usClient = new DocumentClient(
    new Uri("https://contosodb.documents.azure.com"),
    "<Fill your Cosmos DB account's AuthorizationKey>",
    usConnectionPolicy);
```

Uygulama Ayrıca, Kuzey Avrupa bölgesinde sırasını ters tercih edilen bölge ile dağıtılır. Diğer bir deyişle, Kuzey Avrupa bölgesinde düşük gecikme okumaları için ilk kez belirtildi. Ardından, Batı ABD bölgesi olarak yüksek kullanılabilirlik için ikinci tercih edilen bölge bölgesel hatalar sırasında belirtilir.

Aşağıdaki Mimari diyagram, Cosmos DB ile uygulama dört Azure coğrafi bölgesinde kullanılabilmesi için yapılandırıldığı bir çok bölgeli uygulama dağıtım gösterilmektedir.  

![Azure Cosmos DB ile küresel olarak dağıtılan uygulama dağıtımı](./media/regional-failover/app-deployment.png)

Şimdi Cosmos DB hizmetini bölgesel hatalar otomatik yük devretme işlemleri aracılığıyla nasıl işlediğini konumunda göz atalım. 

## <a id="AutomaticFailovers"></a>Otomatik Yük devretme işlemleri
Azure bölgesel kesinti veya veri merkezi arızasına ender olayda, Cosmos DB, otomatik olarak yük devretmeleri etkilenen bölgede bulunan tüm Cosmos DB hesapları tetikler. 

**Kesinti okuma bölgesi varsa ne olur?**

Cosmos DB hesapları etkilenen bölgelerinden birini salt okunur bir bölgede ile otomatik olarak kendi yazma bölgesinden bağlantısı kesildi ve çevrimdışı olarak işaretlenmiş. Cosmos DB SDK'ları, bir bölgede kullanılabilir olduğunda ve yeniden yönlendirme tercih edilen bölge listesindeki bir sonraki kullanılabilir bölgeye çağrıları okuma otomatik olarak algıla izin veren bir bölgesel bulma protokolünü uygular. Tercih edilen bölge listesi bölgelerde hiçbiri kullanılabilir haldeyse, çağrıları otomatik olarak geçerli yazma bölgesine dönmesi. Değişiklik uygulama kodunuzda bölgesel yük devretme işlemleri işlemek için gerekli değildir. Bu işlemi sırasında tutarlılık garantisi, Cosmos DB tarafından kabul geçin.

![Azure Cosmos DB'de okuma bölgesi hataları](./media/regional-failover/read-region-failures.png)

Etkilenen bölge kesintisinden kurtarır sonra etkilenen tüm Cosmos DB hesapları bölgede hizmet tarafından otomatik olarak kurtarılır. Etkilenen bölgede okuma bölgesi olan bir cosmos DB hesapları sonra otomatik olarak geçerli yazma bölgesine ile eşitleme ve çevrimiçi açın. Cosmos DB SDK'ları, yeni bölge kullanılabilirliğini keşfedin ve uygulama tarafından yapılandırılan tercih edilen bölge listesinden göre geçerli okuma bölgesi olarak bir bölge seçilmelidir olup olmadığını değerlendirin. Sonraki Okuma, uygulama kodunuz için herhangi bir değişikliğe gerek kalmadan kurtarılan bölgeye yönlendirilir.

**Yazma bölgesinde kesinti yaşandığında ne olur?**

Geçerli yazma bölgesine etkilenen bölgedir ve otomatik yük devretme Azure Cosmos DB hesabı için etkinleştirilir, ardından bölgeye otomatik olarak çevrimdışı olarak işaretlenir. Ardından, alternatif bölgelere etkilenen Azure Cosmos DB hesabı için yazma bölgesi olarak yükseltilir. Otomatik yük devretmeyi etkinleştirmek ve Azure portalı, Azure Cosmos DB hesapları için bölge seçim sırası tam olarak denetlemek veya [program aracılığıyla](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/databaseaccounts#DatabaseAccounts_FailoverPriorityChange). 

![Azure Cosmos DB için yük devretme önceliklerini](./media/regional-failover/failover-priorities.png)

Otomatik Yük devretme sırasında Azure Cosmos DB, sonraki yazma bölgesi belirtilen öncelikli sıraya göre belirli bir Azure Cosmos DB hesabı için otomatik olarak seçer. Uygulamalar yazma bölgesinde değişikliği algılamak için DocumentClient sınıfının WriteEndpoint özelliğini kullanabilirsiniz.

![Azure Cosmos DB'de yazma bölgesi hataları](./media/regional-failover/write-region-failures.png)

Etkilenen bölge kesintisinden kurtarır sonra etkilenen tüm Cosmos DB hesapları bölgede hizmet tarafından otomatik olarak kurtarılır. 

* Kesinti sırasında okuma bölgeleri yinelenemedi önceki yazma bölgesine bulunan verileri, akışı bir çakışma yayımlanır. Uygulamaları çakışma akışı okuma, belirli uygulama mantığına göre çakışmaları çözün ve güncelleştirilmiş veriler Azure Cosmos DB hesabı uygun olarak geri yazma. 
* Önceki yazma bölgesine bir okuma bölgesi yeniden ve otomatik olarak yeniden çevrimiçi duruma. 
* Tekrar çevrimiçi Azure Portalı aracılığıyla el ile bir yük devretme gerçekleştirerek yazma bölgesi otomatik olarak gündeme okuma bölgesi yeniden yapılandırabilirsiniz veya [program aracılığıyla](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/databaseaccounts#DatabaseAccounts_CreateOrUpdate).

Aşağıdaki kod parçacığı, etkilenen bölge kesintiden kurtarıldıktan sonra çakışmaları işlemek üzere verilmektedir.

```cs
string conflictsFeedContinuationToken = null;
do
{
    FeedResponse<Conflict> conflictsFeed = client.ReadConflictFeedAsync(collectionLink,
        new FeedOptions { RequestContinuation = conflictsFeedContinuationToken }).Result;

    foreach (Conflict conflict in conflictsFeed)
    {
        Document doc = conflict.GetResource<Document>();
        Console.WriteLine("Conflict record ResourceId = {0} ResourceType= {1}", conflict.ResourceId, conflict.ResourceType);

        // Perform application specific logic to process the conflict record / resource
    }

    conflictsFeedContinuationToken = conflictsFeed.ResponseContinuation;
} while (conflictsFeedContinuationToken != null);
```

## <a id="ManualFailovers"></a>El ile yük devretme işlemleri

Otomatik Yük devretme işlemleri ek olarak, belirli bir Cosmos DB hesabının geçerli yazma bölgesine el ile dinamik olarak mevcut okuma bölgelerinden birine değiştirilebilir. El ile yük devretme işlemleri Azure portalı üzerinden başlatılabilir veya [program aracılığıyla](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/databaseaccounts#DatabaseAccounts_CreateOrUpdate). 

El ile yük devretme sağlamak **sıfır veri kaybı** ve **sıfır kullanılabilirlik** kaybı ve düzgün bir şekilde eski aktarımı yazma durumu yazma bölgesi belirtilen Cosmos DB hesabı için yeni bir tane. Gibi otomatik yük devretme Cosmos DB SDK'sı otomatik olarak el ile yük devretme sırasında yazma bölgesi değişiklikleri işleme ve çağrıları yeni yazma bölgesine otomatik olarak yönlendirilir sağlar. Yük devretme işlemleri yönetmek için uygulamanızda hiçbir kod ya da yapılandırma değişiklikleri gerekir. 

![Azure Cosmos DB'de el ile yük devretme işlemleri](./media/regional-failover/manual-failovers.png)

Burada, el ile yük devretme yararlı olabilir yaygın senaryolardan bazıları şunlardır:

**Saat modelini**: tahmin edilebilir trafik düzenlerini günün saatini temel alan uygulamalarınız varsa, düzenli aralıklarla yazma durumu günün saatini temel alan en etkin coğrafi bölgeyi değiştirebilirsiniz.

**Hizmet Güncelleştirmesi**: trafik traffic manager aracılığıyla farklı bir bölge için planlanan hizmet güncelleştirmesi sırasında yeniden yönlendirilmesine belirli Global olarak dağıtılmış bir uygulama dağıtımı gerektirebilir. Artık böyle uygulama dağıtımı bölgeye yazma durumunu korumak için el ile yük devretme kullanabilirsiniz oluşacak burada etkin trafik hizmeti güncelleştirme penceresi sırasında olması.

**İş sürekliliği ve olağanüstü durum kurtarma (BCDR) ve yüksek kullanılabilirlik ve olağanüstü durum kurtarma (HADR) tatbikatları**: çoğu kuruluş uygulamaları, geliştirme ve sürüm işleminin bir parçası iş sürekliliği testleri içerir. BCDR ve HADR test genellikle önemli bir adımdır uyumluluk sertifikaları ve bölgesel kesintilerden söz konusu olduğunda hizmet kullanılabilirliği güvence altına alır. Cosmos DB, Cosmos DB hesabınızın el ile yük devretmeyi tetiklemenin ve/veya ekleme ve bir bölgeye dinamik olarak kaldırma için depolama kullanan uygulamalarınızı BCDR hazır olup olmadığını test edebilirsiniz.

Bu makalede, Cosmos DB'de nasıl elle ve otomatik yük devretme iş ve Cosmos DB hesapları ve küresel olarak kullanılabilir olması için uygulamaları nasıl yapılandıracağınızı gözden. Cosmos DB'nin genel çoğaltma desteği kullanarak uçtan uca gecikme süresini iyileştirip ve hatta bölge hataları durumunda yüksek oranda kullanılabilir olduğundan emin olun. 

## <a id="NextSteps"></a>Sonraki adımlar
* Cosmos DB nasıl desteklediği hakkında bilgi edinin [genel dağıtım](distribute-data-globally.md)
* Hakkında bilgi edinin [Azure Cosmos DB ile genel tutarlılık](consistency-levels.md)
* Azure Cosmos DB kullanarak birden çok bölgeye ile geliştirme [SQL API'si](tutorial-global-distribution-sql-api.md)
* Nasıl oluşturacağınızı öğrenin [çok bölgeli yazıcı mimarileri](multi-region-writers.md) Azure Cosmos DB ile

