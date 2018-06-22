---
title: Azure Service Fabric ters proxy | Microsoft Docs
description: Service Fabric'ın ters proxy mikro içinden ve dışından küme iletişimi için kullanın.
services: service-fabric
documentationcenter: .net
author: BharatNarasimman
manager: timlt
editor: vturecek
ms.assetid: 47f5c1c1-8fc8-4b80-a081-bc308f3655d3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 11/03/2017
ms.author: bharatn
ms.openlocfilehash: a72873678323d31181654923caf07ba509c9ab81
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36301589"
---
# <a name="reverse-proxy-in-azure-service-fabric"></a>Azure Service Fabric ters proxy
Azure Service Fabric yerleşik ters proxy bulmak ve http uç noktaları olan diğer hizmetleri ile iletişim Service Fabric kümede çalışan mikro yardımcı olur.

## <a name="microservices-communication-model"></a>Mikro iletişim modelini
Service Fabric mikro kümedeki düğümlerin bir alt çalışır ve çeşitli nedenlerle düğümler arasında geçiş yapabilirsiniz. Sonuç olarak, uç noktaları mikro için dinamik olarak değiştirebilirsiniz. Mikro hizmet bulmak ve kümedeki diğer hizmetler ile iletişim kurmak için aşağıdaki adımlarda size gitmeniz gerekir:

1. Adlandırma hizmeti aracılığıyla hizmet konumu çözümleyin.
2. Hizmetine bağlanın.
3. Yukarıdaki adımları hizmet çözümleme uygulayan bir döngüde kaydırma ve bağlantı hataları üzerinde uygulanacak ilkeler yeniden deneyin

Daha fazla bilgi için bkz: [Bağlan ve Hizmetleri ile iletişim](service-fabric-connect-and-communicate-with-services.md).

### <a name="communicating-by-using-the-reverse-proxy"></a>Ters proxy kullanarak iletişim
Ters proxy hizmettir her düğüm üzerinde çalışır ve uç nokta çözümleme işleyen bir otomatik yeniden deneme ve İstemci Hizmetleri adına diğer bağlantı hatası. Ters proxy istemci hizmetleri gelen istekleri işleme gibi çeşitli ilkeleri uygulamak için yapılandırılabilir. Ters Ara sunucu kullanarak istemci hizmetinin tüm istemci-tarafı HTTP iletişim kitaplıkları kullanın ve değil özel çözümlemesini gerektirir ve hizmet mantığı yeniden izin verir. 

Ters proxy diğer hizmetlere istekleri göndermek için kullanılacak İstemci Hizmetleri için yerel düğümünde bir veya daha fazla uç noktalarını kullanıma sunar.

![İç iletişim][1]

> [!NOTE]
> **Desteklenen platformlar**
>
> Service Fabric ters proxy şu anda aşağıdaki platformları destekler
> * *Windows Küme*: Windows 8 ve sonraki veya Windows Server 2012 ve üzeri
> * *Linux kümesi*: Ters Proxy Linux kümeleri için şu anda kullanılamıyor
>

## <a name="reaching-microservices-from-outside-the-cluster"></a>Mikro hizmetler küme dışındaki ulaşmasını
Varsayılan dış iletişim modelini mikro için burada her hizmetin doğrudan dış istemcilerden erişilemez bir katılımı modelidir. [Azure yük dengeleyici](../load-balancer/load-balancer-overview.md), mikro dış istemcileri arasındaki bir ağ sınırında olduğu ağ adresi çevirisi gerçekleştirir ve iç IP: BağlantıNoktası uç noktaları dış isteklerini iletir. Mikro hizmet ait uç nokta dış istemcilere doğrudan erişilebilir olması için ilk küme hizmetinin kullandığı her bağlantı noktası trafik iletmek için yük dengeleyici yapılandırmanız gerekir. Ayrıca, çoğu mikro, özellikle durum bilgisi olan mikro kümenin tüm düğümlerinde dinamik yok. Mikro yük devretme düğümlerinde arasında taşıyabilirsiniz. Böyle durumlarda, yük dengeleyici etkin olduğu trafiği ileterek çoğaltmalarının hedef düğüm konumu belirlenemiyor.

### <a name="reaching-microservices-via-the-reverse-proxy-from-outside-the-cluster"></a>Küme dışında ters proxy sunucudan aracılığıyla mikro ulaşmasını
Yük dengeleyicisi bir bireysel hizmet bağlantı noktasını yapılandırmak yerine, yük dengeleyici yalnızca ters Ara sunucu bağlantı noktasını yapılandırabilirsiniz. Bu yapılandırma, kümenin dışındaki istemcilerin ek yapılandırma olmadan ters proxy kullanarak küme içindeki hizmetlere erişmek sağlar.

![Dış iletişimi][0]

> [!WARNING]
> Yük dengeleyicisi öğesi ters proxy bağlantı noktası yapılandırdığınızda, bir HTTP uç noktası kullanıma tüm mikro kümedeki küme dışında adreslenebilir. Başka bir deyişle, iç olma amacını mikro belirlendiği niyetli bir kullanıcı tarafından bulunabilir olması olabilir. Bu potenially yararlanılabilir ciddi güvenlik açıkları gösterir; Örneğin:
>
> * Kötü niyetli bir kullanıcı, sürekli olarak yeterince sıkı saldırı yüzeyini sahip olmayan bir iç hizmet çağırarak bir hizmet reddi saldırısı başlatabilir.
> * Kötü niyetli bir kullanıcının hatalı biçimlendirilen paketler istenmeyen davranışı kaynaklanan bir iç hizmet sunmak.
> * İç olma amacını bir hizmet, böylece kötü niyetli bir kullanıcı için bu hassas bilgileri gösterme küme dışındaki hizmetlerine açığa çıkarılması amaçlanmamıştır özel ya da hassas bilgi döndürebilir. 
>
> Tam olarak anlamanız ve olası güvenlik ayrımlar kümenizi ve ters proxy bağlantı noktası ortak yapmadan önce bunun üzerinde çalışan uygulamalar için azaltmak emin olun. 
>


## <a name="uri-format-for-addressing-services-by-using-the-reverse-proxy"></a>Ters proxy kullanarak Hizmetleri adresleme için URI biçimi
Ters proxy gelen istek iletilmesi gereken hizmet bölümü tanımlamak için belirli bir Tekdüzen Kaynak Tanımlayıcısı (URI) biçimi kullanır:

```
http(s)://<Cluster FQDN | internal IP>:Port/<ServiceInstanceName>/<Suffix path>?PartitionKey=<key>&PartitionKind=<partitionkind>&ListenerName=<listenerName>&TargetReplicaSelector=<targetReplicaSelector>&Timeout=<timeout_in_seconds>
```

* **http (s):** ters proxy HTTP veya HTTPS trafiğini kabul edecek şekilde yapılandırılabilir. HTTPS iletme için başvurmak [güvenli hizmetine ters proxy ile bağlanma](service-fabric-reverseproxy-configure-secure-communication.md) HTTPS üzerinde dinlemek için ters proxy ayarladıktan sonra.
* **Küme tam etki alanı adı (FQDN) | iç IP:** dış istemcileri için böylece mycluster.eastus.cloudapp.azure.com gibi küme etki alanıyla üzerinden erişilebilen ters proxy yapılandırabilirsiniz. Varsayılan olarak, ters proxy her düğüm üzerinde çalışır. İç trafiği için ters proxy 10.0.0.1 gibi herhangi bir iç düğüm IP'yi veya localhost üzerinde erişilebilir.
* **Bağlantı noktası:** için ters proxy belirtilen gibi bağlantı noktası 19081, budur.
* **ServiceInstanceName:** olmadan Ulaşmaya çalıştığınız dağıtılan hizmet örneği tam adını budur "fabric: /" düzeni. Örneğin, ulaşmak için *fabric: / myapp/myservice/* hizmeti, kullandığınız *myapp/myservice*.

    Hizmet örneği adı büyük/küçük harf duyarlıdır. URL'de hizmet örnek adı için farklı büyük/küçük harf kullanarak 404 ile (bulunamadı) yapılan isteklerin başarısız olmasına neden olur.
* **Sonek yol:** gerçek URL yolu gibi budur *myapi/değerleri/ekleme/3*, bağlanmak istediğiniz hizmeti.
* **PartitionKey:** bölümlenmiş bir hizmet için bu hesaplanan bölüm erişmek istediğiniz bölümün anahtarıdır. Bu Not *değil* bölüm kimliği GUID. Bu parametre tek bölüm düzeni kullanan Hizmetleri için gerekli değildir.
* **PartitionKind:** hizmet bölüm düzeni budur. Bu, 'Int64Range' veya 'Adlı' olabilir. Bu parametre tek bölüm düzeni kullanan Hizmetleri için gerekli değildir.
* **ListenerName** hizmet uç noktaları biçimidir {"Bitiş": {"Listener1": "Bitiş noktası 1", "Listener2": "Endpoint2"...}}. Hizmet birden çok uç nokta gösterir, bu istemci isteği iletilmesi gereken uç nokta tanımlar. Bu hizmet yalnızca bir dinleyici varsa atlanabilir.
* **TargetReplicaSelector** bu hedef çoğaltma veya örnek nasıl seçili belirtir.
  * Hedef hizmet durum bilgisi olan olduğunda TargetReplicaSelector şunlardan biri olabilir: 'PrimaryReplica', 'RandomSecondaryReplica' veya 'RandomReplica'. Bu parametre belirtilmediğinde, varsayılan değer 'PrimaryReplica' dir.
  * Hedef hizmet durum bilgisiz olduğunda ters proxy isteği iletmek için hizmet bölüm rastgele bir örneğini seçer.
* **Zaman aşımı:** bu istemci istek adına hizmeti için ters proxy tarafından oluşturulan HTTP isteği için zaman aşımını belirtir. Varsayılan değer 60 saniyedir. Bu isteğe bağlı bir parametredir.

### <a name="example-usage"></a>Örnek Kullanım
Örnek olarak, atalım *fabric: / MyApp/MyService* aşağıdaki URL'yi bir HTTP dinleyicisini açar hizmeti:

```
http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/
```

Hizmet için kaynakları şunlardır:

* `/index.html`
* `/api/users/<userId>`

Hizmet, bölümleme singleton kullanıyorsa *PartitionKey* ve *PartitionKind* sorgu dizesi parametreleri gerekli değildir ve ağ geçidi olarak kullanarak hizmet erişilebilir:

* Harici olarak: `http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService`
* Dahili olarak: `http://localhost:19081/MyApp/MyService`

Hizmet Tekdüzen Int64 bölümleme düzeni kullanıyorsa *PartitionKey* ve *PartitionKind* sorgu dizesi parametreleri kullanılan, hizmetin bir bölüm ulaşmak için:

* Harici olarak: `http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`
* Dahili olarak: `http://localhost:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`

Hizmet sunan kaynaklara ulaşmak için basitçe URL'de hizmet adından sonra kaynak yolu yerleştir:

* Harici olarak: `http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService/index.html?PartitionKey=3&PartitionKind=Int64Range`
* Dahili olarak: `http://localhost:19081/MyApp/MyService/api/users/6?PartitionKey=3&PartitionKind=Int64Range`

Ağ geçidi, ardından bu istekleri hizmetin URL'sine iletir:

* `http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/index.html`
* `http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/api/users/6`

## <a name="special-handling-for-port-sharing-services"></a>Bağlantı noktası paylaşımı için bir özel işleme Hizmetleri
Service Fabric ters proxy hizmeti adresi yeniden çözün ve bir hizmet erişildiğinde isteği yeniden deneyin dener. Genellikle, ne zaman bir hizmet, farklı bir düğüme, normal yaşam döngüsü kapsamında taşınmış hizmet örneği veya çoğaltma erişilemiyor. Bu gerçekleştiğinde, ters proxy bir uç nokta artık ilk olarak çözümlenmiş adresinde açık olduğunu belirten bir ağ bağlantısı hatası alabilirsiniz.

Ancak, çoğaltmalar veya hizmet örneklerinin bir ana bilgisayar işlemi paylaşabilir ve ayrıca bir http.sys tabanlı bir web sunucusu tarafından barındırılan bir bağlantı noktası paylaşabilen dahil olmak üzere:

* [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener%28v=vs.110%29.aspx)
* [ASP.NET Core WebListener](https://docs.asp.net/latest/fundamentals/servers.html#weblistener)
* [Katana](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.OwinSelfHost/)

Bu durumda, web sunucusunun ana bilgisayar işlemi ve isteklere yanıt kullanılabilir, ancak çözümlenen hizmet örneği ya da çoğaltma artık ana bilgisayarda kullanılabilir değildir. Bu durumda, ağ geçidi web sunucusundan bir HTTP 404 yanıtı alırsınız. Bu nedenle, bir HTTP 404 yanıt iki ayrı anlama sahip olabilir:

- #1: Hizmet adresi doğru durumdur, ancak kullanıcının istenen kaynak yok.
- Durum #2: Hizmet adresi yanlış ve kullanıcının istenen kaynak üzerinde farklı bir düğüme mevcut.

İlk olarak bir normal HTTP kullanıcı hata olarak kabul edilen 404, olur. Ancak, İkinci durumda, mevcut bir kaynak kullanıcı istedi. Ters proxy hizmeti taşınmış olduğundan dosyasını bulamadı. Ters proxy adresini yeniden çözümlemek ve isteği yeniden deneyin gerekir.

Ters proxy, bu nedenle bu iki örnekleri arasında ayrım yapmak için bir yol gerekir. Bu ayrım yapmak için sunucudan bir ipucu gereklidir.

* Varsayılan olarak, ters proxy durum #2 varsayar ve çözümleyin ve isteği yeniden gönderin dener.
* Hizmet durum #1 ters proxy belirtmek için aşağıdaki HTTP yanıt üstbilgisi döndürmesi gerekir:

  `X-ServiceFabric : ResourceNotFound`

Bu bir HTTP yanıt üstbilgisi istenen kaynak yok ve ters proxy hizmeti adresi yeniden çözümlemeyi dener olmayan normal bir HTTP 404 durumu gösterir.

## <a name="setup-and-configuration"></a>Kurulum ve yapılandırma

### <a name="enable-reverse-proxy-via-azure-portal"></a>Azure Portalı aracılığıyla ters proxy etkinleştir

Azure portal, yeni bir Service Fabric kümesi oluşturulurken ters proxy etkinleştirmek için bir seçenek sağlar.
Altında **oluşturma Service Fabric kümesi**, adım 2: küme yapılandırması, düğüm türü yapılandırması için "Ters proxy etkinleştir" onay kutusunu işaretleyin.
Güvenli ters proxy yapılandırmak için SSL sertifikası adım 3'te belirtilebilir: güvenlik, küme güvenlik ayarlarını yapılandırma, sertifika ayrıntılarını girin ve "ters proxy için bir SSL sertifikası içerir" onay kutusunu seçin.

### <a name="enable-reverse-proxy-via-azure-resource-manager-templates"></a>Ters proxy aracılığıyla Azure Resource Manager şablonları etkinleştir

Kullanabileceğiniz [Azure Resource Manager şablonu](service-fabric-cluster-creation-via-arm.md) Service Fabric kümesi için ters proxy etkinleştirmek için.

Başvurmak [HTTPS Ters Proxy Yapılandırma güvenli bir kümede](https://github.com/ChackDan/Service-Fabric/tree/master/ARM Templates/ReverseProxySecureSample#configure-https-reverse-proxy-in-a-secure-cluster) için Azure Resource Manager şablon örnekleri güvenli yapılandırmak için ters proxy ile bir sertifika ve işleme sertifika aktarma.

İlk olarak, dağıtmak istediğiniz küme için Şablon Al. Örnek şablonları kullanabilir veya özel bir Resource Manager şablonu oluşturun. Ardından, aşağıdaki adımları kullanarak ters proxy etkinleştirebilirsiniz:

1. Ters proxy için bir bağlantı noktasını tanımlayın [parametreleri bölümüne](../azure-resource-manager/resource-group-authoring-templates.md) şablonun.

    ```json
    "SFReverseProxyPort": {
        "type": "int",
        "defaultValue": 19081,
        "metadata": {
            "description": "Endpoint for Service Fabric Reverse proxy"
        }
    },
    ```
2. Nodetype nesnelerin her biri için bağlantı noktasını belirtin **küme** [kaynak türü bölümü](../azure-resource-manager/resource-group-authoring-templates.md).

    Bağlantı noktası parametre adı, reverseProxyEndpointPort tanımlanır.

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "reverseProxyEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```
3. Azure küme dışındaki ters proxy sunucudan adres için 1. adımda belirttiğiniz bağlantı noktası için Azure yük dengeleyici kuralları ayarlayın.

    ```json
    {
        "apiVersion": "[variables('lbApiVersion')]",
        "type": "Microsoft.Network/loadBalancers",
        ...
        ...
        "loadBalancingRules": [
            ...
            {
                "name": "LBSFReverseProxyRule",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID0')]"
                    },
                    "backendPort": "[parameters('SFReverseProxyPort')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig0')]"
                    },
                    "frontendPort": "[parameters('SFReverseProxyPort')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[concat(variables('lbID0'),'/probes/SFReverseProxyProbe')]"
                    },
                    "protocol": "tcp"
                }
            }
        ],
        "probes": [
            ...
            {
                "name": "SFReverseProxyProbe",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port":     "[parameters('SFReverseProxyPort')]",
                    "protocol": "tcp"
                }
            }  
        ]
    }
    ```
4. SSL sertifikaları için ters Ara sunucu bağlantı noktasını yapılandırmak için sertifikaya eklemek ***reverseProxyCertificate*** özelliğinde **küme** [kaynak türü bölümü](../resource-group-authoring-templates.md) .

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "reverseProxyCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```

### <a name="supporting-a-reverse-proxy-certificate-thats-different-from-the-cluster-certificate"></a>Küme sertifikadan farklı bir ters proxy sertifikası destekleme
 Ters proxy sertifika küme güvenliğini sağlar sertifikasından farklıysa, ardından daha önce belirtilen sertifika sanal makinede yüklü ve Service Fabric erişebilmesi erişim denetim listesi (ACL) eklenir. Bu yapılabilir **virtualMachineScaleSets** [kaynak türü bölümü](../resource-group-authoring-templates.md). Yükleme için bu sertifika için osProfile ekleyin. Şablon uzantısı bölümünü ACL sertifikada güncelleştirebilirsiniz.

  ```json
  {
    "apiVersion": "[variables('vmssApiVersion')]",
    "type": "Microsoft.Compute/virtualMachineScaleSets",
    ....
      "osProfile": {
          "adminPassword": "[parameters('adminPassword')]",
          "adminUsername": "[parameters('adminUsername')]",
          "computernamePrefix": "[parameters('vmNodeType0Name')]",
          "secrets": [
            {
              "sourceVault": {
                "id": "[parameters('sfReverseProxySourceVaultValue')]"
              },
              "vaultCertificates": [
                {
                  "certificateStore": "[parameters('sfReverseProxyCertificateStoreValue')]",
                  "certificateUrl": "[parameters('sfReverseProxyCertificateUrlValue')]"
                }
              ]
            }
          ]
        }
   ....
   "extensions": [
          {
              "name": "[concat(parameters('vmNodeType0Name'),'_ServiceFabricNode')]",
              "properties": {
                      "type": "ServiceFabricNode",
                      "autoUpgradeMinorVersion": false,
                      ...
                      "publisher": "Microsoft.Azure.ServiceFabric",
                      "settings": {
                        "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
                        "nodeTypeRef": "[parameters('vmNodeType0Name')]",
                        "dataPath": "D:\\\\SvcFab",
                        "durabilityLevel": "Bronze",
                        "testExtension": true,
                        "reverseProxyCertificate": {
                          "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                          "x509StoreName": "[parameters('sfReverseProxyCertificateStoreValue')]"
                        },
                  },
                  "typeHandlerVersion": "1.0"
              }
          },
      ]
    }
  ```
> [!NOTE]
> Var olan bir kümede ters proxy etkinleştirmek için küme sertifikasından farklı sertifikaları kullanırken, ters proxy sertifikasını yükleyin ve ters proxy etkinleştirmeden önce küme üzerinde ACL güncelleştirmesi. Tamamlamak [Azure Resource Manager şablonu](service-fabric-cluster-creation-via-arm.md) bahsedilen ayarlarını kullanarak dağıtım önceden ters proxy etkinleştirmek için bir dağıtım başlamadan önce adımları 1-4.

## <a name="next-steps"></a>Sonraki adımlar
* HTTP iletişimi Hizmetleri'nde arasında örneğine bakın bir [örnek proje github'da](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).
* [Ters proxy ile iletme için Güvenli HTTP hizmeti](service-fabric-reverseproxy-configure-secure-communication.md)
* [Güvenilir hizmetler remoting ile uzak yordam çağrıları](service-fabric-reliable-services-communication-remoting.md)
* [Web API OWIN güvenilir Hizmetleri'nde kullanır](service-fabric-reliable-services-communication-webapi.md)
* [Güvenilir hizmetler kullanarak WCF iletişimi](service-fabric-reliable-services-communication-wcf.md)
* Ek ters proxy yapılandırma seçenekleri için ApplicationGateway/Http bölümüne başvurun [özelleştirme Service Fabric kümesi ayarlarını](service-fabric-cluster-fabric-settings.md).

[0]: ./media/service-fabric-reverseproxy/external-communication.png
[1]: ./media/service-fabric-reverseproxy/internal-communication.png
