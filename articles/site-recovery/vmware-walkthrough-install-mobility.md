---
title: "VMware Mobility hizmeti için Azure çoğaltmayı yükleme | Microsoft Docs"
description: "Bu makalede, Azure Site Recovery hizmeti ile Azure çoğaltma VMware için Mobility hizmeti aracısı yüklemeyi açıklar."
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
ms.openlocfilehash: bc520bd2ea54208889861a7a3b275e3008a05d53
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="step-10-install-the-mobility-service"></a>10. adım: mobilite hizmeti yükleme


Bu makale, şirket içi VMware sanal makinelerini Azure'a çoğaltırken kaynak ve hedef ayarlarını yapılandırmak açıklamaktadır kullanarak [Azure Site Recovery](site-recovery-overview.md) Azure portalında hizmet.

Mobility hizmetinin yakalar bir makinede veri yazar ve işlem sunucusuna gönderir. Azure'a çoğaltmak istediğiniz her makinede yüklenmesi gerekir.

Mobility hizmeti el ile çoğaltma etkin olduğunda Site kurtarma işlemi sunucusundan bir anında yükleme kullanarak yükleyin veya System Center Configuration Manager aracını kullanın. Anında yükleme kullanırsanız, çoğaltma etkin hizmet VM yüklenir.

POST açıklamaları ve soruları alt bu makalenin veya üzerinde [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="install-manually"></a>El ile yükleme

1. Denetleme [Önkoşullar](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) el ile yükleme.
2. İzleyin [bu yönergeleri](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) Portalı'nı kullanarak el ile yükleme.
3. Komut satırından yüklemeyi tercih ediyorsanız izleyin [bu yönergeleri](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).

## <a name="install-from-the-process-server"></a>İşlem sunucusundan yükle

Bir sanal makine için çoğaltmayı etkinleştirdiğinizde mobilite hizmeti yükleme işlem sunucusundan itmek istiyorsanız, VM erişmek için işlem sunucusu tarafından kullanılan bir hesabınızın olması gerekir. Hesabı yalnızca gönderme yüklemesi için kullanılır.

1. Olması [hesap oluşturup](vmware-walkthrough-prepare-vmware.md) gönderme yüklemesi için kullanılabilir. Ardından, Site Recovery dağıtımı sırasında kaynak ayarlarını yapılandırırken kullanmak istediğiniz hesabı belirt
2. Ardından izleyin [bu yönergeleri](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) Mobility hizmeti Windows veya Linux çalıştıran sanal makinelerin itmek istiyorsanız.

## <a name="other-methods"></a>Diğer yöntemleri

Daha fazla bilgi edinmek [Yapılandırma Yöneticisi'ni kullanarak Mobility hizmeti yükleniyor](site-recovery-install-mobility-service-using-sccm.md), veya kullanarak [Azure Otomasyonu DSC](site-recovery-automate-mobility-service-install.md).

## <a name="next-steps"></a>Sonraki adımlar

Git [adım 11: çoğaltmasını etkinleştir](vmware-walkthrough-enable-replication.md)
