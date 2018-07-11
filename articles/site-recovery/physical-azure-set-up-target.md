---
title: Hedef (Fizikselden azure'a) hazırlama | Microsoft Docs
description: Bu makalede, Azure'da Windows veya Linux çalıştıran fiziksel sunucuları çoğaltmaya başlamak üzere Azure ortamınızı hazırlamak açıklar.
services: site-recovery
author: bsiva
manager: abhemraj
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: bsiva
ms.openlocfilehash: 370d245e39b848acade18d0e73f60a3246737629
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37915529"
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
