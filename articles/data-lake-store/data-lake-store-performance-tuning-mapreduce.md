---
title: Azure Data Lake depolama Gen1 MapReduce performansı ayarlama yönergeleri | Microsoft Docs
description: Azure Data Lake depolama Gen1 MapReduce performansı ayarlama yönergeleri
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
ms.openlocfilehash: b9e5d034db4711384d2ac8a1083da5c93ea11900
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61437251"
---
# <a name="performance-tuning-guidance-for-mapreduce-on-hdinsight-and-azure-data-lake-storage-gen1"></a>HDInsight ve Azure Data Lake depolama Gen1 MapReduce için performans ayarlama Kılavuzu

## <a name="prerequisites"></a>Önkoşullar

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Azure Data Lake depolama Gen1 hesap**. Bir oluşturma hakkında yönergeler için bkz: [Azure Data Lake depolama Gen1 ile çalışmaya başlama](data-lake-store-get-started-portal.md)
* **Azure HDInsight kümesinde** bir Data Lake depolama Gen1 hesabına erişim. Bkz: [ile Data Lake depolama Gen1 bir HDInsight kümesi oluşturma](data-lake-store-hdinsight-hadoop-use-portal.md). Küme için Uzak Masaüstü etkinleştirdiğinizden emin olun.
* **HDInsight üzerinde MapReduce kullanarak**.  Daha fazla bilgi için [, HDInsight üzerinde Hadoop MapReduce kullanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-use-mapreduce)
* **Performans ayarlama yönergeleri Data Lake depolama Gen1**.  Genel performans için bkz [Data Lake depolama Gen1 performans ayarlama Kılavuzu](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-performance-tuning-guidance)

## <a name="parameters"></a>Parametreler

MapReduce işleri çalıştırırken, Data Lake depolama Gen1 performansı artırmak için yapılandırabileceğiniz en önemli parametreleri şunlardır:

* **Mapreduce.Map.Memory.MB** – her Eşleştiricisi için ayrılacak bellek miktarı
* **Mapreduce.job.Maps** – iş başına harita görev sayısı
* **Mapreduce.reduce.Memory.MB** – her Azaltıcı için ayrılacak bellek miktarı
* **Mapreduce.job.reduces** – iş başına azaltın görev sayısı

**Mapreduce.Map.Memory / Mapreduce.reduce.memory** bu sayı ne kadar bellek eşlemesi için gereken temel ayarlanması gereken ve/veya görev azaltın.  Varsayılan değerlerini mapreduce.map.memory mapreduce.reduce.memory ve Ambari Yarn yapılandırma görüntülenebilir.  Ambari, YARN için gidin ve yapılandırmaları sekmesini görüntüleyin.  YARN bellek görüntülenir.  

**Mapreduce.job.Maps / Mapreduce.job.reduces** bu azaltıcının veya genişletin oluşturulacak en fazla sayısını belirler.  MapReduce iş için kaç azaltıcının oluşturulacak bölmelerini sayısını belirler.  Bu nedenle, istenen azaltıcının sayısından daha az bölmelerini olup olmadığını istenenden daha az azaltıcının alabilirsiniz.       

## <a name="guidance"></a>Rehber

**1. adım: Çalışan işlerin sayısını belirlemek** -varsayılan olarak, MapReduce, işiniz için tüm küme kullanır.  Kümenin daha az kullanılabilir kapsayıcı sayısından daha az azaltıcının kullanarak kullanabilirsiniz.  Bu belgedeki bilgiler, uygulamanız, küme üzerinde çalışan tek bir uygulama olduğunu varsayar.      

**2. adım: Mapreduce.Map.Memory/mapreduce.reduce.Memory ayarlamak** – eşleme için bellek boyutu ve görevleri, belirli bir projede bağımlı olur.  Eşzamanlılığı artırmak istiyorsanız bellek boyutunu küçültebilirsiniz.  Eşzamanlı olarak çalışan görevlerin sayısını kapsayıcıların sayısına bağlıdır.  Eşleyici veya azaltıcı başına bellek miktarını azaltarak, daha fazla kapsayıcı, daha fazla azaltıcının veya eşzamanlı olarak çalıştırılmasını genişletin etkinleştiren oluşturulabilir.  Çok fazla bellek miktarını azaltarak, belleğin tükenmek üzere bazı işlemler neden olabilir.  İş çalışırken bir yığın hata alırsanız, Eşleyici veya azaltıcı başına bellek yükseltmeniz gerekir.  Daha fazla kapsayıcı ekleme ekleyeceksiniz dikkate almanız gereken potansiyel olarak performansı düşürebilir ek her kapsayıcı için fazladan ek yükü.  Başka bir alternatif, kümenizdeki düğümlerin sayısını artırmak ya da daha büyük miktarda bellek sahip bir küme kullanarak daha fazla bellek almaktır.  Daha fazla bellek, daha fazla eşzamanlılık başka bir deyişle, kullanılacak, daha fazla kapsayıcı olanak sağlar.  

**3. adım: Toplam YARN bellek belirlemek** - mapreduce.job.maps/mapreduce.job.reduces, ayarlamak için kullanılabilir toplam YARN bellek miktarını göz önünde bulundurmanız gerekir.  Bu bilgiler, Ambari içinde kullanılabilir.  YARN için gidin ve yapılandırmaları sekmesini görüntüleyin.  YARN bellek Bu pencerede görüntülenir.  Toplam YARN bellek almak için kümenizdeki düğüm sayısını YARN bellekle çarpmalıdır.

    Total YARN memory = nodes * YARN memory per node
Ardından bir boş küme kullanıyorsanız, bellek toplam YARN bellek, kümeniz olabilir.  Diğer uygulamaları bellek kullanıyorsanız, kullanmak istediğiniz kapsayıcı sayısı için azaltıcının veya genişletin sayısını azaltarak kümenizin belleğin bir kısmını yalnızca kullanmayı seçebilirsiniz.  

**4. adım: YARN kapsayıcıları sayısını hesaplamak** – YARN kapsayıcıları dikte miktarını eşzamanlılık iş için kullanılabilir.  Toplam YARN bellek alın ve tarafından mapreduce.map.memory bölen.  

    # of YARN containers = total YARN memory / mapreduce.map.memory

**5. adım: Mapreduce.job.Maps/mapreduce.job.reduces ayarlamak** mapreduce.job.maps/mapreduce.job.reduces en az biri olarak ayarlayın kullanılabilir kapsayıcı sayısı.  Denemeler yapabilir ve azaltıcının daha iyi performans elde olmadığını görmek için genişletin sayısını artırarak daha fazla.  Çok fazla azaltıcının sahip performansı düşebilir için daha fazla azaltıcının ek yük olacağını aklınızda bulundurun.  

YARN kapsayıcı sayısı bellekle sınırlıdır. Bu nedenle CPU zamanlama ve CPU yalıtım varsayılan olarak kapalıdır.

## <a name="example-calculation"></a>Örnek hesaplama

Şu anda 8 D14 düğümlerinin oluşan bir kümeniz varsa ve bir g/ç kullanımı yoğun işini çalıştırmak istediğiniz varsayalım.  Yapmanız gerekenler hesaplamalar şunlardır:

**1. adım: Çalışan işlerin sayısını belirlemek** -Bizim örneğimizde, biz bizim işi yalnızca tek bir çalışan olduğunu varsayın.  

**2. adım: Mapreduce.Map.Memory/mapreduce.reduce.Memory ayarlamak** – bizim örneğimizin g/ç yoğunluklu iş çalıştırıyorsanız ve 3 GB bellek eşlemesi görevler için yeterli olacağına karar verin.

    mapreduce.map.memory = 3GB
**3. adım: Toplam YARN bellek belirleme**

    total memory from the cluster is 8 nodes * 96GB of YARN memory for a D14 = 768GB
**4. adım: YARN kapsayıcı sayısı hesaplayın**

    # of YARN containers = 768GB of available memory / 3 GB of memory =   256

**5. adım: Mapreduce.job.Maps/mapreduce.job.reduces ayarlayın**

    mapreduce.map.jobs = 256

## <a name="limitations"></a>Sınırlamalar

**Data Lake depolama Gen1 azaltma**

Çok kiracılı bir hizmet Data Lake depolama Gen1 hesap düzeyi bant genişliği sınırlarını ayarlar.  Bu sınırına ulaşırsanız, görev hataları görmeye başlarsınız. Bu görev günlüklerde gözlemci azaltma hataları tarafından tanımlanabilir.  Daha fazla bant genişliği işiniz için ihtiyacınız varsa lütfen bizimle iletişime geçin.   

Kaynaklarınızın azaltılıp olmadığını denetlemek için hata ayıklama günlüğü istemci tarafında etkinleştirmeniz gerekir. İşte nasıl bunu yapabilirsiniz:

1. Ambari log4j özelliklerinde aşağıdaki özellik put > YARN > yapılandırma > yarn log4j Gelişmiş: log4j.logger.com.microsoft.azure.datalake.store=DEBUG

2. Tüm düğümleri/hizmet için yapılandırmanın etkili olması yeniden başlatın.

3. Kaynaklarınızın azaltılıp YARN günlüğü dosyası HTTP 429 hata kodunu görürsünüz. YARN Günlüğü dosyasıdır /tmp/ içinde&lt;kullanıcı&gt;/yarn.log

## <a name="examples-to-run"></a>Örnekleri çalıştırma

MapReduce Data Lake depolama Gen1 üzerinde nasıl çalıştığını göstermek için aşağıdaki ayarlara sahip bir kümede çalışan bazı örnek kodlar aşağıda verilmiştir:

* 16 düğüm D14v2
* HDI 3.6 çalışan Hadoop kümesi

İçin bir başlangıç noktası, MapReduce Teragen Terasort ve Teravalidate çalıştırmak için bazı örnek komutlar aşağıdadır.  Bu komutlar, kaynaklar temelinde ayarlayabilirsiniz.

**Teragen**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapreduce.job.maps=2048 -Dmapreduce.map.memory.mb=3072 10000000000 adl://example/data/1TB-sort-input

**Terasort**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapreduce.job.maps=2048 -Dmapreduce.map.memory.mb=3072 -Dmapreduce.job.reduces=512 -Dmapreduce.reduce.memory.mb=3072 adl://example/data/1TB-sort-input adl://example/data/1TB-sort-output

**Teravalidate**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapreduce.job.maps=512 -Dmapreduce.map.memory.mb=3072 adl://example/data/1TB-sort-output adl://example/data/1TB-sort-validate
