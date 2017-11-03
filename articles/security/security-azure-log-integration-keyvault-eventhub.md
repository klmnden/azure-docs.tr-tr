---
title: "Azure anahtar kasası günlükleri Olay hub'ları kullanarak tümleştirme | Microsoft Docs"
description: "Anahtar kasası günlükleri için bir SIEM Azure günlük tümleştirmeyi kullanarak kullanılabilmesi için gerekli adımları sağlar Öğreticisi"
services: security
author: barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 
ms.service: security
ms.topic: article
ms.date: 08/07/2017
ms.author: Barclayn
ms.custom: AzLog
ms.openlocfilehash: 3cd80817bf8b2ef2f66e9942eddc186a3eb5b5e4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-log-integration-tutorial-process-azure-key-vault-events-by-using-event-hubs"></a>Azure günlük tümleştirme Öğreticisi: olay hub'ları kullanarak işlem Azure anahtar kasası olayları

Günlüğe kaydedilen olayları almak ve güvenlik bilgileri ve Olay yönetimi (SIEM) sisteminiz için kullanılabilir duruma getirmek için Azure günlük tümleştirme özelliğini kullanabilirsiniz. Bu öğreticide Azure günlük tümleştirme aracılığıyla Azure Event Hubs alınan günlükleri işlemek için nasıl kullanılabileceği bir örnek gösterilmiştir.
 
Azure günlük tümleştirme ve Event Hubs birlikte nasıl örnek adımları izleyerek ve her adım çözümü nasıl destekler? anlama çalışır ile tanıyalım için bu öğreticiyi kullanın. Daha sonra buraya oluşturmak, şirketinizin benzersiz gereksinimlerini desteklemek için kendi adımları öğrendiklerinizi alabilir.

>[!WARNING]
Adımları ve Bu öğreticide komutları kopyalanan ve yapıştırılan amaçlanmamıştır. Bunlar yalnızca örnek demektir. "Olduğu gibi" PowerShell komutlarını Canlı ortamınızda kullanmayın. Bunları benzersiz ortamınıza bağlı özelleştirmeniz gerekir.


Bu öğreticide, alma olay hub'ına oturum Azure anahtar kasası etkinliği ve JSON dosyaları olarak SIEM sisteminizi kullanıma işlemi açıklanmaktadır. SIEM sisteminizi JSON dosyalarını işlemek için daha sonra yapılandırabilirsiniz.

>[!NOTE]
>Bu öğreticideki adımlardan çoğu anahtar kasalarını, depolama hesapları ve olay hub'ları yapılandırmayı içerir. Belirli Azure günlük tümleştirme adımları Bu öğreticinin sonunda ' dir. Bu adımları bir üretim ortamında gerçekleştirmeyin. Bunlar yalnızca bir laboratuvar ortamı için tasarlanmıştır. Üretimde kullanmadan önce adımları özelleştirmeniz gerekir.

Yol boyunca sağlanan bilgiler, her adım nedenler anlamanıza yardımcı olur. Diğer makalelerinin bağlantıları belirli konular hakkında daha fazla ayrıntı sağlayacaktır.

Bu öğretici değinmektedir hizmetleri hakkında daha fazla bilgi için bkz: 

- [Azure Anahtar Kasası.](../key-vault/key-vault-whatis.md)
- [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md)
- [Azure günlük tümleştirme](security-azure-log-integration-overview.md)


## <a name="initial-setup"></a>İlk kurulumu

Bu makaledeki adımları tamamlayabilmeniz için önce aşağıdakiler gerekir:

1. Bir Azure aboneliği ve hesabı bu abonelikte yönetici haklarına sahip. Bir aboneliğiniz yoksa oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).
 
2. Azure günlük tümleştirme yükleme gereksinimlerini karşılayan internet erişimi olan bir sistemi. Sistem veya şirket içinde barındırılan bir bulut hizmetinde olabilir.

3. [Azure günlük tümleştirme](https://www.microsoft.com/download/details.aspx?id=53324) yüklü. Yüklemek için:

   a. 2. adımda bahsedilen sisteme bağlanmak için Uzak Masaüstü'nü kullanın.   
   b. Azure günlük tümleştirme yükleyici sisteme kopyalayın. Yapabilecekleriniz [yükleme dosyalarını indirmek](https://www.microsoft.com/download/details.aspx?id=53324).   
   c. Yükleyiciyi başlatma ve Microsoft Yazılım Lisans Koşulları'nı kabul edin.   
   d. Telemetri bilgileri sağlarız, onay kutusunu seçili bırakın. Bunun yerine kullanım bilgilerini Microsoft'a göndermek, onay kutusunu temizleyin.
   
   Azure günlük tümleştirme ve nasıl yükleneceği hakkında daha fazla bilgi için bkz: [Azure tanılama günlüğünü ve Windows Olay iletme Azure günlük tümleştirme](security-azure-log-integration-get-started.md).

4. En son PowerShell sürümü.
 
   Windows Server 2016 sahip sonra en az PowerShell 5.0. Herhangi bir Windows Server sürümünü kullanıyorsanız, PowerShell yüklü önceki bir sürümü olabilir. Sürüm girerek denetleyebilirsiniz ```get-host``` bir PowerShell penceresinde. PowerShell 5.0 yüklü yoksa, şunları yapabilirsiniz [karşıdan](https://www.microsoft.com/download/details.aspx?id=50395).

   En az sonra PowerShell 5. 0'da, devam en son sürümünü yüklemek için:
   
   a. Bir PowerShell penceresinde girin ```Install-Module Azure``` komutu. Yükleme adımlarını tamamlayın.    
   b. Girin ```Install-Module AzureRM``` komutu. Yükleme adımlarını tamamlayın.

   Daha fazla bilgi için bkz: [Azure PowerShell yükleme](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0).


## <a name="create-supporting-infrastructure-elements"></a>Destekleyen altyapı öğeleri oluşturma

1. Yükseltilmiş bir PowerShell penceresi açın ve gidin **C:\Program Files\Microsoft Azure günlük tümleştirme**.
2. AzLog cmdlet'leri LoadAzLogModule.ps1 komut dosyası çalıştırarak içeri aktarın. Girin `.\LoadAzLogModule.ps1` komutu. (Bildirim ". \" Bu komutta.) Şuna benzer bir şey görmeniz gerekir:</br>

   ![Yüklenen modüller listesi](./media/security-azure-log-integration-keyvault-eventhub/loaded-modules.png)

3. Girin `Login-AzureRmAccount` komutu. Oturum açma penceresinde, Bu öğretici için kullanacağı aboneliği için kimlik bilgilerini girin.

   >[!NOTE]
   >Bu, Azure'a bu makineden oturum açtığınızdan ilk kez kullanıyorsanız, PowerShell kullanım verilerini toplamak için Microsoft izin verme hakkında bir ileti görürsünüz. Azure PowerShell geliştirmek için kullanılacak çünkü bu veri toplama etkinleştirmenizi öneririz.

4. Başarılı bir kimlik doğrulamasından sonra oturum açtınız ve aşağıdaki ekran görüntüsünde bilgilerine bakın. Sonraki adımları tamamlamanız gerekir çünkü abonelik kimliği ve abonelik adını not edin.

   ![PowerShell penceresi](./media/security-azure-log-integration-keyvault-eventhub/login-azurermaccount.png)
5. Daha sonra kullanılacak değerlerini depolamak için değişkenler oluşturun. Aşağıdaki PowerShell satırların her biri girin. Ortamınıza uyum sağlaması için değerleri ayarlamanız gerekebilir.
    - ```$subscriptionName = ‘Visual Studio Ultimate with MSDN’```(Abonelik adınızı farklı olabilir. Önceki komutunun çıktısını bir parçası olarak görebileceğiniz.)
    - ```$location = 'West US'```(Bu değişken kaynakların nerede oluşturulacağını konumu geçirmek için kullanılır. "Seçtiğiniz herhangi bir yerde olması için bu değişkeni değiştirebilirsiniz.)
    - ```$random = Get-Random```
    - ``` $name = 'azlogtest' + $random```(Ad, herhangi bir şey olabilir, ancak yalnızca küçük harf ve sayı içermelidir.)
    - ``` $storageName = $name```(Depolama hesabı adı için bu değişkeni kullanılır.)
    - ```$rgname = $name ```(Kaynak grubu adı için bu değişkeni kullanılır.)
    - ``` $eventHubNameSpaceName = $name```(Bu olay hub'ı ad alanının adıdır.)
6. İle çalışacaksınız abonelik belirtin:
    
    ```Select-AzureRmSubscription -SubscriptionName $subscriptionName```
7. Kaynak grubu oluşturun:
    
    ```$rg = New-AzureRmResourceGroup -Name $rgname -Location $location```
    
   Girerseniz `$rg` bu noktada, bu ekran görüntüsüne benzer bir çıktı görmeniz gerekir:

   ![Çıktıdan sonra kaynak grubu oluşturma](./media/security-azure-log-integration-keyvault-eventhub/create-rg.png)
8. Durum bilgileri izlemek için kullanılan bir depolama hesabı oluşturun:
    
    ```$storage = New-AzureRmStorageAccount -ResourceGroupName $rgname -Name $storagename -Location $location -SkuName Standard_LRS```
9. Olay hub'ı ad alanını oluşturun. Bu, bir olay hub'ı oluşturmak için gereklidir.
    
    ```$eventHubNameSpace = New-AzureRmEventHubNamespace -ResourceGroupName $rgname -NamespaceName $eventHubnamespaceName -Location $location```
10. Öngörüler sağlayıcı ile kullanılacak kural kimliği alın:
    
    ```$sbruleid = $eventHubNameSpace.Id +'/authorizationrules/RootManageSharedAccessKey' ```
11. Tüm olası Azure konumlarını almak ve daha sonraki bir adımda kullanılabilir bir değişken adları ekleyin:
    
    a. ```$locationObjects = Get-AzureRMLocation```    
    b. ```$locations = @('global') + $locationobjects.location```
    
    Girerseniz `$locations` bu noktada, Get-AzureRmLocation tarafından döndürülen ek bilgiler olmadan konum adlarını görürsünüz.
12. Bir Azure Resource Manager günlük profili oluşturun: 
    
    ```Add-AzureRmLogProfile -Name $name -ServiceBusRuleId $sbruleid -Locations $locations```
    
    Azure günlük profili hakkında daha fazla bilgi için bkz: [Azure etkinlik günlüğü'ne genel bakış](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

> [!NOTE]
> Bir günlük profil oluşturmaya çalışırken bir hata iletisi alabilirsiniz. Get-AzureRmLogProfile ve Kaldır-AzureRmLogProfile belgelerine daha sonra gözden geçirebilirsiniz. Get-AzureRmLogProfile çalıştırırsanız, günlük profili hakkındaki bilgileri görebilirsiniz. Girerek mevcut günlük profili silebilirsiniz ```Remove-AzureRmLogProfile -name 'Log Profile Name' ``` komutu.
>
>![Resource Manager profili hatası](./media/security-azure-log-integration-keyvault-eventhub/rm-profile-error.png)

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

1. Anahtar kasası oluşturun:

   ```$kv = New-AzureRmKeyVault -VaultName $name -ResourceGroupName $rgname -Location $location ```

2. Anahtar kasası için günlüğe kaydetmeyi yapılandırın:

   ```Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -ServiceBusRuleId $sbruleid -Enabled $true ```

## <a name="generate-log-activity"></a>Günlük etkinliği oluştur

İstekleri anahtar kasası için günlük etkinliği oluşturmak için gönderilmesi gerekir. Eylemler anahtar oluşturma, gizli saklamak ister veya gizli anahtar Kasası'nı okuma günlük girişi oluşturur.

1. Geçerli depolama anahtarlarını görüntüle:
    
   ```Get-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname  | ft -a```
2. Yeni bir **key2**:
    
   ```New-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname -KeyName key2```
3. Anahtarları yeniden görüntülemek ve görüp **key2** farklı bir değer içerir:
    
   ```Get-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname  | ft -a```
4. Ayarlama ve ek günlük girişleri oluşturmak için bir gizlilik okuyun:
    
   a. ```Set-AzureKeyVaultSecret -VaultName $name -Name TestSecret -SecretValue (ConvertTo-SecureString -String 'Hi There!' -AsPlainText -Force)``` b. ```(Get-AzureKeyVaultSecret -VaultName $name -Name TestSecret).SecretValueText```

   ![Gizli döndürdü](./media/security-azure-log-integration-keyvault-eventhub/keyvaultsecret.png)


## <a name="configure-azure-log-integration"></a>Azure günlük tümleştirmesini yapılandırma

Olay hub'ına oturum anahtar kasası için gerekli olan tüm öğelerin yapılandırdığınıza göre Azure günlük tümleştirmeyi de yapılandırmanız gerekir:

1. ```$storage = Get-AzureRmStorageAccount -ResourceGroupName $rgname -Name $storagename```
2. ```$eventHubKey = Get-AzureRmEventHubNamespaceKey -ResourceGroupName $rgname -NamespaceName $eventHubNamespace.name -AuthorizationRuleName RootManageSharedAccessKey```
3. ```$storagekeys = Get-AzureRmStorageAccountKey -ResourceGroupName $rgname -Name $storagename```
4. ``` $storagekey = $storagekeys[0].Value```

Her olay hub'ına yönelik AzLog komutu çalıştırın:

1. ```$eventhubs = Get-AzureRmEventHub -ResourceGroupName $rgname -NamespaceName $eventHubNamespaceName```
2. ```$eventhubs.Name | %{Add-AzLogEventSource -Name $sub' - '$_ -StorageAccount $storage.StorageAccountName -StorageKey $storageKey -EventHubConnectionString $eventHubKey.PrimaryConnectionString -EventHubName $_}```

Bir dakika veya bunu son iki komutlarını çalıştırarak, sonra oluşturulan JSON dosyaları görmeniz gerekir. Dizin izleyerek onaylayın **C:\users\AzLog\EventHubJson**.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure günlük tümleştirme hakkında SSS](security-azure-log-integration-faq.md)
- [Azure günlük tümleştirme ile çalışmaya başlama](security-azure-log-integration-get-started.md)
- [Azure kaynaklarını günlüklerinden SIEM sistemlerinizi tümleştirme](security-azure-log-integration-overview.md)
