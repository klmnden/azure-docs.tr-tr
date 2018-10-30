---
title: Azure'a VMware çoğaltma için hedef ortamı hazırlama | Microsoft Docs
description: Bu makalede Azure ortamı azure'a VMware VM çoğaltma için hedef hazırlamayı öğrenin.
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 10/29/2018
ms.author: ramamill
ms.openlocfilehash: a6f983b08415659b9a989ebed824cddd210396e1
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50233438"
---
# <a name="prepare-the-target-environment-for-disaster-recovery-of-vmware-vms-or-physical-servers-to-azure"></a>Fiziksel sunucuları azure'a VMware vm'lerinin olağanüstü durum kurtarma için hedef ortamı hazırlama

Bu makalede, hedef VMware sanal makineleri veya fiziksel sunucuları Azure'a çoğaltmaya başlamak için Azure ortamını hazırlama açıklar.

## <a name="prerequisites"></a>Önkoşullar

Varsayılır:
- Kurtarma Hizmetleri kasası üzerinde oluşturduğunuz [Azure portalında](http://portal.azure.com "Azure portalında") kaynak makinelerinizi korumak için
- Kaynak çoğaltmak için şirket içi ortamınızı kurulumun [VMware sanal makinelerini](vmware-azure-set-up-source.md) veya [fiziksel sunucuları](physical-azure-set-up-source.md) azure'a.

## <a name="prepare-target"></a>Hedefi hazırla

Tamamladıktan sonra **1. adım: koruma seçin hedefi** ve **2. adım: kaynak hazırlama**, yönlendirilirsiniz **3. adım: hedef**

![Hedefi hazırla](./media/vmware-azure-set-up-target/prepare-target-vmware-to-azure.png)

1. **Abonelik:** aşağı açılan menüden sanal makineleri veya fiziksel sunucuları çoğaltmak istediğiniz aboneliği seçin.
2. **Dağıtım modeli:** (Klasik veya Resource Manager) dağıtım modeli seçin

Seçilen dağıtım modeline bağlı olarak, sanal makine veya fiziksel sunucu için en az bir uyumlu depolama hesabı ve sanal ağı hedef abonelikte çoğaltmak ve yük devretme olduğundan emin olmak için bir doğrulama çalıştırılır.

Doğrulama başarıyla tamamladıktan sonra sonraki adıma geçmek için Tamam'a tıklayın.

Uyumlu Resource Manager depolama hesabı veya sanal ağ yoksa, bir tıklayarak oluşturabilirsiniz **+ depolama hesabı** veya **+ ağ** sayfanın üstündeki düğmeleri.

## <a name="next-steps"></a>Sonraki adımlar
[Çoğaltma ayarlarını yapılandırma](vmware-azure-set-up-replication.md).
