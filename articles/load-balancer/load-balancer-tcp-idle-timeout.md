---
title: Yük Dengeleyici TCP boşta kalma zaman aşımı yapılandırma | Microsoft Docs
description: Yük Dengeleyici TCP boşta kalma zaman aşımı yapılandırın
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
ms.assetid: 4625c6a8-5725-47ce-81db-4fa3bd055891
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: f19ac77f7c7f7d4ab8909d628f9dcce08c07c928
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23855232"
---
# <a name="configure-tcp-idle-timeout-settings-for-azure-load-balancer"></a>Azure yük dengeleyici için TCP boşta kalma zaman aşımı ayarlarını yapılandırın

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Varsayılan yapılandırmasında, Azure yük dengeleyici, 4 dakikalık bir boşta kalma zaman aşımı ayarının. Bir süre işlem yapılmadığında zaman aşımı değeri uzun olması durumunda, TCP veya HTTP oturumu istemci ile bulut hizmetiniz arasında korunur garanti yoktur.

Bağlantı kapalı olduğunda istemci uygulamanızı aşağıdaki hata iletisini alabilirsiniz: "arka plandaki bağlantı kesildi: Canlı tutulacağı beklenen bir bağlantı sunucu tarafından kapatıldı."

Bir TCP tutma kullanan yaygın bir uygulamadır. Bu uygulama için daha uzun bir süre bağlantıyı etkin tutar. Daha fazla bilgi için bkz: [.NET örnekleri](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx). Etkin tutma, bağlantıda kaldıktan dönemlerde gönderilen paket. Bu tutma paketler boşta kalma zaman aşımı değeri hiçbir zaman ulaştı ve bağlantı uzun bir süre boyunca tutulur emin olun.

Bu ayar yalnızca gelen bağlantılar için çalışır. Bağlantı kaybını önlemek için boşta kalma zaman aşımı ayarını daha küçük bir aralık ile tutma TCP yapılandırın veya boşta kalma zaman aşımı değerini artırın. Bu tür senaryoları desteklemek için yapılandırılabilir boşta kalma zaman aşımı için destek ekledik. Şimdi 4 ila 30 dakika süresince ayarlayabilirsiniz.

TCP tutma iyi pil ömrünün bir kısıtlaması olduğu senaryolar için çalışır. Mobil uygulamalar için önerilmez. Bir mobil uygulama tutma bir TCP kullanarak aygıt pil daha hızlı tükenir.

![TCP zaman aşımı](./media/load-balancer-tcp-idle-timeout/image1.png)

Aşağıdaki bölümlerde, sanal makineleri boşta kalma zaman aşımı ayarlarını değiştirin ve bulut hizmetlerini açıklar.

## <a name="configure-the-tcp-timeout-for-your-instance-level-public-ip-to-15-minutes"></a>Örnek düzeyinde ortak IP-15 dakika için TCP zaman aşımı yapılandırın

```powershell
Set-AzurePublicIP -PublicIPName webip -VM MyVM -IdleTimeoutInMinutes 15
```

`IdleTimeoutInMinutes` isteğe bağlıdır. Ayarlanmazsa, varsayılan zaman aşımı 4 dakikadır. Kabul edilebilir zaman aşımı aralığı 4-30 dakikadır.

## <a name="set-the-idle-timeout-when-creating-an-azure-endpoint-on-a-virtual-machine"></a>Azure bir uç nokta bir sanal makinede oluştururken boşta kalma zaman aşımını ayarlama

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

## <a name="set-the-tcp-timeout-on-a-load-balanced-endpoint-set"></a>TCP zaman aşımı üzerinde bir yük dengeli uç nokta kümesi

Uç nokta yük dengeli uç nokta kümesi parçası ise, TCP zaman aşımı yük dengeli uç nokta kümesi ayarlamanız gerekir. Örneğin:

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName "MyService" -LBSetName "LBSet1" -Protocol tcp -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 -IdleTimeoutInMinutes 15
```

## <a name="change-timeout-settings-for-cloud-services"></a>Bulut Hizmetleri zaman aşımı ayarlarını değiştirme

Bulut hizmeti güncelleştirmek için Azure SDK'sını kullanabilirsiniz. Bulut Hizmetleri için uç nokta ayarlarını .csdef dosyasında yapın. Bir bulut hizmeti dağıtımının TCP zaman aşımı güncelleştirme dağıtımı yükseltme gerektirir. Yalnızca genel IP için TCP zaman aşımı belirtilmezse dışındadır. Genel IP ayarlarını .cscfg dosyasında yer alır ve bunları dağıtım güncelleştirme ve yükseltme güncelleştirebilirsiniz.

Uç noktası ayarları için .csdef değişir:

```xml
<WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" idleTimeoutInMinutes="tcp-timeout" />
    </Endpoints>
</WorkerRole>
```

Zaman aşımı ayarı genel IP'ler için .cscfg değişiklikleri şunlardır:

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

Hizmet Yönetimi API kullanarak TCP boşta kalma zaman aşımı yapılandırabilirsiniz. Olduğundan emin olun `x-ms-version` üstbilgisi sürüme ayarlandı `2014-06-01` veya sonraki bir sürümü. Belirtilen yük dengeli giriş uç noktaları bir dağıtımdaki tüm sanal makinelerde yapılandırmasını güncelleştirin.

### <a name="request"></a>İstek

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>

### <a name="response"></a>Yanıt

```xml
<LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
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

[Bir Internet'e yönelik Yük Dengeleyici yapılandırma kullanmaya başlama](load-balancer-get-started-internet-arm-ps.md)

[Yük dengeleyici dağıtım modu yapılandırma](load-balancer-distribution-mode.md)
