---
title: "Vm'leri Azure'a AWS geçirme | Microsoft Docs"
description: "Bu makalede, Azure Site Kurtarma'yı kullanarak Azure Amazon Web Hizmetleri (AWS) çalışan sanal makineleri geçirmek açıklar."
services: site-recovery
documentationcenter: 
author: bsiva
manager: jwhit
editor: 
ms.assetid: ddb412fd-32a8-4afa-9e39-738b11b91118
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/31/2017
ms.author: bsiva
ms.openlocfilehash: b3c0727a279649f4f7dae30d41027129ce5b04ee
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="migrate-virtual-machines-in-amazon-web-services-aws-to-azure-with-azure-site-recovery"></a>Sanal makineler Amazon Web Hizmetleri (AWS) Azure Site Recovery ile azure'a geçirme

Bu makalede Azure sanal makinelerle AWS Windows örneklerini geçirmek nasıl [Azure Site Recovery](site-recovery-overview.md) hizmet.

Geçiş AWS bir yük devretmeyi Azure için etkin değil. AWS geri dönme makinelere olamaz ve devam eden çoğaltılmaz. Bu makalede, Azure portalında geçiş için gereken adımları açıklar ve yönergeler için temel alan [bir fiziksel makine Azure'a çoğaltma](site-recovery-vmware-to-azure.md).

Tüm yorumlarınızı ve sorularınızı bu makalenin veya üzerinde altındaki sonrası [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Site Recovery, aşağıdaki işletim sistemlerinden birini çalıştıran EC2 örneklerini geçirmek için kullanılabilir:

- Windows (yalnızca 64 bit)
    - Windows Server 2008 R2 SP1 + (Citrix PV sürücüleri veya yalnızca AWS PV sürücüler. **RedHat PV sürücüleri çalışan örnekleri desteklenmez**) Windows Server 2012 Windows Server 2012 R2
- Linux (yalnızca 64 bit)
    - Red Hat Enterprise Linux 6.7 (yalnızca HVM sanallaştırılmış örnekleri)

## <a name="prerequisites"></a>Ön koşullar

İşte bu dağıtım için gerekenler:

* **Yapılandırma sunucusu**: Windows Server 2012 R2 çalıştıran bir Amazon EC2 VM yapılandırma sunucusu olarak dağıtılır. Yapılandırma sunucusu dağıtırken varsayılan olarak, diğer Azure Site Recovery bileşenleri (işlem sunucusu ve ana hedef sunucusu) yüklenir. Bu makalede, Azure portalında geçiş için gereken adımları açıklar ve yönergeler için temel alan [daha fazla bilgi edinin](site-recovery-components.md)

* **EC2 örnekleri**: geçirmek istediğiniz Amazon EC2 sanal makineleri örnekleri.

## <a name="deployment-steps"></a>Dağıtım adımları

1. Kurtarma Hizmetleri kasası oluşturun.
2. EC2 örneklerinizi güvenlik grubu kuralları geçirmek istediğiniz EC2 örneği ve yapılandırma sunucusu dağıtmayı planladığınız örneği arasında iletişime izin verecek şekilde yapılandırılmış olması gerekir.

3. Aynı Amazon sanal üzerine özel buluta EC2 örneklerinizi olarak, bir ASR yapılandırma sunucusu dağıtın. VMware başvurmak / yapılandırma sunucusu dağıtım gereksinimleri Azure önkoşulları fiziksel.

    ![DeployCS](./media/site-recovery-migrate-aws-to-azure/migration_pic2.png)

4.  Yapılandırma sunucusu AWS içinde dağıtılır ve, Kurtarma Hizmetleri kasasına kayıtlı sonra Site Recovery altyapısı altında işlem sunucusu ve yapılandırma sunucusu aşağıda gösterildiği gibi görmeniz gerekir:

    ![CSinVault](./media/site-recovery-migrate-aws-to-azure/migration_pic3.png)

5. (En fazla 15 minustes görünmesi için sürebilir) yapılandırma sunucusu dağıtıldıktan sonra iletişim kurabildiğini doğrulayın, geçirmek istediğiniz sanal makineleri ile.

6. [Çoğaltma ayarlarını belirleme](site-recovery-setup-replication-settings-vmware.md).

7. Çoğaltmayı etkinleştirme: çoğaltma geçiş yapmak istediğiniz sanal makineleri için etkinleştirin. EC2 Konsolu'ndan alabilirsiniz özel IP adresleri kullanan EC2 örneklerini bulabilir.

    ![SelectVM](./media/site-recovery-migrate-aws-to-azure/migration_pic4.png)

8. EC2 örnekleri korumalı ve azure'a çoğaltma tamamlandıktan sonra [bir yük devretme testi](site-recovery-test-failover-to-azure.md) uygulamanızın performansını Azure doğrulanacak.

    ![TFI](./media/site-recovery-migrate-aws-to-azure/migration_pic5.png)

9. Bir yük devretme AWS her VM için Azure'a çalıştırın. İsteğe bağlı olarak, bir kurtarma planı oluşturun ve Azure'a AWS birden çok sanal makineleri geçirmek için bir yük devretme çalıştırın. [Daha fazla bilgi edinin](site-recovery-create-recovery-plans.md) kurtarma planları hakkında.

10. Geçiş için yük devretme yapmanız gerekmez. Bunun yerine, geçirmek istediğiniz her makine için tam geçiş seçeneğini belirleyin. Geçişi tamamlamak eylem geçiş işlemini tamamlar, makine için çoğaltmayı kaldırır ve makine için Site Recovery Faturalaması durdurulur.

    ![Geçiş](./media/site-recovery-migrate-aws-to-azure/migration_pic6.png)

## <a name="next-steps"></a>Sonraki adımlar

- Olağanüstü durum kurtarma ihtiyaçlarına yönelik olarak başka bir bölgeye [çoğaltmayı etkinleştirmek için, geçirilen makineleri hazırlayın](site-recovery-azure-to-azure-after-migration.md).
- [Azure sanal makinelerini çoğaltarak](site-recovery-azure-to-azure.md) iş yüklerinizi korumaya başlayın.
