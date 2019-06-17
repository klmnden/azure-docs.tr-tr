---
title: PaaS kaynaklarına - Azure PowerShell ağ erişimini kısıtlama | Microsoft Docs
description: Bu makalede, sınırlandırmak ve Azure PowerShell kullanarak sanal ağ hizmet uç noktaları ile Azure depolama ve Azure SQL veritabanı gibi Azure kaynaklarına ağ erişimini kısıtlama hakkında bilgi edinin.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: twooley
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
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: b76256ef70b85df0c504427179518d175f08b645
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66727667"
---
# <a name="restrict-network-access-to-paas-resources-with-virtual-network-service-endpoints-using-powershell"></a>PowerShell kullanarak sanal ağ hizmet uç noktaları ile PaaS kaynaklarına ağ erişimini kısıtlama

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Sanal ağ hizmet uç noktaları bazı Azure hizmet uç noktalarına ağ erişimini bir sanal ağ alt ağı ile sınırlamanıza olanak tanır. Ayrıca, kaynaklara internet erişimini de kaldırabilirsiniz. Hizmet uç noktaları, sanal ağınızdan desteklenen Azure hizmetlerine doğrudan bağlantı sağlar, böylece Azure hizmetlerine erişmek için sanal ağınızın özel adres alanını kullanabilirsiniz. Hizmet uç noktaları aracılığıyla Azure kaynaklarına gönderilen trafik her zaman Microsoft Azure omurga ağı üzerinde kalır. Bu makalede şunları öğreneceksiniz:

* Alt ağ ile sanal ağ oluşturma
* Alt ağ ekleme ve hizmet uç noktasını etkinleştirme
* Azure kaynağı oluşturma ve yalnızca bir alt ağdan ağ erişimine izin verme
* Her alt ağa bir sanal makine (VM) dağıtma
* Bir alt ağdan kaynağa erişimi onaylama
* Bir alt ağdan ve internetten kaynağa erişimin reddedildiğini onaylama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu makale Azure PowerShell modülü sürüm 1.0.0 gerekir veya üzeri. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Bir sanal ağ oluşturmadan önce sanal ağ ve bu makalede oluşturulan tüm kaynakları için bir kaynak grubu oluşturmanız gerekir. Bir kaynak grubu oluşturun [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Aşağıdaki örnekte adlı bir kaynak grubu oluşturur *myResourceGroup*: 

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

İle sanal ağ oluşturma [yeni AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork). Aşağıdaki örnekte adlı bir sanal ağ oluşturur *myVirtualNetwork* adres ön eki ile *10.0.0.0/16*.

```azurepowershell-interactive
$virtualNetwork = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

Bir alt ağ yapılandırması ile [yeni AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig). Aşağıdaki örnekte adlı bir alt ağ için bir alt ağ yapılandırması oluşturur *genel*:

```azurepowershell-interactive
$subnetConfigPublic = Add-AzVirtualNetworkSubnetConfig `
  -Name Public `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork
```

Alt ağ ile sanal ağa, alt ağ yapılandırmasını yazarak sanal ağ oluşturmak [kümesi AzVirtualNetwork](/powershell/module/az.network/Set-azVirtualNetwork):

```azurepowershell-interactive
$virtualNetwork | Set-AzVirtualNetwork
```

## <a name="enable-a-service-endpoint"></a>Hizmet uç noktasını girin

Hizmet uç noktaları destekleyen hizmetler için hizmet uç noktaları etkinleştirebilirsiniz. Hizmet uç noktası etkin hizmetler kullanılabilir bir Azure konumu görünümünde [Get-AzVirtualNetworkAvailableEndpointService](/powershell/module/az.network/get-azvirtualnetworkavailableendpointservice). Aşağıdaki örnek, hizmet uç noktası etkin kullanılabilir hizmetlerin listesini döndürür *eastus* bölge. Diğer Azure Hizmetleri etkin hizmet bitiş noktası oldukça döndürülen hizmetlerin listesi zamanla büyüyecektir.

```azurepowershell-interactive
Get-AzVirtualNetworkAvailableEndpointService -Location eastus | Select Name
```

Ek bir alt ağ, sanal ağ oluşturun. Bu örnekte, bir alt ağı adlı *özel* için hizmet uç noktası ile oluşturulan *Microsoft.Storage*: 

```azurepowershell-interactive
$subnetConfigPrivate = Add-AzVirtualNetworkSubnetConfig `
  -Name Private `
  -AddressPrefix 10.0.1.0/24 `
  -VirtualNetwork $virtualNetwork `
  -ServiceEndpoint Microsoft.Storage

$virtualNetwork | Set-AzVirtualNetwork
```

## <a name="restrict-network-access-for-a-subnet"></a>Bir kaynak için ağ erişimini kısıtlama

Ağ güvenlik grubu güvenlik kuralları ile oluşturma [yeni AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig). Aşağıdaki kural, Azure depolama hizmetine atanmış genel IP adreslerine giden erişim sağlar: 

```azurepowershell-interactive
$rule1 = New-AzNetworkSecurityRuleConfig `
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

Aşağıdaki kural, tüm genel IP adresleri erişimi engeller. Önceki kural, Azure Depolama'nın genel IP adreslerine erişim sağlar, daha yüksek önceliği nedeniyle bu kuralı geçersiz kılar.

```azurepowershell-interactive
$rule2 = New-AzNetworkSecurityRuleConfig `
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

Aşağıdaki kural Uzak Masaüstü Protokolü (RDP) trafiğine izin verir. her yerden alt ağa gelen. Uzak Masaüstü bağlantıları, sonraki bir adımda bir kaynağa ağ erişimini onaylayabilmeniz alt ağına izin verilir.

```azurepowershell-interactive
$rule3 = New-AzNetworkSecurityRuleConfig `
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

Bir ağ güvenlik grubu oluşturun [yeni AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup). Aşağıdaki örnekte adlı bir ağ güvenlik grubu oluşturur *myNsgPrivate*.

```azurepowershell-interactive
$nsg = New-AzNetworkSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myNsgPrivate `
  -SecurityRules $rule1,$rule2,$rule3
```

Ağ güvenlik grubuyla ilişkilendirdiğiniz *özel* alt ağ ile [kümesi AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig) ve ardından sanal ağ için alt ağ yapılandırmasını yazın. Aşağıdaki örnek ilişkilendirir *myNsgPrivate* ağ güvenlik grubunu *özel* alt ağı:

```azurepowershell-interactive
Set-AzVirtualNetworkSubnetConfig `
  -VirtualNetwork $VirtualNetwork `
  -Name Private `
  -AddressPrefix 10.0.1.0/24 `
  -ServiceEndpoint Microsoft.Storage `
  -NetworkSecurityGroup $nsg

$virtualNetwork | Set-AzVirtualNetwork
```

## <a name="restrict-network-access-to-a-resource"></a>Bir kaynağa ağ erişimini kısıtlama

Hizmet uç noktaları için etkinleştirilmiş Azure hizmetleri aracılığıyla oluşturulan kaynaklara ağ erişimini kısıtlamak için gereken adımlar, hizmetler arasında farklılık gösterir. Bir hizmete yönelik belirli adımlar için ilgili hizmetin belgelerine bakın. Bu makalenin geri kalanında örnek olarak bir Azure depolama hesabı için ağ erişimini kısıtlamaya yönelik adımlar içerir.

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Bir Azure depolama hesabı oluşturun [yeni AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount). Değiştirin `<replace-with-your-unique-storage-account-name>` 3-24 karakter uzunluğunda, tüm Azure konumlarında benzersiz olan bir ada sahip yalnızca sayı ve küçük harfler kullanarak.

```azurepowershell-interactive
$storageAcctName = '<replace-with-your-unique-storage-account-name>'

New-AzStorageAccount `
  -Location EastUS `
  -Name $storageAcctName `
  -ResourceGroupName myResourceGroup `
  -SkuName Standard_LRS `
  -Kind StorageV2
```

Depolama hesabı oluşturulduktan sonra depolama hesabı anahtarı sahip bir değişken içine almak [Get-AzStorageAccountKey](/powershell/module/az.storage/get-azstorageaccountkey):

```azurepowershell-interactive
$storageAcctKey = (Get-AzStorageAccountKey `
  -ResourceGroupName myResourceGroup `
  -AccountName $storageAcctName).Value[0]
```

Anahtar, daha sonraki bir adımda bir dosya paylaşımı oluşturmak için kullanılır. Girin `$storageAcctKey` ve el ile bir sanal makinede bir sürücüyü dosya paylaşımına eşlediğinizde daha sonraki bir adımda girmek gereksinim duyacağınız değerini not edin.

### <a name="create-a-file-share-in-the-storage-account"></a>Depolama hesabında dosya paylaşımı oluşturma

Depolama hesabınız için bir bağlam oluşturur ve ile anahtar [yeni AzStorageContext](/powershell/module/az.storage/new-AzStoragecontext). Bağlam, depolama hesabı adını ve hesap anahtarını kapsar:

```azurepowershell-interactive
$storageContext = New-AzStorageContext $storageAcctName $storageAcctKey
```

Bir dosya paylaşımı ile [yeni AzStorageShare](/powershell/module/az.storage/new-azstorageshare):

$share yeni AzStorageShare = my-dosya paylaşımı - $storageContext bağlamı

### <a name="deny-all-network-access-to-a-storage-account"></a>Tüm bir depolama hesabına ağ erişimini engelle

Varsayılan olarak, depolama hesapları herhangi bir ağdaki istemcilerden gelen ağ bağlantılarını kabul eder. Seçili ağlar erişimi sınırlamak için varsayılan eylem için değiştirme *Reddet* ile [güncelleştirme AzStorageAccountNetworkRuleSet](/powershell/module/az.storage/update-azstorageaccountnetworkruleset). Ağ erişimi engellendi sonra depolama hesabı herhangi bir ağdan erişilebilir değil.

```azurepowershell-interactive
Update-AzStorageAccountNetworkRuleSet  `
  -ResourceGroupName "myresourcegroup" `
  -Name $storageAcctName `
  -DefaultAction Deny
```

### <a name="enable-network-access-from-a-subnet"></a>Bir alt ağdan ağ erişimini etkinleştirme

Oluşturulan sanal ağ ile alma [Get-AzVirtualNetwork](/powershell/module/az.network/get-azvirtualnetwork) ve sonra bir değişken içine özel alt ağ nesnesini almak [Get-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/get-azvirtualnetworksubnetconfig):

```azurepowershell-interactive
$privateSubnet = Get-AzVirtualNetwork `
  -ResourceGroupName "myResourceGroup" `
  -Name "myVirtualNetwork" `
  | Get-AzVirtualNetworkSubnetConfig `
  -Name "Private"
```

Depolama hesabından için ağ erişimine izin ver *özel* alt ağ ile [Ekle AzStorageAccountNetworkRule](/powershell/module/az.network/add-aznetworksecurityruleconfig).

```azurepowershell-interactive
Add-AzStorageAccountNetworkRule `
  -ResourceGroupName "myresourcegroup" `
  -Name $storageAcctName `
  -VirtualNetworkResourceId $privateSubnet.Id
```

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Bir depolama hesabına ağ erişimini test etmek için her alt ağa bir VM dağıtın.

### <a name="create-the-first-virtual-machine"></a>İlk sanal makineyi oluşturma

Bir sanal makine oluşturma *genel* alt ağ ile [New-AzVM](/powershell/module/az.compute/new-azvm). Sonraki komutu çalıştırırken kimlik bilgileri istenir. Girdiğiniz değerler, sanal makinenin kullanıcı adı ve parolası olarak yapılandırılır. `-AsJob` seçeneği, sonraki adıma devam edebilmeniz için arka planda sanal makineyi oluşturur.

```azurepowershell-interactive
New-AzVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "Public" `
    -Name "myVmPublic" `
    -AsJob
```

Aşağıdaki örnek çıktıya benzer bir çıktı döndürülür:

```powershell
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command                  
--     ----            -------------   -----         -----------     --------             -------                  
1      Long Running... AzureLongRun... Running       True            localhost            New-AzVM     
```

### <a name="create-the-second-virtual-machine"></a>İkinci sanal makineyi oluşturma

Bir sanal makine oluşturma *özel* alt ağı:

```azurepowershell-interactive
New-AzVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "Private" `
    -Name "myVmPrivate"
```

Azure VM oluşturmak birkaç dakika sürer. Azure VM oluşturma işlemini ve PowerShell için bir çıktı döndürür kadar sonraki adıma geçmeyin.

## <a name="confirm-access-to-storage-account"></a>Depolama hesabına erişimi onaylama

Kullanım [Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress) bir VM'nin genel IP adresini döndürmek için. Aşağıdaki örnek, genel IP adresini döndürür *myVmPrivate* VM:

```azurepowershell-interactive
Get-AzPublicIpAddress `
  -Name myVmPrivate `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Aşağıdaki komuttaki `<publicIpAddress>` öğesini, önceki komuttan döndürülen genel IP adresiyle değiştirin ve sonra aşağıdaki komutu girin:

```powershell
mstsc /v:<publicIpAddress>
```

Uzak Masaüstü Protokolü (.rdp) dosyası oluşturulur ve bilgisayarınıza indirilir. İndirilen rdp dosyasını açın. İstendiğinde **Bağlan**’ı seçin. Sanal makine oluştururken belirttiğiniz kullanıcı adını ve parolayı girin. Sanal makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için **Diğer seçenekler**’i ve sonra **Farklı bir hesap kullan** seçeneğini belirlemeniz gerekebilir. **Tamam**’ı seçin. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Uyarı alırsanız, bağlantıya devam etmek için **Evet**’i veya **Bağlan**’ı seçin.

*myVmPrivate* VM üzerinde PowerShell kullanarak Azure dosya paylaşımını Z sürücüsüne eşleyin. Komutları çalıştırmadan önce değiştirin `<storage-account-key>` ve `<storage-account-name>` sağlanan ya da aldığınız değerlerle [depolama hesabı oluşturma](#create-a-storage-account).

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

VM'ye diğer herhangi bir genel IP adreslerine giden bağlantısının olmadığını doğrulayın:

```powershell
ping bing.com
```

*Özel* alt ağ ile ilişkili ağ güvenlik grubu Azure Depolama hizmetine atanan adreslerden başka genel IP adreslerine giden erişime izin vermediği için bir yanıt almazsınız.

*myVmPrivate* VM ile uzak masaüstü oturumunu kapatın.

## <a name="confirm-access-is-denied-to-storage-account"></a>Depolama hesabına erişimin reddedildiğini onaylama

Genel IP adresini alın *myVmPublic* VM:

```azurepowershell-interactive
Get-AzPublicIpAddress `
  -Name myVmPublic `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Aşağıdaki komuttaki `<publicIpAddress>` öğesini, önceki komuttan döndürülen genel IP adresiyle değiştirin ve sonra aşağıdaki komutu girin: 

```powershell
mstsc /v:<publicIpAddress>
```

Üzerinde *myVmPublic* VM, Azure dosya paylaşımını Z sürücüsüne yapılmaya çalışıldı. Komutları çalıştırmadan önce değiştirin `<storage-account-key>` ve `<storage-account-name>` sağlanan ya da aldığınız değerlerle [depolama hesabı oluşturma](#create-a-storage-account).

```powershell
$acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
New-PSDrive -Name Z -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\my-file-share" -Credential $credential
```

Paylaşıma erişim reddedilir ve aldığınız bir `New-PSDrive : Access is denied` hata. *myVmPublic* VM *Genel* alt ağa dağıtıldığı için erişim reddedilir. *Genel* alt ağında Azure Depolama için etkinleştirilmiş bir hizmet uç noktası bulunmaz ve depolama hesabı *Genel* alt ağından değil, yalnızca *Özel* alt ağından ağ erişimine izin verir.

*myVmPublic* VM ile uzak masaüstü oturumunu kapatın.

Aşağıdaki komutla depolama hesabındaki dosya paylaşımlarını görüntülemek, bilgisayarınızdan çalışır:

```powershell-interactive
Get-AzStorageFile `
  -ShareName my-file-share `
  -Context $storageContext
```

Erişim reddedilir ve aldığınız bir *Get-AzStorageFile: Uzak sunucu hata döndürdü: (403) Yasak. HTTP durum kodu: 403 - HTTP hata iletisi: Bu istek bu işlemi gerçekleştirmek için yetkili değil* hata, bilgisayarınızı olmadığı *özel* alt *MyVirtualNetwork* sanal ağ.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için:

```azurepowershell-interactive 
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir sanal ağ alt ağı için hizmet uç noktası etkin. Hizmet uç noktalarının birden fazla Azure hizmeti ile dağıtılmış kaynaklar için etkinleştirilebildiğini öğrendiniz. Bir Azure Depolama hesabı oluşturdunuz ve depolama hesabına ağ erişimini yalnızca bir sanal ağ alt ağındaki kaynaklarla sınırladınız. Hizmet uç noktaları hakkında daha fazla bilgi için bkz. [Hizmet uç noktalarına genel bakış](virtual-network-service-endpoints-overview.md) ve [Alt ağları yönetme](virtual-network-manage-subnet.md).

Hesabınızda birden fazla sanal ağ varsa, her bir sanal ağın içindeki kaynakların birbiriyle iletişim kurabilmesi iki sanal ağı birbirine bağlamak isteyebilirsiniz. Bilgi edinmek için bkz. nasıl [sanal ağları birbirine bağlama](tutorial-connect-virtual-networks-powershell.md).
