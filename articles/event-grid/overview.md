---
title: Azure olay kılavuz genel bakış
description: Azure olay kılavuz ve onun kavramlarını açıklar.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 04/27/2018
ms.author: babanisa
ms.openlocfilehash: 0be2952dc39064eaf2814806e81f16e882a6a6fe
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="an-introduction-to-azure-event-grid"></a>Azure olay kılavuzuna giriş

Azure olay kılavuz, olay tabanlı mimari ile uygulamaları kolayca oluşturmanıza olanak sağlar. Abone olma ve olay işleyicisi veya olayı göndermek için Web kancası uç noktası vermek istediğiniz Azure kaynağı seçin. Olay kılavuz depolama BLOB'ları ve kaynak grupları gibi Azure hizmetlerinden gelen olaylar için yerleşik desteğe sahiptir. Olay kılavuz özel konular ve özel Web kancalarını kullanarak uygulama ve üçüncü taraf olayları özel desteği de vardır. 

Belirli olayları farklı uç noktalar, birden çok uç nokta için çok noktaya yayın yönlendirmek ve olaylarınızı güvenilir bir şekilde teslim emin olmak için filtreleri kullanabilirsiniz. Olay kılavuz, ayrıca özel ve üçüncü taraf olayları için destek oluşturdu.

Şu anda, olay kılavuz aşağıdaki bölgeler destekler:

* Güneydoğu Asya
* Doğu Asya
* Avustralya Doğu
* Avustralya Güneydoğu
* Orta ABD
*   Doğu ABD
*   Doğu ABD 2
* Batı Avrupa
* Kuzey Avrupa
* Japonya Doğu
* Japonya Batı
*   Batı Orta ABD
*   Batı ABD
*   Batı ABD 2

Bu makalede Azure olay kılavuz genel bir bakış sağlar. Olay kılavuzla başlamak istiyorsanız, bkz: [Azure olay kılavuz oluşturma ve rota özel olaylarla](custom-event-quickstart.md). Aşağıdaki resimde kaynakları ve işleyicileri olayı kılavuz nasıl bağlanacağını gösterir, ancak desteklenen seçenekler kapsamlı bir listesini sağlamaz.

![Olay kılavuz işlevsel modeli](./media/overview/functional-model.png)

## <a name="event-sources"></a>Olay kaynakları

Şu anda aşağıdaki Azure hizmetlerini olay kılavuza gönderen olaylar destekler:

* Azure abonelikleri (yönetim işlemlerini)
* Özel konular
* Event Hubs
* IoT Hub
* Media Services
* Kaynak grupları (yönetim işlemlerini)
* Service Bus
* Depolama blobu
* Depolama genel amaçlı v2 (GPv2)

Her olay kaynağı kullanmayı gösteren makalelerinin bağlantıları için bkz: [olay kaynakları Azure olay kılavuzunda](event-sources.md).

## <a name="event-handlers"></a>Olay işleyicileri

Şu anda aşağıdaki Azure hizmetlerini olay kılavuz işleme olaylarından destekler: 

* Azure Otomasyonu
* Azure İşlevleri
* Event Hubs
* Karma Bağlantılar
* Logic Apps
* Microsoft Flow
* Kuyruk Depolama
* WebHooks

Azure işlevleri işleyici olarak kullanırken, olay kılavuz tetikleyici genel HTTP INSTEAD OF Tetikleyicileri kullanın. Event Grid, Event Grid İşlevi tetikleyicilerini otomatik olarak doğrular. Genel HTTP tetikleyicileri ile [doğrulama yanıtını](security-authentication.md#webhook-event-delivery) uygulamanız gerekir.

Her olay işleyicisi kullanmayı gösteren makalelerinin bağlantıları için bkz: [olay işleyicileri Azure olay kılavuzunda](event-handlers.md).

## <a name="concepts"></a>Kavramlar

Azure olay kılavuzunda yapmalarını sağlayan beş kavram vardır:

* **Olayları** -ne oldu.
* **Olay kaynakları/yayımcıları** - burada olay gerçekleşen.
* **Konular** -burada yayımcıları olayları göndereceği.
* **Olay abonelikleri** -rota olayları, bazen birden çok işleyici için uç nokta veya yerleşik mekanizması. Abonelikleri akıllıca gelen olayları filtrelemek için işleyiciler tarafından da kullanılır.
* **Olay işleyicileri** -uygulama veya hizmet olaya tepki verme.

Bu kavramlar hakkında daha fazla bilgi için bkz: [Azure olay kılavuzunda kavramları](concepts.md).

## <a name="capabilities"></a>Özellikler

Azure olay kılavuz önemli özelliklerinden bazıları şunlardır:

* **Basitlik** -Point'ye tıklayın, Azure kaynak olaylarından herhangi bir olay işleyicisi veya uç nokta hedeflenir.
* **Filtreleme Gelişmiş** -filtre olay türü veya olay olay işleyicileri yalnızca ilgili olayları aldığınızdan emin olmak için yol yayımlayın.
* **Yayma** -birden çok uç nokta gerektiğinde sayıda basamağa olay kopyalarını göndermesini aynı olaya abone olun.
* **Güvenilirlik** -olayları teslim edilir emin olmak için üstel geri alma 24 saatlik Denemeyle kullanma.
* **Olay başına ödeme** - ödeme tutarını yalnızca olay kılavuzunu kullanın.
* **Yüksek verimlilik** -yapı yüksek hacimli iş yükleri üzerinde olay kılavuz saniye başına milyonlarca olayı desteği.
* **Yerleşik olayları** - hale getirmek ve hızlı bir şekilde kaynak tanımlanan yerleşik olaylar ile çalışıyor.
* **Özel olaylar** -olay kılavuz rota, filtre ve güvenilir bir şekilde teslim özel olaylar uygulamanızda kullanın.

Olay kılavuz, olay hub'ları ve Service Bus karşılaştırması için bkz: [iletileri teslim Azure hizmetleri arasında seçim yapma](compare-messaging-services.md).

## <a name="what-can-i-do-with-event-grid"></a>Olay kılavuzla ne yapabilirim?

Azure olay kılavuz birkaç çok sunucusuz artırmak özellikleri, ops otomasyon ve tümleştirme çalışma sağlar: 

### <a name="serverless-application-architectures"></a>Sunucusuz uygulama mimarileri

![Sunucusuz bir uygulama](./media/overview/serverless_web_app.png)

Event Grid, veri kaynaklarını ve olay işleyicilerini bağlar. Event Grid’i örneğin bir blob depolama kapsayıcısına eklenen her yeni fotoğrafta görüntü analizini çalıştıracak sunucusuz bir işlev tetiklemek için kullanabilirsiniz. 

### <a name="ops-automation"></a>OPS Otomasyon

![Ops otomasyonu](./media/overview/Ops_automation.png)

Event Grid, otomasyonu hızlandırmanızı ve ilke uygulamayı basitleştirmenizi sağlar. Örneğin Event Grid, sanal makine oluşturulduğunda veya SQL Veritabanı hazırlandığında Azure Otomasyonu’na bilgi verebilir. Bu olaylar hizmet yapılandırmalarının uyumlu olup olmadığını otomatik olarak denetlemek, operasyon araçlarına meta verileri eklemek, sanal makineleri etiketlemek ve iş öğelerini dosyalamak için kullanılabilir.

### <a name="application-integration"></a>Uygulama tümleştirme

![Uygulama tümleştirme](./media/overview/app_integration.png)

Event Grid, uygulamanızı diğer hizmetlerle bağlar. Örneğin, olay kılavuza uygulamanızın olay verileri göndermek ve yönlendirme, Gelişmiş, güvenilir teslim yararlanmak için özel bir konu oluşturun ve Azure ile tümleştirme doğrudan. Ayrıca Event Grid’i Logic Apps ile birlikte kullanarak kod yazmadan her yerden veri işleyebilirsiniz. 

## <a name="how-much-does-event-grid-cost"></a>Nasıl olay kılavuz maliyeti nedir?

Azure olay kılavuz bir olay başına ödeme fiyatlandırma modelini kullanır, bu nedenle, yalnızca, kullanım için ücret ödersiniz. Ayda ilk 100.000 işlemleri ücretsizdir. İşlem eşleştirme, teslim girişimi ve yönetim çağrıları Gelişmiş olay giriş tanımlanır. Ayrıntılar için bkz [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/event-grid/).

## <a name="next-steps"></a>Sonraki adımlar

* [Rota depolama blobu olayları](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json)  
  Olay kılavuz kullanarak depolama blob olaylarına yanıt.
* [Oluşturma ve özel olaylarına abone olma](custom-event-quickstart.md)  
  Sağ ve Azure olay kılavuz hızlı başlangıç kullanarak herhangi bir uç nokta için özel olaylarınızı gönderme başlangıç geçin.
* [Logic Apps olay işleyici kullanma](monitor-virtual-machine-changes-event-grid-logic-app.md)  
  Olay kılavuz tarafından gönderilen olayları tepki vermek için Logic Apps kullanarak bir uygulama oluşturmaya üzerinde bir öğretici.
* [Veri ambarına büyük veri akışı yapma](event-grid-event-hubs-integration.md)  
  Event hubs SQL Data warehouse'a veri akışı Azure işlevlerini kullanan bir öğretici.
* [Olay kılavuz REST API Başvurusu](/rest/api/eventgrid)  
  Olay Aboneliklerini yönetmek için Azure olay kılavuz ve başvurusu hakkında daha fazla teknik bilgi sağlayan Yönlendirme ve filtreleme.