---
title: Bir iç yük dengeleyicisi oluşturma - PowerShell Klasik
titlesuffix: Azure Load Balancer
description: Klasik dağıtım modelinde PowerShell kullanarak iç yük dengeleyici oluşturmayı öğrenin
services: load-balancer
documentationcenter: na
author: genlin
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms:custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: genli
ms.openlocfilehash: ef6aac0d97c38798f826304475779ea8059875c7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60848556"
---
# <a name="get-started-creating-an-internal-load-balancer-classic-using-powershell"></a>PowerShell kullanarak iç yük dengeleyici (klasik) oluşturmaya başlama

> [!div class="op_single_selector"]
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-classic-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-classic-cli.md)
> * [Bulut hizmetleri](../load-balancer/load-balancer-get-started-ilb-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!IMPORTANT]
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md).  Bu makale klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. [Bu adımları Resource Manager modeli kullanarak gerçekleştirmeyi](load-balancer-get-started-ilb-arm-ps.md) öğrenin.

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-internal-load-balancer-set-for-virtual-machines"></a>Sanal makineler için iç yük dengeleyici oluşturma

İç yük dengeleyici kümesi ve trafiğini bu kümeye gönderecek sunucuları oluşturmak için aşağıdaki adımları uygulamanız gerekir:

1. Gelen trafiğin uç noktası olacak ve bu trafiğe yük dengeli bir kümenin sunucularında yük dengelemesi yapacak bir İç Yük Dengeleme örneği oluşturun.
2. Gelen trafiği alacak sanal makinelere karşılık gelen uç noktalar ekleyin.
3. Yük dengelemesi yapılacak trafiği gönderecek sunucuları trafiklerini İç Yük Dengeleme örneğinin sanal IP (VIP) adresine gönderecek şekilde yapılandırın.

### <a name="step-1-create-an-internal-load-balancing-instance"></a>1. Adım: İç Yük Dengeleme örneği oluşturun

Mevcut bir bulut hizmeti veya bölgesel sanal ağ üzerine dağıtılmış bulut hizmeti için aşağıdaki Windows PowerShell komutlarını kullanarak İç Yük Dengeleme örneğin oluşturabilirsiniz:

```powershell
$svc="<Cloud Service Name>"
$ilb="<Name of your ILB instance>"
$subnet="<Name of the subnet within your virtual network>"
$IP="<The IPv4 address to use on the subnet-optional>"

Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP
```

Bu [Add-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx) Windows PowerShell cmdlet örneğinde DefaultProbe parametre kümesi kullanıldığına dikkat edin. Ek parametre kümeleri hakkında daha fazla bilgi için bkz. [Add-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx).

### <a name="step-2-add-endpoints-to-the-internal-load-balancing-instance"></a>2. Adım: İç Yük Dengeleme örneğine uç noktaları ekleyin

Örnek aşağıda verilmiştir:

```powershell
$svc="mytestcloud"
$vmname="DB1"
$epname="TCP-1433-1433"
$lbsetname="lbset"
$prot="tcp"
$locport=1433
$pubport=1433
$ilb="ilbset"
Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -Lbset $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM
```

### <a name="step-3-configure-your-servers-to-send-their-traffic-to-the-new-internal-load-balancing-endpoint"></a>3. Adım: Sunucularınızı trafiklerini yeni iç Yük Dengeleme uç noktasına gönderecek şekilde yapılandırın

Yük dengelemesi uygulanacak trafiğe sahip sunucuları İç Yük Dengeleme örneğinin yeni IP adresini (VIP) kullanacak şekilde yapılandırmanız gerekir. Bu, İç Yük Dengeleme örneğinin dinlediği adrestir. Çoğu durumda tek yapmanız gereken İç Yük Dengeleme örneği VIP’sinin DNS kaydını yapmak veya değiştirmektir.

İç Yük Dengeleme örneğini oluştururken IP adresi belirttiyseniz VIP’ye sahipsiniz demektir. Belirtmediyseniz VIP’yi görmek için aşağıdaki komutları kullanabilirsiniz:

```powershell
$svc="<Cloud Service Name>"
Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer
```

Bu komutları kullanmak için istenen değerleri yazıp < ve > işaretlerini silin. Örnek aşağıda verilmiştir:

```powershell
$svc="mytestcloud"
Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer
```

Get-AzureInternalLoadBalancer komutu ekranındaki IP adresini not edin ve trafiğin VIP’ye gönderildiğinden emin olmak için sunucularınızda veya DNS kayıtlarınızda gerekli değişiklikleri yapın.

> [!NOTE]
> Microsoft Azure platformu, farklı yönetim senaryoları için genel olarak yönlendirilebilen statik bir IPv4 adresi kullanır. IP adresi: 168.63.129.16. Bu IP adresinin güvenlik duvarları tarafından engellenmesi beklenmeyen davranışlara neden olabilir.
> Azure İç Yük Dengeleme açısından bu IP adresi, araştırmaların yük dengeleyiciden izlenerek yük dengeli kümede yer alan sanal makinelerin durumunun belirlenmesini sağlar. Bir iç yük dengeli kümedeki Azure sanal makinelerine gelen trafiği kısıtlamak için bir Ağ Güvenlik Grubu kullanılması veya Sanal Alt Ağ uygulanması halinde 168.63.129.16 adresinden gelen trafiğe izin verecek şekilde bir Ağ Güvenlik Kuralının uygulandığından emin olun.

## <a name="example-of-internal-load-balancing"></a>İç yük dengeleme örneği

İki örnek yapılandırma için yük dengeli küme oluşturmayla ilgili adım adım talimatlar için aşağıdaki bölümlere bakın.

### <a name="an-internet-facing-multi-tier-application"></a>İnternet'e yönelik, çok katmanlı uygulama

İnternet’e yönelik web sunucusu kümesi için yük dengeli veritabanı hizmeti sunmak istiyorsunuz. İki sunucu kümesi de tek bir Azure bulut hizmetinde. 1433 numaralı TCP bağlantı noktasına gelen web sunucusu trafiğinin veritabanı katmanındaki iki sanal makineye dağıtılması gerekiyor. Yapılandırma Şekil 1’de gösterilmektedir.

![Veritabanı katmanı için iç yük dengeli küme](./media/load-balancer-internal-getstarted/IC736321.png)

Yapılandırma şunları içerir:

* Sanal makineleri barındıran mevcut bulut hizmeti mytestcloud olarak adlandırılmıştır.
* Mevcut iki veritabanı sunucusu DB1 ve DB2 olarak adlandırılmıştır.
* Web katmanındaki web sunucuları, veritabanı katmanındaki veritabanı sunucularına özel IP adresini kullanarak bağlanmaktadır. Diğer bir seçenek de sanal ağ için kendi DNS sunucunuzu kullanmak ve iç yük dengeleyici kümesi için A kaydı oluşturmaktır.

Aşağıdaki komutlar **ILBset** adlı yeni bir İç Yük Dengeleme örneği yapılandırır ve iki veritabanına karşılık gelen sanal makinelere uç noktalar ekler:

```powershell
$svc="mytestcloud"
$ilb="ilbset"
Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb
$prot="tcp"
$locport=1433
$pubport=1433
$epname="TCP-1433-1433"
$lbsetname="lbset"
$vmname="DB1"
Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM

$epname="TCP-1433-1433-2"
$vmname="DB2"
Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM
```

## <a name="remove-an-internal-load-balancing-configuration"></a>İç Yük Dengeleme yapılandırmasını kaldırma

Bir sanal makineyi iç yük dengeleme örneğinin uç noktası olmaktan çıkarmak için şu komutları kullanın:

```powershell
$svc="<Cloud service name>"
$vmname="<Name of the VM>"
$epname="<Name of the endpoint>"
Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM
```

Bu komutları kullanmak için istenen değerleri yazıp < ve > işaretlerini silin.

Örnek aşağıda verilmiştir:

```powershell
$svc="mytestcloud"
$vmname="DB1"
$epname="TCP-1433-1433"
Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM
```

Bir bulut hizmetindeki iç yük dengeleyici örneğini kaldırmak için şu komutları kullanın:

```powershell
$svc="<Cloud service name>"
Remove-AzureInternalLoadBalancer -ServiceName $svc
```

Bu komutları kullanmak için istenen değerleri yazıp < ve > işaretlerini silin.

Örnek aşağıda verilmiştir:

```powershell
$svc="mytestcloud"
Remove-AzureInternalLoadBalancer -ServiceName $svc
```

## <a name="additional-information-about-internal-load-balancer-cmdlets"></a>İç yük dengeleyici cmdlet'leri hakkında ek bilgi

İç Yük Dengeleyici cmdlet’leri hakkında ek bilgi almak için Windows PowerShell komut isteminde şu komutları çalıştırın:

```powershell
Get-Help New-AzureInternalLoadBalancerConfig -full
Get-Help Add-AzureInternalLoadBalancer -full
Get-Help Get-AzureInternalLoadbalancer -full
Get-Help Remove-AzureInternalLoadBalancer -full
```

## <a name="next-steps"></a>Sonraki adımlar

[Kaynak IP benzeşimi kullanarak yük dengeleyici dağıtım modunu yapılandırma](load-balancer-distribution-mode.md)

[Yük dengeleyiciniz için boşta TCP zaman aşımı ayarlarını yapılandırma](load-balancer-tcp-idle-timeout.md)

