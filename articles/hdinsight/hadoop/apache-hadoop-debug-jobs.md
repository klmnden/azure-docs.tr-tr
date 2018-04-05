---
title: "Hdınsight'ta Hadoop hata ayıklama: günlüklerini görüntülemek ve hata iletileri - Azure yorumlama | Microsoft Docs"
description: PowerShell kullanarak Hdınsight yönetirken alabilirsiniz hata iletileri ve kurtarmak için atabileceğiniz adımlar hakkında bilgi edinin.
services: hdinsight
tags: azure-portal
editor: cgronlun
manager: jhubbard
author: ashishthaps
documentationcenter: ''
ms.assetid: 7e6ceb0e-8be8-4911-bc80-20714030a3ad
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: ashish
ms.openlocfilehash: a5db3848eda2dbb6f117562e059b909575966993
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="analyze-hadoop-logs"></a>Hadoop günlüklerini analiz etme

Her Azure Hdınsight Hadoop kümesinde varsayılan dosya sistemi olarak kullanılan bir Azure depolama hesabı vardır. Depolama hesabı varsayılan depolama hesabı olarak adlandırılır. Küme günlüklerinin depolamak için varsayılan depolama hesabında Azure Table depolama ve Blob Depolama kullanır.  Kümeniz için varsayılan depolama hesabı öğrenmek için bkz: [Hdınsight'ta Hadoop yönetmek kümeleri](../hdinsight-administer-use-management-portal.md#find-the-default-storage-account). Küme bile silindikten sonra günlükleri depolama hesabında korur.

## <a name="logs-written-to-azure-tables"></a>Azure tablolara yazılan günlükleri

Azure tablolara yazılan günlükleri neler olduğuna dair bir Hdınsight kümesini içine Insight bir düzey sağlar.

Bir Hdınsight kümesi oluştururken, varsayılan Table storage Linux tabanlı kümelerde için altı tabloları otomatik olarak oluşturulur:

* hdinsightagentlog
* syslog
* daemonlog
* hadoopservicelog
* ambariserverlog
* ambariagentlog

Tablo dosya adları **u<ClusterName>DDMonYYYYatHHMMSSsss<TableName>**.

Bu tablolar aşağıdaki alanları içerir:

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
* TIMESTAMP
* TraceLevel

### <a name="tools-for-accessing-the-logs"></a>Günlükleri erişmek için Araçlar
Bu tablolardaki verileri erişmek için kullanılabilen birçok araç vardır:

* Visual Studio
* Azure Depolama Gezgini
* Excel için Power Query

#### <a name="use-power-query-for-excel"></a>Excel için Power Query kullanın
Power Query yüklenebilir [Excel için Microsoft Power Query](http://www.microsoft.com/en-us/download/details.aspx?id=39379). Sistem gereksinimleri için indirme sayfasına bakın.

**Açın ve hizmet günlüğü analiz etmek için Power Query kullanın**

1. Açık **Microsoft Excel**.
2. Gelen **Power Query** menüsünde tıklatın **Azure**ve ardından **From Microsoft Azure Table depolama**.
   
    ![Hdınsight Hadoop Excel PowerQuery Azure Table depolama açın](./media/apache-hadoop-debug-jobs/hdinsight-hadoop-analyze-logs-using-excel-power-query-open.png)
3. Depolama hesabı adı kısa adı veya FQDN girin.
4. Depolama hesabı anahtarı girin. Tabloların bir listesini göreceksiniz:
   
    ![Azure Table storage'da depolanan Hdınsight Hadoop günlükleri](./media/apache-hadoop-debug-jobs/hdinsight-hadoop-analyze-logs-table-names.png)
5. Hadoopservicelog tabloda sağ **Gezgini** bölmesinde ve select **Düzenle**. Dört sütun göreceksiniz. İsteğe bağlı olarak, Sil **bölüm anahtarı**, **satır anahtarını**, ve **zaman damgası** bunları seçtikten sonra tıklatarak sütunları **sütunları kaldırmak** Şerit bulunan seçenekler.
6. Excel elektronik tablosuna içeri aktarmak istediğiniz sütunları seçmek için içerik sütun genişletme simgesine tıklayın. Bu Tanıtım için TraceLevel ve ComponentName seçtiğim: Bu bana bileşenleri üzerinde sorunları olan bazı temel bilgileri verin.
   
    ![Hdınsight Hadoop günlükleri sütunları seçin](./media/apache-hadoop-debug-jobs/hdinsight-hadoop-analyze-logs-using-excel-power-query-filter.png)
7. Tıklatın **Tamam** verileri almak için.
8. Seçin **TraceLevel**, rol ve **ComponentName** sütunları ve ardından **Group By** Şeritte denetim.
9. Tıklatın **Tamam** Group By iletişim kutusunda
10. Uygulama tıklatın ** & Kapat **.

Excel şimdi filtrelemek ve sıralamak gerektiği için de kullanabilirsiniz. Sorunlar ortaya ancak seçerek ve yukarıda açıklanan sütunları gruplandırma Hadoop Hizmetleri ile neler olduğunu makul bir resim sağlanmıştır detaya için diğer sütunları (örneğin, ileti) eklemek isteyebilirsiniz. Aynı fikir setuplog ve hadoopinstalllog tablolara uygulanabilir.

#### <a name="use-visual-studio"></a>Visual Studio'yu kullanma
**Visual Studio'yu kullanma**

1. Visual Studio'yu açın.
2. Gelen **Görünüm** menüsünde tıklatın **Cloud Explorer**. Veya tıklamanız yeterlidir **CTRL +\, CTRL + X**.
3. Gelen **Cloud Explorer**seçin **kaynak türleri**.  Kullanılabilir bir seçenek **kaynak grupları**.
4. Genişletme **depolama hesapları**, kümeniz için varsayılan depolama hesabı ve ardından **tabloları**.
5. Çift **hadoopservicelog**.
6. Bir filtre ekleyin. Örneğin:
   
        TraceLevel eq 'ERROR'
   
    ![Hdınsight Hadoop günlükleri sütunları seçin](./media/apache-hadoop-debug-jobs/hdinsight-hadoop-analyze-logs-visual-studio-filter.png)
   
    Filtreleri oluşturma hakkında daha fazla bilgi için bkz: [Tablo Tasarımcısı için filtre dizelerini oluşturmak](../../vs-azure-tools-table-designer-construct-filter-strings.md).

## <a name="logs-written-to-azure-blob-storage"></a>Azure Blob depolama alanına yazılan günlükleri
[Azure tablolara yazılan günlükler](#log-written-to-azure-tables) bir düzeyde neler olduğuna dair bir Hdınsight kümesini içine Insight sağlar. Ancak, bu tablolar ayrıntılara yararlı olabilir görev düzeyi günlükleri sağlamaz ortaya çıkan sorunları daha. Hdınsight kümeleri, bu ileri düzeyde ayrıntı sağlamak için Blob Storage hesabınıza Templeton gönderilen herhangi bir iş için görev günlüklerini yazma yapılandırılır. Pratikte, bu Microsoft Azure PowerShell cmdlet'lerini veya .NET iş gönderme API'lerini RDP/komut-satırı erişimi kümeye üzerinden gönderilen işler kullanılarak gönderilen işler anlamına gelir. 

Günlükleri görüntülemek için bkz: [erişim YARN uygulama günlüklerini Hdınsight'ta Linux tabanlı](../hdinsight-hadoop-access-yarn-app-logs-linux.md).

Uygulama günlükleri hakkında daha fazla bilgi için bkz: [kullanıcı oturum yönetimi ve YARN erişimi basitleştirme](http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/).

## <a name="view-cluster-health-and-job-logs"></a>Küme durumu ve iş günlüklerine bakın
### <a name="access-the-ambari-ui"></a>Ambari UI erişim
Azure portalından, küme bölmesini açmak için bir Hdınsight küme adına tıklayın. Küme bölmesinden tıklatın **Pano**.

![Küme panosunu başlatma](./media/apache-hadoop-debug-jobs/hdi-debug-launch-dashboard.png)


### <a name="access-the-yarn-ui"></a>Erişim Yarn kullanıcı Arabirimi
Azure portalından, küme bölmesini açmak için bir Hdınsight küme adına tıklayın. Küme bölmesinden tıklatın **Pano**. İstendiğinde, Küme Yöneticisi kimlik bilgilerini girin. Ambari içinde seçin **YARN** soldaki Hizmetleri listesinden. Görüntülenen sayfasında seçin **hızlı bağlantılar**, kaynak yöneticisi kullanıcı Arabirimi ve etkin baş düğüm girişi seçin.

YARN kullanıcı Arabiriminde, aşağıdakileri yapmak için kullanabilirsiniz:

* **Küme durumunu Al**. Sol bölmeden genişletin **küme**, tıklatıp **hakkında**. Bu mevcut durumu ayrıntıları kullanıldığında, çekirdek bellek tahsis edilen toplam Küme Kaynak Yöneticisi, küme sürümü ve benzeri durumu gibi küme.
  
    ![Küme panosunu başlatma](./media/apache-hadoop-debug-jobs/hdi-debug-yarn-cluster-state.png)
* **Düğüm durumunu Al**. Sol bölmeden genişletin **küme**, tıklatıp **düğümleri**. Bu, kümedeki tüm düğümler, HTTP adresi her düğümün her düğüm, vb. için ayrılan kaynakları listeler.
* **İzleme iş durumu**. Sol bölmeden genişletin **küme**ve ardından **uygulamaları** kümedeki tüm işleri listelemek için. Belirli bir durumda (örneğin, yeni, gönderilen, çalışan, vb.) işleri bakmak istiyorsanız altındaki ilgili bağlantıyı tıklatarak **uygulamaları**. Daha fazla iş hakkında daha fazla çıkış, günlükleri, vb. dahil olmak üzere bu tür bulmak için işin adına tıklayabilirsiniz.

### <a name="access-the-hbase-ui"></a>HBase UI erişim
Azure portalından, küme bölmesini açmak için bir Hdınsight HBase küme adına tıklayın. Küme bölmesinden tıklatın **Pano**. İstendiğinde, Küme Yöneticisi kimlik bilgilerini girin. Ambari HBase hizmetler listesinden seçin. Seçin **hızlı bağlantılar** sayfanın üst kısmında etkin Zookeeper düğümü bağlantı noktası ve HBase ana UI'ı tıklatın.

## <a name="hdinsight-error-codes"></a>Hdınsight hata kodları
Bu bölümde listelenen hata iletileri Azure hdınsight'ta Hadoop kullanıcılarının Azure PowerShell kullanarak hizmet yönetirken karşılaşabilirsiniz olası hata koşulları anlamanıza yardımcı olması için ve atılabilecek adımlar bildirmek amacıyla sağlanır hatadan kurtarmak için.

Hdınsight kümeleri yönetmek için kullanıldığında bu hata iletileri bazıları aynı zamanda Azure portalında görülebilir. Ancak, karşılaşabileceğiniz diğer hata iletilerini vardır bu bağlamda olası düzeltici eylemler kısıtlamalar nedeniyle daha az ayrıntılı. Diğer hata iletilerini azaltma belirgin olduğu bağlamlarda sağlanır. 

### <a id="AtleastOneSqlMetastoreMustBeProvided"></a>AtleastOneSqlMetastoreMustBeProvided
* **Açıklama**: Hive ve Oozie meta deponuz için özel ayarları kullanmak için lütfen en az bir bileşen için Azure SQL veritabanı ayrıntıları sağlayın.
* **Azaltma**: kullanıcının geçerli bir SQL Azure meta depo sağlayın ve isteği yeniden deneyin gerekmez.  

### <a id="AzureRegionNotSupported"></a>AzureRegionNotSupported
* **Açıklama**: küme bölgede oluşturulamadı *nameOfYourRegion*. Geçerli bir Hdınsight bölgesi kullanın ve isteği yeniden deneyin.
* **Azaltma**: Müşteri şu anda bunları destekleyen küme bölge oluşturmanız: Güneydoğu Asya, Batı Avrupa, Kuzey Avrupa, Doğu ABD veya Batı ABD.  

### <a id="ClusterContainerRecordNotFound"></a>ClusterContainerRecordNotFound
* **Açıklama**: sunucu istenen küme kaydı bulunamadı.  
* **Azaltma**: işlemi yeniden deneyin.

### <a id="ClusterDnsNameInvalidReservedWord"></a>ClusterDnsNameInvalidReservedWord
* **Açıklama**: küme DNS adına *yourDnsName* geçersiz. Lütfen adı başlatır ve alfasayısal biten emin olun ve yalnızca içerebilir '-' özel karakter  
* **Azaltma**: başlatır ve alfasayısal biten ve hiçbir özel içeren kümenizi tire dışındaki karakterleri için geçerli bir DNS adı kullanılan olduğundan emin olun '-' ve işlemi yeniden deneyin.

### <a id="ClusterNameUnavailable"></a>ClusterNameUnavailable
* **Açıklama**: küme adı *yourClusterName* kullanılamıyor. Lütfen başka bir ad seçin.  
* **Azaltma**: kullanıcı benzersiz olan bir clustername ve değil mevcut belirtip yeniden deneyin. Kullanıcı Portalı'nı kullanarak, kullanıcı Arabirimi oluşturma adımları sırasında bir küme adı zaten kullanılıyorsa, bunları size bildirir.

### <a id="ClusterPasswordInvalid"></a>ClusterPasswordInvalid
* **Açıklama**: küme parola geçersiz. Parola en az 10 karakter uzunluğunda olmalıdır ve en az bir rakam, büyük harf, küçük harf ve boşluksuz özel karakter içermelidir ve bunun bir parçası olarak kullanıcı adı içermemesi gerekir.  
* **Azaltma**: geçerli küme parola sağlayın ve işlemi yeniden deneyin.

### <a id="ClusterUserNameInvalid"></a>ClusterUserNameInvalid
* **Açıklama**: küme kullanıcı adı geçersiz. Lütfen kullanıcı adı özel karakter veya boşluk içermeyen emin olun.  
* **Azaltma**: geçerli küme kullanıcı adını belirtin ve işlemi yeniden deneyin.

### <a id="ClusterUserNameInvalidReservedWord"></a>ClusterUserNameInvalidReservedWord
* **Açıklama**: küme DNS adına *yourDnsClusterName* geçersiz. Lütfen adı başlatır ve alfasayısal biten emin olun ve yalnızca içerebilir '-' özel karakter  
* **Azaltma**: geçerli bir DNS küme kullanıcı adı sağlayın ve işlemi yeniden deneyin.

### <a id="ContainerNameMisMatchWithDnsName"></a>ContainerNameMisMatchWithDnsName
* **Açıklama**: kapsayıcı adı URI *yourcontainerURI* ve DNS adı *yourDnsName* istek gövdesi aynı olmalıdır.  
* **Azaltma**: emin olun, kapsayıcı adı ve DNS adı aynı olan ve işlemi yeniden deneyin.

### <a id="DataNodeDefinitionNotFound"></a>DataNodeDefinitionNotFound
* **Açıklama**: Geçersiz küme yapılandırması. Tüm veri düğümü tanımlarını düğüm boyutu bulunamıyor.  
* **Azaltma**: işlemi yeniden deneyin.

### <a id="DeploymentDeletionFailure"></a>DeploymentDeletionFailure
* **Açıklama**: dağıtımını silme işlemi başarısız oldu küme için  
* **Azaltma**: silme işlemi yeniden deneyin.

### <a id="DnsMappingNotFound"></a>DnsMappingNotFound
* **Açıklama**: hizmet yapılandırma hatası. Gerekli DNS eşlemesi bilgileri bulunamadı.  
* **Azaltma**: küme silip yeni bir küme oluşturun.

### <a id="DuplicateClusterContainerRequest"></a>DuplicateClusterContainerRequest
* **Açıklama**: Yinelenen küme kapsayıcı oluşturma girişimi. Kayıt mevcut *nameOfYourContainer* ancak Etag'ler eşleşmiyor.
* **Azaltma**: kapsayıcı için benzersiz bir ad sağlayın ve oluşturma işlemi yeniden deneyin.

### <a id="DuplicateClusterInHostedService"></a>DuplicateClusterInHostedService
* **Açıklama**: barındırılan hizmeti *nameOfYourHostedService* bir küme zaten içeriyor. Barındırılan bir hizmet birden çok küme içeremez.  
* **Azaltma**: başka bir barındırılan hizmet kümesinde ana bilgisayar.

### <a id="FailureToUpdateDeploymentStatus"></a>FailureToUpdateDeploymentStatus
* **Açıklama**: sunucunun küme dağıtım durumunu güncelleştirilemedi.  
* **Azaltma**: işlemi yeniden deneyin. Birden çok kez aşması durumunda, CSS ile iletişime geçin.

### <a id="HdiRestoreClusterAltered"></a>HdiRestoreClusterAltered
* **Açıklama**: küme *yourClusterName* bakım bir parçası olarak silindi. Lütfen küme oluşturun.
* **Azaltma**: kümeyi yeniden oluşturun.

### <a id="HeadNodeConfigNotFound"></a>HeadNodeConfigNotFound
* **Açıklama**: Geçersiz küme yapılandırması. Baş düğüm yapılandırması düğümü boyutları bulunamadı gereklidir.
* **Azaltma**: işlemi yeniden deneyin.

### <a id="HostedServiceCreationFailure"></a>HostedServiceCreationFailure
* **Açıklama**: barındırılan hizmet oluşturulamıyor *nameOfYourHostedService*. Lütfen isteği yeniden deneyin.  
* **Azaltma**: isteği yeniden deneyin.

### <a id="HostedServiceHasProductionDeployment"></a>HostedServiceHasProductionDeployment
* **Açıklama**: barındırılan hizmeti *nameOfYourHostedService* Üretim dağıtımı zaten sahip. Barındırılan bir hizmet birden çok üretim dağıtımları içeremez. Farklı bir küme adı isteği yeniden deneyin.
* **Azaltma**: farklı bir küme adı kullanın ve isteği yeniden deneyin.

### <a id="HostedServiceNotFound"></a>HostedServiceNotFound
* **Açıklama**: barındırılan hizmeti *nameOfYourHostedService* için küme bulunamadı.  
* **Azaltma**: küme hata durumunda ise, onu silin ve yeniden deneyin.

### <a id="HostedServiceWithNoDeployment"></a>HostedServiceWithNoDeployment
* **Açıklama**: barındırılan hizmeti *nameOfYourHostedService* ilişkili dağıtım sahiptir.  
* **Azaltma**: küme hata durumunda ise, onu silin ve yeniden deneyin.

### <a id="InsufficientResourcesCores"></a>InsufficientResourcesCores
* **Açıklama**: Subscriptionıd *yourSubscriptionId* küme oluşturmak için sol çekirdek yok *yourClusterName*. Gerekli: *resourcesRequired*, kullanılabilir: *resourcesAvailable*.  
* **Azaltma**: kaynakları aboneliğinizde açmak veya abonelik için kullanılabilir kaynakları artırmak ve küme yeniden oluşturmayı deneyin.

### <a id="InsufficientResourcesHostedServices"></a>InsufficientResourcesHostedServices
* **Açıklama**: abonelik kimliği *yourSubscriptionId* küme oluşturmak yeni bir HostedService için kota yok *yourClusterName*.  
* **Azaltma**: kaynakları aboneliğinizde açmak veya abonelik için kullanılabilir kaynakları artırmak ve küme yeniden oluşturmayı deneyin.

### <a id="InternalErrorRetryRequest"></a>InternalErrorRetryRequest
* **Açıklama**: sunucu bir iç hatayla karşılaştı. Lütfen isteği yeniden deneyin.  
* **Azaltma**: isteği yeniden deneyin.

### <a id="InvalidAzureStorageLocation"></a>InvalidAzureStorageLocation
* **Açıklama**: Azure depolama konumu *dataRegionName* geçerli bir konum değil. Bölge doğru olduğundan emin olun ve isteği yeniden deneyin.
* **Azaltma**: Hdınsight destekleyen bir depolama konumu seçin, kümenizi birlikte bulunan olup olmadığını denetleyin ve işlemi yeniden deneyin.

### <a id="InvalidNodeSizeForDataNode"></a>InvalidNodeSizeForDataNode
* **Açıklama**: veri düğümleri için geçersiz VM boyutu. Yalnızca 'Büyük VM' boyutu tüm veri düğümleri için desteklenir.  
* **Azaltma**: veri düğümü için desteklenen düğüm boyutu belirtin ve işlemi yeniden deneyin.

### <a id="InvalidNodeSizeForHeadNode"></a>InvalidNodeSizeForHeadNode
* **Açıklama**: baş düğüm için geçersiz VM boyutu. Yalnızca 'ExtraLarge VM' boyutu baş düğüm için desteklenir.  
* **Azaltma**: baş düğüm için desteklenen düğüm boyutu belirtin ve işlemi yeniden deneyin

### <a id="InvalidRightsForDeploymentDeletion"></a>InvalidRightsForDeploymentDeletion
* **Açıklama**: abonelik kimliği *yourSubscriptionId* kullanılan küme için silme işlemi yürütmek için yeterli izinleri yok *yourClusterName*.  
* **Azaltma**: küme hata durumunda ise, sürükleyip bırakın ve işlemi yeniden deneyin.  

### <a id="InvalidStorageAccountBlobContainerName"></a>InvalidStorageAccountBlobContainerName
* **Açıklama**: dış depolama hesabı blob kapsayıcı adı *yourContainerName* geçersiz. Ad bir harfle başlayan ve yalnızca küçük harf, sayı ve tire içerdiğinden emin olun.  
* **Azaltma**: geçerli bir depolama hesabı blob kapsayıcı adı belirtin ve işlemi yeniden deneyin.

### <a id="InvalidStorageAccountConfigurationSecretKey"></a>InvalidStorageAccountConfigurationSecretKey
* **Açıklama**: dış depolama hesabı için yapılandırması *yourStorageAccountName* ayarlanacak gizli anahtarı ayrıntıları olması gerekir.  
* **Azaltma**: depolama hesabı için geçerli bir gizli anahtarı belirtin ve işlemi yeniden deneyin.

### <a id="InvalidVersionHeaderFormat"></a>InvalidVersionHeaderFormat
* **Açıklama**: sürüm üst bilgisi *yourVersionHeader* geçerli yyyy-aa-gg biçiminde değil  
* **Azaltma**: sürüm üst bilgisi için geçerli bir biçim belirtin ve isteği yeniden deneyin.

### <a id="MoreThanOneHeadNode"></a>MoreThanOneHeadNode
* **Açıklama**: Geçersiz küme yapılandırması. Birden fazla baş düğüm yapılandırması bulunamadı.  
* **Azaltma**: yalnızca tek bir baş düğüm belirtilen şekilde yapılandırmasını düzenleyin.

### <a id="OperationTimedOutRetryRequest"></a>OperationTimedOutRetryRequest
* **Açıklama**: olası en fazla yeniden deneme girişimleri ya da işlemi izin verilen sürede tamamlanamadı. Lütfen isteği yeniden deneyin.  
* **Azaltma**: isteği yeniden deneyin.

### <a id="ParameterNullOrEmpty"></a>ParameterNullOrEmpty
* **Açıklama**: parametre *yourParameterName* null veya boş olamaz.  
* **Azaltma**: parametresi için geçerli bir değer belirtin.

### <a id="PreClusterCreationValidationFailure"></a>PreClusterCreationValidationFailure
* **Açıklama**: bir veya daha fazla küme oluşturma isteği girdi geçerli değil. Giriş değerleri doğru olduğundan ve isteği yeniden deneyin emin olun.  
* **Azaltma**: Giriş değerleri doğru olduğundan ve isteği yeniden deneyin emin olun.

### <a id="RegionCapabilityNotAvailable"></a>RegionCapabilityNotAvailable
* **Açıklama**: bölge yeteneği değil bölgeniz için *yourRegionName* ve abonelik kimliği *yourSubscriptionId*.  
* **Azaltma**: Hdınsight kümeleri destekleyen bir bölge belirtin. Genel olarak desteklenen bölgeler: Güneydoğu Asya, Batı Avrupa, Kuzey Avrupa, Doğu ABD veya Batı ABD.

### <a id="StorageAccountNotColocated"></a>StorageAccountNotColocated
* **Açıklama**: depolama hesabı *yourStorageAccountName* bölgede *currentRegionName*. Küme bölge aynı olmalıdır *yourClusterRegionName*.  
* **Azaltma**: kümeniz bulunduğu aynı bölgede bulunan bir depolama hesabı belirtin veya verilerinizi depolama hesabında zaten varsa, mevcut depolama hesabı ile aynı bölgede yeni bir küme oluşturun. Kullanıcı arabirimini portalı kullanıyorsanız, bunları bu sorunun önceden bildirir.

### <a id="SubscriptionIdNotActive"></a>SubscriptionIdNotActive
* **Açıklama**: verilen abonelik kimliği *yourSubscriptionId* etkin değil.  
* **Azaltma**: aboneliğinizi yeniden etkinleştirmek ya da yeni bir geçerli abonelik edinin.

### <a id="SubscriptionIdNotFound"></a>SubscriptionIdNotFound
* **Açıklama**: abonelik kimliği *yourSubscriptionId* bulunamadı.  
* **Azaltma**: Abonelik Kimliğinizin geçerli olduğundan emin olun ve işlemi yeniden deneyin.

### <a id="UnableToResolveDNS"></a>UnableToResolveDNS
* **Açıklama**: DNS çözümlenemiyor *yourDnsUrl*. Lütfen blob uç için tam bir URL sağladığınızdan emin olun.  
* **Azaltma**: geçerli blob URL'si sağlayın. URL olmalıdır başlayarak dahil tam olarak geçerli *http://* ve biten *.com*.

### <a id="UnableToVerifyLocationOfResource"></a>UnableToVerifyLocationOfResource
* **Açıklama**: kaynak konumunu doğrulanamıyor *yourDnsUrl*. Lütfen blob uç için tam bir URL sağladığınızdan emin olun.  
* **Azaltma**: geçerli blob URL'si sağlayın. URL olmalıdır başlayarak dahil tam olarak geçerli *http://* ve biten *.com*.

### <a id="VersionCapabilityNotAvailable"></a>VersionCapabilityNotAvailable
* **Açıklama**: sürüm yetenek sürümü için mevcut değil *specifiedVersion* ve abonelik kimliği *yourSubscriptionId*.  
* **Azaltma**: kullanılabilir bir sürüm seçin ve işlemi yeniden deneyin.

### <a id="VersionNotSupported"></a>VersionNotSupported
* **Açıklama**: sürüm *specifiedVersion* desteklenmiyor.
* **Azaltma**: desteklenen bir sürümünü seçin ve işlemi yeniden deneyin.

### <a id="VersionNotSupportedInRegion"></a>VersionNotSupportedInRegion
* **Açıklama**: sürüm *specifiedVersion* Azure bölgesinde kullanılamıyor *specifiedRegion*.  
* **Azaltma**: Belirtilen bölgede desteklenen bir sürüm seçin ve işlemi yeniden deneyin.

### <a id="WasbAccountConfigNotFound"></a>WasbAccountConfigNotFound
* **Açıklama**: Geçersiz küme yapılandırması. Gerekli WASB hesabı yapılandırması dış hesaplarında bulunamadı.  
* **Azaltma**: düzgün yapılandırma dosyasında belirtilen hesabı var ve olduğundan emin olun ve işlemi yeniden deneyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Tez işlerinde hdınsight'ta hata ayıklamak için Ambari görünümleri kullanma](../hdinsight-debug-ambari-tez-view.md)
* [Linux tabanlı hdınsight'ta Hadoop Hizmetleri için yığın dökümleri etkinleştir](../hdinsight-hadoop-collect-debug-heap-dump-linux.md)
* [Ambari Web kullanıcı arabirimini kullanarak Hdınsight kümelerini yönetme](../hdinsight-hadoop-manage-ambari.md)
