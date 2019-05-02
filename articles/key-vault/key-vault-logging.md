---
title: Azure anahtar kasası günlüğü | Microsoft Docs
description: Azure Anahtar Kasası günlüğü ile çalışmaya başlamada yardım almak için bu öğreticiyi kullanın.
services: key-vault
author: barclayn
manager: barbkess
tags: azure-resource-manager
ms.service: key-vault
ms.topic: conceptual
ms.date: 01/18/2019
ms.author: barclayn
ms.openlocfilehash: 89f9ef37ed7c53817854442b3a32b32b7d11ae27
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64706015"
---
# <a name="azure-key-vault-logging"></a>Azure Key Vault günlüğü

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bir veya daha fazla anahtar kasası oluşturduktan sonra büyük olasılıkla izlemek isteyebilirsiniz anahtar kasalarınıza nasıl ve ne zaman erişildiğini ve kim tarafından. Bilgi sağlayan bir Azure depolama hesabında kaydeder Azure Key Vault için günlüğe kaydetmeyi etkinleştirerek bunu yapabilirsiniz. Adlı yeni bir kapsayıcı **insights-logs-auditevent** belirtilen depolama hesabınız için otomatik olarak oluşturulur. Birden çok anahtar kasasının günlüklerini toplamak için bu depolama hesabını kullanabilirsiniz.

Günlük bilgilerinize 10 dakika (en fazla) anahtar kasası işleminden sonra erişebilirsiniz. Çoğu durumda, bundan daha hızlı olacaktır.  Depolama hesabınızdaki günlüklerinizi yönetmek size bağlıdır:

* Günlüklerinize erişebilecek kişileri kısıtlayarak güvenliklerini sağlamak için standart Azure erişim denetimi yöntemlerini kullanın.
* Artık depolama hesabınızda tutmak istemediğiniz günlükleri silin.

Azure Anahtar Kasası günlüğü ile çalışmaya başlamada yardım almak için bu öğreticiyi kullanın. Depolama hesabı oluşturma, günlük kaydını etkinleştirmek ve toplanan günlük bilgilerini yorumlamak.  

> [!NOTE]
> Bu öğretici anahtar kasalarının, anahtarların veya gizli anahtarların nasıl oluşturulacağı hakkında yönergeler içermez. Bu bilgi için bkz. [Azure anahtar kasası nedir?](key-vault-overview.md). Veya platformlar arası Azure CLI yönergeleri için bkz. [bu eşdeğer öğreticiye](key-vault-manage-with-cli2.md).
>
> Bu makalede tanılama günlüğünün güncelleştirmek için Azure PowerShell yönergelerini sağlar. Azure İzleyici'de kullanarak da tanılama günlük güncelleştirebilirsiniz **tanılama günlükleri** Azure portal'ın bölümü. 
>

Key Vault hakkında genel bilgi için bkz. [Azure anahtar kasası nedir?](key-vault-whatis.md). Key Vault kullanılabilir olduğu hakkında daha fazla bilgi için bkz: [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakilere sahip olmanız gerekir:

* Kullanmakta olduğunuz var olan bir anahtar kasası.  
* Azure PowerShell, 1.0.0 en düşük sürümü. Azure PowerShell'i yüklemek ve Azure aboneliğinizle ilişkilendirmek için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview). Azure PowerShell'i zaten yüklediyseniz ve sürümünü Azure PowerShell konsolundan bilmediğiniz girin `$PSVersionTable.PSVersion`.  
* Anahtar Kasası günlükleriniz için Azure'da yeterli depolama.

## <a id="connect"></a>Anahtar kasası aboneliğinize bağlanma

Noktası Azure PowerShell günlüğe kaydetmek istediğiniz bir anahtar kasasına bir anahtar günlüğünü ayarlama ilk adımı sağlamaktır.

Azure PowerShell oturumu başlatın ve aşağıdaki komutu kullanarak Azure hesabınızda oturum açın:  

```powershell
Connect-AzAccount
```

Açılır tarayıcı penceresinde Azure hesabı kullanıcı adınızı ve parolanızı girin. Azure PowerShell Bu hesapla ilişkili tüm abonelikleri alır. PowerShell, varsayılan olarak birinciyi kullanır.

Anahtar kasanızı oluşturmak için kullanılan abonelik belirtmeniz gerekebilir. Hesabınız için abonelikleri görmek için aşağıdaki komutu girin:

```powershell
Get-AzSubscription
```

Ardından, günlüğe kaydedilmesi anahtar kasasıyla ilişkili aboneliği belirtmek için şunu girin:

```powershell
Set-AzContext -SubscriptionId <subscription ID>
```

Özellikle hesabınızla ilişkili birden fazla aboneliğiniz varsa doğru aboneliği için PowerShell işaret eden bir önemli adımıdır. Azure Power Shell'i yapılandırma hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).

## <a id="storage"></a>Günlükleriniz için depolama hesabı oluşturma

Günlükleriniz için var olan bir depolama hesabını kullanmanız mümkün olmakla birlikte, anahtar kasası günlükleri için ayrılmış bir depolama hesabı oluşturacağız. Biz bunu daha sonra belirtmeyi olduğunda kolaylık sağlamak için şu ayrıntıları adlı bir değişkende depolayacağınızı **sa**.

Ek yönetim kolaylığı için Ayrıca aynı kaynak grubunda anahtar kasasını içeren bir kullanacağız. Gelen [başlangıç Öğreticisi](key-vault-get-started.md), bu kaynak grubunun adı **ContosoResourceGroup**, Doğu Asya konumunu kullanmaya devam. Bu değerleri uygun şekilde kendi ile değiştirin:

```powershell
 $sa = New-AzStorageAccount -ResourceGroupName ContosoResourceGroup -Name contosokeyvaultlogs -Type Standard_LRS -Location 'East Asia'
```

> [!NOTE]
> Mevcut bir depolama hesabı kullanmaya karar verirseniz anahtar kasanızla aynı aboneliği kullanmanız gerekir. Ve klasik dağıtım modeli yerine, Azure Resource Manager dağıtım modelini kullanması gerekir.
>
>

## <a id="identify"></a>Günlükleriniz için anahtar kasasını tanımlama

İçinde [başlangıç Öğreticisi](key-vault-get-started.md), anahtar kasamızın adı **ContosoKeyVault**. Biz bu adı kullanmaya ve ayrıntıları adlı bir değişkende depolayın eklemeye devam edeceğiz **kv**:

```powershell
$kv = Get-AzKeyVault -VaultName 'ContosoKeyVault'
```

## <a id="enable"></a>Günlüğe kaydetmeyi etkinleştirme

Key Vault için günlüğe kaydetmeyi etkinleştirmek için kullanacağız **kümesi AzDiagnosticSetting** cmdlet'i, yeni depolama hesabını ve anahtar kasası için oluşturduğumuz değişkenleri birlikte. Ayrıca **-etkin** bayrak **$true** ve kategorisini ayarlayın **AuditEvent** (anahtar kasası günlüğü için tek Kategori):

```powershell
Set-AzDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Category AuditEvent
```

Çıktı şuna benzer:

    StorageAccountId   : /subscriptions/<subscription-GUID>/resourceGroups/ContosoResourceGroup/providers/Microsoft.Storage/storageAccounts/ContosoKeyVaultLogs
    ServiceBusRuleId   :
    StorageAccountName :
        Logs
        Enabled           : True
        Category          : AuditEvent
        RetentionPolicy
        Enabled : False
        Days    : 0

Bu çıkış, günlük kaydı artık anahtar kasanız için etkindir ve depolama hesabınıza bilgileri kaydedecek onaylar.

İsteğe bağlı olarak, eski günlüklerin otomatik olarak silinmesi gerektiğini günlükleriniz için bekletme ilkesi ayarlayabilirsiniz. Örneğin, bekletme ilkesi ayarlamak için **- RetentionEnabled** bayrak **$true**, ayarlayıp **- Retentionındays** parametresi **90**böylece 90 günden eski olan günlükler otomatik olarak silinir.

```powershell
Set-AzDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Category AuditEvent -RetentionEnabled $true -RetentionInDays 90
```

Günlüğe kaydedilenler:

* Tüm erişim izinleri, sistem hataları veya hatalı istekler sonucunda başarısız istekleri dahil olmak üzere REST API isteği kimliği.
* Anahtar işlemleri kendisi, oluşturma, silme, anahtar kasası erişim ilkeleri ayarlama dahil olmak üzere ve etiketler gibi anahtar kasası özniteliklerini güncelleştirmeyi kasası.
* Anahtarlar ve gizli anahtarları key vault'ta dahil olmak üzere:
  * Oluşturma, değiştirme veya bu anahtarları ve gizli anahtarları silme.
  * İmzalama, doğrulama, şifreleme, şifre çözme, sarmalama ve açma anahtarları, gizli ve listeleme anahtarlara ve parolalara (ve bunların sürümlerini) alma.
* Bir 401 yanıtına neden olan kimliği doğrulanmamış istekler. Hatalı biçimlendirilmiş ya da süresi dolmuş veya geçersiz bir belirtece sahip olan bir taşıyıcı belirtecine sahip olmayan isteklerinin verilebilir.  

## <a id="access"></a>Günlüklerinize erişme

Anahtar kasası günlükleri depolanır **insights-logs-auditevent** , sağladığınız depolama hesabındaki kapsayıcı. Günlükleri görüntülemek için BLOB indirmek zorunda.

İlk olarak, kapsayıcı adı için bir değişken oluşturun. Gözden geçirme geri kalanında bu değişken kullanacaksınız.

```powershell
$container = 'insights-logs-auditevent'
```

Bu kapsayıcıdaki tüm blobları listelemek için şunu girin:

```powershell
Get-AzStorageBlob -Container $container -Context $sa.Context
```

Çıkış şuna benzer:

```
Container Uri: https://contosokeyvaultlogs.blob.core.windows.net/insights-logs-auditevent

Name

- - -
resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=05/h=01/m=00/PT1H.json

resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=02/m=00/PT1H.json

resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=18/m=00/PT1H.json
```

Bu Çıkışta gördüğünüz gibi bloblar bir adlandırma kuralını izler: `resourceId=<ARM resource ID>/y=<year>/m=<month>/d=<day of month>/h=<hour>/m=<minute>/filename.json`

Tarih ve saat değerleri UTC'yi kullanır.

Birden fazla kaynak için günlükleri toplamak için aynı depolama hesabını kullanabileceğinizden, blob adındaki tam kaynak Kimliğine erişmek veya yalnızca gereksinim duyduğunuz blobları indirmek kullanışlıdır. Ancak bunu yapmadan önce tüm blobların nasıl indirileceğini ele alacağız.

Blobları indirmek için bir klasör oluşturun. Örneğin:

```powershell 
New-Item -Path 'C:\Users\username\ContosoKeyVaultLogs' -ItemType Directory -Force
```

Ardından tüm blobların listesini alın:  

```powershell
$blobs = Get-AzStorageBlob -Container $container -Context $sa.Context
```

Bu listede kanal **Get-AzStorageBlobContent** blobları hedef klasöre yüklemek için:

```powershell
$blobs | Get-AzStorageBlobContent -Destination C:\Users\username\ContosoKeyVaultLogs'
```

Bu ikinci komutu çalıştırdığınızda **/** blob adlarındaki sınırlayıcısı hedef klasörün altında tam klasör yapısı oluşturur. Bu yapı, indirmek ve blobları dosya olarak depolamak için kullanacaksınız.

Blobları seçmeli olarak indirmek için jokerleri kullanın. Örneğin:

* Birden çok anahtar kasanız varsa ve yalnızca CONTOSOKEYVAULT3 adlı bir anahtar kasası için günlük indirmek isterseniz:

  ```powershell
  Get-AzStorageBlob -Container $container -Context $sa.Context -Blob '*/VAULTS/CONTOSOKEYVAULT3
  ```

* Birden çok kaynak grubunuz varsa ve yalnızca bir kaynak grubu için günlük indirmek isterseniz `-Blob '*/RESOURCEGROUPS/<resource group name>/*'` kullanın:

  ```powershell
  Get-AzStorageBlob -Container $container -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'
  ```

* Ocak 2019 ayı için tüm günlükleri indirmek istiyorsanız, kullanmanız `-Blob '*/year=2019/m=01/*'`:

  ```powershell
  Get-AzStorageBlob -Container $container -Context $sa.Context -Blob '*/year=2016/m=01/*'
  ```

Artık günlüklerin içinde neler olduğuna bakmaya başlamak için hazırsınız. Ancak, iki daha fazla komut şu şekilde geçmeden önce bilmeniz gerekenler:

* Anahtar kasası kaynağınızın tanılama ayarlarının durumunu sorgulamak için: `Get-AzDiagnosticSetting -ResourceId $kv.ResourceId`
* Anahtar kasası kaynağınızın günlüğe kaydetmesini devre dışı bırakmak için: `Set-AzDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $false -Category AuditEvent`

## <a id="interpret"></a>Anahtar Kasası günlüklerinizi yorumlama

Tek tek bloblar JSON blobu olarak biçimlendirilip metin olarak depolanır. Bir örnek günlük girişi sırasında göz atalım. Şu komutu çalıştırın:

```powershell
Get-AzKeyVault -VaultName 'contosokeyvault'`
```

Bir günlük girişi buna benzer döndürür:

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

Aşağıdaki tabloda alan adları ve açıklamaları listelenmektedir:

| Alan adı | Açıklama |
| --- | --- |
| **saat** |Tarih ve UTC diliminde saat. |
| **resourceId** |Azure Resource Manager kaynak kimliği. Anahtar kasası günlükleri için bu her zaman anahtar kasası kaynak kimliğidir. |
| **OperationName** |Sonraki tabloda belirtildiği gibi işlemin adı. |
| **operationVersion** |İstemci tarafından istenen REST API sürümü. |
| **Kategori** |Sonuç türü. Anahtar kasası günlükleri için **AuditEvent** kullanılabilir, tek bir değerdir. |
| **resulttype'ı** |REST API'si isteğinin sonucu. |
| **resultSignature** |HTTP durumu. |
| **ResultDescription** |Kullanılabilir olduğunda sonuç hakkında ek açıklama. |
| **süre (MS)** |Milisaniye cinsinden REST API'si isteğini sunmak için geçen süre. Buna ağ gecikme süresi dahil değildir; bu nedenle istemci tarafında ölçtüğünüz süre bu süreyle eşleşmeyebilir. |
| **callerIpAddress** |İsteği gerçekleştiren istemcinin IP adresi. |
| **Bağıntı Kimliği** |İstemci tarafı günlüklerini hizmet tarafı (Anahtar Kasası) günlükleriyle ilişkilendirmek için istemcinin geçirebileceği isteğe bağlı bir GUID. |
| **Kimlik** |REST API isteği sunulan belirteçten kimliği. Bu genellikle "kullanıcı" olan bir "hizmet sorumlusu" veya bir Azure PowerShell cmdlet'inden sonuçları bir istek olduğu gibi "kullanıcı + uygulama kimliği," birleşimi. |
| **Özellikleri** |Bağlı olarak değişir bilgi işlemi (**operationName**). Çoğu durumda, bu alan istemci bilgilerini (istemci tarafından geçirilen kullanıcı aracısı dizesi), REST API'SİNİN tam istek URI'sini ve HTTP durum kodunu içerir. Ayrıca, bir nesne döndürüldüğünde bir istek (örneğin, **KeyCreate** veya **VaultGet**), anahtar da içerir ("id") olarak URI kasa URI'sini veya gizli bir URI. |

**OperationName** alan değerleri olan *ObjectVerb* biçimi. Örneğin:

* Tüm anahtar kasası işlemleri sahip `Vault<action>` gibi biçimlendirme `VaultGet` ve `VaultCreate`.
* Tüm anahtar işlemleri sahip `Key<action>` gibi biçimlendirme `KeySign` ve `KeyList`.
* Tüm gizli anahtar işlemleri sahip `Secret<action>` gibi biçimlendirme `SecretGet` ve `SecretListVersions`.

Aşağıdaki tabloda **operationName** değerler ve ilgili REST API komutları:

| operationName | REST API'si komutu |
| --- | --- |
| **Kimlik doğrulaması** |Azure Active Directory uç noktası aracılığıyla kimlik doğrulaması |
| **VaultGet** |[Bir anahtar kasası hakkında bilgi edinme](https://msdn.microsoft.com/library/azure/mt620026.aspx) |
| **VaultPut** |[Anahtar kasası oluşturma veya güncelleştirme](https://msdn.microsoft.com/library/azure/mt620025.aspx) |
| **VaultDelete** |[Anahtar kasası silme](https://msdn.microsoft.com/library/azure/mt620022.aspx) |
| **VaultPatch** |[Bir anahtar kasasını güncelleştirme](https://msdn.microsoft.com/library/azure/mt620025.aspx) |
| **VaultList** |[Bir kaynak grubundaki tüm anahtar kasalarını listeleme](https://msdn.microsoft.com/library/azure/mt620027.aspx) |
| **KeyCreate** |[Anahtar oluşturma](https://msdn.microsoft.com/library/azure/dn903634.aspx) |
| **KeyGet** |[Bir anahtar hakkında bilgi edinme](https://msdn.microsoft.com/library/azure/dn878080.aspx) |
| **KeyImport** |[Bir kasaya anahtar aktarma](https://msdn.microsoft.com/library/azure/dn903626.aspx) |
| **KeyBackup** |[Anahtarını yedekleyin](https://msdn.microsoft.com/library/azure/dn878058.aspx) |
| **KeyDelete** |[Anahtar silme](https://msdn.microsoft.com/library/azure/dn903611.aspx) |
| **KeyRestore** |[Bir anahtarı geri yükleme](https://msdn.microsoft.com/library/azure/dn878106.aspx) |
| **KeySign** |[Bir anahtar ile oturum açma](https://msdn.microsoft.com/library/azure/dn878096.aspx) |
| **KeyVerify** |[Bir anahtar ile doğrulama](https://msdn.microsoft.com/library/azure/dn878082.aspx) |
| **KeyWrap** |[Bir anahtarı sarmalama](https://msdn.microsoft.com/library/azure/dn878066.aspx) |
| **KeyUnwrap** |[Bir anahtarı kaydırma](https://msdn.microsoft.com/library/azure/dn878079.aspx) |
| **KeyEncrypt** |[Bir anahtar ile şifreleme](https://msdn.microsoft.com/library/azure/dn878060.aspx) |
| **KeyDecrypt** |[Bir anahtar ile şifre çözme](https://msdn.microsoft.com/library/azure/dn878097.aspx) |
| **KeyUpdate** |[Bir anahtarı güncelleştirme](https://msdn.microsoft.com/library/azure/dn903616.aspx) |
| **KeyList** |[Bir kasadaki anahtarları listeleme](https://msdn.microsoft.com/library/azure/dn903629.aspx) |
| **KeyListVersions** |[Bir anahtarın sürümlerini listeleme](https://msdn.microsoft.com/library/azure/dn986822.aspx) |
| **SecretSet** |[Gizli anahtar oluşturma](https://msdn.microsoft.com/library/azure/dn903618.aspx) |
| **SecretGet** |[Gizli dizi alma](https://msdn.microsoft.com/library/azure/dn903633.aspx) |
| **SecretUpdate** |[Gizli anahtarı güncelleştirme](https://msdn.microsoft.com/library/azure/dn986818.aspx) |
| **SecretDelete** |[Gizli anahtarı silme](https://msdn.microsoft.com/library/azure/dn903613.aspx) |
| **SecretList** |[Bir kasadaki gizli anahtarları listeleme](https://msdn.microsoft.com/library/azure/dn903614.aspx) |
| **SecretListVersions** |[Bir gizli anahtarın sürümlerini listeleme](https://msdn.microsoft.com/library/azure/dn986824.aspx) |

## <a id="loganalytics"></a>Azure İzleyici günlüklerine kullanın

Key Vault gözden geçirmek için Azure İzleyici günlüklerine Key Vault çözümünü kullanabilirsiniz **AuditEvent** günlükleri. Azure İzleyici günlüklerine verileri analiz etmek ve ihtiyacınız olan bilgileri almak için günlük sorguları kullanın. 

Bu, nasıl ayarlanacağı dahil olmak üzere daha fazla bilgi için bkz. [Azure Key Vault çözümü Azure İzleyici günlüklerine](../azure-monitor/insights/azure-key-vault.md). Azure İzleyici günlüklerine sırasında önizleme, Azure depolama hesabınız için günlüklerinizi ilk yönlendirilen burada ve konumdan okuyacak şekilde yapılandırılmış Azure İzleyici günlüklerini sunulan eski Key Vault çözümünden geçmeniz gerekiyorsa bu makalede yönergeler de içerir.

## <a id="next"></a>Sonraki adımlar

Bir .NET web uygulamasında Azure Key Vault kullanan bir öğretici için bkz: [bir web uygulamasından Azure Key Vault'u](tutorial-net-create-vault-azure-web-app.md).

Programlama başvuruları için bkz. [Azure Anahtar Kasası geliştirici kılavuzu](key-vault-developers-guide.md).

Azure Key Vault için Azure PowerShell 1.0 cmdlet'leri listesi için bkz. [Azure anahtar kasası cmdlet'leri](/powershell/module/az.keyvault/?view=azps-1.2.0#key_vault).

Anahtar değiştirme ve Azure anahtar kasası ile günlük denetimi hakkında bir öğretici için bkz. [uçtan uca anahtar döndürme ve Denetim ile anahtar kasası ayarlama](key-vault-key-rotation-log-monitoring.md).
