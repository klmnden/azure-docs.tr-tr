---
title: "Azure Data Lake Store'a Hive performans yönergeleri ayarlama | Microsoft Docs"
description: "Azure Data Lake Store'a Hive performans kuralları ayarlama"
services: data-lake-store
documentationcenter: 
author: stewu
manager: amitkul
editor: stewu
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/19/2016
ms.author: stewu
ms.openlocfilehash: e10bf8f7cbae2b81d22823ff74fe652c6bcb2da3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="performance-tuning-guidance-for-hive-on-hdinsight-and-azure-data-lake-store"></a>Performans Kılavuzu Hive Hdınsight ve Azure Data Lake Store için ayarlama

Varsayılan ayarları birçok farklı kullanım örnekleri arasında iyi bir performans sağlamak üzere ayarlanmış.  G/ç yoğun sorgularında Hive ADLS ile daha iyi performans almak için ayarlanabilecek.  

## <a name="prerequisites"></a>Ön koşullar

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Azure Data Lake Store hesabı**. Bir oluşturma hakkında yönergeler için bkz: [Azure Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-portal.md)
* **Azure Hdınsight kümesi** bir Data Lake Store hesabına erişim. Bkz: [Data Lake Store ile bir Hdınsight kümesi oluşturmayı](data-lake-store-hdinsight-hadoop-use-portal.md). Küme için Uzak Masaüstü etkinleştirdiğinizden emin olun.
* **Hdınsight'ta Hive çalıştıran**.  [Hive kullanma hdınsight'ta] Hdınsight'ta Hive işleri çalıştırma hakkında bilgi için bkz (https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-hive)
* **Performans ayarlama yönergeleri ADLS**.  Genel performans için bkz [Data Lake deposu performans rehberi ayarlama](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance)

## <a name="parameters"></a>Parametreler

ADLS performansı için ince ayar için en önemli ayarları şunlardır:

* **Hive.tez.Container.size** – her görevleri tarafından kullanılan bellek miktarı

* **Tez.Grouping.Min boyutu** – minimum boyutu her Eşleyici

* **Tez.Grouping.max boyutu** – maksimum boyutu her Eşleyici

* **Hive.Exec.reducer.bytes.Per.reducer** – her reducer boyutu

**Hive.tez.Container.size** -kapsayıcı boyutu her görev için kullanılabilir bellek miktarını belirler.  Eşzamanlılık kovanında denetlemek için ana giriş budur.  

**Tez.Grouping.Min boyutu** – Bu parametre her Eşleyici minimum boyutu ayarlamanıza olanak tanır.  Tez seçer mappers sayısı bu parametre değerinden daha küçükse, Tez Burada ayarlanan değeri kullanır.  

**Tez.Grouping.max boyutu** – parametresi, her Eşleyici en büyük boyutunu ayarlamanızı sağlar.  Tez seçer mappers sayısı bu parametre değerinden büyükse, Tez Burada ayarlanan değeri kullanır.  

**Hive.Exec.reducer.bytes.Per.reducer** – Bu parametre her reducer boyutunu ayarlar.  Varsayılan olarak, her reducer 256 MB'tır.  

## <a name="guidance"></a>Rehber

**Hive.Exec.reducer.bytes.Per.reducer ayarlamak** – veri sıkıştırılmamış olduğunda varsayılan değer iyi çalışır.  Sıkıştırılmış veri boyutu reducer azaltmanız gerekir.  

**Hive.tez.Container.size ayarlamak** – her bir düğümündeki bellek yarn.nodemanager.resource.memory mb belirtilir ve HDI kümesi üzerinde varsayılan olarak doğru bir şekilde ayarlamanız gerekir.  YARN içinde uygun bellek ayarlama hakkında ek bilgi için bkz [sonrası](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-hive-out-of-memory-error-oom).

G/ç yoğun iş yükleri, Tez kapsayıcı boyutu azaltarak daha fazla paralellik yararlı olabilir. Bu kullanıcı eşzamanlılık artıran daha fazla kapsayıcı sağlar.  Ancak, bazı Hive sorguları önemli miktarda belleği (örneğin MapJoin) gerektirir.  Görev yeterli belleğe sahip değil bir yetersiz bellek özel durumu çalışma zamanı sırasında alırsınız.  Yetersiz bellek özel durumları alırsanız, bellek artırmanız gerekir.   

Çalışan görevler veya paralellik eşzamanlı sayısını, toplam YARN bellek tarafından sınırlanmış.  YARN kapsayıcı sayısı, kaç tane eş zamanlı görevleri çalıştırabilir benimsendiği belirler.  Düğüm başına YARN bellek bulmak için Ambari gidebilirsiniz.  YARN için gidin ve yapılandırmalar sekmesini görüntüleyin.  YARN bellek Bu pencerede görüntülenir.  

        Total YARN memory = nodes * YARN memory per node
        # of YARN containers = Total YARN memory / Tez container size
ADLS kullanarak performansı iyileştirme anahtarını mümkün olduğunca eşzamanlılık artırmaktır.  Tez ayarlayın gerekmez böylece oluşturulması gereken görevlerin sayısını otomatik olarak hesaplar.   

## <a name="example-calculation"></a>Örnek hesaplama

Bir 8 düğüm D14 küme sahip varsayalım.  

    Total YARN memory = nodes * YARN memory per node
    Total YARN memory = 8 nodes * 96GB = 768GB
    # of YARN containers = 768GB / 3072MB = 256

## <a name="limitations"></a>Sınırlamalar
**ADLS azaltma** 

ADLS tarafından sağlanan bant genişliği sınırlarını isabet UIf, görev hataları görmeye başlarsınız. Bu görev günlükleri gözlemci azaltma hatalar nedeniyle tanımlanamadı.  Tez kapsayıcı boyutu artırarak paralellik düşürebilir.  Daha fazla eşzamanlılık işiniz için ihtiyacınız varsa, lütfen bizimle iletişime geçin.   

Kısıtlanan durumunda denetlemek için hata ayıklama istemci tarafında günlüğünü etkinleştirmeniz gerekir. İşte nasıl bunu yapabilirsiniz:

1. Hive config log4j özelliklerinde aşağıdaki özelliği yerleştirin. Bu Ambari görünümünden yapılabilir: log4j.logger.com.microsoft.azure.datalake.store=DEBUG tüm düğümleri/hizmet yapılandırma etkili olması yeniden başlatın.

2. Kısıtlanan HTTP 429 hata kodunu hive günlük dosyasında görürsünüz. Hive günlük dosyası içinde /tmp/,&lt;kullanıcı&gt;/hive.log

## <a name="further-information-on-hive-tuning"></a>Hive ayarlama hakkında daha fazla bilgi

Hive sorgularınızı ince ayar yardımcı olacak birkaç Web günlükleri şunlardır:
* [Hdınsight'ta Hadoop için Hive sorguları en iyi duruma getirme](https://azure.microsoft.com/en-us/documentation/articles/hdinsight-hadoop-optimize-hive-query/)
* [Hive sorgusu performans sorunlarını giderme](https://blogs.msdn.microsoft.com/bigdatasupport/2015/08/13/troubleshooting-hive-query-performance-in-hdinsight-hadoop-cluster/)
* [Konuşma göz atın üzerinde hdınsight'ta Hive en iyi duruma getirme](https://channel9.msdn.com/events/Machine-Learning-and-Data-Sciences-Conference/Data-Science-Summit-2016/MSDSS25)
