---
title: BizTalk Services hizmetinde azaltma hakkında bilgi edinin | Microsoft Docs
description: Çalışma zamanı davranışları için BizTalk Hizmetleri kaynaklanan ve azaltma eşikleri hakkında bilgi edinin. Azaltma, bellek kullanımı ve iletilerin sayısını temel alır. MABS, WABS
services: biztalk-services
documentationcenter: ''
author: MandiOhlinger
manager: anneta
editor: ''
ms.assetid: f6663cf2-cda4-4bac-855e-27d2ad5c4fa4
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: daab61a0ea9321b0fb918c60688215c80088e0bc
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51243360"
---
# <a name="biztalk-services-throttling"></a>BizTalk Services: Azaltma

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Azure BizTalk Hizmetleri uygulayan iki koşullara göre hizmet azaltma: bellek kullanımı ve iletileri eşzamanlı işlem sayısı. Bu konuda kısıtlama eşikleri listeler ve bir azalma durumu oluştuğunda, çalışma zamanı davranışını tanımlar.

## <a name="throttling-thresholds"></a>Azalan eşikler
Kaynak azaltma ve eşikleri aşağıdaki tabloda listelenmektedir:

|  | Açıklama | Düşük eşik | Yüksek eşik |
| --- | --- | --- | --- |
| Bellek |Toplam sistem belleği yok/PageFileBytes yüzdesi. <p><p>Toplam kullanılabilir PageFileBytes yaklaşık 2 katı RAM sisteminin olur. |60% |%70 |
| İleti işleme |Aynı anda işlenen ileti sayısı |40 * çekirdek sayısı |100 * çekirdek sayısı |

Bir üst Eşiğe ulaşıldığında, Azure BizTalk Services kısıtlama başlar. Düşük eşik ulaşıldığında durur azaltma. Örneğin, hizmetiniz % 65 sistem bellek kullanıyor. Bu durumda hizmeti kısıtlamak değil. Hizmetiniz, % 70'in sistem belleği kullanarak başlatır. Bu durumda hizmet kısıtlar ve % 60'a (düşük eşik) sistem belleği hizmetin kullandığı kadar kısıtlama devam eder.

Azure BizTalk Services azaltma durumunu (daraltılmış durumu ve normal durumu) ve azaltma süresi izler.

## <a name="runtime-behavior"></a>Çalışma zamanı davranışı
Azure BizTalk Services bir azaltma durumuna girdiğinde, aşağıdakiler gerçekleşir:

* Azaltma rol örneğidir. Örneğin:<br/>
  RoleInstanceA azaltma. RoleInstanceB azaltma değil. Bu durumda, RoleInstanceB iletilerinde beklenen şekilde işlenir. RoleInstanceA iletilerinde atılır ve şu hata ile başarısız:<br/><br/>
  **Sunucu meşgul. Lütfen yeniden deneyin.**<br/><br/>
* Herhangi bir çekme kaynağı değil yoklama veya bir ileti indirin. Örneğin:<br/>
  Bir işlem hattı iletileri bir dış FTP kaynağından çeker. Azaltma bir duruma çekme yapılması rol örneğini alır. Rol örneği'azaltma durdurur kadar ek iletiler indiriliyor, bu durumda, işlem hattı durdurur.
* İstemciye ileti yeniden gönderebilirsiniz, böylece bir yanıtı istemciye gönderilir.
* Azaltma Sorun çözülene kadar beklemeniz gerekir. Özellikle, düşük eşik ulaşılana kadar beklemelisiniz.

## <a name="important-notes"></a>Önemli Notlar
* Azaltma devre dışı bırakılamaz.
* Azalan eşikler değiştirilemez.
* Azaltma, sistem genelinde uygulanır.
* Azure SQL veritabanı sunucusu, yerleşik azaltma de vardır.

## <a name="additional-azure-biztalk-services-topics"></a>Ek Azure BizTalk Services konuları
* [Azure BizTalk Hizmetleri SDK'sını yükleme](https://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
* [Öğretici: Azure BizTalk Hizmetleri](https://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
* [Azure BizTalk Services SDK'sını Kullanmaya Nasıl Başlarım](https://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [Azure BizTalk Hizmetleri](https://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a>Ayrıca Bkz.
* [BizTalk Services: Geliştirici, temel, standart ve Premium sürümler grafiği](https://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [BizTalk Services: Durum Grafiğini hazırlama](https://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
* [BizTalk Services: Pano, İzleyici ve Ölçek sekmeleri](https://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [BizTalk Services: Yedekleme ve Geri Yükleme](https://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [BizTalk Services: Verenin Adı ve Verenin Anahtarı](https://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>

