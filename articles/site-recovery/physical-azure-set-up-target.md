---
title: Şirket içi fiziksel sunucularını azure'a olağanüstü durum kurtarma için hedef ortamı ayarlama | Microsoft Docs
description: Bu makalede, Azure Site Recovery ile fiziksel sunucuları olağanüstü durum kurtarma için Azure ortamı hedef açıklar.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: ramamill
ms.openlocfilehash: 41220ccdca945610d7d8ca87af0857114e2cef85
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60949088"
---
# <a name="prepare-target-vmware-to-azure"></a>Hedef (Vmware'den azure'a) hazırlama

Bu makalede, Azure'da Windows veya Linux çalıştıran fiziksel sunucuları (x 64) çoğaltmaya başlamak üzere Azure ortamınızı hazırlamak açıklar.

## <a name="prerequisites"></a>Önkoşullar

Varsayılır:
- Fiziksel sunucularınızı koruma için kurtarma Hizmetleri kasası oluşturdunuz. Bir kurtarma Hizmetleri Kasasından oluşturabilirsiniz [Azure portalında](https://portal.azure.com "Azure portalında").
- Sahip olduğunuz [şirket içi ortamınızı kurma](physical-azure-disaster-recovery.md) fiziksel sunucuları Azure'a çoğaltmak için.

## <a name="prepare-target"></a>Hedefi hazırla

Tamamladıktan sonra **adım 1:Select koruma hedefi** ve **2. adım: kaynak hazırlama**, yönlendirilirsiniz **3. adım: Hedef**

![Hedefi hazırla](./media/physical-azure-set-up-target/prepare-target-physical-to-azure.png)

1. **Abonelik:** Aşağı açılan menüden, fiziksel sunucuları çoğaltmak istediğiniz aboneliği seçin.
2. **Dağıtım modeli:** Dağıtım modelini (Klasik veya Resource Manager) seçin

Seçilen dağıtım modeline bağlı olarak, fiziksel sunucularınızın en az bir uyumlu depolama hesabı ve sanal ağı hedef abonelikte çoğaltmak ve yük devretme olmasını sağlamak için bir doğrulama çalıştırılır.

Doğrulama başarıyla tamamladıktan sonra sonraki adıma geçmek için Tamam'a tıklayın.

Uyumlu Resource Manager depolama hesabı veya sanal ağ yoksa, bir tıklayarak oluşturabilirsiniz **+ depolama hesabı** veya **+ ağ** sayfanın üstündeki düğmeleri.

## <a name="next-steps"></a>Sonraki adımlar
[Çoğaltma ayarlarını yapılandırma](vmware-azure-set-up-replication.md).
