---
title: Azure'a VMware çoğaltma için hedef ortamı hazırlama | Microsoft Docs
description: Bu makalede Azure ortamı azure'a VMware VM çoğaltma için hedef hazırlamayı öğrenin.
services: site-recovery
author: bsiva
manager: abhemraj
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: bsiva
ms.openlocfilehash: 4b428dff1cebf353cc081696649494e6e4ec9b92
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37921119"
---
# <a name="prepare-the-target-environment-for-vmware-replication-to-azure"></a>Azure'a VMware çoğaltma için hedef ortamı hazırlama

Bu makalede, hedef VMware sanal makinelerini Azure'a çoğaltmaya başlamak için Azure ortamını hazırlama açıklar.

## <a name="prerequisites"></a>Önkoşullar

Varsayılır:
- VMware sanal makinelerinizi korumak için bir kurtarma Hizmetleri kasası oluşturdunuz. Bir kurtarma Hizmetleri Kasasından oluşturabilirsiniz [Azure portalında](http://portal.azure.com "Azure portalında").
- Sahip olduğunuz [şirket içi ortamınızı kurma](vmware-azure-set-up-source.md) VMware sanal makinelerini Azure'a çoğaltmak için.

## <a name="prepare-target"></a>Hedefi hazırla

Tamamladıktan sonra **adım 1:Select koruma hedefi** ve **2. adım: kaynak hazırlama**, yönlendirilirsiniz **3. adım: hedef**

![Hedefi hazırla](./media/vmware-azure-set-up-target/prepare-target-vmware-to-azure.png)

1. **Abonelik:** aşağı açılan menüden sanal makinelerinize çoğaltmak istediğiniz aboneliği seçin.
2. **Dağıtım modeli:** (Klasik veya Resource Manager) dağıtım modeli seçin

Seçilen dağıtım modeline bağlı olarak, sanal makinenize en az bir uyumlu depolama hesabı ve sanal ağı hedef abonelikte çoğaltmak ve yük devretme olmasını sağlamak için bir doğrulama çalıştırılır.

Doğrulama başarıyla tamamladıktan sonra sonraki adıma geçmek için Tamam'a tıklayın.

Uyumlu Resource Manager depolama hesabı veya sanal ağ yoksa, tıklayarak oluşturabilirsiniz **+ depolama hesabı** veya **+ ağ** sayfanın üstündeki düğmeleri.

## <a name="next-steps"></a>Sonraki adımlar
[Çoğaltma ayarlarını yapılandırma](vmware-azure-set-up-replication.md).
