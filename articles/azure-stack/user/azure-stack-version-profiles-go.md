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
ms.date: 01/19/2019
ms.author: sethm
ms.reviewer: sijuman
ms.openlocfilehash: f865d08f742ebd1072b80a95960609e6ae5f4a82
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2019
ms.locfileid: "55098417"
---
# <a name="use-api-version-profiles-with-go-in-azure-stack"></a>Azure stack'teki Git ile API Sürüm profillerini kullanma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

## <a name="go-and-version-profiles"></a>Go ve sürüm profilleri

Bir profili farklı sürümlerini farklı hizmetlerden farklı kaynak türlerinin birleşimidir. Bir profil kullanarak, karıştırın ve eşleştirin farklı kaynak türleri arasında yardımcı olur. Profilleri, aşağıdaki faydaları sağlayabilir:

- Kararlılık, uygulamanızın belirli API sürümleri için kilitleme tarafından.
- Azure Stack ve bölgesel Azure veri merkezleri ile uyumluluğu, uygulamanız için.

Go SDK sürümü profil yolu altında kullanılabilir profilleri **YYYY-AA-GG** biçimi. Şu anda, en son Azure Stack API profili sürümü **2017-03-09**. Belirli bir hizmete bir profilden içeri aktarmak için karşılık gelen alt modülü profilinden içeri aktarın. Örneğin, içeri aktarmak için **işlem** hizmetinde **2017-03-09** profil, aşağıdaki kodu kullanın:

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/compute/mgmt/compute"
```

## <a name="install-azure-sdk-for-go"></a>Go için Azure SDK'sını yükleme

1. Git'i yükleyin. Yönergeler için [Git'i yükledikten Başlarken -](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
2. Yükleme [Go programlama dili](https://golang.org/dl). Azure için API profilleri, Go 1.9 veya daha yeni bir sürümü gerektirir.
3. Aşağıdaki bash komutunu çalıştırarak Azure Go SDK'sını ve bağımlılıklarını yükleyin:

   ```bash
   go get -u -d github.com/Azure/azure-sdk-for-go/...
   ```

### <a name="the-go-sdk"></a>Go SDK'sı

Aşağıdaki bağlantılarda Azure GO SDK'sı hakkında daha fazla bilgi bulabilirsiniz:

- Azure SDK, Go [Go için Azure SDK'sını yükleme](/azure/azure-sdk-go-install).
- Azure Go SDK'sı github'da herkes tarafından kullanılabilir [go için azure SDK'sı](https://github.com/Azure/azure-sdk-for-go) depo.

### <a name="go-autorest-dependencies"></a>Git AutoRest bağımlılıkları

Go SDK'sı Azure bağlıdır **Git AutoRest** REST istekleri göndermek için Azure Resource Manager uç noktaları için modüller. Azure içeri aktarma **Git AutoRest** modülü bağımlılıklardan [Azure Git-AutoRest github'da](https://github.com/Azure/go-autorest). Yükleme bash komutları bulabilirsiniz **yükleme** bölümü.

## <a name="how-to-use-go-sdk-profiles-on-azure-stack"></a>Azure Stack'te go SDK profilleri kullanma

Azure Stack'te Go kod örneği çalıştırmak için aşağıdaki adımları izleyin:

1. Azure SDK, Go ve onun bağımlılıklarını yükleyin. Yönergeler için önceki bölüme bakın [Go için Azure SDK yükleme](#install-azure-sdk-for-go).
2. Resource Manager uç noktasından meta veri bilgilerini alın. Uç nokta Git kodunuzu çalıştırmak için gerekli bilgileri bir JSON dosyası döndürür.

   > [!NOTE]  
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

3. Yoksa, bir abonelik oluşturur ve daha sonra kullanılmak üzere abonelik Kimliğini kaydedin. Abonelik oluşturma hakkında daha fazla bilgi için bkz: [Azure Stack'te teklifleri abonelikleri oluşturma](../azure-stack-subscribe-plan-provision-vm.md).

4. Bir hizmet sorumlusu oluşturma **abonelik** kapsamı ve **sahibi** rol. Hizmet sorumlusu kimliği ve parolasını kaydedin. Azure Stack için hizmet sorumlusu oluşturma hakkında daha fazla bilgi için bkz: [hizmet sorumlusu oluşturma](azure-stack-create-service-principals.md#create-service-principal-for-azure-ad). Azure Stack ortamınıza şimdi ayarlayın.

5. Bir hizmeti modülü kodunuzda Go SDK profilinden içeri aktarın. Azure Stack profilinin geçerli sürümü **2017-03-09**. Örneğin, ağ modülünü içeri aktarmak için **2017-03-09** profil türü, aşağıdaki kodu kullanın:

   ```go
   package main
    import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network"
   ```

6. İşlevinizi oluşturun ve bir istemci ile kimlik doğrulaması bir **yeni** istemci işlev çağrısı. Bir sanal ağ istemcisi oluşturmak için aşağıdaki kodu kullanabilirsiniz:  

   ```go
   package main

   import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network"

   func main() {
      vnetClient := network.NewVirtualNetworksClientWithBaseURI("<baseURI>", "(subscriptionID>")
      vnetClient.Authorizer = autorest.NewBearerAuthorizer(token)
   ```

   Ayarlama `<baseURI>` için **ResourceManagerUrl** 2. adımda kullanılan değer. Ayarlama `<subscriptionID>` için **Subscriptionıd** 3 adımda kaydedilen değeri.

   Belirteci oluşturmak için aşağıdaki bölüme bakın.  

7. Önceki adımda oluşturduğunuz istemcisini kullanarak API yöntemleri çağırır. Örneğin, önceki adımdan istemcisini kullanarak bir sanal ağ oluşturmak için aşağıdaki örneğe bakın:

   ```go
   package main

   import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network"
   func main() {
   vnetClient := network.NewVirtualNetworksClientWithBaseURI("<baseURI>", "(subscriptionID>")
   vnetClient .Authorizer = autorest.NewBearerAuthorizer(token)

   vnetClient .CreateOrUpdate( )
   ```

Go SDK profilini kullanarak Azure Stack üzerinde bir sanal ağ oluşturma tam bir örnek için bkz [örnek](#example).

## <a name="authentication"></a>Kimlik Doğrulaması

Alınacak **yetkilendirici** Go SDK kullanarak Azure Active Directory özelliğini yükleyin **Git AutoRest** modüller. Bu modüller zaten "Go SDK" yüklemeye yüklenmiş olması gerekir; Aksi takdirde, yükleme [GitHub kimlik doğrulaması paketinden](https://github.com/Azure/go-autorest/tree/master/autorest/adal).

Yetkilendirici yetkilendirici kaynağı istemcisi olarak ayarlanmalıdır. İstemci kimlik bilgilerini kullanarak Azure Stack'te yetkilendirici belirteçlerini almak için farklı yolu vardır:

1. Abonelikte sahip rolü hizmet sorumlusuyla varsa, bu adımı atlayın. Aksi takdirde oluşturma bir [hizmet sorumlusu](azure-stack-create-service-principals.md) ve "sahip" rol atayın [aboneliğinize kapsamlı](azure-stack-create-service-principals.md#assign-the-service-principal-to-a-role). Hizmet sorumlusunun uygulama Kimliğini ve parolasını kaydedin.

2. İçeri aktarma **adal** Git AutoRest paketinden kodunuzda.

   ```go
   package main
   import "github.com/Azure/go-autorest/autorest/adal"
   ```

3. Oluşturma bir **oauthConfig** NewOAuthConfig yöntemi kullanarak **adal** modülü.

   ```go
   package main

   import "github.com/Azure/go-autorest/autorest/ada1"

   func CreateToken() (adal.OAuthTokenProvider, error) {
      var token adal.OAuthTokenProvider
      oauthConfig, err := adal.NewOAuthConfig(activeDirectoryEndpoint, tenantID)
   }
   ```

   Ayarlama `<activeDirectoryEndpoint>` değerine `loginEndpoint` özelliğinden `ResourceManagerUrl` meta verileri bu belgenin önceki bölümde alınır. Ayarlama `<tenantID>` Azure Stack Kiracı kimliğiniz için değeri

4. Son olarak, bir hizmet sorumlusu belirteci kullanarak oluşturma `NewServicePrincipalToken` yönteminden **adal** Modülü:

   ```go
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
   ```

    Ayarlama `<activeDirectoryResourceID>` "audience" listesindeki değerlerden birine **ResourceManagerUrl** meta verileri alındı, bu makalenin önceki bölümde.
    Ayarlama `<clientID>` hizmet sorumlusunun bu makalenin önceki bölümde oluştururken hizmet sorumlusu uygulamasına kayıtlı kimliği.
    Ayarlama `<clientSecret>` için bu makalenin önceki bölümde hizmet sorumlusu oluşturulurken kaydedilen hizmet sorumlusu uygulama gizli anahtarı.

## <a name="example"></a>Örnek

Bu örnekte, Azure Stack üzerinde bir sanal ağ oluşturur, Go kod örneği gösterilmektedir. Go SDK'sı tam örnekler için bkz [Azure Go SDK örnekleri depomuzdan](https://github.com/Azure-Samples/azure-sdk-for-go-samples). Azure Stack örnekleri hizmet klasör deponun içinde karma yolun altında yer alır.

> [!NOTE]  
> Bu örnekte, kodu çalıştırmak için kullanılan abonelik sahip olduğunu doğrulayın **ağ** kaynak sağlayıcısı olarak listelenen **kayıtlı**. Doğrulayın, Azure Stack portalında abonelik arayın ve seçin için **kaynak sağlayıcıları.**

1. Kodunuzda gerekli paketleri içeri aktarın. En son kullanılabilir profil Azure Stack üzerinde ağ modülü içeri aktarmak için kullanın:

   ```go
   package main

   import (
       "context"
       "fmt"
       "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network"
       "github.com/Azure/go-autorest/autorest"
       "github.com/Azure/go-autorest/autorest/adal"
       "github.com/Azure/go-autorest/autorest/to"
   )
   ```

2. Ortam değişkenlerini tanımlayın. Sanal ağ oluşturmak için bir kaynak grubu olması gerekir.

   ```go
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
   ```

3. Ortam değişkenlerini tanımladığınıza göre kullanarak bir kimlik doğrulama belirteci oluşturmak için bir yöntem ekleyin **adal** paket. Kimlik doğrulaması hakkında daha fazla bilgi için önceki bölüme bakın.

   ```go
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
   ```

4. Ekleme `main` yöntemi. `main` Yöntemi önce önceki adımda tanımlanan yöntemini kullanarak bir belirteç alır. Ardından, istemci profilinden ağ modül kullanarak oluşturulur. Son olarak, bir sanal ağ oluşturur.

   ```go
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
   ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack için PowerShell yükleme](azure-stack-powershell-install.md)
- [Azure Stack kullanıcının PowerShell ortamını yapılandırma](azure-stack-powershell-configure-user.md)  
