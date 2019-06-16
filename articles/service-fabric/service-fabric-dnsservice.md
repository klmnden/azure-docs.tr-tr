---
title: Azure Service Fabric DNS hizmeti | Microsoft Docs
description: Mikro hizmetler kümesi içinde bulmak için Service Fabric'in dns hizmetini kullanın.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: vturecek
ms.assetid: 47f5c1c1-8fc8-4b80-a081-bc308f3655d3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/20/2018
ms.author: aljo
ms.openlocfilehash: 3b3262eadc732c23000a66f24aaeeed4d9794db0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60947682"
---
# <a name="dns-service-in-azure-service-fabric"></a>Azure Service fabric'te DNS hizmeti
DNS hizmeti kümenizde DNS protokolünü kullanarak diğer hizmetleri bulmak için etkinleştirebileceğiniz bir isteğe bağlı sistem hizmetidir. 

Birçok hizmet, özellikle kapsayıcılı hizmetler önceden var olan bir URL ulaşılamaz. Service Fabric adlandırma Ağ Geçidi Protokolü yerine standart DNS protokolünü kullanarak bu hizmetlerin çözümlenmesi için tercih edilir. DNS hizmeti, DNS adlarını bir hizmet adına eşlemenize ve bu nedenle bitiş IP adreslerini sağlar. Bu işlevselliğin farklı platformlar arasında taşınabilirlik kapsayıcı hizmetlerinin tutar ve "kaldırma ve kaydırma" yapabilirsiniz senaryoları var olanı Kullan vererek daha kolay, hizmet URL'leri yerine yeniden kod yazmak zorunda kalmadan adlandırma hizmeti yararlanın. 

DNS hizmeti sırayla Adlandırma Hizmeti uç noktası döndürülecek hizmet tarafından çözülen hizmet adları, DNS adlarını eşler. Hizmetin DNS adı, oluşturma sırasında sağlanır. Aşağıdaki diyagramda, durum bilgisi olmayan hizmetler için DNS hizmetinin nasıl çalıştığı gösterilmektedir.

![Hizmet uç noktaları](./media/service-fabric-dnsservice/stateless-dns.png)

Service Fabric sürümü 6.3 ile başlayarak, Service Fabric DNS protokolü bölümlenmiş durum bilgisi olan hizmetler ele alan bir şema içerecek şekilde genişletilmiştir. Bu uzantılar belirli bir bölüm IP adreslerini bölüm adı ve durum bilgisi olan hizmet DNS adı bir birleşimini kullanarak çözümlemek mümkün kılar. Tüm üç bölümleme düzenleri desteklenir:

- Bölümleme adlı
- Aralıklı bölümleme
- Singleton bölümleme

Aşağıdaki diyagramda, bölümlenmiş durum bilgisi olan hizmetler için DNS hizmetinin nasıl çalıştığı gösterilmektedir.

![durum bilgisi olan hizmet uç noktaları](./media/service-fabric-dnsservice/stateful-dns.png)

Dinamik bağlantı noktaları, DNS hizmeti tarafından desteklenmez. Dinamik bağlantı noktaları üzerinde kullanıma sunulan hizmetlerin çözümlenmesi için kullanmak [ters proxy hizmeti](./service-fabric-reverseproxy.md).

## <a name="enabling-the-dns-service"></a>DNS hizmetini etkinleştirme
> [!NOTE]
> Service Fabric Hizmetleri için DNS hizmeti Linux üzerindeki henüz desteklenmiyor.

Portalı kullanarak bir küme oluşturduğunuzda, DNS hizmeti varsayılan olarak etkindir **dahil DNS hizmeti** onay kutusunu **küme yapılandırması** menüsü:

![DNS hizmeti Portalı aracılığıyla etkinleştirme](./media/service-fabric-dnsservice/enable-dns-service.png)

Kümenizi oluşturmak için portalı kullanmıyorsanız veya mevcut bir kümeye güncelleştiriyorsanız şablonunda DNS hizmetini etkinleştirmeniz gerekir:

- Yeni bir kümeye dağıtmak için kullanabilir [örnek şablonlarından](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) veya kendi Resource Manager şablonu oluşturun. 
- Var olan bir kümeyi güncelleştirmek için küme kaynak grubu portalı tıklayıp gidebilirsiniz **Otomasyon betiği** küme ve diğer kaynakları grubu, geçerli durumunu yansıtan bir şablonu çalışmak için. Daha fazla bilgi için bkz. [şablonu kaynak grubundan dışarı aktarma](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template).

Bir şablonu oluşturduktan sonra DNS hizmeti aşağıdaki adımlarla etkinleştirebilirsiniz:

1. Bu maddeyi `apiversion` ayarlanır `2017-07-01-preview` veya üzeri için `Microsoft.ServiceFabric/clusters` kaynak ve aksi takdirde, aşağıdaki örnekte gösterildiği gibi güncelleştirin:

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Artık DNS hizmeti, aşağıdaki yollardan biriyle etkinleştir:

   - Varsayılan ayarlarla DNS hizmetini etkinleştirmek için eklemeniz `addonFeatures` bölümünü `properties` bölümünde aşağıdaki örnekte gösterildiği gibi:

       ```json
           "properties": {
              ...

              "addonFeatures": [
                "DnsService"
              ],
              ...
           }
       ```
   - Varsayılan ayarları dışında hizmetiyle etkinleştirmek için eklemeniz bir `DnsService` bölümünü `fabricSettings` bölümünü `properties` bölümü. Bu durumda, için DnsService eklemenize gerek yoktur `addonFeatures`. DNS hizmeti için ayarlanabilen özellikleri hakkında daha fazla bilgi için bkz. [DNS hizmeti ayarları](./service-fabric-cluster-fabric-settings.md#dnsservice).

       ```json
           "properties": {
             ...  
             "fabricSettings": [
               ...
               {
                 "name": "DnsService",
                 "parameters": [
                   {
                     "name": "IsEnabled",
                     "value": "true"
                   },
                   {
                     "name": "PartitionSuffix",
                     "value": "--"
                   },
                   {
                     "name": "PartitionPrefix",
                     "value": "--"
                   }
                 ]
               },
               ...
              ]
            }
       ```
1. Küme şablonu değişikliklerinizi güncelleştirdikten sonra bunları uygulamak ve izin yükseltme tamamlandı. Yükseltme işlemi tamamlandığında, kümenizde çalışan DNS sistem hizmetini başlatır. Hizmet adı `fabric:/System/DnsService`, ve bunun altında bulabilirsiniz **sistem** hizmet bölümünde Service Fabric Explorer. 


## <a name="setting-the-dns-name-for-your-service"></a>Hizmet DNS adı ayarlama
Hizmetleriniz için bir DNS adı ya da varsayılan hizmetler için bildirimli olarak ApplicationManifest.xml dosyasına veya PowerShell komutları aracılığıyla ayarlayabilirsiniz.

Hizmetiniz için DNS adını küme çözümlenebilir olduğundan küme genelinde benzersiz DNS adını sağlamak önemlidir. 

Adlandırma düzeni kullanmanızı önemle tavsiye edilir `<ServiceDnsName>.<AppInstanceName>`; Örneğin, `service1.application1`. Docker'ı kullanarak bir uygulama dağıtılırsa compose, hizmetleri bu adlandırma şeması kullanarak DNS adlarını otomatik olarak atanır.

### <a name="setting-the-dns-name-for-a-default-service-in-the-applicationmanifestxml"></a>Varsayılan hizmet DNS adı ApplicationManifest.xml ayarlama
Projenizi Visual Studio veya tercih ettiğiniz düzenleyiciyi açın ve ApplicationManifest.xml dosyasını açın. Varsayılan Hizmetleri bölümüne gidin ve her hizmet için ekleme `ServiceDnsName` özniteliği. Aşağıdaki örnek hizmeti DNS adı ayarlamak nasıl gösterir `service1.application1`

```xml
    <Service Name="Stateless1" ServiceDnsName="service1.application1">
      <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
```
Uygulama dağıtıldıktan sonra Service Fabric Explorer hizmet örneği aşağıdaki şekilde gösterildiği gibi bu örneği için DNS adını gösterir: 

![Hizmet uç noktaları](./media/service-fabric-dnsservice/service-fabric-explorer-dns.png)

Aşağıdaki örnek, bir durum bilgisi olan hizmet için DNS adını ayarlar `statefulsvc.app`. Hizmet, adlandırılmış bir bölümleme düzeni kullanır. Bölüm adları küçük harf olduğuna dikkat edin. Bu, DNS sorguları hedeflenen bölümleri için bir gereksinimdir; Daha fazla bilgi için [yapmak DNS sorgularına bir durum bilgisi olan hizmet bölüm](https://docs.microsoft.com/azure/service-fabric/service-fabric-dnsservice#preview-making-dns-queries-on-a-stateful-service-partition).

```xml
    <Service Name="Stateful1" ServiceDnsName="statefulsvc.app" />
      <StatefulService ServiceTypeName="ProcessingType" TargetReplicaSetSize="2" MinReplicaSetSize="2">
        <NamedPartition>
         <Partition Name="partition1" />
         <Partition Name="partition2" />
        </NamedPartition>
      </StatefulService>
    </Service>
```

### <a name="setting-the-dns-name-for-a-service-using-powershell"></a>Powershell kullanarak bir hizmet için DNS adı ayarlama
Bir hizmet için DNS adını kullanarak oluştururken ayarlayabilirsiniz `New-ServiceFabricService` Powershell komutu. Aşağıdaki örnek, DNS adı ile yeni bir durum bilgisi olmayan hizmet oluşturur. `service1.application1`

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

## <a name="preview-making-dns-queries-on-a-stateful-service-partition"></a>[Önizleme] DNS sorguları bir durum bilgisi olan hizmet bölüme yapma
Service Fabric sürümü 6.3 ile başlayarak, Service Fabric DNS hizmeti hizmet bölümleri için sorguları destekler.

DNS sorguları kullanılacak bölümler için aşağıdaki adlandırma kısıtlamaları geçerlidir:

   - Bölüm adları DNS uyumlu olmalıdır.
   - Birden çok etiket bölüm adları (nokta içeren '.', adı) olmamalıdır.
   - Bölüm adları küçük olmalıdır.

Hedef bir bölümünü DNS sorguları aşağıdaki gibi biçimlendirilir:

```
    <First-Label-Of-Partitioned-Service-DNSName><PartitionPrefix><Target-Partition-Name>< PartitionSuffix>.<Remaining- Partitioned-Service-DNSName>
```
Konumlar:

- *First-Label-Of-Partitioned-Service-DNSName* hizmeti DNS adınızı ilk kısmı.
- *PartitionPrefix* DnsService bölümünde gibi küme bildiriminin veya kümenin Resource Manager şablonu aracılığıyla ayarlanan bir değerdir. Varsayılan değer: "-". Daha fazla bilgi için bkz. [DNS hizmeti ayarları](./service-fabric-cluster-fabric-settings.md#dnsservice).
- *Hedef bölüm adı* bölümün adı. 
- *PartitionSuffix* DnsService bölümünde gibi küme bildiriminin veya kümenin Resource Manager şablonu aracılığıyla ayarlanan bir değerdir. Varsayılan değer boş bir dizedir. Daha fazla bilgi için bkz. [DNS hizmeti ayarları](./service-fabric-cluster-fabric-settings.md#dnsservice).
- *Kalan-bölümlenmiş-hizmet-DNSName* hizmeti DNS adınızı kalan parçasıdır.

Aşağıdaki örnekler için varsayılan ayarlara sahip bir kümede çalışan bölümlenmiş Hizmetleri için DNS sorgularını `PartitionPrefix` ve `PartitionSuffix`: 

- Bölüm "0" bir hizmetin DNS adı ile çözümlemek için `backendrangedschemesvc.application` ranged bir bölümleme düzeni kullanan, kullanın `backendrangedschemesvc-0.application`.
- Bölüm çözmek için "first" bir hizmetin DNS adı ile `backendnamedschemesvc.application` kullanın, adlandırılmış bir bölümleme düzeni kullanan `backendnamedschemesvc-first.application`.

DNS hizmetinin birincil çoğaltmasını IP adresini döndürür. Bölüm belirtilirse, hizmeti birincil çoğaltması rastgele seçilen bölüm IP adresini döndürür.

## <a name="using-dns-in-your-services"></a>Hizmetlerinizde DNS kullanma
Birden fazla hizmet dağıtırsanız, uç noktaları ile bir DNS adını kullanarak iletişim kurmak için başka bir hizmetler bulabilirsiniz. Durum bilgisi olmayan hizmetler için ve sürüm 6,3 ve daha sonra durum bilgisi olan hizmetler için Service Fabric DNS hizmeti çalışır. 6\.3 önce Service Fabric sürümlerinde çalışan durum bilgisi olan hizmetler için yerleşik kullanabileceğiniz [ters proxy hizmeti](./service-fabric-reverseproxy.md) belirli hizmet bölüm çağırmak http çağrıları için. 

Dinamik bağlantı noktaları, DNS hizmeti tarafından desteklenmez. Dinamik bağlantı noktalarını kullanacak hizmetlerin çözümlenmesi için ters proxy hizmetini kullanabilirsiniz.

Aşağıdaki kod, DNS üzerinden durum bilgisi olmayan bir hizmeti çağırmak amacıyla gösterilmiştir. Bu yalnızca bir normal http DNS adı, bağlantı noktası ve isteğe bağlı herhangi bir yolu URL'nin bir parçası sağlarsınız çağrısıdır.

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

Aşağıdaki kod, bir durum bilgisi olan hizmet belirli bir bölüme bir çağrı gösterir. Bu durumda, DNS adı (Bölüm1) bölüm adı içeriyor. Bir küme için varsayılan değerlerle çağrı varsayar `PartitionPrefix` ve `PartitionSuffix`.

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
            Uri uri = new Uri("http://service2-partition1.application1:8080/api/values");
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

## <a name="known-issues"></a>Bilinen Sorunlar
* Service Fabric sürümleri 6,3 ve üzeri, DNS adı bir tire içeren hizmet adları için DNS araması bir sorun yoktur. Bu sorun hakkında daha fazla bilgi için lütfen aşağıdaki izleme [GitHub sorunu](https://github.com/Azure/service-fabric-issues/issues/1197). Bunun için bir düzeltme sonraki 6,3 güncelleştirmede kullanıma sunulacaktır. 

* Service Fabric Hizmetleri için DNS hizmeti Linux üzerindeki henüz desteklenmiyor. DNS hizmeti Linux üzerindeki kapsayıcılar için desteklenir. Fabric istemci/ServicePartitionResolver kullanarak el ile çözüm kullanılabilir bir alternatiftir.

## <a name="next-steps"></a>Sonraki adımlar
Hizmet iletişimi ile küme içindeki hakkında daha fazla bilgi [bağlanın ve hizmetlerle iletişim kurma](service-fabric-connect-and-communicate-with-services.md)

