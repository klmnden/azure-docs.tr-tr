---
title: Oluşturma ve Azure günlük analizi günlük sorgularda düzenleme için portalları | Microsoft Docs
description: Bu makalede kullanabileceğiniz portalları oluşturmak ve düzenlemek için Azure günlük analizi oturum arar.
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
ms.date: 06/11/2018
ms.author: magoedte; bwren
ms.component: na
ms.openlocfilehash: e2ea0bf1fb3f1c63f4e6f037e465e8fdfd9a4374
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37133120"
---
# <a name="portals-for-creating-and-editing-log-queries-in-azure-log-analytics"></a>Oluşturma ve Azure günlük analizi günlük sorgularda düzenleme için portalları

Günlük analizi yerlerde çeşitli günlük aramaları çalışma alanından veri almak için kullanın.  Gerçekte oluşturmak ve etkileşimli olarak döndürülen veriler ile çalışmanın yanı sıra sorguları ancak düzenleme için aşağıda açıklanan iki seçeneğiniz vardır.  

## <a name="log-search"></a>Günlük araması 
Günlük arama sayfası Azure portalından erişilebilir.  Tek bir satıra oluşturulabilir temel sorguları oluşturmak için uygundur.  Bir dış portal başlatmadan günlük arama kullanılabilir ve günlük aramalarında uyarı kuralları oluşturma, bilgisayar gruplarını oluşturma ve sorgunun sonuçlarını dışarı aktarma gibi çeşitli işlevleri gerçekleştirmek için kullanabilirsiniz.  

Günlük arama, tam bir sorgu dili bilgisi olmadan sorguyu düzenlemek için birden çok özellik sağlar.  Bu özelliklerin özetini almak [oluşturma günlük günlük arama özelliğini kullanarak Azure günlük analizi aramalarda](log-analytics-log-search-log-search-portal.md).


![Günlük arama sayfası](media/log-analytics-log-search-portals/log-search-portal.png)

## <a name="advanced-analytics-portal"></a>Gelişmiş Analytics portalı
Gelişmiş analizler portal günlük aramada Azure portalından kullanılamaz gelişmiş işlevselliği sağlayan adanmış bir portalıdır.  Birden fazla satırda sorgu düzenleme, seçerek kod yürütme, bağlama duyarlı IntelliSense ve Akıllı Analiz özellikleri mevcuttur.  Advanced Analytics portal ya da günlük arama kaydedilmiş veya kopyalanır ve diğer günlük analizi elemanlara yapıştırılan karmaşık sorgular tasarlamak için en uygun durumda.  Günlük arama sayfasında bir bağlantıyı Advanced Analytics portalından başlatın.

![Gelişmiş Analytics portalı](media/log-analytics-log-search-portals/advanced-analytics-portal.png)


Gelişmiş özelliklerini nedeniyle, genellikle gelişmiş analizler portal birincil aracınız olarak oluşturmak ve sorguları düzenleme için kullanırsınız.  Bir kez sorgu beklendiği gibi çalışır, sonra kopyalayın ve günlük arama sayfası veya Görünüm Tasarımcısı gibi başka bir yere yapıştırın saptadıktan.  

### <a name="firewall-requirements"></a>Güvenlik duvarı gereksinimleri
Tarayıcınız Advanced Analytics portalına erişmek için aşağıdaki adresi erişim gerektirir.  Tarayıcınız bir güvenlik duvarı üzerinden Azure portalına erişim varsa, bu adresleri erişimi etkinleştirmeniz gerekir.

| Uri | IP | Bağlantı Noktaları |
|:---|:---|:---|
| portal.loganalytics.io | Dinamik | 80,443 |
| api.loganalytics.io    | Dinamik | 80,443 |
| docs.loganalytics.io   | Dinamik | 80,443 |


## <a name="next-steps"></a>Sonraki adımlar

- Bir öğreticide kullanarak yol [günlük arama](log-analytics-tutorial-viewdata.md) sorgu dili kullanarak sorgu oluşturma hakkında bilgi edinmek için
- Kullanıma [Advanced Analytics portalı](https://go.microsoft.com/fwlink/?linkid=856587) karmaşık sorgular oluşturun ve günlük aramalarınız için bir geliştirme ortamı olarak kullanmak için.

