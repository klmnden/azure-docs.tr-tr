---
title: "Azure PowerShell'i (Resource Manager) bir SQL Server sanal makine oluşturma | Microsoft Docs"
description: "Bir Azure VM ile SQL Server sanal makineye Galerisi görüntüleri oluşturmak için adımlar ve PowerShell komut dosyaları sağlar."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: 98d50dd8-48ad-444f-9031-5378d8270d7b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/29/2017
ms.author: jroth
ms.openlocfilehash: 33c306258b6be40f2c5cbc016e3c84e36bf61e0d
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-resource-manager"></a>Azure PowerShell (Resource Manager) kullanarak bir SQL Server sanal makine sağlama
> [!div class="op_single_selector"]
> * [Portal](virtual-machines-windows-portal-sql-server-provision.md)
> * [PowerShell](virtual-machines-windows-ps-sql-create.md)
>
>

## <a name="overview"></a>Genel Bakış
Bu öğretici kullanarak bir tek Azure sanal makine oluşturmak gösterilmiştir **Azure Resource Manager** Azure PowerShell cmdlet'lerini kullanarak dağıtım modeli. Bu öğreticide, tek bir disk sürücüsü bir görüntüden SQL Galerisi'nde kullanarak tek bir sanal makine oluşturacağız. Depolama, ağ ve sanal makine tarafından kullanılacak olan işlem kaynakları için yeni sağlayıcıları oluşturacağız. Tüm bu kaynaklar için varolan sağlayıcıları varsa, bunun yerine bu sağlayıcılardan kullanabilirsiniz.

Bu konunun Klasik sürümü gerekirse bkz [Klasik Azure PowerShell kullanarak bir SQL Server sanal makine sağlama](../classic/ps-sql-create.md).

## <a name="prerequisites"></a>Ön koşullar
Bu öğretici için ihtiyacınız vardır:

* Bir Azure hesabı ve başlamadan önce abonelik. Yoksa, kaydolun bir [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/).
* [Azure PowerShell)](/powershell/azure/overview), en düşük sürüm 1.4.0 veya üzeri (sürüm 1.5.0 kullanılarak yazılmış Bu öğretici).
  * Sürümünüz almak için aşağıdakileri yazın **Azure Get-Module - listavailable birlikte**.

## <a name="configure-your-subscription"></a>Aboneliğinizi yapılandırın
Windows PowerShell'i açın ve aşağıdaki cmdlet'i çalıştırarak Azure hesabınıza erişim kurun. Kimlik bilgilerinizi girmeniz için bir oturum ile açma ekranı sunulur. Aynı e-posta ve Azure portalında oturum açmak için kullandığınız parolayı kullanın.

```PowerShell
Add-AzureRmAccount
```

Başarıyla oturum açtıktan sonra bazı bilgiler abonelik adını ve varsayılan aboneliğinizin Kimliğini içeren ekranda görün. Farklı bir aboneliğe değiştirmediğiniz sürece, Bu öğretici için kaynakları oluşturulacak abonelik budur. Birden çok aboneliğiniz varsa, tüm aboneliklerinizi listesini döndürmek için aşağıdaki cmdlet'i çalıştırın:

```PowerShell
Get-AzureRmSubscription
```
Başka bir abonelik değiştirmek için hedef varlığıyla SubscriptionName aşağıdaki komutu çalıştırın.

```PowerShell
Select-AzureRmSubscription -SubscriptionName YourTargetSubscriptionName
```

## <a name="define-image-variables"></a>Görüntü değişkenleri tanımlayın
Kullanılabilirlik ve bu öğreticinin tamamlanan betikten anlayış basitleştirmek için size bir dizi değişken tanımlayarak başlar. Uygun gördüğünüz, ancak sağlanan değerler değiştirirken adı uzunlukları ve özel karakterler için ilgili kısıtlamaları adlandırma dikkat parametre değerlerini değiştirin.

### <a name="location-and-resource-group"></a>Konum ve kaynak grubu
İki değişken veri bölgesi ve sanal makine için diğer kaynaklar oluşturacağınız kaynak grubunu tanımlamak için kullanın.

İstediğiniz şekilde değiştirin ve bu değişkenleri başlatmak için aşağıdaki cmdlet'leri çalıştırın.

```PowerShell
$Location = "SouthCentralUS"
$ResourceGroupName = "sqlvm1"
```

### <a name="storage-properties"></a>Depolama özellikleri
Depolama hesabı ve sanal makine tarafından kullanılacak depolama türünü tanımlamak için aşağıdaki değişkenleri kullanın.

İstediğiniz şekilde değiştirin ve bu değişkenleri başlatmak için aşağıdaki cmdlet'i çalıştırın. Bu örnekte, biz kullanıyoruz Not [Premium depolama](../premium-storage.md), üretim iş yükleri için önerilir. Bu kılavuz ve diğer öneriler hakkında daha fazla bilgi için bkz: [Azure Virtual Machines'de SQL Server için performans en iyi uygulamaları](virtual-machines-windows-sql-performance.md).

```PowerShell
$StorageName = $ResourceGroupName + "storage"
$StorageSku = "Premium_LRS"
```

### <a name="network-properties"></a>Ağ Özellikleri
Ağ arabirimi, TCP/IP'yi ayırma yöntemi, sanal ağ adı, sanal alt ağ adı, sanal ağ için IP aralığı adresleri, IP adresi aralığı alt ağı ve genel etki alanı adı etiketi için tanımlamak için aşağıdaki değişkenleri kullanın Sanal makinede ağ tarafından kullanılmak üzere.

İstediğiniz şekilde değiştirin ve bu değişkenleri başlatmak için aşağıdaki cmdlet'i çalıştırın.

```PowerShell
$InterfaceName = $ResourceGroupName + "ServerInterface"
$TCPIPAllocationMethod = "Dynamic"
$VNetName = $ResourceGroupName + "VNet"
$SubnetName = "Default"
$VNetAddressPrefix = "10.0.0.0/16"
$VNetSubnetAddressPrefix = "10.0.0.0/24"
$DomainName = "sqlvm1"
```

### <a name="virtual-machine-properties"></a>Sanal makine özellikleri
Sanal makine adı, bilgisayar adı, sanal makine boyutu ve sanal makine için işletim sistemi disk adı tanımlamak için aşağıdaki değişkenleri kullanın.

İstediğiniz şekilde değiştirin ve bu değişkenleri başlatmak için aşağıdaki cmdlet'i çalıştırın.

```PowerShell
$VMName = $ResourceGroupName + "VM"
$ComputerName = $ResourceGroupName + "Server"
$VMSize = "Standard_DS13"
$OSDiskName = $VMName + "OSDisk"
```

### <a name="image-properties"></a>Görüntü Özellikleri
Sanal makine için kullanılacak görüntünün tanımlamak için aşağıdaki değişkenleri kullanın. Bu örnekte, SQL Server 2016 Geliştirici sürümü görüntüsü kullanılır. Geliştirici sürümü serbestçe test ve geliştirme için lisanslı ve yalnızca VM çalıştıran maliyetini için ödeme yaparsınız.

İstediğiniz şekilde değiştirin ve bu değişkenleri başlatmak için aşağıdaki cmdlet'i çalıştırın.

```PowerShell
$PublisherName = "MicrosoftSQLServer"
$OfferName = "SQL2016-WS2016"
$Sku = "SQLDEV"
$Version = "latest"
```

Not: SQL Server görüntüsü tekliflerinin Get-AzureRmVMImageOffer komutuyla tam bir liste alabilir

```PowerShell
Get-AzureRmVMImageOffer -Location 'East US' -Publisher 'MicrosoftSQLServer'
```

Ve Get-AzureRmVMImageSku komutuyla bir sunum için kullanılabilir SKU'lar görebilirsiniz. Aşağıdaki komut tüm SKU'ları için kullanılabilir gösterir **SQL2016SP1 WS2016** sunar.

```PowerShell
Get-AzureRmVMImageSku -Location $Location -Publisher 'MicrosoftSQLServer' -Offer 'SQL2016SP1-WS2016' | Select Skus
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Resource Manager dağıtım modeli ile oluşturduğunuz ilk kaynak grubu nesnesidir. Kullanacağız [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) kaynak grubu adı ve daha önce başlatılmış değişkenleri tarafından tanımlanmış konuma sahip bir Azure kaynak grubu ve kaynaklarına oluşturmak için cmdlet'i.

Yeni kaynak grubunuzu oluşturmak için aşağıdaki cmdlet'i çalıştırın.

```PowerShell
New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma
Sanal makine, SQL Server veri ve günlük dosyalarını ve işletim sistemi diski için depolama kaynaklarını gerektirir. Kolaylık olması için tek bir disk için her ikisini de oluşturacağız. Ek diskleri iliştirebilirsiniz daha sonra kullanılarak [Ekle-Azure diski](/powershell/module/azure/add-azuredisk) SQL Server veri ve günlük yerleştirmek için cmdlet dosyaları adanmış diskler üzerinde. Kullanacağız [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) standart depolama oluşturmak için cmdlet'i hesap yeni kaynak grubunuz ve depolama hesabı adı, depolama Sku adı ve konumu değişkenleri kullanılarak tanımlanmış, daha önce başlatıldı.

Yeni depolama hesabınızı oluşturmak için aşağıdaki cmdlet'i çalıştırın.

```PowerShell
$StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location
```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturun
Sanal makine ağ kaynaklarının sayısını için ağ bağlantısı gerektirir.

* Her sanal makine bir sanal ağ gerektirir.
* Bir sanal ağ en az bir alt ağ tanımlanmış olması gerekir.
* Bir ağ arabirimi genel veya özel bir IP adresi ile tanımlanması gerekir.

### <a name="create-a-virtual-network-subnet-configuration"></a>Bir sanal ağ alt ağ yapılandırması oluştur
Biz bizim sanal ağ için bir alt ağ yapılandırması oluşturarak başlar. Bizim öğretici için varsayılan alt ağ kullanılarak oluşturacağız [yeni AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) cmdlet'i. Daha önce başlatılmış değişkenleri kullanılarak tanımlanan alt ağ adı ve adres öneki ile sanal ağ alt ağı yapılandırmamızın oluşturacağız.

> [!NOTE]
> Bu cmdlet kullanarak sanal ağ alt ağ yapılandırması ek özelliklerini tanımlayabilirsiniz, ancak, Bu öğretici kapsamında değildir.

Sanal alt ağ yapılandırmanızı oluşturmak için aşağıdaki cmdlet'i çalıştırın.

```PowerShell
$SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
```

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma
Ardından, sanal ağ kullanarak oluşturacağız [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) cmdlet'i. Yeni kaynak grubunuz bizim sanal ağ oluşturacağız, adını, konumunu ve adres öneki daha önce başlatılmış değişkenleri kullanma ve önceki adımda tanımlanan alt ağ yapılandırması kullanma tanımlı.

Sanal ağınızı oluşturmak için aşağıdaki cmdlet'i çalıştırın.

```PowerShell
$VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
```

### <a name="create-the-public-ip-address"></a>Ortak IP adresi oluştur
Tanımlanan bizim sanal ağ sahibiz, sanal makine bağlantısı için bir IP adresi yapılandırmanız gerekir. Bu öğretici için dinamik IP Internet bağlantısı desteklemek için adresi kullanarak bir ortak IP adresi oluşturacağız. Kullanacağız [yeni AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) genel IP oluşturmak için cmdlet'i adres kaynak grubu oluşturulan prevously ve adını, konumunu, ayırma yöntemi ve DNS etki alanı adı etiketi değişkenleri kullanılarak tanımlanan doğrulayın daha önce başlatıldı.

> [!NOTE]
> Bu cmdlet kullanılarak ortak IP adresinin ek özellikler tanımlayabilirsiniz, ancak, ilk Bu öğretici kapsamında değildir. Statik adresi ile özel bir adres veya adresi de oluşturabilir, ancak, aynı zamanda Bu öğretici kapsamında değildir.

Ortak IP adresi oluşturmak için aşağıdaki cmdlet'i çalıştırın.

```PowerShell
$PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
```

### <a name="create-the-network-interface"></a>Ağ arabirimi oluştur
Biz şimdi bizim sanal makinenin kullanacağı ağ arabirim oluşturmak hazır olursunuz. Kullanacağız [yeni AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) daha önce oluşturduğunuz kaynak grubunda ve adını, konumunu, alt ağ ve genel IP adresi ile bizim ağ arabirim oluşturmak için cmdlet'i önceden tanımlanmış.

Ağ arabirimi oluşturmak üzere aşağıdaki cmdlet'i çalıştırın.

```PowerShell
$Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id
```

## <a name="configure-a-vm-object"></a>Bir VM nesnesine yapılandırın
Biz tanımlanan depolama ve ağ kaynaklarına sahip olduğunuza göre biz sanal makine için işlem kaynakları tanımlamak hazır olursunuz. Bizim öğretici için size sanal makine boyutu ve çeşitli işletim sistemi özelliklerini belirtin, daha önce oluşturduğumuz, ağ arabiriminin blob depolama alanını tanımlayın ve ardından işletim sistemi diski belirtin belirtin.

### <a name="create-the-vm-object"></a>VM nesnesi oluşturun
Biz, sanal makine boyutu belirterek başlar. Bu öğretici için size bir DS13 belirlersiniz. Kullanacağız [yeni AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) adı ve daha önce başlatılmış değişkenleri kullanılarak tanımlanan boyutu ile yapılandırılabilir sanal makine nesnesi oluşturmak için cmdlet'i.

Sanal makine nesnesini oluşturmak için aşağıdaki cmdlet'i çalıştırın.

```PowerShell
$VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
```

### <a name="create-a-credential-object-to-hold-the-name-and-password-for-the-local-administrator-credentials"></a>Adı ve parola yerel yönetici kimlik bilgilerini saklamak için bir kimlik bilgisi nesnesi oluşturun
Sanal makine için işletim sistemi özelliklerini ayarlayabilmeniz için önce güvenli bir dize olarak yerel yönetici hesabının kimlik bilgilerini sağlamanız gerekir. Bunu gerçekleştirmek için kullanacağız [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet'i.

Aşağıdaki cmdlet'i yürütün ve Windows PowerShell kimlik bilgisi isteği penceresinde, adı ve Windows sanal makinesinde yerel yönetici hesabı için kullanılacak parolayı yazın.

```PowerShell
$Credential = Get-Credential -Message "Type the name and password of the local administrator account."
```

### <a name="set-the-operating-system-properties-for-the-virtual-machine"></a>Sanal makine için işletim sistemi özelliklerini ayarlama
Şimdi biz sanal makinenin işletim sistemi özelliklerini ayarlamak hazır olursunuz. Kullanacağız [kümesi AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) , Windows işletim sisteminin türünü ayarlamak için cmdlet gerektiren [sanal makine aracısını](../classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) yüklenmesi için cmdlet otomatik güncelleştirme sağlar belirtin ve sanal makine adı, bilgisayar adı ve daha önce başlatılmış değişkenler kullanarak kimlik bilgilerini ayarlayın.

Sanal makine için işletim sistemi özelliklerini ayarlamak için aşağıdaki cmdlet'i çalıştırın.

```PowerShell
$VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate
```

### <a name="add-the-network-interface-to-the-virtual-machine"></a>Ağ arabirimi sanal makineye ekleyin
Ardından, oluşturduğumuz ağ arabirimi önceden sanal makineye ekleyeceğiz. Kullanacağız [Ekle AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) cmdlet'ini daha önce tanımlanan ağ arabirimi değişkenini kullanarak ağ arabirimi ekleyin.

Ağ arabirimi sanal makineniz için ayarlamak için aşağıdaki cmdlet'i çalıştırın.

```PowerShell
$VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
```

### <a name="set-the-blob-storage-location-for-the-disk-to-be-used-by-the-virtual-machine"></a>Sanal makine tarafından kullanılacak disk için blob depolama konumu ayarlayın
Ardından, daha önce tanımlanan değişkenler kullanarak sanal makine tarafından kullanılacak disk için blob depolama konumu ayarlarız.

Blob depolama konumu ayarlamak için aşağıdaki cmdlet'i çalıştırın.

```PowerShell
$OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
```

### <a name="set-the-operating-system-disk-properties-for-the-virtual-machine"></a>İşletim sistemi sanal makine için disk özelliklerini ayarlama
Ardından, biz işletim sisteminin sanal makine için disk özelliklerini ayarlar. Kullanacağız [kümesi AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk) sanal makine için işletim sistemi yalnızca (SQL Server aynı disk üzerinde yüklü olduğundan) okumak için önbelleğe alma ayarlamak için bir görüntüden gelen ve sanal tanımlamak belirtmek için cmdlet makine adı ve daha önce tanımlanan değişkenler kullanılarak tanımlanan işletim sistemi diski.

İşletim sistemi, sanal makine disk özelliklerini ayarlamak için aşağıdaki cmdlet'i çalıştırın.

```PowerShell
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage
```

### <a name="specify-the-platform-image-for-the-virtual-machine"></a>Sanal makine için platform görüntüsü belirtin
Bizim son yapılandırma adımı bizim sanal makine için platform görüntüsü belirtmektir. En son SQL Server 2016 CTP görüntünün öğreticimizi için kullanıyoruz. Kullanacağız [kümesi AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) cmdlet'ini daha önce tanımlanan değişkenler tarafından tanımlandığı şekilde bu görüntüyü kullanın.

Sanal makineniz için platform görüntüsü belirtmek için aşağıdaki cmdlet'i çalıştırın.

```PowerShell
$VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version
```

## <a name="create-the-sql-vm"></a>SQL VM oluşturma
Yapılandırma adımlarını tamamladıktan, sanal makine oluşturmak hazır olursunuz. Kullanacağız [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) biz tanımladığınız değişkenleri kullanarak sanal makine oluşturmak için cmdlet'i.

Sanal makine oluşturmak için aşağıdaki cmdlet'i çalıştırın.

```PowerShell
New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine
```

Sanal makine oluşturulur. Premium depolama hesabı sanal makinenin disk için belirtilen depolama hesabı olduğu için standart depolama hesabı için önyükleme tanılaması oluşturduğunuz dikkat edin.

Artık bu makineyi görmek için Azure Portalı'nda görüntüleyebilirsiniz [ortak IP adresini ve tam etki alanı adını](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="example-script"></a>Örnek komut dosyası
Aşağıdaki komut dosyası, Bu öğretici için tam PowerShell komut dosyası içerir. İle kullanılacak Azure aboneliğini Kurulum zaten sahip olduğunuzu varsayar **Add-AzureRmAccount** ve **Select-AzureRmSubscription** komutları.

```PowerShell
# Variables

## Global
$Location = "SouthCentralUS"
$ResourceGroupName = "sqlvm1"

## Storage
$StorageName = $ResourceGroupName + "storage"
$StorageSku = "Premium_LRS"

## Network
$InterfaceName = $ResourceGroupName + "ServerInterface"
$VNetName = $ResourceGroupName + "VNet"
$SubnetName = "Default"
$VNetAddressPrefix = "10.0.0.0/16"
$VNetSubnetAddressPrefix = "10.0.0.0/24"
$TCPIPAllocationMethod = "Dynamic"
$DomainName = "sqlvm1"

##Compute
$VMName = $ResourceGroupName + "VM"
$ComputerName = $ResourceGroupName + "Server"
$VMSize = "Standard_DS13"
$OSDiskName = $VMName + "OSDisk"

##Image
$PublisherName = "MicrosoftSQLServer"
$OfferName = "SQL2016-WS2016"
$Sku = "Enterprise"
$Version = "latest"

# Resource Group
New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

# Storage
$StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

# Network
$SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
$VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
$PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
$Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

# Compute
$VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
$Credential = Get-Credential -Message "Type the name and password of the local administrator account."
$VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
$VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
$OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

# Image
$VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

## Create the VM in Azure
New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine
```

## <a name="next-steps"></a>Sonraki adımlar
Sanal makine oluşturulduktan sonra sanal makineye RDP ve Kurulum bağlantısı kullanarak bağlanmak hazır olursunuz. Daha fazla bilgi için bkz: [bir SQL Server sanal makinesine bağlanma Azure (Kaynak Yöneticisi) üzerinde](virtual-machines-windows-sql-connect.md).
