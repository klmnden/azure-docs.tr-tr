---
title: "Mobility hizmeti fiziksel sunucu için Azure çoğaltmayı yükleme | Microsoft Docs"
description: "Bu makalede, Azure Site Recovery hizmeti ile Azure'a çoğaltma fiziksel sunucularda Mobility hizmeti aracısı yüklemeyi açıklar."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 3189fbcd-6b5b-4ffb-b5a9-e2080c37f9d9
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: d73267d7a64221a3138af19e9a2d5dd15809b927
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="step-9-install-the-mobility-service"></a>9. adım: mobilite hizmeti yükleme


Bu makale, şirket içi Windows/Linux fiziksel sunucuları Azure'a çoğaltırken Mobility hizmeti bileşeninin yükleneceği açıklamaktadır kullanarak [Azure Site Recovery](site-recovery-overview.md) Azure portalında hizmet.

Mobility hizmetinin yakalar bir makinede veri yazar ve işlem sunucusuna gönderir. Azure'a çoğaltmak istediğiniz her sunucuya yüklenmesi gerekir.

Mobility hizmeti el ile yükleyebilirsiniz veya çoğaltma etkin olduğunda Site kurtarma işlemi sunucusundan bir anında yükleme veya System Center Configuration Manager gibi bir araç kullanarak. Anında yükleme kullanırsanız, hizmeti çoğaltma etkinleştirdiğinizde, sunucu üzerinde yüklü.

POST açıklamaları ve soruları alt bu makalenin veya üzerinde [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="install-manually"></a>El ile yükleme

1. Denetleme [Önkoşullar](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) el ile yükleme.
2. İzleyin [bu yönergeleri](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) Portalı'nı kullanarak el ile yükleme.
3. Komut satırından yüklemeyi tercih ediyorsanız izleyin [bu yönergeleri](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).

## <a name="install-from-the-process-server"></a>İşlem sunucusundan yükle

Bir makine için çoğaltma etkinleştirdiğinizde mobilite hizmeti yükleme işlem sunucusundan itmek istiyorsanız, makine erişmek için işlem sunucusu tarafından kullanılan bir hesabınızın olması gerekir. Hesabı yalnızca gönderme yüklemesi için kullanılır.

1. Bir hesabı oluşturmadıysanız, bu yönergeleri kullanarak bunu yapın:

    - Bir etki alanı veya yerel hesabı kullanın
    - Windows, bir etki alanı hesabı kullanmıyorsanız yerel makine üzerinde uzak kullanıcı erişimi denetimini devre dışı bırakmak gerekir. Bu, altında kayıttaki yapmak için **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, DWORD girdisi ekleyin **LocalAccountTokenFilterPolicy**, 1 değerine sahip.
    - CLI üzerinden için Windows kayıt defteri girdisini eklemek istiyorsanız, yazın:

        ```
        REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.
        ```

    - Linux için hesabın kaynak Linux sunucusu kökünde olması gerekir.

2. Ardından izleyin [bu yönergeleri](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) Mobility hizmeti Windows veya Linux çalıştıran sanal makinelerin itmek istiyorsanız.

## <a name="other-installation-methods"></a>Diğer yükleme yöntemleri

- [Hakkında bilgi edinin](site-recovery-install-mobility-service-using-sccm.md) Yapılandırma Yöneticisi'ni kullanarak Mobility hizmeti yükleniyor
- [Hakkında bilgi edinin](site-recovery-automate-mobility-service-install.md) Azure Otomasyonu DSC'ye yükleme.


## <a name="next-steps"></a>Sonraki adımlar

Git [adım 10: çoğaltmasını etkinleştir](physical-walkthrough-enable-replication.md)
