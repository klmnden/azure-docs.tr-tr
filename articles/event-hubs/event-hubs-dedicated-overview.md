---
title: "Azure olay hub'ları ayrılmış kapasite genel bakış | Microsoft Docs"
description: "Microsoft Azure olay hub'ları ayrılmış kapasite genel bakış."
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/29/2017
ms.author: sethm;babanisa
ms.openlocfilehash: db8b119178de0e565b2064e9a52d5e9989d60d38
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="overview-of-event-hubs-dedicated"></a>Ayrılmış Event Hubs'a genel bakış

*Olay hub'ları ayrılmış* kapasite zorlu gereksinimleri olan müşteriler için tek Kiracı dağıtımları sunar. Tam ölçekli olarak Azure Event Hubs'a giriş için saniye başına ikinci veya en çok 2 GB tam dayanıklı depolama ve alt ikinci gecikme süresi ile telemetri başına 2 milyondan olay. Bu ayrıca tümleşik çözüm gerçek zamanlı işleme ve aynı sistemde toplu olarak sağlar. İle [olay hub'ları yakalama](event-hubs-capture-overview.md) Sunumda dahil, çözümünüzü karmaşıklığını gerçek zamanlı ve toplu tabanlı ardışık düzen destekleyen tek bir akış sağlayarak azaltabilir.

Aşağıdaki tabloda kullanılabilir hizmet katmanları olay hub'larını karşılaştırır. Olay hub'ları ayrılmış kullanım için standart özelliklerinin çoğu fiyatlandırma karşılaştırıldığında Sabit aylık fiyat, bir tekliftir. Ayrılmış katmanı standart planı ancak yoğun iş yükleri sahip müşteriler için Kurumsal ölçek kapasiteli özellikleri sunar. 

| Özellik | Standart | Adanmış |
| --- |:---:|:---:|:---:|
| Giriş olayları | Milyon olayları ödeme | Dahil |
| İşleme birimi (1 MB/sn giriş, çıkış 2 MB/sn) | Saat başına ödeme | Dahil |
| İleti Boyutu | 256 KB | 1 MB |
| Yayımcı ilkeleri | Evet | Evet |   
| Tüketici grupları | 20 | 20 |
| İleti yeniden yürütme | Evet | Evet |
| En fazla üretilen iş birimi | 20 (100'e esnek)   | 1 CU≈200 |
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
* Saniye başına en çok 2 milyon giriş olayları sağlayan ölçeklenebilir 1 ile 8 kapasite birimlerinin (CU) – arasında.
  * Ö her CU yaklaşık 200 üretilen iş birimleri (TU) denk burada sağlayabilir hub olay ayrılmış için ölçek yönetin.
* Sıfır Bakım: biz Yük Dengeleme, işletim sistemi güncelleştirmeleri, güvenlik yamaları ve bölümleme yönetin.
* Aylık fiyatlandırma sabit.

Olay hub'ları ayrılmış bazı standart sunumun verimlilik sınırlamaları da kaldırır. Üretilen iş birimleri standart katmanındaki saniyede ikinci ya da 1 MB TU ve çift çıkış miktarı başına giriş başına 1000 olay entitle. Özel Ölçek sunumu üzerinde giriş kısıtlama yoktur ve çıkış olay sayar. Bu sınırları, yalnızca işleme kapasitesi satın alınan olay hub'ları tarafından yönetilir.

Bu hizmet en büyük telemetri kullanıcılar hedeflenen ve bir Kurumsal Anlaşma sahip müşteriler için kullanılabilir.

## <a name="how-to-onboard"></a>Nasıl onboarding için

Olay hub'ları ayrılmış platform bir Kurumsal Anlaşma ö farklı boyutlarda ile sunulur. Her CU yaklaşık 200 üretilen iş birimleri denk sağlar. Ekleyerek veya kaldırarak ö gereksinimlerinizi karşılayacak şekilde ay boyunca yukarı veya aşağı kapasitenizi ölçeklendirebilirsiniz. Sizin için uygun olan esnek dağıtım almak için olay hub'ları ürün ekibinin daha uygulamalı ekleme yaşar ayrılmış planı benzersizdir. 

## <a name="next-steps"></a>Sonraki adımlar
Olay hub'ları ayrılmış kapasitesi hakkında ek ayrıntıları almak için Microsoft satış temsilcisi veya Microsoft Support başvurun. Event Hubs hakkında daha fazla bilgi aşağıdaki bağlantıları ziyaret ederek de edinebilirsiniz:

Fiyatlandırma hakkında ayrıntılı bilgi için aşağıdaki bağlantıları ziyaret edin:

- [Olay hub'ları ayrılmış fiyatlandırma](https://azure.microsoft.com/pricing/details/event-hubs/). Olay hub'ları ayrılmış kapasitesi hakkında ek ayrıntıları almak için Microsoft satış temsilcisi veya Microsoft Support de başvurabilirsiniz.
- [Olay hub'ları SSS](event-hubs-faq.md) fiyatlandırma bilgileri içerir ve Event Hubs hakkında sık sorulan bazı sorular yanıtlanmaktadır. 
