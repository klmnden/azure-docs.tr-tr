---
title: Azure kaynakları için yeni abonelik veya kaynak grubu taşıma | Microsoft Docs
description: Kaynakları yeni kaynak grubuna veya aboneliğe taşıma için Azure Resource Manager'ı kullanın.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 05/17/2019
ms.author: tomfitz
ms.openlocfilehash: 1ae1afe103d4c52a2a7d921ef4f34dc030f3c6f7
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65872633"
---
# <a name="move-resources-to-new-resource-group-or-subscription"></a>Kaynakları yeni kaynak grubuna veya aboneliğe taşıma

Bu makalede, başka bir Azure aboneliğine veya başka bir kaynak grubuna aynı abonelik altında Azure kaynakları taşıma işlemini göstermektedir. Kaynakları taşıma için Azure portalı, Azure PowerShell, Azure CLI veya REST API'yi kullanabilirsiniz.

Kaynak grubu hem de hedef grubu taşıma işlemi sırasında kilitlenir. Yazma ve silme işlemleri taşıma işlemi tamamlanana kadar kaynak gruplarında engellenir. Bu kilit ekleyemez, güncelleştirme veya kaynak gruplarındaki kaynakları silin, ancak kaynakları dondurulmuş gelmez anlamına gelir. Örneğin, bir SQL Server ve veritabanı yeni bir kaynak grubuna taşırsanız, veritabanı kullanan bir uygulama kapalı kalma süresi olmadan karşılaşır. Bunu hala okuyabilir ve veritabanına yazma.

Bir kaynak taşıma yalnızca bu yeni bir kaynak grubuna taşınır. Taşıma işlemi, kaynağın konumunu değiştirmez. Yeni kaynak grubu farklı bir konuma sahip olabilir, ancak, kaynak konumunu değiştirmez.

> [!NOTE]
> Bu makalede, kaynakları var olan Azure abonelikler arasında taşıma açıklar. Gerçekte Azure aboneliğiniz (örneğin, boş, Kullandıkça Öde aboneliğine geçiş) yükseltmek istiyorsanız, aboneliğinizin dönüştürmeniz gerekir.
> * Ücretsiz deneme sürümü yükseltmek için bkz: [ücretsiz deneme sürümü ya da Microsoft Imagine Azure aboneliğinizi Kullandıkça Öde aboneliğine yükseltme](..//billing/billing-upgrade-azure-subscription.md).
> * Bir Kullandıkça Öde hesabına değiştirmek için bkz [Azure Kullandıkça Öde aboneliğinizi değiştirmek için farklı bir teklif](../billing/billing-how-to-switch-azure-offer.md).
> * Abonelik dönüştüremezse [bir Azure destek isteği oluşturma](../azure-supportability/how-to-create-azure-support-request.md). Seçin **abonelik yönetimi** sorun türü için.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="when-to-call-azure-support"></a>Azure destek çağrısı yapıldığında

Bu makalede gösterilen Self-Servis işlemler çoğu kaynaklarında taşıyabilirsiniz. Self Servis işlemleri için kullanın:

* Resource Manager kaynaklarını taşıma
* Şunlara göre Klasik kaynakları taşıma [Klasik dağıtım sınırlamalarına](#classic-deployment-limitations).

İlgili kişi [Destek](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) gerektiğinde:

* Kaynaklarınızı bir yeni bir Azure hesabı (ve Azure Active Directory kiracısına) taşımak ve önceki bölümde yönergeleri konusunda Yardım gerekiyor.
* Klasik kaynakları taşıma ancak kısıtlamalarla sorun yaşıyoruz.

## <a name="services-that-can-be-moved"></a>Taşınabilir Hizmetleri

Aşağıdaki listede, bir yeni kaynak grubu ve abonelik taşınabilir Azure hizmetlerinin genel bir özeti verilmiştir. Hangi kaynak türlerini destekleyen taşıma bir listesi için bkz [taşıma işlemi Destek kaynakları için](move-support-resources.md).

* Analysis Services
* API Management
* App Service uygulamaları (web uygulamaları) - bkz [App Service kısıtlamaları](#app-service-limitations)
* App Service sertifikaları - bkz [App Service sertifikası sınırlamaları](#app-service-certificate-limitations)
* Otomasyon - runbook'ları, Otomasyon hesabı aynı kaynak grubunda bulunmalıdır.
* Azure Active Directory B2C
* Azure önbelleği için Redis - sanal ağ sayesinde, örnek Azure Cache Redis örneği için yapılandırılmışsa, farklı bir aboneliğe taşınamaz. Bkz: [sanal ağlar sınırlamaları](#virtual-networks-limitations).
* Azure Cosmos DB
* Azure Veri Gezgini
* MariaDB için Azure Veritabanı
* MySQL için Azure Veritabanı
* PostgreSQL için Azure Veritabanı
* Azure DevOps - için adımları [faturalandırma için kullanılan Azure aboneliğini değiştirmeniz](/azure/devops/organizations/billing/change-azure-subscription?view=azure-devops).
* Azure Haritalar
* Azure İzleyici günlükleri
* Azure Geçişi
* Azure Stack - kayıtları
* Batch
* BizTalk Services
* Bot Hizmeti
* CDN
* Bulut Hizmetleri - bkz [Klasik dağıtım sınırlamalarını](#classic-deployment-limitations)
* Bilişsel Hizmetler
* Container Registry
* Content Moderator
* Maliyet Yönetimi
* Customer Insights
* Veri Kataloğu
* Data Factory
* Data Lake Analytics
* Data Lake Store
* DNS
* Event Grid
* Event Hubs
* Ön kapısı
* Bkz: HDInsight kümeleri - [HDInsight sınırlamaları](#hdinsight-limitations)
* IoT Central
* IoT Hub
* Anahtar kasası - Key Vault disk şifreleme için kullanılan kaynak gruplarına aynı abonelikte veya abonelikler arasında taşınamaz.
* Yük Dengeleyiciler - temel SKU yük Dengeleyicide taşınabilir. Standart SKU yük Dengeleyicide taşınamaz.
* Logic Apps
* Machine Learning - Machine Learning Studio web hizmetleri aynı abonelikte ancak farklı bir abonelikte bir kaynak grubuna taşındı. Diğer Machine Learning kaynakları abonelikler arasında taşınabilir.
* Yönetilen diskler - yönetilen diskler kullanılabilirlik alanlarında, farklı bir aboneliğe taşınamaz
* Yönetilen kimlik - kullanıcı tarafından atanan
* Medya Hizmetleri
* İzleyici - değil emin olmak için yeni abonelik taşıma yapma aşan [abonelik kotaları](../azure-subscription-service-limits.md#monitor-limits)
* Notification Hubs
* Operasyonel İçgörüler
* Operations Management
* Portalı panoları
* Power BI - hem Power BI Embedded ve Power BI çalışma alanı koleksiyonu
* Genel IP - temel SKU ortak IP'sine taşınabilir. Standart SKU ortak IP'sine taşınamaz.
* Kurtarma Hizmetleri kasası - kaydolun bir [Önizleme](#recovery-services-limitations).
* Azure'da SAP HANA
* Scheduler
* Arama - tek bir işlemde farklı bölgelerdeki birden çok arama kaynaklar taşınamıyor. Bunun yerine, bunları ayrı işlemlerde taşıyın.
* Service Bus
* Service Fabric
* Service Fabric Mesh
* SignalR Service
* Depolama - depolama hesapları farklı bölgelerde, aynı işlem içinde taşınamaz. Bunun yerine, her bölge için ayrı işlem kullanın.
* Depolama alanı (Klasik) - [Klasik dağıtım sınırlamalarını](#classic-deployment-limitations)
* Depolama Eşitleme Hizmeti
* Stream Analytics - Stream Analytics işleri çalıştırırken buna taşınamaz durumu.
* SQL veritabanı sunucusu - veritabanı ve sunucu, aynı kaynak grubunda olmalıdır. Bir SQL server taşıdığınızda, tüm veritabanlarını da taşınır. Bu davranış, Azure SQL veritabanı ve Azure SQL veri ambarı veritabanları için geçerlidir.
* Time Series Insights
* Traffic Manager
* Sanal makineler - bkz [sanal makineler sınırlamaları](#virtual-machines-limitations)
* Sanal makineler (Klasik) - bkz [Klasik dağıtım sınırlamalarını](#classic-deployment-limitations)
* Sanal makine ölçek kümeleri - bkz [sanal makineler sınırlamaları](#virtual-machines-limitations)
* Sanal ağlar - bkz [sanal ağlar sınırlamaları](#virtual-networks-limitations)
* VPN Gateway

### <a name="services-that-cannot-be-moved"></a>Taşınamayan Hizmetleri

Aşağıdaki listede, bir yeni kaynak grubu ve abonelik taşınamaz Azure hizmetlerinin genel bir özeti verilmiştir. Daha fazla ayrıntı için bkz. [taşıma işlemi Destek kaynakları için](move-support-resources.md).

* AD etki alanı Hizmetleri
* AD karma sistem durumu hizmeti
* Application Gateway
* Azure veritabanı geçişi
* Azure Databricks
* Azure Güvenlik Duvarı
* Azure Kubernetes Hizmeti (AKS)
* Azure Geçişi
* Azure NetApp Files
* Sertifikalar - App Service sertifikaları taşınabilir, ancak karşıya yüklenen Sertifikalar [sınırlamaları](#app-service-limitations).
* Klasik uygulamaları
* Kapsayıcı Örnekleri
* Kapsayıcı Hizmeti
* Data Box
* Geliştirme alanları
* Dynamics LCS
* ExpressRoute
* Lab Services'i - sınıf Laboratuvarlarını bir yeni kaynak grubuna veya aboneliğe taşınamaz. DevTest Labs, yeni bir kaynak grubu ile aynı abonelikte ancak değil, abonelikler arasında taşınabilir.
* Yönetilen Uygulamalar
* Microsoft Genomiks
* Güvenlik
* Site Recovery
* StorSimple cihaz Yöneticisi
* Sanal ağlar (Klasik) - bkz [Klasik dağıtım sınırlamalarını](#classic-deployment-limitations)

## <a name="limitations"></a>Sınırlamalar

Kaynakları taşıma için karmaşık senaryoları nasıl ele alınacağını açıklamalarını bölüm sağlar. Sınırlamalar vardır:

* [Sanal makineler sınırlamaları](#virtual-machines-limitations)
* [Sanal ağlar sınırlamaları](#virtual-networks-limitations)
* [App Service kısıtlamaları](#app-service-limitations)
* [App Service sertifikası sınırlamaları](#app-service-certificate-limitations)
* [Klasik dağıtım sınırlamalarına](#classic-deployment-limitations)
* [Kurtarma Hizmetleri sınırlamalarını](#recovery-services-limitations)
* [HDInsight sınırlamaları](#hdinsight-limitations)

### <a name="virtual-machines-limitations"></a>Sanal makineler sınırlamaları

Yönetilen diskler, yönetilen görüntüleri, yönetilen anlık görüntüler ve kullanılabilirlik kümeleri ile sanal makineler yönetilen diskleri kullanan sanal makineleri ile taşıyabilirsiniz. Kullanılabilirlik alanına yönetilen diskler, farklı bir aboneliğe taşınamaz.

Henüz, aşağıdaki senaryolar desteklenmez:

* Key Vault'ta depolanan bir sertifika ile sanal makineler için yeni bir kaynak grubu ile aynı abonelikte ancak değil, abonelikler arasında taşınabilir.
* Standart SKU yük Dengeleyicide veya standart SKU genel IP ile sanal makine ölçek kümeleri taşınamaz.
* Market kaynaklardan bağlı planlar ile oluşturulan sanal makineler, kaynak grubu veya abonelik arasında taşınamaz. Geçerli Abonelikteki sanal makine sağlamasını kaldırma ve yeni aboneliği yeniden dağıtın.

Azure Backup ile yapılandırılmış sanal makineleri taşımak için aşağıdaki geçici çözümü kullanın:

* Sanal makinenizin konumu bulun.
* Aşağıdaki adlandırma deseni ile bir kaynak grup bulunamıyor: `AzureBackupRG_<location of your VM>_1` örneğin AzureBackupRG_westus2_1
* Azure portalında, ardından onay "gizli türleri Göster ise"
* PowerShell'de varsa, kullanmak `Get-AzResource -ResourceGroupName AzureBackupRG_<location of your VM>_1` cmdlet'i
* CLI, kullanın `az resource list -g AzureBackupRG_<location of your VM>_1`
* Kaynak türü bulma `Microsoft.Compute/restorePointCollections` adlandırma deseni sahip `AzureBackup_<name of your VM that you're trying to move>_###########`
* Bu kaynağı silme. Bu işlem, yedeklenen verileri kasadaki değil yalnızca anında kurtarma noktalarını siler.
* Silme işlemi tamamlandıktan sonra sanal makineyi taşımak mümkün olacaktır. Hedef aboneliği için sanal makine ve kasa taşıyabilirsiniz. Taşıma sonrası veri kaybı olmadan yedeklemelerde devam edebilirsiniz.
* Taşıma kurtarma hizmeti kasası yedekleme hakkında daha fazla bilgi için bkz. [kurtarma Hizmetleri sınırlamalarını](#recovery-services-limitations).

### <a name="virtual-networks-limitations"></a>Sanal ağlar sınırlamaları

Bir sanal ağ taşırken, bağımlı kaynaklarını da taşımanız gerekir. VPN ağ geçitleri için IP adresleri, sanal ağ geçitleri ve tüm ilişkili bağlantı kaynakları taşımanız gerekir. Yerel ağ geçitleri farklı kaynak grubunda olabilir.

Bir ağ arabirimi kartı ile bir sanal makineyi taşımak için tüm bağımlı kaynaklarla taşımanız gerekir. Sanal ağ için ağ arabirimi kartını sanal ağ ve VPN ağ geçitleri için tüm diğer ağ arabirim kartları taşımalısınız.

Eşlenmiş sanal ağın taşımak için önce sanal ağ eşlemesi devre dışı bırakmanız gerekir. Devre dışı sonra sanal ağ taşıyabilirsiniz. Taşıma sonrasında, sanal ağ eşlemesi yeniden etkinleştirin.

Bir alt ağ kaynak gezinti bağlantılarının bulunduğu sanal ağı içeriyorsa bir sanal ağ başka bir aboneliğe taşınamıyor. Örneğin, bir Azure önbelleği için Redis kaynak bir alt ağa dağıtılırsa, bu alt ağ kaynak Gezinti bağlantısı vardır.

### <a name="app-service-limitations"></a>App Service kısıtlamaları

App Service kaynaklarını taşımak için sınırlamalar kaynakları bir abonelik içinde veya yeni bir aboneliğe taşıma bağlı olarak farklılık gösterir. Web uygulamanızı bir App Service sertifikası kullanıyorsa, bkz. [App Service sertifikası sınırlamaları](#app-service-certificate-limitations)

#### <a name="moving-within-the-same-subscription"></a>Aynı abonelik içinde taşıma

Bir Web uygulaması taşınırken _aynı abonelik içindeki_, üçüncü taraf SSL sertifikaları taşınamıyor. Ancak, üçüncü taraf sertifikasını taşımadan bir Web uygulaması yeni kaynak grubuna taşıyabilirsiniz ve uygulamanızın SSL işlevselliği yine de çalışır.

SSL sertifikası ile Web uygulaması taşımak istiyorsanız, şu adımları izleyin:

1. Web uygulamasından üçüncü taraf sertifika silin, ancak sertifikanızın birer kopya bulundurun
2. Web uygulaması taşıyın.
3. Üçüncü taraf sertifika taşınan Web uygulamasına yükleyin.

#### <a name="moving-across-subscriptions"></a>Abonelikler arasında taşıma

Bir Web uygulaması taşınırken _abonelikler arasında_, aşağıdaki sınırlamalar geçerlidir:

- Hedef kaynak grubu mevcut App Service kaynaklar olmaması gerekir. App Service kaynakları şunlardır:
    - Web Apps
    - App Service planları
    - Karşıya yüklenen veya içeri aktarılan SSL sertifikaları
    - App Service ortamları
- Kaynak grubundaki tüm App Service kaynakların birlikte taşınması gerekir.
- App Service kaynaklarını, bunlar ilk olarak oluşturulduğu kaynak grubunun yalnızca taşınabilir. Bir App Service kaynak artık özgün kaynak grubunda değilse, bunu geri özgün kaynak grubunda ilk taşınmalıdır ve ardından abonelikler arasında taşınabilir.

Özgün kaynak grubunu hatırlamıyorsanız tanılama bulabilirsiniz. Web uygulamanızı seçin **tanılayın ve sorunlarını çözmek**. Ardından, **yapılandırma ve Yönetim**.

![Tanılama seçin](./media/resource-group-move-resources/select-diagnostics.png)

Seçin **geçiş seçenekleri**.

![Geçiş seçenekleri seçin](./media/resource-group-move-resources/select-migration.png)

Web uygulaması taşımak için önerilen adımlar seçeneğini belirleyin.

![Önerilen adımları seçin](./media/resource-group-move-resources/recommended-steps.png)

Kaynakları taşımadan önce gerçekleştirilecek önerilen eylemler görürsünüz. Web uygulaması için orijinal kaynak grubu bilgileri içerir.

![Öneriler](./media/resource-group-move-resources/recommendations.png)

### <a name="app-service-certificate-limitations"></a>App Service sertifikası sınırlamaları

App Service sertifikanız bir yeni kaynak grubuna veya aboneliğe taşıyabilirsiniz. App Service sertifikanız bir web uygulaması ile ilişkili ise, yeni bir abonelik için kaynakları taşımadan önce bazı adımları atmanız gerekir. Özel sertifika ve SSL bağlaması kaynakları taşımadan önce web uygulamasını silin. App Service sertifikası için silinmesi gereken değil yalnızca web App'te özel sertifika.

### <a name="classic-deployment-limitations"></a>Klasik dağıtım sınırlamalarına

Klasik modelle dağıtılmış kaynakları taşımak için seçenek kaynakları bir abonelik içinde veya yeni bir aboneliğe taşıma bağlı olarak farklılık gösterir.

#### <a name="same-subscription"></a>Aynı abonelik

Aynı abonelik içindeki başka bir kaynak grubu için bir kaynak grubundan kaynakları taşırken aşağıdaki kısıtlamalar uygulanır:

* Sanal ağlar (Klasik) taşınamaz.
* Sanal makineler (Klasik) bulut hizmeti ile taşınmalıdır.
* Bulut hizmeti, tüm sanal makineleri taşıma içerdiğinde yalnızca taşınabilir.
* Bir kerede yalnızca bir bulut hizmeti taşınabilir.
* Bir kerede yalnızca bir depolama hesabı (Klasik) taşınabilir.
* Depolama hesabı (Klasik), bir sanal makine veya Bulut hizmeti ile aynı işlemde taşınamaz.

Klasik kaynakları aynı abonelik içindeki yeni bir kaynak grubuna taşımak için aracılığıyla standart taşıma işlemlerini kullanın. [portalı](#use-portal), Azure PowerShell, Azure CLI veya REST API. Resource Manager kaynaklarını taşımak için kullandığınız gibi işlemlerin aynısını kullanın.

#### <a name="new-subscription"></a>Yeni abonelik

Kaynakları yeni bir aboneliğe taşınmasını, aşağıdaki kısıtlamalar uygulanır:

* Abonelikteki tüm Klasik kaynaklar aynı işlem içinde taşınmalıdır.
* Hedef aboneliği diğer Klasik kaynaklar olmaması gerekir.
* Taşıma, yalnızca klasik taşıma için ayrı bir REST API aracılığıyla istenebilir. Klasik kaynakları için yeni bir abonelik taşırken standart Resource Manager'a taşıma komutlar çalışmaz.

Klasik kaynakları için yeni bir aboneliği taşımak, Klasik kaynakları için özel REST işlemlerini kullanın. REST kullanmak için aşağıdaki adımları uygulayın:

1. Kaynak abonelik bir çapraz abonelik taşıma katılabilir, kontrol edin. Aşağıdaki işlemi kullanın:

   ```HTTP
   POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
   ```

     İstek gövdesinde şunları içerir:

   ```json
   {
    "role": "source"
   }
   ```

     Doğrulama işleminde yanıta aşağıdaki biçimdedir:

   ```json
   {
    "status": "{status}",
    "reasons": [
      "reason1",
      "reason2"
    ]
   }
   ```

2. Hedef abonelik bir çapraz abonelik taşıma katılabilir, kontrol edin. Aşağıdaki işlemi kullanın:

   ```HTTP
   POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
   ```

     İstek gövdesinde şunları içerir:

   ```json
   {
    "role": "target"
   }
   ```

     Kaynak abonelik doğrulama ile aynı biçimde yanıttır.
3. Her iki abonelik doğrulama testlerini geçerse, tüm Klasik kaynaklar bir abonelikten şu işlemi başka bir aboneliğe Taşı:

   ```HTTP
   POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01
   ```

    İstek gövdesinde şunları içerir:

   ```json
   {
    "target": "/subscriptions/{target-subscription-id}"
   }
   ```

İşlemi birkaç dakika çalışabilir.

### <a name="recovery-services-limitations"></a>Kurtarma Hizmetleri sınırlamalarını

 Bir kurtarma Hizmetleri kasasına taşımak için içinde kaydedilmesi gereken bir [sınırlı genel Önizleme](../backup/backup-azure-move-recovery-services-vault.md).

Şu anda bir kurtarma Hizmetleri kasası, bölge başına aynı anda taşıyabilirsiniz. Azure dosyaları, Azure dosya eşitleme veya SQL Iaas sanal makinelerini yedekleme kasaları taşıyamazsınız.

Bir sanal makine Kasayla birlikte hareket etmediği, süreleri doluncaya kadar geçerli sanal makine kurtarma noktaları kasada kalır. Veya, kasa ile sanal makinenin geçirildiğini olsun, kasadaki yedekleme geçmişinden sanal makineyi geri yükleyebilirsiniz.

Kurtarma Hizmetleri kasası, çapraz abonelik yedeklemeleri desteklemez. Abonelikler arasında bir sanal makine yedekleme verileri kasaya taşırsanız, sanal makinelerinizi aynı aboneliğe taşıyın ve yedeklemeler devam etmek için aynı hedef kaynak grubu kullanın.

Kasa taşındıktan sonra kasa için tanımlanan yedekleme ilkeleri tutulur. Raporlama ve izleme yeniden için kasayı taşımadan sonra ayarlanması gerekir.

Kurtarma Hizmetleri kasası taşımadan yeni bir abonelik için bir sanal makineyi taşımak için:

 1. Geçici olarak yedeklemeyi Durdur
 1. [Geri yükleme noktasını Sil](#virtual-machines-limitations). Bu işlem, yedeklenen verileri kasadaki değil yalnızca anında kurtarma noktalarını siler.
 1. Yeni Abonelik için sanal makineleri taşıma
 1. Yeni bir kasa Bu abonelik altında yeniden koruma

Taşıma, Azure Site Recovery ile olağanüstü durum kurtarma ayarlamak için kullanılan depolama, ağ ve bilgi işlem kaynakları için etkin değil. Örneğin, bir depolama hesabına (Storage1) şirket içi makinelerinizi çoğaltma işlemini ayarladıktan ve bir sanal ağa (Network1) bağlı sanal makine (VM1) olarak Azure'a yük devretme işleminden sonra görünmesi korunan makinenin istediğiniz varsayalım. Azure şu kaynaklara - Storage1, VM1 ve Network1 - aynı abonelik içindeki kaynak grupları arasında veya abonelikler arasında taşıyamazsınız.

### <a name="hdinsight-limitations"></a>HDInsight sınırlamaları

HDInsight kümeleri, yeni bir abonelik veya kaynak grubuna taşıyabilirsiniz. Ancak, ağ kaynaklarını (örneğin, sanal ağ, NIC veya yük dengeleyici) HDInsight kümesine bağlı abonelikler arasında taşınamaz. Ayrıca, küme için bir sanal makineye ekli NIC yeni bir kaynak grubuna taşınamıyor.

Bir HDInsight kümesi için yeni bir abonelik taşırken, önce diğer kaynakları (örneğin, depolama hesabı) taşıyın. Ardından, HDInsight kümesi, tek başına taşıyın.

## <a name="checklist-before-moving-resources"></a>Kaynakları taşımadan önce Yapılacaklar listesi

Bir kaynağı taşımadan önce yapmanız gereken bazı önemli adımlar vardır. Bu koşulları doğrulayarak hataları önleyebilirsiniz.

1. Kaynak ve hedef abonelikler etkin olması gerekir. Hesabınız devre dışı bırakıldı, etkinleştirme yaşıyorsanız [bir Azure destek isteği oluşturma](../azure-supportability/how-to-create-azure-support-request.md). Seçin **abonelik yönetimi** sorun türü için.

1. Kaynak ve hedef abonelikler aynı içinde bulunmalıdır [Azure Active Directory kiracısı](../active-directory/develop/quickstart-create-new-tenant.md). Her iki aboneliğin aynı Kiracı Kimliğine sahip denetlemek için Azure PowerShell veya Azure CLI'yı kullanın.

   Azure PowerShell için şunu kullanın:

   ```azurepowershell-interactive
   (Get-AzSubscription -SubscriptionName <your-source-subscription>).TenantId
   (Get-AzSubscription -SubscriptionName <your-destination-subscription>).TenantId
   ```

   Azure CLI için şunu kullanın:

   ```azurecli-interactive
   az account show --subscription <your-source-subscription> --query tenantId
   az account show --subscription <your-destination-subscription> --query tenantId
   ```

   Kaynak ve hedef abonelikler için Kiracı kimlikleri aynı değilse, Kiracı kimlikleri karşılaştırmak için aşağıdaki yöntemleri kullanın:

   * [Azure aboneliğinin sahipliğini başka bir hesaba devretme](../billing/billing-subscription-transfer.md)
   * [Azure Active Directory'ye bir Azure aboneliğini ekleme veya ilişkilendirme](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)

1. Hedef abonelik, taşınan kaynağın kaynak sağlayıcısına kayıtlı olmalıdır. Belirten bir hata alırsanız, **kaynak türü için abonelik kayıtlı değil**. Abonelik bu kaynak türü ile hiçbir zaman kullanılmış, ancak yeni bir abonelik için bir kaynak taşıma sırasında şu hatayla karşılaşabilirsiniz.

   PowerShell için kayıt durumunu almak için aşağıdaki komutları kullanın:

   ```azurepowershell-interactive
   Set-AzContext -Subscription <destination-subscription-name-or-id>
   Get-AzResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
   ```

   Bir kaynak sağlayıcısını kaydetmek için kullanın:

   ```azurepowershell-interactive
   Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
   ```

   Azure CLI için kayıt durumunu almak için aşağıdaki komutları kullanın:

   ```azurecli-interactive
   az account set -s <destination-subscription-name-or-id>
   az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
   ```

   Bir kaynak sağlayıcısını kaydetmek için kullanın:

   ```azurecli-interactive
   az provider register --namespace Microsoft.Batch
   ```

1. Kaynakları taşıma hesabı, en az aşağıdaki izinlere sahip olmanız gerekir:

   * **Microsoft.Resources/subscriptions/resourceGroups/moveResources/action** kaynak kaynak grubu.
   * **Microsoft.Resources/subscriptions/resourceGroups/write** hedef kaynak grubunda.

1. Kaynakları taşımadan önce kaynaklara Taşımakta olduğunuz abonelik için abonelik kotaları denetleyin. Kaynakları taşıma abonelik, sınırları aşamaz anlamına gelir, kota artışı isteği olup olmadığını gözden geçirmek gerekir. Limitler ve bir artış istemek nasıl bir listesi için bkz. [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../azure-subscription-service-limits.md).

1. Mümkün olduğunda, kesme büyük ayrı taşıma işlemlerini taşır. Tek bir işlemde kaynakları 800'den fazla olduğunda, Kaynak Yöneticisi'ni hemen bir hata döndürür. Ancak, 800'den daha az kaynağı taşımadan da zaman aşımına göre başarısız olabilir.

1. Hizmet, kaynakları taşıma olanağını sağlamalıdır. Taşıma başarılı olur olup olmadığını belirlemek için [, taşıma isteği doğrulamak](#validate-move). Bu makalede, aşağıdaki bölümlere bakın [Hizmetleri kaynak taşıma olanağı](#services-that-can-be-moved) ve [Hizmetleri kaynakların taşınması etkinleştirmediğiniz](#services-that-cannot-be-moved).

## <a name="validate-move"></a>Taşıma doğrula

[Taşıma işlemi doğrulama](/rest/api/resources/resources/validatemoveresources) kaynakları taşımadan taşıma senaryonuza sınamanızı sağlar. Bu işlem, taşıma başarılı olur belirlemek için kullanın. Bu işlemin çalıştırılması için ihtiyacınız vardır:

* Kaynak kaynak grubu adı
* Hedef kaynak grubunun kaynak kimliği
* Her bir kaynağın kaynak kimliği taşımak için
* [erişim belirteci](/rest/api/azure/#acquire-an-access-token) hesabınız için

Aşağıdaki isteği gönder:

```
POST https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<source-group>/validateMoveResources?api-version=2018-02-01
Authorization: Bearer <access-token>
Content-type: application/json
```

İstek gövdesi ile:

```json
{
 "resources": ["<resource-id-1>", "<resource-id-2>"],
 "targetResourceGroup": "/subscriptions/<subscription-id>/resourceGroups/<target-group>"
}
```

İstek düzgün biçimlendirilmiş olup olmadığını, işlemi döndürür:

```
Response Code: 202
cache-control: no-cache
pragma: no-cache
expires: -1
location: https://management.azure.com/subscriptions/<subscription-id>/operationresults/<operation-id>?api-version=2018-02-01
retry-after: 15
...
```

Doğrulama isteği kabul edildi, ancak henüz taşıma işlemi başarılı olur, belirlenen taşınmadığından 202 durum kodunu gösterir. `location` Değeri uzun süre çalışan işlemin durumunu denetlemek için kullandığınız bir URL içerir.  

Durumu denetlemek için aşağıdaki isteği gönder:

```
GET <location-url>
Authorization: Bearer <access-token>
```

İşlemi hala devam ederken, 202 durum kodu almaya devam eder. Belirtilen saniye sayısı bekleyin `retry-after` yeniden denemeden önce değeri. Taşıma işlemi başarıyla doğrular, 204 durum kodu alırsınız. Taşıma doğrulaması başarısız olursa, bir hata iletisi gibi alırsınız:

```json
{"error":{"code":"ResourceMoveProviderValidationFailed","message":"<message>"...}}
```

## <a name="move-resources"></a>Kaynakları taşıma

### <a name="a-nameuse-portal-by-using-azure-portal"></a><a name="use-portal" />Azure portalını kullanarak

Kaynakları taşıma için bu kaynakları içeren kaynak grubunu seçin ve ardından **taşıma** düğmesi.

![kaynakları taşıma](./media/resource-group-move-resources/select-move.png)

Kaynakları yeni kaynak grubu veya yeni bir aboneliğe taşıyor seçin.

Taşınacak kaynaklar ve hedef kaynak grubunu seçin. Bu kaynaklar için betiklerini güncelleştirin ve seçmek gereken bildirimi **Tamam**. Önceki adımda düzenleme abonelik simgesini seçtiğinizde hedef abonelik de seçmeniz gerekir.

![hedef seçin](./media/resource-group-move-resources/select-destination.png)

İçinde **bildirimleri**, taşıma işleminin çalıştığını görürsünüz.

![taşıma durumu göster](./media/resource-group-move-resources/show-status.png)

Tamamlandığında, sonucunu bildirilir.

![taşıma sonucu göster](./media/resource-group-move-resources/show-result.png)

### <a name="by-using-azure-powershell"></a>Azure PowerShell kullanarak

Var olan kaynakları başka bir kaynak grubuna veya aboneliğe taşıma için kullanın [taşıma AzResource](/powershell/module/az.resources/move-azresource) komutu. Aşağıdaki örnek, çeşitli kaynakları yeni kaynak grubuna taşımak gösterilmektedir.

```azurepowershell-interactive
$webapp = Get-AzResource -ResourceGroupName OldRG -ResourceName ExampleSite
$plan = Get-AzResource -ResourceGroupName OldRG -ResourceName ExamplePlan
Move-AzResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId
```

Yeni bir aboneliğe taşımak için bir değer içerir. `DestinationSubscriptionId` parametresi.

### <a name="by-using-azure-cli"></a>Azure CLI kullanarak

Var olan kaynakları başka bir kaynak grubuna veya aboneliğe taşıma için kullanın [az kaynak taşıma](/cli/azure/resource?view=azure-cli-latest#az-resource-move) komutu. Kaynak taşımak için kaynak kimliklerini sağlayın. Aşağıdaki örnek, çeşitli kaynakları yeni kaynak grubuna taşımak gösterilmektedir. İçinde `--ids` parametresi, kaynak kimliklerini taşımak için boşlukla ayrılmış bir listesini sağlayın.

```azurecli
webapp=$(az resource show -g OldRG -n ExampleSite --resource-type "Microsoft.Web/sites" --query id --output tsv)
plan=$(az resource show -g OldRG -n ExamplePlan --resource-type "Microsoft.Web/serverfarms" --query id --output tsv)
az resource move --destination-group newgroup --ids $webapp $plan
```

Yeni bir aboneliğe taşımak için sağlamak `--destination-subscription-id` parametresi.

### <a name="by-using-rest-api"></a>REST API kullanarak

Var olan kaynakları başka bir kaynak grubuna veya aboneliğe taşıma için şunu çalıştırın:

```HTTP
POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version}
```

İstek gövdesinde taşımak için hedef kaynak grubu ve kaynakları belirtin. Taşıma REST işlemi hakkında daha fazla bilgi için bkz: [kaynakları taşıma](/rest/api/resources/Resources/MoveResources).

## <a name="next-steps"></a>Sonraki adımlar

* Kaynaklarınızı yönetmek için PowerShell cmdlet'leri hakkında bilgi edinmek için bkz. [Azure PowerShell kullanarak Resource Manager ile](manage-resources-powershell.md).
* Kaynaklarınızı yönetmek için Azure CLI komutları hakkında bilgi edinmek için bkz. [Resource Manager ile Azure CLI kullanarak](manage-resources-cli.md).
* Aboneliğinizi yönetmeye yönelik portal özellikleri hakkında bilgi edinmek için bkz. [kaynakları yönetmek için Azure portalını kullanarak](resource-group-portal.md).
* Mantıksal bir kuruluş kaynaklarınıza uygulama hakkında bilgi edinmek için [kaynaklarınızı düzenlemek için etiketleri kullanarak](resource-group-using-tags.md).
