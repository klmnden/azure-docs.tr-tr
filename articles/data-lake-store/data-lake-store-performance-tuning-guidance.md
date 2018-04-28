---
title: Azure Data Lake Store performans ayarlama yönergeleri | Microsoft Docs
description: Azure Data Lake Store performans ayarlama yönergeleri
services: data-lake-store
documentationcenter: ''
author: stewu
manager: amitkul
editor: cgronlun
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/30/2017
ms.author: stewu
ms.openlocfilehash: aa803e823eb3096ea785f1f912293cae82c24b8d
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="tuning-azure-data-lake-store-for-performance"></a>Azure Data Lake Store için performans ayarlama

Data Lake Store, g/ç yoğun analizi ve veri taşıma için yüksek verimlilik destekler.  Azure Data Lake Store'da tüm kullanılabilir verimlilik – okunan veya saniye başına yazılan veri miktarı – kullanarak en iyi performansı elde etmek önemlidir.  Bu sayıda okuma gerçekleştirerek elde edilir ve mümkün olduğunca paralel yazar.

![Data Lake Store performans](./media/data-lake-store-performance-tuning-guidance/throughput.png)

Azure Data Lake Store tüm analytics senaryo için gerekli verimlilik sağlamak için ölçeklendirebilirsiniz. Varsayılan olarak, bir Azure Data Lake Store hesabı geniş bir kullanıcı kategorisi kullanım durumlarının ihtiyaçlarını karşılamak üzere yeterli işleme otomatik olarak sağlar. Müşteriler varsayılan sınır çalıştırdığı durumlarda, ADLS hesabına Microsoft Destek ile iletişim kurarak daha fazla verimlilik sağlamak üzere yapılandırılabilir.

## <a name="data-ingestion"></a>Veri alımı

ADLS için bir kaynak sistemden veri alma, kaynak donanım, kaynak ağ donanım ve ağ bağlantısını ADLS performans sorunu olabilir dikkate almak önemlidir.  

![Data Lake Store performans](./media/data-lake-store-performance-tuning-guidance/bottleneck.png)

Veri taşıma faktörlerin tarafından etkilenmemesini sağlamak önemlidir.

### <a name="source-hardware"></a>Kaynak donanım

Şirket içi makineler veya sanal makineleri Azure'da kullanıp kullanmadığınızı uygun donanım dikkatli seçmeniz gerekir. Kaynak Disk donanımı için SSD HDD için tercih ettiğiniz ve daha hızlı dağılımı ile disk donanımı seçin. Kaynak ağ donanımı için en hızlı NIC'ler olası kullanın.  Azure üzerinde uygun şekilde güçlü disk ve ağ donanımı olan Azure D14 VM'ler öneririz.

### <a name="network-connectivity-to-azure-data-lake-store"></a>Azure Data Lake Store ağ bağlantısı

Veri kaynağı ile Azure Data Lake store arasında ağ bağlantısı bazen performans sorunu olabilir. Veri kaynağınızı şirket içi olduğunda, ile ayrılmış bir bağlantı kullanmayı [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) . Veri kaynağınızı Azure içinde ise, performans verileri Data Lake Store ile aynı Azure bölgesinde olduğunda en iyi olacaktır.

### <a name="configure-data-ingestion-tools-for-maximum-parallelization"></a>Veri alımı araçları maksimum paralelleştirme için yapılandırma

Kaynak donanım ele ve ağ bağlantısı sorunlarını yukarıdaki sonra alım araçlarınızı yapılandırmaya hazırsınız demektir. Aşağıdaki tabloda çeşitli popüler alım Araçlar anahtar ayarlarını özetler ve ayrıntılı performans makaleler için ayarlama sağlar.  Senaryonuz için kullanmak üzere hangi aracı hakkında daha fazla bilgi edinmek için bu ziyaret [makale](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-data-scenarios).

| Aracı               | Ayarlar     | Daha fazla ayrıntı                                                                 |
|--------------------|------------------------------------------------------|------------------------------|
| PowerShell       | PerFileThreadCount, ConcurrentFileCount |  [Bağlantı](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-powershell#performance-guidance-while-using-powershell) |
| AdlCopy    | Azure Data Lake Analytics birimleri  |   [Bağlantı](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob#performance-considerations-for-using-adlcopy)         |
| Distcp'yi            | -m (Eşleyici)   | [Bağlantı](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-wasb-distcp#performance-considerations-while-using-distcp)                             |
| Azure Data Factory| parallelCopies    | [Bağlantı](../data-factory/copy-activity-performance.md)                          |
| Sqoop           | FS.Azure.Block.size, -m (Eşleyici)    |   [Bağlantı](https://blogs.msdn.microsoft.com/bigdatasupport/2015/02/17/sqoop-job-performance-tuning-in-hdinsight-hadoop/)        |

## <a name="structure-your-data-set"></a>Veri kümenizi yapısı

Data Lake Store, dosya boyutu, veriler, dosya ve klasör yapısı sayısı bir performansı etkiler.  Aşağıdaki bölümde bu alanlarda en iyi uygulamaları açıklar.  

### <a name="file-size"></a>Dosya boyutu

Genellikle, analiz altyapıları Hdınsight ve Azure Data Lake Analytics gibi dosya başına ek yükleri vardır.  Verilerinizi kadar küçük dosyalarını depoluyorsa, bu performans olumsuz yönde etkileyebilir.  

Genel olarak, verilerinizin daha iyi performans için daha büyük boyutlu dosyalar halinde düzenleyin.  Altın kural, dosyalar 256 MB veya daha büyük veri kümelerinde düzenleyin. Görüntüleri ve ikili veriler gibi bazı durumlarda, paralel olarak işlemek olası değil.  Bu durumda, tek tek dosyaların 2 GB'den tutmanız önerilir.

Bazı durumlarda, veri ardışık çok sayıda küçük dosya olan ham veriler üzerinde denetim sınırlı.  Aşağı akış uygulamaları için kullanmak üzere büyük dosyaları oluşturur "pişirme" bir işlem olması önerilir.  

### <a name="organizing-time-series-data-in-folders"></a>Zaman serisi veri klasörlerde düzenleme

Hive ve ADLA iş yükleri için yalnızca bir veri alt kümesini performansını artıran okuma bazı sorgular zaman serisi veri bölümü ayıklanmasına yardımcı olabilir.    

Zaman serisi veri alma bu ardışık düzen genellikle dosya ve klasörler için çok yapılandırılmış adlandırma ile dosyalarına yerleştirin. Biz tarihe göre yapılandırılmış veriler için bkz: çok yaygın bir örneği aşağıdadır:

    \DataSet\YYYY\MM\DD\datafile_YYYY_MM_DD.tsv

Datetime bilgi klasörleri hem dosya görüntülendiğine dikkat edin.

Tarih ve saat için genel bir desen verilmiştir

    \DataSet\YYYY\MM\DD\HH\mm\datafile_YYYY_MM_DD_HH_mm.tsv

Yeniden klasörüyle yaptığınız seçim ve dosya organizasyonu daha büyük dosya boyutlarına ve her klasördeki dosyaları makul bir dizi iyileştirmeniz gerekir.

## <a name="optimizing-io-intensive-jobs-on-hadoop-and-spark-workloads-on-hdinsight"></a>Hdınsight'ta Hadoop ve Spark iş yükleri üzerindeki g/ç yoğun işleri en iyi duruma getirme

İşlerini aşağıdaki üç kategoriden ayrılır:

* **CPU yoğun.**  Bu işleri en az g/ç sürelerinin uzun hesaplama zamanlarında vardır.  Örnek makine öğrenme ve doğal dil işleri işleme verilebilir.  
* **Yoğun bellek.**  Bu işlemler büyük miktarda bellek kullanır.  Örnek PageRank ve gerçek zamanlı analytics işleri verilebilir.  
* **G/ç yoğun.**  Bu işleri çoğu g/ç yapılması kendi zaman ayırın.  Yalnızca okuma ve yazma işlemlerinin bir kopya iş buna ortak bir örnektir.  Diğer örnekler çok miktarda veri okuma, bazı veri dönüştürme gerçekleştirir ve ardından veri deposuna Yazar veri hazırlık işlerini içerir.  

Aşağıdaki kılavuz yalnızca g/ç yoğun işleri için geçerlidir.

### <a name="general-considerations-for-an-hdinsight-cluster"></a>Hdınsight kümesi için genel konular

* **Hdınsight sürümleri.** En iyi performans için Hdınsight en son sürümünü kullanın.
* **Bölgeleri.** Hdınsight kümesi ile aynı bölgede Data Lake Store yerleştirin.  

Hdınsight kümesi iki baş düğümler ve bazı çalışan düğümleri oluşur. Her bir çalışan düğümünün belirli sayıda çekirdeğe ve VM türüne göre belirlenir bellek sağlar.  Bir iş çalışırken YARN kullanılabilir bellek ve kapsayıcılar oluşturmak için çekirdek ayırır kaynak uzlaştırıcı ' dir.  Her kapsayıcı işi tamamlamak için gereken görevleri çalıştırır.  Kapsayıcıları görevleri hızlı bir şekilde işlemek için paralel olarak çalıştırın. Bu nedenle, performans mümkün olduğu kadar paralel kapsayıcılar çalıştırarak geliştirildi.

Kapsayıcıları sayısını artırmak ve tüm kullanılabilir işleme kullanmak için ayarlanmış bir Hdınsight kümesi içinde üç katmanı vardır.  

* **Fiziksel katman**
* **YARN katmanı**
* **İş yükü katmanı**

### <a name="physical-layer"></a>Fiziksel katman

**Küme, daha fazla düğüm ve/veya daha büyük boyutlu VM'ler ile çalıştırın.**  Daha büyük bir küme, aşağıdaki resimde gösterildiği gibi daha fazla YARN kapsayıcıları çalışmasına olanak sağlar.

![Data Lake Store performans](./media/data-lake-store-performance-tuning-guidance/VM.png)

**VM'ler daha fazla ağ bant genişliği ile kullanın.**  Data Lake Store verimlilik daha az ağ bant genişliği ise ağ bant genişliği miktarını bir performans sorunu olabilir.  Farklı sanal makineleri, ağ bant genişliği boyutları değişen sahip olur.  En büyük olası ağ bant genişliği olan bir VM-türü seçin.

### <a name="yarn-layer"></a>YARN katmanı

**Daha küçük YARN kapsayıcıları kullanın.**  Daha fazla kapsayıcı kaynaklar ile aynı miktarda ile oluşturmak için her YARN kapsayıcı boyutunu azaltın.

![Data Lake Store performans](./media/data-lake-store-performance-tuning-guidance/small-containers.png)

İş yükünüz bağlı olarak, her zaman olacaktır gereken en az bir YARN kapsayıcı boyutu. Çok küçük bir kapsayıcı seçerseniz, işlerinizi bellek yetersiz sorunla çalıştırın. Genellikle YARN kapsayıcıları Hayır 1 GB'den daha küçük olması gerekir. 3 GB YARN kapsayıcıları görmek için yaygın bir durumdur. Bazı iş yükleri için daha büyük YARN kapsayıcıları gerekebilir.  

**YARN kapsayıcı başına çekirdek artırın.**  Her kapsayıcıya, her kapsayıcı içinde çalışan Paralel Görevler sayısını artırmak için ayrılan çekirdek sayısını artırın.  Bu, kapsayıcı başına birden çok görevi çalıştır Spark gibi uygulamalar için çalışır.  Tek bir iş parçacığı her kapsayıcı içinde çalışan uygulamalar için Hive gibi kapsayıcı başına daha fazla çekirdek yerine daha fazla kapsayıcıları iyidir.   

### <a name="workload-layer"></a>İş yükü katmanı

**Tüm kullanılabilir kapsayıcıları kullanın.**  Böylece tüm kaynakları yararlanılmıştır eşit veya kullanılabilir kapsayıcıları sayısından büyük görevler sayısını ayarlayın.

![Data Lake Store performans](./media/data-lake-store-performance-tuning-guidance/use-containers.png)

**Başarısız görevlerin maliyetlidir.** Her görev büyük miktarda veri işlemeye varsa, bir görev hatasını içinde pahalı bir yeniden deneme sonuçlanır.  Bu nedenle, her biri çok küçük miktarda veri işleyen daha fazla görev oluşturmak iyidir.

Yukarıdaki genel yönergeleri yanı sıra, her uygulamanın bu belirli bir uygulama için ayarlamak kullanılabilen farklı parametreleri vardır. Aşağıdaki tablo bazı performans her uygulama için ayarlama ile çalışmaya başlamak için bağlantıları ve parametreleri listeler.

| İş yükü               | Parametre görevleri ayarlamak için                                                         |
|--------------------|-------------------------------------------------------------------------------------|
| [HDInisight Spark](data-lake-store-performance-tuning-spark.md)       | <ul><li>NUM yürütücüler</li><li>Bellek içi Yürütücü</li><li>Yürütücü çekirdek</li></ul> |
| [Hdınsight'ta hive](data-lake-store-performance-tuning-hive.md)    | <ul><li>Hive.tez.Container.size</li></ul>         |
| [Hdınsight üzerinde MapReduce](data-lake-store-performance-tuning-mapreduce.md)            | <ul><li>Mapreduce.Map.Memory</li><li>Mapreduce.job.Maps</li><li>Mapreduce.reduce.Memory</li><li>Mapreduce.job.reduces</li></ul> |
| [Hdınsight üzerinde Storm](data-lake-store-performance-tuning-storm.md)| <ul><li>Çalışan işlem sayısı</li><li>Spout Yürütücü örneği sayısı</li><li>Cıvata Yürütücü örneği sayısı </li><li>Spout görev sayısı</li><li>Cıvata görev sayısı</li></ul>|

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Data Lake Store'a Genel Bakış](data-lake-store-overview.md)
* [Azure Data Lake Analytics ile Çalışmaya Başlama](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
