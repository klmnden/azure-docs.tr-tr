---
title: Hedef ortamı (VMware/Fizikselden azure'a) hazırlama | Microsoft Docs
description: Bu makalede, VMware VM çoğaltma ve fiziksel sunucuları azure'a çoğaltma için hedef Azure ortamını hazırlama açıklar.
services: site-recovery
author: bsiva
manager: abhemraj
ms.service: site-recovery
ms.topic: article
ms.date: 09/28/2018
ms.author: bsiva
ms.openlocfilehash: 948812f05697362978ad041566d22977efec92a1
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47434645"
---
# <a name="prepare-the-target-environment-vmwarephysical-to-azure"></a>Hedef ortamı (VMware/Fizikselden azure'a) hazırlama

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
