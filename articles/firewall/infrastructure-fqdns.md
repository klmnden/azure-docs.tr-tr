---
title: Azure Güvenlik Duvarı için FQDN altyapı
description: Azure Güvenlik Duvarı'nda FQDN'leri altyapısı hakkında bilgi edinin
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 9/24/2018
ms.author: victorh
ms.openlocfilehash: 1369a82829b2c80768d746ba92daf2482c1fd7f8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46994148"
---
# <a name="infrastructure-fqdns"></a>Altyapı FQDN

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