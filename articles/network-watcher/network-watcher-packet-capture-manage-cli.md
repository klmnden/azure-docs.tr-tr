---
title: "Paket yakalama Azure Ağ İzleyicisi - Azure CLI 2.0 ile yönetme | Microsoft Docs"
description: "Bu sayfa, Azure CLI 2.0 kullanan Ağ İzleyicisi'nin paket yakalama özelliği yönetmek açıklanmaktadır"
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: cb0c1d10-f7f2-4c34-b08c-f73452430be8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 2c0cb9b72d23f46e60c96efe96a9ad32ba6fc746
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-azure-cli-20"></a>Paket yakalama Azure CLI 2.0 kullanan Azure Ağ İzleyicisi ile yönetme

> [!div class="op_single_selector"]
> - [Azure portal](network-watcher-packet-capture-manage-portal.md)
> - [PowerShell](network-watcher-packet-capture-manage-powershell.md)
> - [CLI 1.0](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-packet-capture-manage-cli.md)
> - [Azure REST API'si](network-watcher-packet-capture-manage-rest.md)

Ağ İzleyicisi paket yakalama, bir sanal makine gelen ve giden trafiği izlemek için yakalama oturumları oluşturmanıza olanak sağlar. Filtreler yalnızca trafiği yakalama emin olmak yakalama oturumu için sağlanır. Paket yakalama Tepkisel hem de önceden ağ anormallikleri tanılamanıza yardımcı olur. Diğer kullanımlar ağ yetkisiz erişim, istemci-sunucu iletişimleri ve çok daha fazlasını hata ayıklamak için bilgi sağlamasını ağ istatistikleri toplama içerir. Erişebildiklerinden uzaktan paket yakalamaları tetiklemek için bir paket yakalama el ile ve değerli zaman kazandırır istenen makine üzerinde çalışan iş yükünü Bu yetenek kolaylaştırır.

Bu makalede, Windows, Mac ve Linux için kullanılabilir olduğu, kaynak yönetimi için dağıtım modelini, Azure CLI 2.0 bizim nesil CLI kullanılmaktadır.

Bu makaledeki adımları gerçekleştirmek için gerek [Mac, Linux ve Windows (Azure CLI) için Azure komut satırı arabirimini yükleyin](https://docs.microsoft.com/cli/azure/install-az-cli2).

Bu makalede paket yakalama için şu anda kullanılabilir farklı yönetim görevleri yoluyla alır.

- [**Paket yakalama Başlat**](#start-a-packet-capture)
- [**Paket yakalama işlemini durdurun**](#stop-a-packet-capture)
- [**Paket yakalama Sil**](#delete-a-packet-capture)
- [**Paket yakalama indirin**](#download-a-packet-capture)

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, aşağıdaki kaynaklara sahip olduğunuz varsayılmaktadır:

- Paket yakalama oluşturmak istediğiniz bölgede Ağ İzleyicisi örneği
- Bir sanal makine etkin paket yakalama uzantısına sahip.

> [!IMPORTANT]
> Paket yakalama sanal makine üzerinde çalışan bir aracının gerektirir. Aracı bir uzantısı olarak yüklenir. VM uzantıları hakkında yönergeler için ziyaret [sanal makine uzantıları ve özellikleri](../virtual-machines/windows/extensions-features.md).

## <a name="install-vm-extension"></a>VM uzantısı yükleyin

### <a name="step-1"></a>1. Adım

Çalıştırma `az vm extension set` cmdlet'ini paket yakalama Aracısı Konuk sanal makineye yükleyin.

Windows sanal makineler için:

```azurecli
az vm extension set --resource-group resourceGroupName --vm-name virtualMachineName --publisher Microsoft.Azure.NetworkWatcher --name NetworkWatcherAgentWindows --version 1.4
```

Linux sanal makineleri için:

```azurecli
az vm extension set --resource-group resourceGroupName --vm-name virtualMachineName --publisher Microsoft.Azure.NetworkWatcher --name NetworkWatcherAgentLinux--version 1.4
````

### <a name="step-2"></a>2. Adım

Aracının yüklü olduğundan emin olun, çalıştırmayı `vm extension show` cmdlet'i ve kaynak grubu ve sanal makine adı geçirin. Sonuçta elde edilen listenin aracısının yüklü olduğundan emin olmak için kontrol edin.

```azurecli
az vm extension show --resource-group resourceGroupName --vm-name virtualMachineName --name NetworkWatcherAgentWindows
```

Aşağıdaki örnek yanıt çalışmasını örneğidir`az vm extension show`

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

## <a name="start-a-packet-capture"></a>Paket yakalama Başlat

Yukarıdaki adımları tamamlandıktan sonra paket yakalama Aracısı sanal makineye yüklenir.

### <a name="step-1"></a>1. Adım

Ağ İzleyicisi örneği almak için sonraki adımdır bakın. Ağ İzleyicisi'ni kurulacağı adını iletilir `az network watcher show` 4. adımda cmdlet'i.

```azurecli
az network watcher show --resource-group resourceGroup --name networkWatcherName
```

### <a name="step-2"></a>2. Adım

Bir depolama hesabı alabilirsiniz. Bu depolama hesabını paket yakalama dosyasını depolamak için kullanılır.

```azurecli
azure storage account list
```

### <a name="step-3"></a>3. Adım

Filtreler, paket yakalama tarafından depolanan verileri sınırlamak için kullanılabilir. Aşağıdaki örnek, bir paket yakalama kurduğunuzda birkaç filtrelerle ayarlar.  İlk üç filtreleri giden TCP trafiğine yalnızca yerel bir IP 10.0.0.3 20, 80 ve 443 hedef bağlantı noktalarına toplayın.  Son filtre yalnızca UDP trafiğini toplar.

```azurecli
az network watcher packet-capture create --resource-group {resoureceurceGroupName} --vm {vmName} --name packetCaptureName --storage-account gwteststorage123abc --filters "[{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"20\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"80\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"443\"},{\"protocol\":\"UDP\"}]"
```

Beklenen çıktı çalışmasını aşağıdaki örnekte olduğu `az network watcher packet-capture create` cmdlet'i.

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

## <a name="get-a-packet-capture"></a>Paket yakalama Al

Çalışan `az network watcher packet-capture show` cmdlet, şu anda çalışan ya da tamamlanmış bir paket yakalama durumunu alır.

```azurecli
az network watcher packet-capture show --name packetCaptureName --location westcentralus
```

Aşağıdaki örnek çıktısı olan `az network watcher packet-capture show` cmdlet'i. Yakalama işlemi tamamlandıktan sonra aşağıdaki örnektir. StopReason TimeExceeded ile PacketCaptureStatus değer durdurulmuş. Bu değer, paket yakalama başarılı oldu ve onun kez çalıştıktan gösterir.

```
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
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/packetCaptures/packetCaptureName",
  "name": "packetCaptureName",
  "provisioningState": "Succeeded",
  "resourceGroup": "NetworkWatcherRG",
  "storageLocation": {
    "filePath": null,
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/gwteststorage123abc",
    "storagePath": "https://gwteststorage123abc.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/{resourceGroupName}/providers/microsoft.compute/virtualmachines/{vmName}/2017/05/25/packetcapt
ure_16_22_34_630.cap"
  },
  "target": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}",
  "timeLimitInSeconds": 18000,
  "totalBytesPerSession": 1073741824
}
```

## <a name="stop-a-packet-capture"></a>Paket yakalama işlemini durdurun

Çalıştırarak `az network watcher packet-capture stop` yakalama oturumu Sürüyor ise cmdlet durduruldu.

```azurecli
az network watcher packet-capture stop --name packetCaptureName --location westcentralus
```

> [!NOTE]
> Cmdlet yanıt verir, o anda çalışan bir yakalama oturumu veya zaten durdurulmuş olan bir oturumu.

## <a name="delete-a-packet-capture"></a>Paket yakalama Sil

```azurecli
az network watcher packet-capture delete --name packetCaptureName --location westcentralus
```

> [!NOTE]
> Paket yakalama silindiğinde depolama hesabındaki dosya silinmez.

## <a name="download-a-packet-capture"></a>Paket yakalama indirin

Paket yakalama oturumunuz tamamladıktan sonra blob depolama alanına veya VM üzerindeki yerel bir dosyaya yakalama dosyasını karşıya yüklenebilir. Paket yakalama depolama konumunu oturum oluşturma sırasında tanımlanır. Bunlar erişmek için uygun bir aracı yakalama dosyalarını bir depolama hesabına kaydedilmiş olan burada indirilebilir Microsoft Azure Storage Gezgini: http://storageexplorer.com/

Bir depolama hesabı belirtilirse, paket yakalama dosyaları şu konumda bir depolama hesabına kaydedilir:

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a>Sonraki adımlar

Sanal makine uyarılarla paket yakalamaları görüntüleyerek otomatikleştirmeyi öğrenin [bir uyarı tetiklenen paket yakalama oluşturma](network-watcher-alert-triggered-packet-capture.md)

Belirli trafik içinde veya dışında VM ziyaret ederek izin verilip verilmediğini Bul [denetleyin IP akış doğrulayın](network-watcher-check-ip-flow-verify-portal.md)

<!-- Image references -->
