---
title: Azure Vm'lere RDP bağlantı noktası NSG'de etkin olmadığından bağlanamıyor | Microsoft Docs
description: NSG yapılandırma Azure portalında RDP içinde başarısız olan bir sorunu giderme hakkında bilgi edinin | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: cshepard
editor: v-jesits
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/20/2018
ms.author: genli
ms.openlocfilehash: c32612c411f275220f549eea79276fa5a7232fd0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60318944"
---
#  <a name="cannot-connect-remotely-to-a-vm-because-rdp-port-is-not-enabled-in-nsg"></a>Bir VM'ye RDP bağlantı noktası NSG'de etkinleştirilmediğinden uzaktan bağlanılamıyor

Bu makalede, Uzak Masaüstü Protokolü (RDP) bağlantı noktası ağ güvenlik grubu (NSG) etkin olmadığından bir Azure Windows sanal makinesine (VM) bağlanamıyor bir sorunun nasıl giderileceği açıklanmaktadır.


> [!NOTE] 
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki dağıtım modeli vardır: [Resource Manager ve klasik](../../azure-resource-manager/resource-manager-deployment-model.md). Yeni dağıtımlar için Klasik dağıtım modeli yerine Resource Manager dağıtım modeli kullanmanızı öneririz. 

## <a name="symptom"></a>Belirti

Ağ güvenlik grubunda RDP bağlantı noktası açık değil olduğundan azure'da bir VM ile RDP bağlantısı yapamazsınız.

## <a name="solution"></a>Çözüm 

Yeni bir VM oluşturduğunuzda, varsayılan olarak İnternet'ten gelen tüm trafik engellenir. 

Bir NSG içinde RDP bağlantı noktasını etkinleştirmek için bu adımları izleyin:
1. Oturum [Azure portalında](https://portal.azure.com).
2. İçinde **sanal makineler**, sorunlu VM'yi seçin. 
3. İçinde **ayarları**seçin **ağ**. 
4. İçinde **gelen bağlantı noktası kuralları**, RDP bağlantı noktası doğru şekilde ayarlanıp ayarlanmadığını denetleyin. Yapılandırmasına bir örnek verilmiştir: 

    **Öncelik**: 300 </br>
    **Bağlantı noktası**: 3389 </br>
    **Ad**: Port_3389 </br>
    **Bağlantı noktası**: 3389 </br>
    **Protokol**: TCP </br>
    **Kaynak**: Tüm </br>
    **Hedefleri**: Tüm </br>
    **Eylem**: İzin Ver </br>

Kaynak IP adresi belirtirseniz, bu ayar yalnızca belirli bir IP adresi veya IP adresleri, sanal Makineye bağlanmak için gelen trafiğe izin verir. RDP oturumu başlatmak için kullandığınız bilgisayara aralığında olduğundan emin olun.

Nsg'ler hakkında daha fazla bilgi için bkz. [ağ güvenlik grubu](../../virtual-network/security-overview.md).

> [!NOTE]
> RDP bağlantı noktası 3389 Internet'e kullanıma sunulur. Bu nedenle, bu bağlantı noktası yalnızca, test etmek için önerilen kullanmanızı öneririz. Üretim ortamları için bir VPN veya özel bağlantı kullanmanızı öneririz.

## <a name="next-steps"></a>Sonraki adımlar

NSG'de RDP bağlantı noktası zaten etkin değilse [Azure VM'de bir RDP genel hata giderme](./troubleshoot-rdp-general-error.md).



