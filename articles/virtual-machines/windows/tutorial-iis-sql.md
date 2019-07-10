---
title: Öğretici - oluşturma Vm'leri bir SQL çalıştıran IIS, .NET ile Azure stack | Microsoft Docs
description: Bu öğreticide, Azure’daki bir Windows sanal makinesinde Azure SQL, IIS, .NET yığını yüklemeyi öğrenirsiniz.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/05/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 88a7a1ea736a418f4b08a22b3fa7b45ab0e126ff
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67708031"
---
# <a name="tutorial-install-the-sql-iis-net-stack-in-a-windows-vm-with-azure-powershell"></a>Öğretici: Azure PowerShell ile bir Windows VM'de SQL, IIS, .NET yığını yükleme

Bu öğreticide, SQL, IIS, .NET yüklemesini Azure PowerShell kullanarak yığın. Bu yığın, Windows Server 2016’da çalışan, biri IIS ve .NET, diğeri SQL Server’a sahip iki sanal makineden oluşur.

> [!div class="checklist"]
> * VM oluşturma 
> * Sanal makineye IIS ve .NET Core SDK’sı yükleme
> * SQL Server çalıştıran bir sanal makine oluşturma
> * SQL Server uzantısı yükleme

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. 

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. İsterseniz [https://shell.azure.com/powershell](https://shell.azure.com/powershell) adresine giderek Cloud Shell'i ayrı bir tarayıcı sekmesinde de başlatabilirsiniz. **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.

## <a name="create-an-iis-vm"></a>Bir IIS sanal makinesi oluşturma 

Bu örnekte [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) hızlıca bir Windows Server 2016 VM oluşturma ve IIS ve .NET Framework'ü yüklemeyi PowerShell Cloud shell'de cmdlet'i. IIS ve SQL sanal makineleri bir kaynak grubu ile sanal ağ paylaşır, dolayısıyla bu adlara yönelik değişkenler oluştururuz.


```azurepowershell-interactive
$vmName = "IISVM"
$vNetName = "myIISSQLvNet"
$resourceGroup = "myIISSQLGroup"
New-AzVm `
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

IIS ve .NET framework ile özel betik uzantısı kullanarak yükleme [kümesi AzVMExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmextension) cmdlet'i.

```azurepowershell-interactive
Set-AzVMExtension `
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

SQL sanal makinesi için ikincil alt ağ oluşturun. [Get-AzVirtualNetwork]{/powershell/module/az.network/get-azvirtualnetwork}. kullanarak Vnet'i alın

```azurepowershell-interactive
$vNet = Get-AzVirtualNetwork `
   -Name $vNetName `
   -ResourceGroupName $resourceGroup
```

Alt ağı kullanmak için bir yapılandırma oluşturmak [Ekle AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/add-azvirtualnetworksubnetconfig).


```azurepowershell-interactive
Add-AzVirtualNetworkSubnetConfig `
   -AddressPrefix 192.168.0.0/24 `
   -Name mySQLSubnet `
   -VirtualNetwork $vNet `
   -ServiceEndpoint Microsoft.Sql
```

Vnet'i yeni alt ağ bilgileriyle ile güncelleştirme [AzVirtualNetwork Ayarla](https://docs.microsoft.com/powershell/module/az.network/set-azvirtualnetwork)
   
```azurepowershell-interactive   
$vNet | Set-AzVirtualNetwork
```

## <a name="azure-sql-vm"></a>Azure SQL VM

SQL VM oluşturmak için bir SQL Server'ın önceden yapılandırılmış Azure Market görüntüsünü kullanın. İlk olarak VM’i oluştururuz, ardından bu VM’e SQL Server Uzantısı’nı yükleriz. 


```azurepowershell-interactive
New-AzVm `
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

Kullanım [kümesi AzVMSqlServerExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmsqlserverextension) eklemek için [SQL Server uzantısı](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension) SQL VM.

```azurepowershell-interactive
Set-AzVMSqlServerExtension `
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

