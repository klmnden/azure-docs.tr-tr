---
title: Azure sanal ağ geçidi ve bağlantı - PowerShell sorunlarını giderme | Microsoft Docs
description: Bu sayfa, Azure Ağ İzleyicisi'ni kullanmayı açıklar PowerShell cmdlet'i sorunlarını giderme
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: f6f0a813-38b6-4a1f-8cfc-1dfdf979f595
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: jdial
ms.openlocfilehash: 67c34156d6a397cdeb4bb50c712b1bb651c2f257
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090437"
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-powershell"></a>Sanal ağ geçidi ve Azure Ağ İzleyicisi PowerShell kullanarak bağlantı sorunlarını giderme

> [!div class="op_single_selector"]
> - [Portal](diagnose-communication-problem-between-networks.md)
> - [PowerShell](network-watcher-troubleshoot-manage-powershell.md)
> - [Azure CLI](network-watcher-troubleshoot-manage-cli.md)
> - [REST API](network-watcher-troubleshoot-manage-rest.md)

Ağ İzleyicisi, ağ kaynaklarınıza azure'da anlamak için bağlantılı olarak çok sayıda özellik sağlar. Bu özelliklerin biri kaynak sorunlarını giderme. Kaynak sorun giderme portalı, PowerShell, CLI veya REST API çağrılabilir. Çağrıldığında, Ağ İzleyicisi, bir sanal ağ geçidi veya bağlantı durumunu inceler ve bulguları döndürür.

## <a name="before-you-begin"></a>Başlamadan önce

Bu senaryo, zaten uyguladığınız adımları varsayar [Ağ İzleyicisi oluşturma](network-watcher-create.md) Ağ İzleyicisi oluşturmak için.

Desteklenen ağ geçidi türleri ziyaret listesini [desteklenen ağ geçidi türleri](network-watcher-troubleshoot-overview.md#supported-gateway-types).

## <a name="overview"></a>Genel Bakış

Kaynak sorunlarını giderme özelliği sağlar bağlantı ile sanal ağ geçitleri ile ortaya çıkan sorunları giderme. Kaynak sorunlarını gidermek için bir istek yapıldığında, günlükleri gönderildiğini sorgulanabilir ve inceledi. İnceleme işlemi tamamlandıktan sonra sonuçlar döndürülür. Sorun giderme isteği uzun kaynak, hangi birden çok dakika sürebilir bir sonuç döndürmek için ister. Sorun giderme gelen günlükler, belirtilen depolama hesabında bir kapsayıcıda depolanır.

## <a name="retrieve-network-watcher"></a>Ağ İzleyicisi alınamıyor

Ağ İzleyicisi örneğini almak için ilk adımdır bakın. `$networkWatcher` Değişken geçirilir `Start-AzureRmNetworkWatcherResourceTroubleshooting` 4. adımda cmdlet'i.

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="retrieve-a-virtual-network-gateway-connection"></a>Bir sanal ağ ağ geçidi bağlantısı

Bu örnekte, kaynak sorun giderme olan çalıştığı bir bağlantıda. Ayrıca bir sanal ağ geçidi geçirebilirsiniz.

```powershell
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name "2to3" -ResourceGroupName "testrg"
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Kaynak durumu hakkında daha fazla veri kaynağı sorunlarını giderme döndürür, ayrıca gözden geçirilmesi için bir depolama hesabı günlükleri kaydeder. Bu adımda, bir depolama hesabı oluşturuyoruz, mevcut bir depolama hesabı zaten varsa bunu kullanabilirsiniz.

```powershell
$sa = New-AzureRmStorageAccount -Name "contosoexamplesa" -SKU "Standard_LRS" -ResourceGroupName "testrg" -Location "WestCentralUS"
Set-AzureRmCurrentStorageAccount -ResourceGroupName $sa.ResourceGroupName -Name $sa.StorageAccountName
$sc = New-AzureStorageContainer -Name logs
```

## <a name="run-network-watcher-resource-troubleshooting"></a>Kaynak Ağ İzleyicisi sorun giderme çalıştırın

Kaynakları ile ilgili sorunları giderme `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet'i. Cmdlet Ağ İzleyicisi nesne, bağlantı veya sanal ağ geçidi kimliği, depolama hesabı kimliği ve yolun Sonuçların depolanacağı geçirin.

> [!NOTE]
> `Start-AzureRmNetworkWatcherResourceTroubleshooting` Cmdlet uzun çalışıyor ve tamamlanması birkaç dakika sürebilir.

```powershell
Start-AzureRmNetworkWatcherResourceTroubleshooting -NetworkWatcher $networkWatcher -TargetResourceId $connection.Id -StorageId $sa.Id -StoragePath "$($sa.PrimaryEndpoints.Blob)$($sc.name)"
```

Cmdlet'i çalıştırdıktan sonra Ağ İzleyicisi sistem durumu doğrulamak için kaynak inceler. Shell sonuçları döndürür ve belirtilen depolama hesabında günlükleri sonuçlarını depolar.

## <a name="understanding-the-results"></a>Sonuçlarını anlama

Eylem metin sorunu hakkında genel rehberlik sağlar. Bir eylem sorununu gerçekleştirilebiliyorsa bir bağlantı ile ek yönergeler sağlanır. Bu durumda hiçbir ek yönergeler olduğunda, bir destek olayı açmak için URL'yi isteğin yanıtını verir.  Yanıt ve dahil edilen nedir özellikleri hakkında daha fazla bilgi için ziyaret [Ağ İzleyicisi sorun giderme genel bakış](network-watcher-troubleshoot-overview.md)

Azure depolama hesaplarından dosyaları indirme ile ilgili yönergeler için başvurmak [.NET kullanarak Azure Blob depolamayı kullanmaya başlama](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Kullanılabilen başka bir Depolama Gezgini aracıdır. Aşağıdaki bağlantıda Depolama Gezgini hakkında daha fazla bilgi burada bulunabilir: [Depolama Gezgini](http://storageexplorer.com/)

## <a name="next-steps"></a>Sonraki adımlar

Ayarları durdurma VPN bağlantısının değiştirilmiş olup [ağ güvenlik grupları yönet](../virtual-network/manage-network-security-group.md) söz konusu olabilecek ağ güvenlik grubu ve güvenlik kuralları izlemek için.
