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
ms.devlang: na
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: magoedte; bwren
ms.component: na
ms.openlocfilehash: 996502ffe5a31fcfa1b73dab9a041c336c4ea98f
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45602640"
---
# <a name="viewing-and-analyzing-data-in-log-analytics"></a>Log analytics'te verileri çözümleme ve görüntüleme
Azure portalında Log analytics içinde depolanan verileri analiz etmek için ve geçici analiz sorguları oluşturmak için iki seçenek vardır. Bu portalı kullanarak oluşturduğunuz sorguları, uyarılar ve panolar gibi diğer özellikler için kullanılabilir.

## <a name="log-analytics-page"></a>Log Analytics sayfası
Log Analytics sayfasından açın **günlükleri** Log Analytics menüsünde. Günlük verileri ile çalışma ve sorgular oluşturmak için yeni bir deneyim budur. Bu portalı giriş yapın ve onun özelliklerini inceleme [Azure portalında Log Analytics sayfası ile çalışmaya başlama](query-language/get-started-analytics-portal.md).

Log Analytics sayfası üzerinde aşağıdaki iyileştirmeleri sağlar [günlük araması](#log-search) karşılaşırsınız.

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


### <a name="firewall-requirements"></a>Güvenlik duvarı gereksinimleri
Tarayıcınız Log Analytics sayfası ve Gelişmiş analiz portalını erişmek için aşağıdaki adresi erişim gerektirir.  Tarayıcınız Azure portalında bir güvenlik duvarı üzerinden erişiyorsanız, bu adresleri erişimi etkinleştirmeniz gerekir.

| Uri | IP | Bağlantı Noktaları |
|:---|:---|:---|
| portal.loganalytics.io | Dinamik | 80,443 |
| api.loganalytics.io    | Dinamik | 80,443 |
| docs.loganalytics.io   | Dinamik | 80,443 |


## <a name="log-search-classic"></a>Günlük araması (Klasik)
Günlük arama sayfasından açın **günlükleri (Klasik)** Log Analytics menüsünde veya gelen **Log Analytics** Azure İzleyici menüsünde. Temel sorgular kullanarak günlük verilerini çözümlemek için uygundur. Bunu birden çok özellik düzenleme sorgular için sorgu dili hakkında tam bilgi zorunda kalmadan sağlar.  Bu özelliklerin bir özetini elde edebilirsiniz [oluşturma günlük arama özelliğini kullanarak Azure Log analytics'te günlük aramaları](log-analytics-log-search-log-search-portal.md). 


![Günlük arama sayfası](media/log-analytics-log-search-portals/log-search-portal.png)


## <a name="next-steps"></a>Sonraki adımlar

- İzlenecek yol bir [günlük arama özelliğini kullanarak öğretici](log-analytics-tutorial-viewdata.md) sorgu dilini kullanarak sorguları oluşturma hakkında bilgi edinmek için
- İzlenecek yol bir [Gelişmiş analiz portalını kullanarak Ders](query-language/get-started-analytics-portal.md) Log Analytics sayfası olarak aynı deneyimi sunar.

