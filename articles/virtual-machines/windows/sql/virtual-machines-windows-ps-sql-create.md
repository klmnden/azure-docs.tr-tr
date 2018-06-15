---
title: Azure PowerShell ile SQL Server Vm'lerinin Kılavuzu Sağlama | Microsoft Docs
description: Bir Azure VM ile SQL Server sanal makineye Galerisi görüntüleri oluşturmak için adımlar ve PowerShell komutlarını sağlar.
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: craigg
editor: ''
tags: azure-resource-manager
ms.assetid: 98d50dd8-48ad-444f-9031-5378d8270d7b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 02/15/2018
ms.author: jroth
ms.openlocfilehash: 2f0d9c42e32f2dd1181eac8d74c324b5ff2b0c53
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33944527"
---
# <a name="how-to-provision-sql-server-virtual-machines-with-azure-powershell"></a>Azure PowerShell ile SQL Server sanal makineler sağlamak nasıl

Bu kılavuz, Azure PowerShell ile Windows SQL Server Vm'lerinin oluşturmak için seçenekleri açıklar. Daha fazla varsayılan değerlerle kolaylaştırılmış bir Azure PowerShell örnek için bkz: [SQL VM Azure PowerShell quickstart](quickstart-sql-vm-create-powershell.md).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Bu makalede, Azure PowerShell modülü 3,6 veya sonraki bir sürümü gerektiriyor. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-azurerm-ps).

## <a name="configure-your-subscription"></a>Aboneliğinizi yapılandırın

1. PowerShell'i açıp **Connect-AzureRmAccount** komutunu çalıştırarak Azure hesabınıza erişim sağlayın.

   ```PowerShell
   Connect-AzureRmAccount
   ```

1. Kimlik bilgilerinizi girebileceğiniz bir oturum açma ekranı görmeniz gerekir. Azure portala giriş yapmak için aynı e-posta adresini ve parolayı kullanın.

## <a name="define-image-variables"></a>Görüntü değişkenleri tanımlayın
Yeniden kullanma ve komut dosyası oluşturma işlemini basitleştirmek için bir dizi değişken tanımlayarak başlatın. Uygun gördüğünüz, ancak sağlanan değerler değiştirirken adı uzunlukları ve özel karakterler için ilgili kısıtlamaları adlandırma dikkat parametre değerlerini değiştirin.

### <a name="location-and-resource-group"></a>Konum ve kaynak grubu
İki değişken veri bölgesi ve sanal makine için diğer kaynaklar oluşturduğunuz kaynak grubunu tanımlamak için kullanın.

İstediğiniz şekilde değiştirin ve bu değişkenleri başlatmak için aşağıdaki cmdlet'leri çalıştırın.

```PowerShell
$Location = "SouthCentralUS"
$ResourceGroupName = "sqlvm2"
```

### <a name="storage-properties"></a>Depolama özellikleri
Depolama hesabı ve sanal makine tarafından kullanılacak depolama türünü tanımlamak için aşağıdaki değişkenleri kullanın.

İstediğiniz şekilde değiştirin ve bu değişkenleri başlatmak için aşağıdaki cmdlet'i çalıştırın. Bu örnekte, biz kullanıyoruz Not [Premium depolama](../premium-storage.md), üretim iş yükleri için önerilir.

```PowerShell
$StorageName = $ResourceGroupName + "storage"
$StorageSku = "Premium_LRS"
```

### <a name="network-properties"></a>Ağ Özellikleri
Ağ arabirimi, TCP/IP'yi ayırma yöntemi, sanal ağ adı, sanal alt ağ adı, sanal ağ için IP aralığı adresleri, IP adresi aralığı alt ağı ve genel etki alanı adı etiketi için tanımlamak için aşağıdaki değişkenleri kullanın Sanal makinede ağ tarafından kullanılmak üzere.

İstediğiniz şekilde değiştirin ve bu değişkenleri başlatmak için aşağıdaki cmdlet'i çalıştırın.

```PowerShell
$InterfaceName = $ResourceGroupName + "ServerInterface"
$NsgName = $ResourceGroupName + "nsg"
$TCPIPAllocationMethod = "Dynamic"
$VNetName = $ResourceGroupName + "VNet"
$SubnetName = "Default"
$VNetAddressPrefix = "10.0.0.0/16"
$VNetSubnetAddressPrefix = "10.0.0.0/24"
$DomainName = $ResourceGroupName
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

### <a name="choose-a-sql-server-image"></a>Bir SQL Server görüntüsü seçin
Sanal makine için kullanılacak SQL Server görüntüsü tanımlamak için aşağıdaki değişkenleri kullanın.

1. İlk olarak, tüm SQL Server görüntüsü teklifleriyle listesinde **Get-AzureRmVMImageOffer** komutu:

   ```PowerShell
   Get-AzureRmVMImageOffer -Location $Location -Publisher 'MicrosoftSQLServer'
   ```

1. Bu öğretici için aşağıdaki değişkenleri Windows Server 2016 SQL Server 2017 belirtmek için kullanın.

   ```PowerShell
   $OfferName = "SQL2017-WS2016"
   $PublisherName = "MicrosoftSQLServer"
   $Version = "latest"
   ```

1. Teklifiniz için kullanılabilir sürümlerin listesini İleri.

   ```PowerShell
   Get-AzureRmVMImageSku -Location $Location -Publisher 'MicrosoftSQLServer' -Offer $OfferName | Select Skus
   ```

1. Bu öğretici için SQL Server 2017 Geliştirici sürümü kullanın (**SQLDEV**). Geliştirici sürümü serbestçe test ve geliştirme için lisanslı ve yalnızca VM çalıştıran maliyetini için ödeme yaparsınız.

   ```PowerShell
   $Sku = "SQLDEV"
   ```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Resource Manager dağıtım modeli ile oluşturduğunuz ilk kaynak grubu nesnesidir. Kullanım [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) kaynak grubu adı ve daha önce başlatılmış değişkenleri tarafından tanımlanmış konuma sahip bir Azure kaynak grubu ve kaynaklarına oluşturmak için cmdlet'i.

Yeni kaynak grubunuzu oluşturmak için aşağıdaki cmdlet'i çalıştırın.

```PowerShell
New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma
Sanal makine, SQL Server veri ve günlük dosyalarını ve işletim sistemi diski için depolama kaynaklarını gerektirir. Kolaylık olması için tek bir disk için her ikisini de oluşturuyoruz. Ek diskleri iliştirebilirsiniz daha sonra kullanılarak [Ekle-Azure diski](/powershell/module/azure/add-azuredisk) SQL Server veri ve günlük yerleştirmek için cmdlet dosyaları adanmış diskler üzerinde. Kullanım [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) yeni kaynak grubunuz ve depolama hesabı adı, depolama Sku adı ve konumu daha önce başlatılmış değişkenleri kullanılarak tanımlanan bir standart depolama hesabı oluşturmak için cmdlet'i .

Yeni depolama hesabınızı oluşturmak için aşağıdaki cmdlet'i çalıştırın.

```PowerShell
$StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName `
   -Name $StorageName -SkuName $StorageSku `
   -Kind "Storage" -Location $Location
```

> [!TIP]
> Depolama hesabı oluşturma, birkaç dakika sürebilir.

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma
Sanal makine ağ kaynaklarının sayısını için ağ bağlantısı gerektirir.

* Her sanal makine bir sanal ağ gerektirir.
* Bir sanal ağ en az bir alt ağ tanımlanmış olması gerekir.
* Bir ağ arabirimi genel veya özel bir IP adresi ile tanımlanması gerekir.

### <a name="create-a-virtual-network-subnet-configuration"></a>Bir sanal ağ alt ağ yapılandırması oluştur
Bizim sanal ağ için bir alt ağ yapılandırması oluşturmaya başlayın. Bizim öğretici için kullanarak bir varsayılan alt ağ olan oluşturuyoruz [yeni AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) cmdlet'i. Daha önce başlatılmış değişkenleri kullanılarak tanımlanan alt ağ adı ve adres öneki ile sanal ağ alt ağı yapılandırmamızın oluşturuyoruz.

> [!NOTE]
> Bu cmdlet kullanarak sanal ağ alt ağ yapılandırması ek özelliklerini tanımlayabilirsiniz, ancak, Bu öğretici kapsamında değildir.

Sanal alt ağ yapılandırmanızı oluşturmak için aşağıdaki cmdlet'i çalıştırın.

```PowerShell
$SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
```

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma
Ardından, kullanarak sanal ağ oluşturma [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) cmdlet'i. Adını, konumunu ve adres öneki daha önce başlatılmış değişkenleri kullanma ve önceki adımda tanımlanan alt ağ yapılandırması kullanma tanımlı, yeni kaynak grubunuz sanal ağ oluşturun.

Sanal ağınızı oluşturmak için aşağıdaki cmdlet'i çalıştırın.

```PowerShell
$VNet = New-AzureRmVirtualNetwork -Name $VNetName `
   -ResourceGroupName $ResourceGroupName -Location $Location `
   -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
```

### <a name="create-the-public-ip-address"></a>Ortak IP adresi oluştur
Tanımlanan bizim sanal ağ sahibiz, sanal makine bağlantısı için bir IP adresi yapılandırmanız gerekir. Bu öğretici için dinamik IP Internet bağlantısı desteklemek için adresi kullanarak bir ortak IP adresi oluşturun. Kullanım [yeni AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) genel IP oluşturmak için cmdlet'i adres daha önce oluşturduğunuz kaynak grubunda ve adını, konumunu, ayırma yöntemi ve değişkenleri kullanılarak tanımlanmış DNS etki alanı adı etiketi ile daha önce başlatıldı.

> [!NOTE]
> Bu cmdlet kullanılarak ortak IP adresinin ek özellikler tanımlayabilirsiniz, ancak, ilk Bu öğretici kapsamında değildir. Statik adresi ile özel bir adres veya adresi de oluşturabilir, ancak, aynı zamanda Bu öğretici kapsamında değildir.

Ortak IP adresi oluşturmak için aşağıdaki cmdlet'i çalıştırın.

```PowerShell
$PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName `
   -ResourceGroupName $ResourceGroupName -Location $Location `
   -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
```

### <a name="create-the-network-security-group"></a>Ağ güvenlik grubu oluşturun
VM ve SQL Server trafiğinin güvenliğini sağlamak için bir ağ güvenlik grubu oluşturun.

1. İlk olarak, Uzak Masaüstü bağlantılara izin vermek RDP için ağ güvenlik grubu kural oluşturun.

   ```PowerShell
   $NsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name "RDPRule" -Protocol Tcp `
      -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 3389 -Access Allow
   ```
1. TCP bağlantı noktası 1433 trafiğe izin veren bir ağ güvenlik grubu kural yapılandırabilir. Bu, internet üzerinden SQL Server bağlantılarını sağlar.

   ```PowerShell
   $NsgRuleSQL = New-AzureRmNetworkSecurityRuleConfig -Name "MSSQLRule"  -Protocol Tcp `
      -Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 1433 -Access Allow
   ```

1. Ardından ağ güvenlik grubu oluşturun.

   ```PowerShell
   $Nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $ResourceGroupName `
      -Location $Location -Name $NsgName `
      -SecurityRules $NsgRuleRDP,$NsgRuleSQL
   ```

### <a name="create-the-network-interface"></a>Ağ arabirimi oluştur
Biz şimdi bizim sanal makinenin kullanacağı ağ arabirim oluşturmak hazır olursunuz. Diyoruz [yeni AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) daha önce oluşturduğunuz kaynak grubunda ve adını, konumunu, alt ağ ve genel IP adresi ile bizim ağ arabirim oluşturmak için cmdlet'i önceden tanımlanmış.

Ağ arabirimi oluşturmak üzere aşağıdaki cmdlet'i çalıştırın.

```PowerShell
$Interface = New-AzureRmNetworkInterface -Name $InterfaceName `
   -ResourceGroupName $ResourceGroupName -Location $Location `
   -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id `
   -NetworkSecurityGroupId $Nsg.Id
```

## <a name="configure-a-vm-object"></a>Bir VM nesnesine yapılandırın
Biz tanımlanan depolama ve ağ kaynaklarına sahip olduğunuza göre biz sanal makine için işlem kaynakları tanımlamak hazır olursunuz. Bizim öğretici için sanal makine boyutu ve çeşitli işletim sistemi özelliklerini belirtin, daha önce oluşturduğumuz, ağ arabiriminin blob depolama alanını tanımlayın ve ardından işletim sistemi diski belirtin belirtin.

### <a name="create-the-vm-object"></a>VM nesnesi oluşturun
Sanal makine boyutu belirterek başlayın. Bu öğretici için size bir DS13 belirlersiniz. Diyoruz [yeni AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) adı ve daha önce başlatılmış değişkenleri kullanılarak tanımlanan boyutu ile yapılandırılabilir sanal makine nesnesi oluşturmak için cmdlet'i.

Sanal makine nesnesini oluşturmak için aşağıdaki cmdlet'i çalıştırın.

```PowerShell
$VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
```

### <a name="create-a-credential-object-to-hold-the-name-and-password-for-the-local-administrator-credentials"></a>Adı ve parola yerel yönetici kimlik bilgilerini saklamak için bir kimlik bilgisi nesnesi oluşturun
Sanal makine için işletim sistemi özelliklerini ayarlayabilmeniz için önce güvenli bir dize olarak yerel yönetici hesabının kimlik bilgilerini sağlamanız gerekir. Bunu gerçekleştirmek için kullanın [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet'i.

Aşağıdaki cmdlet'i yürütün ve PowerShell kimlik bilgisi isteği penceresinde, adı ve sanal makinesinde yerel yönetici hesabı için kullanılacak parolayı yazın.

```PowerShell
$Credential = Get-Credential -Message "Type the name and password of the local administrator account."
```

### <a name="set-the-operating-system-properties-for-the-virtual-machine"></a>Sanal makine için işletim sistemi özelliklerini ayarlama
Biz sanal makinenin işletim sistemi özellikleri ile ayarlamak artık [kümesi AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) , Windows işletim sisteminin türünü ayarlamak için cmdlet gerektiren [sanal makine aracısını](../../extensions/agent-windows.md) Yüklenecek, cmdlet otomatik güncelleştirme sağlar belirtin ve sanal makine adı, bilgisayar adı ve daha önce başlatılmış değişkenler kullanarak kimlik bilgilerini ayarlayın.

Sanal makine için işletim sistemi özelliklerini ayarlamak için aşağıdaki cmdlet'i çalıştırın.

```PowerShell
$VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine `
   -Windows -ComputerName $ComputerName -Credential $Credential `
   -ProvisionVMAgent -EnableAutoUpdate
```

### <a name="add-the-network-interface-to-the-virtual-machine"></a>Ağ arabirimi sanal makineye ekleyin
Ardından, biz oluşturduğumuz ağ arabirimini daha önce sanal makineye ekleyin. Çağrı [Ekle AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) cmdlet'ini daha önce tanımlanan ağ arabirimi değişkenini kullanarak ağ arabirimi ekleyin.

Ağ arabirimi sanal makineniz için ayarlamak için aşağıdaki cmdlet'i çalıştırın.

```PowerShell
$VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
```

### <a name="set-the-blob-storage-location-for-the-disk-to-be-used-by-the-virtual-machine"></a>Sanal makine tarafından kullanılacak disk için blob depolama konumu ayarlayın
Ardından, daha önce tanımlanan değişkenler kullanarak sanal makine tarafından kullanılacak disk için blob depolama konumunu ayarlayın.

Blob depolama konumu ayarlamak için aşağıdaki cmdlet'i çalıştırın.

```PowerShell
$OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
```

### <a name="set-the-operating-system-disk-properties-for-the-virtual-machine"></a>İşletim sistemi sanal makine için disk özelliklerini ayarlama
Ardından, işletim sisteminin sanal makine için disk özellikleri ayarlayın. Kullanım [kümesi AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk) sanal makine için işletim sisteminin bir görüntüsünü yalnızca (SQL Server aynı disk üzerinde yüklü olduğundan) okumak için önbelleğe alma ayarlama ve sanal makine tanımlamak için gelecek belirtmek için cmdlet ad ve daha önce tanımlanan değişkenler kullanılarak tanımlanan işletim sistemi diski.

İşletim sistemi, sanal makine disk özelliklerini ayarlamak için aşağıdaki cmdlet'i çalıştırın.

```PowerShell
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name `
   $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage
```

### <a name="specify-the-platform-image-for-the-virtual-machine"></a>Sanal makine için platform görüntüsü belirtin
Bizim son yapılandırma adımı bizim sanal makine için platform görüntüsü belirtmektir. En son SQL Server 2016 CTP görüntünün öğreticimizi için kullanıyoruz. Kullanmak [kümesi AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) cmdlet'ini daha önce tanımlanan değişkenler tarafından tanımlandığı şekilde bu görüntüyü kullanın.

Sanal makineniz için platform görüntüsü belirtmek için aşağıdaki cmdlet'i çalıştırın.

```PowerShell
$VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine `
   -PublisherName $PublisherName -Offer $OfferName `
   -Skus $Sku -Version $Version
```

## <a name="create-the-sql-vm"></a>SQL VM'sini oluşturma
Yapılandırma adımlarını tamamladıktan, sanal makine oluşturmak hazır olursunuz. Kullanım [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) biz tanımladığınız değişkenleri kullanarak sanal makine oluşturmak için cmdlet'i.

Sanal makine oluşturmak için aşağıdaki cmdlet'i çalıştırın.

```PowerShell
New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine
```

Sanal makine oluşturulur.

> [!NOTE]
> Önyükleme tanılaması ilgili hatayı yoksayabilirsiniz. Premium depolama hesabı sanal makinenin disk için belirtilen depolama hesabı olduğu için önyükleme tanılaması için standart depolama hesabı oluşturulur.

## <a name="install-the-sql-iaas-agent"></a>SQL Iaas Aracısı'nı yükleme
SQL Server sanal makineleri ile otomatik yönetim özelliklerini desteklemek [SQL Server Iaas Aracısı uzantısı](virtual-machines-windows-sql-server-agent-extension.md). Aracıyı yeni VM'ye yüklemek için oluşturulduktan sonra aşağıdaki komutu çalıştırın.

   ```PowerShell
   Set-AzureRmVMSqlServerExtension -ResourceGroupName $ResourceGroupName -VMName $VMName -name "SQLIaasExtension" -version "1.2" -Location $Location
   ```

## <a name="remove-a-test-vm"></a>Test VM Kaldır

VM'nin sürekli çalıştırılması gerekmiyorsa, kullanımda olmadığında durdurarak gereksiz ödeme yapmaktan kaçının. Aşağıdaki komut VM'yi durdurur ancak özelliği daha sonra kullanılmak üzere muhafaza eder.

```PowerShell
Stop-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
```

Ayrıca **Remove-AzureRmResourceGroup** komutuyla sanal makineyle ilişkilendirilmiş olan tüm kaynakları kalıcı olarak silebilirsiniz. Bu işlem sanal makineyi de kalıcı olarak sildiğinden, bu komutu dikkatli kullanın.

## <a name="example-script"></a>Örnek betik
Aşağıdaki komut dosyası, Bu öğretici için tam PowerShell komut dosyası içerir. İle kullanılacak Azure aboneliğini Kurulum zaten sahip olduğunuzu varsayar **Connect-AzureRmAccount** ve **Select-AzureRmSubscription** komutları.

```PowerShell
# Variables

## Global
$Location = "SouthCentralUS"
$ResourceGroupName = "sqlvm2"

## Storage
$StorageName = $ResourceGroupName + "storage"
$StorageSku = "Premium_LRS"

## Network
$InterfaceName = $ResourceGroupName + "ServerInterface"
$NsgName = $ResourceGroupName + "nsg"
$VNetName = $ResourceGroupName + "VNet"
$SubnetName = "Default"
$VNetAddressPrefix = "10.0.0.0/16"
$VNetSubnetAddressPrefix = "10.0.0.0/24"
$TCPIPAllocationMethod = "Dynamic"
$DomainName = $ResourceGroupName

##Compute
$VMName = $ResourceGroupName + "VM"
$ComputerName = $ResourceGroupName + "Server"
$VMSize = "Standard_DS13"
$OSDiskName = $VMName + "OSDisk"

##Image
$PublisherName = "MicrosoftSQLServer"
$OfferName = "SQL2017-WS2016"
$Sku = "SQLDEV"
$Version = "latest"

# Resource Group
New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

# Storage
$StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

# Network
$SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
$VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
$PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
$NsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name "RDPRule" -Protocol Tcp -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389 -Access Allow
$NsgRuleSQL = New-AzureRmNetworkSecurityRuleConfig -Name "MSSQLRule"  -Protocol Tcp -Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 1433 -Access Allow
$Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id -NetworkSecurityGroupId $Nsg.Id

# Compute
$VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
$Credential = Get-Credential -Message "Type the name and password of the local administrator account."
$VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
$VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
$OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

# Image
$VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

# Create the VM in Azure
New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

# Add the SQL IaaS Extension
Set-AzureRmVMSqlServerExtension -ResourceGroupName $ResourceGroupName -VMName $VMName -name "SQLIaasExtension" -version "1.2" -Location $Location
```

## <a name="next-steps"></a>Sonraki adımlar
Sanal makine oluşturulduktan sonra şunları yapabilirsiniz:

- Uzak Masaüstü (RDP) kullanarak sanal makineye bağlanın.
- SQL Server ayarlarını Portalı'nda, VM için yapılandırmak da dahil olmak üzere:
   - [Depolama ayarları](virtual-machines-windows-sql-server-storage-configuration.md) 
   - [Otomatik yönetim görevleri](virtual-machines-windows-sql-server-agent-extension.md)
- [Bağlantı yapılandırma](virtual-machines-windows-sql-connect.md).
- İstemciler ile uygulamaları yeni SQL Server örneğine bağlanın.

