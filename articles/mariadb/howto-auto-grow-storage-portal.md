---
title: Otomatik, Azure portalını kullanarak MariaDB için Azure veritabanı'nda depolama büyütün
description: Bu makalede, otomatik nasıl olanak sağlayabileceğiniz açıklanmaktadır. depolama için Azure veritabanı için Azure portalını kullanarak MariaDB büyütün.
author: ambhatna
ms.author: ambhatna
ms.service: mariadb
ms.topic: conceptual
ms.date: 5/29/2019
ms.openlocfilehash: bb3291b66776a5f0f6be16069b2d6a999b2d1f32
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66676886"
---
# <a name="auto-grow-storage-in-azure-database-for-mariadb-using-the-azure-portal"></a>Otomatik, Azure portalını kullanarak MariaDB için Azure veritabanı'nda depolama büyütün
Bu makalede, iş yükünü etkilemeden büyümesine MariaDB sunucu depolama için Azure veritabanı nasıl yapılandırılacağını açıklar.

Bir sunucu ayrılmış depolama sınırına ulaştığında, sunucunun salt okunur olarak işaretlenmiş. Ancak etkinleştirirseniz, depolama otomatik büyüme, büyüyen veri uyum sağlamak için sunucu alanının artmasına neden olur. Boş depolama alanı 1 GB veya 10 sağlanan depolama yüzdesi büyük olarak sağlanan 100 GB depolama alanı miktarından daha azıyla çalışabilse sunucular için 5 GB ile sağlanan depolama boyutu artar. Boş depolama alanı, sağlanan depolama boyutu %5 altında olduğunda 100 GB'den fazla sağlanan depolama alanı ile sunucular için sağlanan depolama boyutu %5 oranında artar. Maksimum depolama sınırları olarak belirtilen [burada](https://docs.microsoft.com/azure/mariadb/concepts-pricing-tiers#storage) uygulayın.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda tamamlanması gerekir:
- Bir [MariaDB için Azure veritabanı](./quickstart-create-mariadb-server-database-using-azure-portal.md)

## <a name="enable-storage-auto-grow"></a>Depolama Otomatik etkinleştirme büyütün 

Sunucu depolama otomatik büyüme MariaDB ayarlamak için aşağıdaki adımları izleyin:

1. İçinde [Azure portalında](https://portal.azure.com/), MariaDB için Azure veritabanı sunucunuza seçin.

2. MariaDB sunucusu sayfasında, altında **ayarları** başlığını tıklatın **fiyatlandırma katmanı** fiyatlandırma katmanı sayfasını açın.

3. Otomatik büyüme bölümünde **Evet** etkinleştirmek için depolama otomatik büyütün.

    ![MariaDB - Settings_Pricing_tier - otomatik-büyüme için Azure veritabanı](./media/howto-auto-grow-storage-portal/3-auto-grow.png)

4. Değişiklikleri kaydetmek için **Tamam** 'a tıklayın.

5. Bir bildirim, otomatik büyüme onaylar başarıyla etkinleştirildi.

    ![MariaDB - otomatik büyüme başarı için Azure veritabanı](./media/howto-auto-grow-storage-portal/5-auto-grow-successful.png)

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [ölçümler üzerinde uyarı oluşturma nasıl](howto-alert-metric.md).
