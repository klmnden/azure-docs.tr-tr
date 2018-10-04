---
title: Azure ayırmaları görüntüleme | Microsoft Docs
description: Azure portalında Azure rezervasyonların öğrenin.
services: billing
documentationcenter: ''
author: yashesvi
manager: yashar
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/03/2018
ms.author: cwatson
ms.openlocfilehash: 2ae30ca55f3ca03a64438025960ddd807e288216
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48272662"
---
# <a name="view-azure-reservations-in-the-azure-portal"></a>Azure portalında Azure ayırmalar görünümü

Abonelik türü ve izinlerine bağlı olarak çeşitli şekillerde Azure rezervasyonların vardır.

## <a name="view-reservations-as-owner-or-reader"></a>Rezervasyonları sahibi veya okuyucu olarak görüntüleme

Varsayılan olarak, bir ayırma satın aldığınızda ve Hesap Yöneticisi ayırma görüntüleyebilirsiniz. Otomatik olarak siz ve Hesap Yöneticisi sahip rolü üzerinde ayırma alın. Görüntülemek diğer kişilere izin vermek için bunları olarak eklemelisiniz bir **sahibi** veya **okuyucu** rezervasyonu. Daha fazla bilgi için [rezervasyon yönetebilirsiniz ekleme veya değiştirme kullanıcılar](billing-manage-reserved-vm-instance.md#add-or-change-users-who-can-manage-a-reservation).
 
Bir sahibi veya okuyucu olarak görüntülemek için

1. [Azure Portal]( http://portal.azure.com)’da oturum açın.
1. Arama **ayırmaları**.

    ![Azure portalı arama gösteren ekran görüntüsü](./media/billing-view-reservation/portal-reservation-search.png)

1. Sahip veya okuyucu rolüne sahip olduğunuz ayırmaları listesini görürsünüz.

Rezervasyon kapsamı değiştirmeniz gerekiyorsa, bölünmüş bir ayırma veya bir ayırma yönetebilen değişiklik bkz [Azure ayırmalarını yönetme](billing-manage-reserved-vm-instance.md).

## <a name="view-reservation-transactions-for-enterprise-enrollments"></a>Kurumsal kayıtları için ayırma işlemleri görüntüle

 Bir iş ortağı LED'i Kurumsal kayıt varsa, giderek rezervasyonların **raporları** EA portalında. Diğer Kurumsal kayıtları için ayırmaları EA portal ve Azure portalında görüntüleyebilirsiniz. Ayırma işlemleri görüntülemek için EA yönetici olmanız gerekir.

Azure portalında ayırma işlemleri görüntülemek için

1. [Azure Portal]( http://portal.azure.com)’da oturum açın.
1. Arama **maliyet Yönetimi + faturalandırma**.

    ![Azure portalı arama gösteren ekran görüntüsü](./media/billing-view-reservation/portal-cm-billing-search.png)

1. Seçin **ayırma işlemleri**.
1. Sonuçları filtrelemek için seçin **Timespan**, **türü**, veya **açıklama**.
1. **Uygula**’yı seçin.

    ![Ayırma işlemleri sonuçlarını gösteren ekran görüntüsü](./media/billing-view-reservation/portal-billing-reservation-transaction-results.png)

Bir API kullanarak veri almak için bkz: [Kurumsal müşteriler için ayrılmış örnek alma işlem ücretleri](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-charges).

## <a name="next-steps"></a>Sonraki adımlar

Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure ayırmaları nelerdir?](billing-save-compute-costs-reservations.md)
- [Cosmos DB ayrılmış kapasite için ön ödeme](../cosmos-db/cosmos-db-reserved-capacity.md)
- [Azure SQL Veritabanı ayrılmış kapasitesi ile SQL Veritabanı işlem kaynakları için ön ödeme yapma](../sql-database/sql-database-reserved-capacity.md)
- [Azure Ayrılmış VM Örnekleri ile Sanal Makinelere ön ödeme yapma](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Azure Ayırmalarını yönetme](billing-manage-reserved-vm-instance.md)
- [Kullandıkça Öde aboneliğinizi için ayırma kullanımını anlama](billing-understand-reserved-instance-usage.md)
- [Kurumsal kayıt için ayırma kullanımını anlama](billing-understand-reserved-instance-usage-ea.md)
- [CSP abonelikleri için ayırma kullanımını anlama](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Hala başka sorularınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.
