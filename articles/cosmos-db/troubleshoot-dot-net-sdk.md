---
title: Tanılama ve Azure Cosmos DB .NET SDK'sı kullanırken sorunlarını giderme
description: İstemci tarafı günlüğe kaydetme ve tanımlamak, tanılamak ve .NET SDK kullanarak Azure Cosmos DB sorunlarını gidermek için diğer üçüncü taraf araçları gibi özellikleri kullanın.
author: j82w
ms.service: cosmos-db
ms.date: 01/19/2019
ms.author: jawilley
ms.devlang: c#
ms.subservice: cosmosdb-sql
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 7f969ab6059140ec32c9c5bf5045c546602a3c15
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60404732"
---
# <a name="diagnose-and-troubleshoot-issues-when-using-azure-cosmos-db-net-sdk"></a>Tanılama ve Azure Cosmos DB .NET SDK'sı kullanırken sorunlarını giderme
Bu makalede kullanırken yaygın sorunlar, geçici çözümler, tanılama adımları ve araçları kapsayan [.NET SDK'sı](sql-api-sdk-dotnet.md) Azure Cosmos DB SQL API hesabı olan.
.NET SDK'sı, Azure Cosmos DB SQL API'sine erişmek için istemci tarafı mantıksal gösterim sağlar. Bu makalede, araçları ve yaklaşımları herhangi bir sorunla karşılaşırsanız çalıştırırsanız yardımcı olması için açıklar.

## <a name="checklist-for-troubleshooting-issues"></a>Sorunları gidermek için Denetim listesi:
Uygulamanızı üretime geçmeden önce aşağıdaki denetim listesini göz önünde bulundurun. Denetim listesi kullanarak görebileceğiniz bazı yaygın sorunlar engeller. Bir sorun oluştuğunda, ayrıca bir kolayca tanılayabilirsiniz:

*   En günceli kullan [SDK](https://github.com/Azure/azure-cosmos-dotnet-v2/blob/master/changelog.md). Önizleme SDK'ları üretim için kullanılmamalıdır. Bu, zaten düzeltilmiş bilinen sorunlar ulaşmaktan engeller.
*   Gözden geçirme [performans ipuçları](performance-tips.md)ve önerilen uygulamaları izleyin. Bu, ölçeklendirme, gecikme süresi ve diğer performans sorunlarını önlemeye yardımcı olur.
*   Bir sorun gidermenize yardımcı olması için SDK günlüklerini etkinleştirin. Günlüğe kaydetmeyi etkinleştirme ile ilgili sorunları giderirken etkinleştirmek en iyi şekilde performansını etkileyebilir. Günlükleri etkinleştirebilirsiniz:
    *   [Oturum ölçümleri](monitor-accounts.md) Azure portalını kullanarak. Portal ölçümler sorunu, Azure Cosmos DB için karşılık gelen veya istemci tarafı ise, saptama açısından yararlı olan Azure Cosmos DB telemetri gösterilmektedir.
    *   Günlük [tanılama dize](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.resourceresponsebase.requestdiagnosticsstring?view=azure-dotnet) noktası işlemi yanıtlardan.
    *   Günlük [SQL sorgu ölçümleri](sql-api-query-metrics.md) tüm sorgu yanıtlardan 
    *   Kurulum için izleyin [SDK günlüğe kaydetme]( https://github.com/Azure/azure-cosmos-dotnet-v2/blob/master/docs/documentdb-sdk_capture_etl.md)

Bir göz atın [genel sorunlar ve çözümleri](#common-issues-workarounds) bu makaledeki bir bölüm.

Denetleyin [GitHub sorunları bölümüne](https://github.com/Azure/azure-cosmos-dotnet-v2/issues) , etkin olarak izlenir. Geçici bir çözüm ile benzer bir sorun önceden dosyalanmış denetleyin. Bir çözüm bulamadıysanız, sonra bir GitHub sorunu oluşturun. Acil sorunları için destek tık açabilirsiniz.


## <a name="common-issues-workarounds"></a>Genel sorunlar ve çözümleri

### <a name="general-suggestions"></a>Genel öneriler
* Azure Cosmos DB hesabı, mümkün olduğunda aynı Azure bölgesinde uygulamanızı çalıştırın. 
* Bağlantı/kullanılabilirlik sorunları içine, istemci makinenizde kaynağı alınamadığından çalıştırabilirsiniz. Azure Cosmos DB istemci çalışan düğümlerinde, CPU kullanımı izleme öneririz ve ölçeklendirme yukarı/out yüksek yük çalıştırıyorsanız.

### <a name="check-the-portal-metrics"></a>Portal ölçümleri denetleyin
Denetimi [portal ölçümleri](monitor-accounts.md) sorun istemci tarafından ise veya hizmeti ile bir sorun olduğunda belirlemenize yardımcı olur. Örneğin ölçümleri oranı sınırlı istekleri (HTTP durum kodu 429) yüksek oranda içerip içermediğini istek kısıtlandı anlamına sonra iade [istek oranı çok büyük] bölümü. 

### <a name="request-timeouts"></a>İstek zaman aşımı
RequestTimeout genellikle doğrudan/TCP kullanırken gerçekleşir, ancak ağ geçidi modunda sorun yapabilirsiniz. Bilinen yaygın nedenleri ve sorunun nasıl çözüleceğini öneriler şunlardır.

* Hangi gecikmeye neden ve/veya istek zaman aşımları CPU kullanımı yüksek olduğunda. Daha fazla kaynak sağlamak için konak makinesini müşteri ölçeğini veya daha fazla makine arasında yük dağıtılabilir.
* Yuva / bağlantı noktası kullanılabilirliği düşük olabilir. .NET 2.0 sürümünden önce yayımlanan SDK'larını kullanarak, Azure'da çalışan istemciler oluşabilir [Azure SNAT (PAT) bağlantı noktası tükenmesi]. Bu örneği neden her zaman en son SDK'sı sürümünü çalıştırmak için önerilir.
* Birden çok DocumentClient örneği oluşturma, bağlantı talebi ve zaman aşımı sorunları için neden olabilir. İzleyin [performans ipuçları](performance-tips.md)ve arasında sürecinin tamamını tek bir DocumentClient örneğini kullanın.
* Kullanıcılar, kendi koleksiyonları yeterince sağlanır, arka uç, istekleri kısıtlar ve istemci bu çağırana görünmesini olmadan dahili olarak yeniden dener dolayı yükseltilmiş gecikme veya istek zaman aşımı bazen görebilirsiniz. Denetleme [portal ölçümleri](monitor-accounts.md).
* Azure Cosmos DB genel sağlanan aktarım hızı fiziksel bölümler arasında eşit olarak dağıtır. İş yükü bir etkin değilse karşılaşıyor görmek için portal ölçümleri işaretleyin [bölüm anahtarı](partition-data.md). Bu toplam tüketilen verimlilik görünür olması için altında sağlanan RU (RU/sn) neden olur, ancak sağlanan aktarım hızı kullanılan tek bölüm işleme (RU/s) aşıyor. 
* Ayrıca, 2.0 SDK'sı kanal semantiği doğrudan/TCP bağlantısı ekler. Bir TCP bağlantısı, aynı anda birden çok istek için kullanılır. Bu özel durumları altında iki sorunlara neden olabilir:
    * Yüksek derecede eşzamanlılık çakışma için kanal neden olabilir.
    * Büyük istekler veya yanıtlar, head satır dışı kanalda engellemek üzere müşteri adayı ve eşzamanlılık, bile oldukça düşük bir ölçüde ile çekişmesini exacerbate.
    * Durum bu iki kategori hiçbirinde düşerse (veya yüksek CPU kullanımı şüpheli varsa), olası risk azaltmaları şunlardır:
        * Yukarı/out uygulama ölçeklendirmeyi deneyin.
        * Ayrıca, SDK'sı günlükleri aracılığıyla yakalanabilir [İzleme dinleyicisi](https://github.com/Azure/azure-cosmosdb-dotnet/blob/master/docs/documentdb-sdk_capture_etl.md) daha ayrıntılı bilgi edinmek için.

### <a name="connection-throttling"></a>Bağlantı daraltma
Bağlantı azaltma, bir konak makinesi üzerinde bir bağlantı sınırı nedeniyle oluşabilir. 2.0 yüklenmeden önce Azure'da çalışan istemciler oluşabilir [Azure SNAT (PAT) bağlantı noktası tükenmesi].

### <a name="snat"></a>Azure SNAT (PAT) bağlantı noktası tükenmesi

Uygulamanızı Azure sanal makineler üzerinde bir genel IP adresi, varsayılan olarak dağıtılırsa [Azure SNAT bağlantı noktaları](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports) VM'nizi dışında herhangi bir uç noktaya bağlantı. Sanal makineden Azure Cosmos DB uç noktasına izin verilen bağlantı sayısı tarafından sınırlı [Azure SNAT yapılandırma](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports).

 Yalnızca, sanal Makinenizin özel IP adresi vardır ve bir işlem VM'den bir genel IP adresine bağlanmaya Azure SNAT bağlantı noktaları kullanılır. Azure SNAT sınırlama önlemek için iki geçici çözüm vardır:

* Azure Cosmos DB Hizmeti uç noktanızı Azure sanal makineler sanal ağınızın alt ağa ekleyin. Daha fazla bilgi için [Azure sanal ağ hizmet uç noktaları](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview). 

    Hizmet uç noktası etkinleştirildiğinde, istekleri artık Azure Cosmos DB'ye bir genel IP ile gönderilir. Bunun yerine, sanal ağ ve alt ağ kimlik gönderilir. Bu değişiklik, genel IP'ler izin verilir, yalnızca güvenlik duvarı bırakmaları neden olabilir. Hizmet uç noktasını etkinleştirdiğinizde, bir güvenlik duvarı kullanıyorsanız, bir alt ağ için Güvenlik Duvarı'nı kullanarak eklemek [sanal ağ ACL'leri](https://docs.microsoft.com/azure/virtual-network/virtual-networks-acl).
* Azure VM için genel bir IP atayın.

### <a name="http-proxy"></a>HTTP Ara sunucusu
Bir HTTP Proxy'si kullanıyorsanız, SDK'yı yapılandırılmış bağlantı sayısını destekleyen emin `ConnectionPolicy`.
Aksi takdirde, bağlantı sorunlarını yönetmektir.

### İstek oranı çok büyük<a name="request-rate-too-large"></a>
Sağlanan aktarım hızı tüketilen işleme (RU/s) aştığından 'istek oranı çok büyük' veya hata kodu 429 isteklerinizi kısıtlanan olduğunu, gösterir. SDK belirtilen istekleri otomatik olarak yeniden dener [yeniden deneme ilkesi](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.connectionpolicy.retryoptions?view=azure-dotnet). Genellikle bu hata alırsanız, koleksiyonun aktarım hızını artırma göz önünde bulundurun. Denetleme [portalın ölçümleri](use-metrics.md) 429 hataları alıyorsanız, görmek için. Gözden geçirme, [bölüm anahtarı](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview#choose-partitionkey) depolama ve istek hacmi eşit dağıtımı içinde sonuç emin olmak için. 

### <a name="slow-query-performance"></a>Yavaş sorgu performansı
[Sorgu ölçümleri](sql-api-query-metrics.md) sorgu çoğu zaman harcadığı yerleri burada belirlemenize yardımcı olur. Sorgu ölçümlerinden ne kadar ettiğinden görebilirsiniz istemci uç vs üzerinde harcanan.
* Arka uç sorgu hızlı bir şekilde döndürülür ve istemcide büyük bir zaman harcadığı makinede yük kontrol edin. Büyük olasılıkla yeterli kaynak yok ve SDK'sı kaynakları yanıta işlemek kullanılabilir olmasını bekliyor.
* Arka uç sorgu yavaşsa deneyin [sorgu en iyi duruma getirme](optimize-cost-queries.md) ve geçerli [dizin oluşturma ilkesi](index-overview.md) 

 <!--Anchors-->
[Common issues and workarounds]: #common-issues-workarounds
[Enable client SDK logging]: #logging
[İstek oranı çok büyük]: #request-rate-too-large
[Request Timeouts]: #request-timeouts
[Azure SNAT (PAT) bağlantı noktası tükenmesi]: #snat
[Production check list]: #production-check-list


