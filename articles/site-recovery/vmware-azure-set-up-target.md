---
title: "Hedef ortamı VMware çoğaltma Azure için hazırlama | Microsoft Docs"
description: "Bu makalede, Azure ortamı VMware VM çoğaltma Azure için hedef hazırlama açıklar."
services: site-recovery
author: bsiva
manager: abhemraj
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: bsiva
ms.openlocfilehash: 5f14bbed7ae59737f62fb736591775cb7ba495c5
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="prepare-the-target-environment-for-vmware-replication-to-azure"></a>Azure'da VMware çoğaltma işlemini hedef ortamını hazırlayın

Bu makalede, hedef VMware sanal makinelerini Azure'a çoğaltma başlatmak için Azure ortamı hazırlamak açıklar.

## <a name="prerequisites"></a>Önkoşullar

Makaleyi varsayılır:
- VMware sanal makineleri korumak için bir kurtarma Hizmetleri kasası oluşturdunuz. Kurtarma Hizmetleri Kasası'nı oluşturabilirsiniz [Azure portal](http://portal.azure.com "Azure portal").
- Sahip olduğunuz [, şirket içi ortamınızın Kurulumu](vmware-azure-set-up-source.md) VMware sanal makinelerini Azure'a çoğaltma için.

## <a name="prepare-target"></a>Hedef hazırlama

Tamamladıktan sonra **adım 1:Select koruma hedefi** ve **2. adım: kaynak hazırlama**, gittiğiniz **adım 3: hedef**

![Hedef hazırlama](./media/vmware-azure-set-up-target/prepare-target-vmware-to-azure.png)

1. **Abonelik:** açılır menüsünden sanal makinelerinizi çoğaltmak istediğiniz aboneliği seçin.
2. **Dağıtım modeli:** (Klasik veya Resource Manager) dağıtım modeli seçin

Seçilen dağıtım modeline bağlı olarak, sanal makine için en az bir uyumlu depolama hesabı ve sanal ağ çoğaltmak için hedef abonelik ve yük devretme olmasını sağlamak için bir doğrulama çalıştırılır.

Doğrulamaları başarıyla tamamlandığında, sonraki adıma dönmek için Tamam'ı tıklatın.

Uyumlu Resource Manager depolama hesabı veya sanal ağ yoksa, tıklayarak oluşturabilirsiniz **+ depolama hesabı** veya **+ ağ** sayfanın üst kısmındaki düğmeler.

## <a name="next-steps"></a>Sonraki adımlar
[Çoğaltma ayarlarını yapılandırın](vmware-azure-set-up-replication.md).
