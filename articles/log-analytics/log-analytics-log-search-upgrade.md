---
title: "Yeni günlük arama Azure günlük analizi yükseltme | Microsoft Docs"
description: "Yeni günlük analizi sorgu dili neredeyse işte ve genel önizlemede katılabilirsiniz.  Bu makalede yeni dil ve çalışma alanınızda Dönüştür nasıl avantajları açıklanmıştır."
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
ms.date: 10/10/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 36a08cab2b1d409d2de382a07cfd7259a1c94e13
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="azure-log-analytics-upgrade-to-new-log-search"></a>Yeni günlük arama Azure günlük analizi yükseltme

Yeni günlük analizi sorgu dili işte ve çalışma alanınızı onu yararlanmak için yükseltilmesi gerekiyor.  Çalışma alanınızı kendiniz yükseltin ya da geç Ekim ayında başlar ve yılın sonunu gider sunum döneminde otomatik olarak yükseltilmesi bekleyin.  Bu makalede yeni dil ve çalışma alanınızda Dönüştür nasıl avantajları açıklanmıştır.  

## <a name="why-the-new-language"></a>Neden yeni dil?
Biz, tüm geçiş sorunları yoktur ve biz yalnızca eğlenceli sorgu dilini değiştirme olmayan anlayın.  Bu değişiklik günlük analizi müşterilerimizin önemli değeri sağlayacak birkaç nedeni vardır.

- **Basit ancak güçlü.** Yeni anlamak daha kolay ve daha eski dilinden doğal dil gibi yapıları ile SQL benzer dilidir.
- **Tam yöneltme dili.**  Yeni dil eski dilinden daha kapsamlı Yöneltme özellikleri vardır.  Daha önce mümkün olandan daha karmaşık sorgular oluşturmak için başka bir komuta yöneltilen neredeyse tüm çıkış.
- **Arama zaman alan ayıklamaları.**  Yeni dil eski dilinden daha gelişmiş çalışma zamanı hesaplanan alanları destekler.  Karmaşık hesaplamalar için genişletilmiş alanları kullanın ve ardından hesaplanan alanlar birleşimler ve toplamalar dahil olmak üzere ek komutlar için kullanın.
- **Gelişmiş birleştirmeler.**  Yeni dil birden çok alan tablolarda katılma, iç ve dış birleştirmeler kullanın ve genişletilmiş alanları katılma olanağı dahil olmak üzere eski dilinden daha gelişmiş birleştirmeler sağlar.
- **İşlevler tarih veya saat.**  Yeni dil tarih/saat işlevleri eski dil daha gelişmiş.
- **Akıllı çözümlemeleri.**  Yeni dil kümelerindeki desenleri değerlendirmek ve farklı veri kümelerinin karşılaştırmak için algoritmaları Gelişmiş.
- **Gelişmiş Analytics portalı.**  Gelişmiş analizler portal çok satırlı dahil olmak üzere günlük analizi portalında kullanılamaz analiz özellikler sunar sorguları, ek görselleştirmeleri ve Gelişmiş tanılama düzenleme.
- **Diğer uygulamalarla tutarlılık.**  Zaten yeni dil ve Gelişmiş analizi portalında Application Insights analizleri için kullanılır.  Günlük analizi için uygulama tutarlılığı Azure Hizmetleri genelinde sağlar.
- **Power BI ile daha iyi tümleştirme.** Zengin veri dönüştürme yeteneklerini kullanabilir şekilde yeni dil sorgularda Power BI Desktop için verilebilir.
- **Daha fazlasını.** Başvurmak [Azure günlük analizi sorgu dili](https://docs.loganalytics.io) sitede ayrıntılarının ve öğreticiler yeni dili.


## <a name="when-can-i-upgrade"></a>Ne zaman ı yükseltebilir miyim?
Bu diğerlerinden önce bazı bölgelerde kullanılabilir olabilir şekilde yükseltme tüm Azure bölgeler arasında alınacak.  Çalışma alanınızı yükseltmek için davet çalışma alanınızı üstte bir başlık gördüğünüzde, yükseltilecek kullanılabilir olduğunda bildiğiniz.

![Yükseltme 1](media/log-analytics-log-search-upgrade/upgrade-01a.png)

Ardından, çalışma alanı otomatik olarak yükselttiyseniz, herhangi bir sorunla karşılaşıldı olup olmadığını belirleme özeti ile yükseltilir belirten bir başlık görürsünüz.

 ![Otomatik yükseltme](media/log-analytics-log-search-upgrade/auto-upgrade.png)


## <a name="what-happens-after-the-upgrade"></a>Yükseltmeden sonra ne olur?
Dönüştürüldüğünde aşağıdaki değişiklikler alanınıza yapılır:

- Tüm kayıtlı aramaları, uyarı kuralları ve Görünüm Tasarımcısı ile oluşturduğunuz görünümler otomatik olarak yeni dil dönüştürülür.  Çözümlerinde dahil aramaları otomatik olarak dönüştürülmez, ancak bunları açtığınızda, bunun yerine anında dönüştürüldüğüne.  
- [Benim Panom](log-analytics-dashboards.md) şunun için kullanım [Görünüm Tasarımcısı](log-analytics-view-designer.md) ve [Azure panolar](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards.md).  My panoya eklenen döşeme hala kullanılabildiği ancak salt okunur.
- [Power BI tümleştirme](log-analytics-powerbi.md) yeni bir işlem ile değiştirilir.  Oluşturduğunuz tüm var olan bir Power BI zamanlamalar devre dışı bırakılır ve yeni işlem ile değiştirdiğinizden gerekiyor.
- Gelen yanıtları [uyarı eylemleri](log-analytics-alerts-actions.md) Web kancalarını ve runbook'ları kullanarak yeni bir biçime sahiptir ve uyarı kurallarınızı uygun şekilde güncelleştirmeniz gerekebilir.
- Bakın [günlük arama SSS](log-analytics-log-search-faq.md) yükseltme hakkında sık sorulan sorular için.

## <a name="how-do-i-know-if-there-were-any-issues-from-the-upgrade"></a>Yükseltme gelen sorunları olup olmadığını nasıl anlayabilirim?
Yükseltme tamamlandıktan sonra olacaktır bir **yükseltme Özet** çalışma alanı için ayarları bölümünde.  Karşılaşılan sorunları görüntülemek için ve yükseltme hakkında bilgi için bu bölümüne bakın.

 ![Yükseltme özeti](media/log-analytics-log-search-upgrade/upgrade-summary.png)

## <a name="how-do-i-manually-perform-the-upgrade"></a>Yükseltme el ile nasıl yaparım?
Başlık portalı üstündeki gördüğünüzde, çalışma alanınızı yükseltebilirsiniz.  

1.  Bildiren başlığında tıklayarak yükseltme işlemini başlatmadan **daha fazla bilgi edinin ve yükseltme**.

    ![Yükseltme 2](media/log-analytics-log-search-upgrade/upgrade-01a.png)<br>

2.  Yükseltme hakkında ek bilgi yükseltme bilgileri sayfasında aracılığıyla okuyun.

    ![Yükseltme 2](media/log-analytics-log-search-upgrade/upgrade-03.png)<br>

3.  Tıklatın **Şimdi Yükselt** yükseltme işlemini başlatmak için.

    ![Yükseltme 4](media/log-analytics-log-search-upgrade/upgrade-04.png)<br>Sağ üst köşesinde bir bildirim kutusunda durumu gösterilir.
    
    ![5 yükseltme](media/log-analytics-log-search-upgrade/upgrade-05.png)

4.  Bu kadar!  Yükseltilen çalışma alanınızı bakmak için günlük arama sayfasına gidin.

    ![Yükseltme 6](media/log-analytics-log-search-upgrade/upgrade-06.png)

Yükseltme başarısız olmasına neden olan bir sorunla karşılaşırsanız, gidebilirsiniz [tartışma Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=opinsights) ve sorunuzu gönderin veya [bir destek isteği oluşturmak](../azure-supportability/how-to-create-azure-support-request.md) Azure portalından.

## <a name="how-do-i-learn-the-new-language"></a>Yeni dil nasıl bilgi?
Birden fazla hizmet tarafından kullanıldığından oluşturduk bir [belgelerine barındırmak için dış site](https://docs.loganalytics.io/) yeni dil için.  Bu öğreticiler, örnekler ve hızlıca gelen yardımcı olması için tam bir başvuru içerir. Yeni dilinin bir öğretici size yol [sorguları ile çalışmaya başlama](https://go.microsoft.com/fwlink/?linkid=856078) ve dil başvurusu, erişim [günlük analizi sorgu dili](https://go.microsoft.com/fwlink/?linkid=856079).  

Bir grup örnek verileri de dahil olmak üzere bir tanıtım ortamında yeni dil denemek istiyorsanız, göz atın sahip [playground ortam](https://portal.loganalytics.io/demo#/discover/home).

Daha sonra zaten ancak eski günlük analizi sorgu dili ile konusunda bilginiz varsa, yükseltme işleminin bir parçası olarak alanınıza eklenen dil dönüştürücü kullanabilirsiniz.  Yalnızca eski sorgunuzu yazın ve ardından **Dönüştür** çevrilmiş sürümü görmek için.  Bundan sonra ya da arama veya kopya çalıştırın ve bir uyarı kuralı gibi başka bir yerde kullanmayı dönüştürülen sorgu yapıştırmak için arama düğmesini kullanabilirsiniz.  Bakmayı da sahip olabilirsiniz bizim [kopya sayfası](log-analytics-log-search-transition.md) , doğrudan eski dilden genel sorgular karşılaştırır.

![Dil dönüştürücü](media/log-analytics-log-search-upgrade/language-converter.png)


## <a name="next-steps"></a>Sonraki adımlar
- Kullanıma bir [yeni dil öğretici](https://go.microsoft.com/fwlink/?linkid=856078).
- İzlenecek yol bir [günlük arama Portalı'nı kullanarak öğretici](log-analytics-log-search-log-search-portal.md) yeni sorgu dili.
- Yeni bir giriş almak [Advanced Analytics portalı](https://go.microsoft.com/fwlink/?linkid=856587).
