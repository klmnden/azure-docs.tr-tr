---
title: Azure Data Lake depolama Gen2 Spark performansı ayarlama yönergeleri | Microsoft Docs
description: Azure Data Lake depolama Gen2 Spark performansı ayarlama yönergeleri
services: storage
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: feefcf4f9f4448ab2b36c415cb745fd98277eb28
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64939329"
---
# <a name="performance-tuning-guidance-for-spark-on-hdinsight-and-azure-data-lake-storage-gen2"></a>HDInsight ve Azure Data Lake depolama Gen2 üzerinde Spark için performans ayarlama Kılavuzu

Spark üzerinde performans ayarlama, kümenizde çalışan uygulamaların sayısı göz önüne almanız gerekir.  Varsayılan olarak, 4 çalıştırabileceğiniz uygulamalar, HDI kümesi üzerinde aynı anda (Not: değiştirilebilir varsayılan ayardır).  Varsayılan ayarları geçersiz kılar ve kümenin daha fazla bilgi için bu uygulamaları kullanmak için daha az uygulamaları kullanmaya karar verebilirsiniz.  

## <a name="prerequisites"></a>Önkoşullar

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Azure Data Lake depolama Gen2 hesap**. Bir oluşturma hakkında yönergeler için bkz: [hızlı başlangıç: Bir Azure Data Lake depolama Gen2'ye depolama hesabı oluşturma](data-lake-storage-quickstart-create-account.md).
* **Azure HDInsight kümesinde** bir Data Lake depolama Gen2 hesabına erişim. Bkz: [kullanımı Azure Data Lake depolama Gen2 Azure HDInsight ile kümeleri](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2). Küme için Uzak Masaüstü etkinleştirdiğinizden emin olun.
* **Data Lake depolama Gen2'de Spark kümesi çalıştıran**.  Daha fazla bilgi için [Data Lake depolama Gen2 analiz etmek için kullanım HDInsight Spark kümesi](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-use-with-data-lake-store)
* **Performans ayarlama yönergeleri Data Lake depolama Gen2**.  Genel performans için bkz [veri Lake depolama Gen2 performans ayarlama Kılavuzu](data-lake-storage-performance-tuning-guidance.md) 

## <a name="parameters"></a>Parametreler

Spark işleri çalıştırırken, Data Lake depolama Gen2 performansı artırmak için ayarlanabilecek en önemli ayarlar şunlardır:

* **Yürütücü sayısı** -yürütülebilecek eşzamanlı görev sayısı.

* **Bellek içi Yürütücü** -her Yürütücü için ayrılmış bellek miktarı.

* **Yürütücü çekirdek** -her Yürütücü için ayrılan çekirdek sayısı.                     

**Yürütücü sayısı** Yürütücü sayısı en fazla paralel olarak çalıştırabileceğiniz görevlerin sayısını ayarlar.  Paralel olarak çalıştırılabilir görevleri gerçek sayısını bellek ve CPU kaynaklarını kümenizde kullanılabilir sınırlıdır.

**Bellek içi Yürütücü** her yürütücüsüne atanan bellek miktarıdır.  Her Yürütücü için gereken belleği işine bağlıdır.  Karmaşık işlemleri için bellek daha yüksek olması gerekir.  Okuma ve yazma gibi basit işlemler için bellek gereksinimleri daha düşük olacaktır.  Her Yürütücü için bellek miktarı Ambari görüntülenebilir.  Ambari, Spark için gidin ve yapılandırmaları sekmesini görüntüleyin.  

**Yürütücü çekirdek** bu Yürütücü çalıştırılabilir paralel iş parçacığı sayısını belirleyen Yürütücü başına kullanılan çekirdek sayısını ayarlar.  Örneğin, çekirdek Yürütücü = 2, her Yürütücü Yürütücü içinde 2 Paralel Görevler çalıştırabilirsiniz.  Yürütücü çekirdek işin bağımlı olacaktır.  Daha fazla Paralel Görevler her Yürütücü işleyebilmesi için büyük miktarda bellek görev başına g/ç yoğun iş gerektirmez.

Varsayılan olarak, iki sanal YARN çekirdek Spark HDInsight üzerinde çalışırken her fiziksel çekirdek için tanımlanır.  Bu sayı, eşzamanlılık ve bağlam birden fazla iş parçacığından değiştirme miktarı iyi bir dengesini sunar.  

## <a name="guidance"></a>Rehber

Data Lake depolama Gen2 verilerle çalışmak için analitik iş yükleri çalıştırırken Spark, Data Lake depolama Gen2 ile'en iyi performansı elde etmek için en son HDInsight sürümü kullanmanızı öneririz. Daha fazla g/ç yoğun iş olduğunda, belirli parametreleri performansını artırmak için yapılandırılabilir.  Data Lake depolama Gen2, yüksek aktarım hızını işleyebilir bir yüksek düzeyde ölçeklenebilir depolama platformudur.  Data Lake depolama Gen2'ye gelen ve eşzamanlılık g/ç için yeniden artırır, iş okuma veya yazmaları ağırlıklı olarak oluşuyorsa, performansı arttırabilir.

Yoğun g/ç işleri için eşzamanlılığı artırmak için birkaç genel yol vardır.

**1. adım: Kaç tane uygulamalarını kümenizde çalışan belirlemek** – geçerli de dahil olmak üzere kümede kaç çalıştırmaya bilmeniz gerekir.  4 vardır varsayılan değerleri ayarı varsayar her Spark için aynı anda çalıştırılan uygulamalar.  Bu nedenle yalnızca % 25 oranında kümenin her uygulama için kullanılabilir olacaktır.  Daha iyi performans elde etmek için Yürütücü sayısına değiştirerek Varsayılanları geçersiz kılabilir.  

**2. adım: Bellek içi Yürütücü ayarlamak** – ayarlamak için ilk şey Yürütücü-bellektir.  Bellek üzerinde çalıştırmak için oluşturacağınız işi bağımlı olacaktır.  Yürütücü başına daha az bellek ayırarak eşzamanlılığı artırabilir.  Yetersiz bellek özel durumları iş çalıştırdığınızda görürseniz, bu parametrenin değerini artırmanız gerekir.  Daha fazla bellek, daha büyük miktarda bellek sahip olan bir kümeye kullanarak veya kümenizin boyutunu artırmayı almak bir alternatiftir.  Daha fazla bellek, daha fazla eşzamanlılık başka bir deyişle, kullanılacak, daha fazla yürütücüler olanak sağlar.

**3. adım: Yürütücü çekirdek kümesi** – karmaşık işlemleri sahip olmaması için g/ç kullanımlı iş yükleri, İşte bu kadar çok sayıda Paralel Görevler Yürütücü başına sayısını artırmak için Yürütücü çekirdek başlatmak iyi.  Yürütücü çekirdek sayısı 4'e ayarlanması, iyi bir başlangıç noktasıdır.   

    executor-cores = 4
Yürütücü çekirdek sayısının artırılması farklı Yürütücü çekirdeğiyle deneyebilirsiniz. Bu nedenle daha fazla paralellik verir.  Daha karmaşık işlemler sahip işler için Yürütücü başına çekirdek sayısını azaltmanız gerekir.  Yürütücü çekirdek sayısı 4'ten yüksek ayarlanmış ardından çöp toplama verimsiz ve performansı düşebilir.

**4. adım: Kümenin YARN bellek miktarını belirlemek** – Ambari bu bilgi kullanılabilir.  YARN için gidin ve yapılandırmaları sekmesini görüntüleyin.  YARN bellek Bu pencerede görüntülenir.  
Not penceresinde olmakla birlikte, varsayılan YARN kapsayıcı boyutu da görebilirsiniz.  YARN kapsayıcı boyutu Yürütücü parametresi başına bellek ile aynıdır.

    Total YARN memory = nodes * YARN memory per node
**5. adım: Yürütücü sayısı hesaplayın**

**Bellek kısıtlama hesaplamak** -Yürütücü sayısı parametresi, bellek veya CPU sınırlıdır.  Bellek kısıtlama, uygulamanız için kullanılabilir YARN bellek miktarına göre belirlenir.  Toplam YARN bellek alın ve yürütücü bellekle bölen gerekir.  Kısıtlama, biz uygulamaları sayısına göre bölmek için uygulama sayısı için XML'deki ölçeklendirilmiş olması gerekiyor.

    Memory constraint = (total YARN memory / executor memory) / # of apps   
**CPU kısıtlaması hesaplamak** -CPU sınırlama bölü Yürütücü başına çekirdek sayısı, toplam sanal çekirdek olarak hesaplanır.  2 sanal çekirdek her fiziksel çekirdek için vardır.  Bellek kısıtlama benzer uygulamalar sayısına göre bölmek sahibiz.

    virtual cores = (nodes in cluster * # of physical cores in node * 2)
    CPU constraint = (total virtual cores / # of cores per executor) / # of apps
**Yürütücü sayısı Ayarla** – Yürütücü sayısı parametresi en az bellek kısıtlaması ile CPU alarak belirlenir. 

    num-executors = Min (total virtual Cores / # of cores per executor, available YARN memory / executor-memory)   
Yürütücü sayısı, daha yüksek bir sayı ayarlandığında, performans mutlaka artırmaz.  Daha fazla yürütücüler ekleme ekleyeceksiniz dikkate almanız gereken potansiyel olarak performansı düşürebilir her ek Yürütücü için fazladan ek yükü.  Yürütücü sayısı küme kaynakları tarafından sınırlanan.    

## <a name="example-calculation"></a>Örnek hesaplama

Şu anda kullandığınız 2 çalıştıran 8 D4v2 düğümlerinin oluşan bir küme varsayalım uygulamaları çalıştırmak için seçeceğiz de dahil olmak üzere.  

**1. adım: Kaç tane uygulamalarını kümenizde çalışan belirlemek** – 2 olduğunu bildiğiniz çalıştırılacak seçeceğiz de dahil olmak üzere kümenizdeki uygulamaları.  

**2. adım: Bellek içi Yürütücü ayarlamak** – Bu örnekte, 6 GB bellek Yürütücü g/ç yoğunluklu iş için yeterli olacağını olan belirleriz.  

    executor-memory = 6GB
**3. adım: Yürütücü çekirdek kümesi** – bu bir g/ç yoğunluklu iş olduğundan, size çekirdek sayısı 4 her Yürütücü ayarlayabilirsiniz.  Yürütücü başına çekirdek sayısı 4 çöp toplama sorunlara neden olabilir, daha büyük için ayarlanıyor.  

    executor-cores = 4
**4. adım: Kümenin YARN bellek miktarını belirlemek** – biz her D4v2 25 GB YARN bellek olduğunu öğrenmek için ambarı'na gidin.  YARN bellek, 8 düğüm olduğundan, 8 ile çarpılır.

    Total YARN memory = nodes * YARN memory* per node
    Total YARN memory = 8 nodes * 25GB = 200GB
**5. adım: Yürütücü sayısı hesaplamak** – Yürütücü sayısı parametresi en düşük bellek kısıtlamanın alarak belirlenir ve CPU sınırlama bölünmüş tarafından Spark üzerinde çalıştırılan uygulamalar için sayısı.    

**Bellek kısıtlama hesaplamak** – bellek kısıtlama bellek Yürütücü başına bölü toplam YARN bellek olarak hesaplanır.

    Memory constraint = (total YARN memory / executor memory) / # of apps   
    Memory constraint = (200GB / 6GB) / 2   
    Memory constraint = 16 (rounded)
**CPU kısıtlaması hesaplamak** -CPU sınırlama bölü Yürütücü başına çekirdek sayısı toplam yarn çekirdek olarak hesaplanır.
    
    YARN cores = nodes in cluster * # of cores per node * 2   
    YARN cores = 8 nodes * 8 cores per D14 * 2 = 128
    CPU constraint = (total YARN cores / # of cores per executor) / # of apps
    CPU constraint = (128 / 4) / 2
    CPU constraint = 16
**Kümesi Yürütücü sayısı**

    num-executors = Min (memory constraint, CPU constraint)
    num-executors = Min (16, 16)
    num-executors = 16    

