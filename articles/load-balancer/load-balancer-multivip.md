---
title: "Bir bulut hizmeti için birden çok VIP'ler"
description: "MultiVIP ve bir bulut hizmetinde birden çok Vip ayarlama genel bakış"
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
ms.assetid: 85f6d26a-3df5-4b8e-96a1-92b2793b5284
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: b6b7b0b2d7a7f33facaf72bbd2d7937364770673
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configure-multiple-vips-for-a-cloud-service"></a>Birden çok Vip bir bulut hizmeti için yapılandırma

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Azure tarafından sağlanan bir IP adresi kullanarak genel Internet üzerinden Azure bulut hizmetlerine erişebilir. Bu genel IP adresi VIP (sanal IP) adlandırılır Azure yük dengeleyiciye bağlı ve sanal makine (VM) bulut hizmetinde örnekleri. Bir bulut hizmeti içinde herhangi bir VM örneğine tek bir VIP kullanarak erişebilirsiniz.

Ancak, bir veya daha çok VIP olarak bir giriş noktası aynı bulut hizmetine gerekebilir senaryo vardır. Örneğin, bulut hizmetinizin her site için farklı bir müşteri barındırılan olarak varsayılan bağlantı noktası 443'ü kullanarak SSL bağlantısı gerektiren veya Kiracı birden çok Web sitesi barındırabilir. Bu senaryoda, farklı genel kullanıma yönelik bir IP adresi her Web sitesi için olması gerekir. Aşağıdaki diyagram tipik bir çok kiracılı web birden çok SSL sertifikaları aynı ortak bağlantı noktasına sahip barındırma gösterir.

![Çoklu VIP SSL senaryosu](./media/load-balancer-multivip/Figure1.png)

Yukarıdaki örnekte, tüm VIP'ler aynı genel bağlantı noktası (443) kullanın ve trafik birine yönlendirilir veya daha fazla yük dengeli sanal makinelerin tüm Web siteleri barındırma bulut hizmetinin iç IP adresi için bir benzersiz özel bağlantı noktası.

> [!NOTE]
> Birden çok Vip kullanımı gerektiren başka bir durum aynı sanal makineler kümesi üzerinde birden çok SQL AlwaysOn Kullanılabilirlik grubu dinleyicileri barındırıyor.

VIP'ler dinamik varsayılan olarak bulut hizmetine atanan gerçek IP adresi zaman içinde değişebilir anlamına gelir. Oluşmasını önlemek için hizmetiniz için bir VIP ayırabilirsiniz. Ayrılmış VIP'ler hakkında daha fazla bilgi için bkz: [ayrılmış genel IP](../virtual-network/virtual-networks-reserved-public-ip.md).

> [!NOTE]
> Lütfen bakın [IP adresi fiyatlandırma](https://azure.microsoft.com/pricing/details/ip-addresses/) üzerinde VIP'ler fiyatlandırma ve ayrılmış IP hakkında bilgi.

Bulut hizmetlerinizi tarafından kullanılan VIP'ler doğrulamak için PowerShell kullanın yanı sıra ekleyin ve VIP'ler kaldırmak, bir uç nokta için bir VIP ilişkilendirmek ve Yük Dengeleme özgü bir VIP yapılandırın.

## <a name="limitations"></a>Sınırlamalar

Şu anda aşağıdaki senaryolar için çoklu VIP işlevselliği sınırlıdır:

* **Iaas yalnızca**. Bu gibi durumlarda, çoklu VIP yalnızca VM içermesi için bulut hizmetlerini etkinleştirebilirsiniz. Çoklu VIP PaaS rol örnekleri senaryolarla kullanamazsınız.
* **Yalnızca PowerShell**. PowerShell kullanarak yalnızca çoklu VIP yönetebilirsiniz.

Bu sınırlamaların geçicidir ve herhangi bir zamanda değişebilir. Gelecekteki değişiklikleri doğrulamak için bu sayfayı yeniden ziyaret etmeniz emin olun.

## <a name="how-to-add-a-vip-to-a-cloud-service"></a>Bir bulut hizmeti için bir VIP ekleme
Bir VIP hizmetinize eklemek için aşağıdaki PowerShell komutunu çalıştırın:

```powershell
Add-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService
```

Bu komut, aşağıdaki örneğe benzer bir sonuç görüntüler:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Add-AzureVirtualIP   4bd7b638-d2e7-216f-ba38-5221233d70ce Succeeded

## <a name="how-to-remove-a-vip-from-a-cloud-service"></a>Bir bulut hizmetinden bir VIP kaldırma
Yukarıdaki örnekte hizmetinizi eklenen VIP kaldırmak için aşağıdaki PowerShell komutunu çalıştırın:

```powershell
Remove-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService
```

> [!IMPORTANT]
> Kendisine ilişkili hiçbir uç noktası varsa, yalnızca bir VIP kaldırabilirsiniz.


## <a name="how-to-retrieve-vip-information-from-a-cloud-service"></a>Bir bulut hizmetinden VIP bilgi alma
Bir bulut hizmetiyle ilişkili VIP'ler almak için aşağıdaki PowerShell betiğini çalıştırın:

```powershell
$deployment = Get-AzureDeployment -ServiceName myService
$deployment.VirtualIPs
```

Komut dosyasını aşağıdaki örneğe benzer bir sonuç görüntüler:

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

* **Vıp1** varsayılan VIP olup, bildiğiniz IsDnsProgrammedName değeri ayarlanamıyor çünkü true.
* **Vıp2** ve **Vip3** herhangi bir IP adresi yok olarak kullanılmaz. VIP için bir uç nokta ilişkilendirirseniz yalnızca kullanılır.

> [!NOTE]
> Bir uç nokta ile ilişkili olduğunda, aboneliğiniz için ek VIP'ler yalnızca ücretlendirilir. Fiyatlandırma hakkında daha fazla bilgi için bkz: [IP adresi fiyatlandırma](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="how-to-associate-a-vip-to-an-endpoint"></a>Bir uç nokta için bir VIP ilişkilendirme

Bir bulut hizmeti için bir bitiş noktasına bir VIP ilişkilendirmek için aşağıdaki PowerShell komutunu çalıştırın:

```powershell
Get-AzureVM -ServiceName myService -Name myVM1 |
    Add-AzureEndpoint -Name myEndpoint -Protocol tcp -LocalPort 8080 -PublicPort 80 -VirtualIPName Vip2 |
    Update-AzureVM
```

Komutu adlı VIP bağlı bir uç nokta oluşturuyor *vıp2* bağlantı noktasında *80*ve adlı VM'ye bağlantılar *myVM1* bir bulut hizmetinde adlı *myService* kullanarak *TCP* bağlantı noktasında *8080*.

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

## <a name="how-to-enable-load-balancing-on-a-specific-vip"></a>Yük Dengeleme özgü bir VIP etkinleştirme

Yük Dengeleyici amaçları için birden çok sanal makine tek bir VIP ilişkilendirebilirsiniz. Örneğin, adlandırılmış bir bulut hizmetine sahip *myService*ve adlı iki sanal makine *myVM1* ve *myVM2*. Ve bunların adlı birden çok Vip bulut hizmetiniz sahip *vıp2*. Tüm bağlantı noktasına trafiği emin olmak istiyorsanız *81* üzerinde *vıp2* arasında dengeli *myVM1* ve *myVM2* bağlantı noktasında *8181* , aşağıdaki PowerShell betiğini çalıştırın:

```powershell
Get-AzureVM -ServiceName myService -Name myVM1 |
    Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2 -DefaultProbe |
    Update-AzureVM

Get-AzureVM -ServiceName myService -Name myVM2 |
    Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe |
    Update-AzureVM
```

Ayrıca, farklı bir VIP kullanmak için yük dengeleyici güncelleştirebilirsiniz. Örneğin, aşağıdaki PowerShell komutu çalıştırırsanız, Yük Dengeleme vıp1 adlı bir VIP kullanılacak kümesi değişir:

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName myService -LBSetName myLBSet -VirtualIPName Vip1
```

## <a name="next-steps"></a>Sonraki Adımlar

[Azure Yük Dengeleme için günlük analizi](load-balancer-monitor-log.md)

[Internet kullanıma yönelik Yük Dengeleyici genel bakış](load-balancer-internet-overview.md)

[Yük Dengeleyici Internet'e üzerinde çalışmaya başlama](load-balancer-get-started-internet-arm-ps.md)

[Sanal ağ genel bakış](../virtual-network/virtual-networks-overview.md)

[Ayrılmış IP REST API'leri](https://msdn.microsoft.com/library/azure/dn722420.aspx)
