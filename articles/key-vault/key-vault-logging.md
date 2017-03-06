---
title: "Azure Anahtar Kasası Günlüğe Kaydetme | Microsoft Belgeleri"
description: "Azure Anahtar Kasası günlüğü ile çalışmaya başlamada yardım almak için bu öğreticiyi kullanın."
services: key-vault
documentationcenter: 
author: cabailey
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 43f96a2b-3af8-4adc-9344-bc6041fface8
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/07/2017
ms.author: cabailey
translationtype: Human Translation
ms.sourcegitcommit: 30b30513d5563cf64679e29c4858bf15f65d3a44
ms.openlocfilehash: 015c997135eae9c936af1a1ec0b0064912baaa04
ms.lasthandoff: 02/28/2017


---
# <a name="azure-key-vault-logging"></a>Azure Anahtar Kasası Günlüğü
Azure Anahtar Kasası çoğu bölgede kullanılabilir. Daha fazla bilgi için bkz. [Anahtar Kasası fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Giriş
Bir veya daha çok anahtar kasası oluşturduktan sonra, anahtar kasalarınıza nasıl, ne zaman ve kim tarafından erişildiğini büyük olasılıkla izlemek istersiniz. Anahtar Kasası için günlüğe kaydetmeyi etkinleştirerek bunu yapabilirsiniz; böylece sağladığınız Azure depolama hesabında bilgiler kaydedilir. Belirttiğiniz depolama hesabı için **insights-logs-auditevent** adlı yeni bir kapsayıcı oluşturulur ve birden çok anahtar kasasının günlüklerini toplamak için bu depolama hesabını kullanabilirsiniz.

Günlük bilgilerinize anahtar kasası işleminden en fazla 10 dakika sonra erişebilirsiniz. Çoğu durumda, bundan daha hızlı olacaktır.  Depolama hesabınızdaki günlüklerinizi yönetmek size bağlıdır:

* Günlüklerinize erişebilecek kişileri kısıtlayarak güvenliklerini sağlamak için standart Azure erişim denetimi yöntemlerini kullanın.
* Artık depolama hesabınızda tutmak istemediğiniz günlükleri silin.

Azure Anahtar Kasası günlüğü ile çalışmaya başlamada yardım almak, depolama hesabınızı oluşturmak, günlüğü etkinleştirmek ve toplanan günlük bilgilerini yorumlamak için bu öğreticiyi kullanın.  

> [!NOTE]
> Bu öğretici anahtar kasalarının, anahtarların veya gizli anahtarların nasıl oluşturulacağı hakkında yönergeler içermez. Bu bilgi için bkz. [Azure Anahtar Kasası ile çalışmaya başlama](key-vault-get-started.md). Alternatif olarak, Platformlar Arası Komut Satırı Arabirimi yönergeleri için [bu eşdeğer öğreticiye](key-vault-manage-with-cli.md) bakın.
> 
> Şu anda Azure portalında Azure Anahtar Kasası'nı yapılandıramazsınız. Bunun yerine, bu Azure PowerShell yönergelerini kullanın.
> 
> 

Azure Anahtar Kasası genel bakış bilgileri için bkz. [Azure Anahtar Kasası nedir?](key-vault-whatis.md)

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi tamamlamak için aşağıdakilere sahip olmanız gerekir:

* Kullanmakta olduğunuz var olan bir anahtar kasası.  
* Azure PowerShell'in, **en az 1.0.1 sürümü**. Azure PowerShell'i yüklemek ve Azure aboneliğinizle ilişkilendirmek için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs). Azure PowerShell'i zaten yüklediyseniz ve sürümünü bilmiyorsanız Azure PowerShell konsolunda `(Get-Module azure -ListAvailable).Version` yazın.  
* Anahtar Kasası günlükleriniz için Azure'da yeterli depolama.

## <a name="a-idconnectaconnect-to-your-subscriptions"></a><a id="connect"></a>Aboneliklerinize bağlanma
Bir Azure PowerShell oturumu başlatın ve aşağıdaki komutla Azure hesabınızda oturum açın:  

    Login-AzureRmAccount

Açılır tarayıcı penceresinde Azure hesabı kullanıcı adınızı ve parolanızı girin. Azure PowerShell bu hesapla ilişkili tüm abonelikleri alır ve varsayılan olarak birinciyi kullanır.

Birden çok aboneliğiniz varsa Azure Anahtar Kasanızı oluşturmak için kullanılan belirli bir tanesini belirtmeniz gerekebilir. Hesabınız için abonelikleri görmek üzere aşağıdakini yazın:

    Get-AzureRmSubscription

Ardından, günlüğünü tutacağınız anahtar kasasıyla ilişkili aboneliği belirtmek için şunu yazın:

    Set-AzureRmContext -SubscriptionId <subscription ID>

Azure Power Shell'i yapılandırma hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs).

## <a name="a-idstorageacreate-a-new-storage-account-for-your-logs"></a><a id="storage"></a>Günlükleriniz için yeni bir depolama hesabı oluşturma
Günlükleriniz için var olan depolama hesabını kullanabiliyor olsanız da, Anahtar Kasası günlüklerine özgü yeni bir depolama hesabı oluşturacağız. Bunu daha sonra belirtmemiz gerektiğinde kolaylık sağlamak için ayrıntıları **sa** adlı bir değişkende depolayacağız.

Ayrıca, ek yönetim kolaylığı için anahtar kasamızı içeren kaynak grubunu kullanacağız. [Başlangıç öğreticisi](key-vault-get-started.md)'nde bu kaynak grubu **ContosoResourceGroup** adına sahiptir ve Doğu Asya konumunu kullanmaya devam edeceğiz. Bunları uygun şekilde kendi değerlerinizle değiştirin:

    $sa = New-AzureRmStorageAccount -ResourceGroupName ContosoResourceGroup -Name contosokeyvaultlogs -Type Standard_LRS -Location 'East Asia'


> [!NOTE]
> Var olan bir depolama hesabını kullanmaya karar verirseniz anahtar kasanızla aynı aboneliği kullanmanız gerekir ve bunun da Klasik dağıtım modeli yerine Resource Manager dağıtım modelini kullanması gerekir.
> 
> 

## <a name="a-ididentifyaidentify-the-key-vault-for-your-logs"></a><a id="identify"></a>Günlükleriniz için anahtar kasasını tanımlama
Başlama öğreticimizde anahtar kasamızın adı **ContosoKeyVault**'tu; bu nedenle bu adı kullanmaya ve ayrıntıları **kv** adlı bir değişkende depolamaya devam edeceğiz:

    $kv = Get-AzureRmKeyVault -VaultName 'ContosoKeyVault'


## <a name="a-idenableaenable-logging"></a><a id="enable"></a>Günlüğe kaydetmeyi etkinleştirme
Anahtar Kasası için günlüğü etkinleştirmek üzere, yeni depolama hesabımız ve anahtar kasamız için oluşturduğumuz değişkenlerle birlikte Set-AzureRmDiagnosticSetting cmdlet'ini kullanacağız. Ayrıca, **-Enabled** bayrağını **$true** olarak ve kategoriyi de AuditEvent (Anahtar Kasası günlüğü için tek kategori budur) olarak ayarlayacağız:

    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

Bunun için çıkış şunları içerir:

    StorageAccountId   : /subscriptions/<subscription-GUID>/resourceGroups/ContosoResourceGroup/providers/Microsoft.Storage/storageAccounts/ContosoKeyVaultLogs
    ServiceBusRuleId   :
    StorageAccountName :
        Logs
        Enabled           : True
        Category          : AuditEvent
        RetentionPolicy
        Enabled : False
        Days    : 0


Böylece anahtar kasanız için günlüğün artık etkinleştirilmiş ve depolama hesabınıza bilgileri kaydediyor olduğu doğrulanır.

İsteğe bağlı olarak günlükleriniz için eski günlüklerin otomatik olarak silinmesi gibi bir bekletme ilkesi de ayarlayabilirsiniz. Örneğin, **-RetentionEnabled** bayrağını kullanarak bekletme ilkesini **$true** olarak ayarlayın ve **-RetentionInDays** parametresini **90**’a ayarlayarak 90 günden eski günlüklerin otomatik olarak silinmesini sağlayın.

    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent -RetentionEnabled $true -RetentionInDays 90

Günlüğe kaydedilenler:

* Erişim izinleri, sistem hataları veya hatalı istekler sonucunda başarısız olan istekleri içeren tüm kimliği doğrulanmış REST API istekleri günlüğe kaydedilir.
* Oluşturma, silme, anahtar kasası erişim ilkelerini ayarlama ve etiketler gibi anahtar kasası özniteliklerini güncelleştirmeyi kapsayan anahtar kasasının kendisine ait işlemler.
* Anahtar kasasındaki anahtarlar ve gizli dizelere yönelik oluşturma, değiştirme veya silmeyi, anahtarları imzalama, doğrulama, şifreleme, şifrelerini çözme, sarmalama ve kaydırmayı, gizli dizeleri almayı ve anahtarları, gizli dizeleri ve bunların sürümlerini listelemeyi kapsayan işlemler.
* Bir 401 yanıtına neden olan kimliği doğrulanmamış istekler. Örneğin, bir taşıyıcı belirtecine sahip olmayan veya hatalı biçimlendirilmiş ya da süresi dolmuş veya geçersiz bir belirtece sahip olan istekler.  

## <a name="a-idaccessaaccess-your-logs"></a><a id="access"></a>Günlüklerinize erişme
Anahtar kasası günlükleri, sağladığınız depolama hesabındaki **insights-logs-auditevent** kapsayıcısında depolanır. Bu kapsayıcıdaki tüm blobları listelemek için şunu yazın:

    Get-AzureStorageBlob -Container 'insights-logs-auditevent' -Context $sa.Context

Çıkış buna benzer şekilde görünür:

**Kapsayıcı Uri'si: https://contosokeyvaultlogs.blob.core.windows.net/insights-logs-auditevent**

**Ad**

- - -
**resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=05/h=01/m=00/PT1H.json**

**resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=02/m=00/PT1H.json**

**resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=18/m=00/PT1H.json****

Bu çıkışta gördüğünüz üzere, bloblar bir adlandırma kuralını izler: **resourceId=<ARM resource ID>/y=<year>/m=<month>/d=<day of month>/h=<hour>/m=<minute>/dosya adı.json**

Tarih ve saat değerleri UTC'yi kullanır.

Aynı depolama hesabı birden çok kaynak için günlük toplamak amacıyla kullanılabileceğinden, blob adındaki tam kaynak kimliğine erişmek veya yalnızca gereksinim duyduğunuz blobları indirmek çok kullanışlıdır. Ancak bunu yapmadan önce tüm blobların nasıl indirileceğini ele alacağız.

İlk olarak, blobları yüklemek için bir klasör oluşturun. Örnek:

    New-Item -Path 'C:\Users\username\ContosoKeyVaultLogs' -ItemType Directory -Force

Ardından tüm blobların listesini alın:  

    $blobs = Get-AzureStorageBlob -Container $container -Context $sa.Context

Blobları hedef klasörümüze indirmek için bu listeye 'Get-AzureStorageBlobContent' aracılığıyla kanal oluşturun:

    $blobs | Get-AzureStorageBlobContent -Destination 'C:\Users\username\ContosoKeyVaultLogs'

Bu ikinci komutu çalıştırdığınızda blob adlarındaki **/** sınırlayıcısı hedef klasörün altında tam bir klasör yapısı oluşturur ve blobları dosya olarak indirmek ve depolamak için bu yapı kullanılır.

Blobları seçmeli olarak indirmek için jokerleri kullanın. Örnek:

* Birden çok anahtar kasanız varsa ve yalnızca CONTOSOKEYVAULT3 adlı bir anahtar kasası için günlük indirmek isterseniz:
  
        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/VAULTS/CONTOSOKEYVAULT3
* Birden çok kaynak grubunuz varsa ve yalnızca bir kaynak grubu için günlük indirmek isterseniz `-Blob '*/RESOURCEGROUPS/<resource group name>/*'` kullanın:
  
        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'
* Ocak 2016 ayı için tüm günlükleri indirmek isterseniz `-Blob '*/year=2016/m=01/*'` kullanın:
  
        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/year=2016/m=01/*'

Artık günlüklerin içinde neler olduğuna bakmaya başlamak için hazırsınız. Ancak buna geçmeden önce Get-AzureRmDiagnosticSetting için bilmeniz gereken iki parametre daha var:

* Anahtar kasası kaynağınızın tanılama ayarlarının durumunu sorgulamak için: `Get-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId`
* Anahtar kasası kaynağınızın günlüğe kaydetmesini devre dışı bırakmak için: `Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories AuditEvent`

## <a name="a-idinterpretainterpret-your-key-vault-logs"></a><a id="interpret"></a>Anahtar Kasası günlüklerinizi yorumlama
Tek tek bloblar JSON blobu olarak biçimlendirilip metin olarak depolanır. Bu `Get-AzureRmKeyVault -VaultName 'contosokeyvault'` çalıştırılarak oluşturulmuş bir günlük girişi örneğidir:

    {
        "records":
        [
            {
                "time": "2016-01-05T01:32:01.2691226Z",
                "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
                "operationName": "VaultGet",
                "operationVersion": "2015-06-01",
                "category": "AuditEvent",
                "resultType": "Success",
                "resultSignature": "OK",
                "resultDescription": "",
                "durationMs": "78",
                "callerIpAddress": "104.40.82.76",
                "correlationId": "",
                "identity": {"claim":{"http://schemas.microsoft.com/identity/claims/objectidentifier":"d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn":"live.com#username@outlook.com","appid":"1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},
                "properties": {"clientInfo":"azure-resource-manager/2.0","requestUri":"https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id":"https://contosokeyvault.vault.azure.net/","httpStatusCode":200}
            }
        ]
    }


Aşağıdaki tabloda alan adları ve açıklamaları listelenmektedir.

| Alan adı | Açıklama |
| --- | --- |
| time |Tarih ve saat (UTC). |
| resourceId |Azure Resource Manager Kaynak Kimliği. Anahtar Kasası günlükleri için bu her zaman Anahtar Kasası kaynak kimliğidir. |
| operationName |Sonraki tabloda belirtildiği gibi işlemin adı. |
| operationVersion |Bu, istemci tarafından istenen REST API'si sürümüdür. |
| category |Anahtar Kasası günlükleri için AuditEvent kullanılabilen tek değerdir. |
| resultType |REST API'si isteğinin sonucu. |
| resultSignature |HTTP durumu. |
| resultDescription |Kullanılabilir olduğunda sonuç hakkında ek açıklama. |
| durationMs |Milisaniye cinsinden REST API'si isteğini sunmak için geçen süre. Buna ağ gecikme süresi dahil değildir; bu nedenle istemci tarafında ölçtüğünüz süre bu süreyle eşleşmeyebilir. |
| callerIpAddress |İsteği yapan istemcinin IP adresi. |
| correlationId |İstemci tarafı günlüklerini hizmet tarafı (Anahtar Kasası) günlükleriyle ilişkilendirmek için istemcinin geçirebileceği isteğe bağlı bir GUID. |
| identity |REST API'si isteği yapılırken sunulan belirteçten gelen kimlik. Bu genellikle bir "kullanıcı", "hizmet sorumlusu" veya bir Azure PowerShell cmdlet'inden kaynaklanan bir istekte olduğu gibi "kullanıcı+uygulama kimliği" birleşimidir. |
| properties |Bu alan işleme (operationName) bağlı olarak farklı bilgiler içerir. Çoğu durumda, istemci bilgilerini (istemci tarafından geçirilen useragent dizesi), REST API'sinin tam istek URI'sini ve HTTP durum kodunu içerir. Buna ek olarak, bir nesne bir istek (örneğin, KeyCreate veya VaultGet) nedeniyle döndürüldüğünde, Anahtar URI'sini ("id" olarak), Kasa URI'sini veya Gizli Anahtar URI'sini de içerir. |

**operationName** alanının değerleri ObjectVerb biçimindedir. Örnek:

* Tüm anahtar kasası işlemleri `VaultGet` ve `VaultCreate` gibi 'Vault`<action>`' biçimine sahiptir.
* Tüm anahtar işlemleri `KeySign` ve `KeyList` gibi 'Key`<action>`' biçimine sahiptir.
* Tüm gizli anahtar işlemleri `SecretGet` ve `SecretListVersions` gibi 'Secret`<action>`' biçimine sahiptir.

Aşağıdaki tabloda operationName ve karşılık gelen REST API'si komutu listelenmektedir.

| operationName | REST API'si Komutu |
| --- | --- |
| Kimlik Doğrulaması |Azure Active Directory uç noktası aracılığıyla |
| VaultGet |[Bir anahtar kasası hakkında bilgi edinme](https://msdn.microsoft.com/en-us/library/azure/mt620026.aspx) |
| VaultPut |[Anahtar kasası oluşturma veya güncelleştirme](https://msdn.microsoft.com/en-us/library/azure/mt620025.aspx) |
| VaultDelete |[Anahtar kasası silme](https://msdn.microsoft.com/en-us/library/azure/mt620022.aspx) |
| VaultPatch |[Bir anahtar kasasını güncelleştirme](https://msdn.microsoft.com/library/azure/mt620025.aspx) |
| VaultList |[Bir kaynak grubundaki tüm anahtar kasalarını listeleme](https://msdn.microsoft.com/en-us/library/azure/mt620027.aspx) |
| KeyCreate |[Anahtar oluşturma](https://msdn.microsoft.com/en-us/library/azure/dn903634.aspx) |
| KeyGet |[Bir anahtar hakkında bilgi edinme](https://msdn.microsoft.com/en-us/library/azure/dn878080.aspx) |
| KeyImport |[Bir kasaya anahtar aktarma](https://msdn.microsoft.com/en-us/library/azure/dn903626.aspx) |
| KeyBackup |[Bir anahtarı yedekleme](https://msdn.microsoft.com/en-us/library/azure/dn878058.aspx). |
| KeyDelete |[Anahtar silme](https://msdn.microsoft.com/en-us/library/azure/dn903611.aspx) |
| KeyRestore |[Bir anahtarı geri yükleme](https://msdn.microsoft.com/en-us/library/azure/dn878106.aspx) |
| KeySign |[Bir anahtar ile oturum açma](https://msdn.microsoft.com/en-us/library/azure/dn878096.aspx) |
| KeyVerify |[Bir anahtar ile doğrulama](https://msdn.microsoft.com/en-us/library/azure/dn878082.aspx) |
| KeyWrap |[Bir anahtarı sarmalama](https://msdn.microsoft.com/en-us/library/azure/dn878066.aspx) |
| KeyUnwrap |[Bir anahtarı kaydırma](https://msdn.microsoft.com/en-us/library/azure/dn878079.aspx) |
| KeyEncrypt |[Bir anahtar ile şifreleme](https://msdn.microsoft.com/en-us/library/azure/dn878060.aspx) |
| KeyDecrypt |[Bir anahtar ile şifre çözme](https://msdn.microsoft.com/en-us/library/azure/dn878097.aspx) |
| KeyUpdate |[Bir anahtarı güncelleştirme](https://msdn.microsoft.com/en-us/library/azure/dn903616.aspx) |
| KeyList |[Bir kasadaki anahtarları listeleme](https://msdn.microsoft.com/en-us/library/azure/dn903629.aspx) |
| KeyListVersions |[Bir anahtarın sürümlerini listeleme](https://msdn.microsoft.com/en-us/library/azure/dn986822.aspx) |
| SecretSet |[Gizli anahtar oluşturma](https://msdn.microsoft.com/en-us/library/azure/dn903618.aspx) |
| SecretGet |[Gizli anahtar alma](https://msdn.microsoft.com/en-us/library/azure/dn903633.aspx) |
| SecretUpdate |[Gizli anahtarı güncelleştirme](https://msdn.microsoft.com/en-us/library/azure/dn986818.aspx) |
| SecretDelete |[Gizli anahtarı silme](https://msdn.microsoft.com/en-us/library/azure/dn903613.aspx) |
| SecretList |[Bir kasadaki gizli anahtarları listeleme](https://msdn.microsoft.com/en-us/library/azure/dn903614.aspx) |
| SecretListVersions |[Bir gizli anahtarın sürümlerini listeleme](https://msdn.microsoft.com/en-us/library/azure/dn986824.aspx) |

## <a name="a-idloganalyticsause-log-analytics"></a><a id="loganalytics"></a>Log Analytics'i kullanma

Azure Key Vault AuditEvent günlüklerini incelemek için Log Analytics içindeki Azure Key Vault çözümünü kullanabilirsiniz. Kurulum adımları ve daha fazla bilgi için bkz. [Log Analytics'te Azure Key Vault çözümü](../log-analytics/log-analytics-azure-key-vault.md). Bu makalede ayrıca Log Analytics önizlemesinde sunulan ve günlüklerinizi önce bir Azure Depolama hesabına yönlendirip Log Analytics'i bu konumdan okuyacak şekilde yapılandırdığınız eski Key Vault çözümünden geçiş yapmak için kullanabileceğiniz talimatlara da yer verilmektedir.

## <a name="a-idnextanext-steps"></a><a id="next"></a>Sonraki adımlar
Azure Anahtar Kasası'nın bir web uygulamasında kullanıldığı bir öğretici için bkz. [Azure Anahtar Kasası'nı bir Web Uygulamasından Kullanma](key-vault-use-from-web-application.md).

Programlama başvuruları için bkz. [Azure Anahtar Kasası geliştirici kılavuzu](key-vault-developers-guide.md).

Azure Anahtar Kasası'na yönelik Azure PowerShell 1.0 cmdlet'leri listesi için bkz. [Azure Anahtar Kasası Cmdlet'leri](https://msdn.microsoft.com/library/azure/dn868052.aspx).

Azure Anahtar Kasası ile anahtar döndürme ve günlük denetimine ilişkin bir öğretici için bkz. [Uçtan uca anahtar döndürme ve denetim ile Anahtar Kasası ayarlama](key-vault-key-rotation-log-monitoring.md).


