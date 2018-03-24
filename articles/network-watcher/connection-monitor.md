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
ms.openlocfilehash: c7d98350dc8f66ebd4097f22b44dcbbe2653b25d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="monitor-network-connections-with-azure-network-watcher-using-the-azure-portal"></a>Azure portalını kullanarak Azure Ağ İzleyicisi ile ağ bağlantılarını izleme

Bağlantı İzleyici bir Azure sanal makinesi ve bir IP adresi arasında ağ bağlantısı izlemek için nasıl kullanılacağını öğrenin. Bağlantı İzleyicisi, bir bağlantı düzeyde izleme sağlar. Bir bağlantı kaynak ve hedef IP adresi ve bağlantı noktası bileşimi tanımlanır. Bağlantı İzleyici SQL server 1433 numaralı bağlantı noktası üzerinden aynı veya farklı sanal ağında çalışan bir sanal makine bir sanal ağ içindeki bir sanal makineye bağlantısını izleme gibi senaryolara olanak sağlar. Bağlantı İzleyici bağlantı gecikmesi 60 saniyede kaydedilen bir Azure İzleyici ölçümü olarak sağlar. Ayrıca bir atlama atlamalı topolojisi sağlar ve bağlantınızı etkileyen yapılandırma sorunlarını tanımlar.


## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları gerçekleştirmeden önce aşağıdaki gereksinimleri karşılaması gerekir:

* Ağ İzleyicisi bir bağlantı için izlemek istediğiniz bölgede bir örneği. Zaten yoksa, bir içindeki adımları tamamlayarak oluşturabilirsiniz [Azure Ağ İzleyicisi örnek oluşturmak](network-watcher-create.md).
* Gelen izlemek için bir sanal makine.
* Sahip `AzureNetworkWatcherExtension` öğesinden bir bağlantı izlemek istediğiniz sanal makinede yüklü. Uzantıyı Windows sanal makine olarak yüklemek için bkz: [Windows için Azure Ağ İzleyicisi Aracısı sanal makine uzantısı](../virtual-machines/windows/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) ve bir Linux sanal makine bakın uzantıyı yüklemek için [Azure Ağ İzleyicisi Aracısı Linux için sanal makine uzantısı](../virtual-machines/linux/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma 

[Azure Portal](http://portal.azure.com)’da oturum açın.

## <a name="create-a-connection-monitor"></a>Bağlantı izleyici oluşturmak

Aşağıdaki adımlar, bağlantı 80 ve 1433 bağlantı noktaları üzerinden bir hedef sanal makineye izleme etkinleştirin:

1. Portalın sol taraftan **daha fazla hizmet**.
2. Yazmaya başlayın *Ağ İzleyicisi*. Zaman **Ağ İzleyicisi** arama sonuçlarında görünür.
3. Altında **izleme**seçin **Bağlantı İzleyicisi'ni (Önizleme)**. Önizleme sürümü özelliklerinde genel yayın aynı düzeyde güvenilirlik veya bölge kullanılabilirliği özellikleri olarak yok.
4. Seçin **+ Ekle**.
5. Bağlantı için izleme ve ardından istediğiniz bilgileri girin veya seçin **Ekle**. Aşağıdaki resimde gösterilen örnekte, izlenen arasında bağlantıdır *MultiTierApp0* ve *Database0* sanal makineler:

    ![Bağlantı monitör ekleme](./media/connection-monitor/add-connection-monitor.png)

    İzlemeye başlar. Bağlantı İzleyici 60 saniyede araştırmaları.

## <a name="view-connection-monitoring"></a>Bağlantı izleme görünümü

1. Adım 1-3'te tamamlamak [bağlantı izleyici oluşturmak](#create-a-connection-monitor) bağlantı izleme görüntülemek için.
2. Aşağıdaki resimde AppToDB(80) bağlantı için ayrıntıları gösterir. **Durum** ulaşılamıyor. **Grafik görünümü** gösterir **ortalama gidiş dönüş süresi** ve **% yoklamaları başarısız**. Grafik atlama atlamalı bilgi sağlar ve hedef ulaşılabilirlik herhangi bir sorun etkilediğini olduğunu gösterir.

    ![Görünüm Bağlantı İzleyicisi](./media/connection-monitor/view-connection-monitor.png)

3. Görüntüleme *AppToDB(1433)* izleme, gösterilen aşağıdaki resimde gördüğünüz aynı kaynak ve hedef sanal makine için bağlantı noktası 1433 üzerinden durumu erişilemiyor. **Izgara Görünümü** Bu senaryoda atlama atlamalı bilgilerini ve ulaşılabilirlik etkileyen sorunu sağlar. Bu durumda, bir NSG kuralı ikinci atlama adresindeki 1433 numaralı bağlantı noktasındaki tüm trafiğini engelliyor.

    ![Görünüm Bağlantı İzleyicisi](./media/connection-monitor/view-connection-monitor-2.png)

## <a name="next-steps"></a>Sonraki adımlar

- Paket yakalama tarafından sanal makine uyarılarla otomatikleştirmeyi öğrenin [bir uyarıyı tetikleyen paket yakalama oluşturma](network-watcher-alert-triggered-packet-capture.md).
- Belirli trafik içinde veya dışında sanal makinenize kullanarak izin verilip verilmediğini belirlemek [IP akış doğrulayın](network-watcher-check-ip-flow-verify-portal.md).