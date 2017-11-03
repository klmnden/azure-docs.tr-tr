---
title: "Azure sanal ağ geçidi ve bağlantıları - Azure CLI 1.0 sorunlarını giderme | Microsoft Docs"
description: "Bu sayfa, Azure Ağ İzleyicisi'ni kullanma açıklanmaktadır Azure CLI 1.0 sorun giderme"
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 2838bc61-b182-4da8-8533-27db8fdbd177
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: jdial
ms.openlocfilehash: 2acbc47970acf0eb2aa1aea8535d7157bc73cbb6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-azure-cli-10"></a>Sanal ağ geçidi ve Azure Ağ İzleyicisi Azure CLI 1.0 kullanarak bağlantı sorunlarını giderme

> [!div class="op_single_selector"]
> - [Portal](network-watcher-troubleshoot-manage-portal.md)
> - [PowerShell](network-watcher-troubleshoot-manage-powershell.md)
> - [CLI 1.0](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-troubleshoot-manage-cli.md)
> - [REST API](network-watcher-troubleshoot-manage-rest.md)

Ağ kaynaklarınızı Azure anlamak için ilgili olarak Ağ İzleyicisi çok sayıda özellik sağlar. Bu özelliklerin biri kaynak sorun giderme. Kaynak sorun giderme portalı, PowerShell, CLI veya REST API çağrılabilir. Çağrıldığında, Ağ İzleyicisi bir sanal ağ geçidi veya bağlantı durumunu inceler ve bulduklarını döndürür.

Bu makalede, platformlar arası Azure CLI 1.0, Windows, Mac ve Linux için kullanılabilir olduğu kullanır. 

## <a name="before-you-begin"></a>Başlamadan önce

Bu senaryo zaten izlediğiniz adımlarda varsayar [bir Ağ İzleyicisi oluşturma](network-watcher-create.md) bir Ağ İzleyicisi oluşturmak için.

Desteklenen ağ geçidi türleri ziyaret listesi [desteklenen ağ geçidi türleri](/network-watcher-troubleshoot-overview.md#supported-gateway-types).

## <a name="overview"></a>Genel Bakış

Kaynak sorunlarını giderme özelliği sağlar sanal ağ geçitleri ve bağlantılarla ortaya çıkan sorunları giderme. Kaynak sorun giderme için bir istek yapıldığında, günlükleri yükleniyor sorgulanan ve sahip denetlenir. İnceleme tamamlandığında, sonuçları döndürülür. Sorun giderme istekleri uzun süredir çalışıp kaynak, hangi birden çok dakika sürebilir bir sonuç ister. Sorun giderme gelen günlükleri, belirtilen bir depolama hesabı üzerinde bir kapsayıcıda depolanır.

## <a name="retrieve-a-virtual-network-gateway-connection"></a>Sanal ağ geçidi bağlantısı alma

Bu örnekte, kaynak sorun giderme olduğu olan tükendi bağlantı üzerinde. Ayrıca bir sanal ağ geçidi geçirebilirsiniz. Aşağıdaki cmdlet'i bir kaynak grubunda vpn bağlantılarını listeler.

```azurecli
azure network vpn-connection list -g resourceGroupName
```

Ayrıca, bir abonelik bağlantıları görmek için komutu çalıştırabilirsiniz.

```azurecli
azure network vpn-connection list -s subscription
```

Bağlantının adını sahip olduğunda, kendi kaynak kimliği almak için bu komutu çalıştırabilirsiniz:

```azurecli
azure network vpn-connection show -g resourceGroupName -n connectionName
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Kaynak durumu hakkında veriler kaynak sorun giderme döndürür, ayrıca gözden geçirilmesi için bir depolama hesabı günlükleri kaydeder. Bu adımda, biz depolama hesabı oluşturma, depolama hesabınız varsa bunu kullanabilirsiniz.

1. Depolama hesabı oluşturma

    ```azurecli
    azure storage account create -n storageAccountName -l location -g resourceGroupName
    ```

1. Depolama hesabı anahtarlarını alma

    ```azurecli
    azure storage account keys list storageAccountName -g resourcegroupName
    ```

1. Kapsayıcı oluşturma

    ```azurecli
    azure storage container create --account-name storageAccountName -g resourcegroupName --account-key {storageAccountKey} --container logs
    ```

## <a name="run-network-watcher-resource-troubleshooting"></a>Ağ İzleyicisi kaynak sorun giderme çalıştırın

Kaynaklarla ilgili sorunları giderme `network watcher troubleshoot` cmdlet'i. Kaynak grubu, depolama hesabı kimliği bağlantının kimliği adlı Ağ İzleyicisi adını biz cmdlet geçirebilir ve sorun giderme depolamak için blob yolu sonuçlanır.

```azurecli
azure network watcher troubleshoot -g resourceGroupName -n networkWatcherName -t connectionId -i storageId -p storagePath
```

Cmdlet'ini çalıştırdıktan sonra Ağ İzleyicisi sistem durumunu doğrulamak için kaynak inceler. Kabuk sonuçları döndürür ve belirtilen depolama hesabında günlüklerini sonuçlarını kaydeder.

## <a name="understanding-the-results"></a>Sonuçları anlama

Eylem metin sorunun nasıl çözümleneceği hakkında genel kılavuzluk sağlar. Bir eylem için sorunu gerçekleştirilebiliyorsa bir bağlantı ile ek yönergeler sağlanır. Durumda hiçbir ek yönergeler bulunduğu bir destek servis talebi açmaya url yanıt sağlar.  Yanıt ve dahil edilen nedir özellikleri hakkında daha fazla bilgi için ziyaret [Ağ İzleyicisi sorun giderme genel bakış](network-watcher-troubleshoot-overview.md)

Azure depolama hesaplarından dosyaları indirme ile ilgili yönergeler için bkz [.NET kullanarak Azure Blob storage'ı kullanmaya başlama](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Kullanılabilir başka bir Depolama Gezgini aracıdır. Aşağıdaki bağlantıda Depolama Gezgini hakkında daha fazla bilgi şurada bulunabilir: [Depolama Gezgini](http://storageexplorer.com/)

## <a name="next-steps"></a>Sonraki adımlar

Bu Dur VPN bağlantısı ayarları değiştirilmiş olup [ağ güvenlik grupları yönet](../virtual-network/virtual-network-manage-nsg-arm-portal.md) söz konusu olabilecek ağ güvenlik grubu ve güvenlik kuralları izlemek için.
