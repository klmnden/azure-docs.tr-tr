---
title: Paket yakalama işlemlerini Azure Ağ İzleyicisi - PowerShell ile yönetme | Microsoft Docs
description: Bu sayfa, PowerShell kullanarak Ağ İzleyicisi paket yakalama özelliğini yönetmek açıklanmaktadır
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 04d82085-c9ea-4ea1-b050-a3dd4960f3aa
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 267b2c375ef9672c8e5bd7cb8280b4dd40dbcd0d
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59045552"
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-powershell"></a>PowerShell kullanarak Azure Ağ İzleyicisi ile paket yakalamayı yönetme

> [!div class="op_single_selector"]
> - [Azure portal](network-watcher-packet-capture-manage-portal.md)
> - [PowerShell](network-watcher-packet-capture-manage-powershell.md)
> - [Azure CLI](network-watcher-packet-capture-manage-cli.md)
> - [Azure REST API](network-watcher-packet-capture-manage-rest.md)

Ağ İzleyicisi paket yakalama, bir sanal makineye gelen ve giden trafiği izlemek için yakalama oturumu oluşturmanıza olanak sağlar. Sağlamak istediğiniz trafiği yakalamak yakalama oturumu için filtreler sağlanır. Paket yakalama ağ anomalileri öngörülebiliyorsa ve proaktif tanılamaya yardımcı olur. Diğer kullanımlar ağ izinsiz girişi, istemci-sunucu iletişimleri ve daha fazlasını hata ayıklamak için bilgi elde etme, ağ istatistikleri toplama içerir. Çağırabildiğinden uzaktan paket yakalamaları tetiklemek için bir paket yakalama el ile ve değerli zaman kazandırır istediğiniz makineye çalışan yükünü bu özellik kolaylaştırır.

Bu makalede paket yakalaması için şu anda kullanılabilir olan farklı yönetim görevleri alır.

- [**Paket Yakalamayı Başlat**](#start-a-packet-capture)
- [**Paket Yakalamayı Durdur**](#stop-a-packet-capture)
- [**bir paket yakalamasını Sil**](#delete-a-packet-capture)
- [**Paket yakalaması indirin**](#download-a-packet-capture)


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, aşağıdaki kaynaklara sahip olduğunu varsayar:

* Ağ İzleyicisi paket yakalama oluşturmak istediğiniz bölgede örneği

* Bir sanal makine etkin paket yakalama uzantısına sahip.

> [!IMPORTANT]
> Paket yakalaması gerektiren bir sanal makine uzantısı `AzureNetworkWatcherExtension`. Bir Windows VM'de uzantıyı yüklemek için ziyaret [Windows için Azure Ağ İzleyicisi Aracısı sanal makine uzantısı](../virtual-machines/windows/extensions-nwa.md) ve Linux VM ziyaret [LinuxiçinAzureAğİzleyicisiAracısısanalmakineuzantısı](../virtual-machines/linux/extensions-nwa.md).

## <a name="install-vm-extension"></a>VM uzantıları yükleme

### <a name="step-1"></a>1. Adım

```powershell
$VM = Get-AzVM -ResourceGroupName testrg -Name VM1
```

### <a name="step-2"></a>2. Adım

Aşağıdaki örnek çalıştırmak için gerekli uzantı bilgileri alır. `Set-AzVMExtension` cmdlet'i. Bu cmdlet, paket Yakalama aracı Konuk sanal makineye yükler.

> [!NOTE]
> `Set-AzVMExtension` Cmdlet'inin tamamlanması birkaç dakika sürebilir.

Windows sanal makineler için:

```powershell
$AzureNetworkWatcherExtension = Get-AzVMExtensionImage -Location WestCentralUS -PublisherName Microsoft.Azure.NetworkWatcher -Type NetworkWatcherAgentWindows -Version 1.4.585.2
$ExtensionName = "AzureNetworkWatcherExtension"
Set-AzVMExtension -ResourceGroupName $VM.ResourceGroupName  -Location $VM.Location -VMName $VM.Name -Name $ExtensionName -Publisher $AzureNetworkWatcherExtension.PublisherName -ExtensionType $AzureNetworkWatcherExtension.Type -TypeHandlerVersion $AzureNetworkWatcherExtension.Version.Substring(0,3)
```

Linux sanal makineleri için:

```powershell
$AzureNetworkWatcherExtension = Get-AzVMExtensionImage -Location WestCentralUS -PublisherName Microsoft.Azure.NetworkWatcher -Type NetworkWatcherAgentLinux -Version 1.4.13.0
$ExtensionName = "AzureNetworkWatcherExtension"
Set-AzVMExtension -ResourceGroupName $VM.ResourceGroupName  -Location $VM.Location -VMName $VM.Name -Name $ExtensionName -Publisher $AzureNetworkWatcherExtension.PublisherName -ExtensionType $AzureNetworkWatcherExtension.Type -TypeHandlerVersion $AzureNetworkWatcherExtension.Version.Substring(0,3)
```

Aşağıdaki örnek, başarılı bir yanıt çalıştırdıktan sonra `Set-AzVMExtension` cmdlet'i.

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   
```

### <a name="step-3"></a>3. Adım

Aracısının yüklü olduğundan emin olmak için çalıştırmanız `Get-AzVMExtension` cmdlet'i ve sanal makine adı ve uzantı adı geçirin.

```powershell
Get-AzVMExtension -ResourceGroupName $VM.ResourceGroupName  -VMName $VM.Name -Name $ExtensionName
```

Aşağıdaki örnek bir yanıt çalışmasını örneğidir. `Get-AzVMExtension`

```
ResourceGroupName       : testrg
VMName                  : testvm1
Name                    : AzureNetworkWatcherExtension
Location                : westcentralus
Etag                    : null
Publisher               : Microsoft.Azure.NetworkWatcher
ExtensionType           : NetworkWatcherAgentWindows
TypeHandlerVersion      : 1.4
Id                      : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Compute/virtualMachines/testvm1/
                          extensions/AzureNetworkWatcherExtension
PublicSettings          : 
ProtectedSettings       : 
ProvisioningState       : Succeeded
Statuses                : 
SubStatuses             : 
AutoUpgradeMinorVersion : True
ForceUpdateTag          : 
```

## <a name="start-a-packet-capture"></a>Paket Yakalamayı Başlat

Yukarıdaki adımlar tamamlandıktan sonra paket yakalama Aracısı sanal makineye yüklenir.

### <a name="step-1"></a>1. Adım

Ağ İzleyicisi örneğini almak için sonraki adımdır bakın. Bu değişkenin geçirilen `New-AzNetworkWatcherPacketCapture` 4. adımda cmdlet'i.

```powershell
$nw = Get-AzResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" }
$networkWatcher = Get-AzNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName  
```

### <a name="step-2"></a>2. Adım

Bir depolama hesabı alın. Bu depolama hesabı, paket yakalama dosyasını depolamak için kullanılır.

```powershell
$storageAccount = Get-AzStorageAccount -ResourceGroupName testrg -Name testrgsa123
```

### <a name="step-3"></a>3. Adım

Paket yakalaması tarafından depolanan verileri sınırlamak için filtreleri kullanılabilir. Aşağıdaki örnek, iki filtreleri ayarlar.  Bir filtre, 20, 80 ve 443 numaralı hedef bağlantı noktalarına giden TCP trafiğine yalnızca yerel IP 10.0.0.3 toplar.  İkinci filtre yalnızca UDP trafiğini toplar.

```powershell
$filter1 = New-AzPacketCaptureFilterConfig -Protocol TCP -RemoteIPAddress "1.1.1.1-255.255.255.255" -LocalIPAddress "10.0.0.3" -LocalPort "1-65535" -RemotePort "20;80;443"
$filter2 = New-AzPacketCaptureFilterConfig -Protocol UDP
```

> [!NOTE]
> Bir paket yakalaması için birden çok filtre tanımlanabilir.

### <a name="step-4"></a>4. Adım

Çalıştırma `New-AzNetworkWatcherPacketCapture` gerekli değerler geçirerek paket yakalama işlemini başlatmak için cmdlet, önceki adımlarda alınır.
```powershell

New-AzNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -TargetVirtualMachineId $vm.Id -PacketCaptureName "PacketCaptureTest" -StorageAccountId $storageAccount.id -TimeLimitInSeconds 60 -Filter $filter1, $filter2
```

Aşağıdaki örnek, beklenen çıktıyı çalışmasını `New-AzNetworkWatcherPacketCapture` cmdlet'i.

```
Name                    : PacketCaptureTest
Id                      : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatcher
                          s/NetworkWatcher_westcentralus/packetCaptures/PacketCaptureTest
Etag                    : W/"3bf27278-8251-4651-9546-c7f369855e4e"
ProvisioningState       : Succeeded
Target                  : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Compute/virtualMachines/testvm1
BytesToCapturePerPacket : 0
TotalBytesPerSession    : 1073741824
TimeLimitInSeconds      : 60
StorageLocation         : {
                            "StorageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Storage/storageA
                          ccounts/examplestorage",
                            "StoragePath": "https://examplestorage.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-00000
                          0000000/resourcegroups/testrg/providers/microsoft.compute/virtualmachines/testvm1/2017/02/01/packetcapture_22_42_48_238.cap"
                          }
Filters                 : [
                            {
                              "Protocol": "TCP",
                              "RemoteIPAddress": "1.1.1.1-255.255.255",
                              "LocalIPAddress": "10.0.0.3",
                              "LocalPort": "1-65535",
                              "RemotePort": "20;80;443"
                            },
                            {
                              "Protocol": "UDP",
                              "RemoteIPAddress": "",
                              "LocalIPAddress": "",
                              "LocalPort": "",
                              "RemotePort": ""
                            }
                          ]


```

## <a name="get-a-packet-capture"></a>Paket yakalaması Al

Çalışan `Get-AzNetworkWatcherPacketCapture` cmdlet'i, şu anda çalışan veya tamamlanmış bir paket yakalaması durumunu alır.

```powershell
Get-AzNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
```

Aşağıdaki örnek çıktısı, `Get-AzNetworkWatcherPacketCapture` cmdlet'i. Yakalama işlemi tamamlandıktan sonra aşağıdaki örnekte bulunur. PacketCaptureStatus değeri, stopreason & TimeExceeded ile durdurulur. Bu değer, paket yakalaması başarılı oldu ve kendi zamanı çalıştırdığınız gösterir.
```
Name                    : PacketCaptureTest
Id                      : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatcher
                          s/NetworkWatcher_westcentralus/packetCaptures/PacketCaptureTest
Etag                    : W/"4b9a81ed-dc63-472e-869e-96d7166ccb9b"
ProvisioningState       : Succeeded
Target                  : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Compute/virtualMachines/testvm1
BytesToCapturePerPacket : 0
TotalBytesPerSession    : 1073741824
TimeLimitInSeconds      : 60
StorageLocation         : {
                            "StorageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Storage/storageA
                          ccounts/examplestorage",
                            "StoragePath": "https://examplestorage.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-00000
                          0000000/resourcegroups/testrg/providers/microsoft.compute/virtualmachines/testvm1/2017/02/01/packetcapture_22_42_48_238.cap"
                          }
Filters                 : [
                            {
                              "Protocol": "TCP",
                              "RemoteIPAddress": "1.1.1.1-255.255.255",
                              "LocalIPAddress": "10.0.0.3",
                              "LocalPort": "1-65535",
                              "RemotePort": "20;80;443"
                            },
                            {
                              "Protocol": "UDP",
                              "RemoteIPAddress": "",
                              "LocalIPAddress": "",
                              "LocalPort": "",
                              "RemotePort": ""
                            }
                          ]
CaptureStartTime        : 2/1/2017 10:43:01 PM
PacketCaptureStatus     : Stopped
StopReason              : TimeExceeded
PacketCaptureError      : []
```

## <a name="stop-a-packet-capture"></a>Paket Yakalamayı Durdur

Çalıştırarak `Stop-AzNetworkWatcherPacketCapture` yakalama oturumu, sürüyorsa cmdlet'i durduruldu.

```powershell
Stop-AzNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
```

> [!NOTE]
> Cmdlet hiç yanıt döndürür, o anda çalışan bir yakalama oturumu veya zaten durduran bir var olan oturumu çalıştı.

## <a name="delete-a-packet-capture"></a>bir paket yakalamasını Sil

```powershell
Remove-AzNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
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














