---
title: Azure kaynakları için yeni abonelik veya kaynak grubu taşıma | Microsoft Docs
description: Kaynakları yeni kaynak grubuna veya aboneliğe taşıma için Azure Resource Manager'ı kullanın.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
ms.assetid: ab7d42bd-8434-4026-a892-df4a97b60a9b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/07/2018
ms.author: tomfitz
ms.openlocfilehash: 0b0ddedde49208a85628cdfc226f870a32ff7170
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "45985873"
---
# <a name="move-resources-to-new-resource-group-or-subscription"></a>Kaynakları yeni kaynak grubuna veya aboneliğe taşıma

Bu makalede, yeni bir abonelik veya aynı abonelikte yeni bir kaynak grubu için kaynakları taşıma işlemini göstermektedir. Kaynak taşıma için portal, PowerShell, Azure CLI veya REST API'yi kullanın. Bu makalede taşıma işlemlerini Azure desteği'nden herhangi bir Yardım için kullanılabilir.

Kaynakları taşırken, hem kaynak grubunun hem de hedef grubu işlem sırasında kilitlenir. Yazma ve silme işlemleri taşıma işlemi tamamlanana kadar kaynak gruplarında engellenir. Bu kilit ekleyemez, güncelleştirme veya kaynak gruplarındaki kaynakları silin, ancak kaynakları dondurulmuş gelmez anlamına gelir. Örneğin, bir SQL Server ve veritabanı yeni bir kaynak grubuna taşırsanız, veritabanı kullanan bir uygulama kapalı kalma süresi olmadan karşılaşır. Bunu hala okuyabilir ve veritabanına yazma.

Kaynağın konumu değiştirilemez. Bir kaynak taşıma yalnızca bu yeni bir kaynak grubuna taşınır. Yeni kaynak grubu farklı bir konuma sahip olabilir, ancak, kaynak konumunu değiştirmez.

> [!NOTE]
> Bu makale, mevcut bir Azure içinde kaynaklar teklifi hesap taşıma açıklamaktadır. Aslında (ön ödeme için Kullandıkça Öde'den yükseltme gibi) sunarak Azure hesabınızı değiştirmek devam ederken, mevcut bir kaynak ile çalışmak için bkz istiyorsanız [Azure aboneliğinizi başka bir teklife geç](../billing/billing-how-to-switch-azure-offer.md).
>
>

## <a name="checklist-before-moving-resources"></a>Kaynakları taşımadan önce Yapılacaklar listesi

Bir kaynağı taşımadan önce gerçekleştirmeniz gereken bazı önemli adımlar vardır. Bu koşulları doğrulayarak hataları önleyebilirsiniz.

1. Kaynak ve hedef abonelikler aynı içinde bulunmalıdır [Azure Active Directory kiracısı](../active-directory/develop/quickstart-create-new-tenant.md). Her iki aboneliğin aynı Kiracı Kimliğine sahip denetlemek için Azure PowerShell veya Azure CLI'yı kullanın.

  Azure PowerShell için şunu kullanın:

  ```powershell
  (Get-AzureRmSubscription -SubscriptionName <your-source-subscription>).TenantId
  (Get-AzureRmSubscription -SubscriptionName <your-destination-subscription>).TenantId
  ```

  Azure CLI için şunu kullanın:

  ```azurecli-interactive
  az account show --subscription <your-source-subscription> --query tenantId
  az account show --subscription <your-destination-subscription> --query tenantId
  ```

  Kaynak ve hedef abonelikler için Kiracı kimlikleri aynı değilse, Kiracı kimlikleri karşılaştırmak için aşağıdaki yöntemleri kullanın:

  * [Azure aboneliğinin sahipliğini başka bir hesaba devretme](../billing/billing-subscription-transfer.md)
  * [Azure Active Directory'ye bir Azure aboneliğini ekleme veya ilişkilendirme](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)

1. Hedef abonelik, taşınan kaynağın kaynak sağlayıcısına kayıtlı olmalıdır. Belirten bir hata alırsanız, **kaynak türü için abonelik kayıtlı değil**. Kaynağı taşıdığınız yeni abonelik, ilgili kaynak türüyle daha önce kullanılmamışsa bu sorunla karşılaşabilirsiniz.

  PowerShell için kayıt durumunu almak için aşağıdaki komutları kullanın:

  ```powershell
  Set-AzureRmContext -Subscription <destination-subscription-name-or-id>
  Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
  ```

  Bir kaynak sağlayıcısını kaydetmek için kullanın:

  ```powershell
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
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

1. Mümkün olduğunda, kesme büyük ayrı taşıma işlemlerini taşır. Kaynak Yöneticisi'ni hemen 800'den fazla kaynakları tek bir işlemde taşıma girişimleri başarısız olur. Ancak, 800'den daha az kaynağı taşımadan da zaman aşımına göre başarısız olabilir.

1. Hizmet, kaynakları taşıma olanağını sağlamalıdır. Taşıma başarılı olur olup olmadığını belirlemek için [, taşıma isteği doğrulamak](#validate-move). Bu makalede, aşağıdaki bölümlere bakın [Hizmetleri kaynak taşıma olanağı](#services-that-can-be-moved) ve [Hizmetleri kaynakların taşınması etkinleştirmediğiniz](#services-that-cannot-be-moved).

## <a name="when-to-call-support"></a>Destek çağrısı yapıldığında

Bu makalede gösterilen Self-Servis işlemler çoğu kaynaklarında taşıyabilirsiniz. Self Servis işlemleri için kullanın:

* Resource Manager kaynaklarını taşıma
* Şunlara göre Klasik kaynakları taşıma [Klasik dağıtım sınırlamalarına](#classic-deployment-limitations).

İlgili kişi [Destek](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) gerektiğinde:

* Kaynaklarınızı bir yeni bir Azure hesabı (ve Azure Active Directory kiracısına) taşımak ve önceki bölümde yönergeleri konusunda Yardım gerekiyor.
* Klasik kaynakları taşıma ancak kısıtlamalarla sorun yaşıyoruz.

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
 "resources": ['<resource-id-1>', '<resource-id-2>'],
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

## <a name="services-that-can-be-moved"></a>Taşınabilir Hizmetleri

Aşağıdaki listede, bir yeni kaynak grubu ve abonelik taşınabilir Azure hizmetlerinin genel bir özeti verilmiştir. Daha fazla ayrıntı için bkz. [taşıma işlemi Destek kaynakları için](move-support-resources.md).

* Analysis Services
* API Management
* App Service uygulamaları (web uygulamaları) - bkz [App Service kısıtlamaları](#app-service-limitations)
* App Service Sertifikaları
* Application Insights
* Otomasyon
* Azure Active Directory B2C
* Azure Cosmos DB
* Azure DevOps - Microsoft dışı uzantılı Azure DevOps kuruluşlarına satın gereken [aldıklarını iptal](https://go.microsoft.com/fwlink/?linkid=871160) abonelikler arasında hesap taşınabilmesi.
* Azure Haritalar
* Azure Geçişi
* Azure Stack - kayıtları
* Batch
* BizTalk Services
* Bot Hizmeti
* CDN
* Bulut Hizmetleri - bkz [Klasik dağıtım sınırlamalarını](#classic-deployment-limitations)
* Bilişsel Hizmetler
* Container Kayıt Defteri
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
* Bkz: HDInsight kümeleri - [HDInsight sınırlamaları](#hdinsight-limitations)
* IoT Central
* IoT Hub
* Key Vault
* Yük Dengeleyiciler - bkz [yük dengeleyici sınırlamaları](#lb-limitations)
* Log Analytics
* Logic Apps
* Machine Learning - Machine Learning Studio web hizmetleri aynı abonelikte ancak farklı bir abonelikte bir kaynak grubuna taşındı. Diğer Machine Learning kaynakları abonelikler arasında taşınabilir.
* Yönetilen kimlik - kullanıcı tarafından atanan
* Media Services
* Mobile Engagement
* Notification Hubs
* Operasyonel İçgörüler
* Operations Management
* Portalı panoları
* Power BI - hem Power BI Embedded ve Power BI çalışma alanı koleksiyonu
* Genel IP - bkz [genel IP kısıtlamaları](#pip-limitations)
* Redis Cache - Redis Cache örneği sanal ağ ile yapılandırılmışsa, örneği farklı bir aboneliğe taşınamaz. Bkz: [sanal ağlar sınırlamaları](#virtual-networks-limitations).
* Scheduler
* Arama
* Service Bus
* Service Fabric
* Service Fabric Mesh
* SignalR hizmeti
* Depolama - depolama hesapları farklı bölgelerde, aynı işlem içinde taşınamaz. Bunun yerine, her bölge için ayrı işlem kullanın.
* Depolama alanı (Klasik) - [Klasik dağıtım sınırlamalarını](#classic-deployment-limitations)
* Stream Analytics - Stream Analytics işleri çalıştırırken buna taşınamaz durumu.
* SQL veritabanı sunucusu - veritabanı ve sunucu, aynı kaynak grubunda bulunmalıdır. Bir SQL server taşıdığınızda, tüm veritabanlarını da taşınır. Bu davranış, Azure SQL veritabanı ve Azure SQL veri ambarı veritabanları için geçerlidir.
* Time Series Insights
* Traffic Manager
* Sanal makineler - yönetilen disklere sahip VM'ler taşınamaz. Bkz: [sanal makineler sınırlamaları](#virtual-machines-limitations)
* Sanal makineler (Klasik) - bkz [Klasik dağıtım sınırlamalarını](#classic-deployment-limitations)
* Sanal makine ölçek kümeleri - bkz [sanal makineler sınırlamaları](#virtual-machines-limitations)
* Sanal ağlar - bkz [sanal ağlar sınırlamaları](#virtual-networks-limitations)
* VPN Gateway

## <a name="services-that-cannot-be-moved"></a>Taşınamayan Hizmetleri

Aşağıdaki listede, bir yeni kaynak grubu ve abonelik taşınamaz Azure hizmetlerinin genel bir özeti verilmiştir. Daha fazla ayrıntı için bkz. [taşıma işlemi Destek kaynakları için](move-support-resources.md).

* AD etki alanı Hizmetleri
* AD karma sistem durumu hizmeti
* Application Gateway
* MySQL için Azure Veritabanı
* PostgreSQL için Azure Veritabanı
* Azure veritabanı geçişi
* Azure Databricks
* Azure Geçişi
* Batch AI
* Sertifikalar - App Service sertifikaları taşınabilir, ancak karşıya yüklenen Sertifikalar [sınırlamaları](#app-service-limitations).
* Container Instances
* Kapsayıcı Hizmeti
* Data Box
* Geliştirme alanları
* Dynamics LCS
* Express Route
* Kubernetes Service
* Lab Services'i - aynı Abonelikteki yeni kaynak grubuna taşıma etkin, ancak çapraz abonelik taşıma etkin değil.
* Yük Dengeleyiciler - bkz [yük dengeleyici sınırlamaları](#lb-limitations)
* Yönetilen Uygulamalar
* Bkz: yönetilen diskler - [sanal makineler sınırlamaları](#virtual-machines-limitations)
* Microsoft Genomiks
* NetApp
* Genel IP - bkz [genel IP kısıtlamaları](#pip-limitations)
* Kurtarma Hizmetleri kasası - ayrıca yoksa, Kurtarma Hizmetleri kasası ile ilişkili işlem, ağ ve depolama kaynakları taşıma bkz [kurtarma Hizmetleri sınırlamalarını](#recovery-services-limitations).
* Azure’da SAP HANA
* Güvenlik
* Site Recovery
* StorSimple cihaz Yöneticisi
* Sanal ağlar (Klasik) - bkz [Klasik dağıtım sınırlamalarını](#classic-deployment-limitations)

## <a name="virtual-machines-limitations"></a>Sanal makineler sınırlamaları

Yönetilen diskler taşımayı desteklemez. Bu kısıtlama, birkaç ilgili kaynakları çok taşınamaması anlamına gelir. Taşınamıyor:

* Yönetilen diskler
* Yönetilen disklere sahip sanal makineler
* Yönetilen diskleri oluşturulan görüntüleri
* Yönetilen diskleri oluşturulan anlık görüntüleri
* Yönetilen disklere sahip sanal makineleri ile kullanılabilirlik kümeleri

Yönetilen disk taşınamıyor olsa da, bir kopyasını oluşturun ve ardından mevcut bir yönetilen diskten yeni bir sanal makine oluşturun. Daha fazla bilgi için bkz.

* Yönetilen diskleri aynı abonelikte veya farklı aboneliğe kopyalama [PowerShell](../virtual-machines/scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md) veya [Azure CLI](../virtual-machines/scripts/virtual-machines-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription.md)
* Mevcut bir yönetilen işletim sistemi diski ile kullanarak bir sanal makine oluşturma [PowerShell](../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md) veya [Azure CLI](../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md).

Market kaynaklardan bağlı planlar ile oluşturulan sanal makineler, kaynak grubu veya abonelik arasında taşınamaz. Geçerli Abonelikteki sanal makine sağlamasını kaldırma ve yeni aboneliği yeniden dağıtın.

Key Vault'ta depolanan bir sertifika ile sanal makineler için yeni bir kaynak grubu ile aynı abonelikte ancak değil, abonelikler arasında taşınabilir.

## <a name="virtual-networks-limitations"></a>Sanal ağlar sınırlamaları

Bir sanal ağ taşırken, bağımlı kaynaklarını da taşımanız gerekir. VPN ağ geçitleri için IP adresleri, sanal ağ geçitleri ve tüm ilişkili bağlantı kaynakları taşımanız gerekir. Yerel ağ geçitleri farklı kaynak grubunda olabilir.

Eşlenmiş sanal ağın taşımak için önce sanal ağ eşlemesi devre dışı bırakmanız gerekir. Devre dışı sonra sanal ağ taşıyabilirsiniz. Taşıma sonrasında, sanal ağ eşlemesi yeniden etkinleştirin.

Bir alt ağ kaynak gezinti bağlantılarının bulunduğu sanal ağı içeriyorsa bir sanal ağ başka bir aboneliğe taşınamıyor. Örneğin, bir Redis Cache kaynak bir alt ağa dağıtılırsa, bu alt ağ kaynak Gezinti bağlantısı vardır.

## <a name="app-service-limitations"></a>App Service kısıtlamaları

App Service kaynaklarını taşımak için sınırlamalar kaynakları bir abonelik içinde veya yeni bir aboneliğe taşıma bağlı olarak farklılık gösterir.

Karşıya yüklenen sertifikalar, App Service sertifikaları şu bölümlerde açıklanan sınırlamalar uygulanır. App Service sertifikaları, yeni kaynak grubu veya abonelik sınırlamaları olmadan taşıyabilirsiniz. Sonra önce tüm web uygulamaları taşıyın aynı App Service sertifikası kullanan birden fazla web uygulaması varsa sertifikayı taşıyın.

### <a name="moving-within-the-same-subscription"></a>Aynı abonelik içinde taşıma

Bir Web uygulaması taşınırken _aynı abonelik içindeki_, karşıya yüklenen SSL sertifikaları taşınamıyor. Ancak, kendi karşıya yüklenmiş SSL sertifikasını taşımadan bir Web uygulaması yeni kaynak grubuna taşıyabilirsiniz ve uygulamanızın SSL işlevselliği yine de çalışır.

SSL sertifikası ile Web uygulaması taşımak istiyorsanız, şu adımları izleyin:

1.  Web uygulamasından yüklenen sertifikayı silin.
2.  Web uygulaması taşıyın.
3.  Sertifikayı taşınan Web uygulamasına yükleyin.

### <a name="moving-across-subscriptions"></a>Abonelikler arasında taşıma

Bir Web uygulaması taşınırken _abonelikler arasında_, aşağıdaki sınırlamalar geçerlidir:

- Hedef kaynak grubu mevcut App Service kaynaklar olmaması gerekir. App Service kaynakları şunlardır:
    - Web Apps
    - App Service planları
    - Karşıya yüklenen veya içeri aktarılan SSL sertifikaları
    - App Service ortamları
- Kaynak grubundaki tüm App Service kaynakların birlikte taşınması gerekir.
- App Service kaynaklarını, bunlar ilk olarak oluşturulduğu kaynak grubunun yalnızca taşınabilir. Bir App Service kaynak artık özgün kaynak grubunda değilse, bunu geri özgün kaynak grubunda ilk taşınmalıdır ve ardından abonelikler arasında taşınabilir.

## <a name="classic-deployment-limitations"></a>Klasik dağıtım sınırlamalarına

Klasik modelle dağıtılmış kaynakları taşımak için seçenek kaynakları bir abonelik içinde veya yeni bir aboneliğe taşıma bağlı olarak farklılık gösterir.

### <a name="same-subscription"></a>Aynı abonelik

Aynı abonelik içindeki başka bir kaynak grubu için bir kaynak grubundan kaynakları taşırken aşağıdaki kısıtlamalar uygulanır:

* Sanal ağlar (Klasik) taşınamaz.
* Sanal makineler (Klasik) bulut hizmeti ile taşınmalıdır.
* Bulut hizmeti, tüm sanal makineleri taşıma içerdiğinde yalnızca taşınabilir.
* Bir kerede yalnızca bir bulut hizmeti taşınabilir.
* Bir kerede yalnızca bir depolama hesabı (Klasik) taşınabilir.
* Depolama hesabı (Klasik), bir sanal makine veya Bulut hizmeti ile aynı işlemde taşınamaz.

Klasik kaynakları aynı abonelik içindeki yeni bir kaynak grubuna taşımak için aracılığıyla standart taşıma işlemlerini kullanın. [portalı](#use-portal), [Azure PowerShell](#use-powershell), [Azure CLI](#use-azure-cli), veya [REST API](#use-rest-api). Resource Manager kaynaklarını taşımak için kullandığınız gibi işlemlerin aynısını kullanın.

### <a name="new-subscription"></a>Yeni abonelik

Kaynakları yeni bir aboneliğe taşınmasını, aşağıdaki kısıtlamalar uygulanır:

* Abonelikteki tüm Klasik kaynaklar aynı işlem içinde taşınmalıdır.
* Hedef aboneliği diğer Klasik kaynaklar içermemelidir.
* Taşıma, yalnızca klasik taşıma için ayrı bir REST API aracılığıyla istenebilir. Klasik kaynakları için yeni bir abonelik taşırken standart Resource Manager'a taşıma komutlar çalışmaz.

Klasik kaynakları için yeni bir aboneliği taşımak, Klasik kaynakları için özel REST işlemlerini kullanın. REST kullanmak için aşağıdaki adımları gerçekleştirin:

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

## <a name="recovery-services-limitations"></a>Kurtarma Hizmetleri sınırlamalarını

Taşıma, Azure Site Recovery ile olağanüstü durum kurtarma ayarlamak için kullanılan depolama, ağ ve bilgi işlem kaynakları için etkin değil.

Örneğin, bir depolama hesabına (Storage1) şirket içi makinelerinizi çoğaltma işlemini ayarladıktan ve bir sanal ağa (Network1) bağlı sanal makine (VM1) olarak Azure'a yük devretme işleminden sonra görünmesi korunan makinenin istediğiniz varsayalım. Azure şu kaynaklara - Storage1, VM1 ve Network1 - aynı abonelik içindeki kaynak grupları arasında veya abonelikler arasında taşıyamazsınız.

Kaydedilmiş bir VM'yi taşıma için **Azure yedekleme** kaynak grupları arasında:
 1. Geçici olarak yedeklemeyi Durdur ve yedekleme verilerini koru
 2. Hedef kaynak grubu için VM'yi taşıma
 3. Kullanıcılar taşıma işleminden önce oluşturulan mevcut geri yükleme noktalarından geri aynı/yeni kasa altında yeniden koruyun.
Kullanıcı yedeklenen VM'yi abonelikler arasında taşınırsa, adım 1 ve 2. adım aynı kalır. 3. adımda sanal Makineyi altında mevcut / oluşturulan hedef abonelikte yeni bir kasa korumak kullanıcı gerekir. Kurtarma Hizmetleri kasası, çapraz abonelik yedeklemeleri desteklemez.

## <a name="hdinsight-limitations"></a>HDInsight sınırlamaları

HDInsight kümeleri, yeni bir abonelik veya kaynak grubuna taşıyabilirsiniz. Ancak, ağ kaynaklarını (örneğin, sanal ağ, NIC veya yük dengeleyici) HDInsight kümesine bağlı abonelikler arasında taşınamaz. Ayrıca, küme için bir sanal makineye ekli NIC yeni bir kaynak grubuna taşınamıyor.

Bir HDInsight kümesi için yeni bir abonelik taşırken, önce diğer kaynakları (örneğin, depolama hesabı) taşıyın. Ardından, HDInsight kümesi, tek başına taşıyın.

## <a name="search-limitations"></a>Arama sınırlamaları

Farklı bölgelerde tümünü tek seferde yerleştirilen birden çok arama kaynaklar taşınamıyor.
Böyle bir durumda, bunları ayrı ayrı taşımanız gerekiyor.

## <a name="lb-limitations"></a> Yük Dengeleyici sınırlamaları

Temel SKU yük Dengeleyicide taşınabilir.
Standart SKU yük Dengeleyicide taşınamaz.

## <a name="pip-limitations"></a> Genel IP kısıtlamaları

Temel SKU ortak IP'sine taşınabilir.
Standart SKU ortak IP'sine taşınamaz.

## <a name="use-portal"></a>Portal kullanma

Kaynakları taşıma için bu kaynakları içeren kaynak grubunu seçin ve ardından **taşıma** düğmesi.

![kaynakları taşıma](./media/resource-group-move-resources/select-move.png)

Kaynakları yeni kaynak grubu veya yeni bir aboneliğe taşıyor seçin.

Taşınacak kaynaklar ve hedef kaynak grubunu seçin. Bu kaynaklar için betiklerini güncelleştirin ve seçmek gereken bildirimi **Tamam**. Önceki adımda düzenleme abonelik simgesini seçtiğinizde hedef abonelik de seçmeniz gerekir.

![hedef seçin](./media/resource-group-move-resources/select-destination.png)

İçinde **bildirimleri**, taşıma işleminin çalıştığını görürsünüz.

![taşıma durumu göster](./media/resource-group-move-resources/show-status.png)

Tamamlandığında, sonucunu bildirilir.

![taşıma sonucu göster](./media/resource-group-move-resources/show-result.png)

## <a name="use-powershell"></a>PowerShell kullanma

Var olan kaynakları başka bir kaynak grubuna veya aboneliğe taşıma için kullanın [Move-AzureRmResource](/powershell/module/azurerm.resources/move-azurermresource) komutu. Aşağıdaki örnek, birden çok kaynakları yeni kaynak grubuna taşımak gösterilmektedir.

```powershell
$webapp = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExampleSite
$plan = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExamplePlan
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId
```

Yeni bir aboneliğe taşımak için bir değer içerir. `DestinationSubscriptionId` parametresi.

## <a name="use-azure-cli"></a>Azure CLI kullanma

Var olan kaynakları başka bir kaynak grubuna veya aboneliğe taşıma için kullanın [az kaynak taşıma](/cli/azure/resource?view=azure-cli-latest#az-resource-move) komutu. Kaynak taşımak için kaynak kimliklerini sağlayın. Aşağıdaki örnek, birden çok kaynakları yeni kaynak grubuna taşımak gösterilmektedir. İçinde `--ids` parametresi, kaynak kimliklerini taşımak için boşlukla ayrılmış bir listesini sağlayın.

```azurecli
webapp=$(az resource show -g OldRG -n ExampleSite --resource-type "Microsoft.Web/sites" --query id --output tsv)
plan=$(az resource show -g OldRG -n ExamplePlan --resource-type "Microsoft.Web/serverfarms" --query id --output tsv)
az resource move --destination-group newgroup --ids $webapp $plan
```

Yeni bir aboneliğe taşımak için sağlamak `--destination-subscription-id` parametresi.

## <a name="use-rest-api"></a>REST API’yi kullanma

Var olan kaynakları başka bir kaynak grubuna veya aboneliğe taşıma için şunu çalıştırın:

```HTTP
POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version}
```

İstek gövdesinde taşımak için hedef kaynak grubu ve kaynakları belirtin. Taşıma REST işlemi hakkında daha fazla bilgi için bkz: [kaynakları taşıma](/rest/api/resources/Resources/MoveResources).

## <a name="next-steps"></a>Sonraki adımlar

* Aboneliğinizi yönetmeye yönelik PowerShell cmdlet'leri hakkında bilgi edinmek için bkz. [Azure PowerShell kullanarak Resource Manager ile](powershell-azure-resource-manager.md).
* Aboneliğinizi yönetmeye yönelik Azure CLI komutları hakkında bilgi edinmek için bkz. [Resource Manager ile Azure CLI kullanarak](xplat-cli-azure-resource-manager.md).
* Aboneliğinizi yönetmeye yönelik portal özellikleri hakkında bilgi edinmek için bkz. [kaynakları yönetmek için Azure portalını kullanarak](resource-group-portal.md).
* Mantıksal bir kuruluş kaynaklarınıza uygulama hakkında bilgi edinmek için [kaynaklarınızı düzenlemek için etiketleri kullanarak](resource-group-using-tags.md).
