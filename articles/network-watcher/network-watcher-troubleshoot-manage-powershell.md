---
title: "Azure sanal ağ geçidi ve bağlantıları - PowerShell sorunlarını giderme | Microsoft Docs"
description: "Bu sayfa, Azure Ağ İzleyicisi'ni kullanma açıklanmaktadır PowerShell cmdlet'ini sorun giderme"
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: f6f0a813-38b6-4a1f-8cfc-1dfdf979f595
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: jdial
ms.openlocfilehash: d7ae5599b3fa1876e2b5af79f56548cd17c1c8ed
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-powershell"></a>Sanal ağ geçidi ve Azure Ağ İzleyicisi PowerShell kullanarak bağlantı sorunlarını giderme

> [!div class="op_single_selector"]
> - [Portal](network-watcher-troubleshoot-manage-portal.md)
> - [PowerShell](network-watcher-troubleshoot-manage-powershell.md)
> - [CLI 1.0](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-troubleshoot-manage-cli.md)
> - [REST API](network-watcher-troubleshoot-manage-rest.md)

Ağ kaynaklarınızı Azure anlamak için ilgili olarak Ağ İzleyicisi çok sayıda özellik sağlar. Bu özelliklerin biri kaynak sorun giderme. Kaynak sorun giderme portalı, PowerShell, CLI veya REST API çağrılabilir. Çağrıldığında, Ağ İzleyicisi bir sanal ağ geçidi veya bağlantı durumunu inceler ve bulduklarını döndürür.

## <a name="before-you-begin"></a>Başlamadan önce

Bu senaryo zaten izlediğiniz adımlarda varsayar [bir Ağ İzleyicisi oluşturma](network-watcher-create.md) bir Ağ İzleyicisi oluşturmak için.

Desteklenen ağ geçidi türleri ziyaret listesi [desteklenen ağ geçidi türleri](network-watcher-troubleshoot-overview.md#supported-gateway-types).

## <a name="overview"></a>Genel Bakış

Kaynak sorunlarını giderme özelliği sağlar sanal ağ geçitleri ve bağlantılarla ortaya çıkan sorunları giderme. Kaynak sorun giderme için bir istek yapıldığında, günlükleri yükleniyor sorgulanan ve sahip denetlenir. İnceleme tamamlandığında, sonuçları döndürülür. Sorun giderme istekleri uzun süredir çalışıp kaynak, hangi birden çok dakika sürebilir bir sonuç ister. Sorun giderme gelen günlükleri, belirtilen bir depolama hesabı üzerinde bir kapsayıcıda depolanır.

## <a name="retrieve-network-watcher"></a>Ağ İzleyicisi alma

Ağ İzleyicisi örneği almak için ilk adımdır bakın. `$networkWatcher` Değişkeni iletilir `Start-AzureRmNetworkWatcherResourceTroubleshooting` 4. adımda cmdlet'i.

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="retrieve-a-virtual-network-gateway-connection"></a>Sanal ağ geçidi bağlantısı alma

Bu örnekte, kaynak sorun giderme olduğu olan tükendi bağlantı üzerinde. Ayrıca bir sanal ağ geçidi geçirebilirsiniz.

```powershell
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name "2to3" -ResourceGroupName "testrg"
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Kaynak durumu hakkında veriler kaynak sorun giderme döndürür, ayrıca gözden geçirilmesi için bir depolama hesabı günlükleri kaydeder. Bu adımda, biz depolama hesabı oluşturma, depolama hesabınız varsa bunu kullanabilirsiniz.

```powershell
$sa = New-AzureRmStorageAccount -Name "contosoexamplesa" -SKU "Standard_LRS" -ResourceGroupName "testrg" -Location "WestCentralUS"
Set-AzureRmCurrentStorageAccount -ResourceGroupName $sa.ResourceGroupName -Name $sa.StorageAccountName
$sc = New-AzureStorageContainer -Name logs
```

## <a name="run-network-watcher-resource-troubleshooting"></a>Ağ İzleyicisi kaynak sorun giderme çalıştırın

Kaynaklarla ilgili sorunları giderme `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet'i. Cmdlet Ağ İzleyicisi nesnesinin, bağlantı veya sanal ağ geçidi kimliği, depolama hesabı kimliği ve yol Sonuçların depolanacağı geçirin.

> [!NOTE]
> `Start-AzureRmNetworkWatcherResourceTroubleshooting` Cmdlet uzun çalışıyor ve tamamlanması birkaç dakika sürebilir.

```powershell
Start-AzureRmNetworkWatcherResourceTroubleshooting -NetworkWatcher $networkWatcher -TargetResourceId $connection.Id -StorageId $sa.Id -StoragePath "$($sa.PrimaryEndpoints.Blob)$($sc.name)"
```

Cmdlet'ini çalıştırdıktan sonra Ağ İzleyicisi sistem durumunu doğrulamak için kaynak inceler. Kabuk sonuçları döndürür ve belirtilen depolama hesabında günlüklerini sonuçlarını kaydeder.

## <a name="understanding-the-results"></a>Sonuçları anlama

Eylem metin sorunun nasıl çözümleneceği hakkında genel kılavuzluk sağlar. Bir eylem için sorunu gerçekleştirilebiliyorsa bir bağlantı ile ek yönergeler sağlanır. Durumda hiçbir ek yönergeler bulunduğu bir destek servis talebi açmaya url yanıt sağlar.  Yanıt ve dahil edilen nedir özellikleri hakkında daha fazla bilgi için ziyaret [Ağ İzleyicisi sorun giderme genel bakış](network-watcher-troubleshoot-overview.md)

Azure depolama hesaplarından dosyaları indirme ile ilgili yönergeler için bkz [.NET kullanarak Azure Blob storage'ı kullanmaya başlama](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Kullanılabilir başka bir Depolama Gezgini aracıdır. Aşağıdaki bağlantıda Depolama Gezgini hakkında daha fazla bilgi şurada bulunabilir: [Depolama Gezgini](http://storageexplorer.com/)

## <a name="next-steps"></a>Sonraki adımlar

Bu Dur VPN bağlantısı ayarları değiştirilmiş olup [ağ güvenlik grupları yönet](../virtual-network/virtual-network-manage-nsg-arm-portal.md) söz konusu olabilecek ağ güvenlik grubu ve güvenlik kuralları izlemek için.
