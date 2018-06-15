---
title: Azure Güvenlik Merkezi arama | Microsoft Docs
description: Günlük analizi arama Azure Güvenlik Merkezi almak ve güvenlik verilerinizi çözümlemek için nasıl kullandığını öğrenin.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/11/2017
ms.author: terrylan
ms.openlocfilehash: 513c98237a322dabd6b2bf13443e8998ca843b1d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23866404"
---
# <a name="azure-security-center-search"></a>Azure Güvenlik Merkezi arama
Azure Güvenlik Merkezi kullanan [günlük analizi arama](../log-analytics/log-analytics-log-searches.md) almak ve güvenlik verilerinizi çözümlemek için. Günlük analizi hızlı bir şekilde almak ve veri birleştirmek için bir sorgu dili içerir. Güvenlik Merkezi'nden sorgularınızı oluşturmaya ve toplanan verileri çözümlemek için günlük analizi arama yararlanabilirsiniz.

Arama, ücretsiz katmanı ve Güvenlik Merkezi'nin standart katmanı kullanılabilir.  Günlük aramalarınız kullanılabilir veri alanınıza uygulanan katmanı düzeyini bağımlıdır.  Güvenlik Merkezi bkz [fiyatlandırma sayfası](../security-center/security-center-pricing.md) daha fazla bilgi için.


> [!NOTE]
> Güvenlik Merkezi, ücretsiz katmanı altında bir çalışma alanı için güvenlik verileri kaydetmez. Çalışma alanına ücretsiz katmanı ve bu verileri arama altında günlükleri çeşitli gönderebilirsiniz ancak arama sonuçları Güvenlik Merkezi verilerinden içermez. Güvenlik Merkezi, verileri yalnızca standart katmanı altında bir çalışma alanına kaydeder.
>
>

## <a name="access-search"></a>Erişim arama
1. Güvenlik Merkezi ana menüsündeki seçin **arama**.

  ![Günlük Ara'yı seçin][1]

2. Güvenlik Merkezi, Azure aboneliklerinize altındaki tüm çalışma alanları listeler. Bir çalışma alanı seçin. (Yalnızca bir çalışma alanı varsa, bu çalışma alanı seçicisinde görünmez.)

  ![Bir çalışma alanı seçin][2]

3. **Arama oturum** açar. Seçilen çalışma alanı altında daha fazla veri sorgulamak için bu örnek sorgu girin:

  SecurityEvent | Burada EventID 4625 == | TargetAccount tarafından Count() özetler

  Sonuç (olay 4625) oturum açılamadı. tüm hesapları gösterir.

  ![Arama sonuçları][3]

Bkz: [günlük analizi sorgu dili](../log-analytics/log-analytics-search-reference.md) sorgulamak için seçilen çalışma alanı altında verileri hakkında daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede Güvenlik Merkezi'nde arama erişim öğrendiniz. Güvenlik Merkezi günlük analizi arama kullanır. Günlük analizi arama hakkında daha fazla bilgi için bkz:

- [Log Analytics nedir?](../log-analytics/log-analytics-overview.md) – Günlük analizi genel bakış
- [Anlama günlük arar günlük analizi](../log-analytics/log-analytics-log-search-new.md) - günlük aramaları günlük analizi nasıl kullanıldığını açıklar ve günlük arama oluşturmadan önce anlaşılmalıdır kavramları sağlar
- [Günlük analizi günlük aramaları kullanarak veri bulma](../log-analytics/log-analytics-log-searches.md) – günlük arama kullanma Öğreticisi
- [Günlük analizi arama başvuru](../log-analytics/log-analytics-search-reference.md) – günlük analizi sorgu dilinde açıklar

Güvenlik Merkezi hakkında daha fazla bilgi için bkz:

- [Güvenlik Merkezi'ne genel bakış](security-center-intro.md) – Describes Güvenlik Merkezi'nin anahtar özellikleri

<!--Image references-->
[1]: ./media/security-center-search/search.png
[2]: ./media/security-center-search/workspace-selector.png
[3]: ./media/security-center-search/log-search.png
