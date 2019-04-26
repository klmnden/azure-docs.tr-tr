---
title: Event Hubs'ı kullanarak Azure Key vault'tan günlükleri tümleştirme | Microsoft Docs
description: Anahtar kasası günlükleri için bir SIEM Azure günlük Tümleştirmesi'ni kullanarak kullanılabilmesi için gerekli adımlar sağlanmaktadır Öğreticisi
services: security
author: barclayn
manager: barbkess
editor: TomShinder
ms.assetid: ''
ms.service: security
ms.topic: article
ms.date: 01/14/2019
ms.author: Barclayn
ms.custom: AzLog
ms.openlocfilehash: 7e70920e806b3d9838d693ff1fc74a3e9371319d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60445272"
---
# <a name="azure-log-integration-tutorial-process-azure-key-vault-events-by-using-event-hubs"></a>Azure günlük tümleştirme Öğreticisi: Event Hubs kullanarak Azure Key Vault olayları işleyin

>[!IMPORTANT]
> Azure günlük tümleştirme özelliği 06/01/2019 tarafından kullanımdan kaldırılacaktır. AzLog yüklemeleri, 27 Haziran 2018'de devre dışı bırakıldı. Taşıma iletme gözden geçirme sonrası yapmanız gerekenler hakkında rehberlik için [SIEM araçlarla tümleştirmek için kullanım Azure İzleyici](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/) 

Günlüğe kaydedilen olayları alıp, güvenlik bilgileri ve Olay yönetimi (SIEM) sistemine ayıklanarak Azure günlük Tümleştirmesi'ni kullanabilirsiniz. Bu öğreticide, Azure günlük tümleştirmesi Azure Event Hubs'a alınan günlükleri işlemek için nasıl kullanılabileceğini örneği gösterilmektedir.

SIEM satıcınızın Azure İzleyici Bağlayıcısı'nı kullanarak ve bunlar aşağıdaki Azure günlük tümleştirme için tercih edilen yöntem olan [yönergeleri](../azure-monitor/platform/stream-monitoring-data-event-hubs.md). Azure İzleyici için bir bağlayıcı SIEM satıcınıza sağlamıyorsa (sıem sistemlerinizden alınabileceği gibi Azure günlük tümleştirmesi tarafından destekleniyorsa) bu tür bir bağlayıcı kullanılabilir hale gelene kadar Azure günlük tümleştirmesi geçici bir çözüm kullanmanız mümkün olabilir.

 
Bu öğreticide, nasıl Azure günlük tümleştirmesi ve Event Hubs örnek adımları izleyerek ve her adım çözümü nasıl desteklediğini anlamak çalışılmasını ile tanışın için kullanırsınız. Daha sonra buraya oluşturmak şirketinizin benzersiz gereksinimlerini desteklemek için kendi adımları öğrendiklerinizi alabilir.

> [!WARNING]
> Bu öğreticideki komutları ve adımları kopyalanır ve yapıştırılan kullanılmaya yönelik değildir. Bunlar yalnızca örnek hedeflenmiştir. "Olduğu gibi" PowerShell komutlarını Canlı ortamınızda kullanmayın. Bunları benzersiz ortamınıza bağlı özelleştirmeniz gerekir.


Bu öğretici Azure Key Vault etkinlik bir olay hub'ına günlüğe alma ve kullanılabilir JSON dosyaları, SIEM sisteminize hale işlemi size kılavuzluk eder. SIEM sisteminizi JSON dosyalarını işlemek için daha sonra yapılandırabilirsiniz.

>[!NOTE]
>Bu öğreticideki adımların çoğu anahtar kasalarını, depolama hesapları ve olay hub'ları yapılandırma gerektirir. Belirli Azure günlük tümleştirme adımları Bu öğreticinin sonunda ' dir. Bu adımlar, bir üretim ortamında gerçekleştirmeyin. Bunlar, yalnızca bir laboratuvar ortamı için tasarlanmıştır. Üretim ortamında kullanmadan önce adımları özelleştirmeniz gerekir.

Süreç boyunca sağlanan bilgiler her adım ardındaki nedenler anlamanıza yardımcı olur. Diğer makalelere bağlantılar belirli konuları hakkında daha fazla ayrıntı sağlayacaktır.

Bu öğreticide bahsetmeleri hizmetleri hakkında daha fazla bilgi için bkz: 

- [Azure Anahtar Kasası.](../key-vault/key-vault-whatis.md)
- [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md)
- [Azure günlük tümleştirmesi](security-azure-log-integration-overview.md)


## <a name="initial-setup"></a>Başlangıç kurulumu

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu makaledeki adımları tamamlayabilmeniz için aşağıdakiler gerekir:

* Bir Azure aboneliği ve hesabı yönetici haklarına sahip bu abonelikte. Bir aboneliğiniz yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).
 
* Azure günlük Tümleştirmesi'ni yükleme gereksinimlerini karşılayan internet erişimi olan bir sistemi. Sistem veya şirket içinde barındırılan bir bulut hizmeti üzerinde olabilir.

* Azure günlük tümleştirmesi yüklü. Yüklemek için:

   a. 2. adımda bahsedilen sisteme bağlanmak için Uzak Masaüstü'nü kullanın.   
   b. Azure günlük tümleştirmesi yükleyicisi, sunucuya kopyalayın. c. Yükleyiciyi başlatın ve Microsoft yazılım lisans koşullarını kabul edin.

* Telemetri bilgilerini sağlayacaksa onay kutusunu seçili bırakın. Yerine kullanım bilgilerini Microsoft'a gönderdiğiniz değil, onay kutusunu temizleyin.

   Azure günlük tümleştirmesi ve nasıl yükleneceği hakkında daha fazla bilgi için bkz. [Azure tanılama günlüğünü ve Windows Olay iletme'yi Azure günlük tümleştirme](security-azure-log-integration-get-started.md).

* En son PowerShell sürümü.

   Windows Server 2016'ın yüklü olması durumunda en az PowerShell 5.0. Herhangi bir Windows Server sürümünü kullanıyorsanız, PowerShell'in önceki bir sürümü olabilir. Girerek sürümü denetleyebilirsiniz ```get-host``` bir PowerShell penceresinde. PowerShell 5.0 yüklü yoksa, şunları yapabilirsiniz [indirdiği](https://www.microsoft.com/download/details.aspx?id=50395).

   En az sonra PowerShell 5.0 devam edebilirsiniz yönergeleri izleyerek en son sürümünü yüklemek için [Azure PowerShell yükleme](/powershell/azure/install-az-ps).


## <a name="create-supporting-infrastructure-elements"></a>Destekleyici altyapı öğelerini oluşturma

1. Yükseltilmiş bir PowerShell penceresi açın ve gidin **C:\Program Files\Microsoft Azure günlük tümleştirmesi**.
1. AzLog cmdlet'leri LoadAzLogModule.ps1 betiği çalıştırarak alın. Girin `.\LoadAzLogModule.ps1` komutu. (Bildirim ". \" Bu komutta.) Şuna benzer bir şey görmeniz gerekir:</br>

   ![Yüklü modülleri listesini](./media/security-azure-log-integration-keyvault-eventhub/loaded-modules.png)

1. Girin `Connect-AzAccount` komutu. Oturum açma penceresinde, Bu öğretici için kullanacağınız aboneliği için kimlik bilgilerini girin.

   >[!NOTE]
   >Bu, Azure'a bu makineden oturum açtığınızdan ilk kez ise, PowerShell kullanım verileri toplamasına izin vererek Microsoft hakkında bir ileti görürsünüz. Azure PowerShell geliştirmek için kullanılacağından, bu veri toplamayı etkinleştirmenizi öneririz.

1. Başarılı kimlik doğrulamadan sonra oturum açtınız. Sonraki adımları tamamlamak için gerekeceği için abonelik kimliği ve abonelik adını not alın.

1. Daha sonra kullanılacak değerleri depolamak için değişkenler oluşturun. Aşağıdaki PowerShell satırların her biri girin. Değerlerin ortamınızla eşleşecek şekilde ayarlamanız gerekebilir.
    - ```$subscriptionName = 'Visual Studio Ultimate with MSDN'``` (Abonelik adınız farklı olabilir. Önceki komutun çıktısındaki bir parçası olarak görebileceğiniz.)
    - ```$location = 'West US'``` (Bu değişkeni kaynaklar nerede oluşturulması gereken konumun geçirmek için kullanılır. "Bu değişken, seçtiğiniz herhangi bir konuma olacak şekilde değiştirebilirsiniz.)
    - ```$random = Get-Random```
    - ```$name = 'azlogtest' + $random``` (Ad, herhangi bir şey olabilir, ancak yalnızca küçük harf ve rakam içermelidir.)
    - ```$storageName = $name``` (Bu değişken için depolama hesabı adı kullanılır.)
    - ```$rgname = $name``` (Bu değişkeni kaynak grubu adı kullanılır.)
    - ```$eventHubNameSpaceName = $name``` (Bu olay hub'ı ad alanının adıdır.)
1. İle çalışacaksınız aboneliği belirtin:
    
    ```Select-AzSubscription -SubscriptionName $subscriptionName```
1. Kaynak grubu oluşturun:
    
    ```$rg = New-AzResourceGroup -Name $rgname -Location $location```
    
   Girerseniz `$rg` bu noktada, bu ekran görüntüsüne benzer bir çıktı görmeniz gerekir:

   ![Kaynak grubu oluşturulduktan sonra çıktı](./media/security-azure-log-integration-keyvault-eventhub/create-rg.png)
1. Durum bilgileri izlemek için kullanılan depolama hesabı oluşturun:
    
    ```$storage = New-AzStorageAccount -ResourceGroupName $rgname -Name $storagename -Location $location -SkuName Standard_LRS```
1. Olay hub'ı ad alanı oluşturun. Bu, bir olay hub'ı oluşturmak için gereklidir.
    
    ```$eventHubNameSpace = New-AzEventHubNamespace -ResourceGroupName $rgname -NamespaceName $eventHubnamespaceName -Location $location```
1. Insights sağlayıcı ile kullanılacak kural Kimliğini alın:
    
    ```$sbruleid = $eventHubNameSpace.Id +'/authorizationrules/RootManageSharedAccessKey'```
1. Tüm olası Azure konumları alın ve daha sonraki bir adımda kullanılabilmesi için bir değişken adlarını ekleyin:
    
    a. ```$locationObjects = Get-AzLocation```    
    b. ```$locations = @('global') + $locationobjects.location```
    
    Girerseniz `$locations` bu noktada, Get-AzLocation tarafından döndürülen ek bilgiler olmadan konum adlarını görürsünüz.
1. Bir Azure Resource Manager günlük profilini oluşturun: 
    
    ```Add-AzLogProfile -Name $name -ServiceBusRuleId $sbruleid -Locations $locations```
    
    Azure günlük profili hakkında daha fazla bilgi için bkz. [Azure etkinlik günlüğü'ne genel bakış](../azure-monitor/platform/activity-logs-overview.md).

> [!NOTE]
> Günlük profilini oluşturmaya çalıştığınızda bir hata iletisi alabilirsiniz. Daha sonra Get-AzLogProfile ve Remove-AzLogProfile belgelerini gözden geçirebilirsiniz. Get-AzLogProfile çalıştırırsanız, günlük profili hakkındaki bilgileri görürsünüz. Var olan günlük profilini girerek silebilirsiniz ```Remove-AzLogProfile -name 'Log Profile Name'``` komutu.
>
>![Resource Manager profil hatası](./media/security-azure-log-integration-keyvault-eventhub/rm-profile-error.png)

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

1. Anahtar kasası oluşturun:

   ```$kv = New-AzKeyVault -VaultName $name -ResourceGroupName $rgname -Location $location```

1. Anahtar kasası için günlüğe kaydetmeyi yapılandırın:

   ```Set-AzDiagnosticSetting -ResourceId $kv.ResourceId -ServiceBusRuleId $sbruleid -Enabled $true```

## <a name="generate-log-activity"></a>Günlük etkinliği oluştur

İstekleri anahtar Kasası'na, Etkinlik günlüğünü üretmek için gönderilmesi gerekir. Eylemler gibi gizli dizileri, depolama, anahtar oluşturma veya Key Vault'tan gizli dizileri okunurken günlük girişi oluşturur.

1. Geçerli depolama anahtarlarını görüntüle:
    
   ```Get-AzStorageAccountKey -Name $storagename -ResourceGroupName $rgname  | ft -a```
1. Yeni bir **key2**:
    
   ```New-AzStorageAccountKey -Name $storagename -ResourceGroupName $rgname -KeyName key2```
1. Anahtarları yeniden görüntülemek ve görüp **key2** farklı bir değer tutar:
    
   ```Get-AzStorageAccountKey -Name $storagename -ResourceGroupName $rgname  | ft -a```
1. Ayarlayın ve ek günlük girişleri oluşturmak için bir gizli dizi okumak:
    
   a. ```Set-AzKeyVaultSecret -VaultName $name -Name TestSecret -SecretValue (ConvertTo-SecureString -String 'Hi There!' -AsPlainText -Force)``` b. ```(Get-AzKeyVaultSecret -VaultName $name -Name TestSecret).SecretValueText```

   ![Gizli dizi döndürdü](./media/security-azure-log-integration-keyvault-eventhub/keyvaultsecret.png)


## <a name="configure-azure-log-integration"></a>Azure günlük tümleştirmesini yapılandırma

Anahtar kasası günlüğü olay hub'ına sağlamak için gereken tüm öğeleri yapılandırdığınıza göre Azure günlük Tümleştirmesi'ni yapılandırmanız gerekir:

1. ```$storage = Get-AzStorageAccount -ResourceGroupName $rgname -Name $storagename```
1. ```$eventHubKey = Get-AzEventHubNamespaceKey -ResourceGroupName $rgname -NamespaceName $eventHubNamespace.name -AuthorizationRuleName RootManageSharedAccessKey```
1. ```$storagekeys = Get-AzStorageAccountKey -ResourceGroupName $rgname -Name $storagename```
1. ```$storagekey = $storagekeys[0].Value```

Her olay hub'ın AzLog komutu çalıştırın:

1. ```$eventhubs = Get-AzEventHub -ResourceGroupName $rgname -NamespaceName $eventHubNamespaceName```
1. ```$eventhubs.Name | %{Add-AzLogEventSource -Name $sub' - '$_ -StorageAccount $storage.StorageAccountName -StorageKey $storageKey -EventHubConnectionString $eventHubKey.PrimaryConnectionString -EventHubName $_}```

Bir dakika ya da son iki komutları çalıştırma sonrasında, oluşturulan JSON dosyaları görmeniz gerekir. Bu dizine izleyerek onaylayın **C:\users\AzLog\EventHubJson**.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure günlük tümleştirmesi hakkında SSS](security-azure-log-integration-faq.md)
- [Azure günlük tümleştirmesini kullanmaya başlama](security-azure-log-integration-get-started.md)
- [Azure kaynaklarını günlüklerini SIEM Sistemlerinizle tümleştirin](security-azure-log-integration-overview.md)
