---
title: "Hedef (fiziksel Azure) hazırlama | Microsoft Docs"
description: "Bu makalede, Azure için Windows veya Linux çalıştıran fiziksel sunucuları çoğaltıyor başlatmak üzere Azure ortamınızı hazırlamak açıklar."
services: site-recovery
author: bsiva
manager: abhemraj
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: bsiva
ms.openlocfilehash: a2465bb3397a175b6ad8b8be0de933dfae1dee5b
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="prepare-target-vmware-to-azure"></a>Hedef (VMware Azure için) hazırlama

Bu makalede, Azure'da Windows veya Linux çalıştıran fiziksel sunucuları (x 64) çoğaltılması başlatmak üzere Azure ortamınızı hazırlamak açıklar.

## <a name="prerequisites"></a>Önkoşullar

Makaleyi varsayılır:
- Fiziksel sunucuyu korumak için bir kurtarma Hizmetleri kasası oluşturdunuz. Kurtarma Hizmetleri Kasası'nı oluşturabilirsiniz [Azure portal](http://portal.azure.com "Azure portal").
- Sahip olduğunuz [, şirket içi ortamınızın Kurulumu](physical-azure-disaster-recovery.md) fiziksel sunucuları Azure'a çoğaltma için.

## <a name="prepare-target"></a>Hedef hazırlama

Tamamladıktan sonra **adım 1:Select koruma hedefi** ve **2. adım: kaynak hazırlama**, gittiğiniz **adım 3: hedef**

![Hedef hazırlama](./media/physical-azure-set-up-target/prepare-target-physical-to-azure.png)

1. **Abonelik:** aşağı açılır menüden, fiziksel sunucuları çoğaltmak istediğiniz aboneliği seçin.
2. **Dağıtım modeli:** (Klasik veya Resource Manager) dağıtım modeli seçin

Seçilen dağıtım modelini temel alan bir doğrulama fiziksel sunucularınızın en az bir uyumlu depolama hesabı ve sanal ağ çoğaltmak için hedef abonelik ve yük devretme olmasını sağlamak için çalıştırılır.

Doğrulamaları başarıyla tamamlandığında, sonraki adıma dönmek için Tamam'ı tıklatın.

Uyumlu Resource Manager depolama hesabı veya sanal ağ yoksa, tıklatarak bir oluşturabilirsiniz **+ depolama hesabı** veya **+ ağ** sayfanın üst kısmındaki düğmeler.

## <a name="next-steps"></a>Sonraki adımlar
[Çoğaltma ayarlarını yapılandırın](vmware-azure-set-up-replication.md).
