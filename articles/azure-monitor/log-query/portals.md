---
title: Görüntüleme ve Azure İzleyici'de günlük verilerini analiz etme | Microsoft Docs
description: Bu makalede, Azure İzleyici'de günlük sorguları oluşturup Azure portalında Log Analytics kullanarak açıklanır.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/05/2019
ms.author: bwren
ms.openlocfilehash: 0e5b9b43e528b37fd994f9131f145abadb33c53b
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59259040"
---
# <a name="viewing-and-analyzing-log-data-in-azure-monitor"></a>Görüntüleme ve Azure İzleyici'de günlük verilerini analiz etme
Log Analytics, Azure İzleyicisi'nde sorgular oluşturma ve günlük verileri ile çalışma için birincil deneyimidir. Log Analytics bağlantısı açmak **günlükleri** içinde **Azure İzleyici** menüsü. Bu portalı giriş yapın ve onun özelliklerini inceleme [Azure portalında Log Analytics ile çalışmaya başlama](get-started-portal.md).

Log Analytics, günlük sorguları ile çalışmak için aşağıdaki özellikleri sağlar.

* Birden fazla sekme – birden çok sorgularla çalışmak için ayrı sekmeler oluşturun.
* Zengin görselleştirmeler – çeşitli grafik seçenekleri.
* Geliştirilmiş IntelliSense ve dil otomatik tamamlama.
* Söz dizimi vurgulama – sorguları okunabilirliğini artırır. 
* Sorgu Gezgini – erişim kaydedilmiş sorgular ve işlevleri.
* Şema görünümü: sorguları yazma yardımcı olmak için verilerinizin yapısını inceleyin.
* Paylaşın: sorguları veya herhangi bir paylaşılan Azure panosuna Sabitle sorguları bağlantılar oluşturabilirsiniz.
* Akıllı analiz - grafiklerinizi ve nedenini hızlı bir analizini artış tanımlar.
* Sütun seçimini – sıralayın ve sorgu sonuçlarında sütun grubu.

> [!NOTE]
> Log Analytics, Azure portal dışındaki dış bir araç olan gelişmiş analiz portalını ile aynı işlevlere sahiptir. Gelişmiş analiz portalını hala kullanılabilir, ancak bağlantılar ve Azure portalında, diğer başvurular, bu yeni bir sayfa ile değiştirilmektedir.

![Log Analytics](media/portals/log-analytics.png)

## <a name="resource-logs"></a>Kaynak günlükleri
Log Analytics, sanal makineler gibi çeşitli Azure kaynakları ile entegre olur. Azure İzleyicisi'ne geçiş ve kaynak bağlam kaybı olmadan doğrudan kaynağın izleme menüsü üzerinden Log Analytics açabileceğiniz anlamına gelir. **Günlükleri** tüm Azure kaynakları, ancak, farklı kaynaklar için portal menüsünde türleri görünmeye başlayacak için henüz etkinleştirilmedi.

Log Analytics belirli bir kaynaktan açarken, yalnızca o kaynak kayıtlarını günlüğe kaydetmek üzere otomatik olarak kapsamlıdır.   Diğer kayıtlarını içeren bir sorgu yazmak istiyorsanız, Azure İzleyici Menüsü'nden açmak gerekir.

Aşağıdaki seçenekler henüz Log Analytics kaynak görünümü kullanılamaz:

- Kaydet
- Uyarı ayarlama
- Sorgu gezgini
- Farklı çalışma / (şu anda değil planlanmış) kaynak için değiştirme


## <a name="firewall-requirements"></a>Güvenlik duvarı gereksinimleri
Tarayıcınız Log Analytics'e erişmek için aşağıdaki adresi erişim gerektirir.  Tarayıcınız Azure portalında bir güvenlik duvarı üzerinden erişiyorsanız, bu adresleri erişimi etkinleştirmeniz gerekir.

| Uri | IP | Bağlantı Noktaları |
|:---|:---|:---|
| portal.loganalytics.io | Dinamik | 80,443 |
| api.loganalytics.io    | Dinamik | 80,443 |
| docs.loganalytics.io   | Dinamik | 80,443 |


## <a name="next-steps"></a>Sonraki adımlar

- İzlenecek yol bir [öğretici kullanarak Log Analytics](../../azure-monitor/log-query/get-started-portal.md).
- İzlenecek yol bir [günlük arama özelliğini kullanarak öğretici](../../azure-monitor/learn/tutorial-viewdata.md).

