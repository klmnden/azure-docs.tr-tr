---
title: Azure altyapısının güvenliğini | Microsoft Docs
description: Bu makalede, Microsoft Güvenlik bizim Azure veri merkezleri nasıl sağlar anlatılmaktadır.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: 313fbc0fea317e8888bf64e7f7817ab0e5c9f049
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102698"
---
# <a name="security-of-azure-infrastructure"></a>Azure altyapısının güvenliğini
Microsoft Azure, yönetilen ve Microsoft tarafından işletilen veri merkezlerinde çalışır. Coğrafi olarak dağınık bu veri merkezleri, ISO/IEC 27001: 2013 ve güvenlik ve güvenilirlik için NIST SP 800-53 gibi anahtar endüstri standartları ile uyumlu. Veri merkezleri izlenen, yönetilen ve Microsoft işlem personeli tarafından yönetilir. İşlem personeli ile 7 x 24 sürekliliği dünyanın en büyük çevrimiçi hizmetler sunan içinde deneyim sahiptir.

Bu makale dizisinde, Microsoft Azure altyapı güvenli hale getirmek için yaptığı hakkında bilgi sağlar. Makaleler adresi:

- [Fiziksel güvenlik](azure-physical-security.md)
- [Kullanılabilirlik](azure-infrastructure-availability.md)
- [Bileşenleri ve sınırlar](azure-infrastructure-components.md)
- [Ağ mimarisi](azure-infrastructure-network.md)
- [Üretim ağı](azure-production-network.md)
- [SQL Database](azure-infrastructure-sql.md)
- [İşlemler](azure-infrastructure-operations.md)
- [İzleme](azure-infrastructure-monitoring.md)
- [Bütünlük](azure-infrastructure-integrity.md)
- [Veri koruma](azure-protection-of-customer-data.md)

## <a name="shared-responsibility-model"></a>Paylaşılan sorumluluk modeli
Ve Microsoft arasında sorumluluk bölümünün anlamak önemlidir. Şirket içi, tüm yığın sahipseniz, ancak bazı sorumlulukları buluta taşımak gibi transfer Microsoft. Aşağıdaki sorumluluk matrisi yığın alanlarının bir yazılım hizmet (SaaS) bir hizmet (PaaS) ve altyapı olarak platform olarak hizmet (Iaas) olarak sorumlu olan dağıtım ve Microsoft sorumlu gösterilir.

![Paylaşılan sorumluluk][1]

Her zaman dağıtım türü ne olursa olsun, tarafından korunur sorumlulukları şunlardır:

- Veriler
- Uç Noktalar
- Hesap
- Erişim Yönetimi

SaaS, PaaS ve Iaas dağıtımında ve Microsoft arasında sorumluluk bölümünün anladığınızdan emin olun. Bkz: [bulut bilgi işlemin paylaşılan sorumlulukları](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/153019/1/Shared%20responsibilities%20for%20cloud%20computing.pdf) daha fazla ayrıntı için.

## <a name="next-steps"></a>Sonraki adımlar
Microsoft Azure altyapı güvenli hale getirmek için yaptığı hakkında daha fazla bilgi için bkz:

- [Azure olanakları, şirket içi ve fiziksel güvenlik](azure-physical-security.md)
- [Azure altyapı kullanılabilirliği](azure-infrastructure-availability.md)
- [Azure Information sistem bileşenleri ve sınırlar](azure-infrastructure-components.md)
- [Azure ağ mimarisi](azure-infrastructure-network.md)
- [Azure üretim ağı](azure-production-network.md)
- [Microsoft Azure SQL veritabanı güvenlik özellikleri](azure-infrastructure-sql.md)
- [Azure üretim işlemleri ve Yönetimi](azure-infrastructure-operations.md)
- [Azure altyapı izleme](azure-infrastructure-monitoring.md)
- [Azure altyapı bütünlüğü](azure-infrastructure-integrity.md)
- [Azure müşteri verileri koruma](azure-protection-of-customer-data.md)

<!--Image references-->
[1]: ./media/azure-security-infrastructure/responsibility-zones.png
