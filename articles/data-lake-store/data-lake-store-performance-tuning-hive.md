---
title: Azure Data Lake depolama Gen1 Hive performans ayarlama yönergeleri | Microsoft Docs
description: Azure Data Lake depolama Gen1 Hive performans ayarlama yönergeleri
services: data-lake-store
documentationcenter: ''
author: stewu
manager: amitkul
editor: stewu
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: stewu
ms.openlocfilehash: 433c6b7d70cea9406b67d65e23cc357939cb5aa0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61437285"
---
# <a name="performance-tuning-guidance-for-hive-on-hdinsight-and-azure-data-lake-storage-gen1"></a>HDInsight ve Azure Data Lake depolama Gen1 Hive için performans ayarlama Kılavuzu

Varsayılan ayarlar, birçok farklı kullanım örnekleri arasında iyi bir performans sağlamak üzere ayarlandı.  G/ç kullanımı yoğun sorgular için Hive ile Azure Data Lake depolama Gen1 daha iyi performans elde etmek için ayarlanabilecek.  

## <a name="prerequisites"></a>Önkoşullar

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Data Lake depolama Gen1 hesabı**. Bir oluşturma hakkında yönergeler için bkz: [Azure Data Lake depolama Gen1 ile çalışmaya başlama](data-lake-store-get-started-portal.md)
* **Azure HDInsight kümesinde** bir Data Lake depolama Gen1 hesabına erişim. Bkz: [ile Data Lake depolama Gen1 bir HDInsight kümesi oluşturma](data-lake-store-hdinsight-hadoop-use-portal.md). Küme için Uzak Masaüstü etkinleştirdiğinizden emin olun.
* **HDInsight üzerinde Hive'ı çalıştıran**.  HDInsight üzerinde Hive işlerini çalıştırma hakkında bilgi edinmek için bkz. [HDInsight Hive kullanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-use-hive)
* **Performans ayarlama yönergeleri Data Lake depolama Gen1**.  Genel performans için bkz [Data Lake depolama Gen1 performans ayarlama Kılavuzu](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-performance-tuning-guidance)

## <a name="parameters"></a>Parametreler

Data Lake depolama Gen1 performansı için ayarlamak için en önemli ayarları şunlardır:

* **Hive.tez.Container.size** – her görevleri tarafından kullanılan bellek miktarı

* **Tez.Grouping.Min boyutu** – minimum boyutu her Eşleyici

* **Tez.Grouping.max boyutu** – maksimum boyutunu her Eşleyici

* **Hive.Exec.reducer.bytes.Per.reducer** – her Azaltıcı boyutu

**Hive.tez.Container.size** -her görev için ne kadar bellek kullanılabilir kapsayıcı boyutu belirler.  Eşzamanlılık kovanında denetlemek için ana giriş budur.  

**Tez.Grouping.Min boyutu** – Bu parametre için en küçük boyut her Eşleyici ayarlamanıza olanak tanır.  Tez seçtiği azaltıcının sayısı, bu parametrenin değeri küçükse, Tez Burada ayarlanan değer kullanın.

**Tez.Grouping.max boyutu** – parametresi her Eşleyici en büyük boyutunu ayarlamanızı sağlar.  Tez seçtiği azaltıcının sayısı, bu parametrenin değeri büyükse, Tez Burada ayarlanan değer kullanın.

**Hive.Exec.reducer.bytes.Per.reducer** – Bu parametre her Azaltıcı boyutunu ayarlar.  Varsayılan olarak, her Azaltıcı 256 MB'tır.  

## <a name="guidance"></a>Rehber

**Hive.Exec.reducer.bytes.Per.reducer ayarlamak** – veri sıkıştırılmamış olduğunda varsayılan değer iyi çalışır.  Sıkıştırılmış veri Azaltıcı boyutunu azaltmanız gerekir.  

**Hive.tez.Container.size ayarlamak** – her düğümde bellek yarn.nodemanager.resource.memory mb belirtilir ve HDI kümesi üzerinde varsayılan olarak doğru şekilde ayarlanması gerekir.  YARN içinde uygun bellek ayarlama hakkında ek bilgi için bkz. Bu [sonrası](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-hive-out-of-memory-error-oom).

G/ç kullanımlı iş yükleri, Tez kapsayıcı boyutu azaltarak daha fazla paralellik yararlı olabilir. Bu kullanıcı eşzamanlılık artıran daha fazla kapsayıcı sağlar.  Ancak, bazı Hive sorguları önemli miktarda bellek (örneğin MapJoin) gerektirir.  Görev yeterli bellek yoksa çalışma zamanı sırasında yetersiz bellek özel dışı alırsınız.  Yetersiz bellek özel durumları alırsanız, bellek yükseltmeniz gerekir.   

Eş zamanlı çalışan görevler veya paralellik sayısı, toplam YARN bellek tarafından sınırlanmış.  YARN kapsayıcı sayısı, kaç tane eş zamanlı görevleri çalıştırabilir gösterecektir.  Düğüm başına YARN bellek bulmak için ambarı'na gidebilirsiniz.  YARN için gidin ve yapılandırmaları sekmesini görüntüleyin.  YARN bellek Bu pencerede görüntülenir.  

        Total YARN memory = nodes * YARN memory per node
        # of YARN containers = Total YARN memory / Tez container size
Data Lake depolama Gen1 kullanarak performans geliştirme için anahtar mümkün olduğunca eşzamanlılık artırmaktır.  Tez otomatik olarak ayarlamanız gerek kalmayacak şekilde oluşturulması gereken görev sayısını hesaplar.   

## <a name="example-calculation"></a>Örnek hesaplama

Bir 8 düğüm D14 kümesine sahip olursunuz varsayalım.  

    Total YARN memory = nodes * YARN memory per node
    Total YARN memory = 8 nodes * 96GB = 768GB
    # of YARN containers = 768GB / 3072MB = 256

## <a name="limitations"></a>Sınırlamalar

**Data Lake depolama Gen1 azaltma** 

Data Lake depolama Gen1 tarafından sağlanan bant genişliği sınırına ulaşırsanız, görev hataları görmeye başlarsınız. Bu görev günlüklerde gözlemci azaltma hataları tanımlanamadı.  Tez kapsayıcı boyutu artırarak paralellik düşürebilir.  Daha fazla eşzamanlılık işiniz için ihtiyacınız varsa lütfen bizimle iletişime geçin.

Kaynaklarınızın azaltılıp olmadığını denetlemek için hata ayıklama günlüğü istemci tarafında etkinleştirmeniz gerekir. İşte nasıl bunu yapabilirsiniz:

1. Şu özellik Hive yapılandırma log4j özelliklerinde yerleştirin. Ambari görünümünden bu yapılabilir: log4j.logger.com.microsoft.azure.datalake.store=DEBUG tüm düğümleri/hizmet için yapılandırmanın etkili olması yeniden başlatın.

2. Kaynaklarınızın azaltılıp yığın günlük dosyasının HTTP 429 hata kodunu görürsünüz. İçinde /tmp/ hive günlük dosyasıdır&lt;kullanıcı&gt;/hive.log

## <a name="further-information-on-hive-tuning"></a>Hive ayarlama hakkında daha fazla bilgi

Hive sorgularınızı kolayca yardımcı olacak birkaç blogları şunlardır:
* [HDInsight, Hadoop için Hive sorgularını en iyi duruma getirme](https://azure.microsoft.com/documentation/articles/hdinsight-hadoop-optimize-hive-query/)
* [Hive sorgu performansı sorunlarını giderme](https://blogs.msdn.microsoft.com/bigdatasupport/2015/08/13/troubleshooting-hive-query-performance-in-hdinsight-hadoop-cluster/)
* [Konuşma ıgnite üzerinde Hive HDInsight üzerinde iyileştirme](https://channel9.msdn.com/events/Machine-Learning-and-Data-Sciences-Conference/Data-Science-Summit-2016/MSDSS25)
