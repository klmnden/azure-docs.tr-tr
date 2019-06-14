---
title: Internet'e yönelik Yük Dengeleyici - Azure Klasik CLI oluşturma
titlesuffix: Azure Load Balancer
description: Klasik Azure CLI kullanarak klasik dağıtımda İnternet’e yönelik yük dengeleyici oluşturmayı öğrenin
services: load-balancer
documentationcenter: na
author: genlin
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: fa89117e85bc3d3c9664e6aa037fac923b7432ce
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60544892"
---
# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-classic-cli"></a>Klasik Azure CLI'de Internet'e yönelik Yük Dengeleyici (Klasik) oluşturmaya başlama

> [!div class="op_single_selector"]
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [Azure klasik CLI](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [Azure Cloud Services](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> Azure kaynaklarıyla çalışmadan önce Azure'da şu anda iki dağıtım modeli olduğunu anlamak önemlidir: Azure Resource Manager ve klasik. Azure kaynaklarıyla çalışmadan önce [dağıtım modellerini ve araçlarlarını](../azure-classic-rm.md) iyice anladığınızdan emin olun. Bu makalenin en üstündeki sekmelere tıklayarak farklı araçlarla ilgili belgeleri görüntüleyebilirsiniz. Bu makale, klasik dağıtım modelini kapsamaktadır. [Azure Resource Manager kullanarak İnternet’e yönelik yük dengeleyici oluşturma](load-balancer-get-started-internet-arm-ps.md) sayfasını da inceleyebilirsiniz.

[!INCLUDE [requires-classic-cli](../../includes/contains-classic-cli-content.md)]

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="create-an-internet-facing-load-balancer-using-cli"></a>CLI kullanarak İnternet'e yönelik yük dengeleyici oluşturma

Bu kılavuz, yukarıdaki senaryoya göre İnternet’e yönelik yük dengeleyicinin nasıl oluşturulacağını göstermektedir.

1. Hiç Azure klasik CLI kullanmadıysanız bkz. [Azure klasik CLI’yi Yükleme ve Yapılandırma](../cli-install-nodejs.md); sonra da, Azure hesabınızı ve aboneliğinizi seçtiğiniz noktaya kadar yönergeleri uygulayın.
2. Klasik moda geçmek için **azure config mode** komutunu aşağıda gösterildiği gibi çalıştırın.

    ```azurecli
    azure config mode asm
    ```

    Beklenen çıktı:

        info:    New mode is asm

## <a name="create-endpoint-and-load-balancer-set"></a>Uç nokta ve yük dengeleyici kümesi oluşturma

Bu senaryoda "web1" ve "web2" adlı sanal makinelerin oluşturulduğu varsayılmaktadır.
Bu kılavuzda hem genel hem de yerel bağlantı noktası olarak 80 numaralı bağlantı noktası kullanılarak bir yük dengeleyici kümesi oluşturulmaktadır. 80 numaralı bağlantı noktasında da bir araştırma bağlantı noktası yapılandırılmakta ve yük dengeleyici kümesi "lbset" olarak adlandırılmaktadır.

### <a name="step-1"></a>1\. Adım

`azure network vm endpoint create` kullanarak "web1" adlı sanal makine için ilk uç noktayı ve yük dengeleyici kümesini oluşturun.

```azurecli
azure vm endpoint create web1 80 --local-port 80 --protocol tcp --probe-port 80 --load-balanced-set-name lbset
```

### <a name="step-2"></a>2\. Adım

Yük dengeleyici kümesine "web2" adlı ikinci bir sanal makine ekleyin.

```azurecli
azure vm endpoint create web2 80 --local-port 80 --protocol tcp --probe-port 80 --load-balanced-set-name lbset
```

### <a name="step-3"></a>3\. Adım

`azure vm show` komutunu kullanarak yük dengeleyici yapılandırmasını doğrulayın.

```azurecli
azure vm show web1
```

Çıktı şu şekilde olacaktır:

    data:    DNSName "contoso.cloudapp.net"
    data:    Location "East US"
    data:    VMName "web1"
    data:    IPAddress "10.0.0.5"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-2015
    6-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "joaoma-1-web1-0-201509251804250879"
    data:    OSDisk mediaLink "https://XXXXXXXXXXXXXXX.blob.core.windows.
    /vhds/joaomatest-web1-2015-09-25.vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Se
    r-2012-R2-20150916-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "XXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 name "XXXXXXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    Network Endpoints 0 loadBalancedEndpointSetName "lbset"
    data:    Network Endpoints 0 localPort 80
    data:    Network Endpoints 0 name "tcp-80-80"
    data:    Network Endpoints 0 port 80
    data:    Network Endpoints 0 loadBalancerProbe port 80
    data:    Network Endpoints 0 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 0 loadBalancerProbe intervalInSeconds 15
    data:    Network Endpoints 0 loadBalancerProbe timeoutInSeconds 31
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 5986
    data:    Network Endpoints 1 name "PowerShell"
    data:    Network Endpoints 1 port 5986
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 3389
    data:    Network Endpoints 2 name "Remote Desktop"
    data:    Network Endpoints 2 port 58081
    info:    vm show command OK

## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a>Bir sanal makine için uzak masaüstü uç noktası oluşturma

`azure vm endpoint create` kullanarak belirli bir sanal makine için genel bağlantı noktasına gelen trafiği yerel bağlantı noktasına yönlendirme amacıyla uzak masaüstü uç noktası oluşturabilirsiniz.

```azurecli
azure vm endpoint create web1 54580 -k 3389
```

## <a name="remove-virtual-machine-from-load-balancer"></a>Sanal makineyi yük dengeleyiciden kaldırma

Yük dengeleyici kümesiyle ilişkilendirilmiş uç noktayı sanal makineden silmeniz gerekir. Uç nokta kaldırıldığında ilgili sanal makine artık yük dengeleyici kümesine ait olmayacaktır.

Yukarıdaki örneği kullanarak "web1" sanal makinesi için oluşturulan uç noktayı "lbset" yük dengeleyiciden `azure vm endpoint delete` komutuyla kaldırabilirsiniz.

```azurecli
azure vm endpoint delete web1 tcp-80-80
```

> [!NOTE]
> `azure vm endpoint --help` komutunu kullanarak diğer uç nokta yönetim seçeneklerini keşfedebilirsiniz

## <a name="next-steps"></a>Sonraki adımlar

[Bir iç yük dengeleyici yapılandırmaya başlayın](load-balancer-get-started-ilb-arm-ps.md)

[Yük dengeleyici dağıtım modu yapılandırma](load-balancer-distribution-mode.md)

[Yük dengeleyiciniz için boşta TCP zaman aşımı ayarlarını yapılandırma](load-balancer-tcp-idle-timeout.md)
