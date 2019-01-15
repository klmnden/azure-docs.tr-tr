---
title: Azure stack'teki GO ile API Sürüm profillerini kullanma | Microsoft Docs
description: Azure stack'teki GO ile API Sürüm profillerini kullanma hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2018
ms.author: sethm
ms.reviewer: sijuman
ms.openlocfilehash: 30969dcb7549f4107eb72b54d444a639c35b7ba0
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2019
ms.locfileid: "54264793"
---
# <a name="use-api-version-profiles-with-go-in-azure-stack"></a>Azure stack'teki Git ile API Sürüm profillerini kullanma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

## <a name="go-and-version-profiles"></a>Go ve sürüm profilleri

Bir profili farklı sürümlerini farklı hizmetlerden farklı kaynak türlerinin birleşimidir. Bir profil kullanarak, karıştırın ve eşleştirin farklı kaynak türleri arasında yardımcı olur. Profilleri sağlayabilir:

 - Kararlılık, uygulamanızın belirli API sürümleri için kilitleme tarafından.
 - Azure Stack ve bölgesel Azure veri merkezleri ile uyumluluğu, uygulamanız için.

Go SDK profiller profiller altında kullanılabilir / sürümü yolu **YYYY-AA-GG** biçimi. Şu anda en son Azure profil sürümü Stack **2017-03-09**. Belirli bir hizmete bir profilden içeri aktarmak için karşılık gelen modülü profilinden içeri aktarmak gerekir. Örneğin, içeri aktarmak için **işlem** hizmetinde **2017-03-09** profili:

````go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/compute/mgmt/compute" 
````

## <a name="install-azure-sdk-for-go"></a>Go için Azure SDK'sını yükleme

  1. Git'i yükleyin. Yönergeler için [Git'i yükledikten Başlarken -](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
  2. Yükleme [Go programlama dili](https://golang.org/dl).  
  Azure için API profillerini Go 1.9 veya daha yeni bir sürümü gerektirir.
  3. Aşağıdaki bash komutunu çalıştırarak Azure Go SDK'sını ve bağımlılıklarını yükleyin:
  ```
    go get -u -d github.com/Azure/azure-sdk-for-go/...
  ```

### <a name="the-go-sdk"></a>GO SDK'sı

Azure SDK Git hakkında daha fazla bilgi bulabilirsiniz:
- Azure SDK, Go [Go için Azure SDK'sını yükleme](https://docs.microsoft.com/go/azure/azure-sdk-go-install).
- Azure Go SDK'sı github'da herkes tarafından kullanılabilir [go için azure SDK'sı](https://github.com/Azure/azure-sdk-for-go).

### <a name="go-autorest-dependencies"></a>Git AutoRest bağımlılıkları

GO SDK'sı, Azure Resource Manager uç noktaları için REST istekleri göndermek için Azure Git-AutoRest modülleri bağlıdır. Azure Git-AutoRest modülü bağımlılıklardan aktarmanız gerekecektir [Azure Git-AutoRest github'da](https://github.com/Azure/go-autorest). Yükleme bash komutları bulabilirsiniz **yükleme** bölümü.

## <a name="how-to-use-go-sdk-profiles-on-azure-stack"></a>Azure Stack'te GO SDK profilleri kullanma

Azure Stack'te Go kod örneği çalıştırmak için:
  1. Azure SDK, Go ve onun bağımlılıklarını yükleyin. Yönerge için önceki bölüme bakın [Go için Azure SDK yükleme](#install-azure-sdk-for-go).
  2. Resource Manager uç noktasından meta veri bilgilerini alın. Uç nokta Git kodunuzu çalıştırmak için gerekli bilgileri bir JSON dosyası döndürür.

  > [!Note]  
  > **ResourceManagerUrl** olan Azure Stack geliştirme Seti'ni (ASDK): `https://management.local.azurestack.external/`  
  > **ResourceManagerUrl** tümleşik sisteminde: `https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/`  
  > Gerekli meta verilerini almak için: `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0`
  
  Örnek JSON dosyası:

  ```json
  { "galleryEndpoint": "https://portal.local.azurestack.external:30015/",  
    "graphEndpoint": "https://graph.windows.net/",  
    "portal Endpoint": "https://portal.local.azurestack.external/", 
    "authentication": {
      "loginEndpoint": "https://login.windows.net/", 
      "audiences": ["https://management.<yourtenant>.onmicrosoft.com/3cc5febd-e4b7-4a85-a2ed-1d730e2f5928"]
    }
  }
  ```

  3. Yoksa, bir abonelik oluşturur ve daha sonra kullanılmak üzere abonelik Kimliğini kaydedin. Abonelik oluşturma hakkında daha fazla bilgi için bkz. [Azure Stack'te teklifleri abonelikleri oluşturma](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm). 
  4. "Abonelik" kapsam ile hizmet sorumlusu oluşturma ve **sahibi** rol. Hizmet sorumluları, Kimliğini ve parolasını kaydedin. Azure Stack için hizmet sorumlusu oluşturma hakkında daha fazla bilgi için bkz. [hizmet sorumlusu oluşturma](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals#create-service-principal-for-azure-ad). Azure Stack ortamınızı ayarlayın.
  5. Bir hizmeti modülü kodunuzda Go SDK profilinden içeri aktarın. Azure Stack profilinin geçerli sürümü **2017-03-09**. Örneğin, ağ modülünü içeri aktarmak için **2017-03-09** profil türü: 

  ````go
    package main 
    import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network"
  ````
  
  6. İşlevinizi oluşturun ve bir istemci ile kimlik doğrulaması bir **yeni** istemci işlev çağrısı. Bir sanal ağ istemcisi oluşturmak için aşağıdaki kodu kullanabilirsiniz:  

  ````go
  package main 

  import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network" 

  func main() { 
      vnetClient := network.NewVirtualNetworksClientWithBaseURI("<baseURI>", "(subscriptionID>")
      vnetClient.Authorizer = autorest.NewBearerAuthorizer(token)
  ````

  Ayarlama `<baseURI>` için **ResourceManagerUrl** iki adımda kullanılan değer.
  Ayarlama `<subscriptionID>` için **Subscriptionıd** üç adımda kaydedilen değeri.
  Belirteci oluşturmak için bkz: kimlik doğrulaması bölümüne bakın.  

  7. Önceki adımda oluşturduğunuz istemcisini kullanarak API yöntemleri çağırır. Örneğin, önceki adımda istemci kullanarak bir sanal ağ oluşturmak için şunu yazın: 
  
````go
package main

import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network" 
func main() { 
  vnetClient := network.NewVirtualNetworksClientWithBaseURI("<baseURI>", "(subscriptionID>")
  vnetClient .Authorizer = autorest.NewBearerAuthorizer(token)

  vnetClient .CreateOrUpdate( ) 
````
  
  Go SDK profilini kullanarak Azure Stack üzerinde bir sanal ağ oluşturma tam bir örnek için bkz [örnek](#example).

## <a name="authentication"></a>Kimlik Doğrulaması

Azure Active Go SDK'sını kullanarak Directory'den yetkilendirici özelliği almak için Git AutoRest modüllerini yükleyin. Bu modüller zaten "Go SDK" yüklemeye yüklenmiş olması gerekir; Aksi takdirde, yükleme [github'da kimlik doğrulama paketi](https://github.com/Azure/go-autorest/tree/master/autorest/adal).

Yetkilendirici yetkilendirici kaynağı istemcisi olarak ayarlanmalıdır. Bir yetkilendirici almak için farklı yöntemler vardır. bir tam listesini görmek için buraya.

Bu bölümde, istemci kimlik bilgilerini kullanarak Azure Stack'te yetkilendirici belirteçlerini almak için ortak bir yol sunar:

  1. Abonelikte sahip rolü hizmet sorumlusuyla varsa, bu adımı atlayın. Aksi takdirde bir hizmet sorumlusu oluşturma [yönergeleri]( https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals) ve aboneliğinize kapsamlı bir "sahip" rol atayın [yönergeleri]( https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals#assign-role-to-service-principal). Hizmet sorumlusunun uygulama Kimliğini ve parolasını kaydedin. 

  2. İçeri aktarma **adal** Git AutoRest paketinden kodunuzda. 
  
  ````go
  package main
  import "github.com/Azure/go-autorest/autorest/adal" 
  ````

  3. NewOAuthConfig yöntemini kullanarak bir oauthConfig oluşturma **adal** modülü. 
  
  ````go
  package main 

  import "github.com/Azure/go-autorest/autorest/ada1" 

  func CreateToken() (adal.OAuthTokenProvider, error) {
      var token adal.OAuthTokenProvider
      oauthConfig, err := adal.NewOAuthConfig(activeDirectoryEndpoint, tenantID)
  ````
   
  Ayarlama `<activeDirectoryEndpoint>` "loginEndpoint" değerine ResourceManagerUrl meta veri özelliği bu belgenin önceki bölümde üzerinde alınır.
  Ayarlama `<tenantID>` Azure Stack Kiracı kimliğiniz için değeri 

  4. Son olarak, adal modülündeki NewServicePrincipalToken yöntemini kullanarak bir hizmet sorumlusu belirteci oluşturun. 

  ````go
  package main 

  import "github.com/Azure/go-autorest/autorest/adal" 

  func CreateToken() (adal.OAuthTokenProvider, error) {
      var token adal.OAuthTokenProvider
      oauthConfig, err := adal.NewOAuthConfig(activeDirectoryEndpoint, tenantID)
      token, err = adal.NewServicePrincipalToken(
          *oauthConfig,
          clientID,
          clientSecret,
          activeDirectoryResourceID)
      return token, err
  ````
  
  Ayarlama `<activeDirectoryResourceID>` ResourceManagerUrl meta verileri listeden bir değerler "audience" Bu belgenin önceki bölümde üzerinde alınan.  
  Ayarlama `<clientID>` hizmet sorumlusu uygulama kimliği kaydettiğiniz hizmet sorumlusu, bu belgenin önceki bölümde üzerinde oluşturulduğu zaman.  
  Ayarlama `<clientSecret>` hizmet sorumlusu, bu belgenin önceki bölümde üzerinde oluşturulduğu sırada hizmet sorumlusu uygulama gizli anahtarı kaydedildi.  

## <a name="example"></a>Örnek

Bu bölümde, Azure Stack üzerinde sanal ağ oluşturmak için Go kod örneği gösterilmektedir. Go SDK'ın tam örnekler için bkz [Azure Go SDk örnekleri depomuzdan](https://github.com/Azure-Samples/azure-sdk-for-go-samples). Azure Stack örnekleri karma altında kullanılabilir / hizmet klasör deponun içinde yolu.

> [!Note]  
> Bu örnekte, kodu çalıştırmak için kullanılan abonelik sahip olduğunu doğrulayın **ağ** kaynak sağlayıcısı olarak listelenen **kayıtlı**. Doğrulamak için Azure Stack portalında abonelik bulun ve tıklayarak **kaynak sağlayıcıları.**

1. Kodunuzda gerekli paketleri içeri aktarın. Ağ modülü içeri aktarmak için Azure Stack üzerinde en son kullanılabilir profili kullanmanız gerekir. 
  
  ````go
  package main

  import (
      "context"
      "fmt"
      "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network"
      "github.com/Azure/go-autorest/autorest"
      "github.com/Azure/go-autorest/autorest/adal"
      "github.com/Azure/go-autorest/autorest/to"
  )
  ````

2. Ortam değişkenlerini tanımlayın. Bir sanal ağ oluşturmak için bir kaynak grubu olması gerekir. 

  ````go
  var (
      activeDirectoryEndpoint = "yourLoginEndpointFromResourceManagerUrlMetadata"
      tenantID = "yourAzureStackTenantID"
      clientID = "yourServicePrincipalApplicationID"
      clientSecret = "yourServicePrincipalSecret"
      activeDirectoryResourceID = "yourAudienceFromResourceManagerUrlMetadata"
      subscriptionID = "yourSubscriptionID"
      baseURI = "yourResourceManagerURL"
      resourceGroupName = "existingResourceGroupName"
  )
  ````

3. Ortam değişkenlerini tanımladığınıza göre kullanarak kimlik doğrulama belirteci oluşturmak için bir yöntem ekleyin **adal** paket. Önceki bölümde kimlik doğrulaması hakkındaki ayrıntıları bakın.
  
  ````go
  //CreateToken creates a service principal token
  func CreateToken() (adal.OAuthTokenProvider, error) {
      var token adal.OAuthTokenProvider
      oauthConfig, err := adal.NewOAuthConfig(activeDirectoryEndpoint, tenantID)
      token, err = adal.NewServicePrincipalToken(
          *oauthConfig,
          clientID,
          clientSecret,
          activeDirectoryResourceID)
      return token, err
  }
  ````

4. Main yöntemini ekleyin. Main yöntemini önce önceki adımda tanımlanan yöntemini kullanarak bir belirteç alır. Ardından, Profilinden ağ modülü kullanarak bir istemci oluşturur. Son olarak, bir sanal ağ oluşturur. 
  
````go
package main

import (
    "context"
    "fmt"
    "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network"
    "github.com/Azure/go-autorest/autorest"
    "github.com/Azure/go-autorest/autorest/adal"
    "github.com/Azure/go-autorest/autorest/to"
)

var (
    activeDirectoryEndpoint = "yourLoginEndpointFromResourceManagerUrlMetadata"
    tenantID = "yourAzureStackTenantID"
    clientID = "yourServicePrincipalApplicationID"
    clientSecret = "yourServicePrincipalSecret"
    activeDirectoryResourceID = "yourAudienceFromResourceManagerUrlMetadata"
    subscriptionID = "yourSubscriptionID"
    baseURI = "yourResourceManagerURL"
    resourceGroupName = "existingResourceGroupName"
)

//CreateToken creates a service principal token
func CreateToken() (adal.OAuthTokenProvider, error) {
    var token adal.OAuthTokenProvider
    oauthConfig, err := adal.NewOAuthConfig(activeDirectoryEndpoint, tenantID)
    token, err = adal.NewServicePrincipalToken(
        *oauthConfig,
        clientID,
        clientSecret,
        activeDirectoryResourceID)
    return token, err
}

func main() {
    token, _ := CreateToken()
    vnetClient := network.NewVirtualNetworksClientWithBaseURI(baseURI, subscriptionID)
    vnetClient.Authorizer = autorest.NewBearerAuthorizer(token)
    future, _ := vnetClient.CreateOrUpdate(
        context.Background(),
        resourceGroupName,
        "sampleVnetName",
        network.VirtualNetwork{
            Location: to.StringPtr("local"),
            VirtualNetworkPropertiesFormat: &network.VirtualNetworkPropertiesFormat{
                AddressSpace: &network.AddressSpace{
                    AddressPrefixes: &[]string{"10.0.0.0/8"},
                },
                Subnets: &[]network.Subnet{
                    {
                        Name: to.StringPtr("subnetName"),
                        SubnetPropertiesFormat: &network.SubnetPropertiesFormat{
                            AddressPrefix: to.StringPtr("10.0.0.0/16"),
                        },
                    },
                },
            },
        })
    err := future.WaitForCompletion(context.Background(), vnetClient.Client)
    if err != nil {
        fmt.Printf(err.Error())
        return
    }
}

````

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stack için PowerShell yükleme](azure-stack-powershell-install.md)
* [Azure Stack kullanıcının PowerShell ortamını yapılandırma](azure-stack-powershell-configure-user.md)  
