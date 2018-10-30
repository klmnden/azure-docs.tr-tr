---
title: Oluşturma veya bir veritabanı veya Azure veri Gezgini'nde tablo silme hatası
description: Bu makale, veritabanlarını ve tabloları Azure veri Gezgini'nde oluşturma ve silme için sorun giderme adımları açıklanmaktadır.
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: ec66066fe51af97f6355b78dd7af3480a39a5a03
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50215098"
---
# <a name="troubleshoot-failure-to-create-or-delete-a-database-or-table-in-azure-data-explorer"></a>Sorunlarını giderme: hata oluşturma veya bir veritabanı veya Azure veri Gezgini'nde tablo silme

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

1. Aynı ada sahip bir tablo zaten yoksa emin olun. Mevcut sonra şunları yapabilirsiniz: farklı bir adla; tablosu oluşturma Varolan bir tabloyu yeniden adlandır (gerektirir *tablo yönetici* rol); veya var olan tablo bırakma (gerektirir *veritabanında ADMIN* rol). Aşağıdaki komutları kullanın.

    ```Kusto
    .drop table <TableName>

   .rename table <OldTableName> to <NewTableName>
    ```

## <a name="deleting-or-renaming-a-table"></a>Bir tabloyu yeniden adlandırma veya silme

Yeterli izinlere sahip olun. Bir tabloyu yeniden adlandırma ya da silmek için bir üyesi olmanız *veritabanında ADMIN* veya *tablo yönetici* veritabanı rolü. Gerekirse, aboneliğiniz ile birlikte çalışmak veya Küme Yöneticisi, uygun role ekleyebilirsiniz.

İzinler hakkında daha fazla bilgi için bkz. [veritabanı izinlerini yönetmek](manage-database-permissions.md).

## <a name="general-guidance"></a>Genel kılavuz

1. Denetleme [Azure hizmet durumu Panosu](https://azure.microsoft.com/status/>). Azure veri Gezgini'nde bir veritabanı veya tablo ile çalışmak için burada çalıştığınız bölge durumunu arayın.

    Durum yoksa **iyi** (yeşil onay işareti) durumu iyileştirildikten sonra yeniden deneyin.

1. Hala sorununuzun çözümüyle ilgili yardıma ihtiyacınız varsa, Lütfen bir destek isteği açın [Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).

## <a name="next-steps"></a>Sonraki adımlar

[Küme durumunu denetleme](check-cluster-health.md)