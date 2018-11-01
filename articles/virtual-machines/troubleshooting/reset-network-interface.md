---
title: Azure Windows VM için ağ arabirimi sıfırlama | Microsoft Docs
description: Azure Windows VM için ağ arabirimi sıfırlama gösterir
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: genlin
manager: willchen
editor: ''
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 23cf02e8cc33b3a66a04ae0472b1e5a6baa59cc2
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50419002"
---
# <a name="how-to-reset-network-interface-for-azure-windows-vm"></a>Azure Windows VM için ağ arabirimi sıfırlama 

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Varsayılan ağ arabirimi (NIC) devre dışı bırakmak veya bir statik IP NIC için el ile ayarlar sonra Microsoft Azure Windows sanal makinesi (VM) için bağlantı kurulamıyor Bu makalede, Azure Windows Uzak bağlantı sorunu çözer, VM için ağ arabirimini sıfırlayın gösterilmektedir.

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]
## <a name="reset-network-interface"></a>Ağ arabirimini sıfırlayın

### <a name="for-classic-vms"></a>Klasik VM'ler için

Ağ arabirimi sıfırlamak için şu adımları izleyin:

1.  [Azure Portal]( https://ms.portal.azure.com) gidin.
2.  Seçin **sanal makineler (Klasik)**.
3.  Etkilenen sanal makineyi seçin.
4.  Seçin **IP adresleri**.
5.  Varsa **özel IP ataması** değil **statik**, değiştirin **statik**.
6.  Değişiklik **IP adresi** başka bir IP adresine alt ağdaki kullanılabilir.
7.  Kaydet'i seçin.
8.  Sanal makine sistemde yeni NIC'yi başlatmak için yeniden başlatılır.
9.  Makinenize yönelik RDP deneyin. İsterseniz başarılı olursa, özgün durumuna geri özel IP adresi değişebilir. Aksi takdirde, tutabilirsiniz. 

### <a name="for-vms-deployed-in-resource-group-model"></a>Kaynak grubu modelde dağıtılan VM'ler için

1.  [Azure Portal]( https://ms.portal.azure.com) gidin.
2.  Etkilenen sanal makineyi seçin.
3.  Seçin **ağ arabirimleri**.
4.  Makineyle ilişkili ağ arabirimi seçin
5.  Seçin **IP yapılandırmaları**.
6.  IP adresi seçin. 
7.  Varsa **özel IP ataması** değil **statik**, değiştirin **statik**.
8.  Değişiklik **IP adresi** başka bir IP adresine alt ağdaki kullanılabilir.
9. Sanal makine sistemde yeni NIC'yi başlatmak için yeniden başlatılır.
10. Makinenize yönelik RDP deneyin. İsterseniz başarılı olursa, özgün durumuna geri özel IP adresi değişebilir. Aksi takdirde, tutabilirsiniz. 

## <a name="delete-the-unavailable-nics"></a>Kullanılamayan NIC Sil
Makinede Uzak Masaüstü yapabilecekleriniz sonra olası bir sorundan kaçınmak için eski NIC'yi silmeniz gerekir:

1.  Cihaz Yöneticisi'ni açın.
2.  Seçin **görünümü** > **gizli aygıtları göster**.
3.  Seçin **ağ bağdaştırıcıları**. 
4.  "Microsoft Hyper-V ağ bağdaştırıcısı" olarak adlandırılmış bağdaştırıcıları denetleyin.
5.  Kullanılamaz durumdaki gri renkli bir bağdaştırıcı görebilirsiniz. Bağdaştırıcısına sağ tıklayın ve ardından Kaldır'ı seçin.

    ![NIC görüntüsü](media/reset-network-interface/nicpage.png)

    > [!NOTE]
    > Yalnızca "Microsoft Hyper-V ağ bağdaştırıcısı" adlı kullanılamayan bağdaştırıcılar kaldırın. Herhangi bir gizli bağdaştırıcılarının kaldırırsanız, ek sorunları neden olabilir.
    >
    >

6.  Artık tüm kullanılamayan bağdaştırıcılar sisteminizden temizlenmiş olmalıdır.
