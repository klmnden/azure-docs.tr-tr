---
title: Şirket içi fiziksel sunucularını azure'a olağanüstü durum kurtarma için hedef ortamı ayarlama | Microsoft Docs
description: Bu makalede, Azure Site Recovery ile fiziksel sunucuları olağanüstü durum kurtarma için Azure ortamı hedef açıklar.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: ramamill
ms.openlocfilehash: a45e8c7bdb616eb389d95be8421bea7d31eafe29
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2018
ms.locfileid: "51974178"
---
# <a name="prepare-target-vmware-to-azure"></a>Hedef (Vmware'den azure'a) hazırlama

Bu makalede, Azure'da Windows veya Linux çalıştıran fiziksel sunucuları (x 64) çoğaltmaya başlamak üzere Azure ortamınızı hazırlamak açıklar.

## <a name="prerequisites"></a>Önkoşullar

Varsayılır:
- Fiziksel sunucularınızı koruma için kurtarma Hizmetleri kasası oluşturdunuz. Bir kurtarma Hizmetleri Kasasından oluşturabilirsiniz [Azure portalında](http://portal.azure.com "Azure portalında").
- Sahip olduğunuz [şirket içi ortamınızı kurma](physical-azure-disaster-recovery.md) fiziksel sunucuları Azure'a çoğaltmak için.

## <a name="prepare-target"></a>Hedefi hazırla

Tamamladıktan sonra **adım 1:Select koruma hedefi** ve **2. adım: kaynak hazırlama**, yönlendirilirsiniz **3. adım: hedef**

![Hedefi hazırla](./media/physical-azure-set-up-target/prepare-target-physical-to-azure.png)

1. **Abonelik:** aşağı açılan menüden, fiziksel sunucuları çoğaltmak istediğiniz aboneliği seçin.
2. **Dağıtım modeli:** (Klasik veya Resource Manager) dağıtım modeli seçin

Seçilen dağıtım modeline bağlı olarak, fiziksel sunucularınızın en az bir uyumlu depolama hesabı ve sanal ağı hedef abonelikte çoğaltmak ve yük devretme olmasını sağlamak için bir doğrulama çalıştırılır.

Doğrulama başarıyla tamamladıktan sonra sonraki adıma geçmek için Tamam'a tıklayın.

Uyumlu Resource Manager depolama hesabı veya sanal ağ yoksa, bir tıklayarak oluşturabilirsiniz **+ depolama hesabı** veya **+ ağ** sayfanın üstündeki düğmeleri.

## <a name="next-steps"></a>Sonraki adımlar
[Çoğaltma ayarlarını yapılandırma](vmware-azure-set-up-replication.md).
