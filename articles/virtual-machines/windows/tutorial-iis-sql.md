---
title: Öğretici - Azure’da bir SQL&#47;IIS&#47;.NET yığını çalıştıran sanal makineler oluşturma | Microsoft Docs
description: Bu öğreticide, Azure’daki bir Windows sanal makinesinde Azure SQL, IIS, .NET yığını yüklemeyi öğrenirsiniz.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/27/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: cb423fb43e2e7620aef989c0ead99f9bdb5a58e3
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49389400"
---
# <a name="tutorial-install-the-sql47iis47net-stack-in-a-windows-vm-with-azure-powershell"></a>Öğretici: Azure PowerShell ile bir Windows sanal makinesinde SQL&#47;IIS&#47;.NET yığını yükleme

Bu öğreticide, Azure PowerShell kullanarak SQL&#47;IIS&#47;.NET yığını yüklüyoruz. Bu yığın, Windows Server 2016’da çalışan, biri IIS ve .NET, diğeri SQL Server’a sahip iki sanal makineden oluşur.

> [!div class="checklist"]
> * VM oluşturma 
> * Sanal makineye IIS ve .NET Core SDK’sı yükleme
> * SQL Server çalıştıran bir sanal makine oluşturma
> * SQL Server uzantısı yükleme

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici, AzureRM.Compute modülü 5.7.0 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM.Compute` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps).

## <a name="create-a-iis-vm"></a>IIS sanal makinesi oluşturma 

Bu örnekte, Windows Server 2016 sanal makinesini hızlıca oluşturup IIS ile .NET Framework yüklemek amacıyla PowerShell Cloud Shell’de [New-AzureRMVM](/powershell/module/azurerm.compute/new-azurermvm) cmdlet’ini kullanıyoruz. IIS ve SQL sanal makineleri bir kaynak grubu ile sanal ağ paylaşır, dolayısıyla bu adlara yönelik değişkenler oluştururuz.


```azurepowershell-interactive
$vmName = "IISVM"
$vNetName = "myIISSQLvNet"
$resourceGroup = "myIISSQLGroup"
New-AzureRmVm `
    -ResourceGroupName $resourceGroup `
    -Name $vmName `
    -Location "East US" `
    -VirtualNetworkName $vNetName `
    -SubnetName "myIISSubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -AddressPrefix 192.168.0.0/16 `
    -PublicIpAddressName "myIISPublicIpAddress" `
    -OpenPorts 80,3389 
```

Özel betik uzantısı kullanarak IIS ve .NET Framework yükleyin.

```azurepowershell-interactive
Set-AzureRmVMExtension `
    -ResourceGroupName $resourceGroup `
    -ExtensionName IIS `
    -VMName $vmName `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server,Web-Asp-Net45,NET-Framework-Features"}' `
    -Location EastUS
```

## <a name="create-another-subnet"></a>Başka bir alt ağ oluşturma

SQL sanal makinesi için ikincil alt ağ oluşturun. [Get-AzureRmVirtualNetwork]{/powershell/module/azurerm.network/get-azurermvirtualnetwork} kullanarak vNet’i alın.

```azurepowershell-interactive
$vNet = Get-AzureRmVirtualNetwork `
   -Name $vNetName `
   -ResourceGroupName $resourceGroup
```

[Add-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig) kullanarak alt ağ için bir yapılandırma oluşturun.


```azurepowershell-interactive
Add-AzureRmVirtualNetworkSubnetConfig `
   -AddressPrefix 192.168.0.0/24 `
   -Name mySQLSubnet `
   -VirtualNetwork $vNet `
   -ServiceEndpoint Microsoft.Sql
```

[Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork) komutunu kullanarak vNet’i yeni alt ağ bilgileriyle güncelleştirin
   
```azurepowershell-interactive   
$vNet | Set-AzureRmVirtualNetwork
```

## <a name="azure-sql-vm"></a>Azure SQL VM

SQL VM oluşturmak için bir SQL Server'ın önceden yapılandırılmış Azure Market görüntüsünü kullanın. İlk olarak VM’i oluştururuz, ardından bu VM’e SQL Server Uzantısı’nı yükleriz. 


```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName $resourceGroup `
    -Name "mySQLVM" `
    -ImageName "MicrosoftSQLServer:SQL2016SP1-WS2016:Enterprise:latest" `
    -Location eastus `
    -VirtualNetworkName $vNetName `
    -SubnetName "mySQLSubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "mySQLPublicIpAddress" `
    -OpenPorts 3389,1401 
```

[SQL Server uzantısını](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension) SQL VM’e eklemek için [Set-AzureRmVMSqlServerExtension](/powershell/module/azurerm.compute/set-azurermvmsqlserverextension) komutunu kullanın.

```azurepowershell-interactive
Set-AzureRmVMSqlServerExtension `
   -ResourceGroupName $resourceGroup  `
   -VMName mySQLVM `
   -Name "SQLExtension" `
   -Location "EastUS"
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure PowerShell kullanarak SQL&#92;IIS&#92;.NET yığını yüklediniz. Şunları öğrendiniz:

> [!div class="checklist"]
> * VM oluşturma 
> * Sanal makineye IIS ve .NET Core SDK’sı yükleme
> * SQL Server çalıştıran bir sanal makine oluşturma
> * SQL Server uzantısı yükleme

SSL sertifikalarını kullanarak IIS web sunucusunun güvenliğini nasıl sağlayabileceğinizi öğrenmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [SSL sertifikalarını kullanarak IIS web sunucusunun güvenliğini sağlama](tutorial-secure-web-server.md)

