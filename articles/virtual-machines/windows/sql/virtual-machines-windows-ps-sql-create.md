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
ms.openlocfilehash: edf5f2b681123243f55b1c2bf19a500e68171c0e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66165743"
---
# <a name="how-to-provision-sql-server-virtual-machines-with-azure-powershell"></a>Azure PowerShell ile SQL Server sanal makineleri sağlama

Bu kılavuz, Azure PowerShell ile Windows SQL Server Vm'leri oluşturmak için seçenekleri açıklar. Daha fazla varsayılan değerlerle kolaylaştırılmış bir Azure PowerShell örnek için bkz [SQL VM Azure PowerShell Hızlı Başlangıç](quickstart-sql-vm-create-powershell.md).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [updated-for-az.md](../../../../includes/updated-for-az.md)]

## <a name="configure-your-subscription"></a>Aboneliğinizi yapılandırma

1. PowerShell'i açın ve çalıştırarak Azure hesabınıza erişim oluşturmanız **Connect AzAccount** komutu.

   ```powershell
   Connect-AzAccount
   ```

1. Kimlik bilgilerinizi girmeniz için bir ekran görürsünüz. Azure portala giriş yapmak için aynı e-posta adresini ve parolayı kullanın.

## <a name="define-image-variables"></a>Görüntü değişkenleri tanımlayın
Değerleri yeniden ve betik oluşturma işlemini basitleştirmek için değişken sayısı tanımlayarak başlatın. Ancak adlandırma kısıtlamaları Ad uzunlukları ve özel karakterler için sağlanan değerler değiştirirken ilgili uyumlu parametre değerlerini değiştirin.

### <a name="location-and-resource-group"></a>Konum ve kaynak grubu
Veri bölgesi ve diğer VM kaynakları oluşturduğunuz kaynak grubunu tanımlayın.

Daha sonra bu değişkenleri başlatmak için bu cmdlet'leri çalıştırmak ve istediğiniz gibi değiştirin.

```powershell
$Location = "SouthCentralUS"
$ResourceGroupName = "sqlvm2"
```

### <a name="storage-properties"></a>Depolama özellikleri
Depolama hesabı ve sanal makine tarafından kullanılan depolama türünü tanımlar.

Ardından bu değişkenlerini başlatmak için aşağıdaki cmdlet'i çalıştırın ve istediğiniz gibi değiştirin. Kullanmanızı öneririz [premium SSD](../disks-types.md#premium-ssd) üretim iş yükleri için.

```powershell
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

```powershell
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

```powershell
$VMName = $ResourceGroupName + "VM"
$ComputerName = $ResourceGroupName + "Server"
$VMSize = "Standard_DS13"
$OSDiskName = $VMName + "OSDisk"
```

### <a name="choose-a-sql-server-image"></a>Bir SQL Server görüntüsü seçin

Sanal makine için kullanılacak SQL Server görüntüsünü tanımlamak için aşağıdaki değişkenleri kullanın. 

1. İlk olarak, tüm SQL Server görüntü teklifleriyle listesinde `Get-AzVMImageOffer` komutu. Bu komut, Azure Portalı'nda kullanılabilir olan geçerli görüntüleri ve ayrıca yalnızca PowerShell ile yüklenebilir eski görüntüler listeler:

   ```powershell
   Get-AzVMImageOffer -Location $Location -Publisher 'MicrosoftSQLServer'
   ```

1. Bu öğretici için Windows Server 2016 üzerinde SQL Server 2017 belirtmek için aşağıdaki değişkenleri kullanın.

   ```powershell
   $OfferName = "SQL2017-WS2016"
   $PublisherName = "MicrosoftSQLServer"
   $Version = "latest"
   ```

1. Ardından, teklifiniz için kullanılabilir sürümler listesinde.

   ```powershell
   Get-AzVMImageSku -Location $Location -Publisher 'MicrosoftSQLServer' -Offer $OfferName | Select Skus
   ```

1. Bu öğreticide, SQL Server 2017 Developer edition kullanın (**SQLDEV**). Developer sürümü test ve geliştirme için ücretsiz lisanslıdır ve yalnızca VM çalıştırmanın maliyeti için ödeme yaparsınız.

   ```powershell
   $Sku = "SQLDEV"
   ```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturun
Resource Manager dağıtım modeliyle, oluşturduğunuz ilk nesnenin kaynak grubudur. Kullanım [yeni AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) bir Azure kaynak grubu ve kaynaklarını oluşturmak için cmdlet'i. Kaynak grubu adı ve konumu için daha önce başlatılmış değişkenleri belirtin.

Yeni kaynak grubunuzu oluşturmak için bu cmdlet'i çalıştırın.

```powershell
New-AzResourceGroup -Name $ResourceGroupName -Location $Location
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma
Sanal makine işletim sistemi diski ve SQL Server veri ve günlük dosyaları için depolama kaynaklarını gerektirir. Kolaylık olması için her ikisi için tek bir diske oluşturacaksınız. Ek diskleri ekleyebilirsiniz daha sonra kullanarak [Ekle-Azure Disk](https://docs.microsoft.com/powershell/module/servicemanagement/azure/add-azuredisk) adanmış diskler üzerinde SQL Server verilerini ve günlük yerleştirmek için cmdlet dosyaları. Kullanım [yeni AzStorageAccount](https://docs.microsoft.com/powershell/module/az.storage/new-azstorageaccount) yeni kaynak grubunuzda bir standart depolama hesabı oluşturmak için cmdlet'i. Depolama hesabı adı, depolama Sku adı ve konumu için daha önce başlatılmış değişkenleri belirtin.

Yeni depolama hesabınızı oluşturmak için bu cmdlet'i çalıştırın.

```powershell
$StorageAccount = New-AzStorageAccount -ResourceGroupName $ResourceGroupName `
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
Sanal ağınız için bir alt ağ yapılandırması oluşturmaya başlayın. Bu öğretici için varsayılan alt ağ kullanarak oluşturma [yeni AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig) cmdlet'i. Alt ağ adı ve adres ön eki için daha önce başlatılmış değişkenleri belirtin.

> [!NOTE]
> Bu cmdlet kullanarak sanal ağ alt ağ yapılandırmasını ek özelliklerini tanımlayabilirsiniz, ancak, bu öğreticinin kapsamı dışındadır.

Sanal alt ağ yapılandırmanızı oluşturmak için bu cmdlet'i çalıştırın.

```powershell
$SubnetConfig = New-AzVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
```

### <a name="create-a-virtual-network"></a>Sanal ağ oluştur
Ardından, yeni kaynak grubu kullanarak sanal ağınızı oluşturma [yeni AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork) cmdlet'i. Adı, konumu ve adres ön eki için daha önce başlatılmış değişkenleri belirtin. Önceki adımda tanımlanan alt ağ yapılandırmasını kullanın.

Sanal ağınızı oluşturmak için bu cmdlet'i çalıştırın.

```powershell
$VNet = New-AzVirtualNetwork -Name $VNetName `
   -ResourceGroupName $ResourceGroupName -Location $Location `
   -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
```

### <a name="create-the-public-ip-address"></a>Genel IP adresi oluşturma
Sanal ağınızda tanımlanmış, sanal makine bağlantısı için bir IP adresi yapılandırmanız gerekir. Bu öğretici için dinamik IP adresleme Internet bağlantısı desteklemek için kullanarak genel bir IP adresi oluşturun. Kullanım [yeni AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress) genel IP adresini yeni kaynak grubunuzu oluşturmak için cmdlet'i. Adı, konum, ayırma yöntemini ve DNS etki alanı adı etiketi için daha önce başlatılmış değişkenleri belirtin.

> [!NOTE]
> Bu cmdlet kullanarak genel IP adresini ek özelliklerini tanımlayabilirsiniz, ancak bu ilk Bu öğreticinin kapsamı dışındadır. Statik bir adres ile özel bir adres veya adres da oluşturabilirsiniz, ancak, ayrıca bu öğreticinin kapsamı dışındadır.

Genel IP adresi oluşturmak için bu cmdlet'i çalıştırın.

```powershell
$PublicIp = New-AzPublicIpAddress -Name $InterfaceName `
   -ResourceGroupName $ResourceGroupName -Location $Location `
   -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
```

### <a name="create-the-network-security-group"></a>Ağ güvenlik grubu oluşturma
VM ve SQL Server trafiği güvenli hale getirmek için bir ağ güvenlik grubu oluşturun.

1. İlk olarak Uzak Masaüstü bağlantılarına izin verecek şekilde RDP için bir ağ güvenlik grubu kuralı oluşturun.

   ```powershell
   $NsgRuleRDP = New-AzNetworkSecurityRuleConfig -Name "RDPRule" -Protocol Tcp `
      -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 3389 -Access Allow
   ```
1. 1433 numaralı TCP bağlantı noktası üzerinde trafiğe izin veren bir ağ güvenlik grubu kuralı yapılandırın. Bunun yapılması internet üzerinden SQL Server'a bağlantılar sağlar.

   ```powershell
   $NsgRuleSQL = New-AzNetworkSecurityRuleConfig -Name "MSSQLRule"  -Protocol Tcp `
      -Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 1433 -Access Allow
   ```

1. Ağ güvenlik grubu oluşturun.

   ```powershell
   $Nsg = New-AzNetworkSecurityGroup -ResourceGroupName $ResourceGroupName `
      -Location $Location -Name $NsgName `
      -SecurityRules $NsgRuleRDP,$NsgRuleSQL
   ```

### <a name="create-the-network-interface"></a>Ağ arabirimini oluşturun
Sanal makineniz için ağ arabirimi oluşturmak artık hazırsınız. Kullanım [yeni AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/new-aznetworkinterface) cmdlet'ini yeni kaynak grubunuz, ağ arabirimini oluşturun. Adı, konum, alt ağ ve daha önce tanımlanan genel IP adresi belirtin.

Ağ Arabiriminizin oluşturmak için bu cmdlet'i çalıştırın.

```powershell
$Interface = New-AzNetworkInterface -Name $InterfaceName `
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
Sanal makine boyutunu belirleyerek başlayın. Bu öğreticide, bir DS13 belirtin. Kullanım [yeni AzVMConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azvmconfig) bir sanal makine yapılandırılabilir nesnesi oluşturmak için cmdlet'i. Ad ve boyut için daha önce başlatılmış değişkenleri belirtin.

Sanal makine nesnesini oluşturmak için bu cmdlet'i çalıştırın.

```powershell
$VirtualMachine = New-AzVMConfig -VMName $VMName -VMSize $VMSize
```

### <a name="create-a-credential-object-to-hold-the-name-and-password-for-the-local-administrator-credentials"></a>Adı ve parola yerel yönetici kimlik bilgileri tutmak için bir kimlik bilgisi nesnesini oluşturun
Sanal makine için işletim sistemi özelliklerini ayarlayabilmek için önce güvenli bir dize olarak yerel yönetici hesabının kimlik bilgilerini sağlamanız gerekir. Bunu gerçekleştirmek için kullanın [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet'i.

Aşağıdaki cmdlet'i çalıştırın ve PowerShell kimlik bilgileri isteği penceresinde, adı ve sanal makinede yerel yönetici hesabı için kullanılacak parolayı yazın.

```powershell
$Credential = Get-Credential -Message "Type the name and password of the local administrator account."
```

### <a name="set-the-operating-system-properties-for-the-virtual-machine"></a>Sanal makine için işletim sistemi özelliklerini ayarlama
İle sanal makinenin işletim sistemi özelliklerini ayarlamak hazır artık [kümesi AzVMOperatingSystem](https://docs.microsoft.com/powershell/module/az.compute/set-azvmoperatingsystem) cmdlet'i.

- İşletim sistemi türü Windows ayarlayın.
- Gerekli [sanal makine Aracısı](../../extensions/agent-windows.md) yüklenecek.
- Cmdlet, otomatik güncelleştirme etkinleştirir belirtin.
- Sanal makine adı, bilgisayar adını ve kimlik bilgisi için daha önce başlatılmış değişkenleri belirtin.

Sanal makineniz için işletim sistemi özelliklerini ayarlamak için bu cmdlet'i çalıştırın.

```powershell
$VirtualMachine = Set-AzVMOperatingSystem -VM $VirtualMachine `
   -Windows -ComputerName $ComputerName -Credential $Credential `
   -ProvisionVMAgent -EnableAutoUpdate
```

### <a name="add-the-network-interface-to-the-virtual-machine"></a>Sanal makineye bir ağ arabirimi ekleyin
Ardından, [Ekle AzVMNetworkInterface](https://docs.microsoft.com/powershell/module/az.compute/add-azvmnetworkinterface) cmdlet'ini kullanarak daha önce tanımladığınız değişkeni ağ arabirimi ekleyin.

Sanal makinenizin ağ arabirimine ayarlama için bu cmdlet'i çalıştırın.

```powershell
$VirtualMachine = Add-AzVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
```

### <a name="set-the-blob-storage-location-for-the-disk-to-be-used-by-the-virtual-machine"></a>Sanal makine tarafından kullanılacak disk için blob depolama konumunu ayarla
Ardından, daha önce tanımlanan değişkenleri kullanarak sanal makinenin diski blob depolama konumunu ayarlayın.

Blob depolama konumunu ayarlamak için bu cmdlet'i çalıştırın.

```powershell
$OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
```

### <a name="set-the-operating-system-disk-properties-for-the-virtual-machine"></a>İşletim sistemi sanal makine disk özelliklerini ayarlama
Ardından, işletim sistemi disk özellikleri kullanarak sanal makine için ayarlanmış [kümesi AzVMOSDisk](https://docs.microsoft.com/powershell/module/az.compute/set-azvmosdisk) cmdlet'i. 

- İşletim sistemi için sanal makineyi bir görüntüden gelir belirtin.
- Yalnızca (SQL Server aynı disk üzerinde yüklü olduğundan) okumak için önbelleğe alma ayarlayın.
- VM adını ve işletim sistemi diski için daha önce başlatılmış değişkenleri belirtin.

İşletim sistemi, sanal makine disk özelliklerini ayarlamak için bu cmdlet'i çalıştırın.

```powershell
$VirtualMachine = Set-AzVMOSDisk -VM $VirtualMachine -Name `
   $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage
```

### <a name="specify-the-platform-image-for-the-virtual-machine"></a>Sanal makine için platform görüntüsü belirtin
Son yapılandırma adımı, sanal makineniz için platform görüntüsü belirtmektir. Bu öğreticide, en son SQL Server 2016 CTP görüntüyü kullanın. Kullanma [kümesi AzVMSourceImage](https://docs.microsoft.com/powershell/module/az.compute/set-azvmsourceimage) bu görüntü daha önce tanımlanan değişkenleri ile kullanmak için cmdlet.

Sanal makineniz için platform görüntüsü belirtmek için bu cmdlet'i çalıştırın.

```powershell
$VirtualMachine = Set-AzVMSourceImage -VM $VirtualMachine `
   -PublisherName $PublisherName -Offer $OfferName `
   -Skus $Sku -Version $Version
```

## <a name="create-the-sql-vm"></a>SQL VM'sini oluşturma
Yapılandırma adımları bitirdikten sonra sanal makine oluşturmaya hazırsınız. Kullanım [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) tanımlanan değişkenleri kullanarak sanal makine oluşturmak için cmdlet'i.

> [!TIP]
> VM oluşturma birkaç dakika sürebilir.

Sanal makinenizi oluşturmak için bu cmdlet'i çalıştırın.

```powershell
New-AzVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine
```

Sanal makine oluşturulur.

> [!NOTE]
> Önyükleme tanılaması hakkında bir hata alırsanız, onu yok sayabilirsiniz. Belirtilen depolama hesabı için sanal makinenin disk bir premium depolama hesabı olduğu için önyükleme tanılaması için standart depolama hesabı oluşturulur.

## <a name="install-the-sql-iaas-agent"></a>SQL Iaas Aracısı'nı yükleme
SQL Server sanal makineleri ile otomatik yönetim özelliklerini destekleyen [SQL Server Iaas Aracısı uzantısı](virtual-machines-windows-sql-server-agent-extension.md). Aracıyı yeni VM'ye yüklemek için oluşturulduktan sonra aşağıdaki komutu çalıştırın.


   ```powershell
   Set-AzVMSqlServerExtension -ResourceGroupName $ResourceGroupName -VMName $VMName -name "SQLIaasExtension" -version "1.2" -Location $Location
   ```

## <a name="stop-or-remove-a-vm"></a>Durdurma veya bir VM Kaldır

Sanal Makinenin sürekli olarak çalışmasını gerekmiyorsa, kullanımda olmadığında durdurarak gereksiz ödeme yapmaktan kaçının. Aşağıdaki komut VM'yi durdurur ancak özelliği daha sonra kullanılmak üzere muhafaza eder.

```powershell
Stop-AzVM -Name $VMName -ResourceGroupName $ResourceGroupName
```

Sanal makine ile ilişkili tüm kaynakları da kalıcı olarak silebilirsiniz **Remove-AzResourceGroup** komutu. Sanal makineyi de kalıcı olarak bunu siler, bu komutu dikkatli kullanın.

## <a name="example-script"></a>Örnek betik
Aşağıdaki komut dosyası, Bu öğretici için tam PowerShell Betiği içerir. Bu, zaten Azure aboneliğiniz ile kullanılacak belirlediğinizi varsayar **Connect AzAccount** ve **seçin AzSubscription** komutları.

```powershell
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
New-AzResourceGroup -Name $ResourceGroupName -Location $Location

# Storage
$StorageAccount = New-AzStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

# Network
$SubnetConfig = New-AzVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
$VNet = New-AzVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
$PublicIp = New-AzPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
$NsgRuleRDP = New-AzNetworkSecurityRuleConfig -Name "RDPRule" -Protocol Tcp -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389 -Access Allow
$NsgRuleSQL = New-AzNetworkSecurityRuleConfig -Name "MSSQLRule"  -Protocol Tcp -Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 1433 -Access Allow
$Nsg = New-AzNetworkSecurityGroup -ResourceGroupName $ResourceGroupName -Location $Location -Name $NsgName -SecurityRules $NsgRuleRDP,$NsgRuleSQL
$Interface = New-AzNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id -NetworkSecurityGroupId $Nsg.Id

# Compute
$VirtualMachine = New-AzVMConfig -VMName $VMName -VMSize $VMSize
$Credential = Get-Credential -Message "Type the name and password of the local administrator account."
$VirtualMachine = Set-AzVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
$VirtualMachine = Add-AzVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
$OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
$VirtualMachine = Set-AzVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

# Image
$VirtualMachine = Set-AzVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

# Create the VM in Azure
New-AzVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

# Add the SQL IaaS Extension
Set-AzVMSqlServerExtension -ResourceGroupName $ResourceGroupName -VMName $VMName -name "SQLIaasExtension" -version "1.2" -Location $Location
```

## <a name="next-steps"></a>Sonraki adımlar
Sanal makine oluşturulduktan sonra şunları yapabilirsiniz:

- RDP kullanarak sanal makineye bağlanın
- VM'niz için portalda SQL Server ayarlarını yapılandırma dahil olmak üzere:
   - [Depolama ayarları](virtual-machines-windows-sql-server-storage-configuration.md) 
   - [Otomatik yönetim görevleri](virtual-machines-windows-sql-server-agent-extension.md)
- [Bağlantı yapılandırma](virtual-machines-windows-sql-connect.md)
- İstemciler ve uygulamalar için yeni SQL Server örneğine bağlanın

