---
title: Görüntüleme ve Azure Log analytics'te veri çözümleme | Microsoft Docs
description: Bu makalede, kullanabilirsiniz portalları oluşturmak ve düzenlemek için Azure Log Analytics günlük aramaları.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: bwren
ms.component: ''
ms.openlocfilehash: 8fdf1bed0b75111abef4579565698f0c48b5d843
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2018
ms.locfileid: "52724154"
---
# <a name="viewing-and-analyzing-data-in-log-analytics"></a>Log analytics'te verileri çözümleme ve görüntüleme
Azure portalında Log analytics içinde depolanan verileri analiz etmek için ve geçici analiz sorguları oluşturmak için iki seçenek vardır. Bu portalı kullanarak oluşturduğunuz sorguları, uyarılar ve panolar gibi diğer özellikler için kullanılabilir.

## <a name="log-analytics-page"></a>Log Analytics sayfası
Log Analytics sayfasından açın **günlükleri** Log Analytics menüsünde. Günlük verileri ile çalışma ve sorgular oluşturmak için yeni bir deneyim budur. Bu portalı giriş yapın ve onun özelliklerini inceleme [Azure portalında Log Analytics sayfası ile çalışmaya başlama](query-language/get-started-analytics-portal.md).

Log Analytics sayfası üzerinde aşağıdaki iyileştirmeleri sağlar [günlük araması (Klasik)](#log-search-classic) karşılaşırsınız.

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
> Log Analytics sayfa, Azure portal dışındaki dış bir araç olan gelişmiş analiz portalını ile aynı işlevlere sahiptir. Gelişmiş analiz portalını hala kullanılabilir, ancak bağlantılar ve Azure portalında, diğer başvurular, bu yeni bir sayfa ile değiştirilmektedir.

![Gelişmiş analiz portalı](media/log-analytics-log-search-portals/advanced-analytics-portal.png)

### <a name="resource-logs"></a>Kaynak günlükleri
Yeni Log Analytics deneyimi, sanal makineler gibi çeşitli Azure kaynakları ile entegre olur. Başka bir deyişle, Azure İzleyici ya da Log Analytics değiştirme ve kaynak bağlam kaybı olmadan kaynağın izleme menüsü aracılığıyla doğrudan Log Analytics sayfasını açabilirsiniz. **Günlükleri** tüm Azure kaynakları, ancak, farklı kaynaklar için portal menüsünde türleri görünmeye başlayacak için henüz etkinleştirilmedi.

Log Analytics belirli bir kaynaktan açarken, yalnızca o kaynak kayıtlarını günlüğe kaydetmek üzere otomatik olarak kapsamlıdır.   Diğer kayıtlarını içeren bir sorgu yazmak istiyorsanız, Log Analytics veya Azure İzleyici menüsünden açmak gerekir.

Aşağıdaki seçenekler henüz Log Analytics kaynak görünümü kullanılamaz:

- Kaydet
- Uyarı ayarlama
- Sorgu gezgini
- Farklı çalışma / (şu anda değil planlanmış) kaynak için değiştirme


### <a name="firewall-requirements"></a>Güvenlik duvarı gereksinimleri
Tarayıcınız Log Analytics sayfası ve Gelişmiş analiz portalını erişmek için aşağıdaki adresi erişim gerektirir.  Tarayıcınız Azure portalında bir güvenlik duvarı üzerinden erişiyorsanız, bu adresleri erişimi etkinleştirmeniz gerekir.

| Uri | IP | Bağlantı Noktaları |
|:---|:---|:---|
| portal.loganalytics.io | Dinamik | 80,443 |
| api.loganalytics.io    | Dinamik | 80,443 |
| docs.loganalytics.io   | Dinamik | 80,443 |


## <a name="log-search-classic"></a>Günlük araması (Klasik)
Günlük arama sayfasından açın **günlükleri (Klasik)** Log Analytics menüsünde veya gelen **Log Analytics** Azure İzleyici menüsünde. Bu özellikleri ek eksik Log Analytics sorguları ile çalışmak için kullanılan Klasik sayfasıdır [Log Analytics sayfa](#log-analytics-page) yukarıda listelenen.



![Günlük arama sayfası](media/log-analytics-log-search-portals/log-search-portal.png)


## <a name="next-steps"></a>Sonraki adımlar

- İzlenecek yol bir [günlük arama özelliğini kullanarak öğretici](log-analytics-tutorial-viewdata.md) sorgu dilini kullanarak sorguları oluşturma hakkında bilgi edinmek için
- İzlenecek yol bir [Gelişmiş analiz portalını kullanarak Ders](query-language/get-started-analytics-portal.md) Log Analytics sayfası olarak aynı deneyimi sunar.

