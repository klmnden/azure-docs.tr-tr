---
title: Azure Event Hubs adanmış kapasite genel bakış | Microsoft Docs
description: Microsoft Azure Event Hubs adanmış kapasite genel bakış.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/30/2018
ms.author: shvija
ms.openlocfilehash: 1a7a7593e80f08296e3163e528e880f343366b8a
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "40005713"
---
# <a name="overview-of-event-hubs-dedicated"></a>Ayrılmış Event Hubs'a genel bakış

*Event Hubs adanmış* tek kiracılı dağıtımları En zorlu gereksinimler olan müşteriler için kapasiteyi sunar. Tam ölçekli olarak, Azure Event Hubs'a giriş yapabilir. tam olarak dayanıklı depolama yanı sıra bir saniyenin gecikme süresi ile telemetri ikinci veya en fazla 2 GB saniye başına 2 milyon olay. Bu tümleşik çözümler tarafından gerçek zamanlı işleme ve batch aynı sistemde sağlar. İle [Event Hubs yakalama](event-hubs-capture-overview.md) teklifine dahil edilen, çözümünüzün karmaşıklığı gerçek zamanlı ve toplu işlem tabanlı işlem hatlarını destekleyen tek bir akış sağlanarak azaltabilirsiniz.

Aşağıdaki tablo, Event Hubs kullanılabilir hizmet katmanlarını karşılaştırır. Event Hubs Dedicated çoğu standart özellikler için kullanım fiyatlarıyla karşılaştırıldığında sabit bir aylık fiyatla teklifidir. Adanmış katmanı, tüm özellikleri standart plan, ancak zorlu iş yüklerine sahip müşteriler için Kurumsal ölçekte kapasiteyle sunar. 

| Özellik | Standart | Adanmış |
| --- |:---:|:---:|:---:|
| Giriş olayları | Milyon olay başına ödeme yapın | Dahil |
| Üretilen iş birimi (1 MB/sn giriş, 2 MB/sn çıkış) | Saat başına ödeme yapın | Dahil |
| İleti Boyutu | 256 KB | 1 MB |
| Yayımcı ilkeleri | Evet | Evet |   
| Tüketici grupları | 20 | 20 |
| İleti yeniden yürütme | Evet | Evet |
| En fazla üretilen iş birimi | 20 (100 esnek)   | 1 kapasite birimi (CU) ≈ 50 |
| Aracılı bağlantılar | 1.000 dahil | 100 bin adet dahil |
| Ek Aracılı bağlantılar | Evet | Evet |
| İleti Saklama | 1 gün dahil | 7 gün dahil |
| Capture | Saat başına ödeme yapın | Dahil |

## <a name="benefits-of-event-hubs-dedicated-capacity"></a>Event Hubs adanmış kapasite avantajları

Event Hubs Dedicated kullanılırken aşağıdaki avantajlar kullanılabilir:

* Tek bir kiracı ile diğer kiracılardan etkisiz barındırma.
* Standart için 256 KB karşılaştırıldığında 1 MB ileti boyutunu artırır.
* Tekrarlanabilir bir performans her zaman.
* Veri bloğu ihtiyaçlarınızı karşılamak üzere kapasite garantisi.
* İçerir [yakalama](event-hubs-capture-overview.md) mikro toplu ve uzun süreli saklamaya sahip tümleştirmesi sağlamak için Event Hubs özelliğidir.
* Sıfır Bakım: Yük Dengeleme, işletim sistemi güncelleştirmeleri, güvenlik yamaları ve bölümleme hizmet yönetir.
* Saatlik fiyatlandırma düzeltildi.
* Ek ücret ödemeden ile 7 gün yedekleme bekletme ileti.

Event Hubs adanmış aktarım hızı sınırlarına standart teklifi bazıları da kaldırır. Üretilen iş birimleri standart katmanda, giriş işleme birimi ve çıkış miktarı çift başına saniyede ikinci ya da 1 MB başına 1000 olay entitle sağlar. Ayrılmış ölçek teklifi hakkında daha fazla giriş kısıtlama yoktur ve çıkış olay sayar. Bu sınırlar, yalnızca işlem kapasitesi satın alınan olay hub'ları tarafından yönetilir.

Bu ayrılmış, adanmış bir ortam için bu katmanı, benzersiz diğer özellikleri gibi sağlar:

* Ad alanları, kümenizdeki sayısını denetler.
* Aktarım hızı sınırlarını her ad alanlarınıza belirtir.
* Event hubs'ı her ad alanı altında sayısını yapılandırır.
* Bölüm sınırının belirler.

Bu hizmet, en büyük telemetri kullanıcılarına yönelik ve bir kurumsal anlaşması olan müşteriler için kullanılabilir.

## <a name="how-to-onboard"></a>Nasıl eklemek için

Ekleyerek veya kaldırarak cu gereksinimlerinizi karşılayacak şekilde ay boyunca yukarı veya aşağı kapasitenizi ölçeklendirebilirsiniz. Özel planı, sizin için uygun olan esnek dağıtım almak için Event Hubs ürün ekibinden daha uygulamalı bir ekleme deneyimi açısından benzersizdir. Bu SKU ekleneceği [fatura desteği ile iletişime](https://ms.portal.azure.com/#create/Microsoft.Support) ya da Microsoft temsilcinizle.

## <a name="next-steps"></a>Sonraki adımlar

Event Hubs ayrılmış kapasitesi hakkında ek ayrıntıları almak için Microsoft satış temsilcinize veya Microsoft Support başvurun. Ayrıca daha fazla fiyatlandırma katmanları aşağıdaki bağlantıları inceleyerek Event Hubs hakkında bilgi edinebilirsiniz:

- [Event Hubs Dedicated fiyatlandırması](https://azure.microsoft.com/pricing/details/event-hubs/). Event Hubs adanmış kapasite hakkında ek ayrıntıları almak için Microsoft satış temsilcinize veya Microsoft Support de başvurabilirsiniz.
- [Event Hubs SSS Sayfasındaki](event-hubs-faq.md) fiyatlandırma bilgileri içeriyor ve Event Hubs hakkında sık sorulan bazı sorular yanıtlanmaktadır. 
