---
title: "Olağan dışı tekrarlayan bir iş ile ilgili sorunları giderme | Microsoft Docs"
description: "Olağan dışı tekrarlayan bir işi hata ayıklamak için Visual Studio için Azure Data Lake araçları kullanmayı öğrenin."
services: data-lake-analytics
documentationcenter: 
author: yanancai
manager: 
editor: 
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/27/2017
ms.author: yanacai
ms.openlocfilehash: a358f94b117c12511028a875e56b5c9dba8d3382
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-troubleshoot-an-abnormal-recurring-job"></a>Olağan dışı tekrarlayan bir iş ile ilgili sorunları giderme

Bu belgede, biz nasıl kullanılacağını tanıtılacaktır [Visual Studio için Azure Data Lake Araçları](http://aka.ms/adltoolsvs) yinelenen iş sorunlarını gidermek için. Ardışık Düzen ve yinelenen işleri hakkında daha fazla bilgi [burada](https://blogs.msdn.microsoft.com/azuredatalake/2017/09/19/managing-pipeline-recurring-jobs-in-azure-data-lake-analytics-made-easy/).
Yinelenen işleri genellikle aynı sorgu mantığı ve benzer giriş verileri paylaşır. Örneğin, her Pazartesi sabah 8 da çalışan yinelenen bir işi sahip Geçen haftaki haftalık etkin kullanıcı sayısı için sorgu mantığı içeren bir komut dosyası şablonu bu işleri için komut dosyalarını paylaşabilir ve bu işleri için kullanım verilerini son hafta girişlerdir. Aynı paylaşımı mantığı sorgulamak ve benzer genellikle bu işleri anlamına gelir performansını benzer ve yinelenen işleriniz birine aniden anormal, başarısız çok, size yavaşlamasına gerçekleştirirse kararlı isteyebilirsiniz giriş:

1.  Yinelenen işin ne olduğunu görmek için önizlemeleri çalışmaları için istatistikleri rapor bakın.
2.  Karşılaştırma anormal iş ne yaptığını bulmak için normal bir tane ile değiştirildi.

**İş görünümünde ilgili** Visual Studio yardımcı için Azure Data Lake araçları içinde her iki durumda ile sorun giderme ilerleme hızlandırmak.

## <a name="step-1-find-recurring-jobs-and-open-related-job-view"></a>1. adım: yinelenen işleri bulun ve ilgili iş görünümünde açın

İlgili iş kullanmak için Görünüm yinelenen iş sorun giderme, önce Visual Studio'da yinelenen işi bulun ve ilgili iş görünümünde açmak gerekir.

### <a name="case-1-you-have-the-url-for-the-recurring-job"></a>Durum 1: Yinelenen işi URL'sini sahip

Aracılığıyla **Araçlar > Data Lake > İş görünümünde**, Visual Studio Proje görünümü açmak için iş URL'yi yapıştırabilirsiniz ve görünüm üzerinden açmak için ilgili işleri ilgili iş görüntüleyin.

![Data Lake Analytics araçları ilgili işleri görüntüle](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/view-related-job.png)
 
### <a name="case-2-you-have-the-pipeline-for-the-recurring-job-but-not-the-url"></a>Durum 2: Yinelenen iş ancak URL için ardışık düzenini sahip

Visual Studio'da, ardışık düzen tarayıcı üzerinden açabilirsiniz **Sunucu Gezgini > Data Lake Analytics hesabınızı > ardışık düzen** (Bu düğüm Sunucu Gezgini'nde bulamıyorsanız, lütfen en son sürümünü Aracı'nı edinin [burada](http://aka.ms/adltoolsvs)). Ardışık Düzen tarayıcıda ADLA hesabı için tüm işlem hatlarını solda listelenir, tüm yinelenen işlerin bulmak için tıklatın ardışık düzen genişletebilirsiniz bir sorun var, sağ taraftaki ilgili iş görünümünde açılır.

![Data Lake Analytics araçları ilgili işleri görüntüle](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/pipeline-browser.png)

![Data Lake Analytics araçları ilgili işleri görüntüle](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-view.png)

## <a name="step-2-analyze-statistics-report"></a>2. adım: istatistikleri rapor Çözümle

Bir Özet ve istatistikleri rapor ilgili iş görünümünde üstünde anormal olası kök nedeni alabilirsiniz gösterilmektedir. 

1.  İlk raporda anormal iş bulmanız gerekir. X ekseni anormal iş bulabilir iş gönderme zamanı gösterir.
2.  İstatistikleri denetleyin ve anormal olarak Öngörüler ve olası çözümleri almak için aşağıdaki süreci izleyin.

![Data Lake Analytics araçları ilgili işleri görüntüle](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-metrics-debugging-flow.png)

## <a name="step-3-compare-the-abnormal-recurring-job-to-a-normal-job"></a>3. adım: karşılaştırma normal bir iş anormal yinelenen işi

Tüm yinelenen işlerini ilgili iş görünümünde altındaki iş listesi üzerinden gönderilen bulabilirsiniz. Sağ tıklatma Öngörüler ve olası çözümleri daha fazla iş fark görünümünde bulmak için önceki normal bir anormal işlemiyle karşılaştırabilirsiniz.

![Data Lake Analytics araçları ilgili işleri görüntüle](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/compare-job.png)

Genellikle bu 2 işleri arasında büyük farklar için bunlar büyük olasılıkla performans sorunlarına neden nedenleri ve ayrıca daha fazla denetimi yapmak için aşağıdaki adımları izleyebilirsiniz dikkat etmeniz gerekir.

![Data Lake Analytics araçları ilgili işleri görüntüle](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-diff-debugging-flow.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Hata ayıklama ve veri eğme sorunları gidermek nasıl](data-lake-analytics-data-lake-tools-data-skew-solutions.md)
* [U-SQL iş hatası kullanıcı tanımlı kod hatası için hata ayıklama](data-lake-analytics-debug-u-sql-jobs.md)