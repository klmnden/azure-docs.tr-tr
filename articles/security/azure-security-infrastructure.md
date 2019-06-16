---
title: Azure altyapı güvenlik | Microsoft Docs
description: Bu makalede, Microsoft Azure merkezlerimize güvenli işleyişi anlatılmaktadır.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/06/2018
ms.author: terrylan
ms.openlocfilehash: dc9b4db37e811d8bac6df2d532fd3629d98c9650
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60586573"
---
# <a name="azure-infrastructure-security"></a>Azure Altyapı güvenliği
Microsoft Azure, yönetilen ve Microsoft tarafından işletilen veri merkezlerinde çalışır. Bu coğrafi olarak dağınık veri merkezleri, ISO/IEC 27001: 2013 ve güvenlik ve güvenilirlik için NIST SP 800-53'ü gibi temel sektör standartlarıyla uyumlu. Veri Merkezi yönetilen, izlenen ve Microsoft Operasyon personeli tarafından yönetilir. Operasyon personeli, dünyanın en büyük çevrimiçi hizmetlerini 24 x 7 süreklilikle teslim deneyim vardır.

Bu makale serisi, Microsoft Azure altyapısının güvenliğini sağlamak için yaptığı hakkında bilgi sağlar. Makaleler adresi:

- [fiziksel güvenlik](azure-physical-security.md)
- [Kullanılabilirlik](azure-infrastructure-availability.md)
- [Bileşenleri ve sınırlar](azure-infrastructure-components.md)
- [Ağ mimarisi](azure-infrastructure-network.md)
- [Üretim ağı](azure-production-network.md)
- [SQL Veritabanı](azure-infrastructure-sql.md)
- [İşlemler](azure-infrastructure-operations.md)
- [İzleme](azure-infrastructure-monitoring.md)
- [Bütünlüğü](azure-infrastructure-integrity.md)
- [Veri koruma](azure-protection-of-customer-data.md)

## <a name="shared-responsibility-model"></a>Paylaşılan sorumluluk modeli
Siz ve Microsoft arasında Sorumluluklar anlamak önemlidir. Şirket içinde tam bir yığın sahip, ancak buluta taşımak gibi bazı sorumlulukları Microsoft'a aktarım. Aşağıdaki grafikte, yığının ([SaaS] [PaaS], [ıaas] ve şirket içi altyapı hizmet olarak platform hizmet olarak yazılım) dağıtım türüne göre sorumluluk alanlarını gösterir.

![Gösteren grafik sorumlulukları][1]

Her zaman dağıtım türünden bağımsız olarak, aşağıdaki sorumlulukları şunlardır:

- Veriler
- Uç Noktalar
- Hesap
- Erişim Yönetimi

SaaS, PaaS ve Iaas dağıtımda sorumluluklar ve Microsoft arasında anladığınızdan emin olun. Daha fazla bilgi için [paylaşılan Sorumluluklar için bulut bilgi işlem](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/153019/1/Shared%20responsibilities%20for%20cloud%20computing.pdf).

## <a name="next-steps"></a>Sonraki adımlar
Microsoft Azure altyapısının güvenliğini sağlamak için yaptığı hakkında daha fazla bilgi için bkz:

- [Azure özellikleri, şirket içi ve fiziksel güvenlik](azure-physical-security.md)
- [Azure altyapı kullanılabilirlik](azure-infrastructure-availability.md)
- [Azure Information sistem bileşenleri ve sınırlar](azure-infrastructure-components.md)
- [Azure ağ mimarisi](azure-infrastructure-network.md)
- [Azure üretim ağı](azure-production-network.md)
- [Azure SQL veritabanı güvenlik özellikleri](azure-infrastructure-sql.md)
- [Azure Üretim Operasyon ve Yönetimi](azure-infrastructure-operations.md)
- [Azure altyapı izleme](azure-infrastructure-monitoring.md)
- [Azure altyapı bütünlüğü](azure-infrastructure-integrity.md)
- [Azure müşteri verilerini koruma](azure-protection-of-customer-data.md)

<!--Image references-->
[1]: ./media/azure-security-infrastructure/responsibility-zones.png
