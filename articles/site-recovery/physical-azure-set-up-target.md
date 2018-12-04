---
title: Şirket içi fiziksel sunucularını azure'a olağanüstü durum kurtarma için hedef ortamı ayarlama | Microsoft Docs
description: Bu makalede, Azure Site Recovery ile fiziksel sunucuları olağanüstü durum kurtarma için Azure ortamı hedef açıklar.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: ramamill
ms.openlocfilehash: 43276aad26bc06400c1bc4b5feaace0d5646c213
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52849250"
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
