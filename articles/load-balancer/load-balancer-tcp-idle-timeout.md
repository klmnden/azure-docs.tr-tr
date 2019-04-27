---
title: Azure'da yük dengeleyici TCP boşta kalma zaman aşımı yapılandırma
titlesuffix: Azure Load Balancer
description: Yük Dengeleyici TCP boşta kalma zaman aşımı yapılandırma
services: load-balancer
documentationcenter: na
author: kumudd
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 0c57eec4d739da13d98099a6b2f01fbf0ad0051c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60734618"
---
# <a name="configure-tcp-idle-timeout-settings-for-azure-load-balancer"></a>Azure Load Balancer için TCP boşta kalma zaman aşımı ayarlarını yapılandırma

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Varsayılan yapılandırmasında, Azure Load Balancer bir 4 dakikalık boşta kalma zaman aşımı ayarı vardır. Zaman aşımı değerinden daha uzun bir süre işlem yapılmadığında ise, TCP veya HTTP oturumu, istemci ile bulut hizmetinizi arasında korunacağı süreyi garantisi yoktur.

Bağlantı kapalı olduğunda, istemci uygulamanız aşağıdaki hata iletisini alabilirsiniz: "Temel alınan bağlantı kapatıldı: Canlı tutmak için beklenen bir bağlantı sunucu tarafından kapatıldı."

Bir TCP tutma kullanmak yaygın bir uygulamadır. Bu yöntem daha uzun bir süre bağlantıyı etkin tutar. Daha fazla bilgi için bkz: [.NET örnekleri](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx). Etkin tutma, paket bağlantısı etkin olmadığı dönemler sırasında gönderilir. Tutma bu paketler, boşta kalma zaman aşımı değeri hiçbir zaman ulaştı ve uzun bir süre için bağlantının korunacağı süreyi emin olun.

Bu ayar yalnızca gelen bağlantıları için çalışır. Bağlantı kaybını önlemek için tutma TCP boşta kalma zaman aşımı ayarını değerinden küçük bir aralık yapılandırmak veya boşta kalma zaman aşımı değerini artırın. Böyle senaryoları desteklemek için yapılandırılabilir bir boşta kalma zaman aşımı için destek ekledik. Artık 4 ila 30 dakika boyunca ayarlayabilirsiniz.

TCP tutma iyi pil ömrünü kısıtlama olduğu senaryolar için çalışır. Mobil uygulamalar için önerilmez. Bir mobil uygulama tutma bir TCP kullanarak cihaz pilin daha hızlı tükenir.

![TCP zaman aşımı](./media/load-balancer-tcp-idle-timeout/image1.png)

Aşağıdaki bölümlerde, sanal makineler boşta kalma zaman aşımı ayarlarını değiştirin ve bulut hizmetleri nasıl açıklanmaktadır.

## <a name="configure-the-tcp-timeout-for-your-instance-level-public-ip-to-15-minutes"></a>Örnek düzeyi genel IP ile 15 dakika için TCP zaman aşımı yapılandırma

```powershell
Set-AzurePublicIP -PublicIPName webip -VM MyVM -IdleTimeoutInMinutes 15
```

`IdleTimeoutInMinutes` isteğe bağlıdır. Ayarlanmazsa, varsayılan zaman aşımı 4 dakikadır. Kabul edilebilir zaman aşımı aralığı 4 ila 30 dakikadır.

## <a name="set-the-idle-timeout-when-creating-an-azure-endpoint-on-a-virtual-machine"></a>Bir Azure uç noktası üzerinde bir sanal makine oluştururken boşta kalma zaman aşımını ayarlayın

Bir uç nokta için zaman aşımı ayarını değiştirmek için aşağıdakileri kullanın:

```powershell
Get-AzureVM -ServiceName "mySvc" -Name "MyVM1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 8080 -IdleTimeoutInMinutes 15| Update-AzureVM
```

Boşta kalma zaman aşımı yapılandırmanızı almak için aşağıdaki komutu kullanın:

    PS C:\> Get-AzureVM -ServiceName "MyService" -Name "MyVM" | Get-AzureEndpoint
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

## <a name="set-the-tcp-timeout-on-a-load-balanced-endpoint-set"></a>Yük dengeli uç nokta kümesinde TCP zaman aşımı ayarlayın

Uç noktaları bir yük dengeli uç nokta kümesinin parçasıysa, yük dengeli uç nokta kümesinde TCP zaman aşımı ayarlanmalıdır. Örneğin:

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName "MyService" -LBSetName "LBSet1" -Protocol tcp -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 -IdleTimeoutInMinutes 15
```

## <a name="change-timeout-settings-for-cloud-services"></a>Bulut Hizmetleri için zaman aşımı ayarlarını değiştirme

Bulut hizmetinizi güncelleştirme için Azure SDK'sını kullanabilirsiniz. .Csdef dosyasında bulut Hizmetleri için uç nokta ayarları yapmanızı ister. TCP zaman aşımı bir bulut hizmeti dağıtımı için Güncelleştirme dağıtımı yükseltme gerektirir. Yalnızca bir genel IP için TCP zaman aşımı belirtilmiş olup olmadığını bir özel durumdur. .Cscfg dosyasında genel IP ayarlardır ve bunları dağıtım güncelleştirme ve yükseltme güncelleştirebilirsiniz.

Uç noktası için ayarların .csdef değişiklikler şunlardır:

```xml
<WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" idleTimeoutInMinutes="tcp-timeout" />
    </Endpoints>
</WorkerRole>
```

Genel IP'ler zaman aşımı ayarını için .cscfg değişiklikler şunlardır:

```xml
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

## <a name="rest-api-example"></a>REST API örneği

Hizmet Yönetimi API'sini kullanarak, TCP boşta kalma zaman aşımı yapılandırabilirsiniz. Emin olun `x-ms-version` üstbilgisi sürümüne ayarlandı `2014-06-01` veya üzeri. Belirtilen yük dengeli giriş uç noktaları bir dağıtımdaki tüm sanal makineler üzerinde yapılandırmasını güncelleştirin.

### <a name="request"></a>İstek

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>

### <a name="response"></a>Yanıt

```xml
<LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="https://www.w3.org/2001/XMLSchema-instance">
    <InputEndpoint>
    <LoadBalancedEndpointSetName>endpoint-set-name</LoadBalancedEndpointSetName>
    <LocalPort>local-port-number</LocalPort>
    <Port>external-port-number</Port>
    <LoadBalancerProbe>
        <Path>path-of-probe</Path>
        <Port>port-assigned-to-probe</Port>
        <Protocol>probe-protocol</Protocol>
        <IntervalInSeconds>interval-of-probe</IntervalInSeconds>
        <TimeoutInSeconds>timeout-for-probe</TimeoutInSeconds>
    </LoadBalancerProbe>
    <LoadBalancerName>name-of-internal-loadbalancer</LoadBalancerName>
    <Protocol>endpoint-protocol</Protocol>
    <IdleTimeoutInMinutes>15</IdleTimeoutInMinutes>
    <EnableDirectServerReturn>enable-direct-server-return</EnableDirectServerReturn>
    <EndpointACL>
        <Rules>
        <Rule>
            <Order>priority-of-the-rule</Order>
            <Action>permit-rule</Action>
            <RemoteSubnet>subnet-of-the-rule</RemoteSubnet>
            <Description>description-of-the-rule</Description>
        </Rule>
        </Rules>
    </EndpointACL>
    </InputEndpoint>
</LoadBalancedEndpointList>
```

## <a name="next-steps"></a>Sonraki adımlar

[İç yük dengeleyiciye genel bakış](load-balancer-internal-overview.md)

[Bir Internet'e yönelik Yük Dengeleyici yapılandırmaya başlayın](load-balancer-get-started-internet-arm-ps.md)

[Yük dengeleyici dağıtım modu yapılandırma](load-balancer-distribution-mode.md)
