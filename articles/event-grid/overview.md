---
title: Yayımla ve abone ol uygulama olayları - Azure Event Grid
description: Olay verileri bir kaynaktan işleyicisi Azure Event Grid ile gönderin. Olay tabanlı uygulamalar oluşturmanıza ve Azure Hizmetleri ile tümleştirme.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: overview
ms.date: 04/04/2019
ms.author: babanisa
ms.custom: seodec18
ms.openlocfilehash: 7f501bf8496d1293a45c15908d4f2b21b6ed01d2
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59045414"
---
# <a name="what-is-azure-event-grid"></a>Azure Event Grid nedir?

Azure Event Grid, olay temelli mimarilerle kolayca uygulamalar derlemenize olanak tanır. İlk olarak, abone olun ve ardından olay işleyicisi veya olay göndermek için Web kancası uç noktası vermek istediğiniz Azure kaynağını seçin. Event Grid, depolama blobları ve kaynak grupları gibi Azure hizmetlerinden gelen olaylar için yerleşik destek sunar. Event Grid, özel konular kullanarak kendi olaylarınızı oluşturmanızı da destekler. 

Belirli olayları farklı uç noktalara yönlendirmek, birden fazla uç noktaya yayın yapmak ve olaylarınızın güvenilir bir şekilde teslim edildiğinden emin olmak üzere filtreleri kullanabilirsiniz.

Azure Event Grid şu anda tüm genel bölgelerde kullanılabilir durumdadır. Henüz Azure Almanya, Azure Çin veya Azure kamu bulutlarında kullanılamıyor.

Bu makalede Azure Event Grid’e genel bir bakış sağlanmıştır. Event Grid kullanmaya başlamak istiyorsanız bkz. [Azure Event Grid ile özel olaylar oluşturma ve yönlendirme](custom-event-quickstart.md). 

![Olay ızgarası modeli kaynakları ve işleyicileri](./media/overview/functional-model.png)

Bu görüntü nasıl Event Grid kaynakları ve işleyicilerini bağlar ve desteklenen tümleştirmeler kapsamlı bir listesi değildir gösterir.

## <a name="event-sources"></a>Olay kaynakları

Kaynağın özellikleriyle ilgili tüm ayrıntılar ve ilgili makaleler için bkz. [olay kaynakları](event-sources.md). Şu anda Event Grid’e olay gönderme özelliği aşağıdaki Azure hizmetleri tarafından desteklenmektedir:

* Azure Abonelikleri (yönetim işlemleri)
* Container Kayıt Defteri
* Özel Konu Başlıkları
* Event Hubs
* IoT Hub
* Media Services
* Kaynak Grupları (yönetim işlemleri)
* Service Bus
* Depolama Blobu
* Depolama Genel-amaçlı v2 (GPv2)
* Azure Haritalar

## <a name="event-handlers"></a>Olay işleyicileri

İşleyicinin özellikleriyle ilgili tüm ayrıntılar ve ilgili makaleler için bkz. [olay işleyicileri](event-handlers.md). Şu anda Event Grid’den olay işleme özelliği aşağıdaki Azure hizmetleri tarafından desteklenmektedir: 

* Azure Otomasyonu
* Azure İşlevleri
* Event Hubs
* Karma Bağlantılar
* Logic Apps
* Microsoft Flow
* Kuyruk Depolama
* WebHooks

## <a name="concepts"></a>Kavramlar

Azure Event Grid’de başlangıç yapmanızı sağlayan beş kavram vardır:

* **Olaylar** - Ne olduğu.
* **Olay kaynakları** - Olayın gerçekleştiği yer.
* **Konu Başlıkları** - Yayımcıların olayları gönderdiği uç nokta.
* **Olay abonelikleri** -rota olayları, bazen birden fazla işleyici için uç nokta veya yerleşik mekanizması. Abonelikler ayrıca işleyiciler tarafından gelen olayları akıllıca filtrelemek için de kullanılır.
* **Olay işleyicileri** - Olaya tepki veren uygulama ya da hizmet.

Bu kavramlar hakkında daha fazla bilgi için bkz. [Azure Event Grid’de Kavramlar](concepts.md).

## <a name="capabilities"></a>Özellikler

Azure Event Grid’in önemli özelliklerinden bazıları şunlardır:

* **Basitlik** - Azure kaynağınızdan herhangi bir olay işleyici ya da uç noktaya olayları hedeflemek için üzerine gelip tıklayın.
* **Gelişmiş filtreleme** -filtre olay türü veya olay, olay işleyicileri, yalnızca ilgili olayları aldığınızdan emin olmak için yol yayımlama.
* **Yayma** -birkaç uç noktayı gerektiğinde sayıda basamağa olay kopyasını göndermek için aynı olaya abone olun.
* **Güvenilirlik** -olayları teslim edilir emin olmak için bir üstel geri alma ile 24 saat yeniden deneyin.
* **Olay başına ödeme** - Yalnızca Event Grid’i kullandığınız miktar için ödeme yapın.
* **Yüksek aktarım hızı** - Saniyede milyonlarca olay desteği ile Event Grid’de yüksek hacimli iş yükleri oluşturun.
* **Yerleşik Olaylar** - Kaynak tarafından tanımlanan yerleşik olaylarla hızlıca çalışmaya başlayın.
* **Özel Olaylar** - Event Grid’i kullanarak uygulamanızda özel olayları yönlendirin, filtreleyin ve güvenilir bir şekilde teslim edin.

Event Grid, Event Hubs ve Service Bus hizmetlerinin bir karşılaştırması için bkz. [İleti teslim eden Azure hizmetleri arasında seçim yapma](compare-messaging-services.md).

## <a name="what-can-i-do-with-event-grid"></a>Event Grid ile ne yapabilirim?

Azure Event Grid, birkaç büyük ölçüde sunucusuz geliştiren özellikler, işlem Otomasyonu sağlar ve [tümleştirme](https://azure.com/integration) çalışır: 

### <a name="serverless-application-architectures"></a>Sunucusuz uygulama mimarileri

![Sunucusuz uygulama mimarisi](./media/overview/serverless_web_app.png)

Event Grid, veri kaynaklarını ve olay işleyicilerini bağlar. Örneğin, bir blob depolama kapsayıcısına eklenen görüntülerini inceler sunucusuz bir işlev tetiklemek için Event grid'i kullanın. 

### <a name="ops-automation"></a>İşlem Otomasyonu

![İşlem Otomasyonu](./media/overview/Ops_automation.png)

Event Grid, otomasyonu hızlandırmanızı ve ilke uygulamayı basitleştirmenizi sağlar. Örneğin, bir sanal makine veya SQL veritabanı oluşturulduğunda Azure Otomasyonu bildirmek için Event grid'i kullanın. Meta veri, operasyon araçlarına, sanal makineleri etiketlemek ve iş öğelerini yerleştirin otomatik olarak hizmet yapılandırmalarının uyumlu olup olmadığını denetlemek için olayları kullanın.

### <a name="application-integration"></a>Uygulama tümleştirme

![Azure ile uygulama tümleştirme](./media/overview/app_integration.png)

Event Grid, uygulamanızı diğer hizmetlere bağlar. Örneğin, uygulamanızın olay verilerini Event Grid’e göndermek için özel bir konu başlığı oluşturup Azure ile doğrudan tümleştirme, güvenilir teslim ve gelişmiş yönlendirme özelliklerinden yararlanabilirsiniz. Ya da kod yazmadan her yerden, işlem veri Event grid'i Logic Apps ile kullanabilirsiniz. 

## <a name="how-much-does-event-grid-cost"></a>Event Grid’in maliyeti ne kadardır?

Azure Event Grid, olay başına ödeme fiyatlandırma modeli kullanır, bu nedenle yalnızca kullandığınız kadar ödeme yaparsınız. Her ay ilk 100.000 işlem ücretsizdir. İşlemler olay girişi, abonelik teslim denemeleri, yönetim çağrıları ve konu sonekine göre filtreleme olarak tanımlanır. Ayrıntılar için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/event-grid/).

## <a name="next-steps"></a>Sonraki adımlar

* [Blob Depolama olaylarını yönlendirme](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json)  
  Event Grid kullanarak depolama blobu olaylarına yanıt verin.
* [Oluşturma ve özel olaylara abone](custom-event-quickstart.md)  
  Azure Event Grid hızlı başlangıcını kullanarak hemen giriş yapın ve kendi özel olaylarınızı herhangi bir uç noktaya göndermeye başlayın.
* [Bir olay işleyicisi Logic Apps kullanarak](monitor-virtual-machine-changes-event-grid-logic-app.md)  
  Event Grid tarafından gönderilen olaylara yanıt vermek üzere Logic Apps kullanarak uygulama derleme öğreticisi.
* [Büyük verileri bir veri ambarına akışla aktarma](event-grid-event-hubs-integration.md)  
  Azure İşlevleri’ni kullanarak Event Hubs’dan SQL Veri Ambarı’na veri akışı yapan öğretici.
* [Event Grid REST API Başvurusu](/rest/api/eventgrid)  
  Olay Aboneliklerini yönetmek için başvuru içeriği sağlar Yönlendirme ve filtreleme.
