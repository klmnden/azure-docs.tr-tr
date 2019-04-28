---
title: Azure Güvenlik Duvarı için FQDN altyapı
description: Azure Güvenlik Duvarı'nda FQDN'leri altyapısı hakkında bilgi edinin
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 9/24/2018
ms.author: victorh
ms.openlocfilehash: 34201a0eb4139de64261f77f285096a2aa2dd3aa
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61066337"
---
# <a name="infrastructure-fqdns"></a>Altyapı FQDN'leri

Azure Güvenlik Duvarı'nda varsayılan olarak izin verilen altyapı FQDN'leri için yerleşik bir kural koleksiyonu bulunur. Bu FQDN'ler platforma özgüdür ve başka amaçlarla kullanılamaz. 

Aşağıdaki hizmetler yerleşik kural koleksiyonu içinde dahil edilmiştir:

- Platform görüntü deposuna (PIR) depolama alanına erişimi işlem
- Yönetilen diskler durumu depolama erişim
- Azure tanılama ve günlüğe kaydetme (MDS)
- Azure Active Directory

## <a name="overriding"></a>Geçersiz kılma 

Son işlenen tüm uygulama kuralı koleksiyonu reddetme oluşturarak bu yerleşik altyapı kural koleksiyonu geçersiz kılabilirsiniz. Bu kural her zaman altyapı kuralı koleksiyonundan önce işlenir. Altyapı kuralı koleksiyonunda bulunmayan öğeler varsayılan olarak reddedilir.

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [dağıtma ve bir Azure güvenlik duvarı yapılandırma](tutorial-firewall-deploy-portal.md).