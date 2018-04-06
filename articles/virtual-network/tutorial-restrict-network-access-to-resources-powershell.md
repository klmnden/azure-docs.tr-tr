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
ms.openlocfilehash: 28c95e1333b4641e50284a869135a9608dd3242f
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="restrict-network-access-to-paas-resources-with-virtual-network-service-endpoints-using-powershell"></a>PowerShell kullanarak sanal ağ hizmet uç noktaları ile PaaS kaynaklarına ağ erişimi kısıtla

Sanal ağ hizmet uç noktaları bazı Azure hizmeti kaynaklar sanal ağ alt ağına ağ erişimini sınırlamak etkinleştirin. Internet kaynaklarına erişim de kaldırabilirsiniz. Hizmet uç noktaları desteklenen Azure Hizmetleri Azure hizmetlerine erişmek için sanal ağınızın özel adres alanı kullanmanıza olanak sağlayan, sanal ağ üzerinden doğrudan bağlantı sağlar. Hizmet uç noktaları üzerinden Azure kaynaklarına her zaman hedefleyen trafiğe, Microsoft Azure omurga ağı üzerinde kalır. Bu makalede, bilgi nasıl yapılır:

* Bir alt ağ ile bir sanal ağ oluşturma
* Bir alt ağ ekleyin ve bir hizmet uç noktası etkinleştirin
* Bir Azure kaynağı oluşturun ve ağ erişimi için yalnızca bir alt ağdan izin verin
* Her alt ağda bir sanal makine (VM) dağıtma
* Bir alt ağdan bir kaynağa erişimi onaylayın
* Bir alt ağ ve Internet bağlantısını bir kaynağa erişimi reddedildi onaylayın

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Yüklemek ve PowerShell yerel olarak kullanmak seçerseniz, bu makale Azure PowerShell modülü sürümü 5.4.1 gerektirir veya sonraki bir sürümü. Yüklü sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Bir sanal ağ oluşturmadan önce sanal ağ ve bu makalede oluşturulan tüm kaynaklar için bir kaynak grubu oluşturmanız gerekir. Bir kaynak grubu ile oluşturmak [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Aşağıdaki örnek, bir kaynak grubu oluşturur *myResourceGroup*: 

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

Bir alt ağ yapılandırması ile oluşturma [yeni AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Aşağıdaki örnek, bir alt ağ yapılandırması adlı bir alt ağ için oluşturur *ortak*:

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

## <a name="enable-a-service-endpoint"></a>Hizmet uç noktası etkinleştirme 

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

## <a name="restrict-network-access-for-a-subnet"></a>Bir alt ağ için ağ erişimi kısıtlama

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

Bir ağ güvenlik grubu oluşturma [yeni AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup). Aşağıdaki örnek adlı bir ağ güvenlik grubu oluşturur *myNsgPrivate*.

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

## <a name="restrict-network-access-to-a-resource"></a>Bir kaynağa ağ erişimi kısıtlama

Hizmet uç noktaları için etkin Azure Hizmetleri aracılığıyla oluşturulan kaynaklarına ağ erişimi kısıtlamak için gerekli adımları hizmetleri arasında değişir. Her hizmet için belirli adımlar için tek tek Hizmetleri belgelerine bakın. Bu makalenin sonraki bölümlerinde, örnek bir Azure Storage hesabı ağ erişimini kısıtlamak için adımları içerir.

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

### <a name="create-a-file-share-in-the-storage-account"></a>Depolama hesabında bir dosya paylaşımı oluşturma

Depolama hesabınız için bir bağlam oluşturma ve ile anahtar [yeni AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext). İçerik depolama hesabı adı ve hesap anahtarını kapsar:

```azurepowershell-interactive
$storageContext = New-AzureStorageContext $storageAcctName $storageAcctKey
```

Bir dosya paylaşımı ile oluşturmak [yeni AzureStorageShare](/powershell/module/azure.storage/new-azurestorageshare):

$share yeni AzureStorageShare = my-dosya paylaşımı - bağlam $storageContext

### <a name="deny-all-network-access-to-a-storage-account"></a>Bir depolama hesabı için tüm ağ erişimini engelle

Varsayılan olarak, depolama hesapları herhangi bir ağ istemcilerinden gelen ağ bağlantılarını kabul eder. Seçili ağlara erişimi sınırlamak için varsayılan eylem değiştirme *reddetme* ile [güncelleştirme AzureRmStorageAccountNetworkRuleSet](/powershell/module/azurerm.storage/update-azurermstorageaccountnetworkruleset). Ağ erişim reddedildi sonra depolama hesabı herhangi bir ağdan erişilebilir değil.

```azurepowershell-interactive
Update-AzureRmStorageAccountNetworkRuleSet  `
  -ResourceGroupName "myresourcegroup" `
  -Name $storageAcctName `
  -DefaultAction Deny
```

### <a name="enable-network-access-from-a-subnet"></a>Bir alt ağdaki ağ erişimini etkinleştir

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

Ağ erişimi bir depolama hesabına sınamak için her alt ağ için bir VM dağıtın.

### <a name="create-the-first-virtual-machine"></a>İlk sanal makine oluşturma

Bir sanal makine oluşturmak *ortak* alt ağ ile [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Aşağıdaki komutu çalıştırırken, kimlik bilgileri istenir. Girdiğiniz değerleri VM için kullanıcı adı ve parola yapılandırılır. `-AsJob` Seçeneği bir sonraki adıma devam edebilmesi için bu VM arka planda oluşturur.

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

### <a name="create-the-second-virtual-machine"></a>İkinci sanal makine oluşturma

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

## <a name="confirm-access-to-storage-account"></a>Depolama hesabı erişim Onayla

Kullanım [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) VM genel IP adresi dönün. Aşağıdaki örnek genel IP adresi döndürür *myVmPrivate* VM:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmPrivate `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Değiştir `<publicIpAddress>` aşağıdaki komutta ortak IP adresi önceki komuttan döndürülen ve aşağıdaki komutu girin: 

```powershell
mstsc /v:<publicIpAddress>
```

Bir Uzak Masaüstü Protokolü (.rdp) dosyası oluşturulur ve bilgisayarınıza indirilmeden. İndirilen rdp dosyasını açın. İstenirse, seçin **Bağlan**. Kullanıcı adı ve VM oluştururken belirttiğiniz parolayı girin. Seçmek için gerek duyabileceğiniz **daha fazla seçenek**, ardından **farklı bir hesap kullan**, VM oluşturduğunuz sırada girdiğiniz kimlik bilgileri belirtmek için. **Tamam**’ı seçin. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Uyarı alırsanız, seçin **Evet** veya **devam**, bağlantı ile devam etmek için.

Üzerinde *myVmPrivate* VM, Azure dosya paylaşımı PowerShell kullanarak Z sürücü eşleme. İzleyin komutları çalıştırmadan önce Değiştir `<storage-account-key>` ve `<storage-account-name>` sağlanan veya içinde alınan değerlerle [depolama hesabı oluşturma](#create-a-storage-account).

```powershell
$acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
New-PSDrive -Name Z -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\my-file-share" -Credential $credential
```
PowerShell çıkışı aşağıdaki örnek çıktıya benzer döndürür:

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

Ağ güvenlik grubu ile ilişkili olduğundan hiç yanıt alırsınız *özel* alt ağ Azure Storage hizmetine atanan adresler dışında genel IP adreslerine giden erişim izin vermez.

Uzak Masaüstü oturumu kapatmak *myVmPrivate* VM.

## <a name="confirm-access-is-denied-to-storage-account"></a>Depolama hesabı için erişim reddedildi onaylayın

Genel IP adresi al *myVmPublic* VM:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmPublic `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Değiştir `<publicIpAddress>` aşağıdaki komutta ortak IP adresi önceki komuttan döndürülen ve aşağıdaki komutu girin: 

```powershell
mstsc /v:<publicIpAddress>
```

Üzerinde *myVmPublic* VM, Azure dosya paylaşımı için sürücüsüne Z girişimi. İzleyin komutları çalıştırmadan önce Değiştir `<storage-account-key>` ve `<storage-account-name>` sağlanan veya içinde alınan değerlerle [depolama hesabı oluşturma](#create-a-storage-account).

```powershell
$acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
New-PSDrive -Name Z -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\my-file-share" -Credential $credential
```

Paylaşım erişimi reddedildi ve aldığınız bir `New-PSDrive : Access is denied` hata. Nedeniyle erişim reddedildi *myVmPublic* VM dağıtıldığı *ortak* alt ağ. *Ortak* alt Azure Storage için etkin bir hizmet uç noktası yok ve depolama hesabı yalnızca gelen ağ erişimi sağlayan *özel* alt değil *ortak*alt ağ.

Uzak Masaüstü oturumu kapatmak *myVmPublic* VM.

Depolama hesabı aşağıdaki komutu kullanarak dosya paylaşımlarını görüntülemek, bilgisayarınızdan dener:

```powershell-interactive
Get-AzureStorageFile `
  -ShareName my-file-share `
  -Context $storageContext
```

Erişim reddedildi ve aldığınız bir *Get-AzureStorageFile: Uzak sunucu bir hata döndürdü: (403) Yasak. HTTP durum kodu: 403 - HTTP hata iletisi: Bu isteği bu işlemi gerçekleştirmek için yetkili değil* hatası, bilgisayarınızın içinde olmadığından *özel* alt *MyVirtualNetwork* sanal ağ.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olduğunda, kullanabileceğiniz [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) kaynak grubu ve içerdiği kaynakların tümünü kaldırmak için:

```azurepowershell-interactive 
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir sanal ağ alt ağı için bir hizmet uç noktası etkin. Hizmet uç noktaları birden çok Azure services ile dağıtılan kaynaklar için etkinleştirilebilir öğrendiniz. Bir Azure Storage hesabını ve sınırlı ağ erişimi yalnızca bir sanal ağ alt ağ içindeki kaynaklar için depolama hesabı oluşturuldu. Hizmet uç noktaları hakkında daha fazla bilgi için bkz: [hizmet uç noktaları genel bakış](virtual-network-service-endpoints-overview.md) ve [alt ağlarını yönetin](virtual-network-manage-subnet.md).

Hesabınızı birden çok sanal ağlarınız varsa, her sanal ağ içindeki kaynaklara birbirleri ile iletişim kurabilmesi iki sanal ağları birbirine bağlamak isteyebilirsiniz. Bilgi edinmek için bkz [sanal ağlara bağlanabilir](tutorial-connect-virtual-networks-powershell.md).