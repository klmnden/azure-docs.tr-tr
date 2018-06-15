---
title: Azure olay hub'ları ayrılmış kapasite genel bakış | Microsoft Docs
description: Microsoft Azure olay hub'ları ayrılmış kapasite genel bakış.
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/30/2018
ms.author: sethm
ms.openlocfilehash: 7009710328c96660accdcf9c88313ad92d25d41c
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2018
ms.locfileid: "32311425"
---
# <a name="overview-of-event-hubs-dedicated"></a>Ayrılmış Event Hubs'a genel bakış

*Olay hub'ları ayrılmış* kapasite zorlu gereksinimleri olan müşteriler için tek Kiracı dağıtımları sunar. Tam ölçekli olarak Azure Event Hubs'a giriş için saniye başına ikinci veya en çok 2 GB tam dayanıklı depolama ve alt ikinci gecikme süresi ile telemetri başına 2 milyondan olay. Bu ayrıca tümleşik çözüm gerçek zamanlı işleme ve aynı sistemde toplu olarak sağlar. İle [olay hub'ları yakalama](event-hubs-capture-overview.md) Sunumda dahil, çözümünüzü karmaşıklığını gerçek zamanlı ve toplu tabanlı ardışık düzen destekleyen tek bir akış sağlayarak azaltabilir.

Aşağıdaki tabloda kullanılabilir hizmet katmanları olay hub'larını karşılaştırır. Olay hub'ları ayrılmış kullanım için standart özelliklerinin çoğu fiyatlandırma karşılaştırıldığında Sabit aylık fiyat, bir tekliftir. Ayrılmış katmanı tüm özellikleri, standart plana ancak yoğun iş yükleri sahip müşteriler için Kurumsal ölçek kapasiteli sunar. 

| Özellik | Standart | Adanmış |
| --- |:---:|:---:|:---:|
| Giriş olayları | Milyon olayları ödeme | Dahil |
| İşleme birimi (1 MB/sn giriş, çıkış 2 MB/sn) | Saat başına ödeme | Dahil |
| İleti Boyutu | 256 KB | 1 MB |
| Yayımcı ilkeleri | Evet | Evet |   
| Tüketici grupları | 20 | 20 |
| İleti yeniden yürütme | Evet | Evet |
| En fazla üretilen iş birimi | 20 (100'e esnek)   | 1 kapasite birimi (CU) ≈ 50 |
| Aracılı bağlantılar | 1.000 dahil | 100 dahil K |
| Ek Aracılı bağlantılar | Evet | Evet |
| İleti Saklama | 1 gün dahil | 7 gün dahil |
| Capture | Saat başına ödeme | Dahil |

## <a name="benefits-of-event-hubs-dedicated-capacity"></a>Olay hub'ları ayrılmış kapasite yararları

Olay hub'ları ayrılmış kullanırken aşağıdaki yararları kullanılabilir:

* Etkisiz diğer kiracıdan ile barındırma tek bir kiracı.
* Standart 256 KB karşılaştırıldığında 1 MB ileti boyutunu artırır.
* Yinelenebilir performans her zaman.
* Veri bloğu gereksinimlerinizi karşılayacak şekilde kapasiteyi garanti.
* İçeren [yakalama](event-hubs-capture-overview.md) mikro toplu ve uzun vadeli bekletme ile tümleştirme sağlamak için Event Hubs özelliğidir.
* Sıfır Bakım: Yük Dengeleme, işletim sistemi güncelleştirmeleri, güvenlik yamaları ve bölümleme hizmeti yönetir.
* Saatlik fiyatlandırma sabit.
* Bekletme ekstra ücret ödemeden ile 7 gün için yukarı iletisi.

Olay hub'ları ayrılmış bazı standart sunumun verimlilik sınırlamaları da kaldırır. Üretilen iş birimleri standart katmanındaki saniyede ikinci ya da 1 MB TU ve çift çıkış miktarı başına giriş başına 1000 olay entitle. Özel Ölçek sunumu üzerinde giriş kısıtlama yoktur ve çıkış olay sayar. Bu sınırları, yalnızca işleme kapasitesi satın alınan olay hub'ları tarafından yönetilir.

Bu ayrılmış, özel ortam gibi bu katmanı için benzersiz diğer yetenekleri sağlar:

* Ad alanları kümenizdeki sayısını denetler.
* İşleme sınırları her alanlarınızın belirtir.
* Olay hub'ları her ad alanı altında sayısını yapılandırır.
* Bölüm sayısı sınırını belirler.

Bu hizmet, en büyük telemetri kullanıcılar hedeflenen ve bir Kurumsal Anlaşma sahip müşteriler için kullanılabilir.

## <a name="how-to-onboard"></a>Nasıl onboarding için

Ekleyerek veya kaldırarak ö gereksinimlerinizi karşılayacak şekilde ay boyunca yukarı veya aşağı kapasitenizi ölçeklendirebilirsiniz. Sizin için uygun olan esnek dağıtım almak için olay hub'ları ürün ekibinin daha uygulamalı ekleme yaşar ayrılmış planı benzersizdir. Bu SKU giriş için [fatura desteği ile iletişime geçin](https://ms.portal.azure.com/#create/Microsoft.Support) veya Microsoft temsilcinizle.

## <a name="next-steps"></a>Sonraki adımlar

Olay hub'ları ayrılmış kapasitesi hakkında ek ayrıntıları almak için Microsoft satış temsilcisi veya Microsoft Support başvurun. Aynı zamanda daha fazla olay hub'ın şu bağlantıları ziyaret ederek fiyatlandırma katmanları hakkında bilgi edinebilirsiniz:

- [Olay hub'ları ayrılmış fiyatlandırma](https://azure.microsoft.com/pricing/details/event-hubs/). Olay hub'ları ayrılmış kapasitesi hakkında ek ayrıntıları almak için Microsoft satış temsilcisi veya Microsoft Support de başvurabilirsiniz.
- [Olay hub'ları SSS](event-hubs-faq.md) fiyatlandırma bilgileri içerir ve Event Hubs hakkında sık sorulan bazı sorular yanıtlanmaktadır. 
