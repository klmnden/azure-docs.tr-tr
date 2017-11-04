---
title: "Ağ arabirimi için Azure Windows VM sıfırlama | Microsoft Docs"
description: "Ağ arabirimi için Azure Windows VM sıfırlamak nasıl gösterir"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: genlin
manager: willchen
editor: 
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: 77d6bf3bb8b0e03419f93e97746bc1a0a091045e
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="how-to-reset-network-interface-for-azure-windows-vm"></a>Ağ arabirimi için Azure Windows VM sıfırlama 

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Ağ arabirimi (NIC) varsayılan devre dışı bırakmak veya el ile statik bir IP NIC için ayarlar sonra Microsoft Azure Windows sanal makine (VM) bağlanamıyor Bu makalede, Azure Windows, uzak bağlantı sorunu gidermek için VM, ağ arabiriminin sıfırlama gösterilmektedir.

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]
## <a name="reset-network-interface"></a>Ağ arabirimi Sıfırla

### <a name="for-classic-vms"></a>Klasik VM'ler için

Ağ arabirimi sıfırlamak için şu adımları izleyin:

1.  [Azure Portal]( https://ms.portal.azure.com) gidin.
2.  Seçin **sanal makineleri (Klasik)**.
3.  Etkilenen sanal makineyi seçin.
4.  Seçin **IP adreslerini**.
5.  Varsa **özel IP atama** değil **statik**, kendisine değiştirme **statik**.
6.  Değişiklik **IP adresi** alt ağda kullanılabilir başka bir IP adresi.
7.  Kaydet'i seçin.
8.  Sanal makine sisteme yeni NIC başlatmak için yeniden başlatılacak.
9.  İçin RDP makinenize deneyin. İsterseniz başarılı olursa, özgün durumuna geri özel IP adresi değiştirebilirsiniz. Aksi takdirde, tutabilirsiniz. 

### <a name="for-vms-deployed-in-resource-group-model"></a>Kaynak grubu modelde dağıtılan VM'ler için

1.  [Azure Portal]( https://ms.portal.azure.com) gidin.
2.  Etkilenen sanal makineyi seçin.
3.  Seçin **ağ arabirimleri**.
4.  Makine ile ilişkili ağ arabirimi seçin.
5.  Seçin **IP yapılandırmaları**.
6.  IP adresi seçin. 
7.  Varsa **özel IP atama** değil **statik**, kendisine değiştirme **statik**.
8.  Değişiklik **IP adresi** alt ağda kullanılabilir başka bir IP adresi.
9. Sanal makine sisteme yeni NIC başlatmak için yeniden başlatılacak.
10. İçin RDP makinenize deneyin. İsterseniz başarılı olursa, özgün durumuna geri özel IP adresi değiştirebilirsiniz. Aksi takdirde, tutabilirsiniz. 

## <a name="delete-the-unavailable-nics"></a>Kullanılamayan NICs Sil
Makine için Uzak Masaüstü için sonra olası sorunu önlemek için eski NIC'ler silmeniz gerekir:

1.  Aygıt Yöneticisi'ni açın.
2.  Seçin **Görünüm** > **gizli aygıtları göster**.
3.  Seçin **ağ bağdaştırıcıları**. 
4.  "Microsoft Hyper-V ağ bağdaştırıcısı" adlı bağdaştırıcıları denetleyin.
5.  Gri renkte bağdaştırıcıyı kullanılamaz görebilirsiniz. Bağdaştırıcıyı sağ tıklatın ve ardından Kaldır'ı seçin.

    ![NIC görüntüsü](media/reset-network-interface/nicpage.png)

    > [!NOTE]
    > Yalnızca "Microsoft Hyper-V ağ bağdaştırıcısı" adına sahip kullanılabilir bağdaştırıcıları kaldırın. Gizli bir bağdaştırıcı kaldırırsanız, ek sorunlara neden.
    >
    >

6.  Şimdi tüm kullanılamaz bağdaştırıcısı sisteminizden temizlenmesini.
