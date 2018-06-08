---
title: Azure akış analizi için sorun giderme kılavuzu
description: Bu makalede, Azure akış analizi işleri, bağlantılar, giriş, çıktıları, sorguları ve veri sorun giderme teknikleri açıklar.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/20/2017
ms.openlocfilehash: 2eefabcc0484fca0e6e3ad1dd5037684a759d010
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34850455"
---
# <a name="troubleshooting-guide-for-azure-stream-analytics"></a>Azure akış analizi için sorun giderme kılavuzu

Azure Stream Analytics sorun giderme karmaşık çaba ilk bakışta görünebilir. Birden çok kullanıcıya sahip çalıştıktan sonra işlemini kolaylaştırmak ve konusunda güvenilir girişleri, çıktıları, sorgular ve işlevleri hakkında bilgiler kaldırma yardımcı olmak için bu kılavuzu oluşturduk.

## <a name="troubleshoot-your-stream-analytics-job"></a>Akış analizi işi sorunlarını giderme

Sorun giderme, Stream Analytics işi, en iyi sonuçlar için aşağıdaki yönergeleri kullanın:

1.  Bağlantınızı test edin:
    - Kullanarak girişleri ve çıkışları bağlanabilirliği doğrulamak **Bağlantıyı Sına** her giriş ve çıkış düğmesi.

2.  Giriş verilerinizi inceleyin:
    - Olay Hub'ına giriş verisi akan olduğunu doğrulamak için kullanılan [hizmet veri yolu Gezgini](https://code.msdn.microsoft.com/windowsapps/Service-Bus-Explorer-f2abca5a) (olay hub'ı giriş kullanılıyorsa) Azure Event Hub'ına bağlanmak için.  
    - Kullanım [ **örnek verileri** ](stream-analytics-sample-data-input.md) düğmesini her giriş için ve giriş örnek veri indirin.
    - Veri şekli anlamak amacıyla örnek verileri inceleyin: şema ve [veri türleri](https://msdn.microsoft.com/library/azure/dn835065.aspx).

3.  Sorgunuzu test edin:
    - Üzerinde **sorgu** sekmesinde, kullanmak **Test** sorguyu sınamak için düğmesine tıklayın ve indirilen örnek verileri kullanmak [sorgu test](stream-analytics-test-query.md). Hataları inceleyin ve bunları düzeltmeye çalışın.
    - Kullanırsanız [ **TIMESTAMP By**](https://msdn.microsoft.com/library/azure/mt573293.aspx), olayları zaman damgaları büyük sahip olduğunuzu doğrulayın [iş başlangıç zamanı](stream-analytics-out-of-order-and-late-events.md).

4.  Sorgunuz hata ayıklama:
    - Sorgudan aşamalı olarak basit select deyimi daha karmaşık toplamalara adımları kullanarak yeniden oluşturun. Adım adım sorgu mantığı oluşturan oluşturmak için kullanmak [WITH](https://msdn.microsoft.com/library/azure/dn835049.aspx) yan tümceleri.
    - Kullanım [SELECT INTO](stream-analytics-select-into.md) sorgu adımları hata ayıklamak için.

5.  Ortak Tuzaklar gibi kaldırın:
    - A [ **nerede** ](https://msdn.microsoft.com/library/azure/dn835048.aspx) yan tümcesinin sorgudaki tüm olayları, herhangi bir çıktı oluşturulmasını önler önleme filtrelenmelidir.
    - A [ **CAST** ](https://msdn.microsoft.com/azure/stream-analytics/reference/cast-azure-stream-analytics) işlev başarısız, iş başarısız olmasına neden olur. Tür atama hataları önlemek için [ **TRY_CAST** ](https://msdn.microsoft.com/azure/stream-analytics/reference/try-cast-azure-stream-analytics) yerine.
    - Pencere işlevleri kullandığınızda, sorgudan bir çıktı görmeniz tüm pencereyi süre bekleyin.
    - Zaman damgası olayları için iş başlangıç zamanı önündeki ve olayları bu nedenle, bıraktı.

6.  Olay sıralama kullanın:
    - Yukarıdaki adımların tümünü ince çalışılan, Git **ayarları** dikey penceresinde ve select [ **olay sıralama**](stream-analytics-out-of-order-and-late-events.md). Bu ilke ne senaryonuzda mantıklı yapılandırıldığından emin olun. İlke *değil* kullandığınızda uygulanan **Test** sorguyu sınamak için düğmesi. İş üretimde çalışan karşı tarayıcı içi test arasındaki tek fark sonucudur.

7.  Ölçümleri kullanarak hata ayıklama:
    - (Sorguya dayalı) beklenen süre sonra çıktı elde etmek, aşağıdakileri deneyin:
        - Bakmak [ **izleme ölçümleri** ](stream-analytics-monitoring.md) üzerinde **İzleyici** sekmesi. Değerleri toplanır çünkü ölçümleri birkaç dakika gecikir.
            - Varsa giriş olaylarını > 0 ise, iş giriş verileri okuyabilir. Giriş olayları > 0 ise, ardından değilse:
                - Veri kaynağı geçerli veriler olup olmadığını görmek için bunu kullanarak denetleyin [hizmet veri yolu Gezgini](https://code.msdn.microsoft.com/windowsapps/Service-Bus-Explorer-f2abca5a). İş giriş olarak olay hub'ı kullanıyorsa, bu denetimi uygular.
                - Veri seri hale getirme biçimi ve kodlama verileri beklendiği gibi olup olmadığını denetleyin.
                - İş bir olay hub'ı kullanırken, ileti gövdesi olup olmadığını kontrol edin *Null*.
            - Veri dönüştürme hataları > 0 ve climbing, aşağıdaki olabilir doğru:
                - İş olayları seri durumdan mümkün olmayabilir.
                - Olay şema sorgusunda olaylarının tanımlanan veya beklenen şema eşleşmeyebilir.
                - Veri türlerini bazı alanlar beklentilerini olay eşleşmeyebilir.
            - Çalışma zamanı hataları > 0 ise, bu anlamına gelir iş verileri alabilir ancak sorgu işlenirken bir hata oluşturuluyor.
                - Hataları bulmak için şu adrese gidin [denetim günlüklerini](../azure-resource-manager/resource-group-audit.md) ve filtre *başarısız* durumu.
            - Varsa InputEvents > 0 ve OutputEvents = 0, aşağıdakilerden birini doğru olduğu anlamına gelir:
                - Sorgu işleme sıfır çıkış olayıyla sonuçlandı.
                - Olayları veya kendi alanlar sorgu işlendikten sonra sıfır çıkış kaynaklanan hatalı olabilir.
                - Veri göndermek için iş başaramadı [çıkış havuzunun](stream-analytics-select-into.md) bağlantı veya kimlik doğrulaması nedeniyle.
        - Tüm yukarıda açıklanan hata durumlarda, işlem günlüğü iletileri (olanlar dahil) ek ayrıntılar açıklayan dışındaki sorgu mantığı tüm olayları da burada filtre uygulanmış durumda. Birden çok olayı işleme hatalar oluşturursa, Stream Analytics aynı türde ilk üç hata iletilerini işlem günlükleri için 10 dakika içinde günlüğe kaydeder. Ardından "Hataları çok hızlı bir şekilde bu gizlenen gerçekleştiği." iletisini ek aynı hatalarla gizler

8. Denetim ve tanılama günlüklerini kullanarak hata ayıklama:
    - Kullanım [denetim günlüklerini](../azure-resource-manager/resource-group-audit.md)ve belirlemek ve hataları hata ayıklamak için filtre.
    - Kullanım [iş tanılama günlüklerini](stream-analytics-job-diagnostic-logs.md) tanımlamak ve hata ayıklama hataları için.

9. Çıkışları inceleyin:
    - İş durumu olduğunda *çalıştıran*havuz veri kaynağı çıktıda görebilirsiniz sorguda, belirlenen süre bağlı olarak.
    - Belirli çıktı türü için uygulayacağınız çıkışları göremiyorsanız, Azure Blob gibi daha karmaşık bir çıkış türü için yönlendirin. Depolama Gezgini'ni kullanarak, çıktı görülen olup olmadığını denetleyin. Ayrıca, alınmasını azaltma sınırları çıkış adresindeki veri engelliyor olup olmadığını doğrulayın.

10. Veri akış analizi işi diyagramı Ölçümleriyle kullanın:
    - Veri akışı çözümlemek ve sorunları belirlemek için kullanılan [ölçümleri ile iş diyagramı](stream-analytics-job-diagram-with-metrics.md).

11. Bir destek servis talebi açın:
    - Son olarak, tüm yöntemler başarısız olursa, Microsoft destek servis talebi işinizi içeren Subscriptionıd kullanarak açın.

## <a name="get-help"></a>Yardım alın

Daha fazla yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
