---
title: "Azure yük dengeleyici dağıtım modunu yapılandırmak | Microsoft Docs"
description: "Kaynak IP benzeşimini desteklemek Azure yük dengeleyici için dağıtım modunu yapılandırmak nasıl."
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
ms.assetid: 7df27a4d-67a8-47d6-b73e-32c0c6206e6e
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: d04a469c04553b7d6a14df7054ad5ef795baa500
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="configure-the-distribution-mode-for-azure-load-balancer"></a>Dağıtım modunu Azure yük dengeleyici için yapılandırma

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

## <a name="hash-based-distribution-mode"></a>Karma tabanlı dağıtım modu

Dağıtım için varsayılan Azure yük dengeleyici 5 bölütlü karma moddur. Tuple kaynak IP, kaynak bağlantı noktası, hedef IP, hedef bağlantı noktası ve protokol türü oluşur. Karma trafiği kullanılabilir sunuculara eşlemek için kullanılır ve yalnızca bir aktarım oturumu içinde sürekliliği algoritması sağlar. Aynı oturum olan paketleri aynı veri merkezinde IP'yi (DIP) örneğini yük dengeli uç nokta arkasında yönlendirilir. İstemci aynı kaynak IP yeni bir oturum başlatıldığında, kaynak bağlantı noktası değiştirir ve farklı bir DIP bitiş noktasına gitmek trafiği neden olur.

![5-tanımlama grubu karma tabanlı dağıtım modu](./media/load-balancer-distribution-mode/load-balancer-distribution.png)

## <a name="source-ip-affinity-mode"></a>Kaynak IP benzeşim modu

Yük Dengeleyici kaynak IP benzeşim dağıtım modunu kullanarak da yapılandırılabilir. Bu dağıtım olarak da bilinen oturum benzeşimi veya istemci IP benzeşimi modudur. (Kaynak IP, hedef IP ve protokol türü için) 3 bölütlü karma trafiği kullanılabilir sunuculara eşlemek için veya bir 2 bölütlü (kaynak IP ve hedef IP) modunu kullanır. Kaynak IP benzeşim kullanarak, aynı istemci bilgisayardan başlatılan bağlantıları aynı DIP bitiş noktasına gidin.

Aşağıdaki şekilde 2 bölütlü yapılandırma gösterilmektedir. 2 bölütlü yük dengeleyiciden sanal makineye 1 (VM1) nasıl çalışacağını dikkat edin. VM1 VM2 ve VM3 tarafından yedeklenir.

![2 bölütlü oturum benzeşimi dağıtım modu](./media/load-balancer-distribution-mode/load-balancer-session-affinity.png)

Kaynak IP benzeşim modu Azure yük dengeleyici ve Uzak Masaüstü Ağ Geçidi (RD Ağ Geçidi) arasında bir uyumsuzluk çözer. Bu mod kullanarak, bir tek bulut hizmeti RD Ağ Geçidi grupta oluşturabilirsiniz.

Başka bir kullanım senaryosu medya karşıya yükleme ' dir. UDP veriler karşıya olur, ancak denetim düzlemi TCP sağlanır:

* Bir istemci yük dengeli genel adresine TCP oturumu başlatır ve belirli bir DIP için yönlendirilir. Kanal bağlantı durumunu izlemek için etkin kalır.
* Aynı istemci bilgisayardan yeni bir UDP oturumu için aynı yük dengeli ortak uç başlatılır. Bağlantı, önceki TCP bağlantı olarak aynı DIP uç yönlendirilir. Medya karşıya yükleme denetim kanalı TCP üzerinden korurken yüksek verimlilik çalıştırılabilir.

> [!NOTE]
> Yük dengeli kümesi bir sanal makine ekleyerek veya çıkararak değiştiğinde, istemci isteklerini dağıtımını yeniden. Aynı sunucuda sonuna varolan istemcilerden gelen yeni bağlantıları üzerinde bağımlı olamaz. Ayrıca, kaynak IP kullanarak benzeşim dağıtım modu trafiğinin eşit olmayan bir dağıtım neden olabilir. Proxy çalıştıran istemciler bir benzersiz istemci uygulaması olarak görülebilir.

## <a name="configure-source-ip-affinity-settings"></a>Kaynak IP benzeşim ayarlarını yapılandırın

Sanal makineler için Azure PowerShell zaman aşımı ayarlarını değiştirmek için kullanın. Bir sanal makine için Azure bir uç nokta ekleyin ve yük dengeleyici dağıtım modu yapılandırın:

```powershell
Get-AzureVM -ServiceName mySvc -Name MyVM1 | Add-AzureEndpoint -Name HttpIn -Protocol TCP -PublicPort 80 -LocalPort 8080 –LoadBalancerDistribution sourceIP | Update-AzureVM
```

Değerini `LoadBalancerDistribution` öğesi için Yük Dengeleme istenen miktarı. 2 bölütlü (kaynak IP ve hedef IP) Yük Dengeleme için Sourceıp belirtin. 3-tanımlama grubu (kaynak IP, hedef IP ve protokol türü için) Yük Dengeleme sourceIPProtocol belirtin. Hiçbiri için 5-tanımlama grubu Yük Dengelemesi varsayılan davranışı belirtin.

Bu ayarları kullanarak bir uç nokta yük dengeleyici dağıtım modu yapılandırması Al:

    PS C:\> Get-AzureVM –ServiceName MyService –Name MyVM | Get-AzureEndpoint

    VERBOSE: 6:43:50 PM - Completed Operation: Get Deployment
    LBSetName : MyLoadBalancedSet
    LocalPort : 80
    Name : HTTP
    Port : 80
    Protocol : tcp
    Vip : 65.52.xxx.xxx
    ProbePath :
    ProbePort : 80
    ProbeProtocol : tcp
    ProbeIntervalInSeconds : 15
    ProbeTimeoutInSeconds : 31
    EnableDirectServerReturn : False
    Acl : {}
    InternalLoadBalancerName :
    IdleTimeoutInMinutes : 15
    LoadBalancerDistribution : sourceIP

Zaman `LoadBalancerDistribution` öğesi mevcut değil, Azure yük dengeleyici varsayılan 5-tanımlama grubu algoritması kullanır.

### <a name="configure-distribution-mode-on-load-balanced-endpoint-set"></a>Yük dengeli uç nokta kümesi dağıtım modunu yapılandırma

Uç nokta yük dengeli uç nokta kümesi parçası olduğunda, dağıtım modu yük dengeli uç nokta kümesi üzerinde yapılandırılmalıdır:

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName MyService -LBSetName LBSet1 -Protocol TCP -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 –LoadBalancerDistribution sourceIP
```

### <a name="configure-distribution-mode-for-cloud-services-endpoints"></a>Bulut Hizmetleri uç noktaları için dağıtım modunu yapılandırma

.NET 2.5 için Azure SDK'sı, bulut hizmeti güncelleştirmek için kullanın. Bulut Hizmetleri için uç nokta ayarlarını .csdef dosyasında yapılır. Bulut Hizmetleri dağıtımı için yük dengeleyici dağıtım modu güncelleştirmek için bir dağıtım yükseltme gereklidir.

.Csdef değişiklikleri uç noktası ayarları için bir örneği burada verilmiştir:

```xml
<WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancerDistribution="sourceIP" />
    </Endpoints>
</WorkerRole>
<NetworkConfiguration>
    <VirtualNetworkSite name="VNet"/>
    <AddressAssignments>
<InstanceAddress roleName="VMRolePersisted">
    <PublicIPs>
    <PublicIP name="public-ip-name" idleTimeoutInMinutes="timeout-in-minutes"/>
    </PublicIPs>
</InstanceAddress>
    </AddressAssignments>
</NetworkConfiguration>
```

## <a name="api-example"></a>API örneği

Aşağıdaki örnek, belirtilen yük dengeli Küme dağıtımı için yük dengeleyici dağıtım modu yapılandırılacağını gösterir. 

### <a name="change-distribution-mode-for-deployed-load-balanced-set"></a>Dağıtılmış yük dengeli kümesi için dağıtım modunu değiştirme

Var olan dağıtım yapılandırmasını değiştirmek için Azure Klasik dağıtım modeli kullanın. Ekleme `x-ms-version` başlığı ve sürümü 2014-09-01 için değeri ayarlayın veya sonraki bir sürümü.

#### <a name="request"></a>İstek

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>?comp=UpdateLbSet   x-ms-version: 2014-09-01
    Content-Type: application/xml

    <LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <InputEndpoint>
        <LoadBalancedEndpointSetName> endpoint-set-name </LoadBalancedEndpointSetName>
        <LocalPort> local-port-number </LocalPort>
        <Port> external-port-number </Port>
        <LoadBalancerProbe>
          <Port> port-assigned-to-probe </Port>
          <Protocol> probe-protocol </Protocol>
          <IntervalInSeconds> interval-of-probe </IntervalInSeconds>
          <TimeoutInSeconds> timeout-for-probe </TimeoutInSeconds>
        </LoadBalancerProbe>
        <Protocol> endpoint-protocol </Protocol>
        <EnableDirectServerReturn> enable-direct-server-return </EnableDirectServerReturn>
        <IdleTimeoutInMinutes>idle-time-out</IdleTimeoutInMinutes>
        <LoadBalancerDistribution>sourceIP</LoadBalancerDistribution>
      </InputEndpoint>
    </LoadBalancedEndpointList>

Daha önce açıklanan, ayarlanmış `LoadBalancerDistribution` Sourceıp 2 bölütlü benzeşimi, 3-tanımlama grubu benzeşimi sourceIPProtocol veya hiçbiri yok benzeşimi (5-tanımlama grubu benzeşim) öğesine.

#### <a name="response"></a>Yanıt

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Content-Length: 0
    Server: 1.0.6198.146 (rd_rdfe_stable.141015-1306) Microsoft-HTTPAPI/2.0
    x-ms-servedbyregion: ussouth2
    x-ms-request-id: 9c7bda3e67c621a6b57096323069f7af
    Date: Thu, 16 Oct 2014 22:49:21 GMT

## <a name="next-steps"></a>Sonraki adımlar

* [Azure iç yük dengeleyici genel bakış](load-balancer-internal-overview.md)
* [Bir internet'e yönelik Yük Dengeleyici yapılandırma ile çalışmaya başlama](load-balancer-get-started-internet-arm-ps.md)
* [Yük dengeleyiciniz için boşta TCP zaman aşımı ayarlarını yapılandırma](load-balancer-tcp-idle-timeout.md)
