---
title: Paket yakalama Azure Ağ İzleyicisi - Azure CLI 1.0 ile yönetme | Microsoft Docs
description: Bu sayfa, Azure CLI 1.0 kullanarak Ağ İzleyicisi'nin paket yakalama özelliği yönetmek açıklanmaktadır
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: cb0c1d10-f7f2-4c34-b08c-f73452430be8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: c74d1a94971495f7cd5f5bed42b33869fa9710d9
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32185394"
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-azure-cli-10"></a>Paket yakalama Azure CLI 1.0 kullanarak Azure Ağ İzleyicisi ile yönetme

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-packet-capture-manage-portal.md)
> - [PowerShell](network-watcher-packet-capture-manage-powershell.md)
> - [CLI 1.0](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-packet-capture-manage-cli.md)
> - [Azure REST API'si](network-watcher-packet-capture-manage-rest.md)

Ağ İzleyicisi paket yakalama, bir sanal makine gelen ve giden trafiği izlemek için yakalama oturumları oluşturmanıza olanak sağlar. Filtreler yalnızca trafiği yakalama emin olmak yakalama oturumu için sağlanır. Paket yakalama Tepkisel hem de önceden ağ anormallikleri tanılamanıza yardımcı olur. Diğer kullanımlar ağ yetkisiz erişim, istemci-sunucu iletişimleri ve çok daha fazlasını hata ayıklamak için bilgi sağlamasını ağ istatistikleri toplama içerir. Erişebildiklerinden uzaktan paket yakalamaları tetiklemek için bir paket yakalama el ile ve değerli zaman kazandırır istenen makine üzerinde çalışan iş yükünü Bu yetenek kolaylaştırır.

Bu makalede, platformlar arası Azure CLI 1.0, Windows, Mac ve Linux için kullanılabilir olduğu kullanır.

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

Çalıştırma `azure vm extension set` cmdlet'ini paket yakalama Aracısı Konuk sanal makineye yükleyin.

Windows sanal makineler için:

```azurecli
azure vm extension set -g resourceGroupName -m virtualMachineName -p Microsoft.Azure.NetworkWatcher -r AzureNetworkWatcherExtension -n NetworkWatcherAgentWindows -o 1.4
```

Linux sanal makineleri için:

```azurecli
azure vm extension set -g resourceGroupName -m virtualMachineName -p Microsoft.Azure.NetworkWatcher -r AzureNetworkWatcherExtension -n NetworkWatcherAgentLinux -o 1.4
````

### <a name="step-2"></a>2. Adım

Aracının yüklü olduğundan emin olun, çalıştırmayı `vm extension get` cmdlet'i ve kaynak grubu ve sanal makine adı geçirin. Sonuçta elde edilen listenin aracısının yüklü olduğundan emin olmak için kontrol edin.

```azurecli
azure vm extension get -g resourceGroupName -m virtualMachineName
```

Aşağıdaki örnek yanıt çalışmasını örneğidir `azure vm extension get`

```
info:    Executing command vm extension get
+ Looking up the VM "virtualMachineName"
data:    Publisher                       Name                        Version  State
data:    ------------------------------  -----------------------     -------  ---------
data:    Microsoft.Azure.NetworkWatcher  NetworkWatcherAgentWindows  1.4      Succeeded
info:    vm extension get command OK
```

## <a name="start-a-packet-capture"></a>Paket yakalama Başlat

Yukarıdaki adımları tamamlandıktan sonra paket yakalama Aracısı sanal makineye yüklenir.

### <a name="step-1"></a>1. Adım

Ağ İzleyicisi örneği almak için sonraki adımdır bakın. Bu değişken geçirilir `network watcher show` 4. adımda cmdlet'i.

```azurecli
azure network watcher show -g resourceGroup -n networkWatcherName
```

### <a name="step-2"></a>2. Adım

Bir depolama hesabı alabilirsiniz. Bu depolama hesabını paket yakalama dosyasını depolamak için kullanılır.

```azurecli
azure storage account list
```

### <a name="step-3"></a>3. Adım

Filtreler, paket yakalama tarafından depolanan verileri sınırlamak için kullanılabilir. Aşağıdaki örnek, bir paket yakalama kurduğunuzda birkaç filtrelerle ayarlar.  İlk üç filtreleri giden TCP trafiğine yalnızca yerel bir IP 10.0.0.3 20, 80 ve 443 hedef bağlantı noktalarına toplayın.  Son filtre yalnızca UDP trafiğini toplar.

```azurecli
azure network watcher packet-capture create -g resourceGroupName -w networkWatcherName -n packetCaptureName -t targetResourceId -o storageAccountResourceId -f "[{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"20\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"80\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"443\"},{\"protocol\":\"UDP\"}]"
```

Paket yakalama için birden çok filtre tanımlanabilir. Karmaşık filtre yapısı kullanıyorsanız, filtreleri sözdizimi hataları önlemek için bir json dosyası olarak kullanmak daha iyidir. Örneğin, bayrağını kullanın "-r" (yerine "-f") ve aşağıdaki filtreleri içeren bir json dosyası konumu geçirin:

```json
[
    {
        "protocol":"TCP",
        "remoteIPAddress":"1.1.1.1-255.255.255",
        "localIPAddress":"10.0.0.3",
        "remotePort":"20"
    },
    {
        "protocol":"TCP",
        "remoteIPAddress":"1.1.1.1-255.255.255",
        "localIPAddress":"10.0.0.3",
        "remotePort":"80"
    },
    {
        "protocol":"TCP",
        "remoteIPAddress":"1.1.1.1-255.255.255",
        "localIPAddress":"10.0.0.3",
        "remotePort":"443"
    },
    {
        "protocol":"UDP"
    }
]
```


Beklenen çıktı çalışmasını aşağıdaki örnekte olduğu `network watcher packet-capture create` cmdlet'i.

```
data:    Name                            : packetCaptureName
data:    Etag                            : W/"d59bb2d2-dc95-43da-b740-e0ef8fcacecb"
data:    Target                          : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Compute/virtualMachines/testVM
data:    Bytes To Capture Per Packet     : 0
data:    Total Bytes Per Session         : 1073741824
data:    Time Limit In Seconds           : 18000
data:    Storage Location:
data:      Storage Id                    : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Storage/storageAccounts/testStorage
data:      Storage Path                  : https://testStorage.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/testRG/providers/microsoft.compute/virtualmachines/testVM/2017/02/17/packetcapture_01_21_18_145.cap
data:    Filters                         : []
data:    Provisioning State              : Succeeded
info:    network watcher packet-capture create command OK
```

## <a name="get-a-packet-capture"></a>Paket yakalama Al

Çalışan `network watcher packet-capture show` cmdlet, şu anda çalışan ya da tamamlanmış bir paket yakalama durumunu alır.

```azurecli
azure network watcher packet-capture show -g resourceGroupName -w networkWatcherName -n packetCaptureName
```

Aşağıdaki örnek çıktısı olan `network watcher packet-capture show` cmdlet'i. Yakalama işlemi tamamlandıktan sonra aşağıdaki örnektir. StopReason TimeExceeded ile PacketCaptureStatus değer durdurulmuş. Bu değer, paket yakalama başarılı oldu ve onun kez çalıştıktan gösterir.

```
data:    Name                            : packetCaptureName
data:    Etag                            : W/"d59bb2d2-dc95-43da-b740-e0ef8fcacecb"
data:    Target                          : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Compute/virtualMachines/testVM
data:    Bytes To Capture Per Packet     : 0
data:    Total Bytes Per Session         : 1073741824
data:    Time Limit In Seconds           : 18000
data:    Storage Location:
data:      Storage Id                    : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Storage/storageAccounts/testStorage
data:      Storage Path                  : https://testStorage.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/testRG/providers/microsoft.compute/virtualmachines/testVM/2017/02/17/packetcapture_01_21_18_145.cap
data:    Filters                         : []
data:    Provisioning State              : Succeeded
info:    network watcher packet-capture show command OK
```

## <a name="stop-a-packet-capture"></a>Paket yakalama işlemini durdurun

Çalıştırarak `network watcher packet-capture stop` yakalama oturumu Sürüyor ise cmdlet durduruldu.

```azurecli
azure network watcher packet-capture stop -g resourceGroupName -w networkWatcherName -n packetCaptureName
```

> [!NOTE]
> Cmdlet yanıt verir, o anda çalışan bir yakalama oturumu veya zaten durdurulmuş olan bir oturumu.

## <a name="delete-a-packet-capture"></a>Paket yakalama Sil

```azurecli
azure network watcher packet-capture delete -g resourceGroupName -w networkWatcherName -n packetCaptureName
```

> [!NOTE]
> Paket yakalama silindiğinde depolama hesabındaki dosya silinmez.

## <a name="download-a-packet-capture"></a>Paket yakalama indirin

Paket yakalama oturumunuz tamamladıktan sonra blob depolama alanına veya VM üzerindeki yerel bir dosyaya yakalama dosyasını karşıya yüklenebilir. Paket yakalama depolama konumunu oturum oluşturma sırasında tanımlanır. Bunlar erişmek için uygun bir aracı yakalama dosyalarını bir depolama hesabına kaydedilmiş olan burada indirilebilir Microsoft Azure Storage Gezgini:  http://storageexplorer.com/

Bir depolama hesabı belirtilirse, paket yakalama dosyaları şu konumda bir depolama hesabına kaydedilir:

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a>Sonraki adımlar

Sanal makine uyarılarla paket yakalamaları görüntüleyerek otomatikleştirmeyi öğrenin [bir uyarı tetiklenen paket yakalama oluşturma](network-watcher-alert-triggered-packet-capture.md)

Belirli trafik içinde veya dışında VM ziyaret ederek izin verilip verilmediğini Bul [denetleyin IP akış doğrulayın](diagnose-vm-network-traffic-filtering-problem.md)

<!-- Image references -->
