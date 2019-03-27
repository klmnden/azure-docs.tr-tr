---
title: 'Apache Hadoop hata ayıklama: Günlükleri görüntülemek ve hata iletilerini - Azure HDInsight yorumlama'
description: Hata iletileri, PowerShell kullanarak HDInsight yönetirken alabilirsiniz ve kurtarmak için attığınız adımlar hakkında bilgi edinin.
services: hdinsight
ms.reviewer: jasonh
author: ashishthaps
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/14/2017
ms.author: ashishth
ms.openlocfilehash: a035789af08aa4c0d877a06295d9bd6fdedf6844
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58449494"
---
# <a name="analyze-apache-hadoop-logs"></a>Apache Hadoop günlüklerini analiz etme

Her Azure HDInsight Apache Hadoop kümesinin varsayılan dosya sistemi olarak kullanılan bir Azure depolama hesabına sahiptir. Depolama hesabı varsayılan depolama hesabı olarak adlandırılır. Küme, günlükleri depolamak için varsayılan depolama hesabı Azure tablo depolama ve Blob Depolama kullanır.  Kümeniz için varsayılan depolama hesabını öğrenmek için bkz: [yönetme Apache Hadoop HDInsight kümeleri](../hdinsight-administer-use-portal-linux.md#find-the-storage-accounts). Hatta kümesi silindikten sonra depolama hesabında günlükleri korur.

## <a name="logs-written-to-azure-tables"></a>Azure tabloları yazılan günlükleri

Azure tabloları yazılan günlükler bir düzey bir HDInsight kümesi ile neler Öngörüler sağlar.

Bir HDInsight kümesi oluşturduğunuzda, Linux tabanlı kümelerde varsayılan tablo depolama için altı tabloları otomatik olarak oluşturulur:

* hdinsightagentlog
* syslog
* daemonlog
* hadoopservicelog
* ambariserverlog
* ambariagentlog

Tablo dosya adları **u<ClusterName>DDMonYYYYatHHMMSSsss<TableName>**.

Bu tablo, aşağıdaki alanları içerir:

* ClusterDnsName
* ComponentName
* eventTimestamp
* Host
* MALoggingHash
* İleti
* N
* PreciseTimeStamp
* Rol
* RowIndex
* Kiracı
* ZAMAN DAMGASI
* TraceLevel

### <a name="tools-for-accessing-the-logs"></a>Günlüklere erişmek için Araçlar
Bu tablolardaki verilere erişmek için kullanılabilen birçok araç vardır:

* Visual Studio
* Azure Depolama Gezgini
* Excel için Power Query

#### <a name="use-power-query-for-excel"></a>Excel için Power Query kullanın
Power Query yüklenebilir [Excel için Microsoft Power Query](https://www.microsoft.com/en-us/download/details.aspx?id=39379). Sistem gereksinimleri için indirme sayfasına bakın.

**Açın ve hizmet günlüğü analiz etmek için Power Query kullanın.**

1. Açık **Microsoft Excel**.
2. Gelen **Power Query** menüsünde tıklatın **Azure**ve ardından **gelen Microsoft Azure tablo depolama**.
   
    ![HDInsight Hadoop Excel PowerQuery açın Azure tablo depolama](./media/apache-hadoop-debug-jobs/hdinsight-hadoop-analyze-logs-using-excel-power-query-open.png)
3. Depolama hesabı adı, kısa adını veya FQDN girin.
4. Depolama hesabı anahtarını girin. Tabloların listesini göreceksiniz:
   
    ![HDInsight Hadoop günlüklerini Azure tablo depolamada depolanan](./media/apache-hadoop-debug-jobs/hdinsight-hadoop-analyze-logs-table-names.png)
5. Hadoopservicelog tabloya sağ **Gezgin** bölmesi ve select **Düzenle**. Dört sütun göreceksiniz. İsteğe bağlı olarak silme **bölüm anahtarı**, **satır anahtarı**, ve **zaman damgası** bunları seçip tıklayarak sütunları **sütunları Kaldır** gelen Şeritte seçenekleri.
6. İçerik sütununda Excel elektronik tablosuna içeri aktarmak istediğiniz sütunları seçmek için genişlet simgesini tıklatın. Bu Tanıtım için TraceLevel ve ComponentName seçmiştir: Bana hangi bileşenlerin sorunlar yaşadım bazı temel bilgileri verebilirsiniz.
   
    ![HDInsight Hadoop günlüklerini sütunları seçin](./media/apache-hadoop-debug-jobs/hdinsight-hadoop-analyze-logs-using-excel-power-query-filter.png)
7. Tıklayın **Tamam** verileri içeri aktarma.
8. Seçin **TraceLevel**, rol ve **ComponentName** sütunları ve ardından **Group By** Şerit denetimi.
9. Tıklayın **Tamam** Group By iletişim kutusunda
10. Uygulama tıklayın ** & Kapat **.

Artık, filtreleme ve sıralama gerektiği için Excel kullanabilirsiniz. Sorun ortaya ancak Hadoop Hizmetleri ile neler makul bir resim seçme ve yukarıda açıklanan sütunları gruplandırma sağlar detaya için (ileti gibi) diğer sütunlar eklemek isteyebilirsiniz. Aynı fikir setuplog ve hadoopinstalllog tablolara uygulanabilir.

#### <a name="use-visual-studio"></a>Visual Studio'yu kullanma
**Visual Studio kullanma**

1. Visual Studio'yu açın.
2. Gelen **görünümü** menüsünde tıklatın **Cloud Explorer**. Veya tıklamanız yeterlidir **CTRL +\, CTRL + X**.
3. Gelen **Cloud Explorer**seçin **kaynak türleri**.  Diğer kullanılabilir seçenek **kaynak grupları**.
4. Genişletin **depolama hesapları**, kümenizin, varsayılan depolama hesabını ve ardından **tabloları**.
5. Çift **hadoopservicelog**.
6. Bir filtre ekleyin. Örneğin:
   
        TraceLevel eq 'ERROR'
   
    ![HDInsight Hadoop günlüklerini sütunları seçin](./media/apache-hadoop-debug-jobs/hdinsight-hadoop-analyze-logs-visual-studio-filter.png)
   
    Filtreleri oluşturma hakkında daha fazla bilgi için bkz. [oluşturun, Tablo Tasarımcısı için filtre dizelerinde](../../vs-azure-tools-table-designer-construct-filter-strings.md).

## <a name="logs-written-to-azure-blob-storage"></a>Azure Blob depolama alanına yazılan günlükleri
Azure tabloları yazılan günlükler bir düzey bir HDInsight kümesi ile neler Öngörüler sağlar. Görev düzeyinde günlükleri, ayrıntılara yararlı olabilir ancak, bu tablolar sağlamaz daha da ortaya çıkan sorunlar. Bu sonraki ayrıntı düzeyini sağlamak için Blob Depolama hesabınıza templeton da gönderilen herhangi bir iş için görev günlüklerini yazma izni HDInsight kümeleri yapılandırılır. Pratikte, bu Microsoft Azure PowerShell cmdlet'lerini veya .NET iş gönderme API'leri, RDP/komut satırı erişim kümesine gönderilen işler kullanılarak gönderilen bir iş anlamına gelir. 

Günlükleri görüntülemek için bkz: [erişim Apache Hadoop YARN uygulama günlüklerine üzerinde Linux tabanlı HDInsight](../hdinsight-hadoop-access-yarn-app-logs-linux.md).


Uygulama günlükleri hakkında daha fazla bilgi için bkz. [günlükleri kullanıcı yönetimi ve Apache Hadoop YARN erişimi basitleştirme](https://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/).


## <a name="view-cluster-health-and-job-logs"></a>Küme durumunu ve iş günlüklerini görüntüleme
### <a name="access-the-ambari-ui"></a>Ambari UI erişim
Azure portalından küme bölmesini açmak için bir HDInsight küme adına tıklayın. Küme bölmesinden tıklayın **Pano**.

![Küme panosunu başlatma](./media/apache-hadoop-debug-jobs/hdi-debug-launch-dashboard.png)


### <a name="access-the-yarn-ui"></a>Yarn UI erişim
Azure portalından küme bölmesini açmak için bir HDInsight küme adına tıklayın. Küme bölmesinden tıklayın **Pano**. İstendiğinde, Küme Yöneticisi kimlik bilgilerini girin. Ambari seçin **YARN** Hizmetleri soldaki listesinden. Görüntülenen sayfasında **hızlı bağlantılar**, sonra etkin baş düğüm girişi ve Resource Manager UI'ı seçin.

YARN kullanıcı Arabiriminde, aşağıdakileri yapmak için kullanabilirsiniz:

* **Küme durumunu alma**. Sol bölmeden genişletin **küme**, tıklatıp **hakkında**. Bu mevcut durumu ayrıntıları toplam ayrılan bellek, çekirdek kullanıldığında, Küme Kaynak Yöneticisi, küme sürümü ve benzeri durumu gibi küme.
  
    ![Küme panosunu başlatma](./media/apache-hadoop-debug-jobs/hdi-debug-yarn-cluster-state.png)
* **Düğüm durumunu Al**. Sol bölmeden genişletin **küme**, tıklatıp **düğümleri**. Burada, kümedeki tüm düğümlerin, HTTP adresi her düğümün her düğüm, vb. için ayrılan kaynaklar listelenir.
* **İş durumunu izlemek**. Sol bölmeden genişletin **küme**ve ardından **uygulamaları** kümedeki tüm işleri listelemek için. İşleri belirli bir durumda (örneğin, yeni, gönderilen, çalışan, vb.) bakmak isterseniz altındaki ilgili bağlantıyı tıklatın **uygulamaları**. Daha fazla iş adı proje hakkında daha fazla bilgi çıkış, günlükleri, vb. dahil olmak üzere tür bulmak için de tıklayabilirsiniz.

### <a name="access-the-hbase-ui"></a>HBase UI erişim
Azure portalından küme bölmesini açmak için bir HDInsight HBase küme adına tıklayın. Küme bölmesinden tıklayın **Pano**. İstendiğinde, Küme Yöneticisi kimlik bilgilerini girin. Ambari HBase, hizmetler listesinden seçin. Seçin **hızlı bağlantılar** sayfanın üstündeki etkin Zookeeper düğümü bağlantısının üzerine gelin ve HBase Master kullanıcı ARABİRİMİ'ye tıklayın.

## <a name="hdinsight-error-codes"></a>HDInsight hata kodları
Bu bölümde listelenen hata iletileri, Azure HDInsight Hadoop kullanıcıları Azure PowerShell kullanarak hizmet yönetirken sınırlamasıyla olası hata koşulları anlamak yardımcı olmak ve bunları gerçekleştirilebilecek adımları bildirmek için sağlanır. hatadan kurtarmak için.

HDInsight kümelerini yönetmek için kullanıldığında bu hata iletileri bazıları aynı zamanda Azure Portalı'nda görülebilir. Ancak karşılaşabileceğiniz diğer hata iletileri vardır bu bağlamda olası düzeltici eylemlerde kısıtlamaları nedeniyle daha az ayrıntılı. Diğer hata iletileri, risk azaltma açık olduğu bağlamlarda sağlanır. 

### <a id="AtLeastOneSqlMetastoreMustBeProvided"></a>AtLeastOneSqlMetastoreMustBeProvided
* **Açıklama**: Hive ve Oozie meta depolar için özel ayarları kullanmak için lütfen en az bir bileşen için Azure SQL veritabanı ayrıntılarını belirtin.
* **Risk azaltma**: Kullanıcının geçerli bir SQL Azure meta veri deposu sağlayın ve isteği yeniden denemek gerekir.  

### <a id="AzureRegionNotSupported"></a>AzureRegionNotSupported
* **Açıklama**: Küme bölgede oluşturulamadı *nameOfYourRegion*. Geçerli bir HDInsight bölge kullanın ve isteği yeniden deneyin.
* **Risk azaltma**: Müşteri, bunları destekleyen küme bölge oluşturmanız gerekir: Güneydoğu Asya, Batı Avrupa, Kuzey Avrupa, Doğu ABD ve Batı ABD.  

### <a id="ClusterContainerRecordNotFound"></a>ClusterContainerRecordNotFound
* **Açıklama**: Sunucu, istenen küme kaydı bulunamadı.  
* **Risk azaltma**: İşlemi yeniden deneyin.

### <a id="ClusterDnsNameInvalidReservedWord"></a>ClusterDnsNameInvalidReservedWord
* **Açıklama**: Küme DNS adını *yourDnsName* geçersiz. Lütfen adı başlar ve alfasayısal biten olduğundan emin olun ve yalnızca '-' özel karakter  
* **Risk azaltma**: Başlayan ve alfasayısal sona erer ve özel içeren kümenizi dash dışındaki karakterleri için geçerli bir DNS adı kullandığınızı doğrulayın '-' ve sonra işlemi yeniden deneyin.

### <a id="ClusterNameUnavailable"></a>ClusterNameUnavailable
* **Açıklama**: Küme adı *yourClusterName* kullanılamıyor. Lütfen başka bir ad seçin.  
* **Risk azaltma**: Kullanıcı benzersiz olan bir küme adı belirtin ve mevcut değil, yeniden deneyin. Kullanıcı Portalı kullanıyorsanız, kullanıcı Arabirimi oluşturma adımları sırasında bir küme adı zaten kullanılıyor, bunları bildirir.

### <a id="ClusterPasswordInvalid"></a>ClusterPasswordInvalid
* **Açıklama**: Küme parolası geçersiz. Parola en az 10 karakter uzunluğunda olmalıdır ve en az bir rakam, büyük harf, küçük harf ve boşluk içermeyen özel karakter içermelidir ve kullanıcı adı, bir parçası olarak içermemelidir.  
* **Risk azaltma**: Geçerli Küme parolası sağlayın ve işlemi yeniden deneyin.

### <a id="ClusterUserNameInvalid"></a>ClusterUserNameInvalid
* **Açıklama**: Küme kullanıcı adı geçersiz. Lütfen kullanıcı adı özel karakterler veya boşluk içermediğinden emin olun.  
* **Risk azaltma**: Geçerli Küme kullanıcı adını belirtin ve işlemi yeniden deneyin.

### <a id="ClusterUserNameInvalidReservedWord"></a>ClusterUserNameInvalidReservedWord
* **Açıklama**: Küme DNS adını *yourDnsClusterName* geçersiz. Lütfen adı başlar ve alfasayısal biten olduğundan emin olun ve yalnızca '-' özel karakter  
* **Risk azaltma**: Geçerli bir DNS küme kullanıcı adı sağlayın ve işlemi yeniden deneyin.

### <a id="ContainerNameMisMatchWithDnsName"></a>ContainerNameMisMatchWithDnsName
* **Açıklama**: Kapsayıcı adı URI *yourcontainerURI* ve DNS adı *yourDnsName* istek gövdesi aynı olmalıdır.  
* **Risk azaltma**: Kapsayıcınızın adı ve DNS adı aynıdır ve işlemi yeniden deneyin emin olun.

### <a id="DataNodeDefinitionNotFound"></a>DataNodeDefinitionNotFound
* **Açıklama**: Geçersiz küme yapılandırması. Düğüm boyutu içinde herhangi bir veri düğümünü tanımı bulmak yüklenemiyor.  
* **Risk azaltma**: İşlemi yeniden deneyin.

### <a id="DeploymentDeletionFailure"></a>DeploymentDeletionFailure
* **Açıklama**: Küme için dağıtım silme işlemi başarısız oldu  
* **Risk azaltma**: Silme işlemini yeniden deneyin.

### <a id="DnsMappingNotFound"></a>DnsMappingNotFound
* **Açıklama**: Hizmet yapılandırma hatası. Gerekli DNS eşlemesini bilgileri bulunamadı.  
* **Risk azaltma**: Küme silme ve yeni bir küme oluşturun.

### <a id="DuplicateClusterContainerRequest"></a>DuplicateClusterContainerRequest
* **Açıklama**: Yinelenen küme kapsayıcı oluşturma girişimi. Kayıt mevcut için *nameOfYourContainer* ancak Etag'ler eşleşmiyor.
* **Risk azaltma**: Kapsayıcı için benzersiz bir ad sağlayın ve oluşturma işlemi yeniden deneyin.

### <a id="DuplicateClusterInHostedService"></a>DuplicateClusterInHostedService
* **Açıklama**: Barındırılan hizmet *nameOfYourHostedService* bir küme zaten içeriyor. Barındırılan bir hizmet birden fazla küme içeremez.  
* **Risk azaltma**: Başka bir barındırılan hizmet kümesinde barındırın.

### <a id="FailureToUpdateDeploymentStatus"></a>FailureToUpdateDeploymentStatus
* **Açıklama**: Sunucu, küme dağıtım durumu güncelleştirilemedi.  
* **Risk azaltma**: İşlemi yeniden deneyin. Birden çok kez böyle bir durumda, CSS ile iletişime geçin.

### <a id="HdiRestoreClusterAltered"></a>HdiRestoreClusterAltered
* **Açıklama**: Küme *yourClusterName* bakım işleminin bir parçası olarak silindi. Lütfen küme oluşturun.
* **Risk azaltma**: Kümeyi yeniden oluşturun.

### <a id="HeadNodeConfigNotFound"></a>HeadNodeConfigNotFound
* **Açıklama**: Geçersiz küme yapılandırması. Gerekli baş düğüm yapılandırması düğüm boyutu bulunamadı.
* **Risk azaltma**: İşlemi yeniden deneyin.

### <a id="HostedServiceCreationFailure"></a>HostedServiceCreationFailure
* **Açıklama**: Barındırılan hizmet oluşturulamıyor *nameOfYourHostedService*. Lütfen isteği yeniden deneyin.  
* **Risk azaltma**: İsteği yeniden deneyin.

### <a id="HostedServiceHasProductionDeployment"></a>HostedServiceHasProductionDeployment
* **Açıklama**: Barındırılan hizmet *nameOfYourHostedService* Üretim dağıtımı zaten sahip. Barındırılan bir hizmet birden çok üretim dağıtımları içeremez. İstek farklı bir küme adı ile yeniden deneyin.
* **Risk azaltma**: Farklı bir küme adı kullanın ve isteği yeniden deneyin.

### <a id="HostedServiceNotFound"></a>HostedServiceNotFound
* **Açıklama**: Barındırılan hizmet *nameOfYourHostedService* için küme bulunamadı.  
* **Risk azaltma**: Küme hata durumundaysa, silin ve yeniden deneyin.

### <a id="HostedServiceWithNoDeployment"></a>HostedServiceWithNoDeployment
* **Açıklama**: Barındırılan hizmet *nameOfYourHostedService* ilişkili dağıtım sahiptir.  
* **Risk azaltma**: Küme hata durumundaysa, silin ve yeniden deneyin.

### <a id="InsufficientResourcesCores"></a>InsufficientResourcesCores
* **Açıklama**: Subscriptionıd *yourSubscriptionId* kümesi oluşturmak için sol çekirdek yok *yourClusterName*. Gerekli: *resourcesRequired*, kullanılabilir: *resourcesAvailable*.  
* **Risk azaltma**: Aboneliğinizdeki kaynakları boşaltmaya veya abonelik için kullanılabilir kaynakları artırın ve küme yeniden oluşturmayı deneyin.

### <a id="InsufficientResourcesHostedServices"></a>InsufficientResourcesHostedServices
* **Açıklama**: Abonelik kimliği *yourSubscriptionId* kümesi oluşturmak yeni bir HostedService için kota yok *yourClusterName*.  
* **Risk azaltma**: Aboneliğinizdeki kaynakları boşaltmaya veya abonelik için kullanılabilir kaynakları artırın ve küme yeniden oluşturmayı deneyin.

### <a id="InternalErrorRetryRequest"></a>InternalErrorRetryRequest
* **Açıklama**: Sunucu bir iç hatayla karşılaştı. Lütfen isteği yeniden deneyin.  
* **Risk azaltma**: İsteği yeniden deneyin.

### <a id="InvalidAzureStorageLocation"></a>InvalidAzureStorageLocation
* **Açıklama**: Azure depolama konumu *dataRegionName* geçerli bir konum değil. Bölge doğru olduğundan emin olun ve isteği yeniden deneyin.
* **Risk azaltma**: HDInsight'ı destekleyen bir depolama konumu seçin, kümenizi birlikte bulunan olup olmadığını denetleyin ve işlemi yeniden deneyin.

### <a id="InvalidNodeSizeForDataNode"></a>InvalidNodeSizeForDataNode
* **Açıklama**: Veri düğümleri için VM boyutu geçersiz. Yalnızca 'Büyük VM' boyutu, tüm veri düğümleri için desteklenir.  
* **Risk azaltma**: Veri düğümü için desteklenen düğüm boyutu belirtin ve işlemi yeniden deneyin.

### <a id="InvalidNodeSizeForHeadNode"></a>InvalidNodeSizeForHeadNode
* **Açıklama**: Baş düğüm için VM boyutu geçersiz. Yalnızca 'ExtraLarge VM' boyutu, baş düğüm için desteklenir.  
* **Risk azaltma**: Baş düğüm için desteklenen düğüm boyutu belirtin ve işlemi yeniden deneyin

### <a id="InvalidRightsForDeploymentDeletion"></a>InvalidRightsForDeploymentDeletion
* **Açıklama**: Abonelik kimliği *yourSubscriptionId* kullanılan küme için silme işlemi yürütmek için yeterli izinleri yok *yourClusterName*.  
* **Risk azaltma**: Küme hata durumundaysa, sürükleyip bırakın ve işlemi yeniden deneyin.  

### <a id="InvalidStorageAccountBlobContainerName"></a>InvalidStorageAccountBlobContainerName
* **Açıklama**: Dış depolama hesabına blob kapsayıcısı adı *yourContainerName* geçersiz. Adı bir harfle başlar ve yalnızca küçük harf, rakam ve tire içeren emin olun.  
* **Risk azaltma**: Geçerli bir depolama hesabı blob kapsayıcı adı belirtin ve işlemi yeniden deneyin.

### <a id="InvalidStorageAccountConfigurationSecretKey"></a>InvalidStorageAccountConfigurationSecretKey
* **Açıklama**: Harici depolama hesabı için yapılandırmayı *yourStorageAccountName* ayarlamak için gizli anahtar ayrıntılarını sahip olması gerekir.  
* **Risk azaltma**: Depolama hesabı için geçerli bir gizli anahtarı belirtin ve işlemi yeniden deneyin.

### <a id="InvalidVersionHeaderFormat"></a>InvalidVersionHeaderFormat
* **Açıklama**: Sürüm üst bilgisi *yourVersionHeader* yyyy-aa-gg biçiminde geçerli biçimde değil  
* **Risk azaltma**: Sürüm üst bilgisi için geçerli bir biçim belirtin ve isteği yeniden deneyin.

### <a id="MoreThanOneHeadNode"></a>MoreThanOneHeadNode
* **Açıklama**: Geçersiz küme yapılandırması. Birden fazla baş düğüm yapılandırması bulunamadı.  
* **Risk azaltma**: Yalnızca tek bir baş düğüm belirtilen şekilde yapılandırmasını düzenleyin.

### <a id="OperationTimedOutRetryRequest"></a>OperationTimedOutRetryRequest
* **Açıklama**: İşlemi izin verilen sürede tamamlanamadı veya en fazla yeniden deneme mümkün çalışır. Lütfen isteği yeniden deneyin.  
* **Risk azaltma**: İsteği yeniden deneyin.

### <a id="ParameterNullOrEmpty"></a>ParameterNullOrEmpty
* **Açıklama**: Parametre *yourParameterName* null veya boş olamaz.  
* **Risk azaltma**: Parametresi için geçerli bir değer belirtin.

### <a id="PreClusterCreationValidationFailure"></a>PreClusterCreationValidationFailure
* **Açıklama**: Bir veya daha fazla küme oluşturma isteği girişleri geçerli değil. Giriş değerleri doğru olduğundan ve isteği yeniden deneyin emin olun.  
* **Risk azaltma**: Giriş değerleri doğru olduğundan ve isteği yeniden deneyin emin olun.

### <a id="RegionCapabilityNotAvailable"></a>RegionCapabilityNotAvailable
* **Açıklama**: Bölge özellik bölgede kullanılamıyor *yourRegionName* ve abonelik Kimliğinin *yourSubscriptionId*.  
* **Risk azaltma**: HDInsight kümeleri destekleyen bir bölge belirtin. Genel olarak desteklenen bölgeleri şunlardır: Güneydoğu Asya, Batı Avrupa, Kuzey Avrupa, Doğu ABD ve Batı ABD.

### <a id="StorageAccountNotColocated"></a>StorageAccountNotColocated
* **Açıklama**: Depolama hesabı *yourStorageAccountName* bölgede *currentRegionName*. Küme bölgede aynı olmalıdır *yourClusterRegionName*.  
* **Risk azaltma**: Kümenizin bulunduğu aynı bölgede bir depolama hesabı belirtin veya veri depolama hesabında zaten ise, depolama hesabınız ile aynı bölgede yeni bir küme oluşturun. UI portalı kullanıyorsanız, bunları bu sorunun önceden bildirir.

### <a id="SubscriptionIdNotActive"></a>SubscriptionIdNotActive
* **Açıklama**: Abonelik kimliği verilen *yourSubscriptionId* etkin değil.  
* **Risk azaltma**: Aboneliğinizi yeniden etkinleştirmek ya da yeni bir geçerli abonelik alın.

### <a id="SubscriptionIdNotFound"></a>SubscriptionIdNotFound
* **Açıklama**: Abonelik kimliği *yourSubscriptionId* bulunamadı.  
* **Risk azaltma**: Abonelik Kimliğinizi geçerli olduğundan emin olun ve işlemi yeniden deneyin.

### <a id="UnableToResolveDNS"></a>UnableToResolveDNS
* **Açıklama**: DNS çözümlenemiyor *yourDnsUrl*. Lütfen blob uç noktası için tam URL sağlandığından emin olun.  
* **Risk azaltma**: Geçerli bir blob URL'sini sağlayın. URL olmalıdır başlayarak dahil olmak üzere tam olarak geçerli *http://* ve sonu *.com*.

### <a id="UnableToVerifyLocationOfResource"></a>UnableToVerifyLocationOfResource
* **Açıklama**: Kaynak konumu doğrulanamadı *yourDnsUrl*. Lütfen blob uç noktası için tam URL sağlandığından emin olun.  
* **Risk azaltma**: Geçerli bir blob URL'sini sağlayın. URL olmalıdır başlayarak dahil olmak üzere tam olarak geçerli *http://* ve sonu *.com*.

### <a id="VersionCapabilityNotAvailable"></a>VersionCapabilityNotAvailable
* **Açıklama**: Sürüm için kullanılabilir sürüm özelliği *specifiedVersion* ve abonelik Kimliğinin *yourSubscriptionId*.  
* **Risk azaltma**: Kullanılabilir bir sürümü seçin ve işlemi yeniden deneyin.

### <a id="VersionNotSupported"></a>VersionNotSupported
* **Açıklama**: Sürüm *specifiedVersion* desteklenmiyor.
* **Risk azaltma**: Desteklenen bir sürümünü seçin ve işlemi yeniden deneyin.

### <a id="VersionNotSupportedInRegion"></a>VersionNotSupportedInRegion
* **Açıklama**: Sürüm *specifiedVersion* Azure bölgesinde kullanılamıyor *specifiedRegion*.  
* **Risk azaltma**: Belirtilen bir bölgede desteklenen bir sürümünü seçin ve işlemi yeniden deneyin.

### <a id="WasbAccountConfigNotFound"></a>WasbAccountConfigNotFound
* **Açıklama**: Geçersiz küme yapılandırması. Gerekli WASB hesap yapılandırması dış hesapları bulunamadı.  
* **Risk azaltma**: Doğru yapılandırma dosyasında belirtilen hesabı var ve olduğunu doğrulayın ve işlemi yeniden deneyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Linux tabanlı HDInsight üzerinde Apache Hadoop Hizmetleri için yığın dökümlerini etkinleştirme](../hdinsight-hadoop-collect-debug-heap-dump-linux.md)
* [HDInsight kümelerini Apache Ambari Web arabiriminden yönetme](../hdinsight-hadoop-manage-ambari.md)
