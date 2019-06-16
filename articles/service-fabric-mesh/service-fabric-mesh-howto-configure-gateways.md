---
title: Bir ağ geçidi istekleri yapılandırma | Microsoft Docs
description: Service Fabric Mesh üzerinde çalışan, uygulamaları için gelen trafiği işleyen ağ geçidi yapılandırmayı öğrenin.
services: service-fabric-mesh
documentationcenter: .net
author: dkkapur
manager: chakdan
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: azure-cli
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/28/2018
ms.author: dekapur
ms.custom: mvc, devcenter
ms.openlocfilehash: 2e2502e35b3720ddbfe5950b89e2388de378f2ba
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60583650"
---
# <a name="configure-a-gateway-resource-to-route-requests"></a>Bir ağ geçidi kaynağı istekleri yapılandırın

Bir ağ geçidi kaynağı, uygulamanızı barındıran ağa gelen trafiği yönlendirmek için kullanılır. Belirli hizmetler veya istek yapısına dayanarak uç noktaları üzerinden isteklerinin yönlendirildiği kurallarını belirtmek için yapılandırın. Bkz: [Service Fabric Mesh ağ giriş](service-fabric-mesh-networks-and-gateways.md) ağlar ve ağ, ağ geçitleri hakkında daha fazla bilgi için. 

Ağ geçidi kaynakları, dağıtım şablon (JSON veya yaml) bir parçası olarak bildirilmesi gerekir ve bir ağ kaynağına bağlıdır. Bu belge, ağ geçidiniz için ayarlanabilir ve bir örnek ağ geçidi yapılandırması kapsayan çeşitli özellikler özetlenmektedir.

## <a name="options-for-configuring-your-gateway-resource"></a>Ağ geçidi kaynak yapılandırma seçenekleri

Ağ geçidi kaynağı uygulamanızın ağ ve temel alınan altyapının ağ arasında bir köprü görevi görür bu yana ( `open` ağ). Yalnızca bir yapılandırma gerekir (kafes Önizleme sürümünde mevcuttur uygulama başına bir ağ geçidi sınırı). Kaynak bildirimi iki ana bölümden oluşur: Kaynak meta veriler ve özellikler. 

### <a name="gateway-resource-metadata"></a>Ağ geçidi kaynak meta verileri

Bir ağ geçidi, aşağıdaki meta verileri ile bildirilir:
* `apiVersion` -ayarlanması için "2018-09-01-preview" (veya daha sonra gelecekte) gerekir
* `name` -Bu ağ geçidi için bir dize adı
* `type` -"Microsoft.ServiceFabricMesh/gateways"
* `location` -Uygulamanızı konumunu ayarlama / ağ olmalıdır; Genellikle, dağıtımınızdaki konum parametresi için bir başvuru olacaktır
* `dependsOn` -kendisi için bu ağ geçidi olarak hizmet edecektir bir giriş noktası için ağ

Bir Azure Resource Manager'ı (JSON) dağıtım şablonunun nasıl göründüğüne aşağıda verilmiştir: 

```json
{
  "apiVersion": "2018-09-01-preview",
  "name": "myGateway",
  "type": "Microsoft.ServiceFabricMesh/gateways",
  "location": "[parameters('location')]",
  "dependsOn": [
    "Microsoft.ServiceFabricMesh/networks/myNetwork"
  ],
  "properties": {
    [...]
  }
}
```

### <a name="gateway-properties"></a>Ağ geçidi özellikleri

Özellikler bölümü, ağları arasında ağ geçidi arasındadır ve yönlendirme istekleri için kurallar tanımlamak için kullanılır. 

#### <a name="source-and-destination-network"></a>Kaynak ve hedef ağ 

Her ağ geçidi gerektirir bir `sourceNetwork` ve `destinationNetwork`. Kaynak ağı, uygulamanızın gelen istekler alacağı ağda şu şekilde tanımlanır. Kendi ad özelliği her zaman "Açık" olarak ayarlanmalıdır. Hedef ağ istekleri hedeflediğiniz ağdır. Ad değeri bu kaynak adı, uygulamanızın yerel ağ (kaynak tam başvuru içermelidir) olarak ayarlanmalıdır. Bu bir dağıtımda "myNetwork" adlı bir ağ için nasıl göründüğünü bir örnek yapılandırma için aşağıya bakın.

```json 
"properties": {
  "description": "Service Fabric Mesh Gateway",
  "sourceNetwork": {
    "name": "Open"
  },
  "destinationNetwork": {
    "name": "[resourceId('Microsoft.ServiceFabricMesh/networks', 'myNetwork')]"
  },
  [...]
}
```

#### <a name="rules"></a>Kurallar 

Bir ağ geçidi, birden fazla yönlendirme olabilir nasıl gelen trafiği belirten kuralları işlenecek. Yönlendirme kuralı dinleme bağlantı noktası ve hedef uç nokta için belirli bir uygulama arasındaki ilişkiyi tanımlar. TCP için yönlendirme kuralları, bağlantı noktası: uç nokta arasında 1:1 eşleme vardır. HTTP yönlendirme kuralları, talep ve isteğe bağlı üst bilgiler, isteğin yönlendirildiği nasıl karar vermek için yolunu inceleyin daha karmaşık yönlendirme kuralları ayarlayabilirsiniz. 

Yönlendirme kuralları, bir bağlantı noktası başına temelinde belirtilir. Her giriş bağlantı noktası, ağ geçidi yapılandırması özellikler bölümü, kendi kuralları dizisi vardır. 

#### <a name="tcp-routing-rules"></a>TCP yönlendirme kuralları 

TCP yönlendirme kuralını aşağıdaki özellikleri içerir: 
* `name` -tercih ettiğiniz herhangi bir dize olabilir kural başvurusu 
* `port` -gelen istekler için Dinlemenin yapılacağı bağlantı noktası 
* `destination` -içeren uç noktası belirtimi `applicationName`, `serviceName`, ve `endpointName`, istekleri için yönlendirilmesi gereken yeri için

Bir örnek TCP yönlendirme kuralı şu şekildedir:

```json
"properties": {
  [...]
  "tcp": [
    {
      "name": "web",
      "port": 80,
      "destination": {
        "applicationName": "myApp",
        "serviceName": "myService",
        "endpointName": "myListener"
      }
    }
  ]
}
```


#### <a name="http-routing-rules"></a>HTTP yönlendirme kuralları 

HTTP yönlendirme kuralını aşağıdaki özellikleri içerir: 
* `name` -tercih ettiğiniz herhangi bir dize olabilir kural başvurusu 
* `port` -gelen istekler için Dinlemenin yapılacağı bağlantı noktası 
* `hosts` -bir dizi çeşitli "ana bilgisayarlar için" yukarıda belirtilen bağlantı noktasında gelen isteklere uygulanan ilkeler. Konaklar için uygulamalar ve hizmetler ağda çalışan ve gelen istekler, yani bir web uygulaması hizmet verebilen yer alır. Ana bilgisayar ilkeleri belirginliğe düzeylerini azalan sırada aşağıdaki oluşturmanız sırayla yorumlanır
    * `name` -Aşağıdaki yönlendirme kurallarını belirtilir ana bilgisayar DNS adı. Kullanarak "*" tüm konaklar için yönlendirme kuralları burada oluşturursunuz.
    * `routes` -ilkeleri bu belirli ana bilgisayar için bir dizi
        * `match` -belirtimi gelen isteği yapısı uygulamak bu kural için temel bir `path`
            * `path` -içeren bir `value` (gelen URI) `rewrite` (iletilecek istek istediğiniz) ve `type` (şu an için yalnızca "Ön eki" olabilir.)
            * `header` -İsteğe bağlı bir dizi olması durumunda isteğinin üst bilgisinde eşleştirilecek üstbilgi değerlerini istek yolunu (yukarıda) eşleşir.
              * Her bir giriş içeriyor `name` (dize adı) eşleştirilecek üstbilginin `value` (istek üstbilgisinin değerini string) ve bir `type` (şu an için yalnızca "Tam" olabilir)
        * `destination` -İstek eşleşiyorsa kullanarak belirtilen bu hedefe yönlendirileceğini bir `applicationName`, `serviceName`, ve `endpointName`

Bu ağdaki uygulamaları tarafından sunulan tüm Konaklara bağlantı noktası 80, gelen istekleri için uygulanacak bir örnek HTTP yönlendirme kuralı aşağıda verilmiştir. İstek URL'si, yol belirtimi, yani, eşleşen bir yapısı varsa `<IPAddress>:80/pickme/<requestContent>`, sonra da üzere yönlendirilirsiniz `myListener` uç noktası.  

```json
"properties": {
  [...]
  "http": [
    {
      "name": "web",
      "port": 80,
      "hosts": [
        {
          "name": "*",
          "routes": [
            {
              "match": {
                "path": {
                  "value": "/pickme",
                  "rewrite": "/",
                  "type": "Prefix"
                }
              },
              "destination": {
                "applicationName": "meshApp",
                "serviceName": "myService",
                "endpointName": "myListener"
              }
            }
          ]
        }
      ]
    }
  ]
}
```

## <a name="sample-config-for-a-gateway-resource"></a>Bir ağ geçidi kaynağı için örnek yapılandırma 

Tam bir ağ geçidi kaynak yapılandırma gibi görünür (Bu bulunan giriş örnekten uyarlanmış [kafes örnekleri deposu](https://github.com/Azure-Samples/service-fabric-mesh/blob/2018-09-01-preview/templates/ingress/meshingress.linux.json)):

```json
{
  "apiVersion": "2018-09-01-preview",
  "name": "ingressGatewayLinux",
  "type": "Microsoft.ServiceFabricMesh/gateways",
  "location": "[parameters('location')]",
  "dependsOn": [
    "Microsoft.ServiceFabricMesh/networks/meshNetworkLinux"
  ],
  "properties": {
    "description": "Service Fabric Mesh Gateway for Linux mesh samples.",
    "sourceNetwork": {
      "name": "Open"
    },
    "destinationNetwork": {
      "name": "[resourceId('Microsoft.ServiceFabricMesh/networks', 'meshNetworkLinux')]"
    },
    "http": [
      {
        "name": "web",
        "port": 80,
        "hosts": [
          {
            "name": "*",
            "routes": [
              {
                "match": {
                  "path": {
                    "value": "/hello",
                    "rewrite": "/",
                    "type": "Prefix"
                  }
                },
                "destination": {
                  "applicationName": "meshAppLinux",
                  "serviceName": "helloWorldService",
                  "endpointName": "helloWorldListener"
                }
              },
              {
                "match": {
                  "path": {
                    "value": "/counter",
                    "rewrite": "/",
                    "type": "Prefix"
                  }
                },
                "destination": {
                  "applicationName": "meshAppLinux",
                  "serviceName": "counterService",
                  "endpointName": "counterServiceListener"
                }
              }
            ]
          }
        ]
      }
    ]
  }
}
```

Bu ağ geçidi, bir Linux uygulaması, "" helloWorldService"ve 80 numaralı bağlantı noktasında dinler" counterService"olmak üzere en az iki hizmet oluşan meshAppLinux" için yapılandırılır. Gelen istek URL'si yapısını bağlı olarak, bu isteği bu hizmetlerden biri için yönlendirir. 
* "\<IPADDRESS >: 80/helloWorld/\<isteği\>" helloWorldService "helloWorldListener" yönlendirilmesi isteğinde neden olur. 
* "\<IPADDRESS >: 80/sayaç/\<isteği\>" counterService "counterListener" yönlendirilmesi isteğinde neden olur. 

## <a name="next-steps"></a>Sonraki adımlar
* Dağıtma [giriş örnek](https://github.com/Azure-Samples/service-fabric-mesh/tree/2018-09-01-preview/templates/ingress) ağ geçitleri iş başında görmek için
