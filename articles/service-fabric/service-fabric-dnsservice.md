---
title: Azure Service Fabric DNS hizmeti | Microsoft Docs
description: "Service Fabric'in dns hizmeti mikro hizmetler küme içindeki bulmak için kullanın."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: vturecek
ms.assetid: 47f5c1c1-8fc8-4b80-a081-bc308f3655d3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/27/2017
ms.author: msfussell
ms.openlocfilehash: 9871bc5aa4e74ab0faef401d67c4e9558eb5e14b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="dns-service-in-azure-service-fabric"></a>Azure Service Fabric DNS hizmeti
DNS hizmeti DNS protokolünü kullanarak diğer hizmetleri bulmak için kümeyi olanak veren bir isteğe bağlı sistem hizmetidir.

Birçok hizmetler, özellikle kapsayıcılı hizmetler var olan bir URL adına sahip olabilir ve standart DNS Protokolü (adlandırma hizmeti Protokolü yerine) kullanarak bunları gidermek için özellikle "kaldırın ve shift" senaryolarda tercih edilir. DNS hizmeti, bir hizmet adı DNS adlarını eşlemek ve bu nedenle uç noktası IP adreslerini çözümlemesine olanak sağlar. 

DNS hizmeti sırayla Adlandırma Hizmeti uç noktası döndürmek için hizmet tarafından çözümlenen hizmet adları için DNS adlarını eşler. Hizmeti için DNS ad oluşturma sırasında sağlanır. 

![Hizmet uç noktaları][0]

## <a name="enabling-the-dns-service"></a>DNS hizmeti etkinleştirme
İlk DNS hizmeti kümenizdeki etkinleştirmeniz gerekir. Şablonu dağıtmak istediğiniz kümenin alın. Kullanabilir [örnek şablonlarından](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) veya Resource Manager şablonu oluşturun. Aşağıdaki adımlarla DNS hizmeti etkinleştirebilirsiniz:

1. Denetleyin `apiversion` ayarlanır `2017-07-01-preview` için `Microsoft.ServiceFabric/clusters` kaynak ve aksi durumda aşağıdaki kod parçacığında gösterildiği gibi güncelleştirebilirsiniz:

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. DNS hizmeti aşağıdakileri ekleyerek şimdi etkinleştirmek `addonFeatures` sonra bölümünde `fabricSettings` bölümünde aşağıdaki kod parçacığında gösterildiği gibi: 

    ```json
        "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "DnsService"
        ],
    ```

3. Küme şablonunuzu önceki değişiklikleriyle güncelleştirdikten sonra bunları uygulamak ve yükseltme izin tamamlandı. Tamamlandıktan sonra DNS sistem hizmetini başlatır çağrılan kümenizde çalışan `fabric:/System/DnsService` sistem hizmeti bölümünde bulunan Service Fabric explorer. 

Alternatif olarak, küme oluşturma sırasında portal üzerinden DNS hizmeti etkinleştirebilirsiniz. DNS hizmeti için kutuyu işaretleyerek etkin hale getirilebilir `Include DNS service` içinde `Cluster configuration` aşağıdaki ekran görüntüsünde gösterildiği gibi menüsü:

![DNS hizmeti portal üzerinden etkinleştirme][2]


## <a name="setting-the-dns-name-for-your-service"></a>Hizmetiniz için DNS adı ayarlama
DNS hizmeti kümenizdeki çalışmaya başladıktan sonra hizmetleriniz için bir DNS adı ya da varsayılan Hizmetleri için bildirimli olarak ayarlayabileceğiniz `ApplicationManifest.xml` veya Powershell komutları ile.

### <a name="setting-the-dns-name-for-a-default-service-in-the-applicationmanifestxml"></a>Varsayılan hizmet için DNS adı ApplicationManifest.xml ayarlama
Visual Studio veya tercih ettiğiniz Düzenleyicisi projenizi açın ve açık `ApplicationManifest.xml` dosya. Varsayılan Hizmetleri bölümüne gidin ve her hizmet için ekleyin `ServiceDnsName` özniteliği. Aşağıdaki örnek hizmeti DNS adı ayarlamak nasıl gösterir`service1.application1`

```xml
    <Service Name="Stateless1" ServiceDnsName="service1.application1">
    <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
        <SingletonPartition />
    </StatelessService>
    </Service>
```
Uygulama dağıtıldığında, Service Fabric Explorer'da hizmet örneği aşağıdaki resimde gösterildiği gibi bu örneği için DNS adını gösterir: 

![Hizmet uç noktaları][1]

### <a name="setting-the-dns-name-for-a-service-using-powershell"></a>Powershell kullanarak bir hizmet için DNS adı ayarlama
Bir hizmet için DNS adını kullanarak oluştururken ayarlayabilirsiniz `New-ServiceFabricService` Powershell. Aşağıdaki örnek, DNS adıyla yeni bir durum bilgisiz hizmeti oluşturur`service1.application1`

```powershell
    New-ServiceFabricService `
    -Stateless `
    -PartitionSchemeSingleton `
    -ApplicationName `fabric:/application1 `
    -ServiceName fabric:/application1/service1 `
    -ServiceTypeName Service1Type `
    -InstanceCount 1 `
    -ServiceDnsName service1.application1
```

## <a name="using-dns-in-your-services"></a>DNS, Hizmetleri'ni kullanarak
Birden fazla hizmet dağıtırsanız, uç noktaları ile bir DNS adı kullanarak iletişim kurmak için diğer hizmetlerin bulabilirsiniz. DNS hizmeti, yalnızca DNS protokolü durum bilgisi olan Hizmetleri ile iletişim kuramıyor beri durum bilgisi olmayan hizmetler için geçerlidir. Durum bilgisi olan hizmetler için belirli bir hizmet bölümü çağırmak için yerleşik bir ters proxy http çağrıları için kullanabilirsiniz.

Aşağıdaki kod, yalnızca bir normal http bağlantı noktası ve isteğe bağlı bir yoldur URL'SİNİN bir parçası verdiğiniz çağrıdır başka bir hizmete çağrı gösterilmektedir.

```csharp
public class ValuesController : Controller
{
    // GET api
    [HttpGet]
    public async Task<string> Get()
    {
        string result = "";
        try
        {
            Uri uri = new Uri("http://service1.application1:8080/api/values");
            HttpClient client = new HttpClient();
            var response = await client.GetAsync(uri);
            result = await response.Content.ReadAsStringAsync();
            
        }
        catch (Exception e)
        {
            Console.Write(e.Message);
        }

        return result;
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
Hizmet iletişimi ile küme içindeki hakkında daha fazla bilgi [bağlanmak ve Hizmetleri ile iletişim](service-fabric-connect-and-communicate-with-services.md)

[0]: ./media/service-fabric-connect-and-communicate-with-services/dns.png
[1]: ./media/service-fabric-dnsservice/servicefabric-explorer-dns.PNG
[2]: ./media/service-fabric-dnsservice/DNSService.PNG
