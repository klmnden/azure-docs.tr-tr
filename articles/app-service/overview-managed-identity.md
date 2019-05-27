---
title: Kimlikleri genel bakış - Azure App Service yönetilen | Microsoft Docs
description: Azure App Service ve Azure işlevleri yönetilen kimlikleri için kavramsal başvurusu ve Kurulum Kılavuzu
services: app-service
author: mattchenderson
manager: cfowler
editor: ''
ms.service: app-service
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/20/2018
ms.author: mahender
ms.openlocfilehash: 0942d5ba7b31ddb2c0dec5fe979f1331d1bf3bfd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66136957"
---
# <a name="how-to-use-managed-identities-for-app-service-and-azure-functions"></a>App Service ve Azure işlevleri için yönetilen kimliklerini kullanma

> [!NOTE] 
> App Service'in Linux'ta ve kapsayıcılar için Web Apps için yönetilen kimlik desteği şu anda Önizleme aşamasındadır.

> [!Important] 
> App Service ve Azure işlevleri için yönetilen kimlikleri, uygulamanızı abonelikleri/kiracılar genelinde geçirdiyseniz beklendiği gibi davranmaz. Uygulamayı devre dışı bırakıp yeniden özelliğini etkinleştirerek yapılabilir yeni bir kimliği edinmeniz gerekir. Bkz: [Kimlikteki kaldırma](#remove) aşağıda. Aşağı Akış kaynakları da erişim ilkelerini yeni bir kimlik kullanacak şekilde güncelleştirilmiş olması gerekir.

Bu konuda, App Service ve Azure işlevleri uygulamaları için yönetilen bir kimlik oluşturma ve diğer kaynaklarına erişmek için kullanma gösterilmektedir. Azure Key Vault gibi diğer AAD korumalı kaynakları kolayca erişmek için uygulamanızı Azure Active Directory'den yönetilen bir kimlik sağlar. Kimlik Azure platformu tarafından yönetilir ve sağlama veya herhangi bir gizli anahtar döndürme gerektirmez. Aad'de yönetilen kimlikleri hakkında daha fazla bilgi için bkz. [kimliklerini Azure kaynakları için yönetilen](../active-directory/managed-identities-azure-resources/overview.md).

Uygulamanızı kimlikler iki tür verilebilir: 
- A **sistem tarafından atanan kimlik** uygulamanıza bağlıdır ve uygulamanızı silinirse silinir. Bir uygulama yalnızca tek bir sistem tarafından atanan kimlik olabilir. Sistem tarafından atanan kimlik desteği, Windows uygulamaları için genel kullanıma sunulmuştur. 
- A **kullanıcı tarafından atanan kimlik** tek başına uygulamanıza atanan Azure kaynağı olduğundan. Bir uygulamanın birden çok kullanıcı tarafından atanan kimlik olabilir. Kullanıcı tarafından atanan kimlik desteği, tüm uygulama türleri için önizlemeye sunulmuştur.

## <a name="adding-a-system-assigned-identity"></a>Sistem tarafından atanan bir kimlik ekleniyor

Sistem tarafından atanan bir kimlikle bir uygulama oluşturmak, uygulama üzerinde ayarlamak için ek bir özellik gerekir.

### <a name="using-the-azure-portal"></a>Azure portalını kullanma

Portalda yönetilen bir kimlik ayarlamak için ilk olarak normal bir uygulama oluşturun ve ardından özelliği etkinleştirmek.

1. Normalde yaptığınız gibi Portalı'nda bir uygulama oluşturun. İçin portalda gidin.

2. Bir işlev uygulaması kullanıyorsanız gidin **Platform özellikleri**. Diğer uygulama türleri için aşağı kaydırarak **ayarları** sol gezinti grubu.

3. Seçin **yönetilen kimliği**.

4. İçinde **sistem tarafından atanan** sekmesinde, geçiş **durumu** için **üzerinde**. **Kaydet**’e tıklayın.

![App Service içindeki yönetilen kimlik](media/app-service-managed-service-identity/msi-blade-system.png)

### <a name="using-the-azure-cli"></a>Azure CLI kullanma

Azure CLI kullanarak bir yönetilen kimlik için kullanmanız gerekecektir `az webapp identity assign` mevcut bir uygulamaya göre komutu. Bu bölümdeki örnekler çalıştırmak için üç seçeneğiniz vardır:

- Kullanım [Azure Cloud Shell](../cloud-shell/overview.md) Azure portalından.
- Katıştırılmış Azure Cloud Shell aracılığıyla her kod bloğunun sağ üst köşesinde bulunan "Try It" düğmesini kullanın.
- [Azure CLI'ın en son sürümü yükleyin](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.31 veya üzeri) yerel bir CLI konsol kullanmak istiyorsanız. 

Aşağıdaki adımlar, bir web uygulaması oluşturma ve CLI kullanarak bir kimlik atama anlatılmaktadır:

1. Azure CLI'yi yerel bir konsolda kullanıyorsanız, önce [az login](/cli/azure/reference-index#az-login) kullanarak Azure'da oturum açın. Uygulamayı dağıtmak altında istediğiniz Azure aboneliği ile ilişkili olan bir hesabı kullanın:

    ```azurecli-interactive
    az login
    ```
2. CLI kullanarak bir web uygulaması oluşturun. App Service ile CLI'yı kullanma konusunda daha fazla örnek için bkz: [App Service CLI örnekleri](../app-service/samples-cli.md):

    ```azurecli-interactive
    az group create --name myResourceGroup --location westus
    az appservice plan create --name myPlan --resource-group myResourceGroup --sku S1
    az webapp create --name myApp --resource-group myResourceGroup --plan myPlan
    ```

3. Çalıştırma `identity assign` komutu bu uygulama için kimlik oluşturmak için:

    ```azurecli-interactive
    az webapp identity assign --name myApp --resource-group myResourceGroup
    ```

### <a name="using-azure-powershell"></a>Azure PowerShell’i kullanma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Aşağıdaki adımlar, bir web uygulaması oluşturma ve Azure PowerShell kullanarak bir kimlik atama anlatılmaktadır:

1. Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/overview) bulunan yönergeleri kullanarak Azure PowerShell’i yükleyin ve ardından Azure ile bağlantı oluşturmak için `Login-AzAccount` komutunu çalıştırın.

2. Azure PowerShell kullanarak bir web uygulaması oluşturun. App Service ile Azure PowerShell'i kullanma konusunda daha fazla örnek için bkz: [App Service PowerShell örnekleri](../app-service/samples-powershell.md):

    ```azurepowershell-interactive
    # Create a resource group.
    New-AzResourceGroup -Name myResourceGroup -Location $location
    
    # Create an App Service plan in Free tier.
    New-AzAppServicePlan -Name $webappname -Location $location -ResourceGroupName myResourceGroup -Tier Free
    
    # Create a web app.
    New-AzWebApp -Name $webappname -Location $location -AppServicePlan $webappname -ResourceGroupName myResourceGroup
    ```

3. Çalıştırma `Set-AzWebApp -AssignIdentity` komutu bu uygulama için kimlik oluşturmak için:

    ```azurepowershell-interactive
    Set-AzWebApp -AssignIdentity $true -Name $webappname -ResourceGroupName myResourceGroup 
    ```

### <a name="using-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu kullanma

Bir Azure Resource Manager şablonu kullanarak Azure kaynaklarınızı dağıtımını otomatik hale getirmek için kullanılabilir. App Service ve İşlevler için dağıtma hakkında daha fazla bilgi edinmek için [App Service'te kaynak dağıtımını otomatikleştirme](../app-service/deploy-complex-application-predictably.md) ve [Azure işlevleri'nde kaynak dağıtımını otomatikleştirme](../azure-functions/functions-infrastructure-as-code.md).

Herhangi bir kaynak türü `Microsoft.Web/sites` kaynak tanımı'nda aşağıdaki özellikler dahil olmak üzere bir kimlikle oluşturulabilir:
```json
"identity": {
    "type": "SystemAssigned"
}    
```

> [!NOTE] 
> Bir uygulamanın aynı anda hem sistem tarafından atanan ve kullanıcı tarafından atanan kimlikleri olabilir. Bu durumda, `type` özelliği olacaktır `SystemAssigned,UserAssigned`

Sistem tarafından atanan türü ekleme oluşturmak ve uygulamanız için kimlik yönetimi için Azure söyler.

Örneğin, bir web uygulaması aşağıdakine benzeyebilir:
```json
{
    "apiVersion": "2016-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('appName')]",
    "location": "[resourceGroup().location]",
    "identity": {
        "type": "SystemAssigned"
    },
    "properties": {
        "name": "[variables('appName')]",
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "hostingEnvironment": "",
        "clientAffinityEnabled": false,
        "alwaysOn": true
    },
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"
    ]
}
```

Site oluşturulduğunda, aşağıdaki ek özellikler vardır:
```json
"identity": {
    "type": "SystemAssigned",
    "tenantId": "<TENANTID>",
    "principalId": "<PRINCIPALID>"
}
```

Burada `<TENANTID>` ve `<PRINCIPALID>` GUID'lerini aşağıdaki ile değiştirilir. AAD kiracısının kimliği ait ne Tenantıd özelliği tanımlar. Principalıd, uygulamanın yeni kimlik için benzersiz bir tanımlayıcıdır. AAD içinde hizmet sorumlusu, App Service veya Azure işlevleri Örneğinize verdiğiniz aynı ada sahip.


## <a name="adding-a-user-assigned-identity-preview"></a>Bir kullanıcı tarafından atanan kimliği (Önizleme) ekleme

> [!NOTE] 
> Kullanıcı tarafından atanan kimlikleri, şu anda Önizleme aşamasındadır. Bağımsız bulutlarda henüz desteklenmemektedir.

Uygulama bir kullanıcı tarafından atanan kimliği oluşturma, kimlik oluşturmak ve ardından, uygulama yapılandırması için kaynak tanımlayıcısını ekleyin gerektirir.

### <a name="using-the-azure-portal"></a>Azure portalını kullanma

> [!NOTE] 
> Bu portal deneyimi, dağıtılmakta ve henüz tüm bölgelerde kullanılamayabilir.

İlk olarak, bir kullanıcı tarafından atanan kimlik kaynağı oluşturmanız gerekir.

1. Bir kullanıcı tarafından atanan yönetilen kimlik kaynağı göre oluşturun [bu yönergeleri](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity).

2. Normalde yaptığınız gibi Portalı'nda bir uygulama oluşturun. İçin portalda gidin.

3. Bir işlev uygulaması kullanıyorsanız gidin **Platform özellikleri**. Diğer uygulama türleri için aşağı kaydırarak **ayarları** sol gezinti grubu.

4. Seçin **yönetilen kimliği**.

5. İçinde **kullanıcı (Önizleme) atanmış** sekmesinde **Ekle**.

6. Daha önce oluşturduğunuz kimlik için arama yapın ve seçin. **Ekle**'yi tıklatın.

![App Service içindeki yönetilen kimlik](media/app-service-managed-service-identity/msi-blade-user.png)

### <a name="using-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu kullanma

Bir Azure Resource Manager şablonu kullanarak Azure kaynaklarınızı dağıtımını otomatik hale getirmek için kullanılabilir. App Service ve İşlevler için dağıtma hakkında daha fazla bilgi edinmek için [App Service'te kaynak dağıtımını otomatikleştirme](../app-service/deploy-complex-application-predictably.md) ve [Azure işlevleri'nde kaynak dağıtımını otomatikleştirme](../azure-functions/functions-infrastructure-as-code.md).

Herhangi bir kaynak türü `Microsoft.Web/sites` kaynak tanımı'nda aşağıdaki bloğu dahil olmak üzere bir kimlikle oluşturulabilir değiştirerek `<RESOURCEID>` istenen kimlik kaynak kimliği:
```json
"identity": {
    "type": "UserAssigned",
    "userAssignedIdentities": {
        "<RESOURCEID>": {}
    }
}    
```

> [!NOTE] 
> Bir uygulamanın aynı anda hem sistem tarafından atanan ve kullanıcı tarafından atanan kimlikleri olabilir. Bu durumda, `type` özelliği olacaktır `SystemAssigned,UserAssigned`

Kullanıcı tarafından atanan türü ekleme ve bir oluşturmak ve uygulamanız için kimlik yönetimi için Azure cotells.

Örneğin, bir web uygulaması aşağıdakine benzeyebilir:
```json
{
    "apiVersion": "2016-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('appName')]",
    "location": "[resourceGroup().location]",
    "identity": {
        "type": "UserAssigned",
        "userAssignedIdentities": {
            "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', variables('identityName'))]": {}
        }
    },
    "properties": {
        "name": "[variables('appName')]",
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "hostingEnvironment": "",
        "clientAffinityEnabled": false,
        "alwaysOn": true
    },
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', variables('identityName'))]"
    ]
}
```

Site oluşturulduğunda, aşağıdaki ek özellikler vardır:
```json
"identity": {
    "type": "UserAssigned",
    "userAssignedIdentities": {
        "<RESOURCEID>": {
            "principalId": "<PRINCIPALID>",
            "clientId": "<CLIENTID>"
        }
    }
}
```

Burada `<PRINCIPALID>` ve `<CLIENTID>` GUID'lerini aşağıdaki ile değiştirilir. Principalıd AAD Yönetim için kullanılan kimliği için benzersiz bir tanımlayıcıdır. ClientID, uygulamanın çalışma zamanı çağrılar sırasında kullanılacak kimliği belirtmek için kullanılan yeni kimlik için benzersiz bir tanımlayıcıdır.


## <a name="obtaining-tokens-for-azure-resources"></a>Azure kaynakları için belirteç edinme

Uygulama kimliğini, Azure Key Vault gibi bir AAD tarafından korunan diğer kaynaklara belirteçlerini almak için kullanabilirsiniz. Bu belirteçler kaynağı ve belirli hiçbir kullanıcı uygulamanın erişen uygulamada temsil eder. 

> [!IMPORTANT]
> Hedef kaynak, uygulamanızdan erişime izin verecek şekilde yapılandırmanız gerekebilir. Örneğin, anahtar kasası için bir belirteç isteği, uygulamanızın kimliğini içeren bir erişim ilkesi eklediğinizden emin olun gerekir. Belirteç içeriyorsa bile Aksi takdirde, aramalarınız için Key Vault, reddedilir. Azure Active Directory belirteçleri daha destek hangi kaynakları hakkında bilgi edinmek için [Azure Hizmetleri söz konusu destek Azure AD kimlik doğrulamasını](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication).

App Service ve Azure işlevleri, bir belirteç almak için basit bir REST protokolü yoktur. .NET uygulamaları için Microsoft.Azure.Services.AppAuthentication kitaplığını, bu protokolü üzerinden bir Özet sağlar ve bir yerel geliştirme deneyimini destekler.

### <a name="asal"></a>.NET için Microsoft.Azure.Services.AppAuthentication kitaplığını kullanma

.NET uygulamaları ve işlevleri için yönetilen bir kimlik ile çalışmak için en basit yolu Microsoft.Azure.Services.AppAuthentication paketidir. Bu kitaplık, Visual Studio, kullanıcı hesabını kullanarak yerel olarak geliştirme makinenizde, kodunuzu test etmek de sağlayacak [Azure CLI](/cli/azure), ya da Active Directory tümleşik kimlik doğrulaması. Bu kitaplığı ile yerel geliştirme seçenekleri hakkında daha fazla bilgi için bkz. [Microsoft.Azure.Services.AppAuthentication başvurusu]. Bu bölümde, kitaplığı kodunuza kullanmaya başlama işlemini göstermektedir.

1. Başvuruları Ekle [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) ve tüm diğer gerekli NuGet paketlerini uygulamanıza. Aşağıdaki örnekte de kullanır [Microsoft.Azure.KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault).

2. Aşağıdaki kodu doğru kaynağı hedeflemek için değiştirme uygulamanıza ekleyin. Bu örnek, Azure anahtar kasası ile çalışmak için iki yolunu gösterir:

```csharp
using Microsoft.Azure.Services.AppAuthentication;
using Microsoft.Azure.KeyVault;
// ...
var azureServiceTokenProvider = new AzureServiceTokenProvider();
string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync("https://vault.azure.net");
// OR
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider.KeyVaultTokenCallback));
```

Microsoft.Azure.Services.AppAuthentication ve kullanıma sunduğu işlemleri hakkında daha fazla bilgi için bkz: [Microsoft.Azure.Services.AppAuthentication başvurusu] ve [App Service ve Azure anahtar kasası MSI .NET ile örnek](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet).

### <a name="using-the-rest-protocol"></a>REST protokolü kullanarak

Yönetilen bir kimlik ile bir uygulama tanımlı iki ortam değişkenleri vardır:

- MSI_ENDPOINT - yerel belirteç Hizmeti'ne URL.
- MSI_SECRET - sunucu tarafı istek sahteciliği (SSRF) saldırılarını önlemeye yardımcı olmak için kullanılan üstbilgi. Değer, platform tarafından döndürülür.

**MSI_ENDPOINT** içinden uygulamanızın belirteçleri isteyebilir yerel bir URL. Bir kaynak için bir belirteç almak için aşağıdaki parametreleri de dahil olmak üzere bu uç nokta için bir HTTP GET isteği olun:

> |Parametre adı|İçinde|Açıklama|
> |-----|-----|-----|
> |kaynak|Sorgu|AAD kaynak kaynağın URI'sini için bir belirteç elde edilmelidir. Bu, aşağıdakilerden biri olabilir [Azure Hizmetleri söz konusu destek Azure AD kimlik doğrulamasını](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication) veya başka bir kaynağa bir URI.|
> |API sürümü|Sorgu|Kullanılacak belirteç API sürümü. "2017-09-01", şu anda desteklenen tek sürüm aşamasındadır.|
> |gizli dizi|Üst bilgi|MSI_SECRET ortam değişkeninin değeri. Bu üst bilgi, sunucu tarafı istek sahteciliği (SSRF) saldırılarını önlemeye yardımcı olmak için kullanılır.|
> |ClientID|Sorgu|(İsteğe bağlı) Kullanılacak kullanıcı tarafından atanan kimlik kimliği. Atlanırsa, sistem tarafından atanan kimlik kullanılır.|

Başarılı 200 Tamam yanıtı bir JSON gövdesi aşağıdaki özellikleri içerir:

> |Özellik adı|Açıklama|
> |-------------|----------|
> |access_token|İstenen erişim belirteci. Çağıran web hizmeti, alıcı web hizmetinde kimlik doğrulaması için bu belirteci kullanabilirsiniz.|
> |expires_on|Erişim belirtecinin süresinin sona erdiği zaman. Tarih 1970'ten saniye sayısı temsil edilen-01-kadar süre sonu UTC 01T0:0:0Z. Bu değer, önbelleğe alınan belirteç ömrünü belirlemek için kullanılır.|
> |kaynak|Alıcı web hizmeti uygulama kimliği URI'si.|
> |token_type|Belirteç türü değeri gösterir. Azure AD destekleyen tek taşıyıcı türüdür. Taşıyıcı belirteçleri hakkında daha fazla bilgi için bkz: [OAuth 2.0 yetkilendirme Framework: Taşıyıcı belirteç kullanımı (RFC 6750)](https://www.rfc-editor.org/rfc/rfc6750.txt).|

Bu yanıt aynıdır [AAD hizmetten hizmete erişim belirteci isteği için yanıt](../active-directory/develop/v1-oauth2-client-creds-grant-flow.md#service-to-service-access-token-response).

> [!NOTE]
> Ortam değişkenlerini ayarlandığı işlemi ilk kez başlatıldığında, böylece uygulamanız için bir yönetilen kimlik etkinleştirdikten sonra uygulamanızı yeniden başlatın ya da kendi kod önce dağıtmanız gerekebilir `MSI_ENDPOINT` ve `MSI_SECRET` kodunuz için kullanılabilir.

### <a name="rest-protocol-examples"></a>REST Protokolü örnekleri

Bir örnek istek aşağıdaki gibi görünebilir:

```
GET /MSI/token?resource=https://vault.azure.net&api-version=2017-09-01 HTTP/1.1
Host: localhost:4141
Secret: 853b9a84-5bfa-4b22-a3f3-0b9a43d9ad8a
```

Ve bir örnek yanıt aşağıdaki gibi görünebilir:

```
HTTP/1.1 200 OK
Content-Type: application/json

{
    "access_token": "eyJ0eXAi…",
    "expires_on": "09/14/2017 00:00:00 PM +00:00",
    "resource": "https://vault.azure.net",
    "token_type": "Bearer"
}
```

### <a name="code-examples"></a>Kod örnekleri

<a name="token-csharp"></a>Bu istekte bulunma C# için:

```csharp
public static async Task<HttpResponseMessage> GetToken(string resource, string apiversion)  {
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Add("Secret", Environment.GetEnvironmentVariable("MSI_SECRET"));
    return await client.GetAsync(String.Format("{0}/?resource={1}&api-version={2}", Environment.GetEnvironmentVariable("MSI_ENDPOINT"), resource, apiversion));
}
```

> [!TIP]
> .NET dilleri için de kullanabilirsiniz [Microsoft.Azure.Services.AppAuthentication](#asal) oluşturmak yerine kendiniz isteyin.

<a name="token-js"></a>Node.js'de:

```javascript
const rp = require('request-promise');
const getToken = function(resource, apiver, cb) {
    let options = {
        uri: `${process.env["MSI_ENDPOINT"]}/?resource=${resource}&api-version=${apiver}`,
        headers: {
            'Secret': process.env["MSI_SECRET"]
        }
    };
    rp(options)
        .then(cb);
}
```

<a name="token-powershell"></a>PowerShell'de:

```powershell
$apiVersion = "2017-09-01"
$resourceURI = "https://<AAD-resource-URI-for-resource-to-obtain-token>"
$tokenAuthURI = $env:MSI_ENDPOINT + "?resource=$resourceURI&api-version=$apiVersion"
$tokenResponse = Invoke-RestMethod -Method Get -Headers @{"Secret"="$env:MSI_SECRET"} -Uri $tokenAuthURI
$accessToken = $tokenResponse.access_token
```

## <a name="remove"></a>Bir kimlik kaldırılıyor

Bir sistem tarafından atanan kimliği, oluşturulduğu aynı şekilde portal, PowerShell veya CLI kullanarak özelliğini devre dışı bırakarak kaldırılabilir. Kullanıcı tarafından atanan kimlikleri ayrı ayrı kaldırabilirsiniz. Tüm kimlikleri REST/ARM şablonu protokolünde kaldırmak için bu ayar türü için "None" gerçekleştirilir:

```json
"identity": {
    "type": "None"
}
```

Bir sistem tarafından atanan kimliği bu şekilde kaldırarak Ayrıca, AAD'den siler. Uygulama kaynağı silindiğinde sistem tarafından atanan kimlikleri AAD'den da otomatik olarak kaldırılır.

> [!NOTE]
> Ayarlanabilen, bir uygulama ayarı yalnızca yerel belirteç hizmeti devre dışı bırakan WEBSITE_DISABLE_MSI yoktur. Ancak, yerinde kimlik bırakır ve araçları yönetilen kimlik olarak "on" veya "etkin" hala Göster Sonuç olarak, bu ayarın kullanılması önerilmez.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yönetilen kimlik kullanarak güvenli bir şekilde erişim SQL veritabanı](app-service-web-tutorial-connect-msi.md)

[Microsoft.Azure.Services.AppAuthentication başvurusu]: https://go.microsoft.com/fwlink/p/?linkid=862452
