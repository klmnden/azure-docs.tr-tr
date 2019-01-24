---
title: Azure HDInsight kümeleri ile kullanılmak üzere depolama seçeneklerini karşılaştırma
description: Depolama türleri ve Azure HDInsight ile nasıl çalıştıkları hakkında genel bir bakış sağlar.
services: hdinsight,storage
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/20/2019
ms.openlocfilehash: 14db76068cc11d3f57a72e3e540a5e0da7e1c254
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54853621"
---
# <a name="compare-storage-options-for-use-with-azure-hdinsight-clusters"></a>Azure HDInsight kümeleri ile kullanılmak üzere depolama seçeneklerini karşılaştırma

Azure HDInsight kullanıcıları, HDInsight kümeleri oluştururken birkaç farklı depolama seçeneğinden birini seçebilirsiniz:

* Azure Data Lake Storage Gen2
* Azure Storage
* Azure Data Lake Storage Gen1

Bu makalede, bu farklı depolama türlerini ve bunların benzersiz özelliklerine genel bakış sağlar.

## <a name="use-azure-data-lake-storage-gen2-with-apache-hadoop-in-azure-hdinsight"></a>Azure HDInsight, Apache Hadoop ile Azure Data Lake depolama Gen2'ı kullanma

Azure Data Lake depolama Gen2 hakkında daha fazla bilgi için bkz. [Azure Data Lake depolama Gen2'ye Giriş](../storage/blobs/data-lake-storage-introduction.md).

Azure Data Lake depolama Gen2'ye alır çekirdek özellikler Azure Data Lake depolama Gen1 gelen gibi Hadoop uyumlu bir dosya sistemi, Azure Active Directory ve POSIX tabanlı erişim denetim listeleri (ACL) ve bunları Azure Blob Depolama tümleştirilir. Bu birleşim de Blob Depolama'nın katmanlama ve veri Yaşam Döngüsü Yönetimi'ni kullanırken Azure Data Lake depolama Gen1 performansını yararlanmanızı sağlar.

### <a name="core-functionality-of-azure-data-lake-storage-gen2"></a>Azure Data Lake depolama Gen2'ın çekirdek işlevselliği

* Hadoop uyumlu erişim: Azure Data Lake depolama Gen2, yönetmek ve bir Hadoop dağıtılmış dosya sistemi (HDFS) ile olduğu gibi veri erişim sağlar. Azure Blob dosya sistemi (ABFS) sürücü, Data Lake depolama 2. nesil'deki depolanan verilere erişmek için Azure HDInsight ve Azure Databricks dahil olmak üzere tüm Apache Hadoop ortamlarında kullanılabilir.

* POSIX izinleri kümesi: ACL ve POSIX izinlerle birlikte için Data Lake depolama Gen2'ye özel bazı ek ayrıntı Data Lake Gen2 güvenlik modelini destekler. Ayarları, Yönetim Araçları veya Apache Hive ve Apache Spark gibi çerçeveleri aracılığıyla yapılandırılabilir.

* Uygun maliyetli: Data Lake depolama Gen2, düşük maliyetli depolama kapasitesi ve işlem sunar. Azure Blob Depolama yaşam döngüsü gibi özellikler, veri yaşam döngüsü ile hareket ettikçe faturalandırma ücretleri ayarlayarak maliyetleri yardımcı olur.

* Blob Depolama Araçlar, çerçeveler ve uygulamalar ile çalışır: Data Lake depolama Gen2'ye bir çeşit Araçlar, çerçeveler ve Blob Depolama için bugün mevcut uygulamaları ile çalışmaya devam eder.

* En iyi duruma getirilmiş sürücü: ABFS sürücü, özellikle büyük veri analizi için optimize edilmiştir. Karşılık gelen REST API'leri dfs uç noktası aracılığıyla çıkmış dfs.core.windows.net.

### <a name="whats-new-about-azure-data-lake-storage-gen-2"></a>Azure Data Lake depolama Gen 2 yenilikler

#### <a name="managed-identities-for-secure-file-access"></a>Güvenli dosya erişimi için kimlikleri yönetilen

Azure HDInsight, Azure Data Lake depolama Gen2 dosyalarında küme erişimin güvenliğini sağlamak için yönetilen kimlikleri kullanır. Yönetilen kimlikleri, otomatik olarak yönetilen kimlik bilgileri kümesi ile Azure hizmetleri sağlayan Azure Active Directory özelliğidir. Bu kimlik bilgileri, AD kimlik doğrulamasını destekleyen herhangi bir hizmet için kimlik doğrulaması için kullanılabilir. Yönetilen kimliklerle kod veya yapılandırma dosyalarında kimlik bilgilerini saklamak amacıyla gerektirmez.

Daha fazla bilgi için [Azure kaynakları için yönetilen kimlikleri nedir](../active-directory/managed-identities-azure-resources/overview.md).

#### <a name="azure-blob-filesystem-abfs-driver"></a>Azure Blob dosya sistemi (ABFS) sürücü

Apache Hadoop uygulamaları yerel olarak okuma ve yerel disk depolama alanından verileri yazma beklenir. Bir Hadoop dosya sistemi sürücü ABFS gibi Hadoop uygulamaları tarafından normal Hadoop dosya sistemi işlemleri öykünen bulut depolamasıyla çalışmayı sağlar. Sürücü, gerçek bir bulut depolama platformu anlayan işlemler uygulamadan alınan komutlarda dönüştürür.

Daha önce Hadoop dosya sistemi sürücü tüm dosya sistemi işlemleri Azure depolama REST API çağrıları istemci tarafında dönüştürün ve ardından REST API çağırma. Birden çok REST API ile sonuçlandı. Bu istemci-tarafı dönüştürme, ancak dosyayı yeniden adlandırma gibi bir tek bir dosya sistemi işlemi için çağırır. ABFS bazı Hadoop dosya sistemi mantığı istemci tarafı sunucu tarafı ve Azure Data Lake depolama 2. nesil API için şimdi Blob API ile paralel çalışır taşınmıştır. Genel bir Hadoop dosya sistemi işlemleri artık yürütülebilir olduğundan bu geçiş, performansı geliştirir. bir REST API çağrısı ile.

Daha fazla bilgi için [Azure Blob dosya sistemi sürücü (ABFS): Hadoop için adanmış bir Azure depolama sürücüsü](../storage/blobs/data-lake-storage-abfs-driver.md).

#### <a name="azure-data-lake-storage-gen-2-uri-scheme"></a>Azure Data Lake depolama Gen 2 URI düzeni

Azure Data Lake depolama Gen2'ye HDInsight Azure depolamadaki dosyalara erişmek için yeni bir URI şeması kullanır:

`abfs[s]://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/<PATH>`

SSL şifreli erişim URI düzeni sağlar (`abfss://` önek) ve şifrelenmemiş erişim (`abfs://` önek). Kullanmanızı öneririz `abfss` mümkün olduğunda, hatta azure'da aynı bölgede bulunan verilere erişirken.

`<FILE_SYSTEM_NAME>` Data Lake depolama Gen2'ye dosya sistemi yolunu tanımlar.

`<ACCOUNT_NAME>` Azure depolama hesabı adını tanımlar. Tam uygun etki alanı adı (FQDN) gereklidir.

`<PATH>` Dosya veya dizin HDFS yol adıdır.

Değilse değerleri `<FILE_SYSTEM_NAME>` ve `<ACCOUNT_NAME>` olmayan belirtilmezse, varsayılan dosya sistemi kullanılır. Varsayılan dosya sistemindeki dosyalar için göreli bir yol veya mutlak bir yol kullanabilirsiniz. Örneğin, `hadoop-mapreduce-examples.jar` HDInsight kümeleriyle gelen dosya başvurulabilir aşağıdaki yollardan birini kullanarak:

```
abfss://myfilesystempath@myaccount.dfs.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
abfss:///example/jars/hadoop-mapreduce-examples.jar /example/jars/hadoop-mapreduce-examples.jar
```

> [!Note]
> Dosya adı `hadoop-examples.jar` HDInsight sürüm 2.1 ve 1.6 kümelerinde. HDInsight dışında dosyalarla çalışırken, yardımcı programların çoğu ABFS biçimlendirmek ve bunun yerine bir temel yol biçimi gibi bekler tanımaz `example/jars/hadoop-mapreduce-examples.jar`.

Daha fazla bilgi için [Azure Data Lake depolama Gen2 URI'si kullanma](../storage/blobs/data-lake-storage-introduction-abfs-uri.md).

## <a name="use-azure-storage"></a>Azure depolamayı kullanma

Azure depolama, HDInsight ile sorunsuz bir şekilde tümleşen, sağlam ve genel amaçlı bir depolama çözümüdür. HDInsight, Azure Depolama’daki bir blob kapsayıcıyı kümenin varsayılan dosya sistemi olarak kullanabilir. Hadoop dağıtılmış dosya sistemi (HDFS) arabirimi aracılığıyla, HDInsight’taki bileşenler kümesinin tümü blob olarak depolanan yapılandırılmış veya yapılandırılmamış veriler üzerinde doğrudan çalışabilir.

> [!WARNING]  
> Azure Depolama hesabı oluşturmanın birden fazla yolu vardır. Aşağıdaki tabloda HdInsight ile hangi seçeneklerin desteklendiği gösterilmektedir:

| Depolama hesabı türü | Desteklenen hizmetler | Desteklenen performans katmanları | Desteklenen erişim katmanları |
|----------------------|--------------------|-----------------------------|------------------------|
| Genel amaçlı V2   | Blob               | Standart                    | Sık, seyrek arşiv *    |
| Genel amaçlı V1   | Blob               | Standart                    | Yok                    |
| Blob depolama         | Blob               | Standart                    | Sık, seyrek arşiv *    |

İş verilerini depolamak için varsayılan blob kapsayıcısını kullanmanızı önermemekteyiz. Depolama maliyetini azaltmak için blob kapsayıcısının her kullanımdan sonra silinmesi iyi bir uygulamadır. Uygulama ve sistem varsayılan kapsayıcı içeren günlükleri. Kapsayıcıyı silmeden önce günlükleri aldığınızdan emin olun.

Bir blob kapsayıcısının birden fazla küme için varsayılan dosya sistemi olarak kullanma desteklenmiyor.
 
 > [!NOTE]  
 > Arşiv erişim katmanı bir birkaç saatlik alma gecikmesinin ve HDInsight ile kullanım için önerilmez çevrimdışı bir katmandır. Daha fazla bilgi için [arşiv erişim katmanı](../storage/blobs/storage-blob-storage-tiers.md#archive-access-tier).

### <a name="hdinsight-storage-architecture"></a>HDInsight depolama mimarisi
Aşağıdaki diyagram, Azure Depolama ile kullanılan HDInsight depolama mimarisine ilişkin bir özet görünüm sağlar:

![Hadoop kümeleri, Blob Depolamada yapılandırılmış ve yapılandırılmamış verilere erişmek ve depolamak için HDFS API’sini kullanır.](./media/hdinsight-hadoop-compare-storage-options/HDI.WASB.Arch.png "HDInsight Depolama Mimarisi")

HDInsight, işlem düğümlerine yerel olarak bağlı olan dağıtılmış dosya sistemine erişim imkanı sağlar. Bu dosya sistemine tam uygun URI kullanılarak erişilebilir, örneğin:

    hdfs://<namenodehost>/<path>

Ayrıca HDInsight, Azure Depolama'da depolanan verilere erişebilmenizi de sağlar. Söz dizimi aşağıdaki gibidir:

    wasb[s]://<containername>@<accountname>.blob.core.windows.net/<path>

HDInsight kümeleriyle Azure Depolama hesabını kullanırken dikkat etmeniz gereken bazı noktalar mevcuttur.

* **Bir kümeye bağlı depolama hesaplarındaki kapsayıcılar:** Oluşturma sırasında hesap adı ve anahtar kümeyle ilişkili olduğundan bu kapsayıcılardaki blob'lara tam erişiminiz vardır.

* **Genel kapsayıcılar veya bir kümeye bağlı olmayan depolama hesaplarındaki genel BLOB'lar:** Kapsayıcılardaki blob'lara salt okunur iznine sahip.
  
  > [!NOTE]  
  > Genel kapsayıcılar, bu kapsayıcıda bulunan tüm blob’ların bir listesini ve kapsayıcı meta verilerini almanıza olanak tanır. Genel blob'lar, yalnızca tam URL'yi biliyorsanız blob erişiminize izin verir. Daha fazla bilgi için [kapsayıcılara ve blob'lara erişimi yönetme](../storage/blobs/storage-manage-access-to-resources.md).

* **Bir kümeye bağlı olmayan depolama hesaplarındaki özel kapsayıcılar:** WebHCat işleri gönderdiğinizde depolama hesabını tanımlamadığınız sürece kapsayıcılardaki blob'lara erişemezsiniz. Bu, bu makalenin sonraki bölümlerinde açıklanmıştır.

Oluşturma işleminde tanımlanan depolama hesapları ve bunların anahtarların %HADOOP_HOME%/conf/core-site.xml küme düğümlerinde depolanır. HDInsight’ın varsayılan davranışı core-site.xml dosyasında tanımlanan depolama hesaplarını kullanmaktır. Bu ayarı kullanarak değiştirebilirsiniz [Apache Ambari](./hdinsight-hadoop-manage-ambari.md).

Depolama hesapları ve bunların meta veri açıklamasını Apache Hive, MapReduce, akış Apache Hadoop ve Apache Pig dahil olmak üzere birden çok WebHCat işleri gerçekleştirebilirsiniz. (Bu şu anda yalnızca Pig depolama hesapları ile çalışmakta, ancak meta verilerle çalışmamaktadır.) Daha fazla bilgi için bkz. [Alternatif Depolama Hesapları ve Meta Depolarla HDInsight Kümesi kullanma](https://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).

Bloblar yapılandırılmış ve yapılandırılmamış veriler için kullanılabilir. Blob kapsayıcıları, verileri anahtar/değer çiftleri olarak depolar ve dizin hiyerarşisi bulunmaz. Ancak, bir dosyayı dizin yapısında depolanmış gibi göstermek için anahtar adında eğik çizgi karakteri (/) kullanılabilir. Örneğin, bir blob'un anahtarı olabilir `input/log1.txt`. Hayır gerçek `input` dizin var, ancak anahtar adında eğik çizgi karakteri bulunması nedeniyle, bir dosya yolu görünümüne sahiptir.

### <a id="benefits"></a>Azure Depolamanın yararları
İşlem kümelerini ve depolama kaynaklarını yeniden bulmamanın örtülü performans maliyeti, yüksek hızlı ağın işlem düğümlerinin Azure Depolama’daki verilere erişimini verimli hale getirdiği, Azure bölgesindeki depolama hesabı kaynaklarına yakın şekilde oluşturulmasıyla azaltılabilir.

Verileri HDFS yerine Azure Depolama’da depolamanın çeşitli avantajları vardır:

* **Verileri yeniden kullanma ve paylaşma:** HDFS'deki veriler işlem kümesi içinde bulunur. Yalnızca işlem kümesi erişimi olan uygulamalar HDFS API'lerini kullanarak verileri kullanabilir. Azure Depolama'daki verilere HDFS API'si aracılığıyla veya Blob Depolama REST API'leri aracılığıyla erişilebilir. Bu nedenle, daha büyük uygulama kümeleri (diğer HDInsight kümeleri dahil olmak üzere) ve araçları veri üretmek ve kullanmak için kullanılabilir.
* **Veri arşivleme:** Verileri Azure depolamada depolamak, hesaplama için kullanılan kullanıcı verilerini kaybetmeden güvenle silinmesini HDInsight kümeleri sağlar.
* **Veri depolama maliyeti:** Verileri DFS uzun süreli depolamak, işlem kümesinin maliyeti Azure depolama maliyeti yüksek olduğu için verileri Azure depolamada depolamaktan daha maliyetlidir. Ayrıca, verilerin her işlem kümesi oluşturmada yeniden yüklenmesi olmadığından veri yükleme maliyetlerinden de kaydediyorsanız.
* **Esnek ölçeklendirme:** HDFS size ölçeklendirilmiş dosya sistemi ile sağlar ancak ölçek kümeniz için oluşturduğunuz düğüm sayısı tarafından belirlenir. Ölçeği değiştirmek, Azure depolamada otomatik olarak aldığınız esnek ölçeklendirme özelliklere bağlı kalmaktan daha karmaşık bir işlem haline gelebilir.
* **Coğrafi çoğaltma:** Azure depolama, coğrafi olarak çoğaltılmış olabilir. Bu, size coğrafi kurtarma ve veri yedekliği sağlamakla birlikte, coğrafi olarak çoğaltılan bir konuma yük devretmek performansınızı ciddi bir şekilde etkiler ve ek ücrete neden olabilir. Bu nedenle, coğrafi çoğaltmayı akıllıca ve yalnızca verilerin değeri ek maliyetlere değer durumdaysa tercih etmenizi öneririz.

Bazı MapReduce işleri ve paketleri gerçekte Azure depolamada depolamak istemediğiniz ara sonuçlar oluşturabilir. Bu durumda, verileri yerel HDFS’de depolamak üzere seçebilirsiniz. Aslında, HDInsight Hive işleri ve diğer işlemlerdeki bu ara sonuçların bazıları için DFS kullanır.

> [!NOTE]  
> Çoğu HDFS komutu (örneğin, `ls`, `copyFromLocal` ve `mkdir`) hala beklendiği gibi çalışmayabilir. Gibi (DFS adlandırılır) yerel HDFS uygulamasına özgü komutlar `fschk` ve `dfsadmin`, Azure depolamada farklı bir davranış gösterir.

## <a name="use-azure-data-lake-storage-gen1"></a>Azure Data Lake depolama Gen1 kullanın

Azure Data Lake depolama Gen1 hakkında daha fazla bilgi için bkz. [genel bakış, Azure Data Lake depolama Gen1](../data-lake-store/data-lake-store-overview.md).

Azure Data Lake depolama Gen1 bir büyük veri analizi iş yükleri için kuruluş çapında hiper ölçekli depodur. Azure Data Lake, işletimsel ve keşifsel analiz için herhangi bir boyuta, türe ve alma hızına sahip olan verileri tek bir konumda yakalamanıza olanak sağlar.

Data Lake depolama Gen1 Hadoop (bir HDInsight kümesiyle kullanılabilir) erişilebilir WebHDFS ile uyumlu REST API'lerini kullanarak. Bu depolanan veriler üzerinde analiz sağlamak üzere tasarlanmıştır ve veri analizi senaryoları için performans için ayarlanmıştır. Sağlandığı şekilde, gerçek kurumsal kullanım vakaları için temel önem taşıyan güvenlik, yönetilebilirlik, ölçeklenebilirlik, güvenilirlik ve kullanılabilirlik gibi tüm kurumsal düzeydeki özellikleri içerir.

![Azure Data Lake depolama](./media/hdinsight-hadoop-compare-storage-options/data-lake-store-concept.png "HDInsight depolama mimarisi")

Data Lake depolama Gen1 anahtar özelliklerinin bazıları aşağıda verilmiştir.

### <a name="built-for-hadoop"></a>Hadoop için geliştirilmiştir

Data Lake depolama Gen1, bir Apache Hadoop dosya sistemi Hadoop ekosistemiyle çalışan ve Hadoop dağıtılmış dosya sistemi (HDFS) ile uyumlu değil.  Var olan HDInsight uygulamaları veya WebHDFS API'sini kullanan hizmetler, Data Lake depolama Gen1 ile kolayca tümleştirilebilir. Data Lake depolama Gen1 WebHDFS ile uyumlu REST arabirimi uygulamalar için de sunar.

Data Lake depolama Gen1 içinde depolanan veriler, MapReduce veya Hive gibi Hadoop analitik çerçeveler kullanılarak kolayca çözümlenebilir. Microsoft Azure HDInsight kümeleri sağlanabilir ve Data Lake depolama Gen1 içinde depolanan verileri doğrudan erişim sağlamak için yapılandırılmış.

### <a name="unlimited-storage-petabyte-files"></a>Sınırsız depolama, petabayt boyutlu dosyalar

Data Lake depolama Gen1 sınırsız depolama sağlar ve veri analizi için çeşitli depolamak için uygundur. Hesap boyutları, dosya boyutları veya bir veri gölü içinde depolanabilen veri miktarı için herhangi bir sınırlama uygulamaz. Ayrı dosyalar kilobayttan petabayta kadar uzanan boyutlarda olabilir ve bu da herhangi bir tür verinin depolanması için harika bir seçim olmasını sağlar. Birden çok kopya oluşturularak veriler sağlam bir şekilde depolanır ve verilerin data lake içinde depolanmasına yönelik bir süre sınırı mevcut değildir.

### <a name="performance-tuned-for-big-data-analytics"></a>Büyük veri analizi için performans ayarı yapılmıştır

Data Lake depolama Gen1, büyük ölçekli sorgulanması ve büyük miktarlarda verinin çözümlenmesi için devasa bir işlem hacmi gerektiren analiz sistemlerini çalıştırmak üzere tasarlanmıştır. Veri gölü, bir dosyanın parçalarını birkaç ayrı depolama sunucusu üzerinde dağıtır. Bu, veri analizinin gerçekleştirilmesi için dosyanın paralel olarak okunması sırasında okuma verimini artırır.

### <a name="enterprise-ready-highly-available-and-secure"></a>Kurumsal kullanıma hazır: Yüksek oranda kullanılabilir ve güvenli

Data Lake depolama Gen1 endüstri standardı kullanılabilirlik ve güvenilirlik sağlar. Veri varlıklarınız, herhangi bir beklenmeyen arızaya karşı koruma sağlamak üzere yedekli kopyaların oluşturulmasıyla sağlam bir şekilde depolanır. Kuruluşlar, kendi çözümlerinde, mevcut veri platformu önemli bir parçası Data Lake depolama Gen1 kullanabilirsiniz.

Data Lake depolama Gen1 ayrıca depolanan veriler için kurumsal düzeyde güvenlik sağlar. Daha fazla bilgi için [Azure Data Lake depolama Gen1 verileri güvenli hale getirme](#DataLakeStoreSecurity).

### <a name="all-data"></a>Tüm Veriler

Data Lake depolama Gen1 depolayabileceğiniz tüm verileri yerel biçiminde, olduğu gibi herhangi bir önceki dönüştürme gerektirmeden. Data Lake depolama Gen1, veriler yüklenmeden önce tanımlanacak bir şema kadar verilerin yorumlanmasını ve şema analiz zamanında tanımlamak için ayrı analitik çerçeveye bırakır gerektirmez. İsteğe bağlı boyut ve biçimlerdeki dosyaların depolamak, yapılandırılmış, yarı yapılandırılmış ve yapılandırılmamış verileri işlemek Data Lake depolama Gen1 için mümkün kılar.

Veriler için Data Lake depolama Gen1 kapsayıcılar olan temelde klasörleri ve dosyaları. SDK'ları, Azure portalı ve Azure PowerShell'i kullanarak depolanan veriler üzerinde çalışır. Verilerinizi bu arabirimleri kullanarak depoya yerleştirdiğiniz ve ilgili kapsayıcıları kullandığınız sürece, istediğiniz türde veriyi depolayabilirsiniz. Data Lake depolama Gen1, depoladığı verilerin türüne göre veri herhangi bir özel işlem gerçekleştirmez.

## <a name="DataLakeStoreSecurity"></a>Data Lake depolama Gen1 veri güvenliğini sağlama
Data Lake depolama Gen1 kullanan Azure Active Directory kimlik doğrulaması ve erişim için verilerinizi listeleri (ACL) erişimi yönetmek üzere denetler.

| Özellik | Açıklama |
| --- | --- |
| Kimlik Doğrulaması |Data Lake depolama Gen1, Data Lake depolama Gen1 içinde depolanan tüm veriler için kimlik ve erişim yönetimi için Azure Active Directory (AAD) ile tümleşir. Tümleştirme sonucunda, Data Lake depolama Gen1 çok faktörlü kimlik doğrulaması, koşullu erişim, rol tabanlı erişim denetimi, uygulama kullanımını izleme, güvenlik izleme ve uyarı verme, vb. dahil olmak üzere tüm AAD özelliklerinden faydalanır. Data Lake depolama Gen1, REST arabirimi içinde kimlik doğrulaması için OAuth 2.0 protokolünü destekler. Bkz: [Data Lake depolama Gen1 kimlik doğrulaması](../data-lake-store/data-lakes-store-authentication-using-azure-active-directory.md)|
| Erişim denetimi |Data Lake depolama Gen1 WebHDFS protokolünün kullanıma sunulan POSIX stili izinleri destekleyerek erişim denetimi sağlar. ACL’ler kök klasörde, alt klasörlerde ve dosyalarda tek tek etkinleştirilebilir. ACL'ler Data Lake depolama Gen1 bağlamında nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Data Lake depolama Gen1'deki erişim denetimi](../data-lake-store/data-lake-store-access-control.md). |
| Şifreleme |Data Lake depolama Gen1 ayrıca hesapta depolanan veriler için şifreleme sağlar. Bir Data Lake depolama Gen1 hesabı oluşturulurken şifreleme ayarlarını belirtirsiniz. Verilerinizin şifrelenmesini tercih edebilir ya da şifrelemeyi kabul etmeyebilirsiniz. Daha fazla bilgi için [şifreleme Data Lake depolama Gen1](../data-lake-store/data-lake-store-encryption.md). Şifreleme tabanlı yapılandırma sağlama konusunda yönergeler için bkz. [Azure Data Lake depolama Gen1 ile çalışmaya başlama Azure portalını kullanarak](../data-lake-store/data-lake-store-get-started-portal.md). |

Data Lake depolama Gen1 veri güvenliğini sağlama hakkında daha fazla öğrenmek ister misiniz? Aşağıdaki bağlantıları izleyin.

* Data Lake depolama Gen1 güvenli verilerde konusunda yönergeler için bkz: [Azure Data Lake depolama Gen1 verileri güvenli hale getirme](../data-lake-store/data-lake-store-secure-data.md).

## <a name="applications-compatible-with-data-lake-storage-gen1"></a>Data Lake depolama Gen1 ile uyumlu uygulamalar
Data Lake depolama Gen1, Hadoop ekosistemindeki çoğu açık kaynak bileşenler ile uyumludur. Ayrıca diğer Azure hizmetleriyle sorunsuz şekilde tümleştirilir.  Data Lake depolama Gen1 hem açık kaynak bileşenlerini ve bunun yanı sıra diğer Azure Hizmetleri ile kullanma hakkında hakkında daha fazla bilgi için aşağıdaki bağlantıları izleyin.

* Bkz: [uygulamaların ve hizmetlerin Azure Data Lake depolama Gen1 ile uyumlu](../data-lake-store/data-lake-store-compatible-oss-other-applications.md) açık kaynaklı uygulamaları Data Lake depolama Gen1 ile birlikte çalışabilen bir listesi.
* Bkz: [diğer Azure hizmetleriyle tümleştirme](../data-lake-store/data-lake-store-integrate-with-other-services.md) nasıl Data Lake depolama Gen1 diğer Azure hizmetleriyle geniş bir senaryoları etkinleştirmek için kullanılabileceğini anlamak için.
* Bkz: [Data Lake depolama Gen1 kullanma senaryoları](../data-lake-store/data-lake-store-data-scenarios.md) veri alma, veri işleme, veri indirme ve veri görselleştirme gibi senaryolarda Data Lake depolama Gen1 kullanmayı öğrenin.

## <a name="what-is-data-lake-storage-gen1-file-system-adl"></a>Data Lake depolama Gen1 dosya sistemi nedir (adl: / /)?
Data Lake depolama Gen1 erişilebilir yeni dosya olan AzureDataLakeFilesystem (adl: / /), Hadoop ortamlarında (HDInsight kümesiyle kullanılabilir). adl:// kullanan uygulamalar ve hizmetler, geçerli olarak WebHDFS için kullanılabilir olmayan ek performans iyileştirmelerinden faydalanabilir. Sonuç olarak, Data Lake depolama Gen1, ya da önerilen seçeneğiyle adl kullanmanın en iyi performans esnekliği sağlar: / / veya doğrudan WebHDFS API'yi kullanmaya devam ederek var olan kod güncelleştirin. Azure HDInsight, Data Lake depolama Gen1 üzerinde en iyi performansı sağlamak üzere AzureDataLakeFilesystem tam olarak yararlanır.

Data Lake depolama Gen1 kullanarak verilerinize erişebilirsiniz `adl://<data_lake_storage_gen1_name>.azuredatalakestore.net`. Data Lake depolama Gen1 verilere erişim hakkında daha fazla bilgi için bkz: [depolanan verilerin özelliklerini görüntüleme](../data-lake-store/data-lake-store-get-started-portal.md#properties)



## <a name="next-steps"></a>Sonraki adımlar

* [Azure Data Lake depolama Gen2'ye Giriş](../storage/blobs/data-lake-storage-introduction.md).
* [Azure Depolama’ya giriş](../storage/common/storage-introduction.md)