---
title: Azure Data Lake depolama Gen1 performansı ayarlama yönergeleri | Microsoft Docs
description: Azure Data Lake depolama Gen1 performansı ayarlama yönergeleri
services: data-lake-store
documentationcenter: ''
author: stewu
manager: amitkul
editor: cgronlun
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: stewu
ms.openlocfilehash: a8a50db5ece242bc00a28e66e21c863388950d6f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61437636"
---
# <a name="tuning-azure-data-lake-storage-gen1-for-performance"></a>Azure Data Lake depolama Gen1 için performans ayarlama

Azure Data Lake depolama Gen1 g/ç kullanımı yoğun analiz ve veri taşıma için yüksek performanslı destekler.  Data Lake depolama Gen1 tüm kullanılabilir aktarım hızı – okuma veya saniye başına yazılan veri miktarı – kullanarak en iyi performansı elde etmek önemlidir.  Bu kadar çok okuma gerçekleştirerek elde edilir ve mümkün olduğunca paralel yazar.

![Data Lake depolama Gen1 performans](./media/data-lake-store-performance-tuning-guidance/throughput.png)

Data Lake depolama Gen1, tüm analytics senaryosu için gerekli aktarım hızını sağlamak üzere ölçeklendirebilirsiniz. Varsayılan olarak, bir Data Lake depolama Gen1 hesabı kullanım geniş bir kategori ihtiyaçlarını karşılamak üzere otomatik olarak yeterli performans sağlar. Müşteriler varsayılan sınırı içinde çalıştırdığı durumlar için Data Lake depolama Gen1 hesabı, Microsoft Destek'e başvurarak daha fazla aktarım hızını sağlamak üzere yapılandırılabilir.

## <a name="data-ingestion"></a>Veri alımı

Data Lake depolama Gen1 için bir kaynak sistemden veri alma, kaynak donanım, kaynak ağ donanımına ve Data Lake depolama Gen1 ağ bağlantısı sorunu olabileceğini göz önünde bulundurmanız önemlidir.  

![Data Lake depolama Gen1 performans](./media/data-lake-store-performance-tuning-guidance/bottleneck.png)

Veri taşıma bu faktörler tarafından etkilenmemesini sağlamak önemlidir.

### <a name="source-hardware"></a>Kaynak donanım

Şirket içi makineler veya sanal makineleri Azure'da kullanıp kullanmadığınızı uygun donanım dikkatli seçmeniz gerekir. Kaynak Disk donanım için SSD Hdd'lere tercih ve disk donanım ile daha hızlı iğne seçin. Kaynak ağ donanımı için mümkün olan en hızlı NIC kullanın.  Azure üzerinde uygun bir şekilde güçlü disk ve ağ donanımı olan Azure D14 Vm'leri öneririz.

### <a name="network-connectivity-to-data-lake-storage-gen1"></a>Data Lake depolama Gen1 ağ bağlantısı

Veri kaynağı ile Data Lake depolama Gen1 arasında ağ bağlantısı sorunu bazen olabilir. Şirket içi veri kaynağınızı olduğunda, ayrılmış bir bağlantı ile kullanmayı göz önünde bulundurun [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) . Kaynak verilerinizi Azure'da ise, performans verileri Data Lake depolama Gen1 hesabıyla aynı Azure bölgesinde olduğunda en iyi olacaktır.

### <a name="configure-data-ingestion-tools-for-maximum-parallelization"></a>En fazla paralelleştirme için veri alma araçları yapılandırma

Kaynak donanım giderdik ve ağ bağlantısı sorunları yukarıdaki sonra alımı araçlarınızı yapılandırmaya hazır olursunuz. Aşağıdaki tabloda, çeşitli popüler alma araçları için anahtar ayarları özetlenmekte ve ayrıntılı performans makaleler için bunları ayarlama sağlar.  Senaryonuz için kullanmak için hangi aracı hakkında daha fazla bilgi edinmek için bu ziyaret [makale](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-data-scenarios).

| Tool               | Ayarlar     | Diğer Ayrıntılar                                                                 |
|--------------------|------------------------------------------------------|------------------------------|
| PowerShell       | PerFileThreadCount, ConcurrentFileCount |  [Bağlantı](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-powershell) |
| AdlCopy    | Azure Data Lake Analytics birimi  |   [Bağlantı](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob#performance-considerations-for-using-adlcopy)         |
| DistCp            | -m (Eşleyici)   | [Bağlantı](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-wasb-distcp#performance-considerations-while-using-distcp)                             |
| Azure Data Factory| parallelCopies    | [Bağlantı](../data-factory/copy-activity-performance.md)                          |
| Sqoop           | fs.azure.block.size, -m (mapper)    |   [Bağlantı](https://blogs.msdn.microsoft.com/bigdatasupport/2015/02/17/sqoop-job-performance-tuning-in-hdinsight-hadoop/)        |

## <a name="structure-your-data-set"></a>Veri kümeniz yapısı

Dosya boyutu, Data Lake depolama Gen1 veriler, dosya ve klasör yapısını bir performansı etkiler.  Aşağıdaki bölümde, bu alanlarda en iyi uygulamalar açıklanmaktadır.  

### <a name="file-size"></a>Dosya boyutu

Genellikle, HDInsight ve Azure Data Lake Analytics gibi analiz altyapıları bir dosya başına ek yükleri vardır.  Çok küçük dosya verilerinizi depolayın, bu performansı olumsuz yönde etkileyebilir.  

Genel olarak, verilerinizi daha iyi performans için daha büyük boyutlu dosyalar halinde düzenleyin.  Bir kural karşısında, dosyalar 256 MB veya daha büyük veri kümelerinde düzenleyin. Görüntüler ve ikili veri gibi bazı durumlarda, paralel olarak işlemek mümkün değildir.  Bu gibi durumlarda, tek tek dosyaları 2 GB'tan tutmak için önerilir.

Bazı durumlarda, veri işlem hatlarını çok sayıda küçük dosya olan ham veriler üzerinde denetim sınırlıdır.  Aşağı akış uygulamaları için daha büyük dosyaları oluşturur "bir" bir süreç kullanmanız önerilir.

### <a name="organizing-time-series-data-in-folders"></a>Zaman serisi verileri klasörlerindeki düzenleme

Hive ve ADLA iş yükleri için yalnızca bir veri alt kümesini performansı artıran okuma bazı sorgular zaman serisi verilerini bölüm ayıklanmasına yardımcı olabilir.    

Zaman serisi verilerini alma Bu işlem hatları, dosyalar ve klasörler için çok yapılandırılmış adlandırma ile kullanıcıların dosyaları genellikle yerleştirin. Tarihe göre yapılandırılmış veriler için görüyoruz çok yaygın bir örnek aşağıda verilmiştir:

    \DataSet\YYYY\MM\DD\datafile_YYYY_MM_DD.tsv

Klasör ve dosya adında tarih/saat bilgileri göründüğüne dikkat edin.

Tarih ve saat için yaygın bir düzen aşağıda verilmiştir

    \DataSet\YYYY\MM\DD\HH\mm\datafile_YYYY_MM_DD_HH_mm.tsv

Yeniden klasörü yaptığınız seçimi ve büyük dosya boyutu ve her klasördeki dosyaları makul sayıda dosya organizasyonu iyileştirmeniz gerekir.

## <a name="optimizing-io-intensive-jobs-on-hadoop-and-spark-workloads-on-hdinsight"></a>HDInsight üzerinde Hadoop ve Spark iş yüklerinde en iyi duruma getirme g/ç yoğunluklu işler

İşleri şu üç kategoriden birine girer:

* **CPU yoğun.**  Bu işlemler uzun bir hesaplama süreleri ile en az g/ç sürelerinin vardır.  Makine öğrenme ve doğal dil işleme işleri verilebilir.  
* **Yoğun bellek.**  Bu işlemler büyük miktarda bellek kullanır.  PageRank ve gerçek zamanlı analiz işleri verilebilir.  
* **Yoğun g/ç.**  Bu işlerin çoğu g/ç bulunurken kendi zaman ayırın.  Yaygın olarak karşılaşılan örneklerden yalnızca okuma ve yazma işlemlerini bir kopyalama işi var.  Diğer örnekler veri hazırlama işlerinin, çok miktarda veri okuma, bazı veri dönüştürme gerçekleştirir ve ardından veri deposuna yazar.  

Aşağıdaki yönergeler, yalnızca g/ç yoğunluklu işler için geçerlidir.

### <a name="general-considerations-for-an-hdinsight-cluster"></a>Bir HDInsight kümesi için gereken temel noktalar

* **HDInsight sürümleri.** En iyi performans için HDInsight'ın en son sürümü kullanın.
* **Bölgeleri.** HDInsight kümesi ile aynı bölgede Data Lake depolama Gen1 hesabı yerleştirin.  

Bir HDInsight kümesi, iki baş düğüm ve bazı çalışan düğümleri oluşur. Her çalışan düğümüne belirli sayıda çekirdek ve bellek, sanal makine türüne göre belirlenir sağlar.  Bir iş çalışırken, YARN kapsayıcıları oluşturmak için çekirdek ve bellek ayırır resource negotiator ' dir.  Her kapsayıcı, işin tamamlanması gereken görevleri çalıştırır.  Kapsayıcıları görevlerini hızlı bir şekilde işlemek için paralel olarak çalıştırın. Bu nedenle, mümkün olduğunca çok paralel kapsayıcılar çalıştırarak performansı geliştirildi.

Kapsayıcıları sayısını artırmak ve tüm kullanılabilir işleme kullanmak için ayarlanabilecek bir HDInsight kümesi içinde üç katmanı vardır.  

* **Fiziksel katman**
* **YARN katmanı**
* **İş yükü katmanı**

### <a name="physical-layer"></a>Fiziksel katman

**Küme, daha fazla düğüm ve/veya daha büyük boyutlu sanal makineleri ile çalıştırın.**  Daha büyük bir küme, aşağıdaki resimde gösterildiği gibi daha fazla YARN kapsayıcıları çalıştırmaya olanak sağlar.

![Data Lake depolama Gen1 performans](./media/data-lake-store-performance-tuning-guidance/VM.png)

**Daha fazla ağ bant genişliği ile Vm'leri kullanır.**  Data Lake depolama Gen1 aktarım hızı değerinden daha az ağ bant genişliği olduğunda ağ bant genişliği miktarını bir performans sorunu olabilir.  Farklı Vm'leri, ağ bant genişliği boyutları farklı olacaktır.  En büyük olası ağ bant genişliğine sahip bir VM-türü seçin.

### <a name="yarn-layer"></a>YARN katmanı

**Daha küçük YARN kapsayıcıları kullanın.**  Daha fazla kapsayıcı ile aynı kaynak miktarını oluşturmak için her YARN kapsayıcı boyutunu küçültün.

![Data Lake depolama Gen1 performans](./media/data-lake-store-performance-tuning-guidance/small-containers.png)

İş yükünüze bağlı olarak, her zaman olmayacaktır gereken en az bir YARN kapsayıcı boyutu. Çok küçük bir kapsayıcı seçerseniz, işlerinizi bellek yetersiz bir sorunla karşılaşırsanız çalıştırılır. YARN kapsayıcıları genellikle 1 GB'tan küçük olmalıdır. 3 GB YARN kapsayıcıları görmek için yaygındır. Bazı iş yükleri için daha büyük YARN kapsayıcıları gerekebilir.  

**YARN kapsayıcı başına çekirdek artırın.**  Her bir kapsayıcıda çalıştırılan Paralel Görevler sayısını artırmak için her bir kapsayıcıya ayrılan çekirdek sayısını artırın.  Bu, Spark gibi kapsayıcı başına birden çok görev çalışan uygulamalar için çalışır.  Tek bir iş parçacığı her kapsayıcıda çalışan uygulamalar için Hive gibi kapsayıcı başına daha fazla çekirdek yerine daha fazla kapsayıcı iyidir.

### <a name="workload-layer"></a>İş yükü katmanı

**Tüm kullanılabilir kapsayıcıları kullanın.**  Böylece tüm kaynaklar kullanılan kullanılabilir kapsayıcıları sayısından daha büyük veya eşit olmasını görevlerin sayısını ayarlayın.

![Data Lake depolama Gen1 performans](./media/data-lake-store-performance-tuning-guidance/use-containers.png)

**Başarısız olan görevleri maliyetlidir.** Her görev, çok miktarda veri işlemek için varsa, bir görev hatasını pahalı bir yeniden deneme sonuçlanır.  Bu nedenle, daha fazla görevi, her biri bir küçük miktarda veri işler oluşturmak iyidir.

Yukarıdaki genel yönergeleri ek olarak, her uygulama, belirli bir uygulama için ayarlanacak farklı parametre sunar. Aşağıdaki tabloda bazı performans için her bir uygulama ayarı kullanmaya başlamak için bağlantıları ve parametreleri listeler.

| İş yükü               | Parametre görevleri ayarlamak için                                                         |
|--------------------|-------------------------------------------------------------------------------------|
| [HDInsight üzerinde Spark](data-lake-store-performance-tuning-spark.md)       | <ul><li>Yürütücü sayısı</li><li>Bellek içi Yürütücü</li><li>Yürütücü çekirdek sayısı</li></ul> |
| [HDInsight üzerinde hive](data-lake-store-performance-tuning-hive.md)    | <ul><li>Hive.tez.Container.size</li></ul>         |
| [HDInsight MapReduce](data-lake-store-performance-tuning-mapreduce.md)            | <ul><li>Mapreduce.Map.Memory</li><li>Mapreduce.job.Maps</li><li>Mapreduce.reduce.Memory</li><li>Mapreduce.job.reduces</li></ul> |
| [HDInsight üzerinde Storm](data-lake-store-performance-tuning-storm.md)| <ul><li>Çalışan işlemi sayısı</li><li>Spout Yürütücü örneği sayısı</li><li>Bolt Yürütücü örneği sayısı </li><li>Spout görev sayısı</li><li>Bolt görev sayısı</li></ul>|

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Data Lake depolama Gen1 genel bakış](data-lake-store-overview.md)
* [Azure Data Lake Analytics ile Çalışmaya Başlama](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
