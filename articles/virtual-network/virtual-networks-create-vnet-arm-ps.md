---
title: "Birden çok alt - PowerShell ile bir Azure sanal ağ oluşturma | Microsoft Docs"
description: "PowerShell kullanarak birden çok alt ağı ile bir sanal ağ oluşturmayı öğrenin."
services: virtual-network
documentationcenter: 
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/01/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: c90bdc9381bf98e5c1457e8e28f74105227d8f8d
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="create-a-virtual-network-with-multiple-subnets-using-powershell"></a>PowerShell kullanarak birden çok alt ağı ile bir sanal ağ oluşturma

Bir sanal ağ Internet ile ve özel olarak birbirleriyle iletişim kurmak için Azure kaynaklarını çeşitli türleri sağlar. Bir sanal ağ içinde birden fazla alt ağ oluşturmak, böylece filtre uygulayabilirsiniz ağınız segmentlere ayırmak veya denetim alt ağlar arasındaki trafik akışını sağlar. Bu makalede, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Sanal ağ oluşturma
> * Alt ağ oluşturma
> * Sanal makineler arasındaki ağ iletişimi test

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Yüklemek ve PowerShell yerel olarak kullanmak seçerseniz, bu öğreticide Azure PowerShell modülü sürüm 3,6 veya üstü gerektirir. Çalıştırma ` Get-Module -ListAvailable AzureRM` yüklü olan sürümü bulunamıyor. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir. 

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Bir kaynak grubu ile oluşturmak [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Aşağıdaki örnek, bir kaynak grubu oluşturur *myResourceGroup* içinde *EastUS* konumu.

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

[New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) ile sanal ağ oluşturun. Aşağıdaki örnek adlı bir sanal ağ oluşturur *myVirtualNetwork* adres ön ekine sahip *10.0.0.0/16*.

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

**AddressPrefix** CIDR gösteriminde belirtilir. Belirtilen adres ön eki IP adreslerini 10.0.0.0-10.0.255.254 içerir.

## <a name="create-a-subnet"></a>Alt ağ oluşturma

Bir alt ağ, ilk alt ağ yapılandırması oluşturma ve ardından sanal ağ alt ağ yapılandırması ile güncelleştirme tarafından oluşturulur. Bir alt ağ yapılandırması ile oluşturma [yeni AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Aşağıdaki örnek için iki alt ağ yapılandırmalarını oluşturur *ortak* ve *özel* alt ağlar:

```azurepowershell-interactive
$subnetConfigPublic = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Public `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork

$subnetConfigPrivate = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Private `
  -AddressPrefix 10.0.1.0/24 `
  -VirtualNetwork $virtualNetwork
```

**AddressPrefix** belirtilen bir alt sanal ağ için tanımlanan adres ön eki içinde olmalıdır. Azure DHCP, IP adresleri bir alt ağda dağıtılan kaynak ait bir alt ağ adres öneklerini atar. Azure yalnızca atar adresleri 10.0.0.4-10.0.0.254 içinde dağıtılan kaynaklara **ortak** alt ağ, Azure ilk dört adresleri ayırdığından (Bu örnekte alt ağ için 10.0.0.0-10.0.0.3) ve son adres () Bu örnekte alt ağ için 10.0.0.255) her alt ağda.

Sanal ağ ile alt ağ yapılandırmalarını yazmak [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork), sanal ağ alt ağlar oluşturur:

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

Azure sanal ağlar ve alt ağları üretim kullanımı için dağıtmadan önce kapsamlı bir adres alanıyla öğrenmeniz olduğunu önerilir [konuları](manage-virtual-network.md#create-a-virtual-network) ve [sanal ağ sınırları](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Kaynakların alt dağıtıldığında, bazı sanal ağ ve adres aralıkları değiştirme gibi alt ağ değişiklikleri içindeki alt ağlara dağıtılan var olan Azure kaynak çözümünüzün yeniden dağıtımını gerektirebilir.

## <a name="test-network-communication"></a>Test ağ iletişimi

Bir sanal ağ Internet ile ve özel olarak birbirleriyle iletişim kurmak için Azure kaynaklarını çeşitli türleri sağlar. Tek bir sanal ağa dağıttığınız kaynak türü, bir sanal makinedir. İki sanal makine sanal ağ oluşturun, bir sonraki adımda bunları ve Internet arasındaki ağ iletişimi test edebilirsiniz. 

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Bir sanal makine oluşturma [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Aşağıdaki örnek, bir sanal makine adlı oluşturur *myVmWeb* içinde *ortak* alt *myVirtualNetwork* sanal ağ. `-AsJob` Seçeneği bir sonraki adıma devam etmek için bu sanal makine arka planda oluşturur. İstendiğinde, kullanıcı adı ve sanal makine ile oturum açmak için istediğiniz parolayı girin.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "Public" `
    -ImageName "Win2016Datacenter" `
    -Name "myVmWeb" `
    -AsJob
```

Azure otomatik olarak atanan 10.0.0.4 sanal makinenin özel IP adresi 10.0.0.4 ilk kullanılabilir IP adresi olduğundan *ortak* alt ağ. 

Bir sanal makine oluşturmak *özel* alt ağ.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "Private" `
    -ImageName "Win2016Datacenter" `
    -Name "myVmMgmt"
```

Sanal makine oluşturmak için birkaç dakika sürer. 10.0.1.4 ilk kullanılabilir IP adresi olduğundan döndürülen çıkış Azure 10.0.1.4 sanal makinenin özel IP adresi atanmamış olsa *özel* alt *myVirtualNetwork*. 

Kalan adımlar ile sanal makine oluşturulur ve PowerShell çıkışı döndürür kadar devam etmeyin.

Bu makalede oluşturulan sanal makineler bir tane [ağ arabirimi](virtual-network-network-interface.md) dinamik olarak ağ arabirimine atanmış bir IP adresine sahip. VM dağıtıldıktan sonra şunları yapabilirsiniz [birden çok ortak ve özel IP adreslerini ekleyin ya da statik IP adresi ataması yöntemi Değiştir](virtual-network-network-interface-addresses.md#add-ip-addresses). Yapabilecekleriniz [ağ arabirimlerini ekleyin](virtual-network-network-interface-vm.md#vm-add-nic), desteklediği sınırına kadar [VM boyutu](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) bir sanal makine oluşturduğunuzda seçin. Ayrıca [tek köklü g/ç Sanallaştırması (SR-IOV) etkinleştir](create-vm-accelerated-networking-powershell.md) bir VM, ancak yalnızca özelliğini destekleyen bir VM boyutu ile bir VM oluşturun.

### <a name="communicate-between-virtual-machines-and-with-the-internet"></a>Sanal makineler arasında ve internet ile iletişim

Bir sanal makineye ait genel IP adresi için Internet'ten bağlanabilir. Azure oluşturduğunuzda *myVmMgmt* sanal makine, adlandırılmış bir ortak IP adresi *myVmMgmt* de oluşturulur ve sanal makineye atanmış. Bir sanal makine kendisine atanmış bir ortak IP adresi olması gerekli değildir ancak Azure varsayılan oluşturmak her bir sanal makine bir ortak IP adresi atar. Bir sanal makineye Internet üzerinden iletişim kurmak için bir ortak IP adresi sanal makineyi atanması gerekir. Bir ortak IP adresi sanal makineye atanmış olup olmadığına bakılmaksızın tüm sanal makinelerin giden Internet ile iletişim kurabilir. Azure'a giden Internet bağlantıları hakkında daha fazla bilgi için bkz: [azure'da giden bağlantılar](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

Kullanım [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) ortak IP adresi, bir sanal makinenin dönün. Aşağıdaki örnek genel IP adresi döndürür *myVmMgmt* sanal makine:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmMgmt `
  -ResourceGroupName myResourceGroup | Select IpAddress
```

İle Uzak Masaüstü oturumu oluşturmak için aşağıdaki komutu kullanın *myVmMgmt* yerel bilgisayarınızdan sanal makine. Değiştir `<publicIpAddress>` IP adresi ile döndürülen önceki komutu.

```
mstsc /v:<publicIpAddress>
```

Bir Uzak Masaüstü Protokolü (.rdp) dosyası oluşturulur, bilgisayarınıza indirilmeden ve açılır. Kullanıcı adı ve sanal makine oluştururken belirttiğiniz parolayı girin (seçmek için gerek duyabileceğiniz **daha fazla seçenek**, ardından **farklı bir hesap kullan**, ne zaman girdiğiniz kimlik bilgileri belirtmek için sanal makine oluşturulan) ve ardından **Tamam**. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıya devam etmek için **Evet** veya **Devam**’a tıklayın. 

Bir sonraki adımda ping ile iletişim kurmak için kullanılan *myVmMgmt* sanal makineden *myVmWeb* sanal makine. Varsayılan olarak Windows Güvenlik Duvarı üzerinden engellenir ICMP ping komutunu kullanır. ICMP, bir komut isteminden aşağıdaki komutu girerek Windows Güvenlik Duvarı aracılığıyla etkinleştirin:

```
netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
```

Bu makalede ping kullanılsa da ICMP üretim dağıtımları için Windows Güvenlik Duvarı aracılığıyla izin vererek önerilmez.

Güvenlik nedenleriyle, sanal makinelere uzaktan sanal bir ağa bağlanabilir sayısını sınırlamak için yaygın bir sorundur. Bu öğreticide *myVmMgmt* yönetmek için kullanılan sanal makine *myVmWeb* sanal ağdaki sanal makine. Uzak Masaüstü için aşağıdaki komutu kullanın *myVmWeb* sanal makineden *myVmMgmt* sanal makine:

``` 
mstsc /v:myVmWeb
```

İçin iletişim kurmak için *myVmMgmt* sanal makineden *myVmWeb* sanal makine, bir komut isteminden aşağıdaki komutu girin:

```
ping myvmmgmt
```

Aşağıdaki örnek çıkış benzer bir çıktı alırsınız:
    
```
Pinging myvmmgmt.dar5p44cif3ulfq00wxznl3i3f.bx.internal.cloudapp.net [10.0.1.4] with 32 bytes of data:
Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
    
Ping statistics for 10.0.1.4:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```
      
Görebilirsiniz adresini *myVmMgmt* sanal makinedir 10.0.1.4. 10.0.1.4 adres aralığı içinde ilk kullanılabilir IP adresi olan *özel* dağıttığınız alt *myVmMgmt* sanal makineye bir önceki adımda.  Sanal makinenin tam etki alanı adı olup olmadığını *myvmmgmt.dar5p44cif3ulfq00wxznl3i3f.bx.internal.cloudapp.net*. Ancak *dar5p44cif3ulfq00wxznl3i3f* etki alanı adı bölümü, sanal makine için farklı olduğundan, etki alanı adını Kalan bölümleri aynıdır. Varsayılan olarak, tüm Azure sanal makineleri varsayılan Azure DNS hizmeti kullanın. Bir sanal ağ içindeki tüm sanal makineleri Azure'nın varsayılan DNS hizmeti ile aynı sanal ağdaki diğer tüm sanal makinelerin adlarını çözümleyebilir. Azure'nın varsayılan DNS hizmeti kullanmak yerine, kendi DNS sunucusu veya Azure DNS hizmeti, özel etki alanı özelliği kullanabilirsiniz. Ayrıntılar için bkz [kendi DNS sunucu kullanılarak ad çözümleme](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) veya [kullanarak Azure DNS özel etki alanları için](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Windows Server için Internet Information Services (IIS) yüklemek için *myVmWeb* sanal makine, bir PowerShell oturumunda aşağıdaki komutu girin:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

IIS yüklemesi tamamlandıktan sonra bağlantı kesme *myVmWeb* size bırakır Uzak Masaüstü oturumu *myVmMgmt* Uzak Masaüstü oturumu. Bir web tarayıcısı açın ve http://myvmweb için göz atın. Karşılama sayfası IIS bakın.

Bağlantı kesme *myVmMgmt* Uzak Masaüstü oturumu.

Azure için atanan ortak adres Al *myVmWeb* sanal makine:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmWeb `
  -ResourceGroupName myResourceGroup | Select IpAddress
```

Genel IP adresi için kendi bilgisayarınızda Gözat *myVmWeb* sanal makine. Kendi bilgisayardan IIS Hoş Geldiniz sayfasını görüntülemek için denemesi başarısız olur. Sanal makineler dağıtıldığında, bu Azure varsayılan olarak her bir sanal makine için bir ağ güvenlik grubu oluşturduğundan denemesi başarısız olur. 

Ağ güvenlik grubu izin veren veya reddeden IP adresi ve bağlantı noktasına gelen ve giden ağ trafiğinin güvenlik kuralları içerir. Oluşturulan Azure varsayılan ağ güvenlik grubu, aynı sanal ağdaki kaynakları arasındaki tüm bağlantı noktaları üzerinden iletişim sağlar. Windows sanal makineler için varsayılan ağ güvenlik grubu tüm gelen trafiği tüm bağlantı noktaları üzerinden Internet'ten engellediği, TCP bağlantı noktası 3389 (RDP) kabul edin. Sonuç olarak, varsayılan olarak, aynı zamanda RDP doğrudan yapabilecekleriniz *myVmWeb* sanal makine Internet'ten gelen değil isteyebilirsiniz olsa bile 3389 açık bir web sunucusuna bağlantı noktası. Bağlantı noktası 80 üzerinden trafiğe izin varsayılan ağ güvenlik grubu kural olduğundan iletişim bağlantı noktası 80 üzerinden iletişim kurar Web'e gözatma olduğundan, Internet'ten başarısız olur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerektiğinde kullanmak [Remove-AzureRmResourcegroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) kaynak grubu ve içerdiği kaynakların tümünü kaldırmak için.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir sanal ağ birden fazla alt ağı dağıtmayı öğrendiniz. Ayrıca, Windows sanal makine oluşturduğunuzda, Azure'nın, sanal makineye ekler ve yalnızca trafiğe 3389, bağlantı noktası üzerinden Internet'ten izin veren bir ağ güvenlik grubu oluşturur bir ağ arabirimi oluşturur öğrendiniz. Ağ trafiği alt ağlara yerine tek tek sanal makineleri için filtre öğrenmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Alt ağ trafiği filtreleme](./virtual-networks-create-nsg-arm-ps.md)
