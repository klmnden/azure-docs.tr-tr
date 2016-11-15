---
title: "Klasik dağıtım modelinde Azure CLI kullanarak iç yük dengeleyici oluşturma | Microsoft Belgeleri"
description: "Klasik dağıtım modelinde Azure CLI kullanarak iç yük dengeleyici oluşturmayı öğrenin"
services: load-balancer
documentationcenter: na
author: sdwheeler
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: becbbbde-a118-4269-9444-d3153f00bf34
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2016
ms.author: sewhee
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 4364d3bcdffd278bef35b224a8e22062814ca490


---
# <a name="get-started-creating-an-internal-load-balancer-classic-using-the-azure-cli"></a>Azure CLI kullanarak iç yük dengeleyici (klasik) oluşturmaya başlama
[!INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]

[Bu adımları Resource Manager modeli kullanarak gerçekleştirmeyi](load-balancer-get-started-ilb-arm-cli.md) öğrenin.

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="to-create-an-internal-load-balancer-set-for-virtual-machines"></a>Sanal makineler için iç yük dengeleyici oluşturma
İç yük dengeleyici kümesi ve trafiğini bu kümeye gönderecek sunucuları oluşturmak için aşağıdaki adımları uygulamanız gerekir:

1. Gelen trafiğin uç noktası olacak ve bu trafiğe yük dengeli bir kümenin sunucularında yük dengelemesi yapacak bir İç Yük Dengeleme örneği oluşturun.
2. Gelen trafiği alacak sanal makinelere karşılık gelen uç noktalar ekleyin.
3. Yük dengelemesi yapılacak trafiği gönderecek sunucuları trafiklerini İç Yük Dengeleme örneğinin sanal IP (VIP) adresine gönderecek şekilde yapılandırın.

## <a name="step-by-step-creating-an-internal-load-balancer-using-cli"></a>CLI kullanarak iç yük dengeleyici oluşturma adımları
Bu kılavuz, yukarıdaki senaryoya göre iç yük dengeleyicinin nasıl oluşturulacağını göstermektedir.

1. Hiç Azure CLI kullanmadıysanız bkz. [Azure CLI’yi Yükleme ve Yapılandırma](../xplat-cli-install.md); sonra da, Azure hesabınızı ve aboneliğinizi seçtiğiniz noktaya kadar yönergeleri uygulayın.
2. Klasik moda geçmek için **azure config mode** komutunu aşağıda gösterildiği gibi çalıştırın.
   
        azure config mode asm
   
    Beklenen çıktı:
   
        info:    New mode is asm

## <a name="create-endpoint-and-load-balancer-set"></a>Uç nokta ve yük dengeleyici kümesi oluşturma
Bu senaryo, "mytestcloud" adlı bir bulut hizmetinde çalışan "DB1" ve "DB2" adlı iki sanal makine olduğunu varsaymaktadır. Her iki sanal makine de "subnet-1" alt ağına sahip "testvnet" adlı bir sanal ağ kullanmaktadır.

Bu kılavuzda hem özel hem de yerel bağlantı noktası olarak 1433 numaralı bağlantı noktası kullanılarak bir iç yük dengeleyici kümesi oluşturulmaktadır.

Bu, genelde veritabanı sunucularının genel IP adresi kullanarak doğrudan sunulmamasını garanti etmek için arka uçta iç yük dengeleyici kullanan SQL sanal makineleri olduğuna kullanılan bir senaryodur.

### <a name="step-1"></a>1. Adım
`azure network service internal-load-balancer add` kullanarak iç yük dengeleyici kümesi oluşturun.

     azure service internal-load-balancer add -r mytestcloud -n ilbset -t subnet-1 -a 192.168.2.7

Kullanılan parametreler:

**-r** - bulut hizmeti adı<BR>
**-n** - iç yük dengeleyici adı<BR>
**-t** - alt ağ adı (iç yük dengeleyiciye ekleyeceğiniz sanal makinelerle aynı alt ağ)<BR>
**-a** - (isteğe bağlı) statik özel IP adresi ekleyin<BR>

Daha fazla bilgi için bkz. `azure service internal-load-balancer --help`.

`azure service internal-load-balancer list` *bulut hizmeti adı* komutunu kullanarak iç yük dengeleyici özelliklerini denetleyebilirsiniz.

Çıktı örneği aşağıda verilmiştir:

    azure service internal-load-balancer list my-testcloud
    info:    Executing command service internal-load-balancer list
    + Getting cloud service deployment
    data:    Name    Type     SubnetName  StaticVirtualNetworkIPAddress
    data:    ------  -------  ----------  -----------------------------
    data:    ilbset  Private  subnet-1    192.168.2.7
    info:    service internal-load-balancer list command OK


## <a name="step-2"></a>2. Adım
İlk uç noktayı eklediğinizde iç yük dengeleyici kümesini yapılandırırsınız. Bu adımda uç noktası, sanal makine ve araştırma bağlantı noktasını iç yük dengeleyici kümesiyle ilişkilendireceksiniz.

    azure vm endpoint create db1 1433 -k 1433 tcp -t 1433 -r tcp -e 300 -f 600 -i ilbset

Kullanılan parametreler:

**-k** - yerel sanal makine bağlantı noktası<BR>
**-t** - araştırma bağlantı noktası<BR>
**-r** - araştırma protokolü<BR>
**-e** - araştırma aralığı (saniye)<BR>
**-f** - zaman aşımı aralığı (saniye) <BR>
**-i** - iç yük dengeleyici adı <BR>

## <a name="step-3"></a>3. Adım
`azure vm show` *sanal makine adı* komutunu kullanarak yük dengeleyici yapılandırmasını doğrulayın

    azure vm show DB1

Çıktı şu şekilde olacaktır:

        azure vm show DB1
    info:    Executing command vm show
    + Getting virtual machines
    data:    DNSName "mytestcloud.cloudapp.net"
    data:    Location "East US 2"
    data:    VMName "DB1"
    data:    IPAddress "192.168.2.4"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "db1-DB1-0-201511120457370846"
    data:    OSDisk mediaLink "https://XXXX.blob.core.windows.net/vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "137.116.64.107"
    data:    VirtualIPAddresses 0 name "db1ContractContract"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    VirtualIPAddresses 1 address "192.168.2.7"
    data:    VirtualIPAddresses 1 name "ilbset"
    data:    Network Endpoints 0 localPort 5986
    data:    Network Endpoints 0 name "PowerShell"
    data:    Network Endpoints 0 port 5986
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 3389
    data:    Network Endpoints 1 name "Remote Desktop"
    data:    Network Endpoints 1 port 60173
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 1433
    data:    Network Endpoints 2 name "tcp-1433-1433"
    data:    Network Endpoints 2 port 1433
    data:    Network Endpoints 2 loadBalancerProbe port 1433
    data:    Network Endpoints 2 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 2 loadBalancerProbe intervalInSeconds 300
    data:    Network Endpoints 2 loadBalancerProbe timeoutInSeconds 600
    data:    Network Endpoints 2 protocol "tcp"
    data:    Network Endpoints 2 virtualIPAddress "192.168.2.7"
    data:    Network Endpoints 2 enableDirectServerReturn false
    data:    Network Endpoints 2 loadBalancerName "ilbset"
    info:    vm show command OK


## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a>Bir sanal makine için uzak masaüstü uç noktası oluşturma
`azure vm endpoint create` kullanarak belirli bir sanal makine için genel bağlantı noktasına gelen trafiği yerel bağlantı noktasına yönlendirme amacıyla uzak masaüstü uç noktası oluşturabilirsiniz.

    azure vm endpoint create web1 54580 -k 3389


## <a name="remove-virtual-machine-from-load-balancer"></a>Sanal makineyi yük dengeleyiciden kaldırma
İlgili uç noktayı silerek iç yük dengeleyici kümesinde yer alan bir sanal makineyi kaldırabilirsiniz. Uç nokta kaldırıldığında ilgili sanal makine artık yük dengeleyici kümesine ait olmayacaktır.

 Yukarıdaki örneği kullanarak "DB1" sanal makinesi için oluşturulan uç noktayı "ilbset" iç yük dengeleyici kümesinden `azure vm endpoint delete` komutuyla kaldırabilirsiniz.

    azure vm endpoint delete DB1 tcp-1433-1433


Daha fazla bilgi için bkz. `azure vm endpoint --help`.

## <a name="next-steps"></a>Sonraki adımlar
[Kaynak IP benzeşimi kullanarak yük dengeleyici dağıtım modunu yapılandırma](load-balancer-distribution-mode.md)

[Yük dengeleyiciniz için boşta TCP zaman aşımı ayarlarını yapılandırma](load-balancer-tcp-idle-timeout.md)




<!--HONumber=Nov16_HO2-->


