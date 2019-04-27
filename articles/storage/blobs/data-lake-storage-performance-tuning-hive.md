---
title: Azure Data Lake depolama Gen2 Hive performans ayarlama yönergeleri | Microsoft Docs
description: Azure Data Lake depolama Gen2 Hive performans ayarlama yönergeleri
services: storage
author: swums
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: stewu
ms.openlocfilehash: 9e5570b937fe97cc9b6ccd9ac804a35ff8e07d6f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60805550"
---
# <a name="performance-tuning-guidance-for-hive-on-hdinsight-and-azure-data-lake-storage-gen2"></a>HDInsight ve Azure Data Lake depolama Gen2 Hive için performans ayarlama Kılavuzu

Varsayılan ayarlar, birçok farklı kullanım örnekleri arasında iyi bir performans sağlamak üzere ayarlandı.  G/ç kullanımı yoğun sorgular için Hive ile Azure Data Lake depolama Gen2 daha iyi performans elde etmek için ayarlanabilecek.  

## <a name="prerequisites"></a>Önkoşullar

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Data Lake depolama Gen2 hesabı**. Bir oluşturma hakkında yönergeler için bkz: [hızlı başlangıç: Bir Azure Data Lake depolama Gen2'ye depolama hesabı oluşturma](data-lake-storage-quickstart-create-account.md)
* **Azure HDInsight kümesinde** bir Data Lake depolama Gen2 hesabına erişim. Bkz: [kullanımı Azure Data Lake depolama Gen2 Azure HDInsight ile kümeleri](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2)
* **HDInsight üzerinde Hive'ı çalıştıran**.  HDInsight üzerinde Hive işlerini çalıştırma hakkında bilgi edinmek için bkz. [HDInsight Hive kullanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-use-hive)
* **Performans ayarlama yönergeleri Data Lake depolama Gen2**.  Genel performans için bkz [veri Lake depolama Gen2 performans ayarlama Kılavuzu](data-lake-storage-performance-tuning-guidance.md)

## <a name="parameters"></a>Parametreler

Data Lake depolama Gen2 performansı için ayarlamak için en önemli ayarları şunlardır:

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
Data Lake depolama Gen2'ı kullanarak performans geliştirme için anahtar mümkün olduğunca eşzamanlılık artırmaktır.  Tez otomatik olarak ayarlamanız gerek kalmayacak şekilde oluşturulması gereken görev sayısını hesaplar.   

## <a name="example-calculation"></a>Örnek hesaplama

Bir 8 düğüm D14 kümesine sahip olursunuz varsayalım.  

    Total YARN memory = nodes * YARN memory per node
    Total YARN memory = 8 nodes * 96GB = 768GB
    # of YARN containers = 768GB / 3072MB = 256

## <a name="further-information-on-hive-tuning"></a>Hive ayarlama hakkında daha fazla bilgi

Hive sorgularınızı kolayca yardımcı olacak birkaç blogları şunlardır:
* [HDInsight, Hadoop için Hive sorgularını en iyi duruma getirme](https://azure.microsoft.com/documentation/articles/hdinsight-hadoop-optimize-hive-query/)
* [Hive sorgu performansı sorunlarını giderme](https://blogs.msdn.microsoft.com/bigdatasupport/2015/08/13/troubleshooting-hive-query-performance-in-hdinsight-hadoop-cluster/)
* [Konuşma ıgnite üzerinde Hive HDInsight üzerinde iyileştirme](https://channel9.msdn.com/events/Machine-Learning-and-Data-Sciences-Conference/Data-Science-Summit-2016/MSDSS25)
