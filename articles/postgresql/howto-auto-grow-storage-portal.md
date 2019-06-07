---
title: Otomatik büyütme - tek bir sunucu PostgreSQL için Azure veritabanı'nda Azure portalını kullanarak depolama
description: Bu makalede, otomatik nasıl olanak sağlayabileceğiniz açıklanmaktadır PostegreSQL - tek bir sunucu için Azure veritabanı'nda Azure portalını kullanarak depolama büyütün
author: ambhatna
ms.author: ambhatna
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/29/2019
ms.openlocfilehash: 9f88fe3e8a30dd80c5331bd6c8ea7aec401c6fb9
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66676766"
---
# <a name="auto-grow-storage-using-the-azure-portal-in-azure-database-for-postgresql---single-server"></a>Otomatik büyütme - tek bir sunucu PostgreSQL için Azure veritabanı'nda Azure portalını kullanarak depolama
Bu makalede, iş yükünü etkilemeden büyütmek PostgreSQL sunucusu depolama için Azure veritabanı nasıl yapılandırılacağını açıklar.

Bir sunucu ayrılmış depolama sınırına ulaştığında, sunucunun salt okunur olarak işaretlenmiş. Ancak etkinleştirirseniz, depolama otomatik büyüme, büyüyen veri uyum sağlamak için sunucu alanının artmasına neden olur. Boş depolama alanı 1 GB veya 10 sağlanan depolama yüzdesi büyük olarak sağlanan 100 GB depolama alanı miktarından daha azıyla çalışabilse sunucular için 5 GB ile sağlanan depolama boyutu artar. Boş depolama alanı, sağlanan depolama boyutu %5 altında olduğunda 100 GB'den fazla sağlanan depolama alanı ile sunucular için sağlanan depolama boyutu %5 oranında artar. Maksimum depolama sınırları olarak belirtilen [burada](https://docs.microsoft.com/azure/postgresql/concepts-pricing-tiers#storage) uygulayın.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda tamamlanması gerekir:
- Bir [PostgreSQL sunucusu için Azure veritabanı](quickstart-create-server-database-portal.md)

## <a name="enable-storage-auto-grow"></a>Depolama Otomatik etkinleştirme büyütün 

PostgreSQL sunucusu depolama otomatik büyüme ayarlamak için aşağıdaki adımları izleyin:

1. İçinde [Azure portalında](https://portal.azure.com/), PostgreSQL için Azure veritabanı sunucunuza seçin.

2. PostgreSQL sunucusu sayfasında, altında **ayarları**, tıklayın **fiyatlandırma katmanı** fiyatlandırma katmanı sayfasını açın.

3. İçinde **otomatik büyüme** bölümünden **Evet** etkinleştirmek için depolama otomatik büyütün.

    ![Otomatik-büyüme - Settings_Pricing_tier - PostgreSQL için Azure veritabanı](./media/howto-auto-grow-storage-portal/3-auto-grow.png)

4. Değişiklikleri kaydetmek için **Tamam** 'a tıklayın.

5. Bir bildirim, otomatik büyüme onaylar başarıyla etkinleştirildi.

    ![-Otomatik büyüme başarılı olan PostgreSQL için Azure veritabanı](./media/howto-auto-grow-storage-portal/5-auto-grow-successful.png)

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [ölçümler üzerinde uyarı oluşturma nasıl](howto-alert-on-metric.md).
