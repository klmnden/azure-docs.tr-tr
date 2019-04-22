---
title: Paket yakalama işlemlerini Azure Ağ İzleyicisi - REST API'si ile yönetme | Microsoft Docs
description: Bu sayfa, Azure REST API'sini kullanarak Ağ İzleyicisi paket yakalama özelliğini yönetmek açıklanmaktadır
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 53fe0324-835f-4005-afc8-145eeb314aeb
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 614c3e1831680fff50058cd5192449f8e6e28179
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59049805"
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-azure-rest-api"></a>Paket yakalama işlemlerini Azure REST API'sini kullanarak Azure Ağ İzleyicisi ile yönetme

> [!div class="op_single_selector"]
> - [Azure portal](network-watcher-packet-capture-manage-portal.md)
> - [PowerShell](network-watcher-packet-capture-manage-powershell.md)
> - [Azure CLI](network-watcher-packet-capture-manage-cli.md)
> - [Azure REST API](network-watcher-packet-capture-manage-rest.md)

Ağ İzleyicisi paket yakalama, bir sanal makineye gelen ve giden trafiği izlemek için yakalama oturumu oluşturmanıza olanak sağlar. Sağlamak istediğiniz trafiği yakalamak yakalama oturumu için filtreler sağlanır. Paket yakalama ağ anomalileri öngörülebiliyorsa ve proaktif tanılamaya yardımcı olur. Diğer kullanımlar ağ izinsiz girişi, istemci-sunucu iletişimleri ve daha fazlasını hata ayıklamak için bilgi elde etme, ağ istatistikleri toplama içerir. Çağırabildiğinden uzaktan paket yakalamaları tetiklemek için bir paket yakalama el ile ve değerli zaman kazandırır istediğiniz makineye çalışan yükünü bu özellik kolaylaştırır.

Bu makalede paket yakalaması için şu anda kullanılabilir olan farklı yönetim görevleri alır.

- [**Paket yakalaması Al**](#get-a-packet-capture)
- [**Tüm paket yakalamaları listesi**](#list-all-packet-captures)
- [**Paket yakalaması durumu sorgu**](#query-packet-capture-status)
- [**Paket Yakalamayı Başlat**](#start-packet-capture)
- [**Paket Yakalamayı Durdur**](#stop-packet-capture)
- [**bir paket yakalamasını Sil**](#delete-packet-capture)


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="before-you-begin"></a>Başlamadan önce

Bu senaryoda, IP akışı doğrulama çalıştırmak için Ağ İzleyicisi Rest API çağrısı. ARMclient, PowerShell kullanarak REST API'sini çağırmak için kullanılır. ARMClient bulunur, chocolatey [ARMClient Chocolatey üzerinde](https://chocolatey.org/packages/ARMClient)

Bu senaryo, zaten uyguladığınız adımları varsayar [Ağ İzleyicisi oluşturma](network-watcher-create.md) Ağ İzleyicisi oluşturmak için.

> Paket yakalaması gerektiren bir sanal makine uzantısı `AzureNetworkWatcherExtension`. Bir Windows VM'de uzantıyı yüklemek için ziyaret [Windows için Azure Ağ İzleyicisi Aracısı sanal makine uzantısı](../virtual-machines/windows/extensions-nwa.md) ve Linux VM ziyaret [LinuxiçinAzureAğİzleyicisiAracısısanalmakineuzantısı](../virtual-machines/linux/extensions-nwa.md).

## <a name="log-in-with-armclient"></a>Oturum ARMClient oturum açın

```powershell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a>Bir sanal makine alma

Bir sanal makine döndürmek için aşağıdaki betiği çalıştırın. Bu bilgiler, bir paket yakalaması başlatılıyor için gereklidir.

Aşağıdaki kodu, değişkenleri gerekir:

- **Subscriptionıd** -abonelik kimliği ile de alınabilir **Get-AzSubscription** cmdlet'i.
- **resourceGroupName** -sanal makine içeren bir kaynak grubu adı.

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

Aşağıdaki çıkışı, sonraki örnekte sanal makinenin kimliği kullanılır.

```json
...
,
      "type": "Microsoft.Compute/virtualMachines",
      "location": "westcentralus",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute
/virtualMachines/ContosoVM",
      "name": "ContosoVM"
    }
  ]
}
```


## <a name="get-a-packet-capture"></a>Paket yakalaması Al

Aşağıdaki örnek, tek bir paket yakalamayı durumunu alır.

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures/${packetCaptureName}/querystatus?api-version=2016-12-01"
```

Aşağıdaki yanıtlar bir paket yakalama durumu sorgulanırken döndürülen tipik bir yanıt bir örnektir.

```json
{
  "name": "TestPacketCapture5",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/packetCaptures/TestPacketCapture6",
  "captureStartTime": "2016-12-06T17:20:01.5671279Z",
  "packetCaptureStatus": "Running",
  "packetCaptureError": []
}
```

```json
{
  "name": "TestPacketCapture5",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/packetCaptures/TestPacketCapture6",
  "captureStartTime": "2016-12-06T17:20:01.5671279Z",
  "packetCaptureStatus": "Stopped",
  "stopReason": "TimeExceeded",
  "packetCaptureError": []
}
```

## <a name="list-all-packet-captures"></a>Tüm paket yakalamaları listesi

Aşağıdaki örnek, bir bölgede tüm paket yakalama oturumu alır.

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
armclient get "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures?api-version=2016-12-01"
```

Aşağıdaki yanıt, tüm paket alırken döndürülen tipik bir yanıt örneği yakalar.

```json
{
  "value": [
    {
      "name": "TestPacketCapture6",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/packetCaptures/TestPacketCapture6",
      "etag": "W/\"091762e1-c23f-448b-89d5-37cf56e4c045\"",
      "properties": {
        "provisioningState": "Succeeded",
        "target": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute/virtualMachines/ContosoVM",
        "bytesToCapturePerPacket": 0,
        "totalBytesPerSession": 1073741824,
        "timeLimitInSeconds": 60,
        "storageLocation": {
          "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Storage/storageAccounts/contosoexamplergdiag374",
          "storagePath": "https://contosoexamplergdiag374.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/contosoexamplerg/providers/microsoft.compute/virtualmachines/contosovm/2016/12/06/packetcap
ture_17_19_53_056.cap",
          "filePath": "c:\\temp\\packetcapture.cap"
        },
        "filters": [
          {
            "protocol": "Any",
            "localIPAddress": "",
            "localPort": "",
            "remoteIPAddress": "",
            "remotePort": ""
          }
        ]
      }
    },
    {
      "name": "TestPacketCapture7",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/packetCaptures/TestPacketCapture7",
      "etag": "W/\"091762e1-c23f-448b-89d5-37cf56e4c045\"",
      "properties": {
        "provisioningState": "Failed",
        "target": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute/virtualMachines/ContosoVM",
        "bytesToCapturePerPacket": 0,
        "totalBytesPerSession": 1073741824,
        "timeLimitInSeconds": 60,
        "storageLocation": {
          "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Storage/storageAccounts/contosoexamplergdiag374",
          "storagePath": "https://contosoexamplergdiag374.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/contosoexamplerg/providers/microsoft.compute/virtualmachines/contosovm/2016/12/06/packetcap
ture_17_23_15_364.cap",
          "filePath": "c:\\temp\\packetcapture.cap"
        },
        "filters": [
          {
            "protocol": "Any",
            "localIPAddress": "",
            "localPort": "",
            "remoteIPAddress": "",
            "remotePort": ""
          }
        ]
      }
    }
  ]
}
```

## <a name="query-packet-capture-status"></a>Paket yakalama durumu sorgu

Aşağıdaki örnek, bir bölgede tüm paket yakalama oturumu alır.

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$packetCaptureName = "TestPacketCapture5"
armclient get "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures/${packetCaptureName}/querystatus?api-version=2016-12-01"
```

Şu yanıtı bir paket yakalama durumu sorgulanırken döndürülen tipik bir yanıt örneğidir.

```json
{
    "name": "vm1PacketCapture",
    "id": "/subscriptions/{guid}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkWatchers/{networkWatcherName}/packetCaptures/{packetCaptureName}",
    "captureStartTime" : "9/7/2016 12:35:24PM",
    "packetCaptureStatus" : "Stopped",
    "stopReason" : "TimeExceeded",
    "packetCaptureError" : [ ]
}
```

## <a name="start-packet-capture"></a>Paket Yakalamayı Başlat

Aşağıdaki örnek, bir sanal makinede bir paket yakalama oluşturur.  Örneğin, örnek oluşturma esneklik sağlamak amacıyla parametreli.

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$packetCaptureName = "TestPacketCapture5"
$storageaccountname = "contosoexamplergdiag374"
$vmName = "ContosoVM"
$bytestoCaptureperPacket = "0"
$bytesPerSession = "1073741824"
$captureTimeinSeconds = "60"
$localIP = ""
$localPort = "" # Examples are: 80, or 80-120
$remoteIP = ""
$remotePort = "" # Examples are: 80, or 80-120
$protocol = "" # Valid values are TCP, UDP and Any.
$targetUri = "" # Example: /subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.compute/virtualMachine/$vmName
$storageId = "" # Example: "https://mytestaccountname.blob.core.windows.net/capture/vm1Capture.cap"
$storagePath = ""
$localFilePath = "c:\\temp\\packetcapture.cap" # Example: "d:\capture\vm1Capture.cap"

$requestBody = @"
{
    'properties':  {
                       'target':  '/${targetUri}',
                       'bytesToCapturePerPacket':  '${bytestoCaptureperPacket}',
                       'totalBytesPerSession':  '${bytesPerSession}',
                       'timeLimitinSeconds':  '${captureTimeinSeconds}',
                       'storageLocation':  {
                                               'storageId':  '${storageId}',
                                               'storagePath':  '${storagePath}',
                                               'filePath':  '${localFilePath}'
                                           },
                       'filters':  [
                                       {
                                           'protocol':  '${protocol}',
                                           'localIPAddress':  '${localIP}',
                                           'localPort':  '${localPort}',
                                           'remoteIPAddress':  '${remoteIP}',
                                           'remotePort':  '${remotePort}'
                                       }
                                   ]
                   }
}
"@

armclient PUT "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures/${packetCaptureName}?api-version=2016-07-01" $requestbody
```

## <a name="stop-packet-capture"></a>Paket Yakalamayı Durdur

Aşağıdaki örnek, bir sanal makinede bir paket yakalama durdurur.  Örneğin, örnek oluşturma esneklik sağlamak amacıyla parametreli.

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$packetCaptureName = "TestPacketCapture5"
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures/${packetCaptureName}/stop?api-version=2016-12-01"
```

## <a name="delete-packet-capture"></a>Paket yakalamasını sil

Aşağıdaki örnek, bir sanal makinede bir paket yakalama siler.  Örneğin, örnek oluşturma esneklik sağlamak amacıyla parametreli.

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$packetCaptureName = "TestPacketCapture5"

armclient delete "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures/${packetCaptureName}?api-version=2016-12-01"
```

> [!NOTE]
> Paket yakalaması siliniyor depolama hesabındaki dosya silmez

## <a name="next-steps"></a>Sonraki adımlar

Azure depolama hesaplarından dosyaları indirme ile ilgili yönergeler için başvurmak [.NET kullanarak Azure Blob depolamayı kullanmaya başlama](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Kullanılabilen başka bir Depolama Gezgini aracıdır. Depolama Gezgini hakkında daha fazla bilgi aşağıdaki bağlantıda burada bulunabilir: [Depolama Gezgini](https://storageexplorer.com/)

Sanal makine uyarılarla paket yakalamaları görüntüleyerek otomatikleştirmeyi öğrenme [uyarı tetiklendi paket yakalama oluşturma](network-watcher-alert-triggered-packet-capture.md)













