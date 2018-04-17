---
title: VMware VM ve Azure Site Recovery ile fiziksel sunucuya yeniden çalışma için bir işlem sunucusu Azure ayarlama | Microsoft Docs
description: Bu makalede, azure'da yeniden çalışma VMware Azure Vm'lerine bir işlem sunucusu kurma açıklar.
services: site-recovery
author: AnoopVasudavan
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: anoopkv
ms.openlocfilehash: 7bbe690e749680edde08facadf6d5910d7896f7e
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="set-up-a-process-server-in-azure-for-failback"></a>Yeniden çalışma için azure'da bir işlem sunucusu kurma

VMware Vm'leri veya Azure kullanarak fiziksel sunucular üzerinden başarısız olduktan sonra [Site Recovery](site-recovery-overview.md), yeniden çalışır olduğunda, bunları şirket içi siteye başarısız olabilir. Yeniden çalışmak için şirket içi azure'dan çoğaltmayı düzenlemek için azure'da geçici işlem sunucusu ayarlamanız gerekir. Yeniden çalışma işlemi tamamlandıktan sonra bu VM'yi silebilirsiniz.

## <a name="before-you-start"></a>Başlamadan önce

Daha fazla bilgi edinmek [yükü](vmware-azure-reprotect.md) ve [geri dönme](vmware-azure-failback.md) işlemi.

[!INCLUDE [site-recovery-vmware-process-server-prerequ](../../includes/site-recovery-vmware-azure-process-server-prereq.md)]

## <a name="deploy-a-process-server-in-azure"></a>Azure'da bir işlem sunucusu Dağıt

1. Kasadaki > **Site Recovery altyapısı**> **Yönet** > **yapılandırma sunucularına**, yapılandırma sunucusu seçin.
2. Sunucu sayfasında tıklatın **+ işlem sunucusu**
3. İçinde **işlem Sunucu Ekle** sayfası ve Azure işlem sunucusu dağıtmak için seçin.
4. Yük devretme, bir kaynak grubu, yük devretme ve Azure sanal makinelerini bulunan sanal ağ için kullanılan Azure bölgesi için kullanılan abonelik dahil olmak üzere Azure ayarlarını belirtin. Birden çok Azure ağları kullandıysanız, her biri bir işlem sunucusu gerekir.
5. İçinde **sunucu adı**, **kullanıcı adı**, ve **parola**, işlem sunucusu ve sunucuda yönetici izinleri atanmış kimlik bilgileri için bir ad belirtin.
6. Sunucu VM diskleri, işlem sunucusu VM bulunacağı alt ağı ve VM başladığında atanacak sunucu IP adresi için kullanılacak bir depolama hesabı belirtin.
7. Tıklatın **Tamam** işlem sunucusu VM dağıtma başlamak için Başlat.

>

## <a name="registering-the-process-server-running-in-azure-to-a-configuration-server-running-on-premises"></a>Yapılandırma (şirket içi çalıştıran) bir sunucuya (Azure'da çalışan) işlem sunucusu kaydetme

İşlem sunucusu VM çalışır durumda sonra aşağıdaki gibi şirket içi yapılandırma sunucusu ile kaydetmeniz gerekir:

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]


