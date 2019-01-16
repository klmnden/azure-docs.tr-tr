---
title: Azure PowerShell ile SQL Server Vm'leri için sağlama Kılavuzu | Microsoft Docs
description: SQL Server sanal makine galeri görüntüleri ile Azure VM oluşturmak için adımları ve PowerShell komutları sağlar.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-resource-manager
ms.assetid: 98d50dd8-48ad-444f-9031-5378d8270d7b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 12/21/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 8282901dfcacb7ca0330274d114cd4c2990fdec1
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54330459"
---
# <a name="how-to-provision-sql-server-virtual-machines-with-azure-powershell"></a>Azure PowerShell ile SQL Server sanal makineleri sağlama

Bu kılavuz, Azure PowerShell ile Windows SQL Server Vm'leri oluşturmak için seçenekleri açıklar. Daha fazla varsayılan değerlerle kolaylaştırılmış bir Azure PowerShell örnek için bkz [SQL VM Azure PowerShell Hızlı Başlangıç](quickstart-sql-vm-create-powershell.md).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Bu makalede, Azure PowerShell modülü 3.6 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-azurerm-ps).

## <a name="configure-your-subscription"></a>Aboneliğinizi yapılandırma

1. PowerShell'i açıp **Connect-AzureRmAccount** komutunu çalıştırarak Azure hesabınıza erişim sağlayın.

   ```PowerShell
   Connect-AzureRmAccount
   ```

1. Kimlik bilgilerinizi girmeniz için bir ekran görürsünüz. Azure portala giriş yapmak için aynı e-posta adresini ve parolayı kullanın.

## <a name="define-image-variables"></a>Görüntü değişkenleri tanımlayın
Değerleri yeniden ve betik oluşturma işlemini basitleştirmek için değişken sayısı tanımlayarak başlatın. Ancak adlandırma kısıtlamaları Ad uzunlukları ve özel karakterler için sağlanan değerler değiştirirken ilgili uyumlu parametre değerlerini değiştirin.

### <a name="location-and-resource-group"></a>Konum ve kaynak grubu
Veri bölgesi ve diğer VM kaynakları oluşturduğunuz kaynak grubunu tanımlayın.

Daha sonra bu değişkenleri başlatmak için bu cmdlet'leri çalıştırmak ve istediğiniz gibi değiştirin.

```PowerShell
$Location = "SouthCentralUS"
$ResourceGroupName = "sqlvm2"
```

### <a name="storage-properties"></a>Depolama özellikleri
Depolama hesabı ve sanal makine tarafından kullanılan depolama türünü tanımlar.

Ardından bu değişkenlerini başlatmak için aşağıdaki cmdlet'i çalıştırın ve istediğiniz gibi değiştirin. Kullanmanızı öneririz [Premium depolama](../premium-storage.md) üretim iş yükleri için.

```PowerShell
$StorageName = $ResourceGroupName + "storage"
$StorageSku = "Premium_LRS"
```

### <a name="network-properties"></a>Ağ özellikleri
Sanal makinedeki ağ tarafından kullanılan özellikleri tanımlar. 

- Ağ arabirimi
- TCP/IP'yi ayırma yöntemi
- Sanal ağ adı
- Sanal alt ağ adı
- IP adresi aralığı için sanal ağ
- IP adresi aralığı alt ağ için
- Genel etki alanı adı etiketi

Daha sonra bu değişkenleri başlatmak için bu cmdlet'i çalıştırmak ve istediğiniz gibi değiştirin.

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
Sanal makine adı, bilgisayar adı, sanal makine boyutu ve sanal makine için işletim sistemi disk adı tanımlayın.

Daha sonra bu değişkenleri başlatmak için bu cmdlet'i çalıştırmak ve istediğiniz gibi değiştirin.

```PowerShell
$VMName = $ResourceGroupName + "VM"
$ComputerName = $ResourceGroupName + "Server"
$VMSize = "Standard_DS13"
$OSDiskName = $VMName + "OSDisk"
```

### <a name="choose-a-sql-server-image"></a>Bir SQL Server görüntüsü seçin
SQL Server sanal makine için kullanılacak görüntüyü tanımlayın.

1. İlk olarak, tüm SQL Server görüntü teklifleriyle listesi **Get-Azurermvmımageoffer** komutu:

   ```PowerShell
   Get-AzureRmVMImageOffer -Location $Location -Publisher 'MicrosoftSQLServer'
   ```

1. Bu öğretici için Windows Server 2016 üzerinde SQL Server 2017 belirtmek için aşağıdaki değişkenleri kullanın.

   ```PowerShell
   $OfferName = "SQL2017-WS2016"
   $PublisherName = "MicrosoftSQLServer"
   $Version = "latest"
   ```

1. Ardından, teklifiniz için kullanılabilir sürümler listesinde.

   ```PowerShell
   Get-AzureRmVMImageSku -Location $Location -Publisher 'MicrosoftSQLServer' -Offer $OfferName | Select Skus
   ```

1. Bu öğreticide, SQL Server 2017 Developer edition kullanın (**SQLDEV**). Developer sürümü test ve geliştirme için ücretsiz lisanslıdır ve yalnızca VM çalıştırmanın maliyeti için ödeme yaparsınız.

   ```PowerShell
   $Sku = "SQLDEV"
   ```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Resource Manager dağıtım modeliyle, oluşturduğunuz ilk nesnenin kaynak grubudur. Kullanım [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) bir Azure kaynak grubu ve kaynaklarını oluşturmak için cmdlet'i. Kaynak grubu adı ve konumu için daha önce başlatılmış değişkenleri belirtin.

Yeni kaynak grubunuzu oluşturmak için bu cmdlet'i çalıştırın.

```PowerShell
New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma
Sanal makine işletim sistemi diski ve SQL Server veri ve günlük dosyaları için depolama kaynaklarını gerektirir. Kolaylık olması için her ikisi için tek bir diske oluşturacaksınız. Ek diskleri ekleyebilirsiniz daha sonra kullanarak [Ekle-Azure Disk](/powershell/module/servicemanagement/azure/add-azuredisk) adanmış diskler üzerinde SQL Server verilerini ve günlük yerleştirmek için cmdlet dosyaları. Kullanım [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) yeni kaynak grubunuzda bir standart depolama hesabı oluşturmak için cmdlet'i. Depolama hesabı adı, depolama Sku adı ve konumu için daha önce başlatılmış değişkenleri belirtin.

Yeni depolama hesabınızı oluşturmak için bu cmdlet'i çalıştırın.

```PowerShell
$StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName `
   -Name $StorageName -SkuName $StorageSku `
   -Kind "Storage" -Location $Location
```

> [!TIP]
> Depolama hesabı oluşturma birkaç dakika sürebilir.

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma
Sanal makine ağ kaynaklarının sayısını için ağ bağlantısı gerektirir.

* Her sanal makine bir sanal ağ gerektirir.
* Bir sanal ağ, tanımlanan en az bir alt ağ olması gerekir.
* Bir ağ arabirimi, genel veya özel bir IP adresi ile tanımlanmalıdır.

### <a name="create-a-virtual-network-subnet-configuration"></a>Bir sanal ağ alt ağ yapılandırması
Sanal ağınız için bir alt ağ yapılandırması oluşturmaya başlayın. Bu öğretici için varsayılan alt ağ kullanarak oluşturma [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) cmdlet'i. Alt ağ adı ve adres ön eki için daha önce başlatılmış değişkenleri belirtin.

> [!NOTE]
> Bu cmdlet kullanarak sanal ağ alt ağ yapılandırmasını ek özelliklerini tanımlayabilirsiniz, ancak, bu öğreticinin kapsamı dışındadır.

Sanal alt ağ yapılandırmanızı oluşturmak için bu cmdlet'i çalıştırın.

```PowerShell
$SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
```

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma
Ardından, yeni kaynak grubu kullanarak sanal ağınızı oluşturma [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) cmdlet'i. Adı, konumu ve adres ön eki için daha önce başlatılmış değişkenleri belirtin. Önceki adımda tanımlanan alt ağ yapılandırmasını kullanın.

Sanal ağınızı oluşturmak için bu cmdlet'i çalıştırın.

```PowerShell
$VNet = New-AzureRmVirtualNetwork -Name $VNetName `
   -ResourceGroupName $ResourceGroupName -Location $Location `
   -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
```

### <a name="create-the-public-ip-address"></a>Genel IP adresi oluşturma
Sanal ağınızda tanımlanmış, sanal makine bağlantısı için bir IP adresi yapılandırmanız gerekir. Bu öğretici için dinamik IP adresleme Internet bağlantısı desteklemek için kullanarak genel bir IP adresi oluşturun. Kullanım [New-Azurermpublicıpaddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) genel IP adresini yeni kaynak grubunuzu oluşturmak için cmdlet'i. Adı, konum, ayırma yöntemini ve DNS etki alanı adı etiketi için daha önce başlatılmış değişkenleri belirtin.

> [!NOTE]
> Bu cmdlet kullanarak genel IP adresini ek özelliklerini tanımlayabilirsiniz, ancak bu ilk Bu öğreticinin kapsamı dışındadır. Statik bir adres ile özel bir adres veya adres da oluşturabilirsiniz, ancak, ayrıca bu öğreticinin kapsamı dışındadır.

Genel IP adresi oluşturmak için bu cmdlet'i çalıştırın.

```PowerShell
$PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName `
   -ResourceGroupName $ResourceGroupName -Location $Location `
   -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
```

### <a name="create-the-network-security-group"></a>Ağ güvenlik grubu oluşturma
VM ve SQL Server trafiği güvenli hale getirmek için bir ağ güvenlik grubu oluşturun.

1. İlk olarak Uzak Masaüstü bağlantılarına izin verecek şekilde RDP için bir ağ güvenlik grubu kuralı oluşturun.

   ```PowerShell
   $NsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name "RDPRule" -Protocol Tcp `
      -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 3389 -Access Allow
   ```
1. 1433 numaralı TCP bağlantı noktası üzerinde trafiğe izin veren bir ağ güvenlik grubu kuralı yapılandırın. Bunun yapılması internet üzerinden SQL Server'a bağlantılar sağlar.

   ```PowerShell
   $NsgRuleSQL = New-AzureRmNetworkSecurityRuleConfig -Name "MSSQLRule"  -Protocol Tcp `
      -Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 1433 -Access Allow
   ```

1. Ağ güvenlik grubu oluşturun.

   ```PowerShell
   $Nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $ResourceGroupName `
      -Location $Location -Name $NsgName `
      -SecurityRules $NsgRuleRDP,$NsgRuleSQL
   ```

### <a name="create-the-network-interface"></a>Ağ arabirimini oluşturun
Sanal makineniz için ağ arabirimi oluşturmak artık hazırsınız. Kullanım [New-Azurermnetworkınterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) cmdlet'ini yeni kaynak grubunuz, ağ arabirimini oluşturun. Adı, konum, alt ağ ve daha önce tanımlanan genel IP adresi belirtin.

Ağ Arabiriminizin oluşturmak için bu cmdlet'i çalıştırın.

```PowerShell
$Interface = New-AzureRmNetworkInterface -Name $InterfaceName `
   -ResourceGroupName $ResourceGroupName -Location $Location `
   -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id `
   -NetworkSecurityGroupId $Nsg.Id
```

## <a name="configure-a-vm-object"></a>Bir VM nesnesine yapılandırın
Depolama ve ağ kaynaklarını tanımlanır, sanal makine için işlem kaynaklarını tanımlayın hazırsınız.

- Sanal makine boyutu ve çeşitli işletim sistemi özelliklerini belirtin.
- Daha önce oluşturduğunuz ağ arabirimini belirtin.
- BLOB Depolama tanımlayın.
- İşletim sistemi diski belirtin.

### <a name="create-the-vm-object"></a>VM nesnesini oluşturun
Sanal makine boyutunu belirleyerek başlayın. Bu öğreticide, bir DS13 belirtin. Kullanım [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) bir sanal makine yapılandırılabilir nesnesi oluşturmak için cmdlet'i. Ad ve boyut için daha önce başlatılmış değişkenleri belirtin.

Sanal makine nesnesini oluşturmak için bu cmdlet'i çalıştırın.

```PowerShell
$VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
```

### <a name="create-a-credential-object-to-hold-the-name-and-password-for-the-local-administrator-credentials"></a>Adı ve parola yerel yönetici kimlik bilgileri tutmak için bir kimlik bilgisi nesnesini oluşturun
Sanal makine için işletim sistemi özelliklerini ayarlayabilmek için önce güvenli bir dize olarak yerel yönetici hesabının kimlik bilgilerini sağlamanız gerekir. Bunu gerçekleştirmek için kullanın [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet'i.

Aşağıdaki cmdlet'i çalıştırın ve PowerShell kimlik bilgileri isteği penceresinde, adı ve sanal makinede yerel yönetici hesabı için kullanılacak parolayı yazın.

```PowerShell
$Credential = Get-Credential -Message "Type the name and password of the local administrator account."
```

### <a name="set-the-operating-system-properties-for-the-virtual-machine"></a>Sanal makine için işletim sistemi özelliklerini ayarlama
İle sanal makinenin işletim sistemi özelliklerini ayarlamak hazır artık [kümesi AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) cmdlet'i.

- İşletim sistemi türü Windows ayarlayın.
- Gerekli [sanal makine Aracısı](../../extensions/agent-windows.md) yüklenecek.
- Cmdlet, otomatik güncelleştirme etkinleştirir belirtin.
- Sanal makine adı, bilgisayar adını ve kimlik bilgisi için daha önce başlatılmış değişkenleri belirtin.

Sanal makineniz için işletim sistemi özelliklerini ayarlamak için bu cmdlet'i çalıştırın.

```PowerShell
$VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine `
   -Windows -ComputerName $ComputerName -Credential $Credential `
   -ProvisionVMAgent -EnableAutoUpdate
```

### <a name="add-the-network-interface-to-the-virtual-machine"></a>Sanal makineye bir ağ arabirimi ekleyin
Ardından, [Add-Azurermvmnetworkınterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) cmdlet'ini kullanarak daha önce tanımladığınız değişkeni ağ arabirimi ekleyin.

Sanal makinenizin ağ arabirimine ayarlama için bu cmdlet'i çalıştırın.

```PowerShell
$VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
```

### <a name="set-the-blob-storage-location-for-the-disk-to-be-used-by-the-virtual-machine"></a>Sanal makine tarafından kullanılacak disk için blob depolama konumunu ayarla
Ardından, daha önce tanımlanan değişkenleri kullanarak sanal makinenin diski blob depolama konumunu ayarlayın.

Blob depolama konumunu ayarlamak için bu cmdlet'i çalıştırın.

```PowerShell
$OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
```

### <a name="set-the-operating-system-disk-properties-for-the-virtual-machine"></a>İşletim sistemi sanal makine disk özelliklerini ayarlama
Ardından, işletim sistemi disk özellikleri kullanarak sanal makine için ayarlanmış [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk) cmdlet'i. 

- İşletim sistemi için sanal makineyi bir görüntüden gelir belirtin.
- Yalnızca (SQL Server aynı disk üzerinde yüklü olduğundan) okumak için önbelleğe alma ayarlayın.
- VM adını ve işletim sistemi diski için daha önce başlatılmış değişkenleri belirtin.

İşletim sistemi, sanal makine disk özelliklerini ayarlamak için bu cmdlet'i çalıştırın.

```PowerShell
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name `
   $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage
```

### <a name="specify-the-platform-image-for-the-virtual-machine"></a>Sanal makine için platform görüntüsü belirtin
Son yapılandırma adımı, sanal makineniz için platform görüntüsü belirtmektir. Bu öğreticide, en son SQL Server 2016 CTP görüntüyü kullanın. Kullanma [kümesi AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) bu görüntü daha önce tanımlanan değişkenleri ile kullanmak için cmdlet.

Sanal makineniz için platform görüntüsü belirtmek için bu cmdlet'i çalıştırın.

```PowerShell
$VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine `
   -PublisherName $PublisherName -Offer $OfferName `
   -Skus $Sku -Version $Version
```

## <a name="create-the-sql-vm"></a>SQL VM'sini oluşturma
Yapılandırma adımları bitirdikten sonra sanal makine oluşturmaya hazırsınız. Kullanım [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) tanımlanan değişkenleri kullanarak sanal makine oluşturmak için cmdlet'i.

> [!TIP]
> VM oluşturma birkaç dakika sürebilir.

Sanal makinenizi oluşturmak için bu cmdlet'i çalıştırın.

```PowerShell
New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine
```

Sanal makine oluşturulur.

> [!NOTE]
> Önyükleme tanılaması hakkında bir hata alırsanız, onu yok sayabilirsiniz. Belirtilen depolama hesabı için sanal makinenin disk bir premium depolama hesabı olduğu için önyükleme tanılaması için standart depolama hesabı oluşturulur.

## <a name="install-the-sql-iaas-agent"></a>SQL Iaas Aracısı'nı yükleme
SQL Server sanal makineleri ile otomatik yönetim özelliklerini destekleyen [SQL Server Iaas Aracısı uzantısı](virtual-machines-windows-sql-server-agent-extension.md). Aracıyı yeni VM'ye yüklemek için oluşturulduktan sonra aşağıdaki komutu çalıştırın.


   ```PowerShell
   Set-AzureRmVMSqlServerExtension -ResourceGroupName $ResourceGroupName -VMName $VMName -name "SQLIaasExtension" -version "1.2" -Location $Location
   ```

## <a name="stop-or-remove-a-vm"></a>Durdurma veya bir VM Kaldır

Sanal Makinenin sürekli olarak çalışmasını gerekmiyorsa, kullanımda olmadığında durdurarak gereksiz ödeme yapmaktan kaçının. Aşağıdaki komut VM'yi durdurur ancak özelliği daha sonra kullanılmak üzere muhafaza eder.

```PowerShell
Stop-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
```

Ayrıca **Remove-AzureRmResourceGroup** komutuyla sanal makineyle ilişkilendirilmiş olan tüm kaynakları kalıcı olarak silebilirsiniz. Sanal makineyi de kalıcı olarak bunu siler, bu komutu dikkatli kullanın.

## <a name="example-script"></a>Örnek betik
Aşağıdaki komut dosyası, Bu öğretici için tam PowerShell Betiği içerir. Bu, zaten Azure aboneliğiniz ile kullanılacak belirlediğinizi varsayar **Connect-AzureRmAccount** ve **Select-AzureRmSubscription** komutları.

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
$Nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $ResourceGroupName -Location $Location -Name $NsgName -SecurityRules $NsgRuleRDP,$NsgRuleSQL
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

- RDP kullanarak sanal makineye bağlanın
- VM'niz için portalda SQL Server ayarlarını yapılandırma dahil olmak üzere:
   - [Depolama ayarları](virtual-machines-windows-sql-server-storage-configuration.md) 
   - [Otomatik yönetim görevleri](virtual-machines-windows-sql-server-agent-extension.md)
- [Bağlantı yapılandırma](virtual-machines-windows-sql-connect.md)
- İstemciler ve uygulamalar için yeni SQL Server örneğine bağlanın

