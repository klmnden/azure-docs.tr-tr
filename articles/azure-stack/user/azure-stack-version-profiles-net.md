---
title: Azure stack'teki .NET SDK'sı ile API Sürüm profillerini kullanma | Microsoft Docs
description: . NET'te Azure Stack ile API Sürüm profillerini kullanma hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2018
ms.author: sethm
ms.reviewer: sijuman
ms.openlocfilehash: 20e96ad7a99fdb8c90f3b7990965d7225aef8be0
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2018
ms.locfileid: "53555022"
---
# <a name="use-api-version-profiles-with-net-in-azure-stack"></a>. NET'te Azure Stack ile API Sürüm profillerini kullanma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

.NET SDK'sı için Azure Stack Kaynak Yöneticisi'ni oluşturmanıza ve altyapınızı yönetmenize yardımcı olacak araçlar sağlar. İşlem, ağ, depolama, uygulama hizmetleri, SDK kaynak sağlayıcılarını içerir ve [KeyVault](../../key-vault/key-vault-whatis.md). .NET SDK'sı 14 NuGet paketlerini içerir. Bu paketler projesi çözümünüze profil bilgilerini içeren her zaman yüklenmesi gerekir. Ancak, özellikle hangi kaynak sağlayıcısı yükleyebilir, uygulamanız için bellek iyileştirmek için 2018-03-01-karma veya 2017-03-09-profile için kullanır. Her paket, kaynak sağlayıcısı, ilgili API sürümü ve ait olduğu API profili oluşur. .NET SDK'sı API profillerinde genel Azure kaynakları ve Azure Stack'te kaynakları arasında geçiş yardımcı olarak karma bulut geliştirmeyi etkinleştirin.

## <a name="net-and-api-version-profiles"></a>.NET ve API sürümü profillerini

Bir API profili, kaynak sağlayıcıları ve API sürümlerini birleşimidir. Bir API profili, bir kaynak sağlayıcısı paketindeki her kaynak türünün en son ve en çok kararlı sürümünü almak için kullanabilirsiniz.

-   Tüm hizmetler en son sürümlerini kullanın, yapmak **son** paketlerin profili. Bu profili parçasıdır **Microsoft.Azure.Management** NuGet paketi.

-   Azure Stack ile uyumlu hizmetleri kullanmak için **Microsoft.Azure.Management.Profiles.hybrid\_2018\_03\_01. *ResourceProvider*. 0.9.0-preview.nupkg** veya **Microsoft.Azure.Management.Profiles.hybrid\_2017\_03\_09. *ResourceProvider*. 0.9.0-preview.nupkg** paketleri.

    -   Her bir profil için her kaynak sağlayıcısı için iki paket vardır.

    -   Emin **ResourceProvider** bölümü yukarıdaki NuGet paketinin doğru sağlayıcıya değiştirilir.

-   Bir hizmetin en son API-version'ı kullanmak için **son** belirli NuGet paketinin profili. Örneğin kullanmak istiyorsanız, **son API** işlem hizmetinin sürümü tek başına kullanım **son** profilini **işlem** paket. **Son** profilidir parçası **Microsoft.Azure.Management** NuGet paketi.

-   Belirli bir kaynak sağlayıcısındaki bir kaynak türü için belirli API sürümlerini kullanmak için bir paket içinde tanımlanan belirli API sürümlerini kullanın.

Tüm seçeneklerin aynı uygulamada birleştirebilirsiniz.

## <a name="install-the-azure-net-sdk"></a>Azure .NET SDK'sını yükleyin

1.  Git'i yükleyin. Yönergeler için [Başlarken - Git yükleme][].

2.  Doğru NuGet paketlerini yüklemek için bkz [bulma ve bir paket yükleme][].

3.  Yüklenmesi gereken paketleri kullanmak istediğiniz profili sürümüne bağlıdır. Paket adları profil sürümleri şunlardır:

    1.  **Microsoft.Azure.Management.Profiles.hybrid\_2018\_03\_01. *ResourceProvider*. 0.9.0-preview.nupkg**

    2.  **Microsoft.Azure.Management.Profiles.hybrid\_2017\_03\_09. *ResourceProvider*. 0.9.0-preview.nupkg**

4.  Visual Studio Code için doğru NuGet paketlerini yüklemek için indirmek için aşağıdaki bağlantıya bakın [NuGet Paket Yöneticisi yönergeleri][].

5.  Yoksa, bir abonelik oluşturur ve daha sonra kullanılmak üzere abonelik Kimliğini kaydedin. Bir abonelik oluşturmak yönergeler için bkz: [Azure Stack'te teklifleri abonelikleri oluşturma][].

6.  Hizmet sorumlusu oluşturma ve istemci Kimliğini ve istemci gizli anahtarını kaydedin. Azure Stack için hizmet sorumlusu oluşturma hakkında yönergeler için bkz: [Azure Stack için uygulamalar erişim sağlayın][]. İstemci kimliği olarak da bilinir uygulama bir hizmet sorumlusu oluştururken kimliğidir.

7.  Hizmet sorumlunuzu aboneliğinizde katkıda bulunan/sahip rolü olduğundan emin olun. Hizmet sorumlusuna bir rol atamak yönergeler için bkz: [Azure Stack için uygulamalar erişim sağlayın][].

## <a name="prerequisites"></a>Önkoşullar

Azure .NET SDK'sı, Azure Stack ile kullanmak için aşağıdaki değerleri girin ve ardından ortam değişkenleriyle değerleri ayarlayın. Ortam değişkenlerini ayarlamak için tablonun işletim sisteminiz için aşağıdaki yönergelere bakın.

| Değer                     | Ortam değişkenleri   | Açıklama                                                                                                             |
|---------------------------|-------------------------|-------------------------------------------------------------------------------------------------------------------------|
| Kiracı Kimliği                 | AZURE_TENANT_ID       | Azure Stack değerini [ *Kiracı kimliği*][].                                                                          |
| İstemci Kimliği                 | AZURE_CLIENT_ID       | Hizmet sorumlusunun bu makalenin önceki bölümde oluşturulduğunda kayıtlı sorumlusu uygulama kimliği hizmeti. |
| Abonelik Kimliği           | AZURE_SUBSCRIPTION_ID | [ *Abonelik kimliği* ][] nasıl, teklifler eriştiği Azure Stack'te.                                                      |
| İstemci Gizli Anahtarı             | AZURE_CLIENT_SECRET   | Hizmet sorumlusu oluşturulurken kaydedilen hizmet sorumlusu uygulama gizli anahtarı.                                      |
| Resource Manager uç noktası | ARM_ENDPOINT           | Bkz: [ *Azure Stack Kaynak Yöneticisi uç noktası*][].                                                                    |
| Konum                  | RESOURCE_LOCATION     | Azure Stack için konum.

Azure Stack için Kiracı Kimliğini bulmak için bulunan yönergeleri takip [burada](../azure-stack-csp-ref-operations.md). Ortam değişkenlerini ayarlamak için aşağıdaki adımları uygulayın:

### <a name="microsoft-windows"></a>Microsoft Windows

Bir Windows Komut İstemi'nde ortam değişkenlerini ayarlamak için aşağıdaki biçimi kullanın:

```shell
Set Azure_Tenant_ID=Your_Tenant_ID
```

### <a name="macos-linux-and-unix-based-systems"></a>macOS, Linux ve UNIX tabanlı sistemlerde

UNIX tabanlı sistemlerde, aşağıdaki komutu kullanabilirsiniz:

```shell
Export Azure_Tenant_ID=Your_Tenant_ID
```

### <a name="the-azure-stack-resource-manager-endpoint"></a>Azure Stack Kaynak Yöneticisi uç noktası

Microsoft Azure Resource Manager dağıtma, yönetme ve Azure kaynaklarını izleme olanağı tanıyan bir yönetim çerçevesidir. Azure Resource Manager, bir grup olarak yerine tek tek bir işlemde bu görevleri işleyebilir.

Resource Manager uç noktasından meta veri bilgilerini alabilirsiniz. Uç nokta, kodunuzu çalıştırmak için gereken bilgileri bir JSON dosyası döndürür.

Aşağıdaki konuları göz önünde bulundurun:

- **ResourceManagerUrl** olan Azure Stack geliştirme Seti'ni (ASDK): https://management.local.azurestack.external/

- **ResourceManagerUrl** tümleşik sisteminde: `https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/` Gerekli meta verilerini almak için: `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0`

Örnek JSON dosyası:

```json
{ 
   "galleryEndpoint": "https://portal.local.azurestack.external:30015/",
   "graphEndpoint": "https://graph.windows.net/",
   "portal Endpoint": "https://portal.local.azurestack.external/",
   "authentication": 
      {
      "loginEndpoint": "https://login.windows.net/",
      "audiences": ["https://management.yourtenant.onmicrosoft.com/3cc5febd-e4b7-4a85-a2ed-1d730e2f5928"]
      }
}
```

## <a name="existing-api-profiles"></a>Mevcut API profilleri

1.  **Microsoft.Azure.Management.Profiles.hybrid\_2018\_03\_01. *ResourceProvider*. 0.9.0-preview.nupkg**: Azure Stack için yerleşik son profili. En Azure Stack ile uyumlu 1808 damgada olduğu sürece veya diğer hizmetler için bu profili kullanın.

2.  **Microsoft.Azure.Management.Profiles.hybrid\_2017\_03\_09. *ResourceProvider*. 0.9.0-preview.nupkg**: 1808 yapı düşük bir damga kullanıyorsanız, bu profili kullanın.

3.  **En son**: Tüm hizmetler en son sürümlerine oluşan profili. Tüm hizmetler en son sürümlerini kullanın. Bu profili parçasıdır **Microsoft.Azure.Management** NuGet paketi.

Azure Stack ve API profilleri hakkında daha fazla bilgi için bkz. bir [API profillerini özeti][].

## <a name="azure-net-sdk-api-profile-usage"></a>Azure .NET SDK'sı API profili kullanımı

Aşağıdaki kod, bir kaynak yönetim istemcisi oluşturmak için kullanılmalıdır. Benzer bir kod (örneğin, işlem, ağ ve depolama) başka bir kaynak sağlayıcı örneği için kullanılabilir istemciler. 

```csharp
var client = new ResourceManagementClient(armEndpoint, credentials)
{
    SubscriptionId = subscriptionId
};
```

`credentials` Parametresinde Yukarıdaki kod, bir istemci örneği oluşturmak için gereklidir. Aşağıdaki kod, Kiracı kimliği ve hizmet sorumlusu tarafından bir kimlik doğrulama belirteci oluşturur.

```csharp
var azureStackSettings = getActiveDirectoryServiceSettings(armEndpoint);
var credentials = ApplicationTokenProvider.LoginSilentAsync(tenantId, servicePrincipalId, servicePrincipalSecret, azureStackSettings).GetAwaiter().GetResult();
```
`getActiveDirectoryServiceSettings` Çağrı kodda, Azure Stack uç noktaları meta veri uç noktasından alır. Bu, yapılan çağrı ortam değişkenlerinden durumları: 

```csharp
public static ActiveDirectoryServiceSettings getActiveDirectoryServiceSettings(string armEndpoint)
{
    var settings = new ActiveDirectoryServiceSettings();
    try
    {
        var request = (HttpWebRequest)HttpWebRequest.Create(string.Format("{0}/metadata/endpoints?api-version=1.0", armEndpoint));
        request.Method = "GET";
        request.UserAgent = ComponentName;
        request.Accept = "application/xml";
        using (HttpWebResponse response = (HttpWebResponse)request.GetResponse())
        {
            using (StreamReader sr = new StreamReader(response.GetResponseStream()))
            {
                var rawResponse = sr.ReadToEnd();
                var deserialized = JObject.Parse(rawResponse);
                var authenticationObj = deserialized.GetValue("authentication").Value<JObject>();
                var loginEndpoint = authenticationObj.GetValue("loginEndpoint").Value<string>();
                var audiencesObj = authenticationObj.GetValue("audiences").Value<JArray>();
                settings.AuthenticationEndpoint = new Uri(loginEndpoint);
                settings.TokenAudience = new Uri(audiencesObj[0].Value<string>());
                settings.ValidateAuthority = loginEndpoint.TrimEnd('/').EndsWith("/adfs", StringComparison.OrdinalIgnoreCase) ? false : true;
            }
        }
    }
    catch (Exception ex)
    {
        Console.WriteLine(String.Format("Could not get AD service settings. Exception: {0}", ex.Message));
    }
    return settings;
}
```
Bu, uygulamanızı Azure Stack başarıyla dağıtmak için API profili NuGet paketlerini kullanmanıza olanak sağlayacaktır.

## <a name="samples-using-api-profiles"></a>API profillerini kullanma örnekleri

Aşağıdaki örnekler, .NET ve Azure Stack API profilleriyle çözümleri oluşturmak için referans olarak kullanılabilir.
- [Kaynak gruplarını yönetme](https://github.com/Azure-Samples/hybrid-resources-dotnet-manage-resource-group)
- [Depolama hesaplarını yönetme](https://github.com/Azure-Samples/hybird-storage-dotnet-manage-storage-accounts)
- [Bir sanal makineyi yönetin](https://github.com/Azure-Samples/hybrid-compute-dotnet-manage-vm)

## <a name="next-steps"></a>Sonraki adımlar

API profilleri hakkında daha fazla bilgi için bkz:

- [Azure stack'teki API sürümü profillerini yönetme](azure-stack-version-profiles.md)
- [Profilleri tarafından desteklenen kaynak sağlayıcısı API sürümleri](azure-stack-profiles-azure-resource-manager-versions.md)

  [Başlarken - Git yükleme]: https://git-scm.com/book/en/v2/Getting-Started-Installing-Git
  [Bulma ve bir paket yükleme]: /nuget/tools/package-manager-ui
  [NuGet Paket Yöneticisi yönergeleri]: https://marketplace.visualstudio.com/items?itemName=jmrog.vscode-nuget-package-manager
  [Azure Stack'te teklifleri abonelikleri oluşturma]: ../azure-stack-subscribe-plan-provision-vm.md
  [Azure Stack için uygulamalar erişim sağlayın]: ../azure-stack-create-service-principals.md
  [* Kiracı kimliği *]: ../azure-stack-identity-overview.md
  [* Abonelik kimliği *]: ../azure-stack-plan-offer-quota-overview.md#subscriptions
  [* Azure Stack Kaynak Yöneticisi uç noktası *]: ../user/azure-stack-version-profiles-ruby.md#the-azure-stack-resource-manager-endpoint
  [API profillerini özeti]: ../user/azure-stack-version-profiles.md#summary-of-api-profiles
  [Test Project to Virtual Machine, vNet, resource groups, and storage account]: https://github.com/seyadava/azure-sdk-for-net-samples/tree/master/TestProject
  [Use Azure PowerShell to create a service principal with a certificate]: ../azure-stack-create-service-principals.md
  [Run unit tests with Test Explorer.]: /visualstudio/test/run-unit-tests-with-test-explorer?view=vs-2017
