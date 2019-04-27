---
title: Paket yakalama işlemlerini Azure Ağ İzleyicisi - Azure CLI ile yönetme | Microsoft Docs
description: Bu sayfa, Azure CLI kullanarak Ağ İzleyicisi paket yakalama özelliğini yönetmek açıklanmaktadır
services: network-watcher
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
ms.assetid: cb0c1d10-f7f2-4c34-b08c-f73452430be8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: kumud
ms.openlocfilehash: cf03872607546f38d19a280f65f641abf627268b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60726885"
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-the-azure-cli"></a>Azure CLI kullanarak Azure Ağ İzleyicisi ile paket yakalamayı yönetme

> [!div class="op_single_selector"]
> - [Azure portal](network-watcher-packet-capture-manage-portal.md)
> - [PowerShell](network-watcher-packet-capture-manage-powershell.md)
> - [Azure CLI](network-watcher-packet-capture-manage-cli.md)
> - [Azure REST API](network-watcher-packet-capture-manage-rest.md)

Ağ İzleyicisi paket yakalama, bir sanal makineye gelen ve giden trafiği izlemek için yakalama oturumu oluşturmanıza olanak sağlar. Sağlamak istediğiniz trafiği yakalamak yakalama oturumu için filtreler sağlanır. Paket yakalama ağ anomalileri öngörülebiliyorsa ve proaktif tanılamaya yardımcı olur. Diğer kullanımlar ağ izinsiz girişi, istemci-sunucu iletişimleri ve daha fazlasını hata ayıklamak için bilgi elde etme, ağ istatistikleri toplama içerir. Çağırabildiğinden uzaktan paket yakalamaları tetiklemek için bir paket yakalama el ile ve değerli zaman kazandırır istediğiniz makineye çalışan yükünü bu özellik kolaylaştırır.

Bu makaledeki adımları gerçekleştirmek için yapmanız [Azure komut satırı arabirimi için Mac, Linux ve Windows (Azure CLI) yükleme](/cli/azure/install-azure-cli).

Bu makalede paket yakalaması için şu anda kullanılabilir olan farklı yönetim görevleri alır.

- [**Paket Yakalamayı Başlat**](#start-a-packet-capture)
- [**Paket Yakalamayı Durdur**](#stop-a-packet-capture)
- [**bir paket yakalamasını Sil**](#delete-a-packet-capture)
- [**Paket yakalaması indirin**](#download-a-packet-capture)

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, aşağıdaki kaynaklara sahip olduğunu varsayar:

- Ağ İzleyicisi paket yakalama oluşturmak istediğiniz bölgede örneği
- Bir sanal makine etkin paket yakalama uzantısına sahip.

> [!IMPORTANT]
> Paket yakalaması aracıyı sanal makinede çalışıyor olmasını gerektirir. Aracıyı bir uzantısı olarak yüklenir. VM uzantıları hakkında yönergeler için ziyaret [sanal makine uzantıları ve özellikleri](../virtual-machines/windows/extensions-features.md).

## <a name="install-vm-extension"></a>VM uzantıları yükleme

### <a name="step-1"></a>1. Adım

Çalıştırma `az vm extension set` Konuk sanal makinede paket yakalama aracıyı yüklemek için cmdlet'i.

Windows sanal makineler için:

```azurecli
az vm extension set --resource-group resourceGroupName --vm-name virtualMachineName --publisher Microsoft.Azure.NetworkWatcher --name NetworkWatcherAgentWindows --version 1.4
```

Linux sanal makineleri için:

```azurecli
az vm extension set --resource-group resourceGroupName --vm-name virtualMachineName --publisher Microsoft.Azure.NetworkWatcher --name NetworkWatcherAgentLinux--version 1.4
```

### <a name="step-2"></a>2. Adım

Aracısının yüklü olduğundan emin olmak için çalıştırmanız `vm extension show` cmdlet'i ve kaynak grubunu ve sanal makine adı geçirin. Aracının yüklü emin olmak için sonuç listesini kontrol edin.

```azurecli
az vm extension show --resource-group resourceGroupName --vm-name virtualMachineName --name NetworkWatcherAgentWindows
```

Aşağıdaki örnek bir yanıt çalışmasını örneğidir. `az vm extension show`

```json
{
  "autoUpgradeMinorVersion": true,
  "forceUpdateTag": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}/extensions/NetworkWatcherAgentWindows",
  "instanceView": null,
  "location": "westcentralus",
  "name": "NetworkWatcherAgentWindows",
  "protectedSettings": null,
  "provisioningState": "Succeeded",
  "publisher": "Microsoft.Azure.NetworkWatcher",
  "resourceGroup": "{resourceGroupName}",
  "settings": null,
  "tags": null,
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "typeHandlerVersion": "1.4",
  "virtualMachineExtensionType": "NetworkWatcherAgentWindows"
}
```

## <a name="start-a-packet-capture"></a>Paket Yakalamayı Başlat

Yukarıdaki adımlar tamamlandıktan sonra paket yakalama Aracısı sanal makineye yüklenir.

### <a name="step-1"></a>1. Adım

Ağ İzleyicisi örneğini almak için sonraki adımdır bakın. Ağ İzleyicisi'nin adı geçirildiğinde `az network watcher show` 4. adımda cmdlet'i.

```azurecli
az network watcher show --resource-group resourceGroup --name networkWatcherName
```

### <a name="step-2"></a>2. Adım

Bir depolama hesabı alın. Bu depolama hesabı, paket yakalama dosyasını depolamak için kullanılır.

```azurecli
azure storage account list
```

### <a name="step-3"></a>3. Adım

Paket yakalaması tarafından depolanan verileri sınırlamak için filtreleri kullanılabilir. Aşağıdaki örnek bir paket yakalama birkaç filtrelerle ayarlar.  İlk üç filtreleri, 20, 80 ve 443 numaralı hedef bağlantı noktalarına giden TCP trafiğine yalnızca yerel IP 10.0.0.3 toplayın.  Son filtre yalnızca UDP trafiğini toplar.

```azurecli
az network watcher packet-capture create --resource-group {resourceGroupName} --vm {vmName} --name packetCaptureName --storage-account {storageAccountName} --filters "[{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"20\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"80\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"443\"},{\"protocol\":\"UDP\"}]"
```

Aşağıdaki örnek, beklenen çıktıyı çalışmasını `az network watcher packet-capture create` cmdlet'i.

```json
{
  "bytesToCapturePerPacket": 0,
  "etag": "W/\"b8cf3528-2e14-45cb-a7f3-5712ffb687ac\"",
  "filters": [
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "20"
    },
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "80"
    },
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "443"
    },
    {
      "localIpAddress": "",
      "localPort": "",
      "protocol": "UDP",
      "remoteIpAddress": "",
      "remotePort": ""
    }
  ],
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/pa
cketCaptures/packetCaptureName",
  "name": "packetCaptureName",
  "provisioningState": "Succeeded",
  "resourceGroup": "NetworkWatcherRG",
  "storageLocation": {
    "filePath": null,
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/gwteststorage123abc",
    "storagePath": "https://gwteststorage123abc.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/{resourceGroupName}/p
roviders/microsoft.compute/virtualmachines/{vmName}/2017/05/25/packetcapture_16_22_34_630.cap"
  },
  "target": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}",
  "timeLimitInSeconds": 18000,
  "totalBytesPerSession": 1073741824
}
```

## <a name="get-a-packet-capture"></a>Paket yakalaması Al

Çalışan `az network watcher packet-capture show-status` cmdlet'i, şu anda çalışan veya tamamlanmış bir paket yakalaması durumunu alır.

```azurecli
az network watcher packet-capture show-status --name packetCaptureName --location {networkWatcherLocation}
```

Aşağıdaki örnek çıktısı, `az network watcher packet-capture show-status` cmdlet'i. Aşağıdaki örnek, yakalama durdurulduğunda, stopreason & TimeExceeded ile aynıdır. 

```
{
  "additionalProperties": {
    "status": "Succeeded"
  },
  "captureStartTime": "2016-12-06T17:20:01.5671279Z",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/pa
cketCaptures/packetCaptureName",
  "name": "packetCaptureName",
  "packetCaptureError": [],
  "packetCaptureStatus": "Stopped",
  "stopReason": "TimeExceeded"
}
```

## <a name="stop-a-packet-capture"></a>Paket Yakalamayı Durdur

Çalıştırarak `az network watcher packet-capture stop` yakalama oturumu, sürüyorsa cmdlet'i durduruldu.

```azurecli
az network watcher packet-capture stop --name packetCaptureName --location westcentralus
```

> [!NOTE]
> Cmdlet hiç yanıt döndürür, o anda çalışan bir yakalama oturumu veya zaten durduran bir var olan oturumu çalıştı.

## <a name="delete-a-packet-capture"></a>bir paket yakalamasını Sil

```azurecli
az network watcher packet-capture delete --name packetCaptureName --location westcentralus
```

> [!NOTE]
> Paket yakalaması siliniyor depolama hesabındaki dosya silinmez.

## <a name="download-a-packet-capture"></a>Paket yakalaması indirin

Paket yakalama oturumunuz tamamladıktan sonra blob depolama veya yerel bir dosyaya VM üzerinde yakalama dosyasını karşıya yüklenebilir. Paket yakalaması depolama konumunu oluşturma oturumu sırasında tanımlanır. Bunlar erişmek için uygun bir araç yakalama dosyalarını bir depolama hesabına kayıtlı olduğundan burada indirilebilir Microsoft Azure Depolama Gezgini:  https://storageexplorer.com/

Bir depolama hesabı belirttiyseniz, paket yakalama dosyaları şu konumda bir depolama hesabına kaydedilir:

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a>Sonraki adımlar

Sanal makine uyarılarla paket yakalamaları görüntüleyerek otomatikleştirmeyi öğrenme [uyarı tetiklendi paket yakalama oluşturma](network-watcher-alert-triggered-packet-capture.md)

Belirli trafiğe içine veya dışına VM'nizi ederek izin verilip verilmediğini Bul [denetleyin IP akışı doğrulama](diagnose-vm-network-traffic-filtering-problem.md)

<!-- Image references -->
