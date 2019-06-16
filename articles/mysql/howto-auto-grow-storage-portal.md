---
title: Otomatik, Azure portalını kullanarak MySQL için Azure veritabanı'nda depolama büyütün
description: Bu makalede, otomatik nasıl olanak sağlayabileceğiniz açıklanmaktadır. depolama için Azure portalını kullanarak MySQL için Azure veritabanı büyütün
author: ambhatna
ms.author: ambhatna
ms.service: mysql
ms.topic: conceptual
ms.date: 5/29/2019
ms.openlocfilehash: 5343475f38dd5389d6b0e266ff7167925cd38d71
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66676796"
---
# <a name="auto-grow-storage-in-azure-database-for-mysql-using-the-azure-portal"></a>Otomatik, Azure portalını kullanarak MySQL için Azure veritabanı'nda depolama büyütün
Bu makalede, iş yükünü etkilemeden büyütmek MySQL server depolama için Azure veritabanı nasıl yapılandırılacağını açıklar.

Bir sunucu ayrılmış depolama sınırına ulaştığında, sunucunun salt okunur olarak işaretlenmiş. Ancak etkinleştirirseniz, depolama otomatik büyüme, büyüyen veri uyum sağlamak için sunucu alanının artmasına neden olur. Boş depolama alanı 1 GB veya 10 sağlanan depolama yüzdesi büyük olarak sağlanan 100 GB depolama alanı miktarından daha azıyla çalışabilse sunucular için 5 GB ile sağlanan depolama boyutu artar. Boş depolama alanı, sağlanan depolama boyutu %5 altında olduğunda 100 GB'den fazla sağlanan depolama alanı ile sunucular için sağlanan depolama boyutu %5 oranında artar. Maksimum depolama sınırları olarak belirtilen [burada](https://docs.microsoft.com/azure/mysql/concepts-pricing-tiers#storage) uygulayın.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda tamamlanması gerekir:
- Bir [MySQL sunucusu için Azure veritabanı](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="enable-storage-auto-grow"></a>Depolama Otomatik etkinleştirme büyütün 

MySQL Server depolama otomatik büyüme ayarlamak için aşağıdaki adımları izleyin:

1. İçinde [Azure portalında](https://portal.azure.com/), MySQL sunucusu için Azure veritabanı'nı seçin.

2. MySQL server sayfasında altında **ayarları** başlığını tıklatın **fiyatlandırma katmanı** fiyatlandırma katmanı sayfasını açın.

3. Otomatik büyüme bölümünde **Evet** etkinleştirmek için depolama otomatik büyütün.

    ![Otomatik-büyüme - Settings_Pricing_tier - MySQL için Azure veritabanı](./media/howto-auto-grow-storage-portal/3-auto-grow.png)

4. Değişiklikleri kaydetmek için **Tamam** 'a tıklayın.

5. Bir bildirim, otomatik büyüme onaylar başarıyla etkinleştirildi.

    ![MySQL - otomatik büyüme başarı için Azure veritabanı](./media/howto-auto-grow-storage-portal/5-auto-grow-success.png)

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [ölçümler üzerinde uyarı oluşturma nasıl](howto-alert-on-metric.md).
