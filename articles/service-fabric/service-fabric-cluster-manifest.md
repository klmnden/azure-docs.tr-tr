---
title: Azure Service Fabric tek başına kümenizi yapılandırma | Microsoft Docs
description: Tek başına veya şirket içi Azure Service Fabric kümenizi yapılandırmayı öğrenin.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: chackdan
editor: ''
ms.assetid: 0c5ec720-8f70-40bd-9f86-cd07b84a219d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/12/2018
ms.author: dekapur
ms.openlocfilehash: f94a65e469fdb3cee4f02bc5a8f6f5a4a1ea5a16
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60386729"
---
# <a name="configuration-settings-for-a-standalone-windows-cluster"></a>Tek başına bir Windows kümesi için yapılandırma ayarları
Bu makalede bir tek başına Azure Service Fabric kümesinin ayarlanabilir yapılandırma ayarlarını *ClusterConfig.json* dosya. Bu dosya, hata ve yükseltme etki alanlarında bakımından ağ topolojisini yanı sıra küme düğümleri, güvenlik yapılandırmaları hakkında bilgi belirtmek için kullanır.  Değiştirme veya yapılandırma ayarı da ekledikten sonra yapabilirsiniz veya [tek başına küme oluşturma](service-fabric-cluster-creation-for-windows-server.md) veya [tek başına küme yapılandırmasını yükseltme](service-fabric-cluster-config-upgrade-windows-server.md).

Olduğunda, [tek başına Service Fabric paketini indirme](service-fabric-cluster-creation-for-windows-server.md#downloadpackage), ClusterConfig.json örnekleri da dahil edilir. "DevCluster", adlarında örnekleri mantıksal düğümleri kullanarak aynı makinede üç tüm düğümlerle bir küme oluşturun. Bu düğümler sırasız; en az bir birincil düğüm olarak işaretlenmelidir. Bu tür bir küme, geliştirme ve test ortamları için kullanışlıdır. Bir üretim kümesi olarak desteklenmiyor. "MultiMachine", adlarında örnekleri, üretim her düğümü ayrı bir makineye sınıf kümelerini oluşturmak yardımcı olur. Bu küme için birincil düğüm sayısını kümenin üzerinde alan [güvenilirlik düzeyi](#reliability). Sürümde 5.7, API Sürüm 2017-05, güvenilirlik düzeyi özelliği kaldırdık. Bunun yerine, kümeniz için en iyileştirilmiş güvenilirlik düzeyi kodumuz hesaplar. Ve üzeri sürümleri 5.7 içinde bu özellik için bir değer ayarlamaya çalışmayın.

* ClusterConfig.Unsecure.DevCluster.json ve ClusterConfig.Unsecure.MultiMachine.json sırasıyla bir güvenli olmayan test veya üretim küme oluşturma işlemini göstermektedir.

* ClusterConfig.Windows.DevCluster.json ve ClusterConfig.Windows.MultiMachine.json Göster kullanarak güvenli bir test veya üretim kümeleri oluşturma işlemini [Windows Güvenlik](service-fabric-windows-cluster-windows-security.md).

* ClusterConfig.X509.DevCluster.json ve ClusterConfig.X509.MultiMachine.json Göster kullanarak güvenli bir test veya üretim kümeleri oluşturma işlemini [X509 sertifika tabanlı güvenlik](service-fabric-windows-cluster-x509-security.md).

Artık bir ClusterConfig.json dosyasının çeşitli bölümlerini inceleyelim.

## <a name="general-cluster-configurations"></a>Genel küme yapılandırmaları
Genel küme yapılandırmaları, aşağıdaki JSON kod parçacığında gösterildiği gibi geniş bir kümeye özgü yapılandırmaları kapsar:

```json
    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "01-2017",
```

Service Fabric kümenize adı değişkene atayarak, herhangi bir kolay ad verebilirsiniz. ClusterConfigurationVersion kümenizin sürüm numarasıdır. Service Fabric kümenizi yükseltmek her zaman bu artırın. ApiVersion varsayılan değeri bırakın.

## <a name="nodes-on-the-cluster"></a>Kümedeki düğümlerden
Aşağıdaki kod parçacığında gösterildiği gibi düğümler bölümünde kullanarak Service Fabric kümenizde düğümleri yapılandırabilirsiniz:
```json
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
```

Service Fabric kümesi en az üç düğüm içermelidir. Kurulumunuza göre bu bölüme, daha fazla düğüm ekleyebilirsiniz. Aşağıdaki tabloda, her düğüm için yapılandırma ayarları açıklanmaktadır:

| **Düğüm yapılandırması** | **Açıklama** |
| --- | --- |
| nodeName |Düğüme herhangi bir kolay ad verebilirsiniz. |
| IP adresi |Bir komut penceresi açıp yazarak, düğümün IP adresini bulmanın `ipconfig`. IPv4 adresini not alın ve IPADDRESS değişkene atayın. |
| nodeTypeRef |Her düğüm, farklı düğüm türü atanabilir. [Düğüm türleri](#node-types) aşağıdaki bölümde tanımlanır. |
| faultDomain |Hata etki alanları küme yöneticileri aynı zamanda paylaşılan fiziksel bağımlılıklar nedeniyle başarısız olabilir fiziksel düğümde tanımlamak etkinleştirin. |
| upgradeDomain |Yükseltme etki alanları, yaklaşık aynı zamanda Service Fabric yükseltmelerinin kapatıldığından düğüm kümesini açıklar. Bunlar tüm fiziksel gereksinimler ile sınırlı değildir çünkü hangi yükseltme etki alanları atamak için hangi düğümleri seçebilirsiniz. |

## <a name="cluster-properties"></a>Küme Özellikleri
ClusterConfig.json özellikleri bölümünde gösterildiği gibi küme yapılandırmak için kullanılır:

### <a name="reliability"></a>Güvenilirlik
Yineleme sayısı veya birincil küme düğümler üzerinde çalışan Service Fabric sistem hizmetlerinin örnekleri düzeyinden kavramını tanımlar. Bu hizmetlerin güvenilirliğini belirler ve bu nedenle küme. Değer, küme oluşturma ve yükseltme anda sistem tarafından hesaplanır.

### <a name="diagnostics"></a>Tanılama
DiagnosticsStore bölümünde, tanılama ve düğümünün veya kümenin, aşağıdaki kod parçacığında gösterildiği gibi sorunlarını giderme parametrelerini yapılandırabilirsiniz: 

```json
"diagnosticsStore": {
    "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
    "dataDeletionAgeInDays": "7",
    "storeType": "FileShare",
    "IsEncrypted": "false",
    "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
}
```

Meta veri kümesi tanılama açıklamasıdır ve kurulumunuza göre ayarlanabilir. Bu değişkenler ETW İzleme günlüklerini toplama Yardım ve kilitlenme dökümleri yanı sıra performans sayaçları. ETW İzleme günlüklerini hakkında daha fazla bilgi için bkz. [Tracelog](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) ve [ETW İzleme](https://msdn.microsoft.com/library/ms751538.aspx). Tüm günlükler de dahil olmak üzere, [kilitlenme dökümleri](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) ve [performans sayaçları](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx), makinenizde connectionString klasörüne yönlendirilebilir. AzureStorage, tanılama verilerini depolamak için de kullanabilirsiniz. Aşağıdaki örnek kod parçacığına bakın:

```json
"diagnosticsStore": {
    "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
    "dataDeletionAgeInDays": "7",
    "storeType": "AzureStorage",
    "IsEncrypted": "false",
    "connectionstring": "xstore:DefaultEndpointsProtocol=https;AccountName=[AzureAccountName];AccountKey=[AzureAccountKey]"
}
```

### <a name="security"></a>Güvenlik
Güvenlik bölümüne güvenli tek başına Service Fabric kümesi için gereklidir. Aşağıdaki kod parçacığı, bu bölümde bir parçası gösterir:

```json
"security": {
    "metadata": "This cluster is secured using X509 certificates.",
    "ClusterCredentialType": "X509",
    "ServerCredentialType": "X509",
    . . .
}
```

Meta verileri güvenli kümenize açıklamasını ve kurulumunuza göre ayarlanabilir. ClusterCredentialType ve ServerCredentialType küme ve düğümleri uygulayan güvenlik türünü belirler. Bunlar için her iki ayarlanabilir *X509* sertifika tabanlı bir güvenlik veya *Windows* Azure Active Directory tabanlı güvenlik için. Güvenlik bölümün geri kalanında güvenlik türüne bağlıdır. Kalan güvenlik bölümüne doldurun hakkında daha fazla bilgi için bkz: [tek başına küme sertifikaları tabanlı güvenlik](service-fabric-windows-cluster-x509-security.md) veya [tek başına küme Windows Güvenlik](service-fabric-windows-cluster-windows-security.md).

### <a name="node-types"></a>Düğüm türleri
NodeType bölüm kümenizi sahip düğüm türünü açıklar. En az bir düğüm türü, aşağıdaki kod parçacığında gösterildiği gibi bir küme için belirtilmelidir: 

```json
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
```

Bu belirli düğüm türü için kolay ad adıdır. Bu düğüm türü, bir düğüm oluşturmak için farklı bu düğüm için nodeTypeRef değişkenine, kolay ad Ata [daha önce bahsedilen](#nodes-on-the-cluster). Her düğüm türü için kullanılan bağlantı uç noktaları tanımlayın. Bu kümedeki diğer tüm uç noktalar ile çakışmadığını sürece, bu bağlantı uç noktaları için herhangi bir bağlantı noktası numarası seçebilirsiniz. Çok düğümlü bir kümede bulunan bir veya daha fazla birincil düğüm vardır (diğer bir deyişle, Isprimary ayarlanmış *true*) bağlı olarak [düzeyinden](#reliability). Birincil ve birincil olmayan düğüm türleri hakkında daha fazla bilgi için bkz: [Service Fabric kümesi kapasite planlaması konuları](service-fabric-cluster-capacity.md) Nodetype'lar ve düzeyinden hakkında bilgi için. 

#### <a name="endpoints-used-to-configure-the-node-types"></a>Düğüm türleri yapılandırmak için kullanılan uç noktaları
* clientConnectionEndpointPort istemci API'leri kullanıldığında, kümeye bağlanmak için istemci tarafından kullanılan bağlantı noktasıdır. 
* clusterConnectionEndpointPort düğümleri birbirleri ile iletişim kurduğu bağlantı noktasıdır.
* leaseDriverEndpointPort düğümler hala etkin olup olmadığını öğrenmek için küme kira sürücüsü tarafından kullanılan bağlantı noktasıdır. 
* serviceConnectionEndpointPort bu belirli düğüm üzerindeki Service Fabric istemcisi ile iletişim kurmak için bir düğümde dağıtılmış hizmetlerin ve uygulamaların tarafından kullanılan bağlantı noktasıdır.
* httpGatewayEndpointPort tarafından Service Fabric Explorer, kümeye bağlanmak için kullanılan bağlantı noktasıdır.
* ephemeralPorts geçersiz kılma [işletim sistemi tarafından kullanılan dinamik bağlantı noktaları](https://support.microsoft.com/kb/929851). Service Fabric, bu bağlantı noktaları bir parçası bir uygulama bağlantı noktası kullanır ve kalan işletim sistemi için kullanılabilir. Tüm amaçlar için örnek JSON dosyalarında verilen aralıklar kullanabilmeniz için de bu aralıkta işletim sisteminde mevcut mevcut aralığı eşlenir. Başlangıç ve bitiş bağlantı noktaları arasındaki farkı 255'en az olduğundan emin olun. Bu aralık, işletim sistemi ile paylaşıldığından bu fark düşükse, çakışma çalışabilir. Yapılandırılmış bir dinamik bağlantı noktası aralığını görmek için şunu çalıştırın `netsh int ipv4 show dynamicport tcp`.
* applicationPorts Service Fabric uygulamaları tarafından kullanılan bağlantı noktalarıdır. Uygulama bağlantı noktası aralığı, uygulamalarınızın uç nokta gereksinimi karşılamak için yeteri kadar büyük olmalıdır. Bu aralık ephemeralPorts aralığı yapılandırmasında belirlenen diğer bir deyişle, makine üzerinde dinamik bağlantı noktası aralığından özel olmalıdır. Service Fabric yeni bağlantı noktaları gerekli olduğunda bu bağlantı noktalarını kullanır ve bu bağlantı noktaları için güvenlik duvarını açma üstlenir. 
* reverseProxyEndpointPort bir isteğe bağlı bir ters proxy uç noktadır. Daha fazla bilgi için [Service Fabric ters proxy'si](service-fabric-reverseproxy.md). 

### <a name="log-settings"></a>Günlük ayarları
FabricSettings bölümde, Service Fabric verileri ve günlükleri için kök dizinler ayarlayabilirsiniz. Yalnızca ilk küme oluşturma sırasında bu dizinler özelleştirebilirsiniz. Bu bölümün aşağıdaki örnek kod parçacığına bakın:

```json
"fabricSettings": [{
    "name": "Setup",
    "parameters": [{
        "name": "FabricDataRoot",
        "value": "C:\\ProgramData\\SF"
    }, {
        "name": "FabricLogRoot",
        "value": "C:\\ProgramData\\SF\\Log"
}]
```

Bir işletim sistemi olmayan sürücü FabricDataRoot ve FabricLogRoot olarak kullanmanızı öneririz. Bu işletim Sisteminin yanıt vermediğinde durumları önleme, daha fazla güvenilirlik sağlar. Yalnızca veri kökü özelleştirirseniz, günlük kök veri kökünün altındaki bir düzey yerleştirilir.

### <a name="stateful-reliable-services-settings"></a>Durum bilgisi olan Reliable Services ayarları
KtlLogger bölümünde Reliable Services genel yapılandırma ayarları ayarlayabilirsiniz. Bu ayarlar hakkında daha fazla bilgi için bkz. [durum bilgisi olan Reliable Services yapılandırma](service-fabric-reliable-services-configuration.md). Aşağıdaki örnek, durum bilgisi olan hizmetler için herhangi bir güvenilir koleksiyonlar yedeklemek için oluşturulan paylaşılan işlem günlüğünü değiştirmek gösterilmektedir:

```json
"fabricSettings": [{
    "name": "KtlLogger",
    "parameters": [{
        "name": "SharedLogSizeInMB",
        "value": "4096"
    }]
}]
```

### <a name="add-on-features"></a>Eklenti Özellikleri
Eklenti özellikleri yapılandırmak için apiVersion 04-2017 olarak ya da daha yüksek yapılandırın ve addonFeatures burada gösterildiği gibi yapılandırın:

```json
"apiVersion": "04-2017",
"properties": {
    "addOnFeatures": [
        "DnsService",
        "RepairManager"
    ]
}
```

### <a name="container-support"></a>Kapsayıcı desteği
Windows Server kapsayıcıları hem Hyper-V kapsayıcıları tek başına kümeler için kapsayıcı desteğini etkinleştirmek için DnsService eklenti özelliği etkinleştirilmelidir.

## <a name="next-steps"></a>Sonraki adımlar
Eksiksiz bir sonra *ClusterConfig.json* tek başına Küme kurulumu göre yapılandırılmış dosya kümenizin dağıtabilirsiniz. Bağlantısındaki [tek başına Service Fabric kümesi oluşturma](service-fabric-cluster-creation-for-windows-server.md). 

Dağıtılan bir tek başına kümeniz varsa, ayrıca [tek başına küme yapılandırmasını yükseltme](service-fabric-cluster-config-upgrade-windows-server.md). 

Bilgi edinmek için nasıl [Service Fabric Explorer ile kümenizi görselleştirme](service-fabric-visualizing-your-cluster.md).

