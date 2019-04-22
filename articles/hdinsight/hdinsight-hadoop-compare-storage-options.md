---
title: Azure HDInsight kümeleri ile kullanılmak üzere depolama seçeneklerini karşılaştırma
description: Depolama türleri ve Azure HDInsight ile nasıl çalıştıkları hakkında genel bir bakış sağlar.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/08/2019
ms.openlocfilehash: ac1a0e4eadc0b84fdd2a170c2e0f6e0a2f2af3a4
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59361788"
---
# <a name="compare-storage-options-for-use-with-azure-hdinsight-clusters"></a>Azure HDInsight kümeleri ile kullanılmak üzere depolama seçeneklerini karşılaştırma

HDInsight kümeleri oluştururken birkaç farklı Azure depolama hizmetleri arasında seçim yapabilirsiniz:

* Azure Storage
* Azure Data Lake Storage Gen2
* Azure Data Lake Storage Gen1

Bu makalede, bu depolama türleri ve benzersiz özelliklerine genel bakış sağlar.

HDInsight'ın farklı sürümleriyle desteklenen Azure depolama hizmetleri aşağıdaki tabloda özetlenmiştir:

| Depolama birimi hizmeti | Hesap türü | Namespace türü | Desteklenen hizmetler | Desteklenen performans katmanları | Desteklenen erişim katmanları | HDInsight Sürümü | Küme türü |
|---|---|---|---|---|---|---|---|
|Azure Data Lake Storage Gen2| Genel amaçlı V2 | Hiyerarşik (dosya sistemi) | Blob | Standart | Sık erişimli, seyrek erişimli ve Arşiv | 3.6 + | Tümü |
|Azure Storage| Genel amaçlı V2 | Nesne | Blob | Standart | Sık erişimli, seyrek erişimli ve Arşiv | 3.6 + | Tümü |
|Azure Storage| Genel amaçlı V1 | Nesne | Blob | Standart | Yok | Tümü | Tümü |
|Azure Storage| Blob Depolama | Nesne | Blob | Standart | Sık erişimli, seyrek erişimli ve Arşiv | Tümü | Tümü |
|Azure Data Lake Storage Gen1| Yok | Hiyerarşik (dosya sistemi) | Yok | Yok | Yok | Yalnızca 3.6 | HBase dışında tümü |

Azure depolama erişim katmanları hakkında daha fazla bilgi için bkz. [Azure Blob Depolama: Premium (Önizleme), sık erişimli, seyrek erişimli ve Arşiv depolama katmanları](../storage/blobs/storage-blob-storage-tiers.md)

Birincil ve isteğe bağlı ikincil depolama hizmetleri farklı birleşimlerini kullanarak bir küme oluşturabilirsiniz. Aşağıdaki tabloda, HDInsight şu anda desteklenen küme depolama yapılandırmaları özetlenmektedir:

| HDInsight Sürümü | Birincil Depolama | İkincil depolama | Desteklenen |
|---|---|---|---|
| 3.6 & 4.0 | Standard Blob | Standard Blob | Evet |
| 3.6 & 4.0 | Standard Blob | Data Lake Storage Gen2 | Hayır |
| 3.6 & 4.0 | Standard Blob | Data Lake Storage Gen1 | Evet |
| 3.6 & 4.0 | Data Lake depolama 2. nesil * | Data Lake Storage Gen2 | Evet |
| 3.6 & 4.0 | Data Lake depolama 2. nesil * | Standard Blob | Evet |
| 3.6 & 4.0 | Data Lake Storage Gen2 | Data Lake Storage Gen1 | Hayır |
| 3.6 | Data Lake Storage Gen1 | Data Lake Storage Gen1 | Evet |
| 3.6 | Data Lake Storage Gen1 | Standard Blob | Evet |
| 3.6 | Data Lake Storage Gen1 | Data Lake Storage Gen2 | Hayır |
| 4.0 | Data Lake Storage Gen1 | Herhangi biri | Hayır |

* = Aynı yönetilen kimliği kümeye erişim için kullanılacak tüm Kurulum oldukları sürece bu bir veya birden çok Data Lake depolama Gen2 hesapları olabilir.

## <a name="use-azure-data-lake-storage-gen2-with-apache-hadoop-in-azure-hdinsight"></a>Azure HDInsight, Apache Hadoop ile Azure Data Lake depolama Gen2'ı kullanma

Azure Data Lake depolama Gen2'ye alır core, Azure Data Lake depolama Gen1 özellikleri ve bunları Azure Blob depolama alanına tümleştirir. Bu özellikler Azure Active Directory (Azure AD), Hadoop ile uyumlu bir dosya sistemi ve POSIX tabanlı erişim denetimi listeleri (ACL'ler). Bu birleşim çalışırken de Blob Depolama katmanlama ve veri Yaşam Döngüsü Yönetimi'ni kullanarak Azure Data Lake depolama Gen1 performansını yararlanmanızı sağlar.

Azure Data Lake depolama Gen2 hakkında daha fazla bilgi için bkz. [Azure Data Lake depolama Gen2'ye Giriş](../storage/blobs/data-lake-storage-introduction.md).

### <a name="core-functionality-of-azure-data-lake-storage-gen2"></a>Azure Data Lake depolama Gen2'ın çekirdek işlevselliği

* **Hadoop ile uyumlu erişim:** Azure Data Lake depolama Gen2 yönetin ve bir Hadoop dağıtılmış dosya sistemi (HDFS) ile olduğu gibi veri erişim. Azure Blob dosya sistemi (ABFS) sürücü, Azure HDInsight ve Azure Databricks dahil olmak üzere tüm Apache Hadoop ortamlarında içinde kullanılabilir. Data Lake depolama 2. nesil'deki depolanan verilere erişmek için ABFS kullanın.

* **POSIX izinleri kümesi:** ACL ve POSIX izinlerle birlikte için Data Lake depolama Gen2'ye özel bazı ek ayrıntı Data Lake Gen2 güvenlik modelini destekler. Ayarlar, Yönetim Araçları veya Apache Hive ve Apache Spark gibi çerçeveleri aracılığıyla yapılandırılabilir.

* **Maliyet verimliliği:** Data Lake depolama Gen2, düşük maliyetli depolama kapasitesi ve işlem sunar. Azure Blob Depolama yaşam döngüsü gibi özellikler, faturalandırma ücretleri, yaşam döngüsü boyunca veri taşıyan olarak ayarlayarak maliyetleri yardımcı olur.

* **Blob Depolama Araçlar, çerçeveler ve uygulamalar ile uyumluluğu:** Data Lake depolama Gen2'ye bir çeşit Araçlar, çerçeveler ve uygulamalar için Blob Depolama ile çalışmaya devam eder.

* **En iyi duruma getirilmiş sürücü:** ABFS sürücü, özellikle büyük veri analizi için optimize edilmiştir. Karşılık gelen REST API'leri, dağıtılmış dosya sistemi (DFS) uç noktası aracılığıyla çıkmış dfs.core.windows.net.

### <a name="whats-new-for-azure-data-lake-storage-gen-2"></a>Azure Data Lake depolama Gen 2 için yenilikler nelerdir?

#### <a name="managed-identities-for-secure-file-access"></a>Güvenli dosya erişimi için kimlikleri yönetilen

Azure HDInsight, Azure Data Lake depolama Gen2 dosyalarında küme erişimin güvenliğini sağlamak için yönetilen kimlikleri kullanır. Yönetilen kimlikleri, otomatik olarak yönetilen kimlik bilgileri kümesi ile Azure hizmetleri sağlayan Azure Active Directory özelliğidir. Bu kimlik bilgileri Active Directory kimlik doğrulamasını destekleyen herhangi bir hizmet için kimlik doğrulaması için kullanılabilir. Yönetilen kimliklerle kod veya yapılandırma dosyalarında kimlik bilgilerini saklamak amacıyla gerektirmez.

Daha fazla bilgi için [kimliklerini Azure kaynakları için yönetilen](../active-directory/managed-identities-azure-resources/overview.md).

#### <a name="azure-blob-file-system-driver"></a>Azure Blob dosya sistemi sürücüsü

Apache Hadoop uygulamaları yerel olarak okuma ve yerel disk depolama alanından verileri yazma beklenir. Bir Hadoop dosya sistemi sürücüsü ABFS gibi Hadoop uygulamaları tarafından normal Hadoop dosya sistemi işlemleri öykünen bulut depolamasıyla çalışmayı sağlar. Sürücü, gerçek bir bulut depolama platformu anlayan işlemler uygulamadan alınan komutlarda dönüştürür.

Daha önce Hadoop dosya sistemi sürücüsü, tüm dosya sistemi işlemleri Azure depolama REST API çağrıları istemci tarafında dönüştürülür ve sonra REST API çağrılır. Dosya yeniden adlandırma gibi tek bir dosya sistemi işlemi için birden çok REST API ile sonuçlandı. Bu istemci-tarafı dönüştürme, ancak çağırır. ABFS Hadoop dosya sisteminin mantığı bazıları için sunucu tarafı istemci tarafı taşınmıştır. Azure Data Lake depolama Gen2 API artık Blob API ile paralel çalışır. Artık ortak Hadoop dosya sistemi işlemleri yürütülür çünkü bu geçiş, performansı geliştirir. bir REST API çağrısı ile.

Daha fazla bilgi için [Azure Blob dosya sistemi sürücü (ABFS): Hadoop için adanmış bir Azure depolama sürücüsü](../storage/blobs/data-lake-storage-abfs-driver.md).

#### <a name="uri-scheme-for-azure-data-lake-storage-gen-2"></a>Azure Data Lake depolama Gen 2 için URI şeması 

Azure Data Lake depolama Gen2 yeni bir URI şeması HDInsight Azure depolamadaki dosyalara erişmek için kullanır:

`abfs[s]://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/<PATH>`

SSL şifreli erişim URI düzeni sağlar (`abfss://` önek) ve şifrelenmemiş erişim (`abfs://` önek). Kullanım `abfss` mümkün olduğunda, hatta azure'da aynı bölgede bulunan verilere erişirken.

`<FILE_SYSTEM_NAME>` Data Lake depolama Gen2'ye dosya sistemi yolunu tanımlar.

`<ACCOUNT_NAME>` Azure depolama hesabı adını tanımlar. Tam uygun etki alanı adı (FQDN) gereklidir.

`<PATH>` Dosya veya dizin HDFS yol adıdır.

Değilse değerleri `<FILE_SYSTEM_NAME>` ve `<ACCOUNT_NAME>` olmayan belirtilmezse, varsayılan dosya sistemi kullanılır. Varsayılan dosya sistemindeki dosyalar için göreli bir yol veya mutlak bir yol kullanın. Örneğin, `hadoop-mapreduce-examples.jar` HDInsight kümeleriyle gelen dosya başvurulabilir aşağıdaki yollardan birini kullanarak:

```
abfss://myfilesystempath@myaccount.dfs.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
abfss:///example/jars/hadoop-mapreduce-examples.jar /example/jars/hadoop-mapreduce-examples.jar
```

> [!Note]
> Dosya adı `hadoop-examples.jar` HDInsight sürüm 2.1 ve 1.6 kümelerinde. HDInsight dışında dosyalarla çalışırken, yardımcı programların çoğu ABFS biçimlendirme ancak bunun yerine bir temel yol biçimi gibi bekler tanımıyoruz bulabilirsiniz `example/jars/hadoop-mapreduce-examples.jar`.

Daha fazla bilgi için [Azure Data Lake depolama Gen2 URI'si kullanma](../storage/blobs/data-lake-storage-introduction-abfs-uri.md).

## <a name="azure-storage"></a>Azure Storage

Azure depolama, HDInsight ile sorunsuz bir şekilde tümleşen güçlü genel amaçlı depolama çözümüdür. HDInsight, Azure Depolama’daki bir blob kapsayıcıyı kümenin varsayılan dosya sistemi olarak kullanabilir. HDFS arabirimi aracılığıyla, eksiksiz bir bileşen HDInsight BLOB olarak depolanan doğrudan yapılandırılmış veya yapılandırılmamış veriler üzerinde çalışabilir.

HDInsight günlüklerini ve geçici dosyalar kendi iş verilerinden yalıtmak için ayrı depolama kapsayıcıları için varsayılan küme depolama alanınızı ve iş verilerinizi kullanılması önerilir. Ayrıca uygulama ve sistem günlüklerini içeren varsayılan blob kapsayıcısını silme öneririz depolama maliyetini azaltmak için her kullanımdan sonra. Kapsayıcıyı silmeden önce günlükleri aldığınızdan emin olun.

Depolama hesabınızın güvenliğini sağlamak tercih ederseniz **güvenlik duvarları ve sanal ağlar** kısıtlamalar **seçili ağlar**, özel durum etkinleştirdiğinizden emin olun **güvenilen Microsoft izin ver Hizmetleri...**  HDInsight depolama hesabınıza erişebilmesi için.

### <a name="hdinsight-storage-architecture"></a>HDInsight depolama mimarisi

Aşağıdaki diyagram, HDInsight Azure depolama mimarisine ilişkin özet görünümünü sağlar:

![Nasıl Hadoop kümeleri erişmek ve Blob depolamada yapılandırılmış ve yapılandırılmamış verileri depolamak için HDFS API'sini kullanır gösteren diyagram](./media/hdinsight-hadoop-compare-storage-options/HDI.WASB.Arch.png "HDInsight depolama mimarisi")

HDInsight, işlem düğümlerine yerel olarak bağlı olan dağıtılmış dosya sistemine erişim imkanı sağlar. Bu dosya sistemine tam uygun URI kullanılarak erişilebilir, örneğin:

    hdfs://<namenodehost>/<path>

HDInsight ile Azure Depolama'daki verilere erişebilir. Söz dizimi aşağıdaki gibidir:

    wasb[s]://<containername>@<accountname>.blob.core.windows.net/<path>

HDInsight kümeleri ile bir Azure depolama hesabını kullanırken aşağıdaki ilkeleri göz önünde bulundurun:

* **Bir kümeye bağlı depolama hesaplarındaki kapsayıcılar:** Oluşturma sırasında hesap adı ve anahtar kümeyle ilişkili olduğundan bu kapsayıcılardaki blob'lara tam erişiminiz vardır.

* **Genel kapsayıcılar veya genel BLOB'lar olduğu depolama hesaplarında *değil* bir kümeye bağlı:** Kapsayıcılardaki blob'lara salt okunur iznine sahip.
  
  > [!NOTE]  
  > Genel kapsayıcılar, bu kapsayıcıda bulunan tüm BLOB'ları listesini almak ve kapsayıcı meta verilerini almak için olanak sağlar. Genel blob'lar, yalnızca tam URL'yi biliyorsanız blob erişiminize izin verir. Daha fazla bilgi için bkz. [Kapsayıcılara ve bloblara anonim okuma erişimini yönetme](../storage/blobs/storage-manage-access-to-resources.md).

* **Olan depolama hesaplarındaki özel kapsayıcılar *değil* bir kümeye bağlı:** WebHCat işleri gönderdiğinizde depolama hesabını tanımlamadığınız sürece kapsayıcılardaki blob'lara erişemezsiniz. 

Oluşturma işleminde tanımlanan depolama hesapları ve bunların anahtarların %HADOOP_HOME%/conf/core-site.xml küme düğümlerinde depolanır. Varsayılan olarak, HDInsight core-site.xml dosyasında tanımlanan depolama hesaplarını kullanır. Bu ayarı kullanarak değiştirebilirsiniz [Apache Ambari](./hdinsight-hadoop-manage-ambari.md).

Depolama hesapları ve bunların meta veri açıklamasını Apache Hive, MapReduce, akış Apache Hadoop ve Apache Pig dahil olmak üzere birden çok WebHCat işleri gerçekleştirebilirsiniz. (Bu Pig için şu anda depolama hesaplarıyla ancak meta veriler için geçerlidir.) Daha fazla bilgi için [alternatif depolama hesapları ve meta depolarla HDInsight kümesi kullanarak](https://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).

Bloblar yapılandırılmış ve yapılandırılmamış veriler için kullanılabilir. Blob kapsayıcıları, anahtar/değer çiftlerinin ve dizin hiyerarşisi olan verileri depolar. Ancak anahtar adı, bir dosyayı dizin yapısında depolanmış gibi görünmesini sağlamak için bir eğik çizgi karakteri (/) içerebilir. Örneğin, bir blob'un anahtarı olabilir `input/log1.txt`. Hayır gerçek `input` dizin var, ancak anahtar adında eğik çizgi karakteri nedeniyle bir dosya yolu gibi anahtar arar.

### <a id="benefits"></a>Azure Depolamanın yararları
İşlem kümelerini ve birlikte olmayan depolama kaynaklarını performans maliyetleri kapsanan. Hesaplama kümeleri Azure bölgesindeki depolama hesabı kaynaklarına yakın oluştururken olduğu gibi tarafından bu maliyetleri azalır. Bu bölgede, işlem düğümlerine verimli bir şekilde Azure depolama içinde yüksek hızlı ağ üzerinden verilere erişebilir.

Verileri HDFS yerine Azure depolama içinde depolamak, çeşitli avantajlar alın:

* **Verileri yeniden kullanma ve paylaşma:** HDFS'deki veriler işlem kümesi içinde bulunur. Yalnızca işlem kümesi erişimi olan uygulamalar HDFS API'lerini kullanarak verileri kullanabilir. Aksine, Azure Depolama'daki verilere HDFS API'si veya Blob Depolama REST API'leri erişilebilir. Bu düzenleme nedeniyle daha büyük bir (diğer HDInsight kümeleri dahil olmak üzere) uygulama ve araçların oluşturmak ve verileri kullanmak için kullanılabilir.

* **Veri arşivleme:** Veriler Azure Depolama'da depolanır, hesaplama için kullanılan HDInsight kümelerinin kullanıcı verilerini kaybetmeden güvenle silinebilir.

* **Veri depolama maliyeti:** Verileri DFS uzun süreli depolamak, işlem kümesinin maliyeti Azure depolama maliyeti yüksek olduğu için verileri Azure depolamada depolamaktan daha maliyetlidir. Ayrıca, verilerin her işlem kümesi oluşturmada yeniden yüklenmesi olmadığı için veri yükleme maliyetleri kaydediyorsanız.

* **Esnek ölçeklendirme:** HDFS size ölçeklendirilmiş dosya sistemi ile sağlar ancak ölçek kümeniz için oluşturduğunuz düğüm sayısı tarafından belirlenir. Ölçeği değiştirmek, esnek ölçeklendirme özelliklere Azure depolamada otomatik olarak aldığınız güvenmek değerinden daha karmaşık olabilir.

* **Coğrafi çoğaltma:** Azure depolama, coğrafi olarak çoğaltılmış olabilir. Coğrafi çoğaltma, coğrafi kurtarma ve veri yedekliği sağlamakla rağmen coğrafi olarak çoğaltılmış konumu için bir yük devretme performansınızı ciddi bir şekilde etkiler ve ek ücrete neden olabilecek. Bu nedenle dikkatli ve yalnızca coğrafi çoğaltma seçin, veri değeri ek maliyetlere hizalar.

Bazı MapReduce işleri ve paketleri, Azure depolamada depolamak için istemezsiniz Ara sonuçlar oluşturabilir. Bu durumda, verileri yerel HDFS'de depolamak üzere seçebilirsiniz. HDInsight Hive işleri ve diğer işlemlerdeki bu Ara sonuçların bazıları için DFS kullanır.

> [!NOTE]  
> Çoğu HDFS komutu (örneğin, `ls`, `copyFromLocal`, ve `mkdir`) Azure Depolama'da beklendiği gibi çalışmayabilir. Gibi (DFS adlandırılır) yerel HDFS uygulamasına özgü komutlar `fschk` ve `dfsadmin`, Azure depolamada farklı bir davranış gösterir.

## <a name="overview-of-azure-data-lake-storage-gen1"></a>Azure Data Lake depolama Gen1 genel bakış

Azure Data Lake depolama Gen1, büyük veri analizi iş yükleri için kuruluş çapında hiper ölçekli bir depodur. Azure Data Lake kullanarak, herhangi bir boyut, türü ve işletimsel ve keşfe dönük çözümleme için tek bir yerde alma hızı verileri yakalayabilirsiniz.

Erişim Data Lake depolama Gen1 hadoop'tan WebHDFS ile uyumlu REST API'leri kullanarak (bir HDInsight kümesiyle kullanılabilir). Data Lake depolama Gen1 depolanan veriler üzerinde analiz sağlamak üzere tasarlanmıştır ve veri analizi senaryoları performans için ayarlanmıştır. Kullanıma hazır, gerçek Kurumsal kullanım vakaları için temel özellikleri içerir. Bu özellikler, güvenlik, yönetilebilirlik, ölçeklenebilirlik, güvenilirlik ve kullanılabilirlik içerir.

Azure Data Lake depolama Gen1 hakkında daha fazla bilgi için bkz. ayrıntılı [genel bakış, Azure Data Lake depolama Gen1](../data-lake-store/data-lake-store-overview.md).

Data Lake depolama Gen1 temel özellikleri aşağıda verilmiştir.

### <a name="compatibility-with-hadoop"></a>Hadoop ile uyumluluk

Data Lake depolama Gen1 ve Hadoop ekosistemiyle çalışan HDFS ile uyumlu bir Apache Hadoop dosya sistemidir.  Var olan HDInsight uygulamaları veya WebHDFS API'sini kullanan hizmetler, Data Lake depolama Gen1 ile kolayca tümleştirilebilir. Data Lake depolama Gen1 ayrıca bir uygulamalar için WebHDFS ile uyumlu REST arabirimini kullanıma sunar.

Data Lake depolama Gen1 içinde depolanan veriler, MapReduce veya Hive gibi Hadoop analitik çerçeveler kullanılarak kolayca çözümlenebilir. Azure HDInsight kümeleri sağlanabilir ve Data Lake depolama Gen1 içinde depolanan verileri doğrudan erişim sağlamak için yapılandırılmış.

### <a name="unlimited-storage-petabyte-files"></a>Sınırsız depolama, petabayt boyutlu dosyalar

Data Lake depolama Gen1 sınırsız depolama sağlar ve veri analizi için çeşitli depolamak için uygundur. Bu hesap boyutları, dosya boyutları veya bir veri gölü içinde depolanabilen veri miktarı barındırabileceğiniz koymaz. Tek tek dosya boyutu kilobayt petabayta kadar Data Lake depolama Gen1 her türden veriyi depolamak için harika bir seçim yaparak değişebilir. Birden çok kopya oluşturularak veriler arızaya depolanır ve sınırı yoktur verileri data lake üzerinde depolanabilir ne kadar süre.

### <a name="performance-tuning-for-big-data-analytics"></a>Performans için büyük veri analizi ayarlama

Data Lake depolama Gen1, sorgulanması ve büyük miktarlarda verinin çözümlenmesi için devasa bir işlem hacmi gerektiren büyük ölçekli analiz sistemlerini çalıştırmak için yerleşik olarak bulunur. Veri gölü, bir dosyanın parçalarını birkaç ayrı depolama sunucusu üzerinde yayılır. Veri çözümlediğiniz zaman, bu kurulum dosyanın paralel olarak okunduğunda okuma verimini artırır.

### <a name="readiness-for-enterprise-highly-available-and-secure"></a>Kuruluş için hazırlık: Yüksek oranda kullanılabilir ve güvenli

Data Lake depolama Gen1 endüstri standardı kullanılabilirlik ve güvenilirlik sağlar. Veri varlıklarını arızaya depolanır: yedek kopyaları beklenmeyen hatalarına karşı koruma sağlamak. Kuruluşlar, kendi çözümlerinde, mevcut veri platformu önemli bir parçası Data Lake depolama Gen1 kullanabilirsiniz.

Data Lake depolama Gen1 ayrıca depolanan veriler için kurumsal düzeyde güvenlik sağlar. Daha fazla bilgi için [Azure Data Lake depolama Gen1 verileri güvenli hale getirme](#DataLakeStoreSecurity).

### <a name="flexible-data-structures"></a>Esnek veri yapıları

Data Lake depolama Gen1 herhangi bir veri depolayabilir, yerel biçiminde, olduğu gibi önceki dönüşümleri gerek kalmadan. Data Lake depolama Gen1 bir şema, veriler yüklenmeden önce tanımlanmasını gerektirmez. Ayrı analitik çerçeveye verileri yorumlar ve analiz zamanında bir şema tanımlar. İsteğe bağlı boyut ve biçimlerdeki dosyaların depolayabileceğiniz için Data Lake depolama Gen1 yapılandırılmış, yarı yapılandırılmış ve yapılandırılmamış verileri işleyebilir.

Veriler için Data Lake depolama Gen1 kapsayıcılar olan temelde klasörleri ve dosyaları. Depolanan verilerde, SDK'ları, Azure portalı ve Azure PowerShell kullanarak çalışır. Verilerinizi bu arabirimleri ve ilgili kapsayıcıları kullanarak depoya yerleştirdiğiniz sürece, her türden veriyi depolayabilirsiniz. Data Lake depolama Gen1, depoladığı verilerin türüne göre veri herhangi bir özel işlem gerçekleştirmez.

## <a name="DataLakeStoreSecurity"></a>Data Lake depolama Gen1 veri güvenliği
Data Lake depolama Gen1 kullanan Azure Active Directory kimlik doğrulaması ve kullandığı erişim için verilerinizi listeleri (ACL) erişimi yönetmek üzere denetler.

| **Özellik** | **Açıklama** |
| --- | --- |
| Authentication |Data Lake depolama Gen1, Data Lake depolama Gen1 içinde depolanan tüm veriler için kimlik ve erişim yönetimi için Azure Active Directory (Azure AD) ile tümleşir. Tümleştirme nedeniyle, Data Lake depolama Gen1 tüm Azure AD özelliklerden faydalanır. Bu özellikler, çok faktörlü kimlik doğrulaması, koşullu erişim, rol tabanlı erişim denetimi, uygulama kullanımını izleme, güvenlik izleme ve uyarı içerir ve benzeri. Data Lake depolama Gen1 REST arabirimi içinde kimlik doğrulaması için OAuth 2.0 protokolünü destekler. Bkz: [kimlik doğrulaması Azure Active Directory'yi kullanarak Azure Data Lake depolama Gen1 içinde](../data-lake-store/data-lakes-store-authentication-using-azure-active-directory.md)|
| Erişim denetimi |Data Lake depolama Gen1 WebHDFS protokolünün kullanıma sunulan POSIX stili izinleri destekleyerek erişim denetimi sağlar. ACL’ler kök klasörde, alt klasörlerde ve dosyalarda tek tek etkinleştirilebilir. ACL'ler Data Lake depolama Gen1 bağlamında nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Data Lake depolama Gen1'deki erişim denetimi](../data-lake-store/data-lake-store-access-control.md). |
| Şifreleme |Data Lake depolama Gen1 ayrıca hesapta depolanan veriler için şifreleme sağlar. Bir Data Lake depolama Gen1 hesabı oluşturulurken şifreleme ayarlarını belirtirsiniz. Verilerinizin şifrelenmesini tercih ya da şifrelemeyi kabul seçebilirsiniz. Daha fazla bilgi için [şifreleme Data Lake depolama Gen1](../data-lake-store/data-lake-store-encryption.md). Bir şifreleme tabanlı yapılandırma sağlama konusunda yönergeler için bkz. [Azure Data Lake depolama Gen1 ile çalışmaya başlama Azure portalını kullanarak](../data-lake-store/data-lake-store-get-started-portal.md). |

Data Lake depolama Gen1 veri güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure Data Lake depolama Gen1 depolanan verilerin güvenliğini sağlama](../data-lake-store/data-lake-store-secure-data.md).

## <a name="applications-that-are-compatible-with-data-lake-storage-gen1"></a>Data Lake depolama Gen1 ile uyumlu olmayan uygulamaları
Data Lake depolama Gen1, Hadoop ekosistemindeki çoğu açık kaynak bileşenler ile uyumludur. Ayrıca diğer Azure hizmetleriyle sorunsuz şekilde tümleştirilir.  Data Lake depolama Gen1 hem açık kaynak bileşenlerini ve bunun yanı sıra diğer Azure Hizmetleri ile kullanma hakkında hakkında daha fazla bilgi için aşağıdaki bağlantıları izleyin.

* Bkz: [Azure Data Lake depolama Gen1 ile çalışan açık kaynaklı büyük veri uygulamaları](../data-lake-store/data-lake-store-compatible-oss-other-applications.md) Data Lake depolama Gen1 ile çalışan açık kaynaklı uygulamaları listesi.
* Bkz: [diğer Azure Hizmetleri ile Azure Data Lake depolama Gen1 tümleştirme](../data-lake-store/data-lake-store-integrate-with-other-services.md) Data Lake depolama Gen1 diğer Azure hizmetleriyle geniş bir senaryoları etkinleştirmek için nasıl kullanılacağını anlamak için.
* Bkz: [büyük veri gereksinimleri için Azure Data Lake depolama Gen1 kullanarak](../data-lake-store/data-lake-store-data-scenarios.md) veri alma, veri işleme, veri indirme ve veri görselleştirme gibi senaryolarda Data Lake depolama Gen1 kullanmayı öğrenin.

## <a name="data-lake-storage-gen1-file-system-adl"></a>Data Lake depolama Gen1 dosya sistemi (adl: / /)
Hadoop ortamlarında (bir HDInsight kümesiyle kullanılabilir) yeni dosya sistemi AzureDataLakeFilesystem Data Lake depolama Gen1 erişebilirsiniz (adl: / /). Adl kullanan Hizmetleri ve uygulamaları performans: / / geçerli olarak WebHDFS şu anda kullanılabilir olmayan bir şekilde iyileştirilebilir. Data Lake depolama Gen1 kullandığınızda, sonuç olarak, ya da mevcut için en iyi performansı önerilen adl kullanarak bağlayabilir: / / veya doğrudan WebHDFS API'yi kullanmaya devam ederek var olan kod güncelleştirin. Azure HDInsight üzerinde Data Lake depolama Gen1 en iyi performansı sağlamak üzere AzureDataLakeFilesystem yararlanır.

Aşağıdakileri kullanarak verilerinizi Data Lake depolama Gen1 erişin:

`adl://<data_lake_storage_gen1_name>.azuredatalakestore.net`

Data Lake depolama Gen1 verilere erişim hakkında daha fazla bilgi için bkz: [depolanan verilerde kullanılabilen eylemler](../data-lake-store/data-lake-store-get-started-portal.md#properties).



## <a name="next-steps"></a>Sonraki adımlar

* [Azure Data Lake depolama Gen2'ye Giriş](../storage/blobs/data-lake-storage-introduction.md)
* [Azure Depolama’ya giriş](../storage/common/storage-introduction.md)
