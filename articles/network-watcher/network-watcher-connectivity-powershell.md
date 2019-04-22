---
title: Azure Ağ İzleyicisi - PowerShell ile bağlantı sorunlarını giderme | Microsoft Docs
description: Bağlantıyı kullanmayı öğrenin özelliği PowerShell kullanarak Azure Ağ İzleyicisi, sorun giderme.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/11/2017
ms.author: jdial
ms.openlocfilehash: 7df84638d7b0601352b1f77ca9c0a61b7fd5af07
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59047254"
---
# <a name="troubleshoot-connections-with-azure-network-watcher-using-powershell"></a>PowerShell kullanarak Azure Ağ İzleyicisi ile bağlantı sorunlarını giderme

> [!div class="op_single_selector"]
> - [Portal](network-watcher-connectivity-portal.md)
> - [PowerShell](network-watcher-connectivity-powershell.md)
> - [Azure CLI](network-watcher-connectivity-cli.md)
> - [Azure REST API](network-watcher-connectivity-rest.md)

Bağlantı kullanmayı öğrenin belirli bir uç noktaya doğrudan TCP bağlantısı bir sanal makineden oluşturulan olup olmadığını doğrulamak için sorun giderme.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="before-you-begin"></a>Başlamadan önce

* Ağ İzleyicisi bağlantı sorunlarını gidermek için istediğiniz bölgede bir örneği.
* Bağlantı sorunlarını gidermek için sanal makineler.

> [!IMPORTANT]
> Bağlantı sorunlarını giderme, sorun giderme VM'ye sahip olması gerekir `AzureNetworkWatcherExtension` VM uzantısı yüklü. Bir Windows VM'de uzantıyı yüklemek için ziyaret [Windows için Azure Ağ İzleyicisi Aracısı sanal makine uzantısı](../virtual-machines/windows/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) ve Linux VM ziyaret [LinuxiçinAzureAğİzleyicisiAracısısanalmakineuzantısı](../virtual-machines/linux/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json). Hedef uç noktada bir uzantı gerekli değildir.

## <a name="check-connectivity-to-a-virtual-machine"></a>Bir sanal makine bağlantısını kontrol edin

Bu örnek, bir hedef sanal makineye bir bağlantı bağlantı noktası 80 üzerinden denetler. Bu örnek, Ağ İzleyicisi VM kaynağı içeren bölgede etkin olmasını gerektirir.  

### <a name="example"></a>Örnek

```powershell
$rgName = "ContosoRG"
$sourceVMName = "MultiTierApp0"
$destVMName = "Database0"

$RG = Get-AzResourceGroup -Name $rgName

$VM1 = Get-AzVM -ResourceGroupName $rgName | Where-Object -Property Name -EQ $sourceVMName
$VM2 = Get-AzVM -ResourceGroupName $rgName | Where-Object -Property Name -EQ $destVMName

$nw = Get-AzResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $VM1.Location} 
$networkWatcher = Get-AzNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

Test-AzNetworkWatcherConnectivity -NetworkWatcher $networkWatcher -SourceId $VM1.Id -DestinationId $VM2.Id -DestinationPort 80
```

### <a name="response"></a>Yanıt

Önceki örnekte yanıttır.  Bu yanıt `ConnectionStatus` olduğu **ulaşılamıyor**. Tüm araştırmaları başarısız gönderilen görebilirsiniz. Sanal gereç kullanıcının yapılandırdığı nedeniyle bağlantı başarısız `NetworkSecurityRule` adlı **UserRule_Port80**, 80 numaralı bağlantı noktasında gelen trafiği engellemek için yapılandırılmış. Bu bilgiler, bağlantı sorunlarını araştırmak için kullanılabilir.

```
ConnectionStatus : Unreachable
AvgLatencyInMs   : 
MinLatencyInMs   : 
MaxLatencyInMs   : 
ProbesSent       : 100
ProbesFailed     : 100
Hops             : [
                     {
                       "Type": "Source",
                       "Id": "c5222ea0-3213-4f85-a642-cee63217c2f3",
                       "Address": "10.1.1.4",
                       "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGrou
                   ps/ContosoRG/providers/Microsoft.Network/networkInterfaces/appNic0/ipConfigurat
                   ions/ipconfig1",
                       "NextHopIds": [
                         "9283a9f0-cc5e-4239-8f5e-ae0f3c19fbaa"
                       ],
                       "Issues": []
                     },
                     {
                       "Type": "VirtualAppliance",
                       "Id": "9283a9f0-cc5e-4239-8f5e-ae0f3c19fbaa",
                       "Address": "10.1.2.4",
                       "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGrou
                   ps/ContosoRG/providers/Microsoft.Network/networkInterfaces/fwNic/ipConfiguratio
                   ns/ipconfig1",
                       "NextHopIds": [
                         "0f1500cd-c512-4d43-b431-7267e4e67017"
                       ],
                       "Issues": []
                     },
                     {
                       "Type": "VirtualAppliance",
                       "Id": "0f1500cd-c512-4d43-b431-7267e4e67017",
                       "Address": "10.1.3.4",
                       "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGrou
                   ps/ContosoRG/providers/Microsoft.Network/networkInterfaces/auNic/ipConfiguratio
                   ns/ipconfig1",
                       "NextHopIds": [
                         "a88940f8-5fbe-40da-8d99-1dee89240f64"
                       ],
                       "Issues": [
                         {
                           "Origin": "Outbound",
                           "Severity": "Error",
                           "Type": "NetworkSecurityRule",
                           "Context": [
                             {
                               "key": "RuleName",
                               "value": "UserRule_Port80"
                             }
                           ]
                         }
                       ]
                     },
                     {
                       "Type": "VnetLocal",
                       "Id": "a88940f8-5fbe-40da-8d99-1dee89240f64",
                       "Address": "10.1.4.4",
                       "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGrou
                   ps/ContosoRG/providers/Microsoft.Network/networkInterfaces/dbNic0/ipConfigurati
                   ons/ipconfig1",
                       "NextHopIds": [],
                       "Issues": []
                     }
                   ]
```

## <a name="validate-routing-issues"></a>Yönlendirme sorunları doğrula

Bu örnek, bir sanal makine ve uzak uç noktası arasındaki bağlantıyı denetler. Bu örnek, Ağ İzleyicisi VM kaynağı içeren bölgede etkin olmasını gerektirir.  

### <a name="example"></a>Örnek

```powershell
$rgName = "ContosoRG"
$sourceVMName = "MultiTierApp0"

$RG = Get-AzResourceGroup -Name $rgName
$VM1 = Get-AzVM -ResourceGroupName $rgName | Where-Object -Property Name -EQ $sourceVMName

$nw = Get-AzResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $VM1.Location } 
$networkWatcher = Get-AzNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

Test-AzNetworkWatcherConnectivity -NetworkWatcher $networkWatcher -SourceId $VM1.Id -DestinationAddress 13.107.21.200 -DestinationPort 80
```

### <a name="response"></a>Yanıt

Aşağıdaki örnekte, `ConnectionStatus` olarak gösterilen **ulaşılamıyor**. İçinde `Hops` ayrıntıları görebilirsiniz altında `Issues` trafiği nedeniyle engellenen bir `UserDefinedRoute`. 

```
ConnectionStatus : Unreachable
AvgLatencyInMs   : 
MinLatencyInMs   : 
MaxLatencyInMs   : 
ProbesSent       : 100
ProbesFailed     : 100
Hops             : [
                     {
                       "Type": "Source",
                       "Id": "b4f7bceb-07a3-44ca-8bae-adec6628225f",
                       "Address": "10.1.1.4",
                       "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/appNic0/ipConfigurations/ipconfig1",
                       "NextHopIds": [
                         "3fee8adf-692f-4523-b742-f6fdf6da6584"
                       ],
                       "Issues": [
                         {
                           "Origin": "Outbound",
                           "Severity": "Error",
                           "Type": "UserDefinedRoute",
                           "Context": [
                             {
                               "key": "RouteType",
                               "value": "User"
                             }
                           ]
                         }
                       ]
                     },
                     {
                       "Type": "Destination",
                       "Id": "3fee8adf-692f-4523-b742-f6fdf6da6584",
                       "Address": "13.107.21.200",
                       "ResourceId": "Unknown",
                       "NextHopIds": [],
                       "Issues": []
                     }
                   ]
```

## <a name="check-website-latency"></a>Onay Web sitesi gecikme süresi

Aşağıdaki örnek, bir Web sitesine bağlantı denetler. Bu örnek, Ağ İzleyicisi VM kaynağı içeren bölgede etkin olmasını gerektirir.  

### <a name="example"></a>Örnek

```powershell
$rgName = "ContosoRG"
$sourceVMName = "MultiTierApp0"

$RG = Get-AzResourceGroup -Name $rgName
$VM1 = Get-AzVM -ResourceGroupName $rgName | Where-Object -Property Name -EQ $sourceVMName

$nw = Get-AzResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $VM1.Location } 
$networkWatcher = Get-AzNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName


Test-AzNetworkWatcherConnectivity -NetworkWatcher $networkWatcher -SourceId $VM1.Id -DestinationAddress https://bing.com/
```

### <a name="response"></a>Yanıt

Aşağıdaki yanıtta gördüğünüz `ConnectionStatus` olarak gösterir **erişilebilir**. Bağlantı başarılı olduğunda, gecikme süresi değerleri sağlanır.

```
ConnectionStatus : Reachable
AvgLatencyInMs   : 1
MinLatencyInMs   : 0
MaxLatencyInMs   : 7
ProbesSent       : 100
ProbesFailed     : 0
Hops             : [
                     {
                       "Type": "Source",
                       "Id": "1f0e3415-27b0-4bf7-a59d-3e19fb854e3e",
                       "Address": "10.1.1.4",
                       "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/appNic0/ipConfigurations/ipconfig1",
                       "NextHopIds": [
                         "f99f2bd1-42e8-4bbf-85b6-5d21d00c84e0"
                       ],
                       "Issues": []
                     },
                     {
                       "Type": "Internet",
                       "Id": "f99f2bd1-42e8-4bbf-85b6-5d21d00c84e0",
                       "Address": "204.79.197.200",
                       "ResourceId": "Internet",
                       "NextHopIds": [],
                       "Issues": []
                     }
                   ]
```

## <a name="check-connectivity-to-a-storage-endpoint"></a>Depolama uç noktası bağlantısını denetleyin

Aşağıdaki örnek, bir sanal makineden bir blog depolama hesabı bağlantı denetler. Bu örnek, Ağ İzleyicisi VM kaynağı içeren bölgede etkin olmasını gerektirir.  

### <a name="example"></a>Örnek

```powershell
$rgName = "ContosoRG"
$sourceVMName = "MultiTierApp0"

$RG = Get-AzResourceGroup -Name $rgName

$VM1 = Get-AzVM -ResourceGroupName $rgName | Where-Object -Property Name -EQ $sourceVMName

$nw = Get-AzResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $VM1.Location }
$networkWatcher = Get-AzNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

Test-AzNetworkWatcherConnectivity -NetworkWatcher $networkWatcher -SourceId $VM1.Id -DestinationAddress https://contosostorageexample.blob.core.windows.net/ 
```

### <a name="response"></a>Yanıt

Aşağıdaki json, önceki cmdlet çalışmasını örnek yanıttır. Hedef erişilebilir, olduğu gibi `ConnectionStatus` özellik gösterilmiştir olarak **erişilebilir**.  Atlama gecikme süresi ve depolama blobu erişmek için gerekli sayısı ile ilgili ayrıntılar verilmiştir.

```json
ConnectionStatus : Reachable
AvgLatencyInMs   : 1
MinLatencyInMs   : 0
MaxLatencyInMs   : 8
ProbesSent       : 100
ProbesFailed     : 0
Hops             : [
                     {
                       "Type": "Source",
                       "Id": "9e7f61d9-fb45-41db-83e2-c815a919b8ed",
                       "Address": "10.1.1.4",
                       "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/appNic0/ipConfigurations/ipconfig1",
                       "NextHopIds": [
                         "1e6d4b3c-7964-4afd-b959-aaa746ee0f15"
                       ],
                       "Issues": []
                     },
                     {
                       "Type": "Internet",
                       "Id": "1e6d4b3c-7964-4afd-b959-aaa746ee0f15",
                       "Address": "13.71.200.248",
                       "ResourceId": "Internet",
                       "NextHopIds": [],
                       "Issues": []
                     }
                   ]
```

## <a name="next-steps"></a>Sonraki adımlar

Belirli bir trafik içine veya dışına VM'nizi ederek izin verilip verilmediğini belirlemek [denetleyin IP akışı doğrulama](diagnose-vm-network-traffic-filtering-problem.md).

Trafik engelleniyor ve olmamalıdır bkz [ağ güvenlik grupları yönet](../virtual-network/manage-network-security-group.md) tanımlanmış ağ güvenlik grubu ve güvenlik kuralları izlemek için.