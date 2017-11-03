---
title: "Azure Data Lake Store Spark performans yönergeleri ayarlama | Microsoft Docs"
description: "Azure Data Lake Store Spark performans kuralları ayarlama"
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
ms.openlocfilehash: 2109744fb7ffdfafb7a86bbea355e119718af099
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="performance-tuning-guidance-for-spark-on-hdinsight-and-azure-data-lake-store"></a>Performans yönergeler Spark Hdınsight ve Azure Data Lake Store için ayarlama

Spark üzerinde performans ayarlama olduğunda, küme üzerinde çalışan uygulama sayısı dikkate almanız gerekir.  Varsayılan olarak 4 çalıştırabilirsiniz, HDI kümesi üzerinde aynı anda uygulamaları (Not: değiştirilebilir varsayılan ayardır).  Varsayılan ayarları geçersiz kılar ve kümenin daha fazla bilgi için bu uygulamaları kullanmak için daha az uygulamaları kullanmaya karar verebilirsiniz.  

## <a name="prerequisites"></a>Ön koşullar

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Azure Data Lake Store hesabı**. Bir oluşturma hakkında yönergeler için bkz: [Azure Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-portal.md)
* **Azure Hdınsight kümesi** bir Data Lake Store hesabına erişim. Bkz: [Data Lake Store ile bir Hdınsight kümesi oluşturmayı](data-lake-store-hdinsight-hadoop-use-portal.md). Küme için Uzak Masaüstü etkinleştirdiğinizden emin olun.
* **Azure Data Lake Store üzerinde Spark kümesinde çalışan**.  Daha fazla bilgi için bkz: [Data Lake Store'da verileri çözümlemek üzere Hdınsight Spark kullanma küme](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-use-with-data-lake-store)
* **Performans ayarlama yönergeleri ADLS**.  Genel performans için bkz [Data Lake deposu performans rehberi ayarlama](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance) 

## <a name="parameters"></a>Parametreler

Spark işlerinin çalıştırırken ADLS performansını artırmak için ayarlanmış en önemli ayarları şunlardır:

* **NUM yürütücüler** -yürütülebilir eşzamanlı görev sayısı.

* **Bellek içi Yürütücü** -her Yürütücü ayrılmış bellek miktarı.

* **Yürütücü çekirdek** -her Yürütücü ayrılan çekirdek sayısı.                     

**NUM yürütücüler** Num yürütücüler paralel olarak çalıştırılabilir görevleri üst sınırını ayarlar.  Paralel olarak çalıştırılabilir görevleri gerçek sayısını bellek ve CPU kaynaklarını kümenizdeki kullanılabilir sınırlıdır.

**Bellek içi Yürütücü** her Yürütücü için ayrılan bellek miktarıdır.  Her Yürütücü için gereken bellek işine bağlıdır.  Karmaşık işlemleri için bellek daha yüksek olması gerekir.  Okuma ve yazma gibi basit işlemleri için bellek gereksinimlerini daha düşük olacaktır.  Her Yürütücü belleğin Ambari içinde görüntülenebilir.  Ambari, Spark için gidin ve yapılandırmalar sekmesini görüntüleyin.  

**Yürütücü çekirdek** bu Yürütücü çalıştırılabilir paralel iş parçacığı sayısını belirler Yürütücü başına kullanılan çekirdek miktarını belirler.  Örneğin, varsa Yürütücü çekirdek = 2, her Yürütücü Yürütücü 2 Paralel Görevler çalıştırabilirsiniz.  Yürütücü çekirdek işinde bağımlı olur.  Daha fazla Paralel Görevler her Yürütücü işleyebilmesi için g/ç yoğun iş büyük miktarda bellek görev başına gerektirmez.

Varsayılan olarak, Hdınsight'ta Spark çalıştırıldığında, her fiziksel çekirdek için iki sanal YARN çekirdek tanımlanır.  Bu bağlam birden çok iş parçacığından değiştirme miktarına ve concurrecy iyi bir denge sağlar.  

## <a name="guidance"></a>Rehber

Spark Data Lake Store'da verilerin çalışmak için analitik iş yükleri çalıştırırken, Data Lake Store ile en iyi performansı elde etmek en son Hdınsight sürüm kullanmanızı öneririz. İşinizi daha fazla g/ç yoğun olduğunda, belirli parametreleri performansını artırmak için yapılandırılabilir.  Azure Data Lake Store yüksek verimlilik işleyebilecek bir yüksek oranda ölçeklenebilir depolama platformudur.  İş, çoğunlukla okuma veya yazma oluşuyorsa, ardından eşzamanlılık g/ç için Azure Data Lake Store gelen ve giden performansı artırma.

G/ç yoğun işleri için eşzamanlılık artırmak için genel birkaç yolu vardır.

**1. adım: kümenizde çalışan kaç uygulamalar belirlemek** – geçerli de dahil olmak üzere kümede çalışan kaç uygulamalar bilmeniz gerekir.  4 vardır ayarı varsayar her Spark varsayılan değerleri aynı anda çalıştırılan uygulamalar.  Bu nedenle, her uygulama için kullanılabilir küme % 25'ini yalnızca sahip olur.  Daha iyi performans almak için yürütücüler sayısını değiştirerek Varsayılanları geçersiz kılabilir.  

**2. adım: Bellek içi Yürütücü ayarlama** – ayarlamak için ilk Yürütücü bellek şeydir.  Bellek çalışmasına olmaya işinde bağımlı olur.  Yürütücü başına daha az bellek ayırarak eşzamanlılık artırabilir.  İşinizi çalıştırdığınızda yetersiz bellek özel durumları görürseniz, bu parametre için değer artırmanız gerekir.  Daha fazla bellek daha yüksek miktarlarda bellek sahip bir küme kullanarak veya kümenizin boyutunu artırmayı almak bir alternatiftir.  Daha fazla bellek, daha fazla eşzamanlılık başka bir deyişle, kullanılacak, daha fazla yürütücüler olanak sağlar.

**3. adım: Ayarlama Yürütücü çekirdek** – karmaşık işlemleri sahip olmaması için g/ç yoğun iş yükleri, bu çok sayıda Paralel Görevler Yürütücü başına sayısını artırmak için Yürütücü-çekirdek ile başlatmak iyi.  Yürütücü çekirdek 4 olarak ayarlamak, iyi bir başlangıç noktasıdır.   

    executor-cores = 4
Yürütücü çekirdek sayısının artırılması farklı Yürütücü çekirdeğiyle deneyebilirsiniz şekilde daha fazla paralellik tanıyacaktır.  Daha karmaşık işlemleri sahip işleri Yürütücü başına çekirdek sayısı azaltmanız gerekir.  Yürütücü çekirdek ise 4'ten yüksek ayarlanmış sonra atık toplama verimsiz hale ve performansı düşebilir.

**Adım 4: küme YARN bellek miktarını belirlemek** – bu bilgiler Ambari içinde kullanılabilir.  YARN için gidin ve yapılandırmalar sekmesini görüntüleyin.  YARN bellek Bu pencerede görüntülenir.  
Not: penceresinde olmakla birlikte, aynı zamanda varsayılan YARN kapsayıcı boyutu görebilirsiniz.  YARN kapsayıcı boyutu Yürütücü parametresi başına bellek ile aynıdır.

    Total YARN memory = nodes * YARN memory per node
**5. adım: num yürütücüler Hesapla**

**Bellek kısıtlama hesaplamak** -num yürütücüler parametre bellek veya CPU tarafından sınırlanır.  Bellek kısıtlama, uygulamanız için kullanılabilir YARN bellek miktarı tarafından belirlenir.  Toplam YARN bellek alın ve yürütücü bellekle bölün.  Kısıtlama biz uygulamaları sayısına göre Böl şekilde uygulama sayısı için XML'deki ölçeklendirilmiş olması gerekir.

    Memory constraint = (total YARN memory / executor memory) / # of apps   
**CPU kısıtlaması hesaplamak** -CPU kısıtlaması bölü Yürütücü başına çekirdek sayısı toplam sanal çekirdek olarak hesaplanır.  Her fiziksel çekirdek için 2 sanal çekirdek vardır.  Bellek kısıtlama benzer, bölme uygulamaları sayısına göre sunuyoruz.

    virtual cores = (nodes in cluster * # of physical cores in node * 2)
    CPU constraint = (total virtual cores / # of cores per executor) / # of apps
**Ayarlama num yürütücüler** – num yürütücüler parametresi en az bellek kısıtlama ve CPU kısıtlaması gerçekleştirerek belirlenir. 

    num-executors = Min (total virtual Cores / # of cores per executor, available YARN memory / executor-memory)   
Daha yüksek bir sayı yürütücüler sayısı ayarlanması, performans mutlaka artırmaz.  Daha fazla yürütücüler ekleme ekleyeceksiniz dikkate almanız gereken potansiyel olarak performansı düşürebilir her ek Yürütücü için fazladan genel gider.  NUM yürütücüler tarafından küme kaynağı ilişkisindeki.    

## <a name="example-calculation"></a>Örnek hesaplama

2 çalışan 8 D4v2 düğümden oluşan bir küme şu anda sahip düşünelim bulacağınızı çalıştırmak için de dahil olmak üzere uygulamalar.  

**1. adım: kümenizde çalışan kaç uygulamalar belirlemek** – 2 olduğunu bildiğiniz bulacağınızı çalıştırmak için de dahil olmak üzere kümenizde uygulamaları.  

**2. adım: Bellek içi Yürütücü ayarlama** – Bu örnekte, 6 GB bellek Yürütücü g/ç yoğun iş için yeterli olacaktır olan belirleriz.  

    executor-memory = 6GB
**3. adım: Ayarlama Yürütücü çekirdek** – bir g/ç yoğun iş olacağından, size çekirdek sayısı her Yürütücü 4 ayarlayabilirsiniz.  Yürütücü başına çekirdek 4 atık toplama sorunlara neden olabilirsiniz daha büyük için ayarlama.  

    executor-cores = 4
**Adım 4: küme YARN bellek miktarını belirlemek** – biz her D4v2 25 GB YARN bellek olduğunu öğrenmek için ambarı'na gidin.  8 düğüm olduğundan, kullanılabilir YARN bellek 8 ile çarpılır.

    Total YARN memory = nodes * YARN memory* per node
    Total YARN memory = 8 nodes * 25GB = 200GB
**5. adım: num yürütücüler hesaplamak** – num yürütücüler parametre en düşük bellek kısıtlamanın gerçekleştirerek belirlenir ve CPU kısıtlaması bölünmüş Spark üzerinde çalışan uygulamalar # tarafından.    

**Bellek kısıtlama hesaplamak** – bellek kısıtlama toplam YARN bellek Yürütücü başına bellek bölünmesiyle hesaplanır.

    Memory constraint = (total YARN memory / executor memory) / # of apps   
    Memory constraint = (200GB / 6GB) / 2   
    Memory constraint = 16 (rounded)
**CPU kısıtlaması hesaplamak** -CPU kısıtlaması bölü Yürütücü başına çekirdek sayısı toplam yarn çekirdek olarak hesaplanır.
    
    YARN cores = nodes in cluster * # of cores per node * 2   
    YARN cores = 8 nodes * 8 cores per D14 * 2 = 128
    CPU constraint = (total YARN cores / # of cores per executor) / # of apps
    CPU constraint = (128 / 4) / 2
    CPU constraint = 16
**Set num-yürütücüler**

    num-executors = Min (memory constraint, CPU constraint)
    num-executors = Min (16, 16)
    num-executors = 16    

