---
title: Azure'da bir işlem sunucusu VMware VM ve Azure Site Recovery ile fiziksel sunucuda yeniden çalışma için ayarlama | Microsoft Docs
description: Bu makalede, Azure Vm'leri vmware'e yeniden çalışma, azure'da bir işlem sunucusu ayarlamak açıklar.
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: ramamill
ms.openlocfilehash: 037f0ff64b114ce9341702564147825099695aa0
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62110039"
---
# <a name="set-up-a-process-server-in-azure-for-failback"></a>Azure'da yeniden çalışma için bir işlem sunucusu ayarlama

VMware Vm'lerini veya fiziksel sunucuları aracılığıyla Azure'a yük devretme sonra [Site Recovery](site-recovery-overview.md), yeniden çalışır olduğunda bunları şirket içi siteye geri dönebilirsiniz. Yeniden çalışma için şirket içi azure'dan çoğaltmayı düzenlemek için azure'da bir geçici işlem sunucusu ayarlamanız gerekir. Yeniden çalışma işlemi tamamlandıktan sonra bu VM'yi silebilirsiniz.

## <a name="before-you-start"></a>Başlamadan önce

Daha fazla bilgi edinin [yeniden koruma](vmware-azure-reprotect.md) ve [geri dönme](vmware-azure-failback.md) işlem.

[!INCLUDE [site-recovery-vmware-process-server-prerequ](../../includes/site-recovery-vmware-azure-process-server-prereq.md)]


## <a name="deploy-a-process-server-in-azure"></a>Azure'da bir işlem sunucusu dağıtma

1. Kasadaki > **Site Recovery altyapısı**> **Yönet** > **Configuration Servers**, yapılandırma sunucusunu seçin.
2. Sunucu sayfasında tıklatın **+ işlem sunucusu**
3. İçinde **işlem Sunucu Ekle** sayfası ve azure'da işlem sunucusu dağıtmak için seçin.
4. Yük devretme, bir kaynak grubunu, yük devretme ve Azure Vm'leri bulunan sanal ağ için kullanılan Azure bölgesi için kullanılan abonelik dahil olmak üzere Azure ayarlarını belirtin. Birden fazla Azure ağını kullandıysanız, her biri bir işlem sunucusu gerekir.

   ![İşlem sunucusu galeri öğesi Ekle](./media/vmware-azure-set-up-process-server-azure/add-ps-page-1.png)

4. İçinde **sunucu adı**, **kullanıcı adı**, ve **parola**, işlem sunucusu ve sunucuda yönetici izinleri atanacak kimlik bilgileri için bir ad belirtin.
5. Server VM disklerini, işlem sunucusu VM'nin yerleştirileceği alt ağ ve VM başladığında atanacak olan sunucu IP adresi için kullanılacak bir depolama hesabı belirtin.
6. Tıklayın **Tamam** işlem sunucusu VM dağıtmaya başlamak için düğme.

>

## <a name="registering-the-process-server-running-in-azure-to-a-configuration-server-running-on-premises"></a>(Çalışan) işlem sunucusu (şirket içinde çalışan) bir yapılandırma Sunucusu'na kaydetme

İşlem sunucusu sanal makine çalışır duruma geldikten sonra şirket içi yapılandırma sunucusu ile şu şekilde kaydetmeniz gerekir:

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]


