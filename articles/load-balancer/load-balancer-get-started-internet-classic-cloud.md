---
title: Bir Azure bulut Hizmetleri için Internet'e yönelik Yük Dengeleyici oluşturma
titlesuffix: Azure Load Balancer
description: Klasik dağıtımda bulut hizmetleri için İnternet’e yönelik yük dengeleyici oluşturmayı öğrenin
services: load-balancer
documentationcenter: na
author: genlin
manager: cshepard
tags: azure-service-management
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: genli
ms.openlocfilehash: 66c978a7eb151ce9df939a11e2e3c0016c8e7c9e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60532526"
---
# <a name="get-started-creating-an-internet-facing-load-balancer-for-cloud-services"></a>Bulut hizmetleri için İnternet’e yönelik yük dengeleyici oluşturmaya başlama

> [!div class="op_single_selector"]
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [Azure Cloud Services](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> Azure kaynaklarıyla çalışmadan önce Azure'da şu anda iki dağıtım modeli olduğunu anlamak önemlidir: Azure Resource Manager ve klasik. Azure kaynaklarıyla çalışmadan önce [dağıtım modellerini ve araçlarlarını](../azure-classic-rm.md) iyice anladığınızdan emin olun. Bu makalenin en üstündeki sekmelere tıklayarak farklı araçlarla ilgili belgeleri görüntüleyebilirsiniz. Bu makale, klasik dağıtım modelini kapsamaktadır. [Azure Resource Manager kullanarak İnternet’e yönelik yük dengeleyici oluşturma](load-balancer-get-started-internet-arm-ps.md) sayfasını da inceleyebilirsiniz.

Bulut hizmetleri yük dengeleyici ile otomatik olarak yapılandırılır ve hizmet modeliyle özelleştirilebilir.

## <a name="create-a-load-balancer-using-the-service-definition-file"></a>Hizmet tanım dosyası kullanarak yük dengeleyici oluşturma

Bulut hizmetinizi güncelleştirme amacıyla .NET 2.5 için Azure SDK kullanabilirsiniz. Bulut hizmetleri için uç nokta ayarları [servicedefinition](https://msdn.microsoft.com/library/azure/gg557553.aspx).csdef dosyasında yapılır.

Aşağıdaki örnekte servicedefinition.csdef dosyasının bir bulut dağıtımı için nasıl yapılandırıldığı gösterilmektedir:

Bir bulut dağıtımı tarafından oluşturulmuş .csdef dosyası parçacığını denetlediğinizde, dış uç noktanın 10000, 10001 ve 10002 numaralı bağlantı noktaları üzerinde HTTP bağlantı noktalarını kullanacak şekilde yapılandırıldığını görebilirsiniz.

```xml
<ServiceDefinition name="Tenant">
    <WorkerRole name="FERole" vmsize="Small">
        <Endpoints>
            <InputEndpoint name="FE_External_Http" protocol="http" port="10000" />
            <InputEndpoint name="FE_External_Tcp"  protocol="tcp"  port="10001" />
            <InputEndpoint name="FE_External_Udp"  protocol="udp"  port="10002" />

            <InputEndpoint name="HTTP_Probe" protocol="http" port="80" loadBalancerProbe="MyProbe" />

            <InstanceInputEndpoint name="InstanceEP" protocol="tcp" localPort="80">
                <AllocatePublicPortFrom>
                    <FixedPortRange min="10110" max="10120"  />
                </AllocatePublicPortFrom>
            </InstanceInputEndpoint>
            <InternalEndpoint name="FE_InternalEP_Tcp" protocol="tcp" />
        </Endpoints>
    </WorkerRole>
</ServiceDefinition>
```

## <a name="check-load-balancer-health-status-for-cloud-services"></a>Bulut hizmetleri için yük dengeleyici sistem durumunu denetleme

Aşağıda bir sistem durumu araştırma örneği verilmiştir:

```xml
<LoadBalancerProbes>
    <LoadBalancerProbe name="MyProbe" protocol="http" path="Probe.aspx" intervalInSeconds="5" timeoutInSeconds="100" />
</LoadBalancerProbes>
```

Yük dengeleyici uç nokta bilgilerini ve araştırma bilgilerini kullanarak sistem durumunu sorgulamak için kullanılabilecek `http://{DIP of VM}:80/Probe.aspx` biçiminde bir URL oluşturur.

Hizmet aynı IP adresinden gelen aralıklı araştırmaları algılar. Bu, sanal makinenin çalıştığı düğümün ana bilgisayarından gelen sistem durumu araştırma isteğidir. Yük dengeleyicinin, hizmetin iyi durumda olduğunu kabul etmesi için hizmetin HTTP 200 durum koduyla yanıt vermesi gerekir. Diğer tüm HTTP durum kodları (503 gibi) sanal makineyi sistemin dışında bırakır.

Araştırma tanımı ayrıca araştırma sıklığını da denetler. Yukarıdaki durumda yük dengeleyici uç noktayı 5 saniyede bir araştırmaktadır. 10 saniye (iki araştırma aralığı) boyunca olumlu yanıt gelmemesi halinde araştırma sonucu olumsuz kabul edilir ve sanal makine sistemin dışında tutulur. Benzer şekilde, hizmetin sistem dışında tutulması sırasında olumlu yanıt alınması halinde hizmet yeniden devreye alınır. Hizmet durumu sağlam ve sağlam değil şeklinde değişiklik gösteriyorsa yük dengeleyici, hizmetin birkaç araştırma boyunca sağlam olduğunu algılaması halinde hizmetin devreye alınmasını geciktirmeye karar verebilir.

Daha fazla bilgi için [sistem durumu araştırması](https://msdn.microsoft.com/library/azure/jj151530.aspx) hizmet tanım düzenini denetleyin.

## <a name="next-steps"></a>Sonraki adımlar

[Bir iç yük dengeleyici yapılandırmaya başlayın](load-balancer-get-started-ilb-arm-ps.md)

[Yük dengeleyici dağıtım modu yapılandırma](load-balancer-distribution-mode.md)

[Yük dengeleyiciniz için boşta TCP zaman aşımı ayarlarını yapılandırma](load-balancer-tcp-idle-timeout.md)

