---
title: "Oluşturma ve Azure günlük analizi günlük sorgularda düzenleme için portalları | Microsoft Docs"
description: "Bu makalede kullanabileceğiniz portalları oluşturmak ve düzenlemek için Azure günlük analizi oturum arar."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2017
ms.author: magoedte; bwren
ms.openlocfilehash: b205f226d95d94b938a70a834ac0147e76d459ea
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="portals-for-creating-and-editing-log-queries-in-azure-log-analytics"></a>Oluşturma ve Azure günlük analizi günlük sorgularda düzenleme için portalları

Günlük analizi yerlerde çeşitli günlük aramaları çalışma alanından veri almak için kullanın.  Gerçekte oluşturmak ve etkileşimli olarak döndürülen veriler ile çalışmanın yanı sıra sorguları ancak düzenleme için aşağıda açıklanan iki seçeneğiniz vardır.  

## <a name="log-search"></a>Günlük araması 
Günlük arama sayfası Azure portalından erişilebilir.  Tek bir satıra oluşturulabilir temel sorguları oluşturmak için uygundur.  Bir dış portal başlatmadan günlük arama kullanılabilir ve günlük aramalarında uyarı kuralları oluşturma, bilgisayar gruplarını oluşturma ve sorgunun sonuçlarını dışarı aktarma gibi çeşitli işlevleri gerçekleştirmek için kullanabilirsiniz.  

Günlük arama, tam bir sorgu dili bilgisi olmadan sorguyu düzenlemek için birden çok özellik sağlar.  Bu özelliklerin özetini almak [oluşturma günlük günlük arama özelliğini kullanarak Azure günlük analizi aramalarda](log-analytics-log-search-log-search-portal.md).


![Günlük arama sayfası](media/log-analytics-log-search-portals/log-search-portal.png)

## <a name="advanced-analytics-portal"></a>Gelişmiş Analytics portalı
Gelişmiş analizler portal günlük aramada Azure portalından kullanılamaz gelişmiş işlevselliği sağlayan adanmış bir portalıdır.  Özellikler sorguda birden çok satıra düzenleme, seçime bağlı olarak kod, içeriğe duyarlı IntelliSense ve akıllı analizi yürütmek yeteneğini içerir.  Advanced Analytics portal ya da günlük arama kaydedilmiş veya kopyalanır ve diğer günlük analizi elemanlara yapıştırılan karmaşık sorgular tasarlamak için en uygun durumda.  Günlük arama sayfasında bir bağlantıyı Advanced Analytics portalından başlatın.

![Gelişmiş Analytics portalı](media/log-analytics-log-search-portals/advanced-analytics-portal.png)


Gelişmiş özelliklerini nedeniyle, genellikle gelişmiş analizler portal birincil aracınız olarak oluşturmak ve sorguları düzenleme için kullanırsınız.  Bir kez sorgu beklendiği gibi çalışır, sonra kopyalayın ve günlük arama sayfası veya Görünüm Tasarımcısı gibi başka bir yere yapıştırın saptadıktan.  Advanced Analytics portalı birden fazla satır sorgularıyla ancak desteklediğinden, bir sorgu bu portaldan kopyalarken aşağıdaki noktaları dikkate alın gerekir.

- Kopyalanır ve başka bir konuma yapıştırılan önce açıklamaları sorgudan kaldırılması gerekir.  İki eğik çizgi ile koyarak bir satır yorum yapabileceği (/ /).  Tek bir satıra birden çok satır sorgu yapıştırın, satır sonları kaldırılır.  Yorumlar eklenirse, ilk açıklama sonra tüm karakterleri yorum bir parçası olarak kabul edilir.


## <a name="next-steps"></a>Sonraki adımlar

- Bir öğreticide kullanarak yol [günlük arama](log-analytics-tutorial-viewdata.md) sorgu dili kullanarak sorgu oluşturma hakkında bilgi edinmek için
- Kullanıma [Advanced Analytics portalı](https://go.microsoft.com/fwlink/?linkid=856587) karmaşık sorgular oluşturun ve günlük aramalarınız için bir geliştirme ortamı olarak kullanmak için.

