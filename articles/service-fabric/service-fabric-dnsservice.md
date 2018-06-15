---
title: Azure Service Fabric DNS hizmeti | Microsoft Docs
description: Service Fabric'in dns hizmeti mikro hizmetler küme içindeki bulmak için kullanın.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: vturecek
ms.assetid: 47f5c1c1-8fc8-4b80-a081-bc308f3655d3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/27/2017
ms.author: msfussell
ms.openlocfilehash: 656aed1f1fbd3294c4318520058ace480fd2219c
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34205003"
---
# <a name="dns-service-in-azure-service-fabric"></a>Azure Service Fabric DNS hizmeti
DNS hizmeti DNS protokolünü kullanarak diğer hizmetleri bulmak için kümeyi olanak veren bir isteğe bağlı sistem hizmetidir. 

Birçok hizmetler, özellikle kapsayıcılı hizmetler var olan bir URL adına sahip olabilir ve standart DNS Protokolü (adlandırma hizmeti Protokolü yerine) kullanarak bunları gidermek için özellikle "kaldırın ve shift" senaryolarda tercih edilir. DNS hizmeti, bir hizmet adı DNS adlarını eşlemek ve bu nedenle uç noktası IP adreslerini çözümlemesine olanak sağlar. 

DNS hizmeti sırayla Adlandırma Hizmeti uç noktası döndürmek için hizmet tarafından çözümlenen hizmet adları için DNS adlarını eşler. Hizmeti için DNS ad oluşturma sırasında sağlanır.

![Hizmet uç noktaları](./media/service-fabric-dnsservice/dns.png)

Dinamik bağlantı noktaları DNS hizmeti tarafından desteklenmez. Dinamik bağlantı noktalarını kullanıma sunulan hizmetlerin çözümlenmesi için kullanmak [ters proxy hizmeti](./service-fabric-reverseproxy.md).

## <a name="enabling-the-dns-service"></a>DNS hizmeti etkinleştirme
Portalı kullanarak bir küme oluştururken, DNS hizmeti varsayılan olarak etkindir **dahil DNS hizmeti** onay kutusunu **küme yapılandırması** menüsü:

![DNS hizmeti portal üzerinden etkinleştirme][2]

Kümenizi oluşturmak için portal kullanmadığınızı veya varolan bir küme güncelleştiriliyor, bir şablon DNS hizmeti etkinleştirin yapmanız gerekir:

- Yeni bir küme dağıtmak için kullanabilir [örnek şablonlarından](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) veya oluşturma bir kendi Resource Manager şablonu. 
- Varolan bir kümeye güncelleştirmek için küme kaynak grubu tıklatın ve portal gidebilirsiniz **Otomasyon betiğini** küme ve gruptaki diğer kaynakların geçerli durumunu yansıtır bir şablonla çalışmak. Daha fazla bilgi için bkz: [şablonu kaynak grubundan dışarı aktarma](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template#export-the-template-from-resource-group).

Bir şablonu oluşturduktan sonra aşağıdaki adımlarla DNS hizmeti etkinleştirebilirsiniz:

1. Denetleyin `apiversion` ayarlanır `2017-07-01-preview` veya daha yeni `Microsoft.ServiceFabric/clusters` kaynak ve aksi durumda aşağıdaki kod parçacığında gösterildiği gibi güncelleştirebilirsiniz:

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

3. Küme şablonunuzu önceki değişiklikleriyle güncelleştirdikten sonra bunları uygulamak ve yükseltme izin tamamlandı. Yükseltme işlemi tamamlandığında, kümenizde çalışan DNS sistem hizmetini başlatır. Hizmet adı `fabric:/System/DnsService`, ve bunun altında bulabilirsiniz **sistem** service Service Fabric explorer bölümünde. 


## <a name="setting-the-dns-name-for-your-service"></a>Hizmetiniz için DNS adı ayarlama
DNS hizmeti kümenizdeki çalışmaya başladıktan sonra hizmetleriniz için bir DNS adı ya da varsayılan Hizmetleri için bildirimli olarak ayarlayabileceğiniz `ApplicationManifest.xml` veya PowerShell komutları ile.

DNS hizmetiniz için küme çözümlenebilir adıdır. Alanının adlandırma şeması kullanmanız önerilir `<ServiceDnsName>.<AppInstanceName>`; Örneğin, `service1.application1`. Bunun yapılması küme DNS adının benzersizliğini sağlar. Docker kullanarak bir uygulama dağıtılırsa oluşturma, hizmetleri bu adlandırma şeması kullanarak DNS adlarını otomatik olarak atanır.

### <a name="setting-the-dns-name-for-a-default-service-in-the-applicationmanifestxml"></a>Varsayılan hizmet için DNS adı ApplicationManifest.xml ayarlama
Visual Studio veya tercih ettiğiniz Düzenleyicisi projenizi açın ve açık `ApplicationManifest.xml` dosya. Varsayılan Hizmetleri bölümüne gidin ve her hizmet için ekleyin `ServiceDnsName` özniteliği. Aşağıdaki örnek hizmeti DNS adı ayarlamak nasıl gösterir `service1.application1`

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
Bir hizmet için DNS adını kullanarak oluştururken ayarlayabilirsiniz `New-ServiceFabricService` Powershell. Aşağıdaki örnek, DNS adıyla yeni bir durum bilgisiz hizmeti oluşturur `service1.application1`

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
Birden fazla hizmet dağıtırsanız, uç noktaları ile bir DNS adı kullanarak iletişim kurmak için diğer hizmetlerin bulabilirsiniz. DNS hizmeti, yalnızca DNS protokolü durum bilgisi olan Hizmetleri ile iletişim kuramıyor beri durum bilgisi olmayan hizmetler için geçerlidir. Durum bilgisi olan hizmetler için yerleşik kullanabilirsiniz [ters proxy hizmeti](./service-fabric-reverseproxy.md) belirli bir hizmet bölümü çağırmak http çağrıları için. Dinamik bağlantı noktaları DNS hizmeti tarafından desteklenmez. Dinamik bağlantı noktaları kullanan Hizmetleri çözümlemek için ters proxy kullanabilirsiniz.

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
