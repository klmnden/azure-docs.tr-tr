---
title: Azure HDInsight - depolama en iyi uygulamaları şirket içi Apache Hadoop kümelerini geçirme
description: Azure HDInsight için geçirme şirket içi Hadoop kümeleri için en iyi depolama öğrenin.
author: hrasheed-msft
ms.reviewer: ashishth
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: hrasheed
ms.openlocfilehash: c62a5384edf66fd9309bc7afcb50ada48e3fca7d
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64691513"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---storage-best-practices"></a>Azure HDInsight - depolama en iyi uygulamaları şirket içi Apache Hadoop kümelerini geçirme

Bu makalede, Azure HDInsight sistemlerinde veri depolamaya yönelik öneriler sunar. Geçirme şirket içi Apache Hadoop sistemler ile Azure HDInsight için yardımcı olması için en iyi yöntemler sağlayan bir dizi gereksinimlerimizim bir parçasıdır.

## <a name="choose-right-storage-system-for-hdinsight-clusters"></a>HDInsight kümeleri için doğru depolama sistemi seçin

Şirket içi Apache Hadoop dosya sistemi (HDFS) dizin yapısı Azure depolama veya Azure Data Lake Storage yeniden oluşturulabilir. Kullanıcı verilerini kaybetmeden hesaplama için kullanılan HDInsight kümelerini ardından güvenli bir şekilde silebilirsiniz. Her iki hizmet de bir HDInsight kümesi için varsayılan dosya sistemi ve bir ek dosya sistemi kullanılabilir. HDInsight kümesi ve depolama hesabının aynı bölgede barındırılması gerekir.

### <a name="azure-storage"></a>Azure Storage

HDInsight kümeleri blob kapsayıcısını varsayılan dosya sistemi veya bir ek dosya sistemi olarak Azure Storage'da kullanabilirsiniz. Standart katmanı depolama hesabı, HDInsight kümeleri ile kullanım için desteklenir. Premier katmanı desteklenmiyor. Varsayılan Blob kapsayıcısı iş geçmişi ve iş günlükleri gibi kümeye özel bilgileri depolar. Bir blob kapsayıcısının birden fazla küme için varsayılan dosya sistemi olarak paylaşılması desteklenmez.

Oluşturma işlemi ve ilgili anahtarlarla tanımlanan depolama hesapları depolanan `%HADOOP_HOME%/conf/core-site.xml` küme düğümlerinde. Ambari UI, HDFS yapılandırmasında "Özel çekirdek site" bölümünde de erişilebilir. Depolama hesabı anahtarı varsayılan olarak şifrelenir ve özel şifre çözme betik Hadoop Daemon'ları iletilmeden önce anahtarların şifresini çözmek için kullanılır. İşleri dahil olmak üzere Hive, MapReduce, Hadoop akış ve Pig depolama hesapları ve bunların meta veri açıklamasını taşır.

Azure depolama, coğrafi olarak çoğaltılmış olabilir. Coğrafi çoğaltma coğrafi kurtarma ve veri yedekliği sağlamakla rağmen coğrafi olarak çoğaltılmış konumu için bir yük devretme performansını ciddi bir şekilde etkiler ve ek ücrete neden. Coğrafi çoğaltmayı akıllıca ve yalnızca seçmeniz önerilir verilerin değeri ek maliyetlere değer durumdaysa.

Aşağıdaki biçimlerden birini Azure Depolama'da depolanan verilere erişmek için kullanılabilir:

|Veri erişim biçimi |Açıklama |
|---|---|
|`wasb:///`|Şifrelenmemiş iletişimin kullanarak erişim varsayılan depolama alanı.|
|`wasbs:///`|Şifreli iletişim kullanarak erişim varsayılan depolama alanı.|
|`wasb://<container-name>@<account-name>.blob.core.windows.net/`|Varsayılan olmayan depolama hesabıyla iletişimde kullanılacak. |


[Azure depolama ölçeklenebilirlik ve performans hedefleri](../../storage/common/storage-scalability-targets.md) Azure depolama hesaplarında geçerli olan sınırlar listelenmektedir. Uygulamanızın gereksinimlerine tek bir depolama hesabı ölçeklenebilirlik hedefleri aşarsa, uygulamanın birden fazla depolama hesabı kullanma ve ardından bu depolama hesabı arasında veri nesneleri bölüm oluşturulabilir.

[Azure depolama analizi](../../storage/storage-analytics.md) tüm depolama hizmetleri ve Azure portalı grafikleri görünür için yapılandırılmış toplama ölçümleri olabilir ölçümleri sağlar. Depolama kaynak ölçümleri için eşikler üst sınırına ulaştınız bilgilendirmek üzere uyarılar oluşturulabilir.

Azure depolama sunar [blob nesneler için geçici silme](../../storage/blobs/storage-blob-soft-delete.md) , yanlışlıkla değişiklik veya bir uygulama veya başka bir depolama hesabı kullanıcı tarafından silinmiş verileri kurtarmak için.

Oluşturabileceğiniz [blob anlık görüntüsü](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob). Bir noktada zamanında alınmış bir blob salt okunur bir sürümünü bir anlık görüntüdür ve blob yedeklemek için bir yol sağlar. Anlık görüntü oluşturulduktan sonra okunabildiğini, kopyalanan, veya silinmiş, ancak değiştirilmedi.

> [!Note]
> "Wasbs" sertifika yoksa, şirket içi Hadoop dağıtımları daha eski sürümleri için bunların Java güven deposu için içeri aktarılması gerekir.

Java güven deposuna sertifikaları içeri aktarmak için aşağıdaki yöntemleri kullanılabilir:

Azure Blob ssl sertifikası için dosya indirme

```bash
echo -n | openssl s_client -connect <storage-account>.blob.core.windows.net:443 | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > Azure_Storage.cer
```

Tüm düğümlerde Java güven deposu için yukarıdaki dosyasını içeri aktar

```bash
keytool -import -trustcacerts -keystore /path/to/jre/lib/security/cacerts -storepass changeit -noprompt -alias blobtrust -file Azure_Storage.cer
```

Eklenmiş bir sertifika güven deposunda olduğundan emin olun

```bash
keytool -list -v -keystore /path/to/jre/lib/security/cacerts
```

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure HDInsight kümeleri ile Azure depolama kullanma](../hdinsight-hadoop-use-blob-storage.md)
- [Azure Depolama Ölçeklenebilirlik ve Performans Hedefleri](../../storage/common/storage-scalability-targets.md)
- [Microsoft Azure Depolama Performansı ve Ölçeklenebilirlik Onay Listesi](../../storage/common/storage-performance-checklist.md)
- [Microsoft Azure Depolama izleme, tanılama ve sorun giderme](../../storage/common/storage-monitoring-diagnosing-troubleshooting.md)
- [Azure portalında depolama hesabı izleme](../../storage/common/storage-monitor-storage-account.md)

### <a name="azure-data-lake-storage-gen1"></a>Azure Data Lake Storage Gen1

Azure Data Lake depolama, HDFS ve POSIX stili erişim denetimi modeli kullanır. Bu, AAD ile birinci sınıf tümleştirme için ince ayrıntılı erişim denetimi sağlar. Depolayabilir veri veya yüksek düzeyde paralel analizler çalıştırma yeteneğini boyutu için sınır yoktur.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure portalını kullanarak Data Lake Store ile HDInsight kümeleri oluşturma](../../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)
- [Data Lake Storage Azure HDInsight kümeleri ile kullanma](../hdinsight-hadoop-use-data-lake-store.md)

### <a name="azure-data-lake-storage-gen2"></a>Azure Data Lake Storage Gen2

Azure Data Lake depolama Gen2, en son depolama teklifidir. Azure Data Lake Storage first neslini gelen çekirdek özellikler, Azure Blob depolama alanına doğrudan tümleştirilmiş bir Hadoop uyumlu bir dosya sistemi uç noktası ile birleştirir. Bu geliştirme nesne depolama ölçek ve maliyet avantajlarını genellikle yalnızca şirket içi dosya sistemleri ile ilişkili performans ve güvenilirliği ile birleştirir.

ADLS Gen 2 üzerine kurulmuştur [Azure Blob Depolama](../../storage/blobs/storage-blobs-introduction.md) ve her iki dosya sistemi ve nesne depolama paradigmalarını kullanarak verilerle arabirim sağlar. Öğesinden özellikleri [Azure Data Lake depolama Gen1](../../data-lake-store/index.md), dosya sistemi sematiğini gibi dosya düzeyinde güvenlik ve ölçek, düşük maliyetli, katmanlı depolama, yüksek kullanılabilirlik/olağanüstü durum kurtarma özellikleri ve büyük bir SDK/Araçları ile birleştirilir ekosisteminden [Azure Blob Depolama](../../storage/blobs/storage-blobs-introduction.md). Analiz için iş yükleri için iyileştirilmiş bir dosya sistemi arabirimi avantajları ekleme sırasında nesne depolama, tüm kalitelerini Data Lake depolama Gen2 ' kalır.

Temel Data Lake depolama Gen2 eklenmesini özelliğidir bir [hiyerarşik ad alanı](../../storage/data-lake-storage/namespace.md) Blob Depolama hizmetine dizinlerin yüksek performanslı veri erişimi için bir hiyerarşiye nesneleri/dosyalar düzenler. Hiyerarşik yapısı, yeniden adlandırma veya dizin atomik meta veri işlemleri bir dizini silme yerine numaralandırma ve dizin adı öneki paylaşan tüm nesneleri işleme gibi işlemler sağlar.

Geçmişte, bulut tabanlı analiz performansı, yönetim ve güvenlik alanlarında tehlikeye gerekiyordu. Azure Data Lake Storage (ADLS) 2. nesil anahtar özelliklerini aşağıdaki gibidir:

- **Hadoop uyumlu erişim**: Azure Data Lake depolama Gen2'ye yönetmenizi ve sahip olduğu gibi veri erişim sağlayan bir [Hadoop dağıtılmış dosya sistemi (HDFS)](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html). Yeni [ABFS sürücü](../../storage/data-lake-storage/abfs-driver.md) dahil tüm Apache Hadoop ortamlarında kullanılabilir [Azure HDInsight](../index.yml). Bu sürücü, Data Lake depolama 2. nesil'deki depolanan verilere erişmek sağlar.

- **POSIX izinleri kümesi**: Data Lake Gen2 için güvenlik modeli, ACL ve POSIX izinleri için Data Lake depolama Gen2'ye özel bazı ek ayrıntı birlikte tam olarak destekler. Ayarları, Yönetim Araçları veya Hive ve Spark gibi çerçeveleri aracılığıyla yapılandırılabilir.

- **Uygun maliyetli**: Data Lake depolama Gen2, düşük maliyetli depolama kapasitesi ve işlem sunar. Kendi tam yaşam döngüsü boyunca veri geçişi gibi yerleşik özellikler aracılığıyla maliyetleri en aza indirmek için faturalandırma ücretleri değiştirme [Azure Blob Depolama yaşam döngüsü](../../storage/common/storage-lifecycle-management-concepts.md).

- **Blob Depolama Araçlar, çerçeveler ve uygulamalar ile çalışır**: Data Lake depolama Gen2'ye bir çeşit Araçlar, çerçeveler ve Blob Depolama için bugün mevcut uygulamaları ile çalışmaya devam eder.

- **En iyi duruma getirilmiş sürücü**: Azure Blob dosya sistemi sürücü (ABFS) [özellikle en iyi duruma getirilmiş](../../storage/data-lake-storage/abfs-driver.md) büyük veri analizi için. Karşılık gelen REST API'leri dfs uç noktası aracılığıyla çıkmış dfs.core.windows.net.

Aşağıdaki biçimlerden birini ADLS 2. nesil'deki depolanan verilere erişmek için kullanılabilir:
- `abfs:///`: ' % S'varsayılan Data Lake depolama kümesi için erişim.
- `abfs[s]://file_system@account_name.dfs.core.windows.net`: Varsayılan olmayan Data Lake depolama ile iletişim kurarken kullanılan.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure Data Lake depolama Gen2'ye Giriş](../../storage/data-lake-storage/introduction.md)
- [Azure Blob dosya sistemi sürücü (ABFS.md)](../../storage/data-lake-storage/abfs-driver.md)
- [Azure Data Lake depolama Gen2 Azure HDInsight kümeleri ile kullanma](../hdinsight-hadoop-use-data-lake-storage-gen2.md)

## <a name="secure-azure-storage-keys-within-on-premises-hadoop-cluster-configuration"></a>Şirket içi Hadoop küme yapılandırması içinde güvenli Azure depolama anahtarları

Hadoop yapılandırma dosyaları için eklenen Azure depolama anahtarlarını, şirket içi HDFS ve Azure Blob Depolama arasında bağlantı kurun. Bu anahtarları ile Hadoop kimlik bilgisi sağlayıcısı çerçevesi şifreleyerek korunabilir. Şifrelenmiş sonra edilebilmeleri depolanan ve güvenli bir şekilde erişilebilir.

**Kimlik bilgilerini sağlamak için:**

```bash
hadoop credential create fs.azure.account.key.account.blob.core.windows.net -value <storage key> -provider jceks://hdfs@headnode.xx.internal.cloudapp.net/path/to/jceks/file
```

**Core-site.xml veya özel çekirdek-site Ambari yapılandırmada yukarıdaki sağlayıcı yolu eklemek için:**

```xml
<property>
    <name>hadoop.security.credential.provider.path</name>
        <value>
        jceks://hdfs@headnode.xx.internal.cloudapp.net/path/to/jceks
        </value>
    <description>Path to interrogate for protected credentials.</description>
</property>
```

> [!Note]
> Sağlayıcı yolu özelliği, core-site.xml kümesi düzeyinde anahtarında gibi depolamak yerine distcp komut satırına da eklenebilir:

```bash
hadoop distcp -D hadoop.security.credential.provider.path=jceks://hdfs@headnode.xx.internal.cloudapp.net/path/to/jceks /user/user1/ wasb:<//yourcontainer@youraccount.blob.core.windows.net/>user1
```

## <a name="restrict-azure-storage-data-access-using-sas"></a>SAS kullanarak Azure depolama veri erişimi kısıtlama

Varsayılan olarak HDInsight kümesi ile ilişkili Azure depolama hesaplarında veri tam erişimi vardır. Paylaşılan erişim imzaları (SAS) blob kapsayıcısında kullanılabilir veri erişiminin kısıtlama, kullanıcılara gibi verilere salt okunur erişim sağlar.

### <a name="using-the-sas-token-created-with-python"></a>Python ile oluşturulan SAS belirteci kullanma

1. Açık [SASToken.py](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature/blob/master/Python/SASToken.py) dosyasını açıp aşağıdaki değerleri değiştirin:

    |Belirteç özelliği|Açıklama|
    |---|---|
    |policy_name|Saklı ilkesi oluşturmak için kullanılacak ad.|
    |storage_account_name|Depolama hesabınızın adı.|
    |storage_account_key|Depolama hesabı anahtarı.|
    |storage_container_name|Erişimi kısıtlamak istediğiniz depolama hesabındaki kapsayıcı.|
    |example_file_path|Kapsayıcı için karşıya bir dosya yolu.|

2. SASToken.py dosyası ile birlikte gelen `ContainerPermissions.READ + ContainerPermissions.LIST` izinleri ve kullanım durumunu temel alınarak ayarlanabilir.

3. Şu şekilde betiği yürütün: `python SASToken.py`

4. Betik tamamlandığında aşağıdaki metne benzer bir SAS belirteci görüntüler: `sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14`

5. Kapsayıcı paylaşılan erişim imzası ile sınırlamak için Ambari HDFS yapılandırmaları Gelişmiş özel çekirdek site ekleme özelliği altında bir küme için çekirdek site yapılandırması özel bir giriş ekleyin.

6. İçin aşağıdaki değerleri kullanın **anahtarı** ve **değer** alanlar:

    **Anahtar**: `fs.azure.sas.YOURCONTAINER.YOURACCOUNT.blob.core.windows.net` **Değer**: SAS ANAHTARINI Python uygulama FROM adım 4 yukarıdaki döndürdü.

7. Tıklayın **Ekle** bu anahtar ve değer Kaydet düğmesine ve ardından tıklayın **Kaydet** yapılandırma değişikliklerini kaydetmek için düğme. İstendiğinde, değişikliği ("SAS depolama erişim örneğin ekleme") bir açıklama ekleyin ve ardından **Kaydet**.

8. İçinde Ambari web kullanıcı Arabirimi, HDFS, sol taraftaki listeden seçin ve ardından **yeniden tüm etkilenen** hizmet Eylemler, sağdaki listeden açılır. Sorulduğunda, **tüm yeniden onaylayın**.

9. MapReduce2 ve YARN için bu işlemi yineleyin.

SAS belirteçlerini azure'da kullanımına ilişkin unutmayın gereken üç önemli nokta vardır:

1. SAS belirteçleri "+ listesi okuma" izinleri ile oluşturulduğunda bu SAS belirteci ile Blob kapsayıcısı erişen kullanıcılar "yazma ve silme" mümkün olmayacaktır veri. Bu SAS belirteci ile Blob kapsayıcısına erişmek ve yazma deneyin ya da silme işlemi, kullanıcılar gibi bir ileti alırsınız `"This request is not authorized to perform this operation"`.

2. SAS belirteçleri oluşturulduğunda ile `READ + LIST + WRITE` izinleri (kısıtlamak için `DELETE` yalnızca), gibi komutlar `hadoop fs -put` ilk yazma bir `\_COPYING\_` dosya ve dosyayı yeniden adlandırmak deneyin. Bu HDFS işlem eşleyen bir `copy+delete` WASB için. Bu yana `DELETE` izni sağlanmamış, "put" başarısız olur. `\_COPYING\_` İşlemi bazı eşzamanlılık denetimi sağlamaya yönelik Hadoop özelliğidir. Şu anda yalnızca "Sil" işlemi, "Yazma" işlemleri de etkilemeden kısıtlamak için hiçbir yolu yoktur.

3. Ne yazık ki, SAS belirteçleri ile hadoop kimlik bilgisi sağlayıcısı ve şifre çözme anahtarı sağlayıcısı (ShellDecryptionKeyProvider) şu anda çalışmaz ve bu nedenle, şu anda görünürlük korunamaz.

Daha fazla bilgi için [kullanımı Azure depolama paylaşılan erişim HDInsight verilere erişimi kısıtlamak için imzaları](../hdinsight-storage-sharedaccesssignature-permissions.md).

## <a name="use-data-encryption-and-replication"></a>Kullanım verileri şifreleme ve çoğaltma

Azure Depolama'ya yazılan tüm veriler, kullanılarak otomatik olarak şifrelenir [depolama hizmeti şifrelemesi (SSE)](../../storage/common/storage-service-encryption.md). Azure depolama hesabındaki verilerin her zaman yüksek kullanılabilirlik için çoğaltılır. Bir depolama hesabı oluşturduğunuzda şu çoğaltma seçeneklerinden birini seçebilirsiniz:

- [Yerel olarak yedekli depolama (LRS)](../../storage/common/storage-redundancy-lrs.md)
- [Alanlar arası yedekli depolama (ZRS)](../../storage/common/storage-redundancy-zrs.md)
- [Coğrafi olarak yedekli depolama (GRS)](../../storage/common/storage-redundancy-grs.md)
- [Okuma erişimli coğrafi olarak yedekli depolama (RA-GRS)](../../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage)

Yerel olarak yedekli depolama (LRS) Azure Data Lake depolama sağlar, ancak ayrıca kritik verileri başka bir bölgede başka bir Data Lake Storage hesabına olağanüstü durum kurtarma planı gereksinimlerine uygun bir sıklıkta ile kopyalamanız. Çeşitli veriler dahil olmak üzere kopyalamak için yöntemler vardır [ADLCopy](../../data-lake-store/data-lake-store-copy-data-azure-storage-blob.md), DistCp, [Azure PowerShell](../../data-lake-store/data-lake-store-get-started-powershell.md), veya [Azure Data Factory](../../data-factory/connector-azure-data-lake-store.md). Ayrıca, Data Lake Storage hesabını yanlışlıkla silinmesini engellemek erişim ilkeleri zorunlu tutmanıza olanak önerilir.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure depolama çoğaltma](../../storage/common/storage-redundancy.md)
- [Azure Data Lake Storage (ADLS) için olağanüstü durum Kılavuzu](../../data-lake-store/data-lake-store-disaster-recovery-guidance.md)

## <a name="attach-additional-azure-storage-accounts-to-cluster"></a>Küme için ek Azure depolama hesapları ekleme

HDInsight oluşturma işlemi sırasında bir Azure depolama hesabına veya Azure Data Lake depolama hesabı varsayılan dosya sistemi olarak seçilir. Bu varsayılan depolama hesabının yanı sıra, ek depolama hesapları aynı Azure aboneliğinden veya farklı Azure aboneliklerinden veya Küme oluşturulduktan sonra küme oluşturma işlemi sırasında eklenebilir.

Ek depolama hesabı aşağıdaki yollardan birinde eklenebilir:
- Ambari HDFS yapılandırma özel Gelişmiş çekirdek-site depolama hesap adı ekleyin ve anahtar hizmetleri yeniden başlatma
- Kullanarak [betik eylemi](../hdinsight-hadoop-add-storage.md) depolama hesabı adını ve anahtarını geçirerek

> [!Note]
> Geçerli kullanım örnekleri, Azure depolama sınırları yapılan bir isteği aracılığıyla artırılabilir [Azure Destek](https://azure.microsoft.com/support/faq/).

Daha fazla bilgi için aşağıdaki makalelere bakın:
- [HDInsight için ek depolama hesapları ekleme](../hdinsight-hadoop-add-storage.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu serideki sonraki makaleyi okuyun:

- [Şirket içi-Azure HDInsight Hadoop geçiş için veri geçişi en iyi uygulamalar](apache-hadoop-on-premises-migration-best-practices-data-migration.md)
