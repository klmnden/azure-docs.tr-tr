---
title: Azure anahtar kasası günlüğü - Azure anahtar kasası | Microsoft Docs
description: Azure Anahtar Kasası günlüğü ile çalışmaya başlamada yardım almak için bu öğreticiyi kullanın.
services: key-vault
documentationcenter: ''
author: barclayn
manager: barbkess
tags: azure-resource-manager
ms.assetid: 43f96a2b-3af8-4adc-9344-bc6041fface8
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/18/2019
ms.author: barclayn
ms.openlocfilehash: 61f277eda721c1490d9f4b354442718558830fe7
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2019
ms.locfileid: "56300208"
---
# <a name="azure-key-vault-logging"></a>Azure Anahtar Kasası Günlüğü

Azure Anahtar Kasası çoğu bölgede kullanılabilir. Daha fazla bilgi için bkz. [Anahtar Kasası fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Giriş

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bir veya daha çok anahtar kasası oluşturduktan sonra, anahtar kasalarınıza nasıl, ne zaman ve kim tarafından erişildiğini büyük olasılıkla izlemek istersiniz. Anahtar Kasası için günlüğe kaydetmeyi etkinleştirerek bunu yapabilirsiniz; böylece sağladığınız Azure depolama hesabında bilgiler kaydedilir. Belirttiğiniz depolama hesabı için **insights-logs-auditevent** adlı yeni bir kapsayıcı oluşturulur ve birden çok anahtar kasasının günlüklerini toplamak için bu depolama hesabını kullanabilirsiniz.

Günlük bilgilerinize anahtar kasası işleminden en fazla 10 dakika sonra erişebilirsiniz. Çoğu durumda, bundan daha hızlı olacaktır.  Depolama hesabınızdaki günlüklerinizi yönetmek size bağlıdır:

* Günlüklerinize erişebilecek kişileri kısıtlayarak güvenliklerini sağlamak için standart Azure erişim denetimi yöntemlerini kullanın.
* Artık depolama hesabınızda tutmak istemediğiniz günlükleri silin.

Azure Anahtar Kasası günlüğü ile çalışmaya başlamada yardım almak, depolama hesabınızı oluşturmak, günlüğü etkinleştirmek ve toplanan günlük bilgilerini yorumlamak için bu öğreticiyi kullanın.  

> [!NOTE]
> Bu öğretici anahtar kasalarının, anahtarların veya gizli anahtarların nasıl oluşturulacağı hakkında yönergeler içermez. Bu bilgi için bkz. [Azure anahtar kasası nedir?](key-vault-overview.md). Alternatif olarak, Platformlar Arası Komut Satırı Arabirimi yönergeleri için [bu eşdeğer öğreticiye](key-vault-manage-with-cli2.md) bakın.
>
> Bu makalede tanılama günlüğünün güncelleştirmek için Azure PowerShell yönergelerini sağlar. Ancak, aynı Azure portalında Azure İzleyicisi'ni kullanarak etkinleştirilebilir **tanılama günlükleri** bölümü. 
>
>

Azure Anahtar Kasası genel bakış bilgileri için bkz. [Azure Anahtar Kasası nedir?](key-vault-whatis.md)

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakilere sahip olmanız gerekir:

* Kullanmakta olduğunuz var olan bir anahtar kasası.  
* Azure PowerShell **1.0.0 en düşük sürümü**. Azure PowerShell'i yüklemek ve Azure aboneliğinizle ilişkilendirmek için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview). Azure PowerShell'i zaten yüklediyseniz ve sürümünü bilmiyorsanız Azure PowerShell konsolunda `$PSVersionTable.PSVersion` yazın.  
* Anahtar Kasası günlükleriniz için Azure'da yeterli depolama.

## <a id="connect"></a>Aboneliklerinize bağlanma

Bir Azure PowerShell oturumu başlatın ve aşağıdaki komutla Azure hesabınızda oturum açın:  

```PowerShell
Connect-AzAccount
```

Açılır tarayıcı penceresinde Azure hesabı kullanıcı adınızı ve parolanızı girin. Azure PowerShell bu hesapla ilişkili tüm abonelikleri alır ve varsayılan olarak birinciyi kullanır.

Birden çok aboneliğiniz varsa Azure Anahtar Kasanızı oluşturmak için kullanılan belirli bir tanesini belirtmeniz gerekebilir. Hesabınız için abonelikleri görmek üzere aşağıdakini yazın:

```PowerShell
    Get-AzSubscription
```

Ardından, günlüğünü tutacağınız anahtar kasasıyla ilişkili aboneliği belirtmek için şunu yazın:

```PowerShell
Set-AzContext -SubscriptionId <subscription ID>
```

> [!NOTE]
> Bu önemli bir adımdır ve hesabınızla ilişkili birden çok abonelik varsa özellikle yararlıdır. Bu adım atlanırsa Microsoft.Insights’ı kaydetme hatası alabilirsiniz.
>   
>

Azure Power Shell'i yapılandırma hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).

## <a id="storage"></a>Günlükleriniz için yeni bir depolama hesabı oluşturma

Günlükleriniz için var olan depolama hesabını kullanabiliyor olsanız da, Anahtar Kasası günlüklerine özgü yeni bir depolama hesabı oluşturacağız. Bunu daha sonra belirtmemiz gerektiğinde kolaylık sağlamak için ayrıntıları **sa** adlı bir değişkende depolayacağız.

Ayrıca, ek yönetim kolaylığı için anahtar kasamızı içeren kaynak grubunu kullanacağız. [Başlangıç öğreticisi](key-vault-overview.md)'nde bu kaynak grubu **ContosoResourceGroup** adına sahiptir ve Doğu Asya konumunu kullanmaya devam edeceğiz. Bunları uygun şekilde kendi değerlerinizle değiştirin:

```PowerShell
 $sa = New-AzStorageAccount -ResourceGroupName ContosoResourceGroup -Name contosokeyvaultlogs -Type Standard_LRS -Location 'East Asia'
```

> [!NOTE]
> Var olan bir depolama hesabını kullanmaya karar verirseniz anahtar kasanızla aynı aboneliği kullanmanız gerekir ve bunun da Klasik dağıtım modeli yerine Resource Manager dağıtım modelini kullanması gerekir.
>
>

## <a id="identify"></a>Günlükleriniz için anahtar kasasını tanımlama

Başlama öğreticimizde anahtar kasamızın adı **ContosoKeyVault**'tu; bu nedenle bu adı kullanmaya ve ayrıntıları **kv** adlı bir değişkende depolamaya devam edeceğiz:

```PowerShell
$kv = Get-AzKeyVault -VaultName 'ContosoKeyVault'
```

## <a id="enable"></a>Günlüğe kaydetmeyi etkinleştirme

Key Vault için günlüğe kaydetmeyi etkinleştirmek için yeni depolama hesabımız ve anahtar kasamız için oluşturduğumuz değişkenleri birlikte Set-AzDiagnosticSetting cmdlet'ini kullanacağız. Ayrıca **-etkin** bayrak **$true** ve kategori AuditEvent (anahtar kasası günlüğü için tek Kategori) olarak ayarlayın:

```PowerShell
Set-AzDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Category AuditEvent
```

Çıkış şuna benzeyecektir:

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

```PowerShell
Set-AzDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Category AuditEvent -RetentionEnabled $true -RetentionInDays 90
```

Günlüğe kaydedilenler:

* Erişim izinleri, sistem hataları veya hatalı istekler sonucunda başarısız olan istekleri içeren tüm kimliği doğrulanmış REST API istekleri günlüğe kaydedilir.
* Oluşturma, silme, anahtar kasası erişim ilkelerini ayarlama ve etiketler gibi anahtar kasası özniteliklerini güncelleştirmeyi kapsayan anahtar kasasının kendisine ait işlemler.
* Anahtar kasasındaki anahtarlar ve gizli dizelere yönelik oluşturma, değiştirme veya silmeyi, anahtarları imzalama, doğrulama, şifreleme, şifrelerini çözme, sarmalama ve kaydırmayı, gizli dizeleri almayı ve anahtarları, gizli dizeleri ve bunların sürümlerini listelemeyi kapsayan işlemler.
* Bir 401 yanıtına neden olan kimliği doğrulanmamış istekler. Örneğin, bir taşıyıcı belirtecine sahip olmayan veya hatalı biçimlendirilmiş ya da süresi dolmuş veya geçersiz bir belirtece sahip olan istekler.  

## <a id="access"></a>Günlüklerinize erişme

Anahtar kasası günlükleri, sağladığınız depolama hesabındaki **insights-logs-auditevent** kapsayıcısında depolanır. Bu kapsayıcıdaki tüm blobları listelemek için şunu yazın:

İlk olarak, kapsayıcı adı için bir değişken oluşturun. Bu gözden geçirme geri kalanı kullanılır.

```PowerShell
$container = 'insights-logs-auditevent'
```

Bu kapsayıcıdaki tüm blobları listelemek için şunu yazın:

```PowerShell
Get-AzStorageBlob -Container $container -Context $sa.Context
```

Çıktı aşağıdakine benzer görünecektir:

```
Container Uri: https://contosokeyvaultlogs.blob.core.windows.net/insights-logs-auditevent

Name

- - -
resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=05/h=01/m=00/PT1H.json

resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=02/m=00/PT1H.json

resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=18/m=00/PT1H.json
```

Bu çıkışta gördüğünüz üzere, bloblar bir adlandırma kuralını izler: **resourceId=<ARM resource ID>/y=<year>/m=<month>/d=<day of month>/h=<hour>/m=<minute>/dosya adı.json**

Tarih ve saat değerleri UTC'yi kullanır.

Aynı depolama hesabında birden fazla kaynak için günlükleri toplamak için kullanılabildiğinden, blob adındaki tam kaynak Kimliğine erişmek veya yalnızca gereksinim duyduğunuz blobları indirmek kullanışlıdır. Ancak bunu yapmadan önce tüm blobların nasıl indirileceğini ele alacağız.

İlk olarak, blobları yüklemek için bir klasör oluşturun. Örneğin:

```PowerShell 
New-Item -Path 'C:\Users\username\ContosoKeyVaultLogs' -ItemType Directory -Force
```

Ardından tüm blobların listesini alın:  

```PowerShell
$blobs = Get-AzStorageBlob -Container $container -Context $sa.Context
```

Bu liste blobları hedef klasörümüze indirmek için 'Get-AzStorageBlobContent' aracılığıyla kanal oluşturun:

```PowerShell
$blobs | Get-AzStorageBlobContent -Destination C:\Users\username\ContosoKeyVaultLogs'
```

Bu ikinci komutu çalıştırdığınızda blob adlarındaki **/** sınırlayıcısı hedef klasörün altında tam bir klasör yapısı oluşturur ve blobları dosya olarak indirmek ve depolamak için bu yapı kullanılır.

Blobları seçmeli olarak indirmek için jokerleri kullanın. Örneğin:

* Birden çok anahtar kasanız varsa ve yalnızca CONTOSOKEYVAULT3 adlı bir anahtar kasası için günlük indirmek isterseniz:

```PowerShell
Get-AzStorageBlob -Container $container -Context $sa.Context -Blob '*/VAULTS/CONTOSOKEYVAULT3
```

* Birden çok kaynak grubunuz varsa ve yalnızca bir kaynak grubu için günlük indirmek isterseniz `-Blob '*/RESOURCEGROUPS/<resource group name>/*'` kullanın:

```PowerShell
Get-AzStorageBlob -Container $container -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'
```

* Ocak 2016 ayı için tüm günlükleri indirmek isterseniz `-Blob '*/year=2016/m=01/*'` kullanın:

```PowerShell
Get-AzStorageBlob -Container $container -Context $sa.Context -Blob '*/year=2016/m=01/*'
```

Artık günlüklerin içinde neler olduğuna bakmaya başlamak için hazırsınız. Ancak, bilmeniz gereken Get-AzDiagnosticSetting için iki parametre daha üzerine taşımadan önce:

* Anahtar kasası kaynağınızın tanılama ayarlarının durumunu sorgulamak için: `Get-AzDiagnosticSetting -ResourceId $kv.ResourceId`
* Anahtar kasası kaynağınızın günlüğe kaydetmesini devre dışı bırakmak için: `Set-AzDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $false -Category AuditEvent`

## <a id="interpret"></a>Anahtar Kasası günlüklerinizi yorumlama

Tek tek bloblar JSON blobu olarak biçimlendirilip metin olarak depolanır. Çalışıyor

```PowerShell
Get-AzKeyVault -VaultName 'contosokeyvault'`
```

bir günlük girişi için aşağıda gösterilene benzer döndürür:

```json
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
```

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
| properties |Bu alan işleme (operationName) bağlı olarak farklı bilgiler içerir. Çoğu durumda, istemci bilgilerini (istemci tarafından geçirilen kullanıcı aracısı dizesi), içerir REST API'SİNİN tam istek URI'sini ve HTTP durum kodu. Buna ek olarak, bir nesne bir istek (örneğin, KeyCreate veya VaultGet) nedeniyle döndürüldüğünde, Anahtar URI'sini ("id" olarak), Kasa URI'sini veya Gizli Anahtar URI'sini de içerir. |

**operationName** alanının değerleri ObjectVerb biçimindedir. Örneğin:

* Tüm anahtar kasası işlemleri `VaultGet` ve `VaultCreate` gibi 'Vault`<action>`' biçimine sahiptir.
* Tüm anahtar işlemleri `KeySign` ve `KeyList` gibi 'Key`<action>`' biçimine sahiptir.
* Tüm gizli anahtar işlemleri `SecretGet` ve `SecretListVersions` gibi 'Secret`<action>`' biçimine sahiptir.

Aşağıdaki tabloda operationName ve karşılık gelen REST API'si komutu listelenmektedir.

| operationName | REST API'si Komutu |
| --- | --- |
| Authentication |Azure Active Directory uç noktası aracılığıyla |
| VaultGet |[Bir anahtar kasası hakkında bilgi edinme](https://msdn.microsoft.com/library/azure/mt620026.aspx) |
| VaultPut |[Anahtar kasası oluşturma veya güncelleştirme](https://msdn.microsoft.com/library/azure/mt620025.aspx) |
| VaultDelete |[Anahtar kasası silme](https://msdn.microsoft.com/library/azure/mt620022.aspx) |
| VaultPatch |[Bir anahtar kasasını güncelleştirme](https://msdn.microsoft.com/library/azure/mt620025.aspx) |
| VaultList |[Bir kaynak grubundaki tüm anahtar kasalarını listeleme](https://msdn.microsoft.com/library/azure/mt620027.aspx) |
| KeyCreate |[Anahtar oluşturma](https://msdn.microsoft.com/library/azure/dn903634.aspx) |
| KeyGet |[Bir anahtar hakkında bilgi edinme](https://msdn.microsoft.com/library/azure/dn878080.aspx) |
| KeyImport |[Bir kasaya anahtar aktarma](https://msdn.microsoft.com/library/azure/dn903626.aspx) |
| KeyBackup |[Bir anahtarı yedekleme](https://msdn.microsoft.com/library/azure/dn878058.aspx). |
| KeyDelete |[Anahtar silme](https://msdn.microsoft.com/library/azure/dn903611.aspx) |
| KeyRestore |[Bir anahtarı geri yükleme](https://msdn.microsoft.com/library/azure/dn878106.aspx) |
| KeySign |[Bir anahtar ile oturum açma](https://msdn.microsoft.com/library/azure/dn878096.aspx) |
| KeyVerify |[Bir anahtar ile doğrulama](https://msdn.microsoft.com/library/azure/dn878082.aspx) |
| KeyWrap |[Bir anahtarı sarmalama](https://msdn.microsoft.com/library/azure/dn878066.aspx) |
| KeyUnwrap |[Bir anahtarı kaydırma](https://msdn.microsoft.com/library/azure/dn878079.aspx) |
| KeyEncrypt |[Bir anahtar ile şifreleme](https://msdn.microsoft.com/library/azure/dn878060.aspx) |
| KeyDecrypt |[Bir anahtar ile şifre çözme](https://msdn.microsoft.com/library/azure/dn878097.aspx) |
| KeyUpdate |[Bir anahtarı güncelleştirme](https://msdn.microsoft.com/library/azure/dn903616.aspx) |
| KeyList |[Bir kasadaki anahtarları listeleme](https://msdn.microsoft.com/library/azure/dn903629.aspx) |
| KeyListVersions |[Bir anahtarın sürümlerini listeleme](https://msdn.microsoft.com/library/azure/dn986822.aspx) |
| SecretSet |[Gizli anahtar oluşturma](https://msdn.microsoft.com/library/azure/dn903618.aspx) |
| SecretGet |[Gizli anahtar alma](https://msdn.microsoft.com/library/azure/dn903633.aspx) |
| SecretUpdate |[Gizli anahtarı güncelleştirme](https://msdn.microsoft.com/library/azure/dn986818.aspx) |
| SecretDelete |[Gizli anahtarı silme](https://msdn.microsoft.com/library/azure/dn903613.aspx) |
| SecretList |[Bir kasadaki gizli anahtarları listeleme](https://msdn.microsoft.com/library/azure/dn903614.aspx) |
| SecretListVersions |[Bir gizli anahtarın sürümlerini listeleme](https://msdn.microsoft.com/library/azure/dn986824.aspx) |

## <a id="loganalytics"></a>Log Analytics'i kullanma

Azure Key Vault AuditEvent günlüklerini incelemek için Log Analytics içindeki Azure Key Vault çözümünü kullanabilirsiniz. Kurulum adımları ve daha fazla bilgi için bkz. [Log Analytics'te Azure Key Vault çözümü](../azure-monitor/insights/azure-key-vault.md). Bu makalede ayrıca Log Analytics önizlemesinde sunulan ve günlüklerinizi önce bir Azure Depolama hesabına yönlendirip Log Analytics'i bu konumdan okuyacak şekilde yapılandırdığınız eski Key Vault çözümünden geçiş yapmak için kullanabileceğiniz talimatlara da yer verilmektedir.

## <a id="next"></a>Sonraki adımlar

Bir .NET web uygulamasında Azure Key Vault kullanan bir öğretici için bkz: [bir Web uygulamasından Azure Key Vault'u](tutorial-net-create-vault-azure-web-app.md).

Programlama başvuruları için bkz. [Azure Anahtar Kasası geliştirici kılavuzu](key-vault-developers-guide.md).

Azure Anahtar Kasası'na yönelik Azure PowerShell 1.0 cmdlet'leri listesi için bkz. [Azure Anahtar Kasası Cmdlet'leri](/powershell/module/az.keyvault/?view=azps-1.2.0#key_vault).

Azure Anahtar Kasası ile anahtar döndürme ve günlük denetimine ilişkin bir öğretici için bkz. [Uçtan uca anahtar döndürme ve denetim ile Anahtar Kasası ayarlama](key-vault-key-rotation-log-monitoring.md).
