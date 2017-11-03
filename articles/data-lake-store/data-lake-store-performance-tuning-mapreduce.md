---
title: "Azure Data Lake Store MapReduce performans yönergeleri ayarlama | Microsoft Docs"
description: "Azure Data Lake Store MapReduce performans kuralları ayarlama"
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
ms.openlocfilehash: 9528148792f083cb0e48d356e61cf61762ee954f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="performance-tuning-guidance-for-mapreduce-on-hdinsight-and-azure-data-lake-store"></a>Performans MapReduce Hdınsight ve Azure Data Lake Store için yönergeler ayarlama


## <a name="prerequisites"></a>Ön koşullar

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Azure Data Lake Store hesabı**. Bir oluşturma hakkında yönergeler için bkz: [Azure Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-portal.md)
* **Azure Hdınsight kümesi** bir Data Lake Store hesabına erişim. Bkz: [Data Lake Store ile bir Hdınsight kümesi oluşturmayı](data-lake-store-hdinsight-hadoop-use-portal.md). Küme için Uzak Masaüstü etkinleştirdiğinizden emin olun.
* **MapReduce kullanarak**.  Daha fazla bilgi için bkz: [hdınsight'ta Hadoop içinde kullanım MapReduce](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-mapreduce)  
* **Performans ayarlama yönergeleri ADLS**.  Genel performans için bkz [Data Lake deposu performans rehberi ayarlama](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance)  

## <a name="parameters"></a>Parametreler

MapReduce işleri çalıştırırken ADLS performansını artırmak için yapılandırabileceğiniz en önemli Parametreler şunlardır:

* **Mapreduce.Map.Memory.MB** – her Eşleyici ayrılacak bellek miktarı
* **Mapreduce.job.Maps** – harita görev iş başına sayısı
* **Mapreduce.reduce.Memory.MB** – her reducer ayrılacak bellek miktarı
* **Mapreduce.job.reduces** – Küçült görevleri iş başına sayısı

**Mapreduce.Map.Memory / Mapreduce.reduce.memory** bu sayı ne kadar bellek eşlemesi için gereken temel ayarlanması gereken ve/veya görev azaltın.  Varsayılan değerleri mapreduce.map.memory ve mapreduce.reduce.memory Ambari Yarn yapılandırma görüntülenebilir.  Ambari, YARN için gidin ve yapılandırmalar sekmesini görüntüleyin.  YARN bellek görüntülenir.  

**Mapreduce.job.Maps / Mapreduce.job.reduces** bu mappers veya reducers oluşturulacak en fazla sayısını belirler.  MapReduce işi için kaç tane mappers oluşturulacak bölmelerini sayısını belirler.  Bu nedenle, istenen mappers sayısından daha az bölmelerini olup olmadığını istenenden daha az mappers alabilirsiniz.       

## <a name="guidance"></a>Rehber

**1. adım: çalışan işlerin sayısını belirlemek** -varsayılan olarak, MapReduce, işiniz için tüm küme kullanır.  Kümenin daha az kullanılabilir kapsayıcıları sayısından daha az mappers kullanarak kullanabilirsiniz.  Bu belgedeki bilgiler, uygulamanız, küme üzerinde çalışan tek uygulama olduğunu varsayar.      

**2. adım: mapreduce.map.memory/mapreduce.reduce.memory ayarlama** – eşlemesi için bellek boyutu ve azaltmak görevler belirli işinizde bağımlı olacaktır.  Eşzamanlılık artırmak istiyorsanız bellek boyutunu azaltabilirsiniz.  Görevler eşzamanlı olarak çalışan sayısı kapsayıcıları sayısına bağlıdır.  Eşleyici veya reducer başına bellek miktarını azaltarak daha fazla kapsayıcıları, daha fazla mappers veya aynı anda çalışmasına reducers etkinleştirmek oluşturulabilir.  Çok fazla bellek miktarını azaltarak belleğin tükenmek üzere bazı işlemler neden olabilir.  İşinizi çalıştırırken bir yığın hata alırsanız, Eşleyici veya reducer başına bellek artırmanız gerekir.  Daha fazla kapsayıcı ekleme ekleyeceksiniz dikkate almanız gereken potansiyel olarak performansı düşürebilir her ek kapsayıcısı için fazladan genel gider.  Başka bir alternatif, yüksek miktarda bellek sahip bir küme kullanarak veya kümenizdeki düğümlerin sayısını artırarak daha fazla bellek elde etmektir.  Daha fazla bellek, daha fazla eşzamanlılık başka bir deyişle, kullanılacak, daha fazla kapsayıcı olanak sağlar.  

**3. adım: Toplam YARN bellek belirleme** - mapreduce.job.maps/mapreduce.job.reduces, ayarlamak için kullanılabilir toplam YARN bellek miktarını göz önünde bulundurmanız gerekir.  Bu bilgiler, Ambari içinde kullanılabilir.  YARN için gidin ve yapılandırmalar sekmesini görüntüleyin.  YARN bellek Bu pencerede görüntülenir.  Toplam YARN bellek almak için kümedeki düğümlerin sayısını YARN bellekle çarpın.

    Total YARN memory = nodes * YARN memory per node
Boş bir küme kullanıyorsanız, bellek, kümeniz için toplam YARN bellek olabilir.  Diğer uygulamalar bellek kullanıyorsanız, kullanmak istediğiniz kapsayıcı sayısını mappers veya reducers sayısını azaltarak kümenizin belleğin bir kısmını yalnızca kullanmayı seçebilirsiniz.  

**4. adım: YARN kapsayıcıları sayısını hesaplayın** – YARN kapsayıcıları dikte eşzamanlılık iş için kullanılabilir miktarı.  Toplam YARN bellek alın ve mapreduce.map.memory tarafından bölün.  

    # of YARN containers = total YARN memory / mapreduce.map.memory

**5. adım: mapreduce.job.maps/mapreduce.job.reduces ayarlama** mapreduce.job.maps/mapreduce.job.reduces için en az biri olarak ayarlamak kullanılabilir kapsayıcıları sayısı.  Denemeler yapabilirsiniz mappers ve reducers daha iyi performans elde olmadığını görmek için sayısını artırarak daha fazla.  Daha fazla mappers çok fazla mappers sahip performansı düşebilir şekilde ek yükü sahip olduğunuzu göz önünde bulundurun.  

YARN kapsayıcı sayısı bellek tarafından sınırlandığı için CPU zamanlama ve CPU yalıtım varsayılan olarak kapalıdır.

## <a name="example-calculation"></a>Örnek hesaplama

Şu anda 8 D14 düğümden oluşan bir kümeniz olduğuna ve g/ç yoğun iş çalıştırmak istediğiniz varsayalım.  Yapmanız gerektiğini hesaplamalar şunlardır:

**1. adım: çalışan işlerin sayısını belirlemek** -Bizim örneğimizde, biz bizim işi yalnızca bir çalışan olduğunu varsayın.  

**2. adım: mapreduce.map.memory/mapreduce.reduce.memory ayarlama** – bizim Örneğin, bir g/ç yoğun iş çalıştırıyorsanız ve 3 GB bellek eşlemesi görevler için yeterli olacağına karar verin.

    mapreduce.map.memory = 3GB
**3. adım: Toplam YARN bellek belirleme**

    total memory from the cluster is 8 nodes * 96GB of YARN memory for a D14 = 768GB
**4. adım: Hesaplama YARN kapsayıcıları sayısı**

    # of YARN containers = 768GB of available memory / 3 GB of memory =   256

**5. adım: mapreduce.job.maps/mapreduce.job.reduces ayarlama**

    mapreduce.map.jobs = 256

## <a name="limitations"></a>Sınırlamalar

**ADLS azaltma**

Çok kiracılı bir hizmet ADLS hesabına düzey bant genişliği sınırlarını ayarlar.  Bu sınırlar isabet, görev hataları görmek başlatılır. Bu görev günlüklerine gözlemci azaltma hataları tanımlanabilir.  Daha fazla bant genişliği, işiniz için ihtiyacınız varsa, lütfen bizimle iletişime geçin.   

Kısıtlanan durumunda denetlemek için hata ayıklama istemci tarafında günlüğünü etkinleştirmeniz gerekir. İşte nasıl bunu yapabilirsiniz:

1. Ambari log4j özelliklerinde aşağıdaki özelliği put > YARN > Config > yarn log4j Gelişmiş: log4j.logger.com.microsoft.azure.datalake.store=DEBUG

2. Tüm düğümleri/hizmet yapılandırma etkili olması yeniden başlatın.

3. Kısıtlanan HTTP 429 hata kodunu YARN günlüğü dosyasında görürsünüz. İçinde /tmp/ YARN Günlüğü dosyasıdır&lt;kullanıcı&gt;/yarn.log

## <a name="examples-to-run"></a>Örnekleri çalıştırmak için

MapReduce Azure Data Lake Store üzerinde nasıl çalışacağını tanıtmak için aşağıdaki ayarlara sahip bir kümede çalışan bazı örnek kod aşağıda verilmiştir:

* 16 düğüm D14v2
* Hadoop kümesi HDI 3.6 çalıştırma

İçin bir başlangıç noktası, MapReduce Teragen, Terasort ve Teravalidate çalıştırmak için bazı örnek komutları şunlardır.  Bu komutlar, kaynaklara göre ayarlayabilirsiniz.

**Teragen**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapreduce.job.maps=2048 -Dmapreduce.map.memory.mb=3072 10000000000 adl://example/data/1TB-sort-input

**Terasort**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapreduce.job.maps=2048 -Dmapreduce.map.memory.mb=3072 -Dmapreduce.job.reduces=512 -Dmapreduce.reduce.memory.mb=3072 adl://example/data/1TB-sort-input adl://example/data/1TB-sort-output

**Teravalidate**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapreduce.job.maps=512 -Dmapreduce.map.memory.mb=3072 adl://example/data/1TB-sort-output adl://example/data/1TB-sort-validate
