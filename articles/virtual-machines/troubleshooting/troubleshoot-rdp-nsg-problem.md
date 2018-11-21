---
title: Azure sanal makinelere RDP bağlantı noktası NSG'de etkin olmadığından bağlanamıyor | Microsoft Docs
description: NSG yapılandırma Azure portalında RDP içinde başarısız olan bir sorunu giderme hakkında bilgi edinin | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: cshepard
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/20/2018
ms.author: genli
ms.openlocfilehash: 89af30533e10df0968b60039d7ea15886e89bc25
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52267101"
---
#  <a name="cannot-rdp-to-a-vm-because-rdp-port-is-not-enabled-in-nsg"></a>RDP bağlantı noktası NSG'de etkin olmadığından bir VM'ye RDP yapılamıyor

Bu makalede, bağlantı noktası ağ güvenlik grubunda etkin olmadığından Azure Windows sanal makinelerine (VM'ler) bağlanamıyor bir sorunun nasıl çözüleceği gösterilmektedir.


> [!NOTE] 
> Azure, kaynak oluşturmak ve bu kaynaklarla çalışmak için iki dağıtım modeli kullanır: [Resource Manager ve klasik](../../azure-resource-manager/resource-manager-deployment-model.md). Bu makale, Klasik dağıtım modeli yerine yeni dağıtımlar için kullanmanızı öneririz Resource Manager dağıtım modelini kullanarak kapsar. 

## <a name="symptom"></a>Belirti

Bir Uzak Masaüstü Protokolü (RDP bağlantı noktası nedeniyle bağlantı azure'da VM için ağ güvenlik grubunda açılmadı RDP) yapamaz.

## <a name="solution"></a>Çözüm 

Yeni bir VM oluşturduğunuzda, varsayılan olarak İnternet'ten gelen tüm trafik engellenir. 

Ağ güvenlik grubunda RDP bağlantı noktasını etkinleştirmek için bu adımları izleyin:
1. [Azure portalda](https://portal.azure.com) oturum açın.
2. İçinde **sanal makineler**, sorunlu VM. 
3. İçinde **ayarları**seçin **ağ**. 
4. İçinde **gelen bağlantı noktası kuralları**, RDP bağlantı noktası doğru şekilde ayarlanıp ayarlanmadığını denetleyin. Örnek Yapılandırması aşağıda verilmiştir. 

    **Öncelik**: 300 </br>
    **Bağlantı noktası**: 3389 </br>
    **Ad**: Port_3389 </br>
    **Bağlantı noktası**: 3389 </br>
    **Protokol**: TCP </br>
    **Kaynak**: tüm </br>
    **Hedefleri**: tüm </br>
    **Eylem**: izin ver </br>

İçinde kaynak IP adresini belirtin, bu ayar yalnızca belirli IP veya IP aralığı VM'ye bağlanmak için gelen trafiğe izin verir. RDP oturumu başlatmak için kullandığınız bilgisayara aralığında olduğundan emin olun.

Ağ güvenlik grubu hakkında daha fazla bilgi için bkz: [ağ güvenlik grubu](../../virtual-network/security-overview.md).

> [!NOTE]
> RDP bağlantı noktası 3389 Internet'e kullanıma sunulur. Bu yalnızca test etmek için önerilir. Üretim ortamları için bir VPN veya özel bağlantı kullanmanızı öneririz.

## <a name="next-steps"></a>Sonraki adımlar

Ağ güvenlik grubunda RDP bağlantı noktası zaten etkin değilse [Azure VM'de bir RDP genel hata giderme](./troubleshoot-rdp-general-error.md).



