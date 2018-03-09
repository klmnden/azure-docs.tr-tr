---
title: "Hedef (fiziksel Azure) hazırlama | Microsoft Docs"
description: "Bu makalede, Azure için Windows veya Linux çalıştıran fiziksel sunucuları çoğaltıyor başlatmak üzere Azure ortamınızı hazırlamak açıklar."
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhemraj
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 02/22/2018
ms.author: bsiva
ms.openlocfilehash: 6704ddc24db8415051a6064bbde79a6fc527871a
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2018
---
# <a name="prepare-target-physical-to-azure"></a>Hedef (fiziksel Azure) hazırlama
> [!div class="op_single_selector"]
> * [Vmware’den Azure’a](./site-recovery-prepare-target-vmware-to-azure.md)
> * [Azure için fiziksel](./site-recovery-prepare-target-physical-to-azure.md)

Bu makalede, Azure'da Windows veya Linux çalıştıran fiziksel sunucuları (x 64) çoğaltılması başlatmak üzere Azure ortamınızı hazırlamak açıklar.

## <a name="prerequisites"></a>Önkoşullar

Makaleyi varsayılır:
- Fiziksel sunucuyu korumak için bir kurtarma Hizmetleri kasası oluşturdunuz. Kurtarma Hizmetleri Kasası'nı oluşturabilirsiniz [Azure portal](http://portal.azure.com "Azure portal").
- Sahip olduğunuz [, şirket içi ortamınızın Kurulumu](./site-recovery-set-up-physical-to-azure.md) fiziksel sunucuları Azure'a çoğaltma için.

## <a name="prepare-target"></a>Hedef hazırlama

Tamamladıktan sonra **adım 1:Select koruma hedefi** ve **2. adım: kaynak hazırlama**, gittiğiniz **adım 3: hedef**

![Hedef hazırlama](./media/site-recovery-prepare-target-physical-to-azure/prepare-target-physical-to-azure.png)

1. **Abonelik:** aşağı açılır menüden, fiziksel sunucuları çoğaltmak istediğiniz aboneliği seçin.
2. **Dağıtım modeli:** (Klasik veya Resource Manager) dağıtım modeli seçin

Seçilen dağıtım modelini temel alan bir doğrulama fiziksel sunucularınızın en az bir uyumlu depolama hesabı ve sanal ağ çoğaltmak için hedef abonelik ve yük devretme olmasını sağlamak için çalıştırılır.

Doğrulamaları başarıyla tamamlandığında, sonraki adıma dönmek için Tamam'ı tıklatın.

Uyumlu Resource Manager depolama hesabı veya sanal ağ yoksa, tıklatarak bir oluşturabilirsiniz **+ depolama hesabı** veya **+ ağ** sayfanın üst kısmındaki düğmeler.

## <a name="next-steps"></a>Sonraki adımlar
[Çoğaltma ayarlarını yapılandırın](./site-recovery-setup-replication-settings-vmware.md).
