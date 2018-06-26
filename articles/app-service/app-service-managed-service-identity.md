---
title: Uygulama hizmeti ve Azure işlevleri hizmet kimliği yönetilen | Microsoft Docs
description: Azure App Service ve Azure işlevleri yönetilen hizmet kimliği desteği için kavramsal başvurusu ve Kurulum Kılavuzu
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
ms.openlocfilehash: e6aa0d477f94cd5ab087beface65e3a28e5094f5
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36936981"
---
# <a name="how-to-use-azure-managed-service-identity-in-app-service-and-azure-functions"></a>Azure yönetilen hizmet kimliği uygulama hizmetini ve Azure işlevleri kullanma

> [!NOTE] 
> Linux ve Web uygulaması kapsayıcıları için uygulama hizmeti şu anda desteklemediği yönetilen hizmet kimliği.

> [!Important] 
> Uygulama hizmeti ve Azure işlevleri için Yönetilen hizmet kimliği uygulamanızı abonelikleri/kiracılar arasında geçirdiyseniz beklendiği gibi davranır değil. Uygulama devre dışı bırakma ve yeniden özelliğini etkinleştirme yapılabilir yeni bir kimlik edinmeniz gerekir. Bkz: [Kimlikteki kaldırma](#remove) aşağıda. Aşağı Akış kaynakları da erişim ilkelerini yeni kimlik kullanmak için güncelleştirilmiş olması gerekir.

Bu konuda, uygulama hizmeti ve Azure işlevleri uygulamaları için bir yönetilen uygulama kimliği oluşturma ve diğer kaynaklarına erişmek için kullandıkları gösterir. Yönetilen hizmet kimliği Azure Active Directory'den kolayca Azure anahtar kasası gibi diğer AAD korumalı kaynaklara erişmek uygulamanızı sağlar. Kimlik ve Azure platformu tarafından yönetilir ve sağlamak veya tüm gizli döndürmek gerektirmez. Yönetilen hizmet kimliği hakkında daha fazla bilgi için bkz: [yönetilen hizmet Kimliği'ne genel bakış](../active-directory/managed-service-identity/overview.md).

## <a name="creating-an-app-with-an-identity"></a>Bir kimliği ile bir uygulama oluşturma

Bir uygulama bir kimlikle oluşturma uygulamanın ayarlanması için ek bir özelliği gerektirir.

### <a name="using-the-azure-portal"></a>Azure portalını kullanma

Bir yönetilen hizmet kimliği portalında ayarlamak için önce bir uygulamayı normal olarak oluşturur ve özelliği etkinleştirmek.

1. Normal olarak Portalı'nda bir uygulama oluşturun. İçin portalda gidin.

2. Bir işlev uygulaması kullanıyorsanız gidin **Platform özellikleri**. Diğer uygulama türleri için doğru aşağı kaydırın **ayarları** sol gezinti bölmesinde grup.

3. Seçin **yönetilen hizmet kimliği**.

4. Anahtar **kaydetmek Azure Active Directory ile** için **üzerinde**. **Kaydet**’e tıklayın.

![Yönetilen uygulama Hizmeti'nde hizmet kimliği](media/app-service-managed-service-identity/msi-blade.png)

### <a name="using-the-azure-cli"></a>Azure CLI kullanma

Azure CLI kullanarak bir yönetilen hizmet kimliği ayarlamak için kullanmanız gerekecektir `az webapp identity assign` varolan bir uygulamaya göre komutu. Bu bölümde örnekleri çalıştırmak için üç seçeneğiniz vardır:

- Kullanım [Azure bulut Kabuk](../cloud-shell/overview.md) Azure portalından.
- Her kod bloğunun sağ üst köşesinde yer alan "deneyin" düğmesini, aracılığıyla katıştırılmış Azure bulut kabuğunu kullanın.
- [CLI 2.0'ın en son sürümünü yüklemek](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.31 veya sonrası) yerel CLI konsol kullanmayı tercih ederseniz. 

Aşağıdaki adımlar bir web uygulaması oluşturma ve CLI kullanarak bir kimlik atama size yol gösterir:

1. Yerel bir konsolda Azure CLI kullanıyorsanız, ilk kez Azure kullanarak oturum [az oturum açma](/cli/azure/reference-index#az_login). Altında uygulamayı dağıtmak istediğiniz Azure aboneliğiyle ilişkili olan bir hesabı kullanın:

    ```azurecli-interactive
    az login
    ```
2. CLI kullanarak bir web uygulaması oluşturun. CLI App Service ile kullanmak nasıl daha fazla örnekleri için bkz: [App Service CLI örnekleri](../app-service/app-service-cli-samples.md):

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

Aşağıdaki adımlar bir web uygulaması oluşturma ve Azure PowerShell kullanarak bir kimlik atama size yol gösterir:

1. Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/overview) bulunan yönergeleri kullanarak Azure PowerShell’i yükleyin ve ardından Azure ile bağlantı oluşturmak için `Login-AzureRmAccount` komutunu çalıştırın.

2. Azure PowerShell kullanarak bir web uygulaması oluşturun. Daha fazla App Service ile Azure PowerShell'i kullanma örnekleri için bkz [App Service PowerShell örnekleri](../app-service/app-service-powershell-samples.md):

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

### <a name="using-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu kullanarak

Bir Azure Resource Manager şablonu, Azure kaynaklarınızı dağıtımını otomatik hale getirmek için kullanılabilir. Uygulama hizmeti ve işlevleri dağıtma hakkında daha fazla bilgi için bkz: [App Service'te kaynak dağıtımı otomatikleştirme](../app-service/app-service-deploy-complex-application-predictably.md) ve [Azure işlevlerinde kaynak dağıtımı otomatikleştirme](../azure-functions/functions-infrastructure-as-code.md).

Türünde herhangi bir kaynağa `Microsoft.Web/sites` kaynak tanımı'nda aşağıdaki özellikler dahil olmak üzere bir kimlikle oluşturulabilir:
```json
"identity": {
    "type": "SystemAssigned"
}    
```

Bu oluşturmak ve uygulamanız için kimlik yönetmek için Azure bildirir.

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

Site oluşturulduğunda, aşağıdaki ek özelliklere sahiptir:
```json
"identity": {
    "tenantId": "<TENANTID>",
    "principalId": "<PRINCIPALID>"
}
```

Burada `<TENANTID>` ve `<PRINCIPALID>` GUID'lerini aşağıdaki ile değiştirilir. Tenantıd özelliği kimliği ait hangi AAD kiracısı tanımlar. Principalıd, uygulamanın yeni kimliği benzersiz tanımlayıcısıdır. AAD içinde hizmet sorumlusu uygulama hizmeti ya da Azure işlevleri örneğinizle verdiğiniz aynı ada sahip.

## <a name="obtaining-tokens-for-azure-resources"></a>Azure kaynakları için belirteç edinme

Bir uygulama kimliğini Azure anahtar kasası gibi AAD tarafından korunan diğer kaynaklara belirteçleri almak için kullanabilirsiniz. Bu belirteçler, kaynak ve hiçbir belirli bir kullanıcı uygulamanın erişim uygulama temsil eder. 

> [!IMPORTANT]
> Hedef kaynak, uygulamanızdan erişime izin verecek şekilde yapılandırmanız gerekebilir. Örneğin, anahtar kasası için bir belirteç istemek, uygulamanızın kimliğini içeren bir erişim ilkesi eklediğiniz emin olmanız gerekir. Belirteç içeriyorsa bile Aksi halde, anahtar kasası aramalarınız, reddedilir. Yönetilen hizmet kimliği belirteçleri daha destek hangi kaynakları hakkında bilgi edinmek için [Azure Hizmetleri, destek Azure AD kimlik doğrulamasının](../active-directory/managed-service-identity/overview.md#which-azure-services-support-managed-service-identity).

Uygulama hizmeti ve Azure işlevleri bir belirteç almak için basit bir REST protokol yoktur. .NET uygulamaları için Microsoft.Azure.Services.AppAuthentication kitaplığı bu protokolü üzerinden bir Özet sağlar ve bir yerel geliştirme deneyimi destekler.

### <a name="asal"></a>.NET için Microsoft.Azure.Services.AppAuthentication kitaplığı kullanma

.NET uygulamaları ve işlevleri için bir yönetilen hizmet kimliği ile çalışmak için en basit yolu Microsoft.Azure.Services.AppAuthentication paketidir. Bu kitaplık ayrıca kodunuzu makinenizde Visual Studio'dan kullanıcı hesabınızı kullanarak yerel olarak geliştirme, test sağlayacak [Azure CLI 2.0](https://docs.microsoft.com/cli/azure?view=azure-cli-latest), ya da Active Directory tümleşik kimlik doğrulaması. Bu kitaplığı ile yerel geliştirme seçenekleri hakkında daha fazla bilgi için bkz: [Microsoft.Azure.Services.AppAuthentication başvurusu]. Bu bölümde, kodunuzu kitaplıkta kullanmaya başlama gösterilmiştir.

1. Başvurular ekleyin [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) ve [Microsoft.Azure.KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault) uygulamanıza NuGet paketleri.

2.  Uygulamanız için aşağıdaki kodu ekleyin:

```csharp
using Microsoft.Azure.Services.AppAuthentication;
using Microsoft.Azure.KeyVault;
// ...
var azureServiceTokenProvider = new AzureServiceTokenProvider();
string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync("https://management.azure.com/");
// OR
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider.KeyVaultTokenCallback));
```

Microsoft.Azure.Services.AppAuthentication ve onu sunan işlemleri hakkında daha fazla bilgi için bkz: [Microsoft.Azure.Services.AppAuthentication başvurusu] ve [App Service ve MSI .NET ile KeyVault örnek](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet).

### <a name="using-the-rest-protocol"></a>REST protokolünü kullanarak

Yönetilen hizmet kimliği ile bir uygulama tanımlı iki ortam değişkeni vardır:
- MSI_ENDPOINT
- MSI_SECRET

**MSI_ENDPOINT** içinden uygulamanızı belirteçleri isteyebileceği yerel bir URL. Bir kaynak için bir belirteç almak için aşağıdaki parametreleri de dahil olmak üzere bu uç noktaya bir HTTP GET isteği olun:

> [!div class="mx-tdBreakAll"]
> |Parametre adı|İçinde|Açıklama|
> |-----|-----|-----|
> |kaynak|Sorgu|AAD kaynak kaynak URI'si için bir belirteç elde.|
> |API sürümü|Sorgu|Kullanılacak belirteci API sürümü. "2017-09-01" şu anda desteklenen tek sürümdür.|
> |gizli dizi|Üstbilgi|MSI_SECRET ortam değişkeni değeri.|


Başarılı bir 200 Tamam yanıt JSON gövdesi aşağıdaki özellikleri içerir:

> [!div class="mx-tdBreakAll"]
> |Özellik adı|Açıklama|
> |-------------|----------|
> |access_token|İstenen erişim belirteci. Çağrıyı yapan web hizmeti alıcı web hizmeti için kimlik doğrulaması için bu belirteci kullanabilirsiniz.|
> |expires_on|Erişim belirtecinin süresi dolduğunda süre. Tarih 1970'ten saniyeyi temsil edilir-01-01T0:0:0Z UTC sona erme zamanı kadar. Bu değer, önbelleğe alınan belirteç ömrü belirlemek için kullanılır.|
> |kaynak|Alıcı web hizmeti uygulama kimliği URI'si.|
> |token_type|Belirteç türü değeri gösterir. Azure AD destekleyen tek taşıyıcı türüdür. Taşıyıcı belirteçlerini hakkında daha fazla bilgi için bkz: [OAuth 2.0 yetkilendirme Framework: taşıyıcı belirteci kullanımı (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt).|


Bu yanıt aynıdır [AAD hizmetten hizmete erişim belirteci istek için yanıt](../active-directory/develop/active-directory-protocols-oauth-service-to-service.md#service-to-service-access-token-response).

> [!NOTE] 
> Ortam değişkenleri ayarlandığı işlemi ilk kez başlatıldığında, yönetilen hizmet kimliği uygulamanız için etkinleştirdikten sonra uygulamanızı yeniden başlatın ya da kendi kodu önce dağıtmanız gerekebilir şekilde `MSI_ENDPOINT` ve `MSI_SECRET` kodunuz için kullanılabilir.

### <a name="rest-protocol-examples"></a>REST Protokolü örnekleri
Bir örnek isteği aşağıdakine benzeyebilir:
```
GET /MSI/token?resource=https://vault.azure.net&api-version=2017-09-01 HTTP/1.1
Host: localhost:4141
Secret: 853b9a84-5bfa-4b22-a3f3-0b9a43d9ad8a
```
Ve bir örnek yanıt aşağıdakine benzeyebilir:
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
Bu istek C# ' ta yapmak için:
```csharp
public static async Task<HttpResponseMessage> GetToken(string resource, string apiversion)  {
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Add("Secret", Environment.GetEnvironmentVariable("MSI_SECRET"));
    return await client.GetAsync(String.Format("{0}/?resource={1}&api-version={2}", Environment.GetEnvironmentVariable("MSI_ENDPOINT"), resource, apiversion));
}
```
> [!TIP]
> .NET diller için de kullanabilirsiniz [Microsoft.Azure.Services.AppAuthentication](#asal) bu hazırlayın yerine kendiniz isteyin.

Node.JS içinde:
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

PowerShell içinde:
```powershell
$apiVersion = "2017-09-01"
$resourceURI = "https://<AAD-resource-URI-for-resource-to-obtain-token>"
$tokenAuthURI = $env:MSI_ENDPOINT + "?resource=$resourceURI&api-version=$apiVersion"
$tokenResponse = Invoke-RestMethod -Method Get -Headers @{"Secret"="$env:MSI_SECRET"} -Uri $tokenAuthURI
$accessToken = $tokenResponse.access_token
```

## <a name="remove"></a>Kimlikteki kaldırma

Kimlikteki oluşturulduğu aynı şekilde portal, PowerShell veya CLI kullanarak özelliğini devre dışı bırakarak kaldırılabilir. REST/ARM şablonu Protokolü bu tür "None" ayarlayarak yapılır:

```json
"identity": {
    "type": "None"
}    
```

Bu şekilde kimliğini kaldırmak da asıl AAD siler. Uygulama kaynağı silindiğinde, sistem tarafından atanan kimlikleri AAD otomatik olarak kaldırılır.

> [!NOTE] 
> Ayarlanabilir, ayarı bir uygulamayı yalnızca yerel belirteci hizmeti devre dışı bırakır WEBSITE_DISABLE_MSI yoktur. Ancak, kimlik yerinde bırakır ve Araçları "etkinleştirilmiş" veya "açık" olarak MSI hala Göster Sonuç olarak, bu seçeneğin kullanılması recommmended değil.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yönetilen hizmet kimliğini kullanarak SQL Veritabanına güvenli bir şekilde erişme](app-service-web-tutorial-connect-msi.md)

[Microsoft.Azure.Services.AppAuthentication başvurusu]: https://go.microsoft.com/fwlink/p/?linkid=862452
