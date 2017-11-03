---
title: "API Management Hızlı Başlangıç ile Azure Service Fabric | Microsoft Docs"
description: "Bu kılavuz hızlı bir şekilde Azure API Management ve Service Fabric nasıl başlayacağınızı gösterir."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/01/2017
ms.author: vturecek
ms.openlocfilehash: 2969834713fc7c2f1a2e281a6c988158d803dc45
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="service-fabric-with-azure-api-management-quick-start"></a>Service Fabric ile Azure API Management hızlı başlangıç

Bu kılavuz size Azure API Management Service Fabric ile ayarlama ve arka uç hizmetlerine Service Fabric trafiği göndermek için ilk API işlemi yapılandırma gösterir. Service Fabric ile Azure API Management senaryoları hakkında daha fazla bilgi için bkz: [genel bakış](service-fabric-api-management-overview.md) makalesi. 

## <a name="deploy-api-management-and-service-fabric-to-azure"></a>API Management ve Service Fabric Azure'a dağıtma

Paylaşılan bir sanal ağ ile Azure API Management ve Service Fabric kümesi dağıtmak için ilk adımdır bakın. Bu API, hizmet bulma, hizmet bölümü çözümleme ve iletme trafiği doğrudan herhangi bir arka uç hizmeti için Service Fabric gerçekleştirebilmek için Service Fabric ile doğrudan iletişim kurmak yönetim sağlar.

### <a name="topology"></a>Topoloji

Bu kılavuz aşağıdaki topoloji Azure API Management ve Service Fabric aynı sanal ağ alt ağlarında olduğu dağıtır:

 ![Resim yazısı][sf-apim-topology-overview]

Hızlıca başlamak için Resource Manager şablonları her dağıtım adımı için sağlanır:

 - Ağ topolojisi:
    - [Network.JSON][network-arm]
    - [Network.Parameters.JSON][network-parameters-arm]
 - Service Fabric kümesi:
    - [Cluster.JSON][cluster-arm]
    - [Cluster.Parameters.JSON][cluster-parameters-arm]
 - API Management:
    - [apim.JSON][apim-arm]
    - [apim.Parameters.JSON][apim-parameters-arm]

### <a name="sign-in-to-azure-and-select-your-subscription"></a>Azure'da oturum açın ve aboneliğinizi seçin

Bu kılavuzu kullanır [Azure PowerShell][azure-powershell]. Yeni bir PowerShell oturumu başlattığınızda, Azure hesabınızda oturum açın ve Azure komutları çalıştırmadan önce aboneliğinizi seçin.
 
Azure hesabınızda oturum açın:

```powershell
Login-AzureRmAccount
```

Aboneliğinizi seçin:

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Dağıtımınız için yeni bir kaynak grubu oluşturun. Bir ad ve bir konum girin.

```powershell
New-AzureRmResourceGroup -Name <my-resource-group> -Location westus
```

### <a name="deploy-the-network-topology"></a>Ağ topolojisi dağıtmak

İlk adım, API Management ve Service Fabric kümesi dağıtılacak ağ topolojisini ayarlamaktır. [Network.json] [ network-arm] Resource Manager şablonu iki alt ağ ve iki ağ güvenlik grupları (NSG) ile sanal ağ (VNET) oluşturmak için yapılandırılır. 

[Network.parameters.json] [ network-parameters-arm] parametreler dosyası alt ağları ve API Management ve Service Fabric dağıtılacağı Nsg'ler adlarını içerir. Bu kılavuz, parametre değerlerini değiştirilmesi gerekmez. Bu değerleri, bunlar burada değiştirilirse, onları diğer Resource Manager şablonları uygun şekilde değiştirmeniz gerekir, böylece API Management ve Service Fabric Resource Manager şablonları kullanın. 

 1. Aşağıdaki Resource Manager şablonu ve parametre dosyasını karşıdan yükleyin:

    - [Network.JSON][network-arm]
    - [Network.Parameters.JSON][network-parameters-arm]

 2. Ağ Kurulumu Resource Manager şablonu ve parametre dosyalarını dağıtmak için aşağıdaki PowerShell komutunu kullanın:

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\network.json -TemplateParameterFile .\network.parameters.json -Verbose
    ```

### <a name="deploy-the-service-fabric-cluster"></a>Service Fabric kümesi dağıtma

Ağ kaynaklarını dağıtma tamamladıktan sonra sonraki adım bir Service Fabric kümesi sanal ağ alt ağındaki ve Service Fabric kümesi için belirlenmiş NSG dağıtmaktır. Bu öğreticide, Service Fabric Resource Manager şablonu VNET, alt ağ ve önceki adımda ayarladığınız NSG adları kullanmak üzere önceden yapılandırılmıştır. 

Service Fabric kümesi Resource Manager şablonu, sertifika güvenliği ile güvenli bir küme oluşturmak için yapılandırılır. Sertifika, küme düğümü, düğümü iletişimin güvenliğini sağlamak için ve Service Fabric kümesi kullanıcı erişimini yönetmek için kullanılır. API Management hizmet bulma için Service Fabric adlandırma hizmetine erişim için bu sertifikayı kullanır.

Bu adım, bir sertifikanın anahtar kasası için küme güvenlik sonucunda gerektirir. Anahtar kasası ile güvenli bir küme ayarlama hakkında daha fazla bilgi için bkz: [bu kılavuz Azure Kaynak Yöneticisi'ni kullanarak bir küme oluşturma](service-fabric-cluster-creation-via-arm.md)

> [!NOTE]
> Azure Active Directory kimlik doğrulaması küme erişimi için kullanılan sertifikaya ek olarak ekleyebilirsiniz. Azure Active Directory Service Fabric kümesi kullanıcı erişimini yönetmek için önerilen yöntem, ancak bu öğreticiyi tamamlamak gerekli değildir. Bir sertifika her iki durumda da Azure Active Directory ile bir Service Fabric arka ucu için kimlik doğrulaması şu anda desteklemiyor Azure API Management kimlik doğrulama ve küme düğümü, düğümü güvenlik için gereklidir.

 1. Aşağıdaki Resource Manager şablonu ve parametre dosyasını karşıdan yükleyin:
 
    - [Cluster.JSON][cluster-arm]
    - [Cluster.Parameters.JSON][cluster-parameters-arm]

 2. Boş parametre doldurun `cluster.parameters.json` dosya, dağıtımınız için de dahil olmak üzere [anahtar kasası bilgileri](service-fabric-cluster-creation-via-arm.md#set-up-a-key-vault) küme sertifikanızın.

 3. Service Fabric kümesi oluşturmak için Resource Manager şablonu ve parametre dosyalarını dağıtmak için aşağıdaki PowerShell komutunu kullanın:

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\cluster.json -TemplateParameterFile .\cluster.parameters.json -Verbose
    ```

### <a name="deploy-api-management"></a>API Management dağıtma

Son olarak, alt ağdaki sanal ağa API Management dağıtın ve API yönetimi için NSG atanmış. API Management dağıtmadan önce tamamlanması Service Fabric Küme dağıtımı için beklemesi gerekmez. 

Bu öğreticide, API Management Resource Manager şablonu VNET, alt ağ ve önceki adımda ayarladığınız NSG adları kullanmak üzere önceden yapılandırılmıştır. 

 1. Aşağıdaki Resource Manager şablonu ve parametre dosyasını karşıdan yükleyin:
 
    - [apim.JSON][apim-arm]
    - [apim.Parameters.JSON][apim-parameters-arm]

 2. Boş parametre doldurun `apim.parameters.json` dağıtımınız için.

 3. API yönetimi için Resource Manager şablonu ve parametre dosyalarını dağıtmak için aşağıdaki PowerShell komutunu kullanın:

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\apim.json -TemplateParameterFile .\apim.parameters.json -Verbose
    ```

## <a name="configure-api-management"></a>API Management yapılandırın

API Management ve Service Fabric kümesi dağıtıldığı sonra Service Fabric arka uç API Management'te yapılandırabilirsiniz. Bu, Service Fabric kümesi trafiği gönderen bir arka uç hizmet ilkesi oluşturmanıza olanak sağlar.

### <a name="api-management-security"></a>API yönetim güvenliği

Service Fabric arka uç yapılandırmak için ilk API Management güvenlik ayarlarını yapılandırmanız gerekir. Güvenlik ayarlarını yapılandırmak üzere API Management hizmetiniz Azure portalında gidin.

#### <a name="enable-the-api-management-rest-api"></a>API Management REST API etkinleştir

API Management REST API şu anda bir arka uç hizmeti yapılandırmak için tek yoludur. API Management REST API etkinleştirmek ve onu güvenliğini sağlamak için ilk adımdır bakın.

 1. API Management hizmeti seçin **Yönetimi API - Önizleme** altında **güvenlik**.
 2. Denetleme **API Management REST API etkinleştir** onay kutusu.
 3. Yönetim API'si URL Not - Bu daha sonra Service Fabric arka ucu ayarlamak için kullanacağız URL'si
 4. Generate bir **belirtecine erişmesini** bir sona erme tarihi ve bir anahtar seçerek, ardından **Generate** sayfasının en altına doğru düğmesi.
 5. Kopya **erişim belirteci** ve onu Kaydet - Bu aşağıdaki adımlarda kullanacağız. Bu birincil anahtar ve ikincil anahtar farklı olduğuna dikkat edin.

#### <a name="upload-a-service-fabric-client-certificate"></a>Service Fabric istemci sertifikasını karşıya yükle

API Management Service Fabric kümeniz kümenizi erişimi olan bir istemci sertifikası kullanılarak hizmet bulma için kimlik doğrulaması gerekir. Kolaylık olması için Bu öğretici, kümenizi erişmek için kullanılan varsayılan Service Fabric kümesi oluşturulurken belirtilen aynı sertifikayı kullanır.

 1. API Management hizmeti seçin **istemci sertifikalarını - Önizleme** altında **güvenlik**.
 2. Tıklatın **+ Ekle** düğmesi
 2. Service Fabric kümesi oluştururken, belirtilen küme sertifikanın özel anahtar dosyası (.pfx) seçin, bir ad verin ve özel anahtar parolası sağlayın.

> [!NOTE]
> Bu öğretici, istemci kimlik doğrulaması ve küme düğümü, düğümü güvenlik için aynı sertifikayı kullanır. Bir Service Fabric kümesi erişmek için yapılandırılan yoksa ayrı bir istemci sertifikası kullanabilir.

### <a name="configure-the-backend"></a>Arka uç yapılandırın

API Management güvenlik yapılandırıldığında, Service Fabric arka uç yapılandırabilirsiniz. Service Fabric arka uçlarını için Service Fabric kümesi, belirli bir Service Fabric hizmeti yerine arka uç grubudur. Bu, kümedeki birden fazla hizmeti yönlendirmek tek bir ilke sağlar.

Bu adım, önceki adımda API Management karşıya küme sertifikanızın daha önce oluşturulan erişim belirteci ve parmak izi gerektirir.

> [!NOTE]
> API yönetimi için önceki adımda ayrı istemci sertifikası kullandıysanız, bu adımda küme sertifika parmak izi yanı sıra istemci sertifikasının parmak izi gerekir.

Aşağıdaki HTTP PUT, Service Fabric arka uç hizmetini yapılandırmak API Management REST API etkinleştirirken daha önce Not API Management API URL'sine isteği gönderin. Görmeniz gerekir bir `HTTP 201 Created` komutu başarılı olduğunda yanıt. Her bir alan hakkında daha fazla bilgi için bkz: API Yönetimi [arka uç API başvuru belgeleri](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend).

HTTP komutu ve URL:
```http
PUT https://your-apim.management.azure-api.net/backends/servicefabric?api-version=2016-10-10
```

İstek üstbilgilerini:
```http
Authorization: SharedAccessSignature <your access token>
Content-Type: application/json
```

İstek gövdesi:
```http
{
    "description": "<description>",
    "url": "<fallback service name>",
    "protocol": "http",
    "resourceId": "<cluster HTTP management endpoint>",
    "properties": {
        "serviceFabricCluster": {
            "managementEndpoints": [ "<cluster HTTP management endpoint>" ],
            "clientCertificateThumbprint": "<client cert thumbprint>",
            "serverCertificateThumbprints": [ "<cluster cert thumbprint>" ],
            "maxPartitionResolutionRetries" : 5
        }
    }
}
```

**Url** burada parametredir bir hizmet tam adı bir hizmetin kümenizdeki bir arka uç ilkesinde hiçbir hizmet adı belirtilmezse, tüm istekleri varsayılan olarak yönlendirilir. Sahte hizmet adı gibi kullanabilir "fabric: / sahte/service" bir geri dönüş hizmetine sahip düşünmüyorsanız. Unutmayın, **url** biçiminde olmalıdır "fabric: / uygulama/hizmet" sahte bir geri dönüş hizmet olsa bile.

API Management başvuran [arka uç API başvuru belgeleri](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend) her bir alan hakkında daha fazla ayrıntı için.

#### <a name="example"></a>Örnek

```http
PUT https://your-apim.management.azure-api.net/backends/servicefabric?api-version=2016-10-10
Authorization: SharedAccessSignature 230948023984&Ld93cRGcNU6KZ4uVz7JlfTec4eX43Q9Nu8ndatOgBzs6+f559Pkf3iHX2cSge+r42pn35qGY3TitjrIl13hwcQ==
Content-Type: application/json

{
    "description": "My Service Fabric backend",
    "url": "fabric:/myapp/myservice",
    "protocol": "http",
    "resourceId": "https://your-cluster.westus.cloudapp.azure.com:19080",
    "properties": {
        "serviceFabricCluster": {
            "managementEndpoints": ["https://your-cluster.westus.cloudapp.azure.com:19080"],
            "clientCertificateThumbprint": "57bc463aba3aea3a12a18f36f44154f819f0fe32",
            "serverCertificateThumbprints": ["57bc463aba3aea3a12a18f36f44154f819f0fe32"],
            "maxPartitionResolutionRetries" : 5
        }
    }
}
```

## <a name="deploy-a-service-fabric-back-end-service"></a>Service Fabric arka uç hizmet dağıtma

API Management için bir arka uç olarak yapılandırılan Service Fabric sahip olduğunuza göre Apı'lerinizi Service Fabric hizmetleriniz için trafiği göndermek için arka uç ilkeleri yazabilirsiniz. Ancak önce isteklerini kabul etmek için Service Fabric içinde çalışan bir hizmete gerekir.

### <a name="create-a-service-fabric-service-with-an-http-endpoint"></a>Bir HTTP uç noktası ile bir Service Fabric hizmet oluşturma

Bu öğreticide, temel bir durum bilgisiz ASP.NET Core güvenilir varsayılan Web API projesi şablonunu kullanarak hizmet oluşturacağız. Azure API Management kullanıma hizmetiniz için bir HTTP uç noktası oluşturur:

```
/api/values
```

Tarafından Başlat [ASP.NET Core geliştirme için geliştirme ortamınızı kurma](service-fabric-add-a-web-frontend.md#set-up-your-environment-for-aspnet-core).

Geliştirme ortamınızı ayarladıktan sonra Visual Studio'yu yönetici olarak başlatın ve ASP.NET Core hizmet oluşturun:

 1. Visual Studio'da yeni proje dosyası seçin ->.
 2. Bulut altında Service Fabric uygulaması şablonu seçin ve adlandırın **"ApiApplication"**.
 3. ASP.NET Core hizmet şablonu seçin ve projeyi adlandırın **"WebApiService"**.
 4. Web API ASP.NET Core 1.1 proje şablonu seçin.
 5. Proje oluşturulduktan sonra açmak `PackageRoot\ServiceManifest.xml` kaldırıp `Port` uç nokta kaynak yapılandırması özniteliğinden:
 
    ```xml
    <Resources>
      <Endpoints>
        <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" />
      </Endpoints>
    </Resources>
    ```

    Bu, bir bağlantı noktasından, biz ağ güvenlik grubu için API Management trafiğe izin verme küme Resource Manager şablonunda aracılığıyla açılan dinamik olarak uygulama bağlantı noktası aralığını belirtmek Service Fabric sağlar.
 
 6. Web API doğrulamak için Visual Studio'da F5 tuşuna basarak yerel olarak kullanılabilir. 

    Açık Service Fabric Explorer ve ASP.NET Core hizmeti belirli bir örneği inceleyin taban adresi görmek için hizmet üzerinde dinliyor. Ekleme `/api/values` bankasına adresi ve bir tarayıcıda açın. Bu Web API şablonunda ValuesController Get yöntemini çağırır. Bu şablon tarafından sağlanan varsayılan yanıt, iki dizeyi içeren bir JSON dizisi döndürür:

    ```json
    ["value1", "value2"]`
    ```

    Bu, Azure API Management'te aracılığıyla kullanıma uç noktadır.

 7. Son olarak, Azure kümenizdeki uygulamayı dağıtın. [Visual Studio kullanarak](service-fabric-publish-app-remote-cluster.md#to-publish-an-application-using-the-publish-service-fabric-application-dialog-box), uygulama projesine sağ tıklatın ve **Yayımla**. Küme uç noktası sağlayın (örneğin, `mycluster.westus.cloudapp.azure.com:19000`) Azure Service Fabric kümenizdeki uygulamayı dağıtmak için.

Adlı bir ASP.NET Core durum bilgisiz hizmeti `fabric:/ApiApplication/WebApiService` artık Azure Service Fabric kümenizdeki çalıştırması gerekir.

## <a name="create-an-api-operation"></a>Bir API işlem oluşturma

Şimdi biz, dış istemcilere Service Fabric kümede çalışan ASP.NET Core durum bilgisiz hizmeti ile iletişim kurmak için API Management bir işlem oluşturmak için hazır.

 1. Azure portalında oturum açın ve API Management hizmeti dağıtımınızı gidin.
 2. API Management hizmeti dikey penceresinde, seçin **API'leri - Önizleme**
 3. Tıklayarak yeni bir API eklemek **boş API** kutusu ve iletişim kutusunu doldurma:

     - **Web hizmeti URL'si**: Service Fabric arka uçlarını için değil Bu URL değeri kullanılır. Herhangi bir değer buraya koyabilirsiniz. Bu öğretici için kullanın: `http://servicefabric`.
     - **Ad**: API'nizi için herhangi bir ad sağlayın. Bu öğretici için kullanmak `Service Fabric App`.
     - **URL şeması**: HTTP, HTTPS ya da her ikisini de seçin. Bu öğretici için kullanmak `both`.
     - **API'si URL soneki**: bir sonek için bizim API sağlar. Bu öğretici için kullanmak `myapp`.
 
 4. API oluşturulduktan sonra tıklatın **+ ekleme işlemi** bir ön uç API işlemi eklemek için. Değerlerin doldurun:
    
     - **URL**: seçin `GET` ve bir URL yolu için API sağlar. Bu öğretici için kullanmak `/api/values`.
     
       Varsayılan olarak, Service Fabric hizmeti arka ucuna gönderilen URL yolunu URL yolu burada belirtilen. Hizmetinizi kullanır, bu durumda aynı URL yolunu buraya kullanırsanız `/api/values`, sonra işlemi daha fazla değişiklik yapılmadan çalışır. Ayrıca, arka uç Service Fabric hizmeti tarafından kullanılan URL yolu farklı bir URL yolu buraya belirtebilir, bu durumda, aynı zamanda bir yolu yeniden yazma işlemi ilkenizde daha sonra belirtmeniz gerekir.
     - **Görünen ad**: API için herhangi bir ad sağlayın. Bu öğretici için kullanmak `Values`.

## <a name="configure-a-backend-policy"></a>Arka uç ilkesini yapılandırma

Arka uç ilke her şeyi birbirine bağlar. İstekleri yönlendirilen arka uç Service Fabric hizmeti yapılandırdığınız budur. Bu ilke için herhangi bir API işlemi uygulayabilirsiniz. [Service Fabric için arka uç yapılandırması](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService) aşağıdaki isteği yönlendirme denetimleri sağlar: 
 - Hizmet örneği seçimi ya da sabit kodlanmış bir Service Fabric hizmeti örnek adı belirterek (örneğin, `"fabric:/myapp/myservice"`) veya HTTP isteğinden oluşturulan (örneğin, `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`).
 - Tüm Service Fabric bölümleme düzenini kullanarak bir bölüm anahtarı oluşturma tarafından bölüm çözünürlüğü.
 - Durum bilgisi olan hizmetler için çoğaltma seçimi.
 - Bir hizmet konumu yeniden çözümlemeyi ve isteği yeniden göndermeyi koşullarını belirtmenize olanak veren çözümleme yeniden deneme koşulları.

Bu öğretici için daha önce dağıttığınız ASP.NET Core durum bilgisiz hizmete isteklerini doğrudan yönlendiren bir arka uç ilkesi oluşturun:

 1. Seçme ve düzenleme **gelen ilkeleri** için `Values` Düzenle simgesine tıklayarak ve ardından seçerek işlemi **kod görünümü**.
 2. İlke Kod düzenleyicisinde ekleyin bir `set-backend-service` ilkesi altında aşağıda gösterildiği gibi ilkeleri gelen ve tıklatın **kaydetmek** düğmesi:
    
    ```xml
    <policies>
      <inbound>
        <base/>
        <set-backend-service 
           backend-id="servicefabric"
           sf-service-instance-name="fabric:/ApiApplication/WebApiService"
           sf-resolve-condition="@((int)context.Response.StatusCode != 200)" />
      </inbound>
      <backend>
        <base/>
      </backend>
      <outbound>
        <base/>
      </outbound>
    </policies>
    ```

Service Fabric arka uç İlkesi özniteliklerini tam kümesi için başvurmak [API Management arka uç belgeleri](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService)

### <a name="add-the-api-to-a-product"></a>Bir ürüne API ekleme. 

API aramadan önce burada kullanıcılara erişim izni verebilirsiniz ürün eklenmesi gerekir. 

 1. API Management hizmeti seçin **ürünler - Önizleme**.
 2. Varsayılan olarak, API Management sağlayıcıları iki ürünler: Starter ve sınırsız. Sınırsız ürün seçin.
 3. API seçin.
 4. Tıklatın **+ Ekle** düğmesi.
 5. Seçin `Service Fabric App` tıklayın ve önceki adımları içinde oluşturduğunuz API **seçin** düğmesi.

### <a name="test-it"></a>test

Şimdi doğrudan Azure Portalı'ndan API Management üzerinden Service Fabric, arka uç hizmetinde bir istek göndermesini deneyin.

 1. API Management hizmeti seçin **API - Önizleme**.
 2. İçinde `Service Fabric App` önceki adımlarda, select oluşturduğunuz API **Test** sekmesi.
 3. Tıklatın **Gönder** düğmesi bir test isteği arka uç hizmetine göndermek için.

## <a name="next-steps"></a>Sonraki adımlar

Bu noktada, Service Fabric ve API Management temel bir kurulumu olması gerekir.

Bu öğretici temel kullanıcı sertifika tabanlı kimlik doğrulaması, Service Fabric kümesi için hızlı bir şekilde ayarlanan almak için kullanır. Service Fabric kümesi için daha gelişmiş kullanıcı kimlik doğrulaması ile tercih [Azure Active Directory kimlik doğrulaması](service-fabric-cluster-creation-via-arm.md#set-up-azure-active-directory-for-client-authentication). 

Ardından, [oluşturma ve Gelişmiş Ürün ayarları Azure API Management'te yapılandırma](https://docs.microsoft.com/azure/api-management/api-management-howto-product-with-rules) uygulamanızı gerçek dünya trafiği için hazırlamak için.

<!-- links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/

[network-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.json
[network-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.parameters.json

[apim-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.json
[apim-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.parameters.json

[cluster-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.json
[cluster-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.parameters.json


<!-- pics -->
[sf-apim-topology-overview]: ./media/service-fabric-api-management-quickstart/sf-apim-topology-overview.png
