---
title: "Bir Azure VM yönetilen hizmet kimliği için oturum açma ve belirteç edinme kullanma"
description: "Adım adım bir Azure VM MSI kullanma yönergeleri için oturum açma ve erişim belirtecini alma için asıl hizmet."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/17/2017
ms.author: bryanla
ms.openlocfilehash: 168b2ab3676d3f3e2830966f850e14adbe579f85
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2017
---
# <a name="how-to-use-an-azure-vm-managed-service-identity-msi-for-sign-in-and-token-acquisition"></a>Bir Azure VM yönetilen hizmet kimliği (MSI) için oturum açma ve belirteç edinme kullanma 
[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]Azure VM'de MSI etkinleştirdikten sonra oturum açma için ve bir erişim belirteci istemek için MSI kullanabilirsiniz. Bu makalede, bir MSI kullanmak için çeşitli yollar gösterilmektedir [hizmet sorumlusu](develop/active-directory-dev-glossary.md#service-principal-object) için oturum açma ve edinmek bir [yalnızca uygulama erişim belirteci](develop/active-directory-dev-glossary.md#access-token) dahil olmak üzere diğer kaynaklarına erişmek için:

- PowerShell veya Azure CLI sessiz/katılımsız oturum aç
- .NET, PowerShell, Bash/CURL, REST dahil olmak üzere çeşitli istemciler için belirteç edinme
- .NET, Java, Node.js, Python, Ruby dahil olmak üzere bir Azure SDK kullanarak oturum açın

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

Bu makalede PowerShell örneklerini kullanmayı planlıyorsanız, yüklediğinizden emin olun [Azure PowerShell sürüm 4.3.1](https://www.powershellgallery.com/packages/AzureRM) veya daha büyük. Bu makalede Azure CLI örnekler kullanmayı planlıyorsanız, üç seçeneğiniz vardır:
- Kullanım [Azure bulut Kabuk](../cloud-shell/overview.md) Azure portalından.
- Azure CLI kod blokları sağ üst köşesinde yer alan "deneyin" düğmesini, aracılığıyla katıştırılmış Azure bulut kabuğunu kullanın.
- [CLI 2.0'ın en son sürümünü yüklemek](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 veya sonrası) yerel bir komut istemi CLI kullanmayı tercih ederseniz. 


> [!IMPORTANT]
> - Tüm örnek kodu/komut dosyası bu makalede varsayar istemci MSI özellikli sanal makine üzerinde çalışan. Bir VM'de MSI etkinleştirme hakkında daha fazla bilgi için bkz: [bir VM yönetilen hizmet kimliği (Azure Portalı'nı kullanarak MSI) yapılandırma](msi-qs-configure-portal-windows-vm.md), veya değişken makaleleri (PowerShell, CLI, şablon veya bir Azure SDK'sını kullanarak). 
> - Yetkilendirme hataları önlemek için (403/AuthorizationFailed) kod/komut dosyası örneklerde, VM'ın kimlik VM üzerinde Azure Resource Manager işlemler izin vermek için VM kapsamda "Okuyucu" erişim verilmelidir. Bkz: [bir yönetilen hizmet kimliği (MSI) erişim Azure Portalı'nı kullanarak bir kaynak atama](msi-howto-assign-access-portal.md) Ayrıntılar için.
> - Aşağıdaki bölümlerde biri için devam etmeden önce Uzaktan MSI etkin VM'nize bağlanmak için Azure portalında "Bağlan" VM özelliğini kullanın.

## <a name="how-to-sign-in-from-powershell-or-cli-using-msi"></a>PowerShell veya CLI MSI kullanarak oturum açma

Daha önce amacı kendi kimlik altında bir komut dosyası çalıştırma:
- Azure AD ile gizli/web istemci uygulaması olarak kaydetme
- uygulamanın istemci kimliği/parola kimlik bilgilerini kullanarak oturum açma

MSI ile komut dosyası istemciniz artık Azure AD'ye kaydedin veya kimlik bilgilerini sağlamanız gerekir. Aşağıdaki örneklerde, oturum açma için hizmet sorumlusu VM'in MSI kullanma gösteriyoruz.

### <a name="azure-powershell"></a>Azure PowerShell

Aşağıdaki komut dosyası gösterilmektedir nasıl yapılır:

- bir Azure VM için bir MSI erişim belirteci alma
- karşılık gelen MSI hizmet sorumlusu altında Azure AD oturum açmak için erişim belirteci kullanın
- MSI hizmet sorumlusu hizmet sorumlusu Kimliğini almak için bir Azure Resource Manager araması yapmak için kullanın

```powershell
# Get an access token from MSI
$response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token `
                              -Method GET -Body @{resource="https://management.azure.com/"} -Headers @{Metadata="true"}
$content =$response.Content | ConvertFrom-Json
$access_token = $content.access_token

# Use the access token to sign in under the MSI service principal
Login-AzureRmAccount -AccessToken $access_token -AccountId “CLIENT”

# The MSI service principal is now signed in for this session.
# Next, a call to Azure Resource Manager is made to get the service principal ID for the VM's MSI. 
$spID = (Get-AzureRMVM -ResourceGroupName <RESOURCE-GROUP> -Name <VM-NAME>).identity.principalid
echo "The MSI service principal ID is $spID"
```
   
### <a name="azure-cli"></a>Azure CLI

Aşağıdaki komut dosyası gösterilmektedir nasıl yapılır:

- VM'ın MSI hizmet sorumlusu altında Azure AD oturum açın
- MSI hizmet sorumlusu hizmet sorumlusu Kimliğini almak için bir Azure Resource Manager araması yapmak için kullanın

```azurecli-interactive
az login --msi
spID=$(az resource list -n <VM-NAME> --query [*].identity.principalId --out tsv)
echo The MSI service principal ID is $spID
```

## <a name="how-to-acquire-an-access-token-from-msi"></a>MSI erişim belirtecinden elde etme

MSI kullanırken, istemci uygulama/komut artık Azure AD'ye kaydolacak ya da kendi istemci Kimliğini ve parolasını bir erişim belirteci almak için sunmak gerekir. İstemci yalnızca yerel MSI uç nokta için bir erişim belirteci, uygulama kimlik bilgilerini sunmadan sorabilirsiniz. Yalnızca uygulama erişim belirtecini bir taşıyıcı belirteci olarak kullanmaya uygun MSI hizmet sorumlusu için verilen [service-to-service çağrıları gerektiren istemci kimlik bilgileri](active-directory-protocols-oauth-service-to-service.md).

Aşağıdaki örneklerde, belirteç alımı için bir VM'ın MSI kullanmayı gösterir.

### <a name="net"></a>.NET

> [!IMPORTANT]
> Azure AD Authentication Library (ADAL) ile MSI tümleşik değildir. MSI kullanarak bir erişim belirteci edinmek için bir isteği yerel MSI son nokta bir VM oluşturun. Daha fazla bilgi için bkz: [MSI SSS ve bilinen sorunlar](msi-known-issues.md#frequently-asked-questions-faqs).

```csharp
// Build request to acquire MSI token
HttpWebRequest request = (HttpWebRequest)WebRequest.Create("http://localhost:50342/oauth2/token?resource=https://management.azure.com/");
request.Headers["Metadata"] = "true";
request.Method = "GET";

try
{
    // Call /token endpoint
    HttpWebResponse response = (HttpWebResponse)request.GetResponse();

    // Pipe response Stream to a StreamReader, and extract access token
    StreamReader streamResponse = new StreamReader(response.GetResponseStream()); 
    string stringResponse = streamResponse.ReadToEnd();
    JavaScriptSerializer j = new JavaScriptSerializer();
    Dictionary<string, string> list = (Dictionary<string, string>) j.Deserialize(stringResponse, typeof(Dictionary<string, string>));
    string accessToken = list["access_token"];
}
catch (Exception e)
{
    string errorText = String.Format("{0} \n\n{1}", e.Message, e.InnerException != null ? e.InnerException.Message : "Acquire token failed");
}

```

### <a name="azure-powershell-token"></a>Azure PowerShell

```powershell
# Get an access token from MSI
$response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token `
                              -Method GET -Body @{resource="https://management.azure.com/"} -Headers @{Metadata="true"}
$content =$response.Content | ConvertFrom-Json
$access_token = $content.access_token
echo "The MSI access token is $access_token"
```

### <a name="bashcurl"></a>Bash/CURL

```bash
response=$(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s)
access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
echo The MSI access token is $access_token
```

### <a name="rest"></a>REST

Örnek istek:

```
GET http://localhost:50342/oauth2/token?resource=https%3A%2F%2Fmanagement.azure.com%2F HTTP/1.1
Metadata: true
```

| Öğe | Açıklama |
| ------- | ----------- |
| `GET` | Uç noktasından verileri almak istediğiniz belirten HTTP fiili. Bu durumda, bir OAuth belirteç erişin. | 
| `http://localhost:50342/oauth2/token` | Burada 50342 varsayılan bağlantı noktası ve yapılandırılabilir MSI endpoint. |
| `resource` | Hedef kaynak uygulama kimliği URI'sini belirten bir sorgu dizesi parametresi. Ayrıca görünür `aud` verilen belirteç talep (İzleyici). Bu örnekte, biz https://management.azure.com/ bir uygulama kimliği URI'sini olan Azure Kaynak Yöneticisi'ne erişmek için bir belirteç istiyor. |
| `Metadata` | Sunucu tarafı istek sahtekarlığı (SSRF) saldırılara karşı azaltma olarak MSI tarafından gerekli bir HTTP isteği üstbilgisi alanının. Bu değer "tüm alt durumda true olarak" olarak ayarlanmalıdır.

Örnek yanıt:

```
HTTP/1.1 200 OK
Content-Type: application/json
{
  "access_token": "eyJ0eXAi...",
  "refresh_token": "",
  "expires_in": "3599",
  "expires_on": "1506484173",
  "not_before": "1506480273",
  "resource": "https://management.azure.com/",
  "token_type": "Bearer"
}
```

| Öğe | Açıklama |
| ------- | ----------- |
| `access_token` | İstenen erişim belirteci. Belirteç katıştırılmış olduğu bir REST API'si çağrılırken `Authorization` isteği üstbilgisi alanının arayan kimliğini doğrulamak API izin verme "bearer" Token olarak. | 
| `refresh_token` | MSI tarafından kullanılmaz. |
| `expires_in` | Erişim belirteci, verme zamandan süresi dolmadan önce geçerli olmaya devam saniye sayısı. Verme süresini belirtecin içinde bulunabilir `iat` talep. |
| `expires_on` | Erişim belirtecinin süresi dolduğunda timespan. Tarih saniyeden sayısı olarak temsil edilir "1970'ten-01-01T0:0:0Z UTC" (belirtecin karşılık gelen `exp` talep). |
| `not_before` | Erişim belirteci etkili olur ve kabul edilebilir timespan. Tarih saniyeden sayısı olarak temsil edilir "1970'ten-01-01T0:0:0Z UTC" (belirtecin karşılık gelen `nbf` talep). |
| `resource` | Erişim belirteci için hangi eşleşen istenen kaynak `resource` sorgu dizesi parametresi istek. |
| `token_type` | Kaynak bu belirteç taşıyıcı için erişmenizi anlamına gelir "Bearer" erişim belirteci Belirtecin türü. |

## <a name="how-to-use-msi-with-azure-sdk-libraries"></a>MSI Azure SDK kitaplıkları ile kullanma

Azure destekleyen bir dizi programlama birden çok platform [Azure SDK'ları](https://azure.microsoft.com/downloads). Birkaç bir MSI kullanarak oturum açın destekleyecek şekilde güncelleştirilmiştir ve kullanım göstermek için karşılık gelen örnekleri sağlayın. Ek destek eklendikçe bu listeyi güncelleştirilmiştir:

| SDK | Örnek |
| --- | ------ | 
| .NET | [Yönetilen hizmet kimliği kullanarak bir Windows VM bir ARM şablonu dağıtma](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet) |
| .NET Core | [Azure Hizmetleri yönetilen hizmet kimliği kullanarak bir Linux VM çağırın](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/) |
| Node.js| [Yönetilen hizmet kimliği kullanarak kaynakları yönetme](https://azure.microsoft.com/resources/samples/resources-node-manage-resources-with-msi/) |
| Python | [Yalnızca gelen bir VM içinde kimlik doğrulaması için MSI kullanın](https://azure.microsoft.com/resources/samples/resource-manager-python-manage-resources-with-msi/) |
| Ruby   | [MSI özellikli bir sanal makineden kaynaklarını yönetme](https://azure.microsoft.com/resources/samples/resources-ruby-manage-resources-with-msi/) | 

## <a name="additional-information"></a>Ek bilgiler

### <a name="token-expiration"></a>Belirteç süre sonu

Yerel MSI alt sistemi belirteçleri önbelleğe alır. Bu nedenle, istediğiniz ve yalnızca Azure AD üzerinde hat çağrısına sonuçları kadar çağırabilirsiniz:
- önbellek isabetsizliği önbelleğinde herhangi bir belirteci nedeniyle oluşur
- belirtecin süresi doldu

Kodunuzda belirteç önbelleğe alma, burada belirtecinin süresi doldu kaynağı gösterir senaryoları işlemek için hazırlıklı olmalıdır.

### <a name="resource-ids-for-azure-services"></a>Azure Hizmetleri için kaynak kimlikleri

Bkz: [Azure Hizmetleri, destek Azure AD kimlik doğrulamasının](msi-overview.md#azure-services-that-support-azure-ad-authentication) MSI ve bunların ilgili kaynak kimlikleri destekleyen hizmetlerin listesi.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="sign-in-or-token-acquisition-fails"></a>Başarısız oturum açma veya belirteç edinme

MSI doğru etkin olduğunu doğrulayın. Azure VM'yi dönün [Azure portal](https://portal.azure.com) ve:

- "Yapılandırma" sayfasına bakın ve etkin MSI olun = "Yes."
- "Uzantılarla" sayfasına bakın ve başarılı bir şekilde dağıtılan MSI uzantısı emin olun.

Ya da yanlışsa, kaynakta MSI yeniden dağıtmanız veya dağıtım hatası sorun giderme gerekebilir.

### <a name="http-request-error-responses"></a>HTTP isteği hata yanıtları

- *bad_request_102: gerekli meta veriler üstbilgisi belirtilmedi*

  Her iki `Metadata` isteği üstbilgisi alanının isteğinizden eksik veya yanlış biçimlendirilmiş. Değer olarak belirtilmelidir `true`, tüm alt durumda. "Örnek istek" bölümüne bakın [geri KALAN bölümü önceki](#rest) bir örnek.

- *Bilinmeyen: Active Directory'den belirteci alınamadı. Ayrıntılar için günlükleri Bkz \<dosya yolu\>*

  HTTP GET isteği URI özellikle URI sorgu dizesinde belirtilen kaynak doğru şekilde biçimlendirildiğini doğrulayın. "Örnek istek" bölümüne bakın [geri KALAN bölümü önceki](#rest) bir örnek veya [Azure Hizmetleri, destek Azure AD kimlik doğrulamasının](msi-overview.md#azure-services-that-support-azure-ad-authentication) Hizmetleri ve bunların ilgili kaynak kimlikleri listesi.

- *unknown_source: Bilinmeyen bir kaynaktan \<URI\>*

  HTTP GET isteği URI doğru biçimlendirildiğinden emin olun. `scheme:host/resource-path` Bölümü olarak belirtilmelidir `http://localhost:50342/oauth2/token`. "Örnek istek" bölümüne bakın [geri KALAN bölümü önceki](#rest) bir örnek.

## <a name="related-content"></a>İlgili içerik

- MSI genel bakış için bkz: [yönetilen hizmet Kimliği'ne genel bakış](msi-overview.md).
- Azure VM'de MSI etkinleştirmek için bkz: [bir Azure VM yönetilen hizmet kimliği (PowerShell kullanarak MSI) yapılandırma](msi-qs-configure-powershell-windows-vm.md).

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.

