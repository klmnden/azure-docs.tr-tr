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
ms.date: 09/13/2017
ms.author: ryanwi
ms.openlocfilehash: 8ff8c425189efdd7ea21984528bf7ea765e17955
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="deploy-api-management-with-service-fabric"></a>API Management Service Fabric ile dağıtma
Bu öğretici iki serinin bir parçasıdır. Bu öğretici nasıl ayarlanacağını gösterir [Azure API Management](../api-management/api-management-key-concepts.md) Service Fabric arka uç hizmetinde trafiğini yönlendirmek için Service Fabric ile.  İşlemi tamamladığınızda, bir sanal AĞA API Management dağıtılmış vardır, bir API işlemi arka uç durum bilgisi olmayan hizmetler için trafiği göndermek için yapılandırılmış. Service Fabric ile Azure API Management senaryoları hakkında daha fazla bilgi için bkz: [genel bakış](service-fabric-api-management-overview.md) makalesi.

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
> * API Management Service Fabric ile dağıtma

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce:
- Bir Azure aboneliğiniz yoksa, oluşturma bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Yükleme [Azure Powershell modülü sürümü 4.1 veya üzerini](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) veya [Azure CLI 2.0](/cli/azure/install-azure-cli).
- Güvenli oluşturma [Windows Küme](service-fabric-tutorial-create-vnet-and-windows-cluster.md) veya [Linux kümesi](service-fabric-tutorial-create-vnet-and-linux-cluster.md) Azure ile ilgili
- Windows Küme dağıtırsanız, bir Windows geliştirme ortamını ayarlama. Yükleme [Visual Studio 2017](http://www.visualstudio.com) ve **Azure geliştirme**, **ASP.NET ve web geliştirme**, ve **.NET Core platformlar arası geliştirme**iş yükleri.  Daha sonra ayarlanmış bir [.NET geliştirme ortamı](service-fabric-get-started.md).
- Linux kümesi dağıtırsanız, üzerinde bir Java geliştirme ortamını ayarlama [Linux](service-fabric-get-started-linux.md) veya [MacOS](service-fabric-get-started-mac.md).  Yükleme [doku CLI hizmet](service-fabric-cli.md). 

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

## <a name="deploy-api-management"></a>API Management dağıtma
Bulut uygulamalarını genellikle kullanıcılar, aygıtlar veya diğer uygulamalar için tek bir giriş noktası sağlamak için bir ön uç ağ geçidi gerekir. Service Fabric bir ağ geçidi herhangi bir ASP.NET Core uygulaması gibi durum bilgisiz hizmet veya olay hub'ları, IOT hub'ı ya da Azure API Management gibi trafik giriş için tasarlanan başka bir hizmet olabilir. Bu öğretici, Service Fabric uygulamaları için bir ağ geçidi olarak Azure API Yönetimi'ni kullanarak bir giriş var. API Management Service Fabric, arka uç Service Fabric hizmetlerinize yönlendirme kuralları zengin bir dizi API yayımlamanıza olanak tanıyan ile doğrudan tümleşir. 

Güvenli sahip olduğunuza [Windows Küme](service-fabric-tutorial-create-vnet-and-windows-cluster.md) veya [Linux kümesi](service-fabric-tutorial-create-vnet-and-linux-cluster.md) Azure üzerinde API Management API yönetimi için tasarlanmış NSG ve alt ağ sanal bir ağa (VNET) dağıtın. Bu öğreticide, API Management Resource Manager şablonu VNET, alt ağ ve önceki ayarladığınız NSG adları kullanmak üzere önceden yapılandırılmıştır [Windows Küme öğretici](service-fabric-tutorial-create-vnet-and-windows-cluster.md) veya [Linux küme öğretici](service-fabric-tutorial-create-vnet-and-linux-cluster.md). 

Aşağıdaki Resource Manager şablonu ve parametre dosyasını karşıdan yükleyin:
 
- [apim.JSON][apim-arm]
- [apim.Parameters.JSON][apim-parameters-arm]

Boş parametre doldurun `apim.parameters.json` dağıtımınız için.

API yönetimi için Resource Manager şablonu ve parametre dosyalarını dağıtmak için aşağıdaki komut dosyasını kullanın:

```powershell
$ResourceGroupName = "tutorialgroup"
New-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile .\apim.json -TemplateParameterFile .\apim.parameters.json -Verbose
```

```azurecli
ResourceGroupName="tutorialgroup"
az group deployment create --name ApiMgmtDeployment --resource-group $ResourceGroupName --template-file apim.json --parameters @apim.parameters.json 
```

## <a name="configure-api-management"></a>API Management yapılandırın

API Management ve Service Fabric kümesi dağıtıldığı sonra güvenlik ayarlarını ve Service Fabric arka uç API Management'te yapılandırabilirsiniz. Bu, Service Fabric kümesi trafiği gönderen bir arka uç hizmet ilkesi oluşturmanıza olanak sağlar.

### <a name="configure-api-management-security"></a>API yönetim güvenliği yapılandırma

Service Fabric arka uç yapılandırmak için ilk API Management güvenlik ayarlarını yapılandırmanız gerekir. Güvenlik ayarlarını yapılandırmak üzere API Management hizmetiniz Azure portalında gidin.

#### <a name="enable-the-api-management-rest-api"></a>API Management REST API etkinleştir

API Management REST API şu anda bir arka uç hizmeti yapılandırmak için tek yoludur. API Management REST API etkinleştirmek ve onu güvenliğini sağlamak için ilk adımdır bakın.

 1. API Management hizmeti seçin **Yönetimi API** altında **güvenlik**.
 2. Denetleme **API Management REST API etkinleştir** onay kutusu.
 3. Not **yönetim API'si URL**, hangi daha sonra Service Fabric arka ucu ayarlamak için kullanırız.
 4. Generate bir **erişim belirteci** bir sona erme tarihi ve bir anahtar seçerek, ardından **Generate** sayfasının en altına doğru düğmesi.
 5. Kopya **erişim belirteci** ve kaydedin.  Aşağıdaki adımlarda erişim belirteci kullanırız. Bu birincil anahtar ve ikincil anahtar farklı olduğuna dikkat edin.

#### <a name="upload-a-service-fabric-client-certificate"></a>Service Fabric istemci sertifikasını karşıya yükle

API Management Service Fabric kümeniz kümenizi erişimi olan bir istemci sertifikası kullanılarak hizmet bulma için kimlik doğrulaması gerekir. Kolaylık olması için Bu öğretici oluştururken, daha önce belirtilen aynı sertifikayı kullanır. [Windows Küme](service-fabric-tutorial-create-vnet-and-windows-cluster.md#createvaultandcert_anchor) veya [Linux kümesi](service-fabric-tutorial-create-vnet-and-linux-cluster.md#createvaultandcert_anchor), varsayılan olarak, kümeye erişmek için kullanılabilir.

 1. API Management hizmeti seçin **istemci sertifikalarını** altında **güvenlik**.
 2. Tıklatın **+ Ekle** düğmesi.
 2. Service Fabric kümesi oluştururken, belirtilen küme sertifikanın özel anahtar dosyası (.pfx) seçin, bir ad verin ve özel anahtar parolası sağlayın.

> [!NOTE]
> Bu öğretici, istemci kimlik doğrulaması ve küme düğümü, düğümü güvenlik için aynı sertifikayı kullanır. Bir Service Fabric kümesi erişmek için yapılandırılan yoksa ayrı bir istemci sertifikası kullanabilir.

### <a name="configure-the-backend"></a>Arka uç yapılandırın

API Management güvenlik yapılandırıldığında, Service Fabric arka uç yapılandırabilirsiniz. Service Fabric arka uçlarını için Service Fabric kümesi, belirli bir Service Fabric hizmeti yerine arka uç grubudur. Bu, kümedeki birden fazla hizmeti yönlendirmek tek bir ilke sağlar.

Bu adım, API Management daha önce karşıya küme sertifikanızın daha önce oluşturulan erişim belirteci ve parmak izi gerektirir.

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

**Url** burada parametredir tam hizmet adı bir hizmetin kümenizdeki bir arka uç ilkesinde hiçbir hizmet adı belirtilmezse, tüm istekleri varsayılan olarak yönlendirilir. Sahte hizmet adı gibi kullanabilir "fabric: / sahte/service" bir geri dönüş hizmetine sahip düşünmüyorsanız.

API Management başvuran [arka uç API başvuru belgeleri](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend) her bir alan hakkında daha fazla ayrıntı için.

```python
#import requests library for making REST calls
import requests
import json

#specify url
url = 'https://your-apim.management.azure-api.net/backends/servicefabric?api-version=2016-10-10'

token = "SharedAccessSignature integration&201710140514&Lqnbei7n2Sot6doiNtxMFPUi/m9LsNa+1ZK/PdxqFl49JFWjXh1wW5Gd99r/tIOeHp6dU8UV5iZUdOPfcrm5hg=="

payload = {
    "description": "My Service Fabric backend",
    "url": "fabric:/ApiApplication/ApiWebService",
    "protocol": "http",
    "resourceId": "https://tutorialcluster.eastus.cloudapp.azure.com:19080",
    "properties": {
        "serviceFabricCluster": {
            "managementEndpoints": [ "https://tutorialcluster.eastus.cloudapp.azure.com:19080" ],
            "clientCertificateThumbprint": "97EDD7E4979FB072AF3E480A5E5EE34B1B89DD80",
            "serverCertificateThumbprints": [ "97EDD7E4979FB072AF3E480A5E5EE34B1B89DD80" ],
            "maxPartitionResolutionRetries" : 5
        }
    }
}

headers = {'Authorization': token, "Content-Type": "application/json"}

#Call REST API
response = requests.put(url, data=json.dumps(payload), headers=headers)

#Print Response
print(response.status_code)
print(response.text)
```

## <a name="deploy-a-service-fabric-back-end-service"></a>Service Fabric arka uç hizmet dağıtma

API Management için bir arka uç olarak yapılandırılan Service Fabric sahip olduğunuza göre Apı'lerinizi Service Fabric hizmetleriniz için trafiği göndermek için arka uç ilkeleri yazabilirsiniz. Ancak önce isteklerini kabul etmek için Service Fabric içinde çalışan bir hizmete gerekir.  Daha önce oluşturduğunuz varsa bir [Windows Küme](service-fabric-tutorial-create-vnet-and-windows-cluster.md), .NET Service Fabric hizmeti dağıtın.  Daha önce oluşturduğunuz varsa bir [Linux kümesi](service-fabric-tutorial-create-vnet-and-linux-cluster.md), Java Service Fabric hizmeti dağıtın.

### <a name="deploy-a-net-service-fabric-service"></a>.NET Service Fabric hizmet dağıtma

Bu öğreticide, temel bir durum bilgisiz ASP.NET Core güvenilir varsayılan Web API projesi şablonunu kullanarak hizmet oluşturun. Azure API Management kullanıma hizmetiniz için bir HTTP uç noktası oluşturur:

```
/api/values
```

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

    Bu, bir bağlantı noktasından, biz ağ güvenlik grubu için API Management trafiğe izin verme küme Resource Manager şablonunda aracılığıyla açılan dinamik olarak uygulama bağlantı noktası aralığını belirtmek Service Fabric sağlar.
 
 6. Web API doğrulamak için Visual Studio'da F5 tuşuna basarak yerel olarak kullanılabilir. 

    Açık Service Fabric Explorer ve ASP.NET Core hizmeti belirli bir örneği inceleyin taban adresi görmek için hizmet üzerinde dinliyor. Ekleme `/api/values` bankasına adresi ve bir tarayıcıda açın. Bu Web API şablonunda ValuesController Get yöntemini çağırır. Bu şablon tarafından sağlanan varsayılan yanıt, iki dizeyi içeren bir JSON dizisi döndürür:

    ```json
    ["value1", "value2"]`
    ```

    Bu, Azure API Management'te aracılığıyla kullanıma uç noktadır.

 7. Son olarak, Azure kümenizdeki uygulamayı dağıtın. [Visual Studio kullanarak](service-fabric-publish-app-remote-cluster.md#to-publish-an-application-using-the-publish-service-fabric-application-dialog-box), uygulama projesine sağ tıklatın ve **Yayımla**. Küme uç noktası sağlayın (örneğin, `mycluster.southcentralus.cloudapp.azure.com:19000`) Azure Service Fabric kümenizdeki uygulamayı dağıtmak için.

Adlı bir ASP.NET Core durum bilgisiz hizmeti `fabric:/ApiApplication/WebApiService` artık Azure Service Fabric kümenizdeki çalıştırması gerekir.

### <a name="create-a-java-service-fabric-service"></a>Java Service Fabric hizmeti oluşturma
Bu öğretici için ileti geri kullanıcıya görüntülemektedir bir temel web sunucusu dağıtın. Echo sunucu örnek uygulaması, Azure API Management kullanıma hizmetiniz için bir HTTP uç noktası içerir.

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


## <a name="create-an-api-operation"></a>Bir API işlem oluşturma

Şimdi biz, dış istemcilere Service Fabric kümede çalışan ASP.NET Core durum bilgisiz hizmeti ile iletişim kurmak için API Management bir işlem oluşturmak için hazır.

1. Azure portalında oturum açın ve API Management hizmeti dağıtımınızı gidin.
2. API Management hizmeti dikey penceresinde, seçin **API'leri**
3. Tıklayarak yeni bir API eklemek **+ API**, ardından **boş API** kutusu ve iletişim kutusunu doldurma:

    - **Web hizmeti URL'si**: Service Fabric arka uçlarını için değil Bu URL değeri kullanılır. Herhangi bir değer buraya koyabilirsiniz. Bu öğretici için kullanın: "http://servicefabric".
    - **Görünen ad**: API'nizi için herhangi bir ad sağlayın. Bu öğretici için "Service Fabric uygulaması" kullanın.
    - **Ad**: "service fabric uygulama" girin.
    - **URL şeması**: seçin **HTTP**, **HTTPS**, veya **her ikisi de**. Bu öğretici için kullanmak **her ikisi de**.
    - **API'si URL soneki**: bir sonek için bizim API sağlar. Bu öğretici için "Uygulamam" kullanın.
 
4. Seçin **Service Fabric uygulaması** API'leri tıklatın ve listeden **+ ekleme işlemi** bir ön uç API işlemi eklemek için. Değerlerin doldurun:
    
    - **URL**: seçin **almak** ve bir URL yolu için API sağlar. Bu öğretici için "/ api/değerler" kullanın.  Varsayılan olarak, Service Fabric hizmeti arka ucuna gönderilen URL yolunu URL yolu burada belirtilen. Hizmetinizin kullandığı aynı URL yolunu buraya kullanırsanız, bu durumda "/ api/değerler" ve sonra işlemi çalışır daha fazla değişiklik yapılmadan. Bu durumda, ayrıca yol yeniden yazma işlemi ilkenizde daha sonra belirtmeniz gerekir, arka uç Service Fabric hizmeti tarafından kullanılan URL yolu farklı bir URL yolu buraya de belirtebilir.
    - **Görünen ad**: API için herhangi bir ad sağlayın. Bu öğretici için "Değerler" kullanın.

5. **Kaydet** düğmesine tıklayın.

## <a name="configure-a-backend-policy"></a>Arka uç ilkesini yapılandırma

Arka uç ilke her şeyi birbirine bağlar. İstekleri yönlendirilen arka uç Service Fabric hizmeti yapılandırdığınız budur. Bu ilke için herhangi bir API işlemi uygulayabilirsiniz. [Service Fabric için arka uç yapılandırması](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService) aşağıdaki isteği yönlendirme denetimleri sağlar: 
 - Hizmet örneği seçimi ya da sabit kodlanmış bir Service Fabric hizmeti örnek adı belirterek (örneğin, `"fabric:/myapp/myservice"`) veya HTTP isteğinden oluşturulan (örneğin, `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`).
 - Tüm Service Fabric bölümleme düzenini kullanarak bir bölüm anahtarı oluşturma tarafından bölüm çözünürlüğü.
 - Durum bilgisi olan hizmetler için çoğaltma seçimi.
 - Bir hizmet konumu yeniden çözümlemeyi ve isteği yeniden göndermeyi koşullarını belirtmenize olanak veren çözümleme yeniden deneme koşulları.

Bu öğretici için daha önce dağıttığınız ASP.NET Core durum bilgisiz hizmete isteklerini doğrudan yönlendiren bir arka uç ilkesi oluşturun:

 1. Seçme ve düzenleme **gelen ilkeleri** Düzenle simgesine tıklayarak ve ardından seçerek değerleri işlemi için **kod görünümü**.
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

### <a name="add-the-api-to-a-product"></a>Bir ürüne API ekleme 

API aramadan önce burada kullanıcılara erişim izni verebilirsiniz ürün eklenmesi gerekir. 

 1. API Management hizmeti seçin **ürünleri**.
 2. Varsayılan olarak, API Management sağlayıcıları iki ürünler: Starter ve sınırsız. Sınırsız ürün seçin.
 3. Seçin **+ API Ekle**.
 4. Seçin `Service Fabric App` tıklayın ve önceki adımları içinde oluşturduğunuz API **seçin** düğmesi.

### <a name="test-it"></a>test

Şimdi doğrudan Azure Portalı'ndan API Management üzerinden Service Fabric, arka uç hizmetinde bir istek göndermesini deneyin.

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
$ResourceGroupName = "tutorialgroup"
Remove-AzureRmResourceGroup -Name $ResourceGroupName -Force
```

```azurecli
ResourceGroupName="tutorialgroup"
az group delete --name $ResourceGroupName
```

## <a name="conclusion"></a>Sonuç
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