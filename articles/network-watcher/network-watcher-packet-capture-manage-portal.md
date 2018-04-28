---
title: Paket yakalama Azure Ağ İzleyicisi - Azure portal ile yönetme | Microsoft Docs
description: Bu sayfa, Azure portalını kullanarak Ağ İzleyicisi'nin paket yakalama özelliği yönetmek açıklanmaktadır
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
ms.openlocfilehash: 508b9e7eef757277d4bc0e93a26f3a63045f31e4
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-the-portal"></a>Paket yakalama Portalı'nı kullanarak Azure Ağ İzleyicisi ile yönetme

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-packet-capture-manage-portal.md)
> - [PowerShell](network-watcher-packet-capture-manage-powershell.md)
> - [CLI 1.0](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-packet-capture-manage-cli.md)
> - [Azure REST API'si](network-watcher-packet-capture-manage-rest.md)

Ağ İzleyicisi paket yakalama, bir sanal makine gelen ve giden trafiği izlemek için yakalama oturumları oluşturmanıza olanak sağlar. Filtreler yalnızca trafiği yakalama emin olmak yakalama oturumu için sağlanır. Paket yakalama Tepkisel hem de önceden ağ anormallikleri tanılamanıza yardımcı olur. Diğer kullanımlar ağ yetkisiz erişim, istemci-sunucu iletişimleri ve çok daha fazlasını hata ayıklamak için bilgi sağlamasını ağ istatistikleri toplama içerir. Erişebildiklerinden uzaktan paket yakalamaları tetiklemek için bir paket yakalama el ile ve değerli zaman kazandırır istenen makine üzerinde çalışan iş yükünü Bu yetenek kolaylaştırır.

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
> Paket yakalama gerektiren bir sanal makine uzantısı `AzureNetworkWatcherExtension`. Bir Windows VM uzantısı yüklemek için ziyaret [Windows için Azure Ağ İzleyicisi Aracısı sanal makine uzantısı](../virtual-machines/windows/extensions-nwa.md) ve Linux VM ziyaret edin: [Linux için Azure Ağ İzleyicisi Aracısı sanal makine uzantısı](../virtual-machines/linux/extensions-nwa.md).

### <a name="packet-capture-agent-extension-through-the-portal"></a>Paket Yakalama aracı uzantısı portal üzerinden

Portal üzerinden paket yakalama VM aracısı yüklemek için sanal makinenize gidin, **uzantıları** > **Ekle** arayın ve **için Ağ İzleyicisi Aracısı Windows**

![Aracı görünümü][agent]

## <a name="packet-capture-overview"></a>Paket yakalama genel bakış

Gidin [Azure portal](https://portal.azure.com) tıklatıp **ağ** > **Ağ İzleyicisi** > **paket yakalama**

Genel bakış sayfasında, tüm paket listesini olsun durumu mevcut yakalar gösterir.

> [!NOTE]
> Paket yakalama conectivity gerektirir.
> * Depolama hesabı bağlantı noktası 443 üzerinden giden bağlantı.
> * 169.254.169.254 gelen ve giden bağlantı
> * 168.63.129.16 gelen ve giden bağlantı

![Paket yakalama genel bakış ekranı][1]

## <a name="start-a-packet-capture"></a>Paket yakalama Başlat

Tıklatın **Ekle** bir paket yakalama oluşturma.

Bir paket yakalama tanımlanabilir özellikleri şunlardır:

**Ana ayarları**

- **Abonelik** -bu değeri kullanılan abonelik, Ağ İzleyicisi örneği her abonelik için gereklidir.
- **Kaynak grubu** -sanal makinenin hedeflenen kaynak grubu.
- **Hedef sanal makine** -paket yakalama çalıştıran sanal makine
- **Paket yakalama adı** -bu değer paket yakalama adıdır.

**Yapılandırma Yakalama**

- **Yerel dosya yolu** -paket yakalama kaydettiğiniz yere sanal makinedeki yerel yol (yalnızca olduğunda geçerlidir **[dosya]** seçilir). Geçerli bir yol belirtmeniz gerekir. Linux sanal makine kullanıyorsanız, yol ile başlamalıdır / var / yakalar.
- **Depolama hesabı** -paket yakalama bir depolama hesabında kaydedilip kaydedilmeyeceğini belirler.
- **Dosya** -paket yakalama sanal makinede yerel olarak kaydedilirse belirler.
- **Depolama hesapları** - seçili paket yakalama kaydetmek için depolama hesabı. Varsayılan konumdur https://{storage hesap name}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscription kimliği} /resourcegroups/ {kaynak grubu name}/providers/microsoft.compute/virtualmachines/{virtual makine adı} / {YY} / {MM} / {gg} / {ss} packetcapture__{MM}_{SS} _ {XXX} .cap. (Yalnızca etkin olup **depolama** seçili)
- **Yerel dosya yolu** -paket yakalama kaydetmek için bir sanal makinede yerel yolu. (Yalnızca etkin olup **dosya** seçilir). Geçerli bir yol sağlanmalıdır. Bir Linux sanal makine için yol ile başlamalıdır *yakalar/var/*.
- **Paket başına en fazla bayt** - sayı yakalanan bayt gelen her paket, tüm baytlar boş bırakılırsa yakalanır.
- **Oturum başına en fazla bayt** - toplam değeri paket yakalama durakları ulaşıldıktan sonra yakalanan bayt sayısı.
- **Süre (saniye)** -durdurmak paket yakalama için zaman sınırını ayarlar. Varsayılan değer 18000 saniyedir.

> [!NOTE]
> Paket depolama yakalar için premium depolama hesapları şu anda desteklenmemektedir.

**(İsteğe bağlı) filtreleme**

- **Protokol** -paket yakalama için filtre uygulamak için protokol. Kullanılabilir TCP, UDP ve herhangi bir değerlerdir.
- **Yerel IP adresi** -bu değer, yerel IP adresi bu filtre değeri eşleştiği paket için paket yakalama filtreler.
- **Yerel bağlantı noktası** -bu değer, yerel bağlantı noktası bu filtre değeri eşleştiği paket için paket yakalama filtreler.
- **Uzak IP adresi** -uzak IP Bu filtre değeri eşleştiği paket için paket yakalama bu değeri filtreler.
- **Uzak bağlantı noktası** -uzak bağlantı noktası bu filtre değeri eşleştiği paket için paket yakalama bu değeri filtreler.

> [!NOTE]
> Bağlantı noktası ve IP adresi değerleri, tek bir değer, değer aralığı veya bir kümesi olabilir. (diğer bir deyişle, bağlantı noktası 80-1024 için) İstediğiniz sayıda filtreleri tanımlayabilirsiniz.

Değerleri doldurulduktan sonra tıklayın **Tamam** paket yakalama oluşturma.

![Paket yakalama oluşturma][2]

Paket yakalama ayarlamak süre sona erdiğinde, paket yakalama durdurur ve gözden geçirilebilir. Paket yakalama oturumları el ile de durdurabilirsiniz.

## <a name="delete-a-packet-capture"></a>Paket yakalama Sil

Paket yakalama Görüntüle'yi tıklatın **bağlam menüsü** (...) veya sağ tıklayın ve tıklayın **silmek** paket yakalama durdurmak için

![Paket yakalama Sil][3]

> [!NOTE]
> Paket yakalama silindiğinde depolama hesabındaki dosya silinmez.

Paket yakalama silmek, tıklatın istediğinizi onaylamak için sorulur **Evet**

![Onaylama][4]

## <a name="stop-a-packet-capture"></a>Paket yakalama işlemini durdurun

Paket yakalama Görüntüle'yi tıklatın **bağlam menüsü** (...) veya sağ tıklayın ve tıklayın **durdurmak** paket yakalama durdurmak için

## <a name="download-a-packet-capture"></a>Paket yakalama indirin

Paket yakalama oturumunuz tamamladıktan sonra yakalama dosyasını blob depolama alanına veya VM üzerindeki yerel bir dosyaya yüklenir. Paket yakalama depolama konumunu oturum oluşturma sırasında tanımlanır. Bunlar erişmek için uygun bir aracı yakalama dosyalarını bir depolama hesabına kaydedilmiş olan burada indirilebilir Microsoft Azure Storage Gezgini:  http://storageexplorer.com/

Bir depolama hesabı belirtilirse, paket yakalama dosyaları şu konumda bir depolama hesabına kaydedilir:
```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a>Sonraki adımlar

Sanal makine uyarılarla paket yakalamaları görüntüleyerek otomatikleştirmeyi öğrenin [bir uyarı tetiklenen paket yakalama oluşturma](network-watcher-alert-triggered-packet-capture.md)

Belirli trafik içinde veya dışında VM ziyaret ederek izin verilip verilmediğini Bul [denetleyin IP akış doğrulayın](diagnose-vm-network-traffic-filtering-problem.md)

<!-- Image references -->
[1]: ./media/network-watcher-packet-capture-manage-portal/figure1.png
[2]: ./media/network-watcher-packet-capture-manage-portal/figure2.png
[3]: ./media/network-watcher-packet-capture-manage-portal/figure3.png
[4]: ./media/network-watcher-packet-capture-manage-portal/figure4.png
[agent]: ./media/network-watcher-packet-capture-manage-portal/agent.png













