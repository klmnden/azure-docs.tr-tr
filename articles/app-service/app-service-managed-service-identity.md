---
title: App Service ve Azure işlevleri kimliklerini yönetilen | Microsoft Docs
description: Azure App Service ve Azure işlevleri yönetilen kimlikleri için kavramsal başvurusu ve Kurulum Kılavuzu
services: app-service
author: mattchenderson
manager: cfowler
editor: ''
ms.service: app-service
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 06/25/2018
ms.author: mahender
ms.openlocfilehash: fb9b50ecb16bd37d005403a14ea11c6d89f50dfe
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46983659"
---
# <a name="how-to-use-managed-identities-for-app-service-and-azure-functions"></a>App Service ve Azure işlevleri için yönetilen kimliklerini kullanma

> [!NOTE] 
> App Service'in Linux'ta ve kapsayıcılar için Web uygulaması şu anda desteklemediği yönetilen kimlikleri.

> [!Important] 
> App Service ve Azure işlevleri için yönetilen kimlikleri, uygulamanızı abonelikleri/kiracılar genelinde geçirdiyseniz beklendiği gibi davranmaz. Uygulamayı devre dışı bırakıp yeniden özelliğini etkinleştirerek yapılabilir yeni bir kimliği edinmeniz gerekir. Bkz: [Kimlikteki kaldırma](#remove) aşağıda. Aşağı Akış kaynakları da erişim ilkelerini yeni bir kimlik kullanacak şekilde güncelleştirilmiş olması gerekir.

Bu konuda, App Service ve Azure işlevleri uygulamaları için yönetilen bir kimlik oluşturma ve diğer kaynaklarına erişmek için kullanma gösterilmektedir. Azure Key Vault gibi diğer AAD korumalı kaynakları kolayca erişmek için uygulamanızı Azure Active Directory'den yönetilen bir kimlik sağlar. Kimlik Azure platformu tarafından yönetilir ve sağlama veya herhangi bir gizli anahtar döndürme gerektirmez. Aad'de yönetilen kimlikleri hakkında daha fazla bilgi için bkz. [kimliklerini Azure kaynakları için yönetilen](../active-directory/managed-identities-azure-resources/overview.md).

## <a name="creating-an-app-with-an-identity"></a>Bir kimlik ile uygulama oluşturma

Bir kimlik ile uygulama oluşturma, uygulama üzerinde ayarlamak için ek bir özellik gerektirir.

### <a name="using-the-azure-portal"></a>Azure portalını kullanma

Portalda yönetilen bir kimlik ayarlamak için ilk olarak normal bir uygulama oluşturun ve ardından özelliği etkinleştirmek.

1. Normalde yaptığınız gibi Portalı'nda bir uygulama oluşturun. İçin portalda gidin.

2. Bir işlev uygulaması kullanıyorsanız gidin **Platform özellikleri**. Diğer uygulama türleri için aşağı kaydırarak **ayarları** sol gezinti grubu.

3. Seçin **yönetilen kimliği**.

4. Anahtar **Azure Active Directory ile kayıt** için **üzerinde**. **Kaydet**’e tıklayın.

![App Service içindeki yönetilen kimlik](media/app-service-managed-service-identity/msi-blade.png)

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
2. CLI kullanarak bir web uygulaması oluşturun. App Service ile CLI'yı kullanma konusunda daha fazla örnek için bkz: [App Service CLI örnekleri](../app-service/app-service-cli-samples.md):

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

Aşağıdaki adımlar, bir web uygulaması oluşturma ve Azure PowerShell kullanarak bir kimlik atama anlatılmaktadır:

1. Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/overview) bulunan yönergeleri kullanarak Azure PowerShell’i yükleyin ve ardından Azure ile bağlantı oluşturmak için `Login-AzureRmAccount` komutunu çalıştırın.

2. Azure PowerShell kullanarak bir web uygulaması oluşturun. App Service ile Azure PowerShell'i kullanma konusunda daha fazla örnek için bkz: [App Service PowerShell örnekleri](../app-service/app-service-powershell-samples.md):

    ```azurepowershell-interactive
    # Create a resource group.
    New-AzureRmResourceGroup -Name myResourceGroup -Location $location
    
    # Create an App Service plan in Free tier.
    New-AzureRmAppServicePlan -Name $webappname -Location $location -ResourceGroupName myResourceGroup -Tier Free
    
    # Create a web app.
    New-AzureRmWebApp -Name $webappname -Location $location -AppServicePlan $webappname -ResourceGroupName myResourceGroup
    ```

3. Çalıştırma `identity assign` komutu bu uygulama için kimlik oluşturmak için:

    ```azurepowershell-interactive
    Set-AzureRmWebApp -AssignIdentity $true -Name $webappname -ResourceGroupName myResourceGroup 
    ```

### <a name="using-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu kullanma

Bir Azure Resource Manager şablonu kullanarak Azure kaynaklarınızı dağıtımını otomatik hale getirmek için kullanılabilir. App Service ve İşlevler için dağıtma hakkında daha fazla bilgi edinmek için [App Service'te kaynak dağıtımını otomatikleştirme](../app-service/app-service-deploy-complex-application-predictably.md) ve [Azure işlevleri'nde kaynak dağıtımını otomatikleştirme](../azure-functions/functions-infrastructure-as-code.md).

Herhangi bir kaynak türü `Microsoft.Web/sites` kaynak tanımı'nda aşağıdaki özellikler dahil olmak üzere bir kimlikle oluşturulabilir:
```json
"identity": {
    "type": "SystemAssigned"
}    
```

Bu, oluşturmak ve uygulamanız için kimlik yönetimi için Azure bildirir.

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
    "tenantId": "<TENANTID>",
    "principalId": "<PRINCIPALID>"
}
```

Burada `<TENANTID>` ve `<PRINCIPALID>` GUID'lerini aşağıdaki ile değiştirilir. AAD kiracısının kimliği ait ne Tenantıd özelliği tanımlar. Principalıd, uygulamanın yeni kimlik için benzersiz bir tanımlayıcıdır. AAD içinde hizmet sorumlusu, App Service veya Azure işlevleri Örneğinize verdiğiniz aynı ada sahip.

## <a name="obtaining-tokens-for-azure-resources"></a>Azure kaynakları için belirteç edinme

Uygulama kimliğini, Azure Key Vault gibi bir AAD tarafından korunan diğer kaynaklara belirteçlerini almak için kullanabilirsiniz. Bu belirteçler kaynağı ve belirli hiçbir kullanıcı uygulamanın erişen uygulamada temsil eder. 

> [!IMPORTANT]
> Hedef kaynak, uygulamanızdan erişime izin verecek şekilde yapılandırmanız gerekebilir. Örneğin, anahtar kasası için bir belirteç isteği, uygulamanızın kimliğini içeren bir erişim ilkesi eklediğinizden emin olun gerekir. Belirteç içeriyorsa bile Aksi takdirde, aramalarınız için Key Vault, reddedilir. Azure Active Directory belirteçleri daha destek hangi kaynakları hakkında bilgi edinmek için [Azure Hizmetleri söz konusu destek Azure AD kimlik doğrulamasını](../active-directory/managed-identities-azure-resources/services-support-msi.md#azure-services-that-support-azure-ad-authentication).

App Service ve Azure işlevleri, bir belirteç almak için basit bir REST protokolü yoktur. .NET uygulamaları için Microsoft.Azure.Services.AppAuthentication kitaplığını, bu protokolü üzerinden bir Özet sağlar ve bir yerel geliştirme deneyimini destekler.

### <a name="asal"></a>.NET için Microsoft.Azure.Services.AppAuthentication kitaplığını kullanma

.NET uygulamaları ve işlevleri için yönetilen bir kimlik ile çalışmak için en basit yolu Microsoft.Azure.Services.AppAuthentication paketidir. Bu kitaplık, Visual Studio, kullanıcı hesabını kullanarak yerel olarak geliştirme makinenizde, kodunuzu test etmek de sağlayacak [Azure CLI](/cli/azure), ya da Active Directory tümleşik kimlik doğrulaması. Bu kitaplığı ile yerel geliştirme seçenekleri hakkında daha fazla bilgi için bkz. [Microsoft.Azure.Services.AppAuthentication başvurusu]. Bu bölümde, kitaplığı kodunuza kullanmaya başlama işlemini göstermektedir.

1. Başvuruları Ekle [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) ve [Microsoft.Azure.KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault) uygulamanıza NuGet paketleri.

2.  Uygulamanız için aşağıdaki kodu ekleyin:

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
- MSI_ENDPOINT
- MSI_SECRET

**MSI_ENDPOINT** içinden uygulamanızın belirteçleri isteyebilir yerel bir URL. Bir kaynak için bir belirteç almak için aşağıdaki parametreleri de dahil olmak üzere bu uç nokta için bir HTTP GET isteği olun:

> [!div class="mx-tdBreakAll"]
> |Parametre adı|İçinde|Açıklama|
> |-----|-----|-----|
> |kaynak|Sorgu|AAD kaynak kaynağın URI'sini için bir belirteç elde edilmelidir.|
> |API sürümü|Sorgu|Kullanılacak belirteç API sürümü. "2017-09-01", şu anda desteklenen tek sürüm aşamasındadır.|
> |gizli dizi|Üst bilgi|MSI_SECRET ortam değişkeninin değeri.|


Başarılı 200 Tamam yanıtı bir JSON gövdesi aşağıdaki özellikleri içerir:

> [!div class="mx-tdBreakAll"]
> |Özellik adı|Açıklama|
> |-------------|----------|
> |access_token|İstenen erişim belirteci. Çağıran web hizmeti, alıcı web hizmetinde kimlik doğrulaması için bu belirteci kullanabilirsiniz.|
> |expires_on|Erişim belirtecinin süresinin sona erdiği zaman. Tarih 1970'ten saniye sayısı temsil edilen-01-kadar süre sonu UTC 01T0:0:0Z. Bu değer, önbelleğe alınan belirteç ömrünü belirlemek için kullanılır.|
> |kaynak|Alıcı web hizmeti uygulama kimliği URI'si.|
> |token_type|Belirteç türü değeri gösterir. Azure AD destekleyen tek taşıyıcı türüdür. Taşıyıcı belirteçleri hakkında daha fazla bilgi için bkz: [OAuth 2.0 yetkilendirme Framework: taşıyıcı belirteç kullanımı (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt).|


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
const rp = require('request-promise');
const getToken = function(resource, apiver, cb) {
    var options = {
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

Bir kimlik, oluşturulduğu aynı şekilde portal, PowerShell veya CLI kullanarak özelliğini devre dışı bırakarak kaldırılabilir. REST/ARM şablonu protokolünde "None" türüne ayarlayarak gerçekleştirilir:

```json
"identity": {
    "type": "None"
}    
```

Bu şekilde kimlik kaldırma da asıl AAD'den silinir. Uygulama kaynağı silindiğinde sistem tarafından atanan kimlikleri AAD'den otomatik olarak kaldırılır.

> [!NOTE] 
> Ayarlanabilen, bir uygulama ayarı yalnızca yerel belirteç hizmeti devre dışı bırakan WEBSITE_DISABLE_MSI yoktur. Ancak, yerinde kimlik bırakır ve araçları yönetilen kimlik olarak "on" veya "etkin" hala Göster Sonuç olarak, bu seçeneğin önerilen değil.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yönetilen kimlik kullanarak güvenli bir şekilde erişim SQL veritabanı](app-service-web-tutorial-connect-msi.md)

[Microsoft.Azure.Services.AppAuthentication başvurusu]: https://go.microsoft.com/fwlink/p/?linkid=862452
