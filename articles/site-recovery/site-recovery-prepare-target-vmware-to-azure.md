---
title: Hedef (VMware Azure için) hazırlama | Microsoft Docs
description: Bu makalede, VMware sanal makinelerini Azure'a çoğaltma başlatmak üzere Azure ortamınızı hazırlamak açıklar.
services: site-recovery
documentationcenter: ''
author: bsiva
manager: abhemraj
editor: ''
ms.assetid: ''
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 02/22/2018
ms.author: bsiva
ms.openlocfilehash: ec1322e95431d5fd38d8e05a66322239d3184ac0
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2018
ms.locfileid: "29768427"
---
# <a name="prepare-target-vmware-to-azure"></a>Hedef (VMware Azure için) hazırlama
> [!div class="op_single_selector"]
> * [Vmware’den Azure’a](./site-recovery-prepare-target-vmware-to-azure.md)
> * [Azure için fiziksel](./site-recovery-prepare-target-physical-to-azure.md)

Bu makalede, VMware sanal makinelerini Azure'a çoğaltma başlatmak üzere Azure ortamınızı hazırlamak açıklar.

## <a name="prerequisites"></a>Önkoşullar

Makaleyi varsayılır:
- VMware sanal makineleri korumak için bir kurtarma Hizmetleri kasası oluşturdunuz. Kurtarma Hizmetleri Kasası'nı oluşturabilirsiniz [Azure portal](http://portal.azure.com "Azure portal").
- Sahip olduğunuz [, şirket içi ortamınızın Kurulumu](./site-recovery-set-up-vmware-to-azure.md) VMware sanal makinelerini Azure'a çoğaltma için.

## <a name="prepare-target"></a>Hedef hazırlama

Tamamladıktan sonra **adım 1:Select koruma hedefi** ve **2. adım: kaynak hazırlama**, gittiğiniz **adım 3: hedef**

![Hedef hazırlama](./media/site-recovery-prepare-target-vmware-to-azure/prepare-target-vmware-to-azure.png)

1. **Abonelik:** açılır menüsünden sanal makinelerinizi çoğaltmak istediğiniz aboneliği seçin.
2. **Dağıtım modeli:** (Klasik veya Resource Manager) dağıtım modeli seçin

Seçilen dağıtım modeline bağlı olarak, sanal makine için en az bir uyumlu depolama hesabı ve sanal ağ çoğaltmak için hedef abonelik ve yük devretme olmasını sağlamak için bir doğrulama çalıştırılır.

Doğrulamaları başarıyla tamamlandığında, sonraki adıma dönmek için Tamam'ı tıklatın.

Uyumlu Resource Manager depolama hesabı veya sanal ağ yoksa, tıklayarak oluşturabilirsiniz **+ depolama hesabı** veya **+ ağ** sayfanın üst kısmındaki düğmeler.

## <a name="next-steps"></a>Sonraki adımlar
[Çoğaltma ayarlarını yapılandırın](./site-recovery-setup-replication-settings-vmware.md).
