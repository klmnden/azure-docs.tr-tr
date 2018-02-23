---
title: "Ağ bağlantıları Azure Ağ İzleyicisi - Azure portal ile izleme | Microsoft Docs"
description: "Azure portalını kullanarak Azure Ağ İzleyicisi ile ağ bağlantısını izlemeniz öğrenin."
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2018
ms.author: jdial
ms.openlocfilehash: beaa458d727a05eccf496933deb3c998828868cd
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="monitor-network-connections-with-azure-network-watcher-using-the-azure-portal"></a>Azure portalını kullanarak Azure Ağ İzleyicisi ile ağ bağlantılarını izleme

Bağlantı İzleyici bir Azure sanal makinesi ve bir IP adresi arasında ağ bağlantısı izlemek için nasıl kullanılacağını öğrenin. IP adresi, başka bir Azure kaynak veya bir Internet veya şirket içi kaynağa atanabilir.

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları gerçekleştirmeden önce aşağıdaki gereksinimleri karşılaması gerekir:

* Ağ İzleyicisi bir bağlantı için izlemek istediğiniz bölgede bir örneği. Zaten yoksa, bir içindeki adımları tamamlayarak oluşturabilirsiniz [Azure Ağ İzleyicisi örnek oluşturmak](network-watcher-create.md).
* Gelen izlemek için bir sanal makine.
* Sahip `AzureNetworkWatcherExtension` öğesinden bir bağlantı izlemek istediğiniz sanal makinede yüklü. Uzantıyı Windows sanal makine olarak yüklemek için bkz: [Windows için Azure Ağ İzleyicisi Aracısı sanal makine uzantısı](../virtual-machines/windows/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) ve bir Linux sanal makine bakın uzantıyı yüklemek için [Azure Ağ İzleyicisi Aracısı Linux için sanal makine uzantısı](../virtual-machines/linux/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma 

[Azure Portal](http://portal.azure.com)’da oturum açın.

## <a name="create-a-connection-monitor"></a>Bağlantı izleyici oluşturmak

1. Portalın sol taraftan **daha fazla hizmet**.
2. Yazmaya başlayın *Ağ İzleyicisi*. Zaman **Ağ İzleyicisi** arama sonuçlarında görünür.
3. Altında **izleme**seçin **Bağlantı İzleyicisi'ni (Önizleme)**. Önizleme sürümü özelliklerinde genel yayın aynı düzeyde güvenilirlik veya bölge kullanılabilirliği özellikleri olarak yok.
4. Seçin **+ Ekle**.
5. İzleme ve ardından istediğiniz bağlantı için uygun bilgileri girin veya seçin **Ekle**. Bu örnekte, arasında bir bağlantı *myVmSource* ve *myVmDestination* sanal makineler, bağlantı noktası 80 üzerinden izlenir.
    
    |  Ayar                                 |  Değer               |
    |  -------------------------------------   |  ------------------- |
    |  Ad                                    |  myConnectionMonitor |
    |  Kaynak sanal makine                  |  myVmSource          |
    |  Kaynak bağlantı noktası                             |                      |
    |  Hedef, bir sanal makine seçin   |  myVmDestination     |
    |  Hedef bağlantı noktası                        |  80                  |

6. İzlemeye başlar. Bağlantı İzleyici 60 saniyede araştırmaları.
7. Bağlantı İzleyici başarısız ortalama gidiş dönüş süresi ve yüzde araştırmalar gösterir. Kılavuz veya bir grafik izleme verilerini görüntüleyebilir.

## <a name="next-steps"></a>Sonraki adımlar

- Paket yakalama tarafından sanal makine uyarılarla otomatikleştirmeyi öğrenin [bir uyarıyı tetikleyen paket yakalama oluşturma](network-watcher-alert-triggered-packet-capture.md).

- Belirli trafik içinde veya dışında sanal makinenize kullanarak izin verilip verilmediğini belirlemek [IP akış doğrulayın](network-watcher-check-ip-flow-verify-portal.md).