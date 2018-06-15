---
title: Azure Service Fabric tek başına kümenizi yapılandırmak | Microsoft Docs
description: Tek başına veya şirket içi Azure Service Fabric kümenizi yapılandırmayı öğrenin.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: 0c5ec720-8f70-40bd-9f86-cd07b84a219d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/06/2017
ms.author: dekapur
ms.openlocfilehash: e0fed608ac9dd02a6fe5563eefc30edb63d224b1
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34205374"
---
# <a name="configuration-settings-for-a-standalone-windows-cluster"></a>Tek başına Windows kümesi için yapılandırma ayarları
Bu makalede, bir tek başına Azure Service Fabric kümesi ClusterConfig.json dosyasını kullanarak yapılandırmak açıklar. Küme düğümleri, güvenlik yapılandırmalarını yanı sıra, hata ve yükseltme etki alanları bakımından ağ topolojisi hakkında bilgi belirtmek için bu dosyayı kullanır.

Olduğunda, [tek başına Service Fabric paketini karşıdan](service-fabric-cluster-creation-for-windows-server.md#downloadpackage), ClusterConfig.json örnekleri dahil de. "DevCluster" adlarında sahip örnekleri mantıksal düğümleri kullanarak aynı makine üzerindeki tüm üç düğümü olan bir küme oluşturun. Bu düğümler dışında en az bir birincil düğüm olarak işaretlenmesi gerekir. Bu tür bir küme, geliştirme veya test ortamları için yararlıdır. Bir üretim kümesi olarak desteklenmiyor. "MultiMachine" adlarında sahip örnekleri üretim düzeyde kümeler, her bir düğümde ayrı bir makine oluşturmak yardımcı olur. Küme üzerinde bu kümeleri için birincil düğüm sayısına dayalı olarak [güvenilirlik düzeyi](#reliability). Sürüm 5.7, API sürümü 05-2017, güvenilirlik düzeyi özelliği kaldırıldı. Bunun yerine, kodumuza, kümeniz için en iyileştirilmiş güvenilirlik düzeyi hesaplar. Sürümlerde 5.7 veya sonraki sürümleri bu özellik için bir değer ayarlamak çalışmayın.


* ClusterConfig.Unsecure.DevCluster.json ve ClusterConfig.Unsecure.MultiMachine.json güvenli test veya üretim kümesi sırasıyla oluşturmayı gösterir.

* ClusterConfig.Windows.DevCluster.json ve ClusterConfig.Windows.MultiMachine.json Göster kullanılarak güvenli hale getirilir test veya üretim kümeleri oluşturmak nasıl [Windows Güvenliği](service-fabric-windows-cluster-windows-security.md).

* ClusterConfig.X509.DevCluster.json ve ClusterConfig.X509.MultiMachine.json Göster kullanılarak güvenli hale getirilir test veya üretim kümeleri oluşturmak nasıl [X509 sertifika tabanlı güvenlik](service-fabric-windows-cluster-x509-security.md).

Şimdi ClusterConfig.json dosya çeşitli bölümlerini inceleyelim.

## <a name="general-cluster-configurations"></a>Genel küme yapılandırmaları
Genel küme yapılandırmaları aşağıdaki JSON parçacığında gösterildiği gibi geniş kümeye özgü yapılandırmaları kapsar:

```json
    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "01-2017",
```

Adı değişkenine atayarak, herhangi bir kolay ad, Service Fabric kümesi verebilirsiniz. ClusterConfigurationVersion kümenizin sürüm numarasıdır. Service Fabric kümesi yükseltme her zaman artırın. ApiVersion kümesi için varsayılan değeri bırakın.

## <a name="nodes-on-the-cluster"></a>Küme düğümlerinde

    <a id="clusternodes"></a>

Aşağıdaki kod parçacığında gösterildiği gibi düğümler bölümünü kullanarak, Service Fabric kümesi düğümleri yapılandırabilirsiniz:

    "nodes": [{
        "nodeName": "vm0",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType1",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType2",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],

Service Fabric kümesi en az üç düğümü içermesi gerekir. Bu bölümde kurulumunuzu göre daha fazla düğüm ekleyebilirsiniz. Aşağıdaki tabloda her düğüm için yapılandırma ayarları açıklanmaktadır:

| **Düğüm yapılandırması** | **Açıklama** |
| --- | --- |
| nodeName |Düğüme kolay adı verebilirsiniz. |
| IP adresi |Bir komut penceresi açıp yazarak, düğümün IP adresini bulmak `ipconfig`. IPv4 adresini not alın ve IPADDRESS değişkenine atayın. |
| nodeTypeRef |Her düğüm farklı düğüm türü atanabilir. [Düğüm türleri](#node-types) aşağıdaki bölümünde tanımlanır. |
| faultDomain |Hata etki alanı küme yöneticileri aynı zamanda paylaşılan fiziksel bağımlılıkları nedeniyle başarısız olabilir fiziksel düğümlerin tanımlamak etkinleştirin. |
| upgradeDomain |Yükseltme etki alanlarının yaklaşık aynı zamanda Service Fabric yükseltmelerinin kapatıldığından düğüm kümesi açıklanmaktadır. Fiziksel gereksinimlere göre sınırlı değildir çünkü hangi yükseltme etki alanlarının atamak için hangi düğümlerin seçebilirsiniz. |

## <a name="cluster-properties"></a>Küme Özellikleri
ClusterConfig.json özellikleri bölümünde gösterildiği gibi küme yapılandırmak için kullanılır:

### <a name="reliability"></a>Güvenilirlik
Yineleme sayısı veya birincil küme düğümlerinde çalıştırabilirsiniz Service Fabric sistem hizmet örneklerinin reliabilityLevel kavramı tanımlar. Bu hizmetler güvenilirliğini belirler ve bu nedenle küme. Değer, küme oluşturma ve yükseltme aynı anda sistem tarafından hesaplanır.

    <a id="reliability"></a>

### <a name="diagnostics"></a>Tanılama
DiagnosticsStore bölümünde, tanılama ve aşağıdaki kod parçacığında gösterildiği gibi düğümü veya küme hataları giderme parametrelerini yapılandırabilirsiniz: 

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
    }

Meta verileri, küme tanılama açıklamasını ve kurulumunuzu göre ayarlanabilir. Bu değişkenler ETW İzleme günlükleri toplama yardımcı olur ve performans sayaçları yanı sıra kilitlenme dökümleri. ETW İzleme günlükleri hakkında daha fazla bilgi için bkz: [işaretlenemedi](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) ve [ETW İzleme](https://msdn.microsoft.com/library/ms751538.aspx). Tüm günlükleri de dahil olmak üzere, [kilitlenme dökümleri](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) ve [performans sayaçları](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx), makinenizde connectionString klasöre yönlendirilebilir. AzureStorage, tanılama depolamak için de kullanabilirsiniz. Aşağıdaki örnek kod parçacığını bakın:

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "AzureStorage",
        "IsEncrypted": "false",
        "connectionstring": "xstore:DefaultEndpointsProtocol=https;AccountName=[AzureAccountName];AccountKey=[AzureAccountKey]"
    }

### <a name="security"></a>Güvenlik
Güvenli tek başına Service Fabric kümesi için gerekli güvenlik bölümüdür. Aşağıdaki kod parçacığında bu bölümün parçası gösterir:

    "security": {
        "metadata": "This cluster is secured using X509 certificates.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        . . .
    }

Meta verileri güvenli kümenizi açıklamasını ve kurulumunuzu göre ayarlanabilir. ClusterCredentialType ve ServerCredentialType küme ve düğümler uygulayan güvenlik türünü belirler. Bunlar için her iki ayarlanabilir *X509* sertifika tabanlı bir güvenlik veya *Windows* Azure Active Directory tabanlı güvenlik için. Güvenlik bölümüne geri kalanı güvenlik türüne bağlıdır. Kalan güvenlik bölümüne doldurun hakkında daha fazla bilgi için bkz: [tek başına kümede güvenlik tabanlı sertifikalar](service-fabric-windows-cluster-x509-security.md) veya [tek başına kümede Windows Güvenlik](service-fabric-windows-cluster-windows-security.md).

### <a name="node-types"></a>Düğüm türleri

    <a id="nodetypes"></a>

NodeTypes bölüm kümenizi sahip düğümleri türünü açıklar. En az bir düğüm türü bir küme için aşağıdaki kod parçacığında gösterildiği gibi belirtilmesi gerekir: 

    "nodeTypes": [{
        "name": "NodeType0",
        "clientConnectionEndpointPort": "19000",
        "clusterConnectionEndpointPort": "19001",
        "leaseDriverEndpointPort": "19002"
        "serviceConnectionEndpointPort": "19003",
        "httpGatewayEndpointPort": "19080",
        "reverseProxyEndpointPort": "19081",
        "applicationPorts": {
            "startPort": "20575",
            "endPort": "20605"
        },
        "ephemeralPorts": {
            "startPort": "20606",
            "endPort": "20861"
        },
        "isPrimary": true
    }]

Bu belirli düğüm türü için kolay ad adıdır. Bu düğüm türünde bir düğüm oluşturmak için olarak bu düğüm için kolay adı nodeTypeRef değişkenine atayın [daha önce bahsedilen](#nodes-on-the-cluster). Her düğüm türü için kullanılan bağlantı uç tanımlayın. Bu kümedeki başka bir uç nokta ile çakışmayacak sürece, bu bağlantı uç noktaları için herhangi bir bağlantı noktası numarası seçebilirsiniz. Birden çok düğümlü bir kümede bir veya daha fazla birincil düğüm vardır (diğer bir deyişle, isPrimary ayarlanır *true*) bağlı olarak [reliabilityLevel](#reliability). Birincil ve nonprimary düğüm türleri hakkında daha fazla bilgi için bkz: [Service Fabric kümesi kapasite planlama konuları](service-fabric-cluster-capacity.md) nodeTypes ve reliabilityLevel hakkında bilgi. 

#### <a name="endpoints-used-to-configure-the-node-types"></a>Düğüm türleri yapılandırmak için kullanılan uç noktaları
* clientConnectionEndpointPort istemci API kullanıldığında kümeye bağlanmak için istemci tarafından kullanılan bağlantı noktasıdır. 
* clusterConnectionEndpointPort, düğümleri birbirleri ile iletişim bağlantı noktasıdır.
* leaseDriverEndpointPort düğümleri hala etkin olup olmadığını öğrenmek için küme kira sürücüsü tarafından kullanılan bağlantı noktasıdır. 
* serviceConnectionEndpointPort bu belirli düğüm üzerindeki Service Fabric istemcisi ile iletişim kurmak için bir düğümde dağıtılan hizmetler ve uygulamalar tarafından kullanılan bağlantı noktasıdır.
* httpGatewayEndpointPort kümeye bağlanmak için Service Fabric Explorer tarafından kullanılan bağlantı noktasıdır.
* ephemeralPorts geçersiz kılma [işletim sistemi tarafından kullanılan dinamik bağlantı noktaları](https://support.microsoft.com/kb/929851). Service Fabric Bu bağlantı noktaları bir parçası uygulama bağlantı noktalarını kullanır ve kalan işletim sistemi için kullanılabilir. Tüm amaçlar için örnek JSON dosyalarında verilen aralıklar kullanabilmeniz için Ayrıca bu aralık işletim sisteminde mevcut mevcut aralığı eşler. Başlangıç ve bitiş bağlantı noktaları arasındaki farkı en az 255 olduğundan emin olun. Bu aralık işletim sistemiyle paylaşıldığından bu fark, düşükse çakışmaları çalıştırabilirsiniz. Yapılandırılmış dinamik bağlantı noktası aralığını görmek için çalıştırın `netsh int ipv4 show dynamicport tcp`.
* applicationPorts Service Fabric uygulamaları tarafından kullanılan bağlantı noktalarıdır. Uygulama bağlantı noktası aralığı, uygulamalarınızın uç nokta gereksinimi karşılamak için yeterli büyüklükte olması gerekir. Bu aralık makinede, diğer bir deyişle, ephemeralPorts aralık yapılandırma kümesinde olarak dinamik bağlantı noktası aralığından özel olması gerekir. Service Fabric yeni bağlantı noktaları gerekli olduğunda bu bağlantı noktaları kullanır ve bu bağlantı noktaları için güvenlik duvarını açma mvc'deki. 
* reverseProxyEndpointPort bir isteğe bağlı ters proxy uç noktadır. Daha fazla bilgi için bkz: [Service Fabric ters proxy](service-fabric-reverseproxy.md). 

### <a name="log-settings"></a>Günlük ayarları
FabricSettings bölümünde kök dizinler Service Fabric veri ve günlükleri için ayarlayabilirsiniz. Yalnızca ilk küme oluşturma sırasında bu dizinleri özelleştirebilirsiniz. Aşağıdaki örnek kod parçacığında, bu bölümün bakın:

    "fabricSettings": [{
        "name": "Setup",
        "parameters": [{
            "name": "FabricDataRoot",
            "value": "C:\\ProgramData\\SF"
        }, {
            "name": "FabricLogRoot",
            "value": "C:\\ProgramData\\SF\\Log"
    }]

FabricDataRoot ve FabricLogRoot bir işletim sistemi olmayan sürücü kullanmanızı öneririz. İşletim Sisteminin yanıt vermediğinde durumları önleme, daha fazla güvenilirlik sağlar. Yalnızca veri kök özelleştirirseniz günlük kök veri kökü altındaki bir düzey yerleştirilir.

### <a name="stateful-reliable-services-settings"></a>Durum bilgisi olan güvenilir hizmetler ayarları
KtlLogger bölümünde güvenilir hizmetler için genel yapılandırma ayarları ayarlayabilirsiniz. Bu ayarlar hakkında daha fazla bilgi için bkz: [durum bilgisi olan güvenilir hizmetler yapılandırma](service-fabric-reliable-services-configuration.md). Aşağıdaki örnek, durum bilgisi olan hizmetler için herhangi bir güvenilir koleksiyonu yedeklemek için oluşturulan paylaşılan işlem günlüğü değiştirmek gösterilmektedir:

    "fabricSettings": [{
        "name": "KtlLogger",
        "parameters": [{
            "name": "SharedLogSizeInMB",
            "value": "4096"
        }]
    }]

### <a name="add-on-features"></a>Eklenti Özellikleri
Eklenti özelliklerini yapılandırmak için 04 2017 olarak ya da daha yüksek apiVersion yapılandırın ve addonFeatures aşağıda gösterildiği gibi yapılandırın:

    "apiVersion": "04-2017",
    "properties": {
      "addOnFeatures": [
          "DnsService",
          "RepairManager"
      ]
    }

### <a name="container-support"></a>Kapsayıcı desteği
Windows Server kapsayıcıları ve tek başına kümeleri için Hyper-V kapsayıcı kapsayıcı desteğini etkinleştirmek için DnsService eklenti özelliği etkinleştirilmelidir.


## <a name="next-steps"></a>Sonraki adımlar
Tek başına küme kurulumunuzu göre yapılandırılmış bir tam ClusterConfig.json dosyanız olduktan sonra kümenizi dağıtabilirsiniz. Adımları [bir tek başına Service Fabric kümesi oluştur](service-fabric-cluster-creation-for-windows-server.md). İle devam [Service Fabric Explorer ile kümenizi görselleştirme](service-fabric-visualizing-your-cluster.md) adımları izleyin.

