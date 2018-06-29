---
title: Azure altyapı izleme
description: Bu makalede Azure üretim ağ izleme anlatılmaktadır.
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
ms.openlocfilehash: 17e7183ff56725462dc43cba21db418a86d86b51
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102713"
---
# <a name="monitoring-of-azure-infrastructure"></a>Azure altyapı izleme   

## <a name="configuration-and-change-management"></a>Yapılandırma ve değişiklik Yönetimi
Microsoft Azure gözden geçirir ve yapılandırma ayarlarını ve donanım, yazılım ve ağ aygıtlarının temel yapılandırmalar yıllık olarak güncelleştirir. Değişiklikleri geliştirilmiş, test ve geliştirme ve/veya test ortamından üretim ortamına girme önce onaylanan.

Azure tabanlı hizmetler için gerekli temel yapılandırmalar Azure güvenliği ve uyumluluğu ekibi tarafından ve hizmet ekipleri tarafından gözden. Bir hizmet ekibin gözden geçirme, kendi üretim hizmet dağıtım öncesinde sınama parçasıdır.

## <a name="vulnerability-management"></a>Güvenlik Açığı Yönetimi
Güvenlik güncelleştirmesi yönetim sistemleri bilinen açıklarından korunmasına yardımcı olur. Dağıtım ve yükleme için Microsoft yazılımları için güvenlik güncelleştirmelerini yönetmek için tümleşik dağıtım sistemleri azure kullanır. Azure kaynakları Microsoft Güvenlik Yanıt Merkezi (MSRC) çizmek kullanabilirsiniz. MSRC tanımlar, izler, yanıt verir ve güvenlik olayları ve bulut çözümler aralıksız güvenlik açıkları, yılın her günü.

## <a name="vulnerability-scanning"></a>Güvenlik Açığı taraması
Güvenlik Açığı taraması sunucu işletim sistemleri, veritabanları ve ağ aygıtları üzerinde gerçekleştirilir. Güvenlik Açığı taramaları en az üç aylık olarak gerçekleştirilir. Microsoft Azure sınırını sızma sınamasını gerçekleştirmek için Microsoft Azure Sözleşmelerle bağımsız assessors. Kırmızı takım alıştırmaları da düzenli olarak yapılan ve sonuçları güvenlik geliştirmeler yapmak için kullanılır.

## <a name="protective-monitoring"></a>Koruyucu izleme
Microsoft Azure güvenlik etkin izleme gereksinimleri tanımlanır. Hizmet ekipleri bu gereksinimlere uygun etkin izleme araçları yapılandırın. İzleme Aracısı (MA) ve System Center Operations Manager etkin izleme araçları içerir. Bu araçlar, Microsoft Azure güvenliği personeli Acil eylem gerektiren durumlarda zaman uyarıları sağlamak için yapılandırılır.

## <a name="incident-management"></a>Olay yönetimi
Microsoft, bir gerçekleşeceğini olaylar, Eşgüdümlü yanıt kolaylaştırmak için bir güvenlik olay yönetimi süreci uygular.

Microsoft, donanım veya kendi tesis ya da böyle bir donanım veya kayıp, açığa çıkması veya Müşteri verilerinin değişikliğinin kaynaklanan tesis yetkisiz erişim depolanan müşteri verileri yetkisiz erişim farkında olursa, Microsoft aşağıdaki alır Eylemler:

- Güvenlik olayı müşteri hemen bildirir
- Derhal güvenlik olayı araştırır ve müşteri güvenlik olay hakkında ayrıntılı bilgi sağlar
- Etkilerini azaltmak ve güvenlik olay kaynaklanan bir zarar en aza indirmek için makul ve komut istemi tedbirleri alır.

Bir olay yönetim çerçevesi tanımlanmış roller ve Sorumluluklar ayrılan ile kuruldu. Windows Azure güvenlik olay Yönetimi (WASIM) ekibin güvenlik olayları yönetme, yükseltme de dahil olmak üzere ve uzmanı takımlar gerektiğinde katılımı sağlama sorumludur. Azure operasyon yöneticileri, araştırma ve güvenlik ve gizlilik olayların çözümlenmesi gözlemledikten sorumludur.

## <a name="next-steps"></a>Sonraki adımlar
Microsoft Azure altyapı güvenli hale getirmek için yaptığı hakkında daha fazla bilgi için bkz:

- [Azure olanakları, şirket içi ve fiziksel güvenlik](azure-physical-security.md)
- [Azure altyapı kullanılabilirliği](azure-infrastructure-availability.md)
- [Azure Information sistem bileşenleri ve sınırlar](azure-infrastructure-components.md)
- [Azure ağ mimarisi](azure-infrastructure-network.md)
- [Azure üretim ağı](azure-production-network.md)
- [Microsoft Azure SQL veritabanı güvenlik özellikleri](azure-infrastructure-sql.md)
- [Azure üretim işlemleri ve Yönetimi](azure-infrastructure-operations.md)
- [Azure altyapı bütünlüğü](azure-infrastructure-integrity.md)
- [Azure müşteri verileri koruma](azure-protection-of-customer-data.md)
