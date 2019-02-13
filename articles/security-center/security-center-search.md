---
title: Azure Güvenlik Merkezi araması | Microsoft Docs
description: Log Analytics arama Azure Güvenlik Merkezi güvenlik verilerinizi analiz edin ve almak için nasıl kullandığını öğrenin.
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
ms.openlocfilehash: c02a9f61a4a8b88f8b6c4d861f1a6cbe904ad70d
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56110549"
---
# <a name="azure-security-center-search"></a>Azure Güvenlik Merkezi arama
Azure Güvenlik Merkezi kullanan [Log Analytics arama](../log-analytics/log-analytics-log-searches.md) almak ve güvenlik verilerinizi analiz edin. Log Analytics, hızlı bir şekilde verileri almak ve birleştirmek için bir sorgu dili içerir. Güvenlik Merkezi'nden Log Analytics arama sorguları oluşturma ve toplanan verileri çözümlemek için yararlanabilirsiniz.

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

Bkz: [Log Analytics sorgu diline](../log-analytics/log-analytics-search-reference.md) seçilen çalışma alanı altında verileri için sorgulama hakkında daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede Güvenlik Merkezi'ndeki arama erişim öğrendiniz. Güvenlik Merkezi, Log Analytics arama kullanır. Log Analytics arama hakkında daha fazla bilgi için bkz:

- [Log Analytics nedir?](../log-analytics/log-analytics-overview.md) – Log Analytics'e genel bakış
- [Günlük aramalarını anlama Log Analytics'te](../log-analytics/log-analytics-log-search-new.md) - Log Analytics'te günlük aramaları nasıl kullanıldığını açıklar ve bir günlük araması oluşturmadan önce anlaşılması kavramlar sağlar
- [Log Analytics'te günlük aramaları kullanarak veri bulmalarına](../log-analytics/log-analytics-log-searches.md) – günlük araması kullanma Öğreticisi
- [Log Analytics Arama başvurusu](../log-analytics/log-analytics-search-reference.md) – Log Analytics sorgu dilinde tanımlar

Güvenlik Merkezi hakkında daha fazla bilgi için bkz:

- [Güvenlik merkezine genel bakış](security-center-intro.md) – Describes Güvenlik Merkezi'nin temel özellikleri

<!--Image references-->
[1]: ./media/security-center-search/search.png
[2]: ./media/security-center-search/workspace-selector.png
[3]: ./media/security-center-search/log-search.png
