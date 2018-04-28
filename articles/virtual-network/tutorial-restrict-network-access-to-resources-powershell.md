---
title: PaaS kaynaklarına - Azure PowerShell ağ erişimi kısıtlama | Microsoft Docs
description: Bu makalede, sınırlayabilir ve Azure PowerShell kullanarak sanal ağ hizmet uç noktaları ile Azure Storage ve Azure SQL veritabanı gibi Azure kaynakları için ağ erişimini kısıtlayabilir öğrenin.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want only resources in a virtual network subnet to access an Azure PaaS resource, such as an Azure Storage account.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/14/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: b3977e045751165947243c67291e81b998b5fcb5
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="restrict-network-access-to-paas-resources-with-virtual-network-service-endpoints-using-powershell"></a>PowerShell kullanarak sanal ağ hizmet uç noktaları ile PaaS kaynaklarına ağ erişimi kısıtla

Sanal ağ hizmet uç noktaları bazı Azure hizmet uç noktalarına ağ erişimini bir sanal ağ alt ağı ile sınırlamanıza olanak tanır. Ayrıca, kaynaklara internet erişimini de kaldırabilirsiniz. Hizmet uç noktaları, sanal ağınızdan desteklenen Azure hizmetlerine doğrudan bağlantı sağlar, böylece Azure hizmetlerine erişmek için sanal ağınızın özel adres alanını kullanabilirsiniz. Hizmet uç noktaları aracılığıyla Azure kaynaklarına gönderilen trafik her zaman Microsoft Azure omurga ağı üzerinde kalır. Bu makalede şunları öğreneceksiniz:

* Alt ağ ile sanal ağ oluşturma
* Alt ağ ekleme ve hizmet uç noktasını etkinleştirme
* Azure kaynağı oluşturma ve yalnızca bir alt ağdan ağ erişimine izin verme
* Her alt ağa bir sanal makine (VM) dağıtma
* Bir alt ağdan kaynağa erişimi onaylama
* Bir alt ağdan ve internetten kaynağa erişimin reddedildiğini onaylama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Yüklemek ve PowerShell yerel olarak kullanmak seçerseniz, bu makale Azure PowerShell modülü sürümü 5.4.1 gerektirir veya sonraki bir sürümü. Yüklü sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Bir sanal ağ oluşturmadan önce sanal ağ ve bu makalede oluşturulan tüm kaynaklar için bir kaynak grubu oluşturmanız gerekir. [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) komutunu kullanarak bir kaynak grubu oluşturun. Aşağıdaki örnek, bir kaynak grubu oluşturur *myResourceGroup*: 

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

[New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) ile bir alt ağ yapılandırması oluşturun. Aşağıdaki örnek, bir alt ağ yapılandırması adlı bir alt ağ için oluşturur *ortak*:

```azurepowershell-interactive
$subnetConfigPublic = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Public `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork
```

Alt ağ ile sanal ağ alt ağ yapılandırması yazarak sanal ağ oluşturmak [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork):

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="enable-a-service-endpoint"></a>Hizmet uç noktasını girin 

Hizmet uç noktalarına hizmet uç noktaları destekleyen hizmetler için etkinleştirebilirsiniz. Bir Azure konumdaki Hizmeti uç noktası etkin hizmetleri kullanılabilir görüntülemek [Get-AzureRmVirtualNetworkAvailableEndpointService](/powershell/module/azurerm.network/get-azurermvirtualnetworkavailableendpointservice). Aşağıdaki örnek hizmet uç noktası etkin olmayan kullanılabilir hizmetlerin listesini döndürür *eastus* bölge. Daha fazla Azure Hizmetleri Hizmeti uç noktası etkin hale geldikçe döndürülen hizmetlerin listesini zamanla büyüyecektir.

```azurepowershell-interactive
Get-AzureRmVirtualNetworkAvailableEndpointService -Location eastus | Select Name
``` 

Ek bir alt ağ, sanal ağ oluşturun. Bu örnekte, bir alt ağ adlı *özel* için hizmet uç noktası ile oluşturulan *Microsoft.Storage*: 

```azurepowershell-interactive
$subnetConfigPrivate = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Private `
  -AddressPrefix 10.0.1.0/24 `
  -VirtualNetwork $virtualNetwork `
  -ServiceEndpoint Microsoft.Storage

$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="restrict-network-access-for-a-subnet"></a>Bir kaynak için ağ erişimini kısıtlama

Ağ güvenlik grubu güvenlik kuralları oluşturma [yeni AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig). Aşağıdaki kural Azure Storage hizmetine atanan genel IP adreslerine giden erişim sağlar: 

```azurepowershell-interactive
$rule1 = New-AzureRmNetworkSecurityRuleConfig `
  -Name Allow-Storage-All `
  -Access Allow `
  -DestinationAddressPrefix Storage `
  -DestinationPortRange * `
  -Direction Outbound `
  -Priority 100 `
  -Protocol * `
  -SourceAddressPrefix VirtualNetwork `
  -SourcePortRange *
```

Aşağıdaki kural tüm genel IP adresleri erişimini engeller. Önceki kural Azure Storage ortak IP adreslerini erişmesini sağlar, daha yüksek önceliği nedeniyle bu kural geçersiz kılar.

```azurepowershell-interactive
$rule2 = New-AzureRmNetworkSecurityRuleConfig `
  -Name Deny-Internet-All `
  -Access Deny `
  -DestinationAddressPrefix Internet `
  -DestinationPortRange * `
  -Direction Outbound `
  -Priority 110 `
  -Protocol * `
  -SourceAddressPrefix VirtualNetwork `
  -SourcePortRange *
```

Aşağıdaki kural Uzak Masaüstü Protokolü (RDP) trafiğine izin verir alt ağına yerden gelen. Sonraki adımda bir kaynağa ağ erişimi onaylamak için Uzak Masaüstü bağlantıları alt ağına izin verilir.

```azurepowershell-interactive
$rule3 = New-AzureRmNetworkSecurityRuleConfig `
  -Name Allow-RDP-All `
  -Access Allow `
  -DestinationAddressPrefix VirtualNetwork `
  -DestinationPortRange 3389 `
  -Direction Inbound `
  -Priority 120 `
  -Protocol * `
  -SourceAddressPrefix * `
  -SourcePortRange *
```

[New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) ile bir ağ güvenlik grubu oluşturun. Aşağıdaki örnek adlı bir ağ güvenlik grubu oluşturur *myNsgPrivate*.

```azurepowershell-interactive
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myNsgPrivate `
  -SecurityRules $rule1,$rule2,$rule3
```

Ağ güvenlik grubuyla ilişkilendirdiğiniz *özel* alt ağ ile [kümesi AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) ve ardından sanal ağ alt ağ yapılandırması yazın. Aşağıdaki örnek ilişkilendirir *myNsgPrivate* ağ güvenlik grubuna *özel* alt ağ:

```azurepowershell-interactive
Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $VirtualNetwork `
  -Name Private `
  -AddressPrefix 10.0.1.0/24 `
  -ServiceEndpoint Microsoft.Storage `
  -NetworkSecurityGroup $nsg

$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="restrict-network-access-to-a-resource"></a>Bir kaynağa ağ erişimini kısıtlama

Hizmet uç noktaları için etkinleştirilmiş Azure hizmetleri aracılığıyla oluşturulan kaynaklara ağ erişimini kısıtlamak için gereken adımlar, hizmetler arasında farklılık gösterir. Bir hizmete yönelik belirli adımlar için ilgili hizmetin belgelerine bakın. Bu makalenin sonraki bölümlerinde, örnek bir Azure Storage hesabı ağ erişimini kısıtlamak için adımları içerir.

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Bir Azure depolama hesabıyla oluşturma [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount). Değiştir `<replace-with-your-unique-storage-account-name>` 3-24 karakter uzunluğunda, tüm Azure konumları arasında benzersiz bir ad yalnızca sayılar ve küçük harfler kullanarak.

```azurepowershell-interactive
$storageAcctName = '<replace-with-your-unique-storage-account-name>'

New-AzureRmStorageAccount `
  -Location EastUS `
  -Name $storageAcctName `
  -ResourceGroupName myResourceGroup `
  -SkuName Standard_LRS `
  -Kind StorageV2
```

Depolama hesabı oluşturulduktan sonra depolama hesabı için anahtarı sahip bir değişken içine alıp [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey):

```azurepowershell-interactive
$storageAcctKey = (Get-AzureRmStorageAccountKey `
  -ResourceGroupName myResourceGroup `
  -AccountName $storageAcctName).Value[0]
```

Anahtar, bir sonraki adımda bir dosya paylaşımı oluşturmak için kullanılır. Girin `$storageAcctKey` ve bir sürücüye bir VM'de dosya paylaşımı eşlediğinizde el ile bir sonraki adımda girmek gereksinim duyacağınız değerini not edin.

### <a name="create-a-file-share-in-the-storage-account"></a>Depolama hesabında dosya paylaşımı oluşturma

Depolama hesabınız için bir bağlam oluşturma ve ile anahtar [yeni AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext). İçerik depolama hesabı adı ve hesap anahtarını kapsar:

```azurepowershell-interactive
$storageContext = New-AzureStorageContext $storageAcctName $storageAcctKey
```

Bir dosya paylaşımı ile oluşturmak [yeni AzureStorageShare](/powershell/module/azure.storage/new-azurestorageshare):

$share yeni AzureStorageShare = my-dosya paylaşımı - bağlam $storageContext

### <a name="deny-all-network-access-to-a-storage-account"></a>Bir depolama hesabı için tüm ağ erişimini engelle

Varsayılan olarak, depolama hesapları herhangi bir ağdaki istemcilerden gelen ağ bağlantılarını kabul eder. Seçili ağlara erişimi sınırlamak için varsayılan eylem değiştirme *reddetme* ile [güncelleştirme AzureRmStorageAccountNetworkRuleSet](/powershell/module/azurerm.storage/update-azurermstorageaccountnetworkruleset). Ağ erişim reddedildi sonra depolama hesabı herhangi bir ağdan erişilebilir değil.

```azurepowershell-interactive
Update-AzureRmStorageAccountNetworkRuleSet  `
  -ResourceGroupName "myresourcegroup" `
  -Name $storageAcctName `
  -DefaultAction Deny
```

### <a name="enable-network-access-from-a-subnet"></a>Bir alt ağdan ağ erişimini etkinleştirme

Oluşturulan sanal ağ ile almak [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork) ve sahip bir değişken içine özel alt ağ nesnesini almak [Get-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig):

```azurepowershell-interactive
$privateSubnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName "myResourceGroup" `
  -Name "myVirtualNetwork" `
  | Get-AzureRmVirtualNetworkSubnetConfig `
  -Name "Private"
```

Depolama hesabından ağ erişim izni *özel* alt ağ ile [Ekle AzureRmStorageAccountNetworkRule](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig).

```azurepowershell-interactive
Add-AzureRmStorageAccountNetworkRule `
  -ResourceGroupName "myresourcegroup" `
  -Name $storageAcctName `
  -VirtualNetworkResourceId $privateSubnet.Id
```

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Bir depolama hesabına ağ erişimini test etmek için her alt ağa bir VM dağıtın.

### <a name="create-the-first-virtual-machine"></a>İlk sanal makineyi oluşturma

Bir sanal makine oluşturmak *ortak* alt ağ ile [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Sonraki komutu çalıştırırken kimlik bilgileri istenir. Girdiğiniz değerler, sanal makinenin kullanıcı adı ve parolası olarak yapılandırılır. `-AsJob` seçeneği, sonraki adıma devam edebilmeniz için arka planda sanal makineyi oluşturur.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "Public" `
    -Name "myVmPublic" `
    -AsJob
```

Aşağıdaki örnek çıkış benzer bir çıktı döndürdü:

```powershell
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command                  
--     ----            -------------   -----         -----------     --------             -------                  
1      Long Running... AzureLongRun... Running       True            localhost            New-AzureRmVM     
```

### <a name="create-the-second-virtual-machine"></a>İkinci sanal makineyi oluşturma

Bir sanal makine oluşturmak *özel* alt ağ:

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "Private" `
    -Name "myVmPrivate"
```

Azure VM oluşturması birkaç dakika sürer. Azure VM oluşturduktan ve PowerShell çıkışı döndürür kadar sonraki adıma devam etmeyin. 

## <a name="confirm-access-to-storage-account"></a>Depolama hesabına erişimi onaylama

Bir sanal makinenin genel IP adresini döndürmek için [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) komutunu kullanın. Aşağıdaki örnek genel IP adresi döndürür *myVmPrivate* VM:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmPrivate `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Aşağıdaki komuttaki `<publicIpAddress>` öğesini, önceki komuttan döndürülen genel IP adresiyle değiştirin ve sonra aşağıdaki komutu girin: 

```powershell
mstsc /v:<publicIpAddress>
```

Uzak Masaüstü Protokolü (.rdp) dosyası oluşturulur ve bilgisayarınıza indirilir. İndirilen rdp dosyasını açın. İstendiğinde **Bağlan**’ı seçin. Sanal makine oluştururken belirttiğiniz kullanıcı adını ve parolayı girin. Sanal makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için **Diğer seçenekler**’i ve sonra **Farklı bir hesap kullan** seçeneğini belirlemeniz gerekebilir. **Tamam**’ı seçin. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Uyarı alırsanız, bağlantıya devam etmek için **Evet**’i veya **Bağlan**’ı seçin.

*myVmPrivate* VM üzerinde PowerShell kullanarak Azure dosya paylaşımını Z sürücüsüne eşleyin. İzleyin komutları çalıştırmadan önce Değiştir `<storage-account-key>` ve `<storage-account-name>` sağlanan veya içinde alınan değerlerle [depolama hesabı oluşturma](#create-a-storage-account).

```powershell
$acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
New-PSDrive -Name Z -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\my-file-share" -Credential $credential
```
PowerShell aşağıdaki örnek çıktıya benzer bir çıktı döndürür:

```powershell
Name           Used (GB)     Free (GB) Provider      Root
----           ---------     --------- --------      ----
Z                                      FileSystem    \\vnt.file.core.windows.net\my-f...
```

Z sürücüsüne başarıyla eşlenen Azure dosya paylaşımı.

VM diğer herhangi bir genel IP adreslerine giden bağlantıya sahip onaylayın:

```powershell
ping bing.com
```

*Özel* alt ağ ile ilişkili ağ güvenlik grubu Azure Depolama hizmetine atanan adreslerden başka genel IP adreslerine giden erişime izin vermediği için bir yanıt almazsınız.

*myVmPrivate* VM ile uzak masaüstü oturumunu kapatın.

## <a name="confirm-access-is-denied-to-storage-account"></a>Depolama hesabına erişimin reddedildiğini onaylama

Genel IP adresi al *myVmPublic* VM:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmPublic `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Aşağıdaki komuttaki `<publicIpAddress>` öğesini, önceki komuttan döndürülen genel IP adresiyle değiştirin ve sonra aşağıdaki komutu girin: 

```powershell
mstsc /v:<publicIpAddress>
```

Üzerinde *myVmPublic* VM, Azure dosya paylaşımı için sürücüsüne Z girişimi. İzleyin komutları çalıştırmadan önce Değiştir `<storage-account-key>` ve `<storage-account-name>` sağlanan veya içinde alınan değerlerle [depolama hesabı oluşturma](#create-a-storage-account).

```powershell
$acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
New-PSDrive -Name Z -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\my-file-share" -Credential $credential
```

Paylaşım erişimi reddedildi ve aldığınız bir `New-PSDrive : Access is denied` hata. *myVmPublic* VM *Genel* alt ağa dağıtıldığı için erişim reddedilir. *Genel* alt ağında Azure Depolama için etkinleştirilmiş bir hizmet uç noktası bulunmaz ve depolama hesabı *Genel* alt ağından değil, yalnızca *Özel* alt ağından ağ erişimine izin verir.

*myVmPublic* VM ile uzak masaüstü oturumunu kapatın.

Depolama hesabı aşağıdaki komutu kullanarak dosya paylaşımlarını görüntülemek, bilgisayarınızdan dener:

```powershell-interactive
Get-AzureStorageFile `
  -ShareName my-file-share `
  -Context $storageContext
```

Erişim reddedildi ve aldığınız bir *Get-AzureStorageFile: Uzak sunucu bir hata döndürdü: (403) Yasak. HTTP durum kodu: 403 - HTTP hata iletisi: Bu isteği bu işlemi gerçekleştirmek için yetkili değil* hatası, bilgisayarınızın içinde olmadığından *özel* alt *MyVirtualNetwork* sanal ağ.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) komutunu kullanarak kaynak grubunu ve içerdiği tüm kaynakları kaldırabilirsiniz:

```azurepowershell-interactive 
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir sanal ağ alt ağı için bir hizmet uç noktası etkin. Hizmet uç noktalarının birden fazla Azure hizmeti ile dağıtılmış kaynaklar için etkinleştirilebildiğini öğrendiniz. Bir Azure Depolama hesabı oluşturdunuz ve depolama hesabına ağ erişimini yalnızca bir sanal ağ alt ağındaki kaynaklarla sınırladınız. Hizmet uç noktaları hakkında daha fazla bilgi için bkz. [Hizmet uç noktalarına genel bakış](virtual-network-service-endpoints-overview.md) ve [Alt ağları yönetme](virtual-network-manage-subnet.md).

Hesabınızda birden fazla sanal ağ varsa, her bir sanal ağın içindeki kaynakların birbiriyle iletişim kurabilmesi iki sanal ağı birbirine bağlamak isteyebilirsiniz. Bilgi edinmek için bkz [sanal ağlara bağlanabilir](tutorial-connect-virtual-networks-powershell.md).
