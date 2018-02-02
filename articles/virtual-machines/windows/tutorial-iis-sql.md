---
title: "Bir SQL &#92; IIS &#92;çalışan sanal makineler oluşturun;. Azure ağ yığını | Microsoft Docs"
description: "Öğretici - Azure SQL, IIS, .NET bir Windows sanal makinelerde yığın yükleyin."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/24/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 6533ab205e07243e2f757ea0a66028e1d140c52b
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="install-a-sql92iis92net-stack-in-azure"></a>Bir SQL &#92; IIS &#92; yükle. Azure ağ yığını

Bu öğreticide, bir SQL &#92; IIS &#92;yüklememiz;. Azure PowerShell kullanarak ağ yığını. Windows Server 2016 çalışan iki VM IIS ve .NET ile diğeri SQL Server ile bu yığını oluşur.

> [!div class="checklist"]
> * Yeni AzVM kullanarak bir VM oluşturma
> * IIS ve .NET Core SDK'nı VM'e yükleyin
> * SQL Server çalıştıran bir VM oluşturma
> * SQL Server uzantısını yükleyin

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici, Azure PowerShell modülü 5.1.1 veya sonraki bir sürümü gerektirir. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-iis-vm"></a>Bir IIS VM oluşturma 

Bu örnekte, kullandığımız [yeni AzVM](https://www.powershellgallery.com/packages/AzureRM.Compute.Experiments) hızlı bir şekilde bir Windows Server 2016 VM oluşturmak ve ardından IIS ve .NET Framework'ü yüklemek için PowerShell bulut Kabuğu cmdlet'ini. Biz bu adları için değişkenleri oluşturmak için IIS ve SQL VM'ler bir kaynak grubu ve sanal ağ paylaşır.

Tıklayın **deneyin** bulut Kabuğu'nda bu pencereyi başlatmak için şu kod bloğu üst düğme. Komut isteminde sanal makine için kimlik bilgilerini sağlamanız istenir.

```azurepowershell-interactive
$vmName = "IISVM$(Get-Random)"
$vNetName = "myIISSQLvNet"
$resourceGroup = "myIISSQLGroup"
New-AzureRMVm -Name $vmName -ResourceGroupName $resourceGroup -VirtualNetworkName $vNetName 
```

IIS ve özel betik uzantısı kullanılarak .NET Framework'ü yükleyin.

```azurepowershell-interactive

Set-AzureRmVMExtension -ResourceGroupName $resourceGroup `
    -ExtensionName IIS `
    -VMName $vmName `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server,Web-Asp-Net45,NET-Framework-Features"}' `
    -Location EastUS
```

## <a name="azure-sql-vm"></a>Azure SQL VM

SQL VM oluşturmak için bir SQL server'ın önceden yapılandırılmış Azure Market görüntüsü kullanıyoruz. İlk VM oluşturuyoruz, ardından biz SQL Server uzantısı'nı VM'e yükleyin. 


```azurepowershell-interactive
# Create user object. You get a pop-up prompting you to enter the credentials for the VM.
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a subnet configuration
$vNet = Get-AzureRmVirtualNetwork -Name $vNetName -ResourceGroupName $resourceGroup
Add-AzureRmVirtualNetworkSubnetConfig -Name mySQLSubnet -VirtualNetwork $vNet -AddressPrefix "192.168.2.0/24"
Set-AzureRmVirtualNetwork -VirtualNetwork $vNet


# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName $resourceGroup -Location eastus `
  -Name "mypublicdns$(Get-Random)" -AllocationMethod Static -IdleTimeoutInMinutes 4

# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP  -Protocol Tcp `
  -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 3389 -Access Allow


# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location eastus `
  -Name myNetworkSecurityGroup -SecurityRules $nsgRuleRDP

# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name mySQLNic -ResourceGroupName $resourceGroup -Location eastus `
  -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName mySQLVM -VMSize Standard_D1 | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName mySQLVM -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName MicrosoftSQLServer -Offer SQL2014SP2-WS2012R2 -Skus Enterprise -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

# Create the VM
New-AzureRmVM -ResourceGroupName $resourceGroup -Location eastus -VM $vmConfig
```

Kullanım [kümesi AzureRmVMSqlServerExtension](/powershell/module/azurerm.compute/set-azurermvmsqlserverextension) eklemek için [SQL Server uzantısı](/sql/virtual-machines-windows-sql-server-agent-extension.md) SQL VM.

```azurepowershell-interactive
Set-AzureRmVMSqlServerExtension -ResourceGroupName $resourceGroup -VMName mySQLVM -name "SQLExtension"
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir SQL &#92; IIS &#92; yüklü. Azure PowerShell kullanarak ağ yığını. Şunları öğrendiniz:

> [!div class="checklist"]
> * Yeni AzVM kullanarak bir VM oluşturma
> * IIS ve .NET Core SDK'nı VM'e yükleyin
> * SQL Server çalıştıran bir VM oluşturma
> * SQL Server uzantısını yükleyin

IIS web sunucusunun SSL sertifikalarıyla güvenliğini öğrenmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [SSL sertifikaları ile güvenli IIS web sunucusu](tutorial-secure-web-server.md)

