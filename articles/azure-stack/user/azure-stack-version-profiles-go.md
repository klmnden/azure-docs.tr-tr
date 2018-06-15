---
title: API sürümü profillerini Azure yığınında Git ile kullanma | Microsoft Docs
description: Azure yığınında Git ile API sürümü profillerini kullanma hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: mabrigg
ms.reviewer: sijuman
ms.openlocfilehash: dd2d0c46c0829a73d32c96b506b9f2111eda3c84
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "34010073"
---
# <a name="use-api-version-profiles-with-go-in-azure-stack"></a>Azure yığınında Git ile API sürümü profilleri kullanma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

## <a name="go-and-version-profiles"></a>Git ve sürüm profilleri

Bir profil farklı hizmetlerinden farklı sürümlerini farklı kaynak türlerinin birleşimidir. Bir profili kullanarak karıştırmak ve farklı kaynak türleri arasında eşleşen yardımcı olur. Profilleri sağlayabilir:

 - Kararlılık belirli API sürümleri için kilitleme tarafından uygulamanız için.
 - Azure yığını ve bölgesel Azure veri merkezleri ile uyumluluğu, uygulamanız için.

Git SDK profilleri profiller altında kullanılabilir / yoluyla kendi sürümünde **YYYY-AA-GG** biçimi. Şimdi, en son Azure profil sürümü yığınının **2017-03-09**. Belirli bir hizmeti bir profilden içeri aktarmak için karşılık gelen modülü profilden içeri aktarmanız gerekir. Örneğin, içeri aktarmak için **işlem** gelen hizmet **2017-03-09** profili:

````go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/compute/mgmt/compute" 
````

## <a name="install-azure-sdk-for-go"></a>Git için Azure SDK'sını yükleyin

  1. Git'i yükleyin. Yönergeler için bkz: [Git yükleme Başlarken -](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
  2. Yükleme [Git programlama dili](https://golang.org/dl).  
  Azure için API profilleri Git 1.9 veya daha yeni sürümünü gerektirir.
  3. Git Azure SDK'sını ve bağımlılıklarını aşağıdaki bash komutu çalıştırarak yükleyin:
  ```
    go get -u -d github.com/Azure/azure-sdk-for-go/...
  ```

### <a name="the-go-sdk"></a>Git SDK'sı

Git Azure SDK hakkında daha fazla bilgi bulabilirsiniz:
- Azure SDK Git [Git için Azure SDK'sını yükleme](https://docs.microsoft.com/go/azure/azure-sdk-go-install).
- Azure Git SDK'sı github'da genel olarak kullanılabilir [Git için azure sdk](https://github.com/Azure/azure-sdk-for-go).

### <a name="go-autorest-dependencies"></a>Git AutoRest bağımlılıkları

Azure Resource Manager Uç noktalara REST istekleri göndermek için Azure Git-AutoRest modülleri Git SDK bağlıdır. Azure Git-AutoRest modülü bağımlılıklardan içe aktarmanız gerekir [Azure Git-AutoRest github'da](https://github.com/Azure/go-autorest). Yükleme bash komutlar bulabilirsiniz **yükleme** bölümü.

## <a name="how-to-use-go-sdk-profiles-on-azure-stack"></a>Azure yığında Git SDK profilleri kullanma

Azure yığında Git kod örneği çalıştırmak için:
  1. Git ve bağımlılıkları için Azure SDK'sını yükleyin. Yönerge için önceki bölüm bakın [Git için Azure SDK yüklemek](#install-azure-sdk-for-go).
  2. Meta veri bilgileri Resource Manager uç noktasından alır. Uç nokta Git kodunuzu çalıştırmak için gerekli bilgileri bir JSON dosyası döndürür.

  > [!Note]  
  > **ResourceManagerUrl** Azure yığın Geliştirme Seti (ASDK) olan: `https://management.local.azurestack.external/`  
  > **ResourceManagerUrl** tümleşik sistemlerindeki: `https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/`  
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

  3. Yoksa, bir abonelik oluşturun ve daha sonra kullanılmak üzere abonelik kimliği kaydedin. Bir abonelik oluşturma hakkında daha fazla bilgi için bkz: [Azure yığınında teklifleri için abonelikleri oluşturma](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm). 
  4. Bir hizmet sorumlusu "Abonelik" kapsamlı bir grup oluşturun ve **sahibi** rol. Hizmet sorumluları Kimliğini ve parolasını kaydedin. Azure yığını için bir hizmet sorumlusu oluşturma hakkında daha fazla bilgi için bkz: [hizmet sorumlusu oluşturma](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals#create-service-principal-for-azure-ad). Azure yığın ortamınızı ayarlayın.
  5. Kodunuzda Git SDK profilinden hizmet modülünü alın. Azure yığın profili geçerli sürümü **2017-03-09**. Örneğin, ağ modülünden içe aktarmak için **2017-03-09** profil türü: 

  ````go
    package main 
    import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network"
  ````
  
  6. İşlevinizi oluşturun ve sahip bir istemci kimlik doğrulaması bir **yeni** istemci işlevi çağrısı. Bir sanal ağ istemcisi oluşturmak için aşağıdaki kodu kullanabilirsiniz:  

  ````go
  package main 

  import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network" 

  func main() { 
      vnetClient := network.NewVirtualNetworksClientWithBaseURI("<baseURI>", "(subscriptionID>")
      vnetClient.Authorizer = autorest.NewBearerAuthorizer(token)
  ````

  Ayarlama `<baseURI>` için **ResourceManagerUrl** iki adımda kullanılan değer.
  Ayarlama `<subscriptionID>` için **Subscriptionıd** üç adımda kaydedilen değeri.
  Bkz: kimlik doğrulaması belirteci oluşturmak için aşağıdaki bölümü.  

  7. Önceki adımda oluşturduğunuz istemci kullanarak API yöntemlerini çağırın. Örneğin, önceki adımda bizim istemciden kullanarak bir sanal ağ oluşturmak için şunu yazın: 
  
````go
package main

import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network" 
func main() { 
  vnetC1ient := network.NewVirtualNetworksClientWithBaseURI("<baseURI>", "(subscriptionID>")
  vnetClient .Authorizer = autorest.NewBearerAuthorizer(token)

  vnetClient .CreateOrUpdate( ) 
````
  
  Git SDK profilini kullanarak Azure yığın üzerinde bir sanal ağ oluşturma tam örnek için bkz [örnek](#example).

## <a name="authentication"></a>Kimlik Doğrulaması

Azure SDK'sı Git kullanarak Active Directory Yetkilendiricisi özelliği almak için Git AutoRest modüllerini yükleyin. Bu modüller zaten "SDK Git" yüklemeye yüklenmiş olması gerekir; değilse, yükleme [github'da kimlik doğrulama paketi](https://github.com/Azure/go-autorest/tree/master/autorest/adal).

Yetkilendiricisi kaynak istemci Yetkilendiricisi olarak ayarlamanız gerekir. Bir Yetkilendiricisi almak için farklı yöntemler vardır; bir tam listesi için bkz. burada.

Bu bölümde, istemci kimlik bilgilerini kullanarak Azure yığında Yetkilendiricisi belirteçleri almak için ortak bir yol sunar:

  1. Bir hizmet sorumlusu abonelik sahibi rolü varsa, bu adımı atlayın. Aksi takdirde bir hizmet sorumlusu oluşturun [yönergeleri]( https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals) ve aboneliğiniz için kapsamlı bir "sahip" rolü atayın [yönergeleri]( https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals#assign-role-to-service-principal). Hizmet Asıl uygulama Kimliğini ve parolasını kaydedin. 

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
   
  Ayarlama `<activeDirectoryEndpoint>` "loginEndpoint" değerine ResourceManagerUrl meta veri özelliği, bu belgenin önceki bölümünde bulunan alınır.
  Ayarlama `<tenantID>` Azure yığın Kiracı kimliğinizi değerine 

  4. Son olarak, bir hizmet asıl belirteci adal modülden NewServicePrincipalToken yöntemini kullanarak oluşturun. 

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
  
  Ayarlama `<activeDirectoryResourceID>` ResourceManagerUrl meta verileri listesinden "hedef kitleyi" değerlerden biri için bu belgenin önceki bölümünde bulunan alınan.  
  Ayarlama `<clientID>` hizmet asıl uygulama kimliği kaydedilen hizmet sorumlusu, bu belgenin önceki bölümünde bulunan oluşturulduğu zaman.  
  Ayarlama `<clientSecret>` hizmet sorumlusu, bu belgenin önceki bölümünde bulunan oluşturulduğunda hizmet asıl uygulama gizli anahtarı kaydedildi.  

## <a name="example"></a>Örnek

Bu bölümde Azure yığında sanal ağ oluşturmak için Git kodu örneği gösterir. Git SDK tam örnekleri görmek için [Azure SDK'sını Git örnekleri depo](https://github.com/Azure-Samples/azure-sdk-for-go-samples). Karma altında Azure yığın örnekleri kullanılabilir / hizmet klasörleri depo içinde yolu.

> [!Note]  
> Bu örnekte kodu çalıştırmak için kullanılan abonelikle sahip olduğunu doğrulayın **ağ** kaynak sağlayıcısı olarak listelenen **kayıtlı**. Bunu doğrulamak için Azure yığın portal abonelikte arayın ve tıklayın **kaynak sağlayıcıları.**

1. Kodunuzda gerekli paketleri içeri aktarın. Ağ modülü içeri aktarmak için Azure yığında en son kullanılabilir profil kullanmanız gerekir. 
  
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

2. Ortam değişkenleri tanımlayın. Bir sanal ağ oluşturmak için bir kaynak grubu olması gerekir. 

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

3. Ortam değişkenlerini tanımladığınız, kullanarak kimlik doğrulama belirtecini oluşturmak için bir yöntem ekleyin **adal** paket. Önceki bölümde kimlik doğrulama hakkında ayrıntılar bakın.
  
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

4. Main yöntemi ekleyin. Main yöntemi önceki adımda tanımlanan yöntemi kullanarak bir belirteç ilk alır. Ardından, Profilinden ağ modülü kullanarak bir istemci oluşturur. Son olarak, bir sanal ağ oluşturur. 
  
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
* [Azure yığın kullanıcının PowerShell ortamını yapılandırma](azure-stack-powershell-configure-user.md)  
