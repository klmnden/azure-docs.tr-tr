---
title: Büyük veri analiz çözümlerinizi Azure Data Lake depolama 2. nesil için Azure Data Lake depolama Gen1 ' yükseltme
description: Azure Data Lake depolama Gen2 kullanmak için çözümünüzün yükseltme
services: storage
author: normesta
ms.topic: conceptual
ms.author: normesta
ms.date: 02/07/2019
ms.service: storage
ms.subservice: data-lake-storage-gen2
ms.openlocfilehash: 84e3aff9c1c8cb3e7fe399c861c2c7d58c278fed
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62107882"
---
# <a name="upgrade-your-big-data-analytics-solutions-from-azure-data-lake-storage-gen1-to-azure-data-lake-storage-gen2"></a>Büyük veri analiz çözümlerinizi Azure Data Lake depolama 2. nesil için Azure Data Lake depolama Gen1 ' yükseltme

Büyük veri analiz çözümlerinizi Azure Data Lake depolama Gen1 kullanıyorsanız, bu kılavuzda, Azure Data Lake depolama Gen2 kullanmak için bu çözümleri yükseltmenize yardımcı olur. Bu belge, Data Lake depolama Gen1 üzerinde çözümünüzü olan bağımlılıkları değerlendirmek için kullanabilirsiniz. Bu kılavuzda ayrıca nasıl planlayıp yükseltmek gösterir.

Aşağıdaki görevleri size yardımcı olacağız:

:heavy_check_mark: Yükseltme Hazırlık düzeyinizi değerlendirin

:heavy_check_mark: Yükseltme planlama

:heavy_check_mark: Yükseltme gerçekleştirme

## <a name="assess-your-upgrade-readiness"></a>Yükseltme Hazırlık düzeyinizi değerlendirin

Amacımız, Data Lake depolama 2. nesil'deki Data Lake depolama Gen1 içinde mevcut olan tüm özellikleri sunulur sağlamaktır. Nasıl Bu SDK, CLI vb. örn sunulan Data Lake depolama Gen1 ve Data Lake depolama 2. nesil arasında farklılık gösterebilir. Data Lake depolama Gen1 ile çalışma, uygulamalar ve hizmetler ile Data Lake depolama Gen2'ye benzer şekilde çalışabilmek için gerekir. Son olarak, bazı özelliklerini hemen Data Lake depolama 2. nesil'deki kullanılabilir olmaz. Kullanılabilir oldukça, biz bu belgede duyurulacaktır.

Bu sonraki bölümlerde ve Data Lake depolama Gen2'ye, bunu yapmak için en mantıklı, yükseltmek en iyi şekilde nasıl karar vermenize yardımcı olur.

### <a name="data-lake-storage-gen1-solution-components"></a>Data Lake depolama Gen1 çözüm bileşenleri

Büyük olasılıkla, Data Lake depolama Gen1 analiz çözümleri veya işlem hatları kullandığınızda, uçtan uca genel işlevselliği elde etmek için kullanan birçok ek teknolojiler vardır. Bu [makale](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-data-scenarios) başlayan kümeniz, işleme ve verileri tüketme dahil çeşitli veri akışı bileşenlerinin açıklar.

Ayrıca, sağlama, yönetme ve bu bileşenleri izlemek için çapraz kesme bileşenleri vardır. Bileşenlerin her biriyle çalışmaya ile Data Lake depolama Gen1 kendilerine en uygun bir arabirim kullanarak. Çözümünüz için Data Lake depolama Gen2'ye yükseltme planlıyorsanız, kullanılan arabirimleri dikkat etmeniz gerekir. Her arabirim farklı gereksinimlere sahip olduğundan hem yönetim arabirimleri hem de veri arabirimleri yükseltmeniz gerekir.

![Data Lake depolama çözüm bileşenleri](./media/data-lake-storage-upgrade/solution-components.png)

**Şekil 1** yukarıdaki gösterir işlevselliği bileşenleri çoğu analiz çözümlerini görür.

**Şekil 2** belirli teknolojileri kullanılarak bu bileşenlerin nasıl uygulanacak bir örnek gösterilmektedir.

Depolama işlevindeki **Şekil 1** Data Lake depolama Gen1 tarafından sağlanan (**Şekil 2**). Çeşitli veri akışı bileşenleri REST API'leri veya Java SDK'sını kullanarak Data Lake depolama Gen1 ile nasıl etkileşime unutmayın. Ayrıca, çapraz kesme işlevleri bileşenleri Data Lake depolama Gen1 ile nasıl etkileşimde bulunduğunu unutmayın. Azure İzleyici günlüklerine kullanan İzleme bileşeni Data Lake depolama Gen1 gelen işletimsel verileri kullanan Azure kaynak şablonlarını sağlama bileşeni kullanır.

Bir çözüm kullanarak Data Lake depolama Gen1 Data Lake depolama Gen2'ye yükseltmek için ihtiyacınız olacak meta verileri ve veri kopyalamak bir veri akışı yeniden bağlayın ve ardından, tüm bileşenlerin Data Lake depolama 2. nesil ile çalışabilmek gerekir.

Aşağıdaki bölümlerde, daha iyi kararlar almanıza yardımcı olacak bilgiler verilmektedir:

:heavy_check_mark: Platform özellikleri

:heavy_check_mark: Programlama arabirimleri

:heavy_check_mark: Azure ekosistemi

:heavy_check_mark: İş ortağı ekosistemi

:heavy_check_mark: Operasyonel bilgi

Her bölümde, "gerekir-haves" yükseltmeniz için belirlemek mümkün olacaktır. Var olan özelliklerini veya yerinde makul geçici çözümler olduğunu garanti garanti sonra devam [yükseltme için planlama](#planning-for-an-upgrade) başlığına.

### <a name="platform-capabilities"></a>Platform özellikleri

Bu bölümde, Data Lake depolama 2. nesil'deki şu anda kullanılabilir hangi Data Lake depolama Gen1 platform özellikleri açıklanmaktadır.

| | Data Lake Storage Gen1 | Data Lake depolama 2. nesil - hedef | Data Lake depolama Gen2 - kullanılabilirlik durumu  |
|-|------------------------|-------------------------------|-----------------------------------------------|
| Verileri düzenleme| Klasörleri ve dosyaları depolanan verileri destekler | Nesnelerin/blobların yanı sıra dosya ve klasörleri - depolanan verileri destekler [bağlantı](https://docs.microsoft.com/azure/storage/data-lake-storage/namespace) | Klasör ve dosya – depolanan verileri destekler *kullanıma sunuldu* <br><br> Nesne/BLOB olarak - depolanan verileri destekler *henüz kullanılamıyor* |
| Ad alanı| Hiyerarşik | Hiyerarşik |  *Kullanıma sunuldu*  |
| API  | HTTPS üzerinden REST API | HTTP/HTTPS üzerinden REST API| *Kullanıma sunuldu* |
| Sunucu tarafı API'si| [WebHDFS ile uyumlu REST API](https://msdn.microsoft.com/library/azure/mt693424.aspx) | Azure Blob hizmeti REST API'si [Data Lake depolama Gen2 REST API](https://docs.microsoft.com/rest/api/storageservices/data-lake-storage-gen2) | Data Lake depolama Gen2 REST API'si – *artık kullanılabilir* <br><br> Azure Blob hizmeti REST API'si – *henüz kullanılamıyor*       |
| Hadoop dosya sistemi istemci | Evet ([Azure Data Lake depolama](https://hadoop.apache.org/docs/current/hadoop-azure-datalake/index.html)) | Evet ([ABFS](https://jira.apache.org/jira/browse/HADOOP-15407))  | *Kullanıma sunuldu*  |  
| Veri işlemleri – yetkilendirme  | POSIX erişim denetimi Azure Active Directory kimliği temel listeleri (ACL'ler) dosya ve klasör düzeyinde  | Dosya ve klasör düzeyinde POSIX erişim denetimi Azure Active Directory kimliği temel listeleri (ACL'ler) [paylaşım anahtarını](https://docs.microsoft.com/rest/api/storageservices/authorize-with-shared-key) için hesap düzeyinde yetkilendirme rol tabanlı erişim denetimi ([RBAC](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac)) erişim kapsayıcılarına | *Kullanıma sunuldu* |
| Veri işlemleri – günlükleri  | Evet | Destek bileti Azure izleme tümleştirmesini kullanarak belirli bir süre için günlükleri için bir kerelik istekleri | Bir kerelik isteklerini kullanarak belirli bir süre için günlükleri için destek bileti – *kullanıma sunuldu*<br><br> Azure izleme tümleştirmesini – *henüz kullanılamıyor* |
| Bekleyen verileri şifreleme | Hizmet tarafından yönetilen anahtarlarla ve Azure anahtar Kasası'nda müşteri tarafından yönetilen anahtarlarla saydam sunucu tarafı | Saydam sunucu tarafı hizmet tarafından yönetilen anahtarlarla ve müşteri anahtarlarıyla Azure anahtar Kasası'nda tarafından yönetilen anahtarlar | Hizmet tarafından yönetilen anahtarlar – *kullanıma sunuldu*<br><br> Müşteri tarafından yönetilen anahtarlar – *kullanıma sunuldu*  |
| Yönetim işlemlerini (örneğin hesap oluştur) | [Rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/role-based-access-control/overview) (RBAC), hesap yönetimi için Azure tarafından sağlanan | [Rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/role-based-access-control/overview) (RBAC), hesap yönetimi için Azure tarafından sağlanan | *Kullanıma sunuldu*|
| Geliştirici SDK'ları | .NET, Java, Python, Node.js  | .NET, Java, Python, Node.js, C++, Ruby, PHP, Go, Android, iOS| *Henüz kullanılamıyor* |
| |Paralel analiz iş yükleri için iyileştirilmiş performans. Yüksek aktarım hızı ve IOPS. | Paralel analiz iş yükleri için iyileştirilmiş performans. Yüksek aktarım hızı ve IOPS. | *Kullanıma sunuldu* |
| Sanal ağ (VNet) desteği  | [Sanal ağ tümleştirmesi](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-network-security)  | [Hizmet uç noktası için Azure depolama kullanma](https://docs.microsoft.com/azure/storage/common/storage-network-security?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) | *Kullanıma sunuldu* |
| Boyutu sınırları | Hesap boyutları, dosya boyutları veya dosya sayısı üst sınırı yoktur | Sınırsız hesap boyutları veya dosya sayısı. Dosya boyutu 5 TB ile sınırlıdır. | *Kullanıma sunuldu*|
| Coğrafi yedeklilik| Yerel olarak yedekli (LRS) | Yerel olarak yedekli (LRS) bölgesel olarak yedekli (ZRS) genel olarak yedekli (GRS) okuma erişimi genel olarak yedekli (RA-GRS) bakın [burada](https://docs.microsoft.com/azure/storage/common/storage-redundancy) daha fazla bilgi için| *Kullanıma sunuldu* |
| Bölgesel kullanılabilirlik | Bkz: [burada](https://azure.microsoft.com/regions/) | Tüm [Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/)                                                                                                                                                                                                                                                                                                                                       | *Kullanıma sunuldu*                                                                                                                           |
| Fiyat                                       | Bkz: [fiyatlandırması](https://azure.microsoft.com/pricing/details/data-lake-store/)                                                                            | Bkz: [fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/data-lake/)                                                                                                                                                                                                                                                                                                                                         |                                                                                                                                           |
| Kullanılabilirlik Hizmet Düzeyi Anlaşması                            | [SLA bakın](https://azure.microsoft.com/support/legal/sla/data-lake-store/v1_0/)                                                                   | [SLA bakın](https://azure.microsoft.com/support/legal/sla/storage/v1_3/)                                                                                                                                                                                                                                                                                                                                                | *Kullanıma sunuldu*                                                                                                                           |
| Veri Yönetimi                             | Dosya süre sonu                                                                                                                                        | Yaşam döngüsü ilkeleri                                                                                                                                                                                                                                                                                                                                                                                                          | *Henüz kullanılamıyor*                                                                                                                       |

### <a name="programming-interfaces"></a>Programlama arabirimleri

Bu tablo API'si için özel uygulamalarınızın kullanılabilir olan ayarlayan açıklar. Şeyler biraz daha anlaşılır hale getirmek için Biz bu API kümeleri 2 türlerini ayrılmış: Yönetim API'leri ve dosya sistemi API'ları.

API yönetim hesapları, dosya sistemi API'leri, dosyalar ve klasörler üzerinde çalışan yardımcı sırasında yönetmenize yardımcı.

|  API kümesi                           |  Data Lake Storage Gen1                                                                                                                                                                                                                                                                                                   | Paylaşılan anahtar kimlik doğrulaması için Data Lake depolama Gen2 - kullanılabilirlik | OAuth kimlik doğrulaması için Data Lake depolama Gen2 - kullanılabilirlik                                                                                                  |
|----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| .NET SDK'sı - Yönetim                  | [Bağlantı](https://docs.microsoft.com/dotnet/api/overview/azure/datalakestore/management?view=azure-dotnet)                                                                                                                                                                                                                 | *Desteklenmiyor*                                                      | *Artık kullanılabilir -* [bağlantı](https://docs.microsoft.com/rest/api/storageservices/operations-on-the-account--blob-service-)                                    |
| .NET SDK'sı – dosya sistemi                  | [Bağlantı](https://docs.microsoft.com/dotnet/api/overview/azure/datalakestore/client?view=azure-dotnet)                                                                                                                                                                                                                     | *Henüz kullanılamıyor*                                                | *Henüz kullanılamıyor*                                                                                                                                             |
| Java SDK'sı - Yönetim                  | [Bağlantı](https://docs.microsoft.com/java/api/overview/azure/datalakestore/management)                                                                                                                                                                                                                                     | *Desteklenmiyor*                                                      | *Artık kullanılabilir –* [bağlantı](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob?view=azure-java-stable)                                     |
| Java SDK'sı – dosya sistemi                  | [Bağlantı](https://docs.microsoft.com/java/api/overview/azure/datalake)                                                                                                                                                                                                                                         | *Henüz kullanılamıyor*                                                | *Henüz kullanılamıyor*                                                                                                                                             |
| Node.js - Yönetim                   | [Bağlantı](https://www.npmjs.com/package/azure-arm-datalake-store)                                                                                                                                                                                                                                                                | Desteklenmiyor                                                      | *Artık kullanılabilir -* [bağlantı](https://azure.github.io/azure-storage-node/)                                                                                            |
| Node.js - dosya sistemi                   | [Bağlantı](https://www.npmjs.com/package/azure-arm-datalake-store)                                                                                                                                                                                                                                                                | *Henüz kullanılamıyor*                                                | *Henüz kullanılamıyor*                                                                                                                                             |
| Python - Yönetim                    | [Bağlantı](https://docs.microsoft.com/python/api/overview/azure/datalakestore/management?view=azure-python)                                                                                                                                                                                                                 | *Desteklenmiyor*                                                      | *Artık kullanılabilir -* [bağlantı](https://docs.microsoft.com/python/api/overview/azure/storage/management?view=azure-python)                                       |
| Python – dosya sistemi                    | [Bağlantı](https://azure-datalake-store.readthedocs.io/en/latest/)                                                                                                                                                                                                                                                                 | *Henüz kullanılamıyor*                                                | *Henüz kullanılamıyor*                                                                                                                                             |
| REST API - Yönetim                  | [Bağlantı](https://docs.microsoft.com/rest/api/datalakestore/accounts)                                                                                                                                                                                                                                                      | *Desteklenmiyor*                                                      | *Artık kullanılabilir-*                                                                                                                                               |
| REST API'si - dosya sistemi                  | [Bağlantı](https://docs.microsoft.com/rest/api/datalakestore/webhdfs-filesystem-apis)                                                                                                                                                                                                                                       | *Kullanıma sunuldu*                                                    | *Artık kullanılabilir -* [bağlantı](https://docs.microsoft.com/rest/api/storageservices/data-lake-storage-gen2)                                                      |
| PowerShell - yönetim ve dosya sistemi | [Bağlantı](https://docs.microsoft.com/powershell/module/az.datalakestore)                                                                                                                                                                                                                        | Yönetim – dosya sistemi - desteklenmeyen *henüz kullanılamıyor*        | Yönetim – *artık kullanılabilir -* [bağlantı](https://docs.microsoft.com/powershell/module/az.storage) <br><br>Dosya sistemi - *henüz kullanılamıyor* |
| CLI – Yönetimi                       | [Bağlantı](https://docs.microsoft.com/cli/azure/dls/account?view=azure-cli-latest)                                                                                                                                                                                                                                          | *Desteklenmiyor*                                                      | *Artık kullanılabilir -* [bağlantı](https://docs.microsoft.com/cli/azure/storage?view=azure-cli-latest)                                                              |
| CLI - dosya sistemi                       | [Bağlantı](https://docs.microsoft.com/cli/azure/dls/fs?view=azure-cli-latest)                                                                                                                                                                                                                                               | *Henüz kullanılamıyor*                                                | *Henüz kullanılamıyor*                                                                                                                                             |
| Azure Resource Manager şablonları - Yönetim             | [1](https://azure.microsoft.com/resources/templates/101-data-lake-store-no-encryption/)  [Template2](https://azure.microsoft.com/resources/templates/101-data-lake-store-encryption-adls/)  [Template3](https://azure.microsoft.com/resources/templates/101-data-lake-store-encryption-key-vault/)  | *Desteklenmiyor*                                                      | *Artık kullanılabilir -* [bağlantı](https://docs.microsoft.com/azure/templates/microsoft.storage/2018-07-01/storageaccounts)                                         |

### <a name="azure-ecosystem"></a>Azure ekosistemi

Data Lake depolama Gen1 kullanırken, Microsoft Hizmetleri ve ürünleriyle çeşitli uçtan uca işlem hatlarınızı kullanabilirsiniz. Bu hizmet ve ürünleri ile Data Lake depolama Gen1 doğrudan veya dolaylı olarak çalışır. Bu tablo, biz Data Lake depolama Gen1 ile çalışacak şekilde değiştirdiğinizi ve hangilerinin şu anda Data Lake depolama Gen2'ile uyumlu olduğunu gösteren hizmetlerin bir listesini gösterir.

| **Alan**             | **Data Lake depolama Gen1 kullanılabilirliği**                                                                                                                                    | **Paylaşılan anahtar kimlik doğrulaması ile Data Lake depolama Gen2 için– kullanılabilirlik**                                                                                                           | **Data Lake depolama Gen2: OAuth için kullanılabilirlik**                                                                                        |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| Çerçeve analizi  | [Apache Hadoop](https://hadoop.apache.org/docs/current/hadoop-azure-datalake/index.html)                                                                                       | *Kullanıma sunuldu*                                                                                                                                                              | *Kullanıma sunuldu*                                                                                                                                 |
|                      | [HDInsight](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal)                                                               | [HDInsight](https://docs.microsoft.com/azure/storage/data-lake-storage/quickstart-create-connect-hdi-cluster) 3.6 - *kullanıma sunuldu* HDInsight 4.0 - *henüz kullanılamıyor*      | HDInsight 3.6 ESP – *artık kullanılabilir* <br><br>  HDInsight 4.0 ESP - *henüz kullanılamıyor*                                                                 |
|                      | Databricks çalışma zamanı 3.1 ve üzeri                                                                                                                                               | [Databricks çalışma zamanı 5.1 ve üzeri](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake-gen2.html#azure-data-lake-storage-gen2) *- kullanıma sunuldu* | [Databricks çalışma zamanı 5.1 ve üzeri](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake-gen2.html#azure-data-lake-storage-gen2) – *artık kullanılabilir*                                                                                              |
|                      | [SQL Veri Ambarı](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-from-azure-data-lake-store)                                            | *Desteklenmiyor*                                                                                                                                                              | *Artık kullanılabilir* [bağlantı](https://docs.microsoft.com/sql/t-sql/statements/create-external-data-source-transact-sql?view=sql-server-2017) |
| Veri tümleştirmesi     | [Data Factory](https://docs.microsoft.com/azure/data-factory/load-azure-data-lake-store)                                                                                | [Sürüm 2](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage) – *kullanıma sunuldu* sürüm 1 – *desteklenmiyor*                               | [Sürüm 2](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage) – *artık kullanılabilir* <br><br> Sürüm 1 – *desteklenmiyor*  |
|                      | [AdlCopy](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob)                                                                 | *Desteklenmiyor*                                                                                                                                                              | *Desteklenmiyor*                                                                                                                                 |
|                      | [SQL Server Integration Services](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-data-lake-store-connection-manager?view=sql-server-2017) | *Henüz kullanılamıyor*                                                                                                                                                          | *Henüz kullanılamıyor*                                                                                                                             |
|                      | [Veri Kataloğu](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-with-data-catalog)                                                                       | *Henüz kullanılamıyor*                                                                                                                                                          | *Henüz kullanılamıyor*                                                                                                                             |
|                      | [Logic Apps](https://docs.microsoft.com/connectors/azuredatalake/)                                                                                                      | *Henüz kullanılamıyor*                                                                                                                                                          | *Henüz kullanılamıyor*                                                                                                                             |
| IoT                  | [Event Hubs – yakalama](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-archive-eventhub-capture)                                                       | *Henüz kullanılamıyor*                                                                                                                                                          | *Henüz kullanılamıyor*                                                                                                                             |
|                      | [Akış Analizi](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-stream-analytics)                                                                   | *Henüz kullanılamıyor*                                                                                                                                                          | *Henüz kullanılamıyor*                                                                                                                             |
| Tüketim          | [Power BI Desktop](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-power-bi)                                                                           | *Henüz kullanılamıyor*                                                                                                                                                          | *Henüz kullanılamıyor*                                                                                                                             |
|                      | [Excel](https://techcommunity.microsoft.com/t5/Excel-Blog/Announcing-the-Azure-Data-Lake-Store-Connector-in-Excel/ba-p/91677)                                                 | *Henüz kullanılamıyor*                                                                                                                                                          | *Henüz kullanılamıyor*                                                                                                                             |
|                      | [Analysis Services](https://blogs.msdn.microsoft.com/analysisservices/2017/09/05/using-azure-analysis-services-on-top-of-azure-data-lake-storage/)                            | *Henüz kullanılamıyor*                                                                                                                                                          | *Henüz kullanılamıyor*                                                                                                                             |
| Üretkenlik         | [Azure portal](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal)                                                                      | *Desteklenmiyor*                                                                                                                                                              | Hesap Yönetimi *– kullanıma sunuldu* <br><br>Veri işlemleri *–**henüz kullanılamıyor*                                                                   |
|                      | [Visual Studio için Data Lake araçları](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-data-lake-tools-install)                                   | *Henüz kullanılamıyor*                                                                                                                                                          | *Henüz kullanılamıyor*                                                                                                                             |
|                      | [Azure Depolama Gezgini](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-in-storage-explorer)                                                          | *Kullanıma sunuldu*                                                                                                                                                              | *Kullanıma sunuldu*                                                                                                                                 |
|                      | [Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=usqlextpublisher.usql-vscode-ext)                                                                     | *Henüz kullanılamıyor*                                                                                                                                                          | *Henüz kullanılamıyor*                                                                                                                             |

### <a name="partner-ecosystem"></a>İş ortağı ekosistemi

Bu tablo, üçüncü taraf hizmetler ve Data Lake depolama Gen1 ile çalışmak için değiştirilmiş olan ürünlerin listesini gösterir. Ayrıca, hangilerinin şu anda Data Lake depolama Gen2'ile uyumlu olduğunu gösterir.

| Alan            | Partner  | Ürün/hizmet  | Data Lake depolama Gen1 kullanılabilirliği                                                                                                                                               | Paylaşılan anahtar kimlik doğrulaması ile Data Lake depolama Gen2 için– kullanılabilirlik                                                   | Data Lake depolama Gen2: Oauth için kullanılabilirlik                                                             |
|---------------------|--------------|----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| Çerçeve analizi | Cloudera     | CDH                  | [Bağlantı](https://www.cloudera.com/documentation/enterprise/5-11-x/topics/admin_adls_config.html)                                                                                            | *Henüz kullanılamıyor*                                                                                                  | [Bağlantı](https://www.cloudera.com/documentation/enterprise/latest/topics/admin_adls2_config.html)                                                                                                  |
|                     | Cloudera     | Altus                | [Bağlantı](https://www.cloudera.com/more/news-and-blogs/press-releases/2017-09-27-cloudera-introduces-altus-data-engineering-microsoft-azure-hybrid-multi-cloud-data-analytic-workflows.html) | *Desteklenmiyor*                                                                                                                  | *Henüz kullanılamıyor*                                                                                                  |
|                     | HortonWorks  | HDP 3.0              | [Bağlantı](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.3/bk_cloud-data-access/content/adls-get-started.html)                                                                       | *Henüz kullanılamıyor*                                                                                                  | *Henüz kullanılamıyor*                                                                                                  |
|                     | Qubole       |                      | [Bağlantı](https://www.qubole.com/blog/big-data-analytics-microsoft-azure-data-lake-store-qubole/)                                                                                            | *Henüz kullanılamıyor*                                                                                                  | *Henüz kullanılamıyor*                                                                                                  |
| ETL                 | StreamSets   |                      | [Bağlantı](https://streamsets.com/blog/ingest-data-azure)                                                                                                                                     | *Henüz kullanılamıyor*                                                                                                  | *Henüz kullanılamıyor*                                                                                                  |
|                     | Informatica  |                      | [Bağlantı](https://kb.informatica.com/proddocs/Product%20Documentation/6/IC_Spring2017_MicrosoftAzureDataLakeStoreV2ConnectorGuide_en.pdf)                                                    | *Henüz kullanılamıyor*                                                                                                  | *Henüz kullanılamıyor*                                                                                                  |
|                     | Attunity     |                      | [Bağlantı](https://www.attunity.com/company/press-releases/microsoft-and-attunity-announce-strategic-partnership-for-data-migration/)                                                         | *Henüz kullanılamıyor*                                                                                                  | *Henüz kullanılamıyor*                                                                                                  |
|                     | Alteryx      |                      | [Bağlantı](https://help.alteryx.com/2018.2/DataSources/AzureDataLake.htm)                                                                                                                     | *Henüz kullanılamıyor*                                                                                                  | *Henüz kullanılamıyor*                                                                                                  |
|                     | ImanisData   |                      | [Bağlantı](https://www.imanisdata.com/expansion-azure-support-azure-data-lake-store-integration/)                                                                                             | *Henüz kullanılamıyor*                                                                                                  | *Henüz kullanılamıyor*                                                                                                  |
|                     | WANdisco     |                      | [Bağlantı](https://azure.microsoft.com/blog/globally-replicated-data-lakes-with-livedata-using-wandisco-on-azure/)                                                                      | [Bağlantı](https://azure.microsoft.com/blog/globally-replicated-data-lakes-with-livedata-using-wandisco-on-azure/) | [Bağlantı](https://azure.microsoft.com/blog/globally-replicated-data-lakes-with-livedata-using-wandisco-on-azure/) |

### <a name="operational-information"></a>Operasyonel bilgi

Data Lake depolama Gen1 belirli bilgileri ve veri işlem hatlarınızı kullanıma hazır hale getirmeye yardımcı olan diğer hizmetlere iter. Bu tablo, Data Lake depolama Gen2'ye karşılık gelen destek kullanılabilirliğini gösterir.

| Veri türü                                                                                       | Data Lake depolama Gen1 kullanılabilirliği                                                                 | Data Lake depolama Gen2 için'kullanılabilirlik                                                                                               |
|--------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| Faturalama verileri - ticaret için gönderilen ölçümleri faturalandırma için takım ve müşteriler için kullanılabilir hale  | *Kullanıma sunuldu*                                                                                             | *Kullanıma sunuldu*                                                                                                                           |
| Etkinlik günlükleri                                                                                          | [Bağlantı](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-diagnostic-logs#audit-logs)   | Bir kerelik isteklerini kullanarak belirli bir süre için günlükleri için destek bileti – *kullanıma sunuldu* tümleştirme - Azure izleme *henüz kullanılamıyor* |
| Tanılama günlükleri                                                                                        | [Bağlantı](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-diagnostic-logs#request-logs) | Bir kerelik isteklerini kullanarak belirli bir süre için günlükleri için destek bileti – *kullanıma sunuldu* tümleştirme - Azure izleme *henüz kullanılamıyor* |
| Ölçümler                                                                                                | *Desteklenmiyor*                                                                                               | *Artık kullanılabilir -* [bağlantı](https://docs.microsoft.com/azure/storage/common/storage-metrics-in-azure-monitor)                          |

## <a name="planning-for-an-upgrade"></a>Yükseltme için planlama

Bu bölümde, gözden geçirdiğinizi varsayar [Yükseltme Hazırlık düzeyinizi değerlendirin](#assess-your-upgrade-readiness) başlığına ve bağımlılıklarınızı tümünün karşılandığından. Data Lake depolama 2. nesil'deki hala kullanılabilir olmayan yetenekleri varsa, yalnızca ilgili geçici çözümler biliyorsanız Lütfen geçin. Aşağıdaki bölümlerde, işlem hatlarınızı yükseltmesi için nasıl planlayabilirsiniz hakkında rehberlik sağlar. Yükseltmeyi gerçekleştirmek açıklanan [yükseltme yapmadan](#performing-the-upgrade) başlığına.

### <a name="upgrade-strategy"></a>Yükseltme stratejisi

Yükseltme en kritik bir parçası stratejisi karar vermektir. Bu karar seçenekleri belirler.

Bu tablo, veritabanları, Hadoop kümelerini geçirmek için kullanılan bazı iyi bilinen stratejileri listelenmektedir. Biz kılavuzumuzu içinde benzer stratejiler benimseyin ve bunları bizim bağlamına uyarlayabilirsiniz.

| Stratejisi                   | Uzmanları                                                                                  | Simgeler                                                           | Ne zaman kullanılır?                                                                                                                                                                                             |
|--------------------------------|-------------------------------------------------------------------------------------------|--------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Lift-and-shift ile taşıma                 | En basit.                                                                                 | Veri, taşıma işler ve hareketli giriş ve çıkış kopyalamak için kapalı kalma süresi gerektirir.                                             | Basit çözüm, birden çok çözümü olduğu hesap aynı Gen1 erişme ve bu nedenle birlikte hızlı denetimli bir biçimde taşıyabilirsiniz.                                                  |
| Kopyalama-bir kez-ve-kopyalama artımlı | Kaynak Sistem hala etkin durumdayken arka planda kopyalama gerçekleştirerek kapalı kalma süresini azaltın. | Giriş ve çıkış taşımak için kapalı kalma süresi gerektirir.                   | Üzerinden kopyalanacak veri miktarı büyükse ve yaşam-and-shift ile taşıma ile ilişkili bir kapalı kalma süresi kabul edilebilir değil. Test geçiş önce hedef sistem üzerinde önemli üretim verileri ile gerekli olabilir. |
| Paralel benimseme              | En az kapalı kalma süresi sağlar zaman kendi kararımıza geçirmek uygulamalar için.           | 2 yönlü eşitleme bu yana en karmaşık iki sistem arasındaki gereklidir. | Data Lake depolama Gen1 üzerinde oluşturulan uygulamalar burada tek seferde tam geçişi olamaz ve olmalıdır karmaşık senaryolar artımlı bir şekilde taşındığını için.                                                      |

Aşağıdaki adımlar hakkında daha fazla ayrıntı her stratejileri için dahil edilir. Yükseltmenin ilgili bileşenleri ile yaptığınız adımları listeler. Bu genel kaynak sistemi, hedef sistemin, giriş kaynakları kaynak sistemi için kaynak sistemi ve kaynak sistemi üzerinde çalışan işler için çıkış hedefler içerir.

Bu adımları belirleyici değildir. Biz her stratejisi hakkında nasıl düşünüyor hakkında framework ayarlanacak yöneliktir. Biz bunları uygulanan gördüğünüz gibi her stratejisi için örnek olay incelemeleri sağlarız.

#### <a name="lift-and-shift"></a>Lift-and-shift ile taşıma

1. Kaynak sistemi – giriş kaynakları, işleri, çıkış hedeflerine duraklatın.
2. Tüm veriler kaynak sistemden hedef sunucuya kopyalayın.
3. Tüm giriş kaynakları hedef sisteme gelin. Hedef sistemden çıkış hedef konuma gelin.
4. Taşıma, değiştirme, hedef sistem için tüm işleri çalıştırın.
5. Kaynak sistemi devre dışı bırakın.

#### <a name="copy-once-and-copy-incremental"></a>Kopyalama-sonra ve artımlı kopyalama

1. Veriler üzerinde kaynak sistemden hedef sunucuya kopyalayın.
2. Artımlı veriler üzerinde kaynak sistemden düzenli aralıklarla hedef sunucuya kopyalayın.
3. Hedef sistemden çıkış hedef konuma gelin.
4. Taşıma, değiştirme, tüm işleri hedef sistem üzerinde çalışacak.
5. Giriş kaynakları artımlı olarak hedef sistem kolaylık göre gelin.
6. Tüm giriş kaynakları hedef sisteme işaret eden bir kez.
    1. Artımlı kopyalama devre dışı bırakın.
    2. Kaynak sistemi devre dışı bırakın.

#### <a name="parallel-adoption"></a>Paralel benimseme

1. Hedef sistemini ayarlayın.
2. Kaynak ve hedef sistemi arasında iki yönlü çoğaltmayı ayarlayın.
3. Giriş kaynakları hedef sisteme kademeli olarak işaretleyin.
4. Taşıma, değiştirme, işleri artımlı olarak hedef sistem için çalıştırın.
5. Çıkış hedeflerine hedef sistemden artımlı olarak işaretleyin.
6. Tüm özgün giriş kaynakları, işlerini ve çıkış hedef ile hedef sistem çalışır duruma geldikten sonra kaynak sistemi devre dışı bırakın.

### <a name="data-upgrade"></a>Veri yükseltme

Yükseltmeyi gerçekleştirmek için kullandığınız genel stratejisi (açıklanan [yükseltme stratejisi](#upgrade-strategy) başlığına), veri yükseltme için kullandığınız araçları belirler. Aşağıda listelenen araçlar hakkında güncel bilgiler alarak ve önerilerdir. 

#### <a name="tools-guidance"></a>Araçları Kılavuzu

| Stratejisi                       | Araçlar                                                                                                             | Uzmanları                                                                                                                             | Dikkat edilmesi gerekenler                                                                                                                                                                                                                                                                                                                |
|------------------------------------|-----------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Lift-and-shift ile taşıma**                 | [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/load-azure-data-lake-storage-gen2-from-gen1) | Yönetilen bir bulut hizmeti                                                                                                                | Yalnızca veriler üzerinde kopyalar. ACL'ler üzerinden şu anda kopyalanamıyor.                                                                                                                                                                                                                                                                      |
|                                    | [Distcp](https://hadoop.apache.org/docs/r1.2.1/distcp.html)                                                           | Bu araç ile iyi bilinen sağlanan Hadoop aracını yani ACL'leri izinleri kopyalanabilir.                                                   | Data Lake depolama Gen1 ve 2. nesil için aynı anda bağlanabilir bir küme gerekir.                                                                                                                                                                                   |
| **Kopyalama-bir kez-ve-kopyalama artımlı** | Azure Data Factory                                                                                                    | Yönetilen bir bulut hizmeti                                                                                                                | Artımlı kopyalama ADF'de desteklemek için verilerin bir zaman serisi şekilde düzenlenmesini gerekir. Artımlı kopyalar arasındaki en kısa aralık [15 dakika](https://docs.microsoft.com/azure/data-factory/how-to-create-tumbling-window-trigger). Daha kısa aralıklar için ADF çalışmaz. ACL'ler üzerinden şu anda kopyalanamıyor. |
| **Paralel benimseme**              | [WANdisco](https://docs.wandisco.com/bigdata/wdfusion/adls/)                                                           | Saf bir Hadoop ortamı kullanarak Azure Data Lake depolama alanına bağlıysa destek tutarlı çoğaltma iki yönlü çoğaltmayı destekler. | Bir Hadoop saf ortamı kullanılmıyorsa, Çoğaltmada bir gecikme olabilir.                                                                                                                                                                                                                                                  |

Data Lake depolama Gen2'ye yükseltme için Data Lake depolama Gen1 Yukarıdaki veri/meta-araçları veri kopyalama işlemini karıştırılmaksızın işleyebileceği Üçüncü taraflardan olduğunu unutmayın (örneğin: [Cloudera](https://blog.cloudera.com/blog/2017/08/use-amazon-s3-with-cloudera-bdr/)). Bunlar, veri taşıma ve bunun yanı sıra iş yükü geçişi gerçekleştiren bir "tek" deneyimi sağlar. Kendi ekosistemi dışında olan araçlar için bir bant dışı yükseltme gerçekleştirmek zorunda kalabilirsiniz.

#### <a name="considerations"></a>Dikkat edilmesi gerekenler

* Geçerli Kılavuzu Gen1 hesap bilgilerinizi göre herhangi bir otomatik Gen2 hesabı işlem içermediğinden, yükseltme, herhangi bir bölümünü başlamadan önce el ile Data Lake depolama Gen2 hesabı oluşturmanız gerekir. Hesapları oluşturma işlemleri için karşılaştırma sağlamak [Gen1](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal) ve [Gen2](https://docs.microsoft.com/azure/storage/data-lake-storage/quickstart-create-account).

* Data Lake depolama Gen2, yalnızca desteklediği en çok 5 TB boyutundaki dosyaları. Data Lake depolama 2. nesil yükseltmek için Data Lake depolama Gen1 dosyalarında, 5 TB boyutunda değerinden küçük olacak şekilde yeniden boyutlandırmak gerekebilir.

* ACL'ler kopyalamaz bir araç kullanmanız veya ACL'ler kopyalamak istemiyorsanız, ACL'leri uygun en üst düzey hedefte el ile ayarlamanız gerekir. Depolama Gezgini'ni kullanarak yapabilirsiniz. Bu ACL'ler böylece kopyalayabilirsiniz, klasör ve dosya bunları devralmasını varsayılan ACL'ler olduğundan emin olun.

* Data Lake depolama Gen1 içinde ACL'leri ayarlayabilirsiniz en üst düzey hesabının kök dizininde ' dir. Data Lake depolama Gen2'içinde ancak ACL'leri ayarlayabilirsiniz yüksek Kök klasörde bir dosya sistemi, tüm hesap düzeydir. Hesap düzeyinde varsayılan ACL'ler ayarlamak istiyorsanız, bu nedenle, Data Lake depolama Gen2 hesabınızdaki tüm dosya sistemleri kullananlar çoğaltmak gerekir.

* Dosya adlandırma kısıtlamaları, iki depolama sistemleri arasında farklılık gösterir. Bu fark özellikle ne zaman ilgili ikinci daha kısıtlamaları kısıtlı olduğundan, Data Lake depolama Gen1 için Data Lake depolama Gen2 ' kopyalama.

### <a name="application-upgrade"></a>Uygulama yükseltme

Data Lake depolama Gen1 veya Data Lake depolama 2. nesil uygulamalar oluşturmak ihtiyacınız olduğunda, önce uygun bir programlama arabirimi seçmeniz gerekir. Bu arabirimdeki bir API'nin çağrılması durumunda uygun URI ve uygun kimlik bilgilerini sağlamanız gerekir. Bu üç öğe, API, URI gösterimini ve kimlik bilgilerinin sağlanmasına, Data Lake depolama Gen2.So, Data Lake depolama Gen1 arasında farklı uygulama yükseltme işleminin bir parçası olarak, bu üç yapıları uygun şekilde eşlemeniz gerekir.

#### <a name="uri-changes"></a>URI değişiklikleri

Burada ana görevi öneki olan URI'si Çevir, `adl://` sahip URI'si içine bir `abfss://` önek.

Data Lake depolama Gen1 URI şeması bahsedilen [burada](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-store) ayrıntı, ancak genel anlamda içinde olduğu *adl://mydatalakestore.azuredatalakestore.net/\<Dosya_yolu\>.*

Data Lake depolama Gen2 dosyalara erişmek için URI şeması açıklanan [burada](../../hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2.md) ayrıntı, ancak genel anlamda içinde olduğu `abfss://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.widows.net/<PATH>`.

Var olan uygulamalarınız gidin ve bir URI'leri uygun şekilde için Data Lake depolama Gen2'ye işaret edecek şekilde değiştirdiğinizi emin olmak ihtiyacınız olanları. Ayrıca, uygun kimlik bilgilerini eklemeniz gerekecektir. Son olarak, özgün uygulamaları devre dışı bırakma ve yeni bir uygulama ile değiştirmek nasıl genel yükseltme stratejinizi yakından hizalanması gerekir.

#### <a name="custom-applications"></a>Özel uygulamalar

Data Lake depolama Gen1 ile uygulamanızın kullandığı arabirime bağlı olarak Data Lake depolama 2. nesil uyarlamak için değiştirmeniz gerekir.

##### <a name="rest-apis"></a>REST API'leri

Uygulamanız, Data Lake depolama REST API'lerini kullanıyorsa, uygulamanızı Data Lake depolama Gen2 REST API'lerini kullanmak için değiştirmeniz gerekecektir. Bağlantılar sunulmaktadır *programlama arabirimleri* bölümü.

##### <a name="sdks"></a>SDK’lar

Aşımı çağrılan *Yükseltme Hazırlık düzeyinizi değerlendirin* bölümünde, SDK'ları şu anda kullanıma sunulmaz. Data Lake depolama Gen2'ye, uygulamalarınızın üzerinde bağlantı noktası isterseniz, desteklenen Sdk'lardan kullanılabilir olmasını bekleyin öneririz.

##### <a name="powershell"></a>PowerShell

Yükseltme Hazırlığı bölümünüzü değerlendirme içinde bahsedilen gibi PowerShell desteği için veri düzlemi şu anda kullanılabilir değil.

Data Lake depolama Gen2 içinde'uygun fiyatlarla yönetim düzlemi commandlet'ler yerini alabilir. Bağlantılar sunulmaktadır *programlama arabirimleri* bölümü.

##### <a name="cli"></a>CLI

Aşımı çağrılan *Yükseltme Hazırlık düzeyinizi değerlendirin* bölümünde CLI desteği için veri düzlemi şu anda desteklenmiyor.

Data Lake depolama Gen2 içinde'uygun fiyatlarla yönetim düzlemi komutların yerini alabilir. Bağlantılar sunulmaktadır *programlama arabirimleri* bölümü.

### <a name="analytics-frameworks-upgrade"></a>Analytics çerçeveleri yükseltme

Uygulamanızı deposunda açık dosya ve klasör yolları gibi bilgiler hakkında daha fazla meta veri oluşturursa, veri/meta-data store yükselttikten sonra ek eylemler gerçekleştirmeniz gerekir. Bu, özellikle Azure HDInsight gibi analiz çerçevesini genellikle Kataloğu veri deposu verileri oluşturma Databricks vb. geçerlidir.

Analytics çerçeveleri veriler ve Data Lake depolama Gen1 ve 2. nesil gibi uzak depolar depolanan meta veriler ile çalışır. Teoride, altyapıları kısa ömürlü olabilir ve yalnızca depolanan veriler üzerinde çalıştırılacak işleri ihtiyacınız olduğunda getirilmesi için.

Ancak, performansı iyileştirmek için analiz çerçeveleri dosyalara ve klasörlere uzak depoda depolanan açık başvuruları oluşturmak ve ardından bunları tutmak için bir önbellek oluşturun. Uzak Veri URI'si, örneğin, Data Lake depolama Gen1 daha önce ve artık Data Lake depolama Gen2 içinde depolamak isteyen veri depolarken bir küme değiştirmelisiniz URI aynı kopyalanan içerik için farklı olacaktır. Verileri ve meta verileri Bu altyapılar önbellekleri yükselttikten sonra bu nedenle, da yeniden başlatılmış veya güncelleştirilmesi gerekir

Planlama işleminin bir parçası, uygulamanızı tanımlamak ve nasıl meta veri bilgileri artık Data Lake depolama 2. nesil'deki depolanan verileri işaret edecek şekilde yeniden başlatılmış olabilir kullanıma şekil gerekecektir. Aşağıda, yükseltme adımlarla yardımcı olmak yaygın olarak benimsenen analitik çerçeveler için yönergeler verilmiştir.

#### <a name="azure-databricks"></a>Azure Databricks

Seçtiğiniz yükseltme strateji bağlı olarak, adımları farklılık gösterir. Geçerli bölüm "Yaşam-and-shift" stratejisi seçmiş olduğunuz varsayılır. Ayrıca, bir Data Lake depolama Gen1 hesaptaki verilere erişmek için kullanılan mevcut Databricks çalışma alanı, Data Lake depolama Gen2 hesabına kopyalanır üzerinden verilerle çalışın beklenir.

İlk olarak, 2. nesil hesabı oluşturduğunuz ve ardından verileri ve meta Gen1 Gen2 için uygun bir araç kullanarak kopyalanır, emin olun. Bu araçların anılmaktadır [veri yükseltme](#data-upgrade) başlığına.

Ardından, [yükseltme](https://docs.azuredatabricks.net/user-guide/clusters/index.html) , Data Lake depolama Gen2 desteklemelidir Databricks çalışma zamanı 5.1 veya sonraki bir sürümünü kullanmaya başlamak için mevcut Databricks kümesine.

Adımları, bundan sonra varolan bir Databricks çalışma alanını Data Lake depolama Gen1 hesabındaki verilerin nasıl eriştiğini temel alır. Bunu çağıran adl ile yapılabilir: / / URI [doğrudan](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake.html#access-azure-data-lake-store-directly) dizüstü bilgisayarlar veya aracılığıyla [bağlama](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake.html#mount-azure-data-lake-store-with-dbfs).

Tam adl sağlayarak doğrudan Not defterlerinden erişiyorsanız: / / URI, her bir not defteri gidin ve karşılık gelen Data Lake depolama Gen2 URI erişmek üzere yapılandırmayı değiştirdiğinizde aktarmanız gerekir.

Bundan sonra bu Data Lake depolama Gen2 hesabına işaret edecek şekilde yeniden yapılandırmanız gerekir. Daha fazla değişiklik gereklidir ve Not Defterleri önceki gibi çalışmaya görüyor olmalısınız.

Herhangi bir yükseltme stratejileri kullanıyorsanız, gereksinimlerinizi karşılamak için yukarıdaki adımları çeşitlemesi oluşturabilirsiniz.

### <a name="azure-ecosystem-upgrade"></a>Azure ekosistemi yükseltme

Her araç ve Hizmetleri olarak adlandırılan bir [Azure ekosistemi](#azure-ecosystem) başlığına Data Lake depolama 2. nesil ile çalışacak şekilde yapılandırılması gerekir.

İlk olarak, Data Lake depolama 2. nesil ile tümleştirme olmasını sağlayın.

Ardından, öğeleri çekilerek yukarıda (örneğin: Değiştirilecek URI'si ve kimlik bilgileri) olacaktır. Data Lake depolama Gen1 ile çalışan var olan bir örneğini değiştirebilir veya Data Lake depolama 2. nesil ile işe yarar yeni bir örneğini oluşturabilirsiniz.

### <a name="partner-ecosystem-upgrade"></a>İş ortağı ekosistemi yükseltme

Lütfen Data Lake depolama 2. nesil ile çalışabilirler bileşen ve araçları sağlayarak iş ortağının çalışın. 

## <a name="performing-the-upgrade"></a>Yükseltme gerçekleştirme

### <a name="pre-upgrade"></a>Yükseltme öncesi

Bunun bir parçası olarak çalıştınız *Yükseltme Hazırlık düzeyinizi değerlendirin* bölümü ve [yükseltme için planlama](#planning-for-an-upgrade) bölümü bu kılavuz, tüm gerekli bilgileri aldığınız ve seçtiğiniz ihtiyaçlarınıza uygun bir plan oluşturuldu. Bu gibi durumlarda, bir test görevi büyük olasılıkla bu aşamada gerekir.

### <a name="in-upgrade"></a>Yükseltme

Seçtiğiniz strateji bağlı olarak ve bu aşama, çözümünüzün karmaşıklığını kısa bir ya da genişletilmiş bir olabilir. birden çok iş yükleri için Data Lake depolama Gen2 artımlı olarak taşınması bekleyen olduğu. Bu yükseltme en kritik bir parçası olacaktır.

### <a name="post-upgrade"></a>Yükseltme sonrası

Geçiş işlemi tamamlandıktan sonra tam doğrulama ilişkin son adımlar içerir. Bu dahil ancak bunlarla sınırlı değildir veri doğrulama kopyalandı üzerinden güvenilir bir şekilde, doğrulanıyor uçtan uca işlem hatları düzgün vb. doğrulanıyor ACL'leri doğru şekilde ayarlandı. Doğrulamaları tamamladıktan sonra artık eski, işlem hattını Aç, kaynak Data Lake depolama Gen1 hesaplarınızı silebilir ve Data Lake depolama Gen2 tabanlı çözümlerinizi tam hız gidin.

## <a name="conclusion"></a>Sonuç

Bu belgede sağlanan yönergeler Data Lake depolama Gen2 kullanmak için çözümünüzün yükseltmenize yardımcı. 

Daha fazla sorunuz varsa veya Geri bildiriminiz varsa, aşağıdaki yorum belirtin veya geri bildirim sağlayın [Azure geri bildirim Forumu](https://feedback.azure.com/forums/327234-data-lake).
