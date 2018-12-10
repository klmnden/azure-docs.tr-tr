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
ms.openlocfilehash: cfebbdb9b88a1de6a05f06e6ed72ebc9cddddcf6
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53074460"
---
# <a name="use-api-version-profiles-with-net-in-azure-stack"></a>. NET'te Azure Stack ile API Sürüm profillerini kullanma

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

.NET SDK'sı için Azure Stack Kaynak Yöneticisi'ni oluşturmanıza ve altyapınızı yönetmenize yardımcı olacak araçlar sağlar. İşlem, ağ, depolama, uygulama hizmetleri, SDK kaynak sağlayıcılarını içerir ve [KeyVault](../../key-vault/key-vault-whatis.md). .NET SDK'sı, profil bilgileri bir araya getiren projesi çözümünüze her defasında karşıdan yüklenmesi gereken 14 NuGet paketlerini içerir. Ancak, özellikle hangi kaynak sağlayıcısı yükleyebilir, uygulamanız için bellek iyileştirmek için 2018-03-01-karma veya 2017-03-09-profile için kullanır. Her paket, kaynak sağlayıcısı, ilgili API sürümü ve ait olduğu API profili oluşur. .NET SDK'sı API profillerinde genel Azure kaynakları ve Azure Stack'te kaynakları arasında geçiş yardımcı olarak karma bulut geliştirmeyi etkinleştirin.

## <a name="net-and-api-version-profiles"></a>.NET ve API sürümü profillerini

Bir API profili, kaynak sağlayıcıları ve API sürümlerini birleşimidir. Bir API profili, bir kaynak sağlayıcısı paketindeki her kaynak türünün en son ve en çok kararlı sürümünü almak için kullanabilirsiniz.

-   Tüm hizmetler en son sürümlerini kullanın, yapmak **son** paketlerin profili. Bu profili parçasıdır **Microsoft.Azure.Management** NuGet paketi.

-   Azure Stack ile uyumlu hizmetleri kullanmak için **Microsoft.Azure.Management.Profiles.hybrid\_2018\_03\_01. *ResourceProvider*. 0.9.0-preview.nupkg** veya **Microsoft.Azure.Management.Profiles.hybrid\_2017\_03\_09. *ResourceProvider*. 0.9.0-preview.nupkg** paketleri.

    -   Her bir profil için her kaynak sağlayıcısı için iki paket vardır.

    -   Emin **ResourceProvider** bölümü yukarıdaki NuGet paketinin doğru sağlayıcıya değiştirilir.

-   Bir hizmetin en son API-version'ı kullanmak için **son** belirli NuGet paketinin profili. Örneğin kullanmak istiyorsanız, **son API** işlem hizmetinin sürümü tek başına kullanım **son** profilini **işlem** paket. **Son** profilidir parçası **Microsoft.Azure.Management** NuGet paketi.

-   Belirli bir kaynak sağlayıcısındaki bir kaynak türü için belirli API sürümlerini kullanmak için bir paket içinde tanımlanan belirli API sürümlerini kullanın.

Tüm seçeneklerin aynı uygulamada birleştirebilirsiniz unutmayın.

## <a name="install-the-azure-net-sdk"></a>Azure .NET SDK'sını yükleyin

1.  Git'i yükleyin. Yönergeler için [Başlarken - Git yükleme][].

2.  Doğru NuGet paketlerini yüklemek için bkz [bulma ve bir paket yükleme][].

3.  Yüklenmesi gereken paketleri kullanmak istediğiniz profili sürümüne bağlıdır. Paket adı profil sürümleri şunlardır:

    1.  **Microsoft.Azure.Management.Profiles.hybrid\_2018\_03\_01. *ResourceProvider*. 0.9.0-preview.nupkg**

    2.  **Microsoft.Azure.Management.Profiles.hybrid\_2017\_03\_09. *ResourceProvider*. 0.9.0-preview.nupkg**

4.  Visual Studio Code için doğru NuGet paketlerini yüklemek için indirmek için aşağıdaki bağlantıya bakın [NuGet Paket Yöneticisi yönergeleri][].

5.  Yoksa, bir abonelik oluşturur ve daha sonra kullanılmak üzere abonelik Kimliğini kaydedin. Bir abonelik oluşturmak yönergeler için bkz: [Azure Stack'te teklifleri abonelikleri oluşturma][].

6.  Hizmet sorumlusu oluşturma ve istemci Kimliğini ve istemci gizli anahtarını kaydedin. Azure Stack için hizmet sorumlusu oluşturma hakkında yönergeler için bkz: [Azure Stack için uygulamalar erişim sağlayın][]. İstemci kimliği olarak da bilinen uygulama kimliği bir hizmet sorumlusu oluştururken olduğuna dikkat edin.

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

Azure Stack için Kiracı Kimliğini bulmak için bulunan yönergeleri takip [burada](../azure-stack-csp-ref-operations.md). Ortam değişkenlerini ayarlamak için aşağıdakileri yapın:

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

- **ResourceManagerUrl** tümleşik sistemlerindeki: `https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/` gerekli meta verilerini almak için: `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0`

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

1.  **Microsoft.Azure.Management.Profiles.hybrid\_2018\_03\_01. *ResourceProvider*. 0.9.0-preview.nupkg**: son profil, Azure Stack için yerleşik. En Azure Stack ile uyumlu 1808 damgada olduğu sürece veya diğer hizmetler için bu profili kullanın.

2.  **Microsoft.Azure.Management.Profiles.hybrid\_2017\_03\_09. *ResourceProvider*. 0.9.0-preview.nupkg**: 1808 yapı düşük bir damga kullanıyorsanız, bu profili kullanın.

3.  **En son**: tüm hizmetleri en son sürümlerine oluşan profili. Tüm hizmetler en son sürümlerini kullanın. Bu profili parçasıdır **Microsoft.Azure.Management** NuGet paketi.

Azure Stack ve API profilleri hakkında daha fazla bilgi için bkz. bir [API profillerini özeti][].

## <a name="azure-net-sdk-api-profile-usage"></a>Azure .NET SDK'sı API profili kullanımı

Aşağıdaki kod, profil istemci örneklemek için kullanılmalıdır. Bu parametre yalnızca Azure Stack veya diğer özel bulut için gereklidir. Küresel Azure, bu ayarlar varsayılan olarak zaten sahiptir.

Aşağıdaki kod, Azure Stack üzerinde hizmet sorumlusunun kimliğini doğrulamak için gereklidir. Kimliği ve Azure Stack için özel kimlik doğrulaması temel Kiracı tarafından bir belirteç oluşturur.

```csharp
public class CustomLoginCredentials : ServiceClientCredentials
{
    private string clientId;
    private string clientSecret;
    private string resourceId;
    private string tenantId;

    private const string authenticationBase = "https://login.windows.net/{0}";

    public CustomLoginCredentials(string servicePrincipalId, string servicePrincipalSecret, string azureEnvironmentResourceId, string azureEnvironmentTenandId)
    {
        clientId = servicePrincipalId;
        clientSecret = servicePrincipalSecret;
        resourceId = azureEnvironmentResourceId;
        tenantId = azureEnvironmentTenandId;
    }
```

Bu, uygulamanızı Azure Stack başarıyla dağıtmak için API profili NuGet paketlerini kullanmanıza olanak sağlayacaktır.

## <a name="define-azure-stack-environment-setting-functions"></a>Azure Stack ortamı ayarı işlevleri tanımlayın

Azure Stack ortamına hizmet sorumlusunun kimliğini doğrulamak için aşağıdaki kodu kullanın:

```csharp
private string AuthenticationToken { get; set; }
public override void InitializeServiceClient<T>(ServiceClient<T> client)
{
    var authenticationContext = new AuthenticationContext(String.Format(authenticationBase, tenantId));
    var credential = new ClientCredential(clientId, clientSecret);
    var result = authenticationContext.AcquireTokenAsync(resource: resourceId,
    clientCredential: credential).Result;
    if (result == null)
    {
        throw new InvalidOperationException("Failed to obtain the JWT token");
    }
    AuthenticationToken = result.AccessToken;
}
```

Bu başlatma hizmeti istemcisi, Azure Stack için kimlik doğrulaması için geçersiz kılar.

## <a name="samples-using-api-profiles"></a>API profillerini kullanma örnekleri

.NET ve Azure Stack API profilleriyle çözümleri oluşturmaya yönelik bir başvuru olarak GitHub depoları içinde bulunan aşağıdaki örnekleri kullanabilirsiniz.

-   [Sanal makine, sanal ağ, kaynak grupları ve depolama hesabı için test projesi][]
-   .NET kullanarak sanal makineleri yönetme

### <a name="sample-unit-test-project"></a>Örnek birim testi projesi 

1.  Aşağıdaki komutu kullanarak depoyu kopyalayın:

    ```shell
    git clone https://github.com/Azure-Samples/hybrid-compute-dotnet-manage-vm.git
    ```

2.  Bir Azure hizmet sorumlusu oluşturma ve aboneliğe erişmek için bir rol atayın. Bir hizmet sorumlusu oluşturma hakkında yönergeler için bkz: [Bir sertifika ile hizmet sorumlusu oluşturmak için Azure PowerShell'i kullanma][].

3.  Aşağıdaki gerekli değerleri alın:

    1.  Kiracı Kimliği
    2.  İstemci Kimliği
    3.  İstemci Gizli Anahtarı
    4.  Abonelik Kimliği
    5.  Resource Manager uç noktası

4.  Komut istemi kullanarak oluşturduğunuz asıl hizmetinden alınan bilgileri kullanarak, aşağıdaki ortam değişkenlerini ayarlayın:

    1.  dışarı aktarma AZURE_TENANT_ID {Kiracı kimliğinizi} =
    2.  dışarı aktarma AZURE_CLIENT_ID {istemci kimliğiniz} =
    3.  dışarı aktarma AZURE_CLIENT_SECRET {istemci gizli anahtarı} =
    4.  dışarı aktarma AZURE_SUBSCRIPTION_ID {abonelik kimliğinizi} =
    5.  dışarı aktarma ARM_ENDPOINT {, Azure Stack Kaynak Yöneticisi URL'si} =

   Windows içinde kullanmak **ayarlamak** yerine **dışarı**.

5.  Azure Stack konumunuza konumu değişkeninin ayarlandığından emin olun. Örneğin, yerel = "local".

6.  Azure Stack için kimlik doğrulaması sağlayacak özel oturum açma kimlik bilgilerini ayarlayın. Bu kod bölümünü yetkilendirme klasöründe bu örnekteki eklendiğini unutmayın.

   ```csharp
   public class CustomLoginCredentials : ServiceClientCredentials
   {
       private string clientId;
       private string clientSecret;
       private string resourceId;
       private string tenantId;
       private const string authenticationBase = "https://login.windows.net/{0}";
       public CustomLoginCredentials(string servicePrincipalId, string servicePrincipalSecret, string azureEnvironmentResourceId, string azureEnvironmentTenandId)
       {
           clientId = servicePrincipalId;
           clientSecret = servicePrincipalSecret;
           resourceId = azureEnvironmentResourceId;
           tenantId = azureEnvironmentTenandId;
       }
   private string AuthenticationToken { get; set; }
   ```

7.  Azure Stack, Azure Stack için kimlik doğrulaması için başlatma hizmeti istemcisi geçersiz kılmak için kullanıyorsanız, aşağıdaki kodu ekleyin. Yetkilendirme klasöründe bu örnekteki kod bir kısmı zaten dahil olduğunu unutmayın.

   ```csharp
   public override void InitializeServiceClient<T>(ServiceClient<T> client)
   {
      var authenticationContext = new AuthenticationContext(String.Format(authenticationBase, tenantId));
      var credential = new ClientCredential(clientId, clientSecret);
      var result = authenticationContext.AcquireTokenAsync(resource: resourceId,
                clientCredential: credential).Result;
      if (result == null)
      {
          throw new InvalidOperationException("Failed to obtain the JWT token");
      }
      AuthenticationToken = result.AccessToken;
   }
   ```
 
8.  NuGet Paket Yöneticisi'ni kullanarak "2018-03-01-karma için" işlem, ağ, depolama, KeyVault ve uygulama hizmetleri kaynak sağlayıcıları için bu profil ile ilişkili paketlerini arayıp yükleyin.

2.  .Cs dosyasındaki her bir görev içinde Azure Stack ile çalışmak için gerekli olan parametreleri ayarlayın. Örnek görev için şu şekilde gösterilir `CreateResourceGroupTest`:

   ```csharp
   var location = Environment.GetEnvironmentVariable("AZURE_LOCATION");
   var baseUriString = Environment.GetEnvironmentVariable("AZURE_BASE_URL");
   var resourceGroupName = Environment.GetEnvironmentVariable("AZURE_RESOURCEGROUP");
   var servicePrincipalId = Environment.GetEnvironmentVariable("AZURE_CLIENT_ID");
   var servicePrincipalSecret = Environment.GetEnvironmentVariable("AZURE_CLIENT_SECRET");
   var azureResourceId = Environment.GetEnvironmentVariable("AZURE_RESOURCE_ID");
   var tenantId = Environment.GetEnvironmentVariable("AZURE_TENANT_ID");
   var subscriptionId = Environment.GetEnvironmentVariable("AZURE_SUBSCRIPTION_ID");
   var credentials = new CustomLoginCredentials(servicePrincipalId, servicePrincipalSecret, azureResourceId, tenantId);
   ```

1.  Sağ tıklayın her görev ve select **test çalıştırması**.

    1.  Her görev verilen parametrelere göre başarıyla oluşturulduğunu yeşil onay işaretleri koyacağız yan bölme penceresi hakkında sizi uyarır. Kaynakları başarıyla oluşturulup oluşturulmadığını emin olmak için Azure Stack aboneliğine gözden geçirin.

    2.  Birim testleri çalıştırma hakkında daha fazla bilgi için bkz. [Test Gezgini ile birim testleri çalıştırın.][]

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
  [Sanal makine, sanal ağ, kaynak grupları ve depolama hesabı için test projesi]: https://github.com/seyadava/azure-sdk-for-net-samples/tree/master/TestProject
  [Bir sertifika ile hizmet sorumlusu oluşturmak için Azure PowerShell'i kullanma]: ../azure-stack-create-service-principals.md
  [Test Gezgini ile birim testleri çalıştırın.]: /visualstudio/test/run-unit-tests-with-test-explorer?view=vs-2017
