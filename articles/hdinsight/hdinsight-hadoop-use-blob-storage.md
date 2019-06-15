---
title: HDFS uyumlu Azure Depolama'da veri sorgulama - Azure HDInsight
description: Azure depolama ve analiz sonuçlarınızı depolamak için Azure Data Lake Storage verilerini sorgulamayı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/23/2019
ms.openlocfilehash: 6e0192029decef95dcaecc0c60dce5fd5b6f99ff
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66479912"
---
# <a name="use-azure-storage-with-azure-hdinsight-clusters"></a>Azure HDInsight kümeleri ile Azure Depolama'yı kullanma

HDInsight kümesindeki verileri çözümlemek için veri ya da depolayabilirsiniz içinde [Azure depolama](../storage/common/storage-introduction.md), [Azure Data Lake depolama Gen 1](../data-lake-store/data-lake-store-overview.md)/[Azure Data Lake depolama Gen 2](../storage/blobs/data-lake-storage-introduction.md), veya bir birleşimi. Bu depolama seçeneklerinin, kullanıcı verilerini kaybetmeden hesaplama için kullanılan HDInsight kümelerini güvenle silmenizi sağlar.

Apache Hadoop varsayılan dosya sistemi kavramını destekler. Varsayılan dosya sistemi varsayılan şema ve yetkilisi anlamına gelir. Bu göreceli yolları çözümlemek için de kullanılabilir. HDInsight kümesi oluşturma işlemi sırasında varsayılan dosya sistemi olarak Azure storage'da bir blob kapsayıcısı belirtebilirsiniz veya HDInsight 3.6 ile Azure depolama veya Azure Data Lake depolama Gen 1 / Azure Data Lake seçebilirsiniz depolama Gen 2 varsayılan dosyaları Sistem birkaç özel durum. Hem varsayılan hem de bağlı depolama olarak Data Lake depolama Gen 1 kullanmanın desteklenebilirliği için bkz: [HDInsight kümesi için kullanılabilirlik](./hdinsight-hadoop-use-data-lake-store.md#availability-for-hdinsight-clusters).

Bu makalede Azure Depolama'nın HDInsight kümeleri ile nasıl çalıştığı hakkında bilgi edinebilirsiniz. Data Lake depolama Gen 1 HDInsight kümeleriyle nasıl çalıştığı hakkında bilgi için bkz: [kullanımı Azure Data Lake Storage, Azure HDInsight kümeleri](hdinsight-hadoop-use-data-lake-store.md). HDInsight kümesi oluşturma hakkında daha fazla bilgi için bkz. [Apache Hadoop kümeleri oluşturma HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

Azure depolama, HDInsight ile sorunsuz bir şekilde tümleşen, sağlam ve genel amaçlı bir depolama çözümüdür. HDInsight, Azure Depolama’daki bir blob kapsayıcıyı kümenin varsayılan dosya sistemi olarak kullanabilir. Hadoop dağıtılmış dosya sistemi (HDFS) arabirimi aracılığıyla, HDInsight’taki bileşenler kümesinin tümü blob olarak depolanan yapılandırılmış veya yapılandırılmamış veriler üzerinde doğrudan çalışabilir.

> [!WARNING]  
> Depolama hesabı türü **BlobStorage** yalnızca HDInsight kümeleri için ikincil depolama alanı olarak kullanılabilir.

| Depolama hesabı türü | Desteklenen hizmetler | Desteklenen performans katmanları | Desteklenen erişim katmanları |
|----------------------|--------------------|-----------------------------|------------------------|
| StorageV2 (genel amaçlı v2)  | Blob     | Standart                    | Sık erişimli, seyrek erişimli ve Arşiv\*   |
| Depolama (genel amaçlı v1)   | Blob     | Standart                    | Yok                    |
| BlobStorage                    | Blob     | Standart                    | Sık erişimli, seyrek erişimli ve Arşiv\*   |

İş verilerini depolamak için, varsayılan blob kapsayıcısını kullanmanızı önermiyoruz. Depolama maliyetini azaltmak için blob kapsayıcısının her kullanımdan sonra silinmesi iyi bir uygulamadır. Uygulama ve sistem varsayılan kapsayıcı içeren günlükleri. Kapsayıcıyı silmeden önce günlükleri aldığınızdan emin olun.

Bir blob kapsayıcısının birden fazla küme için varsayılan dosya sistemi olarak paylaşılması desteklenmez.

> [!NOTE]  
> Arşiv erişim katmanı bir birkaç saatlik alma gecikmesinin ve HDInsight ile kullanım için önerilmez çevrimdışı bir katmandır. Daha fazla bilgi için [arşiv erişim katmanı](../storage/blobs/storage-blob-storage-tiers.md#archive-access-tier).

Depolama hesabınızın güvenliğini sağlamak tercih ederseniz **güvenlik duvarları ve sanal ağlar** kısıtlamalar **seçili ağlar**, özel durum etkinleştirdiğinizden emin olun **güvenilen Microsoft izin ver Hizmetleri...**  HDInsight depolama hesabınıza erişebilmesi için.

## <a name="hdinsight-storage-architecture"></a>HDInsight depolama mimarisi
Aşağıdaki diyagram, Azure Depolama ile kullanılan HDInsight depolama mimarisine ilişkin bir özet görünüm sağlar:

![Hadoop kümeleri, Blob Depolamada yapılandırılmış ve yapılandırılmamış verilere erişmek ve depolamak için HDFS API’sini kullanır.](./media/hdinsight-hadoop-use-blob-storage/HDI.WASB.Arch.png "HDInsight Depolama Mimarisi")

HDInsight, işlem düğümlerine yerel olarak bağlı olan dağıtılmış dosya sistemine erişim imkanı sağlar. Bu dosya sistemine tam uygun URI kullanılarak erişilebilir, örneğin:

    hdfs://<namenodehost>/<path>

Ayrıca HDInsight, Azure Depolama'da depolanan verilere erişebilmenizi de sağlar. Sözdizimi şöyledir:

    wasb://<containername>@<accountname>.blob.core.windows.net/<path>

HDInsight kümeleriyle Azure Depolama hesabını kullanırken dikkat etmeniz gereken bazı noktalar mevcuttur.

* **Bir kümeye bağlı depolama hesaplarındaki kapsayıcılar:** Oluşturma sırasında hesap adı ve anahtar kümeyle ilişkili olduğundan bu kapsayıcılardaki blob'lara tam erişiminiz vardır.

* **Genel kapsayıcılar veya bir kümeye bağlı olmayan depolama hesaplarındaki genel BLOB'lar:** Kapsayıcılardaki blob'lara salt okunur iznine sahip.
  
> [!NOTE]  
> Genel kapsayıcılar, bu kapsayıcıda bulunan tüm blob’ların bir listesini ve kapsayıcı meta verilerini almanıza olanak tanır. Genel blob'lar, yalnızca tam URL'yi biliyorsanız blob erişiminize izin verir. Daha fazla bilgi için [kapsayıcılara ve blob'lara erişimi yönetme](../storage/blobs/storage-manage-access-to-resources.md).

* **Bir kümeye bağlı olmayan depolama hesaplarındaki özel kapsayıcılar:** WebHCat işleri gönderdiğinizde depolama hesabını tanımlamadığınız sürece kapsayıcılardaki blob'lara erişemezsiniz. Bu, bu makalenin sonraki bölümlerinde açıklanmıştır.

Oluşturma işlemi ve bunların anahtarların tanımlanan depolama hesaplarını depolanan `%HADOOP_HOME%/conf/core-site.xml` küme düğümlerinde. HDInsight’ın varsayılan davranışı core-site.xml dosyasında tanımlanan depolama hesaplarını kullanmaktır. Bu ayarı kullanarak değiştirebilirsiniz [Apache Ambari](./hdinsight-hadoop-manage-ambari.md).

Depolama hesapları ve bunların meta veri açıklamasını Apache Hive, MapReduce, akış Apache Hadoop ve Apache Pig dahil olmak üzere birden çok WebHCat işleri gerçekleştirebilirsiniz. (Bu şu anda yalnızca Pig depolama hesapları ile çalışmakta, ancak meta verilerle çalışmamaktadır.) Daha fazla bilgi için bkz. [Alternatif Depolama Hesapları ve Meta Depolarla HDInsight Kümesi kullanma](https://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).

Bloblar yapılandırılmış ve yapılandırılmamış veriler için kullanılabilir. Blob kapsayıcıları, verileri anahtar/değer çiftleri olarak depolar ve dizin hiyerarşisi bulunmaz. Ancak, bir dosyayı dizin yapısında depolanmış gibi göstermek için anahtar adında eğik çizgi karakteri (/) kullanılabilir. Örneğin, bir blob'un anahtarı *input/log1.txt* şeklinde olabilir. Gerçek *giriş* dizini yoktur, ancak anahtar adında eğik çizgi karakteri bulunması nedeniyle, bir dosya yolu görünümüne sahiptir.

## <a id="benefits"></a>Azure Depolamanın yararları
İşlem kümelerini ve depolama kaynaklarını birlikte bulundurma değil örtülü performans maliyeti işlem kümeleri oluşturulur nerede yüksek hızlı ağ verimli hale Azure bölgesindeki depolama hesabı kaynaklarına yakın şekilde tarafından azalır işlem düğümleri, Azure depolama içinde verilere erişmek için.

Verileri HDFS yerine Azure Depolama’da depolamanın çeşitli avantajları vardır:

* **Verileri yeniden kullanma ve paylaşma:** HDFS'deki veriler işlem kümesi içinde bulunur. Yalnızca işlem kümesi erişimi olan uygulamalar HDFS API'lerini kullanarak verileri kullanabilir. Azure Depolama'daki verilere HDFS API'si aracılığıyla veya aracılığıyla erişilebilen [Blob Depolama REST API'leri](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API). Bu nedenle, daha büyük uygulama kümeleri (diğer HDInsight kümeleri dahil olmak üzere) ve araçları veri üretmek ve kullanmak için kullanılabilir.

* **Veri arşivleme:** Verileri Azure depolamada depolamak, hesaplama için kullanılan kullanıcı verilerini kaybetmeden güvenle silinmesini HDInsight kümeleri sağlar.

* **Veri depolama maliyeti:** Verileri DFS uzun süreli depolamak, işlem kümesinin maliyeti Azure depolama maliyeti yüksek olduğu için verileri Azure depolamada depolamaktan daha maliyetlidir. Ayrıca, verilerin her işlem kümesi oluşturmada yeniden yüklenmesi gerekmediğinden, veri yükleme maliyetlerinden de tasarruf edersiniz.

* **Esnek ölçeklendirme:** HDFS size ölçeklendirilmiş dosya sistemi ile sağlar ancak ölçek kümeniz için oluşturduğunuz düğüm sayısı tarafından belirlenir. Ölçeği değiştirmek, Azure depolamada otomatik olarak aldığınız esnek ölçeklendirme özelliklere bağlı kalmaktan daha karmaşık bir işlem haline gelebilir.

* **Coğrafi çoğaltma:** Azure depolama, coğrafi olarak çoğaltılmış olabilir. Bu, size coğrafi kurtarma ve veri yedekliği sağlamakla birlikte, coğrafi olarak çoğaltılan bir konuma yük devretmek performansınızı ciddi bir şekilde etkiler ve ek ücrete neden olabilir. Bu nedenle, coğrafi çoğaltmayı akıllıca ve yalnızca verilerin değeri ek maliyetlere değer durumdaysa tercih etmenizi öneririz.

Bazı MapReduce işleri ve paketleri gerçekte Azure depolamada depolamak istemediğiniz ara sonuçlar oluşturabilir. Bu durumda, verileri yerel HDFS’de depolamak üzere seçebilirsiniz. Aslında, HDInsight Hive işleri ve diğer işlemlerdeki bu ara sonuçların bazıları için DFS kullanır.

> [!NOTE]  
> Çoğu HDFS komutu (örneğin, `ls`, `copyFromLocal` ve `mkdir`) hala beklendiği gibi çalışmayabilir. Gibi (DFS adlandırılır) yerel HDFS uygulamasına özgü komutlar `fschk` ve `dfsadmin`, Azure depolamada farklı bir davranış gösterir.

## <a name="address-files-in-azure-storage"></a>Azure depolamada dosyaları adresleme
HDInsight’ta Azure depolamadaki dosyalara erişmek için URI şeması aşağıdaki gibidir:

```config
wasb://<BlobStorageContainerName>@<StorageAccountName>.blob.core.windows.net/<path>
```

URI şeması şifrelenmemiş erişim (ile *wasb:* öneki ile) ve SSL şifreli erişim (*wasbs* ile) şifrelenmemiş erişim sağlar. Azure’da aynı bölgede bulunan verilere erişirken dahi mümkün olduğunda *wasbs* kullanmanızı öneririz.

`<BlobStorageContainerName>` Azure depolamada blob kapsayıcının adını tanımlar.
`<StorageAccountName>` Azure depolama hesabı adını tanımlar. Tam uygun etki alanı adı (FQDN) gereklidir.

Kullanılmazsa `<BlobStorageContainerName>` ya da `<StorageAccountName>` belirtilmediyse varsayılan dosya sistemi kullanılır. Varsayılan dosya sistemindeki dosyalar için göreli bir yol veya mutlak bir yol kullanabilirsiniz. Örneğin, HDInsight kümeleriyle gelen *hadoop mapreduce examples.jar* dosyasına aşağıdakilerden birini kullanarak başvurulabilir:

```config
wasb://mycontainer@myaccount.blob.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
wasb:///example/jars/hadoop-mapreduce-examples.jar
/example/jars/hadoop-mapreduce-examples.jar
```

> [!NOTE]  
> Dosya adı `hadoop-examples.jar` HDInsight sürüm 2.1 ve 1.6 kümelerinde.

Dosya veya dizin HDFS yol adı yoludur. Azure depolamadaki kapsayıcılar anahtar-değer depoları olduğundan, doğru hiyerarşik dosya sistemi yoktur. Bir blob anahtarındaki bir eğik çizgi karakteri (/) dizin ayırıcı olarak yorumlanır. Örneğin, *hadoop mapreduce examples.jar* için blob adı aşağıdaki gibidir:

```bash
example/jars/hadoop-mapreduce-examples.jar
```

> [!NOTE]  
> HDInsight dışındaki blob'larla çalışırken, yardımcı programların çoğu WASB biçimini tanımaz ve bunun yerine, `example/jars/hadoop-mapreduce-examples.jar` gibi temel yol biçimi gibi bekler.

##  <a name="blob-containers"></a>BLOB kapsayıcıları
BLOB'ları kullanmak için önce oluşturmanız bir [Azure depolama hesabı](../storage/common/storage-create-storage-account.md). Bunun bir parçası olarak depolama hesabının oluşturulduğu Azure bölgesini belirtirsiniz. Küme ve depolama hesabının aynı bölgede barındırılması gerekir. Hive meta depo SQL Server veritabanı ve Apache Oozie meta depo SQL Server veritabanı da aynı bölgede bulunmalıdır.

Nerede olursa olsun, oluşturduğunuz her blob Azure Storage hesabınızdaki bir kapsayıcıya aittir. Bu kapsayıcı HDInsight dışında oluşturulmuş bir blob veya bir HDInsight kümesi için oluşturulan bir kapsayıcı olabilir.

Varsayılan Blob kapsayıcısı iş geçmişi ve iş günlükleri gibi kümeye özel bilgileri depolar. Varsayılan Blob kapsayıcısını birden çok HDInsight kümesiyle paylaşmayın. Bu durum iş geçmişinin bozulmasına neden olabilir. Her küme için farklı bir kapsayıcı kullanmanız ve paylaşılan verileri varsayılan depolama hesabı yerine tüm ilgili kümelerin dağıtımında belirtilen bağlantılı depolama hesabına yerleştirmeniz önerilir. Bağlantılı Depolama hesaplarını yapılandırma hakkında daha fazla bilgi için bkz: [oluşturma HDInsight kümeleri](hdinsight-hadoop-provision-linux-clusters.md). Ancak özgün HDInsight kümesi silindikten sonra varsayılan depolama kapsayıcısını yeniden kullanabilirsiniz. HBase kümeleri için, silinmiş bir HBase kümesi tarafından kullanılan varsayılan blob kapsayıcısını kullanan yeni bir HBase kümesi oluşturarak HBase tablo şemasını ve verileri tutabilirsiniz.

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]

## <a name="interacting-with-azure-storage"></a>Azure depolama ile etkileşim kurma

Microsoft Azure depolama ile çalışmak için aşağıdaki araçları sağlar:

| Aracı | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Azure portal](../storage/blobs/storage-quickstart-blobs-portal.md) |✔ |✔ |✔ |
| [Azure CLI](../storage/blobs/storage-quickstart-blobs-cli.md) |✔ |✔ |✔ |
| [Azure PowerShell](../storage/blobs/storage-quickstart-blobs-powershell.md) | | |✔ |
| [AzCopy](../storage/common/storage-use-azcopy-v10.md) |✔ | |✔ |

## <a name="use-additional-storage-accounts"></a>Ek depolama hesaplarını kullanma

HDInsight kümesi oluştururken ilişkilendirmek istediğiniz Azure Depolama hesabını belirtirsiniz. Bu depolama hesabına ek olarak, oluşturma işlemi sırasında veya bir küme oluşturulduktan sonra aynı Azure aboneliğinden veya farklı Azure aboneliklerinden başka depolama hesapları ekleyebilirsiniz. Ek depolama hesapları ekleme hakkında yönergeler için bkz. [HDInsight kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md).

> [!WARNING]  
> HDInsight kümesinden farklı bir konumda ek depolama hesabının kullanılması desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede HDFS ile uyumlu Azure Depolama'yı HDInsight ile nasıl kullanacağınızı öğrendiniz. Bu, ölçeklenebilir, uzun vadeli, arşivlemeli veri edinme çözümleri oluşturmanıza ve depolanan yapılandırılmış ve yapılandırılmamış verilerdeki bilgilerin kilidini açmak için HDInsight kullanmanıza olanak sağlar.

Daha fazla bilgi için bkz.

* [Azure HDInsight ile çalışmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Azure Data Lake Store ile çalışmaya başlama](../data-lake-store/data-lake-store-get-started-portal.md)
* [HDInsight için karşıya veri yükleme](hdinsight-upload-data.md)
* [Apache Hive, HDInsight ile kullanma](hadoop/hdinsight-use-hive.md)
* [Apache Pig, HDInsight ile kullanma](hadoop/hdinsight-use-pig.md)
* [HDInsight ile verilere erişimi kısıtlamak için Azure depolama paylaşılan erişim imzaları kullanma](hdinsight-storage-sharedaccesssignature-permissions.md)
* [Azure Data Lake depolama Gen2 Azure HDInsight kümeleri ile kullanma](hdinsight-hadoop-use-data-lake-storage-gen2.md)