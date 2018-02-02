---
title: "Azure - PowerShell sanal ağ oluşturma | Microsoft Docs"
description: "Hızlı bir şekilde PowerShell kullanarak bir sanal ağ oluşturmayı öğrenin. Bir sanal ağ türlerde özel olarak birbirleri ile iletişim kurmak için Azure kaynaklarını sağlar."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: 
ms.topic: 
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 01/25/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 091e7e6cabf325cdd9d4289e7d22e71c583d91db
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="create-a-virtual-network-using-powershell"></a>PowerShell kullanarak bir sanal ağ oluşturma

Bu makalede, bir sanal ağ oluşturmayı öğrenin. Bir sanal ağ oluşturduktan sonra iki sanal makineye sanal ağa dağıtmak ve özel olarak aralarında iletişim.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-powershell.md)]

Yüklemek ve PowerShell yerel olarak kullanmak seçerseniz, bu öğreticide Azure PowerShell modülü sürümü 5.1.1 gerektirir veya sonraki bir sürümü. Yüklü olan sürümü bulmak için Çalıştır ` Get-Module -ListAvailable AzureRM`. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup) ile yeni bir Azure kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur. Tüm Azure kaynakları bir Azure konumu (veya bölge) içinde oluşturulur.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Bir sanal ağ ile oluşturma [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). Aşağıdaki örnek adlı bir varsayılan sanal ağ oluşturur *myVirtualNetwork* içinde *EastUS* konumu:

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/24
```

Tüm sanal ağları atanmış bir veya daha fazla adres öneklerini vardır. Adres alanı CIDR gösteriminde belirtilir. 10.0.0.0/24 adres alanı 10.0.0.0-10.0.0.254 kapsar. Sanal ağlar bunları içindeki sıfır veya daha fazla alt ağlara sahip. Kaynakları bir sanal ağdaki bir alt ağ içinde dağıtılır. 

Bir alt ağ yapılandırması ile oluşturma [yeni AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Tüm alt ağlar sanal ağın adres ön ekiyle mevcut bir adres öneki sahip. Bu örnekte, bir alt ağ yapılandırması sanal ağın adres ön eki aynı adres ön eki ile oluşturulur:

```azurepowershell-interactive
$subnetConfig = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name default `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork
```

Alt ağ adresi öneki 10.0.0.0-10.0.0.254 kapsayan olsa, yalnızca adresleri 10.0.0.4-10.0.0.254 kullanılabilir, Azure ilk dört adresler (0-3), her alt ağda son adresi ayırdığından. Yalnızca bir alt ağ, alt ağ adresi öneki sanal ağ adres ön eki aynı olduğundan, bu sanal ağda var olabilir.

Sanal ağ ile alt ağ yapılandırması yazma [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork), alt ağı oluşturur:

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Bir sanal ağ çeşitli özel olarak birbirleri ile iletişim kurmak için Azure kaynaklarını sağlar. Tek bir sanal ağa dağıttığınız kaynak türü, bir sanal makinedir. Doğrulayın ve daha sonraki bir adımda bir sanal ağdaki sanal makineler arasındaki iletişimi nasıl çalıştığını anlamak için iki sanal makineye sanal ağ oluşturun.

Bir sanal makine oluşturma [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Bu adımı çalıştırırken kimlik bilgileri istenir. Girdiğiniz değerler, sanal makinenin kullanıcı adı ve parolası olarak yapılandırılır. Bir sanal makine oluşturulur konumu sanal ağ içinde aynı konumda olmalıdır. Bu makalede olmasına rağmen sanal makinenin sanal makine ile aynı kaynak grubunda olması gerekli değildir. `-AsJob` Parametresi komutu ile sonraki görev devam edebilmek için arka planda çalışmaya izin verir.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "default" `
    -Name "myVm1" `
    -AsJob
```

Aşağıdaki örnek çıkış benzer bir çıktı döndürülür ve Azure arka planda sanal makine oluşturmaya başlar.

```powershell
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command                  
--     ----            -------------   -----         -----------     --------             -------                  
1      Long Running... AzureLongRun... Running       True            localhost            New-AzureRmVM     
```

Azure DHCP otomatik olarak atar 10.0.0.4 sanal makine oluşturma sırasında listedeki ilk kullanılabilir adres olduğundan *varsayılan* alt ağ.

İkinci bir sanal makine oluşturun. 

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "default" `
  -Name "myVm2"
```
Sanal makine oluşturmak için birkaç dakika sürer. Oluşturulan sanal makine çıktısı oluşturduktan sonra Azure döndürür. Azure olmayan döndürülen çıktıda atanan ancak *10.0.0.5* için *myVm2* sanal makine, bir sonraki kullanılabilir adres alt ağda olduğundan.

## <a name="connect-to-a-virtual-machine"></a>Bir sanal makineye bağlanma

Kullanım [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) ortak IP adresi, bir sanal makinenin döndürülecek komutu. Azure genel, bir Internet yönlendirilebilir IP adresi her bir sanal makine için varsayılan olarak atar. Genel IP adresi, sanal makineden atanmış bir [her bir Azure bölgesine atanan adresler havuzuna](https://www.microsoft.com/download/details.aspx?id=41653). Hangi ortak IP adresi bir sanal makineye atanan Azure bilir olsa da, bir sanal makinede çalışan işletim sistemi atanmış tüm ortak IP adresinin hiçbir şeyin sahiptir. Aşağıdaki örnek genel IP adresi döndürür *myVm1* sanal makine:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress -Name myVm1 -ResourceGroupName myResourceGroup | Select IpAddress
```

İle Uzak Masaüstü oturumu oluşturmak için aşağıdaki komutu kullanın *myVm1* yerel bilgisayarınızdan sanal makine. Değiştir `<publicIpAddress>` IP adresi ile döndürülen önceki komutu.

```
mstsc /v:<publicIpAddress>
```

Bir Uzak Masaüstü Protokolü (.rdp) dosyası oluşturulur, bilgisayarınıza indirilmeden ve açılır. Kullanıcı adı ve sanal makine oluştururken belirttiğiniz parolayı girin ve ardından **Tamam**. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıya devam etmek için **Evet** veya **Devam**’a tıklayın.

## <a name="validate-communication"></a>İletişim doğrula

Ping varsayılan olarak Windows Güvenlik Duvarı aracılığıyla izin verilmediğinden bir Windows sanal makine başarısız ping çalışılıyor. Ping işlemine izin vermek için *myVm1*, bir komut isteminden aşağıdaki komutu girin:

```
netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
```

İle iletişim doğrulamak için *myVm2*, bir komut isteminden aşağıdaki komutu girin *myVm1* sanal makine. Sanal makine oluştururken kullandığınız kimlik bilgilerini sağlayın ve ardından bağlantıyı tamamlayın:

```
mstsc /v:myVm2
```

Her iki sanal makine gelen atanan özel IP adresleri olduğundan Uzak Masaüstü bağlantısı başarılı *varsayılan* alt ağı ve Uzak Masaüstü varsayılan olarak Windows Güvenlik Duvarı üzerinden açık olduğundan. Bağlanabilmeleri için *myVm2* ana bilgisayar adı tarafından çünkü Azure otomatik olarak bir sanal ağ içindeki tüm ana bilgisayarlar için DNS ad çözümlemesi sağlar. Bir komut isteminden ping my *myVm1*, gelen *myVm2*.

```
ping myvm1
```

Ping işlemi başarılı, onu Windows Güvenlik Duvarı aracılığıyla izin *myVm1* bir önceki adımda sanal makine. İnternet giden iletişim onaylamak için aşağıdaki komutu girin:

```
ping bing.com
```

Aratıp dört yanıt alırsınız. Varsayılan olarak, herhangi bir sanal makine bir sanal ağdaki internet giden iletişim kurabilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olduğunda, kullanabileceğiniz [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) kaynak grubu ve içerdiği kaynakların tümünü kaldırmak için komutu. Uzak Masaüstü oturumu çıkın ve ardından bilgisayarınızdan kaynak grubunu silmek için aşağıdaki komutu çalıştırın:

```azurepowershell-interactive 
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir varsayılan sanal ağ bir alt ağı ve iki sanal makine ile dağıtılabilir. Birden çok alt ağ ile özel bir sanal ağ oluşturma ve temel sanal ağ yönetim görevlerini gerçekleştirme hakkında bilgi almak için özel bir sanal ağ oluşturma ve yönetmeyi öğretici devam edin.


> [!div class="nextstepaction"]
> [Özel bir sanal ağ oluşturma ve yönetme](virtual-networks-create-vnet-arm-pportal.md#powershell)
