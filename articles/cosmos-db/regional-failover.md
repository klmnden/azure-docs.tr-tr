---
title: Bölgesel yük devretme Azure Cosmos veritabanı | Microsoft Docs
description: Azure Cosmos DB ile nasıl elle ve otomatik yük devretme çalıştığı hakkında bilgi edinin.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/27/2018
ms.author: sngun
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 947ecb2e6cd122ad98429db93e43b2b5c57744b7
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34614008"
---
# <a name="automatic-regional-failover-for-business-continuity-in-azure-cosmos-db"></a>İş sürekliliği Azure Cosmos veritabanı için bölgesel otomatik yük devretme
Azure Cosmos DB basitleştirir verilerin genel dağıtım sunarak tam olarak yönetilen, [bölgeli veritabanı hesaplarını](distribute-data-globally.md) tutarlılık, kullanılabilirlik ve karşılık gelen tüm ile performans arasında NET bileşim sağlayın güvence altına alır. Cosmos DB hesapları teklif yüksek kullanılabilirlik, tek bir basamak ms gecikme [iyi tanımlanmış tutarlılık düzeylerini](consistency-levels.md), çok girişli API'leri ile bölgesel saydam yük devretme ve Özellikler esnek işleme ve depolama genelinde ölçeklenme olanağı Dünya. 

Cosmos DB açık destekler ve yük devretme işlemlerini güdümlü İlkesi, hataları durumunda uçtan uca sistem davranışını denetlemek için izin verir. Bu makalede ele:

* El ile yük devretme işlemlerini Cosmos DB'de nasıl çalışır?
* Nasıl Cosmos DB otomatik yük devretme iş ve veri olduğunda neler gider aşağı center?
* Nasıl el ile yük devretme işlemlerini uygulama mimarilerinde kullanabilir miyim?

Bölgesel yük devretme dahil olmak üzere genel dağıtım özelliklerini gösteren Azure Cosmos DB Program Yöneticisi Barış Liu tarafından bu videoda bölgesel yük devretmeler hakkında bilgi edinebilirsiniz.

>[!VIDEO https://www.youtube.com/embed/1D06yjTVxt8]
>

## <a id="ConfigureMultiRegionApplications"></a>Bölgeli uygulamaları yapılandırma
Biz yük devretme modları dalın önce bölgeli kullanılabilirlik yararlanabilir ve bölgesel yük devretme işlemlerini karşısında dayanıklı olmasını bir uygulama nasıl yapılandırabileceğiniz en arayın.

* İlk olarak, birden çok bölgeye uygulamanızda dağıtma
* Uygulamanın dağıtıldığı her bölgesinden düşük gecikmeli erişim sağlamak için ilgili yapılandırma [tercih edilen bölge listesi](https://msdn.microsoft.com/library/microsoft.azure.documents.client.connectionpolicy.preferredlocations.aspx#P:Microsoft.Azure.Documents.Client.ConnectionPolicy.PreferredLocations) desteklenen Sdk'lardan birini aracılığıyla her bölge için.

Aşağıdaki kod parçacığında bölgeli uygulama başlatma gösterilmektedir. Burada, Azure Cosmos DB hesabı `contoso.documents.azure.com` iki bölgede ile - Batı ABD ve Kuzey Avrupa yapılandırılır. 

* Uygulama (örneğin Azure App Services kullanarak) Batı ABD bölgesinde dağıtılır 
* İle yapılandırılmış `West US` düşük gecikme süresi için ilk tercih edilen bölge olarak okur
* İle yapılandırılmış `North Europe` ikinci tercih edilen bölge (için yüksek kullanılabilirlik bölgesel arızalara sırasında) olarak

SQL API, bu yapılandırma aşağıdaki kod parçacığını gibi görünür:

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

Uygulamayı da tercih edilen bölgeleri ters sırasını ile Kuzey Avrupa bölgede dağıtılmış. Diğer bir deyişle, Kuzey Avrupa bölge ilk düşük gecikme süresi okuma için belirtilir. Ardından, Batı ABD bölgesi olarak yüksek kullanılabilirlik için ikinci tercih edilen bölge bölgesel hataları sırasında belirtilir.

Aşağıdaki mimarisi diyagramı Cosmos DB ve uygulama dört Azure coğrafi bölgelerde kullanılabilir olması için yapılandırıldığı bir bölgeli uygulama dağıtımı gösterir.  

![Genel olarak dağıtılmış uygulama dağıtımı Azure Cosmos DB ile](./media/regional-failover/app-deployment.png)

Şimdi, Cosmos DB hizmetiyle otomatik yük devretme işlemlerini aracılığıyla bölgesel arızalara nasıl işlediğini konumundaki bakalım. 

## <a id="AutomaticFailovers"></a>Otomatik Yük devretme
Bir Azure bölgesel kesinti veya veri merkezi kesintisinden ender olayda Cosmos DB etkilenen bölgede bir varlığı tüm Cosmos DB hesaplarıyla yük otomatik olarak tetikler. 

**Okuma bölge kesinti varsa ne olur?**

Cosmos DB etkilenen bölgelerinden okuma bir bölgede hesaplarıyla otomatik olarak kendi yazma bölgesinden bağlantısı kesilen ve çevrimdışı olarak işaretli. Cosmos DB SDK'ları bir bölge kullanılabilir olduğunda ve yeniden yönlendirme tercih edilen bölge listesinde sonraki kullanılabilir bölge çağrıları okuma otomatik olarak algıla olanak veren bir bölgesel bulma protokolünü uygular. Tercih edilen bölge listesi bölgelerde hiçbiri kullanılabilir durumdaysa çağrıları otomatik olarak geçerli yazma bölgesine geri. Değişiklik uygulama kodunuzda bölgesel yük devretme işlemlerini işlemek için gerekli değildir. Tüm bu işlem sırasında tutarlılık Cosmos DB tarafından ilkelere geçin.

![Azure Cosmos veritabanı okuma bölge hataları](./media/regional-failover/read-region-failures.png)

Etkilenen bölge kesintisi kurtarır sonra etkilenen tüm Cosmos DB hesapların bulunduğu bölgede hizmeti tarafından otomatik olarak kurtarılır. Etkilenen bölgede okuma bölge vardı cosmos DB hesapları sonra otomatik olarak geçerli yazma bölge ile eşitleme ve çevrimiçi Aç. Cosmos DB SDK'ları yeni bölge kullanılabilirliğini bulmak ve uygulama tarafından yapılandırılmış tercih edilen bölge listesi göre geçerli okuma bölge olarak bölge seçili olup olmadığını değerlendirin. Sonraki Okuma, uygulama kodunuzda herhangi bir değişiklik gerektirmeden kurtarılan bölgesine yönlendirilir.

**Bir yazma bölge kesinti varsa ne olur?**

Etkilenen bölge geçerli yazma bölgedir ve otomatik yük devretme için Azure Cosmos DB hesabı etkinleştirildiğinde, ardından bölge otomatik olarak çevrimdışı olarak işaretlenir. Ardından, alternatif bir bölge etkilenen Azure Cosmos DB hesabı için yazma bölge olarak yükseltilir. Otomatik Yük devretme etkinleştirin ve tam olarak Azure Portalı aracılığıyla Azure Cosmos DB hesaplarınız için bölge seçimi sırasını denetlemek veya [program aracılığıyla](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/databaseaccounts#DatabaseAccounts_FailoverPriorityChange). 

![Azure Cosmos DB için yük devretme öncelikler](./media/regional-failover/failover-priorities.png)

Otomatik Yük devretme sırasındaki sonraki yazma bölge belirtilen öncelik sırasına göre belirli bir Azure Cosmos DB hesabı için Azure Cosmos DB otomatik olarak seçer. Uygulamalar yazma bölgede değişikliği saptamak için DocumentClient sınıfının WriteEndpoint özelliğini kullanabilirsiniz.

![Azure Cosmos DB'de yazma bölge hataları](./media/regional-failover/write-region-failures.png)

Etkilenen bölge kesintisi kurtarır sonra etkilenen tüm Cosmos DB hesapların bulunduğu bölgede hizmeti tarafından otomatik olarak kurtarılır. 

* Bölgeler sırasında kesinti okumak için yinelenemedi önceki yazma bölgede mevcut verileri akış çakışma yayımlanır. Uygulamaları çakışması akış okuyun, uygulama belirli mantığına göre çakışmalarını çözme ve güncelleştirilen verileri Azure Cosmos DB hesabı uygun olarak geri yazma. 
* Önceki yazma bölge okuma bir bölgede yeniden ve otomatik olarak yeniden çevrimiçi duruma. 
* Yeniden çevrimiçi otomatik yazma bölge Azure Portalı aracılığıyla el ile bir yük devretme gerçekleştirerek olana okuma bölge yapılandırabilirsiniz ya da [program aracılığıyla](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/databaseaccounts#DatabaseAccounts_CreateOrUpdate).

Aşağıdaki kod parçacığında, etkilenen bölge kesintisi kurtarıldıktan sonra çakışmaları işlemek üzere verilmektedir.

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

## <a id="ManualFailovers"></a>El ile yük devretme

Otomatik Yük devretme işlemlerini ek olarak, geçerli yazma bölge belirtilen Cosmos DB hesabının el ile dinamik olarak mevcut okuma bölgeler biri olarak değiştirilebilir. El ile yük devretme işlemlerini Azure portalı üzerinden başlatılabilir veya [program aracılığıyla](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/databaseaccounts#DatabaseAccounts_CreateOrUpdate). 

El ile yük devretme sağlamak **sıfır veri kaybı** ve **sıfır kullanılabilirlik** kaybına ve düzgün biçimde aktarımı yazma eski durumundan belirtilen Cosmos DB hesap için yeni bir bölge yazma. Gibi otomatik yük devretme Cosmos DB SDK'yı otomatik olarak yazma bölge değişiklikleri el ile yük devretme işlemleri sırasında işler ve çağrıları otomatik olarak yeni yazma bölgesi yönlendirilir sağlar. Kod veya yapılandırma değişiklik yük devretme işlemlerini yönetmek için uygulamanızda gerekli değildir. 

![El ile yük devretme işlemlerini Azure Cosmos veritabanı](./media/regional-failover/manual-failovers.png)

Burada el ile yük devretme yararlı olabilir yaygın senaryolardan bazıları şunlardır:

**Saat modelini izler**: uygulamalarınızı günün saati esas alarak tahmin edilebilir trafik düzenlerini varsa, düzenli aralıklarla yazma durumu günün zamana dayalı en aktif coğrafi bölge değiştirebilirsiniz.

**Hizmet Güncelleştirmesi**: belirli genel dağıtılmış uygulama dağıtımı trafiği trafik Yöneticisi aracılığıyla farklı bir bölge için planlanan hizmet güncelleştirmesi sırasında yeniden yönlendirilmesine gerektirebilir. Artık böyle uygulama dağıtım bir bölgeye yazma durumu tutmak için el ile yük devretme kullanabilirsiniz var olduğuna Burada farklı hizmet güncelleştirme penceresi sırasında etkin trafiği olması.

**İş devamlılığı ve olağanüstü durum kurtarma (BCDR) ve yüksek kullanılabilirlik ve olağanüstü durum kurtarma (HADR) ayrıntısına**: çoğu kuruluş uygulamaları kendi geliştirme ve sürüm işleminin bir parçası iş sürekliliği testleri içerir. BCDR ve HADR sınama genellikle önemli bir adımdır uyumluluk sertifikaları ve bölgesel kesintiler durumunda hizmet kullanılabilirlik güvence altına alır. Cosmos DB depolama için el ile bir yük devretme Cosmos DB hesabınızın tetikleme ve/veya ekleme ve bir bölge dinamik olarak kaldırma kullanan uygulamalar BCDR hazırlık test edebilirsiniz.

Bu makalede, Cosmos DB nasıl elle ve otomatik yük devretme işlemlerini işlerinde ve Cosmos DB hesaplar ve genel olarak kullanılabilir olması için uygulamaları nasıl yapılandıracağınızı gözden. Cosmos DB'ın genel çoğaltma desteği kullanarak, uçtan uca gecikme geliştirmek ve hatta bölge hataları durumunda yüksek oranda kullanılabilir olduğundan emin olun. 

## <a id="NextSteps"></a>Sonraki adımlar
* Cosmos DB nasıl desteklediği hakkında bilgi edinin [genel dağıtım](distribute-data-globally.md)
* Hakkında bilgi edinin [Azure Cosmos DB ile genel tutarlılık](consistency-levels.md)
* Azure Cosmos veritabanı kullanarak birden fazla bölge ile geliştirme [SQL API](tutorial-global-distribution-sql-api.md)
* Oluşturmayı öğrenin [bölgeli yazan mimarileri](multi-region-writers.md) Azure Cosmos DB ile

