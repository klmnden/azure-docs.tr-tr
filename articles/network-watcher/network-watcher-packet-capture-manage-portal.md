---
title: Paket yakalama işlemlerini Azure Ağ İzleyicisi - Azure portal ile yönetme | Microsoft Docs
description: Bu sayfa, Azure portalını kullanarak Ağ İzleyicisi paket yakalama özelliğini yönetmek açıklanmaktadır
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 59edd945-34ad-4008-809e-ea904781d918
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 18e5f8eda51f8834f6346eef3d7ad31bc556290a
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090206"
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-the-portal"></a>Portalı kullanarak Azure Ağ İzleyicisi ile paket yakalamayı yönetme

> [!div class="op_single_selector"]
> - [Azure portal](network-watcher-packet-capture-manage-portal.md)
> - [PowerShell](network-watcher-packet-capture-manage-powershell.md)
> - [Azure CLI](network-watcher-packet-capture-manage-cli.md)
> - [Azure REST API](network-watcher-packet-capture-manage-rest.md)

Ağ İzleyicisi paket yakalama, bir sanal makineye gelen ve giden trafiği izlemek için yakalama oturumu oluşturmanıza olanak sağlar. Sağlamak istediğiniz trafiği yakalamak yakalama oturumu için filtreler sağlanır. Paket yakalama ağ anomalileri öngörülebiliyorsa ve proaktif tanılamaya yardımcı olur. Diğer kullanımlar ağ izinsiz girişi, istemci-sunucu iletişimleri ve daha fazlasını hata ayıklamak için bilgi elde etme, ağ istatistikleri toplama içerir. Çağırabildiğinden uzaktan paket yakalamaları tetiklemek için bir paket yakalama el ile ve değerli zaman kazandırır istediğiniz makineye çalışan yükünü bu özellik kolaylaştırır.

Bu makalede paket yakalaması için şu anda kullanılabilir olan farklı yönetim görevleri alır.

- [**Paket Yakalamayı Başlat**](#start-a-packet-capture)
- [**Paket Yakalamayı Durdur**](#stop-a-packet-capture)
- [**Bir paket yakalamasını Sil**](#delete-a-packet-capture)
- [**Paket yakalaması indirin**](#download-a-packet-capture)

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, aşağıdaki kaynaklara sahip olduğunuzu varsayar:

- Ağ İzleyicisi paket yakalama oluşturmak istediğiniz bölgede örneği
- Bir sanal makine etkin paket yakalama uzantısına sahip.

> [!IMPORTANT]
> Paket yakalaması gerektiren bir sanal makine uzantısı `AzureNetworkWatcherExtension`. Bir Windows VM'de uzantıyı yüklemek için ziyaret [Windows için Azure Ağ İzleyicisi Aracısı sanal makine uzantısı](../virtual-machines/windows/extensions-nwa.md) ve Linux VM ziyaret [LinuxiçinAzureAğİzleyicisiAracısısanalmakineuzantısı](../virtual-machines/linux/extensions-nwa.md).

### <a name="packet-capture-agent-extension-through-the-portal"></a>Portal üzerinden paket Yakalama aracı uzantısı

Portal üzerinden paket yakalama VM Aracısı'nı yüklemek için sanal makinenize gidin, **uzantıları** > **Ekle** araması **Windows için Ağ İzleyicisi Aracısı**

![Aracı görünümü][agent]

## <a name="packet-capture-overview"></a>Paket yakalamaya genel bakış

Gidin [Azure portalında](https://portal.azure.com) tıklatıp **ağ** > **Ağ İzleyicisi** > **paket yakalama**

Genel bakış sayfasında, mevcut durumu ne olursa olsun tüm paket listesini yakalar gösterir.

> [!NOTE]
> Paket yakalaması, conectivity gerektirir.
> * Depolama hesabı bağlantı noktası 443 üzerinden giden bağlantı.
> * 169.254.169.254 numaralı gelen ve giden bağlantı
> * 168.63.129.16 gelen ve giden bağlantı

![Paket yakalama genel bakış ekranı][1]

## <a name="start-a-packet-capture"></a>Paket Yakalamayı Başlat

Tıklayın **Ekle** paket yakalaması oluşturmak için.

Paket yakalamayı üzerinde tanımlanan özellikler şunlardır:

**Ana Ayarlar**

- **Abonelik** -bu değeri kullanılan abonelik, Ağ İzleyicisi örneği her bir aboneliği gereklidir.
- **Kaynak grubu** -hedeflenmiş sanal makinenin kaynak grubu.
- **Hedef sanal makine** -paket yakalaması çalıştıran sanal makine
- **Paket yakalama adı** -bu değer paket yakalama adıdır.

**Yapılandırma Yakala**

- **Yerel dosya yolu** -paket yakalaması kaydedildiği sanal makinede yerel yol (yalnızca geçerli olduğunda **[file]** seçili). Geçerli bir yol belirtmeniz gerekir. Linux sanal makinesi kullanıyorsanız, yol ile başlamalıdır / var / yakalar.
- **Depolama hesabı** -paket yakalaması bir depolama hesabında kaydedilip kaydedilmeyeceğini belirler.
- **Dosya** -paket yakalama, sanal makinede yerel olarak kaydedilirse belirler.
- **Depolama hesapları** - paket yakalama kaydetmek için depolama hesabı seçilmedi. Varsayılan konumu: https://{storage hesap name}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscription kimliği} /resourcegroups/ {kaynak grubu name}/providers/microsoft.compute/virtualmachines/{virtual makine adı} / {YY} / {MM} / {DD} / {HH} packetcapture__{MM}_{SS} _ {XXX} .cap. (Yalnızca etkin olup **depolama** seçili)
- **Yerel dosya yolu** -paket yakalaması kaydetmek için bir sanal makinede yerel yolu. (Yalnızca etkin olup **dosya** seçili). Geçerli bir yol sağlanmalıdır. Bir Linux sanal makinesi için yol ile başlamalıdır */var/yakalar*.
- **Paket başına en fazla bayt** - sayı yakalanan bayt her paket, tüm baytları boş bırakılırsa yakalanır.
- **Oturum başına en fazla bayt** - toplam değeri, paket yakalama durakları ulaşıldıktan sonra yakalanan bayt sayısı.
- **Süre (saniye)** -durdurmak paket yakalaması için bir zaman sınırı ayarlar. Varsayılan 18000 saniyedir.

> [!NOTE]
> Paket depolama yakalar için premium depolama hesapları şu anda desteklenmemektedir.

**(İsteğe bağlı) filtreleme**

- **Protokol** -paket yakalaması için filtre uygulamak için protokol. TCP, UDP ve değerleri kullanılabilir.
- **Yerel IP adresi** -yerel IP adresi bu filtre değeri eşleştiği paketleri paket yakalaması bu değeri filtreler.
- **Yerel bağlantı noktası** -yerel bağlantı noktası bu filtre değeri eşleştiği paketleri paket yakalaması bu değeri filtreler.
- **Uzak IP adresi** -uzak IP Bu filtre değeri eşleştiği paketleri paket yakalaması bu değeri filtreler.
- **Uzak bağlantı noktası** -uzak bağlantı noktası bu filtre değeri eşleştiği paketleri paket yakalaması bu değeri filtreler.

> [!NOTE]
> Bağlantı noktası ve IP adresi değerleri tek bir değer, değer aralığı veya bir kümesi olabilir. (diğer bir deyişle, bağlantı noktası 80-1024 için) İstediğiniz sayıda filtreleri tanımlayabilirsiniz.

Değerleri doldurulduktan sonra tıklayın **Tamam** paket yakalaması oluşturmak için.

![Paket Yakalaması oluşturma][2]

Paket yakalama süresi sınırını süresi dolduktan sonra paket yakalaması durdurur ve incelenebilir. El ile paket yakalamaları oturumları durdurabilirsiniz.

## <a name="delete-a-packet-capture"></a>Bir paket yakalamasını Sil

Paket yakalama Görüntüle'yi tıklatın **bağlam menüsü** (...) sağ tıklayın ve tıklayın **Sil** paket yakalaması durdurulamadı

![bir paket yakalamasını Sil][3]

> [!NOTE]
> Paket yakalaması siliniyor depolama hesabındaki dosya silinmez.

İstediğiniz paket yakalamasını silmek onaylamanız istenir **Evet**

![Onaylama][4]

## <a name="stop-a-packet-capture"></a>Paket Yakalamayı Durdur

Paket yakalama Görüntüle'yi tıklatın **bağlam menüsü** (...) sağ tıklayın ve tıklayın **Durdur** paket yakalaması durdurulamadı

## <a name="download-a-packet-capture"></a>Paket yakalaması indirin

Paket yakalama oturumunuz tamamladıktan sonra yakalama dosyası VM'deki yerel bir dosyaya veya blob depolama alanına yüklenir. Paket yakalaması depolama konumunu oluşturma oturumu sırasında tanımlanır. Bunlar erişmek için uygun bir araç yakalama dosyalarını bir depolama hesabına kayıtlı olduğundan burada indirilebilir Microsoft Azure Depolama Gezgini:  http://storageexplorer.com/

Bir depolama hesabı belirttiyseniz, paket yakalama dosyaları şu konumda bir depolama hesabına kaydedilir:
```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a>Sonraki adımlar

Sanal makine uyarılarla paket yakalamaları görüntüleyerek otomatikleştirmeyi öğrenme [uyarı tetiklendi paket yakalama oluşturma](network-watcher-alert-triggered-packet-capture.md)

Belirli trafiğe içine veya dışına VM'nizi ederek izin verilip verilmediğini Bul [denetleyin IP akışı doğrulama](diagnose-vm-network-traffic-filtering-problem.md)

<!-- Image references -->
[1]: ./media/network-watcher-packet-capture-manage-portal/figure1.png
[2]: ./media/network-watcher-packet-capture-manage-portal/figure2.png
[3]: ./media/network-watcher-packet-capture-manage-portal/figure3.png
[4]: ./media/network-watcher-packet-capture-manage-portal/figure4.png
[agent]: ./media/network-watcher-packet-capture-manage-portal/agent.png













