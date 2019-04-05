---
title: Oluşturma veya bir veritabanı veya Azure veri Gezgini'nde tablo silme hatası
description: Bu makale, veritabanlarını ve tabloları Azure veri Gezgini'nde oluşturma ve silme için sorun giderme adımları açıklanmaktadır.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 0d221138914230d5455dc0addbe08cdaaed36a0b
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59050433"
---
# <a name="troubleshoot-failure-to-create-or-delete-a-database-or-table-in-azure-data-explorer"></a>Sorun giderme: Oluşturma veya bir veritabanı veya Azure veri Gezgini'nde tablo silme hatası

Azure veri Gezgini'nde düzenli olarak veritabanları ve tablolar ile çalışırsınız. Bu makalede, bulmalı sorunları için sorun giderme adımları sağlar.

## <a name="creating-a-database"></a>Veritabanı oluşturma

1. Yeterli izinlere sahip olun. Bir veritabanı oluşturmak için bir üyesi olmanız *katkıda bulunan* veya *sahibi* rolü için Azure aboneliği. Gerekirse, bunlar için uygun rolü ekleyebilmeniz için abonelik yöneticinize iş durumunda.

1. Veritabanı adı için adı doğrulama hataları olduğundan emin olun. Ad, alfasayısal, en fazla 260 karakter uzunluğunu ile olmalıdır.

1. Veritabanı saklama ve değerleri önbelleğe alma, izin verilen aralıkta olduğundan emin olun. Bekletme 1 ile 36500 gün (100 yıl) arasında olmalıdır. Önbelleğe alma, 1 ile bekletme için maksimum değer arasında olmalıdır.

## <a name="deleting-or-renaming-a-database"></a>Bir veritabanını yeniden adlandırma veya silme

Yeterli izinlere sahip olun. Bir veritabanını yeniden adlandırın ya da silmek için bir üyesi olmanız *katkıda bulunan* veya *sahibi* rolü için Azure aboneliği. Gerekirse, bunlar için uygun rolü ekleyebilmeniz için abonelik yöneticinize iş durumunda.

## <a name="creating-a-table"></a>Tablo oluşturma

1. Yeterli izinlere sahip olun. Bir tablo oluşturmak için bir üyesi olmanız *veritabanında ADMIN* veya *veritabanı kullanıcısı* veritabanı rolüne veya *katkıda bulunan* veya *sahibi* rolü için Azure aboneliği. Gerekirse, aboneliğiniz ile birlikte çalışmak veya Küme Yöneticisi, uygun role ekleyebilirsiniz.

    İzinler hakkında daha fazla bilgi için bkz. [veritabanı izinlerini yönetmek](manage-database-permissions.md).

1. Aynı ada sahip bir tablo zaten yoksa emin olun. Varsa, şunları yapabilirsiniz: Farklı ada sahip bir tablo oluşturun; Varolan bir tabloyu yeniden adlandır (gerektirir *tablo yönetici* rol); veya var olan tablo bırakma (gerektirir *veritabanında ADMIN* rol). Aşağıdaki komutları kullanın.

    ```Kusto
    .drop table <TableName>

   .rename table <OldTableName> to <NewTableName>
    ```

## <a name="deleting-or-renaming-a-table"></a>Bir tabloyu yeniden adlandırma veya silme

Yeterli izinlere sahip olun. Bir tabloyu yeniden adlandırma ya da silmek için bir üyesi olmanız *veritabanında ADMIN* veya *tablo yönetici* veritabanı rolü. Gerekirse, aboneliğiniz ile birlikte çalışmak veya Küme Yöneticisi, uygun role ekleyebilirsiniz.

İzinler hakkında daha fazla bilgi için bkz. [veritabanı izinlerini yönetmek](manage-database-permissions.md).

## <a name="general-guidance"></a>Genel kılavuz

1. Denetleme [Azure hizmet durumu Panosu](https://azure.microsoft.com/status/). Azure veri Gezgini'nde bir veritabanı veya tablo ile çalışmak için burada çalıştığınız bölge durumunu arayın.

    Durum yoksa **iyi** (yeşil onay işareti) durumu iyileştirildikten sonra yeniden deneyin.

1. Hala sorununuzun çözümüyle ilgili yardıma ihtiyacınız varsa, Lütfen bir destek isteği açın [Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).

## <a name="next-steps"></a>Sonraki adımlar

[Küme durumunu denetleme](check-cluster-health.md)