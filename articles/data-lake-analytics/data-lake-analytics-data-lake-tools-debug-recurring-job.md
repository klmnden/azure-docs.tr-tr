---
title: Azure Data Lake Analytics yinelenen projelerde hata ayıklama
description: Olağan dışı tekrarlayan bir işi hata ayıklamak için Visual Studio için Azure Data Lake araçları kullanmayı öğrenin.
services: data-lake-analytics
author: yanancai
manager: kfile
editor: jasonwhowell
ms.author: yanacai
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 05/20/2018
ms.openlocfilehash: 6a181e0cb4014f80673c1bd33e89af69c5677b37
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34623579"
---
# <a name="troubleshoot-an-abnormal-recurring-job"></a>Olağan dışı tekrarlayan bir iş sorunlarını giderme

Bu makalede nasıl kullanılacağını gösterir [Visual Studio için Azure Data Lake Araçları](http://aka.ms/adltoolsvs) yinelenen işleri ile ilgili sorunları gidermek için. Ardışık Düzen ve yinelenen işleri hakkında daha fazla bilgi [Azure Data Lake ve Azure Hdınsight blog](https://blogs.msdn.microsoft.com/azuredatalake/2017/09/19/managing-pipeline-recurring-jobs-in-azure-data-lake-analytics-made-easy/).

Yinelenen işleri genellikle aynı sorgu mantığı ve benzer giriş verileri paylaşır. Örneğin, her Pazartesi sabahı 8 sabah çalışan yinelenen bir işi olduğunu düşünün. Geçen haftaki haftalık etkin kullanıcı sayısı için. Bu işleri için komut dosyalarını sorgu mantığı içeren bir komut dosyası şablon paylaşır. Bu işleri için kullanım verilerini son hafta girdileridir. Aynı sorgu mantığı ve benzer giriş genellikle paylaşımı bu işleri performansını benzer ve kararlı anlamına gelir. Yinelenen işleriniz birine aniden aşırı, başarısız gerçekleştirir veya çok yavaşlatır, bırakmak isteyebilirsiniz:

- Yinelenen işin ne olduğunu görmek için önceki çalışmaları için istatistikleri rapor bakın.
- Karşılaştırma anormal iş ne yaptığını bulmak için normal bir tane ile değiştirildi.

**İş görünümünde ilgili** Visual Studio yardımcı için Azure Data Lake araçları içinde her iki durumda ile sorun giderme ilerleme hızlandırmak.

## <a name="step-1-find-recurring-jobs-and-open-related-job-view"></a>1. adım: yinelenen işleri bulun ve ilgili iş görünümünde açın

Yinelenen bir iş sorununu gidermek için ilgili iş görünümünde kullanmak için önce Visual Studio'da yinelenen işi bulun ve ilgili iş görünümünde açmak gerekir.

### <a name="case-1-you-have-the-url-for-the-recurring-job"></a>Durum 1: Yinelenen işi URL'sini sahip

Aracılığıyla **Araçları** > **Data Lake** > **iş görünümünde**, Visual Studio Proje görünümü açmak için iş URL'yi yapıştırabilirsiniz. Seçin **ilgili işleri görüntüle** ilgili iş görünümünü açın.

![Görünüm ilgili işleri bağlantıyı Data Lake Analytics araçları](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/view-related-job.png)
 
### <a name="case-2-you-have-the-pipeline-for-the-recurring-job-but-not-the-url"></a>Durum 2: Yinelenen iş ancak URL için ardışık düzenini sahip

Visual Studio'da, ardışık düzen tarayıcı Sunucu Gezgini üzerinden açabilirsiniz > Azure Data Lake Analytics hesabınızı > **ardışık düzen**. (Bu düğüm Sunucu Gezgini'nde bulamıyorsanız, [son eklentiyi karşıdan](http://aka.ms/adltoolsvs).) 

![Ardışık Düzen düğümü seçme](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/pipeline-browser.png)

Ardışık Düzen tarayıcıda Data Lake Analytics hesabı için tüm işlem hatlarını sol altında listelenir. Tüm yinelenen işlerin bulmak için ardışık düzen genişletin ve ardından sorunları olan bir tanesini seçin. İlgili iş görünümünde, sağ taraftaki açılır.

![Bir işlem hattı seçerek ve ilişkili iş görünümünü açma](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-view.png)

## <a name="step-2-analyze-a-statistics-report"></a>2. adım: bir istatistikleri rapor Çözümle

Bir Özet ve istatistikleri rapor ilgili iş görünümünde üstünde gösterilir. Burada, sorunun olası kök nedeni bulabilirsiniz. 

1.  Raporda, x ekseni iş gönderme zamanı gösterir. Anormal iş bulmak için bunu kullanın.
2.  İstatistikleri denetleyin ve sorunu ve olası çözümleri hakkında bilgiler edinmek için aşağıdaki şemada işlemini kullanın.

![İstatistikleri denetleme işlemi diyagramı](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-metrics-debugging-flow.png)

## <a name="step-3-compare-the-abnormal-job-to-a-normal-job"></a>3. adım: bir normal iş anormal iş Karşılaştır

Tüm yinelenen işlerin ilgili iş görünümünde ekranın alt kısmındaki iş listesi kullanılarak gönderilen bulabilirsiniz. Daha fazla Öngörüler ve olası çözümler bulmak için anormal iş sağ tıklayın. Anormal iş önceki normal bir ile karşılaştırmak için iş fark görünümünü kullanın.

![İşlerini karşılaştırma için kısayol menüsü](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/compare-job.png)

Bu iki işleri arasında büyük farklar dikkat edin. Bu farklılıklar, büyük olasılıkla performans sorunlarına neden oluyor. Daha fazla denetlemek için aşağıdaki diyagramda adımları kullanın:

![İşlerini arasındaki farklar denetleme işlemi diyagramı](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-diff-debugging-flow.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Veri eğme sorunlarını gidermek](data-lake-analytics-data-lake-tools-data-skew-solutions.md)
* [Hata ayıklama başarısız U-SQL işleri için kullanıcı tanımlı C# kodu](data-lake-analytics-debug-u-sql-jobs.md)
