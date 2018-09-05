---
title: Azure Stream Analytics için sorun giderme kılavuzu
description: Bu makalede, Azure Stream Analytics işleri, bağlantılar, girişler, çıkışlar, sorguları ve veri sorun giderme teknikleri açıklar.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/20/2017
ms.openlocfilehash: b1b5d0af3f2b149959bcb97ddaf29ba2fe1f4668
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2018
ms.locfileid: "43702230"
---
# <a name="troubleshooting-guide-for-azure-stream-analytics"></a>Azure Stream Analytics için sorun giderme kılavuzu

Azure Stream Analytics sorunlarını giderme karmaşık bir çaba ilk bakışta görünebilir. Çok sayıda kullanıcı cihazını çalıştıktan sonra girişler, çıkışlar, sorguları ve işlevleri hakkında kararın kaldırın ve işlemini kolaylaştırmaya yardımcı olmak için bu kılavuzu oluşturduk.

## <a name="troubleshoot-your-stream-analytics-job"></a>Stream Analytics işinizi sorunlarını giderme

Stream Analytics işinizi ilgili sorunu giderme en iyi sonuçları almak için aşağıdaki yönergeleri kullanın:

1.  Bağlantınızı test edin:
    - Kullanarak giriş ve çıkış bağlantısını doğrulayın **Test Bağlantısı** her giriş ve çıkış düğmesi.

2.  Girişinizi inceleyin:
    - Giriş verileri olay Hub'ına geçiyor doğrulamak için [hizmet veri yolu Gezgini](https://code.msdn.microsoft.com/windowsapps/Service-Bus-Explorer-f2abca5a) (olay hub'ı girişi kullanılıyorsa), Azure olay Hub'ına bağlanmak için.  
    - Kullanım [ **örnek verileri** ](stream-analytics-sample-data-input.md) düğme için her bir giriş ve örnek giriş verilerini indirin.
    - Veri şekli anlamak için örnek verileri İnceleme: şema ve [veri türleri](https://msdn.microsoft.com/library/azure/dn835065.aspx).

3.  Sorgunuzu test edin:
    - Üzerinde **sorgu** için sekmesinde, kullanın **Test** sorguyu sınamak için düğme ve indirilen örnek verileri kullanmak [sorguyu test](stream-analytics-test-query.md). Hataları inceleyin ve bunları düzeltmeye çalışın.
    - Kullanırsanız [ **TIMESTAMP By**](https://msdn.microsoft.com/library/azure/mt573293.aspx), olayları zaman damgaları büyüktür sahip olduğunuzu doğrulayın [iş başlangıç zamanı](stream-analytics-out-of-order-and-late-events.md).

4.  Sorgunuzu hata ayıklama:
    - Sorgudan aşamalı olarak basit bir select deyimi daha karmaşık toplamalara adımları kullanarak yeniden oluşturun. Adım adım sorgu mantığı oluşturma oluşturmak için kullanın [ile](https://msdn.microsoft.com/library/azure/dn835049.aspx) yan tümceleri.
    - Kullanım [SELECT INTO](stream-analytics-select-into.md) sorgu adımları hata ayıklamak için.

5.  Yaygın görülen tehlikeleri gibi kaldırın:
    - A [ **burada** ](https://msdn.microsoft.com/library/azure/dn835048.aspx) yan tümcesinin sorgudaki tüm olayları, herhangi bir çıktı oluşturulmasını önleme filtrelendi.
    - A [ **ATAMA** ](https://msdn.microsoft.com/azure/stream-analytics/reference/cast-azure-stream-analytics) işlevi başarısız, işinin başarısız olmasına neden olur. Tür atama hataları önlemek için [ **TRY_CAST** ](https://msdn.microsoft.com/azure/stream-analytics/reference/try-cast-azure-stream-analytics) yerine.
    - Pencere işlevleri kullandığınızda, tüm pencere süresi sorgudan bir sonuç görmek bekleyin.
    - Zaman damgası olayları için iş başlangıç zamanından önce gelir ve bu nedenle, olayların bırakılma.

6.  Olay sıralama kullanın:
    - Yukarıdaki adımların tümünü normal çalıştıysa Git **ayarları** dikey penceresinde ve select [ **olay sıralama**](stream-analytics-out-of-order-and-late-events.md). Bu ilke ne senaryonuzda mantıklı yapılandırıldığını doğrulayın. İlke *değil* kullandığınızda uygulanan **Test** düğmesi sorguyu test edin. Test işi üretimde çalışan ve tarayıcı arasındaki tek fark sonucudur.

7.  Ölçümleri kullanarak hata ayıklama:
    - Beklenen süre (sorguya bağlı olarak) sonra hiç çıkış elde etmek, aşağıdakileri deneyin:
        - Bakmak [ **izleme ölçümleri** ](stream-analytics-monitoring.md) üzerinde **İzleyici** sekmesi. Değerler toplanır çünkü ölçümleri birkaç dakika gecikir.
            - Giriş olayları > 0 ise, iş giriş verilerini okuyabilir. Giriş olayları > 0 ise, ardından değilse:
                - Veri kaynağı geçerli veriler olup olmadığını görmek için onu kullanarak kontrol [hizmet veri yolu Gezgini](https://code.msdn.microsoft.com/windowsapps/Service-Bus-Explorer-f2abca5a). Bu denetim, iş girdisi olarak olay hub'ı kullanıyorsanız geçerlidir.
                - Veri seri hale getirme biçiminin ve kodlama verileri beklendiği gibi olup olmadığını denetleyin.
                - İleti gövdesi olup olmadığını görmek için işin bir olay hub'ı kullanıyorsanız, denetleyin *Null*.
            - Veri dönüştürme hataları > 0 ve tırmanma, aşağıdaki olabilir true:
                - İş olayları seri durumdan çıkarılamıyor mümkün olmayabilir.
                - Olay şeması sorguda olayların tanımlı veya beklenen şema eşleşmeyebilir.
                - Bazı alanların veri türlerini beklentileri olay eşleşmeyebilir.
            - Çalışma zamanı hataları > 0 ise, geldiğini iş verileri alabilir ancak sorguyu işlerken hata oluşturuyor.
                - Hataları bulmak için Git [denetim günlüklerini](../azure-resource-manager/resource-group-audit.md) ve filtre *başarısız* durumu.
            - Varsa Inputevents > 0 ve OutputEvents = 0, aşağıdakilerden birini true olduğu anlamına gelir:
                - Sorgu işleme sıfır çıkış olayıyla sonuçlandı.
                - Olaylar veya bunların alanları sonra sorgu işleme sıfır çıkış kaynaklanan hatalı olabilir.
                - Veri göndermek için işi oluşturamadı [çıkış havuzu](stream-analytics-select-into.md) bağlantı veya kimlik doğrulama nedenleriyle.
        - Tüm daha önce bahsedilen hata durumlarda, işlem günlüğü iletilerine (olanlar dahil) ek ayrıntılar açıklayan dışındaki sorgu mantığının tüm olayları da burada filtre uygulanmış durumda. Birden çok olayın işlenmesi hataların oluşturursa, işlem günlükleri 10 dakika içerisinde aynı türdeki ilk üç hata iletileri Stream Analytics günlüğe kaydeder. Ardından aynı hataların diğer örneklerini "Hatalar çok hızlı gerçekleşiyor, bunlar gizlenen olay meydana gelir." yazan iletisiyle bastırır

8. Denetim ve tanılama günlüklerini kullanarak hata ayıklama:
    - Kullanım [denetim günlüklerini](../azure-resource-manager/resource-group-audit.md), hatalarını ayıklamanıza ve tanımlamak için filtreleyin.
    - Kullanım [iş tanılama günlükleri](stream-analytics-job-diagnostic-logs.md) tanımlamak ve hata ayıklama hataları için.

9. Çıktıları inceleyin:
    - İş durumu olduğunda *çalıştıran*bağlı olarak çıkış havuz veri kaynağında görebilirsiniz sorguda, belirlenen süre.
    - Bir özel çıkış türüne giden çıkışlar göremiyorsanız, bir Azure Blob gibi daha az karmaşık olan bir çıkış türüne yönlendirin. Depolama Gezgini'ni kullanarak çıkış görülebilir olup olmadığını denetleyin. Ayrıca, alınmasını çıkış, azaltma sınırları veri engelliyor olup olmadığını doğrulayın.

10. Veri akış analizi işi diyagram Ölçümleriyle kullanın:
    - Veri akışı çözümleme ve sorunları belirlemek için kullanın [iş diyagramı ölçümlerle](stream-analytics-job-diagram-with-metrics.md).

11. Bir destek talebi açın:
    - Son olarak, tüm denemeler başarısız olursa, bir Microsoft destek talebi işinizi içeren Subscriptionıd kullanarak açın.

## <a name="get-help"></a>Yardım alın

Daha fazla yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
