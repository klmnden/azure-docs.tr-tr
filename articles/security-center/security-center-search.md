---
title: Azure Güvenlik Merkezi araması | Microsoft Docs
description: Azure İzleyici günlüklerine arama Azure Güvenlik Merkezi güvenlik verilerinizi analiz edin ve almak için nasıl kullandığını öğrenin.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/11/2017
ms.author: rkarlin
ms.openlocfilehash: 6cbf3d70bd835ce1b838b19c93507f7d9487a418
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60332613"
---
# <a name="azure-security-center-search"></a>Azure Güvenlik Merkezi arama
Azure Güvenlik Merkezi kullanan [Azure İzleyici, arama günlüklerini](../log-analytics/log-analytics-log-searches.md) almak ve güvenlik verilerinizi analiz edin. Azure İzleyici günlüklerine hızlı bir şekilde verileri almak ve birleştirmek için bir sorgu dili içerir. Güvenlik Merkezi'nde Azure İzleyici günlüklerine arama sorguları oluşturma ve toplanan verileri çözümlemek için yararlanabilirsiniz.

Arama, ücretsiz katman ve Güvenlik Merkezi'nin standart katmanında kullanılabilir.  Günlük aramalarınızı kullanılabilir verileri çalışma alanınıza uygulanan katman düzeyi bağlıdır.  Güvenlik Merkezi'ni [fiyatlandırma sayfası](../security-center/security-center-pricing.md) daha fazla bilgi için.


> [!NOTE]
> Güvenlik Merkezi'nin ücretsiz katmanı altında bir çalışma alanı için güvenlik verilerini kaydetmez. Bir çalışma alanı söz konusu veriler üzerinde arama ve ücretsiz katmanı altında günlükleri çeşitli gönderebilir, ancak arama sonuçları, Güvenlik Merkezi verilerinden içermez. Güvenlik Merkezi standart katmanı altında bir çalışma alanı yalnızca veri kaydeder.
>
>

## <a name="access-search"></a>Erişim arama
1. Güvenlik Merkezi ana menüsünde seçin **arama**.

   ![Günlük araması'nı seçin][1]

2. Güvenlik Merkezi, Azure aboneliklerinizle altındaki tüm çalışma alanlarını listeler. Bir çalışma alanı seçin. (Yalnızca bir çalışma alanı varsa, bu çalışma alanı Seçici görüntülenmez.)

   ![Çalışma alanı seçin][2]

3. **Günlük arama** açılır. Seçilen çalışma alanı altında daha fazla veri sorgulamak için bu örnek sorgu girin:

   SecurityEvent | Burada EventID 4625 == | Count() by TargetAccount özetleme

   Sonucu (olay 4625) oturum açmak için başarısız olan tüm hesapları gösterir.

   ![Arama sonuçları][3]

Bkz: [Kusto sorgu dili](../log-analytics/log-analytics-search-reference.md) seçilen çalışma alanı altında verileri için sorgulama hakkında daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede Güvenlik Merkezi'ndeki arama erişim öğrendiniz. Güvenlik Merkezi, Azure İzleyici günlüklerine arama kullanır. Azure İzleyici günlüklerine arama hakkında daha fazla bilgi için bkz:

- [Azure İzleyici günlüklerine nedir?](../log-analytics/log-analytics-overview.md) – Azure İzleyici günlüklerine genel bakış
- [Günlük aramalarını anlama Azure İzleyici günlüklerine](../log-analytics/log-analytics-log-search-new.md) - günlük aramaları Azure İzleyici günlüklerine nasıl kullanıldığını açıklar ve bir günlük araması oluşturmadan önce anlaşılması kavramlar sağlar
- [Azure İzleyici günlüklerine günlük aramaları kullanarak veri bulmalarına](../log-analytics/log-analytics-log-searches.md) – günlük araması kullanma Öğreticisi
- [Kusto Arama başvurusu](../log-analytics/log-analytics-search-reference.md) – Azure İzleyici günlüklerine sorgu dilinde tanımlar

Güvenlik Merkezi hakkında daha fazla bilgi için bkz:

- [Güvenlik merkezine genel bakış](security-center-intro.md) – Describes Güvenlik Merkezi'nin temel özellikleri

<!--Image references-->
[1]: ./media/security-center-search/search.png
[2]: ./media/security-center-search/workspace-selector.png
[3]: ./media/security-center-search/log-search.png
