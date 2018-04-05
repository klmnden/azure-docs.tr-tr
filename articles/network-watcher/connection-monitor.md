---
title: Ağ bağlantıları Azure Ağ İzleyicisi - Azure portal ile izleme | Microsoft Docs
description: Azure portalını kullanarak Azure Ağ İzleyicisi ile ağ bağlantısını izlemeniz öğrenin.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2018
ms.author: jdial
ms.openlocfilehash: 0d550d3bda119cfcb9ecc6f852006d5e325fdfa3
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="monitor-network-connections-with-azure-network-watcher-using-the-azure-portal"></a>Azure portalını kullanarak Azure Ağ İzleyicisi ile ağ bağlantılarını izleme

Bağlantı İzleyici bir Azure sanal makine (VM) ve bir IP adresi arasında ağ bağlantısı izlemek için nasıl kullanılacağını öğrenin. Bağlantı İzleyicisi, kaynak ve hedef IP adresi ve bağlantı noktası arasında izleme sağlar. Bağlantı İzleyici bağlantısını VM sanal ağ ile aynı veya farklı sanal ağda VM çalışan SQL server 1433 numaralı bağlantı noktası üzerinden izleme gibi senaryolara olanak sağlar. Bağlantı İzleyici bağlantı gecikmesi 60 saniyede kaydedilen bir Azure İzleyici ölçümü olarak sağlar. Ayrıca bir atlama atlamalı topolojisi sağlar ve bağlantınızı etkileyen yapılandırma sorunlarını tanımlar.

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları gerçekleştirmeden önce aşağıdaki gereksinimleri karşılaması gerekir:

* Ağ İzleyicisi bir bağlantı için izlemek istediğiniz bölgede bir örneği. Zaten yoksa, bir içindeki adımları tamamlayarak oluşturabilirsiniz [Azure Ağ İzleyicisi örnek oluşturmak](network-watcher-create.md).
* Gelen izlemek için bir VM. Bir VM oluşturma konusunda bilgi almak için bkz bir [Windows](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) veya [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) VM.
* Sahip `AzureNetworkWatcherExtension` öğesinden bir bağlantı izlemek istediğiniz VM yüklenir. Bir Windows VM uzantısı yüklemek için bkz: [Windows için Azure Ağ İzleyicisi Aracısı sanal makine uzantısı](../virtual-machines/windows/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) ve bir Linux VM bakın uzantıyı yüklemek için [Azure Ağ İzleyicisi Aracısı sanal makine uzantısı Linux](../virtual-machines/linux/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma 

[Azure Portal](http://portal.azure.com)’da oturum açın.

## <a name="create-a-connection-monitor"></a>Bağlantı izleyici oluşturmak

Aşağıdaki adımları bağlantı 80 ve 1433 bağlantı noktaları üzerinden bir hedef VM için izlemeyi etkinleştir:

1. Portalın sol taraftan **tüm hizmetleri**.
2. Yazmaya başlayın *Ağ İzleyicisi* içinde **filtre** kutusu. Zaman **Ağ İzleyicisi** arama sonuçlarında görünür.
3. Altında **izleme**seçin **Bağlantı İzleyicisi**.
4. Seçin **+ Ekle**.
5. Bağlantı için izleme ve ardından istediğiniz bilgileri girin veya seçin **Ekle**. Aşağıdaki resimde gösterilen örnekte, izlenen bağlantı arasındadır *MultiTierApp0* VM *Database0* bağlantı noktası 80 üzerinden VM:

    ![Bağlantı monitör ekleme](./media/connection-monitor/add-connection-monitor.png)

    İzlemeye başlar. Bağlantı İzleyici 60 saniyede araştırmaları.
6. Aynı kaynak ve hedef VM'ler ve aşağıdaki değerleri belirtme adım 5 yeniden tamamlayın:
    
    |Ayar  |Değer          |
    |---------|---------      |
    |Ad     | AppToDB(1433) |
    |Bağlantı noktası     | 1433          |

## <a name="view-connection-monitoring"></a>Bağlantı izleme görünümü

1. Adım 1-3'te tamamlamak [bağlantı izleyici oluşturmak](#create-a-connection-monitor) bağlantı izleme görüntülemek için.
2. Aşağıdaki resimde için ayrıntıları gösterir *AppToDB(80)* bağlantı. **Durum** ulaşılamıyor. **Grafik görünümü** gösterir **ortalama gidiş dönüş süresi** ve **% yoklamaları başarısız**. Grafik atlama atlamalı bilgi sağlar ve hedef ulaşılabilirlik herhangi bir sorun etkilediğini olduğunu gösterir.

    ![Grafik görünümü](./media/connection-monitor/view-graph.png)

3. Görüntüleme *AppToDB(1433)* bağlantı gösterilen aşağıdaki resimde gördüğünüz aynı kaynak ve hedef sanal makineleri için bağlantı noktası 1433 üzerinden durumu erişilemiyor. **Izgara Görünümü** Bu senaryoda atlama atlamalı bilgilerini ve ulaşılabilirlik etkileyen sorunu sağlar. Bu durumda, bir NSG kuralı ikinci atlama adresindeki 1433 numaralı bağlantı noktasındaki tüm trafiğini engelliyor.

    ![Izgara görünümü](./media/connection-monitor/view-grid.png)

## <a name="next-steps"></a>Sonraki adımlar

- Paket yakalama tarafından VM uyarılarla otomatikleştirmeyi öğrenin [bir uyarıyı tetikleyen paket yakalama oluşturma](network-watcher-alert-triggered-packet-capture.md).
- Belirli trafik içinde veya dışında VM kullanarak izin verilip verilmediğini belirlemek [IP akış doğrulayın](network-watcher-check-ip-flow-verify-portal.md).