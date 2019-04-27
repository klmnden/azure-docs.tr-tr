---
title: Bir bulut hizmeti için birden çok VIP
titlesuffix: Azure Load Balancer
description: MultiVIP ve bir bulut hizmetinde birden çok VIP ayarlama genel bakış
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: bf5721e206316a4ce576253743e9ac65de47094a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60591743"
---
# <a name="configure-multiple-vips-for-a-cloud-service"></a>Bir bulut hizmeti için birden çok VIP yapılandırma

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Azure bulut Hizmetleri, Azure tarafından sağlanan bir IP adresi kullanarak, bir genel Internet üzerinden erişebilirsiniz. Bu genel IP adresi için bir VIP (sanal IP) adlandırılır, Azure yük dengeleyiciye bağlı ve bulut hizmetinden değil sanal makine (VM) örnekleri. Bir bulut hizmetindeki herhangi bir VM örneği, tek bir VIP kullanarak erişebilirsiniz.

Ancak, birden fazla VIP olarak bir giriş noktası aynı bulut hizmetine gerektiren senaryolar da vardır. Örneğin, bulut hizmetinizin her site için farklı bir müşteri barındırılan olarak kullanarak varsayılan bağlantı noktası 443, SSL bağlantısı gerektiren veya Kiracı birden fazla Web sitesi barındırabilir. Bu senaryoda, farklı bir genel kullanıma yönelik IP adresi her Web sitesi için olması gerekir. Aşağıdaki diyagram tipik bir çok kiracılı web aynı ortak bağlantı noktasında birden çok SSL sertifikası ihtiyacı olan barındırma gösterilmektedir.

![Birden çok VIP SSL senaryosu](./media/load-balancer-multivip/Figure1.png)

Yukarıdaki örnekte, tüm VIP aynı genel bağlantı noktası (443) kullanın ve trafiği birine yönlendirilen veya daha fazla yük dengeli VM'ler tüm Web sitelerinin barındıran bulut hizmetinin iç IP adresi için benzersiz bir özel bağlantı.

> [!NOTE]
> Birden çok VIP kullanımı gerektiren başka bir durum, aynı küme sanal makinede birden fazla SQL AlwaysOn Kullanılabilirlik grubu dinleyicisi barındırıyor.

VIP'ler, yani bulut hizmetine atanan gerçek IP adresini zaman içinde değişebilir ve varsayılan olarak dinamiktir. Oluşmasını önlemek için hizmetiniz için bir VIP ayırabilirsiniz. Ayrılmış VIP hakkında daha fazla bilgi için bkz: [ayrılmış genel IP](../virtual-network/virtual-networks-reserved-public-ip.md).

> [!NOTE]
> Lütfen [IP adresi fiyatlandırması](https://azure.microsoft.com/pricing/details/ip-addresses/) fiyatlandırması hakkında daha fazla VIP ve ayrılan IP'ler hakkında bilgi için.

Bulut Hizmetleri tarafından kullanılan VIP doğrulamak için PowerShell kullanma yanı sıra ekleyin ve VIP'ler kaldırın, bir uç nokta için bir VIP ilişkilendirmek ve belirli bir VIP yük dengelemeyi yapılandırın.

## <a name="limitations"></a>Sınırlamalar

Şu anda aşağıdaki senaryolar için birden çok VIP işlevsellik sınırlıdır:

* **Iaas yalnızca**. Bu gibi durumlarda, birden çok VIP yalnızca sanal makineleri içeren bulut Hizmetleri için etkinleştirebilirsiniz. Birden çok VIP PaaS rol örnekleri senaryolarıyla kullanamazsınız.
* **Yalnızca PowerShell**. Birden çok VIP yalnızca PowerShell kullanarak da yönetebilirsiniz.

Bu sınırlamalar, geçicidir ve herhangi bir zamanda değişebilir. Gelecekteki değişiklikleri doğrulamak için bu sayfayı yeniden ziyaret etmeniz emin olun.

## <a name="how-to-add-a-vip-to-a-cloud-service"></a>Bir bulut hizmetine bir VIP ekleme
Hizmetinize bir VIP eklemek için aşağıdaki PowerShell komutunu çalıştırın:

```powershell
Add-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService
```

Bu komut, aşağıdaki örneğe benzer bir sonuç gösterir:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Add-AzureVirtualIP   4bd7b638-d2e7-216f-ba38-5221233d70ce Succeeded

## <a name="how-to-remove-a-vip-from-a-cloud-service"></a>Bir bulut hizmetine bir VIP kaldırma
Yukarıdaki örnekte hizmetine eklenen VIP kaldırmak için aşağıdaki PowerShell komutunu çalıştırın:

```powershell
Remove-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService
```

> [!IMPORTANT]
> İlişkili uç nokta varsa, yalnızca bir VIP kaldırabilirsiniz.


## <a name="how-to-retrieve-vip-information-from-a-cloud-service"></a>Bir bulut hizmetinden VIP bilgilerini alma
Bir bulut hizmetiyle ilişkili VIP'ler almak için aşağıdaki PowerShell betiğini çalıştırın:

```powershell
$deployment = Get-AzureDeployment -ServiceName myService
$deployment.VirtualIPs
```

Betik, aşağıdaki örneğe benzer bir sonuç görüntüler:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

Bu örnekte, bulut hizmeti 3 olan VIP'ler:

* **Vıp1** varsayılan VIP, bildiğiniz IsDnsProgrammedName değeri ayarlandığından true.
* **Vıp2** ve **Vip3** , herhangi bir IP adresi olmadığı olarak kullanılmaz. Bir uç noktaya VIP ilişkilendirirseniz yalnızca kullanılır.

> [!NOTE]
> Aboneliğiniz, yalnızca bir uç noktası ile ilişkili olduğunda ek VIP'ler için ücretlendirilirsiniz. Fiyatlandırma hakkında daha fazla bilgi için bkz. [IP adresi fiyatlandırması](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="how-to-associate-a-vip-to-an-endpoint"></a>Bir uç nokta için bir VIP ilişkilendirme

Bir bulut hizmeti bir uç nokta için bir VIP ilişkilendirmek için aşağıdaki PowerShell komutunu çalıştırın:

```powershell
Get-AzureVM -ServiceName myService -Name myVM1 |
    Add-AzureEndpoint -Name myEndpoint -Protocol tcp -LocalPort 8080 -PublicPort 80 -VirtualIPName Vip2 |
    Update-AzureVM
```

Komut adlı VIP için bağlı bir uç nokta oluşturur *vıp2* noktasında *80*ve adlı VM'ye bağlantılar *myVM1* bir bulut hizmetinde adlı *myService* kullanarak *TCP* noktasında *8080*.

Yapılandırmayı doğrulamak için aşağıdaki PowerShell komutunu çalıştırın:

```powershell
$deployment = Get-AzureDeployment -ServiceName myService
$deployment.VirtualIPs
```

Çıktı aşağıdaki örneğe benzer:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         : 191.238.74.13
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

## <a name="how-to-enable-load-balancing-on-a-specific-vip"></a>Belirli bir VIP yük dengelemeyi etkinleştirme

Yük Dengeleme amaçlar için birden çok sanal makine ile tek bir VIP ilişkilendirebilirsiniz. Örneğin, bir bulut hizmeti adlı sahip *myService*ve adlı iki sanal makine *myVM1* ve *myVM2*. Ve bulut hizmetinizi bunlardan birinin adında birden çok VIP *vıp2*. Tüm trafik için bağlantı noktası emin olmak istiyorsanız *81* üzerinde *vıp2* arasında dengeli *myVM1* ve *myVM2* noktasında *8181* , aşağıdaki PowerShell betiğini çalıştırın:

```powershell
Get-AzureVM -ServiceName myService -Name myVM1 |
    Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2 -DefaultProbe |
    Update-AzureVM

Get-AzureVM -ServiceName myService -Name myVM2 |
    Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe |
    Update-AzureVM
```

Farklı bir VIP kullanmak için yük dengeleyicinizi de güncelleştirebilirsiniz. Örneğin, aşağıdaki PowerShell komutu çalıştırırsanız, yük dengeleyici kümesi vıp1 adlı bir VIP kullanmak için değiştirilecektir:

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName myService -LBSetName myLBSet -VirtualIPName Vip1
```

## <a name="next-steps"></a>Sonraki Adımlar

[Azure Yük Dengeleme için Azure izleme günlükleri](load-balancer-monitor-log.md)

[Internet'e yönelik yük dengeleyiciye genel bakış](load-balancer-internet-overview.md)

[E yönelik Yük Dengeleyici Internet'te kullanmaya başlayın](load-balancer-get-started-internet-arm-ps.md)

[Sanal Ağ’a Genel Bakış](../virtual-network/virtual-networks-overview.md)

[Ayrılmış IP REST API'leri](https://msdn.microsoft.com/library/azure/dn722420.aspx)
