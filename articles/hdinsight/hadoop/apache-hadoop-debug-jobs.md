---
title: 'HDInsight Hadoop hata ayıklama: günlükleri görüntüleyebilir ve hata iletilerini - Azure yorumlama '
description: Hata iletileri, PowerShell kullanarak HDInsight yönetirken alabilirsiniz ve kurtarmak için attığınız adımlar hakkında bilgi edinin.
services: hdinsight
editor: jasonwhowell
author: ashishthaps
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/14/2017
ms.author: ashishth
ms.openlocfilehash: 00d09619db11ea0026f5386048f1c10a8f831948
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39592790"
---
# <a name="analyze-hadoop-logs"></a>Hadoop günlüklerini analiz etme

Her Azure HDInsight Hadoop kümesinin varsayılan dosya sistemi olarak kullanılan bir Azure depolama hesabına sahiptir. Depolama hesabı varsayılan depolama hesabı olarak adlandırılır. Küme, günlükleri depolamak için varsayılan depolama hesabı Azure tablo depolama ve Blob Depolama kullanır.  Kümeniz için varsayılan depolama hesabını öğrenmek için bkz: [yönetme Hadoop kümeleri HDInsight](../hdinsight-administer-use-management-portal.md#find-the-default-storage-account). Hatta kümesi silindikten sonra depolama hesabında günlükleri korur.

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
Power Query yüklenebilir [Excel için Microsoft Power Query](http://www.microsoft.com/en-us/download/details.aspx?id=39379). Sistem gereksinimleri için indirme sayfasına bakın.

**Açın ve hizmet günlüğü analiz etmek için Power Query kullanın.**

1. Açık **Microsoft Excel**.
2. Gelen **Power Query** menüsünde tıklatın **Azure**ve ardından **gelen Microsoft Azure tablo depolama**.
   
    ![HDInsight Hadoop Excel PowerQuery açın Azure tablo depolama](./media/apache-hadoop-debug-jobs/hdinsight-hadoop-analyze-logs-using-excel-power-query-open.png)
3. Depolama hesabı adı, kısa adını veya FQDN girin.
4. Depolama hesabı anahtarını girin. Tabloların listesini göreceksiniz:
   
    ![HDInsight Hadoop günlüklerini Azure tablo depolamada depolanan](./media/apache-hadoop-debug-jobs/hdinsight-hadoop-analyze-logs-table-names.png)
5. Hadoopservicelog tabloya sağ **Gezgin** bölmesi ve select **Düzenle**. Dört sütun göreceksiniz. İsteğe bağlı olarak silme **bölüm anahtarı**, **satır anahtarı**, ve **zaman damgası** bunları seçip tıklayarak sütunları **sütunları Kaldır** gelen Şeritte seçenekleri.
6. İçerik sütununda Excel elektronik tablosuna içeri aktarmak istediğiniz sütunları seçmek için genişlet simgesini tıklatın. Bu Tanıtım için TraceLevel ve ComponentName seçtiğim: Bu benim bileşenleri üzerinde sorunları olan bazı temel bilgileri verin.
   
    ![HDInsight Hadoop günlüklerini sütunları seçin](./media/apache-hadoop-debug-jobs/hdinsight-hadoop-analyze-logs-using-excel-power-query-filter.png)
7. Tıklayın **Tamam** verileri içeri aktarma.
8. Seçin **TraceLevel**, rol ve **ComponentName** sütunları ve ardından **Group By** Şerit denetimi.
9. Tıklayın **Tamam** Group By iletişim kutusunda
10. Uygulama tıklayın ** & Kapat **.

Artık, filtreleme ve sıralama gerektiği için Excel kullanabilirsiniz. Sorun ortaya ancak Hadoop Hizmetleri ile neler makul bir resim seçme ve yukarıda açıklanan sütunları gruplandırma sağlar detaya için (ileti gibi) diğer sütunlar eklemek isteyebilirsiniz. Aynı fikir setuplog ve hadoopinstalllog tablolara uygulanabilir.

#### <a name="use-visual-studio"></a>Visual Studio'yu kullanma
**Visual Studio'yu kullanın.**

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
[Azure tabloları yazılan günlükler](#log-written-to-azure-tables) bir düzeyde bir HDInsight kümesi ile neler Öngörüler sağlar. Görev düzeyinde günlükleri, ayrıntılara yararlı olabilir ancak, bu tablolar sağlamaz daha da ortaya çıkan sorunlar. Bu sonraki ayrıntı düzeyini sağlamak için Blob Depolama hesabınıza templeton da gönderilen herhangi bir iş için görev günlüklerini yazma izni HDInsight kümeleri yapılandırılır. Pratikte, bu Microsoft Azure PowerShell cmdlet'lerini veya .NET iş gönderme API'leri, RDP/komut satırı erişim kümesine gönderilen işler kullanılarak gönderilen bir iş anlamına gelir. 

Günlükleri görüntülemek için bkz: [erişim YARN uygulama günlüklerine üzerinde Linux tabanlı HDInsight](../hdinsight-hadoop-access-yarn-app-logs-linux.md).

Uygulama günlükleri hakkında daha fazla bilgi için bkz. [günlükleri kullanıcı yönetimi ve erişim yarn'da basitleştirme](http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/).

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

### <a id="AtleastOneSqlMetastoreMustBeProvided"></a>AtleastOneSqlMetastoreMustBeProvided
* **Açıklama**: Hive ve Oozie meta depolar için özel ayarları kullanmak için lütfen en az bir bileşen için Azure SQL veritabanı ayrıntılarını belirtin.
* **Risk azaltma**: kullanıcının erişmesi geçerli bir SQL Azure meta veri deposu sağlayın ve isteği yeniden deneyin.  

### <a id="AzureRegionNotSupported"></a>AzureRegionNotSupported
* **Açıklama**: küme bölgede oluşturulamadı *nameOfYourRegion*. Geçerli bir HDInsight bölge kullanın ve isteği yeniden deneyin.
* **Risk azaltma**: Müşteri şu anda bunları destekleyen küme bölge oluşturmalıdır: Güneydoğu Asya, Batı Avrupa, Kuzey Avrupa, Doğu ABD ve Batı ABD.  

### <a id="ClusterContainerRecordNotFound"></a>ClusterContainerRecordNotFound
* **Açıklama**: sunucu istenen küme kaydı bulunamadı.  
* **Risk azaltma**: işlemi yeniden deneyin.

### <a id="ClusterDnsNameInvalidReservedWord"></a>ClusterDnsNameInvalidReservedWord
* **Açıklama**: küme DNS adı *yourDnsName* geçersiz. Lütfen adı başlar ve alfasayısal biten olduğundan emin olun ve yalnızca '-' özel karakter  
* **Risk azaltma**: başlar ve alfasayısal biten özel içeren kümenizdeki dash dışındaki karakterleri için geçerli bir DNS adı kullandığınızı doğrulayın '-' ve sonra işlemi yeniden deneyin.

### <a id="ClusterNameUnavailable"></a>ClusterNameUnavailable
* **Açıklama**: küme adını *yourClusterName* kullanılamıyor. Lütfen başka bir ad seçin.  
* **Risk azaltma**: kullanıcı benzersiz olan bir küme adı belirtin ve mevcut değil, yeniden deneyin. Kullanıcı Portalı kullanıyorsanız, kullanıcı Arabirimi oluşturma adımları sırasında bir küme adı zaten kullanılıyor, bunları bildirir.

### <a id="ClusterPasswordInvalid"></a>ClusterPasswordInvalid
* **Açıklama**: Küme parolası geçersiz. Parola en az 10 karakter uzunluğunda olmalıdır ve en az bir rakam, büyük harf, küçük harf ve boşluk içermeyen özel karakter içermelidir ve kullanıcı adı, bir parçası olarak içermemelidir.  
* **Risk azaltma**: Geçerli Küme parolası sağlayın ve işlemi yeniden deneyin.

### <a id="ClusterUserNameInvalid"></a>ClusterUserNameInvalid
* **Açıklama**: küme kullanıcı adı geçersiz. Lütfen kullanıcı adı özel karakterler veya boşluk içermediğinden emin olun.  
* **Risk azaltma**: geçerli küme kullanıcı adını belirtin ve işlemi yeniden deneyin.

### <a id="ClusterUserNameInvalidReservedWord"></a>ClusterUserNameInvalidReservedWord
* **Açıklama**: küme DNS adı *yourDnsClusterName* geçersiz. Lütfen adı başlar ve alfasayısal biten olduğundan emin olun ve yalnızca '-' özel karakter  
* **Risk azaltma**: geçerli bir DNS küme kullanıcı adı sağlayın ve işlemi yeniden deneyin.

### <a id="ContainerNameMisMatchWithDnsName"></a>ContainerNameMisMatchWithDnsName
* **Açıklama**: kapsayıcı adı URI *yourcontainerURI* ve DNS adı *yourDnsName* istek gövdesi aynı olmalıdır.  
* **Risk azaltma**: emin olun, kapsayıcınızın adı ve DNS adı aynıdır ve işlemi yeniden deneyin.

### <a id="DataNodeDefinitionNotFound"></a>DataNodeDefinitionNotFound
* **Açıklama**: Geçersiz küme yapılandırması. Düğüm boyutu içinde herhangi bir veri düğümünü tanımı bulmak yüklenemiyor.  
* **Risk azaltma**: işlemi yeniden deneyin.

### <a id="DeploymentDeletionFailure"></a>DeploymentDeletionFailure
* **Açıklama**: dağıtım silme işlemi, küme için başarısız oldu  
* **Risk azaltma**: silme işlemini yeniden deneyin.

### <a id="DnsMappingNotFound"></a>DnsMappingNotFound
* **Açıklama**: hizmet yapılandırma hatası. Gerekli DNS eşlemesini bilgileri bulunamadı.  
* **Risk azaltma**: küme silip yeni bir küme oluşturun.

### <a id="DuplicateClusterContainerRequest"></a>DuplicateClusterContainerRequest
* **Açıklama**: Yinelenen küme kapsayıcı oluşturma girişimi. Kayıt mevcut için *nameOfYourContainer* ancak Etag'ler eşleşmiyor.
* **Risk azaltma**: kapsayıcısı için benzersiz bir ad belirtin ve oluşturma işlemi yeniden deneyin.

### <a id="DuplicateClusterInHostedService"></a>DuplicateClusterInHostedService
* **Açıklama**: barındırılan hizmet *nameOfYourHostedService* bir küme zaten içeriyor. Barındırılan bir hizmet birden fazla küme içeremez.  
* **Risk azaltma**: başka bir barındırılan hizmet kümesinde barındırın.

### <a id="FailureToUpdateDeploymentStatus"></a>FailureToUpdateDeploymentStatus
* **Açıklama**: sunucu, küme dağıtım durumu güncelleştirilemedi.  
* **Risk azaltma**: işlemi yeniden deneyin. Birden çok kez böyle bir durumda, CSS ile iletişime geçin.

### <a id="HdiRestoreClusterAltered"></a>HdiRestoreClusterAltered
* **Açıklama**: küme *yourClusterName* bakım işleminin bir parçası olarak silindi. Lütfen küme oluşturun.
* **Risk azaltma**: küme oluşturun.

### <a id="HeadNodeConfigNotFound"></a>HeadNodeConfigNotFound
* **Açıklama**: Geçersiz küme yapılandırması. Gerekli baş düğüm yapılandırması düğüm boyutu bulunamadı.
* **Risk azaltma**: işlemi yeniden deneyin.

### <a id="HostedServiceCreationFailure"></a>HostedServiceCreationFailure
* **Açıklama**: barındırılan hizmet oluşturulamıyor *nameOfYourHostedService*. Lütfen isteği yeniden deneyin.  
* **Risk azaltma**: isteği yeniden deneyin.

### <a id="HostedServiceHasProductionDeployment"></a>HostedServiceHasProductionDeployment
* **Açıklama**: barındırılan hizmet *nameOfYourHostedService* Üretim dağıtımı zaten sahip. Barındırılan bir hizmet birden çok üretim dağıtımları içeremez. İstek farklı bir küme adı ile yeniden deneyin.
* **Risk azaltma**: farklı bir küme adı kullanın ve isteği yeniden deneyin.

### <a id="HostedServiceNotFound"></a>HostedServiceNotFound
* **Açıklama**: barındırılan hizmet *nameOfYourHostedService* için küme bulunamadı.  
* **Risk azaltma**: küme hata durumundaysa, silin ve yeniden deneyin.

### <a id="HostedServiceWithNoDeployment"></a>HostedServiceWithNoDeployment
* **Açıklama**: barındırılan hizmet *nameOfYourHostedService* ilişkili dağıtım sahiptir.  
* **Risk azaltma**: küme hata durumundaysa, silin ve yeniden deneyin.

### <a id="InsufficientResourcesCores"></a>InsufficientResourcesCores
* **Açıklama**: Subscriptionıd *yourSubscriptionId* kümesi oluşturmak için sol çekirdek yok *yourClusterName*. Gerekli: *resourcesRequired*, kullanılabilir: *resourcesAvailable*.  
* **Risk azaltma**:, aboneliğinizdeki kaynaklar boşaltın veya abonelik için kullanılabilir kaynakları artırın ve küme yeniden oluşturmayı deneyin.

### <a id="InsufficientResourcesHostedServices"></a>InsufficientResourcesHostedServices
* **Açıklama**: abonelik kimliği *yourSubscriptionId* kümesi oluşturmak yeni bir HostedService için kota yok *yourClusterName*.  
* **Risk azaltma**:, aboneliğinizdeki kaynaklar boşaltın veya abonelik için kullanılabilir kaynakları artırın ve küme yeniden oluşturmayı deneyin.

### <a id="InternalErrorRetryRequest"></a>InternalErrorRetryRequest
* **Açıklama**: sunucu bir iç hatayla karşılaştı. Lütfen isteği yeniden deneyin.  
* **Risk azaltma**: isteği yeniden deneyin.

### <a id="InvalidAzureStorageLocation"></a>InvalidAzureStorageLocation
* **Açıklama**: Azure depolama konumunda *dataRegionName* geçerli bir konum değil. Bölge doğru olduğundan emin olun ve isteği yeniden deneyin.
* **Risk azaltma**: HDInsight'ı destekleyen bir depolama konumu seçin, kümenizi birlikte bulunan olup olmadığını denetleyin ve işlemi yeniden deneyin.

### <a id="InvalidNodeSizeForDataNode"></a>InvalidNodeSizeForDataNode
* **Açıklama**: veri düğümleri için geçersiz VM boyutu. Yalnızca 'Büyük VM' boyutu, tüm veri düğümleri için desteklenir.  
* **Risk azaltma**: veri düğümü için desteklenen düğüm boyutu belirtin ve işlemi yeniden deneyin.

### <a id="InvalidNodeSizeForHeadNode"></a>InvalidNodeSizeForHeadNode
* **Açıklama**: baş düğüm için geçersiz VM boyutu. Yalnızca 'ExtraLarge VM' boyutu, baş düğüm için desteklenir.  
* **Risk azaltma**: baş düğüm için desteklenen düğüm boyutu belirtin ve işlemi yeniden deneyin

### <a id="InvalidRightsForDeploymentDeletion"></a>InvalidRightsForDeploymentDeletion
* **Açıklama**: abonelik kimliği *yourSubscriptionId* kullanılan küme için silme işlemi yürütmek için yeterli izinleri yok *yourClusterName*.  
* **Risk azaltma**: küme hata durumundaysa, sürükleyip bırakın ve işlemi yeniden deneyin.  

### <a id="InvalidStorageAccountBlobContainerName"></a>InvalidStorageAccountBlobContainerName
* **Açıklama**: dış depolama hesabına blob kapsayıcısı adı *yourContainerName* geçersiz. Adı bir harfle başlar ve yalnızca küçük harf, rakam ve tire içeren emin olun.  
* **Risk azaltma**: geçerli bir depolama hesabı blob kapsayıcı adı belirtin ve işlemi yeniden deneyin.

### <a id="InvalidStorageAccountConfigurationSecretKey"></a>InvalidStorageAccountConfigurationSecretKey
* **Açıklama**: dış depolama hesabı için yapılandırmayı *yourStorageAccountName* ayarlamak için gizli anahtar ayrıntılarını sahip olması gerekir.  
* **Risk azaltma**: depolama hesabı için geçerli bir gizli anahtarı belirtin ve işlemi yeniden deneyin.

### <a id="InvalidVersionHeaderFormat"></a>InvalidVersionHeaderFormat
* **Açıklama**: sürüm üst bilgisi *yourVersionHeader* yyyy-aa-gg biçiminde geçerli biçimde değil  
* **Risk azaltma**: sürüm üst bilgisi için geçerli bir biçim belirtin ve isteği yeniden deneyin.

### <a id="MoreThanOneHeadNode"></a>MoreThanOneHeadNode
* **Açıklama**: Geçersiz küme yapılandırması. Birden fazla baş düğüm yapılandırması bulunamadı.  
* **Risk azaltma**: yalnızca tek bir baş düğüm belirtilen şekilde yapılandırmasını düzenleyin.

### <a id="OperationTimedOutRetryRequest"></a>OperationTimedOutRetryRequest
* **Açıklama**: işlemi izin verilen sürede tamamlanamadı veya en fazla yeniden deneme mümkün çalışır. Lütfen isteği yeniden deneyin.  
* **Risk azaltma**: isteği yeniden deneyin.

### <a id="ParameterNullOrEmpty"></a>ParameterNullOrEmpty
* **Açıklama**: parametre *yourParameterName* null veya boş olamaz.  
* **Risk azaltma**: parametresi için geçerli bir değer belirtin.

### <a id="PreClusterCreationValidationFailure"></a>PreClusterCreationValidationFailure
* **Açıklama**: bir veya daha fazla küme oluşturma isteği girişleri geçerli değil. Giriş değerleri doğru olduğundan ve isteği yeniden deneyin emin olun.  
* **Risk azaltma**: Giriş değerleri doğru olduğundan ve isteği yeniden deneyin emin olun.

### <a id="RegionCapabilityNotAvailable"></a>RegionCapabilityNotAvailable
* **Açıklama**: bölge özellik bölgede kullanılamıyor *yourRegionName* ve abonelik Kimliğinin *yourSubscriptionId*.  
* **Risk azaltma**: HDInsight kümeleri destekleyen bir bölge belirtin. Genel olarak desteklenen bölgeler: Güneydoğu Asya, Batı Avrupa, Kuzey Avrupa, Doğu ABD ve Batı ABD.

### <a id="StorageAccountNotColocated"></a>StorageAccountNotColocated
* **Açıklama**: depolama hesabı *yourStorageAccountName* bölgede *currentRegionName*. Küme bölgede aynı olmalıdır *yourClusterRegionName*.  
* **Risk azaltma**: kümenizin bulunduğu aynı bölgede bir depolama hesabı belirtin veya veri depolama hesabında zaten ise, depolama hesabınız ile aynı bölgede yeni bir küme oluşturun. UI portalı kullanıyorsanız, bunları bu sorunun önceden bildirir.

### <a id="SubscriptionIdNotActive"></a>SubscriptionIdNotActive
* **Açıklama**: Belirtilen abonelik kimliği *yourSubscriptionId* etkin değil.  
* **Risk azaltma**: aboneliğinizi yeniden etkinleştirmek ya da yeni bir geçerli aboneliği edinin.

### <a id="SubscriptionIdNotFound"></a>SubscriptionIdNotFound
* **Açıklama**: abonelik kimliği *yourSubscriptionId* bulunamadı.  
* **Risk azaltma**: Abonelik Kimliğinizi geçerli olduğundan emin olun ve işlemi yeniden deneyin.

### <a id="UnableToResolveDNS"></a>UnableToResolveDNS
* **Açıklama**: DNS çözümlenemiyor *yourDnsUrl*. Lütfen blob uç noktası için tam URL sağlandığından emin olun.  
* **Risk azaltma**: geçerli blob URL'sini sağlayın. URL olmalıdır başlayarak dahil olmak üzere tam olarak geçerli *http://* ve sonu *.com*.

### <a id="UnableToVerifyLocationOfResource"></a>UnableToVerifyLocationOfResource
* **Açıklama**: kaynak konumunu doğrulanamadı *yourDnsUrl*. Lütfen blob uç noktası için tam URL sağlandığından emin olun.  
* **Risk azaltma**: geçerli blob URL'sini sağlayın. URL olmalıdır başlayarak dahil olmak üzere tam olarak geçerli *http://* ve sonu *.com*.

### <a id="VersionCapabilityNotAvailable"></a>VersionCapabilityNotAvailable
* **Açıklama**: sürüm için kullanılabilir sürüm özelliği *specifiedVersion* ve abonelik Kimliğinin *yourSubscriptionId*.  
* **Risk azaltma**: kullanılabilir bir sürümü seçin ve işlemi yeniden deneyin.

### <a id="VersionNotSupported"></a>VersionNotSupported
* **Açıklama**: sürüm *specifiedVersion* desteklenmiyor.
* **Risk azaltma**: desteklenen bir sürümünü seçin ve işlemi yeniden deneyin.

### <a id="VersionNotSupportedInRegion"></a>VersionNotSupportedInRegion
* **Açıklama**: sürüm *specifiedVersion* Azure bölgesinde kullanılamıyor *specifiedRegion*.  
* **Risk azaltma**: belirtilen bir bölgede desteklenen bir sürümünü seçin ve işlemi yeniden deneyin.

### <a id="WasbAccountConfigNotFound"></a>WasbAccountConfigNotFound
* **Açıklama**: Geçersiz küme yapılandırması. Gerekli WASB hesap yapılandırması dış hesapları bulunamadı.  
* **Risk azaltma**: düzgün yapılandırmasında belirtilen hesabı var ve olduğunu doğrulayın ve işlemi yeniden deneyin.

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight üzerinde Tez işlerinin hatalarını ayıklamak için Ambari görünümlerini kullanma](../hdinsight-debug-ambari-tez-view.md)
* [Linux tabanlı HDInsight üzerinde Hadoop Hizmetleri için yığın dökümlerini etkinleştirme](../hdinsight-hadoop-collect-debug-heap-dump-linux.md)
* [HDInsight kümeleri Ambari Web kullanıcı arabirimini kullanarak yönetme](../hdinsight-hadoop-manage-ambari.md)
