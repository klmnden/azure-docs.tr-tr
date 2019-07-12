---
title: Bir Azure Databricks ön satın alma indirim nasıl uygulanır
description: Kullanım için bir Azure Databricks ön satın alma indirim uygulanması hakkında bilgi edinin.
services: billing
author: yashesvi
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: banders
ms.openlocfilehash: 7c1855b587ab1d603e9c6ac24a67b0f50065361f
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827630"
---
# <a name="how-azure-databricks-pre-purchase-discount-is-applied"></a>Azure Databricks ön satın alma indirimi nasıl uygulanır

Satın alma dönem boyunca herhangi bir zamanda önceden satın alınan Azure Databricks işleme birimlerini (DBCU) kullanabilirsiniz. Tüm Azure Databricks kullanımı önceden satın alınan DBCUs otomatik olarak çıkarır.

Vm'lerden farklı olarak, saatlik olarak önceden satın alınan birim dolmasın. Satın alma dönemi sırasında istediğiniz zaman kullanabilirsiniz. Ön satın alma indirimleri almak için dağıtmanız veya önceden satın alınan bir plan kullanım için Azure Databricks çalışma alanları atama gerekmez.

Ön satın alma indirimi, yalnızca Azure Databricks birimi (DBU) kullanımı için geçerlidir. İşlem, depolama ve ağ gibi diğer ücretleri ayrı olarak ücretlendirilir.

## <a name="pre-purchase-discount-application"></a>Ön satın alma indirimi

Databricks ön satın alma, tüm Databricks iş yükleri ve katmanları için geçerlidir. Ön satın alma ön ödemeli Databricks işleme birimlerini oluşan bir havuz olarak düşünebilirsiniz. Kullanımı havuz, iş yükü veya katmanını bağımsız olarak çıkarılır. Kullanım, aşağıdaki oranı çıkarılır:

| **İş yükü** | **DBU uygulama oranı — standart katman** | **DBU uygulama oranı — Premium katman** |
| --- | --- | --- |
| Veri Analizi | 0.4 | 0.55 |
| Veri Mühendisliği | 0.15 | 0.30 |
| Veri Mühendisliği Hafif Düzey | 0,07 | 0.22 |

Örneğin, bir miktar Data Analytics – standart katman kullanıldığında önceden satın alınan Databricks işleme birimleri kesinti 0,4 birimlerle. Işık mühendislik veri – miktarı standart katman kullanılır, önceden satın alınan Databricks işleme birimi 0,07 birimlerle çıkarılır.

## <a name="determine-plan-use"></a>Planı kullanın

DBCU-planı kullanımınız belirlemek için Azure portalına gidin > **ayırmaları** ve satın alınan Databricks plana tıklayın. Kalan tüm birimleri ile kullanımı başından bu yana gösterilmektedir. Rezervasyonunuz belirleme hakkında daha fazla bilgi için bkz: [bkz ayırma kullanım](billing-reservation-apis.md#see-reservation-usage) makalesi.

## <a name="how-discount-application-shows-in-usage-data"></a>İndirim uygulama kullanım verileri nasıl gösterir

Ön satın alma indirim Databricks kullanımınız için geçerli olduğu durumlarda, isteğe bağlı ücretler, kullanım verilerindeki sıfır olarak görülür. Maliyet ayırma ve kullanımı hakkında daha fazla bilgi için bkz. [Kurumsal Anlaşma edinin ayırma maliyetleri ve kullanım](billing-understand-reserved-instance-usage-ea.md).

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Sonraki adımlar

- Rezervasyon yönetme konusunda bilgi almak için bkz: [Azure ayırmalarını yönetme](billing-manage-reserved-vm-instance.md).
- Azure Databricks, paradan tasarruf etmek için önceden satın alma hakkında daha fazla bilgi için bkz. [bir ön satın alma Azure Databricks en iyi duruma getirme maliyetlerle](billing-prepay-databricks-reserved-capacity.md).
- Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
  - [Azure ayırmaları nelerdir?](billing-save-compute-costs-reservations.md)
  - [Azure'da ayırmalarını yönetme](billing-manage-reserved-vm-instance.md)
  - [Kullandıkça Öde tarifesine göre olan bir abonelik için ayırma kullanımını anlama](billing-understand-reserved-instance-usage.md)
  - [Kurumsal kayıt için ayırma kullanımını anlama](billing-understand-reserved-instance-usage-ea.md)
