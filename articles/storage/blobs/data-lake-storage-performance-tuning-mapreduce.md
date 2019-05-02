---
title: Azure Data Lake depolama 2. nesil MapReduce performansı ayarlama yönergeleri | Microsoft Docs
description: Azure Data Lake depolama 2. nesil MapReduce performansı ayarlama yönergeleri
services: storage
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: 7d20f1b398c50a3b98ee862332338dbf3aaece59
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64939375"
---
# <a name="performance-tuning-guidance-for-mapreduce-on-hdinsight-and-azure-data-lake-storage-gen2"></a>HDInsight ve Azure Data Lake depolama 2. nesil MapReduce için performans ayarlama Kılavuzu

Map Reduce işleri performansını ayarlamak, dikkate almanız gereken faktörler anlayın. Bu makale, bir dizi performans ayarlama yönergeleri kapsar.

## <a name="prerequisites"></a>Önkoşullar

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Azure Data Lake depolama Gen2 hesap**. Bir oluşturma hakkında yönergeler için bkz: [hızlı başlangıç: Bir Azure Data Lake depolama Gen2'ye depolama hesabı oluşturma](data-lake-storage-quickstart-create-account.md).
* **Azure HDInsight kümesinde** bir Data Lake depolama Gen2 hesabına erişim. Bkz: [kullanımı Azure Data Lake depolama Gen2 Azure HDInsight ile kümeleri](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2)
* **HDInsight üzerinde MapReduce kullanarak**.  Daha fazla bilgi için [, HDInsight üzerinde Hadoop MapReduce kullanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-use-mapreduce)
* **Performans ayarlama yönergeleri Data Lake depolama Gen2**.  Genel performans için bkz [veri Lake depolama Gen2 performans ayarlama Kılavuzu](data-lake-storage-performance-tuning-guidance.md)

## <a name="parameters"></a>Parametreler

MapReduce işleri çalıştırırken, Data Lake depolama Gen2 performansı artırmak için yapılandırma parametreleri şunlardır:

* **Mapreduce.Map.Memory.MB** – her Eşleştiricisi için ayrılacak bellek miktarı
* **Mapreduce.job.Maps** – iş başına harita görev sayısı
* **Mapreduce.reduce.Memory.MB** – her Azaltıcı için ayrılacak bellek miktarı
* **Mapreduce.job.reduces** – iş başına azaltın görev sayısı

**Mapreduce.Map.Memory / Mapreduce.reduce.memory** bu sayı ne kadar bellek eşlemesi için gereken temel ayarlanması gereken ve/veya görev azaltın.  Varsayılan değerlerini mapreduce.map.memory mapreduce.reduce.memory ve Ambari Yarn yapılandırma görüntülenebilir.  Ambari, YARN için gidin ve yapılandırmaları sekmesini görüntüleyin.  YARN bellek görüntülenir.  

**Mapreduce.job.Maps / Mapreduce.job.reduces** bu azaltıcının veya genişletin oluşturulacak en fazla sayısını belirler.  MapReduce iş için kaç azaltıcının oluşturulacak bölmelerini sayısını belirler.  Bu nedenle, istenen azaltıcının sayısından daha az bölmelerini olup olmadığını istenenden daha az azaltıcının alabilirsiniz.       

## <a name="guidance"></a>Rehber

> [!NOTE]
> Bu belgedeki bilgiler, uygulamanız, küme üzerinde çalışan tek bir uygulama olduğunu varsayar.

**1. adım: Çalışan işlerin sayısını belirleme**

Varsayılan olarak, işiniz için tüm küme MapReduce kullanın.  Kümenin daha az kullanılabilir kapsayıcı sayısından daha az azaltıcının kullanarak kullanabilirsiniz.        

**2. adım: Mapreduce.Map.Memory/mapreduce.reduce.Memory ayarlayın**

Eşleme için bellek boyutu ve görevleri, belirli bir projede bağımlı olur.  Eşzamanlılığı artırmak istiyorsanız bellek boyutunu küçültebilirsiniz.  Eşzamanlı olarak çalışan görevlerin sayısını kapsayıcıların sayısına bağlıdır.  Eşleyici veya azaltıcı başına bellek miktarını azaltarak, daha fazla kapsayıcı, daha fazla azaltıcının veya eşzamanlı olarak çalıştırılmasını genişletin etkinleştiren oluşturulabilir.  Çok fazla bellek miktarını azaltarak, belleğin tükenmek üzere bazı işlemler neden olabilir.  İş çalışırken bir yığın hata alırsanız, Eşleyici veya azaltıcı başına bellek yükseltmeniz gerekir.  Daha fazla kapsayıcı ekleme ekleyeceksiniz dikkate almanız gereken potansiyel olarak performansı düşürebilir ek her kapsayıcı için fazladan ek yükü.  Başka bir alternatif, kümenizdeki düğümlerin sayısını artırmak ya da daha büyük miktarda bellek sahip bir küme kullanarak daha fazla bellek almaktır.  Daha fazla bellek, daha fazla eşzamanlılık başka bir deyişle, kullanılacak, daha fazla kapsayıcı olanak sağlar.  

**3. adım: Toplam YARN bellek belirleme**

Mapreduce.job.Maps/mapreduce.job.reduces ayarlamak için kullanılabilir toplam YARN bellek miktarını düşünmelisiniz.  Bu bilgiler, Ambari içinde kullanılabilir.  YARN için gidin ve yapılandırmaları sekmesini görüntüleyin.  YARN bellek Bu pencerede görüntülenir.  Toplam YARN bellek almak için kümenizdeki düğüm sayısını YARN bellekle çarpmalıdır.

    Total YARN memory = nodes * YARN memory per node

Ardından bir boş küme kullanıyorsanız, bellek toplam YARN bellek, kümeniz olabilir.  Diğer uygulamaları bellek kullanıyorsanız, kullanmak istediğiniz kapsayıcı sayısı için azaltıcının veya genişletin sayısını azaltarak kümenizin belleğin bir kısmını yalnızca kullanmayı seçebilirsiniz.  

**4. adım: YARN kapsayıcıları sayısını hesaplayın**

YARN kapsayıcıları eşzamanlılık iş için kullanılabilir miktarını gerektirir.  Toplam YARN bellek alın ve tarafından mapreduce.map.memory bölen.  

    # of YARN containers = total YARN memory / mapreduce.map.memory

**5. adım: Mapreduce.job.Maps/mapreduce.job.reduces ayarlayın**

Mapreduce.job.Maps/mapreduce.job.reduces en az biri olarak ayarlayın kullanılabilir kapsayıcı sayısı.  Denemeler yapabilir ve azaltıcının daha iyi performans elde olmadığını görmek için genişletin sayısını artırarak daha fazla.  Çok fazla azaltıcının sahip performansı düşebilir için daha fazla azaltıcının ek yük olacağını aklınızda bulundurun.  

YARN kapsayıcı sayısı bellekle sınırlıdır. Bu nedenle CPU zamanlama ve CPU yalıtım varsayılan olarak kapalıdır.

## <a name="example-calculation"></a>Örnek hesaplama

8 D14 düğümlerinin oluşan bir kümesini sunuyoruz ve g/ç yoğunluklu iş çalıştırılmasına istiyoruz varsayalım.  Yapmanız gerekenler hesaplamalar şunlardır:

**1. adım: Çalışan işlerin sayısını belirleme**

Bu örnekte, bizim işi çalıştıran tek bir iş olduğunu varsayalım.  

**2. adım: Mapreduce.Map.Memory/mapreduce.reduce.Memory ayarlayın**

Bu örnekte, g/ç yoğunluklu iş çalışıyor ve 3 GB bellek eşlemesi görevler için yeterli olacağına karar verin.

    mapreduce.map.memory = 3GB

**3. adım: Toplam YARN bellek belirleme**

    Total memory from the cluster is 8 nodes * 96GB of YARN memory for a D14 = 768GB
**4. adım: YARN kapsayıcı sayısı hesaplayın**

    # of YARN containers = 768GB of available memory / 3 GB of memory =   256

**5. adım: Mapreduce.job.Maps/mapreduce.job.reduces ayarlayın**

    mapreduce.map.jobs = 256

## <a name="examples-to-run"></a>Örnekleri çalıştırmak için

MapReduce Data Lake depolama Gen2'de nasıl çalıştığını göstermek için aşağıdaki ayarlara sahip bir kümede çalışan bazı örnek kodlar aşağıda verilmiştir:

* 16 düğüm D14v2
* HDI 3.6 çalışan Hadoop kümesi

İçin bir başlangıç noktası, MapReduce Teragen Terasort ve Teravalidate çalıştırmak için bazı örnek komutlar aşağıdadır.  Bu komutlar, kaynaklar temelinde ayarlayabilirsiniz.

**Teragen**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapreduce.job.maps=2048 -Dmapreduce.map.memory.mb=3072 10000000000 abfs://example/data/1TB-sort-input

**Terasort**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapreduce.job.maps=2048 -Dmapreduce.map.memory.mb=3072 -Dmapreduce.job.reduces=512 -Dmapreduce.reduce.memory.mb=3072 abfs://example/data/1TB-sort-input abfs://example/data/1TB-sort-output

**Teravalidate**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapreduce.job.maps=512 -Dmapreduce.map.memory.mb=3072 abfs://example/data/1TB-sort-output abfs://example/data/1TB-sort-validate
