---
title: Azure altyapı izleme
description: Bu makalede, Azure üretim ağı izleme açıklanmaktadır.
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
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: 88bc76116392140c533f67402642c68d714227c0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60591652"
---
# <a name="azure-infrastructure-monitoring"></a>Azure altyapı izleme   

## <a name="configuration-and-change-management"></a>Yapılandırma ve değişiklik Yönetimi
Azure gözden geçirir ve yapılandırma ayarlarını ve donanım, yazılım ve ağ aygıtlarının temel yapılandırmalar yıllık güncelleştirir. Değişiklikleri geliştirilen, test ve geliştirme ve/veya test ortamından üretim ortamına girme önce onaylandı.

Azure tabanlı Hizmetleri için gerekli olan temel yapılandırmalar, hizmet ekipleri ve Azure güvenlik ve uyumluluk ekibi tarafından gözden geçirilir. Bir hizmet ekibi gözden geçirme, üretim hizmetini dağıtımdan önce gerçekleşen, test parçasıdır.

## <a name="vulnerability-management"></a>Güvenlik Açığı Yönetimi
Güvenlik güncelleştirme yönetimi, sistemleri bilinen güvenlik açıklarına karşı korumaya yardımcı olur. Dağıtımını ve yüklenmesini Microsoft yazılımları için güvenlik güncelleştirmeleri yönetmek için tümleşik dağıtım sistemleri azure kullanır. Azure ayrıca Microsoft Güvenlik Yanıt Merkezi (MSRC), kaynaklar üzerinde çizim yapabilir. MSRC tanımlar, izler, yanıt ve ilgili güvenlik etkinliklerini ve bulut çözümler sistemlerimizdeki güvenlik açıkları, yılın her günü.

## <a name="vulnerability-scanning"></a>Güvenlik Açığı taraması
Güvenlik Açığı taraması, sunucu işletim sistemleri, veritabanları ve ağ cihazları üzerinde gerçekleştirilir. Güvenlik Açığı taramaları, en az üç aylık olarak gerçekleştirilir. Azure Sınırları sızma testi gerçekleştirmek için azure ile anlaşma bağımsız denetleyiciler. Kırmızı takım alıştırmaları de düzenli olarak gerçekleştirilir ve sonuçları güvenlik geliştirmeleri yapmak için kullanılır.

## <a name="protective-monitoring"></a>Koruyucu izleme
Azure güvenlik izleme etkin gereksinimleri tanımlanır. Hizmet ekipleri, etkin izleme araçları bu gereksinimlerin uygun şekilde yapılandırın. Etkin izleme araçları Microsoft Monitoring Agent (MMA) ve System Center Operations Manager'ı içerir. Bu araçlar, Azure güvenliği personeli Acil eylem gerektiren durumlarda uyarılar sağlamak üzere yapılandırılır.

## <a name="incident-management"></a>Olay yönetimi
Microsoft, olaylar, Eşgüdümlü yanıt kolaylaştırmak için bir güvenlik olay Yönetimi işlemini bir gerçekleşmelidir uygular.

Microsoft, donanım veya kendi tesislerinde saklanan müşteri verilerine yetkisiz erişimi haberdar olur ya da böyle bir donanım veya kayıp, açığa çıkması veya Müşteri verilerinin değişikliğinin kaynaklanan tesis yetkisiz erişim haberdar olur Microsoft, aşağıdaki işlemleri yapar:

- Güvenlik olayının müşteri derhal bildirir.
- En kısa sürede güvenlik olayı araştırdıktan ve müşterilerine güvenlik olayı hakkında ayrıntılı bilgi.
- Etkilerini azaltmak ve güvenlik olayı kaynaklanan herhangi bir zarar en aza indirmek için makul ve komut istemi adımlarında rehberlik eder.

Bir olay Yönetimi framework rolleri tanımlar ve sorumlulukları ayırır kuruldu. Azure güvenlik olay yönetim ekibi, güvenlik olaylarını yönetme, yükseltme de dahil olmak üzere ve gerektiğinde uzmanı takımlar katılımı sağlama sorumludur. Azure operasyon yöneticileri, güvenlik ve gizlilik olayları ve araştırma giderek sorumludur.

## <a name="next-steps"></a>Sonraki adımlar
Microsoft Azure altyapısının güvenliğini sağlamak için yaptığı hakkında daha fazla bilgi için bkz:

- [Azure özellikleri, şirket içi ve fiziksel güvenlik](azure-physical-security.md)
- [Azure altyapı kullanılabilirlik](azure-infrastructure-availability.md)
- [Azure Information sistem bileşenleri ve sınırlar](azure-infrastructure-components.md)
- [Azure ağ mimarisi](azure-infrastructure-network.md)
- [Azure üretim ağı](azure-production-network.md)
- [Azure SQL veritabanı güvenlik özellikleri](azure-infrastructure-sql.md)
- [Azure Üretim Operasyon ve Yönetimi](azure-infrastructure-operations.md)
- [Azure altyapı bütünlüğü](azure-infrastructure-integrity.md)
- [Azure müşteri verilerini koruma](azure-protection-of-customer-data.md)
