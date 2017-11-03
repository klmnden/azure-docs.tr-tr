---
title: "BizTalk Services'da azaltma hakkında bilgi edinin | Microsoft Docs"
description: "Eşikleri azaltma ve BizTalk Services için çalışma zamanı davranışlarını kaynaklanan hakkında bilgi edinin. Azaltma, bellek kullanımı ve ileti sayısını temel alır. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: f6663cf2-cda4-4bac-855e-27d2ad5c4fa4
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 145e7470bbc01c676a1fb5856c0f9a8726e667fc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="biztalk-services-throttling"></a>BizTalk Services: Azaltma

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Azure BizTalk Services uygulayan iki koşullara göre hizmet azaltma: bellek kullanımı ve işleme eşzamanlı iletilerinin sayısı. Bu konuda kısıtlama eşikleri listeler ve azaltma bir koşul oluştuğunda çalışma zamanı davranışını tanımlar.

## <a name="throttling-thresholds"></a>Daraltma eşikleri
Azaltma kaynak ve eşikleri aşağıdaki tabloda listelenmektedir:

|  | Açıklama | Düşük eşik | Yüksek eşiği |
| --- | --- | --- | --- |
| Bellek |Toplam sistem bellek kullanılabilir/PageFileBytes yüzdesi. <p><p>Toplam kullanılabilir PageFileBytes yaklaşık 2 kez RAM sisteminin olur. |60% |70% |
| İleti işleme |Aynı anda işlenen ileti sayısı |40 * çekirdek sayısı |100 * çekirdek sayısı |

Bir üst Eşiğe ulaşıldığında, Azure BizTalk Services kısıtlama başlar. Düşük Eşiğe ulaşıldığında durakları azaltma. Örneğin, hizmetiniz %65 sistem bellek kullanıyor. Bu durumda, hizmet kısıtlama yok. Hizmetinizi % 70 sistem belleği kullanarak başlatır. Bu durumda, hizmet kısıtlar ve hizmeti, % 60 (düşük eşik) sistem belleği kullanır kadar kısıtlama devam eder.

Azure BizTalk Services azaltma durumunu (daraltılmış durumu karşılaştırması normal durum) ve azaltma süresi izler.

## <a name="runtime-behavior"></a>Çalışma zamanı davranışı
Azure BizTalk Services azaltma durumuna girdiğinde, aşağıdakiler gerçekleşir:

* Azaltma rol örneğidir. Örneğin:<br/>
  RoleInstanceA azaltma. RoleInstanceB azaltma değil. Bu durumda, RoleInstanceB iletilerinde beklendiği gibi işlenir. RoleInstanceA iletilerinde atılır ve şu hatayı vererek başarısız:<br/><br/>
  **Sunucu meşgul. Lütfen yeniden deneyin.**<br/><br/>
* Herhangi bir çekme kaynağına etmeyin yoklamak veya bir ileti indirin. Örneğin:<br/>
  Ardışık iletilerin bir dış FTP kaynaktan çeker. Çekme yapılması rol örneği azaltma bir duruma alır. Bu durumda, ardışık düzen azaltma rol örneği durdurur kadar ek iletilerin indirilmesi durdurur.
* İstemci ileti yeniden gönderebilirsiniz şekilde yanıt istemciye gönderilir.
* Azaltma Sorun çözülene kadar beklemeniz gerekir. Özellikle, düşük eşik ulaşılana kadar beklemelisiniz.

## <a name="important-notes"></a>Önemli Notlar
* Azaltma devre dışı bırakılamaz.
* Daraltma eşikleri değiştirilemez.
* Azaltma, sistem genelinde uygulanır.
* Azure SQL veritabanı sunucusu, yerleşik azaltma da sahiptir.

## <a name="additional-azure-biztalk-services-topics"></a>Ek Azure BizTalk Services konuları
* [Azure BizTalk Services SDK yükleme](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
* [Öğretici: Azure BizTalk Hizmetleri](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
* [Azure BizTalk Services SDK'sını Kullanmaya Nasıl Başlarım](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [Azure BizTalk Hizmetleri](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a>Ayrıca Bkz.
* [BizTalk Services: Geliştirici, temel, standart ve Premium sürümler grafiği](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [BizTalk Services: Klasik portalı kullanarak Azure hazırlama](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [BizTalk Services: Durum Grafiğini hazırlama](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
* [BizTalk Services: Pano, İzleyici ve Ölçek sekmeleri](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [BizTalk Services: Yedekleme ve Geri Yükleme](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [BizTalk Services: Verenin Adı ve Verenin Anahtarı](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>

