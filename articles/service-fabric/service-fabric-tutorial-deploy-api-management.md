---
title: "Azure Service Fabric API Management ile tümleştirme | Microsoft Docs"
description: "Hızlı bir şekilde Azure API Management ve Service Fabric nasıl başlayacağınızı öğrenin."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/10/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 722a3f0f428bb972b2835df65a67707bf4d8e7d7
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
---
# <a name="deploy-api-management-with-service-fabric"></a>API Management Service Fabric ile dağıtma
Bu öğretici dört serinin bir parçasıdır.  Azure API Management Service Fabric ile dağıtma Gelişmiş bir senaryodur.  API Management, arka uç Service Fabric hizmetleriniz için yönlendirme kurallarını zengin bir dizi API yayımlamak gereken yararlıdır. Bulut uygulamalarını genellikle kullanıcılar, aygıtlar veya diğer uygulamalar için tek bir giriş noktası sağlamak için bir ön uç ağ geçidi gerekir. Service Fabric bir ağ geçidi APP.NET çekirdek uygulama, olay hub'ları, IOT hub'ı veya Azure API Management gibi trafik giriş için tasarlanmış herhangi bir durum bilgisiz hizmeti olabilir. 

Bu öğretici nasıl ayarlanacağını gösterir [Azure API Management](../api-management/api-management-key-concepts.md) Service Fabric arka uç hizmetinde trafiğini yönlendirmek için Service Fabric ile.  İşlemi tamamladığınızda, bir sanal AĞA API Management dağıtılmış vardır, bir API işlemi arka uç durum bilgisi olmayan hizmetler için trafiği göndermek için yapılandırılmış. Service Fabric ile Azure API Management senaryoları hakkında daha fazla bilgi için bkz: [genel bakış](service-fabric-api-management-overview.md) makalesi.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * API Management dağıtma
> * API Management yapılandırın
> * Bir API işlem oluşturma
> * Arka uç ilkesini yapılandırma
> * Bir ürüne API ekleme

Bu öğretici serisinde öğrenin nasıl yapılır:
> [!div class="checklist"]
> * Güvenli oluşturma [Windows Küme](service-fabric-tutorial-create-vnet-and-windows-cluster.md) veya [Linux kümesi](service-fabric-tutorial-create-vnet-and-linux-cluster.md) bir şablonu kullanarak azure'da
> * [Bir küme veya ölçeklendirme](/service-fabric-tutorial-scale-cluster.md)
> * [Yükseltme küme çalışma zamanı](service-fabric-tutorial-upgrade-cluster.md)
> * API Management Service Fabric ile dağıtma

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce:
- Bir Azure aboneliğiniz yoksa, oluşturma bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Yükleme [Azure Powershell modülü sürümü 4.1 veya üzerini](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) veya [Azure CLI 2.0](/cli/azure/install-azure-cli).
- Güvenli oluşturma [Windows Küme](service-fabric-tutorial-create-vnet-and-windows-cluster.md) veya [Linux kümesi](service-fabric-tutorial-create-vnet-and-linux-cluster.md) Azure ile ilgili
- Windows Küme dağıtırsanız, bir Windows geliştirme ortamını ayarlama. Yükleme [Visual Studio 2017](http://www.visualstudio.com) ve **Azure geliştirme**, **ASP.NET ve web geliştirme**, ve **.NET Core platformlar arası geliştirme**iş yükleri.  Daha sonra ayarlanmış bir [.NET geliştirme ortamı](service-fabric-get-started.md).
- Linux kümesi dağıtırsanız, üzerinde bir Java geliştirme ortamını ayarlama [Linux](service-fabric-get-started-linux.md) veya [MacOS](service-fabric-get-started-mac.md).  Yükleme [doku CLI hizmet](service-fabric-cli.md). 

## <a name="network-topology"></a>Ağ topolojisi
Güvenli sahip olduğunuza [Windows Küme](service-fabric-tutorial-create-vnet-and-windows-cluster.md) veya [Linux kümesi](service-fabric-tutorial-create-vnet-and-linux-cluster.md) Azure üzerinde API Management API yönetimi için tasarlanmış NSG ve alt ağ sanal bir ağa (VNET) dağıtın. Bu öğreticide, API Management Resource Manager şablonu VNET, alt ağ ve önceki ayarladığınız NSG adları kullanmak üzere önceden yapılandırılmıştır [Windows Küme öğretici](service-fabric-tutorial-create-vnet-and-windows-cluster.md) veya [Linux küme öğretici](service-fabric-tutorial-create-vnet-and-linux-cluster.md). Bu öğretici Azure API Management ve Service Fabric aynı sanal ağ alt ağlarında olan aşağıdaki topoloji dağıtır:

 ![Resim yazısı][sf-apim-topology-overview]

## <a name="sign-in-to-azure-and-select-your-subscription"></a>Azure'da oturum açın ve aboneliğinizi seçin
Azure komutları çalıştırmadan önce Azure hesabınızda oturum açın, aboneliğinizi seçin.

```powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

```azurecli
az login
az account set --subscription <guid>
```

## <a name="deploy-a-service-fabric-back-end-service"></a>Service Fabric arka uç hizmet dağıtma

API Management'ın bir Service Fabric arka uç hizmetine trafiği yönlendirmek için yapılandırmadan önce ilk isteklerini kabul etmek için çalışan bir hizmete gereksinim duyarsınız.  Daha önce oluşturduğunuz varsa bir [Windows Küme](service-fabric-tutorial-create-vnet-and-windows-cluster.md), .NET Service Fabric hizmeti dağıtın.  Daha önce oluşturduğunuz varsa bir [Linux kümesi](service-fabric-tutorial-create-vnet-and-linux-cluster.md), Java Service Fabric hizmeti dağıtın.

### <a name="deploy-a-net-service-fabric-service"></a>.NET Service Fabric hizmet dağıtma

Bu öğreticide, temel bir durum bilgisiz ASP.NET Core güvenilir varsayılan Web API projesi şablonunu kullanarak hizmet oluşturun. Bu, Azure API Management kullanıma hizmetiniz için bir HTTP uç noktası oluşturur.

Visual Studio'yu yönetici olarak başlatın ve ASP.NET Core hizmet oluşturun:

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

    Bağlantı noktası kaldırma ağ güvenlik grubu için API Management trafiğe izin verme küme Resource Manager şablonunda aracılığıyla açılan dinamik olarak uygulama bağlantı noktası aralığından, bağlantı noktasını belirlemek Service Fabric sağlar.
 
 6. Web API doğrulamak için Visual Studio'da F5 tuşuna basarak yerel olarak kullanılabilir. 

    Açık Service Fabric Explorer ve ASP.NET Core hizmeti belirli bir örneği inceleyin taban adresi görmek için hizmet üzerinde dinliyor. Ekleme `/api/values` bankasına adres ve Web API şablonunda ValuesController Get yöntemini çağıran bir tarayıcıda açın. Bu şablon tarafından sağlanan varsayılan yanıt, iki dizeyi içeren bir JSON dizisi döndürür:

    ```json
    ["value1", "value2"]`
    ```

    Bu, Azure API Management'te aracılığıyla kullanıma uç noktadır.

 7. Son olarak, Azure kümenizdeki uygulamayı dağıtın. Visual Studio'da Uygulama projesine sağ tıklatın ve **Yayımla**. Küme uç noktası sağlayın (örneğin, `mycluster.southcentralus.cloudapp.azure.com:19000`) Azure Service Fabric kümenizdeki uygulamayı dağıtmak için.

Adlı bir ASP.NET Core durum bilgisiz hizmeti `fabric:/ApiApplication/WebApiService` artık Azure Service Fabric kümenizdeki çalıştırması gerekir.

### <a name="create-a-java-service-fabric-service"></a>Java Service Fabric hizmeti oluşturma
Bu öğretici dağıtmak için görüntülemektedir bir temel web sunucusu iletileri kullanıcıya yedekleyin. Echo sunucu örnek uygulaması, Azure API Management kullanıma hizmetiniz için bir HTTP uç noktası içerir.

1. Java Başlarken kopya örnekleri başlatıldı.

   ```bash
   git clone https://github.com/Azure-Samples/service-fabric-java-getting-started.git
   cd service-fabric-java-getting-started
   ```

2. Düzen *Services/EchoServer/EchoServer1.0/EchoServerApplication/EchoServerPkg/ServiceManifest.xml*. Hizmet bağlantı noktasını 8081 dinler şekilde uç noktasını güncelleyin.

   ```xml
   <Endpoint Name="WebEndpoint" Protocol="http" Port="8081" />
   ```

3. Kaydet *ServiceManifest.xml*, EchoServer1.0 uygulamayı oluşturun.

   ```bash
   cd Services/EchoServer/EchoServer1.0/
   gradle
   ```

4. Küme uygulamayı dağıtın.

   ```bash
   cd Scripts
   sfctl cluster select --endpoint http://mycluster.southcentralus.cloudapp.azure.com:19080
   ./install.sh
   ```

   Adlı bir Java durum bilgisiz hizmette `fabric:/EchoServerApplication/EchoServerService` artık Azure Service Fabric kümenizdeki çalıştırması gerekir.

5. Bir tarayıcı açın ve şunu yazın http://mycluster.southcentralus.cloudapp.azure.com:8081/getMessage, görmeniz gerekir "[sürüm 1.0] Hello World!!!" görüntülenir.

## <a name="download-and-understand-the-resource-manager-template"></a>İndirme ve Resource Manager şablonu anlama
Aşağıdaki Resource Manager şablonu ve parametre dosyasını indirip kaydedin:
 
- [apim.JSON][apim-arm]
- [apim.Parameters.JSON][apim-parameters-arm]

Aşağıdaki bölümlerde tarafından tanımlanan kaynakları *apim.json* şablonu. Daha fazla bilgi için her bölüm içerisindeki şablon başvurusu belgelere bağlantıları izleyin. Tanımlanan yapılandırılabilir parametreler *apim.parameters.json* parametreler dosyası, bu makalenin sonraki bölümlerinde ayarlanır.

### <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service
[Microsoft.ApiManagement/service](/azure/templates/microsoft.apimanagement/service) API Management hizmet örneği açıklar: ad, SKU veya katmanı, kaynak grubu konumu, yayımcı bilgilerini ve sanal ağ.

### <a name="microsoftapimanagementservicecertificates"></a>Microsoft.ApiManagement/service/certificates
[Microsoft.ApiManagement/service/certificates](/azure/templates/microsoft.apimanagement/service/certificates) API Management güvenliğini yapılandırır. API Management Service Fabric kümeniz kümenizi erişimi olan bir istemci sertifikası kullanılarak hizmet bulma için kimlik doğrulaması gerekir. Bu öğretici oluştururken, daha önce belirtilen aynı sertifikayı kullanan [Windows Küme](service-fabric-tutorial-create-vnet-and-windows-cluster.md#createvaultandcert_anchor) veya [Linux kümesi](service-fabric-tutorial-create-vnet-and-linux-cluster.md#createvaultandcert_anchor), varsayılan olarak, kümeye erişmek için kullanılabilir. 

Bu öğretici, istemci kimlik doğrulaması ve küme düğümü, düğümü güvenlik için aynı sertifikayı kullanır. Bir Service Fabric kümesi erişmek için yapılandırılan yoksa ayrı bir istemci sertifikası kullanabilir. Sağlamak **adı**, **parola**, ve **veri** (base-64 kodlamalı dizesi) oluştururken, belirtilen küme sertifikanın özel anahtar dosyası (.pfx), Service Fabric kümesi.

### <a name="microsoftapimanagementservicebackends"></a>Microsoft.ApiManagement/service/backends
[Microsoft.ApiManagement/service/backends](/azure/templates/microsoft.apimanagement/service/backends) trafiği iletilir arka uç hizmetine açıklar. 

Service Fabric arka uçlarını için Service Fabric kümesi arka uç yerine belirli bir Service Fabric hizmeti ' dir. Bu, kümedeki birden fazla hizmeti yönlendirmek tek bir ilke sağlar. **Url** burada alandır tam hizmet adı bir hizmetin kümenizdeki bir arka uç ilkesinde hiçbir hizmet adı belirtilmezse, tüm istekleri varsayılan olarak yönlendirilir. Sahte hizmet adı gibi kullanabilir "fabric: / sahte/service" bir geri dönüş hizmetine sahip düşünmüyorsanız. **ResourceId** küme yönetim uç nokta belirtir.  **clientCertificateThumbprint** ve **serverCertificateThumbprints** kümeyle kimliğini doğrulamak için kullanılan sertifikaları tanımlayın.

### <a name="microsoftapimanagementserviceproducts"></a>Microsoft.ApiManagement/service/products
[Microsoft.ApiManagement/service/products](/azure/templates/microsoft.apimanagement/service/products) bir ürün oluşturur. Azure API Management'te bir ürün kullanım kotası ve kullanım koşullarını yanı sıra bir veya daha fazla API'leri içerir. Bir ürün yayımlandığında, geliştiriciler ürüne abone ve ürün API'lerine kullanmaya başlar. 

Bir tanımlayıcı girin **displayName** ve **açıklama** ürün için. Bu öğretici için bir abonelik gerekiyor ancak abonelik onayı bir yönetici tarafından değil.  Bu ürün **durumu** "yayımlanır" ve abonelere görünür. 

### <a name="microsoftapimanagementserviceapis"></a>Microsoft.ApiManagement/service/apis
[Microsoft.ApiManagement/service/apis](/azure/templates/microsoft.apimanagement/service/apis) bir API oluşturur. API Management bir API İstemci uygulamaları tarafından çağrılabilen işlemler kümesini temsil eder. Operations eklendikten sonra API bir ürüne eklenebilir ve yayımlanabilir. Bir API yayımlandığında abone ve geliştiriciler tarafından kullanılır.

- **displayName** API'nizi için herhangi bir ad olabilir. Bu öğretici için "Service Fabric uygulaması" kullanın.
- **ad** "service fabric uygulama" gibi API için benzersiz ve açıklayıcı bir ad sağlar. Geliştirici ve yayımcı portallarında görüntülenir. 
- **serviceUrl** API uygulama HTTP hizmeti başvuruyor. API management bu adrese isteklerini iletir. Service Fabric arka uçlarını için bu URL değeri kullanılmaz. Herhangi bir değer buraya koyabilirsiniz. Bu öğreticide, örneğin "http://servicefabric". 
- **yol** API management hizmeti temel URL'si eklenir. Temel URL bir API Management hizmet örneği tarafından barındırılan tüm API'leri yaygındır. API Management API'leri kendi soneki ayırır ve bu nedenle soneki her API için belirtilen yayımcı için benzersiz olmalıdır. 
- **protokolleri** API erişmek için hangi protokollerin kullanılabilir belirleyin. Bu öğretici için liste **http** ve **https**.
- **yol** API'si sonekidir. Bu öğretici için "Uygulamam" kullanın.

### <a name="microsoftapimanagementserviceapisoperations"></a>Microsoft.ApiManagement/service/apis/operations
[Microsoft.ApiManagement/service/apis/operations](/azure/templates/microsoft.apimanagement/service/apis/operations) önce bir API API Management kullanılabilir, işlemleri API'sine eklenmesi gerekiyor.  Dış istemcilere bir Service Fabric kümede çalışan ASP.NET Core durum bilgisiz hizmeti ile iletişim kurmak için kullanın.

Bir ön uç API işlemi eklemek için değerlerin doldurun:

- **displayName** ve **açıklama** işlemini açıklar. Bu öğretici için "Değerler" kullanın.
- **yöntem** HTTP fiilini belirtir.  Bu öğretici için belirtmek **almak**.
- **urlTemplate** API temel URL'sine eklenir ve tek bir HTTP işlemi tanımlar.  Bu öğretici için kullanmak `/api/values` .NET arka uç hizmetine eklediyseniz veya `getMessage` Java arka uç hizmetine eklediyseniz.  Varsayılan olarak, Service Fabric hizmeti arka ucuna gönderilen URL yolunu URL yolu burada belirtilen. "/ Api/değerleri gibi" hizmetinizin kullandığı aynı URL yolunu buraya kullanırsanız, işlem daha fazla değişiklik yapılmadan çalışır. Bu durumda, ayrıca yol yeniden yazma işlemi ilkenizde daha sonra belirtmeniz gerekir, arka uç Service Fabric hizmeti tarafından kullanılan URL yolu farklı bir URL yolu buraya de belirtebilir.

### <a name="microsoftapimanagementserviceapispolicies"></a>Microsoft.ApiManagement/service/apis/policies
[Microsoft.ApiManagement/service/apis/policies](/azure/templates/microsoft.apimanagement/service/apis/policies) her şeyi birlikte bağlar, bir arka uç ilkesi oluşturur. İstekleri yönlendirilen arka uç Service Fabric hizmeti yapılandırdığınız budur. Bu ilke için herhangi bir API işlemi uygulayabilirsiniz.  Daha fazla bilgi için bkz: [ilkelerine genel bakış](/azure/api-management/api-management-howto-policies). 

[Service Fabric için arka uç yapılandırması](/azure/api-management/api-management-transformation-policies#SetBackendService) aşağıdaki isteği yönlendirme denetimleri sağlar: 
 - Hizmet örneği seçimi ya da sabit kodlanmış bir Service Fabric hizmeti örnek adı belirterek (örneğin, `"fabric:/myapp/myservice"`) veya HTTP isteğinden oluşturulan (örneğin, `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`).
 - Tüm Service Fabric bölümleme düzenini kullanarak bir bölüm anahtarı oluşturma tarafından bölüm çözünürlüğü.
 - Durum bilgisi olan hizmetler için çoğaltma seçimi.
 - Bir hizmet konumu yeniden çözümlemeyi ve isteği yeniden göndermeyi koşullarını belirtmenize olanak veren çözümleme yeniden deneme koşulları.

**policyContent** olan Json kaçışlı İlkesi XML içeriği.  Bu öğretici için rota isteklerini daha önce dağıtılan doğrudan .NET veya Java durum bilgisiz hizmet için bir arka uç ilkesi oluşturun. Ekleme bir `set-backend-service` ilkesi gelen ilkeleri altında.  "Hizmet-adı" yerine `fabric:/ApiApplication/WebApiService` .NET arka uç hizmeti daha önce dağıttıysanız veya `fabric:/EchoServerApplication/EchoServerService` Java hizmet dağıttıysanız.
    
```xml
<policies>
  <inbound>
    <base/>
    <set-backend-service 
        backend-id="servicefabric"
        sf-service-instance-name="service-name"
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

## <a name="set-parameters-and-deploy-api-management"></a>Parametrelerini ayarlamak ve API Management dağıtma
Aşağıdaki boş parametre doldurun *apim.parameters.json* dağıtımınız için. 

|Parametre|Değer|
|---|---|
|apimInstanceName|BT apim|
|apimPublisherEmail|myemail@contosos.com|
|apimSku|Geliştirici|
|serviceFabricCertificateName|sfclustertutorialgroup320171031144217|
|CertificatePassword|q6D7nN %6ck@6| 
|serviceFabricCertificateThumbprint|C4C1E541AD512B8065280292A8BA6079C3F26F10 |
|serviceFabricCertificate|&lt;Base-64 kodlu bir dize&gt;|
|url_path|/ api/değerleri|
|clusterHttpManagementEndpoint|https://mysfcluster.southcentralus.cloudapp.Azure.com:19080|
|inbound_policy|&lt;XML dizesi&gt;|

*certificatePassword* ve *serviceFabricCertificateThumbprint* kümesini ayarlamak için kullanılan Küme sertifikanın eşleşmesi gerekir.  

*serviceFabricCertificate* sertifika aşağıdaki komut dosyası kullanılarak oluşturulan bir base-64 kodlu bir dize, değil:

```powershell
$bytes = [System.IO.File]::ReadAllBytes("C:\mycertificates\sfclustertutorialgroup220171109113527.pfx");
$b64 = [System.Convert]::ToBase64String($bytes);
[System.Io.File]::WriteAllText("C:\mycertificates\sfclustertutorialgroup220171109113527.txt", $b64);
```

İçinde *inbound_policy*, "service-name" yerine `fabric:/ApiApplication/WebApiService` .NET arka uç hizmeti daha önce dağıttıysanız veya `fabric:/EchoServerApplication/EchoServerService` Java hizmet dağıttıysanız.

```xml
<policies>
  <inbound>
    <base/>
    <set-backend-service 
        backend-id="servicefabric"
        sf-service-instance-name="service-name"
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

API yönetimi için Resource Manager şablonu ve parametre dosyalarını dağıtmak için aşağıdaki komut dosyasını kullanın:

```powershell
$ResourceGroupName = "sfclustertutorialgroup"
New-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile .\apim.json -TemplateParameterFile .\apim.parameters.json -Verbose
```

```azurecli
ResourceGroupName="sfclustertutorialgroup"
az group deployment create --name ApiMgmtDeployment --resource-group $ResourceGroupName --template-file apim.json --parameters @apim.parameters.json 
```

## <a name="test-it"></a>test

Artık Service Fabric API Yönetimi yoluyla, arka uç hizmetine bir istek göndermeden deneyebilirsiniz [Azure portal](https://portal.azure.com).

 1. API Management hizmeti seçin **API**.
 2. İçinde **Service Fabric uygulaması** önceki adımlarda, select oluşturduğunuz API **Test** sekmesini ve ardından **değerleri** işlemi.
 3. Tıklatın **Gönder** düğmesi bir test isteği arka uç hizmetine göndermek için.  Benzer şekilde bir HTTP yanıtı görmeniz gerekir:

    ```http
    HTTP/1.1 200 OK

    Transfer-Encoding: chunked

    Content-Type: application/json; charset=utf-8

    Vary: Origin

    Access-Control-Allow-Origin: https://apimanagement.hosting.portal.azure.net

    Access-Control-Allow-Credentials: true

    Access-Control-Expose-Headers: Transfer-Encoding,Date,Server,Vary,Ocp-Apim-Trace-Location

    Ocp-Apim-Trace-Location: https://apimgmtstuvyx3wa3oqhdbwy.blob.core.windows.net/apiinspectorcontainer/RaVVuJBQ9yxtdyH55BAsjQ2-1?sv=2015-07-08&sr=b&sig=Ab6dPyLpTGAU6TdmlEVu32DMfdCXTiKAASUlwSq3jcY%3D&se=2017-09-15T05%3A49%3A53Z&sp=r&traceId=ed9f1f4332e34883a774c34aa899b832

    Date: Thu, 14 Sep 2017 05:49:56 GMT


    [
    "value1",
    "value2"
    ]
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Küme, küme kaynağının yanı sıra diğer Azure kaynaklarından oluşur. Kümeyi ve kullandığı tüm kaynakları silmenin en basit yolu, kaynak grubunun silinmesidir.

Azure'da oturum açma ve küme kaldırmak istediğiniz abonelik kimliği seçin.  Abonelik Kimliğiniz için oturum açarak bulabileceğiniz [Azure portal](http://portal.azure.com). Kaynak grubu ve kullanarak tüm küme kaynakları silmek [Remove-AzureRMResourceGroup cmdlet'i](/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup).

```powershell
$ResourceGroupName = "sfclustertutorialgroup"
Remove-AzureRmResourceGroup -Name $ResourceGroupName -Force
```

```azurecli
ResourceGroupName="sfclustertutorialgroup"
az group delete --name $ResourceGroupName
```

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * API Management dağıtma
> * API Management yapılandırın
> * Bir API işlem oluşturma
> * Arka uç ilkesini yapılandırma
> * Bir ürüne API ekleme

[azure-powershell]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/

[apim-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.json
[apim-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.parameters.json

[network-arm]: https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.json
[network-parameters-arm]: https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.parameters.json

[cluster-arm]: https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.json
[cluster-parameters-arm]: https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.parameters.json

<!-- pics -->
[sf-apim-topology-overview]: ./media/service-fabric-tutorial-deploy-api-management/sf-apim-topology-overview.png
